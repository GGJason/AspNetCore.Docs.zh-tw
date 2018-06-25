---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: 加入的檢視 (C#) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: 50ce4a2024ffd9e2bbb5526717052d486689ff38
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "30873166"
---
<a name="adding-a-view-c"></a><span data-ttu-id="cacf8-103">加入的檢視 (C#)</span><span class="sxs-lookup"><span data-stu-id="cacf8-103">Adding a View (C#)</span></span>
====================
<span data-ttu-id="cacf8-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="cacf8-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="cacf8-105">本教學課程的更新的版本時使用[這裡](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="cacf8-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="cacf8-106">它更安全、 容易遵循，及示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="cacf8-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="cacf8-107">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="cacf8-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="cacf8-108">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="cacf8-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="cacf8-109">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="cacf8-110">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="cacf8-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="cacf8-111">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="cacf8-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="cacf8-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="cacf8-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="cacf8-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="cacf8-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="cacf8-114">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="cacf8-115">使用本主題隨附在 Visual Web Developer 專案中的使用 C# 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="cacf8-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="cacf8-116">[下載的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="cacf8-117">如果您偏好 Visual Basic，切換至[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="cacf8-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="cacf8-118">本節中您要修改`HelloWorldController`類別若要使用範本檔案來完全封裝產生 HTML 回應至用戶端的程序的檢視。</span><span class="sxs-lookup"><span data-stu-id="cacf8-118">In this section you're going to modify the `HelloWorldController` class to use view template files to cleanly encapsulate the process of generating HTML responses to a client.</span></span>

<span data-ttu-id="cacf8-119">您將建立使用新的檢視範本檔案[Razor 檢視引擎](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)所導入的 ASP.NET MVC 3。</span><span class="sxs-lookup"><span data-stu-id="cacf8-119">You'll create a view template file using the new [Razor view engine](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) introduced with ASP.NET MVC 3.</span></span> <span data-ttu-id="cacf8-120">Razor 為基礎的檢視範本具有 *.cshtml*副檔名，且提供簡潔的方式建立 HTML 輸出使用 C#。</span><span class="sxs-lookup"><span data-stu-id="cacf8-120">Razor-based view templates have a *.cshtml* file extension, and provide an elegant way to create HTML output using C#.</span></span> <span data-ttu-id="cacf8-121">Razor 的字元和撰寫檢視範本時所需按鍵數目降到最低，並可讓快速，流體，程式碼撰寫工作流程。</span><span class="sxs-lookup"><span data-stu-id="cacf8-121">Razor minimizes the number of characters and keystrokes required when writing a view template, and enables a fast, fluid coding workflow.</span></span>

<span data-ttu-id="cacf8-122">開始使用檢視範本與`Index`方法中的`HelloWorldController`類別。</span><span class="sxs-lookup"><span data-stu-id="cacf8-122">Start by using a view template with the `Index` method in the `HelloWorldController` class.</span></span> <span data-ttu-id="cacf8-123">目前，`Index` 方法會傳回字串，內含在控制器類別中硬式編碼的訊息。</span><span class="sxs-lookup"><span data-stu-id="cacf8-123">Currently the `Index` method returns a string with a message that is hard-coded in the controller class.</span></span> <span data-ttu-id="cacf8-124">變更`Index`方法以傳回`View`物件，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cacf8-124">Change the `Index` method to return a `View` object, as shown in the following:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

<span data-ttu-id="cacf8-125">這個程式碼中使用檢視範本產生瀏覽器的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="cacf8-125">This code uses a view template to generate an HTML response to the browser.</span></span> <span data-ttu-id="cacf8-126">在專案中，加入您可以使用檢視範本`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="cacf8-126">In the project, add a view template that you can use with the `Index` method.</span></span> <span data-ttu-id="cacf8-127">若要這樣做，以滑鼠右鍵按一下`Index`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="cacf8-127">To do this, right-click inside the `Index` method and click **Add View**.</span></span>

![](adding-a-view/_static/image1.png)

<span data-ttu-id="cacf8-128">**加入檢視** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="cacf8-128">The **Add View** dialog box appears.</span></span> <span data-ttu-id="cacf8-129">保留預設值的方式，然後按一下 **新增**按鈕：</span><span class="sxs-lookup"><span data-stu-id="cacf8-129">Leave the defaults the way they are and click the **Add** button:</span></span>

![](adding-a-view/_static/image2.png)

<span data-ttu-id="cacf8-130">*MvcMovie\Views\HelloWorld*資料夾和*MvcMovie\Views\HelloWorld\Index.cshtml*建立檔案。</span><span class="sxs-lookup"><span data-stu-id="cacf8-130">The *MvcMovie\Views\HelloWorld* folder and the *MvcMovie\Views\HelloWorld\Index.cshtml* file are created.</span></span> <span data-ttu-id="cacf8-131">您可以看到它們在**方案總管 中**:</span><span class="sxs-lookup"><span data-stu-id="cacf8-131">You can see them in **Solution Explorer**:</span></span>

![](adding-a-view/_static/image3.png)

<span data-ttu-id="cacf8-132">下列示範*Index.cshtml*所建立的檔案：</span><span class="sxs-lookup"><span data-stu-id="cacf8-132">The following shows the *Index.cshtml* file that was created:</span></span>

<span data-ttu-id="cacf8-133">[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="cacf8-133">[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)</span></span>

<span data-ttu-id="cacf8-134">將一些 HTML 下的`<h2>`標記。</span><span class="sxs-lookup"><span data-stu-id="cacf8-134">Add some HTML under the `<h2>` tag.</span></span> <span data-ttu-id="cacf8-135">修改*MvcMovie\Views\HelloWorld\Index.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="cacf8-135">The modified *MvcMovie\Views\HelloWorld\Index.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

<span data-ttu-id="cacf8-136">執行應用程式，並瀏覽至`HelloWorld`控制站 (`http://localhost:xxxx/HelloWorld`)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-136">Run the application and browse to the `HelloWorld` controller (`http://localhost:xxxx/HelloWorld`).</span></span> <span data-ttu-id="cacf8-137">`Index`在控制器方法不執行許多工作; 它只需執行陳述式`return View()`，其中指定的方法來呈現瀏覽器的回應，使用檢視範本檔案。</span><span class="sxs-lookup"><span data-stu-id="cacf8-137">The `Index` method in your controller didn't do much work; it simply ran the statement `return View()`, which specified that the method should use a view template file to render a response to the browser.</span></span> <span data-ttu-id="cacf8-138">因為您沒有明確指定要使用的檢視範本檔案的名稱，預設使用 ASP.NET MVC *Index.cshtml*中的檢視檔案*\Views\HelloWorld*資料夾。</span><span class="sxs-lookup"><span data-stu-id="cacf8-138">Because you didn't explicitly specify the name of the view template file to use, ASP.NET MVC defaulted to using the *Index.cshtml* view file in the *\Views\HelloWorld* folder.</span></span> <span data-ttu-id="cacf8-139">下圖顯示字串硬式編碼在檢視中。</span><span class="sxs-lookup"><span data-stu-id="cacf8-139">The image below shows the string hard-coded in the view.</span></span>

![](adding-a-view/_static/image6.png)

<span data-ttu-id="cacf8-140">看起來很好。</span><span class="sxs-lookup"><span data-stu-id="cacf8-140">Looks pretty good.</span></span> <span data-ttu-id="cacf8-141">不過，請注意，在瀏覽器標題列會顯示"Index"大的標題，在頁面上顯示 「 我 MVC 應用程式。 」</span><span class="sxs-lookup"><span data-stu-id="cacf8-141">However, notice that the browser's title bar says "Index" and the big title on the page says "My MVC Application."</span></span> <span data-ttu-id="cacf8-142">讓我們將這些變更。</span><span class="sxs-lookup"><span data-stu-id="cacf8-142">Let's change those.</span></span>

## <a name="changing-views-and-layout-pages"></a><span data-ttu-id="cacf8-143">變更檢視和版面配置頁</span><span class="sxs-lookup"><span data-stu-id="cacf8-143">Changing Views and Layout Pages</span></span>

<span data-ttu-id="cacf8-144">首先，您會想要變更在頁面頂端的 「 我的 MVC 應用程式 」 標題。</span><span class="sxs-lookup"><span data-stu-id="cacf8-144">First, you want to change the "My MVC Application" title at the top of the page.</span></span> <span data-ttu-id="cacf8-145">該文字會每一頁。</span><span class="sxs-lookup"><span data-stu-id="cacf8-145">That text is common to every page.</span></span> <span data-ttu-id="cacf8-146">它實際上被實作僅在一個地方，在專案中，雖然會顯示應用程式中的每一頁上。</span><span class="sxs-lookup"><span data-stu-id="cacf8-146">It actually is implemented in only one place in the project, even though it appears on every page in the application.</span></span> <span data-ttu-id="cacf8-147">移至 */檢視表/共用*資料夾中的**方案總管 中**並開啟 *\_Layout.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="cacf8-147">Go to the */Views/Shared* folder in **Solution Explorer** and open the *\_Layout.cshtml* file.</span></span> <span data-ttu-id="cacf8-148">這個檔案稱為*版面配置頁*而且它是共用 「 殼層 」 的所有其他頁面使用。</span><span class="sxs-lookup"><span data-stu-id="cacf8-148">This file is called a *layout page* and it's the shared "shell" that all other pages use.</span></span>

<span data-ttu-id="cacf8-149">[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="cacf8-149">[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)</span></span>

<span data-ttu-id="cacf8-150">版面配置範本可讓您指定 HTML 容器的配置，您的網站在同一個地方，然後將它套用到您的網站中的多個頁面。</span><span class="sxs-lookup"><span data-stu-id="cacf8-150">Layout templates allow you to specify the HTML container layout of your site in one place and then apply it across multiple pages in your site.</span></span> <span data-ttu-id="cacf8-151">請注意`@RenderBody()`檔案底部附近的行。</span><span class="sxs-lookup"><span data-stu-id="cacf8-151">Note the `@RenderBody()` line near the bottom of the file.</span></span> <span data-ttu-id="cacf8-152">`RenderBody` 這是的預留位置，其中您所建立的所有檢視特定頁面會都出現 「 包裝 」 中之版面配置頁。</span><span class="sxs-lookup"><span data-stu-id="cacf8-152">`RenderBody` is a placeholder where all the view-specific pages you create show up, "wrapped" in the layout page.</span></span> <span data-ttu-id="cacf8-153">變更從 「 我 MVC 應用程式 」 到 「 MVC 電影應用程式 」 的版面配置範本中的標題。</span><span class="sxs-lookup"><span data-stu-id="cacf8-153">Change the title heading in the layout template from "My MVC Application" to "MVC Movie App".</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

<span data-ttu-id="cacf8-154">執行應用程式，並注意它現在說 「 MVC 電影應用程式 」。</span><span class="sxs-lookup"><span data-stu-id="cacf8-154">Run the application and notice that it now says "MVC Movie App".</span></span> <span data-ttu-id="cacf8-155">按一下**有關**連結，而且您會看到如何該頁面會顯示"MVC 影片 App"太。</span><span class="sxs-lookup"><span data-stu-id="cacf8-155">Click the **About** link, and you see how that page shows "MVC Movie App", too.</span></span> <span data-ttu-id="cacf8-156">我們能夠在版面配置範本的一次進行變更，並已在網站上的所有頁面都反映新的標題。</span><span class="sxs-lookup"><span data-stu-id="cacf8-156">We were able to make the change once in the layout template and have all pages on the site reflect the new title.</span></span>

![](adding-a-view/_static/image9.png)

<span data-ttu-id="cacf8-157">完整 *\_Layout.cshtml*檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="cacf8-157">The complete *\_Layout.cshtml* file is shown below:</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

<span data-ttu-id="cacf8-158">現在，我們來變更索引頁面 （檢視） 的標題。</span><span class="sxs-lookup"><span data-stu-id="cacf8-158">Now, let's change the title of the Index page (view).</span></span>

<span data-ttu-id="cacf8-159">開啟*MvcMovie\Views\HelloWorld\Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="cacf8-159">Open *MvcMovie\Views\HelloWorld\Index.cshtml*.</span></span> <span data-ttu-id="cacf8-160">若要變更的兩個地方： 第一次，文字會出現在標題的瀏覽器，然後在次要標頭 (`<h2>`項目)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-160">There are two places to make a change: first, the text that appears in the title of the browser, and then in the secondary header (the `<h2>` element).</span></span> <span data-ttu-id="cacf8-161">您會使這些變更略有不同，因此可看出哪一段程式碼變更了應用程式的哪個部分。</span><span class="sxs-lookup"><span data-stu-id="cacf8-161">You'll make them slightly different so you can see which bit of code changes which part of the app.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

<span data-ttu-id="cacf8-162">若要表示要顯示，請設定上方的程式碼的 HTML 標題`Title`屬性`ViewBag`物件 (它是*Index.cshtml*檢視範本)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-162">To indicate the HTML title to display, the code above sets a `Title` property of the `ViewBag` object (which is in the *Index.cshtml* view template).</span></span> <span data-ttu-id="cacf8-163">如果您查看上一步的版面配置範本的原始程式碼，您會發現，範本會使用這個值`<title>`一部分的項目`<head>`> 一節的 html。</span><span class="sxs-lookup"><span data-stu-id="cacf8-163">If you look back at the source code of the layout template, you'll notice that the template uses this value in the `<title>` element as part of the `<head>` section of the HTML.</span></span> <span data-ttu-id="cacf8-164">使用這個方法，可以檢視範本與版面配置檔案之間輕鬆地傳遞其他參數。</span><span class="sxs-lookup"><span data-stu-id="cacf8-164">Using this approach, you can easily pass other parameters between your view template and your layout file.</span></span>

<span data-ttu-id="cacf8-165">執行應用程式，並瀏覽至`http://localhost:xx/HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="cacf8-165">Run the application and browse to `http://localhost:xx/HelloWorld`.</span></span> <span data-ttu-id="cacf8-166">請注意，瀏覽器標題、主要標題和次要標題已變更</span><span class="sxs-lookup"><span data-stu-id="cacf8-166">Notice that the browser title, the primary heading, and the secondary headings have changed.</span></span> <span data-ttu-id="cacf8-167">(如果您在瀏覽器中沒有看到變更，可能檢視的是快取的內容。</span><span class="sxs-lookup"><span data-stu-id="cacf8-167">(If you don't see changes in the browser, you might be viewing cached content.</span></span> <span data-ttu-id="cacf8-168">請在瀏覽器中按 Ctrl + F5 以強制載入來自伺服器的回應)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-168">Press Ctrl+F5 in your browser to force the response from the server to be loaded.)</span></span>

<span data-ttu-id="cacf8-169">同時也請注意如何在內容*Index.cshtml*檢視範本與合併，而 *\_Layout.cshtml*檢視範本和單一 HTML 回應已傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="cacf8-169">Also notice how the content in the *Index.cshtml* view template was merged with the *\_Layout.cshtml* view template and a single HTML response was sent to the browser.</span></span> <span data-ttu-id="cacf8-170">版面配置範本可讓您輕鬆進行會套用到應用程式之所有頁面的變更。</span><span class="sxs-lookup"><span data-stu-id="cacf8-170">Layout templates make it really easy to make changes that apply across all of the pages in your application.</span></span>

![](adding-a-view/_static/image10.png)

<span data-ttu-id="cacf8-171">然而，我們的這一點點「資料」(在此案例中為 "Hello from our View Template!"</span><span class="sxs-lookup"><span data-stu-id="cacf8-171">Our little bit of "data" (in this case the "Hello from our View Template!"</span></span> <span data-ttu-id="cacf8-172">訊息) 是硬式編碼的資料。</span><span class="sxs-lookup"><span data-stu-id="cacf8-172">message) is hard-coded, though.</span></span> <span data-ttu-id="cacf8-173">MVC 應用程式具有 "V" (檢視)，並已取得 "C" (控制器)，但還沒有 "M" (模型)。</span><span class="sxs-lookup"><span data-stu-id="cacf8-173">The MVC application has a "V" (view) and you've got a "C" (controller), but no "M" (model) yet.</span></span> <span data-ttu-id="cacf8-174">稍後，我們將逐步解說如何建立資料庫，並從其擷取模型資料。</span><span class="sxs-lookup"><span data-stu-id="cacf8-174">Shortly, we'll walk through how create a database and retrieve model data from it.</span></span>

## <a name="passing-data-from-the-controller-to-the-view"></a><span data-ttu-id="cacf8-175">將資料從控制器傳遞至檢視</span><span class="sxs-lookup"><span data-stu-id="cacf8-175">Passing Data from the Controller to the View</span></span>

<span data-ttu-id="cacf8-176">我們移至資料庫，並討論模型之前，不過，我們要先討論有關將資訊從控制器傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="cacf8-176">Before we go to a database and talk about models, though, let's first talk about passing information from the controller to a view.</span></span> <span data-ttu-id="cacf8-177">控制器類別會叫用，以回應傳入的 URL 要求。</span><span class="sxs-lookup"><span data-stu-id="cacf8-177">Controller classes are invoked in response to an incoming URL request.</span></span> <span data-ttu-id="cacf8-178">控制器類別是回應的您撰寫的程式碼會處理傳入的參數、 從資料庫擷取資料並最終決定傳送回瀏覽器類型的地方。</span><span class="sxs-lookup"><span data-stu-id="cacf8-178">A controller class is where you write the code that handles the incoming parameters, retrieves data from a database, and ultimately decides what type of response to send back to the browser.</span></span> <span data-ttu-id="cacf8-179">然後從控制器來產生並格式化 HTML 回應至瀏覽器使用檢視範本。</span><span class="sxs-lookup"><span data-stu-id="cacf8-179">View templates can then be used from a controller to generate and format an HTML response to the browser.</span></span>

<span data-ttu-id="cacf8-180">控制站會負責提供任何資料或物件必須轉譯至瀏覽器的回應的檢視範本的順序。</span><span class="sxs-lookup"><span data-stu-id="cacf8-180">Controllers are responsible for providing whatever data or objects are required in order for a view template to render a response to the browser.</span></span> <span data-ttu-id="cacf8-181">檢視範本應該永遠不會執行商務邏輯，或直接與資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="cacf8-181">A view template should never perform business logic or interact with a database directly.</span></span> <span data-ttu-id="cacf8-182">相反地，它應該僅適用於由控制器提供給它的資料。</span><span class="sxs-lookup"><span data-stu-id="cacf8-182">Instead, it should work only with the data that's provided to it by the controller.</span></span> <span data-ttu-id="cacf8-183">維護此 「 重要性分離 」，可協助確保您的程式碼，全新且更易於維護。</span><span class="sxs-lookup"><span data-stu-id="cacf8-183">Maintaining this "separation of concerns" helps keep your code clean and more maintainable.</span></span>

<span data-ttu-id="cacf8-184">目前，`Welcome`中的動作方法`HelloWorldController`類別會採用`name`和`numTimes`參數，然後輸出至瀏覽器直接的值。</span><span class="sxs-lookup"><span data-stu-id="cacf8-184">Currently, the `Welcome` action method in the `HelloWorldController` class takes a `name` and a `numTimes` parameter and then outputs the values directly to the browser.</span></span> <span data-ttu-id="cacf8-185">而非保有呈現為字串的這個回應的控制站，我們來變更要改為使用檢視範本的控制站。</span><span class="sxs-lookup"><span data-stu-id="cacf8-185">Rather than have the controller render this response as a string, let's change the controller to use a view template instead.</span></span> <span data-ttu-id="cacf8-186">檢視範本會產生動態回應，這表示您需要將適當數量的資料從控制器傳遞至檢視，以便產生回應。</span><span class="sxs-lookup"><span data-stu-id="cacf8-186">The view template will generate a dynamic response, which means that you need to pass appropriate bits of data from the controller to the view in order to generate the response.</span></span> <span data-ttu-id="cacf8-187">您可以透過將動態檢視需要的範本中的資料放在控制器`ViewBag`檢視範本可以存取的物件。</span><span class="sxs-lookup"><span data-stu-id="cacf8-187">You can do this by having the controller put the dynamic data that the view template needs in a `ViewBag` object that the view template can then access.</span></span>

<span data-ttu-id="cacf8-188">返回*HelloWorldController.cs*檔案，並且變更`Welcome`方法，將`Message`和`NumTimes`值設定為`ViewBag`物件。</span><span class="sxs-lookup"><span data-stu-id="cacf8-188">Return to the *HelloWorldController.cs* file and change the `Welcome` method to add a `Message` and `NumTimes` value to the `ViewBag` object.</span></span> <span data-ttu-id="cacf8-189">`ViewBag` 是動態的物件，這表示您可以將您所要的任何。`ViewBag`物件沒有任何定義的屬性，直到您將放在它之內的項目。</span><span class="sxs-lookup"><span data-stu-id="cacf8-189">`ViewBag` is a dynamic object, which means you can put whatever you want in to it; the `ViewBag` object has no defined properties until you put something inside it.</span></span> <span data-ttu-id="cacf8-190">完整的 *HelloWorldController.cs* 檔案如下所示：</span><span class="sxs-lookup"><span data-stu-id="cacf8-190">The complete *HelloWorldController.cs* file looks like this:</span></span>

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

<span data-ttu-id="cacf8-191">現在`ViewBag`物件包含會自動傳遞到檢視的資料。</span><span class="sxs-lookup"><span data-stu-id="cacf8-191">Now the `ViewBag` object contains data that will be passed to the view automatically.</span></span>

<span data-ttu-id="cacf8-192">接下來，您需要一個歡迎視圖模板！</span><span class="sxs-lookup"><span data-stu-id="cacf8-192">Next, you need a Welcome view template!</span></span> <span data-ttu-id="cacf8-193">在**偵錯**功能表上，選取**建置 MvcMovie**確定編譯專案。</span><span class="sxs-lookup"><span data-stu-id="cacf8-193">In the **Debug** menu, select **Build MvcMovie** to make sure the project is compiled.</span></span>

<span data-ttu-id="cacf8-194">[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="cacf8-194">[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)</span></span>

<span data-ttu-id="cacf8-195">然後以滑鼠右鍵按一下`Welcome`方法，然後按一下**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="cacf8-195">Then right-click inside the `Welcome` method and click **Add View**.</span></span> <span data-ttu-id="cacf8-196">以下是**加入檢視**對話方塊看起來像：</span><span class="sxs-lookup"><span data-stu-id="cacf8-196">Here's what the **Add View** dialog box looks like:</span></span>

![](adding-a-view/_static/image13.png)

<span data-ttu-id="cacf8-197">按一下**新增**，然後加入下列程式碼`<h2>`中新項目*Welcome.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="cacf8-197">Click **Add**, and then add the following code under the `<h2>` element in the new *Welcome.cshtml* file.</span></span> <span data-ttu-id="cacf8-198">您會建立迴圈，依需求多次使用者指出應該顯示"Hello"。</span><span class="sxs-lookup"><span data-stu-id="cacf8-198">You'll create a loop that says "Hello" as many times as the user says it should.</span></span> <span data-ttu-id="cacf8-199">完整*Welcome.cshtml*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="cacf8-199">The complete *Welcome.cshtml* file is shown below.</span></span>

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

<span data-ttu-id="cacf8-200">執行應用程式，並瀏覽至下列 URL:</span><span class="sxs-lookup"><span data-stu-id="cacf8-200">Run the application and browse to the following URL:</span></span>

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

<span data-ttu-id="cacf8-201">現在從 URL 取得及資料自動傳送到控制器。</span><span class="sxs-lookup"><span data-stu-id="cacf8-201">Now data is taken from the URL and passed to the controller automatically.</span></span> <span data-ttu-id="cacf8-202">控制器封裝資料`ViewBag`物件，以及檢視該物件傳遞。</span><span class="sxs-lookup"><span data-stu-id="cacf8-202">The controller packages the data into a `ViewBag` object and passes that object to the view.</span></span> <span data-ttu-id="cacf8-203">檢視然後會顯示為 HTML 資料給使用者。</span><span class="sxs-lookup"><span data-stu-id="cacf8-203">The view then displays the data as HTML to the user.</span></span>

![](adding-a-view/_static/image14.png)

<span data-ttu-id="cacf8-204">這是一種代表模型的 "M"，但不是資料庫類型。</span><span class="sxs-lookup"><span data-stu-id="cacf8-204">Well, that was a kind of an "M" for model, but not the database kind.</span></span> <span data-ttu-id="cacf8-205">讓我們運用所學的內容，建立電影的資料庫。</span><span class="sxs-lookup"><span data-stu-id="cacf8-205">Let's take what we've learned and create a database of movies.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="cacf8-206">[上一頁](adding-a-controller.md)
> [下一頁](adding-a-model.md)</span><span class="sxs-lookup"><span data-stu-id="cacf8-206">[Previous](adding-a-controller.md)
[Next](adding-a-model.md)</span></span>
