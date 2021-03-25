---
description: 其他 C# 编译器选项。 这些选项为编译器提供常规选项。
title: 不适合其他类别的 C# 编译器选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- ResponseFiles compiler option [C#]
- NoLogo compiler option [C#]
- NoConfig compiler option [C#]
ms.openlocfilehash: ec7d9c3685c9acb5ca3cda28aca55abb26b836cf
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482410"
---
# <a name="miscellaneous-c-compiler-options"></a>其他 C# 编译器选项

下面的选项控制其他编译器行为。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **ResponseFiles** / `-@`：读取响应文件以获取更多选项。
- **NoLogo** / `-nologo`：禁止显示编译器版权消息。
- **NoConfig** / `-noconfig`：不自动包括 CSC.RSP 文件。

## <a name="responsefiles"></a>ResponseFiles

通过 ResponseFiles 选项，可以指定包含编译器选项和要编译的源代码文件的文件。

```xml
<ResponseFiles>response_file</ResponseFiles>
```

`response_file` 指定一个文件来列出编译器选项或要编译的源代码文件。 编译器选项和源代码文件将由编译器处理，就像它们在命令行中被指定一样。 若要在一次编译中指定多个响应文件，请指定多个响应文件选项。 在响应文件中，多个编译器选项和源代码文件可以出现在同一行中。 单个编译器选项的指定必须出现在同一行中（不能跨行）。 响应文件的注释可以 # 符号开始。 从响应文件内指定编译器选项就如同在命令行发出这些命令。 编译器在读取命令选项时会进行处理。 命令行参数可以重写先前在响应文件中列出的选项。 反之，响应文件中的选项也将重写先前在命令行或其他响应文件中列出的选项。 C# 提供 csc.rsp 文件，该文件与 csc.exe 文件位于同一目录。 有关响应文件格式的详细信息，请参阅 [NoConfig](#noconfig)。 不能在 Visual Studio 开发环境中设置此编译器选项，也不能以编程方式对其进行更改。 以下几行来自示例响应文件：

```console
# build the first output file
-target:exe -out:MyExe.exe source1.cs source2.cs
```

## <a name="nologo"></a>NoLogo

NoLogo 选项可在编译器启动时禁止显示登录版权标志，并在编译期间禁止显示信息性消息。

```xml
<NoLogo>true</NoLogo>
```

## <a name="noconfig"></a>NoConfig

NoConfig 选项会告知编译器不要使用 csc.rsp 文件进行编译。

```xml
<NoConfig>true</NoConfig>
```

csc.rsp 文件引用 .NET Framework 随附的所有程序集。 Visual Studio .NET 开发环境包括的实际引用取决于项目类型。 可以修改 csc.rsp 文件，并指定每次编译时应包括的其他编译器选项。 如果你不需要编译器查询和使用 csc.rsp 文件中的设置，请指定 NoConfig。 此编译器选项在 Visual Studio 中不可用，并且无法以编程方式更改。
