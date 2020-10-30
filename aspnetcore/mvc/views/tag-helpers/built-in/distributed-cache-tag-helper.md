---
title: ASP.NET Core 中的分散式快取標籤協助程式
author: pkellner
description: 了解如何使用分散式快取標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2020
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper
ms.openlocfilehash: 04ab5be4d9cec066a4b7cd422a1566bcbb5a291a
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93061154"
---
# <a name="distributed-cache-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的分散式快取標籤協助程式

由 [Peter Kellner](https://peterkellner.net) 提供

分散式快取標籤協助程式可將 ASP.NET Core 應用程式內容快取至分散式快取來源，因此大幅提升應用程式的效能。

如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。

分散式快取標籤協助程式繼承自與快取標籤協助程式相同的基底類別。 分散式標籤協助程式可以使用所有[快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)屬性。

分散式快取標籤協助程式會使用[建構函式插入](xref:fundamentals/dependency-injection#constructor-injection-behavior)。 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 介面會被傳入分散式快取標籤協助程式的建構函式。 如果 `Startup.ConfigureServices` 中未建立 `IDistributedCache` 的具體實作 ( *Startup.cs* )， [分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)將會使用與快取標籤協助程式相同的記憶體內部提供者來儲存快取資料。

## <a name="distributed-cache-tag-helper-attributes"></a>分散式快取標籤協助程式屬性

### <a name="attributes-shared-with-the-cache-tag-helper"></a>與快取標籤協助程式共用的屬性

* `enabled`
* `expires-on`
* `expires-after`
* `expires-sliding`
* `vary-by-header`
* `vary-by-query`
* `vary-by-route`
* `vary-by-cookie`
* `vary-by-user`
* `vary-by priority`

分散式快取標籤協助程式繼承自與快取標籤協助程式相同的類別。 如需這些屬性的描述，請參閱[快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。

### <a name="name"></a>NAME

| 屬性類型 | 範例                               |
| -------------- | ------------------------------------- |
| String         | `my-distributed-cache-unique-key-101` |

`name` 是必要的。 `name` 屬性會作為每個預存快取執行個體的索引鍵。 不同于快取標籤協助程式會根據頁面名稱和頁面上的位置，將快取索引鍵指派給每個實例，分散式快取標記協助程式只會以其索引鍵做為 Razor Razor 屬性 `name` 。

範例：

```cshtml
<distributed-cache name="my-distributed-cache-unique-key-101">
    Time Inside Cache Tag Helper: @DateTime.Now
</distributed-cache>
```

## <a name="distributed-cache-tag-helper-idistributedcache-implementations"></a>分散式快取標記協助程式 IDistributedCache 實作

ASP.NET Core 內建兩項 <xref:Microsoft.Extensions.Caching.Distributed.IDistributedCache> 實作。 其中一個是以 SQL Server 為基礎，另一項是以 Redis 為基礎。 此外也提供協力廠商的實作為 [NCache](http://www.alachisoft.com/ncache/aspnet-core-idistributedcache-ncache.html)。 如需這些實作的詳細資料，請參閱<xref:performance/caching/distributed>。 這兩個實作都涉及在 `Startup` 的 `IDistributedCache` 中設定執行個體。

沒有特別與使用 `IDistributedCache` 的任何特定實作建立關聯的標籤屬性。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:fundamentals/dependency-injection>
* <xref:performance/caching/distributed>
* <xref:performance/caching/memory>
* <xref:security/authentication/identity>
