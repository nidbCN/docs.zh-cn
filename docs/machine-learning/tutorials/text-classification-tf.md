---
title: 教程：使用 TensorFlow 模型分析评审情绪
description: 本教程演示如何使用预先训练的 TensorFlow 模型对网站评论中的情绪进行分类。 二元情绪分类器是使用 Visual Studio 开发的 C# 控制台应用程序。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 0c2cfd46375406726913a83d67c45ff922a1085b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877002"
---
# <a name="tutorial-analyze-sentiment-of-movie-reviews-using-a-pre-trained-tensorflow-model-in-mlnet"></a><span data-ttu-id="f50fb-104">教程：在 ML.NET 中使用预先训练的 TensorFlow 模型分析电影评论的情绪</span><span class="sxs-lookup"><span data-stu-id="f50fb-104">Tutorial: Analyze sentiment of movie reviews using a pre-trained TensorFlow model in ML.NET</span></span>

<span data-ttu-id="f50fb-105">本教程演示如何使用预先训练的 TensorFlow 模型对网站评论中的情绪进行分类。</span><span class="sxs-lookup"><span data-stu-id="f50fb-105">This tutorial shows you how to use a pre-trained TensorFlow model to classify sentiment in website comments.</span></span> <span data-ttu-id="f50fb-106">二元情绪分类器是使用 Visual Studio 开发的 C# 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="f50fb-106">The binary sentiment classifier is a C# console application developed using Visual Studio.</span></span>

<span data-ttu-id="f50fb-107">本教程中使用的 TensorFlow 模型是使用 IMDB 数据库中的电影评论训练的。</span><span class="sxs-lookup"><span data-stu-id="f50fb-107">The TensorFlow model used in this tutorial was trained using movie reviews from the IMDB database.</span></span> <span data-ttu-id="f50fb-108">完成应用程序的开发后，你将能够提供电影评论文本，应用程序将会告诉你该评论是正面情绪还是负面情绪。</span><span class="sxs-lookup"><span data-stu-id="f50fb-108">Once you have finished developing the application, you will be able to supply movie review text and the application will tell you whether the review has positive or negative sentiment.</span></span>

<span data-ttu-id="f50fb-109">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="f50fb-109">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="f50fb-110">加载预先训练的 TensorFlow 模型</span><span class="sxs-lookup"><span data-stu-id="f50fb-110">Load a pre-trained TensorFlow model</span></span>
> * <span data-ttu-id="f50fb-111">将网站评论文本转换为适用于模型的特征</span><span class="sxs-lookup"><span data-stu-id="f50fb-111">Transform website comment text into features suitable for the model</span></span>
> * <span data-ttu-id="f50fb-112">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="f50fb-112">Use the model to make a prediction</span></span>

<span data-ttu-id="f50fb-113">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TextClassificationTF) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="f50fb-113">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TextClassificationTF) repository.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f50fb-114">先决条件</span><span class="sxs-lookup"><span data-stu-id="f50fb-114">Prerequisites</span></span>

* <span data-ttu-id="f50fb-115">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2017 版本 15.6 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。</span><span class="sxs-lookup"><span data-stu-id="f50fb-115">[Visual Studio 2017 version 15.6 or later](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="setup"></a><span data-ttu-id="f50fb-116">安装</span><span class="sxs-lookup"><span data-stu-id="f50fb-116">Setup</span></span>

### <a name="create-the-application"></a><span data-ttu-id="f50fb-117">创建应用程序</span><span class="sxs-lookup"><span data-stu-id="f50fb-117">Create the application</span></span>

1. <span data-ttu-id="f50fb-118">创建名为“TextClassificationTF”的“.NET Core 控制台应用程序”。</span><span class="sxs-lookup"><span data-stu-id="f50fb-118">Create a **.NET Core Console Application** called "TextClassificationTF".</span></span>

2. <span data-ttu-id="f50fb-119">在项目中创建名为“Data”的目录，用于保存数据集文件。</span><span class="sxs-lookup"><span data-stu-id="f50fb-119">Create a directory named *Data* in your project to save your data set files.</span></span>

3. <span data-ttu-id="f50fb-120">安装“Microsoft.ML NuGet 包”  ：</span><span class="sxs-lookup"><span data-stu-id="f50fb-120">Install the **Microsoft.ML NuGet Package**:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    <span data-ttu-id="f50fb-121">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="f50fb-121">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="f50fb-122">选择“nuget.org”作为包源，然后选择“浏览”选项卡。搜索“Microsoft.ML”，选择所需的包，然后选择“安装”按钮 。</span><span class="sxs-lookup"><span data-stu-id="f50fb-122">Choose "nuget.org" as the package source, and then select the **Browse** tab. Search for **Microsoft.ML**, select the package you want, and then select the **Install** button.</span></span> <span data-ttu-id="f50fb-123">同意所选包的许可条款，继续执行安装。</span><span class="sxs-lookup"><span data-stu-id="f50fb-123">Proceed with the installation by agreeing to the license terms for the package you choose.</span></span> <span data-ttu-id="f50fb-124">对 Microsoft.ML.TensorFlow、Microsoft.ML.SampleUtils 和 SciSharp.TensorFlow.Redist，重复上述步骤。  </span><span class="sxs-lookup"><span data-stu-id="f50fb-124">Repeat these steps for **Microsoft.ML.TensorFlow**, **Microsoft.ML.SampleUtils** and **SciSharp.TensorFlow.Redist**.</span></span>

### <a name="add-the-tensorflow-model-to-the-project"></a><span data-ttu-id="f50fb-125">将 TensorFlow 模型添加到项目</span><span class="sxs-lookup"><span data-stu-id="f50fb-125">Add the TensorFlow model to the project</span></span>

> [!NOTE]
> <span data-ttu-id="f50fb-126">本教程的模型来自 [dotnet/machinelearning-testdata](https://github.com/dotnet/machinelearning-testdata/tree/main/Microsoft.ML.TensorFlow.TestModels/sentiment_model) GitHub 存储库。</span><span class="sxs-lookup"><span data-stu-id="f50fb-126">The model for this tutorial is from the [dotnet/machinelearning-testdata](https://github.com/dotnet/machinelearning-testdata/tree/main/Microsoft.ML.TensorFlow.TestModels/sentiment_model) GitHub repo.</span></span> <span data-ttu-id="f50fb-127">该模型采用 TensorFlow SavedModel 格式。</span><span class="sxs-lookup"><span data-stu-id="f50fb-127">The model is in TensorFlow SavedModel format.</span></span>

1. <span data-ttu-id="f50fb-128">下载 [sentiment_model zip 文件](https://github.com/dotnet/samples/blob/main/machine-learning/models/textclassificationtf/sentiment_model.zip?raw=true)并将其解压缩。</span><span class="sxs-lookup"><span data-stu-id="f50fb-128">Download the [sentiment_model zip file](https://github.com/dotnet/samples/blob/main/machine-learning/models/textclassificationtf/sentiment_model.zip?raw=true), and unzip.</span></span>

    <span data-ttu-id="f50fb-129">该 zip 文件包含：</span><span class="sxs-lookup"><span data-stu-id="f50fb-129">The zip file contains:</span></span>

    * <span data-ttu-id="f50fb-130">`saved_model.pb`：TensorFlow 模型本身。</span><span class="sxs-lookup"><span data-stu-id="f50fb-130">`saved_model.pb`: the TensorFlow model itself.</span></span> <span data-ttu-id="f50fb-131">该模型采用表示 IMDB 评论字符串中文本的特征的固定长度（大小为 600）整数数组，并输出两个概率（总和为 1）：输入评论具有正面情绪的概率，以及输入评论具有负面情绪的概率。</span><span class="sxs-lookup"><span data-stu-id="f50fb-131">The model takes a fixed length (size 600) integer array of features representing the text in an IMDB review string, and outputs two probabilities which sum to 1: the probability that the input review has positive sentiment, and the probability that the input review has negative sentiment.</span></span>
    * <span data-ttu-id="f50fb-132">`imdb_word_index.csv`：从单个单词到整数值的映射。</span><span class="sxs-lookup"><span data-stu-id="f50fb-132">`imdb_word_index.csv`: a mapping from individual words to an integer value.</span></span> <span data-ttu-id="f50fb-133">映射用于为 TensorFlow 模型生成输入特征。</span><span class="sxs-lookup"><span data-stu-id="f50fb-133">The mapping is used to generate the input features for the TensorFlow model.</span></span>

2. <span data-ttu-id="f50fb-134">将最内层 `sentiment_model` 目录的内容复制到 TextClassificationTF 项目的 `sentiment_model` 目录中。</span><span class="sxs-lookup"><span data-stu-id="f50fb-134">Copy the contents of the innermost `sentiment_model` directory into your *TextClassificationTF* project `sentiment_model` directory.</span></span> <span data-ttu-id="f50fb-135">此目录包含本教程所需的模型和其他支持文件，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="f50fb-135">This directory contains the model and additional support files needed for this tutorial, as shown in the following image:</span></span>

   ![sentiment_model 目录内容](./media/text-classification-tf/sentiment-model-files.png)

3. <span data-ttu-id="f50fb-137">在“解决方案资源管理器”中，右键单击 `sentiment_model` 目录和子目录中的每个文件，然后选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="f50fb-137">In Solution Explorer, right-click each of the files in the `sentiment_model` directory and subdirectory and select **Properties**.</span></span> <span data-ttu-id="f50fb-138">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”  。</span><span class="sxs-lookup"><span data-stu-id="f50fb-138">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

### <a name="add-using-statements-and-global-variables"></a><span data-ttu-id="f50fb-139">添加 using 语句和全局变量</span><span class="sxs-lookup"><span data-stu-id="f50fb-139">Add using statements and global variables</span></span>

1. <span data-ttu-id="f50fb-140">将以下附加的 `using` 语句添加到“Program.cs”文件顶部：</span><span class="sxs-lookup"><span data-stu-id="f50fb-140">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

   [!code-csharp[AddUsings](./snippets/text-classification-tf/csharp/Program.cs#AddUsings "Add necessary usings")]

1. <span data-ttu-id="f50fb-141">在 `Main` 方法正上方创建两个全局变量，用来保留已保存的模型文件路径和特征向量长度。</span><span class="sxs-lookup"><span data-stu-id="f50fb-141">Create two global variables right above the `Main` method to hold the saved model file path, and the feature vector length.</span></span>

   [!code-csharp[DeclareGlobalVariables](./snippets/text-classification-tf/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

    * <span data-ttu-id="f50fb-142">`_modelPath` 是已训练模型的文件路径。</span><span class="sxs-lookup"><span data-stu-id="f50fb-142">`_modelPath` is the file path of the trained model.</span></span>
    * <span data-ttu-id="f50fb-143">`FeatureLength` 是模型需要的整数特征数组的长度。</span><span class="sxs-lookup"><span data-stu-id="f50fb-143">`FeatureLength` is the length of the integer feature array that the model is expecting.</span></span>

### <a name="model-the-data"></a><span data-ttu-id="f50fb-144">为数据建模</span><span class="sxs-lookup"><span data-stu-id="f50fb-144">Model the data</span></span>

<span data-ttu-id="f50fb-145">电影评论是自由格式的文本。</span><span class="sxs-lookup"><span data-stu-id="f50fb-145">Movie reviews are free form text.</span></span> <span data-ttu-id="f50fb-146">应用程序会将文本转换为模型在多个离散阶段中所需的输入格式。</span><span class="sxs-lookup"><span data-stu-id="f50fb-146">Your application converts the text into the input format expected by the model in a number of discrete stages.</span></span>

<span data-ttu-id="f50fb-147">首先是将文本拆分为单独的单词，然后使用提供的映射文件将每个单词映射到整数编码。</span><span class="sxs-lookup"><span data-stu-id="f50fb-147">The first is to split the text into separate words and use the provided mapping file to map each word onto an integer encoding.</span></span> <span data-ttu-id="f50fb-148">这种转换的结果是一个可变长度的整数数组，其长度对应于句子中的单词数。</span><span class="sxs-lookup"><span data-stu-id="f50fb-148">The result of this transformation is a variable length integer array with a length corresponding to the number of words in the sentence.</span></span>

|<span data-ttu-id="f50fb-149">Property</span><span class="sxs-lookup"><span data-stu-id="f50fb-149">Property</span></span>| <span data-ttu-id="f50fb-150">值</span><span class="sxs-lookup"><span data-stu-id="f50fb-150">Value</span></span>|<span data-ttu-id="f50fb-151">类型</span><span class="sxs-lookup"><span data-stu-id="f50fb-151">Type</span></span>|
|-------------|-----------------------|------|
|<span data-ttu-id="f50fb-152">ReviewText</span><span class="sxs-lookup"><span data-stu-id="f50fb-152">ReviewText</span></span>|<span data-ttu-id="f50fb-153">这部电影非常不错</span><span class="sxs-lookup"><span data-stu-id="f50fb-153">this film is really good</span></span>|<span data-ttu-id="f50fb-154">string</span><span class="sxs-lookup"><span data-stu-id="f50fb-154">string</span></span>|
|<span data-ttu-id="f50fb-155">VariableLengthFeatures</span><span class="sxs-lookup"><span data-stu-id="f50fb-155">VariableLengthFeatures</span></span>|<span data-ttu-id="f50fb-156">14,22,9,66,78,...</span><span class="sxs-lookup"><span data-stu-id="f50fb-156">14,22,9,66,78,...</span></span> |<span data-ttu-id="f50fb-157">int[]</span><span class="sxs-lookup"><span data-stu-id="f50fb-157">int[]</span></span>|

<span data-ttu-id="f50fb-158">然后将可变长度特征数组的大小调整为固定长度 600。</span><span class="sxs-lookup"><span data-stu-id="f50fb-158">The variable length feature array is then resized to a fixed length of 600.</span></span> <span data-ttu-id="f50fb-159">这是 TensorFlow 模型所需的长度。</span><span class="sxs-lookup"><span data-stu-id="f50fb-159">This is the length that the TensorFlow model expects.</span></span>

|<span data-ttu-id="f50fb-160">Property</span><span class="sxs-lookup"><span data-stu-id="f50fb-160">Property</span></span>| <span data-ttu-id="f50fb-161">值</span><span class="sxs-lookup"><span data-stu-id="f50fb-161">Value</span></span>|<span data-ttu-id="f50fb-162">类型</span><span class="sxs-lookup"><span data-stu-id="f50fb-162">Type</span></span>|
|-------------|-----------------------|------|
|<span data-ttu-id="f50fb-163">ReviewText</span><span class="sxs-lookup"><span data-stu-id="f50fb-163">ReviewText</span></span>|<span data-ttu-id="f50fb-164">这部电影非常不错</span><span class="sxs-lookup"><span data-stu-id="f50fb-164">this film is really good</span></span>|<span data-ttu-id="f50fb-165">string</span><span class="sxs-lookup"><span data-stu-id="f50fb-165">string</span></span>|
|<span data-ttu-id="f50fb-166">VariableLengthFeatures</span><span class="sxs-lookup"><span data-stu-id="f50fb-166">VariableLengthFeatures</span></span>|<span data-ttu-id="f50fb-167">14,22,9,66,78,...</span><span class="sxs-lookup"><span data-stu-id="f50fb-167">14,22,9,66,78,...</span></span> |<span data-ttu-id="f50fb-168">int[]</span><span class="sxs-lookup"><span data-stu-id="f50fb-168">int[]</span></span>|
|<span data-ttu-id="f50fb-169">特征</span><span class="sxs-lookup"><span data-stu-id="f50fb-169">Features</span></span>|<span data-ttu-id="f50fb-170">14,22,9,66,78,...</span><span class="sxs-lookup"><span data-stu-id="f50fb-170">14,22,9,66,78,...</span></span> |<span data-ttu-id="f50fb-171">int[600]</span><span class="sxs-lookup"><span data-stu-id="f50fb-171">int[600]</span></span>|

1. <span data-ttu-id="f50fb-172">在 `Main` 方法之后为输入数据创建一个类：</span><span class="sxs-lookup"><span data-stu-id="f50fb-172">Create a class for your input data, after the `Main` method:</span></span>

    [!code-csharp[MovieReviewClass](./snippets/text-classification-tf/csharp/Program.cs#MovieReviewClass "Declare movie review type")]

    <span data-ttu-id="f50fb-173">输入数据类 `MovieReview` 具有用于用户评论 (`ReviewText`) 的 `string`。</span><span class="sxs-lookup"><span data-stu-id="f50fb-173">The input data class, `MovieReview`, has a `string` for user comments (`ReviewText`).</span></span>

1. <span data-ttu-id="f50fb-174">在 `Main` 方法之后为可变长度特征创建一个类：</span><span class="sxs-lookup"><span data-stu-id="f50fb-174">Create a class for the variable length features, after the `Main` method:</span></span>

    [!code-csharp[VariableLengthFeatures](./snippets/text-classification-tf/csharp/Program.cs#VariableLengthFeatures "Declare variable length features type")]

    <span data-ttu-id="f50fb-175">`VariableLengthFeatures` 属性的 [VectorType](xref:Microsoft.ML.Data.VectorTypeAttribute.%23ctor%2A) 特性用于将其指定为向量。</span><span class="sxs-lookup"><span data-stu-id="f50fb-175">The `VariableLengthFeatures` property has a [VectorType](xref:Microsoft.ML.Data.VectorTypeAttribute.%23ctor%2A) attribute to designate it as a vector.</span></span>  <span data-ttu-id="f50fb-176">所有向量元素都必须是同一类型。</span><span class="sxs-lookup"><span data-stu-id="f50fb-176">All of the vector elements must be the same type.</span></span> <span data-ttu-id="f50fb-177">在具有大量列的数据集中，将多个列作为单个向量加载会减少应用数据转换时的数据传递次数。</span><span class="sxs-lookup"><span data-stu-id="f50fb-177">In data sets with a large number of columns, loading multiple columns as a single vector reduces the number of data passes when you apply data transformations.</span></span>

    <span data-ttu-id="f50fb-178">此类在 `ResizeFeatures` 操作中使用。</span><span class="sxs-lookup"><span data-stu-id="f50fb-178">This class is used in the `ResizeFeatures` action.</span></span> <span data-ttu-id="f50fb-179">它的属性名称（本例中只有一个）用于指示 DataView 中的哪些列可用作自定义映射操作的输入。</span><span class="sxs-lookup"><span data-stu-id="f50fb-179">The names of its properties (in this case only one) are used to indicate which columns in the DataView can be used as the _input_ to the custom mapping action.</span></span>

1. <span data-ttu-id="f50fb-180">在 `Main` 方法之后为固定长度特征创建一个类：</span><span class="sxs-lookup"><span data-stu-id="f50fb-180">Create a class for the fixed length features, after the `Main` method:</span></span>

    [!code-csharp[FixedLengthFeatures](./snippets/text-classification-tf/csharp/Program.cs#FixedLengthFeatures)]

    <span data-ttu-id="f50fb-181">此类在 `ResizeFeatures` 操作中使用。</span><span class="sxs-lookup"><span data-stu-id="f50fb-181">This class is used in the `ResizeFeatures` action.</span></span> <span data-ttu-id="f50fb-182">它的属性名称（本例中只有一个）用于指示 DataView 中的哪些列可用作自定义映射操作的输出。</span><span class="sxs-lookup"><span data-stu-id="f50fb-182">The names of its properties (in this case only one) are used to indicate which columns in the DataView can be used as the _output_ of the custom mapping action.</span></span>

    <span data-ttu-id="f50fb-183">请注意，属性 `Features` 的名称由 TensorFlow 模型确定。</span><span class="sxs-lookup"><span data-stu-id="f50fb-183">Note that the name of the property `Features` is determined by the TensorFlow model.</span></span> <span data-ttu-id="f50fb-184">此属性名称无法更改。</span><span class="sxs-lookup"><span data-stu-id="f50fb-184">You cannot change this property name.</span></span>

1. <span data-ttu-id="f50fb-185">在 `Main` 方法之后为预测创建一个类：</span><span class="sxs-lookup"><span data-stu-id="f50fb-185">Create a class for the prediction after the `Main` method:</span></span>

    [!code-csharp[Prediction](./snippets/text-classification-tf/csharp/Program.cs#Prediction "Declare prediction class")]

    <span data-ttu-id="f50fb-186">`MovieReviewSentimentPrediction` 是在训练模型后使用的预测类。</span><span class="sxs-lookup"><span data-stu-id="f50fb-186">`MovieReviewSentimentPrediction` is the prediction class used after the model training.</span></span> <span data-ttu-id="f50fb-187">`MovieReviewSentimentPrediction` 有一个 `float` 数组 (`Prediction`) 和一个 `VectorType` 属性。</span><span class="sxs-lookup"><span data-stu-id="f50fb-187">`MovieReviewSentimentPrediction` has a single `float` array (`Prediction`) and a `VectorType` attribute.</span></span>

### <a name="create-the-mlcontext-lookup-dictionary-and-action-to-resize-features"></a><span data-ttu-id="f50fb-188">创建 MLContext、查找字典以及用于调整特征大小的操作</span><span class="sxs-lookup"><span data-stu-id="f50fb-188">Create the MLContext, lookup dictionary, and action to resize features</span></span>

<span data-ttu-id="f50fb-189">[MLContext 类](xref:Microsoft.ML.MLContext)是所有 ML.NET 操作的起点。</span><span class="sxs-lookup"><span data-stu-id="f50fb-189">The [MLContext class](xref:Microsoft.ML.MLContext) is a starting point for all ML.NET operations.</span></span> <span data-ttu-id="f50fb-190">初始化 `mlContext` 会创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="f50fb-190">Initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="f50fb-191">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="f50fb-191">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

1. <span data-ttu-id="f50fb-192">使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 mlContext 变量：</span><span class="sxs-lookup"><span data-stu-id="f50fb-192">Replace the `Console.WriteLine("Hello World!")` line in the `Main` method with the following code to declare and initialize the mlContext variable:</span></span>

   [!code-csharp[CreateMLContext](./snippets/text-classification-tf/csharp/Program.cs#CreateMLContext "Create the ML Context")]

1. <span data-ttu-id="f50fb-193">通过使用 [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) 方法创建字典来将单词编码为整数，以便从文件中加载映射数据，如下表所示：</span><span class="sxs-lookup"><span data-stu-id="f50fb-193">Create a dictionary to encode words as integers by using the [`LoadFromTextFile`](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%2A) method to load mapping data from a file, as seen in the following table:</span></span>

    |<span data-ttu-id="f50fb-194">字</span><span class="sxs-lookup"><span data-stu-id="f50fb-194">Word</span></span>     |<span data-ttu-id="f50fb-195">Index</span><span class="sxs-lookup"><span data-stu-id="f50fb-195">Index</span></span>    |
    |---------|---------|
    |<span data-ttu-id="f50fb-196">kids</span><span class="sxs-lookup"><span data-stu-id="f50fb-196">kids</span></span>     |  <span data-ttu-id="f50fb-197">362</span><span class="sxs-lookup"><span data-stu-id="f50fb-197">362</span></span>    |
    |<span data-ttu-id="f50fb-198">want</span><span class="sxs-lookup"><span data-stu-id="f50fb-198">want</span></span>     |  <span data-ttu-id="f50fb-199">181</span><span class="sxs-lookup"><span data-stu-id="f50fb-199">181</span></span>    |
    |<span data-ttu-id="f50fb-200">wrong</span><span class="sxs-lookup"><span data-stu-id="f50fb-200">wrong</span></span>    |  <span data-ttu-id="f50fb-201">355</span><span class="sxs-lookup"><span data-stu-id="f50fb-201">355</span></span>    |
    |<span data-ttu-id="f50fb-202">effects</span><span class="sxs-lookup"><span data-stu-id="f50fb-202">effects</span></span>  |  <span data-ttu-id="f50fb-203">302</span><span class="sxs-lookup"><span data-stu-id="f50fb-203">302</span></span>    |
    |<span data-ttu-id="f50fb-204">feeling</span><span class="sxs-lookup"><span data-stu-id="f50fb-204">feeling</span></span>  |  <span data-ttu-id="f50fb-205">547</span><span class="sxs-lookup"><span data-stu-id="f50fb-205">547</span></span>    |

    <span data-ttu-id="f50fb-206">添加下面的代码，创建查找映射：</span><span class="sxs-lookup"><span data-stu-id="f50fb-206">Add the code below to create the lookup map:</span></span>

    [!code-csharp[CreateLookupMap](./snippets/text-classification-tf/csharp/Program.cs#CreateLookupMap)]

1. <span data-ttu-id="f50fb-207">添加一个 `Action`，将可变长度单词整数数组的大小调整为固定大小的整数数组，再加上下面的几行代码：</span><span class="sxs-lookup"><span data-stu-id="f50fb-207">Add an `Action` to resize the variable length word integer array to an integer array of fixed size, with the next lines of code:</span></span>

   [!code-csharp[ResizeFeatures](./snippets/text-classification-tf/csharp/Program.cs#ResizeFeatures)]

## <a name="load-the-pre-trained-tensorflow-model"></a><span data-ttu-id="f50fb-208">加载预先训练的 TensorFlow 模型</span><span class="sxs-lookup"><span data-stu-id="f50fb-208">Load the pre-trained TensorFlow model</span></span>

1. <span data-ttu-id="f50fb-209">添加代码以加载 TensorFlow 模型：</span><span class="sxs-lookup"><span data-stu-id="f50fb-209">Add code to load the TensorFlow model:</span></span>

    [!code-csharp[LoadTensorFlowModel](./snippets/text-classification-tf/csharp/Program.cs#LoadTensorFlowModel)]

    <span data-ttu-id="f50fb-210">加载模型后，可以提取其输入和输出架构。</span><span class="sxs-lookup"><span data-stu-id="f50fb-210">Once the model is loaded, you can extract its input and output schema.</span></span> <span data-ttu-id="f50fb-211">所显示的架构仅供参考和学习。</span><span class="sxs-lookup"><span data-stu-id="f50fb-211">The schemas are displayed for interest and learning only.</span></span> <span data-ttu-id="f50fb-212">最终应用程序不需要此代码即可运行：</span><span class="sxs-lookup"><span data-stu-id="f50fb-212">You do not need this code for the final application to function:</span></span>

    [!code-csharp[GetModelSchema](./snippets/text-classification-tf/csharp/Program.cs#GetModelSchema)]

    <span data-ttu-id="f50fb-213">输入架构是整数编码单词的固定长度数组。</span><span class="sxs-lookup"><span data-stu-id="f50fb-213">The input schema is the fixed-length array of integer encoded words.</span></span> <span data-ttu-id="f50fb-214">输出架构是概率的浮点数组，指示评论的情绪是负面情绪还是正面情绪。</span><span class="sxs-lookup"><span data-stu-id="f50fb-214">The output schema is a float array of probabilities indicating whether a review's sentiment is negative, or positive .</span></span> <span data-ttu-id="f50fb-215">这些值的总和为 1，因为正面情绪的概率与负面情绪的概率相互补足。</span><span class="sxs-lookup"><span data-stu-id="f50fb-215">These values sum to 1, as the probability of being positive is the complement of the probability of the sentiment being negative.</span></span>

## <a name="create-the-mlnet-pipeline"></a><span data-ttu-id="f50fb-216">创建 ML.NET 管道</span><span class="sxs-lookup"><span data-stu-id="f50fb-216">Create the ML.NET pipeline</span></span>

1. <span data-ttu-id="f50fb-217">创建管道并使用 [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) 转换将输入文本拆分为单词，从而将文本拆分为单词以作为下一行代码：</span><span class="sxs-lookup"><span data-stu-id="f50fb-217">Create the pipeline and split the input text into words using [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) transform to break the text into words as the next line of code:</span></span>

   [!code-csharp[TokenizeIntoWords](./snippets/text-classification-tf/csharp/Program.cs#TokenizeIntoWords)]

   <span data-ttu-id="f50fb-218">[TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) 转换使用空格将文本/字符串分析为单词。</span><span class="sxs-lookup"><span data-stu-id="f50fb-218">The [TokenizeIntoWords](xref:Microsoft.ML.TextCatalog.TokenizeIntoWords%2A) transform uses spaces to parse the text/string into words.</span></span> <span data-ttu-id="f50fb-219">它将创建一个新列，并基于用户定义的分隔符将每个输入字符串拆分为子字符串的向量。</span><span class="sxs-lookup"><span data-stu-id="f50fb-219">It creates a new column and splits each input string to a vector of substrings based on the user-defined separator.</span></span>

1. <span data-ttu-id="f50fb-220">使用你在上面声明的查找表将单词映射到其整数编码：</span><span class="sxs-lookup"><span data-stu-id="f50fb-220">Map the words onto their integer encoding using the lookup table that you declared above:</span></span>

    [!code-csharp[MapValue](./snippets/text-classification-tf/csharp/Program.cs#MapValue)]

1. <span data-ttu-id="f50fb-221">将可变长度整数编码调整为模型所需的固定长度：</span><span class="sxs-lookup"><span data-stu-id="f50fb-221">Resize the variable length integer encodings to the fixed-length one required by the model:</span></span>

    [!code-csharp[CustomMapping](./snippets/text-classification-tf/csharp/Program.cs#CustomMapping)]

1. <span data-ttu-id="f50fb-222">使用加载的 TensorFlow 模型对输入进行分类：</span><span class="sxs-lookup"><span data-stu-id="f50fb-222">Classify the input with the loaded TensorFlow model:</span></span>

    [!code-csharp[ScoreTensorFlowModel](./snippets/text-classification-tf/csharp/Program.cs#ScoreTensorFlowModel)]

    <span data-ttu-id="f50fb-223">TensorFlow 模型输出称为 `Prediction/Softmax`。</span><span class="sxs-lookup"><span data-stu-id="f50fb-223">The TensorFlow model output is called `Prediction/Softmax`.</span></span> <span data-ttu-id="f50fb-224">请注意，名称 `Prediction/Softmax` 由 TensorFlow 模型确定。</span><span class="sxs-lookup"><span data-stu-id="f50fb-224">Note that the name `Prediction/Softmax` is determined by the TensorFlow model.</span></span> <span data-ttu-id="f50fb-225">此名称无法更改。</span><span class="sxs-lookup"><span data-stu-id="f50fb-225">You cannot change this name.</span></span>

1. <span data-ttu-id="f50fb-226">为输出预测创建一个新列：</span><span class="sxs-lookup"><span data-stu-id="f50fb-226">Create a new column for the output prediction:</span></span>

    [!code-csharp[SnippetCopyColumns](./snippets/text-classification-tf/csharp/Program.cs#SnippetCopyColumns)]

    <span data-ttu-id="f50fb-227">需要将 `Prediction/Softmax` 列复制到其名称可用作 C# 类中的属性的列：`Prediction`。</span><span class="sxs-lookup"><span data-stu-id="f50fb-227">You need to copy the `Prediction/Softmax` column into one with a name that can be used as a property in a C# class: `Prediction`.</span></span> <span data-ttu-id="f50fb-228">不允许在 C# 属性名称中使用 `/` 字符。</span><span class="sxs-lookup"><span data-stu-id="f50fb-228">The `/` character is not allowed in a C# property name.</span></span>

## <a name="create-the-mlnet-model-from-the-pipeline"></a><span data-ttu-id="f50fb-229">从管道创建 ML.NET 模型</span><span class="sxs-lookup"><span data-stu-id="f50fb-229">Create the ML.NET model from the pipeline</span></span>

1. <span data-ttu-id="f50fb-230">添加代码以从管道创建模型：</span><span class="sxs-lookup"><span data-stu-id="f50fb-230">Add the code to create the model from the pipeline:</span></span>

    [!code-csharp[SnippetCreateModel](./snippets/text-classification-tf/csharp/Program.cs#SnippetCreateModel)]

    <span data-ttu-id="f50fb-231">通过调用 `Fit` 方法，从管道中的估算器链创建 ML.NET 模型。</span><span class="sxs-lookup"><span data-stu-id="f50fb-231">An ML.NET model is created from the chain of estimators in the pipeline by calling the `Fit` method.</span></span> <span data-ttu-id="f50fb-232">在这种情况下，我们不会调整任何数据以创建模型，因为 TensorFlow 模型此前已经过训练。</span><span class="sxs-lookup"><span data-stu-id="f50fb-232">In this case, we are not fitting any data to create the model, as the TensorFlow model has already been previously trained.</span></span> <span data-ttu-id="f50fb-233">我们提供一个空的数据视图对象，以满足 `Fit` 方法的要求。</span><span class="sxs-lookup"><span data-stu-id="f50fb-233">We supply an empty data view object to satisfy the requirements of the `Fit` method.</span></span>

## <a name="use-the-model-to-make-a-prediction"></a><span data-ttu-id="f50fb-234">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="f50fb-234">Use the model to make a prediction</span></span>

1. <span data-ttu-id="f50fb-235">在 `Main` 方法下添加 `PredictSentiment` 方法：</span><span class="sxs-lookup"><span data-stu-id="f50fb-235">Add the `PredictSentiment` method below the `Main` method:</span></span>

    ```csharp
    public static void PredictSentiment(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. <span data-ttu-id="f50fb-236">添加以下代码以创建 `PredictionEngine` 作为 `PredictSentiment()` 方法中的第一行：</span><span class="sxs-lookup"><span data-stu-id="f50fb-236">Add the following code to create the `PredictionEngine` as the first line in the `PredictSentiment()` method:</span></span>

    [!code-csharp[CreatePredictionEngine](./snippets/text-classification-tf/csharp/Program.cs#CreatePredictionEngine)]

    <span data-ttu-id="f50fb-237">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="f50fb-237">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to perform a prediction on a single instance of data.</span></span> <span data-ttu-id="f50fb-238">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="f50fb-238">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="f50fb-239">可以在单线程环境或原型环境中使用。</span><span class="sxs-lookup"><span data-stu-id="f50fb-239">It's acceptable to use in single-threaded or prototype environments.</span></span> <span data-ttu-id="f50fb-240">为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="f50fb-240">For improved performance and thread safety in production environments, use the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span> <span data-ttu-id="f50fb-241">请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。</span><span class="sxs-lookup"><span data-stu-id="f50fb-241">See this guide on how to [use `PredictionEnginePool` in an ASP.NET Core Web API](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).</span></span>

    > [!NOTE]
    > <span data-ttu-id="f50fb-242">`PredictionEnginePool` 服务扩展目前处于预览状态。</span><span class="sxs-lookup"><span data-stu-id="f50fb-242">`PredictionEnginePool` service extension is currently in preview.</span></span>

1. <span data-ttu-id="f50fb-243">通过创建一个 `MovieReview` 实例，在 `Predict()` 方法中添加一个注释来测试定型模型的预测：</span><span class="sxs-lookup"><span data-stu-id="f50fb-243">Add a comment to test the trained model's prediction in the `Predict()` method by creating an instance of `MovieReview`:</span></span>

    [!code-csharp[CreateTestData](./snippets/text-classification-tf/csharp/Program.cs#CreateTestData)]

1. <span data-ttu-id="f50fb-244">通过在 `PredictSentiment()` 方法中添加接下来的几行代码，将测试评论数据传递到 `Prediction Engine`：</span><span class="sxs-lookup"><span data-stu-id="f50fb-244">Pass the test comment data to the `Prediction Engine` by adding the next lines of code in the `PredictSentiment()` method:</span></span>

    [!code-csharp[Predict](./snippets/text-classification-tf/csharp/Program.cs#Predict)]

1. <span data-ttu-id="f50fb-245">[Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 函数对单行数据进行预测：</span><span class="sxs-lookup"><span data-stu-id="f50fb-245">The [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) function makes a prediction on a single row of data:</span></span>

    |<span data-ttu-id="f50fb-246">Property</span><span class="sxs-lookup"><span data-stu-id="f50fb-246">Property</span></span>| <span data-ttu-id="f50fb-247">值</span><span class="sxs-lookup"><span data-stu-id="f50fb-247">Value</span></span>|<span data-ttu-id="f50fb-248">类型</span><span class="sxs-lookup"><span data-stu-id="f50fb-248">Type</span></span>|
    |-------------|-----------------------|------|
    |<span data-ttu-id="f50fb-249">预测</span><span class="sxs-lookup"><span data-stu-id="f50fb-249">Prediction</span></span>|<span data-ttu-id="f50fb-250">[0.5459937, 0.454006255]</span><span class="sxs-lookup"><span data-stu-id="f50fb-250">[0.5459937, 0.454006255]</span></span>|<span data-ttu-id="f50fb-251">float[]</span><span class="sxs-lookup"><span data-stu-id="f50fb-251">float[]</span></span>|

1. <span data-ttu-id="f50fb-252">使用以下代码显示情绪预测：</span><span class="sxs-lookup"><span data-stu-id="f50fb-252">Display sentiment prediction using the following code:</span></span>

    [!code-csharp[DisplayPredictions](./snippets/text-classification-tf/csharp/Program.cs#DisplayPredictions)]

1. <span data-ttu-id="f50fb-253">在 `Main` 方法末尾添加对 `PredictSentiment` 的调用：</span><span class="sxs-lookup"><span data-stu-id="f50fb-253">Add a call to `PredictSentiment` at the end of the `Main` method:</span></span>

    [!code-csharp[CallPredictSentiment](./snippets/text-classification-tf/csharp/Program.cs#CallPredictSentiment)]

## <a name="results"></a><span data-ttu-id="f50fb-254">结果</span><span class="sxs-lookup"><span data-stu-id="f50fb-254">Results</span></span>

<span data-ttu-id="f50fb-255">生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="f50fb-255">Build and run your application.</span></span>

<span data-ttu-id="f50fb-256">结果应如下所示。</span><span class="sxs-lookup"><span data-stu-id="f50fb-256">Your results should be similar to the following.</span></span> <span data-ttu-id="f50fb-257">处理期间将显示消息。</span><span class="sxs-lookup"><span data-stu-id="f50fb-257">During processing, messages are displayed.</span></span> <span data-ttu-id="f50fb-258">你可能会看到警告或处理消息。</span><span class="sxs-lookup"><span data-stu-id="f50fb-258">You may see warnings, or processing messages.</span></span> <span data-ttu-id="f50fb-259">为简便起见，已从以下结果中删除这些消息。</span><span class="sxs-lookup"><span data-stu-id="f50fb-259">These messages have been removed from the following results for clarity.</span></span>

```console
Number of classes: 2
Is sentiment/review positive ? Yes
```

<span data-ttu-id="f50fb-260">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="f50fb-260">Congratulations!</span></span> <span data-ttu-id="f50fb-261">现已通过在 ML.NET 中重用预先训练的 `TensorFlow` 模型成功生成用于分类和预测消息情绪的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="f50fb-261">You've now successfully built a machine learning model for classifying and predicting messages sentiment by reusing a pre-trained `TensorFlow` model in ML.NET.</span></span>

<span data-ttu-id="f50fb-262">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TextClassificationTF) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="f50fb-262">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TextClassificationTF) repository.</span></span>

<span data-ttu-id="f50fb-263">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="f50fb-263">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="f50fb-264">加载预先训练的 TensorFlow 模型</span><span class="sxs-lookup"><span data-stu-id="f50fb-264">Load a pre-trained TensorFlow model</span></span>
> * <span data-ttu-id="f50fb-265">将网站评论文本转换为适用于模型的特征</span><span class="sxs-lookup"><span data-stu-id="f50fb-265">Transform website comment text into features suitable for the model</span></span>
> * <span data-ttu-id="f50fb-266">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="f50fb-266">Use the model to make a prediction</span></span>
