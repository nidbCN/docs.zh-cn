---
title: 指针类型 - C# 编程指南
description: 了解指针类型。 参阅不同指针的示例、代码示例，并查看其他可用资源。
ms.date: 04/20/2018
helpviewer_keywords:
- unsafe code [C#], pointers
- pointers [C#]
ms.openlocfilehash: 0e384084ef1fbb9139c11880ffa8a79bad23b32e
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103480578"
---
# <a name="pointer-types-c-programming-guide"></a><span data-ttu-id="52c9b-104">指针类型（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="52c9b-104">Pointer types (C# Programming Guide)</span></span>

<span data-ttu-id="52c9b-105">在不安全的上下文中，类型可以是指针类型、值类型或引用类型。</span><span class="sxs-lookup"><span data-stu-id="52c9b-105">In an unsafe context, a type may be a pointer type, a value type, or a reference type.</span></span> <span data-ttu-id="52c9b-106">指针类型声明采用下列形式之一：</span><span class="sxs-lookup"><span data-stu-id="52c9b-106">A pointer type declaration takes one of the following forms:</span></span>

``` csharp
type* identifier;
void* identifier; //allowed but not recommended
```

<span data-ttu-id="52c9b-107">在指针类型中的 `*` 之前指定的类型被称为“referent 类型”  。</span><span class="sxs-lookup"><span data-stu-id="52c9b-107">The type specified before the `*` in a pointer type is called the **referent type**.</span></span> <span data-ttu-id="52c9b-108">只有[非托管类型](../../language-reference/builtin-types/unmanaged-types.md)可为引用类型。</span><span class="sxs-lookup"><span data-stu-id="52c9b-108">Only an [unmanaged type](../../language-reference/builtin-types/unmanaged-types.md) can be a referent type.</span></span>

<span data-ttu-id="52c9b-109">指针类型不从[对象](../../language-reference/builtin-types/reference-types.md)继承，并且指针类型与 `object` 之间不存在转换。</span><span class="sxs-lookup"><span data-stu-id="52c9b-109">Pointer types do not inherit from [object](../../language-reference/builtin-types/reference-types.md) and no conversions exist between pointer types and `object`.</span></span> <span data-ttu-id="52c9b-110">此外，装箱和取消装箱不支持指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-110">Also, boxing and unboxing do not support pointers.</span></span> <span data-ttu-id="52c9b-111">但是，你可在不同的指针类型之间以及指针类型和整型之间进行转换。</span><span class="sxs-lookup"><span data-stu-id="52c9b-111">However, you can convert between different pointer types and between pointer types and integral types.</span></span>

<span data-ttu-id="52c9b-112">在同一个声明中声明多个指针时，星号 (\*) 仅与基础类型一起写入；而不是用作每个指针名称的前缀。</span><span class="sxs-lookup"><span data-stu-id="52c9b-112">When you declare multiple pointers in the same declaration, the asterisk (\*) is written together with the underlying type only; it is not used as a prefix to each pointer name.</span></span> <span data-ttu-id="52c9b-113">例如：</span><span class="sxs-lookup"><span data-stu-id="52c9b-113">For example:</span></span>

```csharp
int* p1, p2, p3;   // Ok
int *p1, *p2, *p3;   // Invalid in C#
```

<span data-ttu-id="52c9b-114">指针不能指向引用或包含引用的[结构](../../language-reference/builtin-types/struct.md)，因为无法对对象引用进行垃圾回收，即使有指针指向它也是如此。</span><span class="sxs-lookup"><span data-stu-id="52c9b-114">A pointer cannot point to a reference or to a [struct](../../language-reference/builtin-types/struct.md) that contains references, because an object reference can be garbage collected even if a pointer is pointing to it.</span></span> <span data-ttu-id="52c9b-115">垃圾回收器并不跟踪是否有任何类型的指针指向对象。</span><span class="sxs-lookup"><span data-stu-id="52c9b-115">The garbage collector does not keep track of whether an object is being pointed to by any pointer types.</span></span>

<span data-ttu-id="52c9b-116">`myType*` 类型的指针变量的值为 `myType` 类型的变量的地址。</span><span class="sxs-lookup"><span data-stu-id="52c9b-116">The value of the pointer variable of type `myType*` is the address of a variable of type `myType`.</span></span> <span data-ttu-id="52c9b-117">下面是指针类型声明的示例：</span><span class="sxs-lookup"><span data-stu-id="52c9b-117">The following are examples of pointer type declarations:</span></span>

|<span data-ttu-id="52c9b-118">示例</span><span class="sxs-lookup"><span data-stu-id="52c9b-118">Example</span></span>|<span data-ttu-id="52c9b-119">描述</span><span class="sxs-lookup"><span data-stu-id="52c9b-119">Description</span></span>|
|-------------|-----------------|
|`int* p`|<span data-ttu-id="52c9b-120">`p` 是指向整数的指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-120">`p` is a pointer to an integer.</span></span>|
|`int** p`|<span data-ttu-id="52c9b-121">`p` 是指向整数的指针的指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-121">`p` is a pointer to a pointer to an integer.</span></span>|
|`int*[] p`|<span data-ttu-id="52c9b-122">`p` 是指向整数的指针的一维数组。</span><span class="sxs-lookup"><span data-stu-id="52c9b-122">`p` is a single-dimensional array of pointers to integers.</span></span>|
|`char* p`|<span data-ttu-id="52c9b-123">`p` 是指向字符的指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-123">`p` is a pointer to a char.</span></span>|
|`void* p`|<span data-ttu-id="52c9b-124">`p` 是指向未知类型的指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-124">`p` is a pointer to an unknown type.</span></span>|

<span data-ttu-id="52c9b-125">指针间接寻址运算符 \* 可用于访问位于指针变量所指向的位置的内容。</span><span class="sxs-lookup"><span data-stu-id="52c9b-125">The pointer indirection operator \* can be used to access the contents at the location pointed to by the pointer variable.</span></span> <span data-ttu-id="52c9b-126">例如，请考虑以下声明：</span><span class="sxs-lookup"><span data-stu-id="52c9b-126">For example, consider the following declaration:</span></span>

```csharp
int* myVariable;
```

<span data-ttu-id="52c9b-127">表达式 `*myVariable` 表示在 `int` 中包含的地址处找到的 `myVariable` 变量。</span><span class="sxs-lookup"><span data-stu-id="52c9b-127">The expression `*myVariable` denotes the `int` variable found at the address contained in `myVariable`.</span></span>

<span data-ttu-id="52c9b-128">[fixed 语句](../../language-reference/keywords/fixed-statement.md)和[指针转换](./pointer-conversions.md)主题中有几个指针示例。</span><span class="sxs-lookup"><span data-stu-id="52c9b-128">There are several examples of pointers in the topics [fixed Statement](../../language-reference/keywords/fixed-statement.md) and [Pointer Conversions](./pointer-conversions.md).</span></span> <span data-ttu-id="52c9b-129">下面的示例使用 `unsafe` 关键字和 `fixed` 语句，并显示如何递增内部指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-129">The following example uses the `unsafe` keyword and the `fixed` statement, and shows how to increment an interior pointer.</span></span>  <span data-ttu-id="52c9b-130">你可将此代码粘贴到控制台应用程序的 Main 函数中来运行它。</span><span class="sxs-lookup"><span data-stu-id="52c9b-130">You can paste this code into the Main function of a console application to run it.</span></span> <span data-ttu-id="52c9b-131">这些示例必须使用 [AllowUnsafeBlocks](../../language-reference/compiler-options/language.md#allowunsafeblocks) 编译器选项集进行编译。</span><span class="sxs-lookup"><span data-stu-id="52c9b-131">These examples must be compiled with the [**AllowUnsafeBlocks**](../../language-reference/compiler-options/language.md#allowunsafeblocks) compiler option set.</span></span>

[!code-csharp[Using pointer types](snippets/FixedKeywordExamples.cs#5)]

<span data-ttu-id="52c9b-132">你无法对 `void*` 类型的指针应用间接寻址运算符。</span><span class="sxs-lookup"><span data-stu-id="52c9b-132">You cannot apply the indirection operator to a pointer of type `void*`.</span></span> <span data-ttu-id="52c9b-133">但是，你可以使用强制转换将 void 指针转换为任何其他指针类型，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="52c9b-133">However, you can use a cast to convert a void pointer to any other pointer type, and vice versa.</span></span>

<span data-ttu-id="52c9b-134">指针可以为 `null`。</span><span class="sxs-lookup"><span data-stu-id="52c9b-134">A pointer can be `null`.</span></span> <span data-ttu-id="52c9b-135">将间接寻址运算符应用于 null 指针将导致由实现定义的行为。</span><span class="sxs-lookup"><span data-stu-id="52c9b-135">Applying the indirection operator to a null pointer causes an implementation-defined behavior.</span></span>

<span data-ttu-id="52c9b-136">在方法之间传递指针会导致未定义的行为。</span><span class="sxs-lookup"><span data-stu-id="52c9b-136">Passing pointers between methods can cause undefined behavior.</span></span> <span data-ttu-id="52c9b-137">考虑这种方法，该方法通过 `in`、`out` 或 `ref` 参数或作为函数结果返回一个指向局部变量的指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-137">Consider a method that returns a pointer to a local variable through an `in`, `out`, or `ref` parameter or as the function result.</span></span> <span data-ttu-id="52c9b-138">如果已在固定块中设置指针，则它指向的变量不再是固定的。</span><span class="sxs-lookup"><span data-stu-id="52c9b-138">If the pointer was set in a fixed block, the variable to which it points may no longer be fixed.</span></span>

<span data-ttu-id="52c9b-139">下表列出了可在不安全的上下文中对指针执行的运算符和语句：</span><span class="sxs-lookup"><span data-stu-id="52c9b-139">The following table lists the operators and statements that can operate on pointers in an unsafe context:</span></span>

|<span data-ttu-id="52c9b-140">运算符/语句</span><span class="sxs-lookup"><span data-stu-id="52c9b-140">Operator/Statement</span></span>|<span data-ttu-id="52c9b-141">使用</span><span class="sxs-lookup"><span data-stu-id="52c9b-141">Use</span></span>|
|-------------------------|---------|
|`*`|<span data-ttu-id="52c9b-142">执行指针间接寻址。</span><span class="sxs-lookup"><span data-stu-id="52c9b-142">Performs pointer indirection.</span></span>|
|`->`|<span data-ttu-id="52c9b-143">通过指针访问结构的成员。</span><span class="sxs-lookup"><span data-stu-id="52c9b-143">Accesses a member of a struct through a pointer.</span></span>|
|`[]`|<span data-ttu-id="52c9b-144">为指针建立索引。</span><span class="sxs-lookup"><span data-stu-id="52c9b-144">Indexes a pointer.</span></span>|
|`&`|<span data-ttu-id="52c9b-145">获取变量的地址。</span><span class="sxs-lookup"><span data-stu-id="52c9b-145">Obtains the address of a variable.</span></span>|
|<span data-ttu-id="52c9b-146">`++` 和 `--`</span><span class="sxs-lookup"><span data-stu-id="52c9b-146">`++` and `--`</span></span>|<span data-ttu-id="52c9b-147">递增和递减指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-147">Increments and decrements pointers.</span></span>|
|<span data-ttu-id="52c9b-148">`+` 和 `-`</span><span class="sxs-lookup"><span data-stu-id="52c9b-148">`+` and `-`</span></span>|<span data-ttu-id="52c9b-149">执行指针算法。</span><span class="sxs-lookup"><span data-stu-id="52c9b-149">Performs pointer arithmetic.</span></span>|
|<span data-ttu-id="52c9b-150">`==`、`!=`、`<`、`>`、`<=` 和 `>=`</span><span class="sxs-lookup"><span data-stu-id="52c9b-150">`==`, `!=`, `<`, `>`, `<=`, and `>=`</span></span>|<span data-ttu-id="52c9b-151">比较指针。</span><span class="sxs-lookup"><span data-stu-id="52c9b-151">Compares pointers.</span></span>|
|[`stackalloc`](../../language-reference/operators/stackalloc.md)|<span data-ttu-id="52c9b-152">在堆栈上分配内存。</span><span class="sxs-lookup"><span data-stu-id="52c9b-152">Allocates memory on the stack.</span></span>|
|[<span data-ttu-id="52c9b-153">`fixed` 语句</span><span class="sxs-lookup"><span data-stu-id="52c9b-153">`fixed` statement</span></span>](../../language-reference/keywords/fixed-statement.md)|<span data-ttu-id="52c9b-154">临时固定变量以便找到其地址。</span><span class="sxs-lookup"><span data-stu-id="52c9b-154">Temporarily fixes a variable so that its address may be found.</span></span>|

<span data-ttu-id="52c9b-155">要详细了解与指针相关的运算符，请参阅[与指针相关的运算符](../../language-reference/operators/pointer-related-operators.md)。</span><span class="sxs-lookup"><span data-stu-id="52c9b-155">For more information about pointer related operators, see [Pointer related operators](../../language-reference/operators/pointer-related-operators.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="52c9b-156">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="52c9b-156">C# language specification</span></span>

<span data-ttu-id="52c9b-157">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[指针类型](~/_csharplang/spec/unsafe-code.md#pointer-types)部分。</span><span class="sxs-lookup"><span data-stu-id="52c9b-157">For more information, see the [Pointer types](~/_csharplang/spec/unsafe-code.md#pointer-types) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="52c9b-158">请参阅</span><span class="sxs-lookup"><span data-stu-id="52c9b-158">See also</span></span>

- [<span data-ttu-id="52c9b-159">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="52c9b-159">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="52c9b-160">不安全代码和指针</span><span class="sxs-lookup"><span data-stu-id="52c9b-160">Unsafe Code and Pointers</span></span>](index.md)
- [<span data-ttu-id="52c9b-161">指针转换</span><span class="sxs-lookup"><span data-stu-id="52c9b-161">Pointer Conversions</span></span>](pointer-conversions.md)
- [<span data-ttu-id="52c9b-162">引用类型</span><span class="sxs-lookup"><span data-stu-id="52c9b-162">Reference types</span></span>](../../language-reference/keywords/reference-types.md)
- [<span data-ttu-id="52c9b-163">值类型</span><span class="sxs-lookup"><span data-stu-id="52c9b-163">Value types</span></span>](../../language-reference/builtin-types/value-types.md)
- [<span data-ttu-id="52c9b-164">unsafe</span><span class="sxs-lookup"><span data-stu-id="52c9b-164">unsafe</span></span>](../../language-reference/keywords/unsafe.md)
