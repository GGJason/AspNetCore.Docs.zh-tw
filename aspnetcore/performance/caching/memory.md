---
title: 快取在記憶體中的 ASP.NET Core
author: rick-anderson
description: 了解如何快取在記憶體中 ASP.NET Core 的資料。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/caching/memory
ms.openlocfilehash: eca6610caf4e0a654c9a31f89a42e2ac82e94d23
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734480"
---
# <a name="cache-in-memory-in-aspnet-core"></a><span data-ttu-id="93cf1-103">快取在記憶體中的 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="93cf1-103">Cache in-memory in ASP.NET Core</span></span>

<span data-ttu-id="93cf1-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [John Luo](https://github.com/JunTaoLuo)，和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="93cf1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [John Luo](https://github.com/JunTaoLuo), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="93cf1-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="93cf1-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/memory/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="caching-basics"></a><span data-ttu-id="93cf1-106">快取的基本概念</span><span class="sxs-lookup"><span data-stu-id="93cf1-106">Caching basics</span></span>

<span data-ttu-id="93cf1-107">快取可大幅改善的效能和延展性的應用程式藉由減少產生內容所需的工作。</span><span class="sxs-lookup"><span data-stu-id="93cf1-107">Caching can significantly improve the performance and scalability of an app by reducing the work required to generate content.</span></span> <span data-ttu-id="93cf1-108">快取就可以最適合不常變更的資料。</span><span class="sxs-lookup"><span data-stu-id="93cf1-108">Caching works best with data that changes infrequently.</span></span> <span data-ttu-id="93cf1-109">快取會建立一份可以傳回大部分的資料較快，從原始來源。</span><span class="sxs-lookup"><span data-stu-id="93cf1-109">Caching makes a copy of data that can be returned much faster than from the original source.</span></span> <span data-ttu-id="93cf1-110">您應該撰寫並測試您的應用程式永遠不會取決於快取的資料。</span><span class="sxs-lookup"><span data-stu-id="93cf1-110">You should write and test your app to never depend on cached data.</span></span>

<span data-ttu-id="93cf1-111">ASP.NET Core 支援數個不同的快取。</span><span class="sxs-lookup"><span data-stu-id="93cf1-111">ASP.NET Core supports several different caches.</span></span> <span data-ttu-id="93cf1-112">最簡單的快取根據[IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)，代表儲存在 web 伺服器的記憶體中快取。</span><span class="sxs-lookup"><span data-stu-id="93cf1-112">The simplest cache is based on the [IMemoryCache](/dotnet/api/microsoft.extensions.caching.memory.imemorycache), which represents a cache stored in the memory of the web server.</span></span> <span data-ttu-id="93cf1-113">在多部伺服器的伺服器陣列執行的應用程式應該確保使用記憶體中快取時，會自黏工作階段。</span><span class="sxs-lookup"><span data-stu-id="93cf1-113">Apps which run on a server farm of multiple servers should ensure that sessions are sticky when using the in-memory cache.</span></span> <span data-ttu-id="93cf1-114">自黏工作階段會確保後續所有用戶端的要求移到相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="93cf1-114">Sticky sessions ensure that subsequent requests from a client all go to the same server.</span></span> <span data-ttu-id="93cf1-115">例如，Azure Web 應用程式使用[應用程式要求路由](https://www.iis.net/learn/extensions/planning-for-arr)(ARR) 會將所有的後續要求路由至相同的伺服器。</span><span class="sxs-lookup"><span data-stu-id="93cf1-115">For example, Azure Web apps use [Application Request Routing](https://www.iis.net/learn/extensions/planning-for-arr) (ARR) to route all subsequent requests to the same server.</span></span>

<span data-ttu-id="93cf1-116">Web 伺服陣列中的非黏性工作階段需要[分散式快取](distributed.md)以避免快取一致性問題。</span><span class="sxs-lookup"><span data-stu-id="93cf1-116">Non-sticky sessions in a web farm require a [distributed cache](distributed.md) to avoid cache consistency problems.</span></span> <span data-ttu-id="93cf1-117">對於某些應用程式中，分散式快取可以支援更高範圍外比記憶體中快取。</span><span class="sxs-lookup"><span data-stu-id="93cf1-117">For some apps, a distributed cache can support higher scale out than an in-memory cache.</span></span> <span data-ttu-id="93cf1-118">使用分散式快取卸載到外部處理序的快取記憶體。</span><span class="sxs-lookup"><span data-stu-id="93cf1-118">Using a distributed cache offloads the cache memory to an external process.</span></span>

<span data-ttu-id="93cf1-119">`IMemoryCache`快取將會收回快取記憶體不足壓力下的項目，除非[快取優先順序](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority)設`CacheItemPriority.NeverRemove`。</span><span class="sxs-lookup"><span data-stu-id="93cf1-119">The `IMemoryCache` cache will evict cache entries under memory pressure unless the [cache priority](/dotnet/api/microsoft.extensions.caching.memory.cacheitempriority) is set to `CacheItemPriority.NeverRemove`.</span></span> <span data-ttu-id="93cf1-120">您可以設定`CacheItemPriority`調整與快取收回項目記憶體不足壓力下的優先順序。</span><span class="sxs-lookup"><span data-stu-id="93cf1-120">You can set the `CacheItemPriority` to adjust the priority with which the cache evicts items under memory pressure.</span></span>

<span data-ttu-id="93cf1-121">記憶體中快取可以儲存任何物件。分散式快取介面僅限於`byte[]`。</span><span class="sxs-lookup"><span data-stu-id="93cf1-121">The in-memory cache can store any object; the distributed cache interface is limited to `byte[]`.</span></span>

## <a name="using-imemorycache"></a><span data-ttu-id="93cf1-122">使用 IMemoryCache</span><span class="sxs-lookup"><span data-stu-id="93cf1-122">Using IMemoryCache</span></span>

<span data-ttu-id="93cf1-123">記憶體中快取是*服務*參考從您的應用程式使用[相依性插入](../../fundamentals/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="93cf1-123">In-memory caching is a *service* that's referenced from your app using [Dependency Injection](../../fundamentals/dependency-injection.md).</span></span> <span data-ttu-id="93cf1-124">呼叫`AddMemoryCache`中`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="93cf1-124">Call `AddMemoryCache` in `ConfigureServices`:</span></span>

[!code-csharp[](memory/sample/WebCache/Startup.cs?highlight=9)]

<span data-ttu-id="93cf1-125">要求`IMemoryCache`建構函式中的執行個體：</span><span class="sxs-lookup"><span data-stu-id="93cf1-125">Request the `IMemoryCache` instance in the constructor:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ctor)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="93cf1-126">`IMemoryCache` 需要 NuGet 封裝[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)。</span><span class="sxs-lookup"><span data-stu-id="93cf1-126">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="93cf1-127">`IMemoryCache` 需要 NuGet 封裝[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)，也就是在可用[Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)。</span><span class="sxs-lookup"><span data-stu-id="93cf1-127">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.0"

<span data-ttu-id="93cf1-128">`IMemoryCache` 需要 NuGet 封裝[Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/)，也就是在可用[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="93cf1-128">`IMemoryCache` requires NuGet package [Microsoft.Extensions.Caching.Memory](https://www.nuget.org/packages/Microsoft.Extensions.Caching.Memory/), which is avaiable in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

::: moniker-end

<span data-ttu-id="93cf1-129">下列程式碼會使用[TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__)檢查一次是否快取中。</span><span class="sxs-lookup"><span data-stu-id="93cf1-129">The following code uses [TryGetValue](/dotnet/api/microsoft.extensions.caching.memory.imemorycache.trygetvalue?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_IMemoryCache_TryGetValue_System_Object_System_Object__) to check if a time is in the cache.</span></span> <span data-ttu-id="93cf1-130">如果不快取的時間，建立並與快取中加入新項目[設定](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_)。</span><span class="sxs-lookup"><span data-stu-id="93cf1-130">If a time isn't cached, a new entry is created and added to the cache with [Set](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.set?view=aspnetcore-2.0#Microsoft_Extensions_Caching_Memory_CacheExtensions_Set__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object___0_Microsoft_Extensions_Caching_Memory_MemoryCacheEntryOptions_).</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="93cf1-131">會顯示目前的時間和快取的時間：</span><span class="sxs-lookup"><span data-stu-id="93cf1-131">The current time and the cached time are displayed:</span></span>

[!code-cshtml[](memory/sample/WebCache/Views/Home/Cache.cshtml)]

<span data-ttu-id="93cf1-132">快取`DateTime`值時仍繼續使用快取中沒有要求內的逾時期限 （及任何因記憶體不足的壓力而收回）。</span><span class="sxs-lookup"><span data-stu-id="93cf1-132">The cached `DateTime` value remains in the cache while there are requests within the timeout period (and no eviction due to memory pressure).</span></span> <span data-ttu-id="93cf1-133">下圖顯示目前的時間和時間早從快取擷取：</span><span class="sxs-lookup"><span data-stu-id="93cf1-133">The following image shows the current time and an older time retrieved from the cache:</span></span>

![具有兩個不同的時間顯示的索引檢視](memory/_static/time.png)

<span data-ttu-id="93cf1-135">下列程式碼會使用[GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__)和[GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___)快取資料。</span><span class="sxs-lookup"><span data-stu-id="93cf1-135">The following code uses [GetOrCreate](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreate__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry___0__) and [GetOrCreateAsync](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions#Microsoft_Extensions_Caching_Memory_CacheExtensions_GetOrCreateAsync__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_System_Func_Microsoft_Extensions_Caching_Memory_ICacheEntry_System_Threading_Tasks_Task___0___) to cache data.</span></span> 

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet2&highlight=3-7,14-19)]

<span data-ttu-id="93cf1-136">下列程式碼會呼叫[取得](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_)來擷取快取的時間：</span><span class="sxs-lookup"><span data-stu-id="93cf1-136">The following code calls [Get](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions.get#Microsoft_Extensions_Caching_Memory_CacheExtensions_Get__1_Microsoft_Extensions_Caching_Memory_IMemoryCache_System_Object_) to fetch the cached time:</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_gct)]

<span data-ttu-id="93cf1-137">請參閱[IMemoryCache 方法](/dotnet/api/microsoft.extensions.caching.memory.imemorycache)和[CacheExtensions 方法](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions)的快取方法的描述。</span><span class="sxs-lookup"><span data-stu-id="93cf1-137">See [IMemoryCache methods](/dotnet/api/microsoft.extensions.caching.memory.imemorycache) and [CacheExtensions methods](/dotnet/api/microsoft.extensions.caching.memory.cacheextensions) for a description of the cache methods.</span></span>

## <a name="using-memorycacheentryoptions"></a><span data-ttu-id="93cf1-138">使用 MemoryCacheEntryOptions</span><span class="sxs-lookup"><span data-stu-id="93cf1-138">Using MemoryCacheEntryOptions</span></span>

<span data-ttu-id="93cf1-139">下列範例：</span><span class="sxs-lookup"><span data-stu-id="93cf1-139">The following sample:</span></span>

- <span data-ttu-id="93cf1-140">設定絕對到期時間。</span><span class="sxs-lookup"><span data-stu-id="93cf1-140">Sets the absolute expiration time.</span></span> <span data-ttu-id="93cf1-141">這是可以快取之項目的時間上限，並防止持續更新變動到期後變成過時的項目。</span><span class="sxs-lookup"><span data-stu-id="93cf1-141">This is the maximum time the entry can be cached and prevents the item from becoming too stale when the sliding expiration is continuously renewed.</span></span>
- <span data-ttu-id="93cf1-142">設定滑動期限。</span><span class="sxs-lookup"><span data-stu-id="93cf1-142">Sets a sliding expiration time.</span></span> <span data-ttu-id="93cf1-143">要求存取此快取的項目會重設滑動到期時鐘。</span><span class="sxs-lookup"><span data-stu-id="93cf1-143">Requests that access this cached item will reset the sliding expiration clock.</span></span>
- <span data-ttu-id="93cf1-144">若要設定快取優先權`CacheItemPriority.NeverRemove`。</span><span class="sxs-lookup"><span data-stu-id="93cf1-144">Sets the cache priority to `CacheItemPriority.NeverRemove`.</span></span>
- <span data-ttu-id="93cf1-145">設定[PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) ，將會在從快取收回項目之後呼叫。</span><span class="sxs-lookup"><span data-stu-id="93cf1-145">Sets a [PostEvictionDelegate](/dotnet/api/microsoft.extensions.caching.memory.postevictiondelegate) that will be called after the entry is evicted from the cache.</span></span> <span data-ttu-id="93cf1-146">回呼的程式碼快取中移除的項目與另一個執行緒上執行。</span><span class="sxs-lookup"><span data-stu-id="93cf1-146">The callback is run on a different thread from the code that removes the item from the cache.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_et&highlight=14-21)]

## <a name="cache-dependencies"></a><span data-ttu-id="93cf1-147">快取相依性</span><span class="sxs-lookup"><span data-stu-id="93cf1-147">Cache dependencies</span></span>

<span data-ttu-id="93cf1-148">下列範例會示範如何過期的快取項目，如果相依項目到期為止。</span><span class="sxs-lookup"><span data-stu-id="93cf1-148">The following sample shows how to expire a cache entry if a dependent entry expires.</span></span> <span data-ttu-id="93cf1-149">A`CancellationChangeToken`新增至快取的項目。</span><span class="sxs-lookup"><span data-stu-id="93cf1-149">A `CancellationChangeToken` is added to the cached item.</span></span> <span data-ttu-id="93cf1-150">當`Cancel`上呼叫`CancellationTokenSource`，收回兩個快取項目。</span><span class="sxs-lookup"><span data-stu-id="93cf1-150">When `Cancel` is called on the `CancellationTokenSource`, both cache entries are evicted.</span></span>

[!code-csharp[](memory/sample/WebCache/Controllers/HomeController.cs?name=snippet_ed)]

<span data-ttu-id="93cf1-151">使用`CancellationTokenSource`可讓多個快取項目被收回為群組。</span><span class="sxs-lookup"><span data-stu-id="93cf1-151">Using a `CancellationTokenSource` allows multiple cache entries to be evicted as a group.</span></span> <span data-ttu-id="93cf1-152">與`using`上述程式碼模式，快取項目內建立`using`區塊將會繼承觸發程序和到期日設定。</span><span class="sxs-lookup"><span data-stu-id="93cf1-152">With the `using` pattern in the code above, cache entries created inside the `using` block will inherit triggers and expiration settings.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="93cf1-153">其他備註</span><span class="sxs-lookup"><span data-stu-id="93cf1-153">Additional notes</span></span>

- <span data-ttu-id="93cf1-154">使用時回呼重新擴展快取項目：</span><span class="sxs-lookup"><span data-stu-id="93cf1-154">When using a callback to repopulate a cache item:</span></span>

  - <span data-ttu-id="93cf1-155">多個要求可以找到的快取索引鍵值空因為尚未完成的回呼。</span><span class="sxs-lookup"><span data-stu-id="93cf1-155">Multiple requests can find the cached key value empty because the callback hasn't completed.</span></span>
  - <span data-ttu-id="93cf1-156">這會導致多個執行緒重新填入快取的項目。</span><span class="sxs-lookup"><span data-stu-id="93cf1-156">This can result in several threads repopulating the cached item.</span></span>

- <span data-ttu-id="93cf1-157">若要建立另一個使用一個快取項目時，子將複製的父項目到期語彙基元和以時間為基礎的到期日設定。</span><span class="sxs-lookup"><span data-stu-id="93cf1-157">When one cache entry is used to create another, the child copies the parent entry's expiration tokens and time-based expiration settings.</span></span> <span data-ttu-id="93cf1-158">子系不過期手動移除或更新的父項目。</span><span class="sxs-lookup"><span data-stu-id="93cf1-158">The child isn't expired by manual removal or updating of the parent entry.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="93cf1-159">其他資源</span><span class="sxs-lookup"><span data-stu-id="93cf1-159">Additional resources</span></span>

* [<span data-ttu-id="93cf1-160">使用分散式快取</span><span class="sxs-lookup"><span data-stu-id="93cf1-160">Work with a distributed cache</span></span>](xref:performance/caching/distributed)
* [<span data-ttu-id="93cf1-161">使用變更權杖來偵測變更</span><span class="sxs-lookup"><span data-stu-id="93cf1-161">Detect changes with change tokens</span></span>](xref:fundamentals/primitives/change-tokens)
* [<span data-ttu-id="93cf1-162">回應快取</span><span class="sxs-lookup"><span data-stu-id="93cf1-162">Response caching</span></span>](xref:performance/caching/response)
* [<span data-ttu-id="93cf1-163">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="93cf1-163">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
* [<span data-ttu-id="93cf1-164">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="93cf1-164">Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)
* [<span data-ttu-id="93cf1-165">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="93cf1-165">Distributed Cache Tag Helper</span></span>](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)
