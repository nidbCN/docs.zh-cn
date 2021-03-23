---
title: XML 文档注释 - C# 编程指南
description: 了解 XML 文档注释。 可以通过在特殊注释字段中包含 XML 元素来为代码创建文档。
ms.date: 07/20/2015
f1_keywords:
- cs.xml
helpviewer_keywords:
- XML [C#], code comments
- comments [C#], XML
- documentation comments [C#]
- C# source code files
- C# language, XML code comments
- XML documentation comments [C#]
ms.assetid: 803b7f7b-7428-4725-b5db-9a6cff273199
ms.openlocfilehash: d784ec58096e44cf010edd279f682555df58a8ef
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478396"
---
# <a name="xml-documentation-comments-c-programming-guide"></a><span data-ttu-id="e0957-104">XML 文档注释（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="e0957-104">XML documentation comments (C# programming guide)</span></span>

<span data-ttu-id="e0957-105">使用 C#，可以通过以下方式为代码创建文档：将特殊注释字段中的 XML 元素包含在源代码中注释引用的代码块的前面，例如：</span><span class="sxs-lookup"><span data-stu-id="e0957-105">In C#, you can create documentation for your code by including XML elements in special comment fields (indicated by triple slashes) in the source code directly before the code block to which the comments refer, for example.</span></span>

```csharp
/// <summary>
///  This class performs an important function.
/// </summary>
public class MyClass {}
```

<span data-ttu-id="e0957-106">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 选项进行编译时，编译器会在源代码中搜索所有 XML 标记，并创建一个 XML 文档文件。</span><span class="sxs-lookup"><span data-stu-id="e0957-106">When you compile with the [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) option, the compiler will search for all XML tags in the source code and create an XML documentation file.</span></span> <span data-ttu-id="e0957-107">若要基于编译器生成的文件创建最终文档，可以创建一个自定义工具，也可以使用 [DocFX](https://dotnet.github.io/docfx/) 或 [Sandcastle](https://github.com/EWSoftware/SHFB) 等工具。</span><span class="sxs-lookup"><span data-stu-id="e0957-107">To create the final documentation based on the compiler-generated file, you can create a custom tool or use a tool such as [DocFX](https://dotnet.github.io/docfx/) or [Sandcastle](https://github.com/EWSoftware/SHFB).</span></span>

<span data-ttu-id="e0957-108">若要引用 XML 元素（例如，你的函数将处理你要在 XML 文档注释中描述的特定 XML 元素），你可使用标准引用机制（`<` 和 `>`）。</span><span class="sxs-lookup"><span data-stu-id="e0957-108">To refer to XML elements (for example, your function processes specific XML elements that you want to describe in an XML documentation comment), you can use the standard quoting mechanism (`<` and `>`).</span></span>  <span data-ttu-id="e0957-109">若要引用代码引用 (`cref`) 元素中的通用标识符，可使用转义字符（例如，`cref="List&lt;T&gt;"`）或大括号 (`cref="List{T}"`)。</span><span class="sxs-lookup"><span data-stu-id="e0957-109">To refer to generic identifiers in code reference (`cref`) elements, you can use either the escape characters (for example, `cref="List&lt;T&gt;"`) or braces (`cref="List{T}"`).</span></span>  <span data-ttu-id="e0957-110">作为特例，编译器会将大括号解析为尖括号以在引用通用标识符时使作者能够更轻松地进行文档注释。</span><span class="sxs-lookup"><span data-stu-id="e0957-110">As a special case, the compiler parses the braces as angle brackets to make the documentation comment less cumbersome to author when referring to generic identifiers.</span></span>

> [!NOTE]
> <span data-ttu-id="e0957-111">XML 文档注释不是元数据；它们不包括在编译的程序集中，因此无法通过反射对其进行访问。</span><span class="sxs-lookup"><span data-stu-id="e0957-111">The XML documentation comments are not metadata; they are not included in the compiled assembly and therefore they are not accessible through reflection.</span></span>

## <a name="in-this-section"></a><span data-ttu-id="e0957-112">本节内容</span><span class="sxs-lookup"><span data-stu-id="e0957-112">In this section</span></span>

- [<span data-ttu-id="e0957-113">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="e0957-113">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)

- [<span data-ttu-id="e0957-114">处理 XML 文件</span><span class="sxs-lookup"><span data-stu-id="e0957-114">Processing the XML file</span></span>](./processing-the-xml-file.md)

- [<span data-ttu-id="e0957-115">文档标记分隔符</span><span class="sxs-lookup"><span data-stu-id="e0957-115">Delimiters for documentation tags</span></span>](./delimiters-for-documentation-tags.md)

- [<span data-ttu-id="e0957-116">如何使用 XML 文档功能</span><span class="sxs-lookup"><span data-stu-id="e0957-116">How to use the XML documentation features</span></span>](./how-to-use-the-xml-documentation-features.md)

## <a name="related-sections"></a><span data-ttu-id="e0957-117">相关章节</span><span class="sxs-lookup"><span data-stu-id="e0957-117">Related sections</span></span>

<span data-ttu-id="e0957-118">有关详细信息，请参见:</span><span class="sxs-lookup"><span data-stu-id="e0957-118">For more information, see:</span></span>

- [<span data-ttu-id="e0957-119">DocumentationFile（处理文档注释）</span><span class="sxs-lookup"><span data-stu-id="e0957-119">**DocumentationFile** (Process Documentation Comments)</span></span>](../../language-reference/compiler-options/output.md#documentationfile)

## <a name="c-language-specification"></a><span data-ttu-id="e0957-120">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="e0957-120">C# language specification</span></span>

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a><span data-ttu-id="e0957-121">请参阅</span><span class="sxs-lookup"><span data-stu-id="e0957-121">See also</span></span>

- [<span data-ttu-id="e0957-122">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="e0957-122">C# Programming Guide</span></span>](../index.md)
