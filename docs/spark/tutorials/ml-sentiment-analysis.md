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
# <a name="tutorial-sentiment-analysis-with-net-for-apache-spark-and-mlnet"></a><span data-ttu-id="48c9b-103">教程：使用 .NET for Apache Spark 和 ML.NET 进行情绪分析</span><span class="sxs-lookup"><span data-stu-id="48c9b-103">Tutorial: Sentiment analysis with .NET for Apache Spark and ML.NET</span></span>

<span data-ttu-id="48c9b-104">本教程介绍了如何使用 ML.NET 和 .NET for Apache Spark 对在线评论进行情绪分析。</span><span class="sxs-lookup"><span data-stu-id="48c9b-104">This tutorial teaches you how to do sentiment analysis of online reviews using ML.NET and .NET for Apache Spark.</span></span> <span data-ttu-id="48c9b-105">[ML.NET](http://dot.net/ml) 是免费的跨平台开放源代码机器学习框架。</span><span class="sxs-lookup"><span data-stu-id="48c9b-105">[ML.NET](http://dot.net/ml) is a free, cross-platform, open-source machine learning framework.</span></span> <span data-ttu-id="48c9b-106">可以搭配使用 ML.NET 和 .NET for Apache Spark，来缩放机器学习算法训练和预测。</span><span class="sxs-lookup"><span data-stu-id="48c9b-106">You can use ML.NET with .NET for Apache Spark to scale the training and prediction of machine learning algorithms.</span></span>

<span data-ttu-id="48c9b-107">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="48c9b-107">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="48c9b-108">使用 Visual Studio 中的 ML.NET 模型生成器创建情绪分析模型。</span><span class="sxs-lookup"><span data-stu-id="48c9b-108">Create a sentiment analysis model using ML.NET Model Builder in Visual Studio.</span></span>
> * <span data-ttu-id="48c9b-109">创建 .NET for Apache Spark 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="48c9b-109">Create a .NET for Apache Spark console app.</span></span>
> * <span data-ttu-id="48c9b-110">编写并实现用户定义的函数。</span><span class="sxs-lookup"><span data-stu-id="48c9b-110">Write and implement a user-defined function.</span></span>
> * <span data-ttu-id="48c9b-111">运行 .NET for Apache Spark 控制台应用。</span><span class="sxs-lookup"><span data-stu-id="48c9b-111">Run a .NET for Apache Spark console app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48c9b-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="48c9b-112">Prerequisites</span></span>

* <span data-ttu-id="48c9b-113">如果以前未开发过 .NET for Apache Spark 应用程序，请从[入门教程](get-started.md)开始，以熟悉基础知识。</span><span class="sxs-lookup"><span data-stu-id="48c9b-113">If you haven't developed a .NET for Apache Spark application before, start with the [Getting Started tutorial](get-started.md) to become familiar with the basics.</span></span> <span data-ttu-id="48c9b-114">完成“入门教程”的所有先决条件，然后再继续使用此教程。</span><span class="sxs-lookup"><span data-stu-id="48c9b-114">Complete all of the prerequisites for the Getting Started tutorial before you continue with this tutorial.</span></span>

* <span data-ttu-id="48c9b-115">此教程使用 Visual Studio 提供的可视界面 ML.NET 模型生成器（预览）。</span><span class="sxs-lookup"><span data-stu-id="48c9b-115">This tutorial uses the ML.NET Model Builder (preview), a visual interface available in Visual Studio.</span></span> <span data-ttu-id="48c9b-116">如果还没有 Visual Studio，可免费[下载 Visual Studio Community 版本](https://visualstudio.microsoft.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="48c9b-116">If you don't already have Visual Studio, you can [download the Community version of Visual Studio](https://visualstudio.microsoft.com/downloads/) for free.</span></span>

* <span data-ttu-id="48c9b-117">[下载并安装](https://marketplace.visualstudio.com/items?itemName=MLNET.07) ML.NET 模型生成器（预览）。</span><span class="sxs-lookup"><span data-stu-id="48c9b-117">[Download and install](https://marketplace.visualstudio.com/items?itemName=MLNET.07) ML.NET Model Builder (preview).</span></span>

* <span data-ttu-id="48c9b-118">下载 Yelp 评论数据集：[yelptest.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptest.csv) 和 [yelptrain.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv)。</span><span class="sxs-lookup"><span data-stu-id="48c9b-118">Download the [yelptest.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptest.csv) and [yelptrain.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv) Yelp review datasets.</span></span>

## <a name="review-the-data"></a><span data-ttu-id="48c9b-119">查看数据</span><span class="sxs-lookup"><span data-stu-id="48c9b-119">Review the data</span></span>

<span data-ttu-id="48c9b-120">Yelp 评论数据集包含关于各种服务的 Yelp 在线评论。</span><span class="sxs-lookup"><span data-stu-id="48c9b-120">The Yelp reviews dataset contains online Yelp reviews about various services.</span></span> <span data-ttu-id="48c9b-121">打开 [yelptrain.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv)，并注意观察其数据结构。</span><span class="sxs-lookup"><span data-stu-id="48c9b-121">Open [yelptrain.csv](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment/Resources/yelptrain.csv) and notice the structure of the data.</span></span> <span data-ttu-id="48c9b-122">第一列包含评论文本，第二列包含情绪评分。</span><span class="sxs-lookup"><span data-stu-id="48c9b-122">The first column contains review text, and the second column contains sentiment scores.</span></span> <span data-ttu-id="48c9b-123">若情绪评分为 1，则为积极评论，若情绪评分为 0，则为消极评论。</span><span class="sxs-lookup"><span data-stu-id="48c9b-123">If the sentiment score is 1, the review is positive, and if the sentiment score is 0, the review is negative.</span></span>

<span data-ttu-id="48c9b-124">下表包含示例数据：</span><span class="sxs-lookup"><span data-stu-id="48c9b-124">The following table contains sample data:</span></span>

|<span data-ttu-id="48c9b-125">ReviewText</span><span class="sxs-lookup"><span data-stu-id="48c9b-125">ReviewText</span></span>|<span data-ttu-id="48c9b-126">情绪</span><span class="sxs-lookup"><span data-stu-id="48c9b-126">Sentiment</span></span>|
|-|-|
|<span data-ttu-id="48c9b-127">哇...喜欢这个地方。</span><span class="sxs-lookup"><span data-stu-id="48c9b-127">Wow... Loved this place.</span></span>|    <span data-ttu-id="48c9b-128">1</span><span class="sxs-lookup"><span data-stu-id="48c9b-128">1</span></span>|
|<span data-ttu-id="48c9b-129">酥皮不行。</span><span class="sxs-lookup"><span data-stu-id="48c9b-129">Crust is not good.</span></span>|    <span data-ttu-id="48c9b-130">0</span><span class="sxs-lookup"><span data-stu-id="48c9b-130">0</span></span>|

## <a name="build-your-machine-learning-model"></a><span data-ttu-id="48c9b-131">构建机器学习模型</span><span class="sxs-lookup"><span data-stu-id="48c9b-131">Build your machine learning model</span></span>

1. <span data-ttu-id="48c9b-132">打开 Visual Studio 并创建新的 C# 控制台应用程序 (.NET Core)。</span><span class="sxs-lookup"><span data-stu-id="48c9b-132">Open Visual Studio and create a new C# Console App (.NET Core).</span></span> <span data-ttu-id="48c9b-133">将项目命名为 MLSparkModel  。</span><span class="sxs-lookup"><span data-stu-id="48c9b-133">Name the project *MLSparkModel*.</span></span>

1. <span data-ttu-id="48c9b-134">在“解决方案资源管理器”  中，右键单击“MLSparkModel”项目。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-134">In **Solution Explorer**, right-click the *MLSparkModel* project.</span></span> <span data-ttu-id="48c9b-135">然后选择“添加 > 机器学习”  。</span><span class="sxs-lookup"><span data-stu-id="48c9b-135">Then select **Add > Machine Learning**.</span></span>

1. <span data-ttu-id="48c9b-136">从 ML.NET 模型生成器中选择“情绪分析”方案磁贴。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-136">From the ML.NET Model Builder, select the **Sentiment Analysis** scenario tile.</span></span>

1. <span data-ttu-id="48c9b-137">在“添加数据”页中上传 yelptrain.csv 数据集。  </span><span class="sxs-lookup"><span data-stu-id="48c9b-137">On the **Add data** page, upload the *yelptrain.csv* data set.</span></span>

1. <span data-ttu-id="48c9b-138">从“要预测的列”下拉列表中选择“情绪”。  </span><span class="sxs-lookup"><span data-stu-id="48c9b-138">Choose *Sentiment* from the **Columns to Predict** dropdown.</span></span>

1. <span data-ttu-id="48c9b-139">在“训练”页中，将训练时间设置为 60 秒，然后选择“开始训练”。   </span><span class="sxs-lookup"><span data-stu-id="48c9b-139">On the **Train** page, set the time to train to *60 seconds* and select **Start training**.</span></span> <span data-ttu-id="48c9b-140">注意“进度”中的训练状态。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-140">Notice the status of your training under **Progress**.</span></span>

1. <span data-ttu-id="48c9b-141">模型生成器完成训练后，评估训练结果。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-141">Once Model Builder is finished training, **Evaluate** the training results.</span></span> <span data-ttu-id="48c9b-142">可以将短语输入到“试用模型”下方的文本框，然后选择“预测”，查看输出。  </span><span class="sxs-lookup"><span data-stu-id="48c9b-142">You can type phrases into the text box below **Try your model** and select **Predict** to see the output.</span></span>

1. <span data-ttu-id="48c9b-143">选择“代码”，然后选择“添加项目”，将此 ML 模型添加到解决方案。  </span><span class="sxs-lookup"><span data-stu-id="48c9b-143">Select **Code** and then select **Add Projects** to add the ML model to the solution.</span></span>

1. <span data-ttu-id="48c9b-144">请注意将两个项目添加到了解决方案：**MLSparkModelML.ConsoleApp** 和 **MLSparkModelML.Model**。</span><span class="sxs-lookup"><span data-stu-id="48c9b-144">Notice that two projects are added to your solutions: **MLSparkModelML.ConsoleApp** and **MLSparkModelML.Model**.</span></span>

1. <span data-ttu-id="48c9b-145">双击 MLSpark C# 项目，请注意此时已添加了下面的项目引用。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-145">Double-click on your *MLSpark* C# project and notice that the following project reference has been added.</span></span>

   ```xml
   <ItemGroup>
       <ProjectReference Include="..\MLSparkModelML.Model\MLSparkModelML.Model.csproj" />
   </ItemGroup>
   ```

## <a name="create-a-console-app"></a><span data-ttu-id="48c9b-146">创建控制台应用</span><span class="sxs-lookup"><span data-stu-id="48c9b-146">Create a console app</span></span>

<span data-ttu-id="48c9b-147">模型生成器会生成控制台应用。</span><span class="sxs-lookup"><span data-stu-id="48c9b-147">Model Builder creates a console app for you.</span></span>

1. <span data-ttu-id="48c9b-148">在解决方案资源管理器中，右键单击“MLSparkModelML.Console”，然后选择“管理 NuGet 包”   。</span><span class="sxs-lookup"><span data-stu-id="48c9b-148">Right-click on **MLSparkModelML.Console** in Solution Explorer, and select **Manage NuGet Packages**.</span></span>

1. <span data-ttu-id="48c9b-149">搜索 Microsoft.Spark，并安装此包。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-149">Search for **Microsoft.Spark** and install the package.</span></span> <span data-ttu-id="48c9b-150">模型生成器会自动安装 Microsoft.ML。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-150">**Microsoft.ML** is automatically installed for you by Model Builder.</span></span>

### <a name="create-a-sparksession"></a><span data-ttu-id="48c9b-151">创建 SparkSession</span><span class="sxs-lookup"><span data-stu-id="48c9b-151">Create a SparkSession</span></span>

1. <span data-ttu-id="48c9b-152">打开 MLSparkModelML.ConsoleApp 的 Program.cs 文件。  </span><span class="sxs-lookup"><span data-stu-id="48c9b-152">Open the *Program.cs* file for **MLSparkModelML.ConsoleApp**.</span></span> <span data-ttu-id="48c9b-153">此文件已由模型生成器自动生成。</span><span class="sxs-lookup"><span data-stu-id="48c9b-153">This file was autogenerated by Model Builder.</span></span> <span data-ttu-id="48c9b-154">删除 `using` 语句、Main() 方法的内容以及 `CreateSingleDataSample` 区域。</span><span class="sxs-lookup"><span data-stu-id="48c9b-154">Delete the `using` statements, the contents of the Main() method, and the `CreateSingleDataSample` region.</span></span>

1. <span data-ttu-id="48c9b-155">将以下附加的 `using` 语句添加到“Program.cs”  顶部：</span><span class="sxs-lookup"><span data-stu-id="48c9b-155">Add the following additional `using` statements to the top of the *Program.cs*:</span></span>

   ```csharp
   using System;
   using System.Collections.Generic;
   using Microsoft.ML;
   using Microsoft.ML.Data;
   using Microsoft.Spark.Sql;
   using MLSparkModelML.Model;
   ```

1. <span data-ttu-id="48c9b-156">将 `DATA_FILEPATH` 更改为 yelptest.csv 的路径。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-156">Change the `DATA_FILEPATH` to the path of your *yelptest.csv*.</span></span>

1. <span data-ttu-id="48c9b-157">将以下代码添加到 `Main` 方法，以创建新的 `SparkSession`。</span><span class="sxs-lookup"><span data-stu-id="48c9b-157">Add the following code to your `Main` method to create a new `SparkSession`.</span></span> <span data-ttu-id="48c9b-158">Spark 会话是使用数据集和数据帧 API 对 Spark 进行编程的入口点。</span><span class="sxs-lookup"><span data-stu-id="48c9b-158">The Spark Session is the entry point to programming Spark with the Dataset and DataFrame API.</span></span>

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName(".NET for Apache Spark Sentiment Analysis")
        .GetOrCreate();
   ```

   <span data-ttu-id="48c9b-159">通过调用上面创建的 spark 对象，可在整个程序中访问 Spark 和数据帧功能  。</span><span class="sxs-lookup"><span data-stu-id="48c9b-159">Calling the *spark* object created above allows you to access Spark and DataFrame functionality throughout your program.</span></span>

### <a name="create-a-dataframe-and-print-to-console"></a><span data-ttu-id="48c9b-160">创建 DataFrame 并打印到控制台</span><span class="sxs-lookup"><span data-stu-id="48c9b-160">Create a DataFrame and print to console</span></span>

<span data-ttu-id="48c9b-161">将 yelptest.csv 文件作为 `DataFrame`，读入其中的 Yelp 评论数据。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-161">Read in the Yelp review data from the *yelptest.csv* file as a `DataFrame`.</span></span> <span data-ttu-id="48c9b-162">包括 `header` 和 `inferSchema` 选项。</span><span class="sxs-lookup"><span data-stu-id="48c9b-162">Include `header` and `inferSchema` options.</span></span> <span data-ttu-id="48c9b-163">`header` 选项将 yelptest.csv 的第一行作为列名称读取，而非作为数据。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-163">The `header` option reads the first line of *yelptest.csv* as column names instead of data.</span></span> <span data-ttu-id="48c9b-164">`inferSchema` 选项基于数据推断列类型。</span><span class="sxs-lookup"><span data-stu-id="48c9b-164">The `inferSchema` option infers column types based on the data.</span></span>

```csharp
DataFrame df = spark
    .ReadStream()
    .Option("header", true)
    .Option("inferSchema", true)
    .Csv(DATA_FILEPATH);

df.Show();
```

### <a name="register-a-user-defined-function"></a><span data-ttu-id="48c9b-165">注册用户定义的函数</span><span class="sxs-lookup"><span data-stu-id="48c9b-165">Register a user-defined function</span></span>

<span data-ttu-id="48c9b-166">可以使用 Spark 应用程序中的 UDF（用户定义的函数）来对数据执行计算和分析  。</span><span class="sxs-lookup"><span data-stu-id="48c9b-166">You can use UDFs, *user-defined functions*, in Spark applications to do calculations and analysis on your data.</span></span> <span data-ttu-id="48c9b-167">本教程搭配使用 ML.NET 和 UDF 来评估每个 Yelp 评论。</span><span class="sxs-lookup"><span data-stu-id="48c9b-167">In this tutorial, you use ML.NET with a UDF to evaluate each Yelp review.</span></span>

<span data-ttu-id="48c9b-168">将以下代码添加到 `Main` 方法，以注册名为 `MLudf` 的 UDF。</span><span class="sxs-lookup"><span data-stu-id="48c9b-168">Add the following code to your `Main` method to register a UDF called `MLudf`.</span></span>

```csharp
spark.Udf()
    .Register<string, bool>("MLudf", predict);
```

<span data-ttu-id="48c9b-169">此 UDF 将 Yelp 评论字符串用作输入，并分别对积极情绪或消极情绪输出 true 或 false。</span><span class="sxs-lookup"><span data-stu-id="48c9b-169">This UDF takes a Yelp review string as input, and outputs true or false for positive or negative sentiments, respectively.</span></span> <span data-ttu-id="48c9b-170">它使用 predict() 方法，你将在后面的步骤中定义此方法。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-170">It uses the *predict()* method that you define in a later step.</span></span>

### <a name="use-spark-sql-to-call-the-udf"></a><span data-ttu-id="48c9b-171">使用 Spark SQL 调用 UDF</span><span class="sxs-lookup"><span data-stu-id="48c9b-171">Use Spark SQL to call the UDF</span></span>

<span data-ttu-id="48c9b-172">现已读入数据并已并入 ML，接下来使用 Spark SQL 调用对 DataFrame 各行运行情绪分析的 UDF。</span><span class="sxs-lookup"><span data-stu-id="48c9b-172">Now that you've read in your data and incorporated ML, use Spark SQL to call the UDF that will run sentiment analysis on each row of your DataFrame.</span></span> <span data-ttu-id="48c9b-173">将以下代码添加到 `Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="48c9b-173">Add the following code to your `Main` method:</span></span>

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

### <a name="create-predict-method"></a><span data-ttu-id="48c9b-174">创建 predict() 方法</span><span class="sxs-lookup"><span data-stu-id="48c9b-174">Create predict() method</span></span>

<span data-ttu-id="48c9b-175">在 `Main()` 方法的前面添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="48c9b-175">Add the following code before your `Main()` method.</span></span> <span data-ttu-id="48c9b-176">此代码类似于模型生成器生成的 ConsumeModel.cs 的内容。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-176">This code is similar to what is produced by Model Builder in *ConsumeModel.cs*.</span></span> <span data-ttu-id="48c9b-177">将此方法移到控制台后，每次运行应用时，都将加载此模型负载。</span><span class="sxs-lookup"><span data-stu-id="48c9b-177">Moving this method to your console loads the model loading each time you run your app.</span></span>

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

## <a name="add-the-model-to-your-console-app"></a><span data-ttu-id="48c9b-178">将模型添加到控制台应用</span><span class="sxs-lookup"><span data-stu-id="48c9b-178">Add the model to your console app</span></span>

<span data-ttu-id="48c9b-179">在解决方案资源管理器中，从 MLSparkModelML.Model 项目复制 MLModel.zip，并将其粘贴到 MLSparkModelML.ConsoleApp 项目中。   </span><span class="sxs-lookup"><span data-stu-id="48c9b-179">In Solution Explorer, copy the *MLModel.zip* file from the **MLSparkModelML.Model** project and paste it in the **MLSparkModelML.ConsoleApp** project.</span></span> <span data-ttu-id="48c9b-180">将自动在 MLSparkModelML.ConsoleApp.csproj 中添加引用。 </span><span class="sxs-lookup"><span data-stu-id="48c9b-180">A reference is automatically added in *MLSparkModelML.ConsoleApp.csproj*.</span></span>

## <a name="run-your-code"></a><span data-ttu-id="48c9b-181">运行代码</span><span class="sxs-lookup"><span data-stu-id="48c9b-181">Run your code</span></span>

<span data-ttu-id="48c9b-182">使用 `spark-submit` 运行代码。</span><span class="sxs-lookup"><span data-stu-id="48c9b-182">Use `spark-submit` to run your code.</span></span> <span data-ttu-id="48c9b-183">使用命令提示符导航到控制台应用的根文件夹，运行下面的命令。</span><span class="sxs-lookup"><span data-stu-id="48c9b-183">Navigate to your console app's root folder using the command prompt and run the following commands.</span></span>

<span data-ttu-id="48c9b-184">首先，清理并发布应用。</span><span class="sxs-lookup"><span data-stu-id="48c9b-184">First, clean and publish your app.</span></span>

```dotnetcli
dotnet clean
dotnet publish
```

<span data-ttu-id="48c9b-185">然后导航到控制台应用的发布文件夹，运行下面的 `spark-submit` 命令。</span><span class="sxs-lookup"><span data-stu-id="48c9b-185">Then navigate to the console app's publish folder and run the following `spark-submit` command.</span></span> <span data-ttu-id="48c9b-186">务必使用 Microsoft Spark jar 文件的实际路径更新此命令。</span><span class="sxs-lookup"><span data-stu-id="48c9b-186">Remember to update the command with the actual path of your Microsoft Spark jar file.</span></span>

```dotnetcli
%SPARK_HOME%\bin\spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local microsoft-spark-2-4_2.11-1.0.0.jar dotnet MLSparkModelML.ConsoleApp.dll
```

## <a name="get-the-code"></a><span data-ttu-id="48c9b-187">获取代码</span><span class="sxs-lookup"><span data-stu-id="48c9b-187">Get the code</span></span>

<span data-ttu-id="48c9b-188">本教程与[使用大数据进行情绪分析](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment)示例中的代码相似。</span><span class="sxs-lookup"><span data-stu-id="48c9b-188">This tutorial is similar to the code from the [Sentiment Analysis with Big Data](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.CSharp.Examples/MachineLearning/Sentiment) example.</span></span>

## <a name="next-steps"></a><span data-ttu-id="48c9b-189">后续步骤</span><span class="sxs-lookup"><span data-stu-id="48c9b-189">Next steps</span></span>

<span data-ttu-id="48c9b-190">继续阅读下一篇文章，了解如何使用 .NET for Apache Spark 进行结构化流式处理。</span><span class="sxs-lookup"><span data-stu-id="48c9b-190">Advance to the next article to learn how to do Structured Streaming with .NET for Apache Spark.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="48c9b-191">教程：使用 .NET for Apache Spark 进行结构化流式处理</span><span class="sxs-lookup"><span data-stu-id="48c9b-191">Tutorial: Structured Streaming with .NET for Apache Spark</span></span>](streaming.md)
