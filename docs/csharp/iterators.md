---
title: 迭代器
description: 了解如何使用内置 C# 迭代器以及如何创建自己的自定义迭代器方法。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 5cf36f45-f91a-4fca-a0b7-87f233e108e9
ms.openlocfilehash: 357f05f6ae1a4cec7566ddc821036354c932af90
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872634"
---
# <a name="iterators"></a><span data-ttu-id="5c30c-103">迭代器</span><span class="sxs-lookup"><span data-stu-id="5c30c-103">Iterators</span></span>

<span data-ttu-id="5c30c-104">编写的几乎每个程序都需要循环访问集合。</span><span class="sxs-lookup"><span data-stu-id="5c30c-104">Almost every program you write will have some need to iterate over a collection.</span></span> <span data-ttu-id="5c30c-105">因此需要编写代码来检查集合中的每一项。</span><span class="sxs-lookup"><span data-stu-id="5c30c-105">You'll write code that examines every item in a collection.</span></span>

<span data-ttu-id="5c30c-106">还需创建迭代器方法，这些方法可为该类的元素生成迭代器（该对象遍历容器，尤其是列表）。</span><span class="sxs-lookup"><span data-stu-id="5c30c-106">You'll also create iterator methods which are methods that produces an iterator (which is an object that traverses a container, particularly lists) for the elements of that class.</span></span> <span data-ttu-id="5c30c-107">这些方法可用于：</span><span class="sxs-lookup"><span data-stu-id="5c30c-107">These can be used for:</span></span>

+ <span data-ttu-id="5c30c-108">对集合中的每个项执行操作。</span><span class="sxs-lookup"><span data-stu-id="5c30c-108">Performing an action on each item in a collection.</span></span>
+ <span data-ttu-id="5c30c-109">枚举自定义集合。</span><span class="sxs-lookup"><span data-stu-id="5c30c-109">Enumerating a custom collection.</span></span>
+ <span data-ttu-id="5c30c-110">扩展 [LINQ](linq/index.md) 或其他库。</span><span class="sxs-lookup"><span data-stu-id="5c30c-110">Extending [LINQ](linq/index.md) or other libraries.</span></span>
+ <span data-ttu-id="5c30c-111">创建数据管道，以便数据通过迭代器方法在管道中有效流动。</span><span class="sxs-lookup"><span data-stu-id="5c30c-111">Creating a data pipeline where data flows efficiently through iterator methods.</span></span>

<span data-ttu-id="5c30c-112">C# 语言提供了适用于这两种方案的功能。</span><span class="sxs-lookup"><span data-stu-id="5c30c-112">The C# language provides features for both these scenarios.</span></span> <span data-ttu-id="5c30c-113">本文概述了这些功能。</span><span class="sxs-lookup"><span data-stu-id="5c30c-113">This article provides an overview of those features.</span></span>

<span data-ttu-id="5c30c-114">在此教程中，将执行多步操作。</span><span class="sxs-lookup"><span data-stu-id="5c30c-114">This tutorial has multiple steps.</span></span> <span data-ttu-id="5c30c-115">执行每步操作后，都可以运行应用程序，并查看进度。</span><span class="sxs-lookup"><span data-stu-id="5c30c-115">After each step, you can run the application and see the progress.</span></span> <span data-ttu-id="5c30c-116">还可以[查看或下载本主题的已完成示例](https://github.com/dotnet/samples/blob/main/csharp/iterators)。</span><span class="sxs-lookup"><span data-stu-id="5c30c-116">You can also [view or download the completed sample](https://github.com/dotnet/samples/blob/main/csharp/iterators) for this topic.</span></span> <span data-ttu-id="5c30c-117">有关下载说明，请参阅[示例和教程](../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="5c30c-117">For download instructions, see [Samples and Tutorials](../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

## <a name="iterating-with-foreach"></a><span data-ttu-id="5c30c-118">使用 foreach 执行循环访问</span><span class="sxs-lookup"><span data-stu-id="5c30c-118">Iterating with foreach</span></span>

<span data-ttu-id="5c30c-119">枚举集合非常简单：使用 `foreach` 关键字枚举集合，从而为集合中的每个元素执行一次嵌入语句：</span><span class="sxs-lookup"><span data-stu-id="5c30c-119">Enumerating a collection is simple: The `foreach` keyword enumerates a collection, executing the embedded statement once for each element in the collection:</span></span>

```csharp
foreach (var item in collection)
{
   Console.WriteLine(item.ToString());
}
```

<span data-ttu-id="5c30c-120">就这么简单。</span><span class="sxs-lookup"><span data-stu-id="5c30c-120">That's all there is to it.</span></span> <span data-ttu-id="5c30c-121">若要循环访问集合中的所有内容，只需使用 `foreach` 语句。</span><span class="sxs-lookup"><span data-stu-id="5c30c-121">To iterate over all the contents of a collection, the `foreach` statement is all you need.</span></span> <span data-ttu-id="5c30c-122">但 `foreach` 语句并非完美无缺。</span><span class="sxs-lookup"><span data-stu-id="5c30c-122">The `foreach` statement isn't magic, though.</span></span> <span data-ttu-id="5c30c-123">它依赖于 .NET Core 库中定义的 2 个泛型接口，才能生成循环访问集合所需的代码：`IEnumerable<T>` 和 `IEnumerator<T>`。</span><span class="sxs-lookup"><span data-stu-id="5c30c-123">It relies on two generic interfaces defined in the .NET core library in order to generate the code necessary to iterate a collection: `IEnumerable<T>` and `IEnumerator<T>`.</span></span> <span data-ttu-id="5c30c-124">下文对此机制进行了更详细说明。</span><span class="sxs-lookup"><span data-stu-id="5c30c-124">This mechanism is explained in more detail below.</span></span>

<span data-ttu-id="5c30c-125">这 2 种接口还具备相应的非泛型接口：`IEnumerable` 和 `IEnumerator`。</span><span class="sxs-lookup"><span data-stu-id="5c30c-125">Both of these interfaces also have non-generic counterparts: `IEnumerable` and `IEnumerator`.</span></span> <span data-ttu-id="5c30c-126">[泛型](programming-guide/generics/index.md)版本是新式代码的首要选项。</span><span class="sxs-lookup"><span data-stu-id="5c30c-126">The [generic](programming-guide/generics/index.md) versions are preferred for modern code.</span></span>

## <a name="enumeration-sources-with-iterator-methods"></a><span data-ttu-id="5c30c-127">使用迭代器方法的枚举源</span><span class="sxs-lookup"><span data-stu-id="5c30c-127">Enumeration sources with iterator methods</span></span>

<span data-ttu-id="5c30c-128">借助 C# 语言的另一个强大功能，能够生成创建枚举源的方法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-128">Another great feature of the C# language enables you to build methods that create a source for an enumeration.</span></span> <span data-ttu-id="5c30c-129">这些方法称为“迭代器方法”  。</span><span class="sxs-lookup"><span data-stu-id="5c30c-129">These are referred to as *iterator methods*.</span></span> <span data-ttu-id="5c30c-130">迭代器方法用于定义请求时如何在序列中生成对象。</span><span class="sxs-lookup"><span data-stu-id="5c30c-130">An iterator method defines how to generate the objects in a sequence when requested.</span></span> <span data-ttu-id="5c30c-131">使用 `yield return` 上下文关键字定义迭代器方法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-131">You use the `yield return` contextual keywords to define an iterator method.</span></span>

<span data-ttu-id="5c30c-132">可编写此方法以生成从 0 到 9 的整数序列：</span><span class="sxs-lookup"><span data-stu-id="5c30c-132">You could write this method to produce the sequence of integers from 0 through 9:</span></span>

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    yield return 0;
    yield return 1;
    yield return 2;
    yield return 3;
    yield return 4;
    yield return 5;
    yield return 6;
    yield return 7;
    yield return 8;
    yield return 9;
}
```

<span data-ttu-id="5c30c-133">上方的代码显示了不同的 `yield return` 语句，以强调可在迭代器方法中使用多个离散 `yield return` 语句这一事实。</span><span class="sxs-lookup"><span data-stu-id="5c30c-133">The code above shows distinct `yield return` statements to highlight the fact that you can use multiple discrete `yield return` statements in an iterator method.</span></span>
<span data-ttu-id="5c30c-134">可以使用其他语言构造来简化迭代器方法的代码，这也是一贯的做法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-134">You can (and often do) use other language constructs to simplify the code of an iterator method.</span></span> <span data-ttu-id="5c30c-135">以下方法定义可生成完全相同的数字序列：</span><span class="sxs-lookup"><span data-stu-id="5c30c-135">The method definition below produces the exact same sequence of numbers:</span></span>

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;
}
```

<span data-ttu-id="5c30c-136">不必从中选择一个。</span><span class="sxs-lookup"><span data-stu-id="5c30c-136">You don't have to decide one or the other.</span></span> <span data-ttu-id="5c30c-137">可根据需要提供尽可能多的 `yield return` 语句来满足方法需求：</span><span class="sxs-lookup"><span data-stu-id="5c30c-137">You can have as many `yield return` statements as necessary to meet the needs of your method:</span></span>

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    index = 100;
    while (index < 110)
        yield return index++;
}
```

<span data-ttu-id="5c30c-138">这是基本语法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-138">That's the basic syntax.</span></span> <span data-ttu-id="5c30c-139">我们来看一个需要编写迭代器方法的真实示例。</span><span class="sxs-lookup"><span data-stu-id="5c30c-139">Let's consider a real world example where you would write an iterator method.</span></span> <span data-ttu-id="5c30c-140">假设你正在处理一个 IoT 项目，设备传感器生成了大量数据流。</span><span class="sxs-lookup"><span data-stu-id="5c30c-140">Imagine you're on an IoT project and the device sensors generate a very large stream of data.</span></span> <span data-ttu-id="5c30c-141">为了获知数据，需要编写一个对每第 N 个数据元素进行采样的方法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-141">To get a feel for the data, you might write a method that samples every Nth data element.</span></span> <span data-ttu-id="5c30c-142">通过以下小迭代器方法可实现此目的：</span><span class="sxs-lookup"><span data-stu-id="5c30c-142">This small iterator method does the trick:</span></span>

```csharp
public static IEnumerable<T> Sample(this IEnumerable<T> sourceSequence, int interval)
{
    int index = 0;
    foreach (T item in sourceSequence)
    {
        if (index++ % interval == 0)
            yield return item;
    }
}
```

<span data-ttu-id="5c30c-143">迭代器方法有一个重要限制：在同一方法中不能同时使用 `return` 语句和 `yield return` 语句。</span><span class="sxs-lookup"><span data-stu-id="5c30c-143">There is one important restriction on iterator methods: you can't have both a `return` statement and a `yield return` statement in the same method.</span></span> <span data-ttu-id="5c30c-144">不会编译以下内容：</span><span class="sxs-lookup"><span data-stu-id="5c30c-144">The following will not compile:</span></span>

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    // generates a compile time error:
    var items = new int[] {100, 101, 102, 103, 104, 105, 106, 107, 108, 109 };
    return items;
}
```

<span data-ttu-id="5c30c-145">此限制通常不是问题。</span><span class="sxs-lookup"><span data-stu-id="5c30c-145">This restriction normally isn't a problem.</span></span> <span data-ttu-id="5c30c-146">可以选择在整个方法中使用 `yield return`，或选择将原始方法分成多个方法，一些使用 `return`，另一些使用 `yield return`。</span><span class="sxs-lookup"><span data-stu-id="5c30c-146">You have a choice of either using `yield return` throughout the method, or separating the original method into multiple methods, some using `return`, and some using `yield return`.</span></span>

<span data-ttu-id="5c30c-147">可略微修改一下最后一个方法，使其可在任何位置使用 `yield return`：</span><span class="sxs-lookup"><span data-stu-id="5c30c-147">You can modify the last method slightly to use `yield return` everywhere:</span></span>

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;

    yield return 50;

    var items = new int[] {100, 101, 102, 103, 104, 105, 106, 107, 108, 109 };
    foreach (var item in items)
        yield return item;
}
```

<span data-ttu-id="5c30c-148">有时，正确的做法是将迭代器方法拆分成 2 个不同的方法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-148">Sometimes, the right answer is to split an iterator method into two different methods.</span></span> <span data-ttu-id="5c30c-149">一个使用 `return`，另一个使用 `yield return`。</span><span class="sxs-lookup"><span data-stu-id="5c30c-149">One that uses `return`, and a second that uses `yield return`.</span></span> <span data-ttu-id="5c30c-150">考虑这样一种情况：需要基于布尔参数返回一个空集合，或者返回前 5 个奇数。</span><span class="sxs-lookup"><span data-stu-id="5c30c-150">Consider a situation where you might want to return an empty collection, or the first 5 odd numbers, based on a boolean argument.</span></span> <span data-ttu-id="5c30c-151">可编写类似以下 2 种方法的方法：</span><span class="sxs-lookup"><span data-stu-id="5c30c-151">You could write that as these two methods:</span></span>

```csharp
public IEnumerable<int> GetSingleDigitOddNumbers(bool getCollection)
{
    if (getCollection == false)
        return new int[0];
    else
        return IteratorMethod();
}

private IEnumerable<int> IteratorMethod()
{
    int index = 0;
    while (index < 10)
    {
        if (index % 2 == 1)
            yield return index;
        index++;
    }
}
```

<span data-ttu-id="5c30c-152">看看上面的方法。</span><span class="sxs-lookup"><span data-stu-id="5c30c-152">Look at the methods above.</span></span> <span data-ttu-id="5c30c-153">第 1 个方法使用标准 `return` 语句返回空集合，或返回第 2 个方法创建的迭代器。</span><span class="sxs-lookup"><span data-stu-id="5c30c-153">The first uses the standard `return` statement to return either an empty collection, or the iterator created by the second method.</span></span> <span data-ttu-id="5c30c-154">第 2 个方法使用 `yield return` 语句创建请求的序列。</span><span class="sxs-lookup"><span data-stu-id="5c30c-154">The second method uses the `yield return` statement to create the requested sequence.</span></span>

## <a name="deeper-dive-into-foreach"></a><span data-ttu-id="5c30c-155">深入了解 `foreach`</span><span class="sxs-lookup"><span data-stu-id="5c30c-155">Deeper Dive into `foreach`</span></span>

<span data-ttu-id="5c30c-156">`foreach` 语句可扩展为使用 `IEnumerable<T>` 和 `IEnumerator<T>` 接口的标准用语，以便循环访问集合中的所有元素。</span><span class="sxs-lookup"><span data-stu-id="5c30c-156">The `foreach` statement expands into a standard idiom that uses the `IEnumerable<T>` and `IEnumerator<T>` interfaces to iterate across all elements of a collection.</span></span> <span data-ttu-id="5c30c-157">还可最大限度减少开发人员因未正确管理资源所造成的错误。</span><span class="sxs-lookup"><span data-stu-id="5c30c-157">It also  minimizes errors developers make by not properly managing resources.</span></span>

<span data-ttu-id="5c30c-158">编译器将第 1 个示例中显示的 `foreach` 循环转换为类似于此构造的内容：</span><span class="sxs-lookup"><span data-stu-id="5c30c-158">The compiler translates the `foreach` loop shown in the first example into something similar to this construct:</span></span>

```csharp
IEnumerator<int> enumerator = collection.GetEnumerator();
while (enumerator.MoveNext())
{
    var item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

<span data-ttu-id="5c30c-159">上述构造表示由 C# 编译器版本 5 及更高版本生成的代码。</span><span class="sxs-lookup"><span data-stu-id="5c30c-159">The construct above represents the code generated by the C# compiler as of version 5 and above.</span></span> <span data-ttu-id="5c30c-160">在版本 5 之前，`item` 变量的范围有所不同：</span><span class="sxs-lookup"><span data-stu-id="5c30c-160">Prior to version 5, the `item` variable had a different scope:</span></span>

```csharp
// C# versions 1 through 4:
IEnumerator<int> enumerator = collection.GetEnumerator();
int item = default(int);
while (enumerator.MoveNext())
{
    item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

<span data-ttu-id="5c30c-161">此范围更改的原因在于：较早行为可能导致难以诊断出有关 Lambda 表达式的 bug。</span><span class="sxs-lookup"><span data-stu-id="5c30c-161">This was changed because the earlier behavior could lead to subtle and hard to diagnose bugs involving lambda expressions.</span></span> <span data-ttu-id="5c30c-162">若要详细了解 lambda 表达式，请参阅 [lambda 表达式](language-reference/operators/lambda-expressions.md)。</span><span class="sxs-lookup"><span data-stu-id="5c30c-162">For more information about lambda expressions, see [Lambda expressions](language-reference/operators/lambda-expressions.md).</span></span>

<span data-ttu-id="5c30c-163">编译器生成的确切代码更复杂一些，用于处理 `GetEnumerator()` 返回的对象实现 `IDisposable` 的情况。</span><span class="sxs-lookup"><span data-stu-id="5c30c-163">The exact code generated by the compiler is somewhat more complicated, and handles situations where the object returned by `GetEnumerator()` implements the `IDisposable` interface.</span></span> <span data-ttu-id="5c30c-164">完整扩展生成的代码更类似如下：</span><span class="sxs-lookup"><span data-stu-id="5c30c-164">The full expansion generates code more like this:</span></span>

```csharp
{
    var enumerator = collection.GetEnumerator();
    try
    {
        while (enumerator.MoveNext())
        {
            var item = enumerator.Current;
            Console.WriteLine(item.ToString());
        }
    } finally
    {
        // dispose of enumerator.
    }
}
```

<span data-ttu-id="5c30c-165">枚举器的释放方式取决于 `enumerator` 类型的特征。</span><span class="sxs-lookup"><span data-stu-id="5c30c-165">The manner in which the enumerator is disposed of depends on the characteristics of the type of `enumerator`.</span></span> <span data-ttu-id="5c30c-166">一般情况下，`finally` 子句扩展为：</span><span class="sxs-lookup"><span data-stu-id="5c30c-166">In the general case, the `finally` clause expands to:</span></span>

```csharp
finally
{
   (enumerator as IDisposable)?.Dispose();
}
```

<span data-ttu-id="5c30c-167">但是，如果 `enumerator` 的类型为已密封类型，并且不存在从类型 `enumerator` 到 `IDisposable` 的隐式转换，则 `finally` 子句扩展为一个空白块：</span><span class="sxs-lookup"><span data-stu-id="5c30c-167">However, if the type of `enumerator` is a sealed type and there is no implicit conversion from the type of `enumerator` to `IDisposable`, the `finally` clause expands to an empty block:</span></span>

```csharp
finally
{
}
```

<span data-ttu-id="5c30c-168">如果存在从类型 `enumerator` 到 `IDisposable` 的隐式转换，并且 `enumerator` 是不可为 null 的值类型，则 `finally` 子句扩展为：</span><span class="sxs-lookup"><span data-stu-id="5c30c-168">If there is an implicit conversion from the type of `enumerator` to `IDisposable`, and `enumerator` is a non-nullable value type, the `finally` clause expands to:</span></span>

```csharp
finally
{
   ((IDisposable)enumerator).Dispose();
}
```

<span data-ttu-id="5c30c-169">幸运地是，无需记住所有这些细节。</span><span class="sxs-lookup"><span data-stu-id="5c30c-169">Thankfully, you don't need to remember all these details.</span></span> <span data-ttu-id="5c30c-170">`foreach` 语句会为你处理所有这些细微差别。</span><span class="sxs-lookup"><span data-stu-id="5c30c-170">The `foreach` statement handles all those nuances for you.</span></span> <span data-ttu-id="5c30c-171">编译器会为所有这些构造生成正确的代码。</span><span class="sxs-lookup"><span data-stu-id="5c30c-171">The compiler will generate the correct code for any of these constructs.</span></span>
