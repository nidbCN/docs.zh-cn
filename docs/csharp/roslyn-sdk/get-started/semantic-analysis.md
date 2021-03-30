---
title: 语义分析入门
description: 本教程概述如何使用.NET 编译器 SDK 进行语义分析。
ms.date: 02/06/2018
ms.custom: mvc
ms.openlocfilehash: 3119363822328c0e5fc67c2a2a4a917a7d37cfd2
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872556"
---
# <a name="get-started-with-semantic-analysis"></a>语义分析入门

本教程假定你熟悉语法 API。 [语法分析入门](syntax-analysis.md)一文提供了详细介绍。

在本教程中，你将了解“符号”和“绑定 API”。 这些 API 提供关于程序语义含义的信息。 它们帮助你就程序中的符号所代表的类型进行问答。

需要安装 .NET Compiler Platform SDK：

[!INCLUDE[interactive-note](~/includes/roslyn-installation.md)]

## <a name="understanding-compilations-and-symbols"></a>了解编译和符号

随着 .NET 编译器 SDK 的使用越来越多，你会越来越熟悉语法 API 和语义 API 之间的区别。 “语法 API”使你可以看到程序的结构。 但是经常会需要更多关于程序的语义或含义的信息。 虽然可以独立地对松散的代码文件或 Visual Basic 或 C# 代码的代码片段进行语法上的分析，但是凭空提出类似“这是什么类型的变量？”这样的问题毫无意义。 类型名称的含义可能取决于程序集引用、命名空间导入或其他代码文件。 使用 **语义 API** 回答这些问题，特别是 <xref:Microsoft.CodeAnalysis.Compilation?displayProperty=nameWithType> 类。

<xref:Microsoft.CodeAnalysis.Compilation> 实例类似于编译器所看见的单个项目，且代表编译 Visual Basic 或 C# 程序所需的一切。 编译包括一组要编译的源文件、程序集引用和编译器选项。 可以使用此上下文中所有的其他信息来推断代码的含义。 <xref:Microsoft.CodeAnalysis.Compilation> 允许你查找“符号” - 类似名称和其他表达式引用的类型、命名空间、成员和变量的实体。 将名称和表达式与“符号”进行关联的过程被称为“绑定”。

与 <xref:Microsoft.CodeAnalysis.SyntaxTree?displayProperty=nameWithType> 类似，<xref:Microsoft.CodeAnalysis.Compilation> 是一个带有语言特定派生类的抽象类。 在创建编译实例时，必须在 <xref:Microsoft.CodeAnalysis.CSharp.CSharpCompilation?displayProperty=nameWithType>（或 <xref:Microsoft.CodeAnalysis.VisualBasic.VisualBasicCompilation?displayProperty=nameWithType>）类上调用工厂方法。

## <a name="querying-symbols"></a>查询符号

在本教程中，你会再次看到“Hello World”程序。 这次你将在该程序中查询符号，以理解这些符号所代表的类型。 在命名空间中查询类型，并学习如何查找类型上可用的方法。

可以在[我们的 GitHub 存储库](https://github.com/dotnet/samples/tree/main/csharp/roslyn-sdk/SemanticQuickStart)中看到此示例的已完成代码。

> [!NOTE]
> 语法树类型使用继承描述不同的语法元素，这些语法元素在程序中的不同位置生效。 使用这些 API 通常意味着将属性或集合成员强制转换为特定的派生类型。 在以下示例中，作业和强制转换分别是独立的语句，采用显式类型化变量。 你可以读取代码以查看 API 的返回类型以及所返回对象的运行时类型。 在实践中，更常见的是使用隐式类型化变量并靠 API 名称来描述要检查的对象的类型。

新建 C#“独立代码分析工具”项目  ：

* 在 Visual Studio 中，选择“文件”   > “新建”   > “项目”  ，显示新建项目对话框。
* 在“Visual C#”   > “扩展性”  下，选择“独立代码分析工具”  。
* 将项目命名为“SemanticQuickStart”并单击“确定”。

你将分析上面展示过的基本“Hello World!” 程序。
为 Hello World 程序添加文本，作为 `Program` 类中的常量：

[!code-csharp[Declare the program test](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#1 "Declare a constant string for the program text to analyze")]

下一步，添加以下代码，为 `programText` 常量中的代码文本生成语法树。  将下面这行代码添加到 `Main` 方法中：

[!code-csharp[Create the tree](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#2 "Create the syntax tree")]

接下来，从已创建的树生成 <xref:Microsoft.CodeAnalysis.CSharp.CSharpCompilation>。 “Hello World”示例依赖于 <xref:System.String> 和 <xref:System.Console> 类型。 需要引用在编译中声明这两种类型的程序集。 将下面这行代码添加到 `Main` 方法以创建语法树的编译，包括对相应程序集的引用：

[!code-csharp[Create the compilation](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#3 "Create the compilation for the semantic model")]

<xref:Microsoft.CodeAnalysis.CSharp.CSharpCompilation.AddReferences%2A?displayProperty=nameWithType> 方法将引用添加到编译。 <xref:Microsoft.CodeAnalysis.MetadataReference.CreateFromFile%2A?displayProperty=nameWithType> 方法加载程序集作为引用。

## <a name="querying-the-semantic-model"></a>查询语义模型

如果有 <xref:Microsoft.CodeAnalysis.Compilation>，你可以向它查询 <xref:Microsoft.CodeAnalysis.Compilation> 所包含的任何 <xref:Microsoft.CodeAnalysis.SyntaxTree> 的 <xref:Microsoft.CodeAnalysis.SemanticModel>。 你可将语义模型看作通常从智能感知中获取的所有信息的来源。 <xref:Microsoft.CodeAnalysis.SemanticModel> 可回答“此位置中范围内的名称是什么？”、“可通过此方法访问哪些成员？”、“此文本块中使用了什么变量？”和“此名称/表达式指的是什么？”等问题。 添加此声明以创建语义模型：

[!code-csharp[Create the semantic model](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#4 "Create the semantic model")]

## <a name="binding-a-name"></a>绑定名称

<xref:Microsoft.CodeAnalysis.Compilation> 从 <xref:Microsoft.CodeAnalysis.SyntaxTree> 创建 <xref:Microsoft.CodeAnalysis.SemanticModel> 创建模型后，你可以查询以找到第一个 `using` 指令，并检索 `System` 命名空间的符号信息。 将这两行代码添加到 `Main` 方法以创建语义模型，并检索第一个 using 语句的符号：

[!code-csharp[Find the namespace symbol for the first using](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#5 "Find the namespace symbol for the first using")]

上述代码示例演示如何绑定第一个 `using` 指令中的名称以检索 `System` 命名空间的 <xref:Microsoft.CodeAnalysis.SymbolInfo?displayProperty=nameWithType>。 上述代码还说明，使用语法模型查找代码的结构；使用语义模型理解它的含义。 语法模型在 using 语句中找到字符串 `System`。 语义模型具有关于 `System` 命名空间中所定义类型的全部信息。

可以从 <xref:Microsoft.CodeAnalysis.SymbolInfo> 对象获取使用 <xref:Microsoft.CodeAnalysis.SymbolInfo.Symbol?displayProperty=nameWithType> 属性的 <xref:Microsoft.CodeAnalysis.ISymbol?displayProperty=nameWithType>。 此属性返回此表达式所引用的符号。 对于不引用任何内容的表达式（例如数字参数），此属性为 `null`。 若 <xref:Microsoft.CodeAnalysis.SymbolInfo.Symbol?displayProperty=nameWithType> 不为 null，<xref:Microsoft.CodeAnalysis.ISymbol.Kind?displayProperty=nameWithType> 表示符号的类型。 在此示例中，<xref:Microsoft.CodeAnalysis.ISymbol.Kind?displayProperty=nameWithType> 属性是 <xref:Microsoft.CodeAnalysis.SymbolKind.Namespace?displayProperty=nameWithType>。 将以下代码添加到 `Main` 方法。 它检索 `System` 命名空间的符号，然后将 `System` 命名空间中声明的所有子命名空间显示出来：

[!code-csharp[Display all the child namespaces](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#6 "Display all the child namespaces from this compilation")]

运行该程序，然后应看到以下输出：

```output
System.Collections
System.Configuration
System.Deployment
System.Diagnostics
System.Globalization
System.IO
System.Numerics
System.Reflection
System.Resources
System.Runtime
System.Security
System.StubHelpers
System.Text
System.Threading
Press any key to continue . . .
```

> [!NOTE]
> 该输出不包含 `System` 命名空间的每个子命名空间。 它显示此编译中存在的每个命名空间，只引用声明了 `System.String` 的程序集。 此编译不了解其他程序集中所声明的任何命名空间

### <a name="binding-an-expression"></a>绑定表达式

上述代码显示如何通过绑定到名称来查找符号。 在可以绑定的 C# 程序中还有其他不是名称的表达式。 为演示此功能，我们绑定到简单的字符串文本。

“Hello World”程序包含一个 <xref:Microsoft.CodeAnalysis.CSharp.Syntax.LiteralExpressionSyntax?displayProperty=nameWithType>，即“Hello, World!” 向控制台显示的字符串。

通过找到程序中的单个字符串文本 查找到“Hello World!”字符串。 找到语法节点后，从语义模型中获取该节点的类型信息。 将以下代码添加到 `Main` 方法：

[!code-csharp[Find the namespace symbol for the only using](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#7 "Find the namespace symbol for the only using")]

<xref:Microsoft.CodeAnalysis.TypeInfo?displayProperty=nameWithType> 结构包括 <xref:Microsoft.CodeAnalysis.TypeInfo.Type?displayProperty=nameWithType> 属性，此属性可启用对关于文本类型的语义信息的访问。 在此例中为 `string` 类型。 添加将此属性分配至本地变量的声明：

[!code-csharp[Find the semantic information about the string type](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#8 "Use the string literal to access the semantic information in the string type.")]

要完成本教程，让我们生成 LINQ 查询，该查询会创建一个在 `string` 类型上声明并返回 `string` 的所有公共方法的序列。 此查询比较复杂，我们将逐行生成，然后将其重新构造为单个查询。 此查询的源是 `string` 类型上所声明全部成员的序列：

[!code-csharp[Access the sequence of members on the string type](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#9 "Access the sequence of members on the string type.")]

该源序列包含所有成员（包括属性和字段），使用 <xref:System.Collections.Immutable.ImmutableArray%601.OfType%2A?displayProperty=nameWithType> 方法对其进行筛选以查找属于 <xref:Microsoft.CodeAnalysis.IMethodSymbol?displayProperty=nameWithType> 对象的元素：

[!code-csharp[Filter the sequence to only methods](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#10 "Find the subset of the collection that is the methods.")]

接下来，添加另一个筛选器，只返回这些属于公共方法并返回 `string` 的方法：

[!code-csharp[Filter on return type and accessibility](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#11 "Find only the public methods that return a string.")]

只选择名称属性，且只通过删除任何重载来区分名称：

[!code-csharp[find the distinct names.](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#12 "Use the string literal to access the semantic information in the string type.")]

还可以使用 LINQ 查询语法生成完整查询，然后在控制台中显示所有方法名称：

[!code-csharp[build and display the results of this query.](../../../../samples/snippets/csharp/roslyn-sdk/SemanticQuickStart/Program.cs#13 "Build and display the results of the query.")]

生成并运行程序。 应会看到以下输出：

```output
Join
Substring
Trim
TrimStart
TrimEnd
Normalize
PadLeft
PadRight
ToLower
ToLowerInvariant
ToUpper
ToUpperInvariant
ToString
Insert
Replace
Remove
Format
Copy
Concat
Intern
IsInterned
Press any key to continue . . .
```

你已使用语义 API 来查找并显示关于属于此程序的符号的信息。
