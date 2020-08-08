---
title: RazorASP.NET Core 中的頁面單元測試
author: rick-anderson
description: 瞭解如何建立 Razor 頁面應用程式的單元測試。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 7/22/2020
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
uid: test/razor-pages-tests
ms.openlocfilehash: 21810f00548ad9f9c399ec4e453dd914dc186f5d
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88018945"
---
# <a name="no-locrazor-pages-unit-tests-in-aspnet-core"></a>RazorASP.NET Core 中的頁面單元測試

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 支援 Razor 頁面應用程式的單元測試。  (DAL) 和頁面模型的資料存取層測試，有助於確保：

* 頁面應用程式的各個部分會 Razor 在應用程式建立期間獨立和一個單位一起工作。
* 類別和方法的責任範圍有限。
* 應用程式的行為方式有其他檔。
* 自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。

本主題假設您對 Razor 頁面應用程式和單元測試有基本瞭解。 如果您不熟悉 Razor 頁面應用程式或測試概念，請參閱下列主題：

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([如何下載](xref:index#how-to-download-a-sample)) 

範例專案是由兩個應用程式所組成：

| App         | 專案資料夾                     | 描述 |
| ----------- | ---------------------------------- | ----------- |
| 訊息應用程式 | *src/ Razor PagesTestSample*         | 可讓使用者新增訊息、刪除一則訊息、刪除所有訊息，以及分析訊息 (找出每個訊息) 的平均單字數目。 |
| 測試應用程式    | *測試/ Razor PagesTestSample。測試* | 用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。 |

測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/ Razor PagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>訊息應用程式組織

訊息應用程式是 Razor 具有下列特性的頁面訊息系統：

* 應用程式的 [索引] 頁面 (*pages/index. cshtml*和*pages/index. .CS*) 提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析 (尋找每個訊息) 的平均單字數目。
* 訊息由類別描述， `Message` (*Data/message .Cs*) 有兩個屬性： (索引 `Id` 鍵) 和 `Text` (訊息) 。 `Text`屬性是必要的，而且限制為200個字元。
* 訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。
* 應用程式在其資料庫內容類別中包含 DAL， `AppDbContext` (*Data/AppDbCoNtext .cs*) 。 DAL 方法已標記 `virtual` ，可讓您模擬要在測試中使用的方法。
* 如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。 這些*植*入的訊息也會用於測試中。

&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 跨不同測試架構的測試概念和測試執行類似，但不完全相同。

雖然範例應用程式不會使用存放庫模式，而且不是[ (UoW) 模式之工作單位](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效範例，但 Razor 頁面支援這些開發模式。 如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和 <xref:mvc/controllers/testing> (範例會) 中執行存放庫模式。

## <a name="test-app-organization"></a>測試應用程式組織

測試應用程式是 [測試]/[ * Razor PagesTestSample* ] 資料夾內的主控台應用程式。

| 測試應用程式資料夾 | 描述 |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs*包含 DAL 的單元測試。</li><li>*IndexPageTests.cs*包含索引頁面模型的單元測試。</li></ul> |
| *公用程式*     | 包含 `TestDbContextOptions` 用來為每個 DAL 單元測試建立新資料庫內容選項的方法，以便將資料庫重設為每個測試的基準條件。 |

測試架構為[xUnit](https://xunit.github.io/)。 物件模擬架構為[Moq](https://github.com/moq/moq4)。

## <a name="unit-tests-of-the-data-access-layer-dal"></a>資料存取層的單元測試 (DAL) 

訊息應用程式的 DAL 具有四個包含在類別中的方法 `AppDbContext` (*src/ Razor PagesTestSample/Data/AppDbCoNtext .cs*) 。 每個方法在測試應用程式中都有一個或兩個單元測試。

| DAL 方法               | 函式                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | `List<Message>`從依屬性排序的資料庫取得 `Text` 。 |
| `AddMessageAsync`        | 將加入 `Message` 至資料庫。                                          |
| `DeleteAllMessagesAsync` | 刪除 `Message` 資料庫中的所有專案。                           |
| `DeleteMessageAsync`     | `Message`從資料庫刪除單一 `Id` 。                      |

<xref:Microsoft.EntityFrameworkCore.DbContextOptions>建立每個測試的新時，DAL 的單元測試需要 `AppDbContext` 。 為每個測試建立的其中一個方法 `DbContextOptions` 是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder> ：

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。 嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。 若要強制將 `AppDbContext` 新的資料庫內容用於每個測試，請提供以 `DbContextOptions` 新服務提供者為基礎的實例。 測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` (測試/PagesTestSample 來執行此動作* Razor 。測試/公用程式/公用程式 .cs*) ：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

`DbContextOptions`在 DAL 單元測試中使用，可讓每個測試以自動方式與全新的資料庫實例一起執行：

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

類別中的每個測試方法 `DataAccessLayerTest` (*Run-unittests/DataAccessLayerTest*) 遵循類似的相片順序-判斷法模式：

1. 排列：已針對測試設定資料庫，並（或）定義預期的結果。
1. Act：執行測試。
1. 判斷提示：決定測試結果是否成功的判斷提示。

例如， `DeleteMessageAsync` 方法會負責移除其 `Id` (*src/ Razor PagesTestSample/Data/AppDbCoNtext*) 所識別的單一訊息：

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

這個方法有兩個測試。 一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。 另一個方法會測試如果要刪除的訊息不存在，資料庫不會變更 `Id` 。 `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：

[!code-csharp[](razor-pages-tests/samples_snapshot/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。 植入訊息會取得並保留在中 `seedMessages` 。 植入訊息會儲存到資料庫中。 具有 `Id` 之的訊息 `1` 已設定為要刪除。 當 `DeleteMessageAsync` 執行方法時，預期的訊息應該會有所有訊息，但是除了具有的以外， `Id` `1` `expectedMessages`變數代表此預期的結果。

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法的作用： `DeleteMessageAsync` 方法會以傳遞的來執行 `recId` `1` ：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最後，方法 `Messages` 會從內容取得，並將它與判斷提示 `expectedMessages` 兩者相等的判斷提示：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

若要比較兩者 `List<Message>` 相同：

* 訊息會依排序 `Id` 。
* 訊息配對會在屬性上進行比較 `Text` 。

類似的測試方法， `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 會檢查嘗試刪除不存在之訊息的結果。 在此情況下，資料庫中預期的訊息應該等於執行方法之後的實際訊息 `DeleteMessageAsync` 。 資料庫的內容應該不會有任何變更：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>頁面模型方法的單元測試

另一組單元測試負責測試頁面模型方法。 在訊息應用程式中，會在 `IndexModel` 類別中的*src/ Razor PagesTestSample/Pages/index. Cshtml*中找到索引頁面模型。

| 頁面模型方法 | 函式 |
| ----------------- | -------- |
| `OnGetAsync` | 使用方法，從 DAL 取得適用于 UI 的訊息 `GetMessagesAsync` 。 |
| `OnPostAddMessageAsync` | 如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 以將訊息新增至資料庫。 |
| `OnPostDeleteAllMessagesAsync` | 呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。 |
| `OnPostDeleteMessageAsync` | 執行 `DeleteMessageAsync` 以刪除具有指定之的訊息 `Id` 。 |
| `OnPostAnalyzeMessagesAsync` | 如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。 |

頁面模型方法會使用 (測試/PagesTestSample 的類別中的七個測試進行測試 `IndexPageTests` * Razor 。測試/run-unittests/IndexPageTests*) 。 測試會使用熟悉的「排列-判斷提示-Act」模式。 這些測試著重于：

* 判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。
* 確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult> 。
* 檢查是否已正確設定屬性值。

這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。 例如，的 `GetMessagesAsync` 方法 `AppDbContext` 會模擬以產生輸出。 當頁面模型方法執行這個方法時，mock 會傳回結果。 資料不是來自資料庫。 這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。

此 `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何 `GetMessagesAsync` 模擬頁面模型的方法：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。

測試/PagesTestSample 的單元測試 Act 步驟 (test * Razor /Run-unittests/IndexPageTests*) ：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`頁面模型的 `OnGetAsync` 方法 (*src/ Razor PagesTestSample/Pages/Index. cshtml. .cs*) ：

[!code-csharp[](razor-pages-tests/samples/3.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync`DAL 中的方法不會傳回這個方法呼叫的結果。 模擬版本的方法會傳回結果。

在此 `Assert` 步驟中， () 的實際訊息 `actualMessages` 是從頁面模型的屬性指派而來 `Messages` 。 指派訊息時也會執行類型檢查。 預期和實際的訊息會依其屬性進行比較 `Text` 。 測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

此群組中的其他測試會建立頁面模型物件，其中包含 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext> 、、 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary> 、， <xref:Microsoft.AspNetCore.Mvc.ActionContext> 以建立 `PageContext` 、 `ViewDataDictionary` 和 `PageContext` 。 這些適用于進行測試。 例如，訊息應用程式 `ModelState` 會與建立錯誤， <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 以檢查 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 執行時傳回的有效 `OnPostAddMessageAsync` ：

[!code-csharp[](razor-pages-tests/samples/3.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>其他資源

* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [對程式碼進行單元測試](/visualstudio/test/unit-test-your-code) (Visual Studio) 
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq 快速入門](https://github.com/Moq/moq4/wiki/Quickstart)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 支援 Razor 頁面應用程式的單元測試。  (DAL) 和頁面模型的資料存取層測試，有助於確保：

* 頁面應用程式的各個部分會 Razor 在應用程式建立期間獨立和一個單位一起工作。
* 類別和方法的責任範圍有限。
* 應用程式的行為方式有其他檔。
* 自動建立和部署期間會發現回歸（這是程式碼更新所帶來的錯誤）。

本主題假設您對 Razor 頁面應用程式和單元測試有基本瞭解。 如果您不熟悉 Razor 頁面應用程式或測試概念，請參閱下列主題：

* <xref:razor-pages/index>
* <xref:tutorials/razor-pages/razor-pages-start>
* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/test/razor-pages-tests/samples) ([如何下載](xref:index#how-to-download-a-sample)) 

範例專案是由兩個應用程式所組成：

| App         | 專案資料夾                     | 描述 |
| ----------- | ---------------------------------- | ----------- |
| 訊息應用程式 | *src/ Razor PagesTestSample*         | 可讓使用者新增訊息、刪除一則訊息、刪除所有訊息，以及分析訊息 (找出每個訊息) 的平均單字數目。 |
| 測試應用程式    | *測試/ Razor PagesTestSample。測試* | 用來對訊息應用程式的 DAL 和索引頁面模型進行單元測試。 |

測試可以使用 IDE 的內建測試功能來執行，例如[Visual Studio](/visualstudio/test/unit-test-your-code)或[Visual Studio for Mac](/dotnet/core/tutorials/using-on-mac-vs-full-solution)。 如果使用[Visual Studio Code](https://code.visualstudio.com/)或命令列，請在 [*測試/ Razor PagesTestSample* ] 中的命令提示字元執行下列命令。 [測試] 資料夾：

```dotnetcli
dotnet test
```

## <a name="message-app-organization"></a>訊息應用程式組織

訊息應用程式是 Razor 具有下列特性的頁面訊息系統：

* 應用程式的 [索引] 頁面 (*pages/index. cshtml*和*pages/index. .CS*) 提供 UI 和頁面模型方法來控制訊息的新增、刪除和分析 (尋找每個訊息) 的平均單字數目。
* 訊息由類別描述， `Message` (*Data/message .Cs*) 有兩個屬性： (索引 `Id` 鍵) 和 `Text` (訊息) 。 `Text`屬性是必要的，而且限制為200個字元。
* 訊息會使用[Entity Framework 的記憶體內部資料庫](/ef/core/providers/in-memory/)&#8224; 儲存。
* 應用程式在其資料庫內容類別中包含 DAL， `AppDbContext` (*Data/AppDbCoNtext .cs*) 。 DAL 方法已標記 `virtual` ，可讓您模擬要在測試中使用的方法。
* 如果在應用程式啟動時資料庫是空的，則會使用三個訊息來初始化訊息存放區。 這些*植*入的訊息也會用於測試中。

&#8224;EF 主題使用[InMemory 進行測試](/ef/core/miscellaneous/testing/in-memory)中，說明如何使用記憶體內部資料庫來測試 MSTest。 本主題使用[xUnit](https://xunit.github.io/)測試架構。 跨不同測試架構的測試概念和測試執行類似，但不完全相同。

雖然範例應用程式不會使用存放庫模式，而且不是[ (UoW) 模式之工作單位](https://martinfowler.com/eaaCatalog/unitOfWork.html)的有效範例，但 Razor 頁面支援這些開發模式。 如需詳細資訊，請參閱[設計基礎結構持續性層](/dotnet/standard/microservices-architecture/microservice-ddd-cqrs-patterns/infrastructure-persistence-layer-design)和 <xref:mvc/controllers/testing> (範例會) 中執行存放庫模式。

## <a name="test-app-organization"></a>測試應用程式組織

測試應用程式是 [測試]/[ * Razor PagesTestSample* ] 資料夾內的主控台應用程式。

| 測試應用程式資料夾 | 描述 |
| --------------- | ----------- |
| *UnitTests*     | <ul><li>*DataAccessLayerTest.cs*包含 DAL 的單元測試。</li><li>*IndexPageTests.cs*包含索引頁面模型的單元測試。</li></ul> |
| *公用程式*     | 包含 `TestDbContextOptions` 用來為每個 DAL 單元測試建立新資料庫內容選項的方法，以便將資料庫重設為每個測試的基準條件。 |

測試架構為[xUnit](https://xunit.github.io/)。 物件模擬架構為[Moq](https://github.com/moq/moq4)。

## <a name="unit-tests-of-the-data-access-layer-dal"></a>資料存取層的單元測試 (DAL) 

訊息應用程式的 DAL 具有四個包含在類別中的方法 `AppDbContext` (*src/ Razor PagesTestSample/Data/AppDbCoNtext .cs*) 。 每個方法在測試應用程式中都有一個或兩個單元測試。

| DAL 方法               | 函式                                                                   |
| ------------------------ | -------------------------------------------------------------------------- |
| `GetMessagesAsync`       | `List<Message>`從依屬性排序的資料庫取得 `Text` 。 |
| `AddMessageAsync`        | 將加入 `Message` 至資料庫。                                          |
| `DeleteAllMessagesAsync` | 刪除 `Message` 資料庫中的所有專案。                           |
| `DeleteMessageAsync`     | `Message`從資料庫刪除單一 `Id` 。                      |

<xref:Microsoft.EntityFrameworkCore.DbContextOptions>建立每個測試的新時，DAL 的單元測試需要 `AppDbContext` 。 為每個測試建立的其中一個方法 `DbContextOptions` 是使用 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder> ：

```csharp
var optionsBuilder = new DbContextOptionsBuilder<AppDbContext>()
    .UseInMemoryDatabase("InMemoryDb");

using (var db = new AppDbContext(optionsBuilder.Options))
{
    // Use the db here in the unit test.
}
```

這種方法的問題在於，每個測試都會以先前的測試所留下的任何狀態來接收資料庫。 嘗試撰寫不會干擾彼此的不可部分完成的單元測試時，這可能會有問題。 若要強制將 `AppDbContext` 新的資料庫內容用於每個測試，請提供以 `DbContextOptions` 新服務提供者為基礎的實例。 測試應用程式會示範如何使用其 `Utilities` 類別方法 `TestDbContextOptions` (測試/PagesTestSample 來執行此動作* Razor 。測試/公用程式/公用程式 .cs*) ：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/Utilities/Utilities.cs?name=snippet1)]

`DbContextOptions`在 DAL 單元測試中使用，可讓每個測試以自動方式與全新的資料庫實例一起執行：

```csharp
using (var db = new AppDbContext(Utilities.TestDbContextOptions()))
{
    // Use the db here in the unit test.
}
```

類別中的每個測試方法 `DataAccessLayerTest` (*Run-unittests/DataAccessLayerTest*) 遵循類似的相片順序-判斷法模式：

1. 排列：已針對測試設定資料庫，並（或）定義預期的結果。
1. Act：執行測試。
1. 判斷提示：決定測試結果是否成功的判斷提示。

例如， `DeleteMessageAsync` 方法會負責移除其 `Id` (*src/ Razor PagesTestSample/Data/AppDbCoNtext*) 所識別的單一訊息：

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Data/AppDbContext.cs?name=snippet4)]

這個方法有兩個測試。 一項測試會檢查當資料庫中有訊息時，方法是否會刪除訊息。 另一個方法會測試如果要刪除的訊息不存在，資料庫不會變更 `Id` 。 `DeleteMessageAsync_MessageIsDeleted_WhenMessageIsFound`方法如下所示：

[!code-csharp[](razor-pages-tests/samples_snapshot/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

首先，方法會執行 [排列] 步驟，其中會進行 Act 步驟的準備。 植入訊息會取得並保留在中 `seedMessages` 。 植入訊息會儲存到資料庫中。 具有 `Id` 之的訊息 `1` 已設定為要刪除。 當 `DeleteMessageAsync` 執行方法時，預期的訊息應該會有所有訊息，但是除了具有的以外， `Id` `1` `expectedMessages`變數代表此預期的結果。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet1)]

方法的作用： `DeleteMessageAsync` 方法會以傳遞的來執行 `recId` `1` ：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet2)]

最後，方法 `Messages` 會從內容取得，並將它與判斷提示 `expectedMessages` 兩者相等的判斷提示：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet3)]

若要比較兩者 `List<Message>` 相同：

* 訊息會依排序 `Id` 。
* 訊息配對會在屬性上進行比較 `Text` 。

類似的測試方法， `DeleteMessageAsync_NoMessageIsDeleted_WhenMessageIsNotFound` 會檢查嘗試刪除不存在之訊息的結果。 在此情況下，資料庫中預期的訊息應該等於執行方法之後的實際訊息 `DeleteMessageAsync` 。 資料庫的內容應該不會有任何變更：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/DataAccessLayerTest.cs?name=snippet4)]

## <a name="unit-tests-of-the-page-model-methods"></a>頁面模型方法的單元測試

另一組單元測試負責測試頁面模型方法。 在訊息應用程式中，會在 `IndexModel` 類別中的*src/ Razor PagesTestSample/Pages/index. Cshtml*中找到索引頁面模型。

| 頁面模型方法 | 函式 |
| ----------------- | -------- |
| `OnGetAsync` | 使用方法，從 DAL 取得適用于 UI 的訊息 `GetMessagesAsync` 。 |
| `OnPostAddMessageAsync` | 如果[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)有效，會呼叫 `AddMessageAsync` 以將訊息新增至資料庫。 |
| `OnPostDeleteAllMessagesAsync` | 呼叫 `DeleteAllMessagesAsync` 以刪除資料庫中的所有訊息。 |
| `OnPostDeleteMessageAsync` | 執行 `DeleteMessageAsync` 以刪除具有指定之的訊息 `Id` 。 |
| `OnPostAnalyzeMessagesAsync` | 如果資料庫中有一或多個訊息，會計算每個訊息的平均單字數目。 |

頁面模型方法會使用 (測試/PagesTestSample 的類別中的七個測試進行測試 `IndexPageTests` * Razor 。測試/run-unittests/IndexPageTests*) 。 測試會使用熟悉的「排列-判斷提示-Act」模式。 這些測試著重于：

* 判斷當[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary)無效時，方法是否遵循正確的行為。
* 確認方法會產生正確的 <xref:Microsoft.AspNetCore.Mvc.IActionResult> 。
* 檢查是否已正確設定屬性值。

這組測試通常會模擬 DAL 的方法，為執行頁面模型方法的 Act 步驟產生預期的資料。 例如，的 `GetMessagesAsync` 方法 `AppDbContext` 會模擬以產生輸出。 當頁面模型方法執行這個方法時，mock 會傳回結果。 資料不是來自資料庫。 這會建立可預測且可靠的測試條件，以便在頁面模型測試中使用 DAL。

此 `OnGetAsync_PopulatesThePageModel_WithAListOfMessages` 測試會顯示如何 `GetMessagesAsync` 模擬頁面模型的方法：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet1&highlight=3-4)]

當 `OnGetAsync` 方法在 Act 步驟中執行時，它會呼叫頁面模型的 `GetMessagesAsync` 方法。

測試/PagesTestSample 的單元測試 Act 步驟 (test * Razor /Run-unittests/IndexPageTests*) ：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet2)]

`IndexPage`頁面模型的 `OnGetAsync` 方法 (*src/ Razor PagesTestSample/Pages/Index. cshtml. .cs*) ：

[!code-csharp[](razor-pages-tests/samples/2.x/src/RazorPagesTestSample/Pages/Index.cshtml.cs?name=snippet1&highlight=3)]

`GetMessagesAsync`DAL 中的方法不會傳回這個方法呼叫的結果。 模擬版本的方法會傳回結果。

在此 `Assert` 步驟中， () 的實際訊息 `actualMessages` 是從頁面模型的屬性指派而來 `Messages` 。 指派訊息時也會執行類型檢查。 預期和實際的訊息會依其屬性進行比較 `Text` 。 測試判斷提示兩個 `List<Message>` 實例包含相同的訊息。

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet3)]

此群組中的其他測試會建立頁面模型物件，其中包含 <xref:Microsoft.AspNetCore.Http.DefaultHttpContext> 、、 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary> 、， <xref:Microsoft.AspNetCore.Mvc.ActionContext> 以建立 `PageContext` 、 `ViewDataDictionary` 和 `PageContext` 。 這些適用于進行測試。 例如，訊息應用程式 `ModelState` 會與建立錯誤， <xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.AddModelError*> 以檢查 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 執行時傳回的有效 `OnPostAddMessageAsync` ：

[!code-csharp[](razor-pages-tests/samples/2.x/tests/RazorPagesTestSample.Tests/UnitTests/IndexPageTests.cs?name=snippet4&highlight=11,26,29,32)]

## <a name="additional-resources"></a>其他資源

* [使用 dotnet test 與 xUnit 為 .NET Core 中的 C# 進行單元測試](/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* <xref:mvc/controllers/testing>
* [對程式碼進行單元測試](/visualstudio/test/unit-test-your-code) (Visual Studio) 
* <xref:test/integration-tests>
* [xUnit.net](https://xunit.github.io/)
* [使用 Visual Studio for Mac 在 macOS 上建置完整的 .NET Core 方案](/dotnet/core/tutorials/using-on-mac-vs-full-solution)
* [開始使用 xUnit.net：搭配 .NET SDK 命令列使用 .NET Core](https://xunit.github.io/docs/getting-started-dotnet-core)
* [Moq](https://github.com/moq/moq4)
* [Moq 快速入門](https://github.com/Moq/moq4/wiki/Quickstart)
* [JustMockLite](https://github.com/telerik/JustMockLite)：適用于 .net 開發人員的模擬架構。  (*不是由 Microsoft 維護或支援*) 

::: moniker-end
