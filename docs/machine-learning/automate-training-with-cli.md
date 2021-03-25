---
title: 使用 ML.NET CLI 自动进行模型训练
description: 了解如何使用 ML.NET CLI 工具通过命令行自动训练最佳模型。
ms.date: 06/03/2020
ms.custom: how-to, mlnet-tooling
ms.openlocfilehash: 0b230e4a517b6493abdb1ec975776fd286b654e3
ms.sourcegitcommit: b27645cb378d4e8137a267e5467ff31409acf6c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231402"
---
# <a name="automate-model-training-with-the-mlnet-cli"></a><span data-ttu-id="c0e79-103">使用 ML.NET CLI 自动进行模型训练</span><span class="sxs-lookup"><span data-stu-id="c0e79-103">Automate model training with the ML.NET CLI</span></span>

<span data-ttu-id="c0e79-104">ML.NET CLI 可为 .NET 开发人员自动生成模型。</span><span class="sxs-lookup"><span data-stu-id="c0e79-104">The ML.NET CLI automates model generation for .NET developers.</span></span>

<span data-ttu-id="c0e79-105">若要单独使用 ML.NET API（不使用 ML.NET AutoML CLI），需要选择训练程序（针对特定任务的机器学习算法的实现），以及要应用到数据的数据转换集（特征工程）。</span><span class="sxs-lookup"><span data-stu-id="c0e79-105">To use the ML.NET API by itself, (without the ML.NET AutoML CLI) you need to choose a trainer (implementation of a machine learning algorithm for a particular task), and the set of data transformations (feature engineering) to apply to your data.</span></span> <span data-ttu-id="c0e79-106">每个数据集的最佳管道各不相同，从所有选择中选择最佳算法增加了复杂性。</span><span class="sxs-lookup"><span data-stu-id="c0e79-106">The optimal pipeline will vary for each dataset and selecting the optimal algorithm from all the choices adds to the complexity.</span></span> <span data-ttu-id="c0e79-107">此外，每个算法都有一组要调整的超参数。</span><span class="sxs-lookup"><span data-stu-id="c0e79-107">Even further, each algorithm has a set of hyperparameters to be tuned.</span></span> <span data-ttu-id="c0e79-108">因此，可能会花费数周甚至数月时间进行机器学习模型优化，以尝试找到特征工程、学习算法和超参数的最佳组合。</span><span class="sxs-lookup"><span data-stu-id="c0e79-108">Hence, you can spend weeks and sometimes months on machine learning model optimization trying to find the best combinations of feature engineering, learning algorithms, and hyperparameters.</span></span>

<span data-ttu-id="c0e79-109">ML.NET CLI 使用自动机器学习 (AutoML) 简化了此过程。</span><span class="sxs-lookup"><span data-stu-id="c0e79-109">The ML.NET CLI simplifies this process using automated machine learning (AutoML).</span></span>

> [!NOTE]
> <span data-ttu-id="c0e79-110">本主题涉及目前处于预览状态的 ML.NET **CLI** 和 ML.NET **AutoML**，且材料可能会有所变化。</span><span class="sxs-lookup"><span data-stu-id="c0e79-110">This topic refers to ML.NET **CLI** and ML.NET **AutoML**, which are currently in Preview, and material may be subject to change.</span></span>

## <a name="what-is-the-mlnet-command-line-interface-cli"></a><span data-ttu-id="c0e79-111">什么是 ML.NET 命令行接口 (CLI)？</span><span class="sxs-lookup"><span data-stu-id="c0e79-111">What is the ML.NET command-line interface (CLI)?</span></span>

<span data-ttu-id="c0e79-112">ML.NET CLI 是一个 [.NET Core 工具](../core/tools/global-tools.md)。</span><span class="sxs-lookup"><span data-stu-id="c0e79-112">The ML.NET CLI is a [.NET Core tool](../core/tools/global-tools.md).</span></span> <span data-ttu-id="c0e79-113">安装后，你为其指定一个机器学习任务和一个训练数据集，而它将生成一个 ML.NET 模型以及要运行的 C# 代码，以便在应用程序中使用该模型。</span><span class="sxs-lookup"><span data-stu-id="c0e79-113">Once installed, you give it a machine learning task and a training dataset, and it generates an ML.NET model, as well as the C# code to run to use the model in your application.</span></span>

<span data-ttu-id="c0e79-114">如下图所示，生成高质量的 ML.NET 模型（序列化模型 .zip 文件）以及用于运行该模型/对该模型评分的示例 C# 代码非常简单。</span><span class="sxs-lookup"><span data-stu-id="c0e79-114">As shown in the following figure, it is simple to generate a high quality ML.NET model (serialized model .zip file) plus the sample C# code to run/score that model.</span></span> <span data-ttu-id="c0e79-115">此外，还会生成用于创建/训练该模型的 C# 代码，以便可以研究和迭代用于该生成的“最佳模型”的算法和设置。</span><span class="sxs-lookup"><span data-stu-id="c0e79-115">In addition, the C# code to create/train that model is also generated, so that you can research and iterate on the algorithm and settings used for that generated "best model".</span></span>

![在 ML.NET CLI 内部工作的 AutoML 引擎](media/automate-training-with-cli/cli-high-level-process.png)

<span data-ttu-id="c0e79-117">可以从自己的数据集生成这些资产而无需自行编码，因此，即使已经了解 ML.NET，它也可以提高工作效率。</span><span class="sxs-lookup"><span data-stu-id="c0e79-117">You can generate those assets from your own datasets without coding by yourself, so it also improves your productivity even if you already know ML.NET.</span></span>

<span data-ttu-id="c0e79-118">目前，ML.NET CLI 支持的 ML 任务包括：</span><span class="sxs-lookup"><span data-stu-id="c0e79-118">Currently, the ML Tasks supported by the ML.NET CLI are:</span></span>

- <span data-ttu-id="c0e79-119">分类</span><span class="sxs-lookup"><span data-stu-id="c0e79-119">classification</span></span>
- <span data-ttu-id="c0e79-120">回归</span><span class="sxs-lookup"><span data-stu-id="c0e79-120">regression</span></span>
- <span data-ttu-id="c0e79-121">建议</span><span class="sxs-lookup"><span data-stu-id="c0e79-121">recommendation</span></span>
- <span data-ttu-id="c0e79-122">图像分类</span><span class="sxs-lookup"><span data-stu-id="c0e79-122">image classification</span></span>

<span data-ttu-id="c0e79-123">使用情况示例（分类方案）：</span><span class="sxs-lookup"><span data-stu-id="c0e79-123">Example of usage (classification scenario):</span></span>

```console
mlnet classification --dataset "yelp_labelled.txt" --label-col 1 --has-header false --train-time 10
```

![从命令行执行 ML.NET 分类](media/automate-training-with-cli/mlnet-classification-powershell.gif)

<span data-ttu-id="c0e79-125">可以在 Windows PowerShell、macOS/Linux bash 或 Windows CMD 上以相同的方式运行它。  </span><span class="sxs-lookup"><span data-stu-id="c0e79-125">You can run it the same way on *Windows PowerShell*, *macOS/Linux bash*, or *Windows CMD*.</span></span> <span data-ttu-id="c0e79-126">但是，表格自动填写（参数建议）不适用于 *Windows CMD*。</span><span class="sxs-lookup"><span data-stu-id="c0e79-126">However, tabular auto-completion (parameter suggestions) won't work on *Windows CMD*.</span></span>

## <a name="output-assets-generated"></a><span data-ttu-id="c0e79-127">生成的输出资产</span><span class="sxs-lookup"><span data-stu-id="c0e79-127">Output assets generated</span></span>

<span data-ttu-id="c0e79-128">CLI 中的 ML 任务命令在输出文件夹中生成以下资产：</span><span class="sxs-lookup"><span data-stu-id="c0e79-128">The ML task commands in the CLI generate the following assets in the output folder:</span></span>

- <span data-ttu-id="c0e79-129">可供用于运行预测的序列化模型 .zip 文件（“最佳模型”）。</span><span class="sxs-lookup"><span data-stu-id="c0e79-129">A serialized model .zip ("best model") ready to use for running predictions.</span></span>
- <span data-ttu-id="c0e79-130">C# 解决方案以及：</span><span class="sxs-lookup"><span data-stu-id="c0e79-130">C# solution with:</span></span>
  - <span data-ttu-id="c0e79-131">用于运行生成的模型/对生成的模型评分的 C# 代码（使用该模型在最终用户应用中进行预测）。</span><span class="sxs-lookup"><span data-stu-id="c0e79-131">C# code to run/score that generated model (to make predictions in your end-user apps with that model).</span></span>
  - <span data-ttu-id="c0e79-132">包含用于生成该模型的训练代码的 C# 代码（用于学习目的或模型重新训练）。</span><span class="sxs-lookup"><span data-stu-id="c0e79-132">C# code with the training code used to generate that model (for learning purposes or model retraining).</span></span>
- <span data-ttu-id="c0e79-133">日志文件，包含有关评估的多种算法的所有迭代/扫描的信息（包括其详细配置/管道）。</span><span class="sxs-lookup"><span data-stu-id="c0e79-133">Log file with information of all iterations/sweeps across the multiple algorithms evaluated, including their detailed configuration/pipeline.</span></span>

<span data-ttu-id="c0e79-134">前两个资产可以直接用于最终用户应用（ASP.NET Core Web 应用、服务、桌面应用等），以使用生成的 ML 模型进行预测。</span><span class="sxs-lookup"><span data-stu-id="c0e79-134">The first two assets can directly be used in your end-user apps (ASP.NET Core web app, services, desktop app, etc.) to make predictions with that generated ML model.</span></span>

<span data-ttu-id="c0e79-135">第三个资产（训练代码）显示 CLI 用于训练生成的模型的 ML.NET API 代码，因此，可以在后台重新训练模型并调查和迭代 CLI 和 AutoML 选择的特定训练程序/算法及超参数。</span><span class="sxs-lookup"><span data-stu-id="c0e79-135">The third asset, the training code, shows you what ML.NET API code was used by the CLI to train the generated model, so you can retrain your model and investigate and iterate on which specific trainer/algorithm and hyperparameters were selected by the CLI and AutoML under the covers.</span></span>

## <a name="understanding-the-quality-of-the-model"></a><span data-ttu-id="c0e79-136">了解模型的质量</span><span class="sxs-lookup"><span data-stu-id="c0e79-136">Understanding the quality of the model</span></span>

<span data-ttu-id="c0e79-137">使用 CLI 工具生成“最佳模型”时，可看到适用于所针对的 ML 任务的质量指标（例如准确性和 R 平方）。</span><span class="sxs-lookup"><span data-stu-id="c0e79-137">When you generate a 'best model' with the CLI tool, you see quality metrics (such as accuracy and R-Squared) as appropriate for the ML task you're targeting.</span></span>

<span data-ttu-id="c0e79-138">这里总结了按 ML 任务分组的指标，以便于了解自动生成的“最佳模型”的质量。</span><span class="sxs-lookup"><span data-stu-id="c0e79-138">Here those metrics are summarized grouped by ML task so you can understand the quality of your auto-generated 'best model'.</span></span>

### <a name="metrics-for-classification-models"></a><span data-ttu-id="c0e79-139">分类模型的指标</span><span class="sxs-lookup"><span data-stu-id="c0e79-139">Metrics for Classification models</span></span>

<span data-ttu-id="c0e79-140">下图显示 CLI 找到的排名前五的模型的分类指标列表：</span><span class="sxs-lookup"><span data-stu-id="c0e79-140">The following image displays the classification metrics list for the top five models found by the CLI:</span></span>

![排名前五的模型的分类指标](media/automate-training-with-cli/cli-multiclass-classification-metrics.png)

 <span data-ttu-id="c0e79-142">准确性是分类问题的常用指标，但准确性并不总是用于选择最佳模型的最佳指标，如下文参考中所述。</span><span class="sxs-lookup"><span data-stu-id="c0e79-142">Accuracy is a popular metric for classification problems, however accuracy isn't always the best metric to select the best model from as explained in the following references.</span></span> <span data-ttu-id="c0e79-143">在某些情况下，需要使用其他指标评估模型的质量。</span><span class="sxs-lookup"><span data-stu-id="c0e79-143">There are cases where you need to evaluate the quality of your model with additional metrics.</span></span>

<span data-ttu-id="c0e79-144">若要探索和了解由 CLI 输出的指标，请参阅[分类评估指标](resources/metrics.md#evaluation-metrics-for-multi-class-classification)。</span><span class="sxs-lookup"><span data-stu-id="c0e79-144">To explore and understand the metrics that are output by the CLI, see [Evaluation metrics for classification](resources/metrics.md#evaluation-metrics-for-multi-class-classification).</span></span>

### <a name="metrics-for-regression-and-recommendation-models"></a><span data-ttu-id="c0e79-145">回归和建议模型指标</span><span class="sxs-lookup"><span data-stu-id="c0e79-145">Metrics for Regression and Recommendation models</span></span>

<span data-ttu-id="c0e79-146">如果观测值与模型预测值之间的差异很小且无偏差，则回归模型能够很好地拟合数据。</span><span class="sxs-lookup"><span data-stu-id="c0e79-146">A regression model fits the data well if the differences between the observed values and the model's predicted values are small and unbiased.</span></span> <span data-ttu-id="c0e79-147">可以使用特定指标评估回归。</span><span class="sxs-lookup"><span data-stu-id="c0e79-147">Regression can be evaluated with certain metrics.</span></span>

<span data-ttu-id="c0e79-148">你将看到一个类似的列表，其中包含 CLI 找到的排名前五的质量模型的指标，但在这种情况下，排名前五的模型与回归 ML 任务相关：</span><span class="sxs-lookup"><span data-stu-id="c0e79-148">You'll see a similar list of metrics for the top five quality models found by the CLI, except in this case, the top five are related to a regression ML task:</span></span>

![排名前五的模型的回归指标](media/automate-training-with-cli/cli-regression-metrics.png)

<span data-ttu-id="c0e79-150">若要探索和了解 CLI 输出的指标，请参阅[回归评估指标](resources/metrics.md#evaluation-metrics-for-regression-and-recommendation)。</span><span class="sxs-lookup"><span data-stu-id="c0e79-150">To explore and understand the metrics that are output by the CLI, see [Evaluation metrics for regression](resources/metrics.md#evaluation-metrics-for-regression-and-recommendation).</span></span>

## <a name="see-also"></a><span data-ttu-id="c0e79-151">请参阅</span><span class="sxs-lookup"><span data-stu-id="c0e79-151">See also</span></span>

- [<span data-ttu-id="c0e79-152">如何安装 ML.NET CLI 工具</span><span class="sxs-lookup"><span data-stu-id="c0e79-152">How to install the ML.NET CLI tool</span></span>](how-to-guides/install-ml-net-cli.md)
- [<span data-ttu-id="c0e79-153">教程：使用 ML.NET CLI 分析情绪</span><span class="sxs-lookup"><span data-stu-id="c0e79-153">Tutorial: Analyze sentiment using the ML.NET CLI</span></span>](tutorials/sentiment-analysis-cli.md)
- [<span data-ttu-id="c0e79-154">ML.NET CLI 命令参考</span><span class="sxs-lookup"><span data-stu-id="c0e79-154">ML.NET CLI command reference</span></span>](reference/ml-net-cli-reference.md)
- [<span data-ttu-id="c0e79-155">ML.NET CLI 中的遥测</span><span class="sxs-lookup"><span data-stu-id="c0e79-155">Telemetry in ML.NET CLI</span></span>](resources/ml-net-cli-telemetry.md)
