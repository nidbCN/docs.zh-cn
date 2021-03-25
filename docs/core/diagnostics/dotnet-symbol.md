---
title: dotnet-symbol 诊断工具 - .NET CLI
description: 了解如何安装和使用 dotnet-symbol CLI 工具来下载调试 .NET 转储和小型转储所需的文件。
ms.date: 11/17/2020
ms.openlocfilehash: 4543bd965c889d93d7dc0b89ff2d6f62c4343e5f
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103189899"
---
# <a name="symbol-downloader-dotnet-symbol"></a>符号下载器 (dotnet-symbol)

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="install"></a>安装

若要安装最新版 `dotnet-symbol` [NuGet 包](https://www.nuget.org/packages/dotnet-symbol)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：

```dotnetcli
dotnet tool install --global dotnet-symbol
```

## <a name="synopsis"></a>摘要

```console
dotnet-symbol [-h|--help] [options] <FILES>
```

## <a name="description"></a>描述

`dotnet-symbol` 全局工具下载调试核心转储和小型转储所需的文件（符号、DAC、模块等）。 当调试其他计算机上捕获的转储时，这很有用。 `dotnet-symbol` 可用于下载分析转储所需的模块和符号。

## <a name="options"></a>选项

- **`--microsoft-symbol-server`**

  添加“http://msdl.microsoft.com/download/symbols”符号服务器路径（默认）。

- **`--server-path <symbol server path>`**

  将符号服务器添加到服务器路径。

- **`authenticated-server-path <pat> <server path>`**

  使用个人访问令牌 (PAT) 将经过身份验证的符号服务器添加到服务器路径。

- **`--cache-directory <file cache directory>`**

  添加缓存目录。

- **`--recurse-subdirectories`**

  处理所有子目录中的输入文件。

- **`--host-only`**

  仅下载 lldb 加载核心转储所需的主机程序（即 dotnet）。

- **`--symbols`**

  下载符号文件（.pdb、.dbg 和 .dwarf）。

- **`--modules`**

  下载模块文件（.dll、.so 和 .dylib）。

- **`--debugging`**

  下载特殊的调试模块（DAC、DBI 和 SOS）。

- **`--windows-pdbs`**

  当可移植的 PDB 也可用时，会强制下载 Windows PDB。

- **`-o, --output <output directory>`**

  设置输出目录。 否则，请在输入文件旁边写入（默认）。

- **`-d, --diagnostics`**

  启用诊断输出。

- **`-h|--help`**

  显示命令行帮助。

## <a name="download-symbols"></a>下载符号

默认情况下，针对转储文件运行 `dotnet-symbol` 将下载调试转储所需的所有模块、符号和 DAC/DBI 文件，包括托管程序集。 由于 SOS 现在可以按需下载符号，因此可以使用仅带主机 (dotnet) 和调试模块的 lldb 分析大多数 Linux 核心转储。 若要获取使用 lldb 诊断核心转储所需的这些文件，请运行以下内容：

```console
dotnet-symbol --host-only --debugging <dump file path>
```

## <a name="troubleshoot"></a>故障排除

- 下载符号时出现“404 未找到”。

   只有通过官方渠道（例如[官方网站](https://dotnet.microsoft.com/download/dotnet)和 [dotnet 安装脚本中的默认源](../tools/dotnet-install-script.md)）获得的官方 .NET Core 运行时版本才支持符号下载。 下载调试文件时出现 404 错误，这可能表示转储是使用来自其他源的 .NET Core 运行时创建的，例如，从本地源、特定 Linux 发行版或从社区站点（例如 archlinux）构建的转储。 在此类情况下，应从这些源或创建转储文件的环境复制调试所需的文件（dotnet、libcoreclr.so 和 libmscordaccore.so）。

## <a name="see-also"></a>另请参阅

* [使用符号进行调试](/windows/win32/dxtecharts/debugging-with-symbols)
* [符号和可移植 PDB](./symbols.md)
