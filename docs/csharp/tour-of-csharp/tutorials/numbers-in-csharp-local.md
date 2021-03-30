---
title: C# 中的数字 - C# 教程简介
description: 通过探索数字类型及其用途、属性和方法了解 C#。
ms.date: 02/05/2021
ms.openlocfilehash: d6546fc8ac2609066a4f9b829749a4091fce7ce6
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881102"
---
# <a name="manipulate-integral-and-floating-point-numbers-in-c"></a>处理 C\# 中的整数和浮点数

本教程以交互方式介绍了 C# 中的数字类型。 你将编写少量的代码，然后编译并运行这些代码。 本教程包含一系列课程，介绍了 C# 中的数字和数学运算。 这些课程介绍了 C# 语言的基础知识。

## <a name="prerequisites"></a>先决条件

本教程要求安装一台虚拟机，以用于本地开发。 在 Windows、Linux 或 macOS 上，可以使用 .NET CLI 创建、生成和运行应用程序。 在 Mac 或 Windows 上，可以使用 Visual Studio 2019。 有关设置说明，请参阅[设置本地环境](local-environment.md)。

## <a name="explore-integer-math"></a>探索整数数学运算

创建名为 numbers-quickstart  的目录。 将它设为当前目录并运行以下命令：

```dotnetcli
dotnet new console -n NumbersInCSharp -o .
```

在常用编辑器中，打开 Program.cs，并将文件内容替换为以下代码：

```csharp
using System;

int a = 18;
int b = 6;
int c = a + b;
Console.WriteLine(c);
```

通过在命令窗口中键入 `dotnet run` 运行此代码。

上面是基本的整数数学运算之一。 `int` 类型表示整数  （零、正整数或负整数）。 使用 `+` 符号执行加法运算。 其他常见的整数数学运算包括：

- `-`：减法运算
- `*`：乘法运算
- `/`：除法运算

首先，探索这些不同类型的运算。 在写入 `c` 值的行之后添加以下行：

```csharp
// subtraction
c = a - b;
Console.WriteLine(c);

// multiplication
c = a * b;
Console.WriteLine(c);

// division
c = a / b;
Console.WriteLine(c);
```

通过在命令窗口中键入 `dotnet run` 运行此代码。

如果愿意，还可以通过在同一行中编写多个数学运算来进行试验。 例如，请尝试 `c = a + b - 12 * 17;`。 允许混合使用变量和常数。

> [!TIP]
> 在探索 C#（或任何编程语言）的过程中，可能会在编写代码时犯错。 编译器  会发现并报告这些错误。 如果包含错误消息，请仔细比对示例代码和你的窗口中的代码，看看哪些需要纠正。 这样做有助于了解 C# 代码结构。

你已完成第一步。 开始进入下一部分前，先将当前代码移到单独的方法中。 方法是组合在一起并赋予名称的一系列语句。 通过在方法名称后面编写 `()`，可以调用一个方法。 将代码组织为方法，可以更轻松地开始使用新示例。 完成后，代码应如下所示：

```csharp
using System;

WorkWithIntegers();

void WorkWithIntegers()
{
    int a = 18;
    int b = 6;
    int c = a + b;
    Console.WriteLine(c);


    // subtraction
    c = a - b;
    Console.WriteLine(c);

    // multiplication
    c = a * b;
    Console.WriteLine(c);

    // division
    c = a / b;
    Console.WriteLine(c);
}
```

行 `WorkWithIntegers();` 调用方法。 下面的代码声明方法并对其进行定义。

## <a name="explore-order-of-operations"></a>探索运算顺序

注释掉对 `WorkingWithIntegers()` 的调用。 在本节中，它会在你工作时使输出变得不那么杂乱：

```csharp
//WorkWithIntegers();
```

`//` 在 C# 中启动 注释。 注释是你想要保留在源代码中但不能作为代码执行的任何文本。 编译器不会从注释中生成任何可执行代码。 因为 `WorkWithIntegers()` 是一种方法，所以只需注释掉一行。

C# 语言使用与数学运算规则一致的规则，定义不同数学运算的优先级。 乘法和除法的优先级高于加法和减法。 通过在调用 `WorkWithIntegers()` 后添加以下代码并执行 `dotnet run` 来探索：

```csharp
int a = 5;
int b = 4;
int c = 2;
int d = a + b * c;
Console.WriteLine(d);
 ```

输出结果表明，乘法先于加法执行。

可以在要优先执行的一个或多个运算前后添加括号，从而强制改变运算顺序。 添加以下行并再次运行：

```csharp
d = (a + b) * c;
Console.WriteLine(d);
```

通过组合多个不同的运算，进一步探索运算顺序。 添加以下几行内容。 重试 `dotnet run`。

```csharp
d = (a + b) - 6 * c + (12 * 4) / 3 + 12;
Console.WriteLine(d);
```

可能已注意到，整数有一个非常有趣的行为。 整数除法始终生成整数结果，即使预期结果有小数或分数部分也是如此。

如果还没有见过这种行为，请尝试运行以下代码：

```csharp
int e = 7;
int f = 4;
int g = 3;
int h = (e + f) / g;
Console.WriteLine(h);
```

再次键入 `dotnet run`，看看结果如何。

继续之前，让我们获取你在本节编写的所有代码并放在新的方法中。 调用新方法 `OrderPrecedence`。 代码应如下所示：

```csharp
using System;

// WorkWithIntegers();
OrderPrecedence();

void WorkWithIntegers()
{
    int a = 18;
    int b = 6;
    int c = a + b;
    Console.WriteLine(c);


    // subtraction
    c = a - b;
    Console.WriteLine(c);

    // multiplication
    c = a * b;
    Console.WriteLine(c);

    // division
    c = a / b;
    Console.WriteLine(c);
}

void OrderPrecedence()
{
    int a = 5;
    int b = 4;
    int c = 2;
    int d = a + b * c;
    Console.WriteLine(d);

    d = (a + b) * c;
    Console.WriteLine(d);

    d = (a + b) - 6 * c + (12 * 4) / 3 + 12;
    Console.WriteLine(d);

    int e = 7;
    int f = 4;
    int g = 3;
    int h = (e + f) / g;
    Console.WriteLine(h);
}
```

## <a name="explore-integer-precision-and-limits"></a>探索整数运算精度和限值

在上一个示例中，整数除法截断了结果。 可以使用取模  运算符（即 `%` 字符）计算余数  。 对 `OrderPrecedence()` 进行方法调用之后，尝试添加以下代码：

```csharp
int a = 7;
int b = 4;
int c = 3;
int d = (a + b) / c;
int e = (a + b) % c;
Console.WriteLine($"quotient: {d}");
Console.WriteLine($"remainder: {e}");
```

C# 整数类型不同于数学上的整数的另一点是，`int` 类型有最小限值和最大限值。 添加以下代码以查看这些限制：

```csharp
int max = int.MaxValue;
int min = int.MinValue;
Console.WriteLine($"The range of integers is {min} to {max}");
```

如果运算生成的值超过这些限值，则会出现下溢  或溢出  的情况。 答案似乎是从一个限值覆盖到另一个限值的范围。 添加以下两行以查看示例：

```csharp
int what = max + 3;
Console.WriteLine($"An example of overflow: {what}");
```

可以看到，答案非常接近最小（负）整数。 与 `min + 2` 相同。 加法运算会让整数溢出允许的值。 答案是一个非常大的负数，因为溢出从最大整数值覆盖回最小整数值。

如果 `int` 类型无法满足需求，还会用到限值和精度不同的其他数字类型。 接下来，让我们来研究一下其他类型。 在开始下一节内容之前，将你在本节编写的代码移到一个单独的方法中。 将其命名为 `TestLimits`。

## <a name="work-with-the-double-type"></a>使用双精度类型

`double` 数字类型表示双精度浮点数。 这些词可能是第一次听说。 浮点  数可用于表示数量级可能非常大或非常小的非整数。 双精度  是一个相对术语，描述用于存储值的二进制数位数。 双精度  数字的二进制数位数是单精度  的两倍。 在新式计算机上，使用双精度数字比使用单精度数字更为常见。 单精度  数字是使用 `float` 关键字声明的。 接下来，将探索双精度类型。 添加以下代码并查看结果：

```csharp
double a = 5;
double b = 4;
double c = 2;
double d = (a + b) / c;
Console.WriteLine(d);
```

可以看到，答案商包含小数部分。 试试对双精度类型使用更复杂一点的表达式：

```csharp
double e = 19;
double f = 23;
double g = 8;
double h = (e + f) / g;
Console.WriteLine(h);
```

双精度值的范围远大于整数值。 在当前已编写的代码下方试运行以下代码：

```csharp
double max = double.MaxValue;
double min = double.MinValue;
Console.WriteLine($"The range of double is {min} to {max}");
```

打印的这些值用科学记数法表示。 `E` 左侧为有效数字。 右侧为是 10 的 n 次幂。 与数学上的十进制数字一样，C# 中的双精度值可能会有四舍五入误差。 试运行以下代码：

```csharp
double third = 1.0 / 3.0;
Console.WriteLine(third);
```

众所周知，`0.3` 循环小数与 `1/3` 并不完全相等。

***挑战***

尝试使用 `double` 类型执行其他的大小数、乘法和除法运算。 尝试执行更复杂的运算。 花了一些时间应对挑战之后，获取已编写的代码并放在一个新方法中。 将新方法命名为 `WorkWithDoubles`。

## <a name="work-with-decimal-types"></a>使用十进制类型

大家已学习了 C# 中的基本数字类型，即整数和双精度。 还需要学习另一种类型，即 `decimal` 类型。 `decimal` 类型的范围较小，但精度高于 `double`。 让我们来实际操作一下：

```csharp
decimal min = decimal.MinValue;
decimal max = decimal.MaxValue;
Console.WriteLine($"The range of the decimal type is {min} to {max}");
```

可以看到，范围小于 `double` 类型。 通过试运行以下代码，可以看到十进制类型的精度更高：

```csharp
double a = 1.0;
double b = 3.0;
Console.WriteLine(a / b);

decimal c = 1.0M;
decimal d = 3.0M;
Console.WriteLine(c / d);
```

数字中的 `M` 后缀指明了常数应如何使用 `decimal` 类型。 否则，编译器假定为 `double` 类型。

> [!NOTE]
> 字母 `M` 被选为 `double` 与 `decimal` 关键字之间最明显不同的字母。

可以看到，使用十进制类型执行数学运算时，十进制小数点右侧的数字更多。

***挑战***

至此，大家已了解不同的数字类型。请编写代码来计算圆面积（其中，半径为 2.50 厘米）。 请注意，圆面积是用半径的平方乘以 PI 进行计算。 小提示：.NET 包含 PI 常数 <xref:System.Math.PI?displayProperty=nameWithType>，可用于相应的值计算。 与 `System.Math` 命名空间中声明的所有常量一样，<xref:System.Math.PI?displayProperty=nameWithType> 也是 `double` 值。 因此，应使用 `double`（而不是 `decimal`）值来完成这项挑战。

你应获得 19 和 20 之间的答案。 要查看你的答案，可以[查看 GitHub 上的完成示例代码](https://github.com/dotnet/samples/tree/main/csharp/numbers-quickstart/Program.cs#L9-L11)。

如果需要，可以试用一些其他公式。

已完成“C# 中的数字”快速入门教程。 可以在自己的开发环境中继续学习[分支和循环](branches-and-loops-local.md)快速入门教程。

若要详细了解 C# 中的数字，可以参阅下面的文章：

- [整型数值类型](../../language-reference/builtin-types/integral-numeric-types.md)
- [浮点型数值类型](../../language-reference/builtin-types/floating-point-numeric-types.md)
- [内置数值转换](../../language-reference/builtin-types/numeric-conversions.md)
