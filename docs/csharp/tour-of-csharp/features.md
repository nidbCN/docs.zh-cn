---
title: C# 教程 - 主要语言区域
description: 刚开始接触 C#？ 了解 C# 语言的基础知识。
ms.date: 08/06/2020
ms.openlocfilehash: f0e9bff144cc3c853a82f2ee6b400049df60683d
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2020
ms.locfileid: "88068456"
---
# <a name="major-language-areas"></a><span data-ttu-id="94e0a-104">主要语言区域</span><span class="sxs-lookup"><span data-stu-id="94e0a-104">Major language areas</span></span>

## <a name="arrays-collections-and-linq"></a><span data-ttu-id="94e0a-105">数组、集合和 LINQ</span><span class="sxs-lookup"><span data-stu-id="94e0a-105">Arrays, collections, and LINQ</span></span>

<span data-ttu-id="94e0a-106">C# 和 .NET 提供了许多不同的集合类型。</span><span class="sxs-lookup"><span data-stu-id="94e0a-106">C# and .NET provide many different collection types.</span></span> <span data-ttu-id="94e0a-107">数组包含由语言定义的语法。</span><span class="sxs-lookup"><span data-stu-id="94e0a-107">Arrays have syntax defined by the language.</span></span> <span data-ttu-id="94e0a-108">泛型集合类型列在 <xref:System.Collections.Generic?displayProperty=fullName> 命名空间中。</span><span class="sxs-lookup"><span data-stu-id="94e0a-108">Generic collection types are listed in the <xref:System.Collections.Generic?displayProperty=fullName> namespace.</span></span> <span data-ttu-id="94e0a-109">专用集合包括 <xref:System.Span%601?displayProperty=nameWithType>（用于访问堆栈帧上的连续内存），以及 <xref:System.Memory%601?displayProperty=nameWithType>（用于访问托管堆上的连续内存）。</span><span class="sxs-lookup"><span data-stu-id="94e0a-109">Specialized collections include <xref:System.Span%601?displayProperty=nameWithType> for accessing continuous memory on the stack frame, and <xref:System.Memory%601?displayProperty=nameWithType> for accessing continuous memory on the managed heap.</span></span> <span data-ttu-id="94e0a-110">所有集合（包括数组、<xref:System.Span%601> 和 <xref:System.Memory%601>）都遵循一种统一的迭代原则。</span><span class="sxs-lookup"><span data-stu-id="94e0a-110">All collections, including arrays, <xref:System.Span%601>, and <xref:System.Memory%601> share a unifying principle for iteration.</span></span> <span data-ttu-id="94e0a-111">使用 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 接口。</span><span class="sxs-lookup"><span data-stu-id="94e0a-111">You use the <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> interface.</span></span> <span data-ttu-id="94e0a-112">这种统一的原则意味着任何集合类型都可以与 LINQ 查询或其他算法一起使用。</span><span class="sxs-lookup"><span data-stu-id="94e0a-112">This unifying principle means that any of the collection types can be used with LINQ queries or other algorithms.</span></span> <span data-ttu-id="94e0a-113">你可以使用 <xref:System.Collections.Generic.IEnumerable%601> 编写方法，这些算法适用于任何集合。</span><span class="sxs-lookup"><span data-stu-id="94e0a-113">You write methods using <xref:System.Collections.Generic.IEnumerable%601> and those algorithms work with any collection.</span></span>

### <a name="arrays"></a><span data-ttu-id="94e0a-114">数组</span><span class="sxs-lookup"><span data-stu-id="94e0a-114">Arrays</span></span>

<span data-ttu-id="94e0a-115">[数组](../programming-guide/arrays/index.md)是一种数据结构，其中包含许多通过计算索引访问的变量。</span><span class="sxs-lookup"><span data-stu-id="94e0a-115">An [***array***](../programming-guide/arrays/index.md) is a data structure that contains a number of variables that are accessed through computed indices.</span></span> <span data-ttu-id="94e0a-116">数组中的变量（亦称为数组的“元素”）均为同一种类型。</span><span class="sxs-lookup"><span data-stu-id="94e0a-116">The variables contained in an array, also called the ***elements*** of the array, are all of the same type.</span></span> <span data-ttu-id="94e0a-117">我们将这种类型称为数组的“元素类型”。</span><span class="sxs-lookup"><span data-stu-id="94e0a-117">This type is called the ***element type*** of the array.</span></span>

<span data-ttu-id="94e0a-118">数组类型是引用类型，声明数组变量只是为引用数组实例预留空间。</span><span class="sxs-lookup"><span data-stu-id="94e0a-118">Array types are reference types, and the declaration of an array variable simply sets aside space for a reference to an array instance.</span></span> <span data-ttu-id="94e0a-119">实际的数组实例是在运行时使用 `new` 运算符动态创建而成。</span><span class="sxs-lookup"><span data-stu-id="94e0a-119">Actual array instances are created dynamically at runtime using the `new` operator.</span></span> <span data-ttu-id="94e0a-120">`new` 运算指定了新数组实例的长度，然后在此实例的生存期内固定使用这个长度。</span><span class="sxs-lookup"><span data-stu-id="94e0a-120">The `new` operation specifies the ***length*** of the new array instance, which is then fixed for the lifetime of the instance.</span></span> <span data-ttu-id="94e0a-121">数组元素的索引介于 `0` 到 `Length - 1` 之间。</span><span class="sxs-lookup"><span data-stu-id="94e0a-121">The indices of the elements of an array range from `0` to `Length - 1`.</span></span> <span data-ttu-id="94e0a-122">`new` 运算符自动将数组元素初始化为其默认值（例如，所有数值类型的默认值为 0，所有引用类型的默认值为 `null`）。</span><span class="sxs-lookup"><span data-stu-id="94e0a-122">The `new` operator automatically initializes the elements of an array to their default value, which, for example, is zero for all numeric types and `null` for all reference types.</span></span>

<span data-ttu-id="94e0a-123">以下示例创建 `int` 元素数组，初始化此数组，然后打印输出此数组的内容。</span><span class="sxs-lookup"><span data-stu-id="94e0a-123">The following example creates an array of `int` elements, initializes the array, and prints out the contents of the array.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="ArraysSample":::

<span data-ttu-id="94e0a-124">上面的示例创建***一维数组***，并对其执行运算。</span><span class="sxs-lookup"><span data-stu-id="94e0a-124">This example creates and operates on a ***single-dimensional array***.</span></span> <span data-ttu-id="94e0a-125">C# 还支持***多维数组***。</span><span class="sxs-lookup"><span data-stu-id="94e0a-125">C# also supports ***multi-dimensional arrays***.</span></span> <span data-ttu-id="94e0a-126">数组类型的维数（亦称为数组类型的***秩***）是 1 与数组类型方括号内的逗号数量相加的结果。</span><span class="sxs-lookup"><span data-stu-id="94e0a-126">The number of dimensions of an array type, also known as the ***rank*** of the array type, is one plus the number of commas written between the square brackets of the array type.</span></span> <span data-ttu-id="94e0a-127">以下示例分别分配一维、二维、三维数组。</span><span class="sxs-lookup"><span data-stu-id="94e0a-127">The following example allocates a single-dimensional, a two-dimensional, and a three-dimensional array, respectively.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="DeclareArrays":::

<span data-ttu-id="94e0a-128">`a1` 数组包含 10 个元素，`a2` 数组包含 50 个元素 (10 × 5)，`a3` 数组包含 100 个元素 (10 × 5 × 2)。</span><span class="sxs-lookup"><span data-stu-id="94e0a-128">The `a1` array contains 10 elements, the `a2` array contains 50 (10 × 5) elements, and the `a3` array contains 100 (10 × 5 × 2) elements.</span></span>
<span data-ttu-id="94e0a-129">数组的元素类型可以是任意类型（包括数组类型）。</span><span class="sxs-lookup"><span data-stu-id="94e0a-129">The element type of an array can be any type, including an array type.</span></span> <span data-ttu-id="94e0a-130">包含数组类型元素的数组有时称为“交错数组”，因为元素数组的长度不必全都一样。</span><span class="sxs-lookup"><span data-stu-id="94e0a-130">An array with elements of an array type is sometimes called a ***jagged array*** because the lengths of the element arrays don't all have to be the same.</span></span> <span data-ttu-id="94e0a-131">以下示例分配由 `int` 数组构成的数组：</span><span class="sxs-lookup"><span data-stu-id="94e0a-131">The following example allocates an array of arrays of `int`:</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="ArrayOfArrays":::

<span data-ttu-id="94e0a-132">第一行创建包含三个元素的数组，每个元素都是 `int[]` 类型，并且初始值均为 `null`。</span><span class="sxs-lookup"><span data-stu-id="94e0a-132">The first line creates an array with three elements, each of type `int[]` and each with an initial value of `null`.</span></span> <span data-ttu-id="94e0a-133">接下来的代码行将这三个元素初始化为引用长度不同的各个数组实例。</span><span class="sxs-lookup"><span data-stu-id="94e0a-133">The next lines then initialize the three elements with references to individual array instances of varying lengths.</span></span>

<span data-ttu-id="94e0a-134">通过 `new` 运算符，可以使用“数组初始值设定项”（在分隔符 `{` 和 `}` 内编写的表达式列表）指定数组元素的初始值。</span><span class="sxs-lookup"><span data-stu-id="94e0a-134">The `new` operator permits the initial values of the array elements to be specified using an ***array initializer***, which is a list of expressions written between the delimiters `{` and `}`.</span></span> <span data-ttu-id="94e0a-135">以下示例分配 `int[]`，并将其初始化为包含三个元素。</span><span class="sxs-lookup"><span data-stu-id="94e0a-135">The following example allocates and initializes an `int[]` with three elements.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="InitializeArray":::

<span data-ttu-id="94e0a-136">可从 `{` 和 `}` 内的表达式数量推断出数组的长度。</span><span class="sxs-lookup"><span data-stu-id="94e0a-136">The length of the array is inferred from the number of expressions between `{` and `}`.</span></span> <span data-ttu-id="94e0a-137">局部变量和字段声明可以进一步缩短，这样就不用重新声明数组类型了。</span><span class="sxs-lookup"><span data-stu-id="94e0a-137">Local variable and field declarations can be shortened further such that the array type doesn't have to be restated.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="InitializeShortened":::

<span data-ttu-id="94e0a-138">以上两个示例等同于以下代码：</span><span class="sxs-lookup"><span data-stu-id="94e0a-138">Both of the previous examples are equivalent to the following code:</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="InitializeGenerated":::

<span data-ttu-id="94e0a-139">`foreach` 语句可用于枚举任何集合的元素。</span><span class="sxs-lookup"><span data-stu-id="94e0a-139">The `foreach` statement can be used to enumerate the elements of any collection.</span></span> <span data-ttu-id="94e0a-140">以下代码从前一个示例中枚举数组：</span><span class="sxs-lookup"><span data-stu-id="94e0a-140">The following code enumerates the array from the preceding example:</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="EnumerateArray":::

<span data-ttu-id="94e0a-141">`foreach` 语句使用 <xref:System.Collections.Generic.IEnumerable%601> 接口，因此适用于任何集合。</span><span class="sxs-lookup"><span data-stu-id="94e0a-141">The `foreach` statement uses the <xref:System.Collections.Generic.IEnumerable%601> interface, so can work with any collection.</span></span>

## <a name="string-interpolation"></a><span data-ttu-id="94e0a-142">字符串内插</span><span class="sxs-lookup"><span data-stu-id="94e0a-142">String interpolation</span></span>

<span data-ttu-id="94e0a-143">C# [字符串内插](../language-reference/tokens/interpolated.md)使你能够通过定义表达式（其结果放置在格式字符串中）来设置字符串格式。</span><span class="sxs-lookup"><span data-stu-id="94e0a-143">C# [***string interpolation***](../language-reference/tokens/interpolated.md) enables you to format strings by defining expressions whose results are placed in a format string.</span></span> <span data-ttu-id="94e0a-144">例如，以下示例从一组天气数据显示给定日期的温度：</span><span class="sxs-lookup"><span data-stu-id="94e0a-144">For example, the following example prints the temperature on a given day from a set of weather data:</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="StringInterpolation":::

<span data-ttu-id="94e0a-145">内插字符串通过 `$` 标记来声明。</span><span class="sxs-lookup"><span data-stu-id="94e0a-145">An interpolated string is declared using the `$` token.</span></span> <span data-ttu-id="94e0a-146">字符串插内插计算 `{` 和 `}` 之间的表达式，然后将结果转换为 `string`，并将括号内的文本替换为表达式的字符串结果。</span><span class="sxs-lookup"><span data-stu-id="94e0a-146">String interpolation evaluates the expressions between `{` and `}`, then converts the result to a `string`, and replaces the text between the brackets with the string result of the expression.</span></span> <span data-ttu-id="94e0a-147">第一个表达式 (`{weatherData.Data:MM-DD-YYYY}`) 中的 `:` 指定格式字符串。</span><span class="sxs-lookup"><span data-stu-id="94e0a-147">The `:` in the first expression, `{weatherData.Data:MM-DD-YYYY}` specifies the *format string*.</span></span> <span data-ttu-id="94e0a-148">在前一个示例中，这指定日期应以“MM-DD-YYYY”格式显示。</span><span class="sxs-lookup"><span data-stu-id="94e0a-148">In the preceding example, it specifies that the date should be printed in "MM-DD-YYYY" format.</span></span>

## <a name="pattern-matching"></a><span data-ttu-id="94e0a-149">模式匹配</span><span class="sxs-lookup"><span data-stu-id="94e0a-149">Pattern matching</span></span>

<span data-ttu-id="94e0a-150">C# 语言提供[模式匹配](../pattern-matching.md)表达式来查询对象的状态并基于该状态执行代码。</span><span class="sxs-lookup"><span data-stu-id="94e0a-150">The C# language provides [***pattern matching***](../pattern-matching.md) expressions to query the state of an object and execute code based on that state.</span></span> <span data-ttu-id="94e0a-151">你可以检查属性和字段的类型和值，以确定要执行的操作。</span><span class="sxs-lookup"><span data-stu-id="94e0a-151">You can inspect types and the values of properties and fields to determine which action to take.</span></span> <span data-ttu-id="94e0a-152">`switch` 表达式是模式匹配的主要表达式。</span><span class="sxs-lookup"><span data-stu-id="94e0a-152">The `switch` expression is the primary expression for pattern matching.</span></span>

## <a name="delegates-and-lambda-expressions"></a><span data-ttu-id="94e0a-153">委托和 Lambda 表达式</span><span class="sxs-lookup"><span data-stu-id="94e0a-153">Delegates and lambda expressions</span></span>

<span data-ttu-id="94e0a-154">[委托类型](../delegates-overview.md)表示对具有特定参数列表和返回类型的方法的引用。</span><span class="sxs-lookup"><span data-stu-id="94e0a-154">A [***delegate type***](../delegates-overview.md) represents references to methods with a particular parameter list and return type.</span></span> <span data-ttu-id="94e0a-155">通过委托，可以将方法视为可分配给变量并可作为参数传递的实体。</span><span class="sxs-lookup"><span data-stu-id="94e0a-155">Delegates make it possible to treat methods as entities that can be assigned to variables and passed as parameters.</span></span> <span data-ttu-id="94e0a-156">委托还类似于其他一些语言中存在的“函数指针”概念。</span><span class="sxs-lookup"><span data-stu-id="94e0a-156">Delegates are similar to the concept of function pointers found in some other languages.</span></span> <span data-ttu-id="94e0a-157">与函数指针不同，委托是面向对象且类型安全的。</span><span class="sxs-lookup"><span data-stu-id="94e0a-157">Unlike function pointers, delegates are object-oriented and type-safe.</span></span>

<span data-ttu-id="94e0a-158">下面的示例声明并使用 `Function` 委托类型。</span><span class="sxs-lookup"><span data-stu-id="94e0a-158">The following example declares and uses a delegate type named `Function`.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="DelegateExample":::

<span data-ttu-id="94e0a-159">`Function` 委托类型实例可以引用需要使用 `double` 自变量并返回 `double` 值的方法。</span><span class="sxs-lookup"><span data-stu-id="94e0a-159">An instance of the `Function` delegate type can reference any method that takes a `double` argument and returns a `double` value.</span></span> <span data-ttu-id="94e0a-160">`Apply` 方法将给定的 `Function` 应用于 `double[]` 的元素，从而返回包含结果的 `double[]`。</span><span class="sxs-lookup"><span data-stu-id="94e0a-160">The `Apply` method applies a given `Function` to the elements of a `double[]`, returning a `double[]` with the results.</span></span> <span data-ttu-id="94e0a-161">在 `Main` 方法中，`Apply` 用于向 `double[]` 应用三个不同的函数。</span><span class="sxs-lookup"><span data-stu-id="94e0a-161">In the `Main` method, `Apply` is used to apply three different functions to a `double[]`.</span></span>

<span data-ttu-id="94e0a-162">委托可以引用静态方法（如上面示例中的 `Square` 或 `Math.Sin`）或实例方法（如上面示例中的 `m.Multiply`）。</span><span class="sxs-lookup"><span data-stu-id="94e0a-162">A delegate can reference either a static method (such as `Square` or `Math.Sin` in the previous example) or an instance method (such as `m.Multiply` in the previous example).</span></span> <span data-ttu-id="94e0a-163">引用实例方法的委托还会引用特定对象，通过委托调用实例方法时，该对象会变成调用中的 `this`。</span><span class="sxs-lookup"><span data-stu-id="94e0a-163">A delegate that references an instance method also references a particular object, and when the instance method is invoked through the delegate, that object becomes `this` in the invocation.</span></span>

<span data-ttu-id="94e0a-164">还可以使用匿名函数创建委托，这些函数是在声明时创建的“内联方法”。</span><span class="sxs-lookup"><span data-stu-id="94e0a-164">Delegates can also be created using anonymous functions, which are "inline methods" that are created when declared.</span></span> <span data-ttu-id="94e0a-165">匿名函数可以查看周围方法的局部变量。</span><span class="sxs-lookup"><span data-stu-id="94e0a-165">Anonymous functions can see the local variables of the surrounding methods.</span></span> <span data-ttu-id="94e0a-166">以下示例不创建类：</span><span class="sxs-lookup"><span data-stu-id="94e0a-166">The following example doesn't create a class:</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="UseDelegate":::

<span data-ttu-id="94e0a-167">委托不知道或不在意其所引用的方法的类。</span><span class="sxs-lookup"><span data-stu-id="94e0a-167">A delegate doesn't know or care about the class of the method it references.</span></span> <span data-ttu-id="94e0a-168">重要的是，引用的方法具有与委托相同的参数和返回类型。</span><span class="sxs-lookup"><span data-stu-id="94e0a-168">All that matters is that the referenced method has the same parameters and return type as the delegate.</span></span>

## <a name="async--await"></a><span data-ttu-id="94e0a-169">async/await</span><span class="sxs-lookup"><span data-stu-id="94e0a-169">async / await</span></span>

<span data-ttu-id="94e0a-170">C# 支持含两个关键字的异步程序：`async` 和 `await`。</span><span class="sxs-lookup"><span data-stu-id="94e0a-170">C# supports asynchronous programs with two keywords: `async` and `await`.</span></span> <span data-ttu-id="94e0a-171">将 `async` 修饰符添加到方法声明中，以声明这是异步方法。</span><span class="sxs-lookup"><span data-stu-id="94e0a-171">You add the `async` modifier to a method declaration to declare the method is asynchronous.</span></span> <span data-ttu-id="94e0a-172">`await` 运算符通知编译器异步等待结果完成。</span><span class="sxs-lookup"><span data-stu-id="94e0a-172">The `await` operator tells the compiler to asynchronously await for a result to finish.</span></span> <span data-ttu-id="94e0a-173">控件返回给调用方，该方法返回一个管理异步工作状态的结构。</span><span class="sxs-lookup"><span data-stu-id="94e0a-173">Control is returned to the caller, and the method returns a structure that manages the state of the asynchronous work.</span></span> <span data-ttu-id="94e0a-174">结构通常是 <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>，但可以是任何支持 awaiter 模式的类型。</span><span class="sxs-lookup"><span data-stu-id="94e0a-174">The structure is typically a <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>, but can be any type that supports the awaiter pattern.</span></span> <span data-ttu-id="94e0a-175">这些功能使你能够编写这样的代码：以其同步对应项的形式读取，但以异步方式执行。</span><span class="sxs-lookup"><span data-stu-id="94e0a-175">These features enable you to write code that reads as its synchronous counterpart, but executes asynchronously.</span></span> <span data-ttu-id="94e0a-176">例如，以下代码会下载 [Microsoft Docs](https://docs.microsoft.com) 的主页：</span><span class="sxs-lookup"><span data-stu-id="94e0a-176">For example, the following code downloads the home page for [Microsoft docs](https://docs.microsoft.com):</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="AsyncExample":::

<span data-ttu-id="94e0a-177">这一小型示例显示了异步编程的主要功能：</span><span class="sxs-lookup"><span data-stu-id="94e0a-177">This small sample shows the major features for asynchronous programming:</span></span>

- <span data-ttu-id="94e0a-178">方法声明包含 `async` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="94e0a-178">The method declaration includes the `async` modifier.</span></span>
- <span data-ttu-id="94e0a-179">方法 `await` 的主体是 `GetByteArrayAsync` 方法的返回。</span><span class="sxs-lookup"><span data-stu-id="94e0a-179">The body of the method `await`s the return of the `GetByteArrayAsync` method.</span></span>
- <span data-ttu-id="94e0a-180">`return` 语句中指定的类型与方法的 `Task<T>` 声明中的类型参数匹配。</span><span class="sxs-lookup"><span data-stu-id="94e0a-180">The type specified in the `return` statement matches the type argument in the `Task<T>` declaration for the method.</span></span> <span data-ttu-id="94e0a-181">（返回 `Task` 的方法将使用不带任何参数的 `return` 语句）。</span><span class="sxs-lookup"><span data-stu-id="94e0a-181">(A method that returns a `Task` would use `return` statements without any argument).</span></span>

## <a name="attributes"></a><span data-ttu-id="94e0a-182">属性</span><span class="sxs-lookup"><span data-stu-id="94e0a-182">Attributes</span></span>

<span data-ttu-id="94e0a-183">C# 程序中的类型、成员和其他实体支持使用修饰符来控制其行为的某些方面。</span><span class="sxs-lookup"><span data-stu-id="94e0a-183">Types, members, and other entities in a C# program support modifiers that control certain aspects of their behavior.</span></span> <span data-ttu-id="94e0a-184">例如，方法的可访问性是由 `public`、`protected`、`internal` 和 `private` 修饰符控制。</span><span class="sxs-lookup"><span data-stu-id="94e0a-184">For example, the accessibility of a method is controlled using the `public`, `protected`, `internal`, and `private` modifiers.</span></span> <span data-ttu-id="94e0a-185">C# 整合了这种能力，以便可以将用户定义类型的声明性信息附加到程序实体，并在运行时检索此类信息。</span><span class="sxs-lookup"><span data-stu-id="94e0a-185">C# generalizes this capability such that user-defined types of declarative information can be attached to program entities and retrieved at run-time.</span></span> <span data-ttu-id="94e0a-186">程序通过定义和使用[特性](../programming-guide/concepts/attributes/index.md)来指定此类额外的声明性信息。</span><span class="sxs-lookup"><span data-stu-id="94e0a-186">Programs specify this additional declarative information by defining and using [***attributes***](../programming-guide/concepts/attributes/index.md).</span></span>

<span data-ttu-id="94e0a-187">以下示例声明了 `HelpAttribute` 特性，可将其附加到程序实体，以提供指向关联文档的链接。</span><span class="sxs-lookup"><span data-stu-id="94e0a-187">The following example declares a `HelpAttribute` attribute that can be placed on program entities to provide links to their associated documentation.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="DefineAttribute":::

<span data-ttu-id="94e0a-188">所有特性类都派生自 .NET 库提供的 <xref:System.Attribute> 基类。</span><span class="sxs-lookup"><span data-stu-id="94e0a-188">All attribute classes derive from the <xref:System.Attribute> base class provided by the .NET library.</span></span> <span data-ttu-id="94e0a-189">特性的应用方式为，在相关声明前的方括号内指定特性的名称以及任意自变量。</span><span class="sxs-lookup"><span data-stu-id="94e0a-189">Attributes can be applied by giving their name, along with any arguments, inside square brackets just before the associated declaration.</span></span> <span data-ttu-id="94e0a-190">如果特性的名称以 `Attribute` 结尾，那么可以在引用特性时省略这部分名称。</span><span class="sxs-lookup"><span data-stu-id="94e0a-190">If an attribute’s name ends in `Attribute`, that part of the name can be omitted when the attribute is referenced.</span></span> <span data-ttu-id="94e0a-191">例如，可按如下方法使用 `HelpAttribute`。</span><span class="sxs-lookup"><span data-stu-id="94e0a-191">For example, the `HelpAttribute` can be used as follows.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="UseAttributes":::

<span data-ttu-id="94e0a-192">此示例将 `HelpAttribute` 附加到 `Widget` 类。</span><span class="sxs-lookup"><span data-stu-id="94e0a-192">This example attaches a `HelpAttribute` to the `Widget` class.</span></span> <span data-ttu-id="94e0a-193">还向此类中的 `Display` 方法附加了另一个 `HelpAttribute`。</span><span class="sxs-lookup"><span data-stu-id="94e0a-193">It adds another `HelpAttribute` to the `Display` method in the class.</span></span> <span data-ttu-id="94e0a-194">特性类的公共构造函数控制了将特性附加到程序实体时必须提供的信息。</span><span class="sxs-lookup"><span data-stu-id="94e0a-194">The public constructors of an attribute class control the information that must be provided when the attribute is attached to a program entity.</span></span> <span data-ttu-id="94e0a-195">可以通过引用特性类的公共读写属性（如上面示例对 `Topic` 属性的引用），提供其他信息。</span><span class="sxs-lookup"><span data-stu-id="94e0a-195">Additional information can be provided by referencing public read-write properties of the attribute class (such as the reference to the `Topic` property previously).</span></span>

<span data-ttu-id="94e0a-196">可以在运行时使用反射来读取和操纵特性定义的元数据。</span><span class="sxs-lookup"><span data-stu-id="94e0a-196">The metadata defined by attributes can be read and manipulated at runtime using reflection.</span></span> <span data-ttu-id="94e0a-197">如果使用这种方法请求获取特定特性，便会调用特性类的构造函数（在程序源中提供信息），并返回生成的特性实例。</span><span class="sxs-lookup"><span data-stu-id="94e0a-197">When a particular attribute is requested using this technique, the constructor for the attribute class is invoked with the information provided in the program source, and the resulting attribute instance is returned.</span></span> <span data-ttu-id="94e0a-198">如果是通过属性提供其他信息，那么在特性实例返回前，这些属性会设置为给定值。</span><span class="sxs-lookup"><span data-stu-id="94e0a-198">If additional information was provided through properties, those properties are set to the given values before the attribute instance is returned.</span></span>

<span data-ttu-id="94e0a-199">下面的代码示例展示了如何获取与 `Widget` 类及其 `Display` 方法相关联的 `HelpAttribute` 实例。</span><span class="sxs-lookup"><span data-stu-id="94e0a-199">The following code sample demonstrates how to get the `HelpAttribute` instances associated to the `Widget` class and its `Display` method.</span></span>

:::code language="csharp" source="./snippets/shared/Features.cs" ID="ReadAttributes":::

## <a name="learn-more"></a><span data-ttu-id="94e0a-200">了解详细信息</span><span class="sxs-lookup"><span data-stu-id="94e0a-200">Learn more</span></span>

<span data-ttu-id="94e0a-201">可以通过试用其中一个[教程](../tutorials/index.md)来了解更多关于 C# 的内容。</span><span class="sxs-lookup"><span data-stu-id="94e0a-201">You can explore more about C# by trying one of our [tutorials](../tutorials/index.md).</span></span>

>[!div class="step-by-step"]
>[<span data-ttu-id="94e0a-202">上一页</span><span class="sxs-lookup"><span data-stu-id="94e0a-202">Previous</span></span>](program-building-blocks.md)