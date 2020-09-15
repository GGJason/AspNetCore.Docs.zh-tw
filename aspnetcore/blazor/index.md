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
# <a name="introduction-to-aspnet-core-no-locblazor"></a>ASP.NET Core 簡介 Blazor

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

*歡迎使用 Blazor ！*

Blazor 是使用 .NET 建立互動式用戶端 web UI 的架構：

* 使用 C# 而不是 JavaScript 來建立豐富的互動式 UI。
* 共用以 .NET 撰寫的伺服器端與用戶端應用程式邏輯。
* 將 UI 轉譯為 HTML 和 CSS 以支援寬瀏覽器，包括行動裝置瀏覽器。
* 與新式裝載平臺（例如 [Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/index)）整合。

使用 .NET 進行用戶端網頁程式開發可提供下列優點：

* 以 C# 撰寫而不是 JavaScript。
* 利用 .NET 程式庫的現有 .NET 生態系統。
* 跨伺服器和用戶端共用應用程式邏輯。
* 從 .NET 的效能、可靠性和安全性中獲益。
* 使用 Windows、Linux 和 macOS 版的 Visual Studio 保持生產力。
* 以常用的語言、架構和工具建置，不僅穩定、功能豐富，而且容易使用。

## <a name="components"></a>單元

Blazor 應用程式是以 *元件*為基礎。 中的元件 Blazor 是 UI 的元素，例如頁面、對話方塊或資料輸入表單。

元件是內建在 .NET 組件中且具有下列功能的 .NET 類別：

* 定義彈性 UI 轉譯邏輯。
* 處理使用者動作。
* 可以為巢狀結構，且可重複使用。
* 可以共用和散發為[ Razor 類別庫](xref:razor-pages/ui-class)或[NuGet 套件](/nuget/what-is-nuget)。

元件類別通常是以 [Razor](xref:mvc/views/razor) 具有副檔名的標記頁面形式來撰寫 `.razor` 。 中的元件 Blazor 統稱為* Razor 元件*。 Razor 是一種語法，可將 HTML 標籤與設計為開發人員生產力的 c # 程式碼結合。 Razor 可讓您在具有 [IntelliSense](/visualstudio/ide/using-intellisense) 支援的相同檔案中，于 HTML 標籤和 c # 之間切換。 Razor Pages 和 MVC 也會使用 Razor 。 不同 Razor 于建立在要求/回應模型周圍的頁面和 MVC，元件專門用於用戶端 UI 邏輯和組合。

下列 Razor 標記示範 `Dialog.razor` 可在另一個元件中嵌套的元件 () ：

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

對話方塊的內文內容 (`ChildContent`) 和標題 (`Title`) 均由要在 UI 中使用此元件的元件所提供。 `OnYes` 是按鈕的 `onclick` 事件所觸發的 C# 方法。

Blazor 使用自然 HTML 標籤進行 UI 組合。 HTML 元素會指定元件，而標記的屬性 (Attribute) 會將值傳遞至元件的屬性 (Property)。

在下列範例中， `Index` 元件使用 `Dialog` 元件。 `ChildContent` 與 `Title` 是由 `<Dialog>` 元素的屬性與內容設定的。

`Pages/Index.razor`:

```razor
@page "/"

<h1>Hello, world!</h1>

Welcome to your new app.

<Dialog Title="Blazor">
    Do you want to <i>learn more</i> about Blazor?
</Dialog>
```

當您 `Pages/Index.razor` 在瀏覽器中存取父 () 時，會轉譯對話方塊：

![瀏覽器中轉譯的對話方塊元件](index/_static/dialog.png)

在應用程式中使用此元件時，[Visual Studio](/visualstudio/ide/using-intellisense) 和 [Visual Studio Code](https://code.visualstudio.com/docs/editor/intellisense) \(英文\) 中的 IntelliSense 會藉由完成語法和參數來加快開發速度。

元件會轉譯為瀏覽器文件物件模型 (DOM) 的記憶體內部表示法 (稱為「轉譯樹」**)，可用來以彈性且有效率的方式更新 UI。

## Blazor WebAssembly

Blazor WebAssembly 是使用 .NET 建立互動式用戶端 web 應用程式的單一頁面應用程式架構。 Blazor WebAssembly 使用開放式 web 標準，而不使用外掛程式或程式碼轉譯，並適用于所有新式網頁瀏覽器，包括行動瀏覽器。

[WebAssembly](https://webassembly.org) (縮寫) 可讓您在網頁瀏覽器內執行 .net 程式碼 `wasm` 。 WebAssembly 是一種精簡的位元組程式碼格式，針對快速下載和最快執行速度而最佳化。 WebAssembly 是開放式的 Web 標準，在不含外掛程式的網頁瀏覽器中支援。

WebAssembly 程式碼可以透過 JavaScript 存取瀏覽器的完整功能，稱為 「JavaScript 互通性」**(或 *JavaScript Interop*)。 在瀏覽器中透過 WebAssembly 執行的 .NET 程式碼會在瀏覽器的 JavaScript 沙箱執行，且受沙箱所提供對用戶端電腦之惡意動作的保護。

![：：：非 loc (Blazor WebAssembly) ：：：在瀏覽器中使用 WebAssembly 執行 .NET 程式碼。](index/_static/blazor-webassembly.png)

當 Blazor WebAssembly 應用程式在瀏覽器中建立並執行時：

* C # 程式碼檔案和檔案 Razor 會編譯成 .net 元件。
* 組件和 .NET 執行階段會下載至瀏覽器中。
* Blazor WebAssembly 啟動 .NET 執行時間，並設定執行時間以載入應用程式的元件。 執行時間會 Blazor WebAssembly 使用 JavaScript interop 來處理 DOM 操作和瀏覽器 API 呼叫。

發佈的應用程式大小 (它的「承載大小」**) 是應用程式使用性的重要效能因素。 大型應用程式需要相對較長的時間才能下載至瀏覽器，這點會對使用者體驗造成傷害。 Blazor WebAssembly 優化承載大小以縮短下載時間：

::: moniker range=">= aspnetcore-5.0"

* 當應用程式由 [中繼語言 (IL) 修剪器](xref:blazor/host-and-deploy/configure-trimmer)發行時，未使用的程式碼會從應用程式中移除。
* HTTP 回應會進行壓縮。
* .NET 執行階段與組件會在瀏覽器中進行快取。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* 若應用程式是透過[中繼語言 (IL) 連接器](xref:blazor/host-and-deploy/configure-linker)所發佈的，則會移除未使用的程式碼。
* HTTP 回應會進行壓縮。
* .NET 執行階段與組件會在瀏覽器中進行快取。

::: moniker-end

## Blazor Server

Blazor 將元件轉譯邏輯與應用程式 UI 更新的套用方式分離出來。 Blazor Server 提供在 Razor ASP.NET Core 應用程式的伺服器上裝載元件的支援。 UI 更新會透過連接來處理 [SignalR](xref:signalr/introduction) 。

執行階段會處理將 UI 事件從瀏覽器傳送到伺服器，然後在執行元件之後，套用由伺服器傳送回瀏覽器的 UI 更新。

用 Blazor Server 來與瀏覽器通訊的連接也會用來處理 JavaScript interop 呼叫。

![：：：非 loc (Blazor Server) ：：：在伺服器上執行 .NET 程式碼，並透過：：：無 loc (SignalR) ：：： connection 與用戶端上的檔物件模型互動](index/_static/blazor-server.png)

## <a name="javascript-interop"></a>JavaScript Interop

對於需要協力廠商 JavaScript 程式庫和瀏覽器 API 存取的應用程式，元件能夠和 JavaScript 交互操作。 元件可以使用 JavaScript 可以使用的任何程式庫或 API。 C# 程式碼可以呼叫進入 JavaScript 程式碼，而 JavaScript 程式碼可以呼叫進入 C# 程式碼。 如需詳細資訊，請參閱下列文章：

* <xref:blazor/call-javascript-from-dotnet>
* <xref:blazor/call-dotnet-from-javascript>

## <a name="code-sharing-and-net-standard"></a>程式碼共用和 .NET Standard

Blazor 會執行 [.NET Standard 2.1](/dotnet/standard/net-standard)，讓 Blazor 專案參考符合 .NET Standard 2.1 或較早規格的程式庫。 .NET Standard 是所有 .NET 實作都具備的 .NET API 正式規格。 .NET Standard 類別庫可以跨不同的 .NET 平臺共用，例如 Blazor .NET Framework、.Net Core、Xamarin、Mono 和 Unity。

網頁瀏覽器內不適用的 API (例如，存取檔案系統、開啟通訊端與執行緒) 均會擲回 <xref:System.PlatformNotSupportedException>。

## <a name="additional-resources"></a>其他資源

* [WebAssembly](https://webassembly.org/)
* <xref:blazor/hosting-models>
* <xref:tutorials/signalr-blazor-webassembly>
* [C# 指南](/dotnet/csharp/)
* <xref:mvc/views/razor>
* [HTML](https://www.w3.org/html/)
* [很 Blazor 棒](https://github.com/AdrienTorris/awesome-blazor)社區連結
