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
# <a name="dump-collection-and-analysis-utility-dotnet-dump"></a>转储收集和分析实用工具 (dotnet-dump)

 本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本

> [!NOTE]
> macOS 的 `dotnet-dump` 仅在 .NET 5.0 及更高版本中受支持。

## <a name="install"></a>安装

可采用两种方法来下载和安装 `dotnet-dump`：

- **dotnet 全局工具：**

  若要安装最新版 `dotnet-dump` [NuGet 包](https://www.nuget.org/packages/dotnet-dump)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：

  ```dotnetcli
  dotnet tool install --global dotnet-dump
  ```

- **直接下载：**

  下载与平台相匹配的工具可执行文件：

  | (OS)  | 平台 |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-dump/win-x86) \| [x64](https://aka.ms/dotnet-dump/win-x64) \| [arm](https://aka.ms/dotnet-dump/win-arm) \| [arm-x64](https://aka.ms/dotnet-dump/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-dump/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-dump/linux-x64) \| [arm](https://aka.ms/dotnet-dump/linux-arm) \| [arm64](https://aka.ms/dotnet-dump/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-dump/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-dump/linux-musl-arm64) |

> [!NOTE]
> 若要在 x86 应用上使用 `dotnet-dump`，需要使用相应的 x86 版本的工具。

## <a name="synopsis"></a>摘要

```console
dotnet-dump [-h|--help] [--version] <command>
```

## <a name="description"></a>描述

`dotnet-dump` 全局工具是在未涉及任何本机调试器（如 Linux 上的 `lldb`）的情况下收集和分析 Windows 和 Linux 转储的方法。 在 `lldb` 无法正常运行的平台（如 Alpine Linux）上，此工具非常重要。 借助 `dotnet-dump` 工具，可以运行 SOS 命令来分析崩溃和垃圾回收器 (GC)，但它不是本机调试器，因此不支持显示本机堆栈帧之类的操作。

## <a name="options"></a>选项

- **`--version`**

  显示 dotnet-dump 实用工具的版本。

- **`-h|--help`**

  显示命令行帮助。

## <a name="commands"></a>命令

| 命令                                     |
| ------------------------------------------- |
| [dotnet-dump collect](#dotnet-dump-collect) |
| [dotnet-dump analyze](#dotnet-dump-analyze) |

## <a name="dotnet-dump-collect"></a>dotnet-dump collect

从进程捕获转储。

### <a name="synopsis"></a>摘要

```console
dotnet-dump collect [-h|--help] [-p|--process-id] [-n|--name] [--type] [-o|--output] [--diag]
```

### <a name="options"></a>选项

- **`-h|--help`**

  显示命令行帮助。

- **`-p|--process-id <PID>`**

  指定从中收集转储的进程的 ID 号。

- **`-n|--name <name>`**

  指定从中收集转储的进程的名称。

- **`--type <Full|Heap|Mini>`**

  指定转储类型，它确定从进程收集的信息的类型。 有三种类型：

  - `Full` - 最大的转储，包含所有内存（包括模块映像）。
  - `Heap` - 大型且相对全面的转储，其中包含模块列表、线程列表、所有堆栈、异常信息、句柄信息和除映射图像以外的所有内存。
  - `Mini` - 小型转储，其中包含模块列表、线程列表、异常信息和所有堆栈。

  如果未指定，则 `Full` 为默认类型。

- **`-o|--output <output_dump_path>`**

  应在其中写入收集的转储的完整路径和文件名。

  如果未指定：

  - 在 Windows 上默认为 .\dump_YYYYMMDD_HHMMSS.dmp  。
  - 在 Linux 上默认为 ./core_YYYYMMDD_HHMMSS  。

  YYYYMMDD 为年/月/日，HHMMSS 为小时/分钟/秒。

- **`--diag`**

  启用转储收集诊断日志记录。

> [!NOTE]
> 在 Linux 和 macOS 上，此命令需要目标应用程序和 `dotnet-dump` 使用同一 `TMPDIR` 环境变量。 否则，该命令将超时。

> [!NOTE]
> 若要使用 `dotnet-dump` 收集转储，需要以与运行目标进程的用户相同的用户身份或以根身份运行。 否则，该工具将无法与目标进程建立连接。

## <a name="dotnet-dump-analyze"></a>dotnet-dump analyze

启动交互式 shell 以了解转储。 shell 接受各种 [SOS 命令](#analyze-sos-commands)。

### <a name="synopsis"></a>摘要

```console
dotnet-dump analyze <dump_path> [-h|--help] [-c|--command]
```

### <a name="arguments"></a>自变量

- **`<dump_path>`**

  指定要分析的转储文件的路径。

### <a name="options"></a>选项

- **`-c|--command <debug_command>`**

  指定要在启动时在 shell 中运行的[命令](#analyze-sos-commands)。

### <a name="analyze-sos-commands"></a>分析 SOS 命令

| 命令                             | 函数                                                                                      |
| ----------------------------------- | --------------------------------------------------------------------------------------------- |
| `soshelp`                           | 显示所有可用命令                                                               |
| `soshelp|help <command>`            | 执行指定的命令。                                                               |
| `exit|quit`                         | 退出交互模式。                                                                       |
| `clrstack <arguments>`              | 仅提供托管代码的堆栈跟踪。                                                  |
| `clrthreads <arguments>`            | 列出正在运行的托管线程。                                                            |
| `dumpasync <arguments>`             | 显示有关垃圾回收堆上异步状态机的信息。                |
| `dumpassembly <arguments>`          | 显示有关指定地址处程序集的详细信息。                                 |
| `dumpclass <arguments>`             | 显示有关指定地址处的 `EEClass` 结构的信息。                  |
| `dumpdelegate <arguments>`          | 显示有关指定地址处的委托的信息。                             |
| `dumpdomain <arguments>`            | 显示所有 AppDomain 和指定域中的所有程序集的信息。       |
| `dumpheap <arguments>`              | 显示有关垃圾回收堆的信息和有关对象的收集统计信息。       |
| `dumpil <arguments>`                | 显示与托管方法关联的 Microsoft 中间语言 (MSIL)。 |
| `dumplog <arguments>`               | 将内存中压力日志的内容写入到指定文件。                         |
| `dumpmd <arguments>`                | 显示有关指定地址处的 `MethodDesc` 结构的信息。               |
| `dumpmodule <arguments>`            | 显示有关指定地址处的模块的信息。                               |
| `dumpmt <arguments>`                | 显示有关指定地址处的 `MethodTable` 的信息。                        |
| `dumpobj <arguments>`               | 显示有关位于指定地址处的对象的信息。                                      |
| `dso|dumpstackobjects <arguments>`  | 显示在当前堆栈的边界内找到的所有托管对象。                    |
| `eeheap <arguments>`                | 显示有关内部运行时数据结构所使用的进程内存的信息。              |
| `finalizequeue <arguments>`         | 显示所有已进行终结注册的对象。                                             |
| `gcroot <arguments>`                | 显示有关对指定地址处的对象的引用（或根）的信息。             |
| `gcwhere <arguments>`               | 显示传入参数在 GC 堆中的位置。                               |
| `ip2md <arguments>`                 | 显示 JIT 代码中指定地址处的 `MethodDesc` 结构。                     |
| `histclear <arguments>`             | 释放由 `hist*` 命令系列使用的任何资源。                                |
| `histinit <arguments>`              | 从保存在调试对象中的压力日志初始化 SOS 结构。                     |
| `histobj <arguments>`               | 显示与 `<arguments>` 相关的垃圾回收压力日志重定位。              |
| `histobjfind <arguments>`           | 显示在指定地址处引用对象的所有日志项。              |
| `histroot <arguments>`              | 显示与指定根的提升和重定位相关的信息。        |
| `lm|modules`                        | 显示进程中的本机模块。                                                   |
| `name2ee <arguments>`               | 显示 `<argument>` 的 `MethodTable` 和 `EEClass` 结构。                     |
| `pe|printexception <arguments>`     | 显示从 <xref:System.Exception> 类派生的 `<argument>` 的任何对象。      |
| `setsymbolserver <arguments>`       | 启用符号服务器支持                                                             |
| `syncblk <arguments>`               | 显示 SyncBlock 持有者信息。                                                           |
| `threads|setthread <threadid>`      | 设置或显示 SOS 命令的当前线程 ID。                                  |

> [!NOTE]
> 可以在[适用于 .NET 的 SOS 调试扩展](sos-debugging-extension.md)中找到其他详细信息。

## <a name="using-dotnet-dump"></a>使用 `dotnet-dump`

第一步是收集转储。 如果已生成核心转储，则可以跳过此步骤。 操作系统或 .NET Core 运行时的内置[转储生成功能](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md)均可以创建核心转储。

```console
$ dotnet-dump collect --process-id 1902
Writing minidump to file ./core_20190226_135837
Written 98983936 bytes (24166 pages) to core file
Complete
```

现在，使用 `analyze` 命令分析核心转储：

```console
$ dotnet-dump analyze ./core_20190226_135850
Loading core dump: ./core_20190226_135850
Ready to process analysis commands. Type 'help' to list available commands or 'help [command]' to get detailed help on a command.
Type 'quit' or 'exit' to exit the session.
>
```

此操作会显示一个交互式会话，该会话接受以下类似命令：

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

查看已终止应用的未经处理的异常：

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

## <a name="special-instructions-for-docker"></a>Docker 的特殊说明

如果在 Docker 下运行，则转储集合需要 `SYS_PTRACE` 功能（`--cap-add=SYS_PTRACE` 或 `--privileged`）。

在 Microsoft .NET Core SDK Linux Docker 映像上，某些 `dotnet-dump` 命令可能会引发以下异常：

> 未经处理的异常：System.DllNotFoundException:无法加载共享库“libdl.so”或其依赖项之一的异常。

若要解决此问题，请安装“libc6-dev”包。

## <a name="see-also"></a>另请参阅

- [收集和分析内存转储博客](https://devblogs.microsoft.com/dotnet/collecting-and-analyzing-memory-dumps/)
- [堆分析工具 (dotnet-gcdump)](dotnet-gcdump.md)
