---
title: .NET 中的符号
description: .NET 中的符号和可移植 PDB 简介
ms.date: 02/08/2021
ms.openlocfilehash: 8f217bf8b62ff12a6ea1dc6a5b14b34d8037dd2d
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759893"
---
# <a name="symbols"></a>符号

符号可用于调试和其他诊断工具。 符号文件的内容在语言、编译器和平台之间各有不同。 以非常概要的角度来看，符号是源代码和编译器生成的二进制文件之间的映射。 [Visual Studio](/visualstudio/debugger/what-is-debugging) 和 [Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging) 等工具会使用这些映射来解析源行号信息或本地变量名称。

可在[有关符号的 Windows 文档](/windows/win32/dxtecharts/debugging-with-symbols)中更详细地了解适用于 Windows 的符号，不过其中很多概念也不用于其他平台。

## <a name="learn-about-nets-portable-pdb-format"></a>了解 .NET 的可移植 PDB 格式

.NET Core 引入了一种新的符号文件 (PDB) 格式，即可移植 PDB。 与仅限 Windows 的传统 PDB 不同，可在任意平台上创建和读取可移植 PDB。

### <a name="what-is-a-pdb"></a>什么是 PDB？

PDB 文件是编译器生成的辅助文件，目的是向其他工具（尤其是调试程序）提供主可执行文件中的内容及其生成方式的相关信息。 例如，调试程序读取 PDB 来将 foo.cs 第 12 行映射到适当的可执行文件位置，以便它可设置断点。 Windows PDB 格式已存在很长时间，它是从甚至更久远的其他本机调试符号格式演变而来的。 它最初是用作本机 (C/C++) 程序的一种格式。 针对 .NET Framework 的首次发布，Windows PDB 格式进行了扩展以支持 .NET。

## <a name="use-the-correct-pdb-format-for-your-scenario"></a>使用适合你的方案的 PDB 格式

任何位置都不支持可移植 PDB 和 Windows PDB，因此你需要思考要在哪里使用和调试你的项目来确定要使用的具体格式。 如果你有一个项目，而你希望它能在这两种格式中使用和调试，那么可使用不同的生成配置，并生成项目两次以同时支持这两种类型的使用者。

### <a name="support-for-portable-pdbs"></a>支持可移植 PDB

可在任意操作系统上读取可移植 PDB，建议对托管代码使用这种符号格式，但存在一些旧版工具和应用程序不支持这种格式：

* 面向 .NET Framework 4.7.1 或更低版本的应用程序：将带有映射的堆栈跟踪打印回行号（例如在 ASP.NET 错误页面中）。 方法的名称不受影响，只有源文件名和行号不受支持。

* 使用 .NET 反编译程序（例如 ildasm 或 .NET 反射器），且预期看到源行映射或本地参数名称。

* 最新版本的 [DIA](/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk) 和工具用它来读取符号（例如 WinDBG 支持可移植 PDB），但更低的版本不这样做。

* 可能存在不支持可移植 PDB 的更低版本的探查器。

若要在不支持可移植 PDB 的工具上使用这些格式，可尝试使用 [Pdb2Pdb](https://github.com/dotnet/symreader-converter#pdb2pdb)，它会在可移植 PDB 和 Windows PDB 之间进行转换。

### <a name="support-for-windows-pdbs"></a>支持 Windows PDB

仅可在 Windows 上编写或读取 Windows PDB。 对托管代码使用 Windows PDB 的操作已过时，且只有旧版工具需要此操作。 建议使用可移植 PDB 而不是 Windows PDB，原因是一些更新的编译器功能仅支持可移植 PDB。

## <a name="see-also"></a>另请参阅

* [dotnet-symbol](./dotnet-symbol.md) 可用于下载框架二进制文件的符号文件

* [有关符号的 Windows 文档](/windows/win32/dxtecharts/debugging-with-symbols)
