---
title: Versioning gRPC 服務
author: jamesnk
description: 瞭解如何將 gRPC 服務的版本。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 01/09/2020
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
uid: grpc/versioning
ms.openlocfilehash: 38204b16d041f21221862c566b90a6a9571d26a1
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93058697"
---
# <a name="versioning-grpc-services"></a>Versioning gRPC 服務

依 [James 牛頓](https://twitter.com/jamesnk)

新增至應用程式的新功能可能需要提供給用戶端的 gRPC 服務變更，有時會以非預期和中斷的方式進行變更。 當 gRPC services 變更時：

* 您應在變更影響用戶端的方式方面提供考慮。
* 您應執行支援變更的版本控制策略。

## <a name="backwards-compatibility"></a>回溯相容性

GRPC 通訊協定是設計來支援隨時間變化的服務。 一般而言，gRPC 服務和方法的新增專案不會中斷。 非中斷性變更可讓現有的用戶端繼續運作，而不需要變更。 變更或刪除 gRPC 服務是重大變更。 當 gRPC 服務有突破性的變更時，必須更新並重新部署使用該服務的用戶端。

對服務進行非中斷性變更有許多優點：

* 現有的用戶端會繼續執行。
* 避免與通知用戶端中斷的變更及更新它們相關的工作。
* 只有一個服務版本需要記錄和維護。

### <a name="non-breaking-changes"></a>非中斷性變更

這些變更在 gRPC 通訊協定層級和 .NET 二進位層級不會中斷。

* **新增服務**
* **將新方法加入至服務**
* **將欄位加入至要求訊息** -新增至要求訊息的欄位會在未設定時，以伺服器上的 [預設值](https://developers.google.com/protocol-buffers/docs/proto3#default) 還原序列化。 若要成為非重大變更，當舊的用戶端未設定新欄位時，服務必須成功。
* **將欄位加入至回應** 消息-新增至回應訊息的欄位會還原序列化至用戶端上訊息的 [未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns) 集合。
* **將值新增至列舉列舉** 會序列化為數值。 新的列舉值會在用戶端上還原序列化為沒有列舉名稱的列舉值。 若要成為非重大變更，舊版用戶端必須在接收新的列舉值時正確地執行。

### <a name="binary-breaking-changes"></a>二進位中斷性變更

下列變更在 gRPC 通訊協定層級不會中斷，但如果用戶端升級至最新的 *proto* 合約或用戶端 .net 元件，則需要更新用戶端。 如果您打算將 gRPC 程式庫發佈至 NuGet，二進位相容性很重要。

* 從已移除的欄位 **移除欄位** 值會還原序列化為訊息的 [未知欄位](https://developers.google.com/protocol-buffers/docs/proto3#unknowns)。 這不是 gRPC 的通訊協定重大變更，但如果用戶端升級至最新的合約，則需要更新用戶端。 很重要的是，在未來不會意外重複使用移除的欄位編號。 為確保不會發生這種情況，請使用 Protobuf 的 [reserved](https://developers.google.com/protocol-buffers/docs/proto3#reserved) 關鍵字來指定訊息上已刪除的欄位編號和名稱。
* 重新 **命名訊息** 訊息名稱通常不會在網路上傳送，因此這不是 gRPC 的通訊協定中斷變更。 如果用戶端升級至最新的合約，就必須更新用戶端。 當訊息名稱用來識別訊息類型時，網路 **上傳送訊息名稱的** 其中一種情況是使用 [任何](https://developers.google.com/protocol-buffers/docs/proto3#any) 欄位。
* 變更 **csharp_namespace** 變更 `csharp_namespace` 將會變更所產生 .net 類型的命名空間。 這不是 gRPC 的通訊協定重大變更，但如果用戶端升級至最新的合約，則需要更新用戶端。

### <a name="protocol-breaking-changes"></a>通訊協定的重大變更

下列專案是通訊協定和二進位中斷性變更：

* 重新 **命名欄位** -使用 Protobuf 內容時，功能變數名稱只會在產生的程式碼中使用。 欄位號用來識別網路上的欄位。 重新命名欄位不是 Protobuf 的通訊協定重大變更。 但是，如果伺服器使用 JSON 內容，則重新命名欄位是一項重大變更。
* **變更欄位資料類型** -將欄位的資料類型變更為 [不相容的類型](https://developers.google.com/protocol-buffers/docs/proto3#updating) 時，將會在還原序列化訊息時發生錯誤。 即使新的資料類型相容，如果用戶端升級至最新的合約，則可能需要更新用戶端，以支援新的類型。
* **變更欄位編號** -使用 Protobuf 承載時，會使用欄位編號來識別網路上的欄位。
* 重新 **命名封裝、服務或方法** gRPC 時，會使用封裝名稱、服務名稱和方法名稱來建立 URL。 用戶端會從伺服器取得未 *實現* 的狀態。
* **移除服務或方法** -當呼叫已移除的方法時，用戶端會從伺服器 *取得未完成的狀態* 。

### <a name="behavior-breaking-changes"></a>行為重大變更

進行非中斷的變更時，您也必須考慮較舊的用戶端是否可以繼續使用新的服務行為。 例如，將新欄位加入至要求訊息：

* 不是通訊協定的重大變更。
* 如果未設定新欄位，則傳回伺服器上的錯誤狀態，使其成為舊用戶端的重大變更。

行為相容性取決於您的應用程式專屬程式碼。

## <a name="version-number-services"></a>版本號碼服務

服務應該致力於維持與舊版用戶端的回溯相容性。 最後，您的應用程式變更可能需要中斷變更。 中斷舊用戶端，並強制這些用戶端與您的服務一起更新，並不是很好的使用者體驗。 在進行重大變更時維持回溯相容性的方法是發佈多個版本的服務。

gRPC 支援選擇性的 [封裝](https://developers.google.com/protocol-buffers/docs/proto3#packages) 規範，其功能與 .net 命名空間很類似。 事實上， `package` 如果 `option csharp_namespace` 未在 *proto* 檔案中設定，則會使用做為所產生 .net 類型的 .net 命名空間。 封裝可以用來指定您的服務和其訊息的版本號碼：

[!code-protobuf[](versioning/sample/greet.v1.proto?highlight=3)]

封裝名稱會與服務名稱結合以識別服務位址。 服務位址可讓多個版本的服務並行託管：

* `greet.v1.Greeter`
* `greet.v2.Greeter`

已建立版本之服務的實 *Startup.cs* ：

```csharp
app.UseEndpoints(endpoints =>
{
    // Implements greet.v1.Greeter
    endpoints.MapGrpcService<GreeterServiceV1>();

    // Implements greet.v2.Greeter
    endpoints.MapGrpcService<GreeterServiceV2>();
});
```

在套件名稱中包含版本號碼，讓您有機會發行服務的 *v2* 版本與中斷的變更，同時繼續支援呼叫 *v1* 版本的舊版用戶端。 用戶端更新為使用 *v2* 服務之後，您可以選擇移除舊版本。 規劃發行多個版本的服務時：

* 如果合理，請避免中斷的變更。
* 除非進行重大變更，否則請勿更新版本號碼。
* 當您進行重大變更時，請更新版本號碼。

發行多個版本的服務會複製它。 若要減少重複的情況，請考慮將商務邏輯從服務的執行移至可由舊的和新的執行重複使用的集中位置：

[!code-csharp[](versioning/sample/GreeterServiceV1.cs?highlight=10,19)]

使用不同封裝名稱產生的服務和訊息是 **不同的 .net 類型** 。 將商務邏輯移至中央位置需要將訊息對應至一般類型。

## <a name="additional-resources"></a>其他資源

* <xref:grpc/protobuf>
