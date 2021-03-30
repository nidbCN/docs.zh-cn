---
title: 使用 PerfCollect 跟踪 .NET 应用程序。
description: 本教程引导你完成在 .NET 中使用 perfcollect 收集跟踪的过程。
ms.topic: tutorial
ms.date: 10/23/2020
ms.openlocfilehash: d6ee77fea5c419e00e684e8b1472278f752544b0
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874181"
---
# <a name="trace-net-applications-with-perfcollect"></a>使用 PerfCollect 跟踪 .NET 应用程序

本文适用于：✔️ .NET Core 2.1 SDK 及更高版本

在 Linux 上遇到性能问题时，可使用 `perfcollect` 收集跟踪，以便收集有关出现性能问题时计算机上发生的状况的详细信息。

`perfcollect` 是一个 bash 脚本，它利用 [Linux 跟踪工具包下一代 (LTTng)](https://lttng.org) 收集从运行时或任何 [EventSource](xref:System.Diagnostics.Tracing.EventListener) 写入的事件，并利用 [perf](https://perf.wiki.kernel.org/) 收集目标进程的 CPU 示例。

## <a name="prepare-your-machine"></a>准备计算机

按照以下步骤准备你的计算机以使用 `perfcollect` 收集性能跟踪。

> [!NOTE]
> 如果你处于容器环境中，则容器需要具有 `SYS_ADMIN` 功能。 有关使用 PerfCollect 跟踪容器内应用程序的详细信息，请参阅[在容器中收集诊断信息](./diagnostics-in-containers.md)。

1. 下载 `perfcollect`。

    > ```bash
    > curl -OL https://aka.ms/perfcollect
    > ```

2. 使脚本可执行。

    > ```bash
    > chmod +x perfcollect
    > ```

3. 安装跟踪必备组件 - 这些是实际的跟踪库。

    > ```bash
    > sudo ./perfcollect install
    > ```

    这将在你的计算机上安装以下必备组件：

    1. `perf`：Linux 性能事件子系统和配套的用户模式收集/查看器应用程序。 `perf` 是 Linux 内核源的一部分，但是默认情况下通常不安装。

    2. `LTTng`：用于捕获 CoreCLR 在运行时发出的事件数据。 然后使用这些数据分析各种运行时组件（如 GC、JIT 和线程池）的行为。

最新版本的 .NET Core 和 Linux 性能工具支持自动解析框架代码的方法名称。 如果使用的是 .NET Core 3.1 或更低版本，则需要执行额外的步骤。 有关详细信息，请参阅[解析框架符号](#resolve-framework-symbols)。

若要解析本机运行时 DLL 的方法名称（例如 libcoreclr.so），`perfcollect` 将在转换数据时为其解析符号，但前提是存在这些二进制文件的符号。 有关详细信息，请参阅[获取本机运行时的符号](#get-symbols-for-the-native-runtime)部分。

## <a name="collect-a-trace"></a>收集跟踪

1. 有两个可用的 shell - 一个用于控制跟踪，称为 [Trace]，另一个用于运行应用程序，称为 [App] 。

2. [Trace]：启动收集。

    > ```bash
    > sudo ./perfcollect collect sampleTrace
    > ```

    预期输出：

    > ```bash
    > Collection started.  Press CTRL+C to stop.
    > ```

3. [App]：使用以下环境变量设置应用程序 shell - 这将启用 CoreCLR 的跟踪配置。

    > ```bash
    > export COMPlus_PerfMapEnabled=1
    > export COMPlus_EnableEventLog=1
    > ```

4. [App]：运行应用 - 使其运行捕获性能问题所需的时间。 确切时间可以是所需的最短时间，只要足以捕获要调查的性能问题发生的时间窗口。

    > ```bash
    > dotnet run
    > ```

5. [Trace]：停止收集 - 按 CTRL+C。

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

    压缩的跟踪文件现存储在当前工作目录中。

## <a name="view-a-trace"></a>查看跟踪

有许多选项可用于查看收集的跟踪。 在 Windows 上，最好使用 [PerfView](https://aka.ms/perfview) 查看跟踪；但在 Linux 上，可以使用 `PerfCollect` 本身或 `TraceCompass` 直接进行查看。

### <a name="use-perfcollect-to-view-the-trace-file"></a>使用 PerfCollect 查看跟踪文件

你可以使用 perfcollect 本身来查看收集的跟踪。 为此，请使用以下命令：

```bash
./perfcollect view sampleTrace.trace.zip
```

默认情况下，这将使用 `perf` 显示应用程序的 CPU 跟踪。

要查看通过 `LTTng` 收集的事件，可以传入标志 `-viewer lttng` 以查看各个事件：

```bash
./perfcollect view sampleTrace.trace.zip -viewer lttng
```

这将使用 `babeltrace` 查看器打印事件有效负载：

```bash
# [01:02:18.189217659] (+0.020132603) ubuntu-xenial DotNETRuntime:ExceptionThrown_V1: { cpu_id = 0 }, { ExceptionType = "System.Exception", ExceptionMessage = "An exception happened", ExceptionEIP = 139875671834775, ExceptionHRESULT = 2148734208, ExceptionFlags = 16, ClrInstanceID = 0 }
# [01:02:18.189250227] (+0.020165171) ubuntu-xenial DotNETRuntime:ExceptionCatchStart: { cpu_id = 0 }, { EntryEIP = 139873639728404, MethodID = 139873626968120, MethodName = "void [helloworld] helloworld.Program::Main(string[])", ClrInstanceID = 0 }
```

### <a name="use-perfview-to-open-the-trace-file"></a>使用 PerfView 打开跟踪文件

要查看 CPU 示例和事件的聚合视图，可以在 Windows 计算机上使用 `PerfView`。

1. 将 trace.zip 文件从 Linux 复制到 Windows 计算机。

2. 从 <https://aka.ms/perfview> 下载 PerfView。

3. 运行 PerfView.exe

    > ```cmd
    > PerfView.exe <path to trace.zip file>
    > ```

PerfView 将基于跟踪文件中包含的数据显示受支持的视图列表。

- 对于 CPU 调查，请选择“CPU 堆栈”。

- 有关 GC 的详细信息，请选择“GCStats”。

- 有关每个进程/模块/方法的 JIT 信息，请选择“JITStats”。

- 如果没有所需信息的视图，可以尝试在原始事件视图中查找事件。  选择“事件”。

有关如何在 PerfView 中解释视图的详细信息，请参见视图本身的帮助链接，或者从 PerfView 的主窗口中，选择“帮助”->“用户指南”。

> [!NOTE]
> 通过 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=nameWithType> API 编写的事件（包括 Framework 中的事件）不会显示在其提供程序名称下。 相反，它们被编写为 `Microsoft-Windows-DotNETRuntime` 提供程序下的 `EventSourceEvent` 事件，其有效负载是经过 JSON 序列化的。

### <a name="use-tracecompass-to-open-the-trace-file"></a>使用 TraceCompass 打开跟踪文件

[Eclipse TraceCompass](https://www.eclipse.org/tracecompass/) 是另一个可用于查看跟踪的选项。 `TraceCompass` 也可以在 Linux 计算机上工作，因此不需要将跟踪移到 Windows 计算机上。 要使用 `TraceCompass` 打开跟踪文件，需要解压缩该文件。

```bash
unzip myTrace.trace.zip
```

`perfcollect` 将它收集的 LTTng 跟踪保存为 CTF 文件格式，位于 `lttngTrace` 的子目录中。 具体来说，CTF 文件将位于类似于 `lttngTrace/auto-20201025-101230\ust\uid\1000\64-bit\` 的目录中。

可以通过选择 `File -> Open Trace` 打开 `TraceCompass` 中的 CTF 跟踪文件，然后选择 `metadata` 文件。

有关详细信息，请参阅 [`TraceCompass` 文档](https://www.eclipse.org/tracecompass/)。

## <a name="resolve-framework-symbols"></a>解析框架符号

收集跟踪时，需要手动生成框架符号。 它们不同于应用级别符号，因为框架是预编译的，而应用代码是即时编译的。 对于预编译为本机代码的框架代码，需要调用 `crossgen`，它知道如何生成从本机代码到方法名称的映射。

`perfcollect` 可以处理大部分细节，但需要 `crossgen` 可用。 默认情况下，它不随 .NET 分发版一起安装。 如果 `crossgen` 不存在，`perfcollect` 会向你发出警告，并让你参考这些说明。 要修复这些问题，需要为正在使用的运行时获取正确版本的 crossgen。 如果将 crossgen 工具置于 .NET 运行时 DLL 的同一目录中（例如 libcoreclr.so），则 `perfcollect` 可以找到该工具并将框架符号添加到跟踪文件中。

通常，当你创建 .NET 应用程序时，它只为你编写的代码生成 DLL，对其余代码使用运行时的共享副本。   但是，你也可以生成应用程序所谓的“自包含”版本，其中包含所有运行时 DLL。 `crossgen` 是用于创建自包含应用的 NuGet 包的一部分，因此获取正确版本的 `crossgen` 的一种方法是创建应用程序的自包含包。

例如：

   >```bash
   > mkdir helloWorld
   > cd helloWorld
   > dotnet new console
   > dotnet publish --self-contained -r linux-x64
   >```

这将创建一个新的 Hello World 应用程序并将其生成为自包含应用。

创建自包含应用程序的副作用是 dotnet 工具会下载名为 runtime.linux-x64.microsoft.netcore.app 的 NuGet 包，并将其置于目录 ~/.nuget/packages/runtime.linux-x64.microsoft.netcore.app/VERSION 中，其中 VERSION 是 .NET Core 运行时的版本号（例如 2.1.0）。 在这个目录下是一个工具目录，其中有你需要的 crossgen 工具。 从 .NET Core 3.0 开始，包位置为 ~/.nuget/packages/microsoft.netcore.app.runtime.linux-x64/VERSION。

需要将 `crossgen` 工具放在应用程序实际使用的运行时旁边。 通常，你的应用程序使用安装在 /usr/share/dotnet/shared/Microsoft.NETCore.App/VERSION 上的 .NET Core 共享版本，其中 VERSION 是 .NET 运行时的版本号。 这是一个共享位置，因此你需要成为超级用户才能对其进行修改。 如果 VERSION 为 2.1.0，则更新 `crossgen` 的命令为：

   >```bash
   > sudo bash
   > cp ~/.nuget/packages/runtime.linux-x64.microsoft.netcore.app/2.1.0/tools/crossgen /usr/share/dotnet/shared/Microsoft.NETCore.App/2.1.0
   >```

完成此操作后，`perfcollect` 将使用 crossgen 包含框架符号。 `perfcollect` 以前发出的警告应会消失。 这在每台计算机上只需要执行一次（直到更新运行时为止）。

### <a name="alternative-turn-off-use-of-precompiled-code"></a>替代项：禁用预编译代码

如果无法更新 .NET 运行时（以添加 `crossgen`），或者如果上述过程出于某种原因而无效，可以使用另一种方法来获取框架符号。 你可以指示运行时不要使用预编译的框架代码。 代码将即时编译，不需要 `crossgen`。

> [!NOTE]
> 选择此方法可能会增加应用程序的启动时间。

为此，可以添加以下环境变量：

```bash
export COMPlus_ZapDisable=1
```

通过此更改，你应该会获得所有 .NET 代码的符号。

## <a name="get-symbols-for-the-native-runtime"></a>获取本机运行时的符号

大多数情况下，你感兴趣的是自己的代码，`perfcollect` 默认解析这些代码。 有时查看 .NET DLL 内部的情况很有用（这是上一节讨论的内容），但有时查看本机运行时 dll 中的情况（通常为 libcoreclr.so）也很有趣。  `perfcollect` 在转换其数据时将解析这些符号，但前提是存在这些本机 DLL 的符号（并且位于它们所对应的库的旁边）。

有一个名为 [dotnet-symbol](https://github.com/dotnet/symstore/blob/main/src/dotnet-symbol/README.md#symbol-downloader-dotnet-cli-extension) 的全局命令可以执行此操作。 使用 dotnet-symbol 获取本机运行时符号：

1. 安装 `dotnet-symbol`：

    ```bash
    dotnet tool install -g dotnet-symbol
    ```

2. 下载符号。 如果你安装的 .NET Core 运行时版本是 2.1.0，则执行此操作的命令是：

    ```bash
    mkdir mySymbols
    dotnet symbol --symbols --output mySymbols  /usr/share/dotnet/shared/Microsoft.NETCore.App/2.1.0/lib*.so
    ```

3. 将符号复制到正确的位置。

    ```bash
    sudo cp mySymbols/* /usr/share/dotnet/shared/Microsoft.NETCore.App/2.1.0
    ```

    如果由于没有对相应目录的写入访问权限而无法完成此操作，可以使用 `perf buildid-cache` 添加符号。

此后，当你运行 `perfcollect` 时，应获取本机 dll 的符号名称。

## <a name="collect-in-a-docker-container"></a>在 Docker 容器中收集信息

有关如何在容器环境中使用 `perfcollect` 的详细信息，请参阅[在容器中收集诊断信息](./diagnostics-in-containers.md)。

## <a name="learn-more-about-collection-options"></a>了解有关集合选项的详细信息

你可以使用 `perfcollect` 指定以下可选标志，以更好地满足诊断需求。

### <a name="collect-for-a-specific-duration"></a>在特定的时间内收集

如果要收集特定时间内的跟踪，可以使用 `-collectsec` 选项后跟一个数字，该数字指定收集跟踪的总秒数。

### <a name="collect-threadtime-traces"></a>收集线程时间跟踪

使用 `perfcollect` 指定 `-threadtime` 可让你收集每个线程的 CPU 使用率数据。 从而分析每个线程将 CPU 时间用在何处。

### <a name="collect-traces-for-managed-memory-and-garbage-collector-performance"></a>收集托管内存和垃圾回收器性能的跟踪

以下选项可让你专门收集运行时中的 GC 事件。

* `perfcollect collect -gccollectonly`

仅收集一组最少的 GC 收集事件。 这是最不详细的 GC 事件收集配置文件，对目标应用性能的影响最小。 此命令类似于 PerfView 中的 `PerfView.exe /GCCollectOnly collect` 命令。

* `perfcollect collect -gconly`

收集更详细的 GC 收集事件，包括 JIT、加载程序和异常事件。 这会请求更详细的事件（例如分配信息和 GC 联接信息），对目标应用性能产生的影响比 `-gccollectonly` 选项产生的影响更大。 此命令类似于 PerfView 中的 `PerfView.exe /GCOnly collect` 命令。

* `perfcollect collect -gcwithheap`

收集最详细的 GC 收集事件（用于跟踪堆的存活和移动情况）。 这会对 GC 行为进行深入分析，但会对性能产生较大的影响，因为每个 GC 都可能需要两倍的时间。 建议在生产环境中进行跟踪时，了解使用此跟踪选项的性能影响。
