---
title: 相等性比较 - C# 编程指南
description: 了解相等性比较。 查看“值相等性”和“引用相等性”的说明，并查看其他资源。
ms.date: 07/20/2015
helpviewer_keywords:
- object equality [C#]
ms.assetid: 10b865ea-4e7b-4127-9242-c9b8f57d9f04
ms.openlocfilehash: 3bc41e9adeff23dc385d0888163f9edf04772595
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259612"
---
# <a name="equality-comparisons-c-programming-guide"></a><span data-ttu-id="ac5d9-104">相等性比较（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="ac5d9-104">Equality comparisons (C# Programming Guide)</span></span>

<span data-ttu-id="ac5d9-105">有时需要比较两个值是否相等。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-105">It is sometimes necessary to compare two values for equality.</span></span> <span data-ttu-id="ac5d9-106">在某些情况下，测试的是“值相等性”，也称为“等效性”，这意味着两个变量包含的值相等。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-106">In some cases, you are testing for *value equality*, also known as *equivalence*, which means that the values that are contained by the two variables are equal.</span></span> <span data-ttu-id="ac5d9-107">在其他情况下，必须确定两个变量是否引用内存中的同一基础对象。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-107">In other cases, you have to determine whether two variables refer to the same underlying object in memory.</span></span> <span data-ttu-id="ac5d9-108">此类型的相等性称为“引用相等性”或“标识”。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-108">This type of equality is called *reference equality*, or *identity*.</span></span> <span data-ttu-id="ac5d9-109">本主题介绍这两种相等性，并提供指向其他主题的链接，供用户了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-109">This topic describes these two kinds of equality and provides links to other topics for more information.</span></span>  
  
## <a name="reference-equality"></a><span data-ttu-id="ac5d9-110">引用相等性</span><span class="sxs-lookup"><span data-stu-id="ac5d9-110">Reference equality</span></span>

 <span data-ttu-id="ac5d9-111">引用相等性指两个对象引用均引用同一基础对象。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-111">Reference equality means that two object references refer to the same underlying object.</span></span> <span data-ttu-id="ac5d9-112">这可以通过简单的赋值来实现，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-112">This can occur through simple assignment, as shown in the following example.</span></span>  
  
 [!code-csharp[csProgGuideStatements#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#18)]  
  
 <span data-ttu-id="ac5d9-113">在此代码中，创建了两个对象，但在赋值语句后，这两个引用所引用的是同一对象。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-113">In this code, two objects are created, but after the assignment statement, both references refer to the same object.</span></span> <span data-ttu-id="ac5d9-114">因此，它们具有引用相等性。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-114">Therefore they have reference equality.</span></span> <span data-ttu-id="ac5d9-115">使用 <xref:System.Object.ReferenceEquals%2A> 方法确定两个引用是否引用同一对象。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-115">Use the <xref:System.Object.ReferenceEquals%2A> method to determine whether two references refer to the same object.</span></span>  
  
<span data-ttu-id="ac5d9-116">引用相等性的概念仅适用于引用类型。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-116">The concept of reference equality applies only to reference types.</span></span> <span data-ttu-id="ac5d9-117">由于在将值类型的实例赋给变量时将产生值的副本，因此值类型对象无法具有引用相等性。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-117">Value type objects cannot have reference equality because when an instance of a value type is assigned to a variable, a copy of the value is made.</span></span> <span data-ttu-id="ac5d9-118">因此，永远不会有两个未装箱结构引用内存中的同一位置。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-118">Therefore you can never have two unboxed structs that refer to the same location in memory.</span></span> <span data-ttu-id="ac5d9-119">此外，如果使用 <xref:System.Object.ReferenceEquals%2A> 比较两个值类型，结果将始终为 `false`，即使对象中包含的值都相同也是如此。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-119">Furthermore, if you use <xref:System.Object.ReferenceEquals%2A> to compare two value types, the result will always be `false`, even if the values that are contained in the objects are all identical.</span></span> <span data-ttu-id="ac5d9-120">这是因为会将每个变量装箱到单独的对象实例中。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-120">This is because each variable is boxed into a separate object instance.</span></span> <span data-ttu-id="ac5d9-121">有关详细信息，请参阅[如何测试引用相等性（标识）](./how-to-test-for-reference-equality-identity.md)。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-121">For more information, see [How to test for reference equality (Identity)](./how-to-test-for-reference-equality-identity.md).</span></span>

## <a name="value-equality"></a><span data-ttu-id="ac5d9-122">值相等性</span><span class="sxs-lookup"><span data-stu-id="ac5d9-122">Value equality</span></span>

 <span data-ttu-id="ac5d9-123">值相等性指两个对象包含相同的一个或多个值。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-123">Value equality means that two objects contain the same value or values.</span></span> <span data-ttu-id="ac5d9-124">对于基元值类型（例如 [int](../../language-reference/builtin-types/integral-numeric-types.md) 或 [bool](../../language-reference/builtin-types/bool.md)），针对值相等性的测试简单明了。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-124">For primitive value types such as [int](../../language-reference/builtin-types/integral-numeric-types.md) or [bool](../../language-reference/builtin-types/bool.md), tests for value equality are straightforward.</span></span> <span data-ttu-id="ac5d9-125">可以使用 [==](../../language-reference/operators/equality-operators.md#equality-operator-) 运算符，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-125">You can use the [==](../../language-reference/operators/equality-operators.md#equality-operator-) operator, as shown in the following example.</span></span>  
  
```csharp  
int a = GetOriginalValue();  
int b = GetCurrentValue();  
  
// Test for value equality.
if (b == a)
{  
    // The two integers are equal.  
}  
```  
  
 <span data-ttu-id="ac5d9-126">对于大多数其他类型，针对值相等性的测试较为复杂，因为它需要用户了解类型对值相等性的定义方式。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-126">For most other types, testing for value equality is more complex because it requires that you understand how the type defines it.</span></span> <span data-ttu-id="ac5d9-127">对于具有多个字段或属性的类和结构，值相等性的定义通常指所有字段或属性都具有相同的值。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-127">For classes and structs that have multiple fields or properties, value equality is often defined to mean that all fields or properties have the same value.</span></span> <span data-ttu-id="ac5d9-128">例如，如果 pointA.X 等于 pointB.X，并且 pointA.Y 等于 pointB.Y，则可以将两个 `Point` 对象定义为相等。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-128">For example, two `Point` objects might be defined to be equivalent if pointA.X is equal to pointB.X and pointA.Y is equal to pointB.Y.</span></span> <span data-ttu-id="ac5d9-129">对记录来说，值相等性是指如果记录类型的两个变量类型相匹配，且所有属性和字段值都一致，那么记录类型的两个变量是相等的。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-129">For records, value equality means that two variables of a record type are equal if the types match and all property and field values match.</span></span>  
  
<span data-ttu-id="ac5d9-130">但是，并不要求类型中的所有字段均相等。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-130">However, there is no requirement that equivalence be based on all the fields in a type.</span></span> <span data-ttu-id="ac5d9-131">只需子集相等即可。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-131">It can be based on a subset.</span></span> <span data-ttu-id="ac5d9-132">比较不具所有权的类型时，应确保明确了解相等性对于该类型是如何定义的。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-132">When you compare types that you do not own, you should make sure to understand specifically how equivalence is defined for that type.</span></span> <span data-ttu-id="ac5d9-133">若要详细了解如何在自己的类和结构中定义值相等性，请参阅[如何为类型定义值相等性](./how-to-define-value-equality-for-a-type.md)。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-133">For more information about how to define value equality in your own classes and structs, see [How to define value equality for a type](./how-to-define-value-equality-for-a-type.md).</span></span>
  
### <a name="value-equality-for-floating-point-values"></a><span data-ttu-id="ac5d9-134">浮点值的值相等性</span><span class="sxs-lookup"><span data-stu-id="ac5d9-134">Value equality for floating-point values</span></span>

 <span data-ttu-id="ac5d9-135">由于二进制计算机上的浮点算法不精确，因此浮点值（[double](../../language-reference/builtin-types/floating-point-numeric-types.md) 和 [float](../../language-reference/builtin-types/floating-point-numeric-types.md)）的相等比较会出现问题。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-135">Equality comparisons of floating-point values ([double](../../language-reference/builtin-types/floating-point-numeric-types.md) and [float](../../language-reference/builtin-types/floating-point-numeric-types.md)) are problematic because of the imprecision of floating-point arithmetic on binary computers.</span></span> <span data-ttu-id="ac5d9-136">有关更多信息，请参阅 <xref:System.Double?displayProperty=nameWithType> 主题中的备注部分。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-136">For more information, see the remarks in the topic <xref:System.Double?displayProperty=nameWithType>.</span></span>  
  
## <a name="related-topics"></a><span data-ttu-id="ac5d9-137">相关主题</span><span class="sxs-lookup"><span data-stu-id="ac5d9-137">Related topics</span></span>  
  
|<span data-ttu-id="ac5d9-138">Title</span><span class="sxs-lookup"><span data-stu-id="ac5d9-138">Title</span></span>|<span data-ttu-id="ac5d9-139">描述</span><span class="sxs-lookup"><span data-stu-id="ac5d9-139">Description</span></span>|  
|-----------|-----------------|
|[<span data-ttu-id="ac5d9-140">如何测试引用相等性（标识）</span><span class="sxs-lookup"><span data-stu-id="ac5d9-140">How to test for reference equality (Identity)</span></span>](./how-to-test-for-reference-equality-identity.md)|<span data-ttu-id="ac5d9-141">介绍如何确定两个变量是否具有引用相等性。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-141">Describes how to determine whether two variables have reference equality.</span></span>|
|[<span data-ttu-id="ac5d9-142">如何为类型定义值相等性</span><span class="sxs-lookup"><span data-stu-id="ac5d9-142">How to define value equality for a type</span></span>](./how-to-define-value-equality-for-a-type.md)|<span data-ttu-id="ac5d9-143">介绍如何为类型提供值相等性的自定义定义。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-143">Describes how to provide a custom definition of value equality for a type.</span></span>|
|[<span data-ttu-id="ac5d9-144">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="ac5d9-144">C# Programming Guide</span></span>](../index.md)|<span data-ttu-id="ac5d9-145">提供一些链接，这些链接指向重要 C# 语言功能以及通过 .NET 提供给 C# 的功能的相关详细信息。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-145">Provides links to detailed information about important C# language features and features that are available to C# through .NET.</span></span>|
|[<span data-ttu-id="ac5d9-146">类型</span><span class="sxs-lookup"><span data-stu-id="ac5d9-146">Types</span></span>](../types/index.md)|<span data-ttu-id="ac5d9-147">提供有关 C# 类型系统的信息以及指向附加信息的链接。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-147">Provides information about the C# type system and links to additional information.</span></span>|
|[<span data-ttu-id="ac5d9-148">记录</span><span class="sxs-lookup"><span data-stu-id="ac5d9-148">Records</span></span>](../classes-and-structs/records.md)|<span data-ttu-id="ac5d9-149">提供有关记录类型的信息，默认情况下，记录类型会测试值相等性。</span><span class="sxs-lookup"><span data-stu-id="ac5d9-149">Provides information about record types, which test for value equality by default.</span></span>|

## <a name="see-also"></a><span data-ttu-id="ac5d9-150">另请参阅</span><span class="sxs-lookup"><span data-stu-id="ac5d9-150">See also</span></span>

- [<span data-ttu-id="ac5d9-151">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="ac5d9-151">C# Programming Guide</span></span>](../index.md)
