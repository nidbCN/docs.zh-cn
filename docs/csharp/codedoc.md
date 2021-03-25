---
title: 使用 XML 注释记录 C# 代码
description: 了解如何使用 XML 文档注释来记录代码和在编译时生成 XML 文档文件。
ms.date: 01/21/2020
ms.technology: csharp-fundamentals
ms.assetid: 8e75e317-4a55-45f2-a866-e76124171838
ms.openlocfilehash: cdc3937ba3b641b90aed85a604ca05195ea34fe7
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478483"
---
# <a name="document-your-c-code-with-xml-comments"></a>使用 XML 注释记录 C# 代码

XML 文档注释是一种特殊注释，添加在任何用户定义的类型或成员的定义上方。
其特殊之处在于其可由编译器处理，由此在编译时生成 XML 文档文件。
编译器生成的 XML 文件可以与 .NET 程序集一起分发，以便 Visual Studio 和其他 IDE 可使用 IntelliSense 显示关于类型或成员的快速信息。 此外，可以通过 [DocFX](https://dotnet.github.io/docfx/) 和 [Sandcastle](https://github.com/EWSoftware/SHFB) 等工具运行 XML 文件，由此生成 API 引用网站。

编译器会忽略 XML 文档注释，与对待其他注释一样。

可通过执行下列操作之一在编译时生成 XML 文件：

- 如果要使用 .NET Core 从命令行开发应用程序，可以将 `GenerateDocumentationFile` 元素添加到 .csproj 项目文件的 `<PropertyGroup>` 部分。 此外，也可以直接使用 [`DocumentationFile` 元素](/visualstudio/msbuild/common-msbuild-project-properties)指定文档文件的路径。 下面的示例使用与程序集相同的根文件夹名在项目目录中生成 XML 文件：

   ```xml
   <GenerateDocumentationFile>true</GenerateDocumentationFile>
   ```

   此表达式等效于以下表达式：

   ```xml
   <DocumentationFile>bin\$(Configuration)\$(TargetFramework)\$(AssemblyName).xml</DocumentationFile>
   ```

- 如果使用 Visual Studio 开发应用程序，右键单击项目并选择“属性”  。 在属性对话框中，选择“生成”  选项卡，然后选中“XML 文档文件”  。 还可以更改编译器写入文件的位置。

- 如果是从命令行编译 .NET 应用程序，编译时请添加 [DocumentationFile 编译器选项](language-reference/compiler-options/output.md#documentationfile)。  

XML 文档注释使用三个正斜杠 (`///`) 和 XML 格式的注释正文。 例如：

[!code-csharp[XML Documentation Comment](../../samples/snippets/csharp/concepts/codedoc/xml-comment.cs)]

## <a name="walkthrough"></a>演练

现在来演练针对一个十分基本的数学库进行文档编制，以使新手开发人员能够容易地理解/参与，以及使第三方开发人员能够轻松使用。

下面是简单数学库的代码：

[!code-csharp[Sample Library](../../samples/snippets/csharp/concepts/codedoc/sample-library.cs)]

示例库支持 `int` 和 `double` 数据类型的四种主要算术运算（`add`、`subtract`、`multiply` 和 `divide`）。

现在，希望能够从代码中为使用库但无源代码访问权限的第三方开发人员创建 API 引用文档。
如前面所述，XML 文档标记可用于实现此目的。 现在介绍 C# 编译器支持的标准 XML 标记。

## \<summary>

`<summary>` 标记可添加关于类型或成员的信息摘要。
这里通过将其添加到 `Math` 类定义和第一个 `Add` 方法来演示其用法。 可随意将其应用于代码的其余部分。

[!code-csharp[Summary Tag](~/samples/snippets/csharp/concepts/codedoc/summary-tag.cs)]

`<summary>` 标记很重要，建议包含，因为其内容是 IntelliSense 或 API 参考文档中的类型或成员信息的主要来源。

## \<remarks>

`<remarks>`标记可补充关于 `<summary>` 标记提供的类型或成员的信息。 在此示例中，只需将其添加到类。

[!code-csharp[Remarks Tag](~/samples/snippets/csharp/concepts/codedoc/remarks-tag.cs)]

## \<returns>

`<returns>` 标记描述方法声明的返回值。
与以前一样，下面的示例展示第一个 `Add` 方法上的 `<returns>` 标记。 可以对其他方法执行相同操作。

[!code-csharp[Returns Tag](~/samples/snippets/csharp/concepts/codedoc/returns-tag.cs)]

## \<value>

`<value>` 标记类似于 `<returns>` 标记，只不过前者用于属性。
假设 `Math` 库有一个名为 `PI` 的静态属性，下面是此标记的用法：

[!code-csharp[Value Tag](~/samples/snippets/csharp/concepts/codedoc/value-tag.cs)]

## \<example>

使用 `<example>` 标记可在 XML 文档中包含一个示例。
此操作包括使用子 `<code>` 标记。

[!code-csharp[Example Tag](~/samples/snippets/csharp/concepts/codedoc/example-tag.cs)]

`code` 标记保留较长示例的换行符和缩进。

## \<para>

`<para>` 标记可用于设置其父标记内的内容的格式。 `<para>` 通常在标记内使用，如 `<remarks>` 或 `<returns>`，作用是将文本分成段落。
可以为类定义设置 `<remarks>` 标记的内容的格式。

[!code-csharp[Para Tag](~/samples/snippets/csharp/concepts/codedoc/para-tag.cs)]

## \<c>

还是关于格式设置，使用 `<c>` 标记可将部分文本标记为代码。
与 `<code>` 标记类似，但是内联的。 想要显示快速代码示例并将其作为标记内容一部分时，这很有帮助。
现在来更新 `Math` 类的文档。

[!code-csharp[C Tag](~/samples/snippets/csharp/concepts/codedoc/c-tag.cs)]

## \<exception>

通过使用 `<exception>` 标记，可以让开发人员了解到，方法有可能引发特定异常。
查看 `Math` 库，可以看到，如果满足特定条件，两个 `Add` 方法都会引发异常。 如果 `b` 参数为零，则 `Divide` 方法也会引发异常，尽管不是很常见。 现在将异常文档添加到此方法。

[!code-csharp[Exception Tag](~/samples/snippets/csharp/concepts/codedoc/exception-tag.cs)]

`cref` 属性表示可从当前编译环境中实现的异常引用。
其类型可为项目中或引用的程序集中定义的任何类型。 如果无法解析编译器的值，则该编译器将发出一条警告。

## \<see>

使用 `<see>` 标记，可以创建可单击的链接，指向另一个代码元素的文档页面。 在下一个示例中，将创建两个 `Add` 方法之间的可单击的链接。

[!code-csharp[See Tag](~/samples/snippets/csharp/concepts/codedoc/see-tag.cs)]

`cref` 是表示可从当前编译环境引用的类型或其成员的 **必需** 属性。
其类型可为项目中或引用的程序集中定义的任何类型。

## \<seealso>

可以通过使用 `<see>` 标记的方式使用 `<seealso>`标记。 唯一的区别是其内容通常位于“另请参见”部分。 以下将在整数 `Add` 方法上添加 `seealso` 标记，从而在接受整数参数的类中引用其他方法：

[!code-csharp[Seealso Tag](~/samples/snippets/csharp/concepts/codedoc/seealso-tag.cs)]

`cref` 属性表示可从当前编译环境进行的对类型或其成员的引用。
其类型可为项目中或引用的程序集中定义的任何类型。

## \<param>

使用 `<param>` 标记来描述方法的参数。 下面是关于双 `Add` 方法的示例：标记所描述的参数在 **必需的** `name` 属性中指定。

[!code-csharp[Param Tag](~/samples/snippets/csharp/concepts/codedoc/param-tag.cs)]

## \<typeparam>

`<typeparam>` 标记的用法与 `<param>` 标记一样，但前者由泛型类型或方法声明用来描述泛型参数。
将快速泛型方法添加到 `Math` 类，以检查某个数量是否大于另一个数量。

[!code-csharp[Typeparam Tag](~/samples/snippets/csharp/concepts/codedoc/typeparam-tag.cs)]

## \<paramref>

有时可能正在通过一个 `<summary>` 标记描述一个方法的作用，并且想要引用一个参数。 这时 `<paramref>` 标记就很适合用来实现这一目的。 现在来更新双基 `Add` 方法的摘要。 与 `<param>` 标记一样，参数名称在必需的 `name` 属性中指定。

[!code-csharp[Paramref Tag](~/samples/snippets/csharp/concepts/codedoc/paramref-tag.cs)]

## \<typeparamref>

`<typeparamref>` 标记的用法与 `<paramref>` 标记一样，但前者由泛型类型或方法声明用来描述泛型参数。
可以使用之前创建的那个泛型方法。

[!code-csharp[Typeparamref Tag](~/samples/snippets/csharp/concepts/codedoc/typeparamref-tag.cs)]

## \<list>

使用 `<list>` 标记可将文档信息格式化为有序列表、无序列表或表格。 制作 `Math` 库支持的所有数学操作的无序列表。

[!code-csharp[List Tag](~/samples/snippets/csharp/concepts/codedoc/list-tag.cs)]

可以通过将 `type` 属性分别改为 `number` 或 `table` 来制作有序列表或表格。

## \<inheritdoc>

可以使用 `<inheritdoc>` 标记继承基类、接口和类似方法中的 XML 注释。 这样不必复制和粘贴重复的 XML 注释，并自动保持 XML 注释同步。

[!code-csharp-interactive[InheritDoc Tag](~/samples/snippets/csharp/concepts/codedoc/inheritdoc-tag.cs)]

### <a name="put-it-all-together"></a>将其放在一起

如果已按照本教程的操作方法将标记应用于代码中的所需位置，则代码现应如下所示：

[!code-csharp[Tagged Library](~/samples/snippets/csharp/concepts/codedoc/tagged-library.cs)]

可从代码中生成包括可单击的交叉引用的详细文档网站。 但将面临另一问题：代码变得难以阅读。
需要筛查的信息浩如烟海，这对任何要参与此代码编写的开发人员都是噩梦。
幸好有一个 XML 标记，可帮助解决这个问题：

## \<include>

使用 `<include>` 标记，可以引用单独的 XML 文件中的注释（该文件描述源代码中的类型和成员），而不是将文档注释直接放入源代码文件中。

现在，要将所有 XML 标记移到名为 `docs.xml` 的单独的 XML 文件中。 可随时重新命名该文件。

[!code-xml[Sample XML](~/samples/snippets/csharp/concepts/codedoc/include.xml)]

在上面的 XML 中，每个成员的文档注释将直接显示在按其作用命名的标记中。 可选择自己的策略。
现在一个单独的文件中已具有 XML 注释，接下来来看看如何通过使用 `<include>` 标记使代码更易于阅读：

[!code-csharp[Include Tag](~/samples/snippets/csharp/concepts/codedoc/include-tag.cs)]

现在好了，代码又变得可读了，并且未丢失任何文档信息。

`file` 属性表示包含文档的 XML 文件的名称。

`path` 属性表示一个 [XPath](../standard/data/xml/xpath-queries-and-namespaces.md) 查询，该查询的对象为指定的 `file` 中的 `tag name`。

`name` 属性表示位于注释前的标记中的名称说明符。

可用于替换 `name` 的 `id` 属性表示位于注释前的标记的 ID。

### <a name="user-defined-tags"></a>用户定义的标记

上述所有标记均表示由 C# 编译器识别的标记。 但用户可随意定义自己的标记。
Sandcastle 等工具支持其他标记，例如 [\<event>](https://ewsoftware.github.io/XMLCommentsGuide/html/81bf7ad3-45dc-452f-90d5-87ce2494a182.htm) 和 [\<note>](https://ewsoftware.github.io/XMLCommentsGuide/html/4302a60f-e4f4-4b8d-a451-5f453c4ebd46.htm)，甚至支持[编制命名空间文档](https://ewsoftware.github.io/XMLCommentsGuide/html/BD91FAD4-188D-4697-A654-7C07FD47EF31.htm)。
自定义或内部文档生成工具也可与标准标记配合使用，并支持 HTML 到 PDF 等多种输出格式。

## <a name="recommendations"></a>建议

出于多种原因，建议编制代码文档。 接下来将介绍一些最佳做法、常规使用方案，以及在 C# 代码中使用 XML 文档标记时的需知内容。

- 为保持一致性，应编制所有公共可见类型及其成员的文档。 如果必须这么做，请完整全面地完成这一操作。
- 还可使用 XML 注释编制私有成员的文档。 但这会公开库的内部（很可能是机密的）运作情况。
- 但至少类型及其成员应具有 `<summary>` 标记，因为其内容是 IntelliSense 所需要的。
- 应使用句号结尾的完整句子编写文档文本。
- 完全支持部分类，并且该类型的文档信息将串联为单个条目。
- 编译器验证 `<exception>`、`<include>`、`<param>`、`<see>`、`<seealso>` 和 `<typeparam>` 标记的语法。
- 编译器验证某些参数，这些参数包含文件路径和对代码其余部分的引用。

## <a name="see-also"></a>另请参阅

- [XML 文档注释（C# 编程指南）](programming-guide/xmldoc/index.md)
- [建议的文档注释标记（C# 编程指南）](programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)
