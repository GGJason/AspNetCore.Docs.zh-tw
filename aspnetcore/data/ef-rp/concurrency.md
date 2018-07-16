---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 並行 - 8/8
author: rick-anderson
description: 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/concurrency
ms.openlocfilehash: c6ec07eb7bf484490bd7730edc44bf2d89e8fb2a
ms.sourcegitcommit: b8a2f14bf8dd346d7592977642b610bbcb0b0757
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2018
ms.locfileid: "38150479"
---
<span data-ttu-id="13b15-103">zh-tw/</span><span class="sxs-lookup"><span data-stu-id="13b15-103">en-us/</span></span>

# <a name="razor-pages-with-ef-core-in-aspnet-core---concurrency---8-of-8"></a><span data-ttu-id="13b15-104">ASP.NET Core 中的 Razor 頁面與 EF Core - 並行 - 8/8</span><span class="sxs-lookup"><span data-stu-id="13b15-104">Razor Pages with EF Core in ASP.NET Core - Concurrency - 8 of 8</span></span>

<span data-ttu-id="13b15-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)，以及 [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="13b15-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="13b15-106">本教學課程會顯示如何在多位使用者同時並行更新實體時處理衝突。</span><span class="sxs-lookup"><span data-stu-id="13b15-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="13b15-107">若您遭遇到無法解決的問題，請下載[此階段的完整應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8)。</span><span class="sxs-lookup"><span data-stu-id="13b15-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="13b15-108">並行衝突</span><span class="sxs-lookup"><span data-stu-id="13b15-108">Concurrency conflicts</span></span>

<span data-ttu-id="13b15-109">並行衝突發生的時機：</span><span class="sxs-lookup"><span data-stu-id="13b15-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="13b15-110">使用者巡覽至實體的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="13b15-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="13b15-111">另一個使用者在第一個使用者的變更寫入到資料庫前更新了相同的實體。</span><span class="sxs-lookup"><span data-stu-id="13b15-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="13b15-112">當未啟用並行偵測時卻發生並行存取時：</span><span class="sxs-lookup"><span data-stu-id="13b15-112">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="13b15-113">最後作出的更新會成功。</span><span class="sxs-lookup"><span data-stu-id="13b15-113">The last update wins.</span></span> <span data-ttu-id="13b15-114">亦即，最後更新的值會儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="13b15-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="13b15-115">第一個作出的更新會遺失。</span><span class="sxs-lookup"><span data-stu-id="13b15-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="13b15-116">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="13b15-116">Optimistic concurrency</span></span>

<span data-ttu-id="13b15-117">開放式並行存取允許並行衝突發生，並會在衝突發生時適當的作出反應。</span><span class="sxs-lookup"><span data-stu-id="13b15-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="13b15-118">例如，Jane 造訪了 Department 編輯頁面，然後將英文部門的預算從美金 $350,000.00 元調整到美金 $0.00 元。</span><span class="sxs-lookup"><span data-stu-id="13b15-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![將預算變更為 0](concurrency/_static/change-budget.png)

<span data-ttu-id="13b15-120">在 Jane 按一下 [儲存] 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。</span><span class="sxs-lookup"><span data-stu-id="13b15-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![將開始日期變更為 2013 年](concurrency/_static/change-date.png)

<span data-ttu-id="13b15-122">Jane 先按了一下 [儲存]，並且在瀏覽器顯示 [索引] 頁面時看到她作出的變更。</span><span class="sxs-lookup"><span data-stu-id="13b15-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![預算已變更為 0](concurrency/_static/budget-zero.png)

<span data-ttu-id="13b15-124">John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="13b15-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="13b15-125">接下來發生的情況便是由您處理並行衝突的方式決定。</span><span class="sxs-lookup"><span data-stu-id="13b15-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="13b15-126">開放式並行存取包含下列選項：</span><span class="sxs-lookup"><span data-stu-id="13b15-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="13b15-127">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="13b15-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="13b15-128">在此案例中，您將不會遺失任何資料。</span><span class="sxs-lookup"><span data-stu-id="13b15-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="13b15-129">兩位使用者更新了不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="13b15-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="13b15-130">下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="13b15-130">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="13b15-131">這種更新方法可以減少可能會導致資料遺失的衝突數目。</span><span class="sxs-lookup"><span data-stu-id="13b15-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="13b15-132">這種方法：\* 無法在使用者更新相同屬性時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="13b15-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="13b15-133">\* 表示通常在 Web 應用程式中不實用。</span><span class="sxs-lookup"><span data-stu-id="13b15-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="13b15-134">它必須維持大量的狀態來追蹤所有擷取的值和新的值。</span><span class="sxs-lookup"><span data-stu-id="13b15-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="13b15-135">維持大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="13b15-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="13b15-136">\* 與實體上的並行偵測相較之下，可能會增加應用程式的複雜度。</span><span class="sxs-lookup"><span data-stu-id="13b15-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="13b15-137">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="13b15-137">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="13b15-138">下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，以及擷取的美金 $350,000.00 元預算金額。</span><span class="sxs-lookup"><span data-stu-id="13b15-138">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="13b15-139">這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="13b15-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="13b15-140">(所有來自用戶端的值都會優先於資料存放區中的資料。)若您沒有針對並行處理撰寫任何程式碼，便會自動發生「用戶端獲勝 (Client Wins)」的情況。</span><span class="sxs-lookup"><span data-stu-id="13b15-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="13b15-141">您可以防止 John 的變更更新到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="13b15-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="13b15-142">一般而言，應用程式會：\* 顯示一個錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="13b15-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="13b15-143">\* 顯示資料的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="13b15-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="13b15-144">\* 允許使用者重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="13b15-144">\* Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="13b15-145">這稱之為「存放區獲勝 (Store Wins)」案例。</span><span class="sxs-lookup"><span data-stu-id="13b15-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="13b15-146">(資料存放區的值會優先於用戶端所提交的值。)您會在此教學課程中實作存放區獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="13b15-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="13b15-147">這個方法可確保沒有任何變更會在使用者收到警示前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="13b15-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="13b15-148">處理並行</span><span class="sxs-lookup"><span data-stu-id="13b15-148">Handling concurrency</span></span> 

<span data-ttu-id="13b15-149">當屬性已設定為[並行權杖](https://docs.microsoft.com/ef/core/modeling/concurrency)時：</span><span class="sxs-lookup"><span data-stu-id="13b15-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="13b15-150">EF Core 會驗證屬性在擷取之後尚未被修改。</span><span class="sxs-lookup"><span data-stu-id="13b15-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="13b15-151">該檢查會在呼叫 [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) 或 [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 時發生。</span><span class="sxs-lookup"><span data-stu-id="13b15-151">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="13b15-152">若屬性在擷取之後已遭修改，則系統便會擲回 [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="13b15-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="13b15-153">資料庫和資料模型必須設定為支援擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="13b15-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="13b15-154">在屬性上偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="13b15-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="13b15-155">您可以使用 [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) 屬性來在屬性層級上偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="13b15-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="13b15-156">屬性可套用至模型上的多個屬性。</span><span class="sxs-lookup"><span data-stu-id="13b15-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="13b15-157">如需詳細資訊，請參閱[資料註解 - ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations)。</span><span class="sxs-lookup"><span data-stu-id="13b15-157">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="13b15-158">此教學課程中不會使用 `[ConcurrencyCheck]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="13b15-158">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="13b15-159">在資料列上偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="13b15-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="13b15-160">為了偵測並行衝突，一個 [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) 追蹤資料行會新增到模型中。</span><span class="sxs-lookup"><span data-stu-id="13b15-160">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="13b15-161">`rowversion`：</span><span class="sxs-lookup"><span data-stu-id="13b15-161">`rowversion` :</span></span>

* <span data-ttu-id="13b15-162">是 SQL Server 的特定功能。</span><span class="sxs-lookup"><span data-stu-id="13b15-162">Is SQL Server specific.</span></span> <span data-ttu-id="13b15-163">其他資料庫可能不會提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="13b15-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="13b15-164">是用來判斷實體在從資料庫擷取之後是否有變更。</span><span class="sxs-lookup"><span data-stu-id="13b15-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="13b15-165">資料庫會產生一個循序 `rowversion` 數字，每一次資料列更新時該數字都會遞增。</span><span class="sxs-lookup"><span data-stu-id="13b15-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="13b15-166">在 `Update` 或 `Delete` 命令中，`Where` 子句會包含 `rowversion` 的擷取值。</span><span class="sxs-lookup"><span data-stu-id="13b15-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="13b15-167">如果更新的資料列已變更：</span><span class="sxs-lookup"><span data-stu-id="13b15-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="13b15-168">`rowversion` 便不會符合擷取的值。</span><span class="sxs-lookup"><span data-stu-id="13b15-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="13b15-169">`Update` 或 `Delete` 命令便會找不到資料列，因為 `Where` 子句包含了擷取的 `rowversion`。</span><span class="sxs-lookup"><span data-stu-id="13b15-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="13b15-170">於是便會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="13b15-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="13b15-171">在 EF Core 中，當 `Update` 或 `Delete` 命令沒有更新任何資料列時，系統便會擲回並行例外狀況。</span><span class="sxs-lookup"><span data-stu-id="13b15-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="13b15-172">將追蹤屬性新增到 Department 實體</span><span class="sxs-lookup"><span data-stu-id="13b15-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="13b15-173">在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="13b15-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="13b15-174">[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 屬性表示此資料行會包含在 `Update` 和 `Delete` 命令的 `Where` 子句中。</span><span class="sxs-lookup"><span data-stu-id="13b15-174">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="13b15-175">該屬性稱為 `Timestamp`，因為先前版本的 SQL Server 在以 SQL `rowversion` 類型取代之前使用了 SQL `timestamp` 資料類型。</span><span class="sxs-lookup"><span data-stu-id="13b15-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="13b15-176">Fluent API 也可以指定追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="13b15-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="13b15-177">下列程式碼顯示了當 Department 名稱更新時，由 EF Core 產生的一部分 T-SQL：</span><span class="sxs-lookup"><span data-stu-id="13b15-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="13b15-178">上述醒目提示程式碼顯示 `WHERE` 子句中包含 `RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="13b15-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="13b15-179">若資料庫 `RowVersion` 不等於 `RowVersion` 參數 (`@p2`)，則沒有任何資料列會獲得更新。</span><span class="sxs-lookup"><span data-stu-id="13b15-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="13b15-180">下列醒目提示程式碼顯示驗證確實有一個資料列獲得更新的 T-SQL：</span><span class="sxs-lookup"><span data-stu-id="13b15-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="13b15-181">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) 會傳回上一個陳述式所影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="13b15-181">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="13b15-182">當沒有更新任何資料列時，EF Core 便會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="13b15-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="13b15-183">您可以在 Visual Studio 的輸出視窗中看到 EF Core 產生的 T-SQL。</span><span class="sxs-lookup"><span data-stu-id="13b15-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="13b15-184">更新資料庫</span><span class="sxs-lookup"><span data-stu-id="13b15-184">Update the DB</span></span>

<span data-ttu-id="13b15-185">新增 `RowVersion` 屬性會變更資料庫模型，因此您將需要進行移轉。</span><span class="sxs-lookup"><span data-stu-id="13b15-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="13b15-186">建置專案。</span><span class="sxs-lookup"><span data-stu-id="13b15-186">Build the project.</span></span> <span data-ttu-id="13b15-187">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="13b15-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="13b15-188">上述命令：</span><span class="sxs-lookup"><span data-stu-id="13b15-188">The preceding commands:</span></span>

* <span data-ttu-id="13b15-189">新增一個 *Migrations/{time stamp}_RowVersion.cs* 移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="13b15-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="13b15-190">更新 *Migrations/SchoolContextModelSnapshot.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="13b15-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="13b15-191">更新會將下列醒目提示程式碼新增至 `BuildModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="13b15-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="13b15-192">執行移轉，以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="13b15-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="13b15-193">Scaffold Departments 模型</span><span class="sxs-lookup"><span data-stu-id="13b15-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="13b15-194">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="13b15-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="13b15-195">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="13b15-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="13b15-196">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="13b15-196">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

<span data-ttu-id="13b15-197">上述命令會 Scaffold `Department` 模型。</span><span class="sxs-lookup"><span data-stu-id="13b15-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="13b15-198">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="13b15-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="13b15-199">建置專案。</span><span class="sxs-lookup"><span data-stu-id="13b15-199">Build the project.</span></span> <span data-ttu-id="13b15-200">建置時會產生如下錯誤：</span><span class="sxs-lookup"><span data-stu-id="13b15-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="13b15-201">將 `_context.Department` 全域變更為 `_context.Departments` (亦即，在 `Department` 的名稱上新增一個 "s")。</span><span class="sxs-lookup"><span data-stu-id="13b15-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="13b15-202">找到並更新 7 個項目。</span><span class="sxs-lookup"><span data-stu-id="13b15-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="13b15-203">更新 Departments [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="13b15-203">Update the Departments Index page</span></span>

<span data-ttu-id="13b15-204">Scaffolding 引擎會在 [索引] 頁面中建立 `RowVersion` 資料行，但該欄位不應該顯示出來。</span><span class="sxs-lookup"><span data-stu-id="13b15-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="13b15-205">在本教學課程中，`RowVersion` 的最後一個位元組會顯示出來，以協助您了解並行。</span><span class="sxs-lookup"><span data-stu-id="13b15-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="13b15-206">最後一個位元組不一定是唯一的。</span><span class="sxs-lookup"><span data-stu-id="13b15-206">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="13b15-207">實際的應用程式不會顯示 `RowVersion` 或 `RowVersion` 的最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="13b15-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="13b15-208">更新 [索引] 頁面：</span><span class="sxs-lookup"><span data-stu-id="13b15-208">Update the Index page:</span></span>

* <span data-ttu-id="13b15-209">使用 Departments 取代 Index。</span><span class="sxs-lookup"><span data-stu-id="13b15-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="13b15-210">使用 `RowVersion` 的最後一個位元組取代包含 `RowVersion` 的標記。</span><span class="sxs-lookup"><span data-stu-id="13b15-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="13b15-211">使用 FullName 取代 FirstMidName。</span><span class="sxs-lookup"><span data-stu-id="13b15-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="13b15-212">下列標記顯示了更新後的頁面：</span><span class="sxs-lookup"><span data-stu-id="13b15-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="13b15-213">更新 [編輯] 頁面模型</span><span class="sxs-lookup"><span data-stu-id="13b15-213">Update the Edit page model</span></span>

<span data-ttu-id="13b15-214">使用下列程式碼更新 *pages\departments\edit.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="13b15-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="13b15-215">為了偵測並行問題，系統會使用擷取實體時的 `rowVersion` 值更新 [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)。</span><span class="sxs-lookup"><span data-stu-id="13b15-215">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="13b15-216">EF Core 會產生一個帶有包含了原始 `RowVersion` 值 WHERE 子句的 SQL UPDATE 命令。</span><span class="sxs-lookup"><span data-stu-id="13b15-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="13b15-217">若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則便會擲回 `DbUpdateConcurrencyException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="13b15-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="13b15-218">在上述程式碼中，`Department.RowVersion` 的值為擷取實體時的值。</span><span class="sxs-lookup"><span data-stu-id="13b15-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="13b15-219">`OriginalValue` 的值為此方法呼叫 `FirstOrDefaultAsync` 時在資料庫中的值。</span><span class="sxs-lookup"><span data-stu-id="13b15-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="13b15-220">下列程式碼會取得用戶端的值 (POST 到此方法的值) 以及資料庫的值：</span><span class="sxs-lookup"><span data-stu-id="13b15-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="13b15-221">下列程式碼會為每個資料庫中的值與 POST 到 `OnPostAsync` 的值不同的資料行新增一個自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="13b15-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="13b15-222">下列醒目提示程式碼會將 `RowVersion` 的值設為從資料庫取得的新值。</span><span class="sxs-lookup"><span data-stu-id="13b15-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="13b15-223">下一次當使用者按一下 [儲存] 時，只有在上一次顯示 [編輯] 頁面之後發生的並行錯誤會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="13b15-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="13b15-224">`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="13b15-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="13b15-225">在 Razor 頁面中，當兩者同時存在時，欄位的 `ModelState` 值會優先於模型屬性值。</span><span class="sxs-lookup"><span data-stu-id="13b15-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="13b15-226">更新 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="13b15-226">Update the Edit page</span></span>

<span data-ttu-id="13b15-227">以下列標記更新 *Pages/Departments/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="13b15-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="13b15-228">上述標記：</span><span class="sxs-lookup"><span data-stu-id="13b15-228">The preceding markup:</span></span>

* <span data-ttu-id="13b15-229">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="13b15-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="13b15-230">新增一個隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="13b15-230">Adds a hidden row version.</span></span> <span data-ttu-id="13b15-231">您必須新增 `RowVersion`，以讓回傳繫結值。</span><span class="sxs-lookup"><span data-stu-id="13b15-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="13b15-232">顯示 `RowVersion` 的最後一個位元組，作為偵錯用途。</span><span class="sxs-lookup"><span data-stu-id="13b15-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="13b15-233">使用強型別的 `InstructorNameSL` 取代 `ViewData`。</span><span class="sxs-lookup"><span data-stu-id="13b15-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="13b15-234">使用 [編輯] 頁面測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="13b15-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="13b15-235">開啟兩個英文部門上 [編輯] 頁面的瀏覽器執行個體：</span><span class="sxs-lookup"><span data-stu-id="13b15-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="13b15-236">執行應用程式並選取 Departments。</span><span class="sxs-lookup"><span data-stu-id="13b15-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="13b15-237">以滑鼠右鍵按一下英文部門的**編輯**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)。</span><span class="sxs-lookup"><span data-stu-id="13b15-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="13b15-238">在第一個索引標籤中，按一下英文部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="13b15-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="13b15-239">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="13b15-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="13b15-240">在第一個瀏覽器索引標籤中變更名稱，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="13b15-240">Change the name in the first browser tab and click **Save**.</span></span>

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="13b15-242">瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。</span><span class="sxs-lookup"><span data-stu-id="13b15-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="13b15-243">請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。</span><span class="sxs-lookup"><span data-stu-id="13b15-243">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="13b15-244">在第二個瀏覽器索引標籤中變更不同欄位。</span><span class="sxs-lookup"><span data-stu-id="13b15-244">Change a different field in the second browser tab.</span></span>

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="13b15-246">按一下 [儲存] 。</span><span class="sxs-lookup"><span data-stu-id="13b15-246">Click **Save**.</span></span> <span data-ttu-id="13b15-247">您會看到所有不符合資料庫值之欄位的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="13b15-247">You see error messages for all fields that don't match the DB values:</span></span>

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error.png)

<span data-ttu-id="13b15-249">此瀏覽器視窗並未嘗試變更 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="13b15-249">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="13b15-250">複製並將目前的值 (語言 (Language)) 貼上 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="13b15-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="13b15-251">按下 Tab 鍵切換至下一個欄位。用戶端驗證會移除錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="13b15-251">Tab out. Client-side validation removes the error message.</span></span>

![Department [編輯] 頁面錯誤訊息](concurrency/_static/cv.png)

<span data-ttu-id="13b15-253">再按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="13b15-253">Click **Save** again.</span></span> <span data-ttu-id="13b15-254">您在第二個瀏覽器索引標籤中輸入的值已儲存。</span><span class="sxs-lookup"><span data-stu-id="13b15-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="13b15-255">您會在 [索引] 頁面中看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="13b15-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="13b15-256">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="13b15-256">Update the Delete page</span></span>

<span data-ttu-id="13b15-257">以下列程式碼更新 [刪除] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="13b15-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="13b15-258">[刪除] 頁面會在實體擷取之後發生變更時偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="13b15-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="13b15-259">`Department.RowVersion` 是擷取實體時的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="13b15-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="13b15-260">當 EF Core 建立 SQL DELETE 命令時，它會包含一個帶有 `RowVersion` 的 WHERE 子句。</span><span class="sxs-lookup"><span data-stu-id="13b15-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="13b15-261">若 SQL DELETE 命令影響的資料列數目為零：</span><span class="sxs-lookup"><span data-stu-id="13b15-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="13b15-262">表示 SQL DELETE 命令中的 `RowVersion` 不符合資料庫中的 `RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="13b15-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="13b15-263">擲回 DbUpdateConcurrencyException。</span><span class="sxs-lookup"><span data-stu-id="13b15-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="13b15-264">使用 `concurrencyError` 呼叫 `OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="13b15-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="13b15-265">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="13b15-265">Update the Delete page</span></span>

<span data-ttu-id="13b15-266">使用下列程式碼更新 *ges/Departments/Delete.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="13b15-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="13b15-267">上述標記會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="13b15-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="13b15-268">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="13b15-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="13b15-269">新增錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="13b15-269">Adds an error message.</span></span>
* <span data-ttu-id="13b15-270">在 [系統管理員] 欄位中將 FirstMidName 取代為 FullName。</span><span class="sxs-lookup"><span data-stu-id="13b15-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="13b15-271">變更 `RowVersion` 以顯示最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="13b15-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="13b15-272">新增一個隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="13b15-272">Adds a hidden row version.</span></span> <span data-ttu-id="13b15-273">您必須新增 `RowVersion`，以讓回傳繫結值。</span><span class="sxs-lookup"><span data-stu-id="13b15-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="13b15-274">使用 [刪除] 頁面測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="13b15-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="13b15-275">建立一個測試部門。</span><span class="sxs-lookup"><span data-stu-id="13b15-275">Create a test department.</span></span>

<span data-ttu-id="13b15-276">開啟兩個測試部門上 [刪除] 頁面的瀏覽器執行個體：</span><span class="sxs-lookup"><span data-stu-id="13b15-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="13b15-277">執行應用程式並選取 Departments。</span><span class="sxs-lookup"><span data-stu-id="13b15-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="13b15-278">以滑鼠右鍵按一下測試部門的**刪除**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)。</span><span class="sxs-lookup"><span data-stu-id="13b15-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="13b15-279">按一下測試部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="13b15-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="13b15-280">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="13b15-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="13b15-281">在第一個瀏覽器索引標籤中變更預算，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="13b15-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="13b15-282">瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。</span><span class="sxs-lookup"><span data-stu-id="13b15-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="13b15-283">請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。</span><span class="sxs-lookup"><span data-stu-id="13b15-283">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="13b15-284">從第二個索引標籤刪除測試部門。系統會使用從資料庫取得之目前的值顯示並行錯誤。</span><span class="sxs-lookup"><span data-stu-id="13b15-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="13b15-285">按一下 [刪除] 會刪除實體，除非 `RowVersion` 已更新。部門已刪除。</span><span class="sxs-lookup"><span data-stu-id="13b15-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="13b15-286">請參閱[繼承](xref:data/ef-mvc/inheritance)以了解如何繼承資料模型。</span><span class="sxs-lookup"><span data-stu-id="13b15-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="13b15-287">其他資源</span><span class="sxs-lookup"><span data-stu-id="13b15-287">Additional resources</span></span>

* [<span data-ttu-id="13b15-288">EF Core 中的並行權杖</span><span class="sxs-lookup"><span data-stu-id="13b15-288">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="13b15-289">在 EF Core 中處理並行</span><span class="sxs-lookup"><span data-stu-id="13b15-289">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)

> [!div class="step-by-step"]
> [<span data-ttu-id="13b15-290">上一步</span><span class="sxs-lookup"><span data-stu-id="13b15-290">Previous</span></span>](xref:data/ef-rp/update-related-data)
