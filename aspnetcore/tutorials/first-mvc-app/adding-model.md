---
title: 第4部分，將模型新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: ASP.NET Core MVC 之教學課程系列的第4部分。
ms.author: riande
ms.date: 01/13/2020
no-loc:
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: cc9c0447dc032ff0a88d379cd9d542e3406777bf
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88021961"
---
# <a name="part-4-add-a-model-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="237e6-103">第4部分，將模型新增至 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="237e6-103">Part 4, add a model to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="237e6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="237e6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="237e6-105">在本節中，您可以新增類別來管理資料庫中的電影。</span><span class="sxs-lookup"><span data-stu-id="237e6-105">In this section, you add classes for managing movies in a database.</span></span> <span data-ttu-id="237e6-106">這些類別是 **MVC** 應用程式的「模型」\*\*\*\* 部分。</span><span class="sxs-lookup"><span data-stu-id="237e6-106">These classes will be the "**M**odel" part of the **M**VC app.</span></span>

<span data-ttu-id="237e6-107">搭配 [Entity Framework Core](/ef/core) (EF Core) 使用這些類別，即可使用資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-107">You use these classes with [Entity Framework Core](/ef/core) (EF Core) to work with a database.</span></span> <span data-ttu-id="237e6-108">EF Core 是一種物件關聯式對應 (ORM) 架構，可簡化您必須撰寫的資料存取程式碼。</span><span class="sxs-lookup"><span data-stu-id="237e6-108">EF Core is an object-relational mapping (ORM) framework that simplifies the data access code that you have to write.</span></span>

<span data-ttu-id="237e6-109">您所建立的模型類別稱為 POCO 類別 (來自「純舊 CLR 物件」\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*)，因為它們對 EF Core 沒有任何相依性。</span><span class="sxs-lookup"><span data-stu-id="237e6-109">The model classes you create are known as POCO classes (from **P**lain **O**ld **C**LR **O**bjects) because they don't have any dependency on EF Core.</span></span> <span data-ttu-id="237e6-110">它們只會定義資料庫將儲存之資料的屬性。</span><span class="sxs-lookup"><span data-stu-id="237e6-110">They just define the properties of the data that will be stored in the database.</span></span>

<span data-ttu-id="237e6-111">在本教學課程中，您首先要撰寫模型類別，而 EF Core 會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-111">In this tutorial, you write the model classes first, and EF Core creates the database.</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="237e6-112">新增資料模型類別</span><span class="sxs-lookup"><span data-stu-id="237e6-112">Add a data model class</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-114">以滑鼠右鍵按一下 *Models* 資料夾 > [新增]\*\*\*\* > [類別]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="237e6-114">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="237e6-115">將檔案命名為 *Movie.cs*。</span><span class="sxs-lookup"><span data-stu-id="237e6-115">Name the file *Movie.cs*.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="237e6-116">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="237e6-116">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="237e6-117">將名為 *Movie.cs* 的檔案新增到 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="237e6-117">Add a file named *Movie.cs* to the *Models* folder.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="237e6-118">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-118">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="237e6-119">以滑鼠右鍵按一下 [*模型*] 資料夾 **，>**  >  **新增類別**] [  >  **空白類別**]。</span><span class="sxs-lookup"><span data-stu-id="237e6-119">Right-click the *Models* folder > **Add** > **New Class** > **Empty Class**.</span></span> <span data-ttu-id="237e6-120">將檔案命名為 *Movie.cs*。</span><span class="sxs-lookup"><span data-stu-id="237e6-120">Name the file *Movie.cs*.</span></span>

---

<span data-ttu-id="237e6-121">使用下列程式碼更新 *Movie.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="237e6-121">Update the *Movie.cs* file with the following code:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Models/Movie.cs)]

<span data-ttu-id="237e6-122">`Movie` 類別包含 `Id` 欄位，該欄位是資料庫的必要欄位，將作為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="237e6-122">The `Movie` class contains an `Id` field, which is required by the database for the primary key.</span></span>

<span data-ttu-id="237e6-123"><xref:System.ComponentModel.DataAnnotations.DataType>上的屬性會指定要) 的 `ReleaseDate` 資料 (類型 `Date` 。</span><span class="sxs-lookup"><span data-stu-id="237e6-123">The <xref:System.ComponentModel.DataAnnotations.DataType> attribute on `ReleaseDate` specifies the type of the data (`Date`).</span></span> <span data-ttu-id="237e6-124">使用此屬性：</span><span class="sxs-lookup"><span data-stu-id="237e6-124">With this attribute:</span></span>

* <span data-ttu-id="237e6-125">使用者不需要在日期欄位中輸入時間資訊。</span><span class="sxs-lookup"><span data-stu-id="237e6-125">The user is not required to enter time information in the date field.</span></span>
* <span data-ttu-id="237e6-126">只會顯示日期，不會顯示時間資訊。</span><span class="sxs-lookup"><span data-stu-id="237e6-126">Only the date is displayed, not time information.</span></span>

<span data-ttu-id="237e6-127">稍後的教學課程會涵蓋 [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations)。</span><span class="sxs-lookup"><span data-stu-id="237e6-127">[DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) are covered in a later tutorial.</span></span>

## <a name="add-nuget-packages"></a><span data-ttu-id="237e6-128">新增 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="237e6-128">Add NuGet packages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-129">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-129">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-130">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 套件管理員]\*\* [套件管理器主控台]\*\* > \*\*\*\* (PMC)。</span><span class="sxs-lookup"><span data-stu-id="237e6-130">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

![PMC 功能表](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="237e6-132">在 PMC 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-132">In the PMC, run the following command:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="237e6-133">上述命令會新增 EF Core SQL Server 提供者。</span><span class="sxs-lookup"><span data-stu-id="237e6-133">The preceding command adds the EF Core SQL Server provider.</span></span> <span data-ttu-id="237e6-134">提供者套件會將 EF Core 套件作為相依性安裝。</span><span class="sxs-lookup"><span data-stu-id="237e6-134">The provider package installs the EF Core package as a dependency.</span></span> <span data-ttu-id="237e6-135">其他套件會在本教學課程中稍後的 scaffolding 步驟內自動安裝。</span><span class="sxs-lookup"><span data-stu-id="237e6-135">Additional packages are installed automatically in the scaffolding step later in the tutorial.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="237e6-136">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="237e6-136">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/add-EF-NuGet-SQLite-CLI.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="237e6-137">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-137">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="237e6-138">從 [**專案**] 功能表中，選取 [**管理 NuGet 套件**]。</span><span class="sxs-lookup"><span data-stu-id="237e6-138">From the **Project** menu, select **Manage NuGet Packages**.</span></span>

<span data-ttu-id="237e6-139">在右上方的 [**搜尋**] 欄位中，輸入 `Microsoft.EntityFrameworkCore.SQLite` 並按下**Return**鍵進行搜尋。</span><span class="sxs-lookup"><span data-stu-id="237e6-139">In the **Search** field in the upper right, enter `Microsoft.EntityFrameworkCore.SQLite` and press the **Return** key to search.</span></span> <span data-ttu-id="237e6-140">選取相符的 NuGet 套件，然後按 [**新增套件**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="237e6-140">Select the matching NuGet package and press the **Add Package** button.</span></span>

![新增 Entity Framework Core NuGet 套件](~/tutorials/first-mvc-app-mac/adding-model/_static/add-nuget-packages.png)

<span data-ttu-id="237e6-142">[**選取專案**] 對話方塊隨即顯示，並已 `MvcMovie` 選取專案。</span><span class="sxs-lookup"><span data-stu-id="237e6-142">The **Select Projects** dialog will be displayed, with the `MvcMovie` project selected.</span></span> <span data-ttu-id="237e6-143">按下 [**確定]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="237e6-143">Press the **Ok** button.</span></span>

<span data-ttu-id="237e6-144">將會顯示 [**接受授權**] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="237e6-144">A **License Acceptance** dialog will be displayed.</span></span> <span data-ttu-id="237e6-145">查看所需的授權，然後按一下 [**接受**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="237e6-145">Review the licenses as desired, then click the **Accept** button.</span></span>

<span data-ttu-id="237e6-146">重複上述步驟來安裝下列 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="237e6-146">Repeat the above steps to install the following NuGet packages:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.EntityFrameworkCore.Design`

---

<a name="dc"></a>

## <a name="create-a-database-context-class"></a><span data-ttu-id="237e6-147">建立資料庫內容類別</span><span class="sxs-lookup"><span data-stu-id="237e6-147">Create a database context class</span></span>

<span data-ttu-id="237e6-148">資料庫內容類別是協調 `Movie` 模型 EF Core 功能 (建立、讀取、更新、刪除) 的必要項目。</span><span class="sxs-lookup"><span data-stu-id="237e6-148">A database context class is needed to coordinate EF Core functionality (Create, Read, Update, Delete) for the `Movie` model.</span></span> <span data-ttu-id="237e6-149">資料庫內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)，並指定要包含在資料模型中的實體。</span><span class="sxs-lookup"><span data-stu-id="237e6-149">The database context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext) and specifies the entities to include in the data model.</span></span>

<span data-ttu-id="237e6-150">建立 *Data* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="237e6-150">Create a *Data* folder.</span></span>

<span data-ttu-id="237e6-151">使用下列程式碼新增 *Data/MvcMovieContext.cs* 內容：</span><span class="sxs-lookup"><span data-stu-id="237e6-151">Add a *Data/MvcMovieContext.cs* file with the following code:</span></span> 

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/zDocOnly/MvcMovieContext.cs?name=snippet)]

<span data-ttu-id="237e6-152">上述程式碼會建立實體集的[DbSet \<Movie> ](/dotnet/api/microsoft.entityframeworkcore.dbset-1)屬性。</span><span class="sxs-lookup"><span data-stu-id="237e6-152">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="237e6-153">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="237e6-153">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="237e6-154">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="237e6-154">An entity corresponds to a row in the table.</span></span>

<a name="reg"></a>

## <a name="register-the-database-context"></a><span data-ttu-id="237e6-155">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="237e6-155">Register the database context</span></span>

<span data-ttu-id="237e6-156">ASP.NET Core 內建[相依性插入 (DI)](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="237e6-156">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="237e6-157">服務 (例如 EF Core 資料庫內容) 必須在應用程式啟動期間向 DI 進行註冊。</span><span class="sxs-lookup"><span data-stu-id="237e6-157">Services (such as the EF Core DB context) must be registered with DI during application startup.</span></span> <span data-ttu-id="237e6-158">需要這些服務的元件 (例如頁面) 會透過「函式 Razor 參數」提供這些服務。</span><span class="sxs-lookup"><span data-stu-id="237e6-158">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="237e6-159">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="237e6-159">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span> <span data-ttu-id="237e6-160">在本節中，您會向 DI 容器註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-160">In this section, you register the database context with the DI container.</span></span>

<span data-ttu-id="237e6-161">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="237e6-161">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using MvcMovie.Data;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="237e6-162">在 `Startup.ConfigureServices` 中新增下列醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="237e6-162">Add the following highlighted code in `Startup.ConfigureServices`:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-163">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-163">Visual Studio</span></span>](#tab/visual-studio)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_ConfigureServices&highlight=6-7)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-164">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-164">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Startup.cs?name=snippet_UseSqlite&highlight=6-7)]

---

<span data-ttu-id="237e6-165">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-165">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="237e6-166">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="237e6-166">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<a name="cs"></a>

## <a name="add-a-database-connection-string"></a><span data-ttu-id="237e6-167">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="237e6-167">Add a database connection string</span></span>

<span data-ttu-id="237e6-168">將連接字串新增至檔案*中的appsettings.js* ：</span><span class="sxs-lookup"><span data-stu-id="237e6-168">Add a connection string to the *appsettings.json* file:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-169">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-169">Visual Studio</span></span>](#tab/visual-studio)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings.json?highlight=10-12)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-170">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-170">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-json[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/appsettings_SQLite.json?highlight=10-12)]

---

<span data-ttu-id="237e6-171">建置專案以檢查編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="237e6-171">Build the project as a check for compiler errors.</span></span>

## <a name="scaffold-movie-pages"></a><span data-ttu-id="237e6-172">Scaffold 影片頁面</span><span class="sxs-lookup"><span data-stu-id="237e6-172">Scaffold movie pages</span></span>

<span data-ttu-id="237e6-173">使用 scaffolding 工具來為影片模型產生建立、讀取、更新和刪除 (CRUD) 頁面。</span><span class="sxs-lookup"><span data-stu-id="237e6-173">Use the scaffolding tool to produce Create, Read, Update, and Delete (CRUD) pages for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-174">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-174">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-175">在 [方案總管]\*\*\*\* 中以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [新增 Scaffold 項目]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="237e6-175">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![上方步驟的檢視](adding-model/_static/add_controller21.png)

<span data-ttu-id="237e6-177">在 [新增 Scaffold]\*\*\*\* 對話方塊中，選取 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="237e6-177">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="237e6-179">完成 [新增控制器]\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="237e6-179">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="237e6-180">**模型類別:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="237e6-180">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="237e6-181">**資料內容類別：** \*MvcmoviecoNtext.cs (MvcMovie 資料) \*</span><span class="sxs-lookup"><span data-stu-id="237e6-181">**Data context class:** *MvcMovieContext (MvcMovie.Data)*</span></span>

![新增資料內容](adding-model/_static/dc3.png)

* <span data-ttu-id="237e6-183">**檢視：** 保持核取預設的每一個選項</span><span class="sxs-lookup"><span data-stu-id="237e6-183">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="237e6-184">**控制器名稱：** 保留預設值 *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="237e6-184">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="237e6-185">選取 [新增]</span><span class="sxs-lookup"><span data-stu-id="237e6-185">Select **Add**</span></span>

<span data-ttu-id="237e6-186">Visual Studio 會建立：</span><span class="sxs-lookup"><span data-stu-id="237e6-186">Visual Studio creates:</span></span>

* <span data-ttu-id="237e6-187">電影控制器 (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="237e6-187">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="237e6-188">Razor查看用於建立、刪除、詳細資料、編輯和索引頁面的檔案 (*Views/電影/ \* cshtml*) </span><span class="sxs-lookup"><span data-stu-id="237e6-188">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="237e6-189">自動建立這些檔案的流程稱為 *scaffolding*。</span><span class="sxs-lookup"><span data-stu-id="237e6-189">The automatic creation of these files is known as *scaffolding*.</span></span>

### <a name="visual-studio-code"></a>[<span data-ttu-id="237e6-190">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="237e6-190">Visual Studio Code</span></span>](#tab/visual-studio-code) 

* <span data-ttu-id="237e6-191">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="237e6-191">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="237e6-192">在 Linux 上，匯出 scaffold 工具路徑：</span><span class="sxs-lookup"><span data-stu-id="237e6-192">On Linux, export the scaffold tool path:</span></span>

  ```console
  export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="237e6-193">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-193">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

### <a name="visual-studio-for-mac"></a>[<span data-ttu-id="237e6-194">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-194">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="237e6-195">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="237e6-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>

* <span data-ttu-id="237e6-196">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-196">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  [!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of tabs                  -->

<span data-ttu-id="237e6-197">您目前尚無法使用 scaffold 頁面，因為資料庫不存在。</span><span class="sxs-lookup"><span data-stu-id="237e6-197">You can't use the scaffolded pages yet because the database doesn't exist.</span></span> <span data-ttu-id="237e6-198">如果您執行應用程式，並按一下 [**電影應用程式**] 連結，就會出現 [*無法開啟資料庫*] 或 [*沒有這類資料表：電影*錯誤訊息]。</span><span class="sxs-lookup"><span data-stu-id="237e6-198">If you run the app and click on the **Movie App** link, you get a *Cannot open database* or *no such table: Movie* error message.</span></span>

<a name="migration"></a>

## <a name="initial-migration"></a><span data-ttu-id="237e6-199">初始移轉</span><span class="sxs-lookup"><span data-stu-id="237e6-199">Initial migration</span></span>

<span data-ttu-id="237e6-200">使用 EF Core 的 [移轉](xref:data/ef-mvc/migrations) 功能來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-200">Use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to create the database.</span></span> <span data-ttu-id="237e6-201">移轉是一組工具，可讓您建立和更新資料庫，使其與您的資料模型相符。</span><span class="sxs-lookup"><span data-stu-id="237e6-201">Migrations is a set of tools that let you create and update a database to match your data model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-202">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-202">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-203">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 套件管理員]\*\* [套件管理器主控台]\*\* > \*\*\*\* (PMC)。</span><span class="sxs-lookup"><span data-stu-id="237e6-203">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

<span data-ttu-id="237e6-204">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration InitialCreate
Update-Database
```

* <span data-ttu-id="237e6-205">`Add-Migration InitialCreate`：產生*遷移/{timestamp} _InitialCreate .cs*遷移檔案。</span><span class="sxs-lookup"><span data-stu-id="237e6-205">`Add-Migration InitialCreate`: Generates a *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="237e6-206">`InitialCreate` 引數是移轉名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-206">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="237e6-207">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-207">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="237e6-208">因為這是第一次移轉，所產生類別會包含建立資料庫結構描述的程式碼。</span><span class="sxs-lookup"><span data-stu-id="237e6-208">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="237e6-209">資料庫結構描述是以 `MvcMovieContext` 類別為基礎。</span><span class="sxs-lookup"><span data-stu-id="237e6-209">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span>

* <span data-ttu-id="237e6-210">`Update-Database`：將資料庫更新為先前命令所建立的最新遷移。</span><span class="sxs-lookup"><span data-stu-id="237e6-210">`Update-Database`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="237e6-211">此命令會執行 *Migrations/{time-stamp}_InitialCreate.cs* 檔案中的 `Up` 方法，其會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-211">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

  <span data-ttu-id="237e6-212">資料庫更新命令會產生下列警告：</span><span class="sxs-lookup"><span data-stu-id="237e6-212">The database update command generates the following warning:</span></span> 

  > <span data-ttu-id="237e6-213">沒有為實體型別 'Movie' 上的十進位資料行 'Price' 指定型別。</span><span class="sxs-lookup"><span data-stu-id="237e6-213">No type was specified for the decimal column 'Price' on entity type 'Movie'.</span></span> <span data-ttu-id="237e6-214">如果它們不符合預設的有效位數和小數位數，會導致以無訊息模式截斷這些值。</span><span class="sxs-lookup"><span data-stu-id="237e6-214">This will cause values to be silently truncated if they do not fit in the default precision and scale.</span></span> <span data-ttu-id="237e6-215">使用 'HasColumnType()' 明確指定可容納所有值的 SQL 伺服器資料行型別。</span><span class="sxs-lookup"><span data-stu-id="237e6-215">Explicitly specify the SQL server column type that can accommodate all the values using 'HasColumnType()'.</span></span>

  <span data-ttu-id="237e6-216">您可以忽略該警告，稍後的教學課程中將修正此問題。</span><span class="sxs-lookup"><span data-stu-id="237e6-216">You can ignore that warning, it will be fixed in a later tutorial.</span></span>

[!INCLUDE [more information on the PMC tools for EF Core](~/includes/ef-pmc.md)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-217">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-217">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [more information on the CLI for EF Core](~/includes/ef-cli.md)]

<span data-ttu-id="237e6-218">執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-218">Run the following .NET Core CLI commands:</span></span>

```dotnetcli
dotnet ef migrations add InitialCreate
dotnet ef database update
```

* <span data-ttu-id="237e6-219">`ef migrations add InitialCreate`：產生*遷移/{timestamp} _InitialCreate .cs*遷移檔案。</span><span class="sxs-lookup"><span data-stu-id="237e6-219">`ef migrations add InitialCreate`: Generates an *Migrations/{timestamp}_InitialCreate.cs* migration file.</span></span> <span data-ttu-id="237e6-220">`InitialCreate` 引數是移轉名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-220">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="237e6-221">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-221">Any name can be used, but by convention, a name is selected that describes the migration.</span></span> <span data-ttu-id="237e6-222">因為這是第一次移轉，所產生類別會包含建立資料庫結構描述的程式碼。</span><span class="sxs-lookup"><span data-stu-id="237e6-222">Because this is the first migration, the generated class contains code to create the database schema.</span></span> <span data-ttu-id="237e6-223">資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="237e6-223">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span>

* <span data-ttu-id="237e6-224">`ef database update`：將資料庫更新為先前命令所建立的最新遷移。</span><span class="sxs-lookup"><span data-stu-id="237e6-224">`ef database update`: Updates the database to the latest migration, which the previous command created.</span></span> <span data-ttu-id="237e6-225">此命令會執行 *Migrations/{time-stamp}_InitialCreate.cs* 檔案中的 `Up` 方法，其會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-225">This command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

---

### <a name="the-initialcreate-class"></a><span data-ttu-id="237e6-226">InitialCreate 類別</span><span class="sxs-lookup"><span data-stu-id="237e6-226">The InitialCreate class</span></span>

<span data-ttu-id="237e6-227">檢查 *Migrations/{timestamp}_InitialCreate.cs* 移轉檔案：</span><span class="sxs-lookup"><span data-stu-id="237e6-227">Examine the *Migrations/{timestamp}_InitialCreate.cs* migration file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Migrations/20190805165915_InitialCreate.cs?name=snippet)]

<span data-ttu-id="237e6-228">`Up` 方法會建立 Movie 資料表，並將 `Id` 設為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="237e6-228">The `Up` method creates the Movie table and configures `Id` as the primary key.</span></span> <span data-ttu-id="237e6-229">`Down` 方法會還原 `Up` 移轉所做的結構描述變更。</span><span class="sxs-lookup"><span data-stu-id="237e6-229">The `Down` method reverts the schema changes made by the `Up` migration.</span></span>

<a name="test"></a>

## <a name="test-the-app"></a><span data-ttu-id="237e6-230">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="237e6-230">Test the app</span></span>

* <span data-ttu-id="237e6-231">執行應用程式，然後按一下 [影片應用程式]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="237e6-231">Run the app and click the **Movie App** link.</span></span>

  <span data-ttu-id="237e6-232">若您收到與下列內容相似的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="237e6-232">If you get an exception similar to one of the following:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-233">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-233">Visual Studio</span></span>](#tab/visual-studio)

  ```console
  SqlException: Cannot open database "MvcMovieContext-1" requested by the login. The login failed.
  ```

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-234">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-234">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

  ```console
  SqliteException: SQLite Error 1: 'no such table: Movie'.
  ```

---
  <span data-ttu-id="237e6-235">您可能遺漏了[移轉步驟](#migration)。</span><span class="sxs-lookup"><span data-stu-id="237e6-235">You probably missed the [migrations step](#migration).</span></span>

* <span data-ttu-id="237e6-236">測試 [**建立**] 頁面。</span><span class="sxs-lookup"><span data-stu-id="237e6-236">Test the **Create** page.</span></span> <span data-ttu-id="237e6-237">輸入並提交資料。</span><span class="sxs-lookup"><span data-stu-id="237e6-237">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="237e6-238">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="237e6-238">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="237e6-239">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="237e6-239">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="237e6-240">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="237e6-240">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="237e6-241">測試 **Edit**、**Details** 和 **Delete** 頁面。</span><span class="sxs-lookup"><span data-stu-id="237e6-241">Test the **Edit**, **Details**, and **Delete** pages.</span></span>

## <a name="dependency-injection-in-the-controller"></a><span data-ttu-id="237e6-242">控制器中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="237e6-242">Dependency injection in the controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-243">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-243">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-244">開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：</span><span class="sxs-lookup"><span data-stu-id="237e6-244">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller (or use the old one as I did in the 3.0 upgrade) because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="237e6-245">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="237e6-245">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="237e6-246">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-246">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-247">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-247">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="237e6-248">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="237e6-248">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="237e6-249">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-249">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

### <a name="use-sqlite-for-development-sql-server-for-production"></a><span data-ttu-id="237e6-250">使用 SQLite 進行開發，SQL Server 用於生產環境</span><span class="sxs-lookup"><span data-stu-id="237e6-250">Use SQLite for development, SQL Server for production</span></span>

<span data-ttu-id="237e6-251">選取 SQLite 時，範本產生的程式碼就已準備好進行開發。</span><span class="sxs-lookup"><span data-stu-id="237e6-251">When SQLite is selected, the template generated code is ready for development.</span></span> <span data-ttu-id="237e6-252">下列程式碼示範如何插入 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 啟動。</span><span class="sxs-lookup"><span data-stu-id="237e6-252">The following code shows how to inject <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> into Startup.</span></span> <span data-ttu-id="237e6-253">`IWebHostEnvironment`已插入，因此 `ConfigureServices` 可以在開發中使用 SQLite，並在生產環境中 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="237e6-253">`IWebHostEnvironment` is injected so `ConfigureServices` can use SQLite in development and SQL Server in production.</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/StartupDevProd.cs?name=snippet_StartupClass&highlight=5,10,16-28)]

---
<!-- end of tabs --->

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="237e6-254">強型別模型和 @model 關鍵字</span><span class="sxs-lookup"><span data-stu-id="237e6-254">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="237e6-255">稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。</span><span class="sxs-lookup"><span data-stu-id="237e6-255">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="237e6-256">`ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="237e6-256">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="237e6-257">MVC 也提供將強型別模型物件傳遞至檢視的能力。</span><span class="sxs-lookup"><span data-stu-id="237e6-257">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="237e6-258">這種強型別的方法可在編譯時檢查程式碼。</span><span class="sxs-lookup"><span data-stu-id="237e6-258">This strongly typed approach enables compile time code checking.</span></span> <span data-ttu-id="237e6-259">此方法所使用的 scaffolding 機制 (即傳遞強型別模型)，包括 `MoviesController` 類別和檢視。</span><span class="sxs-lookup"><span data-stu-id="237e6-259">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views.</span></span>

<span data-ttu-id="237e6-260">檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：</span><span class="sxs-lookup"><span data-stu-id="237e6-260">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="237e6-261">`id` 參數通常會傳遞為路由資料。</span><span class="sxs-lookup"><span data-stu-id="237e6-261">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="237e6-262">例如，`https://localhost:5001/movies/details/1` 設定：</span><span class="sxs-lookup"><span data-stu-id="237e6-262">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="237e6-263">控制器為 `movies` 控制器 (第一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="237e6-263">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="237e6-264">動作為 `details` (第二個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="237e6-264">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="237e6-265">識別碼為 1 (最後一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="237e6-265">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="237e6-266">您也可以在 `id` 中使用查詢字串傳遞，如下所示：</span><span class="sxs-lookup"><span data-stu-id="237e6-266">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="237e6-267">`id`參數會定義為[可為 null 的類型](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) ，以防未提供識別碼值。</span><span class="sxs-lookup"><span data-stu-id="237e6-267">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="237e6-268">[Lambda 運算式](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `FirstOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。</span><span class="sxs-lookup"><span data-stu-id="237e6-268">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="237e6-269">如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：</span><span class="sxs-lookup"><span data-stu-id="237e6-269">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
```

<span data-ttu-id="237e6-270">檢查 *Views/Movies/Details.cshtml* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="237e6-270">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="237e6-271">檢視檔案頂端的 `@model` 陳述式會指定檢視所預期物件型別。</span><span class="sxs-lookup"><span data-stu-id="237e6-271">The `@model` statement at the top of the view file specifies the type of object that the view expects.</span></span> <span data-ttu-id="237e6-272">建立影片控制器時，會包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="237e6-272">When the movie controller was created, the following `@model` statement was included:</span></span>

```cshtml
@model MvcMovie.Models.Movie
```

<span data-ttu-id="237e6-273">此 `@model` 指示詞會允許存取控制器傳遞給檢視的影片。</span><span class="sxs-lookup"><span data-stu-id="237e6-273">This `@model` directive allows access to the movie that the controller passed to the view.</span></span> <span data-ttu-id="237e6-274">`Model` 物件為強型別物件。</span><span class="sxs-lookup"><span data-stu-id="237e6-274">The `Model` object is strongly typed.</span></span> <span data-ttu-id="237e6-275">例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="237e6-275">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="237e6-276">`Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。</span><span class="sxs-lookup"><span data-stu-id="237e6-276">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="237e6-277">檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="237e6-277">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="237e6-278">請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。</span><span class="sxs-lookup"><span data-stu-id="237e6-278">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="237e6-279">此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="237e6-279">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="237e6-280">建立影片控制器時，scaffolding 會在 *Index.cshtml* 檔案的頂端包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="237e6-280">When the movies controller was created, scaffolding included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="237e6-281">`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。</span><span class="sxs-lookup"><span data-stu-id="237e6-281">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="237e6-282">例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：</span><span class="sxs-lookup"><span data-stu-id="237e6-282">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="237e6-283">因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。</span><span class="sxs-lookup"><span data-stu-id="237e6-283">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="237e6-284">撇開其他優點，這表示您會進行程式碼編譯時期檢查。</span><span class="sxs-lookup"><span data-stu-id="237e6-284">Among other benefits, this means that you get compile time checking of the code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="237e6-285">其他資源</span><span class="sxs-lookup"><span data-stu-id="237e6-285">Additional resources</span></span>

* [<span data-ttu-id="237e6-286">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="237e6-286">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="237e6-287">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="237e6-287">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="237e6-288">[先前加入視圖](adding-view.md) 
> [下一步使用 SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="237e6-288">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

## <a name="add-a-data-model-class"></a><span data-ttu-id="237e6-289">新增資料模型類別</span><span class="sxs-lookup"><span data-stu-id="237e6-289">Add a data model class</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-290">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-290">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-291">以滑鼠右鍵按一下 *Models* 資料夾 > [新增]\*\*\*\* > [類別]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="237e6-291">Right-click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="237e6-292">將類別命名為 **Movie**。</span><span class="sxs-lookup"><span data-stu-id="237e6-292">Name the class **Movie**.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-293">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-293">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="237e6-294">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="237e6-294">Add a class to the *Models* folder named *Movie.cs*.</span></span>

[!INCLUDE [model 1b](~/includes/mvc-intro/model1b.md)]
[!INCLUDE [model 2](~/includes/mvc-intro/model2.md)]

---

## <a name="scaffold-the-movie-model"></a><span data-ttu-id="237e6-295">Scaffold 影片模型</span><span class="sxs-lookup"><span data-stu-id="237e6-295">Scaffold the movie model</span></span>

<span data-ttu-id="237e6-296">在本節中會 scaffold 影片模型。</span><span class="sxs-lookup"><span data-stu-id="237e6-296">In this section, the movie model is scaffolded.</span></span> <span data-ttu-id="237e6-297">亦即 Scaffolding 工具會產生影片模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="237e6-297">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the movie model.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-298">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-298">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-299">在 [方案總管]\*\*\*\* 中以滑鼠右鍵按一下 *Controllers* 資料夾 > [新增] > [新增 Scaffold 項目]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="237e6-299">In **Solution Explorer**, right-click the *Controllers* folder **> Add > New Scaffolded Item**.</span></span>

![上方步驟的檢視](adding-model/_static/add_controller21.png)

<span data-ttu-id="237e6-301">在 [新增 Scaffold]\*\*\*\* 對話方塊中，選取 [使用 Entity Framework 執行檢視的 MVC 控制器] > [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="237e6-301">In the **Add Scaffold** dialog, select **MVC Controller with views, using Entity Framework > Add**.</span></span>

![[新增 Scaffold] 對話方塊](adding-model/_static/add_scaffold21.png)

<span data-ttu-id="237e6-303">完成 [新增控制器]\*\*\*\* 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="237e6-303">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="237e6-304">**模型類別:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="237e6-304">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="237e6-305">**資料內容類別：** 選取 **+** 圖示並新增預設的 **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="237e6-305">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![新增資料內容](adding-model/_static/dc.png)

* <span data-ttu-id="237e6-307">**檢視：** 保持核取預設的每一個選項</span><span class="sxs-lookup"><span data-stu-id="237e6-307">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="237e6-308">**控制器名稱：** 保留預設值 *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="237e6-308">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="237e6-309">選取 [新增]</span><span class="sxs-lookup"><span data-stu-id="237e6-309">Select **Add**</span></span>

![[新增控制器] 對話方塊](adding-model/_static/add_controller2.png)

<span data-ttu-id="237e6-311">Visual Studio 會建立：</span><span class="sxs-lookup"><span data-stu-id="237e6-311">Visual Studio creates:</span></span>

* <span data-ttu-id="237e6-312">Entity Framework Core [資料庫內容類別](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="237e6-312">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="237e6-313">電影控制器 (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="237e6-313">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="237e6-314">Razor查看用於建立、刪除、詳細資料、編輯和索引頁面的檔案 (*Views/電影/ \* cshtml*) </span><span class="sxs-lookup"><span data-stu-id="237e6-314">Razor view files for Create, Delete, Details, Edit, and Index pages (*Views/Movies/\*.cshtml*)</span></span>

<span data-ttu-id="237e6-315">自動建立資料庫內容與 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (建立、讀取、更新和刪除) 動作方法和檢視稱為 *Scaffolding*。</span><span class="sxs-lookup"><span data-stu-id="237e6-315">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="237e6-316">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="237e6-316">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!--  Until https://github.com/aspnet/Scaffolding/issues/582 is fixed windows needs backslash or the namespace is namespace RazorPagesMovie.Pages_Movies rather than namespace RazorPagesMovie.Pages.Movies
-->

* <span data-ttu-id="237e6-317">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="237e6-317">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="237e6-318">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="237e6-318">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="237e6-319">在 Linux 上，匯出 scaffold 工具路徑：</span><span class="sxs-lookup"><span data-stu-id="237e6-319">On Linux, export the scaffold tool path:</span></span>

  ```console
    export PATH=$HOME/.dotnet/tools:$PATH
  ```

* <span data-ttu-id="237e6-320">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-320">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

<!-- Mac -------------------------->

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="237e6-321">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-321">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="237e6-322">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="237e6-322">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="237e6-323">安裝 Scaffolding 工具：</span><span class="sxs-lookup"><span data-stu-id="237e6-323">Install the scaffolding tool:</span></span>

  ```dotnetcli
   dotnet tool install --global dotnet-aspnet-codegenerator
   ```

* <span data-ttu-id="237e6-324">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-324">Run the following command:</span></span>

  ```dotnetcli
   dotnet aspnet-codegenerator controller -name MoviesController -m Movie -dc MvcMovieContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

[!INCLUDE [explains scaffold generated params](~/includes/mvc-intro/model4.md)]

---

<!-- End of VS tabs                  -->

<span data-ttu-id="237e6-325">如果執行應用程式，然後按一下 **Mvc Movie** 連結，則會收到如下錯誤：</span><span class="sxs-lookup"><span data-stu-id="237e6-325">If you run the app and click on the **Mvc Movie** link, you get an error similar to the following:</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-326">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-326">Visual Studio</span></span>](#tab/visual-studio)

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString
```

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-327">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-327">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

```
An unhandled exception occurred while processing the request.
SqliteException: SQLite Error 1: 'no such table: Movie'.
Microsoft.Data.Sqlite.SqliteException.ThrowExceptionForRC(int rc, sqlite3 db)

```

---

<span data-ttu-id="237e6-328">您需要建立資料庫，而且您會使用 EF Core [移轉](xref:data/ef-mvc/migrations)功能來執行此作業。</span><span class="sxs-lookup"><span data-stu-id="237e6-328">You need to create the database, and you use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="237e6-329">移轉可讓您建立符合資料模型的資料庫，並在資料模型變更時，更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="237e6-329">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

<a name="pmc"></a>

## <a name="initial-migration"></a><span data-ttu-id="237e6-330">初始移轉</span><span class="sxs-lookup"><span data-stu-id="237e6-330">Initial migration</span></span>

<span data-ttu-id="237e6-331">在本節中，將會完成下列作業：</span><span class="sxs-lookup"><span data-stu-id="237e6-331">In this section, the following tasks are completed:</span></span>

* <span data-ttu-id="237e6-332">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="237e6-332">Add an initial migration.</span></span>
* <span data-ttu-id="237e6-333">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-333">Update the database with the initial migration.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-334">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-334">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="237e6-335">從 [工具]\*\*\*\* 功能表中，選取 [NuGet 套件管理員]\*\* [套件管理器主控台]\*\* > \*\*\*\* (PMC)。</span><span class="sxs-lookup"><span data-stu-id="237e6-335">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console** (PMC).</span></span>

   ![PMC 功能表](~/tutorials/first-mvc-app/adding-model/_static/pmc.png)

1. <span data-ttu-id="237e6-337">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="237e6-337">In the PMC, enter the following commands:</span></span>

   ```powershell
   Add-Migration Initial
   Update-Database
   ```

   <span data-ttu-id="237e6-338">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="237e6-338">The `Add-Migration` command generates code to create the initial database schema.</span></span>

   <span data-ttu-id="237e6-339">資料庫結構描述是以 `MvcMovieContext` 類別為基礎。</span><span class="sxs-lookup"><span data-stu-id="237e6-339">The database schema is based on the model specified in the `MvcMovieContext` class.</span></span> <span data-ttu-id="237e6-340">`Initial` 引數是移轉名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-340">The `Initial` argument is the migration name.</span></span> <span data-ttu-id="237e6-341">您可以使用任何名稱，但依照慣例，會使用描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-341">Any name can be used, but by convention, a name that describes the migration is used.</span></span> <span data-ttu-id="237e6-342">如需詳細資訊，請參閱<xref:data/ef-mvc/migrations>。</span><span class="sxs-lookup"><span data-stu-id="237e6-342">For more information, see <xref:data/ef-mvc/migrations>.</span></span>

   <span data-ttu-id="237e6-343">`Update-Database` 命令會執行 *Migrations/{時間戳記}_InitialCreate.cs* 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="237e6-343">The `Update-Database` command runs the `Up` method in the *Migrations/{time-stamp}_InitialCreate.cs* file, which creates the database.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-344">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-344">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE [initial migration](~/includes/RP/model3.md)]

<span data-ttu-id="237e6-345">`ef migrations add InitialCreate` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="237e6-345">The `ef migrations add InitialCreate` command generates code to create the initial database schema.</span></span>

<span data-ttu-id="237e6-346">資料庫結構描述會以 `MvcMovieContext` 類別 (在 *Data/MvcMovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="237e6-346">The database schema is based on the model specified in the `MvcMovieContext` class (in the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="237e6-347">`InitialCreate` 引數是移轉名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-347">The `InitialCreate` argument is the migration name.</span></span> <span data-ttu-id="237e6-348">您可以使用任何名稱，但依照慣例，會選取描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="237e6-348">Any name can be used, but by convention, a name is selected that describes the migration.</span></span>

---

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="237e6-349">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="237e6-349">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="237e6-350">ASP.NET Core 內建[相依性插入 (DI)](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="237e6-350">ASP.NET Core is built with [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="237e6-351">服務 (例如 EF Core DB 內容) 會在應用程式啟動期間使用 DI 來註冊。</span><span class="sxs-lookup"><span data-stu-id="237e6-351">Services (such as the EF Core DB context) are registered with DI during application startup.</span></span> <span data-ttu-id="237e6-352">需要這些服務的元件 (例如頁面) 會透過「函式 Razor 參數」提供這些服務。</span><span class="sxs-lookup"><span data-stu-id="237e6-352">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="237e6-353">取得資料庫內容執行個體的建構函式程式碼，本教學課程中稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="237e6-353">The constructor code that gets a DB context instance is shown later in the tutorial.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="237e6-354">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="237e6-354">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="237e6-355">Scaffolding 工具會自動建立 DB 內容，並向 DI 容器註冊該內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-355">The scaffolding tool automatically created a DB context and registered it with the DI container.</span></span>

<span data-ttu-id="237e6-356">請檢查下列 `Startup.ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="237e6-356">Examine the following `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="237e6-357">強調顯示的行由 Scaffolder 新增：</span><span class="sxs-lookup"><span data-stu-id="237e6-357">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=14-15)]

<span data-ttu-id="237e6-358">`MvcMovieContext` 會協調 `Movie` 模型的 EF Core 功能 (建立、更新、刪除等)。</span><span class="sxs-lookup"><span data-stu-id="237e6-358">The `MvcMovieContext` coordinates EF Core functionality (Create, Read, Update, Delete, etc.) for the `Movie` model.</span></span> <span data-ttu-id="237e6-359">資料內容 (`MvcMovieContext`) 衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="237e6-359">The data context (`MvcMovieContext`) is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="237e6-360">資料內容會指定資料模型包含哪些實體：</span><span class="sxs-lookup"><span data-stu-id="237e6-360">The data context specifies which entities are included in the data model:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie3/Data/MvcMovieContext.cs)]

<span data-ttu-id="237e6-361">上述程式碼會建立實體集的[DbSet \<Movie> ](/dotnet/api/microsoft.entityframeworkcore.dbset-1)屬性。</span><span class="sxs-lookup"><span data-stu-id="237e6-361">The preceding code creates a [DbSet\<Movie>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for the entity set.</span></span> <span data-ttu-id="237e6-362">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="237e6-362">In Entity Framework terminology, an entity set typically corresponds to a database table.</span></span> <span data-ttu-id="237e6-363">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="237e6-363">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="237e6-364">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-364">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="237e6-365">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="237e6-365">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="237e6-366">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="237e6-366">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="237e6-367">您已建立 DB 內容，並向 DI 容器註冊該內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-367">You created a DB context and registered it with the DI container.</span></span>

---

<a name="test"></a>

### <a name="test-the-app"></a><span data-ttu-id="237e6-368">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="237e6-368">Test the app</span></span>

* <span data-ttu-id="237e6-369">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL ( `http://localhost:port/movies` )。</span><span class="sxs-lookup"><span data-stu-id="237e6-369">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>

<span data-ttu-id="237e6-370">如果您收到類似如下的資料庫例外狀況：</span><span class="sxs-lookup"><span data-stu-id="237e6-370">If you get a database exception similar to the following:</span></span>

```console
SqlException: Cannot open database "MvcMovieContext-GUID" requested by the login. The login failed.
Login failed for user 'User-name'.
```

<span data-ttu-id="237e6-371">您遺失了[移轉步驟](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="237e6-371">You missed the [migrations step](#pmc).</span></span>

* <span data-ttu-id="237e6-372">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="237e6-372">Test the **Create** link.</span></span> <span data-ttu-id="237e6-373">輸入並提交資料。</span><span class="sxs-lookup"><span data-stu-id="237e6-373">Enter and submit data.</span></span>

  > [!NOTE]
  > <span data-ttu-id="237e6-374">您可能無法在 `Price` 欄位中輸入小數逗號。</span><span class="sxs-lookup"><span data-stu-id="237e6-374">You may not be able to enter decimal commas in the `Price` field.</span></span> <span data-ttu-id="237e6-375">若要對使用逗號 (",") 作為小數點的非英文地區設定和非英文日期格式支援 [jQuery 驗證](https://jqueryvalidation.org/)，則必須將應用程式全球化。</span><span class="sxs-lookup"><span data-stu-id="237e6-375">To support [jQuery validation](https://jqueryvalidation.org/) for non-English locales that use a comma (",") for a decimal point and for non US-English date formats, the app must be globalized.</span></span> <span data-ttu-id="237e6-376">如需全球化指示，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="237e6-376">For globalization instructions, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/4076#issuecomment-326590420).</span></span>

* <span data-ttu-id="237e6-377">測試 [編輯]\*\*\*\*、[詳細資料]\*\*\*\* 和 [刪除]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="237e6-377">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="237e6-378">檢查 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="237e6-378">Examine the `Startup` class:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Startup.cs?name=snippet_ConfigureServices&highlight=13-99)]

<span data-ttu-id="237e6-379">上述醒目提示的程式碼會顯示要新增至[相依性插入](xref:fundamentals/dependency-injection)容器的電影資料庫內容：</span><span class="sxs-lookup"><span data-stu-id="237e6-379">The preceding highlighted code shows the movie database context being added to the [Dependency Injection](xref:fundamentals/dependency-injection) container:</span></span>

* <span data-ttu-id="237e6-380">`services.AddDbContext<MvcMovieContext>(options =>` 指定要使用的資料庫和連線字串。</span><span class="sxs-lookup"><span data-stu-id="237e6-380">`services.AddDbContext<MvcMovieContext>(options =>` specifies the database to use and the connection string.</span></span>
* <span data-ttu-id="237e6-381">`=>`是[Lambda 運算子](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span><span class="sxs-lookup"><span data-stu-id="237e6-381">`=>` is a [lambda operator](/dotnet/articles/csharp/language-reference/operators/lambda-operator)</span></span>

<span data-ttu-id="237e6-382">開啟 *Controllers/MoviesController.cs* 檔案，並檢查建構函式：</span><span class="sxs-lookup"><span data-stu-id="237e6-382">Open the *Controllers/MoviesController.cs* file and examine the constructor:</span></span>

<!-- l.. Make copy of Movies controller because we comment out the initial index method and update it later  -->

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_1)]

<span data-ttu-id="237e6-383">建構函式會使用[相依性插入](xref:fundamentals/dependency-injection)將資料庫內容 (`MvcMovieContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="237e6-383">The constructor uses [Dependency Injection](xref:fundamentals/dependency-injection) to inject the database context (`MvcMovieContext`) into the controller.</span></span> <span data-ttu-id="237e6-384">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="237e6-384">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<a name="strongly-typed-models-keyword-label"></a>
<a name="strongly-typed-models-and-the--keyword"></a>

## <a name="strongly-typed-models-and-the-model-keyword"></a><span data-ttu-id="237e6-385">強型別模型和 @model 關鍵字</span><span class="sxs-lookup"><span data-stu-id="237e6-385">Strongly typed models and the @model keyword</span></span>

<span data-ttu-id="237e6-386">稍早在本教學課程中，您已看到控制器如何使用 `ViewData` 字典傳遞資料或物件給檢視。</span><span class="sxs-lookup"><span data-stu-id="237e6-386">Earlier in this tutorial, you saw how a controller can pass data or objects to a view using the `ViewData` dictionary.</span></span> <span data-ttu-id="237e6-387">`ViewData` 字典是一種動態物件，提供便利的晚期繫結方式，將資訊傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="237e6-387">The `ViewData` dictionary is a dynamic object that provides a convenient late-bound way to pass information to a view.</span></span>

<span data-ttu-id="237e6-388">MVC 也提供將強型別模型物件傳遞至檢視的能力。</span><span class="sxs-lookup"><span data-stu-id="237e6-388">MVC also provides the ability to pass strongly typed model objects to a view.</span></span> <span data-ttu-id="237e6-389">強型別方法可讓程式碼的編譯時期檢查變得更佳。</span><span class="sxs-lookup"><span data-stu-id="237e6-389">This strongly typed approach enables better compile time checking of your code.</span></span> <span data-ttu-id="237e6-390">Scaffolding 機制在建立方法和檢視時，已使用此方法 (也就是傳遞強型別模型) 來搭配 `MoviesController` 類別和檢視。</span><span class="sxs-lookup"><span data-stu-id="237e6-390">The scaffolding mechanism used this approach (that is, passing a strongly typed model) with the `MoviesController` class and views when it created the methods and views.</span></span>

<span data-ttu-id="237e6-391">檢查 *Controllers/MoviesController.cs* 檔案中產生的 `Details` 方法：</span><span class="sxs-lookup"><span data-stu-id="237e6-391">Examine the generated `Details` method in the *Controllers/MoviesController.cs* file:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_details)]

<span data-ttu-id="237e6-392">`id` 參數通常會傳遞為路由資料。</span><span class="sxs-lookup"><span data-stu-id="237e6-392">The `id` parameter is generally passed as route data.</span></span> <span data-ttu-id="237e6-393">例如，`https://localhost:5001/movies/details/1` 設定：</span><span class="sxs-lookup"><span data-stu-id="237e6-393">For example `https://localhost:5001/movies/details/1` sets:</span></span>

* <span data-ttu-id="237e6-394">控制器為 `movies` 控制器 (第一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="237e6-394">The controller to the `movies` controller (the first URL segment).</span></span>
* <span data-ttu-id="237e6-395">動作為 `details` (第二個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="237e6-395">The action to `details` (the second URL segment).</span></span>
* <span data-ttu-id="237e6-396">識別碼為 1 (最後一個 URL 區段)。</span><span class="sxs-lookup"><span data-stu-id="237e6-396">The id to 1 (the last URL segment).</span></span>

<span data-ttu-id="237e6-397">您也可以在 `id` 中使用查詢字串傳遞，如下所示：</span><span class="sxs-lookup"><span data-stu-id="237e6-397">You can also pass in the `id` with a query string as follows:</span></span>

`https://localhost:5001/movies/details?id=1`

<span data-ttu-id="237e6-398">`id`參數會定義為[可為 null 的類型](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) ，以防未提供識別碼值。</span><span class="sxs-lookup"><span data-stu-id="237e6-398">The `id` parameter is defined as a [nullable type](/dotnet/csharp/programming-guide/nullable-types/index) (`int?`) in case an ID value isn't provided.</span></span>

<span data-ttu-id="237e6-399">[Lambda 運算式](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions)會傳遞至 `FirstOrDefaultAsync`，以選取符合路由資料或查詢字串值的電影實體。</span><span class="sxs-lookup"><span data-stu-id="237e6-399">A [lambda expression](/dotnet/articles/csharp/programming-guide/statements-expressions-operators/lambda-expressions) is passed in to `FirstOrDefaultAsync` to select movie entities that match the route data or query string value.</span></span>

```csharp
var movie = await _context.Movie
    .FirstOrDefaultAsync(m => m.Id == id);
```

<span data-ttu-id="237e6-400">如果找到電影，則 `Movie` 模型的執行個體會傳遞至 `Details` 檢視：</span><span class="sxs-lookup"><span data-stu-id="237e6-400">If a movie is found, an instance of the `Movie` model is passed to the `Details` view:</span></span>

```csharp
return View(movie);
   ```

<span data-ttu-id="237e6-401">檢查 *Views/Movies/Details.cshtml* 檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="237e6-401">Examine the contents of the *Views/Movies/Details.cshtml* file:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/DetailsOriginal.cshtml)]

<span data-ttu-id="237e6-402">藉由在檢視檔案的最上方包含 `@model` 陳述式，您可以指定檢視預期要有的物件類型。</span><span class="sxs-lookup"><span data-stu-id="237e6-402">By including a `@model` statement at the top of the view file, you can specify the type of object that the view expects.</span></span> <span data-ttu-id="237e6-403">當您建立電影控制器時，*Details.cshtml* 檔案的最上方會自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="237e6-403">When you created the movie controller, the following `@model` statement was automatically included at the top of the *Details.cshtml* file:</span></span>

```cshtml
@model MvcMovie.Models.Movie
```

<span data-ttu-id="237e6-404">這個 `@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影。</span><span class="sxs-lookup"><span data-stu-id="237e6-404">This `@model` directive allows you to access the movie that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="237e6-405">例如，在 *Details.cshtml* 檢視中，程式碼會使用強型別的 `Model` 物件，將每個電影欄位傳遞至 `DisplayNameFor` 和 `DisplayFor` HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="237e6-405">For example, in the *Details.cshtml* view, the code passes each movie field to the `DisplayNameFor` and `DisplayFor` HTML Helpers with the strongly typed `Model` object.</span></span> <span data-ttu-id="237e6-406">`Create` 與 `Edit` 方法和檢視也會傳遞 `Movie` 模型物件。</span><span class="sxs-lookup"><span data-stu-id="237e6-406">The `Create` and `Edit` methods and views also pass a `Movie` model object.</span></span>

<span data-ttu-id="237e6-407">檢查電影控制器中的 *Index.cshtml* 檢視和 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="237e6-407">Examine the *Index.cshtml* view and the `Index` method in the Movies controller.</span></span> <span data-ttu-id="237e6-408">請注意程式碼如何在呼叫 `View` 方法時建立 `List` 物件。</span><span class="sxs-lookup"><span data-stu-id="237e6-408">Notice how the code creates a `List` object when it calls the `View` method.</span></span> <span data-ttu-id="237e6-409">此程式碼會從 `Index` 動作方法將 `Movies` 清單傳遞至檢視：</span><span class="sxs-lookup"><span data-stu-id="237e6-409">The code passes this `Movies` list from the `Index` action method to the view:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Controllers/MC1.cs?name=snippet_index)]

<span data-ttu-id="237e6-410">當您建立電影控制器時，Scaffolding 會在 *Index.cshtml* 檔案的最上方自動包含下列 `@model` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="237e6-410">When you created the movies controller, scaffolding automatically included the following `@model` statement at the top of the *Index.cshtml* file:</span></span>

<!-- Copy Index.cshtml to IndexOriginal.cshtml -->

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?range=1)]

<span data-ttu-id="237e6-411">`@model` 指示詞可讓您使用強型別的 `Model` 物件，存取控制器傳遞至檢視的電影清單。</span><span class="sxs-lookup"><span data-stu-id="237e6-411">The `@model` directive allows you to access the list of movies that the controller passed to the view by using a `Model` object that's strongly typed.</span></span> <span data-ttu-id="237e6-412">例如，在 *Index.cshtml* 檢視中，程式碼會透過強型別 `Model` 物件的 `foreach` 陳述式循環存取電影：</span><span class="sxs-lookup"><span data-stu-id="237e6-412">For example, in the *Index.cshtml* view, the code loops through the movies with a `foreach` statement over the strongly typed `Model` object:</span></span>

[!code-cshtml[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexOriginal.cshtml?highlight=1,31,34,37,40,43,46-48)]

<span data-ttu-id="237e6-413">因為 `Model` 物件是強型別 (作為 `IEnumerable<Movie>` 物件)，所以迴圈中每個項目的類型為 `Movie`。</span><span class="sxs-lookup"><span data-stu-id="237e6-413">Because the `Model` object is strongly typed (as an `IEnumerable<Movie>` object), each item in the loop is typed as `Movie`.</span></span> <span data-ttu-id="237e6-414">撇開其他優點，這表示您會進行程式碼編譯時期檢查：</span><span class="sxs-lookup"><span data-stu-id="237e6-414">Among other benefits, this means that you get compile time checking of the code:</span></span>

## <a name="additional-resources"></a><span data-ttu-id="237e6-415">其他資源</span><span class="sxs-lookup"><span data-stu-id="237e6-415">Additional resources</span></span>

* [<span data-ttu-id="237e6-416">標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="237e6-416">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="237e6-417">全球化和當地語系化</span><span class="sxs-lookup"><span data-stu-id="237e6-417">Globalization and localization</span></span>](xref:fundamentals/localization)

> [!div class="step-by-step"]
> <span data-ttu-id="237e6-418">[先前加入視圖](adding-view.md) 
> [下一步使用資料庫](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="237e6-418">[Previous Adding a View](adding-view.md)
[Next Working with a database](working-with-sql.md)</span></span>

::: moniker-end
