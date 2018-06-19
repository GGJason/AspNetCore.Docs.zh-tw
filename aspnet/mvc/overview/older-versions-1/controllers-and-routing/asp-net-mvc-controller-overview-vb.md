---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
title: ASP.NET MVC 控制器概觀 (VB) |Microsoft 文件
author: StephenWalther
description: 在本教學課程中，作者： Stephen Walther 向您介紹 ASP.NET MVC 控制器。 您了解如何建立新的控制站，並傳回不同類型的動作 res...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2008
ms.topic: article
ms.assetid: 94c3e5d9-a904-445e-a34e-d92fd1ca108a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-controller-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 0a45630e8f2d5ae0548bb6b8496df08ca5877a40
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868980"
---
<a name="aspnet-mvc-controller-overview-vb"></a><span data-ttu-id="e2f52-104">ASP.NET MVC 控制器概觀 (VB)</span><span class="sxs-lookup"><span data-stu-id="e2f52-104">ASP.NET MVC Controller Overview (VB)</span></span>
====================
<span data-ttu-id="e2f52-105">由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="e2f52-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="e2f52-106">在本教學課程中，作者： Stephen Walther 向您介紹 ASP.NET MVC 控制器。</span><span class="sxs-lookup"><span data-stu-id="e2f52-106">In this tutorial, Stephen Walther introduces you to ASP.NET MVC controllers.</span></span> <span data-ttu-id="e2f52-107">您了解如何建立新的控制站，並傳回不同類型的動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-107">You learn how to create new controllers and return different types of action results.</span></span>


<span data-ttu-id="e2f52-108">本教學課程中，瀏覽 ASP.NET MVC 控制器、 控制器動作和動作結果的主題。</span><span class="sxs-lookup"><span data-stu-id="e2f52-108">This tutorial explores the topic of ASP.NET MVC controllers, controller actions, and action results.</span></span> <span data-ttu-id="e2f52-109">完成本教學課程之後，您將了解如何控制站用來控制與 ASP.NET MVC 網站的訪客互動的方式。</span><span class="sxs-lookup"><span data-stu-id="e2f52-109">After you complete this tutorial, you will understand how controllers are used to control the way a visitor interacts with an ASP.NET MVC website.</span></span>

## <a name="understanding-controllers"></a><span data-ttu-id="e2f52-110">了解控制器</span><span class="sxs-lookup"><span data-stu-id="e2f52-110">Understanding Controllers</span></span>

<span data-ttu-id="e2f52-111">MVC 控制器負責回應對 ASP.NET MVC 網站提出的要求。</span><span class="sxs-lookup"><span data-stu-id="e2f52-111">MVC controllers are responsible for responding to requests made against an ASP.NET MVC website.</span></span> <span data-ttu-id="e2f52-112">每個瀏覽器要求對應到特定的控制站。</span><span class="sxs-lookup"><span data-stu-id="e2f52-112">Each browser request is mapped to a particular controller.</span></span> <span data-ttu-id="e2f52-113">例如，假設您的瀏覽器的網址列輸入下列 URL:</span><span class="sxs-lookup"><span data-stu-id="e2f52-113">For example, imagine that you enter the following URL into the address bar of your browser:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="e2f52-114">在此情況下，會叫用名為 ProductController 的控制站。</span><span class="sxs-lookup"><span data-stu-id="e2f52-114">In this case, a controller named ProductController is invoked.</span></span> <span data-ttu-id="e2f52-115">ProductController 是負責產生瀏覽器要求的回應。</span><span class="sxs-lookup"><span data-stu-id="e2f52-115">The ProductController is responsible for generating the response to the browser request.</span></span> <span data-ttu-id="e2f52-116">例如，控制器可能會傳回至瀏覽器的特定檢視或控制站可能會將使用者重新導向至另一個控制器。</span><span class="sxs-lookup"><span data-stu-id="e2f52-116">For example, the controller might return a particular view back to the browser or the controller might redirect the user to another controller.</span></span>

<span data-ttu-id="e2f52-117">清單 1 包含名為 ProductController 簡單控制器。</span><span class="sxs-lookup"><span data-stu-id="e2f52-117">Listing 1 contains a simple controller named ProductController.</span></span>

<span data-ttu-id="e2f52-118">**Listing1 - Controllers\ProductController.vb**</span><span class="sxs-lookup"><span data-stu-id="e2f52-118">**Listing1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample1.vb)]

<span data-ttu-id="e2f52-119">您可以看到列出 1，控制站是只類別 （Visual Basic.NET 或 C# 類別）。</span><span class="sxs-lookup"><span data-stu-id="e2f52-119">As you can see from Listing 1, a controller is just a class (a Visual Basic .NET or C# class).</span></span> <span data-ttu-id="e2f52-120">控制站是衍生自基底 System.Web.Mvc.Controller 類別的類別。</span><span class="sxs-lookup"><span data-stu-id="e2f52-120">A controller is a class that derives from the base System.Web.Mvc.Controller class.</span></span> <span data-ttu-id="e2f52-121">由於控制器都繼承自這個基底類別，控制站可免費繼承數種有用方法 （我們討論這些方法在不久後）。</span><span class="sxs-lookup"><span data-stu-id="e2f52-121">Because a controller inherits from this base class, a controller inherits several useful methods for free (We discuss these methods in a moment).</span></span>

## <a name="understanding-controller-actions"></a><span data-ttu-id="e2f52-122">了解控制器動作</span><span class="sxs-lookup"><span data-stu-id="e2f52-122">Understanding Controller Actions</span></span>

<span data-ttu-id="e2f52-123">控制器會公開控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="e2f52-123">A controller exposes controller actions.</span></span> <span data-ttu-id="e2f52-124">動作是當您在瀏覽器位址列中輸入特定的 URL 時所呼叫的控制站上的方法。</span><span class="sxs-lookup"><span data-stu-id="e2f52-124">An action is a method on a controller that gets called when you enter a particular URL in your browser address bar.</span></span> <span data-ttu-id="e2f52-125">例如，假設您提出要求，下列 url:</span><span class="sxs-lookup"><span data-stu-id="e2f52-125">For example, imagine that you make a request for the following URL:</span></span>

`http://localhost/Product/Index/3`

<span data-ttu-id="e2f52-126">在此情況下，index （） 方法 ProductController 類別上呼叫。</span><span class="sxs-lookup"><span data-stu-id="e2f52-126">In this case, the Index() method is called on the ProductController class.</span></span> <span data-ttu-id="e2f52-127">Index （） 方法是控制器動作的範例。</span><span class="sxs-lookup"><span data-stu-id="e2f52-127">The Index() method is an example of a controller action.</span></span>

<span data-ttu-id="e2f52-128">控制器動作，必須是公用方法的控制器類別。</span><span class="sxs-lookup"><span data-stu-id="e2f52-128">A controller action must be a public method of a controller class.</span></span> <span data-ttu-id="e2f52-129">根據預設，visual basic.net 方法都是公用的方法。</span><span class="sxs-lookup"><span data-stu-id="e2f52-129">Visual Basic.NET methods, by default, are public methods.</span></span> <span data-ttu-id="e2f52-130">了解的任何公用方法，您將加入至控制器類別會自動公開為控制器動作 （您必須小心這因為控制器動作，可以叫用宇宙中的任何人只要在瀏覽器網址列中輸入正確的 URL）。</span><span class="sxs-lookup"><span data-stu-id="e2f52-130">Realize that any public method that you add to a controller class is exposed as a controller action automatically (You must be careful about this since a controller action can be invoked by anyone in the universe simply by typing the right URL into a browser address bar).</span></span>

<span data-ttu-id="e2f52-131">有一些必須滿足的控制器動作的其他需求。</span><span class="sxs-lookup"><span data-stu-id="e2f52-131">There are some additional requirements that must be satisfied by a controller action.</span></span> <span data-ttu-id="e2f52-132">做為控制器動作方法無法多載。</span><span class="sxs-lookup"><span data-stu-id="e2f52-132">A method used as a controller action cannot be overloaded.</span></span> <span data-ttu-id="e2f52-133">此外，控制器動作不能是靜態方法。</span><span class="sxs-lookup"><span data-stu-id="e2f52-133">Furthermore, a controller action cannot be a static method.</span></span> <span data-ttu-id="e2f52-134">除此之外，您可以使用幾乎任何方法做為控制器動作。</span><span class="sxs-lookup"><span data-stu-id="e2f52-134">Other than that, you can use just about any method as a controller action.</span></span>

## <a name="understanding-action-results"></a><span data-ttu-id="e2f52-135">了解動作結果</span><span class="sxs-lookup"><span data-stu-id="e2f52-135">Understanding Action Results</span></span>

<span data-ttu-id="e2f52-136">控制器動作傳回所謂*動作結果*。</span><span class="sxs-lookup"><span data-stu-id="e2f52-136">A controller action returns something called an *action result*.</span></span> <span data-ttu-id="e2f52-137">動作結果會是在瀏覽器要求的回應中傳回的控制器動作。</span><span class="sxs-lookup"><span data-stu-id="e2f52-137">An action result is what a controller action returns in response to a browser request.</span></span>

<span data-ttu-id="e2f52-138">ASP.NET MVC 架構支援數種類型的動作結果，包括：</span><span class="sxs-lookup"><span data-stu-id="e2f52-138">The ASP.NET MVC framework supports several types of action results including:</span></span>

1. <span data-ttu-id="e2f52-139">ViewResult-代表 HTML 和標記。</span><span class="sxs-lookup"><span data-stu-id="e2f52-139">ViewResult - Represents HTML and markup.</span></span>
2. <span data-ttu-id="e2f52-140">EmptyResult-表示沒有結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-140">EmptyResult - Represents no result.</span></span>
3. <span data-ttu-id="e2f52-141">RedirectResult-代表重新導向至新的 URL。</span><span class="sxs-lookup"><span data-stu-id="e2f52-141">RedirectResult - Represents a redirection to a new URL.</span></span>
4. <span data-ttu-id="e2f52-142">JsonResult-表示可用於 AJAX 應用程式的 JavaScript 物件標記法結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-142">JsonResult - Represents a JavaScript Object Notation result that can be used in an AJAX application.</span></span>
5. <span data-ttu-id="e2f52-143">JavaScriptResult-代表 JavaScript 指令碼。</span><span class="sxs-lookup"><span data-stu-id="e2f52-143">JavaScriptResult - Represents a JavaScript script.</span></span>
6. <span data-ttu-id="e2f52-144">ContentResult-表示文字結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-144">ContentResult - Represents a text result.</span></span>
7. <span data-ttu-id="e2f52-145">FileContentResult-代表可下載檔案 （具有二進位內容）。</span><span class="sxs-lookup"><span data-stu-id="e2f52-145">FileContentResult - Represents a downloadable file (with the binary content).</span></span>
8. <span data-ttu-id="e2f52-146">FilePathResult-表示可下載檔案 （路徑）。</span><span class="sxs-lookup"><span data-stu-id="e2f52-146">FilePathResult - Represents a downloadable file (with a path).</span></span>
9. <span data-ttu-id="e2f52-147">FileStreamResult-代表可下載檔案 （使用檔案資料流）。</span><span class="sxs-lookup"><span data-stu-id="e2f52-147">FileStreamResult - Represents a downloadable file (with a file stream).</span></span>

<span data-ttu-id="e2f52-148">所有這些動作會繼承自基底 ActionResult 類別。</span><span class="sxs-lookup"><span data-stu-id="e2f52-148">All of these action results inherit from the base ActionResult class.</span></span>

<span data-ttu-id="e2f52-149">在大部分情況下，控制器動作傳回 ViewResult。</span><span class="sxs-lookup"><span data-stu-id="e2f52-149">In most cases, a controller action returns a ViewResult.</span></span> <span data-ttu-id="e2f52-150">例如，列出 2 中的 Index 控制器動作傳回 ViewResult。</span><span class="sxs-lookup"><span data-stu-id="e2f52-150">For example, the Index controller action in Listing 2 returns a ViewResult.</span></span>

<span data-ttu-id="e2f52-151">**Listing 2 - Controllers\BookController.vb**</span><span class="sxs-lookup"><span data-stu-id="e2f52-151">**Listing 2 - Controllers\BookController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample2.vb)]

<span data-ttu-id="e2f52-152">當動作傳回 ViewResult 時，HTML 會傳回至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e2f52-152">When an action returns a ViewResult, HTML is returned to the browser.</span></span> <span data-ttu-id="e2f52-153">列出 2 中的 index （） 方法會傳回名為 Index 至瀏覽器的檢視。</span><span class="sxs-lookup"><span data-stu-id="e2f52-153">The Index() method in Listing 2 returns a view named Index to the browser.</span></span>

<span data-ttu-id="e2f52-154">請注意，index （） 中的動作清單 2 不會傳回 ViewResult()。</span><span class="sxs-lookup"><span data-stu-id="e2f52-154">Notice that the Index() action in Listing 2 does not return a ViewResult().</span></span> <span data-ttu-id="e2f52-155">相反地，會呼叫控制器的基底類別 View() 方法。</span><span class="sxs-lookup"><span data-stu-id="e2f52-155">Instead, the View() method of the Controller base class is called.</span></span> <span data-ttu-id="e2f52-156">一般來說，您不會傳回的動作結果直接。</span><span class="sxs-lookup"><span data-stu-id="e2f52-156">Normally, you do not return an action result directly.</span></span> <span data-ttu-id="e2f52-157">相反地，您呼叫其中一個控制器的基底類別的下列方法：</span><span class="sxs-lookup"><span data-stu-id="e2f52-157">Instead, you call one of the following methods of the Controller base class:</span></span>

1. <span data-ttu-id="e2f52-158">檢視-傳回 ViewResult 動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-158">View - Returns a ViewResult action result.</span></span>
2. <span data-ttu-id="e2f52-159">重新導向-傳回 RedirectResult 動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-159">Redirect - Returns a RedirectResult action result.</span></span>
3. <span data-ttu-id="e2f52-160">RedirectToAction-傳回 RedirectToRouteResult 動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-160">RedirectToAction - Returns a RedirectToRouteResult action result.</span></span>
4. <span data-ttu-id="e2f52-161">RedirectToRoute-傳回 RedirectToRouteResult 動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-161">RedirectToRoute - Returns a RedirectToRouteResult action result.</span></span>
5. <span data-ttu-id="e2f52-162">Json-傳回 JsonResult 動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-162">Json - Returns a JsonResult action result.</span></span>
6. <span data-ttu-id="e2f52-163">JavaScriptResult-傳回 JavaScriptResult。</span><span class="sxs-lookup"><span data-stu-id="e2f52-163">JavaScriptResult - Returns a JavaScriptResult.</span></span>
7. <span data-ttu-id="e2f52-164">內容-傳回 ContentResult 動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-164">Content - Returns a ContentResult action result.</span></span>
8. <span data-ttu-id="e2f52-165">檔案-FileContentResult、 FilePathResult 或 FileStreamResult 根據的參數傳遞給方法的傳回。</span><span class="sxs-lookup"><span data-stu-id="e2f52-165">File - Returns a FileContentResult, FilePathResult, or FileStreamResult depending on the parameters passed to the method.</span></span>

<span data-ttu-id="e2f52-166">因此，如果您想要傳回至瀏覽器的檢視，您可以呼叫 View() 方法。</span><span class="sxs-lookup"><span data-stu-id="e2f52-166">So, if you want to return a View to the browser, you call the View() method.</span></span> <span data-ttu-id="e2f52-167">如果您想要重新導向至另一個控制器動作的使用者，您會呼叫 RedirectToAction() 方法。</span><span class="sxs-lookup"><span data-stu-id="e2f52-167">If you want to redirect the user from one controller action to another, you call the RedirectToAction() method.</span></span> <span data-ttu-id="e2f52-168">例如，Details() 動作中列出的 3 顯示的檢視，或是將使用者導向至 index （） 動作，根據是否 Id 參數的值。</span><span class="sxs-lookup"><span data-stu-id="e2f52-168">For example, the Details() action in Listing 3 either displays a view or redirects the user to the Index() action depending on whether the Id parameter has a value.</span></span>

<span data-ttu-id="e2f52-169">**列出 3-CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="e2f52-169">**Listing 3 - CustomerController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample3.vb)]

<span data-ttu-id="e2f52-170">ContentResult 動作結果是特殊的。</span><span class="sxs-lookup"><span data-stu-id="e2f52-170">The ContentResult action result is special.</span></span> <span data-ttu-id="e2f52-171">您可以使用 ContentResult 動作結果傳回成純文字的動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-171">You can use the ContentResult action result to return an action result as plain text.</span></span> <span data-ttu-id="e2f52-172">比方說，在列出的 4 index （） 方法會傳回訊息以純文字，而不是 HTML。</span><span class="sxs-lookup"><span data-stu-id="e2f52-172">For example, the Index() method in Listing 4 returns a message as plain text and not as HTML.</span></span>

<span data-ttu-id="e2f52-173">**列出 4-Controllers\StatusController.vb**</span><span class="sxs-lookup"><span data-stu-id="e2f52-173">**Listing 4 - Controllers\StatusController.vb**</span></span>

> <span data-ttu-id="e2f52-174">StatusController</span><span class="sxs-lookup"><span data-stu-id="e2f52-174">StatusController</span></span>
> 
> 
> <span data-ttu-id="e2f52-175">System.Web.Mvc.Controller</span><span class="sxs-lookup"><span data-stu-id="e2f52-175">System.Web.Mvc.Controller</span></span>


[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample4.vb)]

<span data-ttu-id="e2f52-176">叫用 StatusController.Index() 動作時，不會傳回檢視表。</span><span class="sxs-lookup"><span data-stu-id="e2f52-176">When the StatusController.Index() action is invoked, a view is not returned.</span></span> <span data-ttu-id="e2f52-177">相反地，未經處理的文字"Hello World ！"</span><span class="sxs-lookup"><span data-stu-id="e2f52-177">Instead, the raw text "Hello World!"</span></span> <span data-ttu-id="e2f52-178">會傳回給瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="e2f52-178">is returned to the browser.</span></span>

<span data-ttu-id="e2f52-179">如果控制器動作傳回的結果是不是動作結果-例如，日期或整數-然後結果會包裝在 ContentResult 自動。</span><span class="sxs-lookup"><span data-stu-id="e2f52-179">If a controller action returns a result that is not an action result - for example, a date or an integer - then the result is wrapped in a ContentResult automatically.</span></span> <span data-ttu-id="e2f52-180">例如，叫用的列表 5 中 WorkController index 動作時，日期會當成 ContentResult 自動。</span><span class="sxs-lookup"><span data-stu-id="e2f52-180">For example, when the Index() action of the WorkController in Listing 5 is invoked, the date is returned as a ContentResult automatically.</span></span>

<span data-ttu-id="e2f52-181">**列出 5-WorkController.vb**</span><span class="sxs-lookup"><span data-stu-id="e2f52-181">**Listing 5 - WorkController.vb**</span></span>

[!code-vb[Main](asp-net-mvc-controller-overview-vb/samples/sample5.vb)]

<span data-ttu-id="e2f52-182">Index （） 中的動作清單 5 會傳回 DateTime 物件。</span><span class="sxs-lookup"><span data-stu-id="e2f52-182">The Index() action in Listing 5 returns a DateTime object.</span></span> <span data-ttu-id="e2f52-183">ASP.NET MVC 架構將 DateTime 物件轉換為字串和日期時間值中 ContentResult 會自動換行。</span><span class="sxs-lookup"><span data-stu-id="e2f52-183">The ASP.NET MVC framework converts the DateTime object to a string and wraps the DateTime value in a ContentResult automatically.</span></span> <span data-ttu-id="e2f52-184">瀏覽器接收的日期和時間以純文字。</span><span class="sxs-lookup"><span data-stu-id="e2f52-184">The browser receives the date and time as plain text.</span></span>

## <a name="summary"></a><span data-ttu-id="e2f52-185">總結</span><span class="sxs-lookup"><span data-stu-id="e2f52-185">Summary</span></span>

<span data-ttu-id="e2f52-186">本教學課程的目的是為您介紹的概念 ASP.NET MVC 控制器、 控制器動作，以及控制器動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-186">The purpose of this tutorial was to introduce you to the concepts of ASP.NET MVC controllers, controller actions, and controller action results.</span></span> <span data-ttu-id="e2f52-187">在第一個區段中，您將學會如何將新的控制站新增至 ASP.NET MVC 專案。</span><span class="sxs-lookup"><span data-stu-id="e2f52-187">In the first section, you learned how to add new controllers to an ASP.NET MVC project.</span></span> <span data-ttu-id="e2f52-188">接下來，您學到如何公用方法，控制站都會公開至 universe 當做控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="e2f52-188">Next, you learned how public methods of a controller are exposed to the universe as controller actions.</span></span> <span data-ttu-id="e2f52-189">最後，我們將討論不同類型的可從控制器動作傳回的動作結果。</span><span class="sxs-lookup"><span data-stu-id="e2f52-189">Finally, we discussed the different types of action results that can be returned from a controller action.</span></span> <span data-ttu-id="e2f52-190">特別是，我們將討論如何從控制器動作傳回 ViewResult、 RedirectToActionResult 和 ContentResult。</span><span class="sxs-lookup"><span data-stu-id="e2f52-190">In particular, we discussed how to return a ViewResult, RedirectToActionResult, and ContentResult from a controller action.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e2f52-191">[上一頁](creating-a-custom-route-constraint-cs.md)
> [下一頁](creating-custom-routes-vb.md)</span><span class="sxs-lookup"><span data-stu-id="e2f52-191">[Previous](creating-a-custom-route-constraint-cs.md)
[Next](creating-custom-routes-vb.md)</span></span>
