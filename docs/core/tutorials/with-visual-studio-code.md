---
title: 使用 Visual Studio Code 创建 .NET 控制台应用程序
description: 了解如何使用 Visual Studio Code 和 .NET CLI 创建 .NET 控制台应用程序。
ms.date: 11/17/2020
ms.openlocfilehash: 51e5a897985af7576de03659efdd8520cb8e58e6
ms.sourcegitcommit: 1d3af230ec30d8d061be7a887f6ba38a530c4ece
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511861"
---
# <a name="tutorial-create-a-net-console-application-using-visual-studio-code"></a><span data-ttu-id="b9c79-103">教程：使用 Visual Studio Code 创建 .NET 控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="b9c79-103">Tutorial: Create a .NET console application using Visual Studio Code</span></span>

<span data-ttu-id="b9c79-104">本教程演示如何使用 Visual Studio Code 和 .NET CLI 创建并运行 .NET 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="b9c79-104">This tutorial shows how to create and run a .NET console application by using Visual Studio Code and the .NET CLI.</span></span> <span data-ttu-id="b9c79-105">项目任务（例如创建、编译和运行项目）通过使用 .NET CLI 来完成。</span><span class="sxs-lookup"><span data-stu-id="b9c79-105">Project tasks, such as creating, compiling, and running a project are done by using the .NET CLI.</span></span> <span data-ttu-id="b9c79-106">你可以遵循本教程中的步骤使用其他代码编辑器，然后在终端中运行命令（如果你愿意）。</span><span class="sxs-lookup"><span data-stu-id="b9c79-106">You can follow this tutorial with a different code editor and run commands in a terminal if you prefer.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9c79-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="b9c79-107">Prerequisites</span></span>

1. <span data-ttu-id="b9c79-108">已安装 [C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) 的 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="b9c79-108">[Visual Studio Code](https://code.visualstudio.com/) with the [C# extension](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) installed.</span></span> <span data-ttu-id="b9c79-109">有关如何在 Visual Studio Code 上安装扩展的信息，请访问 [VS Code 扩展市场](https://code.visualstudio.com/docs/editor/extension-gallery)。</span><span class="sxs-lookup"><span data-stu-id="b9c79-109">For information about how to install extensions on Visual Studio Code, see [VS Code Extension Marketplace](https://code.visualstudio.com/docs/editor/extension-gallery).</span></span>
2. <span data-ttu-id="b9c79-110">[.NET 5.0 SDK 或更高版本](https://dotnet.microsoft.com/download)</span><span class="sxs-lookup"><span data-stu-id="b9c79-110">The [.NET 5.0 SDK or later](https://dotnet.microsoft.com/download)</span></span>

## <a name="create-the-app"></a><span data-ttu-id="b9c79-111">创建应用</span><span class="sxs-lookup"><span data-stu-id="b9c79-111">Create the app</span></span>

<span data-ttu-id="b9c79-112">创建一个名为“HelloWorld”的 .NET 控制台应用项目。</span><span class="sxs-lookup"><span data-stu-id="b9c79-112">Create a .NET console app project named "HelloWorld".</span></span>

1. <span data-ttu-id="b9c79-113">启动 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="b9c79-113">Start Visual Studio Code.</span></span>

1. <span data-ttu-id="b9c79-114">从主菜单中选择“文件” > “打开文件夹”（在 macOS 上为“文件” > “打开...”）。</span><span class="sxs-lookup"><span data-stu-id="b9c79-114">Select **File** > **Open Folder** (**File** > **Open...** on macOS) from the main menu.</span></span>

1. <span data-ttu-id="b9c79-115">在“打开文件夹”对话框中，创建“HelloWorld”文件夹，然后单击“选择文件夹”（在 macOS 上为“打开”）。</span><span class="sxs-lookup"><span data-stu-id="b9c79-115">In the **Open Folder** dialog, create a *HelloWorld* folder and click **Select Folder** (**Open** on macOS).</span></span>

   <span data-ttu-id="b9c79-116">默认情况下，文件夹名称将是项目名称和命名空间名称。</span><span class="sxs-lookup"><span data-stu-id="b9c79-116">The folder name becomes the project name and the namespace name by default.</span></span> <span data-ttu-id="b9c79-117">稍后将在本教程中添加代码，假定项目命名空间为 `HelloWorld`。</span><span class="sxs-lookup"><span data-stu-id="b9c79-117">You'll add code later in the tutorial that assumes the project namespace is `HelloWorld`.</span></span>

1. <span data-ttu-id="b9c79-118">在主菜单中选择“视图” > “终端”，从 Visual Studio Code 中打开“终端”  。</span><span class="sxs-lookup"><span data-stu-id="b9c79-118">Open the **Terminal** in Visual Studio Code by selecting **View** > **Terminal** from the main menu.</span></span>

   <span data-ttu-id="b9c79-119">“终端”在“HelloWorld”文件夹中连同命令提示符一起打开。</span><span class="sxs-lookup"><span data-stu-id="b9c79-119">The **Terminal** opens with the command prompt in the *HelloWorld* folder.</span></span>

1. <span data-ttu-id="b9c79-120">在“终端”中输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="b9c79-120">In the **Terminal**, enter the following command:</span></span>

   ```dotnetcli
   dotnet new console
   ```

<span data-ttu-id="b9c79-121">用于创建简单的“Hello World”应用程序的模板。</span><span class="sxs-lookup"><span data-stu-id="b9c79-121">The template creates a simple "Hello World" application.</span></span> <span data-ttu-id="b9c79-122">调用 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法以在控制台窗口中显示“:::no-loc text="Hello World!":::”。</span><span class="sxs-lookup"><span data-stu-id="b9c79-122">It calls the <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> method to display ":::no-loc text="Hello World!":::" in the console window.</span></span>

<span data-ttu-id="b9c79-123">模板代码将定义类 `Program`，其中包含一个需要将 <xref:System.String> 数组用作参数的方法 `Main`：</span><span class="sxs-lookup"><span data-stu-id="b9c79-123">The template code defines a class, `Program`, with a single method, `Main`, that takes a <xref:System.String> array as an argument:</span></span>

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

<span data-ttu-id="b9c79-124">`Main` 是应用程序入口点，同时也是在应用程序启动时由运行时自动调用的方法。</span><span class="sxs-lookup"><span data-stu-id="b9c79-124">`Main` is the application entry point, the method that's called automatically by the runtime when it launches the application.</span></span> <span data-ttu-id="b9c79-125">*args* 数组中包含在应用程序启动时提供的所有命令行自变量。</span><span class="sxs-lookup"><span data-stu-id="b9c79-125">Any command-line arguments supplied when the application is launched are available in the *args* array.</span></span>

## <a name="run-the-app"></a><span data-ttu-id="b9c79-126">运行应用</span><span class="sxs-lookup"><span data-stu-id="b9c79-126">Run the app</span></span>

<span data-ttu-id="b9c79-127">在“终端”中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="b9c79-127">Run the following command in the **Terminal**:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="b9c79-128">程序显示“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="b9c79-128">The program displays "Hello World!"</span></span> <span data-ttu-id="b9c79-129">然后结束。</span><span class="sxs-lookup"><span data-stu-id="b9c79-129">and ends.</span></span>

![dotnet run 命令](media/with-visual-studio-code/dotnet-run-command.png)

## <a name="enhance-the-app"></a><span data-ttu-id="b9c79-131">增强应用</span><span class="sxs-lookup"><span data-stu-id="b9c79-131">Enhance the app</span></span>

<span data-ttu-id="b9c79-132">改进应用程序，使其提示用户输入名字，并将其与日期和时间一同显示。</span><span class="sxs-lookup"><span data-stu-id="b9c79-132">Enhance the application to prompt the user for their name and display it along with the date and time.</span></span>

1. <span data-ttu-id="b9c79-133">单击打开 *Program.cs*。</span><span class="sxs-lookup"><span data-stu-id="b9c79-133">Open *Program.cs* by clicking on it.</span></span>

   <span data-ttu-id="b9c79-134">在 Visual Studio Code 中首次打开 C# 文件时，会在编辑器中加载 [OmniSharp](https://www.omnisharp.net/)。</span><span class="sxs-lookup"><span data-stu-id="b9c79-134">The first time you open a C# file in Visual Studio Code, [OmniSharp](https://www.omnisharp.net/) loads in the editor.</span></span>

   ![打开 Program.cs 文件](media/with-visual-studio-code/open-program-cs.png)

1. <span data-ttu-id="b9c79-136">Visual Studio Code 提示添加缺少的资产时选择“是”，以生成和调试应用。</span><span class="sxs-lookup"><span data-stu-id="b9c79-136">Select **Yes** when Visual Studio Code prompts you to add the missing assets to build and debug your app.</span></span>

   ![提示添加缺少的资产](media/with-visual-studio-code/missing-assets.png)

1. <span data-ttu-id="b9c79-138">将 Program.cs 中 `Main` 方法的内容（当前只是调用 `Console.WriteLine` 的行）替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="b9c79-138">Replace the contents of the `Main` method in *Program.cs*, which is the line that calls `Console.WriteLine`, with the following code:</span></span>

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::

   <span data-ttu-id="b9c79-139">此代码会在控制台窗口中显示一条提示，然后等待用户输入字符串并按 <kbd>Enter</kbd>。</span><span class="sxs-lookup"><span data-stu-id="b9c79-139">This code displays a prompt in the console window and waits until the user enters a string followed by the <kbd>Enter</kbd> key.</span></span> <span data-ttu-id="b9c79-140">它会将此字符串存储到名为 `name` 的变量中。</span><span class="sxs-lookup"><span data-stu-id="b9c79-140">It stores this string in a variable named `name`.</span></span> <span data-ttu-id="b9c79-141">它还会检索 <xref:System.DateTime.Now?displayProperty=nameWithType> 属性的值（其中包含当前的本地时间），并将此值赋给 `date` 变量。</span><span class="sxs-lookup"><span data-stu-id="b9c79-141">It also retrieves the value of the <xref:System.DateTime.Now?displayProperty=nameWithType> property, which contains the current local time, and assigns it to a variable named `date`.</span></span> <span data-ttu-id="b9c79-142">同时会在控制台窗口中显示这些值。</span><span class="sxs-lookup"><span data-stu-id="b9c79-142">And it displays these values in the console window.</span></span> <span data-ttu-id="b9c79-143">最后会在控制台窗口中显示一条提示，并调用 <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> 方法来等待用户输入。</span><span class="sxs-lookup"><span data-stu-id="b9c79-143">Finally, it displays a prompt in the console window and calls the <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> method to wait for user input.</span></span>

   <span data-ttu-id="b9c79-144"><xref:System.Environment.NewLine> 是一种独立于平台和语言的表示换行符的方式。</span><span class="sxs-lookup"><span data-stu-id="b9c79-144"><xref:System.Environment.NewLine> is a platform-independent and language-independent way to represent a line break.</span></span> <span data-ttu-id="b9c79-145">替代方法是在 C# 中使用 `\n` 和在 Visual Basic 中使用 `vbCrLf`。</span><span class="sxs-lookup"><span data-stu-id="b9c79-145">Alternatives are `\n` in C# and `vbCrLf` in Visual Basic.</span></span>

   <span data-ttu-id="b9c79-146">字符串前面的美元符号 (`$`) 使你可以将表达式（如变量名称）放入字符串中的大括号内。</span><span class="sxs-lookup"><span data-stu-id="b9c79-146">The dollar sign (`$`) in front of a string lets you put expressions such as variable names in curly braces in the string.</span></span> <span data-ttu-id="b9c79-147">表达式值将代替表达式插入到字符串中。</span><span class="sxs-lookup"><span data-stu-id="b9c79-147">The expression value is inserted into the string in place of the expression.</span></span> <span data-ttu-id="b9c79-148">此语法称为[内插字符串](../../csharp/language-reference/tokens/interpolated.md)。</span><span class="sxs-lookup"><span data-stu-id="b9c79-148">This syntax is referred to as [interpolated strings](../../csharp/language-reference/tokens/interpolated.md).</span></span>

1. <span data-ttu-id="b9c79-149">保存更改。</span><span class="sxs-lookup"><span data-stu-id="b9c79-149">Save your changes.</span></span>

   > [!IMPORTANT]
   > <span data-ttu-id="b9c79-150">在 Visual Studio Code 中，必须显式保存更改。</span><span class="sxs-lookup"><span data-stu-id="b9c79-150">In Visual Studio Code, you have to explicitly save changes.</span></span> <span data-ttu-id="b9c79-151">与 Visual Studio 不同，生成和运行应用时不会自动保存文件更改。</span><span class="sxs-lookup"><span data-stu-id="b9c79-151">Unlike Visual Studio, file changes are not automatically saved when you build and run an app.</span></span>

1. <span data-ttu-id="b9c79-152">再次运行程序：</span><span class="sxs-lookup"><span data-stu-id="b9c79-152">Run the program again:</span></span>

   ```dotnetcli
   dotnet run
   ```

1. <span data-ttu-id="b9c79-153">出现提示时，输入名称并按 Enter<kbd></kbd> 键。</span><span class="sxs-lookup"><span data-stu-id="b9c79-153">Respond to the prompt by entering a name and pressing the <kbd>Enter</kbd> key.</span></span>

   :::image type="content" source="media/debugging-with-visual-studio-code/run-modified-program.png" alt-text="包含经过修改的程序输出的“终端”窗口":::

1. <span data-ttu-id="b9c79-155">按任意键退出程序。</span><span class="sxs-lookup"><span data-stu-id="b9c79-155">Press any key to exit the program.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9c79-156">其他资源</span><span class="sxs-lookup"><span data-stu-id="b9c79-156">Additional resources</span></span>

- [<span data-ttu-id="b9c79-157">设置 Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="b9c79-157">Setting up Visual Studio Code</span></span>](https://code.visualstudio.com/docs/setup/setup-overview)

## <a name="next-steps"></a><span data-ttu-id="b9c79-158">后续步骤</span><span class="sxs-lookup"><span data-stu-id="b9c79-158">Next steps</span></span>

<span data-ttu-id="b9c79-159">在本教程中，你创建了一个 .NET 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="b9c79-159">In this tutorial, you created a .NET console application.</span></span> <span data-ttu-id="b9c79-160">在下一教程中，你将调试该应用。</span><span class="sxs-lookup"><span data-stu-id="b9c79-160">In the next tutorial, you debug the app.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="b9c79-161">使用 Visual Studio Code 调试 .NET 控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="b9c79-161">Debug a .NET console application using Visual Studio Code</span></span>](debugging-with-visual-studio-code.md)
