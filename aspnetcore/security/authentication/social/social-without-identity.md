---
title: Facebook、Google 和外部提供者驗證（不含 ASP.NET Core Identity
author: rick-anderson
description: 說明如何使用 Facebook、Google、Twitter 等帳戶使用者驗證，而不需要 ASP.NET Core Identity 。
ms.author: riande
ms.date: 12/10/2019
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
uid: security/authentication/social/social-without-identity
ms.openlocfilehash: a91a2f2fb7873e5a672c624e9cf863ae720c8005
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634224"
---
# <a name="use-social-sign-in-provider-authentication-without-no-locaspnet-core-identity"></a>使用社交登入提供者驗證但不使用 ASP.NET Core Identity

[Kirk Larkin](https://twitter.com/serpent5)與[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

<xref:security/authentication/social/index> 說明如何讓使用者使用 OAuth 2.0 搭配外部驗證提供者的認證進行登入。 本主題中所述的方法包括 ASP.NET Core Identity 做為驗證提供者。

這個範例示範如何在不使用的 **情況下**使用外部驗證提供者 ASP.NET Core Identity 。 這適用于不需要所有功能的應用程式 ASP.NET Core Identity ，但仍需要與受信任的外部驗證提供者整合。

此範例會使用 [Google 驗證](xref:security/authentication/google-logins) 來驗證使用者。 使用 Google 驗證可將管理登入程式的許多複雜性轉移至 Google。 若要與不同的外部驗證提供者整合，請參閱下列主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他提供者](xref:security/authentication/otherlogins)

## <a name="configuration"></a>組態

在 `ConfigureServices` 方法中，使用 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 、 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> 和方法來設定應用程式的驗證配置 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> ：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet1)]

用來 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 設定應用程式的呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme> 。 `DefaultScheme`是下列驗證擴充方法所使用的預設配置 `HttpContext` ：

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

將應用程式設定 `DefaultScheme` 為[ Cookie AuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ( " Cookie s" ) 會將應用程式設定為使用 Cookie 做為這些擴充方法的預設配置。 將應用程式設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> 為 [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ( "Google" ) 會將應用程式設定為使用 Google 做為呼叫的預設配置 `ChallengeAsync` 。 `DefaultChallengeScheme` 覆寫 `DefaultScheme` 。 <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>如需在設定時覆寫的其他屬性，請參閱 `DefaultScheme` 。

在中 `Startup.Configure` ，呼叫和之間的呼叫 `UseAuthentication` `UseAuthorization` `UseRouting` 和 `UseEndpoints` 。 這會設定 `HttpContext.User` 屬性，並執行要求的授權中介軟體：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Startup.cs?name=snippet2&highlight=3-4)]

若要深入瞭解驗證配置，請參閱 [驗證概念](xref:security/authentication/index#authentication-concepts)。 若要深入瞭解 cookie 驗證，請參閱 <xref:security/authentication/cookie> 。

## <a name="apply-authorization"></a>套用授權

將 `AuthorizeAttribute` 屬性套用至控制器、動作或頁面，以測試應用程式的驗證設定。 下列程式碼會將 *隱私權* 頁面的存取限制為已驗證的使用者：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>登出

若要登出目前的使用者並刪除其 cookie ，請呼叫 [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)。 下列程式碼會將 `Logout` 頁面處理常式新增至 [ *索引* ] 頁面：

[!code-csharp[](social-without-identity/samples_snapshot/3.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

請注意，的呼叫不 `SignOutAsync` 會指定驗證配置。 的應用程式 `DefaultScheme` `CookieAuthenticationDefaults.AuthenticationScheme` 會用來做為回復。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

<xref:security/authentication/social/index> 說明如何讓使用者使用 OAuth 2.0 搭配外部驗證提供者的認證進行登入。 本主題中所述的方法包括 ASP.NET Core Identity 做為驗證提供者。

這個範例示範如何在不使用的 **情況下**使用外部驗證提供者 ASP.NET Core Identity 。 這適用于不需要所有功能的應用程式 ASP.NET Core Identity ，但仍需要與受信任的外部驗證提供者整合。

此範例會使用 [Google 驗證](xref:security/authentication/google-logins) 來驗證使用者。 使用 Google 驗證可將管理登入程式的許多複雜性轉移至 Google。 若要與不同的外部驗證提供者整合，請參閱下列主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他提供者](xref:security/authentication/otherlogins)

## <a name="configuration"></a>組態

在 `ConfigureServices` 方法中，使用 `AddAuthentication` 、 `AddCookie` 和方法來設定應用程式的驗證配置 `AddGoogle` ：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet1)]

對 [AddAuthentication](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) 的呼叫會設定應用程式的 [DefaultScheme](xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultScheme)。 `DefaultScheme`是下列驗證擴充方法所使用的預設配置 `HttpContext` ：

* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.AuthenticateAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ChallengeAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.ForbidAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>
* <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>

將應用程式設定 `DefaultScheme` 為[ Cookie AuthenticationDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ( " Cookie s" ) 會將應用程式設定為使用 Cookie 做為這些擴充方法的預設配置。 將應用程式設定 <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions.DefaultChallengeScheme> 為 [GoogleDefaults. AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Google.GoogleDefaults.AuthenticationScheme) ( "Google" ) 會將應用程式設定為使用 Google 做為呼叫的預設配置 `ChallengeAsync` 。 `DefaultChallengeScheme` 覆寫 `DefaultScheme` 。 <xref:Microsoft.AspNetCore.Authentication.AuthenticationOptions>如需在設定時覆寫的其他屬性，請參閱 `DefaultScheme` 。

在 `Configure` 方法中，呼叫 `UseAuthentication` 方法以叫用設定屬性的驗證中介軟體 `HttpContext.User` 。 呼叫 `UseAuthentication` 或之前，請先呼叫方法 `UseMvcWithDefaultRoute` `UseMvc` ：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Startup.cs?name=snippet2)]

若要深入瞭解驗證配置，請參閱 [驗證概念](xref:security/authentication/index#authentication-concepts)。 若要深入瞭解 cookie 驗證，請參閱 <xref:security/authentication/cookie> 。

## <a name="apply-authorization"></a>套用授權

將 `AuthorizeAttribute` 屬性套用至控制器、動作或頁面，以測試應用程式的驗證設定。 下列程式碼會將 *隱私權* 頁面的存取限制為已驗證的使用者：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Privacy.cshtml.cs?name=snippet&highlight=1)]

## <a name="sign-out"></a>登出

若要登出目前的使用者並刪除其 cookie ，請呼叫 [SignOutAsync](xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*)。 下列程式碼會將 `Logout` 頁面處理常式新增至 [ *索引* ] 頁面：

[!code-csharp[](social-without-identity/samples_snapshot/2.x/Pages/Index.cshtml.cs?name=snippet&highlight=3-7)]

請注意，的呼叫不 `SignOutAsync` 會指定驗證配置。 的應用程式 `DefaultScheme` `CookieAuthenticationDefaults.AuthenticationScheme` 會用來做為回復。

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/simple>
* <xref:security/authentication/social/additional-claims>

::: moniker-end
