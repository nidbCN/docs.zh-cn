---
description: 了解 C# 中内置数值转换间的隐式转换和显式转换
title: 内置数值转换 - C# 参考
ms.date: 03/17/2021
helpviewer_keywords:
- implicit numeric conversions [C#]
- explicit numeric conversion [C#]
- numeric conversions [C#], implicit
- numeric conversions [C#], explicit
- conversions [C#], implicit numeric
- conversions [C#], explicit numeric
ms.openlocfilehash: 5ff0289f5365a7d3d334dd0130b3b0efcdf34c60
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759685"
---
# <a name="built-in-numeric-conversions-c-reference"></a><span data-ttu-id="18f8f-103">内置数值转换（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="18f8f-103">Built-in numeric conversions (C# reference)</span></span>

<span data-ttu-id="18f8f-104">C# 提供了一组[整型](integral-numeric-types.md)和[浮点](floating-point-numeric-types.md)数值类型。</span><span class="sxs-lookup"><span data-stu-id="18f8f-104">C# provides a set of [integral](integral-numeric-types.md) and [floating-point](floating-point-numeric-types.md) numeric types.</span></span> <span data-ttu-id="18f8f-105">任何两种数值类型之间都可以进行隐式或显式转换。</span><span class="sxs-lookup"><span data-stu-id="18f8f-105">There exists a conversion between any two numeric types, either implicit or explicit.</span></span> <span data-ttu-id="18f8f-106">必须使用[强制转换表达式](../operators/type-testing-and-cast.md#cast-expression)来执行显式转换。</span><span class="sxs-lookup"><span data-stu-id="18f8f-106">You must use a [cast expression](../operators/type-testing-and-cast.md#cast-expression) to perform an explicit conversion.</span></span>

## <a name="implicit-numeric-conversions"></a><span data-ttu-id="18f8f-107">隐式数值转换</span><span class="sxs-lookup"><span data-stu-id="18f8f-107">Implicit numeric conversions</span></span>

<span data-ttu-id="18f8f-108">下表显示内置数值类型之间的预定义隐式转换：</span><span class="sxs-lookup"><span data-stu-id="18f8f-108">The following table shows the predefined implicit conversions between the built-in numeric types:</span></span>

|<span data-ttu-id="18f8f-109">From</span><span class="sxs-lookup"><span data-stu-id="18f8f-109">From</span></span>|<span data-ttu-id="18f8f-110">到</span><span class="sxs-lookup"><span data-stu-id="18f8f-110">To</span></span>|
|----------|--------|
|[<span data-ttu-id="18f8f-111">sbyte</span><span class="sxs-lookup"><span data-stu-id="18f8f-111">sbyte</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-112">`short`、`int`、`long`、`float`、`double`、`decimal` 或 `nint`。</span><span class="sxs-lookup"><span data-stu-id="18f8f-112">`short`, `int`, `long`, `float`, `double`, `decimal`, or `nint`</span></span>|
|[<span data-ttu-id="18f8f-113">byte</span><span class="sxs-lookup"><span data-stu-id="18f8f-113">byte</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-114">`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-114">`short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, `decimal`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-115">short</span><span class="sxs-lookup"><span data-stu-id="18f8f-115">short</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-116">`int`、`long`、`float`、`double`、`decimal` 或 `nint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-116">`int`, `long`, `float`, `double`, or `decimal`, or `nint`</span></span>|
|[<span data-ttu-id="18f8f-117">ushort</span><span class="sxs-lookup"><span data-stu-id="18f8f-117">ushort</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-118">`int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-118">`int`, `uint`, `long`, `ulong`, `float`, `double`, or `decimal`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-119">int</span><span class="sxs-lookup"><span data-stu-id="18f8f-119">int</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-120">`long`、`float`、`double`、`decimal` 或 `nint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-120">`long`, `float`, `double`, or `decimal`, `nint`</span></span>|
|[<span data-ttu-id="18f8f-121">uint</span><span class="sxs-lookup"><span data-stu-id="18f8f-121">uint</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-122">`long`、`ulong`、`float`、`double`、`decimal` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-122">`long`, `ulong`, `float`, `double`, or `decimal`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-123">long</span><span class="sxs-lookup"><span data-stu-id="18f8f-123">long</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-124">`float`、`double` 或 `decimal`</span><span class="sxs-lookup"><span data-stu-id="18f8f-124">`float`, `double`, or `decimal`</span></span>|
|[<span data-ttu-id="18f8f-125">ulong</span><span class="sxs-lookup"><span data-stu-id="18f8f-125">ulong</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-126">`float`、`double` 或 `decimal`</span><span class="sxs-lookup"><span data-stu-id="18f8f-126">`float`, `double`, or `decimal`</span></span>|
|[<span data-ttu-id="18f8f-127">float</span><span class="sxs-lookup"><span data-stu-id="18f8f-127">float</span></span>](floating-point-numeric-types.md)|`double`|
|[<span data-ttu-id="18f8f-128">nint</span><span class="sxs-lookup"><span data-stu-id="18f8f-128">nint</span></span>](nint-nuint.md)|<span data-ttu-id="18f8f-129">`long`、`float`、`double` 或 `decimal`</span><span class="sxs-lookup"><span data-stu-id="18f8f-129">`long`, `float`, `double`, or `decimal`</span></span>|
|[<span data-ttu-id="18f8f-130">nuint</span><span class="sxs-lookup"><span data-stu-id="18f8f-130">nuint</span></span>](nint-nuint.md)|<span data-ttu-id="18f8f-131">`ulong`、`float`、`double` 或 `decimal`</span><span class="sxs-lookup"><span data-stu-id="18f8f-131">`ulong`, `float`, `double`, or `decimal`</span></span>|

> [!NOTE]
> <span data-ttu-id="18f8f-132">从 `int`、`uint`、`long`、`ulong`、`nint` 或 `nuint` 到 `float` 的隐式转换以及从 `long`、`ulong`、`nint` 或 `nuint` 到 `double` 的隐式转换可能会丢失精准率，但绝不会丢失一个数量级。</span><span class="sxs-lookup"><span data-stu-id="18f8f-132">The implicit conversions from `int`, `uint`, `long`, `ulong`, `nint`, or `nuint` to `float` and from `long`, `ulong`, `nint`, or `nuint` to `double` may cause a loss of precision, but never a loss of an order of magnitude.</span></span> <span data-ttu-id="18f8f-133">其他隐式数值转换不会丢失任何信息。</span><span class="sxs-lookup"><span data-stu-id="18f8f-133">The other implicit numeric conversions never lose any information.</span></span>

<span data-ttu-id="18f8f-134">另请注意</span><span class="sxs-lookup"><span data-stu-id="18f8f-134">Also note that</span></span>

- <span data-ttu-id="18f8f-135">任何[整型数值类型](integral-numeric-types.md)都可以隐式转换为任何[浮点数值类型](floating-point-numeric-types.md)。</span><span class="sxs-lookup"><span data-stu-id="18f8f-135">Any [integral numeric type](integral-numeric-types.md) is implicitly convertible to any [floating-point numeric type](floating-point-numeric-types.md).</span></span>

- <span data-ttu-id="18f8f-136">不存在针对 `byte` 和 `sbyte` 类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="18f8f-136">There are no implicit conversions to the `byte` and `sbyte` types.</span></span> <span data-ttu-id="18f8f-137">不存在从 `double` 和 `decimal` 类型的隐式转换。</span><span class="sxs-lookup"><span data-stu-id="18f8f-137">There are no implicit conversions from the `double` and `decimal` types.</span></span>

- <span data-ttu-id="18f8f-138">`decimal` 类型和 `float` 或 `double` 类型之间不存在隐式转换。</span><span class="sxs-lookup"><span data-stu-id="18f8f-138">There are no implicit conversions between the `decimal` type and the `float` or `double` types.</span></span>

- <span data-ttu-id="18f8f-139">类型 `int` 的常量表达式的值（例如，由整数文本所表示的值）如果在目标类型的范围内，则可隐式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`、`nint` 或 `nuint`：</span><span class="sxs-lookup"><span data-stu-id="18f8f-139">A value of a constant expression of type `int` (for example, a value represented by an integer literal) can be implicitly converted to `sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, `nint`, or `nuint`, if it's within the range of the destination type:</span></span>

  ```csharp
  byte a = 13;
  byte b = 300;  // CS0031: Constant value '300' cannot be converted to a 'byte'
  ```

  <span data-ttu-id="18f8f-140">如前面的示例所示，如果该常量值不在目标类型的范围内，则发生编译器错误 [CS0031](../../misc/cs0031.md)。</span><span class="sxs-lookup"><span data-stu-id="18f8f-140">As the preceding example shows, if the constant value is not within the range of the destination type, a compiler error [CS0031](../../misc/cs0031.md) occurs.</span></span>

## <a name="explicit-numeric-conversions"></a><span data-ttu-id="18f8f-141">显式数值转换</span><span class="sxs-lookup"><span data-stu-id="18f8f-141">Explicit numeric conversions</span></span>

<span data-ttu-id="18f8f-142">下表显示不存在[隐式转换](#implicit-numeric-conversions)的内置数值类型之间的预定义显式转换：</span><span class="sxs-lookup"><span data-stu-id="18f8f-142">The following table shows the predefined explicit conversions between the built-in numeric types for which there is no [implicit conversion](#implicit-numeric-conversions):</span></span>

|<span data-ttu-id="18f8f-143">From</span><span class="sxs-lookup"><span data-stu-id="18f8f-143">From</span></span>|<span data-ttu-id="18f8f-144">到</span><span class="sxs-lookup"><span data-stu-id="18f8f-144">To</span></span>|
|----------|--------|
|[<span data-ttu-id="18f8f-145">sbyte</span><span class="sxs-lookup"><span data-stu-id="18f8f-145">sbyte</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-146">`byte`、`ushort`、`uint`、`ulong` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-146">`byte`, `ushort`, `uint`, or `ulong`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-147">byte</span><span class="sxs-lookup"><span data-stu-id="18f8f-147">byte</span></span>](integral-numeric-types.md)|`sbyte`|
|[<span data-ttu-id="18f8f-148">short</span><span class="sxs-lookup"><span data-stu-id="18f8f-148">short</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-149">`sbyte`、`byte`、`ushort`、`uint`、`ulong` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-149">`sbyte`, `byte`, `ushort`, `uint`, `ulong`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-150">ushort</span><span class="sxs-lookup"><span data-stu-id="18f8f-150">ushort</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-151">`sbyte`、`byte` 或 `short`</span><span class="sxs-lookup"><span data-stu-id="18f8f-151">`sbyte`, `byte`, or `short`</span></span>|
|[<span data-ttu-id="18f8f-152">int</span><span class="sxs-lookup"><span data-stu-id="18f8f-152">int</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-153">`sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong` 或 `nuint`。</span><span class="sxs-lookup"><span data-stu-id="18f8f-153">`sbyte`, `byte`, `short`, `ushort`, `uint`, `ulong`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-154">uint</span><span class="sxs-lookup"><span data-stu-id="18f8f-154">uint</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-155">`sbyte`、`byte`、`short`、`ushort` 或 `int`</span><span class="sxs-lookup"><span data-stu-id="18f8f-155">`sbyte`, `byte`, `short`, `ushort`, or `int`</span></span>|
|[<span data-ttu-id="18f8f-156">long</span><span class="sxs-lookup"><span data-stu-id="18f8f-156">long</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-157">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-157">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-158">ulong</span><span class="sxs-lookup"><span data-stu-id="18f8f-158">ulong</span></span>](integral-numeric-types.md)|<span data-ttu-id="18f8f-159">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-159">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-160">float</span><span class="sxs-lookup"><span data-stu-id="18f8f-160">float</span></span>](floating-point-numeric-types.md)|<span data-ttu-id="18f8f-161">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`decimal`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-161">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `decimal`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-162">double</span><span class="sxs-lookup"><span data-stu-id="18f8f-162">double</span></span>](floating-point-numeric-types.md)|<span data-ttu-id="18f8f-163">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`decimal`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-163">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `decimal`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-164">decimal</span><span class="sxs-lookup"><span data-stu-id="18f8f-164">decimal</span></span>](floating-point-numeric-types.md)|<span data-ttu-id="18f8f-165">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、`nint` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-165">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`, `float`, `double`, `nint`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-166">nint</span><span class="sxs-lookup"><span data-stu-id="18f8f-166">nint</span></span>](nint-nuint.md)|<span data-ttu-id="18f8f-167">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong` 或 `nuint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-167">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `ulong`, or `nuint`</span></span>|
|[<span data-ttu-id="18f8f-168">nuint</span><span class="sxs-lookup"><span data-stu-id="18f8f-168">nuint</span></span>](nint-nuint.md)|<span data-ttu-id="18f8f-169">`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long` 或 `nint`</span><span class="sxs-lookup"><span data-stu-id="18f8f-169">`sbyte`, `byte`, `short`, `ushort`, `int`, `uint`, `long`, or `nint`</span></span>|

> [!NOTE]
> <span data-ttu-id="18f8f-170">显式数值转换可能会导致数据丢失或引发异常，通常为 <xref:System.OverflowException>。</span><span class="sxs-lookup"><span data-stu-id="18f8f-170">An explicit numeric conversion might result in data loss or throw an exception, typically an <xref:System.OverflowException>.</span></span>

<span data-ttu-id="18f8f-171">另请注意</span><span class="sxs-lookup"><span data-stu-id="18f8f-171">Also note that</span></span>

- <span data-ttu-id="18f8f-172">将整数类型的值转换为另一个整数类型时，结果取决于溢出[检查上下文](../keywords/checked-and-unchecked.md)。</span><span class="sxs-lookup"><span data-stu-id="18f8f-172">When you convert a value of an integral type to another integral type, the result depends on the overflow [checking context](../keywords/checked-and-unchecked.md).</span></span> <span data-ttu-id="18f8f-173">在已检查的上下文中，如果源值在目标类型的范围内，则转换成功。</span><span class="sxs-lookup"><span data-stu-id="18f8f-173">In a checked context, the conversion succeeds if the source value is within the range of the destination type.</span></span> <span data-ttu-id="18f8f-174">否则会引发 <xref:System.OverflowException>。</span><span class="sxs-lookup"><span data-stu-id="18f8f-174">Otherwise, an <xref:System.OverflowException> is thrown.</span></span> <span data-ttu-id="18f8f-175">在未检查的上下文中，转换始终成功，并按如下方式进行：</span><span class="sxs-lookup"><span data-stu-id="18f8f-175">In an unchecked context, the conversion always succeeds, and proceeds as follows:</span></span>

  - <span data-ttu-id="18f8f-176">如果源类型大于目标类型，则通过放弃其“额外”最高有效位来截断源值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-176">If the source type is larger than the destination type, then the source value is truncated by discarding its "extra" most significant bits.</span></span> <span data-ttu-id="18f8f-177">结果会被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-177">The result is then treated as a value of the destination type.</span></span>

  - <span data-ttu-id="18f8f-178">如果源类型小于目标类型，则源值是符号扩展或零扩展，以使其与目标类型的大小相同。</span><span class="sxs-lookup"><span data-stu-id="18f8f-178">If the source type is smaller than the destination type, then the source value is either sign-extended or zero-extended so that it's of the same size as the destination type.</span></span> <span data-ttu-id="18f8f-179">如果源类型带符号，则是符号扩展；如果源类型是无符号的，则是零扩展。</span><span class="sxs-lookup"><span data-stu-id="18f8f-179">Sign-extension is used if the source type is signed; zero-extension is used if the source type is unsigned.</span></span> <span data-ttu-id="18f8f-180">结果会被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-180">The result is then treated as a value of the destination type.</span></span>

  - <span data-ttu-id="18f8f-181">如果源类型与目标类型的大小相同，则源值将被视为目标类型的值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-181">If the source type is the same size as the destination type, then the source value is treated as a value of the destination type.</span></span>

- <span data-ttu-id="18f8f-182">将 `decimal` 值转换为整型类型时，此值会向零舍入到最接近的整数值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-182">When you convert a `decimal` value to an integral type, this value is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="18f8f-183">如果生成的整数值处于目标类型的范围之外，则会引发 <xref:System.OverflowException>。</span><span class="sxs-lookup"><span data-stu-id="18f8f-183">If the resulting integral value is outside the range of the destination type, an <xref:System.OverflowException> is thrown.</span></span>

- <span data-ttu-id="18f8f-184">将 `double` 或 `float` 值转换为整型类型时，此值会向零舍入到最接近的整数值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-184">When you convert a `double` or `float` value to an integral type, this value is rounded towards zero to the nearest integral value.</span></span> <span data-ttu-id="18f8f-185">如果生成的整数值处于目标类型范围之外，则结果会取决于溢出[上下文](../keywords/checked-and-unchecked.md)。</span><span class="sxs-lookup"><span data-stu-id="18f8f-185">If the resulting integral value is outside the range of the destination type, the result depends on the overflow [checking context](../keywords/checked-and-unchecked.md).</span></span> <span data-ttu-id="18f8f-186">在已检查的上下文中，引发 <xref:System.OverflowException>；而在未检查的上下文中，结果是目标类型的未指定值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-186">In a checked context, an <xref:System.OverflowException> is thrown, while in an unchecked context, the result is an unspecified value of the destination type.</span></span>

- <span data-ttu-id="18f8f-187">将 `double` 转换为 `float` 时，`double` 值舍入为最接近的 `float` 值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-187">When you convert `double` to `float`, the `double` value is rounded to the nearest `float` value.</span></span> <span data-ttu-id="18f8f-188">如果 `double` 值太小或太大，无法匹配 `float` 类型，结果将为零或无穷大。</span><span class="sxs-lookup"><span data-stu-id="18f8f-188">If the `double` value is too small or too large to fit into the `float` type, the result is zero or infinity.</span></span>

- <span data-ttu-id="18f8f-189">将 `float` 或 `double` 转换为 `decimal` 时，源值转换为 `decimal` 表示形式，并并五入到第 28 位小数后最接近的数（如果需要）。</span><span class="sxs-lookup"><span data-stu-id="18f8f-189">When you convert `float` or `double` to `decimal`, the source value is converted to `decimal` representation and rounded to the nearest number after the 28th decimal place if required.</span></span> <span data-ttu-id="18f8f-190">根据源值的值，可能出现以下结果之一：</span><span class="sxs-lookup"><span data-stu-id="18f8f-190">Depending on the value of the source value, one of the following results may occur:</span></span>

  - <span data-ttu-id="18f8f-191">如果源值太小，无法表示为 `decimal`，结果则为零。</span><span class="sxs-lookup"><span data-stu-id="18f8f-191">If the source value is too small to be represented as a `decimal`, the result becomes zero.</span></span>

  - <span data-ttu-id="18f8f-192">如果源值为 NaN（非数值）、无穷大或太大而无法表示为 `decimal`，则引发 <xref:System.OverflowException>。</span><span class="sxs-lookup"><span data-stu-id="18f8f-192">If the source value is NaN (not a number), infinity, or too large to be represented as a `decimal`, an <xref:System.OverflowException> is thrown.</span></span>

- <span data-ttu-id="18f8f-193">将 `decimal` 转换为 `float` 或 `double` 时，源值分别舍入为最接近的 `float` 或 `double` 值。</span><span class="sxs-lookup"><span data-stu-id="18f8f-193">When you convert `decimal` to `float` or `double`, the source value is rounded to the nearest `float` or `double` value, respectively.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="18f8f-194">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="18f8f-194">C# language specification</span></span>

<span data-ttu-id="18f8f-195">有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：</span><span class="sxs-lookup"><span data-stu-id="18f8f-195">For more information, see the following sections of the [C# language specification](~/_csharplang/spec/introduction.md):</span></span>

- [<span data-ttu-id="18f8f-196">隐式数值转换</span><span class="sxs-lookup"><span data-stu-id="18f8f-196">Implicit numeric conversions</span></span>](~/_csharplang/spec/conversions.md#implicit-numeric-conversions)
- [<span data-ttu-id="18f8f-197">显式数值转换</span><span class="sxs-lookup"><span data-stu-id="18f8f-197">Explicit numeric conversions</span></span>](~/_csharplang/spec/conversions.md#explicit-numeric-conversions)

## <a name="see-also"></a><span data-ttu-id="18f8f-198">请参阅</span><span class="sxs-lookup"><span data-stu-id="18f8f-198">See also</span></span>

- [<span data-ttu-id="18f8f-199">C# 参考</span><span class="sxs-lookup"><span data-stu-id="18f8f-199">C# reference</span></span>](../index.md)
- [<span data-ttu-id="18f8f-200">强制转换和类型转换</span><span class="sxs-lookup"><span data-stu-id="18f8f-200">Casting and type conversions</span></span>](../../programming-guide/types/casting-and-type-conversions.md)
