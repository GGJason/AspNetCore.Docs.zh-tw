---
title: ASP.NET Core 中的多重要素驗證
author: damienbod
description: 瞭解如何在 ASP.NET Core 應用程式中 (MFA) 設定多重要素驗證。
monikerRange: '>= aspnetcore-3.1'
ms.author: rick-anderson
ms.custom: mvc
ms.date: 03/17/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/mfa
ms.openlocfilehash: 1ab6e5802e177aeaf77584838feea09a7ff79db7
ms.sourcegitcommit: 84150702757cf7a7b839485382420e8db8e92b9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/05/2020
ms.locfileid: "87819174"
---
# <a name="multi-factor-authentication-in-aspnet-core"></a>ASP.NET Core 中的多重要素驗證

依[Damien Bowden](https://github.com/damienbod)

多重要素驗證 (MFA) 是在登入事件期間要求使用者進行其他形式識別的程式。 此提示可能是從行動電話輸入代碼、使用 FIDO2 索引鍵，或提供指紋掃描。 當您需要第二種形式的驗證時，會增強安全性。 攻擊者無法輕鬆取得或複製額外的因素。

本文涵蓋下列領域：

* 什麼是 MFA 和建議的 MFA 流程
* 使用 ASP.NET Core 設定管理頁面的 MFAIdentity
* 將 MFA 登入需求傳送至 OpenID Connect 伺服器
* 強制 ASP.NET Core OpenID Connect 用戶端要求 MFA

## <a name="mfa-2fa"></a>MFA、2FA

MFA 需要至少兩個或更多類型的證明，例如您知道的專案、您擁有的內容，或使用者要驗證的生物識別驗證。

 (2FA) 的雙因素驗證就像是 MFA 的子集，但不同之處在于 MFA 可能需要兩個或更多的因素來證明身分識別。

### <a name="mfa-totp-time-based-one-time-password-algorithm"></a>MFA TOTP (以時間為基礎的一次性密碼演算法) 

使用 TOTP 的 MFA 是使用 ASP.NET Core 的支援實施 Identity 。 這可與任何相容的驗證器應用程式搭配使用，包括：

* Microsoft Authenticator 應用程式
* Google 驗證器應用程式

如需執行詳細資料，請參閱下列連結：

[在 ASP.NET Core 中為 TOTP 驗證器應用程式啟用 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)

### <a name="mfa-fido2-or-passwordless"></a>MFA FIDO2 或無密碼

FIDO2 目前：

* 達到 MFA 的最安全方式。
* 唯一可保護網路釣魚攻擊的 MFA 流程。

目前，ASP.NET Core 不直接支援 FIDO2。 FIDO2 可用於 MFA 或無密碼流程。

Azure Active Directory 提供 FIDO2 和無密碼流程的支援。 如需詳細資訊，請參閱[Azure Active Directory 的無密碼 authentication 選項](/azure/active-directory/authentication/concept-authentication-passwordless)。

### <a name="mfa-sms"></a>MFA SMS

與密碼驗證相較之下，使用 SMS 的 MFA 會大幅提高安全性， (單一要素) 。 不過，不再建議使用 SMS 做為第二個因素。 此類型的執行有太多已知的攻擊媒介。

[NIST 指導方針](https://pages.nist.gov/800-63-3/sp800-63b.html)

## <a name="configure-mfa-for-administration-pages-using-aspnet-core-no-locidentity"></a>使用 ASP.NET Core 設定管理頁面的 MFAIdentity

MFA 可能會強制使用者存取 ASP.NET Core 應用程式內的機密頁面 Identity 。 這對於不同身分識別有不同存取層級的應用程式很有用。 例如，使用者可能可以使用密碼登入來查看設定檔資料，但系統管理員必須使用 MFA 來存取管理頁面。

### <a name="extend-the-login-with-an-mfa-claim"></a>使用 MFA 宣告擴充登入

示範程式碼是使用 ASP.NET Core 搭配 Identity 和頁面進行設定 Razor 。 `AddIdentity`會使用方法，而不是 `AddDefaultIdentity` 一個，因此 `IUserClaimsPrincipalFactory` 可在成功登入後，使用此方法將宣告新增至身分識別。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlite(
            Configuration.GetConnectionString("DefaultConnection")));
    
    services.AddIdentity<IdentityUser, IdentityRole>(
            options => options.SignIn.RequireConfirmedAccount = false)
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

    services.AddSingleton<IEmailSender, EmailSender>();
    services.AddScoped<IUserClaimsPrincipalFactory<IdentityUser>, 
        AdditionalUserClaimsPrincipalFactory>();

    services.AddAuthorization(options =>
        options.AddPolicy("TwoFactorEnabled",
            x => x.RequireClaim("amr", "mfa")));

    services.AddRazorPages();
}
```

`AdditionalUserClaimsPrincipalFactory`只有在成功登入之後，類別才會將 `amr` 宣告新增至使用者宣告。 宣告的值會從資料庫讀取。 此宣告會加入此處，因為如果身分識別已使用 MFA 登入，使用者應該只存取較高的受保護的檢視。 如果直接從資料庫讀取資料庫檢視，而不是使用宣告，則在啟用 MFA 之後，即可直接存取此視圖，而不需要 MFA。

```csharp
using Microsoft.AspNetCore.Identity;
using Microsoft.Extensions.Options;
using System.Collections.Generic;
using System.Security.Claims;
using System.Threading.Tasks;

namespace IdentityStandaloneMfa
{
    public class AdditionalUserClaimsPrincipalFactory : 
        UserClaimsPrincipalFactory<IdentityUser, IdentityRole>
    {
        public AdditionalUserClaimsPrincipalFactory( 
            UserManager<IdentityUser> userManager,
            RoleManager<IdentityRole> roleManager, 
            IOptions<IdentityOptions> optionsAccessor) 
            : base(userManager, roleManager, optionsAccessor)
        {
        }

        public async override Task<ClaimsPrincipal> CreateAsync(IdentityUser user)
        {
            var principal = await base.CreateAsync(user);
            var identity = (ClaimsIdentity)principal.Identity;

            var claims = new List<Claim>();

            if (user.TwoFactorEnabled)
            {
                claims.Add(new Claim("amr", "mfa"));
            }
            else
            {
                claims.Add(new Claim("amr", "pwd"));
            }

            identity.AddClaims(claims);
            return principal;
        }
    }
}
```

由於 Identity 服務安裝程式在類別中有所變更 `Startup` ，因此需要更新的版面配置 Identity 。 將 Identity 頁面 Scaffold 至應用程式。 在* Identity /Account/Manage/_Layout. cshtml*檔案中定義版面配置。

```cshtml
@{
    Layout = "/Pages/Shared/_Layout.cshtml";
}
```

此外，也請從頁面中指派所有 [管理] 頁面的版面配置 Identity ：

```cshtml
@{
    Layout = "_Layout.cshtml";
}
```

### <a name="validate-the-mfa-requirement-in-the-administration-page"></a>驗證 [系統管理] 頁面中的 MFA 需求

系統管理 Razor 頁面會驗證使用者是否已使用 MFA 登入。 在 `OnGet` 方法中，會使用身分識別來存取使用者宣告。 `amr`會檢查宣告的值是否為 `mfa` 。 如果身分識別缺少此宣告或為，則頁面會重新 `false` 導向至 [啟用 MFA] 頁面。 這是可能的，因為使用者已登入，但沒有 MFA。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;

namespace IdentityStandaloneMfa
{
    public class AdminModel : PageModel
    {
        public IActionResult OnGet()
        {
            var claimTwoFactorEnabled = 
                User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (claimTwoFactorEnabled != null && 
                "mfa".Equals(claimTwoFactorEnabled.Value))
            {
                // You logged in with MFA, do the administrative stuff
            }
            else
            {
                return Redirect(
                    "/Identity/Account/Manage/TwoFactorAuthentication");
            }

            return Page();
        }
    }
}
```

### <a name="ui-logic-to-toggle-user-login-information"></a>切換使用者登入資訊的 UI 邏輯

已在啟動時新增授權原則。 原則需要 `amr` 具有值的宣告 `mfa` 。

```csharp
services.AddAuthorization(options =>
    options.AddPolicy("TwoFactorEnabled",
        x => x.RequireClaim("amr", "mfa")));
```

此原則可接著在視圖中用 `_Layout` 來顯示或隱藏 [**管理**] 功能表，並出現警告：

```cshtml
@using Microsoft.AspNetCore.Authorization
@using Microsoft.AspNetCore.Identity
@inject SignInManager<IdentityUser> SignInManager
@inject UserManager<IdentityUser> UserManager
@inject IAuthorizationService AuthorizationService
```

如果身分識別已使用 MFA 登入，則會顯示 [**管理**] 功能表，且不會出現工具提示警告。 當使用者登入但未進行 MFA 時，系統會顯示 [**管理員 (未啟用]) **功能表會連同工具提示，通知使用者 (說明警告) 。

```cshtml
@if (SignInManager.IsSignedIn(User))
{
    @if ((AuthorizationService.AuthorizeAsync(User, "TwoFactorEnabled")).Result.Succeeded)
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin">Admin</a>
        </li>
    }
    else
    {
        <li class="nav-item">
            <a class="nav-link text-dark" asp-area="" asp-page="/Admin" 
               id="tooltip-demo"  
               data-toggle="tooltip" 
               data-placement="bottom" 
               title="MFA is NOT enabled. This is required for the Admin Page. If you have activated MFA, then logout, login again.">
                Admin (Not Enabled)
            </a>
        </li>
    }
}
```

如果使用者在沒有 MFA 的情況下登入，則會顯示警告：

![系統管理員 MFA 驗證](mfa/_static/identitystandalonemfa_01.png)

當您按一下 [系統**管理員**] 連結時，會將使用者重新導向至 MFA 啟用視圖：

![系統管理員啟用 MFA 驗證](mfa/_static/identitystandalonemfa_02.png)

## <a name="send-mfa-sign-in-requirement-to-openid-connect-server"></a>將 MFA 登入需求傳送至 OpenID Connect 伺服器 

`acr_values`參數可以用來 `mfa` 在驗證要求中，將必要的值從用戶端傳遞至伺服器。

> [!NOTE]
> 必須 `acr_values` 在 OpenID connect 伺服器上處理參數，才能讓此作業正常執行。

### <a name="openid-connect-aspnet-core-client"></a>OpenID Connect ASP.NET Core 用戶端

ASP.NET Core Razor Pages OpenID connect 用戶端應用程式會使用 `AddOpenIdConnect` 方法來登入 OpenID connect 伺服器。 `acr_values`參數是以 `mfa` 值設定，並與驗證要求一起傳送。 `OpenIdConnectEvents`是用來加入這個。

如需建議的 `acr_values` 參數值，請參閱[驗證方法參考值](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08)。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "<OpenID Connect server URL>";
        options.RequireHttpsMetadata = true;
        options.ClientId = "<OpenID Connect client ID>";
        options.ClientSecret = "<>";
        // Code with PKCE can also be used here
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
        options.Events = new OpenIdConnectEvents
        {
            OnRedirectToIdentityProvider = context =>
            {
                context.ProtocolMessage.SetParameter("acr_values", "mfa");
                return Task.FromResult(0);
            }
        };
    });
```

### <a name="example-openid-connect-no-locidentityserver-4-server-with-aspnet-core-no-locidentity"></a>Identity使用 ASP.NET Core 的 OpenID connect 伺服器4伺服器範例Identity

在使用 MVC views 的 ASP.NET Core 所執行的 OpenID Connect 伺服器上 Identity ，會建立名為*ErrorEnable2FA*的新視圖。 檢視：

* 如果來自 Identity 需要 MFA 但使用者未在中啟用此功能的應用程式，則會顯示 Identity 。
* 通知使用者並新增連結來啟動此。

```cshtml
@{
    ViewData["Title"] = "ErrorEnable2FA";
}

<h1>The client application requires you to have MFA enabled. Enable this, try login again.</h1>

<br />

You can enable MFA to login here:

<br />

<a asp-controller="Manage" asp-action="TwoFactorAuthentication">Enable MFA</a>
```

在 `Login` 方法中， `IIdentityServerInteractionService` 介面的實作為 `_interaction` 用來存取 OpenID connect 要求參數。 `acr_values`參數是使用屬性來存取 `AcrValues` 。 當用戶端傳送此 `mfa` 設定時，就可以進行檢查。

如果需要 MFA，而且 ASP.NET Core 中的使用者 Identity 已啟用 mfa，則會繼續登入。 當使用者未啟用 MFA 時，使用者會被重新導向至自訂視圖*ErrorEnable2FA*。 然後 ASP.NET Core Identity 在中登入使用者。

```csharp
//
// POST: /Account/Login
[HttpPost]
[AllowAnonymous]
[ValidateAntiForgeryToken]
public async Task<IActionResult> Login(LoginInputModel model)
{
    var returnUrl = model.ReturnUrl;
    var context = 
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa = 
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    var user = await _userManager.FindByNameAsync(model.Email);
    if (user != null && !user.TwoFactorEnabled && requires2Fa)
    {
        return RedirectToAction(nameof(ErrorEnable2FA));
    }

    // code omitted for brevity
```

`ExternalLoginCallback`方法的運作方式類似本機 Identity 登入。 `AcrValues`會檢查屬性的 `mfa` 值。 如果 `mfa` 值存在，則會在登入完成之前強制 MFA (例如，重新導向至 `ErrorEnable2FA` view) 。

```csharp
//
// GET: /Account/ExternalLoginCallback
[HttpGet]
[AllowAnonymous]
public async Task<IActionResult> ExternalLoginCallback(
    string returnUrl = null,
    string remoteError = null)
{
    var context =
        await _interaction.GetAuthorizationContextAsync(returnUrl);
    var requires2Fa =
        context?.AcrValues.Count(t => t.Contains("mfa")) >= 1;

    if (remoteError != null)
    {
        ModelState.AddModelError(
            string.Empty,
            _sharedLocalizer["EXTERNAL_PROVIDER_ERROR", 
            remoteError]);
        return View(nameof(Login));
    }
    var info = await _signInManager.GetExternalLoginInfoAsync();

    if (info == null)
    {
        return RedirectToAction(nameof(Login));
    }

    var email = info.Principal.FindFirstValue(ClaimTypes.Email);

    if (!string.IsNullOrEmpty(email))
    {
        var user = await _userManager.FindByNameAsync(email);
        if (user != null && !user.TwoFactorEnabled && requires2Fa)
        {
            return RedirectToAction(nameof(ErrorEnable2FA));
        }
    }

    // Sign in the user with this external login provider if the user already has a login.
    var result = await _signInManager
        .ExternalLoginSignInAsync(
            info.LoginProvider, 
            info.ProviderKey, 
            isPersistent: 
            false);

    // code omitted for brevity
```

如果使用者已登入，用戶端應用程式：

* 仍然會驗證宣告 `amr` 。
* 可以使用 ASP.NET Core view 的連結來設定 MFA Identity 。

![acr_values-1](mfa/_static/acr_values-1.png)

## <a name="force-aspnet-core-openid-connect-client-to-require-mfa"></a>強制 ASP.NET Core OpenID Connect 用戶端要求 MFA

這個範例示範 Razor 使用 OpenID connect 登入的 ASP.NET Core 頁面應用程式，是否可以要求使用者使用 MFA 進行驗證。

若要驗證 MFA 需求， `IAuthorizationRequirement` 會建立一個需求。 這會使用需要 MFA 的原則新增至頁面。

```csharp
using Microsoft.AspNetCore.Authorization;
 
namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfa : IAuthorizationRequirement{}
}
```

會實作為 `AuthorizationHandler` ，其會使用宣告 `amr` 並檢查值 `mfa` 。 `amr`會在驗證成功時傳回， `id_token` 而且可以有許多不同的值，如[驗證方法參考值](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08)規格中所定義。

傳回的值取決於身分識別驗證和 OpenID Connect 伺服器的執行方式。

會 `AuthorizationHandler` 使用 `RequireMfa` 需求並驗證宣告 `amr` 。 OpenID Connect 伺服器可以使用伺服器4搭配 ASP.NET Core 來執行 Identity Identity 。 當使用者使用 TOTP 登入時， `amr` 會傳回具有 MFA 值的宣告。 如果使用不同的 OpenID Connect 伺服器實施或不同的 MFA 類型，則宣告 `amr` 可以或具有不同的值。 程式碼也必須擴充，才能接受此程式碼。

```csharp
using Microsoft.AspNetCore.Authorization;
using System;
using System.Linq;
using System.Threading.Tasks;

namespace AspNetCoreRequireMfaOidc
{
    public class RequireMfaHandler : AuthorizationHandler<RequireMfa>
    {
        protected override Task HandleRequirementAsync(
            AuthorizationHandlerContext context, 
            RequireMfa requirement)
        {
            if (context == null)
                throw new ArgumentNullException(nameof(context));
            if (requirement == null)
                throw new ArgumentNullException(nameof(requirement));

            var amrClaim =
                context.User.Claims.FirstOrDefault(t => t.Type == "amr");

            if (amrClaim != null && amrClaim.Value == Amr.Mfa)
            {
                context.Succeed(requirement);
            }

            return Task.CompletedTask;
        }
    }
}
```

在 `Startup.ConfigureServices` 方法中， `AddOpenIdConnect` 方法是用來做為預設的挑戰配置。 用來檢查宣告的授權處理常式 `amr` 會新增至 [反轉控制] 容器。 接著會建立原則，以新增 `RequireMfa` 需求。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.ConfigureApplicationCookie(options =>
        options.Cookie.SecurePolicy =
            CookieSecurePolicy.Always);

    services.AddSingleton<IAuthorizationHandler, RequireMfaHandler>();

    services.AddAuthentication(options =>
    {
        options.DefaultScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme =
            OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.SignInScheme =
            CookieAuthenticationDefaults.AuthenticationScheme;
        options.Authority = "https://localhost:44352";
        options.RequireHttpsMetadata = true;
        options.ClientId = "AspNetCoreRequireMfaOidc";
        options.ClientSecret = "AspNetCoreRequireMfaOidcSecret";
        options.ResponseType = "code id_token";
        options.Scope.Add("profile");
        options.Scope.Add("offline_access");
        options.SaveTokens = true;
    });

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireMfa", policyIsAdminRequirement =>
        {
            policyIsAdminRequirement.Requirements.Add(new RequireMfa());
        });
    });

    services.AddRazorPages();
}
```

然後，此原則會 Razor 視需要在頁面中使用。 該原則也可以針對整個應用程式全域加入。

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.AspNetCore.Mvc.RazorPages;
using Microsoft.Extensions.Logging;

namespace AspNetCoreRequireMfaOidc.Pages
{
    [Authorize(Policy= "RequireMfa")]
    public class IndexModel : PageModel
    {
        private readonly ILogger<IndexModel> _logger;

        public IndexModel(ILogger<IndexModel> logger)
        {
            _logger = logger;
        }

        public void OnGet()
        {
        }
    }
}
```

如果使用者在沒有 MFA 的情況下進行驗證，則宣告 `amr` 可能會有 `pwd` 值。 要求不會獲得存取頁面的授權。 使用預設值時，使用者會被重新導向至 [*帳戶/AccessDenied* ] 頁面。 這種行為可以變更，或您可以在這裡執行您自己的自訂邏輯。 在此範例中，會新增連結，讓有效的使用者可以為其帳戶設定 MFA。

```cshtml
@page
@model AspNetCoreRequireMfaOidc.AccessDeniedModel
@{
    ViewData["Title"] = "AccessDenied";
    Layout = "~/Pages/Shared/_Layout.cshtml";
}

<h1>AccessDenied</h1>

You require MFA to login here

<a href="https://localhost:44352/Manage/TwoFactorAuthentication">Enable MFA</a>
```

現在，只有使用 MFA 進行驗證的使用者才能存取頁面或網站。 如果使用不同的 MFA 類型，或2FA 正常，則宣告 `amr` 將會有不同的值，而且必須正確處理。 不同的 OpenID Connect 伺服器也會針對此宣告傳回不同的值，而且可能不會遵循[驗證方法參考值](https://tools.ietf.org/html/draft-ietf-oauth-amr-values-08)規格。

在沒有 MFA 的情況下登入 (例如，只使用密碼) ：

* `amr`具有 `pwd` 值：

    ![require_mfa_oidc_02.png](mfa/_static/require_mfa_oidc_02.png)

* 拒絕存取：

    ![require_mfa_oidc_03.png](mfa/_static/require_mfa_oidc_03.png)

或者，使用 OTP 搭配下列方式登入 Identity ：

![require_mfa_oidc_01.png](mfa/_static/require_mfa_oidc_01.png)

## <a name="additional-resources"></a>其他資源

* [在 ASP.NET Core 中為 TOTP 驗證器應用程式啟用 QR 代碼產生](xref:security/authentication/identity-enable-qrcodes)
* [Azure Active Directory 的無密碼 authentication 選項](/azure/active-directory/authentication/concept-authentication-passwordless)
* [使用 .NET FIDO2 適用于 FIDO2/WebAuthn 證明和判斷提示的 .NET 程式庫](https://github.com/abergs/fido2-net-lib)
* [WebAuthn 棒](https://github.com/herrjemand/awesome-webauthn)
