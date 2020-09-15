---
title: ASP.NET Core 簡介 Blazor
author: guardrex
description: 探索 ASP.NET Core Blazor ，這是在 ASP.NET Core 應用程式中使用 .net 建立互動式用戶端 WEB UI 的方式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc, seoapril2019
ms.date: 06/19/2020
no-loc:
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
uid: blazor/index
ms.openlocfilehash: ace2285e3265e7bb2ec50c8afce2eb9c296fd524
ms.sourcegitcommit: 600666440398788db5db25dc0496b9ca8fe50915
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2020
ms.locfileid: "90080338"
---
# <a name="introduction-to-aspnet-core-no-locblazor"></a><span data-ttu-id="ebe30-103">ASP.NET Core 簡介 Blazor</span><span class="sxs-lookup"><span data-stu-id="ebe30-103">Introduction to ASP.NET Core Blazor</span></span>

<span data-ttu-id="ebe30-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ebe30-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ebe30-105">*歡迎使用 Blazor ！*</span><span class="sxs-lookup"><span data-stu-id="ebe30-105">*Welcome to Blazor!*</span></span>

<span data-ttu-id="ebe30-106">Blazor 是使用 .NET 建立互動式用戶端 web UI 的架構：</span><span class="sxs-lookup"><span data-stu-id="ebe30-106">Blazor is a framework for building interactive client-side web UI with .NET:</span></span>

* <span data-ttu-id="ebe30-107">使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。</span><span class="sxs-lookup"><span data-stu-id="ebe30-107">Create rich interactive UIs using C# instead of JavaScript.</span></span>
* <span data-ttu-id="ebe30-108">共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="ebe30-108">Share server-side and client-side app logic written in .NET.</span></span>
* <span data-ttu-id="ebe30-109">將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ebe30-109">Render the UI as HTML and CSS for wide browser support, including mobile browsers.</span></span>
* <span data-ttu-id="ebe30-110">與新式裝載平臺（例如 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index)）整合。</span><span class="sxs-lookup"><span data-stu-id="ebe30-110">Integrate with modern hosting platforms, such as [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index).</span></span>

<span data-ttu-id="ebe30-111">使用 .NET 進行用戶端網頁程式開發可提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="ebe30-111">Using .NET for client-side web development offers the following advantages:</span></span>

* <span data-ttu-id="ebe30-112">以 C# 撰寫而不是 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="ebe30-112">Write code in C# instead of JavaScript.</span></span>
* <span data-ttu-id="ebe30-113">利用 .NET 程式庫的現有 .NET 生態系統。</span><span class="sxs-lookup"><span data-stu-id="ebe30-113">Leverage the existing .NET ecosystem of .NET libraries.</span></span>
* <span data-ttu-id="ebe30-114">跨伺服器和用戶端共用應用程式邏輯。</span><span class="sxs-lookup"><span data-stu-id="ebe30-114">Share app logic across server and client.</span></span>
* <span data-ttu-id="ebe30-115">從 .NET 的效能、可靠性和安全性中獲益。</span><span class="sxs-lookup"><span data-stu-id="ebe30-115">Benefit from .NET's performance, reliability, and security.</span></span>
* <span data-ttu-id="ebe30-116">使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。</span><span class="sxs-lookup"><span data-stu-id="ebe30-116">Stay productive with Visual Studio on Windows, Linux, and macOS.</span></span>
* <span data-ttu-id="ebe30-117">以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。</span><span class="sxs-lookup"><span data-stu-id="ebe30-117">Build on a common set of languages, frameworks, and tools that are stable, feature-rich, and easy to use.</span></span>

## <a name="components"></a><span data-ttu-id="ebe30-118">單元</span><span class="sxs-lookup"><span data-stu-id="ebe30-118">Components</span></span>

<span data-ttu-id="ebe30-119">Blazor 應用程式是以 *元件*為基礎。</span><span class="sxs-lookup"><span data-stu-id="ebe30-119">Blazor apps are based on *components*.</span></span> <span data-ttu-id="ebe30-120">中的元件 Blazor 是 UI 的元素，例如頁面、對話方塊或資料輸入表單。</span><span class="sxs-lookup"><span data-stu-id="ebe30-120">A component in Blazor is an element of UI, such as a page, dialog, or data entry form.</span></span>

<span data-ttu-id="ebe30-121">元件是內建在 .NET 組件中且具有下列功能的 .NET 類別：</span><span class="sxs-lookup"><span data-stu-id="ebe30-121">Components are .NET classes built into .NET assemblies that:</span></span>

* <span data-ttu-id="ebe30-122">定義彈性 UI 轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="ebe30-122">Define flexible UI rendering logic.</span></span>
* <span data-ttu-id="ebe30-123">處理使用者動作。</span><span class="sxs-lookup"><span data-stu-id="ebe30-123">Handle user events.</span></span>
* <span data-ttu-id="ebe30-124">可以為巢狀結構，且可重複使用。</span><span class="sxs-lookup"><span data-stu-id="ebe30-124">Can be nested and reused.</span></span>
* <span data-ttu-id="ebe30-125">可以共用和散發為[ Razor 類別庫](xref:razor-pages/ui-class)或[NuGet 套件](/nuget/what-is-nuget)。</span><span class="sxs-lookup"><span data-stu-id="ebe30-125">Can be shared and distributed as [Razor class libraries](xref:razor-pages/ui-class) or [NuGet packages](/nuget/what-is-nuget).</span></span>

<span data-ttu-id="ebe30-126">元件類別通常是以 [Razor](xref:mvc/views/razor) 具有副檔名的標記頁面形式來撰寫 `.razor` 。</span><span class="sxs-lookup"><span data-stu-id="ebe30-126">The component class is usually written in the form of a [Razor](xref:mvc/views/razor) markup page with a `.razor` file extension.</span></span> <span data-ttu-id="ebe30-127">中的元件 Blazor 統稱為\* Razor 元件\*。</span><span class="sxs-lookup"><span data-stu-id="ebe30-127">Components in Blazor are formally referred to as *Razor components*.</span></span> <span data-ttu-id="ebe30-128">Razor 是一種語法，可將 HTML 標籤與設計為開發人員生產力的 c # 程式碼結合。</span><span class="sxs-lookup"><span data-stu-id="ebe30-128">Razor is a syntax for combining HTML markup with C# code designed for developer productivity.</span></span> <span data-ttu-id="ebe30-129">Razor 可讓您在具有 [IntelliSense](/visualstudio/ide/using-intellisense) 支援的相同檔案中，于 HTML 標籤和 c # 之間切換。</span><span class="sxs-lookup"><span data-stu-id="ebe30-129">Razor allows you to switch between HTML markup and C# in the same file with [IntelliSense](/visualstudio/ide/using-intellisense) support.</span></span> <span data-ttu-id="ebe30-130">Razor Pages 和 MVC 也會使用 Razor 。</span><span class="sxs-lookup"><span data-stu-id="ebe30-130">Razor Pages and MVC also use Razor.</span></span> <span data-ttu-id="ebe30-131">不同 Razor 于建立在要求/回應模型周圍的頁面和 MVC，元件專門用於用戶端 UI 邏輯和組合。</span><span class="sxs-lookup"><span data-stu-id="ebe30-131">Unlike Razor Pages and MVC, which are built around a request/response model, components are used specifically for client-side UI logic and composition.</span></span>

<span data-ttu-id="ebe30-132">下列 Razor 標記示範 `Dialog.razor` 可在另一個元件中嵌套的元件 () ：</span><span class="sxs-lookup"><span data-stu-id="ebe30-132">The following Razor markup demonstrates a component (`Dialog.razor`), which can be nested within another component:</span></span>

```razor
<div>
    <h1>@Title</h1>

    @ChildContent

    <button @onclick="OnYes">Yes!</button>
</div>

@code {
    [Parameter]
    public string Title { get; set; }

    [Parameter]
    public RenderFragment ChildContent { get; set; }

    private void OnYes()
    {
        Console.WriteLine("Write to the console in C#! 'Yes' button was selected.");
    }
}
```

<span data-ttu-id="ebe30-133">對話方塊的內文內容 (`ChildContent`) 和標題 (`Title`) 均由要在 UI 中使用此元件的元件所提供。</span><span class="sxs-lookup"><span data-stu-id="ebe30-133">The dialog's body content (`ChildContent`) and title (`Title`) are provided by the component that uses this component in its UI.</span></span> <span data-ttu-id="ebe30-134">`OnYes` 是按鈕的 `onclick` 事件所觸發的 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="ebe30-134">`OnYes` is a C# method triggered by the button's `onclick` event.</span></span>

<span data-ttu-id="ebe30-135">Blazor 使用自然 HTML 標籤進行 UI 組合。</span><span class="sxs-lookup"><span data-stu-id="ebe30-135">Blazor uses natural HTML tags for UI composition.</span></span> <span data-ttu-id="ebe30-136">HTML 元素會指定元件，而標記的屬性 (Attribute) 會將值傳遞至元件的屬性 (Property)。</span><span class="sxs-lookup"><span data-stu-id="ebe30-136">HTML elements specify components, and a tag's attributes pass values to a component's properties.</span></span>

<span data-ttu-id="ebe30-137">在下列範例中， `Index` 元件使用 `Dialog` 元件。</span><span class="sxs-lookup"><span data-stu-id="ebe30-137">In the following example, the `Index` component uses the `Dialog` component.</span></span> <span data-ttu-id="ebe30-138">`ChildContent` 與 `Title` 是由 `<Dialog>` 元素的屬性與內容設定的。</span><span class="sxs-lookup"><span data-stu-id="ebe30-138">`ChildContent` and `Title` are set by the attributes and content of the `<Dialog>` element.</span></span>

<span data-ttu-id="ebe30-139">`Pages/Index.razor`:</span><span class="sxs-lookup"><span data-stu-id="ebe30-139">`Pages/Index.razor`:</span></span>

```razor
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

<span data-ttu-id="ebe30-140">當您 `Pages/Index.razor` 在瀏覽器中存取父 () 時，會轉譯對話方塊：</span><span class="sxs-lookup"><span data-stu-id="ebe30-140">The dialog is rendered when the parent (`Pages/Index.razor`) is accessed in a browser:</span></span>

![瀏覽器中轉譯的對話方塊元件](index/_static/dialog.png)

<span data-ttu-id="ebe30-142">在應用程式中使用此元件時，[Visual Studio](/visualstudio/ide/using-intellisense) 和 [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) \(英文\) 中的 IntelliSense 會藉由完成語法和參數來加快開發速度。</span><span class="sxs-lookup"><span data-stu-id="ebe30-142">When this component is used in the app, IntelliSense in [Visual Studio](/visualstudio/ide/using-intellisense) and [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) speeds development with syntax and parameter completion.</span></span>

<span data-ttu-id="ebe30-143">元件會轉譯為瀏覽器文件物件模型 (DOM) 的記憶體內部表示法 (稱為「轉譯樹」\*\*)，可用來以彈性且有效率的方式更新 UI。</span><span class="sxs-lookup"><span data-stu-id="ebe30-143">Components render into an in-memory representation of the browser's Document Object Model (DOM) called a *render tree*, which is used to update the UI in a flexible and efficient way.</span></span>

## Blazor WebAssembly

<span data-ttu-id="ebe30-144">Blazor WebAssembly 是使用 .NET 建立互動式用戶端 web 應用程式的單一頁面應用程式架構。</span><span class="sxs-lookup"><span data-stu-id="ebe30-144">Blazor WebAssembly is a single-page app framework for building interactive client-side web apps with .NET.</span></span> <span data-ttu-id="ebe30-145">Blazor WebAssembly 使用開放式 web 標準，而不使用外掛程式或程式碼轉譯，並適用于所有新式網頁瀏覽器，包括行動瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ebe30-145">Blazor WebAssembly uses open web standards without plugins or code transpilation and works in all modern web browsers, including mobile browsers.</span></span>

<span data-ttu-id="ebe30-146">[WebAssembly](https://webassembly.org) (縮寫) 可讓您在網頁瀏覽器內執行 .net 程式碼 `wasm` 。</span><span class="sxs-lookup"><span data-stu-id="ebe30-146">Running .NET code inside web browsers is made possible by [WebAssembly](https://webassembly.org) (abbreviated `wasm`).</span></span> <span data-ttu-id="ebe30-147">WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。</span><span class="sxs-lookup"><span data-stu-id="ebe30-147">WebAssembly is a compact bytecode format optimized for fast download and maximum execution speed.</span></span> <span data-ttu-id="ebe30-148">WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。</span><span class="sxs-lookup"><span data-stu-id="ebe30-148">WebAssembly is an open web standard and supported in web browsers without plugins.</span></span>

<span data-ttu-id="ebe30-149">WebAssembly 程式碼可以透過 JavaScript 存取瀏覽器的完整功能，稱為 「JavaScript 互通性」\*\*(或 *JavaScript Interop*)。</span><span class="sxs-lookup"><span data-stu-id="ebe30-149">WebAssembly code can access the full functionality of the browser via JavaScript, called *JavaScript interoperability* (or *JavaScript interop*).</span></span> <span data-ttu-id="ebe30-150">在瀏覽器中透過 WebAssembly 執行的 .NET 程式碼會在瀏覽器的 JavaScript 沙箱執行，且受沙箱所提供對用戶端電腦之惡意動作的保護。</span><span class="sxs-lookup"><span data-stu-id="ebe30-150">.NET code executed via WebAssembly in the browser runs in the browser's JavaScript sandbox with the protections that the sandbox provides against malicious actions on the client machine.</span></span>

![：：：非 loc (Blazor WebAssembly) ：：：在瀏覽器中使用 WebAssembly 執行 .NET 程式碼。](index/_static/blazor-webassembly.png)

<span data-ttu-id="ebe30-152">當 Blazor WebAssembly 應用程式在瀏覽器中建立並執行時：</span><span class="sxs-lookup"><span data-stu-id="ebe30-152">When a Blazor WebAssembly app is built and run in a browser:</span></span>

* <span data-ttu-id="ebe30-153">C # 程式碼檔案和檔案 Razor 會編譯成 .net 元件。</span><span class="sxs-lookup"><span data-stu-id="ebe30-153">C# code files and Razor files are compiled into .NET assemblies.</span></span>
* <span data-ttu-id="ebe30-154">組件和 .NET 執行階段會下載至瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="ebe30-154">The assemblies and the .NET runtime are downloaded to the browser.</span></span>
* <span data-ttu-id="ebe30-155">Blazor WebAssembly 啟動 .NET 執行時間，並設定執行時間以載入應用程式的元件。</span><span class="sxs-lookup"><span data-stu-id="ebe30-155">Blazor WebAssembly bootstraps the .NET runtime and configures the runtime to load the assemblies for the app.</span></span> <span data-ttu-id="ebe30-156">執行時間會 Blazor WebAssembly 使用 JavaScript interop 來處理 DOM 操作和瀏覽器 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="ebe30-156">The Blazor WebAssembly runtime uses JavaScript interop to handle DOM manipulation and browser API calls.</span></span>

<span data-ttu-id="ebe30-157">發佈的應用程式大小 (它的「承載大小」\*\*) 是應用程式使用性的重要效能因素。</span><span class="sxs-lookup"><span data-stu-id="ebe30-157">The size of the published app, its *payload size*, is a critical performance factor for an app's useability.</span></span> <span data-ttu-id="ebe30-158">大型應用程式需要相對較長的時間才能下載至瀏覽器，這點會對使用者體驗造成傷害。</span><span class="sxs-lookup"><span data-stu-id="ebe30-158">A large app takes a relatively long time to download to a browser, which diminishes the user experience.</span></span> <span data-ttu-id="ebe30-159">Blazor WebAssembly 優化承載大小以縮短下載時間：</span><span class="sxs-lookup"><span data-stu-id="ebe30-159">Blazor WebAssembly optimizes payload size to reduce download times:</span></span>

::: moniker range=">= aspnetcore-5.0"

* <span data-ttu-id="ebe30-160">當應用程式由 [中繼語言 (IL) 修剪器](xref:blazor/host-and-deploy/configure-trimmer)發行時，未使用的程式碼會從應用程式中移除。</span><span class="sxs-lookup"><span data-stu-id="ebe30-160">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Trimmer](xref:blazor/host-and-deploy/configure-trimmer).</span></span>
* <span data-ttu-id="ebe30-161">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="ebe30-161">HTTP responses are compressed.</span></span>
* <span data-ttu-id="ebe30-162">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="ebe30-162">The .NET runtime and assemblies are cached in the browser.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* <span data-ttu-id="ebe30-163">若應用程式是透過[中繼語言 (IL) 連接器](xref:blazor/host-and-deploy/configure-linker)所發佈的，則會移除未使用的程式碼。</span><span class="sxs-lookup"><span data-stu-id="ebe30-163">Unused code is stripped out of the app when it's published by the [Intermediate Language (IL) Linker](xref:blazor/host-and-deploy/configure-linker).</span></span>
* <span data-ttu-id="ebe30-164">HTTP 回應會進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="ebe30-164">HTTP responses are compressed.</span></span>
* <span data-ttu-id="ebe30-165">.NET 執行階段與組件會在瀏覽器中進行快取。</span><span class="sxs-lookup"><span data-stu-id="ebe30-165">The .NET runtime and assemblies are cached in the browser.</span></span>

::: moniker-end

## Blazor Server

<span data-ttu-id="ebe30-166">Blazor 將元件轉譯邏輯與應用程式 UI 更新的套用方式分離出來。</span><span class="sxs-lookup"><span data-stu-id="ebe30-166">Blazor decouples component rendering logic from how UI updates are applied.</span></span> <span data-ttu-id="ebe30-167">Blazor Server 提供在 Razor ASP.NET Core 應用程式的伺服器上裝載元件的支援。</span><span class="sxs-lookup"><span data-stu-id="ebe30-167">Blazor Server provides support for hosting Razor components on the server in an ASP.NET Core app.</span></span> <span data-ttu-id="ebe30-168">UI 更新會透過連接來處理 [SignalR](xref:signalr/introduction) 。</span><span class="sxs-lookup"><span data-stu-id="ebe30-168">UI updates are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="ebe30-169">執行階段會處理將 UI 事件從瀏覽器傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="ebe30-169">The runtime handles sending UI events from the browser to the server and applies UI updates sent by the server back to the browser after running the components.</span></span>

<span data-ttu-id="ebe30-170">用 Blazor Server 來與瀏覽器通訊的連接也會用來處理 JavaScript interop 呼叫。</span><span class="sxs-lookup"><span data-stu-id="ebe30-170">The connection used by Blazor Server to communicate with the browser is also used to handle JavaScript interop calls.</span></span>

![：：：非 loc (Blazor Server) ：：：在伺服器上執行 .NET 程式碼，並透過：：：無 loc (SignalR) ：：： connection 與用戶端上的檔物件模型互動](index/_static/blazor-server.png)

## <a name="javascript-interop"></a><span data-ttu-id="ebe30-172">JavaScript Interop</span><span class="sxs-lookup"><span data-stu-id="ebe30-172">JavaScript interop</span></span>

<span data-ttu-id="ebe30-173">對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 存取的應用程式，元件能夠和 JavaScript 交互操作。</span><span class="sxs-lookup"><span data-stu-id="ebe30-173">For apps that require third-party JavaScript libraries and access to browser APIs, components interoperate with JavaScript.</span></span> <span data-ttu-id="ebe30-174">元件可以使用 JavaScript 可以使用的任何程式庫或 API。</span><span class="sxs-lookup"><span data-stu-id="ebe30-174">Components are capable of using any library or API that JavaScript is able to use.</span></span> <span data-ttu-id="ebe30-175">C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="ebe30-175">C# code can call into JavaScript code, and JavaScript code can call into C# code.</span></span> <span data-ttu-id="ebe30-176">如需詳細資訊，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="ebe30-176">For more information, see the following articles:</span></span>

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

## <a name="code-sharing-and-net-standard"></a><span data-ttu-id="ebe30-177">程式碼共用和 .NET Standard</span><span class="sxs-lookup"><span data-stu-id="ebe30-177">Code sharing and .NET Standard</span></span>

<span data-ttu-id="ebe30-178">Blazor 會執行 [.NET Standard 2.1](/dotnet/standard/net-standard)，讓 Blazor 專案參考符合 .NET Standard 2.1 或較早規格的程式庫。</span><span class="sxs-lookup"><span data-stu-id="ebe30-178">Blazor implements [.NET Standard 2.1](/dotnet/standard/net-standard), which enables Blazor projects to reference libraries that conform to .NET Standard 2.1 or earlier specifications.</span></span> <span data-ttu-id="ebe30-179">.NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。</span><span class="sxs-lookup"><span data-stu-id="ebe30-179">.NET Standard is a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="ebe30-180">.NET Standard 類別庫可以跨不同的 .NET 平臺共用，例如 Blazor .NET Framework、.Net Core、Xamarin、Mono 和 Unity。</span><span class="sxs-lookup"><span data-stu-id="ebe30-180">.NET Standard class libraries can be shared across different .NET platforms, such as Blazor, .NET Framework, .NET Core, Xamarin, Mono, and Unity.</span></span>

<span data-ttu-id="ebe30-181">網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端與執行緒) 均會擲回 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="ebe30-181">APIs that aren't applicable inside of a web browser (for example, accessing the file system, opening a socket, and threading) throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ebe30-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="ebe30-182">Additional resources</span></span>

* [<span data-ttu-id="ebe30-183">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="ebe30-183">WebAssembly</span></span>](https://webassembly.org/)
* <xref:blazor/hosting-models>
* <xref:tutorials/signalr-blazor-webassembly>
* [<span data-ttu-id="ebe30-184">C# 指南</span><span class="sxs-lookup"><span data-stu-id="ebe30-184">C# Guide</span></span>](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [<span data-ttu-id="ebe30-185">HTML</span><span class="sxs-lookup"><span data-stu-id="ebe30-185">HTML</span></span>](https://www.w3.org/html/)
* <span data-ttu-id="ebe30-186">[很 Blazor 棒](https://github.com/AdrienTorris/awesome-blazor)社區連結</span><span class="sxs-lookup"><span data-stu-id="ebe30-186">[Awesome Blazor](https://github.com/AdrienTorris/awesome-blazor) community links</span></span>
