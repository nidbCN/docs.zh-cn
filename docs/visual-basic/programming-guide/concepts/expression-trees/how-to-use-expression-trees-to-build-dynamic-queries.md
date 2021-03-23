---
title: '基于运行时状态的查询 (Visual Basic) '
description: 介绍了可由代码用来根据运行时状态，通过改变 LINQ 方法调用或传入这些方法的表达式树来进行动态查询的各种技术。
ms.date: 02/14/2021
ms.assetid: 16278787-7532-4b65-98b2-7a412406c4ee
ms.openlocfilehash: fe59fc8287e67247884cfdcf1bacc6ed2a447e5a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875871"
---
# <a name="querying-based-on-runtime-state-visual-basic"></a><span data-ttu-id="6b494-103">基于运行时状态的查询 (Visual Basic) </span><span class="sxs-lookup"><span data-stu-id="6b494-103">Querying based on runtime state (Visual Basic)</span></span>

<span data-ttu-id="6b494-104">考虑 <xref:System.Linq.IQueryable> 针对数据源定义 [T) 的或 IQueryable (的 ](<xref:System.Linq.IQueryable%601>) 代码：</span><span class="sxs-lookup"><span data-stu-id="6b494-104">Consider code that defines an <xref:System.Linq.IQueryable> or an [IQueryable(Of T)](<xref:System.Linq.IQueryable%601>) against a data source:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Initialize":::

<span data-ttu-id="6b494-105">每次运行此代码时，都将执行相同的确切查询。</span><span class="sxs-lookup"><span data-stu-id="6b494-105">Every time you run this code, the same exact query will be executed.</span></span> <span data-ttu-id="6b494-106">这通常不是很有用，因为你可能希望代码根据运行时的情况执行不同的查询。</span><span class="sxs-lookup"><span data-stu-id="6b494-106">This is frequently not very useful, as you may want your code to execute different queries depending on conditions at runtime.</span></span> <span data-ttu-id="6b494-107">本文介绍如何根据运行时状态执行不同的查询。</span><span class="sxs-lookup"><span data-stu-id="6b494-107">This article describes how you can execute a different query based on runtime state.</span></span>

## <a name="iqueryable--iqueryableof-t-and-expression-trees"></a><span data-ttu-id="6b494-108">T) 和表达式树的 IQueryable/IQueryable (</span><span class="sxs-lookup"><span data-stu-id="6b494-108">IQueryable / IQueryable(Of T) and expression trees</span></span>

<span data-ttu-id="6b494-109">从根本上讲，<xref:System.Linq.IQueryable> 有两个组件：</span><span class="sxs-lookup"><span data-stu-id="6b494-109">Fundamentally, an <xref:System.Linq.IQueryable> has two components:</span></span>

* <span data-ttu-id="6b494-110"><xref:System.Linq.IQueryable.Expression> &mdash; 当前查询的组件的与语言和数据源无关的表示形式，以表达式树的形式表示。</span><span class="sxs-lookup"><span data-stu-id="6b494-110"><xref:System.Linq.IQueryable.Expression>&mdash;a language- and datasource-agnostic representation of the current query's components, in the form of an expression tree.</span></span>
* <span data-ttu-id="6b494-111"><xref:System.Linq.IQueryable.Provider> &mdash; LINQ 提供程序的实例，它知道如何将当前查询具体化为一个值或一组值。</span><span class="sxs-lookup"><span data-stu-id="6b494-111"><xref:System.Linq.IQueryable.Provider>&mdash;an instance of a LINQ provider, which knows how to materialize the current query into a value or set of values.</span></span>

<span data-ttu-id="6b494-112">在动态查询的上下文中，提供程序通常会保持不变；查询的表达式树将因查询而异。</span><span class="sxs-lookup"><span data-stu-id="6b494-112">In the context of dynamic querying, the provider will usually remain the same; the expression tree of the query will differ from query to query.</span></span>

<span data-ttu-id="6b494-113">表达式树是不可变的；如果需要不同的表达式树 &mdash; 并因此需要不同的查询 &mdash; 则需要将现有表达式树转换为新的表达式树，从而转换为新的 <xref:System.Linq.IQueryable>。</span><span class="sxs-lookup"><span data-stu-id="6b494-113">Expression trees are immutable; if you want a different expression tree&mdash;and thus a different query&mdash;you'll need to translate the existing expression tree to a new one, and thus to a new <xref:System.Linq.IQueryable>.</span></span>

<span data-ttu-id="6b494-114">以下各部分介绍了根据运行时状态，以不同方式进行查询的具体技术：</span><span class="sxs-lookup"><span data-stu-id="6b494-114">The following sections describe specific techniques for querying differently in response to runtime state:</span></span>

- <span data-ttu-id="6b494-115">从表达式树中使用运行时状态</span><span class="sxs-lookup"><span data-stu-id="6b494-115">Use runtime state from within the expression tree</span></span>
- <span data-ttu-id="6b494-116">调用其他 LINQ 方法</span><span class="sxs-lookup"><span data-stu-id="6b494-116">Call additional LINQ methods</span></span>
- <span data-ttu-id="6b494-117">改变传入到 LINQ 方法的表达式树</span><span class="sxs-lookup"><span data-stu-id="6b494-117">Vary the expression tree passed into the LINQ methods</span></span>
- <span data-ttu-id="6b494-118">使用工厂方法在 TDelegate) 表达式树构造[表达式 (](xref:System.Linq.Expressions.Expression%601)<xref:System.Linq.Expressions.Expression></span><span class="sxs-lookup"><span data-stu-id="6b494-118">Construct an [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601) expression tree using the factory methods at <xref:System.Linq.Expressions.Expression></span></span>
- <span data-ttu-id="6b494-119">将方法调用节点添加到 <xref:System.Linq.IQueryable> 的表达式树</span><span class="sxs-lookup"><span data-stu-id="6b494-119">Add method call nodes to an <xref:System.Linq.IQueryable>'s expression tree</span></span>
- <span data-ttu-id="6b494-120">构造字符串，并使用 [动态 LINQ 库](https://dynamic-linq.net/)</span><span class="sxs-lookup"><span data-stu-id="6b494-120">Construct strings, and use the [Dynamic LINQ library](https://dynamic-linq.net/)</span></span>

## <a name="use-runtime-state-from-within-the-expression-tree"></a><span data-ttu-id="6b494-121">从表达式树中使用运行时状态</span><span class="sxs-lookup"><span data-stu-id="6b494-121">Use runtime state from within the expression tree</span></span>

<span data-ttu-id="6b494-122">假设 LINQ 提供程序支持，进行动态查询的最简单方式是通过封闭的变量（如以下代码示例中的 `length`）直接在查询中引用运行时状态：</span><span class="sxs-lookup"><span data-stu-id="6b494-122">Assuming the LINQ provider supports it, the simplest way to query dynamically is to reference the runtime state directly in the query via a closed-over variable, such as `length` in the following code example:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Runtime_state_from_within_expression_tree":::

<span data-ttu-id="6b494-123">内部表达式树 &mdash; 以及查询 &mdash; 尚未修改；查询只返回不同的值，因为 `length` 的值已更改。</span><span class="sxs-lookup"><span data-stu-id="6b494-123">The internal expression tree&mdash;and thus the query&mdash;haven't been modified; the query returns different values only because the value of `length` has been changed.</span></span>

## <a name="call-additional-linq-methods"></a><span data-ttu-id="6b494-124">调用其他 LINQ 方法</span><span class="sxs-lookup"><span data-stu-id="6b494-124">Call additional LINQ methods</span></span>

<span data-ttu-id="6b494-125">通常，<xref:System.Linq.Queryable> 的[内置 LINQ 方法](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Linq.Queryable/src/System/Linq/Queryable.cs)执行两个步骤：</span><span class="sxs-lookup"><span data-stu-id="6b494-125">Generally, the [built-in LINQ methods](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Linq.Queryable/src/System/Linq/Queryable.cs) at <xref:System.Linq.Queryable> perform two steps:</span></span>

* <span data-ttu-id="6b494-126">在表示方法调用的 <xref:System.Linq.Expressions.MethodCallExpression> 中包装当前的表达式树。</span><span class="sxs-lookup"><span data-stu-id="6b494-126">Wrap the current expression tree in a <xref:System.Linq.Expressions.MethodCallExpression> representing the method call.</span></span>
* <span data-ttu-id="6b494-127">将包装的表达式树传递回提供程序，以便通过提供程序的 <xref:System.Linq.IQueryProvider.Execute%2A?displayProperty=nameWithType> 方法返回值；或通过 <xref:System.Linq.IQueryProvider.CreateQuery%2A?displayProperty=nameWithType> 方法返回转换后的查询对象。</span><span class="sxs-lookup"><span data-stu-id="6b494-127">Pass the wrapped expression tree back to the provider, either to return a value via the provider's <xref:System.Linq.IQueryProvider.Execute%2A?displayProperty=nameWithType> method; or to return a translated query object via the <xref:System.Linq.IQueryProvider.CreateQuery%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="6b494-128">可以将原始查询替换为返回 T) 方法的 [IQueryable (的 ](xref:System.Linq.IQueryable%601)结果，以获取新的查询。</span><span class="sxs-lookup"><span data-stu-id="6b494-128">You can replace the original query with the result of an [IQueryable(Of T)](xref:System.Linq.IQueryable%601)-returning method, to get a new query.</span></span> <span data-ttu-id="6b494-129">可以基于运行时状态在一定条件下执行此操作，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="6b494-129">You can do this conditionally based on runtime state, as in the following example:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Added_method_calls":::

## <a name="vary-the-expression-tree-passed-into-the-linq-methods"></a><span data-ttu-id="6b494-130">改变传入到 LINQ 方法的表达式树</span><span class="sxs-lookup"><span data-stu-id="6b494-130">Vary the expression tree passed into the LINQ methods</span></span>

<span data-ttu-id="6b494-131">可以将不同的表达式传入到 LINQ 方法，具体取决于运行时状态：</span><span class="sxs-lookup"><span data-stu-id="6b494-131">You can pass in different expressions to the LINQ methods, depending on runtime state:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Varying_expressions":::

<span data-ttu-id="6b494-132">你可能还希望使用第三方库（如 [LinqKit](http://www.albahari.com/nutshell/linqkit.aspx) 的 [PredicateBuilder](http://www.albahari.com/nutshell/predicatebuilder.aspx)）来编写各种子表达式：</span><span class="sxs-lookup"><span data-stu-id="6b494-132">You might also want to compose the various sub-expressions using a third-party library such as [LinqKit](http://www.albahari.com/nutshell/linqkit.aspx)'s [PredicateBuilder](http://www.albahari.com/nutshell/predicatebuilder.aspx):</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Compose_expression":::

## <a name="construct-expression-trees-and-queries-using-factory-methods"></a><span data-ttu-id="6b494-133">使用工厂方法构造表达式树和查询</span><span class="sxs-lookup"><span data-stu-id="6b494-133">Construct expression trees and queries using factory methods</span></span>

<span data-ttu-id="6b494-134">在到此为止的所有示例中，我们已知道编译时的元素类型 &mdash;`String`&mdash; 并因此知道查询的类型 &mdash; `IQueryable(Of String)`。</span><span class="sxs-lookup"><span data-stu-id="6b494-134">In all the examples up to this point, we've known the element type at compile time&mdash;`String`&mdash;and thus the type of the query&mdash;`IQueryable(Of String)`.</span></span> <span data-ttu-id="6b494-135">可能需要将组件添加到任何元素类型的查询中，或者根据元素类型添加不同的组件。</span><span class="sxs-lookup"><span data-stu-id="6b494-135">You may need to add components to a query of any element type, or to add different components depending on the element type.</span></span> <span data-ttu-id="6b494-136">可以使用 <xref:System.Linq.Expressions.Expression?displayProperty=fullName> 的工厂方法从头开始创建表达式树，从而在运行时将表达式定制为特定的元素类型。</span><span class="sxs-lookup"><span data-stu-id="6b494-136">You can create expression trees from the ground up, using the factory methods at <xref:System.Linq.Expressions.Expression?displayProperty=fullName>, and thus tailor the expression at runtime to a specific element type.</span></span>

### <a name="constructing-an-expressionof-tdelegate"></a><span data-ttu-id="6b494-137">构造 [TDelegate 的表达式 () ](xref:System.Linq.Expressions.Expression%601)</span><span class="sxs-lookup"><span data-stu-id="6b494-137">Constructing an [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601)</span></span>

<span data-ttu-id="6b494-138">当构造要传入其中一个 LINQ 方法的表达式时，实际上就是构造一个 [ (为 TDelegate) 的表达式 ](xref:System.Linq.Expressions.Expression%601)实例，其中 `TDelegate` 是某个委托类型，如 `Func(Of String, Boolean)` 、 `Action` 或自定义委托类型。</span><span class="sxs-lookup"><span data-stu-id="6b494-138">When you construct an expression to pass into one of the LINQ methods, you're actually constructing an instance of [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601), where `TDelegate` is some delegate type such as `Func(Of String, Boolean)`, `Action`, or a custom delegate type.</span></span>

<span data-ttu-id="6b494-139">[TDelegate) ) 的 Expression (](xref:System.Linq.Expressions.Expression%601) 继承自 <xref:System.Linq.Expressions.LambdaExpression> ，这表示完整的 lambda 表达式，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6b494-139">[Expression(Of TDelegate))](xref:System.Linq.Expressions.Expression%601) inherits from <xref:System.Linq.Expressions.LambdaExpression>, which represents a complete lambda expression like the following:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Compiler_generated":::

<span data-ttu-id="6b494-140"><xref:System.Linq.Expressions.LambdaExpression> 具有两个组件：</span><span class="sxs-lookup"><span data-stu-id="6b494-140">A <xref:System.Linq.Expressions.LambdaExpression> has two components:</span></span>

* <span data-ttu-id="6b494-141">参数列表 &mdash;`(x As String)`&mdash; 由 <xref:System.Linq.Expressions.LambdaExpression.Parameters> 属性表示</span><span class="sxs-lookup"><span data-stu-id="6b494-141">a parameter list&mdash;`(x As String)`&mdash;represented by the <xref:System.Linq.Expressions.LambdaExpression.Parameters> property</span></span>
* <span data-ttu-id="6b494-142">主体 &mdash;`x.StartsWith("a")`&mdash; 由 <xref:System.Linq.Expressions.LambdaExpression.Body> 属性表示。</span><span class="sxs-lookup"><span data-stu-id="6b494-142">a body&mdash;`x.StartsWith("a")`&mdash;represented by the <xref:System.Linq.Expressions.LambdaExpression.Body> property.</span></span>

<span data-ttu-id="6b494-143">构造 [表达式 (TDelegate) ](xref:System.Linq.Expressions.Expression%601) 的基本步骤如下所示：</span><span class="sxs-lookup"><span data-stu-id="6b494-143">The basic steps in constructing an [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601) are as follows:</span></span>

* <span data-ttu-id="6b494-144">使用 <xref:System.Linq.Expressions.Expression.Parameter%2A> 工厂方法为 lambda 表达式中的每个参数（如果有）定义 <xref:System.Linq.Expressions.ParameterExpression> 的对象。</span><span class="sxs-lookup"><span data-stu-id="6b494-144">Define <xref:System.Linq.Expressions.ParameterExpression> objects for each of the parameters (if any) in the lambda expression, using the <xref:System.Linq.Expressions.Expression.Parameter%2A> factory method.</span></span>

    :::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Factory_method_parameter":::

* <span data-ttu-id="6b494-145">使用你定义的 <xref:System.Linq.Expressions.ParameterExpression> 和 <xref:System.Linq.Expressions.Expression> 的工厂方法来构造 <xref:System.Linq.Expressions.LambdaExpression> 的主体。</span><span class="sxs-lookup"><span data-stu-id="6b494-145">Construct the body of your <xref:System.Linq.Expressions.LambdaExpression>, using the <xref:System.Linq.Expressions.ParameterExpression>(s) you've defined, and the factory methods at <xref:System.Linq.Expressions.Expression>.</span></span> <span data-ttu-id="6b494-146">例如，表示 `x.StartsWith("a")` 的表达式的构造方式如下：</span><span class="sxs-lookup"><span data-stu-id="6b494-146">For instance, an expression representing `x.StartsWith("a")` could be constructed like this:</span></span>

    :::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Factory_method_body":::

* <span data-ttu-id="6b494-147">使用适当的工厂方法重载，在 [TDelegate) ](xref:System.Linq.Expressions.Expression%601)的编译时类型化表达式中包装参数和正文 (<xref:System.Linq.Expressions.Expression.Lambda%2A> ：</span><span class="sxs-lookup"><span data-stu-id="6b494-147">Wrap the parameters and body in a compile-time-typed [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601), using the appropriate <xref:System.Linq.Expressions.Expression.Lambda%2A> factory method overload:</span></span>

    :::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Factory_method_lambda":::

<span data-ttu-id="6b494-148">以下各节介绍了一种方案，在该方案中，你可能想要构造 [TDelegate) 的表达式 (](xref:System.Linq.Expressions.Expression%601) 以传入 LINQ 方法，并提供有关如何使用工厂方法执行此操作的完整示例。</span><span class="sxs-lookup"><span data-stu-id="6b494-148">The following sections describe a scenario in which you might want to construct an [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601) to pass into a LINQ method, and provide a complete example of how to do so using the factory methods.</span></span>

### <a name="scenario"></a><span data-ttu-id="6b494-149">方案</span><span class="sxs-lookup"><span data-stu-id="6b494-149">Scenario</span></span>

<span data-ttu-id="6b494-150">假设你有多个实体类型：</span><span class="sxs-lookup"><span data-stu-id="6b494-150">Let's say you have multiple entity types:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Entities.vb":::

<span data-ttu-id="6b494-151">对于这些实体类型中的任何一个，你都需要筛选并仅返回那些在其某个 `string` 字段内具有给定文本的实体。</span><span class="sxs-lookup"><span data-stu-id="6b494-151">For any of these entity types, you want to filter and return only those entities that have a given text inside one of their `string` fields.</span></span> <span data-ttu-id="6b494-152">对于 `Person`，你希望搜索 `FirstName` 和 `LastName` 属性：</span><span class="sxs-lookup"><span data-stu-id="6b494-152">For `Person`, you'd want to search the `FirstName` and `LastName` properties:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="PersonsQry":::

<span data-ttu-id="6b494-153">但对于 `Car`，你希望仅搜索 `Model` 属性：</span><span class="sxs-lookup"><span data-stu-id="6b494-153">but for `Car`, you'd want to search only the `Model` property:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="CarsQry":::

<span data-ttu-id="6b494-154">尽管可以为 `IQueryable(Of Person)` 编写一个自定义函数，并为 `IQueryable(Of Car)` 编写另一个自定义函数，但以下函数会将此筛选添加到任何现有查询，而不考虑特定的元素类型如何。</span><span class="sxs-lookup"><span data-stu-id="6b494-154">While you could write one custom function for `IQueryable(Of Person)` and another for `IQueryable(Of Car)`, the following function adds this filtering to any existing query, irrespective of the specific element type.</span></span>

### <a name="example"></a><span data-ttu-id="6b494-155">示例</span><span class="sxs-lookup"><span data-stu-id="6b494-155">Example</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Factory_methods_expression_of_tdelegate":::

<span data-ttu-id="6b494-156">由于 `TextFilter` 函数采用并返回 [T) 的 IQueryable (](xref:System.Linq.IQueryable%601) (而不只是 <xref:System.Linq.IQueryable>) ，因此您可以在文本筛选器后添加更多的编译时类型的查询元素。</span><span class="sxs-lookup"><span data-stu-id="6b494-156">Because the `TextFilter` function takes and returns an [IQueryable(Of T)](xref:System.Linq.IQueryable%601) (and not just an <xref:System.Linq.IQueryable>), you can add further compile-time-typed query elements after the text filter.</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Factory_methods_expression_of_tdelegate_usage":::

## <a name="add-method-call-nodes-to-the-xrefsystemlinqiqueryables-expression-tree"></a><span data-ttu-id="6b494-157">将方法调用节点添加到 <xref:System.Linq.IQueryable> 的表达式树</span><span class="sxs-lookup"><span data-stu-id="6b494-157">Add method call nodes to the <xref:System.Linq.IQueryable>'s expression tree</span></span>

<span data-ttu-id="6b494-158">如果有 <xref:System.Linq.IQueryable> 而不是 [ (T) 的 IQueryable ](xref:System.Linq.IQueryable%601)，则不能直接调用泛型 LINQ 方法。</span><span class="sxs-lookup"><span data-stu-id="6b494-158">If you have an <xref:System.Linq.IQueryable> instead of an [IQueryable(Of T)](xref:System.Linq.IQueryable%601), you can't directly call the generic LINQ methods.</span></span> <span data-ttu-id="6b494-159">一种替代方法是按上面所述构建内部表达式树，并在传入表达树时使用反射来调用适当的 LINQ 方法。</span><span class="sxs-lookup"><span data-stu-id="6b494-159">One alternative is to build the inner expression tree as above, and use reflection to invoke the appropriate LINQ method while passing in the expression tree.</span></span>

<span data-ttu-id="6b494-160">还可以通过在表示调用 LINQ 方法的 <xref:System.Linq.Expressions.MethodCallExpression> 中包装整个树来复制 LINQ 方法的功能：</span><span class="sxs-lookup"><span data-stu-id="6b494-160">You could also duplicate the LINQ method's functionality, by wrapping the entire tree in a <xref:System.Linq.Expressions.MethodCallExpression> which represents a call to the LINQ method:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Factory_methods_lambdaexpression":::

<span data-ttu-id="6b494-161">请注意，在这种情况下，你没有编译时 `T` 泛型占位符，因此你将使用 <xref:System.Linq.Expressions.Expression.Lambda%2A> 不需要编译时类型信息的重载，这将生成 <xref:System.Linq.Expressions.LambdaExpression> 而不是 [表达式 (TDelegate) ](xref:System.Linq.Expressions.Expression%601)。</span><span class="sxs-lookup"><span data-stu-id="6b494-161">Note that in this case you don't have a compile-time `T` generic placeholder, so you'll use the <xref:System.Linq.Expressions.Expression.Lambda%2A> overload which doesn't require compile-time type information, and which produces a <xref:System.Linq.Expressions.LambdaExpression> instead of an an [Expression(Of TDelegate)](xref:System.Linq.Expressions.Expression%601).</span></span>

## <a name="the-dynamic-linq-library"></a><span data-ttu-id="6b494-162">动态 LINQ 库</span><span class="sxs-lookup"><span data-stu-id="6b494-162">The Dynamic LINQ library</span></span>

<span data-ttu-id="6b494-163">使用工厂方法构造表达式树比较复杂；编写字符串较为容易。</span><span class="sxs-lookup"><span data-stu-id="6b494-163">Constructing expression trees using factory methods is relatively complex; it is easier to compose strings.</span></span> <span data-ttu-id="6b494-164">[动态 LINQ 库](https://dynamic-linq.net/)公开了 <xref:System.Linq.IQueryable> 上的一组扩展方法，这些方法对应于 <xref:System.Linq.Queryable> 上的标准 LINQ 方法，后者接受采用[特殊语法](https://dynamic-linq.net/expression-language)的字符串而不是表达式树。</span><span class="sxs-lookup"><span data-stu-id="6b494-164">The [Dynamic LINQ library](https://dynamic-linq.net/) exposes a set of extension methods on <xref:System.Linq.IQueryable> corresponding to the standard LINQ methods at <xref:System.Linq.Queryable>, and which accept strings in a [special syntax](https://dynamic-linq.net/expression-language) instead of expression trees.</span></span> <span data-ttu-id="6b494-165">该库基于字符串生成相应的表达式树，并可以返回生成的已转换 <xref:System.Linq.IQueryable>。</span><span class="sxs-lookup"><span data-stu-id="6b494-165">The library generates the appropriate expression tree from the string, and can return the resultant translated <xref:System.Linq.IQueryable>.</span></span>

<span data-ttu-id="6b494-166">例如，前面的示例 (包括表达式树构造) 可以重写，如下所示：</span><span class="sxs-lookup"><span data-stu-id="6b494-166">For instance, the previous example (including the expression tree construction) could be rewritten as follows:</span></span>

:::code language="vb" source="../../../../../samples/snippets/visualbasic/programming-guide/dynamic-linq-expression-trees/Program.vb" id="Dynamic_linq":::

## <a name="see-also"></a><span data-ttu-id="6b494-167">请参阅</span><span class="sxs-lookup"><span data-stu-id="6b494-167">See also</span></span>

- [<span data-ttu-id="6b494-168">表达式树 (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="6b494-168">Expression Trees (Visual Basic)</span></span>](index.md)
- [<span data-ttu-id="6b494-169">如何：执行表达式树 (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="6b494-169">How to: Execute Expression Trees (Visual Basic)</span></span>](how-to-execute-expression-trees.md)
