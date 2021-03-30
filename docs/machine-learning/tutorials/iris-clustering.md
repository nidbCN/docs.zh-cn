---
title: 教程：对鸢尾花进行分类 - K 平均值聚类分析
description: 了解如何在聚类分析方案中使用 ML.NET
author: pkulikov
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0516
ms.openlocfilehash: 9240f365c6721baae03d8537e5e71153abf0f172
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875598"
---
# <a name="tutorial-categorize-iris-flowers-using-k-means-clustering-with-mlnet"></a><span data-ttu-id="43b62-103">教程：配合使用 K 平均值聚类分析和 ML.NET 来对鸢尾花分类</span><span class="sxs-lookup"><span data-stu-id="43b62-103">Tutorial: Categorize iris flowers using k-means clustering with ML.NET</span></span>

<span data-ttu-id="43b62-104">本教程演示如何使用 ML.NET 为[鸢尾花数据集](https://en.wikipedia.org/wiki/Iris_flower_data_set)构建[聚类分析模型](../resources/tasks.md#clustering)。</span><span class="sxs-lookup"><span data-stu-id="43b62-104">This tutorial illustrates how to use ML.NET to build a [clustering model](../resources/tasks.md#clustering) for the [iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set).</span></span>

<span data-ttu-id="43b62-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="43b62-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> - <span data-ttu-id="43b62-106">了解问题</span><span class="sxs-lookup"><span data-stu-id="43b62-106">Understand the problem</span></span>
> - <span data-ttu-id="43b62-107">选择适当的机器学习任务</span><span class="sxs-lookup"><span data-stu-id="43b62-107">Select the appropriate machine learning task</span></span>
> - <span data-ttu-id="43b62-108">准备数据</span><span class="sxs-lookup"><span data-stu-id="43b62-108">Prepare the data</span></span>
> - <span data-ttu-id="43b62-109">加载和转换数据</span><span class="sxs-lookup"><span data-stu-id="43b62-109">Load and transform the data</span></span>
> - <span data-ttu-id="43b62-110">选择学习算法</span><span class="sxs-lookup"><span data-stu-id="43b62-110">Choose a learning algorithm</span></span>
> - <span data-ttu-id="43b62-111">定型模型</span><span class="sxs-lookup"><span data-stu-id="43b62-111">Train the model</span></span>
> - <span data-ttu-id="43b62-112">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="43b62-112">Use the model for predictions</span></span>

## <a name="prerequisites"></a><span data-ttu-id="43b62-113">先决条件</span><span class="sxs-lookup"><span data-stu-id="43b62-113">Prerequisites</span></span>

- <span data-ttu-id="43b62-114">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="43b62-114">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="understand-the-problem"></a><span data-ttu-id="43b62-115">了解问题</span><span class="sxs-lookup"><span data-stu-id="43b62-115">Understand the problem</span></span>

<span data-ttu-id="43b62-116">此问题的本质即基于花卉特征将鸢尾花数据归入不同的组。</span><span class="sxs-lookup"><span data-stu-id="43b62-116">This problem is about dividing the set of iris flowers in different groups based on the flower features.</span></span> <span data-ttu-id="43b62-117">这些特征包括：花萼的长度和宽度以及花瓣的长度和宽度。</span><span class="sxs-lookup"><span data-stu-id="43b62-117">Those features are the length and width of a sepal and the length and width of a petal.</span></span> <span data-ttu-id="43b62-118">此教程假设每朵花的类型都是未知的。</span><span class="sxs-lookup"><span data-stu-id="43b62-118">For this tutorial, assume that the type of each flower is unknown.</span></span> <span data-ttu-id="43b62-119">需通过这些特征了解数据集的结构，并预测数据实例与此结构的拟合程度。</span><span class="sxs-lookup"><span data-stu-id="43b62-119">You want to learn the structure of a data set from the features and predict how a data instance fits this structure.</span></span>

## <a name="select-the-appropriate-machine-learning-task"></a><span data-ttu-id="43b62-120">选择适当的机器学习任务</span><span class="sxs-lookup"><span data-stu-id="43b62-120">Select the appropriate machine learning task</span></span>

<span data-ttu-id="43b62-121">鉴于不知道每朵花属于哪个分组，应选择[非监管式机器学习](../resources/glossary.md#unsupervised-machine-learning)任务。</span><span class="sxs-lookup"><span data-stu-id="43b62-121">As you don't know to which group each flower belongs to, you choose the [unsupervised machine learning](../resources/glossary.md#unsupervised-machine-learning) task.</span></span> <span data-ttu-id="43b62-122">为将数据归入不同的组，并使同一组中的元素互相之间更为相似（与其他组中的元素相比），应使用[聚类分析](../resources/tasks.md#clustering)机器学习任务。</span><span class="sxs-lookup"><span data-stu-id="43b62-122">To divide a data set in groups in such a way that elements in the same group are more similar to each other than to those in other groups, use a [clustering](../resources/tasks.md#clustering) machine learning task.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="43b62-123">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="43b62-123">Create a console application</span></span>

1. <span data-ttu-id="43b62-124">打开 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="43b62-124">Open Visual Studio.</span></span> <span data-ttu-id="43b62-125">从菜单栏中选择“文件”   > “新建”   > “项目”  。</span><span class="sxs-lookup"><span data-stu-id="43b62-125">Select **File** > **New** > **Project** from the menu bar.</span></span> <span data-ttu-id="43b62-126">在“新项目”对话框中，依次选择“Visual C#”和“.NET Core”节点。</span><span class="sxs-lookup"><span data-stu-id="43b62-126">In the **New Project** dialog, select the **Visual C#** node followed by the **.NET Core** node.</span></span> <span data-ttu-id="43b62-127">然后，选择“控制台应用程序(.NET Core)”  项目模板。</span><span class="sxs-lookup"><span data-stu-id="43b62-127">Then select the **Console App (.NET Core)** project template.</span></span> <span data-ttu-id="43b62-128">在“名称”文本框中，键入“IrisFlowerClustering”，然后选择“确定”按钮   。</span><span class="sxs-lookup"><span data-stu-id="43b62-128">In the **Name** text box, type "IrisFlowerClustering" and then select the **OK** button.</span></span>

1. <span data-ttu-id="43b62-129">在项目中创建一个名为“数据”的目录来保存数据集和模型文件  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-129">Create a directory named *Data* in your project to store the data set and model files:</span></span>

    <span data-ttu-id="43b62-130">在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”    。</span><span class="sxs-lookup"><span data-stu-id="43b62-130">In **Solution Explorer**, right-click the project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="43b62-131">键入“Data”，然后按 Enter。</span><span class="sxs-lookup"><span data-stu-id="43b62-131">Type "Data" and hit Enter.</span></span>

1. <span data-ttu-id="43b62-132">安装“Microsoft.ML NuGet”包  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-132">Install the **Microsoft.ML** NuGet package:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="43b62-133">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”   。</span><span class="sxs-lookup"><span data-stu-id="43b62-133">In **Solution Explorer**, right-click the project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="43b62-134">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，再选择“安装”按钮    。</span><span class="sxs-lookup"><span data-stu-id="43b62-134">Choose "nuget.org" as the Package source, select the **Browse** tab, search for **Microsoft.ML** and select the **Install** button.</span></span> <span data-ttu-id="43b62-135">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="43b62-135">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

## <a name="prepare-the-data"></a><span data-ttu-id="43b62-136">准备数据</span><span class="sxs-lookup"><span data-stu-id="43b62-136">Prepare the data</span></span>

1. <span data-ttu-id="43b62-137">下载 [iris.data](https://github.com/dotnet/machinelearning/blob/main/test/data/iris.data) 数据集并将其保存至在上一步中创建的“数据”文件夹  。</span><span class="sxs-lookup"><span data-stu-id="43b62-137">Download the [iris.data](https://github.com/dotnet/machinelearning/blob/main/test/data/iris.data) data set and save it to the *Data* folder you've created at the previous step.</span></span> <span data-ttu-id="43b62-138">若要详细了解鸢尾花数据集，请参阅[鸢尾花数据集](https://en.wikipedia.org/wiki/Iris_flower_data_set)维基百科页面，以及该数据集的源[鸢尾花数据集](http://archive.ics.uci.edu/ml/datasets/Iris)页面。</span><span class="sxs-lookup"><span data-stu-id="43b62-138">For more information about the iris data set, see the [Iris flower data set](https://en.wikipedia.org/wiki/Iris_flower_data_set) Wikipedia page and the [Iris Data Set](http://archive.ics.uci.edu/ml/datasets/Iris) page, which is the source of the data set.</span></span>

1. <span data-ttu-id="43b62-139">在“解决方案资源管理器”中，右键单击“iris.data”文件并选择“属性”    。</span><span class="sxs-lookup"><span data-stu-id="43b62-139">In **Solution Explorer**, right-click the *iris.data* file and select **Properties**.</span></span> <span data-ttu-id="43b62-140">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。</span><span class="sxs-lookup"><span data-stu-id="43b62-140">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

<span data-ttu-id="43b62-141">该 iris.data 文件包含五列，分别代表以下内容  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-141">The *iris.data* file contains five columns that represent:</span></span>

- <span data-ttu-id="43b62-142">花萼长度（厘米）</span><span class="sxs-lookup"><span data-stu-id="43b62-142">sepal length in centimeters</span></span>
- <span data-ttu-id="43b62-143">花萼宽度（厘米）</span><span class="sxs-lookup"><span data-stu-id="43b62-143">sepal width in centimeters</span></span>
- <span data-ttu-id="43b62-144">花瓣长度（厘米）</span><span class="sxs-lookup"><span data-stu-id="43b62-144">petal length in centimeters</span></span>
- <span data-ttu-id="43b62-145">花瓣宽度（厘米）</span><span class="sxs-lookup"><span data-stu-id="43b62-145">petal width in centimeters</span></span>
- <span data-ttu-id="43b62-146">鸢尾花类型</span><span class="sxs-lookup"><span data-stu-id="43b62-146">type of iris flower</span></span>

<span data-ttu-id="43b62-147">考虑到聚类分析示例，本教程忽略最后一列。</span><span class="sxs-lookup"><span data-stu-id="43b62-147">For the sake of the clustering example, this tutorial ignores the last column.</span></span>

## <a name="create-data-classes"></a><span data-ttu-id="43b62-148">创建数据类</span><span class="sxs-lookup"><span data-stu-id="43b62-148">Create data classes</span></span>

<span data-ttu-id="43b62-149">创建输入数据和预测类：</span><span class="sxs-lookup"><span data-stu-id="43b62-149">Create classes for the input data and the predictions:</span></span>

1. <span data-ttu-id="43b62-150">在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新项”  。</span><span class="sxs-lookup"><span data-stu-id="43b62-150">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>
1. <span data-ttu-id="43b62-151">在“添加新项”对话框中，选择“类”并将“名称”字段更改为“IrisData.cs”     。</span><span class="sxs-lookup"><span data-stu-id="43b62-151">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *IrisData.cs*.</span></span> <span data-ttu-id="43b62-152">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="43b62-152">Then, select the **Add** button.</span></span>
1. <span data-ttu-id="43b62-153">将以下 `using` 指令添加到新文件：</span><span class="sxs-lookup"><span data-stu-id="43b62-153">Add the following `using` directive to the new file:</span></span>

   [!code-csharp[Add necessary usings](./snippets/iris-clustering/csharp/IrisData.cs#Usings)]

<span data-ttu-id="43b62-154">删除现有类定义并向“IrisData.cs”文件添加以下代码，其中定义了两个类 `IrisData` 和 `ClusterPrediction` ：</span><span class="sxs-lookup"><span data-stu-id="43b62-154">Remove the existing class definition and add the following code, which defines the classes `IrisData` and `ClusterPrediction`, to the *IrisData.cs* file:</span></span>

[!code-csharp[Define data classes](./snippets/iris-clustering/csharp/IrisData.cs#ClassDefinitions)]

<span data-ttu-id="43b62-155">`IrisData` 是输入数据类，并且具有针对数据集每个特征的定义。</span><span class="sxs-lookup"><span data-stu-id="43b62-155">`IrisData` is the input data class and has definitions for each feature from the data set.</span></span> <span data-ttu-id="43b62-156">使用 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute) 属性在数据集文件中指定源列的索引。</span><span class="sxs-lookup"><span data-stu-id="43b62-156">Use the [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute) attribute to specify the indices of the source columns in the data set file.</span></span>

<span data-ttu-id="43b62-157">`ClusterPrediction` 类表示应用到 `IrisData` 实例的聚类分析模型的输出。</span><span class="sxs-lookup"><span data-stu-id="43b62-157">The `ClusterPrediction` class represents the output of the clustering model applied to an `IrisData` instance.</span></span> <span data-ttu-id="43b62-158">使用 [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute) 属性将 `PredictedClusterId` 和 `Distances` 字段分别绑定至 PredictedLabel 和 Score 列   。</span><span class="sxs-lookup"><span data-stu-id="43b62-158">Use the [ColumnName](xref:Microsoft.ML.Data.ColumnNameAttribute) attribute to bind the `PredictedClusterId` and `Distances` fields to the **PredictedLabel** and **Score** columns respectively.</span></span> <span data-ttu-id="43b62-159">在聚类分析任务中，这些列具有以下含义：</span><span class="sxs-lookup"><span data-stu-id="43b62-159">In case of the clustering task those columns have the following meaning:</span></span>

- <span data-ttu-id="43b62-160">**PredictedLabel** 列包含所预测的群集的 ID。</span><span class="sxs-lookup"><span data-stu-id="43b62-160">**PredictedLabel** column contains the ID of the predicted cluster.</span></span>
- <span data-ttu-id="43b62-161">**Score** 列包含一个数组，该数组中的数与群集形心之间的距离为欧氏距离的平方。</span><span class="sxs-lookup"><span data-stu-id="43b62-161">**Score** column contains an array with squared Euclidean distances to the cluster centroids.</span></span> <span data-ttu-id="43b62-162">该数组的长度等于群集数。</span><span class="sxs-lookup"><span data-stu-id="43b62-162">The array length is equal to the number of clusters.</span></span>

> [!NOTE]
> <span data-ttu-id="43b62-163">使用 `float` 类型来表示输入和预测数据类中的浮点值。</span><span class="sxs-lookup"><span data-stu-id="43b62-163">Use the `float` type to represent floating-point values in the input and prediction data classes.</span></span>

## <a name="define-data-and-model-paths"></a><span data-ttu-id="43b62-164">定义数据和模型路径</span><span class="sxs-lookup"><span data-stu-id="43b62-164">Define data and model paths</span></span>

<span data-ttu-id="43b62-165">返回到 Program.cs 文件并添加两个字段，以保存数据集文件以及用于保存模型的文件的路径  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-165">Go back to the *Program.cs* file and add two fields to hold the paths to the data set file and to the file to save the model:</span></span>

- <span data-ttu-id="43b62-166">`_dataPath` 包含具有用于定型模型的数据集的文件的路径。</span><span class="sxs-lookup"><span data-stu-id="43b62-166">`_dataPath` contains the path to the file with the data set used to train the model.</span></span>
- <span data-ttu-id="43b62-167">`_modelPath` 包含用于存储定型模型的文件的路径。</span><span class="sxs-lookup"><span data-stu-id="43b62-167">`_modelPath` contains the path to the file where the trained model is stored.</span></span>

<span data-ttu-id="43b62-168">将以下代码添加到 `Main` 方法上方，以指定这些路径：</span><span class="sxs-lookup"><span data-stu-id="43b62-168">Add the following code right above the `Main` method to specify those paths:</span></span>

[!code-csharp[Initialize paths](./snippets/iris-clustering/csharp/Program.cs#Paths)]

<span data-ttu-id="43b62-169">要编译前面的代码，请将以下 `using` 指令添加到 Program.cs 文件顶部  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-169">To make the preceding code compile, add the following `using` directives at the top of the *Program.cs* file:</span></span>

[!code-csharp[Add usings for paths](./snippets/iris-clustering/csharp/Program.cs#UsingsForPaths)]

## <a name="create-ml-context"></a><span data-ttu-id="43b62-170">创建 ML 上下文</span><span class="sxs-lookup"><span data-stu-id="43b62-170">Create ML context</span></span>

<span data-ttu-id="43b62-171">将以下附加 `using` 指令添加到 Program.cs 文件顶部  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-171">Add the following additional `using` directives to the top of the *Program.cs* file:</span></span>

[!code-csharp[Add Microsoft.ML usings](./snippets/iris-clustering/csharp/Program.cs#MLUsings)]

<span data-ttu-id="43b62-172">在 `Main` 方法中，请使用以下代码替换 `Console.WriteLine("Hello World!");` 行：</span><span class="sxs-lookup"><span data-stu-id="43b62-172">In the `Main` method, replace the `Console.WriteLine("Hello World!");` line with the following code:</span></span>

[!code-csharp[Create ML context](./snippets/iris-clustering/csharp/Program.cs#CreateContext)]

<span data-ttu-id="43b62-173"><xref:Microsoft.ML.MLContext?displayProperty=nameWithType> 类表示机器学习环境，并提供用于数据加载、模型定型、预测和其他任务的日志记录和入口点的机制。</span><span class="sxs-lookup"><span data-stu-id="43b62-173">The <xref:Microsoft.ML.MLContext?displayProperty=nameWithType> class represents the machine learning environment and provides mechanisms for logging and entry points for data loading, model training, prediction, and other tasks.</span></span> <span data-ttu-id="43b62-174">这在概念上相当于在实体框架中使用 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="43b62-174">This is comparable conceptually to using `DbContext` in Entity Framework.</span></span>

## <a name="set-up-data-loading"></a><span data-ttu-id="43b62-175">设置数据加载</span><span class="sxs-lookup"><span data-stu-id="43b62-175">Set up data loading</span></span>

<span data-ttu-id="43b62-176">将以下代码添加到 `Main` 方法以设置加载数据的方式：</span><span class="sxs-lookup"><span data-stu-id="43b62-176">Add the following code to the `Main` method to set up the way to load data:</span></span>

[!code-csharp[Create text loader](./snippets/iris-clustering/csharp/Program.cs#CreateDataView)]

<span data-ttu-id="43b62-177">泛型 [`MLContext.Data.LoadFromTextFile` 扩展方法](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29)根据所提供的 `IrisData` 类型推断数据集架构，并返回可用作转换器输入的 <xref:Microsoft.ML.IDataView>。</span><span class="sxs-lookup"><span data-stu-id="43b62-177">The generic [`MLContext.Data.LoadFromTextFile` extension method](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) infers the data set schema from the provided `IrisData` type and returns <xref:Microsoft.ML.IDataView> which can be used as input for transformers.</span></span>

## <a name="create-a-learning-pipeline"></a><span data-ttu-id="43b62-178">创建学习管道</span><span class="sxs-lookup"><span data-stu-id="43b62-178">Create a learning pipeline</span></span>

<span data-ttu-id="43b62-179">对于本教程，聚类分析任务的学习管道包含两个以下步骤：</span><span class="sxs-lookup"><span data-stu-id="43b62-179">For this tutorial, the learning pipeline of the clustering task comprises two following steps:</span></span>

- <span data-ttu-id="43b62-180">将加载的列连接到“Features”列，由聚类分析训练程序使用  ；</span><span class="sxs-lookup"><span data-stu-id="43b62-180">concatenate loaded columns into one **Features** column, which is used by a clustering trainer;</span></span>
- <span data-ttu-id="43b62-181">借助 <xref:Microsoft.ML.Trainers.KMeansTrainer> 训练程序使用 k - 平均值 + + 聚类分析算法来定型模型。</span><span class="sxs-lookup"><span data-stu-id="43b62-181">use a <xref:Microsoft.ML.Trainers.KMeansTrainer> trainer to train the model using the k-means++ clustering algorithm.</span></span>

<span data-ttu-id="43b62-182">将以下代码添加到 `Main` 方法中：</span><span class="sxs-lookup"><span data-stu-id="43b62-182">Add the following code to the `Main` method:</span></span>

[!code-csharp[Create pipeline](./snippets/iris-clustering/csharp/Program.cs#CreatePipeline)]

<span data-ttu-id="43b62-183">该代码指定该数据集应拆分为三个群集。</span><span class="sxs-lookup"><span data-stu-id="43b62-183">The code specifies that the data set should be split in three clusters.</span></span>

## <a name="train-the-model"></a><span data-ttu-id="43b62-184">定型模型</span><span class="sxs-lookup"><span data-stu-id="43b62-184">Train the model</span></span>

<span data-ttu-id="43b62-185">前述部分中添加的步骤准备了用于定型的管道，但尚未执行。</span><span class="sxs-lookup"><span data-stu-id="43b62-185">The steps added in the preceding sections prepared the pipeline for training, however, none have been executed.</span></span> <span data-ttu-id="43b62-186">将以下行添加到 `Main` 方法以执行数据加载和模型定型：</span><span class="sxs-lookup"><span data-stu-id="43b62-186">Add the following line to the `Main` method to perform data loading and model training:</span></span>

[!code-csharp[Train the model](./snippets/iris-clustering/csharp/Program.cs#TrainModel)]

### <a name="save-the-model"></a><span data-ttu-id="43b62-187">保存模型</span><span class="sxs-lookup"><span data-stu-id="43b62-187">Save the model</span></span>

<span data-ttu-id="43b62-188">此时，你具有可以集成到任何现有或新 .NET 应用程序的模型。</span><span class="sxs-lookup"><span data-stu-id="43b62-188">At this point, you have a model that can be integrated into any of your existing or new .NET applications.</span></span> <span data-ttu-id="43b62-189">要将模型保存为 .zip 文件，请将以下代码添加到 `Main` 方法中：</span><span class="sxs-lookup"><span data-stu-id="43b62-189">To save your model to a .zip file, add the following code to the `Main` method:</span></span>

[!code-csharp[Save the model](./snippets/iris-clustering/csharp/Program.cs#SaveModel)]

## <a name="use-the-model-for-predictions"></a><span data-ttu-id="43b62-190">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="43b62-190">Use the model for predictions</span></span>

<span data-ttu-id="43b62-191">要进行预测，请使用通过转换器管道获取输入类型实例和生成输出类型实例的 <xref:Microsoft.ML.PredictionEngine%602> 类。</span><span class="sxs-lookup"><span data-stu-id="43b62-191">To make predictions, use the <xref:Microsoft.ML.PredictionEngine%602> class that takes instances of the input type through the transformer pipeline and produces instances of the output type.</span></span> <span data-ttu-id="43b62-192">将以下行添加到 `Main` 方法以创建该类的实例：</span><span class="sxs-lookup"><span data-stu-id="43b62-192">Add the following line to the `Main` method to create an instance of that class:</span></span>

[!code-csharp[Create predictor](./snippets/iris-clustering/csharp/Program.cs#Predictor)]

<span data-ttu-id="43b62-193">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="43b62-193">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to perform a prediction on a single instance of data.</span></span> <span data-ttu-id="43b62-194">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="43b62-194">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="43b62-195">可以在单线程环境或原型环境中使用。</span><span class="sxs-lookup"><span data-stu-id="43b62-195">It's acceptable to use in single-threaded or prototype environments.</span></span> <span data-ttu-id="43b62-196">为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="43b62-196">For improved performance and thread safety in production environments, use the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span> <span data-ttu-id="43b62-197">请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。</span><span class="sxs-lookup"><span data-stu-id="43b62-197">See this guide on how to [use `PredictionEnginePool` in an ASP.NET Core Web API](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).</span></span>

> [!NOTE]
> <span data-ttu-id="43b62-198">`PredictionEnginePool` 服务扩展目前处于预览状态。</span><span class="sxs-lookup"><span data-stu-id="43b62-198">`PredictionEnginePool` service extension is currently in preview.</span></span>

<span data-ttu-id="43b62-199">将 `TestIrisData` 类创建到房屋测试数据实例：</span><span class="sxs-lookup"><span data-stu-id="43b62-199">Create the `TestIrisData` class to house test data instances:</span></span>

1. <span data-ttu-id="43b62-200">在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新项”  。</span><span class="sxs-lookup"><span data-stu-id="43b62-200">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>
1. <span data-ttu-id="43b62-201">在“添加新项”对话框中，选择“类”并将“名称”字段更改为“TestIrisData.cs”     。</span><span class="sxs-lookup"><span data-stu-id="43b62-201">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *TestIrisData.cs*.</span></span> <span data-ttu-id="43b62-202">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="43b62-202">Then, select the **Add** button.</span></span>
1. <span data-ttu-id="43b62-203">将类修改为静态，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="43b62-203">Modify the class to be static like in the following example:</span></span>

   [!code-csharp[Make class static](./snippets/iris-clustering/csharp/TestIrisData.cs#Static)]

<span data-ttu-id="43b62-204">本教程引入此类中的一个鸢尾花数据实例。</span><span class="sxs-lookup"><span data-stu-id="43b62-204">This tutorial introduces one iris data instance within this class.</span></span> <span data-ttu-id="43b62-205">可以添加其他方案来体验此模型。</span><span class="sxs-lookup"><span data-stu-id="43b62-205">You can add other scenarios to experiment with the model.</span></span> <span data-ttu-id="43b62-206">将下面的代码添加到 `TestIrisData` 类中：</span><span class="sxs-lookup"><span data-stu-id="43b62-206">Add the following code into the `TestIrisData` class:</span></span>

[!code-csharp[Test data](./snippets/iris-clustering/csharp/TestIrisData.cs#TestData)]

<span data-ttu-id="43b62-207">若要查找指定项所属的群集，请返回至 Program.cs 文件并将以下代码添加进 `Main` 方法  ：</span><span class="sxs-lookup"><span data-stu-id="43b62-207">To find out the cluster to which the specified item belongs to, go back to the *Program.cs* file and add the following code into the `Main` method:</span></span>

[!code-csharp[Predict and output results](./snippets/iris-clustering/csharp/Program.cs#PredictionExample)]

<span data-ttu-id="43b62-208">运行该程序以查看哪个群集包含所指定的数据实例，以及从该实例到群集形心的距离的平方值。</span><span class="sxs-lookup"><span data-stu-id="43b62-208">Run the program to see which cluster contains the specified data instance and squared distances from that instance to the cluster centroids.</span></span> <span data-ttu-id="43b62-209">结果应如下所示：</span><span class="sxs-lookup"><span data-stu-id="43b62-209">Your results should be similar to the following:</span></span>

```text
Cluster: 2
Distances: 11.69127 0.02159119 25.59896
```

<span data-ttu-id="43b62-210">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="43b62-210">Congratulations!</span></span> <span data-ttu-id="43b62-211">现已成功地生成用于鸢尾花聚类分析的机器学习模型并将其用于预测。</span><span class="sxs-lookup"><span data-stu-id="43b62-211">You've now successfully built a machine learning model for iris clustering and used it to make predictions.</span></span> <span data-ttu-id="43b62-212">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/IrisFlowerClustering) GitHub 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="43b62-212">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/IrisFlowerClustering) GitHub repository.</span></span>

## <a name="next-steps"></a><span data-ttu-id="43b62-213">后续步骤</span><span class="sxs-lookup"><span data-stu-id="43b62-213">Next steps</span></span>

<span data-ttu-id="43b62-214">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="43b62-214">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> - <span data-ttu-id="43b62-215">了解问题</span><span class="sxs-lookup"><span data-stu-id="43b62-215">Understand the problem</span></span>
> - <span data-ttu-id="43b62-216">选择适当的机器学习任务</span><span class="sxs-lookup"><span data-stu-id="43b62-216">Select the appropriate machine learning task</span></span>
> - <span data-ttu-id="43b62-217">准备数据</span><span class="sxs-lookup"><span data-stu-id="43b62-217">Prepare the data</span></span>
> - <span data-ttu-id="43b62-218">加载和转换数据</span><span class="sxs-lookup"><span data-stu-id="43b62-218">Load and transform the data</span></span>
> - <span data-ttu-id="43b62-219">选择学习算法</span><span class="sxs-lookup"><span data-stu-id="43b62-219">Choose a learning algorithm</span></span>
> - <span data-ttu-id="43b62-220">定型模型</span><span class="sxs-lookup"><span data-stu-id="43b62-220">Train the model</span></span>
> - <span data-ttu-id="43b62-221">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="43b62-221">Use the model for predictions</span></span>

<span data-ttu-id="43b62-222">查看我们的 GitHub 存储库以继续学习，并找到更多示例。</span><span class="sxs-lookup"><span data-stu-id="43b62-222">Check out our GitHub repository to continue learning and find more samples.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="43b62-223">dotnet/machinelearning GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="43b62-223">dotnet/machinelearning GitHub repository</span></span>](https://github.com/dotnet/machinelearning/)
