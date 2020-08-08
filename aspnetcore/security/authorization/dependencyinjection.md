---
title: ASP.NET Core 中的需求處理常式中的相依性插入
author: rick-anderson
description: 瞭解如何使用相依性插入將授權需求處理常式插入 ASP.NET Core 應用程式中。
ms.author: riande
ms.date: 10/14/2016
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
uid: security/authorization/dependencyinjection
ms.openlocfilehash: e58498cc7a28b598b919c5cab128249448bde32a
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88020999"
---
# <a name="dependency-injection-in-requirement-handlers-in-aspnet-core"></a><span data-ttu-id="ba142-103">ASP.NET Core 中的需求處理常式中的相依性插入</span><span class="sxs-lookup"><span data-stu-id="ba142-103">Dependency injection in requirement handlers in ASP.NET Core</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="ba142-104">在使用相依性[插入](xref:fundamentals/dependency-injection)) 的設定 (期間，必須在服務集合中[註冊授權處理常式](xref:security/authorization/policies#handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="ba142-104">[Authorization handlers must be registered](xref:security/authorization/policies#handler-registration) in the service collection during configuration (using [dependency injection](xref:fundamentals/dependency-injection)).</span></span>

<span data-ttu-id="ba142-105">假設您有想要在授權處理常式中評估的規則存放庫，而且該儲存機制已在服務集合中註冊。</span><span class="sxs-lookup"><span data-stu-id="ba142-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="ba142-106">授權將會解析，並將其插入您的函式。</span><span class="sxs-lookup"><span data-stu-id="ba142-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="ba142-107">例如，如果您想要使用 ASP。您想要插入處理常式中的網路記錄基礎結構 `ILoggerFactory` 。</span><span class="sxs-lookup"><span data-stu-id="ba142-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="ba142-108">這類處理常式看起來可能像這樣：</span><span class="sxs-lookup"><span data-stu-id="ba142-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="ba142-109">您會向註冊處理常式 `services.AddSingleton()` ：</span><span class="sxs-lookup"><span data-stu-id="ba142-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="ba142-110">當您的應用程式啟動時，將會建立處理常式的實例，而 DI 會將註冊的插入您的函式 `ILoggerFactory` 中。</span><span class="sxs-lookup"><span data-stu-id="ba142-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="ba142-111">使用 Entity Framework 的處理常式不應該註冊為單次個體。</span><span class="sxs-lookup"><span data-stu-id="ba142-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>
