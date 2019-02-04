---
title: 存取 ASP.NET Core 中的 HttpContext
author: coderandhiker
description: 了解如何存取 ASP.NET Core 中的 HttpContext。
ms.author: riande
ms.custom: mvc
ms.date: 07/27/2018
uid: fundamentals/httpcontext
ms.openlocfilehash: babc637cdec8590ac14f7924c17e862e5b2f6a81
ms.sourcegitcommit: d22b3c23c45a076c4f394a70b1c8df2fbcdf656d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/31/2019
ms.locfileid: "55428482"
---
# <a name="access-httpcontext-in-aspnet-core"></a>存取 ASP.NET Core 中的 HttpContext

ASP.NET Core 應用程式透過 [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor) 介面與其預設實作 [HttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.httpcontextaccessor) 來存取 `HttpContext`。 只有當您需要存取服務內的 `HttpContext` 時，才需要使用 `IHttpContextAccessor`。

::: moniker range=">= aspnetcore-2.0"

## <a name="use-httpcontext-from-razor-pages"></a>從 Razor 頁面使用 HttpContext

Razor 頁面 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 會公開 [HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.httpcontext) 屬性：

```csharp
public class AboutModel : PageModel
{
    public string Message { get; set; }

    public void OnGet()
    {
        Message = HttpContext.Request.PathBase;
    }
}
```

::: moniker-end

## <a name="use-httpcontext-from-a-razor-view"></a>從 Razor 檢視使用 HttpContext

Razor 檢視會透過檢視上的 [RazorPage.Context](/dotnet/api/microsoft.aspnetcore.mvc.razor.razorpage.context#Microsoft_AspNetCore_Mvc_Razor_RazorPage_Context) 屬性，直接公開 `HttpContext`。 以下範例會在內部網路應用程式中使用 Windows 驗證，擷取目前的使用者名稱：

```cshtml
@{
    var username = Context.User.Identity.Name;
}
```

## <a name="use-httpcontext-from-a-controller"></a>從控制器使用 HttpContext

控制器會公開 [ControllerBase.HttpContext](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.httpcontext) 屬性：

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;
        // Do something with the PathBase.

        return View();
    }
}
```

## <a name="use-httpcontext-from-middleware"></a>從中介軟體使用 HttpContext

使用自訂中介軟體元件時，`HttpContext` 會傳入 `Invoke` 或 `InvokeAsync` 方法，並且可以在設定中介軟體時存取：

```csharp
public class MyCustomMiddleware
{
    public Task InvokeAsync(HttpContext context)
    {
        // Middleware initialization optionally using HttpContext
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>從自訂元件中使用 HttpContext

對於需要存取 `HttpContext` 的其他架構和自訂元件，建議的方法是使用內建的[相依性插入容器](xref:fundamentals/dependency-injection)來註冊相依性。 相依性插入容器會將 `IHttpContextAccessor` 提供給在其建構函式中將其聲明為相依性的任何類別。

::: moniker range=">= aspnetcore-2.1"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc();
     services.AddSingleton<IHttpContextAccessor, HttpContextAccessor>();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

在以下範例中：

* `UserRepository` 宣告其對 `IHttpContextAccessor` 的相依性。
* 當相依性插入解析相依性鏈結並建立 `UserRepository` 的實例時，將提供相依性。

```csharp
public class UserRepository : IUserRepository
{
    private readonly IHttpContextAccessor _httpContextAccessor;

    public UserRepository(IHttpContextAccessor httpContextAccessor)
    {
        _httpContextAccessor = httpContextAccessor;
    }

    public void LogCurrentUser()
    {
        var username = _httpContextAccessor.HttpContext.User.Identity.Name;
        service.LogAccessRequest(username);
    }
}
```
