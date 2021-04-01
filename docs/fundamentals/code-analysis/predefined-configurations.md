---
title: 预定义的配置文件（代码分析）
description: 了解如何面向特定代码分析类型使用预定义的 EditorConfig 和规则集文件。
ms.date: 09/24/2020
ms.topic: conceptual
ms.openlocfilehash: 748ab8a9ddfcfcadeb33da877769cedac901655a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876586"
---
# <a name="predefined-configuration-files"></a><span data-ttu-id="6b2d4-103">预定义的配置文件</span><span class="sxs-lookup"><span data-stu-id="6b2d4-103">Predefined configuration files</span></span>

<span data-ttu-id="6b2d4-104">使用预定义的 EditorConfig 和[规则集](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules)文件，可以快速轻松地启用某一类别的代码质量规则，如安全性或设计规则。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-104">Predefined EditorConfig and [rule set](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules) files are available that make it quick and easy to enable a category of code quality rules, such as security or design rules.</span></span> <span data-ttu-id="6b2d4-105">通过启用特定类别的规则，可以确定目标问题和特定情况。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-105">By enabling a specific category of rules, you can identify targeted issues and specific conditions.</span></span> <span data-ttu-id="6b2d4-106">若要访问这些预定义的文件，请安装 [Microsoft.CodeAnalysis.NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) NuGet 分析器包。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-106">To access these predefined files, install the [Microsoft.CodeAnalysis.NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) NuGet analyzer package.</span></span>

<span data-ttu-id="6b2d4-107">[Microsoft.CodeAnalysis.NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) 包括用于以下规则类别的预定义 EditorConfig 文件和规则集：</span><span class="sxs-lookup"><span data-stu-id="6b2d4-107">[Microsoft.CodeAnalysis.NetAnalyzers](https://github.com/dotnet/roslyn-analyzers#microsoftcodeanalysisnetanalyzers) includes predefined EditorConfig files and rule sets for the following rule categories:</span></span>

- <span data-ttu-id="6b2d4-108">┮Τ砏玥</span><span class="sxs-lookup"><span data-stu-id="6b2d4-108">All rules</span></span>
- <span data-ttu-id="6b2d4-109">数据流</span><span class="sxs-lookup"><span data-stu-id="6b2d4-109">Dataflow</span></span>
- <span data-ttu-id="6b2d4-110">设计</span><span class="sxs-lookup"><span data-stu-id="6b2d4-110">Design</span></span>
- <span data-ttu-id="6b2d4-111">文档</span><span class="sxs-lookup"><span data-stu-id="6b2d4-111">Documentation</span></span>
- <span data-ttu-id="6b2d4-112">全球化</span><span class="sxs-lookup"><span data-stu-id="6b2d4-112">Globalization</span></span>
- <span data-ttu-id="6b2d4-113">互操作性</span><span class="sxs-lookup"><span data-stu-id="6b2d4-113">Interoperability</span></span>
- <span data-ttu-id="6b2d4-114">可维护性</span><span class="sxs-lookup"><span data-stu-id="6b2d4-114">Maintainability</span></span>
- <span data-ttu-id="6b2d4-115">命名</span><span class="sxs-lookup"><span data-stu-id="6b2d4-115">Naming</span></span>
- <span data-ttu-id="6b2d4-116">性能</span><span class="sxs-lookup"><span data-stu-id="6b2d4-116">Performance</span></span>
- <span data-ttu-id="6b2d4-117">从 FxCop 移植</span><span class="sxs-lookup"><span data-stu-id="6b2d4-117">Ported from FxCop</span></span>
- <span data-ttu-id="6b2d4-118">可靠性</span><span class="sxs-lookup"><span data-stu-id="6b2d4-118">Reliability</span></span>
- <span data-ttu-id="6b2d4-119">安全性</span><span class="sxs-lookup"><span data-stu-id="6b2d4-119">Security</span></span>
- <span data-ttu-id="6b2d4-120">使用情况</span><span class="sxs-lookup"><span data-stu-id="6b2d4-120">Usage</span></span>

<span data-ttu-id="6b2d4-121">每类规则都有一个 EditorConfig 或规则集文件，用于：</span><span class="sxs-lookup"><span data-stu-id="6b2d4-121">Each of those categories of rules has an EditorConfig or rule set file to:</span></span>

- <span data-ttu-id="6b2d4-122">启用相应类别中的所有规则（并禁用所有其他规则）</span><span class="sxs-lookup"><span data-stu-id="6b2d4-122">Enable all the rules in the category (and disable all other rules)</span></span>
- <span data-ttu-id="6b2d4-123">使用每个规则由默认设置启用的默认严重性（并禁用所有其他规则）</span><span class="sxs-lookup"><span data-stu-id="6b2d4-123">Use each rule's default severity and enabled by default setting (and disable all other rules)</span></span>

> [!TIP]
> <span data-ttu-id="6b2d4-124">“所有规则”类别具有一个额外的 EditorConfig 或规则集文件，用于禁用所有规则。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-124">The "all rules" category has an additional EditorConfig or rule set file to disable all rules.</span></span> <span data-ttu-id="6b2d4-125">可使用此文件快速清除项目中的任何分析器警告或错误。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-125">Use this file to quickly get rid of any analyzer warnings or errors in a project.</span></span>

## <a name="predefined-editorconfig-files"></a><span data-ttu-id="6b2d4-126">预定义的 EditorConfig 文件</span><span class="sxs-lookup"><span data-stu-id="6b2d4-126">Predefined EditorConfig files</span></span>

<span data-ttu-id="6b2d4-127">Microsoft.CodeAnalysis.NetAnalyzers 分析器包的预定义 EditorConfig 文件位于 NuGet 包安装位置的“editorconfig”子目录中。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-127">The predefined EditorConfig files for the Microsoft.CodeAnalysis.NetAnalyzers analyzer package are located in the *editorconfig* subdirectory of where the NuGet package was installed.</span></span> <span data-ttu-id="6b2d4-128">例如，用于启用所有安全规则的 EditorConfig 文件位于 editorconfig/SecurityRulesEnabled/.editorconfig。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-128">For example, the EditorConfig file to enable all security rules is located at *editorconfig/SecurityRulesEnabled/.editorconfig*.</span></span>

<span data-ttu-id="6b2d4-129">请将所选的 .editorconfig 文件复制到项目的根目录中。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-129">Copy the chosen *.editorconfig* file to your project's root directory.</span></span>

## <a name="predefined-rule-sets"></a><span data-ttu-id="6b2d4-130">预定义规则集</span><span class="sxs-lookup"><span data-stu-id="6b2d4-130">Predefined rule sets</span></span>

<span data-ttu-id="6b2d4-131">Microsoft.CodeAnalysis.NetAnalyzers 分析器包的预定义规则集文件位于 NuGet 包安装位置的“rulesets”子目录中。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-131">The predefined rule set files for the Microsoft.CodeAnalysis.NetAnalyzers analyzer package are located in the *rulesets* subdirectory of where the NuGet package was installed.</span></span> <span data-ttu-id="6b2d4-132">例如，用于启用所有安全规则的规则集文件位于 rulesets/SecurityRulesEnabled.ruleset。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-132">For example, the rule set file to enable all security rules is located at *rulesets/SecurityRulesEnabled.ruleset*.</span></span> <span data-ttu-id="6b2d4-133">请复制一个或多个规则集，并将其粘贴到包含你的项目的目录中。</span><span class="sxs-lookup"><span data-stu-id="6b2d4-133">Copy one or more of the rule sets and paste them in the directory that contains your project.</span></span>

## <a name="see-also"></a><span data-ttu-id="6b2d4-134">请参阅</span><span class="sxs-lookup"><span data-stu-id="6b2d4-134">See also</span></span>

- [<span data-ttu-id="6b2d4-135">分析器配置</span><span class="sxs-lookup"><span data-stu-id="6b2d4-135">Analyzer configuration</span></span>](https://github.com/dotnet/roslyn-analyzers/blob/main/docs/Analyzer%20Configuration.md)
- [<span data-ttu-id="6b2d4-136">EditorConfig 的 .NET 代码样式规则选项</span><span class="sxs-lookup"><span data-stu-id="6b2d4-136">.NET code style rule options for EditorConfig</span></span>](code-style-rule-options.md)
