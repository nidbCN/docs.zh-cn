---
title: 基于运行时状态进行查询 (C#)
description: 介绍了可由代码用来根据运行时状态，通过改变 LINQ 方法调用或传入这些方法的表达式树来进行动态查询的各种技术。
ms.date: 02/11/2021
ms.assetid: 52cd44dd-a3ec-441e-b93a-4eca388119c7
ms.openlocfilehash: 1a3bbe8c9ba6752c541d69502b7c4ac020fd9708
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876716"
---
# <a name="querying-based-on-runtime-state-c"></a>基于运行时状态进行查询 (C#)

考虑针对数据源定义 <xref:System.Linq.IQueryable> 或 [IQueryable\<T>](<xref:System.Linq.IQueryable%601>) 的代码：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Initialize":::

每次运行此代码时，都将执行相同的确切查询。 这通常不是很有用，因为你可能希望代码根据运行时的情况执行不同的查询。 本文介绍如何根据运行时状态执行不同的查询。

## <a name="iqueryable--iqueryablet-and-expression-trees"></a>IQueryable/IQueryable\<T> 和表达式树

从根本上讲，<xref:System.Linq.IQueryable> 有两个组件：

* <xref:System.Linq.IQueryable.Expression> &mdash; 当前查询的组件的与语言和数据源无关的表示形式，以表达式树的形式表示。
* <xref:System.Linq.IQueryable.Provider> &mdash; LINQ 提供程序的实例，它知道如何将当前查询具体化为一个值或一组值。

在动态查询的上下文中，提供程序通常会保持不变；查询的表达式树将因查询而异。

表达式树是不可变的；如果需要不同的表达式树 &mdash; 并因此需要不同的查询 &mdash; 则需要将现有表达式树转换为新的表达式树，从而转换为新的 <xref:System.Linq.IQueryable>。

以下各部分介绍了根据运行时状态，以不同方式进行查询的具体技术：

- 从表达式树中使用运行时状态
- 调用其他 LINQ 方法
- 改变传入到 LINQ 方法的表达式树
- 使用 <xref:System.Linq.Expressions.Expression> 中的工厂方法构造 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601) 表达式树
- 将方法调用节点添加到 <xref:System.Linq.IQueryable> 的表达式树
- 构造字符串，并使用 [动态 LINQ 库](https://dynamic-linq.net/)

## <a name="use-runtime-state-from-within-the-expression-tree"></a>从表达式树中使用运行时状态

假设 LINQ 提供程序支持，进行动态查询的最简单方式是通过封闭的变量（如以下代码示例中的 `length`）直接在查询中引用运行时状态：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Runtime_state_from_within_expression_tree":::

内部表达式树 &mdash; 以及查询 &mdash; 尚未修改；查询只返回不同的值，因为 `length` 的值已更改。

## <a name="call-additional-linq-methods"></a>调用其他 LINQ 方法

通常，<xref:System.Linq.Queryable> 的[内置 LINQ 方法](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Linq.Queryable/src/System/Linq/Queryable.cs)执行两个步骤：

* 在表示方法调用的 <xref:System.Linq.Expressions.MethodCallExpression> 中包装当前的表达式树。
* 将包装的表达式树传递回提供程序，以便通过提供程序的 <xref:System.Linq.IQueryProvider.Execute%2A?displayProperty=nameWithType> 方法返回值；或通过 <xref:System.Linq.IQueryProvider.CreateQuery%2A?displayProperty=nameWithType> 方法返回转换后的查询对象。

可以将原始查询替换为 [IQueryable\<T>](xref:System.Linq.IQueryable%601) 返回方法的结果，以获取新的查询。 可以基于运行时状态在一定条件下执行此操作，如以下示例中所示：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Added_method_calls":::

## <a name="vary-the-expression-tree-passed-into-the-linq-methods"></a>改变传入到 LINQ 方法的表达式树

可以将不同的表达式传入到 LINQ 方法，具体取决于运行时状态：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Varying_expressions":::

你可能还希望使用第三方库（如 [LinqKit](http://www.albahari.com/nutshell/linqkit.aspx) 的 [PredicateBuilder](http://www.albahari.com/nutshell/predicatebuilder.aspx)）来编写各种子表达式：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Compose_expressions":::

## <a name="construct-expression-trees-and-queries-using-factory-methods"></a>使用工厂方法构造表达式树和查询

在到此为止的所有示例中，我们已知道编译时的元素类型 &mdash;`string`&mdash; 并因此知道查询的类型 &mdash; `IQueryable<string>`。 可能需要将组件添加到任何元素类型的查询中，或者添加不同的组件，具体取决于元素类型。 可以使用 <xref:System.Linq.Expressions.Expression?displayProperty=fullName> 的工厂方法从头开始创建表达式树，从而在运行时将表达式定制为特定的元素类型。

### <a name="constructing-an-expressiontdelegate"></a>构造 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601)

构造要传入到某个 LINQ 方法的表达式时，实际上是在构造 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601) 的实例，其中 `TDelegate` 是某个委托类型，例如 `Func<string, bool>`、`Action` 或自定义委托类型。

[Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601) 继承自 <xref:System.Linq.Expressions.LambdaExpression>，后者表示完整的 lambda 表达式，如下所示：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Compiler_generated_expression_tree":::

<xref:System.Linq.Expressions.LambdaExpression> 具有两个组件：

* 参数列表 &mdash;`(string x)`&mdash; 由 <xref:System.Linq.Expressions.LambdaExpression.Parameters> 属性表示
* 主体 &mdash;`x.StartsWith("a")`&mdash; 由 <xref:System.Linq.Expressions.LambdaExpression.Body> 属性表示。

构造 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601) 的基本步骤如下所示：

* 使用 <xref:System.Linq.Expressions.Expression.Parameter%2A> 工厂方法为 lambda 表达式中的每个参数（如果有）定义 <xref:System.Linq.Expressions.ParameterExpression> 的对象。

    :::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Factory_method_expression_tree_parameter":::

* 使用你定义的 <xref:System.Linq.Expressions.ParameterExpression> 和 <xref:System.Linq.Expressions.Expression> 的工厂方法来构造 <xref:System.Linq.Expressions.LambdaExpression> 的主体。 例如，表示 `x.StartsWith("a")` 的表达式的构造方式如下：

    :::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Factory_method_expression_tree_body":::

* 使用适当的 <xref:System.Linq.Expressions.Expression.Lambda%2A> 工厂方法重载，将参数和主体包装到编译时类型的 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601) 中：

    :::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Factory_method_expression_tree_lambda":::

以下部分介绍了一种方案（在该方案中，你可能需要构造要传递到 LINQ 方法中的 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601)），并提供了使用工厂方法完成此操作的完整示例。

### <a name="scenario"></a>方案

假设你有多个实体类型：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Entities":::

对于这些实体类型中的任何一个，你都需要筛选并仅返回那些在其某个 `string` 字段内具有给定文本的实体。 对于 `Person`，你希望搜索 `FirstName` 和 `LastName` 属性：

```csharp
string term = /* ... */;
var personsQry = new List<Person>()
    .AsQueryable()
    .Where(x => x.FirstName.Contains(term) || x.LastName.Contains(term));
```

但对于 `Car`，你希望仅搜索 `Model` 属性：

```csharp
string term = /* ... */;
var carsQry = new List<Car>()
    .AsQueryable()
    .Where(x => x.Model.Contains(term));
```

尽管可以为 `IQueryable<Person>` 编写一个自定义函数，并为 `IQueryable<Car>` 编写另一个自定义函数，但以下函数会将此筛选添加到任何现有查询，而不考虑特定的元素类型如何。

### <a name="example"></a>示例

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Factory_methods_expression_of_tdelegate":::

由于 `TextFilter` 函数采用并返回 [IQueryable\<T>](xref:System.Linq.IQueryable%601)（而不仅仅是 <xref:System.Linq.IQueryable>），因此你可以在文本筛选器后添加更多的编译时类型的查询元素。

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Factory_methods_expression_of_tdelegate_usage":::

## <a name="add-method-call-nodes-to-the-xrefsystemlinqiqueryables-expression-tree"></a>将方法调用节点添加到 <xref:System.Linq.IQueryable> 的表达式树

如果你有 <xref:System.Linq.IQueryable>（而不是 [IQueryable\<T>](xref:System.Linq.IQueryable%601)），则不能直接调用泛型 LINQ 方法。 一种替代方法是按上面所述构建内部表达式树，并在传入表达树时使用反射来调用适当的 LINQ 方法。

还可以通过在表示调用 LINQ 方法的 <xref:System.Linq.Expressions.MethodCallExpression> 中包装整个树来复制 LINQ 方法的功能：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Factory_methods_lambdaexpression":::

请注意，在这种情况下，你没有编译时 `T` 泛型占位符，因此你将使用不需要编译时类型信息的 <xref:System.Linq.Expressions.Expression.Lambda%2A> 重载，这会生成 <xref:System.Linq.Expressions.LambdaExpression>，而不是 [Expression\<TDelegate>](xref:System.Linq.Expressions.Expression%601)。

## <a name="the-dynamic-linq-library"></a>动态 LINQ 库

使用工厂方法构造表达式树比较复杂；编写字符串较为容易。 [动态 LINQ 库](https://dynamic-linq.net/)公开了 <xref:System.Linq.IQueryable> 上的一组扩展方法，这些方法对应于 <xref:System.Linq.Queryable> 上的标准 LINQ 方法，后者接受采用[特殊语法](https://dynamic-linq.net/expression-language)的字符串而不是表达式树。 该库基于字符串生成相应的表达式树，并可以返回生成的已转换 <xref:System.Linq.IQueryable>。

例如，可以重新编写上一示例，如下所示：

:::code language="csharp" source="../../../../../samples/snippets/csharp/programming-guide/dynamic-linq-expression-trees/Program.cs" id="Dynamic_linq":::

## <a name="see-also"></a>请参阅

- [表达式树 (C#)](./index.md)
- [如何执行表达式树 (C#)](./how-to-execute-expression-trees.md)
- [在运行时动态指定谓词筛选器](../../../linq/dynamically-specify-predicate-filters-at-runtime.md)
