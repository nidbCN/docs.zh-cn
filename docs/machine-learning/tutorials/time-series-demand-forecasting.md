---
title: 教程：预测自行车租赁需求 - 时序
description: 本教程介绍如何使用单变量时序分析和 ML.NET 来预测自行车租赁服务需求。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.author: luquinta
author: luisquintanilla
ms.openlocfilehash: aea264036ab4766696699980f464cca3d8229499
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875585"
---
# <a name="tutorial-forecast-bike-rental-service-demand-with-time-series-analysis-and-mlnet"></a>教程：使用时序分析和 ML.NET 预测自行车租赁服务需求

了解如何通过 ML.NET 对 SQL Server 数据库中存储的数据进行单变量时序分析，以预测自行车租赁服务需求。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 了解问题
> * 从数据库加载数据
> * 创建预测模型
> * 评估预测模型
> * 保存预测模型
> * 使用预测模型

## <a name="prerequisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。

## <a name="time-series-forecasting-sample-overview"></a>时序预测示例概述

此示例为 C# .NET Core 控制台应用程序，它使用单变量时序分析算法（称为单谱分析）来预测自行车租赁需求。 此示例的代码可以在 GitHub 上的 [dotnet/machinelearning-samples 存储库](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand)找到。

## <a name="understand-the-problem"></a>了解问题

为了实现高效运营，其中库存管理的作用不可或缺。 产品库存过多意味着产品积压，无法产生收入。 产品库存过少会损失销售额，导致客户转而购买竞争对手的产品。 因此，一个永恒的问题就是：保有多少库存才最合适呢？ 借助时序分析，可通过查看历史数据、识别模式并使用此信息来预测未来某个时间的值，从而帮助找到这些问题的答案。

此教程使用的数据分析技术为单变量时序分析。 单变量时序分析可按照特定间隔（如月销售额）查看一个时段内的单个数值观测。

本教程中使用的算法是[单谱分析 (SSA)](http://ssa.cf.ac.uk/zhigljavsky/pdfs/SSA/SSA_encyclopedia.pdf)。 SSA 会将时序分解为一组主要成分， 可以将这些成分解释为信号的组成部分，对应于趋势、噪音、季节性及许多其他的因素。 然后重新构建这些成分，并用来预测未来某个时间的值。

## <a name="create-console-application"></a>创建控制台应用程序

1. 新建一个名称为“BikeDemandForecasting”的“C# .NET Core 控制台应用程序”。
1. 安装 Microsoft.ML 版本 NuGet 包

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    1. 在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”。
    1. 选择“nuget.org”作为“包源”，选择“浏览”选项卡，再搜索“Microsoft.ML”。
    1. 选中“包括预发行版”复选框。
    1. 选择“安装”按钮  。
    1. 选择“预览更改”对话框中的“确定”按钮；如果同意所列包的许可条款，请选择“接受许可”对话框中的“我接受”按钮。
    1. 针对 System.Data.SqlClient 和 Microsoft.ML.TimeSeries 重复上述步骤 。

### <a name="prepare-and-understand-the-data"></a>准备和了解数据

1. 创建一个名为“Data”的目录。
1. 下载 [DailyDemand.mdf 数据库文件](https://github.com/dotnet/machinelearning-samples/raw/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Data/DailyDemand.mdf)并将其保存到“Data”目录中。 

> [!NOTE]
> 此教程使用的数据来自 [UCI 自行车共享数据集](http://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset)。 作者 Fanaee-T,Hadi 和 Gama, Joao，“事件标签结合集合探测器和背景知识”，人工智能进展 (2013)：1-15 页，Springer Berlin Heidelberg，[网页链接](https://link.springer.com/article/10.1007%2Fs13748-013-0040-3)。

原始数据集包含与季节和天气相对应的若干列。 为了简洁起见，并且由于本教程使用的算法仅需要单个数值列中的值，因此，已将原始数据集精简为仅包括以下列：

- **dteday**：观测日期。
- **year**：观测年份编码（0=2011，1=2012）。
- **cnt**：观测日当天自行车租赁总数。

原始数据集映射到 SQL Server 数据库中具有以下架构的数据库表。

```sql
CREATE TABLE [Rentals] (
    [RentalDate] DATE NOT NULL,
    [Year] INT NOT NULL,
    [TotalRentals] INT NOT NULL
);
```

以下是数据示例：

| RentalDate | 年 | TotalRentals |
| --- | --- | --- |
|1/1/2011|0|985|
|1/2/2011|0|801|
|1/3/2011|0|1349|

### <a name="create-input-and-output-classes"></a>创建输入和输出类

1. 打开 Program.cs 文件，将现有 `using` 语句替换为以下内容：

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L1-L8)]

1. 创建 `ModelInput` 类。 在 `Program` 类下面，添加以下代码。

    [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L120-L127)]

    `ModelInput` 类包含以下列：

    - **RentalDate**：观测日期。
    - **Year**：观测年份编码（0=2011，1=2012）。
    - **TotalRentals**：观测日当天自行车租赁总数。

1. 在新建的 `ModelOutput` 类的下面，创建 `ModelInput` 类。

    [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L129-L136)]

    `ModelOutput` 类包含以下列：

    - **ForecastedRentals**：预测时段内的预测值。
    - **LowerBoundRentals**：预测时段内的最低预测值。
    - **UpperBoundRentals**：预测时段内的最高预测值。

### <a name="define-paths-and-initialize-variables"></a>定义路径并初始化变量

1. 在 `Main` 方法中，定义变量，用于存储数据位置、连接字符串，以及保存培训的模型位置。

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L16-L19)]

1. 通过将以下行添加到 `Main` 方法，使用新的 [`MLContext`](xref:Microsoft.ML.MLContext) 实例初始化 `mlContext` 变量。

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L21)]

    执行所有 ML.NET 操作都是从 [`MLContext`](xref:Microsoft.ML.MLContext) 类开始，初始化 mlContext 将创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

## <a name="load-the-data"></a>加载数据

1. 创建 `DatabaseLoader`，用于加载 `ModelInput` 类型的记录。

    [!code-csharp [CreateDBLoader](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L23)]

1. 定义查询，以从数据库加载数据。

    [!code-csharp [DefineSQLQuery](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L25)]

    ML.NET 算法要求数据是 [`Single`](xref:System.Single) 类型。 因此，必须将来自数据库的非 [`Real`](xref:System.Data.SqlDbType) 类型的数值（单精度浮点值）转换为 [`Real`](xref:System.Data.SqlDbType)。

    数据库中的 `Year` 和 `TotalRental` 列都是整数类型。 使用 `CAST` 内置函数将它们都转换为 `Real`。

1. 创建 `DatabaseSource` 以连接到数据库，并执行查询。

    [!code-csharp [CreateDBSource](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L27-L29)]

1. 将数据加载到 `IDataView` 中。

    [!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L31)]

1. 此数据集包含两年的重要数据。 第一年的数据仅用于培训，第二年的数据用于将实际值与模型生成的预测进行比较。 使用 [`FilterRowsByColumn`](xref:Microsoft.ML.DataOperationsCatalog.FilterRowsByColumn%2A) 转换筛选数据。

    [!code-csharp [SplitData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L33-L34)]

    对于第一年，通过将 `upperBound` 参数设置为 1 来仅选择 `Year` 列中小于 1 的值。 相反，对于第二年，通过将 `lowerBound` 参数设置为 1 来仅选择大于或等于 1 的值。

## <a name="define-time-series-analysis-pipeline"></a>定义时序分析管道

1. 定义使用 [SsaForecastingEstimator](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator) 预测时序数据集中的值的管道。

    [!code-csharp [DefinePipeline](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L36-L45)]

    `forecastingPipeline` 在第一年数据中获取 365 个数据点，并按 `seriesLength` 参数指定的间隔从时序数据集采样或将其分为 30 天（每月）的间隔。 以一周或 7 天为一个时段分析各个样本。 确定下一个时段的预测值时，使用前面 7 天的值进行预测。 根据 `horizon` 参数的定义，该模型设置为预测将来的 7 个时段。 由于预测属于合理猜测，它不总是完全准确。 因此，最好了解上限和下限定义的最佳和最坏情况下的范围值。 在本案例中，设置的上下限可信度为 95%。 可信度可以相应地提高或降低。 值越高，上限和下限之间的范围越大，以便达到所需的可信度。

1. 使用 [`Fit`](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator.Fit%2A) 方法培训模型，使数据适用于前面定义的 `forecastingPipeline`。

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L47)]

## <a name="evaluate-the-model"></a>评估模型

通过预测下一年的数据并将其与实际值进行比较，评估模型的执行情况。

1. 在 `Main` 方法下面，创建一个名为 `Evaluate` 的新实用方法。

    ```csharp
    static void Evaluate(IDataView testData, ITransformer model, MLContext mlContext)
    {

    }
    ```

1. 在 `Evaluate` 方法中，通过结合使用 [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) 方法和培训模型，预测第二年的数据。

    [!code-csharp [EvaluateForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L62)]

1. 使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法，从数据中获取实际值。

    [!code-csharp [GetActualRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L65-L67)]

1. 使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法获取预测值。

    [!code-csharp [GetForecastRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L70-L72)]

1. 计算实际值和预测值之间的差值（通常称为“误差”）。

    [!code-csharp [CalculateError](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L75)]

1. 通过计算平均绝对误差和均方根误差值测量性能。

    [!code-csharp [CalculateMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L78-L79)]

    使用以下指标来评估性能：

    - **平均绝对误差**：度量预测与实际值之间的接近程度。 此值介于 0 到无限大之间。 越接近 0，模型的质量越好。
    - **均方根误差**：汇总模型中的错误。 此值介于 0 到无限大之间。 越接近 0，模型的质量越好。

1. 将指标输出到控制台。

    [!code-csharp [OutputMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L82-L85)]

1. 在 `Main` 方法中使用 `Evaluate` 方法。

    [!code-csharp [EvaluateModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L49)]

## <a name="save-the-model"></a>保存模型

如果对模型满意，则保存它，以便以后用于其他应用程序。

1. 在 `Main` 方法中创建 [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602)。 [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602) 是进行单个预测的一个便捷方法。

    [!code-csharp [CreateTimeSeriesEngine](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L51)]

1. 将此模型保存到由先前定义的 `modelPath` 变量指定的名为 `MLModel.zip` 的文件。 使用 [`Checkpoint`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.CheckPoint%2A) 方法保存模型。

    [!code-csharp [SaveModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L52)]

## <a name="use-the-model-to-forecast-demand"></a>使用模型预测需求

1. 在 `Evaluate` 方法下面，创建一个名为 `Forecast` 的新实用方法。

    ```csharp
    static void Forecast(IDataView testData, int horizon, TimeSeriesPredictionEngine<ModelInput, ModelOutput> forecaster, MLContext mlContext)
    {

    }
    ```

1. 在 `Forecast` 方法中，使用 [`Predict`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.Predict%2A) 方法预测接下来的 7 天的租赁数量。

    [!code-csharp [SingleForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L91)]

1. 排列 7 个时段的实际值和预测值。

    [!code-csharp [GetForecastOutput](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L93-L108)]

1. 循环访问预测输出，并在控制台上显示它。

    [!code-csharp [DisplayForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L111-L116)]

## <a name="run-the-application"></a>运行此应用程序

1. 在 `Main` 方法中，调用 `Forecast` 方法。

    [!code-csharp [BuildForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L54)]

1. 运行该应用程序。 控制台应显示类似以下内容的输出。 为简洁起见，输出已进行压缩。

    ```text
    Evaluation Metrics
    ---------------------
    Mean Absolute Error: 726.416
    Root Mean Squared Error: 987.658

    Rental Forecast
    ---------------------
    Date: 1/1/2012
    Actual Rentals: 2294
    Lower Estimate: 1197.842
    Forecast: 2334.443
    Upper Estimate: 3471.044

    Date: 1/2/2012
    Actual Rentals: 1951
    Lower Estimate: 1148.412
    Forecast: 2360.861
    Upper Estimate: 3573.309
    ```

通过观测实际值和预测值，获得以下关系：

![实际值和预测值比较](./media/time-series-demand-forecasting/forecast.png)

尽管预测值并不能预测准确的租赁数，但它们缩小了值的范围，企业可以通过它们优化资源利用。

祝贺你！ 你已成功生成用于预测自行车租赁需求的时序机器学习模型。

可以在 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand) 存储库中找到本教程的源代码。

## <a name="next-steps"></a>后续步骤

- [ML.NET 中的机器学习任务](../resources/tasks.md)
- [提高模型准确性](../resources/improve-machine-learning-model-ml-net.md)
