---
title: <para> - C# 编程指南
description: 了解 XML <para> 标记。 使用此标记，可以将结构添加到另一个标记中的文本，例如 <summary>, <remarks>或 <returns>.
ms.date: 07/20/2015
f1_keywords:
- <para>
- para
helpviewer_keywords:
- <para> C# XML tag
- para C# XML tag
ms.assetid: c74b8705-29df-40b1-bff5-237492b0e978
ms.openlocfilehash: 5cb1c22dceae7a45a47fcb8807303d11e1220935
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478006"
---
# <a name="para-c-programming-guide"></a>\<para>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<para>content</para>
```

## <a name="parameters"></a>参数

- `content`

  段落文本。

## <a name="remarks"></a>备注

`<para>` 标记用于标记内，例如 [\<summary>](./summary.md)、[\<remarks>](./remarks.md) 或 [\<returns>](./returns.md)，允许向文本添加结构。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

有关使用 `<para>` 的示例，请参阅 [\<summary>](./summary.md)。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
