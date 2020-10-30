---
title: 使用 ASP.NET Core 的 Microsoft 身分識別平臺和 Azure Active Directory
author: rick-anderson
description: 探索與 Microsoft 身分識別平臺的驗證相關的主題，這些主題適用于 ASP.NET Core 中的 web 應用程式和 Api 的 Azure Active Directory。
ms.author: riande
ms.date: 01/21/2020
ms.custom: mvc, seodec18
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
uid: security/authentication/azure-active-directory/index
ms.openlocfilehash: c8a3d6838b9b93ff58dfaff8423bd6946b5ca743
ms.sourcegitcommit: ca34c1ac578e7d3daa0febf1810ba5fc74f60bbf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93061401"
---
# <a name="azure-active-directory-with-aspnet-core"></a><span data-ttu-id="922de-103">Azure Active Directory 與 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="922de-103">Azure Active Directory with ASP.NET Core</span></span>

<span data-ttu-id="922de-104">這些教學課程和範例示範如何使用 Microsoft 身分識別平臺和 Azure Active Directory 在 ASP.NET Core 中進行驗證。</span><span class="sxs-lookup"><span data-stu-id="922de-104">These tutorials and samples demonstrate authentication in ASP.NET Core using Microsoft identity platform and Azure Active Directory.</span></span> <span data-ttu-id="922de-105">如需使用 ASP.NET Core 搭配 Azure AD 的其他教學課程和範例，請參閱 [Microsoft 身分識別平臺](/azure/active-directory/develop/)。</span><span class="sxs-lookup"><span data-stu-id="922de-105">For additional tutorials and samples using ASP.NET Core with Azure AD, see [Microsoft identity platform](/azure/active-directory/develop/).</span></span>

## <a name="application-scenarios"></a><span data-ttu-id="922de-106">應用程式情節</span><span class="sxs-lookup"><span data-stu-id="922de-106">Application Scenarios</span></span>

* [<span data-ttu-id="922de-107">快速入門：將「使用 Microsoft 登入」新增至 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="922de-107">Quickstart: Add sign-in with Microsoft to an ASP.NET Core web app</span></span>](/azure/active-directory/develop/quickstart-v2-aspnet-core-webapp)
* [<span data-ttu-id="922de-108">登入使用者的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="922de-108">Web app that signs in users</span></span>](/azure/active-directory/develop/scenario-web-app-sign-user-overview?tabs=aspnetcore)
* [<span data-ttu-id="922de-109">呼叫 Web API 的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="922de-109">Web app that calls web APIs</span></span>](/azure/active-directory/develop/scenario-web-app-call-api-overview)
* [<span data-ttu-id="922de-110">受保護的 Web API</span><span class="sxs-lookup"><span data-stu-id="922de-110">Protected web API</span></span>](/azure/active-directory/develop/scenario-protected-web-api-overview)
* [<span data-ttu-id="922de-111">可呼叫其他 Web API 的 Web API</span><span class="sxs-lookup"><span data-stu-id="922de-111">Web API that calls other web APIs</span></span>](/azure/active-directory/develop/scenario-web-api-call-api-overview)
* [<span data-ttu-id="922de-112">使用 Azure AD B2C 登入使用者的 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="922de-112">Web app that signs in users with Azure AD B2C</span></span>](xref:security/authentication/azure-ad-b2c)

## <a name="samples"></a><span data-ttu-id="922de-113">範例</span><span class="sxs-lookup"><span data-stu-id="922de-113">Samples</span></span>

* <span data-ttu-id="922de-114">[使用 Azure AD V2 讓您的 ASP.NET Core 應用程式登入使用者及呼叫 Web api](/samples/azure-samples/active-directory-aspnetcore-webapp-openidconnect-v2/enable-webapp-signin/)：</span><span class="sxs-lookup"><span data-stu-id="922de-114">[Enable your ASP.NET Core app to sign-in users and call web APIs using Azure AD V2](/samples/azure-samples/active-directory-aspnetcore-webapp-openidconnect-v2/enable-webapp-signin/):</span></span> 
  * <span data-ttu-id="922de-115">請參閱[這部相關影片](https://channel9.msdn.com/Events/Build/2018/THR5001)</span><span class="sxs-lookup"><span data-stu-id="922de-115">See [this associated video](https://channel9.msdn.com/Events/Build/2018/THR5001)</span></span>
* <span data-ttu-id="922de-116">[使用 Azure AD V2 從 WPF 應用程式呼叫 ASP.NET Core 2.0 WEB API](/samples/azure-samples/active-directory-dotnet-native-aspnetcore-v2/calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2/)：</span><span class="sxs-lookup"><span data-stu-id="922de-116">[Calling an ASP.NET Core 2.0 Web API from a WPF application using Azure AD V2](/samples/azure-samples/active-directory-dotnet-native-aspnetcore-v2/calling-an-aspnet-core-web-api-from-a-wpf-application-using-azure-ad-v2/):</span></span> 
  * <span data-ttu-id="922de-117">請參閱[這部相關影片](https://channel9.msdn.com/Events/Build/2018/THR5000)</span><span class="sxs-lookup"><span data-stu-id="922de-117">See [this associated video](https://channel9.msdn.com/Events/Build/2018/THR5000)</span></span>
* [<span data-ttu-id="922de-118">使用 Azure AD B2C 的 ASP.NET Core Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="922de-118">An ASP.NET Core web app with Azure AD B2C</span></span>](/samples/azure-samples/active-directory-b2c-dotnetcore-webapp/an-aspnet-core-web-app-with-azure-ad-b2c/)
