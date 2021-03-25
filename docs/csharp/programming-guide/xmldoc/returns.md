---
title: <returns> - C# 编程指南
description: 了解 XML <returns> 标记。 在方法声明的注释中使用了此标记来描述返回值。
ms.date: 07/20/2015
f1_keywords:
- returns
- <returns>
helpviewer_keywords:
- <returns> C# XML tag
- returns C# XML tag
ms.assetid: bb2d9958-62fc-47c7-9511-6311171f119f
ms.openlocfilehash: 6a098208a51ca31fe2278b7c696deb15a13f8e1e
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477804"
---
# <a name="returns-c-programming-guide"></a>\<returns>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<returns>description</returns>
```

## <a name="parameters"></a>参数

- `description`

  返回值的说明。

## <a name="remarks"></a>备注

在方法声明的注释中应使用 `<returns>` 标记来描述返回值。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#10)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
