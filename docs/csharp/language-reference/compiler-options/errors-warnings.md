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
# <a name="c-compiler-options-to-report-errors-and-warnings"></a><span data-ttu-id="fa38a-104">用于报告错误和警告的 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="fa38a-104">C# Compiler Options to report errors and warnings</span></span>

<span data-ttu-id="fa38a-105">以下选项控制编译器如何报告错误和警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-105">The following options control how the compiler reports errors and warnings.</span></span> <span data-ttu-id="fa38a-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="fa38a-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="fa38a-107">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="fa38a-107">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="fa38a-108">**WarningLevel** / `-warn`：设置警告等级。</span><span class="sxs-lookup"><span data-stu-id="fa38a-108">**WarningLevel** / `-warn`: Set warning level.</span></span>
- <span data-ttu-id="fa38a-109">**TreatWarningsAsErrors** / `-warnaserror`：将所有警告视为错误</span><span class="sxs-lookup"><span data-stu-id="fa38a-109">**TreatWarningsAsErrors** / `-warnaserror`: Treat all warnings as errors</span></span>
- <span data-ttu-id="fa38a-110">**WarningsAsErrors** / `-warnaserror`：将一个或多个警告视为错误</span><span class="sxs-lookup"><span data-stu-id="fa38a-110">**WarningsAsErrors** / `-warnaserror`: Treat one or more warnings as errors</span></span>
- <span data-ttu-id="fa38a-111">**WarningsNotAsErrors** / `-warnaserror`：将一个或多个警告不视为错误</span><span class="sxs-lookup"><span data-stu-id="fa38a-111">**WarningsNotAsErrors** / `-warnaserror`: Treat one or more warnings not as errors</span></span>
- <span data-ttu-id="fa38a-112">**DisabledWarnings** / `-nowarn`：设置禁用的警告的列表。</span><span class="sxs-lookup"><span data-stu-id="fa38a-112">**DisabledWarnings** / `-nowarn`: Set a list of disabled warnings.</span></span>
- <span data-ttu-id="fa38a-113">**CodeAnalysisRuleSet** / `-ruleset`：指定可禁用特定诊断的规则集文件。</span><span class="sxs-lookup"><span data-stu-id="fa38a-113">**CodeAnalysisRuleSet** / `-ruleset`: Specify a ruleset file that disables specific diagnostics.</span></span>
- <span data-ttu-id="fa38a-114">**ErrorLog** / `-errorlog`：指定要记录所有编译器和分析器诊断的文件。</span><span class="sxs-lookup"><span data-stu-id="fa38a-114">**ErrorLog** / `-errorlog`: Specify a file to log all compiler and analyzer diagnostics.</span></span>
- <span data-ttu-id="fa38a-115">**ReportAnalyzer** / `-reportanalyzer`：报告其他分析器信息，如执行时间。</span><span class="sxs-lookup"><span data-stu-id="fa38a-115">**ReportAnalyzer** / `-reportanalyzer`:  Report additional analyzer information, such as execution time.</span></span>

## <a name="warninglevel"></a><span data-ttu-id="fa38a-116">WarningLevel</span><span class="sxs-lookup"><span data-stu-id="fa38a-116">WarningLevel</span></span>

<span data-ttu-id="fa38a-117">WarningLevel 选项指定编译器显示的警告等级。</span><span class="sxs-lookup"><span data-stu-id="fa38a-117">The **WarningLevel** option specifies the warning level for the compiler to display.</span></span>

```xml
<WarningLevel>3</WarningLevel>
```

<span data-ttu-id="fa38a-118">此元素值是要为编译显示的警告等级：较低的数字仅显示高严重性警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-118">The element value is the warning level you want displayed for the compilation: Lower numbers show only high severity warnings.</span></span> <span data-ttu-id="fa38a-119">较高的数字显示更多警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-119">Higher numbers show more warnings.</span></span> <span data-ttu-id="fa38a-120">该值必须是零或正整数：</span><span class="sxs-lookup"><span data-stu-id="fa38a-120">The value must be zero or a positive integer:</span></span>

|<span data-ttu-id="fa38a-121">警告级别</span><span class="sxs-lookup"><span data-stu-id="fa38a-121">Warning level</span></span>|<span data-ttu-id="fa38a-122">含义</span><span class="sxs-lookup"><span data-stu-id="fa38a-122">Meaning</span></span>|
|-------------------|-------------|
|<span data-ttu-id="fa38a-123">0</span><span class="sxs-lookup"><span data-stu-id="fa38a-123">0</span></span>|<span data-ttu-id="fa38a-124">关闭发出所有警告消息。</span><span class="sxs-lookup"><span data-stu-id="fa38a-124">Turns off emission of all warning messages.</span></span>|
|<span data-ttu-id="fa38a-125">1</span><span class="sxs-lookup"><span data-stu-id="fa38a-125">1</span></span>|<span data-ttu-id="fa38a-126">显示严重警告消息。</span><span class="sxs-lookup"><span data-stu-id="fa38a-126">Displays severe warning messages.</span></span>|  
|<span data-ttu-id="fa38a-127">2</span><span class="sxs-lookup"><span data-stu-id="fa38a-127">2</span></span>|<span data-ttu-id="fa38a-128">显示等级 1 警告以及某些不太严重的警告，如有关隐藏类成员的警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-128">Displays level 1 warnings plus certain, less-severe warnings, such as warnings about hiding class members.</span></span>|
|<span data-ttu-id="fa38a-129">3</span><span class="sxs-lookup"><span data-stu-id="fa38a-129">3</span></span>|<span data-ttu-id="fa38a-130">显示等级 2 警告以及某些不太严重的警告，如有关经常计算为 `true` 或 `false` 的表达式的警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-130">Displays level 2 warnings plus certain, less-severe warnings, such as warnings about expressions that always evaluate to `true` or `false`.</span></span>|
|<span data-ttu-id="fa38a-131">4（默认值）</span><span class="sxs-lookup"><span data-stu-id="fa38a-131">4 (the default)</span></span>|<span data-ttu-id="fa38a-132">显示所有等级 3 警告以及信息性警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-132">Displays all level 3 warnings plus informational warnings.</span></span>|
|<span data-ttu-id="fa38a-133">5</span><span class="sxs-lookup"><span data-stu-id="fa38a-133">5</span></span>|<span data-ttu-id="fa38a-134">显示等级 4 警告以及 C# 9.0 附带的编译器中的[其他警告](https://github.com/dotnet/roslyn/blob/a6013f3213c902c0973b2d371c3007217d610533/docs/compilers/CSharp/Warnversion%20Warning%20Waves.md)。</span><span class="sxs-lookup"><span data-stu-id="fa38a-134">Displays level 4 warnings plus [additional warnings](https://github.com/dotnet/roslyn/blob/a6013f3213c902c0973b2d371c3007217d610533/docs/compilers/CSharp/Warnversion%20Warning%20Waves.md) from the compiler shipped with C# 9.0.</span></span>|
|<span data-ttu-id="fa38a-135">大于 5</span><span class="sxs-lookup"><span data-stu-id="fa38a-135">Greater than 5</span></span>|<span data-ttu-id="fa38a-136">任何大于 5 的值都将被视为 5。</span><span class="sxs-lookup"><span data-stu-id="fa38a-136">Any value greater than 5 will be treated as 5.</span></span> <span data-ttu-id="fa38a-137">为了确保在编译器更新了新的警告等级时，你始终能收到所有警告，可以输入一个任意的大值（例如 `9999`）。</span><span class="sxs-lookup"><span data-stu-id="fa38a-137">To make sure you always have all warnings if the compiler is updated with new warning levels, put an arbitrary large value (for example, `9999`).</span></span>

<span data-ttu-id="fa38a-138">若要获取有关错误或警告的信息，可以在帮助索引中查找错误代码。</span><span class="sxs-lookup"><span data-stu-id="fa38a-138">To get information about an error or warning, you can look up the error code in the Help Index.</span></span> <span data-ttu-id="fa38a-139">有关获取错误或警告信息的其他方法，请参阅 [C# 编译器错误](../compiler-messages/index.md)。</span><span class="sxs-lookup"><span data-stu-id="fa38a-139">For other ways to get information about an error or warning, see [C# Compiler Errors](../compiler-messages/index.md).</span></span> <span data-ttu-id="fa38a-140">使用 [TreatWarningsAsErrors](#treatwarningsaserrors) 将所有警告视为错误。</span><span class="sxs-lookup"><span data-stu-id="fa38a-140">Use [**TreatWarningsAsErrors**](#treatwarningsaserrors) to treat all warnings as errors.</span></span> <span data-ttu-id="fa38a-141">使用 [DisabledWarnings](#disabledwarnings) 禁用某些警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-141">Use [**DisabledWarnings**](#disabledwarnings) to disable certain warnings.</span></span>  

## <a name="treatwarningsaserrors"></a><span data-ttu-id="fa38a-142">TreatWarningsAsErrors</span><span class="sxs-lookup"><span data-stu-id="fa38a-142">TreatWarningsAsErrors</span></span>

<span data-ttu-id="fa38a-143">TreatWarningsAsErrors 选项将所有警告视为错误。</span><span class="sxs-lookup"><span data-stu-id="fa38a-143">The **TreatWarningsAsErrors** option treats all warnings as errors.</span></span> <span data-ttu-id="fa38a-144">你还可以使用 TreatWarningsAsErrors 仅将部分警告设置为错误。</span><span class="sxs-lookup"><span data-stu-id="fa38a-144">You can also use the **TreatWarningsAsErrors** to set only some warnings as errors.</span></span> <span data-ttu-id="fa38a-145">如果启用 TreatWarningsAsErrors，则可以使用 TreatWarningsAsErrors 列出不应被视为错误的警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-145">If you turn on **TreatWarningsAsErrors**, you can use **TreatWarningsAsErrors** to list warnings that shouldn't be treated as errors.</span></span>

```xml
<TreatWarningsAsErrors></TreatWarningsAsErrors>
```

<span data-ttu-id="fa38a-146">而是将所有警告消息报告为错误。</span><span class="sxs-lookup"><span data-stu-id="fa38a-146">All warning messages are instead reported as errors.</span></span> <span data-ttu-id="fa38a-147">生成过程会停止（不生成输出文件）。</span><span class="sxs-lookup"><span data-stu-id="fa38a-147">The build process halts (no output files are built).</span></span> <span data-ttu-id="fa38a-148">默认情况下，TreatWarningsAsErrors 不生效，这意味着警告不会阻止生成输出文件。</span><span class="sxs-lookup"><span data-stu-id="fa38a-148">By default, **TreatWarningsAsErrors** isn't in effect, which means warnings don't prevent the generation of an output file.</span></span> <span data-ttu-id="fa38a-149">（可选）如果希望仅将一些特定警告视为错误，则可以指定视为错误的警告编号的逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="fa38a-149">Optionally, if you want only a few specific warnings to be treated as errors, you may specify a comma-separated list of warning numbers to treat as errors.</span></span> <span data-ttu-id="fa38a-150">可以使用 [Nullable](language.md#nullable) 的简写形式指定所有为 Null 性警告的集合。</span><span class="sxs-lookup"><span data-stu-id="fa38a-150">The set of all nullability warnings can be specified with the [**Nullable**](language.md#nullable) shorthand.</span></span> <span data-ttu-id="fa38a-151">使用 [WarningLevel](#warninglevel) 指定你希望编译器显示的警告等级。</span><span class="sxs-lookup"><span data-stu-id="fa38a-151">Use [**WarningLevel**](#warninglevel) to specify the level of warnings that you want the compiler to display.</span></span> <span data-ttu-id="fa38a-152">使用 [DisabledWarnings](#disabledwarnings) 禁用某些警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-152">Use [**DisabledWarnings**](#disabledwarnings) to disable certain warnings.</span></span>

## <a name="warningsaserrors-and-warningsnotaserrors"></a><span data-ttu-id="fa38a-153">WarningsAsErrors 和 WarningsNotAsErrors</span><span class="sxs-lookup"><span data-stu-id="fa38a-153">WarningsAsErrors and WarningsNotAsErrors</span></span>

<span data-ttu-id="fa38a-154">WarningsAsErrors 和 WarningsNotAsErrors 选项会覆盖警告列表的 TreatWarningsAsErrors 选项。</span><span class="sxs-lookup"><span data-stu-id="fa38a-154">The **WarningsAsErrors** and **WarningsNotAsErrors** options override the **TreatWarningsAsErrors** option for a list of warnings.</span></span>

<span data-ttu-id="fa38a-155">允许将警告 0219 和 0168 视为错误：</span><span class="sxs-lookup"><span data-stu-id="fa38a-155">Enable warnings 0219 and 0168 as errors:</span></span>

```xml
<WarningsAsErrors>0219,0168</WarningsAsErrors>
```

<span data-ttu-id="fa38a-156">禁止将相同的警告视为错误：</span><span class="sxs-lookup"><span data-stu-id="fa38a-156">Disable the same warnings as errors:</span></span>

```xml
<WarningsNotAsErrors>0219,0168</WarningsNotAsErrors>
```

<span data-ttu-id="fa38a-157">可以使用 WarningsAsErrors 将一组警告配置为错误。</span><span class="sxs-lookup"><span data-stu-id="fa38a-157">You use **WarningsAsErrors** to configure a set of warnings as errors.</span></span> <span data-ttu-id="fa38a-158">将所有警告都设置为错误时，使用 WarningsNotAsErrors 配置一组不应为错误的警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-158">Use **WarningsNotAsErrors** to configure a set of warnings that should not be errors when you've set all warnings as errors.</span></span>

## <a name="disabledwarnings"></a><span data-ttu-id="fa38a-159">DisabledWarnings</span><span class="sxs-lookup"><span data-stu-id="fa38a-159">DisabledWarnings</span></span>

<span data-ttu-id="fa38a-160">使用 DisabledWarnings 选项可以禁止编译器显示一个或多个警告。</span><span class="sxs-lookup"><span data-stu-id="fa38a-160">The **DisabledWarnings** option lets you suppress the compiler from displaying one or more warnings.</span></span> <span data-ttu-id="fa38a-161">使用逗号分隔多个警告编号。</span><span class="sxs-lookup"><span data-stu-id="fa38a-161">Separate multiple warning numbers with a comma.</span></span>

```xml
<DisabledWarnings>number1, number2</DisabledWarnings>
```

<span data-ttu-id="fa38a-162">`number1`, `number2` 是你希望编译器禁止显示的警告编号。</span><span class="sxs-lookup"><span data-stu-id="fa38a-162">`number1`, `number2` Warning number(s) that you want the compiler to suppress.</span></span> <span data-ttu-id="fa38a-163">指定警告标识符的数值部分。</span><span class="sxs-lookup"><span data-stu-id="fa38a-163">You specify the numeric part of the warning identifier.</span></span> <span data-ttu-id="fa38a-164">例如，如果要禁止显示 CS0028，可以指定 `<DisabledWarnings>28</DisabledWarnings>`。</span><span class="sxs-lookup"><span data-stu-id="fa38a-164">For example, if you want to suppress *CS0028*, you could specify `<DisabledWarnings>28</DisabledWarnings>`.</span></span> <span data-ttu-id="fa38a-165">编译器会以无提示方式忽略传递给 DisabledWarnings 的警告编号，这些编号在之前的版本中有效，但已被移除。</span><span class="sxs-lookup"><span data-stu-id="fa38a-165">The compiler silently ignores warning numbers passed to **DisabledWarnings** that were valid in previous releases, but that have been removed.</span></span> <span data-ttu-id="fa38a-166">例如，CS0679 在 Visual Studio .NET 2002 的编译器中有效，但后来已被移除。</span><span class="sxs-lookup"><span data-stu-id="fa38a-166">For example, *CS0679* was valid in the compiler in Visual Studio .NET 2002 but was removed later.</span></span>

 <span data-ttu-id="fa38a-167">通过 DisabledWarnings 选项不能禁止显示以下警告：</span><span class="sxs-lookup"><span data-stu-id="fa38a-167">The following warnings cannot be suppressed by the **DisabledWarnings** option:</span></span>

- <span data-ttu-id="fa38a-168">编译器警告（等级 1）CS2002</span><span class="sxs-lookup"><span data-stu-id="fa38a-168">Compiler Warning (level 1) CS2002</span></span>  
- <span data-ttu-id="fa38a-169">编译器警告（等级 1）CS2023</span><span class="sxs-lookup"><span data-stu-id="fa38a-169">Compiler Warning (level 1) CS2023</span></span>
- <span data-ttu-id="fa38a-170">编译器警告（等级 1）CS2029</span><span class="sxs-lookup"><span data-stu-id="fa38a-170">Compiler Warning (level 1) CS2029</span></span>

## <a name="codeanalysisruleset"></a><span data-ttu-id="fa38a-171">CodeAnalysisRuleSet</span><span class="sxs-lookup"><span data-stu-id="fa38a-171">CodeAnalysisRuleSet</span></span>

<span data-ttu-id="fa38a-172">指定可配置特定诊断的规则集文件。</span><span class="sxs-lookup"><span data-stu-id="fa38a-172">Specify a ruleset file that configures specific diagnostics.</span></span>

```xml
<CodeAnalysisRuleSet>MyConfiguration.ruleset</CodeAnalysisRuleSet>
```

<span data-ttu-id="fa38a-173">其中 `MyConfiguration.ruleset` 是规则集文件的路径。</span><span class="sxs-lookup"><span data-stu-id="fa38a-173">Where `MyConfiguration.ruleset` is the path to the ruleset file.</span></span> <span data-ttu-id="fa38a-174">有关使用规则集的详细信息，请参阅[有关规则集的 Visual Studio 文档](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules)中的文章。</span><span class="sxs-lookup"><span data-stu-id="fa38a-174">For more information on using rule sets, see the article in the [Visual Studio documentation on Rule sets](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules).</span></span>

## <a name="errorlog"></a><span data-ttu-id="fa38a-175">ErrorLog</span><span class="sxs-lookup"><span data-stu-id="fa38a-175">ErrorLog</span></span>

<span data-ttu-id="fa38a-176">指定要记录所有编译器和分析器诊断的文件。</span><span class="sxs-lookup"><span data-stu-id="fa38a-176">Specify a file to log all compiler and analyzer diagnostics.</span></span>

```xml
<ErrorLog>MyConfiguration.ruleset</ErrorLog>
```

<span data-ttu-id="fa38a-177">ErrorLog 选项会导致编译器输出[静态分析结果交换格式 (SARIF) 日志](https://github.com/microsoft/sarif-tutorials/blob/main/docs/1-Introduction.md#:~:text=What%20is%20SARIF%3F,for%20use%20by%20simpler%20tools)。</span><span class="sxs-lookup"><span data-stu-id="fa38a-177">The **ErrorLog** option causes the compiler to output a [Static Analysis Results Interchange Format (SARIF) log](https://github.com/microsoft/sarif-tutorials/blob/main/docs/1-Introduction.md#:~:text=What%20is%20SARIF%3F,for%20use%20by%20simpler%20tools).</span></span> <span data-ttu-id="fa38a-178">SARIF 日志通常由分析编译器和分析器诊断结果的工具来读取。</span><span class="sxs-lookup"><span data-stu-id="fa38a-178">SARIF logs are typically read by tools that analyze the results from compiler and analyzer diagnostics.</span></span>

## <a name="reportanalyzer"></a><span data-ttu-id="fa38a-179">ReportAnalyzer</span><span class="sxs-lookup"><span data-stu-id="fa38a-179">ReportAnalyzer</span></span>

<span data-ttu-id="fa38a-180">报告其他分析器信息，如执行时间。</span><span class="sxs-lookup"><span data-stu-id="fa38a-180">Report additional analyzer information, such as execution time.</span></span>

```xml
<ReportAnalyzer>true</ReportAnalyzer>
```

<span data-ttu-id="fa38a-181">ReportAnalyzer 选项会导致编译器发出额外的 MSBuild 日志信息，这些信息详细说明生成中分析器的性能特征。</span><span class="sxs-lookup"><span data-stu-id="fa38a-181">The **ReportAnalyzer** option causes the compiler to emit extra MSBuild log information that details the performance characteristics of analyzers in the build.</span></span> <span data-ttu-id="fa38a-182">它通常由分析器作者在验证分析器时使用。</span><span class="sxs-lookup"><span data-stu-id="fa38a-182">It's typically used by analyzer authors as part of validating the analyzer.</span></span>
