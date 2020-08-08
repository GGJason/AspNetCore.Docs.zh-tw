---
title: 保護 ASP.NET Core Blazor Server 應用程式
author: guardrex
description: 瞭解如何以 Blazor Server ASP.NET Core 的應用程式來保護應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2020
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
uid: blazor/security/server/index
ms.openlocfilehash: 4dc9040b9410304eb33e5df7c47db2f9a42152d3
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88013992"
---
# <a name="secure-aspnet-core-no-locblazor-server-apps"></a>保護 ASP.NET Core Blazor Server 應用程式

作者：[Luke Latham](https://github.com/guardrex)

Blazor Server應用程式的安全性設定方式與 ASP.NET Core 應用程式相同。 如需詳細資訊，請參閱底下的文章 <xref:security/index> 。 此總覽下的主題特別適用于 Blazor Server 。 

## <a name="no-locblazor-server-project-template"></a>Blazor Server專案範本

Blazor Server專案範本可以設定為在建立專案時進行驗證。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

遵循中的 Visual Studio 指導方針 <xref:blazor/tooling> ，使用驗證機制來建立新的 Blazor Server 專案。

選擇 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中的** Blazor Server 應用程式**範本之後，請選取 [**驗證**] 底下的 [**變更**]。

對話方塊隨即開啟，並提供可供其他 ASP.NET Core 專案使用的相同驗證機制集合：

* **無驗證**
* **個別使用者帳戶**：可以儲存使用者帳戶：
  * 在應用程式中使用 ASP.NET Core 的 [Identity](xref:security/authentication/identity) 系統。
  * 使用 [Azure AD B2C](xref:security/authentication/azure-ad-b2c) 儲存。
* **工作或學校帳戶**
* **Windows 驗證**

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

遵循中的 Visual Studio Code 指導方針， <xref:blazor/tooling> 使用驗證機制來建立新的 Blazor Server 專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制 | 描述 |
| ------------------------ | ----------- |
| `None` (預設值)         | 不需要驗證 |
| `Individual`             | 使用 ASP.NET Core 儲存在應用程式中的使用者Identity |
| `IndividualB2C`          | 儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者 |
| `SingleOrg`              | 單一租使用者的組織驗證 |
| `MultiOrg`               | 多個租使用者的組織驗證 |
| `Windows`                | Windows 驗證 |

使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：

* 建立專案的資料夾。
* 命名專案。

如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

1. 遵循中的 Visual Studio for Mac 指導方針 <xref:blazor/tooling> 。

1. 在 [**設定新的 Blazor Server 應用程式**] 步驟中，從 [**驗證**] 下拉式下選取 [**個別驗證] ([應用程式內) ** ]。

1. 應用程式會針對以 ASP.NET Core 儲存在應用程式中的個別使用者而建立 Identity 。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

Blazor Server在命令 shell 中使用下列命令，以驗證機制建立新的專案：

```dotnetcli
dotnet new blazorserver -o {APP NAME} -au {AUTHENTICATION}
```

下表顯示允許的驗證值 (`{AUTHENTICATION}`)。

| 驗證機制 | 描述 |
| ------------------------ | ----------- |
| `None` (預設值)         | 不需要驗證 |
| `Individual`             | 使用 ASP.NET Core 儲存在應用程式中的使用者Identity |
| `IndividualB2C`          | 儲存在[Azure AD B2C](xref:security/authentication/azure-ad-b2c)中的使用者 |
| `SingleOrg`              | 單一租使用者的組織驗證 |
| `MultiOrg`               | 多個租使用者的組織驗證 |
| `Windows`                | Windows 驗證 |

使用 `-o|--output` 選項時，此命令會使用提供給預留位置的值 `{APP NAME}` 來執行下列動作：

* 建立專案的資料夾。
* 命名專案。

如需詳細資訊，請參閱 [`dotnet new`](/dotnet/core/tools/dotnet-new) .Net Core 指南中的命令。

---

## <a name="scaffold-no-locidentity"></a>ScaffoldIdentity

Scaffold Identity 至 Blazor Server 專案：

* [沒有現有的授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-without-existing-authorization)。
* [具有授權](xref:security/authentication/scaffold-identity#scaffold-identity-into-a-blazor-server-project-with-authorization)。
