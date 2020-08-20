---
title: ASP.NET Core 中的已驗證加密詳細資料
author: rick-anderson
description: 瞭解 ASP.NET Core 資料保護驗證加密的執行詳細資料。
ms.author: riande
ms.date: 10/14/2016
no-loc:
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
uid: security/data-protection/implementation/authenticated-encryption-details
ms.openlocfilehash: ed75ab235a95a88bbe60615526137b4c2bb719ef
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88630844"
---
# <a name="authenticated-encryption-details-in-aspnet-core"></a>ASP.NET Core 中的已驗證加密詳細資料

<a name="data-protection-implementation-authenticated-encryption-details"></a>

對 >idataprotector 的呼叫會受到驗證的加密作業。 保護方法提供機密性和真實性，並系結至用來從根 IDataProtectionProvider 衍生這個特定 >idataprotector 實例的目的鏈。

>idataprotector 會採用 byte [] 純文字參數，並產生 byte [] 受保護的裝載，其格式如下所述。  (還有一個擴充方法多載，它會採用字串純文字參數，並傳回字串受保護的裝載。 如果使用此 API，則受保護的裝載格式仍會有下列結構，但會以 [base64url 編碼](https://tools.ietf.org/html/rfc4648#section-5)。 ) 

## <a name="protected-payload-format"></a>受保護的裝載格式

受保護的裝載格式包含三個主要元件：

* 識別資料保護系統版本的32位魔術標頭。

* 一種128位金鑰識別碼，可識別用來保護此特定裝載的金鑰。

* 受保護承載的其餘部分是 [此金鑰所封裝的加密程式專用](xref:security/data-protection/implementation/subkeyderivation#data-protection-implementation-subkey-derivation)的。 在下列範例中，金鑰代表 AES-256-CBC + HMACSHA256 加密程式，而承載會進一步細分，如下所示：
  * 128位索引鍵修飾詞。
  * 128位的初始化向量。
  * 48個位元組的 AES-256-CBC 輸出。
  * HMACSHA256 authentication 標記。

範例受保護的承載如下所示。

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

從高於前32位的裝載格式，或4個位元組是識別版本 (09 F0 C9 F0 的魔術標頭) 

接下來的128位或16個位元組是金鑰識別碼 (80 9C 81 0C 19 66 19 40 95 36 53 F8 AA FF EE 57) 

其餘部分包含裝載，而且是所使用的格式所特有的。

> [!WARNING]
> 所有受指定金鑰保護的承載，都將以相同的20位元組 (魔術值、金鑰識別碼) 標頭為開頭。 系統管理員可以使用這種事實來進行診斷，以估計產生的承載。 例如，上述承載對應到 key {0c819c80-6619-4019-9536-53f8aaffee57}。 如果檢查金鑰存放庫之後，您發現此特定金鑰的啟用日期為2015-01-01，且到期日為2015-03-01，則合理假設 (如果未在該時段內產生) ，請在任一端提供或採取小巧克力因素。
