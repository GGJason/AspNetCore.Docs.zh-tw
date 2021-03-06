---
title: ASP.NET Core 中的記錄和診斷 SignalR
author: anurse
description: 瞭解如何從您的 ASP.NET Core 應用程式收集診斷資訊 SignalR 。
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: devx-track-csharp, signalr, devx-track-js
ms.date: 06/12/2020
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
uid: signalr/diagnostics
ms.openlocfilehash: 49029bbd53b98425a05bdb82517238e3aa2e3b1f
ms.sourcegitcommit: 6b87f2e064cea02e65dacd206394b44f5c604282
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97506626"
---
# <a name="logging-and-diagnostics-in-aspnet-core-no-locsignalr"></a>ASP.NET Core 中的記錄和診斷 SignalR

[Andrew Stanton-護士](https://twitter.com/anurse)

本文提供從 ASP.NET Core SignalR 應用程式收集診斷資訊，以協助疑難排解問題的指引。

## <a name="server-side-logging"></a>伺服器端記錄

> [!WARNING]
> 伺服器端記錄檔可能包含來自您應用程式的機密資訊。 **切勿** 將未經處理的記錄從生產環境應用程式張貼到 GitHub 之類的公用論壇。

由於 SignalR 是 ASP.NET Core 的一部分，因此會使用 ASP.NET Core 記錄系統。 在預設設定中， SignalR 會記錄極少的資訊，但這可以設定。 如需有關設定 ASP.NET Core 記錄的詳細資訊，請參閱 [ASP.NET Core 記錄](xref:fundamentals/logging/index#configuration) 檔。

SignalR 使用兩個記錄器類別：

* `Microsoft.AspNetCore.SignalR`：適用于與中樞通訊協定相關的記錄、啟動中樞、叫用方法，以及其他中樞相關的活動。
* `Microsoft.AspNetCore.Http.Connections`：適用于傳輸的相關記錄，例如 Websocket、長時間輪詢、Server-Sent 事件，以及低層級的 SignalR 基礎結構。

若要從啟用詳細記錄 SignalR ，請將 `Debug` *appsettings.json* 下列專案新增至檔案中的 `LogLevel` 子區段，以將上述兩個前置詞設定為檔案中的層級 `Logging` ：

[!code-json[](diagnostics/logging-config.json?highlight=7-8)]

您也可以在方法的程式碼中進行這項設定 `CreateWebHostBuilder` ：

[!code-csharp[](diagnostics/logging-config-code.cs?highlight=5-6)]

如果您未使用以 JSON 為基礎的設定，請在設定系統中設定下列設定值：

* `Logging:LogLevel:Microsoft.AspNetCore.SignalR` = `Debug`
* `Logging:LogLevel:Microsoft.AspNetCore.Http.Connections` = `Debug`

請檢查設定系統的檔，以判斷如何指定嵌套設定值。 例如，使用環境變數時， `_` 會使用兩個字元，而不是 `:` (例如 `Logging__LogLevel__Microsoft.AspNetCore.SignalR`) 。

`Debug`針對您的應用程式收集更詳細的診斷時，建議使用此層級。 `Trace`層級會產生非常低層級的診斷，而且很少需要診斷您應用程式中的問題。

## <a name="access-server-side-logs"></a>存取伺服器端記錄

您存取伺服器端記錄的方式取決於您執行的環境。

### <a name="as-a-console-app-outside-iis"></a>作為 IIS 外部的主控台應用程式

如果您是在主控台應用程式中執行，則預設應啟用 [主控台記錄器](xref:fundamentals/logging/index#console) 。 SignalR 記錄檔將會出現在主控台中。

### <a name="within-iis-express-from-visual-studio"></a>從 Visual Studio 的 IIS Express 內

Visual Studio 會在 [ **輸出** ] 視窗中顯示記錄輸出。 選取 [ **ASP.NET Core Web 服務器** ] 下拉式清單選項。

### <a name="azure-app-service"></a>Azure App Service

在 Azure App Service 入口網站的 [**診斷記錄**] 區段中，啟用 **(Filesystem) 選項的應用程式記錄**，並將 **層級** 設定為 `Verbose` 。 記錄應該可從 **記錄串流** 服務和 App Service 檔案系統的記錄中取得。 如需詳細資訊，請參閱 [Azure 記錄資料流程](xref:fundamentals/logging/index#azure-log-streaming)。

### <a name="other-environments"></a>其他環境

如果應用程式部署到另一個環境 (例如 Docker、Kubernetes 或 Windows 服務) ，請參閱， <xref:fundamentals/logging/index> 以取得如何設定適用于環境的記錄提供者的詳細資訊。

## <a name="javascript-client-logging"></a>JavaScript 用戶端記錄

> [!WARNING]
> 用戶端記錄可能包含來自您應用程式的機密資訊。 **切勿** 將未經處理的記錄從生產環境應用程式張貼到 GitHub 之類的公用論壇。

使用 JavaScript 用戶端時，您可以使用上的方法設定記錄選項 `configureLogging` `HubConnectionBuilder` ：

[!code-javascript[](diagnostics/logging-config-js.js?highlight=3)]

若要完全停用記錄，請 `signalR.LogLevel.None` 在方法中指定 `configureLogging` 。

下表顯示適用于 JavaScript 用戶端的記錄層級。 將記錄層級設定為下列其中一個值，就可以在該層級以及在資料表中其上方的所有層級進行記錄。

| 層級 | 描述 |
| ----- | ----------- |
| `None` | 不會記錄任何訊息。 |
| `Critical` | 指出整個應用程式中失敗的訊息。 |
| `Error` | 指出目前操作失敗的訊息。 |
| `Warning` | 指出非嚴重問題的訊息。 |
| `Information` | 參考訊息。 |
| `Debug` | 適用于偵錯工具的診斷訊息。 |
| `Trace` | 專為診斷特定問題而設計的非常詳細診斷訊息。 |

設定詳細資訊之後，記錄會寫入至瀏覽器主控台 (或 NodeJS 應用程式) 中的標準輸出。

如果您想要將記錄傳送至自訂記錄系統，您可以提供執行介面的 JavaScript 物件 `ILogger` 。 唯一需要執行的方法是 `log` ，它會採用事件的層級以及與事件相關聯的訊息。 例如︰

[!code-typescript[](diagnostics/custom-logger.ts?highlight=3-7,13)]

## <a name="net-client-logging"></a>.NET 用戶端記錄

> [!WARNING]
> 用戶端記錄可能包含來自您應用程式的機密資訊。 **切勿** 將未經處理的記錄從生產環境應用程式張貼到 GitHub 之類的公用論壇。

若要從 .NET 用戶端取得記錄，您可以 `ConfigureLogging` 在上使用方法 `HubConnectionBuilder` 。 其運作方式與與上的 `ConfigureLogging` 方法相同 `WebHostBuilder` `HostBuilder` 。 您可以設定 ASP.NET Core 中所使用的相同記錄提供者。 不過，您必須手動安裝和啟用個別記錄提供者的 NuGet 套件。

若要將 .NET 用戶端記錄新增至 Blazor WebAssembly 應用程式，請參閱 <xref:blazor/fundamentals/logging#signalr-net-client-logging> 。

### <a name="console-logging"></a>主控台記錄

若要啟用主控台記錄，請新增 [ [記錄] 主控台](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Console) 套件。 然後，使用 `AddConsole` 方法來設定主控台記錄器：

[!code-csharp[](diagnostics/net-client-console-log.cs?highlight=6)]

### <a name="debug-output-window-logging"></a>調試輸出視窗記錄

您也可以在 Visual Studio 中設定記錄以移至 [ **輸出** ] 視窗。 安裝 [ [記錄](https://www.nuget.org/packages/Microsoft.Extensions.Logging.Debug) 檔]，然後使用 `AddDebug` 方法：

[!code-csharp[](diagnostics/net-client-debug-log.cs?highlight=6)]

### <a name="other-logging-providers"></a>其他記錄提供者

SignalR 支援其他記錄提供者，例如 Serilog、Seq、NLog 或與整合的任何其他記錄系統 `Microsoft.Extensions.Logging` 。 如果您的記錄系統提供 `ILoggerProvider` ，您可以使用下列程式來註冊 `AddProvider` ：

[!code-csharp[](diagnostics/net-client-custom-log.cs?highlight=6)]

### <a name="control-verbosity"></a>控制詳細資訊

如果您是從應用程式中的其他位置進行記錄，將預設層級變更為 `Debug` 可能太詳細。 您可以使用篩選來設定記錄的記錄層級 SignalR 。 這可以在程式碼中完成，與伺服器上的方式大致相同：

[!code-csharp[Controlling verbosity in .NET client](diagnostics/logging-config-client-code.cs?highlight=9-10)]

## <a name="network-traces"></a>網路追蹤

> [!WARNING]
> 網路追蹤包含您的應用程式所傳送之每個訊息的完整內容。 **切勿** 將未經處理的網路追蹤從生產環境應用程式張貼到 GitHub 之類的公用論壇。

如果您遇到問題，網路追蹤有時可能會提供許多有用的資訊。 如果您要在問題追蹤程式上提出問題，這項功能特別有用。

## <a name="collect-a-network-trace-with-fiddler-preferred-option"></a>使用 Fiddler (慣用選項收集網路追蹤) 

此方法適用于所有應用程式。

Fiddler 是一個非常強大的工具，可用於收集 HTTP 追蹤。 從 [telerik.com/fiddler](https://www.telerik.com/fiddler)安裝它、加以啟動，然後執行您的應用程式並重現問題。 Fiddler 適用于 Windows，而且有適用于 macOS 和 Linux 的 Beta 版。

如果您使用 HTTPS 進行連線，則有一些額外的步驟可確保 Fiddler 可將 HTTPS 流量解密。 如需詳細資訊，請參閱 [Fiddler 檔](https://docs.telerik.com/fiddler/Configure-Fiddler/Tasks/DecryptHTTPS)。

收集追蹤之後，您可以從功能表列 **選擇 [** 檔案  >  **儲存**  >  **所有會話**] 來匯出追蹤。

![從 Fiddler 匯出所有會話](diagnostics/fiddler-export.png)

## <a name="collect-a-network-trace-with-tcpdump-macos-and-linux-only"></a>使用 tcpdump 只收集網路追蹤)  (的 macOS 和 Linux

此方法適用于所有應用程式。

您可以從命令列介面執行下列命令，以使用 tcpdump 收集原始 TCP 追蹤。 `root` `sudo` 如果您收到許可權錯誤，則可能需要或在命令前面加上前置詞：

```console
tcpdump -i [interface] -w trace.pcap
```

取代 `[interface]` 為您想要捕捉的網路介面。 通常，這類似于 `/dev/eth0` 標準乙太網路介面的 (，) 或 `/dev/lo0` (localhost 流量) 。 如需詳細資訊，請參閱 `tcpdump` 主機系統上的 man 頁面。

## <a name="collect-a-network-trace-in-the-browser"></a>在瀏覽器中收集網路追蹤

此方法僅適用于以瀏覽器為基礎的應用程式。

大部分的瀏覽器開發人員工具都有一個 [網路] 索引標籤，可讓您在瀏覽器與伺服器之間抓取網路活動。 不過，這些追蹤不會包含 WebSocket 和 Server-Sent 事件訊息。 如果您使用這些傳輸，則使用 Fiddler 或 TcpDump 等工具 (如下所述) 是較佳的方法。

### <a name="microsoft-edge-and-internet-explorer"></a>Microsoft Edge 和 Internet Explorer

 (Edge 和 Internet Explorer 的指示都相同) 

1. 按 F12 以開啟 Dev Tools
2. 按一下 [網路] 索引標籤
3. 視需要重新整理頁面 (，) 並重現問題
4. 按一下工具列中的 [儲存] 圖示，將追蹤匯出為「HAR」檔案：

![[Microsoft Edge Dev 工具網路] 索引標籤上的 [儲存] 圖示](diagnostics/ie-edge-har-export.png)

### <a name="google-chrome"></a>Google Chrome

1. 按 F12 以開啟 Dev Tools
2. 按一下 [網路] 索引標籤
3. 視需要重新整理頁面 (，) 並重現問題
4. 以滑鼠右鍵按一下要求清單中的任意位置，然後選擇 [以內容另存為 HAR]：

![Google Chrome 開發工具網路索引標籤中的 [以內容另存為 HAR] 選項](diagnostics/chrome-har-export.png)

### <a name="mozilla-firefox"></a>Mozilla Firefox

1. 按 F12 以開啟 Dev Tools
2. 按一下 [網路] 索引標籤
3. 視需要重新整理頁面 (，) 並重現問題
4. 以滑鼠右鍵按一下要求清單中的任意位置，然後選擇 [全部儲存為 HAR]

![Mozilla Firefox Dev Tools 網路索引標籤中的 [全部儲存為 HAR] 選項](diagnostics/firefox-har-export.png)

## <a name="attach-diagnostics-files-to-github-issues"></a>將診斷檔案附加至 GitHub 問題

您可以藉由重新命名來將診斷檔案附加至 GitHub 問題，使其具有延伸模組，然後將 `.txt` 其拖放到問題上。

> [!NOTE]
> 請勿將記錄檔或網路追蹤的內容貼入 GitHub 問題。 這些記錄和追蹤可能很大，而且 GitHub 通常會將它們截斷。

![將記錄檔拖曳至 GitHub 問題](diagnostics/attaching-diagnostics-files.png)

## <a name="metrics"></a>計量

計量是在一段時間內的資料量值標記法。 例如，每秒的要求數。 計量資料可讓您觀察高階應用程式的狀態。 .NET gRPC 計量是使用發出的 <xref:System.Diagnostics.Tracing.EventCounter> 。

### <a name="no-locsignalr-server-metrics"></a>SignalR 伺服器計量

SignalR 系統會報告事件來源的伺服器計量 <xref:Microsoft.AspNetCore.Http.Connections> 。

| 名稱                    | 說明                 |
|-------------------------|-----------------------------|
| `connections-started`   | 已啟動的連接總數   |
| `connections-stopped`   | 已停止的連接總數   |
| `connections-timed-out` | 總連接逾時 |
| `current-connections`   | 目前的連線數         |
| `connections-duration`  | 平均連接持續時間 |

### <a name="observe-metrics"></a>觀察計量

[dotnet-計數器](/dotnet/core/diagnostics/dotnet-counters) 是一種效能監視工具，適用于臨機操作健全狀況監視和第一層效能調查。 使用 `Microsoft.AspNetCore.Http.Connections` 作為提供者名稱監視 .net 應用程式。 例如：

```console
> dotnet-counters monitor --process-id 37016 Microsoft.AspNetCore.Http.Connections

Press p to pause, r to resume, q to quit.
    Status: Running
[Microsoft.AspNetCore.Http.Connections]
    Average Connection Duration (ms)       16,040.56
    Current Connections                         1
    Total Connections Started                   8
    Total Connections Stopped                   7
    Total Connections Timed Out                 0
```

## <a name="additional-resources"></a>其他資源

* <xref:signalr/configuration>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
