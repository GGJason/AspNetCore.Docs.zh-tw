---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: 使用資料註解驗證程式 (VB) 驗證 |Microsoft 文件
author: microsoft
description: 利用資料的附註模型繫結器來執行 ASP.NET MVC 應用程式中的驗證。 了解如何使用不同類型的驗證程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: d1987182a44a0ad3f91f455342dc934d1dd50267
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870163"
---
<a name="validation-with-the-data-annotation-validators-vb"></a><span data-ttu-id="c8446-104">使用資料註解驗證程式 (VB) 驗證</span><span class="sxs-lookup"><span data-stu-id="c8446-104">Validation with the Data Annotation Validators (VB)</span></span>
====================
<span data-ttu-id="c8446-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="c8446-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="c8446-106">利用資料的附註模型繫結器來執行 ASP.NET MVC 應用程式中的驗證。</span><span class="sxs-lookup"><span data-stu-id="c8446-106">Take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="c8446-107">了解如何使用不同類型的驗證程式屬性，並在 Microsoft Entity Framework 中使用它們。</span><span class="sxs-lookup"><span data-stu-id="c8446-107">Learn how to use the different types of validator attributes and work with them in the Microsoft Entity Framework.</span></span>


<span data-ttu-id="c8446-108">在本教學課程中，您會學習如何使用資料註解驗證程式在 ASP.NET MVC 應用程式中執行驗證。</span><span class="sxs-lookup"><span data-stu-id="c8446-108">In this tutorial, you learn how to use the Data Annotation validators to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="c8446-109">使用資料註解驗證器的優點是可讓您執行驗證，只要新增一或多個屬性 – 例如 Required 或 StringLength 屬性 – 至類別內容。</span><span class="sxs-lookup"><span data-stu-id="c8446-109">The advantage of using the Data Annotation validators is that they enable you to perform validation simply by adding one or more attributes – such as the Required or StringLength attribute – to a class property.</span></span>

<span data-ttu-id="c8446-110">您可以使用資料註解驗證器之前，您必須下載資料註解模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="c8446-110">Before you can use the Data Annotation validators, you must download the Data Annotations Model Binder.</span></span> <span data-ttu-id="c8446-111">您可以從 CodePlex 網站下載資料註解的模型繫結器範例，依序按一下[這裡](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)。</span><span class="sxs-lookup"><span data-stu-id="c8446-111">You can download the Data Annotations Model Binder Sample from the CodePlex website by clicking [here](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).</span></span>


<span data-ttu-id="c8446-112">請務必了解資料註解模型繫結器不是官方 Microsoft ASP.NET MVC framework 的一部分。</span><span class="sxs-lookup"><span data-stu-id="c8446-112">It is important to understand that the Data Annotations Model Binder is not an official part of the Microsoft ASP.NET MVC framework.</span></span> <span data-ttu-id="c8446-113">雖然資料註解模型繫結器 Microsoft ASP.NET MVC 團隊所建立，但是 Microsoft 不提供資料註解模型繫結器提供官方產品支援描述，在本教學課程中使用。</span><span class="sxs-lookup"><span data-stu-id="c8446-113">Although the Data Annotations Model Binder was created by the Microsoft ASP.NET MVC team, Microsoft does not offer official product support for the Data Annotations Model Binder described and used in this tutorial.</span></span>


## <a name="using-the-data-annotation-model-binder"></a><span data-ttu-id="c8446-114">使用資料註解模型繫結器</span><span class="sxs-lookup"><span data-stu-id="c8446-114">Using the Data Annotation Model Binder</span></span>

<span data-ttu-id="c8446-115">若要在 ASP.NET MVC 應用程式中使用資料註解模型繫結器，您需要新增 Microsoft.Web.Mvc.DataAnnotations.dll 組件和 System.ComponentModel.DataAnnotations.dll 組件的參考。</span><span class="sxs-lookup"><span data-stu-id="c8446-115">In order to use the Data Annotations Model Binder in an ASP.NET MVC application, you first need to add a reference to the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly.</span></span> <span data-ttu-id="c8446-116">選取功能表選項**專案中，加入參考**。</span><span class="sxs-lookup"><span data-stu-id="c8446-116">Select the menu option **Project, Add Reference**.</span></span> <span data-ttu-id="c8446-117">按一下 下一步**瀏覽**索引標籤上，瀏覽至您下載 （並解壓縮） 資料註解模型繫結器範例的位置 (請參閱**圖 1**)。</span><span class="sxs-lookup"><span data-stu-id="c8446-117">Next click the **Browse** tab and browse to the location where you downloaded (and unzipped) the Data Annotations Model Binder sample (see **Figure 1**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

<span data-ttu-id="c8446-118">**圖 1**： 將參考加入至資料註解模型繫結器 ([按一下以檢視完整大小的影像](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c8446-118">**Figure 1**: Adding a reference to the Data Annotations Model Binder ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image3.png))</span></span>

<span data-ttu-id="c8446-119">選取 Microsoft.Web.Mvc.DataAnnotations.dll 組件和 System.ComponentModel.DataAnnotations.dll 組件，然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="c8446-119">Select both the Microsoft.Web.Mvc.DataAnnotations.dll assembly and the System.ComponentModel.DataAnnotations.dll assembly and click the **OK** button.</span></span>


<span data-ttu-id="c8446-120">您無法使用 System.ComponentModel.DataAnnotations.dll 組件隨附於.NET Framework Service Pack 1 及資料註解模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="c8446-120">You cannot use the System.ComponentModel.DataAnnotations.dll assembly included with .NET Framework Service Pack 1 with the Data Annotations Model Binder.</span></span> <span data-ttu-id="c8446-121">您必須使用隨附的資料附註模型繫結器範例下載 System.ComponentModel.DataAnnotations.dll 組件的版本。</span><span class="sxs-lookup"><span data-stu-id="c8446-121">You must use the version of the System.ComponentModel.DataAnnotations.dll assembly included with the Data Annotations Model Binder Sample download.</span></span>


<span data-ttu-id="c8446-122">最後，您要在 Global.asax 檔案中註冊 DataAnnotations 模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="c8446-122">Finally, you need to register the DataAnnotations Model Binder in the Global.asax file.</span></span> <span data-ttu-id="c8446-123">將下列程式碼行加入至應用程式\_start （） 事件處理常式，讓應用程式\_start （） 方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="c8446-123">Add the following line of code to the Application\_Start() event handler so that the Application\_Start() method looks like this:</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

<span data-ttu-id="c8446-124">這行程式碼會註冊 DataAnnotationsModelBinder 為整個 ASP.NET MVC 應用程式的預設模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="c8446-124">This line of code registers the DataAnnotationsModelBinder as the default model binder for the entire ASP.NET MVC application.</span></span>

## <a name="using-the-data-annotation-validator-attributes"></a><span data-ttu-id="c8446-125">使用資料註解驗證程式屬性</span><span class="sxs-lookup"><span data-stu-id="c8446-125">Using the Data Annotation Validator Attributes</span></span>

<span data-ttu-id="c8446-126">當您使用資料註解模型繫結器時，您可以使用驗證程式屬性來執行驗證。</span><span class="sxs-lookup"><span data-stu-id="c8446-126">When you use the Data Annotations Model Binder, you use validator attributes to perform validation.</span></span> <span data-ttu-id="c8446-127">System.ComponentModel.DataAnnotations 命名空間包含下列的驗證程式屬性：</span><span class="sxs-lookup"><span data-stu-id="c8446-127">The System.ComponentModel.DataAnnotations namespace includes the following validator attributes:</span></span>

- <span data-ttu-id="c8446-128">範圍-可讓您驗證是否屬性的值介於指定的值範圍。</span><span class="sxs-lookup"><span data-stu-id="c8446-128">Range – Enables you to validate whether the value of a property falls between a specified range of values.</span></span>
- <span data-ttu-id="c8446-129">ReqularExpression – 可讓您以驗證是否屬性的值符合指定的規則運算式模式。</span><span class="sxs-lookup"><span data-stu-id="c8446-129">ReqularExpression – Enables you to validate whether the value of a property matches a specified regular expression pattern.</span></span>
- <span data-ttu-id="c8446-130">所需 – 可讓您將屬性標記為必要。</span><span class="sxs-lookup"><span data-stu-id="c8446-130">Required – Enables you to mark a property as required.</span></span>
- <span data-ttu-id="c8446-131">StringLength – 可讓您指定為字串屬性的最大長度。</span><span class="sxs-lookup"><span data-stu-id="c8446-131">StringLength – Enables you to specify a maximum length for a string property.</span></span>
- <span data-ttu-id="c8446-132">驗證-所有驗證程式屬性的基底類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-132">Validation – The base class for all validator attributes.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c8446-133">如果您的驗證需求不符合任何的標準驗證然後一定繼承自基底的驗證屬性的 新的驗證程式屬性，以建立自訂驗證程式屬性的選項。</span><span class="sxs-lookup"><span data-stu-id="c8446-133">If your validation needs are not satisfied by any of the standard validators then you always have the option of creating a custom validator attribute by inheriting a new validator attribute from the base Validation attribute.</span></span>


<span data-ttu-id="c8446-134">中的產品類別**列出 1**說明如何使用這些驗證程式屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-134">The Product class in **Listing 1** illustrates how to use these validator attributes.</span></span> <span data-ttu-id="c8446-135">名稱、 描述和 UnitPrice 屬性會標示為必要。</span><span class="sxs-lookup"><span data-stu-id="c8446-135">The Name, Description, and UnitPrice properties are marked as required.</span></span> <span data-ttu-id="c8446-136">Name 屬性必須是少於 10 個字元的字串長度。</span><span class="sxs-lookup"><span data-stu-id="c8446-136">The Name property must have a string length that is less than 10 characters.</span></span> <span data-ttu-id="c8446-137">最後，UnitPrice 屬性必須符合規則運算式模式，表示貨幣金額。</span><span class="sxs-lookup"><span data-stu-id="c8446-137">Finally, the UnitPrice property must match a regular expression pattern that represents a currency amount.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

<span data-ttu-id="c8446-138">**清單 1**: Models\Product.vb</span><span class="sxs-lookup"><span data-stu-id="c8446-138">**Listing 1**: Models\Product.vb</span></span>

<span data-ttu-id="c8446-139">產品類別將說明如何使用一個額外的屬性： DisplayName 屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-139">The Product class illustrates how to use one additional attribute: the DisplayName attribute.</span></span> <span data-ttu-id="c8446-140">DisplayName 屬性可讓您修改的屬性名稱時顯示錯誤訊息中的屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-140">The DisplayName attribute enables you to modify the name of the property when the property is displayed in an error message.</span></span> <span data-ttu-id="c8446-141">而不是顯示錯誤訊息"UnitPrice 欄位必要的 「 您可以顯示錯誤訊息 「 [價格] 欄位是必要的 」。</span><span class="sxs-lookup"><span data-stu-id="c8446-141">Instead of displaying the error message "The UnitPrice field is required" you can display the error message "The Price field is required".</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c8446-142">如果您想要自訂驗證程式所顯示的錯誤訊息然後您就可以指派自訂錯誤訊息給驗證程式的錯誤訊息屬性，就像這樣： `<Required(ErrorMessage:="This field needs a value!")>`</span><span class="sxs-lookup"><span data-stu-id="c8446-142">If you want to completely customize the error message displayed by a validator then you can assign a custom error message to the validator's ErrorMessage property like this: `<Required(ErrorMessage:="This field needs a value!")>`</span></span>


<span data-ttu-id="c8446-143">您可以使用中的產品類別**列出 1** create （） 的控制器動作中**列出 2**。</span><span class="sxs-lookup"><span data-stu-id="c8446-143">You can use the Product class in **Listing 1** with the Create() controller action in **Listing 2**.</span></span> <span data-ttu-id="c8446-144">模型狀態中包含的任何錯誤時，此控制器動作重新顯示建立檢視。</span><span class="sxs-lookup"><span data-stu-id="c8446-144">This controller action redisplays the Create view when model state contains any errors.</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

<span data-ttu-id="c8446-145">**表 2**: Controllers\ProductController.vb</span><span class="sxs-lookup"><span data-stu-id="c8446-145">**Listing 2**: Controllers\ProductController.vb</span></span>

<span data-ttu-id="c8446-146">最後，您可以建立的檢視**列出 3** create （） 動作上按一下滑鼠右鍵，然後選取功能表選項**加入檢視**。</span><span class="sxs-lookup"><span data-stu-id="c8446-146">Finally, you can create the view in **Listing 3** by right-clicking the Create() action and selecting the menu option **Add View**.</span></span> <span data-ttu-id="c8446-147">建立強型別檢視與產品類別，為模型類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-147">Create a strongly-typed view with the Product class as the model class.</span></span> <span data-ttu-id="c8446-148">選取**建立**從檢視內容的下拉式清單 (請參閱**圖 2**)。</span><span class="sxs-lookup"><span data-stu-id="c8446-148">Select **Create** from the view content dropdown list (see **Figure 2**).</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

<span data-ttu-id="c8446-149">**圖 2**： 加入建立檢視</span><span class="sxs-lookup"><span data-stu-id="c8446-149">**Figure 2**: Adding the Create View</span></span>

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

<span data-ttu-id="c8446-150">**列出 3**: Views\Product\Create.aspx</span><span class="sxs-lookup"><span data-stu-id="c8446-150">**Listing 3**: Views\Product\Create.aspx</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c8446-151">移除所產生的建立表單的 [識別碼] 欄位**加入檢視**功能表選項。</span><span class="sxs-lookup"><span data-stu-id="c8446-151">Remove the Id field from the Create form generated by the **Add View** menu option.</span></span> <span data-ttu-id="c8446-152">由於 [識別碼] 欄位會對應到 Identity 資料行，您不想允許使用者輸入此欄位的值。</span><span class="sxs-lookup"><span data-stu-id="c8446-152">Because the Id field corresponds to an Identity column, you don't want to allow users to enter a value for this field.</span></span>


<span data-ttu-id="c8446-153">如果您送出的表單建立產品，而且您沒有輸入值為必要欄位，則驗證錯誤訊息中**圖 3**會顯示。</span><span class="sxs-lookup"><span data-stu-id="c8446-153">If you submit the form for creating a Product and you do not enter values for the required fields, then the validation error messages in **Figure 3** are displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

<span data-ttu-id="c8446-154">**圖 3**： 遺漏必要的欄位</span><span class="sxs-lookup"><span data-stu-id="c8446-154">**Figure 3**: Missing required fields</span></span>

<span data-ttu-id="c8446-155">如果您輸入無效的貨幣金額，則錯誤訊息中的**圖 4**隨即出現。</span><span class="sxs-lookup"><span data-stu-id="c8446-155">If you enter an invalid currency amount, then the error message in **Figure 4** is displayed.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

<span data-ttu-id="c8446-156">**圖 4**： 無效的貨幣金額</span><span class="sxs-lookup"><span data-stu-id="c8446-156">**Figure 4**: Invalid currency amount</span></span>

## <a name="using-data-annotation-validators-with-the-entity-framework"></a><span data-ttu-id="c8446-157">Entity Framework 搭配使用資料註解驗證器</span><span class="sxs-lookup"><span data-stu-id="c8446-157">Using Data Annotation Validators with the Entity Framework</span></span>

<span data-ttu-id="c8446-158">如果您使用 Microsoft Entity Framework 來產生資料模型類別然後您就無法套用的驗證程式屬性直接加入您的類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-158">If you are using the Microsoft Entity Framework to generate your data model classes then you cannot apply the validator attributes directly to your classes.</span></span> <span data-ttu-id="c8446-159">因為 Entity Framework Designer 產生的模型類別，您對模型類別進行任何變更將會被覆寫的下次您在設計工具中進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="c8446-159">Because the Entity Framework Designer generates the model classes, any changes you make to the model classes will be overwritten the next time you make any changes in the Designer.</span></span>

<span data-ttu-id="c8446-160">如果您想要使用 Entity Framework 所產生的類別中的驗證程式您需要建立中繼資料類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-160">If you want to use the validators with the classes generated by the Entity Framework then you need to create meta data classes.</span></span> <span data-ttu-id="c8446-161">您套用中繼資料類別，而不是實際的類別中套用的驗證程式驗證程式。</span><span class="sxs-lookup"><span data-stu-id="c8446-161">You apply the validators to the meta data class instead of applying the validators to the actual class.</span></span>

<span data-ttu-id="c8446-162">例如，假設您已建立使用 Entity Framework 的電影類別 (請參閱**圖 5**)。</span><span class="sxs-lookup"><span data-stu-id="c8446-162">For example, imagine that you have created a Movie class using the Entity Framework (see **Figure 5**).</span></span> <span data-ttu-id="c8446-163">此外，假設您想要將電影標題和主管屬性的必要的屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-163">Imagine, furthermore, that you want to make the Movie Title and Director properties required properties.</span></span> <span data-ttu-id="c8446-164">在此情況下，您可以在其中建立部分類別和中的中繼資料類別**列出 4**。</span><span class="sxs-lookup"><span data-stu-id="c8446-164">In that case, you can create the partial class and meta data class in **Listing 4**.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

<span data-ttu-id="c8446-165">**圖 5**: Entity Framework 所產生的電影類別</span><span class="sxs-lookup"><span data-stu-id="c8446-165">**Figure 5**: Movie class generated by Entity Framework</span></span>

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

<span data-ttu-id="c8446-166">**列出 4**: Models\Movie.vb</span><span class="sxs-lookup"><span data-stu-id="c8446-166">**Listing 4**: Models\Movie.vb</span></span>

<span data-ttu-id="c8446-167">中的檔案**列出 4**包含名為電影和 MovieMetaData 的兩個類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-167">The file in **Listing 4** contains two classes named Movie and MovieMetaData.</span></span> <span data-ttu-id="c8446-168">影片類別是部分類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-168">The Movie class is a partial class.</span></span> <span data-ttu-id="c8446-169">它會對應至 DataModel.Designer.vb 檔案中包含的 Entity Framework 所產生的部分類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-169">It corresponds to the partial class generated by the Entity Framework that is contained in the DataModel.Designer.vb file.</span></span>

<span data-ttu-id="c8446-170">目前，.NET framework 不支援部分屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-170">Currently, the .NET framework does not support partial properties.</span></span> <span data-ttu-id="c8446-171">因此，沒有任何方法來驗證程式屬性會套用到透過將驗證程式屬性套用至電影類別檔中定義的屬性定義 DataModel.Designer.vb 檔案中的影片類別屬性**列出 4**.</span><span class="sxs-lookup"><span data-stu-id="c8446-171">Therefore, there is no way to apply the validator attributes to the properties of the Movie class defined in the DataModel.Designer.vb file by applying the validator attributes to the properties of the Movie class defined in the file in **Listing 4**.</span></span>

<span data-ttu-id="c8446-172">請注意，以指向 MovieMetaData 類別 MetadataType 屬性裝飾影片部分類別。</span><span class="sxs-lookup"><span data-stu-id="c8446-172">Notice that the Movie partial class is decorated with a MetadataType attribute that points at the MovieMetaData class.</span></span> <span data-ttu-id="c8446-173">MovieMetaData 類別包含內容的影片類別的 proxy 屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-173">The MovieMetaData class contains proxy properties for the properties of the Movie class.</span></span>

<span data-ttu-id="c8446-174">驗證程式屬性套用至 MovieMetaData 類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-174">The validator attributes are applied to the properties of the MovieMetaData class.</span></span> <span data-ttu-id="c8446-175">所有標記為必要的屬性標題、 導演和 DateReleased 屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-175">The Title, Director, and DateReleased properties are all marked as required properties.</span></span> <span data-ttu-id="c8446-176">主管屬性必須被指派包含 5 個字元的字串。</span><span class="sxs-lookup"><span data-stu-id="c8446-176">The Director property must be assigned a string that contains less than 5 characters.</span></span> <span data-ttu-id="c8446-177">最後，DisplayName 屬性會套用到 DateReleased 屬性來顯示錯誤訊息一樣 」 此日期發行為必要欄位。 」</span><span class="sxs-lookup"><span data-stu-id="c8446-177">Finally, the DisplayName attribute is applied to the DateReleased property to display an error message like "The Date Released field is required."</span></span> <span data-ttu-id="c8446-178">而不是錯誤 「 DateReleased 為必要欄位。 」</span><span class="sxs-lookup"><span data-stu-id="c8446-178">instead of the error "The DateReleased field is required."</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="c8446-179">請注意，proxy 類別中的屬性 MovieMetaData 不需要代表影片類別中的對應屬性相同的類型。</span><span class="sxs-lookup"><span data-stu-id="c8446-179">Notice that the proxy properties in the MovieMetaData class do not need to represent the same types as the corresponding properties in the Movie class.</span></span> <span data-ttu-id="c8446-180">例如，主管屬性是電影類別中的字串屬性和 MovieMetaData 類別中的物件屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-180">For example, the Director property is a string property in the Movie class and an object property in the MovieMetaData class.</span></span>


<span data-ttu-id="c8446-181">在頁面**圖 6**說明影片屬性中輸入無效值時，傳回的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c8446-181">The page in **Figure 6** illustrates the error messages returned when you enter invalid values for the Movie properties.</span></span>

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

<span data-ttu-id="c8446-182">**圖 6**: Entity Framework 搭配使用的驗證程式 ([按一下以檢視完整大小的影像](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="c8446-182">**Figure 6**: Using validators with the Entity Framework ([Click to view full-size image](validation-with-the-data-annotation-validators-vb/_static/image14.png))</span></span>

## <a name="summary"></a><span data-ttu-id="c8446-183">總結</span><span class="sxs-lookup"><span data-stu-id="c8446-183">Summary</span></span>

<span data-ttu-id="c8446-184">在本教學課程中，您學會如何充分利用資料的附註模型繫結器來執行 ASP.NET MVC 應用程式中的驗證。</span><span class="sxs-lookup"><span data-stu-id="c8446-184">In this tutorial, you learned how to take advantage of the Data Annotation Model Binder to perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="c8446-185">您已學習如何使用不同的驗證程式屬性，例如 Required 和 StringLength 屬性類型。</span><span class="sxs-lookup"><span data-stu-id="c8446-185">You learned how to use the different types of validator attributes such as the Required and StringLength attributes.</span></span> <span data-ttu-id="c8446-186">您也學到如何使用 Microsoft Entity Framework 時，使用這些屬性。</span><span class="sxs-lookup"><span data-stu-id="c8446-186">You also learned how to use these attributes when working with the Microsoft Entity Framework.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c8446-187">上一步</span><span class="sxs-lookup"><span data-stu-id="c8446-187">Previous</span></span>](validating-with-a-service-layer-vb.md)
