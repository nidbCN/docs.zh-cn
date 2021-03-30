---
title: EventPipe 概述
description: 了解 EventPipe 以及如何使用它来跟踪 .NET 应用程序，以诊断性能问题。
ms.date: 11/09/2020
ms.topic: overview
ms.openlocfilehash: de00f448dea9752c3dff2af35977d97c07aec6f3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874155"
---
# <a name="eventpipe"></a>EventPipe

EventPipe 是类似于 ETW 或 LTTng 的运行时组件，可用于收集跟踪数据。 EventPipe 的目标是使 .NET 开发人员能够轻松地跟踪其 .NET 应用程序，而无需依赖于平台特定的 OS 本机组件（如 ETW 或 LTTng）。

EventPipe 是众多诊断工具背后的一种机制，可用于接收运行时发出的事件以及通过 [EventSource](xref:System.Diagnostics.Tracing.EventSource) 编写的自定义事件。

本文概括介绍了 EventPipe，说明何时以及如何使用 EventPipe，以及如何对其进行配置以最大程度地满足你的需求。

## <a name="eventpipe-basics"></a>EventPipe 基础知识

EventPipe 聚合由运行时组件发出的事件（例如实时编译器或垃圾回收器）以及从库和用户代码中的 [EventSource](xref:System.Diagnostics.Tracing.EventSource) 实例编写的事件。

然后，将这些事件序列化，并直接写入文件或通过诊断端口在进程外使用。 在 Windows 上，诊断端口作为 `NamedPipe` 实现。 在非 Windows 平台（如 Linux 或 macOS）上，该端口可使用 Unix 域套接字实现。 有关诊断端口以及如何通过其自定义进程内通信协议与之交互的详细信息，请参阅[诊断 IPC 协议文档](https://github.com/dotnet/diagnostics/blob/main/documentation/design-docs/ipc-protocol.md)。

然后，EventPipe 以 `.nettrace` 文件格式写入序列化事件，可作为流通过诊断端口写入，也可直接写入文件。 若要了解有关 EventPipe 序列化格式的详细信息，请参阅 [EventPipe 格式文档](https://github.com/microsoft/perfview/blob/master/src/TraceEvent/EventPipe/EventPipeFormat.md)。

## <a name="eventpipe-vs-etwlttng"></a>EventPipe 与ETW/LTTng

EventPipe 是 .NET 运行时 (CoreCLR) 的一部分，旨在跨 .NET Core 支持的所有平台以相同的方式工作。 这将允许基于 EventPipe 的跟踪工具（例如 `dotnet-counters`、`dotnet-gcdump` 和 `dotnet-trace`）无缝地跨平台工作。

但是，因为 EventPipe 是一个运行时内置组件，所以它的作用域仅限于托管代码和运行时本身。 EventPipe 不能用于跟踪一些较低级别的事件，例如，解析本机代码堆栈或获取各种内核事件。 如果在应用中使用 C/C++ 互操作或者要跟踪运行时本身（使用 C++ 编写的），或者要更深入地诊断需要内核事件（即本机线程上下文切换事件）的应用的行为，则应使用 ETW 或 [Perf/LTTng](./trace-perfcollect-lttng.md)。

EventPipe 和 ETW/LTTng 之间的另一个主要区别是管理员/根用户权限要求。 若要使用 ETW 或 LTTng 跟踪应用程序，你需要是管理员/根用户。 可使用 EventPipe 跟踪应用程序，前提是跟踪器（如 `dotnet-trace`）是由启动该应用程序的同一用户运行的。

下表汇总了 EventPipe 和 ETW/LTTng 之间的差异。

|功能|EventPipe|ETW|LTTng|
|-------|---------|---|-----------|
|跨平台|是|否（仅在 Windows 上）|否（仅在受支持的 Linux 发行版上）|
|需要管理员/根用户权限|否|是|是|
|可获取 OS/内核事件|否|是|是|
|可解析本机调用堆栈|否|是|是|

## <a name="use-eventpipe-to-trace-your-net-application"></a>使用 EventPipe 跟踪 .NET 应用程序

可通过多种方式使用 EventPipe 跟踪 .NET 应用程序：

* 使用基于 EventPipe 构建的[诊断工具](#tools-that-use-eventpipe)之一。

* 使用 [Microsoft.Diagnostics.NETCore.Client](https://github.com/dotnet/diagnostics/blob/main/documentation/diagnostics-client-library-instructions.md) 库来编写自己的工具，以自行配置和启动 EventPipe 会话。

* 使用[环境变量](#trace-using-environment-variables)来启动 EventPipe。

生成包含 EventPipe 事件的 `nettrace` 文件之后，可在 [`PerfView`](https://github.com/Microsoft/perfview#perfview-overview) 或 Visual Studio 中查看该文件。 在非 Windows 平台上，可使用 [`dotnet-trace convert`](./dotnet-trace.md#dotnet-trace-convert) 命令将 `nettrace` 文件转换为 `speedscope` 或 `Chromium` 跟踪格式，并使用 [`speedscope`](https://www.speedscope.app/) 或 Chrome DevTools 查看该文件。

还以通过 [TraceEvent](https://github.com/Microsoft/perfview/blob/master/documentation/TraceEvent/TraceEventLibrary.md) 以编程方式分析 EventPipe 跟踪。

### <a name="tools-that-use-eventpipe"></a>使用 EventPipe 的工具

这是使用 EventPipe 跟踪应用的最简单方法。 若要详细了解如何使用这些工具，请参阅每个工具的文档。

* [dotnet-counters](./dotnet-counters.md) 使你能够监视和收集由 .NET 运行时和核心库发出的各种指标，以及可以编写的自定义指标。

* [dotnet-gcdump](./dotnet-gcdump.md) 使你能够收集实时进程的 GC 堆转储以分析应用程序的托管堆。

* [dotnet-trace](./dotnet-trace.md) 使你能够收集应用程序的跟踪以进行性能分析。

## <a name="trace-using-environment-variables"></a>使用环境变量进行跟踪

使用 EventPipe 的首选机制是使用 `dotnet-trace` 或 `Microsoft.Diagnostics.NETCore.Client` 库。

但是，可使用以下环境变量在应用上设置 EventPipe 会话，并使其将跟踪直接写入到文件。 若要停止跟踪，请退出应用程序。

* `COMPlus_EnableEventPipe`：将此值设置为 `1` 以启动直接写入到文件的 EventPipe 会话。 默认值为 `0`。

* `COMPlus_EventPipeOutputPath`：输出 EventPipe 跟踪文件（配置为通过 `COMPlus_EnableEventPipe` 运行时）的路径。 默认值为 `trace.nettrace`，将在运行应用的同一目录中创建该默认值。

* `COMPlus_EventPipeCircularMB`：一个十六进制值，它表示 EventPipe 的内部缓冲区大小（以 MB 为单位）。 仅当 EventPipe 配置为通过 `COMPlus_EnableEventPipe` 运行时，才使用此配置值。 默认缓冲区大小为 1024 MB，而由于 `0x400` == `1024`，它转换为设置成 `400` 的环境变量。

  > [!NOTE]
  > 如果目标进程过于频繁地写入事件，则它可能会溢出此缓冲区，并且某些事件可能会被丢弃。 如果丢弃的事件过多，请增加缓冲区大小，查看丢弃的事件数是否减少。 如果丢弃的事件数未随缓冲区大小的增加而减少，则可能是因为读取器的速度较慢，导致无法刷新目标进程的缓冲区。

* `COMPlus_EventPipeProcNumbers`：将此项设置为 `1`，以在 EventPipe 事件标头中捕获处理器数。 默认值为 `0`。

* `COMPlus_EventPipeConfig`：使用 `COMPlus_EnableEventPipe` 启动 EventPipe 会话时设置 EventPipe 会话配置。
  语法如下：

  `<provider>:<keyword>:<level>`

  还可通过使用逗号连接多个提供程序来指定它们：

  `<provider1>:<keyword1>:<level1>,<provider2>:<keyword2>:<level2>`

  如果未设置此环境变量但通过 `COMPlus_EnableEventPipe` 启用了 EventPipe，则会通过使用以下关键字和级别启用以下提供程序来启动跟踪：

  - `Microsoft-Windows-DotNETRuntime:4c14fccbd:5`
  - `Microsoft-Windows-DotNETRuntimePrivate:4002000b:5`
  - `Microsoft-DotNETCore-SampleProfiler:0:5`

  若要详细了解 .NET 中的一些已知提供程序，请参阅[已知事件提供程序](./well-known-event-providers.md)。
