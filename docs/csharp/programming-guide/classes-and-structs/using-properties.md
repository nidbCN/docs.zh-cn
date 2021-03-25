---
title: 使用属性 - C# 编程指南
description: 这些示例演示如何使用 C# 中的属性。 了解 get 和 set 访问器如何实现读取和写入访问，并了解属性的用法。
ms.date: 07/20/2015
helpviewer_keywords:
- set accessor [C#]
- get accessor [C#]
- properties [C#], about properties
ms.assetid: f7f67b05-0983-4cdb-96af-1855d24c967c
ms.openlocfilehash: 16ff0f02db9640ad8cfe41fce9ce954cb75b4e08
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190328"
---
# <a name="using-properties-c-programming-guide"></a>使用属性（C# 编程指南）

属性结合了字段和方法的多个方面。 对于对象的用户来说，属性似乎是一个字段，访问属性需要相同的语法。 对于类的实现者来说，属性是一两个代码块，表示 [get](../../language-reference/keywords/get.md) 访问器和/或 [set](../../language-reference/keywords/set.md) 访问器。 读取属性时，执行 `get` 访问器的代码块；向属性赋予新值时，执行 `set` 访问器的代码块。 将不带 `set` 访问器的属性视为只读。 将不带 `get` 访问器的属性视为只写。 将具有以上两个访问器的属性视为读写。 在 C# 9 及更高版本中，可以使用 `init` 访问器代替 `set` 访问器将属性设为只读。

与字段不同，属性不会被归类为变量。 因此，不能将属性作为 [ref](../../language-reference/keywords/ref.md) 或 [out](../../language-reference/keywords/out-parameter-modifier.md) 参数传递。

属性具有许多用途：它们可以先验证数据，再允许进行更改；可以在类上透明地公开数据，其中数据实际是从某个其他源（如数据库）检索到的；可以在数据发生更改时采取措施，例如引发事件或更改其他字段的值。

通过依次指定字段的访问级别、属性类型、属性名、声明 `get` 访问器和/或 `set` 访问器的代码块，在类块中声明属性。 例如：

[!code-csharp[csProgGuideProperties#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#7)]

此示例中将 `Month` 声明为属性，以便 `set` 访问器可确保 `Month` 值设置在 1 至 12 之间。 `Month` 属性使用私有字段跟踪实际值。 属性的数据的实际位置通常被称为属性的“后备存储”。 属性使用私有字段作为后备存储，这很常见。 将字段被标记为私有，确保只能通过调用属性来对其进行更改。 有关公共和专用访问限制的详细信息，请参阅[访问修饰符](./access-modifiers.md)。

自动实现的属性为简单属性声明提供简化的语法。 有关详细信息，请参阅[自动实现的属性](auto-implemented-properties.md)。

## <a name="the-get-accessor"></a>get 访问器

`get` 访问器的正文类似于方法。 它必须返回属性类型的值。 执行 `get` 访问器等效于读取字段的值。 例如，在从 `get` 访问器返回私有变量且已启用优化时，对 `get` 访问器方法的调用由编译器内联，因此不存在方法调用开销。 但是，无法内联虚拟 `get` 访问器方法，因为编译器在编译时不知道在运行时实际可调用哪些方法。 以下是一个 `get` 访问器，它返回私有字段 `_name` 的值：

[!code-csharp[csProgGuideProperties#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#8)]

引用属性时，除了作为赋值目标外，还调用 `get` 访问器读取属性值。 例如：

[!code-csharp[csProgGuideProperties#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#9)]

`get` 访问器必须以 [return](../../language-reference/keywords/return.md) 或 [throw](../../language-reference/keywords/throw.md) 语句结尾，且控件不能超出访问器正文。

使用 `get` 访问器更改对象的状态是一种糟糕的编程风格。 例如，以下访问器将产生在每次访问 `_number` 字段时更改对象状态的副作用。

[!code-csharp[csProgGuideProperties#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#10)]

`get` 访问器可以用于返回字段值或计算并返回字段值。 例如：

[!code-csharp[csProgGuideProperties#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#11)]

在上一个代码段中，如果不给 `Name` 属性赋值，它将返回值 `NA`。

## <a name="the-set-accessor"></a>set 访问器

`set` 访问器类似于返回类型为 [void](../../language-reference/builtin-types/void.md) 的方法。 它使用名为 `value` 的隐式参数，该参数的类型为属性的类型。 在下面的示例中，将 `set` 访问器添加到 `Name` 属性：

[!code-csharp[csProgGuideProperties#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#12)]

向属性赋值时，通过使用提供新值的自变量调用 `set` 访问器。 例如：

[!code-csharp[csProgGuideProperties#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#13)]

为 `set` 访问器中的本地变量声明使用隐式参数名 `value` 是错误的。

## <a name="the-init-accessor"></a>init 访问器

用于创建 `init` 访问器的代码与用于创建 `set` 访问器的代码相同，只不过前者使用的关键字是 `init` 而不是 `set`。 不同之处在于，`init` 访问器只能在构造函数中使用，或通过[对象初始值设定项](object-and-collection-initializers.md)使用。

## <a name="remarks"></a>备注

可以将属性标记为 `public`、`private`、`protected`、`internal`、`protected internal` 或 `private protected`。 这些访问修饰符定义该类的用户访问该属性的方式。 相同属性的 `get` 和 `set` 访问器可以具有不同的访问修饰符。 例如，`get` 可能为 `public`允许从类型外部进行只读访问；而 `set` 可能为 `private` 或 `protected`。 有关详细信息，请参阅[访问修饰符](./access-modifiers.md)。

可以通过使用 `static` 关键字将属性声明为静态属性。 这使属性可供调用方在任何时候使用，即使不存在类的任何实例。 有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。

可以通过使用 [virtual](../../language-reference/keywords/virtual.md) 关键字将属性标记为虚拟属性。 这可使派生类使用 [override](../../language-reference/keywords/override.md) 关键字重写属性行为。 有关这些选项的详细信息，请参阅[继承](inheritance.md)。

重写虚拟属性的属性也可以是 [sealed](../../language-reference/keywords/sealed.md)，指定对于派生类，它不再是虚拟的。 最后，可以将属性声明为 [abstract](../../language-reference/keywords/abstract.md)。 这意味着类中没有实现，派生类必须写入自己的实现。 有关这些选项的详细信息，请参阅[抽象类、密封类及类成员](abstract-and-sealed-classes-and-class-members.md)。
  
> [!NOTE]
> 在 [static](../../language-reference/keywords/static.md) 属性的访问器上使用 [virtual](../../language-reference/keywords/virtual.md)、[abstract](../../language-reference/keywords/abstract.md) 或 [override](../../language-reference/keywords/override.md) 修饰符是错误的。

## <a name="examples"></a>示例

此示例演示实例、静态和只读属性。 它接收通过键盘键入的员工姓名，按 1 递增 `NumberOfEmployees`，并显示员工姓名和编号。

[!code-csharp[csProgGuideProperties#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#2)]

## <a name="hidden-property-example"></a>Hidden 属性示例

此示例演示如何访问由派生类中同名的另一属性隐藏的基类中的属性：

[!code-csharp[csProgGuideProperties#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#3)]

以下内容是前一示例中的要点：

- 派生类中的属性 `Name` 隐藏基类中的属性 `Name`。 在这种情况下，`new` 修饰符用于派生类的属性声明中：

     [!code-csharp[csProgGuideProperties#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#4)]  

- `(Employee)` 转换用于访问基类中的隐藏属性：

     [!code-csharp[csProgGuideProperties#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#5)]

     有关隐藏成员的详细信息，请参阅 [new 修饰符](../../language-reference/keywords/new-modifier.md)。

## <a name="override-property-example"></a>Override 属性示例

在此示例中，两个类（`Cube` 和 `Square`）实现抽象类 `Shape`，并重写其抽象 `Area` 属性。 请注意属性上的 [override](../../language-reference/keywords/override.md) 修饰符的使用。 程序接受将边长作为输入，计算正方形和立方体的面积。 它还接受将面积作为输入，计算正方形和立方体的相应边长。

[!code-csharp[csProgGuideProperties#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideProperties/CS/Properties.cs#6)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [属性](properties.md)
- [接口属性](interface-properties.md)
- [自动实现的属性](auto-implemented-properties.md)
