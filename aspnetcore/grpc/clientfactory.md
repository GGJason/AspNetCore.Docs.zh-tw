---
title: .NET Core 中的 gRPC 用戶端 factory 整合
author: jamesnk
description: 瞭解如何使用用戶端 factory 建立 gRPC 用戶端。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 05/26/2020
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
uid: grpc/clientfactory
ms.openlocfilehash: 3ba7130f532777da72e3c84c9b0f6a375c2f1814
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88016254"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>.NET Core 中的 gRPC 用戶端 factory 整合

的 gRPC 整合 `HttpClientFactory` 提供了建立 gRPC 用戶端的集中方式。 它可以用來做為設定[獨立 gRPC 用戶端實例](xref:grpc/client)的替代方案。 Factory 整合可在[Grpc ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet 套件中取得。

Factory 提供下列優點：

* 提供設定邏輯 gRPC 用戶端實例的集中位置
* 管理基礎的存留期`HttpClientMessageHandler`
* 在 ASP.NET Core gRPC 服務中自動傳播期限和取消

## <a name="register-grpc-clients"></a>註冊 gRPC 用戶端

若要註冊 gRPC 用戶端， `AddGrpcClient` 可以在內使用泛型擴充方法 `Startup.ConfigureServices` ，並指定 gRPC 具類型的用戶端類別和服務位址：

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

GRPC 用戶端類型會使用 (DI) 的相依性插入來註冊為暫時性。 現在可以在 DI 所建立的類型中直接插入和取用用戶端。 ASP.NET Core MVC 控制器、 SignalR 中樞和 gRPC 服務都是可自動插入 gRPC 用戶端的地方：

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a>設定 HttpClient

`HttpClientFactory`建立 `HttpClient` gRPC 用戶端所使用的。 標準 `HttpClientFactory` 方法可以用來新增外寄要求中介軟體，或設定的基礎 `HttpClientHandler` `HttpClient` ：

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

如需詳細資訊，請參閱[使用 IHttpClientFactory 提出 HTTP 要求](xref:fundamentals/http-requests)。

## <a name="configure-channel-and-interceptors"></a>設定通道和攔截器

gRPC 特有的方法適用于：

* 設定 gRPC 用戶端的基礎通道。
* 新增 `Interceptor` 用戶端在進行 gRPC 呼叫時將使用的實例。

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>期限和取消傳播

在 gRPC 服務中，由處理站所建立的 gRPC 用戶端可以設定 `EnableCallContextPropagation()` 為，以自動將期限和取消權杖傳播至子呼叫。 `EnableCallContextPropagation()`擴充方法可在[Grpc. AspNetCore. ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet 套件中取得。

呼叫內容傳播的運作方式是從目前的 gRPC 要求內容讀取期限和解除標記，並自動將它們傳播至 gRPC 用戶端所發出的撥出電話。 呼叫內容傳播是確保複雜的嵌套 gRPC 案例一律會傳播期限和取消的絕佳方式。

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

根據預設， `EnableCallContextPropagation` 如果用戶端是在 gRPC 呼叫的內容之外使用，就會引發錯誤。 此錯誤是設計來警示您沒有要傳播的呼叫內容。 如果您想要在呼叫內容外使用用戶端，請在設定用戶端時，隱藏錯誤 `SuppressContextNotFoundErrors` ：

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation(o => o.SuppressContextNotFoundErrors = true);
```

如需有關期限和 RPC 取消的詳細資訊，請參閱[rpc 生命週期](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle)。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
