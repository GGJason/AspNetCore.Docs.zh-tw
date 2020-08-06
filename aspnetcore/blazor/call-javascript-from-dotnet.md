---
title: 在 ASP.NET Core 中從 .NET 方法呼叫 JavaScript 函式Blazor
author: guardrex
description: 瞭解如何從應用程式中的 .NET 方法叫用 JavaScript 函數 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/07/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/call-javascript-from-dotnet
ms.openlocfilehash: f3afa9592422da911799a137f9943631ed12ab59
ms.sourcegitcommit: 84150702757cf7a7b839485382420e8db8e92b9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87819122"
---
# <a name="call-javascript-functions-from-net-methods-in-aspnet-core-no-locblazor"></a><span data-ttu-id="ba079-103">在 ASP.NET Core 中從 .NET 方法呼叫 JavaScript 函式Blazor</span><span class="sxs-lookup"><span data-stu-id="ba079-103">Call JavaScript functions from .NET methods in ASP.NET Core Blazor</span></span>

<span data-ttu-id="ba079-104">By [Javier Calvarro Nelson](https://github.com/javiercn)、 [Daniel Roth](https://github.com/danroth27)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ba079-104">By [Javier Calvarro Nelson](https://github.com/javiercn), [Daniel Roth](https://github.com/danroth27), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ba079-105">Blazor應用程式可以從 javascript 函數的 .net 方法和 .net 方法叫用 javascript 函式。</span><span class="sxs-lookup"><span data-stu-id="ba079-105">A Blazor app can invoke JavaScript functions from .NET methods and .NET methods from JavaScript functions.</span></span> <span data-ttu-id="ba079-106">這些案例稱為*JavaScript 互通性* (*JS interop*) 。</span><span class="sxs-lookup"><span data-stu-id="ba079-106">These scenarios are called *JavaScript interoperability* (*JS interop*).</span></span>

<span data-ttu-id="ba079-107">本文涵蓋從 .NET 叫用 JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="ba079-107">This article covers invoking JavaScript functions from .NET.</span></span> <span data-ttu-id="ba079-108">如需如何從 JavaScript 呼叫 .NET 方法的詳細資訊，請參閱 <xref:blazor/call-dotnet-from-javascript> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-108">For information on how to call .NET methods from JavaScript, see <xref:blazor/call-dotnet-from-javascript>.</span></span>

<span data-ttu-id="ba079-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="ba079-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/blazor/common/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ba079-110">若要從 .NET 呼叫 JavaScript，請使用 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="ba079-110">To call into JavaScript from .NET, use the <xref:Microsoft.JSInterop.IJSRuntime> abstraction.</span></span> <span data-ttu-id="ba079-111">若要發出 JS interop 呼叫，請 <xref:Microsoft.JSInterop.IJSRuntime> 在您的元件中插入抽象概念。</span><span class="sxs-lookup"><span data-stu-id="ba079-111">To issue JS interop calls, inject the <xref:Microsoft.JSInterop.IJSRuntime> abstraction in your component.</span></span> <span data-ttu-id="ba079-112"><xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A>取得您想要叫用之 JavaScript 函式的識別碼，以及任何數目的 JSON 可序列化引數。</span><span class="sxs-lookup"><span data-stu-id="ba079-112"><xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A> takes an identifier for the JavaScript function that you wish to invoke along with any number of JSON-serializable arguments.</span></span> <span data-ttu-id="ba079-113">函數識別碼相對於全域範圍 (`window`) 。</span><span class="sxs-lookup"><span data-stu-id="ba079-113">The function identifier is relative to the global scope (`window`).</span></span> <span data-ttu-id="ba079-114">如果您想要呼叫 `window.someScope.someFunction` ，識別碼會是 `someScope.someFunction` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-114">If you wish to call `window.someScope.someFunction`, the identifier is `someScope.someFunction`.</span></span> <span data-ttu-id="ba079-115">在呼叫函式之前，不需要先註冊函式。</span><span class="sxs-lookup"><span data-stu-id="ba079-115">There's no need to register the function before it's called.</span></span> <span data-ttu-id="ba079-116">傳回型別 `T` 也必須是 JSON 可序列化。</span><span class="sxs-lookup"><span data-stu-id="ba079-116">The return type `T` must also be JSON serializable.</span></span> <span data-ttu-id="ba079-117">`T`應符合最佳對應至所傳回 JSON 類型的 .NET 類型。</span><span class="sxs-lookup"><span data-stu-id="ba079-117">`T` should match the .NET type that best maps to the JSON type returned.</span></span>

<span data-ttu-id="ba079-118">對於 Blazor Server 已啟用已匯入功能的應用程式，在初始的預處理期間，不可能呼叫 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="ba079-118">For Blazor Server apps with prerendering enabled, calling into JavaScript isn't possible during the initial prerendering.</span></span> <span data-ttu-id="ba079-119">在建立與瀏覽器的連線之後，必須延後 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="ba079-119">JavaScript interop calls must be deferred until after the connection with the browser is established.</span></span> <span data-ttu-id="ba079-120">如需詳細資訊，請參閱偵測[ Blazor Server 應用程式何時進行預呈現](#detect-when-a-blazor-server-app-is-prerendering)一節。</span><span class="sxs-lookup"><span data-stu-id="ba079-120">For more information, see the [Detect when a Blazor Server app is prerendering](#detect-when-a-blazor-server-app-is-prerendering) section.</span></span>

<span data-ttu-id="ba079-121">下列範例是 [`TextDecoder`](https://developer.mozilla.org/docs/Web/API/TextDecoder) 以 JavaScript 為基礎的解碼器為基礎。</span><span class="sxs-lookup"><span data-stu-id="ba079-121">The following example is based on [`TextDecoder`](https://developer.mozilla.org/docs/Web/API/TextDecoder), a JavaScript-based decoder.</span></span> <span data-ttu-id="ba079-122">此範例示範如何從 c # 方法叫用 JavaScript 函式，將開發人員程式碼的需求卸載至現有的 JavaScript API。</span><span class="sxs-lookup"><span data-stu-id="ba079-122">The example demonstrates how to invoke a JavaScript function from a C# method that offloads a requirement from developer code to an existing JavaScript API.</span></span> <span data-ttu-id="ba079-123">JavaScript 函式會接受 c # 方法的位元組陣列、解碼陣列，然後將文字傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="ba079-123">The JavaScript function accepts a byte array from a C# method, decodes the array, and returns the text to the component for display.</span></span>

<span data-ttu-id="ba079-124">在 `<head>` `wwwroot/index.html` (Blazor WebAssembly) 或 () 的元素內 `Pages/_Host.cshtml` ，提供 JavaScript 函式來 Blazor Server 使用 `TextDecoder` 來解碼傳遞的陣列，並傳回解碼的值：</span><span class="sxs-lookup"><span data-stu-id="ba079-124">Inside the `<head>` element of `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` (Blazor Server), provide a JavaScript function that uses `TextDecoder` to decode a passed array and return the decoded value:</span></span>

[!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-convertarray.html)]

<span data-ttu-id="ba079-125">JavaScript 程式碼（如上述範例所示的程式碼）也可以從 JavaScript 檔案載入， (`.js`) ，並具有腳本檔案的參考：</span><span class="sxs-lookup"><span data-stu-id="ba079-125">JavaScript code, such as the code shown in the preceding example, can also be loaded from a JavaScript file (`.js`) with a reference to the script file:</span></span>

```html
<script src="exampleJsInterop.js"></script>
```

<span data-ttu-id="ba079-126">下列元件：</span><span class="sxs-lookup"><span data-stu-id="ba079-126">The following component:</span></span>

* <span data-ttu-id="ba079-127">`convertArray` `JSRuntime` 當選取元件按鈕 () 時，會使用來叫用 JavaScript 函數 **`Convert Array`** 。</span><span class="sxs-lookup"><span data-stu-id="ba079-127">Invokes the `convertArray` JavaScript function using `JSRuntime` when a component button (**`Convert Array`**) is selected.</span></span>
* <span data-ttu-id="ba079-128">呼叫 JavaScript 函式之後，傳遞的陣列會轉換成字串。</span><span class="sxs-lookup"><span data-stu-id="ba079-128">After the JavaScript function is called, the passed array is converted into a string.</span></span> <span data-ttu-id="ba079-129">此字串會傳回給元件以供顯示。</span><span class="sxs-lookup"><span data-stu-id="ba079-129">The string is returned to the component for display.</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/call-js-example.razor?highlight=2,34-35)]

## <a name="ijsruntime"></a><span data-ttu-id="ba079-130">IJSRuntime</span><span class="sxs-lookup"><span data-stu-id="ba079-130">IJSRuntime</span></span>

<span data-ttu-id="ba079-131">若要使用 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念，請採用下列任何一種方法：</span><span class="sxs-lookup"><span data-stu-id="ba079-131">To use the <xref:Microsoft.JSInterop.IJSRuntime> abstraction, adopt any of the following approaches:</span></span>

* <span data-ttu-id="ba079-132">將 <xref:Microsoft.JSInterop.IJSRuntime> 抽象 Razor () 中插入元件 `.razor` ：</span><span class="sxs-lookup"><span data-stu-id="ba079-132">Inject the <xref:Microsoft.JSInterop.IJSRuntime> abstraction into the Razor component (`.razor`):</span></span>

  [!code-razor[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction.razor?highlight=1)]

  <span data-ttu-id="ba079-133">在 `<head>` `wwwroot/index.html` (Blazor WebAssembly) 或 () 的元素內 `Pages/_Host.cshtml` Blazor Server ，提供 `handleTickerChanged` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="ba079-133">Inside the `<head>` element of `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="ba079-134">呼叫函式時 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> ，不會傳回值：</span><span class="sxs-lookup"><span data-stu-id="ba079-134">The function is called with <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> and doesn't return a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged1.html)]

* <span data-ttu-id="ba079-135">將 <xref:Microsoft.JSInterop.IJSRuntime> 抽象概念插入類別 (`.cs`) ：</span><span class="sxs-lookup"><span data-stu-id="ba079-135">Inject the <xref:Microsoft.JSInterop.IJSRuntime> abstraction into a class (`.cs`):</span></span>

  [!code-csharp[](call-javascript-from-dotnet/samples_snapshot/inject-abstraction-class.cs?highlight=5)]

  <span data-ttu-id="ba079-136">在 `<head>` `wwwroot/index.html` (Blazor WebAssembly) 或 () 的元素內 `Pages/_Host.cshtml` Blazor Server ，提供 `handleTickerChanged` JavaScript 函數。</span><span class="sxs-lookup"><span data-stu-id="ba079-136">Inside the `<head>` element of `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` (Blazor Server), provide a `handleTickerChanged` JavaScript function.</span></span> <span data-ttu-id="ba079-137">使用呼叫函式 `JSRuntime.InvokeAsync` ，並傳回值：</span><span class="sxs-lookup"><span data-stu-id="ba079-137">The function is called with `JSRuntime.InvokeAsync` and returns a value:</span></span>

  [!code-html[](call-javascript-from-dotnet/samples_snapshot/index-script-handleTickerChanged2.html)]

* <span data-ttu-id="ba079-138">針對使用[BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic)的動態內容產生，請使用 `[Inject]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="ba079-138">For dynamic content generation with [BuildRenderTree](xref:blazor/advanced-scenarios#manual-rendertreebuilder-logic), use the `[Inject]` attribute:</span></span>

  ```razor
  [Inject]
  IJSRuntime JSRuntime { get; set; }
  ```

<span data-ttu-id="ba079-139">在本主題隨附的用戶端範例應用程式中，有兩個 JavaScript 函式可供與 DOM 互動的應用程式使用，以接收使用者輸入並顯示歡迎訊息：</span><span class="sxs-lookup"><span data-stu-id="ba079-139">In the client-side sample app that accompanies this topic, two JavaScript functions are available to the app that interact with the DOM to receive user input and display a welcome message:</span></span>

* <span data-ttu-id="ba079-140">`showPrompt`：產生提示，接受使用者輸入 (使用者的名稱) 並將名稱傳回給呼叫者。</span><span class="sxs-lookup"><span data-stu-id="ba079-140">`showPrompt`: Produces a prompt to accept user input (the user's name) and returns the name to the caller.</span></span>
* <span data-ttu-id="ba079-141">`displayWelcome`：將歡迎訊息從呼叫者指派給具有之的 DOM 物件 `id` `welcome` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-141">`displayWelcome`: Assigns a welcome message from the caller to a DOM object with an `id` of `welcome`.</span></span>

<span data-ttu-id="ba079-142">`wwwroot/exampleJsInterop.js`:</span><span class="sxs-lookup"><span data-stu-id="ba079-142">`wwwroot/exampleJsInterop.js`:</span></span>

[!code-javascript[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/exampleJsInterop.js?highlight=2-7)]

<span data-ttu-id="ba079-143">將 `<script>` 參考 JavaScript 檔案的標記放在檔案 `wwwroot/index.html` (Blazor WebAssembly) 或檔案 `Pages/_Host.cshtml` () 中 Blazor Server 。</span><span class="sxs-lookup"><span data-stu-id="ba079-143">Place the `<script>` tag that references the JavaScript file in the `wwwroot/index.html` file (Blazor WebAssembly) or `Pages/_Host.cshtml` file (Blazor Server).</span></span>

<span data-ttu-id="ba079-144">`wwwroot/index.html` (Blazor WebAssembly):</span><span class="sxs-lookup"><span data-stu-id="ba079-144">`wwwroot/index.html` (Blazor WebAssembly):</span></span>

[!code-html[](./common/samples/3.x/BlazorWebAssemblySample/wwwroot/index.html?highlight=22)]

<span data-ttu-id="ba079-145">`Pages/_Host.cshtml` (Blazor Server):</span><span class="sxs-lookup"><span data-stu-id="ba079-145">`Pages/_Host.cshtml` (Blazor Server):</span></span>

[!code-cshtml[](./common/samples/3.x/BlazorServerSample/Pages/_Host.cshtml?highlight=35)]

<span data-ttu-id="ba079-146">請勿將 `<script>` 標記放在元件檔中，因為 `<script>` 無法動態更新標記。</span><span class="sxs-lookup"><span data-stu-id="ba079-146">Don't place a `<script>` tag in a component file because the `<script>` tag can't be updated dynamically.</span></span>

<span data-ttu-id="ba079-147">.NET 方法會藉由呼叫，與檔案中的 JavaScript 函式進行 interop `exampleJsInterop.js` <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-147">.NET methods interop with the JavaScript functions in the `exampleJsInterop.js` file by calling <xref:Microsoft.JSInterop.IJSRuntime.InvokeAsync%2A?displayProperty=nameWithType>.</span></span>

<span data-ttu-id="ba079-148"><xref:Microsoft.JSInterop.IJSRuntime>抽象概念是非同步，以允許 Blazor Server 案例。</span><span class="sxs-lookup"><span data-stu-id="ba079-148">The <xref:Microsoft.JSInterop.IJSRuntime> abstraction is asynchronous to allow for Blazor Server scenarios.</span></span> <span data-ttu-id="ba079-149">如果應用程式是 Blazor WebAssembly 應用程式，而且您想要以同步方式叫用 JavaScript 函式，請改為向下轉換 <xref:Microsoft.JSInterop.IJSInProcessRuntime> 並呼叫 <xref:Microsoft.JSInterop.IJSInProcessRuntime.Invoke%2A> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-149">If the app is a Blazor WebAssembly app and you want to invoke a JavaScript function synchronously, downcast to <xref:Microsoft.JSInterop.IJSInProcessRuntime> and call <xref:Microsoft.JSInterop.IJSInProcessRuntime.Invoke%2A> instead.</span></span> <span data-ttu-id="ba079-150">我們建議大部分的 JS interop 程式庫都使用非同步 Api，以確保所有案例中都有可用的程式庫。</span><span class="sxs-lookup"><span data-stu-id="ba079-150">We recommend that most JS interop libraries use the async APIs to ensure that the libraries are available in all scenarios.</span></span>

<span data-ttu-id="ba079-151">範例應用程式包含一個可示範 JS interop 的元件。</span><span class="sxs-lookup"><span data-stu-id="ba079-151">The sample app includes a component to demonstrate JS interop.</span></span> <span data-ttu-id="ba079-152">元件：</span><span class="sxs-lookup"><span data-stu-id="ba079-152">The component:</span></span>

* <span data-ttu-id="ba079-153">透過 JavaScript 提示接收使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="ba079-153">Receives user input via a JavaScript prompt.</span></span>
* <span data-ttu-id="ba079-154">將文字傳回到元件以進行處理。</span><span class="sxs-lookup"><span data-stu-id="ba079-154">Returns the text to the component for processing.</span></span>
* <span data-ttu-id="ba079-155">呼叫與 DOM 互動的第二個 JavaScript 函式，以顯示歡迎訊息。</span><span class="sxs-lookup"><span data-stu-id="ba079-155">Calls a second JavaScript function that interacts with the DOM to display a welcome message.</span></span>

<span data-ttu-id="ba079-156">`Pages/JsInterop.razor`:</span><span class="sxs-lookup"><span data-stu-id="ba079-156">`Pages/JsInterop.razor`:</span></span>

```razor
@page "/JSInterop"
@using {APP ASSEMBLY}.JsInteropClasses
@inject IJSRuntime JSRuntime

<h1>JavaScript Interop</h1>

<h2>Invoke JavaScript functions from .NET methods</h2>

<button type="button" class="btn btn-primary" @onclick="TriggerJsPrompt">
    Trigger JavaScript Prompt
</button>

<h3 id="welcome" style="color:green;font-style:italic"></h3>

@code {
    public async Task TriggerJsPrompt()
    {
        var name = await JSRuntime.InvokeAsync<string>(
                "exampleJsFunctions.showPrompt",
                "What's your name?");

        await JSRuntime.InvokeVoidAsync(
                "exampleJsFunctions.displayWelcome",
                $"Hello {name}! Welcome to Blazor!");
    }
}
```

<span data-ttu-id="ba079-157">預留位置 `{APP ASSEMBLY}` 為應用程式的應用程式元件名稱 (例如， `BlazorSample`) 。</span><span class="sxs-lookup"><span data-stu-id="ba079-157">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

1. <span data-ttu-id="ba079-158">當您 `TriggerJsPrompt` 選取元件的按鈕來執行時 **`Trigger JavaScript Prompt`** ， `showPrompt` 會呼叫檔案中提供的 JavaScript 函數 `wwwroot/exampleJsInterop.js` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-158">When `TriggerJsPrompt` is executed by selecting the component's **`Trigger JavaScript Prompt`** button, the JavaScript `showPrompt` function provided in the `wwwroot/exampleJsInterop.js` file is called.</span></span>
1. <span data-ttu-id="ba079-159">函式 `showPrompt` 會接受使用者輸入 (使用者的名稱) ，這是 HTML 編碼並傳回給元件。</span><span class="sxs-lookup"><span data-stu-id="ba079-159">The `showPrompt` function accepts user input (the user's name), which is HTML-encoded and returned to the component.</span></span> <span data-ttu-id="ba079-160">元件會將使用者的名稱儲存在本機變數中 `name` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-160">The component stores the user's name in a local variable, `name`.</span></span>
1. <span data-ttu-id="ba079-161">儲存在中的字串 `name` 會併入歡迎使用的訊息中，其會傳遞至 JavaScript 函式，以將 `displayWelcome` 歡迎訊息轉譯為標題標記。</span><span class="sxs-lookup"><span data-stu-id="ba079-161">The string stored in `name` is incorporated into a welcome message, which is passed to a JavaScript function, `displayWelcome`, which renders the welcome message into a heading tag.</span></span>

## <a name="call-a-void-javascript-function"></a><span data-ttu-id="ba079-162">呼叫 void JavaScript 函數</span><span class="sxs-lookup"><span data-stu-id="ba079-162">Call a void JavaScript function</span></span>

<span data-ttu-id="ba079-163">傳回[void (0) /void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void)或[undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined)的 JavaScript 函式會使用呼叫 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-163">JavaScript functions that return [void(0)/void 0](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Operators/void) or [undefined](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/undefined) are called with <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType>.</span></span>

## <a name="detect-when-a-no-locblazor-server-app-is-prerendering"></a><span data-ttu-id="ba079-164">偵測 Blazor Server 應用程式何時已進行預呈現</span><span class="sxs-lookup"><span data-stu-id="ba079-164">Detect when a Blazor Server app is prerendering</span></span>
 
[!INCLUDE[](~/includes/blazor-prerendering.md)]

## <a name="capture-references-to-elements"></a><span data-ttu-id="ba079-165">捕捉元素的參考</span><span class="sxs-lookup"><span data-stu-id="ba079-165">Capture references to elements</span></span>

<span data-ttu-id="ba079-166">某些 JS interop 案例需要 HTML 元素的參考。</span><span class="sxs-lookup"><span data-stu-id="ba079-166">Some JS interop scenarios require references to HTML elements.</span></span> <span data-ttu-id="ba079-167">例如，UI 程式庫可能需要專案參考以進行初始化，或者您可能需要在專案上呼叫類似命令的 Api，例如 `focus` 或 `play` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-167">For example, a UI library may require an element reference for initialization, or you might need to call command-like APIs on an element, such as `focus` or `play`.</span></span>

<span data-ttu-id="ba079-168">使用下列方法來抓取元件中的 HTML 元素參考：</span><span class="sxs-lookup"><span data-stu-id="ba079-168">Capture references to HTML elements in a component using the following approach:</span></span>

* <span data-ttu-id="ba079-169">將 `@ref` 屬性加入至 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="ba079-169">Add an `@ref` attribute to the HTML element.</span></span>
* <span data-ttu-id="ba079-170">定義類型的欄位， <xref:Microsoft.AspNetCore.Components.ElementReference> 其名稱符合屬性的值 `@ref` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-170">Define a field of type <xref:Microsoft.AspNetCore.Components.ElementReference> whose name matches the value of the `@ref` attribute.</span></span>

<span data-ttu-id="ba079-171">下列範例顯示如何捕捉專案的參考 `username` `<input>` ：</span><span class="sxs-lookup"><span data-stu-id="ba079-171">The following example shows capturing a reference to the `username` `<input>` element:</span></span>

```razor
<input @ref="username" ... />

@code {
    ElementReference username;
}
```

> [!WARNING]
> <span data-ttu-id="ba079-172">只使用專案參考來改變不會與互動之空白元素的內容 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="ba079-172">Only use an element reference to mutate the contents of an empty element that doesn't interact with Blazor.</span></span> <span data-ttu-id="ba079-173">當協力廠商 API 提供內容給元素時，此案例很有用。</span><span class="sxs-lookup"><span data-stu-id="ba079-173">This scenario is useful when a third-party API supplies content to the element.</span></span> <span data-ttu-id="ba079-174">因為 Blazor 不會與專案互動，所以專案和 DOM 的標記法之間不可能發生衝突 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="ba079-174">Because Blazor doesn't interact with the element, there's no possibility of a conflict between Blazor's representation of the element and the DOM.</span></span>
>
> <span data-ttu-id="ba079-175">在下列範例中，將未排序清單的內容 () 變得很*危險*， `ul` 因為會 Blazor 與 DOM 互動以填入此元素的清單專案 (`<li>`) ：</span><span class="sxs-lookup"><span data-stu-id="ba079-175">In the following example, it's *dangerous* to mutate the contents of the unordered list (`ul`) because Blazor interacts with the DOM to populate this element's list items (`<li>`):</span></span>
>
> ```razor
> <ul ref="MyList">
>     @foreach (var item in Todos)
>     {
>         <li>@item.Text</li>
>     }
> </ul>
> ```
>
> <span data-ttu-id="ba079-176">如果 JS interop 變動此元素的內容， `MyList` 並 Blazor 嘗試將差異套用至專案，則差異不會符合 DOM。</span><span class="sxs-lookup"><span data-stu-id="ba079-176">If JS interop mutates the contents of element `MyList` and Blazor attempts to apply diffs to the element, the diffs won't match the DOM.</span></span>

<span data-ttu-id="ba079-177">就 .NET 程式碼而言， <xref:Microsoft.AspNetCore.Components.ElementReference> 是一個不透明的控制碼。</span><span class="sxs-lookup"><span data-stu-id="ba079-177">As far as .NET code is concerned, an <xref:Microsoft.AspNetCore.Components.ElementReference> is an opaque handle.</span></span> <span data-ttu-id="ba079-178">您*唯一*可以做的事 <xref:Microsoft.AspNetCore.Components.ElementReference> ，就是透過 JS interop 將它傳遞至 JavaScript 程式碼。</span><span class="sxs-lookup"><span data-stu-id="ba079-178">The *only* thing you can do with <xref:Microsoft.AspNetCore.Components.ElementReference> is pass it through to JavaScript code via JS interop.</span></span> <span data-ttu-id="ba079-179">當您這麼做時，JavaScript 端程式碼會接收一個 `HTMLElement` 實例，它可以搭配一般的 DOM api 來使用。</span><span class="sxs-lookup"><span data-stu-id="ba079-179">When you do so, the JavaScript-side code receives an `HTMLElement` instance, which it can use with normal DOM APIs.</span></span>

<span data-ttu-id="ba079-180">例如，下列程式碼會定義 .NET 擴充方法，讓您能夠將焦點設定在元素上：</span><span class="sxs-lookup"><span data-stu-id="ba079-180">For example, the following code defines a .NET extension method that enables setting the focus on an element:</span></span>

<span data-ttu-id="ba079-181">`exampleJsInterop.js`:</span><span class="sxs-lookup"><span data-stu-id="ba079-181">`exampleJsInterop.js`:</span></span>

```javascript
window.exampleJsFunctions = {
  focusElement : function (element) {
    element.focus();
  }
}
```

<span data-ttu-id="ba079-182">若要呼叫不會傳回值的 JavaScript 函數，請使用 <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-182">To call a JavaScript function that doesn't return a value, use <xref:Microsoft.JSInterop.JSRuntimeExtensions.InvokeVoidAsync%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="ba079-183">下列程式碼會藉由使用已捕捉的來呼叫前面的 JavaScript 函式，將焦點放在使用者名稱輸入上 <xref:Microsoft.AspNetCore.Components.ElementReference> ：</span><span class="sxs-lookup"><span data-stu-id="ba079-183">The following code sets the focus on the username input by calling the preceding JavaScript function with the captured <xref:Microsoft.AspNetCore.Components.ElementReference>:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component1.razor?highlight=1,3,11-12)]

<span data-ttu-id="ba079-184">若要使用擴充方法，請建立會接收實例的靜態擴充方法 <xref:Microsoft.JSInterop.IJSRuntime> ：</span><span class="sxs-lookup"><span data-stu-id="ba079-184">To use an extension method, create a static extension method that receives the <xref:Microsoft.JSInterop.IJSRuntime> instance:</span></span>

```csharp
public static async Task Focus(this ElementReference elementRef, IJSRuntime jsRuntime)
{
    await jsRuntime.InvokeVoidAsync(
        "exampleJsFunctions.focusElement", elementRef);
}
```

<span data-ttu-id="ba079-185">`Focus`方法是直接在物件上呼叫。</span><span class="sxs-lookup"><span data-stu-id="ba079-185">The `Focus` method is called directly on the object.</span></span> <span data-ttu-id="ba079-186">下列範例假設 `Focus` 方法可從 `JsInteropClasses` 命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="ba079-186">The following example assumes that the `Focus` method is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component2.razor?highlight=1-4,12)]

> [!IMPORTANT]
> <span data-ttu-id="ba079-187">`username`只有在呈現元件之後，才會填入變數。</span><span class="sxs-lookup"><span data-stu-id="ba079-187">The `username` variable is only populated after the component is rendered.</span></span> <span data-ttu-id="ba079-188">如果將擴展 <xref:Microsoft.AspNetCore.Components.ElementReference> 傳遞至 javascript 程式碼，javascript 程式碼就會收到的值 `null` 。</span><span class="sxs-lookup"><span data-stu-id="ba079-188">If an unpopulated <xref:Microsoft.AspNetCore.Components.ElementReference> is passed to JavaScript code, the JavaScript code receives a value of `null`.</span></span> <span data-ttu-id="ba079-189">若要在元件完成轉譯之後操作專案參考 (以設定元素的初始焦點) 使用[ `OnAfterRenderAsync` 或 `OnAfterRender` 元件生命週期方法](xref:blazor/components/lifecycle#after-component-render)。</span><span class="sxs-lookup"><span data-stu-id="ba079-189">To manipulate element references after the component has finished rendering (to set the initial focus on an element) use the [`OnAfterRenderAsync` or `OnAfterRender` component lifecycle methods](xref:blazor/components/lifecycle#after-component-render).</span></span>

<span data-ttu-id="ba079-190">當使用泛型型別並傳回值時，請使用 <xref:System.Threading.Tasks.ValueTask%601> ：</span><span class="sxs-lookup"><span data-stu-id="ba079-190">When working with generic types and returning a value, use <xref:System.Threading.Tasks.ValueTask%601>:</span></span>

```csharp
public static ValueTask<T> GenericMethod<T>(this ElementReference elementRef, 
    IJSRuntime jsRuntime)
{
    return jsRuntime.InvokeAsync<T>(
        "exampleJsFunctions.doSomethingGeneric", elementRef);
}
```

<span data-ttu-id="ba079-191">`GenericMethod`會在具有類型的物件上直接呼叫。</span><span class="sxs-lookup"><span data-stu-id="ba079-191">`GenericMethod` is called directly on the object with a type.</span></span> <span data-ttu-id="ba079-192">下列範例假設 `GenericMethod` 可以從 `JsInteropClasses` 命名空間取得：</span><span class="sxs-lookup"><span data-stu-id="ba079-192">The following example assumes that the `GenericMethod` is available from the `JsInteropClasses` namespace:</span></span>

[!code-razor[](call-javascript-from-dotnet/samples_snapshot/component3.razor?highlight=17)]

## <a name="reference-elements-across-components"></a><span data-ttu-id="ba079-193">跨元件的參考元素</span><span class="sxs-lookup"><span data-stu-id="ba079-193">Reference elements across components</span></span>

<span data-ttu-id="ba079-194"><xref:Microsoft.AspNetCore.Components.ElementReference>只有在元件的 (方法中才會保證有效， <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 而元素參考則是 `struct`) ，因此無法在元件之間傳遞專案參考。</span><span class="sxs-lookup"><span data-stu-id="ba079-194">An <xref:Microsoft.AspNetCore.Components.ElementReference> is only guaranteed valid in a component's <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> method (and an element reference is a `struct`), so an element reference can't be passed between components.</span></span>

<span data-ttu-id="ba079-195">若要讓父系元件能夠將專案參考提供給其他元件，父系元件可以：</span><span class="sxs-lookup"><span data-stu-id="ba079-195">For a parent component to make an element reference available to other components, the parent component can:</span></span>

* <span data-ttu-id="ba079-196">允許子元件註冊回呼。</span><span class="sxs-lookup"><span data-stu-id="ba079-196">Allow child components to register callbacks.</span></span>
* <span data-ttu-id="ba079-197">使用傳遞的元素參考，在事件期間叫用已註冊的回呼 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-197">Invoke the registered callbacks during the <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> event with the passed element reference.</span></span> <span data-ttu-id="ba079-198">這個方法會間接讓子元件與父系的元素參考互動。</span><span class="sxs-lookup"><span data-stu-id="ba079-198">Indirectly, this approach allows child components to interact with the parent's element reference.</span></span>

<span data-ttu-id="ba079-199">下列 Blazor WebAssembly 範例說明方法。</span><span class="sxs-lookup"><span data-stu-id="ba079-199">The following Blazor WebAssembly example illustrates the approach.</span></span>

<span data-ttu-id="ba079-200">在 `<head>` 的中 `wwwroot/index.html` ：</span><span class="sxs-lookup"><span data-stu-id="ba079-200">In the `<head>` of `wwwroot/index.html`:</span></span>

```html
<style>
    .red { color: red }
</style>
```

<span data-ttu-id="ba079-201">在 `<body>` 的中 `wwwroot/index.html` ：</span><span class="sxs-lookup"><span data-stu-id="ba079-201">In the `<body>` of `wwwroot/index.html`:</span></span>

```html
<script>
    function setElementClass(element, className) {
        /** @type {HTMLElement} **/
        var myElement = element;
        myElement.classList.add(className);
    }
</script>
```

<span data-ttu-id="ba079-202">`Pages/Index.razor` (父元件) ：</span><span class="sxs-lookup"><span data-stu-id="ba079-202">`Pages/Index.razor` (parent component):</span></span>

```razor
@page "/"

<h1 @ref="title">Hello, world!</h1>

Welcome to your new app.

<SurveyPrompt Parent="this" Title="How is Blazor working for you?" />
```

<span data-ttu-id="ba079-203">`Pages/Index.razor.cs`:</span><span class="sxs-lookup"><span data-stu-id="ba079-203">`Pages/Index.razor.cs`:</span></span>

```csharp
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Components;

namespace {APP ASSEMBLY}.Pages
{
    public partial class Index : 
        ComponentBase, IObservable<ElementReference>, IDisposable
    {
        private bool disposing;
        private IList<IObserver<ElementReference>> subscriptions = 
            new List<IObserver<ElementReference>>();
        private ElementReference title;

        protected override void OnAfterRender(bool firstRender)
        {
            base.OnAfterRender(firstRender);

            foreach (var subscription in subscriptions)
            {
                try
                {
                    subscription.OnNext(title);
                }
                catch (Exception)
                {
                    throw;
                }
            }
        }

        public void Dispose()
        {
            disposing = true;

            foreach (var subscription in subscriptions)
            {
                try
                {
                    subscription.OnCompleted();
                }
                catch (Exception)
                {
                }
            }

            subscriptions.Clear();
        }

        public IDisposable Subscribe(IObserver<ElementReference> observer)
        {
            if (disposing)
            {
                throw new InvalidOperationException("Parent being disposed");
            }

            subscriptions.Add(observer);

            return new Subscription(observer, this);
        }

        private class Subscription : IDisposable
        {
            public Subscription(IObserver<ElementReference> observer, Index self)
            {
                Observer = observer;
                Self = self;
            }

            public IObserver<ElementReference> Observer { get; }
            public Index Self { get; }

            public void Dispose()
            {
                Self.subscriptions.Remove(Observer);
            }
        }
    }
}
```

<span data-ttu-id="ba079-204">預留位置 `{APP ASSEMBLY}` 為應用程式的應用程式元件名稱 (例如， `BlazorSample`) 。</span><span class="sxs-lookup"><span data-stu-id="ba079-204">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

<span data-ttu-id="ba079-205">`Shared/SurveyPrompt.razor` (子元件) ：</span><span class="sxs-lookup"><span data-stu-id="ba079-205">`Shared/SurveyPrompt.razor` (child component):</span></span>

```razor
@inject IJSRuntime JS

<div class="alert alert-secondary mt-4" role="alert">
    <span class="oi oi-pencil mr-2" aria-hidden="true"></span>
    <strong>@Title</strong>

    <span class="text-nowrap">
        Please take our
        <a target="_blank" class="font-weight-bold" 
            href="https://go.microsoft.com/fwlink/?linkid=2109206">brief survey</a>
    </span>
    and tell us what you think.
</div>

@code {
    [Parameter]
    public string Title { get; set; }
}
```

<span data-ttu-id="ba079-206">`Shared/SurveyPrompt.razor.cs`:</span><span class="sxs-lookup"><span data-stu-id="ba079-206">`Shared/SurveyPrompt.razor.cs`:</span></span>

```csharp
using System;
using Microsoft.AspNetCore.Components;

namespace {APP ASSEMBLY}.Shared
{
    public partial class SurveyPrompt : 
        ComponentBase, IObserver<ElementReference>, IDisposable
    {
        private IDisposable subscription = null;

        [Parameter]
        public IObservable<ElementReference> Parent { get; set; }

        protected override void OnParametersSet()
        {
            base.OnParametersSet();

            if (subscription != null)
            {
                subscription.Dispose();
            }

            subscription = Parent.Subscribe(this);
        }

        public void OnCompleted()
        {
            subscription = null;
        }

        public void OnError(Exception error)
        {
            subscription = null;
        }

        public void OnNext(ElementReference value)
        {
            JS.InvokeAsync<object>(
                "setElementClass", new object[] { value, "red" });
        }

        public void Dispose()
        {
            subscription?.Dispose();
        }
    }
}
```

<span data-ttu-id="ba079-207">預留位置 `{APP ASSEMBLY}` 為應用程式的應用程式元件名稱 (例如， `BlazorSample`) 。</span><span class="sxs-lookup"><span data-stu-id="ba079-207">The placeholder `{APP ASSEMBLY}` is the app's app assembly name (for example, `BlazorSample`).</span></span>

## <a name="harden-js-interop-calls"></a><span data-ttu-id="ba079-208">強化 JS interop 呼叫</span><span class="sxs-lookup"><span data-stu-id="ba079-208">Harden JS interop calls</span></span>

<span data-ttu-id="ba079-209">JS interop 可能會因為網路錯誤而失敗，而且應該視為不可靠。</span><span class="sxs-lookup"><span data-stu-id="ba079-209">JS interop may fail due to networking errors and should be treated as unreliable.</span></span> <span data-ttu-id="ba079-210">根據預設， Blazor Server 應用程式會在一分鐘後呼叫伺服器上的 JS interop。</span><span class="sxs-lookup"><span data-stu-id="ba079-210">By default, a Blazor Server app times out JS interop calls on the server after one minute.</span></span> <span data-ttu-id="ba079-211">如果應用程式可以容忍更積極的超時時間，請使用下列其中一種方法來設定超時時間：</span><span class="sxs-lookup"><span data-stu-id="ba079-211">If an app can tolerate a more aggressive timeout, set the timeout using one of the following approaches:</span></span>

* <span data-ttu-id="ba079-212">全域在中 `Startup.ConfigureServices` ，指定 [超時]：</span><span class="sxs-lookup"><span data-stu-id="ba079-212">Globally in `Startup.ConfigureServices`, specify the timeout:</span></span>

  ```csharp
  services.AddServerSideBlazor(
      options => options.JSInteropDefaultCallTimeout = TimeSpan.FromSeconds({SECONDS}));
  ```

* <span data-ttu-id="ba079-213">在元件程式碼中的每個調用，單一呼叫可以指定 timeout：</span><span class="sxs-lookup"><span data-stu-id="ba079-213">Per-invocation in component code, a single call can specify the timeout:</span></span>

  ```csharp
  var result = await JSRuntime.InvokeAsync<string>("MyJSOperation", 
      TimeSpan.FromSeconds({SECONDS}), new[] { "Arg1" });
  ```

<span data-ttu-id="ba079-214">如需資源耗盡的詳細資訊，請參閱 <xref:blazor/security/server/threat-mitigation> 。</span><span class="sxs-lookup"><span data-stu-id="ba079-214">For more information on resource exhaustion, see <xref:blazor/security/server/threat-mitigation>.</span></span>

[!INCLUDE[](~/includes/blazor-share-interop-code.md)]

## <a name="avoid-circular-object-references"></a><span data-ttu-id="ba079-215">避免迴圈物件參考</span><span class="sxs-lookup"><span data-stu-id="ba079-215">Avoid circular object references</span></span>

<span data-ttu-id="ba079-216">在用戶端上，包含迴圈參考的物件無法序列化為下列任一項：</span><span class="sxs-lookup"><span data-stu-id="ba079-216">Objects that contain circular references can't be serialized on the client for either:</span></span>

* <span data-ttu-id="ba079-217">.NET 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="ba079-217">.NET method calls.</span></span>
* <span data-ttu-id="ba079-218">當傳回類型具有迴圈參考時，來自 c # 的 JavaScript 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="ba079-218">JavaScript method calls from C# when the return type has circular references.</span></span>

<span data-ttu-id="ba079-219">如需詳細資訊，請參閱下列問題：</span><span class="sxs-lookup"><span data-stu-id="ba079-219">For more information, see the following issues:</span></span>

* [<span data-ttu-id="ba079-220">不支援迴圈參考，請採用兩個 (dotnet/aspnetcore #20525) </span><span class="sxs-lookup"><span data-stu-id="ba079-220">Circular references are not supported, take two (dotnet/aspnetcore #20525)</span></span>](https://github.com/dotnet/aspnetcore/issues/20525)
* [<span data-ttu-id="ba079-221">提案：在序列化 (dotnet/runtime #30820 時，加入處理迴圈參考的機制) </span><span class="sxs-lookup"><span data-stu-id="ba079-221">Proposal: Add mechanism to handle circular references when serializing (dotnet/runtime #30820)</span></span>](https://github.com/dotnet/runtime/issues/30820)

## <a name="additional-resources"></a><span data-ttu-id="ba079-222">其他資源</span><span class="sxs-lookup"><span data-stu-id="ba079-222">Additional resources</span></span>

* <xref:blazor/call-dotnet-from-javascript>
* [<span data-ttu-id="ba079-223">InteropComponent razor 範例 (dotnet/AspNetCore GitHub 存放庫，3.1 發行分支) </span><span class="sxs-lookup"><span data-stu-id="ba079-223">InteropComponent.razor example (dotnet/AspNetCore GitHub repository, 3.1 release branch)</span></span>](https://github.com/dotnet/AspNetCore/blob/release/3.1/src/Components/test/testassets/BasicTestApp/InteropComponent.razor)
* [<span data-ttu-id="ba079-224">在應用程式中執行大型資料傳輸 Blazor Server</span><span class="sxs-lookup"><span data-stu-id="ba079-224">Perform large data transfers in Blazor Server apps</span></span>](xref:blazor/advanced-scenarios#perform-large-data-transfers-in-blazor-server-apps)
