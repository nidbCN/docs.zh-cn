---
title: 自动实现的属性 - C# 编程指南
description: 对于 C# 中自动实现的属性，编译器将创建仅通过属性的 get 和 set 访问器访问的专用、匿名支持字段。
ms.date: 01/31/2020
helpviewer_keywords:
- auto-implemented properties [C#]
- properties [C#], auto-implemented
ms.assetid: aa55fa97-ccec-431f-b5e9-5ac789fd32b7
ms.openlocfilehash: ef3e2d6dd5851801ea06d65b87c2274d8e44b4f1
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190276"
---
# <a name="auto-implemented-properties-c-programming-guide"></a>自动实现的属性（C# 编程指南）

在 C# 3.0 及更高版本，当属性访问器中不需要任何其他逻辑时，自动实现的属性会使属性声明更加简洁。 它们还允许客户端代码创建对象。 当你声明以下示例中所示的属性时，编译器将创建仅可以通过该属性的 `get` 和 `set` 访问器访问的专用、匿名支持字段。 在 C# 9 及更高版本中， `init` 访问器也可以声明为自动实现的属性。
  
## <a name="example"></a>示例

下列示例演示一个简单的类，它具有某些自动实现的属性：  

[!code-csharp[csProgGuideLINQ#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#28)]  

不能在接口中声明自动实现的属性。 自动实现的属性声明一个私有实例支持字段，并且接口可能不声明实例字段。 如果在接口中声明属性而不定义主体，请使用访问器声明属性，访问器必须由实现该接口的每个类型实现。

在 C# 6 和更高版本中，你可以像字段一样初始化自动实现属性：  

```csharp  
public string FirstName { get; set; } = "Jane";  
```  

上一示例中所示的类是可变的。 客户端代码在创建后可以更改对象中的值。 在包含重要行为（方法）以及数据的复杂类中，通常有必要具有公共属性。 但是，对于那些仅封装一组值（数据）且很少或没有行为的小型类或结构，应该使用以下选项之一使对象不可变：

* 只声明 `get` 访问器（除了能在构造函数中可变，在其他任何位置都不可变）。
* 声明 `get` 访问器和 `init` 访问器（除了能在对象构造函数中可变，在其他任何位置都不可变）。
* 将 `set` 访问器声明为[专用](../../language-reference/keywords/private.md)（对使用者不可变）。

有关详细信息，请参阅[如何使用自动实现的属性实现轻量类](./how-to-implement-a-lightweight-class-with-auto-implemented-properties.md)。

## <a name="see-also"></a>请参阅

- [属性](./properties.md)
- [修饰符](../../language-reference/keywords/index.md)
