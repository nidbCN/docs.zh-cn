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
# <a name="eventcounters-in-net-core"></a>.NET Core 中的 EventCounters

**本文适用于：** ✔️ .NET Core 3.0 SDK 及更高版本

EventCounters 是一些 .NET Core API，用于轻量级、跨平台、准实时性能指标收集。 EventCounters 添加为 Windows 上的 .NET 框架的“性能计数器”的跨平台替代。 本文将介绍什么是 EventCounters，如何实现它们，以及如何使用它们。

.NET Core 运行时和几个 .NET 库使用从 .NET Core 3.0 开始引入的 EventCounters 发布基本诊断信息。 除了 .NET 运行时提供的 EventCounters 外，你还可以选择实现自己的 EventCounters。 可使用 EventCounters 跟踪各种指标。 在 [.NET 中的已知 EventCounters](available-counters.md) 中详细了解其信息

EventCounters 作为 <xref:System.Diagnostics.Tracing.EventSource> 的一部分实时自动定期推送到侦听器工具。 与 <xref:System.Diagnostics.Tracing.EventSource> 上所有其他事件一样，可以通过 <xref:System.Diagnostics.Tracing.EventListener> 和 [EventPipe](./eventpipe.md) 在进程内和进程外使用它们。 本文重点介绍 EventCounters 的跨平台功能，并特意排除 PerfView 和 ETW（Windows 事件跟踪）- 尽管两者都可用于 EventCounters。

![EventCounters 进程内和进程外示意图](media/event-counters.svg)

## <a name="eventcounter-api-overview"></a>EventCounter API 概述

有两种主要类别的计数器。 某些计数器用于计算“比率”的值，例如异常总数、GC 总数和请求总数。 其他计数器是“快照”值，例如堆使用情况、CPU 使用率和工作集大小。 在这两个类别的计数器中，各有两种类型的计数器，由获取值的方式区分。 轮询计数器通过回调检索其值，非轮询计数器直接在计数器实例上设置其值。

计数器由以下实现表示：

* <xref:System.Diagnostics.Tracing.EventCounter>
* <xref:System.Diagnostics.Tracing.IncrementingEventCounter>
* <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>
* <xref:System.Diagnostics.Tracing.PollingCounter>

事件侦听器指定测量间隔的时长。 在每个间隔结束时，每个计数器的值将传输到侦听器。 计数器的实现确定使用哪些 API 和计算来生成每个间隔的值。

1. <xref:System.Diagnostics.Tracing.EventCounter> 记录一组值。 <xref:System.Diagnostics.Tracing.EventCounter.WriteMetric%2A?displayProperty=nameWithType> 方法将新值添加到集。 在每个间隔中，将计算集的统计摘要，如最小值、最大值和平均值。 [dotnet-counters](dotnet-counters.md) 工具将始终显示平均值。 <xref:System.Diagnostics.Tracing.EventCounter> 用于描述一组离散的操作。 常见用法包括监视最近 IO 操作的平均大小（以字节为单位）或一组金融交易的平均货币价值。

1. <xref:System.Diagnostics.Tracing.IncrementingEventCounter> 记录每个时间间隔的运行总计。 <xref:System.Diagnostics.Tracing.IncrementingEventCounter.Increment%2A?displayProperty=nameWithType> 方法添加到总计。 例如，如果在一段间隔内调用三次 `Increment()`，其值分别为 `1`、`2` 和 `5`，则此间隔的计数器值将报告运行总计 `8`。 [dotnet-counters](dotnet-counters.md) 工具将比率显示为记录的总计/时间。 <xref:System.Diagnostics.Tracing.IncrementingEventCounter> 用于测量操作发生的频率，例如每秒处理的请求数。

1. <xref:System.Diagnostics.Tracing.PollingCounter> 使用回调来确定报告的值。 在每个时间间隔中，调用用户提供的回调函数，然后返回值用作计数器值。 可以使用 <xref:System.Diagnostics.Tracing.PollingCounter> 从外部源查询指标，例如获取磁盘上的当前可用字节。 它还可用于报告应用程序可按需计算的自定义统计信息。 示例包括报告最近请求延迟的第 95 个百分位，或缓存的当前命中或错过比率。

1. <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 使用回调来确定报告的增量值。 对于每个时间间隔，调用回调，然后当前调用与最后一个调用之间的差值是报告的值。 [dotnet-counters](dotnet-counters.md) 工具始终将比率显示为报告的值/时间。 如果不可在每次发生事件时调用 API，但可以查询事件总数，则此计数器很有用。 例如，可以报告每秒写入文件的字节数，即使每次写入字节时没有通知。

## <a name="implement-an-eventsource"></a>实现 EventSource

下面的代码实现作为命名 `"Sample.EventCounter.Minimal"` 提供程序公开的示例 <xref:System.Diagnostics.Tracing.EventSource>。 此源包含表示请求处理时间的 <xref:System.Diagnostics.Tracing.EventCounter>。 此类计数器具有名称（即其在源中的唯一 ID）和显示名称，这两个名称都可由侦听器工具（如 [dotnet-counter](dotnet-counters.md)）使用。

:::code language="csharp" source="snippets/EventCounters/MinimalEventCounterSource.cs":::

可以使用 `dotnet-counters ps` 来显示可监视的 .NET 进程的列表：

```console
dotnet-counters ps
   1398652 dotnet     C:\Program Files\dotnet\dotnet.exe
   1399072 dotnet     C:\Program Files\dotnet\dotnet.exe
   1399112 dotnet     C:\Program Files\dotnet\dotnet.exe
   1401880 dotnet     C:\Program Files\dotnet\dotnet.exe
   1400180 sample-counters C:\sample-counters\bin\Debug\netcoreapp3.1\sample-counters.exe
```

将 <xref:System.Diagnostics.Tracing.EventSource> 名称传递到 `--counters` 选项，以开始监视计数器：

```console
dotnet-counters monitor --process-id 1400180 --counters Sample.EventCounter.Minimal
```

以下示例显示监视器输出：

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[Samples-EventCounterDemos-Minimal]
    Request Processing Time (ms)                            0.445
```

按 <kbd>q</kbd> 停止监视命令。

#### <a name="conditional-counters"></a>条件计数器

实现 <xref:System.Diagnostics.Tracing.EventSource> 时，通过 <xref:System.Diagnostics.Tracing.EventCommandEventArgs.Command> 值 `EventCommand.Enable` 调用 <xref:System.Diagnostics.Tracing.EventSource.OnEventCommand%2A?displayProperty=nameWithType> 方法时，可以有条件地实例化包含计数器。 要仅在计数器实例为 `null` 时将其安全地实例化，请使用 [null 合并赋值运算符](../../csharp/language-reference/operators/null-coalescing-operator.md)。 此外，自定义方法可以计算 <xref:System.Diagnostics.DiagnosticSource.IsEnabled%2A> 方法，以确定是否启用了当前事件源。

:::code language="csharp" source="snippets/EventCounters/ConditionalEventCounterSource.cs":::

> [!TIP]
> 条件计数器是有条件地实例化的计数器，即微优化。 对于通常不使用计数器的场景，运行时采用此模式来节省不到一毫秒的时间。

### <a name="net-core-runtime-example-counters"></a>.NET Core 运行时示例计数器

在 .NET Core 运行时中有许多很好的示例实现。 下面是跟踪应用程序工作集大小的计数器的运行时实现。

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

<xref:System.Diagnostics.Tracing.PollingCounter> 报告映射到应用的进程（工作集）的当前物理内存量，因为它在一个时刻捕获一个指标。 轮询值的回调是提供的 lambda 表达式，这只是对 <xref:System.Environment.WorkingSet?displayProperty=fullName> API 的调用。 <xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayName> 和 <xref:System.Diagnostics.Tracing.DiagnosticCounter.DisplayUnits> 是可选属性，可以设置它们，帮助计数器的使用者方更清楚地显示值。 例如，[dotnet-counters](dotnet-counters.md) 使用这些属性来显示计数器名称的更具有显示友好性的版本。

> [!IMPORTANT]
> `DisplayName` 属性未本地化。

对于 <xref:System.Diagnostics.Tracing.PollingCounter> 和 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter>，无需执行任何其他操作。 它们本身都按使用者请求的时间间隔轮询值。

下面是使用 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 实现的运行时计数器的示例。

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

<xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 使用 <xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> API 报告锁争用数总计的增量。 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> 属性可选，但使用它时，它可以提供有关计数器最佳显示时间间隔的提示。 例如，锁争用计数最好显示为“每秒计数”，因此其 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> 设置为一秒。 可为不同类型的比率计数器调整显示比率。

> [!NOTE]
> <xref:System.Diagnostics.Tracing.IncrementingPollingCounter.DisplayRateTimeScale> 不由 [dotnet-counters](dotnet-counters.md) 使用，不需要事件侦听器即可使用它。

在 [.NET 运行时](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/Diagnostics/Tracing/RuntimeEventSource.cs)存储库中，有更多的计数器实现可用作参考。

## <a name="concurrency"></a>并发

> [!TIP]
> EventCounters API 不能保证线程安全性。 当传递到 <xref:System.Diagnostics.Tracing.PollingCounter> 或 <xref:System.Diagnostics.Tracing.IncrementingPollingCounter> 实例的委托由多个线程调用时，你有责任保证委托的线程安全性。

例如，请考虑使用以下 <xref:System.Diagnostics.Tracing.EventSource> 来跟踪请求。

:::code language="csharp" source="snippets/EventCounters/RequestEventSource.cs":::

可以从请求处理程序调用 `AddRequest()` 方法，并且 `RequestRateCounter` 按计数器使用者指定的间隔轮询值。 但是，`AddRequest()` 方法可以同时由多个线程调用，将争用条件置于 `_requestCount`。 增加 `_requestCount` 的线程安全替代方法是使用 <xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType>。

```csharp
public void AddRequest() => Interlocked.Increment(ref _requestCount);
```

若要防止破坏（在 32 位体系结构上）对 `long` 字段 `_requestCount` 的读取，请使用 <xref:System.Threading.Interlocked.Read%2A?displayProperty=nameWithType>。

```csharp
_requestRateCounter = new IncrementingPollingCounter("request-rate", this, () => Interlocked.Read(ref _requestCount))
{
    DisplayName = "Request Rate",
    DisplayRateTimeScale = TimeSpan.FromSeconds(1)
};
```

## <a name="consume-eventcounters"></a>使用 EventCounters

使用 EventCounters 主要方式有两种：进程内或进程外。 EventCounters 的使用可以分为三层不同的使用技术。

- 通过 ETW 或 EventPipe 在原始流中传输事件：
  - ETW API 附带 Windows OS，EventPipe 可作为 [.NET API](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/diagnostics-client-library.md#1-attaching-to-a-process-and-dumping-out-all-the-runtime-gc-events-in-real-time-to-the-console) 或诊断 [IPC 协议](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/ipc-protocol.md)进行访问。
- 将二进制事件流解码为事件：
  - [TraceEvent 库](https://www.nuget.org/packages/Microsoft.Diagnostics.Tracing.TraceEvent)可处理 ETW 和 EventPipe 流格式。
- 命令行和 GUI 工具：
  - PerfView（ETW 或 EventPipe）、dotnet-counters（仅 EventPipe）和 dotnet-monitor（仅 EventPipe）等工具。

### <a name="consume-out-of-proc"></a>进程外使用

进程外使用 EventCounters 是一种非常常见的方法。 你可以使用 [dotnet-counters](dotnet-counters.md) 通过 EventPipe 以跨平台方式使用它们。 `dotnet-counters` 工具是一个跨平台 dotnet CLI 全局工具，可用于监视计数器值。 要了解如何使用 `dotnet-counters` 监视计数器，请参阅 [dotnet-counters](dotnet-counters.md) 或浏览[使用 EventCounters 衡量性能](event-counter-perf.md)教程。

#### <a name="dotnet-trace"></a>dotnet-trace

`dotnet-trace` 工具可用于通过 EventPipe 使用计数器数据。 下面是使用 `dotnet-trace` 收集计数器数据的一个示例。

```console
dotnet-trace collect --process-id <pid> Sample.EventCounter.Minimal:0:0:EventCounterIntervalSec=1
```

有关如何随着时间的推移收集计数器值的详细信息，请参阅 [dotnet-trace](dotnet-trace.md#use-dotnet-trace-to-collect-counter-values-over-time) 文档。

#### <a name="azure-application-insights"></a>Azure Application Insights

EventCounters 可由 Azure Monitor 使用，特别是 Azure Application Insights。 可以添加和删除计数器，并且可以自由指定自定义计数器或已知计数器。 有关详细信息，请参阅[自定义要收集的计数器](/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected)。

#### <a name="dotnet-monitor"></a>dotnet-monitor

`dotnet-monitor` 工具是一个实验性工具，通过它可以更轻松地访问 .NET 进程中的诊断信息。 该工具用作所有诊断工具的超集。 除跟踪外，它还可以监视指标、收集内存转储和收集 GC 转储。 它以 CLI 工具和 docker 映像的形式发布。 它公开了 REST API，以及通过 REST 调用发生的诊断项目集合。

有关详细信息，请参阅[实验性工具 dotnet-monitor 简介](https://devblogs.microsoft.com/dotnet/introducing-dotnet-monitor)。

### <a name="consume-in-proc"></a>进程内使用

可以通过 <xref:System.Diagnostics.Tracing.EventListener> API 使用计数器值。 <xref:System.Diagnostics.Tracing.EventListener> 是使用由应用程序中 <xref:System.Diagnostics.Tracing.EventSource> 的所有实例编写的任何事件的一种进程内方法。 有关如何使用 `EventListener` API 的详细信息，请参阅 <xref:System.Diagnostics.Tracing.EventListener>。

首先，需要启用生成计数器值的 <xref:System.Diagnostics.Tracing.EventSource>。 替代 <xref:System.Diagnostics.Tracing.EventListener.OnEventSourceCreated%2A?displayProperty=nameWithType> 方法以在创建 <xref:System.Diagnostics.Tracing.EventSource> 时获取通知，如果对于 EventCounters 这是正确的 <xref:System.Diagnostics.Tracing.EventSource>，则可在其上调用 <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A?displayProperty=nameWithType>。 下面是示例替代：

:::code language="csharp" source="snippets/EventCounters/SimpleEventListener.cs" range="11-22":::

#### <a name="sample-code"></a>代码示例

下面是一个示例 <xref:System.Diagnostics.Tracing.EventListener> 类，它打印出 .NET 运行时的 <xref:System.Diagnostics.Tracing.EventSource> 的所有计数器名称和值，用于每秒发布其内部计数器 (`System.Runtime`)。

:::code language="csharp" source="snippets/EventCounters/SimpleEventListener.cs":::

如下所示，调用 <xref:System.Diagnostics.Tracing.EventListener.EnableEvents%2A> 时必须确保在 `filterPayload` 参数中设置 `"EventCounterIntervalSec"` 参数。 否则，计数器将无法清空值，因为它不知道应清空哪个时间间隔。

## <a name="see-also"></a>另请参阅

- [dotnet-counters](dotnet-counters.md)
- [dotnet-trace](dotnet-trace.md)
- <xref:System.Diagnostics.Tracing.EventCounter>
- <xref:System.Diagnostics.Tracing.EventListener>
- <xref:System.Diagnostics.Tracing.EventSource>
