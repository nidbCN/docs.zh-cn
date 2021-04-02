---
title: C# 介绍 - C# 指南
description: 刚开始接触 C#？ 了解 C# 语言的基础知识。 从此概述开始。
ms.date: 01/28/2021
ms.openlocfilehash: b58aed5e75be8c71df295506480a2d97c22675aa
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102104908"
---
# <a name="a-tour-of-the-c-language"></a>C# 语言介绍

C#（读作“See Sharp”）是一种新式编程语言，不仅面向对象，还类型安全。 开发人员利用 C# 能够生成在 .NET 生态系统中运行的多种安全可靠的应用程序。 C# 源于 C 语言系列，C、C++、Java 和 JavaScript 程序员很快就可以上手使用。 本教程概述了 C# 8 及更高版本中该语言的主要组件。 如果想要通过交互式示例探索语言，请尝试 [C# 简介](./tutorials/index.md)教程。

C# 是面向对象的、面向组件的编程语言。 C# 提供了语言构造来直接支持这些概念，让 C# 成为一种非常自然的语言，可用于创建和使用软件组件。 自诞生之日起，C# 就添加了支持新工作负载和新兴软件设计实践的功能。

多项 C# 功能有助于创建可靠且持久的应用程序。 [垃圾回收](../../standard/garbage-collection/index.md)*_自动回收不可访问的未用对象所占用的内存。 [可以为 null 的类型](../nullable-references.md)可防范不引用已分配对象的变量。 [异常处理](../programming-guide/exceptions/index.md)提供了一种结构化且可扩展的方法来进行错误检测和恢复。 [Lambda 表达式](../language-reference/operators/lambda-expressions.md)支持函数编程技术。 [语言集成查询 (LINQ)](../linq/index.md) 语法创建一个公共模式，用于处理来自任何源的数据。 [异步操作](../programming-guide/concepts/async/index.md)语言支持提供用于构建分布式系统的语法。 C# 有[统一类型系统](../programming-guide/types/index.md)_*。 所有 C# 类型（包括 `int` 和 `double` 等基元类型）均继承自一个根 `object` 类型。 所有类型共用一组通用运算。 任何类型的值都可以一致地进行存储、传输和处理。 此外，C# 还支持用户定义的[引用类型](../language-reference/builtin-types/reference-types.md)和[值类型](../language-reference/builtin-types/value-types.md)。 C# 允许动态分配轻型结构的对象和内嵌存储。 C# 支持泛型方法和类型，因此增强了类型安全性和性能。 C# 可提供迭代器，使集合类的实现者可以定义客户端代码的自定义行为。

C# 强调版本控制，以确保程序和库以兼容方式随时间推移而变化。 C# 设计中受版本控制加强直接影响的方面包括：单独的 `virtual` 和 `override` 修饰符，关于方法重载决策的规则，以及对显式接口成员声明的支持。

## <a name="net-architecture"></a>.NET 体系结构

C# 程序在 .NET 上运行，而 .NET 是名为公共语言运行时 (CLR) 的虚执行系统和一组类库。 CLR 是 Microsoft 对公共语言基础结构 (CLI) 国际标准的实现。 CLI 是创建执行和开发环境的基础，语言和库可以在其中无缝地协同工作。

用 C# 编写的源代码被编译成符合 CLI 规范的[中间语言 (IL)](../../standard/managed-code.md)。 IL 代码和资源（如位图和字符串）存储在扩展名通常为 .dll 的程序集中。 程序集包含一个介绍程序集的类型、版本和区域性的清单。

执行 C# 程序时，程序集将加载到 CLR。 CLR 会直接执行实时 (JIT) 编译，将 IL 代码转换成本机指令。 CLR 可提供其他与自动垃圾回收、异常处理和资源管理相关的服务。 CLR 执行的代码有时称为“托管代码”（而不是“非托管代码”），被编译成面向特定平台的本机语言。

语言互操作性是 .NET 的一项重要功能。 C# 编译器生成的 IL 代码符合公共类型规范 (CTS)。 通过 C# 生成的 IL 代码可以与通过 .NET 版本的 F#、Visual Basic、C++ 或其他 20 多种与 CTS 兼容的任何语言所生成的代码进行交互。 一个程序集可能包含多个用不同 .NET 语言编写的模块，且类型可以相互引用，就像是用同一种语言编写的一样。

除了运行时服务之外，.NET 还包含大量库。 这些库支持多种不同的工作负载。 它们已整理到命名空间中，这些命名空间提供各种实用功能，包括文件输入输出、字符串控制、XML 分析、Web 应用程序框架和 Windows 窗体控件。 典型的 C# 应用程序广泛使用 .NET 类库来处理常见的“管道”零碎工作。

有关 .NET 的详细信息，请参阅 [.NET 概述](../../core/introduction.md)。

## <a name="hello-world"></a>Hello world

“Hello, World”程序历来都用于介绍编程语言。 下面展示了此程序的 C# 代码：

:::code language="csharp" interactive="try-dotnet" source="./snippets/shared/HelloWorld.cs":::

“Hello, World”程序始于引用 `System` 命名空间的 `using` 指令。 命名空间提供了一种用于组织 C# 程序和库的分层方法。 命名空间包含类型和其他命名空间。例如，`System` 命名空间包含许多类型（如程序中引用的 `Console` 类）和其他许多命名空间（如 `IO` 和 `Collections`）。 借助引用给定命名空间的 `using` 指令，可以非限定的方式使用作为相应命名空间成员的类型。 由于使用 `using` 指令，因此程序可以使用 `Console.WriteLine` 作为 `System.Console.WriteLine` 的简写。

“Hello, World”程序声明的 `Hello` 类只有一个成员，即 `Main` 方法。 `Main` 方法使用 `static` 修饰符进行声明。 实例方法可以使用关键字 `this` 引用特定的封闭对象实例，而静态方法则可以在不引用特定对象的情况下运行。 按照约定，`Main` 静态方法是 C# 程序的入口点。

程序的输出是由 `System` 命名空间中 `Console` 类的 `WriteLine` 方法生成。 此类由标准类库提供。默认情况下，编译器会自动引用标准类库。

## <a name="types-and-variables"></a>类型和变量

C# 有两种类型：*值类型* 和 *引用类型*。 值类型的变量直接包含它们的数据。 引用类型的变量存储对数据（称为“对象”）的引用。 对于引用类型，两个变量可以引用同一个对象；对一个变量执行的运算可能会影响另一个变量引用的对象。 借助值类型，每个变量都有自己的数据副本；因此，对一个变量执行的运算不会影响另一个变量（`ref` 和 `out` 参数变量除外）。

标识符为变量名称。 标识符是不包含任何空格的 unicode 字符序列。 如果标识符的前缀为 `@`，则该标识符可以是 C# 保留字。 在与其他语言交互时，使用保留字作为标识符很有用。

C# 的值类型进一步分为：简单类型、枚举类型、结构类型、可以为 null 的值类型和元组值类型。 C# 引用类型又细分为类类型、接口类型、数组类型和委托类型。    

以下大纲概述了 C# 的类型系统。

- [值类型](../language-reference/builtin-types/value-types.md)
  - [简单类型](../language-reference/builtin-types/value-types.md#built-in-value-types)
    - [有符号整型](../language-reference/builtin-types/integral-numeric-types.md)：`sbyte`、`short`、`int`、`long`
    - [无符号整型](../language-reference/builtin-types/integral-numeric-types.md)：`byte`、`ushort`、`uint`、`ulong`
    - [Unicode 字符](../../standard/base-types/character-encoding-introduction.md)：`char`，表示 UTF-16 代码单元
    - [IEEE 二进制浮点](../language-reference/builtin-types/floating-point-numeric-types.md)：`float`、`double`
    - [高精度十进制浮点数](../language-reference/builtin-types/floating-point-numeric-types.md)：`decimal`
    - 布尔值：`bool`，表示布尔值（`true` 或 `false`）
  - [枚举类型](../language-reference/builtin-types/enum.md)
    - `enum E {...}` 格式的用户定义类型。 `enum` 类型是一种包含已命名常量的独特类型。 每个 `enum` 类型都有一个基础类型（必须是八种整型类型之一）。 `enum` 类型的值集与基础类型的值集相同。
  - [结构类型](../language-reference/builtin-types/struct.md)
    - 格式为 `struct S {...}` 的用户定义类型
  - [可以为 null 的值类型](../language-reference/builtin-types/nullable-value-types.md)
    - 值为 `null` 的其他所有值类型的扩展
  - [元组值类型](../language-reference/builtin-types/value-tuples.md)
    - 格式为 `(T1, T2, ...)` 的用户定义类型
- [引用类型](../language-reference/keywords/reference-types.md)
  - [类类型](../language-reference/keywords/class.md)
    - 其他所有类型的最终基类：`object`
    - [Unicode 字符串](../../standard/base-types/character-encoding-introduction.md)：`string`，表示 UTF-16 代码单元序列
    - 格式为 `class C {...}` 的用户定义类型
  - [接口类型](../language-reference/keywords/interface.md)
    - 格式为 `interface I {...}` 的用户定义类型
  - [数组类型](../programming-guide/arrays/index.md)
    - 一维、多维和交错。 例如：`int[]`、`int[,]` 和 `int[][]`
  - [委托类型](../language-reference/builtin-types/reference-types.md#the-delegate-type)
    - 格式为 `delegate int D(...)` 的用户定义类型

C# 程序使用 *类型声明* 创建新类型。 类型声明指定新类型的名称和成员。 用户可定义以下六种 C# 类型：类类型、结构类型、接口类型、枚举类型、委托类型和元组值类型。

- `class` 类型定义包含数据成员（字段）和函数成员（方法、属性及其他）的数据结构。 类类型支持单一继承和多形性，即派生类可以扩展和专门针对基类的机制。
- `struct` 类型定义包含数据成员和函数成员的结构，这一点与类类型相似。 不过，与类不同的是，结构是值类型，通常不需要进行堆分配。 结构类型不支持用户指定的继承，并且所有结构类型均隐式继承自类型 `object`。
- `interface` 类型将协定定义为一组已命名的公共成员。 实现 `interface` 的 `class` 或 `struct` 必须提供接口成员的实现代码。 `interface` 可以继承自多个基接口，`class` 和 `struct` 可以实现多个接口。
- `delegate` 类型表示引用包含特定参数列表和返回类型的方法。 通过委托，可以将方法视为可分配给变量并可作为参数传递的实体。 委托类同于函数式语言提供的函数类型。 它们还类似于其他一些语言中存在的“函数指针”概念。 与函数指针不同，委托是面向对象且类型安全的。

`class`、`struct`、`interface` 和 `delegate` 类型全部都支持泛型，因此可以使用其他类型对它们进行参数化。

C# 支持任意类型的一维和多维数组。 与上述类型不同，数组类型无需先声明即可使用。 相反，数组类型是通过在类型名称后面添加方括号构造而成。 例如，`int[]` 是 `int` 类型的一维数组，`int[,]` 是 `int` 类型的二维数组，`int[][]` 是由 `int` 类型的一维数组或“交错”数组构成的一维数组。

可以为 null 的类型不需要单独定义。 对于所有不可以为 null 的类型 `T`，都有对应的可以为 null 的类型 `T?`，后者可以包含附加值 `null`。 例如，`int?` 是可保存任何 32 位整数或 `null` 值的类型，`string?` 是可以保存任何 `string` 或 `null` 值的类型。

C# 采用统一的类型系统，因此任意类型的值都可视为 `object`。 每种 C# 类型都直接或间接地派生自 `object` 类类型，而 `object` 是所有类型的最终基类。 只需将值视为类型 `object`，即可将引用类型的值视为对象。 通过执行 *装箱* 和 *取消装箱操作*，可以将值类型的值视为对象。 在以下示例中，`int` 值被转换成 `object`，然后又恢复成 `int`。

:::code language="csharp" source="./snippets/shared/Program.cs" ID="boxing" :::

将值类型的值分配给 `object` 对象引用时，会分配一个“箱”来保存此值。 该箱是引用类型的实例，此值会被复制到该箱。 相反，当 `object` 引用被显式转换成值类型时，将检查引用的 `object` 是否是具有正确值类型的箱。 如果检查成功，则会将箱中的值复制到值类型。

C# 的统一类型系统实际上意味着“按需”将值类型视为 `object` 引用。 鉴于这种统一性，使用类型 `object` 的常规用途库可以与派生自 `object` 的所有类型结合使用，包括引用类型和值类型。

C# 有多种 *变量*，其中包括字段、数组元素、局部变量和参数。 变量表示存储位置。 每个变量都具有一种类型，用于确定可以在变量中存储哪些值，如下文所述。

- 不可以为 null 的值类型
  - 具有精确类型的值
- 可以为 null 的值类型
  - `null` 值或具有精确类型的值
- object
  - `null` 引用、对任意引用类型的对象的引用，或对任意值类型的装箱值的引用
- 类类型
  - `null` 引用、对类类型实例的引用，或对派生自类类型的类实例的引用
- 接口类型
  - `null` 引用、对实现接口类型的类类型实例的引用，或对实现接口类型的值类型的装箱值的引用
- 数组类型
  - `null` 引用、对数组类型实例的引用，或对兼容的数组类型实例的引用
- 委托类型
  - `null` 引用或对兼容的委托类型实例的引用

## <a name="program-structure"></a>程序结构

C# 中的关键组织结构概念包括[程序](../programming-guide/inside-a-program/index.md)、[命名空间](../programming-guide/namespaces/index.md)、[类型](../programming-guide/types/index.md)、[成员](../programming-guide/classes-and-structs/members.md)和[程序集](../../standard/assembly/index.md)*    。 程序声明类型，而类型则包含成员，并被整理到命名空间中。 类型示例包括类、结构和接口。 成员示例包括字段、方法、属性和事件。 编译完的 C# 程序实际上会打包到程序集中。 程序集的文件扩展名通常为 `.exe` 或 `.dll`，具体视其分别实现的是***应用程序***还是***库***而定。

作为一个小示例，请考虑包含以下代码的程序集：

:::code language="csharp" source="./snippets/shared/AcmeStack.cs":::

此类的完全限定的名称为 `Acme.Collections.Stack`。 此类包含多个成员：一个 `top` 字段、两个方法（`Push` 和 `Pop`）和一个 `Entry` 嵌套类。 `Entry` 类还包含三个成员：一个 `next` 字段、一个 `data` 字段和一个构造函数。 `Stack` 是泛型类。 它具有一个类型参数 `T`，在使用时替换为具体类型。

> [!NOTE]
> 堆栈是一个“先进后出”(FILO) 集合。 添加到堆栈顶部的新元素。 删除元素时，将从堆栈顶部删除该元素。

程序集包含中间语言 (IL) 指令形式的可执行代码和元数据形式的符号信息。 执行前，.NET 公共语言运行时的实时 (JIT) 编译器会将程序集中的 IL 代码转换为特定于处理器的代码。

由于程序集是包含代码和元数据的自描述功能单元，因此无需在 C# 中使用 `#include` 指令和头文件。 只需在编译程序时引用特定的程序集，即可在 C# 程序中使用此程序集中包含的公共类型和成员。 例如，此程序使用 `acme.dll` 程序集中的 `Acme.Collections.Stack` 类：

:::code language="csharp" source="./snippets/shared/StackUsage.cs":::

若要编译此程序，需要引用包含前面示例中定义的堆栈类的程序集。

C# 程序可以存储在多个源文件中。 在编译 C# 程序时，将同时处理所有源文件，并且源文件可以自由地相互引用。 从概念上讲，就好像所有源文件在被处理之前都连接到一个大文件。 在 C# 中永远都不需要使用前向声明，因为声明顺序无关紧要（极少数例外情况除外）。 C# 并不限制源文件只能声明一种公共类型，也不要求源文件的文件名必须与其中声明的类型相匹配。

本教程中的后续文章介绍了这些组织块。

>[!div class="step-by-step"]
>[下一页](types.md)
