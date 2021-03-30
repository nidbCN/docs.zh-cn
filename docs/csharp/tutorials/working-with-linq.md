---
title: 使用 LINQ
description: 此教程将介绍如何使用 LINQ 生成序列、编写用于 LINQ 查询的方法，以及如何区分及早计算和惰性计算。
ms.date: 10/29/2018
ms.technology: csharp-linq
ms.assetid: 0db12548-82cb-4903-ac88-13103d70aa77
ms.openlocfilehash: e6c241e90865335707f8d050f4bf8772f945355d
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874493"
---
# <a name="work-with-language-integrated-query-linq"></a>使用语言集成查询 (LINQ)

## <a name="introduction"></a>简介

本教程将介绍 .NET Core 和 C# 语言的功能。 你将了解如何执行以下操作：

- 使用 LINQ 生成序列。
- 编写可轻松用于 LINQ 查询的方法。
- 区分及早计算和惰性计算。

你将通过生成应用程序来了解如何执行这些操作，应用程序体现了所有魔术师都具备的一项基本技能，即[完美洗牌](https://en.wikipedia.org/wiki/Faro_shuffle)。 简而言之，完美洗牌这项技能是指，将一副纸牌分成两半，然后两手各拿一半交错洗牌（一张隔一张），以便重新生成原来的一副纸牌。

魔术师之所以要掌握这项技能是因为，每次洗牌后每张纸牌的位置已知，且顺序为重复模式。

考虑到你的目的，数据序列控制起来就会非常轻松。 生成的应用程序会构造一副纸牌，然后执行一系列洗牌操作，每次都会输出序列。 还可以将更新后的顺序与原始顺序进行比较。

在此教程中，将执行多步操作。 执行每步操作后，都可以运行应用程序，并查看进度。 还可参阅 dotnet/samples GitHub 存储库中的[完整示例](https://github.com/dotnet/samples/blob/main/csharp/getting-started/console-linq)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

## <a name="prerequisites"></a>先决条件

必须将计算机设置为运行 .Net Core。 有关安装说明，请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页。 可以在 Windows、Ubuntu Linux、OS X 或 Docker 容器中运行此应用程序。 必须安装常用的代码编辑器。 在以下说明中，我们使用的是开放源代码跨平台编辑器 [Visual Studio Code](https://code.visualstudio.com/)。 不过，你可以使用习惯使用的任意工具。

## <a name="create-the-application"></a>创建应用程序

第一步是新建应用程序。 打开命令提示符，然后新建应用程序的目录。 将新建的目录设为当前目录。 在命令提示符处，键入命令 `dotnet new console`。 这将为基本的“Hello World”应用程序创建起始文件。

如果之前从未用过 C#，请参阅[这篇教程](console-teleprompter.md)，其中介绍了 C# 程序的结构。 可以阅读相应的内容，然后回到此教程详细了解 LINQ。

## <a name="create-the-data-set"></a>创建数据集

开始前，请确保下列行在 `dotnet new console` 生成的 `Program.cs` 文件的顶部：

```csharp
// Program.cs
using System;
using System.Collections.Generic;
using System.Linq;
```

如果这三行（`using` 语句）未在该文件的顶部，程序将无法编译。

现在已具备所需的所有引用，接下来可以考虑一副扑克牌是由什么构成的。 通常一副扑克牌包含四种花色，每种花色包含 13 个值。 通常情况下，你可能会立即考虑创建一个 `Card` 类，然后手动填充一组 `Card` 对象。 相对于通常的方式，使用 LINQ 创建一副扑克牌更加简捷。 可以创建两个序列来分别表示花色和点数，而非创建 `Card` 类。 创建两个非常简单的[迭代器方法](../iterators.md#enumeration-sources-with-iterator-methods)，用于将级别和花色生成为 <xref:System.Collections.Generic.IEnumerable%601> 字符串：

```csharp
// Program.cs
// The Main() method

static IEnumerable<string> Suits()
{
    yield return "clubs";
    yield return "diamonds";
    yield return "hearts";
    yield return "spades";
}

static IEnumerable<string> Ranks()
{
    yield return "two";
    yield return "three";
    yield return "four";
    yield return "five";
    yield return "six";
    yield return "seven";
    yield return "eight";
    yield return "nine";
    yield return "ten";
    yield return "jack";
    yield return "queen";
    yield return "king";
    yield return "ace";
}
```

将它们置于 `Main` 文件的 `Program.cs` 方法的下面。 这两种方法都利用 `yield return` 语法在运行时生成序列。 编译器会生成对象来实现 <xref:System.Collections.Generic.IEnumerable%601>，并在有请求时生成字符串序列。

现在使用这些迭代器创建一副扑克牌。 将 LINQ 查询置于 `Main` 方法中。 具体如下所示：

```csharp
// Program.cs
static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    // Display each card that we've generated and placed in startingDeck in the console
    foreach (var card in startingDeck)
    {
        Console.WriteLine(card);
    }
}
```

多个 `from` 子句生成 <xref:System.Linq.Enumerable.SelectMany%2A>，用于将第一个和第二个序列中的所有元素合并成一个序列。 考虑到我们的目的，顺序非常重要。 第一个源序列（花色）中的首个元素与第二个序列（等级）中的每个元素结合使用。 这就生成了第一个花色的所有十三张纸牌。 对第一个序列（花色）中的每个元素重复此过程。 最后生成按花色排序（后跟值）的一副纸牌。

务必注意，无论是选择使用上文所用的查询语法编写 LINQ，还是使用方法语法，始终都可以从一种语法形式转至另一种。 可使用方法语法编写使用查询语法编写的上述查询，如下所示：

```csharp
var startingDeck = Suits().SelectMany(suit => Ranks().Select(rank => new { Suit = suit, Rank = rank }));
```

编译器会将使用查询语法编写的 LINQ 语句转换为等效的方法调用语法。 因此无论选择哪种语法，两种查询版本生成的结果相同。 选择最适合你的情况的语法：例如，若所在的工作团队中的某些成员不擅长方法语法，则尽量首选使用查询语法。

此时，运行已生成的示例。 将显示一副纸牌中的所有 52 张纸牌。 在调试器模式下运行此示例来观察 `Suits()` 和 `Ranks()` 方法的执行情况，你可能会觉得非常有用。 可以清楚地看到，每个序列中的所有字符串仅在需要时生成。

![显示应用输出 52 张扑克牌的控制台窗口。](./media/working-with-linq/console-52-card-application.png)

## <a name="manipulate-the-order"></a>操作顺序

接下来重点介绍如何洗牌。 正确洗牌的第一步是将一副扑克牌分成两半。 LINQ API 包含的 <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 方法提供了这种功能。 将它们置于 `foreach` 循环的下面：

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    // 52 cards in a deck, so 52 / 2 = 26
    var top = startingDeck.Take(26);
    var bottom = startingDeck.Skip(26);
}
```

但标准库中没有可供使用的洗牌方法，因此必须自行编写。 将要创建的洗牌方法体现了要对基于 LINQ 的程序执行的几种操作，因此我们将逐步介绍此过程的每个部分。

需要编写几种特殊的方法，我们称之为<xref:System.Collections.Generic.IEnumerable%601>扩展方法[，来添加一些功能，以便于与 LINQ 查询返回的 ](../programming-guide/classes-and-structs/extension-methods.md) 交互。 简而言之，扩展方法是具有特殊用途的静态方法，借助它无需修改你想要为其添加功能的已有原始类型，即可向其添加功能。

向程序添加新的静态类文件（名称为 `Extensions.cs`），以用于存放扩展方法，然后开始生成第一个扩展方法：

```csharp
// Extensions.cs
using System;
using System.Collections.Generic;
using System.Linq;

namespace LinqFaroShuffle
{
    public static class Extensions
    {
        public static IEnumerable<T> InterleaveSequenceWith<T>(this IEnumerable<T> first, IEnumerable<T> second)
        {
            // Your implementation will go here soon enough
        }
    }
}
```

仔细观察方法签名，尤其是参数：

```csharp
public static IEnumerable<T> InterleaveSequenceWith<T> (this IEnumerable<T> first, IEnumerable<T> second)
```

可以发现，在扩展方法的第一个自变量中添加了 `this` 修饰符。 也就是说，调用扩展方法，就像是第一个自变量类型的成员方法一样。 此方法声明还遵循标准惯用做法，其中输入和输出类型为 `IEnumerable<T>`。 遵循这种做法，可以将 LINQ 方法链在一起，从而执行更复杂的查询。

正常情况下，将扑克牌分为两半后，需要将这两半合并在一起。 在代码中，这意味着一次性地枚举通过 <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 获得的两个序列，`interleaving` 元素，并创建一个序列：即现在洗牌后的扑克牌。 必须了解 <xref:System.Collections.Generic.IEnumerable%601> 的工作原理，才能编写处理两个序列的 LINQ 方法。

<xref:System.Collections.Generic.IEnumerable%601> 接口有一个方法 (<xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>)。 <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> 返回的对象包含用于移动到下一个元素的方法，以及用于检索序列中当前元素的属性。 将使用这两个成员来枚举集合并返回元素。 由于此交错方法是迭代器方法，因此将使用上面的 `yield return` 语法，而不用生成并返回集合。

下面是此方法的实现代码：

[!CODE-csharp[InterleaveSequenceWith](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet1)]

至此，你已编写好这个方法，请回到 `Main` 方法，并进行一次洗牌：

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = from s in Suits()
                       from r in Ranks()
                       select new { Suit = s, Rank = r };

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    var top = startingDeck.Take(26);
    var bottom = startingDeck.Skip(26);
    var shuffle = top.InterleaveSequenceWith(bottom);

    foreach (var c in shuffle)
    {
        Console.WriteLine(c);
    }
}
```

## <a name="comparisons"></a>比较

进行多少次洗牌才能恢复一副纸牌的原始顺序？ 若要解决这个问题，需要编写用于确定两个序列是否相等的方法。 编写好此方法后，需要循环执行用于洗牌的代码，看看一副纸牌何时才能恢复原始顺序。

编写用于确定两个序列是否相等的方法应该很简单。 此方法的结构与你编写的洗牌方法类似。 不同之处在于，这一次将比较每个序列的匹配元素，而不是 `yield return` 每个元素。 枚举整个序列后，如果每个元素都一致，那么序列就是相同的：

[!CODE-csharp[SequenceEquals](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet2)]

这反映了另一种 LINQ 惯用做法，即终端方法。 此类方法需要将序列（或在此示例中，为两个序列）用作输入，并返回一个标量值。 使用终端方法时，它们始终是 LINQ 查询方法链中最后的方法，因此其名称为“终端”。

使用此类方法来确定一副纸牌何时恢复原始顺序时，就可以了解实际效果。 循环执行洗牌代码，通过应用 `SequenceEquals()` 方法确定序列已恢复原始顺序时停止执行。 你会发现，此类方法始终是任何查询中的最后一个方法，因为返回的是一个值，而不是一个序列：

```csharp
// Program.cs
static void Main(string[] args)
{
    // Query for building the deck

    // Shuffling using InterleaveSequenceWith<T>();

    var times = 0;
    // We can re-use the shuffle variable from earlier, or you can make a new one
    shuffle = startingDeck;
    do
    {
        shuffle = shuffle.Take(26).InterleaveSequenceWith(shuffle.Skip(26));

        foreach (var card in shuffle)
        {
            Console.WriteLine(card);
        }
        Console.WriteLine();
        times++;

    } while (!startingDeck.SequenceEquals(shuffle));

    Console.WriteLine(times);
}
```

运行现有的代码，并记录每次洗牌时扑克牌的重写排列方式。 进行 8 次洗牌后（迭代 do-while 循环），扑克牌恢复从最初的 LINQ 查询首次创建它时的原始配置。

## <a name="optimizations"></a>优化

到目前为止，你已生成的示例执行的是向外洗牌，即每次洗牌时第一张和最后一张纸牌保持不变。 让我们来做一点改变，改为使用向内洗牌，改变全部 52 张纸牌的位置。 向内洗牌是指，交错一副纸牌时，将后一半中的第一张纸牌变成一副纸牌中的第一张纸牌。 也就是说，上半部分中的最后一张纸牌变成一副纸牌中的最后一张纸牌。 这是对单行代码的简单更改。 交换 <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 的位置，来更新当前洗牌查询。 这会更改一副纸牌的上半部分和下半部分的顺序：

```csharp
shuffle = shuffle.Skip(26).InterleaveSequenceWith(shuffle.Take(26));
```

再次运行程序，你会发现需要进行 52 次迭代才能恢复一副纸牌的原始顺序。 随着程序的继续运行，你还会开始注意到一些非常严重的性能下降问题发生。

导致这种情况出现的原因有很多。 可以解决导致性能下降的主要原因之一：[延迟计算](../../standard/linq/deferred-execution-lazy-evaluation.md)的使用效率低下。

简单来说，延迟计算是指直至需要语句的值时才会执行语句计算。 LINQ 查询属于延迟计算的语句。 仅当有元素请求时才生成序列。 通常情况下，这是 LINQ 的主要优势所在。 不过，在诸如此程序之类的用例中，这就会导致执行时间指数式增长。

应该记得原来的一副纸牌是使用 LINQ 查询生成的。 每次洗牌操作是通过对上一副纸牌执行三次 LINQ 查询进行。 所有这些操作均采用惰性执行方式。 也就是说，每当有序列请求时，才会再次执行这些操作。 执行到第 52 次迭代时，就已经重新生成很多很多次原来的一副纸牌。 让我们来编写一个日志方法来阐明此行为。 然后，你就可以解决此问题。

在 `Extensions.cs` 文件中输入或复制下面的方法。 此扩展方法会在项目目录中新建一个名称为 `debug.log` 的文件，并将当前正在执行的查询记录到日志文件中。 可以将此扩展方法追加到任何查询中，以标记查询已执行。

[!CODE-csharp[LogQuery](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet3)]

你将看到 `File` 下显示红色波浪线，这表示它不存在。 它将不编译，因为编译器无法识别 `File`。 要解决此问题，请务必在 `Extensions.cs` 中第一行的下面添加以下代码行：

```csharp
using System.IO;
```

这应可解决问题，随后红色错误将消失。

接下来，使用日志消息来检测每个查询的定义：

```csharp
// Program.cs
public static void Main(string[] args)
{
    var startingDeck = (from s in Suits().LogQuery("Suit Generation")
                        from r in Ranks().LogQuery("Rank Generation")
                        select new { Suit = s, Rank = r }).LogQuery("Starting Deck");

    foreach (var c in startingDeck)
    {
        Console.WriteLine(c);
    }

    Console.WriteLine();
    var times = 0;
    var shuffle = startingDeck;

    do
    {
        // Out shuffle
        /*
        shuffle = shuffle.Take(26)
            .LogQuery("Top Half")
            .InterleaveSequenceWith(shuffle.Skip(26)
            .LogQuery("Bottom Half"))
            .LogQuery("Shuffle");
        */

        // In shuffle
        shuffle = shuffle.Skip(26).LogQuery("Bottom Half")
                .InterleaveSequenceWith(shuffle.Take(26).LogQuery("Top Half"))
                .LogQuery("Shuffle");

        foreach (var c in shuffle)
        {
            Console.WriteLine(c);
        }

        times++;
        Console.WriteLine(times);
    } while (!startingDeck.SequenceEquals(shuffle));

    Console.WriteLine(times);
}
```

请注意，不是每次访问查询都会生成日志。 只有在创建原始查询时，才会生成日志。 程序的运行时间仍然很长，但现在知道原因了。 如果对在启用日志记录的情况下运行向内洗牌失去了耐心，请切换回向外洗牌。 但仍会看到惰性计算效果。 在一次运行中，共执行 2592 次查询，包括生成所有值和花色。

可以提高此处的代码性能，以减少执行次数。 一个简单的修复方法是缓存构造扑克牌的原始 LINQ 查询的结果。 目前，每当 do-while 循环进行迭代时，需要反复执行查询，每次都要重新构造扑克牌并进行洗牌。 可以利用 LINQ 方法 <xref:System.Linq.Enumerable.ToArray%2A> 和 <xref:System.Linq.Enumerable.ToList%2A> 来缓存扑克牌；将这两个方法追加到查询时，它们将执行你已告知它们要执行的同种操作，而现在它们会将结果存储在数组或列表中，具体取决于选择调用的方法。 将 LINQ 方法 <xref:System.Linq.Enumerable.ToArray%2A> 追加到两个查询中，并再次运行程序：

[!CODE-csharp[Main](../../../samples/snippets/csharp/getting-started/console-linq/Program.cs?name=snippet1)]

现在向外洗牌下降到 30 次查询。 再次运行向内洗牌程序，改善情况类似：现在它执行 162 次查询。

请注意，此示例旨在突出显示延迟计算可能会导致性能下降的用例。 了解延迟计算会在何处影响代码性能至关重要，但了解并非所有查询应及早运行也同等重要。 未使用 <xref:System.Linq.Enumerable.ToArray%2A> 会导致性能损失，这是因为每次重新排列一副纸牌都要以先前的排列为依据。 使用惰性计算意味着一副纸牌的每个新配置都以原来的一副纸牌为依据，甚至在执行生成 `startingDeck` 的代码时，也不例外。 这会导致大量额外的工作。

在实践中，一些算法使用及早计算的效果较好，另一些算法使用延迟计算的效果较好。 对于日常使用，如果数据源为单独进程（如数据库引擎），通常更好的选择是使用延迟计算。 对于数据库，使用延迟计算，更复杂的查询可以只对数据库进程执行一次往返，然后返回至剩余的代码。 无论选择使用延迟计算还是及早计算，LINQ 均可以灵活处理，因此请衡量自己的进程，然后选择可为你提供最佳性能的计算种类。

## <a name="conclusion"></a>结束语

在此项目中介绍了下列内容：

- 使用 LINQ 查询，来将数据聚合到有意义的序列中
- 编写扩展方法，来将自己的自定义功能添加到 LINQ 查询
- 查找 LINQ 查询可能会在其中遇到性能问题（如速度下降）的代码区域
- 与 LINQ 查询相关的延迟计算和及早计算，以及它们对查询性能的影响

除 LINQ 外，还简单介绍了魔术师用于扑克牌魔术的一个技术。 魔术师之所以采用完美洗牌是因为，可以控制每张纸牌在一副纸牌中的移动。 现在你了解了，也不要告诉其他人以免破坏他们的兴致！

有关 LINQ 的更多信息，请访问：

- [语言集成查询 (LINQ)](../programming-guide/concepts/linq/index.md)
- [LINQ 介绍](../programming-guide/concepts/linq/index.md)
- [基本 LINQ 查询操作 (C#)](../programming-guide/concepts/linq/basic-linq-query-operations.md)
- [使用 LINQ 进行数据转换 (C#)](../programming-guide/concepts/linq/data-transformations-with-linq.md)
- [LINQ 中的查询语法和方法语法 (C#)](../programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)
- [支持 LINQ 的 C# 功能](../programming-guide/concepts/linq/features-that-support-linq.md)
