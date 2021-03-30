---
title: 控制台应用程序
description: 此教程将介绍 .NET Core 和 C# 语言的许多功能。
ms.date: 03/06/2017
ms.technology: csharp-fundamentals
ms.assetid: 883cd93d-50ce-4144-b7c9-2df28d9c11a0
ms.openlocfilehash: 38b4660a6809ebbfbb079b1c00ad9e9a53341645
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876131"
---
# <a name="console-app"></a><span data-ttu-id="8be71-103">控制台应用</span><span class="sxs-lookup"><span data-stu-id="8be71-103">Console app</span></span>

<span data-ttu-id="8be71-104">此教程将介绍 .NET Core 和 C# 语言的许多功能。</span><span class="sxs-lookup"><span data-stu-id="8be71-104">This tutorial teaches you a number of features in .NET Core and the C# language.</span></span> <span data-ttu-id="8be71-105">你将了解：</span><span class="sxs-lookup"><span data-stu-id="8be71-105">You'll learn:</span></span>

- <span data-ttu-id="8be71-106">.NET Core CLI 的基础知识</span><span class="sxs-lookup"><span data-stu-id="8be71-106">The basics of the .NET Core CLI</span></span>
- <span data-ttu-id="8be71-107">C# 控制台应用程序的结构</span><span class="sxs-lookup"><span data-stu-id="8be71-107">The structure of a C# Console Application</span></span>
- <span data-ttu-id="8be71-108">控制台 I/O</span><span class="sxs-lookup"><span data-stu-id="8be71-108">Console I/O</span></span>
- <span data-ttu-id="8be71-109">.NET 中文件 I/O API 的基础知识</span><span class="sxs-lookup"><span data-stu-id="8be71-109">The basics of File I/O APIs in .NET</span></span>
- <span data-ttu-id="8be71-110">.NET 中基于任务的异步编程基础知识</span><span class="sxs-lookup"><span data-stu-id="8be71-110">The basics of the Task-based Asynchronous Programming in .NET</span></span>

<span data-ttu-id="8be71-111">你将生成一个应用程序，用于读取文本文件，然后将文本文件的内容回显到控制台。</span><span class="sxs-lookup"><span data-stu-id="8be71-111">You'll build an application that reads a text file, and echoes the contents of that text file to the console.</span></span> <span data-ttu-id="8be71-112">按配速大声朗读控制台输出。</span><span class="sxs-lookup"><span data-stu-id="8be71-112">The output to the console is paced to match reading it aloud.</span></span> <span data-ttu-id="8be71-113">可以按“<”（小于）或“>”（大于）键加速或减速显示。</span><span class="sxs-lookup"><span data-stu-id="8be71-113">You can speed up or slow down the pace by pressing the '<' (less than) or '>' (greater than) keys.</span></span>

<span data-ttu-id="8be71-114">此教程将介绍许多功能。</span><span class="sxs-lookup"><span data-stu-id="8be71-114">There are a lot of features in this tutorial.</span></span> <span data-ttu-id="8be71-115">我们将逐个生成这些功能。</span><span class="sxs-lookup"><span data-stu-id="8be71-115">Let's build them one by one.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8be71-116">先决条件</span><span class="sxs-lookup"><span data-stu-id="8be71-116">Prerequisites</span></span>

- <span data-ttu-id="8be71-117">将计算机设置为运行 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="8be71-117">Set up your machine to run .NET Core.</span></span> <span data-ttu-id="8be71-118">有关安装说明，请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页。</span><span class="sxs-lookup"><span data-stu-id="8be71-118">You can find the installation instructions on the [.NET Core downloads](https://dotnet.microsoft.com/download) page.</span></span> <span data-ttu-id="8be71-119">可以在 Windows、Linux、macOS 或 Docker 容器中运行此应用程序。</span><span class="sxs-lookup"><span data-stu-id="8be71-119">You can run this application on Windows, Linux, macOS, or in a Docker container.</span></span>

- <span data-ttu-id="8be71-120">安装最喜爱的代码编辑器。</span><span class="sxs-lookup"><span data-stu-id="8be71-120">Install your favorite code editor.</span></span>

## <a name="create-the-app"></a><span data-ttu-id="8be71-121">创建应用</span><span class="sxs-lookup"><span data-stu-id="8be71-121">Create the app</span></span>

<span data-ttu-id="8be71-122">第一步是新建应用程序。</span><span class="sxs-lookup"><span data-stu-id="8be71-122">The first step is to create a new application.</span></span> <span data-ttu-id="8be71-123">打开命令提示符，然后新建应用程序的目录。</span><span class="sxs-lookup"><span data-stu-id="8be71-123">Open a command prompt and create a new directory for your application.</span></span> <span data-ttu-id="8be71-124">将新建的目录设为当前目录。</span><span class="sxs-lookup"><span data-stu-id="8be71-124">Make that the current directory.</span></span> <span data-ttu-id="8be71-125">在命令提示符处，键入命令 `dotnet new console`。</span><span class="sxs-lookup"><span data-stu-id="8be71-125">Type the command `dotnet new console` at the command prompt.</span></span> <span data-ttu-id="8be71-126">这将为基本的“Hello World”应用程序创建起始文件。</span><span class="sxs-lookup"><span data-stu-id="8be71-126">This creates the starter files for a basic "Hello World" application.</span></span>

<span data-ttu-id="8be71-127">在开始进行修改之前，我们先来逐步了解一下如何运行简单的 Hello World 应用程序。</span><span class="sxs-lookup"><span data-stu-id="8be71-127">Before you start making modifications, let's go through the steps to run the simple Hello World application.</span></span> <span data-ttu-id="8be71-128">创建应用程序之后，在命令提示符处键入 `dotnet restore`。</span><span class="sxs-lookup"><span data-stu-id="8be71-128">After creating the application, type `dotnet restore` at the command prompt.</span></span> <span data-ttu-id="8be71-129">此命令将运行 NuGet 包还原进程。</span><span class="sxs-lookup"><span data-stu-id="8be71-129">This command runs the NuGet package restore process.</span></span> <span data-ttu-id="8be71-130">NuGet 是 .NET 程序包管理器。</span><span class="sxs-lookup"><span data-stu-id="8be71-130">NuGet is a .NET package manager.</span></span> <span data-ttu-id="8be71-131">此命令会下载项目缺少的所有依赖项。</span><span class="sxs-lookup"><span data-stu-id="8be71-131">This command downloads any of the missing dependencies for your project.</span></span> <span data-ttu-id="8be71-132">由于这是一个新项目，尚无任何依赖项，因此首次运行只会下载 .NET Core 框架。</span><span class="sxs-lookup"><span data-stu-id="8be71-132">As this is a new project, none of the dependencies are in place, so the first run will download the .NET Core framework.</span></span> <span data-ttu-id="8be71-133">执行这一初始步骤后，只需运行 `dotnet restore`，即可添加新的依赖项包，或更新任意依赖项的版本。</span><span class="sxs-lookup"><span data-stu-id="8be71-133">After this initial step, you will only need to run `dotnet restore` when you add new dependent packages, or update the versions of any of your dependencies.</span></span>

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

<span data-ttu-id="8be71-134">还原包后，运行 `dotnet build`。</span><span class="sxs-lookup"><span data-stu-id="8be71-134">After restoring packages, you run `dotnet build`.</span></span> <span data-ttu-id="8be71-135">这将运行生成引擎，并创建应用程序可执行文件。</span><span class="sxs-lookup"><span data-stu-id="8be71-135">This executes the build engine and creates your application executable.</span></span> <span data-ttu-id="8be71-136">最后，执行 `dotnet run` 来运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="8be71-136">Finally, you execute `dotnet run` to run your application.</span></span>

<span data-ttu-id="8be71-137">简单的 Hello World 应用程序代码全都在 Program.cs 中。</span><span class="sxs-lookup"><span data-stu-id="8be71-137">The simple Hello World application code is all in Program.cs.</span></span> <span data-ttu-id="8be71-138">使用常用文本编辑器打开此文件。</span><span class="sxs-lookup"><span data-stu-id="8be71-138">Open that file with your favorite text editor.</span></span> <span data-ttu-id="8be71-139">我们将执行首轮更改。</span><span class="sxs-lookup"><span data-stu-id="8be71-139">We're about to make our first changes.</span></span> <span data-ttu-id="8be71-140">在此文件的最上面，你会看到 using 语句：</span><span class="sxs-lookup"><span data-stu-id="8be71-140">At the top of the file, see a using statement:</span></span>

```csharp
using System;
```

<span data-ttu-id="8be71-141">此语句指示编译器，`System` 命名空间中的任何类型都在范围内。</span><span class="sxs-lookup"><span data-stu-id="8be71-141">This statement tells the compiler that any types from the `System` namespace are in scope.</span></span> <span data-ttu-id="8be71-142">与你可能用过的其他面向对象的语言一样，C# 也使用命名空间来整理类型。</span><span class="sxs-lookup"><span data-stu-id="8be71-142">Like other Object Oriented languages you may have used, C# uses namespaces to organize types.</span></span> <span data-ttu-id="8be71-143">此 Hello World 程序也一样。</span><span class="sxs-lookup"><span data-stu-id="8be71-143">This Hello World program is no different.</span></span> <span data-ttu-id="8be71-144">你可以看到，此程序封闭在名称基于当前目录名的命名空间内。</span><span class="sxs-lookup"><span data-stu-id="8be71-144">You can see that the program is enclosed in the namespace with the name based on the name of the current directory.</span></span> <span data-ttu-id="8be71-145">对于本教程，我们将命名空间的名称更改为 `TeleprompterConsole`：</span><span class="sxs-lookup"><span data-stu-id="8be71-145">For this tutorial, let's change the name of the namespace to `TeleprompterConsole`:</span></span>

```csharp
namespace TeleprompterConsole
```

## <a name="reading-and-echoing-the-file"></a><span data-ttu-id="8be71-146">读取和回显文件</span><span class="sxs-lookup"><span data-stu-id="8be71-146">Reading and Echoing the File</span></span>

<span data-ttu-id="8be71-147">要添加的第一项功能是读取文本文件，然后在控制台中显示全部文本。</span><span class="sxs-lookup"><span data-stu-id="8be71-147">The first feature to add is the ability to read a text file and display all that text to the console.</span></span> <span data-ttu-id="8be71-148">首先，让我们来添加文本文件。</span><span class="sxs-lookup"><span data-stu-id="8be71-148">First, let's add a text file.</span></span> <span data-ttu-id="8be71-149">将此[示例](https://github.com/dotnet/samples/tree/main/csharp/getting-started/console-teleprompter)的 GitHub 存储库中的 [sampleQuotes.txt](https://github.com/dotnet/samples/raw/main/csharp/getting-started/console-teleprompter/sampleQuotes.txt) 文件复制到项目目录中。</span><span class="sxs-lookup"><span data-stu-id="8be71-149">Copy the [sampleQuotes.txt](https://github.com/dotnet/samples/raw/main/csharp/getting-started/console-teleprompter/sampleQuotes.txt) file from the GitHub repository for this [sample](https://github.com/dotnet/samples/tree/main/csharp/getting-started/console-teleprompter) into your project directory.</span></span> <span data-ttu-id="8be71-150">这将用作应用程序脚本。</span><span class="sxs-lookup"><span data-stu-id="8be71-150">This will serve as the script for your application.</span></span> <span data-ttu-id="8be71-151">如果需要有关如何下载本主题示例应用的信息，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)主题中的说明。</span><span class="sxs-lookup"><span data-stu-id="8be71-151">If you would like information on how to download the sample app for this topic, see the instructions in the [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples) topic.</span></span>

<span data-ttu-id="8be71-152">接下来，在 `Program` 类中添加以下方法（即 `Main` 方法的下方）：</span><span class="sxs-lookup"><span data-stu-id="8be71-152">Next, add the following method in your `Program` class (right below the `Main` method):</span></span>

```csharp
static IEnumerable<string> ReadFrom(string file)
{
    string line;
    using (var reader = File.OpenText(file))
    {
        while ((line = reader.ReadLine()) != null)
        {
            yield return line;
        }
    }
}
```

<span data-ttu-id="8be71-153">此方法使用两个新命名空间中的类型。</span><span class="sxs-lookup"><span data-stu-id="8be71-153">This method uses types from two new namespaces.</span></span> <span data-ttu-id="8be71-154">为了让编译能够顺利进行，需要在文件的最上面添加以下两行代码：</span><span class="sxs-lookup"><span data-stu-id="8be71-154">For this to compile you'll need to add the following two lines to the top of the file:</span></span>

```csharp
using System.Collections.Generic;
using System.IO;
```

<span data-ttu-id="8be71-155"><xref:System.Collections.Generic.IEnumerable%601> 接口是在 <xref:System.Collections.Generic> 命名空间中进行定义。</span><span class="sxs-lookup"><span data-stu-id="8be71-155">The <xref:System.Collections.Generic.IEnumerable%601> interface is defined in the <xref:System.Collections.Generic> namespace.</span></span> <span data-ttu-id="8be71-156"><xref:System.IO.File> 类是在 <xref:System.IO> 命名空间中进行定义。</span><span class="sxs-lookup"><span data-stu-id="8be71-156">The <xref:System.IO.File> class is defined in the <xref:System.IO> namespace.</span></span>

<span data-ttu-id="8be71-157">这是一种称为“Iterator 方法”的特殊类型 C# 方法。</span><span class="sxs-lookup"><span data-stu-id="8be71-157">This method is a special type of C# method called an *Iterator method*.</span></span> <span data-ttu-id="8be71-158">枚举器方法返回延迟计算的序列。</span><span class="sxs-lookup"><span data-stu-id="8be71-158">Enumerator methods return sequences that are evaluated lazily.</span></span> <span data-ttu-id="8be71-159">也就是说，序列中的每一项是在使用序列的代码提出请求时生成。</span><span class="sxs-lookup"><span data-stu-id="8be71-159">That means each item in the sequence is generated as it is requested by the code consuming the sequence.</span></span> <span data-ttu-id="8be71-160">Enumerator 方法包含一个或多个 [`yield return`](../language-reference/keywords/yield.md) 语句。</span><span class="sxs-lookup"><span data-stu-id="8be71-160">Enumerator methods are methods that contain one or more [`yield return`](../language-reference/keywords/yield.md) statements.</span></span> <span data-ttu-id="8be71-161">`ReadFrom` 方法返回的对象包含用于生成序列中所有项的代码。</span><span class="sxs-lookup"><span data-stu-id="8be71-161">The object returned by the `ReadFrom` method contains the code to generate each item in the sequence.</span></span> <span data-ttu-id="8be71-162">在此示例中，这涉及读取源文件中的下一行文本，然后返回相应的字符串。</span><span class="sxs-lookup"><span data-stu-id="8be71-162">In this example, that involves reading the next line of text from the source file, and returning that string.</span></span> <span data-ttu-id="8be71-163">每当调用代码请求生成序列中的下一项时，代码就会读取并返回文件中的下一行文本。</span><span class="sxs-lookup"><span data-stu-id="8be71-163">Each time the calling code requests the next item from the sequence, the code reads the next line of text from the file and returns it.</span></span> <span data-ttu-id="8be71-164">读取完整个文件时，序列会指示没有其他项。</span><span class="sxs-lookup"><span data-stu-id="8be71-164">When the file is completely read, the sequence indicates that there are no more items.</span></span>

<span data-ttu-id="8be71-165">还有两个 C# 语法元素你可能是刚开始接触。</span><span class="sxs-lookup"><span data-stu-id="8be71-165">There are two other C# syntax elements that may be new to you.</span></span> <span data-ttu-id="8be71-166">此方法中的 [`using`](../language-reference/keywords/using-statement.md) 语句用于管理资源清理。</span><span class="sxs-lookup"><span data-stu-id="8be71-166">The [`using`](../language-reference/keywords/using-statement.md) statement in this method manages resource cleanup.</span></span> <span data-ttu-id="8be71-167">`using` 语句中初始化的变量（在此示例中，为 `reader`）必须实现 <xref:System.IDisposable> 接口。</span><span class="sxs-lookup"><span data-stu-id="8be71-167">The variable that is initialized in the `using` statement (`reader`, in this example) must implement the <xref:System.IDisposable> interface.</span></span> <span data-ttu-id="8be71-168">该接口定义一个方法（`Dispose`），应在释放资源时调用此方法。</span><span class="sxs-lookup"><span data-stu-id="8be71-168">That interface defines a single method, `Dispose`, that should be called when the resource should be released.</span></span> <span data-ttu-id="8be71-169">当快执行到 `using` 语句的右大括号时，编译器会生成此调用。</span><span class="sxs-lookup"><span data-stu-id="8be71-169">The compiler generates that call when execution reaches the closing brace of the `using` statement.</span></span> <span data-ttu-id="8be71-170">编译器生成的代码可确保资源得到释放，即使代码块中用 using 语句定义的代码抛出异常，也不例外。</span><span class="sxs-lookup"><span data-stu-id="8be71-170">The compiler-generated code ensures that the resource is released even if an exception is thrown from the code in the block defined by the using statement.</span></span>

<span data-ttu-id="8be71-171">`reader` 变量是使用 `var` 关键字进行定义。</span><span class="sxs-lookup"><span data-stu-id="8be71-171">The `reader` variable is defined using the `var` keyword.</span></span> <span data-ttu-id="8be71-172">[`var`](../language-reference/keywords/var.md) 定义的是隐式类型本地变量。</span><span class="sxs-lookup"><span data-stu-id="8be71-172">[`var`](../language-reference/keywords/var.md) defines an *implicitly typed local variable*.</span></span> <span data-ttu-id="8be71-173">也就是说，变量的类型是由分配给变量的对象的编译时类型决定的。</span><span class="sxs-lookup"><span data-stu-id="8be71-173">That means the type of the variable is determined by the compile-time type of the object assigned to the variable.</span></span> <span data-ttu-id="8be71-174">此处，它为 <xref:System.IO.File.OpenText(System.String)> 方法的返回值，即 <xref:System.IO.StreamReader> 对象。</span><span class="sxs-lookup"><span data-stu-id="8be71-174">Here, that is the return value from the <xref:System.IO.File.OpenText(System.String)> method, which is a <xref:System.IO.StreamReader> object.</span></span>

<span data-ttu-id="8be71-175">现在，让我们在 `Main` 方法中填充用于读取文件的代码：</span><span class="sxs-lookup"><span data-stu-id="8be71-175">Now, let's fill in the code to read the file in the `Main` method:</span></span>

```csharp
var lines = ReadFrom("sampleQuotes.txt");
foreach (var line in lines)
{
    Console.WriteLine(line);
}
```

<span data-ttu-id="8be71-176">使用 `dotnet run` 运行程序，可以看到控制台中打印输出所有文本行。</span><span class="sxs-lookup"><span data-stu-id="8be71-176">Run the program (using `dotnet run`) and you can see every line printed out to the console.</span></span>

## <a name="adding-delays-and-formatting-output"></a><span data-ttu-id="8be71-177">添加延迟和设置输出格式</span><span class="sxs-lookup"><span data-stu-id="8be71-177">Adding Delays and Formatting output</span></span>

<span data-ttu-id="8be71-178">现在的问题是，输出显示过快，无法大声朗读。</span><span class="sxs-lookup"><span data-stu-id="8be71-178">What you have is being displayed far too fast to read aloud.</span></span> <span data-ttu-id="8be71-179">此时，需要为输出添加延迟。</span><span class="sxs-lookup"><span data-stu-id="8be71-179">Now you need to add the delays in the output.</span></span> <span data-ttu-id="8be71-180">首先，将生成一些可实现异步处理的核心代码。</span><span class="sxs-lookup"><span data-stu-id="8be71-180">As you start, you'll be building some of the core code that enables asynchronous processing.</span></span> <span data-ttu-id="8be71-181">不过，在执行这些初始步骤时，将遵循一些反面模式。</span><span class="sxs-lookup"><span data-stu-id="8be71-181">However, these first steps will follow a few anti-patterns.</span></span> <span data-ttu-id="8be71-182">反面模式会在你添加代码时在注释中指出，代码将在后面的步骤中进行更新。</span><span class="sxs-lookup"><span data-stu-id="8be71-182">The anti-patterns are pointed out in comments as you add the code, and the code will be updated in later steps.</span></span>

<span data-ttu-id="8be71-183">这部分包含两步操作。</span><span class="sxs-lookup"><span data-stu-id="8be71-183">There are two steps to this section.</span></span> <span data-ttu-id="8be71-184">首先，将迭代器方法更新为返回单个字词，而不是整行文本。</span><span class="sxs-lookup"><span data-stu-id="8be71-184">First, you'll update the iterator method to return single words instead of entire lines.</span></span> <span data-ttu-id="8be71-185">为此，执行下面这些修改。</span><span class="sxs-lookup"><span data-stu-id="8be71-185">That's done with these modifications.</span></span> <span data-ttu-id="8be71-186">用以下代码替换 `yield return line;` 语句：</span><span class="sxs-lookup"><span data-stu-id="8be71-186">Replace the `yield return line;` statement with the following code:</span></span>

```csharp
var words = line.Split(' ');
foreach (var word in words)
{
    yield return word + " ";
}
yield return Environment.NewLine;
```

<span data-ttu-id="8be71-187">接下来，需要修改对文件行的使用方式，并在写入每个字词后添加延迟。</span><span class="sxs-lookup"><span data-stu-id="8be71-187">Next, you need to modify how you consume the lines of the file, and add a delay after writing each word.</span></span> <span data-ttu-id="8be71-188">用以下代码块替换 `Main` 方法中的 `Console.WriteLine(line)` 的语句：</span><span class="sxs-lookup"><span data-stu-id="8be71-188">Replace the `Console.WriteLine(line)` statement in the `Main` method with the following block:</span></span>

```csharp
Console.Write(line);
if (!string.IsNullOrWhiteSpace(line))
{
    var pause = Task.Delay(200);
    // Synchronously waiting on a task is an
    // anti-pattern. This will get fixed in later
    // steps.
    pause.Wait();
}
```

<span data-ttu-id="8be71-189">由于 <xref:System.Threading.Tasks.Task> 类位于 <xref:System.Threading.Tasks> 命名空间中，因此需要在文件的最上面添加 `using` 语句：</span><span class="sxs-lookup"><span data-stu-id="8be71-189">The <xref:System.Threading.Tasks.Task> class is in the <xref:System.Threading.Tasks> namespace, so you need to add that `using` statement at the top of file:</span></span>

```csharp
using System.Threading.Tasks;
```

<span data-ttu-id="8be71-190">运行此示例并检查输出。</span><span class="sxs-lookup"><span data-stu-id="8be71-190">Run the sample, and check the output.</span></span> <span data-ttu-id="8be71-191">现在，每打印输出一个字词后，就会有 200 毫秒的延迟。</span><span class="sxs-lookup"><span data-stu-id="8be71-191">Now, each single word is printed, followed by a 200 ms delay.</span></span> <span data-ttu-id="8be71-192">不过，显示的输出反映出一些问题，因为源文本文件有好几行都超过 80 个字符，且没有换行符。</span><span class="sxs-lookup"><span data-stu-id="8be71-192">However, the displayed output shows some issues because the source text file has several lines that have more than 80 characters without a line break.</span></span> <span data-ttu-id="8be71-193">很难滚动读取这些文本。</span><span class="sxs-lookup"><span data-stu-id="8be71-193">That can be hard to read while it's scrolling by.</span></span> <span data-ttu-id="8be71-194">此问题很容易解决。</span><span class="sxs-lookup"><span data-stu-id="8be71-194">That's easy to fix.</span></span> <span data-ttu-id="8be71-195">只需跟踪每行长度，然后在行长度达到特定阈值时生成新的一行即可。</span><span class="sxs-lookup"><span data-stu-id="8be71-195">You'll just keep track of the length of each line, and generate a new line whenever the line length reaches a certain threshold.</span></span> <span data-ttu-id="8be71-196">在 `ReadFrom` 方法中声明 `words` 后声明一个局部变量，用于保存行长度：</span><span class="sxs-lookup"><span data-stu-id="8be71-196">Declare a local variable after the declaration of `words` in the `ReadFrom` method that holds the line length:</span></span>

```csharp
var lineLength = 0;
```

<span data-ttu-id="8be71-197">然后，在 `yield return word + " ";` 语句后（在右大括号前）添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="8be71-197">Then, add the following code after the `yield return word + " ";` statement (before the closing brace):</span></span>

```csharp
lineLength += word.Length + 1;
if (lineLength > 70)
{
    yield return Environment.NewLine;
    lineLength = 0;
}
```

<span data-ttu-id="8be71-198">运行此示例，将能够按预配速大声朗读文本。</span><span class="sxs-lookup"><span data-stu-id="8be71-198">Run the sample, and you'll be able to read aloud at its pre-configured pace.</span></span>

## <a name="async-tasks"></a><span data-ttu-id="8be71-199">异步任务</span><span class="sxs-lookup"><span data-stu-id="8be71-199">Async Tasks</span></span>

<span data-ttu-id="8be71-200">最后一步将是添加代码，以便在一个任务中异步编写输出，同时运行另一任务来读取用户输入（如果用户想要加快或减慢文本显示速度，或完全停止文本显示的话）。</span><span class="sxs-lookup"><span data-stu-id="8be71-200">In this final step, you'll add the code to write the output asynchronously in one task, while also running another task to read input from the user if they want to speed up or slow down the text display, or stop the text display altogether.</span></span> <span data-ttu-id="8be71-201">此过程分为几步操作，最后将完成所需的全部更新。</span><span class="sxs-lookup"><span data-stu-id="8be71-201">This has a few steps in it and by the end, you'll have all the updates that you need.</span></span> <span data-ttu-id="8be71-202">第一步是创建异步 <xref:System.Threading.Tasks.Task> 返回方法，用于表示已创建的用于读取和显示文件的代码。</span><span class="sxs-lookup"><span data-stu-id="8be71-202">The first step is to create an asynchronous <xref:System.Threading.Tasks.Task> returning method that represents the code you've created so far to read and display the file.</span></span>

<span data-ttu-id="8be71-203">将以下方法（截取自 `Main` 方法主体）添加到 `Program` 类中：</span><span class="sxs-lookup"><span data-stu-id="8be71-203">Add this method to your `Program` class (it's taken from the body of your `Main` method):</span></span>

```csharp
private static async Task ShowTeleprompter()
{
    var words = ReadFrom("sampleQuotes.txt");
    foreach (var word in words)
    {
        Console.Write(word);
        if (!string.IsNullOrWhiteSpace(word))
        {
            await Task.Delay(200);
        }
    }
}
```

<span data-ttu-id="8be71-204">你会注意到两处更改。</span><span class="sxs-lookup"><span data-stu-id="8be71-204">You'll notice two changes.</span></span> <span data-ttu-id="8be71-205">首先，此版本在方法主体中使用 `await` 关键字，而不是调用 <xref:System.Threading.Tasks.Task.Wait> 同步等待任务完成。</span><span class="sxs-lookup"><span data-stu-id="8be71-205">First, in the body of the method, instead of calling <xref:System.Threading.Tasks.Task.Wait> to synchronously wait for a task to finish, this version uses the `await` keyword.</span></span> <span data-ttu-id="8be71-206">为此，需要将 `async` 修饰符添加到方法签名中。</span><span class="sxs-lookup"><span data-stu-id="8be71-206">In order to do that, you need to add the `async` modifier to the method signature.</span></span> <span data-ttu-id="8be71-207">此方法返回 `Task`。</span><span class="sxs-lookup"><span data-stu-id="8be71-207">This method returns a `Task`.</span></span> <span data-ttu-id="8be71-208">请注意，没有用于返回 `Task` 对象的返回语句。</span><span class="sxs-lookup"><span data-stu-id="8be71-208">Notice that there are no return statements that return a `Task` object.</span></span> <span data-ttu-id="8be71-209">相反，`Task` 对象由编译器在你使用 `await` 运算符时生成的代码进行创建。</span><span class="sxs-lookup"><span data-stu-id="8be71-209">Instead, that `Task` object is created by code the compiler generates when you use the `await` operator.</span></span> <span data-ttu-id="8be71-210">可以想象，此方法在到达 `await` 时返回。</span><span class="sxs-lookup"><span data-stu-id="8be71-210">You can imagine that this method returns when it reaches an `await`.</span></span> <span data-ttu-id="8be71-211">返回的 `Task` 指示工作未完成。</span><span class="sxs-lookup"><span data-stu-id="8be71-211">The returned `Task` indicates that the work has not completed.</span></span> <span data-ttu-id="8be71-212">在等待的任务完成时，此方法继续执行。</span><span class="sxs-lookup"><span data-stu-id="8be71-212">The method resumes when the awaited task completes.</span></span> <span data-ttu-id="8be71-213">执行完后，返回的 `Task` 会指示已完成。</span><span class="sxs-lookup"><span data-stu-id="8be71-213">When it has executed to completion, the returned `Task` indicates that it is complete.</span></span>
<span data-ttu-id="8be71-214">调用代码可以通过监视返回的 `Task` 来确定完成时间。</span><span class="sxs-lookup"><span data-stu-id="8be71-214">Calling code can monitor that returned `Task` to determine when it has completed.</span></span>

<span data-ttu-id="8be71-215">可以在 `Main` 方法中调用以下新方法：</span><span class="sxs-lookup"><span data-stu-id="8be71-215">You can call this new method in your `Main` method:</span></span>

```csharp
ShowTeleprompter().Wait();
```

<span data-ttu-id="8be71-216">此时，在 `Main` 中，代码确实是同步等待。</span><span class="sxs-lookup"><span data-stu-id="8be71-216">Here, in `Main`, the code does synchronously wait.</span></span> <span data-ttu-id="8be71-217">应尽可能使用 `await` 运算符，而不是采用同步等待的方式。</span><span class="sxs-lookup"><span data-stu-id="8be71-217">You should use the `await` operator instead of synchronously waiting whenever possible.</span></span> <span data-ttu-id="8be71-218">不过，在控制台应用程序的 `Main` 方法中，不能使用 `await` 运算符。</span><span class="sxs-lookup"><span data-stu-id="8be71-218">But, in a console application's `Main` method, you cannot use the `await` operator.</span></span> <span data-ttu-id="8be71-219">这会导致应用程序在所有任务完成前退出。</span><span class="sxs-lookup"><span data-stu-id="8be71-219">That would result in the application exiting before all tasks have completed.</span></span>

> [!NOTE]
> <span data-ttu-id="8be71-220">如果使用 C# 7.1 或更高版本，则可以使用 [`async` `Main` 方法](../whats-new/csharp-7.md#async-main)创建控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="8be71-220">If you use C# 7.1 or later, you can create console applications with [`async` `Main` method](../whats-new/csharp-7.md#async-main).</span></span>

<span data-ttu-id="8be71-221">接下来，需要编写第二个异步方法，从控制台读取键，并监视“<”（小于）、“>”（大于）和“X”或“x”键。</span><span class="sxs-lookup"><span data-stu-id="8be71-221">Next, you need to write the second asynchronous method to read from the Console and watch for the '<' (less than), '>' (greater than) and 'X' or 'x' keys.</span></span> <span data-ttu-id="8be71-222">下面是为此任务添加的方法：</span><span class="sxs-lookup"><span data-stu-id="8be71-222">Here's the method you add for that task:</span></span>

```csharp
private static async Task GetInput()
{
    var delay = 200;
    Action work = () =>
    {
        do {
            var key = Console.ReadKey(true);
            if (key.KeyChar == '>')
            {
                delay -= 10;
            }
            else if (key.KeyChar == '<')
            {
                delay += 10;
            }
            else if (key.KeyChar == 'X' || key.KeyChar == 'x')
            {
                break;
            }
        } while (true);
    };
    await Task.Run(work);
}
```

<span data-ttu-id="8be71-223">这创建了一个表示 <xref:System.Action> 委托的 lambda 表达式，用于在用户按“<”（小于）或“>”（大于）键时，从控制台读取键，并修改表示延迟的局部变量。</span><span class="sxs-lookup"><span data-stu-id="8be71-223">This creates a lambda expression to represent an <xref:System.Action> delegate that reads a key from the Console and modifies a local variable representing the delay when the user presses the '<' (less than) or '>' (greater than) keys.</span></span> <span data-ttu-id="8be71-224">当用户按下“X”或“x”键时，委托方法结束，允许用户随时停止文本显示。</span><span class="sxs-lookup"><span data-stu-id="8be71-224">The delegate method finishes when user presses the 'X' or 'x'  keys, which allow the user to stop the text display at any time.</span></span> <span data-ttu-id="8be71-225">此方法使用 <xref:System.Console.ReadKey> 来阻止并等待用户按键。</span><span class="sxs-lookup"><span data-stu-id="8be71-225">This method uses <xref:System.Console.ReadKey> to block and wait for the user to press a key.</span></span>

<span data-ttu-id="8be71-226">若要完成这项功能，需要新建 `async Task` 返回方法，用于启动这两项任务（`GetInput` 和 `ShowTeleprompter`），并管理这两项任务之间共享的数据。</span><span class="sxs-lookup"><span data-stu-id="8be71-226">To finish this feature, you need to create a new `async Task` returning method that starts both of these tasks (`GetInput` and `ShowTeleprompter`), and also manages the shared data between these two tasks.</span></span>

<span data-ttu-id="8be71-227">是时候创建一个类来处理这两项任务之间共享的数据了。</span><span class="sxs-lookup"><span data-stu-id="8be71-227">It's time to create a class that can handle the shared data between these two tasks.</span></span> <span data-ttu-id="8be71-228">此类包含两个公共属性，即延迟和指示已读取完整个文件的标志 `Done`：</span><span class="sxs-lookup"><span data-stu-id="8be71-228">This class contains two public properties: the delay, and a flag `Done` to indicate that the file has been completely read:</span></span>

```csharp
namespace TeleprompterConsole
{
    internal class TelePrompterConfig
    {
        public int DelayInMilliseconds { get; private set; } = 200;

        public void UpdateDelay(int increment) // negative to speed up
        {
            var newDelay = Min(DelayInMilliseconds + increment, 1000);
            newDelay = Max(newDelay, 20);
            DelayInMilliseconds = newDelay;
        }

        public bool Done { get; private set; }

        public void SetDone()
        {
            Done = true;
        }
    }
}
```

<span data-ttu-id="8be71-229">将此类放入新文件，并将此类封闭在 `TeleprompterConsole` 命名空间中，如上所示。</span><span class="sxs-lookup"><span data-stu-id="8be71-229">Put that class in a new file, and enclose that class in the `TeleprompterConsole` namespace as shown above.</span></span> <span data-ttu-id="8be71-230">还需要添加 `using static` 语句，以便可以引用 `Min` 和 `Max` 方法，而无需使用封闭类或命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="8be71-230">You'll also need to add a `using static` statement so that you can reference the `Min` and `Max` methods without the enclosing class or namespace names.</span></span> <span data-ttu-id="8be71-231">[`using static`](../language-reference/keywords/using-static.md) 语句从一个类导入方法。</span><span class="sxs-lookup"><span data-stu-id="8be71-231">A [`using static`](../language-reference/keywords/using-static.md) statement imports the methods from one class.</span></span> <span data-ttu-id="8be71-232">这与一直使用的 `using` 语句相反，后者导入命名空间中的所有类。</span><span class="sxs-lookup"><span data-stu-id="8be71-232">This is in contrast with the `using` statements used up to this point that have imported all classes from a namespace.</span></span>

```csharp
using static System.Math;
```

<span data-ttu-id="8be71-233">接下来，需要将 `ShowTeleprompter` 和 `GetInput` 方法更新为使用新的 `config` 对象。</span><span class="sxs-lookup"><span data-stu-id="8be71-233">Next, you need to update the `ShowTeleprompter` and `GetInput` methods to use the new `config` object.</span></span> <span data-ttu-id="8be71-234">编写最后一个 `Task` 返回 `async` 方法，用于启动这两项任务，并在第一项任务完成时退出：</span><span class="sxs-lookup"><span data-stu-id="8be71-234">Write one final `Task` returning `async` method to start both tasks and exit when the first task finishes:</span></span>

```csharp
private static async Task RunTeleprompter()
{
    var config = new TelePrompterConfig();
    var displayTask = ShowTeleprompter(config);

    var speedTask = GetInput(config);
    await Task.WhenAny(displayTask, speedTask);
}
```

<span data-ttu-id="8be71-235">此处的一种新方法是 <xref:System.Threading.Tasks.Task.WhenAny(System.Threading.Tasks.Task[])> 调用。</span><span class="sxs-lookup"><span data-stu-id="8be71-235">The one new method here is the <xref:System.Threading.Tasks.Task.WhenAny(System.Threading.Tasks.Task[])> call.</span></span> <span data-ttu-id="8be71-236">这会创建 `Task`，只要自变量列表中的任意一项任务完成，它就会完成。</span><span class="sxs-lookup"><span data-stu-id="8be71-236">That creates a `Task` that finishes as soon as any of the tasks in its argument list completes.</span></span>

<span data-ttu-id="8be71-237">接下来，需要同时将 `ShowTeleprompter` 和 `GetInput` 方法更新为对延迟使用 `config` 对象：</span><span class="sxs-lookup"><span data-stu-id="8be71-237">Next, you need to update both the `ShowTeleprompter` and `GetInput` methods to use the `config` object for the delay:</span></span>

```csharp
private static async Task ShowTeleprompter(TelePrompterConfig config)
{
    var words = ReadFrom("sampleQuotes.txt");
    foreach (var word in words)
    {
        Console.Write(word);
        if (!string.IsNullOrWhiteSpace(word))
        {
            await Task.Delay(config.DelayInMilliseconds);
        }
    }
    config.SetDone();
}

private static async Task GetInput(TelePrompterConfig config)
{
    Action work = () =>
    {
        do {
            var key = Console.ReadKey(true);
            if (key.KeyChar == '>')
                config.UpdateDelay(-10);
            else if (key.KeyChar == '<')
                config.UpdateDelay(10);
            else if (key.KeyChar == 'X' || key.KeyChar == 'x')
                config.SetDone();
        } while (!config.Done);
    };
    await Task.Run(work);
}
```

<span data-ttu-id="8be71-238">这个新版 `ShowTeleprompter` 在 `TeleprompterConfig` 类中调用新方法。</span><span class="sxs-lookup"><span data-stu-id="8be71-238">This new version of `ShowTeleprompter` calls a new method in the `TeleprompterConfig` class.</span></span> <span data-ttu-id="8be71-239">现在，需要将 `Main` 更新为调用 `RunTeleprompter`（而不是 `ShowTeleprompter`）：</span><span class="sxs-lookup"><span data-stu-id="8be71-239">Now, you need to update `Main` to call `RunTeleprompter` instead of `ShowTeleprompter`:</span></span>

```csharp
RunTeleprompter().Wait();
```

## <a name="conclusion"></a><span data-ttu-id="8be71-240">结束语</span><span class="sxs-lookup"><span data-stu-id="8be71-240">Conclusion</span></span>

<span data-ttu-id="8be71-241">此教程介绍了与处理控制台应用程序相关的许多 C# 语言和 .NET Core 库功能。</span><span class="sxs-lookup"><span data-stu-id="8be71-241">This tutorial showed you a number of the features around the C# language and the .NET Core libraries related to working in Console applications.</span></span> <span data-ttu-id="8be71-242">可以在此教程的基础上进一步探索语言和本文介绍的类。</span><span class="sxs-lookup"><span data-stu-id="8be71-242">You can build on this knowledge to explore more about the language, and the classes introduced here.</span></span> <span data-ttu-id="8be71-243">你已了解文件和控制台 I/O 的基础知识、基于任务的异步编程的阻止性和非阻止性用途、C# 语言介绍、C# 程序的组织结构，以及 .NET Core CLI。</span><span class="sxs-lookup"><span data-stu-id="8be71-243">You've seen the basics of File and Console I/O, blocking and non-blocking use of the Task-based asynchronous programming, a tour of the C# language and how C# programs are organized, and the .NET Core CLI.</span></span>

<span data-ttu-id="8be71-244">有关文件 I/O 的详细信息，请参阅[文件和流 I/O](../../standard/io/index.md) 主题。</span><span class="sxs-lookup"><span data-stu-id="8be71-244">For more information about File I/O, see the [File and Stream I/O](../../standard/io/index.md) topic.</span></span> <span data-ttu-id="8be71-245">有关本教程中使用的异步编程模型的详细信息，请参阅[基于任务的异步编程](../../standard/parallel-programming/task-based-asynchronous-programming.md)主题和[异步编程](../async.md)主题。</span><span class="sxs-lookup"><span data-stu-id="8be71-245">For more information about asynchronous programming model used in this tutorial, see the [Task-based Asynchronous Programming](../../standard/parallel-programming/task-based-asynchronous-programming.md) topic and the [Asynchronous programming](../async.md) topic.</span></span>
