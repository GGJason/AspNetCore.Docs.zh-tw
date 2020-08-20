---
title: 使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定
author: rick-anderson
description: 此範例示範如何將 Microsoft 帳戶使用者驗證整合至現有的 ASP.NET Core 應用程式。
ms.author: riande
ms.custom: mvc
ms.date: 03/19/2020
monikerRange: '>= aspnetcore-3.0'
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
uid: security/authentication/microsoft-logins
ms.openlocfilehash: 3430d842b6a4f7da30370977f72e6f132e28bb7f
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88634250"
---
# <a name="microsoft-account-external-login-setup-with-aspnet-core"></a>使用 ASP.NET Core 的 Microsoft 帳戶外部登入設定

作者：[Valeriy Novytskyy](https://github.com/01binary) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

這個範例會示範如何讓使用者使用在 [前一頁](xref:security/authentication/social/index)建立的 ASP.NET Core 3.0 專案來登入他們的工作、學校或個人 Microsoft 帳戶。

## <a name="create-the-app-in-microsoft-developer-portal"></a>在 Microsoft 開發人員入口網站中建立應用程式

* 將 [AspNetCore MicrosoftAccount](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.MicrosoftAccount/) NuGet 套件新增至專案。
* 流覽至 [ [Azure 入口網站應用程式註冊](https://go.microsoft.com/fwlink/?linkid=2083908) ] 頁面，並建立或登入 Microsoft 帳戶：

如果您沒有 Microsoft 帳戶，請選取 [ **建立一個**]。 登入之後，系統會將您重新導向至 **應用程式註冊** 頁面：

* 選取 **新的註冊**
* 輸入 [名稱]。
* 選取 **支援的帳戶類型**的選項。  <!-- Accounts for any org work with MS domain accounts. Most folks probably want the last option, personal MS accounts. It took 24 hours after setting this up for the keys to work -->
* 在 [重新 **導向 URI**] 下，輸入您附加的開發 URL `/signin-microsoft` 。 例如： `https://localhost:5001/signin-microsoft` 。 本範例稍後設定的 Microsoft 驗證配置會自動處理在 `/signin-microsoft` 路由執行的要求，以實行 OAuth 流程。
* 選取 [註冊]

### <a name="create-client-secret"></a>建立用戶端密碼

* 在左側窗格中，選取 [憑證及祕密]。
* 在 [**用戶端密碼**] 下，選取 [**新增用戶端密碼**]

  * 新增用戶端密碼的描述。
  * 選取 [新增] 按鈕。

* 在 [ **用戶端密碼**] 下，複製用戶端密碼的值。

URI 區段 `/signin-microsoft` 會設定為 Microsoft 驗證提供者的預設回呼。 您可以在設定 Microsoft 驗證中介軟體時，透過[MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.authentication.microsoftaccount.microsoftaccountoptions)類別的繼承[RemoteAuthenticationOptions. CallbackPath](/dotnet/api/microsoft.aspnetcore.authentication.remoteauthenticationoptions.callbackpath)屬性來變更預設回呼 URI。

## <a name="store-the-microsoft-client-id-and-secret"></a>儲存 Microsoft 用戶端識別碼和密碼

使用 [秘密管理員](xref:security/app-secrets)來儲存機密設定，例如 Microsoft 用戶端識別碼和秘密值。 針對此範例，請使用下列步驟：

1. 根據 [啟用秘密儲存](xref:security/app-secrets#enable-secret-storage)的指示，為秘密儲存體初始化專案。
1. 將機密設定儲存在具有秘密金鑰和的本機秘密存放區中 `Authentication:Microsoft:ClientId` `Authentication:Microsoft:ClientSecret` ：

    ```dotnetcli
    dotnet user-secrets set "Authentication:Microsoft:ClientId" "<client-id>"
    dotnet user-secrets set "Authentication:Microsoft:ClientSecret" "<client-secret>"
    ```

[!INCLUDE[](~/includes/environmentVarableColon.md)]

## <a name="configure-microsoft-account-authentication"></a>設定 Microsoft 帳戶驗證

將 Microsoft 帳戶服務新增至 `Startup.ConfigureServices` ：

[!code-csharp[](~/security/authentication/social/social-code/3.x/StartupMS3x.cs?name=snippet&highlight=10-14)]

[!INCLUDE [default settings configuration](includes/default-settings.md)]

如需有關 Microsoft 帳戶驗證所支援之設定選項的詳細資訊，請參閱 [MicrosoftAccountOptions](/dotnet/api/microsoft.aspnetcore.builder.microsoftaccountoptions) API 參考。 這可以用來要求使用者的不同資訊。

## <a name="sign-in-with-microsoft-account"></a>使用 Microsoft 帳戶登入帳戶

執行應用程式，然後按一下 [ **登入**]。 使用 Microsoft 登入的選項隨即出現。 當您按一下 [Microsoft] 時，系統會將您重新導向至 Microsoft 進行驗證。 使用您的 Microsoft 帳戶登入之後，系統會提示您讓應用程式存取您的資訊：

請按一下 **[是]** ，系統會將您重新導向回到可設定電子郵件的網站。

您現在已使用您的 Microsoft 認證登入：

[!INCLUDE[](includes/chain-auth-providers.md)]

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="troubleshooting"></a>疑難排解

* 如果 Microsoft 帳戶提供者將您重新導向至 [登入錯誤] 頁面，請注意 Uri 中的 (主題標籤) 的錯誤標題和描述查詢字串參數 `#` 。

  雖然錯誤訊息似乎指出 Microsoft 驗證有問題，但最常見的原因是您的應用程式 Uri 不符合為**Web**平臺指定的任何重新**導向 uri** 。
* 如果 Identity 未透過呼叫來 `services.AddIdentity` 設定 `ConfigureServices` ，嘗試驗證將會導致 *ArgumentException：必須提供 ' SignInScheme ' 選項*。 此範例中使用的專案範本可確保完成此操作。
* 如果未藉由套用初始遷移來建立網站資料庫，則在處理要求錯誤時，您將會收到 *資料庫操作失敗* 。 請按一下 [套用 **遷移** ] 來建立資料庫，並重新整理以繼續發生錯誤。

## <a name="next-steps"></a>後續步驟

* 本文說明您可以如何向 Microsoft 進行驗證。 您可以遵循類似的方法，向 [先前頁面](xref:security/authentication/social/index)中所列的其他提供者進行驗證。

* 當您將網站發佈至 Azure web 應用程式之後，請在 Microsoft 開發人員入口網站中建立新的用戶端密碼。

* `Authentication:Microsoft:ClientId`在 Azure 入口網站中，將和設定 `Authentication:Microsoft:ClientSecret` 為應用程式設定。 設定系統已設定為從環境變數讀取金鑰。
