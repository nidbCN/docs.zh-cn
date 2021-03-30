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
# <a name="console-app"></a>控制台应用

此教程将介绍 .NET Core 和 C# 语言的许多功能。 你将了解：

- .NET Core CLI 的基础知识
- C# 控制台应用程序的结构
- 控制台 I/O
- .NET 中文件 I/O API 的基础知识
- .NET 中基于任务的异步编程基础知识

你将生成一个应用程序，用于读取文本文件，然后将文本文件的内容回显到控制台。 按配速大声朗读控制台输出。 可以按“<”（小于）或“>”（大于）键加速或减速显示。

此教程将介绍许多功能。 我们将逐个生成这些功能。

## <a name="prerequisites"></a>先决条件

- 将计算机设置为运行 .NET Core。 有关安装说明，请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页。 可以在 Windows、Linux、macOS 或 Docker 容器中运行此应用程序。

- 安装最喜爱的代码编辑器。

## <a name="create-the-app"></a>创建应用

第一步是新建应用程序。 打开命令提示符，然后新建应用程序的目录。 将新建的目录设为当前目录。 在命令提示符处，键入命令 `dotnet new console`。 这将为基本的“Hello World”应用程序创建起始文件。

在开始进行修改之前，我们先来逐步了解一下如何运行简单的 Hello World 应用程序。 创建应用程序之后，在命令提示符处键入 `dotnet restore`。 此命令将运行 NuGet 包还原进程。 NuGet 是 .NET 程序包管理器。 此命令会下载项目缺少的所有依赖项。 由于这是一个新项目，尚无任何依赖项，因此首次运行只会下载 .NET Core 框架。 执行这一初始步骤后，只需运行 `dotnet restore`，即可添加新的依赖项包，或更新任意依赖项的版本。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

还原包后，运行 `dotnet build`。 这将运行生成引擎，并创建应用程序可执行文件。 最后，执行 `dotnet run` 来运行应用程序。

简单的 Hello World 应用程序代码全都在 Program.cs 中。 使用常用文本编辑器打开此文件。 我们将执行首轮更改。 在此文件的最上面，你会看到 using 语句：

```csharp
using System;
```

此语句指示编译器，`System` 命名空间中的任何类型都在范围内。 与你可能用过的其他面向对象的语言一样，C# 也使用命名空间来整理类型。 此 Hello World 程序也一样。 你可以看到，此程序封闭在名称基于当前目录名的命名空间内。 对于本教程，我们将命名空间的名称更改为 `TeleprompterConsole`：

```csharp
namespace TeleprompterConsole
```

## <a name="reading-and-echoing-the-file"></a>读取和回显文件

要添加的第一项功能是读取文本文件，然后在控制台中显示全部文本。 首先，让我们来添加文本文件。 将此[示例](https://github.com/dotnet/samples/tree/main/csharp/getting-started/console-teleprompter)的 GitHub 存储库中的 [sampleQuotes.txt](https://github.com/dotnet/samples/raw/main/csharp/getting-started/console-teleprompter/sampleQuotes.txt) 文件复制到项目目录中。 这将用作应用程序脚本。 如果需要有关如何下载本主题示例应用的信息，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)主题中的说明。

接下来，在 `Program` 类中添加以下方法（即 `Main` 方法的下方）：

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

此方法使用两个新命名空间中的类型。 为了让编译能够顺利进行，需要在文件的最上面添加以下两行代码：

```csharp
using System.Collections.Generic;
using System.IO;
```

<xref:System.Collections.Generic.IEnumerable%601> 接口是在 <xref:System.Collections.Generic> 命名空间中进行定义。 <xref:System.IO.File> 类是在 <xref:System.IO> 命名空间中进行定义。

这是一种称为“Iterator 方法”的特殊类型 C# 方法。 枚举器方法返回延迟计算的序列。 也就是说，序列中的每一项是在使用序列的代码提出请求时生成。 Enumerator 方法包含一个或多个 [`yield return`](../language-reference/keywords/yield.md) 语句。 `ReadFrom` 方法返回的对象包含用于生成序列中所有项的代码。 在此示例中，这涉及读取源文件中的下一行文本，然后返回相应的字符串。 每当调用代码请求生成序列中的下一项时，代码就会读取并返回文件中的下一行文本。 读取完整个文件时，序列会指示没有其他项。

还有两个 C# 语法元素你可能是刚开始接触。 此方法中的 [`using`](../language-reference/keywords/using-statement.md) 语句用于管理资源清理。 `using` 语句中初始化的变量（在此示例中，为 `reader`）必须实现 <xref:System.IDisposable> 接口。 该接口定义一个方法（`Dispose`），应在释放资源时调用此方法。 当快执行到 `using` 语句的右大括号时，编译器会生成此调用。 编译器生成的代码可确保资源得到释放，即使代码块中用 using 语句定义的代码抛出异常，也不例外。

`reader` 变量是使用 `var` 关键字进行定义。 [`var`](../language-reference/keywords/var.md) 定义的是隐式类型本地变量。 也就是说，变量的类型是由分配给变量的对象的编译时类型决定的。 此处，它为 <xref:System.IO.File.OpenText(System.String)> 方法的返回值，即 <xref:System.IO.StreamReader> 对象。

现在，让我们在 `Main` 方法中填充用于读取文件的代码：

```csharp
var lines = ReadFrom("sampleQuotes.txt");
foreach (var line in lines)
{
    Console.WriteLine(line);
}
```

使用 `dotnet run` 运行程序，可以看到控制台中打印输出所有文本行。

## <a name="adding-delays-and-formatting-output"></a>添加延迟和设置输出格式

现在的问题是，输出显示过快，无法大声朗读。 此时，需要为输出添加延迟。 首先，将生成一些可实现异步处理的核心代码。 不过，在执行这些初始步骤时，将遵循一些反面模式。 反面模式会在你添加代码时在注释中指出，代码将在后面的步骤中进行更新。

这部分包含两步操作。 首先，将迭代器方法更新为返回单个字词，而不是整行文本。 为此，执行下面这些修改。 用以下代码替换 `yield return line;` 语句：

```csharp
var words = line.Split(' ');
foreach (var word in words)
{
    yield return word + " ";
}
yield return Environment.NewLine;
```

接下来，需要修改对文件行的使用方式，并在写入每个字词后添加延迟。 用以下代码块替换 `Main` 方法中的 `Console.WriteLine(line)` 的语句：

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

由于 <xref:System.Threading.Tasks.Task> 类位于 <xref:System.Threading.Tasks> 命名空间中，因此需要在文件的最上面添加 `using` 语句：

```csharp
using System.Threading.Tasks;
```

运行此示例并检查输出。 现在，每打印输出一个字词后，就会有 200 毫秒的延迟。 不过，显示的输出反映出一些问题，因为源文本文件有好几行都超过 80 个字符，且没有换行符。 很难滚动读取这些文本。 此问题很容易解决。 只需跟踪每行长度，然后在行长度达到特定阈值时生成新的一行即可。 在 `ReadFrom` 方法中声明 `words` 后声明一个局部变量，用于保存行长度：

```csharp
var lineLength = 0;
```

然后，在 `yield return word + " ";` 语句后（在右大括号前）添加以下代码：

```csharp
lineLength += word.Length + 1;
if (lineLength > 70)
{
    yield return Environment.NewLine;
    lineLength = 0;
}
```

运行此示例，将能够按预配速大声朗读文本。

## <a name="async-tasks"></a>异步任务

最后一步将是添加代码，以便在一个任务中异步编写输出，同时运行另一任务来读取用户输入（如果用户想要加快或减慢文本显示速度，或完全停止文本显示的话）。 此过程分为几步操作，最后将完成所需的全部更新。 第一步是创建异步 <xref:System.Threading.Tasks.Task> 返回方法，用于表示已创建的用于读取和显示文件的代码。

将以下方法（截取自 `Main` 方法主体）添加到 `Program` 类中：

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

你会注意到两处更改。 首先，此版本在方法主体中使用 `await` 关键字，而不是调用 <xref:System.Threading.Tasks.Task.Wait> 同步等待任务完成。 为此，需要将 `async` 修饰符添加到方法签名中。 此方法返回 `Task`。 请注意，没有用于返回 `Task` 对象的返回语句。 相反，`Task` 对象由编译器在你使用 `await` 运算符时生成的代码进行创建。 可以想象，此方法在到达 `await` 时返回。 返回的 `Task` 指示工作未完成。 在等待的任务完成时，此方法继续执行。 执行完后，返回的 `Task` 会指示已完成。
调用代码可以通过监视返回的 `Task` 来确定完成时间。

可以在 `Main` 方法中调用以下新方法：

```csharp
ShowTeleprompter().Wait();
```

此时，在 `Main` 中，代码确实是同步等待。 应尽可能使用 `await` 运算符，而不是采用同步等待的方式。 不过，在控制台应用程序的 `Main` 方法中，不能使用 `await` 运算符。 这会导致应用程序在所有任务完成前退出。

> [!NOTE]
> 如果使用 C# 7.1 或更高版本，则可以使用 [`async` `Main` 方法](../whats-new/csharp-7.md#async-main)创建控制台应用程序。

接下来，需要编写第二个异步方法，从控制台读取键，并监视“<”（小于）、“>”（大于）和“X”或“x”键。 下面是为此任务添加的方法：

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

这创建了一个表示 <xref:System.Action> 委托的 lambda 表达式，用于在用户按“<”（小于）或“>”（大于）键时，从控制台读取键，并修改表示延迟的局部变量。 当用户按下“X”或“x”键时，委托方法结束，允许用户随时停止文本显示。 此方法使用 <xref:System.Console.ReadKey> 来阻止并等待用户按键。

若要完成这项功能，需要新建 `async Task` 返回方法，用于启动这两项任务（`GetInput` 和 `ShowTeleprompter`），并管理这两项任务之间共享的数据。

是时候创建一个类来处理这两项任务之间共享的数据了。 此类包含两个公共属性，即延迟和指示已读取完整个文件的标志 `Done`：

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

将此类放入新文件，并将此类封闭在 `TeleprompterConsole` 命名空间中，如上所示。 还需要添加 `using static` 语句，以便可以引用 `Min` 和 `Max` 方法，而无需使用封闭类或命名空间名称。 [`using static`](../language-reference/keywords/using-static.md) 语句从一个类导入方法。 这与一直使用的 `using` 语句相反，后者导入命名空间中的所有类。

```csharp
using static System.Math;
```

接下来，需要将 `ShowTeleprompter` 和 `GetInput` 方法更新为使用新的 `config` 对象。 编写最后一个 `Task` 返回 `async` 方法，用于启动这两项任务，并在第一项任务完成时退出：

```csharp
private static async Task RunTeleprompter()
{
    var config = new TelePrompterConfig();
    var displayTask = ShowTeleprompter(config);

    var speedTask = GetInput(config);
    await Task.WhenAny(displayTask, speedTask);
}
```

此处的一种新方法是 <xref:System.Threading.Tasks.Task.WhenAny(System.Threading.Tasks.Task[])> 调用。 这会创建 `Task`，只要自变量列表中的任意一项任务完成，它就会完成。

接下来，需要同时将 `ShowTeleprompter` 和 `GetInput` 方法更新为对延迟使用 `config` 对象：

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

这个新版 `ShowTeleprompter` 在 `TeleprompterConfig` 类中调用新方法。 现在，需要将 `Main` 更新为调用 `RunTeleprompter`（而不是 `ShowTeleprompter`）：

```csharp
RunTeleprompter().Wait();
```

## <a name="conclusion"></a>结束语

此教程介绍了与处理控制台应用程序相关的许多 C# 语言和 .NET Core 库功能。 可以在此教程的基础上进一步探索语言和本文介绍的类。 你已了解文件和控制台 I/O 的基础知识、基于任务的异步编程的阻止性和非阻止性用途、C# 语言介绍、C# 程序的组织结构，以及 .NET Core CLI。

有关文件 I/O 的详细信息，请参阅[文件和流 I/O](../../standard/io/index.md) 主题。 有关本教程中使用的异步编程模型的详细信息，请参阅[基于任务的异步编程](../../standard/parallel-programming/task-based-asynchronous-programming.md)主题和[异步编程](../async.md)主题。
