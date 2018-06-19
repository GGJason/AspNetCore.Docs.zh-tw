---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
title: 資料庫 (C#) 搭配使用 CascadingDropDown |Microsoft 文件
author: wenz
description: AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 684f0c28-a490-4e5b-b5e5-5dfb77464b49
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 1991c26d408e593999288ea6df0467cea0369457
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879123"
---
<a name="using-cascadingdropdown-with-a-database-c"></a><span data-ttu-id="11ab4-103">CascadingDropDown 資料庫使用的 (C#)</span><span class="sxs-lookup"><span data-stu-id="11ab4-103">Using CascadingDropDown with a Database (C#)</span></span>
====================
<span data-ttu-id="11ab4-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="11ab4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="11ab4-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="11ab4-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1CS.pdf)</span></span>

> <span data-ttu-id="11ab4-106">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="11ab4-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="11ab4-107">為了讓此工作，您必須建立特殊的 web 服務。</span><span class="sxs-lookup"><span data-stu-id="11ab4-107">In order for this to work, a special web service must be created.</span></span>


## <a name="overview"></a><span data-ttu-id="11ab4-108">總覽</span><span class="sxs-lookup"><span data-stu-id="11ab4-108">Overview</span></span>

<span data-ttu-id="11ab4-109">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="11ab4-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="11ab4-110">（比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。為了讓此工作，您必須建立特殊的 web 服務。</span><span class="sxs-lookup"><span data-stu-id="11ab4-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) In order for this to work, a special web service must be created.</span></span>

## <a name="steps"></a><span data-ttu-id="11ab4-111">步驟</span><span class="sxs-lookup"><span data-stu-id="11ab4-111">Steps</span></span>

<span data-ttu-id="11ab4-112">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="11ab4-112">First of all, a data source is required.</span></span> <span data-ttu-id="11ab4-113">這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="11ab4-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="11ab4-114">資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="11ab4-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="11ab4-115">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="11ab4-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="11ab4-116">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="11ab4-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="11ab4-117">此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="11ab4-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="11ab4-118">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="11ab4-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="11ab4-119">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部&lt; `form` &gt;項目):</span><span class="sxs-lookup"><span data-stu-id="11ab4-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the &lt;`form`&gt; element):</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample1.aspx)]

<span data-ttu-id="11ab4-120">在下一個步驟中，兩個 DropDownList 控制項都必須的。</span><span class="sxs-lookup"><span data-stu-id="11ab4-120">In the next step, two DropDownList controls are required.</span></span> <span data-ttu-id="11ab4-121">在此範例中，我們使用 AdventureWorks 的廠商和連絡人資訊，因此我們建立一個清單可用的供應商，另一個可用的連絡人：</span><span class="sxs-lookup"><span data-stu-id="11ab4-121">In this sample, we use the vendor and contact information from AdventureWorks, thus we create one list for the available vendors and one for the available contacts:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample2.aspx)]

<span data-ttu-id="11ab4-122">然後，兩個 CascadingDropDown extender 必須加入至頁面。</span><span class="sxs-lookup"><span data-stu-id="11ab4-122">Then, two CascadingDropDown extenders must be added to the page.</span></span> <span data-ttu-id="11ab4-123">其中一個填滿第一個 （廠商） 清單中，和另一個填滿第二個 （連絡人） 清單。</span><span class="sxs-lookup"><span data-stu-id="11ab4-123">One fills the first (vendors) list, and the other one fills the second (contacts) list.</span></span> <span data-ttu-id="11ab4-124">必須設定下列屬性：</span><span class="sxs-lookup"><span data-stu-id="11ab4-124">The following attributes must be set:</span></span>

- <span data-ttu-id="11ab4-125">`ServicePath`： 提供的清單項目 web 服務 URL</span><span class="sxs-lookup"><span data-stu-id="11ab4-125">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="11ab4-126">`ServiceMethod`: Web 方法傳遞之清單項目</span><span class="sxs-lookup"><span data-stu-id="11ab4-126">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="11ab4-127">`TargetControlID`: ID 的下拉式清單</span><span class="sxs-lookup"><span data-stu-id="11ab4-127">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="11ab4-128">`Category`： 提交給 web 方法的呼叫時類別目錄資訊</span><span class="sxs-lookup"><span data-stu-id="11ab4-128">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="11ab4-129">`PromptText`： 當以非同步方式從伺服器載入清單資料時顯示的文字</span><span class="sxs-lookup"><span data-stu-id="11ab4-129">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>
- <span data-ttu-id="11ab4-130">`ParentControlID`: (選擇性) 父下拉式清單目前清單的觸發程序載入</span><span class="sxs-lookup"><span data-stu-id="11ab4-130">`ParentControlID`: (optional) parent dropdown list that triggers loading of the current list</span></span>

<span data-ttu-id="11ab4-131">根據使用的程式設計語言，web 服務有問題的名稱變更，但是所有其他屬性值都相同。</span><span class="sxs-lookup"><span data-stu-id="11ab4-131">Depending on the programming language used, the name of the web service in question changes, but all other attribute values are the same.</span></span> <span data-ttu-id="11ab4-132">以下是第一個下拉式清單中的 CascadingDropDown 項目：</span><span class="sxs-lookup"><span data-stu-id="11ab4-132">Here is the CascadingDropDown element for the first dropdown list:</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample3.aspx)]

<span data-ttu-id="11ab4-133">第二個清單控制項擴充項需要設定`ParentControlID`，以便在載入的供應商清單觸發程序中選取一個項目相關聯的連絡人清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="11ab4-133">The control extenders for the second list need to set the `ParentControlID` attribute so that selecting an entry in the vendors list triggers loading associated elements in the contacts list.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample4.aspx)]

<span data-ttu-id="11ab4-134">實際工作，然後是在 web 服務設定，如下所示。</span><span class="sxs-lookup"><span data-stu-id="11ab4-134">The actual work is then done in the web service, which is set up as follows.</span></span> <span data-ttu-id="11ab4-135">請注意，`[ScriptService]`屬性時，ASP.NET AJAX 否則無法建立要從用戶端指令碼的程式碼存取的 web 方法的 JavaScript proxy。</span><span class="sxs-lookup"><span data-stu-id="11ab4-135">Note that the `[ScriptService]` attribute is used, otherwise ASP.NET AJAX cannot create the JavaScript proxy to access the web methods from client-side script code.</span></span>

[!code-aspx[Main](using-cascadingdropdown-with-a-database-cs/samples/sample5.aspx)]

<span data-ttu-id="11ab4-136">呼叫 CascadingDropDown 的 web 方法的簽章如下所示：</span><span class="sxs-lookup"><span data-stu-id="11ab4-136">The signature of the web methods called by CascadingDropDown is as follows:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample6.cs)]

<span data-ttu-id="11ab4-137">因此傳回的值必須是類型的陣列`CascadingDropDownNameValue`定義控制項的工具組。</span><span class="sxs-lookup"><span data-stu-id="11ab4-137">So the return value must be an array of type `CascadingDropDownNameValue` which is defined by the Control Toolkit.</span></span> <span data-ttu-id="11ab4-138">`GetVendors()`方法是實作相當簡單： 程式碼會連接到 AdventureWorks 資料庫和查詢的前 25 個廠商。</span><span class="sxs-lookup"><span data-stu-id="11ab4-138">The `GetVendors()` method is quite easy to implement: The code connects to the AdventureWorks database and queries the first 25 vendors.</span></span> <span data-ttu-id="11ab4-139">中的第一個參數`CascadingDropDownNameValue`建構函式是標題的清單項目，第二個它的值 (以 HTML 的 value 屬性&lt; `option` &gt;項目)。</span><span class="sxs-lookup"><span data-stu-id="11ab4-139">The first parameter in the `CascadingDropDownNameValue` constructor is the caption of the list entry, the second one its value (value attribute in HTML's &lt;`option`&gt; element).</span></span> <span data-ttu-id="11ab4-140">下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="11ab4-140">Here is the code:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample7.cs)]

<span data-ttu-id="11ab4-141">供應商取得相關的連絡人 (方法名稱： `GetContactsForVendor()`) 棘手。</span><span class="sxs-lookup"><span data-stu-id="11ab4-141">Getting the associated contacts for a vendor (method name: `GetContactsForVendor()`) is a bit trickier.</span></span> <span data-ttu-id="11ab4-142">首先，您必須決定廠商的第一個下拉式清單中已選取。</span><span class="sxs-lookup"><span data-stu-id="11ab4-142">First of all, the vendor which has been selected in the first dropdown list must be determined.</span></span> <span data-ttu-id="11ab4-143">Control Toolkit 定義該工作的 helper 方法：`ParseKnownCategoryValuesString()`方法會傳回`StringDictionary`下拉式清單中資料的項目：</span><span class="sxs-lookup"><span data-stu-id="11ab4-143">The Control Toolkit defines a helper method for that task: The `ParseKnownCategoryValuesString()` method returns a `StringDictionary` element with the dropdown data:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample8.cs)]

<span data-ttu-id="11ab4-144">基於安全性理由，必須先驗證這項資料。</span><span class="sxs-lookup"><span data-stu-id="11ab4-144">For security reasons, this data must be validated first.</span></span> <span data-ttu-id="11ab4-145">因此，如果廠商項目 (因為`Category`CascadingDropDown，第一個元素的屬性設定為`"Vendor"`)，可擷取所選的廠商識別碼：</span><span class="sxs-lookup"><span data-stu-id="11ab4-145">So if there is a Vendor entry (because the `Category` property of the first CascadingDropDown element is set to `"Vendor"`), the ID of the selected vendor may be retrieved:</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample9.cs)]

<span data-ttu-id="11ab4-146">方法的其餘部分是相當簡單，則。</span><span class="sxs-lookup"><span data-stu-id="11ab4-146">The rest of the method is fairly straight-forward, then.</span></span> <span data-ttu-id="11ab4-147">廠商識別碼用於做為參數的 SQL 查詢，該廠商會擷取所有相關聯的連絡人。</span><span class="sxs-lookup"><span data-stu-id="11ab4-147">The vendor's ID is used as a parameter for an SQL query that retrieves all associated contacts for that vendor.</span></span> <span data-ttu-id="11ab4-148">同樣地，方法會傳回型別的陣列`CascadingDropDownNameValue`。</span><span class="sxs-lookup"><span data-stu-id="11ab4-148">Once again, the method returns an array of type `CascadingDropDownNameValue`.</span></span>

[!code-csharp[Main](using-cascadingdropdown-with-a-database-cs/samples/sample10.cs)]

<span data-ttu-id="11ab4-149">載入 ASP.NET 頁面中，並在短時間之後廠商清單填滿 25 的項目。</span><span class="sxs-lookup"><span data-stu-id="11ab4-149">Load the ASP.NET page, and after a short while the vendor list is filled with 25 entries.</span></span> <span data-ttu-id="11ab4-150">選取一個項目，並注意第二個下拉式清單填入資料的方式。</span><span class="sxs-lookup"><span data-stu-id="11ab4-150">Pick one entry and notice how the second dropdown list is filled with data.</span></span>


<span data-ttu-id="11ab4-151">[![自動填滿第一個清單中](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="11ab4-151">[![The first list is filled automatically](using-cascadingdropdown-with-a-database-cs/_static/image2.png)](using-cascadingdropdown-with-a-database-cs/_static/image1.png)</span></span>

<span data-ttu-id="11ab4-152">自動填滿第一個清單中 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="11ab4-152">The first list is filled automatically ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image3.png))</span></span>


<span data-ttu-id="11ab4-153">[![第二個清單是根據第一個清單中的選取範圍填滿](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="11ab4-153">[![The second list is filled according to the selection in the first list](using-cascadingdropdown-with-a-database-cs/_static/image5.png)](using-cascadingdropdown-with-a-database-cs/_static/image4.png)</span></span>

<span data-ttu-id="11ab4-154">第二個清單根據第一個清單中的選取範圍填滿 ([按一下以檢視完整大小的影像](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="11ab4-154">The second list is filled according to the selection in the first list ([Click to view full-size image](using-cascadingdropdown-with-a-database-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="11ab4-155">[上一頁](filling-a-list-using-cascadingdropdown-cs.md)
> [下一頁](presetting-list-entries-with-cascadingdropdown-cs.md)</span><span class="sxs-lookup"><span data-stu-id="11ab4-155">[Previous](filling-a-list-using-cascadingdropdown-cs.md)
[Next](presetting-list-entries-with-cascadingdropdown-cs.md)</span></span>
