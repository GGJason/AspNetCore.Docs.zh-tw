---
title: 在 ASP.NET Core Web API 中格式化回應資料
author: ardalis
description: 了解如何在 ASP.NET Core Web API 中格式化回應資料。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 04/17/2020
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
uid: web-api/advanced/formatting
ms.openlocfilehash: 8aa94bd1f33d1dd8ce8e7f50468ed60b4ccb2515
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88019933"
---
# <a name="format-response-data-in-aspnet-core-web-api"></a><span data-ttu-id="82249-103">在 ASP.NET Core Web API 中格式化回應資料</span><span class="sxs-lookup"><span data-stu-id="82249-103">Format response data in ASP.NET Core Web API</span></span>

<span data-ttu-id="82249-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="82249-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="82249-105">ASP.NET Core MVC 支援格式化回應資料。</span><span class="sxs-lookup"><span data-stu-id="82249-105">ASP.NET Core MVC has support for formatting response data.</span></span> <span data-ttu-id="82249-106">您可以使用特定格式或回應用戶端要求的格式來格式化回應資料。</span><span class="sxs-lookup"><span data-stu-id="82249-106">Response data can be formatted using specific formats or in response to client requested format.</span></span>

<span data-ttu-id="82249-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([如何下載](xref:index#how-to-download-a-sample)) </span><span class="sxs-lookup"><span data-stu-id="82249-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/advanced/formatting) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="format-specific-action-results"></a><span data-ttu-id="82249-108">特定格式的動作結果</span><span class="sxs-lookup"><span data-stu-id="82249-108">Format-specific Action Results</span></span>

<span data-ttu-id="82249-109">某些動作結果類型是特定格式所特有的，例如 <xref:Microsoft.AspNetCore.Mvc.JsonResult> 和 <xref:Microsoft.AspNetCore.Mvc.ContentResult>。</span><span class="sxs-lookup"><span data-stu-id="82249-109">Some action result types are specific to a particular format, such as <xref:Microsoft.AspNetCore.Mvc.JsonResult> and <xref:Microsoft.AspNetCore.Mvc.ContentResult>.</span></span> <span data-ttu-id="82249-110">無論用戶端喜好設定為何，動作都可以傳回以特定格式格式化的結果。</span><span class="sxs-lookup"><span data-stu-id="82249-110">Actions can return results that are formatted in a particular format, regardless of client preferences.</span></span> <span data-ttu-id="82249-111">例如，傳回會傳回 `JsonResult` JSON 格式的資料。</span><span class="sxs-lookup"><span data-stu-id="82249-111">For example, returning `JsonResult` returns JSON-formatted data.</span></span> <span data-ttu-id="82249-112">傳回 `ContentResult` 或字串會傳回純文字格式的字串資料。</span><span class="sxs-lookup"><span data-stu-id="82249-112">Returning `ContentResult` or a string returns plain-text-formatted string data.</span></span>

<span data-ttu-id="82249-113">動作不需要傳回任何特定的類型。</span><span class="sxs-lookup"><span data-stu-id="82249-113">An action isn't required to return any specific type.</span></span> <span data-ttu-id="82249-114">ASP.NET Core 支援任何物件傳回值。</span><span class="sxs-lookup"><span data-stu-id="82249-114">ASP.NET Core supports any object return value.</span></span>  <span data-ttu-id="82249-115">傳回不是型別物件之動作的結果， <xref:Microsoft.AspNetCore.Mvc.IActionResult> 會使用適當的 <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> 實作為序列化。</span><span class="sxs-lookup"><span data-stu-id="82249-115">Results from actions that return objects that are not <xref:Microsoft.AspNetCore.Mvc.IActionResult> types are serialized using the appropriate <xref:Microsoft.AspNetCore.Mvc.Formatters.IOutputFormatter> implementation.</span></span> <span data-ttu-id="82249-116">如需詳細資訊，請參閱<xref:web-api/action-return-types>。</span><span class="sxs-lookup"><span data-stu-id="82249-116">For more information, see <xref:web-api/action-return-types>.</span></span>

<span data-ttu-id="82249-117">內建 helper 方法會傳回 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> JSON 格式的資料：[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span><span class="sxs-lookup"><span data-stu-id="82249-117">The built-in helper method <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Ok*> returns JSON-formatted data: [!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_get)]</span></span>

<span data-ttu-id="82249-118">下載範例會傳回作者清單。</span><span class="sxs-lookup"><span data-stu-id="82249-118">The sample download returns the list of authors.</span></span> <span data-ttu-id="82249-119">使用 F12 瀏覽器開發人員工具或[Postman](https://www.getpostman.com/tools)搭配先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="82249-119">Using the F12 browser developer tools or [Postman](https://www.getpostman.com/tools) with the previous code:</span></span>

* <span data-ttu-id="82249-120">顯示包含**內容類型：** 的回應標頭 `application/json; charset=utf-8` 。</span><span class="sxs-lookup"><span data-stu-id="82249-120">The response header containing **content-type:** `application/json; charset=utf-8` is displayed.</span></span>
* <span data-ttu-id="82249-121">系統會顯示要求標頭。</span><span class="sxs-lookup"><span data-stu-id="82249-121">The request headers are displayed.</span></span> <span data-ttu-id="82249-122">例如， `Accept` 標頭。</span><span class="sxs-lookup"><span data-stu-id="82249-122">For example, the `Accept` header.</span></span> <span data-ttu-id="82249-123">`Accept`前面的程式碼會忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="82249-123">The `Accept` header is ignored by the preceding code.</span></span>

<span data-ttu-id="82249-124">若要傳回純文字格式化資料，請使用 <xref:Microsoft.AspNetCore.Mvc.ContentResult> 和 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Content%2A> 協助程式：</span><span class="sxs-lookup"><span data-stu-id="82249-124">To return plain text formatted data, use <xref:Microsoft.AspNetCore.Mvc.ContentResult> and the <xref:Microsoft.AspNetCore.Mvc.ControllerBase.Content%2A> helper:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_about)]

<span data-ttu-id="82249-125">在上述程式碼中， `Content-Type` 傳回的是 `text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="82249-125">In the preceding code, the `Content-Type` returned is `text/plain`.</span></span> <span data-ttu-id="82249-126">傳回字串傳遞 `Content-Type` `text/plain` 下列內容：</span><span class="sxs-lookup"><span data-stu-id="82249-126">Returning a string delivers `Content-Type` of `text/plain`:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_string)]

<span data-ttu-id="82249-127">針對具有多個傳回類型的動作，傳回 `IActionResult` 。</span><span class="sxs-lookup"><span data-stu-id="82249-127">For actions with multiple return types, return `IActionResult`.</span></span> <span data-ttu-id="82249-128">例如，根據執行的作業結果傳回不同的 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="82249-128">For example, returning different HTTP status codes based on the result of operations performed.</span></span>

## <a name="content-negotiation"></a><span data-ttu-id="82249-129">內容協調</span><span class="sxs-lookup"><span data-stu-id="82249-129">Content negotiation</span></span>

<span data-ttu-id="82249-130">當用戶端指定[Accept 標頭](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)時，就會發生內容協商。</span><span class="sxs-lookup"><span data-stu-id="82249-130">Content negotiation occurs when the client specifies an [Accept header](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).</span></span> <span data-ttu-id="82249-131">ASP.NET Core 使用的預設格式為[JSON](https://json.org/)。</span><span class="sxs-lookup"><span data-stu-id="82249-131">The default format used by ASP.NET Core is [JSON](https://json.org/).</span></span> <span data-ttu-id="82249-132">內容協商為：</span><span class="sxs-lookup"><span data-stu-id="82249-132">Content negotiation is:</span></span>

* <span data-ttu-id="82249-133">由執行 <xref:Microsoft.AspNetCore.Mvc.ObjectResult> 。</span><span class="sxs-lookup"><span data-stu-id="82249-133">Implemented by <xref:Microsoft.AspNetCore.Mvc.ObjectResult>.</span></span>
* <span data-ttu-id="82249-134">內建自 helper 方法所傳回的狀態碼特定動作結果。</span><span class="sxs-lookup"><span data-stu-id="82249-134">Built into the status code-specific action results returned from the helper methods.</span></span> <span data-ttu-id="82249-135">動作結果 helper 方法是以為基礎 `ObjectResult` 。</span><span class="sxs-lookup"><span data-stu-id="82249-135">The action results helper methods are based on `ObjectResult`.</span></span>

<span data-ttu-id="82249-136">傳回模型型別時，傳回型別會是 `ObjectResult` 。</span><span class="sxs-lookup"><span data-stu-id="82249-136">When a model type is returned,  the return type is `ObjectResult`.</span></span>

<span data-ttu-id="82249-137">下列動作方法使用 `Ok` 和 `NotFound` 協助程式方法：</span><span class="sxs-lookup"><span data-stu-id="82249-137">The following action method uses the `Ok` and `NotFound` helper methods:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_search)]

<span data-ttu-id="82249-138">根據預設，ASP.NET Core 支援 `application/json` 、 `text/json` 和 `text/plain` 媒體類型。</span><span class="sxs-lookup"><span data-stu-id="82249-138">By default, ASP.NET Core supports `application/json`, `text/json`, and `text/plain` media types.</span></span> <span data-ttu-id="82249-139">[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/tools)這類工具可以設定 `Accept` 要求標頭，以指定傳回格式。</span><span class="sxs-lookup"><span data-stu-id="82249-139">Tools such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/tools) can set the `Accept` request header to specify the return format.</span></span> <span data-ttu-id="82249-140">當 `Accept` 標頭包含伺服器支援的類型時，就會傳回該類型。</span><span class="sxs-lookup"><span data-stu-id="82249-140">When the `Accept` header contains a type the server supports, that type is returned.</span></span> <span data-ttu-id="82249-141">下一節將說明如何新增其他格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-141">The next section shows how to add additional formatters.</span></span>

<span data-ttu-id="82249-142">控制器動作可以傳回 Poco () 的簡單舊 CLR 物件。</span><span class="sxs-lookup"><span data-stu-id="82249-142">Controller actions can return POCOs (Plain Old CLR Objects).</span></span> <span data-ttu-id="82249-143">當 POCO 傳回時，執行時間會自動建立 `ObjectResult` 包裝物件的。</span><span class="sxs-lookup"><span data-stu-id="82249-143">When a POCO is returned, the runtime automatically creates an `ObjectResult` that wraps the object.</span></span> <span data-ttu-id="82249-144">用戶端會取得已格式化的序列化物件。</span><span class="sxs-lookup"><span data-stu-id="82249-144">The client gets the formatted serialized object.</span></span> <span data-ttu-id="82249-145">如果傳回的物件是 `null` ，則 `204 No Content` 會傳迴響應。</span><span class="sxs-lookup"><span data-stu-id="82249-145">If the object being returned is `null`, a `204 No Content` response is returned.</span></span>

<span data-ttu-id="82249-146">傳回物件類型：</span><span class="sxs-lookup"><span data-stu-id="82249-146">Returning an object type:</span></span>

[!code-csharp[](./formatting/sample/Controllers/AuthorsController.cs?name=snippet_alias)]

<span data-ttu-id="82249-147">在上述程式碼中，有效作者別名的要求會傳回 `200 OK` 含有作者資料的回應。</span><span class="sxs-lookup"><span data-stu-id="82249-147">In the preceding code, a request for a valid author alias returns a `200 OK` response with the author's data.</span></span> <span data-ttu-id="82249-148">對無效別名的要求會傳回 `204 No Content` 回應。</span><span class="sxs-lookup"><span data-stu-id="82249-148">A request for an invalid alias returns a `204 No Content` response.</span></span>

### <a name="the-accept-header"></a><span data-ttu-id="82249-149">Accept 標頭</span><span class="sxs-lookup"><span data-stu-id="82249-149">The Accept header</span></span>

<span data-ttu-id="82249-150">當*negotiation* `Accept` 要求中出現標頭時，就會進行內容協商。</span><span class="sxs-lookup"><span data-stu-id="82249-150">Content *negotiation* takes place when an `Accept` header appears in the request.</span></span> <span data-ttu-id="82249-151">當要求包含 accept 標頭時，ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="82249-151">When a request contains an accept header, ASP.NET Core:</span></span>

* <span data-ttu-id="82249-152">依照喜好設定順序，列舉 accept 標頭中的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="82249-152">Enumerates the media types in the accept header in preference order.</span></span>
* <span data-ttu-id="82249-153">嘗試尋找可以使用其中一種指定的格式產生回應的格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-153">Tries to find a formatter that can produce a response in one of the formats specified.</span></span>

<span data-ttu-id="82249-154">如果找不到可滿足用戶端要求的格式器，請 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="82249-154">If no formatter is found that can satisfy the client's request, ASP.NET Core:</span></span>

* <span data-ttu-id="82249-155">`406 Not Acceptable`如果 <xref:Microsoft.AspNetCore.Mvc.MvcOptions> 已設定，則會傳回，或-</span><span class="sxs-lookup"><span data-stu-id="82249-155">Returns `406 Not Acceptable` if <xref:Microsoft.AspNetCore.Mvc.MvcOptions> has been set, or -</span></span>
* <span data-ttu-id="82249-156">嘗試尋找可產生回應的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-156">Tries to find the first formatter that can produce a response.</span></span>

<span data-ttu-id="82249-157">如果未針對要求的格式設定格式器，則會使用可格式化物件的第一個格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-157">If no formatter is configured for the requested format, the first formatter that can format the object is used.</span></span> <span data-ttu-id="82249-158">如果 `Accept` 要求中未出現標頭：</span><span class="sxs-lookup"><span data-stu-id="82249-158">If no `Accept` header appears in the request:</span></span>

* <span data-ttu-id="82249-159">第一個可以處理物件的格式器是用來序列化回應。</span><span class="sxs-lookup"><span data-stu-id="82249-159">The first formatter that can handle the object is used to serialize the response.</span></span>
* <span data-ttu-id="82249-160">沒有任何進行中的協商。</span><span class="sxs-lookup"><span data-stu-id="82249-160">There isn't any negotiation taking place.</span></span> <span data-ttu-id="82249-161">伺服器正在決定要傳回的格式。</span><span class="sxs-lookup"><span data-stu-id="82249-161">The server is determining what format to return.</span></span>

<span data-ttu-id="82249-162">如果 Accept 標頭包含 `*/*` ，除非 `RespectBrowserAcceptHeader` 在上將設為 true，否則會忽略標頭 <xref:Microsoft.AspNetCore.Mvc.MvcOptions> 。</span><span class="sxs-lookup"><span data-stu-id="82249-162">If the Accept header contains `*/*`, the Header is ignored unless `RespectBrowserAcceptHeader` is set to true on <xref:Microsoft.AspNetCore.Mvc.MvcOptions>.</span></span>

### <a name="browsers-and-content-negotiation"></a><span data-ttu-id="82249-163">瀏覽器和內容協調</span><span class="sxs-lookup"><span data-stu-id="82249-163">Browsers and content negotiation</span></span>

<span data-ttu-id="82249-164">與一般 API 用戶端不同的是，網頁瀏覽器會提供 `Accept` 標頭。</span><span class="sxs-lookup"><span data-stu-id="82249-164">Unlike typical API clients, web browsers supply `Accept` headers.</span></span> <span data-ttu-id="82249-165">網頁瀏覽器會指定許多格式，包括萬用字元。</span><span class="sxs-lookup"><span data-stu-id="82249-165">Web browser specify many formats, including wildcards.</span></span> <span data-ttu-id="82249-166">根據預設，當架構偵測到要求來自瀏覽器時：</span><span class="sxs-lookup"><span data-stu-id="82249-166">By default, when the framework detects that the request is coming from a browser:</span></span>

* <span data-ttu-id="82249-167">`Accept`已忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="82249-167">The `Accept` header is ignored.</span></span>
* <span data-ttu-id="82249-168">除非另有設定，否則內容會以 JSON 格式傳回。</span><span class="sxs-lookup"><span data-stu-id="82249-168">The content is returned in JSON, unless otherwise configured.</span></span>

<span data-ttu-id="82249-169">使用 Api 時，這會在瀏覽器之間提供更一致的體驗。</span><span class="sxs-lookup"><span data-stu-id="82249-169">This provides a more consistent experience across browsers when consuming APIs.</span></span>

<span data-ttu-id="82249-170">若要設定應用程式以接受瀏覽器 accept 標頭，請將設 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> 為 `true` ：</span><span class="sxs-lookup"><span data-stu-id="82249-170">To configure an app to honor browser accept headers, set <xref:Microsoft.AspNetCore.Mvc.MvcOptions.RespectBrowserAcceptHeader> to `true`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupRespectBrowserAcceptHeader.cs?name=snippet)]
::: moniker-end

### <a name="configure-formatters"></a><span data-ttu-id="82249-171">設定格式器</span><span class="sxs-lookup"><span data-stu-id="82249-171">Configure formatters</span></span>

<span data-ttu-id="82249-172">需要支援其他格式的應用程式可以新增適當的 NuGet 套件，並設定支援。</span><span class="sxs-lookup"><span data-stu-id="82249-172">Apps that need to support additional formats can add the appropriate NuGet packages and configure support.</span></span> <span data-ttu-id="82249-173">輸入和輸出有個別的格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-173">There are separate formatters for input and output.</span></span> <span data-ttu-id="82249-174">[模型](xref:mvc/models/model-binding)系結會使用輸入格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-174">Input formatters are used by [Model Binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="82249-175">輸出格式器是用來格式化回應。</span><span class="sxs-lookup"><span data-stu-id="82249-175">Output formatters are used to format responses.</span></span> <span data-ttu-id="82249-176">如需建立自訂格式器的詳細資訊，請參閱[自訂格式化](xref:web-api/advanced/custom-formatters)器。</span><span class="sxs-lookup"><span data-stu-id="82249-176">For information on creating a custom formatter, see [Custom Formatters](xref:web-api/advanced/custom-formatters).</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="add-xml-format-support"></a><span data-ttu-id="82249-177">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="82249-177">Add XML format support</span></span>

<span data-ttu-id="82249-178">使用執行的 XML 格式器 <xref:System.Xml.Serialization.XmlSerializer> 是透過呼叫來設定 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> ：</span><span class="sxs-lookup"><span data-stu-id="82249-178">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/3.0sample/Startup.cs?name=snippet)]

<span data-ttu-id="82249-179">上述程式碼會使用來序列化結果 `XmlSerializer` 。</span><span class="sxs-lookup"><span data-stu-id="82249-179">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="82249-180">使用上述程式碼時，控制器方法會根據要求的標頭傳回適當的格式 `Accept` 。</span><span class="sxs-lookup"><span data-stu-id="82249-180">When using the preceding code, controller methods return the appropriate format based on the request's `Accept` header.</span></span>

### <a name="configure-systemtextjson-based-formatters"></a><span data-ttu-id="82249-181">設定 System.Text.Json-based 格式器</span><span class="sxs-lookup"><span data-stu-id="82249-181">Configure System.Text.Json-based formatters</span></span>

<span data-ttu-id="82249-182">`System.Text.Json` 型格式器可以使用 `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions` 來設定。</span><span class="sxs-lookup"><span data-stu-id="82249-182">Features for the `System.Text.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.JsonOptions.SerializerOptions`.</span></span>

```csharp
services.AddControllers().AddJsonOptions(options =>
{
    // Use the default property (Pascal) casing.
    options.JsonSerializerOptions.PropertyNamingPolicy = null;

    // Configure a custom converter.
    options.JsonSerializerOptions.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="82249-183">以每個動作為基礎的輸出序列化選項，可以使用進行設定 `JsonResult` 。</span><span class="sxs-lookup"><span data-stu-id="82249-183">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="82249-184">例如：</span><span class="sxs-lookup"><span data-stu-id="82249-184">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerOptions
    {
        WriteIndented = true,
    });
}
```

### <a name="add-newtonsoftjson-based-json-format-support"></a><span data-ttu-id="82249-185">新增 Newtonsoft.Json 型 JSON 格式支援</span><span class="sxs-lookup"><span data-stu-id="82249-185">Add Newtonsoft.Json-based JSON format support</span></span>

<span data-ttu-id="82249-186">在 ASP.NET Core 3.0 之前，預設使用的 JSON 格式器會使用 `Newtonsoft.Json` 封裝來執行。</span><span class="sxs-lookup"><span data-stu-id="82249-186">Prior to ASP.NET Core 3.0, the default used JSON formatters implemented using the `Newtonsoft.Json` package.</span></span> <span data-ttu-id="82249-187">在 ASP.NET Core 3.0 或更新版本中，預設 JSON 格式器是以 `System.Text.Json` 為基礎。</span><span class="sxs-lookup"><span data-stu-id="82249-187">In ASP.NET Core 3.0 or later, the default JSON formatters are based on `System.Text.Json`.</span></span> <span data-ttu-id="82249-188">`Newtonsoft.Json` [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) 在中安裝 NuGet 套件並加以設定，即可取得根據格式的格式器和功能的支援 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="82249-188">Support for `Newtonsoft.Json` based formatters and features is available by installing the [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package and configuring it in `Startup.ConfigureServices`.</span></span>

[!code-csharp[](./formatting/3.0sample/StartupNewtonsoftJson.cs?name=snippet)]

<span data-ttu-id="82249-189">某些功能可能不適用於以為基礎的格式器 `System.Text.Json` ，而且需要參考以為基礎的格式器 `Newtonsoft.Json` 。</span><span class="sxs-lookup"><span data-stu-id="82249-189">Some features may not work well with `System.Text.Json`-based formatters and require a reference to the `Newtonsoft.Json`-based formatters.</span></span> <span data-ttu-id="82249-190">`Newtonsoft.Json`如果應用程式有下列情況，請繼續使用以為基礎的格式器：</span><span class="sxs-lookup"><span data-stu-id="82249-190">Continue using the `Newtonsoft.Json`-based formatters if the app:</span></span>

* <span data-ttu-id="82249-191">使用 `Newtonsoft.Json` 屬性。</span><span class="sxs-lookup"><span data-stu-id="82249-191">Uses `Newtonsoft.Json` attributes.</span></span> <span data-ttu-id="82249-192">例如，`[JsonProperty]` 或 `[JsonIgnore]`。</span><span class="sxs-lookup"><span data-stu-id="82249-192">For example, `[JsonProperty]` or `[JsonIgnore]`.</span></span>
* <span data-ttu-id="82249-193">自訂序列化設定。</span><span class="sxs-lookup"><span data-stu-id="82249-193">Customizes the serialization settings.</span></span>
* <span data-ttu-id="82249-194">依賴提供的功能 `Newtonsoft.Json` 。</span><span class="sxs-lookup"><span data-stu-id="82249-194">Relies on features that `Newtonsoft.Json` provides.</span></span>
* <span data-ttu-id="82249-195">設定 `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`。</span><span class="sxs-lookup"><span data-stu-id="82249-195">Configures `Microsoft.AspNetCore.Mvc.JsonResult.SerializerSettings`.</span></span> <span data-ttu-id="82249-196">在 ASP.NET Core 3.0 版之前，`JsonResult.SerializerSettings` 接受 `Newtonsoft.Json` 專屬的 `JsonSerializerSettings` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="82249-196">Prior to ASP.NET Core 3.0, `JsonResult.SerializerSettings` accepts an instance of `JsonSerializerSettings` that is specific to `Newtonsoft.Json`.</span></span>
* <span data-ttu-id="82249-197">產生 [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) 文件。</span><span class="sxs-lookup"><span data-stu-id="82249-197">Generates [OpenAPI](<xref:tutorials/web-api-help-pages-using-swagger>) documentation.</span></span>

<span data-ttu-id="82249-198">您可以使用來設定以為基礎之格式器的功能 `Newtonsoft.Json` `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings` ：</span><span class="sxs-lookup"><span data-stu-id="82249-198">Features for the `Newtonsoft.Json`-based formatters can be configured using `Microsoft.AspNetCore.Mvc.MvcNewtonsoftJsonOptions.SerializerSettings`:</span></span>

```csharp
services.AddControllers().AddNewtonsoftJson(options =>
{
    // Use the default property (Pascal) casing
    options.SerializerSettings.ContractResolver = new DefaultContractResolver();

    // Configure a custom converter
    options.SerializerSettings.Converters.Add(new MyCustomJsonConverter());
});
```

<span data-ttu-id="82249-199">以每個動作為基礎的輸出序列化選項，可以使用進行設定 `JsonResult` 。</span><span class="sxs-lookup"><span data-stu-id="82249-199">Output serialization options, on a per-action basis, can be configured using `JsonResult`.</span></span> <span data-ttu-id="82249-200">例如：</span><span class="sxs-lookup"><span data-stu-id="82249-200">For example:</span></span>

```csharp
public IActionResult Get()
{
    return Json(model, new JsonSerializerSettings
    {
        Formatting = Formatting.Indented,
    });
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

### <a name="add-xml-format-support"></a><span data-ttu-id="82249-201">新增 XML 格式支援</span><span class="sxs-lookup"><span data-stu-id="82249-201">Add XML format support</span></span>

<span data-ttu-id="82249-202">XML 格式需要[Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="82249-202">XML formatting requires the [Microsoft.AspNetCore.Mvc.Formatters.Xml](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Formatters.Xml/) NuGet package.</span></span>

<span data-ttu-id="82249-203">使用執行的 XML 格式器 <xref:System.Xml.Serialization.XmlSerializer> 是透過呼叫來設定 <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*> ：</span><span class="sxs-lookup"><span data-stu-id="82249-203">XML formatters implemented using <xref:System.Xml.Serialization.XmlSerializer> are configured by calling <xref:Microsoft.Extensions.DependencyInjection.MvcXmlMvcBuilderExtensions.AddXmlSerializerFormatters*>:</span></span>

[!code-csharp[](./formatting/sample/Startup.cs?name=snippet)]

<span data-ttu-id="82249-204">上述程式碼會使用來序列化結果 `XmlSerializer` 。</span><span class="sxs-lookup"><span data-stu-id="82249-204">The preceding code serializes results using `XmlSerializer`.</span></span>

<span data-ttu-id="82249-205">使用上述程式碼時，控制器方法應該根據要求的標頭傳回適當的格式 `Accept` 。</span><span class="sxs-lookup"><span data-stu-id="82249-205">When using the preceding code, controller methods should return the appropriate format based on the request's `Accept` header.</span></span>

::: moniker-end

### <a name="specify-a-format"></a><span data-ttu-id="82249-206">指定格式</span><span class="sxs-lookup"><span data-stu-id="82249-206">Specify a format</span></span>

<span data-ttu-id="82249-207">若要限制回應格式，請套用 [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) 篩選準則。</span><span class="sxs-lookup"><span data-stu-id="82249-207">To restrict the response formats, apply the [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter.</span></span> <span data-ttu-id="82249-208">就像大部分的[篩選器](xref:mvc/controllers/filters)一樣， `[Produces]` 可以在動作、控制器或全域範圍套用：</span><span class="sxs-lookup"><span data-stu-id="82249-208">Like most [Filters](xref:mvc/controllers/filters), `[Produces]` can be applied at the action, controller, or global scope:</span></span>

[!code-csharp[](./formatting/3.0sample/Controllers/WeatherForecastController.cs?name=snippet)]

<span data-ttu-id="82249-209">上述 [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) 篩選準則：</span><span class="sxs-lookup"><span data-stu-id="82249-209">The preceding [`[Produces]`](xref:Microsoft.AspNetCore.Mvc.ProducesAttribute) filter:</span></span>

* <span data-ttu-id="82249-210">強制控制器內的所有動作傳回 JSON 格式的回應。</span><span class="sxs-lookup"><span data-stu-id="82249-210">Forces all actions within the controller to return JSON-formatted responses.</span></span>
* <span data-ttu-id="82249-211">如果設定了其他格式器，而且用戶端指定了不同的格式，則會傳回 JSON。</span><span class="sxs-lookup"><span data-stu-id="82249-211">If other formatters are configured and the client specifies a different format, JSON is returned.</span></span>

<span data-ttu-id="82249-212">如需詳細資訊，請參閱[篩選](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="82249-212">For more information, see [Filters](xref:mvc/controllers/filters).</span></span>

### <a name="special-case-formatters"></a><span data-ttu-id="82249-213">特殊案例格式器</span><span class="sxs-lookup"><span data-stu-id="82249-213">Special case formatters</span></span>

<span data-ttu-id="82249-214">有些特殊案例是使用內建格式器所實作。</span><span class="sxs-lookup"><span data-stu-id="82249-214">Some special cases are implemented using built-in formatters.</span></span> <span data-ttu-id="82249-215">根據預設， `string` 如果透過標頭) 要求，傳回類型會格式化為*text/純* (*text/html* `Accept` 。</span><span class="sxs-lookup"><span data-stu-id="82249-215">By default, `string` return types are formatted as *text/plain* (*text/html* if requested via the `Accept` header).</span></span> <span data-ttu-id="82249-216">您可以藉由移除來刪除這個行為 <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter> 。</span><span class="sxs-lookup"><span data-stu-id="82249-216">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.StringOutputFormatter>.</span></span> <span data-ttu-id="82249-217">已移除方法中的格式器 `ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="82249-217">Formatters are removed in the `ConfigureServices` method.</span></span> <span data-ttu-id="82249-218">傳回時，具有模型物件傳回類型的動作會返回 `204 No Content` `null` 。</span><span class="sxs-lookup"><span data-stu-id="82249-218">Actions that have a model object return type return `204 No Content` when returning `null`.</span></span> <span data-ttu-id="82249-219">您可以藉由移除來刪除這個行為 <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter> 。</span><span class="sxs-lookup"><span data-stu-id="82249-219">This behavior can be deleted by removing the <xref:Microsoft.AspNetCore.Mvc.Formatters.HttpNoContentOutputFormatter>.</span></span> <span data-ttu-id="82249-220">下列程式碼會移除 `StringOutputFormatter` 和 `HttpNoContentOutputFormatter`。</span><span class="sxs-lookup"><span data-stu-id="82249-220">The following code removes the `StringOutputFormatter` and `HttpNoContentOutputFormatter`.</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](./formatting/3.0sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end
::: moniker range="< aspnetcore-3.0"
[!code-csharp[](./formatting/sample/StartupStringOutputFormatter.cs?name=snippet)]
::: moniker-end

<span data-ttu-id="82249-221">若沒有 `StringOutputFormatter` ，內建的 JSON 格式器格式會傳回 `string` 類型。</span><span class="sxs-lookup"><span data-stu-id="82249-221">Without the `StringOutputFormatter`, the built-in JSON formatter formats `string` return types.</span></span> <span data-ttu-id="82249-222">如果已移除內建 JSON 格式器，而且有 XML 格式器，則 XML 格式器格式會傳回 `string` 類型。</span><span class="sxs-lookup"><span data-stu-id="82249-222">If the built-in JSON formatter is removed and an XML formatter is available, the XML formatter formats `string` return types.</span></span> <span data-ttu-id="82249-223">否則，傳回類型會傳回 `string` `406 Not Acceptable` 。</span><span class="sxs-lookup"><span data-stu-id="82249-223">Otherwise, `string` return types return `406 Not Acceptable`.</span></span>

<span data-ttu-id="82249-224">如果沒有 `HttpNoContentOutputFormatter`，則會使用已設定的格式器來格式化 Null 物件。</span><span class="sxs-lookup"><span data-stu-id="82249-224">Without the `HttpNoContentOutputFormatter`, null objects are formatted using the configured formatter.</span></span> <span data-ttu-id="82249-225">例如：</span><span class="sxs-lookup"><span data-stu-id="82249-225">For example:</span></span>

* <span data-ttu-id="82249-226">JSON 格式器會傳回具有主體的回應 `null` 。</span><span class="sxs-lookup"><span data-stu-id="82249-226">The JSON formatter returns a response with a body of `null`.</span></span>
* <span data-ttu-id="82249-227">XML 格式器會傳回具有屬性集的空 XML 元素 `xsi:nil="true"` 。</span><span class="sxs-lookup"><span data-stu-id="82249-227">The XML formatter returns an empty XML element with the attribute `xsi:nil="true"` set.</span></span>

## <a name="response-format-url-mappings"></a><span data-ttu-id="82249-228">回應格式 URL 對應</span><span class="sxs-lookup"><span data-stu-id="82249-228">Response format URL mappings</span></span>

<span data-ttu-id="82249-229">用戶端可以要求特定格式做為 URL 的一部分，例如：</span><span class="sxs-lookup"><span data-stu-id="82249-229">Clients can request a particular format as part of the URL, for example:</span></span>

* <span data-ttu-id="82249-230">在查詢字串或部分路徑中。</span><span class="sxs-lookup"><span data-stu-id="82249-230">In the query string or part of the path.</span></span>
* <span data-ttu-id="82249-231">使用格式特定的副檔名，例如 .xml 或. json。</span><span class="sxs-lookup"><span data-stu-id="82249-231">By using a format-specific file extension such as .xml or .json.</span></span>

<span data-ttu-id="82249-232">應該在 API 所使用的路由中指定要求路徑的對應。</span><span class="sxs-lookup"><span data-stu-id="82249-232">The mapping from request path should be specified in the route the API is using.</span></span> <span data-ttu-id="82249-233">例如：</span><span class="sxs-lookup"><span data-stu-id="82249-233">For example:</span></span>

[!code-csharp[](./formatting/sample/Controllers/ProductsController.cs?name=snippet)]

<span data-ttu-id="82249-234">先前的路由可讓要求的格式指定為選用的副檔名。</span><span class="sxs-lookup"><span data-stu-id="82249-234">The preceding route allows the requested format to be specified as an optional file extension.</span></span> <span data-ttu-id="82249-235">[`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute)屬性會檢查中的格式值是否存在 `RouteData` ，並在建立回應時，將回應格式對應至適當的格式器。</span><span class="sxs-lookup"><span data-stu-id="82249-235">The [`[FormatFilter]`](xref:Microsoft.AspNetCore.Mvc.FormatFilterAttribute) attribute checks for the existence of the format value in the `RouteData` and maps the response format to the appropriate formatter when the response is created.</span></span>

|           <span data-ttu-id="82249-236">路由</span><span class="sxs-lookup"><span data-stu-id="82249-236">Route</span></span>        |             <span data-ttu-id="82249-237">格式器</span><span class="sxs-lookup"><span data-stu-id="82249-237">Formatter</span></span>              |
|------------------------|------------------------------------|
|   `/api/products/5`    |    <span data-ttu-id="82249-238">預設輸出格式器</span><span class="sxs-lookup"><span data-stu-id="82249-238">The default output formatter</span></span>    |
| `/api/products/5.json` | <span data-ttu-id="82249-239">JSON 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="82249-239">The JSON formatter (if configured)</span></span> |
| `/api/products/5.xml`  | <span data-ttu-id="82249-240">XML 格式器 (如果已設定)</span><span class="sxs-lookup"><span data-stu-id="82249-240">The XML formatter (if configured)</span></span>  |
