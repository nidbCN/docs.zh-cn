---
title: 教程：对支持问题进行分类 - 多类分类
description: 了解如何在多类分类方案中使用 ML.NET 对 GitHub 问题进行分类，将其分配到给定区域。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: dfc007f72fcd67f31139bc7fcbad93dde39d88fb
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875624"
---
# <a name="tutorial-categorize-support-issues-using-multiclass-classification-with-mlnet"></a>教程：将多类分类与 ML.NET 配合使用，对支持问题分类

本示例教程演示如何使用 ML.NET 创建 GitHub 问题分类器来训练模型，使其通过 Visual Studio 中使用 C# 的 .NET Core 控制台应用程序为 GitHub 问题分类和预测区域标签。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 准备数据
> * 转换数据
> * 定型模型
> * 评估模型
> * 使用训练的模型预测
> * 使用加载模型部署和预测

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/GitHubIssueClassification) 存储库中找到本教程的源代码。

## <a name="prerequisites"></a>先决条件

* 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。
* [GitHub 问题制表符分隔文件 (issues_train.tsv)](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv)。
* [GitHub 问题测试制表符分隔文件 (issues_test.tsv)](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv)。

## <a name="create-a-console-application"></a>创建控制台应用程序

### <a name="create-a-project"></a>创建项目

1. 打开 Visual Studio 2017。 从菜单栏中选择“文件” > “新建” > “项目”。 在“新项目”对话框中，依次选择“Visual C#”和“.NET Core”节点。 然后，选择“控制台应用程序(.NET Core)”  项目模板。 在“名称”文本框中，键入“GitHubIssueClassification”，然后选择“确定”按钮。

2. 在项目中创建一个名为“Data”的目录来保存数据集文件：

    在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”。 键入“Data”，然后按 Enter。

3. 在项目中创建一个名为“Models”的目录来保存模型：

    在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”。 键入“Models”，然后按 Enter。

4. 安装“Microsoft.ML NuGet 包”  ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，再选择“安装”按钮 。 选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。

### <a name="prepare-your-data"></a>准备数据

1. 下载 [issues_train.tsv](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) 和 [issues_test.tsv](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) 数据集，并将它们保存到先前创建的“Data”文件夹。 第一个数据集用于定型机器学习模型，第二个数据集可用来评估模型的准确度。

2. 在“解决方案资源管理器”中，右键单击每个 \*.tsv 文件，然后选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

### <a name="create-classes-and-define-paths"></a>创建类和定义路径

将以下附加的 `using` 语句添加到“Program.cs”文件顶部：

[!code-csharp[AddUsings](./snippets/github-issue-classification/csharp/Program.cs#AddUsings)]

创建 3 个全局字段，来保存最近下载的文件的路径以及 `MLContext`、`DataView` 和 `PredictionEngine` 的全局变量：

* `_trainDataPath` 具有用于定型模型的数据集路径。
* `_testDataPath` 具有用于评估模型的数据集路径。
* `_modelPath` 具有在其中保存定型模型的路径。
* `_mlContext` 是用于提供处理上下文的 <xref:Microsoft.ML.MLContext>。
* `_trainingDataView` 是用于处理定型数据集的 <xref:Microsoft.ML.IDataView>。
* `_predEngine` 是用于单个预测的 <xref:Microsoft.ML.PredictionEngine%602>。

将以下代码添加到 `Main` 方法正上方的行中以指定这些路径和其他变量：

[!code-csharp[DeclareGlobalVariables](./snippets/github-issue-classification/csharp/Program.cs#DeclareGlobalVariables)]

为输入数据和预测创建一些类。 向项目添加一个新类：

1. 在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。

1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“GitHubIssueData.cs”  。 然后，选择“添加”  按钮。

    “GitHubIssueData.cs”文件随即在代码编辑器中打开。 将下面的 `using` 语句添加到 GitHubIssueData.cs 的顶部：

[!code-csharp[AddUsings](./snippets/github-issue-classification/csharp/GitHubIssueData.cs#AddUsings)]

删除现有类定义并向“GitHubIssueData.cs”文件添加以下代码，其中有两个类 `GitHubIssue` 和 `IssuePrediction`：

[!code-csharp[DeclareGlobalVariables](./snippets/github-issue-classification/csharp/GitHubIssueData.cs#DeclareTypes)]

`label` 是要预测的列。 标识的 `Features` 是为模型提供的用来预测标签的输入。

使用 [LoadColumnAttribute](xref:Microsoft.ML.Data.LoadColumnAttribute) 在数据集中指定源列的索引。

`GitHubIssue` 是输入数据集类，具有以下 <xref:System.String> 字段：

* 第一列 `ID`（GitHub 问题 ID）
* 第二列 `Area`（定型预测）
* 第三列 `Title`（GitHub 问题标题）是用于预测 `Area` 的第一个 `feature`
* 第四列 `Description` 是用于预测 `Area` 的第二个 `feature`

`IssuePrediction` 是在定型模型后用于预测的类。 它有一个 `string` (`Area`) 和一个 `PredictedLabel` `ColumnName` 属性。  `PredictedLabel` 在预测和评估过程中使用。 对于计算，将使用带定型数据的输入、预测值和模型。

所有 ML.NET 操作都从 [MLContext](xref:Microsoft.ML.MLContext) 类开始。 初始化 `mlContext` 创建了新的 ML.NET 环境，可以在模型创建工作流对象之间共享。 从概念上讲，它与 `Entity Framework` 中的 `DBContext` 类似。

### <a name="initialize-variables-in-main"></a>在 Main 中初始化变量

使用具有随机种子 (`seed: 0`) 的新实例 `MLContext` 初始化 `_mlContext` 全局变量，以获得跨多个定型的可重复/确定性结果。  用下面 `Main` 方法中的代码替换 `Console.WriteLine("Hello World!")` 行：

[!code-csharp[CreateMLContext](./snippets/github-issue-classification/csharp/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a>加载数据

ML.NET 使用 [IDataView 类](xref:Microsoft.ML.IDataView)灵活、有效地描述数字或文本表格数据。 `IDataView` 可以加载文本文件或进行实时加载（例如，SQL 数据库或日志文件）。

要初始化并加载 `_trainingDataView` 全局变量以将其用于管道，请在 `mlContext` 初始化后添加以下代码：

[!code-csharp[LoadTrainData](./snippets/github-issue-classification/csharp/Program.cs#LoadTrainData)]

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 用于定义数据架构并读取文件。 它使用数据路径变量并返回 `IDataView`。

将以下代码作为下一行代码添加到 `Main` 方法中：

[!code-csharp[CallProcessData](./snippets/github-issue-classification/csharp/Program.cs#CallProcessData)]

`ProcessData` 方法执行以下任务：

* 提取并转换数据。
* 返回处理管道。

使用下面的代码紧随 `Main` 方法后创建 `ProcessData` 方法：

```csharp
public static IEstimator<ITransformer> ProcessData()
{

}
```

## <a name="extract-features-and-transform-the-data"></a>提取功能和转换数据

由于要预测 `GitHubIssue` 的区域 GitHub 标签，因此请使用 [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 方法将 `Area` 列转换为数字键类型 `Label` 列（分类算法所接受的格式）并将其添加为新的数据集列：

[!code-csharp[MapValueToKey](./snippets/github-issue-classification/csharp/Program.cs#MapValueToKey)]

接下来，调用 `mlContext.Transforms.Text.FeaturizeText`，它会将文本（`Title` 和 `Description`）列转换为每个名为 `TitleFeaturized` 和 `DescriptionFeaturized` 的值的数字向量。 使用以下代码将两列的特征化附加到管道：

[!code-csharp[FeaturizeText](./snippets/github-issue-classification/csharp/Program.cs#FeaturizeText)]

数据准备最后一步使用 [Concatenate()](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A) 方法将所有特征列合并到“特征”列。 默认情况下，学习算法仅处理“特征”列的特征。 使用以下代码将此转换附加到管道：

[!code-csharp[Concatenate](./snippets/github-issue-classification/csharp/Program.cs#Concatenate)]

 接下来，附加一个 <xref:Microsoft.ML.Data.EstimatorChain%601.AppendCacheCheckpoint%2A> 来缓存数据视图，以便在使用缓存多次循环访问数据时获得更好的性能，如下面的代码所示：

[!code-csharp[AppendCache](./snippets/github-issue-classification/csharp/Program.cs#AppendCache)]

> [!WARNING]
> 对小/中型数据集使用 AppendCacheCheckpoint 可以降低训练时间。 在处理大型数据集时不使用它（删除 .AppendCacheCheckpoint()）。

在 `ProcessData` 方法的末尾返回管道。

[!code-csharp[ReturnPipeline](./snippets/github-issue-classification/csharp/Program.cs#ReturnPipeline)]

此步骤处理预处理/特征化。 使用 ML.NET 中可用的其他组件可以在使用模型时生成更佳结果。

## <a name="build-and-train-the-model"></a>生成和定型模型

将以下调用添加到 `BuildAndTrainModel` 方法作为 `Main` 方法的下一行代码：

[!code-csharp[CallBuildAndTrainModel](./snippets/github-issue-classification/csharp/Program.cs#CallBuildAndTrainModel)]

`BuildAndTrainModel` 方法执行以下任务：

* 创建定型算法类。
* 定型模型。
* 根据定型数据预测区域。
* 返回模型。

使用下面的代码紧随 `Main` 方法后创建 `BuildAndTrainModel` 方法：

```csharp
public static IEstimator<ITransformer> BuildAndTrainModel(IDataView trainingDataView, IEstimator<ITransformer> pipeline)
{

}
```

### <a name="about-the-classification-task"></a>有关分类任务

分类是一项机器学习任务，它使用数据来 **确定** 某个项或数据行的类别、类型或类，并且通常是以下类型之一：

* 二元：A 或 B。
* 多类：可以通过使用单个模型来预测多个类别。

对于此类问题，请使用多类分类学习算法，因为你的问题类别预测可能是多个类别（多类）而不是仅两个（二元）中的一个。

将机器学习算法追加到数据转换定义中，方法是在 `BuildAndTrainModel()` 中添加以下代码作为第一行代码：

[!code-csharp[AddTrainer](./snippets/github-issue-classification/csharp/Program.cs#AddTrainer)]

[SdcaMaximumEntropy](xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer) 即多类分类训练算法。 它追加到 `pipeline` 并接受特征化的 `Title` 和 `Description` (`Features`) 以及 `Label` 输入参数，以便从历史数据中学习。

### <a name="train-the-model"></a>定型模型

在 `BuildAndTrainModel()` 方法中添加以下代码作为下一代码行，使模型适应 `splitTrainSet` 数据，并返回经过训练的模型：

[!code-csharp[TrainModel](./snippets/github-issue-classification/csharp/Program.cs#TrainModel)]

`Fit()` 方法通过转换数据集并应用训练来训练模型。

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可用于传入单个数据实例，然后对其执行预测。 将此 API 添加为 `BuildAndTrainModel()` 方法中的下一行：

[!code-csharp[CreatePredictionEngine1](./snippets/github-issue-classification/csharp/Program.cs#CreatePredictionEngine1)]

### <a name="predict-with-the-trained-model"></a>使用训练的模型预测

通过创建一个 `GitHubIssue` 实例，在 `Predict` 方法中添加一个 GitHub 问题来测试定型模型的预测：

[!code-csharp[CreateTestIssue1](./snippets/github-issue-classification/csharp/Program.cs#CreateTestIssue1)]

使用 [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单行数据进行预测：

[!code-csharp[Predict](./snippets/github-issue-classification/csharp/Program.cs#Predict)]

### <a name="using-the-model-prediction-results"></a>使用模型：预测结果

显示 `GitHubIssue` 和相应的 `Area` 标签预测以便共享结果，并采取相应措施。  使用以下 <xref:System.Console.WriteLine?displayProperty=nameWithType> 代码创建结果显示：

[!code-csharp[OutputPrediction](./snippets/github-issue-classification/csharp/Program.cs#OutputPrediction)]

### <a name="return-the-model-trained-to-use-for-evaluation"></a>返回定型模型以用于评估

在 `BuildAndTrainModel` 方法末尾返回模型。

[!code-csharp[ReturnModel](./snippets/github-issue-classification/csharp/Program.cs#ReturnModel)]

## <a name="evaluate-the-model"></a>评估模型

你已经创建和定型模型，现在需要使用不同的数据集对其进行评估以保证质量和进行验证。 在 `Evaluate` 方法中，将传入在 `BuildAndTrainModel` 中创建的模型以进行评估。 紧随 `BuildAndTrainModel` 后创建 `Evaluate` 方法，如以下代码所示：

```csharp
public static void Evaluate(DataViewSchema trainingDataViewSchema)
{

}
```

`Evaluate` 方法执行以下任务：

* 加载测试数据集。
* 创建多类评估程序。
* 评估模型并创建指标。
* 显示指标。

使用下面的代码，在 `BuildAndTrainModel` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：

[!code-csharp[CallEvaluate](./snippets/github-issue-classification/csharp/Program.cs#CallEvaluate)]

与之前对训练数据集所执行的操作那样，通过将以下代码添加到 `Evaluate` 方法来加载测试数据集：

[!code-csharp[LoadTestDataset](./snippets/github-issue-classification/csharp/Program.cs#LoadTestDataset)]

[Evaluate()](xref:Microsoft.ML.MulticlassClassificationCatalog.Evaluate%2A) 方法使用指定数据集计算模型的质量指标。 它返回 <xref:Microsoft.ML.Data.MulticlassClassificationMetrics> 对象，其中包含由多类分类计算器计算出的总体指标。
要显示指标来确定模型质量，需要先获取这些指标。
请注意使用机器学习 `_trainedModel` 全局变量 ([ITransformer](xref:Microsoft.ML.ITransformer)) 的 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法来输入特征和返回预测。 将以下代码作为下一行添加到 `Evaluate` 方法中：

[!code-csharp[Evaluate](./snippets/github-issue-classification/csharp/Program.cs#Evaluate)]

针对多类分类评估以下指标：

* 微观准确性 - 每个“样本-类”对准确性指标的贡献度相同。  通常会希望微观准确性尽可能接近 1。

* 宏观准确性 - 每个类对准确性指标的贡献度相同。 占比较小的类与占比较大的类拥有同等的权重。 通常会希望宏观准确性尽可能接近 1。

* 对数损失 - 请参阅[对数损失](../resources/glossary.md#log-loss)。 通常会希望对数损失尽可能接近 0。

* 对数损失减小 - 取值范围为 [-inf，1.00]，其中 1.00 表示非常精准的预测结果，0 表示准确性一般的预测。 通常会希望对数损失减少尽可能接近 1。

### <a name="displaying-the-metrics-for-model-validation"></a>显示用于模型验证的指标

使用下面的代码显示指标、共享结果，然后处理它们：

[!code-csharp[DisplayMetrics](./snippets/github-issue-classification/csharp/Program.cs#DisplayMetrics)]

### <a name="save-the-model-to-a-file"></a>将模型保存到文件

对模型满意后，将其保存到文件中以便稍后或在其他应用程序中进行预测。 将以下代码添加到 `Evaluate` 方法中。

[!code-csharp[SnippetCallSaveModel](./snippets/github-issue-classification/csharp/Program.cs#SnippetCallSaveModel)]

在 `Evaluate` 方法下创建 `SaveModelAsFile` 方法。

```csharp
private static void SaveModelAsFile(MLContext mlContext,DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

将以下代码添加到 `SaveModelAsFile` 方法。 此代码使用 [`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save%2A) 方法对训练后的模型进行序列化并将其存储为 zip 文件。

[!code-csharp[SnippetSaveModel](./snippets/github-issue-classification/csharp/Program.cs#SnippetSaveModel)]

## <a name="deploy-and-predict-with-a-model"></a>使用模型进行部署和预测

使用下面的代码，在 `Evaluate` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：

[!code-csharp[CallPredictIssue](./snippets/github-issue-classification/csharp/Program.cs#CallPredictIssue)]

使用以下代码恰好在 `Evaluate` 方法的后面（恰在 `SaveModelAsFile` 方法之前）创建 `PredictIssue` 方法：

```csharp
private static void PredictIssue()
{

}
```

`PredictIssue` 方法执行以下任务：

* 加载已保存的模型
* 创建测试数据的单个问题。
* 根据测试数据预测区域。
* 结合测试数据和预测进行报告。
* 显示预测结果。

通过向 `PredictIssue` 方法中添加以下代码，将保存的模型加载到应用程序中：

[!code-csharp[SnippetLoadModel](./snippets/github-issue-classification/csharp/Program.cs#SnippetLoadModel)]

通过创建一个 `GitHubIssue` 实例，在 `Predict` 方法中添加一个 GitHub 问题来测试定型模型的预测：

[!code-csharp[AddTestIssue](./snippets/github-issue-classification/csharp/Program.cs#AddTestIssue)]

与之前一样，使用以下代码创建 `PredictionEngine` 实例：

[!code-csharp[CreatePredictionEngine](./snippets/github-issue-classification/csharp/Program.cs#CreatePredictionEngine)]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 可以在单线程环境或原型环境中使用。 为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。 请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。

> [!NOTE]
> `PredictionEnginePool` 服务扩展目前处于预览状态。

通过将以下代码添加到预测的 `PredictIssue` 方法，使用 `PredictionEngine` 来预测区域 GitHub 标签：

[!code-csharp[PredictIssue](./snippets/github-issue-classification/csharp/Program.cs#PredictIssue)]

### <a name="using-the-loaded-model-for-prediction"></a>使用加载后的模型进行预测

显示 `Area` 以便对问题进行分类并对其进行相应操作。 使用以下 <xref:System.Console.WriteLine?displayProperty=nameWithType> 代码创建结果显示：

[!code-csharp[DisplayResults](./snippets/github-issue-classification/csharp/Program.cs#DisplayResults)]

## <a name="results"></a>结果

结果应如下所示。 管道处理期间，会显示消息。 你可能会看到警告或处理消息。 为简便起见，已从以下结果中删除这些消息。

```console
=============== Single Prediction just-trained-model - Result: area-System.Net ===============
*************************************************************************************************************
*       Metrics for Multi-class Classification model - Test Data
*------------------------------------------------------------------------------------------------------------
*       MicroAccuracy:    0.738
*       MacroAccuracy:    0.668
*       LogLoss:          .919
*       LogLossReduction: .643
*************************************************************************************************************
=============== Single Prediction - Result: area-System.Data ===============
```

祝贺你！ 现在，已成功生成用于为 GitHub 问题分类和预测区域标签的机器学习模型。 可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/GitHubIssueClassification) 存储库中找到本教程的源代码。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 准备数据
> * 转换数据
> * 定型模型
> * 评估模型
> * 使用训练的模型预测
> * 使用加载模型部署和预测

进入下一教程了解详细信息
> [!div class="nextstepaction"]
> [出租车费预测器](predict-prices.md)
