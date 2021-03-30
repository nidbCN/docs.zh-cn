---
title: 语法分析 (Roslyn API) 入门
description: 介绍如何遍历、查询及浏览语法树。
ms.date: 02/05/2018
ms.custom: mvc
ms.openlocfilehash: 7fea6145ed927ed197295b08ab35dbf177920061
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872504"
---
# <a name="get-started-with-syntax-analysis"></a><span data-ttu-id="ae786-103">语法分析入门</span><span class="sxs-lookup"><span data-stu-id="ae786-103">Get started with syntax analysis</span></span>

<span data-ttu-id="ae786-104">在本教程中，你将了解“语法 API”  。</span><span class="sxs-lookup"><span data-stu-id="ae786-104">In this tutorial, you'll explore the **Syntax API**.</span></span> <span data-ttu-id="ae786-105">语法 API 提供对描述 C# 或 Visual Basic 程序的数据结构的访问。</span><span class="sxs-lookup"><span data-stu-id="ae786-105">The Syntax API provides access to the data structures that describe a C# or Visual Basic program.</span></span> <span data-ttu-id="ae786-106">这些数据结构具备足够的详细信息，它们可以充分表示任何规模的任何程序。</span><span class="sxs-lookup"><span data-stu-id="ae786-106">These data structures have enough detail that they can fully represent any program of any size.</span></span> <span data-ttu-id="ae786-107">这些结构可以描述准确编译并运行的完整程序。</span><span class="sxs-lookup"><span data-stu-id="ae786-107">These structures can describe complete programs that compile and run correctly.</span></span> <span data-ttu-id="ae786-108">当你在编辑器中编写程序时，它们还可以描述这些尚不完整的程序。</span><span class="sxs-lookup"><span data-stu-id="ae786-108">They can also describe incomplete programs, as you write them, in the editor.</span></span>

<span data-ttu-id="ae786-109">要启用此丰富的表达式，组成语法 API 的数据结构和 API 必然是复杂的。</span><span class="sxs-lookup"><span data-stu-id="ae786-109">To enable this rich expression, the data structures and APIs that make up the Syntax API are necessarily complex.</span></span> <span data-ttu-id="ae786-110">让我们看看数据结构是什么样的，从典型的“Hello World”程序的数据结构开始：</span><span class="sxs-lookup"><span data-stu-id="ae786-110">Let's start with what the data structure looks like for the typical "Hello World" program:</span></span>

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

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

<span data-ttu-id="ae786-111">查看以前程序的文本。</span><span class="sxs-lookup"><span data-stu-id="ae786-111">Look at the text of the previous program.</span></span> <span data-ttu-id="ae786-112">识别熟悉的元素。</span><span class="sxs-lookup"><span data-stu-id="ae786-112">You recognize familiar elements.</span></span> <span data-ttu-id="ae786-113">整个文本代表一个源文件，或者一个“编译单元”  。</span><span class="sxs-lookup"><span data-stu-id="ae786-113">The entire text represents a single source file, or a **compilation unit**.</span></span> <span data-ttu-id="ae786-114">源文件的前三行是 using 指令  。</span><span class="sxs-lookup"><span data-stu-id="ae786-114">The first three lines of that source file are **using directives**.</span></span> <span data-ttu-id="ae786-115">剩余源包含在命名空间声明中  。</span><span class="sxs-lookup"><span data-stu-id="ae786-115">The remaining source is contained in a **namespace declaration**.</span></span> <span data-ttu-id="ae786-116">命名空间声明包含一个子类声明  。</span><span class="sxs-lookup"><span data-stu-id="ae786-116">The namespace declaration contains a child **class declaration**.</span></span> <span data-ttu-id="ae786-117">类声明包含一个方法声明  。</span><span class="sxs-lookup"><span data-stu-id="ae786-117">The class declaration contains one **method declaration**.</span></span>

<span data-ttu-id="ae786-118">语法 API 使用代表编译单元的根创建树结构。</span><span class="sxs-lookup"><span data-stu-id="ae786-118">The Syntax API creates a tree structure with the root representing the compilation unit.</span></span> <span data-ttu-id="ae786-119">树中的节点代表 using 指令、命名空间声明和程序中的所有其他元素。</span><span class="sxs-lookup"><span data-stu-id="ae786-119">Nodes in the tree represent the using directives, namespace declaration and all the other elements of the program.</span></span> <span data-ttu-id="ae786-120">树结构一直到最低级别：字符串“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="ae786-120">The tree structure continues down to the lowest levels: the string "Hello World!"</span></span> <span data-ttu-id="ae786-121">是“字符串文本标记”  ，即一种参数的子代  。</span><span class="sxs-lookup"><span data-stu-id="ae786-121">is a **string literal token** that is a descendent of an **argument**.</span></span> <span data-ttu-id="ae786-122">语法 API 提供对程序结构的访问。</span><span class="sxs-lookup"><span data-stu-id="ae786-122">The Syntax API provides access to the structure of the program.</span></span> <span data-ttu-id="ae786-123">你可以查询特定代码实践、浏览整个树以理解代码并通过修改现有树来新建树。</span><span class="sxs-lookup"><span data-stu-id="ae786-123">You can query for specific code practices, walk the entire tree to understand the code, and create new trees by modifying the existing tree.</span></span>

<span data-ttu-id="ae786-124">该简介概述了使用语法 API 可以访问的信息类型。</span><span class="sxs-lookup"><span data-stu-id="ae786-124">That brief description provides an overview of the kind of information accessible using the Syntax API.</span></span> <span data-ttu-id="ae786-125">语法 API 仅仅是描述你从 C# 中熟悉的代码结构的正式 API。</span><span class="sxs-lookup"><span data-stu-id="ae786-125">The Syntax API is nothing more than a formal API that describes the familiar code constructs you know from C#.</span></span> <span data-ttu-id="ae786-126">完整的功能包括关于设置代码格式（包括换行符、空格和缩进）的信息。</span><span class="sxs-lookup"><span data-stu-id="ae786-126">The full capabilities include information about how the code is formatted including line breaks, white space, and indenting.</span></span> <span data-ttu-id="ae786-127">在人工程序员或编译者编写和读取代码时，你可以使用此信息完整表示代码。</span><span class="sxs-lookup"><span data-stu-id="ae786-127">Using this information, you can fully represent the code as written and read by human programmers or the compiler.</span></span> <span data-ttu-id="ae786-128">使用此结构可以在深有意义的级别上与源代码进行交互。</span><span class="sxs-lookup"><span data-stu-id="ae786-128">Using this structure enables you to interact with the source code on a deeply meaningful level.</span></span> <span data-ttu-id="ae786-129">它不再是文本字符串，而是代表 C# 程序结构的数据。</span><span class="sxs-lookup"><span data-stu-id="ae786-129">It's no longer text strings, but data that represents the structure of a C# program.</span></span>

<span data-ttu-id="ae786-130">若要开始，需要安装 .NET 编译器平台 SDK  ：</span><span class="sxs-lookup"><span data-stu-id="ae786-130">To get started, you'll need to install the **.NET Compiler Platform SDK**:</span></span>

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="understanding-syntax-trees"></a><span data-ttu-id="ae786-131">了解语法树</span><span class="sxs-lookup"><span data-stu-id="ae786-131">Understanding syntax trees</span></span>

<span data-ttu-id="ae786-132">将语法 API 用于 C# 代码结构的所有分析。</span><span class="sxs-lookup"><span data-stu-id="ae786-132">You use the Syntax API for any analysis of the structure of C# code.</span></span> <span data-ttu-id="ae786-133">语法 API 公开分析程序、语法树和用于分析并构造语法树的实用程序  。</span><span class="sxs-lookup"><span data-stu-id="ae786-133">The **Syntax API** exposes the parsers, the syntax trees, and utilities for analyzing and constructing syntax trees.</span></span> <span data-ttu-id="ae786-134">这是搜索特定语法元素的代码或读取程序代码的方式。</span><span class="sxs-lookup"><span data-stu-id="ae786-134">It's how you search code for specific syntax elements or read the code for a program.</span></span>

<span data-ttu-id="ae786-135">语法树是 C# 和 Visual Basic 编译器用于理解 C# 和 Visual Basic 程序的数据结构。</span><span class="sxs-lookup"><span data-stu-id="ae786-135">A syntax tree is a data structure used by the C# and Visual Basic compilers to understand C# and Visual Basic programs.</span></span> <span data-ttu-id="ae786-136">语法树由生成项目时或开发人员按 F5 时所运行的分析程序生成。</span><span class="sxs-lookup"><span data-stu-id="ae786-136">Syntax trees are produced by the same parser that runs when a project is built or a developer hits F5.</span></span> <span data-ttu-id="ae786-137">语法树对语言完全保真；代码文件中的每一位信息都在树中。</span><span class="sxs-lookup"><span data-stu-id="ae786-137">The syntax trees have full-fidelity with the language; every bit of information in a code file is represented in the tree.</span></span> <span data-ttu-id="ae786-138">将语法树写入文本会再现已分析的完全原始文本。</span><span class="sxs-lookup"><span data-stu-id="ae786-138">Writing a syntax tree to text reproduces the exact original text that was parsed.</span></span> <span data-ttu-id="ae786-139">语法树也是不可变的  ；一旦创建语法树，就不能再更改。</span><span class="sxs-lookup"><span data-stu-id="ae786-139">The syntax trees are also **immutable**; once created a syntax tree can never be changed.</span></span> <span data-ttu-id="ae786-140">树的使用者可以在多个线程上对树进行分析，不需要锁或其他并发度量，很清楚数据是不会更改的。</span><span class="sxs-lookup"><span data-stu-id="ae786-140">Consumers of the trees can analyze the trees on multiple threads, without locks or other concurrency measures, knowing the data never changes.</span></span> <span data-ttu-id="ae786-141">可使用 API 新建树，新建的树就是对现有树进行修改后的成果。</span><span class="sxs-lookup"><span data-stu-id="ae786-141">You can use APIs to create new trees that are the result of modifying an existing tree.</span></span>

<span data-ttu-id="ae786-142">语法树的四个主要构建基块为：</span><span class="sxs-lookup"><span data-stu-id="ae786-142">The four primary building blocks of syntax trees are:</span></span>

* <span data-ttu-id="ae786-143"><xref:Microsoft.CodeAnalysis.SyntaxTree?displayProperty=nameWithType> 类，它的实例代表整个分析树。</span><span class="sxs-lookup"><span data-stu-id="ae786-143">The <xref:Microsoft.CodeAnalysis.SyntaxTree?displayProperty=nameWithType> class, an instance of which represents an entire parse tree.</span></span> <span data-ttu-id="ae786-144"><xref:Microsoft.CodeAnalysis.SyntaxTree> 是一种带有语言特定派生类的抽象类。</span><span class="sxs-lookup"><span data-stu-id="ae786-144"><xref:Microsoft.CodeAnalysis.SyntaxTree> is an abstract class that has language-specific derivatives.</span></span> <span data-ttu-id="ae786-145">使用 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxTree?displayProperty=nameWithType>（或 <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicSyntaxTree?displayProperty=nameWithType>）类的分析方法对 C#（或 Visual Basic）中的文本进行分析。</span><span class="sxs-lookup"><span data-stu-id="ae786-145">You use the parse methods of the <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxTree?displayProperty=nameWithType> (or <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicSyntaxTree?displayProperty=nameWithType>) class to parse text in C# (or Visual Basic).</span></span>
* <span data-ttu-id="ae786-146"><xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> 类，它的实例表示声明、语句、子句和表达式等语法构造。</span><span class="sxs-lookup"><span data-stu-id="ae786-146">The <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> class, instances of which represent syntactic constructs such as declarations, statements, clauses, and expressions.</span></span>
* <span data-ttu-id="ae786-147"><xref:Microsoft.CodeAnalysis.SyntaxToken?displayProperty=nameWithType> 结构，它代表独立的关键词、标识符、运算符或标点。</span><span class="sxs-lookup"><span data-stu-id="ae786-147">The <xref:Microsoft.CodeAnalysis.SyntaxToken?displayProperty=nameWithType> structure, which represents an individual keyword, identifier, operator, or punctuation.</span></span>
* <span data-ttu-id="ae786-148">最后是 <xref:Microsoft.CodeAnalysis.SyntaxTrivia?displayProperty=nameWithType> 结构，它代表语法上不重要的信息，例如标记、预处理指令和注释之间的空格。</span><span class="sxs-lookup"><span data-stu-id="ae786-148">And lastly the <xref:Microsoft.CodeAnalysis.SyntaxTrivia?displayProperty=nameWithType> structure, which represents syntactically insignificant bits of information such as the white space between tokens, preprocessing directives, and comments.</span></span>

<span data-ttu-id="ae786-149">琐事、标记和节点分层次地构成了树，树完全代表了 Visual Basic 或 C# 代码片段中的所有内容。</span><span class="sxs-lookup"><span data-stu-id="ae786-149">Trivia, tokens, and nodes are composed hierarchically to form a tree that completely represents everything in a fragment of Visual Basic or C# code.</span></span> <span data-ttu-id="ae786-150">你可以使用语法可视化工具窗口  查看此结构。</span><span class="sxs-lookup"><span data-stu-id="ae786-150">You can see this structure using the **Syntax Visualizer** window.</span></span> <span data-ttu-id="ae786-151">在 Visual Studio 中，选择“视图”   > “其他窗口”   > “语法可视化工具”  。</span><span class="sxs-lookup"><span data-stu-id="ae786-151">In Visual Studio, choose **View** > **Other Windows** > **Syntax Visualizer**.</span></span> <span data-ttu-id="ae786-152">例如使用语法可视化工具检查上述 C# 源文件  ，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="ae786-152">For example, the preceding C# source file examined using the **Syntax Visualizer** looks like the following figure:</span></span>

<span data-ttu-id="ae786-153">SyntaxNode：蓝色 | SyntaxToken：绿色 | SyntaxTrivia：红色    ![C# 代码文件](media/walkthrough-csharp-syntax-figure1.png)</span><span class="sxs-lookup"><span data-stu-id="ae786-153">**SyntaxNode**: Blue | **SyntaxToken**: Green | **SyntaxTrivia**: Red ![C# Code File](media/walkthrough-csharp-syntax-figure1.png)</span></span>

<span data-ttu-id="ae786-154">通过在此树结构中导航，可以查找代码文件中的所有语句、表达式、标记或空格位。</span><span class="sxs-lookup"><span data-stu-id="ae786-154">By navigating this tree structure, you can find any statement, expression, token, or bit of white space in a code file.</span></span>

<span data-ttu-id="ae786-155">在使用语法 API 查找代码文件中的内容时，大多数方案都涉及检查代码的小片段或者搜索特定语句或片段。</span><span class="sxs-lookup"><span data-stu-id="ae786-155">While you can find anything in a code file using the Syntax APIs, most scenarios involve examining small snippets of code, or searching for particular statements or fragments.</span></span> <span data-ttu-id="ae786-156">接下来的两个示例展示浏览代码结构或搜索单个语句的典型使用形式。</span><span class="sxs-lookup"><span data-stu-id="ae786-156">The two examples that follow show typical uses to browse the structure of code, or search for single statements.</span></span>

## <a name="traversing-trees"></a><span data-ttu-id="ae786-157">遍历树</span><span class="sxs-lookup"><span data-stu-id="ae786-157">Traversing trees</span></span>

<span data-ttu-id="ae786-158">你可通过两种方式检查语法树中的节点。</span><span class="sxs-lookup"><span data-stu-id="ae786-158">You can examine the nodes in a syntax tree in two ways.</span></span> <span data-ttu-id="ae786-159">可以遍历该树以检查每个节点，也可以查询特定元素或节点。</span><span class="sxs-lookup"><span data-stu-id="ae786-159">You can traverse the tree to examine each node, or you can query for specific elements or nodes.</span></span>

### <a name="manual-traversal"></a><span data-ttu-id="ae786-160">手动遍历</span><span class="sxs-lookup"><span data-stu-id="ae786-160">Manual traversal</span></span>

<span data-ttu-id="ae786-161">可以在[我们的 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SyntaxQuickStart)中看到此示例的已完成代码。</span><span class="sxs-lookup"><span data-stu-id="ae786-161">You can see the finished code for this sample in [our GitHub repository](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SyntaxQuickStart).</span></span>

> [!NOTE]
> <span data-ttu-id="ae786-162">语法树类型使用继承描述不同的语法元素，这些语法元素在程序中的不同位置生效。</span><span class="sxs-lookup"><span data-stu-id="ae786-162">The Syntax Tree types use inheritance to describe the different syntax elements that are valid at different locations in the program.</span></span> <span data-ttu-id="ae786-163">使用这些 API 通常意味着将属性或集合成员强制转换为特定的派生类型。</span><span class="sxs-lookup"><span data-stu-id="ae786-163">Using these APIs often means casting properties or collection members to specific derived types.</span></span> <span data-ttu-id="ae786-164">在以下示例中，作业和强制转换分别是独立的语句，采用显式类型化变量。</span><span class="sxs-lookup"><span data-stu-id="ae786-164">In the following examples, the assignment and the casts are separate statements, using explicitly typed variables.</span></span> <span data-ttu-id="ae786-165">你可以读取代码以查看 API 的返回类型以及所返回对象的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="ae786-165">You can read the code to see the return types of the API and the runtime type of the objects returned.</span></span> <span data-ttu-id="ae786-166">在实践中，更常见的是使用隐式类型化变量并靠 API 名称来描述要检查的对象的类型。</span><span class="sxs-lookup"><span data-stu-id="ae786-166">In practice, it's more common to use implicitly typed variables and rely on API names to describe the type of objects being examined.</span></span>

<span data-ttu-id="ae786-167">新建 C#“独立代码分析工具”项目  ：</span><span class="sxs-lookup"><span data-stu-id="ae786-167">Create a new C# **Stand-Alone Code Analysis Tool** project:</span></span>

* <span data-ttu-id="ae786-168">在 Visual Studio 中，选择“文件”   > “新建”   > “项目”  ，显示新建项目对话框。</span><span class="sxs-lookup"><span data-stu-id="ae786-168">In Visual Studio, choose **File** > **New** > **Project** to display the New Project dialog.</span></span>
* <span data-ttu-id="ae786-169">在“Visual C#”   > “扩展性”  下，选择“独立代码分析工具”  。</span><span class="sxs-lookup"><span data-stu-id="ae786-169">Under **Visual C#** > **Extensibility**, choose **Stand-Alone Code Analysis Tool**.</span></span>
* <span data-ttu-id="ae786-170">将项目命名为 SyntaxTreeManualTraversal  ，然后单击“确定”。</span><span class="sxs-lookup"><span data-stu-id="ae786-170">Name your project "**SyntaxTreeManualTraversal**" and click OK.</span></span>

<span data-ttu-id="ae786-171">你将分析上面展示过的基本“Hello World!”</span><span class="sxs-lookup"><span data-stu-id="ae786-171">You're going to analyze the basic "Hello World!"</span></span> <span data-ttu-id="ae786-172">程序。</span><span class="sxs-lookup"><span data-stu-id="ae786-172">program shown earlier.</span></span>
<span data-ttu-id="ae786-173">为 Hello World 程序添加文本，作为 `Program` 类中的常量：</span><span class="sxs-lookup"><span data-stu-id="ae786-173">Add the text for the Hello World program as a constant in your `Program` class:</span></span>

[!code-csharp[Declare the program text](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#1 "Declare a constant string for the program text to analyze")]

<span data-ttu-id="ae786-174">下一步，添加下列代码以生成 `programText` 常量中的代码文本的语法树。</span><span class="sxs-lookup"><span data-stu-id="ae786-174">Next, add the following code to build the **syntax tree** for the code text in the `programText` constant.</span></span>  <span data-ttu-id="ae786-175">将下面这行代码添加到 `Main` 方法中：</span><span class="sxs-lookup"><span data-stu-id="ae786-175">Add the following line to your `Main` method:</span></span>

[!code-csharp[Create the tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#2 "Create the syntax tree")]

<span data-ttu-id="ae786-176">这两行代码创建树并检索树的根节点。</span><span class="sxs-lookup"><span data-stu-id="ae786-176">Those two lines create the tree and retrieve the root node of that tree.</span></span> <span data-ttu-id="ae786-177">现在，可以检查树的节点。</span><span class="sxs-lookup"><span data-stu-id="ae786-177">You can now examine the nodes in the tree.</span></span> <span data-ttu-id="ae786-178">将这几行代码添加到 `Main` 方法以显示树中根节点的部分属性：</span><span class="sxs-lookup"><span data-stu-id="ae786-178">Add these lines to your `Main` method to display some of the properties of the root node in the tree:</span></span>

[!code-csharp[Examine the root node](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#3 "Examine the root node")]

<span data-ttu-id="ae786-179">运行应用程序，查看代码获取到的关于树中根节点的信息。</span><span class="sxs-lookup"><span data-stu-id="ae786-179">Run the application to see what your code has discovered about the root node in this tree.</span></span>

<span data-ttu-id="ae786-180">通常要遍历树以了解代码。</span><span class="sxs-lookup"><span data-stu-id="ae786-180">Typically, you'd traverse the tree to learn about the code.</span></span> <span data-ttu-id="ae786-181">在此示例中，你通过分析已知代码探索 API。</span><span class="sxs-lookup"><span data-stu-id="ae786-181">In this example, you're analyzing code you know to explore the APIs.</span></span> <span data-ttu-id="ae786-182">添加下列代码检查 `root` 节点的第一个成员：</span><span class="sxs-lookup"><span data-stu-id="ae786-182">Add the following code to examine the first member of the `root` node:</span></span>

[!code-csharp[Find the first member](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#4 "Find the first member")]

<span data-ttu-id="ae786-183">该成员为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NamespaceDeclarationSyntax?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="ae786-183">That member is a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NamespaceDeclarationSyntax?displayProperty=nameWithType>.</span></span> <span data-ttu-id="ae786-184">它代表 `namespace HelloWorld` 声明范围内的所有内容。</span><span class="sxs-lookup"><span data-stu-id="ae786-184">It represents everything in the scope of the `namespace HelloWorld` declaration.</span></span> <span data-ttu-id="ae786-185">添加下列代码检查 `HelloWorld` 命名空间内声明了哪些节点：</span><span class="sxs-lookup"><span data-stu-id="ae786-185">Add the following code to examine what nodes are declared inside the `HelloWorld` namespace:</span></span>

[!code-csharp[Find the class declaration](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#5 "Find the class declaration")]

<span data-ttu-id="ae786-186">运行程序查看你了解到的内容。</span><span class="sxs-lookup"><span data-stu-id="ae786-186">Run the program to see what you've learned.</span></span>

<span data-ttu-id="ae786-187">现在你了解声明为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ClassDeclarationSyntax?displayProperty=nameWithType>，声明一个该类型的新变量以检查类声明。</span><span class="sxs-lookup"><span data-stu-id="ae786-187">Now that you know the declaration is a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ClassDeclarationSyntax?displayProperty=nameWithType>, declare a new variable of that type to examine the class declaration.</span></span> <span data-ttu-id="ae786-188">此类只包含一个成员：`Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-188">This class only contains one member: the `Main` method.</span></span> <span data-ttu-id="ae786-189">添加以下代码找到 `Main` 方法，并将其强制转换为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.MethodDeclarationSyntax?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="ae786-189">Add the following code to find the `Main` method, and cast it to a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.MethodDeclarationSyntax?displayProperty=nameWithType>.</span></span>

[!code-csharp[Find the main declaration](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#6 "Find the main declaration")]

<span data-ttu-id="ae786-190">方法声明节点包含关于该方法的所有语法信息。</span><span class="sxs-lookup"><span data-stu-id="ae786-190">The method declaration node contains all the syntactic information about the method.</span></span> <span data-ttu-id="ae786-191">让我们显示 `Main` 方法的返回类型、参数的数量和类型以及该方法的正文。</span><span class="sxs-lookup"><span data-stu-id="ae786-191">Let's display the return type of the `Main` method, the number and types of the arguments, and the body text of the method.</span></span> <span data-ttu-id="ae786-192">添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="ae786-192">Add the following code:</span></span>

[!code-csharp[Examine the syntax of the main method](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#7 "Display information about the main method")]

<span data-ttu-id="ae786-193">运行程序，查看已获取的关于此程序的所有信息：</span><span class="sxs-lookup"><span data-stu-id="ae786-193">Run the program to see all the information you've discovered about this program:</span></span>

```text
The tree is a CompilationUnit node.
The tree has 1 elements in it.
The tree has 4 using statements. They are:
        System
        System.Collections
        System.Linq
        System.Text
The first member is a NamespaceDeclaration.
There are 1 members declared in this namespace.
The first member is a ClassDeclaration.
There are 1 members declared in the Program class.
The first member is a MethodDeclaration.
The return type of the Main method is void.
The method has 1 parameters.
The type of the args parameter is string[].
The body text of the Main method follows:
        {
            Console.WriteLine("Hello, World!");
        }
```

### <a name="query-methods"></a><span data-ttu-id="ae786-194">查询方法</span><span class="sxs-lookup"><span data-stu-id="ae786-194">Query methods</span></span>

<span data-ttu-id="ae786-195">除了遍历树，还可以使用 <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> 上定义的查询方法来探索语法树。</span><span class="sxs-lookup"><span data-stu-id="ae786-195">In addition to traversing trees, you can also explore the syntax tree using the query methods defined on <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType>.</span></span> <span data-ttu-id="ae786-196">任何一个熟悉 XPath 的人都可以立刻掌握这些方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-196">These methods should be immediately familiar to anyone familiar with XPath.</span></span> <span data-ttu-id="ae786-197">可以结合 LINQ 使用这些方法快速查找树中的内容。</span><span class="sxs-lookup"><span data-stu-id="ae786-197">You can use these methods with LINQ to quickly find things in a tree.</span></span> <span data-ttu-id="ae786-198"><xref:Microsoft.CodeAnalysis.SyntaxNode> 具备类似 <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A>、<xref:Microsoft.CodeAnalysis.SyntaxNode.AncestorsAndSelf%2A> 和 <xref:Microsoft.CodeAnalysis.SyntaxNode.ChildNodes%2A> 的查询方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-198">The <xref:Microsoft.CodeAnalysis.SyntaxNode> has query methods such as <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A>, <xref:Microsoft.CodeAnalysis.SyntaxNode.AncestorsAndSelf%2A> and <xref:Microsoft.CodeAnalysis.SyntaxNode.ChildNodes%2A>.</span></span>

<span data-ttu-id="ae786-199">你可以使用这些查询方法对 `Main` 方法查找参数，而不是在树中进行导航。</span><span class="sxs-lookup"><span data-stu-id="ae786-199">You can use these query methods to find the argument to the `Main` method as an alternative to navigating the tree.</span></span> <span data-ttu-id="ae786-200">将以下代码添加到 `Main` 方法末尾：</span><span class="sxs-lookup"><span data-stu-id="ae786-200">Add the following code to the bottom of your `Main` method:</span></span>

[!code-csharp[Query the tree for the arguments to Main](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#8 "Query the tree for the arguments to Main")]

<span data-ttu-id="ae786-201">第一个语句使用 LINQ 表达式和 <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A> 方法查找与前面的示例相同的参数。</span><span class="sxs-lookup"><span data-stu-id="ae786-201">The first statement uses a LINQ expression and the <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A> method to locate the same parameter as in the previous example.</span></span>

<span data-ttu-id="ae786-202">运行程序后能看到 LINQ 表达式已找到的参数，结果与树的手动导航一样。</span><span class="sxs-lookup"><span data-stu-id="ae786-202">Run the program, and you can see that the LINQ expression found the same parameter as manually navigating the tree.</span></span>

<span data-ttu-id="ae786-203">此示例使用 `WriteLine` 语句，在遍历至语法树的相关信息时显示这些信息。</span><span class="sxs-lookup"><span data-stu-id="ae786-203">The sample uses `WriteLine` statements to display information about the syntax trees as they are traversed.</span></span> <span data-ttu-id="ae786-204">你还可以通过在调试程序下运行完成的程序了解更多内容。</span><span class="sxs-lookup"><span data-stu-id="ae786-204">You can also learn much more by running the finished program under the debugger.</span></span> <span data-ttu-id="ae786-205">你可以检查更多属性和方法，它们是为 Hello World 程序创建的语法树的一部分。</span><span class="sxs-lookup"><span data-stu-id="ae786-205">You can examine more of the properties and methods that are part of the syntax tree created for the hello world program.</span></span>

## <a name="syntax-walkers"></a><span data-ttu-id="ae786-206">语法查看器</span><span class="sxs-lookup"><span data-stu-id="ae786-206">Syntax walkers</span></span>

<span data-ttu-id="ae786-207">你经常需要查找语法树中特定类型的所有节点，例如某个文件中的每个属性声明。</span><span class="sxs-lookup"><span data-stu-id="ae786-207">Often you want to find all nodes of a specific type in a syntax tree, for example, every property declaration in a file.</span></span> <span data-ttu-id="ae786-208">通过扩展 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker?displayProperty=nameWithType> 类并重写 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitPropertyDeclaration(Microsoft.CodeAnalysis.CSharp.Syntax.PropertyDeclarationSyntax)> 方法，处理语法树中的每个属性声明，且事先无需了解它的结构。</span><span class="sxs-lookup"><span data-stu-id="ae786-208">By extending the <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker?displayProperty=nameWithType> class and overriding the <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitPropertyDeclaration(Microsoft.CodeAnalysis.CSharp.Syntax.PropertyDeclarationSyntax)> method, you process every property declaration in a syntax tree without knowing its structure beforehand.</span></span> <span data-ttu-id="ae786-209"><xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 是 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor> 中的一个特定类型，它以递归方式访问节点以及节点的每个子级。</span><span class="sxs-lookup"><span data-stu-id="ae786-209"><xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> is a specific kind of <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor> that recursively visits a node and each of its children.</span></span>

<span data-ttu-id="ae786-210">此示例实现了检查语法树的 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker>。</span><span class="sxs-lookup"><span data-stu-id="ae786-210">This example implements a <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> that examines a syntax tree.</span></span> <span data-ttu-id="ae786-211">它收集所找到的不导入 `System` 命名空间的 `using` 指令。</span><span class="sxs-lookup"><span data-stu-id="ae786-211">It collects `using` directives it finds that aren't importing a `System` namespace.</span></span>

<span data-ttu-id="ae786-212">新建 C#“独立代码分析工具”项目  ，将其命名为 SyntaxWalker  。</span><span class="sxs-lookup"><span data-stu-id="ae786-212">Create a new C# **Stand-Alone Code Analysis Tool** project; name it "**SyntaxWalker**."</span></span>

<span data-ttu-id="ae786-213">可以在[我们的 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SyntaxQuickStart)中看到此示例的已完成代码。</span><span class="sxs-lookup"><span data-stu-id="ae786-213">You can see the finished code for this sample in [our GitHub repository](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SyntaxQuickStart).</span></span> <span data-ttu-id="ae786-214">GitHub 上的示例包含本教程介绍的两个项目。</span><span class="sxs-lookup"><span data-stu-id="ae786-214">The sample on GitHub contains both projects described in this tutorial.</span></span>

<span data-ttu-id="ae786-215">如前面的示例所示，你可以定义字符串常量来保存将要分析的程序的文本：</span><span class="sxs-lookup"><span data-stu-id="ae786-215">As in the previous sample, you can define a string constant to hold the text of the program you're going to analyze:</span></span>

[!code-csharp[Define the code text to analyzer](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#1 "Define the program text to analyze")]

<span data-ttu-id="ae786-216">此源文本包含的 `using` 指令分散在四个不同的位置：文件级、顶级命名空间以及两个嵌套命名空间。</span><span class="sxs-lookup"><span data-stu-id="ae786-216">This source text contains `using` directives scattered across four different locations: the file-level, in the top-level namespace, and in the two nested namespaces.</span></span> <span data-ttu-id="ae786-217">此示例重点介绍使用 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 类以查询代码的核心方案。</span><span class="sxs-lookup"><span data-stu-id="ae786-217">This example highlights a core scenario for using the <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> class to query code.</span></span> <span data-ttu-id="ae786-218">通过访问根语法树的每个节点来查找 using 声明会很麻烦。</span><span class="sxs-lookup"><span data-stu-id="ae786-218">It would be cumbersome to visit every node in the root syntax tree to find using declarations.</span></span> <span data-ttu-id="ae786-219">替代方法是创建派生类，并改用只在树中的当前节点为 using 指令时才会调用的方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-219">Instead, you create a derived class and override the method that gets called only when the current node in the tree is a using directive.</span></span> <span data-ttu-id="ae786-220">访问器不会在任何其他节点类型上做任何工作。</span><span class="sxs-lookup"><span data-stu-id="ae786-220">Your visitor does not do any work on any other node types.</span></span> <span data-ttu-id="ae786-221">这一方法检查每个 `using` 语句并生成命名空间的集合，其中包含的命名空间都不在 `System` 命名空间中。</span><span class="sxs-lookup"><span data-stu-id="ae786-221">This single method examines each of the `using` statements and builds a collection of the namespaces that aren't in the `System` namespace.</span></span> <span data-ttu-id="ae786-222">生成一个检查所有 `using` 语句（但仅检查 `using` 语句）的 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker>。</span><span class="sxs-lookup"><span data-stu-id="ae786-222">You build a <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> that examines all the `using` statements, but only the `using` statements.</span></span>

<span data-ttu-id="ae786-223">现在，你已定义程序文本，需要创建 `SyntaxTree` 并获取该树的根：</span><span class="sxs-lookup"><span data-stu-id="ae786-223">Now that you've defined the program text, you need to create a `SyntaxTree` and get the root of that tree:</span></span>

[!code-csharp[Create the Syntax tree and access the root](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#2 "Create the Syntax tree and access the root node.")]

<span data-ttu-id="ae786-224">接下来，创建一个新类。</span><span class="sxs-lookup"><span data-stu-id="ae786-224">Next, create a new class.</span></span> <span data-ttu-id="ae786-225">在 Visual Studio 中，依次选择“项目”   > “添加新项”  。</span><span class="sxs-lookup"><span data-stu-id="ae786-225">In Visual Studio, choose **Project** > **Add New Item**.</span></span> <span data-ttu-id="ae786-226">在“添加新项”对话框中键入 UsingCollector.cs 作为文件名   。</span><span class="sxs-lookup"><span data-stu-id="ae786-226">In the **Add New Item** dialog type *UsingCollector.cs* as the filename.</span></span>

<span data-ttu-id="ae786-227">在 `UsingCollector` 类中实现 `using` 访问器功能。</span><span class="sxs-lookup"><span data-stu-id="ae786-227">You implement the `using` visitor functionality in the `UsingCollector` class.</span></span> <span data-ttu-id="ae786-228">首先，从 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 派生 `UsingCollector` 类。</span><span class="sxs-lookup"><span data-stu-id="ae786-228">Start by making the `UsingCollector` class derive from <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker>.</span></span>

[!code-csharp[Declare the base class for the using collector](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#3 "Declare the base class for the UsingCollector")]

<span data-ttu-id="ae786-229">需要存储空间来保存收集的命名空间节点。</span><span class="sxs-lookup"><span data-stu-id="ae786-229">You need storage to hold the namespace nodes that you're collecting.</span></span>  <span data-ttu-id="ae786-230">在 `UsingCollector` 类中声明公共只读属性；使用此变量来存储你找到的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> 节点：</span><span class="sxs-lookup"><span data-stu-id="ae786-230">Declare a public read-only property in the `UsingCollector` class; you use this variable to store the <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> nodes you find:</span></span>

[!code-csharp[Declare storage for the using syntax nodes](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#4 "Declare storage for the using syntax nodes")]

<span data-ttu-id="ae786-231">基类，<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 实现访问语法树中每个节点的逻辑。</span><span class="sxs-lookup"><span data-stu-id="ae786-231">The base class, <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> implements the logic to visit each node in the syntax tree.</span></span> <span data-ttu-id="ae786-232">派生类重写你感兴趣的特定节点所调用的方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-232">The derived class overrides the methods called for the specific nodes you're interested in.</span></span> <span data-ttu-id="ae786-233">在这种情况下，你对任何 `using` 指令都感兴趣。</span><span class="sxs-lookup"><span data-stu-id="ae786-233">In this case, you're interested in any `using` directive.</span></span> <span data-ttu-id="ae786-234">也就是说必须重写 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> 方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-234">That means you must override the <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> method.</span></span> <span data-ttu-id="ae786-235">此方法的一个参数是 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> 对象。</span><span class="sxs-lookup"><span data-stu-id="ae786-235">The one argument to this method is a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> object.</span></span> <span data-ttu-id="ae786-236">这是使用访问器的一项重要优势：它们调用已重写的方法，这些方法所包含的参数已经强制转换为特定节点类型。</span><span class="sxs-lookup"><span data-stu-id="ae786-236">That's an important advantage to using the visitors: they call the overridden methods with arguments already cast to the specific node type.</span></span> <span data-ttu-id="ae786-237"><xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> 类有 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.Name> 属性，该属性存储要导入的命名空间的名称。</span><span class="sxs-lookup"><span data-stu-id="ae786-237">The <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> class has a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.Name> property that stores the name of the namespace being imported.</span></span> <span data-ttu-id="ae786-238">它是一个 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="ae786-238">It is a <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType>.</span></span> <span data-ttu-id="ae786-239">在 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> 重写中添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="ae786-239">Add the following code in the <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> override:</span></span>

[!code-csharp[Examine using nodes for the System namespace](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#5 "Examine all using nodes for the System namespace.")]

<span data-ttu-id="ae786-240">如前面的示例所示，你已添加各种 `WriteLine` 语句来协助理解此方法。</span><span class="sxs-lookup"><span data-stu-id="ae786-240">As with the earlier example, you've added a variety of `WriteLine` statements to aid in understanding of this method.</span></span> <span data-ttu-id="ae786-241">你可以查看此方法的调用时间以及每次调用时向它传递的参数。</span><span class="sxs-lookup"><span data-stu-id="ae786-241">You can see when it's called, and what arguments are passed to it each time.</span></span>

<span data-ttu-id="ae786-242">最后，需要添加两行代码以创建 `UsingCollector` 并让其访问根节点，收集所有 `using` 语句。</span><span class="sxs-lookup"><span data-stu-id="ae786-242">Finally, you need to add two lines of code to create the `UsingCollector` and have it visit the root node, collecting all the `using` statements.</span></span> <span data-ttu-id="ae786-243">然后，添加 `foreach` 循环以显示收集器找到的所有 `using` 语句：</span><span class="sxs-lookup"><span data-stu-id="ae786-243">Then, add a `foreach` loop to display all the `using` statements your collector found:</span></span>

[!code-csharp[Create the UsingCollector and visit the root node.](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#6 "Create the UsingCollector and visit the root node.")]

<span data-ttu-id="ae786-244">编译并运行该程序。</span><span class="sxs-lookup"><span data-stu-id="ae786-244">Compile and run the program.</span></span> <span data-ttu-id="ae786-245">您应看到以下输出：</span><span class="sxs-lookup"><span data-stu-id="ae786-245">You should see the following output:</span></span>

```console
        VisitUsingDirective called with System.
        VisitUsingDirective called with System.Collections.Generic.
        VisitUsingDirective called with System.Linq.
        VisitUsingDirective called with System.Text.
        VisitUsingDirective called with Microsoft.CodeAnalysis.
                Success. Adding Microsoft.CodeAnalysis.
        VisitUsingDirective called with Microsoft.CodeAnalysis.CSharp.
                Success. Adding Microsoft.CodeAnalysis.CSharp.
        VisitUsingDirective called with Microsoft.
                Success. Adding Microsoft.
        VisitUsingDirective called with System.ComponentModel.
        VisitUsingDirective called with Microsoft.Win32.
                Success. Adding Microsoft.Win32.
        VisitUsingDirective called with System.Runtime.InteropServices.
        VisitUsingDirective called with System.CodeDom.
        VisitUsingDirective called with Microsoft.CSharp.
                Success. Adding Microsoft.CSharp.
Microsoft.CodeAnalysis
Microsoft.CodeAnalysis.CSharp
Microsoft
Microsoft.Win32
Microsoft.CSharp
Press any key to continue . . .
```

<span data-ttu-id="ae786-246">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="ae786-246">Congratulations!</span></span> <span data-ttu-id="ae786-247">你已使用语法 API 查找特定类型的 C# 语句和 C# 源代码中的声明  。</span><span class="sxs-lookup"><span data-stu-id="ae786-247">You've used the **Syntax API** to locate specific kinds of C# statements and declarations in C# source code.</span></span>
