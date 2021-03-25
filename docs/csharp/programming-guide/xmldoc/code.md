---
title: <code> - C# programming guide
description: 了解 XML <code> tag. This tag is used to indicate multiple lines of code, while <c> marks single-line text in a description as code.
ms.date: 07/20/2015
f1_keywords:
- code
- <code>
helpviewer_keywords:
- code XML tag
- <code> C# XML tag
ms.assetid: f235e3bc-a709-43cf-8a9f-bd57cabdf6da
ms.openlocfilehash: efc4314a634e3349fc5d7584b4c51130ff057e5b
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103479142"
---
# <a name="code-c-programming-guide"></a>\<code>（C# 编程指南）

## <a name="syntax"></a>语法

```xml
<code>content</code>
```

## <a name="parameters"></a>参数

- `content`

  要标记为代码的文本。

## <a name="remarks"></a>备注

`<code>` 标记用于指示多行代码。 使用 [\<c>](./code-inline.md) 指示应将说明内的单行文本标记为代码。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

有关如何使用 `<code>` 标记的示例，请参阅 [\<example>](./example.md) 一文。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
