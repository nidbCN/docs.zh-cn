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
# <a name="what-is-net-for-apache-spark"></a><span data-ttu-id="0857f-103">什么是 .NET for Apache Spark？</span><span class="sxs-lookup"><span data-stu-id="0857f-103">What is .NET for Apache Spark?</span></span>

<span data-ttu-id="0857f-104">[Apache Spark](what-is-spark.md) 是用于分析大型数据集（通常是以 TB 或 PB 为单位的数据）的常规分布式处理引擎。</span><span class="sxs-lookup"><span data-stu-id="0857f-104">[Apache Spark](what-is-spark.md) is a general-purpose distributed processing engine for analytics over large data sets - typically terabytes or petabytes of data.</span></span> <span data-ttu-id="0857f-105">使用 .NET for Apache Spark，为常见开放源代码大数据分析框架提供免费、[开放源代码](https://github.com/dotnet/spark)、跨平台的 .NET 支持，现可使用已知的语言将 Apache Spark 的功能添加到大数据应用程序。</span><span class="sxs-lookup"><span data-stu-id="0857f-105">With .NET for Apache Spark, the free, [open-source](https://github.com/dotnet/spark), and cross-platform .NET Support for the popular open-source big data analytics framework, you can now add the power of Apache Spark to your big data applications using languages you already know.</span></span>

## <a name="why-choose-net-for-apache-spark"></a><span data-ttu-id="0857f-106">为什么选择 .NET for Apache Spark？</span><span class="sxs-lookup"><span data-stu-id="0857f-106">Why choose .NET for Apache Spark?</span></span>

<span data-ttu-id="0857f-107">.NET for Apache Spark 为开发人员提供 .NET 体验或基本代码，以参与大数据分析领域。</span><span class="sxs-lookup"><span data-stu-id="0857f-107">.NET for Apache Spark empowers developers with .NET experience or code bases to participate in the world of big data analytics.</span></span> <span data-ttu-id="0857f-108">.NET for Apache Spark 提供了用于通过 C# 和 F# 使用 Spark 的高性能 API。</span><span class="sxs-lookup"><span data-stu-id="0857f-108">.NET for Apache Spark provides high performance APIs for using Spark from C# and F#.</span></span> <span data-ttu-id="0857f-109">通过 C# 和 F#，可以访问：</span><span class="sxs-lookup"><span data-stu-id="0857f-109">With C# and F#, you can access:</span></span>

* <span data-ttu-id="0857f-110">用于处理结构化数据的 DataFrame 和 SparkSQL。</span><span class="sxs-lookup"><span data-stu-id="0857f-110">DataFrame and SparkSQL for working with structured data.</span></span>
* <span data-ttu-id="0857f-111">用于处理流数据的 Spark 结构化流。</span><span class="sxs-lookup"><span data-stu-id="0857f-111">Spark Structured Streaming for working with streaming data.</span></span>
* <span data-ttu-id="0857f-112">用于采用 SQL 语法编写查询的 Spark SQL。</span><span class="sxs-lookup"><span data-stu-id="0857f-112">Spark SQL for writing queries with SQL syntax.</span></span>
* <span data-ttu-id="0857f-113">通过集成机器学习，可以加快培训和预测速度（即结合使用 .NET for Apache Spark 和 [ML.NET](https://dot.net/ml)）。</span><span class="sxs-lookup"><span data-stu-id="0857f-113">Machine learning integration for faster training and prediction (that is, use .NET for Apache Spark alongside [ML.NET](https://dot.net/ml)).</span></span>

<span data-ttu-id="0857f-114">.NET for Apache Spark 符合 .NET Standard，这是正式的 .NET API 规范，常见于 .NET 实现中。</span><span class="sxs-lookup"><span data-stu-id="0857f-114">.NET for Apache Spark is compliant with .NET Standard, a formal specification of .NET APIs that are common across .NET implementations.</span></span> <span data-ttu-id="0857f-115">这意味着可以在编写 .NET 代码的任何位置使用 .NET for Apache Spark，可在其中重复使用你作为 .NET 开发人员已具有的所有知识、技能、代码和库。</span><span class="sxs-lookup"><span data-stu-id="0857f-115">This means you can use .NET for Apache Spark anywhere you write .NET code allowing you to reuse all the knowledge, skills, code, and libraries you already have as a .NET developer.</span></span>

<span data-ttu-id="0857f-116">.NET for Apache Spark 使用 .NET Core 在 Windows、Linux 和 macOS 上运行。</span><span class="sxs-lookup"><span data-stu-id="0857f-116">.NET for Apache Spark runs on Windows, Linux, and macOS using .NET Core.</span></span> <span data-ttu-id="0857f-117">它还使用 .NET Framework 在 Windows 上运行。</span><span class="sxs-lookup"><span data-stu-id="0857f-117">It also runs on Windows using .NET Framework.</span></span> <span data-ttu-id="0857f-118">可以将应用程序部署到所有主要云提供商，包括 Azure HDInsight Spark、Amazon EMR Spark、Azure Databricks 和 AWS 上的 Databricks。</span><span class="sxs-lookup"><span data-stu-id="0857f-118">You can deploy your applications to all major cloud providers including Azure HDInsight Spark, Amazon EMR Spark, Azure Databricks, and Databricks on AWS.</span></span>

## <a name="net-for-apache-spark-architecture"></a><span data-ttu-id="0857f-119">.NET for Apache Spark 体系结构</span><span class="sxs-lookup"><span data-stu-id="0857f-119">.NET for Apache Spark architecture</span></span>

<span data-ttu-id="0857f-120">C#/F# 语言与 Spark 的绑定是在新的 Spark 互操作层上编写的，该互操作层可提供更轻松的扩展性。</span><span class="sxs-lookup"><span data-stu-id="0857f-120">The C#/ F# language binding to Spark is written on a new Spark interop layer which offers easier extensibility.</span></span> <span data-ttu-id="0857f-121">这一新的 Spark 互操作层是使用语言扩展的最佳做法编写的，并针对互操作性和性能进行了优化。</span><span class="sxs-lookup"><span data-stu-id="0857f-121">This new layer of Spark interop was written using the best practices for language extension and optimizes for interop and performance.</span></span> <span data-ttu-id="0857f-122">长期来看，此扩展性可用于在 Spark 中添加对其他语言的支持。</span><span class="sxs-lookup"><span data-stu-id="0857f-122">Long term, this extensibility can be used for adding support for other languages in Spark.</span></span>

> [!div class="mx-imgBorder"]
> <span data-ttu-id="0857f-123">![.NET for Apache Spark 体系结构](media/dotnet-spark-architecture.png)</span><span class="sxs-lookup"><span data-stu-id="0857f-123">![.NET for Apache Spark architecture](media/dotnet-spark-architecture.png)</span></span>

<span data-ttu-id="0857f-124">可以通过[建议](https://issues.apache.org/jira/browse/SPARK-26257)了解对 Spark 语言扩展的互操作支持。</span><span class="sxs-lookup"><span data-stu-id="0857f-124">You can learn about interop support for Spark language extensions from [the proposal](https://issues.apache.org/jira/browse/SPARK-26257).</span></span>

## <a name="net-for-apache-spark-performance"></a><span data-ttu-id="0857f-125">.NET for Apache Spark 性能</span><span class="sxs-lookup"><span data-stu-id="0857f-125">.NET for Apache Spark performance</span></span>

<span data-ttu-id="0857f-126">与使用 [TPC-H 基准](http://www.tpc.org/tpch/)的 Python 和 Scala 相比，.NET for Apache Spark 在大多数情况下表现良好，并且当用户定义的函数性能至关重要时，其速度比 Python 快两倍。</span><span class="sxs-lookup"><span data-stu-id="0857f-126">When compared against Python and Scala using the [TPC-H benchmark](http://www.tpc.org/tpch/), .NET for Apache Spark performs well in most cases and is 2x faster than Python when user-defined function performance is critical.</span></span> <span data-ttu-id="0857f-127">目前正在努力提升基准性能。</span><span class="sxs-lookup"><span data-stu-id="0857f-127">There is an ongoing effort to improve and benchmark performance.</span></span>

<span data-ttu-id="0857f-128">若要建立自己的基准，请参阅 [.NET for Apache Spark GitHub](https://github.com/dotnet/spark/tree/main/benchmark) 上提供的基准。</span><span class="sxs-lookup"><span data-stu-id="0857f-128">To do your own benchmarking, see the benchmarks available on the [.NET for Apache Spark GitHub](https://github.com/dotnet/spark/tree/main/benchmark).</span></span>

## <a name="net-for-apache-spark-roadmap"></a><span data-ttu-id="0857f-129">.NET for Apache Spark 路线图</span><span class="sxs-lookup"><span data-stu-id="0857f-129">.NET for Apache Spark roadmap</span></span>

<span data-ttu-id="0857f-130">了解官方 [.NET for Apache Spark 路线图](https://github.com/dotnet/spark/blob/main/ROADMAP.md)的短期和长期计划。</span><span class="sxs-lookup"><span data-stu-id="0857f-130">Learn about short term and long term plans from the official [.NET for Apache Spark roadmap](https://github.com/dotnet/spark/blob/main/ROADMAP.md).</span></span>

## <a name="net-foundation"></a><span data-ttu-id="0857f-131">.NET Foundation</span><span class="sxs-lookup"><span data-stu-id="0857f-131">.NET Foundation</span></span>

<span data-ttu-id="0857f-132">.NET for Apache Spark 项目是 [.NET Foundation](https://www.dotnetfoundation.org/) 的一部分。</span><span class="sxs-lookup"><span data-stu-id="0857f-132">The .NET for Apache Spark project is part of the [.NET Foundation](https://www.dotnetfoundation.org/).</span></span>

## <a name="contributions"></a><span data-ttu-id="0857f-133">发布内容</span><span class="sxs-lookup"><span data-stu-id="0857f-133">Contributions</span></span>

<span data-ttu-id="0857f-134">.NET for Apache Spark 团队鼓励发布内容，无论是 GitHub 问题还是拉取请求。</span><span class="sxs-lookup"><span data-stu-id="0857f-134">The .NET for Apache Spark team encourages contributions, both GitHub issues and pull requests.</span></span> <span data-ttu-id="0857f-135">首先，查找[现有问题](https://github.com/dotnet/spark/issues)。</span><span class="sxs-lookup"><span data-stu-id="0857f-135">First, look for an [existing issue](https://github.com/dotnet/spark/issues).</span></span> <span data-ttu-id="0857f-136">如果找不到现有问题，请[创建新问题](https://github.com/dotnet/spark/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+)。</span><span class="sxs-lookup"><span data-stu-id="0857f-136">If you can't find an existing issue, [open a new issue](https://github.com/dotnet/spark/issues?utf8=%E2%9C%93&q=is%3Aissue+is%3Aopen+).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0857f-137">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0857f-137">Next steps</span></span>

<span data-ttu-id="0857f-138">尝试 .NET for Apache Spark。</span><span class="sxs-lookup"><span data-stu-id="0857f-138">Try .NET for Apache Spark.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="0857f-139">教程：.NET for Apache Spark 入门</span><span class="sxs-lookup"><span data-stu-id="0857f-139">Tutorial: Get started with .NET for Apache Spark</span></span>](./tutorials/get-started.md)
