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
# <a name="work-with-language-integrated-query-linq"></a><span data-ttu-id="00e3c-103">使用语言集成查询 (LINQ)</span><span class="sxs-lookup"><span data-stu-id="00e3c-103">Work with Language-Integrated Query (LINQ)</span></span>

## <a name="introduction"></a><span data-ttu-id="00e3c-104">简介</span><span class="sxs-lookup"><span data-stu-id="00e3c-104">Introduction</span></span>

<span data-ttu-id="00e3c-105">本教程将介绍 .NET Core 和 C# 语言的功能。</span><span class="sxs-lookup"><span data-stu-id="00e3c-105">This tutorial teaches you features in .NET Core and the C# language.</span></span> <span data-ttu-id="00e3c-106">你将了解如何执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="00e3c-106">You’ll learn how to:</span></span>

- <span data-ttu-id="00e3c-107">使用 LINQ 生成序列。</span><span class="sxs-lookup"><span data-stu-id="00e3c-107">Generate sequences with LINQ.</span></span>
- <span data-ttu-id="00e3c-108">编写可轻松用于 LINQ 查询的方法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-108">Write methods that can be easily used in LINQ queries.</span></span>
- <span data-ttu-id="00e3c-109">区分及早计算和惰性计算。</span><span class="sxs-lookup"><span data-stu-id="00e3c-109">Distinguish between eager and lazy evaluation.</span></span>

<span data-ttu-id="00e3c-110">你将通过生成应用程序来了解如何执行这些操作，应用程序体现了所有魔术师都具备的一项基本技能，即[完美洗牌](https://en.wikipedia.org/wiki/Faro_shuffle)。</span><span class="sxs-lookup"><span data-stu-id="00e3c-110">You'll learn these techniques by building an application that demonstrates one of the basic skills of any magician: the [faro shuffle](https://en.wikipedia.org/wiki/Faro_shuffle).</span></span> <span data-ttu-id="00e3c-111">简而言之，完美洗牌这项技能是指，将一副纸牌分成两半，然后两手各拿一半交错洗牌（一张隔一张），以便重新生成原来的一副纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-111">Briefly, a faro shuffle is a technique where you split a card deck exactly in half, then the shuffle interleaves each one card from each half to rebuild the original deck.</span></span>

<span data-ttu-id="00e3c-112">魔术师之所以要掌握这项技能是因为，每次洗牌后每张纸牌的位置已知，且顺序为重复模式。</span><span class="sxs-lookup"><span data-stu-id="00e3c-112">Magicians use this technique because every card is in a known location after each shuffle, and the order is a repeating pattern.</span></span>

<span data-ttu-id="00e3c-113">考虑到你的目的，数据序列控制起来就会非常轻松。</span><span class="sxs-lookup"><span data-stu-id="00e3c-113">For your purposes, it is a light hearted look at manipulating sequences of data.</span></span> <span data-ttu-id="00e3c-114">生成的应用程序会构造一副纸牌，然后执行一系列洗牌操作，每次都会输出序列。</span><span class="sxs-lookup"><span data-stu-id="00e3c-114">The application you'll build constructs a card deck and then performs a sequence of shuffles, writing the sequence out each time.</span></span> <span data-ttu-id="00e3c-115">还可以将更新后的顺序与原始顺序进行比较。</span><span class="sxs-lookup"><span data-stu-id="00e3c-115">You'll also compare the updated order to the original order.</span></span>

<span data-ttu-id="00e3c-116">在此教程中，将执行多步操作。</span><span class="sxs-lookup"><span data-stu-id="00e3c-116">This tutorial has multiple steps.</span></span> <span data-ttu-id="00e3c-117">执行每步操作后，都可以运行应用程序，并查看进度。</span><span class="sxs-lookup"><span data-stu-id="00e3c-117">After each step, you can run the application and see the progress.</span></span> <span data-ttu-id="00e3c-118">还可参阅 dotnet/samples GitHub 存储库中的[完整示例](https://github.com/dotnet/samples/blob/main/csharp/getting-started/console-linq)。</span><span class="sxs-lookup"><span data-stu-id="00e3c-118">You can also see the [completed sample](https://github.com/dotnet/samples/blob/main/csharp/getting-started/console-linq) in the dotnet/samples GitHub repository.</span></span> <span data-ttu-id="00e3c-119">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="00e3c-119">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="00e3c-120">先决条件</span><span class="sxs-lookup"><span data-stu-id="00e3c-120">Prerequisites</span></span>

<span data-ttu-id="00e3c-121">必须将计算机设置为运行 .Net Core。</span><span class="sxs-lookup"><span data-stu-id="00e3c-121">You’ll need to set up your machine to run .NET core.</span></span> <span data-ttu-id="00e3c-122">有关安装说明，请访问 [.NET Core 下载](https://dotnet.microsoft.com/download)页。</span><span class="sxs-lookup"><span data-stu-id="00e3c-122">You can find the installation instructions on the [.NET Core Download](https://dotnet.microsoft.com/download) page.</span></span> <span data-ttu-id="00e3c-123">可以在 Windows、Ubuntu Linux、OS X 或 Docker 容器中运行此应用程序。</span><span class="sxs-lookup"><span data-stu-id="00e3c-123">You can run this application on Windows, Ubuntu Linux, or OS X, or in a Docker container.</span></span> <span data-ttu-id="00e3c-124">必须安装常用的代码编辑器。</span><span class="sxs-lookup"><span data-stu-id="00e3c-124">You’ll need to install your favorite code editor.</span></span> <span data-ttu-id="00e3c-125">在以下说明中，我们使用的是开放源代码跨平台编辑器 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="00e3c-125">The descriptions below use [Visual Studio Code](https://code.visualstudio.com/) which is an open source, cross-platform editor.</span></span> <span data-ttu-id="00e3c-126">不过，你可以使用习惯使用的任意工具。</span><span class="sxs-lookup"><span data-stu-id="00e3c-126">However, you can use whatever tools you are comfortable with.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="00e3c-127">创建应用程序</span><span class="sxs-lookup"><span data-stu-id="00e3c-127">Create the Application</span></span>

<span data-ttu-id="00e3c-128">第一步是新建应用程序。</span><span class="sxs-lookup"><span data-stu-id="00e3c-128">The first step is to create a new application.</span></span> <span data-ttu-id="00e3c-129">打开命令提示符，然后新建应用程序的目录。</span><span class="sxs-lookup"><span data-stu-id="00e3c-129">Open a command prompt and create a new directory for your application.</span></span> <span data-ttu-id="00e3c-130">将新建的目录设为当前目录。</span><span class="sxs-lookup"><span data-stu-id="00e3c-130">Make that the current directory.</span></span> <span data-ttu-id="00e3c-131">在命令提示符处，键入命令 `dotnet new console`。</span><span class="sxs-lookup"><span data-stu-id="00e3c-131">Type the command `dotnet new console` at the command prompt.</span></span> <span data-ttu-id="00e3c-132">这将为基本的“Hello World”应用程序创建起始文件。</span><span class="sxs-lookup"><span data-stu-id="00e3c-132">This creates the starter files for a basic "Hello World" application.</span></span>

<span data-ttu-id="00e3c-133">如果之前从未用过 C#，请参阅[这篇教程](console-teleprompter.md)，其中介绍了 C# 程序的结构。</span><span class="sxs-lookup"><span data-stu-id="00e3c-133">If you've never used C# before, [this tutorial](console-teleprompter.md) explains the structure of a C# program.</span></span> <span data-ttu-id="00e3c-134">可以阅读相应的内容，然后回到此教程详细了解 LINQ。</span><span class="sxs-lookup"><span data-stu-id="00e3c-134">You can read that and then return here to learn more about LINQ.</span></span>

## <a name="create-the-data-set"></a><span data-ttu-id="00e3c-135">创建数据集</span><span class="sxs-lookup"><span data-stu-id="00e3c-135">Create the Data Set</span></span>

<span data-ttu-id="00e3c-136">开始前，请确保下列行在 `dotnet new console` 生成的 `Program.cs` 文件的顶部：</span><span class="sxs-lookup"><span data-stu-id="00e3c-136">Before you begin, make sure that the following lines are at the top of the `Program.cs` file generated by `dotnet new console`:</span></span>

```csharp
// Program.cs
using System;
using System.Collections.Generic;
using System.Linq;
```

<span data-ttu-id="00e3c-137">如果这三行（`using` 语句）未在该文件的顶部，程序将无法编译。</span><span class="sxs-lookup"><span data-stu-id="00e3c-137">If these three lines (`using` statements) aren't at the top of the file, our program will not compile.</span></span>

<span data-ttu-id="00e3c-138">现在已具备所需的所有引用，接下来可以考虑一副扑克牌是由什么构成的。</span><span class="sxs-lookup"><span data-stu-id="00e3c-138">Now that you have all of the references that you'll need, consider what constitutes a deck of cards.</span></span> <span data-ttu-id="00e3c-139">通常一副扑克牌包含四种花色，每种花色包含 13 个值。</span><span class="sxs-lookup"><span data-stu-id="00e3c-139">Commonly, a deck of playing cards has four suits, and each suit has thirteen values.</span></span> <span data-ttu-id="00e3c-140">通常情况下，你可能会立即考虑创建一个 `Card` 类，然后手动填充一组 `Card` 对象。</span><span class="sxs-lookup"><span data-stu-id="00e3c-140">Normally, you might consider creating a `Card` class right off the bat and populating a collection of `Card` objects by hand.</span></span> <span data-ttu-id="00e3c-141">相对于通常的方式，使用 LINQ 创建一副扑克牌更加简捷。</span><span class="sxs-lookup"><span data-stu-id="00e3c-141">With LINQ, you can be more concise than the usual way of dealing with creating a deck of cards.</span></span> <span data-ttu-id="00e3c-142">可以创建两个序列来分别表示花色和点数，而非创建 `Card` 类。</span><span class="sxs-lookup"><span data-stu-id="00e3c-142">Instead of creating a `Card` class, you can create two sequences to represent suits and ranks, respectively.</span></span> <span data-ttu-id="00e3c-143">创建两个非常简单的[迭代器方法](../iterators.md#enumeration-sources-with-iterator-methods)，用于将级别和花色生成为 <xref:System.Collections.Generic.IEnumerable%601> 字符串：</span><span class="sxs-lookup"><span data-stu-id="00e3c-143">You'll create a really simple pair of [*iterator methods*](../iterators.md#enumeration-sources-with-iterator-methods) that will generate the ranks and suits as <xref:System.Collections.Generic.IEnumerable%601>s of strings:</span></span>

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

<span data-ttu-id="00e3c-144">将它们置于 `Main` 文件的 `Program.cs` 方法的下面。</span><span class="sxs-lookup"><span data-stu-id="00e3c-144">Place these underneath the `Main` method in your `Program.cs` file.</span></span> <span data-ttu-id="00e3c-145">这两种方法都利用 `yield return` 语法在运行时生成序列。</span><span class="sxs-lookup"><span data-stu-id="00e3c-145">These two methods both utilize the `yield return` syntax to produce a sequence as they run.</span></span> <span data-ttu-id="00e3c-146">编译器会生成对象来实现 <xref:System.Collections.Generic.IEnumerable%601>，并在有请求时生成字符串序列。</span><span class="sxs-lookup"><span data-stu-id="00e3c-146">The compiler builds an object that implements <xref:System.Collections.Generic.IEnumerable%601> and generates the sequence of strings as they are requested.</span></span>

<span data-ttu-id="00e3c-147">现在使用这些迭代器创建一副扑克牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-147">Now, use these iterator methods to create the deck of cards.</span></span> <span data-ttu-id="00e3c-148">将 LINQ 查询置于 `Main` 方法中。</span><span class="sxs-lookup"><span data-stu-id="00e3c-148">You'll place the LINQ query in our `Main` method.</span></span> <span data-ttu-id="00e3c-149">具体如下所示：</span><span class="sxs-lookup"><span data-stu-id="00e3c-149">Here's a look at it:</span></span>

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

<span data-ttu-id="00e3c-150">多个 `from` 子句生成 <xref:System.Linq.Enumerable.SelectMany%2A>，用于将第一个和第二个序列中的所有元素合并成一个序列。</span><span class="sxs-lookup"><span data-stu-id="00e3c-150">The multiple `from` clauses produce a <xref:System.Linq.Enumerable.SelectMany%2A>, which creates a single sequence from combining each element in the first sequence with each element in the second sequence.</span></span> <span data-ttu-id="00e3c-151">考虑到我们的目的，顺序非常重要。</span><span class="sxs-lookup"><span data-stu-id="00e3c-151">The order is important for our purposes.</span></span> <span data-ttu-id="00e3c-152">第一个源序列（花色）中的首个元素与第二个序列（等级）中的每个元素结合使用。</span><span class="sxs-lookup"><span data-stu-id="00e3c-152">The first element in the first source sequence (Suits) is combined with every element in the second sequence (Ranks).</span></span> <span data-ttu-id="00e3c-153">这就生成了第一个花色的所有十三张纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-153">This produces all thirteen cards of first suit.</span></span> <span data-ttu-id="00e3c-154">对第一个序列（花色）中的每个元素重复此过程。</span><span class="sxs-lookup"><span data-stu-id="00e3c-154">That process is repeated with each element in the first sequence (Suits).</span></span> <span data-ttu-id="00e3c-155">最后生成按花色排序（后跟值）的一副纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-155">The end result is a deck of cards ordered by suits, followed by values.</span></span>

<span data-ttu-id="00e3c-156">务必注意，无论是选择使用上文所用的查询语法编写 LINQ，还是使用方法语法，始终都可以从一种语法形式转至另一种。</span><span class="sxs-lookup"><span data-stu-id="00e3c-156">It's important to keep in mind that whether you choose to write your LINQ in the query syntax used above or use method syntax instead, it's always possible to go from one form of syntax to the other.</span></span> <span data-ttu-id="00e3c-157">可使用方法语法编写使用查询语法编写的上述查询，如下所示：</span><span class="sxs-lookup"><span data-stu-id="00e3c-157">The above query written in query syntax can be written in method syntax as:</span></span>

```csharp
var startingDeck = Suits().SelectMany(suit => Ranks().Select(rank => new { Suit = suit, Rank = rank }));
```

<span data-ttu-id="00e3c-158">编译器会将使用查询语法编写的 LINQ 语句转换为等效的方法调用语法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-158">The compiler translates LINQ statements written with query syntax into the equivalent method call syntax.</span></span> <span data-ttu-id="00e3c-159">因此无论选择哪种语法，两种查询版本生成的结果相同。</span><span class="sxs-lookup"><span data-stu-id="00e3c-159">Therefore, regardless of your syntax choice, the two versions of the query produce the same result.</span></span> <span data-ttu-id="00e3c-160">选择最适合你的情况的语法：例如，若所在的工作团队中的某些成员不擅长方法语法，则尽量首选使用查询语法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-160">Choose which syntax works best for your situation: for instance, if you're working in a team where some of the members have difficulty with method syntax, try to prefer using query syntax.</span></span>

<span data-ttu-id="00e3c-161">此时，运行已生成的示例。</span><span class="sxs-lookup"><span data-stu-id="00e3c-161">Go ahead and run the sample you've built at this point.</span></span> <span data-ttu-id="00e3c-162">将显示一副纸牌中的所有 52 张纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-162">It will display all 52 cards in the deck.</span></span> <span data-ttu-id="00e3c-163">在调试器模式下运行此示例来观察 `Suits()` 和 `Ranks()` 方法的执行情况，你可能会觉得非常有用。</span><span class="sxs-lookup"><span data-stu-id="00e3c-163">You may find it very helpful to run this sample under a debugger to observe how the `Suits()` and `Ranks()` methods execute.</span></span> <span data-ttu-id="00e3c-164">可以清楚地看到，每个序列中的所有字符串仅在需要时生成。</span><span class="sxs-lookup"><span data-stu-id="00e3c-164">You can clearly see that each string in each sequence is generated only as it is needed.</span></span>

![显示应用输出 52 张扑克牌的控制台窗口。](./media/working-with-linq/console-52-card-application.png)

## <a name="manipulate-the-order"></a><span data-ttu-id="00e3c-166">操作顺序</span><span class="sxs-lookup"><span data-stu-id="00e3c-166">Manipulate the Order</span></span>

<span data-ttu-id="00e3c-167">接下来重点介绍如何洗牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-167">Next, focus on how you're going to shuffle the cards in the deck.</span></span> <span data-ttu-id="00e3c-168">正确洗牌的第一步是将一副扑克牌分成两半。</span><span class="sxs-lookup"><span data-stu-id="00e3c-168">The first step in any good shuffle is to split the deck in two.</span></span> <span data-ttu-id="00e3c-169">LINQ API 包含的 <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 方法提供了这种功能。</span><span class="sxs-lookup"><span data-stu-id="00e3c-169">The <xref:System.Linq.Enumerable.Take%2A> and <xref:System.Linq.Enumerable.Skip%2A> methods that are part of the LINQ APIs provide that feature for you.</span></span> <span data-ttu-id="00e3c-170">将它们置于 `foreach` 循环的下面：</span><span class="sxs-lookup"><span data-stu-id="00e3c-170">Place them underneath the `foreach` loop:</span></span>

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

<span data-ttu-id="00e3c-171">但标准库中没有可供使用的洗牌方法，因此必须自行编写。</span><span class="sxs-lookup"><span data-stu-id="00e3c-171">However, there's no shuffle method to take advantage of in the standard library, so you'll have to write your own.</span></span> <span data-ttu-id="00e3c-172">将要创建的洗牌方法体现了要对基于 LINQ 的程序执行的几种操作，因此我们将逐步介绍此过程的每个部分。</span><span class="sxs-lookup"><span data-stu-id="00e3c-172">The shuffle method you'll be creating illustrates several techniques that you'll use with LINQ-based programs, so each part of this process will be explained in steps.</span></span>

<span data-ttu-id="00e3c-173">需要编写几种特殊的方法，我们称之为<xref:System.Collections.Generic.IEnumerable%601>扩展方法[，来添加一些功能，以便于与 LINQ 查询返回的 ](../programming-guide/classes-and-structs/extension-methods.md) 交互。</span><span class="sxs-lookup"><span data-stu-id="00e3c-173">In order to add some functionality to how you interact with the <xref:System.Collections.Generic.IEnumerable%601> you'll get back from LINQ queries, you'll need to write some special kinds of methods called [extension methods](../programming-guide/classes-and-structs/extension-methods.md).</span></span> <span data-ttu-id="00e3c-174">简而言之，扩展方法是具有特殊用途的静态方法，借助它无需修改你想要为其添加功能的已有原始类型，即可向其添加功能。</span><span class="sxs-lookup"><span data-stu-id="00e3c-174">Briefly, an extension method is a special purpose *static method* that adds new functionality to an already-existing type without having to modify the original type you want to add functionality to.</span></span>

<span data-ttu-id="00e3c-175">向程序添加新的静态类文件（名称为 `Extensions.cs`），以用于存放扩展方法，然后开始生成第一个扩展方法：</span><span class="sxs-lookup"><span data-stu-id="00e3c-175">Give your extension methods a new home by adding a new *static* class file to your program called `Extensions.cs`, and then start building out the first extension method:</span></span>

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

<span data-ttu-id="00e3c-176">仔细观察方法签名，尤其是参数：</span><span class="sxs-lookup"><span data-stu-id="00e3c-176">Look at the method signature for a moment, specifically the parameters:</span></span>

```csharp
public static IEnumerable<T> InterleaveSequenceWith<T> (this IEnumerable<T> first, IEnumerable<T> second)
```

<span data-ttu-id="00e3c-177">可以发现，在扩展方法的第一个自变量中添加了 `this` 修饰符。</span><span class="sxs-lookup"><span data-stu-id="00e3c-177">You can see the addition of the `this` modifier on the first argument to the method.</span></span> <span data-ttu-id="00e3c-178">也就是说，调用扩展方法，就像是第一个自变量类型的成员方法一样。</span><span class="sxs-lookup"><span data-stu-id="00e3c-178">That means you call the method as though it were a member method of the type of the first argument.</span></span> <span data-ttu-id="00e3c-179">此方法声明还遵循标准惯用做法，其中输入和输出类型为 `IEnumerable<T>`。</span><span class="sxs-lookup"><span data-stu-id="00e3c-179">This method declaration also follows a standard idiom where the input and output types are `IEnumerable<T>`.</span></span> <span data-ttu-id="00e3c-180">遵循这种做法，可以将 LINQ 方法链在一起，从而执行更复杂的查询。</span><span class="sxs-lookup"><span data-stu-id="00e3c-180">That practice enables LINQ methods to be chained together to perform more complex queries.</span></span>

<span data-ttu-id="00e3c-181">正常情况下，将扑克牌分为两半后，需要将这两半合并在一起。</span><span class="sxs-lookup"><span data-stu-id="00e3c-181">Naturally, since you split the deck into halves, you'll need to join those halves together.</span></span> <span data-ttu-id="00e3c-182">在代码中，这意味着一次性地枚举通过 <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 获得的两个序列，`interleaving` 元素，并创建一个序列：即现在洗牌后的扑克牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-182">In code, this means you'll be enumerating both of the sequences you acquired through <xref:System.Linq.Enumerable.Take%2A> and <xref:System.Linq.Enumerable.Skip%2A> at once, *`interleaving`* the elements, and creating one sequence: your now-shuffled deck of cards.</span></span> <span data-ttu-id="00e3c-183">必须了解 <xref:System.Collections.Generic.IEnumerable%601> 的工作原理，才能编写处理两个序列的 LINQ 方法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-183">Writing a LINQ method that works with two sequences requires that you understand how <xref:System.Collections.Generic.IEnumerable%601> works.</span></span>

<span data-ttu-id="00e3c-184"><xref:System.Collections.Generic.IEnumerable%601> 接口有一个方法 (<xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>)。</span><span class="sxs-lookup"><span data-stu-id="00e3c-184">The <xref:System.Collections.Generic.IEnumerable%601> interface has one method: <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A>.</span></span> <span data-ttu-id="00e3c-185"><xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> 返回的对象包含用于移动到下一个元素的方法，以及用于检索序列中当前元素的属性。</span><span class="sxs-lookup"><span data-stu-id="00e3c-185">The object returned by <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> has a method to move to the next element, and a property that retrieves the current element in the sequence.</span></span> <span data-ttu-id="00e3c-186">将使用这两个成员来枚举集合并返回元素。</span><span class="sxs-lookup"><span data-stu-id="00e3c-186">You will use those two members to enumerate the collection and return the elements.</span></span> <span data-ttu-id="00e3c-187">由于此交错方法是迭代器方法，因此将使用上面的 `yield return` 语法，而不用生成并返回集合。</span><span class="sxs-lookup"><span data-stu-id="00e3c-187">This Interleave method will be an iterator method, so instead of building a collection and returning the collection, you'll use the `yield return` syntax shown above.</span></span>

<span data-ttu-id="00e3c-188">下面是此方法的实现代码：</span><span class="sxs-lookup"><span data-stu-id="00e3c-188">Here's the implementation of that method:</span></span>

[!CODE-csharp[InterleaveSequenceWith](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet1)]

<span data-ttu-id="00e3c-189">至此，你已编写好这个方法，请回到 `Main` 方法，并进行一次洗牌：</span><span class="sxs-lookup"><span data-stu-id="00e3c-189">Now that you've written this method, go back to the `Main` method and shuffle the deck once:</span></span>

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

## <a name="comparisons"></a><span data-ttu-id="00e3c-190">比较</span><span class="sxs-lookup"><span data-stu-id="00e3c-190">Comparisons</span></span>

<span data-ttu-id="00e3c-191">进行多少次洗牌才能恢复一副纸牌的原始顺序？</span><span class="sxs-lookup"><span data-stu-id="00e3c-191">How many shuffles it takes to set the deck back to its original order?</span></span> <span data-ttu-id="00e3c-192">若要解决这个问题，需要编写用于确定两个序列是否相等的方法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-192">To find out, you'll need to write a method that determines if two sequences are equal.</span></span> <span data-ttu-id="00e3c-193">编写好此方法后，需要循环执行用于洗牌的代码，看看一副纸牌何时才能恢复原始顺序。</span><span class="sxs-lookup"><span data-stu-id="00e3c-193">After you have that method, you'll need to place the code that shuffles the deck in a loop, and check to see when the deck is back in order.</span></span>

<span data-ttu-id="00e3c-194">编写用于确定两个序列是否相等的方法应该很简单。</span><span class="sxs-lookup"><span data-stu-id="00e3c-194">Writing a method to determine if the two sequences are equal should be straightforward.</span></span> <span data-ttu-id="00e3c-195">此方法的结构与你编写的洗牌方法类似。</span><span class="sxs-lookup"><span data-stu-id="00e3c-195">It's a similar structure to the method you wrote to shuffle the deck.</span></span> <span data-ttu-id="00e3c-196">不同之处在于，这一次将比较每个序列的匹配元素，而不是 `yield return` 每个元素。</span><span class="sxs-lookup"><span data-stu-id="00e3c-196">Only this time, instead of `yield return`ing each element, you'll compare the matching elements of each sequence.</span></span> <span data-ttu-id="00e3c-197">枚举整个序列后，如果每个元素都一致，那么序列就是相同的：</span><span class="sxs-lookup"><span data-stu-id="00e3c-197">When the entire sequence has been enumerated, if every element matches, the sequences are the same:</span></span>

[!CODE-csharp[SequenceEquals](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet2)]

<span data-ttu-id="00e3c-198">这反映了另一种 LINQ 惯用做法，即终端方法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-198">This shows a second LINQ idiom: terminal methods.</span></span> <span data-ttu-id="00e3c-199">此类方法需要将序列（或在此示例中，为两个序列）用作输入，并返回一个标量值。</span><span class="sxs-lookup"><span data-stu-id="00e3c-199">They take a sequence as input (or in this case, two sequences), and return a single scalar value.</span></span> <span data-ttu-id="00e3c-200">使用终端方法时，它们始终是 LINQ 查询方法链中最后的方法，因此其名称为“终端”。</span><span class="sxs-lookup"><span data-stu-id="00e3c-200">When using terminal methods, they are always the final method in a chain of methods for a LINQ query, hence the name "terminal".</span></span>

<span data-ttu-id="00e3c-201">使用此类方法来确定一副纸牌何时恢复原始顺序时，就可以了解实际效果。</span><span class="sxs-lookup"><span data-stu-id="00e3c-201">You can see this in action when you use it to determine when the deck is back in its original order.</span></span> <span data-ttu-id="00e3c-202">循环执行洗牌代码，通过应用 `SequenceEquals()` 方法确定序列已恢复原始顺序时停止执行。</span><span class="sxs-lookup"><span data-stu-id="00e3c-202">Put the shuffle code inside a loop, and stop when the sequence is back in its original order by applying the `SequenceEquals()` method.</span></span> <span data-ttu-id="00e3c-203">你会发现，此类方法始终是任何查询中的最后一个方法，因为返回的是一个值，而不是一个序列：</span><span class="sxs-lookup"><span data-stu-id="00e3c-203">You can see it would always be the final method in any query, because it returns a single value instead of a sequence:</span></span>

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

<span data-ttu-id="00e3c-204">运行现有的代码，并记录每次洗牌时扑克牌的重写排列方式。</span><span class="sxs-lookup"><span data-stu-id="00e3c-204">Run the code you've got so far and take note of how the deck rearranges on each shuffle.</span></span> <span data-ttu-id="00e3c-205">进行 8 次洗牌后（迭代 do-while 循环），扑克牌恢复从最初的 LINQ 查询首次创建它时的原始配置。</span><span class="sxs-lookup"><span data-stu-id="00e3c-205">After 8 shuffles (iterations of the do-while loop), the deck returns to the original configuration it was in when you first created it from the starting LINQ query.</span></span>

## <a name="optimizations"></a><span data-ttu-id="00e3c-206">优化</span><span class="sxs-lookup"><span data-stu-id="00e3c-206">Optimizations</span></span>

<span data-ttu-id="00e3c-207">到目前为止，你已生成的示例执行的是向外洗牌，即每次洗牌时第一张和最后一张纸牌保持不变。</span><span class="sxs-lookup"><span data-stu-id="00e3c-207">The sample you've built so far executes an *out shuffle*, where the top and bottom cards stay the same on each run.</span></span> <span data-ttu-id="00e3c-208">让我们来做一点改变，改为使用向内洗牌，改变全部 52 张纸牌的位置。</span><span class="sxs-lookup"><span data-stu-id="00e3c-208">Let's make one change: we'll use an *in shuffle* instead, where all 52 cards change position.</span></span> <span data-ttu-id="00e3c-209">向内洗牌是指，交错一副纸牌时，将后一半中的第一张纸牌变成一副纸牌中的第一张纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-209">For an in shuffle, you interleave the deck so that the first card in the bottom half becomes the first card in the deck.</span></span> <span data-ttu-id="00e3c-210">也就是说，上半部分中的最后一张纸牌变成一副纸牌中的最后一张纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-210">That means the last card in the top half becomes the bottom card.</span></span> <span data-ttu-id="00e3c-211">这是对单行代码的简单更改。</span><span class="sxs-lookup"><span data-stu-id="00e3c-211">This is a simple change to a singular line of code.</span></span> <span data-ttu-id="00e3c-212">交换 <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 的位置，来更新当前洗牌查询。</span><span class="sxs-lookup"><span data-stu-id="00e3c-212">Update the current shuffle query by switching the positions of <xref:System.Linq.Enumerable.Take%2A> and <xref:System.Linq.Enumerable.Skip%2A>.</span></span> <span data-ttu-id="00e3c-213">这会更改一副纸牌的上半部分和下半部分的顺序：</span><span class="sxs-lookup"><span data-stu-id="00e3c-213">This will change the order of the top and bottom halves of the deck:</span></span>

```csharp
shuffle = shuffle.Skip(26).InterleaveSequenceWith(shuffle.Take(26));
```

<span data-ttu-id="00e3c-214">再次运行程序，你会发现需要进行 52 次迭代才能恢复一副纸牌的原始顺序。</span><span class="sxs-lookup"><span data-stu-id="00e3c-214">Run the program again, and you'll see that it takes 52 iterations for the deck to reorder itself.</span></span> <span data-ttu-id="00e3c-215">随着程序的继续运行，你还会开始注意到一些非常严重的性能下降问题发生。</span><span class="sxs-lookup"><span data-stu-id="00e3c-215">You'll also start to notice some serious performance degradations as the program continues to run.</span></span>

<span data-ttu-id="00e3c-216">导致这种情况出现的原因有很多。</span><span class="sxs-lookup"><span data-stu-id="00e3c-216">There are a number of reasons for this.</span></span> <span data-ttu-id="00e3c-217">可以解决导致性能下降的主要原因之一：[延迟计算](../../standard/linq/deferred-execution-lazy-evaluation.md)的使用效率低下。</span><span class="sxs-lookup"><span data-stu-id="00e3c-217">You can tackle one of the major causes of this performance drop: inefficient use of [*lazy evaluation*](../../standard/linq/deferred-execution-lazy-evaluation.md).</span></span>

<span data-ttu-id="00e3c-218">简单来说，延迟计算是指直至需要语句的值时才会执行语句计算。</span><span class="sxs-lookup"><span data-stu-id="00e3c-218">Briefly, lazy evaluation states that the evaluation of a statement is not performed until its value is needed.</span></span> <span data-ttu-id="00e3c-219">LINQ 查询属于延迟计算的语句。</span><span class="sxs-lookup"><span data-stu-id="00e3c-219">LINQ queries are statements that are evaluated lazily.</span></span> <span data-ttu-id="00e3c-220">仅当有元素请求时才生成序列。</span><span class="sxs-lookup"><span data-stu-id="00e3c-220">The sequences are generated only as the elements are requested.</span></span> <span data-ttu-id="00e3c-221">通常情况下，这是 LINQ 的主要优势所在。</span><span class="sxs-lookup"><span data-stu-id="00e3c-221">Usually, that's a major benefit of LINQ.</span></span> <span data-ttu-id="00e3c-222">不过，在诸如此程序之类的用例中，这就会导致执行时间指数式增长。</span><span class="sxs-lookup"><span data-stu-id="00e3c-222">However, in a use such as this program, this causes exponential growth in execution time.</span></span>

<span data-ttu-id="00e3c-223">应该记得原来的一副纸牌是使用 LINQ 查询生成的。</span><span class="sxs-lookup"><span data-stu-id="00e3c-223">Remember that we generated the original deck using a LINQ query.</span></span> <span data-ttu-id="00e3c-224">每次洗牌操作是通过对上一副纸牌执行三次 LINQ 查询进行。</span><span class="sxs-lookup"><span data-stu-id="00e3c-224">Each shuffle is generated by performing three LINQ queries on the previous deck.</span></span> <span data-ttu-id="00e3c-225">所有这些操作均采用惰性执行方式。</span><span class="sxs-lookup"><span data-stu-id="00e3c-225">All these are performed lazily.</span></span> <span data-ttu-id="00e3c-226">也就是说，每当有序列请求时，才会再次执行这些操作。</span><span class="sxs-lookup"><span data-stu-id="00e3c-226">That also means they are performed again each time the sequence is requested.</span></span> <span data-ttu-id="00e3c-227">执行到第 52 次迭代时，就已经重新生成很多很多次原来的一副纸牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-227">By the time you get to the 52nd iteration, you're regenerating the original deck many, many times.</span></span> <span data-ttu-id="00e3c-228">让我们来编写一个日志方法来阐明此行为。</span><span class="sxs-lookup"><span data-stu-id="00e3c-228">Let's write a log to demonstrate this behavior.</span></span> <span data-ttu-id="00e3c-229">然后，你就可以解决此问题。</span><span class="sxs-lookup"><span data-stu-id="00e3c-229">Then, you'll fix it.</span></span>

<span data-ttu-id="00e3c-230">在 `Extensions.cs` 文件中输入或复制下面的方法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-230">In your `Extensions.cs` file, type in or copy the method below.</span></span> <span data-ttu-id="00e3c-231">此扩展方法会在项目目录中新建一个名称为 `debug.log` 的文件，并将当前正在执行的查询记录到日志文件中。</span><span class="sxs-lookup"><span data-stu-id="00e3c-231">This extension method creates a new file called `debug.log` within your project directory and records what query is currently being executed to the log file.</span></span> <span data-ttu-id="00e3c-232">可以将此扩展方法追加到任何查询中，以标记查询已执行。</span><span class="sxs-lookup"><span data-stu-id="00e3c-232">This extension method can be appended to any query to mark that the query executed.</span></span>

[!CODE-csharp[LogQuery](../../../samples/snippets/csharp/getting-started/console-linq/extensions.cs?name=snippet3)]

<span data-ttu-id="00e3c-233">你将看到 `File` 下显示红色波浪线，这表示它不存在。</span><span class="sxs-lookup"><span data-stu-id="00e3c-233">You will see a red squiggle under `File`, meaning it doesn't exist.</span></span> <span data-ttu-id="00e3c-234">它将不编译，因为编译器无法识别 `File`。</span><span class="sxs-lookup"><span data-stu-id="00e3c-234">It won't compile, since the compiler doesn't know what `File` is.</span></span> <span data-ttu-id="00e3c-235">要解决此问题，请务必在 `Extensions.cs` 中第一行的下面添加以下代码行：</span><span class="sxs-lookup"><span data-stu-id="00e3c-235">To solve this problem, make sure to add the following line of code under the very first line in `Extensions.cs`:</span></span>

```csharp
using System.IO;
```

<span data-ttu-id="00e3c-236">这应可解决问题，随后红色错误将消失。</span><span class="sxs-lookup"><span data-stu-id="00e3c-236">This should solve the issue and the red error disappears.</span></span>

<span data-ttu-id="00e3c-237">接下来，使用日志消息来检测每个查询的定义：</span><span class="sxs-lookup"><span data-stu-id="00e3c-237">Next, instrument the definition of each query with a log message:</span></span>

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

<span data-ttu-id="00e3c-238">请注意，不是每次访问查询都会生成日志。</span><span class="sxs-lookup"><span data-stu-id="00e3c-238">Notice that you don't log every time you access a query.</span></span> <span data-ttu-id="00e3c-239">只有在创建原始查询时，才会生成日志。</span><span class="sxs-lookup"><span data-stu-id="00e3c-239">You log only when you create the original query.</span></span> <span data-ttu-id="00e3c-240">程序的运行时间仍然很长，但现在知道原因了。</span><span class="sxs-lookup"><span data-stu-id="00e3c-240">The program still takes a long time to run, but now you can see why.</span></span> <span data-ttu-id="00e3c-241">如果对在启用日志记录的情况下运行向内洗牌失去了耐心，请切换回向外洗牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-241">If you run out of patience running the in shuffle with logging turned on, switch back to the out shuffle.</span></span> <span data-ttu-id="00e3c-242">但仍会看到惰性计算效果。</span><span class="sxs-lookup"><span data-stu-id="00e3c-242">You'll still see the lazy evaluation effects.</span></span> <span data-ttu-id="00e3c-243">在一次运行中，共执行 2592 次查询，包括生成所有值和花色。</span><span class="sxs-lookup"><span data-stu-id="00e3c-243">In one run, it executes 2592 queries, including all the value and suit generation.</span></span>

<span data-ttu-id="00e3c-244">可以提高此处的代码性能，以减少执行次数。</span><span class="sxs-lookup"><span data-stu-id="00e3c-244">You can improve the performance of the code here to reduce the number of executions you make.</span></span> <span data-ttu-id="00e3c-245">一个简单的修复方法是缓存构造扑克牌的原始 LINQ 查询的结果。</span><span class="sxs-lookup"><span data-stu-id="00e3c-245">A simple fix you can make is to *cache* the results of the original LINQ query that constructs the deck of cards.</span></span> <span data-ttu-id="00e3c-246">目前，每当 do-while 循环进行迭代时，需要反复执行查询，每次都要重新构造扑克牌并进行洗牌。</span><span class="sxs-lookup"><span data-stu-id="00e3c-246">Currently, you're executing the queries again and again every time the do-while loop goes through an iteration, re-constructing the deck of cards and reshuffling it every time.</span></span> <span data-ttu-id="00e3c-247">可以利用 LINQ 方法 <xref:System.Linq.Enumerable.ToArray%2A> 和 <xref:System.Linq.Enumerable.ToList%2A> 来缓存扑克牌；将这两个方法追加到查询时，它们将执行你已告知它们要执行的同种操作，而现在它们会将结果存储在数组或列表中，具体取决于选择调用的方法。</span><span class="sxs-lookup"><span data-stu-id="00e3c-247">To cache the deck of cards, you can leverage the LINQ methods <xref:System.Linq.Enumerable.ToArray%2A> and <xref:System.Linq.Enumerable.ToList%2A>; when you append them to the queries, they'll perform the same actions you've told them to, but now they'll store the results in an array or a list, depending on which method you choose to call.</span></span> <span data-ttu-id="00e3c-248">将 LINQ 方法 <xref:System.Linq.Enumerable.ToArray%2A> 追加到两个查询中，并再次运行程序：</span><span class="sxs-lookup"><span data-stu-id="00e3c-248">Append the LINQ method <xref:System.Linq.Enumerable.ToArray%2A> to both queries and run the program again:</span></span>

[!CODE-csharp[Main](../../../samples/snippets/csharp/getting-started/console-linq/Program.cs?name=snippet1)]

<span data-ttu-id="00e3c-249">现在向外洗牌下降到 30 次查询。</span><span class="sxs-lookup"><span data-stu-id="00e3c-249">Now the out shuffle is down to 30 queries.</span></span> <span data-ttu-id="00e3c-250">再次运行向内洗牌程序，改善情况类似：现在它执行 162 次查询。</span><span class="sxs-lookup"><span data-stu-id="00e3c-250">Run again with the in shuffle and you'll see similar improvements: it now executes 162 queries.</span></span>

<span data-ttu-id="00e3c-251">请注意，此示例旨在突出显示延迟计算可能会导致性能下降的用例。</span><span class="sxs-lookup"><span data-stu-id="00e3c-251">Please note that this example is **designed** to highlight the use cases where lazy evaluation can cause performance difficulties.</span></span> <span data-ttu-id="00e3c-252">了解延迟计算会在何处影响代码性能至关重要，但了解并非所有查询应及早运行也同等重要。</span><span class="sxs-lookup"><span data-stu-id="00e3c-252">While it's important to see where lazy evaluation can impact code performance, it's equally important to understand that not all queries should run eagerly.</span></span> <span data-ttu-id="00e3c-253">未使用 <xref:System.Linq.Enumerable.ToArray%2A> 会导致性能损失，这是因为每次重新排列一副纸牌都要以先前的排列为依据。</span><span class="sxs-lookup"><span data-stu-id="00e3c-253">The performance hit you incur without using <xref:System.Linq.Enumerable.ToArray%2A> is because each new arrangement of the deck of cards is built from the previous arrangement.</span></span> <span data-ttu-id="00e3c-254">使用惰性计算意味着一副纸牌的每个新配置都以原来的一副纸牌为依据，甚至在执行生成 `startingDeck` 的代码时，也不例外。</span><span class="sxs-lookup"><span data-stu-id="00e3c-254">Using lazy evaluation means each new deck configuration is built from the original deck, even executing the code that built the `startingDeck`.</span></span> <span data-ttu-id="00e3c-255">这会导致大量额外的工作。</span><span class="sxs-lookup"><span data-stu-id="00e3c-255">That causes a large amount of extra work.</span></span>

<span data-ttu-id="00e3c-256">在实践中，一些算法使用及早计算的效果较好，另一些算法使用延迟计算的效果较好。</span><span class="sxs-lookup"><span data-stu-id="00e3c-256">In practice, some algorithms run well using eager evaluation, and others run well using lazy evaluation.</span></span> <span data-ttu-id="00e3c-257">对于日常使用，如果数据源为单独进程（如数据库引擎），通常更好的选择是使用延迟计算。</span><span class="sxs-lookup"><span data-stu-id="00e3c-257">For daily usage, lazy evaluation is usually a better choice when the data source is a separate process, like a database engine.</span></span> <span data-ttu-id="00e3c-258">对于数据库，使用延迟计算，更复杂的查询可以只对数据库进程执行一次往返，然后返回至剩余的代码。</span><span class="sxs-lookup"><span data-stu-id="00e3c-258">For databases, lazy evaluation allows more complex queries to execute only one round trip to the database process and back to the rest of your code.</span></span> <span data-ttu-id="00e3c-259">无论选择使用延迟计算还是及早计算，LINQ 均可以灵活处理，因此请衡量自己的进程，然后选择可为你提供最佳性能的计算种类。</span><span class="sxs-lookup"><span data-stu-id="00e3c-259">LINQ is flexible whether you choose to utilize lazy or eager evaluation, so measure your processes and pick whichever kind of evaluation gives you the best performance.</span></span>

## <a name="conclusion"></a><span data-ttu-id="00e3c-260">结束语</span><span class="sxs-lookup"><span data-stu-id="00e3c-260">Conclusion</span></span>

<span data-ttu-id="00e3c-261">在此项目中介绍了下列内容：</span><span class="sxs-lookup"><span data-stu-id="00e3c-261">In this project, you covered:</span></span>

- <span data-ttu-id="00e3c-262">使用 LINQ 查询，来将数据聚合到有意义的序列中</span><span class="sxs-lookup"><span data-stu-id="00e3c-262">using LINQ queries to aggregate data into a meaningful sequence</span></span>
- <span data-ttu-id="00e3c-263">编写扩展方法，来将自己的自定义功能添加到 LINQ 查询</span><span class="sxs-lookup"><span data-stu-id="00e3c-263">writing Extension methods to add our own custom functionality to LINQ queries</span></span>
- <span data-ttu-id="00e3c-264">查找 LINQ 查询可能会在其中遇到性能问题（如速度下降）的代码区域</span><span class="sxs-lookup"><span data-stu-id="00e3c-264">locating areas in our code where our LINQ queries might run into performance issues like degraded speed</span></span>
- <span data-ttu-id="00e3c-265">与 LINQ 查询相关的延迟计算和及早计算，以及它们对查询性能的影响</span><span class="sxs-lookup"><span data-stu-id="00e3c-265">lazy and eager evaluation in regards to LINQ queries and the implications they might have on query performance</span></span>

<span data-ttu-id="00e3c-266">除 LINQ 外，还简单介绍了魔术师用于扑克牌魔术的一个技术。</span><span class="sxs-lookup"><span data-stu-id="00e3c-266">Aside from LINQ, you learned a bit about a technique magicians use for card tricks.</span></span> <span data-ttu-id="00e3c-267">魔术师之所以采用完美洗牌是因为，可以控制每张纸牌在一副纸牌中的移动。</span><span class="sxs-lookup"><span data-stu-id="00e3c-267">Magicians use the Faro shuffle because they can control where every card moves in the deck.</span></span> <span data-ttu-id="00e3c-268">现在你了解了，也不要告诉其他人以免破坏他们的兴致！</span><span class="sxs-lookup"><span data-stu-id="00e3c-268">Now that you know, don't spoil it for everyone else!</span></span>

<span data-ttu-id="00e3c-269">有关 LINQ 的更多信息，请访问：</span><span class="sxs-lookup"><span data-stu-id="00e3c-269">For more information on LINQ, see:</span></span>

- [<span data-ttu-id="00e3c-270">语言集成查询 (LINQ)</span><span class="sxs-lookup"><span data-stu-id="00e3c-270">Language Integrated Query (LINQ)</span></span>](../programming-guide/concepts/linq/index.md)
- [<span data-ttu-id="00e3c-271">LINQ 介绍</span><span class="sxs-lookup"><span data-stu-id="00e3c-271">Introduction to LINQ</span></span>](../programming-guide/concepts/linq/index.md)
- [<span data-ttu-id="00e3c-272">基本 LINQ 查询操作 (C#)</span><span class="sxs-lookup"><span data-stu-id="00e3c-272">Basic LINQ Query Operations (C#)</span></span>](../programming-guide/concepts/linq/basic-linq-query-operations.md)
- [<span data-ttu-id="00e3c-273">使用 LINQ 进行数据转换 (C#)</span><span class="sxs-lookup"><span data-stu-id="00e3c-273">Data Transformations With LINQ (C#)</span></span>](../programming-guide/concepts/linq/data-transformations-with-linq.md)
- [<span data-ttu-id="00e3c-274">LINQ 中的查询语法和方法语法 (C#)</span><span class="sxs-lookup"><span data-stu-id="00e3c-274">Query Syntax and Method Syntax in LINQ (C#)</span></span>](../programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)
- [<span data-ttu-id="00e3c-275">支持 LINQ 的 C# 功能</span><span class="sxs-lookup"><span data-stu-id="00e3c-275">C# Features That Support LINQ</span></span>](../programming-guide/concepts/linq/features-that-support-linq.md)
