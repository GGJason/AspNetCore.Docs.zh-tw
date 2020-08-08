---
title: Debug ASP.NET CoreBlazor WebAssembly
author: guardrex
description: 瞭解如何調試 Blazor 程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/30/2020
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
uid: blazor/debug
ms.openlocfilehash: 225916411550cc8e89c604e1426316843bb0ff52
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88014538"
---
# <a name="debug-aspnet-core-no-locblazor-webassembly"></a><span data-ttu-id="5f2cd-103">Debug ASP.NET CoreBlazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5f2cd-103">Debug ASP.NET Core Blazor WebAssembly</span></span>

[<span data-ttu-id="5f2cd-104">Daniel Roth</span><span class="sxs-lookup"><span data-stu-id="5f2cd-104">Daniel Roth</span></span>](https://github.com/danroth27)

<span data-ttu-id="5f2cd-105">Blazor WebAssembly應用程式可以使用 Chromium 為基礎的瀏覽器開發工具， (Edge/Chrome) 進行調試。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-105">Blazor WebAssembly apps can be debugged using the browser dev tools in Chromium-based browsers (Edge/Chrome).</span></span> <span data-ttu-id="5f2cd-106">或者，您可以使用 Visual Studio 或 Visual Studio Code 來對應用程式進行 debug 錯。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-106">Alternatively, you can debug your app using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="5f2cd-107">可用的案例包括：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-107">Available scenarios include:</span></span>

* <span data-ttu-id="5f2cd-108">設定和移除中斷點。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-108">Set and remove breakpoints.</span></span>
* <span data-ttu-id="5f2cd-109">在 Visual Studio 和 Visual Studio Code (<kbd>F5</kbd>支援) 中，執行具有偵錯工具支援的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-109">Run the app with debugging support in Visual Studio and Visual Studio Code (<kbd>F5</kbd> support).</span></span>
* <span data-ttu-id="5f2cd-110">單一步驟 (<kbd>F10</kbd>) 程式碼。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-110">Single-step (<kbd>F10</kbd>) through the code.</span></span>
* <span data-ttu-id="5f2cd-111">在瀏覽器中使用<kbd>F8</kbd>或 Visual Studio 或 Visual Studio Code 中的<kbd>F5</kbd>繼續執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-111">Resume code execution with <kbd>F8</kbd> in a browser or <kbd>F5</kbd> in Visual Studio or Visual Studio Code.</span></span>
* <span data-ttu-id="5f2cd-112">在 [*區域變數*] 顯示中，觀察本機變數的值。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-112">In the *Locals* display, observe the values of local variables.</span></span>
* <span data-ttu-id="5f2cd-113">查看呼叫堆疊，包括從 JavaScript 轉換成 .NET，以及從 .NET 到 JavaScript 的呼叫鏈。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-113">See the call stack, including call chains that go from JavaScript into .NET and from .NET to JavaScript.</span></span>

<span data-ttu-id="5f2cd-114">目前，您*無法*：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-114">For now, you *can't*:</span></span>

* <span data-ttu-id="5f2cd-115">中斷未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-115">Break on unhandled exceptions.</span></span>
* <span data-ttu-id="5f2cd-116">在應用程式啟動期間叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-116">Hit breakpoints during app startup.</span></span>

<span data-ttu-id="5f2cd-117">我們將繼續改善即將發行的版本中的調試過程。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-117">We will continue to improve the debugging experience in upcoming releases.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5f2cd-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="5f2cd-118">Prerequisites</span></span>

<span data-ttu-id="5f2cd-119">調試需要下列其中一個瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-119">Debugging requires either of the following browsers:</span></span>

* <span data-ttu-id="5f2cd-120">Google Chrome (70 版或更新版本)  (預設值) </span><span class="sxs-lookup"><span data-stu-id="5f2cd-120">Google Chrome (version 70 or later) (default)</span></span>
* <span data-ttu-id="5f2cd-121">Microsoft Edge (80 版或更新版本) </span><span class="sxs-lookup"><span data-stu-id="5f2cd-121">Microsoft Edge (version 80 or later)</span></span>

## <a name="enable-debugging-for-visual-studio-and-visual-studio-code"></a><span data-ttu-id="5f2cd-122">啟用 Visual Studio 和 Visual Studio Code 的偵錯工具</span><span class="sxs-lookup"><span data-stu-id="5f2cd-122">Enable debugging for Visual Studio and Visual Studio Code</span></span>

<span data-ttu-id="5f2cd-123">若要啟用現有應用程式的偵測功能 Blazor WebAssembly ，請更新 `launchSettings.json` 啟始專案中的檔案，以 `inspectUri` 在每個啟動設定檔中包含下列屬性：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-123">To enable debugging for an existing Blazor WebAssembly app, update the `launchSettings.json` file in the startup project to include the following `inspectUri` property in each launch profile:</span></span>

```json
"inspectUri": "{wsProtocol}://{url.hostname}:{url.port}/_framework/debug/ws-proxy?browser={browserInspectUri}"
```

<span data-ttu-id="5f2cd-124">更新之後，檔案 `launchSettings.json` 看起來應該類似下列範例：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-124">Once updated, the `launchSettings.json` file should look similar to the following example:</span></span>

[!code-json[](debug/launchSettings.json?highlight=14,22)]

<span data-ttu-id="5f2cd-125">`inspectUri`屬性：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-125">The `inspectUri` property:</span></span>

* <span data-ttu-id="5f2cd-126">可讓 IDE 偵測應用程式是否為應用程式 Blazor WebAssembly 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-126">Enables the IDE to detect that the app is a Blazor WebAssembly app.</span></span>
* <span data-ttu-id="5f2cd-127">指示腳本的偵錯工具，透過的偵錯工具 proxy 連接到瀏覽器 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-127">Instructs the script debugging infrastructure to connect to the browser through Blazor's debugging proxy.</span></span>

<span data-ttu-id="5f2cd-128">在啟動的瀏覽器 () 上，Websocket 通訊協定的預留位置值 (`wsProtocol`) 、主機 (`url.hostname`) 、埠 (`url.port`) 和偵測器 URI， `browserInspectUri` 是由架構所提供。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-128">The placeholder values for the WebSockets protocol (`wsProtocol`), host (`url.hostname`), port (`url.port`), and inspector URI on the launched browser (`browserInspectUri`) are provided by the framework.</span></span>

## <a name="visual-studio"></a><span data-ttu-id="5f2cd-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5f2cd-129">Visual Studio</span></span>

<span data-ttu-id="5f2cd-130">若要 Blazor WebAssembly 在 Visual Studio 中進行應用程式的 debug：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-130">To debug a Blazor WebAssembly app in Visual Studio:</span></span>

1. <span data-ttu-id="5f2cd-131">建立新的 ASP.NET Core 託管 Blazor WebAssembly 應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-131">Create a new ASP.NET Core hosted Blazor WebAssembly app.</span></span>
1. <span data-ttu-id="5f2cd-132">按<kbd>F5</kbd>以在偵錯工具中執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-132">Press <kbd>F5</kbd> to run the app in the debugger.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5f2cd-133">不支援 (<kbd>Ctrl</kbd>F5) **啟動而不進行調試** + <kbd>F5</kbd> 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-133">**Start Without Debugging** (<kbd>Ctrl</kbd>+<kbd>F5</kbd>) isn't supported.</span></span>

1. <span data-ttu-id="5f2cd-134">在 `Pages/Counter.razor` 方法的中設定中斷點 `IncrementCount` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-134">Set a breakpoint in `Pages/Counter.razor` in the `IncrementCount` method.</span></span>
1. <span data-ttu-id="5f2cd-135">流覽至 [] 索引標籤 **`Counter`** ，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-135">Browse to the **`Counter`** tab and select the button to hit the breakpoint:</span></span>

   ![Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-counter.png)

1. <span data-ttu-id="5f2cd-137">查看 `currentCount` [區域變數] 視窗中的欄位值：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-137">Check out the value of the `currentCount` field in the locals window:</span></span>

   ![View 區域變數](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-locals.png)

1. <span data-ttu-id="5f2cd-139">按<kbd>F5</kbd>繼續執行。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-139">Press <kbd>F5</kbd> to continue execution.</span></span>

<span data-ttu-id="5f2cd-140">在對您的 Blazor WebAssembly 應用程式進行調試時，您也可以對伺服器程式碼進行 debug：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-140">While debugging your Blazor WebAssembly app, you can also debug your server code:</span></span>

1. <span data-ttu-id="5f2cd-141">在的頁面中設定中斷點 `Pages/FetchData.razor` <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A> 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-141">Set a breakpoint in the `Pages/FetchData.razor` page in <xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>.</span></span>
1. <span data-ttu-id="5f2cd-142">在動作方法的中設定中斷點 `WeatherForecastController` `Get` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-142">Set a breakpoint in the `WeatherForecastController` in the `Get` action method.</span></span>
1. <span data-ttu-id="5f2cd-143">流覽至索引 **`Fetch Data`** 標籤，以叫用元件中的第一個中斷點， `FetchData` 然後再對伺服器發出 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-143">Browse to the **`Fetch Data`** tab to hit the first breakpoint in the `FetchData` component just before it issues an HTTP request to the server:</span></span>

   ![Debug Fetch 資料](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-fetch-data.png)

1. <span data-ttu-id="5f2cd-145">按<kbd>F5</kbd>繼續執行，然後在的伺服器上叫用中斷點 `WeatherForecastController` ：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-145">Press <kbd>F5</kbd> to continue execution and then hit the breakpoint on the server in the `WeatherForecastController`:</span></span>

   ![Debug server](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vs-debug-server.png)

1. <span data-ttu-id="5f2cd-147">再按一次<kbd>F5</kbd> ，讓執行繼續，並查看所轉譯的氣象預測資料表。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-147">Press <kbd>F5</kbd> again to let execution continue and see the weather forecast table rendered.</span></span>

<a id="vscode"></a>

## <a name="visual-studio-code"></a><span data-ttu-id="5f2cd-148">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="5f2cd-148">Visual Studio Code</span></span>

### <a name="debug-standalone-no-locblazor-webassembly"></a><span data-ttu-id="5f2cd-149">獨立調試Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5f2cd-149">Debug standalone Blazor WebAssembly</span></span>

1. <span data-ttu-id="5f2cd-150">Blazor WebAssembly在 VS Code 中開啟獨立應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-150">Open the standalone Blazor WebAssembly app in VS Code.</span></span>

   <span data-ttu-id="5f2cd-151">您可能會收到下列通知，指出需要額外的設定才能啟用偵錯工具：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-151">You may receive the following notification that additional setup is required to enable debugging:</span></span>
   
   ![需要額外的設定](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-additional-setup.png)
   
   <span data-ttu-id="5f2cd-153">如果您收到通知：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-153">If you receive the notification:</span></span>

   * <span data-ttu-id="5f2cd-154">確認已安裝最新的[c # for Visual Studio Code 延伸](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)模組。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-154">Confirm that the latest [C# for Visual Studio Code extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) is installed.</span></span> <span data-ttu-id="5f2cd-155">若要檢查已安裝的擴充功能，請從功能表列開啟 [ **View**  >  **extension** ]，或選取 [**活動**] 提要欄位中的 [**擴充**功能] 圖示</span><span class="sxs-lookup"><span data-stu-id="5f2cd-155">To inspect the installed extensions, open **View** > **Extensions** from the menu bar or select the **Extensions** icon in the **Activity** sidebar.</span></span>
   * <span data-ttu-id="5f2cd-156">確認 JavaScript preview 的偵錯工具已啟用。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-156">Confirm that JavaScript preview debugging is enabled.</span></span> <span data-ttu-id="5f2cd-157">從功能表列中開啟**設定 ([** 檔案  >  **喜好**  >  **設定**]) 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-157">Open the settings from the menu bar (**File** > **Preferences** > **Settings**).</span></span> <span data-ttu-id="5f2cd-158">使用關鍵字進行搜尋 `debug preview` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-158">Search using the keywords `debug preview`.</span></span> <span data-ttu-id="5f2cd-159">在搜尋結果中，確認已核取 [ **Debug > JavaScript：使用預覽**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-159">In the search results, confirm that the check box for **Debug > JavaScript: Use Preview** is checked.</span></span> <span data-ttu-id="5f2cd-160">如果不存在啟用預覽偵錯工具的選項，請升級至最新版本的 VS Code 或安裝[JavaScript 偵錯工具延伸](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly)模組 (VS Code 1.46 版或更早版本的) 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-160">If the option to enable preview debugging isn't present, either upgrade to the latest version of VS Code or install the [JavaScript Debugger Extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode.js-debug-nightly) (VS Code versions 1.46 or earlier).</span></span>
   * <span data-ttu-id="5f2cd-161">重載視窗。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-161">Reload the window.</span></span>

1. <span data-ttu-id="5f2cd-162">使用<kbd>F5</kbd>鍵盤快速鍵或功能表項目開始進行調試。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-162">Start debugging using the <kbd>F5</kbd> keyboard shortcut or the menu item.</span></span>

   > [!NOTE]
   > <span data-ttu-id="5f2cd-163">不支援 (<kbd>Ctrl</kbd>F5) **執行而不進行調試** + <kbd>F5</kbd> 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-163">**Run Without Debugging** (<kbd>Ctrl</kbd>+<kbd>F5</kbd>) isn't supported.</span></span>

1. <span data-ttu-id="5f2cd-164">出現提示時，請選取 [ \*\* Blazor WebAssembly Debug\*\* ] 選項來開始進行調試。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-164">When prompted, select the **Blazor WebAssembly Debug** option to start debugging.</span></span>

   ![可用的調試選項清單](index/_static/blazor-vscode-debugtypes.png)

1. <span data-ttu-id="5f2cd-166">隨即啟動獨立應用程式，並開啟偵錯工具瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-166">The standalone app is launched, and a debugging browser is opened.</span></span>

1. <span data-ttu-id="5f2cd-167">在元件的方法中設定中斷點 `IncrementCount` `Counter` ，然後選取按鈕以叫用中斷點：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-167">Set a breakpoint in the `IncrementCount` method in the `Counter` component and then select the button to hit the breakpoint:</span></span>

   ![VS Code 中的 Debug 計數器](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-debug-counter.png)

### <a name="debug-hosted-no-locblazor-webassembly"></a><span data-ttu-id="5f2cd-169">已裝載的調試Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="5f2cd-169">Debug hosted Blazor WebAssembly</span></span>

1. <span data-ttu-id="5f2cd-170">Blazor WebAssembly在 VS Code 中開啟裝載應用程式的 [解決方案] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-170">Open the hosted Blazor WebAssembly app's solution folder in VS Code.</span></span>

1. <span data-ttu-id="5f2cd-171">如果沒有為專案設定啟動設定，則會出現下列通知。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-171">If there's no launch configuration set for the project, the following notification appears.</span></span> <span data-ttu-id="5f2cd-172">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-172">Select **Yes**.</span></span>

   ![新增必要的資產](https://devblogs.microsoft.com/aspnet/wp-content/uploads/sites/16/2020/03/vscode-required-assets.png)

1. <span data-ttu-id="5f2cd-174">在視窗頂端的 [命令選擇區] 中，選取裝載解決方案內的*伺服器*專案。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-174">In the command palette at the top of the window, select the *Server* project within the hosted solution.</span></span>

<span data-ttu-id="5f2cd-175">`launch.json`會使用啟動偵錯工具來產生檔案。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-175">A `launch.json` file is generated with the launch configuration for launching the debugger.</span></span>

### <a name="attach-to-an-existing-debugging-session"></a><span data-ttu-id="5f2cd-176">附加至現有的偵錯工具會話</span><span class="sxs-lookup"><span data-stu-id="5f2cd-176">Attach to an existing debugging session</span></span>

<span data-ttu-id="5f2cd-177">若要附加至執行 Blazor 中的應用程式，請使用下列設定來建立檔案 `launch.json` ：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-177">To attach to a running Blazor app, create a `launch.json` file with the following configuration:</span></span>

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach to Existing Blazor WebAssembly Application"
}
```

> [!NOTE]
> <span data-ttu-id="5f2cd-178">只有針對獨立應用程式才支援附加至「偵測會話」。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-178">Attaching to a debugging session is only supported for standalone apps.</span></span> <span data-ttu-id="5f2cd-179">若要使用完整堆疊的偵錯工具，您必須從 VS Code 啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-179">To use full-stack debugging, you must launch the app from VS Code.</span></span>

### <a name="launch-configuration-options"></a><span data-ttu-id="5f2cd-180">啟動設定選項</span><span class="sxs-lookup"><span data-stu-id="5f2cd-180">Launch configuration options</span></span>

<span data-ttu-id="5f2cd-181">`blazorwasm`Debug 類型 () 支援下列啟動設定選項 `.vscode/launch.json` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-181">The following launch configuration options are supported for the `blazorwasm` debug type (`.vscode/launch.json`).</span></span>

| <span data-ttu-id="5f2cd-182">選項</span><span class="sxs-lookup"><span data-stu-id="5f2cd-182">Option</span></span>    | <span data-ttu-id="5f2cd-183">描述</span><span class="sxs-lookup"><span data-stu-id="5f2cd-183">Description</span></span> |
| --------- | ----------- |
| `request` | <span data-ttu-id="5f2cd-184">使用 `launch` 來啟動並將偵錯工具連結至 Blazor WebAssembly 應用程式，或將 `attach` 偵錯工具附加至已在執行中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-184">Use `launch` to launch and attach a debugging session to a Blazor WebAssembly app or `attach` to attach a debugging session to an already-running app.</span></span> |
| `url`     | <span data-ttu-id="5f2cd-185">要在瀏覽器中開啟的 URL。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-185">The URL to open in the browser when debugging.</span></span> <span data-ttu-id="5f2cd-186">預設值為 `https://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-186">Defaults to `https://localhost:5001`.</span></span> |
| `browser` | <span data-ttu-id="5f2cd-187">要為偵錯工具啟動的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-187">The browser to launch for the debugging session.</span></span> <span data-ttu-id="5f2cd-188">設為 `edge` 或 `chrome`。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-188">Set to `edge` or `chrome`.</span></span> <span data-ttu-id="5f2cd-189">預設值為 `chrome`。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-189">Defaults to `chrome`.</span></span> |
| `trace`   | <span data-ttu-id="5f2cd-190">用來從 JS 偵錯工具產生記錄。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-190">Used to generate logs from the JS debugger.</span></span> <span data-ttu-id="5f2cd-191">將設定為 `true` 以產生記錄。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-191">Set to `true` to generate logs.</span></span> |
| `hosted`  | <span data-ttu-id="5f2cd-192">`true`如果啟動和偵測託管應用程式，則必須設定為 Blazor WebAssembly 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-192">Must be set to `true` if launching and debugging a hosted Blazor WebAssembly app.</span></span> |
| `webRoot` | <span data-ttu-id="5f2cd-193">指定 web 伺服器的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-193">Specifies the absolute path of the web server.</span></span> <span data-ttu-id="5f2cd-194">如果從子路由提供應用程式，則應設定。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-194">Should be set if an app is served from a sub-route.</span></span> |
| `timeout` | <span data-ttu-id="5f2cd-195">等候偵錯工具附加的毫秒數。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-195">The number of milliseconds to wait for the debugging session to attach.</span></span> <span data-ttu-id="5f2cd-196">預設為30000毫秒， (30 秒) 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-196">Defaults to 30,000 milliseconds (30 seconds).</span></span> |
| `program` | <span data-ttu-id="5f2cd-197">可執行檔的參考，可執行裝載應用程式的伺服器。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-197">A reference to the executable to run the server of the hosted app.</span></span> <span data-ttu-id="5f2cd-198">如果為，則必須設定 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-198">Must be set if `hosted` is `true`.</span></span> |
| `cwd`     | <span data-ttu-id="5f2cd-199">要在其下啟動應用程式的工作目錄。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-199">The working directory to launch the app under.</span></span> <span data-ttu-id="5f2cd-200">如果為，則必須設定 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-200">Must be set if `hosted` is `true`.</span></span> |
| `env`     | <span data-ttu-id="5f2cd-201">要提供給已啟動進程的環境變數。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-201">The environment variables to provide to the launched process.</span></span> <span data-ttu-id="5f2cd-202">只有在設為時才適用 `hosted` `true` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-202">Only applicable if `hosted` is set to `true`.</span></span> |

### <a name="example-launch-configurations"></a><span data-ttu-id="5f2cd-203">啟動設定範例</span><span class="sxs-lookup"><span data-stu-id="5f2cd-203">Example launch configurations</span></span>

#### <a name="launch-and-debug-a-standalone-no-locblazor-webassembly-app"></a><span data-ttu-id="5f2cd-204">啟動和調試獨立 Blazor WebAssembly 應用程式</span><span class="sxs-lookup"><span data-stu-id="5f2cd-204">Launch and debug a standalone Blazor WebAssembly app</span></span>

```json
{
  "type": "blazorwasm",
  "request": "launch",
  "name": "Launch and Debug"
}
```

#### <a name="attach-to-a-running-app-at-a-specified-url"></a><span data-ttu-id="5f2cd-205">在指定的 URL 附加至執行中的應用程式</span><span class="sxs-lookup"><span data-stu-id="5f2cd-205">Attach to a running app at a specified URL</span></span>

```json
{
  "type": "blazorwasm",
  "request": "attach",
  "name": "Attach and Debug",
  "url": "http://localhost:5000"
}
```

#### <a name="launch-and-debug-a-hosted-no-locblazor-webassembly-app-with-microsoft-edge"></a><span data-ttu-id="5f2cd-206">使用 Microsoft Edge 啟動和調試託管 Blazor WebAssembly 應用程式</span><span class="sxs-lookup"><span data-stu-id="5f2cd-206">Launch and debug a hosted Blazor WebAssembly app with Microsoft Edge</span></span>

<span data-ttu-id="5f2cd-207">瀏覽器設定預設為 Google Chrome。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-207">Browser configuration defaults to Google Chrome.</span></span> <span data-ttu-id="5f2cd-208">當您使用 Microsoft Edge 進行偵錯工具時，請將設定 `browser` 為 `edge` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-208">When using Microsoft Edge for debugging, set `browser` to `edge`.</span></span> <span data-ttu-id="5f2cd-209">若要使用 Google Chrome，請不要設定 `browser` 選項，或將選項的值設定為 `chrome` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-209">To use Google Chrome, either don't set the `browser` option or set the option's value to `chrome`.</span></span>

```json
{
  "name": "Launch and Debug Hosted Blazor WebAssembly App",
  "type": "blazorwasm",
  "request": "launch",
  "hosted": true,
  "program": "${workspaceFolder}/Server/bin/Debug/netcoreapp3.1/MyHostedApp.Server.dll",
  "cwd": "${workspaceFolder}/Server",
  "browser": "edge"
}
```

<span data-ttu-id="5f2cd-210">在上述範例中， `MyHostedApp.Server.dll` 是*伺服器*應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-210">In the preceding example, `MyHostedApp.Server.dll` is the *Server* app's assembly.</span></span> <span data-ttu-id="5f2cd-211">`.vscode`資料夾位於 `Client` 、 `Server` 和資料夾旁的方案資料夾中 `Shared` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-211">The `.vscode` folder is located in the solution's folder next to the `Client`, `Server`, and `Shared` folders.</span></span>

## <a name="debug-in-the-browser"></a><span data-ttu-id="5f2cd-212">瀏覽器中的 Debug</span><span class="sxs-lookup"><span data-stu-id="5f2cd-212">Debug in the browser</span></span>

1. <span data-ttu-id="5f2cd-213">在開發環境中執行應用程式的 Debug 組建。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-213">Run a Debug build of the app in the Development environment.</span></span>

1. <span data-ttu-id="5f2cd-214">啟動瀏覽器並流覽至應用程式的 URL (例如， `https://localhost:5001`) 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-214">Launch a browser and navigate to the app's URL (for example, `https://localhost:5001`).</span></span>

1. <span data-ttu-id="5f2cd-215">在瀏覽器中，按下<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>，嘗試開始進行遠端偵錯程式。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-215">In the browser, attempt to commence remote debugging by pressing <kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>.</span></span>

   <span data-ttu-id="5f2cd-216">瀏覽器必須在啟用遠端偵測功能的情況下執行，這不是預設值。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-216">The browser must be running with remote debugging enabled, which isn't the default.</span></span> <span data-ttu-id="5f2cd-217">如果已停用遠端偵錯程式，將會顯示 [**找不到可調試的瀏覽器]** 索引標籤錯誤頁面，並提供啟動瀏覽器並開啟偵錯工具的指示。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-217">If remote debugging is disabled, an **Unable to find debuggable browser tab** error page is rendered with instructions for launching the browser with the debugging port open.</span></span> <span data-ttu-id="5f2cd-218">遵循瀏覽器的指示，這會開啟新的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-218">Follow the instructions for your browser, which opens a new browser window.</span></span> <span data-ttu-id="5f2cd-219">關閉先前的瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-219">Close the previous browser window.</span></span>

1. <span data-ttu-id="5f2cd-220">當瀏覽器在啟用遠端偵錯程式的情況下執行時，[偵錯工具] 鍵盤快速鍵 (<kbd>Shift</kbd> + <kbd>Alt</kbd> + <kbd>D</kbd>) 會開啟新的偵錯工具索引標籤</span><span class="sxs-lookup"><span data-stu-id="5f2cd-220">Once the browser is running with remote debugging enabled, the debugging keyboard shortcut (<kbd>Shift</kbd>+<kbd>Alt</kbd>+<kbd>D</kbd>) opens a new debugger tab.</span></span>

1. <span data-ttu-id="5f2cd-221">經過一段時間之後，[**來源**] 索引標籤會顯示該節點內的應用程式 .net 元件清單 `file://` 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-221">After a moment, the **Sources** tab shows a list of the app's .NET assemblies within the `file://` node.</span></span>

1. <span data-ttu-id="5f2cd-222">在元件程式碼中 (檔案 `.razor`) 和 c # 程式碼檔 (`.cs`) 中，您設定的中斷點會在執行程式碼時叫用。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-222">In component code (`.razor` files) and C# code files (`.cs`), breakpoints that you set are hit when code executes.</span></span> <span data-ttu-id="5f2cd-223">叫用中斷點之後，單一步驟 (<kbd>F10</kbd>) 透過程式碼或繼續 (<kbd>F8</kbd>) 程式碼正常執行。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-223">After a breakpoint is hit, single-step (<kbd>F10</kbd>) through the code or resume (<kbd>F8</kbd>) code execution normally.</span></span>

<span data-ttu-id="5f2cd-224">Blazor提供的偵錯工具 proxy 會執行[Chrome DevTools 通訊協定](https://chromedevtools.github.io/devtools-protocol/)，並使用來擴充通訊協定。NET 特定資訊。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-224">Blazor provides a debugging proxy that implements the [Chrome DevTools Protocol](https://chromedevtools.github.io/devtools-protocol/) and augments the protocol with .NET-specific information.</span></span> <span data-ttu-id="5f2cd-225">當您按下 [調試鍵盤快速鍵] 時，會將 Blazor Chrome DevTools 指向 proxy。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-225">When debugging keyboard shortcut is pressed, Blazor points the Chrome DevTools at the proxy.</span></span> <span data-ttu-id="5f2cd-226">Proxy 會連線到您想要進行 debug 的瀏覽器視窗 (因此需要啟用遠端偵錯) 。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-226">The proxy connects to the browser window you're seeking to debug (hence the need to enable remote debugging).</span></span>

## <a name="browser-source-maps"></a><span data-ttu-id="5f2cd-227">瀏覽器來源對應</span><span class="sxs-lookup"><span data-stu-id="5f2cd-227">Browser source maps</span></span>

<span data-ttu-id="5f2cd-228">瀏覽器來源對應可讓瀏覽器將已編譯的檔案對應回原始來源檔案，而且通常會用於用戶端的偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-228">Browser source maps allow the browser to map compiled files back to their original source files and are commonly used for client-side debugging.</span></span> <span data-ttu-id="5f2cd-229">不過， Blazor 目前不會將 c # 直接對應至 JavaScript/WASM。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-229">However, Blazor doesn't currently map C# directly to JavaScript/WASM.</span></span> <span data-ttu-id="5f2cd-230">相反地， Blazor 會在瀏覽器中進行 IL 轉譯，因此來源對應不相關。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-230">Instead, Blazor does IL interpretation within the browser, so source maps aren't relevant.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="5f2cd-231">疑難排解</span><span class="sxs-lookup"><span data-stu-id="5f2cd-231">Troubleshoot</span></span>

<span data-ttu-id="5f2cd-232">如果您遇到錯誤，下列秘訣可能會有説明：</span><span class="sxs-lookup"><span data-stu-id="5f2cd-232">If you're running into errors, the following tips may help:</span></span>

* <span data-ttu-id="5f2cd-233">在 [**偵錯工具**] 索引標籤中，開啟瀏覽器中的開發人員工具。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-233">In the **Debugger** tab, open the developer tools in your browser.</span></span> <span data-ttu-id="5f2cd-234">在主控台中，執行 `localStorage.clear()` 以移除任何中斷點。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-234">In the console, execute `localStorage.clear()` to remove any breakpoints.</span></span>
* <span data-ttu-id="5f2cd-235">確認您已安裝並信任 ASP.NET Core 的 HTTPS 開發憑證。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-235">Confirm that you've installed and trusted the ASP.NET Core HTTPS development certificate.</span></span> <span data-ttu-id="5f2cd-236">如需詳細資訊，請參閱<xref:security/enforcing-ssl#troubleshoot-certificate-problems>。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-236">For more information, see <xref:security/enforcing-ssl#troubleshoot-certificate-problems>.</span></span>
* <span data-ttu-id="5f2cd-237">Visual Studio 需要 [**工具**] [選項] [一般] 中的 [**啟用 ASP.NET (Chrome、Edge 和 IE) 選項的 JavaScript 偵錯工具**]  >  **Options**  >  **Debugging**  >  \*\* \*\*。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-237">Visual Studio requires the **Enable JavaScript debugging for ASP.NET (Chrome, Edge and IE)** option in **Tools** > **Options** > **Debugging** > **General**.</span></span> <span data-ttu-id="5f2cd-238">這是 Visual Studio 的預設設定。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-238">This is the default setting for Visual Studio.</span></span> <span data-ttu-id="5f2cd-239">如果偵錯工具無法運作，請確認已選取此選項。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-239">If debugging isn't working, confirm that the option is selected.</span></span>

### <a name="breakpoints-in-oninitializedasync-not-hit"></a><span data-ttu-id="5f2cd-240">`OnInitialized{Async}`未命中中斷點</span><span class="sxs-lookup"><span data-stu-id="5f2cd-240">Breakpoints in `OnInitialized{Async}` not hit</span></span>

<span data-ttu-id="5f2cd-241">Blazor架構的偵錯工具 proxy 需要短暫的時間來啟動，所以可能不會叫用[ `OnInitialized{Async}` 生命週期方法](xref:blazor/components/lifecycle#component-initialization-methods)中的中斷點。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-241">The Blazor framework's debugging proxy takes a short time to launch, so breakpoints in the [`OnInitialized{Async}` lifecycle method](xref:blazor/components/lifecycle#component-initialization-methods) might not be hit.</span></span> <span data-ttu-id="5f2cd-242">建議您在方法主體的開頭新增延遲，以便在叫用中斷點之前，讓 debug proxy 有一些時間啟動。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-242">We recommend adding a delay at the start of the method body to give the debug proxy some time to launch before the breakpoint is hit.</span></span> <span data-ttu-id="5f2cd-243">您可以根據[ `if` 編譯器](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)指示詞來包含延遲，以確保應用程式的發行組建不會有延遲。</span><span class="sxs-lookup"><span data-stu-id="5f2cd-243">You can include the delay based on an [`if` compiler directive](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if) to ensure that the delay isn't present for a release build of the app.</span></span>

<span data-ttu-id="5f2cd-244"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A>:</span><span class="sxs-lookup"><span data-stu-id="5f2cd-244"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitialized%2A>:</span></span>

```csharp
protected override void OnInitialized()
{
#if DEBUG
    Thread.Sleep(10000)
#endif

    ...
}
```

<span data-ttu-id="5f2cd-245"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>:</span><span class="sxs-lookup"><span data-stu-id="5f2cd-245"><xref:Microsoft.AspNetCore.Components.ComponentBase.OnInitializedAsync%2A>:</span></span>

```csharp
protected override async Task OnInitializedAsync()
{
#if DEBUG
    await Task.Delay(10000)
#endif

    ...
}
```
