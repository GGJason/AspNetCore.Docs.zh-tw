---
title: 在 ASP.NET Core 中使用 Azure Active Directory B2C 進行雲端驗證
author: camsoper
description: 探索如何使用 ASP.NET Core 設定 Azure Active Directory B2C 驗證。
ms.author: casoper
ms.custom: mvc
ms.date: 01/21/2019
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
uid: security/authentication/azure-ad-b2c
ms.openlocfilehash: 78fe4d5dd9e3f64789956e58a4490bef6bdbca1e
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88021701"
---
# <a name="cloud-authentication-with-azure-active-directory-b2c-in-aspnet-core"></a><span data-ttu-id="cafe9-103">在 ASP.NET Core 中使用 Azure Active Directory B2C 進行雲端驗證</span><span class="sxs-lookup"><span data-stu-id="cafe9-103">Cloud authentication with Azure Active Directory B2C in ASP.NET Core</span></span>

<span data-ttu-id="cafe9-104">作者 [Cam Soper](https://twitter.com/camsoper)</span><span class="sxs-lookup"><span data-stu-id="cafe9-104">By [Cam Soper](https://twitter.com/camsoper)</span></span>

<span data-ttu-id="cafe9-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) 是適用于 web 和行動應用程式的雲端身分識別管理解決方案。</span><span class="sxs-lookup"><span data-stu-id="cafe9-105">[Azure Active Directory B2C](/azure/active-directory-b2c/active-directory-b2c-overview) (Azure AD B2C) is a cloud identity management solution for web and mobile apps.</span></span> <span data-ttu-id="cafe9-106">此服務會針對雲端和內部部署中裝載的應用程式提供驗證。</span><span class="sxs-lookup"><span data-stu-id="cafe9-106">The service provides authentication for apps hosted in the cloud and on-premises.</span></span> <span data-ttu-id="cafe9-107">驗證類型包括個別帳戶、社交網路帳戶和同盟企業帳戶。</span><span class="sxs-lookup"><span data-stu-id="cafe9-107">Authentication types include individual accounts, social network accounts, and federated enterprise accounts.</span></span> <span data-ttu-id="cafe9-108">此外，Azure AD B2C 可以使用最少的設定來提供多重要素驗證。</span><span class="sxs-lookup"><span data-stu-id="cafe9-108">Additionally, Azure AD B2C can provide multi-factor authentication with minimal configuration.</span></span>

> [!TIP]
> <span data-ttu-id="cafe9-109">Azure Active Directory (Azure AD) 和 Azure AD B2C 是個別的產品供應專案。</span><span class="sxs-lookup"><span data-stu-id="cafe9-109">Azure Active Directory (Azure AD) and Azure AD B2C are separate product offerings.</span></span> <span data-ttu-id="cafe9-110">Azure AD 租使用者代表組織，而 Azure AD B2C 租使用者代表要與信賴憑證者應用程式搭配使用的身分識別集合。</span><span class="sxs-lookup"><span data-stu-id="cafe9-110">An Azure AD tenant represents an organization, while an Azure AD B2C tenant represents a collection of identities to be used with relying party applications.</span></span> <span data-ttu-id="cafe9-111">若要深入瞭解，請參閱[Azure AD B2C：常見問題 (常見問題) ](/azure/active-directory-b2c/active-directory-b2c-faqs)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-111">To learn more, see [Azure AD B2C: Frequently asked questions (FAQ)](/azure/active-directory-b2c/active-directory-b2c-faqs).</span></span>

<span data-ttu-id="cafe9-112">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="cafe9-112">In this tutorial, learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cafe9-113">建立 Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="cafe9-113">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="cafe9-114">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="cafe9-114">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="cafe9-115">使用 Visual Studio 建立 ASP.NET Core web 應用程式，並將其設定為使用 Azure AD B2C 的租使用者進行驗證</span><span class="sxs-lookup"><span data-stu-id="cafe9-115">Use Visual Studio to create an ASP.NET Core web app configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="cafe9-116">設定原則來控制 Azure AD B2C 租使用者的行為</span><span class="sxs-lookup"><span data-stu-id="cafe9-116">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cafe9-117">必要條件</span><span class="sxs-lookup"><span data-stu-id="cafe9-117">Prerequisites</span></span>

<span data-ttu-id="cafe9-118">此逐步解說需要下列各項：</span><span class="sxs-lookup"><span data-stu-id="cafe9-118">The following are required for this walkthrough:</span></span>

* [<span data-ttu-id="cafe9-119">Microsoft Azure 訂用帳戶</span><span class="sxs-lookup"><span data-stu-id="cafe9-119">Microsoft Azure subscription</span></span>](https://azure.microsoft.com/free/dotnet/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [<span data-ttu-id="cafe9-120">Visual Studio 2019</span><span class="sxs-lookup"><span data-stu-id="cafe9-120">Visual Studio 2019</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

## <a name="create-the-azure-active-directory-b2c-tenant"></a><span data-ttu-id="cafe9-121">建立 Azure Active Directory B2C 租使用者</span><span class="sxs-lookup"><span data-stu-id="cafe9-121">Create the Azure Active Directory B2C tenant</span></span>

<span data-ttu-id="cafe9-122">建立 Azure Active Directory B2C 的租使用者，[如檔中所述](/azure/active-directory-b2c/active-directory-b2c-get-started)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-122">Create an Azure Active Directory B2C tenant [as described in the documentation](/azure/active-directory-b2c/active-directory-b2c-get-started).</span></span> <span data-ttu-id="cafe9-123">出現提示時，在本教學課程中，將租使用者與 Azure 訂用帳戶產生關聯是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="cafe9-123">When prompted, associating the tenant with an Azure subscription is optional for this tutorial.</span></span>

## <a name="register-the-app-in-azure-ad-b2c"></a><span data-ttu-id="cafe9-124">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="cafe9-124">Register the app in Azure AD B2C</span></span>

<span data-ttu-id="cafe9-125">在新建立的 Azure AD B2C 租使用者中，使用**註冊 web 應用程式**一節底下[檔中的步驟](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)來註冊您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-125">In the newly created Azure AD B2C tenant, register your app using [the steps in the documentation](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application) under the **Register a web app** section.</span></span> <span data-ttu-id="cafe9-126">停止**建立 web 應用程式用戶端密碼**一節。</span><span class="sxs-lookup"><span data-stu-id="cafe9-126">Stop at the **Create a web app client secret** section.</span></span> <span data-ttu-id="cafe9-127">本教學課程不需要用戶端密碼。</span><span class="sxs-lookup"><span data-stu-id="cafe9-127">A client secret isn't required for this tutorial.</span></span> 

<span data-ttu-id="cafe9-128">輸入下列值：</span><span class="sxs-lookup"><span data-stu-id="cafe9-128">Use the following values:</span></span>

| <span data-ttu-id="cafe9-129">設定</span><span class="sxs-lookup"><span data-stu-id="cafe9-129">Setting</span></span>                       | <span data-ttu-id="cafe9-130">值</span><span class="sxs-lookup"><span data-stu-id="cafe9-130">Value</span></span>                     | <span data-ttu-id="cafe9-131">備忘錄</span><span class="sxs-lookup"><span data-stu-id="cafe9-131">Notes</span></span>                                                                                                                                                                                              |
|-------------------------------|---------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="cafe9-132">**名稱**</span><span class="sxs-lookup"><span data-stu-id="cafe9-132">**Name**</span></span>                      | <span data-ttu-id="cafe9-133">*&lt;應用程式名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="cafe9-133">*&lt;app name&gt;*</span></span>        | <span data-ttu-id="cafe9-134">輸入應用程式的**名稱**，以向取用者描述您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-134">Enter a **Name** for the app that describes your app to consumers.</span></span>                                                                                                                                 |
| <span data-ttu-id="cafe9-135">**包含 Web 應用程式 / Web API**</span><span class="sxs-lookup"><span data-stu-id="cafe9-135">**Include web app / web API**</span></span> | <span data-ttu-id="cafe9-136">是</span><span class="sxs-lookup"><span data-stu-id="cafe9-136">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="cafe9-137">**允許隱含流程**</span><span class="sxs-lookup"><span data-stu-id="cafe9-137">**Allow implicit flow**</span></span>       | <span data-ttu-id="cafe9-138">是</span><span class="sxs-lookup"><span data-stu-id="cafe9-138">Yes</span></span>                       |                                                                                                                                                                                                    |
| <span data-ttu-id="cafe9-139">**回覆 URL**</span><span class="sxs-lookup"><span data-stu-id="cafe9-139">**Reply URL**</span></span>                 | `https://localhost:44300/signin-oidc` | <span data-ttu-id="cafe9-140">回覆 URL 是 Azure AD B2C 傳回您應用程式要求之任何權杖的所在端點。</span><span class="sxs-lookup"><span data-stu-id="cafe9-140">Reply URLs are endpoints where Azure AD B2C returns any tokens that your app requests.</span></span> <span data-ttu-id="cafe9-141">Visual Studio 提供要使用的回復 URL。</span><span class="sxs-lookup"><span data-stu-id="cafe9-141">Visual Studio provides the Reply URL to use.</span></span> <span data-ttu-id="cafe9-142">現在請輸入 `https://localhost:44300/signin-oidc` 來完成表單。</span><span class="sxs-lookup"><span data-stu-id="cafe9-142">For now, enter `https://localhost:44300/signin-oidc` to complete the form.</span></span> |
| <span data-ttu-id="cafe9-143">**應用程式識別碼 URI**</span><span class="sxs-lookup"><span data-stu-id="cafe9-143">**App ID URI**</span></span>                | <span data-ttu-id="cafe9-144">保留空白</span><span class="sxs-lookup"><span data-stu-id="cafe9-144">Leave blank</span></span>               | <span data-ttu-id="cafe9-145">本教學課程不需要。</span><span class="sxs-lookup"><span data-stu-id="cafe9-145">Not required for this tutorial.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="cafe9-146">**包含原生用戶端**</span><span class="sxs-lookup"><span data-stu-id="cafe9-146">**Include native client**</span></span>     | <span data-ttu-id="cafe9-147">否</span><span class="sxs-lookup"><span data-stu-id="cafe9-147">No</span></span>                        |                                                                                                                                                                                                    |

> [!WARNING]
> <span data-ttu-id="cafe9-148">如果設定非 localhost 回復 URL，請留意[[回復 url] 清單中允許的條件約束](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-148">If setting up a non-localhost Reply URL, be aware of the [constraints on what is allowed in the Reply URL list](/azure/active-directory-b2c/tutorial-register-applications#register-a-web-application).</span></span> 

<span data-ttu-id="cafe9-149">註冊應用程式之後，就會顯示租使用者中的應用程式清單。</span><span class="sxs-lookup"><span data-stu-id="cafe9-149">After the app is registered, the list of apps in the tenant is displayed.</span></span> <span data-ttu-id="cafe9-150">選取剛註冊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-150">Select the app that was just registered.</span></span> <span data-ttu-id="cafe9-151">選取 [**應用程式識別碼**] 欄位右邊的**複製**圖示，將它複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="cafe9-151">Select the **Copy** icon to the right of the **Application ID** field to copy it to the clipboard.</span></span>

<span data-ttu-id="cafe9-152">目前不能在 Azure AD B2C 租使用者中設定其他內容，但讓瀏覽器視窗保持開啟。</span><span class="sxs-lookup"><span data-stu-id="cafe9-152">Nothing more can be configured in the Azure AD B2C tenant at this time, but leave the browser window open.</span></span> <span data-ttu-id="cafe9-153">建立 ASP.NET Core 應用程式之後，會有更多的設定。</span><span class="sxs-lookup"><span data-stu-id="cafe9-153">There is more configuration after the ASP.NET Core app is created.</span></span>

## <a name="create-an-aspnet-core-app-in-visual-studio"></a><span data-ttu-id="cafe9-154">在 Visual Studio 中建立 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="cafe9-154">Create an ASP.NET Core app in Visual Studio</span></span>

<span data-ttu-id="cafe9-155">Visual Studio Web 應用程式範本可以設定為使用 Azure AD B2C 租使用者進行驗證。</span><span class="sxs-lookup"><span data-stu-id="cafe9-155">The Visual Studio Web Application template can be configured to use the Azure AD B2C tenant for authentication.</span></span>

<span data-ttu-id="cafe9-156">在 Visual Studio 中：</span><span class="sxs-lookup"><span data-stu-id="cafe9-156">In Visual Studio:</span></span>

1. <span data-ttu-id="cafe9-157">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-157">Create a new ASP.NET Core Web Application.</span></span> 
2. <span data-ttu-id="cafe9-158">從範本清單中選取 [ **Web 應用程式**]。</span><span class="sxs-lookup"><span data-stu-id="cafe9-158">Select **Web Application** from the list of templates.</span></span>
3. <span data-ttu-id="cafe9-159">選取 [**變更驗證**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="cafe9-159">Select the **Change Authentication** button.</span></span>
    
    ![[變更驗證] 按鈕](./azure-ad-b2c/_static/changeauth.png)

4. <span data-ttu-id="cafe9-161">在 [**變更驗證**] 對話方塊中，選取 [**個別使用者帳戶**]，然後在下拉式清單中選取 [連線**到雲端中現有的使用者存放區]** 。</span><span class="sxs-lookup"><span data-stu-id="cafe9-161">In the **Change Authentication** dialog, select **Individual User Accounts**, and then select **Connect to an existing user store in the cloud** in the dropdown.</span></span> 
    
    ![[變更驗證] 對話方塊](./azure-ad-b2c/_static/changeauthdialog.png)

5. <span data-ttu-id="cafe9-163">使用下列值來完成表單：</span><span class="sxs-lookup"><span data-stu-id="cafe9-163">Complete the form with the following values:</span></span>
    
    | <span data-ttu-id="cafe9-164">設定</span><span class="sxs-lookup"><span data-stu-id="cafe9-164">Setting</span></span>                       | <span data-ttu-id="cafe9-165">值</span><span class="sxs-lookup"><span data-stu-id="cafe9-165">Value</span></span>                                                 |
    |-------------------------------|-------------------------------------------------------|
    | <span data-ttu-id="cafe9-166">**功能變數名稱**</span><span class="sxs-lookup"><span data-stu-id="cafe9-166">**Domain Name**</span></span>               | <span data-ttu-id="cafe9-167">*&lt;B2C 租使用者的功能變數名稱&gt;*</span><span class="sxs-lookup"><span data-stu-id="cafe9-167">*&lt;the domain name of your B2C tenant&gt;*</span></span>          |
    | <span data-ttu-id="cafe9-168">**應用程式識別碼**</span><span class="sxs-lookup"><span data-stu-id="cafe9-168">**Application ID**</span></span>            | <span data-ttu-id="cafe9-169">*&lt;貼上剪貼簿中的應用程式識別碼&gt;*</span><span class="sxs-lookup"><span data-stu-id="cafe9-169">*&lt;paste the Application ID from the clipboard&gt;*</span></span> |
    | <span data-ttu-id="cafe9-170">**回呼路徑**</span><span class="sxs-lookup"><span data-stu-id="cafe9-170">**Callback Path**</span></span>             | <span data-ttu-id="cafe9-171">*&lt;使用預設值&gt;*</span><span class="sxs-lookup"><span data-stu-id="cafe9-171">*&lt;use the default value&gt;*</span></span>                       |
    | <span data-ttu-id="cafe9-172">**註冊或登入原則**</span><span class="sxs-lookup"><span data-stu-id="cafe9-172">**Sign-up or sign-in policy**</span></span> | `B2C_1_SiUpIn`                                        |
    | <span data-ttu-id="cafe9-173">**重設密碼原則**</span><span class="sxs-lookup"><span data-stu-id="cafe9-173">**Reset password policy**</span></span>     | `B2C_1_SSPR`                                          |
    | <span data-ttu-id="cafe9-174">**編輯設定檔原則**</span><span class="sxs-lookup"><span data-stu-id="cafe9-174">**Edit profile policy**</span></span>       | <span data-ttu-id="cafe9-175">*&lt;保留空白&gt;*</span><span class="sxs-lookup"><span data-stu-id="cafe9-175">*&lt;leave blank&gt;*</span></span>                                 |
    
    <span data-ttu-id="cafe9-176">選取 [**回復 uri** ] 旁的 [**複製**] 連結，將 [回復 uri] 複製到剪貼簿。</span><span class="sxs-lookup"><span data-stu-id="cafe9-176">Select the **Copy** link next to **Reply URI** to copy the Reply URI to the clipboard.</span></span> <span data-ttu-id="cafe9-177">選取 **[確定]** 以關閉 [**變更驗證**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="cafe9-177">Select **OK** to close the **Change Authentication** dialog.</span></span> <span data-ttu-id="cafe9-178">選取 **[確定]** 以建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-178">Select **OK** to create the web app.</span></span>

## <a name="finish-the-b2c-app-registration"></a><span data-ttu-id="cafe9-179">完成 B2C 應用程式註冊</span><span class="sxs-lookup"><span data-stu-id="cafe9-179">Finish the B2C app registration</span></span>

<span data-ttu-id="cafe9-180">返回瀏覽器視窗，其中 B2C 應用程式屬性仍為開啟狀態。</span><span class="sxs-lookup"><span data-stu-id="cafe9-180">Return to the browser window with the B2C app properties still open.</span></span> <span data-ttu-id="cafe9-181">將稍早指定的暫時**回復 URL**變更為從 Visual Studio 複製的值。</span><span class="sxs-lookup"><span data-stu-id="cafe9-181">Change the temporary **Reply URL** specified earlier to the value copied from Visual Studio.</span></span> <span data-ttu-id="cafe9-182">選取視窗頂端的 [**儲存**]。</span><span class="sxs-lookup"><span data-stu-id="cafe9-182">Select **Save** at the top of the window.</span></span>

> [!TIP]
> <span data-ttu-id="cafe9-183">如果您未複製回復 URL，請使用 Web 專案屬性之 [調試] 索引標籤中的 HTTPS 位址，並從*appsettings.js*附加的**CallbackPath**值。</span><span class="sxs-lookup"><span data-stu-id="cafe9-183">If you didn't copy the Reply URL, use the HTTPS address from the Debug tab in the web project properties, and append the **CallbackPath** value from *appsettings.json*.</span></span>

## <a name="configure-policies"></a><span data-ttu-id="cafe9-184">設定原則</span><span class="sxs-lookup"><span data-stu-id="cafe9-184">Configure policies</span></span>

<span data-ttu-id="cafe9-185">使用 Azure AD B2C 檔中的步驟來[建立註冊或登入原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)，然後[建立密碼重設原則](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-185">Use the steps in the Azure AD B2C documentation to [create a sign-up or sign-in policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions), and then [create a password reset policy](/azure/active-directory-b2c/active-directory-b2c-reference-policies#user-flow-versions).</span></span> <span data-ttu-id="cafe9-186">使用\*\* Identity 提供者**、**註冊屬性**和**應用程式宣告\*\*檔中提供的範例值。</span><span class="sxs-lookup"><span data-stu-id="cafe9-186">Use the example values provided in the documentation for **Identity providers**, **Sign-up attributes**, and **Application claims**.</span></span> <span data-ttu-id="cafe9-187">使用 [**立即執行**] 按鈕來測試原則（如檔中所述）是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="cafe9-187">Using the **Run now** button to test the policies as described in the documentation is optional.</span></span>

> [!WARNING]
> <span data-ttu-id="cafe9-188">請確定原則名稱與檔中所述的完全相同，因為 Visual Studio 中的 [**變更驗證**] 對話方塊中使用這些原則。</span><span class="sxs-lookup"><span data-stu-id="cafe9-188">Ensure the policy names are exactly as described in the documentation, as those policies were used in the **Change Authentication** dialog in Visual Studio.</span></span> <span data-ttu-id="cafe9-189">原則名稱可在*appsettings.js*中進行驗證。</span><span class="sxs-lookup"><span data-stu-id="cafe9-189">The policy names can be verified in *appsettings.json*.</span></span>

## <a name="configure-the-underlying-openidconnectoptionsjwtbearerno-loccookie-options"></a><span data-ttu-id="cafe9-190">設定基礎 OpenIdConnectOptions/Microsoft.aspnetcore.authentication.jwtbearer/ Cookie 選項</span><span class="sxs-lookup"><span data-stu-id="cafe9-190">Configure the underlying OpenIdConnectOptions/JwtBearer/Cookie options</span></span>

<span data-ttu-id="cafe9-191">若要直接設定基礎選項，請在中使用適當的配置常數 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="cafe9-191">To configure the underlying options directly, use the appropriate scheme constant in `Startup.ConfigureServices`:</span></span>

```csharp
services.Configure<OpenIdConnectOptions>(
    AzureAD[B2C]Defaults.OpenIdScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<CookieAuthenticationOptions>(
    AzureAD[B2C]Defaults.CookieScheme, options => 
    {
        // Omitted for brevity
    });

services.Configure<JwtBearerOptions>(
    AzureAD[B2C]Defaults.JwtBearerAuthenticationScheme, options => 
    {
        // Omitted for brevity
    });
```

## <a name="run-the-app"></a><span data-ttu-id="cafe9-192">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="cafe9-192">Run the app</span></span>

<span data-ttu-id="cafe9-193">在 Visual Studio 中，按**F5**以建立並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-193">In Visual Studio, press **F5** to build and run the app.</span></span> <span data-ttu-id="cafe9-194">在 web 應用程式啟動後，選取 [**接受**] 以接受使用 cookie s (（如果系統提示) ），然後選取 [登**入**]。</span><span class="sxs-lookup"><span data-stu-id="cafe9-194">After the web app launches, select **Accept** to accept the use of cookies (if prompted), and then select **Sign in**.</span></span>

![登入應用程式](./azure-ad-b2c/_static/signin.png)

<span data-ttu-id="cafe9-196">瀏覽器會重新導向至 Azure AD B2C 的租使用者。</span><span class="sxs-lookup"><span data-stu-id="cafe9-196">The browser redirects to the Azure AD B2C tenant.</span></span> <span data-ttu-id="cafe9-197">使用現有的帳戶登入 (如果已建立測試原則) 或選取 [**立即註冊**] 來建立新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="cafe9-197">Sign in with an existing account (if one was created testing the policies) or select **Sign up now** to create a new account.</span></span> <span data-ttu-id="cafe9-198">[**忘記密碼？** ] 連結是用來重設忘記的密碼。</span><span class="sxs-lookup"><span data-stu-id="cafe9-198">The **Forgot your password?** link is used to reset a forgotten password.</span></span>

![Azure AD B2C 登入](./azure-ad-b2c/_static/b2csts.png)

<span data-ttu-id="cafe9-200">成功登入之後，瀏覽器會重新導向至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-200">After successfully signing in, the browser redirects to the web app.</span></span>

![Success](./azure-ad-b2c/_static/success.png)

## <a name="next-steps"></a><span data-ttu-id="cafe9-202">後續步驟</span><span class="sxs-lookup"><span data-stu-id="cafe9-202">Next steps</span></span>

<span data-ttu-id="cafe9-203">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="cafe9-203">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="cafe9-204">建立 Active Directory B2C 租用戶</span><span class="sxs-lookup"><span data-stu-id="cafe9-204">Create an Azure Active Directory B2C tenant</span></span>
> * <span data-ttu-id="cafe9-205">在 Azure AD B2C 中註冊應用程式</span><span class="sxs-lookup"><span data-stu-id="cafe9-205">Register an app in Azure AD B2C</span></span>
> * <span data-ttu-id="cafe9-206">使用 Visual Studio 建立設定為使用 Azure AD B2C 租使用者進行驗證的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="cafe9-206">Use Visual Studio to create an ASP.NET Core Web Application configured to use the Azure AD B2C tenant for authentication</span></span>
> * <span data-ttu-id="cafe9-207">設定原則來控制 Azure AD B2C 租使用者的行為</span><span class="sxs-lookup"><span data-stu-id="cafe9-207">Configure policies controlling the behavior of the Azure AD B2C tenant</span></span>

<span data-ttu-id="cafe9-208">既然 ASP.NET Core 應用程式已設定為使用 Azure AD B2C 進行驗證，就可以使用[授權屬性](xref:security/authorization/simple)來保護您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="cafe9-208">Now that the ASP.NET Core app is configured to use Azure AD B2C for authentication, the [Authorize attribute](xref:security/authorization/simple) can be used to secure your app.</span></span> <span data-ttu-id="cafe9-209">藉由學習來繼續開發您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="cafe9-209">Continue developing your app by learning to:</span></span>

* <span data-ttu-id="cafe9-210">[自訂 Azure AD B2C 使用者介面](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-210">[Customize the Azure AD B2C user interface](/azure/active-directory-b2c/active-directory-b2c-reference-ui-customization).</span></span>
* <span data-ttu-id="cafe9-211">[設定密碼複雜性需求](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-211">[Configure password complexity requirements](/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity).</span></span>
* <span data-ttu-id="cafe9-212">[啟用多重要素驗證](/azure/active-directory-b2c/active-directory-b2c-reference-mfa)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-212">[Enable multi-factor authentication](/azure/active-directory-b2c/active-directory-b2c-reference-mfa).</span></span>
* <span data-ttu-id="cafe9-213">設定其他身分識別提供者，例如[Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app)、 [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app)、 [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app)、 [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app)、 [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app)等等。</span><span class="sxs-lookup"><span data-stu-id="cafe9-213">Configure additional identity providers, such as [Microsoft](/azure/active-directory-b2c/active-directory-b2c-setup-msa-app), [Facebook](/azure/active-directory-b2c/active-directory-b2c-setup-fb-app), [Google](/azure/active-directory-b2c/active-directory-b2c-setup-goog-app), [Amazon](/azure/active-directory-b2c/active-directory-b2c-setup-amzn-app), [Twitter](/azure/active-directory-b2c/active-directory-b2c-setup-twitter-app), and others.</span></span>
* <span data-ttu-id="cafe9-214">[使用 [Azure AD] 圖形 API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet)從 Azure AD B2C 租使用者中取出額外的使用者資訊，例如群組成員資格。</span><span class="sxs-lookup"><span data-stu-id="cafe9-214">[Use the Azure AD Graph API](/azure/active-directory-b2c/active-directory-b2c-devquickstarts-graph-dotnet) to retrieve additional user information, such as group membership, from the Azure AD B2C tenant.</span></span>
* <span data-ttu-id="cafe9-215">[使用 Azure AD B2C 保護 ASP.NET Core Web API](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-215">[Secure an ASP.NET Core web API using Azure AD B2C](https://azure.microsoft.com/resources/samples/active-directory-b2c-dotnetcore-webapi/).</span></span>
* <span data-ttu-id="cafe9-216">[教學課程：使用 Azure Active Directory B2C 授與 ASP.NET Web API 的存取權](/azure/active-directory-b2c/tutorial-web-api-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="cafe9-216">[Tutorial: Grant access to an ASP.NET web API using Azure Active Directory B2C](/azure/active-directory-b2c/tutorial-web-api-dotnet).</span></span>
