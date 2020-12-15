---
title: 存取 ASP.NET Core 中的 HttpContext
author: coderandhiker
description: 了解如何存取 ASP.NET Core 中的 HttpContext。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/5/2020
no-loc:
- appsettings.json
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
uid: fundamentals/httpcontext
ms.openlocfilehash: d5ada72bb03cd74cf77c31450e5eafb9c2841049
ms.sourcegitcommit: 6b87f2e064cea02e65dacd206394b44f5c604282
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2020
ms.locfileid: "97506990"
---
# <a name="access-httpcontext-in-aspnet-core"></a>存取 ASP.NET Core 中的 HttpContext

ASP.NET Core apps 會 `HttpContext` 透過 <xref:Microsoft.AspNetCore.Http.IHttpContextAccessor> 介面和其預設執行來存取 <xref:Microsoft.AspNetCore.Http.HttpContextAccessor> 。 只有當您需要存取服務內的 `HttpContext` 時，才需要使用 `IHttpContextAccessor`。

## <a name="use-httpcontext-from-no-locrazor-pages"></a>從頁面使用 HttpCoNtext Razor

這些 Razor 頁面會 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 公開 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.HttpContext> 屬性：

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

## <a name="use-httpcontext-from-a-no-locrazor-view"></a>從 view 使用 HttpCoNtext Razor

Razorviews 會直接透過頁面公開。 `HttpContext` 此視圖的[ Razor 上下文](xref:Microsoft.AspNetCore.Mvc.Razor.RazorPage.Context)屬性。 下列範例會使用 Windows 驗證，在內部網路應用程式中抓取目前的使用者名稱：

```cshtml
@{
    var username = Context.User.Identity.Name;
    
    ...
}
```

## <a name="use-httpcontext-from-a-controller"></a>從控制器使用 HttpContext

控制器會公開 [ControllerBase.HttpContext](xref:Microsoft.AspNetCore.Mvc.ControllerBase.HttpContext) 屬性：

```csharp
public class HomeController : Controller
{
    public IActionResult About()
    {
        var pathBase = HttpContext.Request.PathBase;

        ...

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
        ...
    }
}
```

## <a name="use-httpcontext-from-custom-components"></a>從自訂元件中使用 HttpContext

對於需要存取 `HttpContext` 的其他架構和自訂元件，建議的方法是使用內建的[相依性插入容器](xref:fundamentals/dependency-injection)來註冊相依性。 相依性插入容器會將提供給任何在其函式中宣告為相依性的 `IHttpContextAccessor` 類別：

::: moniker range=">= aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddControllersWithViews();
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

```csharp
public void ConfigureServices(IServiceCollection services)
{
     services.AddMvc()
         .SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
     services.AddHttpContextAccessor();
     services.AddTransient<IUserRepository, UserRepository>();
}
```

::: moniker-end

在下例中︰

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

## <a name="httpcontext-access-from-a-background-thread"></a>從背景執行緒存取 HttpContext

`HttpContext` 不是安全線程。 在處理要求之外讀取或寫入 `HttpContext` 的屬性，可能會導致 <xref:System.NullReferenceException>。

> [!NOTE]
> 如果您的應用程式產生偶發 `NullReferenceException` 的錯誤，請檢查程式碼中啟動背景處理或在要求完成之後仍繼續處理的部分。 尋找錯誤，例如將控制器方法定義為 `async void` 。

使用 `HttpContext` 資料安全地執行背景工作：

* 在要求處理期間複製所需的資料。
* 將複製的資料傳遞至背景工作。

若要避免不安全的程式碼，絕對不要將傳遞給 `HttpContext` 執行背景工作的方法。 請改為傳遞所需的資料。 在下列範例中， `SendEmailCore` 會呼叫以開始傳送電子郵件。 `correlationId`會傳遞至 `SendEmailCore` ，而不是 `HttpContext` 。 程式碼執行不會等候 `SendEmailCore` 完成：

```csharp
public class EmailController : Controller
{
    public IActionResult SendEmail(string email)
    {
        var correlationId = HttpContext.Request.Headers["x-correlation-id"].ToString();

        _ = SendEmailCore(correlationId);

        return View();
    }

    private async Task SendEmailCore(string correlationId)
    {
        ...
    }
}
```

## <a name="no-locblazor-and-shared-state"></a>Blazor 和共用狀態

[!INCLUDE[](~/blazor/includes/security/blazor-shared-state.md)]
