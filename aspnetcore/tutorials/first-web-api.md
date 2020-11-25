---
title: 教學課程：使用 ASP.NET Core 建立 web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc, devx-track-js
ms.date: 08/13/2020
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
- Models
uid: tutorials/first-web-api
ms.openlocfilehash: 9299ba685c95ced522fc725854a66252e67fc799
ms.sourcegitcommit: aa85f2911792a1e4783bcabf0da3b3e7e218f63a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2020
ms.locfileid: "96024960"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="e4e77-103">教學課程：使用 ASP.NET Core 建立 web API</span><span class="sxs-lookup"><span data-stu-id="e4e77-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="e4e77-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Kirk Larkin](https://twitter.com/serpent5)和 [Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="e4e77-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e4e77-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="e4e77-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="e4e77-106">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="e4e77-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4e77-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-107">Create a web API project.</span></span>
> * <span data-ttu-id="e4e77-108">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e4e77-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e4e77-109">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e4e77-110">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="e4e77-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e4e77-111">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-111">Call the web API with Postman.</span></span>

<span data-ttu-id="e4e77-112">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="e4e77-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e4e77-113">概觀</span><span class="sxs-lookup"><span data-stu-id="e4e77-113">Overview</span></span>

<span data-ttu-id="e4e77-114">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="e4e77-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e4e77-115">API</span><span class="sxs-lookup"><span data-stu-id="e4e77-115">API</span></span> | <span data-ttu-id="e4e77-116">描述</span><span class="sxs-lookup"><span data-stu-id="e4e77-116">Description</span></span> | <span data-ttu-id="e4e77-117">Request body</span><span class="sxs-lookup"><span data-stu-id="e4e77-117">Request body</span></span> | <span data-ttu-id="e4e77-118">回應本文</span><span class="sxs-lookup"><span data-stu-id="e4e77-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|`GET /api/TodoItems` | <span data-ttu-id="e4e77-119">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-119">Get all to-do items</span></span> | <span data-ttu-id="e4e77-120">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-120">None</span></span> | <span data-ttu-id="e4e77-121">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="e4e77-121">Array of to-do items</span></span>|
|`GET /api/TodoItems/{id}` | <span data-ttu-id="e4e77-122">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="e4e77-122">Get an item by ID</span></span> | <span data-ttu-id="e4e77-123">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-123">None</span></span> | <span data-ttu-id="e4e77-124">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-124">To-do item</span></span>|
|`POST /api/TodoItems` | <span data-ttu-id="e4e77-125">新增記錄</span><span class="sxs-lookup"><span data-stu-id="e4e77-125">Add a new item</span></span> | <span data-ttu-id="e4e77-126">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-126">To-do item</span></span> | <span data-ttu-id="e4e77-127">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-127">To-do item</span></span> |
|`PUT /api/TodoItems/{id}` | <span data-ttu-id="e4e77-128">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-128">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e4e77-129">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-129">To-do item</span></span> | <span data-ttu-id="e4e77-130">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-130">None</span></span> |
|<span data-ttu-id="e4e77-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span></span> | <span data-ttu-id="e4e77-132">刪除專案 &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-132">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e4e77-133">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-133">None</span></span> | <span data-ttu-id="e4e77-134">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-134">None</span></span>|

<span data-ttu-id="e4e77-135">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="e4e77-135">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e4e77-141">必要條件</span><span class="sxs-lookup"><span data-stu-id="e4e77-141">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-142">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-5.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-143">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-5.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-144">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-144">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-5.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e4e77-145">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="e4e77-145">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-147">從 [ **檔案** ] 功能表選取 [ **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-147">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e4e77-148">選取 **ASP.NET Core Web 應用程式** 範本，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-148">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e4e77-149">將專案命名為 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-149">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e4e77-150">在 [ **建立新的 ASP.NET Core Web 應用程式** ] 對話方塊中，確認已選取 [ **.net Core** ] 和 [ **ASP.NET Core 5.0** ]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-150">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 5.0** are selected.</span></span> <span data-ttu-id="e4e77-151">選取 **API** 範本，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-151">Select the **API** template and click **Create**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/5/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4e77-154">開啟 [整合式終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機。</span><span class="sxs-lookup"><span data-stu-id="e4e77-154">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e4e77-155">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-155">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e4e77-156">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-156">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="e4e77-157">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-157">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e4e77-158">上述命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-158">The preceding commands:</span></span>

  * <span data-ttu-id="e4e77-159">建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。</span><span class="sxs-lookup"><span data-stu-id="e4e77-159">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e4e77-160">新增下一節需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-160">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-161">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4e77-162">選取 [檔案]**[新增解決方案]** > 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-162">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e4e77-164">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用程式**  >  **API**  >  **]**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-164">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="e4e77-165">在8.6 版或更新版本中，選取 [ **Web] 和 [主控台**  >  **應用程式**  >  **API**  >  **]**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-165">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>

  ![macOS API 範本選取專案](first-web-api-mac/_static/api_template.png)

* <span data-ttu-id="e4e77-167">在 [ **設定新的 ASP.NET Core WEB API** ] 對話方塊中，選取最新的 .net Core 5.X **目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-167">In the **Configure the new ASP.NET Core Web API** dialog, select the latest .NET Core 5.x **Target Framework**.</span></span> <span data-ttu-id="e4e77-168">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-168">Select **Next**.</span></span>

* <span data-ttu-id="e4e77-169">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-169">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="e4e77-171">在專案資料夾中開啟命令終端機，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-171">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-project"></a><span data-ttu-id="e4e77-172">測試專案</span><span class="sxs-lookup"><span data-stu-id="e4e77-172">Test the project</span></span>

<span data-ttu-id="e4e77-173">專案範本會建立 `WeatherForecast` 支援 [Swagger](xref:tutorials/web-api-help-pages-using-swagger)的 API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-173">The project template creates a `WeatherForecast` API with support for [Swagger](xref:tutorials/web-api-help-pages-using-swagger).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4e77-175">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="e4e77-175">Press Ctrl+F5 to run without the debugger.</span></span>

[!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="e4e77-176">Visual Studio 啟動：</span><span class="sxs-lookup"><span data-stu-id="e4e77-176">Visual Studio launches:</span></span>

* <span data-ttu-id="e4e77-177">IIS Express web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-177">The IIS Express web server.</span></span>
* <span data-ttu-id="e4e77-178">預設瀏覽器並流覽至 `https://localhost:<port>/swagger/index.html` ，其中 `<port>` 是隨機播放的埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-178">The default browser and navigates to `https://localhost:<port>/swagger/index.html`, where `<port>` is a randomly chosen port number.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-179">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-179">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/trustCertVSC.md)]

<span data-ttu-id="e4e77-180">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-180">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e4e77-181">在瀏覽器中，移至下列 URL： [https://localhost:5001/swagger](https://localhost:5001/swagger)</span><span class="sxs-lookup"><span data-stu-id="e4e77-181">In a browser, go to following URL: [https://localhost:5001/swagger](https://localhost:5001/swagger)</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-182">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-182">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e4e77-183">選取 [**執行**  >  **開始調試** 程式] 以啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-183">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e4e77-184">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-184">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e4e77-185">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-185">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e4e77-186">將 `/swagger` 附加至 URL (將 URL 變更為 `https://localhost:<port>/swagger`)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-186">Append `/swagger` to the URL (change the URL to `https://localhost:<port>/swagger`).</span></span>

---

<span data-ttu-id="e4e77-187">Swagger 頁面隨即 `/swagger/index.html` 顯示。</span><span class="sxs-lookup"><span data-stu-id="e4e77-187">The Swagger page `/swagger/index.html` is displayed.</span></span> <span data-ttu-id="e4e77-188">選取 [**立即**  >  **試用**]  >  **執行**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-188">Select **GET** > **Try it out** > **Execute**.</span></span> <span data-ttu-id="e4e77-189">頁面會顯示：</span><span class="sxs-lookup"><span data-stu-id="e4e77-189">The page displays:</span></span>

* <span data-ttu-id="e4e77-190">用來測試 WeatherForecast API 的 [捲曲](https://curl.haxx.se/) 命令。</span><span class="sxs-lookup"><span data-stu-id="e4e77-190">The [Curl](https://curl.haxx.se/) command to test the WeatherForecast API.</span></span>
* <span data-ttu-id="e4e77-191">用來測試 WeatherForecast API 的 URL。</span><span class="sxs-lookup"><span data-stu-id="e4e77-191">The URL to test the WeatherForecast API.</span></span>
* <span data-ttu-id="e4e77-192">回應碼、主體和標頭。</span><span class="sxs-lookup"><span data-stu-id="e4e77-192">The response code, body, and headers.</span></span>
* <span data-ttu-id="e4e77-193">具有媒體類型的下拉式清單方塊，以及範例值和架構。</span><span class="sxs-lookup"><span data-stu-id="e4e77-193">A drop down list box with media types and the example value and schema.</span></span>

<!-- Review: Do we care the IE generates several errors. It shows the data, but with  Unrecognized response type; displaying content as text.
-->
<span data-ttu-id="e4e77-194">Swagger 可用來產生 web Api 的實用檔和說明頁面。</span><span class="sxs-lookup"><span data-stu-id="e4e77-194">Swagger is used to generate useful documentation and help pages for web APIs.</span></span> <span data-ttu-id="e4e77-195">本教學課程著重于建立 web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-195">This tutorial focuses on creating a web API.</span></span> <span data-ttu-id="e4e77-196">如需 Swagger 的詳細資訊，請參閱 <xref:tutorials/web-api-help-pages-using-swagger> 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-196">For more information on Swagger, see <xref:tutorials/web-api-help-pages-using-swagger>.</span></span>

<span data-ttu-id="e4e77-197">複製並超過瀏覽器中的 **要求 URL** ：  `https://localhost:<port>/WeatherForecast`</span><span class="sxs-lookup"><span data-stu-id="e4e77-197">Copy and past the **Request URL** in the browser:  `https://localhost:<port>/WeatherForecast`</span></span>

<span data-ttu-id="e4e77-198">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="e4e77-198">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

### <a name="update-the-launchurl"></a><span data-ttu-id="e4e77-199">更新 launchUrl</span><span class="sxs-lookup"><span data-stu-id="e4e77-199">Update the launchUrl</span></span>

<span data-ttu-id="e4e77-200">在 *Properties\launchSettings.js開啟*，請 `launchUrl` 從更新 `"swagger"` 為 `"api/TodoItems"` ：</span><span class="sxs-lookup"><span data-stu-id="e4e77-200">In *Properties\launchSettings.json*, update `launchUrl` from `"swagger"` to `"api/TodoItems"`:</span></span>

```json
"launchUrl": "api/TodoItems",
```

<span data-ttu-id="e4e77-201">由於 Swagger 已移除，因此上述標記會將啟動的 URL 變更為在下列各節中新增之控制器的 GET 方法。</span><span class="sxs-lookup"><span data-stu-id="e4e77-201">Because Swagger has been removed, the preceding markup changes the URL that is launched to the GET method of the controller added in the following sections.</span></span>

## <a name="add-a-model-class"></a><span data-ttu-id="e4e77-202">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="e4e77-202">Add a model class</span></span>

<span data-ttu-id="e4e77-203">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-203">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e4e77-204">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-204">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-206">在 **方案總管** 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-206">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e4e77-207">選取 **[**  >  **新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4e77-208">為資料夾命名 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-208">Name the folder *Models*.</span></span>

* <span data-ttu-id="e4e77-209">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [**加入**  >  **類別**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-209">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e4e77-210">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-210">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e4e77-211">以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-211">Replace the template code with the following:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4e77-213">新增名為的資料夾 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-213">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e4e77-214">`TodoItem`使用下列程式碼，將類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e4e77-214">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-215">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-215">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4e77-216">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-216">Right-click the project.</span></span> <span data-ttu-id="e4e77-217">選取 **[**  >  **新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-217">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4e77-218">為資料夾命名 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-218">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e4e77-220">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 **Add** > [**新增** 檔案 > **一般**] > **空白類別**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-220">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e4e77-221">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-221">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e4e77-222">以下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-222">Replace the template code with the following:</span></span>

---

  [!code-csharp[](first-web-api/samples/5.x/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="e4e77-223">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e4e77-223">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e4e77-224">模型類別可移至專案中的任何位置，但依照慣例，會 *Models* 使用資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-224">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e4e77-225">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e4e77-225">Add a database context</span></span>

<span data-ttu-id="e4e77-226">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-226">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e4e77-227">此類別是透過衍生自 <xref:Microsoft.EntityFrameworkCore.DbContext?displayProperty=fullName> 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="e4e77-227">This class is created by deriving from the <xref:Microsoft.EntityFrameworkCore.DbContext?displayProperty=fullName> class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-228">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-228">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-nuget-packages"></a><span data-ttu-id="e4e77-229">新增 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="e4e77-229">Add NuGet packages</span></span>

* <span data-ttu-id="e4e77-230">在 [工具] 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-230">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="e4e77-231">選取 [瀏覽] 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-231">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
<!-- https://github.com/dotnet/AspNetCore.Docs/issues/19782 Delete this line at RTM -->
* <span data-ttu-id="e4e77-232">選取左窗格中的 [ **microsoft.entityframeworkcore** ]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-232">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="e4e77-233">選取右窗格中的 [專案] 核取方塊，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-233">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="e4e77-234">使用上述指示來新增 **Microsoft.entityframeworkcore InMemory** NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-234">Use the preceding instructions to add the **Microsoft.EntityFrameworkCore.InMemory** NuGet package.</span></span>

<!-- https://github.com/dotnet/AspNetCore.Docs/issues/19782 Update this image at RTM -->
<span data-ttu-id="e4e77-235">![NuGet 套件管理員](first-web-api/_static/5/vsNuGet.png) (英文)</span><span class="sxs-lookup"><span data-stu-id="e4e77-235">![NuGet Package Manager](first-web-api/_static/5/vsNuGet.png)</span></span>

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e4e77-236">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e4e77-236">Add the TodoContext database context</span></span>

* <span data-ttu-id="e4e77-237">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [**加入**  >  **類別**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-237">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e4e77-238">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-238">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-239">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-239">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e4e77-240">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-240">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e4e77-241">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-241">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/5.x/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e4e77-242">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e4e77-242">Register the database context</span></span>

<span data-ttu-id="e4e77-243">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e4e77-243">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e4e77-244">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-244">The container provides the service to controllers.</span></span>

<span data-ttu-id="e4e77-245">使用下列程式碼更新 *Startup.cs* ：</span><span class="sxs-lookup"><span data-stu-id="e4e77-245">Update *Startup.cs* with the following code:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e4e77-246">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-246">The preceding code:</span></span>

* <span data-ttu-id="e4e77-247">移除 Swagger 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-247">Removes the Swagger calls.</span></span>
* <span data-ttu-id="e4e77-248">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="e4e77-248">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e4e77-249">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-249">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e4e77-250">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-250">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e4e77-251">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="e4e77-251">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-252">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-252">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-253">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-253">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e4e77-254">選取 [新增]**[新增 Scaffold 項目]** > 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-254">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e4e77-255">選取 [使用 Entity Framework 執行動作的 API 控制器]，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-255">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="e4e77-256">在 [使用 Entity Framework 執行動作的 API 控制器] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="e4e77-256">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="e4e77-257">選取 **模型類別** 中的 [ **TodoItem (TodoApi] Models) 。**</span><span class="sxs-lookup"><span data-stu-id="e4e77-257">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="e4e77-258">選取 **資料內容類別** 中的 **TodoCoNtext (TodoApi Models) 。**</span><span class="sxs-lookup"><span data-stu-id="e4e77-258">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="e4e77-259">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-259">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-260">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-260">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4e77-261">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-261">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet tool update -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="e4e77-262">上述命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-262">The preceding commands:</span></span>

* <span data-ttu-id="e4e77-263">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-263">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e4e77-264">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-264">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e4e77-265">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-265">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e4e77-266">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-266">The generated code:</span></span>

* <span data-ttu-id="e4e77-267">標記具有屬性的類別 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-267">Marks the class with the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e4e77-268">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-268">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e4e77-269">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="e4e77-269">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e4e77-270">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-270">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e4e77-271">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e4e77-271">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="e4e77-272">的 ASP.NET Core 範本：</span><span class="sxs-lookup"><span data-stu-id="e4e77-272">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="e4e77-273">具有 views 的控制器包含 `[action]` 在路由範本中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-273">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="e4e77-274">API 控制器不包含 `[action]` 在路由範本中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-274">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="e4e77-275">當 `[action]` 權杖不在路由範本中時，會從路由中排除 [動作](xref:mvc/controllers/routing#action) 名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-275">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="e4e77-276">也就是，不會在相符的路由中使用動作的相關聯方法名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-276">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="update-the-posttodoitem-create-method"></a><span data-ttu-id="e4e77-277">更新 PostTodoItem create 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-277">Update the PostTodoItem create method</span></span>

<span data-ttu-id="e4e77-278">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="e4e77-278">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e4e77-279">上述程式碼是 HTTP POST 方法，如屬性所指示 [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-279">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e4e77-280">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="e4e77-280">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e4e77-281">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-281">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e4e77-282"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-282">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e4e77-283">如果成功，則傳回 [HTTP 201 狀態碼](https://developer.mozilla.org/docs/Web/HTTP/Status/201) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-283">Returns an [HTTP 201 status code](https://developer.mozilla.org/docs/Web/HTTP/Status/201) if successful.</span></span> <span data-ttu-id="e4e77-284">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="e4e77-284">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e4e77-285">將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="e4e77-285">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e4e77-286">`Location`標頭會指定新建立之待辦事項的[URI](https://developer.mozilla.org/docs/Glossary/URI) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-286">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e4e77-287">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-287">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e4e77-288">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="e4e77-288">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e4e77-289">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-289">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e4e77-290">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="e4e77-290">Install Postman</span></span>

<span data-ttu-id="e4e77-291">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-291">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e4e77-292">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e4e77-292">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e4e77-293">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-293">Start the web app.</span></span>
* <span data-ttu-id="e4e77-294">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="e4e77-294">Start Postman.</span></span>
* <span data-ttu-id="e4e77-295">停用 [SSL certificate verification] \(SSL 憑證驗證\)</span><span class="sxs-lookup"><span data-stu-id="e4e77-295">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="e4e77-296">從 [檔案]**[設定]** >  ([一般] 索引標籤)，停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-296">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="e4e77-297">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-297">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e4e77-298">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e4e77-298">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="e4e77-299">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-299">Create a new request.</span></span>
* <span data-ttu-id="e4e77-300">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-300">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e4e77-301">將 URI 設定為 `https://localhost:<port>/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-301">Set the URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e4e77-302">例如： `https://localhost:5001/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-302">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e4e77-303">選取 [Body] \(本文\) 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-303">Select the **Body** tab.</span></span>
* <span data-ttu-id="e4e77-304">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="e4e77-304">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e4e77-305">將類型設定為 **JSON (application/json)**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-305">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e4e77-306">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="e4e77-306">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e4e77-307">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-307">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e4e77-309">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="e4e77-309">Test the location header URI</span></span>

<span data-ttu-id="e4e77-310">您可以在瀏覽器中測試位置標頭 URI。</span><span class="sxs-lookup"><span data-stu-id="e4e77-310">The location header URI can be tested in the browser.</span></span> <span data-ttu-id="e4e77-311">複製位置標頭 URI，並將其貼到瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-311">Copy and paste the location header URI into the browser.</span></span>

<span data-ttu-id="e4e77-312">若要在 Postman 中進行測試：</span><span class="sxs-lookup"><span data-stu-id="e4e77-312">To test in Postman:</span></span>

* <span data-ttu-id="e4e77-313">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-313">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e4e77-314">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="e4e77-314">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* <span data-ttu-id="e4e77-316">將 HTTP 方法設為 `GET`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-316">Set the HTTP method to `GET`.</span></span>
* <span data-ttu-id="e4e77-317">將 URI 設定為 `https://localhost:<port>/api/TodoItems/1` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-317">Set the URI to `https://localhost:<port>/api/TodoItems/1`.</span></span> <span data-ttu-id="e4e77-318">例如： `https://localhost:5001/api/TodoItems/1` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-318">For example, `https://localhost:5001/api/TodoItems/1`.</span></span>
* <span data-ttu-id="e4e77-319">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-319">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e4e77-320">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-320">Examine the GET methods</span></span>

<span data-ttu-id="e4e77-321">系統會執行兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="e4e77-321">Two GET endpoints are implemented:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e4e77-322">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-322">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e4e77-323">例如：</span><span class="sxs-lookup"><span data-stu-id="e4e77-323">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="e4e77-324">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="e4e77-324">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e4e77-325">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="e4e77-325">Test Get with Postman</span></span>

* <span data-ttu-id="e4e77-326">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-326">Create a new request.</span></span>
* <span data-ttu-id="e4e77-327">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-327">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="e4e77-328">將要求 URI 設定為 `https://localhost:<port>/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-328">Set the request URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e4e77-329">例如： `https://localhost:5001/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-329">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e4e77-330">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-330">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e4e77-331">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-331">Select **Send**.</span></span>

<span data-ttu-id="e4e77-332">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-332">This app uses an in-memory database.</span></span> <span data-ttu-id="e4e77-333">如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="e4e77-333">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="e4e77-334">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-334">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e4e77-335">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="e4e77-335">Routing and URL paths</span></span>

<span data-ttu-id="e4e77-336">[`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="e4e77-336">The [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e4e77-337">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="e4e77-337">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e4e77-338">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="e4e77-338">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e4e77-339">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-339">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e4e77-340">在此範例中，控制器類別名稱是 **TodoItems** Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="e4e77-340">For this sample, the controller class name is **TodoItems** Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e4e77-341">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-341">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e4e77-342">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="e4e77-342">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e4e77-343">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="e4e77-343">This sample doesn't use a template.</span></span> <span data-ttu-id="e4e77-344">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-344">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e4e77-345">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="e4e77-345">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e4e77-346">當叫 `GetTodoItem` 用時，會將 `"{id}"` URL 中的值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="e4e77-346">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e4e77-347">傳回值</span><span class="sxs-lookup"><span data-stu-id="e4e77-347">Return values</span></span>

<span data-ttu-id="e4e77-348">和方法的傳回型 `GetTodoItems` 別 `GetTodoItem` 為 [ActionResult \<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-348">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e4e77-349">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="e4e77-349">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e4e77-350">如果沒有任何未處理的例外狀況，則此傳回型別的回應碼為 [200 OK](https://developer.mozilla.org/docs/Web/HTTP/Status/200)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-350">The response code for this return type is [200 OK](https://developer.mozilla.org/docs/Web/HTTP/Status/200), assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e4e77-351">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-351">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e4e77-352">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-352">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e4e77-353">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="e4e77-353">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e4e77-354">如果沒有任何專案符合要求的識別碼，方法會傳回 [404 狀態](https://developer.mozilla.org/docs/Web/HTTP/Status/404) <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-354">If no item matches the requested ID, the method returns a [404 status](https://developer.mozilla.org/docs/Web/HTTP/Status/404) <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> error code.</span></span>
* <span data-ttu-id="e4e77-355">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="e4e77-355">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e4e77-356">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="e4e77-356">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e4e77-357">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-357">The PutTodoItem method</span></span>

<span data-ttu-id="e4e77-358">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-358">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e4e77-359">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="e4e77-359">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e4e77-360">回應為 [204 (沒有內容) ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-360">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e4e77-361">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="e4e77-361">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e4e77-362">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-362">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e4e77-363">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="e4e77-363">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e4e77-364">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-364">Test the PutTodoItem method</span></span>

<span data-ttu-id="e4e77-365">這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-365">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e4e77-366">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-366">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e4e77-367">在進行 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-367">Call GET to ensure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e4e77-368">更新識別碼為1的待辦事項，並將其名稱設定為 `"feed fish"` ：</span><span class="sxs-lookup"><span data-stu-id="e4e77-368">Update the to-do item that has Id = 1 and set its name to `"feed fish"`:</span></span>

```json
  {
    "Id":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e4e77-369">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="e4e77-369">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e4e77-371">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-371">The DeleteTodoItem method</span></span>

<span data-ttu-id="e4e77-372">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-372">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e4e77-373">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-373">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e4e77-374">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="e4e77-374">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e4e77-375">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-375">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e4e77-376">設定要刪除之物件的 URI (例如 `https://localhost:5001/api/TodoItems/1`) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-376">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="e4e77-377">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-377">Select **Send**.</span></span>

<a name="over-post-v5"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="e4e77-378">防止過度張貼</span><span class="sxs-lookup"><span data-stu-id="e4e77-378">Prevent over-posting</span></span>

<span data-ttu-id="e4e77-379">範例應用程式目前會公開整個 `TodoItem` 物件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-379">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="e4e77-380">生產環境應用程式通常會限制使用模型子集來輸入和傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="e4e77-380">Production apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="e4e77-381">這項功能有許多原因，而且安全性是主要的原因。</span><span class="sxs-lookup"><span data-stu-id="e4e77-381">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="e4e77-382">模型的子集通常稱為資料傳輸物件 (DTO) 、輸入模型或視圖模型。</span><span class="sxs-lookup"><span data-stu-id="e4e77-382">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="e4e77-383">此文章中使用 **DTO** 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-383">**DTO** is used in this article.</span></span>

<span data-ttu-id="e4e77-384">DTO 可以用來：</span><span class="sxs-lookup"><span data-stu-id="e4e77-384">A DTO may be used to:</span></span>

* <span data-ttu-id="e4e77-385">防止過度張貼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-385">Prevent over-posting.</span></span>
* <span data-ttu-id="e4e77-386">隱藏用戶端不應該看到的屬性。</span><span class="sxs-lookup"><span data-stu-id="e4e77-386">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="e4e77-387">略過某些屬性，以減少承載大小。</span><span class="sxs-lookup"><span data-stu-id="e4e77-387">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="e4e77-388">壓平合併包含嵌套物件的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e4e77-388">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="e4e77-389">簡維物件圖形對於用戶端來說可能更方便。</span><span class="sxs-lookup"><span data-stu-id="e4e77-389">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="e4e77-390">若要示範 DTO 方法，請更新 `TodoItem` 類別以包含秘密欄位：</span><span class="sxs-lookup"><span data-stu-id="e4e77-390">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=8)]

<span data-ttu-id="e4e77-391">此應用程式必須隱藏秘密欄位，但系統管理應用程式可以選擇將它公開。</span><span class="sxs-lookup"><span data-stu-id="e4e77-391">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="e4e77-392">確認您可以張貼並取得秘密欄位。</span><span class="sxs-lookup"><span data-stu-id="e4e77-392">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="e4e77-393">建立 DTO 模型：</span><span class="sxs-lookup"><span data-stu-id="e4e77-393">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="e4e77-394">更新 `TodoItemsController` 以使用 `TodoItemDTO` ：</span><span class="sxs-lookup"><span data-stu-id="e4e77-394">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/5.x/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="e4e77-395">確認您無法張貼或取得秘密欄位。</span><span class="sxs-lookup"><span data-stu-id="e4e77-395">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e4e77-396">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="e4e77-396">Call the web API with JavaScript</span></span>

<span data-ttu-id="e4e77-397">請參閱 [教學課程：使用 JavaScript 呼叫 ASP.NET Core WEB API](xref:tutorials/web-api-javascript)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-397">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0 < aspnetcore-5.0"

<span data-ttu-id="e4e77-398">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="e4e77-398">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4e77-399">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-399">Create a web API project.</span></span>
> * <span data-ttu-id="e4e77-400">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e4e77-400">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e4e77-401">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-401">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e4e77-402">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="e4e77-402">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e4e77-403">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-403">Call the web API with Postman.</span></span>

<span data-ttu-id="e4e77-404">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="e4e77-404">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e4e77-405">概觀</span><span class="sxs-lookup"><span data-stu-id="e4e77-405">Overview</span></span>

<span data-ttu-id="e4e77-406">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="e4e77-406">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e4e77-407">API</span><span class="sxs-lookup"><span data-stu-id="e4e77-407">API</span></span> | <span data-ttu-id="e4e77-408">描述</span><span class="sxs-lookup"><span data-stu-id="e4e77-408">Description</span></span> | <span data-ttu-id="e4e77-409">Request body</span><span class="sxs-lookup"><span data-stu-id="e4e77-409">Request body</span></span> | <span data-ttu-id="e4e77-410">回應本文</span><span class="sxs-lookup"><span data-stu-id="e4e77-410">Response body</span></span> |
|--- | ---- | ---- | ---- |
|`GET /api/TodoItems` | <span data-ttu-id="e4e77-411">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-411">Get all to-do items</span></span> | <span data-ttu-id="e4e77-412">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-412">None</span></span> | <span data-ttu-id="e4e77-413">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="e4e77-413">Array of to-do items</span></span>|
|`GET /api/TodoItems/{id}` | <span data-ttu-id="e4e77-414">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="e4e77-414">Get an item by ID</span></span> | <span data-ttu-id="e4e77-415">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-415">None</span></span> | <span data-ttu-id="e4e77-416">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-416">To-do item</span></span>|
|`POST /api/TodoItems` | <span data-ttu-id="e4e77-417">新增記錄</span><span class="sxs-lookup"><span data-stu-id="e4e77-417">Add a new item</span></span> | <span data-ttu-id="e4e77-418">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-418">To-do item</span></span> | <span data-ttu-id="e4e77-419">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-419">To-do item</span></span> |
|`PUT /api/TodoItems/{id}` | <span data-ttu-id="e4e77-420">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-420">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e4e77-421">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-421">To-do item</span></span> | <span data-ttu-id="e4e77-422">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-422">None</span></span> |
|<span data-ttu-id="e4e77-423">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-423">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span></span> | <span data-ttu-id="e4e77-424">刪除專案 &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-424">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e4e77-425">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-425">None</span></span> | <span data-ttu-id="e4e77-426">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-426">None</span></span>|

<span data-ttu-id="e4e77-427">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="e4e77-427">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e4e77-433">必要條件</span><span class="sxs-lookup"><span data-stu-id="e4e77-433">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-434">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-434">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-436">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e4e77-437">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="e4e77-437">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-438">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-439">從 [ **檔案** ] 功能表選取 [ **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-439">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e4e77-440">選取 **ASP.NET Core Web 應用程式** 範本，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-440">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e4e77-441">將專案命名為 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-441">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e4e77-442">在 [ **建立新的 ASP.NET Core Web 應用程式** ] 對話方塊中，確認已選取 [ **.net Core** ] 和 [ **ASP.NET Core 3.1** ]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-442">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="e4e77-443">選取 **API** 範本，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-443">Select the **API** template and click **Create**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-445">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-445">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4e77-446">開啟 [整合式終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機。</span><span class="sxs-lookup"><span data-stu-id="e4e77-446">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e4e77-447">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-447">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e4e77-448">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-448">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="e4e77-449">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-449">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e4e77-450">上述命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-450">The preceding commands:</span></span>

  * <span data-ttu-id="e4e77-451">建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。</span><span class="sxs-lookup"><span data-stu-id="e4e77-451">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e4e77-452">新增下一節需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-452">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-453">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-453">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4e77-454">選取 [檔案]**[新增解決方案]** > 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-454">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e4e77-456">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用程式**  >  **API**  >  **]**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-456">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="e4e77-457">在8.6 版或更新版本中，選取 [ **Web] 和 [主控台**  >  **應用程式**  >  **API**  >  **]**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-457">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>

  ![macOS API 範本選取專案](first-web-api-mac/_static/api_template.png)

* <span data-ttu-id="e4e77-459">在 [ **設定新的 ASP.NET Core WEB API** ] 對話方塊中，選取最新的 .net Core 3.X **目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-459">In the **Configure the new ASP.NET Core Web API** dialog, select the latest .NET Core 3.x **Target Framework**.</span></span> <span data-ttu-id="e4e77-460">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-460">Select **Next**.</span></span>

* <span data-ttu-id="e4e77-461">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-461">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="e4e77-463">在專案資料夾中開啟命令終端機，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-463">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="e4e77-464">測試 API</span><span class="sxs-lookup"><span data-stu-id="e4e77-464">Test the API</span></span>

<span data-ttu-id="e4e77-465">專案範本會建立 `WeatherForecast` API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-465">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="e4e77-466">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-466">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-467">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-467">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4e77-468">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-468">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e4e77-469">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="e4e77-469">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e4e77-470">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-470">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e4e77-471">在接著出現的 [安全性警告] 對話方塊中，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-471">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-472">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-472">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e4e77-473">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-473">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e4e77-474">在瀏覽器中，前往下列 URL：`https://localhost:5001/WeatherForecast`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-474">In a browser, go to following URL: `https://localhost:5001/WeatherForecast`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-475">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-475">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e4e77-476">選取 [**執行**  >  **開始調試** 程式] 以啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-476">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e4e77-477">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-477">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e4e77-478">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-478">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e4e77-479">將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-479">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="e4e77-480">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="e4e77-480">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="e4e77-481">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="e4e77-481">Add a model class</span></span>

<span data-ttu-id="e4e77-482">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-482">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e4e77-483">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-483">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-484">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-484">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-485">在 **方案總管** 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-485">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e4e77-486">選取 **[**  >  **新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-486">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4e77-487">為資料夾命名 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-487">Name the folder *Models*.</span></span>

* <span data-ttu-id="e4e77-488">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [**加入**  >  **類別**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-488">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e4e77-489">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-489">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e4e77-490">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-490">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-491">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-491">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4e77-492">新增名為的資料夾 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-492">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e4e77-493">`TodoItem`使用下列程式碼，將類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e4e77-493">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-494">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-494">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4e77-495">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-495">Right-click the project.</span></span> <span data-ttu-id="e4e77-496">選取 **[**  >  **新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-496">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4e77-497">為資料夾命名 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-497">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e4e77-499">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 **Add** > [**新增** 檔案 > **一般**] > **空白類別**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-499">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e4e77-500">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-500">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e4e77-501">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-501">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="e4e77-502">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e4e77-502">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e4e77-503">模型類別可移至專案中的任何位置，但依照慣例，會 *Models* 使用資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-503">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e4e77-504">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e4e77-504">Add a database context</span></span>

<span data-ttu-id="e4e77-505">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-505">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e4e77-506">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="e4e77-506">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-507">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-507">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-nuget-packages"></a><span data-ttu-id="e4e77-508">新增 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="e4e77-508">Add NuGet packages</span></span>

* <span data-ttu-id="e4e77-509">在 [工具] 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-509">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="e4e77-510">選取 [瀏覽] 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-510">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="e4e77-511">選取左窗格中的 [ **microsoft.entityframeworkcore** ]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-511">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="e4e77-512">選取右窗格中的 [專案] 核取方塊，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-512">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="e4e77-513">使用上述指示來新增 **Microsoft.entityframeworkcore InMemory** NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-513">Use the preceding instructions to add the **Microsoft.EntityFrameworkCore.InMemory** NuGet package.</span></span>

![NuGet 套件管理員](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e4e77-515">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e4e77-515">Add the TodoContext database context</span></span>

* <span data-ttu-id="e4e77-516">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [**加入**  >  **類別**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-516">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e4e77-517">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-517">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-518">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-518">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e4e77-519">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-519">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e4e77-520">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-520">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e4e77-521">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e4e77-521">Register the database context</span></span>

<span data-ttu-id="e4e77-522">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e4e77-522">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e4e77-523">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-523">The container provides the service to controllers.</span></span>

<span data-ttu-id="e4e77-524">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="e4e77-524">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e4e77-525">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-525">The preceding code:</span></span>

* <span data-ttu-id="e4e77-526">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="e4e77-526">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e4e77-527">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-527">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e4e77-528">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-528">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e4e77-529">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="e4e77-529">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-530">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-530">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-531">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-531">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e4e77-532">選取 [新增]**[新增 Scaffold 項目]** > 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-532">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="e4e77-533">選取 [使用 Entity Framework 執行動作的 API 控制器]，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-533">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="e4e77-534">在 [使用 Entity Framework 執行動作的 API 控制器] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="e4e77-534">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="e4e77-535">選取 **模型類別** 中的 [ **TodoItem (TodoApi] Models) 。**</span><span class="sxs-lookup"><span data-stu-id="e4e77-535">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="e4e77-536">選取 **資料內容類別** 中的 **TodoCoNtext (TodoApi Models) 。**</span><span class="sxs-lookup"><span data-stu-id="e4e77-536">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="e4e77-537">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-537">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-538">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-538">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e4e77-539">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-539">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet tool update -g Dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="e4e77-540">上述命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-540">The preceding commands:</span></span>

* <span data-ttu-id="e4e77-541">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-541">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e4e77-542">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-542">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e4e77-543">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-543">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e4e77-544">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-544">The generated code:</span></span>

* <span data-ttu-id="e4e77-545">標記具有屬性的類別 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-545">Marks the class with the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e4e77-546">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-546">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e4e77-547">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="e4e77-547">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e4e77-548">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-548">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e4e77-549">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e4e77-549">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="e4e77-550">的 ASP.NET Core 範本：</span><span class="sxs-lookup"><span data-stu-id="e4e77-550">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="e4e77-551">具有 views 的控制器包含 `[action]` 在路由範本中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-551">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="e4e77-552">API 控制器不包含 `[action]` 在路由範本中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-552">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="e4e77-553">當 `[action]` 權杖不在路由範本中時，會從路由中排除 [動作](xref:mvc/controllers/routing#action) 名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-553">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="e4e77-554">也就是，不會在相符的路由中使用動作的相關聯方法名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-554">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="e4e77-555">檢查 PostTodoItem 建立方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-555">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="e4e77-556">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="e4e77-556">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e4e77-557">上述程式碼是 HTTP POST 方法，如屬性所指示 [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-557">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e4e77-558">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="e4e77-558">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e4e77-559">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-559">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e4e77-560"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-560">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e4e77-561">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-561">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="e4e77-562">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="e4e77-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e4e77-563">將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) 標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="e4e77-563">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e4e77-564">`Location`標頭會指定新建立之待辦事項的[URI](https://developer.mozilla.org/docs/Glossary/URI) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-564">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e4e77-565">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e4e77-566">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="e4e77-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e4e77-567">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e4e77-568">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="e4e77-568">Install Postman</span></span>

<span data-ttu-id="e4e77-569">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-569">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e4e77-570">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e4e77-570">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="e4e77-571">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-571">Start the web app.</span></span>
* <span data-ttu-id="e4e77-572">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="e4e77-572">Start Postman.</span></span>
* <span data-ttu-id="e4e77-573">停用 [SSL certificate verification] \(SSL 憑證驗證\)</span><span class="sxs-lookup"><span data-stu-id="e4e77-573">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="e4e77-574">從 [檔案]**[設定]** >  ([一般] 索引標籤)，停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-574">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="e4e77-575">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-575">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e4e77-576">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e4e77-576">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="e4e77-577">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-577">Create a new request.</span></span>
* <span data-ttu-id="e4e77-578">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-578">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e4e77-579">將 URI 設定為 `https://localhost:<port>/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-579">Set the URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e4e77-580">例如： `https://localhost:5001/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-580">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e4e77-581">選取 [Body] \(本文\) 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-581">Select the **Body** tab.</span></span>
* <span data-ttu-id="e4e77-582">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="e4e77-582">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e4e77-583">將類型設定為 **JSON (application/json)**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-583">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e4e77-584">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="e4e77-584">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e4e77-585">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-585">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri-with-postman"></a><span data-ttu-id="e4e77-587">使用 Postman 測試 location 標頭 URI</span><span class="sxs-lookup"><span data-stu-id="e4e77-587">Test the location header URI with Postman</span></span>

* <span data-ttu-id="e4e77-588">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-588">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e4e77-589">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="e4e77-589">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* <span data-ttu-id="e4e77-591">將 HTTP 方法設為 `GET`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-591">Set the HTTP method to `GET`.</span></span>
* <span data-ttu-id="e4e77-592">將 URI 設定為 `https://localhost:<port>/api/TodoItems/1` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-592">Set the URI to `https://localhost:<port>/api/TodoItems/1`.</span></span> <span data-ttu-id="e4e77-593">例如： `https://localhost:5001/api/TodoItems/1` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-593">For example, `https://localhost:5001/api/TodoItems/1`.</span></span>
* <span data-ttu-id="e4e77-594">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-594">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e4e77-595">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-595">Examine the GET methods</span></span>

<span data-ttu-id="e4e77-596">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="e4e77-596">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e4e77-597">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-597">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e4e77-598">例如：</span><span class="sxs-lookup"><span data-stu-id="e4e77-598">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="e4e77-599">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="e4e77-599">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e4e77-600">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="e4e77-600">Test Get with Postman</span></span>

* <span data-ttu-id="e4e77-601">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-601">Create a new request.</span></span>
* <span data-ttu-id="e4e77-602">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-602">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="e4e77-603">將要求 URI 設定為 `https://localhost:<port>/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-603">Set the request URI to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e4e77-604">例如： `https://localhost:5001/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-604">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="e4e77-605">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-605">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e4e77-606">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-606">Select **Send**.</span></span>

<span data-ttu-id="e4e77-607">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-607">This app uses an in-memory database.</span></span> <span data-ttu-id="e4e77-608">如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="e4e77-608">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="e4e77-609">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-609">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e4e77-610">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="e4e77-610">Routing and URL paths</span></span>

<span data-ttu-id="e4e77-611">[`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="e4e77-611">The [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e4e77-612">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="e4e77-612">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e4e77-613">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="e4e77-613">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e4e77-614">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-614">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e4e77-615">在此範例中，控制器類別名稱是 **TodoItems** Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="e4e77-615">For this sample, the controller class name is **TodoItems** Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e4e77-616">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-616">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e4e77-617">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="e4e77-617">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e4e77-618">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="e4e77-618">This sample doesn't use a template.</span></span> <span data-ttu-id="e4e77-619">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-619">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e4e77-620">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="e4e77-620">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e4e77-621">當叫 `GetTodoItem` 用時，會將 `"{id}"` URL 中的值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="e4e77-621">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e4e77-622">傳回值</span><span class="sxs-lookup"><span data-stu-id="e4e77-622">Return values</span></span> 

<span data-ttu-id="e4e77-623">和方法的傳回型 `GetTodoItems` 別 `GetTodoItem` 為 [ActionResult \<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-623">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e4e77-624">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="e4e77-624">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e4e77-625">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e4e77-625">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e4e77-626">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-626">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e4e77-627">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-627">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e4e77-628">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="e4e77-628">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e4e77-629">如果沒有任何專案符合要求的識別碼，方法會傳回 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-629">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> error code.</span></span>
* <span data-ttu-id="e4e77-630">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="e4e77-630">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e4e77-631">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="e4e77-631">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e4e77-632">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-632">The PutTodoItem method</span></span>

<span data-ttu-id="e4e77-633">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-633">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e4e77-634">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="e4e77-634">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e4e77-635">回應為 [204 (沒有內容) ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-635">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e4e77-636">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="e4e77-636">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e4e77-637">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-637">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e4e77-638">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="e4e77-638">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e4e77-639">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-639">Test the PutTodoItem method</span></span>

<span data-ttu-id="e4e77-640">這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-640">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e4e77-641">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-641">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e4e77-642">在進行 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-642">Call GET to ensure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e4e77-643">更新識別碼為1的待辦事項，並將其名稱設定為「摘要魚」：</span><span class="sxs-lookup"><span data-stu-id="e4e77-643">Update the to-do item that has Id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "id":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e4e77-644">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="e4e77-644">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e4e77-646">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-646">The DeleteTodoItem method</span></span>

<span data-ttu-id="e4e77-647">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-647">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e4e77-648">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e4e77-648">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e4e77-649">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="e4e77-649">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e4e77-650">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-650">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e4e77-651">設定要刪除之物件的 URI (例如 `https://localhost:5001/api/TodoItems/1`) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-651">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="e4e77-652">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-652">Select **Send**.</span></span>

<a name="over-post"></a>
<a name="over-post-v3"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="e4e77-653">防止過度張貼</span><span class="sxs-lookup"><span data-stu-id="e4e77-653">Prevent over-posting</span></span>

<span data-ttu-id="e4e77-654">範例應用程式目前會公開整個 `TodoItem` 物件。</span><span class="sxs-lookup"><span data-stu-id="e4e77-654">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="e4e77-655">生產環境應用程式通常會限制使用模型子集來輸入和傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="e4e77-655">Production apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="e4e77-656">這項功能有許多原因，而且安全性是主要的原因。</span><span class="sxs-lookup"><span data-stu-id="e4e77-656">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="e4e77-657">模型的子集通常稱為資料傳輸物件 (DTO) 、輸入模型或視圖模型。</span><span class="sxs-lookup"><span data-stu-id="e4e77-657">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="e4e77-658">此文章中使用 **DTO** 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-658">**DTO** is used in this article.</span></span>

<span data-ttu-id="e4e77-659">DTO 可以用來：</span><span class="sxs-lookup"><span data-stu-id="e4e77-659">A DTO may be used to:</span></span>

* <span data-ttu-id="e4e77-660">防止過度張貼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-660">Prevent over-posting.</span></span>
* <span data-ttu-id="e4e77-661">隱藏用戶端不應該看到的屬性。</span><span class="sxs-lookup"><span data-stu-id="e4e77-661">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="e4e77-662">略過某些屬性，以減少承載大小。</span><span class="sxs-lookup"><span data-stu-id="e4e77-662">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="e4e77-663">壓平合併包含嵌套物件的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e4e77-663">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="e4e77-664">簡維物件圖形對於用戶端來說可能更方便。</span><span class="sxs-lookup"><span data-stu-id="e4e77-664">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="e4e77-665">若要示範 DTO 方法，請更新 `TodoItem` 類別以包含秘密欄位：</span><span class="sxs-lookup"><span data-stu-id="e4e77-665">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="e4e77-666">此應用程式必須隱藏秘密欄位，但系統管理應用程式可以選擇將它公開。</span><span class="sxs-lookup"><span data-stu-id="e4e77-666">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="e4e77-667">確認您可以張貼並取得秘密欄位。</span><span class="sxs-lookup"><span data-stu-id="e4e77-667">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="e4e77-668">建立 DTO 模型：</span><span class="sxs-lookup"><span data-stu-id="e4e77-668">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="e4e77-669">更新 `TodoItemsController` 以使用 `TodoItemDTO` ：</span><span class="sxs-lookup"><span data-stu-id="e4e77-669">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="e4e77-670">確認您無法張貼或取得秘密欄位。</span><span class="sxs-lookup"><span data-stu-id="e4e77-670">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e4e77-671">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="e4e77-671">Call the web API with JavaScript</span></span>

<span data-ttu-id="e4e77-672">請參閱 [教學課程：使用 JavaScript 呼叫 ASP.NET Core WEB API](xref:tutorials/web-api-javascript)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-672">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e4e77-673">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="e4e77-673">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e4e77-674">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-674">Create a web API project.</span></span>
> * <span data-ttu-id="e4e77-675">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e4e77-675">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e4e77-676">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-676">Add a controller.</span></span>
> * <span data-ttu-id="e4e77-677">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="e4e77-677">Add CRUD methods.</span></span>
> * <span data-ttu-id="e4e77-678">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="e4e77-678">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="e4e77-679">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="e4e77-679">Specify return values.</span></span>
> * <span data-ttu-id="e4e77-680">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-680">Call the web API with Postman.</span></span>
> * <span data-ttu-id="e4e77-681">使用 JavaScript 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-681">Call the web API with JavaScript.</span></span>

<span data-ttu-id="e4e77-682">結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-682">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview-21"></a><span data-ttu-id="e4e77-683">總覽2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-683">Overview 2.1</span></span>

<span data-ttu-id="e4e77-684">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="e4e77-684">This tutorial creates the following API:</span></span>

|<span data-ttu-id="e4e77-685">API</span><span class="sxs-lookup"><span data-stu-id="e4e77-685">API</span></span> | <span data-ttu-id="e4e77-686">描述</span><span class="sxs-lookup"><span data-stu-id="e4e77-686">Description</span></span> | <span data-ttu-id="e4e77-687">Request body</span><span class="sxs-lookup"><span data-stu-id="e4e77-687">Request body</span></span> | <span data-ttu-id="e4e77-688">回應本文</span><span class="sxs-lookup"><span data-stu-id="e4e77-688">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="e4e77-689">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e4e77-689">GET /api/TodoItems</span></span> | <span data-ttu-id="e4e77-690">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-690">Get all to-do items</span></span> | <span data-ttu-id="e4e77-691">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-691">None</span></span> | <span data-ttu-id="e4e77-692">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="e4e77-692">Array of to-do items</span></span>|
|<span data-ttu-id="e4e77-693">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="e4e77-693">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="e4e77-694">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="e4e77-694">Get an item by ID</span></span> | <span data-ttu-id="e4e77-695">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-695">None</span></span> | <span data-ttu-id="e4e77-696">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-696">To-do item</span></span>|
|<span data-ttu-id="e4e77-697">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e4e77-697">POST /api/TodoItems</span></span> | <span data-ttu-id="e4e77-698">新增記錄</span><span class="sxs-lookup"><span data-stu-id="e4e77-698">Add a new item</span></span> | <span data-ttu-id="e4e77-699">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-699">To-do item</span></span> | <span data-ttu-id="e4e77-700">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-700">To-do item</span></span> |
|<span data-ttu-id="e4e77-701">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="e4e77-701">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="e4e77-702">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-702">Update an existing item &nbsp;</span></span> | <span data-ttu-id="e4e77-703">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e4e77-703">To-do item</span></span> | <span data-ttu-id="e4e77-704">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-704">None</span></span> |
|<span data-ttu-id="e4e77-705">刪除/Api/todoitems/{識別碼} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-705">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="e4e77-706">刪除專案 &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="e4e77-706">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="e4e77-707">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-707">None</span></span> | <span data-ttu-id="e4e77-708">None</span><span class="sxs-lookup"><span data-stu-id="e4e77-708">None</span></span>|

<span data-ttu-id="e4e77-709">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="e4e77-709">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites-21"></a><span data-ttu-id="e4e77-715">必要條件2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-715">Prerequisites 2.1</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-716">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-716">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-717">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-717">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-718">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-718">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project-21"></a><span data-ttu-id="e4e77-719">建立 Web 專案2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-719">Create a web project 2.1</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-720">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-720">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-721">從 [ **檔案** ] 功能表選取 [ **新增** > **專案**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-721">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="e4e77-722">選取 **ASP.NET Core Web 應用程式** 範本，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-722">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="e4e77-723">將專案命名為 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-723">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="e4e77-724">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 2.2]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-724">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="e4e77-725">選取 **API** 範本，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-725">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="e4e77-726">請 **勿** 選取 [Enable Docker Support] \(啟用 Docker 支援\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-726">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-728">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-728">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4e77-729">開啟 [整合式終端](https://code.visualstudio.com/docs/editor/integrated-terminal)機。</span><span class="sxs-lookup"><span data-stu-id="e4e77-729">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="e4e77-730">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-730">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="e4e77-731">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e4e77-731">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="e4e77-732">這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。</span><span class="sxs-lookup"><span data-stu-id="e4e77-732">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="e4e77-733">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-733">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-734">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-734">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4e77-735">選取 [檔案]**[新增解決方案]** > 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-735">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="e4e77-737">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用程式**  >  **API**  >  **]**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-737">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **API** > **Next**.</span></span> <span data-ttu-id="e4e77-738">在8.6 版或更新版本中，選取 [ **Web] 和 [主控台**  >  **應用程式**  >  **API**  >  **]**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-738">In version 8.6 or later, select **Web and Console** > **App** > **API** > **Next**.</span></span>
  
* <span data-ttu-id="e4e77-739">在 [ **設定新的 ASP.NET Core WEB API** ] 對話方塊中，選取最新的 .net Core 2.X **目標 Framework**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-739">In the **Configure the new ASP.NET Core Web API** dialog, select the latest .NET Core 2.x **Target Framework**.</span></span> <span data-ttu-id="e4e77-740">選取 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-740">Select **Next**.</span></span>

* <span data-ttu-id="e4e77-741">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-741">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api-21"></a><span data-ttu-id="e4e77-743">測試 API 2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-743">Test the API 2.1</span></span>

<span data-ttu-id="e4e77-744">專案範本會建立 `values` API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-744">The project template creates a `values` API.</span></span> <span data-ttu-id="e4e77-745">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-745">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-746">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-746">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e4e77-747">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-747">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e4e77-748">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="e4e77-748">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e4e77-749">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-749">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e4e77-750">在接著出現的 [安全性警告] 對話方塊中，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-750">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-751">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-751">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e4e77-752">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-752">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="e4e77-753">在瀏覽器中，前往下列 URL：`https://localhost:5001/api/values`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-753">In a browser, go to following URL: `https://localhost:5001/api/values`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-754">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-754">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e4e77-755">選取 [**執行**  >  **開始調試** 程式] 以啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-755">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e4e77-756">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-756">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e4e77-757">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-757">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e4e77-758">將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-758">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="e4e77-759">隨即傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="e4e77-759">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class-21"></a><span data-ttu-id="e4e77-760">新增模型類別2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-760">Add a model class 2.1</span></span>

<span data-ttu-id="e4e77-761">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-761">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e4e77-762">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-762">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-763">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-763">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-764">在 **方案總管** 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-764">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e4e77-765">選取 **[**  >  **新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-765">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4e77-766">為資料夾命名 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-766">Name the folder *Models*.</span></span>

* <span data-ttu-id="e4e77-767">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [**加入**  >  **類別**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-767">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e4e77-768">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-768">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="e4e77-769">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-769">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e4e77-770">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e4e77-770">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="e4e77-771">新增名為的資料夾 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-771">Add a folder named *Models*.</span></span>

* <span data-ttu-id="e4e77-772">`TodoItem`使用下列程式碼，將類別新增至 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e4e77-772">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-773">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-773">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="e4e77-774">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-774">Right-click the project.</span></span> <span data-ttu-id="e4e77-775">選取 **[**  >  **新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-775">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e4e77-776">為資料夾命名 *Models* 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-776">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="e4e77-778">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 **Add** > [**新增** 檔案 > **一般**] > **空白類別**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-778">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="e4e77-779">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-779">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="e4e77-780">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-780">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e4e77-781">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e4e77-781">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e4e77-782">模型類別可移至專案中的任何位置，但依照慣例，會 *Models* 使用資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-782">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context-21"></a><span data-ttu-id="e4e77-783">新增資料庫內容2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-783">Add a database context 2.1</span></span>

<span data-ttu-id="e4e77-784">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-784">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e4e77-785">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="e4e77-785">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-786">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-786">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-787">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [**加入**  >  **類別**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-787">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e4e77-788">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-788">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-789">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-789">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e4e77-790">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-790">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e4e77-791">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-791">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context-21"></a><span data-ttu-id="e4e77-792">註冊資料庫內容2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-792">Register the database context 2.1</span></span>

<span data-ttu-id="e4e77-793">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e4e77-793">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e4e77-794">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-794">The container provides the service to controllers.</span></span>

<span data-ttu-id="e4e77-795">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="e4e77-795">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="e4e77-796">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-796">The preceding code:</span></span>

* <span data-ttu-id="e4e77-797">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="e4e77-797">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e4e77-798">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="e4e77-798">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e4e77-799">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-799">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller-21"></a><span data-ttu-id="e4e77-800">新增控制器2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-800">Add a controller 2.1</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-801">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-801">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-802">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-802">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="e4e77-803">選取 [ **加入** > **新專案**]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-803">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="e4e77-804">在 [新增項目] 對話方塊中，選取 [API 控制器類別] 範本。</span><span class="sxs-lookup"><span data-stu-id="e4e77-804">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="e4e77-805">將類別命名為 *TodoController*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-805">Name the class *TodoController*, and select **Add**.</span></span>

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-807">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-807">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e4e77-808">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-808">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="e4e77-809">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-809">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="e4e77-810">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e4e77-810">The preceding code:</span></span>

* <span data-ttu-id="e4e77-811">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="e4e77-811">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e4e77-812">標記具有屬性的類別 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-812">Marks the class with the [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e4e77-813">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-813">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e4e77-814">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="e4e77-814">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e4e77-815">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-815">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e4e77-816">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e4e77-816">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="e4e77-817">如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-817">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="e4e77-818">此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-818">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="e4e77-819">如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-819">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="e4e77-820">因此看起來雖然像是刪除失敗，但實際為成功。</span><span class="sxs-lookup"><span data-stu-id="e4e77-820">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods-21"></a><span data-ttu-id="e4e77-821">新增 Get 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-821">Add Get methods 2.1</span></span>

<span data-ttu-id="e4e77-822">若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-822">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="e4e77-823">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="e4e77-823">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="e4e77-824">如果應用程式仍在執行，請將其停止。</span><span class="sxs-lookup"><span data-stu-id="e4e77-824">Stop the app if it's still running.</span></span> <span data-ttu-id="e4e77-825">然後重新予以執行以包含最新的變更。</span><span class="sxs-lookup"><span data-stu-id="e4e77-825">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="e4e77-826">從瀏覽器呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-826">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="e4e77-827">例如：</span><span class="sxs-lookup"><span data-stu-id="e4e77-827">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="e4e77-828">以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="e4e77-828">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths-21"></a><span data-ttu-id="e4e77-829">路由和 URL 路徑2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-829">Routing and URL paths 2.1</span></span>

<span data-ttu-id="e4e77-830">[`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="e4e77-830">The [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e4e77-831">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="e4e77-831">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e4e77-832">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="e4e77-832">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="e4e77-833">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-833">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e4e77-834">在此範例中，控制器類別名稱是 **Todo** Controller，因此容器名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="e4e77-834">For this sample, the controller class name is **Todo** Controller, so the controller name is "todo".</span></span> <span data-ttu-id="e4e77-835">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-835">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e4e77-836">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="e4e77-836">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e4e77-837">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="e4e77-837">This sample doesn't use a template.</span></span> <span data-ttu-id="e4e77-838">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-838">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e4e77-839">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="e4e77-839">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e4e77-840">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="e4e77-840">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values-21"></a><span data-ttu-id="e4e77-841">傳回值2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-841">Return values 2.1</span></span>

<span data-ttu-id="e4e77-842">和方法的傳回型 `GetTodoItems` 別 `GetTodoItem` 為 [ActionResult \<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-842">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e4e77-843">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="e4e77-843">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e4e77-844">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e4e77-844">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e4e77-845">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-845">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e4e77-846">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-846">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e4e77-847">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="e4e77-847">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e4e77-848">如果沒有任何專案符合要求的識別碼，方法會傳回 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-848">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound%2A> error code.</span></span>
* <span data-ttu-id="e4e77-849">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="e4e77-849">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e4e77-850">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="e4e77-850">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method-21"></a><span data-ttu-id="e4e77-851">測試 GetTodoItems 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-851">Test the GetTodoItems method 2.1</span></span>

<span data-ttu-id="e4e77-852">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-852">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="e4e77-853">安裝 [Postman](https://www.getpostman.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-853">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="e4e77-854">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-854">Start the web app.</span></span>
* <span data-ttu-id="e4e77-855">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="e4e77-855">Start Postman.</span></span>
* <span data-ttu-id="e4e77-856">停用 **SSL 憑證驗證**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-856">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e4e77-857">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e4e77-857">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="e4e77-858">從 [檔案]**[設定]** >  ([一般] 索引標籤)，停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-858">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="e4e77-859">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e4e77-859">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e4e77-860">從 **Postman**  >  **喜好** 設定 (**一般**] 索引標籤) ，停用 **SSL 憑證驗證**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-860">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="e4e77-861">或者，選取扳手並選取 [設定]，然後停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="e4e77-861">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="e4e77-862">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-862">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="e4e77-863">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-863">Create a new request.</span></span>
  * <span data-ttu-id="e4e77-864">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-864">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="e4e77-865">將要求 URI 設定為 `https://localhost:<port>/api/todo` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-865">Set the request URI to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="e4e77-866">例如： `https://localhost:5001/api/todo` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-866">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="e4e77-867">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-867">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="e4e77-868">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-868">Select **Send**.</span></span>

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method-21"></a><span data-ttu-id="e4e77-870">新增 Create 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-870">Add a Create method 2.1</span></span>

<span data-ttu-id="e4e77-871">在 *Controllers/TodoController.cs* 內部新增下列 `PostTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-871">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="e4e77-872">上述程式碼是 HTTP POST 方法，如屬性所指示 [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-872">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e4e77-873">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="e4e77-873">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e4e77-874">`CreatedAtAction` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-874">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="e4e77-875">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-875">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="e4e77-876">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="e4e77-876">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e4e77-877">將 `Location` 標頭加到回應中。</span><span class="sxs-lookup"><span data-stu-id="e4e77-877">Adds a `Location` header to the response.</span></span> <span data-ttu-id="e4e77-878">`Location` 標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="e4e77-878">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="e4e77-879">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-879">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e4e77-880">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="e4e77-880">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e4e77-881">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="e4e77-881">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method-21"></a><span data-ttu-id="e4e77-882">測試 PostTodoItem 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-882">Test the PostTodoItem method 2.1</span></span>

* <span data-ttu-id="e4e77-883">建置專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-883">Build the project.</span></span>
* <span data-ttu-id="e4e77-884">在 Postman 中，將 HTTP 方法設定為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-884">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="e4e77-885">將 URI 設定為 `https://localhost:<port>/api/Todo` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-885">Set the URI to `https://localhost:<port>/api/Todo`.</span></span> <span data-ttu-id="e4e77-886">例如： `https://localhost:5001/api/Todo` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-886">For example, `https://localhost:5001/api/Todo`.</span></span>
* <span data-ttu-id="e4e77-887">選取 [Body] \(本文\) 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-887">Select the **Body** tab.</span></span>
* <span data-ttu-id="e4e77-888">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="e4e77-888">Select the **raw** radio button.</span></span>
* <span data-ttu-id="e4e77-889">將類型設定為 **JSON (application/json)**。</span><span class="sxs-lookup"><span data-stu-id="e4e77-889">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="e4e77-890">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="e4e77-890">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="e4e77-891">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-891">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

  <span data-ttu-id="e4e77-893">如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。</span><span class="sxs-lookup"><span data-stu-id="e4e77-893">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri-21"></a><span data-ttu-id="e4e77-894">測試位置標頭 URI 2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-894">Test the location header URI 2.1</span></span>

* <span data-ttu-id="e4e77-895">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e4e77-895">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="e4e77-896">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="e4e77-896">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* <span data-ttu-id="e4e77-898">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="e4e77-898">Set the method to GET.</span></span>
* <span data-ttu-id="e4e77-899">將 URI 設定為 `https://localhost:<port>/api/TodoItems/2` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-899">Set the URI to `https://localhost:<port>/api/TodoItems/2`.</span></span> <span data-ttu-id="e4e77-900">例如： `https://localhost:5001/api/TodoItems/2` 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-900">For example, `https://localhost:5001/api/TodoItems/2`.</span></span>
* <span data-ttu-id="e4e77-901">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-901">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method-21"></a><span data-ttu-id="e4e77-902">新增 PutTodoItem 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-902">Add a PutTodoItem method 2.1</span></span>

<span data-ttu-id="e4e77-903">請新增下列 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-903">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="e4e77-904">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="e4e77-904">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e4e77-905">回應為 [204 (沒有內容) ](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-905">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e4e77-906">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="e4e77-906">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e4e77-907">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-907">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e4e77-908">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="e4e77-908">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method-21"></a><span data-ttu-id="e4e77-909">測試 PutTodoItem 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-909">Test the PutTodoItem method 2.1</span></span>

<span data-ttu-id="e4e77-910">這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-910">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e4e77-911">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e4e77-911">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e4e77-912">在進行 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有專案。</span><span class="sxs-lookup"><span data-stu-id="e4e77-912">Call GET to ensure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e4e77-913">更新識別碼為1的待辦事項，並將其名稱設定為「摘要魚」：</span><span class="sxs-lookup"><span data-stu-id="e4e77-913">Update the to-do item that has Id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "id":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="e4e77-914">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="e4e77-914">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method-21"></a><span data-ttu-id="e4e77-916">新增 DeleteTodoItem 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-916">Add a DeleteTodoItem method 2.1</span></span>

<span data-ttu-id="e4e77-917">請新增下列 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4e77-917">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="e4e77-918">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-918">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method-21"></a><span data-ttu-id="e4e77-919">測試 DeleteTodoItem 方法2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-919">Test the DeleteTodoItem method 2.1</span></span>

<span data-ttu-id="e4e77-920">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="e4e77-920">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="e4e77-921">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-921">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="e4e77-922">設定要刪除之物件的 URI (例如 `https://localhost:5001/api/todo/1`) 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-922">Set the URI of the object to delete (for example, `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="e4e77-923">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="e4e77-923">Select **Send**.</span></span>

<span data-ttu-id="e4e77-924">範例應用程式可讓您刪除所有項目。</span><span class="sxs-lookup"><span data-stu-id="e4e77-924">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="e4e77-925">但刪除最後一個項目之後，模型類別建構函式會在下次呼叫 API 時建立新的項目。</span><span class="sxs-lookup"><span data-stu-id="e4e77-925">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript-21"></a><span data-ttu-id="e4e77-926">使用 JavaScript 2.1 呼叫 web API</span><span class="sxs-lookup"><span data-stu-id="e4e77-926">Call the web API with JavaScript 2.1</span></span>

<span data-ttu-id="e4e77-927">在此節中，將會新增 HTML 網頁，以使用 JavaScript 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="e4e77-927">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="e4e77-928">jQuery 會起始要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-928">jQuery initiates the request.</span></span> <span data-ttu-id="e4e77-929">JavaScript 會使用來自 Web API 回應的詳細資料來更新頁面。</span><span class="sxs-lookup"><span data-stu-id="e4e77-929">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="e4e77-930">藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式 [提供靜態檔案](xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A)並 [啟用預設檔案對應](xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles%2A)：</span><span class="sxs-lookup"><span data-stu-id="e4e77-930">Configure the app to [serve static files](xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles%2A) and [enable default file mapping](xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles%2A) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="e4e77-931">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e4e77-931">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="e4e77-932">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="e4e77-932">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="e4e77-933">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="e4e77-933">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="e4e77-934">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="e4e77-934">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="e4e77-935">以下列程式碼取代其內容：</span><span class="sxs-lookup"><span data-stu-id="e4e77-935">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="e4e77-936">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="e4e77-936">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="e4e77-937">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="e4e77-937">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="e4e77-938">移除 `launchUrl` 屬性，以強制在專案的預設檔案 *index.html* 開啟應用程式 &mdash; 。</span><span class="sxs-lookup"><span data-stu-id="e4e77-938">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="e4e77-939">此範例會呼叫 Web API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="e4e77-939">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="e4e77-940">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="e4e77-940">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items-21"></a><span data-ttu-id="e4e77-941">取得待辦事項的清單2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-941">Get a list of to-do items 2.1</span></span>

<span data-ttu-id="e4e77-942">jQuery 會將 HTTP GET 要求傳送至 web API，該 API 會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="e4e77-942">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="e4e77-943">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="e4e77-943">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="e4e77-944">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="e4e77-944">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item-21"></a><span data-ttu-id="e4e77-945">新增待辦事項2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-945">Add a to-do item 2.1</span></span>

<span data-ttu-id="e4e77-946">jQuery 會使用要求主體中的待辦事項來傳送 HTTP POST 要求。</span><span class="sxs-lookup"><span data-stu-id="e4e77-946">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="e4e77-947">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="e4e77-947">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="e4e77-948">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="e4e77-948">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="e4e77-949">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="e4e77-949">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item-21"></a><span data-ttu-id="e4e77-950">更新待辦事項2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-950">Update a to-do item 2.1</span></span>

<span data-ttu-id="e4e77-951">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="e4e77-951">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="e4e77-952">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="e4e77-952">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item-21"></a><span data-ttu-id="e4e77-953">刪除待辦事項2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-953">Delete a to-do item 2.1</span></span>

<span data-ttu-id="e4e77-954">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="e4e77-954">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api-21"></a><span data-ttu-id="e4e77-955">將驗證支援新增至 web API 2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-955">Add authentication support to a web API 2.1</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources-21"></a><span data-ttu-id="e4e77-956">其他資源2。1</span><span class="sxs-lookup"><span data-stu-id="e4e77-956">Additional resources 2.1</span></span>

<span data-ttu-id="e4e77-957">[檢視或下載本教學課程的範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-957">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="e4e77-958">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="e4e77-958">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="e4e77-959">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="e4e77-959">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="e4e77-960">本教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="e4e77-960">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
