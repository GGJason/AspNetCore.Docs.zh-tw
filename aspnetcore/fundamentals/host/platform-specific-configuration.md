---
title: 在 ASP.NET Core 中使用裝載啟動組件
author: rick-anderson
description: 了解如何使用 IHostingStartup 實作，從外部組件增強 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
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
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: fadd93cf28603653e20ed6c7dceadcabf0dfb9a5
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88627516"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="366c0-103">在 ASP.NET Core 中使用裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="366c0-104">依 [Pavel Krymets](https://github.com/pakrym)</span><span class="sxs-lookup"><span data-stu-id="366c0-104">By [Pavel Krymets](https://github.com/pakrym)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="366c0-105"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup>裝載啟動) 執行的 (，會在啟動時從外部元件新增應用程式的增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-105">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="366c0-106">例如，外部程式庫可以使用裝載啟動實作，向應用程式提供額外的組態提供者或服務。</span><span class="sxs-lookup"><span data-stu-id="366c0-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="366c0-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="366c0-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="366c0-108">HostingStartup 屬性</span><span class="sxs-lookup"><span data-stu-id="366c0-108">HostingStartup attribute</span></span>

<span data-ttu-id="366c0-109">[HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) 屬性指出裝載啟動組件的出現，於執行階段啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-109">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="366c0-110">系統會自動掃描包含 `Startup` 類別的進入組件或組件是否有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-111">搜尋 `HostingStartup` 屬性的組件清單會在執行階段從 [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey) 中的組態載入。</span><span class="sxs-lookup"><span data-stu-id="366c0-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="366c0-112">要從探索中排除的組件清單會從 [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey) 載入。</span><span class="sxs-lookup"><span data-stu-id="366c0-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span>

<span data-ttu-id="366c0-113">在下列範例中，裝載啟動組件的命名空間為 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="366c0-113">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="366c0-114">包含裝載啟動程式碼的類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="366c0-114">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="366c0-115">`HostingStartup` 屬性通常位於裝載啟動組件的 `IHostingStartup` 實作類別檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-115">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="366c0-116">探索載入的裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-116">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="366c0-117">若要探索載入的裝載啟動組件，請啟用記錄並檢查應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="366c0-117">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="366c0-118">系統會記錄載入組件時發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="366c0-118">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="366c0-119">將會在偵錯層級記錄載入的裝載啟動組件，並記錄所有錯誤。</span><span class="sxs-lookup"><span data-stu-id="366c0-119">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="366c0-120">停用自動載入裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-120">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="366c0-121">若要停用自動載入裝載啟動組件，請使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="366c0-121">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="366c0-122">若要防止載入所有裝載啟動組件，請將下列任一項目設為 `true` 或 `1`：</span><span class="sxs-lookup"><span data-stu-id="366c0-122">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>

  * <span data-ttu-id="366c0-123">防止裝載啟動主機設定設定：</span><span class="sxs-lookup"><span data-stu-id="366c0-123">Prevent Hosting Startup host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.PreventHostingStartupKey, "true")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="366c0-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-124">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

* <span data-ttu-id="366c0-125">若要防止載入特定的裝載啟動組件，請將下列任一項目設為以分號分隔的裝載啟動組件字串，藉此於啟動時排除：</span><span class="sxs-lookup"><span data-stu-id="366c0-125">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>

  * <span data-ttu-id="366c0-126">裝載啟動排除元件主機配置設定：</span><span class="sxs-lookup"><span data-stu-id="366c0-126">Hosting Startup Exclude Assemblies host configuration setting:</span></span>

    ```csharp
    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseSetting(
                        WebHostDefaults.HostingStartupExcludeAssembliesKey, 
                        "{ASSEMBLY1;ASSEMBLY2; ...}")
                    .UseStartup<Startup>();
            });
    ```

  * <span data-ttu-id="366c0-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-127">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="366c0-128">如已設定主機組態設定和環境變數，主機設定即會控制行為。</span><span class="sxs-lookup"><span data-stu-id="366c0-128">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="366c0-129">使用主機設定或環境變數停用裝載啟動組件會停用全域的組件，並且可能會停用數項應用程式特性。</span><span class="sxs-lookup"><span data-stu-id="366c0-129">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="366c0-130">Project</span><span class="sxs-lookup"><span data-stu-id="366c0-130">Project</span></span>

<span data-ttu-id="366c0-131">以下列兩種專案類型的任一類型建立裝載啟動：</span><span class="sxs-lookup"><span data-stu-id="366c0-131">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="366c0-132">類別庫</span><span class="sxs-lookup"><span data-stu-id="366c0-132">Class library</span></span>](#class-library)
* [<span data-ttu-id="366c0-133">沒有進入點的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="366c0-133">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="366c0-134">類別庫</span><span class="sxs-lookup"><span data-stu-id="366c0-134">Class library</span></span>

<span data-ttu-id="366c0-135">類別庫可以提供裝載啟動的增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-135">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="366c0-136">此程式庫包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-136">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="366c0-137">[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)包含 Razor 頁面應用程式、 *HostingStartupApp*，以及類別庫 *>hostingstartuplibrary*。</span><span class="sxs-lookup"><span data-stu-id="366c0-137">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="366c0-138">類別庫：</span><span class="sxs-lookup"><span data-stu-id="366c0-138">The class library:</span></span>

* <span data-ttu-id="366c0-139">包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="366c0-139">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="366c0-140">`ServiceKeyInjection` 使用記憶體內部組態提供者 ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*))，將成對的服務字串新增至應用程式的組態。</span><span class="sxs-lookup"><span data-stu-id="366c0-140">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="366c0-141">包含 `HostingStartup` 屬性，可識別裝載啟動的命名空間和類別。</span><span class="sxs-lookup"><span data-stu-id="366c0-141">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="366c0-142">`ServiceKeyInjection`類別的 <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> 方法會使用將 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 增強功能新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-142">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="366c0-143">*HostingStartupLibrary/ServiceKeyInjection.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-143">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="366c0-144">應用程式的 [索引] 頁面會讀取並呈現類別庫裝載啟動組件所設定之兩個索引鍵的組態值：</span><span class="sxs-lookup"><span data-stu-id="366c0-144">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="366c0-145">*HostingStartupApp/Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-145">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="366c0-146">[程式碼範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)也包含提供另一個主機啟動 (*HostingStartupPackage*) 的 NuGet 套件專案。</span><span class="sxs-lookup"><span data-stu-id="366c0-146">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="366c0-147">此套件特性與前文所述之類別庫相同。</span><span class="sxs-lookup"><span data-stu-id="366c0-147">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="366c0-148">套件：</span><span class="sxs-lookup"><span data-stu-id="366c0-148">The package:</span></span>

* <span data-ttu-id="366c0-149">包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="366c0-149">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="366c0-150">`ServiceKeyInjection` 將成對的服務字串新增到應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="366c0-150">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="366c0-151">包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-151">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="366c0-152">*HostingStartupPackage/ServiceKeyInjection.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-152">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="366c0-153">應用程式的 [索引] 頁面會讀取並呈現套件裝載啟動組件所設定之兩個索引鍵的組態值：</span><span class="sxs-lookup"><span data-stu-id="366c0-153">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="366c0-154">*HostingStartupApp/Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-154">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/3.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="366c0-155">沒有進入點的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="366c0-155">Console app without an entry point</span></span>

<span data-ttu-id="366c0-156">這個方法僅適用於 .NET Core 應用程式，不適用於 .NET Framework。\*\*</span><span class="sxs-lookup"><span data-stu-id="366c0-156">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="366c0-157">在沒有包含 `HostingStartup` 屬性之進入點的主控台應用程式中，可以提供不需要啟用編譯時間參考的動態裝載啟動增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-157">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-158">發佈主控台應用程式會產生裝載啟動組件，這些組件可從執行階段存放區取用。</span><span class="sxs-lookup"><span data-stu-id="366c0-158">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="366c0-159">沒有進入點的主控台應用程式會在此程序中使用，因為：</span><span class="sxs-lookup"><span data-stu-id="366c0-159">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="366c0-160">需要相依性檔案才能取用裝載啟動組件中的裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-160">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="366c0-161">相依性檔案是可執行的應用程式資產，這是透過發佈應用程式 (而非程式庫) 所產生。</span><span class="sxs-lookup"><span data-stu-id="366c0-161">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="366c0-162">程式庫無法直接新增至[執行階段套件存放區](/dotnet/core/deploying/runtime-store)，這需要以共用執行階段為目標的可執行專案。</span><span class="sxs-lookup"><span data-stu-id="366c0-162">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="366c0-163">在建立動態裝載啟動階段中：</span><span class="sxs-lookup"><span data-stu-id="366c0-163">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="366c0-164">會從沒有進入點的主控台應用程式建立裝載啟動組件：</span><span class="sxs-lookup"><span data-stu-id="366c0-164">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="366c0-165">包括包含 `IHostingStartup` 實作的類別。</span><span class="sxs-lookup"><span data-stu-id="366c0-165">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="366c0-166">包括 [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) 屬性以識別 `IHostingStartup` 實作類別。</span><span class="sxs-lookup"><span data-stu-id="366c0-166">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="366c0-167">發佈主控台應用程式以取得裝載啟動的相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-167">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="366c0-168">發佈主控台應用程式的結果是從相依性檔案中刪減未使用的相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-168">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="366c0-169">相依性檔案已修改以設定裝載啟動組件的執行階段位置。</span><span class="sxs-lookup"><span data-stu-id="366c0-169">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="366c0-170">裝載啟動組件與其相依性檔案會放入執行階段套件存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-170">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="366c0-171">若要探索裝載啟動組件與其相依性檔案，兩者會在成對的環境變數中列出。</span><span class="sxs-lookup"><span data-stu-id="366c0-171">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="366c0-172">主控台應用程式會參考 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) 套件：</span><span class="sxs-lookup"><span data-stu-id="366c0-172">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.csproj)]

<span data-ttu-id="366c0-173">[HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute)屬性會將類別識別為 `IHostingStartup` 在建立時載入和執行的實作為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 。</span><span class="sxs-lookup"><span data-stu-id="366c0-173">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="366c0-174">在下列範例中，命名空間為 `StartupEnhancement`，而類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="366c0-174">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="366c0-175">類別會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="366c0-175">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="366c0-176">類別的 <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> 方法會使用將 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 增強功能新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-176">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="366c0-177">執行階段會先呼叫裝載啟動組件中的 `IHostingStartup.Configure`，再呼叫使用者程式碼中的 `Startup.Configure`，這可讓使用者程式碼覆寫裝載啟動組件提供的所有組態。</span><span class="sxs-lookup"><span data-stu-id="366c0-177">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="366c0-178">建置 `IHostingStartup` 專案時，相依性檔案 (*.deps.json*) 會將組件的 `runtime` 位置設定為 *bin* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="366c0-178">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/3.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="366c0-179">僅顯示部分檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-179">Only part of the file is shown.</span></span> <span data-ttu-id="366c0-180">範例中的組件名稱是 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="366c0-180">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="366c0-181">由裝載啟動所提供的設定</span><span class="sxs-lookup"><span data-stu-id="366c0-181">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="366c0-182">視您是否要讓裝載啟動設定的優先順序高於應用程式的設定，有兩種方式可用來處理設定：</span><span class="sxs-lookup"><span data-stu-id="366c0-182">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="366c0-183">在應用程式的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 委派執行之後使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 載入設定，以提供設定給應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-183">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="366c0-184">使用此方法時，裝載啟動設定的優先順序高於應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="366c0-184">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="366c0-185">在應用程式的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 委派執行之前使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 載入設定，以提供設定給應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-185">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="366c0-186">使用此方法時，應用程式設定值的優先順序高於裝載啟動程式所提供的設定值。</span><span class="sxs-lookup"><span data-stu-id="366c0-186">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="366c0-187">指定裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-187">Specify the hosting startup assembly</span></span>

<span data-ttu-id="366c0-188">為類別庫 (或主控台應用程式) 提供的裝載啟動，在 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數中指定裝載啟動組件的名稱。</span><span class="sxs-lookup"><span data-stu-id="366c0-188">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="366c0-189">環境變數是以分號分隔的組件清單。</span><span class="sxs-lookup"><span data-stu-id="366c0-189">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="366c0-190">只掃描裝載啟動組件是否有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-190">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-191">範例應用程式 *HostingStartupApp* 若要探索前文所述的裝載啟動，環境變數要設定為下列值：</span><span class="sxs-lookup"><span data-stu-id="366c0-191">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="366c0-192">您也可以使用裝載啟動元件主機設定設定來設定裝載啟動元件：</span><span class="sxs-lookup"><span data-stu-id="366c0-192">A hosting startup assembly can also be set using the Hosting Startup Assemblies host configuration setting:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseSetting(
                    WebHostDefaults.HostingStartupAssembliesKey, 
                    "{ASSEMBLY1;ASSEMBLY2; ...}")
                .UseStartup<Startup>();
        });
```

<span data-ttu-id="366c0-193">當有多個裝載啟動組合時， <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> 會依列出元件的循序執行其方法。</span><span class="sxs-lookup"><span data-stu-id="366c0-193">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="366c0-194">啟用</span><span class="sxs-lookup"><span data-stu-id="366c0-194">Activation</span></span>

<span data-ttu-id="366c0-195">裝載啟動啟用的選項如下：</span><span class="sxs-lookup"><span data-stu-id="366c0-195">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="366c0-196">[執行時間存放區](#runtime-store)：啟用時，不需要編譯時間參考。</span><span class="sxs-lookup"><span data-stu-id="366c0-196">[Runtime store](#runtime-store): Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="366c0-197">範例應用程式會將裝載啟動組件和相依性檔案放入 *deployment* 資料夾，以利在多機器環境中部署裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-197">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="366c0-198">*deployment* 資料夾也包含 PowerShell 指令碼，可在部署系統上建立或修改環境變數，以啟用裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-198">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="366c0-199">啟用所需之編譯時間參考</span><span class="sxs-lookup"><span data-stu-id="366c0-199">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="366c0-200">Nuget 套件</span><span class="sxs-lookup"><span data-stu-id="366c0-200">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="366c0-201">專案 bin 資料夾</span><span class="sxs-lookup"><span data-stu-id="366c0-201">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="366c0-202">執行階段存放區</span><span class="sxs-lookup"><span data-stu-id="366c0-202">Runtime store</span></span>

<span data-ttu-id="366c0-203">裝載啟動實作置於[執行階段存放區](/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="366c0-203">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="366c0-204">增強的應用程式不需要組件的編譯時間參考。</span><span class="sxs-lookup"><span data-stu-id="366c0-204">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="366c0-205">建置裝載啟動之後，會使用資訊清單專案檔與 [dotnet store](/dotnet/core/tools/dotnet-store) 命令來產生執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-205">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="366c0-206">在範例應用程式 (*RuntimeStore* 專案) 中，我們使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="366c0-206">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="366c0-207">為讓執行階段探索執行階段存放區，執行階段存放區的位置會新增到 `DOTNET_SHARED_STORE` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-207">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="366c0-208">**修改並放置裝載啟動的相依性檔案**</span><span class="sxs-lookup"><span data-stu-id="366c0-208">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="366c0-209">若要在沒有對加強功能之套件參考的情況下啟用加強功能，請使用 `additionalDeps` 指定對執行階段的額外相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-209">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="366c0-210">`additionalDeps` 可讓您：</span><span class="sxs-lookup"><span data-stu-id="366c0-210">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="366c0-211">透過提供一組額外的 *.deps.json* 檔案在啟動時與應用程式的自有 *.deps.json* 檔案合併，以延伸應用程式的程式庫圖表。</span><span class="sxs-lookup"><span data-stu-id="366c0-211">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="366c0-212">讓裝載啟動組件可供探索且可載入。</span><span class="sxs-lookup"><span data-stu-id="366c0-212">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="366c0-213">用於產生額外相依性的建議方法是：</span><span class="sxs-lookup"><span data-stu-id="366c0-213">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="366c0-214">在上一節參考的執行階段存放區資訊清單檔上執行 `dotnet publish`。</span><span class="sxs-lookup"><span data-stu-id="366c0-214">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="366c0-215">移除程式庫中的資訊清單參考，以及檔案 `runtime` 產生的 *.deps.js* 區段。</span><span class="sxs-lookup"><span data-stu-id="366c0-215">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="366c0-216">在範例專案中，`store.manifest/1.0.0` 屬已從 `targets` 與 `libraries` 區段移除：</span><span class="sxs-lookup"><span data-stu-id="366c0-216">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v3.0",
    "signature": ""
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v3.0": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp3.0/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-xrhzuNSyM5/f4ZswhooJ9dmIYLP64wMnqUJSyTKVDKDVj5T+qtzypl8JmM/aFJLLpYrf0FYpVWvGujd7/FfMEw==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="366c0-217">將 *.deps.json* 檔案放到下列位置：</span><span class="sxs-lookup"><span data-stu-id="366c0-217">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="366c0-218">`{ADDITIONAL DEPENDENCIES PATH}`：新增至 `DOTNET_ADDITIONAL_DEPS` 環境變數的位置。</span><span class="sxs-lookup"><span data-stu-id="366c0-218">`{ADDITIONAL DEPENDENCIES PATH}`: Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="366c0-219">`{SHARED FRAMEWORK NAME}`：這個額外相依性檔案需要共用架構。</span><span class="sxs-lookup"><span data-stu-id="366c0-219">`{SHARED FRAMEWORK NAME}`: Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="366c0-220">`{SHARED FRAMEWORK VERSION}`：共用 framework 的最小版本。</span><span class="sxs-lookup"><span data-stu-id="366c0-220">`{SHARED FRAMEWORK VERSION}`: Minimum shared framework version.</span></span>
* <span data-ttu-id="366c0-221">`{ENHANCEMENT ASSEMBLY NAME}`：增強的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="366c0-221">`{ENHANCEMENT ASSEMBLY NAME}`: The enhancement's assembly name.</span></span>

<span data-ttu-id="366c0-222">在範例應用程式 (*RuntimeStore* 專案) 中，額外的相依性檔案是放到下列位置：</span><span class="sxs-lookup"><span data-stu-id="366c0-222">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/3.0.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="366c0-223">為讓執行階段探索執行階段存放區，額外的相依性檔案位置會新增到 `DOTNET_ADDITIONAL_DEPS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-223">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="366c0-224">在範例應用程式 (*RuntimeStore* 專案) 中，建置執行階段存放區並產生額外的相依性檔案是透過使用 [PowerShell](/powershell/scripting/powershell-scripting) 指令碼來完成的。</span><span class="sxs-lookup"><span data-stu-id="366c0-224">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="366c0-225">如需如何為各種作業系統設定環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="366c0-225">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="366c0-226">**部署**</span><span class="sxs-lookup"><span data-stu-id="366c0-226">**Deployment**</span></span>

<span data-ttu-id="366c0-227">為利於在多機器環境中部署裝載啟動，範例應用程式會在包含下列項目的發佈輸出中建立 *deployment* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="366c0-227">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="366c0-228">裝載啟動執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-228">The hosting startup runtime store.</span></span>
* <span data-ttu-id="366c0-229">裝載啟動相依性檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-229">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="366c0-230">建立或修改 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`、`DOTNET_SHARED_STORE` 與 `DOTNET_ADDITIONAL_DEPS` 以支援啟用裝載啟動的 PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-230">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="366c0-231">在部署系統上，從系統管理 PowerShell 命令提示字元執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-231">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="366c0-232">Nuget 套件</span><span class="sxs-lookup"><span data-stu-id="366c0-232">NuGet package</span></span>

<span data-ttu-id="366c0-233">NuGet 套件可以提供裝載啟動的增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-233">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="366c0-234">此套件具有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-234">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-235">使用下列兩種方法的任一方法，套件提供的裝載啟動類型即可提供應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="366c0-235">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="366c0-236">增強應用程式的專案檔會在應用程式專案檔 (編譯時間參考) 中建立裝載啟動的套件參考。</span><span class="sxs-lookup"><span data-stu-id="366c0-236">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="366c0-237">當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*.deps.json*)。</span><span class="sxs-lookup"><span data-stu-id="366c0-237">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="366c0-238">這種方法適用於發佈至 [nuget.org](https://www.nuget.org/) 的裝載啟動組件套件。</span><span class="sxs-lookup"><span data-stu-id="366c0-238">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="366c0-239">裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-239">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="366c0-240">如需有關 NuGet 套件和執行階段存放區的詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="366c0-240">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="366c0-241">如何使用跨平台工具建立 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="366c0-241">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="366c0-242">發佈套件</span><span class="sxs-lookup"><span data-stu-id="366c0-242">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="366c0-243">執行階段套件存放區</span><span class="sxs-lookup"><span data-stu-id="366c0-243">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="366c0-244">專案 bin 資料夾</span><span class="sxs-lookup"><span data-stu-id="366c0-244">Project bin folder</span></span>

<span data-ttu-id="366c0-245">部署在增強應用程式 *bin* 下的組件可提供裝載啟動增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-245">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="366c0-246">您可以使用下列任一種方法，讓應用程式能夠使用組件所提供的裝載啟動類型：</span><span class="sxs-lookup"><span data-stu-id="366c0-246">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="366c0-247">增強應用程式的專案檔會建立裝載啟動的組件參考 (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-247">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="366c0-248">當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*.deps.json*)。</span><span class="sxs-lookup"><span data-stu-id="366c0-248">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="366c0-249">當部署案例呼叫以建立裝載啟動組件 (*.dll* 檔案) 的編譯時間參考，並將組件移至下列其中一項時，適用此方法：</span><span class="sxs-lookup"><span data-stu-id="366c0-249">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="366c0-250">取用專案。</span><span class="sxs-lookup"><span data-stu-id="366c0-250">The consuming project.</span></span>
  * <span data-ttu-id="366c0-251">取用專案可存取的位置。</span><span class="sxs-lookup"><span data-stu-id="366c0-251">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="366c0-252">裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-252">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="366c0-253">以 .NET Framework 為目標時，可在預設載入內容中載入組件，在 .NET Framework 上，這表示組件位於下列其中一個位置：</span><span class="sxs-lookup"><span data-stu-id="366c0-253">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="366c0-254">應用程式基底路徑：應用程式可執行檔 (*.exe*) 所在的*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="366c0-254">Application base path: The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="366c0-255">全域組件快取 (GAC) ： GAC 會儲存數個 .NET Framework apps 共用的元件。</span><span class="sxs-lookup"><span data-stu-id="366c0-255">Global Assembly Cache (GAC): The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="366c0-256">如需詳細資訊，請參閱 .NET Framework 檔中的 [如何：將元件安裝到全域組件快取](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) 。</span><span class="sxs-lookup"><span data-stu-id="366c0-256">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="366c0-257">範例程式碼</span><span class="sxs-lookup"><span data-stu-id="366c0-257">Sample code</span></span>

<span data-ttu-id="366c0-258">[程式碼範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([如何下載](xref:index#how-to-download-a-sample)) 示範裝載啟動實作案例：</span><span class="sxs-lookup"><span data-stu-id="366c0-258">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="366c0-259">兩個裝載啟動組件 (類別程式庫) 各設定一對記憶體內部組態索引鍵/值組：</span><span class="sxs-lookup"><span data-stu-id="366c0-259">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="366c0-260">NuGet 套件 (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="366c0-260">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="366c0-261">類別庫 (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="366c0-261">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="366c0-262">從部署在執行階段存放區的組件啟動裝載啟動 (*StartupDiagnostics*)。</span><span class="sxs-lookup"><span data-stu-id="366c0-262">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="366c0-263">此組件會在啟動時將兩個中介軟體新增至應用程式，以提供診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="366c0-263">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="366c0-264">已註冊服務</span><span class="sxs-lookup"><span data-stu-id="366c0-264">Registered services</span></span>
  * <span data-ttu-id="366c0-265">位址 (配置、主機、基底路徑、路徑、查詢字串)</span><span class="sxs-lookup"><span data-stu-id="366c0-265">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="366c0-266">連線 (遠端 IP、遠端連接埠、本機 IP、本機連接埠、用戶端憑證)</span><span class="sxs-lookup"><span data-stu-id="366c0-266">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="366c0-267">要求標頭</span><span class="sxs-lookup"><span data-stu-id="366c0-267">Request headers</span></span>
  * <span data-ttu-id="366c0-268">環境變數</span><span class="sxs-lookup"><span data-stu-id="366c0-268">Environment variables</span></span>

<span data-ttu-id="366c0-269">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="366c0-269">To run the sample:</span></span>

<span data-ttu-id="366c0-270">**從 NuGet 套件啟用**</span><span class="sxs-lookup"><span data-stu-id="366c0-270">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="366c0-271">使用 [dotnet pack](/dotnet/core/tools/dotnet-pack) 命令編譯 *HostingStartupPackage* 套件。</span><span class="sxs-lookup"><span data-stu-id="366c0-271">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="366c0-272">將 *HostingStartupPackage* 的套件組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-272">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="366c0-273">編譯和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-273">Compile and run the app.</span></span> <span data-ttu-id="366c0-274">增強的應用程式中有套件參考 (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-274">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="366c0-275">應用程式專案檔中的 `<PropertyGroup>` 會指定套件專案的輸出 (*../HostingStartupPackage/bin/Debug*) 為套件來源。</span><span class="sxs-lookup"><span data-stu-id="366c0-275">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="366c0-276">這可讓應用程式在不將封裝上傳至 [nuget.org](https://www.nuget.org/)的情況下使用套件。如需詳細資訊，請參閱 HostingStartupApp 的專案檔案中的附注。</span><span class="sxs-lookup"><span data-stu-id="366c0-276">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="366c0-277">觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合套件之 `ServiceKeyInjection.Configure` 方法設定的值。</span><span class="sxs-lookup"><span data-stu-id="366c0-277">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="366c0-278">如果您變更並重新編譯 *HostingStartupPackage* 專案，請清除本機的 NuGet 套件快取，以確保 *HostingStartupApp* 從本機快取接收更新的套件，而非過時的套件。</span><span class="sxs-lookup"><span data-stu-id="366c0-278">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="366c0-279">若要清除本機 NuGet 快取，請執行下列 [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) 命令：</span><span class="sxs-lookup"><span data-stu-id="366c0-279">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="366c0-280">**從類別庫啟用**</span><span class="sxs-lookup"><span data-stu-id="366c0-280">**Activation from a class library**</span></span>

1. <span data-ttu-id="366c0-281">使用 [dotnet build](/dotnet/core/tools/dotnet-build) 命令編譯 *HostingStartupLibrary* 類別庫。</span><span class="sxs-lookup"><span data-stu-id="366c0-281">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="366c0-282">將 *HostingStartupLibrary* 的類別庫組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-282">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="366c0-283">將 *HostingStartupLibrary.dll* 檔案從類別庫的編譯輸出複製到應用程式的 *bin/Debug* 資料夾，在應用程式的 *bin* 下部署類別庫組件。</span><span class="sxs-lookup"><span data-stu-id="366c0-283">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="366c0-284">編譯和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-284">Compile and run the app.</span></span> <span data-ttu-id="366c0-285">`<ItemGroup>`應用程式專案檔中的會參考類別庫的元件 (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*)  (編譯時期參考) 。</span><span class="sxs-lookup"><span data-stu-id="366c0-285">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="366c0-286">如需詳細資訊，請參閱 HostingStartupApp 專案檔中的資訊。</span><span class="sxs-lookup"><span data-stu-id="366c0-286">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp3.0\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp3.0\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion> 
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="366c0-287">觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合類別庫之 `ServiceKeyInjection.Configure` 方法所設定的值。</span><span class="sxs-lookup"><span data-stu-id="366c0-287">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="366c0-288">**從部署在執行階段存放區的組件啟用**</span><span class="sxs-lookup"><span data-stu-id="366c0-288">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="366c0-289">*StartupDiagnostics* 專案使用 [PowerShell](/powershell/scripting/powershell-scripting) 修改其 *StartupDiagnostics.deps.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-289">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="366c0-290">從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 開始，會在 Windows 上預設安裝 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="366c0-290">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="366c0-291">若要在其他平臺上取得 PowerShell，請參閱 [安裝各種版本的 powershell](/powershell/scripting/install/installing-powershell)。</span><span class="sxs-lookup"><span data-stu-id="366c0-291">To obtain PowerShell on other platforms, see [Installing various versions of PowerShell](/powershell/scripting/install/installing-powershell).</span></span>
1. <span data-ttu-id="366c0-292">執行 *RuntimeStore* 資料夾中的 *build.ps1* 指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-292">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="366c0-293">此指令碼會：</span><span class="sxs-lookup"><span data-stu-id="366c0-293">The script:</span></span>
   * <span data-ttu-id="366c0-294">`StartupDiagnostics`在*obj\packages*資料夾中產生封裝。</span><span class="sxs-lookup"><span data-stu-id="366c0-294">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="366c0-295">在 *store* 資料夾中產生 `StartupDiagnostics` 的執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-295">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="366c0-296">指令碼中的 `dotnet store` 命令會使用  的 `win7-x64` [runtime identifier (RID) (執行階段識別碼 (RID))](/dotnet/core/rid-catalog) 作為部署至 Windows 的裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-296">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="366c0-297">為不同的執行階段提供裝載啟動時，請在指令碼的行 37 上替換成正確的 RID。</span><span class="sxs-lookup"><span data-stu-id="366c0-297">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="366c0-298">稍後的執行時間存放區將會 `StartupDiagnostics` 移至將取用元件之電腦上的使用者或系統的執行時間存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-298">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="366c0-299">元件的使用者執行時間存放區安裝位置 `StartupDiagnostics` 為 *. dotnet/store/x64/netcoreapp 3.0/>startupdiagnostics.deps.json/1.0.0/lib/netcoreapp 3.0/StartupDiagnostics.dll*。</span><span class="sxs-lookup"><span data-stu-id="366c0-299">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp3.0/startupdiagnostics/1.0.0/lib/netcoreapp3.0/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="366c0-300">`additionalDeps` `StartupDiagnostics` 在 *>additionaldeps*資料夾中產生的。</span><span class="sxs-lookup"><span data-stu-id="366c0-300">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="366c0-301">其他相依性稍後將移至使用者或系統的其他相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-301">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="366c0-302">使用者的 `StartupDiagnostics` 其他相依性安裝位置為 *. dotnet/X64/>additionaldeps/>startupdiagnostics.deps.json/Shared/NETCore. App/3.0.0/StartupDiagnostics.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="366c0-302">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/3.0.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="366c0-303">將 *deploy.ps1* 檔案置於 *deployment* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="366c0-303">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="366c0-304">執行 *deployment* 資料夾中的 *deploy.ps1* 指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-304">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="366c0-305">該指令碼會附加至：</span><span class="sxs-lookup"><span data-stu-id="366c0-305">The script appends:</span></span>
   * <span data-ttu-id="366c0-306">`StartupDiagnostics` 至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-306">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="366c0-307">>runtimestore 專案的 *部署* 資料夾中的裝載啟動相依性路徑 () 至 `DOTNET_ADDITIONAL_DEPS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-307">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="366c0-308">>runtimestore 專案的 *部署* 資料夾中的執行時間存放區路徑 () 至 `DOTNET_SHARED_STORE` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-308">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="366c0-309">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-309">Run the sample app.</span></span>
1. <span data-ttu-id="366c0-310">要求 `/services` 端點來查看應用程式的註冊服務。</span><span class="sxs-lookup"><span data-stu-id="366c0-310">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="366c0-311">要求 `/diag` 端點來查看診斷資訊。</span><span class="sxs-lookup"><span data-stu-id="366c0-311">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="366c0-312"><xref:Microsoft.AspNetCore.Hosting.IHostingStartup>裝載啟動) 執行的 (，會在啟動時從外部元件新增應用程式的增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-312">An <xref:Microsoft.AspNetCore.Hosting.IHostingStartup> (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="366c0-313">例如，外部程式庫可以使用裝載啟動實作，向應用程式提供額外的組態提供者或服務。</span><span class="sxs-lookup"><span data-stu-id="366c0-313">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="366c0-314">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="366c0-314">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="366c0-315">HostingStartup 屬性</span><span class="sxs-lookup"><span data-stu-id="366c0-315">HostingStartup attribute</span></span>

<span data-ttu-id="366c0-316">[HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) 屬性指出裝載啟動組件的出現，於執行階段啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-316">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="366c0-317">系統會自動掃描包含 `Startup` 類別的進入組件或組件是否有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-317">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-318">搜尋 `HostingStartup` 屬性的組件清單會在執行階段從 [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey) 中的組態載入。</span><span class="sxs-lookup"><span data-stu-id="366c0-318">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupAssembliesKey).</span></span> <span data-ttu-id="366c0-319">要從探索中排除的組件清單會從 [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey) 載入。</span><span class="sxs-lookup"><span data-stu-id="366c0-319">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](xref:Microsoft.AspNetCore.Hosting.WebHostDefaults.HostingStartupExcludeAssembliesKey).</span></span> <span data-ttu-id="366c0-320">如需詳細資訊，請參閱 [Web 主機：裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)和 [Web 主機：裝載啟動排除組件](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)。</span><span class="sxs-lookup"><span data-stu-id="366c0-320">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="366c0-321">在下列範例中，裝載啟動組件的命名空間為 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="366c0-321">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="366c0-322">包含裝載啟動程式碼的類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="366c0-322">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="366c0-323">`HostingStartup` 屬性通常位於裝載啟動組件的 `IHostingStartup` 實作類別檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-323">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="366c0-324">探索載入的裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-324">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="366c0-325">若要探索載入的裝載啟動組件，請啟用記錄並檢查應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="366c0-325">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="366c0-326">系統會記錄載入組件時發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="366c0-326">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="366c0-327">將會在偵錯層級記錄載入的裝載啟動組件，並記錄所有錯誤。</span><span class="sxs-lookup"><span data-stu-id="366c0-327">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="366c0-328">停用自動載入裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-328">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="366c0-329">若要停用自動載入裝載啟動組件，請使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="366c0-329">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="366c0-330">若要防止載入所有裝載啟動組件，請將下列任一項目設為 `true` 或 `1`：</span><span class="sxs-lookup"><span data-stu-id="366c0-330">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="366c0-331">[防止裝載啟動](xref:fundamentals/host/web-host#prevent-hosting-startup)主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="366c0-331">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="366c0-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-332">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="366c0-333">若要防止載入特定的裝載啟動組件，請將下列任一項目設為以分號分隔的裝載啟動組件字串，藉此於啟動時排除：</span><span class="sxs-lookup"><span data-stu-id="366c0-333">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="366c0-334">[裝載啟動排除組件](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="366c0-334">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="366c0-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-335">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="366c0-336">如已設定主機組態設定和環境變數，主機設定即會控制行為。</span><span class="sxs-lookup"><span data-stu-id="366c0-336">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="366c0-337">使用主機設定或環境變數停用裝載啟動組件會停用全域的組件，並且可能會停用數項應用程式特性。</span><span class="sxs-lookup"><span data-stu-id="366c0-337">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="366c0-338">Project</span><span class="sxs-lookup"><span data-stu-id="366c0-338">Project</span></span>

<span data-ttu-id="366c0-339">以下列兩種專案類型的任一類型建立裝載啟動：</span><span class="sxs-lookup"><span data-stu-id="366c0-339">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="366c0-340">類別庫</span><span class="sxs-lookup"><span data-stu-id="366c0-340">Class library</span></span>](#class-library)
* [<span data-ttu-id="366c0-341">沒有進入點的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="366c0-341">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="366c0-342">類別庫</span><span class="sxs-lookup"><span data-stu-id="366c0-342">Class library</span></span>

<span data-ttu-id="366c0-343">類別庫可以提供裝載啟動的增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-343">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="366c0-344">此程式庫包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-344">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="366c0-345">[範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)包含 Razor 頁面應用程式、 *HostingStartupApp*，以及類別庫 *>hostingstartuplibrary*。</span><span class="sxs-lookup"><span data-stu-id="366c0-345">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="366c0-346">類別庫：</span><span class="sxs-lookup"><span data-stu-id="366c0-346">The class library:</span></span>

* <span data-ttu-id="366c0-347">包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="366c0-347">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="366c0-348">`ServiceKeyInjection` 使用記憶體內部組態提供者 ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*))，將成對的服務字串新增至應用程式的組態。</span><span class="sxs-lookup"><span data-stu-id="366c0-348">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection*)).</span></span>
* <span data-ttu-id="366c0-349">包含 `HostingStartup` 屬性，可識別裝載啟動的命名空間和類別。</span><span class="sxs-lookup"><span data-stu-id="366c0-349">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="366c0-350">`ServiceKeyInjection`類別的 <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> 方法會使用將 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 增強功能新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-350">The `ServiceKeyInjection` class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span>

<span data-ttu-id="366c0-351">*HostingStartupLibrary/ServiceKeyInjection.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-351">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="366c0-352">應用程式的 [索引] 頁面會讀取並呈現類別庫裝載啟動組件所設定之兩個索引鍵的組態值：</span><span class="sxs-lookup"><span data-stu-id="366c0-352">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="366c0-353">*HostingStartupApp/Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-353">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="366c0-354">[程式碼範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)也包含提供另一個主機啟動 (*HostingStartupPackage*) 的 NuGet 套件專案。</span><span class="sxs-lookup"><span data-stu-id="366c0-354">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="366c0-355">此套件特性與前文所述之類別庫相同。</span><span class="sxs-lookup"><span data-stu-id="366c0-355">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="366c0-356">套件：</span><span class="sxs-lookup"><span data-stu-id="366c0-356">The package:</span></span>

* <span data-ttu-id="366c0-357">包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="366c0-357">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="366c0-358">`ServiceKeyInjection` 將成對的服務字串新增到應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="366c0-358">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="366c0-359">包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-359">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="366c0-360">*HostingStartupPackage/ServiceKeyInjection.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-360">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="366c0-361">應用程式的 [索引] 頁面會讀取並呈現套件裝載啟動組件所設定之兩個索引鍵的組態值：</span><span class="sxs-lookup"><span data-stu-id="366c0-361">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="366c0-362">*HostingStartupApp/Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="366c0-362">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="366c0-363">沒有進入點的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="366c0-363">Console app without an entry point</span></span>

<span data-ttu-id="366c0-364">這個方法僅適用於 .NET Core 應用程式，不適用於 .NET Framework。\*\*</span><span class="sxs-lookup"><span data-stu-id="366c0-364">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="366c0-365">在沒有包含 `HostingStartup` 屬性之進入點的主控台應用程式中，可以提供不需要啟用編譯時間參考的動態裝載啟動增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-365">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-366">發佈主控台應用程式會產生裝載啟動組件，這些組件可從執行階段存放區取用。</span><span class="sxs-lookup"><span data-stu-id="366c0-366">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="366c0-367">沒有進入點的主控台應用程式會在此程序中使用，因為：</span><span class="sxs-lookup"><span data-stu-id="366c0-367">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="366c0-368">需要相依性檔案才能取用裝載啟動組件中的裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-368">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="366c0-369">相依性檔案是可執行的應用程式資產，這是透過發佈應用程式 (而非程式庫) 所產生。</span><span class="sxs-lookup"><span data-stu-id="366c0-369">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="366c0-370">程式庫無法直接新增至[執行階段套件存放區](/dotnet/core/deploying/runtime-store)，這需要以共用執行階段為目標的可執行專案。</span><span class="sxs-lookup"><span data-stu-id="366c0-370">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="366c0-371">在建立動態裝載啟動階段中：</span><span class="sxs-lookup"><span data-stu-id="366c0-371">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="366c0-372">會從沒有進入點的主控台應用程式建立裝載啟動組件：</span><span class="sxs-lookup"><span data-stu-id="366c0-372">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="366c0-373">包括包含 `IHostingStartup` 實作的類別。</span><span class="sxs-lookup"><span data-stu-id="366c0-373">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="366c0-374">包括 [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) 屬性以識別 `IHostingStartup` 實作類別。</span><span class="sxs-lookup"><span data-stu-id="366c0-374">Includes a [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="366c0-375">發佈主控台應用程式以取得裝載啟動的相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-375">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="366c0-376">發佈主控台應用程式的結果是從相依性檔案中刪減未使用的相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-376">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="366c0-377">相依性檔案已修改以設定裝載啟動組件的執行階段位置。</span><span class="sxs-lookup"><span data-stu-id="366c0-377">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="366c0-378">裝載啟動組件與其相依性檔案會放入執行階段套件存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-378">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="366c0-379">若要探索裝載啟動組件與其相依性檔案，兩者會在成對的環境變數中列出。</span><span class="sxs-lookup"><span data-stu-id="366c0-379">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="366c0-380">主控台應用程式會參考 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) 套件：</span><span class="sxs-lookup"><span data-stu-id="366c0-380">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="366c0-381">[HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute)屬性會將類別識別為 `IHostingStartup` 在建立時載入和執行的實作為 <xref:Microsoft.AspNetCore.Hosting.IWebHost> 。</span><span class="sxs-lookup"><span data-stu-id="366c0-381">A [HostingStartup](xref:Microsoft.AspNetCore.Hosting.HostingStartupAttribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the <xref:Microsoft.AspNetCore.Hosting.IWebHost>.</span></span> <span data-ttu-id="366c0-382">在下列範例中，命名空間為 `StartupEnhancement`，而類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="366c0-382">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="366c0-383">類別會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="366c0-383">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="366c0-384">類別的 <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> 方法會使用將 <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> 增強功能新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-384">The class's <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> method uses an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> to add enhancements to an app.</span></span> <span data-ttu-id="366c0-385">執行階段會先呼叫裝載啟動組件中的 `IHostingStartup.Configure`，再呼叫使用者程式碼中的 `Startup.Configure`，這可讓使用者程式碼覆寫裝載啟動組件提供的所有組態。</span><span class="sxs-lookup"><span data-stu-id="366c0-385">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="366c0-386">建置 `IHostingStartup` 專案時，相依性檔案 (*.deps.json*) 會將組件的 `runtime` 位置設定為 *bin* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="366c0-386">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="366c0-387">僅顯示部分檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-387">Only part of the file is shown.</span></span> <span data-ttu-id="366c0-388">範例中的組件名稱是 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="366c0-388">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="366c0-389">由裝載啟動所提供的設定</span><span class="sxs-lookup"><span data-stu-id="366c0-389">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="366c0-390">視您是否要讓裝載啟動設定的優先順序高於應用程式的設定，有兩種方式可用來處理設定：</span><span class="sxs-lookup"><span data-stu-id="366c0-390">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="366c0-391">在應用程式的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 委派執行之後使用 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 載入設定，以提供設定給應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-391">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="366c0-392">使用此方法時，裝載啟動設定的優先順序高於應用程式的設定。</span><span class="sxs-lookup"><span data-stu-id="366c0-392">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="366c0-393">在應用程式的 <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> 委派執行之前使用 <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> 載入設定，以提供設定給應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-393">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="366c0-394">使用此方法時，應用程式設定值的優先順序高於裝載啟動程式所提供的設定值。</span><span class="sxs-lookup"><span data-stu-id="366c0-394">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority " +
                    "than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority " +
                "than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="366c0-395">指定裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="366c0-395">Specify the hosting startup assembly</span></span>

<span data-ttu-id="366c0-396">為類別庫 (或主控台應用程式) 提供的裝載啟動，在 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數中指定裝載啟動組件的名稱。</span><span class="sxs-lookup"><span data-stu-id="366c0-396">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="366c0-397">環境變數是以分號分隔的組件清單。</span><span class="sxs-lookup"><span data-stu-id="366c0-397">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="366c0-398">只掃描裝載啟動組件是否有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-398">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-399">範例應用程式 *HostingStartupApp* 若要探索前文所述的裝載啟動，環境變數要設定為下列值：</span><span class="sxs-lookup"><span data-stu-id="366c0-399">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="366c0-400">您也可以使用[裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)的主機組態設定來設定裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="366c0-400">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="366c0-401">當有多個裝載啟動組合時， <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> 會依列出元件的循序執行其方法。</span><span class="sxs-lookup"><span data-stu-id="366c0-401">When multiple hosting startup assembles are present, their <xref:Microsoft.AspNetCore.Hosting.IHostingStartup.Configure*> methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="366c0-402">啟用</span><span class="sxs-lookup"><span data-stu-id="366c0-402">Activation</span></span>

<span data-ttu-id="366c0-403">裝載啟動啟用的選項如下：</span><span class="sxs-lookup"><span data-stu-id="366c0-403">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="366c0-404">[執行時間存放區](#runtime-store)：啟用時，不需要編譯時間參考。</span><span class="sxs-lookup"><span data-stu-id="366c0-404">[Runtime store](#runtime-store): Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="366c0-405">範例應用程式會將裝載啟動組件和相依性檔案放入 *deployment* 資料夾，以利在多機器環境中部署裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-405">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="366c0-406">*deployment* 資料夾也包含 PowerShell 指令碼，可在部署系統上建立或修改環境變數，以啟用裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-406">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="366c0-407">啟用所需之編譯時間參考</span><span class="sxs-lookup"><span data-stu-id="366c0-407">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="366c0-408">Nuget 套件</span><span class="sxs-lookup"><span data-stu-id="366c0-408">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="366c0-409">專案 bin 資料夾</span><span class="sxs-lookup"><span data-stu-id="366c0-409">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="366c0-410">執行階段存放區</span><span class="sxs-lookup"><span data-stu-id="366c0-410">Runtime store</span></span>

<span data-ttu-id="366c0-411">裝載啟動實作置於[執行階段存放區](/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="366c0-411">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="366c0-412">增強的應用程式不需要組件的編譯時間參考。</span><span class="sxs-lookup"><span data-stu-id="366c0-412">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="366c0-413">建置裝載啟動之後，會使用資訊清單專案檔與 [dotnet store](/dotnet/core/tools/dotnet-store) 命令來產生執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-413">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```dotnetcli
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="366c0-414">在範例應用程式 (*RuntimeStore* 專案) 中，我們使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="366c0-414">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

```dotnetcli
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="366c0-415">為讓執行階段探索執行階段存放區，執行階段存放區的位置會新增到 `DOTNET_SHARED_STORE` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-415">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="366c0-416">**修改並放置裝載啟動的相依性檔案**</span><span class="sxs-lookup"><span data-stu-id="366c0-416">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="366c0-417">若要在沒有對加強功能之套件參考的情況下啟用加強功能，請使用 `additionalDeps` 指定對執行階段的額外相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-417">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="366c0-418">`additionalDeps` 可讓您：</span><span class="sxs-lookup"><span data-stu-id="366c0-418">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="366c0-419">透過提供一組額外的 *.deps.json* 檔案在啟動時與應用程式的自有 *.deps.json* 檔案合併，以延伸應用程式的程式庫圖表。</span><span class="sxs-lookup"><span data-stu-id="366c0-419">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="366c0-420">讓裝載啟動組件可供探索且可載入。</span><span class="sxs-lookup"><span data-stu-id="366c0-420">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="366c0-421">用於產生額外相依性的建議方法是：</span><span class="sxs-lookup"><span data-stu-id="366c0-421">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="366c0-422">在上一節參考的執行階段存放區資訊清單檔上執行 `dotnet publish`。</span><span class="sxs-lookup"><span data-stu-id="366c0-422">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="366c0-423">移除程式庫中的資訊清單參考，以及檔案 `runtime` 產生的 *.deps.js* 區段。</span><span class="sxs-lookup"><span data-stu-id="366c0-423">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="366c0-424">在範例專案中，`store.manifest/1.0.0` 屬已從 `targets` 與 `libraries` 區段移除：</span><span class="sxs-lookup"><span data-stu-id="366c0-424">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="366c0-425">將 *.deps.json* 檔案放到下列位置：</span><span class="sxs-lookup"><span data-stu-id="366c0-425">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="366c0-426">`{ADDITIONAL DEPENDENCIES PATH}`：新增至 `DOTNET_ADDITIONAL_DEPS` 環境變數的位置。</span><span class="sxs-lookup"><span data-stu-id="366c0-426">`{ADDITIONAL DEPENDENCIES PATH}`: Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="366c0-427">`{SHARED FRAMEWORK NAME}`：這個額外相依性檔案需要共用架構。</span><span class="sxs-lookup"><span data-stu-id="366c0-427">`{SHARED FRAMEWORK NAME}`: Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="366c0-428">`{SHARED FRAMEWORK VERSION}`：共用 framework 的最小版本。</span><span class="sxs-lookup"><span data-stu-id="366c0-428">`{SHARED FRAMEWORK VERSION}`: Minimum shared framework version.</span></span>
* <span data-ttu-id="366c0-429">`{ENHANCEMENT ASSEMBLY NAME}`：增強的元件名稱。</span><span class="sxs-lookup"><span data-stu-id="366c0-429">`{ENHANCEMENT ASSEMBLY NAME}`: The enhancement's assembly name.</span></span>

<span data-ttu-id="366c0-430">在範例應用程式 (*RuntimeStore* 專案) 中，額外的相依性檔案是放到下列位置：</span><span class="sxs-lookup"><span data-stu-id="366c0-430">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
deployment/additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="366c0-431">為讓執行階段探索執行階段存放區，額外的相依性檔案位置會新增到 `DOTNET_ADDITIONAL_DEPS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-431">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="366c0-432">在範例應用程式 (*RuntimeStore* 專案) 中，建置執行階段存放區並產生額外的相依性檔案是透過使用 [PowerShell](/powershell/scripting/powershell-scripting) 指令碼來完成的。</span><span class="sxs-lookup"><span data-stu-id="366c0-432">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="366c0-433">如需如何為各種作業系統設定環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="366c0-433">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="366c0-434">**部署**</span><span class="sxs-lookup"><span data-stu-id="366c0-434">**Deployment**</span></span>

<span data-ttu-id="366c0-435">為利於在多機器環境中部署裝載啟動，範例應用程式會在包含下列項目的發佈輸出中建立 *deployment* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="366c0-435">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="366c0-436">裝載啟動執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-436">The hosting startup runtime store.</span></span>
* <span data-ttu-id="366c0-437">裝載啟動相依性檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-437">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="366c0-438">建立或修改 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`、`DOTNET_SHARED_STORE` 與 `DOTNET_ADDITIONAL_DEPS` 以支援啟用裝載啟動的 PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-438">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="366c0-439">在部署系統上，從系統管理 PowerShell 命令提示字元執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-439">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="366c0-440">Nuget 套件</span><span class="sxs-lookup"><span data-stu-id="366c0-440">NuGet package</span></span>

<span data-ttu-id="366c0-441">NuGet 套件可以提供裝載啟動的增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-441">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="366c0-442">此套件具有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="366c0-442">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="366c0-443">使用下列兩種方法的任一方法，套件提供的裝載啟動類型即可提供應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="366c0-443">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="366c0-444">增強應用程式的專案檔會在應用程式專案檔 (編譯時間參考) 中建立裝載啟動的套件參考。</span><span class="sxs-lookup"><span data-stu-id="366c0-444">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="366c0-445">當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*.deps.json*)。</span><span class="sxs-lookup"><span data-stu-id="366c0-445">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="366c0-446">這種方法適用於發佈至 [nuget.org](https://www.nuget.org/) 的裝載啟動組件套件。</span><span class="sxs-lookup"><span data-stu-id="366c0-446">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="366c0-447">裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-447">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="366c0-448">如需有關 NuGet 套件和執行階段存放區的詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="366c0-448">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="366c0-449">如何使用跨平台工具建立 NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="366c0-449">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="366c0-450">發佈套件</span><span class="sxs-lookup"><span data-stu-id="366c0-450">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="366c0-451">執行階段套件存放區</span><span class="sxs-lookup"><span data-stu-id="366c0-451">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="366c0-452">專案 bin 資料夾</span><span class="sxs-lookup"><span data-stu-id="366c0-452">Project bin folder</span></span>

<span data-ttu-id="366c0-453">部署在增強應用程式 *bin* 下的組件可提供裝載啟動增強功能。</span><span class="sxs-lookup"><span data-stu-id="366c0-453">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="366c0-454">您可以使用下列任一種方法，讓應用程式能夠使用組件所提供的裝載啟動類型：</span><span class="sxs-lookup"><span data-stu-id="366c0-454">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="366c0-455">增強應用程式的專案檔會建立裝載啟動的組件參考 (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-455">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="366c0-456">當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*.deps.json*)。</span><span class="sxs-lookup"><span data-stu-id="366c0-456">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="366c0-457">當部署案例呼叫以建立裝載啟動組件 (*.dll* 檔案) 的編譯時間參考，並將組件移至下列其中一項時，適用此方法：</span><span class="sxs-lookup"><span data-stu-id="366c0-457">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="366c0-458">取用專案。</span><span class="sxs-lookup"><span data-stu-id="366c0-458">The consuming project.</span></span>
  * <span data-ttu-id="366c0-459">取用專案可存取的位置。</span><span class="sxs-lookup"><span data-stu-id="366c0-459">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="366c0-460">裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-460">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="366c0-461">以 .NET Framework 為目標時，可在預設載入內容中載入組件，在 .NET Framework 上，這表示組件位於下列其中一個位置：</span><span class="sxs-lookup"><span data-stu-id="366c0-461">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="366c0-462">應用程式基底路徑：應用程式可執行檔 (*.exe*) 所在的*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="366c0-462">Application base path: The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="366c0-463">全域組件快取 (GAC) ： GAC 會儲存數個 .NET Framework apps 共用的元件。</span><span class="sxs-lookup"><span data-stu-id="366c0-463">Global Assembly Cache (GAC): The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="366c0-464">如需詳細資訊，請參閱 .NET Framework 檔中的 [如何：將元件安裝到全域組件快取](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) 。</span><span class="sxs-lookup"><span data-stu-id="366c0-464">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="366c0-465">範例程式碼</span><span class="sxs-lookup"><span data-stu-id="366c0-465">Sample code</span></span>

<span data-ttu-id="366c0-466">[程式碼範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([如何下載](xref:index#how-to-download-a-sample)) 示範裝載啟動實作案例：</span><span class="sxs-lookup"><span data-stu-id="366c0-466">The [sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="366c0-467">兩個裝載啟動組件 (類別程式庫) 各設定一對記憶體內部組態索引鍵/值組：</span><span class="sxs-lookup"><span data-stu-id="366c0-467">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="366c0-468">NuGet 套件 (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="366c0-468">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="366c0-469">類別庫 (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="366c0-469">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="366c0-470">從部署在執行階段存放區的組件啟動裝載啟動 (*StartupDiagnostics*)。</span><span class="sxs-lookup"><span data-stu-id="366c0-470">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="366c0-471">此組件會在啟動時將兩個中介軟體新增至應用程式，以提供診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="366c0-471">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="366c0-472">已註冊服務</span><span class="sxs-lookup"><span data-stu-id="366c0-472">Registered services</span></span>
  * <span data-ttu-id="366c0-473">位址 (配置、主機、基底路徑、路徑、查詢字串)</span><span class="sxs-lookup"><span data-stu-id="366c0-473">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="366c0-474">連線 (遠端 IP、遠端連接埠、本機 IP、本機連接埠、用戶端憑證)</span><span class="sxs-lookup"><span data-stu-id="366c0-474">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="366c0-475">要求標頭</span><span class="sxs-lookup"><span data-stu-id="366c0-475">Request headers</span></span>
  * <span data-ttu-id="366c0-476">環境變數</span><span class="sxs-lookup"><span data-stu-id="366c0-476">Environment variables</span></span>

<span data-ttu-id="366c0-477">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="366c0-477">To run the sample:</span></span>

<span data-ttu-id="366c0-478">**從 NuGet 套件啟用**</span><span class="sxs-lookup"><span data-stu-id="366c0-478">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="366c0-479">使用 [dotnet pack](/dotnet/core/tools/dotnet-pack) 命令編譯 *HostingStartupPackage* 套件。</span><span class="sxs-lookup"><span data-stu-id="366c0-479">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="366c0-480">將 *HostingStartupPackage* 的套件組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-480">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="366c0-481">編譯和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-481">Compile and run the app.</span></span> <span data-ttu-id="366c0-482">增強的應用程式中有套件參考 (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-482">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="366c0-483">應用程式專案檔中的 `<PropertyGroup>` 會指定套件專案的輸出 (*../HostingStartupPackage/bin/Debug*) 為套件來源。</span><span class="sxs-lookup"><span data-stu-id="366c0-483">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="366c0-484">這可讓應用程式在不將封裝上傳至 [nuget.org](https://www.nuget.org/)的情況下使用套件。如需詳細資訊，請參閱 HostingStartupApp 的專案檔案中的附注。</span><span class="sxs-lookup"><span data-stu-id="366c0-484">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="366c0-485">觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合套件之 `ServiceKeyInjection.Configure` 方法設定的值。</span><span class="sxs-lookup"><span data-stu-id="366c0-485">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="366c0-486">如果您變更並重新編譯 *HostingStartupPackage* 專案，請清除本機的 NuGet 套件快取，以確保 *HostingStartupApp* 從本機快取接收更新的套件，而非過時的套件。</span><span class="sxs-lookup"><span data-stu-id="366c0-486">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="366c0-487">若要清除本機 NuGet 快取，請執行下列 [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) 命令：</span><span class="sxs-lookup"><span data-stu-id="366c0-487">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```dotnetcli
dotnet nuget locals all --clear
```

<span data-ttu-id="366c0-488">**從類別庫啟用**</span><span class="sxs-lookup"><span data-stu-id="366c0-488">**Activation from a class library**</span></span>

1. <span data-ttu-id="366c0-489">使用 [dotnet build](/dotnet/core/tools/dotnet-build) 命令編譯 *HostingStartupLibrary* 類別庫。</span><span class="sxs-lookup"><span data-stu-id="366c0-489">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="366c0-490">將 *HostingStartupLibrary* 的類別庫組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-490">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="366c0-491">將 *HostingStartupLibrary.dll* 檔案從類別庫的編譯輸出複製到應用程式的 *bin/Debug* 資料夾，在應用程式的 *bin* 下部署類別庫組件。</span><span class="sxs-lookup"><span data-stu-id="366c0-491">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="366c0-492">編譯和執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-492">Compile and run the app.</span></span> <span data-ttu-id="366c0-493">應用程式專案檔中的 `<ItemGroup>` 參考類別庫的組件 (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="366c0-493">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="366c0-494">如需詳細資訊，請參閱 HostingStartupApp 專案檔中的資訊。</span><span class="sxs-lookup"><span data-stu-id="366c0-494">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\\bin\\Debug\\netcoreapp2.1\\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="366c0-495">觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合類別庫之 `ServiceKeyInjection.Configure` 方法所設定的值。</span><span class="sxs-lookup"><span data-stu-id="366c0-495">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="366c0-496">**從部署在執行階段存放區的組件啟用**</span><span class="sxs-lookup"><span data-stu-id="366c0-496">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="366c0-497">*StartupDiagnostics* 專案使用 [PowerShell](/powershell/scripting/powershell-scripting) 修改其 *StartupDiagnostics.deps.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="366c0-497">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="366c0-498">從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 開始，會在 Windows 上預設安裝 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="366c0-498">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="366c0-499">若要在其他平臺上取得 PowerShell，請參閱 [安裝各種版本的 powershell](/powershell/scripting/install/installing-powershell)。</span><span class="sxs-lookup"><span data-stu-id="366c0-499">To obtain PowerShell on other platforms, see [Installing various versions of PowerShell](/powershell/scripting/install/installing-powershell).</span></span>
1. <span data-ttu-id="366c0-500">執行 *RuntimeStore* 資料夾中的 *build.ps1* 指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-500">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="366c0-501">此指令碼會：</span><span class="sxs-lookup"><span data-stu-id="366c0-501">The script:</span></span>
   * <span data-ttu-id="366c0-502">`StartupDiagnostics`在*obj\packages*資料夾中產生封裝。</span><span class="sxs-lookup"><span data-stu-id="366c0-502">Generates the `StartupDiagnostics` package in the *obj\packages* folder.</span></span>
   * <span data-ttu-id="366c0-503">在 *store* 資料夾中產生 `StartupDiagnostics` 的執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-503">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="366c0-504">指令碼中的 `dotnet store` 命令會使用  的 `win7-x64` [runtime identifier (RID) (執行階段識別碼 (RID))](/dotnet/core/rid-catalog) 作為部署至 Windows 的裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="366c0-504">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="366c0-505">為不同的執行階段提供裝載啟動時，請在指令碼的行 37 上替換成正確的 RID。</span><span class="sxs-lookup"><span data-stu-id="366c0-505">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span> <span data-ttu-id="366c0-506">稍後的執行時間存放區將會 `StartupDiagnostics` 移至將取用元件之電腦上的使用者或系統的執行時間存放區。</span><span class="sxs-lookup"><span data-stu-id="366c0-506">The runtime store for `StartupDiagnostics` would later be moved to the user's or system's runtime store on the machine where the assembly will be consumed.</span></span> <span data-ttu-id="366c0-507">元件的使用者執行時間存放區安裝位置 `StartupDiagnostics` 為 *. dotnet/store/x64/netcoreapp 2.2/>startupdiagnostics.deps.json/1.0.0/lib/netcoreapp 2.2/StartupDiagnostics.dll*。</span><span class="sxs-lookup"><span data-stu-id="366c0-507">The user runtime store install location for the `StartupDiagnostics` assembly is *.dotnet/store/x64/netcoreapp2.2/startupdiagnostics/1.0.0/lib/netcoreapp2.2/StartupDiagnostics.dll*.</span></span>
   * <span data-ttu-id="366c0-508">`additionalDeps` `StartupDiagnostics` 在 *>additionaldeps*資料夾中產生的。</span><span class="sxs-lookup"><span data-stu-id="366c0-508">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps* folder.</span></span> <span data-ttu-id="366c0-509">其他相依性稍後將移至使用者或系統的其他相依性。</span><span class="sxs-lookup"><span data-stu-id="366c0-509">The additional dependencies would later be moved to the user's or system's additional dependencies.</span></span> <span data-ttu-id="366c0-510">使用者的 `StartupDiagnostics` 其他相依性安裝位置為 *. dotnet/X64/>additionaldeps/>startupdiagnostics.deps.json/Shared/NETCore. App/2.2.0/StartupDiagnostics.deps.json*。</span><span class="sxs-lookup"><span data-stu-id="366c0-510">The user `StartupDiagnostics` additional dependencies install location is *.dotnet/x64/additionalDeps/StartupDiagnostics/shared/Microsoft.NETCore.App/2.2.0/StartupDiagnostics.deps.json*.</span></span>
   * <span data-ttu-id="366c0-511">將 *deploy.ps1* 檔案置於 *deployment* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="366c0-511">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="366c0-512">執行 *deployment* 資料夾中的 *deploy.ps1* 指令碼。</span><span class="sxs-lookup"><span data-stu-id="366c0-512">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="366c0-513">該指令碼會附加至：</span><span class="sxs-lookup"><span data-stu-id="366c0-513">The script appends:</span></span>
   * <span data-ttu-id="366c0-514">`StartupDiagnostics` 至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-514">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="366c0-515">>runtimestore 專案的 *部署* 資料夾中的裝載啟動相依性路徑 () 至 `DOTNET_ADDITIONAL_DEPS` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-515">The hosting startup dependencies path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="366c0-516">>runtimestore 專案的 *部署* 資料夾中的執行時間存放區路徑 () 至 `DOTNET_SHARED_STORE` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="366c0-516">The runtime store path (in the RuntimeStore project's *deployment* folder) to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="366c0-517">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="366c0-517">Run the sample app.</span></span>
1. <span data-ttu-id="366c0-518">要求 `/services` 端點來查看應用程式的註冊服務。</span><span class="sxs-lookup"><span data-stu-id="366c0-518">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="366c0-519">要求 `/diag` 端點來查看診斷資訊。</span><span class="sxs-lookup"><span data-stu-id="366c0-519">Request the `/diag` endpoint to see the diagnostic information.</span></span>

::: moniker-end
