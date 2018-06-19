---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: 第一個使用 ASP.NET MVC 的 EF 資料庫： 自訂檢視 |Microsoft 文件
author: tfitzmac
description: 使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。 此教學課程里...
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2014
ms.topic: article
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 8338603e032329ad03d47c6392e508aa07c6858e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30867654"
---
<a name="ef-database-first-with-aspnet-mvc-customizing-a-view"></a><span data-ttu-id="b98a1-104">第一個使用 ASP.NET MVC 的 EF 資料庫： 自訂檢視</span><span class="sxs-lookup"><span data-stu-id="b98a1-104">EF Database First with ASP.NET MVC: Customizing a View</span></span>
====================
<span data-ttu-id="b98a1-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b98a1-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="b98a1-106">使用 MVC、 Entity Framework 和 ASP.NET Scaffolding，您可以建立 web 應用程式提供的介面到現有的資料庫。</span><span class="sxs-lookup"><span data-stu-id="b98a1-106">Using MVC, Entity Framework, and ASP.NET Scaffolding, you can create a web application that provides an interface to an existing database.</span></span> <span data-ttu-id="b98a1-107">此教學課程的數列會顯示如何自動產生程式碼，可讓使用者顯示、 編輯、 建立和刪除存在於資料庫資料表中的資料。</span><span class="sxs-lookup"><span data-stu-id="b98a1-107">This tutorial series shows you how to automatically generate code that enables users to display, edit, create, and delete data that resides in a database table.</span></span> <span data-ttu-id="b98a1-108">產生的程式碼會對應至資料庫資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="b98a1-108">The generated code corresponds to the columns in the database table.</span></span>
> 
> <span data-ttu-id="b98a1-109">變更自動產生的檢視，以增強簡報著重於數列的這個部分。</span><span class="sxs-lookup"><span data-stu-id="b98a1-109">This part of the series focuses on changing the automatically-generated views to enhance the presentation.</span></span>


## <a name="add-enrolled-courses-to-student-details"></a><span data-ttu-id="b98a1-110">新增已註冊的課程學生詳細資料</span><span class="sxs-lookup"><span data-stu-id="b98a1-110">Add enrolled courses to student details</span></span>

<span data-ttu-id="b98a1-111">產生的程式碼應用程式提供很好的起點，但它不一定會提供所有您需要在您的應用程式中的功能。</span><span class="sxs-lookup"><span data-stu-id="b98a1-111">The generated code provides a good starting point for your application but it does not necessarily provide all of the functionality that you need in your application.</span></span> <span data-ttu-id="b98a1-112">您可以自訂程式碼以符合您的應用程式的特定需求。</span><span class="sxs-lookup"><span data-stu-id="b98a1-112">You can customize the code to meet the particular requirements of your application.</span></span> <span data-ttu-id="b98a1-113">目前，您的應用程式不會顯示所選的學生已註冊的課程。</span><span class="sxs-lookup"><span data-stu-id="b98a1-113">Currently, your application does not display the enrolled courses for the selected student.</span></span> <span data-ttu-id="b98a1-114">在本節中，您會將已註冊的課程的每個學生**詳細資料**學生的檢視。</span><span class="sxs-lookup"><span data-stu-id="b98a1-114">In this section, you will add the enrolled courses for each student to the **Details** view for the student.</span></span>

<span data-ttu-id="b98a1-115">開啟**Students/Details.cshtml**，和最後一個之下&lt;/dl&gt;索引標籤上，但在關閉前&lt;/div&gt;標記中加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b98a1-115">Open **Students/Details.cshtml**, and below the last &lt;/dl&gt; tab, but before the closing &lt;/div&gt; tag, add the following code.</span></span>

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

<span data-ttu-id="b98a1-116">這段程式碼會建立顯示所選的學生在 Enrollment 資料表中每一筆記錄的資料列的資料表。</span><span class="sxs-lookup"><span data-stu-id="b98a1-116">This code creates a table that displays a row for each record in the Enrollment table for the selected student.</span></span> <span data-ttu-id="b98a1-117">**顯示**方法將 HTML 呈現物件 (modelItem)，表示運算式。</span><span class="sxs-lookup"><span data-stu-id="b98a1-117">The **Display** method renders HTML for the object (modelItem) that represents the expression.</span></span> <span data-ttu-id="b98a1-118">使用顯示方法 （而非只內嵌程式碼中的屬性值） 以確定此值的格式正確會根據其類型和該類型的範本。</span><span class="sxs-lookup"><span data-stu-id="b98a1-118">You use the Display method (rather than simply embedding the property value in the code) to make sure the value is formatted correctly based on its type and the template for that type.</span></span> <span data-ttu-id="b98a1-119">在此範例中，每個運算式會傳回單一的屬性與目前的記錄，在迴圈中，且值為基本型別也會轉譯為文字。</span><span class="sxs-lookup"><span data-stu-id="b98a1-119">In this example, each expression returns a single property from the current record in the loop, and the values are primitive types which are rendered as text.</span></span>

<span data-ttu-id="b98a1-120">再次瀏覽至學生/索引檢視，並選取**詳細資料**學生的其中一個。</span><span class="sxs-lookup"><span data-stu-id="b98a1-120">Browse to the Students/Index view again and select **Details** for one of the students.</span></span> <span data-ttu-id="b98a1-121">您會看到已註冊的課程已包含在檢視中。</span><span class="sxs-lookup"><span data-stu-id="b98a1-121">You will see the enrolled courses have been included in the view.</span></span>

![學生註冊](customizing-a-view/_static/image1.png)

> [!div class="step-by-step"]
> <span data-ttu-id="b98a1-123">[上一頁](changing-the-database.md)
> [下一頁](enhancing-data-validation.md)</span><span class="sxs-lookup"><span data-stu-id="b98a1-123">[Previous](changing-the-database.md)
[Next](enhancing-data-validation.md)</span></span>
