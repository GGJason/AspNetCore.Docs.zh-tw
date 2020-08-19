---
title: ASP.NET Core 中的元件標記協助程式
author: guardrex
ms.author: riande
description: 瞭解如何使用 ASP.NET Core 元件標籤協助程式來轉譯 Razor 頁面和視圖中的元件。
ms.custom: mvc
ms.date: 04/15/2020
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
uid: mvc/views/tag-helpers/builtin-th/component-tag-helper
ms.openlocfilehash: 1a0422da6bd48049cac73debe7d335da91e311be
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88633912"
---
# <a name="component-tag-helper-in-aspnet-core"></a><span data-ttu-id="15167-103">ASP.NET Core 中的元件標記協助程式</span><span class="sxs-lookup"><span data-stu-id="15167-103">Component Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="15167-104">作者：[Daniel Roth](https://github.com/danroth27) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="15167-104">By [Daniel Roth](https://github.com/danroth27) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="15167-105">若要從頁面或視圖轉譯元件，請使用 [元件標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper)協助程式。</span><span class="sxs-lookup"><span data-stu-id="15167-105">To render a component from a page or view, use the [Component Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15167-106">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="15167-106">Prerequisites</span></span>

<span data-ttu-id="15167-107">遵循本文中的「 *準備應用程式以使用頁面中的元件」和「流覽* 」一節中的指導方針 <xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps#prepare-the-app> 。</span><span class="sxs-lookup"><span data-stu-id="15167-107">Follow the guidance in the *Prepare the app to use components in pages and views* section of the <xref:blazor/components/integrate-components-into-razor-pages-and-mvc-apps#prepare-the-app> article.</span></span>

## <a name="component-tag-helper"></a><span data-ttu-id="15167-108">元件標記協助程式</span><span class="sxs-lookup"><span data-stu-id="15167-108">Component Tag Helper</span></span>

<span data-ttu-id="15167-109">下列元件標籤協助程式會 `Counter` 在頁面或視圖中呈現元件：</span><span class="sxs-lookup"><span data-stu-id="15167-109">The following Component Tag Helper renders the `Counter` component in a page or view:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Pages

...

<component type="typeof(Counter)" render-mode="ServerPrerendered" />
```

<span data-ttu-id="15167-110">上述範例假設 `Counter` 元件是在應用程式的 *Pages* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="15167-110">The preceding example assumes that the `Counter` component is in the app's *Pages* folder.</span></span> <span data-ttu-id="15167-111">預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如 `@using BlazorSample.Pages`) 。</span><span class="sxs-lookup"><span data-stu-id="15167-111">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `@using BlazorSample.Pages`).</span></span>

<span data-ttu-id="15167-112">元件標記協助程式也可以將參數傳遞至元件。</span><span class="sxs-lookup"><span data-stu-id="15167-112">The Component Tag Helper can also pass parameters to components.</span></span> <span data-ttu-id="15167-113">請考慮下列 `ColorfulCheckbox` 設定核取方塊標籤色彩和大小的元件：</span><span class="sxs-lookup"><span data-stu-id="15167-113">Consider the following `ColorfulCheckbox` component that sets the check box label's color and size:</span></span>

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

<span data-ttu-id="15167-114">`Size` `int` `Color` `string` 元件標記協助程式可以設定 () 和 () [元件參數](xref:blazor/components/index#component-parameters)：</span><span class="sxs-lookup"><span data-stu-id="15167-114">The `Size` (`int`) and `Color` (`string`) [component parameters](xref:blazor/components/index#component-parameters) can be set by the Component Tag Helper:</span></span>

```cshtml
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@using {APP ASSEMBLY}.Shared

...

<component type="typeof(ColorfulCheckbox)" render-mode="ServerPrerendered" 
    param-Size="14" param-Color="@("blue")" />
```

<span data-ttu-id="15167-115">上述範例假設 `ColorfulCheckbox` 元件是在應用程式的 *共用* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="15167-115">The preceding example assumes that the `ColorfulCheckbox` component is in the app's *Shared* folder.</span></span> <span data-ttu-id="15167-116">預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如 `@using BlazorSample.Shared`) 。</span><span class="sxs-lookup"><span data-stu-id="15167-116">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `@using BlazorSample.Shared`).</span></span>

<span data-ttu-id="15167-117">下列 HTML 會在頁面或視圖中呈現：</span><span class="sxs-lookup"><span data-stu-id="15167-117">The following HTML is rendered in the page or view:</span></span>

```html
<label style="font-size:24px;color:blue">
    <input id="survey" name="blazor" type="checkbox">
    Enjoying Blazor?
</label>
```

<span data-ttu-id="15167-118">傳遞引號字串需要明確的 [ Razor 運算式](xref:mvc/views/razor#explicit-razor-expressions)，如 `param-Color` 先前範例中所示。</span><span class="sxs-lookup"><span data-stu-id="15167-118">Passing a quoted string requires an [explicit Razor expression](xref:mvc/views/razor#explicit-razor-expressions), as shown for `param-Color` in the preceding example.</span></span> <span data-ttu-id="15167-119">Razor `string` 因為屬性是類型，所以類型值的剖析行為不會套用至 `param-*` 屬性 `object` 。</span><span class="sxs-lookup"><span data-stu-id="15167-119">The Razor parsing behavior for a `string` type value doesn't apply to a `param-*` attribute because the attribute is an `object` type.</span></span>

<span data-ttu-id="15167-120">參數類型必須是 JSON 可序列化的，這通常表示型別必須具有預設的函式和可設定的屬性。</span><span class="sxs-lookup"><span data-stu-id="15167-120">The parameter type must be JSON serializable, which typically means that the type must have a default constructor and settable properties.</span></span> <span data-ttu-id="15167-121">例如，您可以 `Size` `Color` 在前面的範例中指定和的值，因為和的型 `Size` 別 `Color` 是 (`int` 和 `string`) 的基本類型，但 JSON 序列化程式支援這些類型。</span><span class="sxs-lookup"><span data-stu-id="15167-121">For example, you can specify a value for `Size` and `Color` in the preceding example because the types of `Size` and `Color` are primitive types (`int` and `string`), which are supported by the JSON serializer.</span></span>

<span data-ttu-id="15167-122">在下列範例中，會將類別物件傳遞給元件：</span><span class="sxs-lookup"><span data-stu-id="15167-122">In the following example, a class object is passed to the component:</span></span>

<span data-ttu-id="15167-123">*MyClass.cs*：</span><span class="sxs-lookup"><span data-stu-id="15167-123">*MyClass.cs*:</span></span>

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

<span data-ttu-id="15167-124">**類別必須有公用無參數的函式。**</span><span class="sxs-lookup"><span data-stu-id="15167-124">**The class must have a public parameterless constructor.**</span></span>

<span data-ttu-id="15167-125">*Shared/MyComponent razor*：</span><span class="sxs-lookup"><span data-stu-id="15167-125">*Shared/MyComponent.razor*:</span></span>

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

<span data-ttu-id="15167-126">*Pages/mypage.aspx. cshtml*：</span><span class="sxs-lookup"><span data-stu-id="15167-126">*Pages/MyPage.cshtml*:</span></span>

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

<span data-ttu-id="15167-127">上述範例假設 `MyComponent` 元件是在應用程式的 *共用* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="15167-127">The preceding example assumes that the `MyComponent` component is in the app's *Shared* folder.</span></span> <span data-ttu-id="15167-128">預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如， `@using BlazorSample` 以及 `@using BlazorSample.Shared`) 。</span><span class="sxs-lookup"><span data-stu-id="15167-128">The placeholder `{APP ASSEMBLY}` is the app's assembly name (for example, `@using BlazorSample` and `@using BlazorSample.Shared`).</span></span> <span data-ttu-id="15167-129">`MyClass` 位於應用程式的命名空間中。</span><span class="sxs-lookup"><span data-stu-id="15167-129">`MyClass` is in the app's namespace.</span></span>

<span data-ttu-id="15167-130"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> 設定元件是否為：</span><span class="sxs-lookup"><span data-stu-id="15167-130"><xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode> configures whether the component:</span></span>

* <span data-ttu-id="15167-131">會資源清單到頁面中。</span><span class="sxs-lookup"><span data-stu-id="15167-131">Is prerendered into the page.</span></span>
* <span data-ttu-id="15167-132">會在頁面上轉譯為靜態 HTML，或包含從使用者代理程式啟動應用程式所需的資訊 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="15167-132">Is rendered as static HTML on the page or if it includes the necessary information to bootstrap a Blazor app from the user agent.</span></span>

| <span data-ttu-id="15167-133">轉譯模式</span><span class="sxs-lookup"><span data-stu-id="15167-133">Render Mode</span></span> | <span data-ttu-id="15167-134">描述</span><span class="sxs-lookup"><span data-stu-id="15167-134">Description</span></span> |
| ----------- | ----------- |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.ServerPrerendered> | <span data-ttu-id="15167-135">將元件轉譯為靜態 HTML，並包含 Blazor Server 應用程式的標記。</span><span class="sxs-lookup"><span data-stu-id="15167-135">Renders the component into static HTML and includes a marker for a Blazor Server app.</span></span> <span data-ttu-id="15167-136">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="15167-136">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Server> | <span data-ttu-id="15167-137">轉譯應用程式的標記 Blazor Server 。</span><span class="sxs-lookup"><span data-stu-id="15167-137">Renders a marker for a Blazor Server app.</span></span> <span data-ttu-id="15167-138">不包含元件的輸出。</span><span class="sxs-lookup"><span data-stu-id="15167-138">Output from the component isn't included.</span></span> <span data-ttu-id="15167-139">當使用者代理程式啟動時，會使用此標記來啟動 Blazor 應用程式。</span><span class="sxs-lookup"><span data-stu-id="15167-139">When the user-agent starts, this marker is used to bootstrap a Blazor app.</span></span> |
| <xref:Microsoft.AspNetCore.Mvc.Rendering.RenderMode.Static> | <span data-ttu-id="15167-140">將元件轉譯為靜態 HTML。</span><span class="sxs-lookup"><span data-stu-id="15167-140">Renders the component into static HTML.</span></span> |

<span data-ttu-id="15167-141">雖然頁面和觀點可以使用元件，但反向並不成立。</span><span class="sxs-lookup"><span data-stu-id="15167-141">While pages and views can use components, the converse isn't true.</span></span> <span data-ttu-id="15167-142">元件無法使用 view 和 page 特有的功能，例如部分視圖和區段。</span><span class="sxs-lookup"><span data-stu-id="15167-142">Components can't use view- and page-specific features, such as partial views and sections.</span></span> <span data-ttu-id="15167-143">若要在元件中使用部分視圖的邏輯，請將部分視圖邏輯視為元件。</span><span class="sxs-lookup"><span data-stu-id="15167-143">To use logic from a partial view in a component, factor out the partial view logic into a component.</span></span>

<span data-ttu-id="15167-144">不支援從靜態 HTML 網頁轉譯伺服器元件。</span><span class="sxs-lookup"><span data-stu-id="15167-144">Rendering server components from a static HTML page isn't supported.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15167-145">其他資源</span><span class="sxs-lookup"><span data-stu-id="15167-145">Additional resources</span></span>

* <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ComponentTagHelper>
* <xref:mvc/views/tag-helpers/intro>
* <xref:blazor/components/index>
