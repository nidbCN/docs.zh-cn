---
title: <permission> - C# 编程指南
description: 了解 XML <permission> 标记。 此标记可用于记录成员访问权限，而 PermissionSet 类可用于指定对成员的访问权限。
ms.date: 07/20/2015
f1_keywords:
- permission
- <permission>
helpviewer_keywords:
- <permission> C# XML tag
- permission C# XML tag
ms.assetid: 769e93fe-8404-443f-bf99-577aa42b6a49
ms.openlocfilehash: 379d3fda06c50e9e988784e671061d604e6e5b36
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477850"
---
# <a name="permission-c-programming-guide"></a>\<permission>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<permission cref="member">description</permission>
```

## <a name="parameters"></a>参数

- cref = " `member`"

  对可从当前编译环境调用的成员或字段的引用。 编译器检查是否存在给定的代码元素，并将 `member` 转换为输出 XML 中规范的元素名称。 成员必须出现在双引号 (" ") 内。

  有关如何创建对泛型类型的 cref 引用的信息，请参阅 [cref 特性](./cref-attribute.md)。

- `description`

  对成员访问权限的说明。

## <a name="remarks"></a>备注

使用 `<permission>` 标记可以记录成员访问权限。 <xref:System.Security.PermissionSet> 类可指定对成员的访问权限。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

[!code-csharp[csProgGuideDocComments#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#8)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
