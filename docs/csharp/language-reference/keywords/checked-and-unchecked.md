---
description: Checked 和 Unchecked - C# 参考
title: Checked 和 Unchecked - C# 参考
ms.date: 05/15/2018
helpviewer_keywords:
- operators [C#], checked and unchecked
- exceptions [C#], overflow checking
- checked statement [C#]
- overflow checking [C#]
- unchecked statement [C#]
- statements [C#], checked and unchecked
ms.assetid: a84bc877-2c7f-4396-8735-1ce97c42f35e
ms.openlocfilehash: 0121090265881bfa8287e2f9e83ad4b886bf17c1
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477482"
---
# <a name="checked-and-unchecked-c-reference"></a><span data-ttu-id="e3853-103">Checked 和 Unchecked（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="e3853-103">Checked and Unchecked (C# Reference)</span></span>

<span data-ttu-id="e3853-104">C# 语句既可以在已检查的上下文中执行，也可以在未检查的上下文中执行。</span><span class="sxs-lookup"><span data-stu-id="e3853-104">C# statements can execute in either checked or unchecked context.</span></span> <span data-ttu-id="e3853-105">在已检查的上下文中，算法溢出引发异常。</span><span class="sxs-lookup"><span data-stu-id="e3853-105">In a checked context, arithmetic overflow raises an exception.</span></span> <span data-ttu-id="e3853-106">在未选中的上下文中忽略算术溢出并将结果截断，方法是：丢弃任何不适应目标类型的高序位。</span><span class="sxs-lookup"><span data-stu-id="e3853-106">In an unchecked context, arithmetic overflow is ignored and the result is truncated by discarding any high-order bits that don't fit in the destination type.</span></span>  
  
- <span data-ttu-id="e3853-107">[checked](checked.md) 指定已检查的上下文。</span><span class="sxs-lookup"><span data-stu-id="e3853-107">[checked](checked.md) Specify checked context.</span></span>  
  
- <span data-ttu-id="e3853-108">[unchecked](unchecked.md) 指定未检查的上下文。</span><span class="sxs-lookup"><span data-stu-id="e3853-108">[unchecked](unchecked.md) Specify unchecked context.</span></span>  
  
 <span data-ttu-id="e3853-109">下列操作受溢出检查的影响：</span><span class="sxs-lookup"><span data-stu-id="e3853-109">The following operations are affected by the overflow checking:</span></span>  
  
- <span data-ttu-id="e3853-110">表达式在整型上使用下列预定义运算符：</span><span class="sxs-lookup"><span data-stu-id="e3853-110">Expressions using the following predefined operators on integral types:</span></span>  
  
     <span data-ttu-id="e3853-111">`++`，`--`，一元 `-`，`+`，`-`，`*`，`/`</span><span class="sxs-lookup"><span data-stu-id="e3853-111">`++`, `--`, unary `-`, `+`, `-`, `*`, `/`</span></span>  
  
- <span data-ttu-id="e3853-112">整型类型之间或从 `float` 或 `double` 到整型类型的显式数字转换。</span><span class="sxs-lookup"><span data-stu-id="e3853-112">Explicit numeric conversions between integral types, or from `float` or `double` to an integral type.</span></span>  
  
 <span data-ttu-id="e3853-113">如果既未指定 `checked`，也未指定 `unchecked`，则非常量表达式（在运行时计算的表达式）的默认上下文将由 [CheckForOverflowUnderflow](../compiler-options/language.md#checkforoverflowunderflow) 编译器选项的值定义。</span><span class="sxs-lookup"><span data-stu-id="e3853-113">If neither `checked` nor `unchecked` is specified, the default context for non-constant expressions (expressions that are evaluated at run time) is defined by the value of the [**CheckForOverflowUnderflow**](../compiler-options/language.md#checkforoverflowunderflow) compiler option.</span></span> <span data-ttu-id="e3853-114">默认情况下，该选项的值未设置，且算术运算在未选中的上下文中执行。</span><span class="sxs-lookup"><span data-stu-id="e3853-114">By default the value of that option is unset and arithmetic operations are executed in an unchecked context.</span></span>

 <span data-ttu-id="e3853-115">对于常量表达式（可在编译时完全计算的表达式），将始终选中默认上下文。</span><span class="sxs-lookup"><span data-stu-id="e3853-115">For constant expressions (expressions that can be fully evaluated at compile time), the default context is always checked.</span></span> <span data-ttu-id="e3853-116">除非在未选中的上下文中显式放置常量表达式，否则在编译时间计算表达式过程中出现的溢出将导致编译时错误。</span><span class="sxs-lookup"><span data-stu-id="e3853-116">Unless a constant expression is explicitly placed in an unchecked context, overflows that occur during the compile-time evaluation of the expression cause compile-time errors.</span></span>
  
## <a name="see-also"></a><span data-ttu-id="e3853-117">请参阅</span><span class="sxs-lookup"><span data-stu-id="e3853-117">See also</span></span>

- [<span data-ttu-id="e3853-118">C# 参考</span><span class="sxs-lookup"><span data-stu-id="e3853-118">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="e3853-119">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="e3853-119">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="e3853-120">C# 关键字</span><span class="sxs-lookup"><span data-stu-id="e3853-120">C# Keywords</span></span>](index.md)
- [<span data-ttu-id="e3853-121">语句关键字</span><span class="sxs-lookup"><span data-stu-id="e3853-121">Statement Keywords</span></span>](statement-keywords.md)
