---
title: <c> - C# 编程指南
description: 了解 XML <c> 标记。 此标记将说明中的单行文本标记为代码，而 <code> indicates multiple lines.
ms.date: 07/20/2015
f1_keywords:
- c
- <c>
helpviewer_keywords:
- text, marking as code [C#]
- code, marking text as [C#]
- c C# XML tag
- <c> C# XML tag
ms.assetid: aad5b16e-a29e-445e-bd0d-eea0b138d7b2
ms.openlocfilehash: fc445c7245287c3835543e4bbe4b3b46ec46fd35
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478698"
---
# <a name="c-c-programming-guide"></a><span data-ttu-id="2bba7-104">\<c>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="2bba7-104">\<c> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="2bba7-105">语法</span><span class="sxs-lookup"><span data-stu-id="2bba7-105">Syntax</span></span>

```xml
<c>text</c>
```

## <a name="parameters"></a><span data-ttu-id="2bba7-106">参数</span><span class="sxs-lookup"><span data-stu-id="2bba7-106">Parameters</span></span>

- `text`

  <span data-ttu-id="2bba7-107">要指示为代码的文本。</span><span class="sxs-lookup"><span data-stu-id="2bba7-107">The text you would like to indicate as code.</span></span>

## <a name="remarks"></a><span data-ttu-id="2bba7-108">备注</span><span class="sxs-lookup"><span data-stu-id="2bba7-108">Remarks</span></span>

<span data-ttu-id="2bba7-109">使用 `<c>` 标记可以指示应将说明内的文本标记为代码。</span><span class="sxs-lookup"><span data-stu-id="2bba7-109">The `<c>` tag gives you a way to indicate that text within a description should be marked as code.</span></span> <span data-ttu-id="2bba7-110">使用 [\<code>](./code.md) 指示作为代码的多行文本。</span><span class="sxs-lookup"><span data-stu-id="2bba7-110">Use [\<code>](./code.md) to indicate multiple lines as code.</span></span>

<span data-ttu-id="2bba7-111">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="2bba7-111">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span>

## <a name="example"></a><span data-ttu-id="2bba7-112">示例</span><span class="sxs-lookup"><span data-stu-id="2bba7-112">Example</span></span>

[!code-csharp[csProgGuideDocComments#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#2)]
  
## <a name="see-also"></a><span data-ttu-id="2bba7-113">请参阅</span><span class="sxs-lookup"><span data-stu-id="2bba7-113">See also</span></span>

- [<span data-ttu-id="2bba7-114">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="2bba7-114">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="2bba7-115">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="2bba7-115">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)
