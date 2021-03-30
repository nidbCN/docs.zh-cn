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
# <a name="indexers"></a>索引器

*索引器* 类似于属性。 很多时候，创建索引器与创建[属性](properties.md)所使用的编程语言特性是一样的。 索引器使属性可以被索引：使用一个或多个参数引用的属性。 这些参数为某些值集合提供索引。

## <a name="indexer-syntax"></a>索引器语法

可以通过变量名和方括号访问索引器。 将索引器参数放在方括号内：

```csharp
var item = someObject["key"];
someObject["AnotherKey"] = item;
```

使用 `this` 关键字作为属性名声明索引器，并在方括号内声明参数。 此声明与前一段中所示的用法相匹配：

```csharp
public int this[string key]
{
    get { return storage.Find(key); }
    set { storage.SetAt(key, value); }
}
```

从最初的示例中，可以看到属性语法和索引器语法之间的关系。 此类比在索引器的大部分语法规则中进行。 索引器可以使用任何有效的访问修饰符（public、protected internal、protected、internal、private 或 private protected）。 它们可能是密封、虚拟或抽象的。 与属性一样，可以在索引器中为 get 和 set 访问器指定不同的访问修饰符。
你还可以指定只读索引器（忽略 set 访问器）或只写索引器（忽略 get 访问器）。

属性的各种用法同样适用于索引器。 此规则的唯一例外是“自动实现属性”。 编译器无法始终为索引器生成正确的存储。

用于引用项的集合中的某个项的参数可区分索引器和属性。 只要每个索引器的参数列表是唯一的，就可以对一个类型定义多个索引器。 让我们来探讨可能在类定义中使用一个或多个索引器的不同场景。

## <a name="scenarios"></a>方案

如果类型的 API 对集合进行建模，并且为集合定义了参数，则需要在此类型中定义索引器。 索引器可能直接映射到属于 .NET Core 框架一部分的集合类型，也可能不。 除了对集合进行建模，类型还有其他职责。
通过索引器可提供与类型的抽象化匹配的 API，而无需公开如何存储或计算此抽象化的值的内部细节。

让我们演练一些使用索引器的常见场景。 可以访问[索引器的示例文件夹](https://github.com/dotnet/samples/tree/main/csharp/indexers)。 有关下载说明，请参阅[示例和教程](../samples-and-tutorials/index.md#view-and-download-samples)。

### <a name="arrays-and-vectors"></a>数组和矢量

创建索引器的一个最常见的场景是当类型对数组或矢量进行建模时。 可以创建一个索引器用于对已排序的数据列表进行建模。

创建自己的索引器的优点是你可以为集合定义存储以满足你的需求。 假设以下场景：类型对历史数据进行建模，并且此历史数据太大而无法立即加载到内存中。 需要根据使用情况加载和卸载集合的某些部分。 以下示例对此行为进行建模。 此示例报告存在多少数据点。 此示例按需创建页以存储部分数据。 此示例从内存中删除页，以便为较新的请求所需的页腾出空间。

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

可以按照此设计惯例对任何类型的集合进行建模，其中有充分的理由不将整个数据集加载到内存集合。 请注意，`Page` 类是私有嵌套类，不是公共接口的一部分。 向此类的任何用户隐藏这些详细信息。

### <a name="dictionaries"></a>字典

另一个常见场景是需要对字典或映射进行建模时。 当类型存储基于键（通常是文本键）的值时出现此情况。 本示例创建的字典将命令行参数映射到管理这些选项的 [Lambda 表达式](delegates-overview.md)。 以下示例演示了两个类：`ArgsActions` 类将命令行选项映射到 `Action` 委托；`ArgsProcessor` 类在遇到此选项时使用 `ArgsActions` 执行每个 `Action`。

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

在此示例中，`ArgsAction` 集合紧密映射到基础集合。
`get` 确定是否已配置给定的选项。 如果已配置，则返回与此选项相关联的 `Action`。 如果未配置，则返回不执行任何操作的 `Action`。 公共访问器不包括 `set` 访问器。 相反地，设计使用公共方法来设置选项。

### <a name="multi-dimensional-maps"></a>多维映射

可以创建使用多个参数的索引器。 此外，这些参数未限制为相同的类型。 请看以下两个示例。

第一个示例演示为 Mandelbrot 集合生成值的类。 有关此集合背后的数学原理的详细信息，请参阅[这篇文章](https://en.wikipedia.org/wiki/Mandelbrot_set)。
索引器使用两个双精度型来定义平面 XY 上的一个点。
Get 访问器计算迭代的次数，直到确定某个点不在集合中。 如果达到最大迭代数，并且点在集合中，则返回类的 maxIterations 值。 （Mandelbrot 集合常用的计算机生成的图像定义迭代数量的颜色，以便确定一个点是否在集合外部。

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

Mandelbrot 集合在每个 (x,y) 坐标上为实际数值定义值。
这将定义一个字典，其中可能包含无限数目的值。 因此，集合后面没有任何存储。 相反，当代码调用 `get` 访问器时，此类计算每个点的值。 未使用任何基础存储。

请查看上一次索引器的使用，其中索引器采用多个不同类型的参数。 请考虑一个管理历史温度数据的程序。 此索引器使用一个城市和一个日期来设置或获取位置的高温和低温：

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

此示例创建的索引器将天气数据映射到两个不同的参数：城市（由 `string` 表示）和日期（由 `DateTime` 表示）。 内部存储使用两个 `Dictionary` 类来表示此二维字典。 公共 API 不再表示基础存储。 相反地，凭借索引器的语言特性可以创建表示抽象化的一个公共接口，即使基础存储必须使用不同的核心集合类型也是如此。

一些开发人员可能不熟悉此代码的两部分。 这两个 `using` 指令：

```csharp
using DateMeasurements = System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements = System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;
```

为构造泛型类型创建别名。 通过这些语句，稍后代码可以使用更具描述性的 `DateMeasurements` 和 `CityDateMeasurements` 名称，而不是 `Dictionary<DateTime, Measurements>` 和 `Dictionary<string, Dictionary<DateTime, Measurements> >` 的泛型构造。
此构造要求在 `=` 符号右侧使用完全限定的类型名称。

另一项技术是对任何用于集合的索引的 `DateTime` 对象剥离时间部分。 .NET 不包含仅日期类型。
开发人员使用 `DateTime` 类型，但使用 `Date` 属性来确保这一天的任何 `DateTime` 对象是对等的。

## <a name="summing-up"></a>总结

只要类中有类似于属性的元素就应创建索引器，此属性代表的不是一个值，而是值的集合，其中每一个项由一组参数标识。 这些参数可以唯一标识应引用的集合中的项。
索引器延伸了[属性](properties.md)的概念，索引器中的一个成员被视为类外部的一个数据项，但又类似于内部的一个方法。 索引器允许参数在代表项的集合的属性中查找单个项。
