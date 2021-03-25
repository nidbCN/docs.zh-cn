---
title: <example> - C# 编程指南
description: 了解 XML <example> 标记。 借助此标记，可以指定如何使用方法或其他库成员的示例。
ms.date: 07/20/2015
f1_keywords:
- <example>
- example
helpviewer_keywords:
- <example> C# XML tag
- example C# XML tag
ms.assetid: 32d6e73b-2554-4abb-83ee-a1e321334fd2
ms.openlocfilehash: 8f9b4fa4ac447b853008576a46be9beeb583b018
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103479118"
---
# <a name="example-c-programming-guide"></a><span data-ttu-id="c5979-105">\<example>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="c5979-105">\<example> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="c5979-106">语法</span><span class="sxs-lookup"><span data-stu-id="c5979-106">Syntax</span></span>

```xml
<example>description</example>
```

## <a name="parameters"></a><span data-ttu-id="c5979-107">参数</span><span class="sxs-lookup"><span data-stu-id="c5979-107">Parameters</span></span>

- `description`

  <span data-ttu-id="c5979-108">代码示例的说明。</span><span class="sxs-lookup"><span data-stu-id="c5979-108">A description of the code sample.</span></span>

## <a name="remarks"></a><span data-ttu-id="c5979-109">备注</span><span class="sxs-lookup"><span data-stu-id="c5979-109">Remarks</span></span>

<span data-ttu-id="c5979-110">借助 `<example>` 标记，可以指定如何使用方法或其他库成员的示例。</span><span class="sxs-lookup"><span data-stu-id="c5979-110">The `<example>` tag lets you specify an example of how to use a method or other library member.</span></span> <span data-ttu-id="c5979-111">这通常涉及到使用 [\<code>](./code.md) 标记。</span><span class="sxs-lookup"><span data-stu-id="c5979-111">This commonly involves using the [\<code>](./code.md) tag.</span></span>

<span data-ttu-id="c5979-112">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="c5979-112">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span>

## <a name="example"></a><span data-ttu-id="c5979-113">示例</span><span class="sxs-lookup"><span data-stu-id="c5979-113">Example</span></span>

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

## <a name="see-also"></a><span data-ttu-id="c5979-114">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c5979-114">See also</span></span>

- [<span data-ttu-id="c5979-115">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="c5979-115">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="c5979-116">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="c5979-116">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)
