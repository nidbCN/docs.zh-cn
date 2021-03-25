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
# <a name="example-c-programming-guide"></a>\<example>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<example>description</example>
```

## <a name="parameters"></a>参数

- `description`

  代码示例的说明。

## <a name="remarks"></a>备注

借助 `<example>` 标记，可以指定如何使用方法或其他库成员的示例。 这通常涉及到使用 [\<code>](./code.md) 标记。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#3)]

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
