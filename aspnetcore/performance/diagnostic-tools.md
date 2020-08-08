---
title: Performance Diagnostics 工具
author: mjrousos
description: 用來診斷 ASP.NET Core 應用程式效能問題的公用程式。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.date: 04/11/2019
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
uid: performance/diagnostic-tools
ms.openlocfilehash: 53c613b208c142e1323e593d0c57a42de3150621
ms.sourcegitcommit: 497be502426e9d90bb7d0401b1b9f74b6a384682
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/08/2020
ms.locfileid: "88018061"
---
# <a name="performance-diagnostic-tools"></a><span data-ttu-id="d4e68-103">效能診斷工具</span><span class="sxs-lookup"><span data-stu-id="d4e68-103">Performance Diagnostic Tools</span></span>

<span data-ttu-id="d4e68-104">由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="d4e68-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="d4e68-105">本文列出用來診斷 ASP.NET Core 中效能問題的工具。</span><span class="sxs-lookup"><span data-stu-id="d4e68-105">This article lists tools for diagnosing performance issues in ASP.NET Core.</span></span>

## <a name="visual-studio-diagnostic-tools"></a><span data-ttu-id="d4e68-106">Visual Studio 診斷工具</span><span class="sxs-lookup"><span data-stu-id="d4e68-106">Visual Studio Diagnostic Tools</span></span>

<span data-ttu-id="d4e68-107">內建于 Visual Studio 的程式碼[剖析和診斷工具](/visualstudio/profiling)，是開始調查效能問題的好地方。</span><span class="sxs-lookup"><span data-stu-id="d4e68-107">The [profiling and diagnostic tools](/visualstudio/profiling) built into Visual Studio are a good place to start investigating performance issues.</span></span> <span data-ttu-id="d4e68-108">這些工具在 Visual Studio 開發環境中功能強大且方便使用。</span><span class="sxs-lookup"><span data-stu-id="d4e68-108">These tools are powerful and convenient to use from the Visual Studio development environment.</span></span> <span data-ttu-id="d4e68-109">此工具可讓您分析 ASP.NET Core 應用程式中的 CPU 使用量、記憶體使用量和效能事件。</span><span class="sxs-lookup"><span data-stu-id="d4e68-109">The tooling allows analysis of CPU usage, memory usage, and performance events in ASP.NET Core apps.</span></span> <span data-ttu-id="d4e68-110">內建功能可讓您在開發期間輕鬆進行分析。</span><span class="sxs-lookup"><span data-stu-id="d4e68-110">Being built-in makes profiling easy at development time.</span></span>

<span data-ttu-id="d4e68-111">[Visual Studio 檔](/visualstudio/profiling/profiling-overview)中提供詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="d4e68-111">More information is available in [Visual Studio documentation](/visualstudio/profiling/profiling-overview).</span></span>

## <a name="application-insights"></a><span data-ttu-id="d4e68-112">Application Insights</span><span class="sxs-lookup"><span data-stu-id="d4e68-112">Application Insights</span></span>

<span data-ttu-id="d4e68-113">[Application Insights](/azure/application-insights/app-insights-overview)為您的應用程式提供深入的效能資料。</span><span class="sxs-lookup"><span data-stu-id="d4e68-113">[Application Insights](/azure/application-insights/app-insights-overview) provides in-depth performance data for your app.</span></span> <span data-ttu-id="d4e68-114">Application Insights 會自動收集回應率、失敗率、相依性回應時間等等的相關資料。</span><span class="sxs-lookup"><span data-stu-id="d4e68-114">Application Insights automatically collects data on response rates, failure rates, dependency response times, and more.</span></span> <span data-ttu-id="d4e68-115">Application Insights 支援記錄應用程式特定的自訂事件和計量。</span><span class="sxs-lookup"><span data-stu-id="d4e68-115">Application Insights supports logging custom events and metrics specific to your app.</span></span>

<span data-ttu-id="d4e68-116">Azure 應用程式 Insights 提供多種方式，讓您深入瞭解受監視的應用程式：</span><span class="sxs-lookup"><span data-stu-id="d4e68-116">Azure Application Insights provides multiple ways to give insights on monitored apps:</span></span>

- <span data-ttu-id="d4e68-117">[應用程式對應](/azure/application-insights/app-insights-app-map)-協助找出分散式應用程式所有元件的效能瓶頸或失敗熱點。</span><span class="sxs-lookup"><span data-stu-id="d4e68-117">[Application Map](/azure/application-insights/app-insights-app-map) – helps spot performance bottlenecks or failure hot-spots across all components of distributed apps.</span></span>
- <span data-ttu-id="d4e68-118">[Azure 計量瀏覽器](/azure/azure-monitor/platform/metrics-getting-started)是 Microsoft Azure 入口網站的元件，可讓您繪製圖表、以視覺化方式相互關聯趨勢，以及調查計量值的尖峰和下降。</span><span class="sxs-lookup"><span data-stu-id="d4e68-118">[Azure Metrics Explorer](/azure/azure-monitor/platform/metrics-getting-started) is a component of the Microsoft Azure portal that allows plotting charts, visually correlating trends, and investigating spikes and dips in metrics' values.</span></span>
- <span data-ttu-id="d4e68-119">[Application Insights 入口網站中的 [效能](/azure/application-insights/app-insights-tutorial-performance)] 分頁：</span><span class="sxs-lookup"><span data-stu-id="d4e68-119">[Performance blade in Application Insights portal](/azure/application-insights/app-insights-tutorial-performance):</span></span>

  - <span data-ttu-id="d4e68-120">顯示受監視應用程式中不同作業的效能詳細資料。</span><span class="sxs-lookup"><span data-stu-id="d4e68-120">Shows performance details for different operations in the monitored app.</span></span>
  - <span data-ttu-id="d4e68-121">允許深入探索單一作業，以檢查參與長時間的所有部分/相依性。</span><span class="sxs-lookup"><span data-stu-id="d4e68-121">Allows drilling into a single operation to check all parts/dependencies that contribute to a long duration.</span></span>
  - <span data-ttu-id="d4e68-122">您可以從這裡叫用 Profiler，視需要收集效能追蹤。</span><span class="sxs-lookup"><span data-stu-id="d4e68-122">Profiler can be invoked from here to collect performance traces on-demand.</span></span>

- <span data-ttu-id="d4e68-123">[Azure 應用程式 Insights Profiler](/azure/azure-monitor/app/profiler)可讓您進行 .net 應用程式的一般和隨選程式碼剖析。</span><span class="sxs-lookup"><span data-stu-id="d4e68-123">[Azure Application Insights Profiler](/azure/azure-monitor/app/profiler) allows regular and on-demand profiling of .NET apps.</span></span>  <span data-ttu-id="d4e68-124">Azure 入口網站會顯示使用呼叫堆疊和最忙碌路徑的已捕捉效能追蹤。</span><span class="sxs-lookup"><span data-stu-id="d4e68-124">Azure portal shows captured performance traces with call stacks and hot paths.</span></span> <span data-ttu-id="d4e68-125">您也可以下載追蹤檔案，以使用 PerfView 進行更深入的分析。</span><span class="sxs-lookup"><span data-stu-id="d4e68-125">The trace files can also be downloaded for deeper analysis using PerfView.</span></span>

<span data-ttu-id="d4e68-126">Application Insights 可以在各種環境中使用：</span><span class="sxs-lookup"><span data-stu-id="d4e68-126">Application Insights can be used in a variety environments:</span></span>

- <span data-ttu-id="d4e68-127">已優化，可在 Azure 中工作。</span><span class="sxs-lookup"><span data-stu-id="d4e68-127">Optimized to work in Azure.</span></span>
- <span data-ttu-id="d4e68-128">適用于生產、開發和預備環境。</span><span class="sxs-lookup"><span data-stu-id="d4e68-128">Works in production, development, and staging.</span></span>
- <span data-ttu-id="d4e68-129">在本機[Visual Studio](/azure/application-insights/app-insights-visual-studio)或其他裝載環境中運作。</span><span class="sxs-lookup"><span data-stu-id="d4e68-129">Works locally from [Visual Studio](/azure/application-insights/app-insights-visual-studio) or in other hosting environments.</span></span>

<span data-ttu-id="d4e68-130">如需詳細資訊，請參閱 [ASP.NET Core 的 Application Insights](/azure/application-insights/app-insights-asp-net-core)。</span><span class="sxs-lookup"><span data-stu-id="d4e68-130">For more information, see [Application Insights for ASP.NET Core](/azure/application-insights/app-insights-asp-net-core).</span></span>

## <a name="perfview"></a><span data-ttu-id="d4e68-131">PerfView</span><span class="sxs-lookup"><span data-stu-id="d4e68-131">PerfView</span></span>

<span data-ttu-id="d4e68-132">[PerfView](https://github.com/Microsoft/perfview)是由 .net 小組所建立的效能分析工具，專門用來診斷 .net 效能問題。</span><span class="sxs-lookup"><span data-stu-id="d4e68-132">[PerfView](https://github.com/Microsoft/perfview) is a performance analysis tool created by the .NET team specifically for diagnosing .NET performance issues.</span></span> <span data-ttu-id="d4e68-133">PerfView 可讓您分析 CPU 使用量、記憶體和 GC 行為、效能事件和時鐘時間。</span><span class="sxs-lookup"><span data-stu-id="d4e68-133">PerfView allows analysis of CPU usage, memory and GC behavior, performance events, and wall clock time.</span></span>

<span data-ttu-id="d4e68-134">您可以深入瞭解 PerfView，以及如何開始使用[PerfView 影片教學](https://channel9.msdn.com/Series/PerfView-Tutorial)課程，或閱讀工具或[GitHub 上](https://github.com/Microsoft/perfview)提供的使用者指南。</span><span class="sxs-lookup"><span data-stu-id="d4e68-134">You can learn more about PerfView and how to get started with [PerfView video tutorials](https://channel9.msdn.com/Series/PerfView-Tutorial) or by reading the user's guide available in the tool or [on GitHub](https://github.com/Microsoft/perfview).</span></span>

## <a name="windows-performance-toolkit"></a><span data-ttu-id="d4e68-135">Windows Performance Toolkit</span><span class="sxs-lookup"><span data-stu-id="d4e68-135">Windows Performance Toolkit</span></span>

<span data-ttu-id="d4e68-136">[Windows 效能工具](/windows-hardware/test/wpt/)組 (WPT) 由兩個元件所組成： Windows performance 記錄器 (WPR) 和 Windows performance ANALYZER (WPA) 。</span><span class="sxs-lookup"><span data-stu-id="d4e68-136">[Windows Performance Toolkit](/windows-hardware/test/wpt/) (WPT) consists of two components: Windows Performance Recorder (WPR) and Windows Performance Analyzer (WPA).</span></span> <span data-ttu-id="d4e68-137">這些工具會產生 Windows 作業系統和應用程式的深入效能設定檔。</span><span class="sxs-lookup"><span data-stu-id="d4e68-137">The tools produce in-depth performance profiles of Windows operating systems and apps.</span></span> <span data-ttu-id="d4e68-138">WPT 有更豐富的方式可將資料視覺化，但其資料收集的功能比 PerfView 的更強大。</span><span class="sxs-lookup"><span data-stu-id="d4e68-138">WPT has richer ways of visualizing data, but its data collecting is less powerful than PerfView's.</span></span>

## <a name="perfcollect"></a><span data-ttu-id="d4e68-139">PerfCollect</span><span class="sxs-lookup"><span data-stu-id="d4e68-139">PerfCollect</span></span>

<span data-ttu-id="d4e68-140">雖然 PerfView 是適用于 .NET 案例的實用效能分析工具，但它只會在 Windows 上執行，因此您無法使用它來收集來自在 Linux 環境中執行之 ASP.NET Core 應用程式的追蹤。</span><span class="sxs-lookup"><span data-stu-id="d4e68-140">While PerfView is a useful performance analysis tool for .NET scenarios, it only runs on Windows so you can't use it to collect traces from ASP.NET Core apps running in Linux environments.</span></span>

<span data-ttu-id="d4e68-141">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)是使用原生 Linux 分析[工具 (效能](https://perf.wiki.kernel.org/index.php/Main_Page)和[LTTng](https://lttng.org/)) 的 bash 腳本，可收集可由 PerfView 分析的 Linux 追蹤。</span><span class="sxs-lookup"><span data-stu-id="d4e68-141">[PerfCollect](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md) is a bash script that uses native Linux profiling tools ([Perf](https://perf.wiki.kernel.org/index.php/Main_Page) and [LTTng](https://lttng.org/)) to collect traces on Linux that can be analyzed by PerfView.</span></span> <span data-ttu-id="d4e68-142">當效能問題顯示在無法直接使用 PerfView 的 Linux 環境中時，PerfCollect 會很有用。</span><span class="sxs-lookup"><span data-stu-id="d4e68-142">PerfCollect is useful when performance problems show up in Linux environments where PerfView can't be used directly.</span></span> <span data-ttu-id="d4e68-143">相反地，PerfCollect 可以從 .NET Core 應用程式收集追蹤，然後使用 PerfView 在 Windows 電腦上進行分析。</span><span class="sxs-lookup"><span data-stu-id="d4e68-143">Instead, PerfCollect can collect traces from .NET Core apps that are then analyzed on a Windows computer using PerfView.</span></span>

<span data-ttu-id="d4e68-144">有關如何安裝和開始使用 PerfCollect 的詳細資訊，可[在 GitHub 上](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md)取得。</span><span class="sxs-lookup"><span data-stu-id="d4e68-144">More information about how to install and get started with PerfCollect is available [on GitHub](https://github.com/dotnet/coreclr/blob/master/Documentation/project-docs/linux-performance-tracing.md).</span></span>

## <a name="other-third-party-performance-tools"></a><span data-ttu-id="d4e68-145">其他協力廠商效能工具</span><span class="sxs-lookup"><span data-stu-id="d4e68-145">Other Third-party Performance Tools</span></span>

<span data-ttu-id="d4e68-146">以下列出一些在 .NET Core 應用程式的效能調查中很有用的協力廠商效能工具。</span><span class="sxs-lookup"><span data-stu-id="d4e68-146">The following lists some third-party performance tools that are useful in performance investigation of .NET Core applications.</span></span>

- [<span data-ttu-id="d4e68-147">Miniprofiler 撼動</span><span class="sxs-lookup"><span data-stu-id="d4e68-147">MiniProfiler</span></span>](https://miniprofiler.com/)
- <span data-ttu-id="d4e68-148">來自 JetBrains 的 dotTrace 和 dotMemory</span><span class="sxs-lookup"><span data-stu-id="d4e68-148">dotTrace and dotMemory from JetBrains</span></span>
- <span data-ttu-id="d4e68-149">來自 Intel 的 VTune</span><span class="sxs-lookup"><span data-stu-id="d4e68-149">VTune from Intel</span></span>
