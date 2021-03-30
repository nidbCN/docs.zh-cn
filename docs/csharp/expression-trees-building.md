---
title: 生成表达式树
description: 了解生成表达式树的方法。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 542754a9-7f40-4293-b299-b9f80241902c
ms.openlocfilehash: f6752879dc01080e056221b00ca5377a6abc20db
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875806"
---
# <a name="building-expression-trees"></a>生成表达式树

[上一步 - 解释表达式](expression-trees-interpreting.md)

到目前为止，你所看到的所有表达式树都是由 C# 编译器创建的。 你所要做的是创建一个 lambda 表达式，将其分配给一个类型为 `Expression<Func<T>>` 或某种相似类型的变量。 这不是创建表达式树的唯一方法。 很多情况下，可能需要在运行时在内存中生成一个表达式。

由于这些表达式树是不可变的，所以生成表达式树很复杂。 不可变意味着必须以从叶到根的方式生成表达式树。 用于生成表达式树的 API 体现了这一点：用于生成节点的方法将其所有子级用作参数。 让我们通过几个示例来了解相关技巧。

## <a name="creating-nodes"></a>创建节点

让我们再次从相对简单的内容开始。 我们将使用在这些部分中一直使用的加法表达式：

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

若要构造该表达式树，必须构造叶节点。
叶节点是常量，因此可以使用 `Expression.Constant` 方法创建节点：

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
```

接下来，将生成加法表达式：

```csharp
var addition = Expression.Add(one, two);
```

一旦获得了加法表达式，就可以创建 lambda 表达式：

```csharp
var lambda = Expression.Lambda(addition);
```

这是一个非常简单的 Lambda 表达式，因为它不包含任何参数。
在本节的后续部分，你将了解如何将实参映射到形参并生成更复杂的表达式。

对于此类简单的表达式，可以将所有调用合并到单个语句中：

```csharp
var lambda = Expression.Lambda(
    Expression.Add(
        Expression.Constant(1, typeof(int)),
        Expression.Constant(2, typeof(int))
    )
);
```

## <a name="building-a-tree"></a>生成树

这是在内存中生成表达式树的基础知识。 更复杂的树通常意味着更多的节点类型，并且树中有更多的节点。 让我们再浏览一个示例，了解通常在创建表达式树时创建的其他两个节点类型：参数节点和方法调用节点。

生成一个表达式树以创建此表达式：

```csharp
Expression<Func<double, double, double>> distanceCalc =
    (x, y) => Math.Sqrt(x * x + y * y);
```

首先，创建 `x` 和 `y` 的参数表达式：

```csharp
var xParameter = Expression.Parameter(typeof(double), "x");
var yParameter = Expression.Parameter(typeof(double), "y");
```

按照你所看到的模式创建乘法和加法表达式：

```csharp
var xSquared = Expression.Multiply(xParameter, xParameter);
var ySquared = Expression.Multiply(yParameter, yParameter);
var sum = Expression.Add(xSquared, ySquared);
```

接下来，需要为调用 `Math.Sqrt` 创建方法调用表达式。

```csharp
var sqrtMethod = typeof(Math).GetMethod("Sqrt", new[] { typeof(double) });
var distance = Expression.Call(sqrtMethod, sum);
```

最后，将方法调用放入 lambda 表达式，并确保定义 lambda 表达式的参数：

```csharp
var distanceLambda = Expression.Lambda(
    distance,
    xParameter,
    yParameter);
```

在这个更复杂的示例中，你看到了创建表达式树通常使用的其他几种技巧。

首先，在使用它们之前，需要创建表示参数或局部变量的对象。 创建这些对象后，可以在表达式树中任何需要的位置使用它们。

其次，需要使用反射 API 的一个子集来创建 `MethodInfo` 对象，以便创建表达式树以访问该方法。 必须仅限于 .NET Core 平台上提供的反射 API 的子集。 同样，这些技术将扩展到其他表达式树。

## <a name="building-code-in-depth"></a>深度生成代码

不仅限于使用这些 API 可以生成的代码。 但是，要生成的表达式树越复杂，代码就越难以管理和阅读。

让我们生成一个与此代码等效的表达式树：

```csharp
Func<int, int> factorialFunc = (n) =>
{
    var res = 1;
    while (n > 1)
    {
        res = res * n;
        n--;
    }
    return res;
};
```

请注意上面我未生成表达式树，只是生成了委托。 使用 `Expression` 类不能生成语句 lambda。 下面是生成相同的功能所需的代码。 它很复杂，这是因为没有用于生成 `while` 循环的 API，而是需要生成一个包含条件测试的循环和一个用于中断循环的标签目标。

```csharp
var nArgument = Expression.Parameter(typeof(int), "n");
var result = Expression.Variable(typeof(int), "result");

// Creating a label that represents the return value
LabelTarget label = Expression.Label(typeof(int));

var initializeResult = Expression.Assign(result, Expression.Constant(1));

// This is the inner block that performs the multiplication,
// and decrements the value of 'n'
var block = Expression.Block(
    Expression.Assign(result,
        Expression.Multiply(result, nArgument)),
    Expression.PostDecrementAssign(nArgument)
);

// Creating a method body.
BlockExpression body = Expression.Block(
    new[] { result },
    initializeResult,
    Expression.Loop(
        Expression.IfThenElse(
            Expression.GreaterThan(nArgument, Expression.Constant(1)),
            block,
            Expression.Break(label, result)
        ),
        label
    )
);
```

用于生成阶乘函数的表达式树的代码相对更长、更复杂，它充满了标签和 break 语句以及我们在日常编码任务中想要避免的其他元素。

在本部分中，我还更新了用于访问此表达式树中所有节点的访客代码，并编写了在此示例中创建的节点的相关信息。 可以在 dotnet/docs GitHub 存储库[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/csharp/expression-trees)。 生成并运行这些示例，自行动手试验。 有关下载说明，请参阅[示例和教程](../samples-and-tutorials/index.md#view-and-download-samples)。

## <a name="examining-the-apis"></a>检查 API

表达式树 API 在 .NET Core 中较难导航，但没关系。 它们的用途相当复杂：编写在运行时生成代码的代码。 它们必须具有复杂的结构，才能在支持 C# 语言中提供的所有控件结构和尽可能减小 API 表面积之间保持平衡。 这种平衡意味着许多控件结构不是由其 C# 构造表示，而是由表示基础逻辑的构造表示，这些基础逻辑由编译器从这些较高级别的构造生成。

另外，此时存在一些不能通过使用 `Expression` 类方法直接生成的 C# 表达式。 一般来说，这些将是在 C# 5 和 C# 6 中添加的最新运算符和表达式。 （例如，无法生成 `async` 表达式，并且无法直接创建新 `?.` 运算符。）

[下一步 - 转换表达式](expression-trees-translating.md)
