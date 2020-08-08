---
title: 在 cookie 不 ASP.NET Core 的情況下使用驗證Identity
author: rick-anderson
description: 瞭解如何在 cookie 不 ASP.NET Core 的情況下使用驗證 Identity 。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
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
uid: security/authentication/cookie
ms.openlocfilehash: 325bc3dcd48517d93d5f6f2d56e92651c780d759
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88021883"
---
# <a name="use-no-loccookie-authentication-without-aspnet-core-no-locidentity"></a><span data-ttu-id="9e2d8-103">在 cookie 不 ASP.NET Core 的情況下使用驗證Identity</span><span class="sxs-lookup"><span data-stu-id="9e2d8-103">Use cookie authentication without ASP.NET Core Identity</span></span>

<span data-ttu-id="9e2d8-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9e2d8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9e2d8-105">ASP.NET Core Identity 是完整的完整功能驗證提供者，可用於建立和維護登入。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-105">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="9e2d8-106">不過，您 cookie 可以使用不含 ASP.NET Core 的驗證提供者 Identity 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-106">However, a cookie-based authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="9e2d8-107">如需詳細資訊，請參閱<xref:security/authentication/identity>。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-107">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="9e2d8-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="9e2d8-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9e2d8-109">針對範例應用程式中的示範用途，假設使用者的使用者帳戶（Maria Rodriguez）已硬式編碼到應用程式中。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-109">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="9e2d8-110">使用 [**電子郵件**位址] `maria.rodriguez@contoso.com` 和 [任何密碼] 來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-110">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="9e2d8-111">使用者會在 `AuthenticateUser` *頁面/帳戶/登入. cshtml .cs*檔案的方法中進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-111">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="9e2d8-112">在真實世界的範例中，使用者會針對資料庫進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-112">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="9e2d8-113">組態</span><span class="sxs-lookup"><span data-stu-id="9e2d8-113">Configuration</span></span>

<span data-ttu-id="9e2d8-114">如果應用程式不使用[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)，請在 AspNetCore 的專案檔中建立套件參考[。 Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)套件。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-114">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="9e2d8-115">在 `Startup.ConfigureServices` 方法中，使用和方法建立驗證中介軟體 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 服務 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-115">In the `Startup.ConfigureServices` method, create the Authentication Middleware services with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="9e2d8-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>傳遞以 `AddAuthentication` 設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-116"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="9e2d8-117">`AuthenticationScheme`當有多個 cookie 驗證實例，而您想要以[特定的配置進行授權](xref:security/authorization/limitingidentitybyscheme)時，會很有用。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-117">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="9e2d8-118">將設定 `AuthenticationScheme` 為[ Cookie AuthenticationDefaults](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme)時，AuthenticationScheme 會 Cookie 為配置提供 "s" 的值。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-118">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="9e2d8-119">您可以提供可區分配置的任何字串值。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-119">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="9e2d8-120">應用程式的驗證配置與應用程式的 cookie 驗證配置不同。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-120">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="9e2d8-121">cookie未提供驗證配置給時 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> ，它會使用 `CookieAuthenticationDefaults.AuthenticationScheme` ( " Cookie s" ) 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-121">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="9e2d8-122">cookie根據預設，驗證的 <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> 屬性會設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-122">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="9e2d8-123">cookie當網站訪客未同意資料收集時，允許驗證。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-123">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="9e2d8-124">如需詳細資訊，請參閱<xref:security/gdpr#essential-cookies>。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-124">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="9e2d8-125">在中 `Startup.Configure` ，呼叫 `UseAuthentication` 和， `UseAuthorization` 以設定 `HttpContext.User` 要求的屬性和執行授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-125">In `Startup.Configure`, call `UseAuthentication` and `UseAuthorization` to set the `HttpContext.User` property and run Authorization Middleware for requests.</span></span> <span data-ttu-id="9e2d8-126">呼叫 `UseAuthentication` 和 `UseAuthorization` 方法，再呼叫 `UseEndpoints` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-126">Call the `UseAuthentication` and `UseAuthorization` methods before calling `UseEndpoints`:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="9e2d8-127"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>類別是用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-127">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="9e2d8-128">在 `CookieAuthenticationOptions` 服務設定中，于方法中進行驗證 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-128">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="no-loccookie-policy-middleware"></a><span data-ttu-id="9e2d8-129">Cookie原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="9e2d8-129">Cookie Policy Middleware</span></span>

<span data-ttu-id="9e2d8-130">[ Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)可啟用 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-130">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="9e2d8-131">將中介軟體新增至應用程式處理管線會區分順序， &mdash; 它只會影響在管線中註冊的下游元件。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-131">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="9e2d8-132">使用 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> 提供給 Cookie 原則中介軟體，以控制 cookie cookie 當附加或刪除時，處理和連結處理處理常式的全域特性 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-132">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="9e2d8-133">預設 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> 值是 `SameSiteMode.Lax` 允許 OAuth2 authentication。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-133">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="9e2d8-134">若要嚴格地強制執行的相同網站原則 `SameSiteMode.Strict` ，請設定 `MinimumSameSitePolicy` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-134">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="9e2d8-135">雖然這項設定會中斷 OAuth2 和其他跨原始來源驗證配置，但它會提升 cookie 不依賴跨原始來源要求處理之其他類型應用程式的安全性層級。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-135">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="9e2d8-136">的 Cookie 原則中介軟體設定 `MinimumSameSitePolicy` 可以 `Cookie.SameSite` 根據下列矩陣，影響 [設定] 中的設定 `CookieAuthenticationOptions` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-136">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="9e2d8-137">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="9e2d8-137">MinimumSameSitePolicy</span></span> | <span data-ttu-id="9e2d8-138">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="9e2d8-138">Cookie.SameSite</span></span> | <span data-ttu-id="9e2d8-139">結果 Cookie 。SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="9e2d8-139">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="9e2d8-140">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-140">SameSiteMode.None</span></span>     | <span data-ttu-id="9e2d8-141">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-141">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-142">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-142">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-143">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-143">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e2d8-144">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-144">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-145">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-145">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-146">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-146">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="9e2d8-147">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-147">SameSiteMode.Lax</span></span>      | <span data-ttu-id="9e2d8-148">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-148">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-149">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-149">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-150">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-150">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e2d8-151">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-151">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-152">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-152">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-153">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-153">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="9e2d8-154">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-154">SameSiteMode.Strict</span></span>   | <span data-ttu-id="9e2d8-155">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-155">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-156">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-156">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-157">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-157">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e2d8-158">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-158">SameSiteMode.Strict</span></span><br><span data-ttu-id="9e2d8-159">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-159">SameSiteMode.Strict</span></span><br><span data-ttu-id="9e2d8-160">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-160">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-no-loccookie"></a><span data-ttu-id="9e2d8-161">建立驗證cookie</span><span class="sxs-lookup"><span data-stu-id="9e2d8-161">Create an authentication cookie</span></span>

<span data-ttu-id="9e2d8-162">若要建立 cookie 保存使用者資訊，請建立 <xref:System.Security.Claims.ClaimsPrincipal> 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-162">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="9e2d8-163">使用者資訊會序列化並儲存在中 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-163">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="9e2d8-164"><xref:System.Security.Claims.ClaimsIdentity>使用任何必要的 <xref:System.Security.Claims.Claim> 來建立，並呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> 以登入使用者：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-164">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="9e2d8-165">`SignInAsync`建立已加密的 cookie ，並將它新增至目前的回應。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-165">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="9e2d8-166">如果 `AuthenticationScheme` 未指定，則會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-166">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="9e2d8-167"><xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.RedirectUri>根據預設，只會用於一些特定路徑，例如登入路徑和登出路徑。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-167"><xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.RedirectUri> is only used on a few specific paths by default, for example, the login path and logout paths.</span></span> <span data-ttu-id="9e2d8-168">如需詳細資訊，請參閱[ Cookie AuthenticationHandler 來源](https://github.com/dotnet/aspnetcore/blob/f2e6e6ff334176540ef0b3291122e359c2106d1a/src/Security/Authentication/Cookies/src/CookieAuthenticationHandler.cs#L334)。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-168">For more information see the [CookieAuthenticationHandler source](https://github.com/dotnet/aspnetcore/blob/f2e6e6ff334176540ef0b3291122e359c2106d1a/src/Security/Authentication/Cookies/src/CookieAuthenticationHandler.cs#L334).</span></span>

<span data-ttu-id="9e2d8-169">ASP.NET Core 的[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-169">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="9e2d8-170">若為裝載于多部電腦上的應用程式、跨應用程式的負載平衡，或使用 web 伺服陣列，請[將資料保護設定](xref:security/data-protection/configuration/overview)為使用相同的金鑰環形和應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-170">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="9e2d8-171">登出</span><span class="sxs-lookup"><span data-stu-id="9e2d8-171">Sign out</span></span>

<span data-ttu-id="9e2d8-172">若要登出目前的使用者並刪除其 cookie ，請呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-172">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="9e2d8-173">如果 `CookieAuthenticationDefaults.AuthenticationScheme` (或 " Cookie s" ) 不是用來做為配置 (例如 "Contoso Cookie " ) ，請提供設定驗證提供者時所使用的架構。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-173">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="9e2d8-174">否則，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-174">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="9e2d8-175">對後端變更做出回應</span><span class="sxs-lookup"><span data-stu-id="9e2d8-175">React to back-end changes</span></span>

<span data-ttu-id="9e2d8-176">一旦 cookie 建立之後， cookie 就是身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-176">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="9e2d8-177">如果在後端系統中停用使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-177">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="9e2d8-178">應用程式的 cookie 驗證系統會繼續根據驗證來處理要求 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-178">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="9e2d8-179">只要驗證有效，使用者就會繼續登入應用程式 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-179">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="9e2d8-180"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*>事件可以用來攔截和覆寫身分識別的驗證 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-180">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="9e2d8-181">cookie在每個要求上驗證，可降低撤銷存取應用程式之使用者的風險。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-181">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="9e2d8-182">驗證的其中一個方法 cookie 是以追蹤使用者資料庫變更的時間為基礎。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-182">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="9e2d8-183">如果自從使用者發出之後，資料庫尚未變更 cookie ，則不需要重新驗證使用者（如果 cookie 仍然有效）。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-183">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="9e2d8-184">在範例應用程式中，資料庫會在中實作為， `IUserRepository` 並儲存 `LastChanged` 值。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-184">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="9e2d8-185">當資料庫中的使用者更新時，此 `LastChanged` 值會設為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-185">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="9e2d8-186">若要在 cookie 資料庫根據值而變更時使失效 `LastChanged` ，請 cookie 使用 `LastChanged` 包含 `LastChanged` 資料庫目前值的宣告來建立：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-186">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

<span data-ttu-id="9e2d8-187">若要執行事件的覆寫 `ValidatePrincipal` ，請在衍生自的類別中撰寫具有下列簽章的方法 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-187">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="9e2d8-188">以下是的範例執行 `CookieAuthenticationEvents` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-188">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="9e2d8-189">cookie在方法的服務註冊期間註冊事件實例 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-189">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9e2d8-190">為您的類別提供[限定範圍的服務註冊](xref:fundamentals/dependency-injection#service-lifetimes) `CustomCookieAuthenticationEvents` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-190">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="9e2d8-191">假設使用者的名稱已更新 &mdash; 為不會以任何方式影響安全性的決策。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-191">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="9e2d8-192">如果您想要以非破壞性的更新使用者主體，請呼叫， `context.ReplacePrincipal` 並將 `context.ShouldRenew` 屬性設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-192">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="9e2d8-193">這裡所述的方法會在每個要求上觸發。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-193">The approach described here is triggered on every request.</span></span> <span data-ttu-id="9e2d8-194">cookie針對每個要求的所有使用者驗證驗證，可能會對應用程式造成很大的效能影響。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-194">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-no-loccookies"></a><span data-ttu-id="9e2d8-195">持續性 cookie s</span><span class="sxs-lookup"><span data-stu-id="9e2d8-195">Persistent cookies</span></span>

<span data-ttu-id="9e2d8-196">您可能想要在 cookie 瀏覽器會話之間保存。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-196">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="9e2d8-197">只有在登入或類似的機制上有 [記住我] 核取方塊的明確使用者同意，才能啟用此持續性。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-197">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="9e2d8-198">下列程式碼片段會建立身分識別，並 cookie 透過瀏覽器結束來對應該不受。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-198">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="9e2d8-199">系統會接受先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-199">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="9e2d8-200">如果在 cookie 瀏覽器關閉時過期，瀏覽器會在 cookie 重新開機後清除。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-200">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="9e2d8-201"><xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent>在中，將設為 `true` <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-201">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-no-loccookie-expiration"></a><span data-ttu-id="9e2d8-202">絕對 cookie 到期</span><span class="sxs-lookup"><span data-stu-id="9e2d8-202">Absolute cookie expiration</span></span>

<span data-ttu-id="9e2d8-203">絕對到期時間可以使用來設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc> 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-203">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="9e2d8-204">若要建立持續性 cookie ， `IsPersistent` 也必須設定。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-204">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="9e2d8-205">否則， cookie 會使用以會話為基礎的存留期來建立，而且可能會在它所保留的驗證票證之前或之後過期。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-205">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="9e2d8-206">當 `ExpiresUtc` 設定時，它會覆寫 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> 選項的值 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> （如果已設定的話）。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-206">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="9e2d8-207">下列程式碼片段會建立持續20分鐘的身分識別和對應 cookie 的。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-207">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="9e2d8-208">這會忽略先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-208">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9e2d8-209">ASP.NET Core Identity 是完整的完整功能驗證提供者，可用於建立和維護登入。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-209">ASP.NET Core Identity is a complete, full-featured authentication provider for creating and maintaining logins.</span></span> <span data-ttu-id="9e2d8-210">不過，您 cookie 可以使用不含 ASP.NET Core 的驗證驗證提供者 Identity 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-210">However, a cookie-based authentication authentication provider without ASP.NET Core Identity can be used.</span></span> <span data-ttu-id="9e2d8-211">如需詳細資訊，請參閱<xref:security/authentication/identity>。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-211">For more information, see <xref:security/authentication/identity>.</span></span>

<span data-ttu-id="9e2d8-212">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="9e2d8-212">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9e2d8-213">針對範例應用程式中的示範用途，假設使用者的使用者帳戶（Maria Rodriguez）已硬式編碼到應用程式中。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-213">For demonstration purposes in the sample app, the user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="9e2d8-214">使用 [**電子郵件**位址] `maria.rodriguez@contoso.com` 和 [任何密碼] 來登入使用者。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-214">Use the **Email** address `maria.rodriguez@contoso.com` and any password to sign in the user.</span></span> <span data-ttu-id="9e2d8-215">使用者會在 `AuthenticateUser` *頁面/帳戶/登入. cshtml .cs*檔案的方法中進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-215">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="9e2d8-216">在真實世界的範例中，使用者會針對資料庫進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-216">In a real-world example, the user would be authenticated against a database.</span></span>

## <a name="configuration"></a><span data-ttu-id="9e2d8-217">組態</span><span class="sxs-lookup"><span data-stu-id="9e2d8-217">Configuration</span></span>

<span data-ttu-id="9e2d8-218">如果應用程式不使用[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)，請在 AspNetCore 的專案檔中建立套件參考[。 Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)套件。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-218">If the app doesn't use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), create a package reference in the project file for the [Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/) package.</span></span>

<span data-ttu-id="9e2d8-219">在 `Startup.ConfigureServices` 方法中，使用和方法建立驗證中介軟體 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 服務 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-219">In the `Startup.ConfigureServices` method, create the Authentication Middleware service with the <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> and <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> methods:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<span data-ttu-id="9e2d8-220"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme>傳遞以 `AddAuthentication` 設定應用程式的預設驗證配置。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-220"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> passed to `AddAuthentication` sets the default authentication scheme for the app.</span></span> <span data-ttu-id="9e2d8-221">`AuthenticationScheme`當有多個 cookie 驗證實例，而您想要以[特定的配置進行授權](xref:security/authorization/limitingidentitybyscheme)時，會很有用。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-221">`AuthenticationScheme` is useful when there are multiple instances of cookie authentication and you want to [authorize with a specific scheme](xref:security/authorization/limitingidentitybyscheme).</span></span> <span data-ttu-id="9e2d8-222">將設定 `AuthenticationScheme` 為[ Cookie AuthenticationDefaults](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme)時，AuthenticationScheme 會 Cookie 為配置提供 "s" 的值。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-222">Setting the `AuthenticationScheme` to [CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) provides a value of "Cookies" for the scheme.</span></span> <span data-ttu-id="9e2d8-223">您可以提供可區分配置的任何字串值。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-223">You can supply any string value that distinguishes the scheme.</span></span>

<span data-ttu-id="9e2d8-224">應用程式的驗證配置與應用程式的 cookie 驗證配置不同。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-224">The app's authentication scheme is different from the app's cookie authentication scheme.</span></span> <span data-ttu-id="9e2d8-225">cookie未提供驗證配置給時 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> ，它會使用 `CookieAuthenticationDefaults.AuthenticationScheme` ( " Cookie s" ) 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-225">When a cookie authentication scheme isn't provided to <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>, it uses `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="9e2d8-226">cookie根據預設，驗證的 <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> 屬性會設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-226">The authentication cookie's <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> property is set to `true` by default.</span></span> <span data-ttu-id="9e2d8-227">cookie當網站訪客未同意資料收集時，允許驗證。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-227">Authentication cookies are allowed when a site visitor hasn't consented to data collection.</span></span> <span data-ttu-id="9e2d8-228">如需詳細資訊，請參閱<xref:security/gdpr#essential-cookies>。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-228">For more information, see <xref:security/gdpr#essential-cookies>.</span></span>

<span data-ttu-id="9e2d8-229">在 `Startup.Configure` 方法中，呼叫 `UseAuthentication` 方法來叫用設定屬性的驗證中介軟體 `HttpContext.User` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-229">In the `Startup.Configure` method, call the `UseAuthentication` method to invoke the Authentication Middleware that sets the `HttpContext.User` property.</span></span> <span data-ttu-id="9e2d8-230">呼叫 `UseAuthentication` 或之前，請先呼叫方法 `UseMvcWithDefaultRoute` `UseMvc` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-230">Call the `UseAuthentication` method before calling `UseMvcWithDefaultRoute` or `UseMvc`:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<span data-ttu-id="9e2d8-231"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>類別是用來設定驗證提供者選項。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-231">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> class is used to configure the authentication provider options.</span></span>

<span data-ttu-id="9e2d8-232">在 `CookieAuthenticationOptions` 服務設定中，于方法中進行驗證 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-232">Set `CookieAuthenticationOptions` in the service configuration for authentication in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="no-loccookie-policy-middleware"></a><span data-ttu-id="9e2d8-233">Cookie原則中介軟體</span><span class="sxs-lookup"><span data-stu-id="9e2d8-233">Cookie Policy Middleware</span></span>

<span data-ttu-id="9e2d8-234">[ Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)可啟用 cookie 原則功能。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-234">[Cookie Policy Middleware](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware) enables cookie policy capabilities.</span></span> <span data-ttu-id="9e2d8-235">將中介軟體新增至應用程式處理管線會區分順序， &mdash; 它只會影響在管線中註冊的下游元件。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-235">Adding the middleware to the app processing pipeline is order sensitive&mdash;it only affects downstream components registered in the pipeline.</span></span>

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

<span data-ttu-id="9e2d8-236">使用 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> 提供給 Cookie 原則中介軟體，以控制 cookie cookie 當附加或刪除時，處理和連結處理處理常式的全域特性 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-236">Use <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions> provided to the Cookie Policy Middleware to control global characteristics of cookie processing and hook into cookie processing handlers when cookies are appended or deleted.</span></span>

<span data-ttu-id="9e2d8-237">預設 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> 值是 `SameSiteMode.Lax` 允許 OAuth2 authentication。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-237">The default <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> value is `SameSiteMode.Lax` to permit OAuth2 authentication.</span></span> <span data-ttu-id="9e2d8-238">若要嚴格地強制執行的相同網站原則 `SameSiteMode.Strict` ，請設定 `MinimumSameSitePolicy` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-238">To strictly enforce a same-site policy of `SameSiteMode.Strict`, set the `MinimumSameSitePolicy`.</span></span> <span data-ttu-id="9e2d8-239">雖然這項設定會中斷 OAuth2 和其他跨原始來源驗證配置，但它會提升 cookie 不依賴跨原始來源要求處理之其他類型應用程式的安全性層級。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-239">Although this setting breaks OAuth2 and other cross-origin authentication schemes, it elevates the level of cookie security for other types of apps that don't rely on cross-origin request processing.</span></span>

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

<span data-ttu-id="9e2d8-240">的 Cookie 原則中介軟體設定 `MinimumSameSitePolicy` 可以 `Cookie.SameSite` 根據下列矩陣，影響 [設定] 中的設定 `CookieAuthenticationOptions` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-240">The Cookie Policy Middleware setting for `MinimumSameSitePolicy` can affect the setting of `Cookie.SameSite` in `CookieAuthenticationOptions` settings according to the matrix below.</span></span>

| <span data-ttu-id="9e2d8-241">MinimumSameSitePolicy</span><span class="sxs-lookup"><span data-stu-id="9e2d8-241">MinimumSameSitePolicy</span></span> | <span data-ttu-id="9e2d8-242">Cookie.SameSite</span><span class="sxs-lookup"><span data-stu-id="9e2d8-242">Cookie.SameSite</span></span> | <span data-ttu-id="9e2d8-243">結果 Cookie 。SameSite 設定</span><span class="sxs-lookup"><span data-stu-id="9e2d8-243">Resultant Cookie.SameSite setting</span></span> |
| --------------------- | --------------- | --------------------------------- |
| <span data-ttu-id="9e2d8-244">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-244">SameSiteMode.None</span></span>     | <span data-ttu-id="9e2d8-245">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-245">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-246">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-246">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-247">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-247">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e2d8-248">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-248">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-249">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-249">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-250">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-250">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="9e2d8-251">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-251">SameSiteMode.Lax</span></span>      | <span data-ttu-id="9e2d8-252">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-252">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-253">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-253">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-254">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-254">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e2d8-255">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-255">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-256">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-256">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-257">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-257">SameSiteMode.Strict</span></span> |
| <span data-ttu-id="9e2d8-258">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-258">SameSiteMode.Strict</span></span>   | <span data-ttu-id="9e2d8-259">SameSiteMode。無</span><span class="sxs-lookup"><span data-stu-id="9e2d8-259">SameSiteMode.None</span></span><br><span data-ttu-id="9e2d8-260">SameSiteMode. 寬鬆</span><span class="sxs-lookup"><span data-stu-id="9e2d8-260">SameSiteMode.Lax</span></span><br><span data-ttu-id="9e2d8-261">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-261">SameSiteMode.Strict</span></span> | <span data-ttu-id="9e2d8-262">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-262">SameSiteMode.Strict</span></span><br><span data-ttu-id="9e2d8-263">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-263">SameSiteMode.Strict</span></span><br><span data-ttu-id="9e2d8-264">SameSiteMode。 Strict</span><span class="sxs-lookup"><span data-stu-id="9e2d8-264">SameSiteMode.Strict</span></span> |

## <a name="create-an-authentication-no-loccookie"></a><span data-ttu-id="9e2d8-265">建立驗證cookie</span><span class="sxs-lookup"><span data-stu-id="9e2d8-265">Create an authentication cookie</span></span>

<span data-ttu-id="9e2d8-266">若要建立 cookie 保存使用者資訊，請建立 <xref:System.Security.Claims.ClaimsPrincipal> 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-266">To create a cookie holding user information, construct a <xref:System.Security.Claims.ClaimsPrincipal>.</span></span> <span data-ttu-id="9e2d8-267">使用者資訊會序列化並儲存在中 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-267">The user information is serialized and stored in the cookie.</span></span> 

<span data-ttu-id="9e2d8-268"><xref:System.Security.Claims.ClaimsIdentity>使用任何必要的 <xref:System.Security.Claims.Claim> 來建立，並呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> 以登入使用者：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-268">Create a <xref:System.Security.Claims.ClaimsIdentity> with any required <xref:System.Security.Claims.Claim>s and call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> to sign in the user:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

<span data-ttu-id="9e2d8-269">`SignInAsync`建立已加密的 cookie ，並將它新增至目前的回應。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-269">`SignInAsync` creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="9e2d8-270">如果 `AuthenticationScheme` 未指定，則會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-270">If `AuthenticationScheme` isn't specified, the default scheme is used.</span></span>

<span data-ttu-id="9e2d8-271">ASP.NET Core 的[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-271">ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection) system is used for encryption.</span></span> <span data-ttu-id="9e2d8-272">若為裝載于多部電腦上的應用程式、跨應用程式的負載平衡，或使用 web 伺服陣列，請[將資料保護設定](xref:security/data-protection/configuration/overview)為使用相同的金鑰環形和應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-272">For an app hosted on multiple machines, load balancing across apps, or using a web farm, [configure data protection](xref:security/data-protection/configuration/overview) to use the same key ring and app identifier.</span></span>

## <a name="sign-out"></a><span data-ttu-id="9e2d8-273">登出</span><span class="sxs-lookup"><span data-stu-id="9e2d8-273">Sign out</span></span>

<span data-ttu-id="9e2d8-274">若要登出目前的使用者並刪除其 cookie ，請呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-274">To sign out the current user and delete their cookie, call <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:</span></span>

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

<span data-ttu-id="9e2d8-275">如果 `CookieAuthenticationDefaults.AuthenticationScheme` (或 " Cookie s" ) 不是用來做為配置 (例如 "Contoso Cookie " ) ，請提供設定驗證提供者時所使用的架構。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-275">If `CookieAuthenticationDefaults.AuthenticationScheme` (or "Cookies") isn't used as the scheme (for example, "ContosoCookie"), supply the scheme used when configuring the authentication provider.</span></span> <span data-ttu-id="9e2d8-276">否則，會使用預設配置。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-276">Otherwise, the default scheme is used.</span></span>

## <a name="react-to-back-end-changes"></a><span data-ttu-id="9e2d8-277">對後端變更做出回應</span><span class="sxs-lookup"><span data-stu-id="9e2d8-277">React to back-end changes</span></span>

<span data-ttu-id="9e2d8-278">一旦 cookie 建立之後， cookie 就是身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-278">Once a cookie is created, the cookie is the single source of identity.</span></span> <span data-ttu-id="9e2d8-279">如果在後端系統中停用使用者帳戶：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-279">If a user account is disabled in back-end systems:</span></span>

* <span data-ttu-id="9e2d8-280">應用程式的 cookie 驗證系統會繼續根據驗證來處理要求 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-280">The app's cookie authentication system continues to process requests based on the authentication cookie.</span></span>
* <span data-ttu-id="9e2d8-281">只要驗證有效，使用者就會繼續登入應用程式 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-281">The user remains signed into the app as long as the authentication cookie is valid.</span></span>

<span data-ttu-id="9e2d8-282"><xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*>事件可以用來攔截和覆寫身分識別的驗證 cookie 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-282">The <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> event can be used to intercept and override validation of the cookie identity.</span></span> <span data-ttu-id="9e2d8-283">cookie在每個要求上驗證，可降低撤銷存取應用程式之使用者的風險。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-283">Validating the cookie on every request mitigates the risk of revoked users accessing the app.</span></span>

<span data-ttu-id="9e2d8-284">驗證的其中一個方法 cookie 是以追蹤使用者資料庫變更的時間為基礎。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-284">One approach to cookie validation is based on keeping track of when the user database changes.</span></span> <span data-ttu-id="9e2d8-285">如果自從使用者發出之後，資料庫尚未變更 cookie ，則不需要重新驗證使用者（如果 cookie 仍然有效）。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-285">If the database hasn't been changed since the user's cookie was issued, there's no need to re-authenticate the user if their cookie is still valid.</span></span> <span data-ttu-id="9e2d8-286">在範例應用程式中，資料庫會在中實作為， `IUserRepository` 並儲存 `LastChanged` 值。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-286">In the sample app, the database is implemented in `IUserRepository` and stores a `LastChanged` value.</span></span> <span data-ttu-id="9e2d8-287">當資料庫中的使用者更新時，此 `LastChanged` 值會設為目前的時間。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-287">When a user is updated in the database, the `LastChanged` value is set to the current time.</span></span>

<span data-ttu-id="9e2d8-288">若要在 cookie 資料庫根據值而變更時使失效 `LastChanged` ，請 cookie 使用 `LastChanged` 包含 `LastChanged` 資料庫目前值的宣告來建立：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-288">In order to invalidate a cookie when the database changes based on the `LastChanged` value, create the cookie with a `LastChanged` claim containing the current `LastChanged` value from the database:</span></span>

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

<span data-ttu-id="9e2d8-289">若要執行事件的覆寫 `ValidatePrincipal` ，請在衍生自的類別中撰寫具有下列簽章的方法 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-289">To implement an override for the `ValidatePrincipal` event, write a method with the following signature in a class that derives from <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:</span></span>

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

<span data-ttu-id="9e2d8-290">以下是的範例執行 `CookieAuthenticationEvents` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-290">The following is an example implementation of `CookieAuthenticationEvents`:</span></span>

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

<span data-ttu-id="9e2d8-291">cookie在方法的服務註冊期間註冊事件實例 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-291">Register the events instance during cookie service registration in the `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="9e2d8-292">為您的類別提供[限定範圍的服務註冊](xref:fundamentals/dependency-injection#service-lifetimes) `CustomCookieAuthenticationEvents` ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-292">Provide a [scoped service registration](xref:fundamentals/dependency-injection#service-lifetimes) for your `CustomCookieAuthenticationEvents` class:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

<span data-ttu-id="9e2d8-293">假設使用者的名稱已更新 &mdash; 為不會以任何方式影響安全性的決策。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-293">Consider a situation in which the user's name is updated&mdash;a decision that doesn't affect security in any way.</span></span> <span data-ttu-id="9e2d8-294">如果您想要以非破壞性的更新使用者主體，請呼叫， `context.ReplacePrincipal` 並將 `context.ShouldRenew` 屬性設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-294">If you want to non-destructively update the user principal, call `context.ReplacePrincipal` and set the `context.ShouldRenew` property to `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="9e2d8-295">這裡所述的方法會在每個要求上觸發。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-295">The approach described here is triggered on every request.</span></span> <span data-ttu-id="9e2d8-296">cookie針對每個要求的所有使用者驗證驗證，可能會對應用程式造成很大的效能影響。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-296">Validating authentication cookies for all users on every request can result in a large performance penalty for the app.</span></span>

## <a name="persistent-no-loccookies"></a><span data-ttu-id="9e2d8-297">持續性 cookie s</span><span class="sxs-lookup"><span data-stu-id="9e2d8-297">Persistent cookies</span></span>

<span data-ttu-id="9e2d8-298">您可能想要在 cookie 瀏覽器會話之間保存。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-298">You may want the cookie to persist across browser sessions.</span></span> <span data-ttu-id="9e2d8-299">只有在登入或類似的機制上有 [記住我] 核取方塊的明確使用者同意，才能啟用此持續性。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-299">This persistence should only be enabled with explicit user consent with a "Remember Me" check box on sign in or a similar mechanism.</span></span> 

<span data-ttu-id="9e2d8-300">下列程式碼片段會建立身分識別，並 cookie 透過瀏覽器結束來對應該不受。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-300">The following code snippet creates an identity and corresponding cookie that survives through browser closures.</span></span> <span data-ttu-id="9e2d8-301">系統會接受先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-301">Any sliding expiration settings previously configured are honored.</span></span> <span data-ttu-id="9e2d8-302">如果在 cookie 瀏覽器關閉時過期，瀏覽器會在 cookie 重新開機後清除。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-302">If the cookie expires while the browser is closed, the browser clears the cookie once it's restarted.</span></span>

<span data-ttu-id="9e2d8-303"><xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent>在中，將設為 `true` <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties> ：</span><span class="sxs-lookup"><span data-stu-id="9e2d8-303">Set <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> to `true` in <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-no-loccookie-expiration"></a><span data-ttu-id="9e2d8-304">絕對 cookie 到期</span><span class="sxs-lookup"><span data-stu-id="9e2d8-304">Absolute cookie expiration</span></span>

<span data-ttu-id="9e2d8-305">絕對到期時間可以使用來設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc> 。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-305">An absolute expiration time can be set with <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>.</span></span> <span data-ttu-id="9e2d8-306">若要建立持續性 cookie ， `IsPersistent` 也必須設定。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-306">To create a persistent cookie, `IsPersistent` must also be set.</span></span> <span data-ttu-id="9e2d8-307">否則， cookie 會使用以會話為基礎的存留期來建立，而且可能會在它所保留的驗證票證之前或之後過期。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-307">Otherwise, the cookie is created with a session-based lifetime and could expire either before or after the authentication ticket that it holds.</span></span> <span data-ttu-id="9e2d8-308">當 `ExpiresUtc` 設定時，它會覆寫 <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> 選項的值 <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions> （如果已設定的話）。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-308">When `ExpiresUtc` is set, it overrides the value of the <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> option of <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>, if set.</span></span>

<span data-ttu-id="9e2d8-309">下列程式碼片段會建立持續20分鐘的身分識別和對應 cookie 的。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-309">The following code snippet creates an identity and corresponding cookie that lasts for 20 minutes.</span></span> <span data-ttu-id="9e2d8-310">這會忽略先前設定的任何滑動到期設定。</span><span class="sxs-lookup"><span data-stu-id="9e2d8-310">This ignores any sliding expiration settings previously configured.</span></span>

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9e2d8-311">其他資源</span><span class="sxs-lookup"><span data-stu-id="9e2d8-311">Additional resources</span></span>

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [<span data-ttu-id="9e2d8-312">以原則為基礎的角色檢查</span><span class="sxs-lookup"><span data-stu-id="9e2d8-312">Policy-based role checks</span></span>](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
