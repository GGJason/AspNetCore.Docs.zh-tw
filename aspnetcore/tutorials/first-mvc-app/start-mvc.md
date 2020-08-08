---
title: ASP.NET Core MVC 使用者入門
author: rick-anderson
description: 了解如何開始使用 ASP.NET Core MVC。
ms.author: riande
ms.date: 10/16/2019
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
uid: tutorials/first-mvc-app/start-mvc
ms.openlocfilehash: 3df00af8e6bca0dbf2d7871f383dd67b465aa5da
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88021168"
---
# <a name="get-started-with-aspnet-core-mvc"></a>ASP.NET Core MVC 使用者入門

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。

此應用程式會管理電影標題的資料庫。 您會了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式。
> * 新增並建構模型。
> * 使用資料庫。
> * 新增搜尋和驗證。

結束時，您將會有一個可管理及顯示電影資料的應用程式。

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-app"></a>建立 Web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 中，選取 [建立新專案]****。

* 依序選取 [ASP.NET Core Web 應用程式]**** 和 [下一步]****。

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* 將專案命名為 **MvcMovie**，然後選取 [建立]****。 請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)

* 選取 [Web 應用程式 (Model-View-Controller)]****，然後選取 [建立]****。

![[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web ](start-mvc/_static/new_project30.png)

Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。 您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。 這是基本的入門專案。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

本教學課程假設您熟悉 VS Code。 如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
* 將目錄 (`cd`) 變更為其中包含專案的資料夾。
* 執行下列命令：

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * 此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**  選取 [是]

  * `dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。
  * `code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 選取 [檔案]** [新增解決方案]** > ****。

  ![macOS 新增方案](start-mvc/_static/new_project_vsmac.png)

* 在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式] ([模型-視圖) 控制器**  >  **] [下一步]**。 在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式] [web 應用程式] ([模型-視圖控制器) **  >  **下一步]**

  ![macOS web 應用程式範本選取專案](start-mvc/_static/web_app_template_vsmac.png)

* 在 [**設定您的新 Web 應用程式**] 對話方塊中：

  * 確認 [**驗證**] 已設為 [**無驗證**]。
  * 如果提供選取**目標 Framework**的選項，請選取最新的3.x 版。

  選取 [下一步]  。

* 將專案命名為 **MvcMovie**，然後選取 [建立]****。

  ![macOS 將專案命名為](start-mvc/_static/MvcMovie.png)

---

### <a name="run-the-app"></a>執行應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

選取 **Ctrl-F5** 以非偵錯模式執行應用程式。

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。 請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。
* 使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員偏好使用非偵錯模式來快速啟動應用程式並檢視變更。
* 您可以從 [偵錯]**** 功能表項目的偵錯或非偵錯模式中啟動應用程式：

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* 您可以選取 [IIS Express]**** 按鈕偵錯應用程式

  ![IIS Express](start-mvc/_static/iis_express.png)

  下圖顯示應用程式：

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

按 Ctrl+F5 即可執行而不使用偵錯工具。

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。 位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。

  使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。 Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。

[!INCLUDE[](~/includes/trustCertMac.md)]

* 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。 當您執行應用程式時，會看到不同的連接埠編號。
* 您可以從 [執行]**** 功能表的偵錯或非偵錯模式中啟動應用程式。

  下圖顯示應用程式：

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。

> [!div class="step-by-step"]
> [下一個](adding-controller.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE [consider RP](~/includes/razor.md)]

本教學課程將教導您建置 ASP.NET Core MVC Web 應用程式的基本概念。

此應用程式會管理電影標題的資料庫。 您會了解如何：

> [!div class="checklist"]
> * 建立 Web 應用程式。
> * 新增並建構模型。
> * 使用資料庫。
> * 新增搜尋和驗證。

結束時，您將會有一個可管理及顯示電影資料的應用程式。

[!INCLUDE[](~/includes/mvc-intro/download.md)]

## <a name="prerequisites"></a>必要條件

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---
## <a name="create-a-web-app"></a>建立 Web 應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 中，選取 [建立新專案]****。

* 依序選取 [ASP.NET Core Web 應用程式]**** 和 [下一步]****。

![新增 ASP.NET Core Web 應用程式](start-mvc/_static/np_2.1.png)

* 將專案命名為 **MvcMovie**，然後選取 [建立]****。 請務必將專案命名為 **MvcMovie**，以便在複製程式碼時，命名空間得以相符。

  ![新增 ASP.NET Core Web 應用程式](start-mvc/_static/config.png)


* 選取 [Web 應用程式 (Model-View-Controller)]****，然後選取 [建立]****。

![[新增專案] 對話方塊、左窗格中的 [.Net Core]、ASP.NET Core Web ](start-mvc/_static/new_project22-21.png)

Visual Studio 在您剛才建立的 MVC 專案中使用了預設範本。 您只要輸入專案名稱，然後選取幾個選項，就立刻會有工作中的應用程式。 這是基本的入門專案，讓我們從這裡開始吧。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

本教學課程假設您熟悉 VS Code。 如需詳細資訊，請參閱 [VS Code 使用者入門](https://code.visualstudio.com/docs)和 [Visual Studio Code 說明](#visual-studio-code-help)。

* 開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。
* 將目錄 (`cd`) 變更為其中包含專案的資料夾。
* 執行下列命令：

   ```dotnetcli
   dotnet new mvc -o MvcMovie
   code -r MvcMovie
   ```

  * 此時會出現一個對話方塊，其中包含**組建所需的資產，且 ' MvcMovie ' 中遺漏了 debug。要新增它們嗎？**  選取 [是]

  * `dotnet new mvc -o MvcMovie`：在 *MvcMovie* 資料夾中建立新的 ASP.NET Core MVC 專案。
  * `code -r MvcMovie`：載入 Visual Studio Code 中的*MvcMovie*專案檔案。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* 選取 [檔案]** [新增解決方案]** > ****。

  ![macOS 新增方案](./start-mvc/_static/new_project_vsmac.png)

* 在8.6 版之前的 Visual Studio for Mac 中，選取 [ **.net Core**  >  **應用**  >  **程式 Web 應用程式] ([模型-視圖) 控制器**  >  **] [下一步]**。 在8.6 版或更新版本中，選取 [ **web 和主控台**  >  **應用**  >  **程式] [web 應用程式] ([模型-視圖控制器) **  >  **下一步]**

* 在 [**設定您的新 Web 應用程式**] 對話方塊中：

  * 確認 [**驗證**] 已設為 [**無驗證**]。
  * 如果提供選取**目標 Framework**的選項，請選取最新的2.x 版。

  選取 [下一步]  。

* 將專案命名為 **MvcMovie**，然後選取 [建立]****。

---

### <a name="run-the-app"></a>執行應用程式

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

選取 **Ctrl-F5** 以非偵錯模式執行應用程式。

[!INCLUDE[](~/includes/trustCertVS.md)]

* Visual Studio 會啟動 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)，並執行應用程式。 請注意，位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。
* 使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員偏好使用非偵錯模式來快速啟動應用程式並檢視變更。
* 您可以從 [偵錯]**** 功能表項目的偵錯或非偵錯模式中啟動應用程式：

  ![[偵錯] 功能表](start-mvc/_static/debug_menu.png)

* 您可以選取 [IIS Express]**** 按鈕偵錯應用程式

  ![IIS Express](start-mvc/_static/iis_express.png)

* 選取 [接受] 以同意追蹤。 此應用程式不會追蹤個人資訊。 範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  下圖顯示接受追蹤之後的應用程式：

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

按 Ctrl+F5 即可執行而不使用偵錯工具。

[!INCLUDE[](~/includes/trustCertVSC.md)]

  Visual Studio Code 會啟動 [Kestrel](xref:fundamentals/servers/kestrel)、啟動瀏覽器，然後瀏覽至 `https://localhost:5001`。 位址列會顯示 `localhost:port:5001`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 Localhost 只會為來自本機電腦的 Web 要求提供服務。

  使用 Ctrl + F5 (非偵錯模式) 啟動應用程式，可讓您變更程式碼、儲存檔案、重新整理瀏覽器，以及查看程式碼變更。 許多開發人員想要使用非偵錯模式來重新整理頁面並檢視變更。

* 選取 [接受] 以同意追蹤。 此應用程式不會追蹤個人資訊。 範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](start-mvc/_static/privacy.png)

  下圖顯示接受追蹤之後的應用程式：

  ![Home 或 Index 頁面](start-mvc/_static/home2.2.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

選取 [**執行**]  >  [**啟動但不**進行偵測] 來啟動應用程式。 Visual Studio for Mac 會啟動 [Kestrel](xref:fundamentals/servers/index#kestrel) 伺服器、啟動瀏覽器，然後巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的連接埠號碼。

[!INCLUDE[](~/includes/trustCertMac.md)]

* 位址列會顯示 `localhost:port#`，而不是類似於 `example.com` 的內容。 這是因為 `localhost` 是本機電腦的標準主機名稱。 當 Visual Studio 建立 Web 專案時，會為網頁伺服器使用隨機連接埠。 當您執行應用程式時，會看到不同的連接埠編號。
* 您可以從 [執行]**** 功能表的偵錯或非偵錯模式中啟動應用程式。

* 選取 [接受] 以同意追蹤。 此應用程式不會追蹤個人資訊。 範本產生之程式碼所包含的資產有利於滿足[一般資料保護規定 (GDPR)](xref:security/gdpr)。

  ![Home 或 Index 頁面](./start-mvc/_static/output_privacy_macos.png)

  下圖顯示接受追蹤之後的應用程式：

  ![Home 或 Index 頁面](./start-mvc/_static/output_macos.png)

---

[!INCLUDE[](~/includes/vs-vsc-vsmac-help.md)]

在本教學課程的下一個部分中，您會了解 MVC，並開始撰寫一些程式碼。

> [!div class="step-by-step"]
> [下一個](adding-controller.md)

::: moniker-end
