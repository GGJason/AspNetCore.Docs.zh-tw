---
title: 在 ASP.NET Core 中的開發中安全儲存應用程式秘密
author: rick-anderson
description: 瞭解如何在開發 ASP.NET Core 應用程式期間，將機密資訊儲存為應用程式秘密並加以取出。
ms.author: scaddie
ms.custom: mvc
ms.date: 4/20/2020
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
uid: security/app-secrets
ms.openlocfilehash: 917e698d34a5d4b6c2c3f4737c08f1a590f5df1a
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88017944"
---
# <a name="safe-storage-of-app-secrets-in-development-in-aspnet-core"></a><span data-ttu-id="134ea-103">在 ASP.NET Core 中的開發中安全儲存應用程式秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-103">Safe storage of app secrets in development in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="134ea-104">依[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Kirk Larkin](https://twitter.com/serpent5)、 [Daniel Roth](https://github.com/danroth27)和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="134ea-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="134ea-105">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="134ea-105">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="134ea-106">本檔說明在開發電腦上開發 ASP.NET Core 應用程式期間，儲存和取得敏感性資料的技術。</span><span class="sxs-lookup"><span data-stu-id="134ea-106">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="134ea-107">絕對不要將密碼或其他敏感性資料儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="134ea-107">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="134ea-108">生產秘密不應用於開發或測試。</span><span class="sxs-lookup"><span data-stu-id="134ea-108">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="134ea-109">秘密不應該與應用程式一起部署。</span><span class="sxs-lookup"><span data-stu-id="134ea-109">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="134ea-110">相反地，您應該透過受控制的方式（例如環境變數、Azure Key Vault 等），在生產環境中提供秘密。您可以使用[Azure Key Vault 設定提供者](xref:security/key-vault-configuration)來儲存及保護 Azure 測試和生產密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-110">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="134ea-111">環境變數</span><span class="sxs-lookup"><span data-stu-id="134ea-111">Environment variables</span></span>

<span data-ttu-id="134ea-112">環境變數是用來避免在程式碼或本機設定檔案中儲存應用程式秘密。</span><span class="sxs-lookup"><span data-stu-id="134ea-112">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="134ea-113">環境變數會覆寫所有先前指定之設定來源的設定值。</span><span class="sxs-lookup"><span data-stu-id="134ea-113">Environment variables override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="134ea-114">請考慮啟用**個別使用者帳戶**安全性的 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="134ea-114">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="134ea-115">預設的資料庫連接字串會包含在具有索引鍵之檔案的專案*appsettings.js*中 `DefaultConnection` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-115">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="134ea-116">預設連接字串適用于 LocalDB，它會在使用者模式中執行，而且不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-116">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="134ea-117">在應用程式部署期間， `DefaultConnection` 可以使用環境變數的值來覆寫金鑰值。</span><span class="sxs-lookup"><span data-stu-id="134ea-117">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="134ea-118">環境變數可能會儲存具有敏感性認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="134ea-118">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="134ea-119">環境變數通常會儲存為純文字、未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="134ea-119">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="134ea-120">如果電腦或進程遭到入侵，則不受信任的合作物件可以存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="134ea-120">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="134ea-121">可能需要其他措施來防止洩漏使用者秘密。</span><span class="sxs-lookup"><span data-stu-id="134ea-121">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="134ea-122">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="134ea-122">Secret Manager</span></span>

<span data-ttu-id="134ea-123">秘密管理員工具會在 ASP.NET Core 專案的開發期間儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="134ea-123">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="134ea-124">在此內容中，有一段敏感性資料是應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-124">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="134ea-125">應用程式密碼會儲存在專案樹狀結構的不同位置。</span><span class="sxs-lookup"><span data-stu-id="134ea-125">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="134ea-126">應用程式密碼會與特定專案建立關聯，或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="134ea-126">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="134ea-127">應用程式秘密不會簽入原始檔控制中。</span><span class="sxs-lookup"><span data-stu-id="134ea-127">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="134ea-128">秘密管理員工具不會加密儲存的秘密，也不應視為受信任的存放區。</span><span class="sxs-lookup"><span data-stu-id="134ea-128">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="134ea-129">僅供開發之用。</span><span class="sxs-lookup"><span data-stu-id="134ea-129">It's for development purposes only.</span></span> <span data-ttu-id="134ea-130">金鑰和值會儲存在使用者設定檔目錄的 JSON 設定檔中。</span><span class="sxs-lookup"><span data-stu-id="134ea-130">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="134ea-131">密碼管理員工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="134ea-131">How the Secret Manager tool works</span></span>

<span data-ttu-id="134ea-132">秘密管理員工具會將執行詳細資料（例如儲存值的位置和方式）抽象化出來。</span><span class="sxs-lookup"><span data-stu-id="134ea-132">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="134ea-133">您可以使用此工具，而不需要知道這些執行方式的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="134ea-133">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="134ea-134">這些值會儲存在本機電腦上系統保護的使用者設定檔資料夾中的 JSON 設定檔案：</span><span class="sxs-lookup"><span data-stu-id="134ea-134">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windows"></a>[<span data-ttu-id="134ea-135">Windows</span><span class="sxs-lookup"><span data-stu-id="134ea-135">Windows</span></span>](#tab/windows)

<span data-ttu-id="134ea-136">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="134ea-136">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[<span data-ttu-id="134ea-137">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="134ea-137">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="134ea-138">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="134ea-138">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="134ea-139">在先前的檔案路徑中， `<user_secrets_id>` 將取代為 `UserSecretsId` *.csproj*檔案中指定的值。</span><span class="sxs-lookup"><span data-stu-id="134ea-139">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="134ea-140">請勿撰寫依賴秘密管理員工具所儲存之資料位置或格式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-140">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="134ea-141">這些執行詳細資料可能會變更。</span><span class="sxs-lookup"><span data-stu-id="134ea-141">These implementation details may change.</span></span> <span data-ttu-id="134ea-142">例如，秘密值不會加密，但未來可能會是。</span><span class="sxs-lookup"><span data-stu-id="134ea-142">For example, the secret values aren't encrypted, but could be in the future.</span></span>

## <a name="enable-secret-storage"></a><span data-ttu-id="134ea-143">啟用秘密儲存</span><span class="sxs-lookup"><span data-stu-id="134ea-143">Enable secret storage</span></span>

<span data-ttu-id="134ea-144">「秘密管理員」工具會針對儲存在使用者設定檔中的專案特定設定進行操作。</span><span class="sxs-lookup"><span data-stu-id="134ea-144">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

<span data-ttu-id="134ea-145">「密碼管理員」工具 `init` 在 .NET Core SDK 3.0.100 或更新版本中包含一個命令。</span><span class="sxs-lookup"><span data-stu-id="134ea-145">The Secret Manager tool includes an `init` command in .NET Core SDK 3.0.100 or later.</span></span> <span data-ttu-id="134ea-146">若要使用使用者秘密，請在專案目錄中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-146">To use user secrets, run the following command in the project directory:</span></span>

```dotnetcli
dotnet user-secrets init
```

<span data-ttu-id="134ea-147">上述命令會 `UserSecretsId` 在 `PropertyGroup` *.csproj*檔案的中新增專案。</span><span class="sxs-lookup"><span data-stu-id="134ea-147">The preceding command adds a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="134ea-148">根據預設，的內部文字 `UserSecretsId` 是 GUID。</span><span class="sxs-lookup"><span data-stu-id="134ea-148">By default, the inner text of `UserSecretsId` is a GUID.</span></span> <span data-ttu-id="134ea-149">內部文字是任意的，但對專案而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="134ea-149">The inner text is arbitrary, but is unique to the project.</span></span>

[!code-xml[](app-secrets/samples/3.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

<span data-ttu-id="134ea-150">在 Visual Studio 中，以滑鼠右鍵按一下方案總管中的專案，然後從內容功能表中選取 [**管理使用者秘密**]。</span><span class="sxs-lookup"><span data-stu-id="134ea-150">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="134ea-151">此手勢會將已 `UserSecretsId` 填入 GUID 的元素新增至 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="134ea-151">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="134ea-152">設定密碼</span><span class="sxs-lookup"><span data-stu-id="134ea-152">Set a secret</span></span>

<span data-ttu-id="134ea-153">定義由金鑰和其值組成的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-153">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="134ea-154">此密碼與專案的值相關聯 `UserSecretsId` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-154">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="134ea-155">例如，從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-155">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="134ea-156">在上述範例中，冒號表示 `Movies` 是具有屬性的物件常值 `ServiceApiKey` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-156">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="134ea-157">秘密管理員工具也可以從其他目錄中使用。</span><span class="sxs-lookup"><span data-stu-id="134ea-157">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="134ea-158">使用 `--project` 選項來提供 *.csproj*檔案所在的檔案系統路徑。</span><span class="sxs-lookup"><span data-stu-id="134ea-158">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="134ea-159">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-159">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="134ea-160">Visual Studio 中的 JSON 結構簡維</span><span class="sxs-lookup"><span data-stu-id="134ea-160">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="134ea-161">Visual Studio 的 [**管理使用者秘密**] 手勢會在文字編輯器中開啟檔案*secrets.js* 。</span><span class="sxs-lookup"><span data-stu-id="134ea-161">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="134ea-162">將*secrets.js*的內容取代為要儲存的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="134ea-162">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="134ea-163">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-163">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="134ea-164">JSON 結構會在透過或進行修改之後壓平合併 `dotnet user-secrets remove` `dotnet user-secrets set` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-164">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="134ea-165">例如，執行會折迭 `dotnet user-secrets remove "Movies:ConnectionString"` `Movies` 物件常值。</span><span class="sxs-lookup"><span data-stu-id="134ea-165">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="134ea-166">修改過的檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="134ea-166">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="134ea-167">設定多個秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-167">Set multiple secrets</span></span>

<span data-ttu-id="134ea-168">您可以透過將 JSON 傳送至命令的方式來設定密碼批次 `set` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-168">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="134ea-169">在下列範例中，檔案內容*上的input.js*會以管道傳送至 `set` 命令。</span><span class="sxs-lookup"><span data-stu-id="134ea-169">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windows"></a>[<span data-ttu-id="134ea-170">Windows</span><span class="sxs-lookup"><span data-stu-id="134ea-170">Windows</span></span>](#tab/windows)

<span data-ttu-id="134ea-171">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-171">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[<span data-ttu-id="134ea-172">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="134ea-172">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="134ea-173">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-173">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="134ea-174">存取秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-174">Access a secret</span></span>

<span data-ttu-id="134ea-175">[ASP.NET Core 設定 API](xref:fundamentals/configuration/index)提供秘密管理員密碼的存取權。</span><span class="sxs-lookup"><span data-stu-id="134ea-175">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

<span data-ttu-id="134ea-176">當專案呼叫 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> 以預先設定的預設值來初始化主機的新實例時，會自動在開發模式中新增使用者秘密設定來源。</span><span class="sxs-lookup"><span data-stu-id="134ea-176">The user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="134ea-177">`CreateDefaultBuilder`<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>當為時 <xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName> 呼叫 <xref:Microsoft.Extensions.Hosting.EnvironmentName.Development> ：</span><span class="sxs-lookup"><span data-stu-id="134ea-177">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName> is <xref:Microsoft.Extensions.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program.cs?name=snippet_CreateHostBuilder&highlight=2)]

<span data-ttu-id="134ea-178">若 `CreateDefaultBuilder` 未呼叫，請呼叫以明確新增使用者秘密設定來源 <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> 。</span><span class="sxs-lookup"><span data-stu-id="134ea-178">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>.</span></span> <span data-ttu-id="134ea-179">`AddUserSecrets`只有當應用程式在開發環境中執行時才會呼叫，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="134ea-179">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Program2.cs?name=snippet_Host&highlight=6-9)]

<span data-ttu-id="134ea-180">您可以透過 API 來抓取使用者秘密 `Configuration` ：</span><span class="sxs-lookup"><span data-stu-id="134ea-180">User secrets can be retrieved via the `Configuration` API:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="134ea-181">將秘密對應至 POCO</span><span class="sxs-lookup"><span data-stu-id="134ea-181">Map secrets to a POCO</span></span>

<span data-ttu-id="134ea-182">將整個物件常值對應到 POCO (具有屬性) 的簡單 .NET 類別，對於匯總相關屬性很有用。</span><span class="sxs-lookup"><span data-stu-id="134ea-182">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-183">若要將上述密碼對應到 POCO，請使用 `Configuration` API 的[物件圖形](xref:fundamentals/configuration/index#bind-to-an-object-graph)系結功能。</span><span class="sxs-lookup"><span data-stu-id="134ea-183">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="134ea-184">下列程式碼會系結至自訂 `MovieSettings` POCO 並存取 `ServiceApiKey` 屬性值：</span><span class="sxs-lookup"><span data-stu-id="134ea-184">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

<span data-ttu-id="134ea-185">`Movies:ConnectionString`和 `Movies:ServiceApiKey` 密碼會對應至中的個別屬性 `MovieSettings` ：</span><span class="sxs-lookup"><span data-stu-id="134ea-185">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="134ea-186">使用秘密取代字串</span><span class="sxs-lookup"><span data-stu-id="134ea-186">String replacement with secrets</span></span>

<span data-ttu-id="134ea-187">以純文字儲存密碼並不安全。</span><span class="sxs-lookup"><span data-stu-id="134ea-187">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="134ea-188">例如，儲存在*appsettings.js*中的資料庫連接字串可能會包含指定使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="134ea-188">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/3.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="134ea-189">更安全的方法是將密碼儲存為秘密。</span><span class="sxs-lookup"><span data-stu-id="134ea-189">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="134ea-190">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-190">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="134ea-191">`Password`從*appsettings.json*的連接字串中移除索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="134ea-191">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="134ea-192">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-192">For example:</span></span>

[!code-json[](app-secrets/samples/3.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="134ea-193">您可以在物件的屬性上設定密碼的值 <xref:System.Data.SqlClient.SqlConnectionStringBuilder> <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> ，以完成連接字串：</span><span class="sxs-lookup"><span data-stu-id="134ea-193">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

[!code-csharp[](app-secrets/samples/3.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a><span data-ttu-id="134ea-194">列出秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-194">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-195">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-195">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="134ea-196">下列輸出會出現：</span><span class="sxs-lookup"><span data-stu-id="134ea-196">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="134ea-197">在上述範例中，索引鍵名稱中的冒號代表*secrets.js*中的物件階層。</span><span class="sxs-lookup"><span data-stu-id="134ea-197">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="134ea-198">移除單一秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-198">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-199">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-199">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="134ea-200">應用程式*在檔案上的secrets.js*已修改為移除與機碼相關聯的機碼值組 `MoviesConnectionString` ：</span><span class="sxs-lookup"><span data-stu-id="134ea-200">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="134ea-201">`dotnet user-secrets list`會顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="134ea-201">`dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="134ea-202">移除所有秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-202">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-203">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-203">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="134ea-204">應用程式的所有使用者秘密已從檔案中的*secrets.js*刪除：</span><span class="sxs-lookup"><span data-stu-id="134ea-204">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="134ea-205">`dotnet user-secrets list`執行會顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="134ea-205">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="134ea-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="134ea-206">Additional resources</span></span>

* <span data-ttu-id="134ea-207">如需從 IIS 存取秘密管理員的相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。</span><span class="sxs-lookup"><span data-stu-id="134ea-207">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="134ea-208">由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Daniel Roth](https://github.com/danroth27)和[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="134ea-208">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="134ea-209">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="134ea-209">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/app-secrets/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="134ea-210">本檔說明在開發電腦上開發 ASP.NET Core 應用程式期間，儲存和取得敏感性資料的技術。</span><span class="sxs-lookup"><span data-stu-id="134ea-210">This document explains techniques for storing and retrieving sensitive data during development of an ASP.NET Core app on a development machine.</span></span> <span data-ttu-id="134ea-211">絕對不要將密碼或其他敏感性資料儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="134ea-211">Never store passwords or other sensitive data in source code.</span></span> <span data-ttu-id="134ea-212">生產秘密不應用於開發或測試。</span><span class="sxs-lookup"><span data-stu-id="134ea-212">Production secrets shouldn't be used for development or test.</span></span> <span data-ttu-id="134ea-213">秘密不應該與應用程式一起部署。</span><span class="sxs-lookup"><span data-stu-id="134ea-213">Secrets shouldn't be deployed with the app.</span></span> <span data-ttu-id="134ea-214">相反地，您應該透過受控制的方式（例如環境變數、Azure Key Vault 等），在生產環境中提供秘密。您可以使用[Azure Key Vault 設定提供者](xref:security/key-vault-configuration)來儲存及保護 Azure 測試和生產密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-214">Instead, secrets should be made available in the production environment through a controlled means like environment variables, Azure Key Vault, etc. You can store and protect Azure test and production secrets with the [Azure Key Vault configuration provider](xref:security/key-vault-configuration).</span></span>

## <a name="environment-variables"></a><span data-ttu-id="134ea-215">環境變數</span><span class="sxs-lookup"><span data-stu-id="134ea-215">Environment variables</span></span>

<span data-ttu-id="134ea-216">環境變數是用來避免在程式碼或本機設定檔案中儲存應用程式秘密。</span><span class="sxs-lookup"><span data-stu-id="134ea-216">Environment variables are used to avoid storage of app secrets in code or in local configuration files.</span></span> <span data-ttu-id="134ea-217">環境變數會覆寫所有先前指定之設定來源的設定值。</span><span class="sxs-lookup"><span data-stu-id="134ea-217">Environment variables override configuration values for all previously specified configuration sources.</span></span>

<span data-ttu-id="134ea-218">請考慮啟用**個別使用者帳戶**安全性的 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="134ea-218">Consider an ASP.NET Core web app in which **Individual User Accounts** security is enabled.</span></span> <span data-ttu-id="134ea-219">預設的資料庫連接字串會包含在具有索引鍵之檔案的專案*appsettings.js*中 `DefaultConnection` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-219">A default database connection string is included in the project's *appsettings.json* file with the key `DefaultConnection`.</span></span> <span data-ttu-id="134ea-220">預設連接字串適用于 LocalDB，它會在使用者模式中執行，而且不需要密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-220">The default connection string is for LocalDB, which runs in user mode and doesn't require a password.</span></span> <span data-ttu-id="134ea-221">在應用程式部署期間， `DefaultConnection` 可以使用環境變數的值來覆寫金鑰值。</span><span class="sxs-lookup"><span data-stu-id="134ea-221">During app deployment, the `DefaultConnection` key value can be overridden with an environment variable's value.</span></span> <span data-ttu-id="134ea-222">環境變數可能會儲存具有敏感性認證的完整連接字串。</span><span class="sxs-lookup"><span data-stu-id="134ea-222">The environment variable may store the complete connection string with sensitive credentials.</span></span>

> [!WARNING]
> <span data-ttu-id="134ea-223">環境變數通常會儲存為純文字、未加密的文字。</span><span class="sxs-lookup"><span data-stu-id="134ea-223">Environment variables are generally stored in plain, unencrypted text.</span></span> <span data-ttu-id="134ea-224">如果電腦或進程遭到入侵，則不受信任的合作物件可以存取環境變數。</span><span class="sxs-lookup"><span data-stu-id="134ea-224">If the machine or process is compromised, environment variables can be accessed by untrusted parties.</span></span> <span data-ttu-id="134ea-225">可能需要其他措施來防止洩漏使用者秘密。</span><span class="sxs-lookup"><span data-stu-id="134ea-225">Additional measures to prevent disclosure of user secrets may be required.</span></span>

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="secret-manager"></a><span data-ttu-id="134ea-226">秘密管理員</span><span class="sxs-lookup"><span data-stu-id="134ea-226">Secret Manager</span></span>

<span data-ttu-id="134ea-227">秘密管理員工具會在 ASP.NET Core 專案的開發期間儲存機密資料。</span><span class="sxs-lookup"><span data-stu-id="134ea-227">The Secret Manager tool stores sensitive data during the development of an ASP.NET Core project.</span></span> <span data-ttu-id="134ea-228">在此內容中，有一段敏感性資料是應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-228">In this context, a piece of sensitive data is an app secret.</span></span> <span data-ttu-id="134ea-229">應用程式密碼會儲存在專案樹狀結構的不同位置。</span><span class="sxs-lookup"><span data-stu-id="134ea-229">App secrets are stored in a separate location from the project tree.</span></span> <span data-ttu-id="134ea-230">應用程式密碼會與特定專案建立關聯，或在數個專案之間共用。</span><span class="sxs-lookup"><span data-stu-id="134ea-230">The app secrets are associated with a specific project or shared across several projects.</span></span> <span data-ttu-id="134ea-231">應用程式秘密不會簽入原始檔控制中。</span><span class="sxs-lookup"><span data-stu-id="134ea-231">The app secrets aren't checked into source control.</span></span>

> [!WARNING]
> <span data-ttu-id="134ea-232">秘密管理員工具不會加密儲存的秘密，也不應視為受信任的存放區。</span><span class="sxs-lookup"><span data-stu-id="134ea-232">The Secret Manager tool doesn't encrypt the stored secrets and shouldn't be treated as a trusted store.</span></span> <span data-ttu-id="134ea-233">僅供開發之用。</span><span class="sxs-lookup"><span data-stu-id="134ea-233">It's for development purposes only.</span></span> <span data-ttu-id="134ea-234">金鑰和值會儲存在使用者設定檔目錄的 JSON 設定檔中。</span><span class="sxs-lookup"><span data-stu-id="134ea-234">The keys and values are stored in a JSON configuration file in the user profile directory.</span></span>

## <a name="how-the-secret-manager-tool-works"></a><span data-ttu-id="134ea-235">密碼管理員工具的運作方式</span><span class="sxs-lookup"><span data-stu-id="134ea-235">How the Secret Manager tool works</span></span>

<span data-ttu-id="134ea-236">秘密管理員工具會將執行詳細資料（例如儲存值的位置和方式）抽象化出來。</span><span class="sxs-lookup"><span data-stu-id="134ea-236">The Secret Manager tool abstracts away the implementation details, such as where and how the values are stored.</span></span> <span data-ttu-id="134ea-237">您可以使用此工具，而不需要知道這些執行方式的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="134ea-237">You can use the tool without knowing these implementation details.</span></span> <span data-ttu-id="134ea-238">這些值會儲存在本機電腦上系統保護的使用者設定檔資料夾中的 JSON 設定檔案：</span><span class="sxs-lookup"><span data-stu-id="134ea-238">The values are stored in a JSON configuration file in a system-protected user profile folder on the local machine:</span></span>

# <a name="windows"></a>[<span data-ttu-id="134ea-239">Windows</span><span class="sxs-lookup"><span data-stu-id="134ea-239">Windows</span></span>](#tab/windows)

<span data-ttu-id="134ea-240">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="134ea-240">File system path:</span></span>

`%APPDATA%\Microsoft\UserSecrets\<user_secrets_id>\secrets.json`

# <a name="linux--macos"></a>[<span data-ttu-id="134ea-241">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="134ea-241">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="134ea-242">檔案系統路徑：</span><span class="sxs-lookup"><span data-stu-id="134ea-242">File system path:</span></span>

`~/.microsoft/usersecrets/<user_secrets_id>/secrets.json`

---

<span data-ttu-id="134ea-243">在先前的檔案路徑中， `<user_secrets_id>` 將取代為 `UserSecretsId` *.csproj*檔案中指定的值。</span><span class="sxs-lookup"><span data-stu-id="134ea-243">In the preceding file paths, replace `<user_secrets_id>` with the `UserSecretsId` value specified in the *.csproj* file.</span></span>

<span data-ttu-id="134ea-244">請勿撰寫依賴秘密管理員工具所儲存之資料位置或格式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-244">Don't write code that depends on the location or format of data saved with the Secret Manager tool.</span></span> <span data-ttu-id="134ea-245">這些執行詳細資料可能會變更。</span><span class="sxs-lookup"><span data-stu-id="134ea-245">These implementation details may change.</span></span> <span data-ttu-id="134ea-246">例如，秘密值不會加密，但未來可能會是。</span><span class="sxs-lookup"><span data-stu-id="134ea-246">For example, the secret values aren't encrypted, but could be in the future.</span></span>

## <a name="enable-secret-storage"></a><span data-ttu-id="134ea-247">啟用秘密儲存</span><span class="sxs-lookup"><span data-stu-id="134ea-247">Enable secret storage</span></span>

<span data-ttu-id="134ea-248">「秘密管理員」工具會針對儲存在使用者設定檔中的專案特定設定進行操作。</span><span class="sxs-lookup"><span data-stu-id="134ea-248">The Secret Manager tool operates on project-specific configuration settings stored in your user profile.</span></span>

<span data-ttu-id="134ea-249">若要使用使用者秘密，請 `UserSecretsId` 在 .csproj 檔案的中定義元素。 `PropertyGroup` *.csproj*</span><span class="sxs-lookup"><span data-stu-id="134ea-249">To use user secrets, define a `UserSecretsId` element within a `PropertyGroup` of the *.csproj* file.</span></span> <span data-ttu-id="134ea-250">的內部文字 `UserSecretsId` 是任意的，但對專案而言是唯一的。</span><span class="sxs-lookup"><span data-stu-id="134ea-250">The inner text of `UserSecretsId` is arbitrary, but is unique to the project.</span></span> <span data-ttu-id="134ea-251">開發人員通常會產生的 GUID `UserSecretsId` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-251">Developers typically generate a GUID for the `UserSecretsId`.</span></span>

[!code-xml[](app-secrets/samples/2.x/UserSecrets/UserSecrets.csproj?name=snippet_PropertyGroup&highlight=3)]

> [!TIP]
> <span data-ttu-id="134ea-252">在 Visual Studio 中，以滑鼠右鍵按一下方案總管中的專案，然後從內容功能表中選取 [**管理使用者秘密**]。</span><span class="sxs-lookup"><span data-stu-id="134ea-252">In Visual Studio, right-click the project in Solution Explorer, and select **Manage User Secrets** from the context menu.</span></span> <span data-ttu-id="134ea-253">此手勢會將已 `UserSecretsId` 填入 GUID 的元素新增至 *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="134ea-253">This gesture adds a `UserSecretsId` element, populated with a GUID, to the *.csproj* file.</span></span>

## <a name="set-a-secret"></a><span data-ttu-id="134ea-254">設定密碼</span><span class="sxs-lookup"><span data-stu-id="134ea-254">Set a secret</span></span>

<span data-ttu-id="134ea-255">定義由金鑰和其值組成的應用程式密碼。</span><span class="sxs-lookup"><span data-stu-id="134ea-255">Define an app secret consisting of a key and its value.</span></span> <span data-ttu-id="134ea-256">此密碼與專案的值相關聯 `UserSecretsId` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-256">The secret is associated with the project's `UserSecretsId` value.</span></span> <span data-ttu-id="134ea-257">例如，從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-257">For example, run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345"
```

<span data-ttu-id="134ea-258">在上述範例中，冒號表示 `Movies` 是具有屬性的物件常值 `ServiceApiKey` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-258">In the preceding example, the colon denotes that `Movies` is an object literal with a `ServiceApiKey` property.</span></span>

<span data-ttu-id="134ea-259">秘密管理員工具也可以從其他目錄中使用。</span><span class="sxs-lookup"><span data-stu-id="134ea-259">The Secret Manager tool can be used from other directories too.</span></span> <span data-ttu-id="134ea-260">使用 `--project` 選項來提供 *.csproj*檔案所在的檔案系統路徑。</span><span class="sxs-lookup"><span data-stu-id="134ea-260">Use the `--project` option to supply the file system path at which the *.csproj* file exists.</span></span> <span data-ttu-id="134ea-261">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-261">For example:</span></span>

```dotnetcli
dotnet user-secrets set "Movies:ServiceApiKey" "12345" --project "C:\apps\WebApp1\src\WebApp1"
```

### <a name="json-structure-flattening-in-visual-studio"></a><span data-ttu-id="134ea-262">Visual Studio 中的 JSON 結構簡維</span><span class="sxs-lookup"><span data-stu-id="134ea-262">JSON structure flattening in Visual Studio</span></span>

<span data-ttu-id="134ea-263">Visual Studio 的 [**管理使用者秘密**] 手勢會在文字編輯器中開啟檔案*secrets.js* 。</span><span class="sxs-lookup"><span data-stu-id="134ea-263">Visual Studio's **Manage User Secrets** gesture opens a *secrets.json* file in the text editor.</span></span> <span data-ttu-id="134ea-264">將*secrets.js*的內容取代為要儲存的機碼值組。</span><span class="sxs-lookup"><span data-stu-id="134ea-264">Replace the contents of *secrets.json* with the key-value pairs to be stored.</span></span> <span data-ttu-id="134ea-265">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-265">For example:</span></span>

```json
{
  "Movies": {
    "ConnectionString": "Server=(localdb)\\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true",
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="134ea-266">JSON 結構會在透過或進行修改之後壓平合併 `dotnet user-secrets remove` `dotnet user-secrets set` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-266">The JSON structure is flattened after modifications via `dotnet user-secrets remove` or `dotnet user-secrets set`.</span></span> <span data-ttu-id="134ea-267">例如，執行會折迭 `dotnet user-secrets remove "Movies:ConnectionString"` `Movies` 物件常值。</span><span class="sxs-lookup"><span data-stu-id="134ea-267">For example, running `dotnet user-secrets remove "Movies:ConnectionString"` collapses the `Movies` object literal.</span></span> <span data-ttu-id="134ea-268">修改過的檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="134ea-268">The modified file resembles the following:</span></span>

```json
{
  "Movies:ServiceApiKey": "12345"
}
```

## <a name="set-multiple-secrets"></a><span data-ttu-id="134ea-269">設定多個秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-269">Set multiple secrets</span></span>

<span data-ttu-id="134ea-270">您可以透過將 JSON 傳送至命令的方式來設定密碼批次 `set` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-270">A batch of secrets can be set by piping JSON to the `set` command.</span></span> <span data-ttu-id="134ea-271">在下列範例中，檔案內容*上的input.js*會以管道傳送至 `set` 命令。</span><span class="sxs-lookup"><span data-stu-id="134ea-271">In the following example, the *input.json* file's contents are piped to the `set` command.</span></span>

# <a name="windows"></a>[<span data-ttu-id="134ea-272">Windows</span><span class="sxs-lookup"><span data-stu-id="134ea-272">Windows</span></span>](#tab/windows)

<span data-ttu-id="134ea-273">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-273">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  type .\input.json | dotnet user-secrets set
  ```

# <a name="linux--macos"></a>[<span data-ttu-id="134ea-274">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="134ea-274">Linux / macOS</span></span>](#tab/linux+macos)

<span data-ttu-id="134ea-275">開啟命令 shell，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-275">Open a command shell, and execute the following command:</span></span>

  ```dotnetcli
  cat ./input.json | dotnet user-secrets set
  ```

---

## <a name="access-a-secret"></a><span data-ttu-id="134ea-276">存取秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-276">Access a secret</span></span>

<span data-ttu-id="134ea-277">[ASP.NET Core 設定 API](xref:fundamentals/configuration/index)提供秘密管理員密碼的存取權。</span><span class="sxs-lookup"><span data-stu-id="134ea-277">The [ASP.NET Core Configuration API](xref:fundamentals/configuration/index) provides access to Secret Manager secrets.</span></span>

<span data-ttu-id="134ea-278">如果您的專案是以 .NET Framework 為目標，請安裝[Microsoft.Extensions.Configuration。Usersecrets.xml](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="134ea-278">If your project targets .NET Framework, install the [Microsoft.Extensions.Configuration.UserSecrets](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.UserSecrets) NuGet package.</span></span>

<span data-ttu-id="134ea-279">在 ASP.NET Core 2.0 或更新版本中，當專案呼叫 <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> 以預先設定的預設值來初始化主機的新實例時，就會自動在開發模式中新增使用者秘密設定來源。</span><span class="sxs-lookup"><span data-stu-id="134ea-279">In ASP.NET Core 2.0 or later, the user secrets configuration source is automatically added in development mode when the project calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> to initialize a new instance of the host with preconfigured defaults.</span></span> <span data-ttu-id="134ea-280">`CreateDefaultBuilder`<xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A>當為時 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> 呼叫 <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development> ：</span><span class="sxs-lookup"><span data-stu-id="134ea-280">`CreateDefaultBuilder` calls <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> when the <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.EnvironmentName> is <xref:Microsoft.AspNetCore.Hosting.EnvironmentName.Development>:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Program.cs?name=snippet_CreateWebHostBuilder&highlight=2)]

<span data-ttu-id="134ea-281">當 `CreateDefaultBuilder` 未呼叫時，請在函式中呼叫以明確新增使用者秘密設定來源 <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="134ea-281">When `CreateDefaultBuilder` isn't called, add the user secrets configuration source explicitly by calling <xref:Microsoft.Extensions.Configuration.UserSecretsConfigurationExtensions.AddUserSecrets%2A> in the `Startup` constructor.</span></span> <span data-ttu-id="134ea-282">`AddUserSecrets`只有當應用程式在開發環境中執行時才會呼叫，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="134ea-282">Call `AddUserSecrets` only when the app runs in the Development environment, as shown in the following example:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_StartupConstructor&highlight=12)]

<span data-ttu-id="134ea-283">您可以透過 API 來抓取使用者秘密 `Configuration` ：</span><span class="sxs-lookup"><span data-stu-id="134ea-283">User secrets can be retrieved via the `Configuration` API:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup.cs?name=snippet_StartupClass&highlight=14)]

## <a name="map-secrets-to-a-poco"></a><span data-ttu-id="134ea-284">將秘密對應至 POCO</span><span class="sxs-lookup"><span data-stu-id="134ea-284">Map secrets to a POCO</span></span>

<span data-ttu-id="134ea-285">將整個物件常值對應到 POCO (具有屬性) 的簡單 .NET 類別，對於匯總相關屬性很有用。</span><span class="sxs-lookup"><span data-stu-id="134ea-285">Mapping an entire object literal to a POCO (a simple .NET class with properties) is useful for aggregating related properties.</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-286">若要將上述密碼對應到 POCO，請使用 `Configuration` API 的[物件圖形](xref:fundamentals/configuration/index#bind-to-an-object-graph)系結功能。</span><span class="sxs-lookup"><span data-stu-id="134ea-286">To map the preceding secrets to a POCO, use the `Configuration` API's [object graph binding](xref:fundamentals/configuration/index#bind-to-an-object-graph) feature.</span></span> <span data-ttu-id="134ea-287">下列程式碼會系結至自訂 `MovieSettings` POCO 並存取 `ServiceApiKey` 屬性值：</span><span class="sxs-lookup"><span data-stu-id="134ea-287">The following code binds to a custom `MovieSettings` POCO and accesses the `ServiceApiKey` property value:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup3.cs?name=snippet_BindToObjectGraph)]

<span data-ttu-id="134ea-288">`Movies:ConnectionString`和 `Movies:ServiceApiKey` 密碼會對應至中的個別屬性 `MovieSettings` ：</span><span class="sxs-lookup"><span data-stu-id="134ea-288">The `Movies:ConnectionString` and `Movies:ServiceApiKey` secrets are mapped to the respective properties in `MovieSettings`:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Models/MovieSettings.cs?name=snippet_MovieSettingsClass)]

## <a name="string-replacement-with-secrets"></a><span data-ttu-id="134ea-289">使用秘密取代字串</span><span class="sxs-lookup"><span data-stu-id="134ea-289">String replacement with secrets</span></span>

<span data-ttu-id="134ea-290">以純文字儲存密碼並不安全。</span><span class="sxs-lookup"><span data-stu-id="134ea-290">Storing passwords in plain text is insecure.</span></span> <span data-ttu-id="134ea-291">例如，儲存在*appsettings.js*中的資料庫連接字串可能會包含指定使用者的密碼：</span><span class="sxs-lookup"><span data-stu-id="134ea-291">For example, a database connection string stored in *appsettings.json* may include a password for the specified user:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings-unsecure.json?highlight=3)]

<span data-ttu-id="134ea-292">更安全的方法是將密碼儲存為秘密。</span><span class="sxs-lookup"><span data-stu-id="134ea-292">A more secure approach is to store the password as a secret.</span></span> <span data-ttu-id="134ea-293">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-293">For example:</span></span>

```dotnetcli
dotnet user-secrets set "DbPassword" "pass123"
```

<span data-ttu-id="134ea-294">`Password`從*appsettings.json*的連接字串中移除索引鍵/值組。</span><span class="sxs-lookup"><span data-stu-id="134ea-294">Remove the `Password` key-value pair from the connection string in *appsettings.json*.</span></span> <span data-ttu-id="134ea-295">例如：</span><span class="sxs-lookup"><span data-stu-id="134ea-295">For example:</span></span>

[!code-json[](app-secrets/samples/2.x/UserSecrets/appsettings.json?highlight=3)]

<span data-ttu-id="134ea-296">您可以在物件的屬性上設定密碼的值 <xref:System.Data.SqlClient.SqlConnectionStringBuilder> <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> ，以完成連接字串：</span><span class="sxs-lookup"><span data-stu-id="134ea-296">The secret's value can be set on a <xref:System.Data.SqlClient.SqlConnectionStringBuilder> object's <xref:System.Data.SqlClient.SqlConnectionStringBuilder.Password%2A> property to complete the connection string:</span></span>

[!code-csharp[](app-secrets/samples/2.x/UserSecrets/Startup2.cs?name=snippet_StartupClass&highlight=14-17)]

## <a name="list-the-secrets"></a><span data-ttu-id="134ea-297">列出秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-297">List the secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-298">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-298">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets list
```

<span data-ttu-id="134ea-299">下列輸出會出現：</span><span class="sxs-lookup"><span data-stu-id="134ea-299">The following output appears:</span></span>

```console
Movies:ConnectionString = Server=(localdb)\mssqllocaldb;Database=Movie-1;Trusted_Connection=True;MultipleActiveResultSets=true
Movies:ServiceApiKey = 12345
```

<span data-ttu-id="134ea-300">在上述範例中，索引鍵名稱中的冒號代表*secrets.js*中的物件階層。</span><span class="sxs-lookup"><span data-stu-id="134ea-300">In the preceding example, a colon in the key names denotes the object hierarchy within *secrets.json*.</span></span>

## <a name="remove-a-single-secret"></a><span data-ttu-id="134ea-301">移除單一秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-301">Remove a single secret</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-302">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-302">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets remove "Movies:ConnectionString"
```

<span data-ttu-id="134ea-303">應用程式*在檔案上的secrets.js*已修改為移除與機碼相關聯的機碼值組 `MoviesConnectionString` ：</span><span class="sxs-lookup"><span data-stu-id="134ea-303">The app's *secrets.json* file was modified to remove the key-value pair associated with the `MoviesConnectionString` key:</span></span>

```json
{
  "Movies": {
    "ServiceApiKey": "12345"
  }
}
```

<span data-ttu-id="134ea-304">`dotnet user-secrets list`執行會顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="134ea-304">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
Movies:ServiceApiKey = 12345
```

## <a name="remove-all-secrets"></a><span data-ttu-id="134ea-305">移除所有秘密</span><span class="sxs-lookup"><span data-stu-id="134ea-305">Remove all secrets</span></span>

[!INCLUDE[secrets.json file](~/includes/app-secrets/secrets-json-file-and-text.md)]

<span data-ttu-id="134ea-306">從 *.csproj*檔案所在的目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="134ea-306">Run the following command from the directory in which the *.csproj* file exists:</span></span>

```dotnetcli
dotnet user-secrets clear
```

<span data-ttu-id="134ea-307">應用程式的所有使用者秘密已從檔案中的*secrets.js*刪除：</span><span class="sxs-lookup"><span data-stu-id="134ea-307">All user secrets for the app have been deleted from the *secrets.json* file:</span></span>

```json
{}
```

<span data-ttu-id="134ea-308">`dotnet user-secrets list`執行會顯示下列訊息：</span><span class="sxs-lookup"><span data-stu-id="134ea-308">Running `dotnet user-secrets list` displays the following message:</span></span>

```console
No secrets configured for this application.
```

## <a name="additional-resources"></a><span data-ttu-id="134ea-309">其他資源</span><span class="sxs-lookup"><span data-stu-id="134ea-309">Additional resources</span></span>

* <span data-ttu-id="134ea-310">如需從 IIS 存取秘密管理員的相關資訊，請參閱[此問題](https://github.com/dotnet/AspNetCore.Docs/issues/16328)。</span><span class="sxs-lookup"><span data-stu-id="134ea-310">See [this issue](https://github.com/dotnet/AspNetCore.Docs/issues/16328) for information on accessing Secret Manager from IIS.</span></span>
* <xref:fundamentals/configuration/index>
* <xref:security/key-vault-configuration>

::: moniker-end