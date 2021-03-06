---
title: ASP.NET Core Blazor 專案結構
author: guardrex
description: 瞭解 ASP.NET Core Blazor 應用程式專案結構。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2020
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
uid: blazor/project-structure
ms.openlocfilehash: 6df84366dc3fc99d31a8a0e614ca860a50df7f5c
ms.sourcegitcommit: 6b87f2e064cea02e65dacd206394b44f5c604282
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97513544"
---
# <a name="aspnet-core-no-locblazor-project-structure"></a>ASP.NET Core Blazor 專案結構

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

下列檔案和資料夾構成 Blazor 從專案範本產生的應用程式 Blazor ：

::: moniker range=">= aspnetcore-5.0"

* `Program.cs`：應用程式的進入點，其設定：

  * ASP.NET Core [主機](xref:fundamentals/host/generic-host) (Blazor Server) 
  * WebAssembly 主機 (Blazor WebAssembly) ：此檔案中的程式碼對從 () 的範本建立的應用程式而言是唯一的 Blazor WebAssembly `blazorwasm` 。
    * `App`元件是應用程式的根元件。 `App`元件會指定為 `div` DOM 元素，其中 `id` `app` (的 `<div id="app">Loading...</div>` `wwwroot/index.html`)  () 的根元件集合 `builder.RootComponents.Add<App>("#app")` 。
    * 系統會新增並設定[服務](xref:blazor/fundamentals/dependency-injection) (例如 `builder.Services.AddSingleton<IMyDependency, MyDependency>()`) 。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* `Program.cs`：應用程式的進入點，其設定：

  * ASP.NET Core [主機](xref:fundamentals/host/generic-host) (Blazor Server) 
  * WebAssembly 主機 (Blazor WebAssembly) ：此檔案中的程式碼對從 () 的範本建立的應用程式而言是唯一的 Blazor WebAssembly `blazorwasm` 。
    * `App`元件是應用程式的根元件。 `App`元件會指定為 `app` DOM 元素， (`<app>Loading...</app>` 在 `wwwroot/index.html`)  () 的根元件集合中 `builder.RootComponents.Add<App>("app")` 。
    * 系統會新增並設定[服務](xref:blazor/fundamentals/dependency-injection) (例如 `builder.Services.AddSingleton<IMyDependency, MyDependency>()`) 。

::: moniker-end

* `Startup.cs` (Blazor Server) ：包含應用程式的啟動邏輯。 `Startup`類別會定義兩種方法：

  * `ConfigureServices`：設定應用程式的相依性 [插入 (DI) ](xref:fundamentals/dependency-injection) 服務。 在 Blazor Server 應用程式中，會藉由呼叫來新增服務， <xref:Microsoft.Extensions.DependencyInjection.ComponentServiceCollectionExtensions.AddServerSideBlazor%2A> 並且將 `WeatherForecastService` 加入至服務容器以供範例 `FetchData` 元件使用。
  * `Configure`：設定應用程式的要求處理管線：
    * <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub%2A> 呼叫以設定與瀏覽器的即時連線端點。 使用建立連線 [SignalR](xref:signalr/introduction) ，這是將即時 web 功能新增至應用程式的架構。
    * [`MapFallbackToPage("/_Host")`](xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapFallbackToPage*) 呼叫以設定應用程式的根頁面 (`Pages/_Host.cshtml`) 並啟用導覽。

::: moniker range=">= aspnetcore-5.0"

* `wwwroot/index.html` (Blazor WebAssembly) ：實作為 HTML 網頁的應用程式根頁面：
  * 最初要求應用程式的任何頁面時，會轉譯此頁面，並在回應中傳回。
  * 頁面會指定呈現根元件的位置 `App` 。 元件是以 `div` `id` () 的 DOM 元素位置轉譯 `app` `<div id="app">Loading...</div>` 。
  * `_framework/blazor.webassembly.js`載入 JavaScript 檔案，其：
    * 下載 .NET 執行時間、應用程式和應用程式的相依性。
    * 初始化執行時間以執行應用程式。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* `wwwroot/index.html` (Blazor WebAssembly) ：實作為 HTML 網頁的應用程式根頁面：
  * 最初要求應用程式的任何頁面時，會轉譯此頁面，並在回應中傳回。
  * 頁面會指定呈現根元件的位置 `App` 。 元件會轉譯在 `app` DOM 元素 () 的位置 `<app>Loading...</app>` 。
  * `_framework/blazor.webassembly.js`載入 JavaScript 檔案，其：
    * 下載 .NET 執行時間、應用程式和應用程式的相依性。
    * 初始化執行時間以執行應用程式。

::: moniker-end

* `App.razor`：應用程式的根元件，它會使用元件來設定用戶端路由 <xref:Microsoft.AspNetCore.Components.Routing.Router> 。 <xref:Microsoft.AspNetCore.Components.Routing.Router>元件會攔截瀏覽器導覽，並轉譯符合所要求位址的頁面。

* `Pages` 資料夾：包含組成應用程式的可路由元件/頁面 (`.razor`) ， Blazor 以及 Razor 應用程式的根頁面 Blazor Server 。 每個頁面的路由都是使用指示詞來指定 [`@page`](xref:mvc/views/razor#page) 。 範本包含下列各項：
  * `_Host.cshtml` (Blazor Server) ：作為頁面執行之應用程式的根頁面 Razor ：
    * 最初要求應用程式的任何頁面時，會轉譯此頁面，並在回應中傳回。
    * `_framework/blazor.server.js`JavaScript 檔案會載入，這會設定 SignalR 瀏覽器與伺服器之間的即時連接。
    * [主機] 頁面 `App` 會指定呈現根元件 () 的位置 `App.razor` 。
  * `Counter` 元件 (`Pages/Counter.razor`) ：實行計數器頁面。
  * `Error` 元件 (`Error.razor` ， Blazor Server 僅限應用程式) ：當應用程式中發生未處理的例外狀況時轉譯。
  * `FetchData` 元件 (`Pages/FetchData.razor`) ：實行提取資料頁面。
  * `Index` 元件 (`Pages/Index.razor`) ：實行首頁。
  
* `Properties/launchSettings.json`：保留 [開發環境](xref:fundamentals/environments#development-and-launchsettingsjson)設定。

::: moniker range=">= aspnetcore-5.0"

* `Shared` 資料夾：包含 `.razor` 應用程式所使用 () 的其他 UI 元件：
  * `MainLayout` 元件 (`MainLayout.razor`) ：應用程式的 [版面配置元件](xref:blazor/layouts)。
  * `MainLayout.razor.css`：應用程式主要版面配置的樣式表單。
  * `NavMenu` 元件 (`NavMenu.razor`) ：實行提要欄位導覽。 包含 () 的[ `NavLink` 元件](xref:blazor/fundamentals/routing#navlink-component) <xref:Microsoft.AspNetCore.Components.Routing.NavLink> ，其會呈現其他元件的導覽連結 Razor 。 元件會在 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 載入元件時自動表示選取的狀態，這可協助使用者瞭解目前顯示的元件。
  * `NavMenu.razor.css`：應用程式導覽功能表的樣式表單。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

* `Shared` 資料夾：包含 `.razor` 應用程式所使用 () 的其他 UI 元件：
  * `MainLayout` 元件 (`MainLayout.razor`) ：應用程式的 [版面配置元件](xref:blazor/layouts)。
  * `NavMenu` 元件 (`NavMenu.razor`) ：實行提要欄位導覽。 包含 () 的[ `NavLink` 元件](xref:blazor/fundamentals/routing#navlink-component) <xref:Microsoft.AspNetCore.Components.Routing.NavLink> ，其會呈現其他元件的導覽連結 Razor 。 元件會在 <xref:Microsoft.AspNetCore.Components.Routing.NavLink> 載入元件時自動表示選取的狀態，這可協助使用者瞭解目前顯示的元件。
  
::: moniker-end

* `_Imports.razor`：包含 Razor 要包含在應用程式元件 () 的一般指示詞 `.razor` ，例如 [`@using`](xref:mvc/views/razor#using) 命名空間的指示詞。

* `Data` 資料夾 (Blazor Server) ：包含 `WeatherForecast` `WeatherForecastService` 提供範例天氣資料給應用程式元件的類別和執行 `FetchData` 。

* `wwwroot`：應用程式的 [Web 根](xref:fundamentals/index#web-root) 資料夾，其中包含應用程式的公用靜態資產。

* `appsettings.json`：保留應用程式的 [設定](xref:blazor/fundamentals/configuration) 。 在 Blazor WebAssembly 應用程式中，應用程式佈建檔案位於 `wwwroot` 資料夾中。 在 Blazor Server 應用程式中，應用程式佈建檔案位於專案根目錄。
