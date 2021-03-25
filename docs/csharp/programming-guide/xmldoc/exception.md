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
# <a name="exception-c-programming-guide"></a>\<exception>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<exception cref="member">description</exception>
```

## <a name="parameters"></a>参数

- cref = " `member`"

  对当前编译环境中出现的一个异常的引用。 编译器检查是否存在给定的异常，并将 `member` 转换为输出 XML 中的规范的元素名称。 `member` 必须出现在双引号 (" ") 内。

  有关如何设置 `member` 格式以引用泛型类型的详细信息，请参阅[处理 XML 文件](processing-the-xml-file.md)。

- `description`

  异常的说明。

## <a name="remarks"></a>备注

`<exception>` 标记可用于指定可引发的异常。 此标记可应用于方法、属性、事件和索引器的定义。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

有关异常处理的详细信息，请参阅[异常和异常处理](../exceptions/index.md)。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#4)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](recommended-tags-for-documentation-comments.md)
