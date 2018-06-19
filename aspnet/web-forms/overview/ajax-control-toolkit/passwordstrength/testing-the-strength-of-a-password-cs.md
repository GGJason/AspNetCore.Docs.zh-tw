---
uid: web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
title: 測試密碼 (C#) 的強度 |Microsoft 文件
author: wenz
description: 密碼是必要幾乎任何地方，如此延遲使用者傾向於選擇簡單的密碼，這很容易中斷。 在 ASP PasswordStrength 控制項。N...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: cb4afbae-9b8f-483d-9729-476d4b9f85fc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/passwordstrength/testing-the-strength-of-a-password-cs
msc.type: authoredcontent
ms.openlocfilehash: f5f4a7128f2edbef4fbe95faf9de19bdae5f436e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869214"
---
<a name="testing-the-strength-of-a-password-c"></a><span data-ttu-id="c18f7-104">測試的強度的密碼 (C#)</span><span class="sxs-lookup"><span data-stu-id="c18f7-104">Testing the Strength of a Password (C#)</span></span>
====================
<span data-ttu-id="c18f7-105">由[Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c18f7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c18f7-106">[下載程式碼](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip)或[下載 PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="c18f7-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PasswordStrength0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/passwordstrength0CS.pdf)</span></span>

> <span data-ttu-id="c18f7-107">密碼是必要幾乎任何地方，如此延遲使用者傾向於選擇簡單的密碼，這很容易中斷。</span><span class="sxs-lookup"><span data-stu-id="c18f7-107">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="c18f7-108">在 ASP.NET AJAX Control Toolkit PasswordStrength 控制項可以檢查密碼妥當。</span><span class="sxs-lookup"><span data-stu-id="c18f7-108">The PasswordStrength control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>


## <a name="overview"></a><span data-ttu-id="c18f7-109">總覽</span><span class="sxs-lookup"><span data-stu-id="c18f7-109">Overview</span></span>

<span data-ttu-id="c18f7-110">密碼是必要幾乎任何地方，如此延遲使用者傾向於選擇簡單的密碼，這很容易中斷。</span><span class="sxs-lookup"><span data-stu-id="c18f7-110">Passwords are required almost anywhere, so that lazy users tend to choose simple passwords which are easy to break.</span></span> <span data-ttu-id="c18f7-111">`PasswordStrength` ASP.NET AJAX Control Toolkit 中的控制項可以檢查密碼是否妥當。</span><span class="sxs-lookup"><span data-stu-id="c18f7-111">The `PasswordStrength` control in the ASP.NET AJAX Control Toolkit can check how good a password is.</span></span>

## <a name="steps"></a><span data-ttu-id="c18f7-112">步驟</span><span class="sxs-lookup"><span data-stu-id="c18f7-112">Steps</span></span>

<span data-ttu-id="c18f7-113">`PasswordStrength`控制項延伸文字方塊中，並檢查是否夠好中它的密碼。</span><span class="sxs-lookup"><span data-stu-id="c18f7-113">The `PasswordStrength` control extends a text box and checks whether the password in it is good enough.</span></span> <span data-ttu-id="c18f7-114">它提供豐富的選項，透過屬性;以下是只是某些它們：</span><span class="sxs-lookup"><span data-stu-id="c18f7-114">It offers a wealth of options via attributes; here are just some of them:</span></span>

- <span data-ttu-id="c18f7-115">`MinimumNumericCharacters` 密碼所需的數字字元數目下限</span><span class="sxs-lookup"><span data-stu-id="c18f7-115">`MinimumNumericCharacters` minimum number of numeric characters required in the password</span></span>
- <span data-ttu-id="c18f7-116">`MinimumSymbolCharacters` 密碼所需的符號字元 （沒有字母和數字） 的最小的數目</span><span class="sxs-lookup"><span data-stu-id="c18f7-116">`MinimumSymbolCharacters` minimum number of symbol characters (not letters and digits) required in the password</span></span>
- <span data-ttu-id="c18f7-117">`PreferredPasswordLength` 最小密碼長度</span><span class="sxs-lookup"><span data-stu-id="c18f7-117">`PreferredPasswordLength` minimum length of the password</span></span>
- <span data-ttu-id="c18f7-118">`RequiresUpperAndLowerCaseCharacters` 是否必須使用大寫和小寫字元的密碼</span><span class="sxs-lookup"><span data-stu-id="c18f7-118">`RequiresUpperAndLowerCaseCharacters` whether the password needs to use both uppercase and lowercase characters</span></span>

<span data-ttu-id="c18f7-119">`StrengthIndicatorType`提供的資訊如何呈現的強度密碼，以文字 (值`"Text"`) 或做為一種進度列 (值`"BarIndicator"`)。</span><span class="sxs-lookup"><span data-stu-id="c18f7-119">The `StrengthIndicatorType` provides the information how to present the strength of the password, as text (value `"Text"`) or as a kind of progress bar (value `"BarIndicator"`).</span></span> <span data-ttu-id="c18f7-120">在`DisplayPosition`屬性，您將設定在出現的資訊。</span><span class="sxs-lookup"><span data-stu-id="c18f7-120">In the `DisplayPosition` attribute, you configure where the information appears.</span></span> <span data-ttu-id="c18f7-121">以下是完整的範例，包括 ASP.NET AJAX`ScriptManager`控制項，`PasswordStrength`控制項和當然文字方塊中，使用者可以在其中輸入密碼。</span><span class="sxs-lookup"><span data-stu-id="c18f7-121">Here is a complete example, including the ASP.NET AJAX `ScriptManager` control, the `PasswordStrength` control and of course a text box where the user may enter a password.</span></span> <span data-ttu-id="c18f7-122">為了示範，後者的表單欄位為一般文字和密碼欄位，讓您可以查看在開發期間您輸入。</span><span class="sxs-lookup"><span data-stu-id="c18f7-122">For the sake of demonstration, the latter form field is a regular text field and not a password field so that you can see during development what you are typing.</span></span>

[!code-aspx[Main](testing-the-strength-of-a-password-cs/samples/sample1.aspx)]

<span data-ttu-id="c18f7-123">執行網頁，並立即輸入： 僅為以視為輸入小寫字母、 大寫字母、 數字和符號之後，密碼。</span><span class="sxs-lookup"><span data-stu-id="c18f7-123">Run the page and type away: Only after you have entered lowercase letters, uppercase letters, digits and symbols, the password is deemed as unbreakable .</span></span>


<span data-ttu-id="c18f7-124">[![現在，密碼是 （） 好](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c18f7-124">[![Now the password is (quite) good](testing-the-strength-of-a-password-cs/_static/image2.png)](testing-the-strength-of-a-password-cs/_static/image1.png)</span></span>

<span data-ttu-id="c18f7-125">密碼 （） 好現在 ([按一下以檢視完整大小的影像](testing-the-strength-of-a-password-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c18f7-125">Now the password is (quite) good ([Click to view full-size image](testing-the-strength-of-a-password-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c18f7-126">下一步</span><span class="sxs-lookup"><span data-stu-id="c18f7-126">Next</span></span>](testing-the-strength-of-a-password-vb.md)
