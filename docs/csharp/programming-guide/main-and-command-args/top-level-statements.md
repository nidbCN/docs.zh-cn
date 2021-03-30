---
title: 顶级语句 - C# 编程指南
description: 了解 C# 9 及更高版本中的顶级语句。
ms.date: 03/08/2021
helpviewer_keywords:
- C# language, top-level statements
- C# language, Main method
ms.openlocfilehash: 31a9d3bba302823015058d59ca79da45754b761f
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881049"
---
# <a name="top-level-statements-c-programming-guide"></a><span data-ttu-id="28cc8-103">顶级语句（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="28cc8-103">Top-level statements (C# Programming Guide)</span></span>

<span data-ttu-id="28cc8-104">从 C# 9 开始，无需在控制台应用程序项目中显式包含 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="28cc8-104">Starting in C# 9, you don't have to explicitly include a `Main` method in a console application project.</span></span> <span data-ttu-id="28cc8-105">相反，可以使用顶级语句功能最大程度地减少必须编写的代码。</span><span class="sxs-lookup"><span data-stu-id="28cc8-105">Instead, you can use the *top-level statements* feature to minimize the code you have to write.</span></span> <span data-ttu-id="28cc8-106">在这种情况下，编译器将为应用程序生成类和 `Main` 方法入口点。</span><span class="sxs-lookup"><span data-stu-id="28cc8-106">In this case, the compiler generates a class and `Main` method entry point for the application.</span></span>

<span data-ttu-id="28cc8-107">下面的 Program.cs 文件是 C# 9 中一个完整的 C# 程序：</span><span class="sxs-lookup"><span data-stu-id="28cc8-107">Here's a *Program.cs* file that is a complete C# program in C# 9:</span></span>

:::code language="csharp" source="snippets/top-level-statements-1/Program.cs":::

<span data-ttu-id="28cc8-108">借助顶级语句，可以为小实用程序（如 Azure Functions 和 GitHub Actions）编写简单的程序。</span><span class="sxs-lookup"><span data-stu-id="28cc8-108">Top-level statements let you write simple programs for small utilities such as Azure Functions and GitHub Actions.</span></span> <span data-ttu-id="28cc8-109">它们还使初次接触 C# 的程序员能够更轻松地开始学习和编写代码。</span><span class="sxs-lookup"><span data-stu-id="28cc8-109">They also make it simpler for new C# programmers to get started learning and writing code.</span></span>

<span data-ttu-id="28cc8-110">以下各节介绍了可对顶级语句执行和不能执行的操作的规则。</span><span class="sxs-lookup"><span data-stu-id="28cc8-110">The following sections explain the rules on what you can and can't do with top-level statements.</span></span>

## <a name="only-one-top-level-file"></a><span data-ttu-id="28cc8-111">仅能有一个顶级文件</span><span class="sxs-lookup"><span data-stu-id="28cc8-111">Only one top-level file</span></span>

<span data-ttu-id="28cc8-112">一个应用程序必须只能有一个入口点，因此一个项目只能有一个包含顶级语句的文件。</span><span class="sxs-lookup"><span data-stu-id="28cc8-112">An application must have only one entry point, so a project can have only one file with top-level statements.</span></span> <span data-ttu-id="28cc8-113">在项目中的多个文件中放置顶级语句会导致以下编译器错误：</span><span class="sxs-lookup"><span data-stu-id="28cc8-113">Putting top-level statements in more than one file in a project results in the following compiler error:</span></span>

> <span data-ttu-id="28cc8-114">CS8802：只有一个编译单元可具有顶级语句。</span><span class="sxs-lookup"><span data-stu-id="28cc8-114">CS8802 Only one compilation unit can have top-level statements.</span></span>

<span data-ttu-id="28cc8-115">一个项目可具有任意数量的其他源代码文件，这些文件不包含顶级语句。</span><span class="sxs-lookup"><span data-stu-id="28cc8-115">A project can have any number of additional source code files that don't have top-level statements.</span></span>

## <a name="no-other-entry-points"></a><span data-ttu-id="28cc8-116">没有其他入口点</span><span class="sxs-lookup"><span data-stu-id="28cc8-116">No other entry points</span></span>

<span data-ttu-id="28cc8-117">可以显式编写 `Main` 方法，但它不能作为入口点。</span><span class="sxs-lookup"><span data-stu-id="28cc8-117">You can write a `Main` method explicitly, but it can't function as an entry point.</span></span> <span data-ttu-id="28cc8-118">编译器将发出以下警告：</span><span class="sxs-lookup"><span data-stu-id="28cc8-118">The compiler issues the following warning:</span></span>

> <span data-ttu-id="28cc8-119">CS7022：程序的入口点是全局代码；忽略“Main()”入口点。</span><span class="sxs-lookup"><span data-stu-id="28cc8-119">CS7022 The entry point of the program is global code; ignoring 'Main()' entry point.</span></span>

<span data-ttu-id="28cc8-120">在具有顶级语句的项目中，不能使用 [-main](../../language-reference/compiler-options/advanced.md#mainentrypoint-or-startupobject) 编译器选项来选择入口点，即使该项目具有一个或多个 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="28cc8-120">In a project with top-level statements, you can't use the [-main](../../language-reference/compiler-options/advanced.md#mainentrypoint-or-startupobject) compiler option to select the entry point, even if the project has one or more `Main` methods.</span></span>

## <a name="using-directives"></a><span data-ttu-id="28cc8-121">`using` 指令</span><span class="sxs-lookup"><span data-stu-id="28cc8-121">`using` directives</span></span>

<span data-ttu-id="28cc8-122">如果包含 using 指令，则它们必须首先出现在文件中，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="28cc8-122">If you include using directives, they must come first in the file, as in this example:</span></span>

:::code language="csharp" source="snippets/top-level-statements-1/Program.cs":::

## <a name="global-namespace"></a><span data-ttu-id="28cc8-123">全局命名空间</span><span class="sxs-lookup"><span data-stu-id="28cc8-123">Global namespace</span></span>

<span data-ttu-id="28cc8-124">顶级语句隐式位于全局命名空间中。</span><span class="sxs-lookup"><span data-stu-id="28cc8-124">Top-level statements are implicitly in the global namespace.</span></span>

## <a name="namespaces-and-type-definitions"></a><span data-ttu-id="28cc8-125">命名空间和类型定义</span><span class="sxs-lookup"><span data-stu-id="28cc8-125">Namespaces and type definitions</span></span>

<span data-ttu-id="28cc8-126">具有顶级语句的文件还可以包含命名空间和类型定义，但它们必须位于顶级语句之后。</span><span class="sxs-lookup"><span data-stu-id="28cc8-126">A file with top-level statements can also contain namespaces and type definitions, but they must come after the top-level statements.</span></span> <span data-ttu-id="28cc8-127">例如：</span><span class="sxs-lookup"><span data-stu-id="28cc8-127">For example:</span></span>

:::code language="csharp" source="snippets/top-level-statements-2/Program.cs":::

## `args`

<span data-ttu-id="28cc8-128">顶级语句可以引用 `args` 变量来访问输入的任何命令行参数。</span><span class="sxs-lookup"><span data-stu-id="28cc8-128">Top-level statements can reference the `args` variable to access any command-line arguments that were entered.</span></span> <span data-ttu-id="28cc8-129">`args` 变量永远不会为 null，但如果未提供任何命令行参数，则其 `Length` 将为零。</span><span class="sxs-lookup"><span data-stu-id="28cc8-129">The `args` variable is never null but its `Length` is zero if no command-line arguments were provided.</span></span> <span data-ttu-id="28cc8-130">例如：</span><span class="sxs-lookup"><span data-stu-id="28cc8-130">For example:</span></span>

:::code language="csharp" source="snippets/top-level-statements-3/Program.cs":::

## `await`

<span data-ttu-id="28cc8-131">可以通过使用 `await` 来调用异步方法。</span><span class="sxs-lookup"><span data-stu-id="28cc8-131">You can call an async method by using `await`.</span></span> <span data-ttu-id="28cc8-132">例如：</span><span class="sxs-lookup"><span data-stu-id="28cc8-132">For example:</span></span>

:::code language="csharp" source="snippets/top-level-statements-4/Program.cs":::

## <a name="exit-code-for-the-process"></a><span data-ttu-id="28cc8-133">进程的退出代码</span><span class="sxs-lookup"><span data-stu-id="28cc8-133">Exit code for the process</span></span>

<span data-ttu-id="28cc8-134">若要在应用程序结束时返回 `int` 值，请像在 `Main` 方法中返回 `int` 那样使用 `return` 语句。</span><span class="sxs-lookup"><span data-stu-id="28cc8-134">To return an `int` value when the application ends, use the `return` statement as you would in a `Main` method that returns an `int`.</span></span> <span data-ttu-id="28cc8-135">例如：</span><span class="sxs-lookup"><span data-stu-id="28cc8-135">For example:</span></span>

:::code language="csharp" source="snippets/top-level-statements-5/Program.cs":::

## <a name="implicit-entry-point-method"></a><span data-ttu-id="28cc8-136">隐式入口点方法</span><span class="sxs-lookup"><span data-stu-id="28cc8-136">Implicit entry point method</span></span>

<span data-ttu-id="28cc8-137">编译器会生成一个方法，作为具有顶级语句的项目的程序入口点。</span><span class="sxs-lookup"><span data-stu-id="28cc8-137">The compiler generates a method to serve as the program entry point for a project with top-level statements.</span></span> <span data-ttu-id="28cc8-138">此方法的名称实际上并不是 `Main`，而是代码无法直接引用的实现细节。</span><span class="sxs-lookup"><span data-stu-id="28cc8-138">The name of this method isn't actually `Main`, it's an implementation detail that your code can't reference directly.</span></span> <span data-ttu-id="28cc8-139">方法的签名取决于顶级语句是包含 `await` 关键字还是 `return` 语句。</span><span class="sxs-lookup"><span data-stu-id="28cc8-139">The signature of the method depends on whether the top-level statements contain the `await` keyword or the `return` statement.</span></span> <span data-ttu-id="28cc8-140">下表显示了方法签名的外观，为了方便起见，在表中使用了方法名称 `Main`。</span><span class="sxs-lookup"><span data-stu-id="28cc8-140">The following table shows what the method signature would look like, using the method name `Main` in the table for convenience.</span></span>

| <span data-ttu-id="28cc8-141">顶级代码包含</span><span class="sxs-lookup"><span data-stu-id="28cc8-141">Top-level code contains</span></span>| <span data-ttu-id="28cc8-142">隐式 `Main` 签名</span><span class="sxs-lookup"><span data-stu-id="28cc8-142">Implicit `Main` signature</span></span>                    |
|------------------------|----------------------------------------------|
| <span data-ttu-id="28cc8-143">`await` 和 `return`</span><span class="sxs-lookup"><span data-stu-id="28cc8-143">`await` and `return`</span></span>   | `static async Task<int> Main(string[] args)` |
| `await`                | `static async Task Main(string[] args)`      |
| `return`               | `static int Main(string[] args)`             |
| <span data-ttu-id="28cc8-144">否 `await` 或 `return`</span><span class="sxs-lookup"><span data-stu-id="28cc8-144">No `await` or `return`</span></span> | `static void Main(string[] args)`            |

## <a name="c-language-specification"></a><span data-ttu-id="28cc8-145">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="28cc8-145">C# language specification</span></span>

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]
[<span data-ttu-id="28cc8-146">顶级语句</span><span class="sxs-lookup"><span data-stu-id="28cc8-146">Top-level statements</span></span>](~/_csharplang/proposals/csharp-9.0/top-level-statements.md)

## <a name="see-also"></a><span data-ttu-id="28cc8-147">另请参阅</span><span class="sxs-lookup"><span data-stu-id="28cc8-147">See also</span></span>

- [<span data-ttu-id="28cc8-148">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="28cc8-148">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="28cc8-149">方法</span><span class="sxs-lookup"><span data-stu-id="28cc8-149">Methods</span></span>](../classes-and-structs/methods.md)
- [<span data-ttu-id="28cc8-150">在 C# 程序内部</span><span class="sxs-lookup"><span data-stu-id="28cc8-150">Inside a C# Program</span></span>](../inside-a-program/index.md)
