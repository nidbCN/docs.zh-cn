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
# <a name="learn-conditional-logic-with-branch-and-loop-statements"></a><span data-ttu-id="53a4f-103">通过分支和循环语句了解条件逻辑</span><span class="sxs-lookup"><span data-stu-id="53a4f-103">Learn conditional logic with branch and loop statements</span></span>

<span data-ttu-id="53a4f-104">本教程介绍了如何编写代码，从而检查变量，并根据这些变量更改执行路径。</span><span class="sxs-lookup"><span data-stu-id="53a4f-104">This tutorial teaches you how to write code that examines variables and changes the execution path based on those variables.</span></span> <span data-ttu-id="53a4f-105">读者可以编写 C# 代码并查看代码编译和运行结果。</span><span class="sxs-lookup"><span data-stu-id="53a4f-105">You write C# code and see the results of compiling and running it.</span></span> <span data-ttu-id="53a4f-106">本教程包含一系列课程，介绍了 C# 中的分支和循环构造。</span><span class="sxs-lookup"><span data-stu-id="53a4f-106">The tutorial contains a series of lessons that explore branching and looping constructs in C#.</span></span> <span data-ttu-id="53a4f-107">这些课程介绍了 C# 语言的基础知识。</span><span class="sxs-lookup"><span data-stu-id="53a4f-107">These lessons teach you the fundamentals of the C# language.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="53a4f-108">先决条件</span><span class="sxs-lookup"><span data-stu-id="53a4f-108">Prerequisites</span></span>

<span data-ttu-id="53a4f-109">本教程要求安装一台虚拟机，以用于本地开发。</span><span class="sxs-lookup"><span data-stu-id="53a4f-109">The tutorial expects that you have a machine set up for local development.</span></span> <span data-ttu-id="53a4f-110">在 Windows、Linux 或 macOS 上，可以使用 .NET CLI 创建、生成和运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="53a4f-110">On Windows, Linux, or macOS, you can use the .NET CLI to create, build, and run applications.</span></span> <span data-ttu-id="53a4f-111">在 Mac 和 Windows 上，可以使用 Visual Studio 2019。</span><span class="sxs-lookup"><span data-stu-id="53a4f-111">On Mac and Windows, you can use Visual Studio 2019.</span></span> <span data-ttu-id="53a4f-112">有关创建说明，请参阅[创建本地环境](local-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="53a4f-112">For setup instructions, see [Set up your local environment](local-environment.md).</span></span>

## <a name="make-decisions-using-the-if-statement"></a><span data-ttu-id="53a4f-113">使用 `if` 语句做出决定</span><span class="sxs-lookup"><span data-stu-id="53a4f-113">Make decisions using the `if` statement</span></span>

<span data-ttu-id="53a4f-114">创建名为 branches-tutorial 的目录。</span><span class="sxs-lookup"><span data-stu-id="53a4f-114">Create a directory named *branches-tutorial*.</span></span> <span data-ttu-id="53a4f-115">将其设为当前目录并运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="53a4f-115">Make that the current directory and run the following command:</span></span>

```dotnetcli
dotnet new console -n BranchesAndLoops -o .
```

<span data-ttu-id="53a4f-116">此命令在当前目录中新建一个 .NET 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="53a4f-116">This command creates a new .NET console application in the current directory.</span></span> <span data-ttu-id="53a4f-117">在常用编辑器中，打开 Program.cs，并将内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="53a4f-117">Open *Program.cs* in your favorite editor, and replace the contents with the following code:</span></span>

```csharp
using System;

int a = 5;
int b = 6;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10.");
```

<span data-ttu-id="53a4f-118">通过在控制台窗口键入 `dotnet run` 试运行此代码。</span><span class="sxs-lookup"><span data-stu-id="53a4f-118">Try this code by typing `dotnet run` in your console window.</span></span> <span data-ttu-id="53a4f-119">你应在控制台上看到以下消息：“The answer is greater than 10.</span><span class="sxs-lookup"><span data-stu-id="53a4f-119">You should see the message "The answer is greater than 10."</span></span> <span data-ttu-id="53a4f-120">（答案大于 10）”。</span><span class="sxs-lookup"><span data-stu-id="53a4f-120">printed to your console.</span></span> <span data-ttu-id="53a4f-121">修改 `b` 的声明，让总和小于 10：</span><span class="sxs-lookup"><span data-stu-id="53a4f-121">Modify the declaration of `b` so that the sum is less than 10:</span></span>

```csharp
int b = 3;
```

<span data-ttu-id="53a4f-122">再次键入 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="53a4f-122">Type `dotnet run` again.</span></span> <span data-ttu-id="53a4f-123">由于答案小于 10，因此什么也没有打印出来。</span><span class="sxs-lookup"><span data-stu-id="53a4f-123">Because the answer is less than 10, nothing is printed.</span></span> <span data-ttu-id="53a4f-124">要测试的条件为 false。</span><span class="sxs-lookup"><span data-stu-id="53a4f-124">The **condition** you're testing is false.</span></span> <span data-ttu-id="53a4f-125">没有任何可供执行的代码，因为仅为 `if` 语句编写了一个可能分支，即 true 分支。</span><span class="sxs-lookup"><span data-stu-id="53a4f-125">You don't have any code to execute because you've only written one of the possible branches for an `if` statement: the true branch.</span></span>

> [!TIP]
> <span data-ttu-id="53a4f-126">在探索 C#（或任何编程语言）的过程中，可能会在编写代码时犯错。</span><span class="sxs-lookup"><span data-stu-id="53a4f-126">As you explore C# (or any programming language), you'll make mistakes when you write code.</span></span> <span data-ttu-id="53a4f-127">编译器会发现并报告这些错误。</span><span class="sxs-lookup"><span data-stu-id="53a4f-127">The compiler will find and report the errors.</span></span> <span data-ttu-id="53a4f-128">仔细查看错误输出和生成错误的代码。</span><span class="sxs-lookup"><span data-stu-id="53a4f-128">Look closely at the error output and the code that generated the error.</span></span> <span data-ttu-id="53a4f-129">编译器错误通常可帮助你找出问题。</span><span class="sxs-lookup"><span data-stu-id="53a4f-129">The compiler error can usually help you find the problem.</span></span>

<span data-ttu-id="53a4f-130">第一个示例展示了 `if` 和布尔类型的用途。</span><span class="sxs-lookup"><span data-stu-id="53a4f-130">This first sample shows the power of `if` and Boolean types.</span></span> <span data-ttu-id="53a4f-131">布尔变量可以包含下列两个值之一：`true` 或 `false`。</span><span class="sxs-lookup"><span data-stu-id="53a4f-131">A *Boolean* is a variable that can have one of two values: `true` or `false`.</span></span> <span data-ttu-id="53a4f-132">C# 为布尔变量定义了特殊类型 `bool`。</span><span class="sxs-lookup"><span data-stu-id="53a4f-132">C# defines a special type, `bool` for Boolean variables.</span></span> <span data-ttu-id="53a4f-133">`if` 语句检查 `bool` 的值。</span><span class="sxs-lookup"><span data-stu-id="53a4f-133">The `if` statement checks the value of a `bool`.</span></span> <span data-ttu-id="53a4f-134">如果值为 `true`，执行 `if` 后面的语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-134">When the value is `true`, the statement following the `if` executes.</span></span> <span data-ttu-id="53a4f-135">否则，跳过这些语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-135">Otherwise, it's skipped.</span></span> <span data-ttu-id="53a4f-136">这种检查条件并根据条件执行语句的过程非常强大。</span><span class="sxs-lookup"><span data-stu-id="53a4f-136">This process of checking conditions and executing statements based on those conditions is powerful.</span></span>

## <a name="make-if-and-else-work-together"></a><span data-ttu-id="53a4f-137">让 if 和 else 完美配合</span><span class="sxs-lookup"><span data-stu-id="53a4f-137">Make if and else work together</span></span>

<span data-ttu-id="53a4f-138">若要执行 true 和 false 分支中的不同代码，请创建在条件为 false 时执行的 `else` 分支。</span><span class="sxs-lookup"><span data-stu-id="53a4f-138">To execute different code in both the true and false branches, you create an `else` branch that executes when the condition is false.</span></span> <span data-ttu-id="53a4f-139">尝试使用 `else` 分支。</span><span class="sxs-lookup"><span data-stu-id="53a4f-139">Try an `else` branch.</span></span> <span data-ttu-id="53a4f-140">添加以下代码中的最后两行（你应该已经有前四行）：</span><span class="sxs-lookup"><span data-stu-id="53a4f-140">Add the last two lines in the code below (you should already have the first four):</span></span>

```csharp
int a = 5;
int b = 3;
if (a + b > 10)
    Console.WriteLine("The answer is greater than 10");
else
    Console.WriteLine("The answer is not greater than 10");
```

<span data-ttu-id="53a4f-141">只有当条件的测试结果为 `false` 时，才执行 `else` 关键字后面的语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-141">The statement following the `else` keyword executes only when the condition being tested is `false`.</span></span> <span data-ttu-id="53a4f-142">将 `if`、`else` 与布尔条件相结合，可以满足处理 `true` 和 `false` 所需的一切要求。</span><span class="sxs-lookup"><span data-stu-id="53a4f-142">Combining `if` and `else` with Boolean conditions provides all the power you need to handle both a `true` and a `false` condition.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53a4f-143">`if` 和 `else` 语句下的缩进是为了方便读者阅读。</span><span class="sxs-lookup"><span data-stu-id="53a4f-143">The indentation under the `if` and `else` statements is for human readers.</span></span> <span data-ttu-id="53a4f-144">C# 语言忽略缩进或空格。</span><span class="sxs-lookup"><span data-stu-id="53a4f-144">The C# language doesn't treat indentation or white space as significant.</span></span> <span data-ttu-id="53a4f-145">`if` 或 `else` 关键字后面的语句根据条件决定是否执行。</span><span class="sxs-lookup"><span data-stu-id="53a4f-145">The statement following the `if` or `else` keyword will be executed based on the condition.</span></span> <span data-ttu-id="53a4f-146">本教程中的所有示例都遵循了常见做法，根据语句的控制流缩进代码行。</span><span class="sxs-lookup"><span data-stu-id="53a4f-146">All the samples in this tutorial follow a common practice to indent lines based on the control flow of statements.</span></span>

<span data-ttu-id="53a4f-147">由于缩进会被忽略，因此需要使用 `{` 和 `}` 指明何时要在根据条件决定是否执行的代码块中添加多个语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-147">Because indentation isn't significant, you need to use `{` and `}` to indicate when you want more than one statement to be part of the block that executes conditionally.</span></span> <span data-ttu-id="53a4f-148">C# 程序员通常会对所有 `if` 和 `else` 子句使用这些大括号。</span><span class="sxs-lookup"><span data-stu-id="53a4f-148">C# programmers typically use those braces on all `if` and `else` clauses.</span></span> <span data-ttu-id="53a4f-149">以下示例与你创建的示例相同。</span><span class="sxs-lookup"><span data-stu-id="53a4f-149">The following example is the same as the one you created.</span></span> <span data-ttu-id="53a4f-150">修改上面的代码以匹配下面的代码：</span><span class="sxs-lookup"><span data-stu-id="53a4f-150">Modify your code above to match the following code:</span></span>

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
> <span data-ttu-id="53a4f-151">在本教程的其余部分中，代码示例全都遵循公认做法，添加了大括号。</span><span class="sxs-lookup"><span data-stu-id="53a4f-151">Through the rest of this tutorial, the code samples all include the braces, following accepted practices.</span></span>

<span data-ttu-id="53a4f-152">可以测试更复杂的条件。</span><span class="sxs-lookup"><span data-stu-id="53a4f-152">You can test more complicated conditions.</span></span> <span data-ttu-id="53a4f-153">在当前已编写的代码之后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="53a4f-153">Add the following code after the code you've written so far:</span></span>

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

<span data-ttu-id="53a4f-154">`==` 符号执行相等测试。</span><span class="sxs-lookup"><span data-stu-id="53a4f-154">The `==` symbol tests for *equality*.</span></span> <span data-ttu-id="53a4f-155">使用 `==` 将相等测试与赋值测试区分开来，如在 `a = 5` 中所见。</span><span class="sxs-lookup"><span data-stu-id="53a4f-155">Using `==` distinguishes the test for equality from assignment, which you saw in `a = 5`.</span></span>

<span data-ttu-id="53a4f-156">`&&` 表示“且”。</span><span class="sxs-lookup"><span data-stu-id="53a4f-156">The `&&` represents "and".</span></span> <span data-ttu-id="53a4f-157">也就是说，两个条件必须都为 true，才能执行 true 分支中的语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-157">It means both conditions must be true to execute the statement in the true branch.</span></span>  <span data-ttu-id="53a4f-158">这些示例还表明，可以在每个条件分支中添加多个语句，前提是将它们用 `{` 和 `}` 括住。</span><span class="sxs-lookup"><span data-stu-id="53a4f-158">These examples also show that you can have multiple statements in each conditional branch, provided you enclose them in `{` and `}`.</span></span> <span data-ttu-id="53a4f-159">还可以使用 `||` 表示“或”。</span><span class="sxs-lookup"><span data-stu-id="53a4f-159">You can also use  `||` to represent "or".</span></span> <span data-ttu-id="53a4f-160">在当前已编写的代码之后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="53a4f-160">Add the following code after what you've written so far:</span></span>

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

<span data-ttu-id="53a4f-161">修改 `a`、`b` 和 `c` 的值，并在 `&&` 和 `||` 之间切换浏览。</span><span class="sxs-lookup"><span data-stu-id="53a4f-161">Modify the values of `a`, `b`, and `c` and switch between `&&` and `||` to explore.</span></span> <span data-ttu-id="53a4f-162">你将进一步了解 `&&` 和 `||` 运算符的工作原理。</span><span class="sxs-lookup"><span data-stu-id="53a4f-162">You'll gain more understanding of how the `&&` and `||` operators work.</span></span>

<span data-ttu-id="53a4f-163">你已完成第一步。</span><span class="sxs-lookup"><span data-stu-id="53a4f-163">You've finished the first step.</span></span> <span data-ttu-id="53a4f-164">开始进入下一部分前，先将当前代码移到单独的方法中。</span><span class="sxs-lookup"><span data-stu-id="53a4f-164">Before you start the next section, let's move the current code into a separate method.</span></span> <span data-ttu-id="53a4f-165">这样一来，可以更轻松地开始处理新示例。</span><span class="sxs-lookup"><span data-stu-id="53a4f-165">That makes it easier to start working with a new example.</span></span> <span data-ttu-id="53a4f-166">将现有代码放入名为 `ExploreIf()` 的方法中。</span><span class="sxs-lookup"><span data-stu-id="53a4f-166">Put the existing code in a method called `ExploreIf()`.</span></span> <span data-ttu-id="53a4f-167">从程序顶部调用它。</span><span class="sxs-lookup"><span data-stu-id="53a4f-167">Call it from the top of your program.</span></span> <span data-ttu-id="53a4f-168">完成这些更改后，代码看起来应类似下面这样：</span><span class="sxs-lookup"><span data-stu-id="53a4f-168">When you finished those changes, your code should look like the following:</span></span>

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

<span data-ttu-id="53a4f-169">注释掉对 `ExploreIf()` 的调用。</span><span class="sxs-lookup"><span data-stu-id="53a4f-169">Comment out the call to `ExploreIf()`.</span></span> <span data-ttu-id="53a4f-170">在本节中，它会在你工作时使输出变得不那么杂乱：</span><span class="sxs-lookup"><span data-stu-id="53a4f-170">It will make the output less cluttered as you work in this section:</span></span>

```csharp
//ExploreIf();
```

<span data-ttu-id="53a4f-171">`//` 在 C# 中启动 注释。</span><span class="sxs-lookup"><span data-stu-id="53a4f-171">The `//` starts a **comment** in C#.</span></span> <span data-ttu-id="53a4f-172">注释是你想要保留在源代码中但不能作为代码执行的任何文本。</span><span class="sxs-lookup"><span data-stu-id="53a4f-172">Comments are any text you want to keep in your source code but not execute as code.</span></span> <span data-ttu-id="53a4f-173">编译器不会从注释中生成任何可执行代码。</span><span class="sxs-lookup"><span data-stu-id="53a4f-173">The compiler doesn't generate any executable code from comments.</span></span>

## <a name="use-loops-to-repeat-operations"></a><span data-ttu-id="53a4f-174">使用循环重复执行运算</span><span class="sxs-lookup"><span data-stu-id="53a4f-174">Use loops to repeat operations</span></span>

<span data-ttu-id="53a4f-175">在本节使用循环以重复执行语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-175">In this section, you use **loops** to repeat statements.</span></span> <span data-ttu-id="53a4f-176">在调用 `ExploreIf` 后，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="53a4f-176">Add this code after the call to `ExploreIf`:</span></span>

```csharp
int counter = 0;
while (counter < 10)
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
}
```

<span data-ttu-id="53a4f-177">`while` 语句会检查条件，并执行 `while` 后面的语句或语句块。</span><span class="sxs-lookup"><span data-stu-id="53a4f-177">The `while` statement checks a condition and executes the statement or statement block following the `while`.</span></span> <span data-ttu-id="53a4f-178">除非条件为 false，否则它会重复检查条件并执行这些语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-178">It repeatedly checks the condition and executing those statements until the condition is false.</span></span>

<span data-ttu-id="53a4f-179">此示例新引入了另外一个运算符。</span><span class="sxs-lookup"><span data-stu-id="53a4f-179">There's one other new operator in this example.</span></span> <span data-ttu-id="53a4f-180">`counter` 变量后面的 `++` 是增量运算符。</span><span class="sxs-lookup"><span data-stu-id="53a4f-180">The `++` after the `counter` variable is the **increment** operator.</span></span> <span data-ttu-id="53a4f-181">它将 `counter` 值加 1，并将计算后的值存储在 `counter` 变量中。</span><span class="sxs-lookup"><span data-stu-id="53a4f-181">It adds 1 to the value of `counter` and stores that value in the `counter` variable.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="53a4f-182">请确保 `while` 循环条件在你执行代码时更改为 false。</span><span class="sxs-lookup"><span data-stu-id="53a4f-182">Make sure that the `while` loop condition changes to false as you execute the code.</span></span> <span data-ttu-id="53a4f-183">否则，创建的就是无限循环，即程序永不结束。</span><span class="sxs-lookup"><span data-stu-id="53a4f-183">Otherwise, you create an **infinite loop** where your program never ends.</span></span> <span data-ttu-id="53a4f-184">本示例中没有演示上述情况，因为你必须使用 CTRL-C 或其他方法强制退出程序。</span><span class="sxs-lookup"><span data-stu-id="53a4f-184">That is not demonstrated in this sample, because you have to force your program to quit using **CTRL-C** or other means.</span></span>

<span data-ttu-id="53a4f-185">`while` 循环先测试条件，然后再执行 `while` 后面的代码。</span><span class="sxs-lookup"><span data-stu-id="53a4f-185">The `while` loop tests the condition before executing the code following the `while`.</span></span> <span data-ttu-id="53a4f-186">`do` ... `while` 循环先执行代码，然后再检查条件。</span><span class="sxs-lookup"><span data-stu-id="53a4f-186">The `do` ... `while` loop executes the code first, and then checks the condition.</span></span> <span data-ttu-id="53a4f-187">下面的代码对 do while 循环进行了演示：</span><span class="sxs-lookup"><span data-stu-id="53a4f-187">The *do while* loop is shown in the following code:</span></span>

```csharp
int counter = 0;
do
{
    Console.WriteLine($"Hello World! The counter is {counter}");
    counter++;
} while (counter < 10);
```

<span data-ttu-id="53a4f-188">此 `do` 循环和前面的 `while` 循环生成的输出结果相同。</span><span class="sxs-lookup"><span data-stu-id="53a4f-188">This `do` loop and the earlier `while` loop produce the same output.</span></span>

## <a name="work-with-the-for-loop"></a><span data-ttu-id="53a4f-189">使用 for 循环</span><span class="sxs-lookup"><span data-stu-id="53a4f-189">Work with the for loop</span></span>

<span data-ttu-id="53a4f-190">For 循环通常用在 C# 中。</span><span class="sxs-lookup"><span data-stu-id="53a4f-190">The **for** loop is commonly used in C#.</span></span> <span data-ttu-id="53a4f-191">试运行以下代码：</span><span class="sxs-lookup"><span data-stu-id="53a4f-191">Try this code:</span></span>

```csharp
for (int index = 0; index < 10; index++)
{
    Console.WriteLine($"Hello World! The index is {index}");
}
```

<span data-ttu-id="53a4f-192">上述代码的工作原理与已用过的 `while` 循环和 `do` 循环相同。</span><span class="sxs-lookup"><span data-stu-id="53a4f-192">The previous code does the same work as the `while` loop and the `do` loop you've already used.</span></span> <span data-ttu-id="53a4f-193">`for` 语句包含三个控制具体工作方式的部分。</span><span class="sxs-lookup"><span data-stu-id="53a4f-193">The `for` statement has three parts that control how it works.</span></span>

<span data-ttu-id="53a4f-194">第一部分是 for 初始值设定项：`int index = 0;` 声明 `index` 是循环变量，并将它的初始值设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="53a4f-194">The first part is the **for initializer**: `int index = 0;` declares that `index` is the loop variable, and sets its initial value to `0`.</span></span>

<span data-ttu-id="53a4f-195">中间部分是 for 条件：`index < 10` 声明只要计数器值小于 10，此 `for` 循环就会继续执行。</span><span class="sxs-lookup"><span data-stu-id="53a4f-195">The middle part is the **for condition**: `index < 10` declares that this `for` loop continues to execute as long as the value of counter is less than 10.</span></span>

<span data-ttu-id="53a4f-196">最后一部分是 for 迭代器：`index++` 指定在执行 `for` 语句后面的代码块后，如何修改循环变量。</span><span class="sxs-lookup"><span data-stu-id="53a4f-196">The final part is the **for iterator**: `index++` specifies how to modify the loop variable after executing the block following the `for` statement.</span></span> <span data-ttu-id="53a4f-197">在此示例中，它指定 `index` 应在代码块每次执行时递增 1。</span><span class="sxs-lookup"><span data-stu-id="53a4f-197">Here, it specifies that `index` should be incremented by 1 each time the block executes.</span></span>

<span data-ttu-id="53a4f-198">请自行试验。</span><span class="sxs-lookup"><span data-stu-id="53a4f-198">Experiment yourself.</span></span> <span data-ttu-id="53a4f-199">尝试以下每种变化：</span><span class="sxs-lookup"><span data-stu-id="53a4f-199">Try each of the following variations:</span></span>

- <span data-ttu-id="53a4f-200">将初始值设定项更改为其他初始值。</span><span class="sxs-lookup"><span data-stu-id="53a4f-200">Change the initializer to start at a different value.</span></span>
- <span data-ttu-id="53a4f-201">将结束条件设定项更改为其他值。</span><span class="sxs-lookup"><span data-stu-id="53a4f-201">Change the condition to stop at a different value.</span></span>

<span data-ttu-id="53a4f-202">完成后，继续利用所学知识，试着自己编写一些代码。</span><span class="sxs-lookup"><span data-stu-id="53a4f-202">When you're done, let's move on to write some code yourself to use what you've learned.</span></span>

<span data-ttu-id="53a4f-203">本教程中未介绍的另一个循环语句：`foreach` 语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-203">There's one other looping statement that isn't covered in this tutorial: the `foreach` statement.</span></span> <span data-ttu-id="53a4f-204">`foreach` 语句为项序列中的每一项重复其语句。</span><span class="sxs-lookup"><span data-stu-id="53a4f-204">The `foreach` statement repeats its statement for every item in a sequence of items.</span></span> <span data-ttu-id="53a4f-205">它最常用于集合，因此将在下一教程中介绍。</span><span class="sxs-lookup"><span data-stu-id="53a4f-205">It's most often used with *collections*, so it's covered in the next tutorial.</span></span>

## <a name="created-nested-loops"></a><span data-ttu-id="53a4f-206">已创建嵌套循环</span><span class="sxs-lookup"><span data-stu-id="53a4f-206">Created nested loops</span></span>

<span data-ttu-id="53a4f-207">`while`、`do` 或 `for` 循环可以嵌套在另一个循环内，以使用外部循环中的各项与内部循环中的各项组合来创建矩阵。</span><span class="sxs-lookup"><span data-stu-id="53a4f-207">A `while`, `do`, or `for` loop can be nested inside another loop to create a matrix using the combination of each item in the outer loop with each item in the inner loop.</span></span> <span data-ttu-id="53a4f-208">我们来生成一组字母数字对以表示行和列。</span><span class="sxs-lookup"><span data-stu-id="53a4f-208">Let's do that to build a set of alphanumeric pairs to represent rows and columns.</span></span>

<span data-ttu-id="53a4f-209">一个 `for` 循环可以生成行：</span><span class="sxs-lookup"><span data-stu-id="53a4f-209">One `for` loop can generate the rows:</span></span>

```csharp
for (int row = 1; row < 11; row++)
{
    Console.WriteLine($"The row is {row}");
}
```

<span data-ttu-id="53a4f-210">另一个循环可以生成列：</span><span class="sxs-lookup"><span data-stu-id="53a4f-210">Another loop can generate the columns:</span></span>

```csharp
for (char column = 'a'; column < 'k'; column++)
{
    Console.WriteLine($"The column is {column}");
}
```

<span data-ttu-id="53a4f-211">可以将一个循环嵌套在另一个循环中以形成各个对：</span><span class="sxs-lookup"><span data-stu-id="53a4f-211">You can nest one loop inside the other to form pairs:</span></span>

```csharp
for (int row = 1; row < 11; row++)
{
    for (char column = 'a'; column < 'k'; column++)
    {
        Console.WriteLine($"The cell is ({row}, {column})");
    }
}
```

<span data-ttu-id="53a4f-212">可以看到，对于内部循环的每次完整运行，外部循环会递增一次。</span><span class="sxs-lookup"><span data-stu-id="53a4f-212">You can see that the outer loop increments once for each full run of the inner loop.</span></span> <span data-ttu-id="53a4f-213">反转行和列嵌套，自行查看变化。</span><span class="sxs-lookup"><span data-stu-id="53a4f-213">Reverse the row and column nesting, and see the changes for yourself.</span></span> <span data-ttu-id="53a4f-214">完成后，将此部分中的代码放入名为 `ExploreLoops()` 的方法中。</span><span class="sxs-lookup"><span data-stu-id="53a4f-214">When you're done, place the code from this section in a method called `ExploreLoops()`.</span></span>

## <a name="combine-branches-and-loops"></a><span data-ttu-id="53a4f-215">结合使用分支和循环</span><span class="sxs-lookup"><span data-stu-id="53a4f-215">Combine branches and loops</span></span>

<span data-ttu-id="53a4f-216">支持，大家已了解 C# 语言中的 `if` 语句和循环构造。看看能否编写 C# 代码，计算 1 到 20 中所有可被 3 整除的整数的总和。</span><span class="sxs-lookup"><span data-stu-id="53a4f-216">Now that you've seen the `if` statement and the looping constructs in the C# language, see if you can write C# code to find the sum of all integers 1 through 20 that are divisible by 3.</span></span>  <span data-ttu-id="53a4f-217">下面提供了一些提示：</span><span class="sxs-lookup"><span data-stu-id="53a4f-217">Here are a few hints:</span></span>

- <span data-ttu-id="53a4f-218">`%` 运算符可用于获取除法运算的余数。</span><span class="sxs-lookup"><span data-stu-id="53a4f-218">The `%` operator gives you the remainder of a division operation.</span></span>
- <span data-ttu-id="53a4f-219">`if` 语句中的条件可用于判断是否应将数字计入总和。</span><span class="sxs-lookup"><span data-stu-id="53a4f-219">The `if` statement gives you the condition to see if a number should be part of the sum.</span></span>
- <span data-ttu-id="53a4f-220">`for` 循环有助于对 1 到 20 中的所有数字重复执行一系列步骤。</span><span class="sxs-lookup"><span data-stu-id="53a4f-220">The `for` loop can help you repeat a series of steps for all the numbers 1 through 20.</span></span>

<span data-ttu-id="53a4f-221">亲自试一试吧。</span><span class="sxs-lookup"><span data-stu-id="53a4f-221">Try it yourself.</span></span> <span data-ttu-id="53a4f-222">然后，看看自己是怎么做到的。</span><span class="sxs-lookup"><span data-stu-id="53a4f-222">Then check how you did.</span></span> <span data-ttu-id="53a4f-223">你应获取的答案为 63。</span><span class="sxs-lookup"><span data-stu-id="53a4f-223">You should get 63 for an answer.</span></span> <span data-ttu-id="53a4f-224">通过[在 GitHub 上查看已完成的代码](https://github.com/dotnet/samples/blob/main/csharp/branches-quickstart/Program.cs#L87-L95)，你可以看到一个可能的答案。</span><span class="sxs-lookup"><span data-stu-id="53a4f-224">You can see one possible answer by [viewing the completed code on GitHub](https://github.com/dotnet/samples/blob/main/csharp/branches-quickstart/Program.cs#L87-L95).</span></span>

<span data-ttu-id="53a4f-225">已完成“分支和循环”教程。</span><span class="sxs-lookup"><span data-stu-id="53a4f-225">You've completed the "branches and loops" tutorial.</span></span>

<span data-ttu-id="53a4f-226">可以在自己的开发环境中继续学习[数组和集合](arrays-and-collections.md)教程。</span><span class="sxs-lookup"><span data-stu-id="53a4f-226">You can continue with the [Arrays and collections](arrays-and-collections.md) tutorial in your own development environment.</span></span>

<span data-ttu-id="53a4f-227">若要详细了解这些概念，请参阅下列文章：</span><span class="sxs-lookup"><span data-stu-id="53a4f-227">You can learn more about these concepts in these articles:</span></span>

- [<span data-ttu-id="53a4f-228">If 和 else 语句</span><span class="sxs-lookup"><span data-stu-id="53a4f-228">If and else statement</span></span>](../../language-reference/keywords/if-else.md)
- [<span data-ttu-id="53a4f-229">While 语句</span><span class="sxs-lookup"><span data-stu-id="53a4f-229">While statement</span></span>](../../language-reference/keywords/while.md)
- [<span data-ttu-id="53a4f-230">Do 语句</span><span class="sxs-lookup"><span data-stu-id="53a4f-230">Do statement</span></span>](../../language-reference/keywords/do.md)
- [<span data-ttu-id="53a4f-231">For 语句</span><span class="sxs-lookup"><span data-stu-id="53a4f-231">For statement</span></span>](../../language-reference/keywords/for.md)
