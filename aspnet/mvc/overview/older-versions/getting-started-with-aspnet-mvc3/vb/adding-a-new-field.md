---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: 將新欄位加入至電影模型和資料庫資料表 (VB) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 ASP.NET MVC Web 應用程式的基本概念...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877316"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a><span data-ttu-id="9291b-103">將新欄位加入至電影模型和資料庫資料表 (VB)</span><span class="sxs-lookup"><span data-stu-id="9291b-103">Adding a New Field to the Movie Model and Database Table (VB)</span></span>
====================
<span data-ttu-id="9291b-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="9291b-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="9291b-105">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9291b-105">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="9291b-106">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="9291b-106">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="9291b-107">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="9291b-107">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="9291b-108">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="9291b-108">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="9291b-109">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="9291b-109">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="9291b-110">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="9291b-110">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="9291b-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="9291b-111">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="9291b-112">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="9291b-112">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="9291b-113">使用本主題隨附在 Visual Web Developer 專案中的使用 VB.NET 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="9291b-113">A Visual Web Developer project with VB.NET source code is available to accompany this topic.</span></span> <span data-ttu-id="9291b-114">[下載 VB.NET 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="9291b-114">[Download the VB.NET version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="9291b-115">如果您偏好 C#，切換至[C# 版本](../cs/adding-a-new-field.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="9291b-115">If you prefer C#, switch to the [C# version](../cs/adding-a-new-field.md) of this tutorial.</span></span>


<span data-ttu-id="9291b-116">本節中您將模型類別來進行一些變更，並了解如何更新資料庫結構描述，以符合模型變更。</span><span class="sxs-lookup"><span data-stu-id="9291b-116">In this section you'll make some changes to the model classes and learn how you can update the database schema to match the model changes.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="9291b-117">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="9291b-117">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="9291b-118">開始透過新增`Rating`屬性至現有`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="9291b-118">Start by adding a new `Rating` property to the existing `Movie` class.</span></span> <span data-ttu-id="9291b-119">開啟*Movie.cs*檔案，然後加入`Rating`屬性如下所示：</span><span class="sxs-lookup"><span data-stu-id="9291b-119">Open the *Movie.cs* file and add the `Rating` property like this one:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

<span data-ttu-id="9291b-120">完整`Movie`類別現在看起來類似下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9291b-120">The complete `Movie` class now looks like the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

<span data-ttu-id="9291b-121">重新編譯應用程式使用**偵錯** &gt;**建置影片**功能表命令。</span><span class="sxs-lookup"><span data-stu-id="9291b-121">Recompile the application using the **Debug** &gt;**Build Movie** menu command.</span></span>

<span data-ttu-id="9291b-122">既然您已更新`Model`類別，您也需要更新*\Views\Movies\Index.vbhtml*和*\Views\Movies\Create.vbhtml*檢視範本，以支援新`Rating`屬性。</span><span class="sxs-lookup"><span data-stu-id="9291b-122">Now that you've updated the `Model` class, you also need to update the *\Views\Movies\Index.vbhtml* and *\Views\Movies\Create.vbhtml* view templates in order to support the new `Rating` property.</span></span>

<span data-ttu-id="9291b-123">開啟<em>\Views\Movies\Index.vbhtml</em>檔案，然後加入`<th>Rating</th>`欄名後方<strong>價格</strong>資料行。</span><span class="sxs-lookup"><span data-stu-id="9291b-123">Open the<em>\Views\Movies\Index.vbhtml</em> file and add a `<th>Rating</th>` column heading just after the <strong>Price</strong> column.</span></span> <span data-ttu-id="9291b-124">然後加入`<td>`即將要呈現之範本的結束欄`@item.Rating`值。</span><span class="sxs-lookup"><span data-stu-id="9291b-124">Then add a `<td>` column near the end of the template to render the `@item.Rating` value.</span></span> <span data-ttu-id="9291b-125">以下是哪些更新<em>Index.vbhtml</em>檢視範本外觀就像：</span><span class="sxs-lookup"><span data-stu-id="9291b-125">Below is what the updated <em>Index.vbhtml</em> view template looks like:</span></span>

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

<span data-ttu-id="9291b-126">接下來，開啟*\Views\Movies\Create.vbhtml*檔案，然後加入下列標記表單的結尾附近。</span><span class="sxs-lookup"><span data-stu-id="9291b-126">Next, open the *\Views\Movies\Create.vbhtml* file and add the following markup near the end of the form.</span></span> <span data-ttu-id="9291b-127">這會呈現文字方塊中，以便建立新的電影時，您可以指定評等。</span><span class="sxs-lookup"><span data-stu-id="9291b-127">This renders a text box so that you can specify a rating when a new movie is created.</span></span>

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a><span data-ttu-id="9291b-128">管理模型和資料庫結構描述的差異</span><span class="sxs-lookup"><span data-stu-id="9291b-128">Managing Model and Database Schema Differences</span></span>

<span data-ttu-id="9291b-129">您現在已更新以支援新的應用程式程式碼`Rating`屬性。</span><span class="sxs-lookup"><span data-stu-id="9291b-129">You've now updated the application code to support the new `Rating` property.</span></span>

<span data-ttu-id="9291b-130">現在執行應用程式中，瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="9291b-130">Now run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="9291b-131">當您這樣做時，不過，您會看到下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="9291b-131">When you do this, though, you'll see the following error:</span></span>

![](adding-a-new-field/_static/image1.png)

<span data-ttu-id="9291b-132">您在看到這個錯誤，因為已更新`Movie`應用程式中的模型類別現在是不同的結構描述`Movie`現有資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="9291b-132">You're seeing this error because the updated `Movie` model class in the application is now different than the schema of the `Movie` table of the existing database.</span></span> <span data-ttu-id="9291b-133">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="9291b-133">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="9291b-134">根據預設，當您使用 Entity Framework Code First 來自動建立的資料庫，您可以如同稍早在本教學課程，第一個程式碼將資料表加入至要協助追蹤資料庫的結構描述是否從產生的模型類別同步的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9291b-134">By default, when you use Entity Framework Code First to automatically create a database, as you did earlier in this tutorial, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="9291b-135">如果不是同步，Entity Framework 會擲回錯誤。</span><span class="sxs-lookup"><span data-stu-id="9291b-135">If they aren't in sync, the Entity Framework throws an error.</span></span> <span data-ttu-id="9291b-136">這可讓您更輕鬆地在開發期間，您可能否則只 （藉由晦澀難懂的錯誤） 在執行階段追蹤問題。</span><span class="sxs-lookup"><span data-stu-id="9291b-136">This makes it easier to track down issues at development time that you might otherwise only find (by obscure errors) at run time.</span></span> <span data-ttu-id="9291b-137">檢查同步處理的功能是什麼造成的錯誤訊息，顯示您看到的。</span><span class="sxs-lookup"><span data-stu-id="9291b-137">The sync-checking feature is what causes the error message to be displayed that you just saw.</span></span>

<span data-ttu-id="9291b-138">有兩種方法來解決錯誤：</span><span class="sxs-lookup"><span data-stu-id="9291b-138">There are two approaches to resolving the error:</span></span>

1. <span data-ttu-id="9291b-139">讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="9291b-139">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="9291b-140">這種方法時，非常方便進行開發的測試資料庫上，因為它可讓您快速發展的模型和資料庫結構描述在一起。</span><span class="sxs-lookup"><span data-stu-id="9291b-140">This approach is very convenient when doing active development on a test database, because it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="9291b-141">這個選項的缺點是，您在資料庫中現有的資料遺失，因此您*不*想要在實際執行資料庫上使用此方法 ！</span><span class="sxs-lookup"><span data-stu-id="9291b-141">The downside, though, is that you lose existing data in the database — so you *don't* want to use this approach on a production database!</span></span>
2. <span data-ttu-id="9291b-142">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="9291b-142">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="9291b-143">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="9291b-143">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="9291b-144">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="9291b-144">You can make this change either manually or by creating a database change script.</span></span>

<span data-ttu-id="9291b-145">本教學課程中，我們將使用第一種方法，您必須在 Entity Framework Code First 每當模型變更會自動重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="9291b-145">For this tutorial, we'll use the first approach — you'll have the Entity Framework Code First automatically re-create the database anytime the model changes.</span></span>

## <a name="automatically-re-creating-the-database-on-model-changes"></a><span data-ttu-id="9291b-146">自動重新建立的資料庫上的模型變更</span><span class="sxs-lookup"><span data-stu-id="9291b-146">Automatically Re-Creating the Database on Model Changes</span></span>

<span data-ttu-id="9291b-147">讓我們來更新應用程式，使程式碼第一個自動卸除並重新建立資料庫，每當您變更應用程式的模型。</span><span class="sxs-lookup"><span data-stu-id="9291b-147">Let's update the application so that Code First automatically drops and re-creates the database anytime you change the model for the application.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="9291b-148">**警告**您應該啟用自動卸除和重新建立資料庫，僅當您正在使用開發或測試的資料庫，這種方法和*從未*生產資料庫，其中包含實際資料。</span><span class="sxs-lookup"><span data-stu-id="9291b-148">**Warning** You should enable this approach of automatically dropping and re-creating the database only when you're using a development or test database, and *never* on a production database that contains real data.</span></span> <span data-ttu-id="9291b-149">使用實際執行伺服器上可能會導致資料遺失。</span><span class="sxs-lookup"><span data-stu-id="9291b-149">Using it on a production server can lead to data loss.</span></span>


<span data-ttu-id="9291b-150">在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="9291b-150">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-new-field/_static/image2.png)

<span data-ttu-id="9291b-151">將類別&quot;MovieInitializer&quot;。</span><span class="sxs-lookup"><span data-stu-id="9291b-151">Name the class &quot;MovieInitializer&quot;.</span></span> <span data-ttu-id="9291b-152">更新`MovieInitializer`類別包含下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9291b-152">Update the `MovieInitializer` class to contain the following code:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

<span data-ttu-id="9291b-153">`MovieInitializer`類別會指定應該卸除模型所使用的資料庫，並自動重新建立如果曾變更模型類別。</span><span class="sxs-lookup"><span data-stu-id="9291b-153">The `MovieInitializer` class specifies that the database used by the model should be dropped and automatically re-created if the model classes ever change.</span></span> <span data-ttu-id="9291b-154">程式碼包含`Seed`方法，以指定預設資料會自動加入到資料庫任何時間已建立 （或重新建立）。</span><span class="sxs-lookup"><span data-stu-id="9291b-154">The code includes a `Seed` method to specify some default data to automatically add to the database any time it's created (or re-created).</span></span> <span data-ttu-id="9291b-155">這會提供實用的方式，來填入資料庫與一些範例資料，而不需要您手動擴展每次進行變更的模型。</span><span class="sxs-lookup"><span data-stu-id="9291b-155">This provides a useful way to populate the database with some sample data, without requiring you to manually populate it each time you make a model change.</span></span>

<span data-ttu-id="9291b-156">既然您已定義`MovieInitializer`類別，您會想要連接它，以便每次應用程式執行時，它會檢查是否不同於資料庫中的結構描述模型類別。</span><span class="sxs-lookup"><span data-stu-id="9291b-156">Now that you've defined the `MovieInitializer` class, you'll want to wire it up so that each time the application runs, it checks whether the model classes are different from the schema in the database.</span></span> <span data-ttu-id="9291b-157">如有需要，您可以執行初始設定式來重新建立要符合的模型，並於其中填入範例資料與資料庫的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9291b-157">If they are, you can run the initializer to re-create the database to match the model and then populate the database with the sample data.</span></span>

<span data-ttu-id="9291b-158">開啟*Global.asax*檔案的根目錄，`MvcMovies`專案：</span><span class="sxs-lookup"><span data-stu-id="9291b-158">Open the *Global.asax* file that's at the root of the `MvcMovies` project:</span></span>

<span data-ttu-id="9291b-159">*Global.asax*檔案包含的類別會定義整個應用程式的專案，並包含`Application_Start`應用程式第一次啟動時執行的事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="9291b-159">The *Global.asax* file contains the class that defines the entire application for the project, and contains an `Application_Start` event handler that runs when the application first starts.</span></span>

<span data-ttu-id="9291b-160">尋找`Application_Start`方法與將呼叫加入`Database.SetInitializer`開頭的方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="9291b-160">Find the `Application_Start` method and add a call to `Database.SetInitializer` at the beginning of the method, as shown below:</span></span>

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

<span data-ttu-id="9291b-161">`Database.SetInitializer`您剛才加入的陳述式指出所使用的資料庫`MovieDBContext`執行個體應該會自動刪除並重新建立如果不相符的結構描述和資料庫。</span><span class="sxs-lookup"><span data-stu-id="9291b-161">The `Database.SetInitializer` statement you just added indicates that the database used by the `MovieDBContext` instance should be automatically deleted and re-created if the schema and the database don't match.</span></span> <span data-ttu-id="9291b-162">如您所見，它也會填入範例資料中所指定的資料庫和`MovieInitializer`類別。</span><span class="sxs-lookup"><span data-stu-id="9291b-162">And as you saw, it will also populate the database with the sample data that's specified in the `MovieInitializer` class.</span></span>

<span data-ttu-id="9291b-163">關閉*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="9291b-163">Close the *Global.asax* file.</span></span>

<span data-ttu-id="9291b-164">重新執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="9291b-164">Re-run the application and navigate to the */Movies* URL.</span></span> <span data-ttu-id="9291b-165">當應用程式啟動時，它會偵測模型結構不再符合資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="9291b-165">When the application starts, it detects that the model structure no longer matches the database schema.</span></span> <span data-ttu-id="9291b-166">它會自動重新建立資料庫，以符合新的模型結構，並於其中填入資料庫的範例影片：</span><span class="sxs-lookup"><span data-stu-id="9291b-166">It automatically re-creates the database to match the new model structure and populates the database with the sample movies:</span></span>

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

<span data-ttu-id="9291b-168">按一下**新建**連結加入新的電影。</span><span class="sxs-lookup"><span data-stu-id="9291b-168">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="9291b-169">請注意，您可以加入評等。</span><span class="sxs-lookup"><span data-stu-id="9291b-169">Note that you can add a rating.</span></span>

<span data-ttu-id="9291b-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="9291b-170">[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)</span></span>

<span data-ttu-id="9291b-171">按一下 [建立] 。</span><span class="sxs-lookup"><span data-stu-id="9291b-171">Click **Create**.</span></span> <span data-ttu-id="9291b-172">新的電影，包括評等，現在會顯示列出的電影中：</span><span class="sxs-lookup"><span data-stu-id="9291b-172">The new movie, including the rating, now shows up in the movies listing:</span></span>

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

<span data-ttu-id="9291b-174">本節中您已看到如何修改模型物件和資料庫中與變更保持同步。</span><span class="sxs-lookup"><span data-stu-id="9291b-174">In this section you saw how you can modify model objects and keep the database in sync with the changes.</span></span> <span data-ttu-id="9291b-175">您也學到如何填入新建立的資料庫中的範例資料，因此您可以嘗試的案例。</span><span class="sxs-lookup"><span data-stu-id="9291b-175">You also learned a way to populate a newly created database with sample data so you can try out scenarios.</span></span> <span data-ttu-id="9291b-176">接下來，讓我們看看如何將更豐富的驗證邏輯加入至模型類別，並啟用某些商務規則，以強制執行。</span><span class="sxs-lookup"><span data-stu-id="9291b-176">Next, let's look at how you can add richer validation logic to the model classes and enable some business rules to be enforced.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9291b-177">[上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-validation-to-the-model.md)</span><span class="sxs-lookup"><span data-stu-id="9291b-177">[Previous](examining-the-edit-methods-and-edit-view.md)
[Next](adding-validation-to-the-model.md)</span></span>
