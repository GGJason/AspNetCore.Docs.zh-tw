---
title: ASP.NET Core 中的已驗證加密詳細資料
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護驗證加密的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- ':::no-loc(appsettings.json):::'
- ':::no-loc(ASP.NET Core Identity):::'
- ':::no-loc(cookie):::'
- ':::no-loc(Cookie):::'
- ':::no-loc(Blazor):::'
- ':::no-loc(Blazor Server):::'
- ':::no-loc(Blazor WebAssembly):::'
- ':::no-loc(Identity):::'
- ":::no-loc(Let's Encrypt):::"
- ':::no-loc(Razor):::'
- ':::no-loc(SignalR):::'
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: 7978725534cdd3a5b425851f61b1c7ae3ada88df
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93051833"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a><span data-ttu-id="7ed8d-103">ASP.NET Core 中的已驗證加密詳細資料</span><span class="sxs-lookup"><span data-stu-id="7ed8d-103">Authenticated encryption details in ASP.NET Core</span></span>

<a name="data-protection-implementation-authenticated-encryption-details"></a>

<span data-ttu-id="7ed8d-104">對 >idataprotector 的呼叫會受到驗證的加密作業。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-104">Calls to IDataProtector.Protect are authenticated encryption operations.</span></span> <span data-ttu-id="7ed8d-105">保護方法提供機密性和真實性，並系結至用來從根 IDataProtectionProvider 衍生這個特定 >idataprotector 實例的目的鏈。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-105">The Protect method offers both confidentiality and authenticity, and it's tied to the purpose chain that was used to derive this particular IDataProtector instance from its root IDataProtectionProvider.</span></span>

<span data-ttu-id="7ed8d-106">>idataprotector 會採用 byte [] 純文字參數，並產生 byte [] 受保護的裝載，其格式如下所述。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-106">IDataProtector.Protect takes a byte[] plaintext parameter and produces a byte[] protected payload, whose format is described below.</span></span> <span data-ttu-id="7ed8d-107"> (還有一個擴充方法多載，它會採用字串純文字參數，並傳回字串受保護的裝載。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-107">(There's also an extension method overload which takes a string plaintext parameter and returns a string protected payload.</span></span> <span data-ttu-id="7ed8d-108">如果使用此 API，則受保護的裝載格式仍會有下列結構，但會以 [base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。 ) </span><span class="sxs-lookup"><span data-stu-id="7ed8d-108">If this API is used the protected payload format will still have the below structure, but it will be [base64url-encoded](https://tools.ietf.org/html/rfc4648#section-5).)</span></span>

## <a name="protected-payload-format"></a><span data-ttu-id="7ed8d-109">受保護的裝載格式</span><span class="sxs-lookup"><span data-stu-id="7ed8d-109">Protected payload format</span></span>

<span data-ttu-id="7ed8d-110">受保護的裝載格式包含三個主要元件：</span><span class="sxs-lookup"><span data-stu-id="7ed8d-110">The protected payload format consists of three primary components:</span></span>

* <span data-ttu-id="7ed8d-111">識別資料保護系統版本的32位魔術標頭。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-111">A 32-bit magic header that identifies the version of the data protection system.</span></span>

* <span data-ttu-id="7ed8d-112">一種128位金鑰識別碼，可識別用來保護此特定裝載的金鑰。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-112">A 128-bit key id that identifies the key used to protect this particular payload.</span></span>

* <span data-ttu-id="7ed8d-113">受保護承載的其餘部分是 [此金鑰所封裝的加密程式專用](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)的。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-113">The remainder of the protected payload is [specific to the encryptor encapsulated by this key](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation).</span></span> <span data-ttu-id="7ed8d-114">在下列範例中，金鑰代表 AES-256-CBC + HMACSHA256 加密程式，而承載會進一步細分，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7ed8d-114">In the example below, the key represents an AES-256-CBC + HMACSHA256 encryptor, and the payload is further subdivided as follows:</span></span>
  * <span data-ttu-id="7ed8d-115">128位索引鍵修飾詞。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-115">A 128-bit key modifier.</span></span>
  * <span data-ttu-id="7ed8d-116">128位的初始化向量。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-116">A 128-bit initialization vector.</span></span>
  * <span data-ttu-id="7ed8d-117">48個位元組的 AES-256-CBC 輸出。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-117">48 bytes of AES-256-CBC output.</span></span>
  * <span data-ttu-id="7ed8d-118">HMACSHA256 authentication 標記。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-118">An HMACSHA256 authentication tag.</span></span>

<span data-ttu-id="7ed8d-119">範例受保護的承載如下所示。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-119">A sample protected payload is illustrated below.</span></span>

```
09 F0 C9 F0 80 9C 81 0C 19 66 19 40 95 36 53 F8
AA FF EE 57 57 2F 40 4C 3F 7F CC 9D CC D9 32 3E
84 17 99 16 EC BA 1F 4A A1 18 45 1F 2D 13 7A 28
79 6B 86 9C F8 B7 84 F9 26 31 FC B1 86 0A F1 56
61 CF 14 58 D3 51 6F CF 36 50 85 82 08 2D 3F 73
5F B0 AD 9E 1A B2 AE 13 57 90 C8 F5 7C 95 4E 6A
8A AA 06 EF 43 CA 19 62 84 7C 11 B2 C8 71 9D AA
52 19 2E 5B 4C 1E 54 F0 55 BE 88 92 12 C1 4B 5E
52 C9 74 A0
```

<span data-ttu-id="7ed8d-120">從高於前32位的裝載格式，或4個位元組是識別版本 (09 F0 C9 F0 的魔術標頭) </span><span class="sxs-lookup"><span data-stu-id="7ed8d-120">From the payload format above the first 32 bits, or 4 bytes are the magic header identifying the version (09 F0 C9 F0)</span></span>

<span data-ttu-id="7ed8d-121">接下來的128位或16個位元組是金鑰識別碼 (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57) </span><span class="sxs-lookup"><span data-stu-id="7ed8d-121">The next 128 bits, or 16 bytes is the key identifier (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57)</span></span>

<span data-ttu-id="7ed8d-122">其餘部分包含裝載，而且是所使用的格式所特有的。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-122">The remainder contains the payload and is specific to the format used.</span></span>

> [!WARNING]
> <span data-ttu-id="7ed8d-123">所有受指定金鑰保護的承載，都將以相同的20位元組 (魔術值、金鑰識別碼) 標頭為開頭。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-123">All payloads protected to a given key will begin with the same 20-byte (magic value, key id) header.</span></span> <span data-ttu-id="7ed8d-124">系統管理員可以使用這種事實來進行診斷，以估計產生的承載。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-124">Administrators can use this fact for diagnostic purposes to approximate when a payload was generated.</span></span> <span data-ttu-id="7ed8d-125">例如，上述承載對應到 key {0c819c80-6619-4019-9536-53f8aaffee57}。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-125">For example, the payload above corresponds to key {0c819c80-6619-4019-9536-53f8aaffee57}.</span></span> <span data-ttu-id="7ed8d-126">如果檢查金鑰存放庫之後，您發現此特定金鑰的啟用日期為2015-01-01，且到期日為2015-03-01，則合理假設 (如果未在該時段內產生) ，請在任一端提供或採取小巧克力因素。</span><span class="sxs-lookup"><span data-stu-id="7ed8d-126">If after checking the key repository you find that this specific key's activation date was 2015-01-01 and its expiration date was 2015-03-01, then it's reasonable to assume that the payload (if not tampered with) was generated within that window, give or take a small fudge factor on either side.</span></span>
