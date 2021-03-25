---
title: <param> - C# 编程指南
description: 了解 XML <param> 标记。 在方法声明的注释中使用了此标记来描述方法参数之一。
ms.date: 07/20/2015
f1_keywords:
- param
- <param>
helpviewer_keywords:
- <param> C# XML tag
- param C# XML tag
ms.assetid: 46d329b1-5b84-4537-9e17-73ca97313e4e
ms.openlocfilehash: 1385365b2cdf18563686fdf4a5a1b17b89feafcd
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477998"
---
# <a name="param-c-programming-guide"></a><span data-ttu-id="164ba-105">\<param>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="164ba-105">\<param> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="164ba-106">语法</span><span class="sxs-lookup"><span data-stu-id="164ba-106">Syntax</span></span>

```xml
<param name="name">description</param>
```

## <a name="parameters"></a><span data-ttu-id="164ba-107">参数</span><span class="sxs-lookup"><span data-stu-id="164ba-107">Parameters</span></span>

- `name`

  <span data-ttu-id="164ba-108">方法参数的名称。</span><span class="sxs-lookup"><span data-stu-id="164ba-108">The name of a method parameter.</span></span> <span data-ttu-id="164ba-109">用双引号 (" ") 将名称引起来。</span><span class="sxs-lookup"><span data-stu-id="164ba-109">Enclose the name in double quotation marks (" ").</span></span>

- `description`

  <span data-ttu-id="164ba-110">参数的说明。</span><span class="sxs-lookup"><span data-stu-id="164ba-110">A description for the parameter.</span></span>

## <a name="remarks"></a><span data-ttu-id="164ba-111">备注</span><span class="sxs-lookup"><span data-stu-id="164ba-111">Remarks</span></span>

<span data-ttu-id="164ba-112">在方法声明的注释中，应使用 `<param>` 标记来描述方法参数之一。</span><span class="sxs-lookup"><span data-stu-id="164ba-112">The `<param>` tag should be used in the comment for a method declaration to describe one of the parameters for the method.</span></span> <span data-ttu-id="164ba-113">若要记录多个参数，请使用多个 `<param>` 标记。</span><span class="sxs-lookup"><span data-stu-id="164ba-113">To document multiple parameters, use multiple `<param>` tags.</span></span>

<span data-ttu-id="164ba-114">`<param>` 标记的文本显示在 IntelliSense、对象浏览器和代码注释 Web 报表中。</span><span class="sxs-lookup"><span data-stu-id="164ba-114">The text for the `<param>` tag is displayed in IntelliSense, the Object Browser, and the Code Comment Web Report.</span></span>

<span data-ttu-id="164ba-115">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="164ba-115">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span>

## <a name="example"></a><span data-ttu-id="164ba-116">示例</span><span class="sxs-lookup"><span data-stu-id="164ba-116">Example</span></span>

[!code-csharp[csProgGuideDocComments#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#1)]

## <a name="see-also"></a><span data-ttu-id="164ba-117">请参阅</span><span class="sxs-lookup"><span data-stu-id="164ba-117">See also</span></span>

- [<span data-ttu-id="164ba-118">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="164ba-118">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="164ba-119">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="164ba-119">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)
