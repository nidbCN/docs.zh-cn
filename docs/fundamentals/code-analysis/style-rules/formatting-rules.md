---
title: 代码样式格式设置规则
description: 了解用于设置缩进、空格和换行格式的代码样式规则。
ms.date: 09/25/2020
ms.topic: reference
author: gewarren
ms.author: gewarren
dev_langs:
- CSharp
- VB
f1_keywords:
- IDE0055
- formatting rules
helpviewer_keywords:
- IDE0055
- formatting code style rules [EditorConfig]
- formatting rules
- EditorConfig formatting conventions
ms.openlocfilehash: 866949692341f65a5b78c7dd5b8eec918873d3b7
ms.sourcegitcommit: 1d3af230ec30d8d061be7a887f6ba38a530c4ece
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511796"
---
# <a name="formatting-rules"></a><span data-ttu-id="28d0b-103">格式设置规则</span><span class="sxs-lookup"><span data-stu-id="28d0b-103">Formatting rules</span></span>

<span data-ttu-id="28d0b-104">格式设置规则会影响 .NET 编程语言构造的缩进、空格和换行的排列方式。</span><span class="sxs-lookup"><span data-stu-id="28d0b-104">Formatting rules affect how indentation, spaces, and new lines are aligned around .NET programming language constructs.</span></span> <span data-ttu-id="28d0b-105">规则分为以下几类：</span><span class="sxs-lookup"><span data-stu-id="28d0b-105">The rules fall into the following categories:</span></span>

- <span data-ttu-id="28d0b-106">[.NET 格式设置规则](#net-formatting-rules)：适用于 C# 和 Visual Basic 的规则。</span><span class="sxs-lookup"><span data-stu-id="28d0b-106">[.NET formatting rules](#net-formatting-rules): Rules that apply to both C# and Visual Basic.</span></span> <span data-ttu-id="28d0b-107">这些规则的 EditorConfig 选项名称以 `dotnet_` 前缀开头。</span><span class="sxs-lookup"><span data-stu-id="28d0b-107">The EditorConfig option names for these rules start with `dotnet_` prefix.</span></span>
- <span data-ttu-id="28d0b-108">[C# 格式设置规则](#c-formatting-rules)：仅适用于 C# 语言的规则。</span><span class="sxs-lookup"><span data-stu-id="28d0b-108">[C# formatting rules](#c-formatting-rules): Rules that are specific to C# language only.</span></span> <span data-ttu-id="28d0b-109">这些规则的 EditorConfig 选项名称以 `csharp_` 前缀开头。</span><span class="sxs-lookup"><span data-stu-id="28d0b-109">The EditorConfig option names for these rules start with `csharp_` prefix.</span></span>

## <a name="rule-id-ide0055-fix-formatting"></a><span data-ttu-id="28d0b-110">规则 ID：“IDE0055”（修复格式设置）</span><span class="sxs-lookup"><span data-stu-id="28d0b-110">Rule ID: "IDE0055" (Fix formatting)</span></span>

<span data-ttu-id="28d0b-111">所有格式设置选项都具有规则 ID `IDE0055` 和标题 `Fix formatting`。</span><span class="sxs-lookup"><span data-stu-id="28d0b-111">All formatting options have rule ID `IDE0055` and title `Fix formatting`.</span></span> <span data-ttu-id="28d0b-112">使用以下配置行在 EditorConfig 文件中设置格式设置冲突的严重性。</span><span class="sxs-lookup"><span data-stu-id="28d0b-112">Set the severity of a formatting violation in an EditorConfig file using the following configuration line.</span></span>

```ini
dotnet_diagnostic.IDE0055.severity = <severity value>
```

<span data-ttu-id="28d0b-113">严重性值必须是 `warning` 或 `error` 才能[在生成时强制执行](../overview.md#code-style-analysis)。</span><span class="sxs-lookup"><span data-stu-id="28d0b-113">The severity value must be `warning` or `error` to be [enforced on build](../overview.md#code-style-analysis).</span></span> <span data-ttu-id="28d0b-114">要了解所有可能的严重性值，请参阅[严重性级别](../configuration-options.md#severity-level)。</span><span class="sxs-lookup"><span data-stu-id="28d0b-114">For all possible severity values, see [severity level](../configuration-options.md#severity-level).</span></span>

## <a name="option-format"></a><span data-ttu-id="28d0b-115">选项格式</span><span class="sxs-lookup"><span data-stu-id="28d0b-115">Option format</span></span>

<span data-ttu-id="28d0b-116">可以在 EditorConfig 文件中指定格式设置规则的选项，格式如下：</span><span class="sxs-lookup"><span data-stu-id="28d0b-116">Options for formatting rules can be specified in an EditorConfig file with the following format:</span></span>

`rule_name = value`

<span data-ttu-id="28d0b-117">对于许多规则，可为 `value` 指定 `true`（以此样式为首选项）或 `false`（不以此样式为首选项）。</span><span class="sxs-lookup"><span data-stu-id="28d0b-117">For many rules, you specify either `true` (prefer this style) or `false` (do not prefer this style) for `value`.</span></span> <span data-ttu-id="28d0b-118">对于其他规则，可指定值（如 `flush_left` 或 `before_and_after`）来说明在什么时间以及在什么位置应用此规则。</span><span class="sxs-lookup"><span data-stu-id="28d0b-118">For other rules, you specify a value such as `flush_left` or `before_and_after` to describe when and where to apply the rule.</span></span> <span data-ttu-id="28d0b-119">不需要指定严重性。</span><span class="sxs-lookup"><span data-stu-id="28d0b-119">You don't specify a severity.</span></span>

## <a name="net-formatting-rules"></a><span data-ttu-id="28d0b-120">.NET 格式设置规则</span><span class="sxs-lookup"><span data-stu-id="28d0b-120">.NET formatting rules</span></span>

<span data-ttu-id="28d0b-121">本节中的格式设置规则适用于 C# 和 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="28d0b-121">The formatting rules in this section apply to both C# and Visual Basic.</span></span>

- [<span data-ttu-id="28d0b-122">组织 using</span><span class="sxs-lookup"><span data-stu-id="28d0b-122">Organize usings</span></span>](#organize-using-directives)
  - <span data-ttu-id="28d0b-123">dotnet_sort_system_directives_first</span><span class="sxs-lookup"><span data-stu-id="28d0b-123">dotnet_sort_system_directives_first</span></span>
  - <span data-ttu-id="28d0b-124">dotnet_separate_import_directive_groups</span><span class="sxs-lookup"><span data-stu-id="28d0b-124">dotnet_separate_import_directive_groups</span></span>

### <a name="organize-using-directives"></a><span data-ttu-id="28d0b-125">组织 using 指令</span><span class="sxs-lookup"><span data-stu-id="28d0b-125">Organize using directives</span></span>

<span data-ttu-id="28d0b-126">这些格式设置规则与 `using` 指令和 `Imports` 语句的排序和显示有关。</span><span class="sxs-lookup"><span data-stu-id="28d0b-126">These formatting rules concern the sorting and display of `using` directives and `Imports` statements.</span></span>

<span data-ttu-id="28d0b-127">.editorconfig 文件示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-127">Example *.editorconfig* file:</span></span>

```ini
# .NET formatting rules
[*.{cs,vb}]
dotnet_sort_system_directives_first = true
dotnet_separate_import_directive_groups = true
```

#### <a name="dotnet_sort_system_directives_first"></a><span data-ttu-id="28d0b-128">dotnet\_sort\_system\_directives_first</span><span class="sxs-lookup"><span data-stu-id="28d0b-128">dotnet\_sort\_system\_directives_first</span></span>

|<span data-ttu-id="28d0b-129">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-129">Property</span></span>|<span data-ttu-id="28d0b-130">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-130">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-131">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-131">**Option name**</span></span> | <span data-ttu-id="28d0b-132">dotnet_sort_system_directives_first</span><span class="sxs-lookup"><span data-stu-id="28d0b-132">dotnet_sort_system_directives_first</span></span> |
| <span data-ttu-id="28d0b-133">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-133">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-134">C# 和 Visual Basic</span><span class="sxs-lookup"><span data-stu-id="28d0b-134">C# and Visual Basic</span></span> |
| <span data-ttu-id="28d0b-135">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-135">**Introduced version**</span></span> | <span data-ttu-id="28d0b-136">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-136">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-137">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-137">**Option values**</span></span> | <span data-ttu-id="28d0b-138">`true` - 按字母顺序对 `System.*` `using` 指令排序，并将它们放在其他 using 指令前面。</span><span class="sxs-lookup"><span data-stu-id="28d0b-138">`true` - Sort `System.*` `using` directives alphabetically, and place them before other using directives.</span></span><br /><br /><span data-ttu-id="28d0b-139">`false` - 不要将 `System.*` `using` 指令放在其他 `using` 指令前面。</span><span class="sxs-lookup"><span data-stu-id="28d0b-139">`false` - Do not place `System.*` `using` directives before other `using` directives.</span></span> |

<span data-ttu-id="28d0b-140">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-140">Code examples:</span></span>

```csharp
// dotnet_sort_system_directives_first = true
using System.Collections.Generic;
using System.Threading.Tasks;
using Octokit;

// dotnet_sort_system_directives_first = false
using System.Collections.Generic;
using Octokit;
using System.Threading.Tasks;
```

#### <a name="dotnet_separate_import_directive_groups"></a><span data-ttu-id="28d0b-141">dotnet\_separate\_import\_directive\_groups</span><span class="sxs-lookup"><span data-stu-id="28d0b-141">dotnet\_separate\_import\_directive\_groups</span></span>

|<span data-ttu-id="28d0b-142">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-142">Property</span></span>|<span data-ttu-id="28d0b-143">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-143">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-144">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-144">**Option name**</span></span> | <span data-ttu-id="28d0b-145">dotnet_separate_import_directive_groups</span><span class="sxs-lookup"><span data-stu-id="28d0b-145">dotnet_separate_import_directive_groups</span></span> |
| <span data-ttu-id="28d0b-146">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-146">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-147">C# 和 Visual Basic</span><span class="sxs-lookup"><span data-stu-id="28d0b-147">C# and Visual Basic</span></span> |
| <span data-ttu-id="28d0b-148">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-148">**Introduced version**</span></span> | <span data-ttu-id="28d0b-149">Visual Studio 2017 版本 15.5</span><span class="sxs-lookup"><span data-stu-id="28d0b-149">Visual Studio 2017 version 15.5</span></span> |
| <span data-ttu-id="28d0b-150">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-150">**Option values**</span></span> | <span data-ttu-id="28d0b-151">`true` - 在 `using` 指令组之间放置一个空白行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-151">`true` - Place a blank line between `using` directive groups.</span></span><br /><br /><span data-ttu-id="28d0b-152">`false` - 不在 `using` 指令组之间放置空白行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-152">`false` - Do not place a blank line between `using` directive groups.</span></span> |

<span data-ttu-id="28d0b-153">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-153">Code examples:</span></span>

```csharp
// dotnet_separate_import_directive_groups = true
using System.Collections.Generic;
using System.Threading.Tasks;

using Octokit;

// dotnet_separate_import_directive_groups = false
using System.Collections.Generic;
using System.Threading.Tasks;
using Octokit;
```

## <a name="c-formatting-rules"></a><span data-ttu-id="28d0b-154">C# 格式设置规则</span><span class="sxs-lookup"><span data-stu-id="28d0b-154">C# formatting rules</span></span>

<span data-ttu-id="28d0b-155">本节中的格式设置规则仅适用于 C# 代码。</span><span class="sxs-lookup"><span data-stu-id="28d0b-155">The formatting rules in this section apply only to C# code.</span></span>

- [<span data-ttu-id="28d0b-156">换行符选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-156">Newline options</span></span>](#new-line-options)
  - <span data-ttu-id="28d0b-157">csharp_new_line_before_open_brace</span><span class="sxs-lookup"><span data-stu-id="28d0b-157">csharp_new_line_before_open_brace</span></span>
  - <span data-ttu-id="28d0b-158">csharp_new_line_before_else</span><span class="sxs-lookup"><span data-stu-id="28d0b-158">csharp_new_line_before_else</span></span>
  - <span data-ttu-id="28d0b-159">csharp_new_line_before_catch</span><span class="sxs-lookup"><span data-stu-id="28d0b-159">csharp_new_line_before_catch</span></span>
  - <span data-ttu-id="28d0b-160">csharp_new_line_before_finally</span><span class="sxs-lookup"><span data-stu-id="28d0b-160">csharp_new_line_before_finally</span></span>
  - <span data-ttu-id="28d0b-161">csharp_new_line_before_members_in_object_initializers</span><span class="sxs-lookup"><span data-stu-id="28d0b-161">csharp_new_line_before_members_in_object_initializers</span></span>
  - <span data-ttu-id="28d0b-162">csharp_new_line_before_members_in_anonymous_types</span><span class="sxs-lookup"><span data-stu-id="28d0b-162">csharp_new_line_before_members_in_anonymous_types</span></span>
  - <span data-ttu-id="28d0b-163">csharp_new_line_between_query_expression_clauses</span><span class="sxs-lookup"><span data-stu-id="28d0b-163">csharp_new_line_between_query_expression_clauses</span></span>
- [<span data-ttu-id="28d0b-164">缩进选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-164">Indentation options</span></span>](#indentation-options)
  - <span data-ttu-id="28d0b-165">csharp_indent_case_contents</span><span class="sxs-lookup"><span data-stu-id="28d0b-165">csharp_indent_case_contents</span></span>
  - <span data-ttu-id="28d0b-166">csharp_indent_switch_labels</span><span class="sxs-lookup"><span data-stu-id="28d0b-166">csharp_indent_switch_labels</span></span>
  - <span data-ttu-id="28d0b-167">csharp_indent_labels</span><span class="sxs-lookup"><span data-stu-id="28d0b-167">csharp_indent_labels</span></span>
  - <span data-ttu-id="28d0b-168">csharp_indent_block_contents</span><span class="sxs-lookup"><span data-stu-id="28d0b-168">csharp_indent_block_contents</span></span>
  - <span data-ttu-id="28d0b-169">csharp_indent_braces</span><span class="sxs-lookup"><span data-stu-id="28d0b-169">csharp_indent_braces</span></span>
  - <span data-ttu-id="28d0b-170">csharp_indent_case_contents_when_block</span><span class="sxs-lookup"><span data-stu-id="28d0b-170">csharp_indent_case_contents_when_block</span></span>
- [<span data-ttu-id="28d0b-171">间距选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-171">Spacing options</span></span>](#spacing-options)
  - <span data-ttu-id="28d0b-172">csharp_space_after_cast</span><span class="sxs-lookup"><span data-stu-id="28d0b-172">csharp_space_after_cast</span></span>
  - <span data-ttu-id="28d0b-173">csharp_space_after_keywords_in_control_flow_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-173">csharp_space_after_keywords_in_control_flow_statements</span></span>
  - <span data-ttu-id="28d0b-174">csharp_space_between_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-174">csharp_space_between_parentheses</span></span>
  - <span data-ttu-id="28d0b-175">csharp_space_before_colon_in_inheritance_clause</span><span class="sxs-lookup"><span data-stu-id="28d0b-175">csharp_space_before_colon_in_inheritance_clause</span></span>
  - <span data-ttu-id="28d0b-176">csharp_space_after_colon_in_inheritance_clause</span><span class="sxs-lookup"><span data-stu-id="28d0b-176">csharp_space_after_colon_in_inheritance_clause</span></span>
  - <span data-ttu-id="28d0b-177">csharp_space_around_binary_operators</span><span class="sxs-lookup"><span data-stu-id="28d0b-177">csharp_space_around_binary_operators</span></span>
  - <span data-ttu-id="28d0b-178">csharp_space_between_method_declaration_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-178">csharp_space_between_method_declaration_parameter_list_parentheses</span></span>
  - <span data-ttu-id="28d0b-179">csharp_space_between_method_declaration_empty_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-179">csharp_space_between_method_declaration_empty_parameter_list_parentheses</span></span>
  - <span data-ttu-id="28d0b-180">csharp_space_between_method_declaration_name_and_open_parenthesis</span><span class="sxs-lookup"><span data-stu-id="28d0b-180">csharp_space_between_method_declaration_name_and_open_parenthesis</span></span>
  - <span data-ttu-id="28d0b-181">csharp_space_between_method_call_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-181">csharp_space_between_method_call_parameter_list_parentheses</span></span>
  - <span data-ttu-id="28d0b-182">csharp_space_between_method_call_empty_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-182">csharp_space_between_method_call_empty_parameter_list_parentheses</span></span>
  - <span data-ttu-id="28d0b-183">csharp_space_between_method_call_name_and_opening_parenthesis</span><span class="sxs-lookup"><span data-stu-id="28d0b-183">csharp_space_between_method_call_name_and_opening_parenthesis</span></span>
  - <span data-ttu-id="28d0b-184">csharp_space_after_comma</span><span class="sxs-lookup"><span data-stu-id="28d0b-184">csharp_space_after_comma</span></span>
  - <span data-ttu-id="28d0b-185">csharp_space_before_comma</span><span class="sxs-lookup"><span data-stu-id="28d0b-185">csharp_space_before_comma</span></span>
  - <span data-ttu-id="28d0b-186">csharp_space_after_dot</span><span class="sxs-lookup"><span data-stu-id="28d0b-186">csharp_space_after_dot</span></span>
  - <span data-ttu-id="28d0b-187">csharp_space_before_dot</span><span class="sxs-lookup"><span data-stu-id="28d0b-187">csharp_space_before_dot</span></span>
  - <span data-ttu-id="28d0b-188">csharp_space_after_semicolon_in_for_statement</span><span class="sxs-lookup"><span data-stu-id="28d0b-188">csharp_space_after_semicolon_in_for_statement</span></span>
  - <span data-ttu-id="28d0b-189">csharp_space_before_semicolon_in_for_statement</span><span class="sxs-lookup"><span data-stu-id="28d0b-189">csharp_space_before_semicolon_in_for_statement</span></span>
  - <span data-ttu-id="28d0b-190">csharp_space_around_declaration_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-190">csharp_space_around_declaration_statements</span></span>
  - <span data-ttu-id="28d0b-191">csharp_space_before_open_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-191">csharp_space_before_open_square_brackets</span></span>
  - <span data-ttu-id="28d0b-192">csharp_space_between_empty_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-192">csharp_space_between_empty_square_brackets</span></span>
  - <span data-ttu-id="28d0b-193">csharp_space_between_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-193">csharp_space_between_square_brackets</span></span>
- [<span data-ttu-id="28d0b-194">换行选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-194">Wrap options</span></span>](#wrap-options)
  - <span data-ttu-id="28d0b-195">csharp_preserve_single_line_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-195">csharp_preserve_single_line_statements</span></span>
  - <span data-ttu-id="28d0b-196">csharp_preserve_single_line_blocks</span><span class="sxs-lookup"><span data-stu-id="28d0b-196">csharp_preserve_single_line_blocks</span></span>
- [<span data-ttu-id="28d0b-197">sing 指令选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-197">Using directive options</span></span>](#using-directive-options)
  - <span data-ttu-id="28d0b-198">csharp_using_directive_placement</span><span class="sxs-lookup"><span data-stu-id="28d0b-198">csharp_using_directive_placement</span></span>

### <a name="new-line-options"></a><span data-ttu-id="28d0b-199">换行选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-199">New-line options</span></span>

<span data-ttu-id="28d0b-200">这些格式设置规则与是否使用新行设置代码的格式有关。</span><span class="sxs-lookup"><span data-stu-id="28d0b-200">These formatting rules concern the use of new lines to format code.</span></span>

<span data-ttu-id="28d0b-201">.editorconfig 文件示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-201">Example *.editorconfig* file:</span></span>

```ini
# CSharp formatting rules:
[*.cs]
csharp_new_line_before_open_brace = methods, properties, control_blocks, types
csharp_new_line_before_else = true
csharp_new_line_before_catch = true
csharp_new_line_before_finally = true
csharp_new_line_before_members_in_object_initializers = true
csharp_new_line_before_members_in_anonymous_types = true
csharp_new_line_between_query_expression_clauses = true
```

#### <a name="csharp_new_line_before_open_brace"></a><span data-ttu-id="28d0b-202">csharp\_new\_line\_before\_open_brace</span><span class="sxs-lookup"><span data-stu-id="28d0b-202">csharp\_new\_line\_before\_open_brace</span></span>

<span data-ttu-id="28d0b-203">此规则与左大括号 `{` 应放在前面代码的同一行还是新行上有关。</span><span class="sxs-lookup"><span data-stu-id="28d0b-203">This rule concerns whether an open brace `{` should be placed on the same line as the preceding code, or on a new line.</span></span> <span data-ttu-id="28d0b-204">对于此规则，指定“全部”、“无”或一个或多个码位元素，如方法或属性，从而定义此规则的应用时间   。</span><span class="sxs-lookup"><span data-stu-id="28d0b-204">For this rule, you specify **all**, **none**, or one or more code elements such as **methods** or **properties**, to define when this rule should be applied.</span></span> <span data-ttu-id="28d0b-205">若要指定多个代码元素，请使用逗号 (,) 分隔。</span><span class="sxs-lookup"><span data-stu-id="28d0b-205">To specify multiple code elements, separate them with a comma (,).</span></span>

|<span data-ttu-id="28d0b-206">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-206">Property</span></span>|<span data-ttu-id="28d0b-207">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-207">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-208">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-208">**Option name**</span></span> | <span data-ttu-id="28d0b-209">csharp_new_line_before_open_brace</span><span class="sxs-lookup"><span data-stu-id="28d0b-209">csharp_new_line_before_open_brace</span></span> |
| <span data-ttu-id="28d0b-210">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-210">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-211">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-211">C#</span></span> |
| <span data-ttu-id="28d0b-212">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-212">**Introduced version**</span></span> | <span data-ttu-id="28d0b-213">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-213">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-214">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-214">**Option values**</span></span> | <span data-ttu-id="28d0b-215">`all` - 对于所有表达式，需要将大括号置于新行（“Allman”样式）。</span><span class="sxs-lookup"><span data-stu-id="28d0b-215">`all` - Require braces to be on a new line for all expressions ("Allman" style).</span></span><br /><br /><span data-ttu-id="28d0b-216">`none` - 对于所有表达式，需要将大括号置于同一行（“K&R”）。</span><span class="sxs-lookup"><span data-stu-id="28d0b-216">`none` - Require braces to be on the same line for all expressions ("K&R").</span></span><br /><br /><span data-ttu-id="28d0b-217">`accessors`、`anonymous_methods`、`anonymous_types`、`control_blocks`、`events`、`indexers`、`lambdas`、`local_functions`、`methods`、`object_collection_array_initializers`、`properties`、`types` - 对于指定的代码元素，需要将大括号置于新行（“Allman”样式）。</span><span class="sxs-lookup"><span data-stu-id="28d0b-217">`accessors`, `anonymous_methods`, `anonymous_types`, `control_blocks`, `events`, `indexers`, `lambdas`, `local_functions`, `methods`, `object_collection_array_initializers`, `properties`, `types` - Require braces to be on a new line for the specified code element ("Allman" style).</span></span> |

<span data-ttu-id="28d0b-218">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-218">Code examples:</span></span>

```csharp
// csharp_new_line_before_open_brace = all
void MyMethod()
{
    if (...)
    {
        ...
    }
}

// csharp_new_line_before_open_brace = none
void MyMethod() {
    if (...) {
        ...
    }
}
```

#### <a name="csharp_new_line_before_else"></a><span data-ttu-id="28d0b-219">csharp\_new\_line\_before_else</span><span class="sxs-lookup"><span data-stu-id="28d0b-219">csharp\_new\_line\_before_else</span></span>

|<span data-ttu-id="28d0b-220">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-220">Property</span></span>|<span data-ttu-id="28d0b-221">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-221">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-222">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-222">**Option name**</span></span> | <span data-ttu-id="28d0b-223">csharp_new_line_before_else</span><span class="sxs-lookup"><span data-stu-id="28d0b-223">csharp_new_line_before_else</span></span> |
| <span data-ttu-id="28d0b-224">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-224">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-225">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-225">C#</span></span> |
| <span data-ttu-id="28d0b-226">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-226">**Introduced version**</span></span> | <span data-ttu-id="28d0b-227">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-227">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-228">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-228">**Option values**</span></span> | <span data-ttu-id="28d0b-229">`true` - 将 `else` 语句置于新行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-229">`true` - Place `else` statements on a new line.</span></span><br /><br /><span data-ttu-id="28d0b-230">`false` - 将 `else` 语句置于同一行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-230">`false` - Place `else` statements on the same line.</span></span> |

<span data-ttu-id="28d0b-231">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-231">Code examples:</span></span>

```csharp
// csharp_new_line_before_else = true
if (...) {
    ...
}
else {
    ...
}

// csharp_new_line_before_else = false
if (...) {
    ...
} else {
    ...
}
```

#### <a name="csharp_new_line_before_catch"></a><span data-ttu-id="28d0b-232">csharp\_new\_line\_before_catch</span><span class="sxs-lookup"><span data-stu-id="28d0b-232">csharp\_new\_line\_before_catch</span></span>

|<span data-ttu-id="28d0b-233">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-233">Property</span></span>|<span data-ttu-id="28d0b-234">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-234">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-235">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-235">**Option name**</span></span> | <span data-ttu-id="28d0b-236">csharp_new_line_before_catch</span><span class="sxs-lookup"><span data-stu-id="28d0b-236">csharp_new_line_before_catch</span></span> |
| <span data-ttu-id="28d0b-237">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-237">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-238">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-238">C#</span></span> |
| <span data-ttu-id="28d0b-239">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-239">**Introduced version**</span></span> | <span data-ttu-id="28d0b-240">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-240">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-241">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-241">**Option values**</span></span> | <span data-ttu-id="28d0b-242">`true` - 将 `catch` 语句置于新行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-242">`true` - Place `catch` statements on a new line.</span></span><br /><br /><span data-ttu-id="28d0b-243">`false` - 将 `catch` 语句置于同一行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-243">`false` - Place `catch` statements on the same line.</span></span> |

<span data-ttu-id="28d0b-244">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-244">Code examples:</span></span>

```csharp
// csharp_new_line_before_catch = true
try {
    ...
}
catch (Exception e) {
    ...
}

// csharp_new_line_before_catch = false
try {
    ...
} catch (Exception e) {
    ...
}
```

#### <a name="csharp_new_line_before_finally"></a><span data-ttu-id="28d0b-245">csharp\_new\_line\_before_finally</span><span class="sxs-lookup"><span data-stu-id="28d0b-245">csharp\_new\_line\_before_finally</span></span>

|<span data-ttu-id="28d0b-246">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-246">Property</span></span>|<span data-ttu-id="28d0b-247">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-247">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-248">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-248">**Option name**</span></span> | <span data-ttu-id="28d0b-249">csharp_new_line_before_finally</span><span class="sxs-lookup"><span data-stu-id="28d0b-249">csharp_new_line_before_finally</span></span> |
| <span data-ttu-id="28d0b-250">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-250">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-251">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-251">C#</span></span> |
| <span data-ttu-id="28d0b-252">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-252">**Introduced version**</span></span> | <span data-ttu-id="28d0b-253">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-253">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-254">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-254">**Option values**</span></span> | <span data-ttu-id="28d0b-255">`true` - 需要将 `finally` 语句置于右大括号后的新行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-255">`true` - Require `finally` statements to be on a new line after the closing brace.</span></span><br /><br /><span data-ttu-id="28d0b-256">`false` - 需要将 `finally` 语句置于右大括号所在的同一行。</span><span class="sxs-lookup"><span data-stu-id="28d0b-256">`false` - Require `finally` statements to be on the same line as the closing brace.</span></span> |

<span data-ttu-id="28d0b-257">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-257">Code examples:</span></span>

```csharp
// csharp_new_line_before_finally = true
try {
    ...
}
catch (Exception e) {
    ...
}
finally {
    ...
}

// csharp_new_line_before_finally = false
try {
    ...
} catch (Exception e) {
    ...
} finally {
    ...
}
```

#### <a name="csharp_new_line_before_members_in_object_initializers"></a><span data-ttu-id="28d0b-258">csharp\_new\_line\_before\_members\_in\_object_initializers</span><span class="sxs-lookup"><span data-stu-id="28d0b-258">csharp\_new\_line\_before\_members\_in\_object_initializers</span></span>

|<span data-ttu-id="28d0b-259">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-259">Property</span></span>|<span data-ttu-id="28d0b-260">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-260">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-261">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-261">**Option name**</span></span> | <span data-ttu-id="28d0b-262">csharp_new_line_before_members_in_object_initializers</span><span class="sxs-lookup"><span data-stu-id="28d0b-262">csharp_new_line_before_members_in_object_initializers</span></span> |
| <span data-ttu-id="28d0b-263">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-263">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-264">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-264">C#</span></span> |
| <span data-ttu-id="28d0b-265">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-265">**Introduced version**</span></span> | <span data-ttu-id="28d0b-266">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-266">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-267">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-267">**Option values**</span></span> | <span data-ttu-id="28d0b-268">`true` - 需要将对象初始值设定项的成员置于单独的行</span><span class="sxs-lookup"><span data-stu-id="28d0b-268">`true` - Require members of object initializers to be on separate lines</span></span><br /><br /><span data-ttu-id="28d0b-269">`false` - 需要将对象初始值设定项的成员置于同一行</span><span class="sxs-lookup"><span data-stu-id="28d0b-269">`false` - Require members of object initializers to be on the same line</span></span> |

<span data-ttu-id="28d0b-270">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-270">Code examples:</span></span>

```csharp
// csharp_new_line_before_members_in_object_initializers = true
var z = new B()
{
    A = 3,
    B = 4
}

// csharp_new_line_before_members_in_object_initializers = false
var z = new B()
{
    A = 3, B = 4
}
```

#### <a name="csharp_new_line_before_members_in_anonymous_types"></a><span data-ttu-id="28d0b-271">csharp\_new\_line\_before\_members\_in\_anonymous_types</span><span class="sxs-lookup"><span data-stu-id="28d0b-271">csharp\_new\_line\_before\_members\_in\_anonymous_types</span></span>

|<span data-ttu-id="28d0b-272">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-272">Property</span></span>|<span data-ttu-id="28d0b-273">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-273">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-274">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-274">**Option name**</span></span> | <span data-ttu-id="28d0b-275">csharp_new_line_before_members_in_anonymous_types</span><span class="sxs-lookup"><span data-stu-id="28d0b-275">csharp_new_line_before_members_in_anonymous_types</span></span> |
| <span data-ttu-id="28d0b-276">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-276">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-277">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-277">C#</span></span> |
| <span data-ttu-id="28d0b-278">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-278">**Introduced version**</span></span> | <span data-ttu-id="28d0b-279">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-279">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-280">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-280">**Option values**</span></span> | <span data-ttu-id="28d0b-281">`true` - 需要将匿名类型的成员置于单独的行</span><span class="sxs-lookup"><span data-stu-id="28d0b-281">`true` - Require members of anonymous types to be on separate lines</span></span><br /><br /><span data-ttu-id="28d0b-282">`false` - 需要将匿名类型的成员置于同一行</span><span class="sxs-lookup"><span data-stu-id="28d0b-282">`false` - Require members of anonymous types to be on the same line</span></span> |

<span data-ttu-id="28d0b-283">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-283">Code examples:</span></span>

```csharp
// csharp_new_line_before_members_in_anonymous_types = true
var z = new
{
    A = 3,
    B = 4
}

// csharp_new_line_before_members_in_anonymous_types = false
var z = new
{
    A = 3, B = 4
}
```

#### <a name="csharp_new_line_between_query_expression_clauses"></a><span data-ttu-id="28d0b-284">csharp_new_line_between_query_expression_clauses</span><span class="sxs-lookup"><span data-stu-id="28d0b-284">csharp_new_line_between_query_expression_clauses</span></span>

|<span data-ttu-id="28d0b-285">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-285">Property</span></span>|<span data-ttu-id="28d0b-286">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-286">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-287">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-287">**Option name**</span></span> | <span data-ttu-id="28d0b-288">csharp_new_line_between_query_expression_clauses</span><span class="sxs-lookup"><span data-stu-id="28d0b-288">csharp_new_line_between_query_expression_clauses</span></span> |
| <span data-ttu-id="28d0b-289">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-289">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-290">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-290">C#</span></span> |
| <span data-ttu-id="28d0b-291">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-291">**Introduced version**</span></span> | <span data-ttu-id="28d0b-292">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-292">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-293">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-293">**Option values**</span></span> | <span data-ttu-id="28d0b-294">`true` - 要求将查询表达式子句的元素置于单独的行</span><span class="sxs-lookup"><span data-stu-id="28d0b-294">`true` - Require elements of query expression clauses to be on separate lines</span></span><br /><br /><span data-ttu-id="28d0b-295">`false` - 要求将查询表达式子句的元素置于同一行</span><span class="sxs-lookup"><span data-stu-id="28d0b-295">`false` - Require elements of query expression clauses to be on the same line</span></span> |

<span data-ttu-id="28d0b-296">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-296">Code examples:</span></span>

```csharp
// csharp_new_line_between_query_expression_clauses = true
var q = from a in e
        from b in e
        select a * b;

// csharp_new_line_between_query_expression_clauses = false
var q = from a in e from b in e
        select a * b;
```

### <a name="indentation-options"></a><span data-ttu-id="28d0b-297">缩进选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-297">Indentation options</span></span>

<span data-ttu-id="28d0b-298">这些格式设置规则与是否使用缩进设置代码的格式有关。</span><span class="sxs-lookup"><span data-stu-id="28d0b-298">These formatting rules concern the use of indentation to format code.</span></span>

<span data-ttu-id="28d0b-299">.editorconfig 文件示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-299">Example *.editorconfig* file:</span></span>

```ini
# CSharp formatting rules:
[*.cs]
csharp_indent_case_contents = true
csharp_indent_switch_labels = true
csharp_indent_labels = flush_left
csharp_indent_block_contents = true
csharp_indent_braces = false
csharp_indent_case_contents_when_block = true
```

#### <a name="csharp_indent_case_contents"></a><span data-ttu-id="28d0b-300">csharp\_indent\_case_contents</span><span class="sxs-lookup"><span data-stu-id="28d0b-300">csharp\_indent\_case_contents</span></span>

|<span data-ttu-id="28d0b-301">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-301">Property</span></span>|<span data-ttu-id="28d0b-302">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-302">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-303">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-303">**Option name**</span></span> | <span data-ttu-id="28d0b-304">csharp_indent_case_contents</span><span class="sxs-lookup"><span data-stu-id="28d0b-304">csharp_indent_case_contents</span></span> |
| <span data-ttu-id="28d0b-305">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-305">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-306">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-306">C#</span></span> |
| <span data-ttu-id="28d0b-307">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-307">**Introduced version**</span></span> | <span data-ttu-id="28d0b-308">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-308">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-309">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-309">**Option values**</span></span> | <span data-ttu-id="28d0b-310">`true` - 缩进 `switch` case 内容</span><span class="sxs-lookup"><span data-stu-id="28d0b-310">`true` - Indent `switch` case contents</span></span><br /><br /><span data-ttu-id="28d0b-311">`false` - 不缩进 `switch` case 内容</span><span class="sxs-lookup"><span data-stu-id="28d0b-311">`false` - Do not indent `switch` case contents</span></span> |

- <span data-ttu-id="28d0b-312">如果此规则设置为“true”，则为 i。</span><span class="sxs-lookup"><span data-stu-id="28d0b-312">When this rule is set to **true**, i.</span></span>
- <span data-ttu-id="28d0b-313">如果此规则设置为“false”，则为 d。</span><span class="sxs-lookup"><span data-stu-id="28d0b-313">When this rule is set to **false**, d.</span></span>

<span data-ttu-id="28d0b-314">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-314">Code examples:</span></span>

```csharp
// csharp_indent_case_contents = true
switch(c) {
    case Color.Red:
        Console.WriteLine("The color is red");
        break;
    case Color.Blue:
        Console.WriteLine("The color is blue");
        break;
    default:
        Console.WriteLine("The color is unknown.");
        break;
}

// csharp_indent_case_contents = false
switch(c) {
    case Color.Red:
    Console.WriteLine("The color is red");
    break;
    case Color.Blue:
    Console.WriteLine("The color is blue");
    break;
    default:
    Console.WriteLine("The color is unknown.");
    break;
}
```

#### <a name="csharp_indent_switch_labels"></a><span data-ttu-id="28d0b-315">csharp\_indent\_switch_labels</span><span class="sxs-lookup"><span data-stu-id="28d0b-315">csharp\_indent\_switch_labels</span></span>

|<span data-ttu-id="28d0b-316">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-316">Property</span></span>|<span data-ttu-id="28d0b-317">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-317">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-318">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-318">**Option name**</span></span> | <span data-ttu-id="28d0b-319">csharp_indent_switch_labels</span><span class="sxs-lookup"><span data-stu-id="28d0b-319">csharp_indent_switch_labels</span></span> |
| <span data-ttu-id="28d0b-320">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-320">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-321">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-321">C#</span></span> |
| <span data-ttu-id="28d0b-322">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-322">**Introduced version**</span></span> | <span data-ttu-id="28d0b-323">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-323">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-324">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-324">**Option values**</span></span> | <span data-ttu-id="28d0b-325">`true` - 缩进 `switch` 标签</span><span class="sxs-lookup"><span data-stu-id="28d0b-325">`true` - Indent `switch` labels</span></span><br /><br /><span data-ttu-id="28d0b-326">`false` - 不缩进 `switch` 标签</span><span class="sxs-lookup"><span data-stu-id="28d0b-326">`false` - Do not indent `switch` labels</span></span> |

<span data-ttu-id="28d0b-327">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-327">Code examples:</span></span>

```csharp
// csharp_indent_switch_labels = true
switch(c) {
    case Color.Red:
        Console.WriteLine("The color is red");
        break;
    case Color.Blue:
        Console.WriteLine("The color is blue");
        break;
    default:
        Console.WriteLine("The color is unknown.");
        break;
}

// csharp_indent_switch_labels = false
switch(c) {
case Color.Red:
    Console.WriteLine("The color is red");
    break;
case Color.Blue:
    Console.WriteLine("The color is blue");
    break;
default:
    Console.WriteLine("The color is unknown.");
    break;
}
```

#### <a name="csharp_indent_labels"></a><span data-ttu-id="28d0b-328">csharp\_indent_labels</span><span class="sxs-lookup"><span data-stu-id="28d0b-328">csharp\_indent_labels</span></span>

|<span data-ttu-id="28d0b-329">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-329">Property</span></span>|<span data-ttu-id="28d0b-330">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-330">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-331">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-331">**Option name**</span></span> | <span data-ttu-id="28d0b-332">csharp_indent_labels</span><span class="sxs-lookup"><span data-stu-id="28d0b-332">csharp_indent_labels</span></span> |
| <span data-ttu-id="28d0b-333">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-333">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-334">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-334">C#</span></span> |
| <span data-ttu-id="28d0b-335">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-335">**Introduced version**</span></span> | <span data-ttu-id="28d0b-336">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-336">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-337">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-337">**Option values**</span></span> | <span data-ttu-id="28d0b-338">`flush_left` - 标签置于最左侧的列</span><span class="sxs-lookup"><span data-stu-id="28d0b-338">`flush_left` - Labels are placed at the leftmost column</span></span><br /><br /><span data-ttu-id="28d0b-339">`one_less_than_current` - 将标签置于比当前上下文少一个缩进的位置</span><span class="sxs-lookup"><span data-stu-id="28d0b-339">`one_less_than_current` - Labels are placed at one less indent to the current context</span></span><br /><br /><span data-ttu-id="28d0b-340">`no_change` - 将标签置于与当前上下文相同的缩进位置</span><span class="sxs-lookup"><span data-stu-id="28d0b-340">`no_change` - Labels are placed at the same indent as the current context</span></span> |

<span data-ttu-id="28d0b-341">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-341">Code examples:</span></span>

```csharp
// csharp_indent_labels= flush_left
class C
{
    private string MyMethod(...)
    {
        if (...) {
            goto error;
        }
error:
        throw new Exception(...);
    }
}

// csharp_indent_labels = one_less_than_current
class C
{
    private string MyMethod(...)
    {
        if (...) {
            goto error;
        }
    error:
        throw new Exception(...);
    }
}

// csharp_indent_labels= no_change
class C
{
    private string MyMethod(...)
    {
        if (...) {
            goto error;
        }
        error:
        throw new Exception(...);
    }
}
```

#### <a name="csharp_indent_block_contents"></a><span data-ttu-id="28d0b-342">csharp_indent_block_contents</span><span class="sxs-lookup"><span data-stu-id="28d0b-342">csharp_indent_block_contents</span></span>

|<span data-ttu-id="28d0b-343">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-343">Property</span></span>|<span data-ttu-id="28d0b-344">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-344">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-345">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-345">**Option name**</span></span> | <span data-ttu-id="28d0b-346">csharp_indent_block_contents</span><span class="sxs-lookup"><span data-stu-id="28d0b-346">csharp_indent_block_contents</span></span> |
| <span data-ttu-id="28d0b-347">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-347">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-348">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-348">C#</span></span> |
| <span data-ttu-id="28d0b-349">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-349">**Option values**</span></span> | `true` - <br /><br />`false` -  |

<span data-ttu-id="28d0b-350">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-350">Code examples:</span></span>

```csharp
// csharp_indent_block_contents = true
static void Hello()
{
    Console.WriteLine("Hello");
}

// csharp_indent_block_contents = false
static void Hello()
{
Console.WriteLine("Hello");
}
```

#### <a name="csharp_indent_braces"></a><span data-ttu-id="28d0b-351">csharp_indent_braces</span><span class="sxs-lookup"><span data-stu-id="28d0b-351">csharp_indent_braces</span></span>

|<span data-ttu-id="28d0b-352">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-352">Property</span></span>|<span data-ttu-id="28d0b-353">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-353">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-354">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-354">**Option name**</span></span> | <span data-ttu-id="28d0b-355">csharp_indent_braces</span><span class="sxs-lookup"><span data-stu-id="28d0b-355">csharp_indent_braces</span></span> |
| <span data-ttu-id="28d0b-356">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-356">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-357">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-357">C#</span></span> |
| <span data-ttu-id="28d0b-358">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-358">**Option values**</span></span> | `true` - <br /><br />`false` -  |

<span data-ttu-id="28d0b-359">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-359">Code examples:</span></span>

```csharp
// csharp_indent_braces = true
static void Hello()
    {
    Console.WriteLine("Hello");
    }

// csharp_indent_braces = false
static void Hello()
{
    Console.WriteLine("Hello");
}
```

#### <a name="csharp_indent_case_contents_when_block"></a><span data-ttu-id="28d0b-360">csharp_indent_case_contents_when_block</span><span class="sxs-lookup"><span data-stu-id="28d0b-360">csharp_indent_case_contents_when_block</span></span>

|<span data-ttu-id="28d0b-361">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-361">Property</span></span>|<span data-ttu-id="28d0b-362">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-362">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-363">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-363">**Option name**</span></span> | <span data-ttu-id="28d0b-364">csharp_indent_case_contents_when_block</span><span class="sxs-lookup"><span data-stu-id="28d0b-364">csharp_indent_case_contents_when_block</span></span> |
| <span data-ttu-id="28d0b-365">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-365">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-366">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-366">C#</span></span> |
| <span data-ttu-id="28d0b-367">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-367">**Option values**</span></span> | `true` - <br /><br />`false` -  |

<span data-ttu-id="28d0b-368">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-368">Code examples:</span></span>

```csharp
// csharp_indent_case_contents_when_block = true
case 0:
    {
        Console.WriteLine("Hello");
        break;
    }

// csharp_indent_case_contents_when_block = false
case 0:
{
    Console.WriteLine("Hello");
    break;
}
```

### <a name="spacing-options"></a><span data-ttu-id="28d0b-369">间距选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-369">Spacing options</span></span>

<span data-ttu-id="28d0b-370">这些格式设置规则与是否使用空格字符设置代码的格式有关。</span><span class="sxs-lookup"><span data-stu-id="28d0b-370">These formatting rules concern the use of space characters to format code.</span></span>

<span data-ttu-id="28d0b-371">.editorconfig 文件示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-371">Example *.editorconfig* file:</span></span>

```ini
# CSharp formatting rules:
[*.cs]
csharp_space_after_cast = true
csharp_space_after_keywords_in_control_flow_statements = true
csharp_space_between_parentheses = control_flow_statements, type_casts
csharp_space_before_colon_in_inheritance_clause = true
csharp_space_after_colon_in_inheritance_clause = true
csharp_space_around_binary_operators = before_and_after
csharp_space_between_method_declaration_parameter_list_parentheses = true
csharp_space_between_method_declaration_empty_parameter_list_parentheses = false
csharp_space_between_method_declaration_name_and_open_parenthesis = false
csharp_space_between_method_call_parameter_list_parentheses = true
csharp_space_between_method_call_empty_parameter_list_parentheses = false
csharp_space_between_method_call_name_and_opening_parenthesis = false
csharp_space_after_comma = true
csharp_space_before_comma = false
csharp_space_after_dot = false
csharp_space_before_dot = false
csharp_space_after_semicolon_in_for_statement = true
csharp_space_before_semicolon_in_for_statement = false
csharp_space_around_declaration_statements = false
csharp_space_before_open_square_brackets = false
csharp_space_between_empty_square_brackets = false
csharp_space_between_square_brackets = false
```

#### <a name="csharp_space_after_cast"></a><span data-ttu-id="28d0b-372">csharp\_space\_after_cast</span><span class="sxs-lookup"><span data-stu-id="28d0b-372">csharp\_space\_after_cast</span></span>

|<span data-ttu-id="28d0b-373">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-373">Property</span></span>|<span data-ttu-id="28d0b-374">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-374">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-375">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-375">**Option name**</span></span> | <span data-ttu-id="28d0b-376">csharp_space_after_cast</span><span class="sxs-lookup"><span data-stu-id="28d0b-376">csharp_space_after_cast</span></span> |
| <span data-ttu-id="28d0b-377">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-377">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-378">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-378">C#</span></span> |
| <span data-ttu-id="28d0b-379">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-379">**Introduced version**</span></span> | <span data-ttu-id="28d0b-380">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-380">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-381">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-381">**Option values**</span></span> | <span data-ttu-id="28d0b-382">`true` - 在强制转换和值之间放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-382">`true` - Place a space character between a cast and the value</span></span><br /><br /><span data-ttu-id="28d0b-383">`false` - 删除转换和值之间的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-383">`false` - Remove space between the cast and the value</span></span> |

<span data-ttu-id="28d0b-384">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-384">Code examples:</span></span>

```csharp
// csharp_space_after_cast = true
int y = (int) x;

// csharp_space_after_cast = false
int y = (int)x;
```

#### <a name="csharp_space_after_keywords_in_control_flow_statements"></a><span data-ttu-id="28d0b-385">csharp_space_after_keywords_in_control_flow_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-385">csharp_space_after_keywords_in_control_flow_statements</span></span>

|<span data-ttu-id="28d0b-386">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-386">Property</span></span>|<span data-ttu-id="28d0b-387">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-387">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-388">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-388">**Option name**</span></span> | <span data-ttu-id="28d0b-389">csharp_space_after_keywords_in_control_flow_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-389">csharp_space_after_keywords_in_control_flow_statements</span></span> |
| <span data-ttu-id="28d0b-390">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-390">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-391">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-391">C#</span></span> |
| <span data-ttu-id="28d0b-392">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-392">**Introduced version**</span></span> | <span data-ttu-id="28d0b-393">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-393">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-394">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-394">**Option values**</span></span> | <span data-ttu-id="28d0b-395">`true` - 在控制流语句（如 `for` 循环）中的关键字后放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-395">`true` - Place a space character after a keyword in a control flow statement such as a `for` loop</span></span><br /><br /><span data-ttu-id="28d0b-396">`false` - 删除控制流语句（如 `for` 循环）中的关键字后的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-396">`false` - Remove space after a keyword in a control flow statement such as a `for` loop</span></span> |

<span data-ttu-id="28d0b-397">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-397">Code examples:</span></span>

```csharp
// csharp_space_after_keywords_in_control_flow_statements = true
for (int i;i<x;i++) { ... }

// csharp_space_after_keywords_in_control_flow_statements = false
for(int i;i<x;i++) { ... }
```

#### <a name="csharp_space_between_parentheses"></a><span data-ttu-id="28d0b-398">csharp_space_between_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-398">csharp_space_between_parentheses</span></span>

|<span data-ttu-id="28d0b-399">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-399">Property</span></span>|<span data-ttu-id="28d0b-400">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-400">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-401">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-401">**Option name**</span></span> | <span data-ttu-id="28d0b-402">csharp_space_between_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-402">csharp_space_between_parentheses</span></span> |
| <span data-ttu-id="28d0b-403">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-403">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-404">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-404">C#</span></span> |
| <span data-ttu-id="28d0b-405">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-405">**Introduced version**</span></span> | <span data-ttu-id="28d0b-406">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-406">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-407">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-407">**Option values**</span></span> | <span data-ttu-id="28d0b-408">`control_flow_statements` - 在控制流语句的括号之间放置空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-408">`control_flow_statements` - Place space between parentheses of control flow statements</span></span><br /><br /><span data-ttu-id="28d0b-409">`expressions` - 在表达式的括号之间放置空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-409">`expressions` - Place space between parentheses of expressions</span></span><br /><br /><span data-ttu-id="28d0b-410">`type_casts` - 在类型转换中的括号之间放置空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-410">`type_casts` - Place space between parentheses in type casts</span></span> |

<span data-ttu-id="28d0b-411">如果省略此规则或使用 `control_flow_statements`、`expressions` 或 `type_casts` 以外的值，则不会应用该设置。</span><span class="sxs-lookup"><span data-stu-id="28d0b-411">If you omit this rule or use a value other than `control_flow_statements`, `expressions`, or `type_casts`, the setting is not applied.</span></span>

<span data-ttu-id="28d0b-412">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-412">Code examples:</span></span>

```csharp
// csharp_space_between_parentheses = control_flow_statements
for ( int i = 0; i < 10; i++ ) { }

// csharp_space_between_parentheses = expressions
var z = ( x * y ) - ( ( y - x ) * 3 );

// csharp_space_between_parentheses = type_casts
int y = ( int )x;
```

#### <a name="csharp_space_before_colon_in_inheritance_clause"></a><span data-ttu-id="28d0b-413">csharp\_space\_before\_colon\_in\_inheritance_clause</span><span class="sxs-lookup"><span data-stu-id="28d0b-413">csharp\_space\_before\_colon\_in\_inheritance_clause</span></span>

|<span data-ttu-id="28d0b-414">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-414">Property</span></span>|<span data-ttu-id="28d0b-415">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-415">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-416">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-416">**Option name**</span></span> | <span data-ttu-id="28d0b-417">csharp_space_before_colon_in_inheritance_clause</span><span class="sxs-lookup"><span data-stu-id="28d0b-417">csharp_space_before_colon_in_inheritance_clause</span></span> |
| <span data-ttu-id="28d0b-418">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-418">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-419">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-419">C#</span></span> |
| <span data-ttu-id="28d0b-420">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-420">**Introduced version**</span></span> | <span data-ttu-id="28d0b-421">Visual Studio 2017 15.7 版</span><span class="sxs-lookup"><span data-stu-id="28d0b-421">Visual Studio 2017 version 15.7</span></span> |
| <span data-ttu-id="28d0b-422">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-422">**Option values**</span></span> | <span data-ttu-id="28d0b-423">`true` - 在类型声明中的基或接口冒号前放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-423">`true` - Place a space character before the colon for bases or interfaces in a type declaration</span></span><br /><br /><span data-ttu-id="28d0b-424">`false` - 删除类型声明中基或接口冒号前的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-424">`false` - Remove space before the colon for bases or interfaces in a type declaration</span></span> |

<span data-ttu-id="28d0b-425">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-425">Code examples:</span></span>

```csharp
// csharp_space_before_colon_in_inheritance_clause = true
interface I
{

}

class C : I
{

}

// csharp_space_before_colon_in_inheritance_clause = false
interface I
{

}

class C: I
{

}
```

#### <a name="csharp_space_after_colon_in_inheritance_clause"></a><span data-ttu-id="28d0b-426">csharp\_space\_after\_colon\_in\_inheritance_clause</span><span class="sxs-lookup"><span data-stu-id="28d0b-426">csharp\_space\_after\_colon\_in\_inheritance_clause</span></span>

|<span data-ttu-id="28d0b-427">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-427">Property</span></span>|<span data-ttu-id="28d0b-428">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-428">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-429">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-429">**Option name**</span></span> | <span data-ttu-id="28d0b-430">csharp_space_after_colon_in_inheritance_clause</span><span class="sxs-lookup"><span data-stu-id="28d0b-430">csharp_space_after_colon_in_inheritance_clause</span></span> |
| <span data-ttu-id="28d0b-431">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-431">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-432">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-432">C#</span></span> |
| <span data-ttu-id="28d0b-433">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-433">**Introduced version**</span></span> | <span data-ttu-id="28d0b-434">Visual Studio 2017 15.7 版</span><span class="sxs-lookup"><span data-stu-id="28d0b-434">Visual Studio 2017 version 15.7</span></span> |
| <span data-ttu-id="28d0b-435">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-435">**Option values**</span></span> | <span data-ttu-id="28d0b-436">`true` - 在类型声明中的基或接口冒号后放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-436">`true` - Place a space character after the colon for bases or interfaces in a type declaration</span></span><br /><br /><span data-ttu-id="28d0b-437">`false` - 删除类型声明中基或接口冒号后的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-437">`false` - Remove space after the colon for bases or interfaces in a type declaration</span></span> |

<span data-ttu-id="28d0b-438">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-438">Code examples:</span></span>

```csharp
// csharp_space_after_colon_in_inheritance_clause = true
interface I
{

}

class C : I
{

}

// csharp_space_after_colon_in_inheritance_clause = false
interface I
{

}

class C :I
{

}
```

#### <a name="csharp_space_around_binary_operators"></a><span data-ttu-id="28d0b-439">csharp\_space\_around\_binary_operators</span><span class="sxs-lookup"><span data-stu-id="28d0b-439">csharp\_space\_around\_binary_operators</span></span>

|<span data-ttu-id="28d0b-440">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-440">Property</span></span>|<span data-ttu-id="28d0b-441">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-441">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-442">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-442">**Option name**</span></span> | <span data-ttu-id="28d0b-443">csharp_space_around_binary_operators</span><span class="sxs-lookup"><span data-stu-id="28d0b-443">csharp_space_around_binary_operators</span></span> |
| <span data-ttu-id="28d0b-444">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-444">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-445">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-445">C#</span></span> |
| <span data-ttu-id="28d0b-446">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-446">**Introduced version**</span></span> | <span data-ttu-id="28d0b-447">Visual Studio 2017 15.7 版</span><span class="sxs-lookup"><span data-stu-id="28d0b-447">Visual Studio 2017 version 15.7</span></span> |
| <span data-ttu-id="28d0b-448">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-448">**Option values**</span></span> | <span data-ttu-id="28d0b-449">`before_and_after` - 在二元运算符前后插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-449">`before_and_after` - Insert space before and after the binary operator</span></span><br /><br /><span data-ttu-id="28d0b-450">`none` - 删除二元运算符前后的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-450">`none` - Remove spaces before and after the binary operator</span></span><br /><br /><span data-ttu-id="28d0b-451">`ignore` - 忽略二元运算符前后的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-451">`ignore` - Ignore spaces around binary operators</span></span> |

<span data-ttu-id="28d0b-452">如果省略此规则或使用 `before_and_after`、`none` 或 `ignore` 以外的值，则不会应用该设置。</span><span class="sxs-lookup"><span data-stu-id="28d0b-452">If you omit this rule, or use a value other than `before_and_after`, `none`, or `ignore`, the setting is not applied.</span></span>

<span data-ttu-id="28d0b-453">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-453">Code examples:</span></span>

```csharp
// csharp_space_around_binary_operators = before_and_after
return x * (x - y);

// csharp_space_around_binary_operators = none
return x*(x-y);

// csharp_space_around_binary_operators = ignore
return x  *  (x-y);
```

#### <a name="csharp_space_between_method_declaration_parameter_list_parentheses"></a><span data-ttu-id="28d0b-454">csharp_space_between_method_declaration_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-454">csharp_space_between_method_declaration_parameter_list_parentheses</span></span>

|<span data-ttu-id="28d0b-455">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-455">Property</span></span>|<span data-ttu-id="28d0b-456">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-456">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-457">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-457">**Option name**</span></span> | <span data-ttu-id="28d0b-458">csharp_space_between_method_declaration_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-458">csharp_space_between_method_declaration_parameter_list_parentheses</span></span> |
| <span data-ttu-id="28d0b-459">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-459">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-460">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-460">C#</span></span> |
| <span data-ttu-id="28d0b-461">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-461">**Introduced version**</span></span> | <span data-ttu-id="28d0b-462">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-462">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-463">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-463">**Option values**</span></span> | <span data-ttu-id="28d0b-464">`true` - 在方法声明参数列表的左括号之后和右括号之前放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-464">`true` - Place a space character after the opening parenthesis and before the closing parenthesis of a method declaration parameter list</span></span><br /><br /><span data-ttu-id="28d0b-465">`false` - 删除方法声明参数列表的左括号之后和右括号之前的空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-465">`false` - Remove space characters after the opening parenthesis and before the closing parenthesis of a method declaration parameter list</span></span> |

<span data-ttu-id="28d0b-466">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-466">Code examples:</span></span>

```csharp
// csharp_space_between_method_declaration_parameter_list_parentheses = true
void Bark( int x ) { ... }

// csharp_space_between_method_declaration_parameter_list_parentheses = false
void Bark(int x) { ... }
```

#### <a name="csharp_space_between_method_declaration_empty_parameter_list_parentheses"></a><span data-ttu-id="28d0b-467">csharp_space_between_method_declaration_empty_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-467">csharp_space_between_method_declaration_empty_parameter_list_parentheses</span></span>

|<span data-ttu-id="28d0b-468">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-468">Property</span></span>|<span data-ttu-id="28d0b-469">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-469">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-470">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-470">**Option name**</span></span> | <span data-ttu-id="28d0b-471">csharp_space_between_method_declaration_empty_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-471">csharp_space_between_method_declaration_empty_parameter_list_parentheses</span></span> |
| <span data-ttu-id="28d0b-472">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-472">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-473">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-473">C#</span></span> |
| <span data-ttu-id="28d0b-474">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-474">**Introduced version**</span></span> | <span data-ttu-id="28d0b-475">Visual Studio 2017 15.7 版</span><span class="sxs-lookup"><span data-stu-id="28d0b-475">Visual Studio 2017 version 15.7</span></span> |
| <span data-ttu-id="28d0b-476">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-476">**Option values**</span></span> | <span data-ttu-id="28d0b-477">`true` - 在方法声明的空参数列表括号内插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-477">`true` - Insert space within empty parameter list parentheses for a method declaration</span></span><br /><br /><span data-ttu-id="28d0b-478">`false` - 删除方法声明的空参数列表括号内的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-478">`false` - Remove space within empty parameter list parentheses for a method declaration</span></span> |

<span data-ttu-id="28d0b-479">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-479">Code examples:</span></span>

```csharp
// csharp_space_between_method_declaration_empty_parameter_list_parentheses = true
void Goo( )
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}

// csharp_space_between_method_declaration_empty_parameter_list_parentheses = false
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}
```

#### <a name="csharp_space_between_method_declaration_name_and_open_parenthesis"></a><span data-ttu-id="28d0b-480">csharp_space_between_method_declaration_name_and_open_parenthesis</span><span class="sxs-lookup"><span data-stu-id="28d0b-480">csharp_space_between_method_declaration_name_and_open_parenthesis</span></span>

|<span data-ttu-id="28d0b-481">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-481">Property</span></span>|<span data-ttu-id="28d0b-482">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-482">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-483">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-483">**Option name**</span></span> | <span data-ttu-id="28d0b-484">csharp_space_between_method_declaration_name_and_open_parenthesis</span><span class="sxs-lookup"><span data-stu-id="28d0b-484">csharp_space_between_method_declaration_name_and_open_parenthesis</span></span> |
| <span data-ttu-id="28d0b-485">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-485">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-486">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-486">C#</span></span> |
| <span data-ttu-id="28d0b-487">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-487">**Option values**</span></span> | <span data-ttu-id="28d0b-488">`true` - 在方法声明中方法名称和左括号之间放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-488">`true` - Place a space character between the method name and opening parenthesis in the method declaration</span></span><br /><br /><span data-ttu-id="28d0b-489">`false` - 删除方法声明中方法名称和左括号之间的空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-489">`false` - Remove space characters between the method name and opening parenthesis in the method declaration</span></span> |

<span data-ttu-id="28d0b-490">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-490">Code examples:</span></span>

```csharp
// csharp_space_between_method_declaration_name_and_open_parenthesis = true
void M () { }

// csharp_space_between_method_declaration_name_and_open_parenthesis = false
void M() { }
```

#### <a name="csharp_space_between_method_call_parameter_list_parentheses"></a><span data-ttu-id="28d0b-491">csharp_space_between_method_call_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-491">csharp_space_between_method_call_parameter_list_parentheses</span></span>

|<span data-ttu-id="28d0b-492">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-492">Property</span></span>|<span data-ttu-id="28d0b-493">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-493">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-494">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-494">**Option name**</span></span> | <span data-ttu-id="28d0b-495">csharp_space_between_method_call_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-495">csharp_space_between_method_call_parameter_list_parentheses</span></span> |
| <span data-ttu-id="28d0b-496">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-496">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-497">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-497">C#</span></span> |
| <span data-ttu-id="28d0b-498">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-498">**Introduced version**</span></span> | <span data-ttu-id="28d0b-499">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-499">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-500">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-500">**Option values**</span></span> | <span data-ttu-id="28d0b-501">`true` - 在方法调用的左括号之后和右括号之前放置空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-501">`true` - Place a space character after the opening parenthesis and before the closing parenthesis of a method call</span></span><br /><br /><span data-ttu-id="28d0b-502">`false` - 删除方法调用的左括号之后和右括号之前的空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-502">`false` - Remove space characters after the opening parenthesis and before the closing parenthesis of a method call</span></span> |

<span data-ttu-id="28d0b-503">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-503">Code examples:</span></span>

```csharp
// csharp_space_between_method_call_parameter_list_parentheses = true
MyMethod( argument );

// csharp_space_between_method_call_parameter_list_parentheses = false
MyMethod(argument);
```

#### <a name="csharp_space_between_method_call_empty_parameter_list_parentheses"></a><span data-ttu-id="28d0b-504">csharp_space_between_method_call_empty_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-504">csharp_space_between_method_call_empty_parameter_list_parentheses</span></span>

|<span data-ttu-id="28d0b-505">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-505">Property</span></span>|<span data-ttu-id="28d0b-506">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-506">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-507">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-507">**Option name**</span></span> | <span data-ttu-id="28d0b-508">csharp_space_between_method_call_empty_parameter_list_parentheses</span><span class="sxs-lookup"><span data-stu-id="28d0b-508">csharp_space_between_method_call_empty_parameter_list_parentheses</span></span> |
| <span data-ttu-id="28d0b-509">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-509">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-510">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-510">C#</span></span> |
| <span data-ttu-id="28d0b-511">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-511">**Introduced version**</span></span> | <span data-ttu-id="28d0b-512">Visual Studio 2017 15.7 版</span><span class="sxs-lookup"><span data-stu-id="28d0b-512">Visual Studio 2017 version 15.7</span></span> |
| <span data-ttu-id="28d0b-513">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-513">**Option values**</span></span> | <span data-ttu-id="28d0b-514">`true` - 在空参数列表的括号中插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-514">`true` - Insert space within empty argument list parentheses</span></span><br /><br /><span data-ttu-id="28d0b-515">`false` - 删除空参数列表括号内的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-515">`false` - Remove space within empty argument list parentheses</span></span> |

<span data-ttu-id="28d0b-516">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-516">Code examples:</span></span>

```csharp
// csharp_space_between_method_call_empty_parameter_list_parentheses = true
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo( );
}

// csharp_space_between_method_call_empty_parameter_list_parentheses = false
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}
```

#### <a name="csharp_space_between_method_call_name_and_opening_parenthesis"></a><span data-ttu-id="28d0b-517">csharp_space_between_method_call_name_and_opening_parenthesis</span><span class="sxs-lookup"><span data-stu-id="28d0b-517">csharp_space_between_method_call_name_and_opening_parenthesis</span></span>

|<span data-ttu-id="28d0b-518">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-518">Property</span></span>|<span data-ttu-id="28d0b-519">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-519">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-520">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-520">**Option name**</span></span> | <span data-ttu-id="28d0b-521">csharp_space_between_method_call_name_and_opening_parenthesis</span><span class="sxs-lookup"><span data-stu-id="28d0b-521">csharp_space_between_method_call_name_and_opening_parenthesis</span></span> |
| <span data-ttu-id="28d0b-522">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-522">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-523">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-523">C#</span></span> |
| <span data-ttu-id="28d0b-524">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-524">**Introduced version**</span></span> | <span data-ttu-id="28d0b-525">Visual Studio 2017 15.7 版</span><span class="sxs-lookup"><span data-stu-id="28d0b-525">Visual Studio 2017 version 15.7</span></span> |
| <span data-ttu-id="28d0b-526">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-526">**Option values**</span></span> | <span data-ttu-id="28d0b-527">`true` - 在方法调用名称和左括号之间插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-527">`true` - Insert space between method call name and opening parenthesis</span></span><br /><br /><span data-ttu-id="28d0b-528">`false` - 删除方法调用名称和左括号之间的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-528">`false` - Remove space between method call name and opening parenthesis</span></span> |

<span data-ttu-id="28d0b-529">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-529">Code examples:</span></span>

```csharp
// csharp_space_between_method_call_name_and_opening_parenthesis = true
void Goo()
{
    Goo (1);
}

void Goo(int x)
{
    Goo ();
}

// csharp_space_between_method_call_name_and_opening_parenthesis = false
void Goo()
{
    Goo(1);
}

void Goo(int x)
{
    Goo();
}
```

#### <a name="csharp_space_after_comma"></a><span data-ttu-id="28d0b-530">csharp_space_after_comma</span><span class="sxs-lookup"><span data-stu-id="28d0b-530">csharp_space_after_comma</span></span>

|<span data-ttu-id="28d0b-531">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-531">Property</span></span>|<span data-ttu-id="28d0b-532">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-532">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-533">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-533">**Option name**</span></span> | <span data-ttu-id="28d0b-534">csharp_space_after_comma</span><span class="sxs-lookup"><span data-stu-id="28d0b-534">csharp_space_after_comma</span></span> |
| <span data-ttu-id="28d0b-535">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-535">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-536">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-536">C#</span></span> |
| <span data-ttu-id="28d0b-537">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-537">**Option values**</span></span> | <span data-ttu-id="28d0b-538">`true` - 在逗号后面插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-538">`true` - Insert space after a comma</span></span><br /><br /><span data-ttu-id="28d0b-539">`false` - 删除逗号后面的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-539">`false` - Remove space after a comma</span></span> |

<span data-ttu-id="28d0b-540">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-540">Code examples:</span></span>

```csharp
// csharp_space_after_comma = true
int[] x = new int[] { 1, 2, 3, 4, 5 };

// csharp_space_after_comma = false
int[] x = new int[] { 1,2,3,4,5 }
```

#### <a name="csharp_space_before_comma"></a><span data-ttu-id="28d0b-541">csharp_space_before_comma</span><span class="sxs-lookup"><span data-stu-id="28d0b-541">csharp_space_before_comma</span></span>

|<span data-ttu-id="28d0b-542">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-542">Property</span></span>|<span data-ttu-id="28d0b-543">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-543">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-544">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-544">**Option name**</span></span> | <span data-ttu-id="28d0b-545">csharp_space_before_comma</span><span class="sxs-lookup"><span data-stu-id="28d0b-545">csharp_space_before_comma</span></span> |
| <span data-ttu-id="28d0b-546">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-546">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-547">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-547">C#</span></span> |
| <span data-ttu-id="28d0b-548">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-548">**Option values**</span></span> | <span data-ttu-id="28d0b-549">`true` - 在逗号前插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-549">`true` - Insert space before a comma</span></span><br /><br /><span data-ttu-id="28d0b-550">`false` - 删除逗号前的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-550">`false` - Remove space before a comma</span></span> |

<span data-ttu-id="28d0b-551">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-551">Code examples:</span></span>

```csharp
// csharp_space_before_comma = true
int[] x = new int[] { 1 , 2 , 3 , 4 , 5 };

// csharp_space_before_comma = false
int[] x = new int[] { 1, 2, 3, 4, 5 };
```

#### <a name="csharp_space_after_dot"></a><span data-ttu-id="28d0b-552">csharp_space_after_dot</span><span class="sxs-lookup"><span data-stu-id="28d0b-552">csharp_space_after_dot</span></span>

|<span data-ttu-id="28d0b-553">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-553">Property</span></span>|<span data-ttu-id="28d0b-554">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-554">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-555">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-555">**Option name**</span></span> | <span data-ttu-id="28d0b-556">csharp_space_after_dot</span><span class="sxs-lookup"><span data-stu-id="28d0b-556">csharp_space_after_dot</span></span> |
| <span data-ttu-id="28d0b-557">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-557">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-558">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-558">C#</span></span> |
| <span data-ttu-id="28d0b-559">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-559">**Option values**</span></span> | <span data-ttu-id="28d0b-560">`true` - 在点号后面插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-560">`true` - Insert space after a dot</span></span><br /><br /><span data-ttu-id="28d0b-561">`false` - 删除点号后面的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-561">`false` - Remove space after a dot</span></span> |

<span data-ttu-id="28d0b-562">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-562">Code examples:</span></span>

```csharp
// csharp_space_after_dot = true
this. Goo();

// csharp_space_after_dot = false
this.Goo();
```

#### <a name="csharp_space_before_dot"></a><span data-ttu-id="28d0b-563">csharp_space_before_dot</span><span class="sxs-lookup"><span data-stu-id="28d0b-563">csharp_space_before_dot</span></span>

|<span data-ttu-id="28d0b-564">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-564">Property</span></span>|<span data-ttu-id="28d0b-565">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-565">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-566">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-566">**Option name**</span></span> | <span data-ttu-id="28d0b-567">csharp_space_before_dot</span><span class="sxs-lookup"><span data-stu-id="28d0b-567">csharp_space_before_dot</span></span> |
| <span data-ttu-id="28d0b-568">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-568">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-569">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-569">C#</span></span> |
| <span data-ttu-id="28d0b-570">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-570">**Option values**</span></span> | <span data-ttu-id="28d0b-571">`true` - 在点前插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-571">`true` - Insert space before a dot</span></span> <br /><br /><span data-ttu-id="28d0b-572">`false` - 删除点前的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-572">`false` - Remove space before a dot</span></span> |

<span data-ttu-id="28d0b-573">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-573">Code examples:</span></span>

```csharp
// csharp_space_before_dot = true
this .Goo();

// csharp_space_before_dot = false
this.Goo();
```

#### <a name="csharp_space_after_semicolon_in_for_statement"></a><span data-ttu-id="28d0b-574">csharp_space_after_semicolon_in_for_statement</span><span class="sxs-lookup"><span data-stu-id="28d0b-574">csharp_space_after_semicolon_in_for_statement</span></span>

|<span data-ttu-id="28d0b-575">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-575">Property</span></span>|<span data-ttu-id="28d0b-576">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-576">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-577">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-577">**Option name**</span></span> | <span data-ttu-id="28d0b-578">csharp_space_after_semicolon_in_for_statement</span><span class="sxs-lookup"><span data-stu-id="28d0b-578">csharp_space_after_semicolon_in_for_statement</span></span> |
| <span data-ttu-id="28d0b-579">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-579">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-580">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-580">C#</span></span> |
| <span data-ttu-id="28d0b-581">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-581">**Option values**</span></span> | <span data-ttu-id="28d0b-582">`true` - 在 `for` 语句中的每个分号后面插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-582">`true` - Insert space after each semicolon in a `for` statement</span></span><br /><br /><span data-ttu-id="28d0b-583">`false` - 删除 `for` 语句中每个分号后的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-583">`false` - Remove space after each semicolon in a `for` statement</span></span> |

<span data-ttu-id="28d0b-584">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-584">Code examples:</span></span>

```csharp
// csharp_space_after_semicolon_in_for_statement = true
for (int i = 0; i < x.Length; i++)

// csharp_space_after_semicolon_in_for_statement = false
for (int i = 0;i < x.Length;i++)
```

#### <a name="csharp_space_before_semicolon_in_for_statement"></a><span data-ttu-id="28d0b-585">csharp_space_before_semicolon_in_for_statement</span><span class="sxs-lookup"><span data-stu-id="28d0b-585">csharp_space_before_semicolon_in_for_statement</span></span>

|<span data-ttu-id="28d0b-586">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-586">Property</span></span>|<span data-ttu-id="28d0b-587">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-587">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-588">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-588">**Option name**</span></span> | <span data-ttu-id="28d0b-589">csharp_space_before_semicolon_in_for_statement</span><span class="sxs-lookup"><span data-stu-id="28d0b-589">csharp_space_before_semicolon_in_for_statement</span></span> |
| <span data-ttu-id="28d0b-590">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-590">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-591">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-591">C#</span></span> |
| <span data-ttu-id="28d0b-592">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-592">**Option values**</span></span> | <span data-ttu-id="28d0b-593">`true` - 在 `for` 语句中的每个分号前插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-593">`true` - Insert space before each semicolon in a `for` statement</span></span> <br /><br /><span data-ttu-id="28d0b-594">`false` - 删除 `for` 语句中每个分号前的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-594">`false` - Remove space before each semicolon in a `for` statement</span></span> |

<span data-ttu-id="28d0b-595">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-595">Code examples:</span></span>

```csharp
// csharp_space_before_semicolon_in_for_statement = true
for (int i = 0 ; i < x.Length ; i++)

// csharp_space_before_semicolon_in_for_statement = false
for (int i = 0; i < x.Length; i++)
```

#### <a name="csharp_space_around_declaration_statements"></a><span data-ttu-id="28d0b-596">csharp_space_around_declaration_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-596">csharp_space_around_declaration_statements</span></span>

|<span data-ttu-id="28d0b-597">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-597">Property</span></span>|<span data-ttu-id="28d0b-598">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-598">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-599">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-599">**Option name**</span></span> | <span data-ttu-id="28d0b-600">csharp_space_around_declaration_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-600">csharp_space_around_declaration_statements</span></span> |
| <span data-ttu-id="28d0b-601">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-601">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-602">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-602">C#</span></span> |
| <span data-ttu-id="28d0b-603">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-603">**Option values**</span></span> | <span data-ttu-id="28d0b-604">`ignore` - 不删除声明语句中多余的空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-604">`ignore` - Don't remove extra space characters in declaration statements</span></span><br /><br /><span data-ttu-id="28d0b-605">`false` - 删除声明语句中多余的空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-605">`false` - Remove extra space characters in declaration statements</span></span> |

<span data-ttu-id="28d0b-606">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-606">Code examples:</span></span>

```csharp
// csharp_space_around_declaration_statements = ignore
int    x    =    0   ;

// csharp_space_around_declaration_statements = false
int x = 0;
```

#### <a name="csharp_space_before_open_square_brackets"></a><span data-ttu-id="28d0b-607">csharp_space_before_open_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-607">csharp_space_before_open_square_brackets</span></span>

|<span data-ttu-id="28d0b-608">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-608">Property</span></span>|<span data-ttu-id="28d0b-609">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-609">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-610">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-610">**Option name**</span></span> | <span data-ttu-id="28d0b-611">csharp_space_before_open_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-611">csharp_space_before_open_square_brackets</span></span> |
| <span data-ttu-id="28d0b-612">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-612">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-613">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-613">C#</span></span> |
| <span data-ttu-id="28d0b-614">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-614">**Option values**</span></span> | <span data-ttu-id="28d0b-615">`true` - 在左方括号 `[` 前插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-615">`true` - Insert space before opening square brackets `[`</span></span> <br /><br /><span data-ttu-id="28d0b-616">`false` - 删除左方括号 `[` 前的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-616">`false` - Remove space before opening square brackets `[`</span></span> |

<span data-ttu-id="28d0b-617">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-617">Code examples:</span></span>

```csharp
// csharp_space_before_open_square_brackets = true
int [] numbers = new int [] { 1, 2, 3, 4, 5 };

// csharp_space_before_open_square_brackets = false
int[] numbers = new int[] { 1, 2, 3, 4, 5 };
```

#### <a name="csharp_space_between_empty_square_brackets"></a><span data-ttu-id="28d0b-618">csharp_space_between_empty_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-618">csharp_space_between_empty_square_brackets</span></span>

|<span data-ttu-id="28d0b-619">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-619">Property</span></span>|<span data-ttu-id="28d0b-620">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-620">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-621">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-621">**Option name**</span></span> | <span data-ttu-id="28d0b-622">csharp_space_between_empty_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-622">csharp_space_between_empty_square_brackets</span></span> |
| <span data-ttu-id="28d0b-623">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-623">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-624">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-624">C#</span></span> |
| <span data-ttu-id="28d0b-625">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-625">**Option values**</span></span> | <span data-ttu-id="28d0b-626">`true` - 在空方括号 `[ ]` 之间插入空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-626">`true` - Insert space between empty square brackets `[ ]`</span></span> <br /><br /><span data-ttu-id="28d0b-627">`false` - 删除空方括号 `[]` 之间的空格</span><span class="sxs-lookup"><span data-stu-id="28d0b-627">`false` - Remove space between empty square brackets `[]`</span></span> |

<span data-ttu-id="28d0b-628">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-628">Code examples:</span></span>

```csharp
// csharp_space_between_empty_square_brackets = true
int[ ] numbers = new int[ ] { 1, 2, 3, 4, 5 };

// csharp_space_between_empty_square_brackets = false
int[] numbers = new int[] { 1, 2, 3, 4, 5 };
```

#### <a name="csharp_space_between_square_brackets"></a><span data-ttu-id="28d0b-629">csharp_space_between_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-629">csharp_space_between_square_brackets</span></span>

|<span data-ttu-id="28d0b-630">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-630">Property</span></span>|<span data-ttu-id="28d0b-631">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-631">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-632">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-632">**Option name**</span></span> | <span data-ttu-id="28d0b-633">csharp_space_between_square_brackets</span><span class="sxs-lookup"><span data-stu-id="28d0b-633">csharp_space_between_square_brackets</span></span> |
| <span data-ttu-id="28d0b-634">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-634">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-635">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-635">C#</span></span> |
| <span data-ttu-id="28d0b-636">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-636">**Option values**</span></span> | <span data-ttu-id="28d0b-637">`true` - 在非空方括号 `[ 0 ]` 中插入空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-637">`true` - Insert space characters in non-empty square brackets `[ 0 ]`</span></span> <br /><br /><span data-ttu-id="28d0b-638">`false` - 删除非空方括号 `[0]` 中的空格字符</span><span class="sxs-lookup"><span data-stu-id="28d0b-638">`false` - Remove space characters in non-empty square brackets `[0]`</span></span> |

<span data-ttu-id="28d0b-639">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-639">Code examples:</span></span>

```csharp
// csharp_space_between_square_brackets = true
int index = numbers[ 0 ];

// csharp_space_between_square_brackets = false
int index = numbers[0];
```

### <a name="wrap-options"></a><span data-ttu-id="28d0b-640">换行选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-640">Wrap options</span></span>

<span data-ttu-id="28d0b-641">这些格式设置规则与语句和代码块中单一行以及单独的行的使用有关。</span><span class="sxs-lookup"><span data-stu-id="28d0b-641">These formatting rules concern the use of single lines versus separate lines for statements and code blocks.</span></span>

<span data-ttu-id="28d0b-642">.editorconfig 文件示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-642">Example *.editorconfig* file:</span></span>

```ini
# CSharp formatting rules:
[*.cs]
csharp_preserve_single_line_statements = true
csharp_preserve_single_line_blocks = true
```

#### <a name="csharp_preserve_single_line_statements"></a><span data-ttu-id="28d0b-643">csharp_preserve_single_line_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-643">csharp_preserve_single_line_statements</span></span>

|<span data-ttu-id="28d0b-644">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-644">Property</span></span>|<span data-ttu-id="28d0b-645">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-645">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-646">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-646">**Option name**</span></span> | <span data-ttu-id="28d0b-647">csharp_preserve_single_line_statements</span><span class="sxs-lookup"><span data-stu-id="28d0b-647">csharp_preserve_single_line_statements</span></span> |
| <span data-ttu-id="28d0b-648">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-648">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-649">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-649">C#</span></span> |
| <span data-ttu-id="28d0b-650">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-650">**Introduced version**</span></span> | <span data-ttu-id="28d0b-651">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-651">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-652">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-652">**Option values**</span></span> | <span data-ttu-id="28d0b-653">`true` - 将语句和成员声明保留在同一行上</span><span class="sxs-lookup"><span data-stu-id="28d0b-653">`true` - Leave statements and member declarations on the same line</span></span><br /><br /><span data-ttu-id="28d0b-654">`false` - 将语句和成员声明保留在不同行上</span><span class="sxs-lookup"><span data-stu-id="28d0b-654">`false` - Leave statements and member declarations on different lines</span></span> |

<span data-ttu-id="28d0b-655">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-655">Code examples:</span></span>

```csharp
//csharp_preserve_single_line_statements = true
int i = 0; string name = "John";

//csharp_preserve_single_line_statements = false
int i = 0;
string name = "John";
```

#### <a name="csharp_preserve_single_line_blocks"></a><span data-ttu-id="28d0b-656">csharp_preserve_single_line_blocks</span><span class="sxs-lookup"><span data-stu-id="28d0b-656">csharp_preserve_single_line_blocks</span></span>

|<span data-ttu-id="28d0b-657">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-657">Property</span></span>|<span data-ttu-id="28d0b-658">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-658">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-659">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-659">**Option name**</span></span> | <span data-ttu-id="28d0b-660">csharp_preserve_single_line_blocks</span><span class="sxs-lookup"><span data-stu-id="28d0b-660">csharp_preserve_single_line_blocks</span></span> |
| <span data-ttu-id="28d0b-661">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-661">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-662">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-662">C#</span></span> |
| <span data-ttu-id="28d0b-663">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-663">**Introduced version**</span></span> | <span data-ttu-id="28d0b-664">Visual Studio 2017 版本 15.3</span><span class="sxs-lookup"><span data-stu-id="28d0b-664">Visual Studio 2017 version 15.3</span></span> |
| <span data-ttu-id="28d0b-665">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-665">**Option values**</span></span> | <span data-ttu-id="28d0b-666">`true` - 将代码块保留在单个行上</span><span class="sxs-lookup"><span data-stu-id="28d0b-666">`true` - Leave code block on single line</span></span><br /><br /><span data-ttu-id="28d0b-667">`false` - 将代码块保留在单独的行上</span><span class="sxs-lookup"><span data-stu-id="28d0b-667">`false` - Leave code block on separate lines</span></span> |

<span data-ttu-id="28d0b-668">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-668">Code examples:</span></span>

```csharp
//csharp_preserve_single_line_blocks = true
public int Foo { get; set; }

//csharp_preserve_single_line_blocks = false
public int MyProperty
{
    get; set;
}
```

### <a name="using-directive-options"></a><span data-ttu-id="28d0b-669">using 指令选项</span><span class="sxs-lookup"><span data-stu-id="28d0b-669">Using directive options</span></span>

<span data-ttu-id="28d0b-670">此格式设置规则涉及到使用放置在命名空间内和外的 using 指令。</span><span class="sxs-lookup"><span data-stu-id="28d0b-670">This formatting rule concerns the use of using directives being placed inside versus outside a namespace.</span></span>

<span data-ttu-id="28d0b-671">.editorconfig 文件示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-671">Example *.editorconfig* file:</span></span>

```ini
# 'using' directive preferences
[*.cs]
csharp_using_directive_placement = outside_namespace
csharp_using_directive_placement = inside_namespace
```

#### <a name="csharp_using_directive_placement"></a><span data-ttu-id="28d0b-672">csharp_using_directive_placement</span><span class="sxs-lookup"><span data-stu-id="28d0b-672">csharp_using_directive_placement</span></span>

|<span data-ttu-id="28d0b-673">Property</span><span class="sxs-lookup"><span data-stu-id="28d0b-673">Property</span></span>|<span data-ttu-id="28d0b-674">值</span><span class="sxs-lookup"><span data-stu-id="28d0b-674">Value</span></span>|
|-|-|
| <span data-ttu-id="28d0b-675">**选项名称**</span><span class="sxs-lookup"><span data-stu-id="28d0b-675">**Option name**</span></span> | <span data-ttu-id="28d0b-676">csharp_using_directive_placement</span><span class="sxs-lookup"><span data-stu-id="28d0b-676">csharp_using_directive_placement</span></span> |
| <span data-ttu-id="28d0b-677">**适用的语言**</span><span class="sxs-lookup"><span data-stu-id="28d0b-677">**Applicable languages**</span></span> | <span data-ttu-id="28d0b-678">C#</span><span class="sxs-lookup"><span data-stu-id="28d0b-678">C#</span></span> |
| <span data-ttu-id="28d0b-679">**引入的版本**</span><span class="sxs-lookup"><span data-stu-id="28d0b-679">**Introduced version**</span></span> | <span data-ttu-id="28d0b-680">Visual Studio 2019 版本 16.1</span><span class="sxs-lookup"><span data-stu-id="28d0b-680">Visual Studio 2019 version 16.1</span></span> |
| <span data-ttu-id="28d0b-681">**选项值**</span><span class="sxs-lookup"><span data-stu-id="28d0b-681">**Option values**</span></span> | <span data-ttu-id="28d0b-682">`outside_namespace` - 将 using 指令保留在命名空间之外</span><span class="sxs-lookup"><span data-stu-id="28d0b-682">`outside_namespace` - Leave using directives outside namespace</span></span><br /><br /><span data-ttu-id="28d0b-683">`inside_namespace` - 将 using 指令保留在命名空间之内</span><span class="sxs-lookup"><span data-stu-id="28d0b-683">`inside_namespace` - Leave using directives inside namespace</span></span> |

<span data-ttu-id="28d0b-684">代码示例：</span><span class="sxs-lookup"><span data-stu-id="28d0b-684">Code examples:</span></span>

```csharp
// csharp_using_directive_placement = outside_namespace
using System;

namespace Conventions
{

}

// csharp_using_directive_placement = inside_namespace
namespace Conventions
{
    using System;
}
```

## <a name="see-also"></a><span data-ttu-id="28d0b-685">请参阅</span><span class="sxs-lookup"><span data-stu-id="28d0b-685">See also</span></span>

- [<span data-ttu-id="28d0b-686">语言规则</span><span class="sxs-lookup"><span data-stu-id="28d0b-686">Language rules</span></span>](language-rules.md)
- [<span data-ttu-id="28d0b-687">命名规则</span><span class="sxs-lookup"><span data-stu-id="28d0b-687">Naming rules</span></span>](naming-rules.md)
- [<span data-ttu-id="28d0b-688">.NET 代码样式规则参考</span><span class="sxs-lookup"><span data-stu-id="28d0b-688">.NET code style rules reference</span></span>](index.md)
