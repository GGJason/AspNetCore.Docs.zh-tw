---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
title: ASP.NET MVC 4 自訂動作篩選條件 |Microsoft 文件
author: rick-anderson
description: ASP.NET MVC 提供動作篩選條件之前或之後呼叫動作方法執行篩選邏輯。 動作篩選條件是自訂屬性 tha...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 969ab824-1b98-4552-81fe-b60ef5fc6887
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-custom-action-filters
msc.type: authoredcontent
ms.openlocfilehash: 8b135b23aea64b0c7c7d4368eef9ee80914159e4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877706"
---
# <a name="aspnet-mvc-4-custom-action-filters"></a><span data-ttu-id="27f02-104">ASP.NET MVC 4 自訂動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="27f02-104">ASP.NET MVC 4 Custom Action Filters</span></span>

<span data-ttu-id="27f02-105">由[Web 營小組](https://twitter.com/webcamps)</span><span class="sxs-lookup"><span data-stu-id="27f02-105">By [Web Camps Team](https://twitter.com/webcamps)</span></span>

[<span data-ttu-id="27f02-106">下載 Web 營訓練套件</span><span class="sxs-lookup"><span data-stu-id="27f02-106">Download Web Camps Training Kit</span></span>](https://aka.ms/webcamps-training-kit)

<span data-ttu-id="27f02-107">ASP.NET MVC 提供動作篩選條件之前或之後呼叫動作方法執行篩選邏輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-107">ASP.NET MVC provides Action Filters for executing filtering logic either before or after an action method is called.</span></span> <span data-ttu-id="27f02-108">動作篩選條件是自訂屬性，提供將動作前和動作後的行為加入控制器動作方法的宣告式方法。</span><span class="sxs-lookup"><span data-stu-id="27f02-108">Action Filters are custom attributes that provide declarative means to add pre-action and post-action behavior to the controller's action methods.</span></span>

<span data-ttu-id="27f02-109">在這個實際操作實驗室中，您將建立自訂動作篩選條件屬性到 MvcMusicStore 解決方案來攔截控制器的要求，並記錄在站台到資料庫資料表的活動。</span><span class="sxs-lookup"><span data-stu-id="27f02-109">In this Hands-on Lab you will create a custom action filter attribute into MvcMusicStore solution to catch controller's requests and log the activity of a site into a database table.</span></span> <span data-ttu-id="27f02-110">您可以將記錄篩選的資料隱碼加入至任何控制器或動作。</span><span class="sxs-lookup"><span data-stu-id="27f02-110">You will be able to add your logging filter by injection to any controller or action.</span></span> <span data-ttu-id="27f02-111">最後，您會看到記錄檢視，顯示造訪者的清單。</span><span class="sxs-lookup"><span data-stu-id="27f02-111">Finally, you will see the log view that shows the list of visitors.</span></span>

<span data-ttu-id="27f02-112">這個實際操作實驗室假設您擁有的基本知識**ASP.NET MVC**。</span><span class="sxs-lookup"><span data-stu-id="27f02-112">This Hands-on Lab assumes you have basic knowledge of **ASP.NET MVC**.</span></span> <span data-ttu-id="27f02-113">如果您不使用**ASP.NET MVC**之前，我們建議您在一段**ASP.NET MVC 4 的基本概念**實際操作實驗室。</span><span class="sxs-lookup"><span data-stu-id="27f02-113">If you have not used **ASP.NET MVC** before, we recommend you to go over **ASP.NET MVC 4 Fundamentals** Hands-on Lab.</span></span>

> [!NOTE]
> <span data-ttu-id="27f02-114">所有的範例程式碼和程式碼片段會包含在 Web 營訓練套件，可從在[Microsoft-Web/WebCampTrainingKit 版本](https://aka.ms/webcamps-training-kit)。</span><span class="sxs-lookup"><span data-stu-id="27f02-114">All sample code and snippets are included in the Web Camps Training Kit, available from at [Microsoft-Web/WebCampTrainingKit Releases](https://aka.ms/webcamps-training-kit).</span></span> <span data-ttu-id="27f02-115">這個實驗室中的特定專案將會位於[ASP.NET MVC 4 自訂動作篩選條件](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters)。</span><span class="sxs-lookup"><span data-stu-id="27f02-115">The project specific to this lab is available at [ASP.NET MVC 4 Custom Action Filters](https://github.com/Microsoft-Web/HOL-MVC4CustomActionFilters).</span></span>

<a id="Objectives"></a>
### <a name="objectives"></a><span data-ttu-id="27f02-116">目標</span><span class="sxs-lookup"><span data-stu-id="27f02-116">Objectives</span></span>

<span data-ttu-id="27f02-117">在這個實際操作實驗室中，您將學會如何：</span><span class="sxs-lookup"><span data-stu-id="27f02-117">In this Hands-On Lab, you will learn how to:</span></span>

- <span data-ttu-id="27f02-118">建立自訂動作篩選條件屬性，來擴充篩選的功能</span><span class="sxs-lookup"><span data-stu-id="27f02-118">Create a custom action filter attribute to extend filtering capabilities</span></span>
- <span data-ttu-id="27f02-119">自訂的篩選條件屬性套用至特定層級的資料隱碼</span><span class="sxs-lookup"><span data-stu-id="27f02-119">Apply a custom filter attribute by injection to a specific level</span></span>
- <span data-ttu-id="27f02-120">全域註冊自訂動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="27f02-120">Register a custom action filters globally</span></span>

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a><span data-ttu-id="27f02-121">必要條件</span><span class="sxs-lookup"><span data-stu-id="27f02-121">Prerequisites</span></span>

<span data-ttu-id="27f02-122">您必須具備下列項目才能完成本實驗室：</span><span class="sxs-lookup"><span data-stu-id="27f02-122">You must have the following items to complete this lab:</span></span>

- <span data-ttu-id="27f02-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web)或上層 (讀取[附錄 A](#AppendixA)如需有關如何安裝指示)。</span><span class="sxs-lookup"><span data-stu-id="27f02-123">[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) or superior (read [Appendix A](#AppendixA) for instructions on how to install it).</span></span>

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a><span data-ttu-id="27f02-124">安裝程式</span><span class="sxs-lookup"><span data-stu-id="27f02-124">Setup</span></span>

<span data-ttu-id="27f02-125">**安裝程式碼片段**</span><span class="sxs-lookup"><span data-stu-id="27f02-125">**Installing Code Snippets**</span></span>

<span data-ttu-id="27f02-126">為了方便起見，大部分您將沿著這個實驗室管理的程式碼可做為 Visual Studio 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="27f02-126">For convenience, much of the code you will be managing along this lab is available as Visual Studio code snippets.</span></span> <span data-ttu-id="27f02-127">若要安裝執行的程式碼片段 **.\Source\Setup\CodeSnippets.vsi**檔案。</span><span class="sxs-lookup"><span data-stu-id="27f02-127">To install the code snippets run **.\Source\Setup\CodeSnippets.vsi** file.</span></span>

<span data-ttu-id="27f02-128">如果您不熟悉 Visual Studio 程式碼片段，而且想来了解如何使用它們，您可以從這份文件參考附錄&quot;[附錄 c： 使用程式碼片段](#AppendixC)&quot;。</span><span class="sxs-lookup"><span data-stu-id="27f02-128">If you are not familiar with the Visual Studio Code Snippets, and want to learn how to use them, you can refer to the appendix from this document &quot;[Appendix C: Using Code Snippets](#AppendixC)&quot;.</span></span>

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a><span data-ttu-id="27f02-129">練習</span><span class="sxs-lookup"><span data-stu-id="27f02-129">Exercises</span></span>

<span data-ttu-id="27f02-130">這個實際操作實驗室是由下列練習所組成：</span><span class="sxs-lookup"><span data-stu-id="27f02-130">This Hands-On Lab is comprised by the following exercises:</span></span>

1. [<span data-ttu-id="27f02-131">練習 1： 記錄動作</span><span class="sxs-lookup"><span data-stu-id="27f02-131">Exercise 1: Logging actions</span></span>](#Exercise1)
2. [<span data-ttu-id="27f02-132">練習 2： 管理多個動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="27f02-132">Exercise 2: Managing Multiple Action Filters</span></span>](#Exercise2)

<span data-ttu-id="27f02-133">完成本實驗室估計時間： **30 分鐘**。</span><span class="sxs-lookup"><span data-stu-id="27f02-133">Estimated time to complete this lab: **30 minutes**.</span></span>

> [!NOTE]
> <span data-ttu-id="27f02-134">每個練習會伴隨著**結束**包含所產生的解決方案完成練習之後，您應該取得資料夾。</span><span class="sxs-lookup"><span data-stu-id="27f02-134">Each exercise is accompanied by an **End** folder containing the resulting solution you should obtain after completing the exercises.</span></span> <span data-ttu-id="27f02-135">如果您需要其他協助使用的所有練習，您可以使用這個方案做為指南。</span><span class="sxs-lookup"><span data-stu-id="27f02-135">You can use this solution as a guide if you need additional help working through the exercises.</span></span>


<a id="Exercise1"></a>

<a id="Exercise_1_Logging_Actions"></a>
### <a name="exercise-1-logging-actions"></a><span data-ttu-id="27f02-136">練習 1： 記錄動作</span><span class="sxs-lookup"><span data-stu-id="27f02-136">Exercise 1: Logging Actions</span></span>

<span data-ttu-id="27f02-137">在此練習中，您將學習如何使用 ASP.NET MVC 4 篩選條件提供者建立自訂動作記錄的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-137">In this exercise, you will learn how to create a custom action log filter by using ASP.NET MVC 4 Filter Providers.</span></span> <span data-ttu-id="27f02-138">針對該目的您會將記錄篩選器套用至選取控制站會記錄所有活動的 MusicStore 站台。</span><span class="sxs-lookup"><span data-stu-id="27f02-138">For that purpose you will apply a logging filter to the MusicStore site that will record all the activities in the selected controllers.</span></span>

<span data-ttu-id="27f02-139">篩選會擴充**ActionFilterAttributeClass**並覆寫**OnActionExecuting**攔截每個要求，然後執行 記錄動作方法。</span><span class="sxs-lookup"><span data-stu-id="27f02-139">The filter will extend **ActionFilterAttributeClass** and override **OnActionExecuting** method to catch each request and then perform the logging actions.</span></span> <span data-ttu-id="27f02-140">HTTP 要求的內容資訊中，執行方法、 結果和參數將由 ASP.NET MVC 提供**ActionExecutingContext**類別 **。**</span><span class="sxs-lookup"><span data-stu-id="27f02-140">The context information about HTTP requests, executing methods, results and parameters will be provided by ASP.NET MVC **ActionExecutingContext** class **.**</span></span>

> [!NOTE]
> <span data-ttu-id="27f02-141">ASP.NET MVC 4 也有預設篩選器提供者，您可以使用而建立自訂篩選器。</span><span class="sxs-lookup"><span data-stu-id="27f02-141">ASP.NET MVC 4 also has default filters providers you can use without creating a custom filter.</span></span> <span data-ttu-id="27f02-142">ASP.NET MVC 4 提供下列類型的篩選器：</span><span class="sxs-lookup"><span data-stu-id="27f02-142">ASP.NET MVC 4 provides the following types of filters:</span></span>
> 
> - <span data-ttu-id="27f02-143">**授權**篩選，讓安全性決策，決定是否要執行的動作方法，例如執行驗證或驗證要求的屬性。</span><span class="sxs-lookup"><span data-stu-id="27f02-143">**Authorization** filter, which makes security decisions about whether to execute an action method, such as performing authentication or validating properties of the request.</span></span>
> - <span data-ttu-id="27f02-144">**動作**包裝動作方法執行篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-144">**Action** filter, which wraps the action method execution.</span></span> <span data-ttu-id="27f02-145">此篩選器可以執行額外處理，例如提供動作方法額外的資料、 檢查傳回的值，或取消執行的動作方法</span><span class="sxs-lookup"><span data-stu-id="27f02-145">This filter can perform additional processing, such as providing extra data to the action method, inspecting the return value, or canceling execution of the action method</span></span>
> - <span data-ttu-id="27f02-146">**結果**包裝 ActionResult 物件執行篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-146">**Result** filter, which wraps execution of the ActionResult object.</span></span> <span data-ttu-id="27f02-147">此篩選器可以執行額外處理的結果，例如修改 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="27f02-147">This filter can perform additional processing of the result, such as modifying the HTTP response.</span></span>
> - <span data-ttu-id="27f02-148">**例外狀況**篩選器，它會執行處理的動作方法，授權篩選條件的開始和結束與結果的執行中的某處擲回的例外狀況時。</span><span class="sxs-lookup"><span data-stu-id="27f02-148">**Exception** filter, which executes if there is an unhandled exception thrown somewhere in action method, starting with the authorization filters and ending with the execution of the result.</span></span> <span data-ttu-id="27f02-149">例外狀況篩選條件可以用於工作，例如記錄或顯示錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="27f02-149">Exception filters can be used for tasks such as logging or displaying an error page.</span></span>
> 
> <span data-ttu-id="27f02-150">如需篩選器提供者的詳細資訊，請造訪此 MSDN 連結: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx))。</span><span class="sxs-lookup"><span data-stu-id="27f02-150">For more information about Filters Providers please visit this MSDN link: ([https://msdn.microsoft.com/library/dd410209.aspx](https://msdn.microsoft.com/library/dd410209.aspx)) .</span></span>


<a id="AboutLoggingFeature"></a>

<a id="About_MVC_Music_Store_Application_logging_feature"></a>
#### <a name="about-mvc-music-store-application-logging-feature"></a><span data-ttu-id="27f02-151">有關 MVC 音樂市集應用程式記錄功能</span><span class="sxs-lookup"><span data-stu-id="27f02-151">About MVC Music Store Application logging feature</span></span>

<span data-ttu-id="27f02-152">此 Music Store 方案中有新的資料模型資料表的網站記錄**ActionLog**，使用下列欄位： 接收要求、 呼叫動作、 用戶端 IP 和時間戳記的控制器名稱。</span><span class="sxs-lookup"><span data-stu-id="27f02-152">This Music Store solution has a new data model table for site logging, **ActionLog**, with the following fields: Name of the controller that received a request, Called action, Client IP and Time stamp.</span></span>

<span data-ttu-id="27f02-153">![資料模型。ActionLog 資料表。](aspnet-mvc-4-custom-action-filters/_static/image1.png "資料模型。ActionLog 資料表。")</span><span class="sxs-lookup"><span data-stu-id="27f02-153">![Data model. ActionLog table.](aspnet-mvc-4-custom-action-filters/_static/image1.png "Data model. ActionLog table.")</span></span>

<span data-ttu-id="27f02-154">*資料模型-ActionLog 資料表*</span><span class="sxs-lookup"><span data-stu-id="27f02-154">*Data model - ActionLog table*</span></span>

<span data-ttu-id="27f02-155">解決方案提供動作記錄檔可以找到的 ASP.NET MVC 檢視**MvcMusicStores/檢視表/ActionLog**:</span><span class="sxs-lookup"><span data-stu-id="27f02-155">The solution provides an ASP.NET MVC View for the Action log that can be found at **MvcMusicStores/Views/ActionLog**:</span></span>

<span data-ttu-id="27f02-156">![動作記錄檔檢視](aspnet-mvc-4-custom-action-filters/_static/image2.png "動作記錄檔檢視")</span><span class="sxs-lookup"><span data-stu-id="27f02-156">![Action Log view](aspnet-mvc-4-custom-action-filters/_static/image2.png "Action Log view")</span></span>

<span data-ttu-id="27f02-157">*動作記錄檔檢視*</span><span class="sxs-lookup"><span data-stu-id="27f02-157">*Action Log view*</span></span>

<span data-ttu-id="27f02-158">與此提供結構的所有工作，將都著重於插斷控制器的要求及使用自訂篩選執行記錄中。</span><span class="sxs-lookup"><span data-stu-id="27f02-158">With this given structure, all the work will be focused on interrupting controller's request and performing the logging by using custom filtering.</span></span>

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_Custom_Filter_to_Catch_a_Controllers_Request"></a>
#### <a name="task-1---creating-a-custom-filter-to-catch-a-controllers-request"></a><span data-ttu-id="27f02-159">工作 1-建立自訂篩選器，以攔截控制器的要求</span><span class="sxs-lookup"><span data-stu-id="27f02-159">Task 1 - Creating a Custom Filter to Catch a Controller's Request</span></span>

<span data-ttu-id="27f02-160">在這項工作中，您將建立自訂篩選條件屬性類別將包含記錄邏輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-160">In this task you will create a custom filter attribute class that will contain the logging logic.</span></span> <span data-ttu-id="27f02-161">針對該目的，您將延伸 ASP.NET MVC **Actionfilterattribut**類別和實作的介面**IActionFilter**。</span><span class="sxs-lookup"><span data-stu-id="27f02-161">For that purpose you will extend ASP.NET MVC **ActionFilterAttribute** Class and implement the interface **IActionFilter**.</span></span>

> [!NOTE]
> <span data-ttu-id="27f02-162">**Actionfilterattribut**是所有屬性篩選條件的基底類別。</span><span class="sxs-lookup"><span data-stu-id="27f02-162">The **ActionFilterAttribute** is the base class for all the attribute filters.</span></span> <span data-ttu-id="27f02-163">它提供下列方法來執行特定邏輯之後，以及控制器動作執行之前：</span><span class="sxs-lookup"><span data-stu-id="27f02-163">It provides the following methods to execute a specific logic after and before controller action's execution:</span></span>
> 
> - <span data-ttu-id="27f02-164">**OnActionExecuting**(ActionExecutingContext filterContext): 此動作之前呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="27f02-164">**OnActionExecuting**(ActionExecutingContext filterContext): Just before the action method is called.</span></span>
> - <span data-ttu-id="27f02-165">**OnActionExecuted**(ActionExecutedContext filterContext): 呼叫動作方法之後之前的結果執行 （在之前檢視呈現）。</span><span class="sxs-lookup"><span data-stu-id="27f02-165">**OnActionExecuted**(ActionExecutedContext filterContext): After the action method is called and before the result is executed (before view render).</span></span>
> - <span data-ttu-id="27f02-166">**OnResultExecuting**(ResultExecutingContext filterContext): 之前的結果執行 （在之前檢視呈現）。</span><span class="sxs-lookup"><span data-stu-id="27f02-166">**OnResultExecuting**(ResultExecutingContext filterContext): Just before the result is executed (before view render).</span></span>
> - <span data-ttu-id="27f02-167">**OnResultExecuted**(ResultExecutedContext filterContext): （在呈現檢視） 之後的結果執行之後。</span><span class="sxs-lookup"><span data-stu-id="27f02-167">**OnResultExecuted**(ResultExecutedContext filterContext): After the result is executed (after the view is rendered).</span></span>
> 
> <span data-ttu-id="27f02-168">由衍生類別覆寫任何一種方法時，您可以執行您自己的篩選程式碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-168">By overriding any of these methods into a derived class, you can execute your own filtering code.</span></span>


1. <span data-ttu-id="27f02-169">開啟**開始**方案位於**\Source\Ex01-LoggingActions\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="27f02-169">Open the **Begin** solution located at **\Source\Ex01-LoggingActions\Begin** folder.</span></span>

   1. <span data-ttu-id="27f02-170">您必須下載某些遺漏的 NuGet 封裝，然後再繼續。</span><span class="sxs-lookup"><span data-stu-id="27f02-170">You will need to download some missing NuGet packages before you continue.</span></span> <span data-ttu-id="27f02-171">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="27f02-171">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
   2. <span data-ttu-id="27f02-172">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="27f02-172">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
   3. <span data-ttu-id="27f02-173">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="27f02-173">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

      > [!NOTE]
      > <span data-ttu-id="27f02-174">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="27f02-174">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27f02-175">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="27f02-175">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27f02-176">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="27f02-176">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
      > 
      > <span data-ttu-id="27f02-177">如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="27f02-177">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="27f02-178">加入新 C# 類別到**篩選**資料夾並將其命名*CustomActionFilter.cs*。</span><span class="sxs-lookup"><span data-stu-id="27f02-178">Add a new C# class into the **Filters** folder and name it *CustomActionFilter.cs*.</span></span> <span data-ttu-id="27f02-179">這個資料夾會儲存所有自訂篩選器。</span><span class="sxs-lookup"><span data-stu-id="27f02-179">This folder will store all the custom filters.</span></span>
3. <span data-ttu-id="27f02-180">開啟**CustomActionFilter.cs**並加入參考**System.Web.Mvc**和**MvcMusicStore.Models**命名空間：</span><span class="sxs-lookup"><span data-stu-id="27f02-180">Open **CustomActionFilter.cs** and add a reference to **System.Web.Mvc** and **MvcMusicStore.Models** namespaces:</span></span>

    <span data-ttu-id="27f02-181">(程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex1 CustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="27f02-181">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-CustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample1.cs)]
4. <span data-ttu-id="27f02-182">繼承**CustomActionFilter**類別從**Actionfilterattribut**然後進行**CustomActionFilter**類別實作**IActionFilter**介面。</span><span class="sxs-lookup"><span data-stu-id="27f02-182">Inherit the **CustomActionFilter** class from **ActionFilterAttribute** and then make **CustomActionFilter** class implement **IActionFilter** interface.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample2.cs)]
5. <span data-ttu-id="27f02-183">請**CustomActionFilter**類別覆寫方法**OnActionExecuting**並加入所需的邏輯記錄的篩選條件的執行。</span><span class="sxs-lookup"><span data-stu-id="27f02-183">Make **CustomActionFilter** class override the method **OnActionExecuting** and add the necessary logic to log the filter's execution.</span></span> <span data-ttu-id="27f02-184">若要這樣做，請將加入下列反白顯示的程式碼內**CustomActionFilter**類別。</span><span class="sxs-lookup"><span data-stu-id="27f02-184">To do this, add the following highlighted code within **CustomActionFilter** class.</span></span>

    <span data-ttu-id="27f02-185">(程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex1 LoggingActions*)</span><span class="sxs-lookup"><span data-stu-id="27f02-185">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex1-LoggingActions*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample3.cs#Highlight)]

    > [!NOTE]
    > <span data-ttu-id="27f02-186">**OnActionExecuting**的方法使用**Entity Framework**来加入新的 ActionLog 暫存器。</span><span class="sxs-lookup"><span data-stu-id="27f02-186">**OnActionExecuting** method is using **Entity Framework** to add a new ActionLog register.</span></span> <span data-ttu-id="27f02-187">它會建立並填入新的實體執行個體的內容資訊**filterContext**。</span><span class="sxs-lookup"><span data-stu-id="27f02-187">It creates and fills a new entity instance with the context information from **filterContext**.</span></span>
    > 
    > <span data-ttu-id="27f02-188">您可以深入了解**ControllerContext**類別在[msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx)。</span><span class="sxs-lookup"><span data-stu-id="27f02-188">You can read more about **ControllerContext** class at [msdn](https://msdn.microsoft.com/library/system.web.mvc.controllercontext.aspx).</span></span>

<a id="Ex1Task2"></a>

<a id="Task_2_-_Injecting_a_Code_Interceptor_into_the_Store_Controller_Class"></a>
#### <a name="task-2---injecting-a-code-interceptor-into-the-store-controller-class"></a><span data-ttu-id="27f02-189">工作 2-插入的程式碼攔截到存放區控制器類別</span><span class="sxs-lookup"><span data-stu-id="27f02-189">Task 2 - Injecting a Code Interceptor into the Store Controller Class</span></span>

<span data-ttu-id="27f02-190">在這項工作中，您會加入自訂篩選器插入所有控制器類別和控制器的動作會登入。</span><span class="sxs-lookup"><span data-stu-id="27f02-190">In this task you will add the custom filter by injecting it to all controller classes and controller actions that will be logged.</span></span> <span data-ttu-id="27f02-191">基於此練習中，目的存放區控制器類別會有記錄檔。</span><span class="sxs-lookup"><span data-stu-id="27f02-191">For the purpose of this exercise, the Store Controller class will have a log.</span></span>

<span data-ttu-id="27f02-192">此方法**OnActionExecuting**從**ActionLogFilterAttribute**呼叫插入的項目時要執行自訂篩選器。</span><span class="sxs-lookup"><span data-stu-id="27f02-192">The method **OnActionExecuting** from **ActionLogFilterAttribute** custom filter runs when an injected element is called.</span></span>

<span data-ttu-id="27f02-193">它也可攔截特定控制器方法。</span><span class="sxs-lookup"><span data-stu-id="27f02-193">It is also possible to intercept a specific controller method.</span></span>

1. <span data-ttu-id="27f02-194">開啟**StoreController**在**MvcMusicStore\Controllers**並加入參考**篩選**命名空間：</span><span class="sxs-lookup"><span data-stu-id="27f02-194">Open the **StoreController** at **MvcMusicStore\Controllers** and add a reference to the **Filters** namespace:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample4.cs)]
2. <span data-ttu-id="27f02-195">插入自訂篩選器**CustomActionFilter**到**StoreController**類別藉由新增 **[CustomActionFilter]** 類別宣告之前的屬性。</span><span class="sxs-lookup"><span data-stu-id="27f02-195">Inject the custom filter **CustomActionFilter** into **StoreController** class by adding **[CustomActionFilter]** attribute before the class declaration.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample5.cs)]

   > [!NOTE]
   > <span data-ttu-id="27f02-196">當篩選條件會插入控制器類別時，也插入它的動作。</span><span class="sxs-lookup"><span data-stu-id="27f02-196">When a filter is injected into a controller class, all its actions are also injected.</span></span> <span data-ttu-id="27f02-197">如果您想要將篩選套用到只針對一組動作，您必須將 **[CustomActionFilter]** 到其中的每一個：</span><span class="sxs-lookup"><span data-stu-id="27f02-197">If you would like to apply the filter only for a set of actions, you would have to inject **[CustomActionFilter]** to each one of them:</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample6.cs)]

<a id="Ex1Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a><span data-ttu-id="27f02-198">工作 3-執行應用程式</span><span class="sxs-lookup"><span data-stu-id="27f02-198">Task 3 - Running the Application</span></span>

<span data-ttu-id="27f02-199">在這項工作，您將測試正在記錄篩選器。</span><span class="sxs-lookup"><span data-stu-id="27f02-199">In this task, you will test that the logging filter is working.</span></span> <span data-ttu-id="27f02-200">您將會啟動應用程式，並瀏覽市集，則您將會檢查記錄的活動。</span><span class="sxs-lookup"><span data-stu-id="27f02-200">You will start the application and visit the store, and then you will check logged activities.</span></span>

1. <span data-ttu-id="27f02-201">按 **F5** 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="27f02-201">Press **F5** to run the application.</span></span>
2. <span data-ttu-id="27f02-202">瀏覽至 **/ActionLog**以查看記錄檔檢視的初始狀態：</span><span class="sxs-lookup"><span data-stu-id="27f02-202">Browse to **/ActionLog** to see log view initial state:</span></span>

    <span data-ttu-id="27f02-203">![記錄之前呼叫 」 活動的追蹤器狀態](aspnet-mvc-4-custom-action-filters/_static/image3.png "記錄之前呼叫 」 活動的追蹤器狀態")</span><span class="sxs-lookup"><span data-stu-id="27f02-203">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image3.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="27f02-204">*記錄之前呼叫 」 活動的追蹤器狀態*</span><span class="sxs-lookup"><span data-stu-id="27f02-204">*Log tracker status before page activity*</span></span>

   > [!NOTE]
   > <span data-ttu-id="27f02-205">根據預設，它會永遠顯示一個項目擷取現有的內容類型功能表時所產生。</span><span class="sxs-lookup"><span data-stu-id="27f02-205">By default, it will always show one item that is generated when retrieving the existing genres for the menu.</span></span>
   > 
   > <span data-ttu-id="27f02-206">為了簡單起見我們正在清理**ActionLog**資料表每次應用程式執行時，讓它只會顯示每個特定工作的驗證的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="27f02-206">For simplicity purposes we're cleaning up the **ActionLog** table each time the application runs so it will only show the logs of each particular task's verification.</span></span>
   > 
   > <span data-ttu-id="27f02-207">您可能需要移除下列程式碼從**工作階段\_啟動**方法 (在**Global.asax**類別)，以便儲存在存放區中執行的所有動作的歷程記錄控制站。</span><span class="sxs-lookup"><span data-stu-id="27f02-207">You might need to remove the following code from the **Session\_Start** method (in the **Global.asax** class), in order to save an historical log for all the actions executed within the Store Controller.</span></span>
   > 
   > [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample7.cs)]
3. <span data-ttu-id="27f02-208">按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-208">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
4. <span data-ttu-id="27f02-209">瀏覽至 **/ActionLog** ，而且如果記錄檔是空的按**F5**重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="27f02-209">Browse to **/ActionLog** and if the log is empty press **F5** to refresh the page.</span></span> <span data-ttu-id="27f02-210">追蹤您造訪的檢查：</span><span class="sxs-lookup"><span data-stu-id="27f02-210">Check that your visits were tracked:</span></span>

    <span data-ttu-id="27f02-211">![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image4.png "動作記錄檔與記錄活動")</span><span class="sxs-lookup"><span data-stu-id="27f02-211">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image4.png "Action log with activity logged")</span></span>

    <span data-ttu-id="27f02-212">*動作記錄檔與記錄活動*</span><span class="sxs-lookup"><span data-stu-id="27f02-212">*Action log with activity logged*</span></span>

<a id="Exercise2"></a>

<a id="Exercise_2_Managing_Multiple_Action_Filters"></a>
### <a name="exercise-2-managing-multiple-action-filters"></a><span data-ttu-id="27f02-213">練習 2： 管理多個動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="27f02-213">Exercise 2: Managing Multiple Action Filters</span></span>

<span data-ttu-id="27f02-214">在這個練習中您會將第二個的自訂動作篩選條件加入至 StoreController 類別，並定義特定的順序執行兩個篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-214">In this exercise you will add a second Custom Action Filter to the StoreController class and define the specific order in which both filters will be executed.</span></span> <span data-ttu-id="27f02-215">然後，您將會更新全域註冊篩選條件的程式碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-215">Then you will update the code to register the filter Globally.</span></span>

<span data-ttu-id="27f02-216">沒有定義的篩選條件執行順序時，請考慮不同的選項。</span><span class="sxs-lookup"><span data-stu-id="27f02-216">There are different options to take into account when defining the Filters' execution order.</span></span> <span data-ttu-id="27f02-217">例如，Order 屬性和篩選的範圍：</span><span class="sxs-lookup"><span data-stu-id="27f02-217">For example, the Order property and the Filters' scope:</span></span>

<span data-ttu-id="27f02-218">您可以定義**範圍**針對每個篩選條件，比方說，您無法限制範圍內執行的所有動作篩選條件**控制器都範圍**，和執行中的所有授權篩選條件**全域都範圍**.</span><span class="sxs-lookup"><span data-stu-id="27f02-218">You can define a **Scope** for each of the Filters, for example, you could scope all the Action Filters to run within the **Controller Scope**, and all Authorization Filters to run in **Global scope**.</span></span> <span data-ttu-id="27f02-219">範圍有已定義的執行順序。</span><span class="sxs-lookup"><span data-stu-id="27f02-219">The scopes have a defined execution order.</span></span>

<span data-ttu-id="27f02-220">此外，每個動作篩選條件具有順序屬性用來判斷篩選器的範圍中的執行順序。</span><span class="sxs-lookup"><span data-stu-id="27f02-220">Additionally, each action filter has an Order property which is used to determine the execution order in the scope of the filter.</span></span>

<span data-ttu-id="27f02-221">如需自訂動作篩選條件執行順序的詳細資訊，請瀏覽 MSDN 上的本文: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx))。</span><span class="sxs-lookup"><span data-stu-id="27f02-221">For more information about Custom Action Filters execution order, please visit this MSDN article: ([https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx](https://msdn.microsoft.com/library/dd381609(v=vs.98).aspx)).</span></span>

<a id="Ex2Task1"></a>

<a id="Task_1_Creating_a_new_Custom_Action_Filter"></a>
#### <a name="task-1-creating-a-new-custom-action-filter"></a><span data-ttu-id="27f02-222">工作 1： 建立新的自訂動作篩選條件</span><span class="sxs-lookup"><span data-stu-id="27f02-222">Task 1: Creating a new Custom Action Filter</span></span>

<span data-ttu-id="27f02-223">在這個工作中，您將建立新的自訂動作篩選條件，將插入 StoreController 類別中，了解如何管理篩選器的執行順序。</span><span class="sxs-lookup"><span data-stu-id="27f02-223">In this task, you will create a new Custom Action Filter to inject into the StoreController class, learning how to manage the execution order of the filters.</span></span>

1. <span data-ttu-id="27f02-224">開啟**開始**方案位於**\Source\Ex02-ManagingMultipleActionFilters\Begin**資料夾。</span><span class="sxs-lookup"><span data-stu-id="27f02-224">Open the **Begin** solution located at **\Source\Ex02-ManagingMultipleActionFilters\Begin** folder.</span></span> <span data-ttu-id="27f02-225">否則，您可能會繼續使用**結束**方案所完成的上一個練習中取得。</span><span class="sxs-lookup"><span data-stu-id="27f02-225">Otherwise, you might continue using the **End** solution obtained by completing the previous exercise.</span></span>

    1. <span data-ttu-id="27f02-226">如果您開啟提供**開始**方案，您必須下載某些遺漏的 NuGet 套件才能繼續。</span><span class="sxs-lookup"><span data-stu-id="27f02-226">If you opened the provided **Begin** solution, you will need to download some missing NuGet packages before continue.</span></span> <span data-ttu-id="27f02-227">若要這樣做，請按一下**專案**功能表，然後選取**管理 NuGet 封裝**。</span><span class="sxs-lookup"><span data-stu-id="27f02-227">To do this, click the **Project** menu and select **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="27f02-228">在**管理 NuGet 封裝**] 對話方塊中，按一下 [**還原**才能下載遺漏的封裝。</span><span class="sxs-lookup"><span data-stu-id="27f02-228">In the **Manage NuGet Packages** dialog, click **Restore** in order to download missing packages.</span></span>
    3. <span data-ttu-id="27f02-229">最後，按一下 建置方案**建置** | **建置方案**。</span><span class="sxs-lookup"><span data-stu-id="27f02-229">Finally, build the solution by clicking **Build** | **Build Solution**.</span></span>

        > [!NOTE]
        > <span data-ttu-id="27f02-230">使用 NuGet 的優點之一是您不需要在專案中，所有的程式庫的出貨減少專案大小。</span><span class="sxs-lookup"><span data-stu-id="27f02-230">One of the advantages of using NuGet is that you don't have to ship all the libraries in your project, reducing the project size.</span></span> <span data-ttu-id="27f02-231">NuGet 的強大工具，請藉由指定封裝版本在 Packages.config 檔案中，您將會成功下載所有必要的程式庫第一次您執行專案。</span><span class="sxs-lookup"><span data-stu-id="27f02-231">With NuGet Power Tools, by specifying the package versions in the Packages.config file, you will be able to download all the required libraries the first time you run the project.</span></span> <span data-ttu-id="27f02-232">這就是為什麼您必須從這個實驗室中開啟現有的方案後執行這些步驟。</span><span class="sxs-lookup"><span data-stu-id="27f02-232">This is why you will have to run these steps after you open an existing solution from this lab.</span></span>
        > 
        > <span data-ttu-id="27f02-233">如需詳細資訊，請參閱這篇文章： [ http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages ](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages)。</span><span class="sxs-lookup"><span data-stu-id="27f02-233">For more information, see this article: [http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages](http://docs.nuget.org/docs/workflows/using-nuget-without-committing-packages).</span></span>
2. <span data-ttu-id="27f02-234">加入新 C# 類別到**篩選**資料夾並將其命名*MyNewCustomActionFilter.cs*</span><span class="sxs-lookup"><span data-stu-id="27f02-234">Add a new C# class into the **Filters** folder and name it *MyNewCustomActionFilter.cs*</span></span>
3. <span data-ttu-id="27f02-235">開啟**MyNewCustomActionFilter.cs**並加入參考**System.Web.Mvc**和**MvcMusicStore.Models**命名空間：</span><span class="sxs-lookup"><span data-stu-id="27f02-235">Open **MyNewCustomActionFilter.cs** and add a reference to **System.Web.Mvc** and the **MvcMusicStore.Models** namespace:</span></span>

    <span data-ttu-id="27f02-236">(程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex2 MyNewCustomActionFilterNamespaces*)</span><span class="sxs-lookup"><span data-stu-id="27f02-236">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterNamespaces*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample8.cs)]
4. <span data-ttu-id="27f02-237">預設類別宣告替換成下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-237">Replace the default class declaration with the following code.</span></span>

    <span data-ttu-id="27f02-238">(程式碼片段- *ASP.NET MVC 4 自訂動作篩選條件-Ex2 MyNewCustomActionFilterClass*)</span><span class="sxs-lookup"><span data-stu-id="27f02-238">(Code Snippet - *ASP.NET MVC 4 Custom Action Filters - Ex2-MyNewCustomActionFilterClass*)</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample9.cs)]

    > [!NOTE]
    > <span data-ttu-id="27f02-239">此自訂動作篩選條件是您在上一個練習中建立一個幾乎相同的。</span><span class="sxs-lookup"><span data-stu-id="27f02-239">This Custom Action Filter is almost the same than the one you created in the previous exercise.</span></span> <span data-ttu-id="27f02-240">主要差異在於它有*&quot;記錄由&quot;* 更新這個新類別的名稱來找出根據篩選器屬性登錄記錄。</span><span class="sxs-lookup"><span data-stu-id="27f02-240">The main difference is that it has the *&quot;Logged By&quot;* attribute updated with this new class' name to identify wich filter registered the log.</span></span>

<a id="Ex2Task2"></a>

<a id="Task_2_Injecting_a_new_Code_Interceptor_into_the_StoreController_Class"></a>
#### <a name="task-2-injecting-a-new-code-interceptor-into-the-storecontroller-class"></a><span data-ttu-id="27f02-241">工作 2： 將新的程式碼攔截器插入 StoreController 類別</span><span class="sxs-lookup"><span data-stu-id="27f02-241">Task 2: Injecting a new Code Interceptor into the StoreController Class</span></span>

<span data-ttu-id="27f02-242">在這項工作，您會將新的自訂篩選器加入至 StoreController 類別，並執行方案來驗證這兩種篩選器一起運作。</span><span class="sxs-lookup"><span data-stu-id="27f02-242">In this task, you will add a new custom filter into the StoreController Class and run the solution to verify how both filters work together.</span></span>

1. <span data-ttu-id="27f02-243">開啟**StoreController**類別位於**MvcMusicStore\Controllers** ，以及將新的自訂篩選器**MyNewCustomActionFilter**到**StoreController**之類類別會顯示下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-243">Open the **StoreController** class located at **MvcMusicStore\Controllers** and inject the new custom filter **MyNewCustomActionFilter** into **StoreController** class like is shown in the following code.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample10.cs)]
2. <span data-ttu-id="27f02-244">現在，執行應用程式，以便查看這些兩個自訂動作篩選條件的正常運作。</span><span class="sxs-lookup"><span data-stu-id="27f02-244">Now, run the application in order to see how these two Custom Action Filters work.</span></span> <span data-ttu-id="27f02-245">若要這樣做，請按**F5**並等待應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="27f02-245">To do this, press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="27f02-246">瀏覽至 **/ActionLog**以查看記錄檔檢視的初始狀態。</span><span class="sxs-lookup"><span data-stu-id="27f02-246">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="27f02-247">![記錄之前呼叫 」 活動的追蹤器狀態](aspnet-mvc-4-custom-action-filters/_static/image5.png "記錄之前呼叫 」 活動的追蹤器狀態")</span><span class="sxs-lookup"><span data-stu-id="27f02-247">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image5.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="27f02-248">*記錄之前呼叫 」 活動的追蹤器狀態*</span><span class="sxs-lookup"><span data-stu-id="27f02-248">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="27f02-249">按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-249">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="27f02-250">請檢查此時間;追蹤您造訪兩次： 一旦您在每個自訂動作篩選條件加入**StorageController**類別。</span><span class="sxs-lookup"><span data-stu-id="27f02-250">Check that this time; your visits were tracked twice: once for each of the Custom Action Filters you added in the **StorageController** class.</span></span>

    <span data-ttu-id="27f02-251">![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image6.png "動作記錄檔與記錄活動")</span><span class="sxs-lookup"><span data-stu-id="27f02-251">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image6.png "Action log with activity logged")</span></span>

    <span data-ttu-id="27f02-252">*動作記錄檔與記錄活動*</span><span class="sxs-lookup"><span data-stu-id="27f02-252">*Action log with activity logged*</span></span>
6. <span data-ttu-id="27f02-253">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="27f02-253">Close the browser.</span></span>

<a id="Ex2Task3"></a>

<a id="Task_3_Managing_Filter_Ordering"></a>
#### <a name="task-3-managing-filter-ordering"></a><span data-ttu-id="27f02-254">工作 3： 管理篩選器順序</span><span class="sxs-lookup"><span data-stu-id="27f02-254">Task 3: Managing Filter Ordering</span></span>

<span data-ttu-id="27f02-255">在這項工作，您將學習如何管理使用順序屬性設定的篩選條件執行順序。</span><span class="sxs-lookup"><span data-stu-id="27f02-255">In this task, you will learn how to manage the filters' execution order by using the Order propery.</span></span>

1. <span data-ttu-id="27f02-256">開啟**StoreController**類別位於**MvcMusicStore\Controllers**並指定**順序**喜歡這兩個篩選中的屬性如下所示。</span><span class="sxs-lookup"><span data-stu-id="27f02-256">Open the **StoreController** class located at **MvcMusicStore\Controllers** and specify the **Order** property in both filters like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample11.cs)]
2. <span data-ttu-id="27f02-257">現在，請確認篩選其順序屬性的值而定的執行方式。</span><span class="sxs-lookup"><span data-stu-id="27f02-257">Now, verify how the filters are executed depending on its Order property's value.</span></span> <span data-ttu-id="27f02-258">您會發現具有的最小的順序值的篩選條件 (**CustomActionFilter**) 執行的第一個。</span><span class="sxs-lookup"><span data-stu-id="27f02-258">You will find that the filter with the smallest Order value (**CustomActionFilter**) is the first one that is executed.</span></span> <span data-ttu-id="27f02-259">按**F5**並等待應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="27f02-259">Press **F5** and wait until the application starts.</span></span>
3. <span data-ttu-id="27f02-260">瀏覽至 **/ActionLog**以查看記錄檔檢視的初始狀態。</span><span class="sxs-lookup"><span data-stu-id="27f02-260">Browse to **/ActionLog** to see log view initial state.</span></span>

    <span data-ttu-id="27f02-261">![記錄之前呼叫 」 活動的追蹤器狀態](aspnet-mvc-4-custom-action-filters/_static/image7.png "記錄之前呼叫 」 活動的追蹤器狀態")</span><span class="sxs-lookup"><span data-stu-id="27f02-261">![Log tracker status before page activity](aspnet-mvc-4-custom-action-filters/_static/image7.png "Log tracker status before page activity")</span></span>

    <span data-ttu-id="27f02-262">*記錄之前呼叫 」 活動的追蹤器狀態*</span><span class="sxs-lookup"><span data-stu-id="27f02-262">*Log tracker status before page activity*</span></span>
4. <span data-ttu-id="27f02-263">按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-263">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
5. <span data-ttu-id="27f02-264">核取，此時，您瀏覽追蹤依篩選條件的順序值： **CustomActionFilter**記錄檔的第一次。</span><span class="sxs-lookup"><span data-stu-id="27f02-264">Check that this time, your visits were tracked ordered by the filters' Order value: **CustomActionFilter** logs' first.</span></span>

    <span data-ttu-id="27f02-265">![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image8.png "動作記錄檔與記錄活動")</span><span class="sxs-lookup"><span data-stu-id="27f02-265">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image8.png "Action log with activity logged")</span></span>

    <span data-ttu-id="27f02-266">*動作記錄檔與記錄活動*</span><span class="sxs-lookup"><span data-stu-id="27f02-266">*Action log with activity logged*</span></span>
6. <span data-ttu-id="27f02-267">現在，您將更新的篩選條件的順序值，並確認變更的記錄順序的方式。</span><span class="sxs-lookup"><span data-stu-id="27f02-267">Now, you will update the Filters' order value and verify how the logging order changes.</span></span> <span data-ttu-id="27f02-268">在**StoreController**類別中，更新的篩選條件的順序值，例如如下所示。</span><span class="sxs-lookup"><span data-stu-id="27f02-268">In the **StoreController** class, update the Filters' Order value like shown below.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample12.cs)]
7. <span data-ttu-id="27f02-269">按一次執行應用程式**F5**。</span><span class="sxs-lookup"><span data-stu-id="27f02-269">Run the application again by pressing **F5**.</span></span>
8. <span data-ttu-id="27f02-270">按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-270">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
9. <span data-ttu-id="27f02-271">請檢查這個時間，記錄檔所建立的**MyNewCustomActionFilter**篩選器會先出現。</span><span class="sxs-lookup"><span data-stu-id="27f02-271">Check that this time, the logs created by **MyNewCustomActionFilter** filter appears first.</span></span>

    <span data-ttu-id="27f02-272">![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image9.png "動作記錄檔與記錄活動")</span><span class="sxs-lookup"><span data-stu-id="27f02-272">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image9.png "Action log with activity logged")</span></span>

    <span data-ttu-id="27f02-273">*動作記錄檔與記錄活動*</span><span class="sxs-lookup"><span data-stu-id="27f02-273">*Action log with activity logged*</span></span>

<a id="Ex2Task4"></a>

<a id="Task_4_Registering_Filters_Globally"></a>
#### <a name="task-4-registering-filters-globally"></a><span data-ttu-id="27f02-274">工作 4： 註冊全域篩選</span><span class="sxs-lookup"><span data-stu-id="27f02-274">Task 4: Registering Filters Globally</span></span>

<span data-ttu-id="27f02-275">在這項工作，您將更新方案以註冊新的篩選器 (**MyNewCustomActionFilter**) 做為全域的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-275">In this task, you will update the solution to register the new filter (**MyNewCustomActionFilter**) as a global filter.</span></span> <span data-ttu-id="27f02-276">如此一來，就會觸發的所有動作執行應用程式中並不只是在 StoreController 項目與前一項工作。</span><span class="sxs-lookup"><span data-stu-id="27f02-276">By doing this, it will be triggered by all the actions perfomed in the application and not only in the StoreController ones as in the previous task.</span></span>

1. <span data-ttu-id="27f02-277">在**StoreController**類別中，移除 **[MyNewCustomActionFilter]** 屬性和 [順序] 屬性從 **[CustomActionFilter]**。</span><span class="sxs-lookup"><span data-stu-id="27f02-277">In **StoreController** class, remove **[MyNewCustomActionFilter]** attribute and the order property from **[CustomActionFilter]**.</span></span> <span data-ttu-id="27f02-278">它看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="27f02-278">It should look like the following:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample13.cs)]
2. <span data-ttu-id="27f02-279">開啟**Global.asax**檔案，並尋找**應用程式\_啟動**方法。</span><span class="sxs-lookup"><span data-stu-id="27f02-279">Open **Global.asax** file and locate the **Application\_Start** method.</span></span> <span data-ttu-id="27f02-280">請注意，每次應用程式啟動時，它藉由呼叫註冊全域篩選**RegisterGlobalFilters**方法內**FilterConfig**類別。</span><span class="sxs-lookup"><span data-stu-id="27f02-280">Notice that each time the application starts it is registering the global filters by calling **RegisterGlobalFilters** method within **FilterConfig** class.</span></span>

    <span data-ttu-id="27f02-281">![在 Global.asax 中註冊全域篩選](aspnet-mvc-4-custom-action-filters/_static/image10.png "在 Global.asax 中註冊全域篩選")</span><span class="sxs-lookup"><span data-stu-id="27f02-281">![Registering Global Filters in Global.asax](aspnet-mvc-4-custom-action-filters/_static/image10.png "Registering Global Filters in Global.asax")</span></span>

    <span data-ttu-id="27f02-282">*在 Global.asax 中註冊全域篩選*</span><span class="sxs-lookup"><span data-stu-id="27f02-282">*Registering Global Filters in Global.asax*</span></span>
3. <span data-ttu-id="27f02-283">開啟**FilterConfig.cs**檔案內**應用程式\_啟動**資料夾。</span><span class="sxs-lookup"><span data-stu-id="27f02-283">Open **FilterConfig.cs** file within **App\_Start** folder.</span></span>
4. <span data-ttu-id="27f02-284">將參考加入使用 system.web.mvc 的參考。使用 MvcMusicStore.Filters;命名空間。</span><span class="sxs-lookup"><span data-stu-id="27f02-284">Add a reference to using System.Web.Mvc; using MvcMusicStore.Filters; namespace.</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample14.cs)]
5. <span data-ttu-id="27f02-285">更新**RegisterGlobalFilters**方法加入您的自訂篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-285">Update **RegisterGlobalFilters** method adding your custom filter.</span></span> <span data-ttu-id="27f02-286">若要這樣做，請將反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="27f02-286">To do this, add the highlighted code:</span></span>

    [!code-csharp[Main](aspnet-mvc-4-custom-action-filters/samples/sample15.cs)]
6. <span data-ttu-id="27f02-287">執行應用程式按**F5**。</span><span class="sxs-lookup"><span data-stu-id="27f02-287">Run the application by pressing **F5**.</span></span>
7. <span data-ttu-id="27f02-288">按一下其中一個**內容類型**從功能表，並執行某些動作，例如瀏覽可用的專輯。</span><span class="sxs-lookup"><span data-stu-id="27f02-288">Click one of the **Genres** from the menu and perform some actions there, like browsing an available album.</span></span>
8. <span data-ttu-id="27f02-289">核取現在 **[MyNewCustomActionFilter]** 媵檔晻 HomeController 和 ActionLogController 太。</span><span class="sxs-lookup"><span data-stu-id="27f02-289">Check that now **[MyNewCustomActionFilter]** is being injected in HomeController and ActionLogController too.</span></span>

    <span data-ttu-id="27f02-290">![動作記錄檔與記錄活動](aspnet-mvc-4-custom-action-filters/_static/image11.png "動作記錄檔與記錄活動")</span><span class="sxs-lookup"><span data-stu-id="27f02-290">![Action log with activity logged](aspnet-mvc-4-custom-action-filters/_static/image11.png "Action log with activity logged")</span></span>

    <span data-ttu-id="27f02-291">*動作記錄檔記錄的全域活動*</span><span class="sxs-lookup"><span data-stu-id="27f02-291">*Action log with global activity logged*</span></span>

> [!NOTE]
> <span data-ttu-id="27f02-292">此外，您可以部署此應用程式以 Windows Azure Web Sites 下列[附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy](#AppendixB)。</span><span class="sxs-lookup"><span data-stu-id="27f02-292">Additionally, you can deploy this application to Windows Azure Web Sites following [Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy](#AppendixB).</span></span>


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a><span data-ttu-id="27f02-293">總結</span><span class="sxs-lookup"><span data-stu-id="27f02-293">Summary</span></span>

<span data-ttu-id="27f02-294">藉由完成這個實際操作實驗室中，您已經學會如何擴充動作篩選條件執行自訂動作。</span><span class="sxs-lookup"><span data-stu-id="27f02-294">By completing this Hands-On Lab you have learned how to extend an action filter to execute custom actions.</span></span> <span data-ttu-id="27f02-295">您也已經學會如何插入頁面控制器的任何篩選條件。</span><span class="sxs-lookup"><span data-stu-id="27f02-295">You have also learned how to inject any filter to your page controllers.</span></span> <span data-ttu-id="27f02-296">使用下列概念：</span><span class="sxs-lookup"><span data-stu-id="27f02-296">The following concepts were used:</span></span>

- <span data-ttu-id="27f02-297">如何建立自訂動作篩選條件與 ASP.NET MVC Actionfilterattribut 類別</span><span class="sxs-lookup"><span data-stu-id="27f02-297">How to create Custom Action filters with the ASP.NET MVC ActionFilterAttribute class</span></span>
- <span data-ttu-id="27f02-298">如何將篩選器插入至 ASP.NET MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="27f02-298">How to inject filters into ASP.NET MVC controllers</span></span>
- <span data-ttu-id="27f02-299">如何管理篩選器順序使用 Order 屬性</span><span class="sxs-lookup"><span data-stu-id="27f02-299">How to manage filter ordering using the Order property</span></span>
- <span data-ttu-id="27f02-300">如何註冊全域篩選</span><span class="sxs-lookup"><span data-stu-id="27f02-300">How to register filters globally</span></span>

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a><span data-ttu-id="27f02-301">附錄 a： 安裝 Visual Studio Express 2012 for Web</span><span class="sxs-lookup"><span data-stu-id="27f02-301">Appendix A: Installing Visual Studio Express 2012 for Web</span></span>

<span data-ttu-id="27f02-302">您可以安裝**Microsoft Visual Studio Express 2012 for Web**或另一個&quot;Express&quot;版本使用**[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="27f02-302">You can install **Microsoft Visual Studio Express 2012 for Web** or another &quot;Express&quot; version using the **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**.</span></span> <span data-ttu-id="27f02-303">下列指示將引導您逐步完成安裝所需*Visual studio Express 2012 for Web*使用*Microsoft Web Platform Installer*。</span><span class="sxs-lookup"><span data-stu-id="27f02-303">The following instructions guide you through the steps required to install *Visual studio Express 2012 for Web* using *Microsoft Web Platform Installer*.</span></span>

1. <span data-ttu-id="27f02-304">移至[ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169)。</span><span class="sxs-lookup"><span data-stu-id="27f02-304">Go to [[https://go.microsoft.com/? linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169).</span></span> <span data-ttu-id="27f02-305">或者，如果您已安裝 Web Platform Installer，您可以開啟它，並搜尋產品&quot; <em>Visual Studio Express 2012 for Web 與 Windows Azure SDK</em>&quot;。</span><span class="sxs-lookup"><span data-stu-id="27f02-305">Alternatively, if you already have installed Web Platform Installer, you can open it and search for the product &quot;<em>Visual Studio Express 2012 for Web with Windows Azure SDK</em>&quot;.</span></span>
2. <span data-ttu-id="27f02-306">按一下**立即安裝**。</span><span class="sxs-lookup"><span data-stu-id="27f02-306">Click on **Install Now**.</span></span> <span data-ttu-id="27f02-307">如果您不需要**Web Platform Installer**您會重新導向至下載並安裝第一次。</span><span class="sxs-lookup"><span data-stu-id="27f02-307">If you do not have **Web Platform Installer** you will be redirected to download and install it first.</span></span>
3. <span data-ttu-id="27f02-308">一次**Web Platform Installer**開啟時，按一下 **安裝**，啟動安裝程式。</span><span class="sxs-lookup"><span data-stu-id="27f02-308">Once **Web Platform Installer** is open, click **Install** to start the setup.</span></span>

    <span data-ttu-id="27f02-309">![安裝 Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "安裝 Visual Studio Express")</span><span class="sxs-lookup"><span data-stu-id="27f02-309">![Install Visual Studio Express](aspnet-mvc-4-custom-action-filters/_static/image12.png "Install Visual Studio Express")</span></span>

    <span data-ttu-id="27f02-310">*安裝 Visual Studio Express*</span><span class="sxs-lookup"><span data-stu-id="27f02-310">*Install Visual Studio Express*</span></span>
4. <span data-ttu-id="27f02-311">閱讀所有產品的授權和詞彙，然後按一下**我接受**才能繼續。</span><span class="sxs-lookup"><span data-stu-id="27f02-311">Read all the products' licenses and terms and click **I Accept** to continue.</span></span>

    ![接受授權條款](aspnet-mvc-4-custom-action-filters/_static/image13.png)

    <span data-ttu-id="27f02-313">*接受授權條款*</span><span class="sxs-lookup"><span data-stu-id="27f02-313">*Accepting the license terms*</span></span>
5. <span data-ttu-id="27f02-314">等待直到完成下載和安裝程序。</span><span class="sxs-lookup"><span data-stu-id="27f02-314">Wait until the downloading and installation process completes.</span></span>

    ![安裝進度](aspnet-mvc-4-custom-action-filters/_static/image14.png)

    <span data-ttu-id="27f02-316">*安裝進度*</span><span class="sxs-lookup"><span data-stu-id="27f02-316">*Installation progress*</span></span>
6. <span data-ttu-id="27f02-317">當安裝完成時，按一下**完成**。</span><span class="sxs-lookup"><span data-stu-id="27f02-317">When the installation completes, click **Finish**.</span></span>

    ![安裝已完成](aspnet-mvc-4-custom-action-filters/_static/image15.png)

    <span data-ttu-id="27f02-319">*安裝已完成*</span><span class="sxs-lookup"><span data-stu-id="27f02-319">*Installation completed*</span></span>
7. <span data-ttu-id="27f02-320">按一下**結束**關閉 Web Platform Installer。</span><span class="sxs-lookup"><span data-stu-id="27f02-320">Click **Exit** to close Web Platform Installer.</span></span>
8. <span data-ttu-id="27f02-321">若要開啟 Visual Studio Express for Web，請移至**啟動**畫面上，並開始書寫&quot; **VS Express**&quot;，然後按一下  **VS Express for Web**並排顯示。</span><span class="sxs-lookup"><span data-stu-id="27f02-321">To open Visual Studio Express for Web, go to the **Start** screen and start writing &quot;**VS Express**&quot;, then click on the **VS Express for Web** tile.</span></span>

    ![VS Express for Web 方塊](aspnet-mvc-4-custom-action-filters/_static/image16.png)

    <span data-ttu-id="27f02-323">*VS Express for Web 方塊*</span><span class="sxs-lookup"><span data-stu-id="27f02-323">*VS Express for Web tile*</span></span>

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="27f02-324">附錄 b： 發佈 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="27f02-324">Appendix B: Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

<span data-ttu-id="27f02-325">此附錄將說明如何從 Windows Azure 管理入口網站建立新的網站和發行您取得依照實驗室中，利用 Web Deploy 發行功能的 Windows Azure 所提供的應用程式。</span><span class="sxs-lookup"><span data-stu-id="27f02-325">This appendix will show you how to create a new web site from the Windows Azure Management Portal and publish the application you obtained by following the lab, taking advantage of the Web Deploy publishing feature provided by Windows Azure.</span></span>

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a><span data-ttu-id="27f02-326">工作 1-建立新的網站，從 Windows Azure 入口網站</span><span class="sxs-lookup"><span data-stu-id="27f02-326">Task 1 - Creating a New Web Site from the Windows Azure Portal</span></span>

1. <span data-ttu-id="27f02-327">移至[Windows Azure 管理入口網站](https://manage.windowsazure.com/)並使用與您訂用帳戶相關聯的 Microsoft 認證登入。</span><span class="sxs-lookup"><span data-stu-id="27f02-327">Go to the [Windows Azure Management Portal](https://manage.windowsazure.com/) and sign in using the Microsoft credentials associated with your subscription.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27f02-328">與 Windows Azure 中，您可以免費裝載 10 個 ASP.NET 網站並再擴充隨流量成長。</span><span class="sxs-lookup"><span data-stu-id="27f02-328">With Windows Azure you can host 10 ASP.NET Web Sites for free and then scale as your traffic grows.</span></span> <span data-ttu-id="27f02-329">您可以註冊[這裡](http://aka.ms/aspnet-hol-azure)。</span><span class="sxs-lookup"><span data-stu-id="27f02-329">You can sign up [here](http://aka.ms/aspnet-hol-azure).</span></span>

    <span data-ttu-id="27f02-330">![登入 Windows Azure 入口網站](aspnet-mvc-4-custom-action-filters/_static/image17.png "登入 Windows Azure 入口網站")</span><span class="sxs-lookup"><span data-stu-id="27f02-330">![Log on to Windows Azure portal](aspnet-mvc-4-custom-action-filters/_static/image17.png "Log on to Windows Azure portal")</span></span>

    <span data-ttu-id="27f02-331">*登入 Windows Azure 管理入口網站*</span><span class="sxs-lookup"><span data-stu-id="27f02-331">*Log on to Windows Azure Management Portal*</span></span>
2. <span data-ttu-id="27f02-332">按一下**新增**命令列上。</span><span class="sxs-lookup"><span data-stu-id="27f02-332">Click **New** on the command bar.</span></span>

    <span data-ttu-id="27f02-333">![建立新的網站](aspnet-mvc-4-custom-action-filters/_static/image18.png "建立新的網站")</span><span class="sxs-lookup"><span data-stu-id="27f02-333">![Creating a new Web Site](aspnet-mvc-4-custom-action-filters/_static/image18.png "Creating a new Web Site")</span></span>

    <span data-ttu-id="27f02-334">*建立新的網站*</span><span class="sxs-lookup"><span data-stu-id="27f02-334">*Creating a new Web Site*</span></span>
3. <span data-ttu-id="27f02-335">按一下**計算** | **網站**。</span><span class="sxs-lookup"><span data-stu-id="27f02-335">Click **Compute** | **Web Site**.</span></span> <span data-ttu-id="27f02-336">然後選取**快速建立**選項。</span><span class="sxs-lookup"><span data-stu-id="27f02-336">Then select **Quick Create** option.</span></span> <span data-ttu-id="27f02-337">新的網站上提供可用的 URL，然後按一下**建立網站**。</span><span class="sxs-lookup"><span data-stu-id="27f02-337">Provide an available URL for the new web site and click **Create Web Site**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27f02-338">Windows Azure 網站是您可以控制和管理在雲端中執行的 web 應用程式的主機。</span><span class="sxs-lookup"><span data-stu-id="27f02-338">A Windows Azure Web Site is the host for a web application running in the cloud that you can control and manage.</span></span> <span data-ttu-id="27f02-339">快速建立 選項可讓您部署已完成的 web 應用程式至 Windows Azure 網站從入口網站外部。</span><span class="sxs-lookup"><span data-stu-id="27f02-339">The Quick Create option allows you to deploy a completed web application to the Windows Azure Web Site from outside the portal.</span></span> <span data-ttu-id="27f02-340">它不包含設定資料庫的步驟。</span><span class="sxs-lookup"><span data-stu-id="27f02-340">It does not include steps for setting up a database.</span></span>

    <span data-ttu-id="27f02-341">![建立新的網站使用 快速建立](aspnet-mvc-4-custom-action-filters/_static/image19.png "建立新的網站使用 快速建立")</span><span class="sxs-lookup"><span data-stu-id="27f02-341">![Creating a new Web Site using Quick Create](aspnet-mvc-4-custom-action-filters/_static/image19.png "Creating a new Web Site using Quick Create")</span></span>

    <span data-ttu-id="27f02-342">*建立新的網站使用 快速建立*</span><span class="sxs-lookup"><span data-stu-id="27f02-342">*Creating a new Web Site using Quick Create*</span></span>
4. <span data-ttu-id="27f02-343">等候新**網站**建立。</span><span class="sxs-lookup"><span data-stu-id="27f02-343">Wait until the new **Web Site** is created.</span></span>
5. <span data-ttu-id="27f02-344">建立網站之後按一下底下的連結**URL**資料行。</span><span class="sxs-lookup"><span data-stu-id="27f02-344">Once the Web Site is created click the link under the **URL** column.</span></span> <span data-ttu-id="27f02-345">請檢查新的網站運作。</span><span class="sxs-lookup"><span data-stu-id="27f02-345">Check that the new Web Site is working.</span></span>

    <span data-ttu-id="27f02-346">![瀏覽至新的網站](aspnet-mvc-4-custom-action-filters/_static/image20.png "瀏覽至新的網站")</span><span class="sxs-lookup"><span data-stu-id="27f02-346">![Browsing to the new web site](aspnet-mvc-4-custom-action-filters/_static/image20.png "Browsing to the new web site")</span></span>

    <span data-ttu-id="27f02-347">*瀏覽至新的網站*</span><span class="sxs-lookup"><span data-stu-id="27f02-347">*Browsing to the new web site*</span></span>

    <span data-ttu-id="27f02-348">![執行網站](aspnet-mvc-4-custom-action-filters/_static/image21.png "執行的網站")</span><span class="sxs-lookup"><span data-stu-id="27f02-348">![Web site running](aspnet-mvc-4-custom-action-filters/_static/image21.png "Web site running")</span></span>

    <span data-ttu-id="27f02-349">*執行的網站*</span><span class="sxs-lookup"><span data-stu-id="27f02-349">*Web site running*</span></span>
6. <span data-ttu-id="27f02-350">返回入口網站並按一下底下的網站名稱**名稱**資料行會顯示 [管理] 頁面。</span><span class="sxs-lookup"><span data-stu-id="27f02-350">Go back to the portal and click the name of the web site under the **Name** column to display the management pages.</span></span>

    <span data-ttu-id="27f02-351">![開啟網站管理頁面](aspnet-mvc-4-custom-action-filters/_static/image22.png "開啟網站管理頁面")</span><span class="sxs-lookup"><span data-stu-id="27f02-351">![Opening the web site management pages](aspnet-mvc-4-custom-action-filters/_static/image22.png "Opening the web site management pages")</span></span>

    <span data-ttu-id="27f02-352">*開啟網站管理頁面*</span><span class="sxs-lookup"><span data-stu-id="27f02-352">*Opening the Web Site management pages*</span></span>
7. <span data-ttu-id="27f02-353">在**儀表板**頁面的 **快速概覽**區段中，按一下**下載發行設定檔**連結。</span><span class="sxs-lookup"><span data-stu-id="27f02-353">In the **Dashboard** page, under the **quick glance** section, click the **Download publish profile** link.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27f02-354">*發行設定檔*包含所有 web 應用程式發行至 Windows Azure 網站的每個已啟用的發行方法所需的資訊。</span><span class="sxs-lookup"><span data-stu-id="27f02-354">The *publish profile* contains all of the information required to publish a web application to a Windows Azure website for each enabled publication method.</span></span> <span data-ttu-id="27f02-355">發行設定檔包含 Url、 使用者認證和資料庫連接，並對每個端點已啟用的發行集的方法進行驗證所需的字串。</span><span class="sxs-lookup"><span data-stu-id="27f02-355">The publish profile contains the URLs, user credentials and database strings required to connect to and authenticate against each of the endpoints for which a publication method is enabled.</span></span> <span data-ttu-id="27f02-356">**Microsoft WebMatrix 2**， **Microsoft Visual Studio Express for Web**和**Microsoft Visual Studio 2012**支援讀取發行設定檔自動將這些程式的組態web 應用程式發行至 Windows Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="27f02-356">**Microsoft WebMatrix 2**, **Microsoft Visual Studio Express for Web** and **Microsoft Visual Studio 2012** support reading publish profiles to automate configuration of these programs for publishing web applications to Windows Azure websites.</span></span>

    <span data-ttu-id="27f02-357">![下載網站發行設定檔](aspnet-mvc-4-custom-action-filters/_static/image23.png "下載網站發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="27f02-357">![Downloading the web site publish profile](aspnet-mvc-4-custom-action-filters/_static/image23.png "Downloading the web site publish profile")</span></span>

    <span data-ttu-id="27f02-358">*下載網站發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="27f02-358">*Downloading the Web Site publish profile*</span></span>
8. <span data-ttu-id="27f02-359">下載發行設定檔至已知位置。</span><span class="sxs-lookup"><span data-stu-id="27f02-359">Download the publish profile file to a known location.</span></span> <span data-ttu-id="27f02-360">進一步在這個練習中，您會看到如何使用這個檔案從 Visual Studio 將 web 應用程式至 Windows Azure 網站發行。</span><span class="sxs-lookup"><span data-stu-id="27f02-360">Further in this exercise you will see how to use this file to publish a web application to a Windows Azure Web Sites from Visual Studio.</span></span>

    <span data-ttu-id="27f02-361">![儲存發行設定檔](aspnet-mvc-4-custom-action-filters/_static/image24.png "儲存發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="27f02-361">![Saving the publish profile file](aspnet-mvc-4-custom-action-filters/_static/image24.png "Saving the publish profile")</span></span>

    <span data-ttu-id="27f02-362">*儲存發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="27f02-362">*Saving the publish profile file*</span></span>

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a><span data-ttu-id="27f02-363">工作 2-設定資料庫伺服器</span><span class="sxs-lookup"><span data-stu-id="27f02-363">Task 2 - Configuring the Database Server</span></span>

<span data-ttu-id="27f02-364">如果您的應用程式會使用 SQL Server 資料庫，您必須建立 SQL Database 伺服器。</span><span class="sxs-lookup"><span data-stu-id="27f02-364">If your application makes use of SQL Server databases you will need to create a SQL Database server.</span></span> <span data-ttu-id="27f02-365">如果您想要部署簡單的應用程式不會使用 SQL Server，您可能會略過這項工作。</span><span class="sxs-lookup"><span data-stu-id="27f02-365">If you want to deploy a simple application that does not use SQL Server you might skip this task.</span></span>

1. <span data-ttu-id="27f02-366">您將需要 SQL Database 伺服器來儲存應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="27f02-366">You will need a SQL Database server for storing the application database.</span></span> <span data-ttu-id="27f02-367">您可以從您的訂用帳戶，在 Windows Azure 管理入口網站中檢視 SQL Database 伺服器**Sql 資料庫** | **伺服器** | **伺服器儀表板**。</span><span class="sxs-lookup"><span data-stu-id="27f02-367">You can view the SQL Database servers from your subscription in the Windows Azure Management portal at **Sql Databases** | **Servers** | **Server's Dashboard**.</span></span> <span data-ttu-id="27f02-368">如果您沒有建立伺服器，您可以建立一個使用**新增**命令列上的按鈕。</span><span class="sxs-lookup"><span data-stu-id="27f02-368">If you do not have a server created, you can create one using the **Add** button on the command bar.</span></span> <span data-ttu-id="27f02-369">請注意**伺服器名稱和 URL、 系統管理員身分登入名稱和密碼**，因為您將在下一個工作中使用它們。</span><span class="sxs-lookup"><span data-stu-id="27f02-369">Take note of the **server name and URL, administrator login name and password**, as you will use them in the next tasks.</span></span> <span data-ttu-id="27f02-370">並未建立資料庫，因為它會在後續階段中建立。</span><span class="sxs-lookup"><span data-stu-id="27f02-370">Do not create the database yet, as it will be created in a later stage.</span></span>

    <span data-ttu-id="27f02-371">![SQL Database 伺服器儀表板](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database 伺服器儀表板")</span><span class="sxs-lookup"><span data-stu-id="27f02-371">![SQL Database Server Dashboard](aspnet-mvc-4-custom-action-filters/_static/image25.png "SQL Database Server Dashboard")</span></span>

    <span data-ttu-id="27f02-372">*SQL Database 伺服器儀表板*</span><span class="sxs-lookup"><span data-stu-id="27f02-372">*SQL Database Server Dashboard*</span></span>
2. <span data-ttu-id="27f02-373">下一項工作在您將測試資料庫連接，從 Visual Studio，因此您需要在伺服器的清單中包含您的本機 IP 位址**允許的 IP 位址**。</span><span class="sxs-lookup"><span data-stu-id="27f02-373">In the next task you will test the database connection from Visual Studio, for that reason you need to include your local IP address in the server's list of **Allowed IP Addresses**.</span></span> <span data-ttu-id="27f02-374">若要這樣做，請按一下**設定**，選取 [IP 位址從**目前的用戶端 IP 位址**並將它貼上**起始 IP 位址**和**結束 IP 位址**文字方塊，然後按一下![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="27f02-374">To do that, click **Configure**, select the IP address from **Current Client IP Address** and paste it on the **Start IP Address** and **End IP Address** text boxes and click the ![add-client-ip-address-ok-button](aspnet-mvc-4-custom-action-filters/_static/image26.png) button.</span></span>

    ![加入用戶端 IP 位址](aspnet-mvc-4-custom-action-filters/_static/image27.png)

    <span data-ttu-id="27f02-376">*加入用戶端 IP 位址*</span><span class="sxs-lookup"><span data-stu-id="27f02-376">*Adding Client IP Address*</span></span>
3. <span data-ttu-id="27f02-377">一次**用戶端 IP 位址**新增至允許的 IP 位址清單中，按一下**儲存**來確認變更。</span><span class="sxs-lookup"><span data-stu-id="27f02-377">Once the **Client IP Address** is added to the allowed IP addresses list, click on **Save** to confirm the changes.</span></span>

    ![確認變更](aspnet-mvc-4-custom-action-filters/_static/image28.png)

    <span data-ttu-id="27f02-379">*確認變更*</span><span class="sxs-lookup"><span data-stu-id="27f02-379">*Confirm Changes*</span></span>

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a><span data-ttu-id="27f02-380">工作 3-發行 ASP.NET MVC 4 應用程式使用 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="27f02-380">Task 3 - Publishing an ASP.NET MVC 4 Application using Web Deploy</span></span>

1. <span data-ttu-id="27f02-381">返回至 ASP.NET MVC 4 方案。</span><span class="sxs-lookup"><span data-stu-id="27f02-381">Go back to the ASP.NET MVC 4 solution.</span></span> <span data-ttu-id="27f02-382">在**方案總管 中**，以滑鼠右鍵按一下網站專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="27f02-382">In the **Solution Explorer**, right-click the web site project and select **Publish**.</span></span>

    <span data-ttu-id="27f02-383">![發行應用程式](aspnet-mvc-4-custom-action-filters/_static/image29.png "發行應用程式")</span><span class="sxs-lookup"><span data-stu-id="27f02-383">![Publishing the Application](aspnet-mvc-4-custom-action-filters/_static/image29.png "Publishing the Application")</span></span>

    <span data-ttu-id="27f02-384">*發行網站*</span><span class="sxs-lookup"><span data-stu-id="27f02-384">*Publishing the web site*</span></span>
2. <span data-ttu-id="27f02-385">匯入發行設定檔儲存在第一項工作中。</span><span class="sxs-lookup"><span data-stu-id="27f02-385">Import the publish profile you saved in the first task.</span></span>

    <span data-ttu-id="27f02-386">![匯入發行設定檔](aspnet-mvc-4-custom-action-filters/_static/image30.png "匯入發行設定檔")</span><span class="sxs-lookup"><span data-stu-id="27f02-386">![Importing the publish profile](aspnet-mvc-4-custom-action-filters/_static/image30.png "Importing the publish profile")</span></span>

    <span data-ttu-id="27f02-387">*匯入發行設定檔*</span><span class="sxs-lookup"><span data-stu-id="27f02-387">*Importing publish profile*</span></span>
3. <span data-ttu-id="27f02-388">按一下**驗證連線**。</span><span class="sxs-lookup"><span data-stu-id="27f02-388">Click **Validate Connection**.</span></span> <span data-ttu-id="27f02-389">驗證完成後按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="27f02-389">Once Validation is complete click **Next**.</span></span>

    > [!NOTE]
    > <span data-ttu-id="27f02-390">一旦您看到 [驗證連線] 按鈕旁邊會出現綠色核取記號完成驗證。</span><span class="sxs-lookup"><span data-stu-id="27f02-390">Validation is complete once you see a green checkmark appear next to the Validate Connection button.</span></span>

    <span data-ttu-id="27f02-391">![驗證連線](aspnet-mvc-4-custom-action-filters/_static/image31.png "驗證連線")</span><span class="sxs-lookup"><span data-stu-id="27f02-391">![Validating connection](aspnet-mvc-4-custom-action-filters/_static/image31.png "Validating connection")</span></span>

    <span data-ttu-id="27f02-392">*驗證連接*</span><span class="sxs-lookup"><span data-stu-id="27f02-392">*Validating connection*</span></span>
4. <span data-ttu-id="27f02-393">在**設定**頁面的 **資料庫**區段中，按一下您的資料庫連接文字方塊旁邊的按鈕 (也就是**DefaultConnection**)。</span><span class="sxs-lookup"><span data-stu-id="27f02-393">In the **Settings** page, under the **Databases** section, click the button next to your database connection's textbox (i.e. **DefaultConnection**).</span></span>

    <span data-ttu-id="27f02-394">![Web 部署設定](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web 部署設定")</span><span class="sxs-lookup"><span data-stu-id="27f02-394">![Web deploy configuration](aspnet-mvc-4-custom-action-filters/_static/image32.png "Web deploy configuration")</span></span>

    <span data-ttu-id="27f02-395">*Web 部署設定*</span><span class="sxs-lookup"><span data-stu-id="27f02-395">*Web deploy configuration*</span></span>
5. <span data-ttu-id="27f02-396">設定資料庫連接，如下所示：</span><span class="sxs-lookup"><span data-stu-id="27f02-396">Configure the database connection as follows:</span></span>

   - <span data-ttu-id="27f02-397">在**伺服器名稱**您 SQL Database 伺服器 URL 使用下列方法類型*tcp:* 前置詞。</span><span class="sxs-lookup"><span data-stu-id="27f02-397">In the **Server name** type your SQL Database server URL using the *tcp:* prefix.</span></span>
   - <span data-ttu-id="27f02-398">在**使用者名**輸入您的伺服器系統管理員身分登入名稱。</span><span class="sxs-lookup"><span data-stu-id="27f02-398">In **User name** type your server administrator login name.</span></span>
   - <span data-ttu-id="27f02-399">在**密碼**輸入伺服器系統管理員身分登入密碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-399">In **Password** type your server administrator login password.</span></span>
   - <span data-ttu-id="27f02-400">輸入新的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="27f02-400">Type a new database name.</span></span>

     <span data-ttu-id="27f02-401">![設定目的地連接字串](aspnet-mvc-4-custom-action-filters/_static/image33.png "設定目的地連接字串")</span><span class="sxs-lookup"><span data-stu-id="27f02-401">![Configuring destination connection string](aspnet-mvc-4-custom-action-filters/_static/image33.png "Configuring destination connection string")</span></span>

     <span data-ttu-id="27f02-402">*設定目的地連接字串*</span><span class="sxs-lookup"><span data-stu-id="27f02-402">*Configuring destination connection string*</span></span>
6. <span data-ttu-id="27f02-403">然後按一下 [確定]。 </span><span class="sxs-lookup"><span data-stu-id="27f02-403">Then click **OK**.</span></span> <span data-ttu-id="27f02-404">當系統提示您建立資料庫依序按一下**是**。</span><span class="sxs-lookup"><span data-stu-id="27f02-404">When prompted to create the database click **Yes**.</span></span>

    <span data-ttu-id="27f02-405">![建立資料庫](aspnet-mvc-4-custom-action-filters/_static/image34.png "建立的資料庫字串")</span><span class="sxs-lookup"><span data-stu-id="27f02-405">![Creating the database](aspnet-mvc-4-custom-action-filters/_static/image34.png "Creating the database string")</span></span>

    <span data-ttu-id="27f02-406">*建立資料庫*</span><span class="sxs-lookup"><span data-stu-id="27f02-406">*Creating the database*</span></span>
7. <span data-ttu-id="27f02-407">您將用來連接到在 Windows Azure SQL Database 的連接字串會顯示在預設連接文字方塊中。</span><span class="sxs-lookup"><span data-stu-id="27f02-407">The connection string you will use to connect to SQL Database in Windows Azure is shown within Default Connection textbox.</span></span> <span data-ttu-id="27f02-408">然後按 [下一步] 。</span><span class="sxs-lookup"><span data-stu-id="27f02-408">Then click **Next**.</span></span>

    <span data-ttu-id="27f02-409">![連接字串指向 SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "連接字串指向 SQL 資料庫")</span><span class="sxs-lookup"><span data-stu-id="27f02-409">![Connection string pointing to SQL Database](aspnet-mvc-4-custom-action-filters/_static/image35.png "Connection string pointing to SQL Database")</span></span>

    <span data-ttu-id="27f02-410">*連接字串指向 SQL 資料庫*</span><span class="sxs-lookup"><span data-stu-id="27f02-410">*Connection string pointing to SQL Database*</span></span>
8. <span data-ttu-id="27f02-411">在**預覽**頁面上，按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="27f02-411">In the **Preview** page, click **Publish**.</span></span>

    <span data-ttu-id="27f02-412">![Web 應用程式發行](aspnet-mvc-4-custom-action-filters/_static/image36.png "發行 web 應用程式")</span><span class="sxs-lookup"><span data-stu-id="27f02-412">![Publishing the web application](aspnet-mvc-4-custom-action-filters/_static/image36.png "Publishing the web application")</span></span>

    <span data-ttu-id="27f02-413">*發行 web 應用程式*</span><span class="sxs-lookup"><span data-stu-id="27f02-413">*Publishing the web application*</span></span>
9. <span data-ttu-id="27f02-414">一旦在發佈程序完成時，預設瀏覽器會開啟已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="27f02-414">Once the publishing process finishes, your default browser will open the published web site.</span></span>

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a><span data-ttu-id="27f02-415">附錄 c： 使用程式碼片段</span><span class="sxs-lookup"><span data-stu-id="27f02-415">Appendix C: Using Code Snippets</span></span>

<span data-ttu-id="27f02-416">程式碼片段，您會有您需要在您可以的所有程式碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-416">With code snippets, you have all the code you need at your fingertips.</span></span> <span data-ttu-id="27f02-417">實驗室文件會告訴您完全時您可以使用它們，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="27f02-417">The lab document will tell you exactly when you can use them, as shown in the following figure.</span></span>

<span data-ttu-id="27f02-418">![若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image37.png "使用 Visual Studio 程式碼片段，將程式碼插入您的專案")</span><span class="sxs-lookup"><span data-stu-id="27f02-418">![Using Visual Studio code snippets to insert code into your project](aspnet-mvc-4-custom-action-filters/_static/image37.png "Using Visual Studio code snippets to insert code into your project")</span></span>

<span data-ttu-id="27f02-419">*若要將程式碼插入您的專案中使用 Visual Studio 程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="27f02-419">*Using Visual Studio code snippets to insert code into your project*</span></span>

<span data-ttu-id="27f02-420">***若要加入的程式碼片段，使用鍵盤 （僅限 C#)***</span><span class="sxs-lookup"><span data-stu-id="27f02-420">***To add a code snippet using the keyboard (C# only)***</span></span>

1. <span data-ttu-id="27f02-421">將游標放在您想要插入的程式碼。</span><span class="sxs-lookup"><span data-stu-id="27f02-421">Place the cursor where you would like to insert the code.</span></span>
2. <span data-ttu-id="27f02-422">開始鍵入片段名稱 （不含空格或連字號）。</span><span class="sxs-lookup"><span data-stu-id="27f02-422">Start typing the snippet name (without spaces or hyphens).</span></span>
3. <span data-ttu-id="27f02-423">觀察 IntelliSense 會顯示比對程式碼片段的名稱。</span><span class="sxs-lookup"><span data-stu-id="27f02-423">Watch as IntelliSense displays matching snippets' names.</span></span>
4. <span data-ttu-id="27f02-424">選取正確的程式碼片段 （或直到選取整個程式碼片段名稱時，保留 輸入）。</span><span class="sxs-lookup"><span data-stu-id="27f02-424">Select the correct snippet (or keep typing until the entire snippet's name is selected).</span></span>
5. <span data-ttu-id="27f02-425">按下 Tab 鍵兩次的游標位置插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="27f02-425">Press the Tab key twice to insert the snippet at the cursor location.</span></span>

<span data-ttu-id="27f02-426">![開始輸入程式碼片段名稱](aspnet-mvc-4-custom-action-filters/_static/image38.png "開始鍵入片段名稱")</span><span class="sxs-lookup"><span data-stu-id="27f02-426">![Start typing the snippet name](aspnet-mvc-4-custom-action-filters/_static/image38.png "Start typing the snippet name")</span></span>

<span data-ttu-id="27f02-427">*開始輸入程式碼片段名稱*</span><span class="sxs-lookup"><span data-stu-id="27f02-427">*Start typing the snippet name*</span></span>

<span data-ttu-id="27f02-428">![按 Tab 鍵選取反白顯示的程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image39.png "按 Tab 鍵以選取反白顯示的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="27f02-428">![Press Tab to select the highlighted snippet](aspnet-mvc-4-custom-action-filters/_static/image39.png "Press Tab to select the highlighted snippet")</span></span>

<span data-ttu-id="27f02-429">*按 Tab 鍵選取反白顯示的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="27f02-429">*Press Tab to select the highlighted snippet*</span></span>

<span data-ttu-id="27f02-430">![按 Tab 鍵一次程式碼片段就會擴展](aspnet-mvc-4-custom-action-filters/_static/image40.png "按 Tab 鍵一次程式碼片段就會擴展")</span><span class="sxs-lookup"><span data-stu-id="27f02-430">![Press Tab again and the snippet will expand](aspnet-mvc-4-custom-action-filters/_static/image40.png "Press Tab again and the snippet will expand")</span></span>

<span data-ttu-id="27f02-431">*按 Tab 鍵一次程式碼片段就會擴展*</span><span class="sxs-lookup"><span data-stu-id="27f02-431">*Press Tab again and the snippet will expand*</span></span>

<span data-ttu-id="27f02-432">***若要加入的程式碼片段，使用滑鼠 （C#、 Visual Basic 和 XML）*** 1。</span><span class="sxs-lookup"><span data-stu-id="27f02-432">***To add a code snippet using the mouse (C#, Visual Basic and XML)*** 1.</span></span> <span data-ttu-id="27f02-433">以滑鼠右鍵按一下您要插入程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="27f02-433">Right-click where you want to insert the code snippet.</span></span>

1. <span data-ttu-id="27f02-434">選取**插入程式碼片段**後面**我的程式碼片段**。</span><span class="sxs-lookup"><span data-stu-id="27f02-434">Select **Insert Snippet** followed by **My Code Snippets**.</span></span>
2. <span data-ttu-id="27f02-435">透過在按一下挑選清單中，從相關的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="27f02-435">Pick the relevant snippet from the list, by clicking on it.</span></span>

<span data-ttu-id="27f02-436">![以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image41.png "以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="27f02-436">![Right-click where you want to insert the code snippet and select Insert Snippet](aspnet-mvc-4-custom-action-filters/_static/image41.png "Right-click where you want to insert the code snippet and select Insert Snippet")</span></span>

<span data-ttu-id="27f02-437">*以滑鼠右鍵按一下您要插入程式碼片段，然後選取 插入程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="27f02-437">*Right-click where you want to insert the code snippet and select Insert Snippet*</span></span>

<span data-ttu-id="27f02-438">![透過在按一下挑選清單中，從相關的程式碼片段](aspnet-mvc-4-custom-action-filters/_static/image42.png "上它即可挑選清單中，從相關的程式碼片段")</span><span class="sxs-lookup"><span data-stu-id="27f02-438">![Pick the relevant snippet from the list, by clicking on it](aspnet-mvc-4-custom-action-filters/_static/image42.png "Pick the relevant snippet from the list, by clicking on it")</span></span>

<span data-ttu-id="27f02-439">*透過在按一下挑選清單中，從相關的程式碼片段*</span><span class="sxs-lookup"><span data-stu-id="27f02-439">*Pick the relevant snippet from the list, by clicking on it*</span></span>
