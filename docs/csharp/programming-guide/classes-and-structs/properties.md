---
title: 属性 - C# 编程指南
description: C# 中的属性是一种成员，它使用访问器方法来读、写或计算私有字段的值，就像它是公共数据成员一样。
ms.date: 03/10/2017
f1_keywords:
- cs.properties
helpviewer_keywords:
- properties [C#]
- C# language, properties
ms.assetid: e295a8a2-b357-4ee7-a12e-385a44146fa8
ms.openlocfilehash: 6079fc5d2611ed1111d39d3f39e4c91817db528f
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103189873"
---
# <a name="properties-c-programming-guide"></a>属性（C# 编程指南）

属性是一种成员，它提供灵活的机制来读取、写入或计算私有字段的值。 属性可用作公共数据成员，但它们实际上是称为 *访问器* 的特殊方法。 这使得可以轻松访问数据，还有助于提高方法的安全性和灵活性。  

## <a name="properties-overview"></a>属性概述  
  
- 属性允许类公开获取和设置值的公共方法，而隐藏实现或验证代码。  
  
- [get](../../language-reference/keywords/get.md) 属性访问器用于返回属性值，而 [set](../../language-reference/keywords/set.md) 属性访问器用于分配新值。 在 C# 9 及更高版本中，[init](../../language-reference/keywords/init.md) 属性访问器仅用于在对象构造过程中分配新值。 这些访问器可以具有不同的访问级别。 有关详细信息，请参阅[限制访问器可访问性](./restricting-accessor-accessibility.md)。  
  
- [value](../../language-reference/keywords/value.md) 关键字用于定义由 `set` 或 `init` 访问器分配的值。  
- 属性可以是 *读-写* 属性（既有 `get` 访问器又有 `set` 访问器）、*只读* 属性（有 `get` 访问器，但没有 `set` 访问器）或 *只写* 访问器（有 `set` 访问器，但没有 `get` 访问器）。 只写属性很少出现，常用于限制对敏感数据的访问。

- 不需要自定义访问器代码的简单属性可以作为表达式主体定义或[自动实现的属性](./auto-implemented-properties.md)来实现。

## <a name="properties-with-backing-fields"></a>具有支持字段的属性

有一个实现属性的基本模式，该模式使用私有支持字段来设置和检索属性值。 `get` 访问器返回私有字段的值，`set` 访问器在向私有字段赋值之前可能会执行一些数据验证。 这两个访问器还可以在存储或返回数据之前对其执行某些转换或计算。

下面的示例阐释了此模式。 在此示例中，`TimePeriod` 类表示时间间隔。 在内部，该类将时间间隔以秒为单位存储在名为 `_seconds` 的私有字段中。 名为 `Hours` 的读-写属性允许客户以小时为单位指定时间间隔。 `get` 和 `set` 访问器都会执行小时与秒之间的必要转换。 此外，`set` 访问器还会验证数据，如果小时数无效，则引发 <xref:System.ArgumentOutOfRangeException>。

 [!code-csharp[Properties#1](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-1.cs)]  
  
## <a name="expression-body-definitions"></a>表达式主体定义  

 属性访问器通常由单行语句组成，这些语句只分配或只返回表达式的结果。 可以将这些属性作为 expression-bodied 成员来实现。 `=>` 符号后跟用于为属性赋值或从属性中检索值的表达式，即组成了表达式主体定义。

 从 C# 6 开始，只读属性可以将 `get` 访问器作为 expression-bodied 成员实现。 在这种情况下，既不使用 `get` 访问器关键字，也不使用 `return` 关键字。 下面的示例将只读 `Name` 属性作为 expression-bodied 成员实现。

 [!code-csharp[Properties#2](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-2.cs)]  

 从 C# 7.0 开始，`get` 和 `set` 访问器都可以作为 expression-bodied 成员实现。 在这种情况下，必须使用 `get` 和 `set` 关键字。 下面的示例阐释如何为这两个访问器使用表达式主体定义。 请注意，`return` 关键字不与 `get` 访问器搭配使用。

  [!code-csharp[Properties#3](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-3.cs)]  

## <a name="auto-implemented-properties"></a>自动实现的属性

在某些情况下，属性 `get` 和 `set` 访问器仅向支持字段赋值或仅从其中检索值，而不包括任何附加逻辑。 通过使用自动实现的属性，既能简化代码，还能让 C# 编译器透明地提供支持字段。

如果属性具有 `get` 和 `set`（或 `get` 和 `init`）访问器，则必须自动实现这两个访问器。 自动实现的属性通过以下方式定义：使用 `get` 和 `set` 关键字，但不提供任何实现。 下面的示例与上一个示例基本相同，只不过 `Name` 和 `Price` 是自动实现的属性。 该示例还删除了参数化构造函数，以便通过调用无参数构造函数和[对象初始值设定项](object-and-collection-initializers.md)立即初始化 `SaleItem` 对象。

  [!code-csharp[Properties#4](../../../../samples/snippets/csharp/programming-guide/classes-and-structs/properties-4.cs)]  

## <a name="related-sections"></a>相关章节  
  
- [使用属性](./using-properties.md)  
  
- [接口属性](./interface-properties.md)  
  
- [属性与索引器之间的比较](../indexers/comparison-between-properties-and-indexers.md)  
  
- [限制访问器可访问性](./restricting-accessor-accessibility.md)  
  
- [自动实现的属性](./auto-implemented-properties.md)  
  
## <a name="c-language-specification"></a>C# 语言规范  

有关详细信息，请参阅 [C# 语言规范](/dotnet/csharp/language-reference/language-specification/introduction)中的[属性](~/_csharplang/spec/classes.md#properties)。 该语言规范是 C# 语法和用法的权威资料。
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [使用属性](./using-properties.md)
- [索引器](../indexers/index.md)
- [get 关键字](../../language-reference/keywords/get.md)
- [set 关键字](../../language-reference/keywords/set.md)
