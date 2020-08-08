---
title: ASP.NET Core 中的元件標記協助程式
author: guardrex
ms.author: riande
description: 瞭解如何使用 ASP.NET Core 元件標記協助程式來轉譯 Razor 頁面和視圖中的元件。
ms.custom: mvc
ms.date: 04/15/2020
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
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 09291b537e35d00df6f8006aaccdf4db12acfaea
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88018685"
---
# <a name="component-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的元件標記協助程式

作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)

若要從頁面或視圖呈現元件，請使用[元件標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)協助程式。

## <a name="prerequisites"></a>必要條件

請遵循文章的*準備應用程式以使用頁面和 views 中的元件*一節中的指導方針 <xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps#prepare-the-app> 。

## <a name="component-tag-helper"></a>元件標記協助程式

下列元件標記協助程式會 `Counter` 在頁面或視圖中呈現元件：

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Pages

...

<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

上述範例假設 `Counter` 元件位於應用程式的*Pages*資料夾中。 預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如， `@using BlazorSample.Pages`) 。

元件標記協助程式也可以將參數傳遞給元件。 請考慮下列 `ColorfulCheckbox` 設定核取方塊標籤色彩和大小的元件：

```razor
<label style="font-size:@(Size)px;color:@Color">
    <input @bind="Value"
           id="survey" 
           name="blazor" 
           type="checkbox" />
    Enjoying Blazor?
</label>

@code {
    [Parameter]
    public bool Value { get; set; }

    [Parameter]
    public int Size { get; set; } = 8;

    [Parameter]
    public string Color { get; set; }

    protected override void OnInitialized()
    {
        Size += 10;
    }
}
```

`Size` `int` `Color` `string` 元件標記協助程式可以設定 () 和 () [元件參數](xref:blazor/components/index#component-parameters)：

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Shared

...

<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

上述範例假設 `ColorfulCheckbox` 元件位於應用程式的*共用*資料夾中。 預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如， `@using BlazorSample.Shared`) 。

下列 HTML 會在頁面或視圖中呈現：

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

傳遞加上引號的字串需要[明確的 Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions)，如 `param-Color` 上述範例中所示。 Razor類型值的剖析行為不適用於 `string` 屬性， `param-*` 因為屬性是 `object` 類型。

參數類型必須是可序列化的 JSON，這通常表示該類型必須有預設的函式和可設定的屬性。 例如，您可以 `Size` 在上述範例中指定和的值，因為和的型別 `Color` `Size` `Color` 是 (和) 的基本型別 `int` ，但 `string` JSON 序列化程式支援這些類型。

在下列範例中，類別物件會傳遞至元件：

*MyClass.cs*：

```csharp
public class MyClass
{
    public MyClass()
    {
    }

    public int MyInt { get; set; } = 999;
    public string MyString { get; set; } = "Initial value";
}
```

**類別必須有公用無參數的函式。**

*Shared/mycomponent 之下. razor*：

```razor
<h2>MyComponent</h2>

<p>Int: @MyObject.MyInt</p>
<p>String: @MyObject.MyString</p>

@code
{
    [Parameter]
    public MyClass MyObject { get; set; }
}
```

*Pages/mypage.aspx. cshtml*：

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}
@using {APP ASSEMBLY}.Shared

...

@{
    var myObject = new MyClass();
    myObject.MyInt = 7;
    myObject.MyString = "Set by MyPage";
}

<component type="typeof(MyComponent)" render-mode="ServerPrerendered" 
    param-MyObject="@myObject" />
```

上述範例假設 `MyComponent` 元件位於應用程式的*共用*資料夾中。 預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如， `@using BlazorSample` 而 `@using BlazorSample.Shared`) 。 `MyClass`位於應用程式的命名空間中。

<xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode>設定元件是否：

* 會資源清單到頁面中。
* 會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。

| 轉譯模式 | 描述 |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | 將元件轉譯為靜態 HTML，並包含 Blazor Server 應用程式的標記。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | 呈現應用程式的標記 Blazor Server 。 不包含來自元件的輸出。 當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。 |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | 將元件轉譯為靜態 HTML。 |

雖然頁面和視圖可以使用元件，但相反的情況並非如此。 元件不能使用 view 和 page 特有的功能，例如部分視圖和區段。 若要在元件中使用部分視圖的邏輯，請將部分視圖邏輯分解成元件。

不支援從靜態 HTML 網頁轉譯伺服器元件。

## <a name="additional-resources"></a>其他資源

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components/index>
