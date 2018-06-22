---
title: 若要建立單一頁面應用程式中 ASP.NET Core 使用 JavaScriptServices
author: scottaddie
description: 深入了解使用 JavaScriptServices 建立單一頁面應用程式 (SPA) 支援的 ASP.NET Core 的優點。
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: c3f454ddd91fadf94e4ee4faa8930d8a89d13833
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279619"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a><span data-ttu-id="8884e-103">若要建立單一頁面應用程式中 ASP.NET Core 使用 JavaScriptServices</span><span class="sxs-lookup"><span data-stu-id="8884e-103">Use JavaScriptServices to Create Single Page Applications in ASP.NET Core</span></span>

<span data-ttu-id="8884e-104">由[Scott Addie](https://github.com/scottaddie)和[Fiyaz Hasan](http://fiyazhasan.me/)</span><span class="sxs-lookup"><span data-stu-id="8884e-104">By [Scott Addie](https://github.com/scottaddie) and [Fiyaz Hasan](http://fiyazhasan.me/)</span></span>

<span data-ttu-id="8884e-105">單一頁面應用程式 (SPA) 是熱門的 web 應用程式，因為其本身的豐富使用者經驗類型。</span><span class="sxs-lookup"><span data-stu-id="8884e-105">A Single Page Application (SPA) is a popular type of web application due to its inherent rich user experience.</span></span> <span data-ttu-id="8884e-106">整合用戶端 SPA 架構或程式庫，例如[Angular](https://angular.io/)或[React](https://facebook.github.io/react/)，與伺服器端架構，像 ASP.NET Core 可能相當困難。 </span><span class="sxs-lookup"><span data-stu-id="8884e-106">Integrating client-side SPA frameworks or libraries, such as [Angular](https://angular.io/) or [React](https://facebook.github.io/react/), with server-side frameworks like ASP.NET Core can be difficult.</span></span> <span data-ttu-id="8884e-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices)特別開發來減少摩擦整合程序中的。</span><span class="sxs-lookup"><span data-stu-id="8884e-107">[JavaScriptServices](https://github.com/aspnet/JavaScriptServices) was developed to reduce friction in the integration process.</span></span> <span data-ttu-id="8884e-108">它可讓不同的用戶端和伺服器技術堆疊之間的無縫式作業。</span><span class="sxs-lookup"><span data-stu-id="8884e-108">It enables seamless operation between the different client and server technology stacks.</span></span>

<span data-ttu-id="8884e-109">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8884e-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a><span data-ttu-id="8884e-110">什麼是 JavaScriptServices？</span><span class="sxs-lookup"><span data-stu-id="8884e-110">What is JavaScriptServices?</span></span>

<span data-ttu-id="8884e-111">JavaScriptServices 是 ASP.NET Core 的用戶端技術的集合。</span><span class="sxs-lookup"><span data-stu-id="8884e-111">JavaScriptServices is a collection of client-side technologies for ASP.NET Core.</span></span> <span data-ttu-id="8884e-112">其目標是要為開發人員的慣用伺服器端平台建置 SPAs 定位 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="8884e-112">Its goal is to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span>

<span data-ttu-id="8884e-113">JavaScriptServices 包含三個相異的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="8884e-113">JavaScriptServices consists of three distinct NuGet packages:</span></span>
* <span data-ttu-id="8884e-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span><span class="sxs-lookup"><span data-stu-id="8884e-114">[Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)</span></span>
* <span data-ttu-id="8884e-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span><span class="sxs-lookup"><span data-stu-id="8884e-115">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)</span></span>
* <span data-ttu-id="8884e-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span><span class="sxs-lookup"><span data-stu-id="8884e-116">[Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)</span></span>

<span data-ttu-id="8884e-117">這些封裝會很有用如果您：</span><span class="sxs-lookup"><span data-stu-id="8884e-117">These packages are useful if you:</span></span>
* <span data-ttu-id="8884e-118">在伺服器上執行的 JavaScript</span><span class="sxs-lookup"><span data-stu-id="8884e-118">Run JavaScript on the server</span></span>
* <span data-ttu-id="8884e-119">使用 SPA 架構或程式庫</span><span class="sxs-lookup"><span data-stu-id="8884e-119">Use a SPA framework or library</span></span>
* <span data-ttu-id="8884e-120">建置用戶端 Webpack 資產</span><span class="sxs-lookup"><span data-stu-id="8884e-120">Build client-side assets with Webpack</span></span>

<span data-ttu-id="8884e-121">在這篇文章的焦點會放在使用 SpaServices 封裝。</span><span class="sxs-lookup"><span data-stu-id="8884e-121">Much of the focus in this article is placed on using the SpaServices package.</span></span>

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a><span data-ttu-id="8884e-122">什麼是 SpaServices？</span><span class="sxs-lookup"><span data-stu-id="8884e-122">What is SpaServices?</span></span>

<span data-ttu-id="8884e-123">SpaServices 建立位置為開發人員的慣用伺服器端平台建置 SPAs ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="8884e-123">SpaServices was created to position ASP.NET Core as developers' preferred server-side platform for building SPAs.</span></span> <span data-ttu-id="8884e-124">SpaServices 不需要開發與 ASP.NET Core SPAs，它並不會鎖定您的特定用戶端架構。</span><span class="sxs-lookup"><span data-stu-id="8884e-124">SpaServices isn't required to develop SPAs with ASP.NET Core, and it doesn't lock you into a particular client framework.</span></span>

<span data-ttu-id="8884e-125">SpaServices 提供有用的基礎結構，例如：</span><span class="sxs-lookup"><span data-stu-id="8884e-125">SpaServices provides useful infrastructure such as:</span></span>
* [<span data-ttu-id="8884e-126">伺服器端尚未</span><span class="sxs-lookup"><span data-stu-id="8884e-126">Server-side prerendering</span></span>](#server-prerendering)
* [<span data-ttu-id="8884e-127">Webpack Dev 中介軟體</span><span class="sxs-lookup"><span data-stu-id="8884e-127">Webpack Dev Middleware</span></span>](#webpack-dev-middleware)
* [<span data-ttu-id="8884e-128">熱模組更換</span><span class="sxs-lookup"><span data-stu-id="8884e-128">Hot Module Replacement</span></span>](#hot-module-replacement)
* [<span data-ttu-id="8884e-129">路由的協助程式</span><span class="sxs-lookup"><span data-stu-id="8884e-129">Routing helpers</span></span>](#routing-helpers)

<span data-ttu-id="8884e-130">整體而言，這些基礎結構元件可強化開發工作流程和執行階段經驗。</span><span class="sxs-lookup"><span data-stu-id="8884e-130">Collectively, these infrastructure components enhance both the development workflow and the runtime experience.</span></span> <span data-ttu-id="8884e-131">元件可以個別採用。</span><span class="sxs-lookup"><span data-stu-id="8884e-131">The components can be adopted individually.</span></span>

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a><span data-ttu-id="8884e-132">使用 SpaServices 的必要條件</span><span class="sxs-lookup"><span data-stu-id="8884e-132">Prerequisites for using SpaServices</span></span>

<span data-ttu-id="8884e-133">若要使用 SpaServices，安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="8884e-133">To work with SpaServices, install the following:</span></span>
* <span data-ttu-id="8884e-134">[Node.js](https://nodejs.org/) （版本 6 或更新版本） 與 npm</span><span class="sxs-lookup"><span data-stu-id="8884e-134">[Node.js](https://nodejs.org/) (version 6 or later) with npm</span></span>
  * <span data-ttu-id="8884e-135">若要確認已安裝這些元件，並可以找到，從命令列執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8884e-135">To verify these components are installed and can be found, run the following from the command line:</span></span>

    ```console
    node -v && npm -v
    ```

<span data-ttu-id="8884e-136">注意： 如果您要部署至 Azure 的網站，您不需要執行以下任何動作&mdash;Node.js 已安裝並可在伺服器環境中。</span><span class="sxs-lookup"><span data-stu-id="8884e-136">Note: If you're deploying to an Azure web site, you don't need to do anything here &mdash; Node.js is installed and available in the server environments.</span></span>

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * <span data-ttu-id="8884e-137">如果您使用 Visual Studio 2017 在 Windows 上，選取已安裝的 SDK **.NET Core 跨平台開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="8884e-137">If you're on Windows using Visual Studio 2017, the SDK is installed by selecting the **.NET Core cross-platform development** workload.</span></span>

* <span data-ttu-id="8884e-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="8884e-138">[Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet package</span></span>

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a><span data-ttu-id="8884e-139">伺服器端渲染</span><span class="sxs-lookup"><span data-stu-id="8884e-139">Server-side prerendering</span></span>

<span data-ttu-id="8884e-140">通用 (也稱為 isomorphic) 應用程式是 JavaScript 應用程式能夠執行同時在伺服器和用戶端上。</span><span class="sxs-lookup"><span data-stu-id="8884e-140">A universal (also known as isomorphic) application is a JavaScript application capable of running both on the server and the client.</span></span> <span data-ttu-id="8884e-141">Angular、 React 和其他常用架構提供此應用程式的開發樣式通用平台。</span><span class="sxs-lookup"><span data-stu-id="8884e-141">Angular, React, and other popular frameworks provide a universal platform for this application development style.</span></span> <span data-ttu-id="8884e-142">做法是先呈現架構上的元件透過 Node.js 伺服器，然後進一步委派給用戶端執行。</span><span class="sxs-lookup"><span data-stu-id="8884e-142">The idea is to first render the framework components on the server via Node.js, and then delegate further execution to the client.</span></span>

<span data-ttu-id="8884e-143">ASP.NET Core[標記協助程式](xref:mvc/views/tag-helpers/intro)提供 SpaServices 簡化的伺服器端尚未實作叫用伺服器上的 JavaScript 函式。</span><span class="sxs-lookup"><span data-stu-id="8884e-143">ASP.NET Core [Tag Helpers](xref:mvc/views/tag-helpers/intro) provided by SpaServices simplify the implementation of server-side prerendering by invoking the JavaScript functions on the server.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8884e-144">必要條件</span><span class="sxs-lookup"><span data-stu-id="8884e-144">Prerequisites</span></span>

<span data-ttu-id="8884e-145">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="8884e-145">Install the following:</span></span>
* <span data-ttu-id="8884e-146">[aspnet 尚未](https://www.npmjs.com/package/aspnet-prerendering)npm 封裝：</span><span class="sxs-lookup"><span data-stu-id="8884e-146">[aspnet-prerendering](https://www.npmjs.com/package/aspnet-prerendering) npm package:</span></span>

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a><span data-ttu-id="8884e-147">組態</span><span class="sxs-lookup"><span data-stu-id="8884e-147">Configuration</span></span>

<span data-ttu-id="8884e-148">標記協助程式會在專案的命名空間註冊透過 *_ViewImports.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="8884e-148">The Tag Helpers are made discoverable via namespace registration in the project's *_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

<span data-ttu-id="8884e-149">這些標記協助程式立即抽象複雜的通訊直接與低階 Api 運用在 Razor 檢視內部類似 HTML 的語法：</span><span class="sxs-lookup"><span data-stu-id="8884e-149">These Tag Helpers abstract away the intricacies of communicating directly with low-level APIs by leveraging an HTML-like syntax inside the Razor view:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a><span data-ttu-id="8884e-150">`asp-prerender-module`標記協助程式</span><span class="sxs-lookup"><span data-stu-id="8884e-150">The `asp-prerender-module` Tag Helper</span></span>

<span data-ttu-id="8884e-151">`asp-prerender-module`標記協助程式，使用在上述程式碼範例中，執行*ClientApp/dist/main-server.js*透過 Node.js 伺服器上。</span><span class="sxs-lookup"><span data-stu-id="8884e-151">The `asp-prerender-module` Tag Helper, used in the preceding code example, executes *ClientApp/dist/main-server.js* on the server via Node.js.</span></span> <span data-ttu-id="8884e-152">為了清楚起見， *main 立即轉譯 server.js*檔案是 TypeScript-JavaScript transpilation 工作中的成品[Webpack](http://webpack.github.io/)建置程序。</span><span class="sxs-lookup"><span data-stu-id="8884e-152">For clarity's sake, *main-server.js* file is an artifact of the TypeScript-to-JavaScript transpilation task in the [Webpack](http://webpack.github.io/) build process.</span></span> <span data-ttu-id="8884e-153">Webpack 定義的項目點別名`main-server`; 和此別名的相依性圖表的周遊開始*ClientApp/開機-server.ts*檔案：</span><span class="sxs-lookup"><span data-stu-id="8884e-153">Webpack defines an entry point alias of `main-server`; and, traversal of the dependency graph for this alias begins at the *ClientApp/boot-server.ts* file:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

<span data-ttu-id="8884e-154">在下列的 Angular 範例中， *ClientApp/開機-server.ts*檔案會利用`createServerRenderer`函式和`RenderResult`類型`aspnet-prerendering`設定伺服器轉譯透過 Node.js 的 npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="8884e-154">In the following Angular example, the *ClientApp/boot-server.ts* file utilizes the `createServerRenderer` function and `RenderResult` type of the `aspnet-prerendering` npm package to configure server rendering via Node.js.</span></span> <span data-ttu-id="8884e-155">預定要給伺服器端轉譯會傳遞至解析函式呼叫，包裝強型別在 JavaScript 中的 HTML 標記`Promise`物件。</span><span class="sxs-lookup"><span data-stu-id="8884e-155">The HTML markup destined for server-side rendering is passed to a resolve function call, which is wrapped in a strongly-typed JavaScript `Promise` object.</span></span> <span data-ttu-id="8884e-156">`Promise`物件的重要性是會以非同步方式提供要在 DOM 的預留位置項目插入頁面的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="8884e-156">The `Promise` object's significance is that it asynchronously supplies the HTML markup to the page for injection in the DOM's placeholder element.</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a><span data-ttu-id="8884e-157">`asp-prerender-data`標記協助程式</span><span class="sxs-lookup"><span data-stu-id="8884e-157">The `asp-prerender-data` Tag Helper</span></span>

<span data-ttu-id="8884e-158">如果結合`asp-prerender-module`標記 Helper`asp-prerender-data`標記協助程式可以用來從 Razor 檢視的內容資訊傳遞至伺服器端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="8884e-158">When coupled with the `asp-prerender-module` Tag Helper, the `asp-prerender-data` Tag Helper can be used to pass contextual information from the Razor view to the server-side JavaScript.</span></span> <span data-ttu-id="8884e-159">例如，下列標記使用者會將資料傳遞至`main-server`模組：</span><span class="sxs-lookup"><span data-stu-id="8884e-159">For example, the following markup passes user data to the `main-server` module:</span></span>

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

<span data-ttu-id="8884e-160">接收`UserName`引數會使用內建的 JSON 序列化程式序列化並儲存在`params.data`物件。</span><span class="sxs-lookup"><span data-stu-id="8884e-160">The received `UserName` argument is serialized using the built-in JSON serializer and is stored in the `params.data` object.</span></span> <span data-ttu-id="8884e-161">在下列的角度範例中，資料用來建構內的個人化的問候語`h1`項目：</span><span class="sxs-lookup"><span data-stu-id="8884e-161">In the following Angular example, the data is used to construct a personalized greeting within an `h1` element:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

<span data-ttu-id="8884e-162">注意： 使用表示屬性名稱中標記協助程式傳遞**PascalCase**標記法。</span><span class="sxs-lookup"><span data-stu-id="8884e-162">Note: Property names passed in Tag Helpers are represented with **PascalCase** notation.</span></span> <span data-ttu-id="8884e-163">Javascript 中，會使用以表示相同的屬性名稱相**camelCase**。</span><span class="sxs-lookup"><span data-stu-id="8884e-163">Contrast that to JavaScript, where the same property names are represented with **camelCase**.</span></span> <span data-ttu-id="8884e-164">預設的 JSON 序列化組態會負責這項差異。</span><span class="sxs-lookup"><span data-stu-id="8884e-164">The default JSON serialization configuration is responsible for this difference.</span></span>

<span data-ttu-id="8884e-165">若要展開在上述程式碼範例時，資料可傳遞從伺服器到檢視的 hydrating`globals`屬性提供給`resolve`函式：</span><span class="sxs-lookup"><span data-stu-id="8884e-165">To expand upon the preceding code example, data can be passed from the server to the view by hydrating the `globals` property provided to the `resolve` function:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

<span data-ttu-id="8884e-166">`postList`陣列內定義`globals`物件附加至瀏覽器的全域`window`物件。</span><span class="sxs-lookup"><span data-stu-id="8884e-166">The `postList` array defined inside the `globals` object is attached to the browser's global `window` object.</span></span> <span data-ttu-id="8884e-167">此變數 hoisting 全域範圍排除重複的工作，尤其是它適用於載入相同的資料一次在伺服器上一次用戶端上。</span><span class="sxs-lookup"><span data-stu-id="8884e-167">This variable hoisting to global scope eliminates duplication of effort, particularly as it pertains to loading the same data once on the server and again on the client.</span></span>

![附加至視窗物件的全域 postList 變數](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a><span data-ttu-id="8884e-169">Webpack Dev 中介軟體</span><span class="sxs-lookup"><span data-stu-id="8884e-169">Webpack Dev Middleware</span></span>

<span data-ttu-id="8884e-170">[中介軟體開發人員 」 Webpack](https://webpack.github.io/docs/webpack-dev-middleware.html)引入 Webpack 隨組建資源的藉此簡化的開發工作流程。</span><span class="sxs-lookup"><span data-stu-id="8884e-170">[Webpack Dev Middleware](https://webpack.github.io/docs/webpack-dev-middleware.html) introduces a streamlined development workflow whereby Webpack builds resources on demand.</span></span> <span data-ttu-id="8884e-171">中介軟體會自動編譯和瀏覽器中重新載入頁面時，提供用戶端的資源。</span><span class="sxs-lookup"><span data-stu-id="8884e-171">The middleware automatically compiles and serves client-side resources when a page is reloaded in the browser.</span></span> <span data-ttu-id="8884e-172">替代方法是手動透過叫用 Webpack 專案的 npm 組建指令碼的第三方相依性或自訂程式碼變更時。</span><span class="sxs-lookup"><span data-stu-id="8884e-172">The alternate approach is to manually invoke Webpack via the project's npm build script when a third-party dependency or the custom code changes.</span></span> <span data-ttu-id="8884e-173">Npm 建置指令碼*package.json*檔案在下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="8884e-173">An npm build script in the *package.json* file is shown in the following example:</span></span>

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a><span data-ttu-id="8884e-174">必要條件</span><span class="sxs-lookup"><span data-stu-id="8884e-174">Prerequisites</span></span>

<span data-ttu-id="8884e-175">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="8884e-175">Install the following:</span></span>
* <span data-ttu-id="8884e-176">[aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm 封裝：</span><span class="sxs-lookup"><span data-stu-id="8884e-176">[aspnet-webpack](https://www.npmjs.com/package/aspnet-webpack) npm package:</span></span>

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a><span data-ttu-id="8884e-177">組態</span><span class="sxs-lookup"><span data-stu-id="8884e-177">Configuration</span></span>

<span data-ttu-id="8884e-178">Webpack Dev 中介軟體已註冊至 HTTP 要求管線中的下列程式碼透過*Startup.cs*檔案的`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="8884e-178">Webpack Dev Middleware is registered into the HTTP request pipeline via the following code in the *Startup.cs* file's `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

<span data-ttu-id="8884e-179">`UseWebpackDevMiddleware`之前必須先呼叫擴充方法[註冊靜態檔案裝載](xref:fundamentals/static-files)透過`UseStaticFiles`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8884e-179">The `UseWebpackDevMiddleware` extension method must be called before [registering static file hosting](xref:fundamentals/static-files) via the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="8884e-180">基於安全性理由，在開發模式中執行的應用程式時，才登錄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8884e-180">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="8884e-181">*Webpack.config.js*檔案的`output.publicPath`屬性會告知監看的中介軟體`dist`資料夾是否有變更：</span><span class="sxs-lookup"><span data-stu-id="8884e-181">The *webpack.config.js* file's `output.publicPath` property tells the middleware to watch the `dist` folder for changes:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a><span data-ttu-id="8884e-182">熱模組更換</span><span class="sxs-lookup"><span data-stu-id="8884e-182">Hot Module Replacement</span></span>

<span data-ttu-id="8884e-183">想像 Webpack 的[熱模組更換](https://webpack.js.org/concepts/hot-module-replacement/)(HMR) 功能的進化[Webpack Dev 中介軟體](#webpack-dev-middleware)。</span><span class="sxs-lookup"><span data-stu-id="8884e-183">Think of Webpack's [Hot Module Replacement](https://webpack.js.org/concepts/hot-module-replacement/) (HMR) feature as an evolution of [Webpack Dev Middleware](#webpack-dev-middleware).</span></span> <span data-ttu-id="8884e-184">HMR 完全相同的優點，但它進一步簡化開發工作流程自動編譯所做的變更之後更新頁面內容。</span><span class="sxs-lookup"><span data-stu-id="8884e-184">HMR introduces all the same benefits, but it further streamlines the development workflow by automatically updating page content after compiling the changes.</span></span> <span data-ttu-id="8884e-185">請勿混淆這個與重新整理瀏覽器中，這會干擾 SPA 的偵錯工作階段與目前記憶體中狀態。</span><span class="sxs-lookup"><span data-stu-id="8884e-185">Don't confuse this with a refresh of the browser, which would interfere with the current in-memory state and debugging session of the SPA.</span></span> <span data-ttu-id="8884e-186">沒有 Webpack Dev 中介軟體服務與瀏覽器中，這表示變更推送至瀏覽器之間的即時連結。</span><span class="sxs-lookup"><span data-stu-id="8884e-186">There's a live link between the Webpack Dev Middleware service and the browser, which means changes are pushed to the browser.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8884e-187">必要條件</span><span class="sxs-lookup"><span data-stu-id="8884e-187">Prerequisites</span></span>

<span data-ttu-id="8884e-188">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="8884e-188">Install the following:</span></span>
* <span data-ttu-id="8884e-189">[webpack 熱的中介軟體](https://www.npmjs.com/package/webpack-hot-middleware)npm 封裝：</span><span class="sxs-lookup"><span data-stu-id="8884e-189">[webpack-hot-middleware](https://www.npmjs.com/package/webpack-hot-middleware) npm package:</span></span>

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a><span data-ttu-id="8884e-190">組態</span><span class="sxs-lookup"><span data-stu-id="8884e-190">Configuration</span></span>

<span data-ttu-id="8884e-191">HMR 元件必須註冊到 MVC 的 HTTP 要求管線中`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="8884e-191">The HMR component must be registered into MVC's HTTP request pipeline in the `Configure` method:</span></span>

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

<span data-ttu-id="8884e-192">為已具有 true [Webpack Dev 中介軟體](#webpack-dev-middleware)、`UseWebpackDevMiddleware`之前必須先呼叫擴充方法`UseStaticFiles`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="8884e-192">As was true with [Webpack Dev Middleware](#webpack-dev-middleware), the `UseWebpackDevMiddleware` extension method must be called before the `UseStaticFiles` extension method.</span></span> <span data-ttu-id="8884e-193">基於安全性理由，在開發模式中執行的應用程式時，才登錄中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8884e-193">For security reasons, register the middleware only when the app runs in development mode.</span></span>

<span data-ttu-id="8884e-194">*Webpack.config.js*檔案必須定義`plugins`陣列，即使它空白：</span><span class="sxs-lookup"><span data-stu-id="8884e-194">The *webpack.config.js* file must define a `plugins` array, even if it's left empty:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

<span data-ttu-id="8884e-195">載入瀏覽器中的應用程式之後, 的開發人員工具主控台 索引標籤會提供確認 HMR 啟用：</span><span class="sxs-lookup"><span data-stu-id="8884e-195">After loading the app in the browser, the developer tools' Console tab provides confirmation of HMR activation:</span></span>

![熱模組更換連接的訊息](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a><span data-ttu-id="8884e-197">路由的協助程式</span><span class="sxs-lookup"><span data-stu-id="8884e-197">Routing helpers</span></span>

<span data-ttu-id="8884e-198">在大部分的 ASP.NET 核心 SPAs，您需要路由除了路由伺服器端的用戶端。</span><span class="sxs-lookup"><span data-stu-id="8884e-198">In most ASP.NET Core-based SPAs, you'll want client-side routing in addition to server-side routing.</span></span> <span data-ttu-id="8884e-199">SPA 和 MVC 路由系統可以不受干擾的獨立工作。</span><span class="sxs-lookup"><span data-stu-id="8884e-199">The SPA and MVC routing systems can work independently without interference.</span></span> <span data-ttu-id="8884e-200">不過，一個邊緣案例一起集思廣益面臨的挑戰，是： 識別 404 HTTP 回應。</span><span class="sxs-lookup"><span data-stu-id="8884e-200">There's, however, one edge case posing challenges: identifying 404 HTTP responses.</span></span>

<span data-ttu-id="8884e-201">請考慮案例，其中無副檔名的路由`/some/page`用。</span><span class="sxs-lookup"><span data-stu-id="8884e-201">Consider the scenario in which an extensionless route of `/some/page` is used.</span></span> <span data-ttu-id="8884e-202">假設要求不會對伺服器端路由，但它的模式比對用戶端路由。</span><span class="sxs-lookup"><span data-stu-id="8884e-202">Assume the request doesn't pattern-match a server-side route, but its pattern does match a client-side route.</span></span> <span data-ttu-id="8884e-203">現在請思考一下的連入要求`/images/user-512.png`，這通常會預期找到映像檔在伺服器上的。</span><span class="sxs-lookup"><span data-stu-id="8884e-203">Now consider an incoming request for `/images/user-512.png`, which generally expects to find an image file on the server.</span></span> <span data-ttu-id="8884e-204">如果該要求的資源路徑不符合任何伺服器端路由或靜態檔案，也不太可能用戶端應用程式會處理它，您通常想要傳回 404 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="8884e-204">If that requested resource path doesn't match any server-side route or static file, it's unlikely that the client-side application would handle it — you generally want to return a 404 HTTP status code.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8884e-205">必要條件</span><span class="sxs-lookup"><span data-stu-id="8884e-205">Prerequisites</span></span>

<span data-ttu-id="8884e-206">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="8884e-206">Install the following:</span></span>
* <span data-ttu-id="8884e-207">用戶端路由 npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="8884e-207">The client-side routing npm package.</span></span> <span data-ttu-id="8884e-208">使用 Angular 做為範例：</span><span class="sxs-lookup"><span data-stu-id="8884e-208">Using Angular as an example:</span></span>

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a><span data-ttu-id="8884e-209">組態</span><span class="sxs-lookup"><span data-stu-id="8884e-209">Configuration</span></span>

<span data-ttu-id="8884e-210">擴充方法，名為`MapSpaFallbackRoute`用於`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="8884e-210">An extension method named `MapSpaFallbackRoute` is used in the `Configure` method:</span></span>

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

<span data-ttu-id="8884e-211">提示： 路由設定的順序會進行評估。</span><span class="sxs-lookup"><span data-stu-id="8884e-211">Tip: Routes are evaluated in the order in which they're configured.</span></span> <span data-ttu-id="8884e-212">因此，`default`用於模式比對第一次使用上述的程式碼範例中的路由。</span><span class="sxs-lookup"><span data-stu-id="8884e-212">Consequently, the `default` route in the preceding code example is used first for pattern matching.</span></span>

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a><span data-ttu-id="8884e-213">建立新的專案</span><span class="sxs-lookup"><span data-stu-id="8884e-213">Creating a new project</span></span>

<span data-ttu-id="8884e-214">JavaScriptServices 提供預先設定的應用程式範本。</span><span class="sxs-lookup"><span data-stu-id="8884e-214">JavaScriptServices provides pre-configured application templates.</span></span> <span data-ttu-id="8884e-215">在這些範本，搭配不同的架構和程式庫，例如 Angular、 React 和 Redux 用於 SpaServices。</span><span class="sxs-lookup"><span data-stu-id="8884e-215">SpaServices is used in these templates, in conjunction with different frameworks and libraries such as Angular, React, and Redux.</span></span>

<span data-ttu-id="8884e-216">這些範本可以透過.NET Core CLI 安裝，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8884e-216">These templates can be installed via the .NET Core CLI by running the following command:</span></span>

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

<span data-ttu-id="8884e-217">會顯示可用 SPA 範本的清單：</span><span class="sxs-lookup"><span data-stu-id="8884e-217">A list of available SPA templates is displayed:</span></span>

| <span data-ttu-id="8884e-218">範本</span><span class="sxs-lookup"><span data-stu-id="8884e-218">Templates</span></span>                                 | <span data-ttu-id="8884e-219">簡短名稱</span><span class="sxs-lookup"><span data-stu-id="8884e-219">Short Name</span></span> | <span data-ttu-id="8884e-220">語言</span><span class="sxs-lookup"><span data-stu-id="8884e-220">Language</span></span> | <span data-ttu-id="8884e-221">Tags</span><span class="sxs-lookup"><span data-stu-id="8884e-221">Tags</span></span>        |
|:------------------------------------------|:-----------|:---------|:------------|
| <span data-ttu-id="8884e-222">MVC ASP.NET Core 與角度</span><span class="sxs-lookup"><span data-stu-id="8884e-222">MVC ASP.NET Core with Angular</span></span>             | <span data-ttu-id="8884e-223">angular</span><span class="sxs-lookup"><span data-stu-id="8884e-223">angular</span></span>    | <span data-ttu-id="8884e-224">[C#]</span><span class="sxs-lookup"><span data-stu-id="8884e-224">[C#]</span></span>     | <span data-ttu-id="8884e-225">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="8884e-225">Web/MVC/SPA</span></span> |
| <span data-ttu-id="8884e-226">MVC ASP.NET Core 與 React.js</span><span class="sxs-lookup"><span data-stu-id="8884e-226">MVC ASP.NET Core with React.js</span></span>            | <span data-ttu-id="8884e-227">react</span><span class="sxs-lookup"><span data-stu-id="8884e-227">react</span></span>      | <span data-ttu-id="8884e-228">[C#]</span><span class="sxs-lookup"><span data-stu-id="8884e-228">[C#]</span></span>     | <span data-ttu-id="8884e-229">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="8884e-229">Web/MVC/SPA</span></span> |
| <span data-ttu-id="8884e-230">MVC ASP.NET Core React.js 和 Redux</span><span class="sxs-lookup"><span data-stu-id="8884e-230">MVC ASP.NET Core with React.js and Redux</span></span>  | <span data-ttu-id="8884e-231">reactredux</span><span class="sxs-lookup"><span data-stu-id="8884e-231">reactredux</span></span> | <span data-ttu-id="8884e-232">[C#]</span><span class="sxs-lookup"><span data-stu-id="8884e-232">[C#]</span></span>     | <span data-ttu-id="8884e-233">Web/MVC/SPA</span><span class="sxs-lookup"><span data-stu-id="8884e-233">Web/MVC/SPA</span></span> |

<span data-ttu-id="8884e-234">若要建立新專案使用其中一個 SPA 範本時，包含**簡短名稱**中的範本[dotnet 新](/dotnet/core/tools/dotnet-new)命令。</span><span class="sxs-lookup"><span data-stu-id="8884e-234">To create a new project using one of the SPA templates, include the **Short Name** of the template in the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="8884e-235">下列命令會建立與伺服器端設定的 ASP.NET Core MVC 角度的應用程式：</span><span class="sxs-lookup"><span data-stu-id="8884e-235">The following command creates an Angular application with ASP.NET Core MVC configured for the server side:</span></span>

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a><span data-ttu-id="8884e-236">設定執行階段組態模式</span><span class="sxs-lookup"><span data-stu-id="8884e-236">Set the runtime configuration mode</span></span>

<span data-ttu-id="8884e-237">有兩種主要的執行階段組態模式：</span><span class="sxs-lookup"><span data-stu-id="8884e-237">Two primary runtime configuration modes exist:</span></span>
* <span data-ttu-id="8884e-238">**開發**:</span><span class="sxs-lookup"><span data-stu-id="8884e-238">**Development**:</span></span>
    * <span data-ttu-id="8884e-239">包含來源對應，以便偵錯。</span><span class="sxs-lookup"><span data-stu-id="8884e-239">Includes source maps to ease debugging.</span></span>
    * <span data-ttu-id="8884e-240">不最佳化效能的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="8884e-240">Doesn't optimize the client-side code for performance.</span></span>
* <span data-ttu-id="8884e-241">**生產**:</span><span class="sxs-lookup"><span data-stu-id="8884e-241">**Production**:</span></span>
    * <span data-ttu-id="8884e-242">排除來源對應。</span><span class="sxs-lookup"><span data-stu-id="8884e-242">Excludes source maps.</span></span>
    * <span data-ttu-id="8884e-243">最佳化透過組合和縮製的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="8884e-243">Optimizes the client-side code via bundling & minification.</span></span>

<span data-ttu-id="8884e-244">ASP.NET Core 使用名為環境變數`ASPNETCORE_ENVIRONMENT`來儲存組態模式。</span><span class="sxs-lookup"><span data-stu-id="8884e-244">ASP.NET Core uses an environment variable named `ASPNETCORE_ENVIRONMENT` to store the configuration mode.</span></span> <span data-ttu-id="8884e-245">請參閱**[設定環境](xref:fundamentals/environments#setting-the-environment)** 如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="8884e-245">See **[Setting the environment](xref:fundamentals/environments#setting-the-environment)** for more information.</span></span>

### <a name="running-with-net-core-cli"></a><span data-ttu-id="8884e-246">執行.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="8884e-246">Running with .NET Core CLI</span></span>

<span data-ttu-id="8884e-247">還原所需的 NuGet 及 npm 封裝在專案的根目錄執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8884e-247">Restore the required NuGet and npm packages by running the following command at the project root:</span></span>

```console
dotnet restore && npm i
```

<span data-ttu-id="8884e-248">建置並執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="8884e-248">Build and run the application:</span></span>

```console
dotnet run
```

<span data-ttu-id="8884e-249">根據本機主機上的應用程式啟動[執行階段組態模式](#runtime-config-mode)。</span><span class="sxs-lookup"><span data-stu-id="8884e-249">The application starts on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> <span data-ttu-id="8884e-250">瀏覽至`http://localhost:5000`瀏覽器中顯示 登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="8884e-250">Navigating to `http://localhost:5000` in the browser displays the landing page.</span></span>

### <a name="running-with-visual-studio-2017"></a><span data-ttu-id="8884e-251">使用 Visual Studio 2017 執行</span><span class="sxs-lookup"><span data-stu-id="8884e-251">Running with Visual Studio 2017</span></span>

<span data-ttu-id="8884e-252">開啟 *.csproj*所產生的檔案[dotnet 新](/dotnet/core/tools/dotnet-new)命令。</span><span class="sxs-lookup"><span data-stu-id="8884e-252">Open the *.csproj* file generated by the [dotnet new](/dotnet/core/tools/dotnet-new) command.</span></span> <span data-ttu-id="8884e-253">在專案開啟時自動還原必要的 NuGet 及 npm 封裝。</span><span class="sxs-lookup"><span data-stu-id="8884e-253">The required NuGet and npm packages are restored automatically upon project open.</span></span> <span data-ttu-id="8884e-254">此還原程序可能需要幾分鐘的時間，和應用程式已準備好完成時執行。</span><span class="sxs-lookup"><span data-stu-id="8884e-254">This restoration process may take up to a few minutes, and the application is ready to run when it completes.</span></span> <span data-ttu-id="8884e-255">按一下綠色執行的按鈕或按`Ctrl + F5`，和瀏覽器會開啟應用程式的登陸頁面。</span><span class="sxs-lookup"><span data-stu-id="8884e-255">Click the green run button or press `Ctrl + F5`, and the browser opens to the application's landing page.</span></span> <span data-ttu-id="8884e-256">根據本機主機上執行的應用程式[執行階段組態模式](#runtime-config-mode)。</span><span class="sxs-lookup"><span data-stu-id="8884e-256">The application runs on localhost according to the [runtime configuration mode](#runtime-config-mode).</span></span> 

<a name="app-testing"></a>

## <a name="testing-the-app"></a><span data-ttu-id="8884e-257">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="8884e-257">Testing the app</span></span>

<span data-ttu-id="8884e-258">SpaServices 範本是預先設定為執行用戶端測試使用[Karma](https://karma-runner.github.io/1.0/index.html)和[Jasmine](https://jasmine.github.io/)。</span><span class="sxs-lookup"><span data-stu-id="8884e-258">SpaServices templates are pre-configured to run client-side tests using [Karma](https://karma-runner.github.io/1.0/index.html) and [Jasmine](https://jasmine.github.io/).</span></span> <span data-ttu-id="8884e-259">Jasmine 是熱門單元測試架構適用 JavaScript 的而 Karma 是這些測試的測試執行器。</span><span class="sxs-lookup"><span data-stu-id="8884e-259">Jasmine is a popular unit testing framework for JavaScript, whereas Karma is a test runner for those tests.</span></span> <span data-ttu-id="8884e-260">Karma 設為搭配[Webpack Dev 中介軟體](#webpack-dev-middleware)，開發人員不需要停止並執行測試，每次進行變更。</span><span class="sxs-lookup"><span data-stu-id="8884e-260">Karma is configured to work with the [Webpack Dev Middleware](#webpack-dev-middleware) such that the developer isn't required to stop and run the test every time changes are made.</span></span> <span data-ttu-id="8884e-261">是否為測試案例或測試案例本身上執行的程式碼，會自動執行測試。</span><span class="sxs-lookup"><span data-stu-id="8884e-261">Whether it's the code running against the test case or the test case itself, the test runs automatically.</span></span>

<span data-ttu-id="8884e-262">使用 Angular 的應用程式，例如，兩個 Jasmine 測試案例已提供`CounterComponent`中*counter.component.spec.ts*檔案：</span><span class="sxs-lookup"><span data-stu-id="8884e-262">Using the Angular application as an example, two Jasmine test cases are already provided for the `CounterComponent` in the *counter.component.spec.ts* file:</span></span>

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

<span data-ttu-id="8884e-263">開啟命令提示字元中*ClientApp*目錄。</span><span class="sxs-lookup"><span data-stu-id="8884e-263">Open the command prompt in the *ClientApp* directory.</span></span> <span data-ttu-id="8884e-264">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="8884e-264">Run the following command:</span></span>

```console
npm test
```

<span data-ttu-id="8884e-265">指令碼會啟動 Karma 測試執行器，讀取中定義的設定*karma.conf.js*檔案。</span><span class="sxs-lookup"><span data-stu-id="8884e-265">The script launches the Karma test runner, which reads the settings defined in the *karma.conf.js* file.</span></span> <span data-ttu-id="8884e-266">在其他設定， *karma.conf.js*識別測試檔案執行透過其`files`陣列：</span><span class="sxs-lookup"><span data-stu-id="8884e-266">Among other settings, the *karma.conf.js* identifies the test files to be executed via its `files` array:</span></span>

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a><span data-ttu-id="8884e-267">發行應用程式</span><span class="sxs-lookup"><span data-stu-id="8884e-267">Publishing the application</span></span>

<span data-ttu-id="8884e-268">將產生的用戶端資產和已發行的 ASP.NET Core 成品結合成已備妥部署的封裝可能會相當繁雜。</span><span class="sxs-lookup"><span data-stu-id="8884e-268">Combining the generated client-side assets and the published ASP.NET Core artifacts into a ready-to-deploy package can be cumbersome.</span></span> <span data-ttu-id="8884e-269">幸好 SpaServices 協調整個發行集程序與自訂的 MSBuild 目標，名為`RunWebpack`:</span><span class="sxs-lookup"><span data-stu-id="8884e-269">Thankfully, SpaServices orchestrates that entire publication process with a custom MSBuild target named `RunWebpack`:</span></span>

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

<span data-ttu-id="8884e-270">MSBuild 目標具有下列責任：</span><span class="sxs-lookup"><span data-stu-id="8884e-270">The MSBuild target has the following responsibilities:</span></span>
1. <span data-ttu-id="8884e-271">還原 npm 封裝</span><span class="sxs-lookup"><span data-stu-id="8884e-271">Restore the npm packages</span></span>
1. <span data-ttu-id="8884e-272">建立生產等級的組建的協力廠商的用戶端資產</span><span class="sxs-lookup"><span data-stu-id="8884e-272">Create a production-grade build of the third-party, client-side assets</span></span>
1. <span data-ttu-id="8884e-273">建立自訂用戶端資產的實際執行等級的組建</span><span class="sxs-lookup"><span data-stu-id="8884e-273">Create a production-grade build of the custom client-side assets</span></span>
1. <span data-ttu-id="8884e-274">複製到發佈資料夾的 Webpack 產生的資產</span><span class="sxs-lookup"><span data-stu-id="8884e-274">Copy the Webpack-generated assets to the publish folder</span></span>

<span data-ttu-id="8884e-275">執行時，會叫用 MSBuild 目標：</span><span class="sxs-lookup"><span data-stu-id="8884e-275">The MSBuild target is invoked when running:</span></span>

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a><span data-ttu-id="8884e-276">其他資源</span><span class="sxs-lookup"><span data-stu-id="8884e-276">Additional resources</span></span>

* [<span data-ttu-id="8884e-277">角度的文件</span><span class="sxs-lookup"><span data-stu-id="8884e-277">Angular Docs</span></span>](https://angular.io/docs)
