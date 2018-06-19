---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: 填滿使用 CascadingDropDown (VB) 的清單 |Microsoft 文件
author: wenz
description: AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯 anoth 中的值...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: e488a30443970d5e2ce825abd96d8e4a027585d1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873049"
---
<a name="filling-a-list-using-cascadingdropdown-vb"></a><span data-ttu-id="5a91b-103">填滿使用 CascadingDropDown (VB) 的清單</span><span class="sxs-lookup"><span data-stu-id="5a91b-103">Filling a List Using CascadingDropDown (VB)</span></span>
====================
<span data-ttu-id="5a91b-104">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5a91b-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5a91b-105">[下載程式碼](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5a91b-105">[Download Code](http://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)</span></span>

> <span data-ttu-id="5a91b-106">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="5a91b-106">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="5a91b-107">（比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。若要實際填入下拉式清單中使用此控制項是第一項挑戰，若要解決。</span><span class="sxs-lookup"><span data-stu-id="5a91b-107">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>


## <a name="overview"></a><span data-ttu-id="5a91b-108">總覽</span><span class="sxs-lookup"><span data-stu-id="5a91b-108">Overview</span></span>

<span data-ttu-id="5a91b-109">AJAX Control Toolkit CascadingDropDown 控制項擴充 DropDownList 控制項，使一個 DropDownList 載入中的變更相關聯的另一個 DropDownList 中的值。</span><span class="sxs-lookup"><span data-stu-id="5a91b-109">The CascadingDropDown control in the AJAX Control Toolkit extends a DropDownList control so that changes in one DropDownList loads associated values in another DropDownList.</span></span> <span data-ttu-id="5a91b-110">（比方說，一份清單會提供一份我們狀態，而且下一個清單然後填入處於該狀態中的主要城市）。若要實際填入下拉式清單中使用此控制項是第一項挑戰，若要解決。</span><span class="sxs-lookup"><span data-stu-id="5a91b-110">(For instance, one list provides a list of US states, and the next list is then filled with major cities in that state.) The first challenge to solve is to actually fill a dropdown list using this control.</span></span>

## <a name="steps"></a><span data-ttu-id="5a91b-111">步驟</span><span class="sxs-lookup"><span data-stu-id="5a91b-111">Steps</span></span>

<span data-ttu-id="5a91b-112">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="5a91b-112">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

<span data-ttu-id="5a91b-113">DropDownList 控制項則需要：</span><span class="sxs-lookup"><span data-stu-id="5a91b-113">Then, a DropDownList control is required:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

<span data-ttu-id="5a91b-114">對於此清單中，就會加入 CascadingDropDown 擴充項。</span><span class="sxs-lookup"><span data-stu-id="5a91b-114">For this list, a CascadingDropDown extender is added.</span></span> <span data-ttu-id="5a91b-115">非同步要求會傳送到 web 服務接著會傳回一份清單中顯示的項目。</span><span class="sxs-lookup"><span data-stu-id="5a91b-115">It will send an asynchronous request to a web service which will then return a list of entries to be displayed in the list.</span></span> <span data-ttu-id="5a91b-116">針對此目的，需要設定下列 CascadingDropDown 屬性：</span><span class="sxs-lookup"><span data-stu-id="5a91b-116">For this to work, the following CascadingDropDown attributes need to be set:</span></span>

- <span data-ttu-id="5a91b-117">`ServicePath`： 提供的清單項目 web 服務 URL</span><span class="sxs-lookup"><span data-stu-id="5a91b-117">`ServicePath`: URL of a web service delivering the list entries</span></span>
- <span data-ttu-id="5a91b-118">`ServiceMethod`: Web 方法傳遞之清單項目</span><span class="sxs-lookup"><span data-stu-id="5a91b-118">`ServiceMethod`: Web method delivering the list entries</span></span>
- <span data-ttu-id="5a91b-119">`TargetControlID`: ID 的下拉式清單</span><span class="sxs-lookup"><span data-stu-id="5a91b-119">`TargetControlID`: ID of the dropdown list</span></span>
- <span data-ttu-id="5a91b-120">`Category`： 提交給 web 方法的呼叫時類別目錄資訊</span><span class="sxs-lookup"><span data-stu-id="5a91b-120">`Category`: Category information that is submitted to the web method when called</span></span>
- <span data-ttu-id="5a91b-121">`PromptText`： 當以非同步方式從伺服器載入清單資料時顯示的文字</span><span class="sxs-lookup"><span data-stu-id="5a91b-121">`PromptText`: Text displayed when asynchronously loading list data from the server</span></span>

<span data-ttu-id="5a91b-122">以下是標記`CascadingDropDown`項目。</span><span class="sxs-lookup"><span data-stu-id="5a91b-122">Here is the markup for the `CascadingDropDown` element.</span></span> <span data-ttu-id="5a91b-123">C# 和 VB 的唯一差異是相關聯的 web 服務的名稱：</span><span class="sxs-lookup"><span data-stu-id="5a91b-123">The only difference between C# and VB is the name of the associated web service:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

<span data-ttu-id="5a91b-124">JavaScript 程式碼來自`CascadingDropDown`extender 呼叫 web 服務方法具有下列簽章：</span><span class="sxs-lookup"><span data-stu-id="5a91b-124">The JavaScript code coming from the `CascadingDropDown` extender calls a web service method with the following signature:</span></span>

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

<span data-ttu-id="5a91b-125">因此重要層面是方法必須傳回型別的陣列`CascadingDropDownNameValue`（由 ASP.NET AJAX Control Toolkit 定義）。</span><span class="sxs-lookup"><span data-stu-id="5a91b-125">So the important aspect is that the method needs to return an array of type `CascadingDropDownNameValue` (defined by the ASP.NET AJAX Control Toolkit).</span></span> <span data-ttu-id="5a91b-126">中`CascadingDropDownNameValue`建構函式，第一個清單項目的文字，然後其值必須提供，就如同`<option value="VALUE">NAME</option>`以 HTML 做的一樣。</span><span class="sxs-lookup"><span data-stu-id="5a91b-126">In the `CascadingDropDownNameValue` contructor, first the list entry's text and then its value must be provided, just as `<option value="VALUE">NAME</option>` would do in HTML.</span></span> <span data-ttu-id="5a91b-127">以下是一些範例資料：</span><span class="sxs-lookup"><span data-stu-id="5a91b-127">Here is some sample data:</span></span>

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

<span data-ttu-id="5a91b-128">載入網頁瀏覽器中的，將會觸發三位廠商填入清單。</span><span class="sxs-lookup"><span data-stu-id="5a91b-128">Loading the page in the browser will trigger the list to be filled with three vendors.</span></span>


<span data-ttu-id="5a91b-129">[![清單會自動填入](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5a91b-129">[![The list is filled automatically](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)</span></span>

<span data-ttu-id="5a91b-130">清單會自動填入 ([按一下以檢視完整大小的影像](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5a91b-130">The list is filled automatically ([Click to view full-size image](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5a91b-131">[上一頁](using-auto-postback-with-cascadingdropdown-cs.md)
> [下一頁](using-cascadingdropdown-with-a-database-vb.md)</span><span class="sxs-lookup"><span data-stu-id="5a91b-131">[Previous](using-auto-postback-with-cascadingdropdown-cs.md)
[Next](using-cascadingdropdown-with-a-database-vb.md)</span></span>
