---
title: 教程：使用 TensorFlow 模型分析评审情绪
description: 本教程演示如何使用预先训练的 TensorFlow 模型对网站评论中的情绪进行分类。 二元情绪分类器是使用 Visual Studio 开发的 C# 控制台应用程序。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 0c2cfd46375406726913a83d67c45ff922a1085b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877002"
---
# <a name="tutorial-analyze-sentiment-of-movie-reviews-using-a-pre-trained-tensorflow-model-in-mlnet"></a>教程：在 ML.NET 中使用预先训练的 TensorFlow 模型分析电影评论的情绪

本教程演示如何使用预先训练的 TensorFlow 模型对网站评论中的情绪进行分类。 二元情绪分类器是使用 Visual Studio 开发的 C# 控制台应用程序。

本教程中使用的 TensorFlow 模型是使用 IMDB 数据库中的电影评论训练的。 完成应用程序的开发后，你将能够提供电影评论文本，应用程序将会告诉你该评论是正面情绪还是负面情绪。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 加载预先训练的 TensorFlow 模型
> * 将网站评论文本转换为适用于模型的特征
> * 使用模型进行预测

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TextClassificationTF) 存储库中找到本教程的源代码。

## <a name="prerequisites"></a>先决条件

* 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2017 版本 15.6 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。

## <a name="setup"></a>安装

### <a name="create-the-application"></a>创建应用程序

1. 创建名为“TextClassificationTF”的“.NET Core 控制台应用程序”。

2. 在项目中创建名为“Data”的目录，用于保存数据集文件。

3. 安装“Microsoft.ML NuGet 包”  ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡。搜索“Microsoft.ML”，选择所需的包，然后选择“安装”按钮 。 同意所选包的许可条款，继续执行安装。 对 Microsoft.ML.TensorFlow、Microsoft.ML.SampleUtils 和 SciSharp.TensorFlow.Redist，重复上述步骤。  

### <a name="add-the-tensorflow-model-to-the-project"></a>将 TensorFlow 模型添加到项目

> [!NOTE]
> 本教程的模型来自 [dotnet/machinelearning-testdata](https://github.com/dotnet/machinelearning-testdata/tree/main/Microsoft.ML.TensorFlow.TestModels/sentiment_model) GitHub 存储库。 该模型采用 TensorFlow SavedModel 格式。

1. 下载 [sentiment_model zip 文件](https://github.com/dotnet/samples/blob/main/machine-learning/models/textclassificationtf/sentiment_model.zip?raw=true)并将其解压缩。

    该 zip 文件包含：

    * `saved_model.pb`：TensorFlow 模型本身。 该模型采用表示 IMDB 评论字符串中文本的特征的固定长度（大小为 600）整数数组，并输出两个概率（总和为 1）：输入评论具有正面情绪的概率，以及输入评论具有负面情绪的概率。
    * `imdb_word_index.csv`：从单个单词到整数值的映射。 映射用于为 TensorFlow 模型生成输入特征。

2. 将最内层 `sentiment_model` 目录的内容复制到 TextClassificationTF 项目的 `sentiment_model` 目录中。 此目录包含本教程所需的模型和其他支持文件，如下图所示：

   ![sentiment_model 目录内容](./media/text-classification-tf/sentiment-model-files.png)

3. 在“解决方案资源管理器”中，右键单击 `sentiment_model` 目录和子目录中的每个文件，然后选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”  。

### <a name="add-using-statements-and-global-variables"></a>添加 using 语句和全局变量

1. 将以下附加的 `using` 语句添加到“Program.cs”文件顶部：

   [!code-csharp[AddUsings](./snippets/text-classification-tf/csharp/Program.cs#AddUsings "Add necessary usings")]

1. 在 `Main` 方法正上方创建两个全局变量，用来保留已保存的模型文件路径和特征向量长度。

   [!code-csharp[DeclareGlobalVariables](./snippets/text-classification-tf/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

    * `_modelPath` 是已训练模型的文件路径。
    * `FeatureLength` 是模型需要的整数特征数组的长度。

### <a name="model-the-data"></a>为数据建模

电影评论是自由格式的文本。 应用程序会将文本转换为模型在多个离散阶段中所需的输入格式。

首先是将文本拆分为单独的单词，然后使用提供的映射文件将每个单词映射到整数编码。 这种转换的结果是一个可变长度的整数数组，其长度对应于句子中的单词数。

|Property| 值|类型|
|-------------|-----------------------|------|
|ReviewText|这部电影非常不错|string|
|VariableLengthFeatures|14,22,9,66,78,... |int[]|

然后将可变长度特征数组的大小调整为固定长度 600。 这是 TensorFlow 模型所需的长度。

|Property| 值|类型|
|-------------|-----------------------|------|
|ReviewText|这部电影非常不错|string|
|VariableLengthFeatures|14,22,9,66,78,... |int[]|
|特征|14,22,9,66,78,... |int[600]|

1. 在 `Main` 方法之后为输入数据创建一个类：

    [!code-csharp[MovieReviewClass](./snippets/text-classification-tf/csharp/Program.cs#MovieReviewClass "Declare movie review type")]

    输入数据类 `MovieReview` 具有用于用户评论 (`ReviewText`) 的 `string`。

1. 在 `Main` 方法之后为可变长度特征创建一个类：

    [!code-csharp[VariableLengthFeatures](./snippets/text-classification-tf/csharp/Program.cs#VariableLengthFeatures "Declare variable length features type")]

    `VariableLengthFeatures` 属性的 [VectorType](xref:Microsoft.ML.Data.VectorTypeAttribute.%23ctor%2A) 特性用于将其指定为向量。  所有向量元素都必须是同一类型。 在具有大量列的数据集中，将多个列作为单个向量加载会减少应用数据转换时的数据传递次数。

    此类在 `ResizeFeatures` 操作中使用。 它的属性名称（本例中只有一个）用于指示 DataView 中的哪些列可用作自定义映射操作的输入。

1. 在 `Main` 方法之后为固定长度特征创建一个类：

    [!code-csharp[FixedLengthFeatures](./snippets/text-classification-tf/csharp/Program.cs#FixedLengthFeatures)]

    此类在 `ResizeFeatures` 操作中使用。 它的属性名称（本例中只有一个）用于指示 DataView 中的哪些列可用作自定义映射操作的输出。

    请注意，属性 `Features` 的名称由 TensorFlow 模型确定。 此属性名称无法更改。

1. 在 `Main` 方法之后为预测创建一个类：

    [!code-csharp[Prediction](./snippets/text-classification-tf/csharp/Program.cs#Prediction "Declare prediction class")]

    `MovieReviewSentimentPrediction` 是在训练模型后使用的预测类。 `MovieReviewSentimentPrediction` 有一个 `float` 数组 (`Prediction`) 和一个 `VectorType` 属性。

### <a name="create-the-mlcontext-lookup-dictionary-and-action-to-resize-features"></a>创建 MLContext、查找字典以及用于调整特征大小的操作

[MLContext 类](xref:Microsoft.ML.MLContext)是所有 ML.NET 操作的起点。 初始化 `mlContext` 会创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

1. 使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 mlContext 变量：

   [!code-csharp[CreateMLContext](./snippets/text-classification-tf/csharp/Program.cs#CreateMLContext "Create the ML Context")]

1. 通过使用 [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) 方法创建字典来将单词编码为整数，以便从文件中加载映射数据，如下表所示：

    |字     |Index    |
    |---------|---------|
    |kids     |  362    |
    |want     |  181    |
    |wrong    |  355    |
    |effects  |  302    |
    |feeling  |  547    |

    添加下面的代码，创建查找映射：

    [!code-csharp[CreateLookupMap](./snippets/text-classification-tf/csharp/Program.cs#CreateLookupMap)]

1. 添加一个 `Action`，将可变长度单词整数数组的大小调整为固定大小的整数数组，再加上下面的几行代码：

   [!code-csharp[ResizeFeatures](./snippets/text-classification-tf/csharp/Program.cs#ResizeFeatures)]

## <a name="load-the-pre-trained-tensorflow-model"></a>加载预先训练的 TensorFlow 模型

1. 添加代码以加载 TensorFlow 模型：

    [!code-csharp[LoadTensorFlowModel](./snippets/text-classification-tf/csharp/Program.cs#LoadTensorFlowModel)]

    加载模型后，可以提取其输入和输出架构。 所显示的架构仅供参考和学习。 最终应用程序不需要此代码即可运行：

    [!code-csharp[GetModelSchema](./snippets/text-classification-tf/csharp/Program.cs#GetModelSchema)]

    输入架构是整数编码单词的固定长度数组。 输出架构是概率的浮点数组，指示评论的情绪是负面情绪还是正面情绪。 这些值的总和为 1，因为正面情绪的概率与负面情绪的概率相互补足。

## <a name="create-the-mlnet-pipeline"></a>创建 ML.NET 管道

1. 创建管道并使用 [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) 转换将输入文本拆分为单词，从而将文本拆分为单词以作为下一行代码：

   [!code-csharp[TokenizeIntoWords](./snippets/text-classification-tf/csharp/Program.cs#TokenizeIntoWords)]

   [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) 转换使用空格将文本/字符串分析为单词。 它将创建一个新列，并基于用户定义的分隔符将每个输入字符串拆分为子字符串的向量。

1. 使用你在上面声明的查找表将单词映射到其整数编码：

    [!code-csharp[MapValue](./snippets/text-classification-tf/csharp/Program.cs#MapValue)]

1. 将可变长度整数编码调整为模型所需的固定长度：

    [!code-csharp[CustomMapping](./snippets/text-classification-tf/csharp/Program.cs#CustomMapping)]

1. 使用加载的 TensorFlow 模型对输入进行分类：

    [!code-csharp[ScoreTensorFlowModel](./snippets/text-classification-tf/csharp/Program.cs#ScoreTensorFlowModel)]

    TensorFlow 模型输出称为 `Prediction/Softmax`。 请注意，名称 `Prediction/Softmax` 由 TensorFlow 模型确定。 此名称无法更改。

1. 为输出预测创建一个新列：

    [!code-csharp[SnippetCopyColumns](./snippets/text-classification-tf/csharp/Program.cs#SnippetCopyColumns)]

    需要将 `Prediction/Softmax` 列复制到其名称可用作 C# 类中的属性的列：`Prediction`。 不允许在 C# 属性名称中使用 `/` 字符。

## <a name="create-the-mlnet-model-from-the-pipeline"></a>从管道创建 ML.NET 模型

1. 添加代码以从管道创建模型：

    [!code-csharp[SnippetCreateModel](./snippets/text-classification-tf/csharp/Program.cs#SnippetCreateModel)]

    通过调用 `Fit` 方法，从管道中的估算器链创建 ML.NET 模型。 在这种情况下，我们不会调整任何数据以创建模型，因为 TensorFlow 模型此前已经过训练。 我们提供一个空的数据视图对象，以满足 `Fit` 方法的要求。

## <a name="use-the-model-to-make-a-prediction"></a>使用模型进行预测

1. 在 `Main` 方法下添加 `PredictSentiment` 方法：

    ```csharp
    public static void PredictSentiment(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. 添加以下代码以创建 `PredictionEngine` 作为 `PredictSentiment()` 方法中的第一行：

    [!code-csharp[CreatePredictionEngine](./snippets/text-classification-tf/csharp/Program.cs#CreatePredictionEngine)]

    [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 可以在单线程环境或原型环境中使用。 为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。 请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。

    > [!NOTE]
    > `PredictionEnginePool` 服务扩展目前处于预览状态。

1. 通过创建一个 `MovieReview` 实例，在 `Predict()` 方法中添加一个注释来测试定型模型的预测：

    [!code-csharp[CreateTestData](./snippets/text-classification-tf/csharp/Program.cs#CreateTestData)]

1. 通过在 `PredictSentiment()` 方法中添加接下来的几行代码，将测试评论数据传递到 `Prediction Engine`：

    [!code-csharp[Predict](./snippets/text-classification-tf/csharp/Program.cs#Predict)]

1. [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单行数据进行预测：

    |Property| 值|类型|
    |-------------|-----------------------|------|
    |预测|[0.5459937, 0.454006255]|float[]|

1. 使用以下代码显示情绪预测：

    [!code-csharp[DisplayPredictions](./snippets/text-classification-tf/csharp/Program.cs#DisplayPredictions)]

1. 在 `Main` 方法末尾添加对 `PredictSentiment` 的调用：

    [!code-csharp[CallPredictSentiment](./snippets/text-classification-tf/csharp/Program.cs#CallPredictSentiment)]

## <a name="results"></a>结果

生成并运行应用程序。

结果应如下所示。 处理期间将显示消息。 你可能会看到警告或处理消息。 为简便起见，已从以下结果中删除这些消息。

```console
Number of classes: 2
Is sentiment/review positive ? Yes
```

祝贺你！ 现已通过在 ML.NET 中重用预先训练的 `TensorFlow` 模型成功生成用于分类和预测消息情绪的机器学习模型。

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TextClassificationTF) 存储库中找到本教程的源代码。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 加载预先训练的 TensorFlow 模型
> * 将网站评论文本转换为适用于模型的特征
> * 使用模型进行预测
