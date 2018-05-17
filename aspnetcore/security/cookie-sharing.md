---
title: 使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie
author: rick-anderson
description: 了解如何共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/19/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/cookie-sharing
ms.openlocfilehash: 5f77377f168993d48686217adac54a75313766ec
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="share-cookies-among-apps-with-aspnet-and-aspnet-core"></a><span data-ttu-id="7bed2-103">使用 ASP.NET 和 ASP.NET Core 共用應用程式之間的 cookie</span><span class="sxs-lookup"><span data-stu-id="7bed2-103">Share cookies among apps with ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="7bed2-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7bed2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="7bed2-105">網站通常包含個別 web 應用程式一起使用。</span><span class="sxs-lookup"><span data-stu-id="7bed2-105">Websites often consist of individual web apps working together.</span></span> <span data-ttu-id="7bed2-106">為提供單一登入 (SSO) 體驗，在站台內的 web 應用程式必須共用的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="7bed2-106">To provide a single sign-on (SSO) experience, web apps within a site must share authentication cookies.</span></span> <span data-ttu-id="7bed2-107">若要支援此案例中，資料保護堆疊可讓共用 Katana 的 cookie 驗證和 ASP.NET Core cookie 驗證票證。</span><span class="sxs-lookup"><span data-stu-id="7bed2-107">To support this scenario, the data protection stack allows sharing Katana cookie authentication and ASP.NET Core cookie authentication tickets.</span></span>

<span data-ttu-id="7bed2-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7bed2-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7bed2-109">範例將說明在使用 cookie 驗證的三個應用程式之間共用的 cookie:</span><span class="sxs-lookup"><span data-stu-id="7bed2-109">The sample illustrates cookie sharing across three apps that use cookie authentication:</span></span>

* <span data-ttu-id="7bed2-110">ASP.NET Core 2.0 Razor 頁面應用程式，而不使用[ASP.NET Core 身分識別](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="7bed2-110">ASP.NET Core 2.0 Razor Pages app without using [ASP.NET Core Identity](xref:security/authentication/identity)</span></span>
* <span data-ttu-id="7bed2-111">使用 ASP.NET Core 身分識別的 ASP.NET Core 2.0 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="7bed2-111">ASP.NET Core 2.0 MVC app with ASP.NET Core Identity</span></span>
* <span data-ttu-id="7bed2-112">使用 ASP.NET Identity 的 ASP.NET Framework 4.6.1 MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="7bed2-112">ASP.NET Framework 4.6.1 MVC app with ASP.NET Identity</span></span>

<span data-ttu-id="7bed2-113">在接下來的範例：</span><span class="sxs-lookup"><span data-stu-id="7bed2-113">In the examples that follow:</span></span>

* <span data-ttu-id="7bed2-114">驗證 cookie 名稱設定的共通的值為`.AspNet.SharedCookie`。</span><span class="sxs-lookup"><span data-stu-id="7bed2-114">The authentication cookie name is set to a common value of `.AspNet.SharedCookie`.</span></span>
* <span data-ttu-id="7bed2-115">`AuthenticationType`設`Identity.Application`明確或預設。</span><span class="sxs-lookup"><span data-stu-id="7bed2-115">The `AuthenticationType` is set to `Identity.Application` either explicitly or by default.</span></span>
* <span data-ttu-id="7bed2-116">常見的應用程式名稱用來啟用共用的資料保護金鑰的資料保護系統 (`SharedCookieApp`)。</span><span class="sxs-lookup"><span data-stu-id="7bed2-116">A common app name is used to enable the data protection system to share data protection keys (`SharedCookieApp`).</span></span>
* <span data-ttu-id="7bed2-117">`Identity.Application` 做為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="7bed2-117">`Identity.Application` is used as the authentication scheme.</span></span> <span data-ttu-id="7bed2-118">會使用任何結構描述，則必須以一致的方式使用*內部與跨*共用的 cookie 應用程式的預設配置，或是藉由明確地設定它。</span><span class="sxs-lookup"><span data-stu-id="7bed2-118">Whatever scheme is used, it must be used consistently *within and across* the shared cookie apps either as the default scheme or by explicitly setting it.</span></span> <span data-ttu-id="7bed2-119">加密和解密 cookie，因此必須使用一致的配置，跨應用程式時，會使用配置。</span><span class="sxs-lookup"><span data-stu-id="7bed2-119">The scheme is used when encrypting and decrypting cookies, so a consistent scheme must be used across apps.</span></span>
* <span data-ttu-id="7bed2-120">一般[資料保護金鑰](xref:security/data-protection/implementation/key-management)會使用儲存體位置。</span><span class="sxs-lookup"><span data-stu-id="7bed2-120">A common [data protection key](xref:security/data-protection/implementation/key-management) storage location is used.</span></span> <span data-ttu-id="7bed2-121">範例應用程式會使用名為資料夾*KeyRing*根目錄中的方案來保存資料保護金鑰。</span><span class="sxs-lookup"><span data-stu-id="7bed2-121">The sample app uses a folder named *KeyRing* at the root of the solution to hold the data protection keys.</span></span>
* <span data-ttu-id="7bed2-122">在 ASP.NET Core 應用程式中， [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)用來設定金鑰的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="7bed2-122">In the ASP.NET Core apps, [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) is used to set the key storage location.</span></span> <span data-ttu-id="7bed2-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)用來設定一般的共用應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="7bed2-123">[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) is used to configure a common shared app name.</span></span>
* <span data-ttu-id="7bed2-124">在.NET Framework 應用程式的 cookie 驗證中介軟體會使用的實作[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)。</span><span class="sxs-lookup"><span data-stu-id="7bed2-124">In the .NET Framework app, the cookie authentication middleware uses an implementation of [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider).</span></span> <span data-ttu-id="7bed2-125">`DataProtectionProvider` 提供的加密和解密的驗證 cookie 裝載資料的資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="7bed2-125">`DataProtectionProvider` provides data protection services for the encryption and decryption of authentication cookie payload data.</span></span> <span data-ttu-id="7bed2-126">`DataProtectionProvider`是分開的應用程式其他部分所使用的資料保護系統的執行個體。</span><span class="sxs-lookup"><span data-stu-id="7bed2-126">The `DataProtectionProvider` instance is isolated from the data protection system used by other parts of the app.</span></span>
  * <span data-ttu-id="7bed2-127">[DataProtectionProvider.Create (System.IO.DirectoryInfo、 動作\<IDataProtectionBuilder >)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__)接受[DirectoryInfo](/dotnet/api/system.io.directoryinfo)來指定資料保護的金鑰存放區的位置。</span><span class="sxs-lookup"><span data-stu-id="7bed2-127">[DataProtectionProvider.Create(System.IO.DirectoryInfo, Action\<IDataProtectionBuilder>)](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider.create?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionProvider_Create_System_IO_DirectoryInfo_System_Action_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder__) accepts a [DirectoryInfo](/dotnet/api/system.io.directoryinfo) to specify the location for data protection key storage.</span></span> <span data-ttu-id="7bed2-128">範例應用程式提供的路徑*KeyRing*資料夾`DirectoryInfo`。</span><span class="sxs-lookup"><span data-stu-id="7bed2-128">The sample app provides the path of the *KeyRing* folder to `DirectoryInfo`.</span></span> <span data-ttu-id="7bed2-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_)設定一般應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="7bed2-129">[DataProtectionBuilderExtensions.SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname?view=aspnetcore-2.0#Microsoft_AspNetCore_DataProtection_DataProtectionBuilderExtensions_SetApplicationName_Microsoft_AspNetCore_DataProtection_IDataProtectionBuilder_System_String_) sets the common app name.</span></span>
  * <span data-ttu-id="7bed2-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider)需要[Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="7bed2-130">[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) requires the [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) NuGet package.</span></span> <span data-ttu-id="7bed2-131">若要取得此套件的 ASP.NET Core 2.0 和更新版本的應用程式，請參考[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage。</span><span class="sxs-lookup"><span data-stu-id="7bed2-131">To obtain this package for ASP.NET Core 2.0 and later apps, reference the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage.</span></span> <span data-ttu-id="7bed2-132">當目標為.NET Framework，加入封裝的參考`Microsoft.AspNetCore.DataProtection.Extensions`。</span><span class="sxs-lookup"><span data-stu-id="7bed2-132">When targeting the .NET Framework, add a package reference to `Microsoft.AspNetCore.DataProtection.Extensions`.</span></span>

## <a name="share-authentication-cookies-among-aspnet-core-apps"></a><span data-ttu-id="7bed2-133">共用 ASP.NET Core 應用程式之間的驗證 cookie</span><span class="sxs-lookup"><span data-stu-id="7bed2-133">Share authentication cookies among ASP.NET Core apps</span></span>

<span data-ttu-id="7bed2-134">當使用 ASP.NET Core 身分識別：</span><span class="sxs-lookup"><span data-stu-id="7bed2-134">When using ASP.NET Core Identity:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bed2-135">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="7bed2-136">在`ConfigureServices`方法，請使用[ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie)擴充方法來設定資料保護服務的 cookie。</span><span class="sxs-lookup"><span data-stu-id="7bed2-136">In the `ConfigureServices` method, use the [ConfigureApplicationCookie](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionextensions.configureapplicationcookie) extension method to set up the data protection service for cookies.</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="7bed2-137">資料保護金鑰，應用程式名稱必須在應用程式之間共用。</span><span class="sxs-lookup"><span data-stu-id="7bed2-137">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="7bed2-138">在範例應用程式中，`GetKeyRingDirInfo`傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。</span><span class="sxs-lookup"><span data-stu-id="7bed2-138">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="7bed2-139">使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。</span><span class="sxs-lookup"><span data-stu-id="7bed2-139">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="7bed2-140">如需詳細資訊，請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="7bed2-140">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span>

<span data-ttu-id="7bed2-141">請參閱*CookieAuthWithIdentity.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7bed2-141">See the *CookieAuthWithIdentity.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bed2-142">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-142">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7bed2-143">在`Configure`方法，請使用[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)設定：</span><span class="sxs-lookup"><span data-stu-id="7bed2-143">In the `Configure` method, use the [CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) to set up:</span></span>

* <span data-ttu-id="7bed2-144">Cookie 資料保護服務。</span><span class="sxs-lookup"><span data-stu-id="7bed2-144">The data protection service for cookies.</span></span>
* <span data-ttu-id="7bed2-145">`AuthenticationScheme`要比對 ASP.NET 4.x。</span><span class="sxs-lookup"><span data-stu-id="7bed2-145">The `AuthenticationScheme` to match ASP.NET 4.x.</span></span>

```csharp
app.AddIdentity<ApplicationUser, IdentityRole>(options =>
{
    options.Cookies.ApplicationCookie.AuthenticationScheme = 
        "ApplicationCookie";

    var protectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"));

    options.Cookies.ApplicationCookie.DataProtectionProvider = 
        protectionProvider;

    options.Cookies.ApplicationCookie.TicketDataFormat = 
        new TicketDataFormat(protectionProvider.CreateProtector(
            "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware", 
            "Cookies", 
            "v2"));
});
```

---

<span data-ttu-id="7bed2-146">當直接使用 cookie:</span><span class="sxs-lookup"><span data-stu-id="7bed2-146">When using cookies directly:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bed2-147">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-147">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuth.Core/Startup.cs?name=snippet1)]

<span data-ttu-id="7bed2-148">資料保護金鑰，應用程式名稱必須在應用程式之間共用。</span><span class="sxs-lookup"><span data-stu-id="7bed2-148">Data protection keys and the app name must be shared among apps.</span></span> <span data-ttu-id="7bed2-149">在範例應用程式中，`GetKeyRingDirInfo`傳回一般的索引鍵的存放位置，以[PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem)方法。</span><span class="sxs-lookup"><span data-stu-id="7bed2-149">In the sample apps, `GetKeyRingDirInfo` returns the common key storage location to the [PersistKeysToFileSystem](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.persistkeystofilesystem) method.</span></span> <span data-ttu-id="7bed2-150">使用[SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname)設定共用的應用程式的一般名稱 (`SharedCookieApp`範例中)。</span><span class="sxs-lookup"><span data-stu-id="7bed2-150">Use [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname) to configure a common shared app name (`SharedCookieApp` in the sample).</span></span> <span data-ttu-id="7bed2-151">如需詳細資訊，請參閱[設定資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="7bed2-151">For more information, see [Configuring Data Protection](xref:security/data-protection/configuration/overview).</span></span> 

<span data-ttu-id="7bed2-152">請參閱*CookieAuth.Core*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7bed2-152">See the *CookieAuth.Core* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bed2-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = 
        DataProtectionProvider.Create(
            new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
});
```

---

## <a name="encrypting-data-protection-keys-at-rest"></a><span data-ttu-id="7bed2-154">加密在靜止的資料保護金鑰</span><span class="sxs-lookup"><span data-stu-id="7bed2-154">Encrypting data protection keys at rest</span></span>

<span data-ttu-id="7bed2-155">針對實際執行部署，設定`DataProtectionProvider`來加密金鑰使用 DPAPI 或 X509Certificate 靜止。</span><span class="sxs-lookup"><span data-stu-id="7bed2-155">For production deployments, configure the `DataProtectionProvider` to encrypt keys at rest with DPAPI or an X509Certificate.</span></span> <span data-ttu-id="7bed2-156">請參閱[金鑰靜態加密](xref:security/data-protection/implementation/key-encryption-at-rest)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7bed2-156">See [Key Encryption At Rest](xref:security/data-protection/implementation/key-encryption-at-rest) for more information.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bed2-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
services.AddDataProtection()
    .ProtectKeysWithCertificate("thumbprint");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bed2-158">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-158">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    DataProtectionProvider = DataProtectionProvider.Create(
        new DirectoryInfo(@"PATH_TO_KEY_RING"),
        configure =>
        {
            configure.ProtectKeysWithCertificate("thumbprint");
        })
});
```

---

## <a name="sharing-authentication-cookies-between-aspnet-4x-and-aspnet-core-apps"></a><span data-ttu-id="7bed2-159">共用的驗證 cookie 之間 ASP.NET 4.x 和 ASP.NET Core 應用程式</span><span class="sxs-lookup"><span data-stu-id="7bed2-159">Sharing authentication cookies between ASP.NET 4.x and ASP.NET Core apps</span></span>

<span data-ttu-id="7bed2-160">使用 Katana cookie 驗證中介軟體的 ASP.NET 4.x 應用程式可以設定為產生相容於 ASP.NET Core cookie 驗證中介軟體的驗證 cookie。</span><span class="sxs-lookup"><span data-stu-id="7bed2-160">ASP.NET 4.x apps which use Katana cookie authentication middleware can be configured to generate authentication cookies that are compatible with the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="7bed2-161">這可讓分次升級大站台的個別的應用程式，同時提供跨網站 smooth SSO 的體驗。</span><span class="sxs-lookup"><span data-stu-id="7bed2-161">This allows upgrading a large site's individual apps piecemeal while providing a smooth SSO experience across the site.</span></span>

> [!TIP]
> <span data-ttu-id="7bed2-162">當應用程式使用 Katana cookie 驗證中介軟體時，它會呼叫`UseCookieAuthentication`在專案的*Startup.Auth.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="7bed2-162">When an app uses Katana cookie authentication middleware, it calls `UseCookieAuthentication` in the project's *Startup.Auth.cs* file.</span></span> <span data-ttu-id="7bed2-163">ASP.NET 4.x web 應用程式專案與 Visual Studio 2013 所建立，然後依預設使用 Katana cookie 驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7bed2-163">ASP.NET 4.x web app projects created with Visual Studio 2013 and later use the Katana cookie authentication middleware by default.</span></span>

> [!NOTE]
> <span data-ttu-id="7bed2-164">ASP.NET 4.x 應用程式必須以.NET Framework 4.5.1 為目標或更高版本。</span><span class="sxs-lookup"><span data-stu-id="7bed2-164">An ASP.NET 4.x app must target .NET Framework 4.5.1 or higher.</span></span> <span data-ttu-id="7bed2-165">否則，無法安裝必要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="7bed2-165">Otherwise, the necessary NuGet packages fail to install.</span></span>

<span data-ttu-id="7bed2-166">若要共用的 ASP.NET 4.x 應用程式和 ASP.NET Core 應用程式之間的驗證 cookie，如上所述，設定 ASP.NET Core 應用程式，然後遵循下列步驟來設定 ASP.NET 4.x 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7bed2-166">To share authentication cookies among ASP.NET 4.x apps and ASP.NET Core apps, configure the ASP.NET Core app as stated above, then configure the ASP.NET 4.x apps by following the steps below.</span></span>

1. <span data-ttu-id="7bed2-167">安裝套件[Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/)到每個 ASP.NET 4.x 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7bed2-167">Install the package [Microsoft.Owin.Security.Interop](https://www.nuget.org/packages/Microsoft.Owin.Security.Interop/) into each ASP.NET 4.x app.</span></span>

2. <span data-ttu-id="7bed2-168">在*Startup.Auth.cs*，找出呼叫`UseCookieAuthentication`並加以修改，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7bed2-168">In *Startup.Auth.cs*, locate the call to `UseCookieAuthentication` and modify it as follows.</span></span> <span data-ttu-id="7bed2-169">變更要與 ASP.NET Core cookie 驗證中介軟體所使用的名稱相符的 cookie 名稱。</span><span class="sxs-lookup"><span data-stu-id="7bed2-169">Change the cookie name to match the name used by the ASP.NET Core cookie authentication middleware.</span></span> <span data-ttu-id="7bed2-170">提供的執行個體`DataProtectionProvider`初始化為一般的資料保護金鑰的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="7bed2-170">Provide an instance of a `DataProtectionProvider` initialized to the common data protection key storage location.</span></span> <span data-ttu-id="7bed2-171">請確定應用程式名稱已設定共用 cookie 的所有應用程式所使用的一般應用程式名稱`SharedCookieApp`範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="7bed2-171">Make sure that the app name is set to the common app name used by all apps that share cookies, `SharedCookieApp` in the sample app.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7bed2-172">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-172">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/App_Start/Startup.Auth.cs?name=snippet1)]

<span data-ttu-id="7bed2-173">請參閱*CookieAuthWithIdentity.NETFramework*專案中[範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="7bed2-173">See the *CookieAuthWithIdentity.NETFramework* project in the [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/cookie-sharing/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="7bed2-174">驗證類型時產生使用者識別時，必須符合中定義的類型`AuthenticationType`設定`UseCookieAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="7bed2-174">When generating a user identity, the authentication type must match the type defined in `AuthenticationType` set with `UseCookieAuthentication`.</span></span>

<span data-ttu-id="7bed2-175">*Models/IdentityModels.cs*:</span><span class="sxs-lookup"><span data-stu-id="7bed2-175">*Models/IdentityModels.cs*:</span></span>

[!code-csharp[](cookie-sharing/sample/CookieAuthWithIdentity.NETFramework/CookieAuthWithIdentity.NETFramework/Models/IdentityModels.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7bed2-176">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7bed2-176">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="7bed2-177">設定`CookieManager`以 interop`ChunkingCookieManager`使區塊格式相容。</span><span class="sxs-lookup"><span data-stu-id="7bed2-177">Set the `CookieManager` to interop `ChunkingCookieManager` so the chunking format is compatible.</span></span>

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    AuthenticationType = DefaultAuthenticationTypes.ApplicationCookie,
    CookieName = ".AspNetCore.Cookies",
    // CookieName = ".AspNetCore.ApplicationCookie", (if using ASP.NET Identity)
    // CookiePath = "...", (if necessary)
    // ...
    TicketDataFormat = new AspNetTicketDataFormat(
        new DataProtectorShim(
            DataProtectionProvider.Create(
                new DirectoryInfo(@"PATH_TO_KEY_RING_FOLDER"))
            .CreateProtector(
                "Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationMiddleware",
                "Cookies", 
                "v2"))),
    CookieManager = new ChunkingCookieManager()
});
```

---

## <a name="use-a-common-user-database"></a><span data-ttu-id="7bed2-178">使用一般的使用者資料庫</span><span class="sxs-lookup"><span data-stu-id="7bed2-178">Use a common user database</span></span>

<span data-ttu-id="7bed2-179">請確認每個應用程式的身分識別系統指向相同的使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="7bed2-179">Confirm that the identity system for each app is pointed at the same user database.</span></span> <span data-ttu-id="7bed2-180">否則，識別系統會產生在執行階段失敗時，它會嘗試比對中針對其資料庫中資訊的驗證 cookie 的資訊。</span><span class="sxs-lookup"><span data-stu-id="7bed2-180">Otherwise, the identity system produces failures at runtime when it attempts to match the information in the authentication cookie against the information in its database.</span></span>
