---
title: 遷移驗證和 Identity ASP.NET Core
author: ardalis
description: 瞭解如何將 ASP.NET MVC 專案中的驗證和身分識別遷移至 ASP.NET Core MVC 專案。
ms.author: riande
ms.date: 3/22/2020
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
uid: migration/identity
ms.openlocfilehash: c8e6a1a8bf9ef06d98db0e7e0a6a0e5ff393e322
ms.sourcegitcommit: f09407d128634d200c893bfb1c163e87fa47a161
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/26/2020
ms.locfileid: "88865529"
---
# <a name="migrate-authentication-and-no-locidentity-to-aspnet-core"></a><span data-ttu-id="cc83c-103">遷移驗證和 Identity ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc83c-103">Migrate Authentication and Identity to ASP.NET Core</span></span>

<span data-ttu-id="cc83c-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="cc83c-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="cc83c-105">在前一篇文章中，我們 [已將設定從 ASP.NET mvc 專案遷移至 ASP.NET CORE mvc](xref:migration/configuration)。</span><span class="sxs-lookup"><span data-stu-id="cc83c-105">In the previous article, we [migrated configuration from an ASP.NET MVC project to ASP.NET Core MVC](xref:migration/configuration).</span></span> <span data-ttu-id="cc83c-106">在本文中，我們會遷移註冊、登入和使用者管理功能。</span><span class="sxs-lookup"><span data-stu-id="cc83c-106">In this article, we migrate the registration, login, and user management features.</span></span>

## <a name="configure-no-locidentity-and-membership"></a><span data-ttu-id="cc83c-107">設定 Identity 和成員資格</span><span class="sxs-lookup"><span data-stu-id="cc83c-107">Configure Identity and Membership</span></span>

<span data-ttu-id="cc83c-108">在 ASP.NET MVC 中，會使用 Identity *Startup.Auth.cs*和\* Identity Config.cs*中的 ASP.NET （位於*App_Start\*資料夾）來設定驗證和身分識別功能。</span><span class="sxs-lookup"><span data-stu-id="cc83c-108">In ASP.NET MVC, authentication and identity features are configured using ASP.NET Identity in *Startup.Auth.cs* and *IdentityConfig.cs*, located in the *App_Start* folder.</span></span> <span data-ttu-id="cc83c-109">在 ASP.NET Core MVC 中，這些功能是在 *Startup.cs*中設定。</span><span class="sxs-lookup"><span data-stu-id="cc83c-109">In ASP.NET Core MVC, these features are configured in *Startup.cs*.</span></span>

<span data-ttu-id="cc83c-110">安裝下列 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="cc83c-110">Install the following NuGet packages:</span></span>

* `Microsoft.AspNetCore.Identity.EntityFrameworkCore`
* `Microsoft.AspNetCore.Authentication.Cookies`
* `Microsoft.EntityFrameworkCore.SqlServer`

<span data-ttu-id="cc83c-111">在 *Startup.cs*中，更新 `Startup.ConfigureServices` 方法以使用 Entity Framework 和 Identity 服務：</span><span class="sxs-lookup"><span data-stu-id="cc83c-111">In *Startup.cs*, update the `Startup.ConfigureServices` method to use Entity Framework and Identity services:</span></span>

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

<span data-ttu-id="cc83c-112">此時，上述程式碼中所參考的兩種類型尚未從 ASP.NET MVC 專案中遷移： `ApplicationDbContext` 和 `ApplicationUser` 。</span><span class="sxs-lookup"><span data-stu-id="cc83c-112">At this point, there are two types referenced in the above code that we haven't yet migrated from the ASP.NET MVC project: `ApplicationDbContext` and `ApplicationUser`.</span></span> <span data-ttu-id="cc83c-113">在 ASP.NET Core 專案中建立新的 [ *模型* ] 資料夾，並將兩個類別新增至對應至這些類型的類別。</span><span class="sxs-lookup"><span data-stu-id="cc83c-113">Create a new *Models* folder in the ASP.NET Core project, and add two classes to it corresponding to these types.</span></span> <span data-ttu-id="cc83c-114">您會在 */Models/ Identity Models.cs*中找到這些類別的 ASP.NET MVC 版本，但我們會在已遷移的專案中針對每個類別使用一個檔案，因為這比較清楚。</span><span class="sxs-lookup"><span data-stu-id="cc83c-114">You will find the ASP.NET MVC versions of these classes in */Models/IdentityModels.cs*, but we will use one file per class in the migrated project since that's more clear.</span></span>

<span data-ttu-id="cc83c-115">*ApplicationUser.cs*：</span><span class="sxs-lookup"><span data-stu-id="cc83c-115">*ApplicationUser.cs*:</span></span>

```csharp
using Microsoft.AspNetCore.Identity.EntityFrameworkCore;

namespace NewMvcProject.Models
{
  public class ApplicationUser : IdentityUser
  {
  }
}
```

<span data-ttu-id="cc83c-116">*ApplicationDbCoNtext.cs*：</span><span class="sxs-lookup"><span data-stu-id="cc83c-116">*ApplicationDbContext.cs*:</span></span>

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

<span data-ttu-id="cc83c-117">ASP.NET Core MVC 入門 Web 專案不包含許多使用者自訂或 `ApplicationDbContext` 。</span><span class="sxs-lookup"><span data-stu-id="cc83c-117">The ASP.NET Core MVC Starter Web project doesn't include much customization of users, or the `ApplicationDbContext`.</span></span> <span data-ttu-id="cc83c-118">在遷移實際的應用程式時，您也需要遷移應用程式使用者和類別的所有自訂屬性和方法 `DbContext` ，以及應用程式所使用的任何其他模型類別。</span><span class="sxs-lookup"><span data-stu-id="cc83c-118">When migrating a real app, you also need to migrate all of the custom properties and methods of your app's user and `DbContext` classes, as well as any other Model classes your app utilizes.</span></span> <span data-ttu-id="cc83c-119">例如，如果您 `DbContext` 有 `DbSet<Album>` ，則需要遷移 `Album` 類別。</span><span class="sxs-lookup"><span data-stu-id="cc83c-119">For example, if your `DbContext` has a `DbSet<Album>`, you need to migrate the `Album` class.</span></span>

<span data-ttu-id="cc83c-120">使用這些檔案時，您可以藉由更新其語句來進行編譯 *Startup.cs* 檔案 `using` ：</span><span class="sxs-lookup"><span data-stu-id="cc83c-120">With these files in place, the *Startup.cs* file can be made to compile by updating its `using` statements:</span></span>

```csharp
using Microsoft.AspNetCore.Builder;
using Microsoft.AspNetCore.Identity;
using Microsoft.AspNetCore.Hosting;
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.DependencyInjection;
```

<span data-ttu-id="cc83c-121">我們的應用程式現在已準備好支援驗證和 Identity 服務。</span><span class="sxs-lookup"><span data-stu-id="cc83c-121">Our app is now ready to support authentication and Identity services.</span></span> <span data-ttu-id="cc83c-122">只需要將這些功能公開給使用者即可。</span><span class="sxs-lookup"><span data-stu-id="cc83c-122">It just needs to have these features exposed to users.</span></span>

## <a name="migrate-registration-and-login-logic"></a><span data-ttu-id="cc83c-123">遷移註冊和登入邏輯</span><span class="sxs-lookup"><span data-stu-id="cc83c-123">Migrate registration and login logic</span></span>

<span data-ttu-id="cc83c-124">透過 Identity 針對使用 Entity Framework 和 SQL Server 所設定的應用程式和資料存取設定的服務，我們已準備好將註冊和登入的支援新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="cc83c-124">With Identity services configured for the app and data access configured using Entity Framework and SQL Server, we're ready to add support for registration and login to the app.</span></span> <span data-ttu-id="cc83c-125">回想一下，稍[早在遷移過程中](xref:migration/mvc#migrate-the-layout-file)，我們會將 *_Layout*的 *_LoginPartial*參考批註化。</span><span class="sxs-lookup"><span data-stu-id="cc83c-125">Recall that [earlier in the migration process](xref:migration/mvc#migrate-the-layout-file) we commented out a reference to *_LoginPartial* in *_Layout.cshtml*.</span></span> <span data-ttu-id="cc83c-126">現在可以回到該程式碼、將它取消批註，然後加入必要的控制器和 views 來支援登入功能。</span><span class="sxs-lookup"><span data-stu-id="cc83c-126">Now it's time to return to that code, uncomment it, and add in the necessary controllers and views to support login functionality.</span></span>

<span data-ttu-id="cc83c-127">將 `@Html.Partial` *_Layout*中的行取消批註：</span><span class="sxs-lookup"><span data-stu-id="cc83c-127">Uncomment the `@Html.Partial` line in *_Layout.cshtml*:</span></span>

```cshtml
      <li>@Html.ActionLink("Contact", "Contact", "Home")</li>
    </ul>
    @*@Html.Partial("_LoginPartial")*@
  </div>
</div>
```

<span data-ttu-id="cc83c-128">現在，將 Razor 名為 *_LoginPartial* 的新視圖新增至 *Views/Shared* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="cc83c-128">Now, add a new Razor view called *_LoginPartial* to the *Views/Shared* folder:</span></span>

<span data-ttu-id="cc83c-129">以下列程式碼更新 *_LoginPartial，* (取代) 的所有內容：</span><span class="sxs-lookup"><span data-stu-id="cc83c-129">Update *_LoginPartial.cshtml* with the following code (replace all of its contents):</span></span>

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

<span data-ttu-id="cc83c-130">此時，您應該能夠在瀏覽器中重新整理網站。</span><span class="sxs-lookup"><span data-stu-id="cc83c-130">At this point, you should be able to refresh the site in your browser.</span></span>

## <a name="summary"></a><span data-ttu-id="cc83c-131">摘要</span><span class="sxs-lookup"><span data-stu-id="cc83c-131">Summary</span></span>

<span data-ttu-id="cc83c-132">ASP.NET Core 引進 ASP.NET 功能的變更 Identity 。</span><span class="sxs-lookup"><span data-stu-id="cc83c-132">ASP.NET Core introduces changes to the ASP.NET Identity features.</span></span> <span data-ttu-id="cc83c-133">在本文中，您已瞭解如何將 ASP.NET 的驗證和使用者管理功能遷移 Identity 至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="cc83c-133">In this article, you have seen how to migrate the authentication and user management features of ASP.NET Identity to ASP.NET Core.</span></span>
