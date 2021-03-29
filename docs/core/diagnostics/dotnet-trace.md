---
title: dotnet-trace 诊断工具 - .NET CLI
description: 了解如何通过使用 .NET EventPipe 来安装和使用 dotnet-trace CLI 工具，以在没有本机探查器的情况下收集运行中的进程的 .NET 跟踪。
ms.date: 11/17/2020
ms.openlocfilehash: e4e5bf91a7e6a9bf98e8cb006864b4cbc5ca17a2
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624183"
---
# <a name="dotnet-trace-performance-analysis-utility"></a><span data-ttu-id="dfb41-103">dotnet-trace 性能分析实用工具</span><span class="sxs-lookup"><span data-stu-id="dfb41-103">dotnet-trace performance analysis utility</span></span>

<span data-ttu-id="dfb41-104"> 本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="dfb41-104">**This article applies to:** ✔️ .NET Core 3.0 SDK and later versions</span></span>

## <a name="install"></a><span data-ttu-id="dfb41-105">安装</span><span class="sxs-lookup"><span data-stu-id="dfb41-105">Install</span></span>

<span data-ttu-id="dfb41-106">可采用两种方法来下载和安装 `dotnet-trace`：</span><span class="sxs-lookup"><span data-stu-id="dfb41-106">There are two ways to download and install `dotnet-trace`:</span></span>

- <span data-ttu-id="dfb41-107">**dotnet 全局工具：**</span><span class="sxs-lookup"><span data-stu-id="dfb41-107">**dotnet global tool:**</span></span>

  <span data-ttu-id="dfb41-108">若要安装最新版 `dotnet-trace` [NuGet 包](https://www.nuget.org/packages/dotnet-trace)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="dfb41-108">To install the latest release version of the `dotnet-trace` [NuGet package](https://www.nuget.org/packages/dotnet-trace), use the [dotnet tool install](../tools/dotnet-tool-install.md) command:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-trace
  ```

- <span data-ttu-id="dfb41-109">**直接下载：**</span><span class="sxs-lookup"><span data-stu-id="dfb41-109">**Direct download:**</span></span>

  <span data-ttu-id="dfb41-110">下载与平台相匹配的工具可执行文件：</span><span class="sxs-lookup"><span data-stu-id="dfb41-110">Download the tool executable that matches your platform:</span></span>

  | <span data-ttu-id="dfb41-111">(OS)</span><span class="sxs-lookup"><span data-stu-id="dfb41-111">OS</span></span>  | <span data-ttu-id="dfb41-112">平台</span><span class="sxs-lookup"><span data-stu-id="dfb41-112">Platform</span></span> |
  | --- | -------- |
  | <span data-ttu-id="dfb41-113">Windows</span><span class="sxs-lookup"><span data-stu-id="dfb41-113">Windows</span></span> | <span data-ttu-id="dfb41-114">[x86](https://aka.ms/dotnet-trace/win-x86) \| [x64](https://aka.ms/dotnet-trace/win-x64) \| [arm](https://aka.ms/dotnet-trace/win-arm) \| [arm-x64](https://aka.ms/dotnet-trace/win-arm64)</span><span class="sxs-lookup"><span data-stu-id="dfb41-114">[x86](https://aka.ms/dotnet-trace/win-x86) \| [x64](https://aka.ms/dotnet-trace/win-x64) \| [arm](https://aka.ms/dotnet-trace/win-arm) \| [arm-x64](https://aka.ms/dotnet-trace/win-arm64)</span></span> |
  | <span data-ttu-id="dfb41-115">macOS</span><span class="sxs-lookup"><span data-stu-id="dfb41-115">macOS</span></span>   | [<span data-ttu-id="dfb41-116">x64</span><span class="sxs-lookup"><span data-stu-id="dfb41-116">x64</span></span>](https://aka.ms/dotnet-trace/osx-x64) |
  | <span data-ttu-id="dfb41-117">Linux</span><span class="sxs-lookup"><span data-stu-id="dfb41-117">Linux</span></span>   | <span data-ttu-id="dfb41-118">[x64](https://aka.ms/dotnet-trace/linux-x64) \| [arm](https://aka.ms/dotnet-trace/linux-arm) \| [arm64](https://aka.ms/dotnet-trace/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-trace/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-trace/linux-musl-arm64)</span><span class="sxs-lookup"><span data-stu-id="dfb41-118">[x64](https://aka.ms/dotnet-trace/linux-x64) \| [arm](https://aka.ms/dotnet-trace/linux-arm) \| [arm64](https://aka.ms/dotnet-trace/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-trace/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-trace/linux-musl-arm64)</span></span> |

> [!NOTE]
> <span data-ttu-id="dfb41-119">若要在 x86 应用上使用 `dotnet-trace`，需要使用相应的 x86 版本的工具。</span><span class="sxs-lookup"><span data-stu-id="dfb41-119">To use `dotnet-trace` on an x86 app, you need a corresponding x86 version of the tool.</span></span>

## <a name="synopsis"></a><span data-ttu-id="dfb41-120">摘要</span><span class="sxs-lookup"><span data-stu-id="dfb41-120">Synopsis</span></span>

```console
dotnet-trace [-h, --help] [--version] <command>
```

## <a name="description"></a><span data-ttu-id="dfb41-121">描述</span><span class="sxs-lookup"><span data-stu-id="dfb41-121">Description</span></span>

<span data-ttu-id="dfb41-122">`dotnet-trace` 工具：</span><span class="sxs-lookup"><span data-stu-id="dfb41-122">The `dotnet-trace` tool:</span></span>

* <span data-ttu-id="dfb41-123">是一个跨平台的 .NET Core 工具。</span><span class="sxs-lookup"><span data-stu-id="dfb41-123">Is a cross-platform .NET Core tool.</span></span>
* <span data-ttu-id="dfb41-124">在不使用本机探查器的情况下启用正在运行的进程的 .NET Core 跟踪集合。</span><span class="sxs-lookup"><span data-stu-id="dfb41-124">Enables the collection of .NET Core traces of a running process without a native profiler.</span></span>
* <span data-ttu-id="dfb41-125">是基于 .NET Core 运行时的 [`EventPipe`](./eventpipe.md) 构建的。</span><span class="sxs-lookup"><span data-stu-id="dfb41-125">Is built on [`EventPipe`](./eventpipe.md) of the .NET Core runtime.</span></span>
* <span data-ttu-id="dfb41-126">在 Windows、Linux 或 macOS 上提供相同体验。</span><span class="sxs-lookup"><span data-stu-id="dfb41-126">Delivers the same experience on Windows, Linux, or macOS.</span></span>

## <a name="options"></a><span data-ttu-id="dfb41-127">选项</span><span class="sxs-lookup"><span data-stu-id="dfb41-127">Options</span></span>

- **`-h|--help`**

  <span data-ttu-id="dfb41-128">显示命令行帮助。</span><span class="sxs-lookup"><span data-stu-id="dfb41-128">Shows command-line help.</span></span>

- **`--version`**

  <span data-ttu-id="dfb41-129">显示 dotnet-dump 实用工具的版本。</span><span class="sxs-lookup"><span data-stu-id="dfb41-129">Displays the version of the dotnet-trace utility.</span></span>

## <a name="commands"></a><span data-ttu-id="dfb41-130">命令</span><span class="sxs-lookup"><span data-stu-id="dfb41-130">Commands</span></span>

| <span data-ttu-id="dfb41-131">命令</span><span class="sxs-lookup"><span data-stu-id="dfb41-131">Command</span></span>                                                   |
|-----------------------------------------------------------|
| [<span data-ttu-id="dfb41-132">dotnet-trace collect</span><span class="sxs-lookup"><span data-stu-id="dfb41-132">dotnet-trace collect</span></span>](#dotnet-trace-collect)             |
| [<span data-ttu-id="dfb41-133">dotnet-trace convert</span><span class="sxs-lookup"><span data-stu-id="dfb41-133">dotnet-trace convert</span></span>](#dotnet-trace-convert)             |
| [<span data-ttu-id="dfb41-134">dotnet-trace ps</span><span class="sxs-lookup"><span data-stu-id="dfb41-134">dotnet-trace ps</span></span>](#dotnet-trace-ps)                       |
| [<span data-ttu-id="dfb41-135">dotnet-trace list-profiles</span><span class="sxs-lookup"><span data-stu-id="dfb41-135">dotnet-trace list-profiles</span></span>](#dotnet-trace-list-profiles) |

## <a name="dotnet-trace-collect"></a><span data-ttu-id="dfb41-136">dotnet-trace collect</span><span class="sxs-lookup"><span data-stu-id="dfb41-136">dotnet-trace collect</span></span>

<span data-ttu-id="dfb41-137">从正在运行的进程中收集诊断跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-137">Collects a diagnostic trace from a running process.</span></span>

### <a name="synopsis"></a><span data-ttu-id="dfb41-138">摘要</span><span class="sxs-lookup"><span data-stu-id="dfb41-138">Synopsis</span></span>

```console
dotnet-trace collect [--buffersize <size>] [--clreventlevel <clreventlevel>] [--clrevents <clrevents>]
    [--format <Chromium|NetTrace|Speedscope>] [-h|--help]
    [-n, --name <name>] [--diagnostic-port] [-o|--output <trace-file-path>] [-p|--process-id <pid>]
    [--profile <profile-name>] [--providers <list-of-comma-separated-providers>]
    [-- <command>] (for target applications running .NET 5.0 or later)
```

### <a name="options"></a><span data-ttu-id="dfb41-139">选项</span><span class="sxs-lookup"><span data-stu-id="dfb41-139">Options</span></span>

- **`--buffersize <size>`**

  <span data-ttu-id="dfb41-140">设置内存中循环缓冲区的大小（以 MB 表示）。</span><span class="sxs-lookup"><span data-stu-id="dfb41-140">Sets the size of the in-memory circular buffer, in megabytes.</span></span> <span data-ttu-id="dfb41-141">默认值为 256 MB。</span><span class="sxs-lookup"><span data-stu-id="dfb41-141">Default 256 MB.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dfb41-142">如果目标进程过于频繁地写入事件，则它可能会溢出此缓冲区，并且某些事件可能会被丢弃。</span><span class="sxs-lookup"><span data-stu-id="dfb41-142">If the target process writes events too frequently, it can overflow this buffer and some events might be dropped.</span></span> <span data-ttu-id="dfb41-143">如果丢弃的事件过多，请增加缓冲区大小，查看丢弃的事件数是否减少。</span><span class="sxs-lookup"><span data-stu-id="dfb41-143">If too many events are getting dropped, increase the buffer size to see if the number of dropped events reduces.</span></span> <span data-ttu-id="dfb41-144">如果丢弃的事件数未随缓冲区大小的增加而减少，则可能是因为读取器的速度较慢，导致无法刷新目标进程的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="dfb41-144">If the number of dropped events does not decrease with a larger buffer size, it may be due to a slow reader preventing the target process' buffers from being flushed.</span></span>

- **`--clreventlevel <clreventlevel>`**

  <span data-ttu-id="dfb41-145">要发出的 CLR 事件的详细级别。</span><span class="sxs-lookup"><span data-stu-id="dfb41-145">Verbosity of CLR events to be emitted.</span></span>

- **`--clrevents <clrevents>`**

  <span data-ttu-id="dfb41-146">要启用的 CLR 运行时提供程序关键字列表，以 `+` 符号分隔。</span><span class="sxs-lookup"><span data-stu-id="dfb41-146">A list of CLR runtime provider keywords to enable separated by `+` signs.</span></span> <span data-ttu-id="dfb41-147">这是一个简单映射，支持通过字符串别名而不是其十六进制值指定事件关键字。</span><span class="sxs-lookup"><span data-stu-id="dfb41-147">This is a simple mapping that lets you specify event keywords via string aliases rather than their hex values.</span></span> <span data-ttu-id="dfb41-148">例如，`dotnet-trace collect --providers Microsoft-Windows-DotNETRuntime:3:4` 请求与 `dotnet-trace collect --clrevents gc+gchandle --clreventlevel informational` 相同的事件集。</span><span class="sxs-lookup"><span data-stu-id="dfb41-148">For example, `dotnet-trace collect --providers Microsoft-Windows-DotNETRuntime:3:4` requests the same set of events as `dotnet-trace collect --clrevents gc+gchandle --clreventlevel informational`.</span></span> <span data-ttu-id="dfb41-149">下表显示可用关键字的列表：</span><span class="sxs-lookup"><span data-stu-id="dfb41-149">The table below shows the list of available keywords:</span></span>

  | <span data-ttu-id="dfb41-150">关键字字符串别名</span><span class="sxs-lookup"><span data-stu-id="dfb41-150">Keyword String Alias</span></span> | <span data-ttu-id="dfb41-151">关键字十六进制值</span><span class="sxs-lookup"><span data-stu-id="dfb41-151">Keyword Hex Value</span></span> |
  | ------------ | ------------------- |
  | `gc` | `0x1` |
  | `gchandle` | `0x2` |
  | `fusion` | `0x4` |
  | `loader` | `0x8` |
  | `jit` | `0x10` |
  | `ngen` | `0x20` |
  | `startenumeration` | `0x40` |
  | `endenumeration` | `0x80` |
  | `security` | `0x400` |
  | `appdomainresourcemanagement` | `0x800` |
  | `jittracing` | `0x1000` |
  | `interop` | `0x2000` |
  | `contention` | `0x4000` |
  | `exception` | `0x8000` |
  | `threading` | `0x10000` |
  | `jittedmethodiltonativemap` | `0x20000` |
  | `overrideandsuppressngenevents` | `0x40000` |
  | `type` | `0x80000` |
  | `gcheapdump` | `0x100000` |
  | `gcsampledobjectallocationhigh` | `0x200000` |
  | `gcheapsurvivalandmovement` | `0x400000` |
  | `gcheapcollect` | `0x800000` |
  | `gcheapandtypenames` | `0x1000000` |
  | `gcsampledobjectallocationlow` | `0x2000000` |
  | `perftrack` | `0x20000000` |
  | `stack` | `0x40000000` |
  | `threadtransfer` | `0x80000000` |
  | `debugger` | `0x100000000` |
  | `monitoring` | `0x200000000` |
  | `codesymbols` | `0x400000000` |
  | `eventsource` | `0x800000000` |
  | `compilation` | `0x1000000000` |
  | `compilationdiagnostic` | `0x2000000000` |
  | `methoddiagnostic` | `0x4000000000` |
  | `typediagnostic` | `0x8000000000` |

  <span data-ttu-id="dfb41-152">有关 CLR 提供程序的详细信息，请参阅 [.NET 运行时提供程序参考文档](../../fundamentals/diagnostics/runtime-events.md)。</span><span class="sxs-lookup"><span data-stu-id="dfb41-152">You can read about the CLR provider more in detail on the [.NET runtime provider reference documentation](../../fundamentals/diagnostics/runtime-events.md).</span></span>

- **`--format {Chromium|NetTrace|Speedscope}`**

  <span data-ttu-id="dfb41-153">设置跟踪文件转换的输出格式。</span><span class="sxs-lookup"><span data-stu-id="dfb41-153">Sets the output format for the trace file conversion.</span></span> <span data-ttu-id="dfb41-154">默认值为 `NetTrace`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-154">The default is `NetTrace`.</span></span>

- **`-n, --name <name>`**

  <span data-ttu-id="dfb41-155">从中收集跟踪的进程的名称。</span><span class="sxs-lookup"><span data-stu-id="dfb41-155">The name of the process to collect the trace from.</span></span>

- **`--diagnostic-port <path-to-port>`**

  <span data-ttu-id="dfb41-156">要创建的诊断端口的名称。</span><span class="sxs-lookup"><span data-stu-id="dfb41-156">The name of the diagnostic port to create.</span></span> <span data-ttu-id="dfb41-157">请参阅[使用诊断端口从应用启动时开始收集跟踪](#use-diagnostic-port-to-collect-a-trace-from-app-startup)，以了解如何使用此选项从应用启动时开始收集跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-157">See [Use diagnostic port to collect a trace from app startup](#use-diagnostic-port-to-collect-a-trace-from-app-startup) to learn how to use this option to collect a trace from app startup.</span></span>

- **`-o|--output <trace-file-path>`**

  <span data-ttu-id="dfb41-158">收集的跟踪数据的输出路径。</span><span class="sxs-lookup"><span data-stu-id="dfb41-158">The output path for the collected trace data.</span></span> <span data-ttu-id="dfb41-159">如果未指定，则默认为 `trace.nettrace`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-159">If not specified, it defaults to `trace.nettrace`.</span></span>

- **`-p|--process-id <PID>`**

  <span data-ttu-id="dfb41-160">从中收集跟踪的进程 ID。</span><span class="sxs-lookup"><span data-stu-id="dfb41-160">The process ID to collect the trace from.</span></span>

- **`--profile <profile-name>`**

  <span data-ttu-id="dfb41-161">一组命名的预定义提供程序配置，允许简明地指定常见跟踪方案。</span><span class="sxs-lookup"><span data-stu-id="dfb41-161">A named pre-defined set of provider configurations that allows common tracing scenarios to be specified succinctly.</span></span> <span data-ttu-id="dfb41-162">可用配置文件如下：</span><span class="sxs-lookup"><span data-stu-id="dfb41-162">The following profiles are available:</span></span>

 | <span data-ttu-id="dfb41-163">配置文件</span><span class="sxs-lookup"><span data-stu-id="dfb41-163">Profile</span></span> | <span data-ttu-id="dfb41-164">说明</span><span class="sxs-lookup"><span data-stu-id="dfb41-164">Description</span></span> |
 |---------|-------------|
 |`cpu-sampling`|<span data-ttu-id="dfb41-165">可用于跟踪 CPU 使用情况和一般 .NET 运行时信息。</span><span class="sxs-lookup"><span data-stu-id="dfb41-165">Useful for tracking CPU usage and general .NET runtime information.</span></span> <span data-ttu-id="dfb41-166">如果未指定配置文件或提供程序，则这是默认选项。</span><span class="sxs-lookup"><span data-stu-id="dfb41-166">This is the default option if no profile or providers are specified.</span></span>|
 |`gc-verbose`|<span data-ttu-id="dfb41-167">跟踪 GC 集合并示例对象分配。</span><span class="sxs-lookup"><span data-stu-id="dfb41-167">Tracks GC collections and samples object allocations.</span></span>|
 |`gc-collect`|<span data-ttu-id="dfb41-168">仅以极低的开销跟踪 GC 集合。</span><span class="sxs-lookup"><span data-stu-id="dfb41-168">Tracks GC collections only at very low overhead.</span></span>|

- **`--providers <list-of-comma-separated-providers>`**

  <span data-ttu-id="dfb41-169">要启用的 `EventPipe` 提供程序的以逗号分隔的列表。</span><span class="sxs-lookup"><span data-stu-id="dfb41-169">A comma-separated list of `EventPipe` providers to be enabled.</span></span> <span data-ttu-id="dfb41-170">这些提供程序会补充 `--profile <profile-name>` 隐含的任何提供程序。</span><span class="sxs-lookup"><span data-stu-id="dfb41-170">These providers supplement any providers implied by `--profile <profile-name>`.</span></span> <span data-ttu-id="dfb41-171">如果特定提供程序存在任何不一致的情况，此配置将优先于配置文件中的隐式配置。</span><span class="sxs-lookup"><span data-stu-id="dfb41-171">If there's any inconsistency for a particular provider, this configuration takes precedence over the implicit configuration from the profile.</span></span>

  <span data-ttu-id="dfb41-172">此提供程序列表的格式为：</span><span class="sxs-lookup"><span data-stu-id="dfb41-172">This list of providers is in the form:</span></span>

  - `Provider[,Provider]`
  - <span data-ttu-id="dfb41-173">`Provider` 的格式为：`KnownProviderName[:Flags[:Level][:KeyValueArgs]]`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-173">`Provider` is in the form: `KnownProviderName[:Flags[:Level][:KeyValueArgs]]`.</span></span>
  - <span data-ttu-id="dfb41-174">`KeyValueArgs` 的格式为：`[key1=value1][;key2=value2]`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-174">`KeyValueArgs` is in the form: `[key1=value1][;key2=value2]`.</span></span>

  <span data-ttu-id="dfb41-175">若要详细了解 .NET 中的一些已知提供程序，请参阅[已知事件提供程序](./well-known-event-providers.md)。</span><span class="sxs-lookup"><span data-stu-id="dfb41-175">To learn more about some of the well-known providers in .NET, refer to [Well-known Event Providers](./well-known-event-providers.md).</span></span>

- <span data-ttu-id="dfb41-176">`-- <command>`（仅适用于运行 .NET 5.0 的目标应用程序）</span><span class="sxs-lookup"><span data-stu-id="dfb41-176">**`-- <command>` (for target applications running .NET 5.0 only)**</span></span>

  <span data-ttu-id="dfb41-177">在集合配置参数之后，用户可以追加 `--`，后跟一个命令，以启动至少具有 5.0 运行时的 .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="dfb41-177">After the collection configuration parameters, the user can append `--` followed by a command to start a .NET application with at least a 5.0 runtime.</span></span> <span data-ttu-id="dfb41-178">这在过程早期发生诊断问题（如启动性能问题或程序集加载程序和绑定器错误）时可能会有所帮助。</span><span class="sxs-lookup"><span data-stu-id="dfb41-178">This may be helpful when diagnosing issues that happen early in the process, such as startup performance issue or assembly loader and binder errors.</span></span>

  > [!NOTE]
  > <span data-ttu-id="dfb41-179">使用此选项监视第一个 .NET 5.0 进程，该进程与该工具通信，这意味着如果命令启动多个 .NET 应用程序，它将仅收集第一个应用。</span><span class="sxs-lookup"><span data-stu-id="dfb41-179">Using this option monitors the first .NET 5.0 process that communicates back to the tool, which means if your command launches multiple .NET applications, it will only collect the first app.</span></span> <span data-ttu-id="dfb41-180">因此，建议在自包含应用程序上使用此选项，或使用 `dotnet exec <app.dll>` 选项。</span><span class="sxs-lookup"><span data-stu-id="dfb41-180">Therefore, it is recommended you use this option on self-contained applications, or using the `dotnet exec <app.dll>` option.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-181">对于大型应用程序，停止跟踪可能需要较长时间（可达数分钟）。</span><span class="sxs-lookup"><span data-stu-id="dfb41-181">Stopping the trace may take a long time (up to minutes) for large applications.</span></span> <span data-ttu-id="dfb41-182">运行时需要为跟踪中捕获的所有托管代码发送类型缓存。</span><span class="sxs-lookup"><span data-stu-id="dfb41-182">The runtime needs to send over the type cache for all managed code that was captured in the trace.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-183">在 Linux 和 macOS 上，此命令需要目标应用程序和 `dotnet-trace` 使用同一 `TMPDIR` 环境变量。</span><span class="sxs-lookup"><span data-stu-id="dfb41-183">On Linux and macOS, this command expects the target application and `dotnet-trace` to share the same `TMPDIR` environment variable.</span></span> <span data-ttu-id="dfb41-184">否则，该命令将超时。</span><span class="sxs-lookup"><span data-stu-id="dfb41-184">Otherwise, the command will time out.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-185">若要使用 `dotnet-trace` 收集跟踪，需要以与运行目标进程的用户相同的用户身份或以根身份运行。</span><span class="sxs-lookup"><span data-stu-id="dfb41-185">To collect a trace using `dotnet-trace`, it needs to be run as the same user as the user running target process or as root.</span></span> <span data-ttu-id="dfb41-186">否则，该工具将无法与目标进程建立连接。</span><span class="sxs-lookup"><span data-stu-id="dfb41-186">Otherwise, the tool will fail to establish a connection with the target process.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-187">如果你看到一条类似于以下内容的错误消息：`[ERROR] System.ComponentModel.Win32Exception (299): A 32 bit processes cannot access modules of a 64 bit process.`，你正在尝试使用的 `dotnet-trace` 存在与目标进程不一致的位数。</span><span class="sxs-lookup"><span data-stu-id="dfb41-187">If you see an error message similar to the following one: `[ERROR] System.ComponentModel.Win32Exception (299): A 32 bit processes cannot access modules of a 64 bit process.`, you are trying to use `dotnet-trace` that has mismatched bitness against the target process.</span></span> <span data-ttu-id="dfb41-188">请务必在[安装](#install)链接中下载工具的正确位数。</span><span class="sxs-lookup"><span data-stu-id="dfb41-188">Make sure to download the correct bitness of the tool in the [install](#install) link.</span></span>

## <a name="dotnet-trace-convert"></a><span data-ttu-id="dfb41-189">dotnet-trace convert</span><span class="sxs-lookup"><span data-stu-id="dfb41-189">dotnet-trace convert</span></span>

<span data-ttu-id="dfb41-190">将 `nettrace` 跟踪转换为备用格式，以便用于备用跟踪分析工具。</span><span class="sxs-lookup"><span data-stu-id="dfb41-190">Converts `nettrace` traces to alternate formats for use with alternate trace analysis tools.</span></span>

### <a name="synopsis"></a><span data-ttu-id="dfb41-191">摘要</span><span class="sxs-lookup"><span data-stu-id="dfb41-191">Synopsis</span></span>

```console
dotnet-trace convert [<input-filename>] [--format <Chromium|NetTrace|Speedscope>] [-h|--help] [-o|--output <output-filename>]
```

### <a name="arguments"></a><span data-ttu-id="dfb41-192">自变量</span><span class="sxs-lookup"><span data-stu-id="dfb41-192">Arguments</span></span>

- **`<input-filename>`**

  <span data-ttu-id="dfb41-193">要转换的输入跟踪文件。</span><span class="sxs-lookup"><span data-stu-id="dfb41-193">Input trace file to be converted.</span></span> <span data-ttu-id="dfb41-194">默认为 trace.nettrace  。</span><span class="sxs-lookup"><span data-stu-id="dfb41-194">Defaults to *trace.nettrace*.</span></span>

### <a name="options"></a><span data-ttu-id="dfb41-195">选项</span><span class="sxs-lookup"><span data-stu-id="dfb41-195">Options</span></span>

- **`--format <Chromium|NetTrace|Speedscope>`**

  <span data-ttu-id="dfb41-196">设置跟踪文件转换的输出格式。</span><span class="sxs-lookup"><span data-stu-id="dfb41-196">Sets the output format for the trace file conversion.</span></span>

- **`-o|--output <output-filename>`**

  <span data-ttu-id="dfb41-197">输出文件名。</span><span class="sxs-lookup"><span data-stu-id="dfb41-197">Output filename.</span></span> <span data-ttu-id="dfb41-198">将添加目标格式的扩展。</span><span class="sxs-lookup"><span data-stu-id="dfb41-198">Extension of target format will be added.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-199">将 `nettrace` 文件转换为 `chromium` 或 `speedscope` 文件是不可逆操作。</span><span class="sxs-lookup"><span data-stu-id="dfb41-199">Converting `nettrace` files to `chromium` or `speedscope` files is irreversible.</span></span> <span data-ttu-id="dfb41-200">`speedscope` 和 `chromium` 文件不具备重新构造 `nettrace` 文件所需的全部信息。</span><span class="sxs-lookup"><span data-stu-id="dfb41-200">`speedscope` and `chromium` files don't have all the information necessary to reconstruct `nettrace` files.</span></span> <span data-ttu-id="dfb41-201">但是，`convert` 命令保留了原始 `nettrace` 文件，因此，如果打算将来打开该文件，请不要将其删除。</span><span class="sxs-lookup"><span data-stu-id="dfb41-201">However, the `convert` command preserves the original `nettrace` file, so don't delete this file if you plan to open it in the future.</span></span>

## <a name="dotnet-trace-ps"></a><span data-ttu-id="dfb41-202">dotnet-trace ps</span><span class="sxs-lookup"><span data-stu-id="dfb41-202">dotnet-trace ps</span></span>

 <span data-ttu-id="dfb41-203">列出可从中收集跟踪的 dotnet 进程。</span><span class="sxs-lookup"><span data-stu-id="dfb41-203">Lists the dotnet processes that traces can be collected from.</span></span>

### <a name="synopsis"></a><span data-ttu-id="dfb41-204">摘要</span><span class="sxs-lookup"><span data-stu-id="dfb41-204">Synopsis</span></span>

```console
dotnet-trace ps [-h|--help]
```

## <a name="dotnet-trace-list-profiles"></a><span data-ttu-id="dfb41-205">dotnet-trace list-profiles</span><span class="sxs-lookup"><span data-stu-id="dfb41-205">dotnet-trace list-profiles</span></span>

<span data-ttu-id="dfb41-206">列出预生成的跟踪配置文件，并描述每个配置文件中包含的提供程序和筛选器。</span><span class="sxs-lookup"><span data-stu-id="dfb41-206">Lists pre-built tracing profiles with a description of what providers and filters are in each profile.</span></span>

### <a name="synopsis"></a><span data-ttu-id="dfb41-207">摘要</span><span class="sxs-lookup"><span data-stu-id="dfb41-207">Synopsis</span></span>

```console
dotnet-trace list-profiles [-h|--help]
```

## <a name="collect-a-trace-with-dotnet-trace"></a><span data-ttu-id="dfb41-208">使用 dotnet-trace 收集跟踪</span><span class="sxs-lookup"><span data-stu-id="dfb41-208">Collect a trace with dotnet-trace</span></span>

<span data-ttu-id="dfb41-209">若要使用 `dotnet-trace` 收集跟踪，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="dfb41-209">To collect traces using `dotnet-trace`:</span></span>

- <span data-ttu-id="dfb41-210">需要首先查找要从中收集跟踪的 .NET Core 应用程序的进程标识符 (PID)。</span><span class="sxs-lookup"><span data-stu-id="dfb41-210">Get the process identifier (PID) of the .NET Core application to collect traces from.</span></span>

  - <span data-ttu-id="dfb41-211">例如，在 Windows 上，可以使用任务管理器或 `tasklist` 命令。</span><span class="sxs-lookup"><span data-stu-id="dfb41-211">On Windows, you can use Task Manager or the `tasklist` command, for example.</span></span>
  - <span data-ttu-id="dfb41-212">在 Linux 上，使用 `ps` 命令。</span><span class="sxs-lookup"><span data-stu-id="dfb41-212">On Linux, for example, the `ps` command.</span></span>
  - [<span data-ttu-id="dfb41-213">dotnet-trace ps</span><span class="sxs-lookup"><span data-stu-id="dfb41-213">dotnet-trace ps</span></span>](#dotnet-trace-ps)

- <span data-ttu-id="dfb41-214">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="dfb41-214">Run the following command:</span></span>

  ```console
  dotnet-trace collect --process-id <PID>
  ```

  <span data-ttu-id="dfb41-215">前面的命令生成类似于以下内容的输出：</span><span class="sxs-lookup"><span data-stu-id="dfb41-215">The preceding command generates output similar to the following:</span></span>

  ```console
  Press <Enter> to exit...
  Connecting to process: <Full-Path-To-Process-Being-Profiled>/dotnet.exe
  Collecting to file: <Full-Path-To-Trace>/trace.nettrace
  Session Id: <SessionId>
  Recording trace 721.025 (KB)
  ```

- <span data-ttu-id="dfb41-216">按 `<Enter>` 键停止收集。</span><span class="sxs-lookup"><span data-stu-id="dfb41-216">Stop collection by pressing the `<Enter>` key.</span></span> <span data-ttu-id="dfb41-217">`dotnet-trace` 会将完成将事件记录到 trace.nettrace 文件中  。</span><span class="sxs-lookup"><span data-stu-id="dfb41-217">`dotnet-trace` will finish logging events to the *trace.nettrace* file.</span></span>

## <a name="launch-a-child-application-and-collect-a-trace-from-its-startup-using-dotnet-trace"></a><span data-ttu-id="dfb41-218">启动子应用程序，并使用 dotnet-trace 从启动中收集跟踪</span><span class="sxs-lookup"><span data-stu-id="dfb41-218">Launch a child application and collect a trace from its startup using dotnet-trace</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dfb41-219">这仅适用于运行 .NET 5.0 或更高版本的应用。</span><span class="sxs-lookup"><span data-stu-id="dfb41-219">This works for apps running .NET 5.0 or later only.</span></span>

<span data-ttu-id="dfb41-220">有时，从进程启动中收集进程的跟踪可能很有用。</span><span class="sxs-lookup"><span data-stu-id="dfb41-220">Sometimes it may be useful to collect a trace of a process from its startup.</span></span> <span data-ttu-id="dfb41-221">对于运行 .NET 5.0 或更高版本的应用，可以使用 dotnet-trace 来做到这一点。</span><span class="sxs-lookup"><span data-stu-id="dfb41-221">For apps running .NET 5.0 or later, it is possible to do this by using dotnet-trace.</span></span>

<span data-ttu-id="dfb41-222">这将启动 `hello.exe` 并以 `arg1` 和 `arg2` 作为其命令行参数，从其运行时启动中收集跟踪：</span><span class="sxs-lookup"><span data-stu-id="dfb41-222">This will launch `hello.exe` with `arg1` and `arg2` as its command-line arguments and collect a trace from its runtime startup:</span></span>

```console
dotnet-trace collect -- hello.exe arg1 arg2
```

<span data-ttu-id="dfb41-223">前面的命令生成类似于以下内容的输出：</span><span class="sxs-lookup"><span data-stu-id="dfb41-223">The preceding command generates output similar to the following:</span></span>

```console
No profile or providers specified, defaulting to trace profile 'cpu-sampling'

Provider Name                           Keywords            Level               Enabled By
Microsoft-DotNETCore-SampleProfiler     0x0000F00000000000  Informational(4)    --profile
Microsoft-Windows-DotNETRuntime         0x00000014C14FCCBD  Informational(4)    --profile

Process        : E:\temp\gcperfsim\bin\Debug\net5.0\gcperfsim.exe
Output File    : E:\temp\gcperfsim\trace.nettrace


[00:00:00:05]   Recording trace 122.244  (KB)
Press <Enter> or <Ctrl+C> to exit...
```

<span data-ttu-id="dfb41-224">可以通过按 `<Enter>` 或 `<Ctrl + C>` 键来停止收集跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-224">You can stop collecting the trace by pressing `<Enter>` or `<Ctrl + C>` key.</span></span> <span data-ttu-id="dfb41-225">此操作还将退出 `hello.exe`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-225">Doing this will also exit `hello.exe`.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-226">通过 dotnet-trace 启动 `hello.exe` 将重定向其输入/输出，你将无法与其 stdin/stdout 进行交互。</span><span class="sxs-lookup"><span data-stu-id="dfb41-226">Launching `hello.exe` via dotnet-trace will make its input/output to be redirected and you won't be able to interact with its stdin/stdout.</span></span>
> <span data-ttu-id="dfb41-227">通过 CTRL+C 或 SIGTERM 退出工具将安全地结束该工具和子进程。</span><span class="sxs-lookup"><span data-stu-id="dfb41-227">Exiting the tool via CTRL+C or SIGTERM will safely end both the tool and the child process.</span></span>
> <span data-ttu-id="dfb41-228">如果子进程在工具之前退出，工具也将退出，应可安全查看跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-228">If the child process exits before the tool, the tool will exit as well and the trace should be safely viewable.</span></span>

## <a name="use-diagnostic-port-to-collect-a-trace-from-app-startup"></a><span data-ttu-id="dfb41-229">使用诊断端口从应用启动时开始收集跟踪</span><span class="sxs-lookup"><span data-stu-id="dfb41-229">Use diagnostic port to collect a trace from app startup</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="dfb41-230">这仅适用于运行 .NET 5.0 或更高版本的应用。</span><span class="sxs-lookup"><span data-stu-id="dfb41-230">This works for apps running .NET 5.0 or later only.</span></span>

<span data-ttu-id="dfb41-231">诊断端口是 .NET 5 中新增的运行时功能，你可以通过它从应用启动时开始跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-231">Diagnostic port is a new runtime feature that was added in .NET 5 that allows you to start tracing from app startup.</span></span> <span data-ttu-id="dfb41-232">若要使用 `dotnet-trace` 执行此操作，可以使用以上示例中所述的 `dotnet-trace collect -- <command>`，也可以使用 `--diagnostic-port` 选项。</span><span class="sxs-lookup"><span data-stu-id="dfb41-232">To do this using `dotnet-trace`, you can either use `dotnet-trace collect -- <command>` as described in the examples above, or use the `--diagnostic-port` option.</span></span>

<span data-ttu-id="dfb41-233">使用 `dotnet-trace <collect|monitor> -- <command>` 以子进程的形式启动应用程序，是从启动时开始对其进行快速跟踪的最简单方法。</span><span class="sxs-lookup"><span data-stu-id="dfb41-233">Using `dotnet-trace <collect|monitor> -- <command>` to launch the application as a child process is the simplest way to quickly trace it from its startup.</span></span>

<span data-ttu-id="dfb41-234">但是，如果想要更好地控制所跟踪应用的生存期（例如，仅在前 10 分钟内监视应用并继续执行），或者如果需要使用 CLI 与应用进行交互，则使用 `--diagnostic-port` 选项可以同时控制要监视的目标应用和 `dotnet-trace`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-234">However, when you want to gain a finer control over the lifetime of the app being traced (for example, monitor the app for the first 10 minutes only and continue executing) or if you need to interact with the app using the CLI, using `--diagnostic-port` option allows you to control both the target app being monitored and `dotnet-trace`.</span></span>

1. <span data-ttu-id="dfb41-235">以下命令使 `dotnet-trace` 创建一个名为 `myport.sock` 的诊断套接字并等待连接。</span><span class="sxs-lookup"><span data-stu-id="dfb41-235">The command below makes `dotnet-trace` create a diagnostics socket named `myport.sock` and wait for a connection.</span></span>

    > ```dotnet-cli
    > dotnet-trace collect --diagnostic-port myport.sock
    > ```

    <span data-ttu-id="dfb41-236">输出：</span><span class="sxs-lookup"><span data-stu-id="dfb41-236">Output:</span></span>

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ```

2. <span data-ttu-id="dfb41-237">在单独的控制台中，通过将环境变量 `DOTNET_DiagnosticPorts` 设置为 `dotnet-trace` 输出中的值，启动目标应用程序。</span><span class="sxs-lookup"><span data-stu-id="dfb41-237">In a separate console, launch the target application with the environment variable `DOTNET_DiagnosticPorts` set to the value in the `dotnet-trace` output.</span></span>

    > ```bash
    > export DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ./my-dotnet-app arg1 arg2
    > ```

    <span data-ttu-id="dfb41-238">这应该会使 `dotnet-trace` 开始跟踪 `my-dotnet-app`：</span><span class="sxs-lookup"><span data-stu-id="dfb41-238">This should then enable `dotnet-trace` to start tracing `my-dotnet-app`:</span></span>

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=myport.sock
    > Starting a counter session. Press Q to quit.
    > ```

    > [!IMPORTANT]
    > <span data-ttu-id="dfb41-239">通过 `dotnet run` 启动应用可能会产生问题，因为 dotnet CLI 可能会生成许多子进程，这些子程序不是应用，并且可以在应用之前连接到 `dotnet-trace`，从而导致应用在运行时挂起。</span><span class="sxs-lookup"><span data-stu-id="dfb41-239">Launching your app with `dotnet run` can be problematic because the dotnet CLI may spawn many child processes that are not your app and they can connect to `dotnet-trace` before your app, leaving your app to be suspended at runtime.</span></span> <span data-ttu-id="dfb41-240">建议直接使用应用的独立版本或使用 `dotnet exec` 来启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="dfb41-240">It is recommended you directly use a self-contained version of the app or use `dotnet exec` to launch the application.</span></span>

## <a name="view-the-trace-captured-from-dotnet-trace"></a><span data-ttu-id="dfb41-241">查看由 dotnet-trace 捕获的跟踪</span><span class="sxs-lookup"><span data-stu-id="dfb41-241">View the trace captured from dotnet-trace</span></span>

<span data-ttu-id="dfb41-242">在 Windows 上，可以使用 [PerfView](https://github.com/microsoft/perfview) 查看 .nettrace 文件以进行分析  ：对于其他平台上收集的跟踪，可以将跟踪文件移动到 Windows 计算机上，以在 PerfView 上进行查看。</span><span class="sxs-lookup"><span data-stu-id="dfb41-242">On Windows, *.nettrace* files can be viewed on [PerfView](https://github.com/microsoft/perfview) for analysis: For traces collected on other platforms, the trace file can be moved to a Windows machine to be viewed on PerfView.</span></span>

<span data-ttu-id="dfb41-243">在 Linux 上，可以通过将 `dotnet-trace` 的输出格式更改为 `speedscope` 来查看跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-243">On Linux, the trace can be viewed by changing the output format of `dotnet-trace` to `speedscope`.</span></span> <span data-ttu-id="dfb41-244">可以使用 `-f|--format` 选项更改输出文件格式 - `-f speedscope` 会使 `dotnet-trace` 生成 `speedscope` 文件。</span><span class="sxs-lookup"><span data-stu-id="dfb41-244">The output file format can be changed using the `-f|--format` option - `-f speedscope` will make `dotnet-trace` produce a `speedscope` file.</span></span> <span data-ttu-id="dfb41-245">可以在 `nettrace`（默认选项）和 `speedscope` 之间进行选择。</span><span class="sxs-lookup"><span data-stu-id="dfb41-245">You can choose between `nettrace` (the default option) and `speedscope`.</span></span> <span data-ttu-id="dfb41-246">可以在 <https://www.speedscope.app> 打开 `Speedscope` 文件。</span><span class="sxs-lookup"><span data-stu-id="dfb41-246">`Speedscope` files can be opened at <https://www.speedscope.app>.</span></span>

> [!NOTE]
> <span data-ttu-id="dfb41-247">.NET Core 运行时以 `nettrace` 格式生成跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-247">The .NET Core runtime generates traces in the `nettrace` format.</span></span> <span data-ttu-id="dfb41-248">跟踪完成后，跟踪将转换为 speedscope（如果指定）。</span><span class="sxs-lookup"><span data-stu-id="dfb41-248">The traces are converted to speedscope (if specified) after the trace is completed.</span></span> <span data-ttu-id="dfb41-249">由于某些转换可能会导致数据丢失，因此，原始 `nettrace` 文件将保留在转换后的文件旁边。</span><span class="sxs-lookup"><span data-stu-id="dfb41-249">Since some conversions may result in loss of data, the original `nettrace` file is preserved next to the converted file.</span></span>

## <a name="use-dotnet-trace-to-collect-counter-values-over-time"></a><span data-ttu-id="dfb41-250">使用 dotnet-trace 收集随时间变化的计数器值</span><span class="sxs-lookup"><span data-stu-id="dfb41-250">Use dotnet-trace to collect counter values over time</span></span>

<span data-ttu-id="dfb41-251">`dotnet-trace` 可以：</span><span class="sxs-lookup"><span data-stu-id="dfb41-251">`dotnet-trace` can:</span></span>

* <span data-ttu-id="dfb41-252">在性能敏感的环境中使用 `EventCounter` 进行基本运行状况监视。</span><span class="sxs-lookup"><span data-stu-id="dfb41-252">Use `EventCounter` for basic health monitoring in performance-sensitive environments.</span></span> <span data-ttu-id="dfb41-253">例如，在生产环境中。</span><span class="sxs-lookup"><span data-stu-id="dfb41-253">For example, in production.</span></span>
* <span data-ttu-id="dfb41-254">收集跟踪，这样就不需要实时查看。</span><span class="sxs-lookup"><span data-stu-id="dfb41-254">Collect traces so they don't need to be viewed in real time.</span></span>

<span data-ttu-id="dfb41-255">例如，若要收集运行时性能计数器值，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="dfb41-255">For example, to collect runtime performance counter values, use the following command:</span></span>

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1
```

<span data-ttu-id="dfb41-256">上述命令通知运行时计数器每秒报告一次，以便进行轻量型运行状况监视。</span><span class="sxs-lookup"><span data-stu-id="dfb41-256">The preceding command tells the runtime counters to report once every second for lightweight health monitoring.</span></span> <span data-ttu-id="dfb41-257">通过使用较大的值（例如 60）替换 `EventCounterIntervalSec=1`，可以收集计数器数据中粒度较小的跟踪。</span><span class="sxs-lookup"><span data-stu-id="dfb41-257">Replacing `EventCounterIntervalSec=1` with a higher value (for example, 60) allows collection of a smaller trace with less granularity in the counter data.</span></span>

<span data-ttu-id="dfb41-258">以下命令比以上命令产生更小的开销和跟踪大小：</span><span class="sxs-lookup"><span data-stu-id="dfb41-258">The following command reduces overhead and trace size more than the preceding one:</span></span>

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1,Microsoft-Windows-DotNETRuntime:0:1,Microsoft-DotNETCore-SampleProfiler:0:1
```

<span data-ttu-id="dfb41-259">以上命令会禁用运行时事件和托管堆栈探查器。</span><span class="sxs-lookup"><span data-stu-id="dfb41-259">The preceding command disables runtime events and the managed stack profiler.</span></span>

## <a name="use-rsp-file-to-avoid-typing-long-commands"></a><span data-ttu-id="dfb41-260">使用 .rsp 文件来避免键入长命令</span><span class="sxs-lookup"><span data-stu-id="dfb41-260">Use .rsp file to avoid typing long commands</span></span>

<span data-ttu-id="dfb41-261">可以使用包含要传递的参数的 `.rsp` 文件启动 `dotnet-trace`。</span><span class="sxs-lookup"><span data-stu-id="dfb41-261">You can launch `dotnet-trace` with an `.rsp` file that contains the arguments to pass.</span></span> <span data-ttu-id="dfb41-262">当启用需要较长参数的提供程序时，或在使用可去除字符的 shell 环境时，这很有用。</span><span class="sxs-lookup"><span data-stu-id="dfb41-262">This can be useful when enabling providers that expect lengthy arguments or when using a shell environment that strips characters.</span></span>

<span data-ttu-id="dfb41-263">例如，以下提供程序在每次要跟踪时都可能要繁琐地键入内容：</span><span class="sxs-lookup"><span data-stu-id="dfb41-263">For example, the following provider can be cumbersome to type out each time you want to trace:</span></span>

```cmd
dotnet-trace collect --providers Microsoft-Diagnostics-DiagnosticSource:0x3:5:FilterAndPayloadSpecs="SqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandBefore@Activity1Start:-Command;Command.CommandText;ConnectionId;Operation;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nSqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandAfter@Activity1Stop:\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuting@Activity2Start:-Command;Command.CommandText;ConnectionId;IsAsync;Command.Connection.ClientConnectionId;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuted@Activity2Stop:",OtherProvider,AnotherProvider
```

<span data-ttu-id="dfb41-264">此外，前一个示例包含 `"` 作为参数的一部分。</span><span class="sxs-lookup"><span data-stu-id="dfb41-264">In addition, the previous example contains `"` as part of the argument.</span></span> <span data-ttu-id="dfb41-265">由于每个 shell 对引号的处理不同，因此在使用不同的 shell 时可能会遇到各种问题。</span><span class="sxs-lookup"><span data-stu-id="dfb41-265">Because quotes are not handled equally by each shell, you may experience various issues when using different shells.</span></span> <span data-ttu-id="dfb41-266">例如，在 `zsh` 中输入的命令与 `cmd` 中的命令不同。</span><span class="sxs-lookup"><span data-stu-id="dfb41-266">For example, the command to enter in `zsh` is different to the command in `cmd`.</span></span>

<span data-ttu-id="dfb41-267">可以将以下文本保存到名为 `myprofile.rsp` 的文件中，而不必每次都键入此内容。</span><span class="sxs-lookup"><span data-stu-id="dfb41-267">Instead of typing this each time, you can save the following text into a file called `myprofile.rsp`.</span></span>

```txt
--providers
Microsoft-Diagnostics-DiagnosticSource:0x3:5:FilterAndPayloadSpecs="SqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandBefore@Activity1Start:-Command;Command.CommandText;ConnectionId;Operation;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nSqlClientDiagnosticListener/System.Data.SqlClient.WriteCommandAfter@Activity1Stop:\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuting@Activity2Start:-Command;Command.CommandText;ConnectionId;IsAsync;Command.Connection.ClientConnectionId;Command.Connection.ServerVersion;Command.CommandTimeout;Command.CommandType;Command.Connection.ConnectionString;Command.Connection.Database;Command.Connection.DataSource;Command.Connection.PacketSize\r\nMicrosoft.EntityFrameworkCore/Microsoft.EntityFrameworkCore.Database.Command.CommandExecuted@Activity2Stop:",OtherProvider,AnotherProvider
```

<span data-ttu-id="dfb41-268">保存 `myprofile.rsp` 后，可以使用以下命令通过此配置启动 `dotnet-trace`：</span><span class="sxs-lookup"><span data-stu-id="dfb41-268">Once you've saved `myprofile.rsp`, you can launch `dotnet-trace` with this configuration using the following command:</span></span>

```bash
dotnet-trace @myprofile.rsp
```

## <a name="see-also"></a><span data-ttu-id="dfb41-269">另请参阅</span><span class="sxs-lookup"><span data-stu-id="dfb41-269">See also</span></span>

- [<span data-ttu-id="dfb41-270">.NET 的已知事件提供程序</span><span class="sxs-lookup"><span data-stu-id="dfb41-270">Well-known event providers from .NET</span></span>](well-known-event-providers.md)
