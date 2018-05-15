---
title: 將欄位新增至 ASP.NET Core 中的 Razor 頁面
author: rick-anderson
description: 示範如何使用 Entity Framework Core 將新欄位新增至 Razor Pages
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/07/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 45a39defc9480b0e4fe85ae7ed6bfa654a35264a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="ca469-103">將欄位新增至 ASP.NET Core 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="ca469-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="ca469-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ca469-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ca469-105">在本節中，您會使用 [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First 移轉，將欄位新增至模型，然後將該變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="ca469-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="ca469-106">當您使用 EF Code First 自動建立資料庫時，Code First 會將資料表新增至資料庫，以協助追蹤資料庫的結構描述是否與其產生的來源模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="ca469-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="ca469-107">如果未同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ca469-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="ca469-108">這可讓您更輕鬆地找出不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="ca469-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="ca469-109">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="ca469-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="ca469-110">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="ca469-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="ca469-111">建置應用程式 (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="ca469-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="ca469-112">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="ca469-112">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="ca469-113">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="ca469-113">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="ca469-114">使用 `Rating` 欄位更新 *Create.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="ca469-114">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="ca469-115">您可以複製/貼上前一個 `<div>` 項目，讓 IntelliSense 協助您更新這些欄位。</span><span class="sxs-lookup"><span data-stu-id="ca469-115">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="ca469-116">IntelliSense 會使用[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="ca469-116">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![在檢視的第二個 Label 項目中，開發人員已針對 asp-for 屬性值鍵入字母 R。](new-field/_static/cr.png)

<span data-ttu-id="ca469-120">下列程式碼會示範 *Create.cshtml* 與 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="ca469-120">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=36-40)]

<span data-ttu-id="ca469-121">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="ca469-121">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="ca469-122">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="ca469-122">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="ca469-123">如果立即執行，應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="ca469-123">If run now, the app throws a `SqlException`:</span></span>

```
SqlException: Invalid column name 'Rating'.
```

<span data-ttu-id="ca469-124">此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述</span><span class="sxs-lookup"><span data-stu-id="ca469-124">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="ca469-125">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="ca469-125">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="ca469-126">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="ca469-126">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="ca469-127">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="ca469-127">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="ca469-128">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="ca469-128">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="ca469-129">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="ca469-129">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="ca469-130">您不想要在生產環境資料庫上使用此方法 ！</span><span class="sxs-lookup"><span data-stu-id="ca469-130">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="ca469-131">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="ca469-131">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="ca469-132">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="ca469-132">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="ca469-133">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="ca469-133">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="ca469-134">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="ca469-134">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="ca469-135">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="ca469-135">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="ca469-136">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="ca469-136">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="ca469-137">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="ca469-137">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="ca469-138">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="ca469-138">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="ca469-139">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="ca469-139">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="ca469-140">建置方案。</span><span class="sxs-lookup"><span data-stu-id="ca469-140">Build the solution.</span></span>

<a name="pmc"></a> <span data-ttu-id="ca469-141">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="ca469-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="ca469-142">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="ca469-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="ca469-143">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="ca469-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="ca469-144">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="ca469-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="ca469-145">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="ca469-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="ca469-146">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="ca469-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="ca469-147">建議您針對移轉檔案使用有意義的名稱，這更加實用。</span><span class="sxs-lookup"><span data-stu-id="ca469-147">It's helpful to use a meaningful name for the migration file.</span></span>

<a name="ssox"></a> <span data-ttu-id="ca469-148">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="ca469-148">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="ca469-149">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="ca469-149">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="ca469-150">若要從 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="ca469-150">To delete the database from SSOX:</span></span>

* <span data-ttu-id="ca469-151">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="ca469-151">Select the database in SSOX.</span></span>
* <span data-ttu-id="ca469-152">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="ca469-152">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="ca469-153">核取 [關閉現有的連接]。</span><span class="sxs-lookup"><span data-stu-id="ca469-153">Check **Close existing connections**.</span></span>
* <span data-ttu-id="ca469-154">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="ca469-154">Select **OK**.</span></span>
* <span data-ttu-id="ca469-155">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="ca469-155">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<span data-ttu-id="ca469-156">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="ca469-156">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="ca469-157">如果未植入資料庫，請停止 IIS Express，然後執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca469-157">If the database isn't seeded, stop IIS Express, and then run the app.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ca469-158">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="ca469-158">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
