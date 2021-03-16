---
title: 教程：检测电话呼叫中的异常
description: 了解如何为时序数据构建异常检测应用程序。 本教程将使用 Visual Studio 2019 和 C# 创建 .NET Core 控制台应用程序。
ms.date: 12/04/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 7edb84ae53f1da7903cf4b3f77d215206ffbf1ef
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259819"
---
# <a name="tutorial-detect-anomalies-in-time-series-with-mlnet"></a><span data-ttu-id="3d38a-104">教程：使用 ML.NET 检测时序中的异常</span><span class="sxs-lookup"><span data-stu-id="3d38a-104">Tutorial: Detect anomalies in time series with ML.NET</span></span>

<span data-ttu-id="3d38a-105">了解如何为时序数据构建异常检测应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d38a-105">Learn how to build an anomaly detection application for time series data.</span></span> <span data-ttu-id="3d38a-106">本教程将使用 Visual Studio 2019 和 C# 创建 .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d38a-106">This tutorial creates a .NET Core console application using C# in Visual Studio 2019.</span></span>

<span data-ttu-id="3d38a-107">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="3d38a-107">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="3d38a-108">加载数据</span><span class="sxs-lookup"><span data-stu-id="3d38a-108">Load the data</span></span>
> * <span data-ttu-id="3d38a-109">检测时序的周期</span><span class="sxs-lookup"><span data-stu-id="3d38a-109">Detect period for a time series</span></span>
> * <span data-ttu-id="3d38a-110">检测周期性时序的异常</span><span class="sxs-lookup"><span data-stu-id="3d38a-110">Detect anomaly for a periodical time series</span></span>

<span data-ttu-id="3d38a-111">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/PhoneCallsAnomalyDetection) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="3d38a-111">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/PhoneCallsAnomalyDetection) repository.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3d38a-112">先决条件</span><span class="sxs-lookup"><span data-stu-id="3d38a-112">Prerequisites</span></span>

* <span data-ttu-id="3d38a-113">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019 16.7.8 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。</span><span class="sxs-lookup"><span data-stu-id="3d38a-113">[Visual Studio 2019 version 16.7.8 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the ".NET Core cross-platform development" workload installed.</span></span>

* <span data-ttu-id="3d38a-114">[phone-calls.csv 数据集](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/getting-started/AnomalyDetection_PhoneCalls/SrEntireDetection/Data/phone-calls.csv)。</span><span class="sxs-lookup"><span data-stu-id="3d38a-114">[The phone-calls.csv dataset](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/getting-started/AnomalyDetection_PhoneCalls/SrEntireDetection/Data/phone-calls.csv).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="3d38a-115">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="3d38a-115">Create a console application</span></span>

1. <span data-ttu-id="3d38a-116">创建一个名为“PhoneCallsAnomalyDetection”的 C# .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d38a-116">Create a **C# .NET Core Console Application** called "PhoneCallsAnomalyDetection".</span></span>

2. <span data-ttu-id="3d38a-117">在项目中创建名为“Data”的目录，用于保存数据集文件。</span><span class="sxs-lookup"><span data-stu-id="3d38a-117">Create a directory named *Data* in your project to save your data set files.</span></span>

3. <span data-ttu-id="3d38a-118">安装“Microsoft.ML NuGet 包”  ：</span><span class="sxs-lookup"><span data-stu-id="3d38a-118">Install the **Microsoft.ML NuGet Package**:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="3d38a-119">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="3d38a-119">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="3d38a-120">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，再选择“安装”按钮 。</span><span class="sxs-lookup"><span data-stu-id="3d38a-120">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML** and select the **Install** button.</span></span> <span data-ttu-id="3d38a-121">选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。</span><span class="sxs-lookup"><span data-stu-id="3d38a-121">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span> <span data-ttu-id="3d38a-122">对“Microsoft.ML.TimeSeries”重复这些步骤。</span><span class="sxs-lookup"><span data-stu-id="3d38a-122">Repeat these steps for **Microsoft.ML.TimeSeries**.</span></span>

4. <span data-ttu-id="3d38a-123">在 Program.cs 文件的顶部添加以下 `using` 语句：</span><span class="sxs-lookup"><span data-stu-id="3d38a-123">Add the following `using` statements at the top of your *Program.cs* file:</span></span>

    [!code-csharp[AddUsings](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#AddUsings "Add necessary usings")]

### <a name="download-your-data"></a><span data-ttu-id="3d38a-124">下载数据</span><span class="sxs-lookup"><span data-stu-id="3d38a-124">Download your data</span></span>

1. <span data-ttu-id="3d38a-125">下载数据集并将其保存到之前创建的 *Data* 文件夹中：</span><span class="sxs-lookup"><span data-stu-id="3d38a-125">Download the dataset and save it to the *Data* folder you previously created:</span></span>

    <span data-ttu-id="3d38a-126">右键单击 [phone-calls.csv](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_PhoneCalls/SrEntireDetection/Data/phone-calls.csv) 并选择“将链接(或目标)另存为…”</span><span class="sxs-lookup"><span data-stu-id="3d38a-126">Right click on [*phone-calls.csv*](https://raw.githubusercontent.com/dotnet/machinelearning-samples/master/samples/csharp/getting-started/AnomalyDetection_PhoneCalls/SrEntireDetection/Data/phone-calls.csv) and select "Save Link (or Target) As..."</span></span>

     <span data-ttu-id="3d38a-127">确保将 \*.csv 文件保存到 *Data* 文件夹，或者在将其保存到其他位置后，将 \*.csv 文件移动到 *Data* 文件夹。</span><span class="sxs-lookup"><span data-stu-id="3d38a-127">Make sure you either save the \*.csv file to the *Data* folder, or after you save it elsewhere, move the \*.csv file to the *Data* folder.</span></span>

2. <span data-ttu-id="3d38a-128">在解决方案资源管理器中，右键单击 \*.csv 文件并选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="3d38a-128">In Solution Explorer, right-click the \*.csv file and select **Properties**.</span></span> <span data-ttu-id="3d38a-129">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”  。</span><span class="sxs-lookup"><span data-stu-id="3d38a-129">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

<span data-ttu-id="3d38a-130">下表是来自 \*.csv 文件的数据预览：</span><span class="sxs-lookup"><span data-stu-id="3d38a-130">The following table is a data preview from your \*.csv file:</span></span>

| <span data-ttu-id="3d38a-131">timestamp</span><span class="sxs-lookup"><span data-stu-id="3d38a-131">timestamp</span></span>  | <span data-ttu-id="3d38a-132">值</span><span class="sxs-lookup"><span data-stu-id="3d38a-132">value</span></span> |
|--------|--------------|
| <span data-ttu-id="3d38a-133">2018/9/3</span><span class="sxs-lookup"><span data-stu-id="3d38a-133">2018/9/3</span></span>  | <span data-ttu-id="3d38a-134">36.69670857</span><span class="sxs-lookup"><span data-stu-id="3d38a-134">36.69670857</span></span>  |
| <span data-ttu-id="3d38a-135">2018/9/4</span><span class="sxs-lookup"><span data-stu-id="3d38a-135">2018/9/4</span></span>  | <span data-ttu-id="3d38a-136">35.74160571</span><span class="sxs-lookup"><span data-stu-id="3d38a-136">35.74160571</span></span>  |
| <span data-ttu-id="3d38a-137">.....</span><span class="sxs-lookup"><span data-stu-id="3d38a-137">.....</span></span>  | <span data-ttu-id="3d38a-138">.....</span><span class="sxs-lookup"><span data-stu-id="3d38a-138">.....</span></span>  |
| <span data-ttu-id="3d38a-139">2018/10/3</span><span class="sxs-lookup"><span data-stu-id="3d38a-139">2018/10/3</span></span>  | <span data-ttu-id="3d38a-140">34.49893429</span><span class="sxs-lookup"><span data-stu-id="3d38a-140">34.49893429</span></span>  |
| <span data-ttu-id="3d38a-141">...</span><span class="sxs-lookup"><span data-stu-id="3d38a-141">...</span></span>    | <span data-ttu-id="3d38a-142">....</span><span class="sxs-lookup"><span data-stu-id="3d38a-142">....</span></span>   |

<span data-ttu-id="3d38a-143">此文件表示时序。</span><span class="sxs-lookup"><span data-stu-id="3d38a-143">This file represents a time-series.</span></span> <span data-ttu-id="3d38a-144">文件中的每一行都是一个数据点。</span><span class="sxs-lookup"><span data-stu-id="3d38a-144">Each row in the file is a data point.</span></span> <span data-ttu-id="3d38a-145">每个数据点都有两个属性，分别是 `timestamp` 和 `value`，表示每天的电话呼叫次数。</span><span class="sxs-lookup"><span data-stu-id="3d38a-145">Each data point has two attributes, namely, `timestamp` and `value`, to represent the number of phone calls at each day.</span></span> <span data-ttu-id="3d38a-146">电话呼叫次数转换为非敏感数据。</span><span class="sxs-lookup"><span data-stu-id="3d38a-146">The number of phone calls is transformed to de-sensitivity.</span></span>

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="3d38a-147">创建类和定义路径</span><span class="sxs-lookup"><span data-stu-id="3d38a-147">Create classes and define paths</span></span>

<span data-ttu-id="3d38a-148">接下来，定义输入和预测类数据结构。</span><span class="sxs-lookup"><span data-stu-id="3d38a-148">Next, define your input and prediction class data structures.</span></span>

<span data-ttu-id="3d38a-149">向项目添加一个新类：</span><span class="sxs-lookup"><span data-stu-id="3d38a-149">Add a new class to your project:</span></span>

1. <span data-ttu-id="3d38a-150">在“解决方案资源管理器”中，右键单击该项目，然后选择“添加”>“新项” 。</span><span class="sxs-lookup"><span data-stu-id="3d38a-150">In **Solution Explorer**, right-click the project, and then select **Add > New Item**.</span></span>

2. <span data-ttu-id="3d38a-151">在“添加新项”对话框中，选择“类”并将“名称”字段更改为 PhoneCallsData.cs  。</span><span class="sxs-lookup"><span data-stu-id="3d38a-151">In the **Add New Item dialog box**, select **Class** and change the **Name** field to *PhoneCallsData.cs*.</span></span> <span data-ttu-id="3d38a-152">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="3d38a-152">Then, select the **Add** button.</span></span>

   <span data-ttu-id="3d38a-153">此时，将在代码编辑器中打开 PhoneCallsData.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="3d38a-153">The *PhoneCallsData.cs* file opens in the code editor.</span></span>

3. <span data-ttu-id="3d38a-154">将以下 `using` 语句添加到 PhoneCallsData.cs 顶部：</span><span class="sxs-lookup"><span data-stu-id="3d38a-154">Add the following `using` statement to the top of *PhoneCallsData.cs*:</span></span>

   ```csharp
   using Microsoft.ML.Data;
   ```

4. <span data-ttu-id="3d38a-155">删除现有类定义并向 PhoneCallsData.cs 文件添加以下代码，其中包含两个类 `PhoneCallsData` 和 `PhoneCallsPrediction`：</span><span class="sxs-lookup"><span data-stu-id="3d38a-155">Remove the existing class definition and add the following code, which has two classes `PhoneCallsData` and `PhoneCallsPrediction`, to the *PhoneCallsData.cs* file:</span></span>

    [!code-csharp[DeclareTypes](./snippets/phone-calls-anomaly-detection/csharp/PhoneCallsData.cs#DeclareTypes "Declare data record types")]

    <span data-ttu-id="3d38a-156">`PhoneCallsData` 指定输入数据类。</span><span class="sxs-lookup"><span data-stu-id="3d38a-156">`PhoneCallsData` specifies an input data class.</span></span> <span data-ttu-id="3d38a-157">[LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性指定应加载数据集中的哪些列（按列索引）。</span><span class="sxs-lookup"><span data-stu-id="3d38a-157">The [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) attribute specifies which columns (by column index) in the dataset should be loaded.</span></span> <span data-ttu-id="3d38a-158">它包含两个属性：`timestamp` 和 `value`，分别对应于数据文件中的相同属性。</span><span class="sxs-lookup"><span data-stu-id="3d38a-158">It has two attributes `timestamp` and `value` that correspond to the same attributes in the data file.</span></span>

    <span data-ttu-id="3d38a-159">`PhoneCallsPrediction` 指定预测数据类。</span><span class="sxs-lookup"><span data-stu-id="3d38a-159">`PhoneCallsPrediction` specifies the prediction data class.</span></span> <span data-ttu-id="3d38a-160">对于 SR-CNN 检测器，预测取决于指定的[检测模式](xref:Microsoft.ML.TimeSeries.SrCnnDetectMode)。</span><span class="sxs-lookup"><span data-stu-id="3d38a-160">For SR-CNN detector, the prediction depends on the [detect mode](xref:Microsoft.ML.TimeSeries.SrCnnDetectMode) specified.</span></span> <span data-ttu-id="3d38a-161">在此示例中，我们选择 `AnomalyAndMargin` 模式。</span><span class="sxs-lookup"><span data-stu-id="3d38a-161">In this sample, we select the `AnomalyAndMargin` mode.</span></span> <span data-ttu-id="3d38a-162">输出包含七列。</span><span class="sxs-lookup"><span data-stu-id="3d38a-162">The output contains seven columns.</span></span> <span data-ttu-id="3d38a-163">在大多数情况下，`IsAnomaly`、`ExpectedValue`、`UpperBoundary` 和 `LowerBoundary` 能够提供足够的信息。</span><span class="sxs-lookup"><span data-stu-id="3d38a-163">In most cases, `IsAnomaly`, `ExpectedValue`, `UpperBoundary`, and `LowerBoundary` are informative enough.</span></span> <span data-ttu-id="3d38a-164">它们会告诉你某个点是否异常，该点的预期值以及该点的上下边界区域。</span><span class="sxs-lookup"><span data-stu-id="3d38a-164">They tell you if a point is an anomaly, the expected value of the point and the lower / upper boundary region of the point.</span></span>

5. <span data-ttu-id="3d38a-165">将以下代码添加到 `Main` 方法正上方的行中，以指定数据文件的路径：</span><span class="sxs-lookup"><span data-stu-id="3d38a-165">Add the following code to the line right above the `Main` method to specify the path to your data file:</span></span>

    [!code-csharp[Declare global variables](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

### <a name="initialize-variables-in-main"></a><span data-ttu-id="3d38a-166">在 Main 中初始化变量</span><span class="sxs-lookup"><span data-stu-id="3d38a-166">Initialize variables in Main</span></span>

1. <span data-ttu-id="3d38a-167">使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 `mlContext` 变量：</span><span class="sxs-lookup"><span data-stu-id="3d38a-167">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the `mlContext` variable:</span></span>

    [!code-csharp[CreateMLContext](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CreateMLContext "Create the ML Context")]

    <span data-ttu-id="3d38a-168">执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="3d38a-168">The [MLContext class](xref:Microsoft.ML.MLContext) is a starting point for all ML.NET operations, and initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="3d38a-169">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="3d38a-169">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

### <a name="load-the-data"></a><span data-ttu-id="3d38a-170">加载数据</span><span class="sxs-lookup"><span data-stu-id="3d38a-170">Load the data</span></span>

<span data-ttu-id="3d38a-171">ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="3d38a-171">Data in ML.NET is represented as an [IDataView class](xref:Microsoft.ML.IDataView).</span></span> <span data-ttu-id="3d38a-172">`IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。</span><span class="sxs-lookup"><span data-stu-id="3d38a-172">`IDataView` is a flexible, efficient way of describing tabular data (numeric and text).</span></span> <span data-ttu-id="3d38a-173">可从文本文件或其他源（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。</span><span class="sxs-lookup"><span data-stu-id="3d38a-173">Data can be loaded from a text file or from other sources (for example, SQL database or log files) to an `IDataView` object.</span></span>

1. <span data-ttu-id="3d38a-174">添加以下代码作为 `Main` 方法的下一行：</span><span class="sxs-lookup"><span data-stu-id="3d38a-174">Add the following code as the next line of the `Main` method:</span></span>

    [!code-csharp[LoadData](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#LoadData "loading dataset")]

    <span data-ttu-id="3d38a-175">[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 用于定义数据架构并读取文件。</span><span class="sxs-lookup"><span data-stu-id="3d38a-175">The [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) defines the data schema and reads in the file.</span></span> <span data-ttu-id="3d38a-176">它使用数据路径变量并返回 `IDataView`。</span><span class="sxs-lookup"><span data-stu-id="3d38a-176">It takes in the data path variables and returns an `IDataView`.</span></span>

## <a name="time-series-anomaly-detection"></a><span data-ttu-id="3d38a-177">时序异常情况检测</span><span class="sxs-lookup"><span data-stu-id="3d38a-177">Time series anomaly detection</span></span>

<span data-ttu-id="3d38a-178">时序异常情况检测是检测时序数据离群值的过程；在给定的输入时序上指向“怪异”或不是预期行为的行为。</span><span class="sxs-lookup"><span data-stu-id="3d38a-178">Time series anomaly detection is the process of detecting time-series data outliers; points on a given input time-series where the behavior isn't what was expected, or "weird".</span></span> <span data-ttu-id="3d38a-179">这些异常通常表示问题领域中的一些重要事件：用户帐户受到网络攻击、断电、服务器上的突发 RPS、内存泄漏等。</span><span class="sxs-lookup"><span data-stu-id="3d38a-179">These anomalies are typically indicative of some events of interest in the problem domain: a cyber-attack on user accounts, power outage, bursting RPS on a server, memory leak, etc.</span></span>

<span data-ttu-id="3d38a-180">要找到时序上的异常，应首先确定时序的周期。</span><span class="sxs-lookup"><span data-stu-id="3d38a-180">To find anomaly on time series, you should first determine the period of the series.</span></span> <span data-ttu-id="3d38a-181">然后，时序可以分为几个部分，以 `Y = T + S + R` 为例，其中 `Y` 是最初序列，`T` 是趋势部分，`S` 是周期性部分，而 `R` 是该时序的剩余部分。</span><span class="sxs-lookup"><span data-stu-id="3d38a-181">Then, the time series can be decomposed into several components as `Y = T + S + R`, where `Y` is the original series, `T` is the trend component, `S` is the seasonal component, and `R` is the residual component of the series.</span></span> <span data-ttu-id="3d38a-182">此步骤称为[分解](https://en.wikipedia.org/wiki/Decomposition_of_time_series)。</span><span class="sxs-lookup"><span data-stu-id="3d38a-182">This step is called [decomposition](https://en.wikipedia.org/wiki/Decomposition_of_time_series).</span></span> <span data-ttu-id="3d38a-183">最后，对剩余部分进行检测以发现异常。</span><span class="sxs-lookup"><span data-stu-id="3d38a-183">Finally, detection is performed on the residual component to find the anomalies.</span></span> <span data-ttu-id="3d38a-184">在 ML.NET 中，SR-CNN 算法是一种新颖的高级算法，它基于频谱残差 (SR) 和卷积神经网络 (CNN) 来检测时序上的异常。</span><span class="sxs-lookup"><span data-stu-id="3d38a-184">In ML.NET, The SR-CNN algorithm is an advanced and novel algorithm that is based on Spectral Residual (SR) and Convolutional Neural Network(CNN) to detect anomaly on time-series.</span></span> <span data-ttu-id="3d38a-185">有关此算法的详细信息，请参阅 [Microsoft 的时序异常检测服务](https://arxiv.org/pdf/1906.03821.pdf)。</span><span class="sxs-lookup"><span data-stu-id="3d38a-185">For more information on this algorithm, see [Time-Series Anomaly Detection Service at Microsoft](https://arxiv.org/pdf/1906.03821.pdf).</span></span>

<span data-ttu-id="3d38a-186">在本教程中，你将了解可以使用两个函数来完成这些过程。</span><span class="sxs-lookup"><span data-stu-id="3d38a-186">In this tutorial, you will see that these procedures can be completed using two functions.</span></span>

## <a name="detect-period"></a><span data-ttu-id="3d38a-187">检测周期</span><span class="sxs-lookup"><span data-stu-id="3d38a-187">Detect Period</span></span>

<span data-ttu-id="3d38a-188">在第一步中，我们调用 `DetectSeasonality` 函数来确定时序的周期。</span><span class="sxs-lookup"><span data-stu-id="3d38a-188">In the first step, we invoke the `DetectSeasonality` function to determine the period of the series.</span></span>

### <a name="create-the-detectperiod-method"></a><span data-ttu-id="3d38a-189">创建 DetectPeriod 方法</span><span class="sxs-lookup"><span data-stu-id="3d38a-189">Create the DetectPeriod method</span></span>

1. <span data-ttu-id="3d38a-190">使用以下代码在 `Main` 方法下创建 `DetectPeriod` 方法：</span><span class="sxs-lookup"><span data-stu-id="3d38a-190">Create the `DetectPeriod` method, just below the `Main` method, using the following code:</span></span>

    ```csharp
    static void DetectPeriod(MLContext mlContext, IDataView phoneCalls)
    {

    }
    ```

2. <span data-ttu-id="3d38a-191">使用 <xref:Microsoft.ML.TimeSeriesCatalog.DetectSeasonality%2A> 函数检测周期。</span><span class="sxs-lookup"><span data-stu-id="3d38a-191">Use the <xref:Microsoft.ML.TimeSeriesCatalog.DetectSeasonality%2A> function to detect period.</span></span> <span data-ttu-id="3d38a-192">使用以下代码将其添加到 `DetectPeriod` 方法中：</span><span class="sxs-lookup"><span data-stu-id="3d38a-192">Add it to the `DetectPeriod` method with the following code:</span></span>

    [!code-csharp[DetectSeasonality](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DetectSeasonality)]

3. <span data-ttu-id="3d38a-193">将以下内容添加为 `DetectPeriod` 方法的下一行代码，以显示周期值：</span><span class="sxs-lookup"><span data-stu-id="3d38a-193">Display the period value by adding the following as the next line of code in the `DetectPeriod` method:</span></span>

    [!code-csharp[DisplayPeriod](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DisplayPeriod)]

4. <span data-ttu-id="3d38a-194">将以下调用添加到 `Main` 方法中的 `DetectPeriod` 方法：</span><span class="sxs-lookup"><span data-stu-id="3d38a-194">Add the following call to the `DetectPeriod` method in the `Main` method:</span></span>

    [!code-csharp[CallDetectPeriod](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CallDetectPeriod)]

### <a name="period-detection-results"></a><span data-ttu-id="3d38a-195">周期检测结果</span><span class="sxs-lookup"><span data-stu-id="3d38a-195">Period detection results</span></span>

<span data-ttu-id="3d38a-196">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d38a-196">Run the application.</span></span> <span data-ttu-id="3d38a-197">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="3d38a-197">Your results should be similar to the following.</span></span>

```console
Period of the series is: 7.
```

## <a name="detect-anomaly"></a><span data-ttu-id="3d38a-198">检测异常</span><span class="sxs-lookup"><span data-stu-id="3d38a-198">Detect Anomaly</span></span>

<span data-ttu-id="3d38a-199">在此步骤中，你将使用 <xref:Microsoft.ML.TimeSeriesCatalog.DetectEntireAnomalyBySrCnn%2A> 方法来查找异常。</span><span class="sxs-lookup"><span data-stu-id="3d38a-199">In this step, you use the <xref:Microsoft.ML.TimeSeriesCatalog.DetectEntireAnomalyBySrCnn%2A> method to find anomalies.</span></span>

### <a name="create-the-detectanomaly-method"></a><span data-ttu-id="3d38a-200">创建 DetectAnomaly 方法</span><span class="sxs-lookup"><span data-stu-id="3d38a-200">Create the DetectAnomaly method</span></span>

1. <span data-ttu-id="3d38a-201">使用以下代码在 `DetectPeriod` 方法下创建 `DetectAnomaly` 方法：</span><span class="sxs-lookup"><span data-stu-id="3d38a-201">Create the `DetectAnomaly` method, just below the `DetectPeriod` method, using the following code:</span></span>

    ```csharp
    static void DetectAnomaly(MLContext mlContext, IDataView phoneCalls, int period)
    {

    }
    ```

2. <span data-ttu-id="3d38a-202">使用以下代码在 `DetectAnomaly` 方法中设置 <xref:Microsoft.ML.TimeSeries.SrCnnEntireAnomalyDetectorOptions>：</span><span class="sxs-lookup"><span data-stu-id="3d38a-202">Set up <xref:Microsoft.ML.TimeSeries.SrCnnEntireAnomalyDetectorOptions> in the `DetectAnomaly` method with the following code:</span></span>

    [!code-csharp[SetupSrCnnParameters](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#SetupSrCnnParameters)]

3. <span data-ttu-id="3d38a-203">在 `DetectAnomaly` 方法中添加以下代码行，以使用 SR-CNN 算法检测异常：</span><span class="sxs-lookup"><span data-stu-id="3d38a-203">Detect anomaly by SR-CNN algorithm by adding the following line of code in the `DetectAnomaly` method:</span></span>

    [!code-csharp[DetectAnomaly](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DetectAnomaly)]

4. <span data-ttu-id="3d38a-204">使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法和以下代码将输出数据视图转换为强类型 `IEnumerable`，以方便显示：</span><span class="sxs-lookup"><span data-stu-id="3d38a-204">Convert the output data view into a strongly typed `IEnumerable` for easier display using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method with the following code:</span></span>

    [!code-csharp[CreateEnumerableForResult](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CreateEnumerableForResult)]

5. <span data-ttu-id="3d38a-205">使用以下代码创建显示标头，用作 `DetectAnomaly` 方法中的下一行：</span><span class="sxs-lookup"><span data-stu-id="3d38a-205">Create a display header with the following code as the next line in the `DetectAnomaly` method:</span></span>

    [!code-csharp[DisplayHeader](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DisplayHeader)]

    <span data-ttu-id="3d38a-206">将在更改点检测结果中显示以下信息：</span><span class="sxs-lookup"><span data-stu-id="3d38a-206">You'll display the following information in your change point detection results:</span></span>

    * <span data-ttu-id="3d38a-207">`Index` 是每个点的索引。</span><span class="sxs-lookup"><span data-stu-id="3d38a-207">`Index` is the index of each point.</span></span>
    * <span data-ttu-id="3d38a-208">`Anomaly` 是判断每个点是否被检测为异常的指标。</span><span class="sxs-lookup"><span data-stu-id="3d38a-208">`Anomaly` is the indicator of whether each point is detected as anomaly.</span></span>
    * <span data-ttu-id="3d38a-209">`ExpectedValue` 是每个点的估计值。</span><span class="sxs-lookup"><span data-stu-id="3d38a-209">`ExpectedValue` is the estimated value of each point.</span></span>
    * <span data-ttu-id="3d38a-210">`LowerBoundary` 是非异常的每个点的最小值。</span><span class="sxs-lookup"><span data-stu-id="3d38a-210">`LowerBoundary` is the lowest value each point can be to be not anomaly.</span></span>
    * <span data-ttu-id="3d38a-211">`UpperBoundary` 是非异常的每个点的最大值。</span><span class="sxs-lookup"><span data-stu-id="3d38a-211">`UpperBoundary` is the highest value each point can be to be not anomaly.</span></span>

6. <span data-ttu-id="3d38a-212">使用以下代码循环访问 `predictions` `IEnumerable` 并显示结果：</span><span class="sxs-lookup"><span data-stu-id="3d38a-212">Iterate through the `predictions` `IEnumerable` and display the results with the following code:</span></span>

    [!code-csharp[DisplayAnomalyDetectionResults](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DisplayAnomalyDetectionResults)]

7. <span data-ttu-id="3d38a-213">将以下调用添加到 `Main` 方法中的 `DetectAnomaly` 方法：</span><span class="sxs-lookup"><span data-stu-id="3d38a-213">Add the following call to the `DetectAnomaly` method in the `Main` method:</span></span>

    [!code-csharp[CallDetectAnomaly](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CallDetectAnomaly)]

## <a name="anomaly-detection-results"></a><span data-ttu-id="3d38a-214">异常情况检测结果</span><span class="sxs-lookup"><span data-stu-id="3d38a-214">Anomaly detection results</span></span>

<span data-ttu-id="3d38a-215">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="3d38a-215">Run the application.</span></span> <span data-ttu-id="3d38a-216">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="3d38a-216">Your results should be similar to the following.</span></span> <span data-ttu-id="3d38a-217">处理期间将显示消息。</span><span class="sxs-lookup"><span data-stu-id="3d38a-217">During processing, messages are displayed.</span></span> <span data-ttu-id="3d38a-218">你可能会看到警告或处理消息。</span><span class="sxs-lookup"><span data-stu-id="3d38a-218">You may see warnings or processing messages.</span></span> <span data-ttu-id="3d38a-219">为清楚起见，已从以下结果中删除某些消息。</span><span class="sxs-lookup"><span data-stu-id="3d38a-219">Some messages have been removed from the following results for clarity.</span></span>

```console
Detect period of the series
Period of the series is: 7.
Detect anomaly points in the series
Index   Data    Anomaly AnomalyScore    Mag     ExpectedValue   BoundaryUnit    UpperBoundary   LowerBoundary
0,0,36.841787256739266,41.14206982401966,32.541504689458876
1,0,35.67303618137362,39.97331874865401,31.372753614093227
2,0,34.710132999891826,39.029491313022824,30.390774686760828
3,0,33.44765248883495,37.786086547816545,29.10921842985335
4,0,28.937110922276364,33.25646923540736,24.61775260914537
5,0,5.143895892785781,9.444178460066171,0.843613325505391
6,0,5.163325228419392,9.463607795699783,0.8630426611390014
7,0,36.76414836240396,41.06443092968435,32.46386579512357
8,0,35.77908590657007,40.07936847385046,31.478803339289676
9,0,34.547259536635245,38.847542103915636,30.246976969354854
10,0,33.55193524820608,37.871293561337076,29.23257693507508
11,0,29.091800129624648,33.392082696905035,24.79151756234426
12,0,5.154836630338823,9.455119197619213,0.8545540630584334
13,0,5.234332502492464,9.534615069772855,0.934049935212073
14,0,36.54992549471526,40.85020806199565,32.24964292743487
15,0,35.79526470980883,40.095547277089224,31.494982142528443
16,0,34.34099013096804,38.64127269824843,30.040707563687647
17,0,33.61201516582131,37.9122977331017,29.31173259854092
18,0,29.223563320561812,33.5238458878422,24.923280753281425
19,0,5.170512168851533,9.470794736131923,0.8702296015711433
20,0,5.2614938889462834,9.561776456226674,0.9612113216658926
21,0,36.37103858487317,40.67132115215356,32.07075601759278
22,0,35.813544599026855,40.113827166307246,31.513262031746464
23,0,34.05600492733225,38.356287494612644,29.755722360051863
24,0,33.65828319077884,37.95856575805923,29.358000623498448
25,0,29.381125690882463,33.681408258162854,25.080843123602072
26,0,5.261543539820418,9.561826107100808,0.9612609725400283
27,0,5.4873712582971805,9.787653825577571,1.1870886910167897
28,1,36.504694001629254,40.804976568909645,32.20441143434886  <-- alert is on, detected anomaly
...
```

<span data-ttu-id="3d38a-220">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="3d38a-220">Congratulations!</span></span> <span data-ttu-id="3d38a-221">你现在已成功构建机器学习模型，用于检测周期性时序中的周期和异常。</span><span class="sxs-lookup"><span data-stu-id="3d38a-221">You've now successfully built machine learning models for detecting period and anomaly on a periodical series.</span></span>

<span data-ttu-id="3d38a-222">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/PhoneCallsAnomalyDetection) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="3d38a-222">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/master/machine-learning/tutorials/PhoneCallsAnomalyDetection) repository.</span></span>

<span data-ttu-id="3d38a-223">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="3d38a-223">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="3d38a-224">加载数据</span><span class="sxs-lookup"><span data-stu-id="3d38a-224">Load the data</span></span>
> * <span data-ttu-id="3d38a-225">检测时序数据的周期</span><span class="sxs-lookup"><span data-stu-id="3d38a-225">Detect period on the time series data</span></span>
> * <span data-ttu-id="3d38a-226">检测时序数据中的异常</span><span class="sxs-lookup"><span data-stu-id="3d38a-226">Detect anomaly on the time series data</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d38a-227">后续步骤</span><span class="sxs-lookup"><span data-stu-id="3d38a-227">Next steps</span></span>

<span data-ttu-id="3d38a-228">请查看机器学习示例 GitHub 存储库，以探索能耗异常情况检测示例。</span><span class="sxs-lookup"><span data-stu-id="3d38a-228">Check out the Machine Learning samples GitHub repository to explore a Power Consumption Anomaly Detection sample.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="3d38a-229">dotnet/machinelearning-samples GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="3d38a-229">dotnet/machinelearning-samples GitHub repository</span></span>](https://github.com/dotnet/machinelearning-samples/tree/master/samples/csharp/getting-started/AnomalyDetection_PowerMeterReadings)
