---
title: 什么是 .NET for Apache Spark？
description: 了解 .NET for Apache Spark，一种免费、开放源代码、跨平台的大数据分析框架，可在编写 .NET 代码的任何位置使用 Spark。
author: mamccrea
ms.topic: overview
ms.date: 10/09/2020
ms.openlocfilehash: 1fa3ae16334176f0e3e888087078d5b98de9d869
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873193"
---
# <a name="what-is-net-for-apache-spark"></a>什么是 .NET for Apache Spark？

[Apache Spark](what-is-spark.md) 是用于分析大型数据集（通常是以 TB 或 PB 为单位的数据）的常规分布式处理引擎。 使用 .NET for Apache Spark，为常见开放源代码大数据分析框架提供免费、[开放源代码](https://github.com/dotnet/spark)、跨平台的 .NET 支持，现可使用已知的语言将 Apache Spark 的功能添加到大数据应用程序。

## <a name="why-choose-net-for-apache-spark"></a>为什么选择 .NET for Apache Spark？

.NET for Apache Spark 为开发人员提供 .NET 体验或基本代码，以参与大数据分析领域。 .NET for Apache Spark 提供了用于通过 C# 和 F# 使用 Spark 的高性能 API。 通过 C# 和 F#，可以访问：

* 用于处理结构化数据的 DataFrame 和 SparkSQL。
* 用于处理流数据的 Spark 结构化流。
* 用于采用 SQL 语法编写查询的 Spark SQL。
* 通过集成机器学习，可以加快培训和预测速度（即结合使用 .NET for Apache Spark 和 [ML.NET](https://dot.net/ml)）。

.NET for Apache Spark 符合 .NET Standard，这是正式的 .NET API 规范，常见于 .NET 实现中。 这意味着可以在编写 .NET 代码的任何位置使用 .NET for Apache Spark，可在其中重复使用你作为 .NET 开发人员已具有的所有知识、技能、代码和库。

.NET for Apache Spark 使用 .NET Core 在 Windows、Linux 和 macOS 上运行。 它还使用 .NET Framework 在 Windows 上运行。 可以将应用程序部署到所有主要云提供商，包括 Azure HDInsight Spark、Amazon EMR Spark、Azure Databricks 和 AWS 上的 Databricks。

## <a name="net-for-apache-spark-architecture"></a>.NET for Apache Spark 体系结构

C#/F# 语言与 Spark 的绑定是在新的 Spark 互操作层上编写的，该互操作层可提供更轻松的扩展性。 这一新的 Spark 互操作层是使用语言扩展的最佳做法编写的，并针对互操作性和性能进行了优化。 长期来看，此扩展性可用于在 Spark 中添加对其他语言的支持。

> [!div class="mx-imgBorder"]
> ![.NET for Apache Spark 体系结构](media/dotnet-spark-architecture.png)

可以通过[建议](https://issues.apache.org/jira/browse/SPARK-26257)了解对 Spark 语言扩展的互操作支持。

## <a name="net-for-apache-spark-performance"></a>.NET for Apache Spark 性能

与使用 [TPC-H 基准](http://www.tpc.org/tpch/)的 Python 和 Scala 相比，.NET for Apache Spark 在大多数情况下表现良好，并且当用户定义的函数性能至关重要时，其速度比 Python 快两倍。 目前正在努力提升基准性能。

若要建立自己的基准，请参阅 [.NET for Apache Spark GitHub](https://github.com/dotnet/spark/tree/main/benchmark) 上提供的基准。

## <a name="net-for-apache-spark-roadmap"></a>.NET for Apache Spark 路线图

了解官方 [.NET for Apache Spark 路线图](https://github.com/dotnet/spark/blob/main/ROADMAP.md)的短期和长期计划。

## <a name="net-foundation"></a>.NET Foundation

.NET for Apache Spark 项目是 [.NET Foundation](https://www.dotnetfoundation.org/) 的一部分。

## <a name="contributions"></a>发布内容

.NET for Apache Spark 团队鼓励发布内容，无论是 GitHub 问题还是拉取请求。 首先，查找[现有问题](https://github.com/dotnet/spark/issues)。 如果找不到现有问题，请[创建新问题](https://github.com/dotnet/spark/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+)。

## <a name="next-steps"></a>后续步骤

尝试 .NET for Apache Spark。
> [!div class="nextstepaction"]
> [教程：.NET for Apache Spark 入门](./tutorials/get-started.md)
