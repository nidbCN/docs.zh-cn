---
title: 方法 - C# 指南
description: 有关方法、方法参数和方法返回值的概述
ms.technology: csharp-fundamentals
ms.date: 03/16/2021
ms.assetid: 577a8527-1081-4b36-9b9e-0685b6553c6e
ms.openlocfilehash: 5d7f654ca268deff4a0c8e69b76e4d636d2f495e
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652927"
---
# <a name="methods-in-c"></a>(C#) 中的方法

方法是包含一系列语句的代码块。 程序通过调用该方法并指定任何所需的方法参数使语句得以执行。 在 C# 中，每个执行的指令均在方法的上下文中执行。 `Main` 方法是每个 C# 应用程序的入口点，并在启动程序时由公共语言运行时 (CLR) 调用。

> [!NOTE]
> 本主题讨论命名的方法。 有关匿名函数的信息，请参阅[匿名函数](programming-guide/statements-expressions-operators/anonymous-functions.md)。

<a name="signatures"></a>

## <a name="method-signatures"></a>方法签名

通过指定在 `class` 或 `struct` 中声明方法：

- 可选的访问级别，如 `public` 或 `private`。 默认值为 `private`。
- 可选的修饰符，如 `abstract` 或 `sealed`。
- 返回值，或 `void`（如果该方法不具有）。
- 方法名称。
- 任何方法参数。 方法参数在括号内，并且用逗号分隔。 空括号指示方法不需要任何参数。

这些部分一同构成方法签名。

> [!IMPORTANT]
> 出于方法重载的目的，方法的返回类型不是方法签名的一部分。 但是在确定委托和它所指向的方法之间的兼容性时，它是方法签名的一部分。

以下实例定义了一个包含五种方法的名为 `Motorcycle` 的类：

[!code-csharp[csSnippets.Methods#40](../../samples/snippets/csharp/concepts/methods/methods40.cs#40)]

请注意，`Motorcycle` 类包括一个重载的方法 `Drive`。 两个方法具有相同的名称，但必须根据其参数类型来区分。

<a name="invocation"></a>

## <a name="method-invocation"></a>方法调用

方法可以是实例的或静态的。 调用实例方法需要将对象实例化，并对该对象调用方法；实例方法可对该实例及其数据进行操作。 通过引用该方法所属类型的名称来调用静态方法；静态方法不对实例数据进行操作。 尝试通过对象实例调用静态方法会引发编译器错误。

调用方法就像访问字段。 在对象名称（如果调用实例方法）或类型名称（如果调用 `static` 方法）后添加一个句点、方法名称和括号。 自变量列在括号里，并且用逗号分隔。

该方法定义指定任何所需参数的名称和类型。 调用方调用该方法时，它为每个参数提供了称为自变量的具体值。 自变量必须与参数类型兼容，但调用代码中使用的自变量名（如果有）不需要与方法中定义的自变量名相同。 在下面示例中，`Square` 方法包含名为 i 的类型为 `int` 的单个参数。 第一种方法调用将向 `Square` 方法传递名为 num 的 `int` 类型的变量；第二个方法调用将传递数值常量；第三个方法调用将传递表达式。

[!code-csharp[csSnippets.Methods#74](../../samples/snippets/csharp/concepts/methods/params74.cs#74)]

方法调用最常见的形式是使用位置自变量；它会以与方法参数相同的顺序提供自变量。 因此，可在以下示例中调用 `Motorcycle` 类的方法。 例如，`Drive` 方法的调用包含两个与方法语法中的两个参数对应的自变量。 第一个成为 `miles` 参数的值，第二个成为 `speed` 参数的值。

[!code-csharp[csSnippets.Methods#41](../../samples/snippets/csharp/concepts/methods/methods40.cs#41)]

调用方法时，也可以使用命名的自变量，而不是位置自变量。 使用命名的自变量时，指定参数名，然后后跟冒号（":"）和自变量。 只要包含了所有必需的自变量，方法的自变量可以任何顺序出现。 下面的示例使用命名的自变量来调用 `TestMotorcycle.Drive` 方法。 在此示例中，命名的自变量以相反于方法参数列表中的顺序进行传递。

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/named1.cs#45)]

可以同时使用位置自变量和命名的自变量调用方法。 但是，只有当命名参数位于正确位置时，才能在命名自变量后面放置位置参数。 下面的示例使用一个位置自变量和一个命名的自变量从上一个示例中调用 `TestMotorcycle.Drive` 方法。

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/named2.cs#46)]

<a name="inherited"></a>

## <a name="inherited-and-overridden-methods"></a>继承和重写方法

除了类型中显式定义的成员，类型还继承在其基类中定义的成员。 由于托管类型系统中的所有类型都直接或间接继承自 <xref:System.Object> 类，因此所有类型都继承其成员，如 <xref:System.Object.Equals(System.Object)>、<xref:System.Object.GetType> 和 <xref:System.Object.ToString>。 下面的示例定义 `Person` 类，实例化两个 `Person` 对象，并调用 `Person.Equals` 方法来确定两个对象是否相等。 但是，`Equals` 方法不是在 `Person` 类中定义；而是继承自 <xref:System.Object>。

[!code-csharp[csSnippets.Methods#104](../../samples/snippets/csharp/concepts/methods/inherited1.cs#104)]

类型可以使用 `override` 关键字并提供重写方法的实现来重写继承的成员。 方法签名必须与重写的方法的签名一样。 下面的示例类似于上一个示例，只不过它重写 <xref:System.Object.Equals(System.Object)> 方法。 （它还重写 <xref:System.Object.GetHashCode> 方法，因为这两种方法用于提供一致的结果。）

[!code-csharp[csSnippets.Methods#105](../../samples/snippets/csharp/concepts/methods/overridden1.cs#105)]

<a name="passing"></a>

## <a name="passing-parameters"></a>快速参考

C# 中的所有类型不是值类型就是引用类型。 有关内置值类型的列表，请参阅[类型](./tour-of-csharp/types.md)。 默认情况下，值类型和引用类型均按值传递给方法。

<a name="byval"></a>

### <a name="passing-parameters-by-value"></a>按值传递参数

值类型按值传递给方法时，传递的是对象的副本而不是对象本身。 因此，当控件返回调用方时，对已调用方法中的对象的更改对原始对象无影响。

下面的示例按值向方法传递值类型，且调用的方法尝试更改值类型的值。 它定义属于值类型的 `int` 类型的变量，将其值初始化为 20，并将该类型传递给将变量值改为 30 的名为 `ModifyValue` 的方法。 但是，返回方法时，变量的值保持不变。

[!code-csharp[csSnippets.Methods#10](../../samples/snippets/csharp/concepts/methods/byvalue10.cs#10)]

引用类型的对象按值传递到方法中时，将按值传递对对象的引用。 也就是说，该方法接收的不是对象本身，而是指示该对象位置的自变量。 控件返回到调用方法时，如果通过使用此引用更改对象的成员，此更改将反映在对象中。 但是，当控件返回到调用方时，替换传递到方法的对象对原始对象无影响。

下面的示例定义名为 `SampleRefType` 的类（属于引用类型）。 它实例化 `SampleRefType` 对象，将 44 赋予其 `value` 字段，并将该对象传递给 `ModifyObject` 方法。 该示例执行的内容实质上与先前示例相同，均按值将自变量传递到方法。 但因为使用了引用类型，结果会有所不同。 `ModifyObject` 中所做的对 `obj.value` 字段的修改，也会将 `Main` 方法中的自变量 `rt` 的 `value` 字段更改为 33，如示例中的输出值所示。

[!code-csharp[csSnippets.Methods#42](../../samples/snippets/csharp/concepts/methods/byvalue42.cs#42)]

<a name="byref"></a>

### <a name="passing-parameters-by-reference"></a>按引用传递参数

如果想要更改方法中的自变量值并想要在控件返回到调用方法时反映出这一更改，请按引用传递参数。 要按引用传递参数，请使用 [`ref`](language-reference/keywords/ref.md) 或 [`out`](language-reference/keywords/out-parameter-modifier.md) 关键字。 还可以使用 [`in`](language-reference/keywords/in-parameter-modifier.md) 关键字，按引用传递值以避免复制，但仍防止修改。

下面的示例与上一个示例完全一样，只是换成按引用将值传递给 `ModifyValue` 方法。 参数值在 `ModifyValue` 方法中修改时，值中的更改将在控件返回调用方时反映出来。

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/byref106.cs#106)]

引用参数所使用的常见模式涉及交换变量值。 将两个变量按引用传递给一个方法，然后该方法将二者内容进行交换。 下面的示例交换整数值。

[!code-csharp[csSnippets.Methods#106](../../samples/snippets/csharp/concepts/methods/swap107.cs#107)]

通过传递引用类型的参数，可以更改引用本身的值，而不是其单个元素或字段的值。

<a name="paramarray"></a>

### <a name="parameter-arrays"></a>参数数组

有时，向方法指定精确数量的自变量这一要求是受限的。 通过使用 `params` 关键字来指示一个参数是一个参数数组，可通过可变数量的自变量来调用方法。 使用 `params` 关键字标记的参数必须为数组类型，并且必须是该方法的参数列表中的最后一个参数。

然后，调用方可通过以下四种方式中的任一种来调用方法：

- 传递相应类型的数组，该类型包含所需数量的元素。
- 向该方法传递相应类型的单独自变量的逗号分隔列表。
- 传递 `null`。
- 不向参数数组提供参数。

以下示例定义了一个名为 `GetVowels` 的方法，该方法返回参数数组中的所有元音。 `Main` 方法演示了调用方法的全部四种方式。 调用方不需要为包含 `params` 修饰符的形参提供任何实参。 在这种情况下，参数是一个空数组。

[!code-csharp[csSnippets.Methods#75](~/samples/snippets/csharp/concepts/methods/params75.cs#75)]

<a name="optional"></a>

## <a name="optional-parameters-and-arguments"></a>可选参数和自变量

方法定义可指定其参数是必需的还是可选的。 默认情况下，参数是必需的。 通过在方法定义中包含参数的默认值来指定可选参数。 调用该方法时，如果未向可选参数提供自变量，则改为使用默认值。

参数的默认值必须由以下几种表达式中的一种来赋予：

- 常量，例如文本字符串或数字。
- `new ValType()` 形式的表达式，其中 `ValType` 是值类型。 请注意，这会调用该值类型的隐式无参数构造函数，该函数不是类型的实际成员。
- `default(ValType)` 形式的表达式，其中 `ValType` 是值类型。

如果某个方法同时包含必需的和可选的参数，则在参数列表末尾定义可选参数，即在定义完所有必需参数之后定义。

下面的示例定义方法 `ExampleMethod`，它具有一个必需参数和两个可选参数。

[!code-csharp[csSnippets.Methods#21](../../samples/snippets/csharp/concepts/methods/optional1.cs#21)]

如果使用位置自变量调用包含多个可选自变量的方法，调用方必须逐一向所有需要自变量的可选参数提供自变量。 例如，在使用 `ExampleMethod` 方法的情况下，如果调用方向 `description` 参数提供自变量，还必须向 `optionalInt` 参数提供一个自变量。 `opt.ExampleMethod(2, 2, "Addition of 2 and 2");` 是一个有效的方法调用；`opt.ExampleMethod(2, , "Addition of 2 and 0");` 生成编译器错误“缺少自变量”。

如果使用命名的自变量或位置自变量和命名的自变量的组合来调用某个方法，调用方可以省略方法调用中的最后一个位置自变量后的任何自变量。

下面的示例三次调用了 `ExampleMethod` 方法。  前两个方法调用使用位置自变量。 第一个方法同时省略了两个可选自变量，而第二个省略了最后一个自变量。 第三个方法调用向必需的参数提供位置自变量，但使用命名的自变量向 `description` 参数提供值，同时省略 `optionalInt` 自变量。

[!code-csharp[csSnippets.Methods#22](../../samples/snippets/csharp/concepts/methods/optional1.cs#22)]

使用可选参数会影响重载决策，或影响 C# 编译器决定方法应调用哪个特定重载时所使用的方式，如下所示：

- 如果方法、索引器或构造函数的每个参数是可选的，或按名称或位置对应于调用语句中的单个自变量，且该自变量可转换为参数的类型，则方法、索引器或构造函数为执行的候选项。
- 如果找到多个候选项，则会将用于首选转换的重载决策规则应用于显式指定的自变量。 将忽略可选形参已省略的实参。
- 如果两个候选项不相上下，则会将没有可选形参的候选项作为首选项，对于这些可选形参，已在调用中为其省略了实参。 这是重载决策中的常规引用的结果，该引用用于参数较少的候选项。

<a name="return"></a>

## <a name="return-values"></a>返回值

方法可以将值返回到调用方。 如果列在方法名之前的返回类型不是 `void`，则该方法可通过使用 `return` 关键字返回值。 带 `return` 关键字且后跟与返回类型匹配的变量、常数或表达式的语句将向方法调用方返回该值。 具有非空的返回类型的方法都需要使用 `return` 关键字来返回值。 `return` 关键字还会停止执行该方法。

如果返回类型为 `void`，没有值的 `return` 语句仍可用于停止执行该方法。 没有 `return` 关键字，当方法到达代码块结尾时，将停止执行。

例如，这两种方法都使用 `return` 关键字来返回整数：

[!code-csharp[csSnippets.Methods#44](../../samples/snippets/csharp/concepts/methods/return44.cs#44)]

若要使用从方法返回的值，调用方法可以在相同类型的值足够的地方使用该方法调用本身。 也可以将返回值分配给变量。 例如，以下两个代码示例实现了相同的目标：

[!code-csharp[csSnippets.Methods#45](../../samples/snippets/csharp/concepts/methods/return44.cs#45)]

[!code-csharp[csSnippets.Methods#46](../../samples/snippets/csharp/concepts/methods/return44.cs#46)]

在这种情况下，使用本地变量 `result`存储值是可选的。 此步骤可以帮助提高代码的可读性，或者如果需要存储该方法整个范围内自变量的原始值，则此步骤可能很有必要。

有时，需要方法返回多个值。 从 C# 7.0 开始，可以使用元组类型和元组文本轻松实现此目的。 元组类型定义元组元素的数据类型。 元组文本提供返回的元组的实际值。 在下面的示例中，`(string, string, string, int)` 定义 `GetPersonalInfo` 方法返回的元组类型。 表达式 `(per.FirstName, per.MiddleName, per.LastName, per.Age)` 是元组文本；方法返回 `PersonInfo` 对象的第一个、中间和最后一个名称及其使用期限。

```csharp
public (string, string, string, int) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

然后调用方可通过类似以下的代码使用返回的元组：

```csharp
var person = GetPersonalInfo("111111111")
Console.WriteLine($"{person.Item1} {person.Item3}: age = {person.Item4}");
```

还可向元组类型定义中的元组元素分配名称。 下面的示例展示 `GetPersonalInfo` 方法的替代版本，该方法使用命名的元素：

```csharp
public (string FName, string MName, string LName, int Age) GetPersonalInfo(string id)
{
    PersonInfo per = PersonInfo.RetrieveInfoById(id);
    return (per.FirstName, per.MiddleName, per.LastName, per.Age);
}
```

然后可修改上一次对 `GetPersonInfo` 方法的调用，如下所示：

```csharp
var person = GetPersonalInfo("111111111");
Console.WriteLine($"{person.FName} {person.LName}: age = {person.Age}");
```

如果将数组作为自变量传递给一个方法，并修改各个元素的值，则该方法不一定会返回该数组，尽管选择这么操作的原因是为了实现更好的样式或功能性的值流。  这是因为 C# 会按值传递所有引用类型，而数组引用的值是指向该数组的指针。 在下面的示例中，引用该数组的任何代码都能观察到在 `DoubleValues` 方法中对 `values` 数组内容的更改。

[!code-csharp[csSnippets.Methods#101](../../samples/snippets/csharp/concepts/methods/returnarray1.cs#101)]

<a name="extension"></a>

## <a name="extension-methods"></a>扩展方法

通常，可以通过两种方式向现有类型添加方法：

- 修改该类型的源代码。 当然，如果并不拥有该类型的源代码，则无法执行该操作。 并且，如果还添加任何专用数据字段来支持该方法，这会成为一项重大更改。
- 在派生类中定义新方法。 无法使用其他类型（如结构和枚举）的继承来通过此方式添加方法。 也不能使用此方式向封闭类“添加”方法。

使用扩展方法，可向现有类型“添加”方法，而无需修改类型本身或在继承的类型中实现新方法。 扩展方法也无需驻留在与其扩展的类型相同的程序集中。 要把扩展方法当作是定义的类型成员一样调用。

有关详细信息，请参阅[扩展方法](programming-guide/classes-and-structs/extension-methods.md)

<a name="async"></a>

## <a name="async-methods"></a>异步方法

通过使用异步功能，你可以调用异步方法而无需使用显式回调，也不需要跨多个方法或 lambda 表达式来手动拆分代码。

如果用 [async](language-reference/keywords/async.md) 修饰符标记方法，则可以在该方法中使用 [await](language-reference/operators/await.md) 运算符。 当控件到达异步方法中的 `await` 表达式时，如果等待的任务未完成，控件将返回到调用方，并在等待任务完成前，包含 `await` 关键字的方法中的进度将一直处于挂起状态。 任务完成后，可以在方法中恢复执行。

> [!NOTE]
> 异步方法在遇到第一个尚未完成的 awaited 对象或到达异步方法的末尾时（以先发生者为准），将返回到调用方。

异步方法通常具有 <xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.Task>、<xref:System.Collections.Generic.IAsyncEnumerable%601> 或 `void` 返回类型。 `void` 返回类型主要用于定义需要 `void` 返回类型的事件处理程序。 无法等待返回 `void` 的异步方法，并且返回 void 方法的调用方无法捕获该方法引发的异常。 从 C# 7.0 开始，异步方法可以有[任何类似任务的返回类型](./whats-new/csharp-7.md#generalized-async-return-types)。

在下面的示例中，`DelayAsync` 是一个异步方法，包含返回整数的 return 语句。 由于它是异步方法，其方法声明必须具有返回类型 `Task<int>`。 因为返回类型是 `Task<int>`，`DoSomethingAsync` 中 `await` 表达式的计算将如以下 `int result = await delayTask` 语句所示得出整数。

:::code language="csharp" source="programming-guide/classes-and-structs/snippets/classes-and-structs/methods/Program.cs":::

异步方法不能声明任何 [in](language-reference/keywords/in-parameter-modifier.md)、[ref](language-reference/keywords/ref.md) 或 [out](language-reference/keywords/out-parameter-modifier.md) 参数，但是可以调用具有这类参数的方法。

 有关异步方法的详细信息，请参阅[使用 Async 和 Await 的异步编程](async.md)和[异步返回类型](programming-guide/concepts/async/async-return-types.md)。

<a name="expr"></a>

## <a name="expression-bodied-members"></a>Expression-Bodied 成员

具有立即仅返回表达式结果，或单个语句作为方法主题的方法定义很常见。  以下是使用 `=>`定义此类方法的语法快捷方式：

```csharp
public Point Move(int dx, int dy) => new Point(x + dx, y + dy);
public void Print() => Console.WriteLine(First + " " + Last);
// Works with operators, properties, and indexers too.
public static Complex operator +(Complex a, Complex b) => a.Add(b);
public string Name => First + " " + Last;
public Customer this[long id] => store.LookupCustomer(id);
```

如果该方法返回 `void` 或是异步方法，则该方法的主体必须是语句表达式（与 lambda 相同）。  对于属性和索引器，两者必须是只读，并且不使用 `get` 访问器关键字。

<a name="iterators"></a>

## <a name="iterators"></a>迭代器

迭代器对集合执行自定义迭代，如列表或数组。 迭代器使用 [yield return](language-reference/keywords/yield.md) 语句返回元素，每次返回一个。 到达 `yield return` 语句后，会记住当前位置，以便调用方可以请求序列中的下一个元素。

迭代器的返回类型可以是 <xref:System.Collections.IEnumerable>、 <xref:System.Collections.Generic.IEnumerable%601>、 <xref:System.Collections.IEnumerator>或 <xref:System.Collections.Generic.IEnumerator%601>。

有关更多信息，请参见 [迭代器](programming-guide/concepts/iterators.md)。

## <a name="see-also"></a>另请参阅

- [访问修饰符](language-reference/keywords/access-modifiers.md)
- [静态类和静态类成员](programming-guide/classes-and-structs/static-classes-and-static-class-members.md)
- [继承](programming-guide/classes-and-structs/inheritance.md)
- [抽象类、密封类及类成员](programming-guide/classes-and-structs/abstract-and-sealed-classes-and-class-members.md)
- [params](language-reference/keywords/params.md)
- [out](language-reference/keywords/out-parameter-modifier.md)
- [ref](language-reference/keywords/ref.md)
- [in](language-reference/keywords/in-parameter-modifier.md)
- [传递参数](programming-guide/classes-and-structs/passing-parameters.md)
