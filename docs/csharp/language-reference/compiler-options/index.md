---
description: C# 编译器选项。 了解控制 C# 编译器行为的选项。
title: C# 编译器选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- compiler options [C#]
- csc.exe
- cl.exe compiler, C# options
- Visual C# compiler
- Visual C#, compiler options
ms.assetid: d3403556-1816-4546-a782-e8223a772e44
ms.openlocfilehash: cbe4db51652e8bfd00c555b6ddd230e124a08360
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478479"
---
# <a name="c-compiler-options"></a>C# 编译器选项

本节介绍 C# 编译器解释的选项。 可以通过两种不同的方法设置 .NET 项目中的编译器选项：

- 在 \*.csproj 文件中指定选项：可以为 \*.csproj 文件中的任何编译器选项添加 XML 元素。 元素名称与编译器选项相同。 用 XML 元素的值设置编译器选项的值。 有关项目文件中设置选项的详细信息，请参阅[适用于 .NET SDK 项目的 MSBuild 属性](../../../core/project-sdk/msbuild-props.md)一文。
- 使用 Visual Studio 属性页：Visual Studio 提供了属性页，可用于编辑生成属性。 若要了解有关详细信息，请参阅[管理项目和解决方案属性 - Windows](/visualstudio/ide/managing-project-and-solution-properties#c-visual-basic-and-f-projects) 或[管理项目和解决方案属性 - Mac](/visualstudio/mac/managing-solutions-and-project-properties)。

## <a name="net-framework-projects"></a>.NET Framework 项目

> [!IMPORTANT]
> 本部分仅适用于 .NET Framework 项目。

除上述机制以外，还可以使用两种附加方法为 .NET Framework 项目设置编译器选项：

- .NET Framework 项目的命令行参数：.NET Framework 项目使用 csc.exe 而不是 `dotnet build` 生成项目。 可以为 .NET Framework 项目指定 csc.exe 的命令行参数。
- 已编译的 ASP.NET 页面：.NET Framework 项目使用 web.config 文件的一部分来编译页面。 对于新的生成系统和 ASP.NET Core 项目，将从项目文件中设置选项。

某些编译器选项的单词从 csc.exe 和 .NET Framework 项目更改为新的 MSBuild 系统。 本部分使用的是新语法。 每个页面顶部同时列出了这两个版本。 对于 csc.exe，所有参数都会在选项和冒号后列出。 例如，`-doc` 选项将为：

```console
-doc:DocFile.xml
```

通过在命令提示符处键入 C# 编译器的可执行文件名称 (csc.exe)，可调用该编译器。

对于 .NET Framework 项目，还可以从命令行运行 csc.exe。 每个编译器选项均有两种形式： **-option** 和 **/option**。 在 .NET Framework Web 项目中，在 web.config 文件中指定用于编译代码隐藏的选项。 有关详细信息，请参阅 [\<compiler> 元素](../../../framework/configure-apps/file-schema/compiler/compiler-element.md)。

如果使用“Visual Studio 开发人员命令提示”窗口，系统将设置所有必需的环境变量。 有关如何访问此工具的信息，请参阅 [Visual Studio 开发人员命令提示](../../../framework/tools/developer-command-prompt-for-vs.md)。

csc.exe 可执行文件通常位于 Windows 目录下的 Microsoft.NET\Framework\\ *\<Version>* 文件夹中。 根据每台计算机上的具体配置，此位置可能有所不同。 如果计算机上安装了不止一个版本的 .NET Framework，你将发现此文件的多个版本。 有关此类安装的详细信息，请参阅[如何：确定安装的 .NET Framework 版本](../../../framework/migration-guide/how-to-determine-which-versions-are-installed.md)。
