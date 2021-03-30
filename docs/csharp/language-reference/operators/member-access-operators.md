---
title: 成员访问运算符和表达式 - C# 参考
description: 了解可用于访问类型成员的 C# 运算符。
ms.date: 11/13/2020
author: pkulikov
f1_keywords:
- ._CSharpKeyword
- '[]_CSharpKeyword'
- ()_CSharpKeyword
- ^_CSharpKeyword
- .._CSharpKeyword
helpviewer_keywords:
- member access operators [C#]
- member access operator [C#]
- dot operator [C#]
- . operator [C#]
- subscript operator [C#]
- square brackets [] operator [C#]
- indexer operator [C#]
- '[] operator [C#]'
- null-conditional operators [C#]
- Elvis operator [C#]
- ?. operator [C#]
- ?[] operator [C#]
- invocation operator [C#]
- method call [C#]
- method invocation [C#]
- delegate invocation [C#]
- () operator [C#]
- ^ operator [C#]
- index from end operator [C#]
- hat operator [C#]
- .. operator [C#]
- range operator [C#]
ms.openlocfilehash: b3ba56c7485d27d2692a589e2a8e5b330ea0de85
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873895"
---
# <a name="member-access-operators-and-expressions-c-reference"></a>成员访问运算符和表达式（C# 参考）

访问类型成员时，可以使用以下运算符和表达式：

- [`.`（成员访问）](#member-access-expression-)：用于访问命名空间或类型的成员
- [`[]`（数组元素或索引器访问）](#indexer-operator-)：用于访问数组元素或类型索引器
- [`?.` 和 `?[]`（null 条件运算符）](#null-conditional-operators--and-)：仅当操作数为非 null 时才用于执行成员或元素访问运算
- [`()`（调用）](#invocation-expression-)：用于调用被访问的方法或调用委托
- [`^`（从末尾开始索引）](#index-from-end-operator-)：指示元素位置来自序列的末尾
- [`..`（范围）](#range-operator-)：指定可用于获取一系列序列元素的索引范围

## <a name="member-access-expression-"></a>成员访问表达式。

可以使用 `.` 标记来访问命名空间或类型的成员，如以下示例所示：

- 使用 `.` 访问命名空间内的嵌套命名空间，如以下 [`using` directive](../keywords/using-directive.md) 的示例所示：

  [!code-csharp[nested namespaces](snippets/shared/MemberAccessOperators.cs#NestedNamespace)]

- 使用 `.` 构成限定名称以访问命名空间中的类型，如下面的代码所示：

  [!code-csharp[qualified name](snippets/shared/MemberAccessOperators.cs#QualifiedName)]

  使用 [`using` 指令](../keywords/using-directive.md)来使用可选的限定名称。

- 使用 `.` 访问[类型成员](../../programming-guide/classes-and-structs/index.md#members)（静态和非静态），如下面的代码所示：

  [!code-csharp-interactive[type members](snippets/shared/MemberAccessOperators.cs#TypeMemberAccess)]

还可以使用 `.` 访问[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)。

## <a name="indexer-operator-"></a>索引器运算符 []

方括号 `[]` 通常用于数组、索引器或指针元素访问。

### <a name="array-access"></a>数组访问

下面的示例演示如何访问数组元素：

[!code-csharp-interactive[array access](snippets/shared/MemberAccessOperators.cs#Arrays)]

如果数组索引超出数组相应维度的边界，将引发 <xref:System.IndexOutOfRangeException>。

如上述示例所示，在声明数组类型或实例化数组实例时，还会使用方括号。

有关数组的详细信息，请参阅[数组](../../programming-guide/arrays/index.md)。

### <a name="indexer-access"></a>索引器访问

下面的示例使用 .NET <xref:System.Collections.Generic.Dictionary%602> 类型来演示索引器访问：

[!code-csharp-interactive[indexer access](snippets/shared/MemberAccessOperators.cs#Indexers)]

使用索引器，可通过类似于编制数组索引的方式对用户定义类型的实例编制索引。 与必须是整数的数组索引不同，可以将索引器参数声明为任何类型。

有关索引器的详细信息，请参阅[索引器](../../programming-guide/indexers/index.md)。

### <a name="other-usages-of-"></a>[] 的其他用法

要了解指针元素访问，请参阅[与指针相关的运算符](pointer-related-operators.md)一文的[指针元素访问运算符 []](pointer-related-operators.md#pointer-element-access-operator-) 部分。

方括号还用于指定[属性](../../programming-guide/concepts/attributes/index.md)：

```csharp
[System.Diagnostics.Conditional("DEBUG")]
void TraceMethod() {}
```

## <a name="null-conditional-operators--and-"></a>Null 条件运算符 ?. 和 ?[]

Null 条件运算符在 C# 6 及更高版本中可用，仅当操作数的计算结果为非 null 时，null 条件运算符才会将[成员访问](#member-access-expression-) `?.` 或[元素访问](#indexer-operator-) `?[]` 运算应用于其操作数；否则，将返回 `null`。 即：

- 如果 `a` 的计算结果为 `null`，则 `a?.x` 或 `a?[x]` 的结果为 `null`。
- 如果 `a` 的计算结果为非 null，则 `a?.x` 或 `a?[x]` 的结果将分别与 `a.x` 或 `a[x]` 的结果相同。

  > [!NOTE]
  > 如果 `a.x` 或 `a[x]` 引发异常，则 `a?.x` 或 `a?[x]` 将对非 null `a` 引发相同的异常。 例如，如果 `a` 为非 null 数组实例且 `x` 在 `a`的边界之外，则 `a?[x]` 将引发 <xref:System.IndexOutOfRangeException>。

NULL 条件运算符采用最小化求值策略。 也就是说，如果条件成员或元素访问运算链中的一个运算返回 `null`，则链的其余部分不会执行。 在以下示例中，如果 `A` 的计算结果为 `null`，则不会计算 `B`；如果 `A` 或 `B` 的计算结果为 `null`，则不会计算 `C`：

```csharp
A?.B?.Do(C);
A?.B?[C];
```

以下示例演示了 `?.` 和 `?[]` 运算符的用法：

[!code-csharp-interactive[null-conditional operators](snippets/shared/MemberAccessOperators.cs#NullConditional)]

前面的示例还使用 [Null 合并运算符 `??`](null-coalescing-operator.md) 来指定替代表达式，以便在 null 条件运算的结果为 `null` 时用于计算。

如果 `a.x` 或 `a[x]` 是不可为 null 的值类型 `T`，则 `a?.x` 或 `a?[x]` 属于对应的[可为 null 的值类型](../builtin-types/nullable-value-types.md) `T?`。 如果需要 `T` 类型的表达式，请将 Null 合并操作符 `??` 应用于 null 条件表达式，如下面的示例所示：

[!code-csharp-interactive[null-conditional with null-coalescing](snippets/shared/MemberAccessOperators.cs#NullConditionalWithNullCoalescing)]

在前面的示例中，如果不使用 `??` 运算符，则在 `numbers` 为 `null` 时，`numbers?.Length < 2` 的计算结果为 `false`。

Null 条件成员访问运算符 `?.` 也称为 Elvis 运算符。

### <a name="thread-safe-delegate-invocation"></a>线程安全的委托调用

使用 `?.` 运算符来检查委托是否非 null 并以线程安全的方式调用它（例如，[引发事件](../../../standard/events/how-to-raise-and-consume-events.md)时），如下面的代码所示：

```csharp
PropertyChanged?.Invoke(…)
```

该代码等同于将在 C# 5 或更低版本中使用的以下代码：

```csharp
var handler = this.PropertyChanged;
if (handler != null)
{
    handler(…);
}
```

这是一种线程安全方法，可确保只调用非 null `handler`。 由于委托实例是不可变的，因此，任何线程都不能更改 `handler` 本地变量所引用的对象。 具体而言，如果另一个线程执行的代码从 `PropertyChanged` 事件中取消订阅，并且 `PropertyChanged` 在调用 `handler` 之前变为 `null`，则 `handler` 引用的对象不受影响。 `?.` 运算符对其左操作数的计算不超过一次，从而确保在验证为非 null 后，不能将其更改为 `null`。

## <a name="invocation-expression-"></a>调用表达式 ()

使用括号 `()` 调用[方法](../../programming-guide/classes-and-structs/methods.md)或调用[委托](../../programming-guide/delegates/index.md)。

以下示例演示如何在使用或不使用参数的情况下调用方法，以及调用委托：

[!code-csharp-interactive[invocation with ()](snippets/shared/MemberAccessOperators.cs#Invocation)]

在调用带 [`new`](new-operator.md) 运算符的[构造函数](../../programming-guide/classes-and-structs/constructors.md)时，还可以使用括号。

### <a name="other-usages-of-"></a>() 的其他用法

此外可以使用括号来调整表达式中计算操作的顺序。 有关详细信息，请参阅 [C# 运算符](index.md)。

[强制转换表达式](type-testing-and-cast.md#cast-expression)，其执行显式类型转换，也可以使用括号。

## <a name="index-from-end-operator-"></a>从末尾运算符 ^ 开始索引

`^` 运算符在 C# 8.0 和更高版本中提供，指示序列末尾的元素位置。 对于长度为 `length` 的序列，`^n` 指向与序列开头偏移 `length - n` 的元素。 例如，`^1` 指向序列的最后一个元素，`^length` 指向序列的第一个元素。

[!code-csharp[index from end](snippets/shared/MemberAccessOperators.cs#IndexFromEnd)]

如前面的示例所示，表达式 `^e` 属于 <xref:System.Index?displayProperty=nameWithType> 类型。 在表达式 `^e` 中，`e` 的结果必须隐式转换为 `int`。

还可以将 `^` 运算符与[范围运算符](#range-operator-)一起使用以创建一个索引范围。 有关详细信息，请参阅[索引和范围](../../whats-new/tutorials/ranges-indexes.md)。

## <a name="range-operator-"></a>范围运算符 .

`..` 运算符在 C# 8.0 和更高版本中提供，指定索引范围的开头和末尾作为其操作数。 左侧操作数是范围的包含性开头。 右侧操作数是范围的包含性末尾。 任一操作数都可以是序列开头或末尾的索引，如以下示例所示：

[!code-csharp[range examples](snippets/shared/MemberAccessOperators.cs#Ranges)]

如前面的示例所示，表达式 `a..b` 属于 <xref:System.Range?displayProperty=nameWithType> 类型。 在表达式 `a..b` 中，`a` 和 `b` 的结果必须隐式转换为 `int` 或 <xref:System.Index>。

可以省略 `..` 运算符的任何操作数来获取无限制范围：

- `a..` 等效于 `a..^0`
- `..b` 等效于 `0..b`
- `..` 等效于 `0..^0`

[!code-csharp[ranges with omitted operands](snippets/shared/MemberAccessOperators.cs#RangesOptional)]

有关详细信息，请参阅[索引和范围](../../whats-new/tutorials/ranges-indexes.md)。

## <a name="operator-overloadability"></a>运算符可重载性

`.`、`()`、`^` 和 `..` 运算符无法进行重载。 `[]` 运算符也被视为非可重载运算符。 使用[索引器](../../programming-guide/indexers/index.md)以支持对用户定义的类型编制索引。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [成员访问](~/_csharplang/spec/expressions.md#member-access)
- [元素访问](~/_csharplang/spec/expressions.md#element-access)
- [Null 条件运算符](~/_csharplang/spec/expressions.md#null-conditional-operator)
- [调用表达式](~/_csharplang/spec/expressions.md#invocation-expressions)

有关索引和范围的详细信息，请参阅[功能建议说明](~/_csharplang/proposals/csharp-8.0/ranges.md)。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [??（空合运算符）](null-coalescing-operator.md)
- [:: 运算符](namespace-alias-qualifier.md)
