---
title: 利用 Visual Studio for Mac 開始使用 ASP.NET Core on macOS 中的 Razor 頁面
author: rick-anderson
description: 了解如何利用 Visual Studio for Mac 開始使用 ASP.NET Core 中的 Razor 頁面。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/razor-pages-start
ms.openlocfilehash: 8da34f0a59976032747edcaf482f75c087ca8d8d
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34688266"
---
# <a name="get-started-with-razor-pages-in-aspnet-core-on-macos-with-visual-studio-for-mac"></a><span data-ttu-id="1106d-103">利用 Visual Studio for Mac 開始使用 ASP.NET Core on macOS 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="1106d-103">Get started with Razor Pages in ASP.NET Core on macOS with Visual Studio for Mac</span></span>

<span data-ttu-id="1106d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="1106d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="1106d-105">本教學課程將教導您建置 ASP.NET Core Razor Pages之 Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="1106d-105">This tutorial teaches the basics of building an ASP.NET Core Razor Pages web app.</span></span> <span data-ttu-id="1106d-106">建議您先檢閱 [Razor 頁面的簡介](xref:mvc/razor-pages/index)，再開始本教學課程。</span><span class="sxs-lookup"><span data-stu-id="1106d-106">We recommend you review [Introduction to Razor Pages](xref:mvc/razor-pages/index) before starting this tutorial.</span></span> <span data-ttu-id="1106d-107">Razor 頁面是在 ASP.NET Core 中建置 Web 應用程式 UI 的建議方式。</span><span class="sxs-lookup"><span data-stu-id="1106d-107">Razor Pages is the recommended way to build UI for web applications in ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1106d-108">必要條件</span><span class="sxs-lookup"><span data-stu-id="1106d-108">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs-macos.md)]

## <a name="create-a-razor-web-app"></a><span data-ttu-id="1106d-109">建立 Razor Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="1106d-109">Create a Razor web app</span></span>

<span data-ttu-id="1106d-110">從終端機中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="1106d-110">From a terminal, run the following commands:</span></span>

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new webapp -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new razor -o RazorPagesMovie
cd RazorPagesMovie
dotnet run
```

::: moniker-end

<span data-ttu-id="1106d-111">上述命令使用 [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) 來建立和執行 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="1106d-111">The preceding commands use the [.NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/dotnet) to create and run a Razor Pages project.</span></span> <span data-ttu-id="1106d-112">將瀏覽器開啟到 http://localhost:5000，以檢視應用程式。</span><span class="sxs-lookup"><span data-stu-id="1106d-112">Open a browser to http://localhost:5000 to view the application.</span></span>

![Home 或 Index 頁面](../razor-pages/razor-pages-start/_static/home.png)

[!INCLUDE [razor-pages-start](../../includes/RP/razor-pages-start.md)]

## <a name="open-the-project"></a><span data-ttu-id="1106d-114">開啟專案</span><span class="sxs-lookup"><span data-stu-id="1106d-114">Open the project</span></span>

<span data-ttu-id="1106d-115">按 Ctrl + C 來關閉應用程式。</span><span class="sxs-lookup"><span data-stu-id="1106d-115">Press Ctrl+C to shut down the application.</span></span>

<span data-ttu-id="1106d-116">從 Visual Studio 中，選取 [檔案] > [開啟]，然後選取 *RazorPagesMovie.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="1106d-116">From Visual Studio, select **File > Open**, and then select the *RazorPagesMovie.csproj* file.</span></span>

### <a name="launch-the-app"></a><span data-ttu-id="1106d-117">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="1106d-117">Launch the app</span></span>

<span data-ttu-id="1106d-118">在 Visual Studio 中，選取 [執行] > [啟動但不偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="1106d-118">In Visual Studio, select **Run > Start Without Debugging** to launch the app.</span></span> <span data-ttu-id="1106d-119">Visual Studio 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)啟動瀏覽器，然後巡覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="1106d-119">Visual Studio starts [Kestrel](xref:fundamentals/servers/kestrel), launches a browser, and navigates to `http://localhost:5000`.</span></span>

<span data-ttu-id="1106d-120">在下一個教學課程中，我們可以將模型新增至專案。</span><span class="sxs-lookup"><span data-stu-id="1106d-120">In the next tutorial, we add a model to the project.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1106d-121">下一步：新增模型</span><span class="sxs-lookup"><span data-stu-id="1106d-121">Next: Adding a model</span></span>](xref:tutorials/razor-pages-mac/model)
