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
# <a name="overview-of-net-source-code-analysis"></a><span data-ttu-id="e46a3-103">.NET 源代码分析概述</span><span class="sxs-lookup"><span data-stu-id="e46a3-103">Overview of .NET source code analysis</span></span>

<span data-ttu-id="e46a3-104">.NET Compiler Platform (Roslyn) 分析器会检查 C# 或 Visual Basic 代码的代码质量和样式问题。</span><span class="sxs-lookup"><span data-stu-id="e46a3-104">.NET compiler platform (Roslyn) analyzers inspect your C# or Visual Basic code for code quality and style issues.</span></span> <span data-ttu-id="e46a3-105">从 .NET 5.0 开始，这些分析器包含在 .NET SDK 中，无需单独安装。</span><span class="sxs-lookup"><span data-stu-id="e46a3-105">Starting in .NET 5.0, these analyzers are included with the .NET SDK and you don't need to install them separately.</span></span> <span data-ttu-id="e46a3-106">如果项目面向 .NET 5 或更高版本，则默认启用代码分析。</span><span class="sxs-lookup"><span data-stu-id="e46a3-106">If your project targets .NET 5 or later, code analysis is enabled by default.</span></span> <span data-ttu-id="e46a3-107">如果项目面向不同的 .NET 实现（例如 .NET Core、.NET Standard 或 .NET Framework），则必须通过将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性设置为 `true` 以手动启用代码分析。</span><span class="sxs-lookup"><span data-stu-id="e46a3-107">If your project targets a different .NET implementation, for example, .NET Core, .NET Standard, or .NET Framework, you must manually enable code analysis by setting the [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) property to `true`.</span></span>

<span data-ttu-id="e46a3-108">如果你不想移动到 .NET 5+ SDK、具有非 SDK 样式的 .NET Framework 项目或更倾向于使用基于 NuGet 包的模型，则也可以在 [Microsoft.CodeAnalysis.NetAnalyzers NuGet 包](https://www.nuget.org/packages/Microsoft.CodeAnalysis.NetAnalyzers)中使用该分析器。</span><span class="sxs-lookup"><span data-stu-id="e46a3-108">If you don't want to move to the .NET 5+ SDK, have a non-SDK-style .NET Framework project, or prefer a NuGet package-based model, the analyzers are also available in the [Microsoft.CodeAnalysis.NetAnalyzers NuGet package](https://www.nuget.org/packages/Microsoft.CodeAnalysis.NetAnalyzers).</span></span> <span data-ttu-id="e46a3-109">对于按需版本更新，你可能更倾向于使用基于包的模型。</span><span class="sxs-lookup"><span data-stu-id="e46a3-109">You might prefer a package-based model for on-demand version updates.</span></span>

> [!NOTE]
> <span data-ttu-id="e46a3-110">.NET 分析器与目标框架无关。</span><span class="sxs-lookup"><span data-stu-id="e46a3-110">.NET analyzers are target-framework agnostic.</span></span> <span data-ttu-id="e46a3-111">即，你的项目不需要面向特定的 .NET 实现。</span><span class="sxs-lookup"><span data-stu-id="e46a3-111">That is, your project does not need to target a specific .NET implementation.</span></span> <span data-ttu-id="e46a3-112">分析器适用于面向 `net5.0` 及早期 .NET 版本（如 `netcoreapp3.1` 和 `net472`）的项目。</span><span class="sxs-lookup"><span data-stu-id="e46a3-112">The analyzers work for projects that target `net5.0` as well as earlier .NET versions, such as `netcoreapp3.1` and `net472`.</span></span> <span data-ttu-id="e46a3-113">但是，若要使用 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性启用代码分析，则项目必须引用[项目 SDK](../../core/project-sdk/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-113">However, to enable code analysis using the [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) property, your project must reference a [project SDK](../../core/project-sdk/overview.md).</span></span>

<span data-ttu-id="e46a3-114">如果分析器发现规则冲突，则这些冲突会被报告为建议、警告或错误，具体取决于每个规则的[配置](configuration-options.md)方式。</span><span class="sxs-lookup"><span data-stu-id="e46a3-114">If rule violations are found by an analyzer, they're reported as a suggestion, warning, or error, depending on how each rule is [configured](configuration-options.md).</span></span> <span data-ttu-id="e46a3-115">代码分析冲突以前缀“CA”或“IDE”显示，以便将它们与编译器错误区分开来。</span><span class="sxs-lookup"><span data-stu-id="e46a3-115">Code analysis violations appear with the prefix "CA" or "IDE" to differentiate them from compiler errors.</span></span>

## <a name="code-quality-analysis"></a><span data-ttu-id="e46a3-116">代码质量分析</span><span class="sxs-lookup"><span data-stu-id="e46a3-116">Code quality analysis</span></span>

<span data-ttu-id="e46a3-117">代码质量分析（“CAxxxx”）规则检查 C# 或 Visual Basic 代码的安全性、性能、设计及其他问题。</span><span class="sxs-lookup"><span data-stu-id="e46a3-117">*Code quality analysis* ("CAxxxx") rules inspect your C# or Visual Basic code for security, performance, design and other issues.</span></span> <span data-ttu-id="e46a3-118">分析功能针对面向 .NET 5.0 或更高版本的项目默认启用。</span><span class="sxs-lookup"><span data-stu-id="e46a3-118">Analysis is enabled, by default, for projects that target .NET 5.0 or later.</span></span> <span data-ttu-id="e46a3-119">可通过将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性设置为 `true`，在面向 .NET 早期版本的项目上启用代码分析。</span><span class="sxs-lookup"><span data-stu-id="e46a3-119">You can enable code analysis on projects that target earlier .NET versions by setting the [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) property to `true`.</span></span> <span data-ttu-id="e46a3-120">你也可通过将 `EnableNETAnalyzers` 设置为 `false`，对项目禁用代码分析。</span><span class="sxs-lookup"><span data-stu-id="e46a3-120">You can also disable code analysis for your project by setting `EnableNETAnalyzers` to `false`.</span></span>

> [!TIP]
> <span data-ttu-id="e46a3-121">如果使用 Visual Studio：</span><span class="sxs-lookup"><span data-stu-id="e46a3-121">If you're using Visual Studio:</span></span>
>
> - <span data-ttu-id="e46a3-122">许多分析器规则都有相关的代码修补程序，可以应用它们来纠正问题。</span><span class="sxs-lookup"><span data-stu-id="e46a3-122">Many analyzer rules have associated *code fixes* that you can apply to correct the problem.</span></span> <span data-ttu-id="e46a3-123">代码修补程序显示在灯泡图标菜单中。</span><span class="sxs-lookup"><span data-stu-id="e46a3-123">Code fixes are shown in the light bulb icon menu.</span></span>
> - <span data-ttu-id="e46a3-124">可通过在解决方案资源管理器中右键单击某个项目，然后选择“属性” > “代码分析”选项卡 >“启用 .NET 分析器”来启用或禁用代码分析  。</span><span class="sxs-lookup"><span data-stu-id="e46a3-124">You can enable or disable code analysis by right-clicking on a project in Solution Explorer and selecting **Properties** > **Code Analysis** tab > **Enable .NET analyzers**.</span></span>

### <a name="enabled-rules"></a><span data-ttu-id="e46a3-125">已启用的规则</span><span class="sxs-lookup"><span data-stu-id="e46a3-125">Enabled rules</span></span>

<span data-ttu-id="e46a3-126">在 .NET 5.0 中，以下规则默认启用。</span><span class="sxs-lookup"><span data-stu-id="e46a3-126">The following rules are enabled, by default, in .NET 5.0.</span></span>

| <span data-ttu-id="e46a3-127">诊断 ID</span><span class="sxs-lookup"><span data-stu-id="e46a3-127">Diagnostic ID</span></span> | <span data-ttu-id="e46a3-128">类别</span><span class="sxs-lookup"><span data-stu-id="e46a3-128">Category</span></span> | <span data-ttu-id="e46a3-129">严重性</span><span class="sxs-lookup"><span data-stu-id="e46a3-129">Severity</span></span> | <span data-ttu-id="e46a3-130">说明</span><span class="sxs-lookup"><span data-stu-id="e46a3-130">Description</span></span> |
| - | - | - | - |
| [<span data-ttu-id="e46a3-131">CA1416</span><span class="sxs-lookup"><span data-stu-id="e46a3-131">CA1416</span></span>](/visualstudio/code-quality/ca1416) | <span data-ttu-id="e46a3-132">互操作性</span><span class="sxs-lookup"><span data-stu-id="e46a3-132">Interoperability</span></span> | <span data-ttu-id="e46a3-133">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-133">Warning</span></span> | <span data-ttu-id="e46a3-134">平台兼容性分析器</span><span class="sxs-lookup"><span data-stu-id="e46a3-134">Platform compatibility analyzer</span></span> |
| [<span data-ttu-id="e46a3-135">CA1417</span><span class="sxs-lookup"><span data-stu-id="e46a3-135">CA1417</span></span>](/visualstudio/code-quality/ca1417) | <span data-ttu-id="e46a3-136">互操作性</span><span class="sxs-lookup"><span data-stu-id="e46a3-136">Interoperability</span></span> | <span data-ttu-id="e46a3-137">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-137">Warning</span></span> | <span data-ttu-id="e46a3-138">请勿对 P/Invokes 的字符串参数使用 `OutAttribute`</span><span class="sxs-lookup"><span data-stu-id="e46a3-138">Do not use `OutAttribute` on string parameters for P/Invokes</span></span> |
| [<span data-ttu-id="e46a3-139">CA1831</span><span class="sxs-lookup"><span data-stu-id="e46a3-139">CA1831</span></span>](/visualstudio/code-quality/ca1831) | <span data-ttu-id="e46a3-140">性能</span><span class="sxs-lookup"><span data-stu-id="e46a3-140">Performance</span></span> | <span data-ttu-id="e46a3-141">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-141">Warning</span></span> | <span data-ttu-id="e46a3-142">在合适的情况下，对字符串使用 `AsSpan` 而不是基于范围的索引器</span><span class="sxs-lookup"><span data-stu-id="e46a3-142">Use `AsSpan` instead of range-based indexers for string when appropriate</span></span> |
| [<span data-ttu-id="e46a3-143">CA2013</span><span class="sxs-lookup"><span data-stu-id="e46a3-143">CA2013</span></span>](/visualstudio/code-quality/ca2013) | <span data-ttu-id="e46a3-144">可靠性</span><span class="sxs-lookup"><span data-stu-id="e46a3-144">Reliability</span></span> | <span data-ttu-id="e46a3-145">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-145">Warning</span></span> | <span data-ttu-id="e46a3-146">请勿将 `ReferenceEquals` 与值类型结合使用</span><span class="sxs-lookup"><span data-stu-id="e46a3-146">Do not use `ReferenceEquals` with value types</span></span> |
| [<span data-ttu-id="e46a3-147">CA2014</span><span class="sxs-lookup"><span data-stu-id="e46a3-147">CA2014</span></span>](/visualstudio/code-quality/ca2014) | <span data-ttu-id="e46a3-148">可靠性</span><span class="sxs-lookup"><span data-stu-id="e46a3-148">Reliability</span></span> | <span data-ttu-id="e46a3-149">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-149">Warning</span></span> | <span data-ttu-id="e46a3-150">请勿在循环中使用 `stackalloc`</span><span class="sxs-lookup"><span data-stu-id="e46a3-150">Do not use `stackalloc` in loops</span></span> |
| [<span data-ttu-id="e46a3-151">CA2015</span><span class="sxs-lookup"><span data-stu-id="e46a3-151">CA2015</span></span>](/visualstudio/code-quality/ca2015) | <span data-ttu-id="e46a3-152">可靠性</span><span class="sxs-lookup"><span data-stu-id="e46a3-152">Reliability</span></span> | <span data-ttu-id="e46a3-153">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-153">Warning</span></span> | <span data-ttu-id="e46a3-154">请勿为派生自 <xref:System.Buffers.MemoryManager%601> 的类型定义终结器</span><span class="sxs-lookup"><span data-stu-id="e46a3-154">Do not define finalizers for types derived from <xref:System.Buffers.MemoryManager%601></span></span> |
| [<span data-ttu-id="e46a3-155">CA2200</span><span class="sxs-lookup"><span data-stu-id="e46a3-155">CA2200</span></span>](/visualstudio/code-quality/ca2200) | <span data-ttu-id="e46a3-156">使用情况</span><span class="sxs-lookup"><span data-stu-id="e46a3-156">Usage</span></span> | <span data-ttu-id="e46a3-157">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-157">Warning</span></span> | <span data-ttu-id="e46a3-158">再次引发以保留堆栈详细信息</span><span class="sxs-lookup"><span data-stu-id="e46a3-158">Rethrow to preserve stack details</span></span>
| [<span data-ttu-id="e46a3-159">CA2247</span><span class="sxs-lookup"><span data-stu-id="e46a3-159">CA2247</span></span>](/visualstudio/code-quality/ca2247) | <span data-ttu-id="e46a3-160">使用情况</span><span class="sxs-lookup"><span data-stu-id="e46a3-160">Usage</span></span> | <span data-ttu-id="e46a3-161">警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-161">Warning</span></span> | <span data-ttu-id="e46a3-162">传递到 TaskCompletionSource 构造函数的参数应为 <xref:System.Threading.Tasks.TaskCreationOptions> 枚举，而不是 <xref:System.Threading.Tasks.TaskContinuationOptions></span><span class="sxs-lookup"><span data-stu-id="e46a3-162">Argument passed to TaskCompletionSource constructor should be <xref:System.Threading.Tasks.TaskCreationOptions> enum instead of <xref:System.Threading.Tasks.TaskContinuationOptions></span></span> |

<span data-ttu-id="e46a3-163">可更改这些规则的严重性，以禁用这些规则或将它们提升为错误。</span><span class="sxs-lookup"><span data-stu-id="e46a3-163">You can change the severity of these rules to disable them or elevate them to errors.</span></span> <span data-ttu-id="e46a3-164">也可[启用更多规则](#enable-additional-rules)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-164">You can also [enable more rules](#enable-additional-rules).</span></span>

- <span data-ttu-id="e46a3-165">有关每个 .NET SDK 版本附带的规则的列表，请参阅[分析器版本](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-165">For a list of rules that are included with each .NET SDK version, see [Analyzer releases](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md).</span></span>
- <span data-ttu-id="e46a3-166">有关所有代码质量规则的列表，请参阅[代码质量规则](quality-rules/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-166">For a list of all the code quality rules, see [Code quality rules](quality-rules/index.md).</span></span>

### <a name="enable-additional-rules"></a><span data-ttu-id="e46a3-167">启用其他规则</span><span class="sxs-lookup"><span data-stu-id="e46a3-167">Enable additional rules</span></span>

<span data-ttu-id="e46a3-168">分析模式指预定义的代码分析配置，在此配置下，未启用任何规则、启用某些规则或启用所有规则。</span><span class="sxs-lookup"><span data-stu-id="e46a3-168">*Analysis mode* refers to a predefined code analysis configuration where none, some, or all rules are enabled.</span></span> <span data-ttu-id="e46a3-169">在默认分析模式下，只有少量规则[作为生成警告启用](#enabled-rules)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-169">In the default analysis mode, only a small number of rules are [enabled as build warnings](#enabled-rules).</span></span> <span data-ttu-id="e46a3-170">可通过在项目文件中设置 [\<AnalysisMode>](../../core/project-sdk/msbuild-props.md#analysismode) 属性来更改项目的分析模式。</span><span class="sxs-lookup"><span data-stu-id="e46a3-170">You can change the analysis mode for your project by setting the [\<AnalysisMode>](../../core/project-sdk/msbuild-props.md#analysismode) property in the project file.</span></span> <span data-ttu-id="e46a3-171">允许的值为：</span><span class="sxs-lookup"><span data-stu-id="e46a3-171">The allowable values are:</span></span>

| <span data-ttu-id="e46a3-172">值</span><span class="sxs-lookup"><span data-stu-id="e46a3-172">Value</span></span> | <span data-ttu-id="e46a3-173">说明</span><span class="sxs-lookup"><span data-stu-id="e46a3-173">Description</span></span> |
| - | - |
| `AllDisabledByDefault` | <span data-ttu-id="e46a3-174">这是最保守的模式。</span><span class="sxs-lookup"><span data-stu-id="e46a3-174">This is the most conservative mode.</span></span> <span data-ttu-id="e46a3-175">默认情况下，禁用所有规则。</span><span class="sxs-lookup"><span data-stu-id="e46a3-175">All rules are disabled by default.</span></span> <span data-ttu-id="e46a3-176">可以选择[选择加入](configuration-options.md)各条规则，以启用它们。</span><span class="sxs-lookup"><span data-stu-id="e46a3-176">You can selectively [opt into](configuration-options.md) individual rules to enable them.</span></span><br /><br />`<AnalysisMode>AllDisabledByDefault</AnalysisMode>` |
| `AllEnabledByDefault` | <span data-ttu-id="e46a3-177">这是最激进的模式。</span><span class="sxs-lookup"><span data-stu-id="e46a3-177">This is the most aggressive mode.</span></span> <span data-ttu-id="e46a3-178">所有规则作为生成警告启用。</span><span class="sxs-lookup"><span data-stu-id="e46a3-178">All rules are enabled as build warnings.</span></span> <span data-ttu-id="e46a3-179">可选择性地[选择退出](configuration-options.md)各条规则以禁用它们。</span><span class="sxs-lookup"><span data-stu-id="e46a3-179">You can selectively [opt out of](configuration-options.md) individual rules to disable them.</span></span><br /><br />`<AnalysisMode>AllEnabledByDefault</AnalysisMode>` |
| `Default` | <span data-ttu-id="e46a3-180">在默认模式下，少量规则作为警告启用、其他规则仅作为带有相应代码修补程序的 Visual Studio IDE 建议启用，而其余规则被完全禁用。</span><span class="sxs-lookup"><span data-stu-id="e46a3-180">The default mode, where a handful of rules are enabled as warnings, others are enabled only as Visual Studio IDE suggestions with corresponding code fixes, and the rest are disabled completely.</span></span> <span data-ttu-id="e46a3-181">可选择性地[选择加入或退出](configuration-options.md)各条规则以禁用它们。</span><span class="sxs-lookup"><span data-stu-id="e46a3-181">You can selectively [opt into or out of](configuration-options.md) individual rules to disable them.</span></span><br /><br />`<AnalysisMode>Default</AnalysisMode>` |

<span data-ttu-id="e46a3-182">若要查找每个可用规则的默认严重性以及了解规则是否在默认分析模式下启用，请参阅[规则列表](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-182">To find the default severity for each available rule and whether or not the rule is enabled in the default analysis mode, see the [full list of rules](https://github.com/dotnet/roslyn-analyzers/blob/main/src/NetAnalyzers/Core/AnalyzerReleases.Shipped.md).</span></span>

### <a name="treat-warnings-as-errors"></a><span data-ttu-id="e46a3-183">视警告为错误</span><span class="sxs-lookup"><span data-stu-id="e46a3-183">Treat warnings as errors</span></span>

<span data-ttu-id="e46a3-184">如果在生成项目时使用 `-warnaserror` 标志，则所有代码分析警告也会被视为错误。</span><span class="sxs-lookup"><span data-stu-id="e46a3-184">If you use the `-warnaserror` flag when you build your projects, all code analysis warnings are also treated as errors.</span></span> <span data-ttu-id="e46a3-185">如果不希望在出现 `-warnaserror` 时将代码质量警告 (CAxxxx) 视为错误，可在项目文件中将 `CodeAnalysisTreatWarningsAsErrors` MSBuild 属性设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="e46a3-185">If you do not want code quality warnings (CAxxxx) to be treated as errors in presence of `-warnaserror`, you can set the `CodeAnalysisTreatWarningsAsErrors` MSBuild property to `false` in your project file.</span></span>

```xml
<PropertyGroup>
  <CodeAnalysisTreatWarningsAsErrors>false</CodeAnalysisTreatWarningsAsErrors>
</PropertyGroup>
```

<span data-ttu-id="e46a3-186">你仍会看到任何代码分析警告，但它们不会中断生成。</span><span class="sxs-lookup"><span data-stu-id="e46a3-186">You'll still see any code analysis warnings, but they won't break your build.</span></span>

### <a name="latest-updates"></a><span data-ttu-id="e46a3-187">最新更新</span><span class="sxs-lookup"><span data-stu-id="e46a3-187">Latest updates</span></span>

<span data-ttu-id="e46a3-188">默认情况下，在升级到较新版本的 .NET SDK 时，你将获得最新的代码分析规则和默认规则严重性。</span><span class="sxs-lookup"><span data-stu-id="e46a3-188">By default, you'll get the latest code analysis rules and default rule severities as you upgrade to newer versions of the .NET SDK.</span></span> <span data-ttu-id="e46a3-189">如果你不希望出现此行为（例如，如果你想要确保未启用或禁用任何新规则），可通过以下方式之一来替代此行为：</span><span class="sxs-lookup"><span data-stu-id="e46a3-189">If you don't want this behavior, for example, if you want to ensure that no new rules are enabled or disabled, you can override it in one of the following ways:</span></span>

- <span data-ttu-id="e46a3-190">将 `AnalysisLevel` MSBuild 属性设置为特定值，以将警告锁定到相应的集。</span><span class="sxs-lookup"><span data-stu-id="e46a3-190">Set the `AnalysisLevel` MSBuild property to a specific value to lock the warnings to that set.</span></span> <span data-ttu-id="e46a3-191">在升级到较新的 SDK 时，你仍会获得针对这些警告的 bug 修补程序，但系统不会启用新的警告，也不会禁用现有的警告。</span><span class="sxs-lookup"><span data-stu-id="e46a3-191">When you upgrade to a newer SDK, you'll still get bug fixes for those warnings, but no new warnings will be enabled and no existing warnings will be disabled.</span></span> <span data-ttu-id="e46a3-192">例如，若要将规则集锁定为随 .NET SDK 5.0 版本一起提供的规则集，请向项目文件添加以下条目。</span><span class="sxs-lookup"><span data-stu-id="e46a3-192">For example, to lock the set of rules to those that ship with version 5.0 of the .NET SDK, add the following entry to your project file.</span></span>

  ```xml
  <PropertyGroup>
    <AnalysisLevel>5.0</AnalysisLevel>
  </PropertyGroup>
  ```

  > [!TIP]
  > <span data-ttu-id="e46a3-193">`AnalysisLevel` 属性的默认值为 `latest`，这意味着在你移动到较新版本的 .NET SDK 时，你始终会获得最新的代码分析规则。</span><span class="sxs-lookup"><span data-stu-id="e46a3-193">The default value for the `AnalysisLevel` property is `latest`, which means you always get the latest code analysis rules as you move to newer versions of the .NET SDK.</span></span>

  <span data-ttu-id="e46a3-194">如需了解详细信息，以及查看可能值的列表，请参阅[AnalysisLevel](../../core/project-sdk/msbuild-props.md#analysislevel)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-194">For more information, and to see a list of possible values, see [AnalysisLevel](../../core/project-sdk/msbuild-props.md#analysislevel).</span></span>

- <span data-ttu-id="e46a3-195">安装 [Microsoft.CodeAnalysis.NetAnalyzers NuGet 包](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers)，将规则更新与 .NET SDK 更新分离。</span><span class="sxs-lookup"><span data-stu-id="e46a3-195">Install the [Microsoft.CodeAnalysis.NetAnalyzers NuGet package](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) to decouple rule updates from .NET SDK updates.</span></span> <span data-ttu-id="e46a3-196">对于面向 .NET 5+ 的项目，安装该包将关闭内置 SDK 分析器。</span><span class="sxs-lookup"><span data-stu-id="e46a3-196">For projects that target .NET 5+, installing the package turns off the built-in SDK analyzers.</span></span> <span data-ttu-id="e46a3-197">如果 SDK 所含的分析器程序集版本比 NuGet 包所含的版本更新，你会收到生成警告。</span><span class="sxs-lookup"><span data-stu-id="e46a3-197">You'll get a build warning if the SDK contains a newer analyzer assembly version than that of the NuGet package.</span></span> <span data-ttu-id="e46a3-198">若要禁用该警告，请将 `_SkipUpgradeNetAnalyzersNuGetWarning` 属性设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="e46a3-198">To disable the warning, set the `_SkipUpgradeNetAnalyzersNuGetWarning` property to `true`.</span></span>

  > [!NOTE]
  > <span data-ttu-id="e46a3-199">如果你安装了 Microsoft.CodeAnalysis.NetAnalyzers NuGet 包，则不应将 [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) 属性添加到项目文件或 Directory.Build.props 文件。</span><span class="sxs-lookup"><span data-stu-id="e46a3-199">If you install the Microsoft.CodeAnalysis.NetAnalyzers NuGet package, you should not add the [EnableNETAnalyzers](../../core/project-sdk/msbuild-props.md#enablenetanalyzers) property to either your project file or a *Directory.Build.props* file.</span></span> <span data-ttu-id="e46a3-200">在安装了 NuGet 包并将 `EnableNETAnalyzers` 属性设置为 `true` 时，一个生成警告随即生成。</span><span class="sxs-lookup"><span data-stu-id="e46a3-200">When the NuGet package is installed and the `EnableNETAnalyzers` property is set to `true`, a build warning is generated.</span></span>

## <a name="code-style-analysis"></a><span data-ttu-id="e46a3-201">代码样式分析</span><span class="sxs-lookup"><span data-stu-id="e46a3-201">Code-style analysis</span></span>

<span data-ttu-id="e46a3-202">通过代码样式分析（“IDExxxx”）规则，可在代码库中定义和维护一致的代码样式。</span><span class="sxs-lookup"><span data-stu-id="e46a3-202">*Code-style analysis* ("IDExxxx") rules enable you to define and maintain consistent code style in your codebase.</span></span> <span data-ttu-id="e46a3-203">默认的启用设置为：</span><span class="sxs-lookup"><span data-stu-id="e46a3-203">The default enablement settings are:</span></span>

- <span data-ttu-id="e46a3-204">命令行生成：默认情况下，对命令行生成上的所有 .NET 项目禁用代码样式分析。</span><span class="sxs-lookup"><span data-stu-id="e46a3-204">Command-line build: Code-style analysis is disabled, by default, for all .NET projects on command-line builds.</span></span>

  <span data-ttu-id="e46a3-205">从 .NET 5.0 开始，无论是在命令行还是在 Visual Studio 内，你都可以[在生成时启用代码样式分析](#enable-on-build)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-205">Starting in .NET 5.0, you can [enable code-style analysis on build](#enable-on-build), both at the command line and inside Visual Studio.</span></span> <span data-ttu-id="e46a3-206">代码样式冲突显示为带有“IDE”前缀的警告或错误。</span><span class="sxs-lookup"><span data-stu-id="e46a3-206">Code style violations appear as warnings or errors with an "IDE" prefix.</span></span> <span data-ttu-id="e46a3-207">这使你能够在生成时强制执行一致的代码样式。</span><span class="sxs-lookup"><span data-stu-id="e46a3-207">This enables you to enforce consistent code styles at build time.</span></span>

- <span data-ttu-id="e46a3-208">Visual Studio：默认情况下，代码样式分析作为[代码重构快速操作](/visualstudio/ide/code-generation-in-visual-studio)对 Visual Studio 中的所有 .NET 项目启用。</span><span class="sxs-lookup"><span data-stu-id="e46a3-208">Visual Studio: Code-style analysis is enabled, by default, for all .NET projects inside Visual Studio as [code refactoring quick actions](/visualstudio/ide/code-generation-in-visual-studio).</span></span>

<span data-ttu-id="e46a3-209">有关代码样式分析规则的完整列表，请参阅[代码样式规则](style-rules/index.md)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-209">For a full list of code-style analysis rules, see [Code style rules](style-rules/index.md).</span></span>

### <a name="enable-on-build"></a><span data-ttu-id="e46a3-210">生成时启用</span><span class="sxs-lookup"><span data-stu-id="e46a3-210">Enable on build</span></span>

<span data-ttu-id="e46a3-211">通过 .NET 5.0 SDK 和更高版本，可在从命令行和 Visual Studio 生成时启用代码样式分析。</span><span class="sxs-lookup"><span data-stu-id="e46a3-211">With the .NET 5.0 SDK and later versions, you can enable code-style analysis when building from the command-line and in Visual Studio.</span></span> <span data-ttu-id="e46a3-212">（然而，出于性能方面的原因，[一些代码样式规则](https://github.com/dotnet/roslyn/blob/9f87b444da9c48a4d492b19f8337339056bf2b95/src/Analyzers/Core/Analyzers/EnforceOnBuildValues.cs#L95)仍仅适用于 Visual Studio IDE。）</span><span class="sxs-lookup"><span data-stu-id="e46a3-212">(However, for performance reasons, [a handful of code-style rules](https://github.com/dotnet/roslyn/blob/9f87b444da9c48a4d492b19f8337339056bf2b95/src/Analyzers/Core/Analyzers/EnforceOnBuildValues.cs#L95) will still apply only in the Visual Studio IDE.)</span></span>

<span data-ttu-id="e46a3-213">执行以下步骤，在生成时启用代码样式分析：</span><span class="sxs-lookup"><span data-stu-id="e46a3-213">Follow these steps to enable code-style analysis on build:</span></span>

1. <span data-ttu-id="e46a3-214">将 MSBuild 属性 [EnforceCodeStyleInBuild](../../core/project-sdk/msbuild-props.md#enforcecodestyleinbuild) 设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="e46a3-214">Set the MSBuild property [EnforceCodeStyleInBuild](../../core/project-sdk/msbuild-props.md#enforcecodestyleinbuild) to `true`.</span></span>

1. <span data-ttu-id="e46a3-215">在 .editorconfig 文件中，[配置](configuration-options.md)你希望在生成时作为警告或错误运行的每个“IDE”代码样式规则。</span><span class="sxs-lookup"><span data-stu-id="e46a3-215">In an *.editorconfig* file, [configure](configuration-options.md) each "IDE" code style rule that you wish to run on build as a warning or an error.</span></span> <span data-ttu-id="e46a3-216">例如：</span><span class="sxs-lookup"><span data-stu-id="e46a3-216">For example:</span></span>

   ```ini
   [*.{cs,vb}]
   # IDE0040: Accessibility modifiers required (escalated to a build warning)
   dotnet_diagnostic.IDE0040.severity = warning
   ```

   <span data-ttu-id="e46a3-217">或者，可将整个类别默认配置为警告或错误，然后选择性地禁用该类别中你不希望在生成时运行的规则。</span><span class="sxs-lookup"><span data-stu-id="e46a3-217">Alternatively, you can configure an entire category to be a warning or error, by default, and then selectively turn off rules in that category that you don't want to run on build.</span></span> <span data-ttu-id="e46a3-218">例如：</span><span class="sxs-lookup"><span data-stu-id="e46a3-218">For example:</span></span>

   ```ini
   [*.{cs,vb}]

   # Default severity for analyzer diagnostics with category 'Style' (escalated to build warnings)
   dotnet_analyzer_diagnostic.category-Style.severity = warning

   # IDE0040: Accessibility modifiers required (disabled on build)
   dotnet_diagnostic.IDE0040.severity = silent
   ```

> [!NOTE]
> <span data-ttu-id="e46a3-219">代码样式分析功能为实验性功能，可能会在 .NET 5 和 .NET 6 版本之间发生更改。</span><span class="sxs-lookup"><span data-stu-id="e46a3-219">The code-style analysis feature is experimental and may change between the .NET 5 and .NET 6 releases.</span></span>

## <a name="suppress-a-warning"></a><span data-ttu-id="e46a3-220">抑制警告</span><span class="sxs-lookup"><span data-stu-id="e46a3-220">Suppress a warning</span></span>

<span data-ttu-id="e46a3-221">一种抑制规则冲突的方法是在 EditorConfig 文件中将该规则 ID 的严重性选项设置为 `none`。</span><span class="sxs-lookup"><span data-stu-id="e46a3-221">One way to suppress a rule violation is to set the severity option for that rule ID to `none` in an EditorConfig file.</span></span> <span data-ttu-id="e46a3-222">例如：</span><span class="sxs-lookup"><span data-stu-id="e46a3-222">For example:</span></span>

```ini
dotnet_diagnostic.CA1822.severity = none
```

<span data-ttu-id="e46a3-223">有关抑制警告的详细信息和其他方式，请参阅[如何抑制代码分析警告](suppress-warnings.md)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-223">For more information and other ways to suppress warnings, see [How to suppress code analysis warnings](suppress-warnings.md).</span></span>

## <a name="third-party-analyzers"></a><span data-ttu-id="e46a3-224">第三方分析器</span><span class="sxs-lookup"><span data-stu-id="e46a3-224">Third-party analyzers</span></span>

<span data-ttu-id="e46a3-225">除了官方 .NET 分析器外，你也可以安装第三方分析器，如 [StyleCop](https://www.nuget.org/packages/StyleCop.Analyzers/)、[Roslynator](https://www.nuget.org/packages/Roslynator.Analyzers/)、[XUnit Analyzers](https://www.nuget.org/packages/xunit.analyzers/) 和 [Sonar Analyzer](https://www.nuget.org/packages/SonarAnalyzer.CSharp/)。</span><span class="sxs-lookup"><span data-stu-id="e46a3-225">In addition to the official .NET analyzers, you can also install third party analyzers, such as [StyleCop](https://www.nuget.org/packages/StyleCop.Analyzers/), [Roslynator](https://www.nuget.org/packages/Roslynator.Analyzers/), [XUnit Analyzers](https://www.nuget.org/packages/xunit.analyzers/), and [Sonar Analyzer](https://www.nuget.org/packages/SonarAnalyzer.CSharp/).</span></span>

## <a name="see-also"></a><span data-ttu-id="e46a3-226">另请参阅</span><span class="sxs-lookup"><span data-stu-id="e46a3-226">See also</span></span>

- [<span data-ttu-id="e46a3-227">代码质量分析规则引用</span><span class="sxs-lookup"><span data-stu-id="e46a3-227">Code quality analysis rule reference</span></span>](quality-rules/index.md)
- [<span data-ttu-id="e46a3-228">代码样式分析规则引用</span><span class="sxs-lookup"><span data-stu-id="e46a3-228">Code style analysis rule reference</span></span>](style-rules/index.md)
- [<span data-ttu-id="e46a3-229">Visual Studio 中的代码分析</span><span class="sxs-lookup"><span data-stu-id="e46a3-229">Code analysis in Visual Studio</span></span>](/visualstudio/code-quality/roslyn-analyzers-overview)
- [<span data-ttu-id="e46a3-230">.NET 编译器平台 SDK</span><span class="sxs-lookup"><span data-stu-id="e46a3-230">.NET Compiler Platform SDK</span></span>](../../csharp/roslyn-sdk/index.md)
- [<span data-ttu-id="e46a3-231">教程：编写第一个分析器和代码修补程序</span><span class="sxs-lookup"><span data-stu-id="e46a3-231">Tutorial: Write your first analyzer and code fix</span></span>](../../csharp/roslyn-sdk/tutorials/how-to-write-csharp-analyzer-code-fix.md)
