---
title: 编写安全有效的 C# 代码
description: 通过 C# 语言最新增强功能，可以编写可验证的安全代码，这些代码的性能以前与不安全代码相关联。
ms.date: 03/17/2020
ms.technology: csharp-advanced-concepts
ms.custom: mvc
ms.openlocfilehash: 4728ff279d0a14adf239c1e177f7f840ea208e6e
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872478"
---
# <a name="write-safe-and-efficient-c-code"></a>编写安全有效的 C# 代码

借助 C# 中的新增功能可编写性能更好的可验证安全代码。 若仔细地应用这些技术，则需要不安全代码的方案更少。 利用这些功能，可更轻易地将对值类型的引用用作方法参数和方法返回。 安全完成后，这些技术可以最大程度地减少值类型的复制操作。 通过使用值类型，可以使分配和垃圾回收过程的数量降至最低。

本文中的很多示例代码都使用了 C# 7.2 中增加的功能。 要使用这些功能，必须将项目配置为使用 C# 7.2 或更高版本。 有关设置语言版本的详细信息，请参阅[配置语言版本](language-reference/configure-language-version.md)。

本文重点介绍有效资源管理的技术。 使用值类型的优点之一是通常可避免堆分配。 缺点是它们按值进行复制。 由于存在这种折衷，因此难以优化针对大量数据执行的算法。 C# 7.2 中新增的语言功能提供了可使用对值类型的引用来实现安全高效代码的机制。 请恰当地使用这些功能，以最大程度地减少分配和复制操作。 本文将介绍这些新功能。

本文重点介绍以下资源管理技术：

- 声明一个 [`readonly struct`](language-reference/builtin-types/struct.md#readonly-struct) 以表示类型是不可变的  。 这使编译器可以在使用 [`in`](language-reference/keywords/in-parameter-modifier.md) 参数时保存防御性副本。
- 如果类型是可变的，请声明 `struct` 成员 [`readonly`](language-reference/builtin-types/struct.md#readonly-instance-members)，以指示该成员不修改状态。
- 当返回值 `struct` 大于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 且存储生存期大于返回值的方法时，请使用 [`ref readonly`](language-reference/keywords/ref.md#reference-return-values) 返回。
- 当 `readonly struct` 的大小大于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 时，出于性能原因，应将其作为 `in` 参数传递。
- 除非使用 `readonly` 修饰符声明 `struct`或方法仅调用该结构的 `readonly` 成员，否则切勿将其作为 `in` 参数传递。 不遵守该指南可能会对性能产生负面影响，并可能导致不明确的行为。
- 使用 [`ref struct`](language-reference/builtin-types/struct.md#ref-struct) 或 `readonly ref struct`（例如 <xref:System.Span%601> 或 <xref:System.ReadOnlySpan%601>）将内存用作字节序列。

这些技术迫使你在“引用”和“值”方面平衡两个相互竞争的目标   。 属于[引用类型](programming-guide/types/index.md#reference-types)的变量包含对内存中位置的引用。 属于[值类型](programming-guide/types/index.md#value-types)的变量直接包含它们的值。 这些差异突出了对管理内存资源非常重要的关键差异。 通常在将“值类型”传递给方法或从方法返回时将其复制  。 此行为包括在调用值类型的成员时复制 `this` 的值。 副本的成本与类型的大小有关。 托管堆上分配了“引用类型”  。 每个新对象都需要一个新的分配，并且随后必须回收。 这两种操作都需要花些时间。 将引用类型作为参数传递给方法或从方法返回时，将复制引用。

本文使用以下三维点结构的示例概念来解释这些建议：

```csharp
public struct Point3D
{
    public double X;
    public double Y;
    public double Z;
}
```

不同的示例使用该概念的不同实现。

## <a name="declare-readonly-structs-for-immutable-value-types"></a>声明不可变值类型的只读结构

使用 `readonly` 修饰符声明 `struct` 将通知编译器你的意图是创建不可变类型。 编译器使用以下规则强制执行该设计决策：

- 所有字段成员必须为 `readonly`
- 所有属性都必须是只读的，包括自动实现的属性。

这两个规则足以确保 `readonly struct` 的任何成员都不会修改该结构的状态。 `struct` 是不可变的。 `Point3D` 结构可以定义为不可变结构，如以下示例所示：

```csharp
readonly public struct ReadonlyPoint3D
{
    public ReadonlyPoint3D(double x, double y, double z)
    {
        this.X = x;
        this.Y = y;
        this.Z = z;
    }

    public double X { get; }
    public double Y { get; }
    public double Z { get; }
}
```

只要你的设计意图是创建不可变值类型，就请遵循此建议。 任何性能改进都是额外权益。 `readonly struct` 清楚地表达了你的设计意图。

## <a name="declare-readonly-members-when-a-struct-cant-be-immutable"></a>结构可变时声明 readonly 成员

在 C# 8.0 及更高版本中，结构类型为可变类型时，应将不会引起变化的成员声明为 `readonly`。 请考虑其他需要三维点结构的应用程序，但必须支持可变性。 以下版本的三维点结构仅将 `readonly` 修饰符添加到不修改结构的成员。 当你的设计必须支持某些成员对结构的修改时，但仍然需要对某些成员强制执行只读操作的便利时，请遵循以下示例：

```csharp
public struct Point3D
{
    public Point3D(double x, double y, double z)
    {
        _x = x;
        _y = y;
        _z = z;
    }

    private double _x;
    public double X
    {
        readonly get => _x;
        set => _x = value;
    }

    private double _y;
    public double Y
    {
        readonly get => _y;
        set => _y = value;
    }

    private double _z;
    public double Z
    {
        readonly get => _z;
        set => _z = value;
    }

    public readonly double Distance => Math.Sqrt(X * X + Y * Y + Z * Z);

    public readonly override string ToString() => $"{X}, {Y}, {Z}";
}
```

前面的示例介绍了可在其中应用 `readonly` 修饰符的许多位置：方法、属性和属性访问器。 如果使用自动实现的属性，则编译器会将 `readonly` 修饰符添加到 `get` 访问器以获取读写属性。 对于仅具有 `get` 访问器的属性，编译器会将 `readonly` 修饰符添加到自动实现的属性声明中。

向不改变状态的成员添加 `readonly` 修饰符有两个相关的好处。 首先，编译器会强制执行你的意图。 该成员无法改变结构的状态。 其次，访问 `readonly` 成员时，编译器不会创建 `in` 参数的防御性副本。 编译器可以安全地进行此优化，因为它可以保证 `readonly` 成员不会修改 `struct`。

## <a name="use-ref-readonly-return-statements-for-large-structures-when-possible"></a>尽可能对大型结构使用 `ref readonly return` 语句

当返回的值不是返回方法的本地值时，可以按引用返回值。 按引用返回意味着仅复制引用，而不是结构。 在以下示例中，`Origin` 属性不能使用 `ref` 返回，因为返回的值是局部变量：

```csharp
public Point3D Origin => new Point3D(0,0,0);
```

但是，可以按引用返回以下属性定义，因为返回的值是静态成员：

```csharp
public struct Point3D
{
    private static Point3D origin = new Point3D(0,0,0);

    // Dangerous! returning a mutable reference to internal storage
    public ref Point3D Origin => ref origin;

    // other members removed for space
}
```

你不希望调用方修改原点，所以应该通过 `ref readonly` 返回值：

```csharp
public struct Point3D
{
    private static Point3D origin = new Point3D(0,0,0);

    public static ref readonly Point3D Origin => ref origin;

    // other members removed for space
}
```

通过返回 `ref readonly` 可以保存复制较大的结构并保留内部数据成员的不变性。

在调用站点，调用方可以选择将 `Origin` 属性用作 `ref readonly` 或值：

[!code-csharp[AssignRefReadonly](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#AssignRefReadonly "Assigning a ref readonly")]

前面的代码中的第一个分配将创建 `Origin` 常数的副本，并分配该副本。 第二个将分配引用。 注意，`readonly` 修饰符必须包含在变量声明中。 无法修改该修饰符引用对象的引用。 尝试执行该操作将导致编译时错误。

`originReference` 的声明需要 `readonly` 修饰符。

编译器可强制使调用方不能修改引用。 直接分配给该值的尝试会生成编译时错误。 在其他情况下，编译器会分配防御性副本，除非它可安全地使用只读引用。 静态分析规则会确定是否可修改结构。 当结构为 `readonly struct`，或者成员为结构的 `readonly` 成员时，编译器不会创建防御性副本。 无需使用防御性副本即可将结构作为 `in` 参数进行传递。

## <a name="apply-the-in-modifier-to-readonly-struct-parameters-larger-than-systemintptrsize"></a>将 `in` 修饰符应用于大于 `System.IntPtr.Size` 的 `readonly struct` 参数

`in` 关键字补充了现有的 `ref` 和 `out` 关键字，以按引用传递参数。 `in` 关键字指定按引用传递参数，但调用的方法不修改值。

这一新增功能可提供完整的词汇，以表达你的设计意图。
如果未在方法签名中指定以下任一修饰符，值类型会在传递给调用的方法时进行复制。 每个修饰符指定变量按引用传递，避免复制操作。 每个修饰符表达一种不同的意图：

- `out`：此方法设置用作此形参的实参的值。
- `ref`：此方法可设置用作此形参的实参的值。
- `in`：此方法不会修改用作此形参的实参的值。

添加 `in` 修饰符，按引用传递参数，并声明设计意图是为了按引用传递参数，避免不必要的复制操作。 你不打算修改用作该参数的对象。

这种做法通常可以提高大于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 的只读值类型的性能。 对于简单类型（`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`char`、`float`、`double`、`decimal` 和 `bool` 以及 `enum` 类型），任何潜在的性能提升都是极小的。 实际上，对于小于 <xref:System.IntPtr.Size?displayProperty=nameWithType> 的类型，使用按引用传递可能会降低性能。

下面的代码演示了一个方法示例，该方法用于计算三维空间中两点间的距离。

[!code-csharp[InArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgument "Specifying an in argument")]

该参数具有两个结构，每个结构包含三个双精度值。 一个双精度值有 8 个字节，所以每个参数有 24 个字节。 通过指定 `in` 修饰符，可向这些参数传递 4 字节或 8 字节引用，具体取决于计算机的体系结构。 大小的差异很小，但是当应用程序使用许多不同的值在一个紧凑的循环中调用此方法时，这些差异将累积。

`in` 修饰符还可以通过其他方式补充 `out` 和 `ref`。 无法创建差异仅为是否具有 `in`、`out` 或 `ref` 的方法的重载。 这些新规则可扩展始终为 `out` 和 `ref` 参数定义的相同行为。 与 `out` 和 `ref` 修饰符类似，值类型未装箱，因为应用了 `in` 修饰符。

`in` 修饰符可能适用于采用以下参数的任何成员：methods、delegates、lambdas、local functions、indexers 和 operators。

`in` 实参的另一个功能是可对 `in` 形参的实参使用文本值或常数。 此外，与 `ref` 或 `out` 参数不同，无需在调用站点应用 `in` 修饰符。 下面的代码演示调用 `CalculateDistance` 方法的两个示例。 第一个示例使用按引用传递的两个本地变量。 第二个示例包含方法调用过程中创建的临时变量。

[!code-csharp[UseInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#UseInArgument "Specifying an In argument")]

有多种方法可让编译器确保强制执行 `in` 参数的只读性质。  首先，调用的方法不能直接分配给 `in` 参数。 当值类型为 `struct` 时，不能直接分配给 `in` 参数的任何字段。 此外，不能向使用 `ref` 或 `out` 修饰符的任何方法传递 `in` 参数。
如果字段为 `struct` 类型且该参数也为 `struct` 类型，则这些规则适用于 `in` 参数的任何字段。 事实上，如果所有级别的成员访问类型都是 `structs`，则这些规则适用于多层成员访问。
编译器强制将 `struct` 类型作为 `in` 参数传递，它们的 `struct` 成员用作其他方法的参数时，为只读变量。

使用 `in` 参数可避免产生复制操作可能产生的性能成本。 这不会改变任何方法调用的语义。 所以不需要在调用站点指定 `in` 修饰符。 在调用站点省略 `in` 修饰符就会通知编译器你允许它出于以下原因复制参数：

- 存在从实参类型到形参类型的隐式转换，但不是标识转换。
- 该参数是一个表达式，但是没有已知的存储变量。
- 存在的重载因 `in` 是否存在而有所不同。 在这种情况下，按值重载的匹配度会更高。

在更新现有代码以使用只读引用参数时，这些规则会很有用。 在调用的方法中，可以调用任何使用按值参数的实例方法。 在这些方法中，将创建 `in` 参数的副本。 由于编译器可为任何 `in` 参数创建临时变量，因此还可指定任何 `in` 参数的默认值。 以下代码指定原点（点 0,0）为第二个点的默认值：

[!code-csharp[InArgumentDefault](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgumentDefault "Specifying defaults for an in parameter")]

要强制编译器按引用传递只读参数，请在调用站点的参数上指定 `in` 修饰符，如下列代码所示：

[!code-csharp[UseInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#ExplicitInArgument "Specifying an In argument")]

这样可以更轻松地在大型代码库中采用一段时间的 `in` 参数，从而实现性能提升。 首先，将 `in` 修饰符添加到方法签名。 然后，可以在调用站点添加 `in` 修饰符，并创建 `readonly struct` 类型，让编译器避免在更多位置创建 `in` 参数的防御性副本。

`in` 参数指定还可用于引用类型或数值。 但是，这两种情况下获得的好处都是最少的（如果有）。

## <a name="avoid-mutable-structs-as-an-in-argument"></a>避免在 `in` 参数中使用可变结构

上述技术解释了如何通过返回引用和按引用传递值来避免创建副本。 当参数类型声明为 `readonly struct` 类型时，这些技术最有效。 否则，编译器必须在许多情况下创建“防御副本”以强制执行任何参数的只读状态  。 请考虑下面这个计算三维点到原点距离的示例：

[!code-csharp[InArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#InArgument "Specifying an in argument")]

`Point3D` 结构不是只读结构  。 此方法的主体中有六个不同的属性访问调用。 在首次检查时，你可能认为这些访问是安全的。 毕竟，`get` 访问器不应该修改对象的状态。 但是没有强制执行的语言规则。 它只是通用约定。 任何类型都可以实现修改内部状态的 `get` 访问器。 如果没有语言保证，编译器必须在调用任何未标记为 `readonly` 修饰符的成员之前创建参数的临时副本。 在堆栈上创建临时存储，将参数的值复制到临时存储中，并将每个成员访问的值作为 `this` 参数复制到堆栈中。 在许多情况下，当参数类型不是 `readonly struct`，并且该方法调用成员未标记为 `readonly` 时，这些副本会降低性能，使得按值传递比按只读引用传递速度更快。 如果将不修改结构状态的所有方法标记为 `readonly`，编译器就可以安全地确定不修改结构状态，并且不需要防御性复制。

相反，如果距离计算使用不可变结构 `ReadonlyPoint3D`，则不需要临时对象：

[!code-csharp[readonlyInArgument](../../samples/snippets/csharp/safe-efficient-code/ref-readonly-struct/Program.cs#ReadOnlyInArgument "Specifying a readonly in argument")]

当你调用 `readonly struct` 的成员时，编译器会生成更有效的代码：`this` 引用始终是按引用成员方法传递的 `in` 参数，而不是接收器的副本。 将 `readonly struct` 用作 `in` 参数时，此优化可以减少复制操作。

不应将可为 null 的值类型作为 `in` 参数传递。 <xref:System.Nullable%601> 类型未声明为只读结构。 这意味着编译器必须为使用参数声明中的 `in` 修饰符传递到方法的任何可以为 null 的值类型参数生成防御性副本。

你可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/tree/main/csharp/safe-efficient-code/benchmark)中看到使用 [BenchmarkDotNet](https://www.nuget.org/packages/BenchmarkDotNet/) 演示性能差异的示例程序。 它对按值和按引用传递可变结构与按值和按引用传递不可变结构进行了比较。 使用不可变结构并按引用传递是最快的。

## <a name="use-ref-struct-types-to-work-with-blocks-or-memory-on-a-single-stack-frame"></a>使用 `ref struct` 类型处理单个堆栈帧上的块或内存

相关语言功能是可声明必须约束为单个堆栈帧的值类型。 此限制可使编译器进行多次优化。 此功能的主要动机是 <xref:System.Span%601> 和相关结构。 借助使用 <xref:System.Span%601> 类型的新的和更新后的 .NET API，可通过这些增强功能实现性能改进。

在使用通过 [`stackalloc`](language-reference/operators/stackalloc.md) 创建的内存或使用互操作 API 中的内存时，可能具有类似要求。 可针对这些需求定义自己的 `ref struct` 类型。

## <a name="readonly-ref-struct-type"></a>`readonly ref struct` 类型

将结构声明为 `readonly ref` 兼具 `ref struct` 和 `readonly struct` 声明的优点和限制。 只读跨度所使用的内存仅限于单个堆栈帧，并且只读跨度使用的内存无法进行修改。

## <a name="conclusions"></a>结论

使用值类型可最大限度地减少分配操作的数量：

- 值类型的存储是为局部变量和方法参数分配的堆栈。
- 作为其他对象成员的值类型的存储被分配为该对象的一部分，而不是作为单独的分配。
- 值类型返回值的存储为堆栈分配。

将其与相同情况下的引用类型进行对比：

- 引用类型的存储是为本地变量和方法参数分配的堆。 引用存储在堆栈中。
- 作为其他对象成员的引用类型的存储在堆上分别进行分配。 包含的对象存储引用。
- 引用类型返回值的存储是堆分配的。 对该存储的引用存储在堆栈中。

最小化分配附带权衡。 当 `struct` 的大小大于引用大小时，可以复制更多内存。 引用通常为 64 位或 32 位，并且取决于目标机器 CPU。

这些权衡通常对性能影响最小。 但是，对于大型结构或大型集合，性能影响会增加。 对于程序的紧密循环和热路径，影响可能很大。

C# 语言的这些增强功能专为性能关键型算法而设计，在这些算法中，使内存分配最小化是实现必需性能的主要因素。 你可能会发现，编写代码时不经常使用这些功能。 但是，整个 .NET 中都已采用这些增强功能。 随着越来越多的 API 使用这些功能，你会发现应用程序的性能得到提升。

## <a name="see-also"></a>请参阅

- [ref 关键字](language-reference/keywords/ref.md)
- [ref 返回结果和局部变量](programming-guide/classes-and-structs/ref-returns.md)
