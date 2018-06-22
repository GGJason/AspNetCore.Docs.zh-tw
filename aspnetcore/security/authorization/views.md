---
title: 在 ASP.NET Core MVC 檢視為基礎的授權
author: rick-anderson
description: 本文件將示範如何插入，並利用授權服務的 ASP.NET Core Razor 檢視內。
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: f25bab61afc93ff14bfd9c36d95a6d2e54b06dfb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277804"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="94744-103">在 ASP.NET Core MVC 檢視為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="94744-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="94744-104">開發人員通常會想要顯示、 隱藏或修改目前的使用者識別為基礎的 UI。</span><span class="sxs-lookup"><span data-stu-id="94744-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="94744-105">您可以存取授權服務會在透過 MVC 檢視內[相依性插入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="94744-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection).</span></span> <span data-ttu-id="94744-106">若要插入 Razor 檢視授權服務，使用`@inject`指示詞：</span><span class="sxs-lookup"><span data-stu-id="94744-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="94744-107">如果您想要授權服務的每個檢視中，放入`@inject`到指示詞 *_ViewImports.cshtml*檔案*檢視*目錄。</span><span class="sxs-lookup"><span data-stu-id="94744-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="94744-108">如需詳細資訊，請參閱[在檢視中插入相依性](xref:mvc/views/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="94744-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="94744-109">使用插入的授權服務叫用`AuthorizeAsync`方式完全相同時，您會檢查[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="94744-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94744-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94744-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94744-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94744-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="94744-112">在某些情況下，資源會是您的檢視模型。</span><span class="sxs-lookup"><span data-stu-id="94744-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="94744-113">叫用`AuthorizeAsync`方式完全相同時，您會檢查[資源為基礎的授權](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="94744-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="94744-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="94744-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="94744-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="94744-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="94744-116">在上述程式碼中，模型會傳遞做為原則評估應該採取的資源納入考量。</span><span class="sxs-lookup"><span data-stu-id="94744-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="94744-117">不要只依賴切換可見性的應用程式的 UI 元素的唯一授權檢查。</span><span class="sxs-lookup"><span data-stu-id="94744-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="94744-118">隱藏 UI 項目可能無法完全防止存取至其關聯之的控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="94744-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="94744-119">例如，請考慮前面的程式碼片段中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="94744-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="94744-120">使用者可以叫用`Edit`動作方法如果他或她知道之相對資源 URL 是 */Document/Edit/1*。</span><span class="sxs-lookup"><span data-stu-id="94744-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="94744-121">基於這個理由，`Edit`動作方法執行它自己的授權檢查。</span><span class="sxs-lookup"><span data-stu-id="94744-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
