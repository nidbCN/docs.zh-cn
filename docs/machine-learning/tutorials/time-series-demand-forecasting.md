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
# <a name="tutorial-forecast-bike-rental-service-demand-with-time-series-analysis-and-mlnet"></a><span data-ttu-id="b6342-103">教程：使用时序分析和 ML.NET 预测自行车租赁服务需求</span><span class="sxs-lookup"><span data-stu-id="b6342-103">Tutorial: Forecast bike rental service demand with time series analysis and ML.NET</span></span>

<span data-ttu-id="b6342-104">了解如何通过 ML.NET 对 SQL Server 数据库中存储的数据进行单变量时序分析，以预测自行车租赁服务需求。</span><span class="sxs-lookup"><span data-stu-id="b6342-104">Learn how to forecast demand for a bike rental service using univariate time series analysis on data stored in a SQL Server database with ML.NET.</span></span>

<span data-ttu-id="b6342-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="b6342-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="b6342-106">了解问题</span><span class="sxs-lookup"><span data-stu-id="b6342-106">Understand the problem</span></span>
> * <span data-ttu-id="b6342-107">从数据库加载数据</span><span class="sxs-lookup"><span data-stu-id="b6342-107">Load data from a database</span></span>
> * <span data-ttu-id="b6342-108">创建预测模型</span><span class="sxs-lookup"><span data-stu-id="b6342-108">Create a forecasting model</span></span>
> * <span data-ttu-id="b6342-109">评估预测模型</span><span class="sxs-lookup"><span data-stu-id="b6342-109">Evaluate forecasting model</span></span>
> * <span data-ttu-id="b6342-110">保存预测模型</span><span class="sxs-lookup"><span data-stu-id="b6342-110">Save a forecasting model</span></span>
> * <span data-ttu-id="b6342-111">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="b6342-111">Use a forecasting model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b6342-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="b6342-112">Prerequisites</span></span>

- <span data-ttu-id="b6342-113">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="b6342-113">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="time-series-forecasting-sample-overview"></a><span data-ttu-id="b6342-114">时序预测示例概述</span><span class="sxs-lookup"><span data-stu-id="b6342-114">Time series forecasting sample overview</span></span>

<span data-ttu-id="b6342-115">此示例为 C# .NET Core 控制台应用程序，它使用单变量时序分析算法（称为单谱分析）来预测自行车租赁需求。</span><span class="sxs-lookup"><span data-stu-id="b6342-115">This sample is a **C# .NET Core console application** that forecasts demand for bike rentals using a univariate time series analysis algorithm known as Singular Spectrum Analysis.</span></span> <span data-ttu-id="b6342-116">此示例的代码可以在 GitHub 上的 [dotnet/machinelearning-samples 存储库](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand)找到。</span><span class="sxs-lookup"><span data-stu-id="b6342-116">The code for this sample can be found on the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand) repository on GitHub.</span></span>

## <a name="understand-the-problem"></a><span data-ttu-id="b6342-117">了解问题</span><span class="sxs-lookup"><span data-stu-id="b6342-117">Understand the problem</span></span>

<span data-ttu-id="b6342-118">为了实现高效运营，其中库存管理的作用不可或缺。</span><span class="sxs-lookup"><span data-stu-id="b6342-118">In order to run an efficient operation, inventory management plays a key role.</span></span> <span data-ttu-id="b6342-119">产品库存过多意味着产品积压，无法产生收入。</span><span class="sxs-lookup"><span data-stu-id="b6342-119">Having too much of a product in stock means unsold products sitting on the shelves not generating any revenue.</span></span> <span data-ttu-id="b6342-120">产品库存过少会损失销售额，导致客户转而购买竞争对手的产品。</span><span class="sxs-lookup"><span data-stu-id="b6342-120">Having too little product leads to lost sales and customers purchasing from competitors.</span></span> <span data-ttu-id="b6342-121">因此，一个永恒的问题就是：保有多少库存才最合适呢？</span><span class="sxs-lookup"><span data-stu-id="b6342-121">Therefore, the constant question is, what is the optimal amount of inventory to keep on hand?</span></span> <span data-ttu-id="b6342-122">借助时序分析，可通过查看历史数据、识别模式并使用此信息来预测未来某个时间的值，从而帮助找到这些问题的答案。</span><span class="sxs-lookup"><span data-stu-id="b6342-122">Time-series analysis helps provide an answer to these questions by looking at historical data, identifying patterns, and using this information to forecast values some time in the future.</span></span>

<span data-ttu-id="b6342-123">此教程使用的数据分析技术为单变量时序分析。</span><span class="sxs-lookup"><span data-stu-id="b6342-123">The technique for analyzing data used in this tutorial is univariate time-series analysis.</span></span> <span data-ttu-id="b6342-124">单变量时序分析可按照特定间隔（如月销售额）查看一个时段内的单个数值观测。</span><span class="sxs-lookup"><span data-stu-id="b6342-124">Univariate time-series analysis takes a look at a single numerical observation over a period of time at specific intervals such as monthly sales.</span></span>

<span data-ttu-id="b6342-125">本教程中使用的算法是[单谱分析 (SSA)](http://ssa.cf.ac.uk/zhigljavsky/pdfs/SSA/SSA_encyclopedia.pdf)。</span><span class="sxs-lookup"><span data-stu-id="b6342-125">The algorithm used in this tutorial is [Singular Spectrum Analysis(SSA)](http://ssa.cf.ac.uk/zhigljavsky/pdfs/SSA/SSA_encyclopedia.pdf).</span></span> <span data-ttu-id="b6342-126">SSA 会将时序分解为一组主要成分，</span><span class="sxs-lookup"><span data-stu-id="b6342-126">SSA works by decomposing a time-series into a set of principal components.</span></span> <span data-ttu-id="b6342-127">可以将这些成分解释为信号的组成部分，对应于趋势、噪音、季节性及许多其他的因素。</span><span class="sxs-lookup"><span data-stu-id="b6342-127">These components can be interpreted as the parts of a signal that correspond to trends, noise, seasonality, and many other factors.</span></span> <span data-ttu-id="b6342-128">然后重新构建这些成分，并用来预测未来某个时间的值。</span><span class="sxs-lookup"><span data-stu-id="b6342-128">Then, these components are reconstructed and used to forecast values some time in the future.</span></span>

## <a name="create-console-application"></a><span data-ttu-id="b6342-129">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="b6342-129">Create console application</span></span>

1. <span data-ttu-id="b6342-130">新建一个名称为“BikeDemandForecasting”的“C# .NET Core 控制台应用程序”。</span><span class="sxs-lookup"><span data-stu-id="b6342-130">Create a new **C# .NET Core console application** called "BikeDemandForecasting".</span></span>
1. <span data-ttu-id="b6342-131">安装 Microsoft.ML 版本 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="b6342-131">Install **Microsoft.ML** version NuGet package</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    1. <span data-ttu-id="b6342-132">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”。</span><span class="sxs-lookup"><span data-stu-id="b6342-132">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span>
    1. <span data-ttu-id="b6342-133">选择“nuget.org”作为“包源”，选择“浏览”选项卡，再搜索“Microsoft.ML”。</span><span class="sxs-lookup"><span data-stu-id="b6342-133">Choose "nuget.org" as the Package source, select the **Browse** tab, search for **Microsoft.ML**.</span></span>
    1. <span data-ttu-id="b6342-134">选中“包括预发行版”复选框。</span><span class="sxs-lookup"><span data-stu-id="b6342-134">Check the **Include prerelease** checkbox.</span></span>
    1. <span data-ttu-id="b6342-135">选择“安装”按钮  。</span><span class="sxs-lookup"><span data-stu-id="b6342-135">Select the **Install** button.</span></span>
    1. <span data-ttu-id="b6342-136">选择“预览更改”对话框中的“确定”按钮；如果同意所列包的许可条款，请选择“接受许可”对话框中的“我接受”按钮。</span><span class="sxs-lookup"><span data-stu-id="b6342-136">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the License Acceptance dialog if you agree with the license terms for the packages listed.</span></span>
    1. <span data-ttu-id="b6342-137">针对 System.Data.SqlClient 和 Microsoft.ML.TimeSeries 重复上述步骤 。</span><span class="sxs-lookup"><span data-stu-id="b6342-137">Repeat these steps for **System.Data.SqlClient** and **Microsoft.ML.TimeSeries**.</span></span>

### <a name="prepare-and-understand-the-data"></a><span data-ttu-id="b6342-138">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="b6342-138">Prepare and understand the data</span></span>

1. <span data-ttu-id="b6342-139">创建一个名为“Data”的目录。</span><span class="sxs-lookup"><span data-stu-id="b6342-139">Create a directory called *Data*.</span></span>
1. <span data-ttu-id="b6342-140">下载 [DailyDemand.mdf 数据库文件](https://github.com/dotnet/machinelearning-samples/raw/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Data/DailyDemand.mdf)并将其保存到“Data”目录中。 </span><span class="sxs-lookup"><span data-stu-id="b6342-140">Download the [*DailyDemand.mdf* database file](https://github.com/dotnet/machinelearning-samples/raw/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Data/DailyDemand.mdf) and save it to the *Data* directory.</span></span>

> [!NOTE]
> <span data-ttu-id="b6342-141">此教程使用的数据来自 [UCI 自行车共享数据集](http://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset)。</span><span class="sxs-lookup"><span data-stu-id="b6342-141">The data used in this tutorial comes from the [UCI Bike Sharing Dataset](http://archive.ics.uci.edu/ml/datasets/bike+sharing+dataset).</span></span> <span data-ttu-id="b6342-142">作者 Fanaee-T,Hadi 和 Gama, Joao，“事件标签结合集合探测器和背景知识”，人工智能进展 (2013)：1-15 页，Springer Berlin Heidelberg，[网页链接](https://link.springer.com/article/10.1007%2Fs13748-013-0040-3)。</span><span class="sxs-lookup"><span data-stu-id="b6342-142">Fanaee-T, Hadi, and Gama, Joao, 'Event labeling combining ensemble detectors and background knowledge', Progress in Artificial Intelligence (2013): pp. 1-15, Springer Berlin Heidelberg, [Web Link](https://link.springer.com/article/10.1007%2Fs13748-013-0040-3).</span></span>

<span data-ttu-id="b6342-143">原始数据集包含与季节和天气相对应的若干列。</span><span class="sxs-lookup"><span data-stu-id="b6342-143">The original dataset contains several columns corresponding to seasonality and weather.</span></span> <span data-ttu-id="b6342-144">为了简洁起见，并且由于本教程使用的算法仅需要单个数值列中的值，因此，已将原始数据集精简为仅包括以下列：</span><span class="sxs-lookup"><span data-stu-id="b6342-144">For brevity and because the algorithm used in this tutorial only requires the values from a single numerical column, the original dataset has been condensed to include only the following columns:</span></span>

- <span data-ttu-id="b6342-145">**dteday**：观测日期。</span><span class="sxs-lookup"><span data-stu-id="b6342-145">**dteday**: The date of the observation.</span></span>
- <span data-ttu-id="b6342-146">**year**：观测年份编码（0=2011，1=2012）。</span><span class="sxs-lookup"><span data-stu-id="b6342-146">**year**: The encoded year of the observation (0=2011, 1=2012).</span></span>
- <span data-ttu-id="b6342-147">**cnt**：观测日当天自行车租赁总数。</span><span class="sxs-lookup"><span data-stu-id="b6342-147">**cnt**: The total number of bike rentals for that day.</span></span>

<span data-ttu-id="b6342-148">原始数据集映射到 SQL Server 数据库中具有以下架构的数据库表。</span><span class="sxs-lookup"><span data-stu-id="b6342-148">The original dataset is mapped to a database table with the following schema in a SQL Server database.</span></span>

```sql
CREATE TABLE [Rentals] (
    [RentalDate] DATE NOT NULL,
    [Year] INT NOT NULL,
    [TotalRentals] INT NOT NULL
);
```

<span data-ttu-id="b6342-149">以下是数据示例：</span><span class="sxs-lookup"><span data-stu-id="b6342-149">The following is a sample of the data:</span></span>

| <span data-ttu-id="b6342-150">RentalDate</span><span class="sxs-lookup"><span data-stu-id="b6342-150">RentalDate</span></span> | <span data-ttu-id="b6342-151">年</span><span class="sxs-lookup"><span data-stu-id="b6342-151">Year</span></span> | <span data-ttu-id="b6342-152">TotalRentals</span><span class="sxs-lookup"><span data-stu-id="b6342-152">TotalRentals</span></span> |
| --- | --- | --- |
|<span data-ttu-id="b6342-153">1/1/2011</span><span class="sxs-lookup"><span data-stu-id="b6342-153">1/1/2011</span></span>|<span data-ttu-id="b6342-154">0</span><span class="sxs-lookup"><span data-stu-id="b6342-154">0</span></span>|<span data-ttu-id="b6342-155">985</span><span class="sxs-lookup"><span data-stu-id="b6342-155">985</span></span>|
|<span data-ttu-id="b6342-156">1/2/2011</span><span class="sxs-lookup"><span data-stu-id="b6342-156">1/2/2011</span></span>|<span data-ttu-id="b6342-157">0</span><span class="sxs-lookup"><span data-stu-id="b6342-157">0</span></span>|<span data-ttu-id="b6342-158">801</span><span class="sxs-lookup"><span data-stu-id="b6342-158">801</span></span>|
|<span data-ttu-id="b6342-159">1/3/2011</span><span class="sxs-lookup"><span data-stu-id="b6342-159">1/3/2011</span></span>|<span data-ttu-id="b6342-160">0</span><span class="sxs-lookup"><span data-stu-id="b6342-160">0</span></span>|<span data-ttu-id="b6342-161">1349</span><span class="sxs-lookup"><span data-stu-id="b6342-161">1349</span></span>|

### <a name="create-input-and-output-classes"></a><span data-ttu-id="b6342-162">创建输入和输出类</span><span class="sxs-lookup"><span data-stu-id="b6342-162">Create input and output classes</span></span>

1. <span data-ttu-id="b6342-163">打开 Program.cs 文件，将现有 `using` 语句替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="b6342-163">Open *Program.cs* file and replace the existing `using` statements with the following:</span></span>

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L1-L8)]

1. <span data-ttu-id="b6342-164">创建 `ModelInput` 类。</span><span class="sxs-lookup"><span data-stu-id="b6342-164">Create `ModelInput` class.</span></span> <span data-ttu-id="b6342-165">在 `Program` 类下面，添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="b6342-165">Below the `Program` class, add the following code.</span></span>

    [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L120-L127)]

    <span data-ttu-id="b6342-166">`ModelInput` 类包含以下列：</span><span class="sxs-lookup"><span data-stu-id="b6342-166">The `ModelInput` class contains the following columns:</span></span>

    - <span data-ttu-id="b6342-167">**RentalDate**：观测日期。</span><span class="sxs-lookup"><span data-stu-id="b6342-167">**RentalDate**: The date of the observation.</span></span>
    - <span data-ttu-id="b6342-168">**Year**：观测年份编码（0=2011，1=2012）。</span><span class="sxs-lookup"><span data-stu-id="b6342-168">**Year**: The encoded year of the observation (0=2011, 1=2012).</span></span>
    - <span data-ttu-id="b6342-169">**TotalRentals**：观测日当天自行车租赁总数。</span><span class="sxs-lookup"><span data-stu-id="b6342-169">**TotalRentals**: The total number of bike rentals for that day.</span></span>

1. <span data-ttu-id="b6342-170">在新建的 `ModelOutput` 类的下面，创建 `ModelInput` 类。</span><span class="sxs-lookup"><span data-stu-id="b6342-170">Create `ModelOutput` class below the newly created `ModelInput` class.</span></span>

    [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L129-L136)]

    <span data-ttu-id="b6342-171">`ModelOutput` 类包含以下列：</span><span class="sxs-lookup"><span data-stu-id="b6342-171">The `ModelOutput` class contains the following columns:</span></span>

    - <span data-ttu-id="b6342-172">**ForecastedRentals**：预测时段内的预测值。</span><span class="sxs-lookup"><span data-stu-id="b6342-172">**ForecastedRentals**: The predicted values for the forecasted period.</span></span>
    - <span data-ttu-id="b6342-173">**LowerBoundRentals**：预测时段内的最低预测值。</span><span class="sxs-lookup"><span data-stu-id="b6342-173">**LowerBoundRentals**: The predicted minimum values for the forecasted period.</span></span>
    - <span data-ttu-id="b6342-174">**UpperBoundRentals**：预测时段内的最高预测值。</span><span class="sxs-lookup"><span data-stu-id="b6342-174">**UpperBoundRentals**: The predicted maximum values for the forecasted period.</span></span>

### <a name="define-paths-and-initialize-variables"></a><span data-ttu-id="b6342-175">定义路径并初始化变量</span><span class="sxs-lookup"><span data-stu-id="b6342-175">Define paths and initialize variables</span></span>

1. <span data-ttu-id="b6342-176">在 `Main` 方法中，定义变量，用于存储数据位置、连接字符串，以及保存培训的模型位置。</span><span class="sxs-lookup"><span data-stu-id="b6342-176">Inside the `Main` method, define variables to store the location of your data, connection string, and where to save the trained model.</span></span>

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L16-L19)]

1. <span data-ttu-id="b6342-177">通过将以下行添加到 `Main` 方法，使用新的 [`MLContext`](xref:Microsoft.ML.MLContext) 实例初始化 `mlContext` 变量。</span><span class="sxs-lookup"><span data-stu-id="b6342-177">Initialize the `mlContext` variable with a new instance of [`MLContext`](xref:Microsoft.ML.MLContext) by adding the following line to the `Main` method.</span></span>

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L21)]

    <span data-ttu-id="b6342-178">执行所有 ML.NET 操作都是从 [`MLContext`](xref:Microsoft.ML.MLContext) 类开始，初始化 mlContext 将创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="b6342-178">The [`MLContext`](xref:Microsoft.ML.MLContext) class is a starting point for all ML.NET operations, and initializing mlContext creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="b6342-179">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="b6342-179">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="b6342-180">加载数据</span><span class="sxs-lookup"><span data-stu-id="b6342-180">Load the data</span></span>

1. <span data-ttu-id="b6342-181">创建 `DatabaseLoader`，用于加载 `ModelInput` 类型的记录。</span><span class="sxs-lookup"><span data-stu-id="b6342-181">Create `DatabaseLoader` that loads records of type `ModelInput`.</span></span>

    [!code-csharp [CreateDBLoader](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L23)]

1. <span data-ttu-id="b6342-182">定义查询，以从数据库加载数据。</span><span class="sxs-lookup"><span data-stu-id="b6342-182">Define the query to load the data from the database.</span></span>

    [!code-csharp [DefineSQLQuery](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L25)]

    <span data-ttu-id="b6342-183">ML.NET 算法要求数据是 [`Single`](xref:System.Single) 类型。</span><span class="sxs-lookup"><span data-stu-id="b6342-183">ML.NET algorithms expect data to be of type [`Single`](xref:System.Single).</span></span> <span data-ttu-id="b6342-184">因此，必须将来自数据库的非 [`Real`](xref:System.Data.SqlDbType) 类型的数值（单精度浮点值）转换为 [`Real`](xref:System.Data.SqlDbType)。</span><span class="sxs-lookup"><span data-stu-id="b6342-184">Therefore, numerical values coming from the database that are not of type [`Real`](xref:System.Data.SqlDbType), a single-precision floating-point value, have to be converted to [`Real`](xref:System.Data.SqlDbType).</span></span>

    <span data-ttu-id="b6342-185">数据库中的 `Year` 和 `TotalRental` 列都是整数类型。</span><span class="sxs-lookup"><span data-stu-id="b6342-185">The `Year` and `TotalRental` columns are both integer types in the database.</span></span> <span data-ttu-id="b6342-186">使用 `CAST` 内置函数将它们都转换为 `Real`。</span><span class="sxs-lookup"><span data-stu-id="b6342-186">Using the `CAST` built-in function, they are both cast to `Real`.</span></span>

1. <span data-ttu-id="b6342-187">创建 `DatabaseSource` 以连接到数据库，并执行查询。</span><span class="sxs-lookup"><span data-stu-id="b6342-187">Create a `DatabaseSource` to connect to the database and execute the query.</span></span>

    [!code-csharp [CreateDBSource](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L27-L29)]

1. <span data-ttu-id="b6342-188">将数据加载到 `IDataView` 中。</span><span class="sxs-lookup"><span data-stu-id="b6342-188">Load the data into an `IDataView`.</span></span>

    [!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L31)]

1. <span data-ttu-id="b6342-189">此数据集包含两年的重要数据。</span><span class="sxs-lookup"><span data-stu-id="b6342-189">The dataset contains two years worth of data.</span></span> <span data-ttu-id="b6342-190">第一年的数据仅用于培训，第二年的数据用于将实际值与模型生成的预测进行比较。</span><span class="sxs-lookup"><span data-stu-id="b6342-190">Only data from the first year is used for training, the second year is held out to compare the actual values against the forecast produced by the model.</span></span> <span data-ttu-id="b6342-191">使用 [`FilterRowsByColumn`](xref:Microsoft.ML.DataOperationsCatalog.FilterRowsByColumn%2A) 转换筛选数据。</span><span class="sxs-lookup"><span data-stu-id="b6342-191">Filter the data using the [`FilterRowsByColumn`](xref:Microsoft.ML.DataOperationsCatalog.FilterRowsByColumn%2A) transform.</span></span>

    [!code-csharp [SplitData](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L33-L34)]

    <span data-ttu-id="b6342-192">对于第一年，通过将 `upperBound` 参数设置为 1 来仅选择 `Year` 列中小于 1 的值。</span><span class="sxs-lookup"><span data-stu-id="b6342-192">For the first year, only the values in the `Year` column less than 1 are selected by setting the `upperBound` parameter to 1.</span></span> <span data-ttu-id="b6342-193">相反，对于第二年，通过将 `lowerBound` 参数设置为 1 来仅选择大于或等于 1 的值。</span><span class="sxs-lookup"><span data-stu-id="b6342-193">Conversely, for the second year, values greater than or equal to 1 are selected by setting the `lowerBound` parameter to 1.</span></span>

## <a name="define-time-series-analysis-pipeline"></a><span data-ttu-id="b6342-194">定义时序分析管道</span><span class="sxs-lookup"><span data-stu-id="b6342-194">Define time series analysis pipeline</span></span>

1. <span data-ttu-id="b6342-195">定义使用 [SsaForecastingEstimator](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator) 预测时序数据集中的值的管道。</span><span class="sxs-lookup"><span data-stu-id="b6342-195">Define a pipeline that uses the [SsaForecastingEstimator](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator) to forecast values in a time-series dataset.</span></span>

    [!code-csharp [DefinePipeline](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L36-L45)]

    <span data-ttu-id="b6342-196">`forecastingPipeline` 在第一年数据中获取 365 个数据点，并按 `seriesLength` 参数指定的间隔从时序数据集采样或将其分为 30 天（每月）的间隔。</span><span class="sxs-lookup"><span data-stu-id="b6342-196">The `forecastingPipeline` takes 365 data points for the first year and samples or splits the time-series dataset into 30-day (monthly) intervals as specified by the `seriesLength` parameter.</span></span> <span data-ttu-id="b6342-197">以一周或 7 天为一个时段分析各个样本。</span><span class="sxs-lookup"><span data-stu-id="b6342-197">Each of these samples is analyzed through weekly or a 7-day window.</span></span> <span data-ttu-id="b6342-198">确定下一个时段的预测值时，使用前面 7 天的值进行预测。</span><span class="sxs-lookup"><span data-stu-id="b6342-198">When determining what the forecasted value for the next period(s) is, the values from previous seven days are used to make a prediction.</span></span> <span data-ttu-id="b6342-199">根据 `horizon` 参数的定义，该模型设置为预测将来的 7 个时段。</span><span class="sxs-lookup"><span data-stu-id="b6342-199">The model is set to forecast seven periods into the future as defined by the `horizon` parameter.</span></span> <span data-ttu-id="b6342-200">由于预测属于合理猜测，它不总是完全准确。</span><span class="sxs-lookup"><span data-stu-id="b6342-200">Because a forecast is an informed guess, it's not always 100% accurate.</span></span> <span data-ttu-id="b6342-201">因此，最好了解上限和下限定义的最佳和最坏情况下的范围值。</span><span class="sxs-lookup"><span data-stu-id="b6342-201">Therefore, it's good to know the range of values in the best and worst-case scenarios as defined by the upper and lower bounds.</span></span> <span data-ttu-id="b6342-202">在本案例中，设置的上下限可信度为 95%。</span><span class="sxs-lookup"><span data-stu-id="b6342-202">In this case, the level of confidence for the lower and upper bounds is set to 95%.</span></span> <span data-ttu-id="b6342-203">可信度可以相应地提高或降低。</span><span class="sxs-lookup"><span data-stu-id="b6342-203">The confidence level can be increased or decreased accordingly.</span></span> <span data-ttu-id="b6342-204">值越高，上限和下限之间的范围越大，以便达到所需的可信度。</span><span class="sxs-lookup"><span data-stu-id="b6342-204">The higher the value, the wider the range is between the upper and lower bounds to achieve the desired level of confidence.</span></span>

1. <span data-ttu-id="b6342-205">使用 [`Fit`](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator.Fit%2A) 方法培训模型，使数据适用于前面定义的 `forecastingPipeline`。</span><span class="sxs-lookup"><span data-stu-id="b6342-205">Use the [`Fit`](xref:Microsoft.ML.Transforms.TimeSeries.SsaForecastingEstimator.Fit%2A) method to train the model and fit the data to the previously defined `forecastingPipeline`.</span></span>

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L47)]

## <a name="evaluate-the-model"></a><span data-ttu-id="b6342-206">评估模型</span><span class="sxs-lookup"><span data-stu-id="b6342-206">Evaluate the model</span></span>

<span data-ttu-id="b6342-207">通过预测下一年的数据并将其与实际值进行比较，评估模型的执行情况。</span><span class="sxs-lookup"><span data-stu-id="b6342-207">Evaluate how well the model performs by forecasting next year's data and comparing it against the actual values.</span></span>

1. <span data-ttu-id="b6342-208">在 `Main` 方法下面，创建一个名为 `Evaluate` 的新实用方法。</span><span class="sxs-lookup"><span data-stu-id="b6342-208">Below the `Main` method, create a new utility method called `Evaluate`.</span></span>

    ```csharp
    static void Evaluate(IDataView testData, ITransformer model, MLContext mlContext)
    {

    }
    ```

1. <span data-ttu-id="b6342-209">在 `Evaluate` 方法中，通过结合使用 [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) 方法和培训模型，预测第二年的数据。</span><span class="sxs-lookup"><span data-stu-id="b6342-209">Inside the `Evaluate` method, forecast the second year's data by using the [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) method with the trained model.</span></span>

    [!code-csharp [EvaluateForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L62)]

1. <span data-ttu-id="b6342-210">使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法，从数据中获取实际值。</span><span class="sxs-lookup"><span data-stu-id="b6342-210">Get the actual values from the data by using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method.</span></span>

    [!code-csharp [GetActualRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L65-L67)]

1. <span data-ttu-id="b6342-211">使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法获取预测值。</span><span class="sxs-lookup"><span data-stu-id="b6342-211">Get the forecast values by using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method.</span></span>

    [!code-csharp [GetForecastRentals](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L70-L72)]

1. <span data-ttu-id="b6342-212">计算实际值和预测值之间的差值（通常称为“误差”）。</span><span class="sxs-lookup"><span data-stu-id="b6342-212">Calculate the difference between the actual and forecast values, commonly referred to as the error.</span></span>

    [!code-csharp [CalculateError](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L75)]

1. <span data-ttu-id="b6342-213">通过计算平均绝对误差和均方根误差值测量性能。</span><span class="sxs-lookup"><span data-stu-id="b6342-213">Measure performance by computing the Mean Absolute Error and Root Mean Squared Error values.</span></span>

    [!code-csharp [CalculateMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L78-L79)]

    <span data-ttu-id="b6342-214">使用以下指标来评估性能：</span><span class="sxs-lookup"><span data-stu-id="b6342-214">To evaluate performance, the following metrics are used:</span></span>

    - <span data-ttu-id="b6342-215">**平均绝对误差**：度量预测与实际值之间的接近程度。</span><span class="sxs-lookup"><span data-stu-id="b6342-215">**Mean Absolute Error**: Measures how close predictions are to the actual value.</span></span> <span data-ttu-id="b6342-216">此值介于 0 到无限大之间。</span><span class="sxs-lookup"><span data-stu-id="b6342-216">This value ranges between 0 and infinity.</span></span> <span data-ttu-id="b6342-217">越接近 0，模型的质量越好。</span><span class="sxs-lookup"><span data-stu-id="b6342-217">The closer to 0, the better the quality of the model.</span></span>
    - <span data-ttu-id="b6342-218">**均方根误差**：汇总模型中的错误。</span><span class="sxs-lookup"><span data-stu-id="b6342-218">**Root Mean Squared Error**: Summarizes the error in the model.</span></span> <span data-ttu-id="b6342-219">此值介于 0 到无限大之间。</span><span class="sxs-lookup"><span data-stu-id="b6342-219">This value ranges between 0 and infinity.</span></span> <span data-ttu-id="b6342-220">越接近 0，模型的质量越好。</span><span class="sxs-lookup"><span data-stu-id="b6342-220">The closer to 0, the better the quality of the model.</span></span>

1. <span data-ttu-id="b6342-221">将指标输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="b6342-221">Output the metrics to the console.</span></span>

    [!code-csharp [OutputMetrics](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L82-L85)]

1. <span data-ttu-id="b6342-222">在 `Main` 方法中使用 `Evaluate` 方法。</span><span class="sxs-lookup"><span data-stu-id="b6342-222">Use the `Evaluate` method inside the `Main` method.</span></span>

    [!code-csharp [EvaluateModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L49)]

## <a name="save-the-model"></a><span data-ttu-id="b6342-223">保存模型</span><span class="sxs-lookup"><span data-stu-id="b6342-223">Save the model</span></span>

<span data-ttu-id="b6342-224">如果对模型满意，则保存它，以便以后用于其他应用程序。</span><span class="sxs-lookup"><span data-stu-id="b6342-224">If you're satisfied with your model, save it for later use in other applications.</span></span>

1. <span data-ttu-id="b6342-225">在 `Main` 方法中创建 [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602)。</span><span class="sxs-lookup"><span data-stu-id="b6342-225">In the `Main` method, create a [`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602).</span></span> <span data-ttu-id="b6342-226">[`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602) 是进行单个预测的一个便捷方法。</span><span class="sxs-lookup"><span data-stu-id="b6342-226">[`TimeSeriesPredictionEngine`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602) is a convenience method to make single predictions.</span></span>

    [!code-csharp [CreateTimeSeriesEngine](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L51)]

1. <span data-ttu-id="b6342-227">将此模型保存到由先前定义的 `modelPath` 变量指定的名为 `MLModel.zip` 的文件。</span><span class="sxs-lookup"><span data-stu-id="b6342-227">Save the model to a file called `MLModel.zip` as specified by the previously defined `modelPath` variable.</span></span> <span data-ttu-id="b6342-228">使用 [`Checkpoint`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.CheckPoint%2A) 方法保存模型。</span><span class="sxs-lookup"><span data-stu-id="b6342-228">Use the [`Checkpoint`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.CheckPoint%2A) method to save the model.</span></span>

    [!code-csharp [SaveModel](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L52)]

## <a name="use-the-model-to-forecast-demand"></a><span data-ttu-id="b6342-229">使用模型预测需求</span><span class="sxs-lookup"><span data-stu-id="b6342-229">Use the model to forecast demand</span></span>

1. <span data-ttu-id="b6342-230">在 `Evaluate` 方法下面，创建一个名为 `Forecast` 的新实用方法。</span><span class="sxs-lookup"><span data-stu-id="b6342-230">Below the `Evaluate` method, create a new utility method called `Forecast`.</span></span>

    ```csharp
    static void Forecast(IDataView testData, int horizon, TimeSeriesPredictionEngine<ModelInput, ModelOutput> forecaster, MLContext mlContext)
    {

    }
    ```

1. <span data-ttu-id="b6342-231">在 `Forecast` 方法中，使用 [`Predict`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.Predict%2A) 方法预测接下来的 7 天的租赁数量。</span><span class="sxs-lookup"><span data-stu-id="b6342-231">Inside the `Forecast` method, use the [`Predict`](xref:Microsoft.ML.Transforms.TimeSeries.TimeSeriesPredictionEngine%602.Predict%2A) method to forecast rentals for the next seven days.</span></span>

    [!code-csharp [SingleForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L91)]

1. <span data-ttu-id="b6342-232">排列 7 个时段的实际值和预测值。</span><span class="sxs-lookup"><span data-stu-id="b6342-232">Align the actual and forecast values for seven periods.</span></span>

    [!code-csharp [GetForecastOutput](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L93-L108)]

1. <span data-ttu-id="b6342-233">循环访问预测输出，并在控制台上显示它。</span><span class="sxs-lookup"><span data-stu-id="b6342-233">Iterate through the forecast output and display it on the console.</span></span>

    [!code-csharp [DisplayForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L111-L116)]

## <a name="run-the-application"></a><span data-ttu-id="b6342-234">运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="b6342-234">Run the application</span></span>

1. <span data-ttu-id="b6342-235">在 `Main` 方法中，调用 `Forecast` 方法。</span><span class="sxs-lookup"><span data-stu-id="b6342-235">Inside the `Main` method, call the `Forecast` method.</span></span>

    [!code-csharp [BuildForecast](~/machinelearning-samples/samples/csharp/getting-started/Forecasting_BikeSharingDemand/BikeDemandForecasting/Program.cs#L54)]

1. <span data-ttu-id="b6342-236">运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="b6342-236">Run the application.</span></span> <span data-ttu-id="b6342-237">控制台应显示类似以下内容的输出。</span><span class="sxs-lookup"><span data-stu-id="b6342-237">Output similar to that below should appear on the console.</span></span> <span data-ttu-id="b6342-238">为简洁起见，输出已进行压缩。</span><span class="sxs-lookup"><span data-stu-id="b6342-238">For brevity, the output has been condensed.</span></span>

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

<span data-ttu-id="b6342-239">通过观测实际值和预测值，获得以下关系：</span><span class="sxs-lookup"><span data-stu-id="b6342-239">Inspection of the actual and forecasted values shows the following relationships:</span></span>

![实际值和预测值比较](./media/time-series-demand-forecasting/forecast.png)

<span data-ttu-id="b6342-241">尽管预测值并不能预测准确的租赁数，但它们缩小了值的范围，企业可以通过它们优化资源利用。</span><span class="sxs-lookup"><span data-stu-id="b6342-241">While the forecasted values are not predicting the exact number of rentals, they provide a more narrow range of values that allows an operation to optimize their use of resources.</span></span>

<span data-ttu-id="b6342-242">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="b6342-242">Congratulations!</span></span> <span data-ttu-id="b6342-243">你已成功生成用于预测自行车租赁需求的时序机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="b6342-243">You've now successfully built a time series machine learning model to forecast bike rental demand.</span></span>

<span data-ttu-id="b6342-244">可以在 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="b6342-244">You can find the source code for this tutorial at the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/Forecasting_BikeSharingDemand) repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b6342-245">后续步骤</span><span class="sxs-lookup"><span data-stu-id="b6342-245">Next steps</span></span>

- [<span data-ttu-id="b6342-246">ML.NET 中的机器学习任务</span><span class="sxs-lookup"><span data-stu-id="b6342-246">Machine learning tasks in ML.NET</span></span>](../resources/tasks.md)
- [<span data-ttu-id="b6342-247">提高模型准确性</span><span class="sxs-lookup"><span data-stu-id="b6342-247">Improve model accuracy</span></span>](../resources/improve-machine-learning-model-ml-net.md)
