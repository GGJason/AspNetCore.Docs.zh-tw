---
title: 教學課程：開始使用 Razor ASP.NET Core 中的頁面
author: rick-anderson
description: 這一系列的教學課程會示範如何使用 Razor ASP.NET Core 中的頁面。 瞭解如何建立模型、產生頁面的程式碼 Razor 、使用 Entity Framework Core 和 SQL Server 進行資料存取、新增搜尋功能、新增輸入驗證，以及使用遷移來更新模型。
ms.author: riande
ms.date: 11/12/2019
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
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: 5fb841de2fa9a04cf05aaf08f255041ee1952638
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88021090"
---
# <a name="tutorial-get-started-with-no-locrazor-pages-in-aspnet-core"></a><span data-ttu-id="b1811-104">教學課程：開始使用 Razor ASP.NET Core 中的頁面</span><span class="sxs-lookup"><span data-stu-id="b1811-104">Tutorial: Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="b1811-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b1811-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="b1811-106">這是一系列的第一個教學課程，會教您建立 ASP.NET Core Razor 頁面 web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="b1811-106">This is the first tutorial of a series that teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="b1811-107">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-107">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="b1811-108">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="b1811-108">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b1811-109">建立 Razor 頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-109">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="b1811-110">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-110">Run the app.</span></span>
> * <span data-ttu-id="b1811-111">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="b1811-111">Examine the project files.</span></span>

<span data-ttu-id="b1811-112">在本教學課程結束時，您將會有一個 Razor 可在稍後的教學課程中建立的工作頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-112">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="b1811-114">必要條件</span><span class="sxs-lookup"><span data-stu-id="b1811-114">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b1811-115">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1811-115">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="b1811-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1811-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b1811-117">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1811-117">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-no-locrazor-pages-web-app"></a><span data-ttu-id="b1811-118">建立 Razor 頁面 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b1811-118">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b1811-119">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1811-119">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1811-120">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="b1811-120">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="b1811-121">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-121">Create a new ASP.NET Core Web Application and select **Next**.</span></span>
  <span data-ttu-id="b1811-122">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)</span><span class="sxs-lookup"><span data-stu-id="b1811-122">![new ASP.NET Core Web Application](razor-pages-start/_static/np_2.1.png)</span></span>
* <span data-ttu-id="b1811-123">將專案命名為\*\* Razor PagesMovie\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-123">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="b1811-124">請務必將專案命名為\* Razor PagesMovie\* ，如此一來，當您複製並貼上程式碼時，命名空間將會相符。</span><span class="sxs-lookup"><span data-stu-id="b1811-124">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>
  <span data-ttu-id="b1811-125">![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)</span><span class="sxs-lookup"><span data-stu-id="b1811-125">![new ASP.NET Core Web Application](razor-pages-start/_static/config.png)</span></span>

* <span data-ttu-id="b1811-126">在下拉式清單 [ **Web 應用程式**] 中選取 [ **ASP.NET Core 3.1** ]，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="b1811-126">Select **ASP.NET Core 3.1** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/3/npx.png)

  <span data-ttu-id="b1811-128">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="b1811-128">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="b1811-130">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1811-130">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1811-131">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="b1811-131">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="b1811-132">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="b1811-132">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="b1811-133">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b1811-133">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="b1811-134">`dotnet new`命令會 Razor 在\* Razor PagesMovie\*資料夾中建立新的頁面專案。</span><span class="sxs-lookup"><span data-stu-id="b1811-134">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="b1811-135">`code`命令會在 Visual Studio Code 的目前實例中開啟\* Razor PagesMovie\*資料夾。</span><span class="sxs-lookup"><span data-stu-id="b1811-135">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="b1811-136">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' PagesMovie ' 中遺漏了 debug Razor 。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="b1811-136">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="b1811-137">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="b1811-137">Select **Yes**.</span></span>

  <span data-ttu-id="b1811-138">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="b1811-138">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b1811-139">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1811-139">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b1811-140">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-140">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="b1811-142">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="b1811-142">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="b1811-143">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="b1811-143">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

  ![macOS web 應用程式範本選取專案](razor-pages-start/_static/web_app_template_vsmac.png)

* <span data-ttu-id="b1811-145">在 [**設定您的新 Web 應用程式**] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="b1811-145">In the **Configure your new Web Application** dialog:</span></span>

  * <span data-ttu-id="b1811-146">確認 [**驗證**] 已設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="b1811-146">Confirm that **Authentication** is set to **No Authentication**.</span></span>
  * <span data-ttu-id="b1811-147">如果提供選取**目標 Framework**的選項，請選取最新的3.x 版。</span><span class="sxs-lookup"><span data-stu-id="b1811-147">If presented an option to select a **Target Framework**, select the latest 3.x version.</span></span>

  <span data-ttu-id="b1811-148">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="b1811-148">Select **Next**.</span></span>

* <span data-ttu-id="b1811-149">將專案命名為\*\* Razor PagesMovie**，然後選取 [**建立\*\*]。</span><span class="sxs-lookup"><span data-stu-id="b1811-149">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![macOS 將專案命名為](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="b1811-151">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b1811-151">Run the app</span></span>

  [!INCLUDE[](~/includes/run-the-app.md)]

## <a name="examine-the-project-files"></a><span data-ttu-id="b1811-152">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="b1811-152">Examine the project files</span></span>

<span data-ttu-id="b1811-153">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="b1811-153">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="b1811-154">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="b1811-154">Pages folder</span></span>

<span data-ttu-id="b1811-155">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="b1811-155">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="b1811-156">每個 Razor 頁面都是一對檔案：</span><span class="sxs-lookup"><span data-stu-id="b1811-156">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="b1811-157">包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="b1811-157">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="b1811-158">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="b1811-158">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="b1811-159">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="b1811-159">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="b1811-160">例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="b1811-160">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="b1811-161">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="b1811-161">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="b1811-162">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="b1811-162">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="b1811-163">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="b1811-163">wwwroot folder</span></span>

<span data-ttu-id="b1811-164">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="b1811-164">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="b1811-165">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="b1811-165">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="b1811-166">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="b1811-166">appSettings.json</span></span>

<span data-ttu-id="b1811-167">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="b1811-167">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="b1811-168">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="b1811-168">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="b1811-169">Program.cs</span><span class="sxs-lookup"><span data-stu-id="b1811-169">Program.cs</span></span>

<span data-ttu-id="b1811-170">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="b1811-170">Contains the entry point for the program.</span></span> <span data-ttu-id="b1811-171">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="b1811-171">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="b1811-172">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="b1811-172">Startup.cs</span></span>

<span data-ttu-id="b1811-173">包含設定應用程式行為的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b1811-173">Contains code that configures app behavior.</span></span> <span data-ttu-id="b1811-174">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="b1811-174">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b1811-175">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b1811-175">Next steps</span></span>

<span data-ttu-id="b1811-176">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="b1811-176">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b1811-177">新增模型</span><span class="sxs-lookup"><span data-stu-id="b1811-177">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end

<!--::: moniker range=">= aspnetcore-3.0" -->

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1811-178">這是本系列的第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="b1811-178">This is the first tutorial of a series.</span></span> <span data-ttu-id="b1811-179">[本系列將](xref:tutorials/razor-pages/index)教您建立 ASP.NET Core Razor Pages web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="b1811-179">[The series](xref:tutorials/razor-pages/index) teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span>

[!INCLUDE[](~/includes/advancedRP.md)]

<span data-ttu-id="b1811-180">在本系列結束時，您將會有一個可管理電影資料庫的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-180">At the end of the series, you'll have an app that manages a database of movies.</span></span>  

[!INCLUDE[View or download sample code](~/includes/rp/download.md)]

<span data-ttu-id="b1811-181">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="b1811-181">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b1811-182">建立 Razor 頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-182">Create a Razor Pages web app.</span></span>
> * <span data-ttu-id="b1811-183">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-183">Run the app.</span></span>
> * <span data-ttu-id="b1811-184">檢查專案檔。</span><span class="sxs-lookup"><span data-stu-id="b1811-184">Examine the project files.</span></span>

<span data-ttu-id="b1811-185">在本教學課程結束時，您將會有一個 Razor 可在稍後的教學課程中建立的工作頁面 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-185">At the end of this tutorial, you'll have a working Razor Pages web app that you'll build on in later tutorials.</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)

## <a name="prerequisites"></a><span data-ttu-id="b1811-187">必要條件</span><span class="sxs-lookup"><span data-stu-id="b1811-187">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b1811-188">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1811-188">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="b1811-189">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1811-189">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b1811-190">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1811-190">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-no-locrazor-pages-web-app"></a><span data-ttu-id="b1811-191">建立 Razor 頁面 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="b1811-191">Create a Razor Pages web app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b1811-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1811-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1811-193">從 Visual Studio 的 [檔案]\*\*\*\* 功能表中，選取 [新增]**[專案]** > \*\*\*\* 。</span><span class="sxs-lookup"><span data-stu-id="b1811-193">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>

* <span data-ttu-id="b1811-194">建立新的 ASP.NET Core Web 應用程式並選取 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-194">Create a new ASP.NET Core Web Application and select **Next**.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2.1.png)

* <span data-ttu-id="b1811-196">將專案命名為\*\* Razor PagesMovie\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-196">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="b1811-197">請務必將專案命名為\* Razor PagesMovie\* ，如此一來，當您複製並貼上程式碼時，命名空間將會相符。</span><span class="sxs-lookup"><span data-stu-id="b1811-197">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy and paste code.</span></span>

  ![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/config.png)

* <span data-ttu-id="b1811-199">在下拉式清單中選取 [ASP.NET Core 2.2]\*\*\*\*，然後選取 [Web 應用程式]\*\*\*\* 及 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-199">Select **ASP.NET Core 2.2** in the dropdown, **Web Application**, and then select **Create**.</span></span>

![新增 ASP.NET Core Web 應用程式](razor-pages-start/_static/np_2_2.2.png)

  <span data-ttu-id="b1811-201">下列起始專案會隨即建立：</span><span class="sxs-lookup"><span data-stu-id="b1811-201">The following starter project is created:</span></span>

  ![方案總管](razor-pages-start/_static/se2.2.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="b1811-203">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1811-203">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="b1811-204">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="b1811-204">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>

* <span data-ttu-id="b1811-205">變更為將包含專案的目錄 (`cd`)。</span><span class="sxs-lookup"><span data-stu-id="b1811-205">Change to the directory (`cd`) which will contain the project.</span></span>

* <span data-ttu-id="b1811-206">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="b1811-206">Run the following commands:</span></span>

  ```dotnetcli
  dotnet new webapp -o RazorPagesMovie
  code -r RazorPagesMovie
  ```

  * <span data-ttu-id="b1811-207">`dotnet new`命令會 Razor 在\* Razor PagesMovie\*資料夾中建立新的頁面專案。</span><span class="sxs-lookup"><span data-stu-id="b1811-207">The `dotnet new` command creates a new Razor Pages project in the *RazorPagesMovie* folder.</span></span>
  * <span data-ttu-id="b1811-208">`code`命令會在 Visual Studio Code 的目前實例中開啟\* Razor PagesMovie\*資料夾。</span><span class="sxs-lookup"><span data-stu-id="b1811-208">The `code` command opens the *RazorPagesMovie* folder in the current instance of Visual Studio Code.</span></span>

* <span data-ttu-id="b1811-209">狀態列的 OmniSharp 火焰圖示變為綠色後，會有一個對話方塊要求**所需的資產建立，而且 ' PagesMovie ' 中遺漏了 debug Razor 。要新增它們嗎？**</span><span class="sxs-lookup"><span data-stu-id="b1811-209">After the status bar's OmniSharp flame icon turns green, a dialog asks **Required assets to build and debug are missing from 'RazorPagesMovie'. Add them?**</span></span> <span data-ttu-id="b1811-210">選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="b1811-210">Select **Yes**.</span></span>

  <span data-ttu-id="b1811-211">*.vscode* 目錄 (其中包含 *launch.json* 和 *tasks.json* 檔案) 會被新增至專案的根目錄。</span><span class="sxs-lookup"><span data-stu-id="b1811-211">A *.vscode* directory, containing *launch.json* and *tasks.json* files, is added to the project's root directory.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b1811-212">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1811-212">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="b1811-213">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b1811-213">Select **File** > **New Solution**.</span></span>

![macOS 新增方案](../first-mvc-app/start-mvc/_static/new_project_vsmac.png)

* <span data-ttu-id="b1811-215">在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="b1811-215">In Visual Studio for Mac earlier than version 8.6, select **.NET Core** > **App** > **Web Application** > **Next**.</span></span> <span data-ttu-id="b1811-216">在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式**  >  **] [下一步]**。</span><span class="sxs-lookup"><span data-stu-id="b1811-216">In version 8.6 or later, select **Web and Console** > **App** > **Web Application** > **Next**.</span></span>

* <span data-ttu-id="b1811-217">在 [**設定您的新 Web 應用程式**] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="b1811-217">In the **Configure your new Web Application** dialog:</span></span>

  * <span data-ttu-id="b1811-218">確認 [**驗證**] 已設為 [**無驗證**]。</span><span class="sxs-lookup"><span data-stu-id="b1811-218">Confirm that **Authentication** is set to **No Authentication**.</span></span>
  * <span data-ttu-id="b1811-219">如果提供選取**目標 Framework**的選項，請選取最新的2.x 版。</span><span class="sxs-lookup"><span data-stu-id="b1811-219">If presented an option to select a **Target Framework**, select the latest 2.x version.</span></span>

  <span data-ttu-id="b1811-220">選取 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="b1811-220">Select **Next**.</span></span>

* <span data-ttu-id="b1811-221">將專案命名為\*\* Razor PagesMovie**，然後選取 [**建立\*\*]。</span><span class="sxs-lookup"><span data-stu-id="b1811-221">Name the project **RazorPagesMovie**, and then select **Create**.</span></span>

  ![nameproj](razor-pages-start/_static/RazorPagesMovie.png)

<!-- End of VS tabs -->

---

## <a name="run-the-app"></a><span data-ttu-id="b1811-223">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="b1811-223">Run the app</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="b1811-224">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b1811-224">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="b1811-225">按 Ctrl+F5 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b1811-225">Press Ctrl+F5 to run without the debugger.</span></span>

  [!INCLUDE[](~/includes/trustCertVS.md)]

  <span data-ttu-id="b1811-226">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="b1811-226">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs the app.</span></span> <span data-ttu-id="b1811-227">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="b1811-227">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b1811-228">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="b1811-228">That's because `localhost` is the standard hostname for the local computer.</span></span> <span data-ttu-id="b1811-229">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="b1811-229">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="b1811-230">當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="b1811-230">When Visual Studio creates a web project, a random port is used for the web server.</span></span>

* <span data-ttu-id="b1811-231">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="b1811-231">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="b1811-232">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="b1811-232">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="b1811-234">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="b1811-234">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-code"></a>[<span data-ttu-id="b1811-236">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b1811-236">Visual Studio Code</span></span>](#tab/visual-studio-code)

  [!INCLUDE[](~/includes/trustCertVSC.md)]

* <span data-ttu-id="b1811-237">按 **Ctrl-F5** 即可執行而不使用偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="b1811-237">Press **Ctrl-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b1811-238">Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="b1811-238">Visual Studio Code starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span> <span data-ttu-id="b1811-239">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="b1811-239">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="b1811-240">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="b1811-240">That's because `localhost` is the standard hostname for  local computer.</span></span> <span data-ttu-id="b1811-241">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="b1811-241">Localhost only serves web requests from the local computer.</span></span>

* <span data-ttu-id="b1811-242">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="b1811-242">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="b1811-243">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="b1811-243">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2.png)

  <span data-ttu-id="b1811-245">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="b1811-245">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2.png)
  
# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="b1811-247">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="b1811-247">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

  [!INCLUDE[](~/includes/trustCertMac.md)]

* <span data-ttu-id="b1811-248">按 **Cmd-Opt-F5** 以在不使用偵錯工具的情況下執行。</span><span class="sxs-lookup"><span data-stu-id="b1811-248">Press **Cmd-Opt-F5** to run without the debugger.</span></span>

  <span data-ttu-id="b1811-249">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5001`。</span><span class="sxs-lookup"><span data-stu-id="b1811-249">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5001`.</span></span>

* <span data-ttu-id="b1811-250">在應用程式的首頁上，選取 [接受]\*\*\*\* 同意追蹤。</span><span class="sxs-lookup"><span data-stu-id="b1811-250">On the app's home page, select **Accept** to consent to tracking.</span></span>

  <span data-ttu-id="b1811-251">此應用程式不會追蹤個人資訊，但專案範本會包含同意功能，以防您需要同意才符合歐盟的[一般資料保護規定 (GDPR)](xref:security/gdpr)。</span><span class="sxs-lookup"><span data-stu-id="b1811-251">This app doesn't track personal information, but the project template includes the consent feature in case you need it to comply with the European Union's [General Data Protection Regulation (GDPR)](xref:security/gdpr).</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/homeGDPR2.2_safari.png)

  <span data-ttu-id="b1811-253">下圖顯示您同意追蹤之後的應用程式：</span><span class="sxs-lookup"><span data-stu-id="b1811-253">The following image shows the app after you give consent to tracking:</span></span>

  ![Home 或 Index 頁面](razor-pages-start/_static/home2.2_safari.png)

<!-- End of VS tabs -->

---

## <a name="examine-the-project-files"></a><span data-ttu-id="b1811-255">檢查專案檔</span><span class="sxs-lookup"><span data-stu-id="b1811-255">Examine the project files</span></span>

<span data-ttu-id="b1811-256">以下概要說明主要專案資料夾和檔案，您將在稍後的教學課程中使用這些資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="b1811-256">Here's an overview of the main project folders and files that you'll work with in later tutorials.</span></span>

### <a name="pages-folder"></a><span data-ttu-id="b1811-257">Pages 資料夾</span><span class="sxs-lookup"><span data-stu-id="b1811-257">Pages folder</span></span>

<span data-ttu-id="b1811-258">包含 Razor 頁面和支援檔案。</span><span class="sxs-lookup"><span data-stu-id="b1811-258">Contains Razor pages and supporting files.</span></span> <span data-ttu-id="b1811-259">每個 Razor 頁面都是一對檔案：</span><span class="sxs-lookup"><span data-stu-id="b1811-259">Each Razor page is a pair of files:</span></span>

* <span data-ttu-id="b1811-260">包含 HTML 標籤的*cshtml*檔案，其 c # 程式碼使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="b1811-260">A *.cshtml* file that contains HTML markup with C# code using Razor syntax.</span></span>
* <span data-ttu-id="b1811-261">*.cshtml.cs* 檔案，其中包含處理頁面事件的 C# 程式碼。</span><span class="sxs-lookup"><span data-stu-id="b1811-261">A *.cshtml.cs* file that contains C# code that handles page events.</span></span>

<span data-ttu-id="b1811-262">支援檔案的名稱以底線開頭。</span><span class="sxs-lookup"><span data-stu-id="b1811-262">Supporting files have names that begin with an underscore.</span></span> <span data-ttu-id="b1811-263">例如，_Layout 的 *. cshtml*檔案會設定所有頁面通用的 UI 元素。</span><span class="sxs-lookup"><span data-stu-id="b1811-263">For example, the *_Layout.cshtml* file configures UI elements common to all pages.</span></span> <span data-ttu-id="b1811-264">此檔案會設定頁面頂端的導覽功能表和頁面底部的著作權標示。</span><span class="sxs-lookup"><span data-stu-id="b1811-264">This file sets up the navigation menu at the top of the page and the copyright notice at the bottom of the page.</span></span> <span data-ttu-id="b1811-265">如需詳細資訊，請參閱<xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="b1811-265">For more information, see <xref:mvc/views/layout>.</span></span>

### <a name="wwwroot-folder"></a><span data-ttu-id="b1811-266">wwwroot 資料夾</span><span class="sxs-lookup"><span data-stu-id="b1811-266">wwwroot folder</span></span>

<span data-ttu-id="b1811-267">包含靜態檔案，例如 HTML 檔案、JavaScript 檔案和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="b1811-267">Contains static files, such as HTML files, JavaScript files, and CSS files.</span></span> <span data-ttu-id="b1811-268">如需詳細資訊，請參閱<xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="b1811-268">For more information, see <xref:fundamentals/static-files>.</span></span>

### <a name="appsettingsjson"></a><span data-ttu-id="b1811-269">appSettings.json</span><span class="sxs-lookup"><span data-stu-id="b1811-269">appSettings.json</span></span>

<span data-ttu-id="b1811-270">包含組態資料，例如連接字串。</span><span class="sxs-lookup"><span data-stu-id="b1811-270">Contains configuration data, such as connection strings.</span></span> <span data-ttu-id="b1811-271">如需詳細資訊，請參閱<xref:fundamentals/configuration/index>。</span><span class="sxs-lookup"><span data-stu-id="b1811-271">For more information, see <xref:fundamentals/configuration/index>.</span></span>

### <a name="programcs"></a><span data-ttu-id="b1811-272">Program.cs</span><span class="sxs-lookup"><span data-stu-id="b1811-272">Program.cs</span></span>

<span data-ttu-id="b1811-273">包含程式的進入點。</span><span class="sxs-lookup"><span data-stu-id="b1811-273">Contains the entry point for the program.</span></span> <span data-ttu-id="b1811-274">如需詳細資訊，請參閱<xref:fundamentals/host/generic-host>。</span><span class="sxs-lookup"><span data-stu-id="b1811-274">For more information, see <xref:fundamentals/host/generic-host>.</span></span>

### <a name="startupcs"></a><span data-ttu-id="b1811-275">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="b1811-275">Startup.cs</span></span>

<span data-ttu-id="b1811-276">包含設定應用程式行為的程式碼，例如它是否需要的同意 cookie 。</span><span class="sxs-lookup"><span data-stu-id="b1811-276">Contains code that configures app behavior, such as whether it requires consent for cookies.</span></span> <span data-ttu-id="b1811-277">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="b1811-277">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1811-278">其他資源</span><span class="sxs-lookup"><span data-stu-id="b1811-278">Additional resources</span></span>

* [<span data-ttu-id="b1811-279">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="b1811-279">Youtube version of this tutorial</span></span>](https://www.youtube.com/watch?v=F0SP7Ry4flQ&feature=youtu.be)

## <a name="next-steps"></a><span data-ttu-id="b1811-280">後續步驟</span><span class="sxs-lookup"><span data-stu-id="b1811-280">Next steps</span></span>

<span data-ttu-id="b1811-281">前進到系列中的下一個教學課程：</span><span class="sxs-lookup"><span data-stu-id="b1811-281">Advance to the next tutorial in the series:</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b1811-282">新增模型</span><span class="sxs-lookup"><span data-stu-id="b1811-282">Add a model</span></span>](xref:tutorials/razor-pages/model)

::: moniker-end
