---
title: 索引器
description: 了解 C# 索引器以及它们如何实现索引属性，这些属性是使用一个或多个参数引用的属性。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 0e9496da-e766-45a9-b92b-91820d4a350e
ms.openlocfilehash: 2efd8735587c35be8e89dba7499a0a661796067d
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875780"
---
# <a name="indexers"></a><span data-ttu-id="02fbc-103">索引器</span><span class="sxs-lookup"><span data-stu-id="02fbc-103">Indexers</span></span>

<span data-ttu-id="02fbc-104">*索引器* 类似于属性。</span><span class="sxs-lookup"><span data-stu-id="02fbc-104">*Indexers* are similar to properties.</span></span> <span data-ttu-id="02fbc-105">很多时候，创建索引器与创建[属性](properties.md)所使用的编程语言特性是一样的。</span><span class="sxs-lookup"><span data-stu-id="02fbc-105">In many ways indexers build on the same language features as [properties](properties.md).</span></span> <span data-ttu-id="02fbc-106">索引器使属性可以被索引：使用一个或多个参数引用的属性。</span><span class="sxs-lookup"><span data-stu-id="02fbc-106">Indexers enable *indexed* properties: properties referenced using one or more arguments.</span></span> <span data-ttu-id="02fbc-107">这些参数为某些值集合提供索引。</span><span class="sxs-lookup"><span data-stu-id="02fbc-107">Those arguments provide an index into some collection of values.</span></span>

## <a name="indexer-syntax"></a><span data-ttu-id="02fbc-108">索引器语法</span><span class="sxs-lookup"><span data-stu-id="02fbc-108">Indexer Syntax</span></span>

<span data-ttu-id="02fbc-109">可以通过变量名和方括号访问索引器。</span><span class="sxs-lookup"><span data-stu-id="02fbc-109">You access an indexer through a variable name and square brackets.</span></span> <span data-ttu-id="02fbc-110">将索引器参数放在方括号内：</span><span class="sxs-lookup"><span data-stu-id="02fbc-110">You place the indexer arguments inside the brackets:</span></span>

```csharp
var item = someObject["key"];
someObject["AnotherKey"] = item;
```

<span data-ttu-id="02fbc-111">使用 `this` 关键字作为属性名声明索引器，并在方括号内声明参数。</span><span class="sxs-lookup"><span data-stu-id="02fbc-111">You declare indexers using the `this` keyword as the property name, and declaring the arguments within square brackets.</span></span> <span data-ttu-id="02fbc-112">此声明与前一段中所示的用法相匹配：</span><span class="sxs-lookup"><span data-stu-id="02fbc-112">This declaration would match the usage shown in the previous paragraph:</span></span>

```csharp
public int this[string key]
{
    get { return storage.Find(key); }
    set { storage.SetAt(key, value); }
}
```

<span data-ttu-id="02fbc-113">从最初的示例中，可以看到属性语法和索引器语法之间的关系。</span><span class="sxs-lookup"><span data-stu-id="02fbc-113">From this initial example, you can see the relationship between the syntax for properties and for indexers.</span></span> <span data-ttu-id="02fbc-114">此类比在索引器的大部分语法规则中进行。</span><span class="sxs-lookup"><span data-stu-id="02fbc-114">This analogy carries through most of the syntax rules for indexers.</span></span> <span data-ttu-id="02fbc-115">索引器可以使用任何有效的访问修饰符（public、protected internal、protected、internal、private 或 private protected）。</span><span class="sxs-lookup"><span data-stu-id="02fbc-115">Indexers can have any valid access modifiers (public, protected internal, protected, internal, private or private protected).</span></span> <span data-ttu-id="02fbc-116">它们可能是密封、虚拟或抽象的。</span><span class="sxs-lookup"><span data-stu-id="02fbc-116">They may be sealed, virtual, or abstract.</span></span> <span data-ttu-id="02fbc-117">与属性一样，可以在索引器中为 get 和 set 访问器指定不同的访问修饰符。</span><span class="sxs-lookup"><span data-stu-id="02fbc-117">As with properties, you can specify different access modifiers for the get and set accessors in an indexer.</span></span>
<span data-ttu-id="02fbc-118">你还可以指定只读索引器（忽略 set 访问器）或只写索引器（忽略 get 访问器）。</span><span class="sxs-lookup"><span data-stu-id="02fbc-118">You may also specify read-only indexers (by omitting the set accessor), or write-only indexers (by omitting the get accessor).</span></span>

<span data-ttu-id="02fbc-119">属性的各种用法同样适用于索引器。</span><span class="sxs-lookup"><span data-stu-id="02fbc-119">You can apply almost everything you learn from working with properties to indexers.</span></span> <span data-ttu-id="02fbc-120">此规则的唯一例外是“自动实现属性”。</span><span class="sxs-lookup"><span data-stu-id="02fbc-120">The only exception to that rule is *auto implemented properties*.</span></span> <span data-ttu-id="02fbc-121">编译器无法始终为索引器生成正确的存储。</span><span class="sxs-lookup"><span data-stu-id="02fbc-121">The compiler cannot always generate the correct storage for an indexer.</span></span>

<span data-ttu-id="02fbc-122">用于引用项的集合中的某个项的参数可区分索引器和属性。</span><span class="sxs-lookup"><span data-stu-id="02fbc-122">The presence of arguments to reference an item in a set of items distinguishes indexers from properties.</span></span> <span data-ttu-id="02fbc-123">只要每个索引器的参数列表是唯一的，就可以对一个类型定义多个索引器。</span><span class="sxs-lookup"><span data-stu-id="02fbc-123">You may define multiple indexers on a type, as long as the argument lists for each indexer is unique.</span></span> <span data-ttu-id="02fbc-124">让我们来探讨可能在类定义中使用一个或多个索引器的不同场景。</span><span class="sxs-lookup"><span data-stu-id="02fbc-124">Let's explore different scenarios where you might use one or more indexers in a class definition.</span></span>

## <a name="scenarios"></a><span data-ttu-id="02fbc-125">方案</span><span class="sxs-lookup"><span data-stu-id="02fbc-125">Scenarios</span></span>

<span data-ttu-id="02fbc-126">如果类型的 API 对集合进行建模，并且为集合定义了参数，则需要在此类型中定义索引器。</span><span class="sxs-lookup"><span data-stu-id="02fbc-126">You would define *indexers* in your type when its API models some collection where you define the arguments to that collection.</span></span> <span data-ttu-id="02fbc-127">索引器可能直接映射到属于 .NET Core 框架一部分的集合类型，也可能不。</span><span class="sxs-lookup"><span data-stu-id="02fbc-127">Your indexers may or may not map directly to the collection types that are part of the .NET core framework.</span></span> <span data-ttu-id="02fbc-128">除了对集合进行建模，类型还有其他职责。</span><span class="sxs-lookup"><span data-stu-id="02fbc-128">Your type may have other responsibilities in addition to modeling a collection.</span></span>
<span data-ttu-id="02fbc-129">通过索引器可提供与类型的抽象化匹配的 API，而无需公开如何存储或计算此抽象化的值的内部细节。</span><span class="sxs-lookup"><span data-stu-id="02fbc-129">Indexers enable you to provide the API that matches your type's abstraction without exposing the inner details of how the values for that abstraction are stored or computed.</span></span>

<span data-ttu-id="02fbc-130">让我们演练一些使用索引器的常见场景。</span><span class="sxs-lookup"><span data-stu-id="02fbc-130">Let's walk through some of the common scenarios for using *indexers*.</span></span> <span data-ttu-id="02fbc-131">可以访问[索引器的示例文件夹](https://github.com/dotnet/samples/tree/main/csharp/indexers)。</span><span class="sxs-lookup"><span data-stu-id="02fbc-131">You can access the [sample folder for indexers](https://github.com/dotnet/samples/tree/main/csharp/indexers).</span></span> <span data-ttu-id="02fbc-132">有关下载说明，请参阅[示例和教程](../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="02fbc-132">For download instructions, see [Samples and Tutorials](../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

### <a name="arrays-and-vectors"></a><span data-ttu-id="02fbc-133">数组和矢量</span><span class="sxs-lookup"><span data-stu-id="02fbc-133">Arrays and Vectors</span></span>

<span data-ttu-id="02fbc-134">创建索引器的一个最常见的场景是当类型对数组或矢量进行建模时。</span><span class="sxs-lookup"><span data-stu-id="02fbc-134">One of the most common scenarios for creating indexers is when your type models an array, or a vector.</span></span> <span data-ttu-id="02fbc-135">可以创建一个索引器用于对已排序的数据列表进行建模。</span><span class="sxs-lookup"><span data-stu-id="02fbc-135">You can create an indexer to model an ordered list of data.</span></span>

<span data-ttu-id="02fbc-136">创建自己的索引器的优点是你可以为集合定义存储以满足你的需求。</span><span class="sxs-lookup"><span data-stu-id="02fbc-136">The advantage of creating your own indexer is that you can define the storage for that collection to suit your needs.</span></span> <span data-ttu-id="02fbc-137">假设以下场景：类型对历史数据进行建模，并且此历史数据太大而无法立即加载到内存中。</span><span class="sxs-lookup"><span data-stu-id="02fbc-137">Imagine a scenario where your type models historical data that is too large to load into memory at once.</span></span> <span data-ttu-id="02fbc-138">需要根据使用情况加载和卸载集合的某些部分。</span><span class="sxs-lookup"><span data-stu-id="02fbc-138">You need to load and unload sections of the collection based on usage.</span></span> <span data-ttu-id="02fbc-139">以下示例对此行为进行建模。</span><span class="sxs-lookup"><span data-stu-id="02fbc-139">The example following models this behavior.</span></span> <span data-ttu-id="02fbc-140">此示例报告存在多少数据点。</span><span class="sxs-lookup"><span data-stu-id="02fbc-140">It reports on how many data points exist.</span></span> <span data-ttu-id="02fbc-141">此示例按需创建页以存储部分数据。</span><span class="sxs-lookup"><span data-stu-id="02fbc-141">It creates pages to hold sections of the data on demand.</span></span> <span data-ttu-id="02fbc-142">此示例从内存中删除页，以便为较新的请求所需的页腾出空间。</span><span class="sxs-lookup"><span data-stu-id="02fbc-142">It removes pages from memory to make room for pages needed by more recent requests.</span></span>

```csharp
public class DataSamples
{
    private class Page
    {
        private readonly List<Measurements> pageData = new List<Measurements>();
        private readonly int startingIndex;
        private readonly int length;
        private bool dirty;
        private DateTime lastAccess;

        public Page(int startingIndex, int length)
        {
            this.startingIndex = startingIndex;
            this.length = length;
            lastAccess = DateTime.Now;

            // This stays as random stuff:
            var generator = new Random();
            for(int i=0; i < length; i++)
            {
                var m = new Measurements
                {
                    HiTemp = generator.Next(50, 95),
                    LoTemp = generator.Next(12, 49),
                    AirPressure = 28.0 + generator.NextDouble() * 4
                };
                pageData.Add(m);
            }
        }
        public bool HasItem(int index) =>
            ((index >= startingIndex) &&
            (index < startingIndex + length));

        public Measurements this[int index]
        {
            get
            {
                lastAccess = DateTime.Now;
                return pageData[index - startingIndex];
            }
            set
            {
                pageData[index - startingIndex] = value;
                dirty = true;
                lastAccess = DateTime.Now;
            }
        }

        public bool Dirty => dirty;
        public DateTime LastAccess => lastAccess;
    }

    private readonly int totalSize;
    private readonly List<Page> pagesInMemory = new List<Page>();

    public DataSamples(int totalSize)
    {
        this.totalSize = totalSize;
    }

    public Measurements this[int index]
    {
        get
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");

            var page = updateCachedPagesForAccess(index);
            return page[index];
        }
        set
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");
            var page = updateCachedPagesForAccess(index);

            page[index] = value;
        }
    }

    private Page updateCachedPagesForAccess(int index)
    {
        foreach (var p in pagesInMemory)
        {
            if (p.HasItem(index))
            {
                return p;
            }
        }
        var startingIndex = (index / 1000) * 1000;
        var newPage = new Page(startingIndex, 1000);
        addPageToCache(newPage);
        return newPage;
    }

    private void addPageToCache(Page p)
    {
        if (pagesInMemory.Count > 4)
        {
            // remove oldest non-dirty page:
            var oldest = pagesInMemory
                .Where(page => !page.Dirty)
                .OrderBy(page => page.LastAccess)
                .FirstOrDefault();
            // Note that this may keep more than 5 pages in memory
            // if too much is dirty
            if (oldest != null)
                pagesInMemory.Remove(oldest);
        }
        pagesInMemory.Add(p);
    }
}
```

<span data-ttu-id="02fbc-143">可以按照此设计惯例对任何类型的集合进行建模，其中有充分的理由不将整个数据集加载到内存集合。</span><span class="sxs-lookup"><span data-stu-id="02fbc-143">You can follow this design idiom to model any sort of collection where there are good reasons not to load the entire set of data into an in- memory collection.</span></span> <span data-ttu-id="02fbc-144">请注意，`Page` 类是私有嵌套类，不是公共接口的一部分。</span><span class="sxs-lookup"><span data-stu-id="02fbc-144">Notice that the `Page` class is a private nested class that is not part of the public interface.</span></span> <span data-ttu-id="02fbc-145">向此类的任何用户隐藏这些详细信息。</span><span class="sxs-lookup"><span data-stu-id="02fbc-145">Those details are hidden from any users of this class.</span></span>

### <a name="dictionaries"></a><span data-ttu-id="02fbc-146">字典</span><span class="sxs-lookup"><span data-stu-id="02fbc-146">Dictionaries</span></span>

<span data-ttu-id="02fbc-147">另一个常见场景是需要对字典或映射进行建模时。</span><span class="sxs-lookup"><span data-stu-id="02fbc-147">Another common scenario is when you need to model a dictionary or a map.</span></span> <span data-ttu-id="02fbc-148">当类型存储基于键（通常是文本键）的值时出现此情况。</span><span class="sxs-lookup"><span data-stu-id="02fbc-148">This scenario is when your type stores values based on key, typically text keys.</span></span> <span data-ttu-id="02fbc-149">本示例创建的字典将命令行参数映射到管理这些选项的 [Lambda 表达式](delegates-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="02fbc-149">This example creates a dictionary that maps command line arguments to [lambda expressions](delegates-overview.md) that manage those options.</span></span> <span data-ttu-id="02fbc-150">以下示例演示了两个类：`ArgsActions` 类将命令行选项映射到 `Action` 委托；`ArgsProcessor` 类在遇到此选项时使用 `ArgsActions` 执行每个 `Action`。</span><span class="sxs-lookup"><span data-stu-id="02fbc-150">The following example shows two classes: an `ArgsActions` class that maps a command line option to an `Action` delegate, and an `ArgsProcessor` that uses the `ArgsActions` to execute each `Action` when it encounters that option.</span></span>

```csharp
public class ArgsProcessor
{
    private readonly ArgsActions actions;

    public ArgsProcessor(ArgsActions actions)
    {
        this.actions = actions;
    }

    public void Process(string[] args)
    {
        foreach(var arg in args)
        {
            actions[arg]?.Invoke();
        }
    }

}
public class ArgsActions
{
    readonly private Dictionary<string, Action> argsActions = new Dictionary<string, Action>();

    public Action this[string s]
    {
        get
        {
            Action action;
            Action defaultAction = () => {} ;
            return argsActions.TryGetValue(s, out action) ? action : defaultAction;
        }
    }

    public void SetOption(string s, Action a)
    {
        argsActions[s] = a;
    }
}
```

<span data-ttu-id="02fbc-151">在此示例中，`ArgsAction` 集合紧密映射到基础集合。</span><span class="sxs-lookup"><span data-stu-id="02fbc-151">In this example, the `ArgsAction` collection maps closely to the underlying collection.</span></span>
<span data-ttu-id="02fbc-152">`get` 确定是否已配置给定的选项。</span><span class="sxs-lookup"><span data-stu-id="02fbc-152">The `get` determines if a given option has been configured.</span></span> <span data-ttu-id="02fbc-153">如果已配置，则返回与此选项相关联的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="02fbc-153">If so, it returns the `Action` associated with that option.</span></span> <span data-ttu-id="02fbc-154">如果未配置，则返回不执行任何操作的 `Action`。</span><span class="sxs-lookup"><span data-stu-id="02fbc-154">If not, it returns an `Action` that does nothing.</span></span> <span data-ttu-id="02fbc-155">公共访问器不包括 `set` 访问器。</span><span class="sxs-lookup"><span data-stu-id="02fbc-155">The public accessor does not include a `set` accessor.</span></span> <span data-ttu-id="02fbc-156">相反地，设计使用公共方法来设置选项。</span><span class="sxs-lookup"><span data-stu-id="02fbc-156">Rather, the design using a public method for setting options.</span></span>

### <a name="multi-dimensional-maps"></a><span data-ttu-id="02fbc-157">多维映射</span><span class="sxs-lookup"><span data-stu-id="02fbc-157">Multi-Dimensional Maps</span></span>

<span data-ttu-id="02fbc-158">可以创建使用多个参数的索引器。</span><span class="sxs-lookup"><span data-stu-id="02fbc-158">You can create indexers that use multiple arguments.</span></span> <span data-ttu-id="02fbc-159">此外，这些参数未限制为相同的类型。</span><span class="sxs-lookup"><span data-stu-id="02fbc-159">In addition, those arguments are not constrained to be the same type.</span></span> <span data-ttu-id="02fbc-160">请看以下两个示例。</span><span class="sxs-lookup"><span data-stu-id="02fbc-160">Let's look at two examples.</span></span>

<span data-ttu-id="02fbc-161">第一个示例演示为 Mandelbrot 集合生成值的类。</span><span class="sxs-lookup"><span data-stu-id="02fbc-161">The first example shows a class that generates values for a Mandelbrot set.</span></span> <span data-ttu-id="02fbc-162">有关此集合背后的数学原理的详细信息，请参阅[这篇文章](https://en.wikipedia.org/wiki/Mandelbrot_set)。</span><span class="sxs-lookup"><span data-stu-id="02fbc-162">For more information on the mathematics behind the set, read [this article](https://en.wikipedia.org/wiki/Mandelbrot_set).</span></span>
<span data-ttu-id="02fbc-163">索引器使用两个双精度型来定义平面 XY 上的一个点。</span><span class="sxs-lookup"><span data-stu-id="02fbc-163">The indexer uses two doubles to define a point in the X, Y plane.</span></span>
<span data-ttu-id="02fbc-164">Get 访问器计算迭代的次数，直到确定某个点不在集合中。</span><span class="sxs-lookup"><span data-stu-id="02fbc-164">The get accessor computes the number of iterations until a point is determined to be not in the set.</span></span> <span data-ttu-id="02fbc-165">如果达到最大迭代数，并且点在集合中，则返回类的 maxIterations 值。</span><span class="sxs-lookup"><span data-stu-id="02fbc-165">If the maximum iterations is reached, the point is in the set, and the class's maxIterations value is returned.</span></span> <span data-ttu-id="02fbc-166">（Mandelbrot 集合常用的计算机生成的图像定义迭代数量的颜色，以便确定一个点是否在集合外部。</span><span class="sxs-lookup"><span data-stu-id="02fbc-166">(The computer generated images popularized for the Mandelbrot set define colors for the number of iterations necessary to determine that a point is outside the set.</span></span>

```csharp
public class Mandelbrot
{
    readonly private int maxIterations;

    public Mandelbrot(int maxIterations)
    {
        this.maxIterations = maxIterations;
    }

    public int this [double x, double y]
    {
        get
        {
            var iterations = 0;
            var x0 = x;
            var y0 = y;

            while ((x*x + y * y < 4) &&
                (iterations < maxIterations))
            {
                var newX = x * x - y * y + x0;
                y = 2 * x * y + y0;
                x = newX;
                iterations++;
            }
            return iterations;
        }
    }
}
```

<span data-ttu-id="02fbc-167">Mandelbrot 集合在每个 (x,y) 坐标上为实际数值定义值。</span><span class="sxs-lookup"><span data-stu-id="02fbc-167">The Mandelbrot Set defines values at every (x,y) coordinate for real number values.</span></span>
<span data-ttu-id="02fbc-168">这将定义一个字典，其中可能包含无限数目的值。</span><span class="sxs-lookup"><span data-stu-id="02fbc-168">That defines a dictionary that could contain an infinite number of values.</span></span> <span data-ttu-id="02fbc-169">因此，集合后面没有任何存储。</span><span class="sxs-lookup"><span data-stu-id="02fbc-169">Therefore, there is no storage behind the set.</span></span> <span data-ttu-id="02fbc-170">相反，当代码调用 `get` 访问器时，此类计算每个点的值。</span><span class="sxs-lookup"><span data-stu-id="02fbc-170">Instead, this class computes the value for each point when code calls the `get` accessor.</span></span> <span data-ttu-id="02fbc-171">未使用任何基础存储。</span><span class="sxs-lookup"><span data-stu-id="02fbc-171">There's no underlying storage used.</span></span>

<span data-ttu-id="02fbc-172">请查看上一次索引器的使用，其中索引器采用多个不同类型的参数。</span><span class="sxs-lookup"><span data-stu-id="02fbc-172">Let's examine one last use of indexers, where the indexer takes multiple arguments of different types.</span></span> <span data-ttu-id="02fbc-173">请考虑一个管理历史温度数据的程序。</span><span class="sxs-lookup"><span data-stu-id="02fbc-173">Consider a program that manages historical temperature data.</span></span> <span data-ttu-id="02fbc-174">此索引器使用一个城市和一个日期来设置或获取位置的高温和低温：</span><span class="sxs-lookup"><span data-stu-id="02fbc-174">This indexer uses a city and a date to set or get the high and low temperatures for that location:</span></span>

```csharp
using DateMeasurements =
    System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements =
    System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;

public class HistoricalWeatherData
{
    readonly CityDataMeasurements storage = new CityDataMeasurements();

    public Measurements this[string city, DateTime date]
    {
        get
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
                throw new ArgumentOutOfRangeException(nameof(city), "City not found");

            // strip out any time portion:
            var index = date.Date;
            var measure = default(Measurements);
            if (cityData.TryGetValue(index, out measure))
                return measure;
            throw new ArgumentOutOfRangeException(nameof(date), "Date not found");
        }
        set
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
            {
                cityData = new DateMeasurements();
                storage.Add(city, cityData);
            }

            // Strip out any time portion:
            var index = date.Date;
            cityData[index] = value;
        }
    }
}
```

<span data-ttu-id="02fbc-175">此示例创建的索引器将天气数据映射到两个不同的参数：城市（由 `string` 表示）和日期（由 `DateTime` 表示）。</span><span class="sxs-lookup"><span data-stu-id="02fbc-175">This example creates an indexer that maps weather data on two different arguments: a city (represented by a `string`) and a date (represented by a `DateTime`).</span></span> <span data-ttu-id="02fbc-176">内部存储使用两个 `Dictionary` 类来表示此二维字典。</span><span class="sxs-lookup"><span data-stu-id="02fbc-176">The internal storage uses two `Dictionary` classes to represent the two-dimensional dictionary.</span></span> <span data-ttu-id="02fbc-177">公共 API 不再表示基础存储。</span><span class="sxs-lookup"><span data-stu-id="02fbc-177">The public API no longer represents the underlying storage.</span></span> <span data-ttu-id="02fbc-178">相反地，凭借索引器的语言特性可以创建表示抽象化的一个公共接口，即使基础存储必须使用不同的核心集合类型也是如此。</span><span class="sxs-lookup"><span data-stu-id="02fbc-178">Rather, the language features of indexers enables you to create a public interface that represents your abstraction, even though the underlying storage must use different core collection types.</span></span>

<span data-ttu-id="02fbc-179">一些开发人员可能不熟悉此代码的两部分。</span><span class="sxs-lookup"><span data-stu-id="02fbc-179">There are two parts of this code that may be unfamiliar to some developers.</span></span> <span data-ttu-id="02fbc-180">这两个 `using` 指令：</span><span class="sxs-lookup"><span data-stu-id="02fbc-180">These two `using` directives:</span></span>

```csharp
using DateMeasurements = System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements = System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;
```

<span data-ttu-id="02fbc-181">为构造泛型类型创建别名。</span><span class="sxs-lookup"><span data-stu-id="02fbc-181">create an *alias* for a constructed generic type.</span></span> <span data-ttu-id="02fbc-182">通过这些语句，稍后代码可以使用更具描述性的 `DateMeasurements` 和 `CityDateMeasurements` 名称，而不是 `Dictionary<DateTime, Measurements>` 和 `Dictionary<string, Dictionary<DateTime, Measurements> >` 的泛型构造。</span><span class="sxs-lookup"><span data-stu-id="02fbc-182">Those statements enable the code later to use the more descriptive `DateMeasurements` and `CityDateMeasurements` names instead of the generic construction of `Dictionary<DateTime, Measurements>` and `Dictionary<string, Dictionary<DateTime, Measurements> >`.</span></span>
<span data-ttu-id="02fbc-183">此构造要求在 `=` 符号右侧使用完全限定的类型名称。</span><span class="sxs-lookup"><span data-stu-id="02fbc-183">This construct does require using the fully qualified type names on the right side of the `=` sign.</span></span>

<span data-ttu-id="02fbc-184">另一项技术是对任何用于集合的索引的 `DateTime` 对象剥离时间部分。</span><span class="sxs-lookup"><span data-stu-id="02fbc-184">The second technique is to strip off the time portions of any `DateTime` object used to index into the collections.</span></span> <span data-ttu-id="02fbc-185">.NET 不包含仅日期类型。</span><span class="sxs-lookup"><span data-stu-id="02fbc-185">.NET doesn't include a date-only type.</span></span>
<span data-ttu-id="02fbc-186">开发人员使用 `DateTime` 类型，但使用 `Date` 属性来确保这一天的任何 `DateTime` 对象是对等的。</span><span class="sxs-lookup"><span data-stu-id="02fbc-186">Developers use the `DateTime` type, but use the `Date` property to ensure that any `DateTime` object from that day are equal.</span></span>

## <a name="summing-up"></a><span data-ttu-id="02fbc-187">总结</span><span class="sxs-lookup"><span data-stu-id="02fbc-187">Summing Up</span></span>

<span data-ttu-id="02fbc-188">只要类中有类似于属性的元素就应创建索引器，此属性代表的不是一个值，而是值的集合，其中每一个项由一组参数标识。</span><span class="sxs-lookup"><span data-stu-id="02fbc-188">You should create indexers anytime you have a property-like element in your class where that property represents not a single value, but rather a collection of values where each individual item is identified by a set of arguments.</span></span> <span data-ttu-id="02fbc-189">这些参数可以唯一标识应引用的集合中的项。</span><span class="sxs-lookup"><span data-stu-id="02fbc-189">Those arguments can uniquely identify which item in the collection should be referenced.</span></span>
<span data-ttu-id="02fbc-190">索引器延伸了[属性](properties.md)的概念，索引器中的一个成员被视为类外部的一个数据项，但又类似于内部的一个方法。</span><span class="sxs-lookup"><span data-stu-id="02fbc-190">Indexers extend the concept of [properties](properties.md), where a member is treated like a data item from outside the class, but like a method on the inside.</span></span> <span data-ttu-id="02fbc-191">索引器允许参数在代表项的集合的属性中查找单个项。</span><span class="sxs-lookup"><span data-stu-id="02fbc-191">Indexers allow arguments to find a single item in a property that represents a set of items.</span></span>
