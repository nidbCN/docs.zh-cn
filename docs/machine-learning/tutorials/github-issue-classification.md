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
# <a name="tutorial-categorize-support-issues-using-multiclass-classification-with-mlnet"></a><span data-ttu-id="967d7-103">教程：将多类分类与 ML.NET 配合使用，对支持问题分类</span><span class="sxs-lookup"><span data-stu-id="967d7-103">Tutorial: Categorize support issues using multiclass classification with ML.NET</span></span>

<span data-ttu-id="967d7-104">本示例教程演示如何使用 ML.NET 创建 GitHub 问题分类器来训练模型，使其通过 Visual Studio 中使用 C# 的 .NET Core 控制台应用程序为 GitHub 问题分类和预测区域标签。</span><span class="sxs-lookup"><span data-stu-id="967d7-104">This sample tutorial illustrates using ML.NET to create a GitHub issue classifier to train a model that classifies and predicts the Area label for a GitHub issue via a .NET Core console application using C# in Visual Studio.</span></span>

<span data-ttu-id="967d7-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="967d7-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="967d7-106">准备数据</span><span class="sxs-lookup"><span data-stu-id="967d7-106">Prepare your data</span></span>
> * <span data-ttu-id="967d7-107">转换数据</span><span class="sxs-lookup"><span data-stu-id="967d7-107">Transform the data</span></span>
> * <span data-ttu-id="967d7-108">定型模型</span><span class="sxs-lookup"><span data-stu-id="967d7-108">Train the model</span></span>
> * <span data-ttu-id="967d7-109">评估模型</span><span class="sxs-lookup"><span data-stu-id="967d7-109">Evaluate the model</span></span>
> * <span data-ttu-id="967d7-110">使用训练的模型预测</span><span class="sxs-lookup"><span data-stu-id="967d7-110">Predict with the trained model</span></span>
> * <span data-ttu-id="967d7-111">使用加载模型部署和预测</span><span class="sxs-lookup"><span data-stu-id="967d7-111">Deploy and Predict with a loaded model</span></span>

<span data-ttu-id="967d7-112">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/GitHubIssueClassification) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="967d7-112">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/GitHubIssueClassification) repository.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="967d7-113">先决条件</span><span class="sxs-lookup"><span data-stu-id="967d7-113">Prerequisites</span></span>

* <span data-ttu-id="967d7-114">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="967d7-114">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" workload installed.</span></span>
* <span data-ttu-id="967d7-115">[GitHub 问题制表符分隔文件 (issues_train.tsv)](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv)。</span><span class="sxs-lookup"><span data-stu-id="967d7-115">The [GitHub issues tab separated file (issues_train.tsv)](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv).</span></span>
* <span data-ttu-id="967d7-116">[GitHub 问题测试制表符分隔文件 (issues_test.tsv)](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv)。</span><span class="sxs-lookup"><span data-stu-id="967d7-116">The [GitHub issues test tab separated file (issues_test.tsv)](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="967d7-117">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="967d7-117">Create a console application</span></span>

### <a name="create-a-project"></a><span data-ttu-id="967d7-118">创建项目</span><span class="sxs-lookup"><span data-stu-id="967d7-118">Create a project</span></span>

1. <span data-ttu-id="967d7-119">打开 Visual Studio 2017。</span><span class="sxs-lookup"><span data-stu-id="967d7-119">Open Visual Studio 2017.</span></span> <span data-ttu-id="967d7-120">从菜单栏中选择“文件” > “新建” > “项目”。</span><span class="sxs-lookup"><span data-stu-id="967d7-120">Select **File** > **New** > **Project** from the menu bar.</span></span> <span data-ttu-id="967d7-121">在“新项目”对话框中，依次选择“Visual C#”和“.NET Core”节点。</span><span class="sxs-lookup"><span data-stu-id="967d7-121">In the **New Project** dialog, select the **Visual C#** node followed by the **.NET Core** node.</span></span> <span data-ttu-id="967d7-122">然后，选择“控制台应用程序(.NET Core)”  项目模板。</span><span class="sxs-lookup"><span data-stu-id="967d7-122">Then select the **Console App (.NET Core)** project template.</span></span> <span data-ttu-id="967d7-123">在“名称”文本框中，键入“GitHubIssueClassification”，然后选择“确定”按钮。</span><span class="sxs-lookup"><span data-stu-id="967d7-123">In the **Name** text box, type "GitHubIssueClassification" and then select the **OK** button.</span></span>

2. <span data-ttu-id="967d7-124">在项目中创建一个名为“Data”的目录来保存数据集文件：</span><span class="sxs-lookup"><span data-stu-id="967d7-124">Create a directory named *Data* in your project to save your data set files:</span></span>

    <span data-ttu-id="967d7-125">在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”。</span><span class="sxs-lookup"><span data-stu-id="967d7-125">In **Solution Explorer**, right-click on your project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="967d7-126">键入“Data”，然后按 Enter。</span><span class="sxs-lookup"><span data-stu-id="967d7-126">Type "Data" and hit Enter.</span></span>

3. <span data-ttu-id="967d7-127">在项目中创建一个名为“Models”的目录来保存模型：</span><span class="sxs-lookup"><span data-stu-id="967d7-127">Create a directory named *Models* in your project to save your model:</span></span>

    <span data-ttu-id="967d7-128">在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”。</span><span class="sxs-lookup"><span data-stu-id="967d7-128">In **Solution Explorer**, right-click on your project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="967d7-129">键入“Models”，然后按 Enter。</span><span class="sxs-lookup"><span data-stu-id="967d7-129">Type "Models" and hit Enter.</span></span>

4. <span data-ttu-id="967d7-130">安装“Microsoft.ML NuGet 包”  ：</span><span class="sxs-lookup"><span data-stu-id="967d7-130">Install the **Microsoft.ML NuGet Package**:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="967d7-131">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="967d7-131">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="967d7-132">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，再选择“安装”按钮 。</span><span class="sxs-lookup"><span data-stu-id="967d7-132">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML** and select the **Install** button.</span></span> <span data-ttu-id="967d7-133">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="967d7-133">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

### <a name="prepare-your-data"></a><span data-ttu-id="967d7-134">准备数据</span><span class="sxs-lookup"><span data-stu-id="967d7-134">Prepare your data</span></span>

1. <span data-ttu-id="967d7-135">下载 [issues_train.tsv](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) 和 [issues_test.tsv](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) 数据集，并将它们保存到先前创建的“Data”文件夹。</span><span class="sxs-lookup"><span data-stu-id="967d7-135">Download the [issues_train.tsv](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_train.tsv) and the [issues_test.tsv](https://raw.githubusercontent.com/dotnet/samples/main/machine-learning/tutorials/GitHubIssueClassification/Data/issues_test.tsv) data sets and save them to the *Data* folder previously created.</span></span> <span data-ttu-id="967d7-136">第一个数据集用于定型机器学习模型，第二个数据集可用来评估模型的准确度。</span><span class="sxs-lookup"><span data-stu-id="967d7-136">The first dataset trains the machine learning model and the second can be used to evaluate how accurate your model is.</span></span>

2. <span data-ttu-id="967d7-137">在“解决方案资源管理器”中，右键单击每个 \*.tsv 文件，然后选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="967d7-137">In Solution Explorer, right-click each of the \*.tsv files and select **Properties**.</span></span> <span data-ttu-id="967d7-138">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。</span><span class="sxs-lookup"><span data-stu-id="967d7-138">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="967d7-139">创建类和定义路径</span><span class="sxs-lookup"><span data-stu-id="967d7-139">Create classes and define paths</span></span>

<span data-ttu-id="967d7-140">将以下附加的 `using` 语句添加到“Program.cs”文件顶部：</span><span class="sxs-lookup"><span data-stu-id="967d7-140">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

[!code-csharp[AddUsings](./snippets/github-issue-classification/csharp/Program.cs#AddUsings)]

<span data-ttu-id="967d7-141">创建 3 个全局字段，来保存最近下载的文件的路径以及 `MLContext`、`DataView` 和 `PredictionEngine` 的全局变量：</span><span class="sxs-lookup"><span data-stu-id="967d7-141">Create three global fields to hold the paths to the recently downloaded files, and global variables for the `MLContext`,`DataView`, and `PredictionEngine`:</span></span>

* <span data-ttu-id="967d7-142">`_trainDataPath` 具有用于定型模型的数据集路径。</span><span class="sxs-lookup"><span data-stu-id="967d7-142">`_trainDataPath` has the path to the dataset used to train the model.</span></span>
* <span data-ttu-id="967d7-143">`_testDataPath` 具有用于评估模型的数据集路径。</span><span class="sxs-lookup"><span data-stu-id="967d7-143">`_testDataPath` has the path to the dataset used to evaluate the model.</span></span>
* <span data-ttu-id="967d7-144">`_modelPath` 具有在其中保存定型模型的路径。</span><span class="sxs-lookup"><span data-stu-id="967d7-144">`_modelPath` has the path where the trained model is saved.</span></span>
* <span data-ttu-id="967d7-145">`_mlContext` 是用于提供处理上下文的 <xref:Microsoft.ML.MLContext>。</span><span class="sxs-lookup"><span data-stu-id="967d7-145">`_mlContext` is the <xref:Microsoft.ML.MLContext> that provides processing context.</span></span>
* <span data-ttu-id="967d7-146">`_trainingDataView` 是用于处理定型数据集的 <xref:Microsoft.ML.IDataView>。</span><span class="sxs-lookup"><span data-stu-id="967d7-146">`_trainingDataView` is the <xref:Microsoft.ML.IDataView> used to process the training dataset.</span></span>
* <span data-ttu-id="967d7-147">`_predEngine` 是用于单个预测的 <xref:Microsoft.ML.PredictionEngine%602>。</span><span class="sxs-lookup"><span data-stu-id="967d7-147">`_predEngine` is the <xref:Microsoft.ML.PredictionEngine%602> used for single predictions.</span></span>

<span data-ttu-id="967d7-148">将以下代码添加到 `Main` 方法正上方的行中以指定这些路径和其他变量：</span><span class="sxs-lookup"><span data-stu-id="967d7-148">Add the following code to the line directly above the `Main` method to specify those paths and the other variables:</span></span>

[!code-csharp[DeclareGlobalVariables](./snippets/github-issue-classification/csharp/Program.cs#DeclareGlobalVariables)]

<span data-ttu-id="967d7-149">为输入数据和预测创建一些类。</span><span class="sxs-lookup"><span data-stu-id="967d7-149">Create some classes for your input data and predictions.</span></span> <span data-ttu-id="967d7-150">向项目添加一个新类：</span><span class="sxs-lookup"><span data-stu-id="967d7-150">Add a new class to your project:</span></span>

1. <span data-ttu-id="967d7-151">在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。</span><span class="sxs-lookup"><span data-stu-id="967d7-151">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="967d7-152">在“添加新项”对话框中，选择“类”并将“名称”字段更改为“GitHubIssueData.cs”  。</span><span class="sxs-lookup"><span data-stu-id="967d7-152">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *GitHubIssueData.cs*.</span></span> <span data-ttu-id="967d7-153">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="967d7-153">Then, select the **Add** button.</span></span>

    <span data-ttu-id="967d7-154">“GitHubIssueData.cs”文件随即在代码编辑器中打开。</span><span class="sxs-lookup"><span data-stu-id="967d7-154">The *GitHubIssueData.cs* file opens in the code editor.</span></span> <span data-ttu-id="967d7-155">将下面的 `using` 语句添加到 GitHubIssueData.cs 的顶部：</span><span class="sxs-lookup"><span data-stu-id="967d7-155">Add the following `using` statement to the top of *GitHubIssueData.cs*:</span></span>

[!code-csharp[AddUsings](./snippets/github-issue-classification/csharp/GitHubIssueData.cs#AddUsings)]

<span data-ttu-id="967d7-156">删除现有类定义并向“GitHubIssueData.cs”文件添加以下代码，其中有两个类 `GitHubIssue` 和 `IssuePrediction`：</span><span class="sxs-lookup"><span data-stu-id="967d7-156">Remove the existing class definition and add the following code, which has two classes `GitHubIssue` and `IssuePrediction`, to the *GitHubIssueData.cs* file:</span></span>

[!code-csharp[DeclareGlobalVariables](./snippets/github-issue-classification/csharp/GitHubIssueData.cs#DeclareTypes)]

<span data-ttu-id="967d7-157">`label` 是要预测的列。</span><span class="sxs-lookup"><span data-stu-id="967d7-157">The `label` is the column you want to predict.</span></span> <span data-ttu-id="967d7-158">标识的 `Features` 是为模型提供的用来预测标签的输入。</span><span class="sxs-lookup"><span data-stu-id="967d7-158">The identified `Features` are the inputs you give the model to predict the Label.</span></span>

<span data-ttu-id="967d7-159">使用 [LoadColumnAttribute](xref:Microsoft.ML.Data.LoadColumnAttribute) 在数据集中指定源列的索引。</span><span class="sxs-lookup"><span data-stu-id="967d7-159">Use the [LoadColumnAttribute](xref:Microsoft.ML.Data.LoadColumnAttribute) to specify the indices of the source columns in the data set.</span></span>

<span data-ttu-id="967d7-160">`GitHubIssue` 是输入数据集类，具有以下 <xref:System.String> 字段：</span><span class="sxs-lookup"><span data-stu-id="967d7-160">`GitHubIssue` is the input dataset class and has the following <xref:System.String> fields:</span></span>

* <span data-ttu-id="967d7-161">第一列 `ID`（GitHub 问题 ID）</span><span class="sxs-lookup"><span data-stu-id="967d7-161">the first column `ID` (GitHub Issue ID)</span></span>
* <span data-ttu-id="967d7-162">第二列 `Area`（定型预测）</span><span class="sxs-lookup"><span data-stu-id="967d7-162">the second column `Area` (the prediction for training)</span></span>
* <span data-ttu-id="967d7-163">第三列 `Title`（GitHub 问题标题）是用于预测 `Area` 的第一个 `feature`</span><span class="sxs-lookup"><span data-stu-id="967d7-163">the third column `Title` (GitHub issue title) is the first `feature` used for predicting the `Area`</span></span>
* <span data-ttu-id="967d7-164">第四列 `Description` 是用于预测 `Area` 的第二个 `feature`</span><span class="sxs-lookup"><span data-stu-id="967d7-164">the fourth column  `Description` is the second `feature` used for predicting the `Area`</span></span>

<span data-ttu-id="967d7-165">`IssuePrediction` 是在定型模型后用于预测的类。</span><span class="sxs-lookup"><span data-stu-id="967d7-165">`IssuePrediction` is the class used for prediction after the model has been trained.</span></span> <span data-ttu-id="967d7-166">它有一个 `string` (`Area`) 和一个 `PredictedLabel` `ColumnName` 属性。</span><span class="sxs-lookup"><span data-stu-id="967d7-166">It has a single `string` (`Area`) and a `PredictedLabel` `ColumnName` attribute.</span></span>  <span data-ttu-id="967d7-167">`PredictedLabel` 在预测和评估过程中使用。</span><span class="sxs-lookup"><span data-stu-id="967d7-167">The `PredictedLabel` is used during prediction and evaluation.</span></span> <span data-ttu-id="967d7-168">对于计算，将使用带定型数据的输入、预测值和模型。</span><span class="sxs-lookup"><span data-stu-id="967d7-168">For evaluation, an input with training data, the predicted values, and the model are used.</span></span>

<span data-ttu-id="967d7-169">所有 ML.NET 操作都从 [MLContext](xref:Microsoft.ML.MLContext) 类开始。</span><span class="sxs-lookup"><span data-stu-id="967d7-169">All ML.NET operations start in the [MLContext](xref:Microsoft.ML.MLContext) class.</span></span> <span data-ttu-id="967d7-170">初始化 `mlContext` 创建了新的 ML.NET 环境，可以在模型创建工作流对象之间共享。</span><span class="sxs-lookup"><span data-stu-id="967d7-170">Initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="967d7-171">从概念上讲，它与 `Entity Framework` 中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="967d7-171">It's similar, conceptually, to `DBContext` in `Entity Framework`.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="967d7-172">在 Main 中初始化变量</span><span class="sxs-lookup"><span data-stu-id="967d7-172">Initialize variables in Main</span></span>

<span data-ttu-id="967d7-173">使用具有随机种子 (`seed: 0`) 的新实例 `MLContext` 初始化 `_mlContext` 全局变量，以获得跨多个定型的可重复/确定性结果。</span><span class="sxs-lookup"><span data-stu-id="967d7-173">Initialize the `_mlContext` global variable  with a new instance of `MLContext` with a random seed (`seed: 0`) for repeatable/deterministic results across multiple trainings.</span></span>  <span data-ttu-id="967d7-174">用下面 `Main` 方法中的代码替换 `Console.WriteLine("Hello World!")` 行：</span><span class="sxs-lookup"><span data-stu-id="967d7-174">Replace the `Console.WriteLine("Hello World!")` line with the following code in the `Main` method:</span></span>

[!code-csharp[CreateMLContext](./snippets/github-issue-classification/csharp/Program.cs#CreateMLContext)]

## <a name="load-the-data"></a><span data-ttu-id="967d7-175">加载数据</span><span class="sxs-lookup"><span data-stu-id="967d7-175">Load the data</span></span>

<span data-ttu-id="967d7-176">ML.NET 使用 [IDataView 类](xref:Microsoft.ML.IDataView)灵活、有效地描述数字或文本表格数据。</span><span class="sxs-lookup"><span data-stu-id="967d7-176">ML.NET uses the [IDataView class](xref:Microsoft.ML.IDataView) as a flexible, efficient way of describing numeric or text tabular data.</span></span> <span data-ttu-id="967d7-177">`IDataView` 可以加载文本文件或进行实时加载（例如，SQL 数据库或日志文件）。</span><span class="sxs-lookup"><span data-stu-id="967d7-177">`IDataView` can load either text files or in real time (for example, SQL database or log files).</span></span>

<span data-ttu-id="967d7-178">要初始化并加载 `_trainingDataView` 全局变量以将其用于管道，请在 `mlContext` 初始化后添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="967d7-178">To initialize and load the `_trainingDataView` global variable in order to use it for the pipeline, add the following code after the  `mlContext` initialization:</span></span>

[!code-csharp[LoadTrainData](./snippets/github-issue-classification/csharp/Program.cs#LoadTrainData)]

<span data-ttu-id="967d7-179">[LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 用于定义数据架构并读取文件。</span><span class="sxs-lookup"><span data-stu-id="967d7-179">The [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) defines the data schema and reads in the file.</span></span> <span data-ttu-id="967d7-180">它使用数据路径变量并返回 `IDataView`。</span><span class="sxs-lookup"><span data-stu-id="967d7-180">It takes in the data path variables and returns an `IDataView`.</span></span>

<span data-ttu-id="967d7-181">将以下代码作为下一行代码添加到 `Main` 方法中：</span><span class="sxs-lookup"><span data-stu-id="967d7-181">Add the following as the next line of code in the `Main` method:</span></span>

[!code-csharp[CallProcessData](./snippets/github-issue-classification/csharp/Program.cs#CallProcessData)]

<span data-ttu-id="967d7-182">`ProcessData` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="967d7-182">The `ProcessData` method executes the following tasks:</span></span>

* <span data-ttu-id="967d7-183">提取并转换数据。</span><span class="sxs-lookup"><span data-stu-id="967d7-183">Extracts and transforms the data.</span></span>
* <span data-ttu-id="967d7-184">返回处理管道。</span><span class="sxs-lookup"><span data-stu-id="967d7-184">Returns the processing pipeline.</span></span>

<span data-ttu-id="967d7-185">使用下面的代码紧随 `Main` 方法后创建 `ProcessData` 方法：</span><span class="sxs-lookup"><span data-stu-id="967d7-185">Create the `ProcessData` method, just after the `Main` method, using the following code:</span></span>

```csharp
public static IEstimator<ITransformer> ProcessData()
{

}
```

## <a name="extract-features-and-transform-the-data"></a><span data-ttu-id="967d7-186">提取功能和转换数据</span><span class="sxs-lookup"><span data-stu-id="967d7-186">Extract Features and transform the data</span></span>

<span data-ttu-id="967d7-187">由于要预测 `GitHubIssue` 的区域 GitHub 标签，因此请使用 [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 方法将 `Area` 列转换为数字键类型 `Label` 列（分类算法所接受的格式）并将其添加为新的数据集列：</span><span class="sxs-lookup"><span data-stu-id="967d7-187">As you want to predict the Area GitHub label for a `GitHubIssue`, use the [MapValueToKey()](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) method to transform the `Area` column into a numeric key type `Label` column (a format accepted by classification algorithms) and add it as a new dataset column:</span></span>

[!code-csharp[MapValueToKey](./snippets/github-issue-classification/csharp/Program.cs#MapValueToKey)]

<span data-ttu-id="967d7-188">接下来，调用 `mlContext.Transforms.Text.FeaturizeText`，它会将文本（`Title` 和 `Description`）列转换为每个名为 `TitleFeaturized` 和 `DescriptionFeaturized` 的值的数字向量。</span><span class="sxs-lookup"><span data-stu-id="967d7-188">Next, call `mlContext.Transforms.Text.FeaturizeText`, which transforms the text (`Title` and `Description`) columns into a numeric vector for each called `TitleFeaturized` and `DescriptionFeaturized`.</span></span> <span data-ttu-id="967d7-189">使用以下代码将两列的特征化附加到管道：</span><span class="sxs-lookup"><span data-stu-id="967d7-189">Append the featurization for both columns to the pipeline with the following code:</span></span>

[!code-csharp[FeaturizeText](./snippets/github-issue-classification/csharp/Program.cs#FeaturizeText)]

<span data-ttu-id="967d7-190">数据准备最后一步使用 [Concatenate()](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A) 方法将所有特征列合并到“特征”列。</span><span class="sxs-lookup"><span data-stu-id="967d7-190">The last step in data preparation combines all of the feature columns into the **Features** column using the [Concatenate()](xref:Microsoft.ML.TransformExtensionsCatalog.Concatenate%2A) method.</span></span> <span data-ttu-id="967d7-191">默认情况下，学习算法仅处理“特征”列的特征。</span><span class="sxs-lookup"><span data-stu-id="967d7-191">By default, a learning algorithm processes only features from the **Features** column.</span></span> <span data-ttu-id="967d7-192">使用以下代码将此转换附加到管道：</span><span class="sxs-lookup"><span data-stu-id="967d7-192">Append this transformation to the pipeline with the following code:</span></span>

[!code-csharp[Concatenate](./snippets/github-issue-classification/csharp/Program.cs#Concatenate)]

 <span data-ttu-id="967d7-193">接下来，附加一个 <xref:Microsoft.ML.Data.EstimatorChain%601.AppendCacheCheckpoint%2A> 来缓存数据视图，以便在使用缓存多次循环访问数据时获得更好的性能，如下面的代码所示：</span><span class="sxs-lookup"><span data-stu-id="967d7-193">Next, append a <xref:Microsoft.ML.Data.EstimatorChain%601.AppendCacheCheckpoint%2A> to cache the DataView so when you iterate over the data multiple times using the cache might get better performance, as with the following code:</span></span>

[!code-csharp[AppendCache](./snippets/github-issue-classification/csharp/Program.cs#AppendCache)]

> [!WARNING]
> <span data-ttu-id="967d7-194">对小/中型数据集使用 AppendCacheCheckpoint 可以降低训练时间。</span><span class="sxs-lookup"><span data-stu-id="967d7-194">Use AppendCacheCheckpoint for small/medium datasets to lower training time.</span></span> <span data-ttu-id="967d7-195">在处理大型数据集时不使用它（删除 .AppendCacheCheckpoint()）。</span><span class="sxs-lookup"><span data-stu-id="967d7-195">Do NOT use it (remove .AppendCacheCheckpoint()) when handling very large datasets.</span></span>

<span data-ttu-id="967d7-196">在 `ProcessData` 方法的末尾返回管道。</span><span class="sxs-lookup"><span data-stu-id="967d7-196">Return the pipeline at the end of the `ProcessData` method.</span></span>

[!code-csharp[ReturnPipeline](./snippets/github-issue-classification/csharp/Program.cs#ReturnPipeline)]

<span data-ttu-id="967d7-197">此步骤处理预处理/特征化。</span><span class="sxs-lookup"><span data-stu-id="967d7-197">This step handles preprocessing/featurization.</span></span> <span data-ttu-id="967d7-198">使用 ML.NET 中可用的其他组件可以在使用模型时生成更佳结果。</span><span class="sxs-lookup"><span data-stu-id="967d7-198">Using additional components available in ML.NET can enable better results with your model.</span></span>

## <a name="build-and-train-the-model"></a><span data-ttu-id="967d7-199">生成和定型模型</span><span class="sxs-lookup"><span data-stu-id="967d7-199">Build and train the model</span></span>

<span data-ttu-id="967d7-200">将以下调用添加到 `BuildAndTrainModel` 方法作为 `Main` 方法的下一行代码：</span><span class="sxs-lookup"><span data-stu-id="967d7-200">Add the following call to the `BuildAndTrainModel`method as the next line of code in the `Main` method:</span></span>

[!code-csharp[CallBuildAndTrainModel](./snippets/github-issue-classification/csharp/Program.cs#CallBuildAndTrainModel)]

<span data-ttu-id="967d7-201">`BuildAndTrainModel` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="967d7-201">The `BuildAndTrainModel` method executes the following tasks:</span></span>

* <span data-ttu-id="967d7-202">创建定型算法类。</span><span class="sxs-lookup"><span data-stu-id="967d7-202">Creates the training algorithm class.</span></span>
* <span data-ttu-id="967d7-203">定型模型。</span><span class="sxs-lookup"><span data-stu-id="967d7-203">Trains the model.</span></span>
* <span data-ttu-id="967d7-204">根据定型数据预测区域。</span><span class="sxs-lookup"><span data-stu-id="967d7-204">Predicts area based on training data.</span></span>
* <span data-ttu-id="967d7-205">返回模型。</span><span class="sxs-lookup"><span data-stu-id="967d7-205">Returns the model.</span></span>

<span data-ttu-id="967d7-206">使用下面的代码紧随 `Main` 方法后创建 `BuildAndTrainModel` 方法：</span><span class="sxs-lookup"><span data-stu-id="967d7-206">Create the `BuildAndTrainModel` method, just after the `Main` method, using the following code:</span></span>

```csharp
public static IEstimator<ITransformer> BuildAndTrainModel(IDataView trainingDataView, IEstimator<ITransformer> pipeline)
{

}
```

### <a name="about-the-classification-task"></a><span data-ttu-id="967d7-207">有关分类任务</span><span class="sxs-lookup"><span data-stu-id="967d7-207">About the classification task</span></span>

<span data-ttu-id="967d7-208">分类是一项机器学习任务，它使用数据来 **确定** 某个项或数据行的类别、类型或类，并且通常是以下类型之一：</span><span class="sxs-lookup"><span data-stu-id="967d7-208">Classification is a machine learning task that uses data to **determine** the category, type, or class of an item or row of data and is frequently one of the following types:</span></span>

* <span data-ttu-id="967d7-209">二元：A 或 B。</span><span class="sxs-lookup"><span data-stu-id="967d7-209">Binary: either A or B.</span></span>
* <span data-ttu-id="967d7-210">多类：可以通过使用单个模型来预测多个类别。</span><span class="sxs-lookup"><span data-stu-id="967d7-210">Multiclass: multiple categories that can be predicted by using a single model.</span></span>

<span data-ttu-id="967d7-211">对于此类问题，请使用多类分类学习算法，因为你的问题类别预测可能是多个类别（多类）而不是仅两个（二元）中的一个。</span><span class="sxs-lookup"><span data-stu-id="967d7-211">For this type of problem, use a Multiclass classification learning algorithm, since your issue category prediction can be one of multiple categories (multiclass) rather than just two (binary).</span></span>

<span data-ttu-id="967d7-212">将机器学习算法追加到数据转换定义中，方法是在 `BuildAndTrainModel()` 中添加以下代码作为第一行代码：</span><span class="sxs-lookup"><span data-stu-id="967d7-212">Append the machine learning algorithm to the data transformation definitions by adding the following as the first line of code in `BuildAndTrainModel()`:</span></span>

[!code-csharp[AddTrainer](./snippets/github-issue-classification/csharp/Program.cs#AddTrainer)]

<span data-ttu-id="967d7-213">[SdcaMaximumEntropy](xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer) 即多类分类训练算法。</span><span class="sxs-lookup"><span data-stu-id="967d7-213">The [SdcaMaximumEntropy](xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer) is your multiclass classification training algorithm.</span></span> <span data-ttu-id="967d7-214">它追加到 `pipeline` 并接受特征化的 `Title` 和 `Description` (`Features`) 以及 `Label` 输入参数，以便从历史数据中学习。</span><span class="sxs-lookup"><span data-stu-id="967d7-214">This is appended to the `pipeline` and accepts the featurized `Title` and `Description` (`Features`) and the `Label` input parameters to learn from the historic data.</span></span>

### <a name="train-the-model"></a><span data-ttu-id="967d7-215">定型模型</span><span class="sxs-lookup"><span data-stu-id="967d7-215">Train the model</span></span>

<span data-ttu-id="967d7-216">在 `BuildAndTrainModel()` 方法中添加以下代码作为下一代码行，使模型适应 `splitTrainSet` 数据，并返回经过训练的模型：</span><span class="sxs-lookup"><span data-stu-id="967d7-216">Fit the model to the `splitTrainSet` data and return the trained model by adding the following as the next line of code in the `BuildAndTrainModel()` method:</span></span>

[!code-csharp[TrainModel](./snippets/github-issue-classification/csharp/Program.cs#TrainModel)]

<span data-ttu-id="967d7-217">`Fit()` 方法通过转换数据集并应用训练来训练模型。</span><span class="sxs-lookup"><span data-stu-id="967d7-217">The `Fit()`method trains your model by transforming the dataset and applying the training.</span></span>

<span data-ttu-id="967d7-218">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可用于传入单个数据实例，然后对其执行预测。</span><span class="sxs-lookup"><span data-stu-id="967d7-218">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to pass in and then perform a prediction on a single instance of data.</span></span> <span data-ttu-id="967d7-219">将此 API 添加为 `BuildAndTrainModel()` 方法中的下一行：</span><span class="sxs-lookup"><span data-stu-id="967d7-219">Add this as the next line in the `BuildAndTrainModel()` method:</span></span>

[!code-csharp[CreatePredictionEngine1](./snippets/github-issue-classification/csharp/Program.cs#CreatePredictionEngine1)]

### <a name="predict-with-the-trained-model"></a><span data-ttu-id="967d7-220">使用训练的模型预测</span><span class="sxs-lookup"><span data-stu-id="967d7-220">Predict with the trained model</span></span>

<span data-ttu-id="967d7-221">通过创建一个 `GitHubIssue` 实例，在 `Predict` 方法中添加一个 GitHub 问题来测试定型模型的预测：</span><span class="sxs-lookup"><span data-stu-id="967d7-221">Add a GitHub issue to test the trained model's prediction in the `Predict` method by creating an instance of `GitHubIssue`:</span></span>

[!code-csharp[CreateTestIssue1](./snippets/github-issue-classification/csharp/Program.cs#CreateTestIssue1)]

<span data-ttu-id="967d7-222">使用 [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单行数据进行预测：</span><span class="sxs-lookup"><span data-stu-id="967d7-222">Use the [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) function makes a prediction on a single row of data:</span></span>

[!code-csharp[Predict](./snippets/github-issue-classification/csharp/Program.cs#Predict)]

### <a name="using-the-model-prediction-results"></a><span data-ttu-id="967d7-223">使用模型：预测结果</span><span class="sxs-lookup"><span data-stu-id="967d7-223">Using the model: prediction results</span></span>

<span data-ttu-id="967d7-224">显示 `GitHubIssue` 和相应的 `Area` 标签预测以便共享结果，并采取相应措施。</span><span class="sxs-lookup"><span data-stu-id="967d7-224">Display `GitHubIssue` and corresponding `Area` label prediction in order to share the results and act on them accordingly.</span></span>  <span data-ttu-id="967d7-225">使用以下 <xref:System.Console.WriteLine?displayProperty=nameWithType> 代码创建结果显示：</span><span class="sxs-lookup"><span data-stu-id="967d7-225">Create a display for the results using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[OutputPrediction](./snippets/github-issue-classification/csharp/Program.cs#OutputPrediction)]

### <a name="return-the-model-trained-to-use-for-evaluation"></a><span data-ttu-id="967d7-226">返回定型模型以用于评估</span><span class="sxs-lookup"><span data-stu-id="967d7-226">Return the model trained to use for evaluation</span></span>

<span data-ttu-id="967d7-227">在 `BuildAndTrainModel` 方法末尾返回模型。</span><span class="sxs-lookup"><span data-stu-id="967d7-227">Return the model at the end of the `BuildAndTrainModel` method.</span></span>

[!code-csharp[ReturnModel](./snippets/github-issue-classification/csharp/Program.cs#ReturnModel)]

## <a name="evaluate-the-model"></a><span data-ttu-id="967d7-228">评估模型</span><span class="sxs-lookup"><span data-stu-id="967d7-228">Evaluate the model</span></span>

<span data-ttu-id="967d7-229">你已经创建和定型模型，现在需要使用不同的数据集对其进行评估以保证质量和进行验证。</span><span class="sxs-lookup"><span data-stu-id="967d7-229">Now that you've created and trained the model, you need to evaluate it with a different dataset for quality assurance and validation.</span></span> <span data-ttu-id="967d7-230">在 `Evaluate` 方法中，将传入在 `BuildAndTrainModel` 中创建的模型以进行评估。</span><span class="sxs-lookup"><span data-stu-id="967d7-230">In the `Evaluate` method, the model created in `BuildAndTrainModel` is passed in to be evaluated.</span></span> <span data-ttu-id="967d7-231">紧随 `BuildAndTrainModel` 后创建 `Evaluate` 方法，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="967d7-231">Create the `Evaluate` method, just after `BuildAndTrainModel`, as in the following code:</span></span>

```csharp
public static void Evaluate(DataViewSchema trainingDataViewSchema)
{

}
```

<span data-ttu-id="967d7-232">`Evaluate` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="967d7-232">The `Evaluate` method executes the following tasks:</span></span>

* <span data-ttu-id="967d7-233">加载测试数据集。</span><span class="sxs-lookup"><span data-stu-id="967d7-233">Loads the test dataset.</span></span>
* <span data-ttu-id="967d7-234">创建多类评估程序。</span><span class="sxs-lookup"><span data-stu-id="967d7-234">Creates the multiclass evaluator.</span></span>
* <span data-ttu-id="967d7-235">评估模型并创建指标。</span><span class="sxs-lookup"><span data-stu-id="967d7-235">Evaluates the model and create metrics.</span></span>
* <span data-ttu-id="967d7-236">显示指标。</span><span class="sxs-lookup"><span data-stu-id="967d7-236">Displays the metrics.</span></span>

<span data-ttu-id="967d7-237">使用下面的代码，在 `BuildAndTrainModel` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="967d7-237">Add a call to the new method from the `Main` method, right under the `BuildAndTrainModel` method call, using the following code:</span></span>

[!code-csharp[CallEvaluate](./snippets/github-issue-classification/csharp/Program.cs#CallEvaluate)]

<span data-ttu-id="967d7-238">与之前对训练数据集所执行的操作那样，通过将以下代码添加到 `Evaluate` 方法来加载测试数据集：</span><span class="sxs-lookup"><span data-stu-id="967d7-238">As you did previously with the training dataset, load the test dataset by adding the following code to the `Evaluate` method:</span></span>

[!code-csharp[LoadTestDataset](./snippets/github-issue-classification/csharp/Program.cs#LoadTestDataset)]

<span data-ttu-id="967d7-239">[Evaluate()](xref:Microsoft.ML.MulticlassClassificationCatalog.Evaluate%2A) 方法使用指定数据集计算模型的质量指标。</span><span class="sxs-lookup"><span data-stu-id="967d7-239">The [Evaluate()](xref:Microsoft.ML.MulticlassClassificationCatalog.Evaluate%2A) method computes the quality metrics for the model using the specified dataset.</span></span> <span data-ttu-id="967d7-240">它返回 <xref:Microsoft.ML.Data.MulticlassClassificationMetrics> 对象，其中包含由多类分类计算器计算出的总体指标。</span><span class="sxs-lookup"><span data-stu-id="967d7-240">It returns a <xref:Microsoft.ML.Data.MulticlassClassificationMetrics> object that contains the overall metrics computed by multiclass classification evaluators.</span></span>
<span data-ttu-id="967d7-241">要显示指标来确定模型质量，需要先获取这些指标。</span><span class="sxs-lookup"><span data-stu-id="967d7-241">To display the metrics to determine the quality of the model, you need to get them first.</span></span>
<span data-ttu-id="967d7-242">请注意使用机器学习 `_trainedModel` 全局变量 ([ITransformer](xref:Microsoft.ML.ITransformer)) 的 [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) 方法来输入特征和返回预测。</span><span class="sxs-lookup"><span data-stu-id="967d7-242">Notice the use of the [Transform()](xref:Microsoft.ML.ITransformer.Transform%2A) method of the machine learning `_trainedModel` global variable (an [ITransformer](xref:Microsoft.ML.ITransformer)) to input the features and return predictions.</span></span> <span data-ttu-id="967d7-243">将以下代码作为下一行添加到 `Evaluate` 方法中：</span><span class="sxs-lookup"><span data-stu-id="967d7-243">Add the following code to the `Evaluate` method as the next line:</span></span>

[!code-csharp[Evaluate](./snippets/github-issue-classification/csharp/Program.cs#Evaluate)]

<span data-ttu-id="967d7-244">针对多类分类评估以下指标：</span><span class="sxs-lookup"><span data-stu-id="967d7-244">The following metrics are evaluated for multiclass classification:</span></span>

* <span data-ttu-id="967d7-245">微观准确性 - 每个“样本-类”对准确性指标的贡献度相同。</span><span class="sxs-lookup"><span data-stu-id="967d7-245">Micro Accuracy - Every sample-class pair contributes equally to the accuracy metric.</span></span>  <span data-ttu-id="967d7-246">通常会希望微观准确性尽可能接近 1。</span><span class="sxs-lookup"><span data-stu-id="967d7-246">You want Micro Accuracy to be as close to one as possible.</span></span>

* <span data-ttu-id="967d7-247">宏观准确性 - 每个类对准确性指标的贡献度相同。</span><span class="sxs-lookup"><span data-stu-id="967d7-247">Macro Accuracy - Every class contributes equally to the accuracy metric.</span></span> <span data-ttu-id="967d7-248">占比较小的类与占比较大的类拥有同等的权重。</span><span class="sxs-lookup"><span data-stu-id="967d7-248">Minority classes are given equal weight as the larger classes.</span></span> <span data-ttu-id="967d7-249">通常会希望宏观准确性尽可能接近 1。</span><span class="sxs-lookup"><span data-stu-id="967d7-249">You want Macro Accuracy to be as close to one as possible.</span></span>

* <span data-ttu-id="967d7-250">对数损失 - 请参阅[对数损失](../resources/glossary.md#log-loss)。</span><span class="sxs-lookup"><span data-stu-id="967d7-250">Log-loss - see [Log Loss](../resources/glossary.md#log-loss).</span></span> <span data-ttu-id="967d7-251">通常会希望对数损失尽可能接近 0。</span><span class="sxs-lookup"><span data-stu-id="967d7-251">You want Log-loss to be as close to zero as possible.</span></span>

* <span data-ttu-id="967d7-252">对数损失减小 - 取值范围为 [-inf，1.00]，其中 1.00 表示非常精准的预测结果，0 表示准确性一般的预测。</span><span class="sxs-lookup"><span data-stu-id="967d7-252">Log-loss reduction - Ranges from [-inf, 1.00], where 1.00 is perfect predictions and 0 indicates mean predictions.</span></span> <span data-ttu-id="967d7-253">通常会希望对数损失减少尽可能接近 1。</span><span class="sxs-lookup"><span data-stu-id="967d7-253">You want Log-loss reduction to be as close to one as possible.</span></span>

### <a name="displaying-the-metrics-for-model-validation"></a><span data-ttu-id="967d7-254">显示用于模型验证的指标</span><span class="sxs-lookup"><span data-stu-id="967d7-254">Displaying the metrics for model validation</span></span>

<span data-ttu-id="967d7-255">使用下面的代码显示指标、共享结果，然后处理它们：</span><span class="sxs-lookup"><span data-stu-id="967d7-255">Use the following code to display the metrics, share the results, and then act on them:</span></span>

[!code-csharp[DisplayMetrics](./snippets/github-issue-classification/csharp/Program.cs#DisplayMetrics)]

### <a name="save-the-model-to-a-file"></a><span data-ttu-id="967d7-256">将模型保存到文件</span><span class="sxs-lookup"><span data-stu-id="967d7-256">Save the model to a file</span></span>

<span data-ttu-id="967d7-257">对模型满意后，将其保存到文件中以便稍后或在其他应用程序中进行预测。</span><span class="sxs-lookup"><span data-stu-id="967d7-257">Once satisfied with your model, save it to a file to make predictions at a later time or in another application.</span></span> <span data-ttu-id="967d7-258">将以下代码添加到 `Evaluate` 方法中。</span><span class="sxs-lookup"><span data-stu-id="967d7-258">Add the following code to the `Evaluate` method.</span></span>

[!code-csharp[SnippetCallSaveModel](./snippets/github-issue-classification/csharp/Program.cs#SnippetCallSaveModel)]

<span data-ttu-id="967d7-259">在 `Evaluate` 方法下创建 `SaveModelAsFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="967d7-259">Create the `SaveModelAsFile` method below your `Evaluate` method.</span></span>

```csharp
private static void SaveModelAsFile(MLContext mlContext,DataViewSchema trainingDataViewSchema, ITransformer model)
{

}
```

<span data-ttu-id="967d7-260">将以下代码添加到 `SaveModelAsFile` 方法。</span><span class="sxs-lookup"><span data-stu-id="967d7-260">Add the following code to your `SaveModelAsFile` method.</span></span> <span data-ttu-id="967d7-261">此代码使用 [`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save%2A) 方法对训练后的模型进行序列化并将其存储为 zip 文件。</span><span class="sxs-lookup"><span data-stu-id="967d7-261">This code uses the [`Save`](xref:Microsoft.ML.ModelOperationsCatalog.Save%2A) method to serialize and store the trained model as a zip file.</span></span>

[!code-csharp[SnippetSaveModel](./snippets/github-issue-classification/csharp/Program.cs#SnippetSaveModel)]

## <a name="deploy-and-predict-with-a-model"></a><span data-ttu-id="967d7-262">使用模型进行部署和预测</span><span class="sxs-lookup"><span data-stu-id="967d7-262">Deploy and Predict with a model</span></span>

<span data-ttu-id="967d7-263">使用下面的代码，在 `Evaluate` 方法调用的正下方，从 `Main` 方法中添加对新方法的调用：</span><span class="sxs-lookup"><span data-stu-id="967d7-263">Add a call to the new method from the `Main` method, right under the `Evaluate` method call, using the following code:</span></span>

[!code-csharp[CallPredictIssue](./snippets/github-issue-classification/csharp/Program.cs#CallPredictIssue)]

<span data-ttu-id="967d7-264">使用以下代码恰好在 `Evaluate` 方法的后面（恰在 `SaveModelAsFile` 方法之前）创建 `PredictIssue` 方法：</span><span class="sxs-lookup"><span data-stu-id="967d7-264">Create the `PredictIssue` method, just after the `Evaluate` method (and just before the `SaveModelAsFile` method), using the following code:</span></span>

```csharp
private static void PredictIssue()
{

}
```

<span data-ttu-id="967d7-265">`PredictIssue` 方法执行以下任务：</span><span class="sxs-lookup"><span data-stu-id="967d7-265">The `PredictIssue` method executes the following tasks:</span></span>

* <span data-ttu-id="967d7-266">加载已保存的模型</span><span class="sxs-lookup"><span data-stu-id="967d7-266">Loads the saved model</span></span>
* <span data-ttu-id="967d7-267">创建测试数据的单个问题。</span><span class="sxs-lookup"><span data-stu-id="967d7-267">Creates a single issue of test data.</span></span>
* <span data-ttu-id="967d7-268">根据测试数据预测区域。</span><span class="sxs-lookup"><span data-stu-id="967d7-268">Predicts Area based on test data.</span></span>
* <span data-ttu-id="967d7-269">结合测试数据和预测进行报告。</span><span class="sxs-lookup"><span data-stu-id="967d7-269">Combines test data and predictions for reporting.</span></span>
* <span data-ttu-id="967d7-270">显示预测结果。</span><span class="sxs-lookup"><span data-stu-id="967d7-270">Displays the predicted results.</span></span>

<span data-ttu-id="967d7-271">通过向 `PredictIssue` 方法中添加以下代码，将保存的模型加载到应用程序中：</span><span class="sxs-lookup"><span data-stu-id="967d7-271">Load the saved model into your application by adding the following code to the `PredictIssue` method:</span></span>

[!code-csharp[SnippetLoadModel](./snippets/github-issue-classification/csharp/Program.cs#SnippetLoadModel)]

<span data-ttu-id="967d7-272">通过创建一个 `GitHubIssue` 实例，在 `Predict` 方法中添加一个 GitHub 问题来测试定型模型的预测：</span><span class="sxs-lookup"><span data-stu-id="967d7-272">Add a GitHub issue to test the trained model's prediction in the `Predict` method by creating an instance of `GitHubIssue`:</span></span>

[!code-csharp[AddTestIssue](./snippets/github-issue-classification/csharp/Program.cs#AddTestIssue)]

<span data-ttu-id="967d7-273">与之前一样，使用以下代码创建 `PredictionEngine` 实例：</span><span class="sxs-lookup"><span data-stu-id="967d7-273">As you did previously, create a `PredictionEngine` instance with the following code:</span></span>

[!code-csharp[CreatePredictionEngine](./snippets/github-issue-classification/csharp/Program.cs#CreatePredictionEngine)]

<span data-ttu-id="967d7-274">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="967d7-274">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to perform a prediction on a single instance of data.</span></span> <span data-ttu-id="967d7-275">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="967d7-275">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="967d7-276">可以在单线程环境或原型环境中使用。</span><span class="sxs-lookup"><span data-stu-id="967d7-276">It's acceptable to use in single-threaded or prototype environments.</span></span> <span data-ttu-id="967d7-277">为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="967d7-277">For improved performance and thread safety in production environments, use the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span> <span data-ttu-id="967d7-278">请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。</span><span class="sxs-lookup"><span data-stu-id="967d7-278">See this guide on how to [use `PredictionEnginePool` in an ASP.NET Core Web API](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).</span></span>

> [!NOTE]
> <span data-ttu-id="967d7-279">`PredictionEnginePool` 服务扩展目前处于预览状态。</span><span class="sxs-lookup"><span data-stu-id="967d7-279">`PredictionEnginePool` service extension is currently in preview.</span></span>

<span data-ttu-id="967d7-280">通过将以下代码添加到预测的 `PredictIssue` 方法，使用 `PredictionEngine` 来预测区域 GitHub 标签：</span><span class="sxs-lookup"><span data-stu-id="967d7-280">Use the `PredictionEngine` to predict the Area GitHub label by adding the following code to the `PredictIssue` method for the prediction:</span></span>

[!code-csharp[PredictIssue](./snippets/github-issue-classification/csharp/Program.cs#PredictIssue)]

### <a name="using-the-loaded-model-for-prediction"></a><span data-ttu-id="967d7-281">使用加载后的模型进行预测</span><span class="sxs-lookup"><span data-stu-id="967d7-281">Using the loaded model for prediction</span></span>

<span data-ttu-id="967d7-282">显示 `Area` 以便对问题进行分类并对其进行相应操作。</span><span class="sxs-lookup"><span data-stu-id="967d7-282">Display `Area` in order to categorize the issue and act on it accordingly.</span></span> <span data-ttu-id="967d7-283">使用以下 <xref:System.Console.WriteLine?displayProperty=nameWithType> 代码创建结果显示：</span><span class="sxs-lookup"><span data-stu-id="967d7-283">Create a display for the results using the following <xref:System.Console.WriteLine?displayProperty=nameWithType> code:</span></span>

[!code-csharp[DisplayResults](./snippets/github-issue-classification/csharp/Program.cs#DisplayResults)]

## <a name="results"></a><span data-ttu-id="967d7-284">结果</span><span class="sxs-lookup"><span data-stu-id="967d7-284">Results</span></span>

<span data-ttu-id="967d7-285">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="967d7-285">Your results should be similar to the following.</span></span> <span data-ttu-id="967d7-286">管道处理期间，会显示消息。</span><span class="sxs-lookup"><span data-stu-id="967d7-286">As the pipeline processes, it displays messages.</span></span> <span data-ttu-id="967d7-287">你可能会看到警告或处理消息。</span><span class="sxs-lookup"><span data-stu-id="967d7-287">You may see warnings, or processing messages.</span></span> <span data-ttu-id="967d7-288">为简便起见，已从以下结果中删除这些消息。</span><span class="sxs-lookup"><span data-stu-id="967d7-288">These messages have been removed from the following results for clarity.</span></span>

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

<span data-ttu-id="967d7-289">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="967d7-289">Congratulations!</span></span> <span data-ttu-id="967d7-290">现在，已成功生成用于为 GitHub 问题分类和预测区域标签的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="967d7-290">You've now successfully built a machine learning model for classifying and predicting an Area label for a GitHub issue.</span></span> <span data-ttu-id="967d7-291">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/GitHubIssueClassification) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="967d7-291">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/GitHubIssueClassification) repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="967d7-292">后续步骤</span><span class="sxs-lookup"><span data-stu-id="967d7-292">Next steps</span></span>

<span data-ttu-id="967d7-293">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="967d7-293">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="967d7-294">准备数据</span><span class="sxs-lookup"><span data-stu-id="967d7-294">Prepare your data</span></span>
> * <span data-ttu-id="967d7-295">转换数据</span><span class="sxs-lookup"><span data-stu-id="967d7-295">Transform the data</span></span>
> * <span data-ttu-id="967d7-296">定型模型</span><span class="sxs-lookup"><span data-stu-id="967d7-296">Train the model</span></span>
> * <span data-ttu-id="967d7-297">评估模型</span><span class="sxs-lookup"><span data-stu-id="967d7-297">Evaluate the model</span></span>
> * <span data-ttu-id="967d7-298">使用训练的模型预测</span><span class="sxs-lookup"><span data-stu-id="967d7-298">Predict with the trained model</span></span>
> * <span data-ttu-id="967d7-299">使用加载模型部署和预测</span><span class="sxs-lookup"><span data-stu-id="967d7-299">Deploy and Predict with a loaded model</span></span>

<span data-ttu-id="967d7-300">进入下一教程了解详细信息</span><span class="sxs-lookup"><span data-stu-id="967d7-300">Advance to the next tutorial to learn more</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="967d7-301">出租车费预测器</span><span class="sxs-lookup"><span data-stu-id="967d7-301">Taxi Fare Predictor</span></span>](predict-prices.md)
