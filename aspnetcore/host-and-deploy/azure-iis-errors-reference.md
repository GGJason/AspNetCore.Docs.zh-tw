---
title: Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考
author: rick-anderson
description: 取得在 Azure Apps Service 與 IIS 上裝載 ASP.NET Core 應用程式時的常見錯誤疑難排解建議。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
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
uid: host-and-deploy/azure-iis-errors-reference
ms.openlocfilehash: 5ec40181927ac491bbec473407609bafc34b78dc
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88627412"
---
# <a name="common-errors-reference-for-azure-app-service-and-iis-with-aspnet-core"></a><span data-ttu-id="b4883-103">Azure App Service 與 IIS 搭配 ASP.NET Core 時的常見錯誤參考</span><span class="sxs-lookup"><span data-stu-id="b4883-103">Common errors reference for Azure App Service and IIS with ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="b4883-104">本主題說明常見的錯誤，並針對在 Azure Apps 服務和 IIS 上裝載 ASP.NET Core 應用程式時，提供特定錯誤的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b4883-104">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="b4883-105">如需一般疑難排解指引，請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="b4883-105">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="b4883-106">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="b4883-106">Collect the following information:</span></span>

* <span data-ttu-id="b4883-107">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="b4883-107">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="b4883-108">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="b4883-108">Application Event Log entries</span></span>
  * <span data-ttu-id="b4883-109">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="b4883-109">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="b4883-110">IIS</span><span class="sxs-lookup"><span data-stu-id="b4883-110">IIS</span></span>
    1. <span data-ttu-id="b4883-111">在 [Windows]\*\*\*\* 功能表中選取 [開始]\*\*\*\*，鍵入「事件檢視器」\*\*，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="b4883-111">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="b4883-112">在 [事件檢視器]\*\*\*\* 開啟之後，展開提要欄位中的 [Windows 記錄檔]**[應用程式]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-112">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="b4883-113">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="b4883-113">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="b4883-114">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="b4883-114">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="b4883-115">IIS：遵循 ASP.NET Core 課程模組主題的 [記錄檔建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) 重新導向和 [增強的診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) 章節中的指示。</span><span class="sxs-lookup"><span data-stu-id="b4883-115">IIS: Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="b4883-116">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="b4883-116">Compare error information to the following common errors.</span></span> <span data-ttu-id="b4883-117">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="b4883-117">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="b4883-118">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="b4883-118">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="b4883-119">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]\*\*\*\* 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="b4883-119">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="b4883-120">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="b4883-120">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="b4883-121">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="b4883-121">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="b4883-122">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="b4883-122">The data is the error.</span></span>

<span data-ttu-id="b4883-123">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-123">Troubleshooting:</span></span>

<span data-ttu-id="b4883-124">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="b4883-124">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="b4883-125">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-125">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="b4883-126">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="b4883-126">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="b4883-127">請參閱 [安裝 .Net Core 裝載](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)套件組合。</span><span class="sxs-lookup"><span data-stu-id="b4883-127">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="b4883-128">執行安裝程式時，請選取 [修復]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-128">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="b4883-129">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="b4883-129">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="b4883-130">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="b4883-130">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="b4883-131">**Browser：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-131">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="b4883-132">**應用程式記錄檔：** 叫用 hostfxr 以找出原生要求處理常式失敗，但未找出任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="b4883-132">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="b4883-133">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-133">Could not find inprocess request handler.</span></span> <span data-ttu-id="b4883-134">從叫用 hostfxr 所捕獲的輸出：找不到任何相容的 framework 版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-134">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-135">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="b4883-135">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="b4883-136">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="b4883-136">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="b4883-137">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的 framework 版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-137">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-138">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="b4883-138">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

* <span data-ttu-id="b4883-139">**ASP.NET Core 模組 Debug 記錄：** 叫用 hostfxr 以找出原生要求處理常式失敗，但未找出任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="b4883-139">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="b4883-140">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="b4883-140">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="b4883-141">失敗的 HRESULT 傳回：0x8000ffff。</span><span class="sxs-lookup"><span data-stu-id="b4883-141">Failed HRESULT returned: 0x8000ffff.</span></span> <span data-ttu-id="b4883-142">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-142">Could not find inprocess request handler.</span></span> <span data-ttu-id="b4883-143">無法找到任何相容的架構版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-143">It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-144">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="b4883-144">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

<span data-ttu-id="b4883-145">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-145">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-146">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b4883-146">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="b4883-147">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="b4883-147">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="b4883-148">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="b4883-148">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="b4883-149">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="b4883-149">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="b4883-150">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-150">Restart the app.</span></span> <span data-ttu-id="b4883-151">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b4883-151">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="b4883-152">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="b4883-152">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="b4883-153">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-153">After removing the site extension, restart the app.</span></span> <span data-ttu-id="b4883-154">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b4883-154">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="b4883-155">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」\*\* 是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="b4883-155">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="b4883-156">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="b4883-156">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="b4883-157">如需詳細資訊，請參閱<xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="b4883-157">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="b4883-158">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="b4883-158">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="b4883-159">**Browser：** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-159">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="b4883-160">**應用程式記錄檔：** 實體根目錄為 ' {PATH} ' 的應用程式 '/LM/W3SVC/5/ROOT ' 遇到未預期的 managed 例外狀況，例外狀況代碼 = ' 0xe0434352 '。</span><span class="sxs-lookup"><span data-stu-id="b4883-160">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="b4883-161">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-161">Please check the stderr logs for more information.</span></span> <span data-ttu-id="b4883-162">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-162">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="b4883-163">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="b4883-163">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="b4883-164">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="b4883-164">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

* <span data-ttu-id="b4883-165">**ASP.NET Core 模組 Debug 記錄：** 失敗的 HRESULT 傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="b4883-165">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8007023e</span></span>

<span data-ttu-id="b4883-166">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="b4883-166">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="b4883-167">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b4883-167">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="b4883-168">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-168">Troubleshooting:</span></span>

<span data-ttu-id="b4883-169">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b4883-169">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="b4883-170">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]\*\*\*\*，然後將 [啟用 32 位元應用程式]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-170">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="b4883-171">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="b4883-171">Platform conflicts with RID</span></span>

* <span data-ttu-id="b4883-172">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-172">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="b4883-173">**應用程式記錄檔：** 實體根目錄為 ' C： path} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "c： \{ PATH} {ASSEMBLY}" 啟動進程。 {exe | dll} "'，ErrorCode = ' 0x80004005： ff。</span><span class="sxs-lookup"><span data-stu-id="b4883-173">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="b4883-174">**ASP.NET Core 模組 Stdout 記錄檔：** 未處理的例外狀況： System. BadImageFormatException：無法載入檔案或元件 ' {ASSEMBLY} .dll '。</span><span class="sxs-lookup"><span data-stu-id="b4883-174">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="b4883-175">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-175">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="b4883-176">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-176">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-177">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="b4883-177">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="b4883-178">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-178">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="b4883-179">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b4883-179">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="b4883-180">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="b4883-180">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="b4883-181">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b4883-181">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="b4883-182">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="b4883-182">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="b4883-183">**Browser：** ERR_CONNECTION_REFUSED **--或--** 無法連接</span><span class="sxs-lookup"><span data-stu-id="b4883-183">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="b4883-184">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-184">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-185">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-185">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="b4883-186">**ASP.NET Core 模組 Debug 記錄：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-186">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-187">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-187">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-188">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="b4883-188">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="b4883-189">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="b4883-189">Check the bindings.</span></span>

* <span data-ttu-id="b4883-190">確認 IIS 網站不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="b4883-190">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="b4883-191">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="b4883-191">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="b4883-192">**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="b4883-192">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="b4883-193">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-193">Troubleshooting:</span></span>

<span data-ttu-id="b4883-194">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="b4883-194">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="b4883-195">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b4883-195">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="b4883-196">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="b4883-196">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="b4883-197">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="b4883-197">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="b4883-198">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-198">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-199">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-199">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="b4883-200">**ASP.NET Core 模組 Debug 記錄：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-200">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-201">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-201">Troubleshooting:</span></span>

<span data-ttu-id="b4883-202">檢查 IIS 網站的 [基本設定]\*\*\*\* 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="b4883-202">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="b4883-203">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b4883-203">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="b4883-204">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="b4883-204">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="b4883-205">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="b4883-205">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="b4883-206">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="b4883-206">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="b4883-207">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-207">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-208">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-208">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="b4883-209">**ASP.NET Core 模組 Debug 記錄：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-209">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-210">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-210">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-211">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="b4883-211">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="b4883-212">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b4883-212">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="b4883-213">開啟 [程式和功能]\*\*\*\* 或 [應用程式與功能]\*\*\*\*，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="b4883-213">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="b4883-214">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="b4883-214">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="b4883-215">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="b4883-215">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="b4883-216">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="b4883-216">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="b4883-217">請確定**應用程式**集區 > **進程模型** > **Identity** 設定為\*\* Identity ApplicationPool\*\* ，或自訂身分識別具有正確的許可權，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b4883-217">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="b4883-218">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="b4883-218">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="b4883-219">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="b4883-219">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="b4883-220">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="b4883-220">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="b4883-221">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="b4883-221">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="b4883-222">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-222">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="b4883-223">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="b4883-223">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="b4883-224">**Browser：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-224">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="b4883-225">**應用程式記錄檔：** 實體根目錄為 ' C： PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "{...}" 啟動進程</span><span class="sxs-lookup"><span data-stu-id="b4883-225">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="b4883-226">'，ErrorCode = ' 0x80070002：0。</span><span class="sxs-lookup"><span data-stu-id="b4883-226">', ErrorCode = '0x80070002 : 0.</span></span> <span data-ttu-id="b4883-227">應用程式 '{PATH}' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b4883-227">Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="b4883-228">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-228">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="b4883-229">無法啟動應用程式 '/LM/W3SVC/2/ROOT'，ErrorCode '0x8007023e'。</span><span class="sxs-lookup"><span data-stu-id="b4883-229">Failed to start application '/LM/W3SVC/2/ROOT', ErrorCode '0x8007023e'.</span></span>

* <span data-ttu-id="b4883-230">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-230">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="b4883-231">**ASP.NET Core 模組 Debug 記錄：** 事件記錄檔：「應用程式 ' {PATH} ' 無法啟動。</span><span class="sxs-lookup"><span data-stu-id="b4883-231">**ASP.NET Core Module Debug Log:** Event Log: 'Application '{PATH}' wasn't able to start.</span></span> <span data-ttu-id="b4883-232">在 '{PATH}' 找不到可執行檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-232">Executable was not found at '{PATH}'.</span></span> <span data-ttu-id="b4883-233">失敗的 HRESULT 傳回：0x8007023e</span><span class="sxs-lookup"><span data-stu-id="b4883-233">Failed HRESULT returned: 0x8007023e</span></span>

<span data-ttu-id="b4883-234">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-234">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-235">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="b4883-235">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="b4883-236">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-236">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="b4883-237">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b4883-237">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="b4883-238">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="b4883-238">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="b4883-239">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="b4883-239">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="b4883-240">確認\*C:\Program Files\dotnet \\ \*存在於 [系統路徑] 設定中。</span><span class="sxs-lookup"><span data-stu-id="b4883-240">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="b4883-241">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="b4883-241">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="b4883-242">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="b4883-242">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="b4883-243">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="b4883-243">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="b4883-244">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="b4883-244">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="b4883-245">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="b4883-245">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="b4883-246">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="b4883-246">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="b4883-247">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-247">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="b4883-248">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="b4883-248">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="b4883-249">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="b4883-249">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="b4883-250">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="b4883-250">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="b4883-251">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="b4883-251">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="b4883-252">元素的引數不正確 \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="b4883-252">Incorrect arguments of \<aspNetCore> element</span></span>

* <span data-ttu-id="b4883-253">**Browser：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-253">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="b4883-254">**應用程式記錄檔：** 叫用 hostfxr 以找出原生要求處理常式失敗，但未找出任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="b4883-254">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="b4883-255">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="b4883-255">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="b4883-256">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-256">Could not find inprocess request handler.</span></span> <span data-ttu-id="b4883-257">從叫用 hostfxr 所捕獲的輸出：執行 dotnet SDK 命令的意思是什麼？</span><span class="sxs-lookup"><span data-stu-id="b4883-257">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="b4883-258">請從下列安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 無法啟動應用程式 '/LM/W3SVC/3/ROOT '，ErrorCode ' 0x8000ffff '。</span><span class="sxs-lookup"><span data-stu-id="b4883-258">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed to start application '/LM/W3SVC/3/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="b4883-259">**ASP.NET Core 模組 Stdout 記錄檔：** 您是指執行 dotnet SDK 命令嗎？</span><span class="sxs-lookup"><span data-stu-id="b4883-259">**ASP.NET Core Module stdout Log:** Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="b4883-260">請從下列位置安裝 dotnet SDK：https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span><span class="sxs-lookup"><span data-stu-id="b4883-260">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409</span></span>

* <span data-ttu-id="b4883-261">**ASP.NET Core 模組 Debug 記錄：** 叫用 hostfxr 以找出原生要求處理常式失敗，但未找出任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="b4883-261">**ASP.NET Core Module Debug Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="b4883-262">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="b4883-262">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="b4883-263">失敗的 HRESULT 傳回：0x8000ffff 找不到進程要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-263">Failed HRESULT returned: 0x8000ffff Could not find inprocess request handler.</span></span> <span data-ttu-id="b4883-264">從叫用 hostfxr 所捕獲的輸出：執行 dotnet SDK 命令的意思是什麼？</span><span class="sxs-lookup"><span data-stu-id="b4883-264">Captured output from invoking hostfxr: Did you mean to run dotnet SDK commands?</span></span> <span data-ttu-id="b4883-265">請從下列安裝 dotnet SDK： https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 失敗的 HRESULT 傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="b4883-265">Please install dotnet SDK from: https://go.microsoft.com/fwlink/?LinkID=798306&clcid=0x409 Failed HRESULT returned: 0x8000ffff</span></span>

<span data-ttu-id="b4883-266">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-266">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-267">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="b4883-267">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="b4883-268">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-268">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="b4883-269">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b4883-269">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="b4883-270">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="b4883-270">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

## <a name="missing-net-core-shared-framework"></a><span data-ttu-id="b4883-271">遺失 .NET Core 共用架構</span><span class="sxs-lookup"><span data-stu-id="b4883-271">Missing .NET Core shared framework</span></span>

* <span data-ttu-id="b4883-272">**Browser：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-272">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="b4883-273">**應用程式記錄檔：** 叫用 hostfxr 以找出原生要求處理常式失敗，但未找出任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="b4883-273">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="b4883-274">這最有可能表示應用程式設定不正確，請檢查應用程式設為目標的 Microsoft.NetCore.App 和 Microsoft.AspNetCore.App 版本，並確定已安裝在電腦上。</span><span class="sxs-lookup"><span data-stu-id="b4883-274">This most likely means the app is misconfigured, please check the versions of Microsoft.NetCore.App and Microsoft.AspNetCore.App that are targeted by the application and are installed on the machine.</span></span> <span data-ttu-id="b4883-275">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-275">Could not find inprocess request handler.</span></span> <span data-ttu-id="b4883-276">從叫用 hostfxr 所捕獲的輸出：找不到任何相容的 framework 版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-276">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-277">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="b4883-277">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

<span data-ttu-id="b4883-278">無法啟動應用程式 '/LM/W3SVC/5/ROOT'，ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="b4883-278">Failed to start application '/LM/W3SVC/5/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="b4883-279">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的 framework 版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-279">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-280">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}'。</span><span class="sxs-lookup"><span data-stu-id="b4883-280">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}' was not found.</span></span>

* <span data-ttu-id="b4883-281">**ASP.NET Core 模組 Debug 記錄：** 失敗的 HRESULT 傳回：0x8000ffff</span><span class="sxs-lookup"><span data-stu-id="b4883-281">**ASP.NET Core Module Debug Log:** Failed HRESULT returned: 0x8000ffff</span></span>

<span data-ttu-id="b4883-282">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-282">Troubleshooting:</span></span>

<span data-ttu-id="b4883-283">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="b4883-283">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="b4883-284">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="b4883-284">Stopped Application Pool</span></span>

* <span data-ttu-id="b4883-285">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="b4883-285">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="b4883-286">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-286">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-287">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-287">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="b4883-288">**ASP.NET Core 模組 Debug 記錄：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-288">**ASP.NET Core Module Debug Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-289">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-289">Troubleshooting:</span></span>

<span data-ttu-id="b4883-290">確認「應用程式集區」不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="b4883-290">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="b4883-291">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="b4883-291">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="b4883-292">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="b4883-292">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="b4883-293">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-293">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-294">**ASP.NET Core 模組 Stdout 記錄檔：** 根應用程式的記錄檔會建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="b4883-294">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="b4883-295">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-295">The sub-app's log file isn't created.</span></span>

* <span data-ttu-id="b4883-296">**ASP.NET Core 模組 Debug 記錄：** 根應用程式的記錄檔會建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="b4883-296">**ASP.NET Core Module Debug Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="b4883-297">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-297">The sub-app's log file isn't created.</span></span>

<span data-ttu-id="b4883-298">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-298">Troubleshooting:</span></span>

<span data-ttu-id="b4883-299">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段，或子應用程式未繼承父應用程式的處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-299">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section or that the sub-app doesn't inherit the parent app's handlers.</span></span>

<span data-ttu-id="b4883-300">父應用程式 *web.config* 的 `<system.webServer>` 區段位於 `<location>` 元素內。</span><span class="sxs-lookup"><span data-stu-id="b4883-300">The parent app's `<system.webServer>` section of *web.config* is placed inside of a `<location>` element.</span></span> <span data-ttu-id="b4883-301"><xref:System.Configuration.SectionInformation.InheritInChildApplications*>屬性設定為， `false` 表示在元素內指定的設定不會 [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) 由位於父應用程式子目錄中的應用程式所繼承。</span><span class="sxs-lookup"><span data-stu-id="b4883-301">The <xref:System.Configuration.SectionInformation.InheritInChildApplications*> property is set to `false` to indicate that the settings specified within the [\<location>](/iis/manage/managing-your-configuration-settings/understanding-iis-configuration-delegation#the-concept-of-location) element aren't inherited by apps that reside in a subdirectory of the parent app.</span></span> <span data-ttu-id="b4883-302">如需詳細資訊，請參閱<xref:host-and-deploy/aspnet-core-module>。</span><span class="sxs-lookup"><span data-stu-id="b4883-302">For more information, see <xref:host-and-deploy/aspnet-core-module>.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="b4883-303">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="b4883-303">stdout log path incorrect</span></span>

* <span data-ttu-id="b4883-304">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="b4883-304">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="b4883-305">**應用程式記錄檔：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-305">**Application Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="b4883-306">例外狀況訊息： HRESULT 0x80070005 在 {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp：84傳回。</span><span class="sxs-lookup"><span data-stu-id="b4883-306">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="b4883-307">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-307">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="b4883-308">例外狀況訊息：在 {PATH} 傳回 HRESULT 0x80070002。</span><span class="sxs-lookup"><span data-stu-id="b4883-308">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="b4883-309">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-309">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

* <span data-ttu-id="b4883-310">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-310">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

* <span data-ttu-id="b4883-311">**ASP.NET Core 模組 Debug 記錄：** 無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-311">**ASP.NET Core Module debug Log:** Could not start stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="b4883-312">例外狀況訊息： HRESULT 0x80070005 在 {PATH} \aspnetcoremodulev2\commonlib\fileoutputmanager.cpp：84傳回。</span><span class="sxs-lookup"><span data-stu-id="b4883-312">Exception message: HRESULT 0x80070005 returned at {PATH}\aspnetcoremodulev2\commonlib\fileoutputmanager.cpp:84.</span></span> <span data-ttu-id="b4883-313">無法在 C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll 中停止 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-313">Could not stop stdout redirection in C:\Program Files\IIS\Asp.Net Core Module\V2\aspnetcorev2.dll.</span></span> <span data-ttu-id="b4883-314">例外狀況訊息：在 {PATH} 傳回 HRESULT 0x80070002。</span><span class="sxs-lookup"><span data-stu-id="b4883-314">Exception message: HRESULT 0x80070002 returned at {PATH}.</span></span> <span data-ttu-id="b4883-315">無法在 {PATH}\aspnetcorev2_inprocess.dll 中啟動 stdout 重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-315">Could not start stdout redirection in {PATH}\aspnetcorev2_inprocess.dll.</span></span>

<span data-ttu-id="b4883-316">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-316">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-317">*web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="b4883-317">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="b4883-318">如需詳細資訊，請參閱 [ASP.NET Core 模組：記錄檔建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-318">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="b4883-319">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b4883-319">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="b4883-320">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="b4883-320">Application configuration general issue</span></span>

* <span data-ttu-id="b4883-321">**Browser：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗 **--或--** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-321">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure **--OR--** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="b4883-322">**應用程式記錄檔：** 變數</span><span class="sxs-lookup"><span data-stu-id="b4883-322">**Application Log:** Variable</span></span>

* <span data-ttu-id="b4883-323">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔會建立，但會在應用程式失敗點之前，以一般專案來建立或建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-323">**ASP.NET Core Module stdout Log:** The log file is created but empty or created with normal entries until the point of the app failing.</span></span>

* <span data-ttu-id="b4883-324">**ASP.NET Core 模組 Debug 記錄：** 變數</span><span class="sxs-lookup"><span data-stu-id="b4883-324">**ASP.NET Core Module Debug Log:** Variable</span></span>

<span data-ttu-id="b4883-325">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-325">Troubleshooting:</span></span>

<span data-ttu-id="b4883-326">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-326">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="b4883-327">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="b4883-327">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="b4883-328">本主題說明常見的錯誤，並針對在 Azure Apps 服務和 IIS 上裝載 ASP.NET Core 應用程式時，提供特定錯誤的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="b4883-328">This topic describes common errors and provides troubleshooting advice for specific errors when hosting ASP.NET Core apps on Azure Apps Service and IIS.</span></span>

<span data-ttu-id="b4883-329">如需一般疑難排解指引，請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="b4883-329">For general troubleshooting guidance, see <xref:test/troubleshoot-azure-iis>.</span></span>

<span data-ttu-id="b4883-330">收集下列資訊：</span><span class="sxs-lookup"><span data-stu-id="b4883-330">Collect the following information:</span></span>

* <span data-ttu-id="b4883-331">瀏覽器行為 (狀態碼和錯誤訊息)</span><span class="sxs-lookup"><span data-stu-id="b4883-331">Browser behavior (status code and error message)</span></span>
* <span data-ttu-id="b4883-332">應用程式事件記錄檔項目</span><span class="sxs-lookup"><span data-stu-id="b4883-332">Application Event Log entries</span></span>
  * <span data-ttu-id="b4883-333">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="b4883-333">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="b4883-334">IIS</span><span class="sxs-lookup"><span data-stu-id="b4883-334">IIS</span></span>
    1. <span data-ttu-id="b4883-335">在 [Windows]\*\*\*\* 功能表中選取 [開始]\*\*\*\*，鍵入「事件檢視器」\*\*，然後按 **Enter** 鍵。</span><span class="sxs-lookup"><span data-stu-id="b4883-335">Select **Start** on the **Windows** menu, type *Event Viewer*, and press **Enter**.</span></span>
    1. <span data-ttu-id="b4883-336">在 [事件檢視器]\*\*\*\* 開啟之後，展開提要欄位中的 [Windows 記錄檔]**[應用程式]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-336">After the **Event Viewer** opens, expand **Windows Logs** > **Application** in the sidebar.</span></span>
* <span data-ttu-id="b4883-337">ASP.NET Core 模組 stdout 和偵錯記錄項目</span><span class="sxs-lookup"><span data-stu-id="b4883-337">ASP.NET Core Module stdout and debug log entries</span></span>
  * <span data-ttu-id="b4883-338">Azure App Service：請參閱 <xref:test/troubleshoot-azure-iis> 。</span><span class="sxs-lookup"><span data-stu-id="b4883-338">Azure App Service: See <xref:test/troubleshoot-azure-iis>.</span></span>
  * <span data-ttu-id="b4883-339">IIS：遵循 ASP.NET Core 課程模組主題的 [記錄檔建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) 重新導向和 [增強的診斷記錄](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) 章節中的指示。</span><span class="sxs-lookup"><span data-stu-id="b4883-339">IIS: Follow the instructions in the [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection) and [Enhanced diagnostic logs](xref:host-and-deploy/aspnet-core-module#enhanced-diagnostic-logs) sections of the ASP.NET Core Module topic.</span></span>

<span data-ttu-id="b4883-340">將錯誤資訊與下列常見錯誤進行比較。</span><span class="sxs-lookup"><span data-stu-id="b4883-340">Compare error information to the following common errors.</span></span> <span data-ttu-id="b4883-341">如果找到相符情況，請依照疑難排解建議進行操作。</span><span class="sxs-lookup"><span data-stu-id="b4883-341">If a match is found, follow the troubleshooting advice.</span></span>

<span data-ttu-id="b4883-342">本主題中的錯誤清單並不完整。</span><span class="sxs-lookup"><span data-stu-id="b4883-342">The list of errors in this topic isn't exhaustive.</span></span> <span data-ttu-id="b4883-343">如果您遇到這裡未列出的錯誤，請使用本主題底部的 [內容意見反應]\*\*\*\* 按鈕來開啟新的問題，並提供如何重現錯誤的詳細指示。</span><span class="sxs-lookup"><span data-stu-id="b4883-343">If you encounter an error not listed here, open a new issue using the **Content feedback** button at the bottom of this topic with detailed instructions on how to reproduce the error.</span></span>

[!INCLUDE[Azure App Service Preview Notice](../includes/azure-apps-preview-notice.md)]

## <a name="os-upgrade-removed-the-32-bit-aspnet-core-module"></a><span data-ttu-id="b4883-344">作業系統升級已移除 32 位元的 ASP.NET Core 模組</span><span class="sxs-lookup"><span data-stu-id="b4883-344">OS upgrade removed the 32-bit ASP.NET Core Module</span></span>

<span data-ttu-id="b4883-345">**應用程式記錄檔：** 無法載入模組 DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll**。</span><span class="sxs-lookup"><span data-stu-id="b4883-345">**Application Log:** The Module DLL **C:\WINDOWS\system32\inetsrv\aspnetcore.dll** failed to load.</span></span> <span data-ttu-id="b4883-346">資料即錯誤。</span><span class="sxs-lookup"><span data-stu-id="b4883-346">The data is the error.</span></span>

<span data-ttu-id="b4883-347">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-347">Troubleshooting:</span></span>

<span data-ttu-id="b4883-348">作業系統升級時不會保留 **C:\Windows\SysWOW64\inetsrv** 目錄中的非作業系統檔案。</span><span class="sxs-lookup"><span data-stu-id="b4883-348">Non-OS files in the **C:\Windows\SysWOW64\inetsrv** directory aren't preserved during an OS upgrade.</span></span> <span data-ttu-id="b4883-349">如果先安裝 ASP.NET Core 模組再升級 OS，然後在 OS 升級之後以 32 位元模式執行任何應用程式集區，就會發生此問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-349">If the ASP.NET Core Module is installed prior to an OS upgrade and then any app pool is run in 32-bit mode after an OS upgrade, this issue is encountered.</span></span> <span data-ttu-id="b4883-350">作業系統升級之後，請修復 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="b4883-350">After an OS upgrade, repair the ASP.NET Core Module.</span></span> <span data-ttu-id="b4883-351">請參閱 [安裝 .Net Core 裝載](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)套件組合。</span><span class="sxs-lookup"><span data-stu-id="b4883-351">See [Install the .NET Core Hosting bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span> <span data-ttu-id="b4883-352">執行安裝程式時，請選取 [修復]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-352">Select **Repair** when the installer is run.</span></span>

## <a name="missing-site-extension-32-bit-x86-and-64-bit-x64-site-extensions-installed-or-wrong-process-bitness-set"></a><span data-ttu-id="b4883-353">遺失網站延伸模組、已安裝 32 位元 (x86) 和 64 位元 (x64) 網站延伸模組，或錯誤的處理序位元集合</span><span class="sxs-lookup"><span data-stu-id="b4883-353">Missing site extension, 32-bit (x86) and 64-bit (x64) site extensions installed, or wrong process bitness set</span></span>

<span data-ttu-id="b4883-354">*適用於 Azure 應用程式服務所裝載的應用程式。*</span><span class="sxs-lookup"><span data-stu-id="b4883-354">*Applies to apps hosted by Azure App Services.*</span></span>

* <span data-ttu-id="b4883-355">**Browser：** HTTP 錯誤 500.0-ANCM 同進程處理常式載入失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-355">**Browser:** HTTP Error 500.0 - ANCM In-Process Handler Load Failure</span></span>

* <span data-ttu-id="b4883-356">**應用程式記錄檔：** 叫用 hostfxr 以找出原生要求處理常式失敗，但未找出任何原生相依性。</span><span class="sxs-lookup"><span data-stu-id="b4883-356">**Application Log:** Invoking hostfxr to find the inprocess request handler failed without finding any native dependencies.</span></span> <span data-ttu-id="b4883-357">找不到同處理序要求處理常式。</span><span class="sxs-lookup"><span data-stu-id="b4883-357">Could not find inprocess request handler.</span></span> <span data-ttu-id="b4883-358">從叫用 hostfxr 所捕獲的輸出：找不到任何相容的 framework 版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-358">Captured output from invoking hostfxr: It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-359">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="b4883-359">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span> <span data-ttu-id="b4883-360">無法啟動應用程式 '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'。</span><span class="sxs-lookup"><span data-stu-id="b4883-360">Failed to start application '/LM/W3SVC/1416782824/ROOT', ErrorCode '0x8000ffff'.</span></span>

* <span data-ttu-id="b4883-361">**ASP.NET Core 模組 Stdout 記錄檔：** 找不到任何相容的 framework 版本。</span><span class="sxs-lookup"><span data-stu-id="b4883-361">**ASP.NET Core Module stdout Log:** It was not possible to find any compatible framework version.</span></span> <span data-ttu-id="b4883-362">找不到指定的架構 'Microsoft.AspNetCore.App' 版本 '{VERSION}-preview-\*'。</span><span class="sxs-lookup"><span data-stu-id="b4883-362">The specified framework 'Microsoft.AspNetCore.App', version '{VERSION}-preview-\*' was not found.</span></span>

<span data-ttu-id="b4883-363">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-363">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-364">如果在預覽執行階段中執行應用程式，請安裝與應用程式位元和應用程式執行階段版本相符的 32 位元 (x86) **或** 64 位元 (x64) 網站延伸模組。</span><span class="sxs-lookup"><span data-stu-id="b4883-364">If running the app on a preview runtime, install either the 32-bit (x86) **or** 64-bit (x64) site extension that matches the bitness of the app and the app's runtime version.</span></span> <span data-ttu-id="b4883-365">**請勿同時安裝這兩個延伸模組或延伸模組的多個執行階段版本。**</span><span class="sxs-lookup"><span data-stu-id="b4883-365">**Don't install both extensions or multiple runtime versions of the extension.**</span></span>

  * <span data-ttu-id="b4883-366">ASP.NET Core {RUNTIME VERSION} (x86) 執行階段</span><span class="sxs-lookup"><span data-stu-id="b4883-366">ASP.NET Core {RUNTIME VERSION} (x86) Runtime</span></span>
  * <span data-ttu-id="b4883-367">ASP.NET Core {RUNTIME VERSION} (x64) 執行階段</span><span class="sxs-lookup"><span data-stu-id="b4883-367">ASP.NET Core {RUNTIME VERSION} (x64) Runtime</span></span>

  <span data-ttu-id="b4883-368">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-368">Restart the app.</span></span> <span data-ttu-id="b4883-369">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b4883-369">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="b4883-370">如果在預覽執行階段中執行應用程式，而且已經安裝 32 位元 (x86) 和 64 位元 (x64) [網站延伸模組](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension)，請將與應用程式位元不符的網站延伸模組解除安裝。</span><span class="sxs-lookup"><span data-stu-id="b4883-370">If running the app on a preview runtime and both the 32-bit (x86) and 64-bit (x64) [site extensions](xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension) are installed, uninstall the site extension that doesn't match the bitness of the app.</span></span> <span data-ttu-id="b4883-371">移除網站延伸模組之後，請重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-371">After removing the site extension, restart the app.</span></span> <span data-ttu-id="b4883-372">請等候數秒，讓應用程式重新啟動。</span><span class="sxs-lookup"><span data-stu-id="b4883-372">Wait several seconds for the app to restart.</span></span>

* <span data-ttu-id="b4883-373">如果在預覽執行階段中執行應用程式，且網站延伸模組位元與應用程式相符，請確認預覽網站延伸模組的「執行階段版本」\*\* 是否與應用程式相符。</span><span class="sxs-lookup"><span data-stu-id="b4883-373">If running the app on a preview runtime and the site extension's bitness matches that of the app, confirm that the preview site extension's *runtime version* matches the app's runtime version.</span></span>

* <span data-ttu-id="b4883-374">確認**應用程式設定**中應用程式的**平台**與應用程式位元相符。</span><span class="sxs-lookup"><span data-stu-id="b4883-374">Confirm that the app's **Platform** in **Application Settings** matches the bitness of the app.</span></span>

<span data-ttu-id="b4883-375">如需詳細資訊，請參閱<xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>。</span><span class="sxs-lookup"><span data-stu-id="b4883-375">For more information, see <xref:host-and-deploy/azure-apps/index#install-the-preview-site-extension>.</span></span>

## <a name="an-x86-app-is-deployed-but-the-app-pool-isnt-enabled-for-32-bit-apps"></a><span data-ttu-id="b4883-376">已部署 x86 應用程式，但未啟用 32 位元應用程式的應用程式集區</span><span class="sxs-lookup"><span data-stu-id="b4883-376">An x86 app is deployed but the app pool isn't enabled for 32-bit apps</span></span>

* <span data-ttu-id="b4883-377">**Browser：** HTTP 錯誤 500.30-ANCM 同進程啟動失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-377">**Browser:** HTTP Error 500.30 - ANCM In-Process Start Failure</span></span>

* <span data-ttu-id="b4883-378">**應用程式記錄檔：** 實體根目錄為 ' {PATH} ' 的應用程式 '/LM/W3SVC/5/ROOT ' 遇到未預期的 managed 例外狀況，例外狀況代碼 = ' 0xe0434352 '。</span><span class="sxs-lookup"><span data-stu-id="b4883-378">**Application Log:** Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' hit unexpected managed exception, exception code = '0xe0434352'.</span></span> <span data-ttu-id="b4883-379">如需詳細資訊，請檢查 stderr 記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-379">Please check the stderr logs for more information.</span></span> <span data-ttu-id="b4883-380">實體根目錄為 '{PATH}' 的應用程式 '/LM/W3SVC/5/ROOT' 無法載入 CLR 和受控應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-380">Application '/LM/W3SVC/5/ROOT' with physical root '{PATH}' failed to load clr and managed application.</span></span> <span data-ttu-id="b4883-381">CLR 背景工作執行緒不當結束</span><span class="sxs-lookup"><span data-stu-id="b4883-381">CLR worker thread exited prematurely</span></span>

* <span data-ttu-id="b4883-382">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="b4883-382">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="b4883-383">SDK 會在發行獨立應用程式時攔截到此案例。</span><span class="sxs-lookup"><span data-stu-id="b4883-383">This scenario is trapped by the SDK when publishing a self-contained app.</span></span> <span data-ttu-id="b4883-384">如果 RID 不符合平台目標 (例如 `win10-x64` RID 在專案檔中使用 `<PlatformTarget>x86</PlatformTarget>`)，SDK 就會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="b4883-384">The SDK produces an error if the RID doesn't match the platform target (for example, `win10-x64` RID with `<PlatformTarget>x86</PlatformTarget>` in the project file).</span></span>

<span data-ttu-id="b4883-385">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-385">Troubleshooting:</span></span>

<span data-ttu-id="b4883-386">針對 x86 架構相依部署 (`<PlatformTarget>x86</PlatformTarget>`)，請啟用 32 位元應用程式的 IIS 應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="b4883-386">For an x86 framework-dependent deployment (`<PlatformTarget>x86</PlatformTarget>`), enable the IIS app pool for 32-bit apps.</span></span> <span data-ttu-id="b4883-387">在 [IIS 管理員] 中，開啟應用程式集區的 [進階設定]\*\*\*\*，然後將 [啟用 32 位元應用程式]\*\*\*\* 設定為 [True]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-387">In IIS Manager, open the app pool's **Advanced Settings** and set **Enable 32-Bit Applications** to **True**.</span></span>

## <a name="platform-conflicts-with-rid"></a><span data-ttu-id="b4883-388">平台發生 RID 衝突</span><span class="sxs-lookup"><span data-stu-id="b4883-388">Platform conflicts with RID</span></span>

* <span data-ttu-id="b4883-389">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-389">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="b4883-390">**應用程式記錄檔：** 實體根目錄為 ' C： path} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "c： \{ PATH} {ASSEMBLY}" 啟動進程。 {exe | dll} "'，ErrorCode = ' 0x80004005： ff。</span><span class="sxs-lookup"><span data-stu-id="b4883-390">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"C:\{PATH}{ASSEMBLY}.{exe|dll}" ', ErrorCode = '0x80004005 : ff.</span></span>

* <span data-ttu-id="b4883-391">**ASP.NET Core 模組 Stdout 記錄檔：** 未處理的例外狀況： System. BadImageFormatException：無法載入檔案或元件 ' {ASSEMBLY} .dll '。</span><span class="sxs-lookup"><span data-stu-id="b4883-391">**ASP.NET Core Module stdout Log:** Unhandled Exception: System.BadImageFormatException: Could not load file or assembly '{ASSEMBLY}.dll'.</span></span> <span data-ttu-id="b4883-392">嘗試載入了格式不正確的程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-392">An attempt was made to load a program with an incorrect format.</span></span>

<span data-ttu-id="b4883-393">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-393">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-394">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="b4883-394">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="b4883-395">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-395">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="b4883-396">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b4883-396">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="b4883-397">如果在升級應用程式和部署更新的組件時，Azure 應用程式部署發生這個例外狀況，請手動刪除來自先前部署的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="b4883-397">If this exception occurs for an Azure Apps deployment when upgrading an app and deploying newer assemblies, manually delete all files from the prior deployment.</span></span> <span data-ttu-id="b4883-398">部署升級的應用程式時，延遲不相容的組件會導致 `System.BadImageFormatException` 例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b4883-398">Lingering incompatible assemblies can result in a `System.BadImageFormatException` exception when deploying an upgraded app.</span></span>

## <a name="uri-endpoint-wrong-or-stopped-website"></a><span data-ttu-id="b4883-399">URI 端點錯誤或停止了網站</span><span class="sxs-lookup"><span data-stu-id="b4883-399">URI endpoint wrong or stopped website</span></span>

* <span data-ttu-id="b4883-400">**Browser：** ERR_CONNECTION_REFUSED **--或--** 無法連接</span><span class="sxs-lookup"><span data-stu-id="b4883-400">**Browser:** ERR_CONNECTION_REFUSED **--OR--** Unable to connect</span></span>

* <span data-ttu-id="b4883-401">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-401">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-402">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-402">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-403">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-403">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-404">確認使用對應用程式正確的 URI 端點。</span><span class="sxs-lookup"><span data-stu-id="b4883-404">Confirm the correct URI endpoint for the app is in use.</span></span> <span data-ttu-id="b4883-405">檢查繫結。</span><span class="sxs-lookup"><span data-stu-id="b4883-405">Check the bindings.</span></span>

* <span data-ttu-id="b4883-406">確認 IIS 網站不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="b4883-406">Confirm that the IIS website isn't in the *Stopped* state.</span></span>

## <a name="corewebengine-or-w3svc-server-features-disabled"></a><span data-ttu-id="b4883-407">已停用 CoreWebEngine 或 W3SVC 伺服器功能</span><span class="sxs-lookup"><span data-stu-id="b4883-407">CoreWebEngine or W3SVC server features disabled</span></span>

<span data-ttu-id="b4883-408">**作業系統例外狀況：** 必須安裝 IIS 7.0 CoreWebEngine 和 W3SVC 功能，才能使用 ASP.NET Core 模組。</span><span class="sxs-lookup"><span data-stu-id="b4883-408">**OS Exception:** The IIS 7.0 CoreWebEngine and W3SVC features must be installed to use the ASP.NET Core Module.</span></span>

<span data-ttu-id="b4883-409">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-409">Troubleshooting:</span></span>

<span data-ttu-id="b4883-410">確認已啟用適當的角色和功能。</span><span class="sxs-lookup"><span data-stu-id="b4883-410">Confirm that the proper role and features are enabled.</span></span> <span data-ttu-id="b4883-411">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b4883-411">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

## <a name="incorrect-website-physical-path-or-app-missing"></a><span data-ttu-id="b4883-412">不正確的網站實體路徑或遺失應用程式</span><span class="sxs-lookup"><span data-stu-id="b4883-412">Incorrect website physical path or app missing</span></span>

* <span data-ttu-id="b4883-413">**瀏覽器：** 403 禁止 - 存取被拒 **--或--** 403.14 禁止 - 網頁伺服器已設為不列出此目錄的內容。</span><span class="sxs-lookup"><span data-stu-id="b4883-413">**Browser:** 403 Forbidden - Access is denied **--OR--** 403.14 Forbidden - The Web server is configured to not list the contents of this directory.</span></span>

* <span data-ttu-id="b4883-414">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-414">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-415">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-415">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-416">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-416">Troubleshooting:</span></span>

<span data-ttu-id="b4883-417">檢查 IIS 網站的 [基本設定]\*\*\*\* 和實體的應用程式資料夾。</span><span class="sxs-lookup"><span data-stu-id="b4883-417">Check the IIS website **Basic Settings** and the physical app folder.</span></span> <span data-ttu-id="b4883-418">確認應用程式位於 IIS 網站**實體路徑**的資料夾中。</span><span class="sxs-lookup"><span data-stu-id="b4883-418">Confirm that the app is in the folder at the IIS website **Physical path**.</span></span>

## <a name="incorrect-role-aspnet-core-module-not-installed-or-incorrect-permissions"></a><span data-ttu-id="b4883-419">角色不正確、ASP.NET Core 模組未安裝，或權限不正確</span><span class="sxs-lookup"><span data-stu-id="b4883-419">Incorrect role, ASP.NET Core Module not installed, or incorrect permissions</span></span>

* <span data-ttu-id="b4883-420">**瀏覽器：** 500.19 內部伺服器錯誤 - 無法存取所要求的網頁，因為該頁面的相關組態資料無效。</span><span class="sxs-lookup"><span data-stu-id="b4883-420">**Browser:** 500.19 Internal Server Error - The requested page cannot be accessed because the related configuration data for the page is invalid.</span></span> <span data-ttu-id="b4883-421">**--或--** 無法顯示此頁面</span><span class="sxs-lookup"><span data-stu-id="b4883-421">**--OR--** This page can't be displayed</span></span>

* <span data-ttu-id="b4883-422">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-422">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-423">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-423">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-424">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-424">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-425">確認已啟用適當的角色。</span><span class="sxs-lookup"><span data-stu-id="b4883-425">Confirm that the proper role is enabled.</span></span> <span data-ttu-id="b4883-426">請參閱 [IIS 組態](xref:host-and-deploy/iis/index#iis-configuration)。</span><span class="sxs-lookup"><span data-stu-id="b4883-426">See [IIS Configuration](xref:host-and-deploy/iis/index#iis-configuration).</span></span>

* <span data-ttu-id="b4883-427">開啟 [程式和功能]\*\*\*\* 或 [應用程式與功能]\*\*\*\*，並確認已安裝 **Windows Server Hosting**。</span><span class="sxs-lookup"><span data-stu-id="b4883-427">Open **Programs & Features** or **Apps & features** and confirm that **Windows Server Hosting** is installed.</span></span> <span data-ttu-id="b4883-428">如果已安裝的程式清單中沒有 **Windows Server Hosting**，請下載並安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="b4883-428">If **Windows Server Hosting** isn't present in the list of installed programs, download and install the .NET Core Hosting Bundle.</span></span>

  [<span data-ttu-id="b4883-429">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="b4883-429">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="b4883-430">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="b4883-430">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

* <span data-ttu-id="b4883-431">請確定**應用程式**集區 > **進程模型** > **Identity** 設定為\*\* Identity ApplicationPool\*\* ，或自訂身分識別具有正確的許可權，可存取應用程式的部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="b4883-431">Make sure that the **Application Pool** > **Process Model** > **Identity** is set to **ApplicationPoolIdentity** or the custom identity has the correct permissions to access the app's deployment folder.</span></span>

* <span data-ttu-id="b4883-432">若解除安裝 ASP.NET Core Hosting Bundle 並安裝舊版裝載套件組合，*applicationHost.config* 檔案不會包括 ASP.NET Core Module 的區段。</span><span class="sxs-lookup"><span data-stu-id="b4883-432">If you uninstalled the ASP.NET Core Hosting Bundle and installed an earlier version of the hosting bundle, the *applicationHost.config* file doesn't include a section for the ASP.NET Core Module.</span></span> <span data-ttu-id="b4883-433">開啟位於 *%windir%/System32/inetsrv/config* 的 *applicationHost.config*，然後尋找 `<configuration><configSections><sectionGroup name="system.webServer">` 區段群組。</span><span class="sxs-lookup"><span data-stu-id="b4883-433">Open *applicationHost.config* at *%windir%/System32/inetsrv/config* and find the `<configuration><configSections><sectionGroup name="system.webServer">` section group.</span></span> <span data-ttu-id="b4883-434">若區段群組中沒有 ASP.NET Core Module 的區段，請新增區段元素：</span><span class="sxs-lookup"><span data-stu-id="b4883-434">If the section for the ASP.NET Core Module is missing from the section group, add the section element:</span></span>

  ```xml
  <section name="aspNetCore" overrideModeDefault="Allow" />
  ```

  <span data-ttu-id="b4883-435">或者，您也可以安裝最新版的「ASP.NET Core 裝載套件組合」。</span><span class="sxs-lookup"><span data-stu-id="b4883-435">Alternatively, install the latest version of the ASP.NET Core Hosting Bundle.</span></span> <span data-ttu-id="b4883-436">最新版本回溯相容，提供支援的 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b4883-436">The latest version is backwards-compatible with supported ASP.NET Core apps.</span></span>

## <a name="incorrect-processpath-missing-path-variable-hosting-bundle-not-installed-systemiis-not-restarted-vc-redistributable-not-installed-or-dotnetexe-access-violation"></a><span data-ttu-id="b4883-437">不正確的 processPath、遺失 PATH 變數、未安裝裝載套件組合、未重新啟動系統/IIS、未安裝 VC++ 可轉散發套件或 dotnet.exe 存取違規</span><span class="sxs-lookup"><span data-stu-id="b4883-437">Incorrect processPath, missing PATH variable, Hosting Bundle not installed, system/IIS not restarted, VC++ Redistributable not installed, or dotnet.exe access violation</span></span>

* <span data-ttu-id="b4883-438">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-438">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="b4883-439">**應用程式記錄檔：** 實體根目錄為 ' C： PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "{...}" 啟動進程</span><span class="sxs-lookup"><span data-stu-id="b4883-439">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"{...}"</span></span> <span data-ttu-id="b4883-440">'，ErrorCode = ' 0x80070002：0。</span><span class="sxs-lookup"><span data-stu-id="b4883-440">', ErrorCode = '0x80070002 : 0.</span></span>

* <span data-ttu-id="b4883-441">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="b4883-441">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="b4883-442">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-442">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-443">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="b4883-443">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="b4883-444">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-444">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="b4883-445">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b4883-445">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="b4883-446">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *processPath* 屬性，以確認它是 `dotnet` (適用於架構相依部署 (FDD)) 或 `.\{ASSEMBLY}.exe` (適用於[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd))。</span><span class="sxs-lookup"><span data-stu-id="b4883-446">Check the *processPath* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's `dotnet` for a framework-dependent deployment (FDD) or `.\{ASSEMBLY}.exe` for a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd).</span></span>

* <span data-ttu-id="b4883-447">若為 FDD，可能無法透過路徑設定存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="b4883-447">For an FDD, *dotnet.exe* might not be accessible via the PATH settings.</span></span> <span data-ttu-id="b4883-448">確認\*C:\Program Files\dotnet \\ \*存在於 [系統路徑] 設定中。</span><span class="sxs-lookup"><span data-stu-id="b4883-448">Confirm that *C:\Program Files\dotnet\\* exists in the System PATH settings.</span></span>

* <span data-ttu-id="b4883-449">若為 FDD，應用程式集區的使用者身分識別可能無法存取 *dotnet.exe*。</span><span class="sxs-lookup"><span data-stu-id="b4883-449">For an FDD, *dotnet.exe* might not be accessible for the user identity of the app pool.</span></span> <span data-ttu-id="b4883-450">確認應用程式集區使用者身分識別可以存取 *C:\Program Files\dotnet* 目錄。</span><span class="sxs-lookup"><span data-stu-id="b4883-450">Confirm that the app pool user identity has access to the *C:\Program Files\dotnet* directory.</span></span> <span data-ttu-id="b4883-451">確認 *C:\Program Files\dotnet* 和應用程式目錄上未針對應用程式集區使用者身分識別設定任何拒絕規則。</span><span class="sxs-lookup"><span data-stu-id="b4883-451">Confirm that there are no deny rules configured for the app pool user identity on the *C:\Program Files\dotnet* and app directories.</span></span>

* <span data-ttu-id="b4883-452">可能已部署 FDD 並安裝 .NET Core，但未重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="b4883-452">An FDD may have been deployed and .NET Core installed without restarting IIS.</span></span> <span data-ttu-id="b4883-453">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，重新啟動伺服器或重新啟動 IIS。</span><span class="sxs-lookup"><span data-stu-id="b4883-453">Either restart the server or restart IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

* <span data-ttu-id="b4883-454">可能已部署 FDD，但未在主控系統上安裝 .NET Core 執行階段。</span><span class="sxs-lookup"><span data-stu-id="b4883-454">An FDD may have been deployed without installing the .NET Core runtime on the hosting system.</span></span> <span data-ttu-id="b4883-455">如果尚未安裝 .NET Core 執行階段，請在系統上執行「.NET Core 裝載套件組合安裝程式」\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="b4883-455">If the .NET Core runtime hasn't been installed, run the **.NET Core Hosting Bundle installer** on the system.</span></span>

  [<span data-ttu-id="b4883-456">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="b4883-456">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

  <span data-ttu-id="b4883-457">如需詳細資訊，請參閱[安裝 .NET Core 裝載套件組合](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle)。</span><span class="sxs-lookup"><span data-stu-id="b4883-457">For more information, see [Install the .NET Core Hosting Bundle](xref:host-and-deploy/iis/index#install-the-net-core-hosting-bundle).</span></span>

  <span data-ttu-id="b4883-458">如果需要特定執行階段，請從 [.NET Download Archives](https://dotnet.microsoft.com/download/archives) (.NET 下載封存) 下載執行階段，並將它安裝在系統上。</span><span class="sxs-lookup"><span data-stu-id="b4883-458">If a specific runtime is required, download the runtime from the [.NET Download Archives](https://dotnet.microsoft.com/download/archives) and install it on the system.</span></span> <span data-ttu-id="b4883-459">從命令提示字元依序執行 **net stop was /y** 和 **net start w3svc**，透過重新啟動系統或重新啟動 IIS 來完成安裝。</span><span class="sxs-lookup"><span data-stu-id="b4883-459">Complete the installation by restarting the system or restarting IIS by executing **net stop was /y** followed by **net start w3svc** from a command prompt.</span></span>

## <a name="incorrect-arguments-of-aspnetcore-element"></a><span data-ttu-id="b4883-460">元素的引數不正確 \<aspNetCore></span><span class="sxs-lookup"><span data-stu-id="b4883-460">Incorrect arguments of \<aspNetCore> element</span></span>

* <span data-ttu-id="b4883-461">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-461">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="b4883-462">**應用程式記錄檔：** 實體根目錄為 ' C： PATH} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' \{ \' 無法以命令列 ' "dotnet" 啟動進程。 \{ASSEMBLY} .dll '，ErrorCode = ' 0x80004005：80008081。</span><span class="sxs-lookup"><span data-stu-id="b4883-462">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' failed to start process with commandline '"dotnet" .\{ASSEMBLY}.dll', ErrorCode = '0x80004005 : 80008081.</span></span>

* <span data-ttu-id="b4883-463">**ASP.NET Core 模組 Stdout 記錄檔：** 要執行的應用程式不存在： ' PATH \{ ASSEMBLY} .dll '</span><span class="sxs-lookup"><span data-stu-id="b4883-463">**ASP.NET Core Module stdout Log:** The application to execute does not exist: 'PATH\{ASSEMBLY}.dll'</span></span>

<span data-ttu-id="b4883-464">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-464">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-465">確認應用程式在 Kestrel 本機上執行。</span><span class="sxs-lookup"><span data-stu-id="b4883-465">Confirm that the app runs locally on Kestrel.</span></span> <span data-ttu-id="b4883-466">處理序失敗，可能是因為應用程式發生問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-466">A process failure might be the result of a problem within the app.</span></span> <span data-ttu-id="b4883-467">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b4883-467">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

* <span data-ttu-id="b4883-468">檢查 *web.config* 中 `<aspNetCore>` 元素上的 *arguments* 屬性，以確認它是 (a) `.\{ASSEMBLY}.dll` (適用於架構相依部署 (FDD))；或 (b) 不存在、空字串 (`arguments=""`)，或是應用程式的引數清單 (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`，適用於自封式部署 (SCD))。</span><span class="sxs-lookup"><span data-stu-id="b4883-468">Examine the *arguments* attribute on the `<aspNetCore>` element in *web.config* to confirm that it's either (a) `.\{ASSEMBLY}.dll` for a framework-dependent deployment (FDD); or (b) not present, an empty string (`arguments=""`), or a list of the app's arguments (`arguments="{ARGUMENT_1}, {ARGUMENT_2}, ... {ARGUMENT_X}"`) for a self-contained deployment (SCD).</span></span>

<span data-ttu-id="b4883-469">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-469">Troubleshooting:</span></span>

<span data-ttu-id="b4883-470">若為結構相依部署 (FDD)，請確認已在系統上安裝正確的執行階段。</span><span class="sxs-lookup"><span data-stu-id="b4883-470">For a framework-dependent deployment (FDD), confirm that the correct runtime installed on the system.</span></span>

## <a name="stopped-application-pool"></a><span data-ttu-id="b4883-471">已停止應用程式集區</span><span class="sxs-lookup"><span data-stu-id="b4883-471">Stopped Application Pool</span></span>

* <span data-ttu-id="b4883-472">**瀏覽器：** 503 服務無法使用</span><span class="sxs-lookup"><span data-stu-id="b4883-472">**Browser:** 503 Service Unavailable</span></span>

* <span data-ttu-id="b4883-473">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-473">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-474">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-474">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-475">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-475">Troubleshooting:</span></span>

<span data-ttu-id="b4883-476">確認「應用程式集區」不是處於「已停止」\*\* 狀態。</span><span class="sxs-lookup"><span data-stu-id="b4883-476">Confirm that the Application Pool isn't in the *Stopped* state.</span></span>

## <a name="sub-application-includes-a-handlers-section"></a><span data-ttu-id="b4883-477">子應用程式包含 \<handlers> 區段</span><span class="sxs-lookup"><span data-stu-id="b4883-477">Sub-application includes a \<handlers> section</span></span>

* <span data-ttu-id="b4883-478">**瀏覽器：** HTTP 錯誤 500.19 - 內部伺服器錯誤</span><span class="sxs-lookup"><span data-stu-id="b4883-478">**Browser:** HTTP Error 500.19 - Internal Server Error</span></span>

* <span data-ttu-id="b4883-479">**應用程式記錄檔：** 無項目</span><span class="sxs-lookup"><span data-stu-id="b4883-479">**Application Log:** No entry</span></span>

* <span data-ttu-id="b4883-480">**ASP.NET Core 模組 Stdout 記錄檔：** 根應用程式的記錄檔會建立，並顯示一般作業。</span><span class="sxs-lookup"><span data-stu-id="b4883-480">**ASP.NET Core Module stdout Log:** The root app's log file is created and shows normal operation.</span></span> <span data-ttu-id="b4883-481">未建立子應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="b4883-481">The sub-app's log file isn't created.</span></span>

<span data-ttu-id="b4883-482">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-482">Troubleshooting:</span></span>

<span data-ttu-id="b4883-483">請確認子應用程式的 *web.config* 檔案不包含 `<handlers>` 區段。</span><span class="sxs-lookup"><span data-stu-id="b4883-483">Confirm that the sub-app's *web.config* file doesn't include a `<handlers>` section.</span></span>

## <a name="stdout-log-path-incorrect"></a><span data-ttu-id="b4883-484">stdout 記錄檔路徑不正確</span><span class="sxs-lookup"><span data-stu-id="b4883-484">stdout log path incorrect</span></span>

* <span data-ttu-id="b4883-485">**瀏覽器：** 應用程式正常回應。</span><span class="sxs-lookup"><span data-stu-id="b4883-485">**Browser:** The app responds normally.</span></span>

* <span data-ttu-id="b4883-486">**應用程式記錄檔：** 警告：無法建立 >stdoutlogfile \\ ？ \{路徑} \ path_doesnt_exist \ stdout_ {處理序識別碼} _ {TIMESTAMP} .log，ErrorCode =-2147024893。</span><span class="sxs-lookup"><span data-stu-id="b4883-486">**Application Log:** Warning: Could not create stdoutLogFile \\?\{PATH}\path_doesnt_exist\stdout_{PROCESS ID}_{TIMESTAMP}.log, ErrorCode = -2147024893.</span></span>

* <span data-ttu-id="b4883-487">**ASP.NET Core 模組 Stdout 記錄檔：** 記錄檔不會建立。</span><span class="sxs-lookup"><span data-stu-id="b4883-487">**ASP.NET Core Module stdout Log:** The log file isn't created.</span></span>

<span data-ttu-id="b4883-488">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-488">Troubleshooting:</span></span>

* <span data-ttu-id="b4883-489">*web.config* 中 `<aspNetCore>` 元素所指定的 `stdoutLogFile` 路徑不存在。</span><span class="sxs-lookup"><span data-stu-id="b4883-489">The `stdoutLogFile` path specified in the `<aspNetCore>` element of *web.config* doesn't exist.</span></span> <span data-ttu-id="b4883-490">如需詳細資訊，請參閱 [ASP.NET Core 模組：記錄檔建立和](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection)重新導向。</span><span class="sxs-lookup"><span data-stu-id="b4883-490">For more information, see [ASP.NET Core Module: Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection).</span></span>

* <span data-ttu-id="b4883-491">應用程式集區使用者沒有 stdout 記錄檔路徑的寫入權限。</span><span class="sxs-lookup"><span data-stu-id="b4883-491">The app pool user doesn't have write access to the stdout log path.</span></span>

## <a name="application-configuration-general-issue"></a><span data-ttu-id="b4883-492">應用程式組態一般問題</span><span class="sxs-lookup"><span data-stu-id="b4883-492">Application configuration general issue</span></span>

* <span data-ttu-id="b4883-493">**瀏覽器：** HTTP 錯誤 502.5 - 處理序失敗</span><span class="sxs-lookup"><span data-stu-id="b4883-493">**Browser:** HTTP Error 502.5 - Process Failure</span></span>

* <span data-ttu-id="b4883-494">**應用程式記錄檔：** 實體根目錄為 ' C： path} 的應用程式 ' MACHINE/WEBROOT/APPHOST/{ASSEMBLY} ' 已 \{ \' 建立具有命令列 ' "c： \{ path} \{ ASSEMBLY} 的進程。 {exe | dll} "'，但已損毀或沒有回應，或未接聽指定的埠 ' {PORT} '，ErrorCode = ' {錯誤碼} '</span><span class="sxs-lookup"><span data-stu-id="b4883-494">**Application Log:** Application 'MACHINE/WEBROOT/APPHOST/{ASSEMBLY}' with physical root 'C:\{PATH}\' created process with commandline '"C:\{PATH}\{ASSEMBLY}.{exe|dll}" ' but either crashed or did not respond or did not listen on the given port '{PORT}', ErrorCode = '{ERROR CODE}'</span></span>

* <span data-ttu-id="b4883-495">**ASP.NET Core 模組 Stdout 記錄檔：** 已建立記錄檔，但卻是空的。</span><span class="sxs-lookup"><span data-stu-id="b4883-495">**ASP.NET Core Module stdout Log:** The log file is created but empty.</span></span>

<span data-ttu-id="b4883-496">疑難排解：</span><span class="sxs-lookup"><span data-stu-id="b4883-496">Troubleshooting:</span></span>

<span data-ttu-id="b4883-497">處理序無法啟動，最可能的原因是應用程式組態或程式設計問題。</span><span class="sxs-lookup"><span data-stu-id="b4883-497">The process failed to start, most likely due to an app configuration or programming issue.</span></span>

<span data-ttu-id="b4883-498">如需詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="b4883-498">For more information, see the following topics:</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:test/troubleshoot>

::: moniker-end
