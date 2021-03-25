---
title: <typeparamref> - C# 编程指南
description: 了解 XML <typeparamref> 标记。 通过此标记，文档文件的使用者可显著设置字体格式，例如采用斜体。
ms.date: 07/20/2015
f1_keywords:
- typeparamref
helpviewer_keywords:
- typeparamref C# XML tag
- <typeparamref> C# XML tag
ms.assetid: 6d8ffc58-12c5-4688-8db6-833a7ded5886
ms.openlocfilehash: 1bb9a73f4122f3b9d521565a7172a9b8f75f7a98
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477666"
---
# <a name="typeparamref-c-programming-guide"></a>\<typeparamref>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<typeparamref name="name"/>
```

## <a name="parameters"></a>parameters

- `name`

  类型参数的名称。 用双引号 (" ") 将名称引起来。

## <a name="remarks"></a>备注

有关泛型类型中的类型参数及方法的详细信息，请参阅[泛型](../generics/index.md)。

通过此标记，文档文件的使用者可显著设置字体格式，例如采用斜体。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#13)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
