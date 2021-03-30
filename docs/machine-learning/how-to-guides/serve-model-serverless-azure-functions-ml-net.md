---
title: 将模型部署到 Azure Functions
description: 使用 Azure Functions 通过 Internet 提供 ML.NET 情绪分析机器学习模型进行预测
ms.date: 02/21/2020
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to
ms.openlocfilehash: 90b52aa295e224eb3744d2576b9a146e4e90dced
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875884"
---
# <a name="deploy-a-model-to-azure-functions"></a><span data-ttu-id="e1fb7-103">将模型部署到 Azure Functions</span><span class="sxs-lookup"><span data-stu-id="e1fb7-103">Deploy a model to Azure Functions</span></span>

<span data-ttu-id="e1fb7-104">了解如何通过 Azure Functions 无服务器环境部署预先训练的 ML.NET 机器学习模型，以便通过 HTTP 进行预测。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-104">Learn how to deploy a pre-trained ML.NET machine learning model for predictions over HTTP through an Azure Functions serverless environment.</span></span>

> [!NOTE]
> <span data-ttu-id="e1fb7-105">此示例运行 `PredictionEnginePool` 服务的预览版本。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-105">This sample runs a preview version of the `PredictionEnginePool` service.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e1fb7-106">先决条件</span><span class="sxs-lookup"><span data-stu-id="e1fb7-106">Prerequisites</span></span>

- <span data-ttu-id="e1fb7-107">安装了“.NET Core 跨平台开发”和“Azure 部署”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-107">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" and "Azure development" workloads installed.</span></span>
- [<span data-ttu-id="e1fb7-108">Azure Functions 工具</span><span class="sxs-lookup"><span data-stu-id="e1fb7-108">Azure Functions Tools</span></span>](/azure/azure-functions/functions-develop-vs#check-your-tools-version)
- <span data-ttu-id="e1fb7-109">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e1fb7-109">PowerShell</span></span>
- <span data-ttu-id="e1fb7-110">预先定型的模型。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-110">Pre-trained model.</span></span> <span data-ttu-id="e1fb7-111">使用 [ML.NET 情绪分析教程](../tutorials/sentiment-analysis.md)生成自己的模型，或下载此[预先训练的情绪分析机器学习模型](https://github.com/dotnet/samples/blob/main/machine-learning/models/sentimentanalysis/sentiment_model.zip)</span><span class="sxs-lookup"><span data-stu-id="e1fb7-111">Use the [ML.NET Sentiment Analysis tutorial](../tutorials/sentiment-analysis.md) to build your own model or download this [pre-trained sentiment analysis machine learning model](https://github.com/dotnet/samples/blob/main/machine-learning/models/sentimentanalysis/sentiment_model.zip)</span></span>

## <a name="azure-functions-sample-overview"></a><span data-ttu-id="e1fb7-112">Azure Functions 示例概述</span><span class="sxs-lookup"><span data-stu-id="e1fb7-112">Azure Functions sample overview</span></span>

<span data-ttu-id="e1fb7-113">此示例是 C# HTTP 触发器 Azure Functions 应用程序  ，它使用预先定型的二进制分类模型将文本的情绪分类为消极或积极。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-113">This sample is a **C# HTTP Trigger Azure Functions application** that uses a pretrained binary classification model to categorize the sentiment of text as positive or negative.</span></span> <span data-ttu-id="e1fb7-114">Azure Functions 提供了一种简便的方法，可以在云中托管的无服务器环境中大规模运行少量代码。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-114">Azure Functions provides an easy way to run small pieces of code at scale on a managed serverless environment in the cloud.</span></span> <span data-ttu-id="e1fb7-115">若要获取此示例的代码，请参阅 GitHub 上的 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction) 存储库。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-115">The code for this sample can be found on the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction) repository on GitHub.</span></span>

## <a name="create-azure-functions-project"></a><span data-ttu-id="e1fb7-116">创建 Azure Functions 项目</span><span class="sxs-lookup"><span data-stu-id="e1fb7-116">Create Azure Functions project</span></span>

1. <span data-ttu-id="e1fb7-117">打开 Visual Studio 2017。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-117">Open Visual Studio 2017.</span></span> <span data-ttu-id="e1fb7-118">从菜单栏中选择“文件”   > “新建”   > “项目”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-118">Select **File** > **New** > **Project** from the menu bar.</span></span> <span data-ttu-id="e1fb7-119">在“新项目”  对话框中，依次选择“Visual C#”  节点和“云”  节点。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-119">In the **New Project** dialog, select the **Visual C#** node followed by the **Cloud** node.</span></span> <span data-ttu-id="e1fb7-120">然后，选择“Azure Functions”  项目模板。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-120">Then select the **Azure Functions** project template.</span></span> <span data-ttu-id="e1fb7-121">在“名称”  文本框中，键入“SentimentAnalysisFunctionsApp”，再选择“确定”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-121">In the **Name** text box, type "SentimentAnalysisFunctionsApp" and then select the **OK** button.</span></span>
1. <span data-ttu-id="e1fb7-122">在“新项目”  对话框中，打开项目选项上方的下拉列表，并选择“Azure Functions v2 (.NET Core)”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-122">In the **New Project** dialog, open the dropdown above the project options and select **Azure Functions v2 (.NET Core)**.</span></span> <span data-ttu-id="e1fb7-123">然后，依次选择“Http 触发器”  项目和“确定”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-123">Then, select the **Http trigger** project and then select the **OK** button.</span></span>
1. <span data-ttu-id="e1fb7-124">在项目中创建“MLModels”  目录，用于保存模型：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-124">Create a directory named *MLModels* in your project to save your model:</span></span>

    <span data-ttu-id="e1fb7-125">在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新文件夹”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-125">In **Solution Explorer**, right-click on your project and select **Add** > **New Folder**.</span></span> <span data-ttu-id="e1fb7-126">键入“MLModels”，再按 Enter。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-126">Type "MLModels" and hit Enter.</span></span>

1. <span data-ttu-id="e1fb7-127">安装 Microsoft.ML NuGet 包  版本 1.3.1  ：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-127">Install the **Microsoft.ML NuGet Package** version **1.3.1**:</span></span>

    <span data-ttu-id="e1fb7-128">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-128">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e1fb7-129">将“nuget.org”选择为“包源”，选择“浏览”选项卡并搜索“Microsoft.ML”  ，在列表中选择该包，然后选择“安装”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-129">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML**, select that package in the list, and select the **Install** button.</span></span> <span data-ttu-id="e1fb7-130">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-130">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

1. <span data-ttu-id="e1fb7-131">安装 Microsoft.Azure.Functions.Extensions NuGet 包  ：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-131">Install the **Microsoft.Azure.Functions.Extensions NuGet Package**:</span></span>

    <span data-ttu-id="e1fb7-132">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-132">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e1fb7-133">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.Azure.Functions.Extensions”，在列表中选择该包，再选择“安装”按钮   。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-133">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.Azure.Functions.Extensions**, select that package in the list, and select the **Install** button.</span></span> <span data-ttu-id="e1fb7-134">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-134">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

1. <span data-ttu-id="e1fb7-135">安装 0.15.1 版本的 Microsoft.Extensions.ML NuGet 包   ：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-135">Install the **Microsoft.Extensions.ML NuGet Package** version **0.15.1**:</span></span>

    <span data-ttu-id="e1fb7-136">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-136">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e1fb7-137">选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.Extensions.ML”，在列表中选择包，再选择“安装”按钮   。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-137">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.Extensions.ML**, select that package in the list, and select the **Install** button.</span></span> <span data-ttu-id="e1fb7-138">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-138">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

1. <span data-ttu-id="e1fb7-139">安装 1.0.31 版本的 Microsoft.NET.Sdk.Functions NuGet 包   ：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-139">Install the **Microsoft.NET.Sdk.Functions NuGet Package** version **1.0.31**:</span></span>

    <span data-ttu-id="e1fb7-140">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-140">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="e1fb7-141">选择“nuget.org”作为包源，然后选择“已安装”选项卡并搜索“Microsoft.NET.Sdk.Functions”，在列表中选择该包，再从“版本”下拉列表中选择“1.0.31”，并选择“更新”按钮   。 </span><span class="sxs-lookup"><span data-stu-id="e1fb7-141">Choose "nuget.org" as the Package source, select the Installed tab, search for **Microsoft.NET.Sdk.Functions**, select that package in the list, select **1.0.31** from the Version dropdown, and select the **Update** button.</span></span> <span data-ttu-id="e1fb7-142">选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-142">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>

## <a name="add-pre-trained-model-to-project"></a><span data-ttu-id="e1fb7-143">将预先训练的模型添加到项目</span><span class="sxs-lookup"><span data-stu-id="e1fb7-143">Add pre-trained model to project</span></span>

1. <span data-ttu-id="e1fb7-144">将预生成的模型复制到 MLModels  文件夹。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-144">Copy your pre-built model to the *MLModels* folder.</span></span>
1. <span data-ttu-id="e1fb7-145">在“解决方案资源管理器”中，右键单击预生成的模型文件，并选择“属性”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-145">In Solution Explorer, right-click your pre-built model file and select **Properties**.</span></span> <span data-ttu-id="e1fb7-146">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-146">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

## <a name="create-azure-function-to-analyze-sentiment"></a><span data-ttu-id="e1fb7-147">创建 Azure 函数用于分析情绪</span><span class="sxs-lookup"><span data-stu-id="e1fb7-147">Create Azure Function to analyze sentiment</span></span>

<span data-ttu-id="e1fb7-148">创建情绪预测类。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-148">Create a class to predict sentiment.</span></span> <span data-ttu-id="e1fb7-149">向项目添加一个新类：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-149">Add a new class to your project:</span></span>

1. <span data-ttu-id="e1fb7-150">在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新项”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-150">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="e1fb7-151">在“添加新项”  对话框中，选择“Azure 函数”  ，并将“名称”  字段更改为“AnalyzeSentiment.cs”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-151">In the **Add New Item** dialog box, select **Azure Function** and change the **Name** field to *AnalyzeSentiment.cs*.</span></span> <span data-ttu-id="e1fb7-152">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-152">Then, select the **Add** button.</span></span>

1. <span data-ttu-id="e1fb7-153">在“新 Azure 函数”  对话框中，选择“Http 触发器”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-153">In the **New Azure Function** dialog box, select **Http Trigger**.</span></span> <span data-ttu-id="e1fb7-154">然后，选择“确定”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-154">Then, select the **OK** button.</span></span>

    <span data-ttu-id="e1fb7-155">此时，AnalyzeSentiment.cs  文件在代码编辑器中打开。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-155">The *AnalyzeSentiment.cs* file opens in the code editor.</span></span> <span data-ttu-id="e1fb7-156">将以下 `using` 语句添加到 *AnalyzeSentiment.cs* 的顶部：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-156">Add the following `using` statement to the top of *AnalyzeSentiment.cs*:</span></span>

    [!code-csharp [AnalyzeUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L1-L11)]

    <span data-ttu-id="e1fb7-157">默认情况下，`AnalyzeSentiment` 类为 `static`。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-157">By default, the `AnalyzeSentiment` class is `static`.</span></span> <span data-ttu-id="e1fb7-158">确保从类定义中删除 `static` 关键字。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-158">Make sure to remove the `static` keyword from the class definition.</span></span>

    ```csharp
    public class AnalyzeSentiment
    {

    }
    ```

## <a name="create-data-models"></a><span data-ttu-id="e1fb7-159">创建数据模型</span><span class="sxs-lookup"><span data-stu-id="e1fb7-159">Create data models</span></span>

<span data-ttu-id="e1fb7-160">需要为输入数据和预测创建一些类。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-160">You need to create some classes for your input data and predictions.</span></span> <span data-ttu-id="e1fb7-161">向项目添加一个新类：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-161">Add a new class to your project:</span></span>

1. <span data-ttu-id="e1fb7-162">在项目中创建“DataModels”  目录，用于保存数据模型：在“解决方案资源管理器”中，右键单击项目，并依次选择“添加”>“新文件夹”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-162">Create a directory named *DataModels* in your project to save your data models: In Solution Explorer, right-click on your project and select **Add > New Folder**.</span></span> <span data-ttu-id="e1fb7-163">键入“DataModels”，再按 Enter。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-163">Type "DataModels" and hit Enter.</span></span>
2. <span data-ttu-id="e1fb7-164">在“解决方案资源管理器”中，右键单击“DataModels”  目录，再依次选择“添加”>“新项”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-164">In Solution Explorer, right-click the *DataModels* directory, and then select **Add > New Item**.</span></span>
3. <span data-ttu-id="e1fb7-165">在“添加新项”  对话框中，选择“类”  并将“名称”  字段更改为“SentimentData.cs”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-165">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *SentimentData.cs*.</span></span> <span data-ttu-id="e1fb7-166">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-166">Then, select the **Add** button.</span></span>

    <span data-ttu-id="e1fb7-167">“SentimentData.cs”  文件随即在代码编辑器中打开。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-167">The *SentimentData.cs* file opens in the code editor.</span></span> <span data-ttu-id="e1fb7-168">将以下 using 语句添加到 SentimentData.cs  顶部：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-168">Add the following using statement to the top of *SentimentData.cs*:</span></span>

    [!code-csharp [SentimentDataUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentData.cs#L1)]

    <span data-ttu-id="e1fb7-169">删除现有类定义，并将以下代码添加到 SentimentData.cs  文件：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-169">Remove the existing class definition and add the following code to the *SentimentData.cs* file:</span></span>

    [!code-csharp [SentimentData](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentData.cs#L5-L13)]

4. <span data-ttu-id="e1fb7-170">在“解决方案资源管理器”中，右键单击“DataModels”  目录，再依次选择“添加”>“新项”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-170">In Solution Explorer, right-click the *DataModels* directory, and then select **Add > New Item**.</span></span>
5. <span data-ttu-id="e1fb7-171">在“添加新项”  对话框中，选择“类”  ，并将“名称”  字段更改为“SentimentPrediction.cs”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-171">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *SentimentPrediction.cs*.</span></span> <span data-ttu-id="e1fb7-172">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-172">Then, select the **Add** button.</span></span> <span data-ttu-id="e1fb7-173">此时，SentimentPrediction.cs  文件在代码编辑器中打开。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-173">The *SentimentPrediction.cs* file opens in the code editor.</span></span> <span data-ttu-id="e1fb7-174">将以下 using 语句添加到 SentimentPrediction.cs  顶部：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-174">Add the following using statement to the top of *SentimentPrediction.cs*:</span></span>

    [!code-csharp [SentimentPredictionUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentPrediction.cs#L1)]

    <span data-ttu-id="e1fb7-175">删除现有类定义，并将以下代码添加到 SentimentPrediction.cs  文件：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-175">Remove the existing class definition and add the following code to the *SentimentPrediction.cs* file:</span></span>

    [!code-csharp [SentimentPrediction](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentPrediction.cs#L5-L14)]

    <span data-ttu-id="e1fb7-176">`SentimentPrediction` 继承自 `SentimentData`，后者提供对 `SentimentText` 属性中原始数据以及模型生成的输出的访问。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-176">`SentimentPrediction` inherits from `SentimentData` which provides access to the original data in the `SentimentText` property as well as the output generated by the model.</span></span>

## <a name="register-predictionenginepool-service"></a><span data-ttu-id="e1fb7-177">注册 PredictionEnginePool 服务</span><span class="sxs-lookup"><span data-stu-id="e1fb7-177">Register PredictionEnginePool service</span></span>

<span data-ttu-id="e1fb7-178">若要进行单一预测，必须创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-178">To make a single prediction, you have to create a [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602).</span></span> <span data-ttu-id="e1fb7-179">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-179">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="e1fb7-180">此外，必须在应用程序中的每一处所需位置创建它的实例。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-180">Additionally, you have to create an instance of it everywhere it is needed within your application.</span></span> <span data-ttu-id="e1fb7-181">随着应用程序的增长，此过程可能会变得难以管理。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-181">As your application grows, this process can become unmanageable.</span></span> <span data-ttu-id="e1fb7-182">为了提高性能和线程安全，请结合使用依赖项注入和 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-182">For improved performance and thread safety, use a combination of dependency injection and the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span>

<span data-ttu-id="e1fb7-183">若要详细了解[依赖项注入](https://en.wikipedia.org/wiki/Dependency_injection)，请单击下面的链接。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-183">The following link provides more information if you want to learn more about [dependency injection](https://en.wikipedia.org/wiki/Dependency_injection).</span></span>

1. <span data-ttu-id="e1fb7-184">在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新项”  。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-184">In **Solution Explorer**, right-click the project, and then select **Add** > **New Item**.</span></span>
1. <span data-ttu-id="e1fb7-185">在“添加新项”对话框中，选择“类”并将“名称”字段更改为“Startup.cs”     。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-185">In the **Add New Item** dialog box, select **Class** and change the **Name** field to *Startup.cs*.</span></span> <span data-ttu-id="e1fb7-186">然后，选择“添加”  按钮。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-186">Then, select the **Add** button.</span></span>
1. <span data-ttu-id="e1fb7-187">将以下 using 语句添加到 Startup.cs 的顶部： </span><span class="sxs-lookup"><span data-stu-id="e1fb7-187">Add the following using statements to the top of *Startup.cs*:</span></span>

    [!code-csharp [StartupUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L1-L6)]

1. <span data-ttu-id="e1fb7-188">删除 using 语句下的现有代码，并添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-188">Remove the existing code below the using statements and add the following code:</span></span>

    ```csharp
    [assembly: FunctionsStartup(typeof(Startup))]
    namespace SentimentAnalysisFunctionsApp
    {
        public class Startup : FunctionsStartup
        {

        }
    }
    ```

1. <span data-ttu-id="e1fb7-189">在 `Startup` 类中定义变量，以存储运行应用的环境以及模型所在的文件路径</span><span class="sxs-lookup"><span data-stu-id="e1fb7-189">Define variables to store the environment the app is running in and the file path where the model is located inside the `Startup` class</span></span>

    [!code-csharp [DefineStartupVars](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L13-L14)]

1. <span data-ttu-id="e1fb7-190">在它下面创建一个构造函数，以设置 `_environment` 和 `_modelPath` 变量的值。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-190">Below that, create a constructor to set the values of the `_environment` and `_modelPath` variables.</span></span> <span data-ttu-id="e1fb7-191">应用程序在本地运行时，默认环境为“开发”。 </span><span class="sxs-lookup"><span data-stu-id="e1fb7-191">When the application is running locally, the default environment is *Development*.</span></span>

    [!code-csharp [StartupCtor](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L16-L29)]

1. <span data-ttu-id="e1fb7-192">然后，在构造函数下面添加一个名为 `Configure` 的新方法，用于注册 `PredictionEnginePool` 服务。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-192">Then, add a new method called `Configure` to register the `PredictionEnginePool` service below the constructor.</span></span>

    [!code-csharp [ConfigureServices](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L31-L35)]

<span data-ttu-id="e1fb7-193">概括地讲，此代码在应用程序请求时自动初始化对象和服务供以后使用，无需手动执行初始化。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-193">At a high level, this code initializes the objects and services automatically for later use when requested by the application instead of having to manually do it.</span></span>

<span data-ttu-id="e1fb7-194">机器学习模型不是静态的。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-194">Machine learning models are not static.</span></span> <span data-ttu-id="e1fb7-195">随着新的训练数据变得可用，模型将重新训练和重新部署。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-195">As new training data becomes available, the model is retrained and redeployed.</span></span> <span data-ttu-id="e1fb7-196">将最新版本的模型引入应用程序的一种方法是重新部署整个应用程序。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-196">One way to get the latest version of the model into your application is to redeploy the entire application.</span></span> <span data-ttu-id="e1fb7-197">但这会导致应用程序关闭。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-197">However, this introduces application downtime.</span></span> <span data-ttu-id="e1fb7-198">`PredictionEnginePool` 服务提供了一种机制，用于在不使应用程序关闭的情况下重新加载已更新的模型。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-198">The `PredictionEnginePool` service provides a mechanism to reload an updated model without taking your application down.</span></span>

<span data-ttu-id="e1fb7-199">将 `watchForChanges` 参数设置为 `true`，则 `PredictionEnginePool` 会启动 [`FileSystemWatcher`](xref:System.IO.FileSystemWatcher)，用于侦听文件系统更改通知并在文件发生更改时引发事件。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-199">Set the `watchForChanges` parameter to `true`, and the `PredictionEnginePool` starts a [`FileSystemWatcher`](xref:System.IO.FileSystemWatcher) that listens to the file system change notifications and raises events when there is a change to the file.</span></span> <span data-ttu-id="e1fb7-200">这会提示 `PredictionEnginePool` 自动重新加载模型。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-200">This prompts the `PredictionEnginePool` to automatically reload the model.</span></span>

<span data-ttu-id="e1fb7-201">模型由 `modelName` 参数标识，因此更改时可以重新加载每个应用程序的多个模型。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-201">The model is identified by the `modelName` parameter so that more than one model per application can be reloaded upon change.</span></span>

> [!TIP]
> <span data-ttu-id="e1fb7-202">或者，如果使用远程存储的模型，则可以使用 `FromUri` 方法。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-202">Alternatively, you can use the `FromUri` method when working with models stored remotely.</span></span> <span data-ttu-id="e1fb7-203">`FromUri` 会轮询远程位置以获取更改，而不是监视文件更改事件。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-203">Rather than watching for file changed events, `FromUri` polls the remote location for changes.</span></span> <span data-ttu-id="e1fb7-204">轮询间隔默认为 5 分钟。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-204">The polling interval defaults to 5 minutes.</span></span> <span data-ttu-id="e1fb7-205">你可以根据应用程序的要求，增加或减少轮询间隔。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-205">You can increase or decrease the polling interval based on your application's requirements.</span></span> <span data-ttu-id="e1fb7-206">在下面的代码示例中，`PredictionEnginePool` 每分钟轮询存储在指定 URI 中的模型。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-206">In the code sample below, the `PredictionEnginePool` polls the model stored at the specified URI every minute.</span></span>
>
>```csharp
>builder.Services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
>   .FromUri(
>       modelName: "SentimentAnalysisModel",
>       uri:"https://github.com/dotnet/samples/raw/main/machine-learning/models/sentimentanalysis/sentiment_model.zip",
>       period: TimeSpan.FromMinutes(1));
>```

## <a name="load-the-model-into-the-function"></a><span data-ttu-id="e1fb7-207">将模型加载到函数中</span><span class="sxs-lookup"><span data-stu-id="e1fb7-207">Load the model into the function</span></span>

<span data-ttu-id="e1fb7-208">在 *AnalyzeSentiment* 类中插入以下代码：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-208">Insert the following code inside the *AnalyzeSentiment* class:</span></span>

[!code-csharp [AnalyzeCtor](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L18-L24)]

<span data-ttu-id="e1fb7-209">此代码通过将 `PredictionEnginePool` 传递给函数的构造函数（通过依赖项注入获得）来分配它。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-209">This code assigns the `PredictionEnginePool` by passing it to the function's constructor which you get via dependency injection.</span></span>

## <a name="use-the-model-to-make-predictions"></a><span data-ttu-id="e1fb7-210">使用模型进行预测</span><span class="sxs-lookup"><span data-stu-id="e1fb7-210">Use the model to make predictions</span></span>

<span data-ttu-id="e1fb7-211">将 AnalyzeSentiment  类中 Run  方法的现有实现替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-211">Replace the existing implementation of *Run* method in *AnalyzeSentiment* class with the following code:</span></span>

[!code-csharp [AnalyzeRunMethod](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L26-L45)]

<span data-ttu-id="e1fb7-212">执行 `Run` 方法时，来自 HTTP 请求的传入数据被反序列化并用作 `PredictionEnginePool` 的输入。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-212">When the `Run` method executes, the incoming data from the HTTP request is deserialized and used as input for the `PredictionEnginePool`.</span></span> <span data-ttu-id="e1fb7-213">然后，调用 `Predict` 方法，使用在 `Startup` 类中注册的 `SentimentAnalysisModel` 进行预测，并在成功时将结果返回给用户。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-213">The `Predict` method is then called to make predictions using the `SentimentAnalysisModel` registered in the `Startup` class and returns the results back to the user if successful.</span></span>

## <a name="test-locally"></a><span data-ttu-id="e1fb7-214">在本地测试</span><span class="sxs-lookup"><span data-stu-id="e1fb7-214">Test locally</span></span>

<span data-ttu-id="e1fb7-215">至此，已完成所有设置，是时候测试应用程序了：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-215">Now that everything is set up, it's time to test the application:</span></span>

1. <span data-ttu-id="e1fb7-216">运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="e1fb7-216">Run the application</span></span>
1. <span data-ttu-id="e1fb7-217">打开 PowerShell，并在提示符中输入代码（其中，PORT 是应用程序运行所在的端口）。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-217">Open PowerShell and enter the code into the prompt where PORT is the port your application is running on.</span></span> <span data-ttu-id="e1fb7-218">端口通常是 7071。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-218">Typically the port is 7071.</span></span>

    ```powershell
    Invoke-RestMethod "http://localhost:<PORT>/api/AnalyzeSentiment" -Method Post -Body (@{SentimentText="This is a very bad steak"} | ConvertTo-Json) -ContentType "application/json"
    ```

    <span data-ttu-id="e1fb7-219">如果成功，输出文本应如下所示：</span><span class="sxs-lookup"><span data-stu-id="e1fb7-219">If successful, the output should look similar to the text below:</span></span>

    ```powershell
    Negative
    ```

<span data-ttu-id="e1fb7-220">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="e1fb7-220">Congratulations!</span></span> <span data-ttu-id="e1fb7-221">你已成功使用 Azure Functions 通过 Internet 提供模型进行预测。</span><span class="sxs-lookup"><span data-stu-id="e1fb7-221">You have successfully served your model to make predictions over the internet using an Azure Function.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e1fb7-222">后续步骤</span><span class="sxs-lookup"><span data-stu-id="e1fb7-222">Next Steps</span></span>

- [<span data-ttu-id="e1fb7-223">部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="e1fb7-223">Deploy to Azure</span></span>](/azure/azure-functions/functions-develop-vs#publish-to-azure)
