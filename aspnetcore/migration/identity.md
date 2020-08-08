---
title: 遷移驗證和 Identity ASP.NET Core
author: ardalis
description: 瞭解如何將驗證和身分識別從 ASP.NET MVC 專案遷移至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 3/22/2020
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
uid: migration/identity
ms.openlocfilehash: 8a6cc1e612dbf59dc2db2eca165dd1a03ab92f81
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88014928"
---
# <a name="migrate-authentication-and-no-locidentity-to-aspnet-core"></a>遷移驗證和 Identity ASP.NET Core

作者：[Steve Smith](https://ardalis.com/)

在前一篇文章中，我們將設定[從 ASP.NET mvc 專案遷移至 ASP.NET CORE mvc](xref:migration/configuration)。 在本文中，我們會遷移註冊、登入和使用者管理功能。

## <a name="configure-no-locidentity-and-membership"></a>設定 Identity 和成員資格

在 ASP.NET MVC 中，驗證和身分識別功能是使用 Identity *Startup.Auth.cs*和* Identity Config.cs*中的 ASP.NET （位於*App_Start*資料夾）來設定。 在 ASP.NET Core MVC 中，這些功能會在*Startup.cs*中設定。

安裝下列 NuGet 套件：

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`
* `Microsoft.AspNetCore.Authentication.Cookies`
* `Microsoft.EntityFrameworkCore.SqlServer`

在*Startup.cs*中，更新 `Startup.ConfigureServices` 方法以使用 Entity Framework 和 Identity 服務：

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add EF services to the services container.
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("DefaultConnection")));

    services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

     services.AddMvc();
}
```

此時，上述程式碼中已參考了兩種類型，我們尚未從 ASP.NET MVC 專案進行遷移： `ApplicationDbContext` 和 `ApplicationUser` 。 在 ASP.NET Core 專案中建立新的 [*模型*] 資料夾，並在其中加入對應至這些類型的兩個類別。 您會在 */Models/ Identity Models.cs*中找到這些類別的 ASP.NET MVC 版本，但在遷移的專案中，每個類別都會使用一個檔案，因為這更為清楚。

*ApplicationUser.cs*：

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

*ApplicationDbCoNtext.cs*：

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;
using Microsoft.Data.Entity;

namespace NewMvcProject.Models
{
    public class ApplicationDbContext : IdentityDbContext<ApplicationUser>
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }

        protected override void OnModelCreating(ModelBuilder builder)
        {
            base.OnModelCreating(builder);
            // Customize the ASP.NET Core Identity model and override the defaults if needed.
            // For example, you can rename the ASP.NET Core Identity table names and more.
            // Add your customizations after calling base.OnModelCreating(builder);
        }
    }
}
```

ASP.NET Core MVC Starter Web 專案不包含許多使用者自訂，或 `ApplicationDbContext` 。 在遷移實際的應用程式時，您也需要遷移應用程式使用者和類別的所有自訂屬性和方法 `DbContext` ，以及應用程式所使用的任何其他模型類別。 例如，如果您的 `DbContext` 具有 `DbSet<Album>` ，則需要遷移 `Album` 類別。

這些檔案都備妥之後，就可以更新*Startup.cs*檔案的語句來進行編譯 `using` ：

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

我們的應用程式現在已準備好支援驗證和 Identity 服務。 這只需要將這些功能公開給使用者。

## <a name="migrate-registration-and-login-logic"></a>遷移註冊和登入邏輯

Identity針對使用 Entity Framework 和 SQL Server 設定的應用程式和資料存取設定的服務，我們已準備好新增對應用程式註冊和登入的支援。 回想一下，稍[早在遷移程式中](xref:migration/mvc#migrate-the-layout-file)，我們已將 *_Layout*中 *_LoginPartial*的參考批註化。 現在可以回到該程式碼，將它取消批註，然後加入必要的控制器和視圖，以支援登入功能。

取消批註 `@Html.Partial` *_Layout*中的那一行：

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

現在，將名為 Razor *_LoginPartial*的新視圖加入*至 Views/Shared*資料夾：

使用下列程式碼更新 *_LoginPartial. cshtml* ， (取代其所有內容) ：

```cshtml
@inject SignInManager<ApplicationUser> SignInManager
@inject UserManager<ApplicationUser> UserManager

@if (SignInManager.IsSignedIn(User))
{
    <form asp-area="" asp-controller="Account" asp-action="Logout" method="post" id="logoutForm" class="navbar-right">
        <ul class="nav navbar-nav navbar-right">
            <li>
                <a asp-area="" asp-controller="Manage" asp-action="Index" title="Manage">Hello @UserManager.GetUserName(User)!</a>
            </li>
            <li>
                <button type="submit" class="btn btn-link navbar-btn navbar-link">Log out</button>
            </li>
        </ul>
    </form>
}
else
{
    <ul class="nav navbar-nav navbar-right">
        <li><a asp-area="" asp-controller="Account" asp-action="Register">Register</a></li>
        <li><a asp-area="" asp-controller="Account" asp-action="Login">Log in</a></li>
    </ul>
}
```

此時，您應該能夠在瀏覽器中重新整理網站。

## <a name="summary"></a>總結

ASP.NET Core 引進 ASP.NET 功能的變更 Identity 。 在本文中，您已瞭解如何將 ASP.NET 的驗證和使用者管理功能遷移 Identity 至 ASP.NET Core。
