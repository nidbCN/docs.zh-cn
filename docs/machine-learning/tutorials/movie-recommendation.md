---
title: 教程：生成影片推荐系统 - 矩阵因子分解
description: 本教程演示如何在 .NET Core 控制台应用程序中使用 ML.NET 生成电影推荐系统。 这些步骤使用 C# 和 Visual Studio 2019。
author: briacht
ms.date: 06/30/2020
ms.custom: mvc, title-hack-0516
ms.topic: tutorial
ms.openlocfilehash: 2df774110d3355bf75a14e211555984a12cf7fa4
ms.sourcegitcommit: b27645cb378d4e8137a267e5467ff31409acf6c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231389"
---
# <a name="tutorial-build-a-movie-recommender-using-matrix-factorization-with-mlnet"></a>教程：使用矩阵因子分解和 ML.NET 生成影片推荐系统

本教程演示如何在 .NET Core 控制台应用程序中使用 ML.NET 生成电影推荐系统。 这些步骤使用 C# 和 Visual Studio 2019。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 选择机器学习算法
> * 准备并加载数据
> * 生成并训练模型
> * 评估模型
> * 部署和使用模型

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/MovieRecommendation) 存储库中找到本教程的源代码。

## <a name="machine-learning-workflow"></a>机器学习工作流

你将使用以下步骤完成任务，以及任何其他 ML.NET 任务：

1. [加载数据](#load-your-data)
2. [生成并训练模型](#build-and-train-your-model)
3. [评估模型](#evaluate-your-model)
4. [使用模型](#use-your-model)

## <a name="prerequisites"></a>先决条件

* 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。

## <a name="select-the-appropriate-machine-learning-task"></a>选择适当的机器学习任务

有几种方法可以解决推荐问题，如推荐影片列表或推荐相关产品列表，但此示例中将预测用户给予特定影片的评分 (1-5) 并在评分高于定义的阈值时推荐该影片（评分越高，用户喜欢特定电影的可能性就越大）。

## <a name="create-a-console-application"></a>创建控制台应用程序

### <a name="create-a-project"></a>创建项目

1. 打开 Visual Studio 2017。 从菜单栏中选择“文件” > “新建” > “项目”。 在“新项目”对话框中，依次选择“Visual C#”和“.NET Core”节点。 然后，选择“控制台应用程序(.NET Core)”  项目模板。 在“名称”文本框中，键入“MovieRecommender”，然后选择“确定”按钮   。

2. 在项目中创建一个名为“数据”的目录来保存数据集文件  ：

    在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”    。 键入“Data”，然后按 Enter。

3. 安装“Microsoft.ML”和“Microsoft.ML.Recommender”NuGet 包   ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”   。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，在列表中选择包，再选择“安装”按钮  。 选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。 对“Microsoft.ML.Recommender”重复这些步骤  。

4. 在 Program.cs 文件的顶部添加以下 `using` 语句  ：

    [!code-csharp[UsingStatements](./snippets/movie-recommendation/csharp/Program.cs#UsingStatements "Add necessary usings")]

### <a name="download-your-data"></a>下载数据

1. 下载两个数据集并将其保存到先前创建的“数据”文件夹中  ：

   * 右键单击 [recommended-ratings-train.csv](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/MatrixFactorization_MovieRecommendation/Data/recommendation-ratings-train.csv)，然后选择“将链接(或目标)另存为...” 
   * 右键单击 [recommendation-ratings-test.csv](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/MatrixFactorization_MovieRecommendation/Data/recommendation-ratings-test.csv)，然后选择“将链接(或目标)另存为...” 

     确保将 .csv 文件保存到“数据”文件夹，或者将其保存到其他位置后，将 .csv 文件移动到“数据”文件夹\*  \*  。

2. 在“解决方案资源管理器”中，右键单击每个 \*.csv 文件，然后选择“属性”  。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

   ![如果在 VS 中较新，则用户选择“复制”的 GIF。](./media/movie-recommendation/copy-to-output-if-newer.gif)

## <a name="load-your-data"></a>加载数据

ML.NET 过程的第一步是准备并加载用于训练和测试数据的模型。

建议分级数据分为 `Train` 和 `Test` 数据集。 `Train` 数据用于适应模型。 `Test` 数据用于使用经过训练的模型进行预测并评估模型性能。 通常使用 `Train` 和 `Test` 数据进行 80/20 拆分。

以下是 .csv 文件中数据的预览：\*

![CVS 数据集预览的屏幕截图。](./media/movie-recommendation/csv-file-dataset-preview.png)

在 .csv 文件中，有四列：\*

* `userId`
* `movieId`
* `rating`
* `timestamp`

在机器学习中，用于进行预测的列称为 [Features](../resources/glossary.md#feature)，带有返回预测的列称为 [Label](../resources/glossary.md#label)。

想要预测影片评分，因此评分列为 `Label`。 其他三列，`userId`、`movieId` 和 `timestamp` 都用 `Features` 来预测 `Label`。

| 特征      | Label         |
| ------------- |:-------------:|
| `userId`        |    `rating`     |
| `movieId`      |               |
| `timestamp`     |               |

由你来决定使用哪个 `Features` 来预测 `Label`。 你还可以使用[排列特征重要性](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md)等方法来帮助选择最佳 `Features`。

在此示例中，应将 `timestamp` 列排除为 `Feature`，因为时间戳并不会真正影响用户对给定影片的评分方式，因此无法进行更准确的预测：

| 特征      | Label         |
| ------------- |:-------------:|
| `userId`        |    `rating`     |
| `movieId`      |               |

接下来，必须为输入类定义数据结构。

向项目添加一个新类：

1. 在“解决方案资源管理器”中，右键单击该项目，然后选择“添加”>“新项”   。

2. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“MovieRatingData.cs”     。 然后，选择“添加”  按钮。

“MovieRatingData.cs”文件随即在代码编辑器中打开  。 将下面的 `using` 语句添加到 MovieRatingData.cs 的顶部  ：

```csharp
using Microsoft.ML.Data;
```

通过删除现有的类定义并在 MovieRatingData.cs 中添加以下代码，创建一个名为 `MovieRating` 的类  ：

[!code-csharp[MovieRatingClass](./snippets/movie-recommendation/csharp/MovieRatingData.cs#MovieRatingClass "Add the Movie Rating class")]

`MovieRating` 指定输入数据类。 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性指定应加载数据集中的哪些列（按列索引）。 `userId` 和 `movieId` 列是你的 `Features`（你将向模型提供预测 `Label` 的输入），而评分列是你将预测的 `Label` 模型的输出）。

创建另一个类 `MovieRatingPrediction`，通过在 MovieRatingData.cs 中的 `MovieRating` 类之后添加以下代码来表示预测结果： 

[!code-csharp[PredictionClass](./snippets/movie-recommendation/csharp/MovieRatingData.cs#PredictionClass "Add the Movie Prediction Class")]

在 Program.cs 中，将 `Console.WriteLine("Hello World!")` 替换为 `Main()` 中的以下代码  ：

[!code-csharp[MLContext](./snippets/movie-recommendation/csharp/Program.cs#MLContext "Add MLContext")]

执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

在 `Main()` 之后，创建一个名为 `LoadData()` 的方法：

```csharp
public static (IDataView training, IDataView test) LoadData(MLContext mlContext)
{

}
```

> [!NOTE]
> 除非在以下步骤中添加返回语句，否则使用此方法将出错。

初始化数据路径变量、从 \*.csv 文件加载数据以及将 `Train` 和 `Test` 数据作为 `IDataView` 对象返回，方法是在 `LoadData()` 中添加以下代码作为下一代码行：

[!code-csharp[LoadData](./snippets/movie-recommendation/csharp/Program.cs#LoadData "Load data from data paths")]

ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。 `IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。 可从文本文件或实时（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。

[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 用于定义数据架构并读取文件。 它使用数据路径变量并返回 `IDataView`。 在这种情况下，需提供 `Test` 和 `Train` 文件的路径，并指示文本文件头（以便正确使用列名称）和逗号字符数据分隔符（默认分隔符是制表符）。

在 `Main()` 方法中添加以下代码，以调用 `LoadData()` 方法并返回 `Train` 和 `Test` 数据：

[!code-csharp[LoadDataMain](./snippets/movie-recommendation/csharp/Program.cs#LoadDataMain "Add LoadData method to Main")]

## <a name="build-and-train-your-model"></a>生成并训练模型

使用下面的代码紧随 `LoadData()` 方法后创建 `BuildAndTrainModel()` 方法：

```csharp
public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView trainingDataView)
{

}
```

> [!NOTE]
> 除非在以下步骤中添加返回语句，否则使用此方法将出错。

通过将以下代码添加到 `BuildAndTrainModel()` 来定义数据转换：

[!code-csharp[DataTransformations](./snippets/movie-recommendation/csharp/Program.cs#DataTransformations "Define data transformations")]

由于 `userId` 和 `movieId` 代表用户和影片标题，而不是实际值，因此使用 [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 方法将每个 `userId` 和每个 `movieId` 转换为数字键类型 `Feature` 列（推荐算法接受的格式）并将它们添加为新的数据集列：

| userId | movieId | Label | userIdEncoded | movieIdEncoded |
| ------------- |:-------------:| -----:|-----:|-----:|
| 1 | 1 | 4 | userKey1 | movieKey1 |
| 1 | 3 | 4 | userKey1 | movieKey2 |
| 1 | 6 | 4 | userKey1 | movieKey3 |

选择机器学习算法并将其添加到数据转换定义中，方法是在 `BuildAndTrainModel()` 中添加以下代码作为下一代码行：

[!code-csharp[AddAlgorithm](./snippets/movie-recommendation/csharp/Program.cs#AddAlgorithm "Add the training algorithm with options")]

[MatrixFactorizationTrainer](xref:Microsoft.ML.RecommendationCatalog.RecommendationTrainers.MatrixFactorization%28Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options%29) 就是推荐训练算法。  当你掌握用户过去如何评价产品的数据时，通常建议使用[矩阵分解](https://en.wikipedia.org/wiki/Matrix_factorization_(recommender_systems))方法，本教程中的数据集就是这种情况。 当你有不同的数据时，还可使用其他推荐算法（请参阅下面的[其他推荐算法](#other-recommendation-algorithms)部分以了解更多信息）。

在本例中，`Matrix Factorization` 算法使用了一种称为“协作筛选”的方法，该方法假设如果用户 1 在某个问题上与用户 2 有相同的观点，那么用户 1 更有可能与用户 2 在另一个问题上有相同的看法。

例如，如果用户 1 和用户 2 对影片的评分相似，那么用户 2 更有可能欣赏用户 1 已观看并给出很高评分的影片：

| | `Incredibles 2 (2018)` | `The Avengers (2012)` | `Guardians of the Galaxy (2014)` |
| -------------:|-------------:| -----:|-----:|
| 用户 1 | 观看和点赞过的影片 | 观看和点赞过的影片 | 观看和点赞过的影片 |
| 用户 2 | 观看和点赞过的影片 | 观看和点赞过的影片 | 没有看过 - 推荐影片 |

`Matrix Factorization` 训练程序有多个[选项](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options)，可在下面的[算法超参数](#algorithm-hyperparameters)部分中详细了解。

在 `BuildAndTrainModel()` 方法中添加以下代码作为下一代码行，使模型适应 `Train` 数据，并返回经过训练的模型：

[!code-csharp[FitModel](./snippets/movie-recommendation/csharp/Program.cs#FitModel "Call the Fit method and return back the trained model")]

[Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) 方法使用提供的训练数据集训练模型。 从技术上讲，该方法通过转换数据并应用训练来执行 `Estimator` 定义，然后返回经过训练的模型，即 `Transformer`。

若要详细了解 ML.NET 中的模型训练工作流，请参阅[什么是 ML.NET 以及它如何工作？](../how-does-mldotnet-work.md#code-workflow)。

将以下内容添加为 `Main()` 方法中的下一代码行，以调用 `BuildAndTrainModel()` 方法并返回经过训练的模型：

[!code-csharp[BuildTrainModelMain](./snippets/movie-recommendation/csharp/Program.cs#BuildTrainModelMain "Add BuildAndTrainModel method in Main")]

## <a name="evaluate-your-model"></a>评估模型

训练模型后，使用测试数据评估模型的执行情况。

使用下面的代码紧随 `BuildAndTrainModel()` 方法后创建 `EvaluateModel()` 方法：

```csharp
public static void EvaluateModel(MLContext mlContext, IDataView testDataView, ITransformer model)
{

}
```

将以下代码添加到 `EvaluateModel()` 以转换 `Test` 数据：

[!code-csharp[Transform](./snippets/movie-recommendation/csharp/Program.cs#Transform "Transform the test data")]

[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对测试数据集的多个提供的输入行进行预测。

通过在 `EvaluateModel()` 方法中添加以下代码作为下一代码行来评估模型：

[!code-csharp[Evaluate](./snippets/movie-recommendation/csharp/Program.cs#Evaluate "Evaluate the model using predictions from the test data")]

获得预测集后，[Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) 方法会对模型进行评估，该模型会将预测值与测试数据集中的实际 `Labels` 进行比较，并返回有关模型执行情况的指标。

在 `EvaluateModel()` 方法中添加以下代码作为下一代码行，将评估指标输出到控制台：

[!code-csharp[PrintMetrics](./snippets/movie-recommendation/csharp/Program.cs#PrintMetrics "Print the evaluation metrics")]

在 `Main()` 方法中添加以下代码作为下一代码行，调用 `EvaluateModel()` 方法：

[!code-csharp[EvaluateModelMain](./snippets/movie-recommendation/csharp/Program.cs#EvaluateModelMain "Add EvaluateModel method in Main")]

到目前为止的输出应类似于以下文本：

```console
=============== Training the model ===============
iter      tr_rmse          obj
   0       1.5403   3.1262e+05
   1       0.9221   1.6030e+05
   2       0.8687   1.5046e+05
   3       0.8416   1.4584e+05
   4       0.8142   1.4209e+05
   5       0.7849   1.3907e+05
   6       0.7544   1.3594e+05
   7       0.7266   1.3361e+05
   8       0.6987   1.3110e+05
   9       0.6751   1.2948e+05
  10       0.6530   1.2766e+05
  11       0.6350   1.2644e+05
  12       0.6197   1.2541e+05
  13       0.6067   1.2470e+05
  14       0.5953   1.2382e+05
  15       0.5871   1.2342e+05
  16       0.5781   1.2279e+05
  17       0.5713   1.2240e+05
  18       0.5660   1.2230e+05
  19       0.5592   1.2179e+05
=============== Evaluating the model ===============
Rms: 0.994051469730769
RSquared: 0.412556298844873
```

在此输出中，有 20 次迭代。 在每次迭代中，误差测量值均会减小并逐渐趋于最小值 0。

`root of mean squared error`（RMS 或 RMSE）用于度量模型预测的值与测试数据集观察到的值之间的差异。 从技术上讲，它是误差的平方的平均值的平方根。 指标越低，模型就越好。

`R Squared` 指明数据与模型的适应程度。 范围从 0 到 1。 值 0 表示数据是随机的，否则就无法适应模型。 值 1 表示模型与数据完全匹配。 通常会希望 `R Squared` 分数尽可能接近 1。

生成成功的模型是一个迭代过程。 由于本教程使用小型数据集来提供快速模型训练，因此该模型的初始质量较低。 如果对模型质量不满意，可以通过尝试提供更大的训练数据集，或通过为每种算法选择具有不同超参数的不同训练算法来改进它。 有关详细信息，请查看下面的[改进模型](#improve-your-model)部分。

## <a name="use-your-model"></a>使用模型

现在，你可以使用经过训练的模型对新数据进行预测。

使用下面的代码紧随 `EvaluateModel()` 方法后创建 `UseModelForSinglePrediction()` 方法：

```csharp
public static void UseModelForSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

使用 `PredictionEngine` 通过将以下代码添加到 `UseModelForSinglePrediction()` 来预测评分：

[!code-csharp[PredictionEngine](./snippets/movie-recommendation/csharp/Program.cs#PredictionEngine "Create Prediction Engine")]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 可以在单线程环境或原型环境中使用。 为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。 请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。

> [!NOTE]
> `PredictionEnginePool` 服务扩展目前处于预览状态。

创建一个名为 `testInput` 的 `MovieRating` 实例，并通过在 `UseModelForSinglePrediction()` 方法中添加以下代码作为下一代码行，将其传递给预测引擎：

[!code-csharp[MakeSinglePrediction](./snippets/movie-recommendation/csharp/Program.cs#MakeSinglePrediction "Make a single prediction with the Prediction Engine")]

[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单列数据进行预测。

然后，你可以使用 `Score` 或预测评分来确定是否要将 movieId 10 的影片推荐给用户 6。 `Score` 越高，用户喜欢特定电影的可能性就越大。 在这种情况下，假设你推荐预测评分大于 3.5 的电影。

若要输出结果，请在 `UseModelForSinglePrediction()` 方法中添加以下代码作为下一代码行：

[!code-csharp[PrintResults](./snippets/movie-recommendation/csharp/Program.cs#PrintResults "Print the recommendation prediction results")]

在 `Main()` 方法中添加以下代码作为下一代码行，调用 `UseModelForSinglePrediction()` 方法：

[!code-csharp[UseModelMain](./snippets/movie-recommendation/csharp/Program.cs#UseModelMain "Add UseModelForSinglePrediction method in Main")]

此方法的输出应类似于以下文本：

```console
=============== Making a prediction ===============
Movie 10 is recommended for user 6
```

### <a name="save-your-model"></a>保存模型

若要使用模型在最终用户应用程序中进行预测，必须先保存模型。

使用下面的代码紧随 `UseModelForSinglePrediction()` 方法后创建 `SaveModel()` 方法：

```csharp
public static void SaveModel(MLContext mlContext, DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

通过在 `SaveModel()` 方法中添加以下代码来保存经过训练的模型：

[!code-csharp[SaveModel](./snippets/movie-recommendation/csharp/Program.cs#SaveModel "Save the model to a zip file")]

此方法会将经过训练的模型保存到 .zip 文件（在“数据”文件夹中），然后可以在其他 .NET 应用程序中使用该文件进行预测。

在 `Main()` 方法中添加以下代码作为下一代码行，调用 `SaveModel()` 方法：

[!code-csharp[SaveModelMain](./snippets/movie-recommendation/csharp/Program.cs#SaveModelMain "Create SaveModel method in Main")]

### <a name="use-your-saved-model"></a>使用保存的模型

保存已定型模型后，可以在不同的环境中使用该模型。 请参阅[保存和加载已定型模型](../how-to-guides/save-load-machine-learning-models-ml-net.md)，了解如何在应用中操作定型的机器学习模型。

## <a name="results"></a>结果

按照上述步骤操作后，运行控制台应用程序 (Ctrl + F5)。 上述单一预测的结果应与以下内容类似。 你可能会看到警告或处理消息，为清楚起见，这些消息已从以下结果中删除。

```console
=============== Training the model ===============
iter      tr_rmse          obj
   0       1.5382   3.1213e+05
   1       0.9223   1.6051e+05
   2       0.8691   1.5050e+05
   3       0.8413   1.4576e+05
   4       0.8145   1.4208e+05
   5       0.7848   1.3895e+05
   6       0.7552   1.3613e+05
   7       0.7259   1.3357e+05
   8       0.6987   1.3121e+05
   9       0.6747   1.2949e+05
  10       0.6533   1.2766e+05
  11       0.6353   1.2636e+05
  12       0.6209   1.2561e+05
  13       0.6072   1.2462e+05
  14       0.5965   1.2394e+05
  15       0.5868   1.2352e+05
  16       0.5782   1.2279e+05
  17       0.5713   1.2227e+05
  18       0.5637   1.2190e+05
  19       0.5604   1.2178e+05
=============== Evaluating the model ===============
Rms: 0.977175077487166
RSquared: 0.43233349213192
=============== Making a prediction ===============
Movie 10 is recommended for user 6
=============== Saving the model to a file ===============
```

祝贺你！ 现已成功构建了用于推荐影片的机器学习模型。 可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/MovieRecommendation) 存储库中找到本教程的源代码。

## <a name="improve-your-model"></a>提升模型

有几种方法可以提升模型的性能，以便可以获得更准确的预测。

### <a name="data"></a>数据

可添加更多训练数据，并在其中包括针对每个用户和影片 ID 的足够样本，以帮助提升推荐模型的质量。

[交叉验证](../how-to-guides/train-machine-learning-model-cross-validation-ml-net.md)是一种评估模型的方法，它将数据随机分成子集（而不是像你在本教程中那样从数据集中提取测试数据），并将一些组作为训练数据，一些组作为测试数据。 从模型质量方面看，该方法优于进行训练-测试拆分。

### <a name="features"></a>特征

在本教程中，只使用数据集提供的三个 `Features`（`user id`、`movie id` 和 `rating`）。

虽然这是一个良好的开端，但实际上你可能希望添加其他属性或 `Features`（例如，年龄、性别、地理位置等），如果它们包含在数据集中。 添加更相关的 `Features` 有助于提升推荐模型的性能。

如果你不确定哪个 `Features` 可能与机器学习任务最相关，还可以使用 ML.NET 提供的特征贡献计算 (FCC) 和[排列特征重要性](../how-to-guides/explain-machine-learning-model-permutation-feature-importance-ml-net.md)来发现最有影响力的 `Features`。

### <a name="algorithm-hyperparameters"></a>算法超参数

虽然 ML.NET 提供了良好的默认训练算法，但可以通过更改算法的[超参数](../resources/glossary.md#hyperparameter)来进一步微调性能。

对于 `Matrix Factorization`，可尝试使用超参数，例如 [NumberOfIterations](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options.NumberOfIterations) 和 [ApproximationRank](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Options.ApproximationRank) 来查看是否可以获得更好的结果。

例如，在本教程中，算法选项是：

```csharp
var options = new MatrixFactorizationTrainer.Options
{
    MatrixColumnIndexColumnName = "userIdEncoded",
    MatrixRowIndexColumnName = "movieIdEncoded",
    LabelColumnName = "Label",
    NumberOfIterations = 20,
    ApproximationRank = 100
};
```

### <a name="other-recommendation-algorithms"></a>其他推荐算法

具有协作筛选的矩阵分解算法只是用于执行影片推荐的一种方法。 在许多情况下，可能没有可用的评分数据，并且只有用户可以获得影片历史记录。 在其他情况下，你可能不仅仅拥有用户的评分数据。

| 算法       | 方案           | 示例  |
| ------------- |:-------------:| -----:|
| 一类矩阵分解 | 当只有 userId 和 movieId 时使用此选项。 这种推荐方式基于共同购买方案或经常一起购买的产品，这意味着它将根据自己的采购订单历史记录向客户推荐一组产品。 | [>试试吧](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/MatrixFactorization_ProductRecommendation) |
| 场感知分解机 | 当拥有的特征不止 userId、productId 和评分（例如产品描述或产品价格）时，可使用此选项进行建议。 此方法也使用协作筛选法。 | [>试试吧](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/end-to-end-apps/Recommendation-MovieRecommender) |

### <a name="new-user-scenario"></a>新用户方案

协作筛选中的一个常见问题是“冷开始问题”，即有一个新用户，没有用于进行推理的任何旧数据。 该问题通常可通过要求新用户创建个人资料来解决，例如，对他们过去看过的影片评分。 虽然此方法会给用户带来一些负担，但它可为没有评分历史记录的新用户提供一些开始数据。

## <a name="resources"></a>资源

本教程中使用的数据源自 [MovieLens 数据集](http://files.grouplens.org/datasets/movielens/)。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 选择机器学习算法
> * 准备并加载数据
> * 生成并训练模型
> * 评估模型
> * 部署和使用模型

进入下一教程了解详细信息
> [!div class="nextstepaction"]
> [情绪分析](sentiment-analysis.md)
