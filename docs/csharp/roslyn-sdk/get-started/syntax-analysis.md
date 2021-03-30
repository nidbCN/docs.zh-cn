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
# <a name="get-started-with-syntax-analysis"></a>语法分析入门

在本教程中，你将了解“语法 API”  。 语法 API 提供对描述 C# 或 Visual Basic 程序的数据结构的访问。 这些数据结构具备足够的详细信息，它们可以充分表示任何规模的任何程序。 这些结构可以描述准确编译并运行的完整程序。 当你在编辑器中编写程序时，它们还可以描述这些尚不完整的程序。

要启用此丰富的表达式，组成语法 API 的数据结构和 API 必然是复杂的。 让我们看看数据结构是什么样的，从典型的“Hello World”程序的数据结构开始：

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

查看以前程序的文本。 识别熟悉的元素。 整个文本代表一个源文件，或者一个“编译单元”  。 源文件的前三行是 using 指令  。 剩余源包含在命名空间声明中  。 命名空间声明包含一个子类声明  。 类声明包含一个方法声明  。

语法 API 使用代表编译单元的根创建树结构。 树中的节点代表 using 指令、命名空间声明和程序中的所有其他元素。 树结构一直到最低级别：字符串“Hello World!” 是“字符串文本标记”  ，即一种参数的子代  。 语法 API 提供对程序结构的访问。 你可以查询特定代码实践、浏览整个树以理解代码并通过修改现有树来新建树。

该简介概述了使用语法 API 可以访问的信息类型。 语法 API 仅仅是描述你从 C# 中熟悉的代码结构的正式 API。 完整的功能包括关于设置代码格式（包括换行符、空格和缩进）的信息。 在人工程序员或编译者编写和读取代码时，你可以使用此信息完整表示代码。 使用此结构可以在深有意义的级别上与源代码进行交互。 它不再是文本字符串，而是代表 C# 程序结构的数据。

若要开始，需要安装 .NET 编译器平台 SDK  ：

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="understanding-syntax-trees"></a>了解语法树

将语法 API 用于 C# 代码结构的所有分析。 语法 API 公开分析程序、语法树和用于分析并构造语法树的实用程序  。 这是搜索特定语法元素的代码或读取程序代码的方式。

语法树是 C# 和 Visual Basic 编译器用于理解 C# 和 Visual Basic 程序的数据结构。 语法树由生成项目时或开发人员按 F5 时所运行的分析程序生成。 语法树对语言完全保真；代码文件中的每一位信息都在树中。 将语法树写入文本会再现已分析的完全原始文本。 语法树也是不可变的  ；一旦创建语法树，就不能再更改。 树的使用者可以在多个线程上对树进行分析，不需要锁或其他并发度量，很清楚数据是不会更改的。 可使用 API 新建树，新建的树就是对现有树进行修改后的成果。

语法树的四个主要构建基块为：

* <xref:Microsoft.CodeAnalysis.SyntaxTree?displayProperty=nameWithType> 类，它的实例代表整个分析树。 <xref:Microsoft.CodeAnalysis.SyntaxTree> 是一种带有语言特定派生类的抽象类。 使用 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxTree?displayProperty=nameWithType>（或 <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicSyntaxTree?displayProperty=nameWithType>）类的分析方法对 C#（或 Visual Basic）中的文本进行分析。
* <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> 类，它的实例表示声明、语句、子句和表达式等语法构造。
* <xref:Microsoft.CodeAnalysis.SyntaxToken?displayProperty=nameWithType> 结构，它代表独立的关键词、标识符、运算符或标点。
* 最后是 <xref:Microsoft.CodeAnalysis.SyntaxTrivia?displayProperty=nameWithType> 结构，它代表语法上不重要的信息，例如标记、预处理指令和注释之间的空格。

琐事、标记和节点分层次地构成了树，树完全代表了 Visual Basic 或 C# 代码片段中的所有内容。 你可以使用语法可视化工具窗口  查看此结构。 在 Visual Studio 中，选择“视图”   > “其他窗口”   > “语法可视化工具”  。 例如使用语法可视化工具检查上述 C# 源文件  ，如下图所示：

SyntaxNode：蓝色 | SyntaxToken：绿色 | SyntaxTrivia：红色    ![C# 代码文件](media/walkthrough-csharp-syntax-figure1.png)

通过在此树结构中导航，可以查找代码文件中的所有语句、表达式、标记或空格位。

在使用语法 API 查找代码文件中的内容时，大多数方案都涉及检查代码的小片段或者搜索特定语句或片段。 接下来的两个示例展示浏览代码结构或搜索单个语句的典型使用形式。

## <a name="traversing-trees"></a>遍历树

你可通过两种方式检查语法树中的节点。 可以遍历该树以检查每个节点，也可以查询特定元素或节点。

### <a name="manual-traversal"></a>手动遍历

可以在[我们的 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SyntaxQuickStart)中看到此示例的已完成代码。

> [!NOTE]
> 语法树类型使用继承描述不同的语法元素，这些语法元素在程序中的不同位置生效。 使用这些 API 通常意味着将属性或集合成员强制转换为特定的派生类型。 在以下示例中，作业和强制转换分别是独立的语句，采用显式类型化变量。 你可以读取代码以查看 API 的返回类型以及所返回对象的运行时类型。 在实践中，更常见的是使用隐式类型化变量并靠 API 名称来描述要检查的对象的类型。

新建 C#“独立代码分析工具”项目  ：

* 在 Visual Studio 中，选择“文件”   > “新建”   > “项目”  ，显示新建项目对话框。
* 在“Visual C#”   > “扩展性”  下，选择“独立代码分析工具”  。
* 将项目命名为 SyntaxTreeManualTraversal  ，然后单击“确定”。

你将分析上面展示过的基本“Hello World!” 程序。
为 Hello World 程序添加文本，作为 `Program` 类中的常量：

[!code-csharp[Declare the program text](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#1 "Declare a constant string for the program text to analyze")]

下一步，添加下列代码以生成 `programText` 常量中的代码文本的语法树。  将下面这行代码添加到 `Main` 方法中：

[!code-csharp[Create the tree](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#2 "Create the syntax tree")]

这两行代码创建树并检索树的根节点。 现在，可以检查树的节点。 将这几行代码添加到 `Main` 方法以显示树中根节点的部分属性：

[!code-csharp[Examine the root node](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#3 "Examine the root node")]

运行应用程序，查看代码获取到的关于树中根节点的信息。

通常要遍历树以了解代码。 在此示例中，你通过分析已知代码探索 API。 添加下列代码检查 `root` 节点的第一个成员：

[!code-csharp[Find the first member](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#4 "Find the first member")]

该成员为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NamespaceDeclarationSyntax?displayProperty=nameWithType>。 它代表 `namespace HelloWorld` 声明范围内的所有内容。 添加下列代码检查 `HelloWorld` 命名空间内声明了哪些节点：

[!code-csharp[Find the class declaration](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#5 "Find the class declaration")]

运行程序查看你了解到的内容。

现在你了解声明为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.ClassDeclarationSyntax?displayProperty=nameWithType>，声明一个该类型的新变量以检查类声明。 此类只包含一个成员：`Main` 方法。 添加以下代码找到 `Main` 方法，并将其强制转换为 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.MethodDeclarationSyntax?displayProperty=nameWithType>。

[!code-csharp[Find the main declaration](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#6 "Find the main declaration")]

方法声明节点包含关于该方法的所有语法信息。 让我们显示 `Main` 方法的返回类型、参数的数量和类型以及该方法的正文。 添加以下代码：

[!code-csharp[Examine the syntax of the main method](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#7 "Display information about the main method")]

运行程序，查看已获取的关于此程序的所有信息：

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

### <a name="query-methods"></a>查询方法

除了遍历树，还可以使用 <xref:Microsoft.CodeAnalysis.SyntaxNode?displayProperty=nameWithType> 上定义的查询方法来探索语法树。 任何一个熟悉 XPath 的人都可以立刻掌握这些方法。 可以结合 LINQ 使用这些方法快速查找树中的内容。 <xref:Microsoft.CodeAnalysis.SyntaxNode> 具备类似 <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A>、<xref:Microsoft.CodeAnalysis.SyntaxNode.AncestorsAndSelf%2A> 和 <xref:Microsoft.CodeAnalysis.SyntaxNode.ChildNodes%2A> 的查询方法。

你可以使用这些查询方法对 `Main` 方法查找参数，而不是在树中进行导航。 将以下代码添加到 `Main` 方法末尾：

[!code-csharp[Query the tree for the arguments to Main](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/HelloSyntaxTree/Program.cs#8 "Query the tree for the arguments to Main")]

第一个语句使用 LINQ 表达式和 <xref:Microsoft.CodeAnalysis.SyntaxNode.DescendantNodes%2A> 方法查找与前面的示例相同的参数。

运行程序后能看到 LINQ 表达式已找到的参数，结果与树的手动导航一样。

此示例使用 `WriteLine` 语句，在遍历至语法树的相关信息时显示这些信息。 你还可以通过在调试程序下运行完成的程序了解更多内容。 你可以检查更多属性和方法，它们是为 Hello World 程序创建的语法树的一部分。

## <a name="syntax-walkers"></a>语法查看器

你经常需要查找语法树中特定类型的所有节点，例如某个文件中的每个属性声明。 通过扩展 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker?displayProperty=nameWithType> 类并重写 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitPropertyDeclaration(Microsoft.CodeAnalysis.CSharp.Syntax.PropertyDeclarationSyntax)> 方法，处理语法树中的每个属性声明，且事先无需了解它的结构。 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 是 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor> 中的一个特定类型，它以递归方式访问节点以及节点的每个子级。

此示例实现了检查语法树的 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker>。 它收集所找到的不导入 `System` 命名空间的 `using` 指令。

新建 C#“独立代码分析工具”项目  ，将其命名为 SyntaxWalker  。

可以在[我们的 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SyntaxQuickStart)中看到此示例的已完成代码。 GitHub 上的示例包含本教程介绍的两个项目。

如前面的示例所示，你可以定义字符串常量来保存将要分析的程序的文本：

[!code-csharp[Define the code text to analyzer](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#1 "Define the program text to analyze")]

此源文本包含的 `using` 指令分散在四个不同的位置：文件级、顶级命名空间以及两个嵌套命名空间。 此示例重点介绍使用 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 类以查询代码的核心方案。 通过访问根语法树的每个节点来查找 using 声明会很麻烦。 替代方法是创建派生类，并改用只在树中的当前节点为 using 指令时才会调用的方法。 访问器不会在任何其他节点类型上做任何工作。 这一方法检查每个 `using` 语句并生成命名空间的集合，其中包含的命名空间都不在 `System` 命名空间中。 生成一个检查所有 `using` 语句（但仅检查 `using` 语句）的 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker>。

现在，你已定义程序文本，需要创建 `SyntaxTree` 并获取该树的根：

[!code-csharp[Create the Syntax tree and access the root](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#2 "Create the Syntax tree and access the root node.")]

接下来，创建一个新类。 在 Visual Studio 中，依次选择“项目”   > “添加新项”  。 在“添加新项”对话框中键入 UsingCollector.cs 作为文件名   。

在 `UsingCollector` 类中实现 `using` 访问器功能。 首先，从 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 派生 `UsingCollector` 类。

[!code-csharp[Declare the base class for the using collector](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#3 "Declare the base class for the UsingCollector")]

需要存储空间来保存收集的命名空间节点。  在 `UsingCollector` 类中声明公共只读属性；使用此变量来存储你找到的 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax> 节点：

[!code-csharp[Declare storage for the using syntax nodes](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#4 "Declare storage for the using syntax nodes")]

基类，<xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxWalker> 实现访问语法树中每个节点的逻辑。 派生类重写你感兴趣的特定节点所调用的方法。 在这种情况下，你对任何 `using` 指令都感兴趣。 也就是说必须重写 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> 方法。 此方法的一个参数是 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> 对象。 这是使用访问器的一项重要优势：它们调用已重写的方法，这些方法所包含的参数已经强制转换为特定节点类型。 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax?displayProperty=nameWithType> 类有 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax.Name> 属性，该属性存储要导入的命名空间的名称。 它是一个 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.NameSyntax?displayProperty=nameWithType>。 在 <xref:Microsoft.CodeAnalysis.CSharp.CSharpSyntaxVisitor.VisitUsingDirective(Microsoft.CodeAnalysis.CSharp.Syntax.UsingDirectiveSyntax)> 重写中添加以下代码：

[!code-csharp[Examine using nodes for the System namespace](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/UsingCollector.cs#5 "Examine all using nodes for the System namespace.")]

如前面的示例所示，你已添加各种 `WriteLine` 语句来协助理解此方法。 你可以查看此方法的调用时间以及每次调用时向它传递的参数。

最后，需要添加两行代码以创建 `UsingCollector` 并让其访问根节点，收集所有 `using` 语句。 然后，添加 `foreach` 循环以显示收集器找到的所有 `using` 语句：

[!code-csharp[Create the UsingCollector and visit the root node.](../../../../samples/snippets/csharp/roslyn-sdk/SyntaxQuickStart/SyntaxWalker/Program.cs#6 "Create the UsingCollector and visit the root node.")]

编译并运行该程序。 您应看到以下输出：

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

祝贺你！ 你已使用语法 API 查找特定类型的 C# 语句和 C# 源代码中的声明  。
