---
title: .NET Core 中的 EventCounters
description: 本文将介绍什么是 EventCounters，如何实现它们，以及如何使用它们。
ms.date: 08/07/2020
ms.openlocfilehash: 8efa3134e83ba6fdc7563e97ef6422cb5f2099b6
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872777"
---
# <a name="eventcounters-in-net-core"></a><span data-ttu-id="8be35-103">.NET Core 中的 EventCounters</span><span class="sxs-lookup"><span data-stu-id="8be35-103">EventCounters in .NET Core</span></span>

<span data-ttu-id="8be35-104">**本文适用于：** ✔️ .NET Core 3.0 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="8be35-104">**This article applies to: ✔️** .NET Core 3.0 SDK and later versions</span></span>

<span data-ttu-id="8be35-105">EventCounters 是一些 .NET Core API，用于轻量级、跨平台、准实时性能指标收集。</span><span class="sxs-lookup"><span data-stu-id="8be35-105">EventCounters are .NET Core APIs used for lightweight, cross-platform, and near real-time performance metric collection.</span></span> <span data-ttu-id="8be35-106">EventCounters 添加为 Windows 上的 .NET 框架的“性能计数器”的跨平台替代。</span><span class="sxs-lookup"><span data-stu-id="8be35-106">EventCounters were added as a cross-platform alternative to the "performance counters" of the .NET Framework on Windows.</span></span> <span data-ttu-id="8be35-107">本文将介绍什么是 EventCounters，如何实现它们，以及如何使用它们。</span><span class="sxs-lookup"><span data-stu-id="8be35-107">In this article you'll learn what EventCounters are, how to implement them, and how to consume them.</span></span>

<span data-ttu-id="8be35-108">.NET Core 运行时和几个 .NET 库使用从 .NET Core 3.0 开始引入的 EventCounters 发布基本诊断信息。</span><span class="sxs-lookup"><span data-stu-id="8be35-108">The .NET Core runtime and a few .NET libraries publish basic diagnostics information using EventCounters starting in .NET Core 3.0.</span></span> <span data-ttu-id="8be35-109">除了 .NET 运行时提供的 EventCounters 外，你还可以选择实现自己的 EventCounters。</span><span class="sxs-lookup"><span data-stu-id="8be35-109">Apart from the EventCounters that are provided by the .NET runtime, you may choose to implement your own EventCounters.</span></span> <span data-ttu-id="8be35-110">可使用 EventCounters 跟踪各种指标。</span><span class="sxs-lookup"><span data-stu-id="8be35-110">EventCounters can be used to track various metrics.</span></span> <span data-ttu-id="8be35-111">在 [.NET 中的已知 EventCounters](available-counters.md) 中详细了解其信息</span><span class="sxs-lookup"><span data-stu-id="8be35-111">Learn more about them in the [well-known EventCounters in .NET](available-counters.md)</span></span>

<span data-ttu-id="8be35-112">EventCounters 作为 <xref:System.Diagnostics.Tracing.EventSource> 的一部分实时自动定期推送到侦听器工具。</span><span class="sxs-lookup"><span data-stu-id="8be35-112">EventCounters live as a part of an <xref:System.Diagnostics.Tracing.EventSource>, and are automatically pushed to listener tools on a regular basis.</span></span> <span data-ttu-id="8be35-113">与 <xref:System.Diagnostics.Tracing.EventSource> 上所有其他事件一样，可以通过 <xref:System.Diagnostics.Tracing.EventListener> 和 [EventPipe](./eventpipe.md) 在进程内和进程外使用它们。</span><span class="sxs-lookup"><span data-stu-id="8be35-113">Like all other events on an <xref:System.Diagnostics.Tracing.EventSource>, they can be consumed both in-proc and out-of-proc via <xref:System.Diagnostics.Tracing.EventListener> and [EventPipe](./eventpipe.md).</span></span> <span data-ttu-id="8be35-114">本文重点介绍 EventCounters 的跨平台功能，并特意排除 PerfView 和 ETW（Windows 事件跟踪）- 尽管两者都可用于 EventCounters。</span><span class="sxs-lookup"><span data-stu-id="8be35-114">This article focuses on the cross-platform capabilities of EventCounters, and intentionally excludes PerfView and ETW (Event Tracing for Windows) - although both can be used with EventCounters.</span></span>

![EventCounters 进程内和进程外示意图](media/event-counters.svg)

## <a name="eventcounter-api-overview"></a><span data-ttu-id="8be35-116">EventCounter API 概述</span><span class="sxs-lookup"><span data-stu-id="8be35-116">EventCounter API overview</span></span>

<span data-ttu-id="8be35-117">有两种主要类别的计数器。</span><span class="sxs-lookup"><span data-stu-id="8be35-117">There are two primary categories of counters.</span></span> <span data-ttu-id="8be35-118">某些计数器用于计算“比率”的值，例如异常总数、GC 总数和请求总数。</span><span class="sxs-lookup"><span data-stu-id="8be35-118">Some counters are for "rate" values, such as total number of exceptions, total number of GCs, and total number of requests.</span></span> <span data-ttu-id="8be35-119">其他计数器是“快照”值，例如堆使用情况、CPU 使用率和工作集大小。</span><span class="sxs-lookup"><span data-stu-id="8be35-119">Other counters are "snapshot" values, such as heap usage, CPU usage, and working set size.</span></span> <span data-ttu-id="8be35-120">在这两个类别的计数器中，各有两种类型的计数器，由获取值的方式区分。</span><span class="sxs-lookup"><span data-stu-id="8be35-120">Within each of these categories of counters, there are two types of counters that vary by how they get their value.</span></span> <span data-ttu-id="8be35-121">轮询计数器通过回调检索其值，非轮询计数器直接在计数器实例上设置其值。</span><span class="sxs-lookup"><span data-stu-id="8be35-121">Polling counters retrieve their value via a callback, and non-polling counters have their values directly set on the counter instance.</span></span>

<span data-ttu-id="8be35-122">计数器由以下实现表示：</span><span class="sxs-lookup"><span data-stu-id="8be35-122">The counters are represented by the following implementations:</span></span>

* <xref:System.Diagnostics.Tracing.EventCounter>
* <xref:System.Diagnostics.Tracing.IncrementingEventCounter>
* <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>
* <xref:System.Diagnostics.Tracing.PollingCounter>

<span data-ttu-id="8be35-123">事件侦听器指定测量间隔的时长。</span><span class="sxs-lookup"><span data-stu-id="8be35-123">An event listener specifies how long measurement intervals are.</span></span> <span data-ttu-id="8be35-124">在每个间隔结束时，每个计数器的值将传输到侦听器。</span><span class="sxs-lookup"><span data-stu-id="8be35-124">At the end of each interval a value is transmitted to the listener for each counter.</span></span> <span data-ttu-id="8be35-125">计数器的实现确定使用哪些 API 和计算来生成每个间隔的值。</span><span class="sxs-lookup"><span data-stu-id="8be35-125">The implementations of a counter determine what APIs and calculations are used to produce the value each interval.</span></span>

1. <span data-ttu-id="8be35-126"><xref:System.Diagnostics.Tracing.EventCounter> 记录一组值。</span><span class="sxs-lookup"><span data-stu-id="8be35-126">The <xref:System.Diagnostics.Tracing.EventCounter> records a set of values.</span></span> <span data-ttu-id="8be35-127"><xref:System.Diagnostics.Tracing.EventCounter.WriteMetric%2A?displayProperty=nameWithType> 方法将新值添加到集。</span><span class="sxs-lookup"><span data-stu-id="8be35-127">The <xref:System.Diagnostics.Tracing.EventCounter.WriteMetric%2A?displayProperty=nameWithType> method adds a new value to the set.</span></span> <span data-ttu-id="8be35-128">在每个间隔中，将计算集的统计摘要，如最小值、最大值和平均值。</span><span class="sxs-lookup"><span data-stu-id="8be35-128">With each interval, a statistical summary for the set is computed, such as the min, max, and mean.</span></span> <span data-ttu-id="8be35-129">[dotnet-counters](dotnet-counters.md) 工具将始终显示平均值。</span><span class="sxs-lookup"><span data-stu-id="8be35-129">The [dotnet-counters](dotnet-counters.md) tool will always display the mean value.</span></span> <span data-ttu-id="8be35-130"><xref:System.Diagnostics.Tracing.EventCounter> 用于描述一组离散的操作。</span><span class="sxs-lookup"><span data-stu-id="8be35-130">The <xref:System.Diagnostics.Tracing.EventCounter> is useful to describe a discrete set of operations.</span></span> <span data-ttu-id="8be35-131">常见用法包括监视最近 IO 操作的平均大小（以字节为单位）或一组金融交易的平均货币价值。</span><span class="sxs-lookup"><span data-stu-id="8be35-131">Common usage may include monitoring the average size in bytes of recent IO operations, or the average monetary value of a set of financial transactions.</span></span>

1. <span data-ttu-id="8be35-132"><xref:System.Diagnostics.Tracing.IncrementingEventCounter> 记录每个时间间隔的运行总计。</span><span class="sxs-lookup"><span data-stu-id="8be35-132">The <xref:System.Diagnostics.Tracing.IncrementingEventCounter> records a running total for each time interval.</span></span> <span data-ttu-id="8be35-133"><xref:System.Diagnostics.Tracing.IncrementingEventCounter.Increment%2A?displayProperty=nameWithType> 方法添加到总计。</span><span class="sxs-lookup"><span data-stu-id="8be35-133">The <xref:System.Diagnostics.Tracing.IncrementingEventCounter.Increment%2A?displayProperty=nameWithType> method adds to the total.</span></span> <span data-ttu-id="8be35-134">例如，如果在一段间隔内调用三次 `Increment()`，其值分别为 `1`、`2` 和 `5`，则此间隔的计数器值将报告运行总计 `8`。</span><span class="sxs-lookup"><span data-stu-id="8be35-134">For example, if `Increment()` is called three times during one interval with values `1`, `2`, and `5`, then the running total of `8` will be reported as the counter value for this interval.</span></span> <span data-ttu-id="8be35-135">[dotnet-counters](dotnet-counters.md) 工具将比率显示为记录的总计/时间。</span><span class="sxs-lookup"><span data-stu-id="8be35-135">The [dotnet-counters](dotnet-counters.md) tool will display the rate as the recorded total / time.</span></span> <span data-ttu-id="8be35-136"><xref:System.Diagnostics.Tracing.IncrementingEventCounter> 用于测量操作发生的频率，例如每秒处理的请求数。</span><span class="sxs-lookup"><span data-stu-id="8be35-136">The <xref:System.Diagnostics.Tracing.IncrementingEventCounter> is useful to measure how frequently an action is occurring, such as the number of requests processed per second.</span></span>

1. <span data-ttu-id="8be35-137"><xref:System.Diagnostics.Tracing.PollingCounter> 使用回调来确定报告的值。</span><span class="sxs-lookup"><span data-stu-id="8be35-137">The <xref:System.Diagnostics.Tracing.PollingCounter> uses a callback to determine the value that is reported.</span></span> <span data-ttu-id="8be35-138">在每个时间间隔中，调用用户提供的回调函数，然后返回值用作计数器值。</span><span class="sxs-lookup"><span data-stu-id="8be35-138">With each time interval, the user provided callback function is invoked and the return value is used as the counter value.</span></span> <span data-ttu-id="8be35-139">可以使用 <xref:System.Diagnostics.Tracing.PollingCounter> 从外部源查询指标，例如获取磁盘上的当前可用字节。</span><span class="sxs-lookup"><span data-stu-id="8be35-139">A <xref:System.Diagnostics.Tracing.PollingCounter> can be used to query a metric from an external source, for example getting the current free bytes on a disk.</span></span> <span data-ttu-id="8be35-140">它还可用于报告应用程序可按需计算的自定义统计信息。</span><span class="sxs-lookup"><span data-stu-id="8be35-140">It can also be used to report custom statistics that can be computed on demand by an application.</span></span> <span data-ttu-id="8be35-141">示例包括报告最近请求延迟的第 95 个百分位，或缓存的当前命中或错过比率。</span><span class="sxs-lookup"><span data-stu-id="8be35-141">Examples include reporting the 95th percentile of recent request latencies, or the current hit or miss ratio of a cache.</span></span>

1. <span data-ttu-id="8be35-142"><xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 使用回调来确定报告的增量值。</span><span class="sxs-lookup"><span data-stu-id="8be35-142">The <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> uses a callback to determine the reported increment value.</span></span> <span data-ttu-id="8be35-143">对于每个时间间隔，调用回调，然后当前调用与最后一个调用之间的差值是报告的值。</span><span class="sxs-lookup"><span data-stu-id="8be35-143">With each time interval, the callback is invoked, and then the difference between the current invocation, and the last invocation is the reported value.</span></span> <span data-ttu-id="8be35-144">[dotnet-counters](dotnet-counters.md) 工具始终将比率显示为报告的值/时间。</span><span class="sxs-lookup"><span data-stu-id="8be35-144">The [dotnet-counters](dotnet-counters.md) tool will always display the difference as a rate, the reported value / time.</span></span> <span data-ttu-id="8be35-145">如果不可在每次发生事件时调用 API，但可以查询事件总数，则此计数器很有用。</span><span class="sxs-lookup"><span data-stu-id="8be35-145">This counter is useful when it isn't feasible to call an API on each occurrence, but it's possible to query the total number of occurrences.</span></span> <span data-ttu-id="8be35-146">例如，可以报告每秒写入文件的字节数，即使每次写入字节时没有通知。</span><span class="sxs-lookup"><span data-stu-id="8be35-146">For example, you could report the number of bytes written to a file per second, even without a notification each time a byte is written.</span></span>

## <a name="implement-an-eventsource"></a><span data-ttu-id="8be35-147">实现 EventSource</span><span class="sxs-lookup"><span data-stu-id="8be35-147">Implement an EventSource</span></span>

<span data-ttu-id="8be35-148">下面的代码实现作为命名 `"Sample.EventCounter.Minimal"` 提供程序公开的示例 <xref:System.Diagnostics.Tracing.EventSource>。</span><span class="sxs-lookup"><span data-stu-id="8be35-148">The following code implements a sample <xref:System.Diagnostics.Tracing.EventSource> exposed as the named `"Sample.EventCounter.Minimal"` provider.</span></span> <span data-ttu-id="8be35-149">此源包含表示请求处理时间的 <xref:System.Diagnostics.Tracing.EventCounter>。</span><span class="sxs-lookup"><span data-stu-id="8be35-149">This source contains an <xref:System.Diagnostics.Tracing.EventCounter> representing request processing time.</span></span> <span data-ttu-id="8be35-150">此类计数器具有名称（即其在源中的唯一 ID）和显示名称，这两个名称都可由侦听器工具（如 [dotnet-counter](dotnet-counters.md)）使用。</span><span class="sxs-lookup"><span data-stu-id="8be35-150">Such a counter has a name (that is, its unique ID in the source) and a display name, both used by listener tools such as [dotnet-counter](dotnet-counters.md).</span></span>

:::code language="csharp" source="snippets/EventCounters/MinimalEventCounterSource.cs":::

<span data-ttu-id="8be35-151">可以使用 `dotnet-counters ps` 来显示可监视的 .NET 进程的列表：</span><span class="sxs-lookup"><span data-stu-id="8be35-151">You use `dotnet-counters ps` to display a list of .NET processes that can be monitored:</span></span>

```console
dotnet-counters ps
   1398652 dotnet     C:\Program Files\dotnet\dotnet.exe
   1399072 dotnet     C:\Program Files\dotnet\dotnet.exe
   1399112 dotnet     C:\Program Files\dotnet\dotnet.exe
   1401880 dotnet     C:\Program Files\dotnet\dotnet.exe
   1400180 sample-counters C:\sample-counters\bin\Debug\netcoreapp3.1\sample-counters.exe
```

<span data-ttu-id="8be35-152">将 <xref:System.Diagnostics.Tracing.EventSource> 名称传递到 `--counters` 选项，以开始监视计数器：</span><span class="sxs-lookup"><span data-stu-id="8be35-152">Pass the <xref:System.Diagnostics.Tracing.EventSource> name to the `--counters` option to start monitoring your counter:</span></span>

```console
dotnet-counters monitor --process-id 1400180 --counters Sample.EventCounter.Minimal
```

<span data-ttu-id="8be35-153">以下示例显示监视器输出：</span><span class="sxs-lookup"><span data-stu-id="8be35-153">The following example shows monitor output:</span></span>

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[Samples-EventCounterDemos-Minimal]
    Request Processing Time (ms)                            0.445
```

<span data-ttu-id="8be35-154">按 <kbd>q</kbd> 停止监视命令。</span><span class="sxs-lookup"><span data-stu-id="8be35-154">Press <kbd>q</kbd> to stop the monitoring command.</span></span>

#### <a name="conditional-counters"></a><span data-ttu-id="8be35-155">条件计数器</span><span class="sxs-lookup"><span data-stu-id="8be35-155">Conditional counters</span></span>

<span data-ttu-id="8be35-156">实现 <xref:System.Diagnostics.Tracing.EventSource> 时，通过 <xref:System.Diagnostics.Tracing.EventCommandEventArgs.Command> 值 `EventCommand.Enable` 调用 <xref:System.Diagnostics.Tracing.EventSource.OnEventCommand%2A?displayProperty=nameWithType> 方法时，可以有条件地实例化包含计数器。</span><span class="sxs-lookup"><span data-stu-id="8be35-156">When implementing an <xref:System.Diagnostics.Tracing.EventSource>, the containing counters can be conditionally instantiated when the <xref:System.Diagnostics.Tracing.EventSource.OnEventCommand%2A?displayProperty=nameWithType> method is called with a <xref:System.Diagnostics.Tracing.EventCommandEventArgs.Command> value of `EventCommand.Enable`.</span></span> <span data-ttu-id="8be35-157">要仅在计数器实例为 `null` 时将其安全地实例化，请使用 [null 合并赋值运算符](../../csharp/language-reference/operators/null-coalescing-operator.md)。</span><span class="sxs-lookup"><span data-stu-id="8be35-157">To safely instantiate a counter instance only if it is `null`, use the [null-coalescing assignment operator](../../csharp/language-reference/operators/null-coalescing-operator.md).</span></span> <span data-ttu-id="8be35-158">此外，自定义方法可以计算 <xref:System.Diagnostics.DiagnosticSource.IsEnabled%2A> 方法，以确定是否启用了当前事件源。</span><span class="sxs-lookup"><span data-stu-id="8be35-158">Additionally, custom methods can evaluate the <xref:System.Diagnostics.DiagnosticSource.IsEnabled%2A> method to determine whether or not the current event source is enabled.</span></span>

:::code language="csharp" source="snippets/EventCounters/ConditionalEventCounterSource.cs":::

> [!TIP]
> <span data-ttu-id="8be35-159">条件计数器是有条件地实例化的计数器，即微优化。</span><span class="sxs-lookup"><span data-stu-id="8be35-159">Conditional counters are counters that are conditionally instantiated, a micro-optimization.</span></span> <span data-ttu-id="8be35-160">对于通常不使用计数器的场景，运行时采用此模式来节省不到一毫秒的时间。</span><span class="sxs-lookup"><span data-stu-id="8be35-160">The runtime adopts this pattern for scenarios where counters are normally not used, to save a fraction of a millisecond.</span></span>

### <a name="net-core-runtime-example-counters"></a><span data-ttu-id="8be35-161">.NET Core 运行时示例计数器</span><span class="sxs-lookup"><span data-stu-id="8be35-161">.NET Core runtime example counters</span></span>

<span data-ttu-id="8be35-162">在 .NET Core 运行时中有许多很好的示例实现。</span><span class="sxs-lookup"><span data-stu-id="8be35-162">There are many great example implementations in the .NET Core runtime.</span></span> <span data-ttu-id="8be35-163">下面是跟踪应用程序工作集大小的计数器的运行时实现。</span><span class="sxs-lookup"><span data-stu-id="8be35-163">Here is the runtime implementation for the counter that tracks the working set size of the application.</span></span>

```csharp
var workingSetCounter = new PollingCounter(
    "working-set",
    this,
    () => (double)(Environment.WorkingSet / 1_000_000))
{
    DisplayName = "Working Set",
    DisplayUnits = "MB"
};
```

<span data-ttu-id="8be35-164"><xref:System.Diagnostics.Tracing.PollingCounter> 报告映射到应用的进程（工作集）的当前物理内存量，因为它在一个时刻捕获一个指标。</span><span class="sxs-lookup"><span data-stu-id="8be35-164">The <xref:System.Diagnostics.Tracing.PollingCounter> reports the current amount of physical memory mapped to the process (working set) of the app, since it captures a metric at a moment in time.</span></span> <span data-ttu-id="8be35-165">轮询值的回调是提供的 lambda 表达式，这只是对 <xref:System.Environment.WorkingSet?displayProperty=fullName> API 的调用。</span><span class="sxs-lookup"><span data-stu-id="8be35-165">The callback for polling a value is the provided lambda expression, which is just a call to the <xref:System.Environment.WorkingSet?displayProperty=fullName> API.</span></span> <span data-ttu-id="8be35-166"><xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayName> 和 <xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayUnits> 是可选属性，可以设置它们，帮助计数器的使用者方更清楚地显示值。</span><span class="sxs-lookup"><span data-stu-id="8be35-166"><xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayName> and <xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayUnits> are optional properties that can be set to help the consumer side of the counter to display the value more clearly.</span></span> <span data-ttu-id="8be35-167">例如，[dotnet-counters](dotnet-counters.md) 使用这些属性来显示计数器名称的更具有显示友好性的版本。</span><span class="sxs-lookup"><span data-stu-id="8be35-167">For example, [dotnet-counters](dotnet-counters.md) uses these properties to display the more display-friendly version of the counter names.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8be35-168">`DisplayName` 属性未本地化。</span><span class="sxs-lookup"><span data-stu-id="8be35-168">The `DisplayName` properties are not localized.</span></span>

<span data-ttu-id="8be35-169">对于 <xref:System.Diagnostics.Tracing.PollingCounter> 和 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>，无需执行任何其他操作。</span><span class="sxs-lookup"><span data-stu-id="8be35-169">For the <xref:System.Diagnostics.Tracing.PollingCounter>, and the <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>, nothing else needs to be done.</span></span> <span data-ttu-id="8be35-170">它们本身都按使用者请求的时间间隔轮询值。</span><span class="sxs-lookup"><span data-stu-id="8be35-170">They both poll the values themselves at an interval requested by the consumer.</span></span>

<span data-ttu-id="8be35-171">下面是使用 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 实现的运行时计数器的示例。</span><span class="sxs-lookup"><span data-stu-id="8be35-171">Here is an example of a runtime counter implemented using <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>.</span></span>

```csharp
var monitorContentionCounter = new IncrementingPollingCounter(
    "monitor-lock-contention-count",
    this,
    () => Monitor.LockContentionCount
)
{
    DisplayName = "Monitor Lock Contention Count",
    DisplayRateTimeScale = TimeSpan.FromSeconds(1)
};
```

<span data-ttu-id="8be35-172"><xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 使用 <xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> API 报告锁争用数总计的增量。</span><span class="sxs-lookup"><span data-stu-id="8be35-172">The <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> uses the <xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> API to report the increment of the total lock contention count.</span></span> <span data-ttu-id="8be35-173"><xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> 属性可选，但使用它时，它可以提供有关计数器最佳显示时间间隔的提示。</span><span class="sxs-lookup"><span data-stu-id="8be35-173">The <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> property is optional, but when used it can provide a hint for what time interval the counter is best displayed at.</span></span> <span data-ttu-id="8be35-174">例如，锁争用计数最好显示为“每秒计数”，因此其 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> 设置为一秒。</span><span class="sxs-lookup"><span data-stu-id="8be35-174">For example, the lock contention count is best displayed as _count per second_, so its <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> is set to one second.</span></span> <span data-ttu-id="8be35-175">可为不同类型的比率计数器调整显示比率。</span><span class="sxs-lookup"><span data-stu-id="8be35-175">The display rate can be adjusted for different types of rate counters.</span></span>

> [!NOTE]
> <span data-ttu-id="8be35-176"><xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> 不由 [dotnet-counters](dotnet-counters.md) 使用，不需要事件侦听器即可使用它。</span><span class="sxs-lookup"><span data-stu-id="8be35-176">The <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> is _not_ used by [dotnet-counters](dotnet-counters.md), and event listeners are not required to use it.</span></span>

<span data-ttu-id="8be35-177">在 [.NET 运行时](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/Diagnostics/Tracing/RuntimeEventSource.cs)存储库中，有更多的计数器实现可用作参考。</span><span class="sxs-lookup"><span data-stu-id="8be35-177">There are more counter implementations to use as a reference in the [.NET runtime](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/Diagnostics/Tracing/RuntimeEventSource.cs) repo.</span></span>

## <a name="concurrency"></a><span data-ttu-id="8be35-178">并发</span><span class="sxs-lookup"><span data-stu-id="8be35-178">Concurrency</span></span>

> [!TIP]
> <span data-ttu-id="8be35-179">EventCounters API 不能保证线程安全性。</span><span class="sxs-lookup"><span data-stu-id="8be35-179">The EventCounters API does not guarantee thread safety.</span></span> <span data-ttu-id="8be35-180">当传递到 <xref:System.Diagnostics.Tracing.PollingCounter> 或 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 实例的委托由多个线程调用时，你有责任保证委托的线程安全性。</span><span class="sxs-lookup"><span data-stu-id="8be35-180">When the delegates passed to <xref:System.Diagnostics.Tracing.PollingCounter> or <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> instances are called by multiple threads, it's your responsibility to guarantee the delegates' thread-safety.</span></span>

<span data-ttu-id="8be35-181">例如，请考虑使用以下 <xref:System.Diagnostics.Tracing.EventSource> 来跟踪请求。</span><span class="sxs-lookup"><span data-stu-id="8be35-181">For example, consider the following <xref:System.Diagnostics.Tracing.EventSource> to keep track of requests.</span></span>

:::code language="csharp" source="snippets/EventCounters/RequestEventSource.cs":::

<span data-ttu-id="8be35-182">可以从请求处理程序调用 `AddRequest()` 方法，并且 `RequestRateCounter` 按计数器使用者指定的间隔轮询值。</span><span class="sxs-lookup"><span data-stu-id="8be35-182">The `AddRequest()` method can be called from a request handler, and the `RequestRateCounter` polls the value at the interval specified by the consumer of the counter.</span></span> <span data-ttu-id="8be35-183">但是，`AddRequest()` 方法可以同时由多个线程调用，将争用条件置于 `_requestCount`。</span><span class="sxs-lookup"><span data-stu-id="8be35-183">However, the `AddRequest()` method can be called by multiple threads at once, putting a race condition on `_requestCount`.</span></span> <span data-ttu-id="8be35-184">增加 `_requestCount` 的线程安全替代方法是使用 <xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="8be35-184">A thread-safe alternative way to increment the `_requestCount` is to use <xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType>.</span></span>

```csharp
public void AddRequest() => Interlocked.Increment(ref _requestCount);
```

<span data-ttu-id="8be35-185">若要防止破坏（在 32 位体系结构上）对 `long` 字段 `_requestCount` 的读取，请使用 <xref:System.Threading.Interlocked.Read%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="8be35-185">To prevent torn reads (on 32-bit architectures) of the `long`-field `_requestCount` use <xref:System.Threading.Interlocked.Read%2A?displayProperty=nameWithType>.</span></span>

```csharp
_requestRateCounter = new IncrementingPollingCounter("request-rate", this, () => Interlocked.Read(ref _requestCount))
{
    DisplayName = "Request Rate",
    DisplayRateTimeScale = TimeSpan.FromSeconds(1)
};
```

## <a name="consume-eventcounters"></a><span data-ttu-id="8be35-186">使用 EventCounters</span><span class="sxs-lookup"><span data-stu-id="8be35-186">Consume EventCounters</span></span>

<span data-ttu-id="8be35-187">使用 EventCounters 主要方式有两种：进程内或进程外。</span><span class="sxs-lookup"><span data-stu-id="8be35-187">There are two primary ways of consuming EventCounters, either in-proc, or out-of-proc.</span></span> <span data-ttu-id="8be35-188">EventCounters 的使用可以分为三层不同的使用技术。</span><span class="sxs-lookup"><span data-stu-id="8be35-188">The consumption of EventCounters can be distinguished into three layers of various consuming technologies.</span></span>

- <span data-ttu-id="8be35-189">通过 ETW 或 EventPipe 在原始流中传输事件：</span><span class="sxs-lookup"><span data-stu-id="8be35-189">Transporting events in a raw stream via ETW or EventPipe:</span></span>
  - <span data-ttu-id="8be35-190">ETW API 附带 Windows OS，EventPipe 可作为 [.NET API](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/diagnostics-client-library.md#1-attaching-to-a-process-and-dumping-out-all-the-runtime-gc-events-in-real-time-to-the-console) 或诊断 [IPC 协议](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/ipc-protocol.md)进行访问。</span><span class="sxs-lookup"><span data-stu-id="8be35-190">ETW APIs come with the Windows OS, and EventPipe is accessible as a [.NET API](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/diagnostics-client-library.md#1-attaching-to-a-process-and-dumping-out-all-the-runtime-gc-events-in-real-time-to-the-console), or the diagnostic [IPC protocol](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/ipc-protocol.md).</span></span>
- <span data-ttu-id="8be35-191">将二进制事件流解码为事件：</span><span class="sxs-lookup"><span data-stu-id="8be35-191">Decoding the binary event stream into events:</span></span>
  - <span data-ttu-id="8be35-192">[TraceEvent 库](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent)可处理 ETW 和 EventPipe 流格式。</span><span class="sxs-lookup"><span data-stu-id="8be35-192">The [TraceEvent library](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent) handles both ETW and EventPipe stream formats.</span></span>
- <span data-ttu-id="8be35-193">命令行和 GUI 工具：</span><span class="sxs-lookup"><span data-stu-id="8be35-193">Command-line and GUI tools:</span></span>
  - <span data-ttu-id="8be35-194">PerfView（ETW 或 EventPipe）、dotnet-counters（仅 EventPipe）和 dotnet-monitor（仅 EventPipe）等工具。</span><span class="sxs-lookup"><span data-stu-id="8be35-194">Tools like PerfView (ETW or EventPipe), dotnet-counters (EventPipe only), and dotnet-monitor (EventPipe only).</span></span>

### <a name="consume-out-of-proc"></a><span data-ttu-id="8be35-195">进程外使用</span><span class="sxs-lookup"><span data-stu-id="8be35-195">Consume out-of-proc</span></span>

<span data-ttu-id="8be35-196">进程外使用 EventCounters 是一种非常常见的方法。</span><span class="sxs-lookup"><span data-stu-id="8be35-196">Consuming EventCounters out-of-proc is a very common approach.</span></span> <span data-ttu-id="8be35-197">你可以使用 [dotnet-counters](dotnet-counters.md) 通过 EventPipe 以跨平台方式使用它们。</span><span class="sxs-lookup"><span data-stu-id="8be35-197">You can use [dotnet-counters](dotnet-counters.md) to consume them in a cross-platform manner via an EventPipe.</span></span> <span data-ttu-id="8be35-198">`dotnet-counters` 工具是一个跨平台 dotnet CLI 全局工具，可用于监视计数器值。</span><span class="sxs-lookup"><span data-stu-id="8be35-198">The `dotnet-counters` tool is a cross-platform dotnet CLI global tool that can be used to monitor the counter values.</span></span> <span data-ttu-id="8be35-199">要了解如何使用 `dotnet-counters` 监视计数器，请参阅 [dotnet-counters](dotnet-counters.md) 或浏览[使用 EventCounters 衡量性能](event-counter-perf.md)教程。</span><span class="sxs-lookup"><span data-stu-id="8be35-199">To find out how to use `dotnet-counters` to monitor your counters, see [dotnet-counters](dotnet-counters.md), or work through the [Measure performance using EventCounters](event-counter-perf.md) tutorial.</span></span>

#### <a name="dotnet-trace"></a><span data-ttu-id="8be35-200">dotnet-trace</span><span class="sxs-lookup"><span data-stu-id="8be35-200">dotnet-trace</span></span>

<span data-ttu-id="8be35-201">`dotnet-trace` 工具可用于通过 EventPipe 使用计数器数据。</span><span class="sxs-lookup"><span data-stu-id="8be35-201">The `dotnet-trace` tool can be used to consume the counter data through an EventPipe.</span></span> <span data-ttu-id="8be35-202">下面是使用 `dotnet-trace` 收集计数器数据的一个示例。</span><span class="sxs-lookup"><span data-stu-id="8be35-202">Here is an example using `dotnet-trace` to collect counter data.</span></span>

```console
dotnet-trace collect --process-id <pid> Sample.EventCounter.Minimal:0:0:EventCounterIntervalSec=1
```

<span data-ttu-id="8be35-203">有关如何随着时间的推移收集计数器值的详细信息，请参阅 [dotnet-trace](dotnet-trace.md#use-dotnet-trace-to-collect-counter-values-over-time) 文档。</span><span class="sxs-lookup"><span data-stu-id="8be35-203">For more information on how to collect counter values over time, see the [dotnet-trace](dotnet-trace.md#use-dotnet-trace-to-collect-counter-values-over-time) documentation.</span></span>

#### <a name="azure-application-insights"></a><span data-ttu-id="8be35-204">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="8be35-204">Azure Application Insights</span></span>

<span data-ttu-id="8be35-205">EventCounters 可由 Azure Monitor 使用，特别是 Azure Application Insights。</span><span class="sxs-lookup"><span data-stu-id="8be35-205">EventCounters can be consumed by Azure Monitor, specifically Azure Application Insights.</span></span> <span data-ttu-id="8be35-206">可以添加和删除计数器，并且可以自由指定自定义计数器或已知计数器。</span><span class="sxs-lookup"><span data-stu-id="8be35-206">Counters can be added and removed, and you're free to specify custom counters, or well-known counters.</span></span> <span data-ttu-id="8be35-207">有关详细信息，请参阅[自定义要收集的计数器](/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected)。</span><span class="sxs-lookup"><span data-stu-id="8be35-207">For more information, see [Customizing counters to be collected](/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).</span></span>

#### <a name="dotnet-monitor"></a><span data-ttu-id="8be35-208">dotnet-monitor</span><span class="sxs-lookup"><span data-stu-id="8be35-208">dotnet-monitor</span></span>

<span data-ttu-id="8be35-209">`dotnet-monitor` 工具是一个实验性工具，通过它可以更轻松地访问 .NET 进程中的诊断信息。</span><span class="sxs-lookup"><span data-stu-id="8be35-209">The `dotnet-monitor` tool is an experimental tool that makes it easier to get access to diagnostics information in a .NET process.</span></span> <span data-ttu-id="8be35-210">该工具用作所有诊断工具的超集。</span><span class="sxs-lookup"><span data-stu-id="8be35-210">The tool serves as a superset of all diagnostics tools.</span></span> <span data-ttu-id="8be35-211">除跟踪外，它还可以监视指标、收集内存转储和收集 GC 转储。</span><span class="sxs-lookup"><span data-stu-id="8be35-211">In addition to traces, it can monitor metrics, collect memory dumps, and collect GC dumps.</span></span> <span data-ttu-id="8be35-212">它以 CLI 工具和 docker 映像的形式发布。</span><span class="sxs-lookup"><span data-stu-id="8be35-212">It's distributed as both a CLI tool and a docker image.</span></span> <span data-ttu-id="8be35-213">它公开了 REST API，以及通过 REST 调用发生的诊断项目集合。</span><span class="sxs-lookup"><span data-stu-id="8be35-213">It exposes a REST API, and the collection of diagnostic artifacts occurs through REST calls.</span></span>

<span data-ttu-id="8be35-214">有关详细信息，请参阅[实验性工具 dotnet-monitor 简介](https://devblogs.microsoft.com/dotnet/introducing-dotnet-monitor)。</span><span class="sxs-lookup"><span data-stu-id="8be35-214">For more information, see [Introducing dotnet-monitor, an experimental tool](https://devblogs.microsoft.com/dotnet/introducing-dotnet-monitor).</span></span>

### <a name="consume-in-proc"></a><span data-ttu-id="8be35-215">进程内使用</span><span class="sxs-lookup"><span data-stu-id="8be35-215">Consume in-proc</span></span>

<span data-ttu-id="8be35-216">可以通过 <xref:System.Diagnostics.Tracing.EventListener> API 使用计数器值。</span><span class="sxs-lookup"><span data-stu-id="8be35-216">You can consume the counter values via the <xref:System.Diagnostics.Tracing.EventListener> API.</span></span> <span data-ttu-id="8be35-217"><xref:System.Diagnostics.Tracing.EventListener> 是使用由应用程序中 <xref:System.Diagnostics.Tracing.EventSource> 的所有实例编写的任何事件的一种进程内方法。</span><span class="sxs-lookup"><span data-stu-id="8be35-217">An <xref:System.Diagnostics.Tracing.EventListener> is an in-proc way of consuming any events written by all instances of an <xref:System.Diagnostics.Tracing.EventSource> in your application.</span></span> <span data-ttu-id="8be35-218">有关如何使用 `EventListener` API 的详细信息，请参阅 <xref:System.Diagnostics.Tracing.EventListener>。</span><span class="sxs-lookup"><span data-stu-id="8be35-218">For more information on how to use the `EventListener` API, see <xref:System.Diagnostics.Tracing.EventListener>.</span></span>

<span data-ttu-id="8be35-219">首先，需要启用生成计数器值的 <xref:System.Diagnostics.Tracing.EventSource>。</span><span class="sxs-lookup"><span data-stu-id="8be35-219">First, the <xref:System.Diagnostics.Tracing.EventSource> that produces the counter value needs to be enabled.</span></span> <span data-ttu-id="8be35-220">替代 <xref:System.Diagnostics.Tracing.EventListener.OnEventSourceCreated%2A?displayProperty=nameWithType> 方法以在创建 <xref:System.Diagnostics.Tracing.EventSource> 时获取通知，如果对于 EventCounters 这是正确的 <xref:System.Diagnostics.Tracing.EventSource>，则可在其上调用 <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="8be35-220">Override the <xref:System.Diagnostics.Tracing.EventListener.OnEventSourceCreated%2A?displayProperty=nameWithType> method to get a notification when an <xref:System.Diagnostics.Tracing.EventSource> is created, and if this is the correct <xref:System.Diagnostics.Tracing.EventSource> with your EventCounters, then you can call <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A?displayProperty=nameWithType> on it.</span></span> <span data-ttu-id="8be35-221">下面是示例替代：</span><span class="sxs-lookup"><span data-stu-id="8be35-221">Here is an example override:</span></span>

:::code language="csharp" source="snippets/EventCounters/SimpleEventListener.cs" range="11-22":::

#### <a name="sample-code"></a><span data-ttu-id="8be35-222">代码示例</span><span class="sxs-lookup"><span data-stu-id="8be35-222">Sample code</span></span>

<span data-ttu-id="8be35-223">下面是一个示例 <xref:System.Diagnostics.Tracing.EventListener> 类，它打印出 .NET 运行时的 <xref:System.Diagnostics.Tracing.EventSource> 的所有计数器名称和值，用于每秒发布其内部计数器 (`System.Runtime`)。</span><span class="sxs-lookup"><span data-stu-id="8be35-223">Here is a sample <xref:System.Diagnostics.Tracing.EventListener> class that prints out all the counter names and values from the .NET runtime's <xref:System.Diagnostics.Tracing.EventSource>, for publishing its internal counters (`System.Runtime`) every second.</span></span>

:::code language="csharp" source="snippets/EventCounters/SimpleEventListener.cs":::

<span data-ttu-id="8be35-224">如下所示，调用 <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A> 时必须确保在 `filterPayload` 参数中设置 `"EventCounterIntervalSec"` 参数。</span><span class="sxs-lookup"><span data-stu-id="8be35-224">As shown above, you _must_ make sure the `"EventCounterIntervalSec"` argument is set in the `filterPayload` argument when calling <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A>.</span></span> <span data-ttu-id="8be35-225">否则，计数器将无法清空值，因为它不知道应清空哪个时间间隔。</span><span class="sxs-lookup"><span data-stu-id="8be35-225">Otherwise the counters will not be able to flush out values since it doesn't know at which interval it should be getting flushed out.</span></span>

## <a name="see-also"></a><span data-ttu-id="8be35-226">另请参阅</span><span class="sxs-lookup"><span data-stu-id="8be35-226">See also</span></span>

- [<span data-ttu-id="8be35-227">dotnet-counters</span><span class="sxs-lookup"><span data-stu-id="8be35-227">dotnet-counters</span></span>](dotnet-counters.md)
- [<span data-ttu-id="8be35-228">dotnet-trace</span><span class="sxs-lookup"><span data-stu-id="8be35-228">dotnet-trace</span></span>](dotnet-trace.md)
- <xref:System.Diagnostics.Tracing.EventCounter>
- <xref:System.Diagnostics.Tracing.EventListener>
- <xref:System.Diagnostics.Tracing.EventSource>
