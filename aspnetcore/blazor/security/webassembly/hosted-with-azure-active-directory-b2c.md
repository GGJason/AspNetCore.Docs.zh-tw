---
title: Blazor WebAssembly使用 Azure Active Directory B2C 保護 ASP.NET Core 託管應用程式
author: guardrex
description: 瞭解如何使用 Azure Active Directory B2C 保護 ASP.NET Core Blazor WebAssembly 託管應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/02/2020
no-loc:
- appsettings.json
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
uid: blazor/security/webassembly/hosted-with-azure-active-directory-b2c
ms.openlocfilehash: 8727fa52acbcf59549c326bd5106e5dfe23c36be
ms.sourcegitcommit: fe2e3174c34bee1e425c6e52dd8f663fe52b8756
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "96174269"
---
# <a name="secure-an-aspnet-core-no-locblazor-webassembly-hosted-app-with-azure-active-directory-b2c"></a>Blazor WebAssembly使用 Azure Active Directory B2C 保護 ASP.NET Core 託管應用程式

由 [Javier Calvarro Nelson](https://github.com/javiercn) 和 [Luke Latham](https://github.com/guardrex)

本文說明如何建立使用[Azure Active Directory (AAD) B2C](/azure/active-directory-b2c/overview)進行驗證的[託管 Blazor WebAssembly 應用程式](xref:blazor/hosting-models#blazor-webassembly)。

## <a name="register-apps-in-aad-b2c-and-create-solution"></a>在 AAD B2C 中註冊應用程式並建立解決方案

### <a name="create-a-tenant"></a>建立租用戶

遵循 [教學課程：建立 Azure Active Directory B2C 租](/azure/active-directory-b2c/tutorial-create-tenant) 使用者建立 AAD B2C 租使用者中的指導方針。

記錄 AAD B2C 實例 (例如， `https://contoso.b2clogin.com/` 其中包含尾端的斜線) 。 實例是 Azure B2C 應用程式註冊的配置和主機，您可以從 Azure 入口網站中的 [**應用程式註冊**] 頁面開啟 [**端點**] 視窗來找到。

### <a name="register-a-server-api-app"></a>註冊伺服器 API 應用程式

遵循教學課程中的指導方針 [：在 Azure Active Directory B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications) 以註冊 *伺服器 API 應用* 程式的 AAD 應用程式，然後執行下列動作：

1. 在 **Azure Active Directory**  >  **應用程式註冊** 中，選取 [**新增註冊**]。
1. 提供應用程式的 **名稱** (例如 **Blazor Server AAD B2C**) 。
1. 針對 **支援的帳戶類型**，請選取 [多租使用者選項： **任何識別提供者或組織目錄中的帳戶]， (以使用使用者流程驗證使用者)**
1. 在此案例中， *伺服器 API 應用程式* 不需要重新 **導向 uri** ，因此請將下拉式清單保持設定為 [ **Web** ]，而不要輸入重新導向 uri。
1. 確認 **Permissions**  >  已選取 [將系統 **管理員同意授與 openid] 和 [offline_access] 許可權**。
1. 選取 [註冊]。

記錄下列資訊：

* *伺服器 API 應用程式* 應用程式 (用戶端) 識別碼 (例如 `41451fa7-82d9-4673-8fa5-69eff5a761fd`) 
* AAD 主要/發行者/租使用者網域 (例如 `contoso.onmicrosoft.com`) ：網域可作為已註冊應用程式之 Azure 入口網站的 [**商標**] 分頁中的 **發行者網域**。

在中 **公開 API**：

1. 選取 [新增範圍]  。
1. 選取 [儲存並繼續]  。
1. 提供 **範圍名稱** (例如 `API.Access`) 。
1. 提供系統 **管理員同意顯示名稱** (例如 `Access API`) 。
1. 提供 **管理員同意描述** (例如 `Allows the app to access server app API endpoints.`) 。
1. 確認 **狀態** 設定為 [ **已啟用**]。
1. 選取 [新增範圍]。

記錄下列資訊：

* 應用程式識別碼 URI (例如， `api://41451fa7-82d9-4673-8fa5-69eff5a761fd` 、 `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd` 或您提供的自訂值) 
* 範圍名稱 (例如， `API.Access`) 

### <a name="register-a-client-app"></a>註冊用戶端應用程式

遵循教學課程中的指導方針 [：再次在 Azure Active Directory B2C 中註冊應用程式](/azure/active-directory-b2c/tutorial-register-applications) ，為應用程式註冊 AAD 應用程式 *`Client`* ，然後執行下列動作：

::: moniker range=">= aspnetcore-5.0"

1. 在 **Azure Active Directory** > **應用程式註冊** 中，選取 [ **新增註冊**]。
1. 提供應用程式的 **名稱** (例如， **Blazor 用戶端 AAD B2C**) 。
1. 針對 **支援的帳戶類型**，請選取 [多租使用者選項： **任何識別提供者或組織目錄中的帳戶]， (以使用使用者流程驗證使用者)**
1. 將 [重新 **導向 uri** ] 下拉式清單設定為 **單一頁面應用程式 (SPA)** 並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行應用程式的預設連接埠是 5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，可在 [ *`Server`* **調試** 程式] 面板的應用程式屬性中找到應用程式隨機產生的埠。 由於應用程式目前不存在，且 IIS Express 埠未知，因此在建立應用程式之後，請返回此步驟，並更新重新導向 URI。 批註會出現在 [ [建立應用程式](#create-the-app) ] 區段中，以提醒 IIS Express 使用者更新重新導向 URI。
1. 確認 **Permissions** > 已選取 [將系統 **管理員同意授與 openid] 和 [offline_access] 許可權**。
1. 選取 [註冊]。

1. 記錄應用程式 (用戶端) 識別碼 (例如 `4369008b-21fa-427c-abaa-9b53bf58e538`) 。

在「**驗證** 平臺設定」的 > **Platform configurations** > **單一頁面應用程式中， (SPA)**：

1. 確認的重新 **導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對 **隱含授** 與，請確定 **未** 選取 **存取權杖** 和 **識別碼權杖** 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存] 按鈕。

::: moniker-end

::: moniker range="< aspnetcore-5.0"

1. 在 **Azure Active Directory** > **應用程式註冊** 中，選取 [ **新增註冊**]。
1. 提供應用程式的 **名稱** (例如， **Blazor 用戶端 AAD B2C**) 。
1. 針對 **支援的帳戶類型**，請選取 [多租使用者選項： **任何識別提供者或組織目錄中的帳戶]， (以使用使用者流程驗證使用者)**
1. 將 [重新 **導向 URI** ] 下拉式清單保持設定為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。 在 Kestrel 上執行應用程式的預設連接埠是 5001。 如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。 針對 IIS Express，可在 [ *`Server`* **調試** 程式] 面板的應用程式屬性中找到應用程式隨機產生的埠。 由於應用程式目前不存在，且 IIS Express 埠未知，因此在建立應用程式之後，請返回此步驟，並更新重新導向 URI。 批註會出現在 [ [建立應用程式](#create-the-app) ] 區段中，以提醒 IIS Express 使用者更新重新導向 URI。
1. 確認 **Permissions** > 已選取 [將系統 **管理員同意授與 openid] 和 [offline_access] 許可權**。
1. 選取 [註冊]。

記錄應用程式 (用戶端) 識別碼 (例如 `4369008b-21fa-427c-abaa-9b53bf58e538`) 。

在 [ **驗證** > **平臺** 設定] > **Web**：

1. 確認的重新 **導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。
1. 針對 **[隱含授** 與]，選取 **存取權杖** 和 **識別碼權杖** 的核取方塊。
1. 此體驗可接受應用程式的其餘預設值。
1. 選取 [儲存] 按鈕。

::: moniker-end

在 **API 許可權** 中：

1. 選取 [ **新增] 許可權** ，然後選取 [我的 **api**]。
1. 從 [**名稱**] 資料行中選取 *伺服器 API 應用程式* (例如 **Blazor Server AAD B2C**) 。
1. 開啟 **API** 清單。
1. 啟用對 API 的存取 (例如 `API.Access`) 。
1. 選取 [新增權限]。
1. 選取 [ **授與系統管理員同意 {租使用者名稱}** ] 按鈕。 選取 [是]  加以確認。

在 **Home**  >  **Azure AD B2C**  >  **使用者流程**：

[建立註冊和登入使用者流程](/azure/active-directory-b2c/tutorial-create-user-flows)

至少選取 [**應用程式宣告**  >  **顯示名稱**] 使用者屬性，以填入 `context.User.Identity.Name` `LoginDisplay` 元件 () 中的 `Shared/LoginDisplay.razor` 。

記錄為應用程式建立的註冊和登入使用者流程名稱 (例如 `B2C_1_signupsignin`) 。

### <a name="create-the-app"></a>建立應用程式

將下列命令中的預留位置取代為稍早記錄的資訊，然後在命令 shell 中執行命令：

```dotnetcli
dotnet new blazorwasm -au IndividualB2C --aad-b2c-instance "{AAD B2C INSTANCE}" --api-client-id "{SERVER API APP CLIENT ID}" --app-id-uri "{SERVER API APP ID URI}" --client-id "{CLIENT APP CLIENT ID}" --default-scope "{DEFAULT SCOPE}" --domain "{TENANT DOMAIN}" -ho -o {APP NAME} -ssp "{SIGN UP OR SIGN IN POLICY}"
```

| 預留位置                   | Azure 入口網站名稱                                     | 範例                                        |
| ----------------------------- | ----------------------------------------------------- | ---------------------------------------------- |
| `{AAD B2C INSTANCE}`          | 執行個體                                              | `https://contoso.b2clogin.com/`                |
| `{APP NAME}`                  | &mdash;                                               | `BlazorSample`                                 |
| `{CLIENT APP CLIENT ID}`      | 應用程式 (用戶端) 的應用程式識別碼 *`Client`*        | `4369008b-21fa-427c-abaa-9b53bf58e538`         |
| `{DEFAULT SCOPE}`             | 範圍名稱                                            | `API.Access`                                   |
| `{SERVER API APP CLIENT ID}`  | *伺服器 API 應用* 程式 (用戶端) 識別碼      | `41451fa7-82d9-4673-8fa5-69eff5a761fd`         |
| `{SERVER API APP ID URI}`     | 應用程式識別碼 URI&dagger;                            | `41451fa7-82d9-4673-8fa5-69eff5a761fd`&dagger; |
| `{SIGN UP OR SIGN IN POLICY}` | 註冊/登入使用者流程                             | `B2C_1_signupsignin1`                          |
| `{TENANT DOMAIN}`             | 主要/發行者/租使用者網域                       | `contoso.onmicrosoft.com`                      |

&dagger;Blazor WebAssembly範本會自動將配置新增 `api://` 至命令中傳遞的應用程式識別碼 URI 引數 `dotnet new` 。 提供預留位置的應用程式識別碼 URI， `{SERVER API APP ID URI}` 而且如果配置為 `api://` ，請從引數中移除配置 (`api://`) ，如同上表中的範例值所示。 如果應用程式識別碼 URI 是自訂值，或有一些其他配置 (例如，如果 `https://` 不受信任的發行者網域類似 `https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd`) ，您必須手動更新預設範圍 URI，並在 `api://` *`Client`* 範本建立應用程式之後移除配置。 如需詳細資訊，請參閱 [存取權杖範圍](#access-token-scopes) 一節中的附注。 此 Blazor WebAssembly 範本可能會在未來的 ASP.NET Core 版本中變更，以解決這些案例。 如需詳細資訊，請參閱 [WASM template (hosted 的應用程式識別碼 URI 的雙配置 Blazor 、單一組織)  (dotnet/aspnetcore #27417) ](https://github.com/dotnet/aspnetcore/issues/27417)。

使用 `-o|--output` 選項指定的輸出位置會建立專案資料夾 (如果不存在)，並成為應用程式名稱的一部分。

> [!NOTE]
> 裝載範本所設定的範圍 Blazor 可能會有重複的應用程式識別碼 URI 主機。 確認 `DefaultAccessTokenScopes` 在 `Program.Main` 應用程式的 () 中為集合設定的範圍正確 `Program.cs` *`Client`* 。

> [!NOTE]
> 在 Azure 入口網站中， *`Client`* 會針對使用預設設定在 Kestrel 伺服器上執行的應用程式，將應用程式的平臺設定重新 **導向 URI** 設定為使用埠5001。
>
> 如果 *`Client`* 應用程式是在隨機的 IIS Express 埠上執行，則可以在 [**調試** 程式] 面板中的 *伺服器 API 應用程式* 屬性中找到應用程式的埠。
>
> 如果先前未使用 *`Client`* 應用程式的已知埠設定埠，請返回 *`Client`* Azure 入口網站中的應用程式註冊，然後以正確的埠更新重新導向 URI。

## <a name="server-app-configuration"></a>*`Server`* 應用程式設定

*本節適用于解決方案的 **`Server`** 應用程式。*

### <a name="authentication-package"></a>驗證套件

對 ASP.NET Core Web Api 的驗證和授權呼叫支援是由 [`Microsoft.AspNetCore.Authentication.AzureADB2C.UI`](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureADB2C.UI) 套件提供：

```xml
<PackageReference Include="Microsoft.AspNetCore.Authentication.AzureADB2C.UI" 
  Version="{VERSION}" />
```

針對預留位置 `{VERSION}` ，可以在套件的 **版本歷程記錄** （ [NuGet.org](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.AzureAD.UI)）中找到符合應用程式共用架構版本的最新穩定版本套件。

### <a name="authentication-service-support"></a>驗證服務支援

方法會在 `AddAuthentication` 應用程式中設定驗證服務，並將 JWT 持有人處理常式設定為預設驗證方法。 <xref:Microsoft.AspNetCore.Authentication.AzureADB2CAuthenticationBuilderExtensions.AddAzureADB2CBearer%2A>方法會設定 JWT 持有人處理常式中的特定參數，以驗證 Azure Active Directory B2C 所發出的權杖：

```csharp
services.AddAuthentication(AzureADB2CDefaults.BearerAuthenticationScheme)
    .AddAzureADB2CBearer(options => Configuration.Bind("AzureAdB2C", options));
```

<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication%2A> 並 <xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization%2A> 確定：

* 應用程式會嘗試剖析並驗證傳入要求的權杖。
* 任何嘗試在沒有適當認證的情況下存取受保護資源的要求都會失敗。

```csharp
app.UseAuthentication();
app.UseAuthorization();
```

### <a name="userno-locidentityname"></a>使用者 ... Identity名字

依預設， `User.Identity.Name` 不會填入。

若要將應用程式設定為接收來自宣告 `name` 類型的值，請 <xref:Microsoft.IdentityModel.Tokens.TokenValidationParameters.NameClaimType?displayProperty=nameWithType> <xref:Microsoft.AspNetCore.Authentication.JwtBearer.JwtBearerOptions> 在中設定 `Startup.ConfigureServices` ：

```csharp
using Microsoft.AspNetCore.Authentication.JwtBearer;

...

services.Configure<JwtBearerOptions>(
    AzureADB2CDefaults.JwtBearerAuthenticationScheme, options =>
    {
        options.TokenValidationParameters.NameClaimType = "name";
    });
```

### <a name="app-settings"></a>應用程式設定

檔案 `appsettings.json` 包含設定用來驗證存取權杖之 JWT 持有人處理常式的選項。

```json
{
  "AzureAdB2C": {
    "Instance": "https://{TENANT}.b2clogin.com/",
    "ClientId": "{SERVER API APP CLIENT ID}",
    "Domain": "{TENANT DOMAIN}",
    "SignUpSignInPolicyId": "{SIGN UP OR SIGN IN POLICY}"
  }
}
```

範例：

```json
{
  "AzureAdB2C": {
    "Instance": "https://contoso.b2clogin.com/",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "Domain": "contoso.onmicrosoft.com",
    "SignUpSignInPolicyId": "B2C_1_signupsignin1",
  }
}
```

### <a name="weatherforecast-controller"></a>WeatherForecast 控制器

WeatherForecast 控制器 (*控制器/WeatherForecastController*) 會公開受保護的 API，並將 [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) 屬性套用至控制器。 請 **務必** 瞭解：

* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)此 api 控制器中的屬性是保護此 api 不會遭到未經授權存取的唯一做法。
* [`[Authorize]`](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute)應用程式中使用的屬性 Blazor WebAssembly 僅做為應用程式的提示，使用者應獲得授權才能讓應用程式正常運作。

```csharp
[Authorize]
[ApiController]
[Route("[controller]")]
public class WeatherForecastController : ControllerBase
{
    [HttpGet]
    public IEnumerable<WeatherForecast> Get()
    {
        ...
    }
}
```

## <a name="client-app-configuration"></a>*`Client`* 應用程式設定

*本節適用于解決方案的 **`Client`** 應用程式。*

### <a name="authentication-package"></a>驗證套件

建立應用程式以使用個別 B2C 帳戶 (`IndividualB2C`) 時，應用程式會自動收到 [Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview) 的套件參考 ([`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)) 。 此套件提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。

如果將驗證新增至應用程式，請手動將套件新增到應用程式的專案檔：

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="{VERSION}" />
```

針對預留位置 `{VERSION}` ，可以在套件的 **版本歷程記錄** （ [NuGet.org](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)）中找到符合應用程式共用架構版本的最新穩定版本套件。

[`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal)封裝可傳遞將 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication) 套件新增至應用程式。

### <a name="authentication-service-support"></a>驗證服務支援

在對 <xref:System.Net.Http.HttpClient> 伺服器專案提出要求時，會加入包含存取權杖的實例支援。

`Program.cs`:

```csharp
builder.Services.AddHttpClient("{APP ASSEMBLY}.ServerAPI", client => 
    client.BaseAddress = new Uri(builder.HostEnvironment.BaseAddress))
    .AddHttpMessageHandler<BaseAddressAuthorizationMessageHandler>();

builder.Services.AddScoped(sp => sp.GetRequiredService<IHttpClientFactory>()
    .CreateClient("{APP ASSEMBLY}.ServerAPI"));
```

預留位置 `{APP ASSEMBLY}` 是應用程式的元件名稱 (例如 `BlazorSample.ServerAPI`) 。

驗證使用者的支援是在服務容器中註冊，並具有 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> 由封裝提供的擴充方法 [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal) 。 此方法會設定應用程式 Identity (IP) 與提供者互動所需的服務。

`Program.cs`:

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAdB2C", options.ProviderOptions.Authentication);
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

<xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。 當您註冊應用程式時，可以從 Azure 入口網站 AAD 設定取得設定應用程式所需的值。

設定是由檔案提供 `wwwroot/appsettings.json` ：

```json
{
  "AzureAdB2C": {
    "Authority": "{AAD B2C INSTANCE}{TENANT DOMAIN}/{SIGN UP OR SIGN IN POLICY}",
    "ClientId": "{CLIENT APP CLIENT ID}",
    "ValidateAuthority": false
  }
}
```

範例：

```json
{
  "AzureAdB2C": {
    "Authority": "https://contoso.b2clogin.com/contoso.onmicrosoft.com/B2C_1_signupsignin1",
    "ClientId": "4369008b-21fa-427c-abaa-9b53bf58e538",
    "ValidateAuthority": false
  }
}
```

### <a name="access-token-scopes"></a>存取權杖範圍

預設存取權杖範圍代表存取權杖範圍的清單，這些是：

* 預設包含在登入要求中。
* 用來在驗證之後立即布建存取權杖。

所有範圍都必須屬於相同的應用程式（依 Azure Active Directory 規則）。 您可以視需要為其他 API 應用程式新增其他範圍：

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

> [!NOTE]
> Blazor WebAssembly範本會自動將配置新增 `api://` 至命令中傳遞的應用程式識別碼 URI 引數 `dotnet new` 。 從專案範本產生應用程式時 Blazor ，請確認預設存取權杖範圍的值使用您在 Azure 入口網站中提供的正確自訂應用程式識別碼 URI 值，或使用下列 **其中一** 種格式的值：
>
> * **信任** 目錄的發行者網域時，預設的存取權杖範圍通常會是類似下列範例的值，其中 `API.Access` 是預設的範圍名稱：
>
>   ```csharp
>   options.ProviderOptions.DefaultAccessTokenScopes.Add(
>       "api://41451fa7-82d9-4673-8fa5-69eff5a761fd/API.Access");
>   ```
>
>   檢查雙精度浮配置的值 (`api://api://...`) 。 如果有雙重配置，請 `api://` 從值中移除第一個配置。
>
> * 當目錄的發行者網域不 **受信任** 時，預設存取權杖範圍通常會是類似下列範例的值，其中 `API.Access` 是預設的範圍名稱：
>
>   ```csharp
>   options.ProviderOptions.DefaultAccessTokenScopes.Add(
>       "https://contoso.onmicrosoft.com/41451fa7-82d9-4673-8fa5-69eff5a761fd/API.Access");
>   ```
>
>   檢查額外 `api://` 配置 () 的值 `api://https://contoso.onmicrosoft.com/...` 。 如果有額外的 `api://` 配置，請 `api://` 從值中移除配置。
>
> 此 Blazor WebAssembly 範本可能會在未來的 ASP.NET Core 版本中變更，以解決這些案例。 如需詳細資訊，請參閱 [WASM template (hosted 的應用程式識別碼 URI 的雙配置 Blazor 、單一組織)  (dotnet/aspnetcore #27417) ](https://github.com/dotnet/aspnetcore/issues/27417)。

使用下列內容指定其他範圍 `AdditionalScopesToConsent` ：

```csharp
options.ProviderOptions.AdditionalScopesToConsent.Add("{ADDITIONAL SCOPE URI}");
```

如需詳細資訊，請參閱下列 *其他案例* 文章的各節：

* [要求其他存取權杖](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [將權杖附加至傳出要求](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

::: moniker range=">= aspnetcore-5.0"

### <a name="login-mode"></a>登入模式

[!INCLUDE[](~/includes/blazor-security/msal-login-mode.md)]

::: moniker-end

### <a name="imports-file"></a>匯入檔案

[!INCLUDE[](~/includes/blazor-security/imports-file-hosted.md)]

### <a name="index-page"></a>索引頁面

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

### <a name="app-component"></a>應用程式元件

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

### <a name="redirecttologin-component"></a>RedirectToLogin 元件

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

### <a name="logindisplay-component"></a>LoginDisplay 元件

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

### <a name="authentication-component"></a>驗證元件

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

### <a name="fetchdata-component"></a>FetchData 元件

[!INCLUDE[](~/includes/blazor-security/fetchdata-component.md)]

## <a name="run-the-app"></a>執行應用程式

從伺服器專案執行應用程式。 使用 Visual Studio 時，您可以：

* 將工具列中的 [ **啟始專案** ] 下拉式清單設定為 *伺服器 API 應用程式* ，然後選取 [ **執行** ] 按鈕。
* 在 **方案總管** 中選取伺服器專案，然後選取工具列中的 [ **執行** ] 按鈕，或從 [ **調試** 程式] 功能表啟動應用程式。

<!-- HOLD
[!INCLUDE[](~/includes/blazor-security/usermanager-signinmanager.md)]
-->

[!INCLUDE[](~/includes/blazor-security/wasm-aad-b2c-userflows.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a>其他資源

* <xref:blazor/security/webassembly/additional-scenarios>
* [具有安全預設用戶端之應用程式中未經驗證或未經授權的 web API 要求](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:security/authentication/azure-ad-b2c>
* [教學課程：建立 Azure Active Directory B2C 租用戶](/azure/active-directory-b2c/tutorial-create-tenant) \(部分機器翻譯\)
* [Microsoft 身分識別平台文件](/azure/active-directory/develop/)
