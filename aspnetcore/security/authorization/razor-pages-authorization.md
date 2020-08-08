---
title: RazorASP.NET Core 中的頁面授權慣例
author: rick-anderson
description: 瞭解如何以授權使用者的慣例控制頁面的存取權，並允許匿名使用者存取頁面的頁面或資料夾。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
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
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 5e0c76ea6c6b4af8e24693b1bfe8ac28bf2ce3a9
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88022273"
---
# <a name="no-locrazor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="ef70f-103">RazorASP.NET Core 中的頁面授權慣例</span><span class="sxs-lookup"><span data-stu-id="ef70f-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ef70f-104">控制頁面應用程式存取權的其中一種方式 Razor 是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="ef70f-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="ef70f-105">這些慣例可讓您授權使用者，並允許匿名使用者存取頁面的個別頁面或資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef70f-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="ef70f-106">本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="ef70f-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="ef70f-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="ef70f-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ef70f-108">範例應用程式[ cookie 會使用驗證， Identity 而不 ASP.NET Core ](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="ef70f-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="ef70f-109">本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 的應用程式 Identity 。</span><span class="sxs-lookup"><span data-stu-id="ef70f-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="ef70f-110">若要使用 ASP.NET Core Identity ，請遵循中的指導方針 <xref:security/authentication/identity> 。</span><span class="sxs-lookup"><span data-stu-id="ef70f-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="ef70f-111">需要授權才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="ef70f-111">Require authorization to access a page</span></span>

<span data-ttu-id="ef70f-112">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面的指定路徑：</span><span class="sxs-lookup"><span data-stu-id="ef70f-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="ef70f-113">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="ef70f-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="ef70f-114">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="ef70f-115"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至具有 filter 屬性的頁面模型類別 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="ef70f-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="ef70f-116">如需詳細資訊，請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="ef70f-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="ef70f-117">需要授權才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="ef70f-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="ef70f-118">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=4)]

<span data-ttu-id="ef70f-119">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ef70f-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="ef70f-120">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="ef70f-121">需要授權才能存取區域頁面</span><span class="sxs-lookup"><span data-stu-id="ef70f-121">Require authorization to access an area page</span></span>

<span data-ttu-id="ef70f-122">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至位於指定路徑的 [區域] 頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="ef70f-123">[頁面名稱] 是檔案的路徑，沒有副檔名相對於指定區域的頁面根目錄。</span><span class="sxs-lookup"><span data-stu-id="ef70f-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef70f-124">例如，檔案*區域/ Identity /Pages/Manage/Accounts.cshtml*的頁面名稱是 */Manage/Accounts*。</span><span class="sxs-lookup"><span data-stu-id="ef70f-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="ef70f-125">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="ef70f-126">需要授權才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="ef70f-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="ef70f-127">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有區域：</span><span class="sxs-lookup"><span data-stu-id="ef70f-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="ef70f-128">資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="ef70f-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef70f-129">例如，*區域/ Identity /Pages/Manage/* 下檔案的資料夾路徑為 */Manage*。</span><span class="sxs-lookup"><span data-stu-id="ef70f-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="ef70f-130">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="ef70f-131">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="ef70f-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="ef70f-132">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="ef70f-133">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="ef70f-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="ef70f-134">允許匿名存取頁面資料夾</span><span class="sxs-lookup"><span data-stu-id="ef70f-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="ef70f-135">使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 慣例，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="ef70f-136">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ef70f-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="ef70f-137">結合已授權和匿名存取的注意事項</span><span class="sxs-lookup"><span data-stu-id="ef70f-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="ef70f-138">指定頁面的資料夾需要授權，然後指定該資料夾內的頁面允許匿名存取，是有效的：</span><span class="sxs-lookup"><span data-stu-id="ef70f-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="ef70f-139">不過，相反的，這是不正確。</span><span class="sxs-lookup"><span data-stu-id="ef70f-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="ef70f-140">您無法針對匿名存取宣告頁面的資料夾，然後在該資料夾內指定需要授權的頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="ef70f-141">在私用頁面上要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="ef70f-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="ef70f-142">當 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 和都套用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面時，會優先使用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 並控制存取。</span><span class="sxs-lookup"><span data-stu-id="ef70f-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef70f-143">其他資源</span><span class="sxs-lookup"><span data-stu-id="ef70f-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="ef70f-144">控制頁面應用程式存取權的其中一種方式 Razor 是在啟動時使用授權慣例。</span><span class="sxs-lookup"><span data-stu-id="ef70f-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="ef70f-145">這些慣例可讓您授權使用者，並允許匿名使用者存取頁面的個別頁面或資料夾。</span><span class="sxs-lookup"><span data-stu-id="ef70f-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="ef70f-146">本主題中所述的慣例會自動套用[授權篩選](xref:mvc/controllers/filters#authorization-filters)來控制存取。</span><span class="sxs-lookup"><span data-stu-id="ef70f-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="ef70f-147">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="ef70f-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ef70f-148">範例應用程式[ cookie 會使用驗證， Identity 而不 ASP.NET Core ](xref:security/authentication/cookie)。</span><span class="sxs-lookup"><span data-stu-id="ef70f-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="ef70f-149">本主題所顯示的概念和範例同樣適用于使用 ASP.NET Core 的應用程式 Identity 。</span><span class="sxs-lookup"><span data-stu-id="ef70f-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="ef70f-150">若要使用 ASP.NET Core Identity ，請遵循中的指導方針 <xref:security/authentication/identity> 。</span><span class="sxs-lookup"><span data-stu-id="ef70f-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="ef70f-151">需要授權才能存取頁面</span><span class="sxs-lookup"><span data-stu-id="ef70f-151">Require authorization to access a page</span></span>

<span data-ttu-id="ef70f-152">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面的指定路徑：</span><span class="sxs-lookup"><span data-stu-id="ef70f-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="ef70f-153">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="ef70f-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="ef70f-154">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizePage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="ef70f-155"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>可以套用至具有 filter 屬性的頁面模型類別 `[Authorize]` 。</span><span class="sxs-lookup"><span data-stu-id="ef70f-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="ef70f-156">如需詳細資訊，請參閱[授權篩選屬性](xref:razor-pages/filter#authorize-filter-attribute)。</span><span class="sxs-lookup"><span data-stu-id="ef70f-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="ef70f-157">需要授權才能存取頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="ef70f-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="ef70f-158">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="ef70f-159">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ef70f-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="ef70f-160">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="ef70f-161">需要授權才能存取區域頁面</span><span class="sxs-lookup"><span data-stu-id="ef70f-161">Require authorization to access an area page</span></span>

<span data-ttu-id="ef70f-162">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至位於指定路徑的 [區域] 頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="ef70f-163">[頁面名稱] 是檔案的路徑，沒有副檔名相對於指定區域的頁面根目錄。</span><span class="sxs-lookup"><span data-stu-id="ef70f-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef70f-164">例如，檔案*區域/ Identity /Pages/Manage/Accounts.cshtml*的頁面名稱是 */Manage/Accounts*。</span><span class="sxs-lookup"><span data-stu-id="ef70f-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="ef70f-165">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaPage](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="ef70f-166">需要授權才能存取區域的資料夾</span><span class="sxs-lookup"><span data-stu-id="ef70f-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="ef70f-167">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至資料夾中指定路徑的所有區域：</span><span class="sxs-lookup"><span data-stu-id="ef70f-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="ef70f-168">資料夾路徑是相對於指定區域的頁面根目錄的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="ef70f-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="ef70f-169">例如，*區域/ Identity /Pages/Manage/* 下檔案的資料夾路徑為 */Manage*。</span><span class="sxs-lookup"><span data-stu-id="ef70f-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="ef70f-170">若要指定[授權原則](xref:security/authorization/policies)，請使用[AuthorizeAreaFolder](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*)多載：</span><span class="sxs-lookup"><span data-stu-id="ef70f-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="ef70f-171">允許匿名存取頁面</span><span class="sxs-lookup"><span data-stu-id="ef70f-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="ef70f-172">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至位於指定路徑的頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="ef70f-173">指定的路徑為 View Engine path，這是 Razor 頁面根相對路徑，沒有副檔名，而且只包含正斜線。</span><span class="sxs-lookup"><span data-stu-id="ef70f-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="ef70f-174">允許匿名存取頁面資料夾</span><span class="sxs-lookup"><span data-stu-id="ef70f-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="ef70f-175">透過使用 <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> 慣例 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ，將新增 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 至資料夾中指定路徑的所有頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="ef70f-176">指定的路徑是 View Engine path，這是 Razor 頁面根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="ef70f-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="ef70f-177">結合已授權和匿名存取的注意事項</span><span class="sxs-lookup"><span data-stu-id="ef70f-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="ef70f-178">有效的方式是指定需要授權的頁面資料夾，並指定該資料夾內的頁面允許匿名存取：</span><span class="sxs-lookup"><span data-stu-id="ef70f-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="ef70f-179">不過，相反的，這是不正確。</span><span class="sxs-lookup"><span data-stu-id="ef70f-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="ef70f-180">您無法針對匿名存取宣告頁面的資料夾，然後在該資料夾內指定需要授權的頁面：</span><span class="sxs-lookup"><span data-stu-id="ef70f-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="ef70f-181">在私用頁面上要求授權失敗。</span><span class="sxs-lookup"><span data-stu-id="ef70f-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="ef70f-182">當 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 和都套用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> 至頁面時，會優先使用 <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> 並控制存取。</span><span class="sxs-lookup"><span data-stu-id="ef70f-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ef70f-183">其他資源</span><span class="sxs-lookup"><span data-stu-id="ef70f-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
