---
title: 將 ASP.NET Core 應用程式發佈到 IIS
author: rick-anderson
description: 了解如何在 IIS 伺服器上裝載 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/03/2019
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
uid: tutorials/publish-to-iis
ms.openlocfilehash: fc8573625328fe18970cd6bcd5627b71680d5ca7
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88020232"
---
# <a name="publish-an-aspnet-core-app-to-iis"></a><span data-ttu-id="21ef5-103">將 ASP.NET Core 應用程式發佈到 IIS</span><span class="sxs-lookup"><span data-stu-id="21ef5-103">Publish an ASP.NET Core app to IIS</span></span>

<span data-ttu-id="21ef5-104">本教學課程會示範如何在 IIS 伺服器上裝載 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-104">This tutorial shows how to host an ASP.NET Core app on an IIS server.</span></span>

<span data-ttu-id="21ef5-105">本教學課程涵蓋下列主題：</span><span class="sxs-lookup"><span data-stu-id="21ef5-105">This tutorial covers the following subjects:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21ef5-106">在 Windows Server 上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="21ef5-106">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="21ef5-107">在 IIS 管理員中建立 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="21ef5-107">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="21ef5-108">部署 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-108">Deploy an ASP.NET Core app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="21ef5-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="21ef5-109">Prerequisites</span></span>

* <span data-ttu-id="21ef5-110">在部署電腦上安裝 [.NET Core SDK](/dotnet/core/sdk)。</span><span class="sxs-lookup"><span data-stu-id="21ef5-110">[.NET Core SDK](/dotnet/core/sdk) installed on the development machine.</span></span>
* <span data-ttu-id="21ef5-111">使用 **Web Server (IIS)** 伺服器角色設定的 Windows Server。</span><span class="sxs-lookup"><span data-stu-id="21ef5-111">Windows Server configured with the **Web Server (IIS)** server role.</span></span> <span data-ttu-id="21ef5-112">若您的伺服器並未設為使用 IIS 來裝載網站，請遵循 <xref:host-and-deploy/iis/index#iis-configuration> 一文中＜IIS 組態＞\*\* 一節內的指導，然後返回本教學課程。</span><span class="sxs-lookup"><span data-stu-id="21ef5-112">If your server isn't configured to host websites with IIS, follow the guidance in the *IIS configuration* section of the <xref:host-and-deploy/iis/index#iis-configuration> article and then return to this tutorial.</span></span>

> [!WARNING]
> <span data-ttu-id="21ef5-113">**IIS 組態和網站安全性涉及本教學課程並未涵蓋的概念。**</span><span class="sxs-lookup"><span data-stu-id="21ef5-113">**IIS configuration and website security involve concepts that aren't covered by this tutorial.**</span></span> <span data-ttu-id="21ef5-114">請諮詢 [Microsoft IIS 文件](https://www.iis.net/)和[使用 IIS 進行裝載的 ASP.NET Core 文章](xref:host-and-deploy/iis/index)，再於 IIS 上裝載生產應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-114">Consult the IIS guidance in the [Microsoft IIS documentation](https://www.iis.net/) and the [ASP.NET Core article on hosting with IIS](xref:host-and-deploy/iis/index) before hosting production apps on IIS.</span></span>
>
> <span data-ttu-id="21ef5-115">本教學課程中尚未涵蓋的 IIS 裝載重要案例包括：</span><span class="sxs-lookup"><span data-stu-id="21ef5-115">Important scenarios for IIS hosting not covered by this tutorial include:</span></span>
>
> * [<span data-ttu-id="21ef5-116">建立 ASP.NET Core 資料保護的登錄區</span><span class="sxs-lookup"><span data-stu-id="21ef5-116">Creation of a registry hive for ASP.NET Core Data Protection</span></span>](xref:host-and-deploy/iis/index#data-protection)
> * [<span data-ttu-id="21ef5-117">設定應用程式集區的存取控制清單 (ACL)</span><span class="sxs-lookup"><span data-stu-id="21ef5-117">Configuration of the app pool's Access Control List (ACL)</span></span>](xref:host-and-deploy/iis/index#application-pool-identity)
> * <span data-ttu-id="21ef5-118">為了聚焦在 IIS 部署概念，本教學課程會在並未於 IIS 上設定 HTTPS 安全性的情況下部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-118">To focus on IIS deployment concepts, this tutorial deploys an app without HTTPS security configured in IIS.</span></span> <span data-ttu-id="21ef5-119">如需裝載啟用 HTTPS 通訊協定應用程式的詳細資訊，請參閱本文[＜其他資源＞](#additional-resources)一節中的安全性主題。</span><span class="sxs-lookup"><span data-stu-id="21ef5-119">For more information on hosting an app enabled for HTTPS protocol, see the security topics in the [Additional resources](#additional-resources) section of this article.</span></span> <span data-ttu-id="21ef5-120"><xref:host-and-deploy/iis/index> 一文中提供了裝載 ASP.NET Core 應用程式的進一步指導。</span><span class="sxs-lookup"><span data-stu-id="21ef5-120">Further guidance for hosting ASP.NET Core apps is provided in the <xref:host-and-deploy/iis/index> article.</span></span>

## <a name="install-the-net-core-hosting-bundle"></a><span data-ttu-id="21ef5-121">安裝 .NET Core 裝載套件組合</span><span class="sxs-lookup"><span data-stu-id="21ef5-121">Install the .NET Core Hosting Bundle</span></span>

<span data-ttu-id="21ef5-122">在 IIS 伺服器上安裝 *.NET Core 裝載套件組合*。</span><span class="sxs-lookup"><span data-stu-id="21ef5-122">Install the *.NET Core Hosting Bundle* on the IIS server.</span></span> <span data-ttu-id="21ef5-123">套件組合會安裝 .NET Core 執行時間、.NET Core 程式庫和[ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)。</span><span class="sxs-lookup"><span data-stu-id="21ef5-123">The bundle installs the .NET Core Runtime, .NET Core Library, and the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module).</span></span> <span data-ttu-id="21ef5-124">此模組可讓 ASP.NET Core 應用程式在 IIS 背後執行。</span><span class="sxs-lookup"><span data-stu-id="21ef5-124">The module allows ASP.NET Core apps to run behind IIS.</span></span>

<span data-ttu-id="21ef5-125">使用下列連結下載安裝程式：</span><span class="sxs-lookup"><span data-stu-id="21ef5-125">Download the installer using the following link:</span></span>

[<span data-ttu-id="21ef5-126">目前的 .NET Core 裝載套件組合安裝程式 (直接下載)</span><span class="sxs-lookup"><span data-stu-id="21ef5-126">Current .NET Core Hosting Bundle installer (direct download)</span></span>](https://dotnet.microsoft.com/permalink/dotnetcore-current-windows-runtime-bundle-installer)

1. <span data-ttu-id="21ef5-127">在 IIS 伺服器上執行安裝程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-127">Run the installer on the IIS server.</span></span>

1. <span data-ttu-id="21ef5-128">在命令殼層中重新啟動伺服器或執行 **net stop was /y**，然後執行 **net start w3svc**。</span><span class="sxs-lookup"><span data-stu-id="21ef5-128">Restart the server or execute **net stop was /y** followed by **net start w3svc** in a command shell.</span></span>

## <a name="create-the-iis-site"></a><span data-ttu-id="21ef5-129">建立 IIS 網站</span><span class="sxs-lookup"><span data-stu-id="21ef5-129">Create the IIS site</span></span>

1. <span data-ttu-id="21ef5-130">在 IIS 伺服器上，建立資料夾以包含應用程式的發佈資料夾和檔案。</span><span class="sxs-lookup"><span data-stu-id="21ef5-130">On the IIS server, create a folder to contain the app's published folders and files.</span></span> <span data-ttu-id="21ef5-131">在下列步驟中，您提供資料夾路徑給 IIS，作為應用程式的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="21ef5-131">In a following step, the folder's path is provided to IIS as the physical path to the app.</span></span>

1. <span data-ttu-id="21ef5-132">在 [IIS 管理員] 中 **，在 [** 連線] 面板中開啟伺服器的節點。</span><span class="sxs-lookup"><span data-stu-id="21ef5-132">In IIS Manager, open the server's node in the **Connections** panel.</span></span> <span data-ttu-id="21ef5-133">以滑鼠右鍵按一下 [網站]\*\*\*\* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="21ef5-133">Right-click the **Sites** folder.</span></span> <span data-ttu-id="21ef5-134">從操作功能表選取 [新增網站]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="21ef5-134">Select **Add Website** from the contextual menu.</span></span>

1. <span data-ttu-id="21ef5-135">提供**網站名稱**，並將**實體路徑**設定為您建立的應用程式部署資料夾。</span><span class="sxs-lookup"><span data-stu-id="21ef5-135">Provide a **Site name** and set the **Physical path** to the app's deployment folder that you created.</span></span> <span data-ttu-id="21ef5-136">提供**繫結**組態，然後選取 [確定]\*\*\*\* 來建立網站。</span><span class="sxs-lookup"><span data-stu-id="21ef5-136">Provide the **Binding** configuration and create the website by selecting **OK**.</span></span>

## <a name="create-an-aspnet-core-no-locrazor-pages-app"></a><span data-ttu-id="21ef5-137">建立 ASP.NET Core Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="21ef5-137">Create an ASP.NET Core Razor Pages app</span></span>

<span data-ttu-id="21ef5-138">依照 <xref:getting-started> 教學課程來建立 Razor 頁面應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-138">Follow the <xref:getting-started> tutorial to create a Razor Pages app.</span></span>

## <a name="publish-and-deploy-the-app"></a><span data-ttu-id="21ef5-139">發佈及部署應用程式</span><span class="sxs-lookup"><span data-stu-id="21ef5-139">Publish and deploy the app</span></span>

<span data-ttu-id="21ef5-140">「發佈應用程式」\*\* 表示產生可由伺服器裝載的編譯應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-140">*Publish an app* means to produce a compiled app that can be hosted by a server.</span></span> <span data-ttu-id="21ef5-141">「部署應用程式」\*\* 表示將發佈後的應用程式移動到裝載系統。</span><span class="sxs-lookup"><span data-stu-id="21ef5-141">*Deploy an app* means to move the published app to a hosting system.</span></span> <span data-ttu-id="21ef5-142">發佈步驟會由 [.NET Core SDK](/dotnet/core/sdk) 處理，部署步驟則是可以透過各種方式處理。</span><span class="sxs-lookup"><span data-stu-id="21ef5-142">The publish step is handled by the [.NET Core SDK](/dotnet/core/sdk), while the deployment step can be handled by a variety of approaches.</span></span> <span data-ttu-id="21ef5-143">本教學課程採用「資料夾」\*\* 部署方法，其中：</span><span class="sxs-lookup"><span data-stu-id="21ef5-143">This tutorial adopts the *folder* deployment approach, where:</span></span>

* <span data-ttu-id="21ef5-144">應用程式會發佈到資料夾。</span><span class="sxs-lookup"><span data-stu-id="21ef5-144">The app is published to a folder.</span></span>
* <span data-ttu-id="21ef5-145">資料夾內容會移動到 IIS 網站的資料夾 (指向 IIS 管理員中網站的**實體路徑**)。</span><span class="sxs-lookup"><span data-stu-id="21ef5-145">The folder's contents are moved to the IIS site's folder (the **Physical path** to the site in IIS Manager).</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="21ef5-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="21ef5-146">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="21ef5-147">以滑鼠右鍵按一下**方案總管**中的專案，然後選取 [**發佈**]。</span><span class="sxs-lookup"><span data-stu-id="21ef5-147">Right-click on the project in **Solution Explorer** and select **Publish**.</span></span>
1. <span data-ttu-id="21ef5-148">在 [挑選發佈目標]\*\*\*\* 對話方塊中，選取 [資料夾]\*\*\*\* 發佈選項。</span><span class="sxs-lookup"><span data-stu-id="21ef5-148">In the **Pick a publish target** dialog, select the **Folder** publish option.</span></span>
1. <span data-ttu-id="21ef5-149">設定 [資料夾或檔案共用]\*\*\*\* 路徑。</span><span class="sxs-lookup"><span data-stu-id="21ef5-149">Set the **Folder or File Share** path.</span></span>
   * <span data-ttu-id="21ef5-150">若您針對部署電腦上提供為網路共用的 IIS 網站建立資料夾，則請提供指向共用的路徑。</span><span class="sxs-lookup"><span data-stu-id="21ef5-150">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="21ef5-151">目前的使用者必須具備寫入存取權限才能發佈到共用。</span><span class="sxs-lookup"><span data-stu-id="21ef5-151">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="21ef5-152">若您無法直接部署到 IIS 伺服器上的 IIS 網站資料夾，請發佈到抽取式媒體上資料夾並透過物理方式將所發佈應用程式實際移動到伺服器上的 IIS 網站資料夾，即 IIS 管理員中的**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="21ef5-152">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="21ef5-153">將 *bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中內容移動到伺服器上的 IIS 網站資料夾，即 IIS 管理員中的**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="21ef5-153">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="21ef5-154">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="21ef5-154">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="21ef5-155">在命令殼層中，使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，以發行組態來發佈應用程式：</span><span class="sxs-lookup"><span data-stu-id="21ef5-155">In a command shell, publish the app in Release configuration with the [dotnet publish](/dotnet/core/tools/dotnet-publish) command:</span></span>

   ```dotnetcli
   dotnet publish --configuration Release
   ```

1. <span data-ttu-id="21ef5-156">將 *bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中內容移動到伺服器上的 IIS 網站資料夾，即 IIS 管理員中的**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="21ef5-156">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="21ef5-157">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="21ef5-157">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="21ef5-158">在 [解決方案]\*\*\*\* 中按一下專案，然後選取 [發佈]\*\*\*\* > [發佈到資料夾]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="21ef5-158">Right-click on the project in **Solution** and select **Publish** > **Publish to Folder**.</span></span>
1. <span data-ttu-id="21ef5-159">設定 [選取資料夾]\*\*\*\* 路徑。</span><span class="sxs-lookup"><span data-stu-id="21ef5-159">Set the **Choose a folder** path.</span></span>
   * <span data-ttu-id="21ef5-160">若您針對部署電腦上提供為網路共用的 IIS 網站建立資料夾，則請提供指向共用的路徑。</span><span class="sxs-lookup"><span data-stu-id="21ef5-160">If you created a folder for the IIS site that's available on the development machine as a network share, provide the path to the share.</span></span> <span data-ttu-id="21ef5-161">目前的使用者必須具備寫入存取權限才能發佈到共用。</span><span class="sxs-lookup"><span data-stu-id="21ef5-161">The current user must have write access to publish to the share.</span></span>
   * <span data-ttu-id="21ef5-162">若您無法直接部署到 IIS 伺服器上的 IIS 網站資料夾，請發佈到抽取式媒體上資料夾並透過物理方式將所發佈應用程式實際移動到伺服器上的 IIS 網站資料夾，即 IIS 管理員中的**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="21ef5-162">If you're unable to deploy directly to the IIS site folder on the IIS server, publish to a folder on removeable media and physically move the published app to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span> <span data-ttu-id="21ef5-163">將 *bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中內容移動到伺服器上的 IIS 網站資料夾，即 IIS 管理員中的**實體路徑**。</span><span class="sxs-lookup"><span data-stu-id="21ef5-163">Move the contents of the *bin/Release/{TARGET FRAMEWORK}/publish* folder to the IIS site folder on the server, which is the site's **Physical path** in IIS Manager.</span></span>

---

## <a name="browse-the-website"></a><span data-ttu-id="21ef5-164">瀏覽網站</span><span class="sxs-lookup"><span data-stu-id="21ef5-164">Browse the website</span></span>

<span data-ttu-id="21ef5-165">應用程式會在收到第一個要求時，在瀏覽器中提供存取。</span><span class="sxs-lookup"><span data-stu-id="21ef5-165">The app is accessible in a browser after it receives the first request.</span></span> <span data-ttu-id="21ef5-166">在您於 IIS 管理員中為網站建立的端點繫結處，對應用程式發出要求。</span><span class="sxs-lookup"><span data-stu-id="21ef5-166">Make a request to the app at the endpoint binding that you established in IIS Manager for the site.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21ef5-167">後續步驟</span><span class="sxs-lookup"><span data-stu-id="21ef5-167">Next steps</span></span>

<span data-ttu-id="21ef5-168">在本教學課程中，您已了解如何：</span><span class="sxs-lookup"><span data-stu-id="21ef5-168">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="21ef5-169">在 Windows Server 上安裝 .NET Core 裝載套件組合。</span><span class="sxs-lookup"><span data-stu-id="21ef5-169">Install the .NET Core Hosting Bundle on Windows Server.</span></span>
> * <span data-ttu-id="21ef5-170">在 IIS 管理員中建立 IIS 網站。</span><span class="sxs-lookup"><span data-stu-id="21ef5-170">Create an IIS site in IIS Manager.</span></span>
> * <span data-ttu-id="21ef5-171">部署 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="21ef5-171">Deploy an ASP.NET Core app.</span></span>

<span data-ttu-id="21ef5-172">若要深入了解在 IIS 上裝載 ASP.NET Core 應用程式，請參閱 IIS 概觀文章：</span><span class="sxs-lookup"><span data-stu-id="21ef5-172">To learn more about hosting ASP.NET Core apps on IIS, see the IIS Overview article:</span></span>

> [!div class="nextstepaction"]
> <xref:host-and-deploy/iis/index>

## <a name="additional-resources"></a><span data-ttu-id="21ef5-173">其他資源</span><span class="sxs-lookup"><span data-stu-id="21ef5-173">Additional resources</span></span>

### <a name="articles-in-the-aspnet-core-documentation-set"></a><span data-ttu-id="21ef5-174">ASP.NET Core 文件集中的文章</span><span class="sxs-lookup"><span data-stu-id="21ef5-174">Articles in the ASP.NET Core documentation set</span></span>

* <xref:host-and-deploy/aspnet-core-module>
* <xref:host-and-deploy/directory-structure>
* <xref:test/troubleshoot-azure-iis>
* <xref:security/enforcing-ssl>

### <a name="articles-pertaining-to-aspnet-core-app-deployment"></a><span data-ttu-id="21ef5-175">與 ASP.NET Core 應用程式部署相關的文章</span><span class="sxs-lookup"><span data-stu-id="21ef5-175">Articles pertaining to ASP.NET Core app deployment</span></span>

* <xref:tutorials/publish-to-azure-webapp-using-vs>
* <xref:tutorials/publish-to-azure-webapp-using-vscode>
* <xref:host-and-deploy/visual-studio-publish-profiles>
* [<span data-ttu-id="21ef5-176">使用 Visual Studio for Mac 將 Web 應用程式發佈到資料夾</span><span class="sxs-lookup"><span data-stu-id="21ef5-176">Publish a Web app to a folder using Visual Studio for Mac</span></span>](/visualstudio/mac/publish-folder)

### <a name="articles-on-iis-https-configuration"></a><span data-ttu-id="21ef5-177">IIS HTTPS 組態的相關文章</span><span class="sxs-lookup"><span data-stu-id="21ef5-177">Articles on IIS HTTPS configuration</span></span>

* [<span data-ttu-id="21ef5-178">在 IIS 管理員中設定 SSL</span><span class="sxs-lookup"><span data-stu-id="21ef5-178">Configuring SSL in IIS Manager</span></span>](/iis/manage/configuring-security/configuring-ssl-in-iis-manager)
* [<span data-ttu-id="21ef5-179">如何在 IIS 上設定 SSL</span><span class="sxs-lookup"><span data-stu-id="21ef5-179">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)

### <a name="articles-on-iis-and-windows-server"></a><span data-ttu-id="21ef5-180">IIS 和 Windows Server 的相關文章</span><span class="sxs-lookup"><span data-stu-id="21ef5-180">Articles on IIS and Windows Server</span></span>

* [<span data-ttu-id="21ef5-181">Microsoft IIS 官方網站</span><span class="sxs-lookup"><span data-stu-id="21ef5-181">The Official Microsoft IIS Site</span></span>](https://www.iis.net/)
* [<span data-ttu-id="21ef5-182">Windows Server 技術內容庫</span><span class="sxs-lookup"><span data-stu-id="21ef5-182">Windows Server technical content library</span></span>](/windows-server/windows-server)
