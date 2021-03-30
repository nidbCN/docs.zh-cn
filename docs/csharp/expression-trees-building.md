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
# <a name="building-expression-trees"></a><span data-ttu-id="7a3a3-103">生成表达式树</span><span class="sxs-lookup"><span data-stu-id="7a3a3-103">Building Expression Trees</span></span>

[<span data-ttu-id="7a3a3-104">上一步 - 解释表达式</span><span class="sxs-lookup"><span data-stu-id="7a3a3-104">Previous -- Interpreting Expressions</span></span>](expression-trees-interpreting.md)

<span data-ttu-id="7a3a3-105">到目前为止，你所看到的所有表达式树都是由 C# 编译器创建的。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-105">All the expression trees you've seen so far have been created by the C# compiler.</span></span> <span data-ttu-id="7a3a3-106">你所要做的是创建一个 lambda 表达式，将其分配给一个类型为 `Expression<Func<T>>` 或某种相似类型的变量。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-106">All you had to do was create a lambda expression that was assigned to a variable typed as an `Expression<Func<T>>` or some similar type.</span></span> <span data-ttu-id="7a3a3-107">这不是创建表达式树的唯一方法。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-107">That's not the only way to create an expression tree.</span></span> <span data-ttu-id="7a3a3-108">很多情况下，可能需要在运行时在内存中生成一个表达式。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-108">For many scenarios you may find that you need to build an expression in memory at runtime.</span></span>

<span data-ttu-id="7a3a3-109">由于这些表达式树是不可变的，所以生成表达式树很复杂。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-109">Building Expression Trees is complicated by the fact that those expression trees are immutable.</span></span> <span data-ttu-id="7a3a3-110">不可变意味着必须以从叶到根的方式生成表达式树。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-110">Being immutable means that you must build the tree from the leaves up to the root.</span></span> <span data-ttu-id="7a3a3-111">用于生成表达式树的 API 体现了这一点：用于生成节点的方法将其所有子级用作参数。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-111">The APIs you'll use to build expression trees reflect this fact: The methods you'll use to build a node take all its children as arguments.</span></span> <span data-ttu-id="7a3a3-112">让我们通过几个示例来了解相关技巧。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-112">Let's walk through a few examples to show you the techniques.</span></span>

## <a name="creating-nodes"></a><span data-ttu-id="7a3a3-113">创建节点</span><span class="sxs-lookup"><span data-stu-id="7a3a3-113">Creating Nodes</span></span>

<span data-ttu-id="7a3a3-114">让我们再次从相对简单的内容开始。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-114">Let's start relatively simply again.</span></span> <span data-ttu-id="7a3a3-115">我们将使用在这些部分中一直使用的加法表达式：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-115">We'll use the addition expression I've been working with throughout these sections:</span></span>

```csharp
Expression<Func<int>> sum = () => 1 + 2;
```

<span data-ttu-id="7a3a3-116">若要构造该表达式树，必须构造叶节点。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-116">To construct that expression tree, you must construct the leaf nodes.</span></span>
<span data-ttu-id="7a3a3-117">叶节点是常量，因此可以使用 `Expression.Constant` 方法创建节点：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-117">The leaf nodes are constants, so you can use the `Expression.Constant` method to create the nodes:</span></span>

```csharp
var one = Expression.Constant(1, typeof(int));
var two = Expression.Constant(2, typeof(int));
```

<span data-ttu-id="7a3a3-118">接下来，将生成加法表达式：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-118">Next, you'll build the addition expression:</span></span>

```csharp
var addition = Expression.Add(one, two);
```

<span data-ttu-id="7a3a3-119">一旦获得了加法表达式，就可以创建 lambda 表达式：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-119">Once you've got the addition expression, you can create the lambda expression:</span></span>

```csharp
var lambda = Expression.Lambda(addition);
```

<span data-ttu-id="7a3a3-120">这是一个非常简单的 Lambda 表达式，因为它不包含任何参数。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-120">This is a very simple lambda expression, because it contains no arguments.</span></span>
<span data-ttu-id="7a3a3-121">在本节的后续部分，你将了解如何将实参映射到形参并生成更复杂的表达式。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-121">Later in this section, you'll see how to map arguments to parameters and build more complicated expressions.</span></span>

<span data-ttu-id="7a3a3-122">对于此类简单的表达式，可以将所有调用合并到单个语句中：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-122">For expressions that are as simple as this one, you may combine all the calls into a single statement:</span></span>

```csharp
var lambda = Expression.Lambda(
    Expression.Add(
        Expression.Constant(1, typeof(int)),
        Expression.Constant(2, typeof(int))
    )
);
```

## <a name="building-a-tree"></a><span data-ttu-id="7a3a3-123">生成树</span><span class="sxs-lookup"><span data-stu-id="7a3a3-123">Building a Tree</span></span>

<span data-ttu-id="7a3a3-124">这是在内存中生成表达式树的基础知识。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-124">That's the basics of building an expression tree in memory.</span></span> <span data-ttu-id="7a3a3-125">更复杂的树通常意味着更多的节点类型，并且树中有更多的节点。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-125">More complex trees generally mean more node types, and more nodes in the tree.</span></span> <span data-ttu-id="7a3a3-126">让我们再浏览一个示例，了解通常在创建表达式树时创建的其他两个节点类型：参数节点和方法调用节点。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-126">Let's run through one more example and show two more node types that you will typically build when you create expression trees: the argument nodes, and method call nodes.</span></span>

<span data-ttu-id="7a3a3-127">生成一个表达式树以创建此表达式：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-127">Let's build an expression tree to create this expression:</span></span>

```csharp
Expression<Func<double, double, double>> distanceCalc =
    (x, y) => Math.Sqrt(x * x + y * y);
```

<span data-ttu-id="7a3a3-128">首先，创建 `x` 和 `y` 的参数表达式：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-128">You'll start by creating parameter expressions for `x` and `y`:</span></span>

```csharp
var xParameter = Expression.Parameter(typeof(double), "x");
var yParameter = Expression.Parameter(typeof(double), "y");
```

<span data-ttu-id="7a3a3-129">按照你所看到的模式创建乘法和加法表达式：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-129">Creating the multiplication and addition expressions follows the pattern you've already seen:</span></span>

```csharp
var xSquared = Expression.Multiply(xParameter, xParameter);
var ySquared = Expression.Multiply(yParameter, yParameter);
var sum = Expression.Add(xSquared, ySquared);
```

<span data-ttu-id="7a3a3-130">接下来，需要为调用 `Math.Sqrt` 创建方法调用表达式。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-130">Next, you need to create a method call expression for the call to `Math.Sqrt`.</span></span>

```csharp
var sqrtMethod = typeof(Math).GetMethod("Sqrt", new[] { typeof(double) });
var distance = Expression.Call(sqrtMethod, sum);
```

<span data-ttu-id="7a3a3-131">最后，将方法调用放入 lambda 表达式，并确保定义 lambda 表达式的参数：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-131">And  then finally, you put the method call into a lambda expression, and make sure to define the arguments to the lambda expression:</span></span>

```csharp
var distanceLambda = Expression.Lambda(
    distance,
    xParameter,
    yParameter);
```

<span data-ttu-id="7a3a3-132">在这个更复杂的示例中，你看到了创建表达式树通常使用的其他几种技巧。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-132">In this more complicated example, you see a couple more techniques that you will often need to create expression trees.</span></span>

<span data-ttu-id="7a3a3-133">首先，在使用它们之前，需要创建表示参数或局部变量的对象。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-133">First, you need to create the objects that represent parameters or local variables before you use them.</span></span> <span data-ttu-id="7a3a3-134">创建这些对象后，可以在表达式树中任何需要的位置使用它们。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-134">Once you've created those objects, you can use them in your expression tree wherever you need.</span></span>

<span data-ttu-id="7a3a3-135">其次，需要使用反射 API 的一个子集来创建 `MethodInfo` 对象，以便创建表达式树以访问该方法。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-135">Second, you need to use a subset of the Reflection APIs to create a `MethodInfo` object so that you can create an expression tree to access that method.</span></span> <span data-ttu-id="7a3a3-136">必须仅限于 .NET Core 平台上提供的反射 API 的子集。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-136">You must limit yourself to the subset of the Reflection APIs that are available on the .NET Core platform.</span></span> <span data-ttu-id="7a3a3-137">同样，这些技术将扩展到其他表达式树。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-137">Again, these techniques will extend to other expression trees.</span></span>

## <a name="building-code-in-depth"></a><span data-ttu-id="7a3a3-138">深度生成代码</span><span class="sxs-lookup"><span data-stu-id="7a3a3-138">Building Code In Depth</span></span>

<span data-ttu-id="7a3a3-139">不仅限于使用这些 API 可以生成的代码。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-139">You aren't limited in what you can build using these APIs.</span></span> <span data-ttu-id="7a3a3-140">但是，要生成的表达式树越复杂，代码就越难以管理和阅读。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-140">However, the more complicated expression tree that you want to build, the more difficult the code is to manage and to read.</span></span>

<span data-ttu-id="7a3a3-141">让我们生成一个与此代码等效的表达式树：</span><span class="sxs-lookup"><span data-stu-id="7a3a3-141">Let's build an expression tree that is the equivalent of this code:</span></span>

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

<span data-ttu-id="7a3a3-142">请注意上面我未生成表达式树，只是生成了委托。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-142">Notice above that I did not build the expression tree, but simply the delegate.</span></span> <span data-ttu-id="7a3a3-143">使用 `Expression` 类不能生成语句 lambda。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-143">Using the `Expression` class, you can't build statement lambdas.</span></span> <span data-ttu-id="7a3a3-144">下面是生成相同的功能所需的代码。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-144">Here's the code that is required to build the same functionality.</span></span> <span data-ttu-id="7a3a3-145">它很复杂，这是因为没有用于生成 `while` 循环的 API，而是需要生成一个包含条件测试的循环和一个用于中断循环的标签目标。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-145">It's complicated by the fact that there isn't an API to build a `while` loop, instead you need to build a loop that contains a conditional test, and a label target to break out of the loop.</span></span>

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

<span data-ttu-id="7a3a3-146">用于生成阶乘函数的表达式树的代码相对更长、更复杂，它充满了标签和 break 语句以及我们在日常编码任务中想要避免的其他元素。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-146">The code to build the expression tree for the factorial function is quite a bit longer, more complicated, and it's riddled with labels and break statements and other elements we'd like to avoid in our everyday coding tasks.</span></span>

<span data-ttu-id="7a3a3-147">在本部分中，我还更新了用于访问此表达式树中所有节点的访客代码，并编写了在此示例中创建的节点的相关信息。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-147">For this section, I've also updated the visitor code to visit every node in this expression tree and write out information about the nodes that are created in this sample.</span></span> <span data-ttu-id="7a3a3-148">可以在 dotnet/docs GitHub 存储库[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/csharp/expression-trees)。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-148">You can [view or download the sample code](https://github.com/dotnet/samples/tree/main/csharp/expression-trees) at the dotnet/docs GitHub repository.</span></span> <span data-ttu-id="7a3a3-149">生成并运行这些示例，自行动手试验。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-149">Experiment for yourself by building and running the samples.</span></span> <span data-ttu-id="7a3a3-150">有关下载说明，请参阅[示例和教程](../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-150">For download instructions, see [Samples and Tutorials](../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

## <a name="examining-the-apis"></a><span data-ttu-id="7a3a3-151">检查 API</span><span class="sxs-lookup"><span data-stu-id="7a3a3-151">Examining the APIs</span></span>

<span data-ttu-id="7a3a3-152">表达式树 API 在 .NET Core 中较难导航，但没关系。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-152">The expression tree APIs are some of the more difficult to navigate in .NET Core, but that's fine.</span></span> <span data-ttu-id="7a3a3-153">它们的用途相当复杂：编写在运行时生成代码的代码。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-153">Their purpose is a rather complex undertaking: writing code that generates code at runtime.</span></span> <span data-ttu-id="7a3a3-154">它们必须具有复杂的结构，才能在支持 C# 语言中提供的所有控件结构和尽可能减小 API 表面积之间保持平衡。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-154">They are necessarily complicated to provide a balance between supporting all the control structures available in the C# language and keeping the surface area of the APIs as small as reasonable.</span></span> <span data-ttu-id="7a3a3-155">这种平衡意味着许多控件结构不是由其 C# 构造表示，而是由表示基础逻辑的构造表示，这些基础逻辑由编译器从这些较高级别的构造生成。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-155">This balance means that many control structures are represented not by their C# constructs, but by constructs that represent the underlying logic that the compiler generates from these higher level constructs.</span></span>

<span data-ttu-id="7a3a3-156">另外，此时存在一些不能通过使用 `Expression` 类方法直接生成的 C# 表达式。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-156">Also, at this time, there are C# expressions that cannot be built directly using `Expression` class methods.</span></span> <span data-ttu-id="7a3a3-157">一般来说，这些将是在 C# 5 和 C# 6 中添加的最新运算符和表达式。</span><span class="sxs-lookup"><span data-stu-id="7a3a3-157">In general, these will be the newest operators and expressions added in C# 5 and C# 6.</span></span> <span data-ttu-id="7a3a3-158">（例如，无法生成 `async` 表达式，并且无法直接创建新 `?.` 运算符。）</span><span class="sxs-lookup"><span data-stu-id="7a3a3-158">(For example, `async` expressions cannot be built, and the new `?.` operator cannot be directly created.)</span></span>

[<span data-ttu-id="7a3a3-159">下一步 - 转换表达式</span><span class="sxs-lookup"><span data-stu-id="7a3a3-159">Next -- Translating Expressions</span></span>](expression-trees-translating.md)
