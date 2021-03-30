---
title: 可为空引用类型
description: 本文概述了在 C# 8.0 中添加的可为空引用类型。 你将了解该功能如何为新项目和现有项目提供针对空引用异常的安全性。
ms.technology: csharp-null-safety
ms.date: 04/21/2020
ms.openlocfilehash: da3b75b28d7501e8436d29c0c325c550f0a44c93
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637155"
---
# <a name="nullable-reference-types"></a><span data-ttu-id="feb70-104">可为空引用类型</span><span class="sxs-lookup"><span data-stu-id="feb70-104">Nullable reference types</span></span>

<span data-ttu-id="feb70-105">C#8.0 引入了“可为空引用类型”和“不可为空引用类型”，使你能够对引用类型变量的属性作出重要声明 ：</span><span class="sxs-lookup"><span data-stu-id="feb70-105">C# 8.0 introduces **nullable reference types** and **non-nullable reference types** that enable you to make important statements about the properties for reference type variables:</span></span>

- <span data-ttu-id="feb70-106">引用不应为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-106">**A reference isn't supposed to be null**.</span></span> <span data-ttu-id="feb70-107">当变量不应为 null 时，编译器会强制执行规则，以确保在不首先检查它们是否为 null 的情况下，取消引用这些变量是安全的：</span><span class="sxs-lookup"><span data-stu-id="feb70-107">When variables aren't supposed to be null, the compiler enforces rules that ensure it's safe to dereference these variables without first checking that it isn't null:</span></span>
  - <span data-ttu-id="feb70-108">必须将变量初始化为非 null 值。</span><span class="sxs-lookup"><span data-stu-id="feb70-108">The variable must be initialized to a non-null value.</span></span>
  - <span data-ttu-id="feb70-109">变量永远不能赋值为 `null`。</span><span class="sxs-lookup"><span data-stu-id="feb70-109">The variable can never be assigned the value `null`.</span></span>
- <span data-ttu-id="feb70-110">引用可为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-110">**A reference may be null**.</span></span> <span data-ttu-id="feb70-111">当变量可以为 null 时，编译器会强制执行不同的规则以确保你已正确检查空引用：</span><span class="sxs-lookup"><span data-stu-id="feb70-111">When variables may be null, the compiler enforces different rules to ensure that you've correctly checked for a null reference:</span></span>
  - <span data-ttu-id="feb70-112">只有当编译器可以保证该值不为 null 时，才可以取消引用该变量。</span><span class="sxs-lookup"><span data-stu-id="feb70-112">The variable may only be dereferenced when the compiler can guarantee that the value isn't null.</span></span>
  - <span data-ttu-id="feb70-113">这些变量可以使用默认的 `null` 值进行初始化，也可以在其他代码中赋值为 `null`。</span><span class="sxs-lookup"><span data-stu-id="feb70-113">These variables may be initialized with the default `null` value and may be assigned the value `null` in other code.</span></span>

<span data-ttu-id="feb70-114">在 C# 的早期版本中，无法从变量声明中确定设计意图，与处理引用变量相比，这个新功能提供了显著的好处。</span><span class="sxs-lookup"><span data-stu-id="feb70-114">This new feature provides significant benefits over the handling of reference variables in earlier versions of C# where the design intent can't be determined from the variable declaration.</span></span> <span data-ttu-id="feb70-115">编译器不提供针对引用类型的空引用异常的安全性：</span><span class="sxs-lookup"><span data-stu-id="feb70-115">The compiler didn't provide safety against null reference exceptions for reference types:</span></span>

- <span data-ttu-id="feb70-116">引用可为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-116">**A reference can be null**.</span></span> <span data-ttu-id="feb70-117">将引用类型变量初始化为 `null` 或稍后将其指定为 `null` 时，编译器不会发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-117">The compiler doesn't issue warnings when a reference-type variable is initialized to `null`, or later assigned `null`.</span></span> <span data-ttu-id="feb70-118">在没有进行 null 检查的情况下取消引用这些变量，编译器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-118">The compiler issues warnings when these variables are dereferenced without null checks.</span></span>
- <span data-ttu-id="feb70-119">假定引用不为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-119">**A reference is assumed to be not null**.</span></span> <span data-ttu-id="feb70-120">当引用类型被取消引用时，编译器不会发出任何警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-120">The compiler doesn't issue any warnings when reference types are dereferenced.</span></span> <span data-ttu-id="feb70-121">如果将变量设置为可以为 null 的表达式，则编译器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-121">The compiler issues warnings if a variable is set to an expression that may be null.</span></span>

<span data-ttu-id="feb70-122">将在编译时发出这些警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-122">These warnings are emitted at compile time.</span></span> <span data-ttu-id="feb70-123">编译器不会在可为 null 的上下文中添加任何 null 检查或其他运行时构造。</span><span class="sxs-lookup"><span data-stu-id="feb70-123">The compiler doesn't add any null checks or other runtime constructs in a nullable context.</span></span> <span data-ttu-id="feb70-124">在运行时，可为 null 的引用和不可为 null 的引用是等效的。</span><span class="sxs-lookup"><span data-stu-id="feb70-124">At runtime, a nullable reference and a non-nullable reference are equivalent.</span></span>

<span data-ttu-id="feb70-125">通过添加可为空引用类型，你可以更清楚地声明你的意图。</span><span class="sxs-lookup"><span data-stu-id="feb70-125">With the addition of nullable reference types, you can declare your intent more clearly.</span></span> <span data-ttu-id="feb70-126">`null` 值是表示变量不引用值的正确方法。</span><span class="sxs-lookup"><span data-stu-id="feb70-126">The `null` value is the correct way to represent that a variable doesn't refer to a value.</span></span> <span data-ttu-id="feb70-127">请勿使用此功能从代码中删除所有 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="feb70-127">Don't use this feature to remove all `null` values from your code.</span></span> <span data-ttu-id="feb70-128">相反，应该向编译器和其他读取代码的开发人员声明你的意图。</span><span class="sxs-lookup"><span data-stu-id="feb70-128">Rather, you should declare your intent to the compiler and other developers that read your code.</span></span> <span data-ttu-id="feb70-129">通过声明意图，编译器会在你编写与该意图不一致的代码时通知你。</span><span class="sxs-lookup"><span data-stu-id="feb70-129">By declaring your intent, the compiler informs you when you write code that is inconsistent with that intent.</span></span>

<span data-ttu-id="feb70-130">使用与 [可为空值类型](language-reference/builtin-types/nullable-value-types.md)相同的语法记录 **可为空引用类型**：将 `?` 附加到变量的类型。</span><span class="sxs-lookup"><span data-stu-id="feb70-130">A **nullable reference type** is noted using the same syntax as [nullable value types](language-reference/builtin-types/nullable-value-types.md): a `?` is appended to the type of the variable.</span></span> <span data-ttu-id="feb70-131">例如，以下变量声明表示可为空的字符串变量 `name`：</span><span class="sxs-lookup"><span data-stu-id="feb70-131">For example, the following variable declaration represents a nullable string variable, `name`:</span></span>

```csharp
string? name;
```

<span data-ttu-id="feb70-132">未将 `?` 附加到类型名称的任何变量都是“不可为 null 引用类型”。</span><span class="sxs-lookup"><span data-stu-id="feb70-132">Any variable where the `?` isn't appended to the type name is a **non-nullable reference type**.</span></span> <span data-ttu-id="feb70-133">这包括启用此功能时现有代码中的所有引用类型变量。</span><span class="sxs-lookup"><span data-stu-id="feb70-133">That includes all reference type variables in existing code when you've enabled this feature.</span></span>

<span data-ttu-id="feb70-134">编译器使用静态分析来确定可为空引用是否为非 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-134">The compiler uses static analysis to determine if a nullable reference is known to be non-null.</span></span> <span data-ttu-id="feb70-135">如果你在一个可为空引用可能是 null 时对其取消引用，编译器将向你发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-135">The compiler warns you if you dereference a nullable reference when it may be null.</span></span> <span data-ttu-id="feb70-136">可以通过使用 [NULL 包容运算符](language-reference/operators/null-forgiving.md) `!` 后跟变量名称来替代此行为。</span><span class="sxs-lookup"><span data-stu-id="feb70-136">You can override this behavior by using the [null-forgiving operator](language-reference/operators/null-forgiving.md) `!` following a variable name.</span></span> <span data-ttu-id="feb70-137">例如，若知道 `name` 变量不为 null 但编译器仍发出警告，则可以编写以下代码来覆盖编译器的分析：</span><span class="sxs-lookup"><span data-stu-id="feb70-137">For example, if you know the `name` variable isn't null but the compiler issues a warning, you can write the following code to override the compiler's analysis:</span></span>

```csharp
name!.Length;
```

## <a name="nullability-of-types"></a><span data-ttu-id="feb70-138">类型为 Null 性</span><span class="sxs-lookup"><span data-stu-id="feb70-138">Nullability of types</span></span>

<span data-ttu-id="feb70-139">任何引用类型都可以具有四个“为 Null 性”中的一个，它描述了何时生成警告：</span><span class="sxs-lookup"><span data-stu-id="feb70-139">Any reference type can have one of four *nullabilities*, which describes when warnings are generated:</span></span>

- <span data-ttu-id="feb70-140">*不可为空*：无法将 null 分配给此类型的变量。</span><span class="sxs-lookup"><span data-stu-id="feb70-140">*Nonnullable*: Null can't be assigned to variables of this type.</span></span> <span data-ttu-id="feb70-141">在取消引用之前，无需对此类型的变量进行 null 检查。</span><span class="sxs-lookup"><span data-stu-id="feb70-141">Variables of this type don't need to be null-checked before dereferencing.</span></span>
- <span data-ttu-id="feb70-142">*可为空*：可将 null 分配给此类型的变量。</span><span class="sxs-lookup"><span data-stu-id="feb70-142">*Nullable*: Null can be assigned to variables of this type.</span></span> <span data-ttu-id="feb70-143">在不首先检查 `null` 的情况下取消引用此类型的变量时发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-143">Dereferencing variables of this type without first checking for `null` causes a warning.</span></span>
- <span data-ttu-id="feb70-144">*无视*：“无视”是 C# 8.0 之前版本的状态。</span><span class="sxs-lookup"><span data-stu-id="feb70-144">*Oblivious*: Oblivious is the pre-C# 8.0 state.</span></span> <span data-ttu-id="feb70-145">可以取消引用或分配此类型的变量而不发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-145">Variables of this type can be dereferenced or assigned without warnings.</span></span>
- <span data-ttu-id="feb70-146">*未知*：“未知”通常针对类型参数，其中约束不告知编译器类型是否必须是“可为 null”或“不可为 null” 。</span><span class="sxs-lookup"><span data-stu-id="feb70-146">*Unknown*: Unknown is generally for type parameters where constraints don't tell the compiler that the type must be *nullable* or *nonnullable*.</span></span>

<span data-ttu-id="feb70-147">变量声明中类型的为 Null 性由声明变量的“可为空上下文”控制。</span><span class="sxs-lookup"><span data-stu-id="feb70-147">The nullability of a type in a variable declaration is controlled by the *nullable context* in which the variable is declared.</span></span>

## <a name="nullable-contexts"></a><span data-ttu-id="feb70-148">可为空上下文</span><span class="sxs-lookup"><span data-stu-id="feb70-148">Nullable contexts</span></span>

<span data-ttu-id="feb70-149">可为空上下文可以对编译器如何解释引用类型变量进行精细控制。</span><span class="sxs-lookup"><span data-stu-id="feb70-149">Nullable contexts enable fine-grained control for how the compiler interprets reference type variables.</span></span> <span data-ttu-id="feb70-150">可以启用或禁用任何给定源代码行的“可为空注释上下文”。</span><span class="sxs-lookup"><span data-stu-id="feb70-150">The **nullable annotation context** of any given source line is either enabled or disabled.</span></span> <span data-ttu-id="feb70-151">可以将 C# 8.0 之前的编译器视为在禁用的可为空上下文中编译所有代码：任何引用类型都可以为空。</span><span class="sxs-lookup"><span data-stu-id="feb70-151">You can think of the pre-C# 8.0 compiler as compiling all your code in a disabled nullable context: any reference type may be null.</span></span> <span data-ttu-id="feb70-152">还可以启用或禁用可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-152">The **nullable warnings context** may also be enabled or disabled.</span></span> <span data-ttu-id="feb70-153">可为空警告上下文指定编译器使用其流分析生成的警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-153">The nullable warnings context specifies the warnings generated by the compiler using its flow analysis.</span></span>

<span data-ttu-id="feb70-154">可以使用 .csproj 文件中的 `Nullable` 元素为项目设置可为空注释上下文和可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-154">The nullable annotation context and nullable warning context can be set for a project using the `Nullable` element in your *.csproj* file.</span></span> <span data-ttu-id="feb70-155">此元素配置编译器如何解释类型的为 Null 性以及生成哪些警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-155">This element configures how the compiler interprets the nullability of types and what warnings are generated.</span></span> <span data-ttu-id="feb70-156">有效设置如下：</span><span class="sxs-lookup"><span data-stu-id="feb70-156">Valid settings are:</span></span>

- <span data-ttu-id="feb70-157">`enable`：“启用”可为空注释上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-157">`enable`: The nullable annotation context is **enabled**.</span></span> <span data-ttu-id="feb70-158">“启用”可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-158">The nullable warning context is **enabled**.</span></span>
  - <span data-ttu-id="feb70-159">引用类型的变量，例如 `string` 是“不可为空”。</span><span class="sxs-lookup"><span data-stu-id="feb70-159">Variables of a reference type, `string` for example, are non-nullable.</span></span>  <span data-ttu-id="feb70-160">启用所有为 Null 性警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-160">All nullability warnings are enabled.</span></span>
- <span data-ttu-id="feb70-161">`warnings`：“禁用”可为空注释上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-161">`warnings`: The nullable annotation context is **disabled**.</span></span> <span data-ttu-id="feb70-162">“启用”可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-162">The nullable warning context is **enabled**.</span></span>
  - <span data-ttu-id="feb70-163">引用类型的变量是“无视”。</span><span class="sxs-lookup"><span data-stu-id="feb70-163">Variables of a reference type are oblivious.</span></span> <span data-ttu-id="feb70-164">启用所有为 Null 性警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-164">All nullability warnings are enabled.</span></span>
- <span data-ttu-id="feb70-165">`annotations`：“启用”可为空注释上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-165">`annotations`: The nullable annotation context is **enabled**.</span></span> <span data-ttu-id="feb70-166">“禁用”可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-166">The nullable warning context is **disabled**.</span></span>
  - <span data-ttu-id="feb70-167">引用类型的变量（例如字符串）不可为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-167">Variables of a reference type, string for example, are non-nullable.</span></span> <span data-ttu-id="feb70-168">禁用所有为 Null 性警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-168">All nullability warnings are disabled.</span></span>
- <span data-ttu-id="feb70-169">`disable`：“禁用”可为空注释上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-169">`disable`: The nullable annotation context is **disabled**.</span></span> <span data-ttu-id="feb70-170">“禁用”可为空警告上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-170">The nullable warning context is **disabled**.</span></span>
  - <span data-ttu-id="feb70-171">引用类型的变量是“无视”，就像早期版本的 C# 一样。</span><span class="sxs-lookup"><span data-stu-id="feb70-171">Variables of a reference type are oblivious, just like earlier versions of C#.</span></span> <span data-ttu-id="feb70-172">禁用所有为 Null 性警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-172">All nullability warnings are disabled.</span></span>

<span data-ttu-id="feb70-173">**示例**：</span><span class="sxs-lookup"><span data-stu-id="feb70-173">**Example**:</span></span>

```xml
<Nullable>enable</Nullable>
```

<span data-ttu-id="feb70-174">你还可以使用指令在项目的任何位置设置这些相同的上下文：</span><span class="sxs-lookup"><span data-stu-id="feb70-174">You can also use directives to set these same contexts anywhere in your project:</span></span>

- <span data-ttu-id="feb70-175">`#nullable enable`：将可为空注释上下文和可为空警告上下文设置为“已启用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-175">`#nullable enable`: Sets the nullable annotation context and nullable warning context to **enabled**.</span></span>
- <span data-ttu-id="feb70-176">`#nullable disable`：将可为空注释上下文和可为空警告上下文设置为“已禁用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-176">`#nullable disable`: Sets the nullable annotation context and nullable warning context to **disabled**.</span></span>
- <span data-ttu-id="feb70-177">`#nullable restore`：将可为空注释上下文和可为空警告上下文还原到项目设置。</span><span class="sxs-lookup"><span data-stu-id="feb70-177">`#nullable restore`: Restores the nullable annotation context and nullable warning context to the project settings.</span></span>
- <span data-ttu-id="feb70-178">`#nullable disable warnings`：将可为空警告上下文设置为“已禁用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-178">`#nullable disable warnings`: Set the nullable warning context to **disabled**.</span></span>
- <span data-ttu-id="feb70-179">`#nullable enable warnings`：将可为空警告上下文设置为“已启用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-179">`#nullable enable warnings`: Set the nullable warning context to **enabled**.</span></span>
- <span data-ttu-id="feb70-180">`#nullable restore warnings`：将可为空警告上下文还原到项目设置。</span><span class="sxs-lookup"><span data-stu-id="feb70-180">`#nullable restore warnings`: Restores the nullable warning context to the project settings.</span></span>
- <span data-ttu-id="feb70-181">`#nullable disable annotations`：将可为空注释上下文设置为“禁用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-181">`#nullable disable annotations`: Set the nullable annotation context to **disabled**.</span></span>
- <span data-ttu-id="feb70-182">`#nullable enable annotations`：将可为空注释上下文设置为“启用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-182">`#nullable enable annotations`: Set the nullable annotation context to **enabled**.</span></span>
- <span data-ttu-id="feb70-183">`#nullable restore annotations`：将注释警告上下文还原到项目设置。</span><span class="sxs-lookup"><span data-stu-id="feb70-183">`#nullable restore annotations`: Restores the annotation warning context to the project settings.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="feb70-184">全局可为空上下文不适用于生成的代码文件。</span><span class="sxs-lookup"><span data-stu-id="feb70-184">The global nullable context does not apply for generated code files.</span></span> <span data-ttu-id="feb70-185">在这两种策略下，都会针对标记为“已生成”的任何源文件禁用可为空上下文。</span><span class="sxs-lookup"><span data-stu-id="feb70-185">Under either strategy, the nullable context is *disabled* for any source file marked as generated.</span></span> <span data-ttu-id="feb70-186">这意味着生成的文件中的所有 API 都没有批注。</span><span class="sxs-lookup"><span data-stu-id="feb70-186">This means any APIs in generated files are not annotated.</span></span> <span data-ttu-id="feb70-187">可采用四种方法将文件标记为“已生成”：</span><span class="sxs-lookup"><span data-stu-id="feb70-187">There are four ways a file is marked as generated:</span></span>
>
> 1. <span data-ttu-id="feb70-188">在 .editorconfig 中，在应用于该文件的部分中指定 `generated_code = true`。</span><span class="sxs-lookup"><span data-stu-id="feb70-188">In the .editorconfig, specify `generated_code = true` in a section that applies to that file.</span></span>
> 1. <span data-ttu-id="feb70-189">将 `<auto-generated>` 或 `<auto-generated/>` 放在文件顶部的注释中。</span><span class="sxs-lookup"><span data-stu-id="feb70-189">Put `<auto-generated>` or `<auto-generated/>` in a comment at the top of the file.</span></span> <span data-ttu-id="feb70-190">它可以位于该注释中的任意行上，但注释块必须是该文件中的第一个元素。</span><span class="sxs-lookup"><span data-stu-id="feb70-190">It can be on any line in that comment, but the comment block must be the first element in the file.</span></span>
> 1. <span data-ttu-id="feb70-191">文件名以 TemporaryGeneratedFile_ 开头</span><span class="sxs-lookup"><span data-stu-id="feb70-191">Start the file name with *TemporaryGeneratedFile_*</span></span>
> 1. <span data-ttu-id="feb70-192">文件名用以 .designer.cs、.generated.cs、.g.cs 或 .g.i.cs 结尾   。</span><span class="sxs-lookup"><span data-stu-id="feb70-192">End the file name with *.designer.cs*, *.generated.cs*, *.g.cs*, or *.g.i.cs*.</span></span>
>
> <span data-ttu-id="feb70-193">生成器可以选择使用 [`#nullable`](language-reference/preprocessor-directives.md#nullable-context) 预处理器指令。</span><span class="sxs-lookup"><span data-stu-id="feb70-193">Generators can opt-in using the [`#nullable`](language-reference/preprocessor-directives.md#nullable-context) preprocessor directive.</span></span>

<span data-ttu-id="feb70-194">默认情况下，可为空注释和警告上下文处于禁用状态，包括新项目。</span><span class="sxs-lookup"><span data-stu-id="feb70-194">By default, nullable annotation and warning contexts are **disabled**, including new projects.</span></span> <span data-ttu-id="feb70-195">这意味着无需更改现有代码即可进行编译，并且不会生成任何新警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-195">That means that your existing code compiles without changes and without generating any new warnings.</span></span>

<span data-ttu-id="feb70-196">这些选项提供两种不同的策略来[更新现有代码库](nullable-migration-strategies.md)以使用可为 null 的引用类型。</span><span class="sxs-lookup"><span data-stu-id="feb70-196">These options provide two distinct strategies to [update an existing codebase](nullable-migration-strategies.md) to use nullable reference types.</span></span>

## <a name="nullable-annotation-context"></a><span data-ttu-id="feb70-197">可为空注释上下文</span><span class="sxs-lookup"><span data-stu-id="feb70-197">Nullable annotation context</span></span>

<span data-ttu-id="feb70-198">编译器在已禁用的可为空注释上下文中使用以下规则：</span><span class="sxs-lookup"><span data-stu-id="feb70-198">The compiler uses the following rules in a disabled nullable annotation context:</span></span>

- <span data-ttu-id="feb70-199">不能在已禁用的上下文中声明可为空引用。</span><span class="sxs-lookup"><span data-stu-id="feb70-199">You can't declare nullable references in a disabled context.</span></span>
- <span data-ttu-id="feb70-200">可以为所有引用变量分配 null 值。</span><span class="sxs-lookup"><span data-stu-id="feb70-200">All reference variables may be assigned a value of null.</span></span>
- <span data-ttu-id="feb70-201">取消引用引用类型的变量时不会生成警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-201">No warnings are generated when a variable of a reference type is dereferenced.</span></span>
- <span data-ttu-id="feb70-202">可能不会在禁用的上下文中使用 null 包容运算符。</span><span class="sxs-lookup"><span data-stu-id="feb70-202">The null-forgiving operator may not be used in a disabled context.</span></span>

<span data-ttu-id="feb70-203">该行为与以前版本的 C# 相同。</span><span class="sxs-lookup"><span data-stu-id="feb70-203">The behavior is the same as previous versions of C#.</span></span>

<span data-ttu-id="feb70-204">编译器在已启用的可为空注释上下文中使用以下规则：</span><span class="sxs-lookup"><span data-stu-id="feb70-204">The compiler uses the following rules in an enabled nullable annotation context:</span></span>

- <span data-ttu-id="feb70-205">引用类型的任何变量都是“不可为空引用”。</span><span class="sxs-lookup"><span data-stu-id="feb70-205">Any variable of a reference type is a **non-nullable reference**.</span></span>
- <span data-ttu-id="feb70-206">任何不可为空引用都可以安全地取消引用。</span><span class="sxs-lookup"><span data-stu-id="feb70-206">Any non-nullable reference may be dereferenced safely.</span></span>
- <span data-ttu-id="feb70-207">任何可为空引用类型（在变量声明中的类型之后由 `?` 标记）可为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-207">Any nullable reference type (noted by `?` after the type in the variable declaration) may be null.</span></span> <span data-ttu-id="feb70-208">静态分析确定在取消引用该值时是否已知该值不为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-208">Static analysis determines if the value is known to be non-null when it's dereferenced.</span></span> <span data-ttu-id="feb70-209">否则，编译器会发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-209">If not, the compiler warns you.</span></span>
- <span data-ttu-id="feb70-210">你可以使用 null 包容运算符声明可为空引用不为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-210">You can use the null-forgiving operator to declare that a nullable reference isn't null.</span></span>

<span data-ttu-id="feb70-211">在已启用的可为空注释上下文中，附加到引用类型的 `?` 字符声明“可为空引用类型”。</span><span class="sxs-lookup"><span data-stu-id="feb70-211">In an enabled nullable annotation context, the `?` character appended to a reference type declares a **nullable reference type**.</span></span> <span data-ttu-id="feb70-212">可将 NULL 包容运算符 `!` 附加到表达式以声明表达式不为 NULL。</span><span class="sxs-lookup"><span data-stu-id="feb70-212">The **null-forgiving operator** `!` may be appended to an expression to declare that the expression isn't null.</span></span>

## <a name="nullable-warning-context"></a><span data-ttu-id="feb70-213">可为空警告上下文</span><span class="sxs-lookup"><span data-stu-id="feb70-213">Nullable warning context</span></span>

<span data-ttu-id="feb70-214">可为空警告上下文与可为空注释上下文不同。</span><span class="sxs-lookup"><span data-stu-id="feb70-214">The nullable warning context is distinct from the nullable annotation context.</span></span> <span data-ttu-id="feb70-215">即使禁用新注释，也可以启用警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-215">Warnings can be enabled even when the new annotations are disabled.</span></span> <span data-ttu-id="feb70-216">编译器使用静态流分析来确定任何引用的“null 状态”。</span><span class="sxs-lookup"><span data-stu-id="feb70-216">The compiler uses static flow analysis to determine the **null state** of any reference.</span></span> <span data-ttu-id="feb70-217">当“可为空警告上下文”未被“禁用”时，null 状态为“非 null”或“可能为 null” 。</span><span class="sxs-lookup"><span data-stu-id="feb70-217">The null state is either **not null** or **maybe null** when the *nullable warning context* isn't **disabled**.</span></span> <span data-ttu-id="feb70-218">如果在编译器确定引用“可能为 null”时取消引用该引用，编译器会向你发出警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-218">If you dereference a reference when the compiler has determined it's **maybe null**, the compiler warns you.</span></span> <span data-ttu-id="feb70-219">除非编译器可以确定以下两个条件之一，否则引用的状态为“可能为 null”：</span><span class="sxs-lookup"><span data-stu-id="feb70-219">The state of a reference is **maybe null** unless the compiler can determine one of two conditions:</span></span>

1. <span data-ttu-id="feb70-220">该变量已明确分配给非 null 值。</span><span class="sxs-lookup"><span data-stu-id="feb70-220">The variable has been definitely assigned a non-null value.</span></span>
1. <span data-ttu-id="feb70-221">在取消引用之前，已检查变量或表达式是否为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-221">The variable or expression has been checked against null before de-referencing it.</span></span>

<span data-ttu-id="feb70-222">在可为 null 警告上下文中取消引用“可能为 null”的变量或表达式时，编译器会生成警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-222">The compiler generates warnings when you dereference a variable or expression that is **maybe null** in a nullable warning context.</span></span> <span data-ttu-id="feb70-223">此外，在将不可为 null 引用类型变量分配给已启用的可为空注释上下文中的可能为 null 变量或表达式时，编译器会生成警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-223">Furthermore, the compiler generates warnings when a nonnullable reference-type variable is assigned a **maybe null** variable or expression in an enabled nullable annotation context.</span></span>

## <a name="attributes-describe-apis"></a><span data-ttu-id="feb70-224">属性描述 API</span><span class="sxs-lookup"><span data-stu-id="feb70-224">Attributes describe APIs</span></span>

<span data-ttu-id="feb70-225">可以向 API 添加属性，以向编译器提供有关参数或返回值何时可以为 null 或不可为 null 的更多信息。</span><span class="sxs-lookup"><span data-stu-id="feb70-225">You add attributes to APIs that provide the compiler more information about when arguments or return values can or can't be null.</span></span> <span data-ttu-id="feb70-226">可在涉及[可为 null 属性](language-reference/attributes/nullable-analysis.md)的语言参考中的文章中了解有关这些属性的更多信息。</span><span class="sxs-lookup"><span data-stu-id="feb70-226">You can learn more about these attributes in our article in the language reference covering the [nullable attributes](language-reference/attributes/nullable-analysis.md).</span></span> <span data-ttu-id="feb70-227">这些属性将通过当前和即将发布的版本添加到 .NET 库中。</span><span class="sxs-lookup"><span data-stu-id="feb70-227">These attributes are being added to .NET libraries over current and upcoming releases.</span></span> <span data-ttu-id="feb70-228">首先更新最常用的 API。</span><span class="sxs-lookup"><span data-stu-id="feb70-228">The most commonly used APIs are being updated first.</span></span>

## <a name="known-pitfalls"></a><span data-ttu-id="feb70-229">已知缺陷</span><span class="sxs-lookup"><span data-stu-id="feb70-229">Known pitfalls</span></span>

<span data-ttu-id="feb70-230">包含引用类型的数组和结构是可为 null 的引用类型功能中的已知缺陷。</span><span class="sxs-lookup"><span data-stu-id="feb70-230">Arrays and structs that contain reference types are known pitfalls in nullable reference types feature.</span></span>

### <a name="structs"></a><span data-ttu-id="feb70-231">结构</span><span class="sxs-lookup"><span data-stu-id="feb70-231">Structs</span></span>

<span data-ttu-id="feb70-232">包含不可为 null 的引用类型的结构允许为其分配 `default`，而不会出现任何警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-232">A struct that contains non-nullable reference types allows assigning `default` for it without any warnings.</span></span> <span data-ttu-id="feb70-233">请考虑以下示例：</span><span class="sxs-lookup"><span data-stu-id="feb70-233">Consider the following example:</span></span>

```csharp
using System;

#nullable enable

public struct Student
{
    public string FirstName;
    public string? MiddleName;
    public string LastName;
}

public static class Program
{
    public static void PrintStudent(Student student)
    {
        Console.WriteLine($"First name: {student.FirstName.ToUpper()}");
        Console.WriteLine($"Middle name: {student.MiddleName.ToUpper()}");
        Console.WriteLine($"Last name: {student.LastName.ToUpper()}");
    }

    public static void Main() => PrintStudent(default);
}
```

<span data-ttu-id="feb70-234">在前面的示例中，不可为 null 的引用类型 `FirstName` 和 `LastName` 为 null 时，`PrintStudent(default)` 中未出现警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-234">In the preceding example, there is no warning in `PrintStudent(default)` while the non-nullable reference types `FirstName` and `LastName` are null.</span></span>

<span data-ttu-id="feb70-235">另一种较为常见的情况是处理泛型结构。</span><span class="sxs-lookup"><span data-stu-id="feb70-235">Another more common case is when you deal with generic structs.</span></span> <span data-ttu-id="feb70-236">请考虑以下示例：</span><span class="sxs-lookup"><span data-stu-id="feb70-236">Consider the following example:</span></span>

```csharp
#nullable enable

public struct Foo<T>
{
    public T Bar { get; set; }
}

public static class Program
{
    public static void Main()
    {
        string s = default(Foo<string>).Bar;
    }
}
```

<span data-ttu-id="feb70-237">在前面的示例中，属性 `Bar` 在运行时将为 `null`，并被分配给不可为 null 的字符串，而未出现任何警告。</span><span class="sxs-lookup"><span data-stu-id="feb70-237">In the preceding example, the property `Bar` is going to be `null` at runtime, and it's assigned to non-nullable string without any warnings.</span></span>

### <a name="arrays"></a><span data-ttu-id="feb70-238">数组</span><span class="sxs-lookup"><span data-stu-id="feb70-238">Arrays</span></span>

<span data-ttu-id="feb70-239">数组也是可为 null 的引用类型中的已知缺陷。</span><span class="sxs-lookup"><span data-stu-id="feb70-239">Arrays are also a known pitfall in nullable reference types.</span></span> <span data-ttu-id="feb70-240">请考虑以下示例，该示例不会生成任何警告：</span><span class="sxs-lookup"><span data-stu-id="feb70-240">Consider the following example which doesn't produce any warnings:</span></span>

```csharp
using System;

#nullable enable

public static class Program
{
    public static void Main()
    {
        string[] values = new string[10];
        string s = values[0];
        Console.WriteLine(s.ToUpper());
    }
}
```

<span data-ttu-id="feb70-241">在前面的示例中，数组的声明显示它保留不可为 null 的字符串，而其元素都已初始化为 null。</span><span class="sxs-lookup"><span data-stu-id="feb70-241">In the preceding example, the declaration of the array shows it holds non-nullable strings, while its elements are all initialized to null.</span></span> <span data-ttu-id="feb70-242">然后，为变量 `s` 分配一个 null 值（数组的第一个元素）。</span><span class="sxs-lookup"><span data-stu-id="feb70-242">Then, the variable `s` is assigned a null value (the first element of the array).</span></span> <span data-ttu-id="feb70-243">最后，取消引用变量 `s`，从而导致运行时异常。</span><span class="sxs-lookup"><span data-stu-id="feb70-243">Finally, the variable `s` is dereferenced causing a runtime exception.</span></span>

## <a name="see-also"></a><span data-ttu-id="feb70-244">请参阅</span><span class="sxs-lookup"><span data-stu-id="feb70-244">See also</span></span>

- [<span data-ttu-id="feb70-245">可为空引用类型规范草案</span><span class="sxs-lookup"><span data-stu-id="feb70-245">Draft nullable reference types specification</span></span>](~/_csharplang/proposals/csharp-9.0/nullable-reference-types-specification.md)
- [<span data-ttu-id="feb70-246">可为空引用教程简介</span><span class="sxs-lookup"><span data-stu-id="feb70-246">Intro to nullable references tutorial</span></span>](whats-new/tutorials/nullable-reference-types.md)
- [<span data-ttu-id="feb70-247">将现有代码库迁移到可为空引用</span><span class="sxs-lookup"><span data-stu-id="feb70-247">Migrate an existing codebase to nullable references</span></span>](whats-new/tutorials/upgrade-to-nullable-references.md)
- [<span data-ttu-id="feb70-248">Nullable（C# 编译器选项）</span><span class="sxs-lookup"><span data-stu-id="feb70-248">**Nullable** (C# Compiler option)</span></span>](language-reference/compiler-options/language.md#nullable)
