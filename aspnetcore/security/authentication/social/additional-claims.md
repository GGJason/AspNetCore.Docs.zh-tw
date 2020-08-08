---
title: 在 ASP.NET Core 中保存外部提供者的其他宣告和權杖
author: rick-anderson
description: 瞭解如何從外部提供者建立額外的宣告和權杖。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
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
uid: security/authentication/social/additional-claims
ms.openlocfilehash: f7a440a13891cd51226cad12924cfc65684632ea
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88020180"
---
# <a name="persist-additional-claims-and-tokens-from-external-providers-in-aspnet-core"></a>在 ASP.NET Core 中保存外部提供者的其他宣告和權杖

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。 每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([如何下載](xref:index#how-to-download-a-sample)) 

## <a name="prerequisites"></a>必要條件

決定要在應用程式中支援的外部驗證提供者。 針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。 如需詳細資訊，請參閱<xref:security/authentication/social/index>。 範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。

## <a name="set-the-client-id-and-client-secret"></a>設定用戶端識別碼和用戶端秘密

OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。 當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。 應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。 如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Google 驗證](xref:security/authentication/google-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他驗證提供者](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>建立驗證範圍

藉由指定，指定要從提供者抓取的許可權清單 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*> 。 下表顯示一般外部提供者的驗證範圍。

| 提供者  | 影響範圍                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

在範例應用程式中， `userinfo.profile` 當 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 在上呼叫時，架構會自動新增 Google 的範圍 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> 。 如果應用程式需要額外的範圍，請將它們新增至選項。 在下列範例中， `https://www.googleapis.com/auth/user.birthday.read` 會新增 Google 領域來取得使用者的生日：

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>對應使用者資料索引鍵和建立宣告

在提供者的選項中，針對 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> 外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定或，以讓應用程式識別在登入時讀取。 如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes> 。

範例應用程式會 `urn:google:locale` `urn:google:picture` 從 `locale` `picture` Google 使用者資料中的和金鑰建立地區設定 () 和圖片 () 宣告：

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

在中 `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync` ， <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) 使用登入應用程式 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> 。 在登入程式期間， <xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 `ApplicationUser` 可從取得之使用者資料的宣告 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> 。

在範例應用程式中， `OnPostConfirmationAsync` (*Account/ExternalLogin*) 會 `urn:google:locale` 針對已登入的) 和圖片 () 宣告，建立地區設定 (`urn:google:picture` `ApplicationUser` ，包括 <xref:System.Security.Claims.ClaimTypes.GivenName> 下列：

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

根據預設，使用者的宣告會儲存在驗證中 cookie 。 如果驗證 cookie 太大，可能會導致應用程式失敗，因為：

* 瀏覽器偵測到 cookie 標頭太長。
* 要求的整體大小太大。

如果需要大量的使用者資料來處理使用者要求：

* 將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。
* 使用 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> Cookie 驗證中介軟體的自訂 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> 來儲存要求之間的身分識別。 在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。

## <a name="save-the-access-token"></a>儲存存取權杖

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>定義在成功授權之後，是否應將存取和重新整理權杖儲存在中 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> 。 `SaveTokens`預設會設定為， `false` 以減少最終驗證的大小 cookie 。

範例應用程式會將的值設定 `SaveTokens` 為 `true` 中的 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> ：

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

`OnPostConfirmationAsync`執行時，請從的外部提供者，將存取權杖儲存 ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 。 `ApplicationUser` `AuthenticationProperties`

範例應用程式會將存取權杖儲存在 `OnPostConfirmationAsync` (新的使用者註冊) ，並 `OnGetCallbackAsync` (先前在*Account/ExternalLogin*中註冊的使用者) ：

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>如何新增額外的自訂權杖

為了示範如何新增會儲存為一部分的自訂權杖 `SaveTokens` ，範例應用程式會在的 AuthenticationToken.Name 中加入 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> 具有目前的 <xref:System.DateTime> [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) `TicketCreated` ：

[!code-csharp[](additional-claims/samples/3.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a>建立和新增宣告

架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。 如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。

使用者可以自訂動作，方法是從衍生 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 並執行抽象 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法。

如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。

## <a name="removal-of-claim-actions-and-claims"></a>移除宣告動作和宣告

[ClaimActionCollection 移除 (字串) ](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)從集合中移除指定的所有宣告動作 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。 [ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection，String) ](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除指定的宣告 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>主要用於[OpenID connect (OIDC) ](/azure/active-directory/develop/v2-protocols-oidc)移除通訊協定產生的宣告。

## <a name="sample-app-output"></a>範例應用程式輸出

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 應用程式可以從外部驗證提供者（例如 Facebook、Google、Microsoft 和 Twitter）建立額外的宣告和權杖。 每個提供者會在其平臺上顯示使用者的不同資訊，但接收和將使用者資料轉換成其他宣告的模式則相同。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/social/additional-claims/samples) ([如何下載](xref:index#how-to-download-a-sample)) 

## <a name="prerequisites"></a>必要條件

決定要在應用程式中支援的外部驗證提供者。 針對每個提供者，註冊應用程式，並取得用戶端識別碼和用戶端密碼。 如需詳細資訊，請參閱<xref:security/authentication/social/index>。 範例應用程式會使用[Google 驗證提供者](xref:security/authentication/google-logins)。

## <a name="set-the-client-id-and-client-secret"></a>設定用戶端識別碼和用戶端秘密

OAuth 驗證提供者會使用用戶端識別碼和用戶端密碼，與應用程式建立信任關係。 當應用程式向提供者註冊時，外部驗證提供者會為應用程式建立用戶端識別碼和用戶端秘密值。 應用程式所使用的每個外部提供者都必須以提供者的用戶端識別碼和用戶端密碼獨立設定。 如需詳細資訊，請參閱適用于您案例的外部驗證提供者主題：

* [Facebook 驗證](xref:security/authentication/facebook-logins)
* [Google 驗證](xref:security/authentication/google-logins)
* [Microsoft 驗證](xref:security/authentication/microsoft-logins)
* [Twitter 驗證](xref:security/authentication/twitter-logins)
* [其他驗證提供者](xref:security/authentication/otherlogins)
* [OpenIdConnect](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2)

範例應用程式會使用 Google 提供的用戶端識別碼和用戶端密碼來設定 Google 驗證提供者：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=4,9)]

## <a name="establish-the-authentication-scope"></a>建立驗證範圍

藉由指定，指定要從提供者抓取的許可權清單 <xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthOptions.Scope*> 。 下表顯示一般外部提供者的驗證範圍。

| 提供者  | 影響範圍                                                            |
| --------- | ---------------------------------------------------------------- |
| Facebook  | `https://www.facebook.com/dialog/oauth`                          |
| Google    | `https://www.googleapis.com/auth/userinfo.profile`               |
| Microsoft | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| Twitter   | `https://api.twitter.com/oauth/authenticate`                     |

在範例應用程式中， `userinfo.profile` 當 <xref:Microsoft.Extensions.DependencyInjection.GoogleExtensions.AddGoogle*> 在上呼叫時，架構會自動新增 Google 的範圍 <xref:Microsoft.AspNetCore.Authentication.AuthenticationBuilder> 。 如果應用程式需要額外的範圍，請將它們新增至選項。 在下列範例中， `https://www.googleapis.com/auth/user.birthday.read` 會新增 Google 領域來取得使用者的生日：

```csharp
options.Scope.Add("https://www.googleapis.com/auth/user.birthday.read");
```

## <a name="map-user-data-keys-and-create-claims"></a>對應使用者資料索引鍵和建立宣告

在提供者的選項中，針對 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonKey*> <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.MapJsonSubKey*> 外部提供者的 JSON 使用者資料中的每個索引鍵/子機碼指定或，以讓應用程式識別在登入時讀取。 如需宣告類型的詳細資訊，請參閱 <xref:System.Security.Claims.ClaimTypes> 。

範例應用程式會 `urn:google:locale` `urn:google:picture` 從 `locale` `picture` Google 使用者資料中的和金鑰建立地區設定 () 和圖片 () 宣告：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=13-14)]

在中 `Microsoft.AspNetCore.Identity.UI.Pages.Account.Internal.ExternalLoginModel.OnPostConfirmationAsync` ， <xref:Microsoft.AspNetCore.Identity.IdentityUser> (`ApplicationUser`) 使用登入應用程式 <xref:Microsoft.AspNetCore.Identity.SignInManager%601.SignInAsync*> 。 在登入程式期間， <xref:Microsoft.AspNetCore.Identity.UserManager%601> 可以儲存 `ApplicationUser` 可從取得之使用者資料的宣告 <xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.Principal*> 。

在範例應用程式中， `OnPostConfirmationAsync` (*Account/ExternalLogin*) 會 `urn:google:locale` 針對已登入的) 和圖片 () 宣告，建立地區設定 (`urn:google:picture` `ApplicationUser` ，包括 <xref:System.Security.Claims.ClaimTypes.GivenName> 下列：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=35-51)]

根據預設，使用者的宣告會儲存在驗證中 cookie 。 如果驗證 cookie 太大，可能會導致應用程式失敗，因為：

* 瀏覽器偵測到 cookie 標頭太長。
* 要求的整體大小太大。

如果需要大量的使用者資料來處理使用者要求：

* 將要求處理的使用者宣告數目和大小限制為僅限應用程式所需的內容。
* 使用 <xref:Microsoft.AspNetCore.Authentication.Cookies.ITicketStore> Cookie 驗證中介軟體的自訂 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.SessionStore> 來儲存要求之間的身分識別。 在伺服器上保留大量的身分識別資訊，同時只將小型會話識別碼金鑰傳送給用戶端。

## <a name="save-the-access-token"></a>儲存存取權杖

<xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.SaveTokens*>定義在成功授權之後，是否應將存取和重新整理權杖儲存在中 <xref:Microsoft.AspNetCore.Http.Authentication.AuthenticationProperties> 。 `SaveTokens`預設會設定為， `false` 以減少最終驗證的大小 cookie 。

範例應用程式會將的值設定 `SaveTokens` 為 `true` 中的 <xref:Microsoft.AspNetCore.Authentication.Google.GoogleOptions> ：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=15)]

`OnPostConfirmationAsync`執行時，請從的外部提供者，將存取權杖儲存 ([ExternalLoginInfo. AuthenticationTokens](xref:Microsoft.AspNetCore.Identity.ExternalLoginInfo.AuthenticationTokens*)) 。 `ApplicationUser` `AuthenticationProperties`

範例應用程式會將存取權杖儲存在 `OnPostConfirmationAsync` (新的使用者註冊) ，並 `OnGetCallbackAsync` (先前在*Account/ExternalLogin*中註冊的使用者) ：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Areas/Identity/Pages/Account/ExternalLogin.cshtml.cs?name=snippet_OnPostConfirmationAsync&highlight=54-56)]

## <a name="how-to-add-additional-custom-tokens"></a>如何新增額外的自訂權杖

為了示範如何新增會儲存為一部分的自訂權杖 `SaveTokens` ，範例應用程式會在的 AuthenticationToken.Name 中加入 <xref:Microsoft.AspNetCore.Authentication.AuthenticationToken> 具有目前的 <xref:System.DateTime> [AuthenticationToken.Name](xref:Microsoft.AspNetCore.Authentication.AuthenticationToken.Name*) `TicketCreated` ：

[!code-csharp[](additional-claims/samples/2.x/ClaimsSample/Startup.cs?name=snippet_AddGoogle&highlight=17-30)]

## <a name="creating-and-adding-claims"></a>建立和新增宣告

架構會提供一般動作和擴充方法，以便建立和加入集合的宣告。 如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions> 和 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionUniqueExtensions>。

使用者可以自訂動作，方法是從衍生 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction> 並執行抽象 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.Run*> 方法。

如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Authentication.OAuth.Claims>。

## <a name="removal-of-claim-actions-and-claims"></a>移除宣告動作和宣告

[ClaimActionCollection 移除 (字串) ](xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimActionCollection.Remove*)從集合中移除指定的所有宣告動作 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。 [ClaimActionCollectionMapExtensions. DeleteClaim (ClaimActionCollection，String) ](xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*)會從身分識別中刪除指定的宣告 <xref:Microsoft.AspNetCore.Authentication.OAuth.Claims.ClaimAction.ClaimType> 。 <xref:Microsoft.AspNetCore.Authentication.ClaimActionCollectionMapExtensions.DeleteClaim*>主要用於[OpenID connect (OIDC) ](/azure/active-directory/develop/v2-protocols-oidc)移除通訊協定產生的宣告。

## <a name="sample-app-output"></a>範例應用程式輸出

```
User Claims

http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier
    9b342344f-7aab-43c2-1ac1-ba75912ca999
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name
    someone@gmail.com
AspNet.Identity.SecurityStamp
    7D4312MOWRYYBFI1KXRPHGOSTBVWSFDE
http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname
    Judy
urn:google:locale
    en
urn:google:picture
    https://lh4.googleusercontent.com/-XXXXXX/XXXXXX/XXXXXX/XXXXXX/photo.jpg

Authentication Properties

.Token.access_token
    yc23.AlvoZqz56...1lxltXV7D-ZWP9
.Token.token_type
    Bearer
.Token.expires_at
    2019-04-11T22:14:51.0000000+00:00
.Token.TicketCreated
    4/11/2019 9:14:52 PM
.TokenNames
    access_token;token_type;expires_at;TicketCreated
.persistent
.issued
    Thu, 11 Apr 2019 20:51:06 GMT
.expires
    Thu, 25 Apr 2019 20:51:06 GMT

```

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [dotnet/AspNetCore 工程 SocialSample 應用程式](https://github.com/dotnet/AspNetCore/tree/master/src/Security/Authentication/samples/SocialSample)：連結的範例應用程式位於[Dotnet/AspNetCore GitHub 存放庫的](https://github.com/dotnet/AspNetCore) `master` 工程分支。 `master`分支包含適用于下一個版本之 ASP.NET Core 的主動式開發程式碼。 若要查看 ASP.NET Core 發行版本的範例應用程式版本，請使用 [**分支**] 下拉式清單來選取發行分支 (例如 `release/{X.Y}`) 。
