---
title: 泛型接口 - C# 编程指南
description: 了解如何通过 C# 使用泛型接口。 查看代码示例和其他可用资源。
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generic interfaces
- generics [C#], interfaces
ms.assetid: a8fa49a1-6e78-4a09-87e5-84a0b9f5ffbe
ms.openlocfilehash: 6089e14bd2b13268a03d3600ef8bf78f9afa1c6d
ms.sourcegitcommit: bdbf6472de867a0a11aaa5b9384a2506c24f27d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102206734"
---
# <a name="generic-interfaces-c-programming-guide"></a><span data-ttu-id="7a502-104">泛型接口（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="7a502-104">Generic Interfaces (C# Programming Guide)</span></span>

<span data-ttu-id="7a502-105">为泛型集合类或表示集合中的项的泛型类定义接口通常很有用处。</span><span class="sxs-lookup"><span data-stu-id="7a502-105">It is often useful to define interfaces either for generic collection classes, or for the generic classes that represent items in the collection.</span></span> <span data-ttu-id="7a502-106">为避免对值类型的装箱和取消装箱操作，泛型类的首选项使用泛型接口，例如 <xref:System.IComparable%601>而不是 <xref:System.IComparable>。</span><span class="sxs-lookup"><span data-stu-id="7a502-106">The preference for generic classes is to use generic interfaces, such as <xref:System.IComparable%601> rather than <xref:System.IComparable>, in order to avoid boxing and unboxing operations on value types.</span></span> <span data-ttu-id="7a502-107">.NET 类库定义多个泛型接口，以便用于 <xref:System.Collections.Generic> 命名空间中的集合类。</span><span class="sxs-lookup"><span data-stu-id="7a502-107">The .NET class library defines several generic interfaces for use with the collection classes in the <xref:System.Collections.Generic> namespace.</span></span>  
  
 <span data-ttu-id="7a502-108">接口被指定为类型参数上的约束时，仅可使用实现接口的类型。</span><span class="sxs-lookup"><span data-stu-id="7a502-108">When an interface is specified as a constraint on a type parameter, only types that implement the interface can be used.</span></span> <span data-ttu-id="7a502-109">如下代码示例演示一个派生自 `GenericList<T>` 类的 `SortedList<T>` 类。</span><span class="sxs-lookup"><span data-stu-id="7a502-109">The following code example shows a `SortedList<T>` class that derives from the `GenericList<T>` class.</span></span> <span data-ttu-id="7a502-110">有关详细信息，请参阅[泛型介绍](./index.md)。</span><span class="sxs-lookup"><span data-stu-id="7a502-110">For more information, see [Introduction to Generics](./index.md).</span></span> <span data-ttu-id="7a502-111">`SortedList<T>` 添加约束 `where T : IComparable<T>`。</span><span class="sxs-lookup"><span data-stu-id="7a502-111">`SortedList<T>` adds the constraint `where T : IComparable<T>`.</span></span> <span data-ttu-id="7a502-112">这可使 `SortedList<T>` 中的 `BubbleSort` 方法在列表元素上使用泛型 <xref:System.IComparable%601.CompareTo%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="7a502-112">This enables the `BubbleSort` method in `SortedList<T>` to use the generic <xref:System.IComparable%601.CompareTo%2A> method on list elements.</span></span> <span data-ttu-id="7a502-113">在此示例中，列表元素是一个实现 `IComparable<Person>` 的简单类 `Person`。</span><span class="sxs-lookup"><span data-stu-id="7a502-113">In this example, list elements are a simple class, `Person`, that implements `IComparable<Person>`.</span></span>  
  
 [!code-csharp[csProgGuideGenerics#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics2.cs#29)]  
  
 <span data-ttu-id="7a502-114">可将多个接口指定为单个类型上的约束，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7a502-114">Multiple interfaces can be specified as constraints on a single type, as follows:</span></span>  
  
 [!code-csharp[csProgGuideGenerics#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#30)]  
  
 <span data-ttu-id="7a502-115">一个接口可定义多个类型参数，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7a502-115">An interface can define more than one type parameter, as follows:</span></span>  
  
 [!code-csharp[csProgGuideGenerics#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#31)]  
  
 <span data-ttu-id="7a502-116">适用于类的继承规则也适用于接口：</span><span class="sxs-lookup"><span data-stu-id="7a502-116">The rules of inheritance that apply to classes also apply to interfaces:</span></span>  
  
 [!code-csharp[csProgGuideGenerics#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#32)]  
  
 <span data-ttu-id="7a502-117">如果泛型接口是协变的（即，仅使用自身的类型参数作为返回值），那么这些接口可继承自非泛型接口。</span><span class="sxs-lookup"><span data-stu-id="7a502-117">Generic interfaces can inherit from non-generic interfaces if the generic interface is covariant, which means it only uses its type parameter as a return value.</span></span> <span data-ttu-id="7a502-118">在 .NET 类库中，<xref:System.Collections.Generic.IEnumerable%601> 继承自 <xref:System.Collections.IEnumerable>，因为 <xref:System.Collections.Generic.IEnumerable%601> 在 <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> 的返回值和 <xref:System.Collections.Generic.IEnumerator%601.Current%2A> 属性 Getter 中仅使用 `T`。</span><span class="sxs-lookup"><span data-stu-id="7a502-118">In the .NET class library, <xref:System.Collections.Generic.IEnumerable%601> inherits from <xref:System.Collections.IEnumerable> because <xref:System.Collections.Generic.IEnumerable%601> only uses `T` in the return value of <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> and in the <xref:System.Collections.Generic.IEnumerator%601.Current%2A> property getter.</span></span>  
  
 <span data-ttu-id="7a502-119">具体类可实现封闭式构造接口，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7a502-119">Concrete classes can implement closed constructed interfaces, as follows:</span></span>  
  
 [!code-csharp[csProgGuideGenerics#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#33)]  
  
 <span data-ttu-id="7a502-120">只要类形参列表提供接口所需的所有实参，泛型类即可实现泛型接口或封闭式构造接口，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7a502-120">Generic classes can implement generic interfaces or closed constructed interfaces as long as the class parameter list supplies all arguments required by the interface, as follows:</span></span>  
  
 [!code-csharp[csProgGuideGenerics#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#34)]  
  
 <span data-ttu-id="7a502-121">控制方法重载的规则对泛型类、泛型结构或泛型接口内的方法一样。</span><span class="sxs-lookup"><span data-stu-id="7a502-121">The rules that control method overloading are the same for methods within generic classes, generic structs, or generic interfaces.</span></span> <span data-ttu-id="7a502-122">有关详细信息，请参阅[泛型方法](./generic-methods.md)。</span><span class="sxs-lookup"><span data-stu-id="7a502-122">For more information, see [Generic Methods](./generic-methods.md).</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="7a502-123">另请参阅</span><span class="sxs-lookup"><span data-stu-id="7a502-123">See also</span></span>

- [<span data-ttu-id="7a502-124">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="7a502-124">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="7a502-125">泛型介绍</span><span class="sxs-lookup"><span data-stu-id="7a502-125">Introduction to Generics</span></span>](./index.md)
- [<span data-ttu-id="7a502-126">interface</span><span class="sxs-lookup"><span data-stu-id="7a502-126">interface</span></span>](../../language-reference/keywords/interface.md)
- [<span data-ttu-id="7a502-127">泛型</span><span class="sxs-lookup"><span data-stu-id="7a502-127">Generics</span></span>](../../../standard/generics/index.md)
