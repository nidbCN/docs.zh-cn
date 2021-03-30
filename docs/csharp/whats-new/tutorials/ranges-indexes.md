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
# <a name="indices-and-ranges"></a>索引和范围

范围和索引为访问序列中的单个元素或范围提供了简洁的语法。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 对某个序列中的范围使用该语法。
> - 了解每个序列开头和末尾的设计决策。
> - 了解 <xref:System.Index> 和 <xref:System.Range> 类型的应用场景。

## <a name="language-support-for-indices-and-ranges"></a>对索引和范围的语言支持

此语言支持依赖于两个新类型和两个新运算符：

- <xref:System.Index?displayProperty=nameWithType> 表示一个序列索引。
- 来自末尾运算符 `^` 的索引，指定一个索引与序列末尾相关。
- <xref:System.Range?displayProperty=nameWithType> 表示序列的子范围。
- 范围运算符 `..`，用于指定范围的开始和末尾，就像操作数一样。

让我们从索引规则开始。 请考虑数组 `sequence`。 `0` 索引与 `sequence[0]` 相同。 `^0` 索引与 `sequence[sequence.Length]` 相同。 表达式 `sequence[^0]` 不会引发异常，就像 `sequence[sequence.Length]` 一样。 对于任何数字 `n`，索引 `^n` 与 `sequence[sequence.Length - n]` 相同。

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

可以使用 `^1` 索引检索最后一个词。 在初始化下面添加以下代码：

[!code-csharp[LastIndex](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastIndex)]

范围指定范围的开始和末尾。 范围是排除的，也就是说“末尾”不包含在范围内。 范围 `[0..^0]` 表示整个范围，就像 `[0..sequence.Length]` 表示整个范围。

以下代码创建了一个包含单词“quick”、“brown”和“fox”的子范围。 它包括 `words[1]` 到 `words[3]`。 元素 `words[4]` 不在该范围内。 将以下代码添加到同一方法中。 将其复制并粘贴到交互式窗口的底部。

[!code-csharp[Range](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Range)]

以下代码使用“lazy”和“dog”返回范围。 它包括 `words[^2]` 和 `words[^1]`。 结束索引 `words[^0]` 不包括在内。 同样添加以下代码：

[!code-csharp[LastRange](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_LastRange)]

下面的示例为开始和/或结束创建了开放范围：

[!code-csharp[PartialRange](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_PartialRanges)]

还可以将范围或索引声明为变量。 然后可以在 `[` 和 `]` 字符中使用该变量：

[!code-csharp[IndexRangeTypes](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_RangeIndexTypes)]

下面的示例展示了使用这些选项的多种原因。 请修改 `x`、`y` 和 `z` 以尝试不同的组合。 在进行实验时，请使用 `x` 小于 `y`且 `y` 小于 `z` 的有效组合值。 在新方法中添加以下代码。 尝试不同的组合：

[!code-csharp[SemanticsExamples](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_Semantics)]

## <a name="type-support-for-indices-and-ranges"></a>索引和范围的类型支持

索引和范围提供清晰、简洁的语法来访问序列中的单个元素或元素的范围。 索引表达式通常返回序列元素的类型。 范围表达式通常返回与源序列相同的序列类型。

若任何类型提供带 <xref:System.Index> 或 <xref:System.Range> 参数的[索引器](../../programming-guide/indexers/index.md)，则该类型可分别显式支持索引或范围。 采用单个 <xref:System.Range> 参数的索引器可能会返回不同的序列类型，如 <xref:System.Span%601?displayProperty=nameWithType>。

> [!IMPORTANT]
> 使用范围运算符的代码的性能取决于序列操作数的类型。
>
> 范围运算符的时间复杂度取决于序列类型。 例如，如果序列是 `string` 或数组，则结果是输入中指定部分的副本，因此，时间复杂度为 O(N)（其中 N 是范围的长度）。 另一方面，如果它是 <xref:System.Span%601?displayProperty=nameWithType> 或 <xref:System.Memory%601?displayProperty=nameWithType>，则结果引用相同的后备存储，这意味着没有副本且操作为 O(1)。
>
> 除了时间复杂度外，这还会产生额外的分配和副本，从而影响性能。 在性能敏感的代码中，考虑使用 `Span<T>` 或 `Memory<T>` 作为序列类型，因为不会为其分配范围运算符。

若类型包含名称为 `Length` 或 `Count` 的属性，属性有可访问的 Getter 并且其返回类型为 `int`，则此类型为可计数类型。 不显式支持索引或范围的可计数类型可能为它们提供隐式支持。 有关详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-8.0/ranges.md)的[隐式索引支持](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-index-support)和[隐式范围支持](~/_csharplang/proposals/csharp-8.0/ranges.md#implicit-range-support)部分。 使用隐式范围支持的范围将返回与源序列相同的序列类型。

例如，以下 .NET 类型同时支持索引和范围：<xref:System.String>、<xref:System.Span%601> 和 <xref:System.ReadOnlySpan%601>。 <xref:System.Collections.Generic.List%601> 支持索引，但不支持范围。

<xref:System.Array> 具有更多的微妙行为。 单个维度数组同时支持索引和范围。 多维数组不支持索引器或范围。 多维数组的索引器具有多个参数，而不是一个参数。 交错数组（也称为数组的数组）同时支持范围和索引器。 下面的示例演示如何循环访问交错数组的矩形子节。 它循环访问位于中心的节，不包括前三行和后三行，以及每个选定行中的前两列和后两列：

[!code-csharp[JaggedArrays](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_JaggedArrays)]

在所有情况下，<xref:System.Array> 的范围运算符都会分配一个数组来存储返回的元素。

## <a name="scenarios-for-indices-and-ranges"></a>索引和范围的应用场景

要分析较大序列的一部分时，通常会使用范围和索引。 在准确读取所涉及的序列部分这一方面，新语法更清晰。 本地函数 `MovingAverage` 以 <xref:System.Range> 为参数。 然后，该方法在计算最小值、最大值和平均值时仅枚举该范围。 在项目中尝试以下代码：

[!code-csharp[MovingAverages](~/samples/snippets/csharp/tutorials/RangesIndexes/IndicesAndRanges.cs#IndicesAndRanges_MovingAverage)]
