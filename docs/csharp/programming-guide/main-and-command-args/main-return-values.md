---
title: Main() 返回值 - C# 编程指南
description: 了解 Main() 返回值。 查看代码示例、编译器生成的代码和其他可用资源。
ms.date: 03/11/2021
helpviewer_keywords:
- Main method [C#], return values
ms.assetid: c2f5a1d8-1676-4bea-bc7e-44a97e72d5bc
ms.openlocfilehash: 6f4001ecd490d5627d3a1ec74ecf7d593451e104
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190354"
---
# <a name="main-return-values-c-programming-guide"></a><span data-ttu-id="8265d-104">Main() 返回值（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="8265d-104">Main() return values (C# Programming Guide)</span></span>

<span data-ttu-id="8265d-105">可以通过以下方式之一定义方法，以从 `Main` 方法返回 `int`：</span><span class="sxs-lookup"><span data-stu-id="8265d-105">You can return an `int` from the `Main` method by defining the method in one of the following ways:</span></span>

| <span data-ttu-id="8265d-106">`Main` 方法代码</span><span class="sxs-lookup"><span data-stu-id="8265d-106">`Main` method code</span></span>             | <span data-ttu-id="8265d-107">`Main` 签名</span><span class="sxs-lookup"><span data-stu-id="8265d-107">`Main` signature</span></span>                             |
|--------------------------------|----------------------------------------------|
| <span data-ttu-id="8265d-108">不使用 `args` 或 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-108">No use of `args` or `await`</span></span>    | `static int Main()`                          |
| <span data-ttu-id="8265d-109">使用 `args`，不使用 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-109">Uses `args`, no use of `await`</span></span> | `static int Main(string[] args)`             |
| <span data-ttu-id="8265d-110">不使用 `args`，使用 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-110">No use of `args`, uses `await`</span></span> | `static async Task<int> Main()`              |
| <span data-ttu-id="8265d-111">使用 `args` 和 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-111">Uses `args` and `await`</span></span>        | `static async Task<int> Main(string[] args)` |

<span data-ttu-id="8265d-112">如果不使用 `Main` 的返回值，则返回 `void` 或 `Task` 可使代码变得略微简单。</span><span class="sxs-lookup"><span data-stu-id="8265d-112">If the return value from `Main` is not used, returning `void` or `Task` allows for slightly simpler code.</span></span>

| <span data-ttu-id="8265d-113">`Main` 方法代码</span><span class="sxs-lookup"><span data-stu-id="8265d-113">`Main` method code</span></span>             | <span data-ttu-id="8265d-114">`Main` 签名</span><span class="sxs-lookup"><span data-stu-id="8265d-114">`Main` signature</span></span>                        |
|--------------------------------|-----------------------------------------|
| <span data-ttu-id="8265d-115">不使用 `args` 或 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-115">No use of `args` or `await`</span></span>    | `static void Main()`                    |
| <span data-ttu-id="8265d-116">使用 `args`，不使用 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-116">Uses `args`, no use of `await`</span></span> | `static void Main(string[] args)`       |
| <span data-ttu-id="8265d-117">不使用 `args`，使用 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-117">No use of `args`, uses `await`</span></span> | `static async Task Main()`              |
| <span data-ttu-id="8265d-118">使用 `args` 和 `await`</span><span class="sxs-lookup"><span data-stu-id="8265d-118">Uses `args` and `await`</span></span>        | `static async Task Main(string[] args)` |

<span data-ttu-id="8265d-119">但是，返回 `int` 或 `Task<int>` 可使程序将状态信息传递给调用可执行文件的其他程序或脚本。</span><span class="sxs-lookup"><span data-stu-id="8265d-119">However, returning `int` or `Task<int>` enables the program to communicate status information to other programs or scripts that invoke the executable file.</span></span>

<span data-ttu-id="8265d-120">下面的示例演示了如何访问进程的退出代码。</span><span class="sxs-lookup"><span data-stu-id="8265d-120">The following example shows how the exit code for the process can be accessed.</span></span>

## <a name="example"></a><span data-ttu-id="8265d-121">示例</span><span class="sxs-lookup"><span data-stu-id="8265d-121">Example</span></span>

<span data-ttu-id="8265d-122">此示例使用 [.NET Core](../../../core/introduction.md) 命令行工具。</span><span class="sxs-lookup"><span data-stu-id="8265d-122">This example uses [.NET Core](../../../core/introduction.md) command-line tools.</span></span> <span data-ttu-id="8265d-123">如果不熟悉 .NET Core 命令行工具，可通过本[入门文章](../../../core/tutorials/with-visual-studio-code.md)进行了解。</span><span class="sxs-lookup"><span data-stu-id="8265d-123">If you are unfamiliar with .NET Core command-line tools, you can learn about them in this [get-started article](../../../core/tutorials/with-visual-studio-code.md).</span></span>

<span data-ttu-id="8265d-124">修改 program.cs 中的 `Main` 方法，如下所示  ：</span><span class="sxs-lookup"><span data-stu-id="8265d-124">Modify the `Main` method in *program.cs* as follows:</span></span>

 [!code-csharp[csProgGuideMain#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#14)]

<span data-ttu-id="8265d-125">在 Windows 中执行程序时，从 `Main` 函数返回的任何值都存储在环境变量中。</span><span class="sxs-lookup"><span data-stu-id="8265d-125">When a program is executed in Windows, any value returned from the `Main` function is stored in an environment variable.</span></span> <span data-ttu-id="8265d-126">可使用批处理文件中的 `ERRORLEVEL` 或 PowerShell 中的 `$LastExitCode` 来检索此环境变量。</span><span class="sxs-lookup"><span data-stu-id="8265d-126">This environment variable can be retrieved using `ERRORLEVEL` from a batch file, or `$LastExitCode` from PowerShell.</span></span>

<span data-ttu-id="8265d-127">可使用 [dotnet CLI](../../../core/tools/dotnet.md) `dotnet build` 命令构建应用程序。</span><span class="sxs-lookup"><span data-stu-id="8265d-127">You can build the application using the [dotnet CLI](../../../core/tools/dotnet.md) `dotnet build` command.</span></span>

<span data-ttu-id="8265d-128">接下来，创建一个 PowerShell 脚本来运行应用程序并显示结果。</span><span class="sxs-lookup"><span data-stu-id="8265d-128">Next, create a PowerShell script to run the application and display the result.</span></span> <span data-ttu-id="8265d-129">将以下代码粘贴到文本文件中，并在包含该项目的文件夹中将其另存为 `test.ps1`。</span><span class="sxs-lookup"><span data-stu-id="8265d-129">Paste the following code into a text file and save it as `test.ps1` in the folder that contains the project.</span></span> <span data-ttu-id="8265d-130">可通过在 PowerShell 提示符下键入 `test.ps1` 来运行 PowerShell 脚本。</span><span class="sxs-lookup"><span data-stu-id="8265d-130">Run the PowerShell script by typing `test.ps1` at the PowerShell prompt.</span></span>

<span data-ttu-id="8265d-131">因为代码返回零，所以批处理文件将报告成功。</span><span class="sxs-lookup"><span data-stu-id="8265d-131">Because the code returns zero, the batch file will report success.</span></span> <span data-ttu-id="8265d-132">但是，如果将 MainReturnValTest.cs 更改为返回非零值，然后重新编译程序，则 PowerShell 脚本的后续执行将报告为失败。</span><span class="sxs-lookup"><span data-stu-id="8265d-132">However, if you change MainReturnValTest.cs to return a non-zero value and then recompile the program, subsequent execution of the PowerShell script will report failure.</span></span>

```dotnetcli
dotnet run
```

```powershell
if ($LastExitCode -eq 0) {
    Write-Host "Execution succeeded"
} else
{
    Write-Host "Execution Failed"
}
Write-Host "Return value = " $LastExitCode
```

## <a name="sample-output"></a><span data-ttu-id="8265d-133">示例输出</span><span class="sxs-lookup"><span data-stu-id="8265d-133">Sample output</span></span>

```txt
Execution succeeded
Return value = 0
```

## <a name="async-main-return-values"></a><span data-ttu-id="8265d-134">Async Main 返回值</span><span class="sxs-lookup"><span data-stu-id="8265d-134">Async Main return values</span></span>

<span data-ttu-id="8265d-135">Async Main 返回值将在 `Main` 中调用异步方法时所需的样本代码移动到编译器生成的代码中。</span><span class="sxs-lookup"><span data-stu-id="8265d-135">Async Main return values move the boilerplate code necessary for calling asynchronous methods in `Main` to code generated by the compiler.</span></span> <span data-ttu-id="8265d-136">以前，需要编写此结构来调用异步代码并确保程序运行至异步操作完成：</span><span class="sxs-lookup"><span data-stu-id="8265d-136">Previously, you would need to write this construct to call asynchronous code and ensure your program ran until the asynchronous operation completed:</span></span>

```csharp
public static void Main()
{
    AsyncConsoleWork().GetAwaiter().GetResult();
}

private static async Task<int> AsyncConsoleWork()
{
    // Main body here
    return 0;
}
```

<span data-ttu-id="8265d-137">现在，可以将其替代为：</span><span class="sxs-lookup"><span data-stu-id="8265d-137">Now, this can be replaced by:</span></span>

[!code-csharp[AsyncMain](../../../../samples/snippets/csharp/main-arguments/program.cs#AsyncMain)]

<span data-ttu-id="8265d-138">新语法的优点是编译器始终生成正确的代码。</span><span class="sxs-lookup"><span data-stu-id="8265d-138">The advantage of the new syntax is that the compiler always generates the correct code.</span></span>

## <a name="compiler-generated-code"></a><span data-ttu-id="8265d-139">编译器生成的代码</span><span class="sxs-lookup"><span data-stu-id="8265d-139">Compiler-generated code</span></span>

<span data-ttu-id="8265d-140">当应用程序入口点返回 `Task` 或 `Task<int>` 时，编译器生成一个新的入口点，该入口点调用应用程序代码中声明的入口点方法。</span><span class="sxs-lookup"><span data-stu-id="8265d-140">When the application entry point returns a `Task` or `Task<int>`, the compiler generates a new entry point that calls the entry point method declared in the application code.</span></span> <span data-ttu-id="8265d-141">假设此入口点名为 `$GeneratedMain`，编译器将为这些入口点生成以下代码：</span><span class="sxs-lookup"><span data-stu-id="8265d-141">Assuming that this entry point is called `$GeneratedMain`, the compiler generates the following code for these entry points:</span></span>

- <span data-ttu-id="8265d-142">`static Task Main()` 导致编译器发出 `private static void $GeneratedMain() => Main().GetAwaiter().GetResult();` 的等效项</span><span class="sxs-lookup"><span data-stu-id="8265d-142">`static Task Main()` results in the compiler emitting the equivalent of `private static void $GeneratedMain() => Main().GetAwaiter().GetResult();`</span></span>
- <span data-ttu-id="8265d-143">`static Task Main(string[])` 导致编译器发出 `private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();` 的等效项</span><span class="sxs-lookup"><span data-stu-id="8265d-143">`static Task Main(string[])` results in the compiler emitting the equivalent of `private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();`</span></span>
- <span data-ttu-id="8265d-144">`static Task<int> Main()` 导致编译器发出 `private static int $GeneratedMain() => Main().GetAwaiter().GetResult();` 的等效项</span><span class="sxs-lookup"><span data-stu-id="8265d-144">`static Task<int> Main()` results in the compiler emitting the equivalent of `private static int $GeneratedMain() => Main().GetAwaiter().GetResult();`</span></span>
- <span data-ttu-id="8265d-145">`static Task<int> Main(string[])` 导致编译器发出 `private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();` 的等效项</span><span class="sxs-lookup"><span data-stu-id="8265d-145">`static Task<int> Main(string[])` results in the compiler emitting the equivalent of `private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();`</span></span>

> [!NOTE]
><span data-ttu-id="8265d-146">如果示例在 `Main` 方法上使用 `async` 修饰符，则编译器将生成相同的代码。</span><span class="sxs-lookup"><span data-stu-id="8265d-146">If the examples used `async` modifier on the `Main` method, the compiler would generate the same code.</span></span>

## <a name="see-also"></a><span data-ttu-id="8265d-147">请参阅</span><span class="sxs-lookup"><span data-stu-id="8265d-147">See also</span></span>

- [<span data-ttu-id="8265d-148">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="8265d-148">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="8265d-149">C# 参考</span><span class="sxs-lookup"><span data-stu-id="8265d-149">C# Reference</span></span>](../../language-reference/index.md)
- [<span data-ttu-id="8265d-150">Main() 和命令行参数</span><span class="sxs-lookup"><span data-stu-id="8265d-150">Main() and Command-Line Arguments</span></span>](index.md)
- [<span data-ttu-id="8265d-151">如何显示命令行参数</span><span class="sxs-lookup"><span data-stu-id="8265d-151">How to display command line arguments</span></span>](./how-to-display-command-line-arguments.md)
