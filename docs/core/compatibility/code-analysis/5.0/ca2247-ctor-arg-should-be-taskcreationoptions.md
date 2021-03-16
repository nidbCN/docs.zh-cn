---
title: 中断性变更：CA2247:TaskCompletionSource 构造函数的参数应为 TaskCreationOptions 值
description: 了解 .NET 5 中因启用代码分析规则 CA2247 而导致的中断性变更。
ms.date: 09/03/2020
ms.openlocfilehash: 6c7accaad312352a1448406f2bbf4189f3df1ee5
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257695"
---
# <a name="warning-ca2247-argument-to-taskcompletionsource-constructor-should-be-taskcreationoptions-value"></a><span data-ttu-id="9de2b-103">警告 CA2247：TaskCompletionSource 构造函数的参数应为 TaskCreationOptions 值</span><span class="sxs-lookup"><span data-stu-id="9de2b-103">Warning CA2247: Argument to TaskCompletionSource constructor should be TaskCreationOptions value</span></span>

<span data-ttu-id="9de2b-104">从 .NET 5.0 开始，默认启用 .NET 代码分析器规则 [CA2247](/visualstudio/code-quality/ca2247)。</span><span class="sxs-lookup"><span data-stu-id="9de2b-104">.NET code analyzer rule [CA2247](/visualstudio/code-quality/ca2247) is enabled, by default, starting in .NET 5.0.</span></span> <span data-ttu-id="9de2b-105">关于对 <xref:System.Threading.Tasks.TaskCompletionSource%601> 构造函数的调用（该函数传递类型为 <xref:System.Threading.Tasks.TaskContinuationOptions> 的参数），它会生成一个生成警告。</span><span class="sxs-lookup"><span data-stu-id="9de2b-105">It produces a build warning for calls to the <xref:System.Threading.Tasks.TaskCompletionSource%601> constructor that pass an argument of type <xref:System.Threading.Tasks.TaskContinuationOptions>.</span></span>

## <a name="change-description"></a><span data-ttu-id="9de2b-106">更改说明</span><span class="sxs-lookup"><span data-stu-id="9de2b-106">Change description</span></span>

<span data-ttu-id="9de2b-107">从 .NET 5 开始，.NET SDK 包括 [.NET 源代码分析器](../../../../fundamentals/code-analysis/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="9de2b-107">Starting in .NET 5, the .NET SDK includes [.NET source code analyzers](../../../../fundamentals/code-analysis/overview.md).</span></span> <span data-ttu-id="9de2b-108">其中一些规则会默认启用，包括 [CA2247](/visualstudio/code-quality/ca2247)。</span><span class="sxs-lookup"><span data-stu-id="9de2b-108">Several of these rules are enabled, by default, including [CA2247](/visualstudio/code-quality/ca2247).</span></span> <span data-ttu-id="9de2b-109">如果项目包含违反此规则的代码，并已被配置为将警告视为错误，则此更改可能会中断生成。</span><span class="sxs-lookup"><span data-stu-id="9de2b-109">If your project contains code that violates this rule and is configured to treat warnings as errors, this change could break your build.</span></span>

<span data-ttu-id="9de2b-110">规则 CA2247 会查找对 <xref:System.Threading.Tasks.TaskCompletionSource%601> 构造函数的调用，这些调用传递类型为 <xref:System.Threading.Tasks.TaskContinuationOptions> 的参数。</span><span class="sxs-lookup"><span data-stu-id="9de2b-110">Rule CA2247 finds calls to the <xref:System.Threading.Tasks.TaskCompletionSource%601> constructor that pass an argument of type <xref:System.Threading.Tasks.TaskContinuationOptions>.</span></span> <span data-ttu-id="9de2b-111"><xref:System.Threading.Tasks.TaskCompletionSource%601> 类型具有一个接受 <xref:System.Threading.Tasks.TaskCreationOptions> 值的构造函数和一个接受 <xref:System.Object> 的构造函数。</span><span class="sxs-lookup"><span data-stu-id="9de2b-111">The <xref:System.Threading.Tasks.TaskCompletionSource%601> type has a constructor that accepts a <xref:System.Threading.Tasks.TaskCreationOptions> value, and another constructor that accepts an <xref:System.Object>.</span></span> <span data-ttu-id="9de2b-112">如果意外传递了 <xref:System.Threading.Tasks.TaskContinuationOptions> 值（而不是 <xref:System.Threading.Tasks.TaskCreationOptions> 值），则在运行时调用带有 <xref:System.Object> 参数的构造函数。</span><span class="sxs-lookup"><span data-stu-id="9de2b-112">If you accidentally pass a <xref:System.Threading.Tasks.TaskContinuationOptions> value instead of a <xref:System.Threading.Tasks.TaskCreationOptions> value, the constructor with the <xref:System.Object> parameter is called at run time.</span></span> <span data-ttu-id="9de2b-113">代码将编译并运行，但没有预期行为。</span><span class="sxs-lookup"><span data-stu-id="9de2b-113">Your code will compile and run but won't have the intended behavior.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="9de2b-114">引入的版本</span><span class="sxs-lookup"><span data-stu-id="9de2b-114">Version introduced</span></span>

<span data-ttu-id="9de2b-115">5.0</span><span class="sxs-lookup"><span data-stu-id="9de2b-115">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="9de2b-116">建议操作</span><span class="sxs-lookup"><span data-stu-id="9de2b-116">Recommended action</span></span>

- <span data-ttu-id="9de2b-117">将 <xref:System.Threading.Tasks.TaskContinuationOptions> 参数替换为相应的 <xref:System.Threading.Tasks.TaskCreationOptions> 值。</span><span class="sxs-lookup"><span data-stu-id="9de2b-117">Replace the <xref:System.Threading.Tasks.TaskContinuationOptions> argument with the corresponding <xref:System.Threading.Tasks.TaskCreationOptions> value.</span></span> <span data-ttu-id="9de2b-118">请勿禁止显示此警告，因为它几乎总是突出显示代码中的 bug。</span><span class="sxs-lookup"><span data-stu-id="9de2b-118">Do not suppress this warning, since it almost always highlights a bug in your code.</span></span> <span data-ttu-id="9de2b-119">有关详细信息，请参阅 [CA2247](/visualstudio/code-quality/ca2247)。</span><span class="sxs-lookup"><span data-stu-id="9de2b-119">For more information, see [CA2247](/visualstudio/code-quality/ca2247).</span></span>

- <span data-ttu-id="9de2b-120">若要完全禁用代码分析，请在项目文件中将 `EnableNETAnalyzers` 设置为 `false`。</span><span class="sxs-lookup"><span data-stu-id="9de2b-120">To disable code analysis completely, set `EnableNETAnalyzers` to `false` in your project file.</span></span> <span data-ttu-id="9de2b-121">有关详细信息，请参阅 [EnableNETAnalyzers](../../../project-sdk/msbuild-props.md#enablenetanalyzers)。</span><span class="sxs-lookup"><span data-stu-id="9de2b-121">For more information, see [EnableNETAnalyzers](../../../project-sdk/msbuild-props.md#enablenetanalyzers).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="9de2b-122">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="9de2b-122">Affected APIs</span></span>

- <xref:System.Threading.Tasks.TaskCompletionSource%601.%23ctor(System.Object)>

<!--

### Affected APIs

- ``M:System.Threading.Tasks.TaskCompletionSource`1.#ctor(System.Object)``

### Category

Code analysis

-->
