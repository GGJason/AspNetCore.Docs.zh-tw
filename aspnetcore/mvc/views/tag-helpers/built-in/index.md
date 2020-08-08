---
title: ASP.NET Core 內建的標記協助程式
author: pkellner
description: 了解 ASP.NET Core 內建標籤協助程式如何提升您的產能。
ms.author: riande
ms.custom: mvc
ms.date: 10/10/2018
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
uid: mvc/views/tag-helpers/builtin-th/Index
ms.openlocfilehash: 5b8d136251a68bd5dc7ee2d75700f53cfd33817f
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88018607"
---
# <a name="aspnet-core-built-in-tag-helpers"></a><span data-ttu-id="a614c-103">ASP.NET Core 內建的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a614c-103">ASP.NET Core built-in Tag Helpers</span></span>

<span data-ttu-id="a614c-104">由 [Peter Kellner](https://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="a614c-104">By [Peter Kellner](https://peterkellner.net)</span></span>

<span data-ttu-id="a614c-105">如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。</span><span class="sxs-lookup"><span data-stu-id="a614c-105">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

<span data-ttu-id="a614c-106">有一些內建標籤協助程式未列於本文件中。</span><span class="sxs-lookup"><span data-stu-id="a614c-106">There are built-in Tag Helpers which aren't listed in this document.</span></span> <span data-ttu-id="a614c-107">未列出的標籤協助程式是由 [Razor](xref:mvc/views/razor) view 引擎在內部使用。</span><span class="sxs-lookup"><span data-stu-id="a614c-107">The unlisted Tag Helpers are used internally by the [Razor](xref:mvc/views/razor) view engine.</span></span> <span data-ttu-id="a614c-108">`~` (波狀符號) 字元的標籤協助程式並未列出。</span><span class="sxs-lookup"><span data-stu-id="a614c-108">The Tag Helper for the `~` (tilde) character is unlisted.</span></span> <span data-ttu-id="a614c-109">波狀符號標籤協助程式會擴展到網站的根路徑。</span><span class="sxs-lookup"><span data-stu-id="a614c-109">The tilde Tag Helper expands to the root path of the website.</span></span>

[!INCLUDE[](~/includes/built-in-TH.md)]

## <a name="additional-resources"></a><span data-ttu-id="a614c-110">其他資源</span><span class="sxs-lookup"><span data-stu-id="a614c-110">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/th-components>
