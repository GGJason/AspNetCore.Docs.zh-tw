---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
no-loc:
- 'appsettings.json'
- 'ASP.NET Core Identity'
- 'cookie'
- 'Cookie'
- 'Blazor'
- 'Blazor Server'
- 'Blazor WebAssembly'
- 'Identity'
- "Let's Encrypt"
- 'Razor'
- 'SignalR'
uid: security/authentication/samples
ms.openlocfilehash: 4153a443748dbff40be19e25fc1c719ee4e39609
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93060335"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="3b431-103">ASP.NET Core 的驗證範例</span><span class="sxs-lookup"><span data-stu-id="3b431-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="3b431-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="3b431-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3b431-105">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)會在 *AspNetCore/src/Security/samples* 資料夾中包含下列驗證範例：</span><span class="sxs-lookup"><span data-stu-id="3b431-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="3b431-106">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="3b431-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/ClaimsTransformation)
* <span data-ttu-id="3b431-107">[Cookie 認證](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)</span><span class="sxs-lookup"><span data-stu-id="3b431-107">[Cookie authentication](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)</span></span>
* [<span data-ttu-id="3b431-108">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="3b431-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="3b431-109">動態驗證方案和選項</span><span class="sxs-lookup"><span data-stu-id="3b431-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/DynamicSchemes)
* <span data-ttu-id="3b431-110">[外部宣告](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)</span><span class="sxs-lookup"><span data-stu-id="3b431-110">[External claims](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)</span></span>
* [<span data-ttu-id="3b431-111">cookie根據要求選取和其他驗證配置</span><span class="sxs-lookup"><span data-stu-id="3b431-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="3b431-112">限制存取靜態檔案</span><span class="sxs-lookup"><span data-stu-id="3b431-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="3b431-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="3b431-113">Run the samples</span></span>

* <span data-ttu-id="3b431-114">選取 [分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="3b431-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="3b431-115">例如， `release/3.1`</span><span class="sxs-lookup"><span data-stu-id="3b431-115">For example, `release/3.1`</span></span>
* <span data-ttu-id="3b431-116">複製或下載 [ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="3b431-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="3b431-117">確認您已安裝符合 ASP.NET Core 存放庫複製的 [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) 版本。</span><span class="sxs-lookup"><span data-stu-id="3b431-117">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="3b431-118">流覽至 *AspNetCore/src/Security/* sample 中的範例，並使用執行範例 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="3b431-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3b431-119">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)會在 *AspNetCore/src/Security/samples* 資料夾中包含下列驗證範例：</span><span class="sxs-lookup"><span data-stu-id="3b431-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="3b431-120">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="3b431-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/ClaimsTransformation)
* <span data-ttu-id="3b431-121">[Cookie 認證](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Cookies)</span><span class="sxs-lookup"><span data-stu-id="3b431-121">[Cookie authentication](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Cookies)</span></span>
* [<span data-ttu-id="3b431-122">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="3b431-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/2.1.3/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="3b431-123">動態驗證方案和選項</span><span class="sxs-lookup"><span data-stu-id="3b431-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/DynamicSchemes)
* <span data-ttu-id="3b431-124">[外部宣告](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Identity.ExternalClaims)</span><span class="sxs-lookup"><span data-stu-id="3b431-124">[External claims](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Identity.ExternalClaims)</span></span>
* [<span data-ttu-id="3b431-125">cookie根據要求選取和其他驗證配置</span><span class="sxs-lookup"><span data-stu-id="3b431-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="3b431-126">限制存取靜態檔案</span><span class="sxs-lookup"><span data-stu-id="3b431-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/2.1.3/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="3b431-127">執行範例</span><span class="sxs-lookup"><span data-stu-id="3b431-127">Run the samples</span></span>

* <span data-ttu-id="3b431-128">選取 [分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="3b431-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="3b431-129">例如， `release/2.1`</span><span class="sxs-lookup"><span data-stu-id="3b431-129">For example, `release/2.1`</span></span>
* <span data-ttu-id="3b431-130">複製或下載 [ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="3b431-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="3b431-131">確認您已安裝符合 ASP.NET Core 存放庫複製的 [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) 版本。</span><span class="sxs-lookup"><span data-stu-id="3b431-131">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="3b431-132">流覽至 *AspNetCore/src/Security/* sample 中的範例，並使用執行範例 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="3b431-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
