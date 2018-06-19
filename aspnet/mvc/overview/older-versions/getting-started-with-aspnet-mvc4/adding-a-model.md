---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: 將模型加入 |Microsoft 文件
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本這裡會提供使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 容易遵循，以及示範...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 562a06e22aad62b6982aca3316a2dfe18a6eba2e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871957"
---
<a name="adding-a-model"></a><span data-ttu-id="74047-104">加入模型</span><span class="sxs-lookup"><span data-stu-id="74047-104">Adding a Model</span></span>
====================
<span data-ttu-id="74047-105">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="74047-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="74047-106">本教學課程的更新的版本時使用[這裡](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="74047-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="74047-107">它更安全、 容易遵循，及示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="74047-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="74047-108">本節中您要加入一些類別，來管理資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="74047-108">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="74047-109">這些類別會&quot;模型&quot;ASP.NET MVC 應用程式的一部分。</span><span class="sxs-lookup"><span data-stu-id="74047-109">These classes will be the &quot;model&quot; part of the ASP.NET MVC application.</span></span>

<span data-ttu-id="74047-110">您將使用的.NET Framework 資料存取技術稱為[Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx)來定義及使用這些模型類別。</span><span class="sxs-lookup"><span data-stu-id="74047-110">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) to define and work with these model classes.</span></span> <span data-ttu-id="74047-111">呼叫開發架構 （通常稱為 EF） 的 Entity Framework 支援*Code First*。</span><span class="sxs-lookup"><span data-stu-id="74047-111">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="74047-112">程式碼第一次可讓您撰寫簡單的類別來建立模型物件。</span><span class="sxs-lookup"><span data-stu-id="74047-112">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="74047-113">(這些是也稱為 POCO 類別，從&quot;純舊 CLR 物件。&quot;)然後您可以立即從您的類別，可讓非常全新且更快速的開發工作流程所建立的資料庫。</span><span class="sxs-lookup"><span data-stu-id="74047-113">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="74047-114">加入模型類別</span><span class="sxs-lookup"><span data-stu-id="74047-114">Adding Model Classes</span></span>

<span data-ttu-id="74047-115">在**方案總管 中**，以滑鼠右鍵按一下*模型*資料夾中，選取**新增**，然後選取**類別**。</span><span class="sxs-lookup"><span data-stu-id="74047-115">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="74047-116">輸入*類別*名稱&quot;影片&quot;。</span><span class="sxs-lookup"><span data-stu-id="74047-116">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="74047-117">加入下列五個屬性，以`Movie`類別：</span><span class="sxs-lookup"><span data-stu-id="74047-117">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="74047-118">我們將使用`Movie`類別來代表在資料庫中的影片。</span><span class="sxs-lookup"><span data-stu-id="74047-118">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="74047-119">每個執行個體`Movie`物件將會對應到資料庫資料表，以及每個內容中的資料列`Movie`類別會對應到資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="74047-119">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="74047-120">在同一個檔案中，加入下列`MovieDBContext`類別：</span><span class="sxs-lookup"><span data-stu-id="74047-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

<span data-ttu-id="74047-121">`MovieDBContext`類別代表 Entity Framework 影片的資料庫內容，用來處理擷取、 儲存及更新`Movie`類別執行個體在資料庫中的。</span><span class="sxs-lookup"><span data-stu-id="74047-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="74047-122">`MovieDBContext`衍生自`DbContext`基底類別由 Entity Framework 所提供。</span><span class="sxs-lookup"><span data-stu-id="74047-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="74047-123">若要能夠參考`DbContext`和`DbSet`，您需要新增下列`using`在檔案最上方的陳述式：</span><span class="sxs-lookup"><span data-stu-id="74047-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="74047-124">完整*Movie.cs*檔案如下所示。</span><span class="sxs-lookup"><span data-stu-id="74047-124">The complete *Movie.cs* file is shown below.</span></span> <span data-ttu-id="74047-125">(數個使用陳述式不需要已被移除。)</span><span class="sxs-lookup"><span data-stu-id="74047-125">(Several using statements that are not needed have been removed.)</span></span>

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a><span data-ttu-id="74047-126">建立的連接字串和使用 SQL Server LocalDB</span><span class="sxs-lookup"><span data-stu-id="74047-126">Creating a Connection String and Working with SQL Server LocalDB</span></span>

<span data-ttu-id="74047-127">`MovieDBContext`您所建立的類別會處理連接到資料庫和對應的工作`Movie`資料庫記錄的物件。</span><span class="sxs-lookup"><span data-stu-id="74047-127">The `MovieDBContext` class you created handles the task of connecting to the database and mapping `Movie` objects to database records.</span></span> <span data-ttu-id="74047-128">您可以詢問一個問題，是如何指定將會連線到哪一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="74047-128">One question you might ask, though, is how to specify which database it will connect to.</span></span> <span data-ttu-id="74047-129">您將會執行，藉由新增中的連接資訊*Web.config*應用程式檔案。</span><span class="sxs-lookup"><span data-stu-id="74047-129">You'll do that by adding connection information in the *Web.config* file of the application.</span></span>

<span data-ttu-id="74047-130">開啟應用程式根目錄*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="74047-130">Open the application root *Web.config* file.</span></span> <span data-ttu-id="74047-131">(不*Web.config*檔案*檢視*資料夾。)開啟*Web.config*檔案加上紅框。</span><span class="sxs-lookup"><span data-stu-id="74047-131">(Not the *Web.config* file in the *Views* folder.) Open the *Web.config* file outlined in red.</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="74047-132">加入下列連接字串至`<connectionStrings>`中的項目*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="74047-132">Add the following connection string to the `<connectionStrings>` element in the *Web.config* file.</span></span>

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

<span data-ttu-id="74047-133">下列範例顯示的某一部分*Web.config*檔案與新加入的連接字串：</span><span class="sxs-lookup"><span data-stu-id="74047-133">The following example shows a portion of the *Web.config* file with the new connection string added:</span></span>

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

<span data-ttu-id="74047-134">少量的程式碼和 XML 是您需要撰寫以代表及電影資料儲存在資料庫中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="74047-134">This small amount of code and XML is everything you need to write in order to represent and store the movie data in a database.</span></span>

<span data-ttu-id="74047-135">接下來，您將建置新`MoviesController`類別可用來顯示電影，並允許使用者建立新的電影清單。</span><span class="sxs-lookup"><span data-stu-id="74047-135">Next, you'll build a new `MoviesController` class that you can use to display the movie data and allow users to create new movie listings.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="74047-136">[上一頁](adding-a-view.md)
> [下一頁](accessing-your-models-data-from-a-controller.md)</span><span class="sxs-lookup"><span data-stu-id="74047-136">[Previous](adding-a-view.md)
[Next](accessing-your-models-data-from-a-controller.md)</span></span>
