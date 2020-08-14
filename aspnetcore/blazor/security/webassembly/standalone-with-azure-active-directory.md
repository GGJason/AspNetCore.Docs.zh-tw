---
title: Blazor WebAssembly使用 Azure Active Directory 保護 ASP.NET Core 獨立應用程式
author: guardrex
description: ''
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: devx-track-csharp, mvc
ms.date: 07/08/2020
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
uid: blazor/security/webassembly/standalone-with-azure-active-directory
ms.openlocfilehash: c64e6f75c4d778a534b1e14e61df39c32cf2943d
ms.sourcegitcommit: ec41ab354952b75557240923756a8c2ac79b49f8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/13/2020
ms.locfileid: "88202745"
---
# <a name="secure-an-aspnet-core-no-locblazor-webassembly-standalone-app-with-azure-active-directory"></a><span data-ttu-id="5e055-102">Blazor WebAssembly使用 Azure Active Directory 保護 ASP.NET Core 獨立應用程式</span><span class="sxs-lookup"><span data-stu-id="5e055-102">Secure an ASP.NET Core Blazor WebAssembly standalone app with Azure Active Directory</span></span>

<span data-ttu-id="5e055-103">By [Javier Calvarro Nelson](https://github.com/javiercn) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="5e055-103">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="5e055-104">若要建立使用[Azure Active Directory (AAD) ](https://azure.microsoft.com/services/active-directory/)進行驗證的[獨立 Blazor WebAssembly 應用程式](xref:blazor/hosting-models#blazor-webassembly)：</span><span class="sxs-lookup"><span data-stu-id="5e055-104">To create a [standalone Blazor WebAssembly app](xref:blazor/hosting-models#blazor-webassembly) that uses [Azure Active Directory (AAD)](https://azure.microsoft.com/services/active-directory/) for authentication:</span></span>

<span data-ttu-id="5e055-105">[建立 AAD 租使用者和 web 應用程式](/azure/active-directory/develop/v2-overview)：</span><span class="sxs-lookup"><span data-stu-id="5e055-105">[Create an AAD tenant and web application](/azure/active-directory/develop/v2-overview):</span></span>

<span data-ttu-id="5e055-106">在 Azure 入口網站的**Azure Active Directory**  >  **應用程式註冊**] 區域中註冊 AAD 應用程式：</span><span class="sxs-lookup"><span data-stu-id="5e055-106">Register a AAD app in the **Azure Active Directory** > **App registrations** area of the Azure portal:</span></span>

1. <span data-ttu-id="5e055-107">提供應用程式 (的**名稱**，例如\*\* Blazor 獨立 AAD\*\*) 。</span><span class="sxs-lookup"><span data-stu-id="5e055-107">Provide a **Name** for the app (for example, **Blazor Standalone AAD**).</span></span>
1. <span data-ttu-id="5e055-108">選擇 **支援的帳戶類型**。</span><span class="sxs-lookup"><span data-stu-id="5e055-108">Choose a **Supported account types**.</span></span> <span data-ttu-id="5e055-109">只有在此體驗中，您可以選取 **此組織目錄中的帳戶** 。</span><span class="sxs-lookup"><span data-stu-id="5e055-109">You may select **Accounts in this organizational directory only** for this experience.</span></span>
1. <span data-ttu-id="5e055-110">將 [重新 **導向 uri** ] 下拉式設定保留為 [ **Web** ]，並提供下列重新導向 uri： `https://localhost:{PORT}/authentication/login-callback` 。</span><span class="sxs-lookup"><span data-stu-id="5e055-110">Leave the **Redirect URI** drop down set to **Web** and provide the following redirect URI: `https://localhost:{PORT}/authentication/login-callback`.</span></span> <span data-ttu-id="5e055-111">在 Kestrel 上執行之應用程式的預設埠是5001。</span><span class="sxs-lookup"><span data-stu-id="5e055-111">The default port for an app running on Kestrel is 5001.</span></span> <span data-ttu-id="5e055-112">如果應用程式是在不同的 Kestrel 埠上執行，請使用應用程式的埠。</span><span class="sxs-lookup"><span data-stu-id="5e055-112">If the app is run on a different Kestrel port, use the app's port.</span></span> <span data-ttu-id="5e055-113">針對 IIS Express，應用程式的隨機產生埠可以在 [ **調試** 程式] 面板的 [屬性] 中找到。</span><span class="sxs-lookup"><span data-stu-id="5e055-113">For IIS Express, the randomly generated port for the app can be found in the app's properties in the **Debug** panel.</span></span> <span data-ttu-id="5e055-114">由於應用程式目前不存在，且 IIS Express 埠未知，請在建立應用程式之後返回此步驟，並更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="5e055-114">Since the app doesn't exist at this point and the IIS Express port isn't known, return to this step after the app is created and update the redirect URI.</span></span> <span data-ttu-id="5e055-115">本主題稍後會出現一個批註，提醒 IIS Express 使用者更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="5e055-115">A remark appears later in this topic to remind IIS Express users to update the redirect URI.</span></span>
1. <span data-ttu-id="5e055-116">停用 **[授與系統**  >  **管理員同意 openid 和 offline_access 許可權**] 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="5e055-116">Disable the **Permissions** > **Grant admin consent to openid and offline_access permissions** check box.</span></span>
1. <span data-ttu-id="5e055-117">選取 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="5e055-117">Select **Register**.</span></span>

<span data-ttu-id="5e055-118">記錄下列資訊：</span><span class="sxs-lookup"><span data-stu-id="5e055-118">Record the following information:</span></span>

* <span data-ttu-id="5e055-119">應用程式 (用戶端) 識別碼 (例如 `41451fa7-82d9-4673-8fa5-69eff5a761fd`) </span><span class="sxs-lookup"><span data-stu-id="5e055-119">Application (client) ID (for example, `41451fa7-82d9-4673-8fa5-69eff5a761fd`)</span></span>
* <span data-ttu-id="5e055-120">目錄 (租使用者) 識別碼 (例如 `e86c78e2-8bb4-4c41-aefd-918e0565a45e`) </span><span class="sxs-lookup"><span data-stu-id="5e055-120">Directory (tenant) ID (for example, `e86c78e2-8bb4-4c41-aefd-918e0565a45e`)</span></span>

<span data-ttu-id="5e055-121">在 [**驗證**  >  **平臺**設定]  >  **Web**：</span><span class="sxs-lookup"><span data-stu-id="5e055-121">In **Authentication** > **Platform configurations** > **Web**:</span></span>

1. <span data-ttu-id="5e055-122">確認的重新 **導向 URI** `https://localhost:{PORT}/authentication/login-callback` 存在。</span><span class="sxs-lookup"><span data-stu-id="5e055-122">Confirm the **Redirect URI** of `https://localhost:{PORT}/authentication/login-callback` is present.</span></span>
1. <span data-ttu-id="5e055-123">針對 **[隱含授**與]，選取 [ **存取權杖** ] 和 [ **識別碼權杖**] 的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="5e055-123">For **Implicit grant**, select the check boxes for **Access tokens** and **ID tokens**.</span></span>
1. <span data-ttu-id="5e055-124">此體驗可接受應用程式的其餘預設值。</span><span class="sxs-lookup"><span data-stu-id="5e055-124">The remaining defaults for the app are acceptable for this experience.</span></span>
1. <span data-ttu-id="5e055-125">選取 [儲存] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="5e055-125">Select the **Save** button.</span></span>

<span data-ttu-id="5e055-126">在空的資料夾中建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e055-126">Create the app in an empty folder.</span></span> <span data-ttu-id="5e055-127">以先前記錄的資訊取代下列命令中的預留位置，並在命令 shell 中執行命令：</span><span class="sxs-lookup"><span data-stu-id="5e055-127">Replace the placeholders in the following command with the information recorded earlier and execute the command in a command shell:</span></span>

```dotnetcli
dotnet new blazorwasm -au SingleOrg --client-id "{CLIENT ID}" -o {APP NAME} --tenant-id "{TENANT ID}"
```

| <span data-ttu-id="5e055-128">預留位置</span><span class="sxs-lookup"><span data-stu-id="5e055-128">Placeholder</span></span>   | <span data-ttu-id="5e055-129">Azure 入口網站名稱</span><span class="sxs-lookup"><span data-stu-id="5e055-129">Azure portal name</span></span>       | <span data-ttu-id="5e055-130">範例</span><span class="sxs-lookup"><span data-stu-id="5e055-130">Example</span></span>                                |
| ------------- | ----------------------- | -------------------------------------- |
| `{APP NAME}`  | &mdash;                 | `BlazorSample`                         |
| `{CLIENT ID}` | <span data-ttu-id="5e055-131">應用程式 (用戶端) 識別碼</span><span class="sxs-lookup"><span data-stu-id="5e055-131">Application (client) ID</span></span> | `41451fa7-82d9-4673-8fa5-69eff5a761fd` |
| `{TENANT ID}` | <span data-ttu-id="5e055-132">目錄 (租用戶) 識別碼</span><span class="sxs-lookup"><span data-stu-id="5e055-132">Directory (tenant) ID</span></span>   | `e86c78e2-8bb4-4c41-aefd-918e0565a45e` |

<span data-ttu-id="5e055-133">使用選項指定的輸出位置會 `-o|--output` 建立專案資料夾（如果不存在），並成為應用程式名稱的一部分。</span><span class="sxs-lookup"><span data-stu-id="5e055-133">The output location specified with the `-o|--output` option creates a project folder if it doesn't exist and becomes part of the app's name.</span></span>

> [!NOTE]
> <span data-ttu-id="5e055-134">在 Azure 入口網站中，應用程式的**驗證**  >  **平臺**  >  設定**Web**重新  >  **導向 URI**會針對使用預設設定在 Kestrel 伺服器上執行的應用程式進行通訊埠5001。</span><span class="sxs-lookup"><span data-stu-id="5e055-134">In the Azure portal, the app's **Authentication** > **Platform configurations** > **Web** > **Redirect URI** is configured for port 5001 for apps that run on the Kestrel server with default settings.</span></span>
>
> <span data-ttu-id="5e055-135">如果應用程式是在隨機 IIS Express 埠上執行，則可以在 [ **調試** 程式] 面板的 [屬性] 中找到應用程式的埠。</span><span class="sxs-lookup"><span data-stu-id="5e055-135">If the app is run on a random IIS Express port, the port for the app can be found in the app's properties in the **Debug** panel.</span></span>
>
> <span data-ttu-id="5e055-136">如果先前未使用應用程式的已知埠設定埠，請回到 Azure 入口網站中的應用程式註冊，並使用正確的埠更新重新導向 URI。</span><span class="sxs-lookup"><span data-stu-id="5e055-136">If the port wasn't configured earlier with the app's known port, return to the app's registration in the Azure portal and update the redirect URI with the correct port.</span></span>

<span data-ttu-id="5e055-137">建立應用程式之後，您應該能夠：</span><span class="sxs-lookup"><span data-stu-id="5e055-137">After creating the app, you should be able to:</span></span>

* <span data-ttu-id="5e055-138">使用 AAD 使用者帳戶登入應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e055-138">Log into the app using an AAD user account.</span></span>
* <span data-ttu-id="5e055-139">要求 Microsoft Api 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="5e055-139">Request access tokens for Microsoft APIs.</span></span> <span data-ttu-id="5e055-140">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="5e055-140">For more information, see:</span></span>
  * [<span data-ttu-id="5e055-141">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="5e055-141">Access token scopes</span></span>](#access-token-scopes)
  * <span data-ttu-id="5e055-142">[快速入門：設定應用程式以公開 Web api](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis)。</span><span class="sxs-lookup"><span data-stu-id="5e055-142">[Quickstart: Configure an application to expose web APIs](/azure/active-directory/develop/quickstart-configure-app-expose-web-apis).</span></span>

## <a name="authentication-package"></a><span data-ttu-id="5e055-143">驗證套件</span><span class="sxs-lookup"><span data-stu-id="5e055-143">Authentication package</span></span>

<span data-ttu-id="5e055-144">當建立應用程式以使用工作或學校帳戶 (`SingleOrg`) 時，應用程式會自動收到 [Microsoft 驗證程式庫](/azure/active-directory/develop/msal-overview) () 的套件參考 [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) 。</span><span class="sxs-lookup"><span data-stu-id="5e055-144">When an app is created to use Work or School Accounts (`SingleOrg`), the app automatically receives a package reference for the [Microsoft Authentication Library](/azure/active-directory/develop/msal-overview) ([`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)).</span></span> <span data-ttu-id="5e055-145">封裝提供一組基本類型，可協助應用程式驗證使用者，並取得權杖以呼叫受保護的 Api。</span><span class="sxs-lookup"><span data-stu-id="5e055-145">The package provides a set of primitives that help the app authenticate users and obtain tokens to call protected APIs.</span></span>

<span data-ttu-id="5e055-146">如果將驗證新增至應用程式，請手動將套件新增至應用程式的專案檔：</span><span class="sxs-lookup"><span data-stu-id="5e055-146">If adding authentication to an app, manually add the package to the app's project file:</span></span>

```xml
<PackageReference Include="Microsoft.Authentication.WebAssembly.Msal" 
  Version="3.2.0" />
```

<span data-ttu-id="5e055-147">[`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/)封裝可傳遞會將 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 套件新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="5e055-147">The [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) package transitively adds the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package to the app.</span></span>

## <a name="authentication-service-support"></a><span data-ttu-id="5e055-148">驗證服務支援</span><span class="sxs-lookup"><span data-stu-id="5e055-148">Authentication service support</span></span>

<span data-ttu-id="5e055-149">使用封裝所提供的擴充方法，在服務容器中註冊驗證使用者的支援 <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) 。</span><span class="sxs-lookup"><span data-stu-id="5e055-149">Support for authenticating users is registered in the service container with the <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> extension method provided by the [`Microsoft.Authentication.WebAssembly.Msal`](https://www.nuget.org/packages/Microsoft.Authentication.WebAssembly.Msal/) package.</span></span> <span data-ttu-id="5e055-150">這個方法會設定應用程式與 Identity 提供者 (IP) 互動所需的服務。</span><span class="sxs-lookup"><span data-stu-id="5e055-150">This method sets up the services required for the app to interact with the Identity Provider (IP).</span></span>

<span data-ttu-id="5e055-151">`Program.cs`:</span><span class="sxs-lookup"><span data-stu-id="5e055-151">`Program.cs`:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    builder.Configuration.Bind("AzureAd", options.ProviderOptions.Authentication);
});
```

<span data-ttu-id="5e055-152"><xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A>方法會接受回呼來設定驗證應用程式所需的參數。</span><span class="sxs-lookup"><span data-stu-id="5e055-152">The <xref:Microsoft.Extensions.DependencyInjection.MsalWebAssemblyServiceCollectionExtensions.AddMsalAuthentication%2A> method accepts a callback to configure the parameters required to authenticate an app.</span></span> <span data-ttu-id="5e055-153">當您註冊應用程式時，可以從 AAD 設定取得設定應用程式所需的值。</span><span class="sxs-lookup"><span data-stu-id="5e055-153">The values required for configuring the app can be obtained from the AAD configuration when you register the app.</span></span>

<span data-ttu-id="5e055-154">設定是由檔案所提供 `wwwroot/appsettings.json` ：</span><span class="sxs-lookup"><span data-stu-id="5e055-154">Configuration is supplied by the `wwwroot/appsettings.json` file:</span></span>

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/{TENANT ID}",
    "ClientId": "{CLIENT ID}",
    "ValidateAuthority": true
  }
}
```

<span data-ttu-id="5e055-155">範例：</span><span class="sxs-lookup"><span data-stu-id="5e055-155">Example:</span></span>

```json
{
  "AzureAd": {
    "Authority": "https://login.microsoftonline.com/e86c78e2-...-918e0565a45e",
    "ClientId": "41451fa7-82d9-4673-8fa5-69eff5a761fd",
    "ValidateAuthority": true
  }
}
```

## <a name="access-token-scopes"></a><span data-ttu-id="5e055-156">存取權杖範圍</span><span class="sxs-lookup"><span data-stu-id="5e055-156">Access token scopes</span></span>

<span data-ttu-id="5e055-157">Blazor WebAssembly範本不會自動將應用程式設定為要求安全 API 的存取權杖。</span><span class="sxs-lookup"><span data-stu-id="5e055-157">The Blazor WebAssembly template doesn't automatically configure the app to request an access token for a secure API.</span></span> <span data-ttu-id="5e055-158">若要在登入流程中布建存取權杖，請將範圍新增至的預設存取權杖範圍 <xref:Microsoft.Authentication.WebAssembly.Msal.Models.MsalProviderOptions> ：</span><span class="sxs-lookup"><span data-stu-id="5e055-158">To provision an access token as part of the sign-in flow, add the scope to the default access token scopes of the <xref:Microsoft.Authentication.WebAssembly.Msal.Models.MsalProviderOptions>:</span></span>

```csharp
builder.Services.AddMsalAuthentication(options =>
{
    ...
    options.ProviderOptions.DefaultAccessTokenScopes.Add("{SCOPE URI}");
});
```

[!INCLUDE[](~/includes/blazor-security/azure-scope.md)]

<span data-ttu-id="5e055-159">如需詳細資訊，請參閱 *其他案例* 文章的下列章節：</span><span class="sxs-lookup"><span data-stu-id="5e055-159">For more information, see the following sections of the *Additional scenarios* article:</span></span>

* [<span data-ttu-id="5e055-160">要求其他存取權杖</span><span class="sxs-lookup"><span data-stu-id="5e055-160">Request additional access tokens</span></span>](xref:blazor/security/webassembly/additional-scenarios#request-additional-access-tokens)
* [<span data-ttu-id="5e055-161">將權杖附加到連出要求</span><span class="sxs-lookup"><span data-stu-id="5e055-161">Attach tokens to outgoing requests</span></span>](xref:blazor/security/webassembly/additional-scenarios#attach-tokens-to-outgoing-requests)

::: moniker range=">= aspnetcore-5.0"

## <a name="login-mode"></a><span data-ttu-id="5e055-162">登入模式</span><span class="sxs-lookup"><span data-stu-id="5e055-162">Login mode</span></span>

[!INCLUDE[](~/includes/blazor-security/msal-login-mode.md)]

::: moniker-end

## <a name="imports-file"></a><span data-ttu-id="5e055-163">匯入檔案</span><span class="sxs-lookup"><span data-stu-id="5e055-163">Imports file</span></span>

[!INCLUDE[](~/includes/blazor-security/imports-file-standalone.md)]

## <a name="index-page"></a><span data-ttu-id="5e055-164">索引頁面</span><span class="sxs-lookup"><span data-stu-id="5e055-164">Index page</span></span>

[!INCLUDE[](~/includes/blazor-security/index-page-msal.md)]

## <a name="app-component"></a><span data-ttu-id="5e055-165">應用程式元件</span><span class="sxs-lookup"><span data-stu-id="5e055-165">App component</span></span>

[!INCLUDE[](~/includes/blazor-security/app-component.md)]

## <a name="redirecttologin-component"></a><span data-ttu-id="5e055-166">RedirectToLogin 元件</span><span class="sxs-lookup"><span data-stu-id="5e055-166">RedirectToLogin component</span></span>

[!INCLUDE[](~/includes/blazor-security/redirecttologin-component.md)]

## <a name="logindisplay-component"></a><span data-ttu-id="5e055-167">LoginDisplay 元件</span><span class="sxs-lookup"><span data-stu-id="5e055-167">LoginDisplay component</span></span>

[!INCLUDE[](~/includes/blazor-security/logindisplay-component.md)]

## <a name="authentication-component"></a><span data-ttu-id="5e055-168">驗證元件</span><span class="sxs-lookup"><span data-stu-id="5e055-168">Authentication component</span></span>

[!INCLUDE[](~/includes/blazor-security/authentication-component.md)]

[!INCLUDE[](~/includes/blazor-security/troubleshoot.md)]

## <a name="additional-resources"></a><span data-ttu-id="5e055-169">其他資源</span><span class="sxs-lookup"><span data-stu-id="5e055-169">Additional resources</span></span>

* <xref:blazor/security/webassembly/additional-scenarios>
* [<span data-ttu-id="5e055-170">在具有安全預設用戶端的應用程式中，未經驗證或未經授權的 Web API 要求</span><span class="sxs-lookup"><span data-stu-id="5e055-170">Unauthenticated or unauthorized web API requests in an app with a secure default client</span></span>](xref:blazor/security/webassembly/additional-scenarios#unauthenticated-or-unauthorized-web-api-requests-in-an-app-with-a-secure-default-client)
* <xref:blazor/security/webassembly/aad-groups-roles>
* <xref:security/authentication/azure-active-directory/index>
* [<span data-ttu-id="5e055-171">Microsoft 身分識別平台文件</span><span class="sxs-lookup"><span data-stu-id="5e055-171">Microsoft identity platform documentation</span></span>](/azure/active-directory/develop/)
