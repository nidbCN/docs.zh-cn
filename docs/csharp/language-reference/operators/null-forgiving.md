---
description: '! （null 包容）运算符 - C# 参考'
title: '! （null 包容）运算符 - C# 参考'
ms.date: 11/13/2020
f1_keywords:
- nullForgiving_CSharpKeyword
helpviewer_keywords:
- null-forgiving operator [C#]
- '! operator [C#]'
ms.openlocfilehash: 14bc4501dfed515e162a6e30137c9def89bb3651
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111070"
---
# <a name="-null-forgiving-operator-c-reference"></a>! （null 包容）运算符（C# 参考）

在 C# 8.0 及更高版本中可用，一元后缀 `!` 运算符是 null 包容运算符或 null 抑制运算符。 在已启用的[可为空的注释上下文](../../nullable-references.md#nullable-annotation-context)中，可以使用 null 包容运算符来声明可为空的引用类型的表达式 `x` 不为 `null`：`x!`。 一元前缀 `!` 运算符是[逻辑非运算符](boolean-logical-operators.md#logical-negation-operator-)。

null 包容运算符在运行时不起作用。 它仅通过更改表达式的 null 状态来影响编译器的静态流分析。 在运行时，表达式 `x!` 的计算结果为基础表达式 `x` 的结果。

有关可为空引用类型特性的详细信息，请参见[可为空引用类型](../builtin-types/nullable-reference-types.md)。

## <a name="examples"></a>示例

null 包容运算符的一个用例是测试参数验证逻辑。 例如，请考虑以下类：

[!code-csharp[Person class](snippets/shared/NullForgivingOperator.cs#PersonClass)]

使用 [ 测试框架](../../../core/testing/unit-testing-with-mstest.md)，可以在构造函数中为验证逻辑创建以下测试：

[!code-csharp[Person test](snippets/shared/NullForgivingOperator.cs#TestPerson)]

如果不使用 null 包容运算符，编译器将为前面的代码生成以下警告：`Warning CS8625: Cannot convert null literal to non-nullable reference type`。 通过使用 null 包容运算符，可以告知编译器传递 `null` 是预期行为，不应发出警告。

如果你明确知道某个表达式不能为 `null`，但编译器无法识别它，也可以使用 null 包容运算符。 在下面的示例中，如果 `IsValid` 方法返回 `true`，则其参数不是 `null`，可以放心取消对它的引用：

[!code-csharp[Use null-forgiving operator](snippets/shared/NullForgivingOperator.cs#UseNullForgiving)]

如果没有 null 包容运算符，编译器将为 `p.Name` 代码生成以下警告：`Warning CS8602: Dereference of a possibly null reference`。

如果可以修改 `IsValid` 方法，则可使用 [NotNullWhen](xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute) 属性告知编译器，当方法返回 `true` 时，`IsValid` 方法的参数不能是 `null`：

[!code-csharp[Use an attribute](snippets/shared/NullForgivingOperator.cs#UseAttribute)]

在前面的例子中，不需要使用 null 包容运算符，因为编译器有足够的信息来发现 `p` 不能是 `if` 语句中的 `null`。 如需深入了解允许你提供有关变量 null 状态的其他信息的属性，请参阅[使用属性升级 API 以定义 null 期望值](../attributes/nullable-analysis.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅[可为空的引用类型规范草案](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md)的 [null 包容性运算符](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md#the-null-forgiving-operator)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [教程：使用可为空引用类型进行设计](../../whats-new/tutorials/nullable-reference-types.md)
