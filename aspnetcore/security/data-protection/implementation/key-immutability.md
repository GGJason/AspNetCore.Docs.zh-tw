---
title: ASP.NET Core 中的金鑰不可變性和金鑰設定
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護金鑰永久性 Api 的執行細節。
ms.author: riande
ms.date: 10/14/2016
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
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 51d7538a98ac2847f018ff1907bb5333bd132f32
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88022390"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a>ASP.NET Core 中的金鑰不可變性和金鑰設定

一旦物件保存到備份存放區，其標記法會永久固定。 新的資料可以新增至備份存放區，但現有的資料永遠不會變化。 此行為的主要目的是要防止資料損毀。

這個行為的其中一個結果是，一旦金鑰寫入至備份存放區，它就是不可變的。 它的建立、啟用和到期日永遠不會變更，但可以使用撤銷 `IKeyManager` 。 此外，其基礎演算法資訊、主要金鑰內容和待用加密屬性也是不可變的。

如果開發人員變更任何會影響金鑰持續性的設定，這些變更在下一次產生金鑰之前都不會生效，不論是透過明確呼叫 `IKeyManager.CreateNewKey` 或透過資料保護系統本身的[自動金鑰產生](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)行為。 影響金鑰持續性的設定如下：

* [預設金鑰存留期](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [待用金鑰加密機制](xref:security/data-protection/implementation/key-encryption-at-rest)

* [包含在索引鍵中的演算法資訊](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

如果您需要這些設定，以在下一個自動的金鑰滾動時間之前開始進行，請考慮明確地呼叫 `IKeyManager.CreateNewKey` 以強制建立新的金鑰。 請記得提供明確的啟用日期 ( {now + 2 天} 是不錯的經驗法則，可讓您在呼叫中傳播) 和到期日的時間。

>[!TIP]
> 所有觸及存放庫的應用程式都應該使用擴充方法來指定相同的設定 `IDataProtectionBuilder` 。 否則，保存的金鑰屬性會相依于叫用金鑰產生常式的特定應用程式。
