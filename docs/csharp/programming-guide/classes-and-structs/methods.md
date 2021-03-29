---
title: 方法 - C# 编程指南
description: C# 中的方法是包含一系列语句的代码块。 程序通过调用该方法并指定参数来运行语句。
ms.date: 03/08/2021
helpviewer_keywords:
- methods [C#]
- C# language, methods
ms.assetid: cc738f07-e8cd-4683-9585-9f40c0667c37
ms.openlocfilehash: d503c394e02f6f384e63de4fcd9cc8d2eec43da0
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653499"
---
# <a name="methods-c-programming-guide"></a>方法（C# 编程指南）

方法是包含一系列语句的代码块。 程序通过调用该方法并指定任何所需的方法参数使语句得以执行。 在 C# 中，每个执行的指令均在方法的上下文中执行。

`Main` 方法是每个 C# 应用程序的入口点，并在启动程序时由公共语言运行时 (CLR) 调用。 在使用[顶级语句](../main-and-command-args/top-level-statements.md)的应用程序中，`Main` 方法由编译器生成并包含所有顶级语句。

> [!NOTE]
> 本文讨论命名的方法。 有关匿名函数的信息，请参阅[匿名函数](../statements-expressions-operators/anonymous-functions.md)。

## <a name="method-signatures"></a>方法签名

通过指定访问级别（如 `public` 或 `private`）、可选修饰符（如 `abstract` 或 `sealed`）、返回值、方法的名称以及任何方法参数，在[类](../../language-reference/keywords/class.md)、[结构](../../language-reference/builtin-types/struct.md)或[接口](../interfaces/index.md)中声明方法。 这些部件一起构成方法的签名。

> [!IMPORTANT]
> 出于方法重载的目的，方法的返回类型不是方法签名的一部分。 但是在确定委托和它所指向的方法之间的兼容性时，它是方法签名的一部分。

方法参数在括号内，并且用逗号分隔。 空括号指示方法不需要任何参数。 此类包含四种方法：

[!code-csharp[DifferentModifiersOnMethods#1](snippets/methods/Program.cs#1)]

## <a name="method-access"></a>方法访问

调用对象上的方法就像访问字段。 在对象名之后添加一个句点、方法名和括号。 参数列在括号里，并且用逗号分隔。 因此，可在以下示例中调用 `Motorcycle` 类的方法：

[!code-csharp[CallingMethods#2](snippets/methods/Program.cs#2)]

## <a name="method-parameters-vs-arguments"></a>方法形参与实参

该方法定义指定任何所需参数的名称和类型。 调用代码调用该方法时，它为每个参数提供了称为参数的具体值。 参数必须与参数类型兼容，但调用代码中使用的参数名（如果有）不需要与方法中定义的参数名相同。 例如：

[!code-csharp[MethodExamples#3](snippets/methods/Program.cs#3)]

## <a name="passing-by-reference-vs-passing-by-value"></a>按引用传递与按值传递

默认情况下，将[值类型](../../language-reference/builtin-types/value-types.md)的实例传递给方法时，传递的是其副本而不是实例本身。 因此，对参数的更改不会影响调用方法中的原始实例。 若要按引用传递值类型实例，请使用 `ref` 关键字。 有关详细信息，请参阅[传递值类型参数](./passing-value-type-parameters.md)。

引用类型的对象传递到方法中时，将传递对对象的引用。 也就是说，该方法接收的不是对象本身，而是指示该对象位置的参数。 如果通过使用此引用更改对象的成员，即使是按值传递该对象，此更改也会反映在调用方法的参数中。

通过使用 `class` 关键字创建引用类型，如以下示例所示：

[!code-csharp[SampleRefTypeClass#4](snippets/methods/Program.cs#4)]

现在，如果将基于此类型的对象传递到方法，则将传递对对象的引用。 下面的示例将 `SampleRefType` 类型的对象传递到 `ModifyObject` 方法：

[!code-csharp[PassingAReferenceType#5](snippets/methods/Program.cs#5)]

该示例执行的内容实质上与先前示例相同，均按值将自变量传递到方法。 但是因为使用了引用类型，结果有所不同。 `ModifyObject` 中所做的对形参 `value` 的 `obj`字段的修改，也会更改 `value` 方法中实参 `rt`的 `TestRefType` 字段。 `TestRefType` 方法显示 33 作为输出。

有关如何通过引用和值传递引用类型的详细信息，请参阅[传递引用类型参数](./passing-reference-type-parameters.md)和[引用类型](../../language-reference/keywords/reference-types.md)。

## <a name="return-values"></a>返回值

方法可以将值返回到调用方。 如果列在方法名之前的返回类型不是 `void`，则该方法可通过使用 `return` 关键字返回值。 带 `return` 关键字，后跟与返回类型匹配的值的语句将该值返回到方法调用方。

值可以按值或[按引用](ref-returns.md)返回到调用方，以 C# 7.0 开头。 如果在方法签名中使用 `ref` 关键字且其跟随每个 `return` 关键字，值将按引用返回到调用方。 例如，以下方法签名和返回语句指示该方法按对调用方的引用返回变量名 `estDistance`。

```csharp
public ref double GetEstimatedDistance()
{
    return ref estDistance;
}
```

`return` 关键字还会停止执行该方法。 如果返回类型为 `void`，没有值的 `return` 语句仍可用于停止执行该方法。 没有 `return` 关键字，当方法到达代码块结尾时，将停止执行。 具有非空的返回类型的方法都需要使用 `return` 关键字来返回值。 例如，这两种方法都使用 `return` 关键字来返回整数：

[!code-csharp[SimpleMathClass#6](snippets/methods/Program.cs#6)]

若要使用从方法返回的值，调用方法可以在相同类型的值足够的地方使用该方法调用本身。 也可以将返回值分配给变量。 例如，以下两个代码示例实现了相同的目标：

[!code-csharp[SquareANumberWithAddTwoNumbersUsingLocalVariable#7](snippets/methods/Program.cs#7)]

[!code-csharp[SquareANumberWithAddTwoNumbersInTheSameLine#8](snippets/methods/Program.cs#8)]

在这种情况下，使用本地变量 `result`存储值是可选的。 此步骤可以帮助提高代码的可读性，或者如果需要存储该方法整个范围内自变量的原始值，则此步骤可能很有必要。

若要使用按引用从方法返回的值，必须声明 [ref local](ref-returns.md#ref-locals) 变量（如果想要修改其值）。 例如，如果 `Planet.GetEstimatedDistance` 方法按引用返回 <xref:System.Double> 值，则可以将其定义为具有如下所示代码的 ref local 变量：

```csharp
ref int distance = plant
```

如果调用函数将数组传递到 `M`，则无需从修改数组内容的方法 `M` 返回多维数组。  你可能会从 `M` 返回生成的数组以获得值的良好样式或功能流，但这是不必要的，因为 C# 按值传递所有引用类型，且数组引用的值是指向数组的指针。 在方法 `M` 中，引用该数组的任何代码都能观察到数组内容的任何更改，如以下示例所示：

```csharp
static void Main(string[] args)
{
    int[,] matrix = new int[2, 2];
    FillMatrix(matrix);
    // matrix is now full of -1
}

public static void FillMatrix(int[,] matrix)
{
    for (int i = 0; i < matrix.GetLength(0); i++)
    {
        for (int j = 0; j < matrix.GetLength(1); j++)
        {
            matrix[i, j] = -1;
        }
    }
}
```

有关详细信息，请参阅 [return](../../language-reference/keywords/return.md)。

## <a name="async-methods"></a>异步方法

通过使用异步功能，你可以调用异步方法而无需使用显式回调，也不需要跨多个方法或 lambda 表达式来手动拆分代码。

如果用 [async](../../language-reference/keywords/async.md) 修饰符标记方法，则可以在该方法中使用 [await](../../language-reference/operators/await.md) 运算符。 当控件到达异步方法中的 await 表达式时，控件将返回到调用方，并在等待任务完成前，方法中进度将一直处于挂起状态。 任务完成后，可以在方法中恢复执行。

> [!NOTE]
> 异步方法在遇到第一个尚未完成的 awaited 对象或到达异步方法的末尾时（以先发生者为准），将返回到调用方。

异步方法通常具有 <xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.Task>、<xref:System.Collections.Generic.IAsyncEnumerable%601> 或 `void` 返回类型。 `void` 返回类型主要用于定义需要 `void` 返回类型的事件处理程序。 无法等待返回 `void` 的异步方法，并且返回 void 方法的调用方无法捕获该方法引发的异常。 从 C# 7.0 开始，异步方法可以有[任何类似任务的返回类型](../../whats-new/csharp-7.md#generalized-async-return-types)。

在以下示例中， `DelayAsync` 是具有 <xref:System.Threading.Tasks.Task%601>返回类型的异步方法。 `DelayAsync` 具有返回整数的 `return` 语句。 因此， `DelayAsync` 的方法声明必须具有 `Task<int>`的返回类型。 因为返回类型是 `Task<int>`， `await` 中 `DoSomethingAsync` 表达式的计算如以下语句所示得出整数： `int result = await delayTask`。

`Main` 方法就是一个具有 <xref:System.Threading.Tasks.Task> 返回类型的异步方法示例。 它会转到 `DoSomethingAsync` 方法，因为它使用单个行进行表示，所以可省略 `async` 和 `await` 关键字。 因为 `DoSomethingAsync` 是异步方法，调用 `DoSomethingAsync` 的任务必须等待，如以下语句所示： `await DoSomethingAsync();`。

:::code language="csharp" source="snippets/classes-and-structs/methods/Program.cs":::

异步方法不能声明任何 [ref](../../language-reference/keywords/ref.md) 或 [out](../../language-reference/keywords/out-parameter-modifier.md) 参数，但是可以调用具有这类参数的方法。

有关异步方法的详细信息，请参阅[使用 Async 和 Await 的异步编程](../concepts/async/index.md)和[异步返回类型](../concepts/async/async-return-types.md)。

## <a name="expression-body-definitions"></a>表达式主体定义

具有立即仅返回表达式结果，或单个语句作为方法主题的方法定义很常见。 以下是使用 `=>`定义此类方法的语法快捷方式：

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

如果该方法返回 `void` 或是异步方法，则该方法的主体必须是语句表达式（与 lambda 相同）。 对于属性和索引器，两者必须是只读的，并且不使用 `get` 访问器关键字。

## <a name="iterators"></a>迭代器

迭代器对集合执行自定义迭代，如列表或数组。 迭代器使用 [yield return](../../language-reference/keywords/yield.md) 语句返回元素，每次返回一个。 当 [yield return](../../language-reference/keywords/yield.md) 语句到达时，将记住当前在代码中的位置。 下次调用迭代器时，将从该位置重新开始执行。

通过使用 [foreach](../../language-reference/keywords/foreach-in.md) 语句从客户端代码调用迭代器。

迭代器的返回类型可以是 <xref:System.Collections.IEnumerable>、 <xref:System.Collections.Generic.IEnumerable%601>、 <xref:System.Collections.IEnumerator>或 <xref:System.Collections.Generic.IEnumerator%601>。

有关更多信息，请参见 [迭代器](../concepts/iterators.md)。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [类和结构](index.md)
- [访问修饰符](access-modifiers.md)
- [静态类和静态类成员](static-classes-and-static-class-members.md)
- [继承](inheritance.md)
- [抽象类、密封类及类成员](abstract-and-sealed-classes-and-class-members.md)
- [params](../../language-reference/keywords/params.md)
- [return](../../language-reference/keywords/return.md)
- [out](../../language-reference/keywords/out.md)
- [ref](../../language-reference/keywords/ref.md)
- [传递参数](passing-parameters.md)
