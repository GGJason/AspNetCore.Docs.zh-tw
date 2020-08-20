---
title: ASP.NET Core 中的檔案提供者
author: rick-anderson
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2020
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
uid: fundamentals/file-providers
ms.openlocfilehash: 30c28e7bd4cd9c926b157f5a7b9e6688bd5b9b9a
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634601"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="e3ca1-103">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="e3ca1-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="e3ca1-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e3ca1-104">By [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e3ca1-105">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="e3ca1-106">檔案提供者是在整個 ASP.NET Core framework 中使用。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-106">File Providers are used throughout the ASP.NET Core framework.</span></span> <span data-ttu-id="e3ca1-107">例如：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-107">For example:</span></span>

* <span data-ttu-id="e3ca1-108"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 將應用程式的 [內容根](xref:fundamentals/index#content-root) 和 [web 根目錄](xref:fundamentals/index#web-root) 公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-108"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="e3ca1-109">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-109">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="e3ca1-110">[Razor](xref:mvc/views/razor) 使用檔案提供者來尋找頁面和瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-110">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="e3ca1-111">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-111">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="e3ca1-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="e3ca1-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="e3ca1-113">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="e3ca1-113">File Provider interfaces</span></span>

<span data-ttu-id="e3ca1-114">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-114">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="e3ca1-115">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-115">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="e3ca1-116">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-116">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="e3ca1-117">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-117">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="e3ca1-118">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-118">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="e3ca1-119">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-119">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="e3ca1-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="e3ca1-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="e3ca1-121"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="e3ca1-121"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="e3ca1-122">您可以使用方法從檔案讀取 <xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-122">You can read from the file using the <xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="e3ca1-123">*FileProviderSample*範例應用程式示範如何設定中的檔案提供者 `Startup.ConfigureServices` ，以透過相依性[插入](xref:fundamentals/dependency-injection)在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-123">The *FileProviderSample* sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="e3ca1-124">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="e3ca1-124">File Provider implementations</span></span>

<span data-ttu-id="e3ca1-125">下表列出的實現 `IFileProvider` 。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-125">The following table lists implementations of `IFileProvider`.</span></span>

| <span data-ttu-id="e3ca1-126">實作</span><span class="sxs-lookup"><span data-stu-id="e3ca1-126">Implementation</span></span> | <span data-ttu-id="e3ca1-127">描述</span><span class="sxs-lookup"><span data-stu-id="e3ca1-127">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="e3ca1-128">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-128">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="e3ca1-129">用來提供一或多個其他提供者對檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-129">Used to provide combined access to files and directories from one or more other providers.</span></span> |
| [<span data-ttu-id="e3ca1-130">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-130">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="e3ca1-131">用來存取內嵌于元件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-131">Used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="e3ca1-132">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-132">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="e3ca1-133">用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-133">Used to access the system's physical files.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="e3ca1-134">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-134">PhysicalFileProvider</span></span>

<span data-ttu-id="e3ca1-135"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-135">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="e3ca1-136">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-136">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="e3ca1-137">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-137">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="e3ca1-138">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-138">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="e3ca1-139">直接具現化此提供者時，必須要有絕對目錄路徑，而且可做為使用提供者所提出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-139">When instantiating this provider directly, an absolute directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="e3ca1-140">目錄路徑不支援 Glob 模式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-140">Glob patterns aren't supported in the directory path.</span></span>

<span data-ttu-id="e3ca1-141">下列程式碼說明如何使用 `PhysicalFileProvider` 來取得目錄內容和檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-141">The following code shows how to use `PhysicalFileProvider` to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var filePath = Path.Combine("wwwroot", "js", "site.js");
var fileInfo = provider.GetFileInfo(filePath);
```

<span data-ttu-id="e3ca1-142">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-142">Types in the preceding example:</span></span>

* <span data-ttu-id="e3ca1-143">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-143">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="e3ca1-144">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-144">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="e3ca1-145">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-145">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="e3ca1-146">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-146">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="e3ca1-147">Glob 模式無法傳遞給 `GetFileInfo` 方法。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-147">Glob patterns can't be passed to the `GetFileInfo` method.</span></span> <span data-ttu-id="e3ca1-148">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-148">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="e3ca1-149">*FileProviderSample*範例應用程式會 `Startup.ConfigureServices` 使用下列方法，在方法中建立提供者 <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider?displayProperty=nameWithType> ：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-149">The *FileProviderSample* sample app creates the provider in the `Startup.ConfigureServices` method using <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider?displayProperty=nameWithType>:</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="e3ca1-150">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-150">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="e3ca1-151"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-151">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="e3ca1-152">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-152">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="e3ca1-153">若要產生內嵌檔案的資訊清單：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-153">To generate a manifest of the embedded files:</span></span>

1. <span data-ttu-id="e3ca1-154">將 [FileProviders 內嵌](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) NuGet 套件新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-154">Add the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) NuGet package to your project.</span></span>
1. <span data-ttu-id="e3ca1-155">將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-155">Set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="e3ca1-156">指定要內嵌的檔案 [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) ：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-156">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

    [!code-xml[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="e3ca1-157">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-157">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="e3ca1-158">*FileProviderSample*範例應用程式會建立 `ManifestEmbeddedFileProvider` ，並將目前執行的元件傳遞至它的函式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-158">The *FileProviderSample* sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="e3ca1-159">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-159">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="e3ca1-160">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-160">Additional overloads allow you to:</span></span>

* <span data-ttu-id="e3ca1-161">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-161">Specify a relative file path.</span></span>
* <span data-ttu-id="e3ca1-162">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-162">Scope files to a last modified date.</span></span>
* <span data-ttu-id="e3ca1-163">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-163">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="e3ca1-164">多載</span><span class="sxs-lookup"><span data-stu-id="e3ca1-164">Overload</span></span> | <span data-ttu-id="e3ca1-165">描述</span><span class="sxs-lookup"><span data-stu-id="e3ca1-165">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="e3ca1-166">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-166">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="e3ca1-167">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-167">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="e3ca1-168">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-168">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="e3ca1-169">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-169">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="e3ca1-170">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-170">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="e3ca1-171">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-171">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="e3ca1-172">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-172">CompositeFileProvider</span></span>

<span data-ttu-id="e3ca1-173"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-173">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="e3ca1-174">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-174">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="e3ca1-175">在 *FileProviderSample* 範例應用程式中， `PhysicalFileProvider` 和會 `ManifestEmbeddedFileProvider` 提供檔案給在 `CompositeFileProvider` 應用程式的服務容器中註冊的。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-175">In the *FileProviderSample* sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container.</span></span> <span data-ttu-id="e3ca1-176">下列程式碼可在專案的方法中找到 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-176">The following code is found in the project's `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="e3ca1-177">監視變更</span><span class="sxs-lookup"><span data-stu-id="e3ca1-177">Watch for changes</span></span>

<span data-ttu-id="e3ca1-178"><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*?displayProperty=nameWithType>方法會提供一個案例，以監看一或多個檔案或目錄的變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-178">The <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*?displayProperty=nameWithType> method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="e3ca1-179">`Watch` 方法：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-179">The `Watch` method:</span></span>

* <span data-ttu-id="e3ca1-180">接受檔案路徑字串，該字串可以使用 [glob 模式](#glob-patterns) 來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-180">Accepts a file path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span>
* <span data-ttu-id="e3ca1-181">傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-181">Returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span>

<span data-ttu-id="e3ca1-182">產生的變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-182">The resulting change token exposes:</span></span>

* <span data-ttu-id="e3ca1-183"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>：可以檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-183"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>: A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="e3ca1-184"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>：當偵測到所指定路徑字串的變更時呼叫。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-184"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>: Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="e3ca1-185">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-185">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="e3ca1-186">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-186">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="e3ca1-187">*WatchConsole*範例應用程式會在*TextFiles*目錄中的 *.txt*檔案遭到修改時，寫入訊息：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-187">The *WatchConsole* sample app writes a message whenever a *.txt* file in the *TextFiles* directory is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1)]

<span data-ttu-id="e3ca1-188">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-188">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="e3ca1-189">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-189">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

### <a name="glob-patterns"></a><span data-ttu-id="e3ca1-190">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="e3ca1-190">Glob patterns</span></span>

<span data-ttu-id="e3ca1-191">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-191">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="e3ca1-192">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-192">Specify groups of files with these patterns.</span></span> <span data-ttu-id="e3ca1-193">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-193">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="e3ca1-194">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-194">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="e3ca1-195">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-195">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="e3ca1-196">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-196">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="e3ca1-197">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-197">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="e3ca1-198">下表提供 glob 模式的常見範例。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-198">The following table provides common examples of glob patterns.</span></span>

|<span data-ttu-id="e3ca1-199">模式</span><span class="sxs-lookup"><span data-stu-id="e3ca1-199">Pattern</span></span>  |<span data-ttu-id="e3ca1-200">描述</span><span class="sxs-lookup"><span data-stu-id="e3ca1-200">Description</span></span>  |
|---------|---------|
|`directory/file.txt`|<span data-ttu-id="e3ca1-201">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-201">Matches a specific file in a specific directory.</span></span>|
|`directory/*.txt`|<span data-ttu-id="e3ca1-202">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-202">Matches all files with *.txt* extension in a specific directory.</span></span>|
|`directory/*/appsettings.json`|<span data-ttu-id="e3ca1-203">會比 \* 對\* 目錄中檔案的所有appsettings.js，完全符合 *目錄* 資料夾底下的一個層級。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-203">Matches all *appsettings.json* files in directories exactly one level below the *directory* folder.</span></span>|
|`directory/**/*.txt`|<span data-ttu-id="e3ca1-204">比對副檔名為 .txt 的所有檔案，其位於*目錄*資料夾底下的任何位置 *。*</span><span class="sxs-lookup"><span data-stu-id="e3ca1-204">Matches all files with a *.txt* extension found anywhere under the *directory* folder.</span></span>|

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e3ca1-205">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-205">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="e3ca1-206">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-206">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="e3ca1-207"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 將應用程式的 [內容根](xref:fundamentals/index#content-root) 和 [web 根目錄](xref:fundamentals/index#web-root) 公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-207"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="e3ca1-208">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-208">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="e3ca1-209">[Razor](xref:mvc/views/razor) 使用檔案提供者來尋找頁面和瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-209">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="e3ca1-210">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-210">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="e3ca1-211">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="e3ca1-211">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="e3ca1-212">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="e3ca1-212">File Provider interfaces</span></span>

<span data-ttu-id="e3ca1-213">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-213">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="e3ca1-214">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-214">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="e3ca1-215">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-215">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="e3ca1-216">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-216">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="e3ca1-217">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-217">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="e3ca1-218">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-218">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="e3ca1-219"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="e3ca1-219"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="e3ca1-220"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="e3ca1-220"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="e3ca1-221">您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-221">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="e3ca1-222">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-222">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="e3ca1-223">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="e3ca1-223">File Provider implementations</span></span>

<span data-ttu-id="e3ca1-224">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-224">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="e3ca1-225">實作</span><span class="sxs-lookup"><span data-stu-id="e3ca1-225">Implementation</span></span> | <span data-ttu-id="e3ca1-226">描述</span><span class="sxs-lookup"><span data-stu-id="e3ca1-226">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="e3ca1-227">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-227">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="e3ca1-228">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-228">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="e3ca1-229">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-229">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="e3ca1-230">資訊清單內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-230">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="e3ca1-231">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-231">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="e3ca1-232">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-232">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="e3ca1-233">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-233">PhysicalFileProvider</span></span>

<span data-ttu-id="e3ca1-234"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-234">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="e3ca1-235">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-235">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="e3ca1-236">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-236">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="e3ca1-237">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-237">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="e3ca1-238">當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-238">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="e3ca1-239">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-239">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="e3ca1-240">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-240">Types in the preceding example:</span></span>

* <span data-ttu-id="e3ca1-241">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-241">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="e3ca1-242">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-242">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="e3ca1-243">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-243">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="e3ca1-244">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-244">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="e3ca1-245">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-245">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="e3ca1-246">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-246">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="e3ca1-247">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-247">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="e3ca1-248"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-248">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="e3ca1-249">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-249">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="e3ca1-250">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-250">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="e3ca1-251">使用[ &lt; EmbeddedResource &gt; ](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)指定要內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-251">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-xml[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="e3ca1-252">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-252">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="e3ca1-253">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-253">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="e3ca1-254">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-254">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="e3ca1-255">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-255">Additional overloads allow you to:</span></span>

* <span data-ttu-id="e3ca1-256">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-256">Specify a relative file path.</span></span>
* <span data-ttu-id="e3ca1-257">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-257">Scope files to a last modified date.</span></span>
* <span data-ttu-id="e3ca1-258">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-258">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="e3ca1-259">多載</span><span class="sxs-lookup"><span data-stu-id="e3ca1-259">Overload</span></span> | <span data-ttu-id="e3ca1-260">描述</span><span class="sxs-lookup"><span data-stu-id="e3ca1-260">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="e3ca1-261">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-261">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="e3ca1-262">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-262">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="e3ca1-263">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-263">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="e3ca1-264">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-264">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="e3ca1-265">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-265">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="e3ca1-266">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-266">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="e3ca1-267">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e3ca1-267">CompositeFileProvider</span></span>

<span data-ttu-id="e3ca1-268"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-268">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="e3ca1-269">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-269">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="e3ca1-270">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-270">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="e3ca1-271">監視變更</span><span class="sxs-lookup"><span data-stu-id="e3ca1-271">Watch for changes</span></span>

<span data-ttu-id="e3ca1-272">[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-272">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="e3ca1-273">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-273">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="e3ca1-274">`Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-274">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="e3ca1-275">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-275">The change token exposes:</span></span>

* <span data-ttu-id="e3ca1-276"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>：可以檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-276"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>: A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="e3ca1-277"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>：當偵測到所指定路徑字串的變更時呼叫。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-277"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>: Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="e3ca1-278">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-278">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="e3ca1-279">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-279">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="e3ca1-280">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-280">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="e3ca1-281">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-281">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="e3ca1-282">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-282">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="e3ca1-283">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="e3ca1-283">Glob patterns</span></span>

<span data-ttu-id="e3ca1-284">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-284">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="e3ca1-285">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-285">Specify groups of files with these patterns.</span></span> <span data-ttu-id="e3ca1-286">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="e3ca1-286">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="e3ca1-287">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-287">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="e3ca1-288">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-288">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="e3ca1-289">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-289">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="e3ca1-290">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-290">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="e3ca1-291">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="e3ca1-291">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="e3ca1-292">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-292">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="e3ca1-293">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-293">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="e3ca1-294">符合「目錄」\*\* 資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-294">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="e3ca1-295">符合在「目錄」\*\* 資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="e3ca1-295">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
