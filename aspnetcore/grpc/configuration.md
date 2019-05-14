---
title: ASP.NET Core 組態 gRPC
author: jamesnk
description: 了解如何設定 gRPC 的 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087333"
---
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="999d2-103">ASP.NET Core 組態 gRPC</span><span class="sxs-lookup"><span data-stu-id="999d2-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="999d2-104">設定服務選項</span><span class="sxs-lookup"><span data-stu-id="999d2-104">Configure services options</span></span>

<span data-ttu-id="999d2-105">下表說明設定 gRPC 服務的選項：</span><span class="sxs-lookup"><span data-stu-id="999d2-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="999d2-106">選項</span><span class="sxs-lookup"><span data-stu-id="999d2-106">Option</span></span> | <span data-ttu-id="999d2-107">預設值</span><span class="sxs-lookup"><span data-stu-id="999d2-107">Default Value</span></span> | <span data-ttu-id="999d2-108">描述</span><span class="sxs-lookup"><span data-stu-id="999d2-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="999d2-109">最大訊息大小可以從伺服器傳送的位元組數。</span><span class="sxs-lookup"><span data-stu-id="999d2-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="999d2-110">嘗試傳送的訊息長度超過設定的最大訊息大小會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="999d2-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="999d2-111">4 MB</span><span class="sxs-lookup"><span data-stu-id="999d2-111">4 MB</span></span> | <span data-ttu-id="999d2-112">最大訊息大小可以由伺服器接收的位元組數。</span><span class="sxs-lookup"><span data-stu-id="999d2-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="999d2-113">如果伺服器收到的訊息超過此限制，它會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="999d2-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="999d2-114">增加此值可讓伺服器接收較大的訊息，但可能會造成負面影響記憶體耗用量。</span><span class="sxs-lookup"><span data-stu-id="999d2-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="999d2-115">如果`true`詳細服務方法擲回例外狀況時，將會傳回給用戶端的例外狀況訊息。</span><span class="sxs-lookup"><span data-stu-id="999d2-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="999d2-116">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="999d2-116">The default is `false`.</span></span> <span data-ttu-id="999d2-117">這個設定設為`true`可能會導致洩漏機密資訊。</span><span class="sxs-lookup"><span data-stu-id="999d2-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="999d2-118">gzip</span><span class="sxs-lookup"><span data-stu-id="999d2-118">gzip</span></span> | <span data-ttu-id="999d2-119">壓縮來壓縮和解壓縮訊息使用的提供者集合。</span><span class="sxs-lookup"><span data-stu-id="999d2-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="999d2-120">可以建立自訂壓縮提供者，並加入至集合。</span><span class="sxs-lookup"><span data-stu-id="999d2-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="999d2-121">預設值設定提供者會支援**gzip**壓縮。</span><span class="sxs-lookup"><span data-stu-id="999d2-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="999d2-122">壓縮演算法，用來壓縮從伺服器傳送的訊息。</span><span class="sxs-lookup"><span data-stu-id="999d2-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="999d2-123">Algorithm 必須符合壓縮提供者中的`CompressionProviders`。</span><span class="sxs-lookup"><span data-stu-id="999d2-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="999d2-124">壓縮回應 algorthm 用戶端必須指出它傳送給它，以支援的演算法**grpc-接受編碼**標頭。</span><span class="sxs-lookup"><span data-stu-id="999d2-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="999d2-125">會壓縮從伺服器傳送的訊息所使用的壓縮層級。</span><span class="sxs-lookup"><span data-stu-id="999d2-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="999d2-126">可以為所有服務設定選項，藉由提供選項委派`AddGrpc`呼叫中`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="999d2-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="999d2-127">單一服務的選項會覆寫中提供的全域選項`AddGrpc`，並可使用設定`AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="999d2-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a><span data-ttu-id="999d2-128">設定 Kestrel 選項</span><span class="sxs-lookup"><span data-stu-id="999d2-128">Configure Kestrel options</span></span>

<span data-ttu-id="999d2-129">Kestrel 伺服器的組態選項，asp.net 會影響 gRPC 的行為。</span><span class="sxs-lookup"><span data-stu-id="999d2-129">Kestrel server has configuration options that affect the behavior of gRPC for ASP.NET.</span></span>

### <a name="request-body-data-rate-limit"></a><span data-ttu-id="999d2-130">要求主體資料速率限制</span><span class="sxs-lookup"><span data-stu-id="999d2-130">Request body data rate limit</span></span>

<span data-ttu-id="999d2-131">根據預設，Kestrel 伺服器加諸[要求主體資料速率下限](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>)。</span><span class="sxs-lookup"><span data-stu-id="999d2-131">By default, the Kestrel server imposes a [minimum request body data rate](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>).</span></span> <span data-ttu-id="999d2-132">用戶端串流處理和串流處理呼叫的雙工，可能不符合此速率，並連接可能會逾時。最小要求內文資料流的用戶端和資料流處理呼叫的雙工，包括 gRPC 服務時，必須停用資料速率限制：</span><span class="sxs-lookup"><span data-stu-id="999d2-132">For client streaming and duplex streaming calls, this rate may not be satisfied and the connection may be timed out. The minimum request body data rate limit must be disabled when the gRPC service includes client streaming and duplex streaming calls:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a><span data-ttu-id="999d2-133">其他資源</span><span class="sxs-lookup"><span data-stu-id="999d2-133">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>