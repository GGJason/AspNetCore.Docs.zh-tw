---
title: 利用 .NET 用戶端呼叫 gRPC 服務
author: jamesnk
description: 瞭解如何使用 .NET gRPC 用戶端呼叫 gRPC 服務。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 07/27/2020
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
uid: grpc/client
ms.openlocfilehash: 5aca81da34e5ed51b2dc4f404c1ba4d7377a422f
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88016241"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="07abb-103">利用 .NET 用戶端呼叫 gRPC 服務</span><span class="sxs-lookup"><span data-stu-id="07abb-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="07abb-104">.NET gRPC 用戶端程式庫可在[gRPC .net. 用戶端](https://www.nuget.org/packages/Grpc.Net.Client)NuGet 套件中取得。</span><span class="sxs-lookup"><span data-stu-id="07abb-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="07abb-105">本檔說明如何：</span><span class="sxs-lookup"><span data-stu-id="07abb-105">This document explains how to:</span></span>

* <span data-ttu-id="07abb-106">將 gRPC 用戶端設定為呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="07abb-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="07abb-107">對一元、伺服器資料流程、用戶端資料流程和雙向串流方法進行 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="07abb-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="07abb-108">設定 gRPC 用戶端</span><span class="sxs-lookup"><span data-stu-id="07abb-108">Configure gRPC client</span></span>

<span data-ttu-id="07abb-109">gRPC 用戶端是[從\* \* proto\*檔案產生](xref:grpc/basics#generated-c-assets)的具體用戶端類型。</span><span class="sxs-lookup"><span data-stu-id="07abb-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="07abb-110">具體的 gRPC 用戶端具有轉譯為\* \* proto\*檔案中 gRPC 服務的方法。</span><span class="sxs-lookup"><span data-stu-id="07abb-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="07abb-111">GRPC 用戶端是從通道建立的。</span><span class="sxs-lookup"><span data-stu-id="07abb-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="07abb-112">首先，使用 `GrpcChannel.ForAddress` 來建立通道，然後使用通道來建立 gRPC 用戶端：</span><span class="sxs-lookup"><span data-stu-id="07abb-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="07abb-113">通道代表 gRPC 服務的長時間連接。</span><span class="sxs-lookup"><span data-stu-id="07abb-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="07abb-114">建立通道時，會使用與呼叫服務相關的選項進行設定。</span><span class="sxs-lookup"><span data-stu-id="07abb-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="07abb-115">例如， `HttpClient` 用來進行呼叫的、傳送和接收訊息大小上限，以及記錄可以在上指定並搭配 `GrpcChannelOptions` 使用 `GrpcChannel.ForAddress` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="07abb-116">如需完整的選項清單，請參閱[用戶端設定選項](xref:grpc/configuration#configure-client-options)。</span><span class="sxs-lookup"><span data-stu-id="07abb-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

### <a name="configure-tls"></a><span data-ttu-id="07abb-117">設定 TLS</span><span class="sxs-lookup"><span data-stu-id="07abb-117">Configure TLS</span></span>

<span data-ttu-id="07abb-118">GRPC 用戶端必須使用與所呼叫服務相同的連接層級安全性。</span><span class="sxs-lookup"><span data-stu-id="07abb-118">A gRPC client must use the same connection-level security as the called service.</span></span> <span data-ttu-id="07abb-119">gRPC 用戶端傳輸層安全性 (TLS) 會在建立 gRPC 通道時設定。</span><span class="sxs-lookup"><span data-stu-id="07abb-119">gRPC client Transport Layer Security (TLS) is configured when the gRPC channel is created.</span></span> <span data-ttu-id="07abb-120">當 gRPC 用戶端呼叫服務，且通道和服務的連線層級安全性不相符時，就會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="07abb-120">A gRPC client throws an error when it calls a service and the connection-level security of the channel and service don't match.</span></span>

<span data-ttu-id="07abb-121">若要將 gRPC 通道設定為使用 TLS，請確定伺服器位址的開頭為 `https` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-121">To configure a gRPC channel to use TLS, ensure the server address starts with `https`.</span></span> <span data-ttu-id="07abb-122">例如，會 `GrpcChannel.ForAddress("https://localhost:5001")` 使用 HTTPS 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="07abb-122">For example, `GrpcChannel.ForAddress("https://localhost:5001")` uses HTTPS protocol.</span></span> <span data-ttu-id="07abb-123">GRPC 通道會自動 negotates TLS 所保護的連接，並使用安全連線來進行 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="07abb-123">The gRPC channel automatically negotates a connection secured by TLS and uses a secure connection to make gRPC calls.</span></span>

> [!TIP]
> <span data-ttu-id="07abb-124">gRPC 支援透過 TLS 的用戶端憑證驗證。</span><span class="sxs-lookup"><span data-stu-id="07abb-124">gRPC supports client certificate authentication over TLS.</span></span> <span data-ttu-id="07abb-125">如需使用 gRPC 通道來設定用戶端憑證的詳細資訊，請參閱 <xref:grpc/authn-and-authz#client-certificate-authentication> 。</span><span class="sxs-lookup"><span data-stu-id="07abb-125">For information on configuring client certificates with a gRPC channel, see <xref:grpc/authn-and-authz#client-certificate-authentication>.</span></span>

<span data-ttu-id="07abb-126">若要呼叫不安全的 gRPC 服務，請確定伺服器位址的開頭為 `http` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-126">To call unsecured gRPC services, ensure the server address starts with `http`.</span></span> <span data-ttu-id="07abb-127">例如，會 `GrpcChannel.ForAddress("http://localhost:5000")` 使用 HTTP 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="07abb-127">For example, `GrpcChannel.ForAddress("http://localhost:5000")` uses HTTP protocol.</span></span> <span data-ttu-id="07abb-128">在 .NET Core 3.1 或更新版本中，需要額外的設定，才能[使用 .net 用戶端呼叫不安全的 gRPC 服務](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)。</span><span class="sxs-lookup"><span data-stu-id="07abb-128">In .NET Core 3.1 or later, additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

### <a name="client-performance"></a><span data-ttu-id="07abb-129">用戶端效能</span><span class="sxs-lookup"><span data-stu-id="07abb-129">Client performance</span></span>

<span data-ttu-id="07abb-130">通道和用戶端效能和使用方式：</span><span class="sxs-lookup"><span data-stu-id="07abb-130">Channel and client performance and usage:</span></span>

* <span data-ttu-id="07abb-131">建立通道可能是昂貴的作業。</span><span class="sxs-lookup"><span data-stu-id="07abb-131">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="07abb-132">重複使用 gRPC 呼叫的通道可提供效能優勢。</span><span class="sxs-lookup"><span data-stu-id="07abb-132">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="07abb-133">系統會使用通道來建立 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="07abb-133">gRPC clients are created with channels.</span></span> <span data-ttu-id="07abb-134">gRPC 用戶端是輕量物件，不需要快取或重複使用。</span><span class="sxs-lookup"><span data-stu-id="07abb-134">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="07abb-135">您可以從通道建立多個 gRPC 用戶端，包括不同類型的用戶端。</span><span class="sxs-lookup"><span data-stu-id="07abb-135">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="07abb-136">通道和從通道建立的用戶端可以安全地由多個執行緒使用。</span><span class="sxs-lookup"><span data-stu-id="07abb-136">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="07abb-137">從通道建立的用戶端可以進行多個同時呼叫。</span><span class="sxs-lookup"><span data-stu-id="07abb-137">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="07abb-138">`GrpcChannel.ForAddress`不是建立 gRPC 用戶端的唯一選項。</span><span class="sxs-lookup"><span data-stu-id="07abb-138">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="07abb-139">如果從 ASP.NET Core 應用程式呼叫 gRPC services，請考慮[gRPC 用戶端 factory 整合](xref:grpc/clientfactory)。</span><span class="sxs-lookup"><span data-stu-id="07abb-139">If calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="07abb-140">gRPC 與的整合 `HttpClientFactory` 提供了建立 gRPC 用戶端的集中式替代方案。</span><span class="sxs-lookup"><span data-stu-id="07abb-140">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="07abb-141">Xamarin 目前不支援透過 HTTP/2 呼叫 gRPC `Grpc.Net.Client` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-141">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="07abb-142">我們正致力於改善未來 Xamarin 版本中的 HTTP/2 支援。</span><span class="sxs-lookup"><span data-stu-id="07abb-142">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="07abb-143">[Grpc](https://www.nuget.org/packages/Grpc.Core)和[Grpc-Web](xref:grpc/browser)是可行的替代方案。</span><span class="sxs-lookup"><span data-stu-id="07abb-143">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="07abb-144">進行 gRPC 呼叫</span><span class="sxs-lookup"><span data-stu-id="07abb-144">Make gRPC calls</span></span>

<span data-ttu-id="07abb-145">GRPC 呼叫是藉由呼叫用戶端上的方法來起始。</span><span class="sxs-lookup"><span data-stu-id="07abb-145">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="07abb-146">GRPC 用戶端會處理訊息序列化，並將 gRPC 呼叫定址至正確的服務。</span><span class="sxs-lookup"><span data-stu-id="07abb-146">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="07abb-147">gRPC 有不同類型的方法。</span><span class="sxs-lookup"><span data-stu-id="07abb-147">gRPC has different types of methods.</span></span> <span data-ttu-id="07abb-148">如何使用用戶端進行 gRPC 呼叫，取決於呼叫的方法類型。</span><span class="sxs-lookup"><span data-stu-id="07abb-148">How the client is used to make a gRPC call depends on the type of method called.</span></span> <span data-ttu-id="07abb-149">GRPC 方法類型為：</span><span class="sxs-lookup"><span data-stu-id="07abb-149">The gRPC method types are:</span></span>

* <span data-ttu-id="07abb-150">一元 (Unary)</span><span class="sxs-lookup"><span data-stu-id="07abb-150">Unary</span></span>
* <span data-ttu-id="07abb-151">伺服器串流</span><span class="sxs-lookup"><span data-stu-id="07abb-151">Server streaming</span></span>
* <span data-ttu-id="07abb-152">用戶端串流</span><span class="sxs-lookup"><span data-stu-id="07abb-152">Client streaming</span></span>
* <span data-ttu-id="07abb-153">雙向串流</span><span class="sxs-lookup"><span data-stu-id="07abb-153">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="07abb-154">一元呼叫</span><span class="sxs-lookup"><span data-stu-id="07abb-154">Unary call</span></span>

<span data-ttu-id="07abb-155">一元呼叫會從傳送要求訊息的用戶端開始。</span><span class="sxs-lookup"><span data-stu-id="07abb-155">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="07abb-156">當服務完成時，就會傳迴響應消息。</span><span class="sxs-lookup"><span data-stu-id="07abb-156">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="07abb-157">\* \* Proto\*檔案中的每個一元服務方法都會在具象 gRPC 用戶端類型上產生兩個 .net 方法，以呼叫方法：非同步方法和封鎖方法。</span><span class="sxs-lookup"><span data-stu-id="07abb-157">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="07abb-158">例如， `GreeterClient` 有兩種方法可以呼叫 `SayHello` ：</span><span class="sxs-lookup"><span data-stu-id="07abb-158">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="07abb-159">`GreeterClient.SayHelloAsync`- `Greeter.SayHello` 以非同步方式呼叫服務。</span><span class="sxs-lookup"><span data-stu-id="07abb-159">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="07abb-160">可以等候。</span><span class="sxs-lookup"><span data-stu-id="07abb-160">Can be awaited.</span></span>
* <span data-ttu-id="07abb-161">`GreeterClient.SayHello`-呼叫 `Greeter.SayHello` 服務並封鎖直到完成為止。</span><span class="sxs-lookup"><span data-stu-id="07abb-161">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="07abb-162">不要在非同步程式碼中使用。</span><span class="sxs-lookup"><span data-stu-id="07abb-162">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="07abb-163">伺服器串流呼叫</span><span class="sxs-lookup"><span data-stu-id="07abb-163">Server streaming call</span></span>

<span data-ttu-id="07abb-164">伺服器串流呼叫會從傳送要求訊息的用戶端開始。</span><span class="sxs-lookup"><span data-stu-id="07abb-164">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="07abb-165">`ResponseStream.MoveNext()`讀取從服務資料流程處理的訊息。</span><span class="sxs-lookup"><span data-stu-id="07abb-165">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="07abb-166">當傳回時，會完成伺服器資料流程呼叫 `ResponseStream.MoveNext()` `false` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-166">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

while (await call.ResponseStream.MoveNext())
{
    Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
    // "Greeting: Hello World" is written multiple times
}
```

<span data-ttu-id="07abb-167">使用 c # 8 或更新版本時， `await foreach` 語法可以用來讀取訊息。</span><span class="sxs-lookup"><span data-stu-id="07abb-167">When using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="07abb-168">`IAsyncStreamReader<T>.ReadAllAsync()`擴充方法會讀取來自回應資料流程的所有訊息：</span><span class="sxs-lookup"><span data-stu-id="07abb-168">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="07abb-169">用戶端串流呼叫</span><span class="sxs-lookup"><span data-stu-id="07abb-169">Client streaming call</span></span>

<span data-ttu-id="07abb-170">用戶端串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。</span><span class="sxs-lookup"><span data-stu-id="07abb-170">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="07abb-171">用戶端可以選擇用來傳送訊息 `RequestStream.WriteAsync` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-171">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="07abb-172">當用戶端完成傳送訊息時， `RequestStream.CompleteAsync` 應該呼叫來通知服務。</span><span class="sxs-lookup"><span data-stu-id="07abb-172">When the client has finished sending messages, `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="07abb-173">當服務傳迴響應消息時，就會完成呼叫。</span><span class="sxs-lookup"><span data-stu-id="07abb-173">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using var call = client.AccumulateCount();

for (var i = 0; i < 3; i++)
{
    await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
}
await call.RequestStream.CompleteAsync();

var response = await call;
Console.WriteLine($"Count: {response.Count}");
// Count: 3
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="07abb-174">雙向串流呼叫</span><span class="sxs-lookup"><span data-stu-id="07abb-174">Bi-directional streaming call</span></span>

<span data-ttu-id="07abb-175">雙向串流呼叫會在沒有用戶端傳送訊息的*情況下*啟動。</span><span class="sxs-lookup"><span data-stu-id="07abb-175">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="07abb-176">用戶端可以選擇用來傳送訊息 `RequestStream.WriteAsync` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-176">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="07abb-177">從服務串流處理的訊息可以透過 `ResponseStream.MoveNext()` 或來存取 `ResponseStream.ReadAllAsync()` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-177">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="07abb-178">當沒有任何訊息時，雙向串流呼叫會完成 `ResponseStream` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-178">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
var client = new Echo.EchoClient(channel);
using var call = client.Echo();

Console.WriteLine("Starting background task to receive messages");
var readTask = Task.Run(async () =>
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine(response.Message);
        // Echo messages sent to the service
    }
});

Console.WriteLine("Starting to send messages");
Console.WriteLine("Type a message to echo then press enter.");
while (true)
{
    var result = Console.ReadLine();
    if (string.IsNullOrEmpty(result))
    {
        break;
    }

    await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
}

Console.WriteLine("Disconnecting");
await call.RequestStream.CompleteAsync();
await readTask;
```

<span data-ttu-id="07abb-179">在雙向串流呼叫期間，用戶端和服務可以隨時傳送訊息給彼此。</span><span class="sxs-lookup"><span data-stu-id="07abb-179">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="07abb-180">與雙向呼叫互動的最佳用戶端邏輯會根據服務邏輯而有所不同。</span><span class="sxs-lookup"><span data-stu-id="07abb-180">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="access-grpc-trailers"></a><span data-ttu-id="07abb-181">存取權 gRPC 尾端</span><span class="sxs-lookup"><span data-stu-id="07abb-181">Access gRPC trailers</span></span>

<span data-ttu-id="07abb-182">gRPC 呼叫可能會傳回 gRPC 尾端。</span><span class="sxs-lookup"><span data-stu-id="07abb-182">gRPC calls may return gRPC trailers.</span></span> <span data-ttu-id="07abb-183">gRPC 尾端用來提供關於呼叫的名稱/值中繼資料。</span><span class="sxs-lookup"><span data-stu-id="07abb-183">gRPC trailers are used to provide name/value metadata about a call.</span></span> <span data-ttu-id="07abb-184">尾端提供類似于 HTTP 標頭的功能，但會在通話結束時收到。</span><span class="sxs-lookup"><span data-stu-id="07abb-184">Trailers provide similar functionality to HTTP headers, but are received at the end of the call.</span></span>

<span data-ttu-id="07abb-185">gRPC 尾端可使用來存取 `GetTrailers()` ，這會傳回中繼資料的集合。</span><span class="sxs-lookup"><span data-stu-id="07abb-185">gRPC trailers are accessible using `GetTrailers()`, which returns a collection of metadata.</span></span> <span data-ttu-id="07abb-186">在回應完成後會傳回結尾，因此，您必須在存取結尾之前等待所有回應訊息。</span><span class="sxs-lookup"><span data-stu-id="07abb-186">Trailers are returned after the response is complete, therefore, you must await all response messages before accessing the trailers.</span></span>

<span data-ttu-id="07abb-187">在呼叫之前，一元和用戶端資料流程呼叫必須等候 `ResponseAsync` `GetTrailers()` ：</span><span class="sxs-lookup"><span data-stu-id="07abb-187">Unary and client streaming calls must await `ResponseAsync` before calling `GetTrailers()`:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
var response = await call.ResponseAsync;

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World

var trailers = call.GetTrailers();
var myValue = trailers.GetValue("my-trailer-name");
```

<span data-ttu-id="07abb-188">在呼叫之前，伺服器和雙向串流呼叫必須完成等待回應資料流程的作業 `GetTrailers()` ：</span><span class="sxs-lookup"><span data-stu-id="07abb-188">Server and bidirectional streaming calls must finish awaiting the response stream before calling `GetTrailers()`:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using var call = client.SayHellos(new HelloRequest { Name = "World" });

await foreach (var response in call.ResponseStream.ReadAllAsync())
{
    Console.WriteLine("Greeting: " + response.Message);
    // "Greeting: Hello World" is written multiple times
}

var trailers = call.GetTrailers();
var myValue = trailers.GetValue("my-trailer-name");
```

<span data-ttu-id="07abb-189">gRPC 尾端也可以從存取 `RpcException` 。</span><span class="sxs-lookup"><span data-stu-id="07abb-189">gRPC trailers are also accessible from `RpcException`.</span></span> <span data-ttu-id="07abb-190">服務可能會傳回尾端和非 OK gRPC 狀態。</span><span class="sxs-lookup"><span data-stu-id="07abb-190">A service may return trailers together with a non-OK gRPC status.</span></span> <span data-ttu-id="07abb-191">在此情況下，會從 gRPC 用戶端擲回的例外狀況中抓取尾端：</span><span class="sxs-lookup"><span data-stu-id="07abb-191">In this situation the trailers are retrieved from the exception thrown by the gRPC client:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
string myValue = null;

try
{
    using var call = client.SayHelloAsync(new HelloRequest { Name = "World" });
    var response = await call.ResponseAsync;

    Console.WriteLine("Greeting: " + response.Message);
    // Greeting: Hello World

    var trailers = call.GetTrailers();
    myValue = trailers.GetValue("my-trailer-name");
}
catch (RpcException ex)
{
    var trailers = ex.Trailers;
    myValue = trailers.GetValue("my-trailer-name");
}
```

## <a name="additional-resources"></a><span data-ttu-id="07abb-192">其他資源</span><span class="sxs-lookup"><span data-stu-id="07abb-192">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
