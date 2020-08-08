---
title: ASP.NET Core Blazor WebAssembly 效能最佳做法
author: pranavkm
description: 提升 ASP.NET Core Blazor WebAssembly 應用程式效能及避免常見效能問題的秘訣。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/25/2020
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
uid: blazor/webassembly-performance-best-practices
ms.openlocfilehash: 64205dcf7aeecbe594b0164a7966b7f96bcca68b
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88013355"
---
# <a name="aspnet-core-no-locblazor-webassembly-performance-best-practices"></a>ASP.NET Core Blazor WebAssembly 效能最佳做法

依[Pranav Krishnamoorthy](https://github.com/pranavkm)

本文提供 ASP.NET Core Blazor WebAssembly 效能最佳做法的指導方針。

## <a name="avoid-unnecessary-component-renders"></a>避免不必要的元件呈現

Blazor當演算法察覺元件尚未變更時，其比較演算法可避免 rerendering 元件。 覆寫 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A?displayProperty=nameWithType> 以精細控制元件轉譯。

如果撰寫的 UI 專用元件永遠不會在初始轉譯之後變更，請將設定 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 為傳回 `false` ：

```razor
@code {
    protected override bool ShouldRender() => false;
}
```

大部分的應用程式都不需要更精細的控制，但是 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 可以用來選擇性地呈現回應 UI 事件的元件。 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>在轉譯大量元件的情況下，使用可能也很重要。 假設有一個方格，其中在方格的一個資料格的某個 <xref:Microsoft.AspNetCore.Components.EventCallback> 儲存格中，使用 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> 。 呼叫 <xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> 會導致每個子元件的重新呈現。 如果只有少數的資料格需要 rerendering，請使用 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 來避免不必要的轉譯效能負面影響。

在下例中︰

* <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A>會覆寫並設定為欄位的值 <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> ，這是一開始 `false` 元件載入時。
* 選取按鈕時， <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> 會設定為 `true` ，這會強制元件 rerender 已更新的 `currentCount` 。
* 緊接在 rerendering 之後，會 <xref:Microsoft.AspNetCore.Components.ComponentBase.OnAfterRender%2A> 將的值設定為， <xref:Microsoft.AspNetCore.Components.ComponentBase.ShouldRender%2A> `false` 以防止進一步 rerendering，直到下一次選取按鈕為止。

```razor
<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>

@code {
    private int currentCount = 0;
    private bool shouldRender;

    protected override bool ShouldRender() => shouldRender;

    protected override void OnAfterRender(bool first)
    {
        shouldRender = false;
    }

    private void IncrementCount()
    {
        currentCount++;
        shouldRender = true;
    }
}
```

如需詳細資訊，請參閱<xref:blazor/components/lifecycle#after-component-render>。

## <a name="virtualize-re-usable-fragments"></a>虛擬化重複使用的片段

元件提供便利的方法來產生可重複使用的程式碼和標記片段。 一般來說，我們建議您撰寫最符合應用程式需求的個別元件。 有一點要注意的是，每個額外的子元件都會產生呈現父元件所需的總時間。 對於大部分的應用程式而言，額外的負荷是可忽略的。 產生大量元件的應用程式應該考慮使用策略來減少處理額外負荷，例如限制轉譯的元件數目。

例如，呈現數百個包含元件之資料列的方格或清單會耗用大量處理器來呈現。 請考慮將方格或清單配置虛擬化，如此一來，就只會在任何指定的時間轉譯元件的子集。 如需元件子集轉譯的範例，請參閱範例應用程式中的下列元件[ `Virtualization` (aspnet/samples GitHub 存放庫) ](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/blazor/Virtualization)：

* `Virtualize`元件 ([`Shared/Virtualize.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Shared/Virtualize.cs)) ：以 c # 撰寫的元件，它會 <xref:Microsoft.AspNetCore.Components.ComponentBase> 根據使用者的滾動來執行，以轉譯一組氣象資料列。
* `FetchData`元件 ([`Pages/FetchData.razor`](https://github.com/aspnet/samples/blob/master/samples/aspnetcore/blazor/Virtualization/Pages/FetchData.razor)) ：使用 `Virtualize` 元件一次顯示25列的天氣資料。

## <a name="avoid-javascript-interop-to-marshal-data"></a>避免 JavaScript interop 來封送處理資料

在中 Blazor WebAssembly ，JavaScript (JS) interop 呼叫必須流經 WebAssembly-js 界限。 在這兩個內容中序列化和還原序列化內容，會建立應用程式的處理額外負荷。 經常的 JS interop 呼叫通常會對效能造成不良影響。 若要減少跨界限的資料封送處理，請判斷應用程式是否可以將許多小型承載合併成單一大型承載，以避免在 WebAssembly 和 JS 之間進行大量的內容切換。

## <a name="use-systemtextjson"></a>使用 System.Text.Js

Blazor的 JS interop 實現依賴 <xref:System.Text.Json> ，這是具有低記憶體配置的高效能 JSON 序列化程式庫。 使用 <xref:System.Text.Json> 不會導致額外的應用程式裝載大小超過新增一或多個替代 JSON 程式庫。

如需遷移指引，請參閱[如何從 `Newtonsoft.Json` 遷移 `System.Text.Json` 至](/dotnet/standard/serialization/system-text-json-migrate-from-newtonsoft-how-to)。

## <a name="use-synchronous-and-unmarshalled-js-interop-apis-where-appropriate"></a>適當時使用同步和 unmarshalled JS interop Api

Blazor WebAssembly在 <xref:Microsoft.JSInterop.IJSRuntime> 應用程式可用的單一版本上，提供兩個額外版本 Blazor Server ：

* <xref:Microsoft.JSInterop.IJSInProcessRuntime>允許同步叫用 JS interop 呼叫，其額外負荷比非同步版本少：

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (IJSInProcessRuntime)JS;

          var value = jsInProcess.Invoke<string>("jsInteropCall");
      }
  }
  ```

* <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime>允許 unmarshalled JS interop 呼叫：

  ```javascript
  function jsInteropCall() {
    return BINDING.js_to_mono_obj("Hello world");
  }
  ```

  ```razor
  @inject IJSRuntime JS

  @code {
      protected override void OnInitialized()
      {
          var jsInProcess = (WebAssemblyJSRuntime)JS;

          var value = jsInProcess.InvokeUnmarshalled<string>("jsInteropCall");
      }
  }
  ```

  > [!WARNING]
  > 雖然使用的是 <xref:Microsoft.JSInterop.WebAssembly.WebAssemblyJSRuntime> JS interop 方法的最小額外負荷，但目前尚未記載與這些 api 互動所需的 JavaScript api，並在未來的版本中受到重大變更的影響。

## <a name="reduce-app-size"></a>減少應用程式大小

### <a name="intermediate-language-il-linking"></a> (IL) 連結的中繼語言

[連結 Blazor WebAssembly 應用程式](xref:blazor/host-and-deploy/configure-linker)會藉由在應用程式的二進位檔中修剪未使用的程式碼，來減少應用程式的大小。 根據預設，只有在設定中建立時，才會啟用連結器 `Release` 。 若要受益于此，請使用命令發佈應用程式以進行部署， [`dotnet publish`](/dotnet/core/tools/dotnet-publish) 並將[-c |--configuration](/dotnet/core/tools/dotnet-publish#options)選項設為 `Release` ：

```dotnetcli
dotnet publish -c Release
```

### <a name="lazy-load-assemblies"></a>延遲載入元件

當路由需要元件時，在執行時間載入元件。 如需詳細資訊，請參閱<xref:blazor/webassembly-lazy-load-assemblies>。

### <a name="compression"></a>壓縮

當 Blazor WebAssembly 應用程式發佈時，會在發佈期間以靜態方式壓縮輸出，以減少應用程式的大小，並移除執行時間壓縮的額外負荷。 Blazor依賴伺服器來執行 content negotation，並提供靜態壓縮檔案。

部署應用程式之後，請確認應用程式會提供壓縮檔案。 檢查瀏覽器開發人員工具中的 [網路] 索引標籤，並確認已使用或提供檔案 `Content-Encoding: br` `Content-Encoding: gz` 。 如果主機未提供壓縮檔案，請遵循中的指示 <xref:blazor/host-and-deploy/webassembly#compression> 。

### <a name="disable-unused-features"></a>停用未使用的功能

Blazor WebAssembly的執行時間包含下列 .NET 功能，如果應用程式不需要它們來取得較小的承載大小，就可以停用這些功能：

* 包含資料檔案可讓時區資訊正確無誤。 如果應用程式不需要這項功能，請考慮將 `BlazorEnableTimeZoneSupport` 應用程式的專案檔中的 MSBuild 屬性設定為來停用它 `false` ：

  ```xml
  <PropertyGroup>
    <BlazorEnableTimeZoneSupport>false</BlazorEnableTimeZoneSupport>
  </PropertyGroup>
  ```

* 已包含定序資訊，讓 Api （如 <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> 工作）正確運作。 如果您確定應用程式不需要定序資料，請考慮將 `BlazorWebAssemblyPreserveCollationData` 應用程式的專案檔中的 MSBuild 屬性設定為來停用它 `false` ：

  ```xml
  <PropertyGroup>
    <BlazorWebAssemblyPreserveCollationData>false</BlazorWebAssemblyPreserveCollationData>
  </PropertyGroup>
  ```
