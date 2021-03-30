---
title: 教程：使用回归预测价格
description: 本教程演示如何使用 ML.NET 生成回归模型来预测价格，特别是纽约市的出租车费。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: f8f7146241368edaca31afd970af54a098d1aafa
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874636"
---
# <a name="tutorial-predict-prices-using-regression-with-mlnet"></a>教程：将回归与 ML.NET 配合使用以预测价格

本教程演示如何使用 ML.NET 生成[回归模型](../resources/glossary.md#regression)来预测价格，特别是纽约市的出租车费。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 准备和了解数据
> * 加载和转换数据
> * 选择学习算法
> * 定型模型
> * 评估模型
> * 使用预测模型

## <a name="prerequisites"></a>先决条件

* 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 创建名为“TaxiFarePrediction”的“.NET Core 控制台应用程序”。

1. 在项目中创建一个名为“数据”的目录来保存数据集和模型文件。

1. 安装“Microsoft.ML”和“Microsoft.ML.FastTree”NuGet 包 ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包” 。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，在列表中选择包，再选择“安装”按钮  。 选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。 对 **Microsoft.ML.FastTree** NuGet 包执行相同操作。

## <a name="prepare-and-understand-the-data"></a>准备和了解数据

1. 下载 [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/main/test/data/taxi-fare-train.csv) 和 [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/main/test/data/taxi-fare-test.csv) 数据集，并将它们保存到先前创建的“数据”文件夹。 我们使用这些数据集定型机器学习模型，然后评估模型的准确性。 这些数据集最初来自 [NYC TLC 出租车行程数据集](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)。

1. 在“解决方案资源管理器”中，右键单击每个 \*.csv 文件，然后选择“属性” 。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

1. 打开“taxi-fare-train.csv”数据集并查看第一行中的列标题。 查看每个列。 了解数据并确定哪些列是“特征”以及哪些是“标签” 。

`label` 是要预测的列。 标识的 `Features` 是为模型提供的用来预测 `Label` 的输入。

提供的数据集包含以下列：

* **vendor_id：** 出租车供应商的 ID 是一项特征。
* **rate_code：** 出租车行程的费率类型是一项特征。
* **passenger_count：** 行程中的乘客人数是一项特征。
* **trip_time_in_secs：** 这次行程所花的时间。 希望在行程完成前预测行程费用。 当时并不知道行程有多长。 因此，行程时间不是一项特征，需要从模型删除此列。
* **trip_distance：** 行程距离是一项特征。
* **payment_type：** 付款方式（现金或信用卡）是一项特征。
* **fare_amount：** 支付的总出租车费用是一个标签。

## <a name="create-data-classes"></a>创建数据类

创建输入数据和预测类：

1. 在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。
1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“TaxiTrip.cs”。 然后，选择“添加”  按钮。
1. 将以下 `using` 指令添加到新文件：

   [!code-csharp[AddUsings](./snippets/predict-prices/csharp/TaxiTrip.cs#1 "Add necessary usings")]

删除现有类定义并向“TaxiTrip.cs”文件添加以下代码，其中有两个类 `TaxiTrip` 和 `TaxiTripFarePrediction`：

[!code-csharp[DefineTaxiTrip](./snippets/predict-prices/csharp/TaxiTrip.cs#2 "Define the taxi trip and fare predictions classes")]

`TaxiTrip` 是输入数据类且具有针对每个数据集列的定义。 使用 <xref:Microsoft.ML.Data.LoadColumnAttribute> 属性在数据集中指定源列的索引。

`TaxiTripFarePrediction` 类表示预测的结果。 它应用了单个浮动 `FareAmount` 字段，附带 `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> 属性。 对于回归任务，“分数”列包含预测的标签值。

> [!NOTE]
> 使用 `float` 类型来表示输入和预测数据类中的浮点值。

### <a name="define-data-and-model-paths"></a>定义数据和模型路径

将以下附加的 `using` 语句添加到“Program.cs”文件顶部：

[!code-csharp[AddUsings](./snippets/predict-prices/csharp/Program.cs#1 "Add necessary usings")]

需要创建三个字段，用于保留有数据集的文件的路径，以及用于保存模型的文件的路径：

* `_trainDataPath` 包含具有用于定型模型的数据集的文件的路径。
* `_testDataPath` 包含具有用于评估模型的数据集的文件的路径。
* `_modelPath` 包含用于存储定型模型的文件的路径。

将以下代码添加到 `Main` 方法正上方，以指定这些路径和 `_textLoader` 变量：

[!code-csharp[InitializePaths](./snippets/predict-prices/csharp/Program.cs#2 "Define variables to store the data file paths")]

所有 ML.NET 操作都从 [MLContext 类](xref:Microsoft.ML.MLContext)开始。 初始化 `mlContext` 创建了新的 ML.NET 环境，可以在模型创建工作流对象之间共享。 从概念上讲，它与实体框架中的 `DBContext` 类似。

### <a name="initialize-variables-in-main"></a>在 Main 中初始化变量

使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 `mlContext` 变量：

[!code-csharp[CreateMLContext](./snippets/predict-prices/csharp/Program.cs#3 "Create the ML Context")]

在 `Main` 方法中添加以下代码作为调用 `Train` 方法的下一行代码：

[!code-csharp[Train](./snippets/predict-prices/csharp/Program.cs#5 "Train your model")]

`Train()` 方法执行以下任务：

* 加载数据。
* 提取并转换数据。
* 定型模型。
* 返回模型。

`Train` 方法定型模型。 使用以下代码在 `Main` 下创建该方法：

```csharp
public static ITransformer Train(MLContext mlContext, string dataPath)
{

}
```

## <a name="load-and-transform-data"></a>加载和转换数据

ML.NET 使用 [IDataView 类](xref:Microsoft.ML.IDataView)灵活、有效地描述数字或文本表格数据。 `IDataView` 可以加载文本文件或进行实时加载（例如，SQL 数据库或日志文件）。 将以下代码添加为 `Train()` 方法的首行：

[!code-csharp[LoadTrainData](./snippets/predict-prices/csharp/Program.cs#6 "loading training dataset")]

由于要预测出租车车费，`FareAmount` 列是将预测的 `Label`（模型的输出）。 使用 `CopyColumnsEstimator` 转换类以复制 `FareAmount`，并添加以下代码：

[!code-csharp[CopyColumnsEstimator](./snippets/predict-prices/csharp/Program.cs#7 "Use the CopyColumnsEstimator")]

定型模型的算法需要数字特性，所以必须将分类数据（`VendorId`、`RateCode` 和 `PaymentType`）值转换为数字（`VendorIdEncoded`、`RateCodeEncoded` 和 `PaymentTypeEncoded`）。 为此，请使用 [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer) 转换类（它将不同的数字键值分配到每列的不同值），并添加以下代码：

[!code-csharp[OneHotEncodingEstimator](./snippets/predict-prices/csharp/Program.cs#8 "Use the OneHotEncodingEstimator")]

数据准备最后一步使用 `mlContext.Transforms.Concatenate` 转换类将所有功能列合并到“功能”列。 默认情况下，学习算法仅处理“特征”列的特征。 添加以下代码：

[!code-csharp[ColumnConcatenatingEstimator](./snippets/predict-prices/csharp/Program.cs#9 "Use the ColumnConcatenatingEstimator")]

## <a name="choose-a-learning-algorithm"></a>选择学习算法

此问题关于预测纽约市的出租车车费。 从表面看，似乎只取决于行程距离。 但是，由于其他因素（比如额外的乘客或使用信用卡而非现金付款），纽约的出租车供应商收费不同。 你想要预测价格值，该值是基于数据集中的其他因素的实际值。 要执行此操作，请选择[回归](../resources/glossary.md#regression)机器学习任务。

将 [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) 机器学习任务追加到数据转换定义中，方法是在 `Train()` 中添加以下代码作为下一行代码：

[!code-csharp[FastTreeRegressionTrainer](./snippets/predict-prices/csharp/Program.cs#10 "Add the FastTreeRegressionTrainer")]

## <a name="train-the-model"></a>定型模型

使模型适合训练 `dataview` 并通过在 `Train()` 方法中添加以下代码行来返回训练的模型：

[!code-csharp[TrainModel](./snippets/predict-prices/csharp/Program.cs#11 "Train the model")]

[Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) 方法通过转换数据集并应用训练来训练模型。

使用 `Train()` 方法中的以下代码行返回训练的模型：

[!code-csharp[ReturnModel](./snippets/predict-prices/csharp/Program.cs#12 "Return the model")]

## <a name="evaluate-the-model"></a>评估模型

接下来，使用测试数据评估模型性能，以确保质量和验证。 使用以下代码在 `Train()` 之后创建 `Evaluate()` 方法：

```csharp
private static void Evaluate(MLContext mlContext, ITransformer model)
{

}
```

`Evaluate` 方法执行以下任务：

* 加载测试数据集。
* 创建回归计算器。
* 评估模型并创建指标。
* 显示指标。

使用下面的代码，在 `Train` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：

[!code-csharp[CallEvaluate](./snippets/predict-prices/csharp/Program.cs#14 "Call the Evaluate method")]

使用 [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) 方法加载测试数据集。 使用此数据集作为质量检查评估模型，方法是在 `Evaluate` 方法中添加以下代码：

[!code-csharp[LoadTestDataset](./snippets/predict-prices/csharp/Program.cs#15 "Load the test dataset")]

接下来，通过将以下代码添加到 `Evaluate()` 来转换 `Test` 数据：

[!code-csharp[PredictWithTransformer](./snippets/predict-prices/csharp/Program.cs#16 "Predict using the Transformer")]

[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对测试数据集输入行进行预测。

`RegressionContext.Evaluate` 方法使用指定数据集计算 `PredictionModel` 的质量指标。 它返回一个 <xref:Microsoft.ML.Data.RegressionMetrics> 对象，其中包含由回归计算器计算出的总体指标。

若要显示这些指标来确定模型质量，需要先获取指标。 将以下代码作为下一行添加到 `Evaluate` 方法中：

[!code-csharp[ComputeMetrics](./snippets/predict-prices/csharp/Program.cs#17 "Compute Metrics")]

获得预测集后，[Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) 方法会对模型进行评估，该模型会将预测值与测试数据集中的实际 `Labels` 进行比较，并返回有关模型执行情况的指标。

添加以下代码以评估模型并生成评估模型：

```csharp
Console.WriteLine();
Console.WriteLine($"*************************************************");
Console.WriteLine($"*       Model quality metrics evaluation         ");
Console.WriteLine($"*------------------------------------------------");
```

[RSquared](../resources/glossary.md#coefficient-of-determination) 是回归模型的另一种评估指标。 RSquared 在 0 和 1 之间取值。 值越接近 1，模型就越好。 将以下代码添加到 `Evaluate` 方法以显示 RSquared 值：

[!code-csharp[DisplayRSquared](./snippets/predict-prices/csharp/Program.cs#18 "Display the RSquared metric.")]

[RMS](../resources/glossary.md#root-of-mean-squared-error-rmse) 是回归模型的一种评估指标。 指标越低，模型就越好。 将以下代码添加到 `Evaluate` 方法以显示 RMS 值：

[!code-csharp[DisplayRMS](./snippets/predict-prices/csharp/Program.cs#19 "Display the RMS metric.")]

## <a name="use-the-model-for-predictions"></a>使用预测模型

使用下面的代码紧随 `Evaluate` 方法后创建 `TestSinglePrediction` 方法：

```csharp
private static void TestSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

`TestSinglePrediction` 方法执行以下任务：

* 创建测试数据的单个注释。
* 根据测试数据预测费用。
* 结合测试数据和预测进行报告。
* 显示预测结果。

使用下面的代码，在 `Evaluate` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：

[!code-csharp[CallTestSinglePrediction](./snippets/predict-prices/csharp/Program.cs#20 "Call the TestSinglePrediction method")]

使用 `PredictionEngine` 通过将以下代码添加到 `TestSinglePrediction()` 来预测车费：

[!code-csharp[MakePredictionEngine](./snippets/predict-prices/csharp/Program.cs#22 "Create the PredictionFunction")]

[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 可以在单线程环境或原型环境中使用。 为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。 请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。

> [!NOTE]
> `PredictionEnginePool` 服务扩展目前处于预览状态。

本教程使用此类中的一个测试行程。 稍后可以添加其他方案，以尝试使用此模型。 通过创建一个 `TaxiTrip` 实例，在 `TestSinglePrediction()` 方法中添加一个行程来测试定型模型的成本预测：

[!code-csharp[PredictionData](./snippets/predict-prices/csharp/Program.cs#23 "Create test data for single prediction")]

接下来，根据出租车行程数据的单个实例预测车费，并通过在 `TestSinglePrediction()` 方法中添加以下代码作为下一行代码将其传递给 `PredictionEngine`：

[!code-csharp[Predict](./snippets/predict-prices/csharp/Program.cs#24 "Create a prediction of taxi fare")]

[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单个数据实例进行预测。

若要显示指定行程的预测费用，请将下面的代码添加到 `TestSinglePrediction` 方法中：

[!code-csharp[Predict](./snippets/predict-prices/csharp/Program.cs#25 "Display the prediction.")]

运行此程序，查看测试用例的预测出租车费。

祝贺你！ 你已成功生成用于预测出租车费的机器学习模型、评估其准确性，并用其进行预测。 可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TaxiFarePrediction) GitHub 存储库中找到本教程的源代码。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 准备和了解数据
> * 创建学习管道
> * 加载和转换数据
> * 选择学习算法
> * 定型模型
> * 评估模型
> * 使用预测模型

进入下一教程了解详细信息。

> [!div class="nextstepaction"]
> [Iris 聚类分析](iris-clustering.md)
