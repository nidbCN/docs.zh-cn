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
# <a name="iterators"></a>迭代器

编写的几乎每个程序都需要循环访问集合。 因此需要编写代码来检查集合中的每一项。

还需创建迭代器方法，这些方法可为该类的元素生成迭代器（该对象遍历容器，尤其是列表）。 这些方法可用于：

+ 对集合中的每个项执行操作。
+ 枚举自定义集合。
+ 扩展 [LINQ](linq/index.md) 或其他库。
+ 创建数据管道，以便数据通过迭代器方法在管道中有效流动。

C# 语言提供了适用于这两种方案的功能。 本文概述了这些功能。

在此教程中，将执行多步操作。 执行每步操作后，都可以运行应用程序，并查看进度。 还可以[查看或下载本主题的已完成示例](https://github.com/dotnet/samples/blob/main/csharp/iterators)。 有关下载说明，请参阅[示例和教程](../samples-and-tutorials/index.md#view-and-download-samples)。

## <a name="iterating-with-foreach"></a>使用 foreach 执行循环访问

枚举集合非常简单：使用 `foreach` 关键字枚举集合，从而为集合中的每个元素执行一次嵌入语句：

```csharp
foreach (var item in collection)
{
   Console.WriteLine(item.ToString());
}
```

就这么简单。 若要循环访问集合中的所有内容，只需使用 `foreach` 语句。 但 `foreach` 语句并非完美无缺。 它依赖于 .NET Core 库中定义的 2 个泛型接口，才能生成循环访问集合所需的代码：`IEnumerable<T>` 和 `IEnumerator<T>`。 下文对此机制进行了更详细说明。

这 2 种接口还具备相应的非泛型接口：`IEnumerable` 和 `IEnumerator`。 [泛型](programming-guide/generics/index.md)版本是新式代码的首要选项。

## <a name="enumeration-sources-with-iterator-methods"></a>使用迭代器方法的枚举源

借助 C# 语言的另一个强大功能，能够生成创建枚举源的方法。 这些方法称为“迭代器方法”  。 迭代器方法用于定义请求时如何在序列中生成对象。 使用 `yield return` 上下文关键字定义迭代器方法。

可编写此方法以生成从 0 到 9 的整数序列：

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

上方的代码显示了不同的 `yield return` 语句，以强调可在迭代器方法中使用多个离散 `yield return` 语句这一事实。
可以使用其他语言构造来简化迭代器方法的代码，这也是一贯的做法。 以下方法定义可生成完全相同的数字序列：

```csharp
public IEnumerable<int> GetSingleDigitNumbers()
{
    int index = 0;
    while (index < 10)
        yield return index++;
}
```

不必从中选择一个。 可根据需要提供尽可能多的 `yield return` 语句来满足方法需求：

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

这是基本语法。 我们来看一个需要编写迭代器方法的真实示例。 假设你正在处理一个 IoT 项目，设备传感器生成了大量数据流。 为了获知数据，需要编写一个对每第 N 个数据元素进行采样的方法。 通过以下小迭代器方法可实现此目的：

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

迭代器方法有一个重要限制：在同一方法中不能同时使用 `return` 语句和 `yield return` 语句。 不会编译以下内容：

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

此限制通常不是问题。 可以选择在整个方法中使用 `yield return`，或选择将原始方法分成多个方法，一些使用 `return`，另一些使用 `yield return`。

可略微修改一下最后一个方法，使其可在任何位置使用 `yield return`：

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

有时，正确的做法是将迭代器方法拆分成 2 个不同的方法。 一个使用 `return`，另一个使用 `yield return`。 考虑这样一种情况：需要基于布尔参数返回一个空集合，或者返回前 5 个奇数。 可编写类似以下 2 种方法的方法：

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

看看上面的方法。 第 1 个方法使用标准 `return` 语句返回空集合，或返回第 2 个方法创建的迭代器。 第 2 个方法使用 `yield return` 语句创建请求的序列。

## <a name="deeper-dive-into-foreach"></a>深入了解 `foreach`

`foreach` 语句可扩展为使用 `IEnumerable<T>` 和 `IEnumerator<T>` 接口的标准用语，以便循环访问集合中的所有元素。 还可最大限度减少开发人员因未正确管理资源所造成的错误。

编译器将第 1 个示例中显示的 `foreach` 循环转换为类似于此构造的内容：

```csharp
IEnumerator<int> enumerator = collection.GetEnumerator();
while (enumerator.MoveNext())
{
    var item = enumerator.Current;
    Console.WriteLine(item.ToString());
}
```

上述构造表示由 C# 编译器版本 5 及更高版本生成的代码。 在版本 5 之前，`item` 变量的范围有所不同：

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

此范围更改的原因在于：较早行为可能导致难以诊断出有关 Lambda 表达式的 bug。 若要详细了解 lambda 表达式，请参阅 [lambda 表达式](language-reference/operators/lambda-expressions.md)。

编译器生成的确切代码更复杂一些，用于处理 `GetEnumerator()` 返回的对象实现 `IDisposable` 的情况。 完整扩展生成的代码更类似如下：

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

枚举器的释放方式取决于 `enumerator` 类型的特征。 一般情况下，`finally` 子句扩展为：

```csharp
finally
{
   (enumerator as IDisposable)?.Dispose();
}
```

但是，如果 `enumerator` 的类型为已密封类型，并且不存在从类型 `enumerator` 到 `IDisposable` 的隐式转换，则 `finally` 子句扩展为一个空白块：

```csharp
finally
{
}
```

如果存在从类型 `enumerator` 到 `IDisposable` 的隐式转换，并且 `enumerator` 是不可为 null 的值类型，则 `finally` 子句扩展为：

```csharp
finally
{
   ((IDisposable)enumerator).Dispose();
}
```

幸运地是，无需记住所有这些细节。 `foreach` 语句会为你处理所有这些细微差别。 编译器会为所有这些构造生成正确的代码。
