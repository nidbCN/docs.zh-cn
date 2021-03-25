---
title: <typeparam> - C# 编程指南
description: 了解 XML <typeparam> 标记。 在泛型类型或方法声明的注释中使用了此标记来描述类型参数。
ms.date: 07/20/2015
f1_keywords:
- typeparam
helpviewer_keywords:
- <typeparam> C# XML tag
- typeparam C# XML tag
ms.assetid: 9b99d400-e911-4e55-99c6-64367c96aa4f
ms.openlocfilehash: 1b9d5d30c1a507e3bb7938ee0c036cdac3ef2f90
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477678"
---
# <a name="typeparam-c-programming-guide"></a><span data-ttu-id="5a051-105">\<typeparam>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="5a051-105">\<typeparam> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="5a051-106">语法</span><span class="sxs-lookup"><span data-stu-id="5a051-106">Syntax</span></span>

```xml
<typeparam name="name">description</typeparam>
```

## <a name="parameters"></a><span data-ttu-id="5a051-107">parameters</span><span class="sxs-lookup"><span data-stu-id="5a051-107">Parameters</span></span>

- `name`

  <span data-ttu-id="5a051-108">类型参数的名称。</span><span class="sxs-lookup"><span data-stu-id="5a051-108">The name of the type parameter.</span></span> <span data-ttu-id="5a051-109">用双引号 (" ") 将名称引起来。</span><span class="sxs-lookup"><span data-stu-id="5a051-109">Enclose the name in double quotation marks (" ").</span></span>

- `description`

  <span data-ttu-id="5a051-110">类型参数的说明。</span><span class="sxs-lookup"><span data-stu-id="5a051-110">A description for the type parameter.</span></span>

## <a name="remarks"></a><span data-ttu-id="5a051-111">备注</span><span class="sxs-lookup"><span data-stu-id="5a051-111">Remarks</span></span>

<span data-ttu-id="5a051-112">在泛型类型或方法声明的注释中，应使用 `<typeparam>` 标记来描述类型参数。</span><span class="sxs-lookup"><span data-stu-id="5a051-112">The `<typeparam>` tag should be used in the comment for a generic type or method declaration to describe a type parameter.</span></span> <span data-ttu-id="5a051-113">为泛型类型或方法的每个类型参数添加标记。</span><span class="sxs-lookup"><span data-stu-id="5a051-113">Add a tag for each type parameter of the generic type or method.</span></span>

<span data-ttu-id="5a051-114">有关详细信息，请参阅[泛型](../generics/index.md)。</span><span class="sxs-lookup"><span data-stu-id="5a051-114">For more information, see [Generics](../generics/index.md).</span></span>

<span data-ttu-id="5a051-115">`<typeparam>` 标记的文本将显示在 IntelliSense、[对象浏览器窗口](/visualstudio/ide/viewing-the-structure-of-code#BKMK_ObjectBrowser)代码注释 Web 报表。</span><span class="sxs-lookup"><span data-stu-id="5a051-115">The text for the `<typeparam>` tag will be displayed in IntelliSense, the [Object Browser Window](/visualstudio/ide/viewing-the-structure-of-code#BKMK_ObjectBrowser) code comment web report.</span></span>

<span data-ttu-id="5a051-116">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="5a051-116">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span>

## <a name="example"></a><span data-ttu-id="5a051-117">示例</span><span class="sxs-lookup"><span data-stu-id="5a051-117">Example</span></span>

[!code-csharp[csProgGuideDocComments#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#13)]

## <a name="see-also"></a><span data-ttu-id="5a051-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="5a051-118">See also</span></span>

- [<span data-ttu-id="5a051-119">C# 参考</span><span class="sxs-lookup"><span data-stu-id="5a051-119">C# reference</span></span>](../../language-reference/index.md)
- [<span data-ttu-id="5a051-120">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="5a051-120">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="5a051-121">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="5a051-121">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)
