---
description: 用于错误和警告的 C# 编译器选项。 这些选项可禁用或启用警告，并将警告控制为错误。
title: C# 编译器选项 - 错误和警告
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- WarningLevel compiler option [C#]
- TreatWarningsAsErrors compiler option [C#]
- WarningsAsErrors compiler option [C#]
- WarningsNotAsErrors compiler option [C#]
- DisabledWarnings compiler option [C#]
- CodeAnalysisRuleSet compiler option [C#]
- ErrorLog compiler option [C#]
- ReportAnalyzer compiler option [C#]
ms.openlocfilehash: 2bdda13d6b8b2b75d80c228da678a5b7fbcb8892
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482417"
---
# <a name="c-compiler-options-to-report-errors-and-warnings"></a>用于报告错误和警告的 C# 编译器选项

以下选项控制编译器如何报告错误和警告。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **WarningLevel** / `-warn`：设置警告等级。
- **TreatWarningsAsErrors** / `-warnaserror`：将所有警告视为错误
- **WarningsAsErrors** / `-warnaserror`：将一个或多个警告视为错误
- **WarningsNotAsErrors** / `-warnaserror`：将一个或多个警告不视为错误
- **DisabledWarnings** / `-nowarn`：设置禁用的警告的列表。
- **CodeAnalysisRuleSet** / `-ruleset`：指定可禁用特定诊断的规则集文件。
- **ErrorLog** / `-errorlog`：指定要记录所有编译器和分析器诊断的文件。
- **ReportAnalyzer** / `-reportanalyzer`：报告其他分析器信息，如执行时间。

## <a name="warninglevel"></a>WarningLevel

WarningLevel 选项指定编译器显示的警告等级。

```xml
<WarningLevel>3</WarningLevel>
```

此元素值是要为编译显示的警告等级：较低的数字仅显示高严重性警告。 较高的数字显示更多警告。 该值必须是零或正整数：

|警告级别|含义|
|-------------------|-------------|
|0|关闭发出所有警告消息。|
|1|显示严重警告消息。|  
|2|显示等级 1 警告以及某些不太严重的警告，如有关隐藏类成员的警告。|
|3|显示等级 2 警告以及某些不太严重的警告，如有关经常计算为 `true` 或 `false` 的表达式的警告。|
|4（默认值）|显示所有等级 3 警告以及信息性警告。|
|5|显示等级 4 警告以及 C# 9.0 附带的编译器中的[其他警告](https://github.com/dotnet/roslyn/blob/a6013f3213c902c0973b2d371c3007217d610533/docs/compilers/CSharp/Warnversion%20Warning%20Waves.md)。|
|大于 5|任何大于 5 的值都将被视为 5。 为了确保在编译器更新了新的警告等级时，你始终能收到所有警告，可以输入一个任意的大值（例如 `9999`）。

若要获取有关错误或警告的信息，可以在帮助索引中查找错误代码。 有关获取错误或警告信息的其他方法，请参阅 [C# 编译器错误](../compiler-messages/index.md)。 使用 [TreatWarningsAsErrors](#treatwarningsaserrors) 将所有警告视为错误。 使用 [DisabledWarnings](#disabledwarnings) 禁用某些警告。  

## <a name="treatwarningsaserrors"></a>TreatWarningsAsErrors

TreatWarningsAsErrors 选项将所有警告视为错误。 你还可以使用 TreatWarningsAsErrors 仅将部分警告设置为错误。 如果启用 TreatWarningsAsErrors，则可以使用 TreatWarningsAsErrors 列出不应被视为错误的警告。

```xml
<TreatWarningsAsErrors></TreatWarningsAsErrors>
```

而是将所有警告消息报告为错误。 生成过程会停止（不生成输出文件）。 默认情况下，TreatWarningsAsErrors 不生效，这意味着警告不会阻止生成输出文件。 （可选）如果希望仅将一些特定警告视为错误，则可以指定视为错误的警告编号的逗号分隔列表。 可以使用 [Nullable](language.md#nullable) 的简写形式指定所有为 Null 性警告的集合。 使用 [WarningLevel](#warninglevel) 指定你希望编译器显示的警告等级。 使用 [DisabledWarnings](#disabledwarnings) 禁用某些警告。

## <a name="warningsaserrors-and-warningsnotaserrors"></a>WarningsAsErrors 和 WarningsNotAsErrors

WarningsAsErrors 和 WarningsNotAsErrors 选项会覆盖警告列表的 TreatWarningsAsErrors 选项。

允许将警告 0219 和 0168 视为错误：

```xml
<WarningsAsErrors>0219,0168</WarningsAsErrors>
```

禁止将相同的警告视为错误：

```xml
<WarningsNotAsErrors>0219,0168</WarningsNotAsErrors>
```

可以使用 WarningsAsErrors 将一组警告配置为错误。 将所有警告都设置为错误时，使用 WarningsNotAsErrors 配置一组不应为错误的警告。

## <a name="disabledwarnings"></a>DisabledWarnings

使用 DisabledWarnings 选项可以禁止编译器显示一个或多个警告。 使用逗号分隔多个警告编号。

```xml
<DisabledWarnings>number1, number2</DisabledWarnings>
```

`number1`, `number2` 是你希望编译器禁止显示的警告编号。 指定警告标识符的数值部分。 例如，如果要禁止显示 CS0028，可以指定 `<DisabledWarnings>28</DisabledWarnings>`。 编译器会以无提示方式忽略传递给 DisabledWarnings 的警告编号，这些编号在之前的版本中有效，但已被移除。 例如，CS0679 在 Visual Studio .NET 2002 的编译器中有效，但后来已被移除。

 通过 DisabledWarnings 选项不能禁止显示以下警告：

- 编译器警告（等级 1）CS2002  
- 编译器警告（等级 1）CS2023
- 编译器警告（等级 1）CS2029

## <a name="codeanalysisruleset"></a>CodeAnalysisRuleSet

指定可配置特定诊断的规则集文件。

```xml
<CodeAnalysisRuleSet>MyConfiguration.ruleset</CodeAnalysisRuleSet>
```

其中 `MyConfiguration.ruleset` 是规则集文件的路径。 有关使用规则集的详细信息，请参阅[有关规则集的 Visual Studio 文档](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules)中的文章。

## <a name="errorlog"></a>ErrorLog

指定要记录所有编译器和分析器诊断的文件。

```xml
<ErrorLog>MyConfiguration.ruleset</ErrorLog>
```

ErrorLog 选项会导致编译器输出[静态分析结果交换格式 (SARIF) 日志](https://github.com/microsoft/sarif-tutorials/blob/main/docs/1-Introduction.md#:~:text=What%20is%20SARIF%3F,for%20use%20by%20simpler%20tools)。 SARIF 日志通常由分析编译器和分析器诊断结果的工具来读取。

## <a name="reportanalyzer"></a>ReportAnalyzer

报告其他分析器信息，如执行时间。

```xml
<ReportAnalyzer>true</ReportAnalyzer>
```

ReportAnalyzer 选项会导致编译器发出额外的 MSBuild 日志信息，这些信息详细说明生成中分析器的性能特征。 它通常由分析器作者在验证分析器时使用。
