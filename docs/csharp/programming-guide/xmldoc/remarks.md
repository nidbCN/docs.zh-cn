---
title: <remarks> - C# 编程指南
description: 了解 XML <remarks> 标记。 此标记用于添加有关某个类型的信息，从而补充由以下指定的信息： <summary>.
ms.date: 07/20/2015
f1_keywords:
- remarks
- <remarks>
helpviewer_keywords:
- remarks C# XML tag
- <remarks> C# XML tag
ms.assetid: f8641391-31f3-4735-af7a-c502a5b6a251
ms.openlocfilehash: 2227dd8bd4d81f5fda8cf529e18c7a613cca6b8e
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477826"
---
# <a name="remarks-c-programming-guide"></a><span data-ttu-id="20400-106">\<remarks>（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="20400-106">\<remarks> (C# programming guide)</span></span>

## <a name="syntax"></a><span data-ttu-id="20400-107">语法</span><span class="sxs-lookup"><span data-stu-id="20400-107">Syntax</span></span>

```xml
<remarks>description</remarks>
```

## <a name="parameters"></a><span data-ttu-id="20400-108">参数</span><span class="sxs-lookup"><span data-stu-id="20400-108">Parameters</span></span>

- `Description`

  <span data-ttu-id="20400-109">对成员的说明。</span><span class="sxs-lookup"><span data-stu-id="20400-109">A description of the member.</span></span>

## <a name="remarks"></a><span data-ttu-id="20400-110">备注</span><span class="sxs-lookup"><span data-stu-id="20400-110">Remarks</span></span>

<span data-ttu-id="20400-111">`<remarks>` 标记用于添加有关某个类型的信息，从而补充由 [\<summary>](./summary.md) 指定的信息。</span><span class="sxs-lookup"><span data-stu-id="20400-111">The `<remarks>` tag is used to add information about a type, supplementing the information specified with [\<summary>](./summary.md).</span></span> <span data-ttu-id="20400-112">此信息显示在对象浏览器窗口中。</span><span class="sxs-lookup"><span data-stu-id="20400-112">This information is displayed in the Object Browser window.</span></span>

<span data-ttu-id="20400-113">使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。</span><span class="sxs-lookup"><span data-stu-id="20400-113">Compile with [**DocumentationFile**](../../language-reference/compiler-options/output.md#documentationfile) to process documentation comments to a file.</span></span>

## <a name="example"></a><span data-ttu-id="20400-114">示例</span><span class="sxs-lookup"><span data-stu-id="20400-114">Example</span></span>

[!code-csharp[csProgGuideDocComments#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#9)]

## <a name="see-also"></a><span data-ttu-id="20400-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="20400-115">See also</span></span>

- [<span data-ttu-id="20400-116">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="20400-116">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="20400-117">建议的文档注释标记</span><span class="sxs-lookup"><span data-stu-id="20400-117">Recommended tags for documentation comments</span></span>](./recommended-tags-for-documentation-comments.md)
