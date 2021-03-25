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
# <a name="explore-code-with-the-roslyn-syntax-visualizer-in-visual-studio"></a><span data-ttu-id="682b9-103">使用 Visual Studio 中的 Roslyn 语法可视化工具浏览代码</span><span class="sxs-lookup"><span data-stu-id="682b9-103">Explore code with the Roslyn syntax visualizer in Visual Studio</span></span>

<span data-ttu-id="682b9-104">本文概述了 .NET Compiler Platform（“Roslyn”）SDK 附带的语法可视化工具。</span><span class="sxs-lookup"><span data-stu-id="682b9-104">This article provides an overview of the Syntax Visualizer tool that ships as part of the .NET Compiler Platform ("Roslyn") SDK.</span></span> <span data-ttu-id="682b9-105">语法可视化工具是一种帮助用户检查和浏览语法树的工具窗口。</span><span class="sxs-lookup"><span data-stu-id="682b9-105">The Syntax Visualizer is a tool window that helps you inspect and explore syntax trees.</span></span> <span data-ttu-id="682b9-106">如果要理解想要分析的代码模型，该工具必不可少。</span><span class="sxs-lookup"><span data-stu-id="682b9-106">It's an essential tool to understand the models for code you want to analyze.</span></span> <span data-ttu-id="682b9-107">在使用 .NET Compiler Platform（“Roslyn”）SDK 开发应用程序时，它还可以提供调试帮助。</span><span class="sxs-lookup"><span data-stu-id="682b9-107">It's also a debugging aid when you develop your own applications using the .NET Compiler Platform (“Roslyn”) SDK.</span></span> <span data-ttu-id="682b9-108">在首次创建分析器时打开该工具。</span><span class="sxs-lookup"><span data-stu-id="682b9-108">Open this tool as you create your first analyzers.</span></span> <span data-ttu-id="682b9-109">该可视化工具可帮助你理解 API 所使用的模型。</span><span class="sxs-lookup"><span data-stu-id="682b9-109">The visualizer helps you understand the models used by the APIs.</span></span> <span data-ttu-id="682b9-110">你也可以使用类似 [SharpLab](https://sharplab.io) 或 [LINQPad](https://www.linqpad.net/) 这样的工具来检查代码和理解语法树。</span><span class="sxs-lookup"><span data-stu-id="682b9-110">You can also use tools like [SharpLab](https://sharplab.io) or [LINQPad](https://www.linqpad.net/) to inspect code and understand syntax trees.</span></span>

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

<span data-ttu-id="682b9-111">通过阅读本[概述](compiler-api-model.md)文章，熟悉 .NET Compiler Platform SDK 中用到的概念。</span><span class="sxs-lookup"><span data-stu-id="682b9-111">Familiarize yourself with the concepts used in the .NET Compiler Platform SDK by reading the [overview](compiler-api-model.md) article.</span></span> <span data-ttu-id="682b9-112">其中介绍了语法树、节点、标记和琐事。</span><span class="sxs-lookup"><span data-stu-id="682b9-112">It provides an introduction to syntax trees, nodes, tokens, and trivia.</span></span>

## <a name="syntax-visualizer"></a><span data-ttu-id="682b9-113">语法可视化工具</span><span class="sxs-lookup"><span data-stu-id="682b9-113">Syntax Visualizer</span></span>

<span data-ttu-id="682b9-114">使用“语法可视化工具”可以检查 Visual Studio IDE 当前活动的编辑器窗口中的 C# 或 Visual Basic 代码文件的语法树  。</span><span class="sxs-lookup"><span data-stu-id="682b9-114">The **Syntax Visualizer** enables inspection of the syntax tree for the C# or Visual Basic code file in the current active editor window inside the Visual Studio IDE.</span></span> <span data-ttu-id="682b9-115">通过单击“视图” > “其他窗口” > “语法可视化工具”，可以启动可视化工具    。</span><span class="sxs-lookup"><span data-stu-id="682b9-115">The visualizer can be launched by clicking on **View** > **Other Windows** > **Syntax Visualizer**.</span></span>  <span data-ttu-id="682b9-116">还可以使用右上角的“快速启动”工具栏  。</span><span class="sxs-lookup"><span data-stu-id="682b9-116">You can also use the **Quick Launch** toolbar in the upper right corner.</span></span> <span data-ttu-id="682b9-117">键入“语法”，然后应该会显示用于开启语法可视化工具的命令  。</span><span class="sxs-lookup"><span data-stu-id="682b9-117">Type "syntax", and the command to open the **Syntax Visualizer** should appear.</span></span>

<span data-ttu-id="682b9-118">此命令会以浮动工具窗口的形式打开语法可视化工具。</span><span class="sxs-lookup"><span data-stu-id="682b9-118">This command opens the Syntax Visualizer as a floating tool window.</span></span> <span data-ttu-id="682b9-119">如果没有打开代码编辑器窗口，则显示为空白，如下图所示。</span><span class="sxs-lookup"><span data-stu-id="682b9-119">If you don't have a code editor window open, the display is blank, as shown in the following figure.</span></span>

![语法可视化工具窗口](media/syntax-visualizer/syntax-visualizer.png)

<span data-ttu-id="682b9-121">将此工具窗口停靠在 Visual Studio 中方便操作的位置，例如左侧。</span><span class="sxs-lookup"><span data-stu-id="682b9-121">Dock this tool window at a convenient location inside Visual Studio, such as the left side.</span></span> <span data-ttu-id="682b9-122">可视化工具显示关于当前代码文件的信息。</span><span class="sxs-lookup"><span data-stu-id="682b9-122">The Visualizer shows information about the current code file.</span></span>

<span data-ttu-id="682b9-123">使用 File > New Project 命令新建项目   。</span><span class="sxs-lookup"><span data-stu-id="682b9-123">Create a new project using the **File** > **New Project** command.</span></span> <span data-ttu-id="682b9-124">可以创建 Visual Basic 项目或 C# 项目。</span><span class="sxs-lookup"><span data-stu-id="682b9-124">You can create either a Visual Basic or C# project.</span></span> <span data-ttu-id="682b9-125">当 Visual Studio 打开此项目的主代码文件时，可视化工具会显示它的语法树。</span><span class="sxs-lookup"><span data-stu-id="682b9-125">When Visual Studio opens the main code file for this project, the visualizer displays the syntax tree for it.</span></span> <span data-ttu-id="682b9-126">可以打开此 Visual Studio 实例中的任何现有 C#/Visual Basic 文件，可视化工具会显示该文件的语法树。</span><span class="sxs-lookup"><span data-stu-id="682b9-126">You can open any existing C# / Visual Basic file in this Visual Studio instance, and the visualizer displays that file's syntax tree.</span></span> <span data-ttu-id="682b9-127">如果在 Visual Studio 中打开了多个代码文件，可视化工具会显示当前活动的代码文件（键盘焦点所在的代码文件）的语法树。</span><span class="sxs-lookup"><span data-stu-id="682b9-127">If you have multiple code files open inside Visual Studio, the visualizer displays the syntax tree for the currently active code file, (the code file that has keyboard focus.)</span></span>

<!-- markdownlint-disable MD025 -->

# <a name="c"></a>[<span data-ttu-id="682b9-128">C#</span><span class="sxs-lookup"><span data-stu-id="682b9-128">C#</span></span>](#tab/csharp)

![将 C# 语法树可视化](media/syntax-visualizer/visualize-csharp.png)

# <a name="visual-basic"></a>[<span data-ttu-id="682b9-130">Visual Basic</span><span class="sxs-lookup"><span data-stu-id="682b9-130">Visual Basic</span></span>](#tab/vb)

![将 Visual Basic 语法树可视化](media/syntax-visualizer/visualize-visual-basic.png)

---

<span data-ttu-id="682b9-132">如前面的图像所示，可视化工具窗口在顶部显示语法树，在底部显示属性网格。</span><span class="sxs-lookup"><span data-stu-id="682b9-132">As shown in the preceding images, the visualizer tool window displays the syntax tree at the top and a property grid at the bottom.</span></span> <span data-ttu-id="682b9-133">属性网格显示当前在树中选中的项的属性，包括项的 .NET 类型和种类（SyntaxKind）   。</span><span class="sxs-lookup"><span data-stu-id="682b9-133">The property grid displays the properties of the item that is currently selected in the tree, including the .NET *Type* and the *Kind* (SyntaxKind) of the item.</span></span>

<span data-ttu-id="682b9-134">语法树包含三种类型的项：节点、标记和琐事    。</span><span class="sxs-lookup"><span data-stu-id="682b9-134">Syntax trees comprise three types of items – *nodes*, *tokens*, and *trivia*.</span></span> <span data-ttu-id="682b9-135">可以在[使用语法](work-with-syntax.md)一文中阅读更多关于这些类型的内容。</span><span class="sxs-lookup"><span data-stu-id="682b9-135">You can read more about these types in the [Work with syntax](work-with-syntax.md) article.</span></span> <span data-ttu-id="682b9-136">每个类型的项都会用不同的颜色表示。</span><span class="sxs-lookup"><span data-stu-id="682b9-136">Items of each type are represented using a different color.</span></span> <span data-ttu-id="682b9-137">单击“图例”按钮，概览所使用的颜色。</span><span class="sxs-lookup"><span data-stu-id="682b9-137">Click on the ‘Legend’ button for an overview of the colors used.</span></span>

<span data-ttu-id="682b9-138">树中的每个项还会显示各自的范围  。</span><span class="sxs-lookup"><span data-stu-id="682b9-138">Each item in the tree also displays its own **span**.</span></span> <span data-ttu-id="682b9-139">范围就是文本文件中的节点的索引（起始位置和结束位置）  。</span><span class="sxs-lookup"><span data-stu-id="682b9-139">The **span** is the indices (the starting and ending position) of that node in the text file.</span></span>  <span data-ttu-id="682b9-140">在前面的 C# 示例中，所选择的“UsingKeyword [0..5)”标记的范围宽度为 5 个字符，即 [0..5)  。</span><span class="sxs-lookup"><span data-stu-id="682b9-140">In the preceding C# example, the selected “UsingKeyword [0..5)” token has a **Span** that is five characters wide, [0..5).</span></span> <span data-ttu-id="682b9-141">“[..)”表示起始索引是范围的一部分，而结束索引并不包含在内。</span><span class="sxs-lookup"><span data-stu-id="682b9-141">The "[..)" notation means that the starting index is part of the span, but the ending index is not.</span></span>

<span data-ttu-id="682b9-142">在树中进行导航有两种方式：</span><span class="sxs-lookup"><span data-stu-id="682b9-142">There are two ways to navigate the tree:</span></span>

* <span data-ttu-id="682b9-143">展开或单击树中的项。</span><span class="sxs-lookup"><span data-stu-id="682b9-143">Expand or click on items in the tree.</span></span> <span data-ttu-id="682b9-144">可视化工具自动选择与代码编辑器中的项的范围对应的文本。</span><span class="sxs-lookup"><span data-stu-id="682b9-144">The visualizer automatically selects the text corresponding to this item’s span in the code editor.</span></span>
* <span data-ttu-id="682b9-145">单击或选择代码编辑器中的文本。</span><span class="sxs-lookup"><span data-stu-id="682b9-145">Click or select text in the code editor.</span></span> <span data-ttu-id="682b9-146">在前面的 Visual Basic 示例中，如果在代码编辑器中选择了包含“Module Module1”的那一行，则可视化工具会在树中自动导航至对应的 ModuleStatement 节点。</span><span class="sxs-lookup"><span data-stu-id="682b9-146">In the preceding Visual Basic example, if you select the line containing "Module Module1" in the code editor, the visualizer automatically navigates to the corresponding ModuleStatement node in the tree.</span></span>

<span data-ttu-id="682b9-147">可视化工具会突出显示树中的项，该项的范围与编辑器中所选择的文本的范围最匹配。</span><span class="sxs-lookup"><span data-stu-id="682b9-147">The visualizer highlights the item in the tree whose span best matches the span of the text selected in the editor.</span></span>

<span data-ttu-id="682b9-148">可视化工具会刷新树，以匹配活动代码文件中的修改。</span><span class="sxs-lookup"><span data-stu-id="682b9-148">The visualizer refreshes the tree to match modifications in the active code file.</span></span> <span data-ttu-id="682b9-149">将调用添加至 `Main()`.中的 `Console.WriteLine()`。</span><span class="sxs-lookup"><span data-stu-id="682b9-149">Add a call to `Console.WriteLine()` inside `Main()`.</span></span> <span data-ttu-id="682b9-150">键入内容时，可视化工具会刷新树。</span><span class="sxs-lookup"><span data-stu-id="682b9-150">As you type, the visualizer refreshes the tree.</span></span>

<span data-ttu-id="682b9-151">请在键入 `Console.` 后暂停键入。</span><span class="sxs-lookup"><span data-stu-id="682b9-151">Pause typing once you have typed `Console.`.</span></span> <span data-ttu-id="682b9-152">树中有一些粉色的项。</span><span class="sxs-lookup"><span data-stu-id="682b9-152">The tree has some items colored in pink.</span></span> <span data-ttu-id="682b9-153">这说明在所键入的代码中存在错误（通常也成为“诊断”）。</span><span class="sxs-lookup"><span data-stu-id="682b9-153">At this point, there are errors (also referred to as ‘Diagnostics’) in the typed code.</span></span> <span data-ttu-id="682b9-154">这些错误会附加到语法树的节点、标记和琐碎内容中。</span><span class="sxs-lookup"><span data-stu-id="682b9-154">These errors are attached to nodes, tokens, and trivia in the syntax tree.</span></span> <span data-ttu-id="682b9-155">可视化工具会显示哪些项存在错误，并用粉色突出显示其背景。</span><span class="sxs-lookup"><span data-stu-id="682b9-155">The visualizer shows you which items have errors attached to them highlighting the background in pink.</span></span> <span data-ttu-id="682b9-156">将鼠标悬停在该项上可以查看任何粉色项的错误。</span><span class="sxs-lookup"><span data-stu-id="682b9-156">You can inspect the errors on any item colored pink by hovering over the item.</span></span> <span data-ttu-id="682b9-157">可视化工具只显示语法错误（这些错误与键入代码的语法相关）；不会显示任何语义错误。</span><span class="sxs-lookup"><span data-stu-id="682b9-157">The visualizer only displays syntactic errors (those errors related to the syntax of the typed code); it doesn't display any semantic errors.</span></span>

## <a name="syntax-graphs"></a><span data-ttu-id="682b9-158">语法关系图</span><span class="sxs-lookup"><span data-stu-id="682b9-158">Syntax Graphs</span></span>

<span data-ttu-id="682b9-159">右键单击树中的任何项，然后单击“查看定向语法关系图”  。</span><span class="sxs-lookup"><span data-stu-id="682b9-159">Right click on any item in the tree and click on **View Directed Syntax Graph**.</span></span>

# <a name="c"></a>[<span data-ttu-id="682b9-160">C#</span><span class="sxs-lookup"><span data-stu-id="682b9-160">C#</span></span>](#tab/csharp)

<span data-ttu-id="682b9-161">可视化工具会以图解形式显示以所选项为根的关系子树。</span><span class="sxs-lookup"><span data-stu-id="682b9-161">The visualizer displays a graphical representation of the subtree rooted at the selected item.</span></span> <span data-ttu-id="682b9-162">针对 C# 示例中对应于 `Main()` 方法的 MethodDeclaration 节点，尝试以下步骤  。</span><span class="sxs-lookup"><span data-stu-id="682b9-162">Try these steps for the **MethodDeclaration** node corresponding to the `Main()` method in the C# example.</span></span> <span data-ttu-id="682b9-163">可视化工具显示如下所示的语法关系图：</span><span class="sxs-lookup"><span data-stu-id="682b9-163">The visualizer displays a syntax graph that looks as follows:</span></span>

![查看 C# 语法关系图](media/syntax-visualizer/csharp-syntax-graph.png)

# <a name="visual-basic"></a>[<span data-ttu-id="682b9-165">Visual Basic</span><span class="sxs-lookup"><span data-stu-id="682b9-165">Visual Basic</span></span>](#tab/vb)

<span data-ttu-id="682b9-166">针对前面的 Visual Basic 示例中对应于 `Main()` 方法的 SubBlock 节点，尝试相同的操作  。</span><span class="sxs-lookup"><span data-stu-id="682b9-166">Try the same for the **SubBlock** node corresponding to the `Main()` method in the preceding Visual Basic example.</span></span> <span data-ttu-id="682b9-167">可视化工具显示如下所示的语法关系图：</span><span class="sxs-lookup"><span data-stu-id="682b9-167">The visualizer displays a syntax graph that looks as follows:</span></span>

![查看 Visual Basic 语法关系图](media/syntax-visualizer/visual-basic-syntax-graph.png)

---

<span data-ttu-id="682b9-169">语法关系图查看器中有一个选项，可用于显示其着色方案的图例。</span><span class="sxs-lookup"><span data-stu-id="682b9-169">The syntax graph viewer has an option to display a legend for its coloring scheme.</span></span> <span data-ttu-id="682b9-170">还可以将鼠标悬停在语法关系图中的每个项上，以查看该项对应的属性。</span><span class="sxs-lookup"><span data-stu-id="682b9-170">You can also hover over individual items in the syntax graph with the mouse to view the properties corresponding to that item.</span></span>

<span data-ttu-id="682b9-171">可以重复查看树中不同项的语法关系图，这些关系图会始终显示在 Visual Studio 的同一窗口中。</span><span class="sxs-lookup"><span data-stu-id="682b9-171">You can view syntax graphs for different items in the tree repeatedly and the graphs will always be displayed in the same window inside Visual Studio.</span></span> <span data-ttu-id="682b9-172">可以将此窗口停靠在 Visual Studio 中方便操作的位置，这样在查看新的语法关系图时就不需要在选项卡之间切换了。</span><span class="sxs-lookup"><span data-stu-id="682b9-172">You can dock this window at a convenient location inside Visual Studio so that you don’t have to switch between tabs to view a new syntax graph.</span></span> <span data-ttu-id="682b9-173">通常放在底部（代码编辑器窗口的下面）会比较方便。</span><span class="sxs-lookup"><span data-stu-id="682b9-173">The bottom, below code editor windows, is often convenient.</span></span>

<span data-ttu-id="682b9-174">下面是可视化工具窗口以及语法关系图窗口所采用的停靠布局：</span><span class="sxs-lookup"><span data-stu-id="682b9-174">Here is the docking layout to use with the visualizer tool window and the syntax graph window:</span></span>

![可视化工具和语法关系图窗口的一个停靠布局](media/syntax-visualizer/docking-layout.png)

<span data-ttu-id="682b9-176">另一种选择在双监视器配置中，将语法关系图窗口放在第二个监视器上。</span><span class="sxs-lookup"><span data-stu-id="682b9-176">Another option is to put the syntax graph window on a second monitor, in a dual monitor setup.</span></span>

## <a name="inspecting-semantics"></a><span data-ttu-id="682b9-177">检查语义</span><span class="sxs-lookup"><span data-stu-id="682b9-177">Inspecting semantics</span></span>

<span data-ttu-id="682b9-178">语法可视化工具可以对符号和语义信息进行基本检查。</span><span class="sxs-lookup"><span data-stu-id="682b9-178">The Syntax Visualizer enables rudimentary inspection of symbols and semantic information.</span></span> <span data-ttu-id="682b9-179">在 C# 示例中的 Main() 内键入 `double x = 1 + 1;`。</span><span class="sxs-lookup"><span data-stu-id="682b9-179">Type `double x = 1 + 1;` inside Main() in the C# example.</span></span> <span data-ttu-id="682b9-180">然后在代码编辑器窗口中选择表达式 `1 + 1`。</span><span class="sxs-lookup"><span data-stu-id="682b9-180">Then, select the expression `1 + 1` in the code editor window.</span></span> <span data-ttu-id="682b9-181">可视化工具突出显示了 AddExpression 节点  。</span><span class="sxs-lookup"><span data-stu-id="682b9-181">The visualizer highlights the **AddExpression** node in the visualizer.</span></span> <span data-ttu-id="682b9-182">右键单击 AddExpression，然后单击“查看符号(如果有)”   。</span><span class="sxs-lookup"><span data-stu-id="682b9-182">Right click on this **AddExpression** and click on **View Symbol (if any)**.</span></span> <span data-ttu-id="682b9-183">请注意，大部分菜单项都带有“如果有”这个限定条件。</span><span class="sxs-lookup"><span data-stu-id="682b9-183">Notice that most of the menu items have the "if any" qualifier.</span></span> <span data-ttu-id="682b9-184">语法可视化工具检查节点的属性，包括不是所有节点都有的属性。</span><span class="sxs-lookup"><span data-stu-id="682b9-184">The Syntax Visualizer inspects properties of a Node, including properties that may not be present for all nodes.</span></span>

<span data-ttu-id="682b9-185">可视化工具中的属性网格更新如下图所示：该表达式的符号是 SynthesizedIntrinsicOperatorSymbol  ，其中种类 = 方法  。</span><span class="sxs-lookup"><span data-stu-id="682b9-185">The property grid in the visualizer updates as shown in the following figure: The symbol for the expression is a **SynthesizedIntrinsicOperatorSymbol** with **Kind = Method**.</span></span>

![Syntax Visualizer 中的符号属性](media/syntax-visualizer/symbol-properties.png)

<span data-ttu-id="682b9-187">针对同一个 AddExpression 节点，请尝试“查看 TypeSymbol (如果有)”   。</span><span class="sxs-lookup"><span data-stu-id="682b9-187">Try **View TypeSymbol (if any)** for the same **AddExpression** node.</span></span> <span data-ttu-id="682b9-188">如下图所示，可视化工具中的属性网格已更新，指示所选表达式的类型为 `Int32`。</span><span class="sxs-lookup"><span data-stu-id="682b9-188">The property grid in the visualizer updates as shown in the following figure, indicating that the type of the selected expression is `Int32`.</span></span>

![TypeSymbol 属性](media/syntax-visualizer/type-symbol-properties.png)

<span data-ttu-id="682b9-190">针对同一个 AddExpression 节点，请尝试“查看转换后的 TypeSymbol (如果有)”   。</span><span class="sxs-lookup"><span data-stu-id="682b9-190">Try **View Converted TypeSymbol (if any)** for the same **AddExpression** node.</span></span> <span data-ttu-id="682b9-191">属性网格的更新内容指示虽然表达式的类型为 `Int32`，但转换后的表达式类型为 `Double`，如下图所示。</span><span class="sxs-lookup"><span data-stu-id="682b9-191">The property grid updates indicating that although the type of the expression is `Int32`, the converted type of the expression is `Double` as shown in the following figure.</span></span> <span data-ttu-id="682b9-192">此节点包含转换后的类型符号信息，因为 `Int32` 表达式所在的上下文要求必须转换为 `Double` 型。</span><span class="sxs-lookup"><span data-stu-id="682b9-192">This node includes converted type symbol information because the `Int32` expression occurs in a context where it must be converted to a `Double`.</span></span> <span data-ttu-id="682b9-193">此转换满足了为赋值运算符左侧的变量 `x` 指定的类型为 `Double` 型的要求。</span><span class="sxs-lookup"><span data-stu-id="682b9-193">This conversion satisfies the `Double` type specified for the variable `x` on the left-hand side of the assignment operator.</span></span>

![转换后的 TypeSymbol 属性](media/syntax-visualizer/converted-type-symbol-properties.png)

<span data-ttu-id="682b9-195">最后，针对同一 AddExpression 节点，尝试“查看常数值(如果有)”   。</span><span class="sxs-lookup"><span data-stu-id="682b9-195">Finally, try **View Constant Value (if any)** for the same **AddExpression** node.</span></span> <span data-ttu-id="682b9-196">属性网格显示该表达式的值是一个值为 `2` 的编译时常数。</span><span class="sxs-lookup"><span data-stu-id="682b9-196">The property grid shows that the value of the expression is a compile time constant with value `2`.</span></span>

![一个常数值](media/syntax-visualizer/constant-value.png)

<span data-ttu-id="682b9-198">在 Visual Basic 中也可以重复上述示例。</span><span class="sxs-lookup"><span data-stu-id="682b9-198">The preceding example can also be replicated in Visual Basic.</span></span> <span data-ttu-id="682b9-199">在 Visual Basic 文件中键入 `Dim x As Double = 1 + 1`。</span><span class="sxs-lookup"><span data-stu-id="682b9-199">Type `Dim x As Double = 1 + 1` in a Visual Basic file.</span></span> <span data-ttu-id="682b9-200">在代码编辑器窗口中选择表达式 `1 + 1`。</span><span class="sxs-lookup"><span data-stu-id="682b9-200">Select the expression `1 + 1` in the code editor window.</span></span> <span data-ttu-id="682b9-201">可视化工具突出显示了对应的 AddExpression 节点  。</span><span class="sxs-lookup"><span data-stu-id="682b9-201">The visualizer highlights the corresponding **AddExpression** node in the visualizer.</span></span> <span data-ttu-id="682b9-202">对此 AddExpression 节点重复前面所述的步骤，应该能看到相同的结果  。</span><span class="sxs-lookup"><span data-stu-id="682b9-202">Repeat the preceding steps for this **AddExpression** and you should see identical results.</span></span>

<span data-ttu-id="682b9-203">检查 Visual Basic 中的更多代码。</span><span class="sxs-lookup"><span data-stu-id="682b9-203">Examine more code in Visual Basic.</span></span> <span data-ttu-id="682b9-204">使用以下代码更新主 Visual Basic 文件：</span><span class="sxs-lookup"><span data-stu-id="682b9-204">Update your main Visual Basic file with the following code:</span></span>

```vb
Imports C = System.Console

Module Program
    Sub Main(args As String())
        C.WriteLine()
    End Sub
End Module
```

<span data-ttu-id="682b9-205">此代码引入了映射到文件顶部的 `System.Console` 类型的 `C` 别名，并在 `Main()` 内使用此别名。</span><span class="sxs-lookup"><span data-stu-id="682b9-205">This code introduces an alias named `C` that maps to the type `System.Console` at the top of the file and uses this alias inside `Main()`.</span></span> <span data-ttu-id="682b9-206">选择在 `Main()` 方法的 `C.WriteLine()` 中使用此别名 `C`。</span><span class="sxs-lookup"><span data-stu-id="682b9-206">Select the use of this alias, the `C` in `C.WriteLine()`, inside the `Main()` method.</span></span> <span data-ttu-id="682b9-207">可视化工具会选择对应的 IdentifierName 节点  。</span><span class="sxs-lookup"><span data-stu-id="682b9-207">The visualizer selects the corresponding **IdentifierName** node in the visualizer.</span></span> <span data-ttu-id="682b9-208">右键单击此节点，并单击“查看符号(如果有)”  。</span><span class="sxs-lookup"><span data-stu-id="682b9-208">Right-click this node and click on **View Symbol (if any)**.</span></span> <span data-ttu-id="682b9-209">属性网格指示此标识符绑定至 `System.Console` 类型，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="682b9-209">The property grid indicates that this identifier is bound to the type `System.Console` as shown in the following figure:</span></span>

![Syntax Visualizer 中的符号“C”的属性](media/syntax-visualizer/symbol-visual-basic.png)

<span data-ttu-id="682b9-211">针对同一 IdentifierName 节点，尝试“查看 AliasSymbol (如果有)”   。</span><span class="sxs-lookup"><span data-stu-id="682b9-211">Try **View AliasSymbol (if any)** for the same **IdentifierName** node.</span></span> <span data-ttu-id="682b9-212">属性网格指示该标识符为绑定至 `System.Console` 目标的别名 `C`。</span><span class="sxs-lookup"><span data-stu-id="682b9-212">The property grid indicates the identifier is an alias with name `C` that is bound to the `System.Console` target.</span></span> <span data-ttu-id="682b9-213">换而言之，属性网格会提供对应于标识符 `C` 的 AliasSymbol 的相关信息  。</span><span class="sxs-lookup"><span data-stu-id="682b9-213">In other words, the property grid provides information regarding the **AliasSymbol** corresponding to the identifier `C`.</span></span>

![AliasSymbol 属性](media/syntax-visualizer/alias-symbol.png)

<span data-ttu-id="682b9-215">检查对应于任何声明的类型、方法和属性的符号。</span><span class="sxs-lookup"><span data-stu-id="682b9-215">Inspect the symbol corresponding to any declared type, method, property.</span></span> <span data-ttu-id="682b9-216">在可视化工具中选择对应节点，单击“查看符号(如果有)”  。</span><span class="sxs-lookup"><span data-stu-id="682b9-216">Select the corresponding node in the visualizer and click on **View Symbol (if any)**.</span></span> <span data-ttu-id="682b9-217">选择 `Sub Main()` 方法，包括该方法的正文。</span><span class="sxs-lookup"><span data-stu-id="682b9-217">Select the method `Sub Main()`, including the body of the method.</span></span> <span data-ttu-id="682b9-218">针对可视化工具中对应的 SubBlock 节点，单击“查看符号(如果有)”   。</span><span class="sxs-lookup"><span data-stu-id="682b9-218">Click on **View Symbol (if any)** for the corresponding **SubBlock** node in the visualizer.</span></span> <span data-ttu-id="682b9-219">属性网格显示此 SubBlock 节点的 MethodSymbol 的名称为 `Main`，返回类型为 `Void`  。</span><span class="sxs-lookup"><span data-stu-id="682b9-219">The property grid shows the **MethodSymbol** for this **SubBlock** has name `Main` with return type `Void`.</span></span>

![查看方法声明的符号](media/syntax-visualizer/method-symbol.png)

<span data-ttu-id="682b9-221">在 C# 中可以轻松重复上述 Visual Basic 示例。</span><span class="sxs-lookup"><span data-stu-id="682b9-221">The above Visual Basic examples can be easily replicated in C#.</span></span> <span data-ttu-id="682b9-222">为别名键入 `using C = System.Console;` 以代替 `Imports C = System.Console`。</span><span class="sxs-lookup"><span data-stu-id="682b9-222">Type `using C = System.Console;` in place of `Imports C = System.Console` for the alias.</span></span> <span data-ttu-id="682b9-223">在 C# 中完成的上述步骤会在可视化工具窗口中产生相同的结果。</span><span class="sxs-lookup"><span data-stu-id="682b9-223">The preceding steps in C# yield identical results in the visualizer window.</span></span>

<span data-ttu-id="682b9-224">语义检查操作只能用于节点。</span><span class="sxs-lookup"><span data-stu-id="682b9-224">Semantic inspection operations are only available on nodes.</span></span> <span data-ttu-id="682b9-225">不能用于标记和琐事。</span><span class="sxs-lookup"><span data-stu-id="682b9-225">They are not available on tokens or trivia.</span></span> <span data-ttu-id="682b9-226">并非所有节点都有相关的语义信息可供检查。</span><span class="sxs-lookup"><span data-stu-id="682b9-226">Not all nodes have interesting semantic information to inspect.</span></span> <span data-ttu-id="682b9-227">如果某个节点不具备相关的语义信息，单击“查看 \* 符号(如果有)”会显示空白的属性网格  。</span><span class="sxs-lookup"><span data-stu-id="682b9-227">When a node doesn't have interesting semantic information, clicking on **View \* Symbol (if any)** shows a blank property grid.</span></span>

<span data-ttu-id="682b9-228">可阅读[使用语义](work-with-semantics.md)概述文档，详细了解执行语义分析的 API。</span><span class="sxs-lookup"><span data-stu-id="682b9-228">You can read more about APIs for performing semantic analysis in the [Work with semantics](work-with-semantics.md) overview document.</span></span>

## <a name="closing-the-syntax-visualizer"></a><span data-ttu-id="682b9-229">关闭语法可视化工具</span><span class="sxs-lookup"><span data-stu-id="682b9-229">Closing the syntax visualizer</span></span>

<span data-ttu-id="682b9-230">在不使用可视化工具窗口检查源代码时，可以关闭该窗口。</span><span class="sxs-lookup"><span data-stu-id="682b9-230">You can close the visualizer window when you are not using it to examine source code.</span></span> <span data-ttu-id="682b9-231">当你在浏览代码、编辑和更改源时，语法可视化工具会更新显示的内容。</span><span class="sxs-lookup"><span data-stu-id="682b9-231">The syntax visualizer updates its display as you navigate through code, editing and changing the source.</span></span> <span data-ttu-id="682b9-232">在你没有使用它的时候，这种情况会分散人的注意力。</span><span class="sxs-lookup"><span data-stu-id="682b9-232">It can get distracting when you are not using it.</span></span>
