---
description: init 关键字 - C# 参考
title: init 关键字 - C# 参考
ms.date: 03/03/2021
f1_keywords:
- init
- init_CSharpKeyword
helpviewer_keywords:
- init keyword [C#]
ms.openlocfilehash: 2271b5332c8bfd3223d0c034a44eca4e2ca0ca54
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190431"
---
# <a name="init-c-reference"></a>init（C# 参考）

在 C# 9 及更高版本中，`init` 关键字在属性或索引器中定义访问器方法。 init-only 资源库仅在对象构造期间为属性或索引器元素赋值。 有关详细信息和示例，请参阅[属性](../../programming-guide/classes-and-structs/properties.md)、[自动实现的属性](../../programming-guide/classes-and-structs/auto-implemented-properties.md)和[索引器](../../programming-guide/indexers/index.md)。

以下示例为名为 `Seconds` 的属性同时定义 `get` 和 `init` 访问器。 它使用名为 `_seconds` 的私有字段备份属性值。

[!code-csharp[init#1](snippets/InitExample1.cs)]

通常，`init` 访问器包含分配一个值的单个语句，如前面的示例所示。 可以将 `init` 访问器作为表达式主体成员实现。 下面的示例将 `get` 和 `init` 访问器都作为表达式主体成员实现。

[!code-csharp[init#3](snippets/InitExample3.cs)]
  
对于属性的 `get` 和 `init` 访问器不执行除设置或检索私有支持字段中的值以外的任何其他操作的简单情况，可以利用 C# 编译器对自动实现的属性的支持。 以下示例将 `Hours` 作为自动实现的属性来实现。

[!code-csharp[init#2](snippets/InitExample2.cs)]
  
## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [属性](../../programming-guide/classes-and-structs/properties.md)
