---
title: ASP.NET Core SignalR 生產環境裝載和調整
author: bradygaster
description: 瞭解如何避免使用 ASP.NET Core 的應用程式中的效能和調整問題 SignalR 。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/17/2020
no-loc:
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: signalr/scale
ms.openlocfilehash: 2d128d54dc9b1189124563e45d72d74b19704ab1
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88022520"
---
# <a name="aspnet-core-no-locsignalr-hosting-and-scaling"></a><span data-ttu-id="a9b09-103">ASP.NET Core SignalR 裝載和調整</span><span class="sxs-lookup"><span data-stu-id="a9b09-103">ASP.NET Core SignalR hosting and scaling</span></span>

<span data-ttu-id="a9b09-104">[Andrew Stanton-護士](https://twitter.com/anurse)、 [Brady Gaster](https://twitter.com/bradygaster)和[Tom 作者: dykstra](https://github.com/tdykstra)，</span><span class="sxs-lookup"><span data-stu-id="a9b09-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse), [Brady Gaster](https://twitter.com/bradygaster), and [Tom Dykstra](https://github.com/tdykstra),</span></span>

<span data-ttu-id="a9b09-105">本文說明使用 ASP.NET Core 之高流量應用程式的裝載和調整考慮 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="a9b09-105">This article explains hosting and scaling considerations for high-traffic apps that use ASP.NET Core SignalR.</span></span>

## <a name="sticky-sessions"></a><span data-ttu-id="a9b09-106">粘滯話</span><span class="sxs-lookup"><span data-stu-id="a9b09-106">Sticky Sessions</span></span>

<span data-ttu-id="a9b09-107">SignalR要求特定連接的所有 HTTP 要求都必須由相同的伺服器進程處理。</span><span class="sxs-lookup"><span data-stu-id="a9b09-107">SignalR requires that all HTTP requests for a specific connection be handled by the same server process.</span></span> <span data-ttu-id="a9b09-108">當在 SignalR 伺服器陣列上執行 (多部伺服器) 時，必須使用「粘滯會話」。</span><span class="sxs-lookup"><span data-stu-id="a9b09-108">When SignalR is running on a server farm (multiple servers), "sticky sessions" must be used.</span></span> <span data-ttu-id="a9b09-109">某些負載平衡器也稱為「粘滯會話」的會話親和性。</span><span class="sxs-lookup"><span data-stu-id="a9b09-109">"Sticky sessions" are also called session affinity by some load balancers.</span></span> <span data-ttu-id="a9b09-110">Azure App Service 使用[應用程式要求路由](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (ARR) 來路由傳送要求。</span><span class="sxs-lookup"><span data-stu-id="a9b09-110">Azure App Service uses [Application Request Routing](https://docs.microsoft.com/iis/extensions/planning-for-arr/application-request-routing-version-2-overview) (ARR) to route requests.</span></span> <span data-ttu-id="a9b09-111">在您的 Azure App Service 中啟用「ARR 親和性」設定，將會啟用「粘滯會話」。</span><span class="sxs-lookup"><span data-stu-id="a9b09-111">Enabling the "ARR Affinity" setting in your Azure App Service will enable "sticky sessions".</span></span> <span data-ttu-id="a9b09-112">不需要粘滯會話的唯一情況如下：</span><span class="sxs-lookup"><span data-stu-id="a9b09-112">The only circumstances in which sticky sessions are not required are:</span></span>

1. <span data-ttu-id="a9b09-113">在單一伺服器上裝載時，在單一進程中。</span><span class="sxs-lookup"><span data-stu-id="a9b09-113">When hosting on a single server, in a single process.</span></span>
1. <span data-ttu-id="a9b09-114">使用 Azure 服務時 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="a9b09-114">When using the Azure SignalR Service.</span></span>
1. <span data-ttu-id="a9b09-115">當所有用戶端都設定為**只**使用 websocket，**而且**用戶端設定中已啟用[SkipNegotiation 設定](xref:signalr/configuration#configure-additional-options)時。</span><span class="sxs-lookup"><span data-stu-id="a9b09-115">When all clients are configured to **only** use WebSockets, **and** the [SkipNegotiation setting](xref:signalr/configuration#configure-additional-options) is enabled in the client configuration.</span></span>

<span data-ttu-id="a9b09-116">在所有其他情況下 (包括使用 Redis 背板) 時，必須針對 [粘滯會話] 設定伺服器環境。</span><span class="sxs-lookup"><span data-stu-id="a9b09-116">In all other circumstances (including when the Redis backplane is used), the server environment must be configured for sticky sessions.</span></span>

<span data-ttu-id="a9b09-117">如需設定 Azure App Service 的相關指引 SignalR ，請參閱 <xref:signalr/publish-to-azure-web-app> 。</span><span class="sxs-lookup"><span data-stu-id="a9b09-117">For guidance on configuring Azure App Service for SignalR, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="tcp-connection-resources"></a><span data-ttu-id="a9b09-118">TCP 連線資源</span><span class="sxs-lookup"><span data-stu-id="a9b09-118">TCP connection resources</span></span>

<span data-ttu-id="a9b09-119">網頁伺服器可支援的並行 TCP 連線數目有限。</span><span class="sxs-lookup"><span data-stu-id="a9b09-119">The number of concurrent TCP connections that a web server can support is limited.</span></span> <span data-ttu-id="a9b09-120">標準 HTTP 用戶端會使用*暫時*連接。</span><span class="sxs-lookup"><span data-stu-id="a9b09-120">Standard HTTP clients use *ephemeral* connections.</span></span> <span data-ttu-id="a9b09-121">當用戶端閒置並在稍後重新開啟時，可以關閉這些連線。</span><span class="sxs-lookup"><span data-stu-id="a9b09-121">These connections can be closed when the client goes idle and reopened later.</span></span> <span data-ttu-id="a9b09-122">另一方面， SignalR 連接則是*持續*性的。</span><span class="sxs-lookup"><span data-stu-id="a9b09-122">On the other hand, a SignalR connection is *persistent*.</span></span> <span data-ttu-id="a9b09-123">SignalR即使用戶端閒置，連線還是會保持開啟狀態。</span><span class="sxs-lookup"><span data-stu-id="a9b09-123">SignalR connections stay open even when the client goes idle.</span></span> <span data-ttu-id="a9b09-124">在服務許多用戶端的高流量應用程式中，這些持續連線可能會導致伺服器達到連線的最大數目。</span><span class="sxs-lookup"><span data-stu-id="a9b09-124">In a high-traffic app that serves many clients, these persistent connections can cause servers to hit their maximum number of connections.</span></span>

<span data-ttu-id="a9b09-125">持續性連接也會耗用一些額外的記憶體，以追蹤每個連接。</span><span class="sxs-lookup"><span data-stu-id="a9b09-125">Persistent connections also consume some additional memory, to track each connection.</span></span>

<span data-ttu-id="a9b09-126">大量使用連線相關的資源， SignalR 可能會影響裝載于相同伺服器上的其他 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a9b09-126">The heavy use of connection-related resources by SignalR can affect other web apps that are hosted on the same server.</span></span> <span data-ttu-id="a9b09-127">當 SignalR 開啟並保存最後一個可用的 TCP 連線時，相同伺服器上的其他 web 應用程式也不會有其他可用的連接。</span><span class="sxs-lookup"><span data-stu-id="a9b09-127">When SignalR opens and holds the last available TCP connections, other web apps on the same server also have no more connections available to them.</span></span>

<span data-ttu-id="a9b09-128">如果伺服器用盡連線，您會看到隨機的通訊端錯誤和連線重設錯誤。</span><span class="sxs-lookup"><span data-stu-id="a9b09-128">If a server runs out of connections, you'll see random socket errors and connection reset errors.</span></span> <span data-ttu-id="a9b09-129">例如：</span><span class="sxs-lookup"><span data-stu-id="a9b09-129">For example:</span></span>

```
An attempt was made to access a socket in a way forbidden by its access permissions...
```

<span data-ttu-id="a9b09-130">若要讓 SignalR 資源使用量不會造成其他 web 應用程式中的錯誤，請 SignalR 在與其他 web 應用程式不同的伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="a9b09-130">To keep SignalR resource usage from causing errors in other web apps, run SignalR on different servers than your other web apps.</span></span>

<span data-ttu-id="a9b09-131">若要讓 SignalR 資源使用量不會造成應用程式中的錯誤 SignalR ，請相應放大以限制伺服器必須處理的連線數目。</span><span class="sxs-lookup"><span data-stu-id="a9b09-131">To keep SignalR resource usage from causing errors in a SignalR app, scale out to limit the number of connections a server has to handle.</span></span>

## <a name="scale-out"></a><span data-ttu-id="a9b09-132">擴增</span><span class="sxs-lookup"><span data-stu-id="a9b09-132">Scale out</span></span>

<span data-ttu-id="a9b09-133">使用的應用程式 SignalR 需要追蹤其所有連線，這會造成伺服器陣列的問題。</span><span class="sxs-lookup"><span data-stu-id="a9b09-133">An app that uses SignalR needs to keep track of all its connections, which creates problems for a server farm.</span></span> <span data-ttu-id="a9b09-134">新增伺服器，並取得其他伺服器不知道的新連接。</span><span class="sxs-lookup"><span data-stu-id="a9b09-134">Add a server, and it gets new connections that the other servers don't know about.</span></span> <span data-ttu-id="a9b09-135">例如，在 SignalR 下圖中的每一部伺服器上，都不知道其他伺服器上的連接。</span><span class="sxs-lookup"><span data-stu-id="a9b09-135">For example, SignalR on each server in the following diagram is unaware of the connections on the other servers.</span></span> <span data-ttu-id="a9b09-136">當 SignalR 其中一個伺服器想要將訊息傳送至所有用戶端時，訊息只會移至連線到該伺服器的用戶端。</span><span class="sxs-lookup"><span data-stu-id="a9b09-136">When SignalR on one of the servers wants to send a message to all clients, the message only goes to the clients connected to that server.</span></span>

![調整：：：無-loc (SignalR) ：：：沒有背板](scale/_static/scale-no-backplane.png)

<span data-ttu-id="a9b09-138">解決此問題的選項包括[Azure SignalR 服務](#azure-signalr-service)和[Redis 背板](#redis-backplane)。</span><span class="sxs-lookup"><span data-stu-id="a9b09-138">The options for solving this problem are the [Azure SignalR Service](#azure-signalr-service) and [Redis backplane](#redis-backplane).</span></span>

## <a name="azure-no-locsignalr-service"></a><span data-ttu-id="a9b09-139">Azure SignalR 服務</span><span class="sxs-lookup"><span data-stu-id="a9b09-139">Azure SignalR Service</span></span>

<span data-ttu-id="a9b09-140">Azure SignalR 服務是一個 proxy，而不是背板。</span><span class="sxs-lookup"><span data-stu-id="a9b09-140">The Azure SignalR Service is a proxy rather than a backplane.</span></span> <span data-ttu-id="a9b09-141">每次用戶端起始與伺服器的連線時，會將用戶端重新導向以連接到服務。</span><span class="sxs-lookup"><span data-stu-id="a9b09-141">Each time a client initiates a connection to the server, the client is redirected to connect to the service.</span></span> <span data-ttu-id="a9b09-142">該程式如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="a9b09-142">That process is illustrated in the following diagram:</span></span>

![建立與 Azure：：： no-loc (SignalR) ：：：服務的連線](scale/_static/azure-signalr-service-one-connection.png)

<span data-ttu-id="a9b09-144">結果是服務會管理所有用戶端連線，而每個伺服器只需要少量的服務連線，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="a9b09-144">The result is that the service manages all of the client connections, while each server needs only a small constant number of connections to the service, as shown in the following diagram:</span></span>

![連線到服務的用戶端連線到服務的伺服器](scale/_static/azure-signalr-service-multiple-connections.png)

<span data-ttu-id="a9b09-146">這種向外延展的方法在 Redis 背板上有數個優點：</span><span class="sxs-lookup"><span data-stu-id="a9b09-146">This approach to scale-out has several advantages over the Redis backplane alternative:</span></span>

* <span data-ttu-id="a9b09-147">因為用戶端會在連線時立即重新導向至 Azure 服務，所以不需要像是「[用戶端親和性](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)」的「粘滯話」 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="a9b09-147">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is not required, because clients are immediately redirected to the Azure SignalR Service when they connect.</span></span>
* <span data-ttu-id="a9b09-148">SignalR應用程式可以根據傳送的訊息數目相應放大，而 Azure SignalR 服務會自動調整以處理任何數目的連線。</span><span class="sxs-lookup"><span data-stu-id="a9b09-148">A SignalR app can scale out based on the number of messages sent, while the Azure SignalR Service automatically scales to handle any number of connections.</span></span> <span data-ttu-id="a9b09-149">例如，可能有數千個用戶端，但如果每秒只傳送幾則訊息， SignalR 應用程式就不需要向外延展至多部伺服器，只是為了處理連線本身。</span><span class="sxs-lookup"><span data-stu-id="a9b09-149">For example, there could be thousands of clients, but if only a few messages per second are sent, the SignalR app won't need to scale out to multiple servers just to handle the connections themselves.</span></span>
* <span data-ttu-id="a9b09-150">SignalR應用程式在沒有的情況下，將無法使用比 web 應用程式更多的連接資源 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="a9b09-150">A SignalR app won't use significantly more connection resources than a web app without SignalR.</span></span>

<span data-ttu-id="a9b09-151">基於這些理由，建議您在 SignalR azure 上裝載的所有 ASP.NET Core SignalR 應用程式（包括 App Service、vm 和容器）都採用 azure 服務。</span><span class="sxs-lookup"><span data-stu-id="a9b09-151">For these reasons, we recommend the Azure SignalR Service for all ASP.NET Core SignalR apps hosted on Azure, including App Service, VMs, and containers.</span></span>

<span data-ttu-id="a9b09-152">如需詳細資訊，請參閱[Azure SignalR 服務檔](/azure/azure-signalr/signalr-overview)。</span><span class="sxs-lookup"><span data-stu-id="a9b09-152">For more information see the [Azure SignalR Service documentation](/azure/azure-signalr/signalr-overview).</span></span>

## <a name="redis-backplane"></a><span data-ttu-id="a9b09-153">Redis 後擋板</span><span class="sxs-lookup"><span data-stu-id="a9b09-153">Redis backplane</span></span>

<span data-ttu-id="a9b09-154">[Redis](https://redis.io/)是記憶體中的索引鍵/值存放區，可支援具有發行/訂閱模型的訊息系統。</span><span class="sxs-lookup"><span data-stu-id="a9b09-154">[Redis](https://redis.io/) is an in-memory key-value store that supports a messaging system with a publish/subscribe model.</span></span> <span data-ttu-id="a9b09-155">SignalRRedis 背板會使用 pub/sub 功能將訊息轉送至其他伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9b09-155">The SignalR Redis backplane uses the pub/sub feature to forward messages to other servers.</span></span> <span data-ttu-id="a9b09-156">當用戶端建立連線時，連線資訊會傳遞至背板。</span><span class="sxs-lookup"><span data-stu-id="a9b09-156">When a client makes a connection, the connection information is passed to the backplane.</span></span> <span data-ttu-id="a9b09-157">當伺服器想要將訊息傳送至所有用戶端時，它會傳送至背板。</span><span class="sxs-lookup"><span data-stu-id="a9b09-157">When a server wants to send a message to all clients, it sends to the backplane.</span></span> <span data-ttu-id="a9b09-158">背板會知道所有連線的用戶端，以及它們所在的伺服器。</span><span class="sxs-lookup"><span data-stu-id="a9b09-158">The backplane knows all connected clients and which servers they're on.</span></span> <span data-ttu-id="a9b09-159">它會透過其各自的伺服器，將訊息傳送至所有用戶端。</span><span class="sxs-lookup"><span data-stu-id="a9b09-159">It sends the message to all clients via their respective servers.</span></span> <span data-ttu-id="a9b09-160">下圖說明此程式：</span><span class="sxs-lookup"><span data-stu-id="a9b09-160">This process is illustrated in the following diagram:</span></span>

![Redis 背板，從一部伺服器傳送至所有用戶端的訊息](scale/_static/redis-backplane.png)

<span data-ttu-id="a9b09-162">對於裝載于您自己的基礎結構上的應用程式，Redis 背板是建議的向外延展方法。</span><span class="sxs-lookup"><span data-stu-id="a9b09-162">The Redis backplane is the recommended scale-out approach for apps hosted on your own infrastructure.</span></span> <span data-ttu-id="a9b09-163">如果您的資料中心與 Azure 資料中心之間有顯著的連線延遲，Azure SignalR 服務對於具有低延遲或高輸送量需求的內部部署應用程式而言，可能不是可行的選項。</span><span class="sxs-lookup"><span data-stu-id="a9b09-163">If there is significant connection latency between your data center and an Azure data center, Azure SignalR Service may not be a practical option for on-premises apps with low latency or high throughput requirements.</span></span>

<span data-ttu-id="a9b09-164">先前所 SignalR 述的 Azure 服務優點是 Redis 背板的缺點：</span><span class="sxs-lookup"><span data-stu-id="a9b09-164">The Azure SignalR Service advantages noted earlier are disadvantages for the Redis backplane:</span></span>

* <span data-ttu-id="a9b09-165">除了下列**兩**個條件都成立時以外，您還需要有粘滯話（也稱為[用戶端親和性](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity)）：</span><span class="sxs-lookup"><span data-stu-id="a9b09-165">Sticky sessions, also known as [client affinity](/iis/extensions/configuring-application-request-routing-arr/http-load-balancing-using-application-request-routing#step-3---configure-client-affinity), is required, except when **both** of the following are true:</span></span>
  * <span data-ttu-id="a9b09-166">所有用戶端都設定為**只**使用 websocket。</span><span class="sxs-lookup"><span data-stu-id="a9b09-166">All clients are configured to **only** use WebSockets.</span></span>
  * <span data-ttu-id="a9b09-167">用戶端設定中已啟用[SkipNegotiation 設定](xref:signalr/configuration#configure-additional-options)。</span><span class="sxs-lookup"><span data-stu-id="a9b09-167">The [SkipNegotiation setting](xref:signalr/configuration#configure-additional-options) is enabled in the client configuration.</span></span> 
   <span data-ttu-id="a9b09-168">一旦在伺服器上起始連接，連接就必須停留在該伺服器上。</span><span class="sxs-lookup"><span data-stu-id="a9b09-168">Once a connection is initiated on a server, the connection has to stay on that server.</span></span>
* <span data-ttu-id="a9b09-169">SignalR應用程式必須根據用戶端數目進行相應放大，即使傳送的訊息很少也一樣。</span><span class="sxs-lookup"><span data-stu-id="a9b09-169">A SignalR app must scale out based on number of clients even if few messages are being sent.</span></span>
* <span data-ttu-id="a9b09-170">SignalR應用程式在沒有的情況下，會使用比 web 應用程式更多的連接資源 SignalR 。</span><span class="sxs-lookup"><span data-stu-id="a9b09-170">A SignalR app uses significantly more connection resources than a web app without SignalR.</span></span>

## <a name="iis-limitations-on-windows-client-os"></a><span data-ttu-id="a9b09-171">Windows 用戶端作業系統上的 IIS 限制</span><span class="sxs-lookup"><span data-stu-id="a9b09-171">IIS limitations on Windows client OS</span></span>

<span data-ttu-id="a9b09-172">Windows 10 和 Windows 8.x 是用戶端作業系統。</span><span class="sxs-lookup"><span data-stu-id="a9b09-172">Windows 10 and Windows 8.x are client operating systems.</span></span> <span data-ttu-id="a9b09-173">用戶端作業系統上的 IIS 具有10個並行連線的限制。</span><span class="sxs-lookup"><span data-stu-id="a9b09-173">IIS on client operating systems has a limit of 10 concurrent connections.</span></span> <span data-ttu-id="a9b09-174">SignalR的連線如下：</span><span class="sxs-lookup"><span data-stu-id="a9b09-174">SignalR's connections are:</span></span>

* <span data-ttu-id="a9b09-175">暫時性且經常重新建立。</span><span class="sxs-lookup"><span data-stu-id="a9b09-175">Transient and frequently re-established.</span></span>
* <span data-ttu-id="a9b09-176">**不會**在不再使用時立即處置。</span><span class="sxs-lookup"><span data-stu-id="a9b09-176">**Not** disposed immediately when no longer used.</span></span>

<span data-ttu-id="a9b09-177">前述條件可能會達到用戶端作業系統上的10個連線限制。</span><span class="sxs-lookup"><span data-stu-id="a9b09-177">The preceding conditions make it likely to hit the 10 connection limit on a client OS.</span></span> <span data-ttu-id="a9b09-178">當用戶端 OS 用於開發時，我們建議：</span><span class="sxs-lookup"><span data-stu-id="a9b09-178">When a client OS is used for development, we recommend:</span></span>

* <span data-ttu-id="a9b09-179">避免 IIS。</span><span class="sxs-lookup"><span data-stu-id="a9b09-179">Avoid IIS.</span></span>
* <span data-ttu-id="a9b09-180">使用 Kestrel 或 IIS Express 作為部署目標。</span><span class="sxs-lookup"><span data-stu-id="a9b09-180">Use Kestrel or IIS Express as deployment targets.</span></span>

## <a name="linux-with-nginx"></a><span data-ttu-id="a9b09-181">使用 Nginx 的 Linux</span><span class="sxs-lookup"><span data-stu-id="a9b09-181">Linux with Nginx</span></span>

<span data-ttu-id="a9b09-182">針對 websocket，將 proxy 的 `Connection` 和 `Upgrade` 標頭設定為下列 SignalR 內容：</span><span class="sxs-lookup"><span data-stu-id="a9b09-182">Set the proxy's `Connection` and `Upgrade` headers to the following for SignalR WebSockets:</span></span>

```nginx
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection $connection_upgrade;
```

<span data-ttu-id="a9b09-183">如需詳細資訊，請參閱[NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/)。</span><span class="sxs-lookup"><span data-stu-id="a9b09-183">For more information, see [NGINX as a WebSocket Proxy](https://www.nginx.com/blog/websocket-nginx/).</span></span>

## <a name="third-party-no-locsignalr-backplane-providers"></a><span data-ttu-id="a9b09-184">協力廠商 SignalR 背板提供者</span><span class="sxs-lookup"><span data-stu-id="a9b09-184">Third-party SignalR backplane providers</span></span>

* [<span data-ttu-id="a9b09-185">NCache</span><span class="sxs-lookup"><span data-stu-id="a9b09-185">NCache</span></span>](https://www.alachisoft.com/ncache/asp-net-core-signalr.html)
* <span data-ttu-id="a9b09-186">[奧爾良](https://github.com/OrleansContrib/SignalR.Orleans)</span><span class="sxs-lookup"><span data-stu-id="a9b09-186">[Orleans](https://github.com/OrleansContrib/SignalR.Orleans)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a9b09-187">後續步驟</span><span class="sxs-lookup"><span data-stu-id="a9b09-187">Next steps</span></span>

<span data-ttu-id="a9b09-188">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="a9b09-188">For more information, see the following resources:</span></span>

* [<span data-ttu-id="a9b09-189">Azure SignalR 服務檔</span><span class="sxs-lookup"><span data-stu-id="a9b09-189">Azure SignalR Service documentation</span></span>](/azure/azure-signalr/signalr-overview)
* [<span data-ttu-id="a9b09-190">設定 Redis 背板</span><span class="sxs-lookup"><span data-stu-id="a9b09-190">Set up a Redis backplane</span></span>](xref:signalr/redis-backplane)
