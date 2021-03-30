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
# <a name="write-and-call-udfs-in-net-for-apache-spark-interactive-environments"></a>在 .NET for Apache Spark 交互环境中编写和调用 UDF

本文介绍如何在 .NET for Apache Spark 交互环境中使用用户定义的函数 (UDF)。

## <a name="prerequisites"></a>先决条件

1. 安装 [.NET Interactive](https://github.com/dotnet/interactive)
2. 安装 [Jupyter Lab](https://jupyter.org/)

## <a name="net-for-apache-spark-interactive-experience"></a>.NET for Apache Spark Interactive 体验

[.NET for Apache Spark](https://github.com/dotnet/spark) 使用 [.NET Interactive](https://devblogs.microsoft.com/dotnet/net-interactive-is-here-net-notebooks-preview-2/) 为 Spark 内的交互式体验提供 .NET 支持。 若要了解如何设置环境以尝试在 Jupyter 笔记本中使用 .NET Interactive，请参阅 [.NET Interactive 存储库](https://github.com/dotnet/interactive)。

此外，我们还建议你阅读[在 .NET for Apache Spark 中序列化 UDF 这篇文章](udf-guide.md)，了解是什么 UDF，以及如何在 .NET for Apache Spark 中对其进行序列化。
本指南使用 Jupyter Notebook 来说明如何在交互式体验中使用 UDF。

## <a name="define-a-udf-in-net-interactive"></a>在 .NET Interactive 中定义 UDF

在交互式体验中，可以将单元格视作在 [REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop) 的一次迭代过程中提交的代码片段。 例如，请查看以下笔记本了解其含义：

![Jupyter Notebook 单元格](./media/dotnet-interactive/dotnet-interactive-cells.png)

标有红色箭头的每个块都是一个单元格，也代表 Spark 中的一次代码提交。 现在，在定义引用自定义的用户定义对象的 UDF 时，必须在定义被该 UDF 引用的对象所在的单元格中定义该 UDF。 让我们看看下面的示例：

![正常运作的 UDF](./media/dotnet-interactive/working-udf.png)

## <a name="call-a-udf-on-a-dataframe"></a>在 DataFrame 上调用 UDF

在 `DataFrame` 上调用以前定义的 UDF 时，请务必确保在提交的单元格中先定义该 UDF，然后再调用它，如前面示例中所示。

现在，让我们看看在定义该 UDF 的单元格中调用它会发生什么情况。

![失败的 UDF 调用](./media/dotnet-interactive/udf_fails.png)

出现上面那种突出显示的错误，是因为 UDF 程序集需要先进行编译并发送到工作器，然后才能在 DataFrame 上调用它。

这些是在 .NET for Apache Spark 交互式体验（如 [Azure Synapse Notebook](/azure/synapse-analytics/spark/apache-spark-development-using-notebooks)）中实现 UDF 时需要记住的几个要点。

## <a name="faqs"></a>常见问题解答

1. **为什么我的 UDF 在引用自定义的用户定义对象时会引发错误`Type Submission#_ is not marked as serializable`？**
    .NET Interactive 使用单元格提交编号的包装类包装每个单元格，以便对要提交的每个单元格进行唯一标识。 正如[本指南](udf-guide.md)中详细解释的那样，现在当序列化引用自定义对象的 UDF 时，还会选取它的目标进行序列化，在这种情况下，.NET Interactive 会由在其中定义自定义对象的单元格的包装类进行包装。
    接下来，让我们看看这会如何影响我们在笔记本中定义 UDF：

    ![UDF 序列化错误](./media/dotnet-interactive/udf-serialization-error.png)

    在 `udf2_fails` 的情况下，我们会看到提示类型 `Submission#7` 未标记为可序列化的错误消息，这是因为 .NET Interactive 包装了单元格中定义的每个对象及其 `Submission#` 类，这是动态生成的，因此未标记为 `Serializable`。

    为此，引用自定义对象的 UDF 必须在该对象所在的单元格中进行定义。

2. **为什么广播变量无法与 .NET Interactive 一起使用？**
    原因和前面所述的一样，广播变量不能与 .NET Interactive 一起使用。 最好是浏览这篇[变量指南](broadcast-guide.md)，更深入地了解什么是广播变量以及如何使用它们。 广播变量不能与交互式方案一起使用的原因在于，.NET Interactive 的设计会将单元格中定义的每个对象都追加到其单元格提交类中，这是因为未标记为可序列化，因此失败，并出现如前所示的同一异常。
    下面的示例对此进行了深入探讨：

    ![广播变量失败](./media/dotnet-interactive/broadcast-fails.png)

    如前面几节中所述，虽然我们在同一单元格中定义 UDF 及其引用的对象（在本例中为广播变量），但仍会看到提示 `Microsoft.Spark.Sql.Session` 未标记为可序列化的 `SerializationException` 错误。 这是因为当编译器尝试序列化广播变量对象 `bv` 时，发现其名称追加到了 [`SparkSession`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/SparkSession.cs#L20) 对象 `spark` 中，而该对象需要标记为可序列化。 通过查看此单元格提交的反向编译程序集，可以更轻松地演示这种情形：

    ![反向编译程序集代码](./media/dotnet-interactive/decompiledAssembly.png)

    如果将 [`SparkSession`](https://github.com/dotnet/spark/blob/main/src/csharp/Microsoft.Spark/Sql/SparkSession.cs#L20) 类标记为 `[Serializable]`，则可以使用此解决方案，但它并不是理想选择，因为我们不想让用户序列化 SparkSession 对象，这可能会导致一些奇怪的非预期行为。 这是一个[已知问题](https://github.com/dotnet/spark/issues/619)，将在未来版本中解决。
