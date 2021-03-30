---
description: where（泛型类型约束）- C# 参考
title: where（泛型类型约束）- C# 参考
ms.date: 04/15/2020
f1_keywords:
- whereconstraint
- whereconstraint_CSharpKeyword
- classconstraint_CSharpKeyword
- structconstraint_CSharpKeyword
- enumconstraint_CSharpKeyword
helpviewer_keywords:
- where (generic type constraint) [C#]
ms.openlocfilehash: 83fb5b562d9e1e4caaef179ca2911adb60fc01fa
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872621"
---
# <a name="where-generic-type-constraint-c-reference"></a>where（泛型类型约束）（C# 参考）

泛型定义中的 `where` 子句指定对用作泛型类型、方法、委托或本地函数中类型参数的参数类型的约束。 约束可指定接口、基类或要求泛型类型为引用、值或非托管类型。 它们声明类型参数必须具备的功能。

例如，可以声明一个泛型类 `AGenericClass`，以使类型参数 `T` 实现 <xref:System.IComparable%601> 接口：

[!code-csharp[using an interface constraint](snippets/GenericWhereConstraints.cs#1)]

> [!NOTE]
> 有关查询表达式中的 where 子句的详细信息，请参阅 [where 子句](where-clause.md)。

`where` 子句还可包括基类约束。 基类约束表明用作该泛型类型的类型参数的类型具有指定的类作为基类（或者是该基类）。 该基类约束一经使用，就必须出现在该类型参数的所有其他约束之前。 某些类型不允许作为基类约束：<xref:System.Object>、<xref:System.Array> 和 <xref:System.ValueType>。 在 C# 7.3 之前，<xref:System.Enum>、<xref:System.Delegate> 和 <xref:System.MulticastDelegate> 也不允许作为基类约束。 以下示例显示现可指定为基类的类型：

[!code-csharp[using an interface constraint](snippets/GenericWhereConstraints.cs#2)]

在 C# 8.0 及更高版本中的可为 null 上下文中，强制执行基类类型的为 null 性。 如果基类不可为 null（例如 `Base`），则类型参数必须不可为 null。 如果基类可为 null（例如 `Base?`），则类型参数可以是可为 null 或不可为 null 的引用类型。 当基类不可为 null 时，如果类型参数是可为 null 的引用类型，编译器将发出警告。

`where` 子句可指定类型为 `class` 或 `struct`。 `struct` 约束不再需要指定 `System.ValueType` 的基类约束。 `System.ValueType` 类型可能不用作基类约束。 以下示例显示 `class` 和 `struct` 约束：

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#3)]

在 C# 8.0 及更高版本中的可为 null 上下文中，`class` 约束要求类型是不可为 null 的引用类型。 若要允许可为 null 的引用类型，请使用 `class?` 约束，该约束允许可为 null 和不可为 null 的引用类型。

`where` 子句可能包含 `notnull` 约束。 `notnull` 约束将类型参数限制为不可为 null 的类型。 该类型可以是[值类型](../builtin-types/value-types.md)，也可以是不可为 null 的引用类型。 对于在 [`nullable enable` 上下文](../../nullable-references.md#nullable-contexts)中编译的代码，从 C# 8.0 开始可以使用 `notnull` 约束。 与其他约束不同，如果类型参数违反 `notnull` 约束，编译器会生成警告而不是错误。 警告仅在 `nullable enable` 上下文中生成。

> [!IMPORTANT]
> 包含 `notnull` 约束的泛型声明可以在可为 null 的不明显上下文中使用，但编译器不会强制执行约束。

[!code-csharp[using the nonnull constraint](snippets/GenericWhereConstraints.cs#NotNull)]

`where` 子句还可包括 `unmanaged` 约束。 `unmanaged` 约束将类型参数限制为名为[“非托管类型”](../builtin-types/unmanaged-types.md)的类型。 `unmanaged` 约束使得在 C# 中编写低级别的互操作代码变得更容易。 此约束支持跨所有非托管类型的可重用例程。 `unmanaged` 约束不能与 `class` 或 `struct` 约束结合使用。 `unmanaged` 约束强制该类型必须为 `struct`：

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#4)]

`where` 子句也可能包括构造函数约束 `new()`。 该约束使得能够使用 `new` 运算符创建类型参数的实例。 [new() 约束](new-constraint.md)可以让编译器知道：提供的任何类型参数都必须具有可访问的无参数构造函数。 例如：

[!code-csharp[using the new constraint](snippets/GenericWhereConstraints.cs#5)]

`new()` 约束出现在 `where` 子句的最后。 `new()` 约束不能与 `struct` 或 `unmanaged` 约束结合使用。 所有满足这些约束的类型必须具有可访问的无参数构造函数，这使得 `new()` 约束冗余。

对于多个类型参数，每个类型参数都使用一个 `where` 子句，例如：

[!code-csharp[using multiple where constraints](snippets/GenericWhereConstraints.cs#6)]

还可将约束附加到泛型方法的类型参数，如以下示例所示：

[!code-csharp[where constraints with generic methods](snippets/GenericWhereConstraints.cs#7)]

请注意，对于委托和方法两者来说，描述类型参数约束的语法是一样的：

[!code-csharp[where constraints with generic methods](snippets/GenericWhereConstraints.cs#8)]

有关泛型委托的信息，请参阅[泛型委托](../../programming-guide/generics/generic-delegates.md)。

有关约束的语法和用法的详细信息，请参阅[类型参数的约束](../../programming-guide/generics/constraints-on-type-parameters.md)。

## <a name="c-language-specification"></a>C# 语言规范

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [泛型介绍](../../programming-guide/generics/index.md)
- [new 约束](./new-constraint.md)
- [类型参数的约束](../../programming-guide/generics/constraints-on-type-parameters.md)
