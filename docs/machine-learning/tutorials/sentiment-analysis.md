---
title: 教程：分析网站评论 - 二元分类
description: 本教程演示如何创建 .NET Core 控制台应用程序，该应用程序对网站评论情绪进行分类并采取适当的措施。 二元情绪分类器在 Visual Studio 中使用 C#。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 3dbcb3cbd4eea2d01638bedc7123f570ff9d64d1
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874585"
---
# <a name="tutorial-analyze-sentiment-of-website-comments-with-binary-classification-in-mlnet"></a>教程：在 ML.NET 中使用二元分类分析网站评论的情绪

本教程演示如何创建 .NET Core 控制台应用程序，该应用程序对网站评论情绪进行分类并采取适当的措施。 二元情绪分类器在 Visual Studio 2017 中使用 C#。

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 创建控制台应用程序
> - 准备数据
> - 加载数据
> - 生成和定型模型
> - 评估模型
> - 使用模型进行预测
> - 查看结果

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) 存储库中找到本教程的源代码。

## <a name="prerequisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2017 版本 15.6 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)

- [UCI Sentiment Labeled Sentences 数据集](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip)（zip 文件）

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 创建名为“SentimentAnalysis”的 **.NET Core 控制台应用程序**。

2. 在项目中创建名为“Data”的目录，用于保存数据集文件。

3. 安装“Microsoft.ML NuGet 包”  ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡。搜索“Microsoft.ML”，选择所需的包，然后选择“安装”按钮 。 同意所选包的许可条款，继续执行安装。

## <a name="prepare-your-data"></a>准备数据

> [!NOTE]
> 本教程的数据集摘自 KDD 2015 中由 Kotzias 等 提出的“From Group to Individual Labels using Deep Features”， 并托管在 UCI 机器学习存储库中（Dua, D. 和 Karra Taniskidou, E.(2017)）。 UCI 机器学习存储库 [http://archive.ics.uci.edu/ml ]。 加利福尼亚州，加利福尼亚大学：欧文分校，信息与计算机科学学院。

1. 下载 [UCI Sentiment Labeled Sentences 数据集 zip 文件](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip)并解压缩。

2. 将 `yelp_labelled.txt` 文件复制到已创建的“Data”目录中。

3. 在“解决方案资源管理器”中，右键单击 `yelp_labeled.txt` 文件并选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

### <a name="create-classes-and-define-paths"></a>创建类和定义路径

1. 将以下附加的 `using` 语句添加到“Program.cs”文件顶部：

    [!code-csharp[AddUsings](./snippets/sentiment-analysis/csharp/Program.cs#AddUsings "Add necessary usings")]

1. 将以下代码添加到 `Main` 方法正上方的行中，以创建字段来保存最近下载的数据集文件路径：

    [!code-csharp[Declare global variables](./snippets/sentiment-analysis/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

1. 接下来，为输入数据和预测结果创建类。 向项目添加一个新类：

    - 在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。

    - 在“添加新项”  对话框中，选择“类”  并将“名称”  字段更改为“SentimentData.cs”  。 然后，选择“添加”  按钮。

1. “SentimentData.cs”  文件随即在代码编辑器中打开。 将下面的 `using` 语句添加到 SentimentData.cs 的顶部：

    [!code-csharp[AddUsings](./snippets/sentiment-analysis/csharp/SentimentData.cs#AddUsings "Add necessary usings")]

1. 删除现有类定义并向“SentimentData.cs”文件添加以下代码，其中有两个类 `SentimentData` 和 `SentimentPrediction`：

    [!code-csharp[DeclareTypes](./snippets/sentiment-analysis/csharp/SentimentData.cs#DeclareTypes "Declare data record types")]

### <a name="how-the-data-was-prepared"></a>如何准备数据

输入数据集类 `SentimentData` 拥有一个用于用户评论 (`SentimentText`) 的 `string`，以及一个用于情绪的 `bool` (`Sentiment`)，值为 1（正面）或 0（负面）。 这两个字段都附加了 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 特性，其描述了每个字段的数据文件顺序。  此外，`Sentiment` 属性具有 [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute.%23ctor%2A) 特性，以将其指定为 `Label` 字段。 下面的示例文件没有标题行，如下所示：

|情绪文本                         |情绪（标签） |
|--------------------------------------|----------|
|女服务员的服务速度有点慢。|    0     |
|酥皮不行。                    |    0     |
|哇...喜欢这个地方。              |    1     |
|服务很及时。              |    1     |

`SentimentPrediction` 是在模型训练后使用的预测类。 它继承自 `SentimentData`，因此输入 `SentimentText` 可与输出预测结果一并显示。 `Prediction` 布尔值是随附新的输入 `SentimentText` 提供时模型预测出的值。

输出类 `SentimentPrediction` 包含另外两个由模型计算得出的属性：`Score`（模型计算得出的原始分数）和 `Probability`（校准到具有积极情绪的文本几率的分数）。

在本教程中，最重要的属性是 `Prediction`。

## <a name="load-the-data"></a>加载数据

ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。 `IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。 可从文本文件或实时（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。

[MLContext 类](xref:Microsoft.ML.MLContext)是所有 ML.NET 操作的起点。 初始化 `mlContext` 会创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

准备应用，然后加载数据：

1. 使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 mlContext 变量：

    [!code-csharp[CreateMLContext](./snippets/sentiment-analysis/csharp/Program.cs#CreateMLContext "Create the ML Context")]

2. 将以下代码作为下一行代码添加到 `Main()` 方法中：

    [!code-csharp[CallLoadData](./snippets/sentiment-analysis/csharp/Program.cs#CallLoadData)]

3. 使用下面的代码紧随 `Main()` 方法后创建 `LoadData()` 方法：

    ```csharp
    public static TrainTestData LoadData(MLContext mlContext)
    {

    }
    ```

    `LoadData()` 方法执行以下任务：

    - 加载数据。
    - 将加载的数据集拆分为训练数据集和测试数据集。
    - 返回拆分的训练数据集和测试数据集。

4. 将以下代码添加为 `LoadData()` 方法的首行：

    [!code-csharp[LoadData](./snippets/sentiment-analysis/csharp/Program.cs#LoadData "loading dataset")]

    [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 方法用于定义数据架构并读取文件。 它使用数据路径变量并返回 `IDataView`。

### <a name="split-the-dataset-for-model-training-and-testing"></a>拆分数据集以进行模型训练和测试

准备模型时，使用部分数据集来训练它，并使用部分数据集来测试模型的准确性。

1. 要将加载的数据拆分为所需的数据集，请添加以下代码作为 `LoadData()` 方法中的下一行：

    [!code-csharp[SplitData](./snippets/sentiment-analysis/csharp/Program.cs#SplitData "Split the Data")]

    上述代码使用 [TrainTestSplit()](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit%2A) 方法将加载的数据集拆分为训练数据集和测试数据集，并在 <xref:Microsoft.ML.DataOperationsCatalog.TrainTestData> 类中返回它们。 使用 `testFraction` 参数指定数据的测试集百分比。 默认值为 10%，在本例中使用 20%，以评估更多数据。

2. 在 `LoadData()` 方法末尾返回 `splitDataView`：

    [!code-csharp[ReturnSplitData](./snippets/sentiment-analysis/csharp/Program.cs#ReturnSplitData)]

## <a name="build-and-train-the-model"></a>生成和定型模型

1. 将以下调用添加到 `BuildAndTrainModel` 方法作为 `Main()` 方法的下一行代码：

    [!code-csharp[CallBuildAndTrainModel](./snippets/sentiment-analysis/csharp/Program.cs#CallBuildAndTrainModel)]

    `BuildAndTrainModel()` 方法执行以下任务：

    - 提取并转换数据。
    - 定型模型。
    - 根据测试数据预测情绪。
    - 返回模型。

2. 使用下面的代码紧随 `Main()` 方法后创建 `BuildAndTrainModel()` 方法：

    ```csharp
    public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView splitTrainSet)
    {

    }
    ```

### <a name="extract-and-transform-the-data"></a>提取和转换数据

1. 将 `FeaturizeText` 作为下一行代码调用：

    [!code-csharp[FeaturizeText](./snippets/sentiment-analysis/csharp/Program.cs#FeaturizeText "Featurize the text")]

    上述代码中的 `FeaturizeText()` 方法将文本列 (`SentimentText`) 转换为机器学习算法使用的数字键类型的 `Features` 列，并将其作为新的数据集列添加：

    |情绪文本                         |情绪 |特征              |
    |--------------------------------------|----------|----------------------|
    |女服务员的服务速度有点慢。|    0     |[0.76, 0.65, 0.44, …] |
    |酥皮不行。                    |    0     |[0.98, 0.43, 0.54, …] |
    |哇...喜欢这个地方。              |    1     |[0.35, 0.73, 0.46, …] |
    |服务很及时。              |    1     |[0.39, 0, 0.75, …]    |

### <a name="add-a-learning-algorithm"></a>添加学习算法

此应用使用对数据项或数据行进行分类的分类算法。 应用将网站评论分类为正面评论或负面评论，因此，请使用二元分类任务。

将机器学习任务追加到数据转换定义中，方法是在 `BuildAndTrainModel()` 中添加以下代码作为下一行代码：

[!code-csharp[SdcaLogisticRegressionBinaryTrainer](./snippets/sentiment-analysis/csharp/Program.cs#AddTrainer "Add a SdcaLogisticRegressionBinaryTrainer")]

[SdcaLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer) 是你的分类训练算法。 此算法会追加到 `estimator` 并接受特征化的 `SentimentText` (`Features`) 和 `Label` 输入参数，以便从历史数据中学习。

### <a name="train-the-model"></a>定型模型

在 `BuildAndTrainModel()` 方法中添加以下代码作为下一代码行，使模型适应 `splitTrainSet` 数据，并返回经过训练的模型：

[!code-csharp[TrainModel](./snippets/sentiment-analysis/csharp/Program.cs#TrainModel "Train the model")]

[Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) 方法通过转换数据集并应用训练来训练模型。

### <a name="return-the-model-trained-to-use-for-evaluation"></a>返回定型模型以用于评估

 在 `BuildAndTrainModel()` 方法末尾返回模型：

[!code-csharp[ReturnModel](./snippets/sentiment-analysis/csharp/Program.cs#ReturnModel "Return the model")]

## <a name="evaluate-the-model"></a>评估模型

训练模型后，使用测试数据验证模型的性能。

1. 使用以下代码紧跟 `BuildAndTrainModel()` 之后创建 `Evaluate()` 方法：

    ```csharp
    public static void Evaluate(MLContext mlContext, ITransformer model, IDataView splitTestSet)
    {

    }
    ```

    `Evaluate()` 方法执行以下任务：

    - 加载测试数据集。
    - 创建 BinaryClassification 计算器。
    - 评估模型并创建指标。
    - 显示指标。

2. 使用下面的代码，在 `BuildAndTrainModel()` 方法调用的正下方，从 `Main()` 方法中添加对新方法的调用：

    [!code-csharp[CallEvaluate](./snippets/sentiment-analysis/csharp/Program.cs#CallEvaluate "Call the Evaluate method")]

3. 将以下代码添加到 `Evaluate()` 以转换 `splitTestSet` 数据：

    [!code-csharp[PredictWithTransformer](./snippets/sentiment-analysis/csharp/Program.cs#TransformData "Predict using the Transformer")]

    之前的代码使用 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对测试数据集提供的多个输入行进行预测。

4. 通过在 `Evaluate()` 方法中添加以下代码作为下一代码行来评估模型：

    [!code-csharp[ComputeMetrics](./snippets/sentiment-analysis/csharp/Program.cs#Evaluate "Compute Metrics")]

获得预测集 (`predictions`) 后，[Evaluate()](xref:Microsoft.ML.BinaryClassificationCatalog.Evaluate%2A) 方法会对模型进行评估，其会将预测值与测试数据集中的实际 `Labels` 进行比较，并返回有关模型执行情况的 [CalibratedBinaryClassificationMetrics](xref:Microsoft.ML.Data.CalibratedBinaryClassificationMetrics) 对象。

### <a name="displaying-the-metrics-for-model-validation"></a>显示用于模型验证的指标

使用以下代码显示指标：

[!code-csharp[DisplayMetrics](./snippets/sentiment-analysis/csharp/Program.cs#DisplayMetrics "Display selected metrics")]

- `Accuracy` 指标可获取模型的准确性，即测试集中正确预测所占的比例。

- `AreaUnderRocCurve` 指标指示模型对正面类和负面类进行正确分类的置信度。 应该使 `AreaUnderRocCurve` 尽可能接近 1。

- `F1Score` 指标可获取模型的 F1 分数，该分数是[查准率](../resources/glossary.md#precision)和[查全率](../resources/glossary.md#recall)之间的平衡关系的度量值。  应该使 `F1Score` 尽可能接近 1。

### <a name="predict-the-test-data-outcome"></a>预测测试数据结果

1. 使用下面的代码紧随 `Evaluate()` 方法后创建 `UseModelWithSingleItem()` 方法：

    ```csharp
    private static void UseModelWithSingleItem(MLContext mlContext, ITransformer model)
    {

    }
    ```

    `UseModelWithSingleItem()` 方法执行以下任务：

    - 创建测试数据的单个注释。
    - 根据测试数据预测情绪。
    - 结合测试数据和预测进行报告。
    - 显示预测结果。

2. 使用下面的代码，在 `Evaluate()` 方法调用的正下方，从 `Main()` 方法中添加对新方法的调用：

    [!code-csharp[CallUseModelWithSingleItem](./snippets/sentiment-analysis/csharp/Program.cs#CallUseModelWithSingleItem "Call the UseModelWithSingleItem method")]

3. 添加以下代码，以便作为 `UseModelWithSingleItem()` 方法中的第一行进行创建：

    [!code-csharp[CreatePredictionEngine](./snippets/sentiment-analysis/csharp/Program.cs#CreatePredictionEngine1 "Create the PredictionEngine")]

    [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 可以在单线程环境或原型环境中使用。 为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。 请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。

    > [!NOTE]
    > `PredictionEnginePool` 服务扩展目前处于预览状态。

4. 通过创建一个 `SentimentData` 实例，在 `UseModelWithSingleItem()` 方法中添加一个注释来测试定型模型的预测：

    [!code-csharp[PredictionData](./snippets/sentiment-analysis/csharp/Program.cs#CreateTestIssue1 "Create test data for single prediction")]

5. 通过在 `UseModelWithSingleItem()` 方法中将以下代码作为下一行代码添加，将测试评论数据传递到 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)：

    [!code-csharp[Predict](./snippets/sentiment-analysis/csharp/Program.cs#Predict "Create a prediction of sentiment")]

    [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单行数据进行预测。

6. 使用以下代码显示 `SentimentText` 和相应的情绪预测：

    [!code-csharp[OutputPrediction](./snippets/sentiment-analysis/csharp/Program.cs#OutputPrediction "Display prediction output")]

## <a name="use-the-model-for-prediction"></a>使用模型进行预测

### <a name="deploy-and-predict-batch-items"></a>部署和预测批项目

1. 使用下面的代码紧随 `UseModelWithSingleItem()` 方法后创建 `UseModelWithBatchItems()` 方法：

    ```csharp
    public static void UseModelWithBatchItems(MLContext mlContext, ITransformer model)
    {

    }
    ```

    `UseModelWithBatchItems()` 方法执行以下任务：

    - 创建批处理测试数据。
    - 根据测试数据预测情绪。
    - 结合测试数据和预测进行报告。
    - 显示预测结果。

2. 使用下面的代码，在 `UseModelWithSingleItem()` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：

    [!code-csharp[CallPredictModelBatchItems](./snippets/sentiment-analysis/csharp/Program.cs#CallUseModelWithBatchItems "Call the CallUseModelWithBatchItems method")]

3. 添加一些评论，以测试 `UseModelWithBatchItems()` 方法中的定型模型预测：

    [!code-csharp[PredictionData](./snippets/sentiment-analysis/csharp/Program.cs#CreateTestIssues "Create test data for predictions")]

### <a name="predict-comment-sentiment"></a>预测评论情绪

使用模型通过 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法预测评论数据情绪：

[!code-csharp[Predict](./snippets/sentiment-analysis/csharp/Program.cs#Prediction "Create predictions of sentiments")]

### <a name="combine-and-display-the-predictions"></a>合并并显示预测结果

使用以下代码为预测创建标头：

[!code-csharp[OutputHeaders](./snippets/sentiment-analysis/csharp/Program.cs#AddInfoMessage "Display prediction outputs")]

由于 `SentimentPrediction` 继承自 `SentimentData`，`Transform()` 方法使用预测字段填充 `SentimentText`。 随着 ML.NET 进程继续执行，每个组件会添加列，这让显示结果变得轻松：

[!code-csharp[DisplayPredictions](./snippets/sentiment-analysis/csharp/Program.cs#DisplayResults "Display the predictions")]

## <a name="results"></a>结果

结果应如下所示。 处理期间将显示消息。 你可能会看到警告或处理消息。 为清楚起见，已经从下面的结果中删除这些内容。

```console
Model quality metrics evaluation
--------------------------------
Accuracy: 83.96%
Auc: 90.51%
F1Score: 84.04%

=============== End of model evaluation ===============

=============== Prediction Test of model with a single sample and test dataset ===============

Sentiment: This was a very bad steak | Prediction: Negative | Probability: 0.1027377
=============== End of Predictions ===============

=============== Prediction Test of loaded model with a multiple samples ===============

Sentiment: This was a horrible meal | Prediction: Negative | Probability: 0.1369192
Sentiment: I love this spaghetti. | Prediction: Positive | Probability: 0.9960636
=============== End of predictions ===============

=============== End of process ===============
Press any key to continue . . .

```

祝贺你！ 现在，你已成功生成用于分类和预测消息情绪的机器学习模型。

生成成功的模型是一个迭代过程。 由于本教程使用小型数据集来提供快速模型训练，因此该模型的初始质量较低。 如果对模型质量不满意，可以通过尝试提供更大的训练数据集，或通过为每种算法选择具有不同[超参数](../resources/glossary.md#hyperparameter)的不同训练算法来改进它。

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) 存储库中找到本教程的源代码。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 创建控制台应用程序
> - 准备数据
> - 加载数据
> - 生成和定型模型
> - 评估模型
> - 使用模型进行预测
> - 查看结果

进入下一教程了解详细信息
> [!div class="nextstepaction"]
> [问题分类](github-issue-classification.md)
