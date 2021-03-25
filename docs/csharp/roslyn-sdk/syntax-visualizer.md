---
title: 使用 Visual Studio 中的 Roslyn 语法可视化工具浏览代码
description: 语法可视化工具提供了可视化工具，用于浏览 .NET Compiler Platform SDK 为代码生成的模型。
ms.date: 03/07/2018
ms.custom: mvc, vs-dotnet
ms.openlocfilehash: 43c69bce93db490fccc3500784623f5736ed935d
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605420"
---
# <a name="explore-code-with-the-roslyn-syntax-visualizer-in-visual-studio"></a>使用 Visual Studio 中的 Roslyn 语法可视化工具浏览代码

本文概述了 .NET Compiler Platform（“Roslyn”）SDK 附带的语法可视化工具。 语法可视化工具是一种帮助用户检查和浏览语法树的工具窗口。 如果要理解想要分析的代码模型，该工具必不可少。 在使用 .NET Compiler Platform（“Roslyn”）SDK 开发应用程序时，它还可以提供调试帮助。 在首次创建分析器时打开该工具。 该可视化工具可帮助你理解 API 所使用的模型。 你也可以使用类似 [SharpLab](https://sharplab.io) 或 [LINQPad](https://www.linqpad.net/) 这样的工具来检查代码和理解语法树。

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

通过阅读本[概述](compiler-api-model.md)文章，熟悉 .NET Compiler Platform SDK 中用到的概念。 其中介绍了语法树、节点、标记和琐事。

## <a name="syntax-visualizer"></a>语法可视化工具

使用“语法可视化工具”可以检查 Visual Studio IDE 当前活动的编辑器窗口中的 C# 或 Visual Basic 代码文件的语法树  。 通过单击“视图” > “其他窗口” > “语法可视化工具”，可以启动可视化工具    。  还可以使用右上角的“快速启动”工具栏  。 键入“语法”，然后应该会显示用于开启语法可视化工具的命令  。

此命令会以浮动工具窗口的形式打开语法可视化工具。 如果没有打开代码编辑器窗口，则显示为空白，如下图所示。

![语法可视化工具窗口](media/syntax-visualizer/syntax-visualizer.png)

将此工具窗口停靠在 Visual Studio 中方便操作的位置，例如左侧。 可视化工具显示关于当前代码文件的信息。

使用 File > New Project 命令新建项目   。 可以创建 Visual Basic 项目或 C# 项目。 当 Visual Studio 打开此项目的主代码文件时，可视化工具会显示它的语法树。 可以打开此 Visual Studio 实例中的任何现有 C#/Visual Basic 文件，可视化工具会显示该文件的语法树。 如果在 Visual Studio 中打开了多个代码文件，可视化工具会显示当前活动的代码文件（键盘焦点所在的代码文件）的语法树。

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[C#](#tab/csharp)

![将 C# 语法树可视化](media/syntax-visualizer/visualize-csharp.png)

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

![将 Visual Basic 语法树可视化](media/syntax-visualizer/visualize-visual-basic.png)

---

如前面的图像所示，可视化工具窗口在顶部显示语法树，在底部显示属性网格。 属性网格显示当前在树中选中的项的属性，包括项的 .NET 类型和种类（SyntaxKind）   。

语法树包含三种类型的项：节点、标记和琐事    。 可以在[使用语法](work-with-syntax.md)一文中阅读更多关于这些类型的内容。 每个类型的项都会用不同的颜色表示。 单击“图例”按钮，概览所使用的颜色。

树中的每个项还会显示各自的范围  。 范围就是文本文件中的节点的索引（起始位置和结束位置）  。  在前面的 C# 示例中，所选择的“UsingKeyword [0..5)”标记的范围宽度为 5 个字符，即 [0..5)  。 “[..)”表示起始索引是范围的一部分，而结束索引并不包含在内。

在树中进行导航有两种方式：

* 展开或单击树中的项。 可视化工具自动选择与代码编辑器中的项的范围对应的文本。
* 单击或选择代码编辑器中的文本。 在前面的 Visual Basic 示例中，如果在代码编辑器中选择了包含“Module Module1”的那一行，则可视化工具会在树中自动导航至对应的 ModuleStatement 节点。

可视化工具会突出显示树中的项，该项的范围与编辑器中所选择的文本的范围最匹配。

可视化工具会刷新树，以匹配活动代码文件中的修改。 将调用添加至 `Main()`.中的 `Console.WriteLine()`。 键入内容时，可视化工具会刷新树。

请在键入 `Console.` 后暂停键入。 树中有一些粉色的项。 这说明在所键入的代码中存在错误（通常也成为“诊断”）。 这些错误会附加到语法树的节点、标记和琐碎内容中。 可视化工具会显示哪些项存在错误，并用粉色突出显示其背景。 将鼠标悬停在该项上可以查看任何粉色项的错误。 可视化工具只显示语法错误（这些错误与键入代码的语法相关）；不会显示任何语义错误。

## <a name="syntax-graphs"></a>语法关系图

右键单击树中的任何项，然后单击“查看定向语法关系图”  。

# <a name="c"></a>[C#](#tab/csharp)

可视化工具会以图解形式显示以所选项为根的关系子树。 针对 C# 示例中对应于 `Main()` 方法的 MethodDeclaration 节点，尝试以下步骤  。 可视化工具显示如下所示的语法关系图：

![查看 C# 语法关系图](media/syntax-visualizer/csharp-syntax-graph.png)

# <a name="visual-basic"></a>[Visual Basic](#tab/vb)

针对前面的 Visual Basic 示例中对应于 `Main()` 方法的 SubBlock 节点，尝试相同的操作  。 可视化工具显示如下所示的语法关系图：

![查看 Visual Basic 语法关系图](media/syntax-visualizer/visual-basic-syntax-graph.png)

---

语法关系图查看器中有一个选项，可用于显示其着色方案的图例。 还可以将鼠标悬停在语法关系图中的每个项上，以查看该项对应的属性。

可以重复查看树中不同项的语法关系图，这些关系图会始终显示在 Visual Studio 的同一窗口中。 可以将此窗口停靠在 Visual Studio 中方便操作的位置，这样在查看新的语法关系图时就不需要在选项卡之间切换了。 通常放在底部（代码编辑器窗口的下面）会比较方便。

下面是可视化工具窗口以及语法关系图窗口所采用的停靠布局：

![可视化工具和语法关系图窗口的一个停靠布局](media/syntax-visualizer/docking-layout.png)

另一种选择在双监视器配置中，将语法关系图窗口放在第二个监视器上。

## <a name="inspecting-semantics"></a>检查语义

语法可视化工具可以对符号和语义信息进行基本检查。 在 C# 示例中的 Main() 内键入 `double x = 1 + 1;`。 然后在代码编辑器窗口中选择表达式 `1 + 1`。 可视化工具突出显示了 AddExpression 节点  。 右键单击 AddExpression，然后单击“查看符号(如果有)”   。 请注意，大部分菜单项都带有“如果有”这个限定条件。 语法可视化工具检查节点的属性，包括不是所有节点都有的属性。

可视化工具中的属性网格更新如下图所示：该表达式的符号是 SynthesizedIntrinsicOperatorSymbol  ，其中种类 = 方法  。

![Syntax Visualizer 中的符号属性](media/syntax-visualizer/symbol-properties.png)

针对同一个 AddExpression 节点，请尝试“查看 TypeSymbol (如果有)”   。 如下图所示，可视化工具中的属性网格已更新，指示所选表达式的类型为 `Int32`。

![TypeSymbol 属性](media/syntax-visualizer/type-symbol-properties.png)

针对同一个 AddExpression 节点，请尝试“查看转换后的 TypeSymbol (如果有)”   。 属性网格的更新内容指示虽然表达式的类型为 `Int32`，但转换后的表达式类型为 `Double`，如下图所示。 此节点包含转换后的类型符号信息，因为 `Int32` 表达式所在的上下文要求必须转换为 `Double` 型。 此转换满足了为赋值运算符左侧的变量 `x` 指定的类型为 `Double` 型的要求。

![转换后的 TypeSymbol 属性](media/syntax-visualizer/converted-type-symbol-properties.png)

最后，针对同一 AddExpression 节点，尝试“查看常数值(如果有)”   。 属性网格显示该表达式的值是一个值为 `2` 的编译时常数。

![一个常数值](media/syntax-visualizer/constant-value.png)

在 Visual Basic 中也可以重复上述示例。 在 Visual Basic 文件中键入 `Dim x As Double = 1 + 1`。 在代码编辑器窗口中选择表达式 `1 + 1`。 可视化工具突出显示了对应的 AddExpression 节点  。 对此 AddExpression 节点重复前面所述的步骤，应该能看到相同的结果  。

检查 Visual Basic 中的更多代码。 使用以下代码更新主 Visual Basic 文件：

```vb
Imports C = System.Console

Module Program
    Sub Main(args As String())
        C.WriteLine()
    End Sub
End Module
```

此代码引入了映射到文件顶部的 `System.Console` 类型的 `C` 别名，并在 `Main()` 内使用此别名。 选择在 `Main()` 方法的 `C.WriteLine()` 中使用此别名 `C`。 可视化工具会选择对应的 IdentifierName 节点  。 右键单击此节点，并单击“查看符号(如果有)”  。 属性网格指示此标识符绑定至 `System.Console` 类型，如下图所示：

![Syntax Visualizer 中的符号“C”的属性](media/syntax-visualizer/symbol-visual-basic.png)

针对同一 IdentifierName 节点，尝试“查看 AliasSymbol (如果有)”   。 属性网格指示该标识符为绑定至 `System.Console` 目标的别名 `C`。 换而言之，属性网格会提供对应于标识符 `C` 的 AliasSymbol 的相关信息  。

![AliasSymbol 属性](media/syntax-visualizer/alias-symbol.png)

检查对应于任何声明的类型、方法和属性的符号。 在可视化工具中选择对应节点，单击“查看符号(如果有)”  。 选择 `Sub Main()` 方法，包括该方法的正文。 针对可视化工具中对应的 SubBlock 节点，单击“查看符号(如果有)”   。 属性网格显示此 SubBlock 节点的 MethodSymbol 的名称为 `Main`，返回类型为 `Void`  。

![查看方法声明的符号](media/syntax-visualizer/method-symbol.png)

在 C# 中可以轻松重复上述 Visual Basic 示例。 为别名键入 `using C = System.Console;` 以代替 `Imports C = System.Console`。 在 C# 中完成的上述步骤会在可视化工具窗口中产生相同的结果。

语义检查操作只能用于节点。 不能用于标记和琐事。 并非所有节点都有相关的语义信息可供检查。 如果某个节点不具备相关的语义信息，单击“查看 \* 符号(如果有)”会显示空白的属性网格  。

可阅读[使用语义](work-with-semantics.md)概述文档，详细了解执行语义分析的 API。

## <a name="closing-the-syntax-visualizer"></a>关闭语法可视化工具

在不使用可视化工具窗口检查源代码时，可以关闭该窗口。 当你在浏览代码、编辑和更改源时，语法可视化工具会更新显示的内容。 在你没有使用它的时候，这种情况会分散人的注意力。
