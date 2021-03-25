---
title: 使用 PerfCollect 跟踪 .NET 应用程序。
description: 本教程引导你完成在 .NET 中使用 perfcollect 收集跟踪的过程。
ms.topic: tutorial
ms.date: 10/23/2020
ms.openlocfilehash: 20e1bf56714fb32b5231d45b0ba35cdfcedaea2e
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103189925"
---
# <a name="trace-net-applications-with-perfcollect"></a><span data-ttu-id="e1600-103">使用 PerfCollect 跟踪 .NET 应用程序</span><span class="sxs-lookup"><span data-stu-id="e1600-103">Trace .NET applications with PerfCollect</span></span>

<span data-ttu-id="e1600-104">本文适用于：✔️ .NET Core 2.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="e1600-104">**This article applies to: ✔️** .NET Core 2.1 SDK and later versions</span></span>

<span data-ttu-id="e1600-105">在 Linux 上遇到性能问题时，可使用 `perfcollect` 收集跟踪，以便收集有关出现性能问题时计算机上发生的状况的详细信息。</span><span class="sxs-lookup"><span data-stu-id="e1600-105">When performance problems are encountered on Linux, collecting a trace with `perfcollect` can be used to gather detailed information about what was happening on the machine at the time of the performance problem.</span></span>

<span data-ttu-id="e1600-106">`perfcollect` 是一个 bash 脚本，它利用 [Linux 跟踪工具包下一代 (LTTng)](https://lttng.org) 收集从运行时或任何 [EventSource](xref:System.Diagnostics.Tracing.EventListener) 写入的事件，并利用 [perf](https://perf.wiki.kernel.org/) 收集目标进程的 CPU 示例。</span><span class="sxs-lookup"><span data-stu-id="e1600-106">`perfcollect` is a bash script that leverages [Linux Tracing Tookit-Next Generation (LTTng)](https://lttng.org) to collect events written from the runtime or any [EventSource](xref:System.Diagnostics.Tracing.EventListener), as well as [perf](https://perf.wiki.kernel.org/) to collect CPU samples of the target process.</span></span>

## <a name="prepare-your-machine"></a><span data-ttu-id="e1600-107">准备计算机</span><span class="sxs-lookup"><span data-stu-id="e1600-107">Prepare your machine</span></span>

<span data-ttu-id="e1600-108">按照以下步骤准备你的计算机以使用 `perfcollect` 收集性能跟踪。</span><span class="sxs-lookup"><span data-stu-id="e1600-108">Follow these steps to prepare your machine to collect a performance trace with `perfcollect`.</span></span>

> [!NOTE]
> <span data-ttu-id="e1600-109">如果你处于容器环境中，则容器需要具有 `SYS_ADMIN` 功能。</span><span class="sxs-lookup"><span data-stu-id="e1600-109">If you are in a container environment, your container needs to have `SYS_ADMIN` capability.</span></span> <span data-ttu-id="e1600-110">有关使用 PerfCollect 跟踪容器内应用程序的详细信息，请参阅[在容器中收集诊断信息](./diagnostics-in-containers.md)。</span><span class="sxs-lookup"><span data-stu-id="e1600-110">For more information on tracing applications inside containers using PerfCollect, see [Collect diagnostics in containers](./diagnostics-in-containers.md).</span></span>

1. <span data-ttu-id="e1600-111">下载 `perfcollect`。</span><span class="sxs-lookup"><span data-stu-id="e1600-111">Download `perfcollect`.</span></span>

    > ```bash
    > curl -OL https://aka.ms/perfcollect
    > ```

2. <span data-ttu-id="e1600-112">使脚本可执行。</span><span class="sxs-lookup"><span data-stu-id="e1600-112">Make the script executable.</span></span>

    > ```bash
    > chmod +x perfcollect
    > ```

3. <span data-ttu-id="e1600-113">安装跟踪必备组件 - 这些是实际的跟踪库。</span><span class="sxs-lookup"><span data-stu-id="e1600-113">Install tracing prerequisites - these are the actual tracing libraries.</span></span>

    > ```bash
    > sudo ./perfcollect install
    > ```

    <span data-ttu-id="e1600-114">这将在你的计算机上安装以下必备组件：</span><span class="sxs-lookup"><span data-stu-id="e1600-114">This will install the following prerequisites on your machine:</span></span>

    1. <span data-ttu-id="e1600-115">`perf`：Linux 性能事件子系统和配套的用户模式收集/查看器应用程序。</span><span class="sxs-lookup"><span data-stu-id="e1600-115">`perf`: the Linux Performance Events subsystem and companion user-mode collection/viewer application.</span></span> <span data-ttu-id="e1600-116">`perf` 是 Linux 内核源的一部分，但是默认情况下通常不安装。</span><span class="sxs-lookup"><span data-stu-id="e1600-116">`perf` is part of the Linux kernel source, but is not usually installed by default.</span></span>

    2. <span data-ttu-id="e1600-117">`LTTng`：用于捕获 CoreCLR 在运行时发出的事件数据。</span><span class="sxs-lookup"><span data-stu-id="e1600-117">`LTTng`: Used to capture event data emitted at runtime by CoreCLR.</span></span> <span data-ttu-id="e1600-118">然后使用这些数据分析各种运行时组件（如 GC、JIT 和线程池）的行为。</span><span class="sxs-lookup"><span data-stu-id="e1600-118">This data is then used to analyze the behavior of various runtime components such as the GC, JIT, and thread pool.</span></span>

<span data-ttu-id="e1600-119">最新版本的 .NET Core 和 Linux 性能工具支持自动解析框架代码的方法名称。</span><span class="sxs-lookup"><span data-stu-id="e1600-119">Recent versions of .NET Core and the Linux perf tool support automatic resolution of method names for framework code.</span></span> <span data-ttu-id="e1600-120">如果使用的是 .NET Core 3.1 或更低版本，则需要执行额外的步骤。</span><span class="sxs-lookup"><span data-stu-id="e1600-120">If you are working with .NET Core version 3.1 or less, an extra step is necessary.</span></span> <span data-ttu-id="e1600-121">有关详细信息，请参阅[解析框架符号](#resolve-framework-symbols)。</span><span class="sxs-lookup"><span data-stu-id="e1600-121">See [Resolving Framework Symbols](#resolve-framework-symbols) for details.</span></span>

<span data-ttu-id="e1600-122">若要解析本机运行时 DLL 的方法名称（例如 libcoreclr.so），`perfcollect` 将在转换数据时为其解析符号，但前提是存在这些二进制文件的符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-122">For resolving method names of native runtime DLLs (such as libcoreclr.so), `perfcollect` will resolve symbols for them when it converts the data, but only if the symbols for these binaries are present.</span></span> <span data-ttu-id="e1600-123">有关详细信息，请参阅[获取本机运行时的符号](#get-symbols-for-the-native-runtime)部分。</span><span class="sxs-lookup"><span data-stu-id="e1600-123">See [Getting Symbols for the Native Runtime](#get-symbols-for-the-native-runtime) section for details.</span></span>

## <a name="collect-a-trace"></a><span data-ttu-id="e1600-124">收集跟踪</span><span class="sxs-lookup"><span data-stu-id="e1600-124">Collect a trace</span></span>

1. <span data-ttu-id="e1600-125">有两个可用的 shell - 一个用于控制跟踪，称为 [Trace]，另一个用于运行应用程序，称为 [App] 。</span><span class="sxs-lookup"><span data-stu-id="e1600-125">Have two shells available - one for controlling tracing, referred to as **[Trace]**, and one for running the application, referred to as **[App]**.</span></span>

2. <span data-ttu-id="e1600-126">[Trace]：启动收集。</span><span class="sxs-lookup"><span data-stu-id="e1600-126">**[Trace]** Start collection.</span></span>

    > ```bash
    > sudo ./perfcollect collect sampleTrace
    > ```

    <span data-ttu-id="e1600-127">预期输出：</span><span class="sxs-lookup"><span data-stu-id="e1600-127">Expected Output:</span></span>

    > ```bash
    > Collection started.  Press CTRL+C to stop.
    > ```

3. <span data-ttu-id="e1600-128">[App]：使用以下环境变量设置应用程序 shell - 这将启用 CoreCLR 的跟踪配置。</span><span class="sxs-lookup"><span data-stu-id="e1600-128">**[App]** Set up the application shell with the following environment variables - this enables tracing configuration of CoreCLR.</span></span>

    > ```bash
    > export COMPlus_PerfMapEnabled=1
    > export COMPlus_EnableEventLog=1
    > ```

4. <span data-ttu-id="e1600-129">[App]：运行应用 - 使其运行捕获性能问题所需的时间。</span><span class="sxs-lookup"><span data-stu-id="e1600-129">**[App]** Run the app - let it run as long as you need to in order to capture the performance problem.</span></span> <span data-ttu-id="e1600-130">确切时间可以是所需的最短时间，只要足以捕获要调查的性能问题发生的时间窗口。</span><span class="sxs-lookup"><span data-stu-id="e1600-130">The exact length can be as short as you need as long as it sufficiently captures the window of time where the performance problem you want to investigate occurs.</span></span>

    > ```bash
    > dotnet run
    > ```

5. <span data-ttu-id="e1600-131">[Trace]：停止收集 - 按 CTRL+C。</span><span class="sxs-lookup"><span data-stu-id="e1600-131">**[Trace]** Stop collection - hit CTRL+C.</span></span>

    > ```bash
    > ^C
    > ...STOPPED.
    >
    > Starting post-processing. This may take some time.
    >
    > Generating native image symbol files
    > ...SKIPPED
    > Saving native symbols
    > ...FINISHED
    > Exporting perf.data file
    > ...FINISHED
    > Compressing trace files
    > ...FINISHED
    > Cleaning up artifacts
    > ...FINISHED
    >
    > Trace saved to sampleTrace.trace.zip
    > ```

    <span data-ttu-id="e1600-132">压缩的跟踪文件现存储在当前工作目录中。</span><span class="sxs-lookup"><span data-stu-id="e1600-132">The compressed trace file is now stored in the current working directory.</span></span>

## <a name="view-a-trace"></a><span data-ttu-id="e1600-133">查看跟踪</span><span class="sxs-lookup"><span data-stu-id="e1600-133">View a trace</span></span>

<span data-ttu-id="e1600-134">有许多选项可用于查看收集的跟踪。</span><span class="sxs-lookup"><span data-stu-id="e1600-134">There are a number of options for viewing the trace that was collected.</span></span> <span data-ttu-id="e1600-135">在 Windows 上，最好使用 [PerfView](https://aka.ms/perfview) 查看跟踪；但在 Linux 上，可以使用 `PerfCollect` 本身或 `TraceCompass` 直接进行查看。</span><span class="sxs-lookup"><span data-stu-id="e1600-135">Traces are best viewed using [PerfView](https://aka.ms/perfview) on Windows, but they can be viewed directly on Linux using `PerfCollect` itself or `TraceCompass`.</span></span>

### <a name="use-perfcollect-to-view-the-trace-file"></a><span data-ttu-id="e1600-136">使用 PerfCollect 查看跟踪文件</span><span class="sxs-lookup"><span data-stu-id="e1600-136">Use PerfCollect to view the trace file</span></span>

<span data-ttu-id="e1600-137">你可以使用 perfcollect 本身来查看收集的跟踪。</span><span class="sxs-lookup"><span data-stu-id="e1600-137">You can use perfcollect itself to view the trace that you collected.</span></span> <span data-ttu-id="e1600-138">为此，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="e1600-138">To do this, use the following command:</span></span>

```bash
./perfcollect view sampleTrace.trace.zip
```

<span data-ttu-id="e1600-139">默认情况下，这将使用 `perf` 显示应用程序的 CPU 跟踪。</span><span class="sxs-lookup"><span data-stu-id="e1600-139">By default, this will show the CPU trace of the application using `perf`.</span></span>

<span data-ttu-id="e1600-140">要查看通过 `LTTng` 收集的事件，可以传入标志 `-viewer lttng` 以查看各个事件：</span><span class="sxs-lookup"><span data-stu-id="e1600-140">To look at the events that were collected via `LTTng`, you can pass in the flag `-viewer lttng` to see the individual events:</span></span>

```bash
./perfcollect view sampleTrace.trace.zip -viewer lttng
```

<span data-ttu-id="e1600-141">这将使用 `babeltrace` 查看器打印事件有效负载：</span><span class="sxs-lookup"><span data-stu-id="e1600-141">This will use `babeltrace` viewer to print the events payload:</span></span>

```bash
# [01:02:18.189217659] (+0.020132603) ubuntu-xenial DotNETRuntime:ExceptionThrown_V1: { cpu_id = 0 }, { ExceptionType = "System.Exception", ExceptionMessage = "An exception happened", ExceptionEIP = 139875671834775, ExceptionHRESULT = 2148734208, ExceptionFlags = 16, ClrInstanceID = 0 }
# [01:02:18.189250227] (+0.020165171) ubuntu-xenial DotNETRuntime:ExceptionCatchStart: { cpu_id = 0 }, { EntryEIP = 139873639728404, MethodID = 139873626968120, MethodName = "void [helloworld] helloworld.Program::Main(string[])", ClrInstanceID = 0 }
```

### <a name="use-perfview-to-open-the-trace-file"></a><span data-ttu-id="e1600-142">使用 PerfView 打开跟踪文件</span><span class="sxs-lookup"><span data-stu-id="e1600-142">Use PerfView to open the trace file</span></span>

<span data-ttu-id="e1600-143">要查看 CPU 示例和事件的聚合视图，可以在 Windows 计算机上使用 `PerfView`。</span><span class="sxs-lookup"><span data-stu-id="e1600-143">To see an aggregate view of both the CPU sample and the events, you can use `PerfView` on a Windows machine.</span></span>

1. <span data-ttu-id="e1600-144">将 trace.zip 文件从 Linux 复制到 Windows 计算机。</span><span class="sxs-lookup"><span data-stu-id="e1600-144">Copy the trace.zip file from Linux to a Windows machine.</span></span>

2. <span data-ttu-id="e1600-145">从 <https://aka.ms/perfview> 下载 PerfView。</span><span class="sxs-lookup"><span data-stu-id="e1600-145">Download PerfView from <https://aka.ms/perfview>.</span></span>

3. <span data-ttu-id="e1600-146">运行 PerfView.exe</span><span class="sxs-lookup"><span data-stu-id="e1600-146">Run PerfView.exe</span></span>

    > ```cmd
    > PerfView.exe <path to trace.zip file>
    > ```

<span data-ttu-id="e1600-147">PerfView 将基于跟踪文件中包含的数据显示受支持的视图列表。</span><span class="sxs-lookup"><span data-stu-id="e1600-147">PerfView will display the list of views that are supported based on the data contained in the trace file.</span></span>

- <span data-ttu-id="e1600-148">对于 CPU 调查，请选择“CPU 堆栈”。</span><span class="sxs-lookup"><span data-stu-id="e1600-148">For CPU investigations, choose **CPU stacks**.</span></span>

- <span data-ttu-id="e1600-149">有关 GC 的详细信息，请选择“GCStats”。</span><span class="sxs-lookup"><span data-stu-id="e1600-149">For detailed GC information, choose **GCStats**.</span></span>

- <span data-ttu-id="e1600-150">有关每个进程/模块/方法的 JIT 信息，请选择“JITStats”。</span><span class="sxs-lookup"><span data-stu-id="e1600-150">For per-process/module/method JIT information, choose **JITStats**.</span></span>

- <span data-ttu-id="e1600-151">如果没有所需信息的视图，可以尝试在原始事件视图中查找事件。</span><span class="sxs-lookup"><span data-stu-id="e1600-151">If there is not a view for the information you need, you can try looking for the events in the raw events view.</span></span>  <span data-ttu-id="e1600-152">选择“事件”。</span><span class="sxs-lookup"><span data-stu-id="e1600-152">Choose **Events**.</span></span>

<span data-ttu-id="e1600-153">有关如何在 PerfView 中解释视图的详细信息，请参见视图本身的帮助链接，或者从 PerfView 的主窗口中，选择“帮助”->“用户指南”。</span><span class="sxs-lookup"><span data-stu-id="e1600-153">For more information on how to interpret views in PerfView, see help links in the view itself, or from the main window in PerfView, choose **Help->Users Guide**.</span></span>

> [!NOTE]
> <span data-ttu-id="e1600-154">通过 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=nameWithType> API 编写的事件（包括 Framework 中的事件）不会显示在其提供程序名称下。</span><span class="sxs-lookup"><span data-stu-id="e1600-154">Events written via <xref:System.Diagnostics.Tracing.EventSource?displayProperty=nameWithType> API (including the events from Framework) won't show up under their provider name.</span></span> <span data-ttu-id="e1600-155">相反，它们被编写为 `Microsoft-Windows-DotNETRuntime` 提供程序下的 `EventSourceEvent` 事件，其有效负载是经过 JSON 序列化的。</span><span class="sxs-lookup"><span data-stu-id="e1600-155">Instead, they are written as `EventSourceEvent` events under `Microsoft-Windows-DotNETRuntime` provider and their payloads are JSON serialized.</span></span>

### <a name="use-tracecompass-to-open-the-trace-file"></a><span data-ttu-id="e1600-156">使用 TraceCompass 打开跟踪文件</span><span class="sxs-lookup"><span data-stu-id="e1600-156">Use TraceCompass to open the trace file</span></span>

<span data-ttu-id="e1600-157">[Eclipse TraceCompass](https://www.eclipse.org/tracecompass/) 是另一个可用于查看跟踪的选项。</span><span class="sxs-lookup"><span data-stu-id="e1600-157">[Eclipse TraceCompass](https://www.eclipse.org/tracecompass/) is another option you can use to view the traces.</span></span> <span data-ttu-id="e1600-158">`TraceCompass` 也可以在 Linux 计算机上工作，因此不需要将跟踪移到 Windows 计算机上。</span><span class="sxs-lookup"><span data-stu-id="e1600-158">`TraceCompass` works on Linux machines as well, so you don't need to move your trace over to a Windows machine.</span></span> <span data-ttu-id="e1600-159">要使用 `TraceCompass` 打开跟踪文件，需要解压缩该文件。</span><span class="sxs-lookup"><span data-stu-id="e1600-159">To use `TraceCompass` to open your trace file, you will need to unzip the file.</span></span>

```bash
unzip myTrace.trace.zip
```

<span data-ttu-id="e1600-160">`perfcollect` 将它收集的 LTTng 跟踪保存为 CTF 文件格式，位于 `lttngTrace` 的子目录中。</span><span class="sxs-lookup"><span data-stu-id="e1600-160">`perfcollect` will save the LTTng trace it collected into a CTF file format in a subdirectory in the `lttngTrace`.</span></span> <span data-ttu-id="e1600-161">具体来说，CTF 文件将位于类似于 `lttngTrace/auto-20201025-101230\ust\uid\1000\64-bit\` 的目录中。</span><span class="sxs-lookup"><span data-stu-id="e1600-161">Specifically, the CTF file will be located in a directory that looks like `lttngTrace/auto-20201025-101230\ust\uid\1000\64-bit\`.</span></span>

<span data-ttu-id="e1600-162">可以通过选择 `File -> Open Trace` 打开 `TraceCompass` 中的 CTF 跟踪文件，然后选择 `metadata` 文件。</span><span class="sxs-lookup"><span data-stu-id="e1600-162">You can open the CTF trace file in `TraceCompass` by selecting `File -> Open Trace` and select the `metadata` file.</span></span>

<span data-ttu-id="e1600-163">有关详细信息，请参阅 [`TraceCompass` 文档](https://www.eclipse.org/tracecompass/)。</span><span class="sxs-lookup"><span data-stu-id="e1600-163">For more details, please refer to [`TraceCompass` documentation](https://www.eclipse.org/tracecompass/).</span></span>

## <a name="resolve-framework-symbols"></a><span data-ttu-id="e1600-164">解析框架符号</span><span class="sxs-lookup"><span data-stu-id="e1600-164">Resolve framework symbols</span></span>

<span data-ttu-id="e1600-165">收集跟踪时，需要手动生成框架符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-165">Framework symbols need to be manually generated at the time the trace is collected.</span></span> <span data-ttu-id="e1600-166">它们不同于应用级别符号，因为框架是预编译的，而应用代码是即时编译的。</span><span class="sxs-lookup"><span data-stu-id="e1600-166">They are different than app-level symbols because the framework is pre-compiled while app code is just-in-time-compiled.</span></span> <span data-ttu-id="e1600-167">对于预编译为本机代码的框架代码，需要调用 `crossgen`，它知道如何生成从本机代码到方法名称的映射。</span><span class="sxs-lookup"><span data-stu-id="e1600-167">For framework code that was precompiled to native code, you need to call `crossgen` that knows how to generate the mapping from the native code to the name of the methods.</span></span>

<span data-ttu-id="e1600-168">`perfcollect` 可以处理大部分细节，但需要 `crossgen` 可用。</span><span class="sxs-lookup"><span data-stu-id="e1600-168">`perfcollect` can handle most of the details for you, but it needs to have `crossgen` available.</span></span> <span data-ttu-id="e1600-169">默认情况下，它不随 .NET 分发版一起安装。</span><span class="sxs-lookup"><span data-stu-id="e1600-169">By default it is not installed with .NET distribution.</span></span> <span data-ttu-id="e1600-170">如果 `crossgen` 不存在，`perfcollect` 会向你发出警告，并让你参考这些说明。</span><span class="sxs-lookup"><span data-stu-id="e1600-170">If `crossgen` is not there, `perfcollect` warns you and refers you to these instructions.</span></span> <span data-ttu-id="e1600-171">要修复这些问题，需要为正在使用的运行时获取正确版本的 crossgen。</span><span class="sxs-lookup"><span data-stu-id="e1600-171">To fix things you need to fetch exactly the right version of crossgen for the runtime you are using.</span></span> <span data-ttu-id="e1600-172">如果将 crossgen 工具置于 .NET 运行时 DLL 的同一目录中（例如 libcoreclr.so），则 `perfcollect` 可以找到该工具并将框架符号添加到跟踪文件中。</span><span class="sxs-lookup"><span data-stu-id="e1600-172">If you place the crossgen tool in the same directory as the .NET Runtime DLLs (for example, libcoreclr.so), then `perfcollect` can find it and add the framework symbols to the trace file for you.</span></span>

<span data-ttu-id="e1600-173">通常，当你创建 .NET 应用程序时，它只为你编写的代码生成 DLL，对其余代码使用运行时的共享副本。</span><span class="sxs-lookup"><span data-stu-id="e1600-173">Normally when you create a .NET application, it just generates the DLL for the code you wrote, using a shared copy of the runtime for the rest.</span></span>   <span data-ttu-id="e1600-174">但是，你也可以生成应用程序所谓的“自包含”版本，其中包含所有运行时 DLL。</span><span class="sxs-lookup"><span data-stu-id="e1600-174">However you can also generate what is called a 'self-contained' version of an application and this contains all runtime DLLs.</span></span> <span data-ttu-id="e1600-175">`crossgen` 是用于创建自包含应用的 NuGet 包的一部分，因此获取正确版本的 `crossgen` 的一种方法是创建应用程序的自包含包。</span><span class="sxs-lookup"><span data-stu-id="e1600-175">`crossgen` is part of the NuGet package that is used to create self-contained apps, so one way of getting the right version of `crossgen` is to create a self-contained package of your application.</span></span>

<span data-ttu-id="e1600-176">例如：</span><span class="sxs-lookup"><span data-stu-id="e1600-176">For example:</span></span>

   >```bash
   > mkdir helloWorld
   > cd helloWorld
   > dotnet new console
   > dotnet publish --self-contained -r linux-x64
   >```

<span data-ttu-id="e1600-177">这将创建一个新的 Hello World 应用程序并将其生成为自包含应用。</span><span class="sxs-lookup"><span data-stu-id="e1600-177">This creates a new Hello World application and builds it as a self-contained app.</span></span>

<span data-ttu-id="e1600-178">创建自包含应用程序的副作用是 dotnet 工具会下载名为 runtime.linux-x64.microsoft.netcore.app 的 NuGet 包，并将其置于目录 ~/.nuget/packages/runtime.linux-x64.microsoft.netcore.app/VERSION 中，其中 VERSION 是 .NET Core 运行时的版本号（例如 2.1.0）。</span><span class="sxs-lookup"><span data-stu-id="e1600-178">As a side effect of creating the self-contained application the dotnet tool will download a NuGet package called runtime.linux-x64.microsoft.netcore.app and place it in the directory ~/.nuget/packages/runtime.linux-x64.microsoft.netcore.app/VERSION, where VERSION is the version number of your .NET Core runtime (for example, 2.1.0).</span></span> <span data-ttu-id="e1600-179">在这个目录下是一个工具目录，其中有你需要的 crossgen 工具。</span><span class="sxs-lookup"><span data-stu-id="e1600-179">Under that is a tools directory and inside there is the crossgen tool you need.</span></span> <span data-ttu-id="e1600-180">从 .NET Core 3.0 开始，包位置为 ~/.nuget/packages/microsoft.netcore.app.runtime.linux-x64/VERSION。</span><span class="sxs-lookup"><span data-stu-id="e1600-180">Starting with .NET Core 3.0, the package location is ~/.nuget/packages/microsoft.netcore.app.runtime.linux-x64/VERSION.</span></span>

<span data-ttu-id="e1600-181">需要将 `crossgen` 工具放在应用程序实际使用的运行时旁边。</span><span class="sxs-lookup"><span data-stu-id="e1600-181">The `crossgen` tool needs to be put next to the runtime that is actually used by your application.</span></span> <span data-ttu-id="e1600-182">通常，你的应用程序使用安装在 /usr/share/dotnet/shared/Microsoft.NETCore.App/VERSION 上的 .NET Core 共享版本，其中 VERSION 是 .NET 运行时的版本号。</span><span class="sxs-lookup"><span data-stu-id="e1600-182">Typically your app uses the shared version of .NET Core that is installed at /usr/share/dotnet/shared/Microsoft.NETCore.App/VERSION where VERSION is the version number of the .NET Runtime.</span></span> <span data-ttu-id="e1600-183">这是一个共享位置，因此你需要成为超级用户才能对其进行修改。</span><span class="sxs-lookup"><span data-stu-id="e1600-183">This is a shared location, so you need to be super-user to modify it.</span></span> <span data-ttu-id="e1600-184">如果 VERSION 为 2.1.0，则更新 `crossgen` 的命令为：</span><span class="sxs-lookup"><span data-stu-id="e1600-184">If the VERSION is 2.1.0 the commands to update `crossgen` would be:</span></span>

   >```bash
   > sudo bash
   > cp ~/.nuget/packages/runtime.linux-x64.microsoft.netcore.app/2.1.0/tools/crossgen /usr/share/dotnet/shared/Microsoft.NETCore.App/2.1.0
   >```

<span data-ttu-id="e1600-185">完成此操作后，`perfcollect` 将使用 crossgen 包含框架符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-185">Once you have done this, `perfcollect` will use crossgen to include framework symbols.</span></span> <span data-ttu-id="e1600-186">`perfcollect` 以前发出的警告应会消失。</span><span class="sxs-lookup"><span data-stu-id="e1600-186">The warning that `perfcollect` used to issue should go away.</span></span> <span data-ttu-id="e1600-187">这在每台计算机上只需要执行一次（直到更新运行时为止）。</span><span class="sxs-lookup"><span data-stu-id="e1600-187">This only has to be one once per machine (until you update your runtime).</span></span>

### <a name="alternative-turn-off-use-of-precompiled-code"></a><span data-ttu-id="e1600-188">替代项：禁用预编译代码</span><span class="sxs-lookup"><span data-stu-id="e1600-188">Alternative: Turn off use of precompiled code</span></span>

<span data-ttu-id="e1600-189">如果无法更新 .NET 运行时（以添加 `crossgen`），或者如果上述过程出于某种原因而无效，可以使用另一种方法来获取框架符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-189">If you don't have the ability to update the .NET Runtime (to add `crossgen`), or if the above procedure did not work for some reason, there is another approach to getting framework symbols.</span></span> <span data-ttu-id="e1600-190">你可以指示运行时不要使用预编译的框架代码。</span><span class="sxs-lookup"><span data-stu-id="e1600-190">You can tell the runtime to simply not use the precompiled framework code.</span></span> <span data-ttu-id="e1600-191">代码将即时编译，不需要 `crossgen`。</span><span class="sxs-lookup"><span data-stu-id="e1600-191">The code will be Just-In-Time compiled and `crossgen` is not needed.</span></span>

> [!NOTE]
> <span data-ttu-id="e1600-192">选择此方法可能会增加应用程序的启动时间。</span><span class="sxs-lookup"><span data-stu-id="e1600-192">Choosing this approach may increase the startup time for your application.</span></span>

<span data-ttu-id="e1600-193">为此，可以添加以下环境变量：</span><span class="sxs-lookup"><span data-stu-id="e1600-193">To do this, you can add the following environment variable:</span></span>

```bash
export COMPlus_ZapDisable=1
```

<span data-ttu-id="e1600-194">通过此更改，你应该会获得所有 .NET 代码的符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-194">With this change, you should get the symbols for all .NET code.</span></span>

## <a name="get-symbols-for-the-native-runtime"></a><span data-ttu-id="e1600-195">获取本机运行时的符号</span><span class="sxs-lookup"><span data-stu-id="e1600-195">Get symbols for the native runtime</span></span>

<span data-ttu-id="e1600-196">大多数情况下，你感兴趣的是自己的代码，`perfcollect` 默认解析这些代码。</span><span class="sxs-lookup"><span data-stu-id="e1600-196">Most of the time you are interested in your own code, which `perfcollect` resolves by default.</span></span> <span data-ttu-id="e1600-197">有时查看 .NET DLL 内部的情况很有用（这是上一节讨论的内容），但有时查看本机运行时 dll 中的情况（通常为 libcoreclr.so）也很有趣。</span><span class="sxs-lookup"><span data-stu-id="e1600-197">Sometimes it is useful to see what is going on inside the .NET DLLs (which is what the last section was about), but sometimes what is going on in the native runtime dlls (typically libcoreclr.so), is interesting.</span></span>  <span data-ttu-id="e1600-198">`perfcollect` 在转换其数据时将解析这些符号，但前提是存在这些本机 DLL 的符号（并且位于它们所对应的库的旁边）。</span><span class="sxs-lookup"><span data-stu-id="e1600-198">`perfcollect` will resolve the symbols for these when it converts its data, but only if the symbols for these native DLLs are present (and are beside the library they are for).</span></span>

<span data-ttu-id="e1600-199">有一个名为 [dotnet-symbol](https://github.com/dotnet/symstore/blob/master/src/dotnet-symbol/README.md#symbol-downloader-dotnet-cli-extension) 的全局命令可以执行此操作。</span><span class="sxs-lookup"><span data-stu-id="e1600-199">There is a global command called [dotnet-symbol](https://github.com/dotnet/symstore/blob/master/src/dotnet-symbol/README.md#symbol-downloader-dotnet-cli-extension) that does this.</span></span> <span data-ttu-id="e1600-200">使用 dotnet-symbol 获取本机运行时符号：</span><span class="sxs-lookup"><span data-stu-id="e1600-200">To use dotnet-symbol to get native runtime symbols:</span></span>

1. <span data-ttu-id="e1600-201">安装 `dotnet-symbol`：</span><span class="sxs-lookup"><span data-stu-id="e1600-201">Install `dotnet-symbol`:</span></span>

    ```bash
    dotnet tool install -g dotnet-symbol
    ```

2. <span data-ttu-id="e1600-202">下载符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-202">Download the symbols.</span></span> <span data-ttu-id="e1600-203">如果你安装的 .NET Core 运行时版本是 2.1.0，则执行此操作的命令是：</span><span class="sxs-lookup"><span data-stu-id="e1600-203">If your installed version of the .NET Core runtime is 2.1.0, the command to do this is:</span></span>

    ```bash
    mkdir mySymbols
    dotnet symbol --symbols --output mySymbols  /usr/share/dotnet/shared/Microsoft.NETCore.App/2.1.0/lib*.so
    ```

3. <span data-ttu-id="e1600-204">将符号复制到正确的位置。</span><span class="sxs-lookup"><span data-stu-id="e1600-204">Copy the symbols to the correct place.</span></span>

    ```bash
    sudo cp mySymbols/* /usr/share/dotnet/shared/Microsoft.NETCore.App/2.1.0
    ```

    <span data-ttu-id="e1600-205">如果由于没有对相应目录的写入访问权限而无法完成此操作，可以使用 `perf buildid-cache` 添加符号。</span><span class="sxs-lookup"><span data-stu-id="e1600-205">If this cannot be done because you do not have write access to the appropriate directory, you can use `perf buildid-cache` to add the symbols.</span></span>

<span data-ttu-id="e1600-206">此后，当你运行 `perfcollect` 时，应获取本机 dll 的符号名称。</span><span class="sxs-lookup"><span data-stu-id="e1600-206">After this, you should get symbolic names for the native dlls when you run `perfcollect`.</span></span>

## <a name="collect-in-a-docker-container"></a><span data-ttu-id="e1600-207">在 Docker 容器中收集信息</span><span class="sxs-lookup"><span data-stu-id="e1600-207">Collect in a Docker container</span></span>

<span data-ttu-id="e1600-208">有关如何在容器环境中使用 `perfcollect` 的详细信息，请参阅[在容器中收集诊断信息](./diagnostics-in-containers.md)。</span><span class="sxs-lookup"><span data-stu-id="e1600-208">For more information on how to use `perfcollect` in container environments, see [Collect diagnostics in containers](./diagnostics-in-containers.md).</span></span>

## <a name="learn-more-about-collection-options"></a><span data-ttu-id="e1600-209">了解有关集合选项的详细信息</span><span class="sxs-lookup"><span data-stu-id="e1600-209">Learn more about collection options</span></span>

<span data-ttu-id="e1600-210">你可以使用 `perfcollect` 指定以下可选标志，以更好地满足诊断需求。</span><span class="sxs-lookup"><span data-stu-id="e1600-210">You can specify the following optional flags with `perfcollect` to better suit your diagnostic needs.</span></span>

### <a name="collect-for-a-specific-duration"></a><span data-ttu-id="e1600-211">在特定的时间内收集</span><span class="sxs-lookup"><span data-stu-id="e1600-211">Collect for a specific duration</span></span>

<span data-ttu-id="e1600-212">如果要收集特定时间内的跟踪，可以使用 `-collectsec` 选项后跟一个数字，该数字指定收集跟踪的总秒数。</span><span class="sxs-lookup"><span data-stu-id="e1600-212">When you want to collect a trace for a specific duration, you can use `-collectsec` option followed by a number specifying the total seconds to collect a trace for.</span></span>

### <a name="collect-threadtime-traces"></a><span data-ttu-id="e1600-213">收集线程时间跟踪</span><span class="sxs-lookup"><span data-stu-id="e1600-213">Collect threadtime traces</span></span>

<span data-ttu-id="e1600-214">使用 `perfcollect` 指定 `-threadtime` 可让你收集每个线程的 CPU 使用率数据。</span><span class="sxs-lookup"><span data-stu-id="e1600-214">Specifying `-threadtime` with `perfcollect` lets you collect per-thread CPU usage data.</span></span> <span data-ttu-id="e1600-215">从而分析每个线程将 CPU 时间用在何处。</span><span class="sxs-lookup"><span data-stu-id="e1600-215">This lets you analyze where every thread was spending its CPU time.</span></span>

### <a name="collect-traces-for-managed-memory-and-garbage-collector-performance"></a><span data-ttu-id="e1600-216">收集托管内存和垃圾回收器性能的跟踪</span><span class="sxs-lookup"><span data-stu-id="e1600-216">Collect traces for managed memory and garbage collector performance</span></span>

<span data-ttu-id="e1600-217">以下选项可让你专门收集运行时中的 GC 事件。</span><span class="sxs-lookup"><span data-stu-id="e1600-217">The following options let you specifically collect the GC events from the runtime.</span></span>

* `perfcollect collect -gccollectonly`

<span data-ttu-id="e1600-218">仅收集一组最少的 GC 收集事件。</span><span class="sxs-lookup"><span data-stu-id="e1600-218">Collect only a minimal set of GC Collection events.</span></span> <span data-ttu-id="e1600-219">这是最不详细的 GC 事件收集配置文件，对目标应用性能的影响最小。</span><span class="sxs-lookup"><span data-stu-id="e1600-219">This is the least verbose GC eventing collection profile with the lowest impact on the target app's performance.</span></span> <span data-ttu-id="e1600-220">此命令类似于 PerfView 中的 `PerfView.exe /GCCollectOnly collect` 命令。</span><span class="sxs-lookup"><span data-stu-id="e1600-220">This command is analogous to `PerfView.exe /GCCollectOnly collect` command in PerfView.</span></span>

* `perfcollect collect -gconly`

<span data-ttu-id="e1600-221">收集更详细的 GC 收集事件，包括 JIT、加载程序和异常事件。</span><span class="sxs-lookup"><span data-stu-id="e1600-221">Collect more verbose GC collection events with JIT, Loader, and Exception events.</span></span> <span data-ttu-id="e1600-222">这会请求更详细的事件（例如分配信息和 GC 联接信息），对目标应用性能产生的影响比 `-gccollectonly` 选项产生的影响更大。</span><span class="sxs-lookup"><span data-stu-id="e1600-222">This requests more verbose events (such as the allocation information and GC join information) and will have more impact to the target app's performance than `-gccollectonly` option.</span></span> <span data-ttu-id="e1600-223">此命令类似于 PerfView 中的 `PerfView.exe /GCOnly collect` 命令。</span><span class="sxs-lookup"><span data-stu-id="e1600-223">This command is analogous to `PerfView.exe /GCOnly collect` command in PerfView.</span></span>

* `perfcollect collect -gcwithheap`

<span data-ttu-id="e1600-224">收集最详细的 GC 收集事件（用于跟踪堆的存活和移动情况）。</span><span class="sxs-lookup"><span data-stu-id="e1600-224">Collect the most verbose GC collection events which tracks the heap survival and movements as well.</span></span> <span data-ttu-id="e1600-225">这会对 GC 行为进行深入分析，但会对性能产生较大的影响，因为每个 GC 都可能需要两倍的时间。</span><span class="sxs-lookup"><span data-stu-id="e1600-225">This gives in-depth analysis of the GC behavior but will incur high performance cost as each GC can take more than two times longer.</span></span> <span data-ttu-id="e1600-226">建议在生产环境中进行跟踪时，了解使用此跟踪选项的性能影响。</span><span class="sxs-lookup"><span data-stu-id="e1600-226">It is recommended you understand the performance implication of using this trace option when tracing in production environments.</span></span>
