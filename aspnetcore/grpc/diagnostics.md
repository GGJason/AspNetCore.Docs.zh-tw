---
title: 在 .NET 上 gRPC 中的記錄和診斷
author: jamesnk
description: 瞭解如何從 .NET 上的 gRPC 應用程式收集診斷資訊。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: grpc/diagnostics
ms.openlocfilehash: 1f25ae76e5a480e5e6f247e4ac78d06dd4e778e9
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93060439"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>在 .NET 上 gRPC 中的記錄和診斷

依 [James 牛頓](https://twitter.com/jamesnk)

本文提供從 gRPC 應用程式收集診斷資訊，以協助疑難排解問題的指引。 涵蓋的主題包括：

* **記錄** -寫入 [.net Core 記錄](xref:fundamentals/logging/index)的結構化記錄。 <xref:Microsoft.Extensions.Logging.ILogger> 應用程式架構會使用它來寫入記錄，以及讓使用者在應用程式中進行自己的記錄。
* **追蹤** -與使用和撰寫之作業相關的事件 `DiaganosticSource` `Activity` 。 診斷來源的追蹤通常用來依程式庫（例如 [Application Insights](/azure/azure-monitor/app/asp-net-core) 和 [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet)）收集應用程式遙測。
* **計量** -依時間間隔表示的資料量值，例如每秒要求數。 計量是使用發出的 `EventCounter` ，可使用 [dotnet 計數器](/dotnet/core/diagnostics/dotnet-counters) 命令列工具或 [Application Insights](/azure/azure-monitor/app/eventcounters)來觀察。

## <a name="logging"></a>記錄

gRPC services 和 gRPC 用戶端會使用 [.Net Core 記錄](xref:fundamentals/logging/index)來寫入記錄。 當您需要在應用程式中偵測非預期的行為時，記錄是很好的開始位置。

### <a name="grpc-services-logging"></a>gRPC services 記錄

> [!WARNING]
> 伺服器端記錄檔可能包含來自您應用程式的機密資訊。 **切勿** 將未經處理的記錄從生產環境應用程式張貼到 GitHub 之類的公用論壇。

由於 gRPC 服務是裝載在 ASP.NET Core 上，因此會使用 ASP.NET Core 記錄系統。 在預設設定中，gRPC 會記錄極少的資訊，但這可以設定。 如需有關設定 ASP.NET Core 記錄的詳細資訊，請參閱 [ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration) 檔。

gRPC 會在類別下新增記錄 `Grpc` 。 若要啟用 gRPC 的詳細記錄，請 `Grpc` `Debug` 將下列專案新增至您檔案中的層級， *appsettings.json* 方法是將下列專案新增至 `LogLevel` 中的子區段 `Logging` ：

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

您也可以在 *Startup.cs* 中使用下列方式進行設定 `ConfigureLogging` ：

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

如果您未使用以 JSON 為基礎的設定，請在設定系統中設定下列設定值：

* `Logging:LogLevel:Grpc` = `Debug`

請檢查設定系統的檔，以判斷如何指定嵌套設定值。 例如，使用環境變數時， `_` 會使用兩個字元，而不是 `:` (例如 `Logging__LogLevel__Grpc`) 。

`Debug`針對您的應用程式收集更詳細的診斷時，建議使用此層級。 `Trace`層級會產生非常低層級的診斷，而且很少需要診斷您應用程式中的問題。

#### <a name="sample-logging-output"></a>範例記錄輸出

以下是 `Debug` gRPC 服務層級的主控台輸出範例：

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

### <a name="access-server-side-logs"></a>存取伺服器端記錄

您存取伺服器端記錄的方式取決於您執行的環境。

#### <a name="as-a-console-app"></a>作為主控台應用程式

如果您是在主控台應用程式中執行，則預設應啟用 [主控台記錄器](xref:fundamentals/logging/index#console) 。 gRPC 記錄將會出現在主控台中。

#### <a name="other-environments"></a>其他環境

如果應用程式部署到另一個環境 (例如 Docker、Kubernetes 或 Windows 服務) ，請參閱， <xref:fundamentals/logging/index> 以取得如何設定適用于環境的記錄提供者的詳細資訊。

### <a name="grpc-client-logging"></a>gRPC 用戶端記錄

> [!WARNING]
> 用戶端記錄可能包含來自您應用程式的機密資訊。 **切勿** 將未經處理的記錄從生產環境應用程式張貼到 GitHub 之類的公用論壇。

若要從 .NET 用戶端取得記錄，您可以 `GrpcChannelOptions.LoggerFactory` 在建立用戶端通道時設定屬性。 如果您是從 ASP.NET Core 應用程式呼叫 gRPC 服務，則可以從 (DI) 的相依性插入來解析記錄器 factory：

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

啟用用戶端記錄的替代方式是使用 [gRPC 用戶端 factory](xref:grpc/clientfactory) 來建立用戶端。 向用戶端處理站註冊並從 DI 解析的 gRPC 用戶端會自動使用應用程式設定的記錄。

如果您的應用程式不使用 DI，則可以 `ILoggerFactory` 使用 [server.loggerfactory](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*)建立新的實例。 若要存取此方法，請將 [Microsoft Extensions. 記錄](https://www.nuget.org/packages/microsoft.extensions.logging/) 套件新增至您的應用程式。

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a>gRPC 用戶端記錄範圍

GRPC 用戶端會將 [記錄範圍](../fundamentals/logging/index.md#log-scopes) 新增至在 gRPC 呼叫期間進行的記錄。 此範圍具有與 gRPC 呼叫相關的中繼資料：

* **GrpcMethodType** -gRPC 方法類型。 可能的值是列舉的名稱 `Grpc.Core.MethodType` ，例如一元
* **GrpcUri** -gRPC 方法的相對 URI，例如/greet。Greeter/SayHellos

#### <a name="sample-logging-output"></a>範例記錄輸出

以下是 `Debug` gRPC 用戶端層級的主控台輸出範例：

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="tracing"></a>追蹤

gRPC services 和 gRPC 用戶端會提供使用 [DiagnosticSource](/dotnet/api/system.diagnostics.diagnosticsource) 和 [活動](/dotnet/api/system.diagnostics.activity)進行 gRPC 呼叫的相關資訊。

* .NET gRPC 會使用活動來代表 gRPC 的呼叫。
* 追蹤事件會在 gRPC 呼叫活動開始和停止時寫入診斷來源。
* 追蹤不會在 gRPC 串流呼叫的存留期傳送訊息時，捕捉相關資訊。

### <a name="grpc-service-tracing"></a>gRPC 服務追蹤

gRPC 服務裝載于 ASP.NET Core，可報告傳入 HTTP 要求的相關事件。 gRPC 特定的中繼資料會新增至 ASP.NET Core 提供的現有 HTTP 要求診斷。

* 診斷來源名稱為 `Microsoft.AspNetCore` 。
* 活動名稱為 `Microsoft.AspNetCore.Hosting.HttpRequestIn` 。
  * GRPC 呼叫所叫用之 gRPC 方法的名稱會加入為具有名稱的標記 `grpc.method` 。
  * GRPC 呼叫完成時的狀態碼會新增為具有名稱的標記 `grpc.status_code` 。

### <a name="grpc-client-tracing"></a>gRPC 用戶端追蹤

.NET gRPC 用戶端會使用 `HttpClient` 來進行 gRPC 呼叫。 雖然 `HttpClient` 會寫入診斷事件，但是 .Net gRPC 用戶端會提供自訂的診斷來源、活動和事件，以便收集 gRPC 呼叫的完整資訊。

* 診斷來源名稱為 `Grpc.Net.Client` 。
* 活動名稱為 `Grpc.Net.Client.GrpcOut` 。
  * GRPC 呼叫所叫用之 gRPC 方法的名稱會加入為具有名稱的標記 `grpc.method` 。
  * GRPC 呼叫完成時的狀態碼會新增為具有名稱的標記 `grpc.status_code` 。

### <a name="collecting-tracing"></a>收集追蹤

最簡單的使用方式 `DiagnosticSource` 是在您的應用程式中設定遙測程式庫（例如 [Application Insights](/azure/azure-monitor/app/asp-net-core) 或 [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet) ）。 此程式庫會處理 gRPC 呼叫的相關資訊，以及其他應用程式遙測。

您可以在受管理的服務（例如 Application Insights）中查看追蹤，也可以選擇執行您自己的分散式追蹤系統。 OpenTelemetry 支援將追蹤資料匯出至 [Jaeger](https://www.jaegertracing.io/) 和 [Zipkin](https://zipkin.io/)。

`DiagnosticSource` 可以使用程式碼中的追蹤事件 `DiagnosticListener` 。 如需有關使用程式碼接聽診斷來源的詳細資訊，請參閱 [DiagnosticSource 使用者手冊](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener)。

> [!NOTE]
> 遙測程式庫目前不會捕獲 gRPC 特定 `Grpc.Net.Client.GrpcOut` 的遙測。 改善遙測程式庫的工作正在進行中。

## <a name="metrics"></a>計量

計量是在一段時間內的資料量值標記法，例如每秒要求數。 計量資料可讓您在高階層級觀察應用程式的狀態。 .NET gRPC 計量是使用發出的 `EventCounter` 。

### <a name="grpc-service-metrics"></a>gRPC 服務計量

系統會報告事件來源的 gRPC 伺服器度量 `Grpc.AspNetCore.Server` 。

| 名稱                      | 描述                   |
| --------------------------|-------------------------------|
| `total-calls`             | 呼叫總數                   |
| `current-calls`           | 目前的呼叫                 |
| `calls-failed`            | 呼叫總數失敗            |
| `calls-deadline-exceeded` | 總呼叫期限超過 |
| `messages-sent`           | 傳送的郵件總數           |
| `messages-received`       | 已接收的訊息總數       |
| `calls-unimplemented`     | 未實現的呼叫總數     |

ASP.NET Core 也會提供自己的 `Microsoft.AspNetCore.Hosting` 事件來源計量。

### <a name="grpc-client-metrics"></a>gRPC 用戶端計量

系統會報告事件來源的 gRPC 用戶端計量 `Grpc.Net.Client` 。

| 名稱                      | 描述                   |
| --------------------------|-------------------------------|
| `total-calls`             | 呼叫總數                   |
| `current-calls`           | 目前的呼叫                 |
| `calls-failed`            | 呼叫總數失敗            |
| `calls-deadline-exceeded` | 總呼叫期限超過 |
| `messages-sent`           | 傳送的郵件總數           |
| `messages-received`       | 已接收的訊息總數       |

### <a name="observe-metrics"></a>觀察計量

[dotnet-計數器](/dotnet/core/diagnostics/dotnet-counters) 是一種效能監視工具，適用于臨機操作健全狀況監視和第一層效能調查。 使用 `Grpc.AspNetCore.Server` 或 `Grpc.Net.Client` 作為提供者名稱監視 .net 應用程式。

```console
> dotnet-counters monitor --process-id 1902 Grpc.AspNetCore.Server

Press p to pause, r to resume, q to quit.
    Status: Running
[Grpc.AspNetCore.Server]
    Total Calls                                 300
    Current Calls                               5
    Total Calls Failed                          0
    Total Calls Deadline Exceeded               0
    Total Messages Sent                         295
    Total Messages Received                     300
    Total Calls Unimplemented                   0
```

另一種觀察 gRPC 計量的方式，就是使用 Application Insights 的 [ApplicationInsights EventCounterCollector 套件](/azure/azure-monitor/app/eventcounters)來捕捉計數器資料。 一旦安裝之後，Application Insights 會在執行時間收集常見的 .NET 計數器。 預設不會收集 gRPC 的計數器，但可以自訂 App Insights [以包含額外的計數器](/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected)。

針對要在 *Startup.cs* 中收集的應用程式見解指定 gRPC 計數器：

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // Configure App Insights to collect gRPC counters gRPC services hosted in an ASP.NET Core app
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "current-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "total-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "calls-failed"));
            }
        );
    }
```

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>