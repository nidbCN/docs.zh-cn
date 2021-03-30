---
title: 教程：检测产品销售中的异常
description: 了解如何构建针对产品销售数据的异常检测应用程序。 本教程将使用 Visual Studio 2019 和 C# 创建 .NET Core 控制台应用程序。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: 2262f2c5e8e389a13c583073bb2002fcd5da91ee
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874649"
---
# <a name="tutorial-detect-anomalies-in-product-sales-with-mlnet"></a><span data-ttu-id="d1060-104">教程：使用 ML.NET 检测产品销售中的异常</span><span class="sxs-lookup"><span data-stu-id="d1060-104">Tutorial: Detect anomalies in product sales with ML.NET</span></span>

<span data-ttu-id="d1060-105">了解如何构建针对产品销售数据的异常检测应用程序。</span><span class="sxs-lookup"><span data-stu-id="d1060-105">Learn how to build an anomaly detection application for product sales data.</span></span> <span data-ttu-id="d1060-106">本教程将使用 Visual Studio 和 C# 创建 .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="d1060-106">This tutorial creates a .NET Core console application using C# in Visual Studio.</span></span>

<span data-ttu-id="d1060-107">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="d1060-107">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="d1060-108">加载数据</span><span class="sxs-lookup"><span data-stu-id="d1060-108">Load the data</span></span>
> * <span data-ttu-id="d1060-109">针对峰值异常情况检测创建转换</span><span class="sxs-lookup"><span data-stu-id="d1060-109">Create a transform for spike anomaly detection</span></span>
> * <span data-ttu-id="d1060-110">使用转换检测峰值异常</span><span class="sxs-lookup"><span data-stu-id="d1060-110">Detect spike anomalies with the transform</span></span>
> * <span data-ttu-id="d1060-111">针对更改点异常情况检测创建转换</span><span class="sxs-lookup"><span data-stu-id="d1060-111">Create a transform for change point anomaly detection</span></span>
> * <span data-ttu-id="d1060-112">使用转换检测更改点异常</span><span class="sxs-lookup"><span data-stu-id="d1060-112">Detect change point anomalies with the transform</span></span>

<span data-ttu-id="d1060-113">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/ProductSalesAnomalyDetection) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="d1060-113">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/ProductSalesAnomalyDetection) repository.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d1060-114">先决条件</span><span class="sxs-lookup"><span data-stu-id="d1060-114">Prerequisites</span></span>

* <span data-ttu-id="d1060-115">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2017 版本 15.6 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。</span><span class="sxs-lookup"><span data-stu-id="d1060-115">[Visual Studio 2017 version 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the ".NET Core cross-platform development" workload installed.</span></span>

* [<span data-ttu-id="d1060-116">product-sales.csv 数据集</span><span class="sxs-lookup"><span data-stu-id="d1060-116">The product-sales.csv dataset</span></span>](https://raw.githubusercontent.com/dotnet/machinelearning-samples/main/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv)

>[!NOTE]
> <span data-ttu-id="d1060-117">`product-sales.csv` 中的数据格式基于“Shampoo Sales Over a Three Year Period”数据集，该数据集最初来自 DataMarket，由 Rob Hyndman 创建的 Time Series Data Library (TSDL) 提供。</span><span class="sxs-lookup"><span data-stu-id="d1060-117">The data format in `product-sales.csv` is based on the dataset “Shampoo Sales Over a Three Year Period” originally sourced from DataMarket and provided by Time Series Data Library (TSDL), created by Rob Hyndman.</span></span>
> <span data-ttu-id="d1060-118">“Shampoo Sales Over a Three Year Period”数据集根据 DataMarket 默认开放许可进行许可。</span><span class="sxs-lookup"><span data-stu-id="d1060-118">“Shampoo Sales Over a Three Year Period” Dataset Licensed Under the DataMarket Default Open License.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="d1060-119">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="d1060-119">Create a console application</span></span>

1. <span data-ttu-id="d1060-120">创建名为“ProductSalesAnomalyDetection”的 **.NET Core 控制台应用程序**。</span><span class="sxs-lookup"><span data-stu-id="d1060-120">Create a **.NET Core Console Application** called "ProductSalesAnomalyDetection".</span></span>

2. <span data-ttu-id="d1060-121">在项目中创建名为“Data”的目录，用于保存数据集文件。</span><span class="sxs-lookup"><span data-stu-id="d1060-121">Create a directory named *Data* in your project to save your data set files.</span></span>

3. <span data-ttu-id="d1060-122">安装“Microsoft.ML NuGet 包”  ：</span><span class="sxs-lookup"><span data-stu-id="d1060-122">Install the **Microsoft.ML NuGet Package**:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="d1060-123">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="d1060-123">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="d1060-124">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，再选择“安装”按钮 。</span><span class="sxs-lookup"><span data-stu-id="d1060-124">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML** and select the **Install** button.</span></span> <span data-ttu-id="d1060-125">选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。</span><span class="sxs-lookup"><span data-stu-id="d1060-125">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span> <span data-ttu-id="d1060-126">对“Microsoft.ML.TimeSeries”重复这些步骤。</span><span class="sxs-lookup"><span data-stu-id="d1060-126">Repeat these steps for **Microsoft.ML.TimeSeries**.</span></span>

4. <span data-ttu-id="d1060-127">在 Program.cs 文件的顶部添加以下 `using` 语句：</span><span class="sxs-lookup"><span data-stu-id="d1060-127">Add the following `using` statements at the top of your *Program.cs* file:</span></span>

    [!code-csharp[AddUsings](./snippets/sales-anomaly-detection/csharp/Program.cs#AddUsings "Add necessary usings")]

### <a name="download-your-data"></a><span data-ttu-id="d1060-128">下载数据</span><span class="sxs-lookup"><span data-stu-id="d1060-128">Download your data</span></span>

1. <span data-ttu-id="d1060-129">下载数据集并将其保存到之前创建的 *Data* 文件夹中：</span><span class="sxs-lookup"><span data-stu-id="d1060-129">Download the dataset and save it to the *Data* folder you previously created:</span></span>

   * <span data-ttu-id="d1060-130">右键单击 [*product-sales.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/main/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv) 并选择“将链接(或目标)另存为...”</span><span class="sxs-lookup"><span data-stu-id="d1060-130">Right click on [*product-sales.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/main/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv) and select "Save Link (or Target) As..."</span></span>

     <span data-ttu-id="d1060-131">确保将 \*.csv 文件保存到 *Data* 文件夹，或者在将其保存到其他位置后，将 \*.csv 文件移动到 *Data* 文件夹。</span><span class="sxs-lookup"><span data-stu-id="d1060-131">Make sure you either save the \*.csv file to the *Data* folder, or after you save it elsewhere, move the \*.csv file to the *Data* folder.</span></span>

2. <span data-ttu-id="d1060-132">在解决方案资源管理器中，右键单击 \*.csv 文件并选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="d1060-132">In Solution Explorer, right-click the \*.csv file and select **Properties**.</span></span> <span data-ttu-id="d1060-133">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”  。</span><span class="sxs-lookup"><span data-stu-id="d1060-133">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

<span data-ttu-id="d1060-134">下表是来自 \*.csv 文件的数据预览：</span><span class="sxs-lookup"><span data-stu-id="d1060-134">The following table is a data preview from your \*.csv file:</span></span>

|<span data-ttu-id="d1060-135">月份</span><span class="sxs-lookup"><span data-stu-id="d1060-135">Month</span></span>  |<span data-ttu-id="d1060-136">ProductSales</span><span class="sxs-lookup"><span data-stu-id="d1060-136">ProductSales</span></span> |
|-------|-------------|
|<span data-ttu-id="d1060-137">1-Jan</span><span class="sxs-lookup"><span data-stu-id="d1060-137">1-Jan</span></span>  |    <span data-ttu-id="d1060-138">271</span><span class="sxs-lookup"><span data-stu-id="d1060-138">271</span></span>      |
|<span data-ttu-id="d1060-139">2-Jan</span><span class="sxs-lookup"><span data-stu-id="d1060-139">2-Jan</span></span>  |    <span data-ttu-id="d1060-140">150.9</span><span class="sxs-lookup"><span data-stu-id="d1060-140">150.9</span></span>    |
|<span data-ttu-id="d1060-141">.....</span><span class="sxs-lookup"><span data-stu-id="d1060-141">.....</span></span>  |    <span data-ttu-id="d1060-142">.....</span><span class="sxs-lookup"><span data-stu-id="d1060-142">.....</span></span>    |
|<span data-ttu-id="d1060-143">1-Feb</span><span class="sxs-lookup"><span data-stu-id="d1060-143">1-Feb</span></span>  |    <span data-ttu-id="d1060-144">199.3</span><span class="sxs-lookup"><span data-stu-id="d1060-144">199.3</span></span>    |
|<span data-ttu-id="d1060-145">.....</span><span class="sxs-lookup"><span data-stu-id="d1060-145">.....</span></span>  |    <span data-ttu-id="d1060-146">.....</span><span class="sxs-lookup"><span data-stu-id="d1060-146">.....</span></span>    |

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="d1060-147">创建类和定义路径</span><span class="sxs-lookup"><span data-stu-id="d1060-147">Create classes and define paths</span></span>

<span data-ttu-id="d1060-148">接下来，定义输入和预测类数据结构。</span><span class="sxs-lookup"><span data-stu-id="d1060-148">Next, define your input and prediction class data structures.</span></span>

<span data-ttu-id="d1060-149">向项目添加一个新类：</span><span class="sxs-lookup"><span data-stu-id="d1060-149">Add a new class to your project:</span></span>

1. <span data-ttu-id="d1060-150">在“解决方案资源管理器”中，右键单击该项目，然后选择“添加”>“新项” 。</span><span class="sxs-lookup"><span data-stu-id="d1060-150">In **Solution Explorer**, right-click the project, and then select **Add > New Item**.</span></span>

2. <span data-ttu-id="d1060-151">在“添加新项”对话框中，选择“类”并将“名称”字段更改为“ProductSalesData.cs”  。</span><span class="sxs-lookup"><span data-stu-id="d1060-151">In the **Add New Item dialog box**, select **Class** and change the **Name** field to *ProductSalesData.cs*.</span></span> <span data-ttu-id="d1060-152">然后，选择“添加”按钮。</span><span class="sxs-lookup"><span data-stu-id="d1060-152">Then, select the **Add** button.</span></span>

   <span data-ttu-id="d1060-153">此时，*ProductSalesData.cs* 文件在代码编辑器中打开。</span><span class="sxs-lookup"><span data-stu-id="d1060-153">The *ProductSalesData.cs* file opens in the code editor.</span></span>

3. <span data-ttu-id="d1060-154">将以下 `using` 语句添加到 *ProductSalesData.cs* 顶部：</span><span class="sxs-lookup"><span data-stu-id="d1060-154">Add the following `using` statement to the top of *ProductSalesData.cs*:</span></span>

   ```csharp
   using Microsoft.ML.Data;
   ```

4. <span data-ttu-id="d1060-155">删除现有类定义并向 *ProductSalesData.cs* 文件添加以下代码，其中有两个类 `ProductSalesData` 和 `ProductSalesPrediction`：</span><span class="sxs-lookup"><span data-stu-id="d1060-155">Remove the existing class definition and add the following code, which has two classes `ProductSalesData` and `ProductSalesPrediction`, to the *ProductSalesData.cs* file:</span></span>

    [!code-csharp[DeclareTypes](./snippets/sales-anomaly-detection/csharp/ProductSalesData.cs#DeclareTypes "Declare data record types")]

    <span data-ttu-id="d1060-156">`ProductSalesData` 指定输入数据类。</span><span class="sxs-lookup"><span data-stu-id="d1060-156">`ProductSalesData` specifies an input data class.</span></span> <span data-ttu-id="d1060-157">[LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性指定应加载数据集中的哪些列（按列索引）。</span><span class="sxs-lookup"><span data-stu-id="d1060-157">The [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) attribute specifies which columns (by column index) in the dataset should be loaded.</span></span>

    <span data-ttu-id="d1060-158">`ProductSalesPrediction` 指定预测数据类。</span><span class="sxs-lookup"><span data-stu-id="d1060-158">`ProductSalesPrediction` specifies the prediction data class.</span></span> <span data-ttu-id="d1060-159">对于异常情况检测，预测包括指示是否存在异常、原始分数和 p 值的警报。</span><span class="sxs-lookup"><span data-stu-id="d1060-159">For anomaly detection, the prediction consists of an alert to indicate whether there is an anomaly, a raw score, and p-value.</span></span> <span data-ttu-id="d1060-160">P 值越接近 0，出现异常的可能性就越大。</span><span class="sxs-lookup"><span data-stu-id="d1060-160">The closer the p-value is to 0, the more likely an anomaly has occurred.</span></span>

5. <span data-ttu-id="d1060-161">创建两个全局字段来存储最近下载的数据集文件路径和已保存的模型文件路径：</span><span class="sxs-lookup"><span data-stu-id="d1060-161">Create two global fields to hold the recently downloaded dataset file path and the saved model file path:</span></span>

    * <span data-ttu-id="d1060-162">`_dataPath` 具有用于定型模型的数据集路径。</span><span class="sxs-lookup"><span data-stu-id="d1060-162">`_dataPath` has the path to the dataset used to train the model.</span></span>
    * <span data-ttu-id="d1060-163">`_docsize` 具有数据集文件中记录的数量。</span><span class="sxs-lookup"><span data-stu-id="d1060-163">`_docsize` has the number of records in dataset file.</span></span> <span data-ttu-id="d1060-164">将使用 `_docSize` 来计算 `pvalueHistoryLength`。</span><span class="sxs-lookup"><span data-stu-id="d1060-164">You'll use `_docSize` to calculate `pvalueHistoryLength`.</span></span>

6. <span data-ttu-id="d1060-165">将以下代码添加到 `Main` 方法上方的行中，以指定这些路径：</span><span class="sxs-lookup"><span data-stu-id="d1060-165">Add the following code to the line right above the `Main` method to specify those paths:</span></span>

    [!code-csharp[Declare global variables](./snippets/sales-anomaly-detection/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

### <a name="initialize-variables-in-main"></a><span data-ttu-id="d1060-166">在 Main 中初始化变量</span><span class="sxs-lookup"><span data-stu-id="d1060-166">Initialize variables in Main</span></span>

1. <span data-ttu-id="d1060-167">使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 `mlContext` 变量：</span><span class="sxs-lookup"><span data-stu-id="d1060-167">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the `mlContext` variable:</span></span>

    [!code-csharp[CreateMLContext](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateMLContext "Create the ML Context")]

    <span data-ttu-id="d1060-168">执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="d1060-168">The [MLContext class](xref:Microsoft.ML.MLContext) is a starting point for all ML.NET operations, and initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="d1060-169">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="d1060-169">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

### <a name="load-the-data"></a><span data-ttu-id="d1060-170">加载数据</span><span class="sxs-lookup"><span data-stu-id="d1060-170">Load the data</span></span>

<span data-ttu-id="d1060-171">ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="d1060-171">Data in ML.NET is represented as an [IDataView class](xref:Microsoft.ML.IDataView).</span></span> <span data-ttu-id="d1060-172">`IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。</span><span class="sxs-lookup"><span data-stu-id="d1060-172">`IDataView` is a flexible, efficient way of describing tabular data (numeric and text).</span></span> <span data-ttu-id="d1060-173">可从文本文件或其他源（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。</span><span class="sxs-lookup"><span data-stu-id="d1060-173">Data can be loaded from a text file or from other sources (for example, SQL database or log files) to an `IDataView` object.</span></span>

1. <span data-ttu-id="d1060-174">添加以下代码作为 `Main()` 方法的下一行：</span><span class="sxs-lookup"><span data-stu-id="d1060-174">Add the following code as the next line of the `Main()` method:</span></span>

    [!code-csharp[LoadData](./snippets/sales-anomaly-detection/csharp/Program.cs#LoadData "loading dataset")]

    <span data-ttu-id="d1060-175">[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 用于定义数据架构并读取文件。</span><span class="sxs-lookup"><span data-stu-id="d1060-175">The [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) defines the data schema and reads in the file.</span></span> <span data-ttu-id="d1060-176">它使用数据路径变量并返回 `IDataView`。</span><span class="sxs-lookup"><span data-stu-id="d1060-176">It takes in the data path variables and returns an `IDataView`.</span></span>

## <a name="time-series-anomaly-detection"></a><span data-ttu-id="d1060-177">时序异常情况检测</span><span class="sxs-lookup"><span data-stu-id="d1060-177">Time series anomaly detection</span></span>

<span data-ttu-id="d1060-178">异常情况检测标记意外或异常事件/行为。</span><span class="sxs-lookup"><span data-stu-id="d1060-178">Anomaly detection flags unexpected or unusual events or behaviors.</span></span> <span data-ttu-id="d1060-179">它提供寻找问题所在位置的线索，并帮助回答“这是否奇怪？”的问题。</span><span class="sxs-lookup"><span data-stu-id="d1060-179">It gives clues where to look for problems and helps you answer the question "Is this weird?".</span></span>

![“这是否奇怪”异常情况检测的示例。](./media/sales-anomaly-detection/time-series-anomaly-detection.png)

<span data-ttu-id="d1060-181">异常情况检测是检测时序数据离群值的过程；在给定的输入时序上指向“怪异”或不是预期行为的行为。</span><span class="sxs-lookup"><span data-stu-id="d1060-181">Anomaly detection is the process of detecting time-series data outliers; points on a given input time-series where the behavior isn't what was expected, or "weird".</span></span>

<span data-ttu-id="d1060-182">异常情况检测在很多方面都很有用。</span><span class="sxs-lookup"><span data-stu-id="d1060-182">Anomaly detection can be useful in lots of ways.</span></span> <span data-ttu-id="d1060-183">例如：</span><span class="sxs-lookup"><span data-stu-id="d1060-183">For instance:</span></span>

<span data-ttu-id="d1060-184">如果你有一辆车，你可能想要知道：此油量计读数是否正常，或者是否存在漏油现象？</span><span class="sxs-lookup"><span data-stu-id="d1060-184">If you have a car, you might want to know: Is this oil gauge reading normal, or do I have a leak?</span></span>
<span data-ttu-id="d1060-185">如果正在监视能耗，你需要知道：是否出现了中断？</span><span class="sxs-lookup"><span data-stu-id="d1060-185">If you're monitoring power consumption, you’d want to know: Is there an outage?</span></span>

<span data-ttu-id="d1060-186">可以检测到两种类型的时序异常情况：</span><span class="sxs-lookup"><span data-stu-id="d1060-186">There are two types of time series anomalies that can be detected:</span></span>

* <span data-ttu-id="d1060-187">**峰值** 指示系统中异常行为的临时突发。</span><span class="sxs-lookup"><span data-stu-id="d1060-187">**Spikes** indicate temporary bursts of anomalous behavior in the system.</span></span>

* <span data-ttu-id="d1060-188">**更改点** 指示系统中一段时间内持续更改的开始。</span><span class="sxs-lookup"><span data-stu-id="d1060-188">**Change points** indicate the beginning of persistent changes over time in the system.</span></span>

<span data-ttu-id="d1060-189">在 ML.NET 中，IID 峰值检测或 IID 更改点检测算法适用于[独立且均匀分布的数据集](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables)。</span><span class="sxs-lookup"><span data-stu-id="d1060-189">In ML.NET, The IID Spike Detection or IID Change point Detection algorithms are suited for [independent and identically distributed datasets](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables).</span></span>

<span data-ttu-id="d1060-190">与其他教程中的模型不同，时序异常检测器转换直接对输入数据进行操作。</span><span class="sxs-lookup"><span data-stu-id="d1060-190">Unlike the models in the other tutorials, the time series anomaly detector transforms operate directly on input data.</span></span> <span data-ttu-id="d1060-191">`IEstimator.Fit()` 方法不需要训练数据来生成转换。</span><span class="sxs-lookup"><span data-stu-id="d1060-191">The `IEstimator.Fit()` method does not need training data to produce the transform.</span></span> <span data-ttu-id="d1060-192">不过，它确实需要数据架构，该架构由从空列表 `ProductSalesData` 中生成的数据视图提供。</span><span class="sxs-lookup"><span data-stu-id="d1060-192">It does need the data schema though, which is provided by a data view generated from an empty list of `ProductSalesData`.</span></span>

<span data-ttu-id="d1060-193">将分析相同的产品销售数据来检测峰值和更改点。</span><span class="sxs-lookup"><span data-stu-id="d1060-193">You'll analyze the same product sales data to detect spikes and change points.</span></span> <span data-ttu-id="d1060-194">峰值检测和更改点检测的模型生成和训练过程相同；主要区别在于使用的特定检测算法。</span><span class="sxs-lookup"><span data-stu-id="d1060-194">The building and training model process is the same for spike detection and change point detection; the main difference is the specific detection algorithm used.</span></span>

## <a name="spike-detection"></a><span data-ttu-id="d1060-195">峰值检测</span><span class="sxs-lookup"><span data-stu-id="d1060-195">Spike detection</span></span>

<span data-ttu-id="d1060-196">峰值检测旨在识别与大部分时序数据值明显不同的突然但临时的突发。</span><span class="sxs-lookup"><span data-stu-id="d1060-196">The goal of spike detection is to identify sudden yet temporary bursts that significantly differ from the majority of the time series data values.</span></span> <span data-ttu-id="d1060-197">及时检测到这些可疑的罕见项、事件或观察值很重要，这样才能尽量减少其产生。</span><span class="sxs-lookup"><span data-stu-id="d1060-197">It's important to detect these suspicious rare items, events, or observations in a timely manner to be minimized.</span></span> <span data-ttu-id="d1060-198">以下方法可用于检测各种异常情况，例如：中断、网络攻击或病毒式 Web 内容。</span><span class="sxs-lookup"><span data-stu-id="d1060-198">The following approach can be used to detect a variety of anomalies such as: outages, cyber-attacks, or viral web content.</span></span> <span data-ttu-id="d1060-199">下图是时序数据集中峰值的示例：</span><span class="sxs-lookup"><span data-stu-id="d1060-199">The following image is an example of spikes in a time series dataset:</span></span>

![显示两个峰值检测的屏幕截图。](./media/sales-anomaly-detection/two-spike-detections.png)

### <a name="add-the-createemptydataview-method"></a><span data-ttu-id="d1060-201">添加 CreateEmptyDataView () 方法</span><span class="sxs-lookup"><span data-stu-id="d1060-201">Add the CreateEmptyDataView() method</span></span>

<span data-ttu-id="d1060-202">将以下方法添加到 `Program.cs`：</span><span class="sxs-lookup"><span data-stu-id="d1060-202">Add the following method to `Program.cs`:</span></span>

[!code-csharp[CreateEmptyDataView](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateEmptyDataView)]

<span data-ttu-id="d1060-203">`CreateEmptyDataView()` 生成一个空数据视图对象，该对象具有正确架构，可用作 `IEstimator.Fit()` 方法的输入。</span><span class="sxs-lookup"><span data-stu-id="d1060-203">The `CreateEmptyDataView()` produces an empty data view object with the correct schema to be used as input to the `IEstimator.Fit()` method.</span></span>

### <a name="create-the-detectspike-method"></a><span data-ttu-id="d1060-204">创建 DetectSpike() 方法</span><span class="sxs-lookup"><span data-stu-id="d1060-204">Create the DetectSpike() method</span></span>

<span data-ttu-id="d1060-205">`DetectSpike()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d1060-205">The `DetectSpike()` method:</span></span>

* <span data-ttu-id="d1060-206">从估算器创建转换。</span><span class="sxs-lookup"><span data-stu-id="d1060-206">Creates the transform from the estimator.</span></span>
* <span data-ttu-id="d1060-207">根据历史销售数据检测峰值。</span><span class="sxs-lookup"><span data-stu-id="d1060-207">Detects spikes based on historical sales data.</span></span>
* <span data-ttu-id="d1060-208">显示结果。</span><span class="sxs-lookup"><span data-stu-id="d1060-208">Displays the results.</span></span>

1. <span data-ttu-id="d1060-209">使用下面的代码紧随 `Main()` 方法后创建 `DetectSpike()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d1060-209">Create the `DetectSpike()` method, just after the `Main()` method, using the following code:</span></span>

    ```csharp
    static void DetectSpike(MLContext mlContext, int docSize, IDataView productSales)
    {

    }
    ```

1. <span data-ttu-id="d1060-210">使用 [IidSpikeEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidSpikeEstimator) 训练模型用于峰值检测。</span><span class="sxs-lookup"><span data-stu-id="d1060-210">Use the [IidSpikeEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidSpikeEstimator) to train the model for spike detection.</span></span> <span data-ttu-id="d1060-211">使用以下代码将其添加到 `DetectSpike()` 方法中：</span><span class="sxs-lookup"><span data-stu-id="d1060-211">Add it to the `DetectSpike()` method with the following code:</span></span>

    [!code-csharp[AddSpikeTrainer](./snippets/sales-anomaly-detection/csharp/Program.cs#AddSpikeTrainer)]

1. <span data-ttu-id="d1060-212">通过在 `DetectSpike()` 方法中添加以下代码作为下一代码行来创建峰值检测转换：</span><span class="sxs-lookup"><span data-stu-id="d1060-212">Create the spike detection transform by adding the following as the next line of code in the `DetectSpike()` method:</span></span>

    [!code-csharp[TrainModel1](./snippets/sales-anomaly-detection/csharp/Program.cs#TrainModel1)]

1. <span data-ttu-id="d1060-213">添加以下代码行将 `productSales` 数据转换为 `DetectSpike()` 方法中的下一行：</span><span class="sxs-lookup"><span data-stu-id="d1060-213">Add the following line of code to transform the `productSales` data as the next line in the `DetectSpike()` method:</span></span>

    [!code-csharp[TransformData1](./snippets/sales-anomaly-detection/csharp/Program.cs#TransformData1)]

    <span data-ttu-id="d1060-214">之前的代码使用 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对数据集的多个输入行进行预测。</span><span class="sxs-lookup"><span data-stu-id="d1060-214">The previous code uses the [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method to make predictions for multiple input rows of a dataset.</span></span>

1. <span data-ttu-id="d1060-215">使用 [CreateEnumerable()](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法和以下代码将 `transformedData` 转换为强类型 `IEnumerable`，以方便显示：</span><span class="sxs-lookup"><span data-stu-id="d1060-215">Convert your `transformedData` into a strongly typed `IEnumerable` for easier display using the [CreateEnumerable()](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method with the following code:</span></span>

    [!code-csharp[CreateEnumerable1](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateEnumerable1)]

1. <span data-ttu-id="d1060-216">使用以下 <xref:System.Console.WriteLine?displayProperty=nameWithType> 代码创建显示标头行：</span><span class="sxs-lookup"><span data-stu-id="d1060-216">Create a display header line using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

    [!code-csharp[DisplayHeader1](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayHeader1)]

    <span data-ttu-id="d1060-217">将在峰值检测结果中显示以下信息：</span><span class="sxs-lookup"><span data-stu-id="d1060-217">You'll display the following information in your spike detection results:</span></span>

    * <span data-ttu-id="d1060-218">`Alert` 指示给定数据点的峰值警报。</span><span class="sxs-lookup"><span data-stu-id="d1060-218">`Alert` indicates a spike alert for a given data point.</span></span>
    * <span data-ttu-id="d1060-219">`Score` 是数据集中给定数据点的 `ProductSales` 值。</span><span class="sxs-lookup"><span data-stu-id="d1060-219">`Score` is the `ProductSales` value for a given data point in the dataset.</span></span>
    * <span data-ttu-id="d1060-220">`P-Value`“P”代表概率，</span><span class="sxs-lookup"><span data-stu-id="d1060-220">`P-Value` The "P" stands for probability.</span></span> <span data-ttu-id="d1060-221">P 值越接近 0，数据点越有可能出现异常情况。</span><span class="sxs-lookup"><span data-stu-id="d1060-221">The closer the p-value is to 0, the more likely the data point is an anomaly.</span></span>

1. <span data-ttu-id="d1060-222">使用以下代码循环访问 `predictions` `IEnumerable` 并显示结果：</span><span class="sxs-lookup"><span data-stu-id="d1060-222">Use the following code to iterate through the `predictions` `IEnumerable` and display the results:</span></span>

    [!code-csharp[DisplayResults1](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayResults1)]

1. <span data-ttu-id="d1060-223">将调用添加到 `Main()` 方法中的 `DetectSpike()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d1060-223">Add the call to the `DetectSpike()`method in the `Main()` method:</span></span>

    [!code-csharp[CallDetectSpike](./snippets/sales-anomaly-detection/csharp/Program.cs#CallDetectSpike)]

## <a name="spike-detection-results"></a><span data-ttu-id="d1060-224">峰值检测结果</span><span class="sxs-lookup"><span data-stu-id="d1060-224">Spike detection results</span></span>

<span data-ttu-id="d1060-225">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="d1060-225">Your results should be similar to the following.</span></span> <span data-ttu-id="d1060-226">处理期间将显示消息。</span><span class="sxs-lookup"><span data-stu-id="d1060-226">During processing, messages are displayed.</span></span> <span data-ttu-id="d1060-227">你可能会看到警告或处理消息。</span><span class="sxs-lookup"><span data-stu-id="d1060-227">You may see warnings, or processing messages.</span></span> <span data-ttu-id="d1060-228">为清楚起见，已从以下结果中删除某些消息。</span><span class="sxs-lookup"><span data-stu-id="d1060-228">Some of the messages have been removed from the following results for clarity.</span></span>

```console
Detect temporary changes in pattern
=============== Training the model ===============
=============== End of training process ===============
Alert   Score   P-Value
0       271.00  0.50
0       150.90  0.00
0       188.10  0.41
0       124.30  0.13
0       185.30  0.47
0       173.50  0.47
0       236.80  0.19
0       229.50  0.27
0       197.80  0.48
0       127.90  0.13
1       341.50  0.00 <-- Spike detected
0       190.90  0.48
0       199.30  0.48
0       154.50  0.24
0       215.10  0.42
0       278.30  0.19
0       196.40  0.43
0       292.00  0.17
0       231.00  0.45
0       308.60  0.18
0       294.90  0.19
1       426.60  0.00 <-- Spike detected
0       269.50  0.47
0       347.30  0.21
0       344.70  0.27
0       445.40  0.06
0       320.90  0.49
0       444.30  0.12
0       406.30  0.29
0       442.40  0.21
1       580.50  0.00 <-- Spike detected
0       412.60  0.45
1       687.00  0.01 <-- Spike detected
0       480.30  0.40
0       586.30  0.20
0       651.90  0.14
```

## <a name="change-point-detection"></a><span data-ttu-id="d1060-229">更改点检测</span><span class="sxs-lookup"><span data-stu-id="d1060-229">Change point detection</span></span>

<span data-ttu-id="d1060-230">`Change points` 是时序事件流值分布的持续更改，例如级别更改和趋势。</span><span class="sxs-lookup"><span data-stu-id="d1060-230">`Change points` are persistent changes in a time series event stream distribution of values, like level changes and trends.</span></span> <span data-ttu-id="d1060-231">这些持续更改的持续时间比 `spikes` 的持续时间长得多，可能指示灾难性事件。</span><span class="sxs-lookup"><span data-stu-id="d1060-231">These persistent changes last much longer than `spikes` and could indicate catastrophic event(s).</span></span> <span data-ttu-id="d1060-232">`Change points` 通常对肉眼不可见，但可以使用诸如以下方法的方法在数据中检测到。</span><span class="sxs-lookup"><span data-stu-id="d1060-232">`Change points` are not usually visible to the naked eye, but can be detected in your data using approaches such as in the following method.</span></span>  <span data-ttu-id="d1060-233">下图是更改点检测的示例：</span><span class="sxs-lookup"><span data-stu-id="d1060-233">The following image is an example of a change point detection:</span></span>

![显示更改点检测的屏幕截图。](./media/sales-anomaly-detection/change-point-detection.png)

### <a name="create-the-detectchangepoint-method"></a><span data-ttu-id="d1060-235">创建 DetectChangepoint() 方法</span><span class="sxs-lookup"><span data-stu-id="d1060-235">Create the DetectChangepoint() method</span></span>

<span data-ttu-id="d1060-236">`DetectChangepoint()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="d1060-236">The `DetectChangepoint()` method executes the following tasks:</span></span>

* <span data-ttu-id="d1060-237">从估算器创建转换。</span><span class="sxs-lookup"><span data-stu-id="d1060-237">Creates the transform from the estimator.</span></span>
* <span data-ttu-id="d1060-238">根据历史销售数据检测更改点。</span><span class="sxs-lookup"><span data-stu-id="d1060-238">Detects change points based on historical sales data.</span></span>
* <span data-ttu-id="d1060-239">显示结果。</span><span class="sxs-lookup"><span data-stu-id="d1060-239">Displays the results.</span></span>

1. <span data-ttu-id="d1060-240">使用下面的代码紧随 `Main()` 方法后创建 `DetectChangepoint()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d1060-240">Create the `DetectChangepoint()` method, just after the `Main()` method, using the following code:</span></span>

    ```csharp
    static void DetectChangepoint(MLContext mlContext, int docSize, IDataView productSales)
    {

    }
    ```

1. <span data-ttu-id="d1060-241">使用以下代码在 `DetectChangepoint()` 方法中创建 [ iidChangePointEstimator ](xref:Microsoft.ML.Transforms.TimeSeries.IidChangePointEstimator)：</span><span class="sxs-lookup"><span data-stu-id="d1060-241">Create the [iidChangePointEstimator](xref:Microsoft.ML.Transforms.TimeSeries.IidChangePointEstimator) in the `DetectChangepoint()` method with the following code:</span></span>

    [!code-csharp[AddChangepointTrainer](./snippets/sales-anomaly-detection/csharp/Program.cs#AddChangepointTrainer)]

1. <span data-ttu-id="d1060-242">和先前的操作一样，通过在 `DetectChangePoint()` 方法中添加以下代码行，从估算器创建转换：</span><span class="sxs-lookup"><span data-stu-id="d1060-242">As you did previously, create the transform from the estimator by adding the following line of code in the `DetectChangePoint()` method:</span></span>

    [!code-csharp[TrainModel2](./snippets/sales-anomaly-detection/csharp/Program.cs#TrainModel2)]

1. <span data-ttu-id="d1060-243">使用 `Transform()` 方法通过将以下代码添加到 `DetectChangePoint()` 来转换数据：</span><span class="sxs-lookup"><span data-stu-id="d1060-243">Use the `Transform()` method to transform the data by adding the following code to `DetectChangePoint()`:</span></span>

    [!code-csharp[TransformData2](./snippets/sales-anomaly-detection/csharp/Program.cs#TransformData2)]

1. <span data-ttu-id="d1060-244">如之前一样，使用 `CreateEnumerable()` 方法和以下代码将 `transformedData` 转换为强类型 `IEnumerable`，以方便显示：</span><span class="sxs-lookup"><span data-stu-id="d1060-244">As you did previously, convert your `transformedData` into a strongly typed `IEnumerable` for easier display using the `CreateEnumerable()`method with the following code:</span></span>

    [!code-csharp[CreateEnumerable2](./snippets/sales-anomaly-detection/csharp/Program.cs#CreateEnumerable2)]

1. <span data-ttu-id="d1060-245">使用以下代码创建显示标头，用作 `DetectChangePoint()` 方法中的下一行：</span><span class="sxs-lookup"><span data-stu-id="d1060-245">Create a display header with the following code as the next line in the `DetectChangePoint()` method:</span></span>

    [!code-csharp[DisplayHeader2](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayHeader2)]

    <span data-ttu-id="d1060-246">将在更改点检测结果中显示以下信息：</span><span class="sxs-lookup"><span data-stu-id="d1060-246">You'll display the following information in your change point detection results:</span></span>

    * <span data-ttu-id="d1060-247">`Alert` 指示给定数据点的更改点警报。</span><span class="sxs-lookup"><span data-stu-id="d1060-247">`Alert` indicates a change point alert for a given data point.</span></span>
    * <span data-ttu-id="d1060-248">`Score` 是数据集中给定数据点的 `ProductSales` 值。</span><span class="sxs-lookup"><span data-stu-id="d1060-248">`Score` is the `ProductSales` value for a given data point in the dataset.</span></span>
    * <span data-ttu-id="d1060-249">`P-Value`“P”代表概率，</span><span class="sxs-lookup"><span data-stu-id="d1060-249">`P-Value` The "P" stands for probability.</span></span> <span data-ttu-id="d1060-250">P 值越接近 0，数据点越有可能出现异常情况。</span><span class="sxs-lookup"><span data-stu-id="d1060-250">The closer the P-value is to 0, the more likely the data point is an anomaly.</span></span>
    * <span data-ttu-id="d1060-251">`Martingale value` 用于根据 P 值序列识别数据点的“奇怪”程度。</span><span class="sxs-lookup"><span data-stu-id="d1060-251">`Martingale value` is used to identify how "weird" a data point is, based on the sequence of P-values.</span></span>

1. <span data-ttu-id="d1060-252">使用以下代码循环访问 `predictions` `IEnumerable` 并显示结果：</span><span class="sxs-lookup"><span data-stu-id="d1060-252">Iterate through the `predictions` `IEnumerable` and display the results with the following code:</span></span>

    [!code-csharp[DisplayResults2](./snippets/sales-anomaly-detection/csharp/Program.cs#DisplayResults2)]

1. <span data-ttu-id="d1060-253">将以下调用添加到 `Main()` 方法中的 `DetectChangepoint()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d1060-253">Add the following call to the `DetectChangepoint()`method in the `Main()` method:</span></span>

    [!code-csharp[CallDetectChangepoint](./snippets/sales-anomaly-detection/csharp/Program.cs#CallDetectChangepoint)]

## <a name="change-point-detection-results"></a><span data-ttu-id="d1060-254">更改点检测结果</span><span class="sxs-lookup"><span data-stu-id="d1060-254">Change point detection results</span></span>

<span data-ttu-id="d1060-255">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="d1060-255">Your results should be similar to the following.</span></span> <span data-ttu-id="d1060-256">处理期间将显示消息。</span><span class="sxs-lookup"><span data-stu-id="d1060-256">During processing, messages are displayed.</span></span> <span data-ttu-id="d1060-257">你可能会看到警告或处理消息。</span><span class="sxs-lookup"><span data-stu-id="d1060-257">You may see warnings, or processing messages.</span></span> <span data-ttu-id="d1060-258">为清楚起见，已从以下结果中删除某些消息。</span><span class="sxs-lookup"><span data-stu-id="d1060-258">Some messages have been removed from the following results for clarity.</span></span>

```console
Detect Persistent changes in pattern
=============== Training the model Using Change Point Detection Algorithm===============
=============== End of training process ===============
Alert   Score   P-Value Martingale value
0       271.00  0.50    0.00
0       150.90  0.00    2.33
0       188.10  0.41    2.80
0       124.30  0.13    9.16
0       185.30  0.47    9.77
0       173.50  0.47    10.41
0       236.80  0.19    24.46
0       229.50  0.27    42.38
1       197.80  0.48    44.23 <-- alert is on, predicted changepoint
0       127.90  0.13    145.25
0       341.50  0.00    0.01
0       190.90  0.48    0.01
0       199.30  0.48    0.00
0       154.50  0.24    0.00
0       215.10  0.42    0.00
0       278.30  0.19    0.00
0       196.40  0.43    0.00
0       292.00  0.17    0.01
0       231.00  0.45    0.00
0       308.60  0.18    0.00
0       294.90  0.19    0.00
0       426.60  0.00    0.00
0       269.50  0.47    0.00
0       347.30  0.21    0.00
0       344.70  0.27    0.00
0       445.40  0.06    0.02
0       320.90  0.49    0.01
0       444.30  0.12    0.02
0       406.30  0.29    0.01
0       442.40  0.21    0.01
0       580.50  0.00    0.01
0       412.60  0.45    0.01
0       687.00  0.01    0.12
0       480.30  0.40    0.08
0       586.30  0.20    0.03
0       651.90  0.14    0.09
```

<span data-ttu-id="d1060-259">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="d1060-259">Congratulations!</span></span> <span data-ttu-id="d1060-260">现在已成功生成用于检测销售数据中的峰值和更改点异常情况的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="d1060-260">You've now successfully built machine learning models for detecting spikes and change point anomalies in sales data.</span></span>

<span data-ttu-id="d1060-261">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/ProductSalesAnomalyDetection) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="d1060-261">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/ProductSalesAnomalyDetection) repository.</span></span>

<span data-ttu-id="d1060-262">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="d1060-262">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="d1060-263">加载数据</span><span class="sxs-lookup"><span data-stu-id="d1060-263">Load the data</span></span>
> * <span data-ttu-id="d1060-264">训练模型用于峰值异常情况检测</span><span class="sxs-lookup"><span data-stu-id="d1060-264">Train the model for spike anomaly detection</span></span>
> * <span data-ttu-id="d1060-265">使用经过训练的模型检测峰值异常情况</span><span class="sxs-lookup"><span data-stu-id="d1060-265">Detect spike anomalies with the trained model</span></span>
> * <span data-ttu-id="d1060-266">训练模型用于更改点异常情况检测</span><span class="sxs-lookup"><span data-stu-id="d1060-266">Train the model for change point anomaly detection</span></span>
> * <span data-ttu-id="d1060-267">使用经过训练的模型检测更改点异常情况</span><span class="sxs-lookup"><span data-stu-id="d1060-267">Detect change point anomalies with the trained mode</span></span>

## <a name="next-steps"></a><span data-ttu-id="d1060-268">后续步骤</span><span class="sxs-lookup"><span data-stu-id="d1060-268">Next steps</span></span>

<span data-ttu-id="d1060-269">请查看机器学习示例 GitHub 存储库，以探索周期性数据异常情况检测示例。</span><span class="sxs-lookup"><span data-stu-id="d1060-269">Check out the Machine Learning samples GitHub repository to explore a seasonality data anomaly detection sample.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d1060-270">dotnet/machinelearning-samples GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="d1060-270">dotnet/machinelearning-samples GitHub repository</span></span>](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/AnomalyDetection_PhoneCalls)
