---
title: ASP.NET Core MVC 中的快取標籤協助程式
author: pkellner
description: 了解如何使用快取標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
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
uid: mvc/views/tag-helpers/builtin-th/cache-tag-helper
ms.openlocfilehash: 7d2ff774b7654993e2cd9b126db252f81a3032d3
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88018750"
---
# <a name="cache-tag-helper-in-aspnet-core-mvc"></a><span data-ttu-id="22635-103">ASP.NET Core MVC 中的快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="22635-103">Cache Tag Helper in ASP.NET Core MVC</span></span>

<span data-ttu-id="22635-104">由 [Peter Kellner](https://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="22635-104">By [Peter Kellner](https://peterkellner.net)</span></span>

<span data-ttu-id="22635-105">快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至內部 ASP.NET Core 快取提供者，以提升應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="22635-105">The Cache Tag Helper provides the ability to improve the performance of your ASP.NET Core app by caching its content to the internal ASP.NET Core cache provider.</span></span>

<span data-ttu-id="22635-106">如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。</span><span class="sxs-lookup"><span data-stu-id="22635-106">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="22635-107">下列標記會快取 Razor 目前的日期：</span><span class="sxs-lookup"><span data-stu-id="22635-107">The following Razor markup caches the current date:</span></span>

```cshtml
<cache>@DateTime.Now</cache>
```

<span data-ttu-id="22635-108">第一個對包含標籤協助程式之頁面的要求會顯示目前的日期/時間。</span><span class="sxs-lookup"><span data-stu-id="22635-108">The first request to the page that contains the Tag Helper displays the current date.</span></span> <span data-ttu-id="22635-109">其他要求則會顯示在快取過期 (預設為 20 分鐘) 之前或快取日期從快取撤銷之前的快取值。</span><span class="sxs-lookup"><span data-stu-id="22635-109">Additional requests show the cached value until the cache expires (default 20 minutes) or until the cached date is evicted from the cache.</span></span>

## <a name="cache-tag-helper-attributes"></a><span data-ttu-id="22635-110">快取標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="22635-110">Cache Tag Helper Attributes</span></span>

### <a name="enabled"></a><span data-ttu-id="22635-111">已啟用</span><span class="sxs-lookup"><span data-stu-id="22635-111">enabled</span></span>

| <span data-ttu-id="22635-112">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-112">Attribute Type</span></span>  | <span data-ttu-id="22635-113">範例</span><span class="sxs-lookup"><span data-stu-id="22635-113">Examples</span></span>        | <span data-ttu-id="22635-114">預設</span><span class="sxs-lookup"><span data-stu-id="22635-114">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="22635-115">Boolean</span><span class="sxs-lookup"><span data-stu-id="22635-115">Boolean</span></span>         | <span data-ttu-id="22635-116">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="22635-116">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="22635-117">`enabled` 會決定是否快取以快取標籤協助程式括住的內容。</span><span class="sxs-lookup"><span data-stu-id="22635-117">`enabled` determines if the content enclosed by the Cache Tag Helper is cached.</span></span> <span data-ttu-id="22635-118">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="22635-118">The default is `true`.</span></span> <span data-ttu-id="22635-119">如果設定為 `false`，則轉譯輸出**不會**被快取。</span><span class="sxs-lookup"><span data-stu-id="22635-119">If set to `false`, the rendered output is **not** cached.</span></span>

<span data-ttu-id="22635-120">範例：</span><span class="sxs-lookup"><span data-stu-id="22635-120">Example:</span></span>

```cshtml
<cache enabled="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-on"></a><span data-ttu-id="22635-121">expires-on</span><span class="sxs-lookup"><span data-stu-id="22635-121">expires-on</span></span>

| <span data-ttu-id="22635-122">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-122">Attribute Type</span></span>   | <span data-ttu-id="22635-123">範例</span><span class="sxs-lookup"><span data-stu-id="22635-123">Example</span></span>                            |
| ---------------- | ---------------------------------- |
| `DateTimeOffset` | `@new DateTime(2025,1,29,17,02,0)` |

<span data-ttu-id="22635-124">`expires-on` 可設定快取項目的絕對到期日。</span><span class="sxs-lookup"><span data-stu-id="22635-124">`expires-on` sets an absolute expiration date for the cached item.</span></span>

<span data-ttu-id="22635-125">下列範例會快取 2025 年 1 月 29 日下午 5:02 以前的快取標籤協助程式內容：</span><span class="sxs-lookup"><span data-stu-id="22635-125">The following example caches the contents of the Cache Tag Helper until 5:02 PM on January 29, 2025:</span></span>

```cshtml
<cache expires-on="@new DateTime(2025,1,29,17,02,0)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="expires-after"></a><span data-ttu-id="22635-126">expires-after</span><span class="sxs-lookup"><span data-stu-id="22635-126">expires-after</span></span>

| <span data-ttu-id="22635-127">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-127">Attribute Type</span></span> | <span data-ttu-id="22635-128">範例</span><span class="sxs-lookup"><span data-stu-id="22635-128">Example</span></span>                      | <span data-ttu-id="22635-129">預設值</span><span class="sxs-lookup"><span data-stu-id="22635-129">Default</span></span>    |
| -------------- | ---------------------------- | ---------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(120)` | <span data-ttu-id="22635-130">20 分鐘</span><span class="sxs-lookup"><span data-stu-id="22635-130">20 minutes</span></span> |

<span data-ttu-id="22635-131">`expires-after` 可設定從第一個要求時間到快取內容的時間長度。</span><span class="sxs-lookup"><span data-stu-id="22635-131">`expires-after` sets the length of time from the first request time to cache the contents.</span></span>

<span data-ttu-id="22635-132">範例：</span><span class="sxs-lookup"><span data-stu-id="22635-132">Example:</span></span>

```cshtml
<cache expires-after="@TimeSpan.FromSeconds(120)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="22635-133">RazorView 引擎會將預設 `expires-after` 值設定為20分鐘。</span><span class="sxs-lookup"><span data-stu-id="22635-133">The Razor View Engine sets the default `expires-after` value to twenty minutes.</span></span>

### <a name="expires-sliding"></a><span data-ttu-id="22635-134">expires-sliding</span><span class="sxs-lookup"><span data-stu-id="22635-134">expires-sliding</span></span>

| <span data-ttu-id="22635-135">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-135">Attribute Type</span></span> | <span data-ttu-id="22635-136">範例</span><span class="sxs-lookup"><span data-stu-id="22635-136">Example</span></span>                     |
| -------------- | --------------------------- |
| `TimeSpan`     | `@TimeSpan.FromSeconds(60)` |

<span data-ttu-id="22635-137">設定快取項目的值多久未被存取時應該撤出。</span><span class="sxs-lookup"><span data-stu-id="22635-137">Sets the time that a cache entry should be evicted if its value hasn't been accessed.</span></span>

<span data-ttu-id="22635-138">範例：</span><span class="sxs-lookup"><span data-stu-id="22635-138">Example:</span></span>

```cshtml
<cache expires-sliding="@TimeSpan.FromSeconds(60)">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-header"></a><span data-ttu-id="22635-139">vary-by-header</span><span class="sxs-lookup"><span data-stu-id="22635-139">vary-by-header</span></span>

| <span data-ttu-id="22635-140">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-140">Attribute Type</span></span> | <span data-ttu-id="22635-141">範例</span><span class="sxs-lookup"><span data-stu-id="22635-141">Examples</span></span>                                    |
| -------------- | ------------------------------------------- |
| <span data-ttu-id="22635-142">String</span><span class="sxs-lookup"><span data-stu-id="22635-142">String</span></span>         | <span data-ttu-id="22635-143">`User-Agent`, `User-Agent,content-encoding`</span><span class="sxs-lookup"><span data-stu-id="22635-143">`User-Agent`, `User-Agent,content-encoding`</span></span> |

<span data-ttu-id="22635-144">`vary-by-header` 接受標頭值的逗號分隔清單，在標頭值變更時，會觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="22635-144">`vary-by-header` accepts a comma-delimited list of header values that trigger a cache refresh when they change.</span></span>

<span data-ttu-id="22635-145">下列範例會監視標頭值 `User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="22635-145">The following example monitors the header value `User-Agent`.</span></span> <span data-ttu-id="22635-146">此範例會快取展示給網頁伺服器之每個不同 `User-Agent` 的內容：</span><span class="sxs-lookup"><span data-stu-id="22635-146">The example caches the content for every different `User-Agent` presented to the web server:</span></span>

```cshtml
<cache vary-by-header="User-Agent">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-query"></a><span data-ttu-id="22635-147">vary-by-query</span><span class="sxs-lookup"><span data-stu-id="22635-147">vary-by-query</span></span>

| <span data-ttu-id="22635-148">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-148">Attribute Type</span></span> | <span data-ttu-id="22635-149">範例</span><span class="sxs-lookup"><span data-stu-id="22635-149">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="22635-150">String</span><span class="sxs-lookup"><span data-stu-id="22635-150">String</span></span>         | <span data-ttu-id="22635-151">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="22635-151">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="22635-152">`vary-by-query` 接受查詢字串 (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) 中 <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> 的逗點分隔清單，當任何所列索引碼的值變更時，會觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="22635-152">`vary-by-query` accepts a comma-delimited list of <xref:Microsoft.AspNetCore.Http.IQueryCollection.Keys*> in a query string (<xref:Microsoft.AspNetCore.Http.HttpRequest.Query*>) that trigger a cache refresh when the value of any listed key changes.</span></span>

<span data-ttu-id="22635-153">下列範例會監視 `Make` 與 `Model` 的值。</span><span class="sxs-lookup"><span data-stu-id="22635-153">The following example monitors the values of `Make` and `Model`.</span></span> <span data-ttu-id="22635-154">此範例會快取展示給網頁伺服器之每個不同 `Make` 與 `Model` 的內容：</span><span class="sxs-lookup"><span data-stu-id="22635-154">The example caches the content for every different `Make` and `Model` presented to the web server:</span></span>

```cshtml
<cache vary-by-query="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-route"></a><span data-ttu-id="22635-155">vary-by-route</span><span class="sxs-lookup"><span data-stu-id="22635-155">vary-by-route</span></span>

| <span data-ttu-id="22635-156">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-156">Attribute Type</span></span> | <span data-ttu-id="22635-157">範例</span><span class="sxs-lookup"><span data-stu-id="22635-157">Examples</span></span>             |
| -------------- | -------------------- |
| <span data-ttu-id="22635-158">String</span><span class="sxs-lookup"><span data-stu-id="22635-158">String</span></span>         | <span data-ttu-id="22635-159">`Make`, `Make,Model`</span><span class="sxs-lookup"><span data-stu-id="22635-159">`Make`, `Make,Model`</span></span> |

<span data-ttu-id="22635-160">`vary-by-route` 接受路由參數名稱的逗點分隔清單，當路由資料參數值變更時，這些路由參數名稱會觸發快取重新整理。</span><span class="sxs-lookup"><span data-stu-id="22635-160">`vary-by-route` accepts a comma-delimited list of route parameter names that trigger a cache refresh when the route data parameter value changes.</span></span>

<span data-ttu-id="22635-161">範例：</span><span class="sxs-lookup"><span data-stu-id="22635-161">Example:</span></span>

<span data-ttu-id="22635-162">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="22635-162">*Startup.cs*:</span></span>

```csharp
routes.MapRoute(
    name: "default",
    template: "{controller=Home}/{action=Index}/{Make?}/{Model?}");
```

<span data-ttu-id="22635-163">*Index. cshtml*：</span><span class="sxs-lookup"><span data-stu-id="22635-163">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by-route="Make,Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-no-loccookie"></a><span data-ttu-id="22635-164">依cookie</span><span class="sxs-lookup"><span data-stu-id="22635-164">vary-by-cookie</span></span>

| <span data-ttu-id="22635-165">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-165">Attribute Type</span></span> | <span data-ttu-id="22635-166">範例</span><span class="sxs-lookup"><span data-stu-id="22635-166">Examples</span></span>                                                                         |
| -------------- | -------------------------------------------------------------------------------- |
| <span data-ttu-id="22635-167">String</span><span class="sxs-lookup"><span data-stu-id="22635-167">String</span></span>         | <span data-ttu-id="22635-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span><span class="sxs-lookup"><span data-stu-id="22635-168">`.AspNetCore.Identity.Application`, `.AspNetCore.Identity.Application,HairColor`</span></span> |

<span data-ttu-id="22635-169">`vary-by-cookie`接受以逗號分隔的名稱清單 cookie ，這些名稱會在值變更時觸發快取重新整理 cookie 。</span><span class="sxs-lookup"><span data-stu-id="22635-169">`vary-by-cookie` accepts a comma-delimited list of cookie names that trigger a cache refresh when the cookie values change.</span></span>

<span data-ttu-id="22635-170">下列範例會監視 cookie 與 ASP.NET Core 相關聯的 Identity 。</span><span class="sxs-lookup"><span data-stu-id="22635-170">The following example monitors the cookie associated with ASP.NET Core Identity.</span></span> <span data-ttu-id="22635-171">當使用者通過驗證時，中的變更會觸發快取重新整理 Identity cookie ：</span><span class="sxs-lookup"><span data-stu-id="22635-171">When a user is authenticated, a change in the Identity cookie triggers a cache refresh:</span></span>

```cshtml
<cache vary-by-cookie=".AspNetCore.Identity.Application">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="vary-by-user"></a><span data-ttu-id="22635-172">vary-by-user</span><span class="sxs-lookup"><span data-stu-id="22635-172">vary-by-user</span></span>

| <span data-ttu-id="22635-173">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-173">Attribute Type</span></span>  | <span data-ttu-id="22635-174">範例</span><span class="sxs-lookup"><span data-stu-id="22635-174">Examples</span></span>        | <span data-ttu-id="22635-175">預設</span><span class="sxs-lookup"><span data-stu-id="22635-175">Default</span></span> |
| --------------- | --------------- | ------- |
| <span data-ttu-id="22635-176">Boolean</span><span class="sxs-lookup"><span data-stu-id="22635-176">Boolean</span></span>         | <span data-ttu-id="22635-177">`true`, `false`</span><span class="sxs-lookup"><span data-stu-id="22635-177">`true`, `false`</span></span> | `true`  |

<span data-ttu-id="22635-178">`vary-by-user` 可指定當登入的使用者 (或內容主體) 變更時，是否重設快取。</span><span class="sxs-lookup"><span data-stu-id="22635-178">`vary-by-user` specifies whether or not the cache resets when the signed-in user (or Context Principal) changes.</span></span> <span data-ttu-id="22635-179">目前的使用者也稱為要求內容主體，而且可以藉 Razor 由參考在視圖中查看 `@User.Identity.Name` 。</span><span class="sxs-lookup"><span data-stu-id="22635-179">The current user is also known as the Request Context Principal and can be viewed in a Razor view by referencing `@User.Identity.Name`.</span></span>

<span data-ttu-id="22635-180">下列範例會監視目前登入的使用者，以觸發快取重新整理：</span><span class="sxs-lookup"><span data-stu-id="22635-180">The following example monitors the current logged in user to trigger a cache refresh:</span></span>

```cshtml
<cache vary-by-user="true">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="22635-181">使用此屬性可保留登入與登出週期中的快取內容。</span><span class="sxs-lookup"><span data-stu-id="22635-181">Using this attribute maintains the contents in cache through a sign-in and sign-out cycle.</span></span> <span data-ttu-id="22635-182">當此值設定為 `true` 時，驗證週期將使已驗證之使用者的快取無效。</span><span class="sxs-lookup"><span data-stu-id="22635-182">When the value is set to `true`, an authentication cycle invalidates the cache for the authenticated user.</span></span> <span data-ttu-id="22635-183">快取已失效，因為 cookie 當使用者通過驗證時，會產生新的唯一值。</span><span class="sxs-lookup"><span data-stu-id="22635-183">The cache is invalidated because a new unique cookie value is generated when a user is authenticated.</span></span> <span data-ttu-id="22635-184">當不 cookie 存在或已過期時，就會針對匿名狀態維護快取 cookie 。</span><span class="sxs-lookup"><span data-stu-id="22635-184">Cache is maintained for the anonymous state when no cookie is present or the cookie has expired.</span></span> <span data-ttu-id="22635-185">如果使用者**未**通過驗證，則會維護快取。</span><span class="sxs-lookup"><span data-stu-id="22635-185">If the user is **not** authenticated, the cache is maintained.</span></span>

### <a name="vary-by"></a><span data-ttu-id="22635-186">vary-by</span><span class="sxs-lookup"><span data-stu-id="22635-186">vary-by</span></span>

| <span data-ttu-id="22635-187">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-187">Attribute Type</span></span> | <span data-ttu-id="22635-188">範例</span><span class="sxs-lookup"><span data-stu-id="22635-188">Example</span></span>  |
| -------------- | -------- |
| <span data-ttu-id="22635-189">String</span><span class="sxs-lookup"><span data-stu-id="22635-189">String</span></span>         | `@Model` |

<span data-ttu-id="22635-190">`vary-by` 可自訂要快取哪些資料。</span><span class="sxs-lookup"><span data-stu-id="22635-190">`vary-by` allows for customization of what data is cached.</span></span> <span data-ttu-id="22635-191">當屬性字串值所參考的物件變更時，就會更新快取標籤協助程式的內容。</span><span class="sxs-lookup"><span data-stu-id="22635-191">When the object referenced by the attribute's string value changes, the content of the Cache Tag Helper is updated.</span></span> <span data-ttu-id="22635-192">通常會對此屬性指派模型值的字串串連。</span><span class="sxs-lookup"><span data-stu-id="22635-192">Often, a string-concatenation of model values are assigned to this attribute.</span></span> <span data-ttu-id="22635-193">實際上，這會導致更新任何串連值都會使快取失效的情節。</span><span class="sxs-lookup"><span data-stu-id="22635-193">Effectively, this results in a scenario where an update to any of the concatenated values invalidates the cache.</span></span>

<span data-ttu-id="22635-194">下列範例假設轉譯檢視的控制器方法加總兩個路由參數 (`myParam1` 與 `myParam2`) 的整數值，並以單一模型屬性傳回加總。</span><span class="sxs-lookup"><span data-stu-id="22635-194">The following example assumes the controller method rendering the view sums the integer value of the two route parameters, `myParam1` and `myParam2`, and returns the sum as the single model property.</span></span> <span data-ttu-id="22635-195">當此總和變更時，快取標籤協助程式的內容會重新轉譯和快取。</span><span class="sxs-lookup"><span data-stu-id="22635-195">When this sum changes, the content of the Cache Tag Helper is rendered and cached again.</span></span>  

<span data-ttu-id="22635-196">動作：</span><span class="sxs-lookup"><span data-stu-id="22635-196">Action:</span></span>

```csharp
public IActionResult Index(string myParam1, string myParam2, string myParam3)
{
    int num1;
    int num2;
    int.TryParse(myParam1, out num1);
    int.TryParse(myParam2, out num2);
    return View(viewName, num1 + num2);
}
```

<span data-ttu-id="22635-197">*Index. cshtml*：</span><span class="sxs-lookup"><span data-stu-id="22635-197">*Index.cshtml*:</span></span>

```cshtml
<cache vary-by="@Model">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

### <a name="priority"></a><span data-ttu-id="22635-198">priority</span><span class="sxs-lookup"><span data-stu-id="22635-198">priority</span></span>

| <span data-ttu-id="22635-199">屬性類型</span><span class="sxs-lookup"><span data-stu-id="22635-199">Attribute Type</span></span>      | <span data-ttu-id="22635-200">範例</span><span class="sxs-lookup"><span data-stu-id="22635-200">Examples</span></span>                               | <span data-ttu-id="22635-201">預設</span><span class="sxs-lookup"><span data-stu-id="22635-201">Default</span></span>  |
| ------------------- | -------------------------------------- | -------- |
| `CacheItemPriority` | <span data-ttu-id="22635-202">`High`, `Low`, `NeverRemove`, `Normal`</span><span class="sxs-lookup"><span data-stu-id="22635-202">`High`, `Low`, `NeverRemove`, `Normal`</span></span> | `Normal` |

<span data-ttu-id="22635-203">`priority` 可提供快取撤銷指導方針給內建快取提供者。</span><span class="sxs-lookup"><span data-stu-id="22635-203">`priority` provides cache eviction guidance to the built-in cache provider.</span></span> <span data-ttu-id="22635-204">當記憶體不足時，網頁伺服器會先撤出 `Low` 快取項目。</span><span class="sxs-lookup"><span data-stu-id="22635-204">The web server evicts `Low` cache entries first when it's under memory pressure.</span></span>

<span data-ttu-id="22635-205">範例：</span><span class="sxs-lookup"><span data-stu-id="22635-205">Example:</span></span>

```cshtml
<cache priority="High">
    Current Time Inside Cache Tag Helper: @DateTime.Now
</cache>
```

<span data-ttu-id="22635-206">`priority` 屬性並不保證特定快取保留層級。</span><span class="sxs-lookup"><span data-stu-id="22635-206">The `priority` attribute doesn't guarantee a specific level of cache retention.</span></span> <span data-ttu-id="22635-207">`CacheItemPriority` 僅供建議。</span><span class="sxs-lookup"><span data-stu-id="22635-207">`CacheItemPriority` is only a suggestion.</span></span> <span data-ttu-id="22635-208">將此屬性設定為 `NeverRemove` 並不保證一定會保留快取項目。</span><span class="sxs-lookup"><span data-stu-id="22635-208">Setting this attribute to `NeverRemove` doesn't guarantee that cached items are always retained.</span></span> <span data-ttu-id="22635-209">請參閱[其他資源](#additional-resources)一節中的主題，以取得詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="22635-209">See the topics in the [Additional Resources](#additional-resources) section for more information.</span></span>

<span data-ttu-id="22635-210">快取標籤協助程式相依於[記憶體快取服務](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="22635-210">The Cache Tag Helper is dependent on the [memory cache service](xref:performance/caching/memory).</span></span> <span data-ttu-id="22635-211">如果尚未新增此服務，快取標籤協助程式會予以新增。</span><span class="sxs-lookup"><span data-stu-id="22635-211">The Cache Tag Helper adds the service if it hasn't been added.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22635-212">其他資源</span><span class="sxs-lookup"><span data-stu-id="22635-212">Additional resources</span></span>

* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
