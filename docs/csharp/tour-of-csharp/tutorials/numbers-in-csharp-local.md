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
# <a name="manipulate-integral-and-floating-point-numbers-in-c"></a><span data-ttu-id="fa14d-103">处理 C\# 中的整数和浮点数</span><span class="sxs-lookup"><span data-stu-id="fa14d-103">Manipulate integral and floating point numbers in C\#</span></span>

<span data-ttu-id="fa14d-104">本教程以交互方式介绍了 C# 中的数字类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-104">This tutorial teaches you about the numeric types in C# interactively.</span></span> <span data-ttu-id="fa14d-105">你将编写少量的代码，然后编译并运行这些代码。</span><span class="sxs-lookup"><span data-stu-id="fa14d-105">You'll write small amounts of code, then you'll compile and run that code.</span></span> <span data-ttu-id="fa14d-106">本教程包含一系列课程，介绍了 C# 中的数字和数学运算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-106">The tutorial contains a series of lessons that explore numbers and math operations in C#.</span></span> <span data-ttu-id="fa14d-107">这些课程介绍了 C# 语言的基础知识。</span><span class="sxs-lookup"><span data-stu-id="fa14d-107">These lessons teach you the fundamentals of the C# language.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fa14d-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="fa14d-108">Prerequisites</span></span>

<span data-ttu-id="fa14d-109">本教程要求安装一台虚拟机，以用于本地开发。</span><span class="sxs-lookup"><span data-stu-id="fa14d-109">The tutorial expects that you have a machine set up for local development.</span></span> <span data-ttu-id="fa14d-110">在 Windows、Linux 或 macOS 上，可以使用 .NET CLI 创建、生成和运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="fa14d-110">On Windows, Linux, or macOS, you can use the .NET CLI to create, build, and run applications.</span></span> <span data-ttu-id="fa14d-111">在 Mac 或 Windows 上，可以使用 Visual Studio 2019。</span><span class="sxs-lookup"><span data-stu-id="fa14d-111">On Mac or Windows, you can use Visual Studio 2019.</span></span> <span data-ttu-id="fa14d-112">有关设置说明，请参阅[设置本地环境](local-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="fa14d-112">For setup instructions, see [Set up your local environment](local-environment.md).</span></span>

## <a name="explore-integer-math"></a><span data-ttu-id="fa14d-113">探索整数数学运算</span><span class="sxs-lookup"><span data-stu-id="fa14d-113">Explore integer math</span></span>

<span data-ttu-id="fa14d-114">创建名为 numbers-quickstart  的目录。</span><span class="sxs-lookup"><span data-stu-id="fa14d-114">Create a directory named *numbers-quickstart*.</span></span> <span data-ttu-id="fa14d-115">将它设为当前目录并运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="fa14d-115">Make it the current directory and run the following command:</span></span>

```dotnetcli
dotnet new console -n NumbersInCSharp -o .
```

<span data-ttu-id="fa14d-116">在常用编辑器中，打开 Program.cs，并将文件内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa14d-116">Open *Program.cs* in your favorite editor, and replace the contents of the file with the following code:</span></span>

```csharp
using System;

int a = 18;
int b = 6;
int c = a + b;
Console.WriteLine(c);
```

<span data-ttu-id="fa14d-117">通过在命令窗口中键入 `dotnet run` 运行此代码。</span><span class="sxs-lookup"><span data-stu-id="fa14d-117">Run this code by typing `dotnet run` in your command window.</span></span>

<span data-ttu-id="fa14d-118">上面是基本的整数数学运算之一。</span><span class="sxs-lookup"><span data-stu-id="fa14d-118">You've seen one of the fundamental math operations with integers.</span></span> <span data-ttu-id="fa14d-119">`int` 类型表示整数  （零、正整数或负整数）。</span><span class="sxs-lookup"><span data-stu-id="fa14d-119">The `int` type represents an **integer**, a zero, positive, or negative whole number.</span></span> <span data-ttu-id="fa14d-120">使用 `+` 符号执行加法运算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-120">You use the `+` symbol for addition.</span></span> <span data-ttu-id="fa14d-121">其他常见的整数数学运算包括：</span><span class="sxs-lookup"><span data-stu-id="fa14d-121">Other common mathematical operations for integers include:</span></span>

- <span data-ttu-id="fa14d-122">`-`：减法运算</span><span class="sxs-lookup"><span data-stu-id="fa14d-122">`-` for subtraction</span></span>
- <span data-ttu-id="fa14d-123">`*`：乘法运算</span><span class="sxs-lookup"><span data-stu-id="fa14d-123">`*` for multiplication</span></span>
- <span data-ttu-id="fa14d-124">`/`：除法运算</span><span class="sxs-lookup"><span data-stu-id="fa14d-124">`/` for division</span></span>

<span data-ttu-id="fa14d-125">首先，探索这些不同类型的运算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-125">Start by exploring those different operations.</span></span> <span data-ttu-id="fa14d-126">在写入 `c` 值的行之后添加以下行：</span><span class="sxs-lookup"><span data-stu-id="fa14d-126">Add these lines after the line that writes the value of `c`:</span></span>

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

<span data-ttu-id="fa14d-127">通过在命令窗口中键入 `dotnet run` 运行此代码。</span><span class="sxs-lookup"><span data-stu-id="fa14d-127">Run this code by typing `dotnet run` in your command window.</span></span>

<span data-ttu-id="fa14d-128">如果愿意，还可以通过在同一行中编写多个数学运算来进行试验。</span><span class="sxs-lookup"><span data-stu-id="fa14d-128">You can also experiment by writing multiple mathematics operations in the same line, if you'd like.</span></span> <span data-ttu-id="fa14d-129">例如，请尝试 `c = a + b - 12 * 17;`。</span><span class="sxs-lookup"><span data-stu-id="fa14d-129">Try `c = a + b - 12 * 17;` for example.</span></span> <span data-ttu-id="fa14d-130">允许混合使用变量和常数。</span><span class="sxs-lookup"><span data-stu-id="fa14d-130">Mixing variables and constant numbers is allowed.</span></span>

> [!TIP]
> <span data-ttu-id="fa14d-131">在探索 C#（或任何编程语言）的过程中，可能会在编写代码时犯错。</span><span class="sxs-lookup"><span data-stu-id="fa14d-131">As you explore C# (or any programming language), you'll make mistakes when you write code.</span></span> <span data-ttu-id="fa14d-132">编译器  会发现并报告这些错误。</span><span class="sxs-lookup"><span data-stu-id="fa14d-132">The **compiler** will find those errors and report them to you.</span></span> <span data-ttu-id="fa14d-133">如果包含错误消息，请仔细比对示例代码和你的窗口中的代码，看看哪些需要纠正。</span><span class="sxs-lookup"><span data-stu-id="fa14d-133">When the contains error messages, look closely at the example code and the code in your window to see what to fix.</span></span> <span data-ttu-id="fa14d-134">这样做有助于了解 C# 代码结构。</span><span class="sxs-lookup"><span data-stu-id="fa14d-134">That exercise will help you learn the structure of C# code.</span></span>

<span data-ttu-id="fa14d-135">你已完成第一步。</span><span class="sxs-lookup"><span data-stu-id="fa14d-135">You've finished the first step.</span></span> <span data-ttu-id="fa14d-136">开始进入下一部分前，先将当前代码移到单独的方法中。</span><span class="sxs-lookup"><span data-stu-id="fa14d-136">Before you start the next section, let's move the current code into a separate *method*.</span></span> <span data-ttu-id="fa14d-137">方法是组合在一起并赋予名称的一系列语句。</span><span class="sxs-lookup"><span data-stu-id="fa14d-137">A method is a series of statements grouped together and given a name.</span></span> <span data-ttu-id="fa14d-138">通过在方法名称后面编写 `()`，可以调用一个方法。</span><span class="sxs-lookup"><span data-stu-id="fa14d-138">You call a method by writing the method's name followed by `()`.</span></span> <span data-ttu-id="fa14d-139">将代码组织为方法，可以更轻松地开始使用新示例。</span><span class="sxs-lookup"><span data-stu-id="fa14d-139">Organizing your code into methods makes it easier to start working with a new example.</span></span> <span data-ttu-id="fa14d-140">完成后，代码应如下所示：</span><span class="sxs-lookup"><span data-stu-id="fa14d-140">When you finish, your code should look like this:</span></span>

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

<span data-ttu-id="fa14d-141">行 `WorkWithIntegers();` 调用方法。</span><span class="sxs-lookup"><span data-stu-id="fa14d-141">The line `WorkWithIntegers();` invokes the method.</span></span> <span data-ttu-id="fa14d-142">下面的代码声明方法并对其进行定义。</span><span class="sxs-lookup"><span data-stu-id="fa14d-142">The following code declares the method and defines it.</span></span>

## <a name="explore-order-of-operations"></a><span data-ttu-id="fa14d-143">探索运算顺序</span><span class="sxs-lookup"><span data-stu-id="fa14d-143">Explore order of operations</span></span>

<span data-ttu-id="fa14d-144">注释掉对 `WorkingWithIntegers()` 的调用。</span><span class="sxs-lookup"><span data-stu-id="fa14d-144">Comment out the call to `WorkingWithIntegers()`.</span></span> <span data-ttu-id="fa14d-145">在本节中，它会在你工作时使输出变得不那么杂乱：</span><span class="sxs-lookup"><span data-stu-id="fa14d-145">It will make the output less cluttered as you work in this section:</span></span>

```csharp
//WorkWithIntegers();
```

<span data-ttu-id="fa14d-146">`//` 在 C# 中启动 注释。</span><span class="sxs-lookup"><span data-stu-id="fa14d-146">The `//` starts a **comment** in C#.</span></span> <span data-ttu-id="fa14d-147">注释是你想要保留在源代码中但不能作为代码执行的任何文本。</span><span class="sxs-lookup"><span data-stu-id="fa14d-147">Comments are any text you want to keep in your source code but not execute as code.</span></span> <span data-ttu-id="fa14d-148">编译器不会从注释中生成任何可执行代码。</span><span class="sxs-lookup"><span data-stu-id="fa14d-148">The compiler doesn't generate any executable code from comments.</span></span> <span data-ttu-id="fa14d-149">因为 `WorkWithIntegers()` 是一种方法，所以只需注释掉一行。</span><span class="sxs-lookup"><span data-stu-id="fa14d-149">Because `WorkWithIntegers()` is a method, you need to only comment out one line.</span></span>

<span data-ttu-id="fa14d-150">C# 语言使用与数学运算规则一致的规则，定义不同数学运算的优先级。</span><span class="sxs-lookup"><span data-stu-id="fa14d-150">The C# language defines the precedence of different mathematics operations with rules consistent with the rules you learned in mathematics.</span></span> <span data-ttu-id="fa14d-151">乘法和除法的优先级高于加法和减法。</span><span class="sxs-lookup"><span data-stu-id="fa14d-151">Multiplication and division take precedence over addition and subtraction.</span></span> <span data-ttu-id="fa14d-152">通过在调用 `WorkWithIntegers()` 后添加以下代码并执行 `dotnet run` 来探索：</span><span class="sxs-lookup"><span data-stu-id="fa14d-152">Explore that by adding the following code after the call to `WorkWithIntegers()`, and executing `dotnet run`:</span></span>

```csharp
int a = 5;
int b = 4;
int c = 2;
int d = a + b * c;
Console.WriteLine(d);
 ```

<span data-ttu-id="fa14d-153">输出结果表明，乘法先于加法执行。</span><span class="sxs-lookup"><span data-stu-id="fa14d-153">The output demonstrates that the multiplication is performed before the addition.</span></span>

<span data-ttu-id="fa14d-154">可以在要优先执行的一个或多个运算前后添加括号，从而强制改变运算顺序。</span><span class="sxs-lookup"><span data-stu-id="fa14d-154">You can force a different order of operation by adding parentheses around the operation or operations you want performed first.</span></span> <span data-ttu-id="fa14d-155">添加以下行并再次运行：</span><span class="sxs-lookup"><span data-stu-id="fa14d-155">Add the following lines and run again:</span></span>

```csharp
d = (a + b) * c;
Console.WriteLine(d);
```

<span data-ttu-id="fa14d-156">通过组合多个不同的运算，进一步探索运算顺序。</span><span class="sxs-lookup"><span data-stu-id="fa14d-156">Explore more by combining many different operations.</span></span> <span data-ttu-id="fa14d-157">添加以下几行内容。</span><span class="sxs-lookup"><span data-stu-id="fa14d-157">Add something like the following lines.</span></span> <span data-ttu-id="fa14d-158">重试 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="fa14d-158">Try `dotnet run` again.</span></span>

```csharp
d = (a + b) - 6 * c + (12 * 4) / 3 + 12;
Console.WriteLine(d);
```

<span data-ttu-id="fa14d-159">可能已注意到，整数有一个非常有趣的行为。</span><span class="sxs-lookup"><span data-stu-id="fa14d-159">You may have noticed an interesting behavior for integers.</span></span> <span data-ttu-id="fa14d-160">整数除法始终生成整数结果，即使预期结果有小数或分数部分也是如此。</span><span class="sxs-lookup"><span data-stu-id="fa14d-160">Integer division always produces an integer result, even when you'd expect the result to include a decimal or fractional portion.</span></span>

<span data-ttu-id="fa14d-161">如果还没有见过这种行为，请尝试运行以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa14d-161">If you haven't seen this behavior, try the following code:</span></span>

```csharp
int e = 7;
int f = 4;
int g = 3;
int h = (e + f) / g;
Console.WriteLine(h);
```

<span data-ttu-id="fa14d-162">再次键入 `dotnet run`，看看结果如何。</span><span class="sxs-lookup"><span data-stu-id="fa14d-162">Type `dotnet run` again to see the results.</span></span>

<span data-ttu-id="fa14d-163">继续之前，让我们获取你在本节编写的所有代码并放在新的方法中。</span><span class="sxs-lookup"><span data-stu-id="fa14d-163">Before moving on, let's take all the code you've written in this section and put it in a new method.</span></span> <span data-ttu-id="fa14d-164">调用新方法 `OrderPrecedence`。</span><span class="sxs-lookup"><span data-stu-id="fa14d-164">Call that new method `OrderPrecedence`.</span></span> <span data-ttu-id="fa14d-165">代码应如下所示：</span><span class="sxs-lookup"><span data-stu-id="fa14d-165">Your code should look something like this:</span></span>

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

## <a name="explore-integer-precision-and-limits"></a><span data-ttu-id="fa14d-166">探索整数运算精度和限值</span><span class="sxs-lookup"><span data-stu-id="fa14d-166">Explore integer precision and limits</span></span>

<span data-ttu-id="fa14d-167">在上一个示例中，整数除法截断了结果。</span><span class="sxs-lookup"><span data-stu-id="fa14d-167">That last sample showed you that integer division truncates the result.</span></span> <span data-ttu-id="fa14d-168">可以使用取模  运算符（即 `%` 字符）计算余数  。</span><span class="sxs-lookup"><span data-stu-id="fa14d-168">You can get the **remainder** by using the **modulo** operator, the `%` character.</span></span> <span data-ttu-id="fa14d-169">对 `OrderPrecedence()` 进行方法调用之后，尝试添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa14d-169">Try the following code after the method call to `OrderPrecedence()`:</span></span>

```csharp
int a = 7;
int b = 4;
int c = 3;
int d = (a + b) / c;
int e = (a + b) % c;
Console.WriteLine($"quotient: {d}");
Console.WriteLine($"remainder: {e}");
```

<span data-ttu-id="fa14d-170">C# 整数类型不同于数学上的整数的另一点是，`int` 类型有最小限值和最大限值。</span><span class="sxs-lookup"><span data-stu-id="fa14d-170">The C# integer type differs from mathematical integers in one other way: the `int` type has minimum and maximum limits.</span></span> <span data-ttu-id="fa14d-171">添加以下代码以查看这些限制：</span><span class="sxs-lookup"><span data-stu-id="fa14d-171">Add this code to see those limits:</span></span>

```csharp
int max = int.MaxValue;
int min = int.MinValue;
Console.WriteLine($"The range of integers is {min} to {max}");
```

<span data-ttu-id="fa14d-172">如果运算生成的值超过这些限值，则会出现下溢  或溢出  的情况。</span><span class="sxs-lookup"><span data-stu-id="fa14d-172">If a calculation produces a value that exceeds those limits, you have an **underflow** or **overflow** condition.</span></span> <span data-ttu-id="fa14d-173">答案似乎是从一个限值覆盖到另一个限值的范围。</span><span class="sxs-lookup"><span data-stu-id="fa14d-173">The answer appears to wrap from one limit to the other.</span></span> <span data-ttu-id="fa14d-174">添加以下两行以查看示例：</span><span class="sxs-lookup"><span data-stu-id="fa14d-174">Add these two lines to see an example:</span></span>

```csharp
int what = max + 3;
Console.WriteLine($"An example of overflow: {what}");
```

<span data-ttu-id="fa14d-175">可以看到，答案非常接近最小（负）整数。</span><span class="sxs-lookup"><span data-stu-id="fa14d-175">Notice that the answer is very close to the minimum (negative) integer.</span></span> <span data-ttu-id="fa14d-176">与 `min + 2` 相同。</span><span class="sxs-lookup"><span data-stu-id="fa14d-176">It's the same as `min + 2`.</span></span> <span data-ttu-id="fa14d-177">加法运算会让整数溢出允许的值。</span><span class="sxs-lookup"><span data-stu-id="fa14d-177">The addition operation **overflowed** the allowed values for integers.</span></span> <span data-ttu-id="fa14d-178">答案是一个非常大的负数，因为溢出从最大整数值覆盖回最小整数值。</span><span class="sxs-lookup"><span data-stu-id="fa14d-178">The answer is a very large negative number because an overflow "wraps around" from the largest possible integer value to the smallest.</span></span>

<span data-ttu-id="fa14d-179">如果 `int` 类型无法满足需求，还会用到限值和精度不同的其他数字类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-179">There are other numeric types with different limits and precision that you would use when the `int` type doesn't meet your needs.</span></span> <span data-ttu-id="fa14d-180">接下来，让我们来研究一下其他类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-180">Let's explore those other types next.</span></span> <span data-ttu-id="fa14d-181">在开始下一节内容之前，将你在本节编写的代码移到一个单独的方法中。</span><span class="sxs-lookup"><span data-stu-id="fa14d-181">Before you start the next section, move the code you wrote in this section into a separate method.</span></span> <span data-ttu-id="fa14d-182">将其命名为 `TestLimits`。</span><span class="sxs-lookup"><span data-stu-id="fa14d-182">Name it `TestLimits`.</span></span>

## <a name="work-with-the-double-type"></a><span data-ttu-id="fa14d-183">使用双精度类型</span><span class="sxs-lookup"><span data-stu-id="fa14d-183">Work with the double type</span></span>

<span data-ttu-id="fa14d-184">`double` 数字类型表示双精度浮点数。</span><span class="sxs-lookup"><span data-stu-id="fa14d-184">The `double` numeric type represents a double-precision floating point number.</span></span> <span data-ttu-id="fa14d-185">这些词可能是第一次听说。</span><span class="sxs-lookup"><span data-stu-id="fa14d-185">Those terms may be new to you.</span></span> <span data-ttu-id="fa14d-186">浮点  数可用于表示数量级可能非常大或非常小的非整数。</span><span class="sxs-lookup"><span data-stu-id="fa14d-186">A **floating point** number is useful to represent non-integral numbers that may be very large or small in magnitude.</span></span> <span data-ttu-id="fa14d-187">双精度  是一个相对术语，描述用于存储值的二进制数位数。</span><span class="sxs-lookup"><span data-stu-id="fa14d-187">**Double-precision** is a relative term that describes the number of binary digits used to store the value.</span></span> <span data-ttu-id="fa14d-188">双精度  数字的二进制数位数是单精度  的两倍。</span><span class="sxs-lookup"><span data-stu-id="fa14d-188">**Double precision** numbers have twice the number of binary digits as **single-precision**.</span></span> <span data-ttu-id="fa14d-189">在新式计算机上，使用双精度数字比使用单精度数字更为常见。</span><span class="sxs-lookup"><span data-stu-id="fa14d-189">On modern computers, it's more common to use double precision than single precision numbers.</span></span> <span data-ttu-id="fa14d-190">单精度  数字是使用 `float` 关键字声明的。</span><span class="sxs-lookup"><span data-stu-id="fa14d-190">**Single precision** numbers are declared using the `float` keyword.</span></span> <span data-ttu-id="fa14d-191">接下来，将探索双精度类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-191">Let's explore.</span></span> <span data-ttu-id="fa14d-192">添加以下代码并查看结果：</span><span class="sxs-lookup"><span data-stu-id="fa14d-192">Add the following code and see the result:</span></span>

```csharp
double a = 5;
double b = 4;
double c = 2;
double d = (a + b) / c;
Console.WriteLine(d);
```

<span data-ttu-id="fa14d-193">可以看到，答案商包含小数部分。</span><span class="sxs-lookup"><span data-stu-id="fa14d-193">Notice that the answer includes the decimal portion of the quotient.</span></span> <span data-ttu-id="fa14d-194">试试对双精度类型使用更复杂一点的表达式：</span><span class="sxs-lookup"><span data-stu-id="fa14d-194">Try a slightly more complicated expression with doubles:</span></span>

```csharp
double e = 19;
double f = 23;
double g = 8;
double h = (e + f) / g;
Console.WriteLine(h);
```

<span data-ttu-id="fa14d-195">双精度值的范围远大于整数值。</span><span class="sxs-lookup"><span data-stu-id="fa14d-195">The range of a double value is much greater than integer values.</span></span> <span data-ttu-id="fa14d-196">在当前已编写的代码下方试运行以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa14d-196">Try the following code below what you've written so far:</span></span>

```csharp
double max = double.MaxValue;
double min = double.MinValue;
Console.WriteLine($"The range of double is {min} to {max}");
```

<span data-ttu-id="fa14d-197">打印的这些值用科学记数法表示。</span><span class="sxs-lookup"><span data-stu-id="fa14d-197">These values are printed in scientific notation.</span></span> <span data-ttu-id="fa14d-198">`E` 左侧为有效数字。</span><span class="sxs-lookup"><span data-stu-id="fa14d-198">The number to the left of the `E` is the significand.</span></span> <span data-ttu-id="fa14d-199">右侧为是 10 的 n 次幂。</span><span class="sxs-lookup"><span data-stu-id="fa14d-199">The number to the right is the exponent, as a power of 10.</span></span> <span data-ttu-id="fa14d-200">与数学上的十进制数字一样，C# 中的双精度值可能会有四舍五入误差。</span><span class="sxs-lookup"><span data-stu-id="fa14d-200">Just like decimal numbers in math, doubles in C# can have rounding errors.</span></span> <span data-ttu-id="fa14d-201">试运行以下代码：</span><span class="sxs-lookup"><span data-stu-id="fa14d-201">Try this code:</span></span>

```csharp
double third = 1.0 / 3.0;
Console.WriteLine(third);
```

<span data-ttu-id="fa14d-202">众所周知，`0.3` 循环小数与 `1/3` 并不完全相等。</span><span class="sxs-lookup"><span data-stu-id="fa14d-202">You know that `0.3` repeating isn't exactly the same as `1/3`.</span></span>

<span data-ttu-id="fa14d-203">***挑战***</span><span class="sxs-lookup"><span data-stu-id="fa14d-203">***Challenge***</span></span>

<span data-ttu-id="fa14d-204">尝试使用 `double` 类型执行其他的大小数、乘法和除法运算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-204">Try other calculations with large numbers, small numbers, multiplication, and division using the `double` type.</span></span> <span data-ttu-id="fa14d-205">尝试执行更复杂的运算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-205">Try more complicated calculations.</span></span> <span data-ttu-id="fa14d-206">花了一些时间应对挑战之后，获取已编写的代码并放在一个新方法中。</span><span class="sxs-lookup"><span data-stu-id="fa14d-206">After you've spent some time with the challenge, take the code you've written and place it in a new method.</span></span> <span data-ttu-id="fa14d-207">将新方法命名为 `WorkWithDoubles`。</span><span class="sxs-lookup"><span data-stu-id="fa14d-207">Name that new method `WorkWithDoubles`.</span></span>

## <a name="work-with-decimal-types"></a><span data-ttu-id="fa14d-208">使用十进制类型</span><span class="sxs-lookup"><span data-stu-id="fa14d-208">Work with decimal types</span></span>

<span data-ttu-id="fa14d-209">大家已学习了 C# 中的基本数字类型，即整数和双精度。</span><span class="sxs-lookup"><span data-stu-id="fa14d-209">You've seen the basic numeric types in C#: integers and doubles.</span></span> <span data-ttu-id="fa14d-210">还需要学习另一种类型，即 `decimal` 类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-210">There's one other type to learn: the `decimal` type.</span></span> <span data-ttu-id="fa14d-211">`decimal` 类型的范围较小，但精度高于 `double`。</span><span class="sxs-lookup"><span data-stu-id="fa14d-211">The `decimal` type has a smaller range but greater precision than `double`.</span></span> <span data-ttu-id="fa14d-212">让我们来实际操作一下：</span><span class="sxs-lookup"><span data-stu-id="fa14d-212">Let's take a look:</span></span>

```csharp
decimal min = decimal.MinValue;
decimal max = decimal.MaxValue;
Console.WriteLine($"The range of the decimal type is {min} to {max}");
```

<span data-ttu-id="fa14d-213">可以看到，范围小于 `double` 类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-213">Notice that the range is smaller than the `double` type.</span></span> <span data-ttu-id="fa14d-214">通过试运行以下代码，可以看到十进制类型的精度更高：</span><span class="sxs-lookup"><span data-stu-id="fa14d-214">You can see the greater precision with the decimal type by trying the following code:</span></span>

```csharp
double a = 1.0;
double b = 3.0;
Console.WriteLine(a / b);

decimal c = 1.0M;
decimal d = 3.0M;
Console.WriteLine(c / d);
```

<span data-ttu-id="fa14d-215">数字中的 `M` 后缀指明了常数应如何使用 `decimal` 类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-215">The `M` suffix on the numbers is how you indicate that a constant should use the `decimal` type.</span></span> <span data-ttu-id="fa14d-216">否则，编译器假定为 `double` 类型。</span><span class="sxs-lookup"><span data-stu-id="fa14d-216">Otherwise, the compiler assumes the `double` type.</span></span>

> [!NOTE]
> <span data-ttu-id="fa14d-217">字母 `M` 被选为 `double` 与 `decimal` 关键字之间最明显不同的字母。</span><span class="sxs-lookup"><span data-stu-id="fa14d-217">The letter `M` was chosen as the most visually distinct letter between the `double` and `decimal` keywords.</span></span>

<span data-ttu-id="fa14d-218">可以看到，使用十进制类型执行数学运算时，十进制小数点右侧的数字更多。</span><span class="sxs-lookup"><span data-stu-id="fa14d-218">Notice that the math using the decimal type has more digits to the right of the decimal point.</span></span>

<span data-ttu-id="fa14d-219">***挑战***</span><span class="sxs-lookup"><span data-stu-id="fa14d-219">***Challenge***</span></span>

<span data-ttu-id="fa14d-220">至此，大家已了解不同的数字类型。请编写代码来计算圆面积（其中，半径为 2.50 厘米）。</span><span class="sxs-lookup"><span data-stu-id="fa14d-220">Now that you've seen the different numeric types, write code that calculates the area of a circle whose radius is 2.50 centimeters.</span></span> <span data-ttu-id="fa14d-221">请注意，圆面积是用半径的平方乘以 PI 进行计算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-221">Remember that the area of a circle is the radius squared multiplied by PI.</span></span> <span data-ttu-id="fa14d-222">小提示：.NET 包含 PI 常数 <xref:System.Math.PI?displayProperty=nameWithType>，可用于相应的值计算。</span><span class="sxs-lookup"><span data-stu-id="fa14d-222">One hint: .NET contains a constant for PI, <xref:System.Math.PI?displayProperty=nameWithType> that you can use for that value.</span></span> <span data-ttu-id="fa14d-223">与 `System.Math` 命名空间中声明的所有常量一样，<xref:System.Math.PI?displayProperty=nameWithType> 也是 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="fa14d-223"><xref:System.Math.PI?displayProperty=nameWithType>, like all constants declared in the `System.Math` namespace, is a `double` value.</span></span> <span data-ttu-id="fa14d-224">因此，应使用 `double`（而不是 `decimal`）值来完成这项挑战。</span><span class="sxs-lookup"><span data-stu-id="fa14d-224">For that reason, you should use `double` instead of `decimal` values for this challenge.</span></span>

<span data-ttu-id="fa14d-225">你应获得 19 和 20 之间的答案。</span><span class="sxs-lookup"><span data-stu-id="fa14d-225">You should get an answer between 19 and 20.</span></span> <span data-ttu-id="fa14d-226">要查看你的答案，可以[查看 GitHub 上的完成示例代码](https://github.com/dotnet/samples/tree/main/csharp/numbers-quickstart/Program.cs#L9-L11)。</span><span class="sxs-lookup"><span data-stu-id="fa14d-226">You can check your answer by [looking at the finished sample code on GitHub](https://github.com/dotnet/samples/tree/main/csharp/numbers-quickstart/Program.cs#L9-L11).</span></span>

<span data-ttu-id="fa14d-227">如果需要，可以试用一些其他公式。</span><span class="sxs-lookup"><span data-stu-id="fa14d-227">Try some other formulas if you'd like.</span></span>

<span data-ttu-id="fa14d-228">已完成“C# 中的数字”快速入门教程。</span><span class="sxs-lookup"><span data-stu-id="fa14d-228">You've completed the "Numbers in C#" quickstart.</span></span> <span data-ttu-id="fa14d-229">可以在自己的开发环境中继续学习[分支和循环](branches-and-loops-local.md)快速入门教程。</span><span class="sxs-lookup"><span data-stu-id="fa14d-229">You can continue with the [Branches and loops](branches-and-loops-local.md) quickstart in your own development environment.</span></span>

<span data-ttu-id="fa14d-230">若要详细了解 C# 中的数字，可以参阅下面的文章：</span><span class="sxs-lookup"><span data-stu-id="fa14d-230">You can learn more about numbers in C# in the following articles:</span></span>

- [<span data-ttu-id="fa14d-231">整型数值类型</span><span class="sxs-lookup"><span data-stu-id="fa14d-231">Integral numeric types</span></span>](../../language-reference/builtin-types/integral-numeric-types.md)
- [<span data-ttu-id="fa14d-232">浮点型数值类型</span><span class="sxs-lookup"><span data-stu-id="fa14d-232">Floating-point numeric types</span></span>](../../language-reference/builtin-types/floating-point-numeric-types.md)
- [<span data-ttu-id="fa14d-233">内置数值转换</span><span class="sxs-lookup"><span data-stu-id="fa14d-233">Built-in numeric conversions</span></span>](../../language-reference/builtin-types/numeric-conversions.md)
