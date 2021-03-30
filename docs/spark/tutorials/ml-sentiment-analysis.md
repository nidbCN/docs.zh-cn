---
title: 使用 .NET for Apache Spark 和 ML.NET 进行情绪分析教程
description: 本教程介绍了如何搭配使用 ML.NET 和 .NET for Apache Spark 来进行情绪分析。
author: mamccrea
ms.author: mamccrea
ms.date: 10/09/2020
ms.topic: tutorial
ms.openlocfilehash: a21b92f910d82cbddd2976adeb292cea5f7bc78b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876859"
---
# <a name="tutorial-sentiment-analysis-with-net-for-apache-spark-and-mlnet"></a>教程：使用 .NET for Apache Spark 和 ML.NET 进行情绪分析

本教程介绍了如何使用 ML.NET 和 .NET for Apache Spark 对在线评论进行情绪分析。 [ML.NET](http://dot.net/ml) 是免费的跨平台开放源代码机器学习框架。 可以搭配使用 ML.NET 和 .NET for Apache Spark，来缩放机器学习算法训练和预测。

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 使用 Visual Studio 中的 ML.NET 模型生成器创建情绪分析模型。
> * 创建 .NET for Apache Spark 控制台应用。
> * 编写并实现用户定义的函数。
> * 运行 .NET for Apache Spark 控制台应用。

## <a name="prerequisites"></a>先决条件

* 如果以前未开发过 .NET for Apache Spark 应用程序，请从[入门教程](get-started.md)开始，以熟悉基础知识。 完成“入门教程”的所有先决条件，然后再继续使用此教程。

* 此教程使用 Visual Studio 提供的可视界面 ML.NET 模型生成器（预览）。 如果还没有 Visual Studio，可免费[下载 Visual Studio Community 版本](https://visualstudio.microsoft.com/downloads/)。

* [下载并安装](https://marketplace.visualstudio.com/items?itemName=MLNET.07) ML.NET 模型生成器（预览）。

* 下载 Yelp 评论数据集：[yelptest.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptest.csv) 和 [yelptrain.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv)。

## <a name="review-the-data"></a>查看数据

Yelp 评论数据集包含关于各种服务的 Yelp 在线评论。 打开 [yelptrain.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv)，并注意观察其数据结构。 第一列包含评论文本，第二列包含情绪评分。 若情绪评分为 1，则为积极评论，若情绪评分为 0，则为消极评论。

下表包含示例数据：

|ReviewText|情绪|
|-|-|
|哇...喜欢这个地方。|    1|
|酥皮不行。|    0|

## <a name="build-your-machine-learning-model"></a>构建机器学习模型

1. 打开 Visual Studio 并创建新的 C# 控制台应用程序 (.NET Core)。 将项目命名为 MLSparkModel  。

1. 在“解决方案资源管理器”  中，右键单击“MLSparkModel”项目。  然后选择“添加 > 机器学习”  。

1. 从 ML.NET 模型生成器中选择“情绪分析”方案磁贴。 

1. 在“添加数据”页中上传 yelptrain.csv 数据集。  

1. 从“要预测的列”下拉列表中选择“情绪”。  

1. 在“训练”页中，将训练时间设置为 60 秒，然后选择“开始训练”。    注意“进度”中的训练状态。 

1. 模型生成器完成训练后，评估训练结果。  可以将短语输入到“试用模型”下方的文本框，然后选择“预测”，查看输出。  

1. 选择“代码”，然后选择“添加项目”，将此 ML 模型添加到解决方案。  

1. 请注意将两个项目添加到了解决方案：**MLSparkModelML.ConsoleApp** 和 **MLSparkModelML.Model**。

1. 双击 MLSpark C# 项目，请注意此时已添加了下面的项目引用。 

   ```xml
   <ItemGroup>
       <ProjectReference Include="..\MLSparkModelML.Model\MLSparkModelML.Model.csproj" />
   </ItemGroup>
   ```

## <a name="create-a-console-app"></a>创建控制台应用

模型生成器会生成控制台应用。

1. 在解决方案资源管理器中，右键单击“MLSparkModelML.Console”，然后选择“管理 NuGet 包”   。

1. 搜索 Microsoft.Spark，并安装此包。  模型生成器会自动安装 Microsoft.ML。 

### <a name="create-a-sparksession"></a>创建 SparkSession

1. 打开 MLSparkModelML.ConsoleApp 的 Program.cs 文件。   此文件已由模型生成器自动生成。 删除 `using` 语句、Main() 方法的内容以及 `CreateSingleDataSample` 区域。

1. 将以下附加的 `using` 语句添加到“Program.cs”  顶部：

   ```csharp
   using System;
   using System.Collections.Generic;
   using Microsoft.ML;
   using Microsoft.ML.Data;
   using Microsoft.Spark.Sql;
   using MLSparkModelML.Model;
   ```

1. 将 `DATA_FILEPATH` 更改为 yelptest.csv 的路径。 

1. 将以下代码添加到 `Main` 方法，以创建新的 `SparkSession`。 Spark 会话是使用数据集和数据帧 API 对 Spark 进行编程的入口点。

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName(".NET for Apache Spark Sentiment Analysis")
        .GetOrCreate();
   ```

   通过调用上面创建的 spark 对象，可在整个程序中访问 Spark 和数据帧功能  。

### <a name="create-a-dataframe-and-print-to-console"></a>创建 DataFrame 并打印到控制台

将 yelptest.csv 文件作为 `DataFrame`，读入其中的 Yelp 评论数据。  包括 `header` 和 `inferSchema` 选项。 `header` 选项将 yelptest.csv 的第一行作为列名称读取，而非作为数据。  `inferSchema` 选项基于数据推断列类型。

```csharp
DataFrame df = spark
    .ReadStream()
    .Option("header", true)
    .Option("inferSchema", true)
    .Csv(DATA_FILEPATH);

df.Show();
```

### <a name="register-a-user-defined-function"></a>注册用户定义的函数

可以使用 Spark 应用程序中的 UDF（用户定义的函数）来对数据执行计算和分析  。 本教程搭配使用 ML.NET 和 UDF 来评估每个 Yelp 评论。

将以下代码添加到 `Main` 方法，以注册名为 `MLudf` 的 UDF。

```csharp
spark.Udf()
    .Register<string, bool>("MLudf", predict);
```

此 UDF 将 Yelp 评论字符串用作输入，并分别对积极情绪或消极情绪输出 true 或 false。 它使用 predict() 方法，你将在后面的步骤中定义此方法。 

### <a name="use-spark-sql-to-call-the-udf"></a>使用 Spark SQL 调用 UDF

现已读入数据并已并入 ML，接下来使用 Spark SQL 调用对 DataFrame 各行运行情绪分析的 UDF。 将以下代码添加到 `Main` 方法：

```csharp
// Use Spark SQL to call ML.NET UDF
// Display results of sentiment analysis on reviews
df.CreateOrReplaceTempView("Reviews");
DataFrame sqlDf = spark.Sql("SELECT ReviewText, MLudf(ReviewText) FROM Reviews");
sqlDf.Show();

// Print out first 20 rows of data
// Prevent data getting cut off by setting truncate = 0
sqlDf.Show(20, 0, false);

spark.Stop();
```

### <a name="create-predict-method"></a>创建 predict() 方法

在 `Main()` 方法的前面添加以下代码。 此代码类似于模型生成器生成的 ConsumeModel.cs 的内容。  将此方法移到控制台后，每次运行应用时，都将加载此模型负载。

```csharp
private static readonly PredictionEngine<ModelInput, ModelOutput> _predictionEngine;

static Program()
{
    MLContext mlContext = new MLContext();
    ITransformer model = mlContext.Model.Load("MLModel.zip", out DataViewSchema schema);
    _predictionEngine = mlContext.Model.CreatePredictionEngine<ModelInput, ModelOutput>(model);
}

static bool predict(string text)
{
    ModelInput input = new ModelInput
    {
        ReviewText = text
    };

    return _predictionEngine.Predict(input).Prediction;
}
```

## <a name="add-the-model-to-your-console-app"></a>将模型添加到控制台应用

在解决方案资源管理器中，从 MLSparkModelML.Model 项目复制 MLModel.zip，并将其粘贴到 MLSparkModelML.ConsoleApp 项目中。    将自动在 MLSparkModelML.ConsoleApp.csproj 中添加引用。 

## <a name="run-your-code"></a>运行代码

使用 `spark-submit` 运行代码。 使用命令提示符导航到控制台应用的根文件夹，运行下面的命令。

首先，清理并发布应用。

```dotnetcli
dotnet clean
dotnet publish
```

然后导航到控制台应用的发布文件夹，运行下面的 `spark-submit` 命令。 务必使用 Microsoft Spark jar 文件的实际路径更新此命令。

```dotnetcli
%SPARK_HOME%\bin\spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local microsoft-spark-2-4_2.11-1.0.0.jar dotnet MLSparkModelML.ConsoleApp.dll
```

## <a name="get-the-code"></a>获取代码

本教程与[使用大数据进行情绪分析](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment)示例中的代码相似。

## <a name="next-steps"></a>后续步骤

继续阅读下一篇文章，了解如何使用 .NET for Apache Spark 进行结构化流式处理。
> [!div class="nextstepaction"]
> [教程：使用 .NET for Apache Spark 进行结构化流式处理](streaming.md)
