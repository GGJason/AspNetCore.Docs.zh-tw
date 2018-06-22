---
title: 索引鍵中 ASP.NET Core 的不變性和金鑰設定
author: rick-anderson
description: 了解 ASP.NET Core 資料保護金鑰不變性應用程式開發介面的實作詳細資料。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/implementation/key-immutability
ms.openlocfilehash: 45460e0bdf6ad0a978f984d55b64f562c13fb575
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279450"
---
# <a name="key-immutability-and-key-settings-in-aspnet-core"></a><span data-ttu-id="c7c6d-103">索引鍵中 ASP.NET Core 的不變性和金鑰設定</span><span class="sxs-lookup"><span data-stu-id="c7c6d-103">Key immutability and key settings in ASP.NET Core</span></span>

<span data-ttu-id="c7c6d-104">一旦物件會保存到備份存放區，其表示是永久固定的。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-104">Once an object is persisted to the backing store, its representation is forever fixed.</span></span> <span data-ttu-id="c7c6d-105">備份存放區中，可以加入新的資料，但是可以永遠不會變更現有的資料。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-105">New data can be added to the backing store, but existing data can never be mutated.</span></span> <span data-ttu-id="c7c6d-106">此行為的主要用途是防止資料損毀。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-106">The primary purpose of this behavior is to prevent data corruption.</span></span>

<span data-ttu-id="c7c6d-107">此行為的一種結果是一旦金鑰會寫入備份存放區，即不可變。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-107">One consequence of this behavior is that once a key is written to the backing store, it's immutable.</span></span> <span data-ttu-id="c7c6d-108">其建立、 啟用和到期日期可以永遠不會變更，但它可以撤銷使用`IKeyManager`。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-108">Its creation, activation, and expiration dates can never be changed, though it can revoked by using `IKeyManager`.</span></span> <span data-ttu-id="c7c6d-109">此外，其基礎的演算法資訊、 主要金鑰處理內容和加密其他屬性也是不變。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-109">Additionally, its underlying algorithmic information, master keying material, and encryption at rest properties are also immutable.</span></span>

<span data-ttu-id="c7c6d-110">如果開發人員變更任何設定，會影響金鑰的持續性，這些變更將不會生效之前產生金鑰時，透過明確呼叫在下一次`IKeyManager.CreateNewKey`或透過資料保護系統本身[自動金鑰產生](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)行為。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-110">If the developer changes any setting that affects key persistence, those changes won't go into effect until the next time a key is generated, either via an explicit call to `IKeyManager.CreateNewKey` or via the data protection system's own [automatic key generation](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management) behavior.</span></span> <span data-ttu-id="c7c6d-111">會影響金鑰的持續性的設定如下所示：</span><span class="sxs-lookup"><span data-stu-id="c7c6d-111">The settings that affect key persistence are as follows:</span></span>

* [<span data-ttu-id="c7c6d-112">預設金鑰存留期</span><span class="sxs-lookup"><span data-stu-id="c7c6d-112">The default key lifetime</span></span>](xref:security/data-protection/implementation/key-management#data-protection-implementation-key-management)

* [<span data-ttu-id="c7c6d-113">在 rest 機制金鑰加密</span><span class="sxs-lookup"><span data-stu-id="c7c6d-113">The key encryption at rest mechanism</span></span>](xref:security/data-protection/implementation/key-encryption-at-rest#data-protection-implementation-key-encryption-at-rest)

* [<span data-ttu-id="c7c6d-114">包含金鑰的演算法資訊</span><span class="sxs-lookup"><span data-stu-id="c7c6d-114">The algorithmic information contained within the key</span></span>](xref:security/data-protection/configuration/overview#changing-algorithms-with-usecryptographicalgorithms)

<span data-ttu-id="c7c6d-115">如果您需要早於下一步 自動金鑰循環時間開始進行這些設定，請考慮進行明確呼叫`IKeyManager.CreateNewKey`強制建立新的金鑰。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-115">If you need these settings to kick in earlier than the next automatic key rolling time, consider making an explicit call to `IKeyManager.CreateNewKey` to force the creation of a new key.</span></span> <span data-ttu-id="c7c6d-116">請記得要提供明確的啟動日期 （{現在 + 2 天} 是最佳經驗法則，讓變更傳播的時間） 與呼叫中的到期日。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-116">Remember to provide an explicit activation date ({ now + 2 days } is a good rule of thumb to allow time for the change to propagate) and expiration date in the call.</span></span>

>[!TIP]
> <span data-ttu-id="c7c6d-117">所有碰觸的儲存機制的應用程式應該指定相同的設定與`IDataProtectionBuilder`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-117">All applications touching the repository should specify the same settings with the `IDataProtectionBuilder` extension methods.</span></span> <span data-ttu-id="c7c6d-118">否則，保存金鑰的屬性將會相依於特定叫用的索引鍵產生常式的應用程式。</span><span class="sxs-lookup"><span data-stu-id="c7c6d-118">Otherwise, the properties of the persisted key will be dependent on the particular application that invoked the key generation routines.</span></span>
