---
description: '! （null 包容）运算符 - C# 参考'
title: '! （null 包容）运算符 - C# 参考'
ms.date: 11/13/2020
f1_keywords:
- nullForgiving_CSharpKeyword
helpviewer_keywords:
- null-forgiving operator [C#]
- '! operator [C#]'
ms.openlocfilehash: 14bc4501dfed515e162a6e30137c9def89bb3651
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111070"
---
# <a name="-null-forgiving-operator-c-reference"></a><span data-ttu-id="bb5de-105">!</span><span class="sxs-lookup"><span data-stu-id="bb5de-105">!</span></span> <span data-ttu-id="bb5de-106">（null 包容）运算符（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="bb5de-106">(null-forgiving) operator (C# reference)</span></span>

<span data-ttu-id="bb5de-107">在 C# 8.0 及更高版本中可用，一元后缀 `!` 运算符是 null 包容运算符或 null 抑制运算符。</span><span class="sxs-lookup"><span data-stu-id="bb5de-107">Available in C# 8.0 and later, the unary postfix `!` operator is the null-forgiving, or null-suppression, operator.</span></span> <span data-ttu-id="bb5de-108">在已启用的[可为空的注释上下文](../../nullable-references.md#nullable-annotation-context)中，可以使用 null 包容运算符来声明可为空的引用类型的表达式 `x` 不为 `null`：`x!`。</span><span class="sxs-lookup"><span data-stu-id="bb5de-108">In an enabled [nullable annotation context](../../nullable-references.md#nullable-annotation-context), you use the null-forgiving operator to declare that expression `x` of a reference type isn't `null`: `x!`.</span></span> <span data-ttu-id="bb5de-109">一元前缀 `!` 运算符是[逻辑非运算符](boolean-logical-operators.md#logical-negation-operator-)。</span><span class="sxs-lookup"><span data-stu-id="bb5de-109">The unary prefix `!` operator is the [logical negation operator](boolean-logical-operators.md#logical-negation-operator-).</span></span>

<span data-ttu-id="bb5de-110">null 包容运算符在运行时不起作用。</span><span class="sxs-lookup"><span data-stu-id="bb5de-110">The null-forgiving operator has no effect at run time.</span></span> <span data-ttu-id="bb5de-111">它仅通过更改表达式的 null 状态来影响编译器的静态流分析。</span><span class="sxs-lookup"><span data-stu-id="bb5de-111">It only affects the compiler's static flow analysis by changing the null state of the expression.</span></span> <span data-ttu-id="bb5de-112">在运行时，表达式 `x!` 的计算结果为基础表达式 `x` 的结果。</span><span class="sxs-lookup"><span data-stu-id="bb5de-112">At run time, expression `x!` evaluates to the result of the underlying expression `x`.</span></span>

<span data-ttu-id="bb5de-113">有关可为空引用类型特性的详细信息，请参见[可为空引用类型](../builtin-types/nullable-reference-types.md)。</span><span class="sxs-lookup"><span data-stu-id="bb5de-113">For more information about the nullable reference types feature, see [Nullable reference types](../builtin-types/nullable-reference-types.md).</span></span>

## <a name="examples"></a><span data-ttu-id="bb5de-114">示例</span><span class="sxs-lookup"><span data-stu-id="bb5de-114">Examples</span></span>

<span data-ttu-id="bb5de-115">null 包容运算符的一个用例是测试参数验证逻辑。</span><span class="sxs-lookup"><span data-stu-id="bb5de-115">One of the use cases of the null-forgiving operator is in testing the argument validation logic.</span></span> <span data-ttu-id="bb5de-116">例如，请考虑以下类：</span><span class="sxs-lookup"><span data-stu-id="bb5de-116">For example, consider the following class:</span></span>

[!code-csharp[Person class](snippets/shared/NullForgivingOperator.cs#PersonClass)]

<span data-ttu-id="bb5de-117">使用 [ 测试框架](../../../core/testing/unit-testing-with-mstest.md)，可以在构造函数中为验证逻辑创建以下测试：</span><span class="sxs-lookup"><span data-stu-id="bb5de-117">Using the [MSTest test framework](../../../core/testing/unit-testing-with-mstest.md), you can create the following test for the validation logic in the constructor:</span></span>

[!code-csharp[Person test](snippets/shared/NullForgivingOperator.cs#TestPerson)]

<span data-ttu-id="bb5de-118">如果不使用 null 包容运算符，编译器将为前面的代码生成以下警告：`Warning CS8625: Cannot convert null literal to non-nullable reference type`。</span><span class="sxs-lookup"><span data-stu-id="bb5de-118">Without the null-forgiving operator, the compiler generates the following warning for the preceding code: `Warning CS8625: Cannot convert null literal to non-nullable reference type`.</span></span> <span data-ttu-id="bb5de-119">通过使用 null 包容运算符，可以告知编译器传递 `null` 是预期行为，不应发出警告。</span><span class="sxs-lookup"><span data-stu-id="bb5de-119">By using the null-forgiving operator, you inform the compiler that passing `null` is expected and shouldn't be warned about.</span></span>

<span data-ttu-id="bb5de-120">如果你明确知道某个表达式不能为 `null`，但编译器无法识别它，也可以使用 null 包容运算符。</span><span class="sxs-lookup"><span data-stu-id="bb5de-120">You can also use the null-forgiving operator when you definitely know that an expression cannot be `null` but the compiler doesn't manage to recognize that.</span></span> <span data-ttu-id="bb5de-121">在下面的示例中，如果 `IsValid` 方法返回 `true`，则其参数不是 `null`，可以放心取消对它的引用：</span><span class="sxs-lookup"><span data-stu-id="bb5de-121">In the following example, if the `IsValid` method returns `true`, its argument is not `null` and you can safely dereference it:</span></span>

[!code-csharp[Use null-forgiving operator](snippets/shared/NullForgivingOperator.cs#UseNullForgiving)]

<span data-ttu-id="bb5de-122">如果没有 null 包容运算符，编译器将为 `p.Name` 代码生成以下警告：`Warning CS8602: Dereference of a possibly null reference`。</span><span class="sxs-lookup"><span data-stu-id="bb5de-122">Without the null-forgiving operator, the compiler generates the following warning for the `p.Name` code: `Warning CS8602: Dereference of a possibly null reference`.</span></span>

<span data-ttu-id="bb5de-123">如果可以修改 `IsValid` 方法，则可使用 [NotNullWhen](xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute) 属性告知编译器，当方法返回 `true` 时，`IsValid` 方法的参数不能是 `null`：</span><span class="sxs-lookup"><span data-stu-id="bb5de-123">If you can modify the `IsValid` method, you can use the [NotNullWhen](xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute) attribute to inform the compiler that an argument of the `IsValid` method cannot be `null` when the method returns `true`:</span></span>

[!code-csharp[Use an attribute](snippets/shared/NullForgivingOperator.cs#UseAttribute)]

<span data-ttu-id="bb5de-124">在前面的例子中，不需要使用 null 包容运算符，因为编译器有足够的信息来发现 `p` 不能是 `if` 语句中的 `null`。</span><span class="sxs-lookup"><span data-stu-id="bb5de-124">In the preceding example, you don't need to use the null-forgiving operator because the compiler has enough information to find out that `p` cannot be `null` inside the `if` statement.</span></span> <span data-ttu-id="bb5de-125">如需深入了解允许你提供有关变量 null 状态的其他信息的属性，请参阅[使用属性升级 API 以定义 null 期望值](../attributes/nullable-analysis.md)。</span><span class="sxs-lookup"><span data-stu-id="bb5de-125">For more information about the attributes that allow you to provide additional information about the null state of a variable, see [Upgrade APIs with attributes to define null expectations](../attributes/nullable-analysis.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="bb5de-126">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="bb5de-126">C# language specification</span></span>

<span data-ttu-id="bb5de-127">有关详细信息，请参阅[可为空的引用类型规范草案](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md)的 [null 包容性运算符](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md#the-null-forgiving-operator)部分。</span><span class="sxs-lookup"><span data-stu-id="bb5de-127">For more information, see [The null-forgiving operator](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md#the-null-forgiving-operator) section of the [draft of the nullable reference types specification](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="bb5de-128">请参阅</span><span class="sxs-lookup"><span data-stu-id="bb5de-128">See also</span></span>

- [<span data-ttu-id="bb5de-129">C# 参考</span><span class="sxs-lookup"><span data-stu-id="bb5de-129">C# reference</span></span>](../index.md)
- [<span data-ttu-id="bb5de-130">C# 运算符和表达式</span><span class="sxs-lookup"><span data-stu-id="bb5de-130">C# operators and expressions</span></span>](index.md)
- [<span data-ttu-id="bb5de-131">教程：使用可为空引用类型进行设计</span><span class="sxs-lookup"><span data-stu-id="bb5de-131">Tutorial: Design with nullable reference types</span></span>](../../whats-new/tutorials/nullable-reference-types.md)
