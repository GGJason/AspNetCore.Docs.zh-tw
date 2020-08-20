---
title: Visual Studio 發佈設定檔 (. .pubxml) 以進行 ASP.NET Core 應用程式部署
author: rick-anderson
description: 了解如何在 Visual Studio 中建立發行設定檔，並使用這些設定檔來管理對各種目標的 ASP.NET Core 應用程式部署。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/28/2020
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
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: a7bbc3f3a0a0d5a320bd290d325f0c01390828b4
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634159"
---
# <a name="visual-studio-publish-profiles-pubxml-for-aspnet-core-app-deployment"></a><span data-ttu-id="9bd58-103">Visual Studio 發佈設定檔 (. .pubxml) 以進行 ASP.NET Core 應用程式部署</span><span class="sxs-lookup"><span data-stu-id="9bd58-103">Visual Studio publish profiles (.pubxml) for ASP.NET Core app deployment</span></span>

<span data-ttu-id="9bd58-104">作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9bd58-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="9bd58-105">本文件的重點在如何使用 Visual Studio 2019 或更新版本來建立及使用發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-105">This document focuses on using Visual Studio 2019 or later to create and use publish profiles.</span></span> <span data-ttu-id="9bd58-106">使用 Visual Studio 建立的發行設定檔，可搭配 MSBuild 和 Visual Studio 使用。</span><span class="sxs-lookup"><span data-stu-id="9bd58-106">The publish profiles created with Visual Studio can be used with MSBuild and Visual Studio.</span></span> <span data-ttu-id="9bd58-107">如需發佈至 Azure 的指示，請參閱<xref:tutorials/publish-to-azure-webapp-using-vs>。</span><span class="sxs-lookup"><span data-stu-id="9bd58-107">For instructions on publishing to Azure, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>

<span data-ttu-id="9bd58-108">此 `dotnet new mvc` 命令會產生包含下列根層級[ \<Project> 元素](/visualstudio/msbuild/project-element-msbuild)的專案檔：</span><span class="sxs-lookup"><span data-stu-id="9bd58-108">The `dotnet new mvc` command produces a project file containing the following root-level [\<Project> element](/visualstudio/msbuild/project-element-msbuild):</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="9bd58-109">上述 `<Project>` 元素的 `Sdk` 屬性會從 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* 和 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*，分別匯入 MSBuild[屬性](/visualstudio/msbuild/msbuild-properties)和[目標](/visualstudio/msbuild/msbuild-targets)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="9bd58-110">`$(MSBuildSDKsPath)` (與 Visual Studio 2019 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9bd58-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="9bd58-111">`Microsoft.NET.Sdk.Web` ([WEB SDK](xref:razor-pages/web-sdk)) 相依于其他 sdk，包括 `Microsoft.NET.Sdk` ([.NET Core SDK](/dotnet/core/project-sdk/msbuild-props)) 和 `Microsoft.NET.Sdk.Razor` ([ Razor SDK](xref:razor-pages/sdk)) 。</span><span class="sxs-lookup"><span data-stu-id="9bd58-111">`Microsoft.NET.Sdk.Web` ([Web SDK](xref:razor-pages/web-sdk)) depends on other SDKs, including `Microsoft.NET.Sdk` ([.NET Core SDK](/dotnet/core/project-sdk/msbuild-props)) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="9bd58-112">系統會匯入與每個相依 SDK 相關聯的 MSBuild 屬性和目標。</span><span class="sxs-lookup"><span data-stu-id="9bd58-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="9bd58-113">發佈目標會根據所使用的發佈方法，匯入一組適當的目標。</span><span class="sxs-lookup"><span data-stu-id="9bd58-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="9bd58-114">當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：</span><span class="sxs-lookup"><span data-stu-id="9bd58-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="9bd58-115">建置專案</span><span class="sxs-lookup"><span data-stu-id="9bd58-115">Build project</span></span>
* <span data-ttu-id="9bd58-116">計算要發佈的檔案</span><span class="sxs-lookup"><span data-stu-id="9bd58-116">Compute files to publish</span></span>
* <span data-ttu-id="9bd58-117">將檔案發佈至目的地</span><span class="sxs-lookup"><span data-stu-id="9bd58-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="9bd58-118">計算專案項目</span><span class="sxs-lookup"><span data-stu-id="9bd58-118">Compute project items</span></span>

<span data-ttu-id="9bd58-119">載入專案時，會計算 [MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items) (檔案)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="9bd58-120">項目類型會決定如何處理檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="9bd58-121">根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="9bd58-122">`Compile` 項目清單中的檔案會予以編譯。</span><span class="sxs-lookup"><span data-stu-id="9bd58-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="9bd58-123">`Content` 項目清單包含除了組建輸出之外還會另行發佈的檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="9bd58-124">符合 `wwwroot\**`、`**\*.config` 和 `**\*.json` 模式的檔案預設會包含在 `Content` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="9bd58-125">例如，`wwwroot\**` [萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)會比對 *wwwroot* 資料夾及其子資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder and its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9bd58-126">[WEB sdk](xref:razor-pages/web-sdk)會匯入[ Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-126">The [Web SDK](xref:razor-pages/web-sdk) imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="9bd58-127">因此，符合 `**\*.cshtml` 和 `**\*.razor` 模式的檔案也會包含在 `Content` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="9bd58-128">[WEB sdk](xref:razor-pages/web-sdk)會匯入[ Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-128">The [Web SDK](xref:razor-pages/web-sdk) imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="9bd58-129">因此，符合 `**\*.cshtml` 模式的檔案也會包含在 `Content` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="9bd58-130">若要明確地將檔案新增至發佈清單，請直接在 *.csproj* 檔案中新增檔案，如[包含檔案](#include-files)一節中所示。</span><span class="sxs-lookup"><span data-stu-id="9bd58-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="9bd58-131">在 Visual Studio 中選取 [發佈]\*\*\*\* 按鈕，或從命令列發佈時：</span><span class="sxs-lookup"><span data-stu-id="9bd58-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="9bd58-132">會計算屬性/項目 (需要建置的檔案)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="9bd58-133">**僅限 Visual Studio**： NuGet 套件已還原。</span><span class="sxs-lookup"><span data-stu-id="9bd58-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="9bd58-134">(CLI 的使用者要明確還原。)</span><span class="sxs-lookup"><span data-stu-id="9bd58-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="9bd58-135">專案隨即建置。</span><span class="sxs-lookup"><span data-stu-id="9bd58-135">The project builds.</span></span>
* <span data-ttu-id="9bd58-136">會計算的發佈項目 (需要發佈的檔案)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="9bd58-137">會發佈專案 (計算的檔案會複製到發佈目的地)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="9bd58-138">當 ASP.NET Core 專案參考專案檔中的 `Microsoft.NET.Sdk.Web` 時，*app_offline.htm* 檔案會放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="9bd58-139">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="9bd58-140">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="9bd58-141">基本命令列發佈</span><span class="sxs-lookup"><span data-stu-id="9bd58-141">Basic command-line publishing</span></span>

<span data-ttu-id="9bd58-142">命令列發佈適用於所有 .NET Core 支援的平台，而不需要 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="9bd58-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="9bd58-143">在下列範例中，.NET Core CLI 的 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令是從專案目錄執行 (此目錄包含 *.csproj* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-143">In the following examples, the .NET Core CLI's [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="9bd58-144">如果專案資料夾不是目前的工作目錄，請在專案檔案路徑中明確傳入。</span><span class="sxs-lookup"><span data-stu-id="9bd58-144">If the project folder isn't the current working directory, explicitly pass in the project file path.</span></span> <span data-ttu-id="9bd58-145">例如：</span><span class="sxs-lookup"><span data-stu-id="9bd58-145">For example:</span></span>

```dotnetcli
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="9bd58-146">執行下列命令來建立及發佈 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="9bd58-146">Run the following commands to create and publish a web app:</span></span>

```dotnetcli
dotnet new mvc
dotnet publish
```

<span data-ttu-id="9bd58-147">`dotnet publish` 命令會產生下列輸出的變化：</span><span class="sxs-lookup"><span data-stu-id="9bd58-147">The `dotnet publish` command produces a variation of the following output:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

<span data-ttu-id="9bd58-148">預設發佈資料夾格式為 *bin\Debug\\{TARGET FRAMEWORK MONIKER}\publish\\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-148">The default publish folder format is *bin\Debug\\{TARGET FRAMEWORK MONIKER}\publish\\*.</span></span> <span data-ttu-id="9bd58-149">例如，*bin\Debug\netcoreapp2.2\publish\\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-149">For example, *bin\Debug\netcoreapp2.2\publish\\*.</span></span>

<span data-ttu-id="9bd58-150">下列命令會指定 `Release` 組建和發佈目錄：</span><span class="sxs-lookup"><span data-stu-id="9bd58-150">The following command specifies a `Release` build and the publishing directory:</span></span>

```dotnetcli
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="9bd58-151">`dotnet publish` 命令會呼叫 MSBuild，這會叫用 `Publish` 目標。</span><span class="sxs-lookup"><span data-stu-id="9bd58-151">The `dotnet publish` command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="9bd58-152">所有傳遞給 `dotnet publish` 的參數都會傳遞給 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="9bd58-152">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="9bd58-153">`-c` 和 `-o` 參數會分別對應至 MSBuild 的 `Configuration` 和 `OutputPath` 屬性。</span><span class="sxs-lookup"><span data-stu-id="9bd58-153">The `-c` and `-o` parameters map to MSBuild's `Configuration` and `OutputPath` properties, respectively.</span></span>

<span data-ttu-id="9bd58-154">您可以使用下列兩種格式其中之一來傳遞 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="9bd58-154">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="9bd58-155">例如，下列命令會將 `Release` 組建發佈到網路共用。</span><span class="sxs-lookup"><span data-stu-id="9bd58-155">For example, the following command publishes a `Release` build to a network share.</span></span> <span data-ttu-id="9bd58-156">網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。</span><span class="sxs-lookup"><span data-stu-id="9bd58-156">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

```dotnetcli
dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb
```

<span data-ttu-id="9bd58-157">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-157">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="9bd58-158">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-158">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="9bd58-159">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="9bd58-159">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="9bd58-160">發佈設定檔</span><span class="sxs-lookup"><span data-stu-id="9bd58-160">Publish profiles</span></span>

<span data-ttu-id="9bd58-161">本節會使用 Visual Studio 2019 或更新版本來建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-161">This section uses Visual Studio 2019 or later to create a publishing profile.</span></span> <span data-ttu-id="9bd58-162">建立設定檔之後，即可從 Visual Studio 或命令列進行發佈。</span><span class="sxs-lookup"><span data-stu-id="9bd58-162">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span> <span data-ttu-id="9bd58-163">發行設定檔可以簡化發佈程序，且設定檔數目並無限制。</span><span class="sxs-lookup"><span data-stu-id="9bd58-163">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span>

<span data-ttu-id="9bd58-164">請在 Visual Studio 中選擇下列其中一個路徑來建立設定檔：</span><span class="sxs-lookup"><span data-stu-id="9bd58-164">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="9bd58-165">在 [方案總管]\*\*\*\* 中以滑鼠右鍵按一下專案，再選取 [發佈]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-165">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="9bd58-166">從 [建置]\*\*\*\* 功能表選取 [發佈 {專案名稱}]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-166">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="9bd58-167">應用程式功能頁面的 [發佈]\*\*\*\* 索引標籤隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="9bd58-167">The **Publish** tab of the app capabilities page is displayed.</span></span> <span data-ttu-id="9bd58-168">如果專案沒有發行設定檔，則會顯示 [挑選發佈目標]\*\*\*\* 頁面。</span><span class="sxs-lookup"><span data-stu-id="9bd58-168">If the project lacks a publish profile, the **Pick a publish target** page is displayed.</span></span> <span data-ttu-id="9bd58-169">系統會要求您選取下列發佈目標之一：</span><span class="sxs-lookup"><span data-stu-id="9bd58-169">You're asked to select one of the following publish targets:</span></span>

* <span data-ttu-id="9bd58-170">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9bd58-170">Azure App Service</span></span>
* <span data-ttu-id="9bd58-171">Linux 上的 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9bd58-171">Azure App Service on Linux</span></span>
* <span data-ttu-id="9bd58-172">Azure 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="9bd58-172">Azure Virtual Machines</span></span>
* <span data-ttu-id="9bd58-173">資料夾</span><span class="sxs-lookup"><span data-stu-id="9bd58-173">Folder</span></span>
* <span data-ttu-id="9bd58-174">IIS、FTP、Web Deploy (適用於任何網頁伺服器)</span><span class="sxs-lookup"><span data-stu-id="9bd58-174">IIS, FTP, Web Deploy (for any web server)</span></span>
* <span data-ttu-id="9bd58-175">匯入設定檔</span><span class="sxs-lookup"><span data-stu-id="9bd58-175">Import Profile</span></span>

<span data-ttu-id="9bd58-176">若要判斷最適合的發佈目標，請參閱[適合我的發佈選項為何](/visualstudio/ide/not-in-toc/web-publish-options)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-176">To determine the most appropriate publish target, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="9bd58-177">選取 [資料夾]\*\*\*\* 發佈目標時，請指定儲存發佈資產的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="9bd58-177">When the **Folder** publish target is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="9bd58-178">預設資料夾路徑為 *bin\\{PROJECT CONFIGURATION}\\{TARGET FRAMEWORK MONIKER}\publish\\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-178">The default folder path is *bin\\{PROJECT CONFIGURATION}\\{TARGET FRAMEWORK MONIKER}\publish\\*.</span></span> <span data-ttu-id="9bd58-179">例如，*bin\Release\netcoreapp2.2\publish\\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-179">For example, *bin\Release\netcoreapp2.2\publish\\*.</span></span> <span data-ttu-id="9bd58-180">選取 [建立設定檔]\*\*\*\* 按鈕來完成操作。</span><span class="sxs-lookup"><span data-stu-id="9bd58-180">Select the **Create Profile** button to finish.</span></span>

<span data-ttu-id="9bd58-181">建立發行設定檔之後，[發佈]\*\*\*\* 索引標籤的內容就會變更。</span><span class="sxs-lookup"><span data-stu-id="9bd58-181">Once a publish profile is created, the **Publish** tab's content changes.</span></span> <span data-ttu-id="9bd58-182">新建立的設定檔會出現在下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="9bd58-183">在以下的下拉式清單中，選取 [建立新設定檔]\*\*\*\* 建立另一個新的設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-183">Below the drop-down list, select **Create new profile** to create another new profile.</span></span>

<span data-ttu-id="9bd58-184">Visual Studio 的發佈工具會產生描述發行設定檔的 *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-184">Visual Studio's publish tool produces a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file describing the publish profile.</span></span> <span data-ttu-id="9bd58-185">*.pubxml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="9bd58-185">The *.pubxml* file:</span></span>

* <span data-ttu-id="9bd58-186">包含發佈組態設定且供發佈程序使用。</span><span class="sxs-lookup"><span data-stu-id="9bd58-186">Contains publish configuration settings and is consumed by the publishing process.</span></span>
* <span data-ttu-id="9bd58-187">可修改以自訂建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="9bd58-187">Can be modified to customize the build and publish process.</span></span>

<span data-ttu-id="9bd58-188">發佈到 Azure 目標時，*.pubxml* 檔案會包含您的 Azure 訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="9bd58-188">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="9bd58-189">使用該目標類型時，不建議將此檔案新增至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="9bd58-189">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="9bd58-190">發佈到非 Azure 目標時，可放心簽入 *.pubxml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-190">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="9bd58-191">機密資訊 (例如發佈密碼) 會依每個使用者/電腦加密。</span><span class="sxs-lookup"><span data-stu-id="9bd58-191">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="9bd58-192">其儲存位置是在 *Properties/PublishProfiles/{設定檔名稱}.pubxml.user* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-192">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="9bd58-193">由於此檔案可以儲存機密資訊，因此不應該將它簽入至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="9bd58-193">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="9bd58-194">如需如何發佈 ASP.NET Core Web 應用程式的概觀，請參閱<xref:host-and-deploy/index>。</span><span class="sxs-lookup"><span data-stu-id="9bd58-194">For an overview of how to publish an ASP.NET Core web app, see <xref:host-and-deploy/index>.</span></span> <span data-ttu-id="9bd58-195">發佈 ASP.NET Core web 應用程式所需的 MSBuild 工作和目標是 [dotnet/websdk 存放庫](https://github.com/dotnet/websdk)中的開放原始碼。</span><span class="sxs-lookup"><span data-stu-id="9bd58-195">The MSBuild tasks and targets necessary to publish an ASP.NET Core web app are open-source in the [dotnet/websdk repository](https://github.com/dotnet/websdk).</span></span>

<span data-ttu-id="9bd58-196">下列命令可以使用資料夾、Msdeploy.exe 和 [Kudu](https://github.com/projectkudu/kudu/wiki) 發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-196">The following commands can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles.</span></span> <span data-ttu-id="9bd58-197">因為 MSDeploy 缺少跨平台支援，所以只有 Windows 支援下列 MSDeploy 選項。</span><span class="sxs-lookup"><span data-stu-id="9bd58-197">Because MSDeploy lacks cross-platform support, the following MSDeploy options are supported only on Windows.</span></span>

<span data-ttu-id="9bd58-198">**資料夾 (可跨平台運作)：**</span><span class="sxs-lookup"><span data-stu-id="9bd58-198">**Folder (works cross-platform):**</span></span>

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="9bd58-199">**MSDeploy：**</span><span class="sxs-lookup"><span data-stu-id="9bd58-199">**MSDeploy:**</span></span>

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="9bd58-200">**MSDeploy 套件：**</span><span class="sxs-lookup"><span data-stu-id="9bd58-200">**MSDeploy package:**</span></span>

```dotnetcli
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

```dotnetcli
dotnet build WebApplication.csproj /p:DeployOnBuild=true /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="9bd58-201">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="9bd58-201">In the preceding examples:</span></span>

* <span data-ttu-id="9bd58-202">`dotnet publish` 和 `dotnet build` 支援從任何平臺發佈至 Azure 的 Kudu api。</span><span class="sxs-lookup"><span data-stu-id="9bd58-202">`dotnet publish` and `dotnet build` support Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="9bd58-203">Visual Studio 發佈支援 Kudu API，但 WebSDK 也支援使用它來跨平台發佈到 Azure。</span><span class="sxs-lookup"><span data-stu-id="9bd58-203">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>
* <span data-ttu-id="9bd58-204">請勿傳遞 `DeployOnBuild` 至 `dotnet publish` 命令。</span><span class="sxs-lookup"><span data-stu-id="9bd58-204">Don't pass `DeployOnBuild` to the `dotnet publish` command.</span></span>

<span data-ttu-id="9bd58-205">如需詳細資訊，請參閱[Microsoft. Sdk。](https://github.com/dotnet/websdk#microsoftnetsdkpublish)</span><span class="sxs-lookup"><span data-stu-id="9bd58-205">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/dotnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="9bd58-206">將含有下列內容的發行設定檔新增至專案的 *Properties/PublishProfiles* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="9bd58-206">Add a publish profile to the project's *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

## <a name="folder-publish-example"></a><span data-ttu-id="9bd58-207">資料夾發佈範例</span><span class="sxs-lookup"><span data-stu-id="9bd58-207">Folder publish example</span></span>

<span data-ttu-id="9bd58-208">使用名為 *FolderProfile*的設定檔發佈時，請使用下列任何一個命令：</span><span class="sxs-lookup"><span data-stu-id="9bd58-208">When publishing with a profile named *FolderProfile*, use any of the following commands:</span></span>

```dotnetcli
dotnet publish /p:Configuration=Release /p:PublishProfile=FolderProfile`
```

```dotnetcli
dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

```bash
msbuild /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="9bd58-209">.NET Core CLI 的 [dotnet build](/dotnet/core/tools/dotnet-build) 命令會呼叫 `msbuild` 來執行建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="9bd58-209">The .NET Core CLI's [dotnet build](/dotnet/core/tools/dotnet-build) command calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="9bd58-210">傳入資料夾設定檔時，`dotnet build` 或 `msbuild` 命令是一樣的。</span><span class="sxs-lookup"><span data-stu-id="9bd58-210">The `dotnet build` and `msbuild` commands are equivalent when passing in a folder profile.</span></span> <span data-ttu-id="9bd58-211">在 Windows 上直接呼叫 `msbuild` 時，會使用 .NET Framework 版本的 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="9bd58-211">When calling `msbuild` directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="9bd58-212">在非資料夾設定檔上呼叫 `dotnet build`：</span><span class="sxs-lookup"><span data-stu-id="9bd58-212">Calling `dotnet build` on a non-folder profile:</span></span>

* <span data-ttu-id="9bd58-213">叫用 `msbuild`，這會使用 MSDeploy。</span><span class="sxs-lookup"><span data-stu-id="9bd58-213">Invokes `msbuild`, which uses MSDeploy.</span></span>
* <span data-ttu-id="9bd58-214">導致失敗 (即使是在 Windows 上執行)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-214">Results in a failure (even when running on Windows).</span></span> <span data-ttu-id="9bd58-215">若要使用非資料夾設定檔發佈，請直接呼叫 `msbuild`。</span><span class="sxs-lookup"><span data-stu-id="9bd58-215">To publish with a non-folder profile, call `msbuild` directly.</span></span>

<span data-ttu-id="9bd58-216">下列資料夾發行設定檔是以 Visual Studio 建立並發佈至網路共用：</span><span class="sxs-lookup"><span data-stu-id="9bd58-216">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="9bd58-217">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="9bd58-217">In the preceding example:</span></span>

* <span data-ttu-id="9bd58-218">`<ExcludeApp_Data>` 屬性的出現僅為滿足 XML 結構描述需求。</span><span class="sxs-lookup"><span data-stu-id="9bd58-218">The `<ExcludeApp_Data>` property is present merely to satisfy an XML schema requirement.</span></span> <span data-ttu-id="9bd58-219">即使專案根目錄中有 *App_Data* 資料夾，`<ExcludeApp_Data>` 屬性對發佈程序也不具效用。</span><span class="sxs-lookup"><span data-stu-id="9bd58-219">The `<ExcludeApp_Data>` property has no effect on the publish process, even if there's an *App_Data* folder in the project root.</span></span> <span data-ttu-id="9bd58-220">*App_Data* 資料夾不會受到和 ASP.NET 4.x 專案一樣的特殊處理。</span><span class="sxs-lookup"><span data-stu-id="9bd58-220">The *App_Data* folder doesn't receive special treatment as it does in ASP.NET 4.x projects.</span></span>
* <span data-ttu-id="9bd58-221">`<LastUsedBuildConfiguration>` 屬性設定為 `Release`。</span><span class="sxs-lookup"><span data-stu-id="9bd58-221">The `<LastUsedBuildConfiguration>` property is set to `Release`.</span></span> <span data-ttu-id="9bd58-222">從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 值。</span><span class="sxs-lookup"><span data-stu-id="9bd58-222">When publishing from Visual Studio, the value of `<LastUsedBuildConfiguration>` is set using the value when the publish process is started.</span></span> <span data-ttu-id="9bd58-223">`<LastUsedBuildConfiguration>` 很特殊，不應該在匯入的 MSBuild 檔案中被覆寫。</span><span class="sxs-lookup"><span data-stu-id="9bd58-223">`<LastUsedBuildConfiguration>` is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="9bd58-224">不過，這個屬性可以從命令列使用下列方法之一覆寫。</span><span class="sxs-lookup"><span data-stu-id="9bd58-224">This property can, however, be overridden from the command line using one of the following approaches.</span></span>
  * <span data-ttu-id="9bd58-225">使用 .NET Core CLI：</span><span class="sxs-lookup"><span data-stu-id="9bd58-225">Using the .NET Core CLI:</span></span>

    ```dotnetcli
    dotnet publish /p:Configuration=Release /p:PublishProfile=FolderProfile
    ```

    ```dotnetcli
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * <span data-ttu-id="9bd58-226">使用 MSBuild：</span><span class="sxs-lookup"><span data-stu-id="9bd58-226">Using MSBuild:</span></span>

    ```bash
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  <span data-ttu-id="9bd58-227">如需詳細資訊，請參閱 [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild：如何設定組態屬性)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-227">For more information, see [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).</span></span>

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="9bd58-228">從命令列發佈至 MSDeploy 端點</span><span class="sxs-lookup"><span data-stu-id="9bd58-228">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="9bd58-229">下列範例使用由 Visual Studio 建立的 ASP.NET Core Web 應用程式，名為 *AzureWebApp*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-229">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="9bd58-230">並使用 Visual Studio 新增了一個 Azure 應用程式發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-230">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="9bd58-231">如需如何建立設定檔的詳細資訊，請參閱[發行設定檔](#publish-profiles)一節。</span><span class="sxs-lookup"><span data-stu-id="9bd58-231">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="9bd58-232">若要使用發行設定檔部署應用程式，請從 **Visual Studio 開發人員命令提示字元**執行 `msbuild` 命令。</span><span class="sxs-lookup"><span data-stu-id="9bd58-232">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="9bd58-233">您可以從 Windows 工作列 [開始]\*\*\*\* 功能表的 [Visual Studio]\*\* 資料夾中存取此命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="9bd58-233">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="9bd58-234">為方便存取，您可以將命令提示字元新增至 Visual Studio 的 [工具]\*\*\*\* 功能表。</span><span class="sxs-lookup"><span data-stu-id="9bd58-234">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="9bd58-235">如需詳細資訊，請參閱 [Visual Studio 的開發人員命令提示字元](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-235">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="9bd58-236">MSBuild 使用下列命令語法：</span><span class="sxs-lookup"><span data-stu-id="9bd58-236">MSBuild uses the following command syntax:</span></span>

```bash
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="9bd58-237">`{PATH}`：應用程式專案檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="9bd58-237">`{PATH}`: Path to the app's project file.</span></span>
* <span data-ttu-id="9bd58-238">`{PROFILE}`：發行設定檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="9bd58-238">`{PROFILE}`: Name of the publish profile.</span></span>
* <span data-ttu-id="9bd58-239">`{USERNAME}`： Msdeploy.exe 使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="9bd58-239">`{USERNAME}`: MSDeploy username.</span></span> <span data-ttu-id="9bd58-240">`{USERNAME}`可以在發行設定檔中找到。</span><span class="sxs-lookup"><span data-stu-id="9bd58-240">The `{USERNAME}` can be found in the publish profile.</span></span>
* <span data-ttu-id="9bd58-241">`{PASSWORD}`： Msdeploy.exe 密碼。</span><span class="sxs-lookup"><span data-stu-id="9bd58-241">`{PASSWORD}`: MSDeploy password.</span></span> <span data-ttu-id="9bd58-242">`{PASSWORD}`從 *{PROFILE} 取得。.Publishsettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-242">Obtain the `{PASSWORD}` from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="9bd58-243">從下列其中一個位置下載 *.PublishSettings* 檔案：</span><span class="sxs-lookup"><span data-stu-id="9bd58-243">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="9bd58-244">**方案總管**：選取**View**  >  **Cloud Explorer**。</span><span class="sxs-lookup"><span data-stu-id="9bd58-244">**Solution Explorer**: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="9bd58-245">連線到您的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="9bd58-245">Connect with your Azure subscription.</span></span> <span data-ttu-id="9bd58-246">開啟 [應用程式服務]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-246">Open **App Services**.</span></span> <span data-ttu-id="9bd58-247">以滑鼠右鍵按一下應用程式。</span><span class="sxs-lookup"><span data-stu-id="9bd58-247">Right-click the app.</span></span> <span data-ttu-id="9bd58-248">選取 [下載發行設定檔]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-248">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="9bd58-249">Azure 入口網站：選取 web 應用程式**總覽**面板中的 [**取得發行設定檔**]。</span><span class="sxs-lookup"><span data-stu-id="9bd58-249">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="9bd58-250">下列範例使用名為 *AzureWebApp - Web Deploy* 的發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="9bd58-250">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```bash
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="9bd58-251">您也可以從 Windows 命令殼層透過 .NET Core CLI 的 [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) 命令來使用發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="9bd58-251">A publish profile can also be used with the .NET Core CLI's [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command shell:</span></span>

```dotnetcli
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> <span data-ttu-id="9bd58-252">`dotnet msbuild` 命令可跨平台使用，並可在 macOS 和 Linux 上編譯 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9bd58-252">The `dotnet msbuild` command is a cross-platform command and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="9bd58-253">不過，macOS 和 Linux 上的 MSBuild 無法將應用程式部署至 Azure 或其他 MSDeploy 端點。</span><span class="sxs-lookup"><span data-stu-id="9bd58-253">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoints.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="9bd58-254">設定環境</span><span class="sxs-lookup"><span data-stu-id="9bd58-254">Set the environment</span></span>

<span data-ttu-id="9bd58-255">在發行設定檔 (*.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性，以設定應用程式的[環境](xref:fundamentals/environments)：</span><span class="sxs-lookup"><span data-stu-id="9bd58-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="9bd58-256">如果您需要進行 *web.config* 轉換 (例如根據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="9bd58-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="9bd58-257">排除檔案</span><span class="sxs-lookup"><span data-stu-id="9bd58-257">Exclude files</span></span>

<span data-ttu-id="9bd58-258">發佈 ASP.NET Core Web 應用程式時，下列資產會包含在內：</span><span class="sxs-lookup"><span data-stu-id="9bd58-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="9bd58-259">組建成品</span><span class="sxs-lookup"><span data-stu-id="9bd58-259">Build artifacts</span></span>
* <span data-ttu-id="9bd58-260">符合下列萬用字元模式的資料夾和檔案：</span><span class="sxs-lookup"><span data-stu-id="9bd58-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="9bd58-261">`**\*.config` (例如，*web.config*)</span><span class="sxs-lookup"><span data-stu-id="9bd58-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="9bd58-262">`**\*.json` (例如，*appsettings.json*)</span><span class="sxs-lookup"><span data-stu-id="9bd58-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="9bd58-263">MSBuild 支援[萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="9bd58-264">例如，下列 `<Content>` 元素會抑制在 *wwwroot/content* 資料夾及其子資料夾中複製文字檔 (*.txt*)：</span><span class="sxs-lookup"><span data-stu-id="9bd58-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="9bd58-265">您可以將上述標記新增至發行設定檔或 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="9bd58-266">當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="9bd58-267">下列 `<MsDeploySkipRules>` 元素會排除 *wwwroot\content* 資料夾中的所有檔案：</span><span class="sxs-lookup"><span data-stu-id="9bd58-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="9bd58-268">`<MsDeploySkipRules>` 不會從部署網站刪除「略過」\*\* 目標。</span><span class="sxs-lookup"><span data-stu-id="9bd58-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="9bd58-269">這會從部署網站刪除 `<Content>` 鎖定目標的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="9bd58-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="9bd58-270">例如，假設所部署的 Web 應用程式包含下列檔案：</span><span class="sxs-lookup"><span data-stu-id="9bd58-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="9bd58-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9bd58-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="9bd58-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9bd58-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="9bd58-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="9bd58-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="9bd58-274">如果新增下列 `<MsDeploySkipRules>` 元素，就不會刪除部署網站上的這些檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="9bd58-275">上述 `<MsDeploySkipRules>` 元素會防止部署「已略過」\*\* 的檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="9bd58-276">如果已部署這些檔案，它並不會將它們刪除。</span><span class="sxs-lookup"><span data-stu-id="9bd58-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="9bd58-277">下列 `<Content>` 元素會刪除部署網站上已鎖定目標的檔案：</span><span class="sxs-lookup"><span data-stu-id="9bd58-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="9bd58-278">使用命令列部署搭配上述 `<Content>` 元素會產生下列輸出變化：</span><span class="sxs-lookup"><span data-stu-id="9bd58-278">Using command-line deployment with the preceding `<Content>` element yields a variation of the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="9bd58-279">Include 檔案</span><span class="sxs-lookup"><span data-stu-id="9bd58-279">Include files</span></span>

<span data-ttu-id="9bd58-280">下列各節會概述不同方法，以便在發佈階段包含檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-280">The following sections outline different approaches for file inclusion at publish time.</span></span> <span data-ttu-id="9bd58-281">[ [一般檔案包含](#general-file-inclusion) ] 區段 `DotNetPublishFiles` 會使用專案，此專案是由 [Web SDK](xref:razor-pages/web-sdk)中的發佈目標檔案提供。</span><span class="sxs-lookup"><span data-stu-id="9bd58-281">The [General file inclusion](#general-file-inclusion) section uses the `DotNetPublishFiles` item, which is provided by a publish targets file in the [Web SDK](xref:razor-pages/web-sdk).</span></span> <span data-ttu-id="9bd58-282">[ [選擇性檔案包含](#selective-file-inclusion) ] 區段 `ResolvedFileToPublish` 會使用專案，此專案是由 [.NET Core SDK](/dotnet/core/project-sdk/msbuild-props)中的發佈目標檔案提供。</span><span class="sxs-lookup"><span data-stu-id="9bd58-282">The [Selective file inclusion](#selective-file-inclusion) section uses the `ResolvedFileToPublish` item, which is provided by a publish targets file in the [.NET Core SDK](/dotnet/core/project-sdk/msbuild-props).</span></span> <span data-ttu-id="9bd58-283">因為 Web SDK 相依於.NET Core SDK，所以 ASP.NET Core 專案可使用兩個項目的任一個。</span><span class="sxs-lookup"><span data-stu-id="9bd58-283">Because the Web SDK depends on the .NET Core SDK, either item can be used in an ASP.NET Core project.</span></span>

### <a name="general-file-inclusion"></a><span data-ttu-id="9bd58-284">包含一般檔案</span><span class="sxs-lookup"><span data-stu-id="9bd58-284">General file inclusion</span></span>

<span data-ttu-id="9bd58-285">下列範例的 `<ItemGroup>` 元素會示範將位在專案目錄外的資料夾複製到發佈網站的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9bd58-285">The following example's `<ItemGroup>` element demonstrates copying a folder located outside of the project directory to a folder of the published site.</span></span> <span data-ttu-id="9bd58-286">預設包含新增至下列標記 `<ItemGroup>` 中的任何檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-286">Any files added to the following markup's `<ItemGroup>` are included by default.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="9bd58-287">上述標記：</span><span class="sxs-lookup"><span data-stu-id="9bd58-287">The preceding markup:</span></span>

* <span data-ttu-id="9bd58-288">可以新增至 *.csproj* 檔案或發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9bd58-288">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="9bd58-289">如果將它新增至 *.csproj* 檔案，它就會包含在專案的每個發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="9bd58-289">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>
* <span data-ttu-id="9bd58-290">宣告 `_CustomFiles` 項目來儲存符合 `Include` 屬性萬用字元模式的檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-290">Declares a `_CustomFiles` item to store files matching the `Include` attribute's globbing pattern.</span></span> <span data-ttu-id="9bd58-291">模式中參考的 *images* 資料夾位於專案目錄外。</span><span class="sxs-lookup"><span data-stu-id="9bd58-291">The *images* folder referenced in the pattern is located outside of the project directory.</span></span> <span data-ttu-id="9bd58-292">名為 `$(MSBuildProjectDirectory)` 的[保留屬性](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)，會解析成專案檔案的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="9bd58-292">A [reserved property](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties), named `$(MSBuildProjectDirectory)`, resolves to the project file's absolute path.</span></span>
* <span data-ttu-id="9bd58-293">向 `DotNetPublishFiles` 項目提供檔案清單。</span><span class="sxs-lookup"><span data-stu-id="9bd58-293">Provides a list of files to the `DotNetPublishFiles` item.</span></span> <span data-ttu-id="9bd58-294">根據預設，項目的 `<DestinationRelativePath>` 元素為空白。</span><span class="sxs-lookup"><span data-stu-id="9bd58-294">By default, the item's `<DestinationRelativePath>` element is empty.</span></span> <span data-ttu-id="9bd58-295">預設值會在標記中覆寫，並使用[已知的項目中繼資料](/visualstudio/msbuild/msbuild-well-known-item-metadata)，例如 `%(RecursiveDir)`。</span><span class="sxs-lookup"><span data-stu-id="9bd58-295">The default value is overridden in the markup and uses [well-known item metadata](/visualstudio/msbuild/msbuild-well-known-item-metadata) such as `%(RecursiveDir)`.</span></span> <span data-ttu-id="9bd58-296">內部文字代表發佈網站的 *wwwroot/images* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9bd58-296">The inner text represents the *wwwroot/images* folder of the published site.</span></span>

### <a name="selective-file-inclusion"></a><span data-ttu-id="9bd58-297">包含選擇性檔案</span><span class="sxs-lookup"><span data-stu-id="9bd58-297">Selective file inclusion</span></span>

<span data-ttu-id="9bd58-298">下列範例中的反白顯示標記會示範：</span><span class="sxs-lookup"><span data-stu-id="9bd58-298">The highlighted markup in the following example demonstrates:</span></span>

* <span data-ttu-id="9bd58-299">將位於專案外部的檔案複製到發佈網站的 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9bd58-299">Copying a file located outside of the project into the published site's *wwwroot* folder.</span></span> <span data-ttu-id="9bd58-300">維持 *ReadMe2.md* 的檔名。</span><span class="sxs-lookup"><span data-stu-id="9bd58-300">The file name of *ReadMe2.md* is maintained.</span></span>
* <span data-ttu-id="9bd58-301">排除 *wwwroot\Content* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9bd58-301">Excluding the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="9bd58-302">排除 *Views\Home\About2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9bd58-302">Excluding *Views\Home\About2.cshtml*.</span></span>

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

<span data-ttu-id="9bd58-303">上述範例會使用 `ResolvedFileToPublish` 項目，其預設行為是一律將 `Include` 屬性提供的檔案複製到發佈的網站。</span><span class="sxs-lookup"><span data-stu-id="9bd58-303">The preceding example uses the `ResolvedFileToPublish` item, whose default behavior is to always copy the files provided in the `Include` attribute to the published site.</span></span> <span data-ttu-id="9bd58-304">藉由包含附帶內部文字 `Never` 或 `PreserveNewest` 的 `<CopyToPublishDirectory>` 子元素，來覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="9bd58-304">Override the default behavior by including a `<CopyToPublishDirectory>` child element with inner text of either `Never` or `PreserveNewest`.</span></span> <span data-ttu-id="9bd58-305">例如：</span><span class="sxs-lookup"><span data-stu-id="9bd58-305">For example:</span></span>

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

<span data-ttu-id="9bd58-306">如需更多部署範例，請參閱 [WEB SDK 讀我檔案](https://github.com/dotnet/sdk/tree/master/src/WebSdk)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-306">For more deployment samples, see the [Web SDK README file](https://github.com/dotnet/sdk/tree/master/src/WebSdk).</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="9bd58-307">在發佈之前或之後執行目標</span><span class="sxs-lookup"><span data-stu-id="9bd58-307">Run a target before or after publishing</span></span>

<span data-ttu-id="9bd58-308">內建的 `BeforePublish` 和 `AfterPublish` 目標可在發佈目標之前或之後執行目標。</span><span class="sxs-lookup"><span data-stu-id="9bd58-308">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="9bd58-309">將下列元素新增至發行設定檔，即可在發佈之前和之後都記錄主控台訊息：</span><span class="sxs-lookup"><span data-stu-id="9bd58-309">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="9bd58-310">使用未受信任的憑證來發佈至伺服器</span><span class="sxs-lookup"><span data-stu-id="9bd58-310">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="9bd58-311">將 `<AllowUntrustedCertificate>` 屬性搭配 `True` 值新增至發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="9bd58-311">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="9bd58-312">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="9bd58-312">The Kudu service</span></span>

<span data-ttu-id="9bd58-313">若要檢視 Azure App Service Web 應用程式部署中的檔案，請使用 [Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。</span><span class="sxs-lookup"><span data-stu-id="9bd58-313">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="9bd58-314">將 `scm` 權杖附加至 Web 應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="9bd58-314">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="9bd58-315">例如：</span><span class="sxs-lookup"><span data-stu-id="9bd58-315">For example:</span></span>

| <span data-ttu-id="9bd58-316">URL</span><span class="sxs-lookup"><span data-stu-id="9bd58-316">URL</span></span>                                    | <span data-ttu-id="9bd58-317">結果</span><span class="sxs-lookup"><span data-stu-id="9bd58-317">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="9bd58-318">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="9bd58-318">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="9bd58-319">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="9bd58-319">Kudu service</span></span> |

<span data-ttu-id="9bd58-320">選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目，以檢視、編輯、刪除或新增檔案。</span><span class="sxs-lookup"><span data-stu-id="9bd58-320">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9bd58-321">其他資源</span><span class="sxs-lookup"><span data-stu-id="9bd58-321">Additional resources</span></span>

* <span data-ttu-id="9bd58-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) 可簡化對 IIS 伺服器的 Web 應用程式和網站部署。</span><span class="sxs-lookup"><span data-stu-id="9bd58-322">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="9bd58-323">[WEB SDK GitHub 存放庫](https://github.com/dotnet/websdk/issues)：用於部署的檔案問題和要求功能。</span><span class="sxs-lookup"><span data-stu-id="9bd58-323">[Web SDK GitHub repository](https://github.com/dotnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="9bd58-324">從 Visual Studio 將 ASP.NET Web 應用程式發行到 Azure VM</span><span class="sxs-lookup"><span data-stu-id="9bd58-324">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
