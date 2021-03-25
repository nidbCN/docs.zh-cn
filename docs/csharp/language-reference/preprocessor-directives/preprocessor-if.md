---
description: '#if 预处理器指令 - C# 参考'
title: '#if 预处理器指令 - C# 参考'
ms.date: 10/27/2019
f1_keywords:
- '#if'
helpviewer_keywords:
- '#if directive [C#]'
ms.assetid: 48cabbff-ca82-491f-a56a-eeccd528c7c2
ms.openlocfilehash: a3f72719ba4ce722aef33bbd5de338d3d06b2aa0
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103480370"
---
# <a name="if-c-reference"></a><span data-ttu-id="ddb90-103">#if（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="ddb90-103">#if (C# reference)</span></span>

<span data-ttu-id="ddb90-104">如果 C# 编译器遇到 `#if` 指令，最终是 [#endif](preprocessor-endif.md) 指令，则仅当定义指定的符号时，它才编译这些指令之间的代码。</span><span class="sxs-lookup"><span data-stu-id="ddb90-104">When the C# compiler encounters an `#if` directive, followed eventually by an [#endif](preprocessor-endif.md) directive, it compiles the code between the directives only if the specified symbol is defined.</span></span> <span data-ttu-id="ddb90-105">与 C 和 C++ 不同，你不能为符号分配数字值。</span><span class="sxs-lookup"><span data-stu-id="ddb90-105">Unlike C and C++, you cannot assign a numeric value to a symbol.</span></span> <span data-ttu-id="ddb90-106">C# 中的 `#if` 语句是布尔值，且仅测试是否已定义该符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-106">The `#if` statement in C# is Boolean and only tests whether the symbol has been defined or not.</span></span> <span data-ttu-id="ddb90-107">例如：</span><span class="sxs-lookup"><span data-stu-id="ddb90-107">For example:</span></span>

```csharp
#if DEBUG
    Console.WriteLine("Debug version");
#endif
```

<span data-ttu-id="ddb90-108">仅可使用运算符 [==](../operators/equality-operators.md#equality-operator-)（相等）和 [!=](../operators/equality-operators.md#inequality-operator-)（不相等）测试[布尔](../builtin-types/bool.md)值 `true` 或 `false`。</span><span class="sxs-lookup"><span data-stu-id="ddb90-108">You can use the operators [==](../operators/equality-operators.md#equality-operator-) (equality) and [!=](../operators/equality-operators.md#inequality-operator-) (inequality) only to test for the [bool](../builtin-types/bool.md) values `true` or `false`.</span></span> <span data-ttu-id="ddb90-109">`true` 表示定义该符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-109">`true` means the symbol is defined.</span></span> <span data-ttu-id="ddb90-110">语句 `#if DEBUG` 具有与 `#if (DEBUG == true)` 相同的含义。</span><span class="sxs-lookup"><span data-stu-id="ddb90-110">The statement `#if DEBUG` has the same meaning as `#if (DEBUG == true)`.</span></span> <span data-ttu-id="ddb90-111">可以使用 [&& (and)](../operators/boolean-logical-operators.md#conditional-logical-and-operator-)、[&#124;&#124; (or)](../operators/boolean-logical-operators.md#conditional-logical-or-operator-)和 [! (not)](../operators/boolean-logical-operators.md#logical-negation-operator-) 运算符来计算是否已定义多个符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-111">You can use the [&& (and)](../operators/boolean-logical-operators.md#conditional-logical-and-operator-), [&#124;&#124; (or)](../operators/boolean-logical-operators.md#conditional-logical-or-operator-), and [! (not)](../operators/boolean-logical-operators.md#logical-negation-operator-) operators to evaluate whether multiple symbols have been defined.</span></span> <span data-ttu-id="ddb90-112">还可以用括号对符号和运算符进行分组。</span><span class="sxs-lookup"><span data-stu-id="ddb90-112">You can also group symbols and operators with parentheses.</span></span>

## <a name="remarks"></a><span data-ttu-id="ddb90-113">备注</span><span class="sxs-lookup"><span data-stu-id="ddb90-113">Remarks</span></span>

<span data-ttu-id="ddb90-114">`#if` 以及 [#else](preprocessor-else.md)、[#elif](preprocessor-elif.md)、[#endif](preprocessor-endif.md)、[#define](preprocessor-define.md) 和 [#undef](preprocessor-undef.md) 指令，允许基于是否存在一个或多个符号包括或排除代码。</span><span class="sxs-lookup"><span data-stu-id="ddb90-114">`#if`, along with the [#else](preprocessor-else.md), [#elif](preprocessor-elif.md), [#endif](preprocessor-endif.md), [#define](preprocessor-define.md), and [#undef](preprocessor-undef.md) directives, lets you include or exclude code based on the existence of one or more symbols.</span></span> <span data-ttu-id="ddb90-115">这在编译调试版本的代码或编译特定配置的代码时会很有用。</span><span class="sxs-lookup"><span data-stu-id="ddb90-115">This can be useful when compiling code for a debug build or when compiling for a specific configuration.</span></span>

<span data-ttu-id="ddb90-116">以 `#if` 指令开头的条件指令必须以 `#endif` 指令显式终止。</span><span class="sxs-lookup"><span data-stu-id="ddb90-116">A conditional directive beginning with a `#if` directive must explicitly be terminated with a `#endif` directive.</span></span>

<span data-ttu-id="ddb90-117">`#define` 允许你定义一个符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-117">`#define` lets you define a symbol.</span></span> <span data-ttu-id="ddb90-118">然后通过将该符号用作传递给 `#if` 指令的表达式，该表达式的计算结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="ddb90-118">By then using the symbol as the expression passed to the `#if` directive, the expression evaluates to `true`.</span></span>

<span data-ttu-id="ddb90-119">还可以通过 [DefineConstants](../compiler-options/language.md#defineconstants) 编译器选项来定义符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-119">You can also define a symbol with the [**DefineConstants**](../compiler-options/language.md#defineconstants) compiler option.</span></span> <span data-ttu-id="ddb90-120">可以通过 [#undef](preprocessor-undef.md) 取消定义符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-120">You can undefine a symbol with [#undef](preprocessor-undef.md).</span></span>

<span data-ttu-id="ddb90-121">使用 `-define` 或 `#define` 定义的符号与具有相同名称的变量不冲突。</span><span class="sxs-lookup"><span data-stu-id="ddb90-121">A symbol that you define with `-define` or with `#define` doesn't conflict with a variable of the same name.</span></span> <span data-ttu-id="ddb90-122">也就是说，变量名称不应传递给预处理器指令，且符号仅能由预处理器指令评估。</span><span class="sxs-lookup"><span data-stu-id="ddb90-122">That is, a variable name should not be passed to a preprocessor directive, and a symbol can only be evaluated by a preprocessor directive.</span></span>

<span data-ttu-id="ddb90-123">使用 `#define` 创建的符号的作用域是在其中定义它的文件。</span><span class="sxs-lookup"><span data-stu-id="ddb90-123">The scope of a symbol created with `#define` is the file in which it was defined.</span></span>

<span data-ttu-id="ddb90-124">此外，生成系统还会感知表示 SDK 样式项目中不同[目标框架](../../../standard/frameworks.md)的预定义预处理器符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-124">The build system is also aware of predefined preprocessor symbols representing different [target frameworks](../../../standard/frameworks.md) in SDK-style projects.</span></span> <span data-ttu-id="ddb90-125">在创建可以面向多个 .NET 版本的应用程序时，这些符号会很有用。</span><span class="sxs-lookup"><span data-stu-id="ddb90-125">They're useful when creating applications that can target more than one .NET version.</span></span>

[!INCLUDE [Preprocessor symbols](~/includes/preprocessor-symbols.md)]

> [!NOTE]
> <span data-ttu-id="ddb90-126">对于传统的非 SDK 样式的项目，必须通过项目的属性页面在 Visual Studio 中为不同目标框架手动配置条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-126">For traditional, non-SDK-style projects, you have to manually configure the conditional compilation symbols for the different target frameworks in Visual Studio via the project's properties pages.</span></span>

<span data-ttu-id="ddb90-127">其他预定义符号包括 DEBUG 和 TRACE 常量。</span><span class="sxs-lookup"><span data-stu-id="ddb90-127">Other predefined symbols include the DEBUG and TRACE constants.</span></span> <span data-ttu-id="ddb90-128">你可以使用 `#define` 替代项目的值集。</span><span class="sxs-lookup"><span data-stu-id="ddb90-128">You can override the values set for the project using `#define`.</span></span> <span data-ttu-id="ddb90-129">例如，会根据生成配置属性（“调试”或者“发布”模式）自动设置 DEBUG 符号。</span><span class="sxs-lookup"><span data-stu-id="ddb90-129">The DEBUG symbol, for example, is automatically set depending on your build configuration properties ("Debug" or "Release" mode).</span></span>

## <a name="examples"></a><span data-ttu-id="ddb90-130">示例</span><span class="sxs-lookup"><span data-stu-id="ddb90-130">Examples</span></span>

<span data-ttu-id="ddb90-131">下例显示如何在文件上定义 MYTEST 符号，然后测试 MYTEST 和 DEBUG 符号的值。</span><span class="sxs-lookup"><span data-stu-id="ddb90-131">The following example shows you how to define a MYTEST symbol on a file and then test the values of the MYTEST and DEBUG symbols.</span></span> <span data-ttu-id="ddb90-132">此示例的输出取决于是在“调试”还是“发布”配置模式下生成项目。</span><span class="sxs-lookup"><span data-stu-id="ddb90-132">The output of this example depends on whether you built the project on Debug or Release configuration mode.</span></span>

```csharp
#define MYTEST
using System;
public class MyClass
{
    static void Main()
    {
#if (DEBUG && !MYTEST)
        Console.WriteLine("DEBUG is defined");
#elif (!DEBUG && MYTEST)
        Console.WriteLine("MYTEST is defined");
#elif (DEBUG && MYTEST)
        Console.WriteLine("DEBUG and MYTEST are defined");  
#else
        Console.WriteLine("DEBUG and MYTEST are not defined");
#endif
    }
}
```

<span data-ttu-id="ddb90-133">下例显示如何针对不同的目标框架进行测试，以便在可能时使用较新的 API：</span><span class="sxs-lookup"><span data-stu-id="ddb90-133">The following example shows you how to test for different target frameworks so you can use newer APIs when possible:</span></span>

```csharp
public class MyClass
{
    static void Main()
    {
#if NET40
        WebClient _client = new WebClient();
#else
        HttpClient _client = new HttpClient();
#endif
    }
    //...
}
```

## <a name="see-also"></a><span data-ttu-id="ddb90-134">请参阅</span><span class="sxs-lookup"><span data-stu-id="ddb90-134">See also</span></span>

- [<span data-ttu-id="ddb90-135">C# 参考</span><span class="sxs-lookup"><span data-stu-id="ddb90-135">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="ddb90-136">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="ddb90-136">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="ddb90-137">C# 预处理器指令</span><span class="sxs-lookup"><span data-stu-id="ddb90-137">C# Preprocessor Directives</span></span>](index.md)
- [<span data-ttu-id="ddb90-138">如何：使用跟踪和调试执行有条件编译</span><span class="sxs-lookup"><span data-stu-id="ddb90-138">How to: Compile Conditionally with Trace and Debug</span></span>](../../../framework/debug-trace-profile/how-to-compile-conditionally-with-trace-and-debug.md)
