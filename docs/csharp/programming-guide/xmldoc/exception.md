---
title: <exception> - C# 编程指南
description: 了解 XML <exception> 标记。 使用此标记，可以指定可引发的异常，以及可应用于方法、属性、事件和索引器的异常。
ms.date: 07/20/2015
f1_keywords:
- exception
- <exception>
helpviewer_keywords:
- <exception> C# XML tag
- exception C# XML tag
ms.assetid: dd73aac5-3c74-4fcf-9498-f11bff3a2f3c
ms.openlocfilehash: 37d119e3cde115104d149d8f35b8937efddd70d4
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103479112"
---
# <a name="exception-c-programming-guide"></a><span data-ttu-id="5d641-104">\<exception>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="5d641-104">\<exception> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="5d641-105">语法</span><span class="sxs-lookup"><span data-stu-id="5d641-105">Syntax</span></span>

```xml
<exception cref="member">description</exception>
```

## <a name="parameters"></a><span data-ttu-id="5d641-106">参数</span><span class="sxs-lookup"><span data-stu-id="5d641-106">Parameters</span></span>

- <span data-ttu-id="5d641-107">cref = " `member`"</span><span class="sxs-lookup"><span data-stu-id="5d641-107">cref = " `member`"</span></span>

  <span data-ttu-id="5d641-108">对当前编译环境中出现的一个异常的引用。</span><span class="sxs-lookup"><span data-stu-id="5d641-108">A reference to an exception that is available from the current compilation environment.</span></span> <span data-ttu-id="5d641-109">编译器检查是否存在给定的异常，并将 `member` 转换为输出 XML 中的规范的元素名称。</span><span class="sxs-lookup"><span data-stu-id="5d641-109">The compiler checks that the given exception exists and translates `member` to the canonical element name in the output XML.</span></span> <span data-ttu-id="5d641-110">`member` 必须出现在双引号 (" ") 内。</span><span class="sxs-lookup"><span data-stu-id="5d641-110">`member` must appear within double quotation marks (" ").</span></span>

  <span data-ttu-id="5d641-111">有关如何设置 `member` 格式以引用泛型类型的详细信息，请参阅[处理 XML 文件](processing-the-xml-file.md)。</span><span class="sxs-lookup"><span data-stu-id="5d641-111">For more information on how to format `member` to reference a generic type, see [Processing the XML File](processing-the-xml-file.md).</span></span>

- `description`

  <span data-ttu-id="5d641-112">异常的说明。</span><span class="sxs-lookup"><span data-stu-id="5d641-112">A description of the exception.</span></span>

## <a name="remarks"></a><span data-ttu-id="5d641-113">备注</span><span class="sxs-lookup"><span data-stu-id="5d641-113">Remarks</span></span>

<span data-ttu-id="5d641-114">`<exception>` 标记可用于指定可引发的异常。</span><span class="sxs-lookup"><span data-stu-id="5d641-114">The `<exception>` tag lets you specify which exceptions can be thrown.</span></span> <span data-ttu-id="5d641-115">此标记可应用于方法、属性、事件和索引器的定义。</span><span class="sxs-lookup"><span data-stu-id="5d641-115">This tag can be applied to definitions for methods, properties, events, and indexers.</span></span>

<span data-ttu-id="5d641-116">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="5d641-116">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span>

<span data-ttu-id="5d641-117">有关异常处理的详细信息，请参阅[异常和异常处理](../exceptions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="5d641-117">For more information about exception handling, see [Exceptions and Exception Handling](../exceptions/index.md).</span></span>

## <a name="example"></a><span data-ttu-id="5d641-118">示例</span><span class="sxs-lookup"><span data-stu-id="5d641-118">Example</span></span>

[!code-csharp[csProgGuideDocComments#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#4)]

## <a name="see-also"></a><span data-ttu-id="5d641-119">请参阅</span><span class="sxs-lookup"><span data-stu-id="5d641-119">See also</span></span>

- [<span data-ttu-id="5d641-120">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="5d641-120">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="5d641-121">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="5d641-121">Recommended tags for documentation comments</span></span>](recommended-tags-for-documentation-comments.md)
