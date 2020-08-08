---
title: 防止跨網站偽造要求 (XSRF/CSRF) 攻擊 ASP.NET Core
author: steve-smith
description: 探索如何防範惡意網站可能會影響用戶端瀏覽器與應用程式之間互動的 web 應用程式攻擊。
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
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
uid: security/anti-request-forgery
ms.openlocfilehash: cc6f7c7e6692224f537f5eeba50b214aa84029db
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88018828"
---
# <a name="prevent-cross-site-request-forgery-xsrfcsrf-attacks-in-aspnet-core"></a><span data-ttu-id="e38bd-103">防止跨網站偽造要求 (XSRF/CSRF) 攻擊 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e38bd-103">Prevent Cross-Site Request Forgery (XSRF/CSRF) attacks in ASP.NET Core</span></span>

<span data-ttu-id="e38bd-104">作者： [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Fiyaz Hasan](https://twitter.com/FiyazBinHasan)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e38bd-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Fiyaz Hasan](https://twitter.com/FiyazBinHasan), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="e38bd-105">跨網站要求偽造 (也稱為 XSRF 或 CSRF) 是對 web 裝載應用程式的攻擊，因此惡意的 web 應用程式可能會影響用戶端瀏覽器與信任該瀏覽器之 web 應用程式之間的互動。</span><span class="sxs-lookup"><span data-stu-id="e38bd-105">Cross-site request forgery (also known as XSRF or CSRF) is an attack against web-hosted apps whereby a malicious web app can influence the interaction between a client browser and a web app that trusts that browser.</span></span> <span data-ttu-id="e38bd-106">這些攻擊是可行的，因為網頁瀏覽器會在每次要求網站時，自動傳送一些類型的驗證權杖。</span><span class="sxs-lookup"><span data-stu-id="e38bd-106">These attacks are possible because web browsers send some types of authentication tokens automatically with every request to a website.</span></span> <span data-ttu-id="e38bd-107">這種形式的惡意探索也稱為單鍵*攻擊*或*會話騎*，因為攻擊會利用使用者先前驗證的會話。</span><span class="sxs-lookup"><span data-stu-id="e38bd-107">This form of exploit is also known as a *one-click attack* or *session riding* because the attack takes advantage of the user's previously authenticated session.</span></span>

<span data-ttu-id="e38bd-108">CSRF 攻擊的範例：</span><span class="sxs-lookup"><span data-stu-id="e38bd-108">An example of a CSRF attack:</span></span>

1. <span data-ttu-id="e38bd-109">使用者 `www.good-banking-site.com` 使用表單驗證登入。</span><span class="sxs-lookup"><span data-stu-id="e38bd-109">A user signs into `www.good-banking-site.com` using forms authentication.</span></span> <span data-ttu-id="e38bd-110">伺服器會驗證使用者，併發出包含驗證的回應 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-110">The server authenticates the user and issues a response that includes an authentication cookie.</span></span> <span data-ttu-id="e38bd-111">網站很容易遭受攻擊，因為它會信任它以有效驗證所收到的任何要求 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-111">The site is vulnerable to attack because it trusts any request that it receives with a valid authentication cookie.</span></span>
1. <span data-ttu-id="e38bd-112">使用者造訪惡意網站 `www.bad-crook-site.com` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-112">The user visits a malicious site, `www.bad-crook-site.com`.</span></span>

   <span data-ttu-id="e38bd-113">惡意網站 `www.bad-crook-site.com` 包含類似下面的 HTML 表單：</span><span class="sxs-lookup"><span data-stu-id="e38bd-113">The malicious site, `www.bad-crook-site.com`, contains an HTML form similar to the following:</span></span>

   ```html
   <h1>Congratulations! You're a Winner!</h1>
   <form action="http://good-banking-site.com/api/account" method="post">
       <input type="hidden" name="Transaction" value="withdraw">
       <input type="hidden" name="Amount" value="1000000">
       <input type="submit" value="Click to collect your prize!">
   </form>
   ```

   <span data-ttu-id="e38bd-114">請注意，表單的 `action` 文章會張貼到易受攻擊的網站，而不是惡意網站。</span><span class="sxs-lookup"><span data-stu-id="e38bd-114">Notice that the form's `action` posts to the vulnerable site, not to the malicious site.</span></span> <span data-ttu-id="e38bd-115">這是 CSRF 的「跨網站」部分。</span><span class="sxs-lookup"><span data-stu-id="e38bd-115">This is the "cross-site" part of CSRF.</span></span>

1. <span data-ttu-id="e38bd-116">使用者選取 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e38bd-116">The user selects the submit button.</span></span> <span data-ttu-id="e38bd-117">瀏覽器會提出要求，並自動包含要求之網域的驗證 cookie `www.good-banking-site.com` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-117">The browser makes the request and automatically includes the authentication cookie for the requested domain, `www.good-banking-site.com`.</span></span>
1. <span data-ttu-id="e38bd-118">此要求 `www.good-banking-site.com` 會在伺服器上以使用者的驗證內容執行，並可執行已驗證使用者允許執行的任何動作。</span><span class="sxs-lookup"><span data-stu-id="e38bd-118">The request runs on the `www.good-banking-site.com` server with the user's authentication context and can perform any action that an authenticated user is allowed to perform.</span></span>

<span data-ttu-id="e38bd-119">除了使用者選取按鈕以提交表單的案例以外，惡意網站可能會：</span><span class="sxs-lookup"><span data-stu-id="e38bd-119">In addition to the scenario where the user selects the button to submit the form, the malicious site could:</span></span>

* <span data-ttu-id="e38bd-120">執行自動提交表單的腳本。</span><span class="sxs-lookup"><span data-stu-id="e38bd-120">Run a script that automatically submits the form.</span></span>
* <span data-ttu-id="e38bd-121">傳送表單提交做為 AJAX 要求。</span><span class="sxs-lookup"><span data-stu-id="e38bd-121">Send the form submission as an AJAX request.</span></span>
* <span data-ttu-id="e38bd-122">使用 CSS 隱藏表單。</span><span class="sxs-lookup"><span data-stu-id="e38bd-122">Hide the form using CSS.</span></span>

<span data-ttu-id="e38bd-123">這些替代案例不需要使用者一開始造訪惡意網站的任何動作或輸入。</span><span class="sxs-lookup"><span data-stu-id="e38bd-123">These alternative scenarios don't require any action or input from the user other than initially visiting the malicious site.</span></span>

<span data-ttu-id="e38bd-124">使用 HTTPS 並不會防止 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-124">Using HTTPS doesn't prevent a CSRF attack.</span></span> <span data-ttu-id="e38bd-125">惡意網站可以傳送要求， `https://www.good-banking-site.com/` 就像傳送不安全的要求一樣簡單。</span><span class="sxs-lookup"><span data-stu-id="e38bd-125">The malicious site can send an `https://www.good-banking-site.com/` request just as easily as it can send an insecure request.</span></span>

<span data-ttu-id="e38bd-126">某些攻擊會以回應 GET 要求的端點為目標，在此情況下，可以使用影像標記來執行動作。</span><span class="sxs-lookup"><span data-stu-id="e38bd-126">Some attacks target endpoints that respond to GET requests, in which case an image tag can be used to perform the action.</span></span> <span data-ttu-id="e38bd-127">這種形式的攻擊在允許影像但封鎖 JavaScript 的論壇網站上很常見。</span><span class="sxs-lookup"><span data-stu-id="e38bd-127">This form of attack is common on forum sites that permit images but block JavaScript.</span></span> <span data-ttu-id="e38bd-128">變更變數或資源的 GET 要求狀態的應用程式很容易遭受惡意攻擊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-128">Apps that change state on GET requests, where variables or resources are altered, are vulnerable to malicious attacks.</span></span> <span data-ttu-id="e38bd-129">**變更狀態的 GET 要求不安全。最佳做法是永遠不要變更 GET 要求的狀態。**</span><span class="sxs-lookup"><span data-stu-id="e38bd-129">**GET requests that change state are insecure. A best practice is to never change state on a GET request.**</span></span>

<span data-ttu-id="e38bd-130">針對使用進行驗證的 web 應用程式，可能會受到 CSRF 攻擊， cookie 因為：</span><span class="sxs-lookup"><span data-stu-id="e38bd-130">CSRF attacks are possible against web apps that use cookies for authentication because:</span></span>

* <span data-ttu-id="e38bd-131">cookieWeb 應用程式所發出的瀏覽器存放區。</span><span class="sxs-lookup"><span data-stu-id="e38bd-131">Browsers store cookies issued by a web app.</span></span>
* <span data-ttu-id="e38bd-132">儲存 cookie cookie 的包含已驗證使用者的會話。</span><span class="sxs-lookup"><span data-stu-id="e38bd-132">Stored cookies include session cookies for authenticated users.</span></span>
* <span data-ttu-id="e38bd-133">cookie無論應用程式在瀏覽器內產生的要求如何，瀏覽器都會將所有與網域相關聯的全部傳送至 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e38bd-133">Browsers send all of the cookies associated with a domain to the web app every request regardless of how the request to app was generated within the browser.</span></span>

<span data-ttu-id="e38bd-134">不過，CSRF 攻擊並不限於利用 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-134">However, CSRF attacks aren't limited to exploiting cookies.</span></span> <span data-ttu-id="e38bd-135">例如，基本和摘要式驗證也很容易受到攻擊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-135">For example, Basic and Digest authentication are also vulnerable.</span></span> <span data-ttu-id="e38bd-136">使用者使用基本或摘要式驗證登入之後，瀏覽器會自動傳送認證，直到會話 &dagger; 結束為止。</span><span class="sxs-lookup"><span data-stu-id="e38bd-136">After a user signs in with Basic or Digest authentication, the browser automatically sends the credentials until the session&dagger; ends.</span></span>

<span data-ttu-id="e38bd-137">&dagger;在此內容中，*會話*指的是使用者驗證期間的用戶端會話。</span><span class="sxs-lookup"><span data-stu-id="e38bd-137">&dagger;In this context, *session* refers to the client-side session during which the user is authenticated.</span></span> <span data-ttu-id="e38bd-138">它與伺服器端會話或[ASP.NET Core 會話中介軟體](xref:fundamentals/app-state)無關。</span><span class="sxs-lookup"><span data-stu-id="e38bd-138">It's unrelated to server-side sessions or [ASP.NET Core Session Middleware](xref:fundamentals/app-state).</span></span>

<span data-ttu-id="e38bd-139">使用者可以採取預防措施來防止 CSRF 的弱點：</span><span class="sxs-lookup"><span data-stu-id="e38bd-139">Users can guard against CSRF vulnerabilities by taking precautions:</span></span>

* <span data-ttu-id="e38bd-140">完成使用 web 應用程式後，請將其登出。</span><span class="sxs-lookup"><span data-stu-id="e38bd-140">Sign off of web apps when finished using them.</span></span>
* <span data-ttu-id="e38bd-141">定期清除瀏覽器 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-141">Clear browser cookies periodically.</span></span>

<span data-ttu-id="e38bd-142">不過，CSRF 弱點基本上是 web 應用程式的問題，而不是終端使用者。</span><span class="sxs-lookup"><span data-stu-id="e38bd-142">However, CSRF vulnerabilities are fundamentally a problem with the web app, not the end user.</span></span>

## <a name="authentication-fundamentals"></a><span data-ttu-id="e38bd-143">驗證基本概念</span><span class="sxs-lookup"><span data-stu-id="e38bd-143">Authentication fundamentals</span></span>

<span data-ttu-id="e38bd-144">Cookie以為基礎的驗證是一種常見的驗證形式。</span><span class="sxs-lookup"><span data-stu-id="e38bd-144">Cookie-based authentication is a popular form of authentication.</span></span> <span data-ttu-id="e38bd-145">以權杖為基礎的驗證系統日益普及，特別是針對單一頁面應用程式 (Spa) 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-145">Token-based authentication systems are growing in popularity, especially for Single Page Applications (SPAs).</span></span>

### <a name="no-loccookie-based-authentication"></a><span data-ttu-id="e38bd-146">Cookie以為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="e38bd-146">Cookie-based authentication</span></span>

<span data-ttu-id="e38bd-147">當使用者使用其使用者名稱和密碼進行驗證時，就會發出權杖，其中包含可用於驗證和授權的驗證票證。</span><span class="sxs-lookup"><span data-stu-id="e38bd-147">When a user authenticates using their username and password, they're issued a token, containing an authentication ticket that can be used for authentication and authorization.</span></span> <span data-ttu-id="e38bd-148">權杖會儲存為 cookie 每個用戶端提出的要求所隨附的。</span><span class="sxs-lookup"><span data-stu-id="e38bd-148">The token is stored as a cookie that accompanies every request the client makes.</span></span> <span data-ttu-id="e38bd-149">產生和驗證這項作業 cookie 是由 Cookie 驗證中介軟體執行。</span><span class="sxs-lookup"><span data-stu-id="e38bd-149">Generating and validating this cookie is performed by the Cookie Authentication Middleware.</span></span> <span data-ttu-id="e38bd-150">[中介軟體](xref:fundamentals/middleware/index)會將使用者主體序列化為加密的 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-150">The [middleware](xref:fundamentals/middleware/index) serializes a user principal into an encrypted cookie.</span></span> <span data-ttu-id="e38bd-151">在後續要求中，中介軟體會驗證、重新建立 cookie 主體，並將主體指派給[HttpCoNtext](/dotnet/api/microsoft.aspnetcore.http.httpcontext)的[User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)屬性。</span><span class="sxs-lookup"><span data-stu-id="e38bd-151">On subsequent requests, the middleware validates the cookie, recreates the principal, and assigns the principal to the [User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user) property of [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span>

### <a name="token-based-authentication"></a><span data-ttu-id="e38bd-152">權杖式驗證</span><span class="sxs-lookup"><span data-stu-id="e38bd-152">Token-based authentication</span></span>

<span data-ttu-id="e38bd-153">當使用者經過驗證後，就會發出權杖 (不是 antiforgery 權杖) 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-153">When a user is authenticated, they're issued a token (not an antiforgery token).</span></span> <span data-ttu-id="e38bd-154">權杖包含[宣告](/dotnet/framework/security/claims-based-identity-model)形式的使用者資訊或參考權杖，可將應用程式指向應用程式中維護的使用者狀態。</span><span class="sxs-lookup"><span data-stu-id="e38bd-154">The token contains user information in the form of [claims](/dotnet/framework/security/claims-based-identity-model) or a reference token that points the app to user state maintained in the app.</span></span> <span data-ttu-id="e38bd-155">當使用者嘗試存取需要驗證的資源時，權杖會以持有人權杖的形式，以額外的授權標頭傳送至應用程式。</span><span class="sxs-lookup"><span data-stu-id="e38bd-155">When a user attempts to access a resource requiring authentication, the token is sent to the app with an additional authorization header in form of Bearer token.</span></span> <span data-ttu-id="e38bd-156">這會讓應用程式無狀態。</span><span class="sxs-lookup"><span data-stu-id="e38bd-156">This makes the app stateless.</span></span> <span data-ttu-id="e38bd-157">在每個後續要求中，會在要求中傳遞權杖以進行伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="e38bd-157">In each subsequent request, the token is passed in the request for server-side validation.</span></span> <span data-ttu-id="e38bd-158">此權杖未*加密*;它會進行*編碼*。</span><span class="sxs-lookup"><span data-stu-id="e38bd-158">This token isn't *encrypted*; it's *encoded*.</span></span> <span data-ttu-id="e38bd-159">在伺服器上，權杖會解碼以存取其資訊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-159">On the server, the token is decoded to access its information.</span></span> <span data-ttu-id="e38bd-160">若要在後續要求中傳送權杖，請將權杖儲存在瀏覽器的本機儲存體中。</span><span class="sxs-lookup"><span data-stu-id="e38bd-160">To send the token on subsequent requests, store the token in the browser's local storage.</span></span> <span data-ttu-id="e38bd-161">如果權杖儲存在瀏覽器的本機儲存體中，請不要擔心 CSRF 弱點。</span><span class="sxs-lookup"><span data-stu-id="e38bd-161">Don't be concerned about CSRF vulnerability if the token is stored in the browser's local storage.</span></span> <span data-ttu-id="e38bd-162">當令牌儲存在中時，CSRF 是一項考慮 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-162">CSRF is a concern when the token is stored in a cookie.</span></span> <span data-ttu-id="e38bd-163">如需詳細資訊，請參閱 GitHub 問題[SPA 程式碼範例 cookie 會加入兩個](https://github.com/dotnet/AspNetCore.Docs/issues/13369)。</span><span class="sxs-lookup"><span data-stu-id="e38bd-163">For more information, see the GitHub issue [SPA code sample adds two cookies](https://github.com/dotnet/AspNetCore.Docs/issues/13369).</span></span>

### <a name="multiple-apps-hosted-at-one-domain"></a><span data-ttu-id="e38bd-164">裝載于一個網域的多個應用程式</span><span class="sxs-lookup"><span data-stu-id="e38bd-164">Multiple apps hosted at one domain</span></span>

<span data-ttu-id="e38bd-165">共用的裝載環境容易遭受會話劫持、登入 CSRF 和其他攻擊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-165">Shared hosting environments are vulnerable to session hijacking, login CSRF, and other attacks.</span></span>

<span data-ttu-id="e38bd-166">雖然 `example1.contoso.net` 和 `example2.contoso.net` 是不同的主機，但網域下的主機之間有隱含的信任關係 `*.contoso.net` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-166">Although `example1.contoso.net` and `example2.contoso.net` are different hosts, there's an implicit trust relationship between hosts under the `*.contoso.net` domain.</span></span> <span data-ttu-id="e38bd-167">這個隱含信任關係允許可能不受信任的主機影響彼此的， cookie (管理 AJAX 要求的相同原始原則不一定適用于 HTTP cookie s) 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-167">This implicit trust relationship allows potentially untrusted hosts to affect each other's cookies (the same-origin policies that govern AJAX requests don't necessarily apply to HTTP cookies).</span></span>

<span data-ttu-id="e38bd-168">在相同網域上裝載的應用程式之間，惡意探索受信任的攻擊， cookie 可以避免共用網域。</span><span class="sxs-lookup"><span data-stu-id="e38bd-168">Attacks that exploit trusted cookies between apps hosted on the same domain can be prevented by not sharing domains.</span></span> <span data-ttu-id="e38bd-169">當每個應用程式裝載于自己的網域時，就不會有隱含 cookie 信任關係可以利用。</span><span class="sxs-lookup"><span data-stu-id="e38bd-169">When each app is hosted on its own domain, there is no implicit cookie trust relationship to exploit.</span></span>

## <a name="aspnet-core-antiforgery-configuration"></a><span data-ttu-id="e38bd-170">ASP.NET Core antiforgery 設定</span><span class="sxs-lookup"><span data-stu-id="e38bd-170">ASP.NET Core antiforgery configuration</span></span>

> [!WARNING]
> <span data-ttu-id="e38bd-171">ASP.NET Core 使用 ASP.NET Core 的[資料保護](xref:security/data-protection/introduction)來執行 antiforgery。</span><span class="sxs-lookup"><span data-stu-id="e38bd-171">ASP.NET Core implements antiforgery using [ASP.NET Core Data Protection](xref:security/data-protection/introduction).</span></span> <span data-ttu-id="e38bd-172">資料保護堆疊必須設定為在伺服器陣列中工作。</span><span class="sxs-lookup"><span data-stu-id="e38bd-172">The data protection stack must be configured to work in a server farm.</span></span> <span data-ttu-id="e38bd-173">如需詳細資訊，請參閱設定[資料保護](xref:security/data-protection/configuration/overview)。</span><span class="sxs-lookup"><span data-stu-id="e38bd-173">See [Configuring data protection](xref:security/data-protection/configuration/overview) for more information.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e38bd-174">當下列其中一個 Api 呼叫時，會將 Antiforgery 中介軟體新增至相依性[插入](xref:fundamentals/dependency-injection)容器 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="e38bd-174">Antiforgery middleware is added to the [Dependency injection](xref:fundamentals/dependency-injection) container when one of the following APIs is called in `Startup.ConfigureServices`:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>
* <xref:Microsoft.AspNetCore.Builder.RazorPagesEndpointRouteBuilderExtensions.MapRazorPages*>
* <xref:Microsoft.AspNetCore.Builder.ControllerEndpointRouteBuilderExtensions.MapControllerRoute*>
* <xref:Microsoft.AspNetCore.Builder.ComponentEndpointRouteBuilderExtensions.MapBlazorHub*>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e38bd-175">呼叫中的時，Antiforgery 中介軟體會新增至相依性[插入](xref:fundamentals/dependency-injection)容器 <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*>`Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="e38bd-175">Antiforgery middleware is added to the [Dependency injection](xref:fundamentals/dependency-injection) container when <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddMvc*> is called in `Startup.ConfigureServices`</span></span>

::: moniker-end

<span data-ttu-id="e38bd-176">在 ASP.NET Core 2.0 或更新版本中， [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)會將 antiforgery TOKEN 插入 HTML 表單元素中。</span><span class="sxs-lookup"><span data-stu-id="e38bd-176">In ASP.NET Core 2.0 or later, the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span> <span data-ttu-id="e38bd-177">檔案中的下列標記 Razor 會自動產生 antiforgery 權杖：</span><span class="sxs-lookup"><span data-stu-id="e38bd-177">The following markup in a Razor file automatically generates antiforgery tokens:</span></span>

```cshtml
<form method="post">
    ...
</form>
```

<span data-ttu-id="e38bd-178">同樣地，如果無法取得表單的方法，則[IHtmlHelper](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform)會依預設產生 antiforgery token。</span><span class="sxs-lookup"><span data-stu-id="e38bd-178">Similarly, [IHtmlHelper.BeginForm](/dotnet/api/microsoft.aspnetcore.mvc.rendering.ihtmlhelper.beginform) generates antiforgery tokens by default if the form's method isn't GET.</span></span>

<span data-ttu-id="e38bd-179">當 `<form>` 標記包含 `method="post"` 屬性且下列其中一項為 true 時，就會自動產生 HTML 表單元素的 antiforgery token：</span><span class="sxs-lookup"><span data-stu-id="e38bd-179">The automatic generation of antiforgery tokens for HTML form elements happens when the `<form>` tag contains the `method="post"` attribute and either of the following are true:</span></span>

* <span data-ttu-id="e38bd-180"> () 的 action 屬性是空的 `action=""` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-180">The action attribute is empty (`action=""`).</span></span>
* <span data-ttu-id="e38bd-181"> () 未提供動作屬性 `<form method="post">` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-181">The action attribute isn't supplied (`<form method="post">`).</span></span>

<span data-ttu-id="e38bd-182">可以停用 HTML 表單元素的自動產生 antiforgery token：</span><span class="sxs-lookup"><span data-stu-id="e38bd-182">Automatic generation of antiforgery tokens for HTML form elements can be disabled:</span></span>

* <span data-ttu-id="e38bd-183">使用屬性明確停用 antiforgery token `asp-antiforgery` ：</span><span class="sxs-lookup"><span data-stu-id="e38bd-183">Explicitly disable antiforgery tokens with the `asp-antiforgery` attribute:</span></span>

  ```cshtml
  <form method="post" asp-antiforgery="false">
      ...
  </form>
  ```

* <span data-ttu-id="e38bd-184">Form 元素會使用標籤協助程式[！退出符號](xref:mvc/views/tag-helpers/intro#opt-out)來退出宣告標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="e38bd-184">The form element is opted-out of Tag Helpers by using the Tag Helper [! opt-out symbol](xref:mvc/views/tag-helpers/intro#opt-out):</span></span>

  ```cshtml
  <!form method="post">
      ...
  </!form>
  ```

* <span data-ttu-id="e38bd-185">`FormTagHelper`從視圖中移除。</span><span class="sxs-lookup"><span data-stu-id="e38bd-185">Remove the `FormTagHelper` from the view.</span></span> <span data-ttu-id="e38bd-186">`FormTagHelper`可以藉由將下列指示詞加入至視圖中，從視圖中移除 Razor ：</span><span class="sxs-lookup"><span data-stu-id="e38bd-186">The `FormTagHelper` can be removed from a view by adding the following directive to the Razor view:</span></span>

  ```cshtml
  @removeTagHelper Microsoft.AspNetCore.Mvc.TagHelpers.FormTagHelper, Microsoft.AspNetCore.Mvc.TagHelpers
  ```

> [!NOTE]
> <span data-ttu-id="e38bd-187">[ Razor 頁面](xref:razor-pages/index)會自動受到 XSRF/CSRF 的保護。</span><span class="sxs-lookup"><span data-stu-id="e38bd-187">[Razor Pages](xref:razor-pages/index) are automatically protected from XSRF/CSRF.</span></span> <span data-ttu-id="e38bd-188">如需詳細資訊，請參閱[XSRF/CSRF 和 Razor Pages](xref:razor-pages/index#xsrf)。</span><span class="sxs-lookup"><span data-stu-id="e38bd-188">For more information, see [XSRF/CSRF and Razor Pages](xref:razor-pages/index#xsrf).</span></span>

<span data-ttu-id="e38bd-189">防禦 CSRF 攻擊最常見的方法是使用*同步器權杖模式* (STP) 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-189">The most common approach to defending against CSRF attacks is to use the *Synchronizer Token Pattern* (STP).</span></span> <span data-ttu-id="e38bd-190">當使用者要求具有表單資料的頁面時，會使用 STP：</span><span class="sxs-lookup"><span data-stu-id="e38bd-190">STP is used when the user requests a page with form data:</span></span>

1. <span data-ttu-id="e38bd-191">伺服器會將與目前使用者的身分識別相關聯的權杖傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="e38bd-191">The server sends a token associated with the current user's identity to the client.</span></span>
1. <span data-ttu-id="e38bd-192">用戶端會將權杖傳回給伺服器以進行驗證。</span><span class="sxs-lookup"><span data-stu-id="e38bd-192">The client sends back the token to the server for verification.</span></span>
1. <span data-ttu-id="e38bd-193">如果伺服器收到的權杖不符合已驗證使用者的身分識別，則會拒絕該要求。</span><span class="sxs-lookup"><span data-stu-id="e38bd-193">If the server receives a token that doesn't match the authenticated user's identity, the request is rejected.</span></span>

<span data-ttu-id="e38bd-194">Token 是唯一且無法預測的。</span><span class="sxs-lookup"><span data-stu-id="e38bd-194">The token is unique and unpredictable.</span></span> <span data-ttu-id="e38bd-195">權杖也可以用來確保一系列要求的適當排序 (例如，確保：第1頁 > 第2頁 > 第3頁) 的要求順序。</span><span class="sxs-lookup"><span data-stu-id="e38bd-195">The token can also be used to ensure proper sequencing of a series of requests (for example, ensuring the request sequence of: page 1 > page 2 > page 3).</span></span> <span data-ttu-id="e38bd-196">ASP.NET Core MVC 和頁面範本中的所有表單都會 Razor 產生 antiforgery token。</span><span class="sxs-lookup"><span data-stu-id="e38bd-196">All of the forms in ASP.NET Core MVC and Razor Pages templates generate antiforgery tokens.</span></span> <span data-ttu-id="e38bd-197">下列對視圖範例會產生 antiforgery token：</span><span class="sxs-lookup"><span data-stu-id="e38bd-197">The following pair of view examples generate antiforgery tokens:</span></span>

```cshtml
<form asp-controller="Manage" asp-action="ChangePassword" method="post">
    ...
</form>

@using (Html.BeginForm("ChangePassword", "Manage"))
{
    ...
}
```

<span data-ttu-id="e38bd-198">將 antiforgery token 明確新增至專案， `<form>` 而不使用標記協助程式搭配 HTML helper [`@Html.AntiForgeryToken`](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken) ：</span><span class="sxs-lookup"><span data-stu-id="e38bd-198">Explicitly add an antiforgery token to a `<form>` element without using Tag Helpers with the HTML helper [`@Html.AntiForgeryToken`](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.htmlhelper.antiforgerytoken):</span></span>

```cshtml
<form action="/" method="post">
    @Html.AntiForgeryToken()
</form>
```

<span data-ttu-id="e38bd-199">在上述每個案例中，ASP.NET Core 新增如下所示的隱藏表單欄位：</span><span class="sxs-lookup"><span data-stu-id="e38bd-199">In each of the preceding cases, ASP.NET Core adds a hidden form field similar to the following:</span></span>

```cshtml
<input name="__RequestVerificationToken" type="hidden" value="CfDJ8NrAkS ... s2-m9Yw">
```

<span data-ttu-id="e38bd-200">ASP.NET Core 包含三個用於處理 antiforgery token 的[篩選](xref:mvc/controllers/filters)條件：</span><span class="sxs-lookup"><span data-stu-id="e38bd-200">ASP.NET Core includes three [filters](xref:mvc/controllers/filters) for working with antiforgery tokens:</span></span>

* [<span data-ttu-id="e38bd-201">ValidateAntiForgeryToken</span><span class="sxs-lookup"><span data-stu-id="e38bd-201">ValidateAntiForgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)
* [<span data-ttu-id="e38bd-202">AutoValidateAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="e38bd-202">AutoValidateAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)
* [<span data-ttu-id="e38bd-203">IgnoreAntiforgeryToken</span><span class="sxs-lookup"><span data-stu-id="e38bd-203">IgnoreAntiforgeryToken</span></span>](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)

## <a name="antiforgery-options"></a><span data-ttu-id="e38bd-204">Antiforgery 選項</span><span class="sxs-lookup"><span data-stu-id="e38bd-204">Antiforgery options</span></span>

<span data-ttu-id="e38bd-205">在中自訂[antiforgery 選項](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="e38bd-205">Customize [antiforgery options](/dotnet/api/Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions) in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    // Set Cookie properties using CookieBuilder properties†.
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.SuppressXFrameOptionsHeader = false;
});
```

<span data-ttu-id="e38bd-206">&dagger;`Cookie`使用[ Cookie Builder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder)類別的屬性來設定 antiforgery 屬性。</span><span class="sxs-lookup"><span data-stu-id="e38bd-206">&dagger;Set the antiforgery `Cookie` properties using the properties of the [CookieBuilder](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder) class.</span></span>

| <span data-ttu-id="e38bd-207">選項</span><span class="sxs-lookup"><span data-stu-id="e38bd-207">Option</span></span> | <span data-ttu-id="e38bd-208">描述</span><span class="sxs-lookup"><span data-stu-id="e38bd-208">Description</span></span> |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="e38bd-209">決定用來建立 antiforgery 的設定 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-209">Determines the settings used to create the antiforgery cookies.</span></span> |
| [<span data-ttu-id="e38bd-210">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="e38bd-210">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="e38bd-211">Antiforgery 系統用來轉譯 views 中 antiforgery 標記的隱藏表單欄位名稱。</span><span class="sxs-lookup"><span data-stu-id="e38bd-211">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="e38bd-212">HeaderName</span><span class="sxs-lookup"><span data-stu-id="e38bd-212">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="e38bd-213">Antiforgery 系統使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="e38bd-213">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="e38bd-214">如果 `null` 為，則系統只會考慮表單資料。</span><span class="sxs-lookup"><span data-stu-id="e38bd-214">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="e38bd-215">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="e38bd-215">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="e38bd-216">指定是否要隱藏 `X-Frame-Options` 標頭的產生。</span><span class="sxs-lookup"><span data-stu-id="e38bd-216">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="e38bd-217">根據預設，會產生值為 "SAMEORIGIN" 的標頭。</span><span class="sxs-lookup"><span data-stu-id="e38bd-217">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="e38bd-218">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="e38bd-218">Defaults to `false`.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
services.AddAntiforgery(options => 
{
    options.CookieDomain = "contoso.com";
    options.CookieName = "X-CSRF-TOKEN-COOKIENAME";
    options.CookiePath = "Path";
    options.FormFieldName = "AntiforgeryFieldname";
    options.HeaderName = "X-CSRF-TOKEN-HEADERNAME";
    options.RequireSsl = false;
    options.SuppressXFrameOptionsHeader = false;
});
```

| <span data-ttu-id="e38bd-219">選項</span><span class="sxs-lookup"><span data-stu-id="e38bd-219">Option</span></span> | <span data-ttu-id="e38bd-220">描述</span><span class="sxs-lookup"><span data-stu-id="e38bd-220">Description</span></span> |
| ------ | ----------- |
| [Cookie](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookie) | <span data-ttu-id="e38bd-221">決定用來建立 antiforgery 的設定 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-221">Determines the settings used to create the antiforgery cookies.</span></span> |
| <span data-ttu-id="e38bd-222">[CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain)</span><span class="sxs-lookup"><span data-stu-id="e38bd-222">[CookieDomain](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiedomain)</span></span> | <span data-ttu-id="e38bd-223">的網域 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-223">The domain of the cookie.</span></span> <span data-ttu-id="e38bd-224">預設值為 `null`。</span><span class="sxs-lookup"><span data-stu-id="e38bd-224">Defaults to `null`.</span></span> <span data-ttu-id="e38bd-225">這個屬性已經過時，將在未來的版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e38bd-225">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e38bd-226">建議的替代做法是 Cookie 。Domain.</span><span class="sxs-lookup"><span data-stu-id="e38bd-226">The recommended alternative is Cookie.Domain.</span></span> |
| <span data-ttu-id="e38bd-227">[Cookie名稱](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename)</span><span class="sxs-lookup"><span data-stu-id="e38bd-227">[CookieName](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiename)</span></span> | <span data-ttu-id="e38bd-228">cookie 的名稱。</span><span class="sxs-lookup"><span data-stu-id="e38bd-228">The name of the cookie.</span></span> <span data-ttu-id="e38bd-229">如果未設定，系統會產生開頭為[預設 Cookie 前置](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix)詞 ( "的唯一名稱。AspNetCore. Antiforgery. ") 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-229">If not set, the system generates a unique name beginning with the [DefaultCookiePrefix](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.defaultcookieprefix) (".AspNetCore.Antiforgery.").</span></span> <span data-ttu-id="e38bd-230">這個屬性已經過時，將在未來的版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e38bd-230">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e38bd-231">建議的替代做法是 Cookie 。檔案名.</span><span class="sxs-lookup"><span data-stu-id="e38bd-231">The recommended alternative is Cookie.Name.</span></span> |
| <span data-ttu-id="e38bd-232">[Cookie路徑名](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath)</span><span class="sxs-lookup"><span data-stu-id="e38bd-232">[CookiePath](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.cookiepath)</span></span> | <span data-ttu-id="e38bd-233">在上設定的路徑 cookie 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-233">The path set on the cookie.</span></span> <span data-ttu-id="e38bd-234">這個屬性已經過時，將在未來的版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e38bd-234">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e38bd-235">建議的替代做法是 Cookie 。路徑名.</span><span class="sxs-lookup"><span data-stu-id="e38bd-235">The recommended alternative is Cookie.Path.</span></span> |
| [<span data-ttu-id="e38bd-236">FormFieldName</span><span class="sxs-lookup"><span data-stu-id="e38bd-236">FormFieldName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.formfieldname) | <span data-ttu-id="e38bd-237">Antiforgery 系統用來轉譯 views 中 antiforgery 標記的隱藏表單欄位名稱。</span><span class="sxs-lookup"><span data-stu-id="e38bd-237">The name of the hidden form field used by the antiforgery system to render antiforgery tokens in views.</span></span> |
| [<span data-ttu-id="e38bd-238">HeaderName</span><span class="sxs-lookup"><span data-stu-id="e38bd-238">HeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.headername) | <span data-ttu-id="e38bd-239">Antiforgery 系統使用的標頭名稱。</span><span class="sxs-lookup"><span data-stu-id="e38bd-239">The name of the header used by the antiforgery system.</span></span> <span data-ttu-id="e38bd-240">如果 `null` 為，則系統只會考慮表單資料。</span><span class="sxs-lookup"><span data-stu-id="e38bd-240">If `null`, the system considers only form data.</span></span> |
| [<span data-ttu-id="e38bd-241">RequireSsl</span><span class="sxs-lookup"><span data-stu-id="e38bd-241">RequireSsl</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.requiressl) | <span data-ttu-id="e38bd-242">指定 antiforgery 系統是否需要 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="e38bd-242">Specifies whether HTTPS is required by the antiforgery system.</span></span> <span data-ttu-id="e38bd-243">如果 `true` 為，則非 HTTPS 要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="e38bd-243">If `true`, non-HTTPS requests fail.</span></span> <span data-ttu-id="e38bd-244">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="e38bd-244">Defaults to `false`.</span></span> <span data-ttu-id="e38bd-245">這個屬性已經過時，將在未來的版本中移除。</span><span class="sxs-lookup"><span data-stu-id="e38bd-245">This property is obsolete and will be removed in a future version.</span></span> <span data-ttu-id="e38bd-246">建議的替代做法是設定 Cookie 。SecurePolicy.</span><span class="sxs-lookup"><span data-stu-id="e38bd-246">The recommended alternative is to set Cookie.SecurePolicy.</span></span> |
| [<span data-ttu-id="e38bd-247">SuppressXFrameOptionsHeader</span><span class="sxs-lookup"><span data-stu-id="e38bd-247">SuppressXFrameOptionsHeader</span></span>](/dotnet/api/microsoft.aspnetcore.antiforgery.antiforgeryoptions.suppressxframeoptionsheader) | <span data-ttu-id="e38bd-248">指定是否要隱藏 `X-Frame-Options` 標頭的產生。</span><span class="sxs-lookup"><span data-stu-id="e38bd-248">Specifies whether to suppress generation of the `X-Frame-Options` header.</span></span> <span data-ttu-id="e38bd-249">根據預設，會產生值為 "SAMEORIGIN" 的標頭。</span><span class="sxs-lookup"><span data-stu-id="e38bd-249">By default, the header is generated with a value of "SAMEORIGIN".</span></span> <span data-ttu-id="e38bd-250">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="e38bd-250">Defaults to `false`.</span></span> |

::: moniker-end

<span data-ttu-id="e38bd-251">如需詳細資訊，請參閱[ Cookie AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions)。</span><span class="sxs-lookup"><span data-stu-id="e38bd-251">For more information, see [CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions).</span></span>

## <a name="configure-antiforgery-features-with-iantiforgery"></a><span data-ttu-id="e38bd-252">使用 IAntiforgery 設定 antiforgery 功能</span><span class="sxs-lookup"><span data-stu-id="e38bd-252">Configure antiforgery features with IAntiforgery</span></span>

<span data-ttu-id="e38bd-253">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)提供 API 來設定 antiforgery 功能。</span><span class="sxs-lookup"><span data-stu-id="e38bd-253">[IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) provides the API to configure antiforgery features.</span></span> <span data-ttu-id="e38bd-254">`IAntiforgery`可以在類別的方法中要求 `Configure` `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-254">`IAntiforgery` can be requested in the `Configure` method of the `Startup` class.</span></span> <span data-ttu-id="e38bd-255">下列範例會使用來自應用程式首頁的中介軟體來產生 antiforgery token，並 cookie 使用本主題稍後所述的預設角度命名慣例，以 (的形式將它傳送至回應中) ：</span><span class="sxs-lookup"><span data-stu-id="e38bd-255">The following example uses middleware from the app's home page to generate an antiforgery token and send it in the response as a cookie (using the default Angular naming convention described later in this topic):</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}
```

### <a name="require-antiforgery-validation"></a><span data-ttu-id="e38bd-256">需要 antiforgery 驗證</span><span class="sxs-lookup"><span data-stu-id="e38bd-256">Require antiforgery validation</span></span>

<span data-ttu-id="e38bd-257">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute)是可套用至個別動作、控制器或全域的動作篩選準則。</span><span class="sxs-lookup"><span data-stu-id="e38bd-257">[ValidateAntiForgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.validateantiforgerytokenattribute) is an action filter that can be applied to an individual action, a controller, or globally.</span></span> <span data-ttu-id="e38bd-258">除非要求包含有效的 antiforgery token，否則會封鎖已套用此篩選之動作的要求。</span><span class="sxs-lookup"><span data-stu-id="e38bd-258">Requests made to actions that have this filter applied are blocked unless the request includes a valid antiforgery token.</span></span>

```csharp
[HttpPost]
[ValidateAntiForgeryToken]
public async Task<IActionResult> RemoveLogin(RemoveLoginViewModel account)
{
    ManageMessageId? message = ManageMessageId.Error;
    var user = await GetCurrentUserAsync();

    if (user != null)
    {
        var result = 
            await _userManager.RemoveLoginAsync(
                user, account.LoginProvider, account.ProviderKey);

        if (result.Succeeded)
        {
            await _signInManager.SignInAsync(user, isPersistent: false);
            message = ManageMessageId.RemoveLoginSuccess;
        }
    }

    return RedirectToAction(nameof(ManageLogins), new { Message = message });
}
```

<span data-ttu-id="e38bd-259">`ValidateAntiForgeryToken`屬性會要求對其所標記之動作方法的要求使用權杖，包括 HTTP GET 要求。</span><span class="sxs-lookup"><span data-stu-id="e38bd-259">The `ValidateAntiForgeryToken` attribute requires a token for requests to the action methods it marks, including HTTP GET requests.</span></span> <span data-ttu-id="e38bd-260">如果在 `ValidateAntiForgeryToken` 應用程式的控制器上套用屬性，則可以使用屬性加以覆寫 `IgnoreAntiforgeryToken` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-260">If the `ValidateAntiForgeryToken` attribute is applied across the app's controllers, it can be overridden with the `IgnoreAntiforgeryToken` attribute.</span></span>

> [!NOTE]
> <span data-ttu-id="e38bd-261">ASP.NET Core 不支援自動新增 antiforgery token 來取得要求。</span><span class="sxs-lookup"><span data-stu-id="e38bd-261">ASP.NET Core doesn't support adding antiforgery tokens to GET requests automatically.</span></span>

### <a name="automatically-validate-antiforgery-tokens-for-unsafe-http-methods-only"></a><span data-ttu-id="e38bd-262">僅針對 unsafe HTTP 方法自動驗證 antiforgery 權杖</span><span class="sxs-lookup"><span data-stu-id="e38bd-262">Automatically validate antiforgery tokens for unsafe HTTP methods only</span></span>

<span data-ttu-id="e38bd-263">ASP.NET Core 應用程式不會 (GET、HEAD、OPTIONS 和 TRACE) 產生安全 HTTP 方法的 antiforgery token。</span><span class="sxs-lookup"><span data-stu-id="e38bd-263">ASP.NET Core apps don't generate antiforgery tokens for safe HTTP methods (GET, HEAD, OPTIONS, and TRACE).</span></span> <span data-ttu-id="e38bd-264">您 `ValidateAntiForgeryToken` `IgnoreAntiforgeryToken` 可以使用[AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute)屬性，而不是廣泛套用屬性，然後使用屬性來覆寫它。</span><span class="sxs-lookup"><span data-stu-id="e38bd-264">Instead of broadly applying the `ValidateAntiForgeryToken` attribute and then overriding it with `IgnoreAntiforgeryToken` attributes, the [AutoValidateAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.autovalidateantiforgerytokenattribute) attribute can be used.</span></span> <span data-ttu-id="e38bd-265">這個屬性與屬性的運作方式完全相同 `ValidateAntiForgeryToken` ，不同之處在于它不需要使用下列 HTTP 方法所提出之要求的權杖：</span><span class="sxs-lookup"><span data-stu-id="e38bd-265">This attribute works identically to the `ValidateAntiForgeryToken` attribute, except that it doesn't require tokens for requests made using the following HTTP methods:</span></span>

* <span data-ttu-id="e38bd-266">GET</span><span class="sxs-lookup"><span data-stu-id="e38bd-266">GET</span></span>
* <span data-ttu-id="e38bd-267">HEAD</span><span class="sxs-lookup"><span data-stu-id="e38bd-267">HEAD</span></span>
* <span data-ttu-id="e38bd-268">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="e38bd-268">OPTIONS</span></span>
* <span data-ttu-id="e38bd-269">TRACE</span><span class="sxs-lookup"><span data-stu-id="e38bd-269">TRACE</span></span>

<span data-ttu-id="e38bd-270">我們建議您在 `AutoValidateAntiforgeryToken` 非 API 案例中廣泛使用。</span><span class="sxs-lookup"><span data-stu-id="e38bd-270">We recommend use of `AutoValidateAntiforgeryToken` broadly for non-API scenarios.</span></span> <span data-ttu-id="e38bd-271">這可確保預設會保護 POST 動作。</span><span class="sxs-lookup"><span data-stu-id="e38bd-271">This ensures POST actions are protected by default.</span></span> <span data-ttu-id="e38bd-272">替代方式是預設忽略 antiforgery token，除非套用 `ValidateAntiForgeryToken` 至個別動作方法。</span><span class="sxs-lookup"><span data-stu-id="e38bd-272">The alternative is to ignore antiforgery tokens by default, unless `ValidateAntiForgeryToken` is applied to individual action methods.</span></span> <span data-ttu-id="e38bd-273">在此案例中，較有可能不小心將 POST 動作方法保留為未受保護，讓應用程式容易遭受 CSRF 攻擊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-273">It's more likely in this scenario for a POST action method to be left unprotected by mistake, leaving the app vulnerable to CSRF attacks.</span></span> <span data-ttu-id="e38bd-274">所有貼文都應該傳送 antiforgery token。</span><span class="sxs-lookup"><span data-stu-id="e38bd-274">All POSTs should send the antiforgery token.</span></span>

<span data-ttu-id="e38bd-275">Api 沒有自動機制來傳送權杖的非 cookie 部分。</span><span class="sxs-lookup"><span data-stu-id="e38bd-275">APIs don't have an automatic mechanism for sending the non-cookie part of the token.</span></span> <span data-ttu-id="e38bd-276">執行可能取決於用戶端程式代碼的執行。</span><span class="sxs-lookup"><span data-stu-id="e38bd-276">The implementation probably depends on the client code implementation.</span></span> <span data-ttu-id="e38bd-277">以下顯示一些範例：</span><span class="sxs-lookup"><span data-stu-id="e38bd-277">Some examples are shown below:</span></span>

<span data-ttu-id="e38bd-278">類別層級範例：</span><span class="sxs-lookup"><span data-stu-id="e38bd-278">Class-level example:</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
```

<span data-ttu-id="e38bd-279">全域範例：</span><span class="sxs-lookup"><span data-stu-id="e38bd-279">Global example:</span></span>

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e38bd-280">伺服器.AddMvc (選項 => 選項。篩選。新增 (新的 AutoValidateAntiforgeryTokenAttribute ( # A3 # A4 # A5;</span><span class="sxs-lookup"><span data-stu-id="e38bd-280">services.AddMvc(options =>     options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

```csharp
services.AddControllersWithViews(options =>
    options.Filters.Add(new AutoValidateAntiforgeryTokenAttribute()));
```

::: moniker-end

### <a name="override-global-or-controller-antiforgery-attributes"></a><span data-ttu-id="e38bd-281">覆寫全域或控制器 antiforgery 屬性</span><span class="sxs-lookup"><span data-stu-id="e38bd-281">Override global or controller antiforgery attributes</span></span>

<span data-ttu-id="e38bd-282">[IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute)篩選器是用來消除給定動作 (或控制器) 的 antiforgery token 需求。</span><span class="sxs-lookup"><span data-stu-id="e38bd-282">The [IgnoreAntiforgeryToken](/dotnet/api/microsoft.aspnetcore.mvc.ignoreantiforgerytokenattribute) filter is used to eliminate the need for an antiforgery token for a given action (or controller).</span></span> <span data-ttu-id="e38bd-283">套用時，此篩選會 `ValidateAntiForgeryToken` `AutoValidateAntiforgeryToken` 在全域或控制器) 上，覆寫在較高層級 (指定的和篩選。</span><span class="sxs-lookup"><span data-stu-id="e38bd-283">When applied, this filter overrides `ValidateAntiForgeryToken` and `AutoValidateAntiforgeryToken` filters specified at a higher level (globally or on a controller).</span></span>

```csharp
[Authorize]
[AutoValidateAntiforgeryToken]
public class ManageController : Controller
{
    [HttpPost]
    [IgnoreAntiforgeryToken]
    public async Task<IActionResult> DoSomethingSafe(SomeViewModel model)
    {
        // no antiforgery token required
    }
}
```

## <a name="refresh-tokens-after-authentication"></a><span data-ttu-id="e38bd-284">驗證後重新整理權杖</span><span class="sxs-lookup"><span data-stu-id="e38bd-284">Refresh tokens after authentication</span></span>

<span data-ttu-id="e38bd-285">使用者通過驗證之後，應該重新整理權杖，方法是將使用者重新導向至 view 或 Razor Pages 頁面。</span><span class="sxs-lookup"><span data-stu-id="e38bd-285">Tokens should be refreshed after the user is authenticated by redirecting the user to a view or Razor Pages page.</span></span>

## <a name="javascript-ajax-and-spas"></a><span data-ttu-id="e38bd-286">JavaScript、AJAX 和 Spa</span><span class="sxs-lookup"><span data-stu-id="e38bd-286">JavaScript, AJAX, and SPAs</span></span>

<span data-ttu-id="e38bd-287">在傳統的 HTML 架構應用程式中，antiforgery 權杖會使用隱藏的表單欄位來傳遞至伺服器。</span><span class="sxs-lookup"><span data-stu-id="e38bd-287">In traditional HTML-based apps, antiforgery tokens are passed to the server using hidden form fields.</span></span> <span data-ttu-id="e38bd-288">在以 JavaScript 為基礎的新式應用程式和 Spa 中，許多要求都是以程式設計方式進行。</span><span class="sxs-lookup"><span data-stu-id="e38bd-288">In modern JavaScript-based apps and SPAs, many requests are made programmatically.</span></span> <span data-ttu-id="e38bd-289">這些 AJAX 要求可能會使用其他技術 (例如要求標頭或 cookie s) 來傳送權杖。</span><span class="sxs-lookup"><span data-stu-id="e38bd-289">These AJAX requests may use other techniques (such as request headers or cookies) to send the token.</span></span>

<span data-ttu-id="e38bd-290">如果 cookie 使用來儲存驗證權杖，並在伺服器上驗證 API 要求，則 CSRF 可能會發生問題。</span><span class="sxs-lookup"><span data-stu-id="e38bd-290">If cookies are used to store authentication tokens and to authenticate API requests on the server, CSRF is a potential problem.</span></span> <span data-ttu-id="e38bd-291">如果使用本機儲存體來儲存權杖，可能會降低 CSRF 弱點，因為本機儲存體中的值不會隨每個要求自動傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="e38bd-291">If local storage is used to store the token, CSRF vulnerability might be mitigated because values from local storage aren't sent automatically to the server with every request.</span></span> <span data-ttu-id="e38bd-292">因此，使用本機儲存體將 antiforgery token 儲存在用戶端上，並以要求標頭的形式傳送權杖是建議的方法。</span><span class="sxs-lookup"><span data-stu-id="e38bd-292">Thus, using local storage to store the antiforgery token on the client and sending the token as a request header is a recommended approach.</span></span>

### <a name="javascript"></a><span data-ttu-id="e38bd-293">JavaScript</span><span class="sxs-lookup"><span data-stu-id="e38bd-293">JavaScript</span></span>

<span data-ttu-id="e38bd-294">使用 JavaScript 搭配 views，可以從視圖內使用服務來建立權杖。</span><span class="sxs-lookup"><span data-stu-id="e38bd-294">Using JavaScript with views, the token can be created using a service from within the view.</span></span> <span data-ttu-id="e38bd-295">將[AspNetCore Antiforgery. IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery)服務插入至視圖，並呼叫[GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens)：</span><span class="sxs-lookup"><span data-stu-id="e38bd-295">Inject the [Microsoft.AspNetCore.Antiforgery.IAntiforgery](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery) service into the view and call [GetAndStoreTokens](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgery.getandstoretokens):</span></span>

[!code-cshtml[](anti-request-forgery/sample/MvcSample/Views/Home/Ajax.cshtml?highlight=4-10,12-13,35-36)]

<span data-ttu-id="e38bd-296">這種方法不需要直接處理 cookie 伺服器的設定，或從用戶端讀取。</span><span class="sxs-lookup"><span data-stu-id="e38bd-296">This approach eliminates the need to deal directly with setting cookies from the server or reading them from the client.</span></span>

<span data-ttu-id="e38bd-297">上述範例會使用 JavaScript 來讀取 AJAX POST 標頭的隱藏欄位值。</span><span class="sxs-lookup"><span data-stu-id="e38bd-297">The preceding example uses JavaScript to read the hidden field value for the AJAX POST header.</span></span>

<span data-ttu-id="e38bd-298">JavaScript 也可以存取中的權杖 cookie ，並使用 cookie 的內容來建立具有權杖值的標頭。</span><span class="sxs-lookup"><span data-stu-id="e38bd-298">JavaScript can also access tokens in cookies and use the cookie's contents to create a header with the token's value.</span></span>

```csharp
context.Response.Cookies.Append("CSRF-TOKEN", tokens.RequestToken, 
    new Microsoft.AspNetCore.Http.CookieOptions { HttpOnly = false });
```

<span data-ttu-id="e38bd-299">假設腳本要求在名為的標頭中傳送權杖 `X-CSRF-TOKEN` ，請將 antiforgery 服務設定為尋找 `X-CSRF-TOKEN` 標頭：</span><span class="sxs-lookup"><span data-stu-id="e38bd-299">Assuming the script requests to send the token in a header called `X-CSRF-TOKEN`, configure the antiforgery service to look for the `X-CSRF-TOKEN` header:</span></span>

```csharp
services.AddAntiforgery(options => options.HeaderName = "X-CSRF-TOKEN");
```

<span data-ttu-id="e38bd-300">下列範例會使用 JavaScript，以適當的標頭建立 AJAX 要求：</span><span class="sxs-lookup"><span data-stu-id="e38bd-300">The following example uses JavaScript to make an AJAX request with the appropriate header:</span></span>

```javascript
function getCookie(cname) {
    var name = cname + "=";
    var decodedCookie = decodeURIComponent(document.cookie);
    var ca = decodedCookie.split(';');
    for(var i = 0; i <ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') {
            c = c.substring(1);
        }
        if (c.indexOf(name) == 0) {
            return c.substring(name.length, c.length);
        }
    }
    return "";
}

var csrfToken = getCookie("CSRF-TOKEN");

var xhttp = new XMLHttpRequest();
xhttp.onreadystatechange = function() {
    if (xhttp.readyState == XMLHttpRequest.DONE) {
        if (xhttp.status == 200) {
            alert(xhttp.responseText);
        } else {
            alert('There was an error processing the AJAX request.');
        }
    }
};
xhttp.open('POST', '/api/password/changepassword', true);
xhttp.setRequestHeader("Content-type", "application/json");
xhttp.setRequestHeader("X-CSRF-TOKEN", csrfToken);
xhttp.send(JSON.stringify({ "newPassword": "ReallySecurePassword999$$$" }));
```

### <a name="angularjs"></a><span data-ttu-id="e38bd-301">AngularJS</span><span class="sxs-lookup"><span data-stu-id="e38bd-301">AngularJS</span></span>

<span data-ttu-id="e38bd-302">AngularJS 會使用慣例來定址 CSRF。</span><span class="sxs-lookup"><span data-stu-id="e38bd-302">AngularJS uses a convention to address CSRF.</span></span> <span data-ttu-id="e38bd-303">如果伺服器 cookie 使用名稱傳送 `XSRF-TOKEN` ，AngularJS `$http` 服務 cookie 會在將要求傳送至伺服器時，將值新增至標頭。</span><span class="sxs-lookup"><span data-stu-id="e38bd-303">If the server sends a cookie with the name `XSRF-TOKEN`, the AngularJS `$http` service adds the cookie value to a header when it sends a request to the server.</span></span> <span data-ttu-id="e38bd-304">此程式是自動的。</span><span class="sxs-lookup"><span data-stu-id="e38bd-304">This process is automatic.</span></span> <span data-ttu-id="e38bd-305">不需要明確地在用戶端中設定標頭。</span><span class="sxs-lookup"><span data-stu-id="e38bd-305">The header doesn't need to be set in the client explicitly.</span></span> <span data-ttu-id="e38bd-306">標頭名稱為 `X-XSRF-TOKEN` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-306">The header name is `X-XSRF-TOKEN`.</span></span> <span data-ttu-id="e38bd-307">伺服器應該會偵測到此標頭並驗證其內容。</span><span class="sxs-lookup"><span data-stu-id="e38bd-307">The server should detect this header and validate its contents.</span></span>

<span data-ttu-id="e38bd-308">若要讓 ASP.NET Core API 在應用程式啟動時使用此慣例：</span><span class="sxs-lookup"><span data-stu-id="e38bd-308">For ASP.NET Core API to work with this convention in your application startup:</span></span>

* <span data-ttu-id="e38bd-309">設定您的應用程式，以在呼叫的中提供權杖 cookie `XSRF-TOKEN` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-309">Configure your app to provide a token in a cookie called `XSRF-TOKEN`.</span></span>
* <span data-ttu-id="e38bd-310">設定 antiforgery 服務以尋找名為的標頭 `X-XSRF-TOKEN` 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-310">Configure the antiforgery service to look for a header named `X-XSRF-TOKEN`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IAntiforgery antiforgery)
{
    app.Use(next => context =>
    {
        string path = context.Request.Path.Value;

        if (
            string.Equals(path, "/", StringComparison.OrdinalIgnoreCase) ||
            string.Equals(path, "/index.html", StringComparison.OrdinalIgnoreCase))
        {
            // The request token can be sent as a JavaScript-readable cookie, 
            // and Angular uses it by default.
            var tokens = antiforgery.GetAndStoreTokens(context);
            context.Response.Cookies.Append("XSRF-TOKEN", tokens.RequestToken, 
                new CookieOptions() { HttpOnly = false });
        }

        return next(context);
    });
}

public void ConfigureServices(IServiceCollection services)
{
    // Angular's default header name for sending the XSRF token.
    services.AddAntiforgery(options => options.HeaderName = "X-XSRF-TOKEN");
}
```

<span data-ttu-id="e38bd-311">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="e38bd-311">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/anti-request-forgery/sample/AngularSample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="extend-antiforgery"></a><span data-ttu-id="e38bd-312">擴充 antiforgery</span><span class="sxs-lookup"><span data-stu-id="e38bd-312">Extend antiforgery</span></span>

<span data-ttu-id="e38bd-313">[IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider)類型可讓開發人員在每個權杖中來回往返額外的資料，以擴充反 CSRF 系統的行為。</span><span class="sxs-lookup"><span data-stu-id="e38bd-313">The [IAntiForgeryAdditionalDataProvider](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider) type allows developers to extend the behavior of the anti-CSRF system by round-tripping additional data in each token.</span></span> <span data-ttu-id="e38bd-314">每次產生欄位標記時，會呼叫[GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata)方法，而傳回值會內嵌在產生的權杖中。</span><span class="sxs-lookup"><span data-stu-id="e38bd-314">The [GetAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.getadditionaldata) method is called each time a field token is generated, and the return value is embedded within the generated token.</span></span> <span data-ttu-id="e38bd-315">實施者可以傳回時間戳記、nonce 或任何其他值，然後在驗證權杖時呼叫[ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata)來驗證此資料。</span><span class="sxs-lookup"><span data-stu-id="e38bd-315">An implementer could return a timestamp, a nonce, or any other value and then call [ValidateAdditionalData](/dotnet/api/microsoft.aspnetcore.antiforgery.iantiforgeryadditionaldataprovider.validateadditionaldata) to validate this data when the token is validated.</span></span> <span data-ttu-id="e38bd-316">用戶端的使用者名稱已內嵌在產生的權杖中，因此不需要包含這項資訊。</span><span class="sxs-lookup"><span data-stu-id="e38bd-316">The client's username is already embedded in the generated tokens, so there's no need to include this information.</span></span> <span data-ttu-id="e38bd-317">如果權杖包含補充資料，但未 `IAntiForgeryAdditionalDataProvider` 設定，則不會驗證補充資料。</span><span class="sxs-lookup"><span data-stu-id="e38bd-317">If a token includes supplemental data but no `IAntiForgeryAdditionalDataProvider` is configured, the supplemental data isn't validated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e38bd-318">其他資源</span><span class="sxs-lookup"><span data-stu-id="e38bd-318">Additional resources</span></span>

* <span data-ttu-id="e38bd-319">[開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)的[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) (OWASP) 。</span><span class="sxs-lookup"><span data-stu-id="e38bd-319">[CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)) on [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page) (OWASP).</span></span>
* <xref:host-and-deploy/web-farm>
