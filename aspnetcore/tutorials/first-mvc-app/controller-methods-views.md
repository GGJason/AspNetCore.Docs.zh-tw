---
title: ASP.NET Core 中的控制器方法和檢視
author: rick-anderson
description: 了解如何在 ASP.NET Core 中使用控制器方法、檢視和 DataAnnotations。
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-mvc-app/controller-methods-views
ms.openlocfilehash: 3f84d242a41bc482110d87ff342fa5b5d8c870ff
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34729849"
---
# <a name="controller-methods-and-views-in-aspnet-core"></a><span data-ttu-id="0d1ce-103">ASP.NET Core 中的控制器方法和檢視</span><span class="sxs-lookup"><span data-stu-id="0d1ce-103">Controller methods and views in ASP.NET Core</span></span>

<span data-ttu-id="0d1ce-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="0d1ce-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0d1ce-105">剛開始使用此電影應用程式還不錯，但其呈現效果卻不理想。</span><span class="sxs-lookup"><span data-stu-id="0d1ce-105">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="0d1ce-106">我們不想看到時間 (下圖的 12:00:00 AM)，而且 **ReleaseDate** 應該是兩個分開的字。</span><span class="sxs-lookup"><span data-stu-id="0d1ce-106">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be two words.</span></span>

![索引檢視：Release Date (發行日期) 是一個字 (不含空格)，且每個電影的發行日期均顯示 12 AM 的時間](working-with-sql/_static/m55.png)

<span data-ttu-id="0d1ce-108">開啟 *Models/Movie.cs* 檔案，然後新增反白顯示的程式碼行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0d1ce-108">Open the *Models/Movie.cs* file and add the highlighted lines shown below:</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="0d1ce-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span><span class="sxs-lookup"><span data-stu-id="0d1ce-109">[!code-csharp[](start-mvc/sample/MvcMovie21/Models/MovieDateFixed.cs?name=snippet_1&highlight=2,3,12-13,17)]</span></span>
::: moniker-end
::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="0d1ce-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span><span class="sxs-lookup"><span data-stu-id="0d1ce-110">[!code-csharp[](start-mvc/sample/MvcMovie/Models/MovieDateWithExtraUsings.cs?name=snippet_1&highlight=13-14)]</span></span>
::: moniker-end

[!INCLUDE [adding-model](~/includes/mvc-intro/controller-methods-views.md)]

> [!div class="step-by-step"]
> <span data-ttu-id="0d1ce-111">[上一頁](working-with-sql.md)
> [下一頁](search.md)</span><span class="sxs-lookup"><span data-stu-id="0d1ce-111">[Previous](working-with-sql.md)
[Next](search.md)</span></span>  
