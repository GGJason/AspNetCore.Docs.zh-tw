---
title: ASP.NET Core 的 Blazor advanced 案例
author: guardrex
description: 瞭解中的先進案例 Blazor ，包括如何將手動 RenderTreeBuilder 邏輯併入應用程式。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/18/2020
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
uid: blazor/advanced-scenarios
ms.openlocfilehash: ce1786f644d1c0a70487f44ec3051de8189c5381
ms.sourcegitcommit: 65add17f74a29a647d812b04517e46cbc78258f9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/19/2020
ms.locfileid: "88625319"
---
# <a name="aspnet-core-no-locblazor-advanced-scenarios"></a><span data-ttu-id="2269c-103">ASP.NET Core 的 Blazor advanced 案例</span><span class="sxs-lookup"><span data-stu-id="2269c-103">ASP.NET Core Blazor advanced scenarios</span></span>

<span data-ttu-id="2269c-104">依 [Luke Latham](https://github.com/guardrex) 和 [Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2269c-104">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="no-locblazor-server-circuit-handler"></a><span data-ttu-id="2269c-105">Blazor Server 電路處理常式</span><span class="sxs-lookup"><span data-stu-id="2269c-105">Blazor Server circuit handler</span></span>

<span data-ttu-id="2269c-106">Blazor Server 允許程式碼定義迴圈 *處理常式*，以允許在使用者的線路狀態變更時執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="2269c-106">Blazor Server allows code to define a *circuit handler*, which allows running code on changes to the state of a user's circuit.</span></span> <span data-ttu-id="2269c-107">在 `CircuitHandler` 應用程式的服務容器中衍生類別並將其註冊，會實作為電路處理常式。</span><span class="sxs-lookup"><span data-stu-id="2269c-107">A circuit handler is implemented by deriving from `CircuitHandler` and registering the class in the app's service container.</span></span> <span data-ttu-id="2269c-108">下列的電路處理常式範例會追蹤開啟的 SignalR 連接：</span><span class="sxs-lookup"><span data-stu-id="2269c-108">The following example of a circuit handler tracks open SignalR connections:</span></span>

```csharp
using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Components.Server.Circuits;

public class TrackingCircuitHandler : CircuitHandler
{
    private HashSet<Circuit> circuits = new HashSet<Circuit>();

    public override Task OnConnectionUpAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        circuits.Add(circuit);

        return Task.CompletedTask;
    }

    public override Task OnConnectionDownAsync(Circuit circuit, 
        CancellationToken cancellationToken)
    {
        circuits.Remove(circuit);

        return Task.CompletedTask;
    }

    public int ConnectedCircuits => circuits.Count;
}
```

<span data-ttu-id="2269c-109">線路處理常式是使用 DI 註冊的。</span><span class="sxs-lookup"><span data-stu-id="2269c-109">Circuit handlers are registered using DI.</span></span> <span data-ttu-id="2269c-110">系統會為每個線路的實例建立範圍實例。</span><span class="sxs-lookup"><span data-stu-id="2269c-110">Scoped instances are created per instance of a circuit.</span></span> <span data-ttu-id="2269c-111">使用 `TrackingCircuitHandler` 上述範例中的，會建立單一服務，因為必須追蹤所有線路的狀態：</span><span class="sxs-lookup"><span data-stu-id="2269c-111">Using the `TrackingCircuitHandler` in the preceding example, a singleton service is created because the state of all circuits must be tracked:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...
    services.AddSingleton<CircuitHandler, TrackingCircuitHandler>();
}
```

<span data-ttu-id="2269c-112">如果自訂電路處理常式的方法擲回未處理的例外狀況，則例外狀況對線路而言是嚴重的 Blazor Server 。</span><span class="sxs-lookup"><span data-stu-id="2269c-112">If a custom circuit handler's methods throw an unhandled exception, the exception is fatal to the Blazor Server circuit.</span></span> <span data-ttu-id="2269c-113">若要容忍處理常式程式碼或呼叫方法中的例外狀況，請將程式碼包裝在一或多個語句中， [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) 並提供錯誤處理和記錄。</span><span class="sxs-lookup"><span data-stu-id="2269c-113">To tolerate exceptions in a handler's code or called methods, wrap the code in one or more [`try-catch`](/dotnet/csharp/language-reference/keywords/try-catch) statements with error handling and logging.</span></span>

<span data-ttu-id="2269c-114">當線路因為使用者已中斷連線而結束，且架構正在清除電路狀態時，架構會處置電路的 DI 範圍。</span><span class="sxs-lookup"><span data-stu-id="2269c-114">When a circuit ends because a user has disconnected and the framework is cleaning up the circuit state, the framework disposes of the circuit's DI scope.</span></span> <span data-ttu-id="2269c-115">處置範圍會處置任何執行的線路範圍 DI 服務 <xref:System.IDisposable?displayProperty=fullName> 。</span><span class="sxs-lookup"><span data-stu-id="2269c-115">Disposing the scope disposes any circuit-scoped DI services that implement <xref:System.IDisposable?displayProperty=fullName>.</span></span> <span data-ttu-id="2269c-116">如果任何 DI 服務在處置期間擲回未處理的例外狀況，則架構會記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2269c-116">If any DI service throws an unhandled exception during disposal, the framework logs the exception.</span></span>

## <a name="manual-rendertreebuilder-logic"></a><span data-ttu-id="2269c-117">手動 RenderTreeBuilder 邏輯</span><span class="sxs-lookup"><span data-stu-id="2269c-117">Manual RenderTreeBuilder logic</span></span>

<span data-ttu-id="2269c-118"><xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> 提供操作元件和元素的方法，包括以 c # 程式碼手動建立元件。</span><span class="sxs-lookup"><span data-stu-id="2269c-118"><xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> provides methods for manipulating components and elements, including building components manually in C# code.</span></span>

> [!NOTE]
> <span data-ttu-id="2269c-119">使用 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> 來建立元件是一個 advanced 案例。</span><span class="sxs-lookup"><span data-stu-id="2269c-119">Use of <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> to create components is an advanced scenario.</span></span> <span data-ttu-id="2269c-120">格式不正確的元件 (例如，未封閉的標記標記) 可能會導致未定義的行為。</span><span class="sxs-lookup"><span data-stu-id="2269c-120">A malformed component (for example, an unclosed markup tag) can result in undefined behavior.</span></span>

<span data-ttu-id="2269c-121">請考慮下列 `PetDetails` 元件，它可以手動內建在另一個元件中：</span><span class="sxs-lookup"><span data-stu-id="2269c-121">Consider the following `PetDetails` component, which can be manually built into another component:</span></span>

```razor
<h2>Pet Details Component</h2>

<p>@PetDetailsQuote</p>

@code
{
    [Parameter]
    public string PetDetailsQuote { get; set; }
}
```

<span data-ttu-id="2269c-122">在下列範例中，方法中的迴圈會 `CreateComponent` 產生三個 `PetDetails` 元件。</span><span class="sxs-lookup"><span data-stu-id="2269c-122">In the following example, the loop in the `CreateComponent` method generates three `PetDetails` components.</span></span> <span data-ttu-id="2269c-123">在 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> 具有序號的方法中，序號是源程式碼號。</span><span class="sxs-lookup"><span data-stu-id="2269c-123">In <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> methods with a sequence number, sequence numbers are source code line numbers.</span></span> <span data-ttu-id="2269c-124">Blazor差異演算法依賴不同行程式碼（而不是不同的呼叫調用）對應的序號。</span><span class="sxs-lookup"><span data-stu-id="2269c-124">The Blazor difference algorithm relies on the sequence numbers corresponding to distinct lines of code, not distinct call invocations.</span></span> <span data-ttu-id="2269c-125">使用方法建立元件時 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> ，請將序號的引數硬式編碼。</span><span class="sxs-lookup"><span data-stu-id="2269c-125">When creating a component with <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> methods, hardcode the arguments for sequence numbers.</span></span> <span data-ttu-id="2269c-126">**使用計算或計數器來產生序號可能會導致效能不佳。**</span><span class="sxs-lookup"><span data-stu-id="2269c-126">**Using a calculation or counter to generate the sequence number can lead to poor performance.**</span></span> <span data-ttu-id="2269c-127">如需詳細資訊，請參閱序號與程式 [程式碼號相關，而不是執行順序](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) 一節。</span><span class="sxs-lookup"><span data-stu-id="2269c-127">For more information, see the [Sequence numbers relate to code line numbers and not execution order](#sequence-numbers-relate-to-code-line-numbers-and-not-execution-order) section.</span></span>

<span data-ttu-id="2269c-128">`BuiltContent` 元件：</span><span class="sxs-lookup"><span data-stu-id="2269c-128">`BuiltContent` component:</span></span>

```razor
@page "/BuiltContent"

<h1>Build a component</h1>

@CustomRender

<button type="button" @onclick="RenderComponent">
    Create three Pet Details components
</button>

@code {
    private RenderFragment CustomRender { get; set; }
    
    private RenderFragment CreateComponent() => builder =>
    {
        for (var i = 0; i < 3; i++) 
        {
            builder.OpenComponent(0, typeof(PetDetails));
            builder.AddAttribute(1, "PetDetailsQuote", "Someone's best friend!");
            builder.CloseComponent();
        }
    };    
    
    private void RenderComponent()
    {
        CustomRender = CreateComponent();
    }
}
```

> [!WARNING]
> <span data-ttu-id="2269c-129">中的類型 <xref:Microsoft.AspNetCore.Components.RenderTree> 允許處理轉譯作業的 *結果* 。</span><span class="sxs-lookup"><span data-stu-id="2269c-129">The types in <xref:Microsoft.AspNetCore.Components.RenderTree> allow processing of the *results* of rendering operations.</span></span> <span data-ttu-id="2269c-130">這些是架構執行的內部詳細資料 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="2269c-130">These are internal details of the Blazor framework implementation.</span></span> <span data-ttu-id="2269c-131">這些類型應該視為不 *穩定* ，未來的版本可能會變更。</span><span class="sxs-lookup"><span data-stu-id="2269c-131">These types should be considered *unstable* and subject to change in future releases.</span></span>

### <a name="sequence-numbers-relate-to-code-line-numbers-and-not-execution-order"></a><span data-ttu-id="2269c-132">序號與程式程式碼號和非執行順序相關</span><span class="sxs-lookup"><span data-stu-id="2269c-132">Sequence numbers relate to code line numbers and not execution order</span></span>

<span data-ttu-id="2269c-133">Razor 元件檔 (`.razor`) 一律會進行編譯。</span><span class="sxs-lookup"><span data-stu-id="2269c-133">Razor component files (`.razor`) are always compiled.</span></span> <span data-ttu-id="2269c-134">相較于解讀程式碼，編譯是可能的優勢，因為編譯步驟可以用來插入可在執行時間改善應用程式效能的資訊。</span><span class="sxs-lookup"><span data-stu-id="2269c-134">Compilation is a potential advantage over interpreting code because the compile step can be used to inject information that improves app performance at runtime.</span></span>

<span data-ttu-id="2269c-135">這些改進的主要範例包含 *序號*。</span><span class="sxs-lookup"><span data-stu-id="2269c-135">A key example of these improvements involves *sequence numbers*.</span></span> <span data-ttu-id="2269c-136">序號會向執行時間指出輸出來自哪些不同和已排序的程式程式碼。</span><span class="sxs-lookup"><span data-stu-id="2269c-136">Sequence numbers indicate to the runtime which outputs came from which distinct and ordered lines of code.</span></span> <span data-ttu-id="2269c-137">執行時間會使用這項資訊，以線性時間產生有效率的樹狀結構差異，這遠比一般樹狀結構差異演算法一般可能更快。</span><span class="sxs-lookup"><span data-stu-id="2269c-137">The runtime uses this information to generate efficient tree diffs in linear time, which is far faster than is normally possible for a general tree diff algorithm.</span></span>

<span data-ttu-id="2269c-138">請考慮下列 Razor 元件 (`.razor`) 檔：</span><span class="sxs-lookup"><span data-stu-id="2269c-138">Consider the following Razor component (`.razor`) file:</span></span>

```razor
@if (someFlag)
{
    <text>First</text>
}

Second
```

<span data-ttu-id="2269c-139">上述程式碼會編譯成如下所示的內容：</span><span class="sxs-lookup"><span data-stu-id="2269c-139">The preceding code compiles to something like the following:</span></span>

```csharp
if (someFlag)
{
    builder.AddContent(0, "First");
}

builder.AddContent(1, "Second");
```

<span data-ttu-id="2269c-140">第一次執行程式碼時，產生器 `someFlag` 會 `true` 收到：</span><span class="sxs-lookup"><span data-stu-id="2269c-140">When the code executes for the first time, if `someFlag` is `true`, the builder receives:</span></span>

| <span data-ttu-id="2269c-141">順序</span><span class="sxs-lookup"><span data-stu-id="2269c-141">Sequence</span></span> | <span data-ttu-id="2269c-142">類型</span><span class="sxs-lookup"><span data-stu-id="2269c-142">Type</span></span>      | <span data-ttu-id="2269c-143">資料</span><span class="sxs-lookup"><span data-stu-id="2269c-143">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="2269c-144">0</span><span class="sxs-lookup"><span data-stu-id="2269c-144">0</span></span>        | <span data-ttu-id="2269c-145">Text node</span><span class="sxs-lookup"><span data-stu-id="2269c-145">Text node</span></span> | <span data-ttu-id="2269c-146">First</span><span class="sxs-lookup"><span data-stu-id="2269c-146">First</span></span>  |
| <span data-ttu-id="2269c-147">1</span><span class="sxs-lookup"><span data-stu-id="2269c-147">1</span></span>        | <span data-ttu-id="2269c-148">Text node</span><span class="sxs-lookup"><span data-stu-id="2269c-148">Text node</span></span> | <span data-ttu-id="2269c-149">Second</span><span class="sxs-lookup"><span data-stu-id="2269c-149">Second</span></span> |

<span data-ttu-id="2269c-150">想像這 `someFlag` 會變成 `false` ，然後再次轉譯標記。</span><span class="sxs-lookup"><span data-stu-id="2269c-150">Imagine that `someFlag` becomes `false`, and the markup is rendered again.</span></span> <span data-ttu-id="2269c-151">這次，產生器會收到：</span><span class="sxs-lookup"><span data-stu-id="2269c-151">This time, the builder receives:</span></span>

| <span data-ttu-id="2269c-152">順序</span><span class="sxs-lookup"><span data-stu-id="2269c-152">Sequence</span></span> | <span data-ttu-id="2269c-153">類型</span><span class="sxs-lookup"><span data-stu-id="2269c-153">Type</span></span>       | <span data-ttu-id="2269c-154">資料</span><span class="sxs-lookup"><span data-stu-id="2269c-154">Data</span></span>   |
| :------: | ---------- | :----: |
| <span data-ttu-id="2269c-155">1</span><span class="sxs-lookup"><span data-stu-id="2269c-155">1</span></span>        | <span data-ttu-id="2269c-156">Text node</span><span class="sxs-lookup"><span data-stu-id="2269c-156">Text node</span></span>  | <span data-ttu-id="2269c-157">Second</span><span class="sxs-lookup"><span data-stu-id="2269c-157">Second</span></span> |

<span data-ttu-id="2269c-158">當執行時間執行差異時，會看到順序中的專案 `0` 已移除，因此它會產生下列簡單的 *編輯腳本*：</span><span class="sxs-lookup"><span data-stu-id="2269c-158">When the runtime performs a diff, it sees that the item at sequence `0` was removed, so it generates the following trivial *edit script*:</span></span>

* <span data-ttu-id="2269c-159">移除第一個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="2269c-159">Remove the first text node.</span></span>

### <a name="the-problem-with-generating-sequence-numbers-programmatically"></a><span data-ttu-id="2269c-160">以程式設計方式產生序號的問題</span><span class="sxs-lookup"><span data-stu-id="2269c-160">The problem with generating sequence numbers programmatically</span></span>

<span data-ttu-id="2269c-161">想像一下，您應該改為撰寫下列轉譯樹產生器邏輯：</span><span class="sxs-lookup"><span data-stu-id="2269c-161">Imagine instead that you wrote the following render tree builder logic:</span></span>

```csharp
var seq = 0;

if (someFlag)
{
    builder.AddContent(seq++, "First");
}

builder.AddContent(seq++, "Second");
```

<span data-ttu-id="2269c-162">現在，第一個輸出是：</span><span class="sxs-lookup"><span data-stu-id="2269c-162">Now, the first output is:</span></span>

| <span data-ttu-id="2269c-163">順序</span><span class="sxs-lookup"><span data-stu-id="2269c-163">Sequence</span></span> | <span data-ttu-id="2269c-164">類型</span><span class="sxs-lookup"><span data-stu-id="2269c-164">Type</span></span>      | <span data-ttu-id="2269c-165">資料</span><span class="sxs-lookup"><span data-stu-id="2269c-165">Data</span></span>   |
| :------: | --------- | :----: |
| <span data-ttu-id="2269c-166">0</span><span class="sxs-lookup"><span data-stu-id="2269c-166">0</span></span>        | <span data-ttu-id="2269c-167">Text node</span><span class="sxs-lookup"><span data-stu-id="2269c-167">Text node</span></span> | <span data-ttu-id="2269c-168">First</span><span class="sxs-lookup"><span data-stu-id="2269c-168">First</span></span>  |
| <span data-ttu-id="2269c-169">1</span><span class="sxs-lookup"><span data-stu-id="2269c-169">1</span></span>        | <span data-ttu-id="2269c-170">Text node</span><span class="sxs-lookup"><span data-stu-id="2269c-170">Text node</span></span> | <span data-ttu-id="2269c-171">Second</span><span class="sxs-lookup"><span data-stu-id="2269c-171">Second</span></span> |

<span data-ttu-id="2269c-172">此結果與先前的案例相同，因此不會有負面問題。</span><span class="sxs-lookup"><span data-stu-id="2269c-172">This outcome is identical to the prior case, so no negative issues exist.</span></span> <span data-ttu-id="2269c-173">`someFlag` 是 `false` 在第二個轉譯上，輸出為：</span><span class="sxs-lookup"><span data-stu-id="2269c-173">`someFlag` is `false` on the second rendering, and the output is:</span></span>

| <span data-ttu-id="2269c-174">順序</span><span class="sxs-lookup"><span data-stu-id="2269c-174">Sequence</span></span> | <span data-ttu-id="2269c-175">類型</span><span class="sxs-lookup"><span data-stu-id="2269c-175">Type</span></span>      | <span data-ttu-id="2269c-176">資料</span><span class="sxs-lookup"><span data-stu-id="2269c-176">Data</span></span>   |
| :------: | --------- | ------ |
| <span data-ttu-id="2269c-177">0</span><span class="sxs-lookup"><span data-stu-id="2269c-177">0</span></span>        | <span data-ttu-id="2269c-178">Text node</span><span class="sxs-lookup"><span data-stu-id="2269c-178">Text node</span></span> | <span data-ttu-id="2269c-179">Second</span><span class="sxs-lookup"><span data-stu-id="2269c-179">Second</span></span> |

<span data-ttu-id="2269c-180">這次，diff 演算法會看到發生 *兩* 項變更，而演算法會產生下列編輯腳本：</span><span class="sxs-lookup"><span data-stu-id="2269c-180">This time, the diff algorithm sees that *two* changes have occurred, and the algorithm generates the following edit script:</span></span>

* <span data-ttu-id="2269c-181">將第一個文位元組點的值變更為 `Second` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-181">Change the value of the first text node to `Second`.</span></span>
* <span data-ttu-id="2269c-182">移除第二個文位元組點。</span><span class="sxs-lookup"><span data-stu-id="2269c-182">Remove the second text node.</span></span>

<span data-ttu-id="2269c-183">產生序號已遺失有關 `if/else` 原始程式碼中的分支和迴圈出現位置的所有實用資訊。</span><span class="sxs-lookup"><span data-stu-id="2269c-183">Generating the sequence numbers has lost all the useful information about where the `if/else` branches and loops were present in the original code.</span></span> <span data-ttu-id="2269c-184">這會導致差異 **兩次（只要** 前面）。</span><span class="sxs-lookup"><span data-stu-id="2269c-184">This results in a diff **twice as long** as before.</span></span>

<span data-ttu-id="2269c-185">這是一個簡單的範例。</span><span class="sxs-lookup"><span data-stu-id="2269c-185">This is a trivial example.</span></span> <span data-ttu-id="2269c-186">在更實際的案例中，如果是複雜且深層的嵌套結構，特別是迴圈，則效能成本通常較高。</span><span class="sxs-lookup"><span data-stu-id="2269c-186">In more realistic cases with complex and deeply nested structures, and especially with loops, the performance cost is usually higher.</span></span> <span data-ttu-id="2269c-187">差異演算法必須在轉譯樹狀結構中進行深度遞迴，而不是立即識別已插入或移除的迴圈區塊或分支。</span><span class="sxs-lookup"><span data-stu-id="2269c-187">Instead of immediately identifying which loop blocks or branches have been inserted or removed, the diff algorithm has to recurse deeply into the render trees.</span></span> <span data-ttu-id="2269c-188">這通常是因為 diff 演算法 misinformed 了舊的和新的結構彼此之間的關聯性，而導致必須建立較長的編輯腳本。</span><span class="sxs-lookup"><span data-stu-id="2269c-188">This usually results in having to build longer edit scripts because the diff algorithm is misinformed about how the old and new structures relate to each other.</span></span>

### <a name="guidance-and-conclusions"></a><span data-ttu-id="2269c-189">指引和結論</span><span class="sxs-lookup"><span data-stu-id="2269c-189">Guidance and conclusions</span></span>

* <span data-ttu-id="2269c-190">如果序號是動態產生的，應用程式效能就會受到影響。</span><span class="sxs-lookup"><span data-stu-id="2269c-190">App performance suffers if sequence numbers are generated dynamically.</span></span>
* <span data-ttu-id="2269c-191">架構無法在執行時間自動建立自己的序號，因為必要的資訊不存在，除非它是在編譯時期進行捕捉。</span><span class="sxs-lookup"><span data-stu-id="2269c-191">The framework can't create its own sequence numbers automatically at runtime because the necessary information doesn't exist unless it's captured at compile time.</span></span>
* <span data-ttu-id="2269c-192">請勿撰寫很長的手動執行 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> 邏輯區塊。</span><span class="sxs-lookup"><span data-stu-id="2269c-192">Don't write long blocks of manually-implemented <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> logic.</span></span> <span data-ttu-id="2269c-193">偏好使用檔案 `.razor` ，並讓編譯器處理序號。</span><span class="sxs-lookup"><span data-stu-id="2269c-193">Prefer `.razor` files and allow the compiler to deal with the sequence numbers.</span></span> <span data-ttu-id="2269c-194">如果您無法避免手動 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> 邏輯，請將很長的程式碼區塊分割成包裝在呼叫中的較小部分 <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.OpenRegion%2A> / <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.CloseRegion%2A> 。</span><span class="sxs-lookup"><span data-stu-id="2269c-194">If you're unable to avoid manual <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder> logic, split long blocks of code into smaller pieces wrapped in <xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.OpenRegion%2A>/<xref:Microsoft.AspNetCore.Components.Rendering.RenderTreeBuilder.CloseRegion%2A> calls.</span></span> <span data-ttu-id="2269c-195">每個區域都有自己的序號不同的空間，因此您可以從零 (或在每個區域內) 任何其他任意數目的任一數字重新開機。</span><span class="sxs-lookup"><span data-stu-id="2269c-195">Each region has its own separate space of sequence numbers, so you can restart from zero (or any other arbitrary number) inside each region.</span></span>
* <span data-ttu-id="2269c-196">如果序號是硬式編碼，則差異演算法只會要求序號增加值。</span><span class="sxs-lookup"><span data-stu-id="2269c-196">If sequence numbers are hardcoded, the diff algorithm only requires that sequence numbers increase in value.</span></span> <span data-ttu-id="2269c-197">初始值與間距無關。</span><span class="sxs-lookup"><span data-stu-id="2269c-197">The initial value and gaps are irrelevant.</span></span> <span data-ttu-id="2269c-198">其中一個合法的選項是使用程式程式碼號作為序號，或從零開始，並依一個或數百個 (或任何偏好的間隔) 來增加。</span><span class="sxs-lookup"><span data-stu-id="2269c-198">One legitimate option is to use the code line number as the sequence number, or start from zero and increase by ones or hundreds (or any preferred interval).</span></span> 
* <span data-ttu-id="2269c-199">Blazor 使用序號，其他樹狀結構比較的 UI 架構則不會使用它們。</span><span class="sxs-lookup"><span data-stu-id="2269c-199">Blazor uses sequence numbers, while other tree-diffing UI frameworks don't use them.</span></span> <span data-ttu-id="2269c-200">使用序號時，比較會更快，且 Blazor 具有可自動處理開發人員撰寫檔案之序號的編譯步驟 `.razor` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-200">Diffing is far faster when sequence numbers are used, and Blazor has the advantage of a compile step that deals with sequence numbers automatically for developers authoring `.razor` files.</span></span>

## <a name="perform-large-data-transfers-in-no-locblazor-server-apps"></a><span data-ttu-id="2269c-201">在應用程式中執行大量資料傳輸 Blazor Server</span><span class="sxs-lookup"><span data-stu-id="2269c-201">Perform large data transfers in Blazor Server apps</span></span>

<span data-ttu-id="2269c-202">在某些情況下，必須在 JavaScript 和之間傳輸大量資料 Blazor 。</span><span class="sxs-lookup"><span data-stu-id="2269c-202">In some scenarios, large amounts of data must be transferred between JavaScript and Blazor.</span></span> <span data-ttu-id="2269c-203">通常會在下列情況進行大量資料傳輸：</span><span class="sxs-lookup"><span data-stu-id="2269c-203">Typically, large data transfers occur when:</span></span>

* <span data-ttu-id="2269c-204">瀏覽器檔案系統 Api 可用來上傳或下載檔案。</span><span class="sxs-lookup"><span data-stu-id="2269c-204">Browser file system APIs are used to upload or download a file.</span></span>
* <span data-ttu-id="2269c-205">需要具有協力廠商程式庫的 Interop。</span><span class="sxs-lookup"><span data-stu-id="2269c-205">Interop with a third party library is required.</span></span>

<span data-ttu-id="2269c-206">在中 Blazor Server ，有一項限制是為了防止傳遞可能導致效能問題的單一大型訊息。</span><span class="sxs-lookup"><span data-stu-id="2269c-206">In Blazor Server, a limitation is in place to prevent passing single large messages that may result in performance issues.</span></span>

<span data-ttu-id="2269c-207">開發在 JavaScript 與之間傳送資料的程式碼時，請考慮下列指導方針 Blazor ：</span><span class="sxs-lookup"><span data-stu-id="2269c-207">Consider the following guidance when developing code that transfers data between JavaScript and Blazor:</span></span>

* <span data-ttu-id="2269c-208">將資料分割成較小的片段，並依序傳送資料區段，直到伺服器收到所有資料為止。</span><span class="sxs-lookup"><span data-stu-id="2269c-208">Slice the data into smaller pieces, and send the data segments sequentially until all of the data is received by the server.</span></span>
* <span data-ttu-id="2269c-209">請勿在 JavaScript 和 c # 程式碼中配置大型物件。</span><span class="sxs-lookup"><span data-stu-id="2269c-209">Don't allocate large objects in JavaScript and C# code.</span></span>
* <span data-ttu-id="2269c-210">傳送或接收資料時，請勿封鎖主要 UI 執行緒長時間。</span><span class="sxs-lookup"><span data-stu-id="2269c-210">Don't block the main UI thread for long periods when sending or receiving data.</span></span>
* <span data-ttu-id="2269c-211">釋放處理常式完成或取消時所耗用的記憶體。</span><span class="sxs-lookup"><span data-stu-id="2269c-211">Free any memory consumed when the process is completed or cancelled.</span></span>
* <span data-ttu-id="2269c-212">基於安全性考慮，強制執行下列其他需求：</span><span class="sxs-lookup"><span data-stu-id="2269c-212">Enforce the following additional requirements for security purposes:</span></span>
  * <span data-ttu-id="2269c-213">宣告可傳遞的檔案或資料大小上限。</span><span class="sxs-lookup"><span data-stu-id="2269c-213">Declare the maximum file or data size that can be passed.</span></span>
  * <span data-ttu-id="2269c-214">宣告從用戶端到伺服器的最小上傳速率。</span><span class="sxs-lookup"><span data-stu-id="2269c-214">Declare the minimum upload rate from the client to the server.</span></span>
* <span data-ttu-id="2269c-215">當伺服器收到資料之後，資料可以是：</span><span class="sxs-lookup"><span data-stu-id="2269c-215">After the data is received by the server, the data can be:</span></span>
  * <span data-ttu-id="2269c-216">暫時儲存在記憶體緩衝區中，直到收集所有區段為止。</span><span class="sxs-lookup"><span data-stu-id="2269c-216">Temporarily stored in a memory buffer until all of the segments are collected.</span></span>
  * <span data-ttu-id="2269c-217">立即使用。</span><span class="sxs-lookup"><span data-stu-id="2269c-217">Consumed immediately.</span></span> <span data-ttu-id="2269c-218">例如，資料可以立即儲存在資料庫中，或在收到每個區段時寫入磁片。</span><span class="sxs-lookup"><span data-stu-id="2269c-218">For example, the data can be stored immediately in a database or written to disk as each segment is received.</span></span>

<span data-ttu-id="2269c-219">下列檔案上載者類別會處理與用戶端的 JS interop。</span><span class="sxs-lookup"><span data-stu-id="2269c-219">The following file uploader class handles JS interop with the client.</span></span> <span data-ttu-id="2269c-220">上載者類別會使用 JS interop 來：</span><span class="sxs-lookup"><span data-stu-id="2269c-220">The uploader class uses JS interop to:</span></span>

* <span data-ttu-id="2269c-221">輪詢用戶端以傳送資料區段。</span><span class="sxs-lookup"><span data-stu-id="2269c-221">Poll the client to send a data segment.</span></span>
* <span data-ttu-id="2269c-222">如果輪詢超時，則中止交易。</span><span class="sxs-lookup"><span data-stu-id="2269c-222">Abort the transaction if polling times out.</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;
using System.Threading.Tasks;
using Microsoft.JSInterop;

public class FileUploader : IDisposable
{
    private readonly IJSRuntime jsRuntime;
    private readonly int segmentSize = 6144;
    private readonly int maxBase64SegmentSize = 8192;
    private readonly DotNetObjectReference<FileUploader> thisReference;
    private List<IMemoryOwner<byte>> uploadedSegments = 
        new List<IMemoryOwner<byte>>();

    public FileUploader(IJSRuntime jsRuntime)
    {
        this.jsRuntime = jsRuntime;
    }

    public async Task<Stream> ReceiveFile(string selector, int maxSize)
    {
        var fileSize = 
            await jsRuntime.InvokeAsync<int>("getFileSize", selector);

        if (fileSize > maxSize)
        {
            return null;
        }

        var numberOfSegments = Math.Floor(fileSize / (double)segmentSize) + 1;
        var lastSegmentBytes = 0;
        string base64EncodedSegment;

        for (var i = 0; i < numberOfSegments; i++)
        {
            try
            {
                base64EncodedSegment = 
                    await jsRuntime.InvokeAsync<string>(
                        "receiveSegment", i, selector);

                if (base64EncodedSegment.Length < maxBase64SegmentSize && 
                    i < numberOfSegments - 1)
                {
                    return null;
                }
            }
            catch
            {
                return null;
            }

          var current = MemoryPool<byte>.Shared.Rent(segmentSize);

          if (!Convert.TryFromBase64String(base64EncodedSegment, 
              current.Memory.Slice(0, segmentSize).Span, out lastSegmentBytes))
          {
              return null;
          }

          uploadedSegments.Add(current);
        }

        var segments = uploadedSegments;
        uploadedSegments = null;

        return new SegmentedStream(segments, segmentSize, lastSegmentBytes);
    }

    public void Dispose()
    {
        if (uploadedSegments != null)
        {
            foreach (var segment in uploadedSegments)
            {
                segment.Dispose();
            }
        }
    }
}
```

<span data-ttu-id="2269c-223">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="2269c-223">In the preceding example:</span></span>

* <span data-ttu-id="2269c-224">`maxBase64SegmentSize`設定為 `8192` ，其計算來源為 `maxBase64SegmentSize = segmentSize * 4 / 3` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-224">The `maxBase64SegmentSize` is set to `8192`, which is calculated from `maxBase64SegmentSize = segmentSize * 4 / 3`.</span></span>
* <span data-ttu-id="2269c-225">低層級的 .NET Core 記憶體管理 Api 是用來將伺服器上的記憶體區段儲存在中 `uploadedSegments` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-225">Low-level .NET Core memory management APIs are used to store the memory segments on the server in `uploadedSegments`.</span></span>
* <span data-ttu-id="2269c-226">`ReceiveFile`方法是用來處理透過 JS interop 的上傳：</span><span class="sxs-lookup"><span data-stu-id="2269c-226">A `ReceiveFile` method is used to handle the upload through JS interop:</span></span>
  * <span data-ttu-id="2269c-227">檔案大小是以位元組透過 JS interop 來決定 `jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-227">The file size is determined in bytes through JS interop with `jsRuntime.InvokeAsync<FileInfo>('getFileSize', selector)`.</span></span>
  * <span data-ttu-id="2269c-228">要接收的區段數目會計算並儲存在中 `numberOfSegments` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-228">The number of segments to receive are calculated and stored in `numberOfSegments`.</span></span>
  * <span data-ttu-id="2269c-229">您可以透過與的 JS interop，在迴圈中要求區段 `for` `jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)` 。</span><span class="sxs-lookup"><span data-stu-id="2269c-229">The segments are requested in a `for` loop through JS interop with `jsRuntime.InvokeAsync<string>('receiveSegment', i, selector)`.</span></span> <span data-ttu-id="2269c-230">在解碼之前，所有區段（但最後一個）必須是8192個位元組。</span><span class="sxs-lookup"><span data-stu-id="2269c-230">All segments but the last must be 8,192 bytes before decoding.</span></span> <span data-ttu-id="2269c-231">用戶端會強制以有效率的方式傳送資料。</span><span class="sxs-lookup"><span data-stu-id="2269c-231">The client is forced to send the data in an efficient manner.</span></span>
  * <span data-ttu-id="2269c-232">針對每個收到的區段，會在使用進行解碼之前執行檢查 <xref:System.Convert.TryFromBase64String%2A> 。</span><span class="sxs-lookup"><span data-stu-id="2269c-232">For each segment received, checks are performed before decoding with <xref:System.Convert.TryFromBase64String%2A>.</span></span>
  * <span data-ttu-id="2269c-233"><xref:System.IO.Stream> `SegmentedStream` 在上傳完成後，會以新的 () 傳回包含資料的資料流程。</span><span class="sxs-lookup"><span data-stu-id="2269c-233">A stream with the data is returned as a new <xref:System.IO.Stream> (`SegmentedStream`) after the upload is complete.</span></span>

<span data-ttu-id="2269c-234">分割的資料流程類別會將區段清單公開為 readonly 不可搜尋 <xref:System.IO.Stream> ：</span><span class="sxs-lookup"><span data-stu-id="2269c-234">The segmented stream class exposes the list of segments as a readonly non-seekable <xref:System.IO.Stream>:</span></span>

```csharp
using System;
using System.Buffers;
using System.Collections.Generic;
using System.IO;

public class SegmentedStream : Stream
{
    private readonly ReadOnlySequence<byte> sequence;
    private long currentPosition = 0;

    public SegmentedStream(IList<IMemoryOwner<byte>> segments, int segmentSize, 
        int lastSegmentSize)
    {
        if (segments.Count == 1)
        {
            sequence = new ReadOnlySequence<byte>(
                segments[0].Memory.Slice(0, lastSegmentSize));
            return;
        }

        var sequenceSegment = new BufferSegment<byte>(
            segments[0].Memory.Slice(0, segmentSize));
        var lastSegment = sequenceSegment;

        for (int i = 1; i < segments.Count; i++)
        {
            var isLastSegment = i + 1 == segments.Count;
            lastSegment = lastSegment.Append(segments[i].Memory.Slice(
                0, isLastSegment ? lastSegmentSize : segmentSize));
        }

        sequence = new ReadOnlySequence<byte>(
            sequenceSegment, 0, lastSegment, lastSegmentSize);
    }

    public override long Position
    {
        get => throw new NotImplementedException();
        set => throw new NotImplementedException();
    }

    public override int Read(byte[] buffer, int offset, int count)
    {
        var bytesToWrite = (int)(currentPosition + count < sequence.Length ? 
            count : sequence.Length - currentPosition);
        var data = sequence.Slice(currentPosition, bytesToWrite);
        data.CopyTo(buffer.AsSpan(offset, bytesToWrite));
        currentPosition += bytesToWrite;

        return bytesToWrite;
    }

    private class BufferSegment<T> : ReadOnlySequenceSegment<T>
    {
        public BufferSegment(ReadOnlyMemory<T> memory)
        {
            Memory = memory;
        }

        public BufferSegment<T> Append(ReadOnlyMemory<T> memory)
        {
            var segment = new BufferSegment<T>(memory)
            {
                RunningIndex = RunningIndex + Memory.Length
            };

            Next = segment;

            return segment;
        }
    }

    public override bool CanRead => true;

    public override bool CanSeek => false;

    public override bool CanWrite => false;

    public override long Length => throw new NotImplementedException();

    public override void Flush() => throw new NotImplementedException();

    public override long Seek(long offset, SeekOrigin origin) => 
        throw new NotImplementedException();

    public override void SetLength(long value) => 
        throw new NotImplementedException();

    public override void Write(byte[] buffer, int offset, int count) => 
        throw new NotImplementedException();
}
```

<span data-ttu-id="2269c-235">下列程式碼會執行 JavaScript 函數來接收資料：</span><span class="sxs-lookup"><span data-stu-id="2269c-235">The following code implements JavaScript functions to receive the data:</span></span>

```javascript
function getFileSize(selector) {
  const file = getFile(selector);
  return file.size;
}

async function receiveSegment(segmentNumber, selector) {
  const file = getFile(selector);
  var segments = getFileSegments(file);
  var index = segmentNumber * 6144;
  return await getNextChunk(file, index);
}

function getFile(selector) {
  const element = document.querySelector(selector);
  if (!element) {
    throw new Error('Invalid selector');
  }
  const files = element.files;
  if (!files || files.length === 0) {
    throw new Error(`Element ${elementId} doesn't contain any files.`);
  }
  const file = files[0];
  return file;
}

function getFileSegments(file) {
  const segments = Math.floor(size % 6144 === 0 ? size / 6144 : 1 + size / 6144);
  return segments;
}

async function getNextChunk(file, index) {
  const length = file.size - index <= 6144 ? file.size - index : 6144;
  const chunk = file.slice(index, index + length);
  index += length;
  const base64Chunk = await this.base64EncodeAsync(chunk);
  return { base64Chunk, index };
}

async function base64EncodeAsync(chunk) {
  const reader = new FileReader();
  const result = new Promise((resolve, reject) => {
    reader.addEventListener('load',
      () => {
        const base64Chunk = reader.result;
        const cleanChunk = 
          base64Chunk.replace('data:application/octet-stream;base64,', '');
        resolve(cleanChunk);
      },
      false);
    reader.addEventListener('error', reject);
  });
  reader.readAsDataURL(chunk);
  return result;
}
```
