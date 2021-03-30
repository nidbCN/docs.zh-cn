---
title: 调试 Linux 转储
description: 在本文中，你将了解如何从 Linux 环境收集和分析转储。
ms.date: 08/27/2020
ms.openlocfilehash: 42038c685c3ad0043c91df140b0133a9ddecec3b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874260"
---
# <a name="debug-linux-dumps"></a>调试 Linux 转储

**本文适用于：** ✔️ .NET Core 3.0 SDK 及更高版本

## <a name="collect-dumps-on-linux"></a>在 Linux 上收集转储

在 Linux 上收集转储的两种建议方法是：

* [`dotnet-dump`](dotnet-dump.md) CLI 工具
* 用于在故障时收集转储的[环境变量](dumps.md#collecting-dumps-on-crash)

### <a name="managed-dumps-with-dotnet-dump"></a>使用 `dotnet-dump` 的托管转储

[`dotnet-dump`](dotnet-dump.md) 工具简单易用，并且不依赖于任何本机调试器。 `dotnet-dump` 可用于各种 Linux 平台（例如 Alpine 或 ARM32/ARM64），在这些平台上，传统调试工具可能不适用。 但是，`dotnet-dump` 只捕获托管状态，因此不能将其用于调试本机代码中的问题。 `dotnet-dump` 收集的转储在具有创建转储的相同 OS 和体系结构的环境中进行分析。 [`dotnet-gcdump`](dotnet-gcdump.md) 工具可用作仅捕获 GC 堆信息但生成可在 Windows 上分析的转储的替代方法。

### <a name="core-dumps-with-createdump"></a>使用 `createdump` 的核心转储

作为创建仅托管转储的 `dotnet-dump` 的替代方法，建议使用 [`createdump`](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md) 这一工具在包含本机和托管信息的 Linux 上创建核心转储。 其他工具（例如 gdb 或 gcore）也可以用于创建核心转储，但可能会缺失托管调试所需的状态，从而导致在分析过程中出现“未知”类型或函数名称。

`createdump` 工具与 .NET Core 运行时一起安装，可以在 libcoreclr.so（通常位于“/usr/share/dotnet/shared/Microsoft.NETCore.App/[version]”中）旁边找到。 该工具采用进程 ID 将转储作为其主要参数收集，还可以采用可选参数来指定要收集的转储类型（带有堆的小型转储是默认类型）。 选项包括：

- **`<input-filename>`**

  要转换的输入跟踪文件。 默认为 trace.nettrace  。

### <a name="options"></a>选项

- **`-f|--name <output-filename>`**

  要将转储写入到其中的文件。 默认值为“/tmp/coredump.%p”，其中 %p 是目标进程的进程 ID。

- **`-n|--normal`**

  创建小型转储。

- **`-h|--withheap`**

  创建带有堆的小型转储（默认）。

- **`-t|--triage`**

  创建会审小型转储。

- **`-u|--full`**

  创建完整的核心转储。

- **`-d|--diag`**

  启用诊断消息。

收集核心转储需要 `SYS_PTRACE` 功能，或者需要 `createdump` 与 sudo 或 su 一起运行。

## <a name="analyze-dumps-on-linux"></a>在 Linux 上分析转储

通过使用 `dotnet-dump analyze` 命令，可以借助 `dotnet-dump` 工具分析使用 `dotnet-dump` 收集的托管转储和使用 `createdump` 收集的核心转储。 `dotnet dump` 要求分析转储的环境与捕获转储的环境具有相同的 OS 和体系结构。

另外，[LLDB](https://lldb.llvm.org/) 可用于分析 Linux 上的核心转储，这允许分析托管帧和本机帧。 LLDB 使用 SOS 扩展调试托管代码。 [`dotnet-sos`](dotnet-sos.md) CLI 工具可用于安装 SOS，它具有[许多用于调试托管代码的有用命令](https://github.com/dotnet/diagnostics/blob/main/documentation/sos-debugging-extension.md)。 若要分析 .NET Core 转储、LLDB 和 SOS，要求在创建转储的环境中使用以下 .NET Core 二进制文件：

1. libmscordaccore.so
2. libcoreclr.so
3. dotnet（用于启动应用的主机）

在大多数情况下，可以使用 [`dotnet-symbol`](dotnet-symbol.md) 工具下载这些二进制文件。 如果无法使用 `dotnet-symbol` 下载所需的二进制文件（例如，如果正在使用从源构建的 .NET Core 专用版本），则可能需要从创建转储的环境复制上面列出的文件。 如果文件不位于转储文件旁边，则可以使用 LLDB/SOS 命令 `setclrpath <path>` 设置应从中加载文件的路径，并使用 `setsymbolserver -directory <path>` 设置用于查找符号文件的路径。

所需文件可用后，就可以通过将 dotnet 主机指定为要调试的可执行文件来将转储加载到 LLDB 中：

```console
lldb --core <dump-file> <host-program>
```

在上面的命令行中，`<dump-file>` 是要分析的转储路径，而 `<host-program>` 是已启动 .NET Core 应用程序的本机程序。 除非应用是独立应用，否则通常为 `dotnet` 二进制文件，在本例中，它是不包含 dll 扩展名的应用程序的名称。

LLDB 启动后，可能需要使用 `setsymbolserver` 命令指向正确的符号位置（`setsymbolserver -ms` 用于使用 Microsoft 的符号服务器或 `setsymbolserver -directory <path>` 用于指定本地路径）。 可以通过运行 `loadsymbols` 来加载本机符号。 此时，[SOS 命令](https://github.com/dotnet/diagnostics/blob/main/documentation/sos-debugging-extension.md)可用于分析转储。

## <a name="see-also"></a>另请参阅

- 若要了解有关安装 SOS 扩展的更多详细信息，请参阅 [dotnet-sos](dotnet-sos.md)。
- 若要了解有关安装和使用符号下载工具的更多详细信息，请参阅 [dotnet-symbol](dotnet-symbol.md)。
- 若要了解有关调试（包括有用的常见问题解答）的更多详细信息，请参阅 [.NET Core 诊断库](https://github.com/dotnet/diagnostics/blob/main/documentation/)。
- 若要获取有关在 Linux 或 Mac 上安装 LLDB 的说明，请参阅[安装 LLDB](https://github.com/dotnet/diagnostics/blob/main/documentation/sos.md#getting-lldb)。
