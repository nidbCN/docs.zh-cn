---
title: C# 发展历史 - C# 指南
description: 这些语言在最早版本中是什么样的，它又是如何演化的？
author: erikdietrich
ms.date: 04/08/2020
ms.openlocfilehash: 0c427563701bd3bff87178022feab19f4dcfc519
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877054"
---
# <a name="the-history-of-c"></a>C\# 发展历史

本页介绍了 C# 语言每个主要版本的发展历史。 C# 团队将继续创新，以添加新功能。 可以在 GitHub 上的 [dotnet/roslyn 存储库](https://github.com/dotnet/roslyn/blob/main/docs/Language%20Feature%20Status.md)上找到详细的语言功能状态，包括考虑在即将发布的版本中添加的功能。

> [!IMPORTANT]
> 为了提供一些功能，C# 语言依赖 C# 规范定义为标准库  所用的类型和方法。 .NET 平台通过许多包交付这些类型和方法。 例如，异常处理。 为了确保引发的对象派生自 <xref:System.Exception>，将会检查每个 `throw` 语句或表达式。 同样，还会检查每个 `catch`，以确保捕获的类型派生自 <xref:System.Exception>。 每个版本都可能会新增要求。 若要在旧版环境中使用最新语言功能，可能需要安装特定库。 每个特定版本的页面中记录了这些依赖项。 若要了解此依赖项的背景信息，可以详细了解[语言与库的关系](relationships-between-language-and-library.md)。

C# 生成工具将最新的主要语言版本视为默认语言版本。 主要版本之间可能有单点修正发行版。有关详细信息，请参阅本节中的其他文章。 若要使用单点版本中的最新功能，需要[配置编译器语言版本](../language-reference/configure-language-version.md)并选择版本。 自 C# 7.0 起，已有三个单点修正发行版：

- C# 7.3：
  - 自 [Visual Studio 2017 版本 15.7](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 和 [.NET Core 2.1 SDK](../../core/whats-new/dotnet-core-2-1.md) 起，开始随附 C# 7.3。
- C# 7.2：
  - 自 [Visual Studio 2017 版本 15.5](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 和 [.NET Core 2.0 SDK](../../core/whats-new/dotnet-core-2-0.md) 起，开始随附 C# 7.2。
- C# 7.1：
  - 自 [Visual Studio 2017 版本 15.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link) 和 [.NET Core 2.0 SDK](../../core/whats-new/dotnet-core-2-0.md) 起，开始随附 C# 7.1。

## <a name="c-version-10"></a>C# 1.0 版

回想起来，和 Visual Studio .NET 2002 一起发布的 C# 版本 1.0 非常像 Java。 在 [ECMA 制定的设计目标](https://feeldotneteasy.blogspot.com/2011/01/c-design-goals.html)中，它旨在成为一种“简单、现代、面向对象的常规用途语言”。  当时，它和 Java 类似，说明已经实现了上述早期设计目标。

不过如果现在回顾 C# 1.0，你会觉得有点晕。 它没有习以为常的内置异步功能和以泛型为中心的巧妙功能。 其实它完全不具备泛型。  那 [LINQ](../linq/index.md) 呢？ 尚不可用。 这些新增内容需要几年才能推出。

与现在的 C# 相比，C# 1.0 版少了很多功能。 你会发现自己的代码很冗长。 不过凡事总要有个开始。 在 Windows 平台上，C# 1.0 版是 Java 的一个可行的替代之选。

C# 1.0 的主要功能包括：

- [类](../programming-guide/classes-and-structs/classes.md)
- [结构](../language-reference/builtin-types/struct.md)
- [接口](../programming-guide/interfaces/index.md)
- [事件](../events-overview.md)
- [属性](../properties.md)
- [委托](../delegates-overview.md)
- [运算符和表达式](../language-reference/operators/index.md)
- [语句](../programming-guide/statements-expressions-operators/statements.md)
- [特性](../programming-guide/concepts/attributes/index.md)

## <a name="c-version-12"></a>C# 版本 1.2

随 Visual Studio .NET 2003 一起提供的 C# 版本 1.2。 它对语言做了一些小改进。 最值得注意的是，从此版本开始，当 <xref:System.Collections.IEnumerator> 实现 <xref:System.IDisposable> 时，`foreach` 循环中生成的代码会在 <xref:System.Collections.IEnumerator> 上调用 <xref:System.IDisposable.Dispose%2A>。

## <a name="c-version-20"></a>C# 2.0 版

从此以后事情变得有趣起来。 让我们看看 C# 2.0（2005 年发布）和 Visual Studio 2005 中的一些主要功能：

- [泛型](../programming-guide/generics/index.md)
- [分部类型](../programming-guide/classes-and-structs/partial-classes-and-methods.md#partial-classes)
- [匿名方法](../language-reference/operators/delegate-operator.md)
- [可以为 null 的值类型](../language-reference/builtin-types/nullable-value-types.md)
- [迭代器](../programming-guide/concepts/iterators.md)
- [协变和逆变](../programming-guide/concepts/covariance-contravariance/index.md)

除现有功能以外的其他 C# 2.0 功能：

- getter/setter 单独可访问性
- 方法组转换（委托）
- 静态类
- 委托推断

C# 一开始是面向对象的 (OO) 通用语言，而 C# 2.0 版很快改变了这一点。 做好基础准备后，他们开始追求解决一些严重影响开发者的难点。 且他们以显著的方式追求这些难点。

通过泛型，类型和方法可以操作任意类型，同时保持类型安全性。 例如，通过 <xref:System.Collections.Generic.List%601>，将获得 `List<string>` 或 `List<int>` 并且可以对这些字符串或整数执行类型安全操作，同时对其进行循环访问。 使用泛型优于创建派生自 `ArrayList` 的 `ListInt` 类型，也优于从每个操作的 `Object` 强制转换。

C# 2.0 版引入了迭代器。 简单来说，迭代器允许使用 `foreach` 循环来检查 `List`（或其他可枚举类型）中的所有项。 拥有迭代器是该语言最重要的一部分，显著提升了语言的可读性以及人们推出代码的能力。

不过 C# 依然在追赶 Java 的道路上。 当时 Java 已发布包含泛型和迭代器的版本。 但是随着语言各自的演化，形势很快发生了变化。

## <a name="c-version-30"></a>C# 3.0 版

C# 3.0 版和 Visual Studio 2008 一起发布于 2007 年下半年，但完整的语言功能是在 .NET Framework 3.5 版中发布的。 此版本标示着 C# 发展过程中的重大更改。 C# 成为了真正强大的编程语言。 我们来看看此版本中的一些主要功能：

- [自动实现的属性](../programming-guide/classes-and-structs/auto-implemented-properties.md)
- [匿名类型](../programming-guide/classes-and-structs/anonymous-types.md)
- [查询表达式](../linq/query-expression-basics.md)
- [Lambda 表达式](../language-reference/operators/lambda-expressions.md)
- [表达式树](../expression-trees.md)
- [扩展方法](../programming-guide/classes-and-structs/extension-methods.md)
- [隐式类型本地变量](../language-reference/keywords/var.md)
- [分部方法](../language-reference/keywords/partial-method.md)
- [对象和集合初始值设定项](../programming-guide/classes-and-structs/object-and-collection-initializers.md)

回顾过去，这些功能中大多数似乎都是不可或缺，难以分割的。 它们的组合都是经过巧妙布局。 我们通常认为 C# 版本的杀手锏是查询表达式，也就是语言集成查询 (LINQ)。

LINQ 的构造可以建立在更细微的视图检查表达式树、Lambda 表达式以及匿名类型的基础上。 不过无论如何 C# 3.0 都提出了革命性的概念。 C# 3.0 开始为 C# 转变为面向对象/函数式混合语言打下基础。

具体来说，你现在可以编写 SQL 样式的声明性查询对集合以及其他项目执行操作。 无需再编写 `for` 循环来计算整数列表的平均值，现在可改用简单的 `list.Average()` 方法。 组合使用查询表达式和扩展方法让各种数字变得智能多了。

人们需要一些时间来掌握和吸收这种概念，不过已经逐渐做到了。 现在又过了几年，代码变得更简洁，功能也更强大了。

## <a name="c-version-40"></a>C# 4.0 版

C# 版本 4.0 随 Visual Studio 2010 一起发布，很难达到版本 3.0 的创新水平。 在 3.0 版中，C# 已经完全从 Java 的阴影中脱颖而出，崭露头角。 很快成为一种简洁精炼的语言。

下一版本引入了一些有趣的新功能：

- [动态绑定](../language-reference/builtin-types/reference-types.md)
- [命名参数/可选参数](../programming-guide/classes-and-structs/named-and-optional-arguments.md)
- [泛型协变和逆变](../../standard/generics/covariance-and-contravariance.md)
- [嵌入的互操作类型](../../framework/interop/type-equivalence-and-embedded-interop-types.md)

嵌入的互操作类型缓和了部署难点。 泛型协变和逆变提供了更强的功能来使用泛型，但风格比较偏学术，应该最受框架和库创建者的喜爱。 命名参数和可选参数帮助消除了很多方法重载，让使用更方便。 但是这些功能都没有完全改变模式。

主要功能是引入 `dynamic` 关键字。 在 C# 4.0 版中引入 `dynamic` 关键字让用户可以替代编译时类型上的编译器。 通过使用 dynamic 关键字，可以创建和动态类型语言（例如 JavaScript）类似的构造。 可以创建 `dynamic x = "a string"` 再向它添加六个，然后让运行时理清下一步操作。

动态绑定存在出错的可能性，不过同时也为你提供了强大的语言功能。

## <a name="c-version-50"></a>C# 5.0 版

C# 版本 5.0 随 Visual Studio 2012 一起发布，是该语言有针对性的一个版本。 对此版本中所做的几乎所有工作都归入另一个突破性语言概念：适用于异步编程的 `async` 和 `await` 模型。  下面是主要功能列表：

- [异步成员](../async.md)
- [调用方信息特性](../language-reference/attributes/caller-information.md)

### <a name="see-also"></a>另请参阅

- [代码工程：C# 5.0 中的调用方信息属性](https://www.codeproject.com/Tips/606379/Caller-Info-Attributes-in-Csharp)

调用方信息特性让你可以轻松检索上下文的信息，不需要采用大量样本反射代码。 这在诊断和日志记录任务中也很有用。

但是 `async` 和 `await` 才是此版本真正的主角。 C# 在 2012 年推出这些功能时，将异步引入语言作为最重要的组成部分，另现状大为改观。 如果你以前处理过冗长的运行操作以及实现回调的 Web，应该会爱上这项语言功能。

## <a name="c-version-60"></a>C# 6.0 版

C# 在 3.0 版和 5.0 版对面向对象的语言添加了主要的新功能。 版本 6.0 随 Visual Studio 2015 一起发布，通过该版本，它不再推出主导性的杀手锏，而是发布了很多使得 C# 编程更有效率的小功能。 以下介绍了部分功能：

- [静态导入](../language-reference/keywords/using-static.md)
- [异常筛选器](../language-reference/keywords/when.md)
- [自动属性初始化表达式](../properties.md)
- [Expression bodied 成员](../language-reference/operators/lambda-operator.md#expression-body-definition)
- [Null 传播器](../language-reference/operators/member-access-operators.md#null-conditional-operators--and-)
- [字符串内插](../language-reference/tokens/interpolated.md)
- [nameof 运算符](../language-reference/operators/nameof.md)

其他新功能包括：

- 索引初始化表达式
- Catch/Finally 块中的 Await
- 仅限 getter 属性的默认值

这些功能每一个都很有趣。 但从整体来看，可以发现一个有趣的模式。 在此版本中，C# 消除语言样本，让代码更简洁且更具可读性。 所以对喜欢简洁代码的用户来说，此语言版本非常成功。

除了发布此版本，他们还做了另一件事，虽然这件事本身与传统的语言功能无关。 他们发布了 [Roslyn 编译器即服务](https://github.com/dotnet/roslyn)。 C# 编译器现在是用 C# 编写的，你可以使用编译器作为编程工作的一部分。

## <a name="c-version-70"></a>C# 7.0 版

C# 7.0 版已与 Visual Studio 2017 一起发布。 虽然该版本继承和发展了 C# 6.0，但不包含编译器即服务。 以下介绍了部分新增功能：

- [Out 变量](./csharp-7.md#out-variables)
- [元组和析构函数](./csharp-7.md#tuples-and-discards)
- [模式匹配](./csharp-7.md#pattern-matching)
- [本地函数](./csharp-7.md#local-functions)
- [已扩展 expression bodied 成员](./csharp-7.md#more-expression-bodied-members)
- [Ref 局部变量和返回结果](./csharp-7.md#ref-locals-and-returns)

其他功能包括：

- [弃元](./csharp-7.md#tuples-and-discards)
- [二进制文本和数字分隔符](./csharp-7.md#numeric-literal-syntax-improvements)
- [引发表达式](./csharp-7.md#throw-expressions)

这些都为开发者提供了很棒的新功能，帮助编写比以往任何时候都简洁的代码。 重点是缩减了使用 `out` 关键字的变量声明，并通过元组实现了多个返回值。

但 C# 的用途更加广泛了。 .NET Core 现在面向所有操作系统，着眼于云和可移植性。  语言设计者除了推出新功能外，也会在这些新功能方面付出时间和精力。

## <a name="c-version-71"></a>C# 7.1 版

C# 已开始随 C# 7.1 发布单点发行  。 此版本增加了[语言版本选择](../language-reference/configure-language-version.md)配置元素、三个新的语言功能和新的编译器行为。

此版本中新增的语言功能包括：

- [`async` `Main` 方法](./csharp-7.md#async-main)
  - 应用程序的入口点可以含有 `async` 修饰符。
- [`default` 文本表达式](./csharp-7.md#default-literal-expressions)
  - 在可以推断目标类型的情况下，可在默认值表达式中使用默认文本表达式。
- [推断元组元素名称](./csharp-7.md#tuples-and-discards)
  - 在许多情况下，可通过元组初始化来推断元组元素的名称。
- [泛型类型参数的模式匹配](./csharp-7.md#pattern-matching)
  - 可以对类型为泛型类型参数的变量使用模式匹配表达式。

最后，编译器有 `-refout` 和 `-refonly` 两个选项，可用于控制[引用程序集生成](./csharp-7.md#reference-assembly-generation)。

## <a name="c-version-72"></a>C# 7.2 版

C# 7.2 版添加了几个小型语言功能：

- [编写安全高效代码的技巧](./csharp-7.md#enabling-more-efficient-safe-code)
  - 结合了多项语法改进，可使用引用语义处理值类型。
- [非尾随命名参数](./csharp-7.md#non-trailing-named-arguments)
  - 命名的参数可后接位置参数。
- [数值文字中的前导下划线](./csharp-7.md#numeric-literal-syntax-improvements)
  - 数值文字现可在任何打印数字前放置前导下划线。
- [`private protected` 访问修饰符](./csharp-7.md#private-protected-access-modifier)
  - `private protected` 访问修饰符允许访问同一程序集中的派生类。
- [条件 `ref` 表达式](./csharp-7.md#conditional-ref-expressions)
  - 现在可以引用条件表达式 (`?:`) 的结果。

## <a name="c-version-73"></a>C# 7.3 版

C# 7.3 版本有两个主要主题。 第一个主题提供使安全代码的性能与不安全代码的性能一样好的功能。 第二个主题提供对现有功能的增量改进。 此外，在此版本中添加了新的编译器选项。

以下新增功能支持使安全代码获得更好的性能的主题：

- [无需固定即可访问固定的字段。](csharp-7.md#indexing-fixed-fields-does-not-require-pinning)
- [可以重新分配 `ref` 本地变量。](csharp-7.md#enabling-more-efficient-safe-code)
- [可以使用 `stackalloc` 数组上的初始值设定项。](csharp-7.md#stackalloc-arrays-support-initializers)
- [可以对支持模式的任何类型使用 `fixed` 语句。](csharp-7.md#more-types-support-the-fixed-statement)
- [可以使用其他泛型约束。](csharp-7.md#enhanced-generic-constraints)

对现有功能进行了以下增强：

- 可以使用元组类型测试 `==` 和 `!=`。
- 可以在多个位置使用表达式变量。
- 可以将属性附加到自动实现的属性的支持字段。
- 由 `in` 区分的参数的方法解析得到了改进。
- 重载解析的多义情况现在变得更少。

新的编译器选项为：

- `-publicsign`，用于启用程序集的开放源代码软件 (OSS) 签名。
- `-pathmap`用于提供源目录的映射。

## <a name="c-version-80"></a>C# 8.0 版

C# 8.0 版是专门面向 .NET C# Core 的第一个主要 C# 版本。 一些功能依赖于新的 CLR 功能，而其他功能依赖于仅在 .NET Core 中添加的库类型。 C# 8.0 向 C# 语言添加了以下功能和增强功能：

- [Readonly 成员](./csharp-8.md#readonly-members)
- [默认接口方法](./csharp-8.md#default-interface-methods)
- [模式匹配增强功能](./csharp-8.md#more-patterns-in-more-places)：
  - [Switch 表达式](./csharp-8.md#switch-expressions)
  - [属性模式](./csharp-8.md#property-patterns)
  - [元组模式](./csharp-8.md#tuple-patterns)
  - [位置模式](./csharp-8.md#positional-patterns)
- [Using 声明](./csharp-8.md#using-declarations)
- [静态本地函数](./csharp-8.md#static-local-functions)
- [可处置的 ref 结构](./csharp-8.md#disposable-ref-structs)
- [可为空引用类型](../language-reference/builtin-types/nullable-reference-types.md)
- [异步流](./csharp-8.md#asynchronous-streams)
- [索引和范围](./csharp-8.md#indices-and-ranges)
- [Null 合并赋值](./csharp-8.md#null-coalescing-assignment)
- [非托管构造类型](./csharp-8.md#unmanaged-constructed-types)
- [嵌套表达式中的 Stackalloc](./csharp-8.md#stackalloc-in-nested-expressions)
- [内插逐字字符串的增强功能](./csharp-8.md#enhancement-of-interpolated-verbatim-strings)

默认接口成员需要 CLR 中的增强功能。 这些功能已添加到 .NET Core 3.0 的 CLR 中。 范围和索引以及异步流需要 .NET Core 3.0 库中的新类型。 在编译器中实现时，可为 null 的引用类型在批注库时更有用，因为它可以提供有关参数和返回值的 null 状态的语义信息。 这些批注将添加到 .NET Core 库中。

_文章_[_最初发布在 NDepend 博客上_](https://blog.ndepend.com/c-versions-look-language-history/) _，由 Erik Dietrich 和 Patrick Smacchia 提供_。
