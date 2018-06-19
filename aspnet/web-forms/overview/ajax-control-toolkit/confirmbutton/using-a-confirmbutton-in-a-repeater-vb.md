---
uid: web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
title: 在中繼器 (VB) 中使用 ConfirmButton |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ConfirmButton extender 建立 Yes/任何快顯，當使用者按一下按鈕時 （包括 LinkButton 控制項）。 只有當是...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 18c31709-3f9d-4d93-8b01-f1356bf610b4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/confirmbutton/using-a-confirmbutton-in-a-repeater-vb
msc.type: authoredcontent
ms.openlocfilehash: 89f412c242a3a5bcd10b72b7f0cbfb23705edb51
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869448"
---
<a name="using-a-confirmbutton-in-a-repeater-vb"></a><span data-ttu-id="0dee0-104">在中繼器 (VB) 中使用 ConfirmButton</span><span class="sxs-lookup"><span data-stu-id="0dee0-104">Using a ConfirmButton In a Repeater (VB)</span></span>
====================
<span data-ttu-id="0dee0-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="0dee0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="0dee0-106">[下載程式碼](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="0dee0-106">[Download Code](http://download.microsoft.com/download/8/6/d/86dea6c6-bb92-4fa6-aa14-f8c0f82100f5/ConfirmButton1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/confirmbutton1VB.pdf)</span></span>

> <span data-ttu-id="0dee0-107">AJAX Control Toolkit ConfirmButton extender 建立 Yes/任何快顯，當使用者按一下按鈕時 （包括 LinkButton 控制項）。</span><span class="sxs-lookup"><span data-stu-id="0dee0-107">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="0dee0-108">只当按一下 [是]，按鈕會執行動作，否則為已取消。</span><span class="sxs-lookup"><span data-stu-id="0dee0-108">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="0dee0-109">這也是在中繼器中。</span><span class="sxs-lookup"><span data-stu-id="0dee0-109">This is also possible in a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="0dee0-110">總覽</span><span class="sxs-lookup"><span data-stu-id="0dee0-110">Overview</span></span>

<span data-ttu-id="0dee0-111">AJAX Control Toolkit ConfirmButton extender 建立 Yes/任何快顯，當使用者按一下按鈕時 （包括 LinkButton 控制項）。</span><span class="sxs-lookup"><span data-stu-id="0dee0-111">The ConfirmButton extender in the AJAX Control Toolkit creates a Yes/No popup when the user clicks on a button (including LinkButton control).</span></span> <span data-ttu-id="0dee0-112">只当按一下 [是]，按鈕會執行動作，否則為已取消。</span><span class="sxs-lookup"><span data-stu-id="0dee0-112">Only if Yes is clicked, the button's action is executed, otherwise cancelled.</span></span> <span data-ttu-id="0dee0-113">這也是在中繼器中。</span><span class="sxs-lookup"><span data-stu-id="0dee0-113">This is also possible in a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="0dee0-114">步驟</span><span class="sxs-lookup"><span data-stu-id="0dee0-114">Steps</span></span>

<span data-ttu-id="0dee0-115">首先，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="0dee0-115">First of all, a data source is required.</span></span> <span data-ttu-id="0dee0-116">這個範例會使用 AdventureWorks 資料庫和 Microsoft SQL Server 2005 Express Edition。</span><span class="sxs-lookup"><span data-stu-id="0dee0-116">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="0dee0-117">資料庫是選擇性的一部分 （包括 express edition） 的 Visual Studio 安裝，也會在個別下載[ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064)。</span><span class="sxs-lookup"><span data-stu-id="0dee0-117">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="0dee0-118">AdventureWorks 資料庫是 SQL Server 2005 範例和範例資料庫的一部分 (從下列網址下載[ https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e &amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en))。</span><span class="sxs-lookup"><span data-stu-id="0dee0-118">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="0dee0-119">若要設定資料庫的最簡單方式是使用 Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) 和附加`AdventureWorks.mdf`資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="0dee0-119">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="0dee0-120">此範例中，我們假設 SQL Server 2005 Express Edition 執行個體稱為`SQLEXPRESS`位在同一部電腦與網頁伺服器; 這也是預設的安裝程式。</span><span class="sxs-lookup"><span data-stu-id="0dee0-120">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="0dee0-121">如果您的設定不同，您必須調整資料庫的連接資訊。</span><span class="sxs-lookup"><span data-stu-id="0dee0-121">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="0dee0-122">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="0dee0-122">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample1.aspx)]

<span data-ttu-id="0dee0-123">然後，資料來源是必要的。</span><span class="sxs-lookup"><span data-stu-id="0dee0-123">Then, a data source is required.</span></span> <span data-ttu-id="0dee0-124">為了簡單起見，已擷取只有前五個項目 AdventureWorks 的供應商資料表中。</span><span class="sxs-lookup"><span data-stu-id="0dee0-124">For the sake of simplicity, only the first five entries in AdventureWorks' Vendors table are retrieved.</span></span> <span data-ttu-id="0dee0-125">請注意，當使用 Visual Studio 精靈來建立資料來源、 資料表名稱 (`Vendors`) 目前不正確加上`Purchasing`。</span><span class="sxs-lookup"><span data-stu-id="0dee0-125">Note that when using the Visual Studio wizard to create the data source, the table name (`Vendors`) is currently not correctly prefixed with `Purchasing`.</span></span> <span data-ttu-id="0dee0-126">下列標記會是正確的其中一個：</span><span class="sxs-lookup"><span data-stu-id="0dee0-126">The following markup is the correct one:</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample2.aspx)]

<span data-ttu-id="0dee0-127">此資料來源可用中繼器中。</span><span class="sxs-lookup"><span data-stu-id="0dee0-127">This data source can then be used within a repeater.</span></span> <span data-ttu-id="0dee0-128">像往常一樣，`DataBinder.Eval()`方法會從資料來源擷取資料。</span><span class="sxs-lookup"><span data-stu-id="0dee0-128">As usual, the `DataBinder.Eval()` method retrieves data from the data source.</span></span> <span data-ttu-id="0dee0-129">`ConfirmButtonExtender`控制項必須再放置內`<ItemTemplate>`中繼器使它顯示資料來源中的每個項目的一節。</span><span class="sxs-lookup"><span data-stu-id="0dee0-129">The `ConfirmButtonExtender` control must then be placed within the `<ItemTemplate>` section of the repeater so that it appears for every entry in the data source.</span></span>

[!code-aspx[Main](using-a-confirmbutton-in-a-repeater-vb/samples/sample3.aspx)]


<span data-ttu-id="0dee0-130">[![[確認] 按鈕旁邊的資料來源的每個項目](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0dee0-130">[![The confirm button appears next to each entry from the data source](using-a-confirmbutton-in-a-repeater-vb/_static/image2.png)](using-a-confirmbutton-in-a-repeater-vb/_static/image1.png)</span></span>

<span data-ttu-id="0dee0-131">資料來源的每個項目旁邊的 [確認] 按鈕會出現 ([按一下以檢視完整大小的影像](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="0dee0-131">The confirm button appears next to each entry from the data source ([Click to view full-size image](using-a-confirmbutton-in-a-repeater-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="0dee0-132">上一步</span><span class="sxs-lookup"><span data-stu-id="0dee0-132">Previous</span></span>](using-a-confirmbutton-in-a-repeater-cs.md)
