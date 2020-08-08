---
title: ASP.NET Core 中的一般資料保護規定 (GDPR) 支援
author: rick-anderson
description: 瞭解如何存取 ASP.NET Core web 應用程式中的 GDPR 擴充點。
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
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
uid: security/gdpr
ms.openlocfilehash: 6392a22e316f903da18cd1a91d1eb779d8dde1b3
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88020011"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a><span data-ttu-id="74745-103">歐盟一般資料保護規定 (GDPR) 在 ASP.NET Core 中提供支援</span><span class="sxs-lookup"><span data-stu-id="74745-103">EU General Data Protection Regulation (GDPR) support in ASP.NET Core</span></span>

<span data-ttu-id="74745-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74745-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="74745-105">ASP.NET Core 提供 Api 和範本，以協助符合[歐盟一般資料保護規定 (GDPR) ](https://ec.europa.eu/info/law/law-topic/data-protection/reform/what-does-general-data-protection-regulation-gdpr-govern_en)需求：</span><span class="sxs-lookup"><span data-stu-id="74745-105">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://ec.europa.eu/info/law/law-topic/data-protection/reform/what-does-general-data-protection-regulation-gdpr-govern_en) requirements:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="74745-106">專案範本包括擴充點和 stub 標記，您可以將其取代為您的隱私權並 cookie 使用原則。</span><span class="sxs-lookup"><span data-stu-id="74745-106">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="74745-107">[ *Pages/隱私權*] 頁面或 [ *Views/Home/隱私權*] 視圖會提供頁面，以詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="74745-107">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span>

<span data-ttu-id="74745-108">若要啟用預設的 cookie 同意功能，例如在 ASP.NET Core 3.0 範本產生的應用程式中的 ASP.NET Core 2.2 範本中找到：</span><span class="sxs-lookup"><span data-stu-id="74745-108">To enable the default cookie consent feature like that found in the ASP.NET Core 2.2 templates in an ASP.NET Core 3.0 template generated app:</span></span>

* <span data-ttu-id="74745-109">將加入 using 指示詞的 `using Microsoft.AspNetCore.Http` 清單。</span><span class="sxs-lookup"><span data-stu-id="74745-109">Add `using Microsoft.AspNetCore.Http` to the list of using directives.</span></span>
* <span data-ttu-id="74745-110">將[ Cookie PolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)新增至 `Startup.ConfigureServices` ，並[使用 Cookie 原則](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)來 `Startup.Configure` 執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="74745-110">Add [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) to `Startup.ConfigureServices` and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) to `Startup.Configure`:</span></span>

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* <span data-ttu-id="74745-111">將 cookie 同意部分新增至 *_Layout 的 cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="74745-111">Add the cookie consent partial to the *_Layout.cshtml* file:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* <span data-ttu-id="74745-112">將\* \_ Cookie ConsentPartial\*檔案新增至專案：</span><span class="sxs-lookup"><span data-stu-id="74745-112">Add the *\_CookieConsentPartial.cshtml* file to the project:</span></span>

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* <span data-ttu-id="74745-113">請選取此文章的 ASP.NET Core 2.2 版本，以閱讀同意功能的相關資訊 cookie 。</span><span class="sxs-lookup"><span data-stu-id="74745-113">Select the ASP.NET Core 2.2 version of this article to read about the cookie consent feature.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* <span data-ttu-id="74745-114">專案範本包括擴充點和 stub 標記，您可以將其取代為您的隱私權並 cookie 使用原則。</span><span class="sxs-lookup"><span data-stu-id="74745-114">The project templates include extension points and stubbed markup that you can replace with your privacy and cookie use policy.</span></span>
* <span data-ttu-id="74745-115">cookie同意功能可讓您要求 (，並追蹤使用者的) 同意，以儲存個人資訊。</span><span class="sxs-lookup"><span data-stu-id="74745-115">A cookie consent feature allows you to ask for (and track) consent from your users for storing personal information.</span></span> <span data-ttu-id="74745-116">如果使用者尚未同意資料收集，而應用程式已將[CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded)設定為，則不會將 `true` 非必要的 cookie 傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="74745-116">If a user hasn't consented to data collection and the app has [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) set to `true`, non-essential cookies aren't sent to the browser.</span></span>
* <span data-ttu-id="74745-117">Cookie可以標示為必要。</span><span class="sxs-lookup"><span data-stu-id="74745-117">Cookies can be marked as essential.</span></span> <span data-ttu-id="74745-118">重要 cookie 的會傳送至瀏覽器，即使使用者尚未同意，也不會停用追蹤。</span><span class="sxs-lookup"><span data-stu-id="74745-118">Essential cookies are sent to the browser even when the user hasn't consented and tracking is disabled.</span></span>
* <span data-ttu-id="74745-119">停用追蹤時[，TempData 和會話 cookie ](#tempdata)無法正常運作。</span><span class="sxs-lookup"><span data-stu-id="74745-119">[TempData and Session cookies](#tempdata) aren't functional when tracking is disabled.</span></span>
* <span data-ttu-id="74745-120">[ [ Identity 管理](#pd)] 頁面提供下載和刪除使用者資料的連結。</span><span class="sxs-lookup"><span data-stu-id="74745-120">The [Identity manage](#pd) page provides a link to download and delete user data.</span></span>

<span data-ttu-id="74745-121">[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)可讓您測試新增至 ASP.NET Core 2.1 範本的大部分 GDPR 擴充點和 api。</span><span class="sxs-lookup"><span data-stu-id="74745-121">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) allows you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span> <span data-ttu-id="74745-122">如需測試指示，請參閱[自述](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample)檔。</span><span class="sxs-lookup"><span data-stu-id="74745-122">See the [ReadMe](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) file for testing instructions.</span></span>

<span data-ttu-id="74745-123">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="74745-123">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a><span data-ttu-id="74745-124">在範本產生的程式碼中 ASP.NET Core GDPR 支援</span><span class="sxs-lookup"><span data-stu-id="74745-124">ASP.NET Core GDPR support in template-generated code</span></span>

<span data-ttu-id="74745-125">Razor使用專案範本建立的頁面和 MVC 專案包含下列 GDPR 支援：</span><span class="sxs-lookup"><span data-stu-id="74745-125">Razor Pages and MVC projects created with the project templates include the following GDPR support:</span></span>

* <span data-ttu-id="74745-126">[ Cookie PolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)和[使用 Cookie 原則](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)會在類別中設定 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="74745-126">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) and [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) are set in the `Startup` class.</span></span>
* <span data-ttu-id="74745-127">\* \_ Cookie ConsentPartial\*的[部分視圖](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="74745-127">The *\_CookieConsentPartial.cshtml* [partial view](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="74745-128">此檔案包含 [**接受**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="74745-128">An **Accept** button is included in this file.</span></span> <span data-ttu-id="74745-129">當使用者按一下 [**接受**] 按鈕時， cookie 就會提供同意 store。</span><span class="sxs-lookup"><span data-stu-id="74745-129">When the user clicks the **Accept** button, consent to store cookies is provided.</span></span>
* <span data-ttu-id="74745-130">[ *Pages/隱私權*] 頁面或 [ *Views/Home/隱私權*] 視圖會提供頁面，以詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="74745-130">The *Pages/Privacy.cshtml* page or *Views/Home/Privacy.cshtml* view provides a page to detail your site's privacy policy.</span></span> <span data-ttu-id="74745-131">\* \_ Cookie ConsentPartial\*會產生 [隱私權] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="74745-131">The *\_CookieConsentPartial.cshtml* file generates a link to the Privacy page.</span></span>
* <span data-ttu-id="74745-132">針對使用個別使用者帳戶所建立的應用程式，[管理] 頁面會提供下載和刪除[個人使用者資料](#pd)的連結。</span><span class="sxs-lookup"><span data-stu-id="74745-132">For apps created with individual user accounts, the Manage page provides links to download and delete [personal user data](#pd).</span></span>

### <a name="no-loccookiepolicyoptions-and-useno-loccookiepolicy"></a><span data-ttu-id="74745-133">CookiePolicyOptions 和使用 Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="74745-133">CookiePolicyOptions and UseCookiePolicy</span></span>

<span data-ttu-id="74745-134">[ Cookie PolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)在中初始化 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="74745-134">[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) are initialized in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

<span data-ttu-id="74745-135">[使用 Cookie原則](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy)會在中呼叫 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="74745-135">[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) is called in `Startup.Configure`:</span></span>

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="_no-loccookieconsentpartialcshtml-partial-view"></a><span data-ttu-id="74745-136">\_CookieConsentPartial. cshtml 部分視圖</span><span class="sxs-lookup"><span data-stu-id="74745-136">\_CookieConsentPartial.cshtml partial view</span></span>

<span data-ttu-id="74745-137">\* \_ Cookie ConsentPartial\*的部分視圖：</span><span class="sxs-lookup"><span data-stu-id="74745-137">The *\_CookieConsentPartial.cshtml* partial view:</span></span>

[!code-cshtml[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

<span data-ttu-id="74745-138">這部分：</span><span class="sxs-lookup"><span data-stu-id="74745-138">This partial:</span></span>

* <span data-ttu-id="74745-139">取得使用者的追蹤狀態。</span><span class="sxs-lookup"><span data-stu-id="74745-139">Obtains the state of tracking for the user.</span></span> <span data-ttu-id="74745-140">如果應用程式設定為需要同意，則使用者必須同意才能 cookie 追蹤。</span><span class="sxs-lookup"><span data-stu-id="74745-140">If the app is configured to require consent, the user must consent before cookies can be tracked.</span></span> <span data-ttu-id="74745-141">如果需要同意，則 cookie 同意面板會固定在配置\* \_ . cshtml\*檔案所建立的導覽列上方。</span><span class="sxs-lookup"><span data-stu-id="74745-141">If consent is required, the cookie consent panel is fixed at top of the navigation bar created by the *\_Layout.cshtml* file.</span></span>
* <span data-ttu-id="74745-142">提供 HTML `<p>` 元素來總結您的隱私權並 cookie 使用原則。</span><span class="sxs-lookup"><span data-stu-id="74745-142">Provides an HTML `<p>` element to summarize your privacy and cookie use policy.</span></span>
* <span data-ttu-id="74745-143">提供 [隱私權] 頁面或 [流覽] 的連結，您可以在其中詳細說明網站的隱私權原則。</span><span class="sxs-lookup"><span data-stu-id="74745-143">Provides a link to Privacy page or view where you can detail your site's privacy policy.</span></span>

## <a name="essential-no-loccookies"></a><span data-ttu-id="74745-144">基本 cookie</span><span class="sxs-lookup"><span data-stu-id="74745-144">Essential cookies</span></span>

<span data-ttu-id="74745-145">如果尚未提供對存放區 cookie 的同意，只有 cookie 標示為必要的才會傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="74745-145">If consent to store cookies hasn't been provided, only cookies marked essential are sent to the browser.</span></span> <span data-ttu-id="74745-146">下列程式碼會產生 cookie 必要的：</span><span class="sxs-lookup"><span data-stu-id="74745-146">The following code makes a cookie essential:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-no-loccookies-arent-essential"></a><span data-ttu-id="74745-147">TempData 提供者和會話狀態 cookie 不是必要的</span><span class="sxs-lookup"><span data-stu-id="74745-147">TempData provider and session state cookies aren't essential</span></span>

<span data-ttu-id="74745-148">[TempData 提供者](xref:fundamentals/app-state#tempdata) cookie 並不重要。</span><span class="sxs-lookup"><span data-stu-id="74745-148">The [TempData provider](xref:fundamentals/app-state#tempdata) cookie isn't essential.</span></span> <span data-ttu-id="74745-149">如果停用追蹤，TempData 提供者就無法運作。</span><span class="sxs-lookup"><span data-stu-id="74745-149">If tracking is disabled, the TempData provider isn't functional.</span></span> <span data-ttu-id="74745-150">若要在停用追蹤時啟用 TempData 提供者，請 cookie 在中將 TempData 標記為必要項 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="74745-150">To enable the TempData provider when tracking is disabled, mark the TempData cookie as essential in `Startup.ConfigureServices`:</span></span>

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

<span data-ttu-id="74745-151">[會話狀態](xref:fundamentals/app-state) cookies 不是必要的。</span><span class="sxs-lookup"><span data-stu-id="74745-151">[Session state](xref:fundamentals/app-state) cookies are not essential.</span></span> <span data-ttu-id="74745-152">停用追蹤時，會話狀態無法運作。</span><span class="sxs-lookup"><span data-stu-id="74745-152">Session state isn't functional when tracking is disabled.</span></span> <span data-ttu-id="74745-153">下列程式碼會讓會話成為 cookie 必要的：</span><span class="sxs-lookup"><span data-stu-id="74745-153">The following code makes session cookies essential:</span></span>

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a><span data-ttu-id="74745-154">個人資料</span><span class="sxs-lookup"><span data-stu-id="74745-154">Personal data</span></span>

<span data-ttu-id="74745-155">使用個別使用者帳戶所建立 ASP.NET Core 應用程式包含下載和刪除個人資料的程式碼。</span><span class="sxs-lookup"><span data-stu-id="74745-155">ASP.NET Core apps created with individual user accounts include code to download and delete personal data.</span></span>

<span data-ttu-id="74745-156">選取使用者名稱，然後選取 [**個人資料**]：</span><span class="sxs-lookup"><span data-stu-id="74745-156">Select the user name and then select **Personal data**:</span></span>

![管理個人資料頁面](gdpr/_static/pd.png)

<span data-ttu-id="74745-158">注意：</span><span class="sxs-lookup"><span data-stu-id="74745-158">Notes:</span></span>

* <span data-ttu-id="74745-159">若要產生程式 `Account/Manage` 代碼，請參閱[Scaffold Identity ](xref:security/authentication/scaffold-identity)。</span><span class="sxs-lookup"><span data-stu-id="74745-159">To generate the `Account/Manage` code, see [Scaffold Identity](xref:security/authentication/scaffold-identity).</span></span>
* <span data-ttu-id="74745-160">[**刪除**] 和 [**下載**] 連結只會對預設身分識別資料採取行動。</span><span class="sxs-lookup"><span data-stu-id="74745-160">The **Delete** and **Download** links only act on the default identity data.</span></span> <span data-ttu-id="74745-161">建立自訂使用者資料的應用程式必須擴充，才能刪除/下載自訂使用者資料。</span><span class="sxs-lookup"><span data-stu-id="74745-161">Apps that create custom user data must be extended to delete/download the custom user data.</span></span> <span data-ttu-id="74745-162">如需詳細資訊，請參閱[新增、下載和刪除自訂使用者 Identity 資料至](xref:security/authentication/add-user-data)。</span><span class="sxs-lookup"><span data-stu-id="74745-162">For more information, see [Add, download, and delete custom user data to Identity](xref:security/authentication/add-user-data).</span></span>
* <span data-ttu-id="74745-163">Identity `AspNetUserTokens` 當使用者因為[外鍵](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152)而透過串聯刪除行為刪除時，會刪除儲存在資料庫資料表中之使用者的已儲存權杖。</span><span class="sxs-lookup"><span data-stu-id="74745-163">Saved tokens for the user that are stored in the Identity database table `AspNetUserTokens` are deleted when the user is deleted via the cascading delete behavior due to the [foreign key](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).</span></span>
* <span data-ttu-id="74745-164">[外部提供者驗證](xref:security/authentication/social/index)（例如 Facebook 和 Google）在接受原則之前無法使用 cookie 。</span><span class="sxs-lookup"><span data-stu-id="74745-164">[External provider authentication](xref:security/authentication/social/index), such as Facebook and Google, isn't available before the cookie policy is accepted.</span></span>

::: moniker-end

## <a name="encryption-at-rest"></a><span data-ttu-id="74745-165">待用加密</span><span class="sxs-lookup"><span data-stu-id="74745-165">Encryption at rest</span></span>

<span data-ttu-id="74745-166">某些資料庫和儲存機制允許待用加密。</span><span class="sxs-lookup"><span data-stu-id="74745-166">Some databases and storage mechanisms allow for encryption at rest.</span></span> <span data-ttu-id="74745-167">待用加密：</span><span class="sxs-lookup"><span data-stu-id="74745-167">Encryption at rest:</span></span>

* <span data-ttu-id="74745-168">自動加密儲存的資料。</span><span class="sxs-lookup"><span data-stu-id="74745-168">Encrypts stored data automatically.</span></span>
* <span data-ttu-id="74745-169">針對存取資料的軟體，不需要設定、程式設計或其他工作即可進行加密。</span><span class="sxs-lookup"><span data-stu-id="74745-169">Encrypts without configuration, programming, or other work for the software that accesses the data.</span></span>
* <span data-ttu-id="74745-170">是最簡單且最安全的選項。</span><span class="sxs-lookup"><span data-stu-id="74745-170">Is the easiest and safest option.</span></span>
* <span data-ttu-id="74745-171">允許資料庫管理金鑰和加密。</span><span class="sxs-lookup"><span data-stu-id="74745-171">Allows the database to manage keys and encryption.</span></span>

<span data-ttu-id="74745-172">例如：</span><span class="sxs-lookup"><span data-stu-id="74745-172">For example:</span></span>

* <span data-ttu-id="74745-173">Microsoft SQL 和 Azure SQL 提供[透明資料加密](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE) 。</span><span class="sxs-lookup"><span data-stu-id="74745-173">Microsoft SQL and Azure SQL provide [Transparent Data Encryption](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).</span></span>
* [<span data-ttu-id="74745-174">SQL Azure 預設會加密資料庫</span><span class="sxs-lookup"><span data-stu-id="74745-174">SQL Azure encrypts the database by default</span></span>](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* <span data-ttu-id="74745-175">[預設會加密 Azure blob、檔案、資料表和佇列儲存體](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/)。</span><span class="sxs-lookup"><span data-stu-id="74745-175">[Azure Blobs, Files, Table, and Queue Storage are encrypted by default](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).</span></span>

<span data-ttu-id="74745-176">對於未提供內建靜態加密的資料庫，您可以使用磁片加密來提供相同的保護。</span><span class="sxs-lookup"><span data-stu-id="74745-176">For databases that don't provide built-in encryption at rest, you may be able to use disk encryption to provide the same protection.</span></span> <span data-ttu-id="74745-177">例如：</span><span class="sxs-lookup"><span data-stu-id="74745-177">For example:</span></span>

* [<span data-ttu-id="74745-178">適用于 Windows Server 的 BitLocker</span><span class="sxs-lookup"><span data-stu-id="74745-178">BitLocker for Windows Server</span></span>](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* <span data-ttu-id="74745-179">Linux：</span><span class="sxs-lookup"><span data-stu-id="74745-179">Linux:</span></span>
  * [<span data-ttu-id="74745-180">eCryptfs</span><span class="sxs-lookup"><span data-stu-id="74745-180">eCryptfs</span></span>](https://launchpad.net/ecryptfs)
  * <span data-ttu-id="74745-181">[EncFS](https://github.com/vgough/encfs)。</span><span class="sxs-lookup"><span data-stu-id="74745-181">[EncFS](https://github.com/vgough/encfs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="74745-182">其他資源</span><span class="sxs-lookup"><span data-stu-id="74745-182">Additional resources</span></span>

* [<span data-ttu-id="74745-183">Microsoft.com/GDPR</span><span class="sxs-lookup"><span data-stu-id="74745-183">Microsoft.com/GDPR</span></span>](https://www.microsoft.com/trustcenter/Privacy/GDPR)
* <span data-ttu-id="74745-184">[GDPR-在 ASP.NET Core 中新增 [撤銷同意] 按鈕](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)</span><span class="sxs-lookup"><span data-stu-id="74745-184">[GDPR - Adding a Revoke Consent Button in ASP.NET Core](https://www.joeaudette.com/blog/2018/08/28/gdpr---adding-a-revoke-consent-button-in-aspnet-core)</span></span>
