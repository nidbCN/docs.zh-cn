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
# <a name="tutorial-analyze-sentiment-of-website-comments-with-binary-classification-in-mlnet"></a><span data-ttu-id="e4c26-104">教程：在 ML.NET 中使用二元分类分析网站评论的情绪</span><span class="sxs-lookup"><span data-stu-id="e4c26-104">Tutorial: Analyze sentiment of website comments with binary classification in ML.NET</span></span>

<span data-ttu-id="e4c26-105">本教程演示如何创建 .NET Core 控制台应用程序，该应用程序对网站评论情绪进行分类并采取适当的措施。</span><span class="sxs-lookup"><span data-stu-id="e4c26-105">This tutorial shows you how to create a .NET Core console application that classifies sentiment from website comments and takes the appropriate action.</span></span> <span data-ttu-id="e4c26-106">二元情绪分类器在 Visual Studio 2017 中使用 C#。</span><span class="sxs-lookup"><span data-stu-id="e4c26-106">The binary sentiment classifier uses C# in Visual Studio 2017.</span></span>

<span data-ttu-id="e4c26-107">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="e4c26-107">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> - <span data-ttu-id="e4c26-108">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="e4c26-108">Create a console application</span></span>
> - <span data-ttu-id="e4c26-109">准备数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-109">Prepare data</span></span>
> - <span data-ttu-id="e4c26-110">加载数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-110">Load the data</span></span>
> - <span data-ttu-id="e4c26-111">生成和定型模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-111">Build and train the model</span></span>
> - <span data-ttu-id="e4c26-112">评估模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-112">Evaluate the model</span></span>
> - <span data-ttu-id="e4c26-113">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="e4c26-113">Use the model to make a prediction</span></span>
> - <span data-ttu-id="e4c26-114">查看结果</span><span class="sxs-lookup"><span data-stu-id="e4c26-114">See the results</span></span>

<span data-ttu-id="e4c26-115">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="e4c26-115">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) repository.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e4c26-116">先决条件</span><span class="sxs-lookup"><span data-stu-id="e4c26-116">Prerequisites</span></span>

- <span data-ttu-id="e4c26-117">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2017 版本 15.6 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)</span><span class="sxs-lookup"><span data-stu-id="e4c26-117">[Visual Studio 2017 version 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the ".NET Core cross-platform development" workload installed</span></span>

- <span data-ttu-id="e4c26-118">[UCI Sentiment Labeled Sentences 数据集](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip)（zip 文件）</span><span class="sxs-lookup"><span data-stu-id="e4c26-118">[UCI Sentiment Labeled Sentences dataset](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip) (ZIP file)</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="e4c26-119">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="e4c26-119">Create a console application</span></span>

1. <span data-ttu-id="e4c26-120">创建名为“SentimentAnalysis”的 **.NET Core 控制台应用程序**。</span><span class="sxs-lookup"><span data-stu-id="e4c26-120">Create a **.NET Core Console Application** called "SentimentAnalysis".</span></span>

2. <span data-ttu-id="e4c26-121">在项目中创建名为“Data”的目录，用于保存数据集文件。</span><span class="sxs-lookup"><span data-stu-id="e4c26-121">Create a directory named *Data* in your project to save your data set files.</span></span>

3. <span data-ttu-id="e4c26-122">安装“Microsoft.ML NuGet 包”  ：</span><span class="sxs-lookup"><span data-stu-id="e4c26-122">Install the **Microsoft.ML NuGet Package**:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="e4c26-123">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="e4c26-123">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e4c26-124">选择“nuget.org”作为包源，然后选择“浏览”选项卡。搜索“Microsoft.ML”，选择所需的包，然后选择“安装”按钮 。</span><span class="sxs-lookup"><span data-stu-id="e4c26-124">Choose "nuget.org" as the package source, and then select the **Browse** tab. Search for **Microsoft.ML**, select the package you want, and then select the **Install** button.</span></span> <span data-ttu-id="e4c26-125">同意所选包的许可条款，继续执行安装。</span><span class="sxs-lookup"><span data-stu-id="e4c26-125">Proceed with the installation by agreeing to the license terms for the package you choose.</span></span>

## <a name="prepare-your-data"></a><span data-ttu-id="e4c26-126">准备数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-126">Prepare your data</span></span>

> [!NOTE]
> <span data-ttu-id="e4c26-127">本教程的数据集摘自 KDD 2015 中由 Kotzias 等</span><span class="sxs-lookup"><span data-stu-id="e4c26-127">The datasets for this tutorial are from the 'From Group to Individual Labels using Deep Features', Kotzias et.</span></span> <span data-ttu-id="e4c26-128">提出的“From Group to Individual Labels using Deep Features”，</span><span class="sxs-lookup"><span data-stu-id="e4c26-128">al,.</span></span> <span data-ttu-id="e4c26-129">并托管在 UCI 机器学习存储库中（Dua, D. 和 Karra Taniskidou, E.(2017)）。</span><span class="sxs-lookup"><span data-stu-id="e4c26-129">KDD 2015, and hosted at the UCI Machine Learning Repository - Dua, D. and Karra Taniskidou, E. (2017).</span></span> <span data-ttu-id="e4c26-130">UCI 机器学习存储库 [http://archive.ics.uci.edu/ml ]。</span><span class="sxs-lookup"><span data-stu-id="e4c26-130">UCI Machine Learning Repository [http://archive.ics.uci.edu/ml].</span></span> <span data-ttu-id="e4c26-131">加利福尼亚州，加利福尼亚大学：欧文分校，信息与计算机科学学院。</span><span class="sxs-lookup"><span data-stu-id="e4c26-131">Irvine, CA: University of California, School of Information and Computer Science.</span></span>

1. <span data-ttu-id="e4c26-132">下载 [UCI Sentiment Labeled Sentences 数据集 zip 文件](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip)并解压缩。</span><span class="sxs-lookup"><span data-stu-id="e4c26-132">Download [UCI Sentiment Labeled Sentences dataset ZIP file](http://archive.ics.uci.edu/ml/machine-learning-databases/00331/sentiment%20labelled%20sentences.zip), and unzip.</span></span>

2. <span data-ttu-id="e4c26-133">将 `yelp_labelled.txt` 文件复制到已创建的“Data”目录中。</span><span class="sxs-lookup"><span data-stu-id="e4c26-133">Copy the `yelp_labelled.txt` file into the *Data* directory you created.</span></span>

3. <span data-ttu-id="e4c26-134">在“解决方案资源管理器”中，右键单击 `yelp_labeled.txt` 文件并选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="e4c26-134">In Solution Explorer, right-click the `yelp_labeled.txt` file and select **Properties**.</span></span> <span data-ttu-id="e4c26-135">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。</span><span class="sxs-lookup"><span data-stu-id="e4c26-135">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="e4c26-136">创建类和定义路径</span><span class="sxs-lookup"><span data-stu-id="e4c26-136">Create classes and define paths</span></span>

1. <span data-ttu-id="e4c26-137">将以下附加的 `using` 语句添加到“Program.cs”文件顶部：</span><span class="sxs-lookup"><span data-stu-id="e4c26-137">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

    [!code-csharp[AddUsings](./snippets/sentiment-analysis/csharp/Program.cs#AddUsings "Add necessary usings")]

1. <span data-ttu-id="e4c26-138">将以下代码添加到 `Main` 方法正上方的行中，以创建字段来保存最近下载的数据集文件路径：</span><span class="sxs-lookup"><span data-stu-id="e4c26-138">Add the following code to the line right above the `Main` method, to create a field to hold the recently downloaded dataset file path:</span></span>

    [!code-csharp[Declare global variables](./snippets/sentiment-analysis/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

1. <span data-ttu-id="e4c26-139">接下来，为输入数据和预测结果创建类。</span><span class="sxs-lookup"><span data-stu-id="e4c26-139">Next, create classes for your input data and predictions.</span></span> <span data-ttu-id="e4c26-140">向项目添加一个新类：</span><span class="sxs-lookup"><span data-stu-id="e4c26-140">Add a new class to your project:</span></span>

    - <span data-ttu-id="e4c26-141">在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。</span><span class="sxs-lookup"><span data-stu-id="e4c26-141">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>

    - <span data-ttu-id="e4c26-142">在“添加新项”  对话框中，选择“类”  并将“名称”  字段更改为“SentimentData.cs”  。</span><span class="sxs-lookup"><span data-stu-id="e4c26-142">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *SentimentData.cs*.</span></span> <span data-ttu-id="e4c26-143">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e4c26-143">Then, select the **Add** button.</span></span>

1. <span data-ttu-id="e4c26-144">“SentimentData.cs”  文件随即在代码编辑器中打开。</span><span class="sxs-lookup"><span data-stu-id="e4c26-144">The *SentimentData.cs* file opens in the code editor.</span></span> <span data-ttu-id="e4c26-145">将下面的 `using` 语句添加到 SentimentData.cs 的顶部：</span><span class="sxs-lookup"><span data-stu-id="e4c26-145">Add the following `using` statement to the top of *SentimentData.cs*:</span></span>

    [!code-csharp[AddUsings](./snippets/sentiment-analysis/csharp/SentimentData.cs#AddUsings "Add necessary usings")]

1. <span data-ttu-id="e4c26-146">删除现有类定义并向“SentimentData.cs”文件添加以下代码，其中有两个类 `SentimentData` 和 `SentimentPrediction`：</span><span class="sxs-lookup"><span data-stu-id="e4c26-146">Remove the existing class definition and add the following code, which has two classes `SentimentData` and `SentimentPrediction`, to the *SentimentData.cs* file:</span></span>

    [!code-csharp[DeclareTypes](./snippets/sentiment-analysis/csharp/SentimentData.cs#DeclareTypes "Declare data record types")]

### <a name="how-the-data-was-prepared"></a><span data-ttu-id="e4c26-147">如何准备数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-147">How the data was prepared</span></span>

<span data-ttu-id="e4c26-148">输入数据集类 `SentimentData` 拥有一个用于用户评论 (`SentimentText`) 的 `string`，以及一个用于情绪的 `bool` (`Sentiment`)，值为 1（正面）或 0（负面）。</span><span class="sxs-lookup"><span data-stu-id="e4c26-148">The input dataset class, `SentimentData`, has a `string` for user comments (`SentimentText`) and a `bool` (`Sentiment`) value of either 1 (positive) or 0 (negative) for sentiment.</span></span> <span data-ttu-id="e4c26-149">这两个字段都附加了 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 特性，其描述了每个字段的数据文件顺序。</span><span class="sxs-lookup"><span data-stu-id="e4c26-149">Both fields have [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) attributes attached to them, which describes the data file order of each field.</span></span>  <span data-ttu-id="e4c26-150">此外，`Sentiment` 属性具有 [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute.%23ctor%2A) 特性，以将其指定为 `Label` 字段。</span><span class="sxs-lookup"><span data-stu-id="e4c26-150">In addition, the `Sentiment` property has a [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute.%23ctor%2A) attribute to designate it as the `Label` field.</span></span> <span data-ttu-id="e4c26-151">下面的示例文件没有标题行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e4c26-151">The following example file doesn't have a header row, and looks like this:</span></span>

|<span data-ttu-id="e4c26-152">情绪文本</span><span class="sxs-lookup"><span data-stu-id="e4c26-152">SentimentText</span></span>                         |<span data-ttu-id="e4c26-153">情绪（标签）</span><span class="sxs-lookup"><span data-stu-id="e4c26-153">Sentiment (Label)</span></span> |
|--------------------------------------|----------|
|<span data-ttu-id="e4c26-154">女服务员的服务速度有点慢。</span><span class="sxs-lookup"><span data-stu-id="e4c26-154">Waitress was a little slow in service.</span></span>|    <span data-ttu-id="e4c26-155">0</span><span class="sxs-lookup"><span data-stu-id="e4c26-155">0</span></span>     |
|<span data-ttu-id="e4c26-156">酥皮不行。</span><span class="sxs-lookup"><span data-stu-id="e4c26-156">Crust is not good.</span></span>                    |    <span data-ttu-id="e4c26-157">0</span><span class="sxs-lookup"><span data-stu-id="e4c26-157">0</span></span>     |
|<span data-ttu-id="e4c26-158">哇...喜欢这个地方。</span><span class="sxs-lookup"><span data-stu-id="e4c26-158">Wow... Loved this place.</span></span>              |    <span data-ttu-id="e4c26-159">1</span><span class="sxs-lookup"><span data-stu-id="e4c26-159">1</span></span>     |
|<span data-ttu-id="e4c26-160">服务很及时。</span><span class="sxs-lookup"><span data-stu-id="e4c26-160">Service was very prompt.</span></span>              |    <span data-ttu-id="e4c26-161">1</span><span class="sxs-lookup"><span data-stu-id="e4c26-161">1</span></span>     |

<span data-ttu-id="e4c26-162">`SentimentPrediction` 是在模型训练后使用的预测类。</span><span class="sxs-lookup"><span data-stu-id="e4c26-162">`SentimentPrediction` is the prediction class used after model training.</span></span> <span data-ttu-id="e4c26-163">它继承自 `SentimentData`，因此输入 `SentimentText` 可与输出预测结果一并显示。</span><span class="sxs-lookup"><span data-stu-id="e4c26-163">It inherits from `SentimentData` so that the input `SentimentText` can be displayed along with the output prediction.</span></span> <span data-ttu-id="e4c26-164">`Prediction` 布尔值是随附新的输入 `SentimentText` 提供时模型预测出的值。</span><span class="sxs-lookup"><span data-stu-id="e4c26-164">The `Prediction` boolean is the value that the model predicts when supplied with new input `SentimentText`.</span></span>

<span data-ttu-id="e4c26-165">输出类 `SentimentPrediction` 包含另外两个由模型计算得出的属性：`Score`（模型计算得出的原始分数）和 `Probability`（校准到具有积极情绪的文本几率的分数）。</span><span class="sxs-lookup"><span data-stu-id="e4c26-165">The output class `SentimentPrediction` contains two other properties calculated by the model: `Score` - the raw score calculated by the model, and `Probability` - the score calibrated to the likelihood of the text having positive sentiment.</span></span>

<span data-ttu-id="e4c26-166">在本教程中，最重要的属性是 `Prediction`。</span><span class="sxs-lookup"><span data-stu-id="e4c26-166">For this tutorial, the most important property is `Prediction`.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="e4c26-167">加载数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-167">Load the data</span></span>

<span data-ttu-id="e4c26-168">ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="e4c26-168">Data in ML.NET is represented as an [IDataView class](xref:Microsoft.ML.IDataView).</span></span> <span data-ttu-id="e4c26-169">`IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。</span><span class="sxs-lookup"><span data-stu-id="e4c26-169">`IDataView` is a flexible, efficient way of describing tabular data (numeric and text).</span></span> <span data-ttu-id="e4c26-170">可从文本文件或实时（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。</span><span class="sxs-lookup"><span data-stu-id="e4c26-170">Data can be loaded from a text file or in real time (for example, SQL database or log files) to an `IDataView` object.</span></span>

<span data-ttu-id="e4c26-171">[MLContext 类](xref:Microsoft.ML.MLContext)是所有 ML.NET 操作的起点。</span><span class="sxs-lookup"><span data-stu-id="e4c26-171">The [MLContext class](xref:Microsoft.ML.MLContext) is a starting point for all ML.NET operations.</span></span> <span data-ttu-id="e4c26-172">初始化 `mlContext` 会创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="e4c26-172">Initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="e4c26-173">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="e4c26-173">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

<span data-ttu-id="e4c26-174">准备应用，然后加载数据：</span><span class="sxs-lookup"><span data-stu-id="e4c26-174">You prepare the app, and then load data:</span></span>

1. <span data-ttu-id="e4c26-175">使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 mlContext 变量：</span><span class="sxs-lookup"><span data-stu-id="e4c26-175">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the mlContext variable:</span></span>

    [!code-csharp[CreateMLContext](./snippets/sentiment-analysis/csharp/Program.cs#CreateMLContext "Create the ML Context")]

2. <span data-ttu-id="e4c26-176">将以下代码作为下一行代码添加到 `Main()` 方法中：</span><span class="sxs-lookup"><span data-stu-id="e4c26-176">Add the following as the next line of code in the `Main()` method:</span></span>

    [!code-csharp[CallLoadData](./snippets/sentiment-analysis/csharp/Program.cs#CallLoadData)]

3. <span data-ttu-id="e4c26-177">使用下面的代码紧随 `Main()` 方法后创建 `LoadData()` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4c26-177">Create the `LoadData()` method, just after the `Main()` method, using the following code:</span></span>

    ```csharp
    public static TrainTestData LoadData(MLContext mlContext)
    {

    }
    ```

    <span data-ttu-id="e4c26-178">`LoadData()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="e4c26-178">The `LoadData()` method executes the following tasks:</span></span>

    - <span data-ttu-id="e4c26-179">加载数据。</span><span class="sxs-lookup"><span data-stu-id="e4c26-179">Loads the data.</span></span>
    - <span data-ttu-id="e4c26-180">将加载的数据集拆分为训练数据集和测试数据集。</span><span class="sxs-lookup"><span data-stu-id="e4c26-180">Splits the loaded dataset into train and test datasets.</span></span>
    - <span data-ttu-id="e4c26-181">返回拆分的训练数据集和测试数据集。</span><span class="sxs-lookup"><span data-stu-id="e4c26-181">Returns the split train and test datasets.</span></span>

4. <span data-ttu-id="e4c26-182">将以下代码添加为 `LoadData()` 方法的首行：</span><span class="sxs-lookup"><span data-stu-id="e4c26-182">Add the following code as the first line of the `LoadData()` method:</span></span>

    [!code-csharp[LoadData](./snippets/sentiment-analysis/csharp/Program.cs#LoadData "loading dataset")]

    <span data-ttu-id="e4c26-183">[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 方法用于定义数据架构并读取文件。</span><span class="sxs-lookup"><span data-stu-id="e4c26-183">The [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) method defines the data schema and reads in the file.</span></span> <span data-ttu-id="e4c26-184">它使用数据路径变量并返回 `IDataView`。</span><span class="sxs-lookup"><span data-stu-id="e4c26-184">It takes in the data path variables and returns an `IDataView`.</span></span>

### <a name="split-the-dataset-for-model-training-and-testing"></a><span data-ttu-id="e4c26-185">拆分数据集以进行模型训练和测试</span><span class="sxs-lookup"><span data-stu-id="e4c26-185">Split the dataset for model training and testing</span></span>

<span data-ttu-id="e4c26-186">准备模型时，使用部分数据集来训练它，并使用部分数据集来测试模型的准确性。</span><span class="sxs-lookup"><span data-stu-id="e4c26-186">When preparing a model, you use part of the dataset to train it and part of the dataset to test the model's accuracy.</span></span>

1. <span data-ttu-id="e4c26-187">要将加载的数据拆分为所需的数据集，请添加以下代码作为 `LoadData()` 方法中的下一行：</span><span class="sxs-lookup"><span data-stu-id="e4c26-187">To split the loaded data into the needed datasets, add the following code as the next line in the `LoadData()` method:</span></span>

    [!code-csharp[SplitData](./snippets/sentiment-analysis/csharp/Program.cs#SplitData "Split the Data")]

    <span data-ttu-id="e4c26-188">上述代码使用 [TrainTestSplit()](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit%2A) 方法将加载的数据集拆分为训练数据集和测试数据集，并在 <xref:Microsoft.ML.DataOperationsCatalog.TrainTestData> 类中返回它们。</span><span class="sxs-lookup"><span data-stu-id="e4c26-188">The previous code uses the [TrainTestSplit()](xref:Microsoft.ML.DataOperationsCatalog.TrainTestSplit%2A) method to split the loaded dataset into train and test datasets and return them in the <xref:Microsoft.ML.DataOperationsCatalog.TrainTestData> class.</span></span> <span data-ttu-id="e4c26-189">使用 `testFraction` 参数指定数据的测试集百分比。</span><span class="sxs-lookup"><span data-stu-id="e4c26-189">Specify the test set percentage of data with the `testFraction`parameter.</span></span> <span data-ttu-id="e4c26-190">默认值为 10%，在本例中使用 20%，以评估更多数据。</span><span class="sxs-lookup"><span data-stu-id="e4c26-190">The default is 10%, in this case you use 20% to evaluate more data.</span></span>

2. <span data-ttu-id="e4c26-191">在 `LoadData()` 方法末尾返回 `splitDataView`：</span><span class="sxs-lookup"><span data-stu-id="e4c26-191">Return the `splitDataView` at the end of the `LoadData()` method:</span></span>

    [!code-csharp[ReturnSplitData](./snippets/sentiment-analysis/csharp/Program.cs#ReturnSplitData)]

## <a name="build-and-train-the-model"></a><span data-ttu-id="e4c26-192">生成和定型模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-192">Build and train the model</span></span>

1. <span data-ttu-id="e4c26-193">将以下调用添加到 `BuildAndTrainModel` 方法作为 `Main()` 方法的下一行代码：</span><span class="sxs-lookup"><span data-stu-id="e4c26-193">Add the following call to the `BuildAndTrainModel`method as the next line of code in the `Main()` method:</span></span>

    [!code-csharp[CallBuildAndTrainModel](./snippets/sentiment-analysis/csharp/Program.cs#CallBuildAndTrainModel)]

    <span data-ttu-id="e4c26-194">`BuildAndTrainModel()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="e4c26-194">The `BuildAndTrainModel()` method executes the following tasks:</span></span>

    - <span data-ttu-id="e4c26-195">提取并转换数据。</span><span class="sxs-lookup"><span data-stu-id="e4c26-195">Extracts and transforms the data.</span></span>
    - <span data-ttu-id="e4c26-196">定型模型。</span><span class="sxs-lookup"><span data-stu-id="e4c26-196">Trains the model.</span></span>
    - <span data-ttu-id="e4c26-197">根据测试数据预测情绪。</span><span class="sxs-lookup"><span data-stu-id="e4c26-197">Predicts sentiment based on test data.</span></span>
    - <span data-ttu-id="e4c26-198">返回模型。</span><span class="sxs-lookup"><span data-stu-id="e4c26-198">Returns the model.</span></span>

2. <span data-ttu-id="e4c26-199">使用下面的代码紧随 `Main()` 方法后创建 `BuildAndTrainModel()` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4c26-199">Create the `BuildAndTrainModel()` method, just after the `Main()` method, using the following code:</span></span>

    ```csharp
    public static ITransformer BuildAndTrainModel(MLContext mlContext, IDataView splitTrainSet)
    {

    }
    ```

### <a name="extract-and-transform-the-data"></a><span data-ttu-id="e4c26-200">提取和转换数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-200">Extract and transform the data</span></span>

1. <span data-ttu-id="e4c26-201">将 `FeaturizeText` 作为下一行代码调用：</span><span class="sxs-lookup"><span data-stu-id="e4c26-201">Call `FeaturizeText` as the next line of code:</span></span>

    [!code-csharp[FeaturizeText](./snippets/sentiment-analysis/csharp/Program.cs#FeaturizeText "Featurize the text")]

    <span data-ttu-id="e4c26-202">上述代码中的 `FeaturizeText()` 方法将文本列 (`SentimentText`) 转换为机器学习算法使用的数字键类型的 `Features` 列，并将其作为新的数据集列添加：</span><span class="sxs-lookup"><span data-stu-id="e4c26-202">The `FeaturizeText()` method in the previous code converts the text column (`SentimentText`) into a numeric key type `Features` column used by the machine learning algorithm and adds it as a new dataset column:</span></span>

    |<span data-ttu-id="e4c26-203">情绪文本</span><span class="sxs-lookup"><span data-stu-id="e4c26-203">SentimentText</span></span>                         |<span data-ttu-id="e4c26-204">情绪</span><span class="sxs-lookup"><span data-stu-id="e4c26-204">Sentiment</span></span> |<span data-ttu-id="e4c26-205">特征</span><span class="sxs-lookup"><span data-stu-id="e4c26-205">Features</span></span>              |
    |--------------------------------------|----------|----------------------|
    |<span data-ttu-id="e4c26-206">女服务员的服务速度有点慢。</span><span class="sxs-lookup"><span data-stu-id="e4c26-206">Waitress was a little slow in service.</span></span>|    <span data-ttu-id="e4c26-207">0</span><span class="sxs-lookup"><span data-stu-id="e4c26-207">0</span></span>     |<span data-ttu-id="e4c26-208">[0.76, 0.65, 0.44, …]</span><span class="sxs-lookup"><span data-stu-id="e4c26-208">[0.76, 0.65, 0.44, …]</span></span> |
    |<span data-ttu-id="e4c26-209">酥皮不行。</span><span class="sxs-lookup"><span data-stu-id="e4c26-209">Crust is not good.</span></span>                    |    <span data-ttu-id="e4c26-210">0</span><span class="sxs-lookup"><span data-stu-id="e4c26-210">0</span></span>     |<span data-ttu-id="e4c26-211">[0.98, 0.43, 0.54, …]</span><span class="sxs-lookup"><span data-stu-id="e4c26-211">[0.98, 0.43, 0.54, …]</span></span> |
    |<span data-ttu-id="e4c26-212">哇...喜欢这个地方。</span><span class="sxs-lookup"><span data-stu-id="e4c26-212">Wow... Loved this place.</span></span>              |    <span data-ttu-id="e4c26-213">1</span><span class="sxs-lookup"><span data-stu-id="e4c26-213">1</span></span>     |<span data-ttu-id="e4c26-214">[0.35, 0.73, 0.46, …]</span><span class="sxs-lookup"><span data-stu-id="e4c26-214">[0.35, 0.73, 0.46, …]</span></span> |
    |<span data-ttu-id="e4c26-215">服务很及时。</span><span class="sxs-lookup"><span data-stu-id="e4c26-215">Service was very prompt.</span></span>              |    <span data-ttu-id="e4c26-216">1</span><span class="sxs-lookup"><span data-stu-id="e4c26-216">1</span></span>     |<span data-ttu-id="e4c26-217">[0.39, 0, 0.75, …]</span><span class="sxs-lookup"><span data-stu-id="e4c26-217">[0.39, 0, 0.75, …]</span></span>    |

### <a name="add-a-learning-algorithm"></a><span data-ttu-id="e4c26-218">添加学习算法</span><span class="sxs-lookup"><span data-stu-id="e4c26-218">Add a learning algorithm</span></span>

<span data-ttu-id="e4c26-219">此应用使用对数据项或数据行进行分类的分类算法。</span><span class="sxs-lookup"><span data-stu-id="e4c26-219">This app uses a classification algorithm that categorizes items or rows of data.</span></span> <span data-ttu-id="e4c26-220">应用将网站评论分类为正面评论或负面评论，因此，请使用二元分类任务。</span><span class="sxs-lookup"><span data-stu-id="e4c26-220">The app categorizes website comments as either positive or negative, so use the binary classification task.</span></span>

<span data-ttu-id="e4c26-221">将机器学习任务追加到数据转换定义中，方法是在 `BuildAndTrainModel()` 中添加以下代码作为下一行代码：</span><span class="sxs-lookup"><span data-stu-id="e4c26-221">Append the machine learning task to the data transformation definitions by adding the following as the next line of code in `BuildAndTrainModel()`:</span></span>

[!code-csharp[SdcaLogisticRegressionBinaryTrainer](./snippets/sentiment-analysis/csharp/Program.cs#AddTrainer "Add a SdcaLogisticRegressionBinaryTrainer")]

<span data-ttu-id="e4c26-222">[SdcaLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer) 是你的分类训练算法。</span><span class="sxs-lookup"><span data-stu-id="e4c26-222">The [SdcaLogisticRegressionBinaryTrainer](xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer) is your classification training algorithm.</span></span> <span data-ttu-id="e4c26-223">此算法会追加到 `estimator` 并接受特征化的 `SentimentText` (`Features`) 和 `Label` 输入参数，以便从历史数据中学习。</span><span class="sxs-lookup"><span data-stu-id="e4c26-223">This is appended to the `estimator` and accepts the featurized `SentimentText` (`Features`) and the `Label` input parameters to learn from the historic data.</span></span>

### <a name="train-the-model"></a><span data-ttu-id="e4c26-224">定型模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-224">Train the model</span></span>

<span data-ttu-id="e4c26-225">在 `BuildAndTrainModel()` 方法中添加以下代码作为下一代码行，使模型适应 `splitTrainSet` 数据，并返回经过训练的模型：</span><span class="sxs-lookup"><span data-stu-id="e4c26-225">Fit the model to the `splitTrainSet` data and return the trained model by adding the following as the next line of code in the `BuildAndTrainModel()` method:</span></span>

[!code-csharp[TrainModel](./snippets/sentiment-analysis/csharp/Program.cs#TrainModel "Train the model")]

<span data-ttu-id="e4c26-226">[Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) 方法通过转换数据集并应用训练来训练模型。</span><span class="sxs-lookup"><span data-stu-id="e4c26-226">The [Fit()](xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer.Fit%28Microsoft.ML.IDataView,Microsoft.ML.IDataView%29) method trains your model by transforming the dataset and applying the training.</span></span>

### <a name="return-the-model-trained-to-use-for-evaluation"></a><span data-ttu-id="e4c26-227">返回定型模型以用于评估</span><span class="sxs-lookup"><span data-stu-id="e4c26-227">Return the model trained to use for evaluation</span></span>

 <span data-ttu-id="e4c26-228">在 `BuildAndTrainModel()` 方法末尾返回模型：</span><span class="sxs-lookup"><span data-stu-id="e4c26-228">Return the model at the end of the `BuildAndTrainModel()` method:</span></span>

[!code-csharp[ReturnModel](./snippets/sentiment-analysis/csharp/Program.cs#ReturnModel "Return the model")]

## <a name="evaluate-the-model"></a><span data-ttu-id="e4c26-229">评估模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-229">Evaluate the model</span></span>

<span data-ttu-id="e4c26-230">训练模型后，使用测试数据验证模型的性能。</span><span class="sxs-lookup"><span data-stu-id="e4c26-230">After your model is trained, use your test data to validate the model's performance.</span></span>

1. <span data-ttu-id="e4c26-231">使用以下代码紧跟 `BuildAndTrainModel()` 之后创建 `Evaluate()` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4c26-231">Create the `Evaluate()` method, just after `BuildAndTrainModel()`, with the following code:</span></span>

    ```csharp
    public static void Evaluate(MLContext mlContext, ITransformer model, IDataView splitTestSet)
    {

    }
    ```

    <span data-ttu-id="e4c26-232">`Evaluate()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="e4c26-232">The `Evaluate()` method executes the following tasks:</span></span>

    - <span data-ttu-id="e4c26-233">加载测试数据集。</span><span class="sxs-lookup"><span data-stu-id="e4c26-233">Loads the test dataset.</span></span>
    - <span data-ttu-id="e4c26-234">创建 BinaryClassification 计算器。</span><span class="sxs-lookup"><span data-stu-id="e4c26-234">Creates the BinaryClassification evaluator.</span></span>
    - <span data-ttu-id="e4c26-235">评估模型并创建指标。</span><span class="sxs-lookup"><span data-stu-id="e4c26-235">Evaluates the model and creates metrics.</span></span>
    - <span data-ttu-id="e4c26-236">显示指标。</span><span class="sxs-lookup"><span data-stu-id="e4c26-236">Displays the metrics.</span></span>

2. <span data-ttu-id="e4c26-237">使用下面的代码，在 `BuildAndTrainModel()` 方法调用的正下方，从 `Main()` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="e4c26-237">Add a call to the new method from the `Main()` method, right under the `BuildAndTrainModel()` method call, using the following code:</span></span>

    [!code-csharp[CallEvaluate](./snippets/sentiment-analysis/csharp/Program.cs#CallEvaluate "Call the Evaluate method")]

3. <span data-ttu-id="e4c26-238">将以下代码添加到 `Evaluate()` 以转换 `splitTestSet` 数据：</span><span class="sxs-lookup"><span data-stu-id="e4c26-238">Transform the `splitTestSet` data by adding the following code to `Evaluate()`:</span></span>

    [!code-csharp[PredictWithTransformer](./snippets/sentiment-analysis/csharp/Program.cs#TransformData "Predict using the Transformer")]

    <span data-ttu-id="e4c26-239">之前的代码使用 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对测试数据集提供的多个输入行进行预测。</span><span class="sxs-lookup"><span data-stu-id="e4c26-239">The previous code uses the [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method to make predictions for multiple provided input rows of a test dataset.</span></span>

4. <span data-ttu-id="e4c26-240">通过在 `Evaluate()` 方法中添加以下代码作为下一代码行来评估模型：</span><span class="sxs-lookup"><span data-stu-id="e4c26-240">Evaluate the model by adding the following as the next line of code in the `Evaluate()` method:</span></span>

    [!code-csharp[ComputeMetrics](./snippets/sentiment-analysis/csharp/Program.cs#Evaluate "Compute Metrics")]

<span data-ttu-id="e4c26-241">获得预测集 (`predictions`) 后，[Evaluate()](xref:Microsoft.ML.BinaryClassificationCatalog.Evaluate%2A) 方法会对模型进行评估，其会将预测值与测试数据集中的实际 `Labels` 进行比较，并返回有关模型执行情况的 [CalibratedBinaryClassificationMetrics](xref:Microsoft.ML.Data.CalibratedBinaryClassificationMetrics) 对象。</span><span class="sxs-lookup"><span data-stu-id="e4c26-241">Once you have the prediction set (`predictions`), the [Evaluate()](xref:Microsoft.ML.BinaryClassificationCatalog.Evaluate%2A) method assesses the model, which compares the predicted values with the actual `Labels` in the test dataset and returns a [CalibratedBinaryClassificationMetrics](xref:Microsoft.ML.Data.CalibratedBinaryClassificationMetrics) object on how the model is performing.</span></span>

### <a name="displaying-the-metrics-for-model-validation"></a><span data-ttu-id="e4c26-242">显示用于模型验证的指标</span><span class="sxs-lookup"><span data-stu-id="e4c26-242">Displaying the metrics for model validation</span></span>

<span data-ttu-id="e4c26-243">使用以下代码显示指标：</span><span class="sxs-lookup"><span data-stu-id="e4c26-243">Use the following code to display the metrics:</span></span>

[!code-csharp[DisplayMetrics](./snippets/sentiment-analysis/csharp/Program.cs#DisplayMetrics "Display selected metrics")]

- <span data-ttu-id="e4c26-244">`Accuracy` 指标可获取模型的准确性，即测试集中正确预测所占的比例。</span><span class="sxs-lookup"><span data-stu-id="e4c26-244">The `Accuracy` metric gets the accuracy of a model, which is the proportion of correct predictions in the test set.</span></span>

- <span data-ttu-id="e4c26-245">`AreaUnderRocCurve` 指标指示模型对正面类和负面类进行正确分类的置信度。</span><span class="sxs-lookup"><span data-stu-id="e4c26-245">The `AreaUnderRocCurve` metric indicates how confident the model is correctly classifying the positive and negative classes.</span></span> <span data-ttu-id="e4c26-246">应该使 `AreaUnderRocCurve` 尽可能接近 1。</span><span class="sxs-lookup"><span data-stu-id="e4c26-246">You want the `AreaUnderRocCurve` to be as close to one as possible.</span></span>

- <span data-ttu-id="e4c26-247">`F1Score` 指标可获取模型的 F1 分数，该分数是[查准率](../resources/glossary.md#precision)和[查全率](../resources/glossary.md#recall)之间的平衡关系的度量值。</span><span class="sxs-lookup"><span data-stu-id="e4c26-247">The `F1Score` metric gets the model's F1 score, which is a measure of balance between [precision](../resources/glossary.md#precision) and [recall](../resources/glossary.md#recall).</span></span>  <span data-ttu-id="e4c26-248">应该使 `F1Score` 尽可能接近 1。</span><span class="sxs-lookup"><span data-stu-id="e4c26-248">You want the `F1Score` to be as close to one as possible.</span></span>

### <a name="predict-the-test-data-outcome"></a><span data-ttu-id="e4c26-249">预测测试数据结果</span><span class="sxs-lookup"><span data-stu-id="e4c26-249">Predict the test data outcome</span></span>

1. <span data-ttu-id="e4c26-250">使用下面的代码紧随 `Evaluate()` 方法后创建 `UseModelWithSingleItem()` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4c26-250">Create the `UseModelWithSingleItem()` method, just after the `Evaluate()` method, using the following code:</span></span>

    ```csharp
    private static void UseModelWithSingleItem(MLContext mlContext, ITransformer model)
    {

    }
    ```

    <span data-ttu-id="e4c26-251">`UseModelWithSingleItem()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="e4c26-251">The `UseModelWithSingleItem()` method executes the following tasks:</span></span>

    - <span data-ttu-id="e4c26-252">创建测试数据的单个注释。</span><span class="sxs-lookup"><span data-stu-id="e4c26-252">Creates a single comment of test data.</span></span>
    - <span data-ttu-id="e4c26-253">根据测试数据预测情绪。</span><span class="sxs-lookup"><span data-stu-id="e4c26-253">Predicts sentiment based on test data.</span></span>
    - <span data-ttu-id="e4c26-254">结合测试数据和预测进行报告。</span><span class="sxs-lookup"><span data-stu-id="e4c26-254">Combines test data and predictions for reporting.</span></span>
    - <span data-ttu-id="e4c26-255">显示预测结果。</span><span class="sxs-lookup"><span data-stu-id="e4c26-255">Displays the predicted results.</span></span>

2. <span data-ttu-id="e4c26-256">使用下面的代码，在 `Evaluate()` 方法调用的正下方，从 `Main()` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="e4c26-256">Add a call to the new method from the `Main()` method, right under the `Evaluate()` method call, using the following code:</span></span>

    [!code-csharp[CallUseModelWithSingleItem](./snippets/sentiment-analysis/csharp/Program.cs#CallUseModelWithSingleItem "Call the UseModelWithSingleItem method")]

3. <span data-ttu-id="e4c26-257">添加以下代码，以便作为 `UseModelWithSingleItem()` 方法中的第一行进行创建：</span><span class="sxs-lookup"><span data-stu-id="e4c26-257">Add the following code to create as the first line in the `UseModelWithSingleItem()` Method:</span></span>

    [!code-csharp[CreatePredictionEngine](./snippets/sentiment-analysis/csharp/Program.cs#CreatePredictionEngine1 "Create the PredictionEngine")]

    <span data-ttu-id="e4c26-258">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="e4c26-258">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to perform a prediction on a single instance of data.</span></span> <span data-ttu-id="e4c26-259">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="e4c26-259">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="e4c26-260">可以在单线程环境或原型环境中使用。</span><span class="sxs-lookup"><span data-stu-id="e4c26-260">It's acceptable to use in single-threaded or prototype environments.</span></span> <span data-ttu-id="e4c26-261">为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="e4c26-261">For improved performance and thread safety in production environments, use the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span> <span data-ttu-id="e4c26-262">请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。</span><span class="sxs-lookup"><span data-stu-id="e4c26-262">See this guide on how to [use `PredictionEnginePool` in an ASP.NET Core Web API](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).</span></span>

    > [!NOTE]
    > <span data-ttu-id="e4c26-263">`PredictionEnginePool` 服务扩展目前处于预览状态。</span><span class="sxs-lookup"><span data-stu-id="e4c26-263">`PredictionEnginePool` service extension is currently in preview.</span></span>

4. <span data-ttu-id="e4c26-264">通过创建一个 `SentimentData` 实例，在 `UseModelWithSingleItem()` 方法中添加一个注释来测试定型模型的预测：</span><span class="sxs-lookup"><span data-stu-id="e4c26-264">Add a comment to test the trained model's prediction in the `UseModelWithSingleItem()` method by creating an instance of `SentimentData`:</span></span>

    [!code-csharp[PredictionData](./snippets/sentiment-analysis/csharp/Program.cs#CreateTestIssue1 "Create test data for single prediction")]

5. <span data-ttu-id="e4c26-265">通过在 `UseModelWithSingleItem()` 方法中将以下代码作为下一行代码添加，将测试评论数据传递到 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)：</span><span class="sxs-lookup"><span data-stu-id="e4c26-265">Pass the test comment data to the [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) by adding the following as the next lines of code in the `UseModelWithSingleItem()` method:</span></span>

    [!code-csharp[Predict](./snippets/sentiment-analysis/csharp/Program.cs#Predict "Create a prediction of sentiment")]

    <span data-ttu-id="e4c26-266">[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单行数据进行预测。</span><span class="sxs-lookup"><span data-stu-id="e4c26-266">The [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) function makes a prediction on a single row of data.</span></span>

6. <span data-ttu-id="e4c26-267">使用以下代码显示 `SentimentText` 和相应的情绪预测：</span><span class="sxs-lookup"><span data-stu-id="e4c26-267">Display `SentimentText` and corresponding sentiment prediction using the following code:</span></span>

    [!code-csharp[OutputPrediction](./snippets/sentiment-analysis/csharp/Program.cs#OutputPrediction "Display prediction output")]

## <a name="use-the-model-for-prediction"></a><span data-ttu-id="e4c26-268">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="e4c26-268">Use the model for prediction</span></span>

### <a name="deploy-and-predict-batch-items"></a><span data-ttu-id="e4c26-269">部署和预测批项目</span><span class="sxs-lookup"><span data-stu-id="e4c26-269">Deploy and predict batch items</span></span>

1. <span data-ttu-id="e4c26-270">使用下面的代码紧随 `UseModelWithSingleItem()` 方法后创建 `UseModelWithBatchItems()` 方法：</span><span class="sxs-lookup"><span data-stu-id="e4c26-270">Create the `UseModelWithBatchItems()` method, just after the `UseModelWithSingleItem()` method, using the following code:</span></span>

    ```csharp
    public static void UseModelWithBatchItems(MLContext mlContext, ITransformer model)
    {

    }
    ```

    <span data-ttu-id="e4c26-271">`UseModelWithBatchItems()` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="e4c26-271">The `UseModelWithBatchItems()` method executes the following tasks:</span></span>

    - <span data-ttu-id="e4c26-272">创建批处理测试数据。</span><span class="sxs-lookup"><span data-stu-id="e4c26-272">Creates batch test data.</span></span>
    - <span data-ttu-id="e4c26-273">根据测试数据预测情绪。</span><span class="sxs-lookup"><span data-stu-id="e4c26-273">Predicts sentiment based on test data.</span></span>
    - <span data-ttu-id="e4c26-274">结合测试数据和预测进行报告。</span><span class="sxs-lookup"><span data-stu-id="e4c26-274">Combines test data and predictions for reporting.</span></span>
    - <span data-ttu-id="e4c26-275">显示预测结果。</span><span class="sxs-lookup"><span data-stu-id="e4c26-275">Displays the predicted results.</span></span>

2. <span data-ttu-id="e4c26-276">使用下面的代码，在 `UseModelWithSingleItem()` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="e4c26-276">Add a call to the new method from the `Main` method, right under the `UseModelWithSingleItem()` method call, using the following code:</span></span>

    [!code-csharp[CallPredictModelBatchItems](./snippets/sentiment-analysis/csharp/Program.cs#CallUseModelWithBatchItems "Call the CallUseModelWithBatchItems method")]

3. <span data-ttu-id="e4c26-277">添加一些评论，以测试 `UseModelWithBatchItems()` 方法中的定型模型预测：</span><span class="sxs-lookup"><span data-stu-id="e4c26-277">Add some comments to test the trained model's predictions in the `UseModelWithBatchItems()` method:</span></span>

    [!code-csharp[PredictionData](./snippets/sentiment-analysis/csharp/Program.cs#CreateTestIssues "Create test data for predictions")]

### <a name="predict-comment-sentiment"></a><span data-ttu-id="e4c26-278">预测评论情绪</span><span class="sxs-lookup"><span data-stu-id="e4c26-278">Predict comment sentiment</span></span>

<span data-ttu-id="e4c26-279">使用模型通过 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法预测评论数据情绪：</span><span class="sxs-lookup"><span data-stu-id="e4c26-279">Use the model to predict the comment data sentiment using the [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method:</span></span>

[!code-csharp[Predict](./snippets/sentiment-analysis/csharp/Program.cs#Prediction "Create predictions of sentiments")]

### <a name="combine-and-display-the-predictions"></a><span data-ttu-id="e4c26-280">合并并显示预测结果</span><span class="sxs-lookup"><span data-stu-id="e4c26-280">Combine and display the predictions</span></span>

<span data-ttu-id="e4c26-281">使用以下代码为预测创建标头：</span><span class="sxs-lookup"><span data-stu-id="e4c26-281">Create a header for the predictions using the following code:</span></span>

[!code-csharp[OutputHeaders](./snippets/sentiment-analysis/csharp/Program.cs#AddInfoMessage "Display prediction outputs")]

<span data-ttu-id="e4c26-282">由于 `SentimentPrediction` 继承自 `SentimentData`，`Transform()` 方法使用预测字段填充 `SentimentText`。</span><span class="sxs-lookup"><span data-stu-id="e4c26-282">Because `SentimentPrediction` is inherited from `SentimentData`, the `Transform()` method populated `SentimentText` with the predicted fields.</span></span> <span data-ttu-id="e4c26-283">随着 ML.NET 进程继续执行，每个组件会添加列，这让显示结果变得轻松：</span><span class="sxs-lookup"><span data-stu-id="e4c26-283">As the ML.NET process processes, each component adds columns, and this makes it easy to display the results:</span></span>

[!code-csharp[DisplayPredictions](./snippets/sentiment-analysis/csharp/Program.cs#DisplayResults "Display the predictions")]

## <a name="results"></a><span data-ttu-id="e4c26-284">结果</span><span class="sxs-lookup"><span data-stu-id="e4c26-284">Results</span></span>

<span data-ttu-id="e4c26-285">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="e4c26-285">Your results should be similar to the following.</span></span> <span data-ttu-id="e4c26-286">处理期间将显示消息。</span><span class="sxs-lookup"><span data-stu-id="e4c26-286">During processing, messages are displayed.</span></span> <span data-ttu-id="e4c26-287">你可能会看到警告或处理消息。</span><span class="sxs-lookup"><span data-stu-id="e4c26-287">You may see warnings, or processing messages.</span></span> <span data-ttu-id="e4c26-288">为清楚起见，已经从下面的结果中删除这些内容。</span><span class="sxs-lookup"><span data-stu-id="e4c26-288">These have been removed from the following results for clarity.</span></span>

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

<span data-ttu-id="e4c26-289">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="e4c26-289">Congratulations!</span></span> <span data-ttu-id="e4c26-290">现在，你已成功生成用于分类和预测消息情绪的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="e4c26-290">You've now successfully built a machine learning model for classifying and predicting messages sentiment.</span></span>

<span data-ttu-id="e4c26-291">生成成功的模型是一个迭代过程。</span><span class="sxs-lookup"><span data-stu-id="e4c26-291">Building successful models is an iterative process.</span></span> <span data-ttu-id="e4c26-292">由于本教程使用小型数据集来提供快速模型训练，因此该模型的初始质量较低。</span><span class="sxs-lookup"><span data-stu-id="e4c26-292">This model has initial lower quality as the tutorial uses small datasets to provide quick model training.</span></span> <span data-ttu-id="e4c26-293">如果对模型质量不满意，可以通过尝试提供更大的训练数据集，或通过为每种算法选择具有不同[超参数](../resources/glossary.md#hyperparameter)的不同训练算法来改进它。</span><span class="sxs-lookup"><span data-stu-id="e4c26-293">If you aren't satisfied with the model quality, you can try to improve it by providing larger training datasets or by choosing different training algorithms with different [hyper-parameters](../resources/glossary.md#hyperparameter) for each algorithm.</span></span>

<span data-ttu-id="e4c26-294">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="e4c26-294">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e4c26-295">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e4c26-295">Next steps</span></span>

<span data-ttu-id="e4c26-296">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="e4c26-296">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> - <span data-ttu-id="e4c26-297">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="e4c26-297">Create a console application</span></span>
> - <span data-ttu-id="e4c26-298">准备数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-298">Prepare data</span></span>
> - <span data-ttu-id="e4c26-299">加载数据</span><span class="sxs-lookup"><span data-stu-id="e4c26-299">Load the data</span></span>
> - <span data-ttu-id="e4c26-300">生成和定型模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-300">Build and train the model</span></span>
> - <span data-ttu-id="e4c26-301">评估模型</span><span class="sxs-lookup"><span data-stu-id="e4c26-301">Evaluate the model</span></span>
> - <span data-ttu-id="e4c26-302">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="e4c26-302">Use the model to make a prediction</span></span>
> - <span data-ttu-id="e4c26-303">查看结果</span><span class="sxs-lookup"><span data-stu-id="e4c26-303">See the results</span></span>

<span data-ttu-id="e4c26-304">进入下一教程了解详细信息</span><span class="sxs-lookup"><span data-stu-id="e4c26-304">Advance to the next tutorial to learn more</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e4c26-305">问题分类</span><span class="sxs-lookup"><span data-stu-id="e4c26-305">Issue Classification</span></span>](github-issue-classification.md)
