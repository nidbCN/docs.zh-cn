---
title: 升级代码库，以使用可为 null 引用类型
description: 选择升级代码库以使用可为 null 引用类型的最佳策略。
ms.technology: csharp-null-safety
ms.date: 07/31/2019
ms.openlocfilehash: fff9ce4d053cc2543b0148d70435bdc793ecccd7
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637152"
---
# <a name="update-libraries-to-use-nullable-reference-types-and-communicate-nullable-rules-to-callers"></a><span data-ttu-id="8251e-103">更新库以使用可为 null 引用类型，并将可为空规则传达给调用方</span><span class="sxs-lookup"><span data-stu-id="8251e-103">Update libraries to use nullable reference types and communicate nullable rules to callers</span></span>

<span data-ttu-id="8251e-104">添加[可为 null 引用类型](nullable-references.md)意味着可以声明是否允许或希望每个变量使用 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="8251e-104">The addition of [nullable reference types](nullable-references.md) means you can declare whether or not a `null` value is allowed or expected for every variable.</span></span> <span data-ttu-id="8251e-105">此外，还可应用多个属性（`AllowNull`、`DisallowNull`、`MaybeNull`、`NotNull`、`NotNullWhen`、`MaybeNullWhen` 和 `NotNullIfNotNull`），以完整描述参数和返回值的 null 状态。</span><span class="sxs-lookup"><span data-stu-id="8251e-105">In addition, you can apply a number of attributes: `AllowNull`, `DisallowNull`, `MaybeNull`, `NotNull`, `NotNullWhen`, `MaybeNullWhen`, and `NotNullIfNotNull` to completely describe the null states of argument and return values.</span></span> <span data-ttu-id="8251e-106">这可为你提供良好的代码编写体验。</span><span class="sxs-lookup"><span data-stu-id="8251e-106">That provides a great experience as you write code.</span></span> <span data-ttu-id="8251e-107">如果将不可为 null 的变量设置为 `null`，你会收到警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-107">You get warnings if a non-nullable variable might be set to `null`.</span></span> <span data-ttu-id="8251e-108">如果未对可为 null 的变量执行 null 检查就对其取消引用，你会收到警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-108">You get warnings if a nullable variable isn't null-checked before you dereference it.</span></span> <span data-ttu-id="8251e-109">更新库可能需要一些时间，但回报是值得的。</span><span class="sxs-lookup"><span data-stu-id="8251e-109">Updating your libraries can take time, but the payoffs are worth it.</span></span> <span data-ttu-id="8251e-110">向编译器提供的有关何时允许或禁止 `null` 值的信息越多，API 用户收到的警告就越详细。</span><span class="sxs-lookup"><span data-stu-id="8251e-110">The more information you provide to the compiler about *when* a `null` value is allowed or prohibited, the better warnings users of your API will get.</span></span> <span data-ttu-id="8251e-111">首先，让我们看一个熟悉的示例。</span><span class="sxs-lookup"><span data-stu-id="8251e-111">Let's start with a familiar example.</span></span> <span data-ttu-id="8251e-112">假设你的库具有以下用于检索资源字符串的 API：</span><span class="sxs-lookup"><span data-stu-id="8251e-112">Imagine your library has the following API to retrieve a resource string:</span></span>

```csharp
bool TryGetMessage(string key, out string message)
```

<span data-ttu-id="8251e-113">前面的示例遵循 .NET 中熟悉的 `Try*` 模式。</span><span class="sxs-lookup"><span data-stu-id="8251e-113">The preceding example follows the familiar `Try*` pattern in .NET.</span></span> <span data-ttu-id="8251e-114">此 API 有两个引用参数：`key` 和 `message` 参数。</span><span class="sxs-lookup"><span data-stu-id="8251e-114">There are two reference arguments for this API: the `key` and the `message` parameter.</span></span> <span data-ttu-id="8251e-115">此 API 具有与这些参数的是否为 null 相关的以下规则：</span><span class="sxs-lookup"><span data-stu-id="8251e-115">This API has the following rules relating to the nullness of these arguments:</span></span>

- <span data-ttu-id="8251e-116">调用方不应将 `null` 作为 `key` 的参数传递。</span><span class="sxs-lookup"><span data-stu-id="8251e-116">Callers shouldn't pass `null` as the argument for `key`.</span></span>
- <span data-ttu-id="8251e-117">调用方可以传递值为 `null` 的变量作为 `message` 的参数。</span><span class="sxs-lookup"><span data-stu-id="8251e-117">Callers can pass a variable whose value is `null` as the argument for `message`.</span></span>
- <span data-ttu-id="8251e-118">如果 `TryGetMessage` 方法返回 `true`，则 `message` 的值不为 null。</span><span class="sxs-lookup"><span data-stu-id="8251e-118">If the `TryGetMessage` method returns `true`, the value of `message` isn't null.</span></span> <span data-ttu-id="8251e-119">如果返回值是 `false,`，则 `message`（及其 null 状态）的值为 null。</span><span class="sxs-lookup"><span data-stu-id="8251e-119">If the return value is `false,` the value of `message` (and its null state) is null.</span></span>

<span data-ttu-id="8251e-120">`key` 的规则完全可以用变量类型表示：`key` 应是不可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-120">The rule for `key` can be completely expressed by the variable type: `key` should be a non-nullable reference type.</span></span> <span data-ttu-id="8251e-121">`message` 参数更复杂。</span><span class="sxs-lookup"><span data-stu-id="8251e-121">The `message` parameter is more complex.</span></span> <span data-ttu-id="8251e-122">它允许 `null` 作为参数，但保证成功时，`out` 参数不为 null。</span><span class="sxs-lookup"><span data-stu-id="8251e-122">It allows `null` as the argument, but guarantees that, on success, that `out` argument isn't null.</span></span> <span data-ttu-id="8251e-123">对于这些情况，需要使用更丰富的词汇来描述期望。</span><span class="sxs-lookup"><span data-stu-id="8251e-123">For these scenarios, you need a richer vocabulary to describe the expectations.</span></span>

<span data-ttu-id="8251e-124">更新库以使用可为 null 引用需要的不仅仅是在一些变量和类型名称上添加 `?`。</span><span class="sxs-lookup"><span data-stu-id="8251e-124">Updating your library for nullable references requires more than sprinkling `?` on some of the variables and type names.</span></span> <span data-ttu-id="8251e-125">前面的示例表明你还需要检查 API，并考虑对每个输入参数的预期。</span><span class="sxs-lookup"><span data-stu-id="8251e-125">The preceding example shows that you need to examine your APIs and consider your expectations for each input argument.</span></span> <span data-ttu-id="8251e-126">考虑对返回值的保证，以及方法返回时的任何 `out` 或 `ref` 参数。</span><span class="sxs-lookup"><span data-stu-id="8251e-126">Consider the guarantees for the return value, and any `out` or `ref` arguments upon the method's return.</span></span> <span data-ttu-id="8251e-127">然后，将这些规则传达给编译器，当调用方不遵守这些规则时，编译器将发出警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-127">Then communicate those rules to the compiler, and the compiler will provide warnings when callers don't abide by those rules.</span></span>

<span data-ttu-id="8251e-128">这项工作需要一些时间。</span><span class="sxs-lookup"><span data-stu-id="8251e-128">This work takes time.</span></span> <span data-ttu-id="8251e-129">首先从策略入手，将库或应用程序设置为可识别可为 null 引用类型，同时均衡其他需求。</span><span class="sxs-lookup"><span data-stu-id="8251e-129">Let's start with strategies to make your library or application nullable-aware, while balancing other requirements.</span></span> <span data-ttu-id="8251e-130">你将了解如何均衡正在进行的开发，以启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-130">You'll see how to balance ongoing development enabling nullable reference types.</span></span> <span data-ttu-id="8251e-131">你将了解泛型类型定义的相关挑战。</span><span class="sxs-lookup"><span data-stu-id="8251e-131">You'll learn challenges for generic type definitions.</span></span> <span data-ttu-id="8251e-132">你将了解如何应用属性来描述各 API 的前置条件和后置条件。</span><span class="sxs-lookup"><span data-stu-id="8251e-132">You'll learn to apply attributes to describe pre- and post-conditions on individual APIs.</span></span>

## <a name="choose-a-strategy-for-nullable-reference-types"></a><span data-ttu-id="8251e-133">选择用于可为 null 引用类型的策略</span><span class="sxs-lookup"><span data-stu-id="8251e-133">Choose a strategy for nullable reference types</span></span>

<span data-ttu-id="8251e-134">第一种选择是，默认应启用还是禁用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-134">The first choice is whether nullable reference types should be on or off by default.</span></span> <span data-ttu-id="8251e-135">有两种策略：</span><span class="sxs-lookup"><span data-stu-id="8251e-135">You have two strategies:</span></span>

- <span data-ttu-id="8251e-136">对整个项目启用可为 null 引用类型，而在尚未就绪的代码中禁用。</span><span class="sxs-lookup"><span data-stu-id="8251e-136">Enable nullable reference types for the entire project, and disable it in code that's not ready.</span></span>
- <span data-ttu-id="8251e-137">仅对带有可为 null 引用类型注释的代码启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-137">Only enable nullable reference types for code that's been annotated for nullable reference types.</span></span>

<span data-ttu-id="8251e-138">通过向库添加其他功能而将库更新为使用可为 null 引用类型时，第一种策略最有效。</span><span class="sxs-lookup"><span data-stu-id="8251e-138">The first strategy works best when you're adding other features to the library as you update it for nullable reference types.</span></span> <span data-ttu-id="8251e-139">所有新开发都可识别可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-139">All new development is nullable aware.</span></span> <span data-ttu-id="8251e-140">更新现有代码时，在这些类中启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-140">As you update existing code, you enable nullable reference types in those classes.</span></span>

<span data-ttu-id="8251e-141">按照第一种策略，须执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="8251e-141">Following this first strategy, you do the following steps:</span></span>

1. <span data-ttu-id="8251e-142">将 `<Nullable>enable</Nullable>` 元素添加到 .csproj 文件，对整个项目启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-142">Enable nullable reference types for the entire project by adding the `<Nullable>enable</Nullable>` element to your *csproj* files.</span></span>
1. <span data-ttu-id="8251e-143">将 `#nullable disable` pragma 添加到项目中的每个源文件。</span><span class="sxs-lookup"><span data-stu-id="8251e-143">Add the `#nullable disable` pragma to every source file in your project.</span></span>
1. <span data-ttu-id="8251e-144">处理每个文件时，请删除 pragma 并解决所有警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-144">As you work on each file, remove the pragma and address any warnings.</span></span>

<span data-ttu-id="8251e-145">第一种策略在将 pragma 添加到每个文件中时，需要的前期工作更多。</span><span class="sxs-lookup"><span data-stu-id="8251e-145">This first strategy has more up-front work to add the pragma to every file.</span></span> <span data-ttu-id="8251e-146">优点在于，添加到项目的每个新代码文件都支持可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-146">The advantage is that every new code file added to the project will be nullable enabled.</span></span> <span data-ttu-id="8251e-147">任何新的代码都可识别可为 null 引用类型；只须更新现有代码。</span><span class="sxs-lookup"><span data-stu-id="8251e-147">Any new work will be nullable aware; only existing code must be updated.</span></span>

<span data-ttu-id="8251e-148">如果库稳定并且开发重点是采用可为 null 引用类型，则第二种策略效果更好。</span><span class="sxs-lookup"><span data-stu-id="8251e-148">The second strategy works better if the library is stable, and the main focus of the development is to adopt nullable reference types.</span></span> <span data-ttu-id="8251e-149">在你注释 API 时，将启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-149">You turn on nullable reference types as you annotate APIs.</span></span> <span data-ttu-id="8251e-150">完成后，将对整个项目启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-150">When you've finished, you enable nullable reference types for the entire project.</span></span>

<span data-ttu-id="8251e-151">按照第二种策略，须执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="8251e-151">Following this second strategy you do the following steps:</span></span>

1. <span data-ttu-id="8251e-152">将 `#nullable enable` pragma 添加到你希望将其设置为可识别可为 null 引用类型的文件。</span><span class="sxs-lookup"><span data-stu-id="8251e-152">Add the `#nullable enable` pragma to the file you want to make nullable aware.</span></span>
1. <span data-ttu-id="8251e-153">解决任何警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-153">Address any warnings.</span></span>
1. <span data-ttu-id="8251e-154">继续执行前两个步骤，直到将整个库都设置为可识别可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-154">Continue these first two steps until you've made the entire library nullable aware.</span></span>
1. <span data-ttu-id="8251e-155">将 `<Nullable>enable</Nullable>` 元素添加到 csproj 文件，对整个项目启用可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-155">Enable nullable types for the entire project by adding the `<Nullable>enable</Nullable>` element to your *csproj* files.</span></span>
1. <span data-ttu-id="8251e-156">当不再需要可为 null 引用类型时，删除 `#nullable enable` pragma。</span><span class="sxs-lookup"><span data-stu-id="8251e-156">Remove the `#nullable enable` pragmas, as they're no longer needed.</span></span>

<span data-ttu-id="8251e-157">第二种策略所需的前期工作较少。</span><span class="sxs-lookup"><span data-stu-id="8251e-157">This second strategy has less work up-front.</span></span> <span data-ttu-id="8251e-158">缺点在于，创建新文件时的第一项任务是添加 pragma，并将文件设置为可识别可为 null 引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-158">The tradeoff is that the first task when you create a new file is to add the pragma and make it nullable aware.</span></span> <span data-ttu-id="8251e-159">如果团队中任何开发人员忘记了这一点，则需要将所有代码设置为可识别可为 null 引用类型，这样，新代码现将变为积压工作 (backlog)。</span><span class="sxs-lookup"><span data-stu-id="8251e-159">If any developers on your team forget, that new code is now in the backlog of work to make all code nullable aware.</span></span>

<span data-ttu-id="8251e-160">选择哪种策略取决于项目中正在进行多少项活动开发。</span><span class="sxs-lookup"><span data-stu-id="8251e-160">Which of these strategies you pick depends on how much active development is taking place in your project.</span></span> <span data-ttu-id="8251e-161">项目越成熟稳定，第二种策略的效果越好。</span><span class="sxs-lookup"><span data-stu-id="8251e-161">The more mature and stable your project, the better the second strategy.</span></span> <span data-ttu-id="8251e-162">开发的功能越多，第一种策略的效果越好。</span><span class="sxs-lookup"><span data-stu-id="8251e-162">The more features being developed, the better the first strategy.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8251e-163">全局可为空上下文不适用于生成的代码文件。</span><span class="sxs-lookup"><span data-stu-id="8251e-163">The global nullable context does not apply for generated code files.</span></span> <span data-ttu-id="8251e-164">在这两种策略下，都会针对标记为“已生成”的任何源文件禁用可为空上下文。</span><span class="sxs-lookup"><span data-stu-id="8251e-164">Under either strategy, the nullable context is *disabled* for any source file marked as generated.</span></span> <span data-ttu-id="8251e-165">这意味着生成的文件中的所有 API 都没有批注。</span><span class="sxs-lookup"><span data-stu-id="8251e-165">This means any APIs in generated files are not annotated.</span></span> <span data-ttu-id="8251e-166">可采用四种方法将文件标记为“已生成”：</span><span class="sxs-lookup"><span data-stu-id="8251e-166">There are four ways a file is marked as generated:</span></span>
>
> 1. <span data-ttu-id="8251e-167">在 .editorconfig 中，在应用于该文件的部分中指定 `generated_code = true`。</span><span class="sxs-lookup"><span data-stu-id="8251e-167">In the .editorconfig, specify `generated_code = true` in a section that applies to that file.</span></span>
> 1. <span data-ttu-id="8251e-168">将 `<auto-generated>` 或 `<auto-generated/>` 放在文件顶部的注释中。</span><span class="sxs-lookup"><span data-stu-id="8251e-168">Put `<auto-generated>` or `<auto-generated/>` in a comment at the top of the file.</span></span> <span data-ttu-id="8251e-169">它可以位于该注释中的任意行上，但注释块必须是该文件中的第一个元素。</span><span class="sxs-lookup"><span data-stu-id="8251e-169">It can be on any line in that comment, but the comment block must be the first element in the file.</span></span>
> 1. <span data-ttu-id="8251e-170">文件名以 TemporaryGeneratedFile_ 开头</span><span class="sxs-lookup"><span data-stu-id="8251e-170">Start the file name with *TemporaryGeneratedFile_*</span></span>
> 1. <span data-ttu-id="8251e-171">文件名用以 .designer.cs、.generated.cs、.g.cs 或 .g.i.cs 结尾   。</span><span class="sxs-lookup"><span data-stu-id="8251e-171">End the file name with *.designer.cs*, *.generated.cs*, *.g.cs*, or *.g.i.cs*.</span></span>
>
> <span data-ttu-id="8251e-172">生成器可以选择使用 [`#nullable`](language-reference/preprocessor-directives.md#nullable-context) 预处理器指令。</span><span class="sxs-lookup"><span data-stu-id="8251e-172">Generators can opt-in using the [`#nullable`](language-reference/preprocessor-directives.md#nullable-context) preprocessor directive.</span></span>

## <a name="should-nullable-warnings-introduce-breaking-changes"></a><span data-ttu-id="8251e-173">是否应为可为 null 警告引入中断性变更？</span><span class="sxs-lookup"><span data-stu-id="8251e-173">Should nullable warnings introduce breaking changes?</span></span>

<span data-ttu-id="8251e-174">启用可为 null 引用类型之前，变量被视作“忽略可为 null”。</span><span class="sxs-lookup"><span data-stu-id="8251e-174">Before you enable nullable reference types, variables are considered *nullable oblivious*.</span></span> <span data-ttu-id="8251e-175">启用可为 null 引用类型后，这些变量变为“不可为 null”。</span><span class="sxs-lookup"><span data-stu-id="8251e-175">Once you enable nullable reference types, all those variables are *non-nullable*.</span></span> <span data-ttu-id="8251e-176">如果这些变量未初始化为非 null 值，编译器将发出警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-176">The compiler will issue warnings if those variables aren't initialized to non-null values.</span></span>

<span data-ttu-id="8251e-177">另一个可能的警告来源是尚未初始化的返回值。</span><span class="sxs-lookup"><span data-stu-id="8251e-177">Another likely source of warnings is return values when the value hasn't been initialized.</span></span>

<span data-ttu-id="8251e-178">解决编译器警告的第一步是在参数和返回类型上使用 `?` 注释，以指示参数或返回值何时可能为 null。</span><span class="sxs-lookup"><span data-stu-id="8251e-178">The first step in addressing the compiler warnings is to use `?` annotations on parameter and return types to indicate when arguments or return values may be null.</span></span> <span data-ttu-id="8251e-179">当引用变量不得为 null 时，最初的声明正确。</span><span class="sxs-lookup"><span data-stu-id="8251e-179">When reference variables must not be null, the original declaration is correct.</span></span> <span data-ttu-id="8251e-180">执行此任务时，目标不只是修复警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-180">As you do this task, your goal isn't just to fix warnings.</span></span> <span data-ttu-id="8251e-181">更重要的目标是让编译器了解潜在 null 值的意图。</span><span class="sxs-lookup"><span data-stu-id="8251e-181">The more important goal is to make the compiler understand your intent for potential null values.</span></span> <span data-ttu-id="8251e-182">检查警告时，你将面临关于库的下一个重要决定。</span><span class="sxs-lookup"><span data-stu-id="8251e-182">As you examine the warnings, you reach your next major decision for your library.</span></span> <span data-ttu-id="8251e-183">是否考虑修改 API 签名，以便更清晰地传达设计意图？</span><span class="sxs-lookup"><span data-stu-id="8251e-183">Do you want to consider modifying API signatures to more clearly communicate your design intent?</span></span> <span data-ttu-id="8251e-184">对于之前检查的 `TryGetMessage` 方法，更好的 API 签名可能是：</span><span class="sxs-lookup"><span data-stu-id="8251e-184">A better API signature for the `TryGetMessage` method examined earlier could be:</span></span>

```csharp
string? TryGetMessage(string key);
```

<span data-ttu-id="8251e-185">返回值指示成功或失败，如果找到值，将包含该值。</span><span class="sxs-lookup"><span data-stu-id="8251e-185">The return value indicates success or failure, and carries the value if the value was found.</span></span> <span data-ttu-id="8251e-186">在许多情况下，更改 API 签名可改善它们传递 null 值的方式。</span><span class="sxs-lookup"><span data-stu-id="8251e-186">In many cases, changing API signatures can improve how they communicate null values.</span></span>

<span data-ttu-id="8251e-187">但对于公共库或用户群较大的库，你可能不想引入任何 API 签名更改。</span><span class="sxs-lookup"><span data-stu-id="8251e-187">However, for public libraries, or libraries with large user bases, you may prefer not introducing any API signature changes.</span></span> <span data-ttu-id="8251e-188">对于这类情况和其他常见情形，可以应用属性来更清楚地定义参数或返回值何时可能为 `null`。</span><span class="sxs-lookup"><span data-stu-id="8251e-188">For those cases, and other common patterns, you can apply attributes to more clearly define when an argument or return value may be `null`.</span></span> <span data-ttu-id="8251e-189">无论是否考虑更改 API 的图面，你都可能会发现，仅使用类型注释不足以描述参数或返回值的 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="8251e-189">Whether or not you consider changing the surface of your API, you'll likely find that type annotations alone aren't sufficient for describing `null` values for arguments or return values.</span></span> <span data-ttu-id="8251e-190">在这些情况下，可应用属性来更清晰地描述 API。</span><span class="sxs-lookup"><span data-stu-id="8251e-190">In those instances, you can apply attributes to more clearly describe an API.</span></span>

## <a name="attributes-extend-type-annotations"></a><span data-ttu-id="8251e-191">特性扩展类型注释</span><span class="sxs-lookup"><span data-stu-id="8251e-191">Attributes extend type annotations</span></span>

<span data-ttu-id="8251e-192">为表示有关变量的 null 状态的附加信息，已添加多个特性。</span><span class="sxs-lookup"><span data-stu-id="8251e-192">Several attributes have been added to express additional information about the null state of variables.</span></span> <span data-ttu-id="8251e-193">在 C# 8 引入可为 null 的引用类型之前编写的所有代码都是忽略 null 的  。</span><span class="sxs-lookup"><span data-stu-id="8251e-193">All code you wrote before C# 8 introduced nullable reference types was *null oblivious*.</span></span> <span data-ttu-id="8251e-194">这意味着任何引用类型变量都可以为 null，但不需要进行 null 检查。</span><span class="sxs-lookup"><span data-stu-id="8251e-194">That means any reference type variable may be null, but null checks aren't required.</span></span> <span data-ttu-id="8251e-195">代码“可识别为 null”后，这些规则就会改变  。</span><span class="sxs-lookup"><span data-stu-id="8251e-195">Once your code is *nullable aware*, those rules change.</span></span> <span data-ttu-id="8251e-196">引用类型不应该是 `null` 值，在取消引用之前，必须对照 `null` 检查可为 null 的引用类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-196">Reference types should never be the `null` value, and nullable reference types must be checked against `null` before being dereferenced.</span></span>

<span data-ttu-id="8251e-197">API 的规则可能更复杂，正如你在 `TryGetValue` API 方案中看到的那样。</span><span class="sxs-lookup"><span data-stu-id="8251e-197">The rules for your APIs are likely more complicated, as you saw with the `TryGetValue` API scenario.</span></span> <span data-ttu-id="8251e-198">许多 API 对于变量何时可以或不可以为 `null` 有更复杂的规则。</span><span class="sxs-lookup"><span data-stu-id="8251e-198">Many of your APIs have more complex rules for when variables can or can't be `null`.</span></span> <span data-ttu-id="8251e-199">在这些情况下，可使用属性来表示这些规则。</span><span class="sxs-lookup"><span data-stu-id="8251e-199">In these cases, you'll use attributes to express those rules.</span></span> <span data-ttu-id="8251e-200">你可以在[影响可为 null 分析的属性](./language-reference/attributes/nullable-analysis.md)一文中找到描述 API 语义的属性。</span><span class="sxs-lookup"><span data-stu-id="8251e-200">The attributes that describe the semantics of your API are found in the article on [Attributes that impact nullable analysis](./language-reference/attributes/nullable-analysis.md).</span></span>

## <a name="generic-definitions-and-nullability"></a><span data-ttu-id="8251e-201">泛型定义和可为 null 性</span><span class="sxs-lookup"><span data-stu-id="8251e-201">Generic definitions and nullability</span></span>

<span data-ttu-id="8251e-202">正确传达泛型类型和泛型方法的 null 状态时，必须格外小心。</span><span class="sxs-lookup"><span data-stu-id="8251e-202">Correctly communicating the null state of generic types and generic methods requires special care.</span></span> <span data-ttu-id="8251e-203">还需要额外注意，可为 null 值类型和可为 null 引用类型在本质上有所不同。</span><span class="sxs-lookup"><span data-stu-id="8251e-203">The extra care stems from the fact that a nullable value type and a nullable reference type are fundamentally different.</span></span> <span data-ttu-id="8251e-204">`int?` 是 `Nullable<int>` 的同义词，而 `string?` 是带有编译器添加的属性的 `string`。</span><span class="sxs-lookup"><span data-stu-id="8251e-204">An `int?` is a synonym for `Nullable<int>`, whereas `string?` is `string` with an attribute added by the compiler.</span></span> <span data-ttu-id="8251e-205">因此，如果不知道 `T` 是 `class` 还是 `struct`，编译器就无法为 `T?` 生成正确的代码。</span><span class="sxs-lookup"><span data-stu-id="8251e-205">The result is that the compiler can't generate correct code for `T?` without knowing if `T` is a `class` or a `struct`.</span></span>

<span data-ttu-id="8251e-206">这一事实不意味着不能使用可为 null 类型（值类型或引用类型）作为封闭式泛型类型的类型参数。</span><span class="sxs-lookup"><span data-stu-id="8251e-206">This fact doesn't mean you can't use a nullable type (either value type or reference type) as the type argument for a closed generic type.</span></span> <span data-ttu-id="8251e-207">`List<string?>` 和 `List<int?>` 都是 `List<T>` 的有效实例化。</span><span class="sxs-lookup"><span data-stu-id="8251e-207">Both `List<string?>` and `List<int?>` are valid instantiations of `List<T>`.</span></span>

<span data-ttu-id="8251e-208">它的意思是，如果没有约束，就无法在泛型类或方法声明中使用 `T?`。</span><span class="sxs-lookup"><span data-stu-id="8251e-208">What it does mean is that you can't use `T?` in a generic class or method declaration without constraints.</span></span> <span data-ttu-id="8251e-209">例如，<xref:System.Linq.Enumerable.FirstOrDefault%60%601(System.Collections.Generic.IEnumerable%7B%60%600%7D)?displayProperty=nameWithType> 将不会被更改为返回 `T?`。</span><span class="sxs-lookup"><span data-stu-id="8251e-209">For example, <xref:System.Linq.Enumerable.FirstOrDefault%60%601(System.Collections.Generic.IEnumerable%7B%60%600%7D)?displayProperty=nameWithType> won't be changed to return `T?`.</span></span> <span data-ttu-id="8251e-210">你可以通过添加 `struct` 或 `class` 约束来克服此限制。</span><span class="sxs-lookup"><span data-stu-id="8251e-210">You can overcome this limitation by adding either the `struct` or `class` constraint.</span></span> <span data-ttu-id="8251e-211">使用上述任一约束，编译器都知道如何为 `T` 和 `T?` 生成代码。</span><span class="sxs-lookup"><span data-stu-id="8251e-211">With either of those constraints, the compiler knows how to generate code for both `T` and `T?`.</span></span>

<span data-ttu-id="8251e-212">你可能想要将用于某一泛型类型参数的类型限制为不可为 null 类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-212">You may want to restrict the types used for a generic type argument to be non-nullable types.</span></span> <span data-ttu-id="8251e-213">通过在该类型参数上添加 `notnull` 约束即可实现此目的。</span><span class="sxs-lookup"><span data-stu-id="8251e-213">You can do that by adding the `notnull` constraint on that type argument.</span></span> <span data-ttu-id="8251e-214">应用该约束后，类型参数将不得为可为 null 类型。</span><span class="sxs-lookup"><span data-stu-id="8251e-214">When that constraint is applied, the type argument must not be a nullable type.</span></span>

## <a name="late-initialized-properties-data-transfer-objects-and-nullability"></a><span data-ttu-id="8251e-215">延迟初始化属性、数据传输对象和可为 null 性</span><span class="sxs-lookup"><span data-stu-id="8251e-215">Late-initialized properties, Data Transfer Objects, and nullability</span></span>

<span data-ttu-id="8251e-216">指示延迟初始化属性（即在构造后设置）的可为 null 性时，可能需要格外注意，以确保类继续正确地表达最初的设计意图。</span><span class="sxs-lookup"><span data-stu-id="8251e-216">Indicating the nullability of properties that are late-initialized, meaning set after construction, may require special consideration to ensure that your class continues to correctly express the original design intent.</span></span>

<span data-ttu-id="8251e-217">包含延迟初始化属性的类型（例如数据传输对象 (DTO)）通常由外部库进行实例化，例如数据库 ORM（对象关系映射器）、反序列化程序或其他自动填充来自其他源的属性的组件。</span><span class="sxs-lookup"><span data-stu-id="8251e-217">Types that contain late-initialized properties, such as Data Transfer Objects (DTOs), are often instantiated by an external library, like a database ORM (Object Relational Mapper), a deserializer, or some other component that automatically populates properties from another source.</span></span>

<span data-ttu-id="8251e-218">在启用可为 null 引用类型之前，请考虑以下 DTO 类（代表学生）：</span><span class="sxs-lookup"><span data-stu-id="8251e-218">Consider the following DTO class, prior to enabling nullable reference types, that represents a student:</span></span>

```csharp
class Student
{
    [Required]
    public string FirstName { get; set; }

    [Required]
    public string LastName { get; set; }

    public string VehicleRegistration { get; set; }
}
```

<span data-ttu-id="8251e-219">设计意图（在此例中由 `Required` 属性指示）表明，在此系统中 `FirstName` 和 `LastName` 属性是必需属性，因此不可为 null。</span><span class="sxs-lookup"><span data-stu-id="8251e-219">The design intent (indicated in this case by the `Required` attribute) suggests that in this system, the `FirstName` and `LastName` properties are **mandatory**, and therefore not null.</span></span>

<span data-ttu-id="8251e-220">`VehicleRegistration` 属性不是必需属性，因此可能为 null。</span><span class="sxs-lookup"><span data-stu-id="8251e-220">The `VehicleRegistration` property is **not mandatory**, so may be null.</span></span>

<span data-ttu-id="8251e-221">启用可为 null 引用类型时，需要指出 DTO 上哪些属性可为 null，这应与最初的意图一致：</span><span class="sxs-lookup"><span data-stu-id="8251e-221">When you enable nullable reference types, you want to indicate which properties on your DTO may be nullable, consistent with your original intent:</span></span>

```csharp
class Student
{
    [Required]
    public string FirstName { get; set; }

    [Required]
    public string LastName { get; set; }

    public string? VehicleRegistration { get; set; }
}
```

<span data-ttu-id="8251e-222">对于此 DTO，可能为 null 的属性只有 ``VehicleRegistration``。</span><span class="sxs-lookup"><span data-stu-id="8251e-222">For this DTO, the only property that may be null is ``VehicleRegistration``.</span></span>

<span data-ttu-id="8251e-223">但是，编译器会针对 `FirstName` 和 `LastName` 发出 `CS8618` 警告，指示不可为 null 属性未进行初始化。</span><span class="sxs-lookup"><span data-stu-id="8251e-223">However, the compiler raises `CS8618` warnings for both `FirstName` and `LastName`, indicating the non-nullable properties are uninitialized.</span></span>

<span data-ttu-id="8251e-224">你可以通过三个方法在保持最初意图的同时解决编译器警告。</span><span class="sxs-lookup"><span data-stu-id="8251e-224">There are three options available to you that resolve the compiler warnings in a way that maintains the original intent.</span></span> <span data-ttu-id="8251e-225">这些方法均有效；你应选择最适合自己编码风格和设计要求的方法。</span><span class="sxs-lookup"><span data-stu-id="8251e-225">Any of these options are valid; you should choose the one that best suits your coding style and design requirements.</span></span>

### <a name="initialize-in-the-constructor"></a><span data-ttu-id="8251e-226">在构造函数中进行初始化</span><span class="sxs-lookup"><span data-stu-id="8251e-226">Initialize in the constructor</span></span>

<span data-ttu-id="8251e-227">解决未初始化警告的理想方法是在构造函数中初始化相应属性：</span><span class="sxs-lookup"><span data-stu-id="8251e-227">The ideal way to resolve the uninitialized warnings is to initialize the properties in the constructor:</span></span>

```csharp
class Student
{
    public Student(string firstName, string lastName)
    {
        FirstName = firstName;
        LastName = lastName;
    }

    [Required]
    public string FirstName { get; set; }

    [Required]
    public string LastName { get; set; }

    public string? VehicleRegistration { get; set; }
}
```

<span data-ttu-id="8251e-228">仅当用于实例化类的库支持在构造函数中传递参数时，此方法才有效。</span><span class="sxs-lookup"><span data-stu-id="8251e-228">This approach only works if the library that you use to instantiate the class supports passing parameters in the constructor.</span></span>

<span data-ttu-id="8251e-229">库可能支持在构造函数中传递某些属性，但不是全部属性。</span><span class="sxs-lookup"><span data-stu-id="8251e-229">A library may support passing *some* properties in the constructor, but not all.</span></span> <span data-ttu-id="8251e-230">例如，EF Core 对普通列属性支持[构造函数绑定](/ef/core/modeling/constructors)，但对导航属性却不支持。</span><span class="sxs-lookup"><span data-stu-id="8251e-230">For example, EF Core supports [constructor binding](/ef/core/modeling/constructors) for normal column properties, but not navigation properties.</span></span>

<span data-ttu-id="8251e-231">查看有关实例化类的库的文档，了解库对构造函数绑定的支持范围。</span><span class="sxs-lookup"><span data-stu-id="8251e-231">Check the documentation on the library that instantiates your class, to understand the extent to which it supports constructor binding.</span></span>

### <a name="property-with-nullable-backing-field"></a><span data-ttu-id="8251e-232">使用带可为 null 支持字段的属性</span><span class="sxs-lookup"><span data-stu-id="8251e-232">Property with nullable backing field</span></span>

<span data-ttu-id="8251e-233">如果构造函数绑定不起作用，处理此问题的一种方法是使用带可为 null 支持字段的不可为 null 属性：</span><span class="sxs-lookup"><span data-stu-id="8251e-233">If constructor binding won't work for you, one way to deal with this problem is to have a non-nullable property with a nullable backing field:</span></span>

```csharp
private string? _firstName;

[Required]
public string FirstName
{
    set => _firstName = value;
    get => _firstName
           ?? throw new InvalidOperationException("Uninitialized " + nameof(FirstName))
}
```

<span data-ttu-id="8251e-234">在这种情况下，如果在初始化 `FirstName` 属性之前访问该属性，则代码将引发 `InvalidOperationException`，因为错误使用了 API 协定。</span><span class="sxs-lookup"><span data-stu-id="8251e-234">In this scenario, if the `FirstName` property is accessed before it has been initialized, then the code throws an `InvalidOperationException`, because the API contract has been used incorrectly.</span></span>

<span data-ttu-id="8251e-235">注意，使用支持字段时，某些库可能有特殊的注意事项。</span><span class="sxs-lookup"><span data-stu-id="8251e-235">Consider that some libraries may have special considerations when using backing fields.</span></span> <span data-ttu-id="8251e-236">例如，可能需要配置 EF Core 才能正确使用[支持字段](/ef/core/modeling/backing-field)。</span><span class="sxs-lookup"><span data-stu-id="8251e-236">For example, EF Core may need to be configured to use [backing fields](/ef/core/modeling/backing-field) correctly.</span></span>

### <a name="initialize-the-property-to-null"></a><span data-ttu-id="8251e-237">将属性初始化为 null</span><span class="sxs-lookup"><span data-stu-id="8251e-237">Initialize the property to null</span></span>

<span data-ttu-id="8251e-238">作为使用可为 null 支持字段的一种更简洁替代方法，或者在实例化类的库不适合该方法的情况下，可以在 null 包容运算符 (`!`) 的帮助下将属性直接初始化为 `null`：</span><span class="sxs-lookup"><span data-stu-id="8251e-238">As a terser alternative to using a nullable backing field, or if the library that instantiates your class isn't compatible with that approach, you can initialize the property to `null` directly, with the help of the null-forgiving operator (`!`):</span></span>

```csharp
[Required]
public string FirstName { get; set; } = null!;

[Required]
public string LastName { get; set; } = null!;

public string? VehicleRegistration { get; set; }
```

<span data-ttu-id="8251e-239">如果还未正确初始化属性就对其进行访问，那么在运行时绝不会看到实际的 null 值（除非编程 bug 导致出现此结果）。</span><span class="sxs-lookup"><span data-stu-id="8251e-239">You'll never observe an actual null value at runtime except as a result of a programming bug, by accessing the property before it has been properly initialized.</span></span>

## <a name="see-also"></a><span data-ttu-id="8251e-240">另请参阅</span><span class="sxs-lookup"><span data-stu-id="8251e-240">See also</span></span>

- [<span data-ttu-id="8251e-241">将现有代码库迁移到可为空引用</span><span class="sxs-lookup"><span data-stu-id="8251e-241">Migrate an existing codebase to nullable references</span></span>](whats-new/tutorials/upgrade-to-nullable-references.md)
- [<span data-ttu-id="8251e-242">在 EF Core 中使用可为 null 引用类型</span><span class="sxs-lookup"><span data-stu-id="8251e-242">Working with Nullable Reference Types in EF Core</span></span>](/ef/core/miscellaneous/nullable-reference-types)
