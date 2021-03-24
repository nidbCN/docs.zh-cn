---
title: 接口 - C# 编程指南
description: C# 中的接口包含非抽象类或结构必须实现的一组相关功能的定义。
ms.date: 02/20/2020
helpviewer_keywords:
- interfaces [C#]
- C# language, interfaces
ms.assetid: 2feda177-ce11-432d-81b4-d50f5f35fd37
ms.openlocfilehash: df96ce4cc2348637b262b7380832dde1e2ee3afb
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876329"
---
# <a name="interfaces-c-programming-guide"></a>接口（C# 编程指南）

接口包含非抽象[类](../../language-reference/keywords/class.md)或[结构](../../language-reference/builtin-types/struct.md)必须实现的一组相关功能的定义。 接口可以定义 `static` 方法，此类方法必须具有实现。 从 C# 8.0 开始，接口可为成员定义默认实现。 接口不能声明实例数据，如字段、自动实现的属性或类似属性的事件。

例如，使用接口可以在类中包括来自多个源的行为。 该功能在 C# 中十分重要，因为该语言不支持类的多重继承。 此外，如果要模拟结构的继承，也必须使用接口，因为它们无法实际从另一个结构或类继承。

可使用 [interface](../../language-reference/keywords/interface.md) 关键字定义接口，如以下示例所示。

[!code-csharp[Equatable](~/samples/snippets/csharp/objectoriented/interfaces.cs#Equatable)]

接口名称必须是有效的 C# [标识符名称](../inside-a-program/identifier-names.md)。 按照约定，接口名称以大写字母 `I` 开头。

实现 <xref:System.IEquatable%601> 接口的任何类或结构都必须包含与该接口指定的签名匹配的 <xref:System.IEquatable%601.Equals%2A> 方法的定义。 因此，可以依靠实现 `IEquatable<T>` 的类来包含 `Equals` 方法，类的实例可以通过该方法确定它是否等于相同类的另一个实例。

`IEquatable<T>` 的定义不为 `Equals` 提供实现。 类或结构可以实现多个接口，但是类只能从单个类继承。

有关抽象类的详细信息，请参阅[抽象类、密封类及类成员](../classes-and-structs/abstract-and-sealed-classes-and-class-members.md)。

接口可以包含实例方法、属性、事件、索引器或这四种成员类型的任意组合。 接口可以包含静态构造函数、字段、常量或运算符。 有关示例的链接，请参阅[相关部分](./index.md#BKMK_RelatedSections)。 接口不能包含实例字段、实例构造函数或终结器。 接口成员默认是公共的，可以显式指定可访问性修饰符（如 `public`、`protected`、`internal`、`private`、`protected internal` 或 `private protected`）。 `private` 成员必须有默认实现。

若要实现接口成员，实现类的对应成员必须是公共、非静态，并且具有与接口成员相同的名称和签名。

当类或结构实现接口时，类或结构必须为该接口声明的所有成员提供实现，但不提供默认实现。 但是，如果基类实现接口，则从基类派生的任何类都会继承该实现。

下面的示例演示 <xref:System.IEquatable%601> 接口的实现。 实现类 `Car` 必须提供 <xref:System.IEquatable%601.Equals%2A> 方法的实现。

[!code-csharp[ImplementEquatable](~/samples/snippets/csharp/objectoriented/interfaces.cs#ImplementEquatable)]

类的属性和索引器可以为接口中定义的属性或索引器定义额外的访问器。 例如，接口可能会声明包含 [get](../../language-reference/keywords/get.md) 取值函数的属性。 实现此接口的类可以声明包含 `get` 和 [set](../../language-reference/keywords/set.md) 取值函数的同一属性。 但是，如果属性或索引器使用显式实现，则访问器必须匹配。 有关显式实现的详细信息，请参阅[显式接口实现](explicit-interface-implementation.md)和[接口属性](../classes-and-structs/interface-properties.md)。

接口可从一个或多个接口继承。 派生接口从其基接口继承成员。 实现派生接口的类必须实现派生接口中的所有成员，包括派生接口的基接口的所有成员。 该类可能会隐式转换为派生接口或任何其基接口。 类可能通过它继承的基类或通过其他接口继承的接口来多次包含某个接口。 但是，类只能提供接口的实现一次，并且仅当类将接口作为类定义的一部分 (`class ClassName : InterfaceName`) 进行声明时才能提供。 如果由于继承实现接口的基类而继承了接口，则基类会提供接口的成员的实现。 但是，派生类可以重新实现任何虚拟接口成员，而不是使用继承的实现。 当接口声明方法的默认实现时，实现该接口的任何类都会继承该实现。 接口中定义的实现是虚拟的，实现类可能会替代该实现。

基类还可以使用虚拟成员实现接口成员。 在这种情况下，派生类可以通过重写虚拟成员来更改接口行为。 有关虚拟成员的详细信息，请参阅[多态性](../classes-and-structs/polymorphism.md)。

## <a name="interfaces-summary"></a>接口摘要

接口具有以下属性：

- 接口通常类似于只有抽象成员的抽象基类。 实现接口的任何类或结构都必须实现其所有成员。 接口可以选择性地定义其部分或全部成员的默认实现。 有关详细信息，请参阅[默认接口方法](../../whats-new/tutorials/default-interface-methods-versions.md)。
- 接口无法直接进行实例化。 其成员由实现接口的任何类或结构来实现。
- 一个类或结构可以实现多个接口。 一个类可以继承一个基类，还可实现一个或多个接口。

## <a name="related-sections"></a><a name="BKMK_RelatedSections"></a>相关部分

- [接口属性](../classes-and-structs/interface-properties.md)  
- [接口中的索引器](../indexers/indexers-in-interfaces.md)  
- [如何实现接口事件](../events/how-to-implement-interface-events.md)
- [类和结构](../classes-and-structs/index.md)  
- [继承](../classes-and-structs/inheritance.md)  
- [接口](../../language-reference/keywords/interface.md)
- [方法](../classes-and-structs/methods.md)  
- [多态性](../classes-and-structs/polymorphism.md)  
- [抽象类、密封类及类成员](../classes-and-structs/abstract-and-sealed-classes-and-class-members.md)  
- [属性](../classes-and-structs/properties.md)  
- [事件](../events/index.md)  
- [索引器](../indexers/index.md)
  
## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [继承](../classes-and-structs/inheritance.md)
- [标识符名称](../inside-a-program/identifier-names.md)
