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
# <a name="c-c-programming-guide"></a>\<c>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<c>text</c>
```

## <a name="parameters"></a>参数

- `text`

  要指示为代码的文本。

## <a name="remarks"></a>备注

使用 `<c>` 标记可以指示应将说明内的文本标记为代码。 使用 [\<code>](./code.md) 指示作为代码的多行文本。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#2)]
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
