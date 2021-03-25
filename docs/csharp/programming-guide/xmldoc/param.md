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
# <a name="param-c-programming-guide"></a>\<param>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<param name="name">description</param>
```

## <a name="parameters"></a>参数

- `name`

  方法参数的名称。 用双引号 (" ") 将名称引起来。

- `description`

  参数的说明。

## <a name="remarks"></a>备注

在方法声明的注释中，应使用 `<param>` 标记来描述方法参数之一。 若要记录多个参数，请使用多个 `<param>` 标记。

`<param>` 标记的文本显示在 IntelliSense、对象浏览器和代码注释 Web 报表中。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#1)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
