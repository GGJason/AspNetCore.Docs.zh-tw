---
title: :::no-loc(SignalR):::將 ASP.NET Core 應用程式發佈至 Azure App Service
author: bradygaster
description: '瞭解如何將 ASP.NET Core :::no-loc(SignalR)::: 應用程式發佈到 Azure App Service。'
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/02/2020
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
uid: signalr/publish-to-azure-web-app
ms.openlocfilehash: 8e6d36fe0b38486f94078b8f9cf12b852da7e0d9
ms.sourcegitcommit: d64bf0cbe763beda22a7728c7f10d07fc5e19262
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/03/2020
ms.locfileid: "93234502"
---
# <a name="publish-an-aspnet-core-no-locsignalr-app-to-azure-app-service"></a><span data-ttu-id="9a348-103">:::no-loc(SignalR):::將 ASP.NET Core 應用程式發佈至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="9a348-103">Publish an ASP.NET Core :::no-loc(SignalR)::: app to Azure App Service</span></span>

<span data-ttu-id="9a348-104">依 [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="9a348-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="9a348-105">[Azure App Service](/azure/app-service/app-service-web-overview) 是用來裝載 web 應用程式的 [Microsoft 雲端](https://azure.microsoft.com/) 運算平臺服務，包括 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="9a348-105">[Azure App Service](/azure/app-service/app-service-web-overview) is a [Microsoft cloud computing](https://azure.microsoft.com/) platform service for hosting web apps, including ASP.NET Core.</span></span>

> [!NOTE]
> <span data-ttu-id="9a348-106">本文是指從 Visual Studio 發佈 ASP.NET Core :::no-loc(SignalR)::: 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a348-106">This article refers to publishing an ASP.NET Core :::no-loc(SignalR)::: app from Visual Studio.</span></span> <span data-ttu-id="9a348-107">如需詳細資訊，請參閱[ :::no-loc(SignalR)::: Azure 服務](https://azure.microsoft.com/services/signalr-service)。</span><span class="sxs-lookup"><span data-stu-id="9a348-107">For more information, see [:::no-loc(SignalR)::: service for Azure](https://azure.microsoft.com/services/signalr-service).</span></span>

## <a name="publish-the-app"></a><span data-ttu-id="9a348-108">發佈應用程式</span><span class="sxs-lookup"><span data-stu-id="9a348-108">Publish the app</span></span>

<span data-ttu-id="9a348-109">本文涵蓋使用 Visual Studio 中的工具進行發佈。</span><span class="sxs-lookup"><span data-stu-id="9a348-109">This article covers publishing using the tools in Visual Studio.</span></span> <span data-ttu-id="9a348-110">Visual Studio Code 使用者可以使用 [Azure CLI](/cli/azure) 命令，將應用程式發佈至 Azure。</span><span class="sxs-lookup"><span data-stu-id="9a348-110">Visual Studio Code users can use [Azure CLI](/cli/azure) commands to publish apps to Azure.</span></span> <span data-ttu-id="9a348-111">如需詳細資訊，請參閱 [使用命令列工具將 ASP.NET Core 應用程式發佈至 Azure](/azure/app-service/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="9a348-111">For more information, see [Publish an ASP.NET Core app to Azure with command line tools](/azure/app-service/app-service-web-get-started-dotnet).</span></span>

1. <span data-ttu-id="9a348-112">以滑鼠右鍵按一下 **方案總管** 中的專案，然後選取 [ **發行** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-112">Right-click on the project in **Solution Explorer** and select **Publish** .</span></span>

1. <span data-ttu-id="9a348-113">確認在 [ **挑選發佈目標** ] 對話方塊中選取了 [ **App Service** 和 **建立新** 的]。</span><span class="sxs-lookup"><span data-stu-id="9a348-113">Confirm that **App Service** and **Create new** are selected in the **Pick a publish target** dialog.</span></span>

1. <span data-ttu-id="9a348-114">從 [ **發行** ] 按鈕下拉式清單中選取 [ **建立設定檔** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-114">Select **Create Profile** from the **Publish** button drop down.</span></span>

   <span data-ttu-id="9a348-115">在 [ **建立 App Service** ] 對話方塊中，輸入下表所述的資訊，然後選取 [ **建立** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-115">Enter the information described in the following table in the **Create App Service** dialog and select **Create** .</span></span>

   | <span data-ttu-id="9a348-116">項目</span><span class="sxs-lookup"><span data-stu-id="9a348-116">Item</span></span>               | <span data-ttu-id="9a348-117">描述</span><span class="sxs-lookup"><span data-stu-id="9a348-117">Description</span></span> |
   | ------------------ | ----------- |
   | <span data-ttu-id="9a348-118">**名稱**</span><span class="sxs-lookup"><span data-stu-id="9a348-118">**Name**</span></span>           | <span data-ttu-id="9a348-119">應用程式的唯一名稱。</span><span class="sxs-lookup"><span data-stu-id="9a348-119">Unique name of the app.</span></span> |
   | <span data-ttu-id="9a348-120">**訂用帳戶**</span><span class="sxs-lookup"><span data-stu-id="9a348-120">**Subscription**</span></span>   | <span data-ttu-id="9a348-121">應用程式使用的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="9a348-121">Azure subscription that the app uses.</span></span> |
   | <span data-ttu-id="9a348-122">**資源群組**</span><span class="sxs-lookup"><span data-stu-id="9a348-122">**Resource Group**</span></span> | <span data-ttu-id="9a348-123">應用程式所屬的相關資源群組。</span><span class="sxs-lookup"><span data-stu-id="9a348-123">Group of related resources to which the app belongs.</span></span> |
   | <span data-ttu-id="9a348-124">**主控方案**</span><span class="sxs-lookup"><span data-stu-id="9a348-124">**Hosting Plan**</span></span>   | <span data-ttu-id="9a348-125">Web 應用程式的定價方案。</span><span class="sxs-lookup"><span data-stu-id="9a348-125">Pricing plan for the web app.</span></span> |

1. <span data-ttu-id="9a348-126">在 [服務相依性 **]** 區段中選取 [ **Azure :::no-loc(SignalR)::: 服務** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-126">Select **Azure :::no-loc(SignalR)::: Service** in the **Service Dependencies** section.</span></span> <span data-ttu-id="9a348-127">選取 **+** 按鈕：</span><span class="sxs-lookup"><span data-stu-id="9a348-127">Select the **+** button:</span></span>

   ![[相依性] 區域會顯示在 [新增] 下拉式清單中選取的 Azure：：： no-loc (SignalR) ：：： Service](publish-to-azure-web-app/_static/signalr-service-dependency.png)

1. <span data-ttu-id="9a348-129">在 [ **azure :::no-loc(SignalR)::: 服務** ] 對話方塊中，選取 [ **建立新的 azure :::no-loc(SignalR)::: 服務實例** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-129">In the **Azure :::no-loc(SignalR)::: Service** dialog, select **Create a new Azure :::no-loc(SignalR)::: Service instance** .</span></span>

1. <span data-ttu-id="9a348-130">提供 **名稱** 、 **資源群組** 和 **位置** 。</span><span class="sxs-lookup"><span data-stu-id="9a348-130">Provide a **Name** , **Resource Group** , and **Location** .</span></span> <span data-ttu-id="9a348-131">返回 [ **Azure :::no-loc(SignalR)::: 服務** ] 對話方塊，然後選取 [ **新增** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-131">Return to the **Azure :::no-loc(SignalR)::: Service** dialog and select **Add** .</span></span>

<span data-ttu-id="9a348-132">Visual Studio 完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="9a348-132">Visual Studio completes the following tasks:</span></span>

* <span data-ttu-id="9a348-133">建立包含發行設定的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="9a348-133">Creates a Publish Profile containing publish settings.</span></span>
* <span data-ttu-id="9a348-134">使用提供的詳細資料建立 *Azure Web 應用程式* 。</span><span class="sxs-lookup"><span data-stu-id="9a348-134">Creates an *Azure Web App* with the provided details.</span></span>
* <span data-ttu-id="9a348-135">發佈應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a348-135">Publishes the app.</span></span>
* <span data-ttu-id="9a348-136">啟動載入 web 應用程式的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="9a348-136">Launches a browser, which loads the web app.</span></span>

<span data-ttu-id="9a348-137">應用程式 URL 的格式為 `{APP SERVICE NAME}.azurewebsites.net` 。</span><span class="sxs-lookup"><span data-stu-id="9a348-137">The format of the app's URL is `{APP SERVICE NAME}.azurewebsites.net`.</span></span> <span data-ttu-id="9a348-138">例如，名為的應用程式 `:::no-loc(SignalR):::ChatApp` 具有的 URL `https://signalrchatapp.azurewebsites.net` 。</span><span class="sxs-lookup"><span data-stu-id="9a348-138">For example, an app named `:::no-loc(SignalR):::ChatApp` has a URL of `https://signalrchatapp.azurewebsites.net`.</span></span>

<span data-ttu-id="9a348-139">如果部署以預覽 .NET Core 版本為目標的應用程式時發生 HTTP *502.2-不正確的閘道* 錯誤，請參閱將 [ASP.NET Core 預覽版本部署至 Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) 以解決此問題。</span><span class="sxs-lookup"><span data-stu-id="9a348-139">If an HTTP *502.2 - Bad Gateway* error occurs when deploying an app that targets a preview .NET Core release, see [Deploy ASP.NET Core preview release to Azure App Service](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service) to resolve it.</span></span>

## <a name="configure-the-app-in-azure-app-service"></a><span data-ttu-id="9a348-140">在 Azure App Service 中設定應用程式</span><span class="sxs-lookup"><span data-stu-id="9a348-140">Configure the app in Azure App Service</span></span>

> [!NOTE]
> <span data-ttu-id="9a348-141">*本節僅適用于不使用 Azure 服務的應用程式 :::no-loc(SignalR)::: 。*</span><span class="sxs-lookup"><span data-stu-id="9a348-141">*This section only applies to apps not using the Azure :::no-loc(SignalR)::: Service.*</span></span>
>
> <span data-ttu-id="9a348-142">如果應用程式使用 Azure :::no-loc(SignalR)::: 服務，App Service 不需要設定應用程式要求路由 (ARR) 親和性和 Web 通訊端（本節所述）。</span><span class="sxs-lookup"><span data-stu-id="9a348-142">If the app uses the Azure :::no-loc(SignalR)::: Service, the App Service doesn't require the configuration of Application Request Routing (ARR) Affinity and Web Sockets described in this section.</span></span> <span data-ttu-id="9a348-143">用戶端會將其 Web 通訊端連接至 Azure :::no-loc(SignalR)::: 服務，而不是直接連線到應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a348-143">Clients connect their Web Sockets to the Azure :::no-loc(SignalR)::: Service, not directly to the app.</span></span>

<span data-ttu-id="9a348-144">針對沒有 Azure 服務所裝載的應用程式 :::no-loc(SignalR)::: ，請啟用：</span><span class="sxs-lookup"><span data-stu-id="9a348-144">For apps hosted without the Azure :::no-loc(SignalR)::: Service, enable:</span></span>

* <span data-ttu-id="9a348-145">[ARR 親和性] (https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity- :::no-loc(cookie)::: - (ARR- :::no-loc(cookie):::) # B0 l) 將使用者的要求路由傳送回相同的 App Service 實例。</span><span class="sxs-lookup"><span data-stu-id="9a348-145">[ARR Affinity](https://azure.github.io/AppService/2016/05/16/Disable-Session-affinity-:::no-loc(cookie):::-(ARR-:::no-loc(cookie):::)-for-Azure-web-apps.html) to route requests from a user back to the same App Service instance.</span></span> <span data-ttu-id="9a348-146">預設設定為 [ **開啟** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-146">The default setting is **On** .</span></span>
* <span data-ttu-id="9a348-147">[Web 通訊端](xref:fundamentals/websockets) ，以允許 Web 通訊端傳輸運作。</span><span class="sxs-lookup"><span data-stu-id="9a348-147">[Web Sockets](xref:fundamentals/websockets) to allow the Web Sockets transport to function.</span></span> <span data-ttu-id="9a348-148">預設設定為 [ **關閉** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-148">The default setting is **Off** .</span></span>

1. <span data-ttu-id="9a348-149">在 Azure 入口網站中，流覽至 **應用程式服務** 中的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9a348-149">In the Azure portal, navigate to the web app in **App Services** .</span></span>
1. <span data-ttu-id="9a348-150">開啟 **Configuration**  >  **[設定一般設定** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-150">Open **Configuration** > **General settings** .</span></span>
1. <span data-ttu-id="9a348-151">將 **Web 通訊端** 設定為 **開啟** 。</span><span class="sxs-lookup"><span data-stu-id="9a348-151">Set **Web sockets** to **On** .</span></span>
1. <span data-ttu-id="9a348-152">確認 [ **ARR 親和性** ] 設為 [ **開啟** ]。</span><span class="sxs-lookup"><span data-stu-id="9a348-152">Verify that **ARR affinity** is set to **On** .</span></span>

## <a name="app-service-plan-limits"></a><span data-ttu-id="9a348-153">App Service 方案限制</span><span class="sxs-lookup"><span data-stu-id="9a348-153">App Service Plan limits</span></span>

<span data-ttu-id="9a348-154">Web 通訊端和其他傳輸會根據選取的 App Service 方案而受到限制。</span><span class="sxs-lookup"><span data-stu-id="9a348-154">Web Sockets and other transports are limited based on the App Service Plan selected.</span></span> <span data-ttu-id="9a348-155">如需詳細資訊，請參閱 [Azure 訂用帳戶和服務限制、配額和限制](/azure/azure-subscription-service-limits#app-service-limits)一文的 *Azure 雲端服務限制* 和 *App Service 限制* 章節。</span><span class="sxs-lookup"><span data-stu-id="9a348-155">For more information, see the *Azure Cloud Services limits* and *App Service limits* sections of the [Azure subscription and service limits, quotas, and constraints](/azure/azure-subscription-service-limits#app-service-limits) article.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9a348-156">其他資源</span><span class="sxs-lookup"><span data-stu-id="9a348-156">Additional resources</span></span>

* [<span data-ttu-id="9a348-157">什麼是 Azure :::no-loc(SignalR)::: 服務？</span><span class="sxs-lookup"><span data-stu-id="9a348-157">What is Azure :::no-loc(SignalR)::: Service?</span></span>](/azure/azure-signalr/signalr-overview)
* <xref:signalr/introduction>
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="9a348-158">使用命令列工具將 ASP.NET Core 應用程式發行到 Azure</span><span class="sxs-lookup"><span data-stu-id="9a348-158">Publish an ASP.NET Core app to Azure with command line tools</span></span>](/azure/app-service/app-service-web-get-started-dotnet)
* [<span data-ttu-id="9a348-159">在 Azure 上裝載和部署 ASP.NET Core 預覽版應用程式</span><span class="sxs-lookup"><span data-stu-id="9a348-159">Host and deploy ASP.NET Core Preview apps on Azure</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
