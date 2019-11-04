---
title: 使用 dotnet-grpc 管理 Protobuf 參考
author: juntaoluo
description: 瞭解如何使用 dotnet-grpc 通用工具新增、更新、移除和列出 Protobuf 參考。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 10/17/2019
uid: grpc/dotnet-grpc
ms.openlocfilehash: 994597c854a95bb33de1686ab025cb3744cf6845
ms.sourcegitcommit: e71b6a85b0e94a600af607107e298f932924c849
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/17/2019
ms.locfileid: "72519044"
---
# <a name="manage-protobuf-references-with-dotnet-grpc"></a><span data-ttu-id="c9fea-103">使用 dotnet-grpc 管理 Protobuf 參考</span><span class="sxs-lookup"><span data-stu-id="c9fea-103">Manage Protobuf references with dotnet-grpc</span></span>

<span data-ttu-id="c9fea-104">作者：[John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="c9fea-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="c9fea-105">`dotnet-grpc` 是 .NET Core 通用工具，可用於管理 .NET gRPC 專案內的[Protobuf （ *. proto*）](xref:grpc/basics#proto-file)參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-105">`dotnet-grpc` is a .NET Core Global Tool for managing [Protobuf (*.proto*)](xref:grpc/basics#proto-file) references within a .NET gRPC project.</span></span> <span data-ttu-id="c9fea-106">此工具可以用來新增、重新整理、移除和列出 Protobuf 的參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-106">The tool can be used to add, refresh, remove, and list Protobuf references.</span></span>

## <a name="installation"></a><span data-ttu-id="c9fea-107">安裝</span><span class="sxs-lookup"><span data-stu-id="c9fea-107">Installation</span></span>

<span data-ttu-id="c9fea-108">若要安裝 `dotnet-grpc` [.Net Core 通用工具](/dotnet/core/tools/global-tools)，請執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="c9fea-108">To install the `dotnet-grpc` [.NET Core Global Tool](/dotnet/core/tools/global-tools), run the following command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-grpc
```

## <a name="add-references"></a><span data-ttu-id="c9fea-109">新增參考</span><span class="sxs-lookup"><span data-stu-id="c9fea-109">Add references</span></span>

<span data-ttu-id="c9fea-110">`dotnet-grpc` 可用來將 Protobuf 參考當做 `<Protobuf />` 專案新增至 *.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="c9fea-110">`dotnet-grpc` can be used to add Protobuf references as `<Protobuf />` items to the *.csproj* file:</span></span>

```xml
<Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
```

<span data-ttu-id="c9fea-111">Protobuf 參考是用來產生C#用戶端和/或伺服器資產。</span><span class="sxs-lookup"><span data-stu-id="c9fea-111">The Protobuf references are used to generate the C# client and/or server assets.</span></span> <span data-ttu-id="c9fea-112">`dotnet-grpc` 工具可以：</span><span class="sxs-lookup"><span data-stu-id="c9fea-112">The `dotnet-grpc` tool can:</span></span>

* <span data-ttu-id="c9fea-113">從磁片上的本機檔案建立 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-113">Create a Protobuf reference from local files on disk.</span></span>
* <span data-ttu-id="c9fea-114">從 URL 所指定的遠端檔案建立 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-114">Create a Protobuf reference from a remote file specified by a URL.</span></span>
* <span data-ttu-id="c9fea-115">請確定已將正確的 gRPC 套件相依性新增至專案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-115">Ensure the correct gRPC package dependencies are added to the project.</span></span>

<span data-ttu-id="c9fea-116">例如，`Grpc.AspNetCore` 套件會新增至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c9fea-116">For example, the `Grpc.AspNetCore` package is added to a web app.</span></span> <span data-ttu-id="c9fea-117">`Grpc.AspNetCore` 包含 gRPC 伺服器和用戶端程式庫和工具支援。</span><span class="sxs-lookup"><span data-stu-id="c9fea-117">`Grpc.AspNetCore` contains gRPC server and client libraries and tooling support.</span></span> <span data-ttu-id="c9fea-118">或者，只包含 gRPC 用戶端程式庫和工具支援的 `Grpc.Net.Client`、`Grpc.Tools` 和 `Google.Protobuf` 套件會新增至主控台應用程式。</span><span class="sxs-lookup"><span data-stu-id="c9fea-118">Alternatively, the `Grpc.Net.Client`, `Grpc.Tools` and `Google.Protobuf` packages, which contain only the gRPC client libraries and tooling support, are added to a Console app.</span></span>

### <a name="add-file"></a><span data-ttu-id="c9fea-119">新增檔案</span><span class="sxs-lookup"><span data-stu-id="c9fea-119">Add file</span></span>

<span data-ttu-id="c9fea-120">`add-file` 命令是用來將磁片上的本機檔案新增為 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-120">The `add-file` command is used to add local files on disk as Protobuf references.</span></span> <span data-ttu-id="c9fea-121">提供的檔案路徑：</span><span class="sxs-lookup"><span data-stu-id="c9fea-121">The file paths provided:</span></span>

* <span data-ttu-id="c9fea-122">可以相對於目前的目錄或絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-122">Can be relative to the current directory or absolute paths.</span></span>
* <span data-ttu-id="c9fea-123">可能包含以模式為基礎之檔案[通配](https://wikipedia.org/wiki/Glob_(programming))的萬用字元。</span><span class="sxs-lookup"><span data-stu-id="c9fea-123">May contain wild cards for pattern-based file [globbing](https://wikipedia.org/wiki/Glob_(programming)).</span></span>

<span data-ttu-id="c9fea-124">如果有任何檔案位於專案目錄外，則會加入 `Link` 元素，以在 Visual Studio 中的資料夾 `Protos` 顯示檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-124">If any files are outside the project directory, a `Link` element is added to display the file under the folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="c9fea-125">使用量</span><span class="sxs-lookup"><span data-stu-id="c9fea-125">Usage</span></span>

```dotnetcli
dotnet grpc add-file [options] <files>...
```

#### <a name="arguments"></a><span data-ttu-id="c9fea-126">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-126">Arguments</span></span>

| <span data-ttu-id="c9fea-127">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-127">Argument</span></span> | <span data-ttu-id="c9fea-128">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-128">Description</span></span> |
|-|-|
| <span data-ttu-id="c9fea-129">個檔案</span><span class="sxs-lookup"><span data-stu-id="c9fea-129">files</span></span> | <span data-ttu-id="c9fea-130">Protobuf 檔案會參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-130">The protobuf file references.</span></span> <span data-ttu-id="c9fea-131">這些可以是本機 protobuf 檔案的 glob 路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-131">These can be a path to glob for local protobuf files.</span></span> |

#### <a name="options"></a><span data-ttu-id="c9fea-132">選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-132">Options</span></span>

| <span data-ttu-id="c9fea-133">Short 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-133">Short option</span></span> | <span data-ttu-id="c9fea-134">Long 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-134">Long option</span></span> | <span data-ttu-id="c9fea-135">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-135">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c9fea-136">-p</span><span class="sxs-lookup"><span data-stu-id="c9fea-136">-p</span></span> | <span data-ttu-id="c9fea-137">--project</span><span class="sxs-lookup"><span data-stu-id="c9fea-137">--project</span></span> | <span data-ttu-id="c9fea-138">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-138">The path to the project file to operate on.</span></span> <span data-ttu-id="c9fea-139">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-139">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="c9fea-140">-s</span><span class="sxs-lookup"><span data-stu-id="c9fea-140">-s</span></span> | <span data-ttu-id="c9fea-141">--服務</span><span class="sxs-lookup"><span data-stu-id="c9fea-141">--services</span></span> | <span data-ttu-id="c9fea-142">應產生的 gRPC 服務類型。</span><span class="sxs-lookup"><span data-stu-id="c9fea-142">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="c9fea-143">如果指定了 `Default`，則會將 `Both` 用於 Web 專案，而 `Client` 則用於非 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-143">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="c9fea-144">接受的值為 `Both`，`Client`，`Default`，`None`，`Server`。</span><span class="sxs-lookup"><span data-stu-id="c9fea-144">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="c9fea-145">-i</span><span class="sxs-lookup"><span data-stu-id="c9fea-145">-i</span></span> | <span data-ttu-id="c9fea-146">--其他-匯入-目錄</span><span class="sxs-lookup"><span data-stu-id="c9fea-146">--additional-import-dirs</span></span> | <span data-ttu-id="c9fea-147">解析 protobuf 檔案的匯入時，所要使用的其他目錄。</span><span class="sxs-lookup"><span data-stu-id="c9fea-147">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="c9fea-148">這是以分號分隔的路徑清單。</span><span class="sxs-lookup"><span data-stu-id="c9fea-148">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="c9fea-149">--access</span><span class="sxs-lookup"><span data-stu-id="c9fea-149">--access</span></span> | <span data-ttu-id="c9fea-150">要用於產生C#之類別的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="c9fea-150">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="c9fea-151">預設值為 `Public`。</span><span class="sxs-lookup"><span data-stu-id="c9fea-151">The default value is `Public`.</span></span> <span data-ttu-id="c9fea-152">接受的值為 `Internal`，`Public`。</span><span class="sxs-lookup"><span data-stu-id="c9fea-152">Accepted values are `Internal` and `Public`.</span></span>

### <a name="add-url"></a><span data-ttu-id="c9fea-153">新增 URL</span><span class="sxs-lookup"><span data-stu-id="c9fea-153">Add URL</span></span>

<span data-ttu-id="c9fea-154">`add-url` 命令是用來將來源 URL 所指定的遠端檔案新增為 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-154">The `add-url` command is used to add a remote file specified by an source URL as Protobuf reference.</span></span> <span data-ttu-id="c9fea-155">必須提供檔案路徑，以指定要下載遠端檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="c9fea-155">A file path must be provided to specify where to download the remote file.</span></span> <span data-ttu-id="c9fea-156">檔案路徑可以相對於目前的目錄或絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-156">The file path can be relative to the current directory or an absolute path.</span></span> <span data-ttu-id="c9fea-157">如果檔案路徑位於專案目錄外，則會新增 `Link` 元素，以在 Visual Studio 中的虛擬資料夾 `Protos` 顯示該檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-157">If the file path is outside the project directory, a `Link` element is added to display the file under the virtual folder `Protos` in Visual Studio.</span></span>

### <a name="usage"></a><span data-ttu-id="c9fea-158">使用量</span><span class="sxs-lookup"><span data-stu-id="c9fea-158">Usage</span></span>

```dotnetcli
dotnet-grpc add-url [options] <url>
```

#### <a name="arguments"></a><span data-ttu-id="c9fea-159">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-159">Arguments</span></span>

| <span data-ttu-id="c9fea-160">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-160">Argument</span></span> | <span data-ttu-id="c9fea-161">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-161">Description</span></span> |
|-|-|
| <span data-ttu-id="c9fea-162">URL</span><span class="sxs-lookup"><span data-stu-id="c9fea-162">url</span></span> | <span data-ttu-id="c9fea-163">遠端 protobuf 檔的 URL。</span><span class="sxs-lookup"><span data-stu-id="c9fea-163">The URL to a remote protobuf file.</span></span> |

#### <a name="options"></a><span data-ttu-id="c9fea-164">選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-164">Options</span></span>

| <span data-ttu-id="c9fea-165">Short 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-165">Short option</span></span> | <span data-ttu-id="c9fea-166">Long 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-166">Long option</span></span> | <span data-ttu-id="c9fea-167">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-167">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c9fea-168">-o</span><span class="sxs-lookup"><span data-stu-id="c9fea-168">-o</span></span> | <span data-ttu-id="c9fea-169">--output</span><span class="sxs-lookup"><span data-stu-id="c9fea-169">--output</span></span> | <span data-ttu-id="c9fea-170">指定遠端 protobuf 檔的下載路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-170">Specifies the download path for the remote protobuf file.</span></span> <span data-ttu-id="c9fea-171">這是必要選項。</span><span class="sxs-lookup"><span data-stu-id="c9fea-171">This is a required option.</span></span>
| <span data-ttu-id="c9fea-172">-p</span><span class="sxs-lookup"><span data-stu-id="c9fea-172">-p</span></span> | <span data-ttu-id="c9fea-173">--project</span><span class="sxs-lookup"><span data-stu-id="c9fea-173">--project</span></span> | <span data-ttu-id="c9fea-174">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-174">The path to the project file to operate on.</span></span> <span data-ttu-id="c9fea-175">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-175">If a file is not specified, the command searches the current directory for one.</span></span>
| <span data-ttu-id="c9fea-176">-s</span><span class="sxs-lookup"><span data-stu-id="c9fea-176">-s</span></span> | <span data-ttu-id="c9fea-177">--服務</span><span class="sxs-lookup"><span data-stu-id="c9fea-177">--services</span></span> | <span data-ttu-id="c9fea-178">應產生的 gRPC 服務類型。</span><span class="sxs-lookup"><span data-stu-id="c9fea-178">The type of gRPC services that should be generated.</span></span> <span data-ttu-id="c9fea-179">如果指定了 `Default`，則會將 `Both` 用於 Web 專案，而 `Client` 則用於非 Web 專案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-179">If `Default` is specified, `Both` is used for Web projects and `Client` is used for non-Web projects.</span></span> <span data-ttu-id="c9fea-180">接受的值為 `Both`，`Client`，`Default`，`None`，`Server`。</span><span class="sxs-lookup"><span data-stu-id="c9fea-180">Accepted values are `Both`, `Client`, `Default`, `None`, `Server`.</span></span>
| <span data-ttu-id="c9fea-181">-i</span><span class="sxs-lookup"><span data-stu-id="c9fea-181">-i</span></span> | <span data-ttu-id="c9fea-182">--其他-匯入-目錄</span><span class="sxs-lookup"><span data-stu-id="c9fea-182">--additional-import-dirs</span></span> | <span data-ttu-id="c9fea-183">解析 protobuf 檔案的匯入時，所要使用的其他目錄。</span><span class="sxs-lookup"><span data-stu-id="c9fea-183">Additional directories to be used when resolving imports for the protobuf files.</span></span> <span data-ttu-id="c9fea-184">這是以分號分隔的路徑清單。</span><span class="sxs-lookup"><span data-stu-id="c9fea-184">This is a semicolon separated list of paths.</span></span>
| | <span data-ttu-id="c9fea-185">--access</span><span class="sxs-lookup"><span data-stu-id="c9fea-185">--access</span></span> | <span data-ttu-id="c9fea-186">要用於產生C#之類別的存取修飾詞。</span><span class="sxs-lookup"><span data-stu-id="c9fea-186">The access modifier to use for the generated C# classes.</span></span> <span data-ttu-id="c9fea-187">預設值為 `Public`。</span><span class="sxs-lookup"><span data-stu-id="c9fea-187">Default value is `Public`.</span></span> <span data-ttu-id="c9fea-188">接受的值為 `Internal`，`Public`。</span><span class="sxs-lookup"><span data-stu-id="c9fea-188">Accepted values are `Internal` and `Public`.</span></span>

## <a name="remove"></a><span data-ttu-id="c9fea-189">移除</span><span class="sxs-lookup"><span data-stu-id="c9fea-189">Remove</span></span>

<span data-ttu-id="c9fea-190">`remove` 命令是用來從 *.csproj*檔案中移除 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-190">The `remove` command is used to remove Protobuf references from the *.csproj* file.</span></span> <span data-ttu-id="c9fea-191">命令接受路徑引數和來源 Url 做為引數。</span><span class="sxs-lookup"><span data-stu-id="c9fea-191">The command accepts path arguments and source URLs as arguments.</span></span> <span data-ttu-id="c9fea-192">工具：</span><span class="sxs-lookup"><span data-stu-id="c9fea-192">The tool:</span></span>

* <span data-ttu-id="c9fea-193">只會移除 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-193">Only removes the Protobuf reference.</span></span>
* <span data-ttu-id="c9fea-194">不會刪除此*proto*檔案，即使該檔案原本是從遠端 URL 下載也一樣。</span><span class="sxs-lookup"><span data-stu-id="c9fea-194">Does not delete the *.proto* file, even if it was originally downloaded from a remote URL.</span></span>

### <a name="usage"></a><span data-ttu-id="c9fea-195">使用量</span><span class="sxs-lookup"><span data-stu-id="c9fea-195">Usage</span></span>

```dotnetcli
dotnet-grpc remove [options] <references>...
```

### <a name="arguments"></a><span data-ttu-id="c9fea-196">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-196">Arguments</span></span>

| <span data-ttu-id="c9fea-197">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-197">Argument</span></span> | <span data-ttu-id="c9fea-198">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-198">Description</span></span> |
|-|-|
| <span data-ttu-id="c9fea-199">參考</span><span class="sxs-lookup"><span data-stu-id="c9fea-199">references</span></span> | <span data-ttu-id="c9fea-200">要移除之 protobuf 參考的 Url 或檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-200">The URLs or file paths of the protobuf references to remove.</span></span> |

### <a name="options"></a><span data-ttu-id="c9fea-201">選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-201">Options</span></span>

| <span data-ttu-id="c9fea-202">Short 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-202">Short option</span></span> | <span data-ttu-id="c9fea-203">Long 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-203">Long option</span></span> | <span data-ttu-id="c9fea-204">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-204">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c9fea-205">-p</span><span class="sxs-lookup"><span data-stu-id="c9fea-205">-p</span></span> | <span data-ttu-id="c9fea-206">--project</span><span class="sxs-lookup"><span data-stu-id="c9fea-206">--project</span></span> | <span data-ttu-id="c9fea-207">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-207">The path to the project file to operate on.</span></span> <span data-ttu-id="c9fea-208">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-208">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="refresh"></a><span data-ttu-id="c9fea-209">重新整理</span><span class="sxs-lookup"><span data-stu-id="c9fea-209">Refresh</span></span>

<span data-ttu-id="c9fea-210">`refresh` 命令是用來以來源 URL 的最新內容來更新遠端參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-210">The `refresh` command is used to update a remote reference with the latest content from the source URL.</span></span> <span data-ttu-id="c9fea-211">下載檔案路徑和來源 URL 都可以用來指定要更新的參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-211">Both the download file path and the source URL can be used to specify the reference to be updated.</span></span> <span data-ttu-id="c9fea-212">注意：</span><span class="sxs-lookup"><span data-stu-id="c9fea-212">Note:</span></span>

* <span data-ttu-id="c9fea-213">檔案內容的雜湊會進行比較，以判斷是否應該更新本機檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-213">The hashes of the file contents are compared to determine whether the local file should be updated.</span></span>
* <span data-ttu-id="c9fea-214">不會比較任何時間戳記資訊。</span><span class="sxs-lookup"><span data-stu-id="c9fea-214">No timestamp information is compared.</span></span>

<span data-ttu-id="c9fea-215">如果需要更新，此工具一律會將本機檔案取代為遠端檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-215">The tool always replaces the local file with the remote file if an update is needed.</span></span>

### <a name="usage"></a><span data-ttu-id="c9fea-216">使用量</span><span class="sxs-lookup"><span data-stu-id="c9fea-216">Usage</span></span>

```dotnetcli
dotnet-grpc refresh [options] [<references>...]
```

### <a name="arguments"></a><span data-ttu-id="c9fea-217">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-217">Arguments</span></span>

| <span data-ttu-id="c9fea-218">引數</span><span class="sxs-lookup"><span data-stu-id="c9fea-218">Argument</span></span> | <span data-ttu-id="c9fea-219">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-219">Description</span></span> |
|-|-|
| <span data-ttu-id="c9fea-220">參考</span><span class="sxs-lookup"><span data-stu-id="c9fea-220">references</span></span> | <span data-ttu-id="c9fea-221">應更新之遠端 protobuf 參考的 Url 或檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-221">The URLs or file paths to remote protobuf references that should be updated.</span></span> <span data-ttu-id="c9fea-222">將此引數保留空白以重新整理所有遠端參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-222">Leave this argument empty to refresh all remote references.</span></span> |

### <a name="options"></a><span data-ttu-id="c9fea-223">選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-223">Options</span></span>

| <span data-ttu-id="c9fea-224">Short 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-224">Short option</span></span> | <span data-ttu-id="c9fea-225">Long 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-225">Long option</span></span> | <span data-ttu-id="c9fea-226">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-226">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c9fea-227">-p</span><span class="sxs-lookup"><span data-stu-id="c9fea-227">-p</span></span> | <span data-ttu-id="c9fea-228">--project</span><span class="sxs-lookup"><span data-stu-id="c9fea-228">--project</span></span> | <span data-ttu-id="c9fea-229">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-229">The path to the project file to operate on.</span></span> <span data-ttu-id="c9fea-230">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-230">If a file is not specified, the command searches the current directory for one.</span></span>
| | <span data-ttu-id="c9fea-231">--試執行</span><span class="sxs-lookup"><span data-stu-id="c9fea-231">--dry-run</span></span> | <span data-ttu-id="c9fea-232">輸出會更新而不下載任何新內容的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="c9fea-232">Outputs a list of files that would be updated without downloading any new content.</span></span>

## <a name="list"></a><span data-ttu-id="c9fea-233">清單</span><span class="sxs-lookup"><span data-stu-id="c9fea-233">List</span></span>

<span data-ttu-id="c9fea-234">`list` 命令會用來顯示專案檔中的所有 Protobuf 參考。</span><span class="sxs-lookup"><span data-stu-id="c9fea-234">The `list` command is used to display all the Protobuf references in the project file.</span></span> <span data-ttu-id="c9fea-235">如果資料行的所有值都是預設值，可能會省略資料行。</span><span class="sxs-lookup"><span data-stu-id="c9fea-235">If all values of a column are default values, the column may be omitted.</span></span>

### <a name="usage"></a><span data-ttu-id="c9fea-236">使用量</span><span class="sxs-lookup"><span data-stu-id="c9fea-236">Usage</span></span>

```dotnetcli
dotnet-grpc list [options]
```

### <a name="options"></a><span data-ttu-id="c9fea-237">選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-237">Options</span></span>

| <span data-ttu-id="c9fea-238">Short 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-238">Short option</span></span> | <span data-ttu-id="c9fea-239">Long 選項</span><span class="sxs-lookup"><span data-stu-id="c9fea-239">Long option</span></span> | <span data-ttu-id="c9fea-240">描述</span><span class="sxs-lookup"><span data-stu-id="c9fea-240">Description</span></span> |
|-|-|-|
| <span data-ttu-id="c9fea-241">-p</span><span class="sxs-lookup"><span data-stu-id="c9fea-241">-p</span></span> | <span data-ttu-id="c9fea-242">--project</span><span class="sxs-lookup"><span data-stu-id="c9fea-242">--project</span></span> | <span data-ttu-id="c9fea-243">要操作之專案檔的路徑。</span><span class="sxs-lookup"><span data-stu-id="c9fea-243">The path to the project file to operate on.</span></span> <span data-ttu-id="c9fea-244">如果未指定檔案，此命令會在目前的目錄中搜尋一個檔案。</span><span class="sxs-lookup"><span data-stu-id="c9fea-244">If a file is not specified, the command searches the current directory for one.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9fea-245">其他資源</span><span class="sxs-lookup"><span data-stu-id="c9fea-245">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>