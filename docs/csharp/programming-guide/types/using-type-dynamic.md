---
title: 使用类型 dynamic - C# 编程指南
description: 了解如何使用动态类型。 动态类型是一种静态类型，但动态对象会跳过静态类型检查。
ms.date: 07/20/2015
helpviewer_keywords:
- dynamic [C#], about dynamic type
- dynamic type [C#]
ms.assetid: 3828989d-c967-4a51-b948-857ebc8fdf26
ms.openlocfilehash: 68e22f9d25b29784f73fefdf80808f9c2ba5e0f1
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478648"
---
# <a name="using-type-dynamic-c-programming-guide"></a><span data-ttu-id="c328a-104">使用类型 dynamic（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="c328a-104">Using type dynamic (C# Programming Guide)</span></span>

<span data-ttu-id="c328a-105">C# 4 引入了一个新类型 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="c328a-105">C# 4 introduces a new type, `dynamic`.</span></span> <span data-ttu-id="c328a-106">该类型是一种静态类型，但类型为 `dynamic` 的对象会跳过静态类型检查。</span><span class="sxs-lookup"><span data-stu-id="c328a-106">The type is a static type, but an object of type `dynamic` bypasses static type checking.</span></span> <span data-ttu-id="c328a-107">大多数情况下，该对象就像具有类型 `object` 一样。</span><span class="sxs-lookup"><span data-stu-id="c328a-107">In most cases, it functions like it has type `object`.</span></span> <span data-ttu-id="c328a-108">在编译时，将假定类型化为 `dynamic` 的元素支持任何操作。</span><span class="sxs-lookup"><span data-stu-id="c328a-108">At compile time, an element that is typed as `dynamic` is assumed to support any operation.</span></span> <span data-ttu-id="c328a-109">因此，不必考虑对象是从 COM API、从动态语言（例如 IronPython）、从 HTML 文档对象模型 (DOM)、从反射还是从程序中的其他位置获取自己的值。</span><span class="sxs-lookup"><span data-stu-id="c328a-109">Therefore, you do not have to be concerned about whether the object gets its value from a COM API, from a dynamic language such as IronPython, from the HTML Document Object Model (DOM), from reflection, or from somewhere else in the program.</span></span> <span data-ttu-id="c328a-110">但是，如果代码无效，则在运行时会捕获到错误。</span><span class="sxs-lookup"><span data-stu-id="c328a-110">However, if the code is not valid, errors are caught at run time.</span></span>

<span data-ttu-id="c328a-111">例如，如果以下代码中的实例方法 `exampleMethod1` 只有一个形参，则编译器会将对该方法的第一个调用 `ec.exampleMethod1(10, 4)` 识别为无效，因为它包含两个实参。</span><span class="sxs-lookup"><span data-stu-id="c328a-111">For example, if instance method `exampleMethod1` in the following code has only one parameter, the compiler recognizes that the first call to the method, `ec.exampleMethod1(10, 4)`, is not valid because it contains two arguments.</span></span> <span data-ttu-id="c328a-112">该调用将导致编译器错误。</span><span class="sxs-lookup"><span data-stu-id="c328a-112">The call causes a compiler error.</span></span> <span data-ttu-id="c328a-113">编译器不会检查对该方法的第二个调用 `dynamic_ec.exampleMethod1(10, 4)`，因为 `dynamic_ec` 的类型为 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="c328a-113">The second call to the method, `dynamic_ec.exampleMethod1(10, 4)`, is not checked by the compiler because the type of `dynamic_ec` is `dynamic`.</span></span> <span data-ttu-id="c328a-114">因此，不会报告编译器错误。</span><span class="sxs-lookup"><span data-stu-id="c328a-114">Therefore, no compiler error is reported.</span></span> <span data-ttu-id="c328a-115">但是，该错误不会被无限期疏忽。</span><span class="sxs-lookup"><span data-stu-id="c328a-115">However, the error does not escape notice indefinitely.</span></span> <span data-ttu-id="c328a-116">它将在运行时被捕获，并导致运行时异常。</span><span class="sxs-lookup"><span data-stu-id="c328a-116">It is caught at run time and causes a run-time exception.</span></span>

[!code-csharp[CsProgGuideTypes#50](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#50)]

[!code-csharp[CsProgGuideTypes#56](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#56)]

<span data-ttu-id="c328a-117">在这些示例中，编译器的作用是将有关每个语句的预期作用的信息一起打包到类型化为 `dynamic` 的对象或表达式。</span><span class="sxs-lookup"><span data-stu-id="c328a-117">The role of the compiler in these examples is to package together information about what each statement is proposing to do to the object or expression that is typed as `dynamic`.</span></span> <span data-ttu-id="c328a-118">在运行时，将对存储的信息进行检查，并且任何无效的语句都将导致运行时异常。</span><span class="sxs-lookup"><span data-stu-id="c328a-118">At run time, the stored information is examined, and any statement that is not valid causes a run-time exception.</span></span>

<span data-ttu-id="c328a-119">大多数动态操作的结果是其本身 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="c328a-119">The result of most dynamic operations is itself `dynamic`.</span></span> <span data-ttu-id="c328a-120">例如，如果将鼠标指针放在以下示例中使用的 `testSum` 上，则 IntelliSense 将显示类型“（局部变量）dynamic testSum”。</span><span class="sxs-lookup"><span data-stu-id="c328a-120">For example, if you rest the mouse pointer over the use of `testSum` in the following example, IntelliSense displays the type **(local variable) dynamic testSum**.</span></span>

[!code-csharp[CsProgGuideTypes#51](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#51)]

<span data-ttu-id="c328a-121">结果不为 `dynamic` 的操作包括：</span><span class="sxs-lookup"><span data-stu-id="c328a-121">Operations in which the result is not `dynamic` include:</span></span>

* <span data-ttu-id="c328a-122">从 `dynamic` 到另一种类型的转换。</span><span class="sxs-lookup"><span data-stu-id="c328a-122">Conversions from `dynamic` to another type.</span></span>
* <span data-ttu-id="c328a-123">包括类型为 `dynamic` 的自变量的构造函数调用。</span><span class="sxs-lookup"><span data-stu-id="c328a-123">Constructor calls that include arguments of type `dynamic`.</span></span>

<span data-ttu-id="c328a-124">例如，以下声明中 `testInstance` 的类型为 `ExampleClass`，而不是 `dynamic`：</span><span class="sxs-lookup"><span data-stu-id="c328a-124">For example, the type of `testInstance` in the following declaration is `ExampleClass`, not `dynamic`:</span></span>

[!code-csharp[CsProgGuideTypes#52](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#52)]

<span data-ttu-id="c328a-125">下一节“转换”中介绍了转换示例。</span><span class="sxs-lookup"><span data-stu-id="c328a-125">Conversion examples are shown in the following section, "Conversions."</span></span>

## <a name="conversions"></a><span data-ttu-id="c328a-126">转换</span><span class="sxs-lookup"><span data-stu-id="c328a-126">Conversions</span></span>

<span data-ttu-id="c328a-127">动态对象和其他类型之间的转换非常简单。</span><span class="sxs-lookup"><span data-stu-id="c328a-127">Conversions between dynamic objects and other types are easy.</span></span> <span data-ttu-id="c328a-128">这样，开发人员将能够在动态行为和非动态行为之间切换。</span><span class="sxs-lookup"><span data-stu-id="c328a-128">This enables the developer to switch between dynamic and non-dynamic behavior.</span></span>

<span data-ttu-id="c328a-129">任何对象都可隐式转换为动态类型，如以下示例所示。</span><span class="sxs-lookup"><span data-stu-id="c328a-129">Any object can be converted to dynamic type implicitly, as shown in the following examples.</span></span>

[!code-csharp[CsProgGuideTypes#53](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#53)]

<span data-ttu-id="c328a-130">反之，隐式转换也可动态地应用于类型为 `dynamic` 的任何表达式。</span><span class="sxs-lookup"><span data-stu-id="c328a-130">Conversely, an implicit conversion can be dynamically applied to any expression of type `dynamic`.</span></span>

[!code-csharp[CsProgGuideTypes#54](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#54)]

## <a name="overload-resolution-with-arguments-of-type-dynamic"></a><span data-ttu-id="c328a-131">使用类型为 dynamic 的参数重载决策</span><span class="sxs-lookup"><span data-stu-id="c328a-131">Overload resolution with arguments of type dynamic</span></span>

<span data-ttu-id="c328a-132">如果方法调用中的一个或多个参数的类型为 `dynamic`，或者方法调用的接收方的类型为 `dynamic`，则会在运行时（而不是在编译时）进行重载决策。</span><span class="sxs-lookup"><span data-stu-id="c328a-132">Overload resolution occurs at run time instead of at compile time if one or more of the arguments in a method call have the type `dynamic`, or if the receiver of the method call is of type `dynamic`.</span></span> <span data-ttu-id="c328a-133">在以下示例中，如果唯一可访问的 `exampleMethod2` 方法定义为接受字符串参数，则将 `d1` 作为参数发送不会导致编译器错误，但却会导致运行时异常。</span><span class="sxs-lookup"><span data-stu-id="c328a-133">In the following example, if the only accessible `exampleMethod2` method is defined to take a string argument, sending `d1` as the argument does not cause a compiler error, but it does cause a run-time exception.</span></span> <span data-ttu-id="c328a-134">重载决策之所以会在运行时失败，是因为 `d1` 的运行时类型为 `int`，而 `exampleMethod2` 要求为字符串。</span><span class="sxs-lookup"><span data-stu-id="c328a-134">Overload resolution fails at run time because the run-time type of `d1` is `int`, and `exampleMethod2` requires a string.</span></span>

[!code-csharp[CsProgGuideTypes#55](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/usingdynamic.cs#55)]

## <a name="dynamic-language-runtime"></a><span data-ttu-id="c328a-135">动态语言运行时</span><span class="sxs-lookup"><span data-stu-id="c328a-135">Dynamic language runtime</span></span>

<span data-ttu-id="c328a-136">动态语言运行时 (DLR) 是 .NET Framework 4 中引入的 API。</span><span class="sxs-lookup"><span data-stu-id="c328a-136">The dynamic language runtime (DLR) is an API that was introduced in .NET Framework 4.</span></span> <span data-ttu-id="c328a-137">它提供了支持 C# 中 `dynamic` 类型的基础结构，还提供了 IronPython 和 IronRuby 等动态编程语言的实现。</span><span class="sxs-lookup"><span data-stu-id="c328a-137">It provides the infrastructure that supports the `dynamic` type in C#, and also the implementation of dynamic programming languages such as IronPython and IronRuby.</span></span> <span data-ttu-id="c328a-138">有关 DLR 的详细信息，请参阅[动态语言运行时概述](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="c328a-138">For more information about the DLR, see [Dynamic Language Runtime Overview](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md).</span></span>

## <a name="com-interop"></a><span data-ttu-id="c328a-139">COM 互操作</span><span class="sxs-lookup"><span data-stu-id="c328a-139">COM interop</span></span>

<span data-ttu-id="c328a-140">C# 4 包括若干功能，这些功能改善了与 COM API（例如 Office 自动化 API）的互操作体验。</span><span class="sxs-lookup"><span data-stu-id="c328a-140">C# 4 includes several features that improve the experience of interoperating with COM APIs such as the Office Automation APIs.</span></span> <span data-ttu-id="c328a-141">这些改进之处包括 `dynamic` 类型以及[命名参数和可选参数](../classes-and-structs/named-and-optional-arguments.md)的用法。</span><span class="sxs-lookup"><span data-stu-id="c328a-141">Among the improvements are the use of the `dynamic` type, and of [named and optional arguments](../classes-and-structs/named-and-optional-arguments.md).</span></span>

<span data-ttu-id="c328a-142">通过将类型指定为 `object`，许多 COM 方法都允许参数类型和返回类型发生变化。</span><span class="sxs-lookup"><span data-stu-id="c328a-142">Many COM methods allow for variation in argument types and return type by designating the types as `object`.</span></span> <span data-ttu-id="c328a-143">这样，就必须显式强制转换值，以便与 C# 中的强类型变量保持协调。</span><span class="sxs-lookup"><span data-stu-id="c328a-143">This has necessitated explicit casting of the values to coordinate with strongly typed variables in C#.</span></span> <span data-ttu-id="c328a-144">如果使用 [EmbedInteropTypes（C# 编译器选项）](../../language-reference/compiler-options/inputs.md#embedinteroptypes)选项进行编译，则可以通过引入 `dynamic` 类型将 COM 签名中出现的 `object` 看作是 `dynamic` 类型，从而避免大量的强制转换。</span><span class="sxs-lookup"><span data-stu-id="c328a-144">If you compile by using the [**EmbedInteropTypes** (C# Compiler Options)](../../language-reference/compiler-options/inputs.md#embedinteroptypes) option, the introduction of the `dynamic` type enables you to treat the occurrences of `object` in COM signatures as if they were of type `dynamic`, and thereby to avoid much of the casting.</span></span> <span data-ttu-id="c328a-145">例如，以下语句对比了在使用 `dynamic` 类型和不使用 `dynamic` 类型的情况下如何访问 Microsoft Office Excel 电子表格中的单元格。</span><span class="sxs-lookup"><span data-stu-id="c328a-145">For example, the following statements contrast how you access a cell in a Microsoft Office Excel spreadsheet with the `dynamic` type and without the `dynamic` type.</span></span>

[!code-csharp[csOfficeWalkthrough#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#12)]

[!code-csharp[csOfficeWalkthrough#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#13)]

## <a name="related-topics"></a><span data-ttu-id="c328a-146">相关主题</span><span class="sxs-lookup"><span data-stu-id="c328a-146">Related topics</span></span>

|<span data-ttu-id="c328a-147">Title</span><span class="sxs-lookup"><span data-stu-id="c328a-147">Title</span></span>|<span data-ttu-id="c328a-148">描述</span><span class="sxs-lookup"><span data-stu-id="c328a-148">Description</span></span>|
|-----------|-----------------|
|[<span data-ttu-id="c328a-149">dynamic</span><span class="sxs-lookup"><span data-stu-id="c328a-149">dynamic</span></span>](../../language-reference/builtin-types/reference-types.md)|<span data-ttu-id="c328a-150">描述 `dynamic` 关键字的用法。</span><span class="sxs-lookup"><span data-stu-id="c328a-150">Describes the usage of the `dynamic` keyword.</span></span>|
|[<span data-ttu-id="c328a-151">动态语言运行时概述</span><span class="sxs-lookup"><span data-stu-id="c328a-151">Dynamic Language Runtime Overview</span></span>](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)|<span data-ttu-id="c328a-152">提供有关 DLR 的概述，DLR 是一种运行时环境，它将一组适用于动态语言的服务添加到公共语言运行时 (CLR)。</span><span class="sxs-lookup"><span data-stu-id="c328a-152">Provides an overview of the DLR, which is a runtime environment that adds a set of services for dynamic languages to the common language runtime (CLR).</span></span>|
|[<span data-ttu-id="c328a-153">演练：创建和使用动态对象</span><span class="sxs-lookup"><span data-stu-id="c328a-153">Walkthrough: Creating and Using Dynamic Objects</span></span>](walkthrough-creating-and-using-dynamic-objects.md)|<span data-ttu-id="c328a-154">提供有关如何创建自定义动态对象以及创建访问 `IronPython` 库的对象的分步说明。</span><span class="sxs-lookup"><span data-stu-id="c328a-154">Provides step-by-step instructions for creating a custom dynamic object and for creating a project that accesses an `IronPython` library.</span></span>|
|[<span data-ttu-id="c328a-155">如何使用 C# 功能访问 Office 互操作对象</span><span class="sxs-lookup"><span data-stu-id="c328a-155">How to access Office interop objects by using C# features</span></span>](../interop/how-to-access-office-onterop-objects.md)|<span data-ttu-id="c328a-156">演示如何创建一个项目，该项目使用命名参数和可选参数、`dynamic` 类型以及可简化对 Office API 对象的访问的其他增强功能。</span><span class="sxs-lookup"><span data-stu-id="c328a-156">Demonstrates how to create a project that uses named and optional arguments, the `dynamic` type, and other enhancements that simplify access to Office API objects.</span></span>|
