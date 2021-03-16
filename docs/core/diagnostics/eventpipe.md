---
title: EventPipe 概述
description: 了解 EventPipe 以及如何使用它来跟踪 .NET 应用程序，以诊断性能问题。
ms.date: 11/09/2020
ms.topic: overview
ms.openlocfilehash: 213d15e48ac9d50af0c87565738f952295c4f041
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102105292"
---
# <a name="eventpipe"></a><span data-ttu-id="96b21-103">EventPipe</span><span class="sxs-lookup"><span data-stu-id="96b21-103">EventPipe</span></span>

<span data-ttu-id="96b21-104">EventPipe 是类似于 ETW 或 LTTng 的运行时组件，可用于收集跟踪数据。</span><span class="sxs-lookup"><span data-stu-id="96b21-104">EventPipe is a runtime component that can be used to collect tracing data, similar to ETW or LTTng.</span></span> <span data-ttu-id="96b21-105">EventPipe 的目标是使 .NET 开发人员能够轻松地跟踪其 .NET 应用程序，而无需依赖于平台特定的 OS 本机组件（如 ETW 或 LTTng）。</span><span class="sxs-lookup"><span data-stu-id="96b21-105">The goal of EventPipe is to allow .NET developers to easily trace their .NET applications without having to rely on platform-specific OS-native components such as ETW or LTTng.</span></span>

<span data-ttu-id="96b21-106">EventPipe 是众多诊断工具背后的一种机制，可用于接收运行时发出的事件以及通过 [EventSource](xref:System.Diagnostics.Tracing.EventSource) 编写的自定义事件。</span><span class="sxs-lookup"><span data-stu-id="96b21-106">EventPipe is the mechanism behind many of the diagnostic tools and can be used for consuming events emitted by the runtime as well as custom events written with [EventSource](xref:System.Diagnostics.Tracing.EventSource).</span></span>

<span data-ttu-id="96b21-107">本文概括介绍了 EventPipe，说明何时以及如何使用 EventPipe，以及如何对其进行配置以最大程度地满足你的需求。</span><span class="sxs-lookup"><span data-stu-id="96b21-107">This article is a high-level overview of EventPipe describing when and how to use EventPipe, and how to configure it to best suit your needs.</span></span>

## <a name="eventpipe-basics"></a><span data-ttu-id="96b21-108">EventPipe 基础知识</span><span class="sxs-lookup"><span data-stu-id="96b21-108">EventPipe basics</span></span>

<span data-ttu-id="96b21-109">EventPipe 聚合由运行时组件发出的事件（例如实时编译器或垃圾回收器）以及从库和用户代码中的 [EventSource](xref:System.Diagnostics.Tracing.EventSource) 实例编写的事件。</span><span class="sxs-lookup"><span data-stu-id="96b21-109">EventPipe aggregates events emitted by runtime components - for example, the Just-In-Time compiler or the garbage collector - and events written from [EventSource](xref:System.Diagnostics.Tracing.EventSource) instances in the libraries and user code.</span></span>

<span data-ttu-id="96b21-110">然后，将这些事件序列化，并直接写入文件或通过诊断端口在进程外使用。</span><span class="sxs-lookup"><span data-stu-id="96b21-110">The events are then serialized and can be written directly to a file or consumed through a Diagnostics Port from out-of-proces.</span></span> <span data-ttu-id="96b21-111">在 Windows 上，诊断端口作为 `NamedPipe` 实现。</span><span class="sxs-lookup"><span data-stu-id="96b21-111">On Windows, Diagnostic Ports are implemented as `NamedPipe`s.</span></span> <span data-ttu-id="96b21-112">在非 Windows 平台（如 Linux 或 macOS）上，该端口可使用 Unix 域套接字实现。</span><span class="sxs-lookup"><span data-stu-id="96b21-112">On non-Windows platforms, such as Linux or macOS, it is implemented using Unix Domain Sockets.</span></span> <span data-ttu-id="96b21-113">有关诊断端口以及如何通过其自定义进程内通信协议与之交互的详细信息，请参阅[诊断 IPC 协议文档](https://github.com/dotnet/diagnostics/blob/master/documentation/design-docs/ipc-protocol.md)。</span><span class="sxs-lookup"><span data-stu-id="96b21-113">For more information about the Diagnostics Port and how to interact with it via its custom inter-process communication protocol, see the [diagnostics IPC protocol documentation](https://github.com/dotnet/diagnostics/blob/master/documentation/design-docs/ipc-protocol.md).</span></span>

<span data-ttu-id="96b21-114">然后，EventPipe 以 `.nettrace` 文件格式写入序列化事件，可作为流通过诊断端口写入，也可直接写入文件。</span><span class="sxs-lookup"><span data-stu-id="96b21-114">EventPipe then writes the serialized events in the `.nettrace` file format, either as a stream via Diagnostic Ports or directly to a file.</span></span> <span data-ttu-id="96b21-115">若要了解有关 EventPipe 序列化格式的详细信息，请参阅 [EventPipe 格式文档](https://github.com/microsoft/perfview/blob/master/src/TraceEvent/EventPipe/EventPipeFormat.md)。</span><span class="sxs-lookup"><span data-stu-id="96b21-115">To learn more about the EventPipe serialization format, refer to the [EventPipe format documentation](https://github.com/microsoft/perfview/blob/master/src/TraceEvent/EventPipe/EventPipeFormat.md).</span></span>

## <a name="eventpipe-vs-etwlttng"></a><span data-ttu-id="96b21-116">EventPipe 与ETW/LTTng</span><span class="sxs-lookup"><span data-stu-id="96b21-116">EventPipe vs. ETW/LTTng</span></span>

<span data-ttu-id="96b21-117">EventPipe 是 .NET 运行时 (CoreCLR) 的一部分，旨在跨 .NET Core 支持的所有平台以相同的方式工作。</span><span class="sxs-lookup"><span data-stu-id="96b21-117">EventPipe is part of the .NET runtime (CoreCLR) and is designed to work the same way across all the platforms .NET Core supports.</span></span> <span data-ttu-id="96b21-118">这将允许基于 EventPipe 的跟踪工具（例如 `dotnet-counters`、`dotnet-gcdump` 和 `dotnet-trace`）无缝地跨平台工作。</span><span class="sxs-lookup"><span data-stu-id="96b21-118">This allows tracing tools based on EventPipe, such as `dotnet-counters`, `dotnet-gcdump`, and `dotnet-trace`, to work seamlessly across platforms.</span></span>

<span data-ttu-id="96b21-119">但是，因为 EventPipe 是一个运行时内置组件，所以它的作用域仅限于托管代码和运行时本身。</span><span class="sxs-lookup"><span data-stu-id="96b21-119">However, because EventPipe is a runtime built-in component, its scope is limited to managed code and the runtime itself.</span></span> <span data-ttu-id="96b21-120">EventPipe 不能用于跟踪一些较低级别的事件，例如，解析本机代码堆栈或获取各种内核事件。</span><span class="sxs-lookup"><span data-stu-id="96b21-120">EventPipe cannot be used for tracking some lower-level events, such as resolving native code stack or getting various kernel events.</span></span> <span data-ttu-id="96b21-121">如果在应用中使用 C/C++ 互操作或者要跟踪运行时本身（使用 C++ 编写的），或者要更深入地诊断需要内核事件（即本机线程上下文切换事件）的应用的行为，则应使用 ETW 或 [Perf/LTTng](./trace-perfcollect-lttng.md)。</span><span class="sxs-lookup"><span data-stu-id="96b21-121">If you use C/C++ interop in your app or you want to trace the runtime itself (which is written in C++), or want deeper diagnostics into the behavior of the app that requires kernel events (that is, native-thread context-switching events) you should use ETW or [perf/LTTng](./trace-perfcollect-lttng.md).</span></span>

<span data-ttu-id="96b21-122">EventPipe 和 ETW/LTTng 之间的另一个主要区别是管理员/根用户权限要求。</span><span class="sxs-lookup"><span data-stu-id="96b21-122">Another major difference between EventPipe and ETW/LTTng is admin/root privilege requirement.</span></span> <span data-ttu-id="96b21-123">若要使用 ETW 或 LTTng 跟踪应用程序，你需要是管理员/根用户。</span><span class="sxs-lookup"><span data-stu-id="96b21-123">To trace an application using ETW or LTTng you need to be an admin/root.</span></span> <span data-ttu-id="96b21-124">可使用 EventPipe 跟踪应用程序，前提是跟踪器（如 `dotnet-trace`）是由启动该应用程序的同一用户运行的。</span><span class="sxs-lookup"><span data-stu-id="96b21-124">Using EventPipe, you can trace applications as long as the tracer (for example, `dotnet-trace`) is run as the same user as the user that launched the application.</span></span>

<span data-ttu-id="96b21-125">下表汇总了 EventPipe 和 ETW/LTTng 之间的差异。</span><span class="sxs-lookup"><span data-stu-id="96b21-125">The following table is a summary of the differences between EventPipe and ETW/LTTng.</span></span>

|<span data-ttu-id="96b21-126">功能</span><span class="sxs-lookup"><span data-stu-id="96b21-126">Feature</span></span>|<span data-ttu-id="96b21-127">EventPipe</span><span class="sxs-lookup"><span data-stu-id="96b21-127">EventPipe</span></span>|<span data-ttu-id="96b21-128">ETW</span><span class="sxs-lookup"><span data-stu-id="96b21-128">ETW</span></span>|<span data-ttu-id="96b21-129">LTTng</span><span class="sxs-lookup"><span data-stu-id="96b21-129">LTTng</span></span>|
|-------|---------|---|-----------|
|<span data-ttu-id="96b21-130">跨平台</span><span class="sxs-lookup"><span data-stu-id="96b21-130">Cross-platform</span></span>|<span data-ttu-id="96b21-131">是</span><span class="sxs-lookup"><span data-stu-id="96b21-131">Yes</span></span>|<span data-ttu-id="96b21-132">否（仅在 Windows 上）</span><span class="sxs-lookup"><span data-stu-id="96b21-132">No (only on Windows)</span></span>|<span data-ttu-id="96b21-133">否（仅在受支持的 Linux 发行版上）</span><span class="sxs-lookup"><span data-stu-id="96b21-133">No (only on supported Linux distros)</span></span>|
|<span data-ttu-id="96b21-134">需要管理员/根用户权限</span><span class="sxs-lookup"><span data-stu-id="96b21-134">Require admin/root privilege</span></span>|<span data-ttu-id="96b21-135">否</span><span class="sxs-lookup"><span data-stu-id="96b21-135">No</span></span>|<span data-ttu-id="96b21-136">是</span><span class="sxs-lookup"><span data-stu-id="96b21-136">Yes</span></span>|<span data-ttu-id="96b21-137">是</span><span class="sxs-lookup"><span data-stu-id="96b21-137">Yes</span></span>|
|<span data-ttu-id="96b21-138">可获取 OS/内核事件</span><span class="sxs-lookup"><span data-stu-id="96b21-138">Can get OS/kernel events</span></span>|<span data-ttu-id="96b21-139">否</span><span class="sxs-lookup"><span data-stu-id="96b21-139">No</span></span>|<span data-ttu-id="96b21-140">是</span><span class="sxs-lookup"><span data-stu-id="96b21-140">Yes</span></span>|<span data-ttu-id="96b21-141">是</span><span class="sxs-lookup"><span data-stu-id="96b21-141">Yes</span></span>|
|<span data-ttu-id="96b21-142">可解析本机调用堆栈</span><span class="sxs-lookup"><span data-stu-id="96b21-142">Can resolve native callstacks</span></span>|<span data-ttu-id="96b21-143">否</span><span class="sxs-lookup"><span data-stu-id="96b21-143">No</span></span>|<span data-ttu-id="96b21-144">是</span><span class="sxs-lookup"><span data-stu-id="96b21-144">Yes</span></span>|<span data-ttu-id="96b21-145">是</span><span class="sxs-lookup"><span data-stu-id="96b21-145">Yes</span></span>|

## <a name="use-eventpipe-to-trace-your-net-application"></a><span data-ttu-id="96b21-146">使用 EventPipe 跟踪 .NET 应用程序</span><span class="sxs-lookup"><span data-stu-id="96b21-146">Use EventPipe to trace your .NET application</span></span>

<span data-ttu-id="96b21-147">可通过多种方式使用 EventPipe 跟踪 .NET 应用程序：</span><span class="sxs-lookup"><span data-stu-id="96b21-147">You can use EventPipe to trace your .NET application in many ways:</span></span>

* <span data-ttu-id="96b21-148">使用基于 EventPipe 构建的[诊断工具](#tools-that-use-eventpipe)之一。</span><span class="sxs-lookup"><span data-stu-id="96b21-148">Use one of the [diagnostics tools](#tools-that-use-eventpipe) that are built on top of EventPipe.</span></span>

* <span data-ttu-id="96b21-149">使用 [Microsoft.Diagnostics.NETCore.Client](https://github.com/dotnet/diagnostics/blob/master/documentation/diagnostics-client-library-instructions.md) 库来编写自己的工具，以自行配置和启动 EventPipe 会话。</span><span class="sxs-lookup"><span data-stu-id="96b21-149">Use [Microsoft.Diagnostics.NETCore.Client](https://github.com/dotnet/diagnostics/blob/master/documentation/diagnostics-client-library-instructions.md) library to write your own tool to configure and start EventPipe sessions yourself.</span></span>

* <span data-ttu-id="96b21-150">使用[环境变量](#trace-using-environment-variables)来启动 EventPipe。</span><span class="sxs-lookup"><span data-stu-id="96b21-150">Use [environment variables](#trace-using-environment-variables) to start EventPipe.</span></span>

<span data-ttu-id="96b21-151">生成包含 EventPipe 事件的 `nettrace` 文件之后，可在 [`PerfView`](https://github.com/Microsoft/perfview#perfview-overview) 或 Visual Studio 中查看该文件。</span><span class="sxs-lookup"><span data-stu-id="96b21-151">After you've produced a `nettrace` file that contains your EventPipe events, you can view the file in [`PerfView`](https://github.com/Microsoft/perfview#perfview-overview) or Visual Studio.</span></span> <span data-ttu-id="96b21-152">在非 Windows 平台上，可使用 [`dotnet-trace convert`](./dotnet-trace.md#dotnet-trace-convert) 命令将 `nettrace` 文件转换为 `speedscope` 或 `Chromium` 跟踪格式，并使用 [`speedscope`](https://www.speedscope.app/) 或 Chrome DevTools 查看该文件。</span><span class="sxs-lookup"><span data-stu-id="96b21-152">On non-Windows platforms, you can convert the `nettrace` file to a `speedscope` or `Chromium` trace format by using [`dotnet-trace convert`](./dotnet-trace.md#dotnet-trace-convert) command and view it with [`speedscope`](https://www.speedscope.app/) or Chrome DevTools.</span></span>

<span data-ttu-id="96b21-153">还以通过 [TraceEvent](https://github.com/Microsoft/perfview/blob/master/documentation/TraceEvent/TraceEventLibrary.md) 以编程方式分析 EventPipe 跟踪。</span><span class="sxs-lookup"><span data-stu-id="96b21-153">You can also analyze EventPipe traces programmatically with [TraceEvent](https://github.com/Microsoft/perfview/blob/master/documentation/TraceEvent/TraceEventLibrary.md).</span></span>

### <a name="tools-that-use-eventpipe"></a><span data-ttu-id="96b21-154">使用 EventPipe 的工具</span><span class="sxs-lookup"><span data-stu-id="96b21-154">Tools that use EventPipe</span></span>

<span data-ttu-id="96b21-155">这是使用 EventPipe 跟踪应用的最简单方法。</span><span class="sxs-lookup"><span data-stu-id="96b21-155">This is the easiest way to use EventPipe to trace your application.</span></span> <span data-ttu-id="96b21-156">若要详细了解如何使用这些工具，请参阅每个工具的文档。</span><span class="sxs-lookup"><span data-stu-id="96b21-156">To learn more about how to use each of these tools, refer to each tool's documentation.</span></span>

* <span data-ttu-id="96b21-157">[dotnet-counters](./dotnet-counters.md) 使你能够监视和收集由 .NET 运行时和核心库发出的各种指标，以及可以编写的自定义指标。</span><span class="sxs-lookup"><span data-stu-id="96b21-157">[dotnet-counters](./dotnet-counters.md) lets you monitor and collect various metrics emitted by the .NET runtime and core libraries, as well as custom metrics you can write.</span></span>

* <span data-ttu-id="96b21-158">[dotnet-gcdump](./dotnet-gcdump.md) 使你能够收集实时进程的 GC 堆转储以分析应用程序的托管堆。</span><span class="sxs-lookup"><span data-stu-id="96b21-158">[dotnet-gcdump](./dotnet-gcdump.md) lets you collect GC heap dumps of live processes for analyzing an application's managed heap.</span></span>

* <span data-ttu-id="96b21-159">[dotnet-trace](./dotnet-trace.md) 使你能够收集应用程序的跟踪以进行性能分析。</span><span class="sxs-lookup"><span data-stu-id="96b21-159">[dotnet-trace](./dotnet-trace.md) lets you collect traces of applications to analyze for performance.</span></span>

## <a name="trace-using-environment-variables"></a><span data-ttu-id="96b21-160">使用环境变量进行跟踪</span><span class="sxs-lookup"><span data-stu-id="96b21-160">Trace using environment variables</span></span>

<span data-ttu-id="96b21-161">使用 EventPipe 的首选机制是使用 `dotnet-trace` 或 `Microsoft.Diagnostics.NETCore.Client` 库。</span><span class="sxs-lookup"><span data-stu-id="96b21-161">The preferred mechanism for using EventPipe is to use `dotnet-trace` or the `Microsoft.Diagnostics.NETCore.Client` library.</span></span>

<span data-ttu-id="96b21-162">但是，可使用以下环境变量在应用上设置 EventPipe 会话，并使其将跟踪直接写入到文件。</span><span class="sxs-lookup"><span data-stu-id="96b21-162">However, you can use the following environment variables to set up an EventPipe session on an app and have it write the trace directly to a file.</span></span> <span data-ttu-id="96b21-163">若要停止跟踪，请退出应用程序。</span><span class="sxs-lookup"><span data-stu-id="96b21-163">To stop tracing, exit the application.</span></span>

* <span data-ttu-id="96b21-164">`COMPlus_EnableEventPipe`：将此值设置为 `1` 以启动直接写入到文件的 EventPipe 会话。</span><span class="sxs-lookup"><span data-stu-id="96b21-164">`COMPlus_EnableEventPipe`: Set this to `1` to start an EventPipe session that writes directly to a file.</span></span> <span data-ttu-id="96b21-165">默认值为 `0`。</span><span class="sxs-lookup"><span data-stu-id="96b21-165">The default value is `0`.</span></span>

* <span data-ttu-id="96b21-166">`COMPlus_EventPipeOutputPath`：输出 EventPipe 跟踪文件（配置为通过 `COMPlus_EnableEventPipe` 运行时）的路径。</span><span class="sxs-lookup"><span data-stu-id="96b21-166">`COMPlus_EventPipeOutputPath`: The path to the output EventPipe trace file when it's configured to run via `COMPlus_EnableEventPipe`.</span></span> <span data-ttu-id="96b21-167">默认值为 `trace.nettrace`，将在运行应用的同一目录中创建该默认值。</span><span class="sxs-lookup"><span data-stu-id="96b21-167">The default value is `trace.nettrace`, which will be created in the same directory that the app is running from.</span></span>

* <span data-ttu-id="96b21-168">`COMPlus_EventPipeCircularMB`：一个十六进制值，它表示 EventPipe 的内部缓冲区大小（以 MB 为单位）。</span><span class="sxs-lookup"><span data-stu-id="96b21-168">`COMPlus_EventPipeCircularMB`: A hexadecimal value that represents the size of EventPipe's internal buffer in megabytes.</span></span> <span data-ttu-id="96b21-169">仅当 EventPipe 配置为通过 `COMPlus_EnableEventPipe` 运行时，才使用此配置值。</span><span class="sxs-lookup"><span data-stu-id="96b21-169">This configuration value is only used when EventPipe is configured to run via `COMPlus_EnableEventPipe`.</span></span> <span data-ttu-id="96b21-170">默认缓冲区大小为 1024 MB，而由于 `0x400` == `1024`，它转换为设置成 `400` 的环境变量。</span><span class="sxs-lookup"><span data-stu-id="96b21-170">The default buffer size is 1024MB which translates to this environment variable being set to `400`, since `0x400` == `1024`.</span></span>

* <span data-ttu-id="96b21-171">`COMPlus_EventPipeProcNumbers`：将此项设置为 `1`，以在 EventPipe 事件标头中捕获处理器数。</span><span class="sxs-lookup"><span data-stu-id="96b21-171">`COMPlus_EventPipeProcNumbers`: Set this to `1` to enable capturing processor numbers in EventPipe event headers.</span></span> <span data-ttu-id="96b21-172">默认值为 `0`。</span><span class="sxs-lookup"><span data-stu-id="96b21-172">The default value is `0`.</span></span>

* <span data-ttu-id="96b21-173">`COMPlus_EventPipeConfig`：使用 `COMPlus_EnableEventPipe` 启动 EventPipe 会话时设置 EventPipe 会话配置。</span><span class="sxs-lookup"><span data-stu-id="96b21-173">`COMPlus_EventPipeConfig`: Sets up the EventPipe session configuration when starting an EventPipe session with `COMPlus_EnableEventPipe`.</span></span>
  <span data-ttu-id="96b21-174">语法如下：</span><span class="sxs-lookup"><span data-stu-id="96b21-174">The syntax is as follows:</span></span>

  `<provider>:<keyword>:<level>`

  <span data-ttu-id="96b21-175">还可通过使用逗号连接多个提供程序来指定它们：</span><span class="sxs-lookup"><span data-stu-id="96b21-175">You can also specify multiple providers by concatenating them with a comma:</span></span>

  `<provider1>:<keyword1>:<level1>,<provider2>:<keyword2>:<level2>`

  <span data-ttu-id="96b21-176">如果未设置此环境变量但通过 `COMPlus_EnableEventPipe` 启用了 EventPipe，则会通过使用以下关键字和级别启用以下提供程序来启动跟踪：</span><span class="sxs-lookup"><span data-stu-id="96b21-176">If this environment variable is not set but EventPipe is enabled by `COMPlus_EnableEventPipe`, it will start tracing by enabling the following providers with the following keywords and levels:</span></span>

  - `Microsoft-Windows-DotNETRuntime:4c14fccbd:5`
  - `Microsoft-Windows-DotNETRuntimePrivate:4002000b:5`
  - `Microsoft-DotNETCore-SampleProfiler:0:5`

  <span data-ttu-id="96b21-177">若要详细了解 .NET 中的一些已知提供程序，请参阅[已知事件提供程序](./well-known-event-providers.md)。</span><span class="sxs-lookup"><span data-stu-id="96b21-177">To learn more about some of the well-known providers in .NET, refer to [Well-known Event Providers](./well-known-event-providers.md).</span></span>
