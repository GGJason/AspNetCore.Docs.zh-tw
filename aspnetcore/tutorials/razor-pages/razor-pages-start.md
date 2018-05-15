---
title: 開始使用 ASP.NET Core 中的 Razor Pages
author: rick-anderson
description: 了解建置 ASP.NET Core Razor 頁面之 Web 應用程式的基本概念。 對於 ASP.NET Core 中的 Web 工作負載，建議使用 Razor 頁面。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 12/22/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/razor-pages-start
ms.openlocfilehash: caf4376c0a02931eeec85e5067a082b37ef9da68
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="get-started-with-razor-pages-in-aspnet-core"></a><span data-ttu-id="73919-104">開始使用 ASP.NET Core 中的 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="73919-104">Get started with Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="73919-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="73919-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="73919-106">本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="73919-106">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="73919-107">Razor Pages 是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="73919-107">Razor Pages is the recommended way to build UI for web apps in ASP.NET Core.</span></span>

<span data-ttu-id="73919-108">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="73919-108">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="73919-109">Windows：本教學課程</span><span class="sxs-lookup"><span data-stu-id="73919-109">Windows: This tutorial</span></span>
* <span data-ttu-id="73919-110">macOS：[Razor 頁面與 Visual Studio for Mac 使用者入門](xref:tutorials/razor-pages-mac/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="73919-110">MacOS: [Get started with Razor Pages with Visual Studio for Mac](xref:tutorials/razor-pages-mac/razor-pages-start)</span></span>
* <span data-ttu-id="73919-111">macOS、Linux 和 Windows：[Visual Studio Code 中的 ASP.NET Core Razor 頁面使用者入門](xref:tutorials/razor-pages-vsc/razor-pages-start)</span><span class="sxs-lookup"><span data-stu-id="73919-111">macOS, Linux, and Windows: [Get started with ASP.NET Core Razor Pages in Visual Studio Code](xref:tutorials/razor-pages-vsc/razor-pages-start)</span></span>

<span data-ttu-id="73919-112">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="73919-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="73919-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="73919-113">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="73919-114">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="73919-114">Create a Razor web app</span></span>

* <span data-ttu-id="73919-115">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="73919-115">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="73919-116">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="73919-116">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="73919-117">將專案命名為 **RazorPagesMovie**。</span><span class="sxs-lookup"><span data-stu-id="73919-117">Name the project **RazorPagesMovie**.</span></span> <span data-ttu-id="73919-118">請務必將專案命名為 *RazorPagesMovie*，因此當您複製/貼上程式碼時，名稱空間會相符。</span><span class="sxs-lookup"><span data-stu-id="73919-118">It's important to name the project *RazorPagesMovie* so the namespaces will match when you copy/paste code.</span></span>
  <span data-ttu-id="73919-119">![新增 ASP.NET Core Web 應用程式](../../mvc/razor-pages/index/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="73919-119">![new ASP.NET Core Web Application](../../mvc/razor-pages/index/_static/np.png)</span></span>
* <span data-ttu-id="73919-120">在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="73919-120">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>

  [!INCLUDE [install 2.0](../../includes/dotnetcore-on-dotnetfx-vs.md)]

<span data-ttu-id="73919-121">Visual Studio 範本會建立入門專案：</span><span class="sxs-lookup"><span data-stu-id="73919-121">The Visual Studio template creates a starter project:</span></span>

![底下提供說明，包括方案總管](razor-pages-start/_static/se.png)

<span data-ttu-id="73919-123">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="73919-123">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

![Home 或 Index 頁面](razor-pages-start/_static/home.png)

* <span data-ttu-id="73919-125">Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="73919-125">Visual Studio starts [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) and runs your app.</span></span> <span data-ttu-id="73919-126">位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。</span><span class="sxs-lookup"><span data-stu-id="73919-126">The address bar shows `localhost:port#` and not something like `example.com`.</span></span> <span data-ttu-id="73919-127">這是因為 `localhost` 是本機電腦的標準主機名稱。</span><span class="sxs-lookup"><span data-stu-id="73919-127">That's because `localhost` is the standard hostname for your local computer.</span></span> <span data-ttu-id="73919-128">Localhost 只會為來自本機電腦的 Web 要求提供服務。</span><span class="sxs-lookup"><span data-stu-id="73919-128">Localhost only serves web requests from the local computer.</span></span> <span data-ttu-id="73919-129">當 Visual Studio 建立 Web 專案時，會對網頁伺服器使用隨機連接埠。</span><span class="sxs-lookup"><span data-stu-id="73919-129">When Visual Studio creates a web project, a random port is used for the web server.</span></span> <span data-ttu-id="73919-130">在上述影像中，連接埠編號為 5000。</span><span class="sxs-lookup"><span data-stu-id="73919-130">In the preceding image, the port number is 5000.</span></span> <span data-ttu-id="73919-131">當您執行應用程式時，會看到不同的連接埠編號。</span><span class="sxs-lookup"><span data-stu-id="73919-131">When you run the app, you'll see a different port number.</span></span>
* <span data-ttu-id="73919-132">使用 **Ctrl + F5** (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="73919-132">Launching the app with **Ctrl+F5** (non-debug mode) allows you to make code changes, save the file, refresh the browser, and see the code changes.</span></span> <span data-ttu-id="73919-133">許多開發人員想要使用非偵錯模式，以便快速啟動應用程式並檢視變更。</span><span class="sxs-lookup"><span data-stu-id="73919-133">Many developers prefer to use non-debug mode to quickly launch the app and view changes.</span></span>

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

> [!div class="step-by-step"]
> [<span data-ttu-id="73919-134">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="73919-134">Next: Adding a model</span></span>](xref:tutorials/razor-pages/model)
