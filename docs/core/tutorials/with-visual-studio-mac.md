---
title: 使用 Visual Studio for Mac 创建 .NET 控制台应用程序
description: 了解如何使用 Visual Studio for Mac 创建 .NET 控制台应用程序。
ms.date: 11/30/2020
ms.openlocfilehash: 4add8309338b8618265a66b9e71dab2df38ca8d0
ms.sourcegitcommit: 1d3af230ec30d8d061be7a887f6ba38a530c4ece
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511822"
---
# <a name="tutorial-create-a-net-console-application-using-visual-studio-for-mac"></a>教程：使用 Visual Studio for Mac 创建 .NET 控制台应用程序

本教程演示如何使用 Visual Studio for Mac 创建和运行 .NET 控制台应用程序。

> [!NOTE]
> 你的反馈非常有价值。 有两种方法可以向开发团队提供有关 Visual Studio for Mac 的反馈：
>
> * 在 Visual Studio for Mac 中，从菜单中选择“帮助” > “报告问题”，或从欢迎屏幕中选择“报告问题”，将打开一个窗口，以供填写 bug 报告。 可在[开发人员社区](https://aka.ms/feedback/report?space=41)门户中跟踪自己的反馈。
> * 若要提出建议，从菜单中选择“帮助” > “提供建议”，或从欢迎屏幕中选择“提供建议”，转到 [Visual Studio for Mac 开发人员社区网页](https://aka.ms/feedback/suggest?space=41)。

## <a name="prerequisites"></a>先决条件

* [Visual Studio for Mac 8.8 或更高版本](https://visualstudio.microsoft.com/vs/mac/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link)。 选择用于安装 .NET Core 的选项。 安装 Xamarin 对于 .NET 开发而言是可选项。 有关更多信息，请参见以下资源：

  * [教程：安装 Visual Studio for Mac](/visualstudio/mac/installation)。
  * [支持的 macOS 版本](../install/windows.md)。
  * [Visual Studio for Mac 支持的 .NET 版本](/visualstudio/mac/net-core-support)。

## <a name="create-the-app"></a>创建应用

1. 启动 Visual Studio for Mac。

1. 在“开始”窗口中，选择“新建”。

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-new-project.png" alt-text="Visual Studio for Mac 开始屏幕上的“新建”按钮":::

1. 在“新建项目”对话框中，选择“Web 和控制台”节点下的“应用”。 选择“控制台应用程序”模板，然后选择“下一步” 。

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-new-dialog.png" alt-text="新项目模板列表":::

1. 在“配置新的控制台应用程序”对话框的“目标框架”下拉列表中，选择“.NET 5.0”，然后选择“下一步”。

1. 键入“HelloWorld”作为“项目名称”，然后选择“创建” 。

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-new-options.png" alt-text="“配置新的控制台应用程序”对话框":::

用于创建简单的“Hello World”应用程序的模板。 它会调用 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法来显示“Hello World!” （在终端窗口中）。

模板代码将定义类 `Program`，其中包含一个将 <xref:System.String> 数组用作参数的方法 `Main`：

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

`Main` 是应用程序入口点，同时也是在应用程序启动时由运行时自动调用的方法。 `args` 数组中包含在应用程序启动时提供的所有命令行参数。

## <a name="run-the-app"></a>运行应用

1. 按 <kbd>⌥</kbd><kbd>⌘</kbd><kbd>↵</kbd> (<kbd>option</kbd>+<kbd>command</kbd>+<kbd>enter</kbd>) 以运行应用而不进行调试。

   :::image type="content" source="media/with-visual-studio-mac/visual-studio-mac-output.png" alt-text="终端显示 Hello World!":::

1. 关闭终端窗口。

## <a name="enhance-the-app"></a>增强应用

改进应用程序，使其提示用户输入名字，并将其与日期和时间一同显示。

1. 在 Program.cs 中，将 `Main` 方法的内容（是调用 `Console.WriteLine` 的行）替换为以下代码：

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::

   此代码会在控制台窗口中显示一条提示，然后等待用户输入字符串并按 <kbd>Enter</kbd>。 它会将此字符串存储到名为 `name` 的变量中。 它还会检索 <xref:System.DateTime.Now?displayProperty=nameWithType> 属性的值（其中包含当前的本地时间），并将此值赋给 `date` 变量。 同时会在控制台窗口中显示这些值。 最后会在控制台窗口中显示一条提示，并调用 <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> 方法来等待用户输入。

   <xref:System.Environment.NewLine> 是一种独立于平台和语言的表示换行符的方式。 替代方法是在 C# 中使用 `\n` 和在 Visual Basic 中使用 `vbCrLf`。

   字符串前面的美元符号 (`$`) 使你可以将表达式（如变量名称）放入字符串中的大括号内。 表达式值将代替表达式插入到字符串中。 此语法称为[内插字符串](../../csharp/language-reference/tokens/interpolated.md)。

1. 按 <kbd>⌥</kbd><kbd>⌘</kbd><kbd>↵</kbd> (<kbd>option</kbd>+<kbd>command</kbd>+<kbd>enter</kbd>) 以运行应用。

1. 输入名称并按 <kbd>Enter</kbd>，从而响应提示。

   :::image type="content" source="media/with-visual-studio-mac/hello-world-update.png" alt-text="终端显示经过修改的程序输出":::

1. 关闭终端。

## <a name="next-steps"></a>后续步骤

在本教程中，你创建了一个 .NET 控制台应用程序。 在下一教程中，你将调试该应用。

> [!div class="nextstepaction"]
> [使用 Visual Studio for Mac 调试 .NET 控制台应用程序](debugging-with-visual-studio-mac.md)
