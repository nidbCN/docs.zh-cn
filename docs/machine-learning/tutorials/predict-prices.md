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
# <a name="tutorial-predict-prices-using-regression-with-mlnet"></a><span data-ttu-id="44dd3-103">教程：将回归与 ML.NET 配合使用以预测价格</span><span class="sxs-lookup"><span data-stu-id="44dd3-103">Tutorial: Predict prices using regression with ML.NET</span></span>

<span data-ttu-id="44dd3-104">本教程演示如何使用 ML.NET 生成[回归模型](../resources/glossary.md#regression)来预测价格，特别是纽约市的出租车费。</span><span class="sxs-lookup"><span data-stu-id="44dd3-104">This tutorial illustrates how to build a [regression model](../resources/glossary.md#regression) using ML.NET to predict prices, specifically, New York City taxi fares.</span></span>

<span data-ttu-id="44dd3-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="44dd3-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="44dd3-106">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="44dd3-106">Prepare and understand the data</span></span>
> * <span data-ttu-id="44dd3-107">加载和转换数据</span><span class="sxs-lookup"><span data-stu-id="44dd3-107">Load and transform the data</span></span>
> * <span data-ttu-id="44dd3-108">选择学习算法</span><span class="sxs-lookup"><span data-stu-id="44dd3-108">Choose a learning algorithm</span></span>
> * <span data-ttu-id="44dd3-109">定型模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-109">Train the model</span></span>
> * <span data-ttu-id="44dd3-110">评估模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-110">Evaluate the model</span></span>
> * <span data-ttu-id="44dd3-111">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-111">Use the model for predictions</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44dd3-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="44dd3-112">Prerequisites</span></span>

* <span data-ttu-id="44dd3-113">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="44dd3-113">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="44dd3-114">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="44dd3-114">Create a console application</span></span>

1. <span data-ttu-id="44dd3-115">创建名为“TaxiFarePrediction”的“.NET Core 控制台应用程序”。</span><span class="sxs-lookup"><span data-stu-id="44dd3-115">Create a **.NET Core Console Application** called "TaxiFarePrediction".</span></span>

1. <span data-ttu-id="44dd3-116">在项目中创建一个名为“数据”的目录来保存数据集和模型文件。</span><span class="sxs-lookup"><span data-stu-id="44dd3-116">Create a directory named *Data* in your project to store the data set and model files.</span></span>

1. <span data-ttu-id="44dd3-117">安装“Microsoft.ML”和“Microsoft.ML.FastTree”NuGet 包 ：</span><span class="sxs-lookup"><span data-stu-id="44dd3-117">Install the **Microsoft.ML** and **Microsoft.ML.FastTree** NuGet Package:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="44dd3-118">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包” 。</span><span class="sxs-lookup"><span data-stu-id="44dd3-118">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="44dd3-119">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，在列表中选择包，再选择“安装”按钮  。</span><span class="sxs-lookup"><span data-stu-id="44dd3-119">Choose "nuget.org" as the Package source, select the **Browse** tab, search for **Microsoft.ML**, select the package in the list, and select the **Install** button.</span></span> <span data-ttu-id="44dd3-120">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="44dd3-120">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span> <span data-ttu-id="44dd3-121">对 **Microsoft.ML.FastTree** NuGet 包执行相同操作。</span><span class="sxs-lookup"><span data-stu-id="44dd3-121">Do the same for the **Microsoft.ML.FastTree** NuGet package.</span></span>

## <a name="prepare-and-understand-the-data"></a><span data-ttu-id="44dd3-122">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="44dd3-122">Prepare and understand the data</span></span>

1. <span data-ttu-id="44dd3-123">下载 [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/main/test/data/taxi-fare-train.csv) 和 [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/main/test/data/taxi-fare-test.csv) 数据集，并将它们保存到先前创建的“数据”文件夹。</span><span class="sxs-lookup"><span data-stu-id="44dd3-123">Download the [taxi-fare-train.csv](https://github.com/dotnet/machinelearning/blob/main/test/data/taxi-fare-train.csv) and the [taxi-fare-test.csv](https://github.com/dotnet/machinelearning/blob/main/test/data/taxi-fare-test.csv) data sets and save them to the *Data* folder you've created at the previous step.</span></span> <span data-ttu-id="44dd3-124">我们使用这些数据集定型机器学习模型，然后评估模型的准确性。</span><span class="sxs-lookup"><span data-stu-id="44dd3-124">We use these data sets to train the machine learning model and then evaluate how accurate the model is.</span></span> <span data-ttu-id="44dd3-125">这些数据集最初来自 [NYC TLC 出租车行程数据集](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page)。</span><span class="sxs-lookup"><span data-stu-id="44dd3-125">These data sets are originally from the [NYC TLC Taxi Trip data set](https://www1.nyc.gov/site/tlc/about/tlc-trip-record-data.page).</span></span>

1. <span data-ttu-id="44dd3-126">在“解决方案资源管理器”中，右键单击每个 \*.csv 文件，然后选择“属性” 。</span><span class="sxs-lookup"><span data-stu-id="44dd3-126">In **Solution Explorer**, right-click each of the \*.csv files and select **Properties**.</span></span> <span data-ttu-id="44dd3-127">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。</span><span class="sxs-lookup"><span data-stu-id="44dd3-127">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

1. <span data-ttu-id="44dd3-128">打开“taxi-fare-train.csv”数据集并查看第一行中的列标题。</span><span class="sxs-lookup"><span data-stu-id="44dd3-128">Open the **taxi-fare-train.csv** data set and look at column headers in the first row.</span></span> <span data-ttu-id="44dd3-129">查看每个列。</span><span class="sxs-lookup"><span data-stu-id="44dd3-129">Take a look at each of the columns.</span></span> <span data-ttu-id="44dd3-130">了解数据并确定哪些列是“特征”以及哪些是“标签” 。</span><span class="sxs-lookup"><span data-stu-id="44dd3-130">Understand the data and decide which columns are **features** and which one is the **label**.</span></span>

<span data-ttu-id="44dd3-131">`label` 是要预测的列。</span><span class="sxs-lookup"><span data-stu-id="44dd3-131">The `label` is the column you want to predict.</span></span> <span data-ttu-id="44dd3-132">标识的 `Features` 是为模型提供的用来预测 `Label` 的输入。</span><span class="sxs-lookup"><span data-stu-id="44dd3-132">The identified `Features`are the inputs you give the model to predict the `Label`.</span></span>

<span data-ttu-id="44dd3-133">提供的数据集包含以下列：</span><span class="sxs-lookup"><span data-stu-id="44dd3-133">The provided data set contains the following columns:</span></span>

* <span data-ttu-id="44dd3-134">**vendor_id：** 出租车供应商的 ID 是一项特征。</span><span class="sxs-lookup"><span data-stu-id="44dd3-134">**vendor_id:** The ID of the taxi vendor is a feature.</span></span>
* <span data-ttu-id="44dd3-135">**rate_code：** 出租车行程的费率类型是一项特征。</span><span class="sxs-lookup"><span data-stu-id="44dd3-135">**rate_code:** The rate type of the taxi trip is a feature.</span></span>
* <span data-ttu-id="44dd3-136">**passenger_count：** 行程中的乘客人数是一项特征。</span><span class="sxs-lookup"><span data-stu-id="44dd3-136">**passenger_count:** The number of passengers on the trip is a feature.</span></span>
* <span data-ttu-id="44dd3-137">**trip_time_in_secs：** 这次行程所花的时间。</span><span class="sxs-lookup"><span data-stu-id="44dd3-137">**trip_time_in_secs:** The amount of time the trip took.</span></span> <span data-ttu-id="44dd3-138">希望在行程完成前预测行程费用。</span><span class="sxs-lookup"><span data-stu-id="44dd3-138">You want to predict the fare of the trip before the trip is completed.</span></span> <span data-ttu-id="44dd3-139">当时并不知道行程有多长。</span><span class="sxs-lookup"><span data-stu-id="44dd3-139">At that moment, you don't know how long the trip would take.</span></span> <span data-ttu-id="44dd3-140">因此，行程时间不是一项特征，需要从模型删除此列。</span><span class="sxs-lookup"><span data-stu-id="44dd3-140">Thus, the trip time is not a feature and you'll exclude this column from the model.</span></span>
* <span data-ttu-id="44dd3-141">**trip_distance：** 行程距离是一项特征。</span><span class="sxs-lookup"><span data-stu-id="44dd3-141">**trip_distance:** The distance of the trip is a feature.</span></span>
* <span data-ttu-id="44dd3-142">**payment_type：** 付款方式（现金或信用卡）是一项特征。</span><span class="sxs-lookup"><span data-stu-id="44dd3-142">**payment_type:** The payment method (cash or credit card) is a feature.</span></span>
* <span data-ttu-id="44dd3-143">**fare_amount：** 支付的总出租车费用是一个标签。</span><span class="sxs-lookup"><span data-stu-id="44dd3-143">**fare_amount:** The total taxi fare paid is the label.</span></span>

## <a name="create-data-classes"></a><span data-ttu-id="44dd3-144">创建数据类</span><span class="sxs-lookup"><span data-stu-id="44dd3-144">Create data classes</span></span>

<span data-ttu-id="44dd3-145">创建输入数据和预测类：</span><span class="sxs-lookup"><span data-stu-id="44dd3-145">Create classes for the input data and the predictions:</span></span>

1. <span data-ttu-id="44dd3-146">在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。</span><span class="sxs-lookup"><span data-stu-id="44dd3-146">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>
1. <span data-ttu-id="44dd3-147">在“添加新项”对话框中，选择“类”并将“名称”字段更改为“TaxiTrip.cs”。</span><span class="sxs-lookup"><span data-stu-id="44dd3-147">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *TaxiTrip.cs*.</span></span> <span data-ttu-id="44dd3-148">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="44dd3-148">Then, select the **Add** button.</span></span>
1. <span data-ttu-id="44dd3-149">将以下 `using` 指令添加到新文件：</span><span class="sxs-lookup"><span data-stu-id="44dd3-149">Add the following `using` directives to the new file:</span></span>

   [!code-csharp[AddUsings](./snippets/predict-prices/csharp/TaxiTrip.cs#1 "Add necessary usings")]

<span data-ttu-id="44dd3-150">删除现有类定义并向“TaxiTrip.cs”文件添加以下代码，其中有两个类 `TaxiTrip` 和 `TaxiTripFarePrediction`：</span><span class="sxs-lookup"><span data-stu-id="44dd3-150">Remove the existing class definition and add the following code, which has two classes `TaxiTrip` and `TaxiTripFarePrediction`, to the *TaxiTrip.cs* file:</span></span>

[!code-csharp[DefineTaxiTrip](./snippets/predict-prices/csharp/TaxiTrip.cs#2 "Define the taxi trip and fare predictions classes")]

<span data-ttu-id="44dd3-151">`TaxiTrip` 是输入数据类且具有针对每个数据集列的定义。</span><span class="sxs-lookup"><span data-stu-id="44dd3-151">`TaxiTrip` is the input data class and has definitions for each of the data set columns.</span></span> <span data-ttu-id="44dd3-152">使用 <xref:Microsoft.ML.Data.LoadColumnAttribute> 属性在数据集中指定源列的索引。</span><span class="sxs-lookup"><span data-stu-id="44dd3-152">Use the <xref:Microsoft.ML.Data.LoadColumnAttribute> attribute to specify the indices of the source columns in the data set.</span></span>

<span data-ttu-id="44dd3-153">`TaxiTripFarePrediction` 类表示预测的结果。</span><span class="sxs-lookup"><span data-stu-id="44dd3-153">The `TaxiTripFarePrediction` class represents predicted results.</span></span> <span data-ttu-id="44dd3-154">它应用了单个浮动 `FareAmount` 字段，附带 `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> 属性。</span><span class="sxs-lookup"><span data-stu-id="44dd3-154">It has a single float field, `FareAmount`, with a `Score` <xref:Microsoft.ML.Data.ColumnNameAttribute> attribute applied.</span></span> <span data-ttu-id="44dd3-155">对于回归任务，“分数”列包含预测的标签值。</span><span class="sxs-lookup"><span data-stu-id="44dd3-155">In case of the regression task, the **Score** column contains predicted label values.</span></span>

> [!NOTE]
> <span data-ttu-id="44dd3-156">使用 `float` 类型来表示输入和预测数据类中的浮点值。</span><span class="sxs-lookup"><span data-stu-id="44dd3-156">Use the `float` type to represent floating-point values in the input and prediction data classes.</span></span>

### <a name="define-data-and-model-paths"></a><span data-ttu-id="44dd3-157">定义数据和模型路径</span><span class="sxs-lookup"><span data-stu-id="44dd3-157">Define data and model paths</span></span>

<span data-ttu-id="44dd3-158">将以下附加的 `using` 语句添加到“Program.cs”文件顶部：</span><span class="sxs-lookup"><span data-stu-id="44dd3-158">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

[!code-csharp[AddUsings](./snippets/predict-prices/csharp/Program.cs#1 "Add necessary usings")]

<span data-ttu-id="44dd3-159">需要创建三个字段，用于保留有数据集的文件的路径，以及用于保存模型的文件的路径：</span><span class="sxs-lookup"><span data-stu-id="44dd3-159">You need to create three fields to hold the paths to the files with data sets and the file to save the model:</span></span>

* <span data-ttu-id="44dd3-160">`_trainDataPath` 包含具有用于定型模型的数据集的文件的路径。</span><span class="sxs-lookup"><span data-stu-id="44dd3-160">`_trainDataPath` contains the path to the file with the data set used to train the model.</span></span>
* <span data-ttu-id="44dd3-161">`_testDataPath` 包含具有用于评估模型的数据集的文件的路径。</span><span class="sxs-lookup"><span data-stu-id="44dd3-161">`_testDataPath` contains the path to the file with the data set used to evaluate the model.</span></span>
* <span data-ttu-id="44dd3-162">`_modelPath` 包含用于存储定型模型的文件的路径。</span><span class="sxs-lookup"><span data-stu-id="44dd3-162">`_modelPath` contains the path to the file where the trained model is stored.</span></span>

<span data-ttu-id="44dd3-163">将以下代码添加到 `Main` 方法正上方，以指定这些路径和 `_textLoader` 变量：</span><span class="sxs-lookup"><span data-stu-id="44dd3-163">Add the following code right above the `Main` method to specify those paths and for the `_textLoader` variable:</span></span>

[!code-csharp[InitializePaths](./snippets/predict-prices/csharp/Program.cs#2 "Define variables to store the data file paths")]

<span data-ttu-id="44dd3-164">所有 ML.NET 操作都从 [MLContext 类](xref:Microsoft.ML.MLContext)开始。</span><span class="sxs-lookup"><span data-stu-id="44dd3-164">All ML.NET operations start in the [MLContext class](xref:Microsoft.ML.MLContext).</span></span> <span data-ttu-id="44dd3-165">初始化 `mlContext` 创建了新的 ML.NET 环境，可以在模型创建工作流对象之间共享。</span><span class="sxs-lookup"><span data-stu-id="44dd3-165">Initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="44dd3-166">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="44dd3-166">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="44dd3-167">在 Main 中初始化变量</span><span class="sxs-lookup"><span data-stu-id="44dd3-167">Initialize variables in Main</span></span>

<span data-ttu-id="44dd3-168">使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 `mlContext` 变量：</span><span class="sxs-lookup"><span data-stu-id="44dd3-168">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the `mlContext` variable:</span></span>

[!code-csharp[CreateMLContext](./snippets/predict-prices/csharp/Program.cs#3 "Create the ML Context")]

<span data-ttu-id="44dd3-169">在 `Main` 方法中添加以下代码作为调用 `Train` 方法的下一行代码：</span><span class="sxs-lookup"><span data-stu-id="44dd3-169">Add the following as the next line of code in the `Main` method to call the `Train` method:</span></span>

[!code-csharp[Train](./snippets/predict-prices/csharp/Program.cs#5 "Train your model")]

<span data-ttu-id="44dd3-170">`Train()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="44dd3-170">The `Train()` method executes the following tasks:</span></span>

* <span data-ttu-id="44dd3-171">加载数据。</span><span class="sxs-lookup"><span data-stu-id="44dd3-171">Loads the data.</span></span>
* <span data-ttu-id="44dd3-172">提取并转换数据。</span><span class="sxs-lookup"><span data-stu-id="44dd3-172">Extracts and transforms the data.</span></span>
* <span data-ttu-id="44dd3-173">定型模型。</span><span class="sxs-lookup"><span data-stu-id="44dd3-173">Trains the model.</span></span>
* <span data-ttu-id="44dd3-174">返回模型。</span><span class="sxs-lookup"><span data-stu-id="44dd3-174">Returns the model.</span></span>

<span data-ttu-id="44dd3-175">`Train` 方法定型模型。</span><span class="sxs-lookup"><span data-stu-id="44dd3-175">The `Train` method trains the model.</span></span> <span data-ttu-id="44dd3-176">使用以下代码在 `Main` 下创建该方法：</span><span class="sxs-lookup"><span data-stu-id="44dd3-176">Create that method just below `Main`, using the following code:</span></span>

```csharp
public static ITransformer Train(MLContext mlContext, string dataPath)
{

}
```

## <a name="load-and-transform-data"></a><span data-ttu-id="44dd3-177">加载和转换数据</span><span class="sxs-lookup"><span data-stu-id="44dd3-177">Load and transform data</span></span>

<span data-ttu-id="44dd3-178">ML.NET 使用 [IDataView 类](xref:Microsoft.ML.IDataView)灵活、有效地描述数字或文本表格数据。</span><span class="sxs-lookup"><span data-stu-id="44dd3-178">ML.NET uses the [IDataView class](xref:Microsoft.ML.IDataView) as a flexible, efficient way of describing numeric or text tabular data.</span></span> <span data-ttu-id="44dd3-179">`IDataView` 可以加载文本文件或进行实时加载（例如，SQL 数据库或日志文件）。</span><span class="sxs-lookup"><span data-stu-id="44dd3-179">`IDataView` can load either text files or in real time (for example, SQL database or log files).</span></span> <span data-ttu-id="44dd3-180">将以下代码添加为 `Train()` 方法的首行：</span><span class="sxs-lookup"><span data-stu-id="44dd3-180">Add the following code as the first line of the `Train()` method:</span></span>

[!code-csharp[LoadTrainData](./snippets/predict-prices/csharp/Program.cs#6 "loading training dataset")]

<span data-ttu-id="44dd3-181">由于要预测出租车车费，`FareAmount` 列是将预测的 `Label`（模型的输出）。</span><span class="sxs-lookup"><span data-stu-id="44dd3-181">As you want to predict the taxi trip fare, the `FareAmount` column is the `Label` that you will predict (the output of the model).</span></span> <span data-ttu-id="44dd3-182">使用 `CopyColumnsEstimator` 转换类以复制 `FareAmount`，并添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="44dd3-182">Use the `CopyColumnsEstimator` transformation class to copy `FareAmount`, and add the following code:</span></span>

[!code-csharp[CopyColumnsEstimator](./snippets/predict-prices/csharp/Program.cs#7 "Use the CopyColumnsEstimator")]

<span data-ttu-id="44dd3-183">定型模型的算法需要数字特性，所以必须将分类数据（`VendorId`、`RateCode` 和 `PaymentType`）值转换为数字（`VendorIdEncoded`、`RateCodeEncoded` 和 `PaymentTypeEncoded`）。</span><span class="sxs-lookup"><span data-stu-id="44dd3-183">The algorithm that trains the model requires **numeric** features, so you have to transform the categorical data (`VendorId`, `RateCode`, and `PaymentType`) values into numbers (`VendorIdEncoded`, `RateCodeEncoded`, and `PaymentTypeEncoded`).</span></span> <span data-ttu-id="44dd3-184">为此，请使用 [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer) 转换类（它将不同的数字键值分配到每列的不同值），并添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="44dd3-184">To do that, use the [OneHotEncodingTransformer](xref:Microsoft.ML.Transforms.OneHotEncodingTransformer) transformation class, which assigns different numeric key values to the different values in each of the columns, and add the following code:</span></span>

[!code-csharp[OneHotEncodingEstimator](./snippets/predict-prices/csharp/Program.cs#8 "Use the OneHotEncodingEstimator")]

<span data-ttu-id="44dd3-185">数据准备最后一步使用 `mlContext.Transforms.Concatenate` 转换类将所有功能列合并到“功能”列。</span><span class="sxs-lookup"><span data-stu-id="44dd3-185">The last step in data preparation combines all of the feature columns into the **Features** column using the `mlContext.Transforms.Concatenate` transformation class.</span></span> <span data-ttu-id="44dd3-186">默认情况下，学习算法仅处理“特征”列的特征。</span><span class="sxs-lookup"><span data-stu-id="44dd3-186">By default, a learning algorithm processes only features from the **Features** column.</span></span> <span data-ttu-id="44dd3-187">添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="44dd3-187">Add the following code:</span></span>

[!code-csharp[ColumnConcatenatingEstimator](./snippets/predict-prices/csharp/Program.cs#9 "Use the ColumnConcatenatingEstimator")]

## <a name="choose-a-learning-algorithm"></a><span data-ttu-id="44dd3-188">选择学习算法</span><span class="sxs-lookup"><span data-stu-id="44dd3-188">Choose a learning algorithm</span></span>

<span data-ttu-id="44dd3-189">此问题关于预测纽约市的出租车车费。</span><span class="sxs-lookup"><span data-stu-id="44dd3-189">This problem is about predicting a taxi trip fare in New York City.</span></span> <span data-ttu-id="44dd3-190">从表面看，似乎只取决于行程距离。</span><span class="sxs-lookup"><span data-stu-id="44dd3-190">At first glance, it may seem to depend simply on the distance traveled.</span></span> <span data-ttu-id="44dd3-191">但是，由于其他因素（比如额外的乘客或使用信用卡而非现金付款），纽约的出租车供应商收费不同。</span><span class="sxs-lookup"><span data-stu-id="44dd3-191">However, taxi vendors in New York charge varying amounts for other factors such as additional passengers or paying with a credit card instead of cash.</span></span> <span data-ttu-id="44dd3-192">你想要预测价格值，该值是基于数据集中的其他因素的实际值。</span><span class="sxs-lookup"><span data-stu-id="44dd3-192">You want to predict the price value, which is a real value, based on the other factors in the dataset.</span></span> <span data-ttu-id="44dd3-193">要执行此操作，请选择[回归](../resources/glossary.md#regression)机器学习任务。</span><span class="sxs-lookup"><span data-stu-id="44dd3-193">To do that, you choose a [regression](../resources/glossary.md#regression) machine learning task.</span></span>

<span data-ttu-id="44dd3-194">将 [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) 机器学习任务追加到数据转换定义中，方法是在 `Train()` 中添加以下代码作为下一行代码：</span><span class="sxs-lookup"><span data-stu-id="44dd3-194">Append the [FastTreeRegressionTrainer](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer) machine learning task to the data transformation definitions by adding the following as the next line of code in `Train()`:</span></span>

[!code-csharp[FastTreeRegressionTrainer](./snippets/predict-prices/csharp/Program.cs#10 "Add the FastTreeRegressionTrainer")]

## <a name="train-the-model"></a><span data-ttu-id="44dd3-195">定型模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-195">Train the model</span></span>

<span data-ttu-id="44dd3-196">使模型适合训练 `dataview` 并通过在 `Train()` 方法中添加以下代码行来返回训练的模型：</span><span class="sxs-lookup"><span data-stu-id="44dd3-196">Fit the model to the training `dataview` and return the trained model by adding the following line of code in the `Train()` method:</span></span>

[!code-csharp[TrainModel](./snippets/predict-prices/csharp/Program.cs#11 "Train the model")]

<span data-ttu-id="44dd3-197">[Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) 方法通过转换数据集并应用训练来训练模型。</span><span class="sxs-lookup"><span data-stu-id="44dd3-197">The [Fit()](xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) method trains your model by transforming the dataset and applying the training.</span></span>

<span data-ttu-id="44dd3-198">使用 `Train()` 方法中的以下代码行返回训练的模型：</span><span class="sxs-lookup"><span data-stu-id="44dd3-198">Return the trained model with the following line of code in the `Train()` method:</span></span>

[!code-csharp[ReturnModel](./snippets/predict-prices/csharp/Program.cs#12 "Return the model")]

## <a name="evaluate-the-model"></a><span data-ttu-id="44dd3-199">评估模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-199">Evaluate the model</span></span>

<span data-ttu-id="44dd3-200">接下来，使用测试数据评估模型性能，以确保质量和验证。</span><span class="sxs-lookup"><span data-stu-id="44dd3-200">Next, evaluate your model performance with your test data for quality assurance and validation.</span></span> <span data-ttu-id="44dd3-201">使用以下代码在 `Train()` 之后创建 `Evaluate()` 方法：</span><span class="sxs-lookup"><span data-stu-id="44dd3-201">Create the `Evaluate()` method, just after `Train()`, with the following code:</span></span>

```csharp
private static void Evaluate(MLContext mlContext, ITransformer model)
{

}
```

<span data-ttu-id="44dd3-202">`Evaluate` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="44dd3-202">The `Evaluate` method executes the following tasks:</span></span>

* <span data-ttu-id="44dd3-203">加载测试数据集。</span><span class="sxs-lookup"><span data-stu-id="44dd3-203">Loads the test dataset.</span></span>
* <span data-ttu-id="44dd3-204">创建回归计算器。</span><span class="sxs-lookup"><span data-stu-id="44dd3-204">Creates the regression evaluator.</span></span>
* <span data-ttu-id="44dd3-205">评估模型并创建指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-205">Evaluates the model and creates metrics.</span></span>
* <span data-ttu-id="44dd3-206">显示指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-206">Displays the metrics.</span></span>

<span data-ttu-id="44dd3-207">使用下面的代码，在 `Train` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="44dd3-207">Add a call to the new method from the `Main` method, right under the `Train` method call, using the following code:</span></span>

[!code-csharp[CallEvaluate](./snippets/predict-prices/csharp/Program.cs#14 "Call the Evaluate method")]

<span data-ttu-id="44dd3-208">使用 [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) 方法加载测试数据集。</span><span class="sxs-lookup"><span data-stu-id="44dd3-208">Load the test dataset using the [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) method.</span></span> <span data-ttu-id="44dd3-209">使用此数据集作为质量检查评估模型，方法是在 `Evaluate` 方法中添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="44dd3-209">Evaluate the model using this dataset as a quality check by adding the following code in the `Evaluate` method:</span></span>

[!code-csharp[LoadTestDataset](./snippets/predict-prices/csharp/Program.cs#15 "Load the test dataset")]

<span data-ttu-id="44dd3-210">接下来，通过将以下代码添加到 `Evaluate()` 来转换 `Test` 数据：</span><span class="sxs-lookup"><span data-stu-id="44dd3-210">Next, transform the `Test` data by adding the following code to `Evaluate()`:</span></span>

[!code-csharp[PredictWithTransformer](./snippets/predict-prices/csharp/Program.cs#16 "Predict using the Transformer")]

<span data-ttu-id="44dd3-211">[Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对测试数据集输入行进行预测。</span><span class="sxs-lookup"><span data-stu-id="44dd3-211">The [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method makes predictions for the test dataset input rows.</span></span>

<span data-ttu-id="44dd3-212">`RegressionContext.Evaluate` 方法使用指定数据集计算 `PredictionModel` 的质量指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-212">The `RegressionContext.Evaluate` method computes the quality metrics for the `PredictionModel` using the specified dataset.</span></span> <span data-ttu-id="44dd3-213">它返回一个 <xref:Microsoft.ML.Data.RegressionMetrics> 对象，其中包含由回归计算器计算出的总体指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-213">It returns a <xref:Microsoft.ML.Data.RegressionMetrics> object that contains the overall metrics computed by regression evaluators.</span></span>

<span data-ttu-id="44dd3-214">若要显示这些指标来确定模型质量，需要先获取指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-214">To display these to determine the quality of the model, you need to get the metrics first.</span></span> <span data-ttu-id="44dd3-215">将以下代码作为下一行添加到 `Evaluate` 方法中：</span><span class="sxs-lookup"><span data-stu-id="44dd3-215">Add the following code as the next line in the `Evaluate` method:</span></span>

[!code-csharp[ComputeMetrics](./snippets/predict-prices/csharp/Program.cs#17 "Compute Metrics")]

<span data-ttu-id="44dd3-216">获得预测集后，[Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) 方法会对模型进行评估，该模型会将预测值与测试数据集中的实际 `Labels` 进行比较，并返回有关模型执行情况的指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-216">Once you have the prediction set, the [Evaluate()](xref:Microsoft.ML.RegressionCatalog.Evaluate%2A) method assesses the model, which compares the predicted values with the actual `Labels` in the test dataset and returns metrics on how the model is performing.</span></span>

<span data-ttu-id="44dd3-217">添加以下代码以评估模型并生成评估模型：</span><span class="sxs-lookup"><span data-stu-id="44dd3-217">Add the following code to evaluate the model and produce the evaluation metrics:</span></span>

```csharp
Console.WriteLine();
Console.WriteLine($"*************************************************");
Console.WriteLine($"*       Model quality metrics evaluation         ");
Console.WriteLine($"*------------------------------------------------");
```

<span data-ttu-id="44dd3-218">[RSquared](../resources/glossary.md#coefficient-of-determination) 是回归模型的另一种评估指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-218">[RSquared](../resources/glossary.md#coefficient-of-determination) is another evaluation metric of the regression models.</span></span> <span data-ttu-id="44dd3-219">RSquared 在 0 和 1 之间取值。</span><span class="sxs-lookup"><span data-stu-id="44dd3-219">RSquared takes values between 0 and 1.</span></span> <span data-ttu-id="44dd3-220">值越接近 1，模型就越好。</span><span class="sxs-lookup"><span data-stu-id="44dd3-220">The closer its value is to 1, the better the model is.</span></span> <span data-ttu-id="44dd3-221">将以下代码添加到 `Evaluate` 方法以显示 RSquared 值：</span><span class="sxs-lookup"><span data-stu-id="44dd3-221">Add the following code into the `Evaluate` method to display the RSquared value:</span></span>

[!code-csharp[DisplayRSquared](./snippets/predict-prices/csharp/Program.cs#18 "Display the RSquared metric.")]

<span data-ttu-id="44dd3-222">[RMS](../resources/glossary.md#root-of-mean-squared-error-rmse) 是回归模型的一种评估指标。</span><span class="sxs-lookup"><span data-stu-id="44dd3-222">[RMS](../resources/glossary.md#root-of-mean-squared-error-rmse) is one of the evaluation metrics of the regression model.</span></span> <span data-ttu-id="44dd3-223">指标越低，模型就越好。</span><span class="sxs-lookup"><span data-stu-id="44dd3-223">The lower it is, the better the model is.</span></span> <span data-ttu-id="44dd3-224">将以下代码添加到 `Evaluate` 方法以显示 RMS 值：</span><span class="sxs-lookup"><span data-stu-id="44dd3-224">Add the following code into the `Evaluate` method to display the RMS value:</span></span>

[!code-csharp[DisplayRMS](./snippets/predict-prices/csharp/Program.cs#19 "Display the RMS metric.")]

## <a name="use-the-model-for-predictions"></a><span data-ttu-id="44dd3-225">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-225">Use the model for predictions</span></span>

<span data-ttu-id="44dd3-226">使用下面的代码紧随 `Evaluate` 方法后创建 `TestSinglePrediction` 方法：</span><span class="sxs-lookup"><span data-stu-id="44dd3-226">Create the `TestSinglePrediction` method, just after the `Evaluate` method, using the following code:</span></span>

```csharp
private static void TestSinglePrediction(MLContext mlContext, ITransformer model)
{

}
```

<span data-ttu-id="44dd3-227">`TestSinglePrediction` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="44dd3-227">The `TestSinglePrediction` method executes the following tasks:</span></span>

* <span data-ttu-id="44dd3-228">创建测试数据的单个注释。</span><span class="sxs-lookup"><span data-stu-id="44dd3-228">Creates a single comment of test data.</span></span>
* <span data-ttu-id="44dd3-229">根据测试数据预测费用。</span><span class="sxs-lookup"><span data-stu-id="44dd3-229">Predicts fare amount based on test data.</span></span>
* <span data-ttu-id="44dd3-230">结合测试数据和预测进行报告。</span><span class="sxs-lookup"><span data-stu-id="44dd3-230">Combines test data and predictions for reporting.</span></span>
* <span data-ttu-id="44dd3-231">显示预测结果。</span><span class="sxs-lookup"><span data-stu-id="44dd3-231">Displays the predicted results.</span></span>

<span data-ttu-id="44dd3-232">使用下面的代码，在 `Evaluate` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="44dd3-232">Add a call to the new method from the `Main` method, right under the `Evaluate` method call, using the following code:</span></span>

[!code-csharp[CallTestSinglePrediction](./snippets/predict-prices/csharp/Program.cs#20 "Call the TestSinglePrediction method")]

<span data-ttu-id="44dd3-233">使用 `PredictionEngine` 通过将以下代码添加到 `TestSinglePrediction()` 来预测车费：</span><span class="sxs-lookup"><span data-stu-id="44dd3-233">Use the `PredictionEngine` to predict the fare by adding the following code to `TestSinglePrediction()`:</span></span>

[!code-csharp[MakePredictionEngine](./snippets/predict-prices/csharp/Program.cs#22 "Create the PredictionFunction")]

<span data-ttu-id="44dd3-234">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="44dd3-234">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to perform a prediction on a single instance of data.</span></span> <span data-ttu-id="44dd3-235">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="44dd3-235">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="44dd3-236">可以在单线程环境或原型环境中使用。</span><span class="sxs-lookup"><span data-stu-id="44dd3-236">It's acceptable to use in single-threaded or prototype environments.</span></span> <span data-ttu-id="44dd3-237">为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="44dd3-237">For improved performance and thread safety in production environments, use the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span> <span data-ttu-id="44dd3-238">请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。</span><span class="sxs-lookup"><span data-stu-id="44dd3-238">See this guide on how to [use `PredictionEnginePool` in an ASP.NET Core Web API](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).</span></span>

> [!NOTE]
> <span data-ttu-id="44dd3-239">`PredictionEnginePool` 服务扩展目前处于预览状态。</span><span class="sxs-lookup"><span data-stu-id="44dd3-239">`PredictionEnginePool` service extension is currently in preview.</span></span>

<span data-ttu-id="44dd3-240">本教程使用此类中的一个测试行程。</span><span class="sxs-lookup"><span data-stu-id="44dd3-240">This tutorial uses one test trip within this class.</span></span> <span data-ttu-id="44dd3-241">稍后可以添加其他方案，以尝试使用此模型。</span><span class="sxs-lookup"><span data-stu-id="44dd3-241">Later you can add other scenarios to experiment with the model.</span></span> <span data-ttu-id="44dd3-242">通过创建一个 `TaxiTrip` 实例，在 `TestSinglePrediction()` 方法中添加一个行程来测试定型模型的成本预测：</span><span class="sxs-lookup"><span data-stu-id="44dd3-242">Add a trip to test the trained model's prediction of cost in the `TestSinglePrediction()` method by creating an instance of `TaxiTrip`:</span></span>

[!code-csharp[PredictionData](./snippets/predict-prices/csharp/Program.cs#23 "Create test data for single prediction")]

<span data-ttu-id="44dd3-243">接下来，根据出租车行程数据的单个实例预测车费，并通过在 `TestSinglePrediction()` 方法中添加以下代码作为下一行代码将其传递给 `PredictionEngine`：</span><span class="sxs-lookup"><span data-stu-id="44dd3-243">Next, predict the fare based on a single instance of the taxi trip data and pass it to the `PredictionEngine` by adding the following as the next lines of code in the `TestSinglePrediction()` method:</span></span>

[!code-csharp[Predict](./snippets/predict-prices/csharp/Program.cs#24 "Create a prediction of taxi fare")]

<span data-ttu-id="44dd3-244">[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单个数据实例进行预测。</span><span class="sxs-lookup"><span data-stu-id="44dd3-244">The [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) function makes a prediction on a single instance of data.</span></span>

<span data-ttu-id="44dd3-245">若要显示指定行程的预测费用，请将下面的代码添加到 `TestSinglePrediction` 方法中：</span><span class="sxs-lookup"><span data-stu-id="44dd3-245">To display the predicted fare of the specified trip, add the following code into the `TestSinglePrediction` method:</span></span>

[!code-csharp[Predict](./snippets/predict-prices/csharp/Program.cs#25 "Display the prediction.")]

<span data-ttu-id="44dd3-246">运行此程序，查看测试用例的预测出租车费。</span><span class="sxs-lookup"><span data-stu-id="44dd3-246">Run the program to see the predicted taxi fare for your test case.</span></span>

<span data-ttu-id="44dd3-247">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="44dd3-247">Congratulations!</span></span> <span data-ttu-id="44dd3-248">你已成功生成用于预测出租车费的机器学习模型、评估其准确性，并用其进行预测。</span><span class="sxs-lookup"><span data-stu-id="44dd3-248">You've now successfully built a machine learning model for predicting taxi trip fares, evaluated its accuracy, and used it to make predictions.</span></span> <span data-ttu-id="44dd3-249">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TaxiFarePrediction) GitHub 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="44dd3-249">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TaxiFarePrediction) GitHub repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="44dd3-250">后续步骤</span><span class="sxs-lookup"><span data-stu-id="44dd3-250">Next steps</span></span>

<span data-ttu-id="44dd3-251">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="44dd3-251">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="44dd3-252">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="44dd3-252">Prepare and understand the data</span></span>
> * <span data-ttu-id="44dd3-253">创建学习管道</span><span class="sxs-lookup"><span data-stu-id="44dd3-253">Create a learning pipeline</span></span>
> * <span data-ttu-id="44dd3-254">加载和转换数据</span><span class="sxs-lookup"><span data-stu-id="44dd3-254">Load and transform the data</span></span>
> * <span data-ttu-id="44dd3-255">选择学习算法</span><span class="sxs-lookup"><span data-stu-id="44dd3-255">Choose a learning algorithm</span></span>
> * <span data-ttu-id="44dd3-256">定型模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-256">Train the model</span></span>
> * <span data-ttu-id="44dd3-257">评估模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-257">Evaluate the model</span></span>
> * <span data-ttu-id="44dd3-258">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="44dd3-258">Use the model for predictions</span></span>

<span data-ttu-id="44dd3-259">进入下一教程了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="44dd3-259">Advance to the next tutorial to learn more.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="44dd3-260">Iris 聚类分析</span><span class="sxs-lookup"><span data-stu-id="44dd3-260">Iris clustering</span></span>](iris-clustering.md)
