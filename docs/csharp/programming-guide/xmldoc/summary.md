---
title: <summary> - C# 编程指南
description: 了解 <summary> 用于描述类型或类型成员的 XML 标记。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
f1_keywords:
- <summary>
- summary
helpviewer_keywords:
- <summary> C# XML tag
- summary C# XML tag
ms.assetid: b4c43d92-2067-4eac-a59a-d32f5248c08b
ms.openlocfilehash: e20970971636f13357c165f3065050fcf5914ada
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477712"
---
# <a name="summary-c-programming-guide"></a><span data-ttu-id="12dda-105">\<summary>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="12dda-105">\<summary> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="12dda-106">语法</span><span class="sxs-lookup"><span data-stu-id="12dda-106">Syntax</span></span>

```xml
<summary>description</summary>
```

## <a name="parameters"></a><span data-ttu-id="12dda-107">参数</span><span class="sxs-lookup"><span data-stu-id="12dda-107">Parameters</span></span>

- `description`

  <span data-ttu-id="12dda-108">对象的摘要。</span><span class="sxs-lookup"><span data-stu-id="12dda-108">A summary of the object.</span></span>

## <a name="remarks"></a><span data-ttu-id="12dda-109">备注</span><span class="sxs-lookup"><span data-stu-id="12dda-109">Remarks</span></span>

<span data-ttu-id="12dda-110">`<summary>` 标记应当用于描述类型或类型成员。</span><span class="sxs-lookup"><span data-stu-id="12dda-110">The `<summary>` tag should be used to describe a type or a type member.</span></span> <span data-ttu-id="12dda-111">使用 [\<remarks>](./remarks.md) 可针对某个类型说明添加补充信息。</span><span class="sxs-lookup"><span data-stu-id="12dda-111">Use [\<remarks>](./remarks.md) to add supplemental information to a type description.</span></span> <span data-ttu-id="12dda-112">使用 [cref 属性](./cref-attribute.md)可启用文档工具（如 [DocFX](https://dotnet.github.io/docfx/) 和 [Sandcastle](https://github.com/EWSoftware/SHFB)）来创建指向代码元素的文档页的内部超链接。</span><span class="sxs-lookup"><span data-stu-id="12dda-112">Use the [cref Attribute](./cref-attribute.md) to enable documentation tools such as [DocFX](https://dotnet.github.io/docfx/) and [Sandcastle](https://github.com/EWSoftware/SHFB) to create internal hyperlinks to documentation pages for code elements.</span></span>

<span data-ttu-id="12dda-113">`<summary>` 标记的文本是唯一有关 IntelliSense 中的类型的信息源，它也显示在对象浏览器窗口中。</span><span class="sxs-lookup"><span data-stu-id="12dda-113">The text for the `<summary>` tag is the only source of information about the type in IntelliSense, and is also displayed in the Object Browser Window.</span></span>

<span data-ttu-id="12dda-114">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="12dda-114">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span> <span data-ttu-id="12dda-115">若要基于编译器生成的文件创建最终文档，可以创建一个自定义工具，也可以使用 [DocFX](https://dotnet.github.io/docfx/) 或 [Sandcastle](https://github.com/EWSoftware/SHFB) 等工具。</span><span class="sxs-lookup"><span data-stu-id="12dda-115">To create the final documentation based on the compiler-generated file, you can create a custom tool, or use a tool such as [DocFX](https://dotnet.github.io/docfx/) or [Sandcastle](https://github.com/EWSoftware/SHFB).</span></span>

## <a name="example"></a><span data-ttu-id="12dda-116">示例</span><span class="sxs-lookup"><span data-stu-id="12dda-116">Example</span></span>

[!code-csharp[csProgGuideDocComments#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#12)]

<span data-ttu-id="12dda-117">前面的示例生成下面的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="12dda-117">The previous example produces the following XML file.</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>YourNamespace</name>
    </assembly>
    <members>
        <member name="T:TestClass">
            text for class TestClass
        </member>
        <member name="M:TestClass.DoWork(System.Int32)">
            <summary>DoWork is a method in the TestClass class.
            <para>Here's how you could make a second paragraph in a description. <see cref="M:System.Console.WriteLine(System.String)"/> for information about output statements.</para>
            </summary>
            <seealso cref="M:TestClass.Main"/>
        </member>
        <member name="M:TestClass.Main">
            text for Main
        </member>
    </members>
</doc>
```

## <a name="cref-example"></a><span data-ttu-id="12dda-118">cref 示例</span><span class="sxs-lookup"><span data-stu-id="12dda-118">cref example</span></span>

<span data-ttu-id="12dda-119">下面的示例演示如何对泛型类型进行 `cref` 引用。</span><span class="sxs-lookup"><span data-stu-id="12dda-119">The following example shows how to make a `cref` reference to a generic type.</span></span>

[!code-csharp[csProgGuideDocComments#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#11)]

<span data-ttu-id="12dda-120">前面的示例生成下面的 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="12dda-120">The previous example produces the following XML file.</span></span>

```xml
<?xml version="1.0"?>
<doc>
    <assembly>
        <name>CRefTest</name>
    </assembly>
    <members>
        <member name="T:A">
            <summary cref="T:C`1">
            </summary>
        </member>
        <member name="T:B">
            <summary cref="T:C`1">
            </summary>
        </member>
        <member name="T:C`1">
            <summary cref="T:A">
            </summary>
            <typeparam name="T"></typeparam>
        </member>
    </members>
</doc>
```

## <a name="see-also"></a><span data-ttu-id="12dda-121">请参阅</span><span class="sxs-lookup"><span data-stu-id="12dda-121">See also</span></span>

- [<span data-ttu-id="12dda-122">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="12dda-122">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="12dda-123">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="12dda-123">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)
