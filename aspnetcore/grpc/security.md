---
title: ASP.NET Core 的 gRPC 中的安全性考慮
author: jamesnk
description: 瞭解 gRPC for ASP.NET Core 的安全性考慮。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
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
uid: grpc/security
ms.openlocfilehash: 4c805ab1e01b4c70644a12c277441d1d242ce720
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88015994"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a><span data-ttu-id="2594c-103">ASP.NET Core 的 gRPC 中的安全性考慮</span><span class="sxs-lookup"><span data-stu-id="2594c-103">Security considerations in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="2594c-104">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="2594c-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="2594c-105">本文提供使用 .NET Core 保護 gRPC 的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="2594c-105">This article provides information on securing gRPC with .NET Core.</span></span>

## <a name="transport-security"></a><span data-ttu-id="2594c-106">傳輸安全性</span><span class="sxs-lookup"><span data-stu-id="2594c-106">Transport security</span></span>

<span data-ttu-id="2594c-107">gRPC 訊息會使用 HTTP/2 進行傳送和接收。</span><span class="sxs-lookup"><span data-stu-id="2594c-107">gRPC messages are sent and received using HTTP/2.</span></span> <span data-ttu-id="2594c-108">我們建議：</span><span class="sxs-lookup"><span data-stu-id="2594c-108">We recommend:</span></span>

* <span data-ttu-id="2594c-109">[傳輸層安全性 (TLS) ](https://tools.ietf.org/html/rfc5246)用來保護生產 gRPC 應用程式中的訊息。</span><span class="sxs-lookup"><span data-stu-id="2594c-109">[Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) be used to secure messages in production gRPC apps.</span></span>
* <span data-ttu-id="2594c-110">gRPC 服務應該只接聽並回應安全的埠。</span><span class="sxs-lookup"><span data-stu-id="2594c-110">gRPC services should only listen and respond over secured ports.</span></span>

<span data-ttu-id="2594c-111">TLS 是在 Kestrel 中設定。</span><span class="sxs-lookup"><span data-stu-id="2594c-111">TLS is configured in Kestrel.</span></span> <span data-ttu-id="2594c-112">如需設定 Kestrel 端點的詳細資訊，請參閱[Kestrel 端點](xref:fundamentals/servers/kestrel#endpoint-configuration)設定。</span><span class="sxs-lookup"><span data-stu-id="2594c-112">For more information on configuring Kestrel endpoints, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="exceptions"></a><span data-ttu-id="2594c-113">例外狀況</span><span class="sxs-lookup"><span data-stu-id="2594c-113">Exceptions</span></span>

<span data-ttu-id="2594c-114">例外狀況訊息通常會被視為不應該向用戶端顯示的敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="2594c-114">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="2594c-115">根據預設，gRPC 不會將 gRPC 服務擲回之例外狀況的詳細資料傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="2594c-115">By default, gRPC doesn't send the details of an exception thrown by a gRPC service to the client.</span></span> <span data-ttu-id="2594c-116">相反地，用戶端會收到一般訊息，指出發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="2594c-116">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="2594c-117">可以覆寫用戶端的例外狀況訊息傳遞 (例如，在使用[EnableDetailedErrors](xref:grpc/configuration#configure-services-options)的開發或測試) 中。</span><span class="sxs-lookup"><span data-stu-id="2594c-117">Exception message delivery to the client can be overridden (for example, in development or test) with [EnableDetailedErrors](xref:grpc/configuration#configure-services-options).</span></span> <span data-ttu-id="2594c-118">例外狀況訊息不應在生產環境應用程式中公開給用戶端。</span><span class="sxs-lookup"><span data-stu-id="2594c-118">Exception messages shouldn't be exposed to the client in production apps.</span></span>

## <a name="message-size-limits"></a><span data-ttu-id="2594c-119">訊息大小限制</span><span class="sxs-lookup"><span data-stu-id="2594c-119">Message size limits</span></span>

<span data-ttu-id="2594c-120">傳入訊息至 gRPC 用戶端和服務會載入記憶體中。</span><span class="sxs-lookup"><span data-stu-id="2594c-120">Incoming messages to gRPC clients and services are loaded into memory.</span></span> <span data-ttu-id="2594c-121">訊息大小限制是一種機制，可協助防止 gRPC 耗用過多的資源。</span><span class="sxs-lookup"><span data-stu-id="2594c-121">Message size limits are a mechanism to help prevent gRPC from consuming excessive resources.</span></span>

<span data-ttu-id="2594c-122">gRPC 會使用每個訊息的大小限制來管理傳入和傳出訊息。</span><span class="sxs-lookup"><span data-stu-id="2594c-122">gRPC uses per-message size limits to manage incoming and outgoing messages.</span></span> <span data-ttu-id="2594c-123">根據預設，gRPC 會將傳入訊息限制為 4 MB。</span><span class="sxs-lookup"><span data-stu-id="2594c-123">By default, gRPC limits incoming messages to 4 MB.</span></span> <span data-ttu-id="2594c-124">外寄訊息沒有任何限制。</span><span class="sxs-lookup"><span data-stu-id="2594c-124">There is no limit on outgoing messages.</span></span>

<span data-ttu-id="2594c-125">在伺服器上，可以使用下列方式為應用程式中的所有服務設定 gRPC 訊息限制 `AddGrpc` ：</span><span class="sxs-lookup"><span data-stu-id="2594c-125">On the server, gRPC message limits can be configured for all services in an app with `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 1 * 1024 * 1024; // 1 MB
        options.MaxSendMessageSize = 1 * 1024 * 1024; // 1 MB
    });
}
```

<span data-ttu-id="2594c-126">您也可以使用來設定個別服務的限制 `AddServiceOptions<TService>` 。</span><span class="sxs-lookup"><span data-stu-id="2594c-126">Limits can also be configured for an individual service using `AddServiceOptions<TService>`.</span></span> <span data-ttu-id="2594c-127">如需設定訊息大小限制的詳細資訊，請參閱[gRPC configuration](xref:grpc/configuration)。</span><span class="sxs-lookup"><span data-stu-id="2594c-127">For more information on configuring message size limits, see [gRPC configuration](xref:grpc/configuration).</span></span>

## <a name="client-certificate-validation"></a><span data-ttu-id="2594c-128">用戶端憑證驗證</span><span class="sxs-lookup"><span data-stu-id="2594c-128">Client certificate validation</span></span>

<span data-ttu-id="2594c-129">建立連接時，一開始會驗證[用戶端憑證](https://tools.ietf.org/html/rfc5246#section-7.4.4)。</span><span class="sxs-lookup"><span data-stu-id="2594c-129">[Client certificates](https://tools.ietf.org/html/rfc5246#section-7.4.4) are initially validated when the connection is established.</span></span> <span data-ttu-id="2594c-130">根據預設，Kestrel 不會對連接的用戶端憑證執行額外的驗證。</span><span class="sxs-lookup"><span data-stu-id="2594c-130">By default, Kestrel doesn't perform additional validation of a connection's client certificate.</span></span>

<span data-ttu-id="2594c-131">我們建議以用戶端憑證保護的 gRPC 服務使用[AspNetCore 憑證](xref:security/authentication/certauth)封裝。</span><span class="sxs-lookup"><span data-stu-id="2594c-131">We recommend that gRPC services secured by client certificates use the [Microsoft.AspNetCore.Authentication.Certificate](xref:security/authentication/certauth) package.</span></span> <span data-ttu-id="2594c-132">ASP.NET Core 認證驗證將在用戶端憑證上執行額外的驗證，包括：</span><span class="sxs-lookup"><span data-stu-id="2594c-132">ASP.NET Core certification authentication will perform additional validation on a client certificate, including:</span></span>

* <span data-ttu-id="2594c-133">憑證具有有效的擴充金鑰使用 (EKU) </span><span class="sxs-lookup"><span data-stu-id="2594c-133">Certificate has a valid extended key use (EKU)</span></span>
* <span data-ttu-id="2594c-134">在其有效期間內</span><span class="sxs-lookup"><span data-stu-id="2594c-134">Is within its validity period</span></span>
* <span data-ttu-id="2594c-135">檢查憑證撤銷</span><span class="sxs-lookup"><span data-stu-id="2594c-135">Check certificate revocation</span></span>
