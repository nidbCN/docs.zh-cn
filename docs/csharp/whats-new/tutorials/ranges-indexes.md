---
title: 使用索引和范围探索数据范围
description: 本高级教程教你使用索引和范围来探索数据，以检查顺序数据集的连续范围。
ms.date: 09/11/2020
ms.technology: csharp-fundamentals
ms.custom: mvc
ms.openlocfilehash: 828cc9cb9abc0be44bb146ba121ff625c6542f03
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878884"
---
# <a name="indices-and-ranges"></a><span data-ttu-id="ee847-103">索引和范围</span><span class="sxs-lookup"><span data-stu-id="ee847-103">Indices and ranges</span></span>

<span data-ttu-id="ee847-104">范围和索引为访问序列中的单个元素或范围提供了简洁的语法。</span><span class="sxs-lookup"><span data-stu-id="ee847-104">Ranges and indices provide a succinct syntax for accessing single elements or ranges in a sequence.</span></span>

<span data-ttu-id="ee847-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="ee847-105">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="ee847-106">对某个序列中的范围使用该语法。</span><span class="sxs-lookup"><span data-stu-id="ee847-106">Use the syntax for ranges in a sequence.</span></span>
> - <span data-ttu-id="ee847-107">了解每个序列开头和末尾的设计决策。</span><span class="sxs-lookup"><span data-stu-id="ee847-107">Understand the design decisions for the start and end of each sequence.</span></span>
> - <span data-ttu-id="ee847-108">了解 <xref:System.Index> 和 <xref:System.Range> 类型的应用场景。</span><span class="sxs-lookup"><span data-stu-id="ee847-108">Learn scenarios for the <xref:System.Index> and <xref:System.Range> types.</span></span>

## <a name="language-support-for-indices-and-ranges"></a><span data-ttu-id="ee847-109">对索引和范围的语言支持</span><span class="sxs-lookup"><span data-stu-id="ee847-109">Language support for indices and ranges</span></span>

<span data-ttu-id="ee847-110">此语言支持依赖于两个新类型和两个新运算符：</span><span class="sxs-lookup"><span data-stu-id="ee847-110">This language support relies on two new types and two new operators:</span></span>

- <span data-ttu-id="ee847-111"><xref:System.Index?displayProperty=nameWithType> 表示一个序列索引。</span><span class="sxs-lookup"><span data-stu-id="ee847-111"><xref:System.Index?displayProperty=nameWithType> represents an index into a sequence.</span></span>
- <span data-ttu-id="ee847-112">来自末尾运算符 `^` 的索引，指定一个索引与序列末尾相关。</span><span class="sxs-lookup"><span data-stu-id="ee847-112">The index from end operator `^`, which specifies that an index is relative to the end of a sequence.</span></span>
- <span data-ttu-id="ee847-113"><xref:System.Range?displayProperty=nameWithType> 表示序列的子范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-113"><xref:System.Range?displayProperty=nameWithType> represents a sub range of a sequence.</span></span>
- <span data-ttu-id="ee847-114">范围运算符 `..`，用于指定范围的开始和末尾，就像操作数一样。</span><span class="sxs-lookup"><span data-stu-id="ee847-114">The range operator `..`, which specifies the start and end of a range as its operands.</span></span>

<span data-ttu-id="ee847-115">让我们从索引规则开始。</span><span class="sxs-lookup"><span data-stu-id="ee847-115">Let's start with the rules for indices.</span></span> <span data-ttu-id="ee847-116">请考虑数组 `sequence`。</span><span class="sxs-lookup"><span data-stu-id="ee847-116">Consider an array `sequence`.</span></span> <span data-ttu-id="ee847-117">`0` 索引与 `sequence[0]` 相同。</span><span class="sxs-lookup"><span data-stu-id="ee847-117">The `0` index is the same as `sequence[0]`.</span></span> <span data-ttu-id="ee847-118">`^0` 索引与 `sequence[sequence.Length]` 相同。</span><span class="sxs-lookup"><span data-stu-id="ee847-118">The `^0` index is the same as `sequence[sequence.Length]`.</span></span> <span data-ttu-id="ee847-119">表达式 `sequence[^0]` 不会引发异常，就像 `sequence[sequence.Length]` 一样。</span><span class="sxs-lookup"><span data-stu-id="ee847-119">The expression `sequence[^0]` does throw an exception, just as `sequence[sequence.Length]` does.</span></span> <span data-ttu-id="ee847-120">对于任何数字 `n`，索引 `^n` 与 `sequence[sequence.Length - n]` 相同。</span><span class="sxs-lookup"><span data-stu-id="ee847-120">For any number `n`, the index `^n` is the same as `sequence[sequence.Length - n]`.</span></span>

```csharp
string[] words = new string[]
{
                // index from start    index from end
    "The",      // 0                   ^9
    "quick",    // 1                   ^8
    "brown",    // 2                   ^7
    "fox",      // 3                   ^6
    "jumped",   // 4                   ^5
    "over",     // 5                   ^4
    "the",      // 6                   ^3
    "lazy",     // 7                   ^2
    "dog"       // 8                   ^1
};              // 9 (or words.Length) ^0
```

<span data-ttu-id="ee847-121">可以使用 `^1` 索引检索最后一个词。</span><span class="sxs-lookup"><span data-stu-id="ee847-121">You can retrieve the last word with the `^1` index.</span></span> <span data-ttu-id="ee847-122">在初始化下面添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="ee847-122">Add the following code below the initialization:</span></span>

[!code-csharp[LastIndex](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastIndex)]

<span data-ttu-id="ee847-123">范围指定范围的开始和末尾。</span><span class="sxs-lookup"><span data-stu-id="ee847-123">A range specifies the *start* and *end* of a range.</span></span> <span data-ttu-id="ee847-124">范围是排除的，也就是说“末尾”不包含在范围内。</span><span class="sxs-lookup"><span data-stu-id="ee847-124">Ranges are exclusive, meaning the *end* isn't included in the range.</span></span> <span data-ttu-id="ee847-125">范围 `[0..^0]` 表示整个范围，就像 `[0..sequence.Length]` 表示整个范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-125">The range `[0..^0]` represents the entire range, just as `[0..sequence.Length]` represents the entire range.</span></span>

<span data-ttu-id="ee847-126">以下代码创建了一个包含单词“quick”、“brown”和“fox”的子范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-126">The following code creates a subrange with the words "quick", "brown", and "fox".</span></span> <span data-ttu-id="ee847-127">它包括 `words[1]` 到 `words[3]`。</span><span class="sxs-lookup"><span data-stu-id="ee847-127">It includes `words[1]` through `words[3]`.</span></span> <span data-ttu-id="ee847-128">元素 `words[4]` 不在该范围内。</span><span class="sxs-lookup"><span data-stu-id="ee847-128">The element `words[4]` isn't in the range.</span></span> <span data-ttu-id="ee847-129">将以下代码添加到同一方法中。</span><span class="sxs-lookup"><span data-stu-id="ee847-129">Add the following code to the same method.</span></span> <span data-ttu-id="ee847-130">将其复制并粘贴到交互式窗口的底部。</span><span class="sxs-lookup"><span data-stu-id="ee847-130">Copy and paste it at the bottom of the interactive window.</span></span>

[!code-csharp[Range](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Range)]

<span data-ttu-id="ee847-131">以下代码使用“lazy”和“dog”返回范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-131">The following code returns the range with "lazy" and "dog".</span></span> <span data-ttu-id="ee847-132">它包括 `words[^2]` 和 `words[^1]`。</span><span class="sxs-lookup"><span data-stu-id="ee847-132">It includes `words[^2]` and `words[^1]`.</span></span> <span data-ttu-id="ee847-133">结束索引 `words[^0]` 不包括在内。</span><span class="sxs-lookup"><span data-stu-id="ee847-133">The end index `words[^0]` isn't included.</span></span> <span data-ttu-id="ee847-134">同样添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="ee847-134">Add the following code as well:</span></span>

[!code-csharp[LastRange](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastRange)]

<span data-ttu-id="ee847-135">下面的示例为开始和/或结束创建了开放范围：</span><span class="sxs-lookup"><span data-stu-id="ee847-135">The following examples create ranges that are open ended for the start, end, or both:</span></span>

[!code-csharp[PartialRange](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_PartialRanges)]

<span data-ttu-id="ee847-136">还可以将范围或索引声明为变量。</span><span class="sxs-lookup"><span data-stu-id="ee847-136">You can also declare ranges or indices as variables.</span></span> <span data-ttu-id="ee847-137">然后可以在 `[` 和 `]` 字符中使用该变量：</span><span class="sxs-lookup"><span data-stu-id="ee847-137">The variable can then be used inside the `[` and `]` characters:</span></span>

[!code-csharp[IndexRangeTypes](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_RangeIndexTypes)]

<span data-ttu-id="ee847-138">下面的示例展示了使用这些选项的多种原因。</span><span class="sxs-lookup"><span data-stu-id="ee847-138">The following sample shows many of the reasons for those choices.</span></span> <span data-ttu-id="ee847-139">请修改 `x`、`y` 和 `z` 以尝试不同的组合。</span><span class="sxs-lookup"><span data-stu-id="ee847-139">Modify `x`, `y`, and `z` to try different combinations.</span></span> <span data-ttu-id="ee847-140">在进行实验时，请使用 `x` 小于 `y`且 `y` 小于 `z` 的有效组合值。</span><span class="sxs-lookup"><span data-stu-id="ee847-140">When you experiment, use values where `x` is less than `y`, and `y` is less than `z` for valid combinations.</span></span> <span data-ttu-id="ee847-141">在新方法中添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="ee847-141">Add the following code in a new method.</span></span> <span data-ttu-id="ee847-142">尝试不同的组合：</span><span class="sxs-lookup"><span data-stu-id="ee847-142">Try different combinations:</span></span>

[!code-csharp[SemanticsExamples](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Semantics)]

## <a name="type-support-for-indices-and-ranges"></a><span data-ttu-id="ee847-143">索引和范围的类型支持</span><span class="sxs-lookup"><span data-stu-id="ee847-143">Type support for indices and ranges</span></span>

<span data-ttu-id="ee847-144">索引和范围提供清晰、简洁的语法来访问序列中的单个元素或元素的范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-144">Indexes and ranges provide clear, concise syntax to access a single element or a range of elements in a sequence.</span></span> <span data-ttu-id="ee847-145">索引表达式通常返回序列元素的类型。</span><span class="sxs-lookup"><span data-stu-id="ee847-145">An index expression typically returns the type of the elements of a sequence.</span></span> <span data-ttu-id="ee847-146">范围表达式通常返回与源序列相同的序列类型。</span><span class="sxs-lookup"><span data-stu-id="ee847-146">A range expression typically returns the same sequence type as the source sequence.</span></span>

<span data-ttu-id="ee847-147">若任何类型提供带 <xref:System.Index> 或 <xref:System.Range> 参数的[索引器](../../programming-guide/indexers/index.md)，则该类型可分别显式支持索引或范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-147">Any type that provides an [indexer](../../programming-guide/indexers/index.md) with an <xref:System.Index> or <xref:System.Range> parameter explicitly supports indices or ranges respectively.</span></span> <span data-ttu-id="ee847-148">采用单个 <xref:System.Range> 参数的索引器可能会返回不同的序列类型，如 <xref:System.Span%601?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="ee847-148">An indexer that takes a single <xref:System.Range> parameter may return a different sequence type, such as <xref:System.Span%601?displayProperty=nameWithType>.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ee847-149">使用范围运算符的代码的性能取决于序列操作数的类型。</span><span class="sxs-lookup"><span data-stu-id="ee847-149">The performance of code using the range operator depends on the type of the sequence operand.</span></span>
>
> <span data-ttu-id="ee847-150">范围运算符的时间复杂度取决于序列类型。</span><span class="sxs-lookup"><span data-stu-id="ee847-150">The time complexity of the range operator depends on the sequence type.</span></span> <span data-ttu-id="ee847-151">例如，如果序列是 `string` 或数组，则结果是输入中指定部分的副本，因此，时间复杂度为 O(N)（其中 N 是范围的长度）。</span><span class="sxs-lookup"><span data-stu-id="ee847-151">For example, if the sequence is a `string` or an array, then the result is a copy of the specified section of the input, so the time complexity is *O(N)* (where N is the length of the range).</span></span> <span data-ttu-id="ee847-152">另一方面，如果它是 <xref:System.Span%601?displayProperty=nameWithType> 或 <xref:System.Memory%601?displayProperty=nameWithType>，则结果引用相同的后备存储，这意味着没有副本且操作为 O(1)。</span><span class="sxs-lookup"><span data-stu-id="ee847-152">On the other hand, if it's a <xref:System.Span%601?displayProperty=nameWithType> or a <xref:System.Memory%601?displayProperty=nameWithType>, the result references the same backing store, which means there is no copy and the operation is *O(1)*.</span></span>
>
> <span data-ttu-id="ee847-153">除了时间复杂度外，这还会产生额外的分配和副本，从而影响性能。</span><span class="sxs-lookup"><span data-stu-id="ee847-153">In addition to the time complexity, this causes extra allocations and copies, impacting performance.</span></span> <span data-ttu-id="ee847-154">在性能敏感的代码中，考虑使用 `Span<T>` 或 `Memory<T>` 作为序列类型，因为不会为其分配范围运算符。</span><span class="sxs-lookup"><span data-stu-id="ee847-154">In performance sensitive code, consider using `Span<T>` or `Memory<T>` as the sequence type, since the range operator does not allocate for them.</span></span>

<span data-ttu-id="ee847-155">若类型包含名称为 `Length` 或 `Count` 的属性，属性有可访问的 Getter 并且其返回类型为 `int`，则此类型为可计数类型。</span><span class="sxs-lookup"><span data-stu-id="ee847-155">A type is **countable** if it has a property named `Length` or `Count` with an accessible getter and a return type of `int`.</span></span> <span data-ttu-id="ee847-156">不显式支持索引或范围的可计数类型可能为它们提供隐式支持。</span><span class="sxs-lookup"><span data-stu-id="ee847-156">A countable type that doesn't explicitly support indices or ranges may provide an implicit support for them.</span></span> <span data-ttu-id="ee847-157">有关详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-8.0/ranges.md)的[隐式索引支持](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-index-support)和[隐式范围支持](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-range-support)部分。</span><span class="sxs-lookup"><span data-stu-id="ee847-157">For more information, see the [Implicit Index support](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-index-support) and [Implicit Range support](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-range-support) sections of the [feature proposal note](~/_csharplang/proposals/csharp-8.0/ranges.md).</span></span> <span data-ttu-id="ee847-158">使用隐式范围支持的范围将返回与源序列相同的序列类型。</span><span class="sxs-lookup"><span data-stu-id="ee847-158">Ranges using implicit range support return the same sequence type as the source sequence.</span></span>

<span data-ttu-id="ee847-159">例如，以下 .NET 类型同时支持索引和范围：<xref:System.String>、<xref:System.Span%601> 和 <xref:System.ReadOnlySpan%601>。</span><span class="sxs-lookup"><span data-stu-id="ee847-159">For example, the following .NET types support both indices and ranges: <xref:System.String>, <xref:System.Span%601>, and <xref:System.ReadOnlySpan%601>.</span></span> <span data-ttu-id="ee847-160"><xref:System.Collections.Generic.List%601> 支持索引，但不支持范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-160">The <xref:System.Collections.Generic.List%601> supports indices but doesn't support ranges.</span></span>

<span data-ttu-id="ee847-161"><xref:System.Array> 具有更多的微妙行为。</span><span class="sxs-lookup"><span data-stu-id="ee847-161"><xref:System.Array> has more nuanced behavior.</span></span> <span data-ttu-id="ee847-162">单个维度数组同时支持索引和范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-162">Single dimension arrays support both indices and ranges.</span></span> <span data-ttu-id="ee847-163">多维数组不支持索引器或范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-163">Multi-dimensional arrays don't support indexers or ranges.</span></span> <span data-ttu-id="ee847-164">多维数组的索引器具有多个参数，而不是一个参数。</span><span class="sxs-lookup"><span data-stu-id="ee847-164">The indexer for a multi-dimensional array has multiple parameters, not a single parameter.</span></span> <span data-ttu-id="ee847-165">交错数组（也称为数组的数组）同时支持范围和索引器。</span><span class="sxs-lookup"><span data-stu-id="ee847-165">Jagged arrays, also referred to as an array of arrays, support both ranges and indexers.</span></span> <span data-ttu-id="ee847-166">下面的示例演示如何循环访问交错数组的矩形子节。</span><span class="sxs-lookup"><span data-stu-id="ee847-166">The following example shows how to iterate a rectangular subsection of a jagged array.</span></span> <span data-ttu-id="ee847-167">它循环访问位于中心的节，不包括前三行和后三行，以及每个选定行中的前两列和后两列：</span><span class="sxs-lookup"><span data-stu-id="ee847-167">It iterates the section in the center, excluding the first and last three rows, and the first and last two columns from each selected row:</span></span>

[!code-csharp[JaggedArrays](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_JaggedArrays)]

<span data-ttu-id="ee847-168">在所有情况下，<xref:System.Array> 的范围运算符都会分配一个数组来存储返回的元素。</span><span class="sxs-lookup"><span data-stu-id="ee847-168">In all cases, the range operator for <xref:System.Array> allocates an array to store the elements returned.</span></span>

## <a name="scenarios-for-indices-and-ranges"></a><span data-ttu-id="ee847-169">索引和范围的应用场景</span><span class="sxs-lookup"><span data-stu-id="ee847-169">Scenarios for indices and ranges</span></span>

<span data-ttu-id="ee847-170">要分析较大序列的一部分时，通常会使用范围和索引。</span><span class="sxs-lookup"><span data-stu-id="ee847-170">You'll often use ranges and indices when you want to analyze a portion of a larger sequence.</span></span> <span data-ttu-id="ee847-171">在准确读取所涉及的序列部分这一方面，新语法更清晰。</span><span class="sxs-lookup"><span data-stu-id="ee847-171">The new syntax is clearer in reading exactly what portion of the sequence is involved.</span></span> <span data-ttu-id="ee847-172">本地函数 `MovingAverage` 以 <xref:System.Range> 为参数。</span><span class="sxs-lookup"><span data-stu-id="ee847-172">The local function `MovingAverage` takes a <xref:System.Range> as its argument.</span></span> <span data-ttu-id="ee847-173">然后，该方法在计算最小值、最大值和平均值时仅枚举该范围。</span><span class="sxs-lookup"><span data-stu-id="ee847-173">The method then enumerates just that range when calculating the min, max, and average.</span></span> <span data-ttu-id="ee847-174">在项目中尝试以下代码：</span><span class="sxs-lookup"><span data-stu-id="ee847-174">Try the following code in your project:</span></span>

[!code-csharp[MovingAverages](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_MovingAverage)]
