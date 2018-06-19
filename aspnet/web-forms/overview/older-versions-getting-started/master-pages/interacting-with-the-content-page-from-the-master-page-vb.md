---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: 互動內容的頁面上，從主版頁面 (VB) |Microsoft 文件
author: rick-anderson
description: 會檢查呼叫方法、 從主版頁面中的程式碼中設定屬性的 [內容] 頁面等等的方式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/11/2008
ms.topic: article
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 9274924b441cb21e33eb57de06ff374428fa036b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889471"
---
<a name="interacting-with-the-content-page-from-the-master-page-vb"></a><span data-ttu-id="98927-103">互動內容的頁面上，從主版頁面 (VB)</span><span class="sxs-lookup"><span data-stu-id="98927-103">Interacting with the Content Page from the Master Page (VB)</span></span>
====================
<span data-ttu-id="98927-104">由[Scott Mitchell](https://twitter.com/ScottOnWriting)</span><span class="sxs-lookup"><span data-stu-id="98927-104">by [Scott Mitchell](https://twitter.com/ScottOnWriting)</span></span>

<span data-ttu-id="98927-105">[下載程式碼](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip)或[下載 PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="98927-105">[Download Code](http://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) or [Download PDF](http://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)</span></span>

> <span data-ttu-id="98927-106">會檢查呼叫方法、 從主版頁面中的程式碼中設定屬性的 [內容] 頁面等等的方式。</span><span class="sxs-lookup"><span data-stu-id="98927-106">Examines how to call methods, set properties, etc. of the Content Page from code in the Master Page.</span></span>


## <a name="introduction"></a><span data-ttu-id="98927-107">簡介</span><span class="sxs-lookup"><span data-stu-id="98927-107">Introduction</span></span>

<span data-ttu-id="98927-108">前述教學課程會檢查如何以程式設計方式互動其主版頁面的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-108">The preceding tutorial examined how to have the content page programmatically interact with its master page.</span></span> <span data-ttu-id="98927-109">我們已更新包括列出五個最近的 GridView 控制項的主版頁面重新叫用加入產品。</span><span class="sxs-lookup"><span data-stu-id="98927-109">Recall that we updated the master page to include a GridView control that listed the five most recently added products.</span></span> <span data-ttu-id="98927-110">然後，我們會建立使用者無法加入新的產品內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-110">We then created a content page from which the user could add a new product.</span></span> <span data-ttu-id="98927-111">時加入新的產品，以指示主版頁面重新整理其 GridView，使它可以包含剛加入的產品所需內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-111">Upon adding a new product, the content page needed to instruct the master page to refresh its GridView so that it would include the just-added product.</span></span> <span data-ttu-id="98927-112">將公用方法加入至主版頁面，重新整理資料繫結至 GridView，，然後叫用該方法，從 [內容] 頁面，被完成這項功能。</span><span class="sxs-lookup"><span data-stu-id="98927-112">This functionality was accomplished by adding a public method to the master page that refreshed the data bound to the GridView, and then invoking that method from the content page.</span></span>

<span data-ttu-id="98927-113">最常見的內容和主版頁面互動形式來自內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-113">The most common form of content and master page interaction originates from the content page.</span></span> <span data-ttu-id="98927-114">不過，主版頁面 rouse 目前的內容頁面，為動作，可能會而且可能需要這類功能，如果主版頁面包含讓使用者修改資料，也會顯示在 [內容] 頁面上的使用者介面項目。</span><span class="sxs-lookup"><span data-stu-id="98927-114">However, it is possible for the master page to rouse the current content page into action, and such functionality may be needed if the master page contains user interface elements that enable users to modify data that is also displayed on the content page.</span></span> <span data-ttu-id="98927-115">請考慮內容頁面，顯示產品中的資訊 GridView 控制項和主版頁面，其中包含按鈕控制項，按下時，會將所有產品的標價加倍。</span><span class="sxs-lookup"><span data-stu-id="98927-115">Consider a content page that displays the products information in a GridView control and a master page that includes a Button control that, when clicked, doubles the prices of all products.</span></span> <span data-ttu-id="98927-116">如同前述的教學課程中的範例，需要重新整理之後，使其顯示新的價格，按下按鈕的 double 價格 GridView 但在此案例中必須為動作 rouse 內容頁面的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-116">Much like the example in the preceding tutorial, the GridView needs to be refreshed after the double price Button is clicked so that it displays the new prices, but in this scenario it's the master page that needs to rouse the content page into action.</span></span>

<span data-ttu-id="98927-117">本教學課程會探討如何叫用功能的內容頁面中定義的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-117">This tutorial explores how to have the master page invoke functionality defined in the content page.</span></span>

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a><span data-ttu-id="98927-118">透過事件和事件處理常式進行程式設計互動</span><span class="sxs-lookup"><span data-stu-id="98927-118">Instigating Programmatic Interaction via an Event and Event Handlers</span></span>

<span data-ttu-id="98927-119">叫用從主版頁面的內容頁面功能會比其他方式的更具有挑戰性。</span><span class="sxs-lookup"><span data-stu-id="98927-119">Invoking content page functionality from a master page is more challenging than the other way around.</span></span> <span data-ttu-id="98927-120">因為內容頁面時進行程式設計的互動，舉凡內容頁面，具有單一主版頁面中，我們知道公用方法和屬性在我們處置時。</span><span class="sxs-lookup"><span data-stu-id="98927-120">Because a content page has a single master page, when instigating the programmatic interaction from the content page we know what public methods and properties are at our disposal.</span></span> <span data-ttu-id="98927-121">主版頁面中，不過，可以有許多不同內容頁面，每個都有它自己的屬性和方法集。</span><span class="sxs-lookup"><span data-stu-id="98927-121">A master page, however, can have many different content pages, each with its own set of properties and methods.</span></span> <span data-ttu-id="98927-122">如何，然後可以我們中撰寫程式碼以其內容頁面中執行某些動作，當我們不知道哪些內容的頁面將會叫用，直到執行階段時的主版頁面？</span><span class="sxs-lookup"><span data-stu-id="98927-122">How, then, can we write code in the master page to perform some action in its content page when we don't know what content page will be invoked until runtime?</span></span>

<span data-ttu-id="98927-123">請考慮的 ASP.NET Web 控制項，例如按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="98927-123">Consider an ASP.NET Web control, such as the Button control.</span></span> <span data-ttu-id="98927-124">按鈕控制項可出現在任意數目的 ASP.NET 網頁，且需要某種機制，它可以警示 頁面，已按一下。</span><span class="sxs-lookup"><span data-stu-id="98927-124">A Button control can appear on any number of ASP.NET pages and needs a mechanism by which it can alert the page that it has been clicked.</span></span> <span data-ttu-id="98927-125">這利用完成*事件*。</span><span class="sxs-lookup"><span data-stu-id="98927-125">This is accomplished using *events*.</span></span> <span data-ttu-id="98927-126">按鈕控制項特別的是，引發其`Click`事件按一下時其; 包含按鈕的 ASP.NET 網頁 （選擇性） 可回應透過該通知*事件處理常式*。</span><span class="sxs-lookup"><span data-stu-id="98927-126">In particular, the Button control raises its `Click` event when it is clicked; the ASP.NET page that contains the Button can optionally respond to that notification via an *event handler*.</span></span>

<span data-ttu-id="98927-127">這個相同的模式可以用來在其內容頁的主版頁面的觸發程序功能：</span><span class="sxs-lookup"><span data-stu-id="98927-127">This same pattern can be used to have a master page trigger functionality in its content pages:</span></span>

1. <span data-ttu-id="98927-128">將事件加入至主版頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-128">Add an event to the master page.</span></span>
2. <span data-ttu-id="98927-129">引發事件時的主版頁面需要其內容的頁面與通訊。</span><span class="sxs-lookup"><span data-stu-id="98927-129">Raise the event whenever the master page needs to communicate with its content page.</span></span> <span data-ttu-id="98927-130">例如，如果主版頁面需要提醒其內容的頁面使用者加倍價格，其事件就會引發價格超過一倍之後，立即。</span><span class="sxs-lookup"><span data-stu-id="98927-130">For example, if the master page needs to alert its content page that the user has doubled the prices, its event would be raised immediately after the prices have been doubled.</span></span>
3. <span data-ttu-id="98927-131">建立事件處理常式中採取某些動作需要這些內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-131">Create an event handler in those content pages that need to take some action.</span></span>

<span data-ttu-id="98927-132">本教學課程的這個其餘部分實作簡介; 中所述的範例也就是內容頁面，列出資料庫中的產品和包含按鈕的主版頁面控制價格增加兩倍。</span><span class="sxs-lookup"><span data-stu-id="98927-132">This remainder of this tutorial implements the example outlined in the Introduction; namely, a content page that lists the products in the database and a master page that includes a Button control to double the prices.</span></span>

## <a name="step-1-displaying-products-in-a-content-page"></a><span data-ttu-id="98927-133">步驟 1： 在內容頁面中顯示的產品</span><span class="sxs-lookup"><span data-stu-id="98927-133">Step 1: Displaying Products in a Content Page</span></span>

<span data-ttu-id="98927-134">我們第一件事是建立列出的產品，從 Northwind 資料庫的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-134">Our first order of business is to create a content page that lists the products from the Northwind database.</span></span> <span data-ttu-id="98927-135">(我們在先前的教學課程中，加入至專案 Northwind 資料庫[*互動主版頁面內容的頁面從*](interacting-with-the-master-page-from-the-content-page-vb.md)。)藉由新增新的 ASP.NET 網頁的開始`~/Admin`資料夾名為`Products.aspx`，務必將它繫結讓`Site.master`主版頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-135">(We added the Northwind database to the project in the preceding tutorial, [*Interacting with the Master Page from the Content Page*](interacting-with-the-master-page-from-the-content-page-vb.md).) Start by adding a new ASP.NET page to the `~/Admin` folder named `Products.aspx`, making sure to bind it to the `Site.master` master page.</span></span> <span data-ttu-id="98927-136">此頁面已加入至網站之後，圖 1 顯示 [方案總管]。</span><span class="sxs-lookup"><span data-stu-id="98927-136">Figure 1 shows the Solution Explorer after this page has been added to the website.</span></span>


<span data-ttu-id="98927-137">[![將新的 ASP.NET 網頁新增至 [Admin] 資料夾](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="98927-137">[![Add a New ASP.NET Page to the Admin Folder](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)</span></span>

<span data-ttu-id="98927-138">**圖 01**： 加入新的 ASP.NET 網頁的`Admin`資料夾 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="98927-138">**Figure 01**: Add a New ASP.NET Page to the `Admin` Folder ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))</span></span>


<span data-ttu-id="98927-139">請記得，在[*主版頁面中指定的標題、 Meta 標記和其他 HTML 標頭*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md)教學課程中我們建立名為基礎的自訂頁面類別`BasePage`如果不是產生頁面的標題明確地設定。</span><span class="sxs-lookup"><span data-stu-id="98927-139">Recall that in the [*Specifying the Title, Meta Tags, and Other HTML Headers in the Master Page*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) tutorial we created a custom base page class named `BasePage` that generates the page's title if it is not explicitly set.</span></span> <span data-ttu-id="98927-140">移至`Products.aspx`網頁的程式碼後置類別，並讓它衍生自`BasePage`(而不是從`System.Web.UI.Page`)。</span><span class="sxs-lookup"><span data-stu-id="98927-140">Go to the `Products.aspx` page's code-behind class and have it derive from `BasePage` (instead of from `System.Web.UI.Page`).</span></span>

<span data-ttu-id="98927-141">最後，更新`Web.sitemap`檔案至這一課中加入一個項目。</span><span class="sxs-lookup"><span data-stu-id="98927-141">Finally, update the `Web.sitemap` file to include an entry for this lesson.</span></span> <span data-ttu-id="98927-142">加入下列標記下方`<siteMapNode>`主版頁面互動課程內容：</span><span class="sxs-lookup"><span data-stu-id="98927-142">Add the following markup beneath the `<siteMapNode>` for the Content to Master Page Interaction lesson:</span></span>


[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

<span data-ttu-id="98927-143">這個加法`<siteMapNode>`項目會反映在課程清單 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="98927-143">The addition of this `<siteMapNode>` element is reflected in the Lessons list (see Figure 5).</span></span>

<span data-ttu-id="98927-144">返回`Products.aspx`。</span><span class="sxs-lookup"><span data-stu-id="98927-144">Return to `Products.aspx`.</span></span> <span data-ttu-id="98927-145">在內容控制項中`MainContent`、 新增 GridView 控制項並將其命名`ProductsGrid`。</span><span class="sxs-lookup"><span data-stu-id="98927-145">In the Content control for `MainContent`, add a GridView control and name it `ProductsGrid`.</span></span> <span data-ttu-id="98927-146">將 GridView 繫結至新的 SqlDataSource 控制項，名為`ProductsDataSource`。</span><span class="sxs-lookup"><span data-stu-id="98927-146">Bind the GridView to a new SqlDataSource control named `ProductsDataSource`.</span></span>


<span data-ttu-id="98927-147">[![將 GridView 繫結至新的 SqlDataSource 控制項](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="98927-147">[![Bind the GridView to a New SqlDataSource Control](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)</span></span>

<span data-ttu-id="98927-148">**圖 02**： 將 GridView 繫結至新的 SqlDataSource 控制項 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="98927-148">**Figure 02**: Bind the GridView to a New SqlDataSource Control  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))</span></span>


<span data-ttu-id="98927-149">設定精靈，使其使用 Northwind 資料庫。</span><span class="sxs-lookup"><span data-stu-id="98927-149">Configure the wizard so that it uses the Northwind database.</span></span> <span data-ttu-id="98927-150">如果您已完成上一個教學課程，則您應該已經有名稱為連接字串`NorthwindConnectionString`中`Web.config`。</span><span class="sxs-lookup"><span data-stu-id="98927-150">If you worked through the previous tutorial then you should already have a connection string named `NorthwindConnectionString` in `Web.config`.</span></span> <span data-ttu-id="98927-151">圖 3 所示，從下拉式清單中，選擇此連接字串。</span><span class="sxs-lookup"><span data-stu-id="98927-151">Choose this connection string from the drop-down list, as shown in Figure 3.</span></span>


<span data-ttu-id="98927-152">[![設定為使用 Northwind 資料庫 SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="98927-152">[![Configure the SqlDataSource to Use the Northwind Database](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)</span></span>

<span data-ttu-id="98927-153">**圖 03**： 設定為使用 Northwind 資料庫 SqlDataSource ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="98927-153">**Figure 03**: Configure the SqlDataSource to Use the Northwind Database  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))</span></span>


<span data-ttu-id="98927-154">接下來，指定資料來源控制項的`SELECT`陳述式，藉由從下拉式清單中選擇 [產品] 資料表，並傳回`ProductName`和`UnitPrice`（請參閱圖 4） 的資料行。</span><span class="sxs-lookup"><span data-stu-id="98927-154">Next, specify the data source control's `SELECT` statement by choosing the Products table from the drop-down list and returning the `ProductName` and `UnitPrice` columns (see Figure 4).</span></span> <span data-ttu-id="98927-155">按一下 [下一步]，然後 「 完成 」 以完成設定資料來源精靈。</span><span class="sxs-lookup"><span data-stu-id="98927-155">Click Next and then Finish to complete the Configure Data Source wizard.</span></span>


<span data-ttu-id="98927-156">[![傳回從 Products 資料表的 [ProductName] 和 UnitPrice 欄位](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="98927-156">[![Return the ProductName and UnitPrice Fields from the Products Table](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)</span></span>

<span data-ttu-id="98927-157">**圖 04**： 傳回`ProductName`和`UnitPrice`欄位從`Products`資料表 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="98927-157">**Figure 04**: Return the `ProductName` and `UnitPrice` Fields from the `Products` Table  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))</span></span>


<span data-ttu-id="98927-158">這就是這麼簡單 ！</span><span class="sxs-lookup"><span data-stu-id="98927-158">That's all there is to it!</span></span> <span data-ttu-id="98927-159">完成精靈之後 Visual Studio 會將兩個 BoundFields 加入 GridView 鏡像 SqlDataSource 控制項所傳回的兩個欄位。</span><span class="sxs-lookup"><span data-stu-id="98927-159">After completing the wizard Visual Studio adds two BoundFields to the GridView to mirror the two fields returned by the SqlDataSource control.</span></span> <span data-ttu-id="98927-160">遵循 GridView 和 SqlDataSource 控制項的標記。</span><span class="sxs-lookup"><span data-stu-id="98927-160">The GridView and SqlDataSource controls' markup follows.</span></span> <span data-ttu-id="98927-161">圖 5 顯示透過瀏覽器檢視時的結果。</span><span class="sxs-lookup"><span data-stu-id="98927-161">Figure 5 shows the results when viewed through a browser.</span></span>


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]


<span data-ttu-id="98927-162">[![每個產品而其價格則列於 GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="98927-162">[![Each Product and its Price is Listed in the GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)</span></span>

<span data-ttu-id="98927-163">**圖 05**: GridView 列出每個產品及其價格 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))</span><span class="sxs-lookup"><span data-stu-id="98927-163">**Figure 05**: Each Product and its Price is Listed in the GridView  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))</span></span>


> [!NOTE]
> <span data-ttu-id="98927-164">請隨意清除 GridView 的外觀。</span><span class="sxs-lookup"><span data-stu-id="98927-164">Feel free to clean up the appearance of the GridView.</span></span> <span data-ttu-id="98927-165">某些建議包括格式化為貨幣顯示的 UnitPrice 值和使用背景色彩和字型，以改善格線的外觀。</span><span class="sxs-lookup"><span data-stu-id="98927-165">Some suggestions include formatting the displayed UnitPrice value as a currency and using background colors and fonts to improve the grid's appearance.</span></span> <span data-ttu-id="98927-166">如需有關顯示和格式化資料在 ASP.NET 中的詳細資訊，請參閱我[使用資料的教學課程系列](../../data-access/index.md)。</span><span class="sxs-lookup"><span data-stu-id="98927-166">For more information on displaying and formatting data in ASP.NET, refer to my [Working with Data tutorial series](../../data-access/index.md).</span></span>


## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a><span data-ttu-id="98927-167">步驟 2： 將雙重價格按鈕加入至主版頁面</span><span class="sxs-lookup"><span data-stu-id="98927-167">Step 2: Adding a Double Prices Button to the Master Page</span></span>

<span data-ttu-id="98927-168">下一步是要加入按鈕 Web 控制項至主要頁面上，按一下時，將雙資料庫中的所有產品的價格。</span><span class="sxs-lookup"><span data-stu-id="98927-168">Our next task is to add a Button Web control to the master page that, when clicked, will double the price of all products in the database.</span></span> <span data-ttu-id="98927-169">開啟`Site.master`主版頁面，並從工具箱拖曳至設計工具中，將它下方拖曳一個按鈕`RecentProductsDataSource`SqlDataSource 控制項，我們在上一個教學課程中加入。</span><span class="sxs-lookup"><span data-stu-id="98927-169">Open the `Site.master` master page and drag a Button from the Toolbox onto the Designer, placing it beneath the `RecentProductsDataSource` SqlDataSource control we added in the previous tutorial.</span></span> <span data-ttu-id="98927-170">設定按鈕的`ID`屬性`DoublePrice`及其`Text`"雙引號產品價格 」 的屬性。</span><span class="sxs-lookup"><span data-stu-id="98927-170">Set the Button's `ID` property to `DoublePrice` and its `Text` property to "Double Product Prices".</span></span>

<span data-ttu-id="98927-171">接下來，將 SqlDataSource 控制項加入主版頁面上，其命名為`DoublePricesDataSource`。</span><span class="sxs-lookup"><span data-stu-id="98927-171">Next, add a SqlDataSource control to the master page, naming it `DoublePricesDataSource`.</span></span> <span data-ttu-id="98927-172">將用來執行此 SqlDataSource`UPDATE`按兩下所有價格的陳述式。</span><span class="sxs-lookup"><span data-stu-id="98927-172">This SqlDataSource will be used to execute the `UPDATE` statement to double all prices.</span></span> <span data-ttu-id="98927-173">具體來說，所以我們需要將其`ConnectionString`和`UpdateCommand`屬性，以適當的連接字串和`UPDATE`陳述式。</span><span class="sxs-lookup"><span data-stu-id="98927-173">Specifically, we need to set its `ConnectionString` and `UpdateCommand` properties to the appropriate connection string and `UPDATE` statement.</span></span> <span data-ttu-id="98927-174">然後我們需要呼叫此 SqlDataSource 控制項`Update`方法時`DoublePrice`按鈕。</span><span class="sxs-lookup"><span data-stu-id="98927-174">Then we need to call this SqlDataSource control's `Update` method when the `DoublePrice` Button is clicked.</span></span> <span data-ttu-id="98927-175">若要設定`ConnectionString`和`UpdateCommand`屬性，選取 SqlDataSource 控制項，然後移至 [屬性] 視窗。</span><span class="sxs-lookup"><span data-stu-id="98927-175">To set the `ConnectionString` and `UpdateCommand` properties, select the SqlDataSource control and then go to the Properties window.</span></span> <span data-ttu-id="98927-176">`ConnectionString`屬性會列出已儲存在這些連接字串`Web.config`在下拉式清單中，選擇`NorthwindConnectionString`選項圖 6 所示。</span><span class="sxs-lookup"><span data-stu-id="98927-176">The `ConnectionString` property lists those connection strings already stored in `Web.config` in a drop-down list; choose the `NorthwindConnectionString` option as shown in Figure 6.</span></span>


<span data-ttu-id="98927-177">[![設定要使用 NorthwindConnectionString SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)</span><span class="sxs-lookup"><span data-stu-id="98927-177">[![Configure the SqlDataSource to Use the NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)</span></span>

<span data-ttu-id="98927-178">**圖 06**： 設定要使用 SqlDataSource `NorthwindConnectionString` ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))</span><span class="sxs-lookup"><span data-stu-id="98927-178">**Figure 06**: Configure the SqlDataSource to Use the `NorthwindConnectionString` ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))</span></span>


<span data-ttu-id="98927-179">若要設定`UpdateCommand`屬性，在 屬性 視窗中，尋找 UpdateQuery 選項。</span><span class="sxs-lookup"><span data-stu-id="98927-179">To set the `UpdateCommand` property, locate the UpdateQuery option in the Properties window.</span></span> <span data-ttu-id="98927-180">選取時，這個屬性會顯示省略符號; 的按鈕按一下此按鈕即可顯示命令及參數編輯器對話方塊圖 7 所示。</span><span class="sxs-lookup"><span data-stu-id="98927-180">This property, when selected, displays a button with ellipses; click this button to display the Command and Parameter Editor dialog box shown in Figure 7.</span></span> <span data-ttu-id="98927-181">輸入下列命令`UPDATE`到對話方塊的文字方塊中的陳述式：</span><span class="sxs-lookup"><span data-stu-id="98927-181">Type the following `UPDATE` statement into the dialog box's textbox:</span></span>


[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

<span data-ttu-id="98927-182">此陳述式，在執行時，將會加倍`UnitPrice`值中的每一筆記錄`Products`資料表。</span><span class="sxs-lookup"><span data-stu-id="98927-182">This statement, when executed, will double the `UnitPrice` value for each record in the `Products` table.</span></span>


<span data-ttu-id="98927-183">[![設定 SqlDataSource 的 UpdateCommand 屬性](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="98927-183">[![Set SqlDataSource's UpdateCommand Property](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)</span></span>

<span data-ttu-id="98927-184">**圖 07**： 設定 SqlDataSource`UpdateCommand`屬性 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))</span><span class="sxs-lookup"><span data-stu-id="98927-184">**Figure 07**: Set SqlDataSource's `UpdateCommand` Property  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))</span></span>


<span data-ttu-id="98927-185">之後設定這些屬性，您按鈕和 SqlDataSource 控制項的宣告式標記看起來應該如下所示：</span><span class="sxs-lookup"><span data-stu-id="98927-185">After setting these properties, your Button and SqlDataSource controls' declarative markup should look similar to the following:</span></span>


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

<span data-ttu-id="98927-186">剩下的只有呼叫其`Update`方法時`DoublePrice`按鈕。</span><span class="sxs-lookup"><span data-stu-id="98927-186">All that remains is to call its `Update` method when the `DoublePrice` Button is clicked.</span></span> <span data-ttu-id="98927-187">建立`Click`事件處理常式`DoublePrice`按鈕並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="98927-187">Create a `Click` event handler for the `DoublePrice` Button and add the following code:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

<span data-ttu-id="98927-188">若要測試這項功能，請瀏覽`~/Admin/Products.aspx`頁面，我們在步驟 1 中建立，並按一下 「 雙重產品價格 」 按鈕。</span><span class="sxs-lookup"><span data-stu-id="98927-188">To test this functionality, visit the `~/Admin/Products.aspx` page we created in Step 1 and click the "Double Product Prices" button.</span></span> <span data-ttu-id="98927-189">按一下按鈕導致回傳，並執行`DoublePrice`按鈕的`Click`事件處理常式，使所有產品的價格加倍。</span><span class="sxs-lookup"><span data-stu-id="98927-189">Clicking the button causes a postback and executes the `DoublePrice` Button's `Click` event handler, doubling the prices of all products.</span></span> <span data-ttu-id="98927-190">然後重新轉譯頁面標記會傳回並重新顯示在瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="98927-190">The page is then re-rendered and the markup is returned and re-displayed in the browser.</span></span> <span data-ttu-id="98927-191">「 雙引號產品價格 」 之前 button 已按下 GridView 在內容頁面中，不過，列出相同的價格。</span><span class="sxs-lookup"><span data-stu-id="98927-191">The GridView in the content page, however, lists the same prices as before the "Double Product Prices" button was clicked.</span></span> <span data-ttu-id="98927-192">這是因為一開始載入在 GridView 的資料儲存在檢視狀態，因此除非另有指示否則，它不會在回傳時載入其狀態。</span><span class="sxs-lookup"><span data-stu-id="98927-192">This is because the data initially loaded in the GridView had its state stored in view state, so it's not reloaded on postbacks unless instructed otherwise.</span></span> <span data-ttu-id="98927-193">如果您瀏覽不同的頁面，然後返回`~/Admin/Products.aspx`頁面，您會看到更新的價格。</span><span class="sxs-lookup"><span data-stu-id="98927-193">If you visit a different page and then return to the `~/Admin/Products.aspx` page you'll see the updated prices.</span></span>

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a><span data-ttu-id="98927-194">引發事件時的價格被重複步驟 3:</span><span class="sxs-lookup"><span data-stu-id="98927-194">Step 3: Raising an Event When the Prices are Doubled</span></span>

<span data-ttu-id="98927-195">因為在 GridView`~/Admin/Products.aspx`頁面不會立即反映價格加倍，使用者可能會理解認為他們沒有按一下"雙引號產品價格 」 按鈕，或無法運作。</span><span class="sxs-lookup"><span data-stu-id="98927-195">Because the GridView in the `~/Admin/Products.aspx` page does not immediately reflect the price doubling, a user may understandably think that they did not click the "Double Product Prices" button, or that it didn't work.</span></span> <span data-ttu-id="98927-196">它們可能嘗試按一下按鈕更多時間，使價格加倍，一再重複。</span><span class="sxs-lookup"><span data-stu-id="98927-196">They may try clicking the button a few more times, doubling the prices again and again.</span></span> <span data-ttu-id="98927-197">若要修正此我們必須將內容中的方格頁面後，顯示新價格立即它們加倍。</span><span class="sxs-lookup"><span data-stu-id="98927-197">To fix this we need to have the grid in the content page display the new prices immediately after they are doubled.</span></span>

<span data-ttu-id="98927-198">如稍早在本教學課程中所討論，我們要引發事件主版頁面中的，每當使用者按一下`DoublePrice` 按鈕。</span><span class="sxs-lookup"><span data-stu-id="98927-198">As discussed earlier in this tutorial, we need to raise an event in the master page whenever the user clicks the `DoublePrice` Button.</span></span> <span data-ttu-id="98927-199">事件是一個類別 （事件發行者） 的方式通知另一組有趣的東西會發生其他類別 （事件訂閱者）。</span><span class="sxs-lookup"><span data-stu-id="98927-199">An event is a way for one class (an event publisher) to notify another a set of other classes (the event subscribers) that something interesting has occurred.</span></span> <span data-ttu-id="98927-200">在此範例中，主版頁面是 「 事件發行者 」。這些內容頁面關心時`DoublePrice`按鈕是 「 訂閱者 」。</span><span class="sxs-lookup"><span data-stu-id="98927-200">In this example, the master page is the event publisher; those content pages that care about when the `DoublePrice` Button is clicked are the subscribers.</span></span>

<span data-ttu-id="98927-201">藉由建立事件訂閱類別*事件處理常式*，這是執行以回應所引發的事件的方法。</span><span class="sxs-lookup"><span data-stu-id="98927-201">A class subscribes to an event by creating an *event handler*, which is a method that is executed in response to the event being raised.</span></span> <span data-ttu-id="98927-202">「 發行者 」 定義他藉由定義所引發的事件*事件委派*。</span><span class="sxs-lookup"><span data-stu-id="98927-202">The publisher defines the events he raises by defining an *event delegate*.</span></span> <span data-ttu-id="98927-203">事件委派指定的事件處理常式必須接受輸入的參數。</span><span class="sxs-lookup"><span data-stu-id="98927-203">The event delegate specifies what input parameters the event handler must accept.</span></span> <span data-ttu-id="98927-204">在.NET Framework 中，事件委派執行不傳回任何值，並接受兩個輸入的參數：</span><span class="sxs-lookup"><span data-stu-id="98927-204">In the .NET Framework, event delegates do not return any value and accept two input parameters:</span></span>

- <span data-ttu-id="98927-205">`Object`，其可識別事件來源，並</span><span class="sxs-lookup"><span data-stu-id="98927-205">An `Object`, which identifies the event source, and</span></span>
- <span data-ttu-id="98927-206">衍生自的類別 `System.EventArgs`</span><span class="sxs-lookup"><span data-stu-id="98927-206">A class derived from `System.EventArgs`</span></span>

<span data-ttu-id="98927-207">傳遞至事件處理常式的第二個參數可以包含其他事件的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="98927-207">The second parameter passed to an event handler can include additional information about the event.</span></span> <span data-ttu-id="98927-208">雖然基底`EventArgs`沿著任何資訊未通過類別，.NET Framework 包含一些類別會擴充`EventArgs`並包含額外的屬性。</span><span class="sxs-lookup"><span data-stu-id="98927-208">While the base `EventArgs` class does not pass along any information, the .NET Framework includes a number of classes that extend `EventArgs` and encompass additional properties.</span></span> <span data-ttu-id="98927-209">例如，`CommandEventArgs`執行個體傳遞至回應的事件處理常式`Command`事件，並包含兩個參考的屬性：`CommandArgument`和`CommandName`。</span><span class="sxs-lookup"><span data-stu-id="98927-209">For example, a `CommandEventArgs` instance is passed to event handlers that respond to the `Command` event, and includes two informational properties: `CommandArgument` and `CommandName`.</span></span>

> [!NOTE]
> <span data-ttu-id="98927-210">如需有關建立的詳細資訊，引發，和處理事件，請參閱[事件和委派](https://msdn.microsoft.com/library/17sde2xt.aspx)和[簡單 english 事件委派](http://www.codeproject.com/KB/cs/eventdelegates.aspx)。</span><span class="sxs-lookup"><span data-stu-id="98927-210">For more information on creating, raising, and handling events, see [Events and Delegates](https://msdn.microsoft.com/library/17sde2xt.aspx) and [Event Delegates in Simple English](http://www.codeproject.com/KB/cs/eventdelegates.aspx).</span></span>


<span data-ttu-id="98927-211">若要定義事件，請使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="98927-211">To define an event use the following syntax:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

<span data-ttu-id="98927-212">因為我們只需要時使用者已按一下 [警示內容] 頁面`DoublePrice`按鈕並不需要任何其他資訊一起傳遞，我們可以使用事件委派`EventHandler`，其為第二個定義可接受的事件處理常式參數型別的物件`System.EventArgs`。</span><span class="sxs-lookup"><span data-stu-id="98927-212">Because we only need to alert the content page when the user has clicked the `DoublePrice` Button and do not need to pass along any other additional information, we can use the event delegate `EventHandler`, which defines an event handler that accepts as its second parameter an object of type `System.EventArgs`.</span></span> <span data-ttu-id="98927-213">若要建立事件主版頁面中，將下列程式碼行加入主版頁面的程式碼後置類別：</span><span class="sxs-lookup"><span data-stu-id="98927-213">To create the event in the master page, add the following line of code to the master page's code-behind class:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

<span data-ttu-id="98927-214">上述程式碼會將公用事件加入至名為的主版頁面`PricesDoubled`。</span><span class="sxs-lookup"><span data-stu-id="98927-214">The above code adds a public event to the master page named `PricesDoubled`.</span></span> <span data-ttu-id="98927-215">我們現在需要價格超過一倍之後引發此事件。</span><span class="sxs-lookup"><span data-stu-id="98927-215">We now need to raise this event after the prices have been doubled.</span></span> <span data-ttu-id="98927-216">若要引發事件會使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="98927-216">To raise an event use the following syntax:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

<span data-ttu-id="98927-217">其中*寄件者*和*eventArgs*是您想要傳遞給訂閱者的事件處理常式的值。</span><span class="sxs-lookup"><span data-stu-id="98927-217">Where *sender* and *eventArgs* are the values you want to pass to the subscriber's event handler.</span></span>

<span data-ttu-id="98927-218">更新`DoublePrice``Click`事件處理常式，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="98927-218">Update the `DoublePrice` `Click` event handler with the following code:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

<span data-ttu-id="98927-219">如往常一般，`Click`事件處理常式會呼叫方式啟動`DoublePricesDataSource`SqlDataSource 控制項`Update`方法的所有產品的價格增加兩倍。</span><span class="sxs-lookup"><span data-stu-id="98927-219">As before, the `Click` event handler starts by calling the `DoublePricesDataSource` SqlDataSource control's `Update` method to double the prices of all products.</span></span> <span data-ttu-id="98927-220">之後，有兩個事件處理常式加入項目。</span><span class="sxs-lookup"><span data-stu-id="98927-220">Following that there are two additions to the event handler.</span></span> <span data-ttu-id="98927-221">首先， `RecentProducts` GridView 的資料重新整理。</span><span class="sxs-lookup"><span data-stu-id="98927-221">First, the `RecentProducts` GridView's data is refreshed.</span></span> <span data-ttu-id="98927-222">此 GridView 新增至主版頁面在先前的教學課程中，以及顯示的最新加入的五個產品。</span><span class="sxs-lookup"><span data-stu-id="98927-222">This GridView was added to the master page in the preceding tutorial and displays the five most recently-added products.</span></span> <span data-ttu-id="98927-223">我們需要重新整理此方格，使它顯示這些五項產品只兩倍的價格。</span><span class="sxs-lookup"><span data-stu-id="98927-223">We need to refresh this grid so that it shows the just-doubled prices for these five products.</span></span> <span data-ttu-id="98927-224">接下來，`PricesDoubled`就會引發事件。</span><span class="sxs-lookup"><span data-stu-id="98927-224">Following that, the `PricesDoubled` event is raised.</span></span> <span data-ttu-id="98927-225">主版頁面本身的參考 (`Me`) 會傳送至事件處理常式的事件來源和空白做`EventArgs`物件傳送做為事件引數。</span><span class="sxs-lookup"><span data-stu-id="98927-225">A reference to the master page itself (`Me`) is sent to the event handler as the event source and an empty `EventArgs` object is sent as the event arguments.</span></span>

## <a name="step-4-handling-the-event-in-the-content-page"></a><span data-ttu-id="98927-226">步驟 4： 處理事件的內容頁面</span><span class="sxs-lookup"><span data-stu-id="98927-226">Step 4: Handling the Event in the Content Page</span></span>

<span data-ttu-id="98927-227">主版頁面此時引發其`PricesDoubled`事件每當`DoublePrice`按一下按鈕控制項。</span><span class="sxs-lookup"><span data-stu-id="98927-227">At this point the master page raises its `PricesDoubled` event whenever the `DoublePrice` Button control is clicked.</span></span> <span data-ttu-id="98927-228">不過，這是只有一半-我們仍需要處理 「 訂閱者 」 中的事件。</span><span class="sxs-lookup"><span data-stu-id="98927-228">However, this is only half the battle - we still need to handle the event in the subscriber.</span></span> <span data-ttu-id="98927-229">這牽涉到兩個步驟： 建立事件處理常式，並加入事件配線程式碼，以便在引發事件時執行的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="98927-229">This involves two steps: creating the event handler and adding event wiring code so that when the event is raised the event handler is executed.</span></span>

<span data-ttu-id="98927-230">藉由建立名為事件處理常式開始`Master_PricesDoubled`。</span><span class="sxs-lookup"><span data-stu-id="98927-230">Start by creating an event handler named `Master_PricesDoubled`.</span></span> <span data-ttu-id="98927-231">因為我們的定義方式`PricesDoubled`主版頁面中的事件的事件處理常式的兩個輸入的參數必須為類型`Object`和`EventArgs`分別。</span><span class="sxs-lookup"><span data-stu-id="98927-231">Because of how we defined the `PricesDoubled` event in the master page the event handler's two input parameters must be of types `Object` and `EventArgs`, respectively.</span></span> <span data-ttu-id="98927-232">在事件處理常式呼叫`ProductsGrid`GridView 的`DataBind`重新繫結至資料格資料的方法。</span><span class="sxs-lookup"><span data-stu-id="98927-232">In the event handler call the `ProductsGrid` GridView's `DataBind` method to rebind the data to the grid.</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

<span data-ttu-id="98927-233">此事件處理常式的程式碼已完成，但我們尚未至網路的主版頁面`PricesDoubled`事件，此事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="98927-233">The code for the event handler is complete but we've yet to wire the master page's `PricesDoubled` event to this event handler.</span></span> <span data-ttu-id="98927-234">「 訂閱者 」 纏繞事件之事件處理常式，透過下列語法：</span><span class="sxs-lookup"><span data-stu-id="98927-234">The subscriber wires an event to an event handler via the following syntax:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

<span data-ttu-id="98927-235">*發行者*提供事件的物件參考*eventName*，和*methodName*是在 「 訂閱者 」 中定義的事件處理常式的名稱。</span><span class="sxs-lookup"><span data-stu-id="98927-235">*publisher* is a reference to the object that offers the event *eventName*, and *methodName* is the name of the event handler defined in the subscriber.</span></span>

<span data-ttu-id="98927-236">此事件配線程式碼必須執行上的第一個頁面瀏覽和後續回傳時，而且應該發生在之前可能會引發事件時的頁面生命週期中的點。</span><span class="sxs-lookup"><span data-stu-id="98927-236">This event wiring code must be executed on the first page visit and subsequent postbacks and should occur at a point in the page lifecycle that precedes when the event may be raised.</span></span> <span data-ttu-id="98927-237">加入事件配線程式碼的好時機是在 PreInit 階段中，就會發生頁面生命週期中及早。</span><span class="sxs-lookup"><span data-stu-id="98927-237">A good time to add event wiring code is in the PreInit stage, which occurs very early in the page lifecycle.</span></span>

<span data-ttu-id="98927-238">開啟`~/Admin/Products.aspx`並建立`Page_PreInit`事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="98927-238">Open `~/Admin/Products.aspx` and create a `Page_PreInit` event handler:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

<span data-ttu-id="98927-239">若要完成此連接程式碼中，我們需要從內容頁面以程式設計方式參考主版頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-239">In order to complete this wiring code we need a programmatic reference to the master page from the content page.</span></span> <span data-ttu-id="98927-240">如先前的教學課程中所述，有兩種方式可以執行這項操作：</span><span class="sxs-lookup"><span data-stu-id="98927-240">As noted in the previous tutorial, there are two ways to do this:</span></span>

- <span data-ttu-id="98927-241">透過將轉型鬆散型別`Page.Master`屬性適當的主版頁面的型別，或</span><span class="sxs-lookup"><span data-stu-id="98927-241">By casting the loosely-typed `Page.Master` property to the appropriate master page type, or</span></span>
- <span data-ttu-id="98927-242">藉由新增`@MasterType`指示詞`.aspx`頁面，然後使用 強型別`Master`屬性。</span><span class="sxs-lookup"><span data-stu-id="98927-242">By adding a `@MasterType` directive in the `.aspx` page and then using the strongly-typed `Master` property.</span></span>

<span data-ttu-id="98927-243">我們將使用第二種方法。</span><span class="sxs-lookup"><span data-stu-id="98927-243">Let's use the latter approach.</span></span> <span data-ttu-id="98927-244">加入下列`@MasterType`指示詞，以便在頁面的宣告式標記的頂端：</span><span class="sxs-lookup"><span data-stu-id="98927-244">Add the following `@MasterType` directive to the top of the page's declarative markup:</span></span>


[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

<span data-ttu-id="98927-245">然後加入下列事件配線程式碼中的`Page_PreInit`事件處理常式：</span><span class="sxs-lookup"><span data-stu-id="98927-245">Then add the following event wiring code in the `Page_PreInit` event handler:</span></span>


[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

<span data-ttu-id="98927-246">GridView 內容頁面中的重新整理此位置的程式碼，每當`DoublePrice`按鈕。</span><span class="sxs-lookup"><span data-stu-id="98927-246">With this code in place, the GridView in the content page is refreshed whenever the `DoublePrice` Button is clicked.</span></span>

<span data-ttu-id="98927-247">數字 8 和 9 說明這項行為。</span><span class="sxs-lookup"><span data-stu-id="98927-247">Figures 8 and 9 illustrate this behavior.</span></span> <span data-ttu-id="98927-248">圖 8 顯示當第一次瀏覽的頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-248">Figure 8 shows the page when first visited.</span></span> <span data-ttu-id="98927-249">請注意，在價格值`RecentProducts`GridView （中的主版頁面的左側資料行） 和`ProductsGrid`GridView （在 [內容] 頁面）。</span><span class="sxs-lookup"><span data-stu-id="98927-249">Note that price values in both the `RecentProducts` GridView (in the left column of the master page) and the `ProductsGrid` GridView (in the content page).</span></span> <span data-ttu-id="98927-250">圖 9 顯示相同畫面之後立即`DoublePrice`按下按鈕。</span><span class="sxs-lookup"><span data-stu-id="98927-250">Figure 9 shows the same screen immediately after the `DoublePrice` Button has been clicked.</span></span> <span data-ttu-id="98927-251">如您所見，這兩個 GridViews 會立即反映新的價格。</span><span class="sxs-lookup"><span data-stu-id="98927-251">As you can see, the new prices are instantaneously reflected in both GridViews.</span></span>


<span data-ttu-id="98927-252">[![初始的價格值](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)</span><span class="sxs-lookup"><span data-stu-id="98927-252">[![The Initial Price Values](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)</span></span>

<span data-ttu-id="98927-253">**圖 08**: 價格初始值 ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))</span><span class="sxs-lookup"><span data-stu-id="98927-253">**Figure 08**: The Initial Price Values  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))</span></span>


<span data-ttu-id="98927-254">[![Just-Doubled 價格會顯示在 GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="98927-254">[![The Just-Doubled Prices are Displayed in the GridViews](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)</span></span>

<span data-ttu-id="98927-255">**圖 09**: The Just-Doubled 價格會顯示在 GridViews ([按一下以檢視完整大小的影像](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))</span><span class="sxs-lookup"><span data-stu-id="98927-255">**Figure 09**: The Just-Doubled Prices are Displayed in the GridViews  ([Click to view full-size image](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))</span></span>


## <a name="summary"></a><span data-ttu-id="98927-256">總結</span><span class="sxs-lookup"><span data-stu-id="98927-256">Summary</span></span>

<span data-ttu-id="98927-257">在理想情況下，主版頁面和其內容的頁面是完全獨立的另一個，而且需要互動的任何層級。</span><span class="sxs-lookup"><span data-stu-id="98927-257">Ideally, a master page and its content pages are completely separate from one another and require no level of interaction.</span></span> <span data-ttu-id="98927-258">不過，如果您的主版頁面或顯示的資料，可以進行修改主版頁面或內容頁面的內容頁面，則您可能需要具有警示內容頁面 （或相反的） 主版頁面的資料修改時，這樣可以更新顯示。</span><span class="sxs-lookup"><span data-stu-id="98927-258">However, if you have a master page or content page that displays data that can be modified from the master page or content page, then you may need to have the master page alert the content page (or vice-a-versa) when data is modified so that the display can be updated.</span></span> <span data-ttu-id="98927-259">在前述教學課程中我們了解如何以程式設計方式互動其主版頁面; 內容頁面在此教學課程中我們討論了如何擁有主版頁面起始互動。</span><span class="sxs-lookup"><span data-stu-id="98927-259">In the preceding tutorial we saw how to have a content page programmatically interact with its master page; in this tutorial we looked at how to have a master page initiate the interaction.</span></span>

<span data-ttu-id="98927-260">以程式設計方式之間的內容和主版頁面的互動可以來自於內容或主版頁面中，而使用互動模式取決於原始。</span><span class="sxs-lookup"><span data-stu-id="98927-260">While programmatic interaction between a content and master page can originate from either the content or master page, the interaction pattern used depends on the origination.</span></span> <span data-ttu-id="98927-261">這些差異是因為，內容頁面具有單一主版頁面中，但主版頁面都可以有許多不同的內容頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-261">The differences are due to the fact that a content page has a single master page, but a master page can have many different content pages.</span></span> <span data-ttu-id="98927-262">而不是主版頁面直接與內容的頁面互動，較好的做法是讓引發事件來表示某些動作都已經發生的主版頁面。</span><span class="sxs-lookup"><span data-stu-id="98927-262">Rather than having a master page directly interact with a content page, a better approach is to have the master page raise an event to signal that some action has taken place.</span></span> <span data-ttu-id="98927-263">這些動作有興趣的內容頁面可以建立事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="98927-263">Those content pages that care about the action can create event handlers.</span></span>

<span data-ttu-id="98927-264">祝您程式設計 ！</span><span class="sxs-lookup"><span data-stu-id="98927-264">Happy Programming!</span></span>

### <a name="further-reading"></a><span data-ttu-id="98927-265">進一步閱讀</span><span class="sxs-lookup"><span data-stu-id="98927-265">Further Reading</span></span>

<span data-ttu-id="98927-266">如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="98927-266">For more information on the topics discussed in this tutorial, refer to the following resources:</span></span>

- [<span data-ttu-id="98927-267">存取及更新在 ASP.NET 中的資料</span><span class="sxs-lookup"><span data-stu-id="98927-267">Accessing and Updating Data in ASP.NET</span></span>](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [<span data-ttu-id="98927-268">事件和委派</span><span class="sxs-lookup"><span data-stu-id="98927-268">Events and Delegates</span></span>](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [<span data-ttu-id="98927-269">主版頁面內容之間傳遞資訊</span><span class="sxs-lookup"><span data-stu-id="98927-269">Passing Information Between Content and Master Pages</span></span>](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [<span data-ttu-id="98927-270">在 ASP.NET 教學課程中使用的資料</span><span class="sxs-lookup"><span data-stu-id="98927-270">Working with Data in ASP.NET Tutorials</span></span>](../../data-access/index.md)

### <a name="about-the-author"></a><span data-ttu-id="98927-271">關於作者</span><span class="sxs-lookup"><span data-stu-id="98927-271">About the Author</span></span>

<span data-ttu-id="98927-272">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的多個 ASP/ASP.NET 書籍和 4GuysFromRolla.com 的創辦，目前正在使用 Microsoft Web 技術從 1998 年。</span><span class="sxs-lookup"><span data-stu-id="98927-272">[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), author of multiple ASP/ASP.NET books and founder of 4GuysFromRolla.com, has been working with Microsoft Web technologies since 1998.</span></span> <span data-ttu-id="98927-273">Scott 可做為獨立顧問、 訓練和寫入器。</span><span class="sxs-lookup"><span data-stu-id="98927-273">Scott works as an independent consultant, trainer, and writer.</span></span> <span data-ttu-id="98927-274">他最新的活頁簿[ *Sam 教導您自己 ASP.NET 3.5 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco)。</span><span class="sxs-lookup"><span data-stu-id="98927-274">His latest book is [*Sams Teach Yourself ASP.NET 3.5 in 24 Hours*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco).</span></span> <span data-ttu-id="98927-275">在可到達 Scott [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)或透過在他的部落格[ http://ScottOnWriting.NET ](http://scottonwriting.net/)。</span><span class="sxs-lookup"><span data-stu-id="98927-275">Scott can be reached at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) or via his blog at [http://ScottOnWriting.NET](http://scottonwriting.net/).</span></span>

### <a name="special-thanks-to"></a><span data-ttu-id="98927-276">特別感謝</span><span class="sxs-lookup"><span data-stu-id="98927-276">Special Thanks To</span></span>

<span data-ttu-id="98927-277">許多有用的檢閱者已檢閱本教學課程系列。</span><span class="sxs-lookup"><span data-stu-id="98927-277">This tutorial series was reviewed by many helpful reviewers.</span></span> <span data-ttu-id="98927-278">在此教學課程的前導檢閱者已 Suchi Banerjee。</span><span class="sxs-lookup"><span data-stu-id="98927-278">Lead reviewer for this tutorial was Suchi Banerjee.</span></span> <span data-ttu-id="98927-279">檢閱我即將推出的 MSDN 文件有興趣嗎？</span><span class="sxs-lookup"><span data-stu-id="98927-279">Interested in reviewing my upcoming MSDN articles?</span></span> <span data-ttu-id="98927-280">如果是這樣，卸除我一行 [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)</span><span class="sxs-lookup"><span data-stu-id="98927-280">If so, drop me a line at [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="98927-281">[上一頁](interacting-with-the-master-page-from-the-content-page-vb.md)
> [下一頁](master-pages-and-asp-net-ajax-vb.md)</span><span class="sxs-lookup"><span data-stu-id="98927-281">[Previous](interacting-with-the-master-page-from-the-content-page-vb.md)
[Next](master-pages-and-asp-net-ajax-vb.md)</span></span>
