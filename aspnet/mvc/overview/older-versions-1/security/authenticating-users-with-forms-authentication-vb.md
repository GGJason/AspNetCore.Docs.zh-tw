---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: 驗證使用者使用表單驗證 (VB) |Microsoft 文件
author: microsoft
description: 了解如何使用 [Authorize] 屬性以密碼保護的 MVC 應用程式中的特定頁面。 您了解如何使用網站管理太...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ff425a4c9728de2eec3d0c94e76cb51a15de487
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870995"
---
<a name="authenticating-users-with-forms-authentication-vb"></a><span data-ttu-id="9b1e9-104">驗證使用者使用表單驗證 (VB)</span><span class="sxs-lookup"><span data-stu-id="9b1e9-104">Authenticating Users with Forms Authentication (VB)</span></span>
====================
<span data-ttu-id="9b1e9-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="9b1e9-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="9b1e9-106">了解如何使用 [Authorize] 屬性以密碼保護的 MVC 應用程式中的特定頁面。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-106">Learn how to use the [Authorize] attribute to password protect particular pages in your MVC application.</span></span> <span data-ttu-id="9b1e9-107">您了解如何使用網站管理工具來建立及管理使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-107">You learn how to use the Web Site Administration Tool to create and manage users and roles.</span></span> <span data-ttu-id="9b1e9-108">您也會了解如何設定使用者帳戶和角色的資訊儲存在何處。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-108">You also learn how to configure where user account and role information is stored.</span></span>


<span data-ttu-id="9b1e9-109">本教學課程的目標是要說明如何使用表單驗證以密碼保護在 ASP.NET MVC 應用程式中的檢視。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-109">The goal of this tutorial is to explain how you can use Forms authentication to password protect the views in your ASP.NET MVC applications.</span></span> <span data-ttu-id="9b1e9-110">您了解如何使用網站管理工具來建立使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-110">You learn how to use the Web Site Administration Tool to create users and roles.</span></span> <span data-ttu-id="9b1e9-111">您也會學習如何叫用控制器的動作時，防止未經授權的使用者。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-111">You also learn how to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="9b1e9-112">最後，您會了解如何設定使用者名稱和密碼的儲存位置。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-112">Finally, you learn how to configure where user names and passwords are stored.</span></span>

#### <a name="using-the-web-site-administration-tool"></a><span data-ttu-id="9b1e9-113">使用網站管理工具</span><span class="sxs-lookup"><span data-stu-id="9b1e9-113">Using the Web Site Administration Tool</span></span>

<span data-ttu-id="9b1e9-114">我們執行任何其他動作之前，我們應該先建立某些使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-114">Before we do anything else, we should start by creating some users and roles.</span></span> <span data-ttu-id="9b1e9-115">若要建立新的使用者和角色最簡單的方式是利用 Visual Studio 2008 網站管理工具。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-115">The easiest way to create new users and roles is to take advantage of the Visual Studio 2008 Web Site Administration Tool.</span></span> <span data-ttu-id="9b1e9-116">您可以選取功能表選項來啟動這個工具**專案時，ASP.NET 組態**。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-116">You can launch this tool by selecting the menu option **Project, ASP.NET Configuration**.</span></span> <span data-ttu-id="9b1e9-117">或者，您可以叫用會出現在 [方案總管] 視窗頂端的世界 hammer （有些嚇人） 圖示，即可啟動網站管理工具 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-117">Alternatively, you can launch the Web Site Administration Tool by clicking the (somewhat scary) icon of the hammer hitting the world that appears at the top of the Solution Explorer window (see Figure 1).</span></span>

<span data-ttu-id="9b1e9-118">**圖 1 – 啟動網站管理工具**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-118">**Figure 1 – Launching the Web Site Administration Tool**</span></span>

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

<span data-ttu-id="9b1e9-120">在網站管理工具中，您可以選取 [安全性] 索引標籤建立新的使用者和角色。按一下**建立使用者**連結，以建立名為 Stephen 的新使用者 （請參閱圖 2）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-120">Within the Web Site Administration Tool, you create new users and roles by selecting the Security tab. Click the **Create user** link to create a new user named Stephen (see Figure 2).</span></span> <span data-ttu-id="9b1e9-121">作者： Stephen 使用者提供您想要的任何密碼 (例如，*密碼*)。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-121">Provide the Stephen user with any password that you want (for example, *secret*).</span></span>

<span data-ttu-id="9b1e9-122">**圖 2 – 建立新的使用者**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-122">**Figure 2 – Creating a new user**</span></span>

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

<span data-ttu-id="9b1e9-124">您可以建立新的角色啟用角色，定義一個或多個角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-124">You create new roles by first enabling roles and defining one or more roles.</span></span> <span data-ttu-id="9b1e9-125">按一下 啟用角色**啟用角色**連結。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-125">Enable roles by clicking the **Enable roles** link.</span></span> <span data-ttu-id="9b1e9-126">接下來，建立名為的角色*管理員*按一下**建立或管理角色**連結 （請參閱圖 3）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-126">Next, create a role named *Administrators* by clicking the **Create or Manage roles** link (see Figure 3).</span></span>

<span data-ttu-id="9b1e9-127">**圖 3 – 建立新的角色**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-127">**Figure 3 – Creating a new role**</span></span>

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

<span data-ttu-id="9b1e9-129">最後，建立名為 Sally 的新使用者並 Sally 關聯的系統管理員角色，按一下 [建立使用者] 連結，然後建立 Sally 時，請選取系統管理員 （請參閱圖 4）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-129">Finally, create a new user named Sally and associate Sally with the Administrators role by clicking the Create User link and selecting Administrators when creating Sally (see Figure 4).</span></span>

<span data-ttu-id="9b1e9-130">**圖 4-新增使用者至角色**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-130">**Figure 4 – Adding a user to a role**</span></span>

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

<span data-ttu-id="9b1e9-132">當完成後時，您應該有兩個名為 Stephen 和 Sally 的新使用者。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-132">When all is said and done, you should have two new users named Stephen and Sally.</span></span> <span data-ttu-id="9b1e9-133">您也應該擁有名為系統管理員的新角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-133">You should also have a new role named Administrators.</span></span> <span data-ttu-id="9b1e9-134">Sally 是系統管理員角色的成員，且 Stephen。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-134">Sally is a member of the Administrators role and Stephen is not.</span></span>

#### <a name="requiring-authorization"></a><span data-ttu-id="9b1e9-135">需要授權</span><span class="sxs-lookup"><span data-stu-id="9b1e9-135">Requiring Authorization</span></span>

<span data-ttu-id="9b1e9-136">您可以要求使用者在使用者叫用 [Authorize] 屬性加入至動作的控制器動作之前，進行驗證。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-136">You can require a user to be authenticated before the user invokes a controller action by adding the [Authorize] attribute to the action.</span></span> <span data-ttu-id="9b1e9-137">您可以將 [Authorize] 屬性套用至個別的控制器動作，或您可以將此屬性套用至整個控制器類別。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-137">You can apply the [Authorize] attribute to an individual controller action or you can apply this attribute to an entire controller class.</span></span>

<span data-ttu-id="9b1e9-138">例如，列表 1 中的控制站會公開名為 CompanySecrets() 的動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-138">For example, the controller in Listing 1 exposes an action named CompanySecrets().</span></span> <span data-ttu-id="9b1e9-139">因為此動作會以 [Authorize] 屬性裝飾，除非使用者已驗證無法呼叫此動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-139">Because this action is decorated with the [Authorize] attribute, this action cannot be invoked unless a user is authenticated.</span></span>

<span data-ttu-id="9b1e9-140">**列出 1 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-140">**Listing 1 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

<span data-ttu-id="9b1e9-141">如果您藉由在您的瀏覽器的網址列中輸入 URL /Home/CompanySecrets 叫用 CompanySecrets() 動作，而且您不是已驗證的使用者，則您會重新導向至登入檢視自動 （請參閱圖 5）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-141">If you invoke the CompanySecrets() action by entering the URL /Home/CompanySecrets in the address bar of your browser, and you are not an authenticated user, then you will be redirected to the Login view automatically (see Figure 5).</span></span>

<span data-ttu-id="9b1e9-142">**圖 5-登入檢視**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-142">**Figure 5 – The Login view**</span></span>

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

<span data-ttu-id="9b1e9-144">輸入您的使用者名稱和密碼，您可以使用登入檢視。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-144">You can use the Login view to enter your user name and password.</span></span> <span data-ttu-id="9b1e9-145">如果您不是已註冊的使用者，則您可以按一下**註冊**連結以瀏覽至登錄檢視 （請參閱圖 6）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-145">If you are not a registered user then you can click the **register** link to navigate to the Register view (see Figure 6).</span></span> <span data-ttu-id="9b1e9-146">您可以使用 [暫存器] 檢視來建立新的使用者帳戶。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-146">You can use the Register view to create a new user account.</span></span>

<span data-ttu-id="9b1e9-147">**圖 6 – 暫存器檢視**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-147">**Figure 6 – The Register view**</span></span>

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

<span data-ttu-id="9b1e9-149">已成功登入之後，您可以看到 CompanySecrets 檢視 （請參閱圖 7）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-149">After you successfully log in, you can see the CompanySecrets view (see Figure 7).</span></span> <span data-ttu-id="9b1e9-150">根據預設，您將繼續登入，直到您關閉瀏覽器視窗。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-150">By default, you will continue to be logged in until you close your browser window.</span></span>

<span data-ttu-id="9b1e9-151">**圖 7 – CompanySecrets 檢視**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-151">**Figure 7 – The CompanySecrets view**</span></span>

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a><span data-ttu-id="9b1e9-153">授權的使用者名稱或使用者角色</span><span class="sxs-lookup"><span data-stu-id="9b1e9-153">Authorizing by User Name or User Role</span></span>

<span data-ttu-id="9b1e9-154">您可以使用 [Authorize] 屬性來限制控制器動作，以一組特定的使用者或一組特定的使用者角色存取。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-154">You can use the [Authorize] attribute to restrict access to a controller action to a particular set of users or a particular set of user roles.</span></span> <span data-ttu-id="9b1e9-155">例如，修改的主控制器清單 2 中包含兩個名為 StephenSecrets() 和 AdministratorSecrets() 的新動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-155">For example, the modified Home controller in Listing 2 contains two new actions named StephenSecrets() and AdministratorSecrets().</span></span>

<span data-ttu-id="9b1e9-156">**Listing 2 – Controllers\HomeController.vb**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-156">**Listing 2 – Controllers\HomeController.vb**</span></span>

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

<span data-ttu-id="9b1e9-157">只有具有使用者名稱 Stephen 使用者可以叫用 StephenSecrets() 動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-157">Only a user with the user name Stephen can invoke the StephenSecrets() action.</span></span> <span data-ttu-id="9b1e9-158">所有其他使用者取得重新導向至登入檢視。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-158">All other users get redirected to the Login view.</span></span> <span data-ttu-id="9b1e9-159">使用者屬性可以接受以逗號分隔清單的使用者帳戶名稱。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-159">The Users property accepts a comma separated list of user account names.</span></span>

<span data-ttu-id="9b1e9-160">只有系統管理員角色中的使用者可以叫用 AdministratorSecrets() 動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-160">Only users in the Administrators role can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="9b1e9-161">例如，因為 Sally 是 Administrators 群組的成員，她可以叫用 AdministratorSecrets() 動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-161">For example, because Sally is a member of the Administrators group, she can invoke the AdministratorSecrets() action.</span></span> <span data-ttu-id="9b1e9-162">所有其他使用者取得重新導向至登入檢視。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-162">All other users get redirected to the Login view.</span></span> <span data-ttu-id="9b1e9-163">角色屬性可以接受以逗號分隔清單的角色名稱。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-163">The Roles property accepts a comma separated list of role names.</span></span>

#### <a name="configuring-authentication"></a><span data-ttu-id="9b1e9-164">設定驗證</span><span class="sxs-lookup"><span data-stu-id="9b1e9-164">Configuring Authentication</span></span>

<span data-ttu-id="9b1e9-165">此時，您可能會想知道所儲存的使用者帳戶和角色資訊。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-165">At this point, you might be wondering where the user account and role information is being stored.</span></span> <span data-ttu-id="9b1e9-166">根據預設，資訊會儲存在 MVC 應用程式的應用程式中 qr 具名的 ASPNETDB.mdf (RANU) SQL Express 的資料庫\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-166">By default, the information is stored in a (RANU) SQL Express database named ASPNETDB.mdf located in your MVC application's App\_Data folder.</span></span> <span data-ttu-id="9b1e9-167">這個資料庫是由 ASP.NET 架構時自動產生您開始使用成員資格。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-167">This database is generated by the ASP.NET framework automatically when you start using membership.</span></span>

<span data-ttu-id="9b1e9-168">若要查看 ASPNETDB.mdf 資料庫，在 [方案總管] 視窗中的，您首先要選取功能表選項專案中，顯示所有檔案。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-168">In order to see the ASPNETDB.mdf database in the Solution Explorer window, you first need to select the menu option Project, Show All Files.</span></span>

<span data-ttu-id="9b1e9-169">開發應用程式時，使用預設 SQL Express 資料庫是正常的。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-169">Using the default SQL Express database is fine when developing an application.</span></span> <span data-ttu-id="9b1e9-170">最可能的原因不過，您不想要用於生產應用程式的預設 ASPNETDB.mdf 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-170">Most likely, however, you won't want to use the default ASPNETDB.mdf database for a production application.</span></span> <span data-ttu-id="9b1e9-171">在此情況下，您可以變更下列兩個步驟，以儲存使用者帳戶資訊：</span><span class="sxs-lookup"><span data-stu-id="9b1e9-171">In that case, you can change where user account information is stored by completing the following two steps:</span></span>

1. <span data-ttu-id="9b1e9-172">將應用程式服務資料庫物件加入至您的生產資料庫-變更您的應用程式的連接字串以指向您的生產資料庫</span><span class="sxs-lookup"><span data-stu-id="9b1e9-172">Add the Application Services database objects to your production database - Change your application connection string to point to your production database</span></span>

<span data-ttu-id="9b1e9-173">第一個步驟會將所有必要的資料庫物件 （資料表和預存程序） 為您的生產資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-173">The first step is to add all of the necessary database objects (tables and stored procedures) to your production database.</span></span> <span data-ttu-id="9b1e9-174">若要將這些物件加入至新的資料庫最簡單的方式是利用 ASP.NET SQL Server 安裝精靈 （請參閱圖 8）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-174">The easiest way to add these objects to a new database is to take advantage of the ASP.NET SQL Server Setup Wizard (see Figure 8).</span></span> <span data-ttu-id="9b1e9-175">您可以啟動此工具，從 Microsoft Visual Studio 2008 程式群組開啟 Visual Studio 2008 命令提示字元，並從命令提示字元執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9b1e9-175">You can launch this tool by opening the Visual Studio 2008 Command Prompt from the Microsoft Visual Studio 2008 program group and executing the following command from the command prompt:</span></span>

<span data-ttu-id="9b1e9-176">aspnet\_regsql</span><span class="sxs-lookup"><span data-stu-id="9b1e9-176">aspnet\_regsql</span></span>

<span data-ttu-id="9b1e9-177">**圖 8 – ASP.NET SQL Server 安裝精靈**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-177">**Figure 8 – The ASP.NET SQL Server Setup Wizard**</span></span>

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

<span data-ttu-id="9b1e9-179">ASP.NET SQL Server 安裝精靈可讓您選取網路上的 SQL Server 資料庫，然後安裝所有 ASP.NET 應用程式服務所需的資料庫物件。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-179">The ASP.NET SQL Server Setup Wizard enables you to select a SQL Server database on your network and install all of the database objects required by the ASP.NET application services.</span></span> <span data-ttu-id="9b1e9-180">資料庫伺服器不需要放在本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-180">The database server is not required to be located on your local machine.</span></span>

> [!NOTE]
> <span data-ttu-id="9b1e9-181">如果您不想要使用 ASP.NET SQL Server 安裝精靈，您可以新增應用程式服務的資料庫物件的下列資料夾中找到 SQL 指令碼：</span><span class="sxs-lookup"><span data-stu-id="9b1e9-181">If you don't want to use the ASP.NET SQL Server Setup Wizard, then you can find SQL scripts for adding the application services database objects in the following folder:</span></span>
> 
> 
> <span data-ttu-id="9b1e9-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span><span class="sxs-lookup"><span data-stu-id="9b1e9-182">C:\Windows\Microsoft.NET\Framework\v2.0.50727</span></span>


<span data-ttu-id="9b1e9-183">建立必要的資料庫物件之後，您需要修改您的 MVC 應用程式所使用的資料庫連接。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-183">After you create the necessary database objects, you need to modify the database connection used by your MVC application.</span></span> <span data-ttu-id="9b1e9-184">修改 ApplicationServices 連接字串，在您 web 組態檔 (web.config) 中的，讓它指向實際執行資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-184">Modify the ApplicationServices connection string in your web configuration (web.config) file so that it points to the production database.</span></span> <span data-ttu-id="9b1e9-185">例如，範例 3 中修改過的連接會指向名為的 MyProductionDB （原始 ApplicationServices 連接字串已經被標記為註解） 的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-185">For example, the modified connection in Listing 3 points to a database named MyProductionDB (the original ApplicationServices connection string has been commented out).</span></span>

<span data-ttu-id="9b1e9-186">**列出 3 – Web.config**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-186">**Listing 3 – Web.config**</span></span>

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a><span data-ttu-id="9b1e9-187">設定資料庫權限</span><span class="sxs-lookup"><span data-stu-id="9b1e9-187">Configuring Database Permissions</span></span>

<span data-ttu-id="9b1e9-188">如果您使用整合式安全性來連接到您的資料庫將會需要將正確的 Windows 使用者帳戶做為登入加入至您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-188">If you use Integrated Security to connect to your database then you will need to add the correct Windows user account as a login to your database.</span></span> <span data-ttu-id="9b1e9-189">正確的帳戶取決於您使用 ASP.NET 程式開發伺服器或 Internet Information Services 為網頁伺服器。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-189">The correct account depends on whether you are using the ASP.NET Development Server or Internet Information Services as your web server.</span></span> <span data-ttu-id="9b1e9-190">正確的使用者帳戶也會取決於您的作業系統。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-190">The correct user account also depends on your operating system.</span></span>

<span data-ttu-id="9b1e9-191">如果您使用 ASP.NET 程式開發伺服器 （供 Visual Studio 的預設 web 伺服器） 應用程式執行您的 Windows 使用者帳戶的內容中。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-191">If you are using the ASP.NET Development Server (the default web server used by Visual Studio) then your application executes within the context of your Windows user account.</span></span> <span data-ttu-id="9b1e9-192">在此情況下，您需要加入您的 Windows 使用者帳戶做為資料庫伺服器登入。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-192">In that case, you need to add your Windows user account as a database server login.</span></span>

<span data-ttu-id="9b1e9-193">或者，如果您使用 Internet Information Services 然後您需要將 ASPNET 帳戶或 NT 授權單位/網路服務帳戶新增為資料庫伺服器登入。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-193">Alternatively, if you are using Internet Information Services then you need to add either the ASPNET account or the NT AUTHORITY/NETWORK SERVICE account as a database server login.</span></span> <span data-ttu-id="9b1e9-194">如果您使用 Windows XP，請將 ASPNET 帳戶做為登入加入至您的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-194">If you are using Windows XP then add the ASPNET account as a login to your database.</span></span> <span data-ttu-id="9b1e9-195">如果您使用較新作業系統，例如 Windows Vista 或 Windows Server 2008，請將 NT 授權單位/網路服務帳戶新增為資料庫登入。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-195">If you are using a more recent operating system, such as Windows Vista or Windows Server 2008, then add the NT AUTHORITY/NETWORK SERVICE account as the database login.</span></span>

<span data-ttu-id="9b1e9-196">可以使用 Microsoft SQL Server Management Studio 將新的使用者帳戶加入至您的資料庫 （請參閱圖 9）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-196">You can add a new user account to your database by using Microsoft SQL Server Management Studio (see Figure 9).</span></span>

<span data-ttu-id="9b1e9-197">**圖 9 – 建立新的 Microsoft SQL Server 登入**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-197">**Figure 9 – Creating a new Microsoft SQL Server login**</span></span>

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

<span data-ttu-id="9b1e9-199">建立必要的登入之後，您需要登入對應到具有正確的資料庫角色的資料庫使用者。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-199">After you create the required login, you need to map the login to a database user with the right database roles.</span></span> <span data-ttu-id="9b1e9-200">按兩下登入，並選取 [使用者對應] 索引標籤。選取一或多個應用程式服務資料庫角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-200">Double-click the login and select the User Mapping tab. Select one or more application services database roles.</span></span> <span data-ttu-id="9b1e9-201">比方說，若要驗證的使用者，您必須啟用 aspnet\_成員資格\_BasicAccess 資料庫角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-201">For example, in order to authenticate users, you need to enable the aspnet\_Membership\_BasicAccess database role.</span></span> <span data-ttu-id="9b1e9-202">若要建立新的使用者，您必須啟用 aspnet\_成員資格\_FullAccess 資料庫角色 （請參閱圖 10）。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-202">In order to create new users, you need to enable the aspnet\_Membership\_FullAccess database role (see Figure 10).</span></span>

<span data-ttu-id="9b1e9-203">**圖 10 – 新增應用程式服務資料庫角色**</span><span class="sxs-lookup"><span data-stu-id="9b1e9-203">**Figure 10 – Adding Application Services database roles**</span></span>

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a><span data-ttu-id="9b1e9-205">總結</span><span class="sxs-lookup"><span data-stu-id="9b1e9-205">Summary</span></span>

<span data-ttu-id="9b1e9-206">在本教學課程中，您學會如何建立 ASP.NET MVC 應用程式時，使用表單驗證。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-206">In this tutorial, you learned how to use Forms authentication when building an ASP.NET MVC application.</span></span> <span data-ttu-id="9b1e9-207">首先，您學到如何利用網站管理工具建立新的使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-207">First, you learned how to create new users and roles by taking advantage of the Web Site Administration Tool.</span></span> <span data-ttu-id="9b1e9-208">接下來，您學會如何使用 [Authorize] 屬性，以避免未經授權的使用者叫用控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-208">Next, you learned how to use the [Authorize] attribute to prevent unauthorized users from invoking controller actions.</span></span> <span data-ttu-id="9b1e9-209">最後，您學會如何設定您的 MVC 應用程式，將使用者和角色資訊儲存在實際執行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="9b1e9-209">Finally, you learned how to configure your MVC application to store user and role information in a production database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9b1e9-210">[上一頁](preventing-javascript-injection-attacks-cs.md)
> [下一頁](authenticating-users-with-windows-authentication-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9b1e9-210">[Previous](preventing-javascript-injection-attacks-cs.md)
[Next](authenticating-users-with-windows-authentication-vb.md)</span></span>
