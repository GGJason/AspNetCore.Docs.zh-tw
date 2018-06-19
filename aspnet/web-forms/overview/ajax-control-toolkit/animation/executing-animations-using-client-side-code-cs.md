---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
title: 執行動畫使用用戶端程式碼 (C#) |Microsoft 文件
author: wenz
description: 動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。 動畫執行...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 0270e0df-6fde-4a8f-a2cb-2cacc55143f2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: cfaed745b4c547d04033ee89d2c6549c5989cb73
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870735"
---
<a name="executing-animations-using-client-side-code-c"></a><span data-ttu-id="e4605-104">執行動畫使用用戶端程式碼 (C#)</span><span class="sxs-lookup"><span data-stu-id="e4605-104">Executing Animations Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="e4605-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e4605-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e4605-106">[下載程式碼](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip)或[下載 PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e4605-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10CS.pdf)</span></span>

> <span data-ttu-id="e4605-107">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="e4605-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e4605-108">動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。</span><span class="sxs-lookup"><span data-stu-id="e4605-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="e4605-109">總覽</span><span class="sxs-lookup"><span data-stu-id="e4605-109">Overview</span></span>

<span data-ttu-id="e4605-110">動畫控制項在 ASP.NET AJAX Control Toolkit 不是只控制項，但若要將動畫加入至控制項的整個架構。</span><span class="sxs-lookup"><span data-stu-id="e4605-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e4605-111">動畫執行也可能使用自訂用戶端 JavaScript 程式碼會觸發。</span><span class="sxs-lookup"><span data-stu-id="e4605-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="e4605-112">步驟</span><span class="sxs-lookup"><span data-stu-id="e4605-112">Steps</span></span>

<span data-ttu-id="e4605-113">首先，包括`ScriptManager`在頁面; 然後，ASP.NET AJAX 程式庫載入，因此您能夠使用控制項的工具組：</span><span class="sxs-lookup"><span data-stu-id="e4605-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="e4605-114">動畫會套用到面板中的文字看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="e4605-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="e4605-115">在 [面板] 中相關聯的 CSS 類別，定義好的背景色彩，也將面板固定的寬度設定：</span><span class="sxs-lookup"><span data-stu-id="e4605-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="e4605-116">接著，新增`AnimationExtender`到頁面上，提供`ID`、`TargetControlID`屬性和必要`runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="e4605-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="e4605-117">內`<Animations>`節點，請使用`<OnClick>`執行動畫一次使用者按一下面板上。</span><span class="sxs-lookup"><span data-stu-id="e4605-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="e4605-118">加入兩個動畫 parallelly 執行：</span><span class="sxs-lookup"><span data-stu-id="e4605-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-cs/samples/sample5.xml)]

<span data-ttu-id="e4605-119">為了示範，這個動畫 （及其他使用此控制項的工具組建立的動畫） 會執行使用 JavaScript 程式碼之後執行網頁。</span><span class="sxs-lookup"><span data-stu-id="e4605-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="e4605-120">我們第一次，所有需要存取`AnimationExtender`控制項。</span><span class="sxs-lookup"><span data-stu-id="e4605-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="e4605-121">ASP.NET AJAX 程式庫提供`$find()`函式，這項工作：</span><span class="sxs-lookup"><span data-stu-id="e4605-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-cs/samples/sample6.cs)]

<span data-ttu-id="e4605-122">`AnimationExtender`控制項不僅會公開一個豐富的 API，包括具有名稱相同的 XML 標記中使用的事件處理常式的方法： `OnClick()`， `OnLoad()`，依此類推。</span><span class="sxs-lookup"><span data-stu-id="e4605-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="e4605-123">比方說，呼叫`OnClick()`方法執行內動畫`<OnClick>`元素`AnimationExtender`控制項：</span><span class="sxs-lookup"><span data-stu-id="e4605-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-cs/samples/sample7.js)]

<span data-ttu-id="e4605-124">以下是完整用戶端 JavaScript 程式碼，來模擬面板上的按一下頁面已完全載入後，請注意，`pageLoad()`函式的名稱會由呼叫 ASP.NET AJAX 一次頁面和 JavaScript 程式庫已包含所有載入。</span><span class="sxs-lookup"><span data-stu-id="e4605-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-cs/samples/sample8.html)]


<span data-ttu-id="e4605-125">[![動畫便會立即執行，不按滑鼠](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e4605-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-cs/_static/image2.png)](executing-animations-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="e4605-126">動畫會立即執行，而按一下滑鼠 ([按一下以檢視完整大小的影像](executing-animations-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e4605-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4605-127">[上一頁](modifying-animations-from-the-server-side-cs.md)
> [下一頁](changing-an-animation-using-client-side-code-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e4605-127">[Previous](modifying-animations-from-the-server-side-cs.md)
[Next](changing-an-animation-using-client-side-code-cs.md)</span></span>
