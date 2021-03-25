---
description: '#define - C# 参考'
title: '#define - C# 参考'
ms.date: 06/30/2018
f1_keywords:
- '#define'
helpviewer_keywords:
- '#define directive [C#]'
ms.assetid: 23638b8f-779c-450e-b600-d55682de7d01
ms.openlocfilehash: dddc60b99a55762ae26a470fcd6b6a0e9b98bcf8
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103480352"
---
# <a name="define-c-reference"></a><span data-ttu-id="4dc7e-103">#define（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="4dc7e-103">#define (C# Reference)</span></span>

<span data-ttu-id="4dc7e-104">使用 `#define` 来定义符号。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-104">You use `#define` to define a symbol.</span></span> <span data-ttu-id="4dc7e-105">将符号用作传递给 [#if](./preprocessor-if.md) 指令的表达式时，该表达式的计算结果为 `true`，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="4dc7e-105">When you use the symbol as the expression that's passed to the [#if](./preprocessor-if.md) directive, the expression will evaluate to `true`, as the following example shows:</span></span>  

 ```csharp
 #define DEBUG
 ```
  
## <a name="remarks"></a><span data-ttu-id="4dc7e-106">备注</span><span class="sxs-lookup"><span data-stu-id="4dc7e-106">Remarks</span></span>  
  
> [!NOTE]
> <span data-ttu-id="4dc7e-107">`#define` 指令不能用于声明常量值，这与 C 和 C++ 中的通常做法一样。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-107">The `#define` directive cannot be used to declare constant values as is typically done in C and C++.</span></span> <span data-ttu-id="4dc7e-108">C# 中的常量最好定义为类或结构的静态成员。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-108">Constants in C# are best defined as static members of a class or struct.</span></span> <span data-ttu-id="4dc7e-109">如果具有多个此类常量，请考虑创建一个单独的“常量”类来容纳它们。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-109">If you have several such constants, consider creating a separate "Constants" class to hold them.</span></span>  
  
 <span data-ttu-id="4dc7e-110">符号可用于指定编译的条件。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-110">Symbols can be used to specify conditions for compilation.</span></span> <span data-ttu-id="4dc7e-111">可通过 [#if](./preprocessor-if.md) 或 [#elif](./preprocessor-elif.md) 测试符号。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-111">You can test for the symbol with either [#if](./preprocessor-if.md) or [#elif](./preprocessor-elif.md).</span></span> <span data-ttu-id="4dc7e-112">还可以使用 <xref:System.Diagnostics.ConditionalAttribute> 来执行条件编译。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-112">You can also use the <xref:System.Diagnostics.ConditionalAttribute> to perform conditional compilation.</span></span>  
  
 <span data-ttu-id="4dc7e-113">可以定义一个符号，但不能向符号分配值。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-113">You can define a symbol, but you cannot assign a value to a symbol.</span></span> <span data-ttu-id="4dc7e-114">文件中必须先出现 `#define` 指令，才能使用并非同时也是预处理器指令的任何指示。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-114">The `#define` directive must appear in the file before you use any instructions that aren't also preprocessor directives.</span></span>  
  
 <span data-ttu-id="4dc7e-115">还可以通过 [DefineConstants](../compiler-options/language.md#defineconstants) 编译器选项来定义符号。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-115">You can also define a symbol with the [**DefineConstants**](../compiler-options/language.md#defineconstants) compiler option.</span></span> <span data-ttu-id="4dc7e-116">可以通过 [#undef](./preprocessor-undef.md) 取消定义符号。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-116">You can undefine a symbol with [#undef](./preprocessor-undef.md).</span></span>  
  
 <span data-ttu-id="4dc7e-117">使用 `-define` 或 `#define` 定义的符号与具有相同名称的变量不冲突。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-117">A symbol that you define with `-define` or with `#define` does not conflict with a variable of the same name.</span></span> <span data-ttu-id="4dc7e-118">也就是说，变量名称不应传递给预处理器指令，且符号仅能由预处理器指令评估。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-118">That is, a variable name should not be passed to a preprocessor directive and a symbol can only be evaluated by a preprocessor directive.</span></span>  
  
 <span data-ttu-id="4dc7e-119">使用 `#define` 创建的符号的作用域是在其中定义该符号的文件。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-119">The scope of a symbol that was created by using `#define` is the file in which the symbol was defined.</span></span>  
  
 <span data-ttu-id="4dc7e-120">如以下示例所示，必须将 `#define` 指令放在文件顶部。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-120">As the following example shows, you must put `#define` directives at the top of the file.</span></span>  
  
```csharp  
#define DEBUG  
//#define TRACE  
#undef TRACE  
  
using System;  
  
public class TestDefine  
{  
    static void Main()  
    {  
#if (DEBUG)  
        Console.WriteLine("Debugging is enabled.");  
#endif  
  
#if (TRACE)  
     Console.WriteLine("Tracing is enabled.");  
#endif  
    }  
}  
// Output:  
// Debugging is enabled.  
```  
  
 <span data-ttu-id="4dc7e-121">有关如何取消对符号进行定义的示例，请参阅 [#undef](./preprocessor-undef.md)。</span><span class="sxs-lookup"><span data-stu-id="4dc7e-121">For an example of how to undefine a symbol, see [#undef](./preprocessor-undef.md).</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="4dc7e-122">请参阅</span><span class="sxs-lookup"><span data-stu-id="4dc7e-122">See also</span></span>

- [<span data-ttu-id="4dc7e-123">C# 参考</span><span class="sxs-lookup"><span data-stu-id="4dc7e-123">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="4dc7e-124">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="4dc7e-124">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="4dc7e-125">C# 预处理器指令</span><span class="sxs-lookup"><span data-stu-id="4dc7e-125">C# Preprocessor Directives</span></span>](./index.md)
- [<span data-ttu-id="4dc7e-126">const</span><span class="sxs-lookup"><span data-stu-id="4dc7e-126">const</span></span>](../keywords/const.md)
- [<span data-ttu-id="4dc7e-127">如何：使用跟踪和调试执行有条件编译</span><span class="sxs-lookup"><span data-stu-id="4dc7e-127">How to: Compile Conditionally with Trace and Debug</span></span>](../../../framework/debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md)
- [<span data-ttu-id="4dc7e-128">#undef</span><span class="sxs-lookup"><span data-stu-id="4dc7e-128">#undef</span></span>](./preprocessor-undef.md)
- [<span data-ttu-id="4dc7e-129">#if</span><span class="sxs-lookup"><span data-stu-id="4dc7e-129">#if</span></span>](./preprocessor-if.md)
