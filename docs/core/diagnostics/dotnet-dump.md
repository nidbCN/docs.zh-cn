---
title: dotnet-dump 诊断工具 - .NET CLI
description: 了解如何安装和使用 dotnet-dump CLI 工具，以在没有任何本机调试器的情况下收集和分析 Windows 和 Linux 转储。
ms.date: 11/17/2020
ms.openlocfilehash: 6e9edbc8075691784028f3293750c92fe53d3782
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874168"
---
# <a name="dump-collection-and-analysis-utility-dotnet-dump"></a><span data-ttu-id="989dc-103">转储收集和分析实用工具 (dotnet-dump)</span><span class="sxs-lookup"><span data-stu-id="989dc-103">Dump collection and analysis utility (dotnet-dump)</span></span>

<span data-ttu-id="989dc-104"> 本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="989dc-104">**This article applies to:** ✔️ .NET Core 3.0 SDK and later versions</span></span>

> [!NOTE]
> <span data-ttu-id="989dc-105">macOS 的 `dotnet-dump` 仅在 .NET 5.0 及更高版本中受支持。</span><span class="sxs-lookup"><span data-stu-id="989dc-105">`dotnet-dump` for macOS is only supported with .NET 5.0 and later versions.</span></span>

## <a name="install"></a><span data-ttu-id="989dc-106">安装</span><span class="sxs-lookup"><span data-stu-id="989dc-106">Install</span></span>

<span data-ttu-id="989dc-107">可采用两种方法来下载和安装 `dotnet-dump`：</span><span class="sxs-lookup"><span data-stu-id="989dc-107">There are two ways to download and install `dotnet-dump`:</span></span>

- <span data-ttu-id="989dc-108">**dotnet 全局工具：**</span><span class="sxs-lookup"><span data-stu-id="989dc-108">**dotnet global tool:**</span></span>

  <span data-ttu-id="989dc-109">若要安装最新版 `dotnet-dump` [NuGet 包](https://www.nuget.org/packages/dotnet-dump)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="989dc-109">To install the latest release version of the `dotnet-dump` [NuGet package](https://www.nuget.org/packages/dotnet-dump), use the [dotnet tool install](../tools/dotnet-tool-install.md) command:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-dump
  ```

- <span data-ttu-id="989dc-110">**直接下载：**</span><span class="sxs-lookup"><span data-stu-id="989dc-110">**Direct download:**</span></span>

  <span data-ttu-id="989dc-111">下载与平台相匹配的工具可执行文件：</span><span class="sxs-lookup"><span data-stu-id="989dc-111">Download the tool executable that matches your platform:</span></span>

  | <span data-ttu-id="989dc-112">(OS)</span><span class="sxs-lookup"><span data-stu-id="989dc-112">OS</span></span>  | <span data-ttu-id="989dc-113">平台</span><span class="sxs-lookup"><span data-stu-id="989dc-113">Platform</span></span> |
  | --- | -------- |
  | <span data-ttu-id="989dc-114">Windows</span><span class="sxs-lookup"><span data-stu-id="989dc-114">Windows</span></span> | <span data-ttu-id="989dc-115">[x86](https://aka.ms/dotnet-dump/win-x86) \| [x64](https://aka.ms/dotnet-dump/win-x64) \| [arm](https://aka.ms/dotnet-dump/win-arm) \| [arm-x64](https://aka.ms/dotnet-dump/win-arm64)</span><span class="sxs-lookup"><span data-stu-id="989dc-115">[x86](https://aka.ms/dotnet-dump/win-x86) \| [x64](https://aka.ms/dotnet-dump/win-x64) \| [arm](https://aka.ms/dotnet-dump/win-arm) \| [arm-x64](https://aka.ms/dotnet-dump/win-arm64)</span></span> |
  | <span data-ttu-id="989dc-116">macOS</span><span class="sxs-lookup"><span data-stu-id="989dc-116">macOS</span></span>   | [<span data-ttu-id="989dc-117">x64</span><span class="sxs-lookup"><span data-stu-id="989dc-117">x64</span></span>](https://aka.ms/dotnet-dump/osx-x64) |
  | <span data-ttu-id="989dc-118">Linux</span><span class="sxs-lookup"><span data-stu-id="989dc-118">Linux</span></span>   | <span data-ttu-id="989dc-119">[x64](https://aka.ms/dotnet-dump/linux-x64) \| [arm](https://aka.ms/dotnet-dump/linux-arm) \| [arm64](https://aka.ms/dotnet-dump/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-dump/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-dump/linux-musl-arm64)</span><span class="sxs-lookup"><span data-stu-id="989dc-119">[x64](https://aka.ms/dotnet-dump/linux-x64) \| [arm](https://aka.ms/dotnet-dump/linux-arm) \| [arm64](https://aka.ms/dotnet-dump/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-dump/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-dump/linux-musl-arm64)</span></span> |

> [!NOTE]
> <span data-ttu-id="989dc-120">若要在 x86 应用上使用 `dotnet-dump`，需要使用相应的 x86 版本的工具。</span><span class="sxs-lookup"><span data-stu-id="989dc-120">To use `dotnet-dump` on an x86 app, you need a corresponding x86 version of the tool.</span></span>

## <a name="synopsis"></a><span data-ttu-id="989dc-121">摘要</span><span class="sxs-lookup"><span data-stu-id="989dc-121">Synopsis</span></span>

```console
dotnet-dump [-h|--help] [--version] <command>
```

## <a name="description"></a><span data-ttu-id="989dc-122">描述</span><span class="sxs-lookup"><span data-stu-id="989dc-122">Description</span></span>

<span data-ttu-id="989dc-123">`dotnet-dump` 全局工具是在未涉及任何本机调试器（如 Linux 上的 `lldb`）的情况下收集和分析 Windows 和 Linux 转储的方法。</span><span class="sxs-lookup"><span data-stu-id="989dc-123">The `dotnet-dump` global tool is a way to collect and analyze Windows and Linux dumps without any native debugger involved like `lldb` on Linux.</span></span> <span data-ttu-id="989dc-124">在 `lldb` 无法正常运行的平台（如 Alpine Linux）上，此工具非常重要。</span><span class="sxs-lookup"><span data-stu-id="989dc-124">This tool is important on platforms like Alpine Linux where a fully working `lldb` isn't available.</span></span> <span data-ttu-id="989dc-125">借助 `dotnet-dump` 工具，可以运行 SOS 命令来分析崩溃和垃圾回收器 (GC)，但它不是本机调试器，因此不支持显示本机堆栈帧之类的操作。</span><span class="sxs-lookup"><span data-stu-id="989dc-125">The `dotnet-dump` tool allows you to run SOS commands to analyze crashes and the garbage collector (GC), but it isn't a native debugger so things like displaying native stack frames aren't supported.</span></span>

## <a name="options"></a><span data-ttu-id="989dc-126">选项</span><span class="sxs-lookup"><span data-stu-id="989dc-126">Options</span></span>

- **`--version`**

  <span data-ttu-id="989dc-127">显示 dotnet-dump 实用工具的版本。</span><span class="sxs-lookup"><span data-stu-id="989dc-127">Displays the version of the dotnet-dump utility.</span></span>

- **`-h|--help`**

  <span data-ttu-id="989dc-128">显示命令行帮助。</span><span class="sxs-lookup"><span data-stu-id="989dc-128">Shows command-line help.</span></span>

## <a name="commands"></a><span data-ttu-id="989dc-129">命令</span><span class="sxs-lookup"><span data-stu-id="989dc-129">Commands</span></span>

| <span data-ttu-id="989dc-130">命令</span><span class="sxs-lookup"><span data-stu-id="989dc-130">Command</span></span>                                     |
| ------------------------------------------- |
| [<span data-ttu-id="989dc-131">dotnet-dump collect</span><span class="sxs-lookup"><span data-stu-id="989dc-131">dotnet-dump collect</span></span>](#dotnet-dump-collect) |
| [<span data-ttu-id="989dc-132">dotnet-dump analyze</span><span class="sxs-lookup"><span data-stu-id="989dc-132">dotnet-dump analyze</span></span>](#dotnet-dump-analyze) |

## <a name="dotnet-dump-collect"></a><span data-ttu-id="989dc-133">dotnet-dump collect</span><span class="sxs-lookup"><span data-stu-id="989dc-133">dotnet-dump collect</span></span>

<span data-ttu-id="989dc-134">从进程捕获转储。</span><span class="sxs-lookup"><span data-stu-id="989dc-134">Captures a dump from a process.</span></span>

### <a name="synopsis"></a><span data-ttu-id="989dc-135">摘要</span><span class="sxs-lookup"><span data-stu-id="989dc-135">Synopsis</span></span>

```console
dotnet-dump collect [-h|--help] [-p|--process-id] [-n|--name] [--type] [-o|--output] [--diag]
```

### <a name="options"></a><span data-ttu-id="989dc-136">选项</span><span class="sxs-lookup"><span data-stu-id="989dc-136">Options</span></span>

- **`-h|--help`**

  <span data-ttu-id="989dc-137">显示命令行帮助。</span><span class="sxs-lookup"><span data-stu-id="989dc-137">Shows command-line help.</span></span>

- **`-p|--process-id <PID>`**

  <span data-ttu-id="989dc-138">指定从中收集转储的进程的 ID 号。</span><span class="sxs-lookup"><span data-stu-id="989dc-138">Specifies the process ID number to collect a dump from.</span></span>

- **`-n|--name <name>`**

  <span data-ttu-id="989dc-139">指定从中收集转储的进程的名称。</span><span class="sxs-lookup"><span data-stu-id="989dc-139">Specifies the name of the process to collect a dump from.</span></span>

- **`--type <Full|Heap|Mini>`**

  <span data-ttu-id="989dc-140">指定转储类型，它确定从进程收集的信息的类型。</span><span class="sxs-lookup"><span data-stu-id="989dc-140">Specifies the dump type, which determines the kinds of information that are collected from the process.</span></span> <span data-ttu-id="989dc-141">有三种类型：</span><span class="sxs-lookup"><span data-stu-id="989dc-141">There are three types:</span></span>

  - <span data-ttu-id="989dc-142">`Full` - 最大的转储，包含所有内存（包括模块映像）。</span><span class="sxs-lookup"><span data-stu-id="989dc-142">`Full` - The largest dump containing all memory including the module images.</span></span>
  - <span data-ttu-id="989dc-143">`Heap` - 大型且相对全面的转储，其中包含模块列表、线程列表、所有堆栈、异常信息、句柄信息和除映射图像以外的所有内存。</span><span class="sxs-lookup"><span data-stu-id="989dc-143">`Heap` - A large and relatively comprehensive dump containing module lists, thread lists, all stacks, exception information, handle information, and all memory except for mapped images.</span></span>
  - <span data-ttu-id="989dc-144">`Mini` - 小型转储，其中包含模块列表、线程列表、异常信息和所有堆栈。</span><span class="sxs-lookup"><span data-stu-id="989dc-144">`Mini` - A small dump containing module lists, thread lists, exception information, and all stacks.</span></span>

  <span data-ttu-id="989dc-145">如果未指定，则 `Full` 为默认类型。</span><span class="sxs-lookup"><span data-stu-id="989dc-145">If not specified, `Full` is the default.</span></span>

- **`-o|--output <output_dump_path>`**

  <span data-ttu-id="989dc-146">应在其中写入收集的转储的完整路径和文件名。</span><span class="sxs-lookup"><span data-stu-id="989dc-146">The full path and file name where the collected dump should be written.</span></span>

  <span data-ttu-id="989dc-147">如果未指定：</span><span class="sxs-lookup"><span data-stu-id="989dc-147">If not specified:</span></span>

  - <span data-ttu-id="989dc-148">在 Windows 上默认为 .\dump_YYYYMMDD_HHMMSS.dmp  。</span><span class="sxs-lookup"><span data-stu-id="989dc-148">Defaults to *.\dump_YYYYMMDD_HHMMSS.dmp* on Windows.</span></span>
  - <span data-ttu-id="989dc-149">在 Linux 上默认为 ./core_YYYYMMDD_HHMMSS  。</span><span class="sxs-lookup"><span data-stu-id="989dc-149">Defaults to *./core_YYYYMMDD_HHMMSS* on Linux.</span></span>

  <span data-ttu-id="989dc-150">YYYYMMDD 为年/月/日，HHMMSS 为小时/分钟/秒。</span><span class="sxs-lookup"><span data-stu-id="989dc-150">YYYYMMDD is Year/Month/Day and HHMMSS is Hour/Minute/Second.</span></span>

- **`--diag`**

  <span data-ttu-id="989dc-151">启用转储收集诊断日志记录。</span><span class="sxs-lookup"><span data-stu-id="989dc-151">Enables dump collection diagnostic logging.</span></span>

> [!NOTE]
> <span data-ttu-id="989dc-152">在 Linux 和 macOS 上，此命令需要目标应用程序和 `dotnet-dump` 使用同一 `TMPDIR` 环境变量。</span><span class="sxs-lookup"><span data-stu-id="989dc-152">On Linux and macOS, this command expects the target application and `dotnet-dump` to share the same `TMPDIR` environment variable.</span></span> <span data-ttu-id="989dc-153">否则，该命令将超时。</span><span class="sxs-lookup"><span data-stu-id="989dc-153">Otherwise, the command will time out.</span></span>

> [!NOTE]
> <span data-ttu-id="989dc-154">若要使用 `dotnet-dump` 收集转储，需要以与运行目标进程的用户相同的用户身份或以根身份运行。</span><span class="sxs-lookup"><span data-stu-id="989dc-154">To collect a dump using `dotnet-dump`, it needs to be run as the same user as the user running target process or as root.</span></span> <span data-ttu-id="989dc-155">否则，该工具将无法与目标进程建立连接。</span><span class="sxs-lookup"><span data-stu-id="989dc-155">Otherwise, the tool will fail to establish a connection with the target process.</span></span>

## <a name="dotnet-dump-analyze"></a><span data-ttu-id="989dc-156">dotnet-dump analyze</span><span class="sxs-lookup"><span data-stu-id="989dc-156">dotnet-dump analyze</span></span>

<span data-ttu-id="989dc-157">启动交互式 shell 以了解转储。</span><span class="sxs-lookup"><span data-stu-id="989dc-157">Starts an interactive shell to explore a dump.</span></span> <span data-ttu-id="989dc-158">shell 接受各种 [SOS 命令](#analyze-sos-commands)。</span><span class="sxs-lookup"><span data-stu-id="989dc-158">The shell accepts various [SOS commands](#analyze-sos-commands).</span></span>

### <a name="synopsis"></a><span data-ttu-id="989dc-159">摘要</span><span class="sxs-lookup"><span data-stu-id="989dc-159">Synopsis</span></span>

```console
dotnet-dump analyze <dump_path> [-h|--help] [-c|--command]
```

### <a name="arguments"></a><span data-ttu-id="989dc-160">自变量</span><span class="sxs-lookup"><span data-stu-id="989dc-160">Arguments</span></span>

- **`<dump_path>`**

  <span data-ttu-id="989dc-161">指定要分析的转储文件的路径。</span><span class="sxs-lookup"><span data-stu-id="989dc-161">Specifies the path to the dump file to analyze.</span></span>

### <a name="options"></a><span data-ttu-id="989dc-162">选项</span><span class="sxs-lookup"><span data-stu-id="989dc-162">Options</span></span>

- **`-c|--command <debug_command>`**

  <span data-ttu-id="989dc-163">指定要在启动时在 shell 中运行的[命令](#analyze-sos-commands)。</span><span class="sxs-lookup"><span data-stu-id="989dc-163">Specifies the [command](#analyze-sos-commands) to run in the shell on start.</span></span>

### <a name="analyze-sos-commands"></a><span data-ttu-id="989dc-164">分析 SOS 命令</span><span class="sxs-lookup"><span data-stu-id="989dc-164">Analyze SOS commands</span></span>

| <span data-ttu-id="989dc-165">命令</span><span class="sxs-lookup"><span data-stu-id="989dc-165">Command</span></span>                             | <span data-ttu-id="989dc-166">函数</span><span class="sxs-lookup"><span data-stu-id="989dc-166">Function</span></span>                                                                                      |
| ----------------------------------- | --------------------------------------------------------------------------------------------- |
| `soshelp`                           | <span data-ttu-id="989dc-167">显示所有可用命令</span><span class="sxs-lookup"><span data-stu-id="989dc-167">Displays all available commands</span></span>                                                               |
| `soshelp|help <command>`            | <span data-ttu-id="989dc-168">执行指定的命令。</span><span class="sxs-lookup"><span data-stu-id="989dc-168">Displays the specified command.</span></span>                                                               |
| `exit|quit`                         | <span data-ttu-id="989dc-169">退出交互模式。</span><span class="sxs-lookup"><span data-stu-id="989dc-169">Exits interactive mode.</span></span>                                                                       |
| `clrstack <arguments>`              | <span data-ttu-id="989dc-170">仅提供托管代码的堆栈跟踪。</span><span class="sxs-lookup"><span data-stu-id="989dc-170">Provides a stack trace of managed code only.</span></span>                                                  |
| `clrthreads <arguments>`            | <span data-ttu-id="989dc-171">列出正在运行的托管线程。</span><span class="sxs-lookup"><span data-stu-id="989dc-171">Lists the managed threads running.</span></span>                                                            |
| `dumpasync <arguments>`             | <span data-ttu-id="989dc-172">显示有关垃圾回收堆上异步状态机的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-172">Displays information about async state machines on the garbage-collected heap.</span></span>                |
| `dumpassembly <arguments>`          | <span data-ttu-id="989dc-173">显示有关指定地址处程序集的详细信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-173">Displays details about the assembly at the specified address.</span></span>                                 |
| `dumpclass <arguments>`             | <span data-ttu-id="989dc-174">显示有关指定地址处的 `EEClass` 结构的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-174">Displays information about the `EEClass` structure at the specified address.</span></span>                  |
| `dumpdelegate <arguments>`          | <span data-ttu-id="989dc-175">显示有关指定地址处的委托的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-175">Displays information about the delegate at the specified address.</span></span>                             |
| `dumpdomain <arguments>`            | <span data-ttu-id="989dc-176">显示所有 AppDomain 和指定域中的所有程序集的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-176">Displays information all the AppDomains and all assemblies within the specified domain.</span></span>       |
| `dumpheap <arguments>`              | <span data-ttu-id="989dc-177">显示有关垃圾回收堆的信息和有关对象的收集统计信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-177">Displays info about the garbage-collected heap and collection statistics about objects.</span></span>       |
| `dumpil <arguments>`                | <span data-ttu-id="989dc-178">显示与托管方法关联的 Microsoft 中间语言 (MSIL)。</span><span class="sxs-lookup"><span data-stu-id="989dc-178">Displays the Microsoft intermediate language (MSIL) that is associated with a managed method.</span></span> |
| `dumplog <arguments>`               | <span data-ttu-id="989dc-179">将内存中压力日志的内容写入到指定文件。</span><span class="sxs-lookup"><span data-stu-id="989dc-179">Writes the contents of an in-memory stress log to the specified file.</span></span>                         |
| `dumpmd <arguments>`                | <span data-ttu-id="989dc-180">显示有关指定地址处的 `MethodDesc` 结构的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-180">Displays information about the `MethodDesc` structure at the specified address.</span></span>               |
| `dumpmodule <arguments>`            | <span data-ttu-id="989dc-181">显示有关指定地址处的模块的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-181">Displays information about the module at the specified address.</span></span>                               |
| `dumpmt <arguments>`                | <span data-ttu-id="989dc-182">显示有关指定地址处的 `MethodTable` 的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-182">Displays information about the `MethodTable` at the specified address.</span></span>                        |
| `dumpobj <arguments>`               | <span data-ttu-id="989dc-183">显示有关位于指定地址处的对象的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-183">Displays info about the object at the specified address.</span></span>                                      |
| `dso|dumpstackobjects <arguments>`  | <span data-ttu-id="989dc-184">显示在当前堆栈的边界内找到的所有托管对象。</span><span class="sxs-lookup"><span data-stu-id="989dc-184">Displays all managed objects found within the bounds of the current stack.</span></span>                    |
| `eeheap <arguments>`                | <span data-ttu-id="989dc-185">显示有关内部运行时数据结构所使用的进程内存的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-185">Displays info about process memory consumed by internal runtime data structures.</span></span>              |
| `finalizequeue <arguments>`         | <span data-ttu-id="989dc-186">显示所有已进行终结注册的对象。</span><span class="sxs-lookup"><span data-stu-id="989dc-186">Displays all objects registered for finalization.</span></span>                                             |
| `gcroot <arguments>`                | <span data-ttu-id="989dc-187">显示有关对指定地址处的对象的引用（或根）的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-187">Displays info about references (or roots) to the object at the specified address.</span></span>             |
| `gcwhere <arguments>`               | <span data-ttu-id="989dc-188">显示传入参数在 GC 堆中的位置。</span><span class="sxs-lookup"><span data-stu-id="989dc-188">Displays the location in the GC heap of the argument passed in.</span></span>                               |
| `ip2md <arguments>`                 | <span data-ttu-id="989dc-189">显示 JIT 代码中指定地址处的 `MethodDesc` 结构。</span><span class="sxs-lookup"><span data-stu-id="989dc-189">Displays the `MethodDesc` structure at the specified address in JIT code.</span></span>                     |
| `histclear <arguments>`             | <span data-ttu-id="989dc-190">释放由 `hist*` 命令系列使用的任何资源。</span><span class="sxs-lookup"><span data-stu-id="989dc-190">Releases any resources used by the family of `hist*` commands.</span></span>                                |
| `histinit <arguments>`              | <span data-ttu-id="989dc-191">从保存在调试对象中的压力日志初始化 SOS 结构。</span><span class="sxs-lookup"><span data-stu-id="989dc-191">Initializes the SOS structures from the stress log saved in the debuggee.</span></span>                     |
| `histobj <arguments>`               | <span data-ttu-id="989dc-192">显示与 `<arguments>` 相关的垃圾回收压力日志重定位。</span><span class="sxs-lookup"><span data-stu-id="989dc-192">Displays the garbage collection stress log relocations related to `<arguments>`.</span></span>              |
| `histobjfind <arguments>`           | <span data-ttu-id="989dc-193">显示在指定地址处引用对象的所有日志项。</span><span class="sxs-lookup"><span data-stu-id="989dc-193">Displays all the log entries that reference the object at the specified address.</span></span>              |
| `histroot <arguments>`              | <span data-ttu-id="989dc-194">显示与指定根的提升和重定位相关的信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-194">Displays information related to both promotions and relocations of the specified root.</span></span>        |
| `lm|modules`                        | <span data-ttu-id="989dc-195">显示进程中的本机模块。</span><span class="sxs-lookup"><span data-stu-id="989dc-195">Displays the native modules in the process.</span></span>                                                   |
| `name2ee <arguments>`               | <span data-ttu-id="989dc-196">显示 `<argument>` 的 `MethodTable` 和 `EEClass` 结构。</span><span class="sxs-lookup"><span data-stu-id="989dc-196">Displays the `MethodTable` and `EEClass` structures for the `<argument>`.</span></span>                     |
| `pe|printexception <arguments>`     | <span data-ttu-id="989dc-197">显示从 <xref:System.Exception> 类派生的 `<argument>` 的任何对象。</span><span class="sxs-lookup"><span data-stu-id="989dc-197">Displays any object derived from the <xref:System.Exception> class for the `<argument>`.</span></span>      |
| `setsymbolserver <arguments>`       | <span data-ttu-id="989dc-198">启用符号服务器支持</span><span class="sxs-lookup"><span data-stu-id="989dc-198">Enables the symbol server support</span></span>                                                             |
| `syncblk <arguments>`               | <span data-ttu-id="989dc-199">显示 SyncBlock 持有者信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-199">Displays the SyncBlock holder info.</span></span>                                                           |
| `threads|setthread <threadid>`      | <span data-ttu-id="989dc-200">设置或显示 SOS 命令的当前线程 ID。</span><span class="sxs-lookup"><span data-stu-id="989dc-200">Sets or displays the current thread ID for the SOS commands.</span></span>                                  |

> [!NOTE]
> <span data-ttu-id="989dc-201">可以在[适用于 .NET 的 SOS 调试扩展](sos-debugging-extension.md)中找到其他详细信息。</span><span class="sxs-lookup"><span data-stu-id="989dc-201">Additional details can be found in [SOS Debugging Extension for .NET](sos-debugging-extension.md).</span></span>

## <a name="using-dotnet-dump"></a><span data-ttu-id="989dc-202">使用 `dotnet-dump`</span><span class="sxs-lookup"><span data-stu-id="989dc-202">Using `dotnet-dump`</span></span>

<span data-ttu-id="989dc-203">第一步是收集转储。</span><span class="sxs-lookup"><span data-stu-id="989dc-203">The first step is to collect a dump.</span></span> <span data-ttu-id="989dc-204">如果已生成核心转储，则可以跳过此步骤。</span><span class="sxs-lookup"><span data-stu-id="989dc-204">This step can be skipped if a core dump has already been generated.</span></span> <span data-ttu-id="989dc-205">操作系统或 .NET Core 运行时的内置[转储生成功能](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md)均可以创建核心转储。</span><span class="sxs-lookup"><span data-stu-id="989dc-205">The operating system or the .NET Core runtime's built-in [dump generation feature](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md) can each create core dumps.</span></span>

```console
$ dotnet-dump collect --process-id 1902
Writing minidump to file ./core_20190226_135837
Written 98983936 bytes (24166 pages) to core file
Complete
```

<span data-ttu-id="989dc-206">现在，使用 `analyze` 命令分析核心转储：</span><span class="sxs-lookup"><span data-stu-id="989dc-206">Now analyze the core dump with the `analyze` command:</span></span>

```console
$ dotnet-dump analyze ./core_20190226_135850
Loading core dump: ./core_20190226_135850
Ready to process analysis commands. Type 'help' to list available commands or 'help [command]' to get detailed help on a command.
Type 'quit' or 'exit' to exit the session.
>
```

<span data-ttu-id="989dc-207">此操作会显示一个交互式会话，该会话接受以下类似命令：</span><span class="sxs-lookup"><span data-stu-id="989dc-207">This action brings up an interactive session that accepts commands like:</span></span>

```console
> clrstack
OS Thread Id: 0x573d (0)
    Child SP               IP Call Site
00007FFD28B42C58 00007fb22c1a8ed9 [HelperMethodFrame_PROTECTOBJ: 00007ffd28b42c58] System.RuntimeMethodHandle.InvokeMethod(System.Object, System.Object[], System.Signature, Boolean, Boolean)
00007FFD28B42DD0 00007FB1B1334F67 System.Reflection.RuntimeMethodInfo.Invoke(System.Object, System.Reflection.BindingFlags, System.Reflection.Binder, System.Object[], System.Globalization.CultureInfo) [/root/coreclr/src/mscorlib/src/System/Reflection/RuntimeMethodInfo.cs @ 472]
00007FFD28B42E20 00007FB1B18D33ED SymbolTestApp.Program.Foo4(System.String) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 54]
00007FFD28B42ED0 00007FB1B18D2FC4 SymbolTestApp.Program.Foo2(Int32, System.String) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 29]
00007FFD28B42F00 00007FB1B18D2F5A SymbolTestApp.Program.Foo1(Int32, System.String) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 24]
00007FFD28B42F30 00007FB1B18D168E SymbolTestApp.Program.Main(System.String[]) [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 19]
00007FFD28B43210 00007fb22aa9cedf [GCFrame: 00007ffd28b43210]
00007FFD28B43610 00007fb22aa9cedf [GCFrame: 00007ffd28b43610]
```

<span data-ttu-id="989dc-208">查看已终止应用的未经处理的异常：</span><span class="sxs-lookup"><span data-stu-id="989dc-208">To see an unhandled exception that killed your app:</span></span>

```console
> pe -lines
Exception object: 00007fb18c038590
Exception type:   System.Reflection.TargetInvocationException
Message:          Exception has been thrown by the target of an invocation.
InnerException:   System.Exception, Use !PrintException 00007FB18C038368 to see more.
StackTrace (generated):
SP               IP               Function
00007FFD28B42DD0 0000000000000000 System.Private.CoreLib.dll!System.RuntimeMethodHandle.InvokeMethod(System.Object, System.Object[], System.Signature, Boolean, Boolean)
00007FFD28B42DD0 00007FB1B1334F67 System.Private.CoreLib.dll!System.Reflection.RuntimeMethodInfo.Invoke(System.Object, System.Reflection.BindingFlags, System.Reflection.Binder, System.Object[], System.Globalization.CultureInfo)+0xa7 [/root/coreclr/src/mscorlib/src/System/Reflection/RuntimeMethodInfo.cs @ 472]
00007FFD28B42E20 00007FB1B18D33ED SymbolTestApp.dll!SymbolTestApp.Program.Foo4(System.String)+0x15d [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 54]
00007FFD28B42ED0 00007FB1B18D2FC4 SymbolTestApp.dll!SymbolTestApp.Program.Foo2(Int32, System.String)+0x34 [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 29]
00007FFD28B42F00 00007FB1B18D2F5A SymbolTestApp.dll!SymbolTestApp.Program.Foo1(Int32, System.String)+0x3a [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 24]
00007FFD28B42F30 00007FB1B18D168E SymbolTestApp.dll!SymbolTestApp.Program.Main(System.String[])+0x6e [/home/mikem/builds/SymbolTestApp/SymbolTestApp/SymbolTestApp.cs @ 19]

StackTraceString: <none>
HResult: 80131604
```

## <a name="special-instructions-for-docker"></a><span data-ttu-id="989dc-209">Docker 的特殊说明</span><span class="sxs-lookup"><span data-stu-id="989dc-209">Special instructions for Docker</span></span>

<span data-ttu-id="989dc-210">如果在 Docker 下运行，则转储集合需要 `SYS_PTRACE` 功能（`--cap-add=SYS_PTRACE` 或 `--privileged`）。</span><span class="sxs-lookup"><span data-stu-id="989dc-210">If you're running under Docker, dump collection requires `SYS_PTRACE` capabilities (`--cap-add=SYS_PTRACE` or `--privileged`).</span></span>

<span data-ttu-id="989dc-211">在 Microsoft .NET Core SDK Linux Docker 映像上，某些 `dotnet-dump` 命令可能会引发以下异常：</span><span class="sxs-lookup"><span data-stu-id="989dc-211">On Microsoft .NET Core SDK Linux Docker images, some `dotnet-dump` commands can throw the following exception:</span></span>

> <span data-ttu-id="989dc-212">未经处理的异常：System.DllNotFoundException:无法加载共享库“libdl.so”或其依赖项之一的异常。</span><span class="sxs-lookup"><span data-stu-id="989dc-212">Unhandled exception: System.DllNotFoundException: Unable to load shared library 'libdl.so' or one of its dependencies' exception.</span></span>

<span data-ttu-id="989dc-213">若要解决此问题，请安装“libc6-dev”包。</span><span class="sxs-lookup"><span data-stu-id="989dc-213">To work around this problem, install the "libc6-dev" package.</span></span>

## <a name="see-also"></a><span data-ttu-id="989dc-214">另请参阅</span><span class="sxs-lookup"><span data-stu-id="989dc-214">See also</span></span>

- [<span data-ttu-id="989dc-215">收集和分析内存转储博客</span><span class="sxs-lookup"><span data-stu-id="989dc-215">Collecting and analyzing memory dumps blog</span></span>](https://devblogs.microsoft.com/dotnet/collecting-and-analyzing-memory-dumps/)
- [<span data-ttu-id="989dc-216">堆分析工具 (dotnet-gcdump)</span><span class="sxs-lookup"><span data-stu-id="989dc-216">Heap analysis tool (dotnet-gcdump)</span></span>](dotnet-gcdump.md)
