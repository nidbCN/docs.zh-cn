---
title: 在 .NET for Apache Spark 交互环境中编写和调用 UDF。
description: 了解如何在 .NET for Apache Spark 交互 shell 中编写和调用 UDF。
ms.author: nidutta
author: Niharikadutta
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: c29c3a9f6269a342d1051d6d979a4e3adb42da02
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875546"
---
# <a name="write-and-call-udfs-in-net-for-apache-spark-interactive-environments"></a><span data-ttu-id="51cf7-103">在 .NET for Apache Spark 交互环境中编写和调用 UDF</span><span class="sxs-lookup"><span data-stu-id="51cf7-103">Write and call UDFs in .NET for Apache Spark interactive environments</span></span>

<span data-ttu-id="51cf7-104">本文介绍如何在 .NET for Apache Spark 交互环境中使用用户定义的函数 (UDF)。</span><span class="sxs-lookup"><span data-stu-id="51cf7-104">In this article, you will learn how to use user-defined functions (UDFs) in a .NET for Apache Spark interactive environment.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="51cf7-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="51cf7-105">Prerequisites</span></span>

1. <span data-ttu-id="51cf7-106">安装 [.NET Interactive](https://github.com/dotnet/interactive)</span><span class="sxs-lookup"><span data-stu-id="51cf7-106">Install [.NET Interactive](https://github.com/dotnet/interactive)</span></span>
2. <span data-ttu-id="51cf7-107">安装 [Jupyter Lab](https://jupyter.org/)</span><span class="sxs-lookup"><span data-stu-id="51cf7-107">Install [Jupyter lab](https://jupyter.org/)</span></span>

## <a name="net-for-apache-spark-interactive-experience"></a><span data-ttu-id="51cf7-108">.NET for Apache Spark Interactive 体验</span><span class="sxs-lookup"><span data-stu-id="51cf7-108">.NET for Apache Spark Interactive experience</span></span>

<span data-ttu-id="51cf7-109">[.NET for Apache Spark](https://github.com/dotnet/spark) 使用 [.NET Interactive](https://devblogs.microsoft.com/dotnet/net-interactive-is-here-net-notebooks-preview-2/) 为 Spark 内的交互式体验提供 .NET 支持。</span><span class="sxs-lookup"><span data-stu-id="51cf7-109">[.NET for Apache Spark](https://github.com/dotnet/spark) uses [.NET Interactive](https://devblogs.microsoft.com/dotnet/net-interactive-is-here-net-notebooks-preview-2/) to provide .NET support for interactive experience inside Spark.</span></span> <span data-ttu-id="51cf7-110">若要了解如何设置环境以尝试在 Jupyter 笔记本中使用 .NET Interactive，请参阅 [.NET Interactive 存储库](https://github.com/dotnet/interactive)。</span><span class="sxs-lookup"><span data-stu-id="51cf7-110">To understand how to set up the environment to try .NET Interactive with Jupyter notebooks, see the [.NET Interactive repository](https://github.com/dotnet/interactive).</span></span>

<span data-ttu-id="51cf7-111">此外，我们还建议你阅读[在 .NET for Apache Spark 中序列化 UDF 这篇文章](udf-guide.md)，了解是什么 UDF，以及如何在 .NET for Apache Spark 中对其进行序列化。</span><span class="sxs-lookup"><span data-stu-id="51cf7-111">Also, it is recommended to go through [this article about UDF serialization in .NET for Apache Spark](udf-guide.md) to understand what UDFs are and how they are serialized in .NET for Apache Spark.</span></span>
<span data-ttu-id="51cf7-112">本指南使用 Jupyter Notebook 来说明如何在交互式体验中使用 UDF。</span><span class="sxs-lookup"><span data-stu-id="51cf7-112">This guide uses Jupyter Notebooks to illustrate how to use UDFs in an interactive experience.</span></span>

## <a name="define-a-udf-in-net-interactive"></a><span data-ttu-id="51cf7-113">在 .NET Interactive 中定义 UDF</span><span class="sxs-lookup"><span data-stu-id="51cf7-113">Define a UDF in .NET Interactive</span></span>

<span data-ttu-id="51cf7-114">在交互式体验中，可以将单元格视作在 [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) 的一次迭代过程中提交的代码片段。</span><span class="sxs-lookup"><span data-stu-id="51cf7-114">In the interactive experience, a cell can be thought of as the code snippet being submitted as part of one iteration of the [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop).</span></span> <span data-ttu-id="51cf7-115">例如，请查看以下笔记本了解其含义：</span><span class="sxs-lookup"><span data-stu-id="51cf7-115">For example, take a look at the following notebook to understand what that means:</span></span>

![Jupyter Notebook 单元格](./media/dotnet-interactive/dotnet-interactive-cells.png)

<span data-ttu-id="51cf7-117">标有红色箭头的每个块都是一个单元格，也代表 Spark 中的一次代码提交。</span><span class="sxs-lookup"><span data-stu-id="51cf7-117">Each of those blocks marked by the red arrow is one cell, or one submission of code to Spark.</span></span> <span data-ttu-id="51cf7-118">现在，在定义引用自定义的用户定义对象的 UDF 时，必须在定义被该 UDF 引用的对象所在的单元格中定义该 UDF。</span><span class="sxs-lookup"><span data-stu-id="51cf7-118">Now when defining a UDF that references a custom user-defined object, it is required that the UDF be defined in the same cell where the object it references is defined.</span></span> <span data-ttu-id="51cf7-119">让我们看看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="51cf7-119">Let's look at what that looks like as an example:</span></span>

![正常运作的 UDF](./media/dotnet-interactive/working-udf.png)

## <a name="call-a-udf-on-a-dataframe"></a><span data-ttu-id="51cf7-121">在 DataFrame 上调用 UDF</span><span class="sxs-lookup"><span data-stu-id="51cf7-121">Call a UDF on a DataFrame</span></span>

<span data-ttu-id="51cf7-122">在 `DataFrame` 上调用以前定义的 UDF 时，请务必确保在提交的单元格中先定义该 UDF，然后再调用它，如前面示例中所示。</span><span class="sxs-lookup"><span data-stu-id="51cf7-122">When calling a previously defined UDF on a `DataFrame` it is important to make sure the UDF is defined in a previously submitted cell, before calling it, as can be seen in the previous example.</span></span>

<span data-ttu-id="51cf7-123">现在，让我们看看在定义该 UDF 的单元格中调用它会发生什么情况。</span><span class="sxs-lookup"><span data-stu-id="51cf7-123">Now let's see what happens if we call the UDF in the same cell where it is defined.</span></span>

![失败的 UDF 调用](./media/dotnet-interactive/udf_fails.png)

<span data-ttu-id="51cf7-125">出现上面那种突出显示的错误，是因为 UDF 程序集需要先进行编译并发送到工作器，然后才能在 DataFrame 上调用它。</span><span class="sxs-lookup"><span data-stu-id="51cf7-125">The above highlighted error is because the UDF assemblies need to first be compiled and shipped to the workers before it can be called on a DataFrame.</span></span>

<span data-ttu-id="51cf7-126">这些是在 .NET for Apache Spark 交互式体验（如 [Azure Synapse Notebook](/azure/synapse-analytics/spark/apache-spark-development-using-notebooks)）中实现 UDF 时需要记住的几个要点。</span><span class="sxs-lookup"><span data-stu-id="51cf7-126">These are a few important things to keep in mind while implementing UDFs in .NET for Apache Spark interactive experience (such as [Azure Synapse Notebooks](/azure/synapse-analytics/spark/apache-spark-development-using-notebooks)).</span></span>

## <a name="faqs"></a><span data-ttu-id="51cf7-127">常见问题解答</span><span class="sxs-lookup"><span data-stu-id="51cf7-127">FAQs</span></span>

1. <span data-ttu-id="51cf7-128">**为什么我的 UDF 在引用自定义的用户定义对象时会引发错误`Type Submission#_ is not marked as serializable`？**</span><span class="sxs-lookup"><span data-stu-id="51cf7-128">**Why does my UDF referencing a custom user-defined object throw the error `Type Submission#_ is not marked as serializable`?**</span></span>
    <span data-ttu-id="51cf7-129">.NET Interactive 使用单元格提交编号的包装类包装每个单元格，以便对要提交的每个单元格进行唯一标识。</span><span class="sxs-lookup"><span data-stu-id="51cf7-129">.NET Interactive wraps each of these cells with a wrapper class of the cell submission number, to uniquely identify each cell being submitted.</span></span> <span data-ttu-id="51cf7-130">正如[本指南](udf-guide.md)中详细解释的那样，现在当序列化引用自定义对象的 UDF 时，还会选取它的目标进行序列化，在这种情况下，.NET Interactive 会由在其中定义自定义对象的单元格的包装类进行包装。</span><span class="sxs-lookup"><span data-stu-id="51cf7-130">Now as explained in detail in [this guide](udf-guide.md), when a UDF that references a custom object is being serialized its target is also picked up for serialization, which in the case of .NET Interactive gets wrapped by the wrapper class of the cell where the custom object is defined.</span></span>
    <span data-ttu-id="51cf7-131">接下来，让我们看看这会如何影响我们在笔记本中定义 UDF：</span><span class="sxs-lookup"><span data-stu-id="51cf7-131">Now let's see how that affects our UDF definition in the notebook:</span></span>

    ![UDF 序列化错误](./media/dotnet-interactive/udf-serialization-error.png)

    <span data-ttu-id="51cf7-133">在 `udf2_fails` 的情况下，我们会看到提示类型 `Submission#7` 未标记为可序列化的错误消息，这是因为 .NET Interactive 包装了单元格中定义的每个对象及其 `Submission#` 类，这是动态生成的，因此未标记为 `Serializable`。</span><span class="sxs-lookup"><span data-stu-id="51cf7-133">As can be seen in the case of `udf2_fails`, we see the error message that says Type `Submission#7` is not marked as serializable, this is because .NET Interactive wraps every object defined in a cell with its `Submission#` class, which is generated on the fly and hence is not marked as `Serializable`.</span></span>

    <span data-ttu-id="51cf7-134">为此，引用自定义对象的 UDF 必须在该对象所在的单元格中进行定义。</span><span class="sxs-lookup"><span data-stu-id="51cf7-134">For this reason, it is **required that a UDF referencing a custom object is defined in the same cell as that object**.</span></span>

2. <span data-ttu-id="51cf7-135">**为什么广播变量无法与 .NET Interactive 一起使用？**</span><span class="sxs-lookup"><span data-stu-id="51cf7-135">**Why don't Broadcast Variables work with .NET Interactive?**</span></span>
    <span data-ttu-id="51cf7-136">原因和前面所述的一样，广播变量不能与 .NET Interactive 一起使用。</span><span class="sxs-lookup"><span data-stu-id="51cf7-136">For the reasons explained previously, broadcast variables don't work with .NET Interactive.</span></span> <span data-ttu-id="51cf7-137">最好是浏览这篇[变量指南](broadcast-guide.md)，更深入地了解什么是广播变量以及如何使用它们。</span><span class="sxs-lookup"><span data-stu-id="51cf7-137">It is a good idea to go through [this guide on broadcast variables](broadcast-guide.md) to get a deeper understanding of what broadcast variables are and how to use them.</span></span> <span data-ttu-id="51cf7-138">广播变量不能与交互式方案一起使用的原因在于，.NET Interactive 的设计会将单元格中定义的每个对象都追加到其单元格提交类中，这是因为未标记为可序列化，因此失败，并出现如前所示的同一异常。</span><span class="sxs-lookup"><span data-stu-id="51cf7-138">The reason broadcast variables don't work with interactive scenarios is because of .NET interactive's design of appending each object defined in a cell with its cell submission class, which since is not marked serializable, fails with the same exception as shown previously.</span></span>
    <span data-ttu-id="51cf7-139">下面的示例对此进行了深入探讨：</span><span class="sxs-lookup"><span data-stu-id="51cf7-139">Let's dive a little deeper with the following example:</span></span>

    ![广播变量失败](./media/dotnet-interactive/broadcast-fails.png)

    <span data-ttu-id="51cf7-141">如前面几节中所述，虽然我们在同一单元格中定义 UDF 及其引用的对象（在本例中为广播变量），但仍会看到提示 `Microsoft.Spark.Sql.Session` 未标记为可序列化的 `SerializationException` 错误。</span><span class="sxs-lookup"><span data-stu-id="51cf7-141">As recommended in the previous sections, we define both the UDF and the object it is referencing (broadcast variable in this case) in the same cell, but we still see the `SerializationException` error complaining about `Microsoft.Spark.Sql.Session` not being marked as serializable.</span></span> <span data-ttu-id="51cf7-142">这是因为当编译器尝试序列化广播变量对象 `bv` 时，发现其名称追加到了 [`SparkSession`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/SparkSession.cs#L20) 对象 `spark` 中，而该对象需要标记为可序列化。</span><span class="sxs-lookup"><span data-stu-id="51cf7-142">This is because when the compiler tries to serialize the broadcast variable object `bv`, it finds its name to be appended with [`SparkSession`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/SparkSession.cs#L20) object `spark`, which it requires to be marked as serializable.</span></span> <span data-ttu-id="51cf7-143">通过查看此单元格提交的反向编译程序集，可以更轻松地演示这种情形：</span><span class="sxs-lookup"><span data-stu-id="51cf7-143">This can be demonstrated with more ease by taking a peek at the decompiled assembly of this cell submission:</span></span>

    ![反向编译程序集代码](./media/dotnet-interactive/decompiledAssembly.png)

    <span data-ttu-id="51cf7-145">如果将 [`SparkSession`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/SparkSession.cs#L20) 类标记为 `[Serializable]`，则可以使用此解决方案，但它并不是理想选择，因为我们不想让用户序列化 SparkSession 对象，这可能会导致一些奇怪的非预期行为。</span><span class="sxs-lookup"><span data-stu-id="51cf7-145">If we mark the [`SparkSession`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/SparkSession.cs#L20) class as `[Serializable]`, we can get this to work, but this is not an ideal solution as we don't want to give the user the ability to serialize a SparkSession object, as that could lead to some weird, undesirable behavior.</span></span> <span data-ttu-id="51cf7-146">这是一个[已知问题](https://github.com/dotnet/spark/issues/619)，将在未来版本中解决。</span><span class="sxs-lookup"><span data-stu-id="51cf7-146">This is a [known issue](https://github.com/dotnet/spark/issues/619) and will be resolved in future versions.</span></span>
