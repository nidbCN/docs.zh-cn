---
title: .NET 中的代码分析
titleSuffix: ''
description: 了解 .NET SDK 中的源代码分析。
ms.date: 12/04/2020
ms.topic: overview
ms.custom: updateeachrelease
helpviewer_keywords:
- code analysis
- code analyzers
ms.openlocfilehash: 1a0b31f7aca6415510ed0fcd08e9f9a0f8f39bf5
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876599"
---
# <a name="overview-of-net-source-code-analysis"></a>.NET 源代码分析概述

.NET Compiler Platform (Roslyn) 分析器会检查 C# 或 Visual Basic 代码的代码质量和样式问题。 从 .NET 5.0 开始，这些分析器包含在 .NET SDK 中，无需单独安装。 如果项目面向 .NET 5 或更高版本，则默认启用代码分析。 如果项目面向不同的 .NET 实现（例如 .NET Core、.NET Standard 或 .NET Framework），则必须通过将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性设置为 `true` 以手动启用代码分析。

如果你不想移动到 .NET 5+ SDK、具有非 SDK 样式的 .NET Framework 项目或更倾向于使用基于 NuGet 包的模型，则也可以在 [Microsoft.CodeAnalysis.NetAnalyzers NuGet 包](https://www.nuget.org/packages/Microsoft.CodeAnalysis.NetAnalyzers)中使用该分析器。 对于按需版本更新，你可能更倾向于使用基于包的模型。

> [!NOTE]
> .NET 分析器与目标框架无关。 即，你的项目不需要面向特定的 .NET 实现。 分析器适用于面向 `net5.0` 及早期 .NET 版本（如 `netcoreapp3.1` 和 `net472`）的项目。 但是，若要使用 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性启用代码分析，则项目必须引用[项目 SDK](../../core/project-sdk/overview.md)。

如果分析器发现规则冲突，则这些冲突会被报告为建议、警告或错误，具体取决于每个规则的[配置](configuration-options.md)方式。 代码分析冲突以前缀“CA”或“IDE”显示，以便将它们与编译器错误区分开来。

## <a name="code-quality-analysis"></a>代码质量分析

代码质量分析（“CAxxxx”）规则检查 C# 或 Visual Basic 代码的安全性、性能、设计及其他问题。 分析功能针对面向 .NET 5.0 或更高版本的项目默认启用。 可通过将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性设置为 `true`，在面向 .NET 早期版本的项目上启用代码分析。 你也可通过将 `EnableNETAnalyzers` 设置为 `false`，对项目禁用代码分析。

> [!TIP]
> 如果使用 Visual Studio：
>
> - 许多分析器规则都有相关的代码修补程序，可以应用它们来纠正问题。 代码修补程序显示在灯泡图标菜单中。
> - 可通过在解决方案资源管理器中右键单击某个项目，然后选择“属性” > “代码分析”选项卡 >“启用 .NET 分析器”来启用或禁用代码分析  。

### <a name="enabled-rules"></a>已启用的规则

在 .NET 5.0 中，以下规则默认启用。

| 诊断 ID | 类别 | 严重性 | 说明 |
| - | - | - | - |
| [CA1416](/visualstudio/code-quality/ca1416) | 互操作性 | 警告 | 平台兼容性分析器 |
| [CA1417](/visualstudio/code-quality/ca1417) | 互操作性 | 警告 | 请勿对 P/Invokes 的字符串参数使用 `OutAttribute` |
| [CA1831](/visualstudio/code-quality/ca1831) | 性能 | 警告 | 在合适的情况下，对字符串使用 `AsSpan` 而不是基于范围的索引器 |
| [CA2013](/visualstudio/code-quality/ca2013) | 可靠性 | 警告 | 请勿将 `ReferenceEquals` 与值类型结合使用 |
| [CA2014](/visualstudio/code-quality/ca2014) | 可靠性 | 警告 | 请勿在循环中使用 `stackalloc` |
| [CA2015](/visualstudio/code-quality/ca2015) | 可靠性 | 警告 | 请勿为派生自 <xref:System.Buffers.MemoryManager%601> 的类型定义终结器 |
| [CA2200](/visualstudio/code-quality/ca2200) | 使用情况 | 警告 | 再次引发以保留堆栈详细信息
| [CA2247](/visualstudio/code-quality/ca2247) | 使用情况 | 警告 | 传递到 TaskCompletionSource 构造函数的参数应为 <xref:System.Threading.Tasks.TaskCreationOptions> 枚举，而不是 <xref:System.Threading.Tasks.TaskContinuationOptions> |

可更改这些规则的严重性，以禁用这些规则或将它们提升为错误。 也可[启用更多规则](#enable-additional-rules)。

- 有关每个 .NET SDK 版本附带的规则的列表，请参阅[分析器版本](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)。
- 有关所有代码质量规则的列表，请参阅[代码质量规则](quality-rules/index.md)。

### <a name="enable-additional-rules"></a>启用其他规则

分析模式指预定义的代码分析配置，在此配置下，未启用任何规则、启用某些规则或启用所有规则。 在默认分析模式下，只有少量规则[作为生成警告启用](#enabled-rules)。 可通过在项目文件中设置 [\<AnalysisMode>](../../core/project-sdk/msbuild-props.md#analysismode) 属性来更改项目的分析模式。 允许的值为：

| 值 | 说明 |
| - | - |
| `AllDisabledByDefault` | 这是最保守的模式。 默认情况下，禁用所有规则。 可以选择[选择加入](configuration-options.md)各条规则，以启用它们。<br /><br />`<AnalysisMode>AllDisabledByDefault</AnalysisMode>` |
| `AllEnabledByDefault` | 这是最激进的模式。 所有规则作为生成警告启用。 可选择性地[选择退出](configuration-options.md)各条规则以禁用它们。<br /><br />`<AnalysisMode>AllEnabledByDefault</AnalysisMode>` |
| `Default` | 在默认模式下，少量规则作为警告启用、其他规则仅作为带有相应代码修补程序的 Visual Studio IDE 建议启用，而其余规则被完全禁用。 可选择性地[选择加入或退出](configuration-options.md)各条规则以禁用它们。<br /><br />`<AnalysisMode>Default</AnalysisMode>` |

若要查找每个可用规则的默认严重性以及了解规则是否在默认分析模式下启用，请参阅[规则列表](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)。

### <a name="treat-warnings-as-errors"></a>视警告为错误

如果在生成项目时使用 `-warnaserror` 标志，则所有代码分析警告也会被视为错误。 如果不希望在出现 `-warnaserror` 时将代码质量警告 (CAxxxx) 视为错误，可在项目文件中将 `CodeAnalysisTreatWarningsAsErrors` MSBuild 属性设置为 `false`。

```xml
<PropertyGroup>
  <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
</PropertyGroup>
```

你仍会看到任何代码分析警告，但它们不会中断生成。

### <a name="latest-updates"></a>最新更新

默认情况下，在升级到较新版本的 .NET SDK 时，你将获得最新的代码分析规则和默认规则严重性。 如果你不希望出现此行为（例如，如果你想要确保未启用或禁用任何新规则），可通过以下方式之一来替代此行为：

- 将 `AnalysisLevel` MSBuild 属性设置为特定值，以将警告锁定到相应的集。 在升级到较新的 SDK 时，你仍会获得针对这些警告的 bug 修补程序，但系统不会启用新的警告，也不会禁用现有的警告。 例如，若要将规则集锁定为随 .NET SDK 5.0 版本一起提供的规则集，请向项目文件添加以下条目。

  ```xml
  <PropertyGroup>
    <AnalysisLevel>5.0</AnalysisLevel>
  </PropertyGroup>
  ```

  > [!TIP]
  > `AnalysisLevel` 属性的默认值为 `latest`，这意味着在你移动到较新版本的 .NET SDK 时，你始终会获得最新的代码分析规则。

  如需了解详细信息，以及查看可能值的列表，请参阅[AnalysisLevel](../../core/project-sdk/msbuild-props.md#analysislevel)。

- 安装 [Microsoft.CodeAnalysis.NetAnalyzers NuGet 包](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers)，将规则更新与 .NET SDK 更新分离。 对于面向 .NET 5+ 的项目，安装该包将关闭内置 SDK 分析器。 如果 SDK 所含的分析器程序集版本比 NuGet 包所含的版本更新，你会收到生成警告。 若要禁用该警告，请将 `_SkipUpgradeNetAnalyzersNuGetWarning` 属性设置为 `true`。

  > [!NOTE]
  > 如果你安装了 Microsoft.CodeAnalysis.NetAnalyzers NuGet 包，则不应将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性添加到项目文件或 Directory.Build.props 文件。 在安装了 NuGet 包并将 `EnableNETAnalyzers` 属性设置为 `true` 时，一个生成警告随即生成。

## <a name="code-style-analysis"></a>代码样式分析

通过代码样式分析（“IDExxxx”）规则，可在代码库中定义和维护一致的代码样式。 默认的启用设置为：

- 命令行生成：默认情况下，对命令行生成上的所有 .NET 项目禁用代码样式分析。

  从 .NET 5.0 开始，无论是在命令行还是在 Visual Studio 内，你都可以[在生成时启用代码样式分析](#enable-on-build)。 代码样式冲突显示为带有“IDE”前缀的警告或错误。 这使你能够在生成时强制执行一致的代码样式。

- Visual Studio：默认情况下，代码样式分析作为[代码重构快速操作](/visualstudio/ide/code-generation-in-visual-studio)对 Visual Studio 中的所有 .NET 项目启用。

有关代码样式分析规则的完整列表，请参阅[代码样式规则](style-rules/index.md)。

### <a name="enable-on-build"></a>生成时启用

通过 .NET 5.0 SDK 和更高版本，可在从命令行和 Visual Studio 生成时启用代码样式分析。 （然而，出于性能方面的原因，[一些代码样式规则](https://github.com/dotnet/roslyn/blob/9f87b444da9c48a4d492b19f8337339056bf2b95/src/Analyzers/Core/Analyzers/EnforceOnBuildValues.cs#L95)仍仅适用于 Visual Studio IDE。）

执行以下步骤，在生成时启用代码样式分析：

1. 将 MSBuild 属性 [EnforceCodeStyleInBuild](../../core/project-sdk/msbuild-props.md#enforcecodestyleinbuild) 设置为 `true`。

1. 在 .editorconfig 文件中，[配置](configuration-options.md)你希望在生成时作为警告或错误运行的每个“IDE”代码样式规则。 例如：

   ```ini
   [*.{cs,vb}]
   # IDE0040: Accessibility modifiers required (escalated to a build warning)
   dotnet_diagnostic.IDE0040.severity = warning
   ```

   或者，可将整个类别默认配置为警告或错误，然后选择性地禁用该类别中你不希望在生成时运行的规则。 例如：

   ```ini
   [*.{cs,vb}]

   # Default severity for analyzer diagnostics with category 'Style' (escalated to build warnings)
   dotnet_analyzer_diagnostic.category-Style.severity = warning

   # IDE0040: Accessibility modifiers required (disabled on build)
   dotnet_diagnostic.IDE0040.severity = silent
   ```

> [!NOTE]
> 代码样式分析功能为实验性功能，可能会在 .NET 5 和 .NET 6 版本之间发生更改。

## <a name="suppress-a-warning"></a>抑制警告

一种抑制规则冲突的方法是在 EditorConfig 文件中将该规则 ID 的严重性选项设置为 `none`。 例如：

```ini
dotnet_diagnostic.CA1822.severity = none
```

有关抑制警告的详细信息和其他方式，请参阅[如何抑制代码分析警告](suppress-warnings.md)。

## <a name="third-party-analyzers"></a>第三方分析器

除了官方 .NET 分析器外，你也可以安装第三方分析器，如 [StyleCop](https://www.nuget.org/packages/StyleCop.Analyzers/)、[Roslynator](https://www.nuget.org/packages/Roslynator.Analyzers/)、[XUnit Analyzers](https://www.nuget.org/packages/xunit.analyzers/) 和 [Sonar Analyzer](https://www.nuget.org/packages/SonarAnalyzer.CSharp/)。

## <a name="see-also"></a>另请参阅

- [代码质量分析规则引用](quality-rules/index.md)
- [代码样式分析规则引用](style-rules/index.md)
- [Visual Studio 中的代码分析](/visualstudio/code-quality/roslyn-analyzers-overview)
- [.NET 编译器平台 SDK](../../csharp/roslyn-sdk/index.md)
- [教程：编写第一个分析器和代码修补程序](../../csharp/roslyn-sdk/tutorials/how-to-write-csharp-analyzer-code-fix.md)
