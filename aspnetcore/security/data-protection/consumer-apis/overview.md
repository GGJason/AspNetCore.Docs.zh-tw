---
title: ASP.NET Core 的取用者 Api 總覽
author: rick-anderson
description: 取得 ASP.NET Core 資料保護程式庫中可用的各種取用者 Api 的簡要總覽。
ms.author: riande
ms.date: 06/11/2019
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
uid: security/data-protection/consumer-apis/overview
ms.openlocfilehash: 985c8cdc3518a51b9ec764407f4e2e3e5ff07e12
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88021129"
---
# <a name="consumer-apis-overview-for-aspnet-core"></a><span data-ttu-id="e3b78-103">ASP.NET Core 的取用者 Api 總覽</span><span class="sxs-lookup"><span data-stu-id="e3b78-103">Consumer APIs overview for ASP.NET Core</span></span>

<span data-ttu-id="e3b78-104">`IDataProtectionProvider`和 `IDataProtector` 介面是取用者用來使用資料保護系統的基本介面。</span><span class="sxs-lookup"><span data-stu-id="e3b78-104">The `IDataProtectionProvider` and `IDataProtector` interfaces are the basic interfaces through which consumers use the data protection system.</span></span> <span data-ttu-id="e3b78-105">它們位於[AspNetCore. DataProtection. 抽象](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/)封裝中。</span><span class="sxs-lookup"><span data-stu-id="e3b78-105">They're located in the [Microsoft.AspNetCore.DataProtection.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Abstractions/) package.</span></span>

## <a name="idataprotectionprovider"></a><span data-ttu-id="e3b78-106">IDataProtectionProvider</span><span class="sxs-lookup"><span data-stu-id="e3b78-106">IDataProtectionProvider</span></span>

<span data-ttu-id="e3b78-107">提供者介面代表資料保護系統的根。</span><span class="sxs-lookup"><span data-stu-id="e3b78-107">The provider interface represents the root of the data protection system.</span></span> <span data-ttu-id="e3b78-108">它不能直接用來保護或解除保護資料。</span><span class="sxs-lookup"><span data-stu-id="e3b78-108">It cannot directly be used to protect or unprotect data.</span></span> <span data-ttu-id="e3b78-109">相反地，取用者必須藉由呼叫來取得的參考 `IDataProtector` `IDataProtectionProvider.CreateProtector(purpose)` ，其中目的是描述預定取用者使用案例的字串。</span><span class="sxs-lookup"><span data-stu-id="e3b78-109">Instead, the consumer must get a reference to an `IDataProtector` by calling `IDataProtectionProvider.CreateProtector(purpose)`, where purpose is a string that describes the intended consumer use case.</span></span> <span data-ttu-id="e3b78-110">如需此參數之意圖的詳細資訊，以及如何選擇適當的值，請參閱[目的字串](xref:security/data-protection/consumer-apis/purpose-strings)。</span><span class="sxs-lookup"><span data-stu-id="e3b78-110">See [Purpose Strings](xref:security/data-protection/consumer-apis/purpose-strings) for much more information on the intent of this parameter and how to choose an appropriate value.</span></span>

## <a name="idataprotector"></a><span data-ttu-id="e3b78-111">Idataprotector 加密</span><span class="sxs-lookup"><span data-stu-id="e3b78-111">IDataProtector</span></span>

<span data-ttu-id="e3b78-112">對的呼叫會傳回保護裝置介面 `CreateProtector` ，而這是取用者用來執行保護和取消保護作業的介面。</span><span class="sxs-lookup"><span data-stu-id="e3b78-112">The protector interface is returned by a call to `CreateProtector`, and it's this interface which consumers can use to perform protect and unprotect operations.</span></span>

<span data-ttu-id="e3b78-113">若要保護資料片段，請將資料傳遞至 `Protect` 方法。</span><span class="sxs-lookup"><span data-stu-id="e3b78-113">To protect a piece of data, pass the data to the `Protect` method.</span></span> <span data-ttu-id="e3b78-114">基本介面會定義轉換 byte []-> byte [] 的方法，但也會提供多載 (做為擴充方法) 以轉換字串 > 字串。</span><span class="sxs-lookup"><span data-stu-id="e3b78-114">The basic interface defines a method which converts byte[] -> byte[], but there's also an overload (provided as an extension method) which converts string -> string.</span></span> <span data-ttu-id="e3b78-115">這兩種方法所提供的安全性完全相同;開發人員應選擇最方便其使用案例的任何多載。</span><span class="sxs-lookup"><span data-stu-id="e3b78-115">The security offered by the two methods is identical; the developer should choose whichever overload is most convenient for their use case.</span></span> <span data-ttu-id="e3b78-116">不論選擇的多載，保護方法所傳回的值現在都會受到保護 (enciphered 和校訂) ，而應用程式可以將它傳送至不受信任的用戶端。</span><span class="sxs-lookup"><span data-stu-id="e3b78-116">Irrespective of the overload chosen, the value returned by the Protect method is now protected (enciphered and tamper-proofed), and the application can send it to an untrusted client.</span></span>

<span data-ttu-id="e3b78-117">若要將先前保護的資料片段解除保護，請將受保護的資料傳遞給 `Unprotect` 方法。</span><span class="sxs-lookup"><span data-stu-id="e3b78-117">To unprotect a previously-protected piece of data, pass the protected data to the `Unprotect` method.</span></span> <span data-ttu-id="e3b78-118"> (有以位元組 [] 為基礎和以字串為基礎的多載，可供開發人員使用。 ) 如果受保護的承載是由先前的呼叫所產生 `Protect` `IDataProtector` ，則此 `Unprotect` 方法會傳回原始未受保護的裝載。</span><span class="sxs-lookup"><span data-stu-id="e3b78-118">(There are byte[]-based and string-based overloads for developer convenience.) If the protected payload was generated by an earlier call to `Protect` on this same `IDataProtector`, the `Unprotect` method will return the original unprotected payload.</span></span> <span data-ttu-id="e3b78-119">如果受保護的內容已遭修改，或是由不同的所產生 `IDataProtector` ，此 `Unprotect` 方法將會擲回 system.security.cryptography.cryptographicexception。</span><span class="sxs-lookup"><span data-stu-id="e3b78-119">If the protected payload has been tampered with or was produced by a different `IDataProtector`, the `Unprotect` method will throw CryptographicException.</span></span>

<span data-ttu-id="e3b78-120">相同與不同之處的概念會 `IDataProtector` 轉回目的概念。</span><span class="sxs-lookup"><span data-stu-id="e3b78-120">The concept of same vs. different `IDataProtector` ties back to the concept of purpose.</span></span> <span data-ttu-id="e3b78-121">如果兩個 `IDataProtector` 實例是從相同的根目錄產生 `IDataProtectionProvider` ，但在對的呼叫中是透過不同的目的字串 `IDataProtectionProvider.CreateProtector` ，則會將它們視為不同的[保護](xref:security/data-protection/consumer-apis/purpose-strings)裝置，而且其中一個將無法取消保護另一個所產生的裝載。</span><span class="sxs-lookup"><span data-stu-id="e3b78-121">If two `IDataProtector` instances were generated from the same root `IDataProtectionProvider` but via different purpose strings in the call to `IDataProtectionProvider.CreateProtector`, then they're considered [different protectors](xref:security/data-protection/consumer-apis/purpose-strings), and one won't be able to unprotect payloads generated by the other.</span></span>

## <a name="consuming-these-interfaces"></a><span data-ttu-id="e3b78-122">使用這些介面</span><span class="sxs-lookup"><span data-stu-id="e3b78-122">Consuming these interfaces</span></span>

<span data-ttu-id="e3b78-123">針對 DI 感知元件，預期的用法是元件會 `IDataProtectionProvider` 在其程式中採用參數，而且 DI 系統會在元件具現化時，自動提供此服務。</span><span class="sxs-lookup"><span data-stu-id="e3b78-123">For a DI-aware component, the intended usage is that the component takes an `IDataProtectionProvider` parameter in its constructor and that the DI system automatically provides this service when the component is instantiated.</span></span>

> [!NOTE]
> <span data-ttu-id="e3b78-124">某些應用程式 (例如主控台應用程式或 ASP.NET 4.x 應用程式) 可能不是 DI 感知，因此無法使用此處所述的機制。</span><span class="sxs-lookup"><span data-stu-id="e3b78-124">Some applications (such as console applications or ASP.NET 4.x applications) might not be DI-aware so cannot use the mechanism described here.</span></span> <span data-ttu-id="e3b78-125">針對這些案例，請參閱[非 Di 感知案例](xref:security/data-protection/configuration/non-di-scenarios)檔，以取得取得 `IDataProtection` 提供者實例而不透過 DI 的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="e3b78-125">For these scenarios consult the [Non DI Aware Scenarios](xref:security/data-protection/configuration/non-di-scenarios) document for more information on getting an instance of an `IDataProtection` provider without going through DI.</span></span>

<span data-ttu-id="e3b78-126">下列範例示範三個概念：</span><span class="sxs-lookup"><span data-stu-id="e3b78-126">The following sample demonstrates three concepts:</span></span>

1. <span data-ttu-id="e3b78-127">[將資料保護系統新增](xref:security/data-protection/configuration/overview)至服務容器，</span><span class="sxs-lookup"><span data-stu-id="e3b78-127">[Add the data protection system](xref:security/data-protection/configuration/overview) to the service container,</span></span>

2. <span data-ttu-id="e3b78-128">使用 DI 來接收的實例 `IDataProtectionProvider` 、和</span><span class="sxs-lookup"><span data-stu-id="e3b78-128">Using DI to receive an instance of an `IDataProtectionProvider`, and</span></span>

3. <span data-ttu-id="e3b78-129">從建立 `IDataProtector` `IDataProtectionProvider` ，並使用它來保護和取消保護資料。</span><span class="sxs-lookup"><span data-stu-id="e3b78-129">Creating an `IDataProtector` from an `IDataProtectionProvider` and using it to protect and unprotect data.</span></span>

[!code-csharp[](../using-data-protection/samples/protectunprotect.cs?highlight=26,34,35,36,37,38,39,40)]

<span data-ttu-id="e3b78-130">AspNetCore. DataProtection 的封裝包含擴充方法 `IServiceProvider.GetDataProtector` ，以方便開發人員使用。</span><span class="sxs-lookup"><span data-stu-id="e3b78-130">The package Microsoft.AspNetCore.DataProtection.Abstractions contains an extension method `IServiceProvider.GetDataProtector` as a developer convenience.</span></span> <span data-ttu-id="e3b78-131">它會將封裝為單一作業 `IDataProtectionProvider` ，並從服務提供者抓取並呼叫 `IDataProtectionProvider.CreateProtector` 。</span><span class="sxs-lookup"><span data-stu-id="e3b78-131">It encapsulates as a single operation both retrieving an `IDataProtectionProvider` from the service provider and calling `IDataProtectionProvider.CreateProtector`.</span></span> <span data-ttu-id="e3b78-132">下列範例示範其使用方式。</span><span class="sxs-lookup"><span data-stu-id="e3b78-132">The following sample demonstrates its usage.</span></span>

[!code-csharp[](./overview/samples/getdataprotector.cs?highlight=15)]

>[!TIP]
> <span data-ttu-id="e3b78-133">和的 `IDataProtectionProvider` 實例 `IDataProtector` 是多個呼叫端的安全線程。</span><span class="sxs-lookup"><span data-stu-id="e3b78-133">Instances of `IDataProtectionProvider` and `IDataProtector` are thread-safe for multiple callers.</span></span> <span data-ttu-id="e3b78-134">這是因為一旦元件透過呼叫取得的參考 `IDataProtector` `CreateProtector` ，它就會使用該參考進行對和的多次呼叫 `Protect` `Unprotect` 。</span><span class="sxs-lookup"><span data-stu-id="e3b78-134">It's intended that once a component gets a reference to an `IDataProtector` via a call to `CreateProtector`, it will use that reference for multiple calls to `Protect` and `Unprotect`.</span></span> <span data-ttu-id="e3b78-135">`Unprotect`如果無法驗證或解密受保護的內容，則的呼叫將會擲回 system.security.cryptography.cryptographicexception。</span><span class="sxs-lookup"><span data-stu-id="e3b78-135">A call to `Unprotect` will throw CryptographicException if the protected payload cannot be verified or deciphered.</span></span> <span data-ttu-id="e3b78-136">某些元件可能會想要在取消保護作業期間忽略錯誤;讀取驗證的元件 cookie 可能會處理此錯誤，並將要求視為 cookie 完全不會使要求失敗。</span><span class="sxs-lookup"><span data-stu-id="e3b78-136">Some components may wish to ignore errors during unprotect operations; a component which reads authentication cookies might handle this error and treat the request as if it had no cookie at all rather than fail the request outright.</span></span> <span data-ttu-id="e3b78-137">需要此行為的元件應該特別捕捉 System.security.cryptography.cryptographicexception，而不是抑制所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e3b78-137">Components which want this behavior should specifically catch CryptographicException instead of swallowing all exceptions.</span></span>
