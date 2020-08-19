---
title: 第8部分： Razor ASP.NET Core 並行中具有 EF Core 的頁面
author: rick-anderson
description: 第8部分的 Razor 頁面和 Entity Framework 的教學課程系列。
ms.author: riande
ms.custom: mvc
ms.date: 07/22/2019
no-loc:
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: data/ef-rp/concurrency
ms.openlocfilehash: e03711d970c83c2b7d6cc76039cb0d556a751018
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88628907"
---
# <a name="part-8-no-locrazor-pages-with-ef-core-in-aspnet-core---concurrency"></a><span data-ttu-id="39638-103">第8部分： Razor ASP.NET Core 並行中具有 EF Core 的頁面</span><span class="sxs-lookup"><span data-stu-id="39638-103">Part 8, Razor Pages with EF Core in ASP.NET Core - Concurrency</span></span>

<span data-ttu-id="39638-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[Tom Dykstra](https://github.com/tdykstra)，以及 [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="39638-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="39638-105">本教學課程會顯示如何在多位使用者同時並行更新實體時處理衝突。</span><span class="sxs-lookup"><span data-stu-id="39638-105">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="39638-106">並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-106">Concurrency conflicts</span></span>

<span data-ttu-id="39638-107">並行衝突發生的時機：</span><span class="sxs-lookup"><span data-stu-id="39638-107">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="39638-108">使用者巡覽至實體的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="39638-108">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="39638-109">另一位使用者在第一位使用者的變更寫入到資料庫前更新相同實體。</span><span class="sxs-lookup"><span data-stu-id="39638-109">Another user updates the same entity before the first user's change is written to the database.</span></span>

<span data-ttu-id="39638-110">若未啟用並行偵測，則最後更新資料庫的任何使用者所作出變更會覆寫前一位使用者所作出變更。</span><span class="sxs-lookup"><span data-stu-id="39638-110">If concurrency detection isn't enabled, whoever updates the database last overwrites the other user's changes.</span></span> <span data-ttu-id="39638-111">若您可以接受這種風險，則並行程式設計所帶來的成本便可能會超過其效益。</span><span class="sxs-lookup"><span data-stu-id="39638-111">If this risk is acceptable, the cost of programming for concurrency might outweigh the benefit.</span></span>

### <a name="pessimistic-concurrency-locking"></a><span data-ttu-id="39638-112">封閉式並行存取 (鎖定)</span><span class="sxs-lookup"><span data-stu-id="39638-112">Pessimistic concurrency (locking)</span></span>

<span data-ttu-id="39638-113">其中一種避免並行衝突的方式是使用資料庫鎖定。</span><span class="sxs-lookup"><span data-stu-id="39638-113">One way to prevent concurrency conflicts is to use database locks.</span></span> <span data-ttu-id="39638-114">這稱之為封閉式並行存取。</span><span class="sxs-lookup"><span data-stu-id="39638-114">This is called pessimistic concurrency.</span></span> <span data-ttu-id="39638-115">在應用程式讀取要更新的資料庫資料列前，它會先要求鎖定。</span><span class="sxs-lookup"><span data-stu-id="39638-115">Before the app reads a database row that it intends to update, it requests a lock.</span></span> <span data-ttu-id="39638-116">針對更新存取鎖定資料列後，在第一個鎖定解除前，其他使用者都無法鎖定該資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-116">Once a row is locked for update access, no other users are allowed to lock the row until the first lock is released.</span></span>

<span data-ttu-id="39638-117">管理鎖定有幾個缺點。</span><span class="sxs-lookup"><span data-stu-id="39638-117">Managing locks has disadvantages.</span></span> <span data-ttu-id="39638-118">這種程式可能會相當複雜，且可能會隨著使用者數量的增加而造成效能問題。</span><span class="sxs-lookup"><span data-stu-id="39638-118">It can be complex to program and can cause performance problems as the number of users increases.</span></span> <span data-ttu-id="39638-119">Entity Framework Core 沒有提供這種功能的內建支援，本教學課程也不會示範如何進行實作。</span><span class="sxs-lookup"><span data-stu-id="39638-119">Entity Framework Core provides no built-in support for it, and this tutorial doesn't show how to implement it.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="39638-120">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="39638-120">Optimistic concurrency</span></span>

<span data-ttu-id="39638-121">開放式並行存取允許並行衝突發生，並會在衝突發生時適當的作出反應。</span><span class="sxs-lookup"><span data-stu-id="39638-121">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="39638-122">例如，Jane 造訪了 Department 編輯頁面，然後將英文部門的預算從美金 $350,000.00 元調整到美金 $0.00 元。</span><span class="sxs-lookup"><span data-stu-id="39638-122">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![將預算變更為 0](concurrency/_static/change-budget30.png)

<span data-ttu-id="39638-124">在 Jane 按一下 [儲存]\*\*\*\* 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。</span><span class="sxs-lookup"><span data-stu-id="39638-124">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![將開始日期變更為 2013 年](concurrency/_static/change-date30.png)

<span data-ttu-id="39638-126">Jane 先按了一下 [儲存]\*\*\*\* 並看到她所做的變更生效，因為瀏覽器顯示 Budget 金額為零的 Index 頁。</span><span class="sxs-lookup"><span data-stu-id="39638-126">Jane clicks **Save** first and sees her change take effect, since the browser displays the Index page with zero as the Budget amount.</span></span>

<span data-ttu-id="39638-127">John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-127">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="39638-128">接下來將發生情況是由您處理並行衝突的方式決定：</span><span class="sxs-lookup"><span data-stu-id="39638-128">What happens next is determined by how you handle concurrency conflicts:</span></span>

* <span data-ttu-id="39638-129">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="39638-129">You can keep track of which property a user has modified and update only the corresponding columns in the database.</span></span>

  <span data-ttu-id="39638-130">在此案例中，您將不會遺失任何資料。</span><span class="sxs-lookup"><span data-stu-id="39638-130">In the scenario, no data would be lost.</span></span> <span data-ttu-id="39638-131">兩位使用者更新了不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="39638-131">Different properties were updated by the two users.</span></span> <span data-ttu-id="39638-132">下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="39638-132">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="39638-133">這種更新方法可以減少可能會導致資料遺失的衝突數目。</span><span class="sxs-lookup"><span data-stu-id="39638-133">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="39638-134">此方法有一些缺點：</span><span class="sxs-lookup"><span data-stu-id="39638-134">This approach has some disadvantages:</span></span>
 
  * <span data-ttu-id="39638-135">無法在使用者更新相同屬性時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="39638-135">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="39638-136">表示通常在 Web 應用程式中不實用。</span><span class="sxs-lookup"><span data-stu-id="39638-136">Is generally not practical in a web app.</span></span> <span data-ttu-id="39638-137">它必須維持大量的狀態來追蹤所有擷取的值和新的值。</span><span class="sxs-lookup"><span data-stu-id="39638-137">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="39638-138">維持大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="39638-138">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="39638-139">與實體上的並行偵測相較之下，可能會增加應用程式的複雜度。</span><span class="sxs-lookup"><span data-stu-id="39638-139">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="39638-140">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="39638-140">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="39638-141">下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，以及擷取的美金 $350,000.00 元預算金額。</span><span class="sxs-lookup"><span data-stu-id="39638-141">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="39638-142">這稱之為「用戶端獲勝 (Client Wins)」\*\* 或「最後寫入為準 (Last in Wins)」\*\* 案例。</span><span class="sxs-lookup"><span data-stu-id="39638-142">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="39638-143"> (用戶端的所有值都優先于資料存放區中的內容。 ) 如果您沒有針對並行處理進行任何程式碼撰寫，用戶端會自動進行。</span><span class="sxs-lookup"><span data-stu-id="39638-143">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="39638-144">您可以防止 John 的變更更新到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="39638-144">You can prevent John's change from being updated in the database.</span></span> <span data-ttu-id="39638-145">一般而言，應用程式會：</span><span class="sxs-lookup"><span data-stu-id="39638-145">Typically, the app would:</span></span>

  * <span data-ttu-id="39638-146">顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="39638-146">Display an error message.</span></span>
  * <span data-ttu-id="39638-147">顯示資料的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="39638-147">Show the current state of the data.</span></span>
  * <span data-ttu-id="39638-148">允許使用者重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="39638-148">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="39638-149">這稱為「存放區獲勝 (Store Wins)」\*\* 案例。</span><span class="sxs-lookup"><span data-stu-id="39638-149">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="39638-150"> (資料存放區值的優先順序高於用戶端所提交的值。 ) 在本教學課程中，您將會執行 Store 獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="39638-150">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="39638-151">這個方法可確保沒有任何變更會在使用者收到警示前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="39638-151">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="conflict-detection-in-ef-core"></a><span data-ttu-id="39638-152">EF Core 中的衝突偵測</span><span class="sxs-lookup"><span data-stu-id="39638-152">Conflict detection in EF Core</span></span>

<span data-ttu-id="39638-153">EF Core 會在偵測到衝突時擲回 `DbConcurrencyException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="39638-153">EF Core throws `DbConcurrencyException` exceptions when it detects conflicts.</span></span> <span data-ttu-id="39638-154">資料模型必須進行設定才能啟用衝突偵測。</span><span class="sxs-lookup"><span data-stu-id="39638-154">The data model has to be configured to enable conflict detection.</span></span> <span data-ttu-id="39638-155">啟用衝突偵測的選項包括下列項目：</span><span class="sxs-lookup"><span data-stu-id="39638-155">Options for enabling conflict detection include the following:</span></span>

* <span data-ttu-id="39638-156">設定 EF Core，在 Update 和 Delete 命令的 Where 子句中將資料行的原始值設為[並行權杖](/ef/core/modeling/concurrency)並包含在其中。</span><span class="sxs-lookup"><span data-stu-id="39638-156">Configure EF Core to include the original values of columns configured as [concurrency tokens](/ef/core/modeling/concurrency) in the Where clause of Update and Delete commands.</span></span>

  <span data-ttu-id="39638-157">當 `SaveChanges` 呼叫時，Where 子句會尋找以屬性標注之任何屬性的原始值 <xref:System.ComponentModel.DataAnnotations.ConcurrencyCheckAttribute> 。</span><span class="sxs-lookup"><span data-stu-id="39638-157">When `SaveChanges` is called, the Where clause looks for the original values of any properties annotated with the <xref:System.ComponentModel.DataAnnotations.ConcurrencyCheckAttribute> attribute.</span></span> <span data-ttu-id="39638-158">若從第一次讀取資料列後有任何的並行權杖屬性產生變更，更新陳述式便不會尋找要更新的資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-158">The update statement won't find a row to update if any of the concurrency token properties changed since the row was first read.</span></span> <span data-ttu-id="39638-159">EF Core 會將其解譯為並行衝突。</span><span class="sxs-lookup"><span data-stu-id="39638-159">EF Core interprets that as a concurrency conflict.</span></span> <span data-ttu-id="39638-160">針對擁有許多資料行的資料庫資料表，這種方法可能導致非常龐大的 Where 子句，且可能會需要維持龐大數量的狀態。</span><span class="sxs-lookup"><span data-stu-id="39638-160">For database tables that have many columns, this approach can result in very large Where clauses, and can require large amounts of state.</span></span> <span data-ttu-id="39638-161">因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="39638-161">Therefore this approach is generally not recommended, and it isn't the method used in this tutorial.</span></span>

* <span data-ttu-id="39638-162">在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。</span><span class="sxs-lookup"><span data-stu-id="39638-162">In the database table, include a tracking column that can be used to determine when a row has been changed.</span></span>

  <span data-ttu-id="39638-163">在 SQL Server 資料庫中，追蹤資料行的資料型別是 `rowversion`。</span><span class="sxs-lookup"><span data-stu-id="39638-163">In a SQL Server database, the data type of the tracking column is `rowversion`.</span></span> <span data-ttu-id="39638-164">`rowversion` 的值是一個循序號碼，每一次資料列更新時都會遞增。</span><span class="sxs-lookup"><span data-stu-id="39638-164">The `rowversion` value is a sequential number that's incremented each time the row is updated.</span></span> <span data-ttu-id="39638-165">在 Update 或 Delete 命令中，Where 子句會包含追蹤資料行的原始值 (原始資料列版本號碼)。</span><span class="sxs-lookup"><span data-stu-id="39638-165">In an Update or Delete command, the Where clause includes the original value of the tracking column (the original row version number).</span></span> <span data-ttu-id="39638-166">若正在更新的資料列已由另一位使用者進行變更，則 `rowversion` 資料行中的值便會和原始值不同。</span><span class="sxs-lookup"><span data-stu-id="39638-166">If the row being updated has been changed by another user, the value in the `rowversion` column is different than the original value.</span></span> <span data-ttu-id="39638-167">在這種情況下，Update 或 Delete 陳述式便會因為 Where 子句而無法找到要更新的資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-167">In that case, the Update or Delete statement can't find the row to update because of the Where clause.</span></span> <span data-ttu-id="39638-168">EF Core 會在 Update 或 Delete 命令沒有影響任何資料列時擲回並行例外狀況。</span><span class="sxs-lookup"><span data-stu-id="39638-168">EF Core throws a concurrency exception when no rows are affected by an Update or Delete command.</span></span>

## <a name="add-a-tracking-property"></a><span data-ttu-id="39638-169">新增追蹤屬性</span><span class="sxs-lookup"><span data-stu-id="39638-169">Add a tracking property</span></span>

<span data-ttu-id="39638-170">在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="39638-170">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu30/Models/Department.cs?highlight=26,27)]

<span data-ttu-id="39638-171"><xref:System.ComponentModel.DataAnnotations.TimestampAttribute>屬性會將資料行識別為並行追蹤資料行。</span><span class="sxs-lookup"><span data-stu-id="39638-171">The <xref:System.ComponentModel.DataAnnotations.TimestampAttribute> attribute is what identifies the column as a concurrency tracking column.</span></span> <span data-ttu-id="39638-172">Fluent API 是指定追蹤屬性的另外一種方式：</span><span class="sxs-lookup"><span data-stu-id="39638-172">The fluent API is an alternative way to specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

# <a name="visual-studio"></a>[<span data-ttu-id="39638-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39638-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="39638-174">針對 SQL Server 資料庫，實體屬性上的 `[Timestamp]` 屬性會定義為位元組陣列：</span><span class="sxs-lookup"><span data-stu-id="39638-174">For a SQL Server database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="39638-175">使資料行包含在 DELETE 和 UPDATE WHERE 子句中。</span><span class="sxs-lookup"><span data-stu-id="39638-175">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="39638-176">將資料庫中的資料行型別設為 [rowversion](/sql/t-sql/data-types/rowversion-transact-sql)。</span><span class="sxs-lookup"><span data-stu-id="39638-176">Sets the column type in the database to [rowversion](/sql/t-sql/data-types/rowversion-transact-sql).</span></span>

<span data-ttu-id="39638-177">資料庫會產生循序的資料列版本號碼，會在每一次更新資料列時遞增。</span><span class="sxs-lookup"><span data-stu-id="39638-177">The database generates a sequential row version number that's incremented each time the row is updated.</span></span> <span data-ttu-id="39638-178">在 `Update` 或 `Delete` 命令中，`Where` 子句會包含擷取到的資料列版本值。</span><span class="sxs-lookup"><span data-stu-id="39638-178">In an `Update` or `Delete` command, the `Where` clause includes the fetched row version value.</span></span> <span data-ttu-id="39638-179">若正在更新的資料列從擷取之後已產生變更：</span><span class="sxs-lookup"><span data-stu-id="39638-179">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="39638-180">目前的資料列版本值會與所擷取值不相符。</span><span class="sxs-lookup"><span data-stu-id="39638-180">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="39638-181">`Update` 或 `Delete` 命令會因為 `Where` 子句尋找的是所擷取資料列版本值，而找不到資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-181">The `Update` or `Delete` commands don't find a row because the `Where` clause looks for the fetched row version value.</span></span>
* <span data-ttu-id="39638-182">於是便會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="39638-182">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="39638-183">下列程式碼顯示了當 Department 名稱更新時，由 EF Core 產生的一部分 T-SQL：</span><span class="sxs-lookup"><span data-stu-id="39638-183">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=2-3)]

<span data-ttu-id="39638-184">上述醒目提示程式碼顯示 `WHERE` 子句中包含 `RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="39638-184">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="39638-185">若資料庫 `RowVersion` 與 `RowVersion` 參數 (`@p2`) 不相同，便不會更新任何資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-185">If the database `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="39638-186">下列醒目提示程式碼顯示驗證確實有一個資料列獲得更新的 T-SQL：</span><span class="sxs-lookup"><span data-stu-id="39638-186">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu30snapshots/8-concurrency/sql.txt?highlight=4-6)]

<span data-ttu-id="39638-187">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) 傳回最後一個語句所影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="39638-187">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="39638-188">若沒有更新任何資料列，則 EF Core 會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="39638-188">If no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="39638-189">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39638-189">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="39638-190">針對 SQLite 資料庫，實體屬性上的 `[Timestamp]` 屬性會定義為位元組陣列：</span><span class="sxs-lookup"><span data-stu-id="39638-190">For a SQLite database, the `[Timestamp]` attribute on an entity property defined as byte array:</span></span>

* <span data-ttu-id="39638-191">使資料行包含在 DELETE 和 UPDATE WHERE 子句中。</span><span class="sxs-lookup"><span data-stu-id="39638-191">Causes the column to be included in DELETE and UPDATE WHERE clauses.</span></span>
* <span data-ttu-id="39638-192">對應到 BLOB 資料行型別。</span><span class="sxs-lookup"><span data-stu-id="39638-192">Maps to a BLOB column type.</span></span>

<span data-ttu-id="39638-193">資料庫觸發程序會在每次資料列更新時，使用新的隨機位元組陣列更新 RowVersion 資料行。</span><span class="sxs-lookup"><span data-stu-id="39638-193">Database triggers update the RowVersion column with a new random byte array whenever a row is updated.</span></span> <span data-ttu-id="39638-194">在 `Update` 或 `Delete` 命令中，`Where` 子句會包含 RowVersion 資料行的擷取值。</span><span class="sxs-lookup"><span data-stu-id="39638-194">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of the RowVersion column.</span></span> <span data-ttu-id="39638-195">若正在更新的資料列從擷取之後已產生變更：</span><span class="sxs-lookup"><span data-stu-id="39638-195">If the row being updated has changed since it was fetched:</span></span>

* <span data-ttu-id="39638-196">目前的資料列版本值會與所擷取值不相符。</span><span class="sxs-lookup"><span data-stu-id="39638-196">The current row version value doesn't match the fetched value.</span></span>
* <span data-ttu-id="39638-197">`Update` 或 `Delete` 命令會因為 `Where` 子句尋找的是原始資料列版本值而找不到資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-197">The `Update` or `Delete` command doesn't find a row because the `Where` clause looks for the original row version value.</span></span>
* <span data-ttu-id="39638-198">於是便會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="39638-198">A `DbUpdateConcurrencyException` is thrown.</span></span>

---

### <a name="update-the-database"></a><span data-ttu-id="39638-199">更新資料庫</span><span class="sxs-lookup"><span data-stu-id="39638-199">Update the database</span></span>

<span data-ttu-id="39638-200">新增 `RowVersion` 屬性會變更資料模型，因此您將需要進行移轉。</span><span class="sxs-lookup"><span data-stu-id="39638-200">Adding the `RowVersion` property changes the data model, which requires a migration.</span></span>

<span data-ttu-id="39638-201">建置專案。</span><span class="sxs-lookup"><span data-stu-id="39638-201">Build the project.</span></span> 

# <a name="visual-studio"></a>[<span data-ttu-id="39638-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39638-202">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="39638-203">在 PMC 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="39638-203">Run the following command in the PMC:</span></span>

  ```powershell
  Add-Migration RowVersion
  ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="39638-204">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39638-204">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="39638-205">在終端機中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="39638-205">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef migrations add RowVersion
  ```

---

<span data-ttu-id="39638-206">此命令：</span><span class="sxs-lookup"><span data-stu-id="39638-206">This command:</span></span>

* <span data-ttu-id="39638-207">建立 *Migrations/{time stamp}_RowVersion.cs* 移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="39638-207">Creates the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="39638-208">更新 *Migrations/SchoolContextModelSnapshot.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="39638-208">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="39638-209">更新會將下列醒目提示程式碼新增至 `BuildModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="39638-209">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu30/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=15-17)]

# <a name="visual-studio"></a>[<span data-ttu-id="39638-210">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39638-210">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="39638-211">在 PMC 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="39638-211">Run the following command in the PMC:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code"></a>[<span data-ttu-id="39638-212">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39638-212">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="39638-213">開啟 `Migrations/<timestamp>_RowVersion.cs` 檔案並新增醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="39638-213">Open the `Migrations/<timestamp>_RowVersion.cs` file and add the highlighted code:</span></span>

  [!code-csharp[](intro/samples/cu30/MigrationsSQLite/20190722151951_RowVersion.cs?highlight=16-42)]

  <span data-ttu-id="39638-214">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="39638-214">The preceding code:</span></span>

  * <span data-ttu-id="39638-215">使用隨機 Blob 值更新現有資料列。</span><span class="sxs-lookup"><span data-stu-id="39638-215">Updates existing rows with random blob values.</span></span>
  * <span data-ttu-id="39638-216">新增資料庫觸發程序，在每次資料列更新時將 RowVersion 資料行設為隨機 Blob 值。</span><span class="sxs-lookup"><span data-stu-id="39638-216">Adds database triggers that set the RowVersion column to a random blob value whenever a row is updated.</span></span>

* <span data-ttu-id="39638-217">在終端機中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="39638-217">Run the following command in a terminal:</span></span>

  ```dotnetcli
  dotnet ef database update
  ```

---

<a name="scaffold"></a>

## <a name="scaffold-department-pages"></a><span data-ttu-id="39638-218">Scaffold Department 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-218">Scaffold Department pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="39638-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39638-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="39638-220">遵循 [Scaffold Student 頁面](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，下列部分除外：</span><span class="sxs-lookup"><span data-stu-id="39638-220">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

* <span data-ttu-id="39638-221">建立 *Pages/Departments* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="39638-221">Create a *Pages/Departments* folder.</span></span>  
* <span data-ttu-id="39638-222">使用 `Department` 作為模型類別。</span><span class="sxs-lookup"><span data-stu-id="39638-222">Use `Department` for the model class.</span></span>
  * <span data-ttu-id="39638-223">使用現有內容類別，而非建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="39638-223">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="39638-224">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39638-224">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="39638-225">建立 *Pages/Departments* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="39638-225">Create a *Pages/Departments* folder.</span></span>

* <span data-ttu-id="39638-226">執行下列命令來 Scaffold Department 頁面。</span><span class="sxs-lookup"><span data-stu-id="39638-226">Run the following command to scaffold the Department pages.</span></span>

  <span data-ttu-id="39638-227">**在 Windows 上：**</span><span class="sxs-lookup"><span data-stu-id="39638-227">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

  <span data-ttu-id="39638-228">**在 Linux 或 macOS 上：**</span><span class="sxs-lookup"><span data-stu-id="39638-228">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages/Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="39638-229">建置專案。</span><span class="sxs-lookup"><span data-stu-id="39638-229">Build the project.</span></span>

## <a name="update-the-index-page"></a><span data-ttu-id="39638-230">更新 Index 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-230">Update the Index page</span></span>

<span data-ttu-id="39638-231">Scaffolding 工具會為 Index 頁面建立 `RowVersion` 資料行，但該欄位不會在生產應用程式中顯示。</span><span class="sxs-lookup"><span data-stu-id="39638-231">The scaffolding tool created a `RowVersion` column for the Index page, but that field wouldn't be displayed in a production app.</span></span> <span data-ttu-id="39638-232">在本教學課程中，會顯示 `RowVersion` 的最後一個位元組，來協助示範並行處理的運作方式。</span><span class="sxs-lookup"><span data-stu-id="39638-232">In this tutorial, the last byte of the `RowVersion` is displayed to help show how concurrency handling works.</span></span> <span data-ttu-id="39638-233">最後一個位元組本身不一定是唯一的。</span><span class="sxs-lookup"><span data-stu-id="39638-233">The last byte isn't guaranteed to be unique by itself.</span></span>

<span data-ttu-id="39638-234">更新 *Pages\Departments\Index.cshtml* 頁面：</span><span class="sxs-lookup"><span data-stu-id="39638-234">Update *Pages\Departments\Index.cshtml* page:</span></span>

* <span data-ttu-id="39638-235">使用 Departments 取代 Index。</span><span class="sxs-lookup"><span data-stu-id="39638-235">Replace Index with Departments.</span></span>
* <span data-ttu-id="39638-236">變更包含 `RowVersion` 的程式碼，僅顯示位元組陣列的最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="39638-236">Change the code containing `RowVersion` to show just the last byte of the byte array.</span></span>
* <span data-ttu-id="39638-237">使用 FullName 取代 FirstMidName。</span><span class="sxs-lookup"><span data-stu-id="39638-237">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="39638-238">下列程式碼會顯示更新後的頁面：</span><span class="sxs-lookup"><span data-stu-id="39638-238">The following code shows the updated page:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Departments/Index.cshtml?highlight=5,8,29,48,51)]

## <a name="update-the-edit-page-model"></a><span data-ttu-id="39638-239">更新 [編輯] 頁面模型</span><span class="sxs-lookup"><span data-stu-id="39638-239">Update the Edit page model</span></span>

<span data-ttu-id="39638-240">使用下列程式碼更新 *Pages\Departments\Edit.cshtml.cs* ：</span><span class="sxs-lookup"><span data-stu-id="39638-240">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_All)]

<span data-ttu-id="39638-241">在 <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.OriginalValue> `rowVersion` 方法中提取時，會以實體中的值更新 `OnGet` 。</span><span class="sxs-lookup"><span data-stu-id="39638-241">The <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.OriginalValue> is updated with the `rowVersion` value from the entity when it was fetched in the `OnGet` method.</span></span> <span data-ttu-id="39638-242">EF Core 會產生一個帶有包含了原始 `RowVersion` 值 WHERE 子句的 SQL UPDATE 命令。</span><span class="sxs-lookup"><span data-stu-id="39638-242">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="39638-243">若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則便會擲回 `DbUpdateConcurrencyException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="39638-243">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_RowVersion&highlight=17-18)]

<span data-ttu-id="39638-244">在上述醒目提示的程式碼中：</span><span class="sxs-lookup"><span data-stu-id="39638-244">In the preceding highlighted code:</span></span>

* <span data-ttu-id="39638-245">`Department.RowVersion` 中的值是原先在 Edit 頁面 Get 要求中所擷取實體中的內容。</span><span class="sxs-lookup"><span data-stu-id="39638-245">The value in `Department.RowVersion` is what was in the entity when it was originally fetched in the Get request for the Edit page.</span></span> <span data-ttu-id="39638-246">此值會 `OnPost` 由頁面中隱藏的欄位提供給方法，該欄位 Razor 會顯示要編輯的實體。</span><span class="sxs-lookup"><span data-stu-id="39638-246">The value is provided to the `OnPost` method by a hidden field in the Razor page that displays the entity to be edited.</span></span> <span data-ttu-id="39638-247">隱藏欄位的值會由模型繫結器複製到 `Department.RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="39638-247">The hidden field value is copied to `Department.RowVersion` by the model binder.</span></span>
* <span data-ttu-id="39638-248">`OriginalValue` 是 EF Core 將在 Where 子句中使用的內容。</span><span class="sxs-lookup"><span data-stu-id="39638-248">`OriginalValue` is what EF Core will use in the Where clause.</span></span> <span data-ttu-id="39638-249">在執行醒目提示的程式碼區段前，`OriginalValue` 會擁有在此方法中呼叫 `FirstOrDefaultAsync` 時原先在資料庫內的值，而該值可能會和 Edit 頁面上顯示的內容不同。</span><span class="sxs-lookup"><span data-stu-id="39638-249">Before the highlighted line of code executes, `OriginalValue` has the value that was in the database when `FirstOrDefaultAsync` was called in this method, which might be different from what was displayed on the Edit page.</span></span>
* <span data-ttu-id="39638-250">醒目提示的程式碼會確保 EF Core 使用 SQL UPDATE 陳述式 WHERE 子句中所顯示 `Department` 實體原始 `RowVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="39638-250">The highlighted code makes sure that EF Core uses the original `RowVersion` value from the displayed `Department` entity in the SQL UPDATE statement's Where clause.</span></span>

<span data-ttu-id="39638-251">發生並行錯誤時，下列醒目提示的程式碼會取得用戶端值 (POST 到此方法的值) 及資料庫值。</span><span class="sxs-lookup"><span data-stu-id="39638-251">When a concurrency error happens, the following highlighted code gets the client values (the values posted to this method) and the database values.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=14,23)]

<span data-ttu-id="39638-252">下列程式碼會為每個其資料庫中值與 POST 到 `OnPostAsync` 值的不同資料行新增一個自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="39638-252">The following code adds a custom error message for each column that has database values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_Error)]

<span data-ttu-id="39638-253">下列醒目提示的程式碼會將 `RowVersion` 值設為從資料庫所擷取新值。</span><span class="sxs-lookup"><span data-stu-id="39638-253">The following highlighted code sets the `RowVersion` value to the new value retrieved from the database.</span></span> <span data-ttu-id="39638-254">下一次當使用者按一下 [儲存]\*\*\*\* 時，只有在上一次顯示 [編輯] 頁面之後發生的並行錯誤會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="39638-254">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Edit.cshtml.cs?name=snippet_TryUpdateModel&highlight=28)]

<span data-ttu-id="39638-255">`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="39638-255">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="39638-256">在 Razor 頁面中， `ModelState` 當兩個欄位都存在時，欄位的值會優先于模型屬性值。</span><span class="sxs-lookup"><span data-stu-id="39638-256">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

### <a name="update-the-edit-page"></a><span data-ttu-id="39638-257">更新 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-257">Update the Edit page</span></span>

<span data-ttu-id="39638-258">使用下列程式碼更新 *Pages/Departments/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="39638-258">Update *Pages/Departments/Edit.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="39638-259">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="39638-259">The preceding code:</span></span>

* <span data-ttu-id="39638-260">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="39638-260">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="39638-261">新增一個隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="39638-261">Adds a hidden row version.</span></span> <span data-ttu-id="39638-262">`RowVersion` 必須加入，以便回傳系結值。</span><span class="sxs-lookup"><span data-stu-id="39638-262">`RowVersion` must be added so postback binds the value.</span></span>
* <span data-ttu-id="39638-263">顯示 `RowVersion` 的最後一個位元組，作為偵錯用途。</span><span class="sxs-lookup"><span data-stu-id="39638-263">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="39638-264">使用強型別的 `InstructorNameSL` 取代 `ViewData`。</span><span class="sxs-lookup"><span data-stu-id="39638-264">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

### <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="39638-265">使用 [編輯] 頁面測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-265">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="39638-266">開啟兩個英文部門上 [編輯] 頁面的瀏覽器執行個體：</span><span class="sxs-lookup"><span data-stu-id="39638-266">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="39638-267">執行應用程式並選取 Departments。</span><span class="sxs-lookup"><span data-stu-id="39638-267">Run the app and select Departments.</span></span>
* <span data-ttu-id="39638-268">以滑鼠右鍵按一下英文部門的**編輯**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-268">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="39638-269">在第一個索引標籤中，按一下英文部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="39638-269">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="39638-270">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="39638-270">The two browser tabs display the same information.</span></span>

<span data-ttu-id="39638-271">在第一個瀏覽器索引標籤中變更名稱，然後按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-271">Change the name in the first browser tab and click **Save**.</span></span>

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-130.png)

<span data-ttu-id="39638-273">瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。</span><span class="sxs-lookup"><span data-stu-id="39638-273">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="39638-274">請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。</span><span class="sxs-lookup"><span data-stu-id="39638-274">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="39638-275">在第二個瀏覽器索引標籤中變更不同欄位。</span><span class="sxs-lookup"><span data-stu-id="39638-275">Change a different field in the second browser tab.</span></span>

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-230.png)

<span data-ttu-id="39638-277">按一下 [檔案] 。</span><span class="sxs-lookup"><span data-stu-id="39638-277">Click **Save**.</span></span> <span data-ttu-id="39638-278">您會看到所有不符合資料庫值欄位的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="39638-278">You see error messages for all fields that don't match the database values:</span></span>

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error30.png)

<span data-ttu-id="39638-280">此瀏覽器視窗並未嘗試變更 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="39638-280">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="39638-281">複製並將目前的值 (語言 (Language)) 貼上 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="39638-281">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="39638-282">Tab 鍵移出。用戶端驗證會移除錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="39638-282">Tab out. Client-side validation removes the error message.</span></span>

<span data-ttu-id="39638-283">再按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-283">Click **Save** again.</span></span> <span data-ttu-id="39638-284">您在第二個瀏覽器索引標籤中輸入的值已儲存。</span><span class="sxs-lookup"><span data-stu-id="39638-284">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="39638-285">您會在 [索引] 頁面中看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="39638-285">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page-model"></a><span data-ttu-id="39638-286">更新刪除頁面模型</span><span class="sxs-lookup"><span data-stu-id="39638-286">Update the Delete page model</span></span>

<span data-ttu-id="39638-287">使用下列程式碼更新 *ges/Departments/Delete.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="39638-287">Update *Pages/Departments/Delete.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="39638-288">[刪除] 頁面會在實體擷取之後發生變更時偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="39638-288">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="39638-289">`Department.RowVersion` 是擷取實體時的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="39638-289">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="39638-290">當 EF Core 建立 SQL DELETE 命令時，它會包含一個帶有 `RowVersion` 的 WHERE 子句。</span><span class="sxs-lookup"><span data-stu-id="39638-290">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="39638-291">若 SQL DELETE 命令影響的資料列數目為零：</span><span class="sxs-lookup"><span data-stu-id="39638-291">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="39638-292">SQL DELETE 命令中 `RowVersion` 與資料庫中 `RowVersion` 不相符。</span><span class="sxs-lookup"><span data-stu-id="39638-292">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the database.</span></span>
* <span data-ttu-id="39638-293">擲回 DbUpdateConcurrencyException。</span><span class="sxs-lookup"><span data-stu-id="39638-293">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="39638-294">使用 `concurrencyError` 呼叫 `OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="39638-294">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="39638-295">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-295">Update the Delete page</span></span>

<span data-ttu-id="39638-296">使用下列程式碼更新 *ges/Departments/Delete.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="39638-296">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Departments/Delete.cshtml?highlight=1,10,39,42,51)]

<span data-ttu-id="39638-297">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="39638-297">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="39638-298">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="39638-298">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="39638-299">新增錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="39638-299">Adds an error message.</span></span>
* <span data-ttu-id="39638-300">在 [系統管理員]\*\*\*\* 欄位中將 FirstMidName 取代為 FullName。</span><span class="sxs-lookup"><span data-stu-id="39638-300">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="39638-301">變更 `RowVersion` 以顯示最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="39638-301">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="39638-302">新增一個隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="39638-302">Adds a hidden row version.</span></span> <span data-ttu-id="39638-303">`RowVersion` 必須加入，以便回傳系結值。</span><span class="sxs-lookup"><span data-stu-id="39638-303">`RowVersion` must be added so postback binds the value.</span></span>

### <a name="test-concurrency-conflicts"></a><span data-ttu-id="39638-304">測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-304">Test concurrency conflicts</span></span>

<span data-ttu-id="39638-305">建立一個測試部門。</span><span class="sxs-lookup"><span data-stu-id="39638-305">Create a test department.</span></span>

<span data-ttu-id="39638-306">開啟兩個測試部門上 [刪除] 頁面的瀏覽器執行個體：</span><span class="sxs-lookup"><span data-stu-id="39638-306">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="39638-307">執行應用程式並選取 Departments。</span><span class="sxs-lookup"><span data-stu-id="39638-307">Run the app and select Departments.</span></span>
* <span data-ttu-id="39638-308">以滑鼠右鍵按一下測試部門的**刪除**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-308">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="39638-309">按一下測試部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="39638-309">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="39638-310">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="39638-310">The two browser tabs display the same information.</span></span>

<span data-ttu-id="39638-311">在第一個瀏覽器索引標籤中變更預算，然後按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-311">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="39638-312">瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。</span><span class="sxs-lookup"><span data-stu-id="39638-312">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="39638-313">請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。</span><span class="sxs-lookup"><span data-stu-id="39638-313">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="39638-314">從第二個索引標籤中刪除測試部門。並行錯誤會與資料庫中目前的值一起顯示。</span><span class="sxs-lookup"><span data-stu-id="39638-314">Delete the test department from the second tab. A concurrency error is display with the current values from the database.</span></span> <span data-ttu-id="39638-315">除非已更新，否則按一下 [ **刪除** ] 會刪除實體 `RowVersion` 。</span><span class="sxs-lookup"><span data-stu-id="39638-315">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="39638-316">其他資源</span><span class="sxs-lookup"><span data-stu-id="39638-316">Additional resources</span></span>

* [<span data-ttu-id="39638-317">EF Core 中的並行權杖</span><span class="sxs-lookup"><span data-stu-id="39638-317">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="39638-318">在 EF Core 中處理並行</span><span class="sxs-lookup"><span data-stu-id="39638-318">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="39638-319">偵錯 ASP.NET Core 2.x 原始檔</span><span class="sxs-lookup"><span data-stu-id="39638-319">Debugging ASP.NET Core 2.x source</span></span>](https://github.com/dotnet/AspNetCore.Docs/issues/4155)

## <a name="next-steps"></a><span data-ttu-id="39638-320">後續步驟</span><span class="sxs-lookup"><span data-stu-id="39638-320">Next steps</span></span>

<span data-ttu-id="39638-321">這是一系列教學課程的最後一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="39638-321">This is the last tutorial in the series.</span></span> <span data-ttu-id="39638-322">[本教學課程系列的 MVC 版本](xref:data/ef-mvc/index)涵蓋了其他主題。</span><span class="sxs-lookup"><span data-stu-id="39638-322">Additional topics are covered in the [MVC version of this tutorial series](xref:data/ef-mvc/index).</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="39638-323">上一個教學課程</span><span class="sxs-lookup"><span data-stu-id="39638-323">Previous tutorial</span></span>](xref:data/ef-rp/update-related-data)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="39638-324">本教學課程會顯示如何在多位使用者同時並行更新實體時處理衝突。</span><span class="sxs-lookup"><span data-stu-id="39638-324">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="39638-325">若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="39638-325">If you run into problems you can't solve, [download or view the completed app.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="39638-326">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="39638-326">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="39638-327">並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-327">Concurrency conflicts</span></span>

<span data-ttu-id="39638-328">並行衝突發生的時機：</span><span class="sxs-lookup"><span data-stu-id="39638-328">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="39638-329">使用者巡覽至實體的編輯頁面。</span><span class="sxs-lookup"><span data-stu-id="39638-329">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="39638-330">另一個使用者在第一個使用者的變更寫入到資料庫前更新了相同的實體。</span><span class="sxs-lookup"><span data-stu-id="39638-330">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="39638-331">當未啟用並行偵測時卻發生並行存取時：</span><span class="sxs-lookup"><span data-stu-id="39638-331">If concurrency detection isn't enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="39638-332">最後作出的更新會成功。</span><span class="sxs-lookup"><span data-stu-id="39638-332">The last update wins.</span></span> <span data-ttu-id="39638-333">亦即，最後更新的值會儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="39638-333">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="39638-334">第一個作出的更新會遺失。</span><span class="sxs-lookup"><span data-stu-id="39638-334">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="39638-335">開放式並行存取</span><span class="sxs-lookup"><span data-stu-id="39638-335">Optimistic concurrency</span></span>

<span data-ttu-id="39638-336">開放式並行存取允許並行衝突發生，並會在衝突發生時適當的作出反應。</span><span class="sxs-lookup"><span data-stu-id="39638-336">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="39638-337">例如，Jane 造訪了 Department 編輯頁面，然後將英文部門的預算從美金 $350,000.00 元調整到美金 $0.00 元。</span><span class="sxs-lookup"><span data-stu-id="39638-337">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![將預算變更為 0](concurrency/_static/change-budget.png)

<span data-ttu-id="39638-339">在 Jane 按一下 [儲存]\*\*\*\* 前，John 造訪了相同的頁面並將 [開始日期] 欄位從 2007/9/1 變更為 2013/9/1。</span><span class="sxs-lookup"><span data-stu-id="39638-339">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![將開始日期變更為 2013 年](concurrency/_static/change-date.png)

<span data-ttu-id="39638-341">Jane 先按了一下 [儲存]\*\*\*\*，並且在瀏覽器顯示 [索引] 頁面時看到她作出的變更。</span><span class="sxs-lookup"><span data-stu-id="39638-341">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![預算已變更為 0](concurrency/_static/budget-zero.png)

<span data-ttu-id="39638-343">John 在仍然顯示預算為美金 $350,000.00 的 [編輯] 頁面上按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-343">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="39638-344">接下來發生的情況便是由您處理並行衝突的方式決定。</span><span class="sxs-lookup"><span data-stu-id="39638-344">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="39638-345">開放式並行存取包含下列選項：</span><span class="sxs-lookup"><span data-stu-id="39638-345">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="39638-346">您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。</span><span class="sxs-lookup"><span data-stu-id="39638-346">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

  <span data-ttu-id="39638-347">在此案例中，您將不會遺失任何資料。</span><span class="sxs-lookup"><span data-stu-id="39638-347">In the scenario, no data would be lost.</span></span> <span data-ttu-id="39638-348">兩位使用者更新了不同的屬性。</span><span class="sxs-lookup"><span data-stu-id="39638-348">Different properties were updated by the two users.</span></span> <span data-ttu-id="39638-349">下一次當有人瀏覽英文部門時，他們便會同時看到 Jane 和 John 所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="39638-349">The next time someone browses the English department, they will see both Jane's and John's changes.</span></span> <span data-ttu-id="39638-350">這種更新方法可以減少可能會導致資料遺失的衝突數目。</span><span class="sxs-lookup"><span data-stu-id="39638-350">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="39638-351">這種方法：</span><span class="sxs-lookup"><span data-stu-id="39638-351">This approach:</span></span>
 
  * <span data-ttu-id="39638-352">無法在使用者更新相同屬性時避免資料遺失。</span><span class="sxs-lookup"><span data-stu-id="39638-352">Can't avoid data loss if competing changes are made to the same property.</span></span>
  * <span data-ttu-id="39638-353">表示通常在 Web 應用程式中不實用。</span><span class="sxs-lookup"><span data-stu-id="39638-353">Is generally not practical in a web app.</span></span> <span data-ttu-id="39638-354">它必須維持大量的狀態來追蹤所有擷取的值和新的值。</span><span class="sxs-lookup"><span data-stu-id="39638-354">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="39638-355">維持大量的狀態可能會影響應用程式的效能。</span><span class="sxs-lookup"><span data-stu-id="39638-355">Maintaining large amounts of state can affect app performance.</span></span>
  * <span data-ttu-id="39638-356">與實體上的並行偵測相較之下，可能會增加應用程式的複雜度。</span><span class="sxs-lookup"><span data-stu-id="39638-356">Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="39638-357">您可以讓 John 的變更覆寫 Jane 的變更。</span><span class="sxs-lookup"><span data-stu-id="39638-357">You can let John's change overwrite Jane's change.</span></span>

  <span data-ttu-id="39638-358">下一次當有人瀏覽英文部門時，他們便會看到開始日期為 2013/9/1，以及擷取的美金 $350,000.00 元預算金額。</span><span class="sxs-lookup"><span data-stu-id="39638-358">The next time someone browses the English department, they will see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="39638-359">這稱之為「用戶端獲勝 (Client Wins)」\*\* 或「最後寫入為準 (Last in Wins)」\*\* 案例。</span><span class="sxs-lookup"><span data-stu-id="39638-359">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="39638-360"> (用戶端的所有值都優先于資料存放區中的內容。 ) 如果您沒有針對並行處理進行任何程式碼撰寫，用戶端會自動進行。</span><span class="sxs-lookup"><span data-stu-id="39638-360">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="39638-361">您可以防止 John 的變更更新到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="39638-361">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="39638-362">一般而言，應用程式會：</span><span class="sxs-lookup"><span data-stu-id="39638-362">Typically, the app would:</span></span>

  * <span data-ttu-id="39638-363">顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="39638-363">Display an error message.</span></span>
  * <span data-ttu-id="39638-364">顯示資料的目前狀態。</span><span class="sxs-lookup"><span data-stu-id="39638-364">Show the current state of the data.</span></span>
  * <span data-ttu-id="39638-365">允許使用者重新套用變更。</span><span class="sxs-lookup"><span data-stu-id="39638-365">Allow the user to reapply the changes.</span></span>

  <span data-ttu-id="39638-366">這稱為「存放區獲勝 (Store Wins)」\*\* 案例。</span><span class="sxs-lookup"><span data-stu-id="39638-366">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="39638-367"> (資料存放區值的優先順序高於用戶端所提交的值。 ) 在本教學課程中，您將會執行 Store 獲勝案例。</span><span class="sxs-lookup"><span data-stu-id="39638-367">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="39638-368">這個方法可確保沒有任何變更會在使用者收到警示前遭到覆寫。</span><span class="sxs-lookup"><span data-stu-id="39638-368">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="39638-369">處理並行</span><span class="sxs-lookup"><span data-stu-id="39638-369">Handling concurrency</span></span> 

<span data-ttu-id="39638-370">當屬性已設定為[並行權杖](/ef/core/modeling/concurrency)時：</span><span class="sxs-lookup"><span data-stu-id="39638-370">When a property is configured as a [concurrency token](/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="39638-371">EF Core 會驗證屬性在擷取之後尚未被修改。</span><span class="sxs-lookup"><span data-stu-id="39638-371">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="39638-372">該檢查會在呼叫 [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) 或 [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 時發生。</span><span class="sxs-lookup"><span data-stu-id="39638-372">The check occurs when [SaveChanges](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="39638-373">若屬性在擷取之後已遭修改，則系統便會擲回 [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="39638-373">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="39638-374">資料庫和資料模型必須設定為支援擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="39638-374">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="39638-375">在屬性上偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-375">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="39638-376">您可以使用 [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) 屬性來在屬性層級上偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="39638-376">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="39638-377">屬性可套用至模型上的多個屬性。</span><span class="sxs-lookup"><span data-stu-id="39638-377">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="39638-378">如需詳細資訊，請參閱[資料註解 - ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations)。</span><span class="sxs-lookup"><span data-stu-id="39638-378">For more information, see [Data Annotations-ConcurrencyCheck](/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="39638-379">此教學課程中不會使用 `[ConcurrencyCheck]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="39638-379">The `[ConcurrencyCheck]` attribute isn't used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="39638-380">在資料列上偵測並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-380">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="39638-381">為了偵測並行衝突，一個 [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) 追蹤資料行會新增到模型中。</span><span class="sxs-lookup"><span data-stu-id="39638-381">To detect concurrency conflicts, a [rowversion](/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="39638-382">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="39638-382">`rowversion` :</span></span>

* <span data-ttu-id="39638-383">是 SQL Server 的特定功能。</span><span class="sxs-lookup"><span data-stu-id="39638-383">Is SQL Server specific.</span></span> <span data-ttu-id="39638-384">其他資料庫可能不會提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="39638-384">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="39638-385">是用來判斷實體在從資料庫擷取之後是否有變更。</span><span class="sxs-lookup"><span data-stu-id="39638-385">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="39638-386">資料庫會產生一個循序 `rowversion` 數字，每一次資料列更新時該數字都會遞增。</span><span class="sxs-lookup"><span data-stu-id="39638-386">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="39638-387">在 `Update` 或 `Delete` 命令中，`Where` 子句會包含 `rowversion` 的擷取值。</span><span class="sxs-lookup"><span data-stu-id="39638-387">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="39638-388">如果更新的資料列已變更：</span><span class="sxs-lookup"><span data-stu-id="39638-388">If the row being updated has changed:</span></span>

* <span data-ttu-id="39638-389">`rowversion` 便不會符合擷取的值。</span><span class="sxs-lookup"><span data-stu-id="39638-389">`rowversion` doesn't match the fetched value.</span></span>
* <span data-ttu-id="39638-390">`Update` 或 `Delete` 命令便會找不到資料列，因為 `Where` 子句包含了擷取的 `rowversion`。</span><span class="sxs-lookup"><span data-stu-id="39638-390">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
* <span data-ttu-id="39638-391">於是便會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="39638-391">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="39638-392">在 EF Core 中，當 `Update` 或 `Delete` 命令沒有更新任何資料列時，系統便會擲回並行例外狀況。</span><span class="sxs-lookup"><span data-stu-id="39638-392">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="39638-393">將追蹤屬性新增到 Department 實體</span><span class="sxs-lookup"><span data-stu-id="39638-393">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="39638-394">在 *Models/Department.cs* 中，新增一個名為 RowVersion 的追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="39638-394">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="39638-395">[Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) 屬性表示此資料行會包含在 `Update` 和 `Delete` 命令的 `Where` 子句中。</span><span class="sxs-lookup"><span data-stu-id="39638-395">The [Timestamp](/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="39638-396">該屬性稱為 `Timestamp`，因為先前版本的 SQL Server 在以 SQL `rowversion` 類型取代之前使用了 SQL `timestamp` 資料類型。</span><span class="sxs-lookup"><span data-stu-id="39638-396">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="39638-397">Fluent API 也可以指定追蹤屬性：</span><span class="sxs-lookup"><span data-stu-id="39638-397">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="39638-398">下列程式碼顯示了當 Department 名稱更新時，由 EF Core 產生的一部分 T-SQL：</span><span class="sxs-lookup"><span data-stu-id="39638-398">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=2-3)]

<span data-ttu-id="39638-399">上述醒目提示程式碼顯示 `WHERE` 子句中包含 `RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="39638-399">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="39638-400">若資料庫 `RowVersion` 不等於 `RowVersion` 參數 (`@p2`)，則沒有任何資料列會獲得更新。</span><span class="sxs-lookup"><span data-stu-id="39638-400">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="39638-401">下列醒目提示程式碼顯示驗證確實有一個資料列獲得更新的 T-SQL：</span><span class="sxs-lookup"><span data-stu-id="39638-401">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/cu21snapshots/sql.txt?highlight=4-6)]

<span data-ttu-id="39638-402">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) 傳回最後一個語句所影響的資料列數目。</span><span class="sxs-lookup"><span data-stu-id="39638-402">[@@ROWCOUNT](/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="39638-403">當沒有更新任何資料列時，EF Core 便會擲回 `DbUpdateConcurrencyException`。</span><span class="sxs-lookup"><span data-stu-id="39638-403">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="39638-404">您可以在 Visual Studio 的輸出視窗中看到 EF Core 產生的 T-SQL。</span><span class="sxs-lookup"><span data-stu-id="39638-404">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="39638-405">更新資料庫</span><span class="sxs-lookup"><span data-stu-id="39638-405">Update the DB</span></span>

<span data-ttu-id="39638-406">新增 `RowVersion` 屬性會變更資料庫模型，因此您將需要進行移轉。</span><span class="sxs-lookup"><span data-stu-id="39638-406">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="39638-407">建置專案。</span><span class="sxs-lookup"><span data-stu-id="39638-407">Build the project.</span></span> <span data-ttu-id="39638-408">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="39638-408">Enter the following in a command window:</span></span>

```dotnetcli
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="39638-409">上述命令：</span><span class="sxs-lookup"><span data-stu-id="39638-409">The preceding commands:</span></span>

* <span data-ttu-id="39638-410">新增一個 *Migrations/{time stamp}_RowVersion.cs* 移轉檔案。</span><span class="sxs-lookup"><span data-stu-id="39638-410">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="39638-411">更新 *Migrations/SchoolContextModelSnapshot.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="39638-411">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="39638-412">更新會將下列醒目提示程式碼新增至 `BuildModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="39638-412">The update adds the following highlighted code to the `BuildModel` method:</span></span>

  [!code-csharp[](intro/samples/cu/Migrations/SchoolContextModelSnapshot.cs?name=snippet_Department&highlight=14-16)]

* <span data-ttu-id="39638-413">執行移轉，以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="39638-413">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>

## <a name="scaffold-the-departments-model"></a><span data-ttu-id="39638-414">Scaffold Departments 模型</span><span class="sxs-lookup"><span data-stu-id="39638-414">Scaffold the Departments model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="39638-415">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="39638-415">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="39638-416">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，並為模型類別使用 `Department`。</span><span class="sxs-lookup"><span data-stu-id="39638-416">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-student-pages) and use `Department` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="39638-417">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="39638-417">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="39638-418">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="39638-418">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
  ```

---

<span data-ttu-id="39638-419">上述命令會 Scaffold `Department` 模型。</span><span class="sxs-lookup"><span data-stu-id="39638-419">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="39638-420">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="39638-420">Open the project in Visual Studio.</span></span>

<span data-ttu-id="39638-421">建置專案。</span><span class="sxs-lookup"><span data-stu-id="39638-421">Build the project.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="39638-422">更新 Departments [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-422">Update the Departments Index page</span></span>

<span data-ttu-id="39638-423">Scaffolding 引擎會在 [索引] 頁面中建立 `RowVersion` 資料行，但該欄位不應該顯示出來。</span><span class="sxs-lookup"><span data-stu-id="39638-423">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="39638-424">在本教學課程中，`RowVersion` 的最後一個位元組會顯示出來，以協助您了解並行。</span><span class="sxs-lookup"><span data-stu-id="39638-424">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="39638-425">最後一個位元組不一定是唯一的。</span><span class="sxs-lookup"><span data-stu-id="39638-425">The last byte isn't guaranteed to be unique.</span></span> <span data-ttu-id="39638-426">實際的應用程式不會顯示 `RowVersion` 或 `RowVersion` 的最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="39638-426">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="39638-427">更新 [索引] 頁面：</span><span class="sxs-lookup"><span data-stu-id="39638-427">Update the Index page:</span></span>

* <span data-ttu-id="39638-428">使用 Departments 取代 Index。</span><span class="sxs-lookup"><span data-stu-id="39638-428">Replace Index with Departments.</span></span>
* <span data-ttu-id="39638-429">使用 `RowVersion` 的最後一個位元組取代包含 `RowVersion` 的標記。</span><span class="sxs-lookup"><span data-stu-id="39638-429">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="39638-430">使用 FullName 取代 FirstMidName。</span><span class="sxs-lookup"><span data-stu-id="39638-430">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="39638-431">下列標記顯示了更新後的頁面：</span><span class="sxs-lookup"><span data-stu-id="39638-431">The following markup shows the updated page:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="39638-432">更新 [編輯] 頁面模型</span><span class="sxs-lookup"><span data-stu-id="39638-432">Update the Edit page model</span></span>

<span data-ttu-id="39638-433">使用下列程式碼更新 *Pages\Departments\Edit.cshtml.cs* ：</span><span class="sxs-lookup"><span data-stu-id="39638-433">Update *Pages\Departments\Edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="39638-434">為了偵測並行問題，系統會使用擷取實體時的 `rowVersion` 值更新 [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue)。</span><span class="sxs-lookup"><span data-stu-id="39638-434">To detect a concurrency issue, the [OriginalValue](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="39638-435">EF Core 會產生一個帶有包含了原始 `RowVersion` 值 WHERE 子句的 SQL UPDATE 命令。</span><span class="sxs-lookup"><span data-stu-id="39638-435">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="39638-436">若 UPDATE 命令並未影響任何資料列 (即沒有任何資料列具有原始的 `RowVersion` 值)，則便會擲回 `DbUpdateConcurrencyException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="39638-436">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-999)]

<span data-ttu-id="39638-437">在上述程式碼中，`Department.RowVersion` 的值為擷取實體時的值。</span><span class="sxs-lookup"><span data-stu-id="39638-437">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="39638-438">`OriginalValue` 的值為此方法呼叫 `FirstOrDefaultAsync` 時在資料庫中的值。</span><span class="sxs-lookup"><span data-stu-id="39638-438">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="39638-439">下列程式碼會取得用戶端的值 (POST 到此方法的值) 以及資料庫的值：</span><span class="sxs-lookup"><span data-stu-id="39638-439">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="39638-440">下列程式碼會為每個資料庫中的值與發佈到 `OnPostAsync` 的值不同的資料行新增一個自訂錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="39638-440">The following code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="39638-441">下列醒目提示程式碼會將 `RowVersion` 的值設為從資料庫取得的新值。</span><span class="sxs-lookup"><span data-stu-id="39638-441">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="39638-442">下一次當使用者按一下 [儲存]\*\*\*\* 時，只有在上一次顯示 [編輯] 頁面之後發生的並行錯誤會被捕捉到。</span><span class="sxs-lookup"><span data-stu-id="39638-442">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="39638-443">`ModelState.Remove` 陳述式是必須的，因為 `ModelState` 具有舊的 `RowVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="39638-443">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="39638-444">在 Razor 頁面中， `ModelState` 當兩個欄位都存在時，欄位的值會優先于模型屬性值。</span><span class="sxs-lookup"><span data-stu-id="39638-444">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="39638-445">更新 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-445">Update the Edit page</span></span>

<span data-ttu-id="39638-446">以下列標記更新 *Pages/Departments/Edit.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="39638-446">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="39638-447">上述標記：</span><span class="sxs-lookup"><span data-stu-id="39638-447">The preceding markup:</span></span>

* <span data-ttu-id="39638-448">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="39638-448">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="39638-449">新增一個隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="39638-449">Adds a hidden row version.</span></span> <span data-ttu-id="39638-450">您必須新增 `RowVersion`，以讓回傳繫結值。</span><span class="sxs-lookup"><span data-stu-id="39638-450">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="39638-451">顯示 `RowVersion` 的最後一個位元組，作為偵錯用途。</span><span class="sxs-lookup"><span data-stu-id="39638-451">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="39638-452">使用強型別的 `InstructorNameSL` 取代 `ViewData`。</span><span class="sxs-lookup"><span data-stu-id="39638-452">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="39638-453">使用 [編輯] 頁面測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-453">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="39638-454">開啟兩個英文部門上 [編輯] 頁面的瀏覽器執行個體：</span><span class="sxs-lookup"><span data-stu-id="39638-454">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="39638-455">執行應用程式並選取 Departments。</span><span class="sxs-lookup"><span data-stu-id="39638-455">Run the app and select Departments.</span></span>
* <span data-ttu-id="39638-456">以滑鼠右鍵按一下英文部門的**編輯**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-456">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="39638-457">在第一個索引標籤中，按一下英文部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="39638-457">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="39638-458">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="39638-458">The two browser tabs display the same information.</span></span>

<span data-ttu-id="39638-459">在第一個瀏覽器索引標籤中變更名稱，然後按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-459">Change the name in the first browser tab and click **Save**.</span></span>

![變更之後的 Department [編輯] 頁面 1](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="39638-461">瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。</span><span class="sxs-lookup"><span data-stu-id="39638-461">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="39638-462">請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。</span><span class="sxs-lookup"><span data-stu-id="39638-462">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="39638-463">在第二個瀏覽器索引標籤中變更不同欄位。</span><span class="sxs-lookup"><span data-stu-id="39638-463">Change a different field in the second browser tab.</span></span>

![變更之後的 Department [編輯] 頁面 2](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="39638-465">按一下 [檔案] 。</span><span class="sxs-lookup"><span data-stu-id="39638-465">Click **Save**.</span></span> <span data-ttu-id="39638-466">您會看到所有不符合資料庫值之欄位的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="39638-466">You see error messages for all fields that don't match the DB values:</span></span>

![Department [編輯] 頁面錯誤訊息](concurrency/_static/edit-error.png)

<span data-ttu-id="39638-468">此瀏覽器視窗並未嘗試變更 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="39638-468">This browser window didn't intend to change the Name field.</span></span> <span data-ttu-id="39638-469">複製並將目前的值 (語言 (Language)) 貼上 [名稱] 欄位。</span><span class="sxs-lookup"><span data-stu-id="39638-469">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="39638-470">Tab 鍵移出。用戶端驗證會移除錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="39638-470">Tab out. Client-side validation removes the error message.</span></span>

![Department [編輯] 頁面錯誤訊息](concurrency/_static/cv.png)

<span data-ttu-id="39638-472">再按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-472">Click **Save** again.</span></span> <span data-ttu-id="39638-473">您在第二個瀏覽器索引標籤中輸入的值已儲存。</span><span class="sxs-lookup"><span data-stu-id="39638-473">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="39638-474">您會在 [索引] 頁面中看到儲存的值。</span><span class="sxs-lookup"><span data-stu-id="39638-474">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="39638-475">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-475">Update the Delete page</span></span>

<span data-ttu-id="39638-476">以下列程式碼更新 [刪除] 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="39638-476">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="39638-477">[刪除] 頁面會在實體擷取之後發生變更時偵測並行衝突。</span><span class="sxs-lookup"><span data-stu-id="39638-477">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="39638-478">`Department.RowVersion` 是擷取實體時的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="39638-478">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="39638-479">當 EF Core 建立 SQL DELETE 命令時，它會包含一個帶有 `RowVersion` 的 WHERE 子句。</span><span class="sxs-lookup"><span data-stu-id="39638-479">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="39638-480">若 SQL DELETE 命令影響的資料列數目為零：</span><span class="sxs-lookup"><span data-stu-id="39638-480">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="39638-481">表示 SQL DELETE 命令中的 `RowVersion` 不符合資料庫中的 `RowVersion`。</span><span class="sxs-lookup"><span data-stu-id="39638-481">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="39638-482">擲回 DbUpdateConcurrencyException。</span><span class="sxs-lookup"><span data-stu-id="39638-482">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="39638-483">使用 `concurrencyError` 呼叫 `OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="39638-483">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="39638-484">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="39638-484">Update the Delete page</span></span>

<span data-ttu-id="39638-485">使用下列程式碼更新 *ges/Departments/Delete.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="39638-485">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,39,51)]

<span data-ttu-id="39638-486">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="39638-486">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="39638-487">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="39638-487">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="39638-488">新增錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="39638-488">Adds an error message.</span></span>
* <span data-ttu-id="39638-489">在 [系統管理員]\*\*\*\* 欄位中將 FirstMidName 取代為 FullName。</span><span class="sxs-lookup"><span data-stu-id="39638-489">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="39638-490">變更 `RowVersion` 以顯示最後一個位元組。</span><span class="sxs-lookup"><span data-stu-id="39638-490">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="39638-491">新增一個隱藏的資料列版本。</span><span class="sxs-lookup"><span data-stu-id="39638-491">Adds a hidden row version.</span></span> <span data-ttu-id="39638-492">您必須新增 `RowVersion`，以讓回傳繫結值。</span><span class="sxs-lookup"><span data-stu-id="39638-492">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="39638-493">使用 [刪除] 頁面測試並行衝突</span><span class="sxs-lookup"><span data-stu-id="39638-493">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="39638-494">建立一個測試部門。</span><span class="sxs-lookup"><span data-stu-id="39638-494">Create a test department.</span></span>

<span data-ttu-id="39638-495">開啟兩個測試部門上 [刪除] 頁面的瀏覽器執行個體：</span><span class="sxs-lookup"><span data-stu-id="39638-495">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="39638-496">執行應用程式並選取 Departments。</span><span class="sxs-lookup"><span data-stu-id="39638-496">Run the app and select Departments.</span></span>
* <span data-ttu-id="39638-497">以滑鼠右鍵按一下測試部門的**刪除**超連結，然後選取 [Open in new tab] (在新索引標籤中開啟)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-497">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="39638-498">按一下測試部門的**編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="39638-498">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="39638-499">兩個瀏覽器索引標籤會顯示相同的資訊。</span><span class="sxs-lookup"><span data-stu-id="39638-499">The two browser tabs display the same information.</span></span>

<span data-ttu-id="39638-500">在第一個瀏覽器索引標籤中變更預算，然後按一下 [儲存]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="39638-500">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="39638-501">瀏覽器會顯示 [索引] 頁面，當中包含了變更之後的值和更新後的 rowVersion 指標。</span><span class="sxs-lookup"><span data-stu-id="39638-501">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="39638-502">請注意更新後的 rowVersion 指標。它會顯示在另一個索引標籤中的第二個回傳上。</span><span class="sxs-lookup"><span data-stu-id="39638-502">Note the updated rowVersion indicator, it's displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="39638-503">從第二個索引標籤中刪除測試部門。並行錯誤會與資料庫目前的值一起顯示。</span><span class="sxs-lookup"><span data-stu-id="39638-503">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="39638-504">除非已更新，否則按一下 [ **刪除** ] 會刪除實體 `RowVersion` 。</span><span class="sxs-lookup"><span data-stu-id="39638-504">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.</span></span>

<span data-ttu-id="39638-505">請參閱[繼承](xref:data/ef-mvc/inheritance)以了解如何繼承資料模型。</span><span class="sxs-lookup"><span data-stu-id="39638-505">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="39638-506">其他資源</span><span class="sxs-lookup"><span data-stu-id="39638-506">Additional resources</span></span>

* [<span data-ttu-id="39638-507">EF Core 中的並行權杖</span><span class="sxs-lookup"><span data-stu-id="39638-507">Concurrency Tokens in EF Core</span></span>](/ef/core/modeling/concurrency)
* [<span data-ttu-id="39638-508">在 EF Core 中處理並行</span><span class="sxs-lookup"><span data-stu-id="39638-508">Handle concurrency in EF Core</span></span>](/ef/core/saving/concurrency)
* [<span data-ttu-id="39638-509">這個教學課程的 YouTube 版本 (處理並行存取衝突)</span><span class="sxs-lookup"><span data-stu-id="39638-509">YouTube version of this tutorial(Handling Concurrency Conflicts)</span></span>](https://youtu.be/EosxHTFgYps)
* [<span data-ttu-id="39638-510">這個教學課程的 YouTube 版本 (第 2 部分)</span><span class="sxs-lookup"><span data-stu-id="39638-510">YouTube version of this tutorial(Part 2)</span></span>](https://www.youtube.com/watch?v=kcxERLnaGO0)
* [<span data-ttu-id="39638-511">這個教學課程的 YouTube 版本 (第 3 部分)</span><span class="sxs-lookup"><span data-stu-id="39638-511">YouTube version of this tutorial(Part 3)</span></span>](https://www.youtube.com/watch?v=d4RbpfvELRs)

> [!div class="step-by-step"]
> <span data-ttu-id="39638-512">[[上一步]](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="39638-512">[Previous](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

