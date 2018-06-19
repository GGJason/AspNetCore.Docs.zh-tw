---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: 定位 ModalPopup (C#) |Microsoft 文件
author: wenz
description: AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。 但是控制項不提供...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: bee5be84259231d8cd5efde74b610d72f5e250cc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874144"
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="f3c40-104">定位 ModalPopup (C#)</span><span class="sxs-lookup"><span data-stu-id="f3c40-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="f3c40-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f3c40-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f3c40-106">[下載程式碼](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip)或[下載 PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f3c40-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="f3c40-107">AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。</span><span class="sxs-lookup"><span data-stu-id="f3c40-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f3c40-108">但是控制項不提供內建的功能，將快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="f3c40-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="f3c40-109">總覽</span><span class="sxs-lookup"><span data-stu-id="f3c40-109">Overview</span></span>

<span data-ttu-id="f3c40-110">AJAX Control Toolkit ModalPopup 控制項提供簡單的方式來建立使用用戶端表示強制回應的快顯。</span><span class="sxs-lookup"><span data-stu-id="f3c40-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="f3c40-111">但是控制項不提供內建的功能，將快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="f3c40-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="f3c40-112">步驟</span><span class="sxs-lookup"><span data-stu-id="f3c40-112">Steps</span></span>

<span data-ttu-id="f3c40-113">為了啟用 ASP.NET AJAX 和控制工具組的功能`ScriptManager`。</span><span class="sxs-lookup"><span data-stu-id="f3c40-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="f3c40-114">控制項必須任意位置放置在頁面 (但內部`<form>`項目):</span><span class="sxs-lookup"><span data-stu-id="f3c40-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="f3c40-115">接著，將會做為強制回應的快顯視窗的面板。</span><span class="sxs-lookup"><span data-stu-id="f3c40-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="f3c40-116">按鈕來關閉快顯視窗：</span><span class="sxs-lookup"><span data-stu-id="f3c40-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="f3c40-117">每當在快顯視窗會顯示，它應該位於網頁中的特定位置。</span><span class="sxs-lookup"><span data-stu-id="f3c40-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="f3c40-118">這項工作，會建立用戶端 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="f3c40-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="f3c40-119">會先嘗試存取面板。</span><span class="sxs-lookup"><span data-stu-id="f3c40-119">It first tries to access the panel.</span></span> <span data-ttu-id="f3c40-120">如果成功，面板的位置會設定使用 CSS 和 JavaScript （在快顯視窗的位置將會的變更）。</span><span class="sxs-lookup"><span data-stu-id="f3c40-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="f3c40-121">不過，`ModalPopupExtender`控制項也會嘗試將快顯視窗。</span><span class="sxs-lookup"><span data-stu-id="f3c40-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="f3c40-122">因此，JavaScript 程式碼重複上面的快顯視窗中，而每個十分之一秒。</span><span class="sxs-lookup"><span data-stu-id="f3c40-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="f3c40-123">您可以看到，傳回值`setTimeout()`JavaScript 方法會儲存在全域變數。</span><span class="sxs-lookup"><span data-stu-id="f3c40-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="f3c40-124">這可停止重複的位置快顯視窗，視使用`clearTimeout()`方法：</span><span class="sxs-lookup"><span data-stu-id="f3c40-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="f3c40-125">現在所有就是使呼叫這些函式，只要有適當的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f3c40-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="f3c40-126">`movePanel()`觸發程序 [面板] 中，按一下此按鈕時，就必須呼叫 JavaScript 函式：</span><span class="sxs-lookup"><span data-stu-id="f3c40-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="f3c40-127">和`stopMoving()`函式來自於快顯視窗已關閉，可能會觸發`ModalPopupExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="f3c40-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="f3c40-128">[![強制回應的快顯視窗會出現在指定的位置](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f3c40-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="f3c40-129">強制回應的快顯視窗會出現在指定的位置 ([按一下以檢視完整大小的影像](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f3c40-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f3c40-130">[上一頁](handling-postbacks-from-a-modalpopup-cs.md)
> [下一頁](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="f3c40-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
