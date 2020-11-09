---
title: ASP.NET Core 中的標籤協助程式
author: rick-anderson
description: 了解何謂標籤協助程式，以及如何在 ASP.NET Core 中使用。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 03/18/2019
no-loc:
- 'appsettings.json'
- 'ASP.NET Core Identity'
- 'cookie'
- 'Cookie'
- 'Blazor'
- 'Blazor Server'
- 'Blazor WebAssembly'
- 'Identity'
- "Let's Encrypt"
- 'Razor'
- 'SignalR'
uid: mvc/views/tag-helpers/intro
ms.openlocfilehash: 781365d99c6d36d8abaec9681128ba712db8cb88
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93060660"
---
# <a name="tag-helpers-in-aspnet-core"></a><span data-ttu-id="d4db2-103">ASP.NET Core 中的標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d4db2-103">Tag Helpers in ASP.NET Core</span></span>

<span data-ttu-id="d4db2-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d4db2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="d4db2-105">什麼是標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d4db2-105">What are Tag Helpers</span></span>

<span data-ttu-id="d4db2-106">標記協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d4db2-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="d4db2-107">例如，內建 `ImageTagHelper` 可以將版本號碼附加至映像名稱。</span><span class="sxs-lookup"><span data-stu-id="d4db2-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="d4db2-108">只要映像變更，伺服器就會產生映像的新唯一版本，以保證用戶端可以取得最新的映像 (而不是過期的快取映像)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="d4db2-109">有許多適用於一般工作 (例如建立表單和連結、載入資產等) 的內建標籤協助程式，還有更多位於公用 GitHub 存放庫及作為 NuGet 套件來提供。</span><span class="sxs-lookup"><span data-stu-id="d4db2-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="d4db2-110">標籤協助程式是以 C# 編寫，並根據項目名稱、屬性名稱或上層標籤來設定目標 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="d4db2-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="d4db2-111">例如，套用 `LabelTagHelper` 屬性時，內建 `LabelTagHelper` 可以將目標設為 HTML `<label>` 項目。</span><span class="sxs-lookup"><span data-stu-id="d4db2-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="d4db2-112">如果您熟悉 [html](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)協助程式，標籤協助程式可減少在 VIEWS 中 Html 和 c # 之間的明確轉換 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d4db2-112">If you're familiar with [HTML Helpers](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="d4db2-113">在許多情況下，HTML 協助程式都會提供特定標籤協助程式的替代方式，但請務必辨識標籤協助程式未取代 HTML 協助程式，而且每個 HTML 協助程式都沒有標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="d4db2-114">[標籤協助程式與 HTML 協助程式的比較](#tag-helpers-compared-to-html-helpers)會詳述差異。</span><span class="sxs-lookup"><span data-stu-id="d4db2-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="d4db2-115">標籤協助程式所提供的內容</span><span class="sxs-lookup"><span data-stu-id="d4db2-115">What Tag Helpers provide</span></span>

<span data-ttu-id="d4db2-116">易於 **使用 HTML 的開發體驗** 大部分的情況下，使用標籤協助程式的 Razor 標記看起來就像標準 HTML。</span><span class="sxs-lookup"><span data-stu-id="d4db2-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="d4db2-117">使用 HTML/CSS/JavaScript 所熟悉的前端設計工具可以進行編輯， Razor 而不需要學習 c # Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="d4db2-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="d4db2-118">用來 **建立 html 和 Razor 標記的豐富 IntelliSense 環境** ，與 html 協助程式很清晰，後者是伺服器端在視圖中建立標記的方式 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d4db2-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="d4db2-119">[標籤協助程式與 HTML 協助程式的比較](#tag-helpers-compared-to-html-helpers)會詳述差異。</span><span class="sxs-lookup"><span data-stu-id="d4db2-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="d4db2-120">[標籤協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)說明 IntelliSense 環境。</span><span class="sxs-lookup"><span data-stu-id="d4db2-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="d4db2-121">即使 Razor 是使用 c # 語法經驗的開發人員，使用標記協助程式比撰寫 c # 標記更具生產力 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d4db2-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="d4db2-122">**讓您更具生產力，而且可以只使用伺服器上的可用資訊來產生更強固、可靠和易維護的程式碼** ：例如，在過去，更新映像的目的是要在您變更映像時變更映像名稱。</span><span class="sxs-lookup"><span data-stu-id="d4db2-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="d4db2-123">基於效能考量，應該主動快取影像，而且除非您變更影像的名稱，否則用戶端會有取得過時複本的風險。</span><span class="sxs-lookup"><span data-stu-id="d4db2-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="d4db2-124">在過去，編輯映像之後，必須變更名稱，而且需要更新 Web 應用程式中映像的每個參考。</span><span class="sxs-lookup"><span data-stu-id="d4db2-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="d4db2-125">這並不只是非常耗費人力，也很容易出錯 (您可能遺漏參考、不小心輸入錯誤的字串等等。 ) 內建 `ImageTagHelper` 可以自動為您進行這項作業。</span><span class="sxs-lookup"><span data-stu-id="d4db2-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="d4db2-126">`ImageTagHelper` 可以將版本號碼附加到映像名稱後面；因此，只要映像變更，伺服器就會自動產生映像的新唯一版本。</span><span class="sxs-lookup"><span data-stu-id="d4db2-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="d4db2-127">用戶端保證會取得目前的映像。</span><span class="sxs-lookup"><span data-stu-id="d4db2-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="d4db2-128">使用 `ImageTagHelper`，此健全性和人力節省基本上是免費的。</span><span class="sxs-lookup"><span data-stu-id="d4db2-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="d4db2-129">大部分的內建的標籤協助程式都可以在標準的 HTML 元素中使用，可為元素提供伺服器端的屬性。</span><span class="sxs-lookup"><span data-stu-id="d4db2-129">Most built-in Tag Helpers target standard HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="d4db2-130">例如，在 [檢視/帳戶]  資料夾內許多檢視中所使用的 `<input>` 元素會包含 `asp-for` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d4db2-130">For example, the `<input>` element used in many views in the *Views/Account* folder contains the `asp-for` attribute.</span></span> <span data-ttu-id="d4db2-131">此屬性會擷取指定之模型屬性的名稱，並將其放入轉譯的 HTML 中。</span><span class="sxs-lookup"><span data-stu-id="d4db2-131">This attribute extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="d4db2-132">請考慮 Razor 具有下列模型的視圖：</span><span class="sxs-lookup"><span data-stu-id="d4db2-132">Consider a Razor view with the following model:</span></span>

```csharp
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}
```

<span data-ttu-id="d4db2-133">下列 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="d4db2-133">The following Razor markup:</span></span>

```cshtml
<label asp-for="Movie.Title"></label>
```

<span data-ttu-id="d4db2-134">產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="d4db2-134">Generates the following HTML:</span></span>

```html
<label for="Movie_Title">Title</label>
```

<span data-ttu-id="d4db2-135">`asp-for` 屬性由 [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0) 中的 `For` 提供。</span><span class="sxs-lookup"><span data-stu-id="d4db2-135">The `asp-for` attribute is made available by the `For` property in the [LabelTagHelper](/dotnet/api/microsoft.aspnetcore.mvc.taghelpers.labeltaghelper?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d4db2-136">如需詳細資訊，請參閱[編寫標籤協助程式](xref:mvc/views/tag-helpers/authoring)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-136">See [Author Tag Helpers](xref:mvc/views/tag-helpers/authoring) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="d4db2-137">管理標籤協助程式範圍</span><span class="sxs-lookup"><span data-stu-id="d4db2-137">Managing Tag Helper scope</span></span>

<span data-ttu-id="d4db2-138">標籤協助程式範圍是透過 `@addTagHelper`、`@removeTagHelper` 和 "!" 退出字元的組合所控制。</span><span class="sxs-lookup"><span data-stu-id="d4db2-138">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="d4db2-139">`@addTagHelper` 讓標籤協助程式可用</span><span class="sxs-lookup"><span data-stu-id="d4db2-139">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="d4db2-140">如果您建立名為 *AuthoringTagHelpers* 的新 ASP.NET Core Web 應用程式，則會將下列 *Views/_ViewImports.cshtml* 檔案新增至您的專案：</span><span class="sxs-lookup"><span data-stu-id="d4db2-140">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* , the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="d4db2-141">`@addTagHelper` 指示詞讓標籤協助程式可供檢視使用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-141">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="d4db2-142">在此情況下，view 檔案是 *pages/_ViewImports. cshtml* ，預設會由 *pages* 資料夾和子資料夾中的所有檔案繼承;讓標籤協助程式可供使用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-142">In this case, the view file is *Pages/_ViewImports.cshtml* , which by default is inherited by all files in the *Pages* folder and subfolders; making Tag Helpers available.</span></span> <span data-ttu-id="d4db2-143">上述程式碼使用萬用字元語法 ( " \* " ) 來指定指定元件中的所有標籤協助程式 ( *>microsoft.aspnetcore.mvc.taghelpers* ) 將可供 *Views* 目錄或子目錄中的每個 view 檔案使用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-143">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly ( *Microsoft.AspNetCore.Mvc.TagHelpers* ) will be available to every view file in the *Views* directory or subdirectory.</span></span> <span data-ttu-id="d4db2-144">`@addTagHelper` 後面的第一個參數指定要載入的標籤協助程式 (使用 "\*" 表示所有標籤協助程式)，而第二個參數 "Microsoft.AspNetCore.Mvc.TagHelpers" 指定包含標籤協助程式的組件。</span><span class="sxs-lookup"><span data-stu-id="d4db2-144">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="d4db2-145">*Microsoft.AspNetCore.Mvc.TagHelpers* 是內建 ASP.NET Core 標籤協助程式的組件。</span><span class="sxs-lookup"><span data-stu-id="d4db2-145">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="d4db2-146">若要公開此專案中的所有標籤協助程式 (這會建立名為 *AuthoringTagHelpers* 的組件)，請使用下列內容：</span><span class="sxs-lookup"><span data-stu-id="d4db2-146">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers* ), you would use the following:</span></span>

[!code-cshtml[](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="d4db2-147">如果您的專案包含具有預設命名空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 的 `EmailTagHelper`，則您可以提供標籤協助程式的完整名稱 (FQN)：</span><span class="sxs-lookup"><span data-stu-id="d4db2-147">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="d4db2-148">若要使用 FQN 將標籤協助程式新增至檢視，請依序新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`) 和組件名稱 ( *AuthoringTagHelpers* )。</span><span class="sxs-lookup"><span data-stu-id="d4db2-148">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name ( *AuthoringTagHelpers* ).</span></span> <span data-ttu-id="d4db2-149">大部分開發人員都會想要使用 "\*" 萬用字元語法。</span><span class="sxs-lookup"><span data-stu-id="d4db2-149">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="d4db2-150">萬用字元語法可讓您在 FQN 中插入萬用字元 "\*" 作為尾碼。</span><span class="sxs-lookup"><span data-stu-id="d4db2-150">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="d4db2-151">例如，下列任何指示詞都會顯示 `EmailTagHelper`：</span><span class="sxs-lookup"><span data-stu-id="d4db2-151">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="d4db2-152">如先前所述，將指示詞新增 `@addTagHelper` 至 *views/_ViewImports cshtml* 檔案，讓標籤協助程式可供 *views* 目錄和子目錄中的所有 view 檔案使用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-152">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and subdirectories.</span></span> <span data-ttu-id="d4db2-153">如果您想要選擇只向那些檢視公開標籤協助程式，則可以使用特定檢視檔案中的 `@addTagHelper` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="d4db2-153">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="d4db2-154">`@removeTagHelper` 會移除標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d4db2-154">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="d4db2-155">`@removeTagHelper` 有兩個參數與 `@addTagHelper` 相同，而且會移除先前新增的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-155">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="d4db2-156">例如，套用至特定檢視的 `@removeTagHelper` 會從檢視中移除指定的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-156">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="d4db2-157">在 *Views/Folder/_ViewImports.cshtml* 檔案中使用 `@removeTagHelper`，會從 *Folder* 的所有檢視中移除所指定的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-157">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder* .</span></span>

### <a name="controlling-tag-helper-scope-with-the-_viewimportscshtml-file"></a><span data-ttu-id="d4db2-158">使用 *_ViewImports.cshtml* 檔案控制標籤協助程式範圍</span><span class="sxs-lookup"><span data-stu-id="d4db2-158">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="d4db2-159">您可以將 *_ViewImports.cshtml* 新增至任何檢視資料夾，而且檢視引擎會套用該檔案和 *Views/_ViewImports.cshtml* 檔案中的指示詞。</span><span class="sxs-lookup"><span data-stu-id="d4db2-159">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="d4db2-160">如果您已為 *Home* 檢視新增空白 *Views/Home/_ViewImports.cshtml* 檔案，則不會進行變更；因為 *_ViewImports.cshtml* 檔案是加總的。</span><span class="sxs-lookup"><span data-stu-id="d4db2-160">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="d4db2-161">您新增至 *Views/Home/_ViewImports.cshtml* 檔案的任何 `@addTagHelper` 指示詞 (不在預設 *Views/_ViewImports.cshtml* 檔案中)，只會向 *Home* 資料夾中的檢視公開這些標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-161">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="d4db2-162">退出個別項目</span><span class="sxs-lookup"><span data-stu-id="d4db2-162">Opting out of individual elements</span></span>

<span data-ttu-id="d4db2-163">您可以停用項目層級中包含標籤協助程式退出字元 ("!") 的標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-163">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="d4db2-164">例如，在包含標籤協助程式退出字元的 `<span>` 中，停用 `Email` 驗證：</span><span class="sxs-lookup"><span data-stu-id="d4db2-164">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="d4db2-165">您必須將標籤協助程式退出字元套用至開頭和結尾標籤 </span><span class="sxs-lookup"><span data-stu-id="d4db2-165">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="d4db2-166">(將退出字元新增至開頭標籤時，Visual Studio 編輯器會將退出字元自動新增至結尾標籤)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-166">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="d4db2-167">在您新增退出字元之後，就不會再以特殊字型顯示項目和標籤協助程式屬性。</span><span class="sxs-lookup"><span data-stu-id="d4db2-167">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="d4db2-168">使用 `@tagHelperPrefix` 以明確使用標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d4db2-168">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="d4db2-169">`@tagHelperPrefix` 指示詞可讓您指定標籤前置詞字串以啟用標籤協助程式支援，並明確使用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-169">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="d4db2-170">例如，您可以將下列標記新增至 *Views/_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="d4db2-170">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```

<span data-ttu-id="d4db2-171">在下列程式碼影像中，標籤協助程式前置詞設定為 `th:`，因此只有使用前置詞 `th:` 的項目才支援標籤協助程式 (啟用標籤協助程式的項目具有特殊字型)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-171">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="d4db2-172">`<label>` 和 `<input>` 項目具有標籤協助程式前置詞並且已啟用標籤協助程式，而 `<span>` 項目則否。</span><span class="sxs-lookup"><span data-stu-id="d4db2-172">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![image](intro/_static/thp.png)

<span data-ttu-id="d4db2-174">套用至 `@addTagHelper` 的相同階層規則也會套用至 `@tagHelperPrefix`。</span><span class="sxs-lookup"><span data-stu-id="d4db2-174">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="self-closing-tag-helpers"></a><span data-ttu-id="d4db2-175">自行結尾的標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d4db2-175">Self-closing Tag Helpers</span></span>

<span data-ttu-id="d4db2-176">許多標籤協助程式無法作為自行結尾的標籤。</span><span class="sxs-lookup"><span data-stu-id="d4db2-176">Many Tag Helpers can't be used as self-closing tags.</span></span> <span data-ttu-id="d4db2-177">某些標籤協助程式專門用作自行結尾的標籤。</span><span class="sxs-lookup"><span data-stu-id="d4db2-177">Some Tag Helpers are designed to be self-closing tags.</span></span> <span data-ttu-id="d4db2-178">使用非專門用作自行結尾的標籤協助程式會隱藏轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="d4db2-178">Using a Tag Helper that was not designed to be self-closing suppresses the rendered output.</span></span> <span data-ttu-id="d4db2-179">讓標籤協助程式自行結尾，會在轉譯輸出中產生自行結尾的標籤。</span><span class="sxs-lookup"><span data-stu-id="d4db2-179">Self-closing a Tag Helper results in a self-closing tag in the rendered output.</span></span> <span data-ttu-id="d4db2-180">如需詳細資訊，請參閱[編寫標籤協助程式](xref:mvc/views/tag-helpers/authoring)中的[這項附註](xref:mvc/views/tag-helpers/authoring#self-closing)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-180">For more information, see [this note](xref:mvc/views/tag-helpers/authoring#self-closing) in [Authoring Tag Helpers](xref:mvc/views/tag-helpers/authoring).</span></span>

## <a name="c-in-tag-helpers-attributedeclaration"></a><span data-ttu-id="d4db2-181">標記協助程式屬性/宣告中的 c #</span><span class="sxs-lookup"><span data-stu-id="d4db2-181">C# in Tag Helpers attribute/declaration</span></span> 

<span data-ttu-id="d4db2-182">標記協助程式不允許在專案的屬性或標記宣告區域中採用 c #。</span><span class="sxs-lookup"><span data-stu-id="d4db2-182">Tag Helpers do not allow C# in the element's attribute or tag declaration area.</span></span> <span data-ttu-id="d4db2-183">例如，下列程式碼無效：</span><span class="sxs-lookup"><span data-stu-id="d4db2-183">For example, the following code is not valid:</span></span>

```cshtml
<input asp-for="LastName"  
       @(Model?.LicenseId == null ? "disabled" : string.Empty) />
```

<span data-ttu-id="d4db2-184">上述程式碼可以撰寫為：</span><span class="sxs-lookup"><span data-stu-id="d4db2-184">The preceding code can be written as:</span></span>

```cshtml
<input asp-for="LastName" 
       disabled="@(Model?.LicenseId == null)" />
```

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="d4db2-185">標籤協助程式的 IntelliSense 支援</span><span class="sxs-lookup"><span data-stu-id="d4db2-185">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="d4db2-186">當您在 Visual Studio 中建立新的 ASP.NET Core web 應用程式時，它會新增 NuGet 套件 "AspNetCore Razor "。工具」。</span><span class="sxs-lookup"><span data-stu-id="d4db2-186">When you create a new ASP.NET Core web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="d4db2-187">這是新增標籤協助程式工具的套件。</span><span class="sxs-lookup"><span data-stu-id="d4db2-187">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="d4db2-188">請考慮撰寫 HTML `<label>` 項目。</span><span class="sxs-lookup"><span data-stu-id="d4db2-188">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="d4db2-189">只要您在 Visual Studio 編輯器中輸入 `<l`，IntelliSense 就會顯示相符的項目：</span><span class="sxs-lookup"><span data-stu-id="d4db2-189">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![image](intro/_static/label.png)

<span data-ttu-id="d4db2-191">您不只可以取得 HTML 協助，還可以取得圖示 (其下的 "@" symbol with "<>")。</span><span class="sxs-lookup"><span data-stu-id="d4db2-191">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![image](intro/_static/tagSym.png)

<span data-ttu-id="d4db2-193">識別標籤協助程式設為目標的項目。</span><span class="sxs-lookup"><span data-stu-id="d4db2-193">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="d4db2-194">純 HTML 項目 (例如 `fieldset`) 會顯示 "<>" 圖示。</span><span class="sxs-lookup"><span data-stu-id="d4db2-194">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="d4db2-195">純 HTML `<label>` 標籤會以棕色字型顯示 HTML 標籤 (具有預設 Visual Studio 色彩佈景主題)、以紅色顯示屬性，並以藍色顯示屬性值。</span><span class="sxs-lookup"><span data-stu-id="d4db2-195">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![image](intro/_static/LableHtmlTag.png)

<span data-ttu-id="d4db2-197">在您輸入 `<label` 之後，IntelliSense 會列出可用的 HTML/CSS 屬性以及設為目標的標籤協助程式屬性：</span><span class="sxs-lookup"><span data-stu-id="d4db2-197">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![image](intro/_static/labelattr.png)

<span data-ttu-id="d4db2-199">IntelliSense 陳述式完成可讓您輸入 Tab 鍵，來完成具有所選取值的陳述式：</span><span class="sxs-lookup"><span data-stu-id="d4db2-199">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![image](intro/_static/stmtcomplete.png)

<span data-ttu-id="d4db2-201">只要輸入標籤協助程式屬性，標籤和屬性字型就會變更。</span><span class="sxs-lookup"><span data-stu-id="d4db2-201">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="d4db2-202">使用預設 Visual Studio "Blue" 或 "Light" 色彩佈景主題，而且字型為粗體紫色。</span><span class="sxs-lookup"><span data-stu-id="d4db2-202">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="d4db2-203">如果要使用 "Dark" 佈景主題，則字型是粗體藍綠色。</span><span class="sxs-lookup"><span data-stu-id="d4db2-203">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="d4db2-204">本文件中的影像是使用預設佈景主題所取得。</span><span class="sxs-lookup"><span data-stu-id="d4db2-204">The images in this document were taken using the default theme.</span></span>

![image](intro/_static/labelaspfor2.png)

<span data-ttu-id="d4db2-206">您可以在雙引號 ("") 內輸入 Visual Studio *CompleteWord* 快速鍵 (Ctrl+空格鍵是 [預設值](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)，而且現在可以使用 C#，就像使用 C# 類別一樣。</span><span class="sxs-lookup"><span data-stu-id="d4db2-206">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="d4db2-207">IntelliSense 會顯示頁面模型上的所有方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="d4db2-207">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="d4db2-208">因為屬性類型是 `ModelExpression`，所以可以使用方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="d4db2-208">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="d4db2-209">在下列影像中，我正在編輯 `Register` 檢視，因此 `RegisterViewModel` 可供使用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-209">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![image](intro/_static/intellemail.png)

<span data-ttu-id="d4db2-211">IntelliSense 會列出頁面上模型可用的屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="d4db2-211">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="d4db2-212">豐富的 IntelliSense 環境可協助您選取 CSS 類別：</span><span class="sxs-lookup"><span data-stu-id="d4db2-212">The rich IntelliSense environment helps you select the CSS class:</span></span>

![image](intro/_static/iclass.png)

![image](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="d4db2-215">標籤協助程式與 HTML 協助程式的比較</span><span class="sxs-lookup"><span data-stu-id="d4db2-215">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="d4db2-216">標籤協助程式會附加至瀏覽器中的 HTML 元素 Razor ，而 [html](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) 協助程式會叫用為與流覽中的 html 一起使用的方法 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d4db2-216">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](https://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="d4db2-217">請考慮下列 Razor 標記，這會建立具有 CSS 類別 "caption" 的 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="d4db2-217">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="d4db2-218">在 (的 `@`) 符號 Razor 會告訴這是程式碼的開頭。</span><span class="sxs-lookup"><span data-stu-id="d4db2-218">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="d4db2-219">下兩個參數 ("FirstName" 和 "First Name:") 是字串，因此 [IntelliSense](/visualstudio/ide/using-intellisense) 沒有幫助。</span><span class="sxs-lookup"><span data-stu-id="d4db2-219">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="d4db2-220">最後一個引數：</span><span class="sxs-lookup"><span data-stu-id="d4db2-220">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="d4db2-221">是用來代表屬性的匿名物件。</span><span class="sxs-lookup"><span data-stu-id="d4db2-221">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="d4db2-222">由於 `class` 是 C# 中的保留關鍵字，因此您可以使用 `@` 符號來強制 C# 將 `@class=` 解譯為符號 (屬性名稱)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-222">Because `class` is a reserved keyword in C#, you use the `@` symbol to force C# to interpret `@class=` as a symbol (property name).</span></span> <span data-ttu-id="d4db2-223">在前端設計工具 (熟悉 HTML/CSS/JavaScript 和其他用戶端技術，但不熟悉 c # 和) 的人 Razor ，大部分的行都是外部的。</span><span class="sxs-lookup"><span data-stu-id="d4db2-223">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="d4db2-224">您必須編寫整行，而 IntelliSense 沒有任何幫助。</span><span class="sxs-lookup"><span data-stu-id="d4db2-224">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="d4db2-225">使用 `LabelTagHelper`，可以將相同的標記撰寫為：</span><span class="sxs-lookup"><span data-stu-id="d4db2-225">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

```cshtml
<label class="caption" asp-for="FirstName"></label>
```

<span data-ttu-id="d4db2-226">使用標籤協助程式版本時，只要您在 Visual Studio 編輯器中輸入 `<l`，IntelliSense 就會顯示相符的項目：</span><span class="sxs-lookup"><span data-stu-id="d4db2-226">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![image](intro/_static/label.png)

<span data-ttu-id="d4db2-228">IntelliSense 可協助您撰寫整行。</span><span class="sxs-lookup"><span data-stu-id="d4db2-228">IntelliSense helps you write the entire line.</span></span>

<span data-ttu-id="d4db2-229">下列程式碼影像顯示 *Views/Account/Register. cshtml* 視圖的表單部分， Razor 從 Visual Studio 隨附的 ASP.NET 4.5. x MVC 範本產生。</span><span class="sxs-lookup"><span data-stu-id="d4db2-229">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the ASP.NET 4.5.x MVC template included with Visual Studio.</span></span>

![image](intro/_static/regCS.png)

<span data-ttu-id="d4db2-231">Visual Studio 編輯器會以灰色背景顯示 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="d4db2-231">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="d4db2-232">例如，`AntiForgeryToken` HTML 協助程式：</span><span class="sxs-lookup"><span data-stu-id="d4db2-232">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="d4db2-233">以灰色背景顯示。</span><span class="sxs-lookup"><span data-stu-id="d4db2-233">is displayed with a grey background.</span></span> <span data-ttu-id="d4db2-234">註冊檢視中的大部分標記都是 C#。</span><span class="sxs-lookup"><span data-stu-id="d4db2-234">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="d4db2-235">將該方法與使用標籤協助程式的對等方法進行比較：</span><span class="sxs-lookup"><span data-stu-id="d4db2-235">Compare that to the equivalent approach using Tag Helpers:</span></span>

![image](intro/_static/regTH.png)

<span data-ttu-id="d4db2-237">與 HTML 協助程式方法相較之下，標記更為簡潔，而且更容易讀取、編輯和維護。</span><span class="sxs-lookup"><span data-stu-id="d4db2-237">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="d4db2-238">C# 程式碼會減少到伺服器所知道的最小值。</span><span class="sxs-lookup"><span data-stu-id="d4db2-238">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="d4db2-239">Visual Studio 編輯器會以特殊字型顯示標籤 (tag) 協助程式設為目標的標籤 (markup)。</span><span class="sxs-lookup"><span data-stu-id="d4db2-239">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="d4db2-240">請考慮使用 *Email* 群組：</span><span class="sxs-lookup"><span data-stu-id="d4db2-240">Consider the *Email* group:</span></span>

[!code-cshtml[](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="d4db2-241">每個 "asp-" 屬性的值都是 "Email"，但 "Email" 不是字串。</span><span class="sxs-lookup"><span data-stu-id="d4db2-241">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="d4db2-242">在此內容中，"Email" 是 `RegisterViewModel` 的 C# 模型運算式屬性。</span><span class="sxs-lookup"><span data-stu-id="d4db2-242">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="d4db2-243">Visual Studio 編輯器可協助您撰寫註冊表單之標籤 (tag) 協助程式方法中的 **所有** 標籤 (markup)，而 Visual Studio 不提供 HTML 協助程式方法中大部分程式碼的協助。</span><span class="sxs-lookup"><span data-stu-id="d4db2-243">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="d4db2-244">[標籤協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)會詳述如何在 Visual Studio 編輯器中使用標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4db2-244">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="d4db2-245">標籤協助程式與網頁伺服器控制項的比較</span><span class="sxs-lookup"><span data-stu-id="d4db2-245">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="d4db2-246">標籤協助程式未擁有與其建立關聯的項目；它們只會參與轉譯項目和內容。</span><span class="sxs-lookup"><span data-stu-id="d4db2-246">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="d4db2-247">ASP.NET <https://docs.microsoft.com/previous-versions/dotnet/netframework-3.0/7698y1f0(v=vs.85)> 是在頁面上宣告和叫用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-247">ASP.NET <https://docs.microsoft.com/previous-versions/dotnet/netframework-3.0/7698y1f0(v=vs.85)> are declared and invoked on a page.</span></span>

* <span data-ttu-id="d4db2-248"><https://docs.microsoft.com/previous-versions/zsyt68f1(v=vs.140)> 具有非一般的生命週期，可讓開發和錯錯變得困難。</span><span class="sxs-lookup"><span data-stu-id="d4db2-248"><https://docs.microsoft.com/previous-versions/zsyt68f1(v=vs.140)> have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="d4db2-249">網頁伺服器控制項可讓您使用用戶端控制項，來新增用戶端文件物件模型 (DOM) 項目的功能。</span><span class="sxs-lookup"><span data-stu-id="d4db2-249">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="d4db2-250">標籤協助程式沒有 DOM。</span><span class="sxs-lookup"><span data-stu-id="d4db2-250">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="d4db2-251">Web 伺服器控制項包括自動瀏覽器偵測。</span><span class="sxs-lookup"><span data-stu-id="d4db2-251">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="d4db2-252">標籤協助程式不知道瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="d4db2-252">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="d4db2-253">雖然您通常無法撰寫網頁伺服器控制項，但是多個標籤協助程式可以處理相同的項目 (請參閱[避免標籤協助程式衝突](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts))。</span><span class="sxs-lookup"><span data-stu-id="d4db2-253">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](xref:mvc/views/tag-helpers/authoring#avoid-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="d4db2-254">標籤協助程式可以修改設為其範圍之 HTML 項目的標籤和內容，但不會直接修改頁面上的其他任何項目。</span><span class="sxs-lookup"><span data-stu-id="d4db2-254">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="d4db2-255">網頁伺服器控制項的範圍更為廣泛，而且可以執行的動作會影響您網頁的其他部分；具有非預期的副作用。</span><span class="sxs-lookup"><span data-stu-id="d4db2-255">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="d4db2-256">網頁伺服器控制項使用類型轉換器以將字串轉換成物件。</span><span class="sxs-lookup"><span data-stu-id="d4db2-256">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="d4db2-257">使用標籤協助程式時，您可以使用 C# 以原生方式工作，因此不需要進行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="d4db2-257">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="d4db2-258">網頁伺服器控制項使用 [System.ComponentModel](/dotnet/api/system.componentmodel)，來實作元件和控制項的執行階段和設計階段行為。</span><span class="sxs-lookup"><span data-stu-id="d4db2-258">Web Server controls use [System.ComponentModel](/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="d4db2-259">`System.ComponentModel` 包含基底類別和介面，以便實作屬性和類型轉換器、繫結至資料來源，以及授權元件。</span><span class="sxs-lookup"><span data-stu-id="d4db2-259">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="d4db2-260">與一般衍生自 `TagHelper` 的標籤協助程式相反，而且 `TagHelper` 基底類別只會公開兩個方法：`Process` 和 `ProcessAsync`。</span><span class="sxs-lookup"><span data-stu-id="d4db2-260">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="d4db2-261">自訂標籤協助程式項目字型</span><span class="sxs-lookup"><span data-stu-id="d4db2-261">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="d4db2-262">您可以從 [ **工具**  >  **選項** ]  >  **環境** 字型  >  **和色彩** 自訂字型和顏色標示：</span><span class="sxs-lookup"><span data-stu-id="d4db2-262">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors** :</span></span>

![image](intro/_static/fontoptions2.png)

[!INCLUDE[](~/includes/built-in-TH.md)]

## <a name="additional-resources"></a><span data-ttu-id="d4db2-264">其他資源</span><span class="sxs-lookup"><span data-stu-id="d4db2-264">Additional resources</span></span>

* [<span data-ttu-id="d4db2-265">編寫標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="d4db2-265">Author Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)
* [<span data-ttu-id="d4db2-266">使用表單</span><span class="sxs-lookup"><span data-stu-id="d4db2-266">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* <span data-ttu-id="d4db2-267">[GitHub 上的 TagHelperSamples](https://github.com/dpaquette/TagHelperSamples) 包含使用[啟動程序](https://getbootstrap.com/) 的標籤協助程式範例。</span><span class="sxs-lookup"><span data-stu-id="d4db2-267">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](https://getbootstrap.com/).</span></span>
