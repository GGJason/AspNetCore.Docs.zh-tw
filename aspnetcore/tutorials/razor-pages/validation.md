---
title: 第8部分，新增驗證
author: rick-anderson
description: 頁面上的第8部分教學課程系列 Razor 。
ms.author: riande
ms.custom: mvc
ms.date: 09/29/2020
no-loc:
- Index
- appsettings.json
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
uid: tutorials/razor-pages/validation
ms.openlocfilehash: f155922c9cb5ea7fdbad0963221ceddd19f4fe60
ms.sourcegitcommit: db0a6eb0be7bd7f22810a71fe9bf30e957fd116a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2020
ms.locfileid: "96419950"
---
# <a name="part-8-of-tutorial-series-on-no-locrazor-pages"></a>頁面上的第8部分教學課程系列 Razor 。

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

在本節中將驗證邏輯新增至 `Movie` 模型。 使用者建立或編輯電影時，就會強制執行驗證規則。

## <a name="validation"></a>驗證

軟體開發的核心原則稱為 [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself)("**D** on't **R** epeat **Y** ourself", 不重複原則)。 Razor 頁面會鼓勵開發環境指定一次，而且會反映在整個應用程式中。 DRY 有助於：

* 降低應用程式中的程式碼數量。
* 使程式碼較少出現錯誤，而且更容易進行測試和維護。

頁面和 Entity Framework 所提供的驗證支援 Razor 是理想的策略範例：

* 驗證規則會以宣告方式指定于模型類別中的一個位置。
* 規則會在應用程式的任何位置強制執行。

## <a name="add-validation-rules-to-the-movie-model"></a>將驗證規則新增至電影模型

`DataAnnotations`命名空間提供：

* 一組內建的驗證屬性，會以宣告方式套用至類別或屬性。
* 格式化屬性 `[DataType]` ，例如格式化的說明，而且不提供任何驗證。

更新 `Movie` 類別，以充分利用內建的 `[Required]`、`[StringLength]`、`[RegularExpression]` 和 `[Range]` 驗證屬性。

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

驗證屬性會指定在其套用的模型屬性上強制執行的行為：

* `[Required]` 和 `[MinimumLength]` 屬性 (attribute) 指出屬性 (property) 必須具有值。 無需防止使用者輸入空白字元來滿足這種驗證。
* `[RegularExpression]` 屬性則用來限制可輸入的字元。 在上述程式碼中， `Genre` ：

  * 必須指使用字母。
  * 第一個字母必須是大寫。 不允許使用空格、數字和特殊字元。

* `RegularExpression` `Rating` ：

  * 第一個字元必須為大寫字母。
  * 允許後續空格中的特殊字元和數位。 "PG-13" 適用于評等，但失敗 `Genre` 。

* `[Range]` 屬性會將值限制在指定的範圍內。
* `[StringLength]`屬性可以設定字串屬性的最大長度，並選擇性地設定其最小長度。
* 實數值型別（例如，、、、） `decimal` `int` 本質上 `float` `DateTime` 是必要的，而且不需要 `[Required]` 屬性。

上述驗證規則是用於示範，不是生產系統的最佳做法。 例如，上述程式可避免輸入只有兩個字元的電影，而不允許在中使用特殊字元 `Genre` 。

具有 ASP.NET Core 自動強制執行的驗證規則有助於：

* 有助於讓應用程式更穩固。
* 減少將無效資料儲存到資料庫的機會。

### <a name="validation-error-ui-in-no-locrazor-pages"></a>頁面中的驗證錯誤 UI Razor

執行應用程式，並巡覽至 Pages/Movies。

選取 **Create New** 連結。 使用某些無效值完成表單。 當 jQuery 用戶端驗證偵測到錯誤時，它會顯示錯誤訊息。

![有多個 jQuery 用戶端驗證錯誤的電影檢視表單](validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

請注意表單在包含無效值的每個欄位中自動呈現驗證錯誤訊息的方式。 當使用者已停用 JavaScript 時，用戶端、使用 JavaScript 和 jQuery 以及伺服器端都會強制執行這些錯誤。

最大的好處是在 [建立] 或 [編輯] 頁面中不需要變更 **任何** 程式碼。 一旦將資料批註套用至模型之後，就會啟用驗證 UI。 Razor在本教學課程中建立的頁面，會在模型類別的屬性上使用驗證屬性，自動挑選驗證規則 `Movie` 。 使用 Edit 頁面測試驗證，會套用相同的驗證。

要一直到沒有任何用戶端驗證錯誤之後，才會將表單資料發佈到伺服器。 請確認表單資料不會經由下列一或多種方式發佈：

* 將中斷點放置在 `OnPostAsync` 方法中。 選取 [ **建立** ] 或 [ **儲存**] 來提交表單。 永遠不會叫用中斷點。
* 使用 [Fiddler 工具](https://www.telerik.com/fiddler)。
* 使用瀏覽器開發人員工具來監視網路流量。

### <a name="server-side-validation"></a>伺服器端驗證

在瀏覽器中停用 JavaScript 時，提交含有錯誤的表單將發佈到伺服器。

選擇性地測試伺服器端驗證：

1. 在瀏覽器中停用 JavaScript。 您可以使用瀏覽器的開發人員工具來停用 JavaScript。 如果無法在瀏覽器中停用 JavaScript，請嘗試另一個瀏覽器。
1. 在 Create 或 Edit 頁面的 `OnPostAsync` 方法中設定中斷點。
1. 提交含有無效資料的表單。
1. 確認模型狀態無效：

   ```csharp
    if (!ModelState.IsValid)
    {
       return Page();
    }
   ```
  
或者， [在伺服器上停用用戶端驗證](xref:mvc/models/validation#disable-client-side-validation)。

下列程式碼顯示稍早在此教學課程中包含 Scaffold 的部分 *Create.cshtml* 頁面。 Create 和 Edit 頁面會使用它來：

* 顯示初始表單。
* 發生錯誤時重新顯示表單。

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

[輸入標記協助程式](xref:mvc/views/working-with-forms)會使用 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。 [驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers)會顯示驗證錯誤。 如需詳細資訊，請參閱[驗證](xref:mvc/models/validation)。

Create 和 Edit 頁面中沒有任何驗證規則。 只有在 `Movie` 類別中才能指定驗證規則和錯誤字串。 這些驗證規則會自動套用至 Razor 編輯模型的頁面 `Movie` 。

當驗證邏輯需要變更時，它只會在模型中進行。 驗證會一致地套用到整個應用程式中，而驗證邏輯則定義于一個位置。 位於一個位置的驗證有助於讓程式碼保持整潔，並可讓您更容易進行維護和更新。

## <a name="use-datatype-attributes"></a>使用 DataType 屬性

檢查 `Movie` 類別。 除了一組內建的驗證屬性之外，`System.ComponentModel.DataAnnotations` 命名空間還提供了格式屬性。 `[DataType]` 屬性會套用到 `ReleaseDate` 和 `Price` 屬性。

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

`[DataType]`屬性提供：

* 查看引擎格式化資料的提示。
* 提供屬性，例如 `<a>` URL 和 `<a href="mailto:EmailAddress.com">` 電子郵件。

使用 `[RegularExpression]` 屬性來驗證資料的格式。 `[DataType]` 屬性可用於指定比資料庫內建類型更特定的資料類型。 `[DataType]` 屬性不是驗證屬性。 在範例應用程式中，只會顯示日期，而不含時間。

`DataType`列舉提供許多資料類型，例如、、 `Date` `Time` `PhoneNumber` 、、等等 `Currency` `EmailAddress` 。 

`[DataType]`屬性：

* 可以讓應用程式自動提供型別特有的功能。 例如，可針對 `DataType.EmailAddress` 建立 `mailto:` 連結。
* 可以 `DataType.Date` 在支援 HTML5 的瀏覽器中提供日期選擇器。
* 發出 HTML 5 `data-` ，發音為「資料虛線」，這是 html 5 瀏覽器使用的屬性。
* 請勿 **提供任何** 驗證。

`DataType.Date` 未指定顯示日期的格式。 根據預設，將依據以伺服器 `CultureInfo` 為基礎的預設格式顯示資料欄位。

`[Column(TypeName = "decimal(18, 2)")]` 資料註解為必要項，因此 Entity Framework Core 可將 `Price` 正確對應到資料庫中的貨幣。 如需詳細資訊，請參閱[資料類型](/ef/core/modeling/relational/data-types)。

`[DisplayFormat]` 屬性用來明確指定日期格式：

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

`ApplyFormatInEditMode`設定會指定在顯示值以供編輯時，將套用格式設定。 某些欄位可能不需要該行為。 例如，在貨幣值中，在編輯 UI 中通常不需要貨幣符號。

`[DisplayFormat]` 屬性可以單獨使用，但通常最好是使用 `[DataType]` 屬性。 `[DataType]` 屬性會將資料的語意以其在螢幕上呈現方式的相反方式傳遞。 `[DataType]`屬性提供下列無法使用的優點 `[DisplayFormat]` ：

* 瀏覽器可以啟用 HTML5 功能，例如顯示行事曆控制項、地區設定適當的貨幣符號、電子郵件連結等等。
* 根據預設，瀏覽器會根據其地區設定，使用正確的格式來呈現資料。
* `[DataType]` 屬性可以啟用 ASP.NET Core 架構，來選擇用於呈現資料的正確欄位範本。 `DisplayFormat`如果本身使用，則會使用字串範本。

**注意：** jQuery 驗證不適用於 `[Range]` 屬性和 `DateTime` 。 例如，下列程式碼一律會顯示用戶端驗證錯誤，即使當日期位在指定範圍中也一樣：

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

最佳做法是避免在模型中編譯硬性日期，因此 `[Range]` `DateTime` 不建議使用屬性。 使用日期範圍的設定以及其他受頻繁 [變更的值](xref:fundamentals/configuration/index) ，而不是在程式碼中指定。

下列程式碼會顯示一行上的結合屬性：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDAmult.cs?name=snippet1)]

[入門 Razor 頁面和 EF Core](xref:data/ef-rp/intro) 會顯示頁面的 advanced EF Core 作業 Razor 。

### <a name="apply-migrations"></a>套用移轉

套用至類別的 DataAnnotations 會變更架構。 例如，套用至 `Title` 欄位的 DataAnnotations：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet11)]

* 將字元限制為 60 個。
* 不允許 `null` 值。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

`Movie` 資料表目前具有下列結構描述：

```sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (MAX)  NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (MAX)  NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (MAX)  NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

上述結構描述變更不會造成 EF 擲回例外狀況。 不過，請建立移轉，讓結構描述與模型一致。

 從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。
在 PMC 中，輸入下列命令：

```powershell
Add-Migration New_DataAnnotations
Update-Database
```

`Update-Database` 會執行 `New_DataAnnotations` 類別的 `Up` 方法。 檢查 `Up` 方法：

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Migrations/20190724163003_New_DataAnnotations.cs?name=snippet)]

已更新的 `Movie` 資料表具有下列結構描述：

```sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (60)   NOT NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (30)   NOT NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (5)    NOT NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

# <a name="visual-studio-code--visual-studio-for-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

SQLite 不需要移轉。

---

### <a name="publish-to-azure"></a>發佈至 Azure

如需部署至 Azure 的詳細資訊，請參閱 [教學課程：使用 SQL Database 在 azure 中建立 ASP.NET Core 應用程式](/azure/app-service/tutorial-dotnetcore-sqldb-app)。

感謝您完成本頁面簡介 Razor 。 [入門 Razor 頁面和 EF Core](xref:data/ef-rp/intro) 是本教學課程的最佳追蹤。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>

> [!div class="step-by-step"]
> [上一步：新增欄位](xref:tutorials/razor-pages/new-field)
