---
title: 分支和循环 - C# 教程简介
description: 在本教程的“分支和循环”中，将编写 C# 代码以研究支持条件分支和循环重复执行语句的语言语法。
ms.date: 02/05/2021
ms.openlocfilehash: ae57b370022bfc86200ca20a913d44f64e46d68d
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881115"
---
# <a name="learn-conditional-logic-with-branch-and-loop-statements"></a>通过分支和循环语句了解条件逻辑

本教程介绍了如何编写代码，从而检查变量，并根据这些变量更改执行路径。 读者可以编写 C# 代码并查看代码编译和运行结果。 本教程包含一系列课程，介绍了 C# 中的分支和循环构造。 这些课程介绍了 C# 语言的基础知识。

## <a name="prerequisites"></a>先决条件

本教程要求安装一台虚拟机，以用于本地开发。 在 Windows、Linux 或 macOS 上，可以使用 .NET CLI 创建、生成和运行应用程序。 在 Mac 和 Windows 上，可以使用 Visual Studio 2019。 有关创建说明，请参阅[创建本地环境](local-environment.md)。

## <a name="make-decisions-using-the-if-statement"></a>使用 `if` 语句做出决定

创建名为 branches-tutorial 的目录。 将其设为当前目录并运行以下命令：

```dotnetcli
dotnet new console -n BranchesAndLoops -o .
```

此命令在当前目录中新建一个 .NET 控制台应用程序。 在常用编辑器中，打开 Program.cs，并将内容替换为以下代码：

```csharp
using System;

int a = 5;
int b = 6;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10.");
```

通过在控制台窗口键入 `dotnet run` 试运行此代码。 你应在控制台上看到以下消息：“The answer is greater than 10. （答案大于 10）”。 修改 `b` 的声明，让总和小于 10：

```csharp
int b = 3;
```

再次键入 `dotnet run`。 由于答案小于 10，因此什么也没有打印出来。 要测试的条件为 false。 没有任何可供执行的代码，因为仅为 `if` 语句编写了一个可能分支，即 true 分支。

> [!TIP]
> 在探索 C#（或任何编程语言）的过程中，可能会在编写代码时犯错。 编译器会发现并报告这些错误。 仔细查看错误输出和生成错误的代码。 编译器错误通常可帮助你找出问题。

第一个示例展示了 `if` 和布尔类型的用途。 布尔变量可以包含下列两个值之一：`true` 或 `false`。 C# 为布尔变量定义了特殊类型 `bool`。 `if` 语句检查 `bool` 的值。 如果值为 `true`，执行 `if` 后面的语句。 否则，跳过这些语句。 这种检查条件并根据条件执行语句的过程非常强大。

## <a name="make-if-and-else-work-together"></a>让 if 和 else 完美配合

若要执行 true 和 false 分支中的不同代码，请创建在条件为 false 时执行的 `else` 分支。 尝试使用 `else` 分支。 添加以下代码中的最后两行（你应该已经有前四行）：

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10");
else
    Console.WriteLine("The answer is not greater than 10");
```

只有当条件的测试结果为 `false` 时，才执行 `else` 关键字后面的语句。 将 `if`、`else` 与布尔条件相结合，可以满足处理 `true` 和 `false` 所需的一切要求。

> [!IMPORTANT]
> `if` 和 `else` 语句下的缩进是为了方便读者阅读。 C# 语言忽略缩进或空格。 `if` 或 `else` 关键字后面的语句根据条件决定是否执行。 本教程中的所有示例都遵循了常见做法，根据语句的控制流缩进代码行。

由于缩进会被忽略，因此需要使用 `{` 和 `}` 指明何时要在根据条件决定是否执行的代码块中添加多个语句。 C# 程序员通常会对所有 `if` 和 `else` 子句使用这些大括号。 以下示例与你创建的示例相同。 修改上面的代码以匹配下面的代码：

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
{
    Console.WriteLine("The answer is greater than 10");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
}
```

> [!TIP]
> 在本教程的其余部分中，代码示例全都遵循公认做法，添加了大括号。

可以测试更复杂的条件。 在当前已编写的代码之后添加以下代码：

```csharp
int c = 4;
if ((a + b + c > 10) && (a == b))
{
    Console.WriteLine("The answer is greater than 10");
    Console.WriteLine("And the first number is equal to the second");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
    Console.WriteLine("Or the first number is not equal to the second");
}
```

`==` 符号执行相等测试。 使用 `==` 将相等测试与赋值测试区分开来，如在 `a = 5` 中所见。

`&&` 表示“且”。 也就是说，两个条件必须都为 true，才能执行 true 分支中的语句。  这些示例还表明，可以在每个条件分支中添加多个语句，前提是将它们用 `{` 和 `}` 括住。 还可以使用 `||` 表示“或”。 在当前已编写的代码之后添加以下代码：

```csharp
if ((a + b + c > 10) || (a == b))
{
    Console.WriteLine("The answer is greater than 10");
    Console.WriteLine("Or the first number is equal to the second");
}
else
{
    Console.WriteLine("The answer is not greater than 10");
    Console.WriteLine("And the first number is not equal to the second");
}
```

修改 `a`、`b` 和 `c` 的值，并在 `&&` 和 `||` 之间切换浏览。 你将进一步了解 `&&` 和 `||` 运算符的工作原理。

你已完成第一步。 开始进入下一部分前，先将当前代码移到单独的方法中。 这样一来，可以更轻松地开始处理新示例。 将现有代码放入名为 `ExploreIf()` 的方法中。 从程序顶部调用它。 完成这些更改后，代码看起来应类似下面这样：

```csharp
using System;

ExploreIf();

void ExploreIf()
{
    int a = 5;
    int b = 3;
    if (a + b > 10)
    {
        Console.WriteLine("The answer is greater than 10");
    }
    else
    {
        Console.WriteLine("The answer is not greater than 10");
    }

    int c = 4;
    if ((a + b + c > 10) && (a > b))
    {
        Console.WriteLine("The answer is greater than 10");
        Console.WriteLine("And the first number is greater than the second");
    }
    else
    {
        Console.WriteLine("The answer is not greater than 10");
        Console.WriteLine("Or the first number is not greater than the second");
    }

    if ((a + b + c > 10) || (a > b))
    {
        Console.WriteLine("The answer is greater than 10");
        Console.WriteLine("Or the first number is greater than the second");
    }
    else
    {
        Console.WriteLine("The answer is not greater than 10");
        Console.WriteLine("And the first number is not greater than the second");
    }
}
```

注释掉对 `ExploreIf()` 的调用。 在本节中，它会在你工作时使输出变得不那么杂乱：

```csharp
//ExploreIf();
```

`//` 在 C# 中启动 注释。 注释是你想要保留在源代码中但不能作为代码执行的任何文本。 编译器不会从注释中生成任何可执行代码。

## <a name="use-loops-to-repeat-operations"></a>使用循环重复执行运算

在本节使用循环以重复执行语句。 在调用 `ExploreIf` 后，添加以下代码：

```csharp
int counter = 0;
while (counter < 10)
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
}
```

`while` 语句会检查条件，并执行 `while` 后面的语句或语句块。 除非条件为 false，否则它会重复检查条件并执行这些语句。

此示例新引入了另外一个运算符。 `counter` 变量后面的 `++` 是增量运算符。 它将 `counter` 值加 1，并将计算后的值存储在 `counter` 变量中。

> [!IMPORTANT]
> 请确保 `while` 循环条件在你执行代码时更改为 false。 否则，创建的就是无限循环，即程序永不结束。 本示例中没有演示上述情况，因为你必须使用 CTRL-C 或其他方法强制退出程序。

`while` 循环先测试条件，然后再执行 `while` 后面的代码。 `do` ... `while` 循环先执行代码，然后再检查条件。 下面的代码对 do while 循环进行了演示：

```csharp
int counter = 0;
do
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
} while (counter < 10);
```

此 `do` 循环和前面的 `while` 循环生成的输出结果相同。

## <a name="work-with-the-for-loop"></a>使用 for 循环

For 循环通常用在 C# 中。 试运行以下代码：

```csharp
for (int index = 0; index < 10; index++)
{
    Console.WriteLine($"Hello World! The index is {index}");
}
```

上述代码的工作原理与已用过的 `while` 循环和 `do` 循环相同。 `for` 语句包含三个控制具体工作方式的部分。

第一部分是 for 初始值设定项：`int index = 0;` 声明 `index` 是循环变量，并将它的初始值设置为 `0`。

中间部分是 for 条件：`index < 10` 声明只要计数器值小于 10，此 `for` 循环就会继续执行。

最后一部分是 for 迭代器：`index++` 指定在执行 `for` 语句后面的代码块后，如何修改循环变量。 在此示例中，它指定 `index` 应在代码块每次执行时递增 1。

请自行试验。 尝试以下每种变化：

- 将初始值设定项更改为其他初始值。
- 将结束条件设定项更改为其他值。

完成后，继续利用所学知识，试着自己编写一些代码。

本教程中未介绍的另一个循环语句：`foreach` 语句。 `foreach` 语句为项序列中的每一项重复其语句。 它最常用于集合，因此将在下一教程中介绍。

## <a name="created-nested-loops"></a>已创建嵌套循环

`while`、`do` 或 `for` 循环可以嵌套在另一个循环内，以使用外部循环中的各项与内部循环中的各项组合来创建矩阵。 我们来生成一组字母数字对以表示行和列。

一个 `for` 循环可以生成行：

```csharp
for (int row = 1; row < 11; row++)
{
    Console.WriteLine($"The row is {row}");
}
```

另一个循环可以生成列：

```csharp
for (char column = 'a'; column < 'k'; column++)
{
    Console.WriteLine($"The column is {column}");
}
```

可以将一个循环嵌套在另一个循环中以形成各个对：

```csharp
for (int row = 1; row < 11; row++)
{
    for (char column = 'a'; column < 'k'; column++)
    {
        Console.WriteLine($"The cell is ({row}, {column})");
    }
}
```

可以看到，对于内部循环的每次完整运行，外部循环会递增一次。 反转行和列嵌套，自行查看变化。 完成后，将此部分中的代码放入名为 `ExploreLoops()` 的方法中。

## <a name="combine-branches-and-loops"></a>结合使用分支和循环

支持，大家已了解 C# 语言中的 `if` 语句和循环构造。看看能否编写 C# 代码，计算 1 到 20 中所有可被 3 整除的整数的总和。  下面提供了一些提示：

- `%` 运算符可用于获取除法运算的余数。
- `if` 语句中的条件可用于判断是否应将数字计入总和。
- `for` 循环有助于对 1 到 20 中的所有数字重复执行一系列步骤。

亲自试一试吧。 然后，看看自己是怎么做到的。 你应获取的答案为 63。 通过[在 GitHub 上查看已完成的代码](https://github.com/dotnet/samples/blob/main/csharp/branches-quickstart/Program.cs#L87-L95)，你可以看到一个可能的答案。

已完成“分支和循环”教程。

可以在自己的开发环境中继续学习[数组和集合](arrays-and-collections.md)教程。

若要详细了解这些概念，请参阅下列文章：

- [If 和 else 语句](../../language-reference/keywords/if-else.md)
- [While 语句](../../language-reference/keywords/while.md)
- [Do 语句](../../language-reference/keywords/do.md)
- [For 语句](../../language-reference/keywords/for.md)
