---
uid: mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
title: '反覆項目 #5 – 建立單元測試 (C#) |Microsoft 文件'
author: microsoft
description: 第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。 我們模擬資料模型類別，並建立單元測試的 o...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 28ad8f80-b8a5-444e-b478-8b15a846060c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-5-create-unit-tests-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a61b5791a40088df9d27f7b1bd37df1831ef22b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875574"
---
<a name="iteration-5--create-unit-tests-c"></a><span data-ttu-id="b3439-104">反覆項目 #5 – 建立單元測試 (C#)</span><span class="sxs-lookup"><span data-stu-id="b3439-104">Iteration #5 – Create unit tests (C#)</span></span>
====================
<span data-ttu-id="b3439-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b3439-105">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b3439-106">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="b3439-106">Download Code</span></span>](iteration-5-create-unit-tests-cs/_static/contactmanager_5_cs1.zip)

> <span data-ttu-id="b3439-107">第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-107">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="b3439-108">我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="b3439-108">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="b3439-109">建立連絡人管理 ASP.NET MVC 應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="b3439-109">Building a Contact Management ASP.NET MVC Application (C#)</span></span>

<span data-ttu-id="b3439-110">在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="b3439-110">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="b3439-111">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="b3439-111">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="b3439-112">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="b3439-112">We build the application over multiple iterations.</span></span> <span data-ttu-id="b3439-113">與每個反覆項目，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-113">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="b3439-114">這個多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="b3439-114">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="b3439-115">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-115">Iteration #1 - Create the application.</span></span> <span data-ttu-id="b3439-116">在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="b3439-116">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="b3439-117">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="b3439-117">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="b3439-118">反覆項目 #2-請看起來很棒的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-118">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="b3439-119">在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="b3439-119">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="b3439-120">反覆項目 #3-加入表單驗證。</span><span class="sxs-lookup"><span data-stu-id="b3439-120">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="b3439-121">第三個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="b3439-121">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="b3439-122">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="b3439-122">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="b3439-123">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-123">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="b3439-124">反覆項目 #4-請鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-124">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="b3439-125">在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-125">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="b3439-126">例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="b3439-126">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="b3439-127">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-127">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="b3439-128">第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-128">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="b3439-129">我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="b3439-129">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="b3439-130">反覆項目 #6-使用測試為導向的開發。</span><span class="sxs-lookup"><span data-stu-id="b3439-130">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="b3439-131">這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-131">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="b3439-132">在這個反覆項目，我們會加入連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="b3439-132">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="b3439-133">反覆項目 #7： 加入 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="b3439-133">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="b3439-134">在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="b3439-134">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="this-iteration"></a><span data-ttu-id="b3439-135">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="b3439-135">This Iteration</span></span>

<span data-ttu-id="b3439-136">在連絡人管理員應用程式的上一個反覆項目，我們重構是更鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-136">In the previous iteration of the Contact Manager application, we refactored the application to be more loosely coupled.</span></span> <span data-ttu-id="b3439-137">我們分隔到不同的控制站、 服務和儲存機制的圖層的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-137">We separated the application into distinct controller, service, and repository layers.</span></span> <span data-ttu-id="b3439-138">每個圖層會與下圖層，透過介面互動。</span><span class="sxs-lookup"><span data-stu-id="b3439-138">Each layer interacts with the layer beneath it through interfaces.</span></span>

<span data-ttu-id="b3439-139">我們重構應用程式，讓您更輕鬆地維護及修改應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-139">We refactored the application to make the application easier to maintain and modify.</span></span> <span data-ttu-id="b3439-140">例如，如果我們需要使用新的資料存取技術，我們只是可以變更儲存機制層但沒有碰觸的控制站或服務層。</span><span class="sxs-lookup"><span data-stu-id="b3439-140">For example, if we need to use a new data access technology, we can simply change the repository layer without touching the controller or service layer.</span></span> <span data-ttu-id="b3439-141">藉由連絡人管理員鬆散結合且我們 ve 進行更有彈性，來變更應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-141">By making the Contact Manager loosely coupled, we ve made the application more resilient to change.</span></span>

<span data-ttu-id="b3439-142">但是，我們需要將新功能新增至連絡人管理員應用程式時，會發生什麼事？</span><span class="sxs-lookup"><span data-stu-id="b3439-142">But, what happens when we need to add a new feature to the Contact Manager application?</span></span> <span data-ttu-id="b3439-143">或者，當我們修正 bug 時，會發生什麼事？</span><span class="sxs-lookup"><span data-stu-id="b3439-143">Or, what happens when we fix a bug?</span></span> <span data-ttu-id="b3439-144">撰寫程式碼的悲傷，但也經過驗證，真是每當您修改程式碼建立引入新的 bug 的風險。</span><span class="sxs-lookup"><span data-stu-id="b3439-144">A sad, but well proven, truth of writing code is that whenever you touch code you create the risk of introducing new bugs.</span></span>

<span data-ttu-id="b3439-145">例如，一個美好的一天，您的管理員可能會要求您新增到連絡人管理員的新功能。</span><span class="sxs-lookup"><span data-stu-id="b3439-145">For example, one fine day, your manager might ask you to add a new feature to the Contact Manager.</span></span> <span data-ttu-id="b3439-146">她想要新增連絡人群組的支援。</span><span class="sxs-lookup"><span data-stu-id="b3439-146">She wants you to add support for Contact Groups.</span></span> <span data-ttu-id="b3439-147">她想要讓使用者將他們的連絡人組織成群組，例如朋友、 商務和等等。</span><span class="sxs-lookup"><span data-stu-id="b3439-147">She wants you to enable users to organize their contacts into groups such as Friends, Business, and so on.</span></span>

<span data-ttu-id="b3439-148">若要實作這項新功能，您必須修改連絡人管理員應用程式的所有三個層級。</span><span class="sxs-lookup"><span data-stu-id="b3439-148">In order to implement this new feature, you'll need to modify all three layers of the Contact Manager application.</span></span> <span data-ttu-id="b3439-149">您必須將新功能加入至控制器、 服務層和儲存機制。</span><span class="sxs-lookup"><span data-stu-id="b3439-149">You'll need to add new functionality to the controllers, the service layer, and the repository.</span></span> <span data-ttu-id="b3439-150">一旦您開始修改程式碼時，就可能會中斷運作正常的功能。</span><span class="sxs-lookup"><span data-stu-id="b3439-150">As soon as you start modifying code, you risk breaking functionality that worked before.</span></span>

<span data-ttu-id="b3439-151">重構至個別圖層，應用程式，如同先前的反覆項目，是個好方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-151">Refactoring our application into separate layers, as we did in the previous iteration, was a good thing.</span></span> <span data-ttu-id="b3439-152">因為它可讓我們對整個圖層中的變更，但沒有碰觸其餘的應用程式，它會是個好方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-152">It was a good thing because it enables us to make changes to entire layers without touching the rest of the application.</span></span> <span data-ttu-id="b3439-153">不過，如果您想要讓您更輕鬆地維護及修改圖層內的程式碼，您需要建立單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-153">However, if you want to make the code within a layer easier to maintain and modify, you need to create unit tests for the code.</span></span>

<span data-ttu-id="b3439-154">您使用單元測試的程式碼個別單位。</span><span class="sxs-lookup"><span data-stu-id="b3439-154">You use a unit test to test an individual unit of code.</span></span> <span data-ttu-id="b3439-155">這些程式碼的單位是小於整個應用程式層級。</span><span class="sxs-lookup"><span data-stu-id="b3439-155">These units of code are smaller than entire application layers.</span></span> <span data-ttu-id="b3439-156">一般而言，您可以使用單元測試來驗證您預期的方式中的行為是否在程式碼中的特定方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-156">Typically, you use a unit test to verify whether a particular method in your code behaves in the way that you expect.</span></span> <span data-ttu-id="b3439-157">例如，您會建立由 ContactManagerService 類別公開的 CreateContact() 方法的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-157">For example, you would create a unit test for the CreateContact() method exposed by the ContactManagerService class.</span></span>

<span data-ttu-id="b3439-158">只要應用程式工作的單元測試，例如安全網。</span><span class="sxs-lookup"><span data-stu-id="b3439-158">The unit tests for an application work just like a safety net.</span></span> <span data-ttu-id="b3439-159">每當您修改應用程式中的程式碼，您可以執行一組要檢查此修改是否中斷現有功能的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-159">Whenever you modify code in an application, you can run a set of unit tests to check whether the modification breaks existing functionality.</span></span> <span data-ttu-id="b3439-160">單元測試讓安全地修改程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-160">Unit tests make your code safe to modify.</span></span> <span data-ttu-id="b3439-161">單元測試讓所有的程式碼應用程式中變更的更有彈性。</span><span class="sxs-lookup"><span data-stu-id="b3439-161">Unit tests make all of the code in your application more resilient to change.</span></span>

<span data-ttu-id="b3439-162">在這個反覆項目，我們加入我們的連絡人管理員應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-162">In this iteration, we add unit tests to our Contact Manager application.</span></span> <span data-ttu-id="b3439-163">這樣一來，在下一個反覆項目，我們可以將連絡人群組加入我們的應用程式而不需擔心中斷現有的功能。</span><span class="sxs-lookup"><span data-stu-id="b3439-163">That way, in the next iteration, we can add Contact Groups to our application without worrying about breaking existing functionality.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b3439-164">有各種不同的單元測試架構，包含適用於 NUnit、 xUnit.net 和 MbUnit。</span><span class="sxs-lookup"><span data-stu-id="b3439-164">There are a variety of unit testing frameworks including NUnit, xUnit.net, and MbUnit.</span></span> <span data-ttu-id="b3439-165">在本教學課程中，我們使用的單元測試架構隨附於 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="b3439-165">In this tutorial, we use the unit testing framework included with Visual Studio.</span></span> <span data-ttu-id="b3439-166">不過，您可以輕鬆地使用其中一種替代的架構。</span><span class="sxs-lookup"><span data-stu-id="b3439-166">However, you could just as easily use one of these alternative frameworks.</span></span>


## <a name="what-gets-tested"></a><span data-ttu-id="b3439-167">取得測試的項目</span><span class="sxs-lookup"><span data-stu-id="b3439-167">What Gets Tested</span></span>

<span data-ttu-id="b3439-168">在理想的世界裡，所有的程式碼會涵蓋單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-168">In the perfect world, all of your code would be covered by unit tests.</span></span> <span data-ttu-id="b3439-169">在理想的世界裡，您必須完美的安全網。</span><span class="sxs-lookup"><span data-stu-id="b3439-169">In the perfect world, you would have the perfect safety net.</span></span> <span data-ttu-id="b3439-170">您可以修改任何應用程式中的程式碼行，即可立即知道，藉由執行單元測試，變更是否中斷現有的功能。</span><span class="sxs-lookup"><span data-stu-id="b3439-170">You would be able to modify any line of code in your application and know instantly, by executing your unit tests, whether the change broke existing functionality.</span></span>

<span data-ttu-id="b3439-171">不過，我們不完美的世界中的即時 t。</span><span class="sxs-lookup"><span data-stu-id="b3439-171">However, we don t live in a perfect world.</span></span> <span data-ttu-id="b3439-172">實際上，撰寫單元測試時，您專注於商務邏輯 （例如，驗證邏輯） 為撰寫測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-172">In practice, when writing unit tests, you concentrate on writing tests for your business logic (for example, validation logic).</span></span> <span data-ttu-id="b3439-173">特別是，您*不*撰寫單元測試您的資料存取邏輯或檢視邏輯。</span><span class="sxs-lookup"><span data-stu-id="b3439-173">In particular, you *do not* write unit tests for your data access logic or your view logic.</span></span>

<span data-ttu-id="b3439-174">若要很有用，單元測試必須非常快速地執行。</span><span class="sxs-lookup"><span data-stu-id="b3439-174">To be useful, unit tests must execute very quickly.</span></span> <span data-ttu-id="b3439-175">您輕鬆地會累積數百個 （或甚至數千） 的應用程式的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-175">You easily can accumulate hundreds (or even thousands) of unit tests for an application.</span></span> <span data-ttu-id="b3439-176">如果單元測試需要很長的時間來執行您可避免執行它們。</span><span class="sxs-lookup"><span data-stu-id="b3439-176">If the unit tests take a long time to run then you'll avoid executing them.</span></span> <span data-ttu-id="b3439-177">換句話說，長時間執行的單元測試是沒有幫助，每天程式碼撰寫用途的。</span><span class="sxs-lookup"><span data-stu-id="b3439-177">In other words, long running unit tests are useless for day to day coding purposes.</span></span>

<span data-ttu-id="b3439-178">基於這個理由，您通常不要撰寫與資料庫互動的程式碼的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-178">For this reason, you typically do not write unit tests for code that interacts with a database.</span></span> <span data-ttu-id="b3439-179">針對即時資料庫，執行數百個單元測試很太慢。</span><span class="sxs-lookup"><span data-stu-id="b3439-179">Running hundreds of unit tests against a live database would be too slow.</span></span> <span data-ttu-id="b3439-180">相反地，您會模擬您的資料庫，並撰寫與模擬的資料庫 （我們討論模擬下的資料庫） 互動的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-180">Instead, you mock your database and write code that interacts with the mock database (we discuss mocking a database below).</span></span>

<span data-ttu-id="b3439-181">針對類似的理由，您通常不要撰寫單元測試的檢視。</span><span class="sxs-lookup"><span data-stu-id="b3439-181">For a similar reason, you typically do not write unit tests for views.</span></span> <span data-ttu-id="b3439-182">若要測試的檢視，您必須備妥 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b3439-182">In order to test a view, you must spin up a web server.</span></span> <span data-ttu-id="b3439-183">因為網頁伺服器，大致是相對比較慢的處理程序，所以不建議建立單元測試您的檢視。</span><span class="sxs-lookup"><span data-stu-id="b3439-183">Because spinning up a web server is a relatively slow process, creating unit tests for your views is not recommended.</span></span>

<span data-ttu-id="b3439-184">如果您的檢視包含複雜的邏輯應該考慮將邏輯移至 Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-184">If your view contains complicated logic then you should consider moving the logic into Helper methods.</span></span> <span data-ttu-id="b3439-185">您可以撰寫單元測試的 Helper 方法，以執行不含才能組織好 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="b3439-185">You can write unit tests for Helper methods that execute without spinning up a web server.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b3439-186">寫入測試的資料存取邏輯或檢視邏輯時，不建議您撰寫單元測試，而這些測試可以是建置功能或整合測試時非常有用。</span><span class="sxs-lookup"><span data-stu-id="b3439-186">While writing tests for data access logic or view logic is not a good idea when writing unit tests, these tests can be very valuable when building functional or integration tests.</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="b3439-187">ASP.NET MVC 是 Web Form 檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="b3439-187">ASP.NET MVC is the Web Forms View Engine.</span></span> <span data-ttu-id="b3439-188">Web Form 檢視引擎相依於 web 伺服器時，可能不是其他檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="b3439-188">While the Web Forms View Engine is dependent on a web server, other view engines might not be.</span></span>


## <a name="using-a-mock-object-framework"></a><span data-ttu-id="b3439-189">使用模擬物件架構</span><span class="sxs-lookup"><span data-stu-id="b3439-189">Using a Mock Object Framework</span></span>

<span data-ttu-id="b3439-190">當建立單元測試時，您幾乎都需要利用模擬物件架構。</span><span class="sxs-lookup"><span data-stu-id="b3439-190">When building unit tests, you almost always need to take advantage of a Mock Object framework.</span></span> <span data-ttu-id="b3439-191">模擬物件架構可讓您在 您的應用程式中建立模擬和虛設常式類別。</span><span class="sxs-lookup"><span data-stu-id="b3439-191">A Mock Object framework enables you to create mocks and stubs for the classes in your application.</span></span>

<span data-ttu-id="b3439-192">例如，您可以使用模擬物件架構來產生模擬的儲存機制類別版本。</span><span class="sxs-lookup"><span data-stu-id="b3439-192">For example, you can use a Mock Object framework to generate a mock version of your repository class.</span></span> <span data-ttu-id="b3439-193">這樣一來，您可以使用模擬儲存機制類別而不是實際的儲存機制類別在您的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-193">That way, you can use the mock repository class instead of the real repository class in your unit tests.</span></span> <span data-ttu-id="b3439-194">使用模擬的儲存機制，可讓您避免執行單元測試時，執行資料庫程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-194">Using the mock repository enables you to avoid executing database code when executing a unit test.</span></span>

<span data-ttu-id="b3439-195">Visual Studio 不包含模擬物件架構。</span><span class="sxs-lookup"><span data-stu-id="b3439-195">Visual Studio does not include a Mock Object framework.</span></span> <span data-ttu-id="b3439-196">不過，有數個商業和開放原始碼模擬物件架構適用於.NET framework:</span><span class="sxs-lookup"><span data-stu-id="b3439-196">However, there are several commercial and open source Mock Object frameworks available for the .NET framework:</span></span>

1. <span data-ttu-id="b3439-197">Moq-此架構可開放原始碼 BSD 授權。</span><span class="sxs-lookup"><span data-stu-id="b3439-197">Moq - This framework is available under the open source BSD license.</span></span> <span data-ttu-id="b3439-198">您可以下載從 Moq [ https://code.google.com/p/moq/ ](https://code.google.com/p/moq/)。</span><span class="sxs-lookup"><span data-stu-id="b3439-198">You can download Moq from [https://code.google.com/p/moq/](https://code.google.com/p/moq/).</span></span>
2. <span data-ttu-id="b3439-199">Rhino Mocks-此架構才可使用的開放原始碼 BSD 授權。</span><span class="sxs-lookup"><span data-stu-id="b3439-199">Rhino Mocks - This framework is available under the open source BSD license.</span></span> <span data-ttu-id="b3439-200">您可以下載從 Mocks Rhino [ http://ayende.com/projects/rhino-mocks.aspx ](http://ayende.com/projects/rhino-mocks.aspx)。</span><span class="sxs-lookup"><span data-stu-id="b3439-200">You can download Rhino Mocks from [http://ayende.com/projects/rhino-mocks.aspx](http://ayende.com/projects/rhino-mocks.aspx).</span></span>
3. <span data-ttu-id="b3439-201">Typemock 絕緣器-這是商業架構。</span><span class="sxs-lookup"><span data-stu-id="b3439-201">Typemock Isolator - This is a commercial framework.</span></span> <span data-ttu-id="b3439-202">您可以下載從試用版[ http://www.typemock.com/ ](http://www.typemock.com/)。</span><span class="sxs-lookup"><span data-stu-id="b3439-202">You can download a trial version from [http://www.typemock.com/](http://www.typemock.com/).</span></span>

<span data-ttu-id="b3439-203">在本教學課程中，我決定使用 Moq。</span><span class="sxs-lookup"><span data-stu-id="b3439-203">In this tutorial, I decided to use Moq.</span></span> <span data-ttu-id="b3439-204">不過，您可以輕鬆地使用 Rhino Mocks 或 Typemock 絕緣器來建立模擬物件連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-204">However, you could just as easily use Rhino Mocks or Typemock Isolator to create the Mock objects for the Contact Manager application.</span></span>

<span data-ttu-id="b3439-205">您可以使用 Moq 之前，您需要完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="b3439-205">Before you can use Moq, you need to complete the following steps:</span></span>

1. <span data-ttu-id="b3439-206">。</span><span class="sxs-lookup"><span data-stu-id="b3439-206">.</span></span>
2. <span data-ttu-id="b3439-207">解壓縮下載之前，請確定您以滑鼠右鍵按一下檔案，然後按一下 [] 按鈕**解除封鎖**（請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="b3439-207">Before you unzip the download, make sure that you right-click the file and click the button labeled **Unblock** (see Figure 1).</span></span>
3. <span data-ttu-id="b3439-208">解壓縮下載。</span><span class="sxs-lookup"><span data-stu-id="b3439-208">Unzip the download.</span></span>
4. <span data-ttu-id="b3439-209">Moq 組件的參考加入 ContactManager.Tests 專案中的 [參考] 資料夾上按一下滑鼠右鍵，然後選取**加入參考**。</span><span class="sxs-lookup"><span data-stu-id="b3439-209">Add a reference to the Moq assembly by right-clicking the References folder in the ContactManager.Tests project and selecting **Add Reference**.</span></span> <span data-ttu-id="b3439-210">在 [瀏覽] 索引標籤中，瀏覽至您解壓縮 Moq 資料夾並選取 Moq.dll 組件。</span><span class="sxs-lookup"><span data-stu-id="b3439-210">Under the Browse tab, browse to the folder where you unzipped Moq and select the Moq.dll assembly.</span></span> <span data-ttu-id="b3439-211">按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b3439-211">Click the **OK** button.</span></span>
5. <span data-ttu-id="b3439-212">完成這些步驟之後，參考資料夾看起來應該像圖 2。</span><span class="sxs-lookup"><span data-stu-id="b3439-212">After you complete these steps, your References folder should look like Figure 2.</span></span>


<span data-ttu-id="b3439-213">[![解除封鎖 Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b3439-213">[![Unblocking Moq](iteration-5-create-unit-tests-cs/_static/image1.jpg)](iteration-5-create-unit-tests-cs/_static/image1.png)</span></span>

<span data-ttu-id="b3439-214">**圖 01**： 解除封鎖 Moq ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="b3439-214">**Figure 01**: Unblocking Moq([Click to view full-size image](iteration-5-create-unit-tests-cs/_static/image2.png))</span></span>


<span data-ttu-id="b3439-215">[![加入 Moq 之後的參考](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="b3439-215">[![References after adding Moq](iteration-5-create-unit-tests-cs/_static/image2.jpg)](iteration-5-create-unit-tests-cs/_static/image3.png)</span></span>

<span data-ttu-id="b3439-216">**圖 02**： 參考加入 Moq 之後 ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="b3439-216">**Figure 02**: References after adding Moq([Click to view full-size image](iteration-5-create-unit-tests-cs/_static/image4.png))</span></span>


## <a name="creating-unit-tests-for-the-service-layer"></a><span data-ttu-id="b3439-217">建立服務層的單元測試</span><span class="sxs-lookup"><span data-stu-id="b3439-217">Creating Unit Tests for the Service Layer</span></span>

<span data-ttu-id="b3439-218">可讓 s 開始建立一組我們連絡人管理員應用程式 s 的服務層的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-218">Let s start by creating a set of unit tests for our Contact Manager application s service layer.</span></span> <span data-ttu-id="b3439-219">若要確認我們驗證邏輯，我們會使用這些測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-219">We'll use these tests to verify our validation logic.</span></span>

<span data-ttu-id="b3439-220">建立新資料夾，名為 ContactManager.Tests 專案中的模型。</span><span class="sxs-lookup"><span data-stu-id="b3439-220">Create a new folder named Models in the ContactManager.Tests project.</span></span> <span data-ttu-id="b3439-221">接下來，以滑鼠右鍵按一下 [模型] 資料夾，然後選取**新增]、 [新的測試**。</span><span class="sxs-lookup"><span data-stu-id="b3439-221">Next, right-click the Models folder and select **Add, New Test**.</span></span> <span data-ttu-id="b3439-222">**加入新測試**圖 3 所示的對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="b3439-222">The **Add New Test** dialog shown in Figure 3 appears.</span></span> <span data-ttu-id="b3439-223">選取**單元測試**範本，並命名新的測試 ContactManagerServiceTest.cs。</span><span class="sxs-lookup"><span data-stu-id="b3439-223">Select the **Unit Test** template and name your new test ContactManagerServiceTest.cs.</span></span> <span data-ttu-id="b3439-224">按一下**確定** 按鈕，測試專案中加入新的測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-224">Click the **OK** button to add your new test to your Test Project.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b3439-225">一般情況下，您想要比對您的 ASP.NET MVC 專案的資料夾結構的測試專案的資料夾結構。</span><span class="sxs-lookup"><span data-stu-id="b3439-225">In general, you want the folder structure of your Test Project to match the folder structure of your ASP.NET MVC project.</span></span> <span data-ttu-id="b3439-226">例如，您會置於 Controllers 資料夾，在 [模型] 資料夾中，模型測試的測試控制器，並以此類推。</span><span class="sxs-lookup"><span data-stu-id="b3439-226">For example, you place controller tests in a Controllers folder, model tests in a Models folder, and so on.</span></span>


<span data-ttu-id="b3439-227">[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="b3439-227">[![Models\ContactManagerServiceTest.cs](iteration-5-create-unit-tests-cs/_static/image3.jpg)](iteration-5-create-unit-tests-cs/_static/image5.png)</span></span>

<span data-ttu-id="b3439-228">**圖 03**: Models\ContactManagerServiceTest.cs ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b3439-228">**Figure 03**: Models\ContactManagerServiceTest.cs([Click to view full-size image](iteration-5-create-unit-tests-cs/_static/image6.png))</span></span>


<span data-ttu-id="b3439-229">一開始，我們想要測試 ContactManagerService 類別所公開的 CreateContact() 方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-229">Initially, we want to test the CreateContact() method exposed by the ContactManagerService class.</span></span> <span data-ttu-id="b3439-230">我們將建立下列五個測試：</span><span class="sxs-lookup"><span data-stu-id="b3439-230">We'll create the following five tests:</span></span>

- <span data-ttu-id="b3439-231">CreateContact()-測試該 CreateContact() 在值為 true 時傳回有效的連絡人會傳遞至方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-231">CreateContact() - Tests that CreateContact() returns the value true when a valid Contact is passed to the method.</span></span>
- <span data-ttu-id="b3439-232">CreateContactRequiredFirstName()-錯誤訊息會加入至模型狀態時遺失的名字與連絡人的測試會傳遞至 CreateContact() 方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-232">CreateContactRequiredFirstName() - Tests that an error message is added to model state when a Contact with a missing first name is passed to the CreateContact() method.</span></span>
- <span data-ttu-id="b3439-233">CreateContactRequredLastName()-測試錯誤訊息會加入至模型狀態時具有遺失的姓氏的連絡人會傳遞至 CreateContact() 方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-233">CreateContactRequredLastName() - Tests that an error message is added to model state when a Contact with a missing last name is passed to the CreateContact() method.</span></span>
- <span data-ttu-id="b3439-234">CreateContactInvalidPhone()-測試錯誤訊息會加入至模型狀態時具有無效電話號碼的連絡人會傳遞至 CreateContact() 方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-234">CreateContactInvalidPhone() - Tests that an error message is added to model state when a Contact with an invalid phone number is passed to the CreateContact() method.</span></span>
- <span data-ttu-id="b3439-235">CreateContactInvalidEmail()-CreateContact() 方法傳遞錯誤訊息會加入至模型狀態時具有無效的電子郵件地址的連絡人的測試...</span><span class="sxs-lookup"><span data-stu-id="b3439-235">CreateContactInvalidEmail() - Tests that an error message is added to model state when a Contact with an invalid email address is passed to the CreateContact() method..</span></span>

<span data-ttu-id="b3439-236">第一項測試會驗證有效的連絡人不會產生驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="b3439-236">The first test verifies that a valid Contact does not generate a validation error.</span></span> <span data-ttu-id="b3439-237">其餘的測試會檢查每個驗證規則。</span><span class="sxs-lookup"><span data-stu-id="b3439-237">The remaining tests check each of the validation rules.</span></span>

<span data-ttu-id="b3439-238">這些測試的程式碼會包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="b3439-238">The code for these tests is contained in Listing 1.</span></span>

<span data-ttu-id="b3439-239">**Listing 1 - Models\ContactManagerServiceTest.cs**</span><span class="sxs-lookup"><span data-stu-id="b3439-239">**Listing 1 - Models\ContactManagerServiceTest.cs**</span></span>

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample1.cs)]


<span data-ttu-id="b3439-240">因為我們使用連絡人類別清單 1 中，我們需要將 Microsoft Entity Framework 的參考加入至我們的測試專案。</span><span class="sxs-lookup"><span data-stu-id="b3439-240">Because we use the Contact class in Listing 1, we need to add a reference to the Microsoft Entity Framework to our Test project.</span></span> <span data-ttu-id="b3439-241">新增 System.Data.Entity 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="b3439-241">Add a reference to the System.Data.Entity assembly.</span></span>


<span data-ttu-id="b3439-242">清單 1 包含名為 initialize （），以 [TestInitialize] 屬性裝飾的方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-242">Listing 1 contains a method named Initialize() that is decorated with the [TestInitialize] attribute.</span></span> <span data-ttu-id="b3439-243">每個單元測試執行之前自動呼叫這個方法 （它每個單元測試之前稱為 5 次）。</span><span class="sxs-lookup"><span data-stu-id="b3439-243">This method is called automatically before each of the unit tests is run (it is called 5 times right before each of the unit tests).</span></span> <span data-ttu-id="b3439-244">Initialize （） 方法會建立模擬的儲存機制，與下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="b3439-244">The Initialize() method creates a mock repository with the following line of code:</span></span>

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample2.cs)]

<span data-ttu-id="b3439-245">這行程式碼會產生模擬的儲存機制從 IContactManagerRepository 介面使用 Moq 架構。</span><span class="sxs-lookup"><span data-stu-id="b3439-245">This line of code uses the Moq framework to generate a mock repository from the IContactManagerRepository interface.</span></span> <span data-ttu-id="b3439-246">模擬儲存機制而不是實際 EntityContactManagerRepository 用於避免每個單元測試執行時存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="b3439-246">The mock repository is used instead of the actual EntityContactManagerRepository to avoid accessing the database when each unit test is run.</span></span> <span data-ttu-id="b3439-247">模擬儲存機制實作介面方法的 IContactManagerRepository，但是方法 don t 實際執行任何動作。</span><span class="sxs-lookup"><span data-stu-id="b3439-247">The mock repository implements the methods of the IContactManagerRepository interface, but the methods don t actually do anything.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="b3439-248">當使用 Moq 架構，沒有區分\_mockRepository 和\_mockRepository.Object。</span><span class="sxs-lookup"><span data-stu-id="b3439-248">When using the Moq framework, there is a distinction between \_mockRepository and \_mockRepository.Object.</span></span> <span data-ttu-id="b3439-249">前者是指模擬&lt;IContactManagerRepository&gt;類別，其中包含方法來指定模擬的儲存機制的行為方式。</span><span class="sxs-lookup"><span data-stu-id="b3439-249">The former refers to the Mock&lt;IContactManagerRepository&gt; class that contains methods for specifying how the mock repository will behave.</span></span> <span data-ttu-id="b3439-250">後者是指實際模擬儲存機制實作 IContactManagerRepository 介面。</span><span class="sxs-lookup"><span data-stu-id="b3439-250">The latter refers to the actual mock repository that implements the IContactManagerRepository interface.</span></span>


<span data-ttu-id="b3439-251">建立 ContactManagerService 類別的執行個體時，會模擬儲存機制使用 initialize （） 方法中。</span><span class="sxs-lookup"><span data-stu-id="b3439-251">The mock repository is used in the Initialize() method when creating an instance of the ContactManagerService class.</span></span> <span data-ttu-id="b3439-252">所有的個別單元測試使用 ContactManagerService 類別執行個體。</span><span class="sxs-lookup"><span data-stu-id="b3439-252">All of the individual unit tests use this instance of the ContactManagerService class.</span></span>

<span data-ttu-id="b3439-253">清單 1 包含五個對應至每個單元測試的方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-253">Listing 1 contains five methods that correspond to each of the unit tests.</span></span> <span data-ttu-id="b3439-254">每一種方法是以 [TestMethod] 屬性裝飾。</span><span class="sxs-lookup"><span data-stu-id="b3439-254">Each of these methods is decorated with the [TestMethod] attribute.</span></span> <span data-ttu-id="b3439-255">當您執行單元測試時，包含這個屬性的任何方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="b3439-255">When you run the unit tests, any method that has this attribute is called.</span></span> <span data-ttu-id="b3439-256">換句話說，任何以 [TestMethod] 屬性裝飾的方法的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-256">In other words, any method that is decorated with the [TestMethod] attribute is a unit test.</span></span>

<span data-ttu-id="b3439-257">名為 CreateContact()，第一個單元測試會驗證呼叫 CreateContact() 值 true 當時，傳回連絡人類別的有效執行個體傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="b3439-257">The first unit test, named CreateContact(), verifies that calling CreateContact() returns the value true when a valid instance of the Contact class is passed to the method.</span></span> <span data-ttu-id="b3439-258">測試建立連絡人類別的執行個體，呼叫 CreateContact() 方法時，並確認 CreateContact() 傳回值 true。</span><span class="sxs-lookup"><span data-stu-id="b3439-258">The test creates an instance of the Contact class, calls the CreateContact() method, and verifies that CreateContact() returns the value true.</span></span>

<span data-ttu-id="b3439-259">剩餘的測試確認 CreateContact() 方法呼叫含有無效的連絡人時然後方法會傳回 false 的預期的驗證錯誤訊息加入至模型狀態。</span><span class="sxs-lookup"><span data-stu-id="b3439-259">The remaining tests verify that when the CreateContact() method is called with an invalid Contact then the method returns false and the expected validation error message is added to model state.</span></span> <span data-ttu-id="b3439-260">比方說，CreateContactRequiredFirstName() 測試會以空字串的 FirstName 屬性建立連絡人類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="b3439-260">For example, the CreateContactRequiredFirstName() test creates an instance of the Contact class with an empty string for its FirstName property.</span></span> <span data-ttu-id="b3439-261">接下來，CreateContact() 方法稱為含有無效的連絡人。</span><span class="sxs-lookup"><span data-stu-id="b3439-261">Next, the CreateContact() method is called with the invalid Contact.</span></span> <span data-ttu-id="b3439-262">最後，測試會 CreateContact() 傳回 false，而且模型狀態包含預期的驗證錯誤訊息 「 名字 」 所需。</span><span class="sxs-lookup"><span data-stu-id="b3439-262">Finally, the test verifies that CreateContact() returns false and that model state contains the expected validation error message "First name is required."</span></span>

<span data-ttu-id="b3439-263">您也可以選取功能表選項清單 1 中執行單元測試 **，執行測試，（CTRL + R、 A） 的方案中的所有測試**。</span><span class="sxs-lookup"><span data-stu-id="b3439-263">You can run the unit tests in Listing 1 by selecting the menu option **Test, Run, All Tests in Solution (CTRL+R, A)**.</span></span> <span data-ttu-id="b3439-264">測試結果會顯示在 [測試結果] 視窗中 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="b3439-264">The results of the tests are displayed in the Test Results window (see Figure 4).</span></span>


<span data-ttu-id="b3439-265">[![測試結果](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b3439-265">[![Test Results](iteration-5-create-unit-tests-cs/_static/image4.jpg)](iteration-5-create-unit-tests-cs/_static/image7.png)</span></span>

<span data-ttu-id="b3439-266">**圖 04**： 測試結果 ([按一下以檢視完整大小的影像](iteration-5-create-unit-tests-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="b3439-266">**Figure 04**: Test Results ([Click to view full-size image](iteration-5-create-unit-tests-cs/_static/image8.png))</span></span>


## <a name="creating-unit-tests-for-controllers"></a><span data-ttu-id="b3439-267">建立單元測試控制器</span><span class="sxs-lookup"><span data-stu-id="b3439-267">Creating Unit Tests for Controllers</span></span>

<span data-ttu-id="b3439-268">ASP.NETMVC 應用程式會控制流程的使用者互動。</span><span class="sxs-lookup"><span data-stu-id="b3439-268">ASP.NETMVC application control the flow of user interaction.</span></span> <span data-ttu-id="b3439-269">在測試控制器時，您會想要測試是否控制器傳回正確的動作結果，並檢視資料。</span><span class="sxs-lookup"><span data-stu-id="b3439-269">When testing a controller, you want to test whether the controller returns the right action result and view data.</span></span> <span data-ttu-id="b3439-270">您也可能會想要測試是否與以預期的方式的模型類別互動的控制器。</span><span class="sxs-lookup"><span data-stu-id="b3439-270">You also might want to test whether a controller interacts with model classes in the manner expected.</span></span>

<span data-ttu-id="b3439-271">例如，列出 2 包含連絡人控制器 create （） 方法的兩個單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-271">For example, Listing 2 contains two unit tests for the Contact controller Create() method.</span></span> <span data-ttu-id="b3439-272">第一個單元測試會確認有效的連絡人給 create （） 方法，則 create （） 方法將重新導向至索引動作。</span><span class="sxs-lookup"><span data-stu-id="b3439-272">The first unit test verifies that when a valid Contact is passed to the Create() method then the Create() method redirects to the Index action.</span></span> <span data-ttu-id="b3439-273">換句話說，當傳遞有效的連絡人，則 create （） 方法應該傳回 RedirectToRouteResult 代表索引動作。</span><span class="sxs-lookup"><span data-stu-id="b3439-273">In other words, when passed a valid Contact, the Create() method should return a RedirectToRouteResult that represents the Index action.</span></span>

<span data-ttu-id="b3439-274">我們不想要測試 ContactManager 服務層，我們所測試的控制器層時。</span><span class="sxs-lookup"><span data-stu-id="b3439-274">We don t want to test the ContactManager service layer when we are testing the controller layer.</span></span> <span data-ttu-id="b3439-275">因此，我們會模擬服務層的初始化方法中的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b3439-275">Therefore, we mock the service layer with the following code in the Initialize method:</span></span>

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample3.cs)]

<span data-ttu-id="b3439-276">在 CreateValidContact() 單元測試中，我們會模擬呼叫服務層與下列程式碼行 CreateContact() 方法的行為：</span><span class="sxs-lookup"><span data-stu-id="b3439-276">In the CreateValidContact() unit test, we mock the behavior of calling the service layer CreateContact() method with the following line of code:</span></span>

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample4.cs)]

<span data-ttu-id="b3439-277">這行程式碼會導致模擬 ContactManager 服務呼叫其 CreateContact() 方法時，傳回值 true。</span><span class="sxs-lookup"><span data-stu-id="b3439-277">This line of code causes the mock ContactManager service to return the value true when its CreateContact() method is called.</span></span> <span data-ttu-id="b3439-278">藉由模擬服務層，我們可以測試我們控制站的行為，而不需要在服務層中執行的任何程式碼。</span><span class="sxs-lookup"><span data-stu-id="b3439-278">By mocking the service layer, we can test the behavior of our controller without needing to execute any code in the service layer.</span></span>

<span data-ttu-id="b3439-279">第二個單元測試會驗證當無效的連絡人會傳遞至方法時，create （） 動作，傳回建立檢視。</span><span class="sxs-lookup"><span data-stu-id="b3439-279">The second unit test verifies that the Create() action returns the Create view when an invalid contact is passed to the method.</span></span> <span data-ttu-id="b3439-280">我們會造成服務層 CreateContact() 方法傳回的值為 false，且下列程式碼行：</span><span class="sxs-lookup"><span data-stu-id="b3439-280">We cause the service layer CreateContact() method to return the value false with the following line of code:</span></span>

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample5.cs)]

<span data-ttu-id="b3439-281">如果 create （） 方法的行為，如我們所預期它應傳回建立檢視時的服務層會傳回其值為 false。</span><span class="sxs-lookup"><span data-stu-id="b3439-281">If the Create() method behaves as we expect then it should return the Create view when the service layer returns the value false.</span></span> <span data-ttu-id="b3439-282">這樣一來，控制器可顯示驗證錯誤訊息中建立檢視和使用者有機會先行修正該無效的連絡人屬性。</span><span class="sxs-lookup"><span data-stu-id="b3439-282">That way, the controller can display the validation error messages in the Create view and the user has a chance to correct that invalid Contact properties.</span></span>


<span data-ttu-id="b3439-283">如果您計劃要建置單元測試您的控制站您需要從控制器動作傳回明確的檢視表名稱。</span><span class="sxs-lookup"><span data-stu-id="b3439-283">If you plan to build unit tests for your controllers then you need to return explicit view names from your controller actions.</span></span> <span data-ttu-id="b3439-284">例如，不會傳回如下的檢視：</span><span class="sxs-lookup"><span data-stu-id="b3439-284">For example, do not return a view like this:</span></span>

<span data-ttu-id="b3439-285">傳回 View();</span><span class="sxs-lookup"><span data-stu-id="b3439-285">return View();</span></span>

<span data-ttu-id="b3439-286">相反地，傳回的檢視就像這樣：</span><span class="sxs-lookup"><span data-stu-id="b3439-286">Instead, return the view like this:</span></span>

<span data-ttu-id="b3439-287">return View("Create");</span><span class="sxs-lookup"><span data-stu-id="b3439-287">return View("Create");</span></span>

<span data-ttu-id="b3439-288">如果您不明確傳回檢視時 ViewResult.ViewName 屬性會傳回空字串。</span><span class="sxs-lookup"><span data-stu-id="b3439-288">If you are not explicit when returning a view then the ViewResult.ViewName property returns an empty string.</span></span>


<span data-ttu-id="b3439-289">**Listing 2 - Controllers\ContactControllerTest.cs**</span><span class="sxs-lookup"><span data-stu-id="b3439-289">**Listing 2 - Controllers\ContactControllerTest.cs**</span></span>

[!code-csharp[Main](iteration-5-create-unit-tests-cs/samples/sample6.cs)]

## <a name="summary"></a><span data-ttu-id="b3439-290">總結</span><span class="sxs-lookup"><span data-stu-id="b3439-290">Summary</span></span>

<span data-ttu-id="b3439-291">在這個反覆項目，我們會建立單元測試連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="b3439-291">In this iteration, we created unit tests for our Contact Manager application.</span></span> <span data-ttu-id="b3439-292">我們可以在任何時間，以確認我們的應用程式仍然運作我們所預期的方式執行這些單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-292">We can run these unit tests at any time to verify that our application still behaves in the manner that we expect.</span></span> <span data-ttu-id="b3439-293">單元測試做為應用程式讓我們來安全地修改應用程式在未來的安全網。</span><span class="sxs-lookup"><span data-stu-id="b3439-293">The unit tests act as a safety net for our application enabling us to safely modify our application in the future.</span></span>

<span data-ttu-id="b3439-294">我們建立兩個集合的單元測試。</span><span class="sxs-lookup"><span data-stu-id="b3439-294">We created two sets of unit tests.</span></span> <span data-ttu-id="b3439-295">首先，我們會透過建立單元測試，我們的服務層的測試我們驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="b3439-295">First, we tested our validation logic by creating unit tests for our service layer.</span></span> <span data-ttu-id="b3439-296">接下來，我們會透過建立單元測試，我們的控制器層的測試我們的流程控制邏輯。</span><span class="sxs-lookup"><span data-stu-id="b3439-296">Next, we tested our flow control logic by creating unit tests for our controller layer.</span></span> <span data-ttu-id="b3439-297">在測試我們的服務層時，我們隔離我們的測試為我們的服務層從我們的儲存機制層模擬我們的儲存機制層。</span><span class="sxs-lookup"><span data-stu-id="b3439-297">When testing our service layer, we isolated our tests for our service layer from our repository layer by mocking our repository layer.</span></span> <span data-ttu-id="b3439-298">在測試控制器的圖層時，我們隔離我們的測試我們控制器層級的模擬服務層。</span><span class="sxs-lookup"><span data-stu-id="b3439-298">When testing the controller layer, we isolated our tests for our controller layer by mocking the service layer.</span></span>

<span data-ttu-id="b3439-299">中的下一個反覆項目中，我們會修改連絡人管理員應用程式，使它支援連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="b3439-299">In the next iteration, we modify the Contact Manager application so that it supports Contact Groups.</span></span> <span data-ttu-id="b3439-300">我們會將這項新功能新增至我們的應用程式使用稱為 「 測試驅動開發軟體設計程序。</span><span class="sxs-lookup"><span data-stu-id="b3439-300">We'll add this new functionality to our application using a software design process called test-driven development.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b3439-301">[上一頁](iteration-4-make-the-application-loosely-coupled-cs.md)
> [下一頁](iteration-6-use-test-driven-development-cs.md)</span><span class="sxs-lookup"><span data-stu-id="b3439-301">[Previous](iteration-4-make-the-application-loosely-coupled-cs.md)
[Next](iteration-6-use-test-driven-development-cs.md)</span></span>
