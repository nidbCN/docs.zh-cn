---
title: 教程：来自 TensorFlow 的 ML.NET 图像分类模型
description: 了解如何将现有 TensorFlow 模型中的知识传输到新的 ML.NET 图像分类模型中。 TensorFlow 模型经过训练，可以将图像分为一千个类别。 ML.NET 模型使用迁移学习将图像分为更多类别。
ms.date: 06/30/2020
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: b3e5617979d1635248f87db6008d3e234bb3ffc5
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104877028"
---
# <a name="tutorial-generate-an-mlnet-image-classification-model-from-a-pre-trained-tensorflow-model"></a><span data-ttu-id="d544e-105">教程：从预先训练的 TensorFlow 模型生成 ML.NET 图像分类模型</span><span class="sxs-lookup"><span data-stu-id="d544e-105">Tutorial: Generate an ML.NET image classification model from a pre-trained TensorFlow model</span></span>

<span data-ttu-id="d544e-106">了解如何将现有 TensorFlow 模型中的知识传输到新的 ML.NET 图像分类模型中。</span><span class="sxs-lookup"><span data-stu-id="d544e-106">Learn how to transfer the knowledge from an existing TensorFlow model into a new ML.NET image classification model.</span></span>

<span data-ttu-id="d544e-107">TensorFlow 模型经过训练，可以将图像分为一千个类别。</span><span class="sxs-lookup"><span data-stu-id="d544e-107">The TensorFlow model was trained to classify images into a thousand categories.</span></span> <span data-ttu-id="d544e-108">ML.NET 模型在其管道中使用 TensorFlow 模型的一部分来训练一个模型，以将图像分为 3 个类别。</span><span class="sxs-lookup"><span data-stu-id="d544e-108">The ML.NET model makes use of part of the TensorFlow model in its pipeline to train a model to classify images into 3 categories.</span></span>

<span data-ttu-id="d544e-109">从头开始定型[图像分类](https://en.wikipedia.org/wiki/Outline_of_object_recognition)模型需要设置数百万个参数、大量已标记的定型数据和海量计算资源（数百个 GPU 小时）。</span><span class="sxs-lookup"><span data-stu-id="d544e-109">Training an [Image Classification](https://en.wikipedia.org/wiki/Outline_of_object_recognition) model from scratch requires setting millions of parameters, a ton of labeled training data and a vast amount of compute resources (hundreds of GPU hours).</span></span> <span data-ttu-id="d544e-110">虽然不如从头开始定型自定义模型有效，但借助迁移学习，可以通过处理数千张图像（与数百万张已标记的图像相比）来简化此过程，并非常快速地生成自定义模型（在没有 GPU 的计算机上一小时内完成）。</span><span class="sxs-lookup"><span data-stu-id="d544e-110">While not as effective as training a custom model from scratch, transfer learning allows you to shortcut this process by working with thousands of images vs. millions of labeled images and build a customized model fairly quickly (within an hour on a machine without a GPU).</span></span> <span data-ttu-id="d544e-111">本教程进一步缩小了该过程，只使用十二个训练图像。</span><span class="sxs-lookup"><span data-stu-id="d544e-111">This tutorial scales that process down even further, using only a dozen training images.</span></span>

<span data-ttu-id="d544e-112">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="d544e-112">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="d544e-113">了解问题</span><span class="sxs-lookup"><span data-stu-id="d544e-113">Understand the problem</span></span>
> * <span data-ttu-id="d544e-114">将经过预先训练的 TensorFlow 模型合并到 ML.NET 管道中</span><span class="sxs-lookup"><span data-stu-id="d544e-114">Incorporate the pre-trained TensorFlow model into the ML.NET pipeline</span></span>
> * <span data-ttu-id="d544e-115">训练和评估 ML.NET 模型</span><span class="sxs-lookup"><span data-stu-id="d544e-115">Train and evaluate the ML.NET model</span></span>
> * <span data-ttu-id="d544e-116">对测试图像进行分类</span><span class="sxs-lookup"><span data-stu-id="d544e-116">Classify a test image</span></span>

<span data-ttu-id="d544e-117">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TransferLearningTF) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="d544e-117">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TransferLearningTF) repository.</span></span> <span data-ttu-id="d544e-118">请注意在本教程中，.NET 项目配置默认面向 .NET core 2.2。</span><span class="sxs-lookup"><span data-stu-id="d544e-118">Note that by default, the .NET project configuration for this tutorial targets .NET core 2.2.</span></span>

## <a name="what-is-transfer-learning"></a><span data-ttu-id="d544e-119">什么是迁移学习？</span><span class="sxs-lookup"><span data-stu-id="d544e-119">What is transfer learning?</span></span>

<span data-ttu-id="d544e-120">迁移学习是使用在解决一个问题时所获得的知识并将其应用于不同但相关的问题的过程。</span><span class="sxs-lookup"><span data-stu-id="d544e-120">Transfer learning is the process of using knowledge gained while solving one problem and applying it to a different but related problem.</span></span>

<span data-ttu-id="d544e-121">在本教程中，在可将图像分为 3 个类别的 ML.NET 模型中使用 TensorFlow 模型的一部分，该模型经过训练，可将图像分为一千个类别。</span><span class="sxs-lookup"><span data-stu-id="d544e-121">For this tutorial, you use part of a TensorFlow model - trained to classify images into a thousand categories - in an ML.NET model that classifies images into 3 categories.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d544e-122">先决条件</span><span class="sxs-lookup"><span data-stu-id="d544e-122">Prerequisites</span></span>

* <span data-ttu-id="d544e-123">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="d544e-123">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" workload installed.</span></span>
* [<span data-ttu-id="d544e-124">教程资产目录 .ZIP 文件</span><span class="sxs-lookup"><span data-stu-id="d544e-124">The tutorial assets directory .ZIP file</span></span>](https://github.com/dotnet/samples/blob/main/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)
* [<span data-ttu-id="d544e-125">InceptionV1 机器学习模型</span><span class="sxs-lookup"><span data-stu-id="d544e-125">The InceptionV1 machine learning model</span></span>](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)

## <a name="select-the-right-machine-learning-task"></a><span data-ttu-id="d544e-126">选择正确的机器学习任务</span><span class="sxs-lookup"><span data-stu-id="d544e-126">Select the right machine learning task</span></span>

### <a name="deep-learning"></a><span data-ttu-id="d544e-127">深度学习</span><span class="sxs-lookup"><span data-stu-id="d544e-127">Deep learning</span></span>

<span data-ttu-id="d544e-128">[深度学习](https://en.wikipedia.org/wiki/Deep_learning)是机器学习的一部分，颠覆了计算机视觉和语音识别等领域。</span><span class="sxs-lookup"><span data-stu-id="d544e-128">[Deep learning](https://en.wikipedia.org/wiki/Deep_learning) is a subset of Machine Learning, which is revolutionizing areas like Computer Vision and Speech Recognition.</span></span>

<span data-ttu-id="d544e-129">深度学习模型是使用包含多个学习层的大量[已标记的数据](https://en.wikipedia.org/wiki/Labeled_data)和[神经网络](https://en.wikipedia.org/wiki/Artificial_neural_network)来定型。</span><span class="sxs-lookup"><span data-stu-id="d544e-129">Deep learning models are trained by using large sets of [labeled data](https://en.wikipedia.org/wiki/Labeled_data) and [neural networks](https://en.wikipedia.org/wiki/Artificial_neural_network) that contain multiple learning layers.</span></span> <span data-ttu-id="d544e-130">深度学习：</span><span class="sxs-lookup"><span data-stu-id="d544e-130">Deep learning:</span></span>

* <span data-ttu-id="d544e-131">执行计算机视觉等一些任务的效果更好。</span><span class="sxs-lookup"><span data-stu-id="d544e-131">Performs better on some tasks like Computer Vision.</span></span>
* <span data-ttu-id="d544e-132">需要大量训练数据。</span><span class="sxs-lookup"><span data-stu-id="d544e-132">Requires huge amounts of training data.</span></span>

<span data-ttu-id="d544e-133">图像分类是常见的机器学习任务，可用于将图像自动分类为多个类别，例如：</span><span class="sxs-lookup"><span data-stu-id="d544e-133">Image Classification is a common Machine Learning task that allows us to automatically classify images into categories such as:</span></span>

* <span data-ttu-id="d544e-134">检测图像中是否有人脸。</span><span class="sxs-lookup"><span data-stu-id="d544e-134">Detecting a human face in an image or not.</span></span>
* <span data-ttu-id="d544e-135">检测是猫还是狗。</span><span class="sxs-lookup"><span data-stu-id="d544e-135">Detecting cats vs. dogs.</span></span>

 <span data-ttu-id="d544e-136">或者在以下图像中，确定图像是食物、玩具还是家用电器：</span><span class="sxs-lookup"><span data-stu-id="d544e-136">Or as in the following images, determining if an image is a(n)  food, toy, or appliance:</span></span>

<span data-ttu-id="d544e-137">![披萨图像](./media/image-classification/220px-Pepperoni_pizza.jpg)
![泰迪熊图像](./media/image-classification/119px-Nalle_-_a_small_brown_teddy_bear.jpg)
![烤面包机图像](./media/image-classification/193px-Broodrooster.jpg)</span><span class="sxs-lookup"><span data-stu-id="d544e-137">![pizza image](./media/image-classification/220px-Pepperoni_pizza.jpg)
![teddy bear image](./media/image-classification/119px-Nalle_-_a_small_brown_teddy_bear.jpg)
![toaster image](./media/image-classification/193px-Broodrooster.jpg)</span></span>

>[!Note]
> <span data-ttu-id="d544e-138">上面的图像属于维基共享资源，并按如下方式进行属性化：</span><span class="sxs-lookup"><span data-stu-id="d544e-138">The preceding images belong to Wikimedia Commons and are attributed as follows:</span></span>
>
> * <span data-ttu-id="d544e-139">“220px-Pepperoni_pizza.jpg”公有领域，[https://commons.wikimedia.org/w/index.php?curid=79505](<https://commons.wikimedia.org/w/index.php?curid=79505>)</span><span class="sxs-lookup"><span data-stu-id="d544e-139">"220px-Pepperoni_pizza.jpg" Public Domain, <https://commons.wikimedia.org/w/index.php?curid=79505>,</span></span>
> * <span data-ttu-id="d544e-140">“119px-Nalle_-_a_small_brown_teddy_bear.jpg”作者为 [Jonik](https://commons.wikimedia.org/wiki/User:Jonik) - 独自拍摄，CC BY-SA 2.0，[https://commons.wikimedia.org/w/index.php?curid=48166](<https://commons.wikimedia.org/w/index.php?curid=48166>)。</span><span class="sxs-lookup"><span data-stu-id="d544e-140">"119px-Nalle_-_a_small_brown_teddy_bear.jpg" By [Jonik](https://commons.wikimedia.org/wiki/User:Jonik) - Self-photographed, CC BY-SA 2.0, <https://commons.wikimedia.org/w/index.php?curid=48166>.</span></span>
> * <span data-ttu-id="d544e-141">“193px-Broodrooster.jpg”作者为 [M.Minderhoud](https://nl.wikipedia.org/wiki/Gebruiker:Michiel1972) - 自有作品，CC BY-SA 3.0，[https://commons.wikimedia.org/w/index.php?curid=27403](<https://commons.wikimedia.org/w/index.php?curid=27403>)</span><span class="sxs-lookup"><span data-stu-id="d544e-141">"193px-Broodrooster.jpg" By [M.Minderhoud](https://nl.wikipedia.org/wiki/Gebruiker:Michiel1972) - Own work, CC BY-SA 3.0, <https://commons.wikimedia.org/w/index.php?curid=27403></span></span>

<span data-ttu-id="d544e-142">`Inception model` 经过训练，可将图像分类为一千个类别，但在本教程中，你只需要在更小的类别集中分类图像。</span><span class="sxs-lookup"><span data-stu-id="d544e-142">The `Inception model` is trained to classify images into a thousand categories, but for this tutorial, you need to classify images in a smaller category set, and only those categories.</span></span> <span data-ttu-id="d544e-143">输入 `transfer learning` 的 `transfer` 部分。</span><span class="sxs-lookup"><span data-stu-id="d544e-143">Enter the `transfer` part of `transfer learning`.</span></span> <span data-ttu-id="d544e-144">可以将 `Inception model` 的识别和分类图像功能迁移到自定义图像分类器的新受限类别。</span><span class="sxs-lookup"><span data-stu-id="d544e-144">You can transfer the `Inception model`'s ability to recognize and classify images to the new limited categories of your custom image classifier.</span></span>

* <span data-ttu-id="d544e-145">食物</span><span class="sxs-lookup"><span data-stu-id="d544e-145">Food</span></span>
* <span data-ttu-id="d544e-146">玩具</span><span class="sxs-lookup"><span data-stu-id="d544e-146">Toy</span></span>
* <span data-ttu-id="d544e-147">家用电器</span><span class="sxs-lookup"><span data-stu-id="d544e-147">Appliance</span></span>

<span data-ttu-id="d544e-148">本教程使用 TensorFlow [Inception 模型](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)深入学习模型，该模型是在 `ImageNet` 数据集上经过训练的常用图像识别模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-148">This tutorial uses the TensorFlow [Inception model](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip) deep learning model, a popular image recognition model trained on the `ImageNet` dataset.</span></span> <span data-ttu-id="d544e-149">TensorFlow 模型将整个图像分为一千个类别，例如“伞”、“运动衫”和“洗碗机”。</span><span class="sxs-lookup"><span data-stu-id="d544e-149">The TensorFlow model classifies entire images into a thousand classes, such as “Umbrella”, “Jersey”, and “Dishwasher”.</span></span>

<span data-ttu-id="d544e-150">由于 `Inception model` 已预先在数千个不同图像上进行过训练，因此内部包含图像识别所需的[图像特征](https://en.wikipedia.org/wiki/Feature_(computer_vision))。</span><span class="sxs-lookup"><span data-stu-id="d544e-150">Because the `Inception model` has already been pre trained on thousands of different images, internally it contains the [image features](https://en.wikipedia.org/wiki/Feature_(computer_vision)) needed for image identification.</span></span> <span data-ttu-id="d544e-151">可以在模型中使用这些内部图像特征，用更少的类训练一个新模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-151">We can make use of these internal image features in the model to train a new model with far fewer classes.</span></span>

<span data-ttu-id="d544e-152">如下图中所示，在 .NET Core 或 .NET Framework 应用中添加对 ML.NET NuGet 包的引用。</span><span class="sxs-lookup"><span data-stu-id="d544e-152">As shown in the following diagram, you add a reference to the ML.NET NuGet packages in your .NET Core or .NET Framework applications.</span></span> <span data-ttu-id="d544e-153">实际上，ML.NET 包括并引用本机 `TensorFlow` 库，可用于编写代码来加载已训练的现有 `TensorFlow` 模型文件。</span><span class="sxs-lookup"><span data-stu-id="d544e-153">Under the covers, ML.NET includes and references the native `TensorFlow` library that allows you to write code that loads an existing trained `TensorFlow` model file.</span></span>

![TensorFlow 转换 ML.NET 体系结构图](./media/image-classification/tensorflow-mlnet.png)

### <a name="multiclass-classification"></a><span data-ttu-id="d544e-155">多类分类</span><span class="sxs-lookup"><span data-stu-id="d544e-155">Multiclass classification</span></span>

<span data-ttu-id="d544e-156">使用 TensorFlow Inception 模型提取适合作为经典机器学习算法输入的特征后，我们添加了一个 ML.NET [多类分类器](../resources/tasks.md#multiclass-classification)。</span><span class="sxs-lookup"><span data-stu-id="d544e-156">After using the TensorFlow inception model to extract features suitable as input for a classical machine learning algorithm, we add an ML.NET [multi-class classifier](../resources/tasks.md#multiclass-classification).</span></span>

<span data-ttu-id="d544e-157">此例中使用的特定训练者是[多项式逻辑回归算法](https://en.wikipedia.org/wiki/Multinomial_logistic_regression)。</span><span class="sxs-lookup"><span data-stu-id="d544e-157">The specific trainer used in this case is the [multinomial logistic regression algorithm](https://en.wikipedia.org/wiki/Multinomial_logistic_regression).</span></span>

<span data-ttu-id="d544e-158">此训练者实现的算法可以很好地处理大量特征存在的问题，这是深度学习模型对图像数据进行操作时遇到的情况。</span><span class="sxs-lookup"><span data-stu-id="d544e-158">The algorithm implemented by this trainer performs well on problems with a large number of features, which is the case for a deep learning model operating on image data.</span></span>

### <a name="data"></a><span data-ttu-id="d544e-159">数据</span><span class="sxs-lookup"><span data-stu-id="d544e-159">Data</span></span>

<span data-ttu-id="d544e-160">数据源有两个：`.tsv` 文件和图像文件。</span><span class="sxs-lookup"><span data-stu-id="d544e-160">There are two data sources: the `.tsv` file, and the image files.</span></span>  <span data-ttu-id="d544e-161">`tags.tsv` 文件包含两个列：第一列定义为 `ImagePath`，第二列是与图像对应的 `Label`。</span><span class="sxs-lookup"><span data-stu-id="d544e-161">The `tags.tsv` file contains two columns: the first one is defined as `ImagePath` and the second one is the `Label` corresponding to the image.</span></span> <span data-ttu-id="d544e-162">下面的示例文件没有标题行，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d544e-162">The following example file doesn't have a header row, and looks like this:</span></span>

<!-- markdownlint-disable MD010 -->
```tsv
broccoli.jpg    food
pizza.jpg   food
pizza2.jpg  food
teddy2.jpg  toy
teddy3.jpg  toy
teddy4.jpg  toy
toaster.jpg appliance
toaster2.png    appliance
```
<!-- markdownlint-enable MD010 -->

<span data-ttu-id="d544e-163">定型图像和测试图像位于下载 zip 文件的资产文件夹中。</span><span class="sxs-lookup"><span data-stu-id="d544e-163">The training and testing images are located in the assets folders that you'll download in a zip file.</span></span> <span data-ttu-id="d544e-164">这些图像属于维基共享资源。</span><span class="sxs-lookup"><span data-stu-id="d544e-164">These images belong to Wikimedia Commons.</span></span>
> <span data-ttu-id="d544e-165">[维基共享资源](https://commons.wikimedia.org/w/index.php?title=Main_Page&oldid=313158208)，免费媒体存储库。</span><span class="sxs-lookup"><span data-stu-id="d544e-165">*[Wikimedia Commons](https://commons.wikimedia.org/w/index.php?title=Main_Page&oldid=313158208), the free media repository.*</span></span> <span data-ttu-id="d544e-166">检索时间：2018 年 10 月 17 日 10:48 检索位置：<https://commons.wikimedia.org/wiki/Pizza>
> <https://commons.wikimedia.org/wiki/Toaster>
> <https://commons.wikimedia.org/wiki/Teddy_bear></span><span class="sxs-lookup"><span data-stu-id="d544e-166">Retrieved 10:48, October 17, 2018 from: <https://commons.wikimedia.org/wiki/Pizza>
> <https://commons.wikimedia.org/wiki/Toaster>
<https://commons.wikimedia.org/wiki/Teddy_bear></span></span>

## <a name="setup"></a><span data-ttu-id="d544e-167">安装</span><span class="sxs-lookup"><span data-stu-id="d544e-167">Setup</span></span>

### <a name="create-a-project"></a><span data-ttu-id="d544e-168">创建项目</span><span class="sxs-lookup"><span data-stu-id="d544e-168">Create a project</span></span>

1. <span data-ttu-id="d544e-169">创建名为“TransferLearningTF”的 **.NET Core 控制台应用程序**。</span><span class="sxs-lookup"><span data-stu-id="d544e-169">Create a **.NET Core Console Application** called "TransferLearningTF".</span></span>

1. <span data-ttu-id="d544e-170">安装“Microsoft.ML NuGet 包”  ：</span><span class="sxs-lookup"><span data-stu-id="d544e-170">Install the **Microsoft.ML NuGet Package**:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    * <span data-ttu-id="d544e-171">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。</span><span class="sxs-lookup"><span data-stu-id="d544e-171">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span>
    * <span data-ttu-id="d544e-172">选择“nuget.org”作为“包源”，选择“浏览”选项卡，再搜索“Microsoft.ML”。</span><span class="sxs-lookup"><span data-stu-id="d544e-172">Choose "nuget.org" as the Package source, select the Browse tab, search for **Microsoft.ML**.</span></span>
    * <span data-ttu-id="d544e-173">选择“安装”按钮。</span><span class="sxs-lookup"><span data-stu-id="d544e-173">Select the **Install** button.</span></span>
    * <span data-ttu-id="d544e-174">选择“预览更改”对话框中的“确定”按钮 。</span><span class="sxs-lookup"><span data-stu-id="d544e-174">Select the **OK** button on the **Preview Changes** dialog.</span></span>
    * <span data-ttu-id="d544e-175">如果同意所列包的许可条款，请选择“接受许可”对话框中的“我接受”按钮。</span><span class="sxs-lookup"><span data-stu-id="d544e-175">Select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>
    * <span data-ttu-id="d544e-176">对 Microsoft.ML.ImageAnalytics、SciSharp.TensorFlow.Redist 和 Microsoft.ML.TensorFlow 重复上述步骤  。</span><span class="sxs-lookup"><span data-stu-id="d544e-176">Repeat these steps for **Microsoft.ML.ImageAnalytics**, **SciSharp.TensorFlow.Redist** and **Microsoft.ML.TensorFlow**.</span></span>

### <a name="download-assets"></a><span data-ttu-id="d544e-177">下载资产</span><span class="sxs-lookup"><span data-stu-id="d544e-177">Download assets</span></span>

1. <span data-ttu-id="d544e-178">下载并解压缩[项目资产目录 zip 文件](https://github.com/dotnet/samples/blob/main/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)。</span><span class="sxs-lookup"><span data-stu-id="d544e-178">Download [The project assets directory zip file](https://github.com/dotnet/samples/blob/main/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip), and unzip.</span></span>

1. <span data-ttu-id="d544e-179">将“`assets`”目录复制到“TransferLearningTF”项目目录中。</span><span class="sxs-lookup"><span data-stu-id="d544e-179">Copy the `assets` directory into your *TransferLearningTF* project directory.</span></span> <span data-ttu-id="d544e-180">此目录及其子目录包含本教程所需的数据和支持文件（Inception 模型除外，将在下一步中下载并添加此模型）。</span><span class="sxs-lookup"><span data-stu-id="d544e-180">This directory and its subdirectories contain the data and support files (except for the Inception model, which you'll download and add in the next step) needed for this tutorial.</span></span>

1. <span data-ttu-id="d544e-181">下载并解压缩 [Inception 模型](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)。</span><span class="sxs-lookup"><span data-stu-id="d544e-181">Download the [Inception model](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip), and unzip.</span></span>

1. <span data-ttu-id="d544e-182">将刚刚解压缩的“`inception5h`”目录的内容复制到 TransferLearningTF 项目的“`assets/inception`”目录中。</span><span class="sxs-lookup"><span data-stu-id="d544e-182">Copy the contents of the `inception5h` directory just unzipped into your *TransferLearningTF* project `assets/inception` directory.</span></span> <span data-ttu-id="d544e-183">此目录包含本教程所需的模型和其他支持文件，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="d544e-183">This directory contains the model and additional support files needed for this tutorial, as shown in the following image:</span></span>

   ![Inception 目录内容](./media/image-classification/inception-files.png)

1. <span data-ttu-id="d544e-185">在“解决方案资源管理器”中，右键单击资产目录和子目录中的每个文件，再选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="d544e-185">In Solution Explorer, right-click each of the files in the asset directory and subdirectories and select **Properties**.</span></span> <span data-ttu-id="d544e-186">在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。</span><span class="sxs-lookup"><span data-stu-id="d544e-186">Under **Advanced**, change the value of **Copy to Output Directory** to **Copy if newer**.</span></span>

### <a name="create-classes-and-define-paths"></a><span data-ttu-id="d544e-187">创建类和定义路径</span><span class="sxs-lookup"><span data-stu-id="d544e-187">Create classes and define paths</span></span>

1. <span data-ttu-id="d544e-188">将以下附加的 `using` 语句添加到“Program.cs”文件顶部：</span><span class="sxs-lookup"><span data-stu-id="d544e-188">Add the following additional `using` statements to the top of the *Program.cs* file:</span></span>

    [!code-csharp[AddUsings](./snippets/image-classification/csharp/Program.cs#AddUsings)]

1. <span data-ttu-id="d544e-189">将以下代码添加到 `Main` 方法正上方的行中，以指定资产路径：</span><span class="sxs-lookup"><span data-stu-id="d544e-189">Add the following code to the line right above the `Main` method to specify the asset paths:</span></span>

    [!code-csharp[DeclareGlobalVariables](./snippets/image-classification/csharp/Program.cs#DeclareGlobalVariables)]

1. <span data-ttu-id="d544e-190">为输入数据和预测结果创建类。</span><span class="sxs-lookup"><span data-stu-id="d544e-190">Create classes for your input data, and predictions.</span></span>

    [!code-csharp[DeclareImageData](./snippets/image-classification/csharp/Program.cs#DeclareImageData)]

    <span data-ttu-id="d544e-191">`ImageData` 是输入图像数据类，包含以下 <xref:System.String> 字段：</span><span class="sxs-lookup"><span data-stu-id="d544e-191">`ImageData` is the input image data class and has the following <xref:System.String> fields:</span></span>

    * <span data-ttu-id="d544e-192">`ImagePath` 包含图像文件名。</span><span class="sxs-lookup"><span data-stu-id="d544e-192">`ImagePath` contains the image file name.</span></span>
    * <span data-ttu-id="d544e-193">`Label` 包含图像标签值。</span><span class="sxs-lookup"><span data-stu-id="d544e-193">`Label` contains a value for the image label.</span></span>

1. <span data-ttu-id="d544e-194">向项目添加 `ImagePrediction` 的新类：</span><span class="sxs-lookup"><span data-stu-id="d544e-194">Add a new class to your project for `ImagePrediction`:</span></span>

    [!code-csharp[DeclareImagePrediction](./snippets/image-classification/csharp/Program.cs#DeclareImagePrediction)]

    <span data-ttu-id="d544e-195">`ImagePrediction` 是图像预测类，包含以下字段：</span><span class="sxs-lookup"><span data-stu-id="d544e-195">`ImagePrediction` is the image prediction class and has the following fields:</span></span>

    * <span data-ttu-id="d544e-196">`Score` 包含给定图像分类的置信度百分比。</span><span class="sxs-lookup"><span data-stu-id="d544e-196">`Score` contains the confidence percentage for a given image classification.</span></span>
    * <span data-ttu-id="d544e-197">`PredictedLabelValue` 包含预测出的图像分类标签的值。</span><span class="sxs-lookup"><span data-stu-id="d544e-197">`PredictedLabelValue` contains a value for the predicted image classification label.</span></span>

    <span data-ttu-id="d544e-198">`ImagePrediction` 是在定型模型后用于预测的类。</span><span class="sxs-lookup"><span data-stu-id="d544e-198">`ImagePrediction` is the class used for prediction after the model has been trained.</span></span> <span data-ttu-id="d544e-199">它包含图像路径的 `string` (`ImagePath`)。</span><span class="sxs-lookup"><span data-stu-id="d544e-199">It has a `string` (`ImagePath`) for the image path.</span></span> <span data-ttu-id="d544e-200">`Label` 用于重用和训练模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-200">The `Label` is used to reuse and train the model.</span></span> <span data-ttu-id="d544e-201">`PredictedLabelValue` 在预测和评估过程中使用。</span><span class="sxs-lookup"><span data-stu-id="d544e-201">The `PredictedLabelValue` is used during prediction and evaluation.</span></span> <span data-ttu-id="d544e-202">对于计算，将使用带定型数据的输入、预测值和模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-202">For evaluation, an input with training data, the predicted values, and the model are used.</span></span>

### <a name="initialize-variables-in-main"></a><span data-ttu-id="d544e-203">在 Main 中初始化变量</span><span class="sxs-lookup"><span data-stu-id="d544e-203">Initialize variables in Main</span></span>

1. <span data-ttu-id="d544e-204">使用 `MLContext` 的新实例初始化 `mlContext` 变量。</span><span class="sxs-lookup"><span data-stu-id="d544e-204">Initialize the `mlContext` variable with a new instance of `MLContext`.</span></span>  <span data-ttu-id="d544e-205">用下面 `Main` 方法中的代码替换 `Console.WriteLine("Hello World!")` 行：</span><span class="sxs-lookup"><span data-stu-id="d544e-205">Replace the `Console.WriteLine("Hello World!")` line with the following code in the `Main` method:</span></span>

    [!code-csharp[CreateMLContext](./snippets/image-classification/csharp/Program.cs#CreateMLContext)]

    <span data-ttu-id="d544e-206">执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="d544e-206">The [MLContext class](xref:Microsoft.ML.MLContext) is a starting point for all ML.NET operations, and initializing `mlContext` creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="d544e-207">从概念上讲，它与实体框架中的 `DBContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="d544e-207">It's similar, conceptually, to `DBContext` in Entity Framework.</span></span>

### <a name="create-a-struct-for-inception-model-parameters"></a><span data-ttu-id="d544e-208">为 Inception 模型参数创建结构</span><span class="sxs-lookup"><span data-stu-id="d544e-208">Create a struct for Inception model parameters</span></span>

1. <span data-ttu-id="d544e-209">Inception 模型具有多个需要传入的参数。</span><span class="sxs-lookup"><span data-stu-id="d544e-209">The Inception model has several parameters you need to pass in.</span></span> <span data-ttu-id="d544e-210">紧跟在 `Main()` 方法后面，使用以下代码创建结构，以将参数值映射到易记名称：</span><span class="sxs-lookup"><span data-stu-id="d544e-210">Create a struct to map the parameter values to friendly names with the following code, just after the `Main()` method:</span></span>

    [!code-csharp[InceptionSettings](./snippets/image-classification/csharp/Program.cs#InceptionSettings)]

### <a name="create-a-display-utility-method"></a><span data-ttu-id="d544e-211">创建显示实用工具方法</span><span class="sxs-lookup"><span data-stu-id="d544e-211">Create a display utility method</span></span>

<span data-ttu-id="d544e-212">由于将多次显示图像数据和相关预测，请创建显示实用工具方法，用于处理图像和预测结果的显示。</span><span class="sxs-lookup"><span data-stu-id="d544e-212">Since you'll display the image data and the related predictions more than once, create a display utility method to handle displaying the image and prediction results.</span></span>

1. <span data-ttu-id="d544e-213">紧跟在 `InceptionSettings` 结构后面，使用下面的代码创建 `DisplayResults()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d544e-213">Create the `DisplayResults()` method, just after the `InceptionSettings` struct, using the following code:</span></span>

    ```csharp
    private static void DisplayResults(IEnumerable<ImagePrediction> imagePredictionData)
    {

    }
    ```

1. <span data-ttu-id="d544e-214">填充 `DisplayResults` 方法的主体：</span><span class="sxs-lookup"><span data-stu-id="d544e-214">Fill in the body of the `DisplayResults` method:</span></span>

    [!code-csharp[DisplayPredictions](./snippets/image-classification/csharp/Program.cs#DisplayPredictions)]

### <a name="create-a-tsv-file-utility-method"></a><span data-ttu-id="d544e-215">创建 .tsv 文件实用工具方法</span><span class="sxs-lookup"><span data-stu-id="d544e-215">Create a .tsv file utility method</span></span>

1. <span data-ttu-id="d544e-216">使用下面的代码紧随 `DisplayResults()` 方法后创建 `ReadFromTsv()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d544e-216">Create the `ReadFromTsv()` method, just after the `DisplayResults()` method, using the following code:</span></span>

    ```csharp
    public static IEnumerable<ImageData> ReadFromTsv(string file, string folder)
    {

    }
    ```

1. <span data-ttu-id="d544e-217">填充 `ReadFromTsv` 方法的主体：</span><span class="sxs-lookup"><span data-stu-id="d544e-217">Fill in the body of the `ReadFromTsv` method:</span></span>

    [!code-csharp[ReadFromTsv](./snippets/image-classification/csharp/Program.cs#ReadFromTsv)]

    <span data-ttu-id="d544e-218">代码分析整个 `tags.tsv` 文件，以将文件路径添加到 `ImagePath` 属性的图像文件名中，并将它和 `Label` 加载到 `ImageData` 对象中。</span><span class="sxs-lookup"><span data-stu-id="d544e-218">The code parses through the `tags.tsv` file to add the file path to the image file name for the `ImagePath` property and load it and the `Label` into an `ImageData` object.</span></span>

### <a name="create-a-method-to-make-a-prediction"></a><span data-ttu-id="d544e-219">创建用于进行预测的方法</span><span class="sxs-lookup"><span data-stu-id="d544e-219">Create a method to make a prediction</span></span>

1. <span data-ttu-id="d544e-220">使用下面的代码在 `DisplayResults()` 方法前创建 `ClassifySingleImage()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d544e-220">Create the `ClassifySingleImage()` method, just before the `DisplayResults()` method, using the following code:</span></span>

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. <span data-ttu-id="d544e-221">创建 `ImageData` 对象，其中包含单个 `ImagePath` 的完全限定路径和图像文件名。</span><span class="sxs-lookup"><span data-stu-id="d544e-221">Create an `ImageData` object that contains the fully qualified path and image file name for the single `ImagePath`.</span></span> <span data-ttu-id="d544e-222">将以下代码添加为 `ClassifySingleImage()` 方法的接下来几行：</span><span class="sxs-lookup"><span data-stu-id="d544e-222">Add the following code as the next  lines in the `ClassifySingleImage()` method:</span></span>

    [!code-csharp[LoadImageData](./snippets/image-classification/csharp/Program.cs#LoadImageData)]

1. <span data-ttu-id="d544e-223">通过添加以下代码作为 `ClassifySingleImage` 方法中的下一行，进行单一预测：</span><span class="sxs-lookup"><span data-stu-id="d544e-223">Make a single prediction, by adding the following code as the next line in the `ClassifySingleImage` method:</span></span>

    [!code-csharp[PredictSingle](./snippets/image-classification/csharp/Program.cs#PredictSingle)]

    <span data-ttu-id="d544e-224">若要获得预测，请使用 [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 方法。</span><span class="sxs-lookup"><span data-stu-id="d544e-224">To get the prediction, use the [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) method.</span></span> <span data-ttu-id="d544e-225">[PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="d544e-225">The [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to perform a prediction on a single instance of data.</span></span> <span data-ttu-id="d544e-226">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。</span><span class="sxs-lookup"><span data-stu-id="d544e-226">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is not thread-safe.</span></span> <span data-ttu-id="d544e-227">可以在单线程环境或原型环境中使用。</span><span class="sxs-lookup"><span data-stu-id="d544e-227">It's acceptable to use in single-threaded or prototype environments.</span></span> <span data-ttu-id="d544e-228">为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。</span><span class="sxs-lookup"><span data-stu-id="d544e-228">For improved performance and thread safety in production environments, use the `PredictionEnginePool` service, which creates an [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601) of [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) objects for use throughout your application.</span></span> <span data-ttu-id="d544e-229">请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。</span><span class="sxs-lookup"><span data-stu-id="d544e-229">See this guide on how to [use `PredictionEnginePool` in an ASP.NET Core Web API](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application).</span></span>

    > [!NOTE]
    > <span data-ttu-id="d544e-230">`PredictionEnginePool` 服务扩展目前处于预览状态。</span><span class="sxs-lookup"><span data-stu-id="d544e-230">`PredictionEnginePool` service extension is currently in preview.</span></span>

1. <span data-ttu-id="d544e-231">`ClassifySingleImage()` 方法的下一行代码用于显示预测结果：</span><span class="sxs-lookup"><span data-stu-id="d544e-231">Display the prediction result as the next line of code in the `ClassifySingleImage()` method:</span></span>

   [!code-csharp[DisplayPrediction](./snippets/image-classification/csharp/Program.cs#DisplayPrediction)]

## <a name="construct-the-mlnet-model-pipeline"></a><span data-ttu-id="d544e-232">构造 ML.NET 模型管道</span><span class="sxs-lookup"><span data-stu-id="d544e-232">Construct the ML.NET model pipeline</span></span>

<span data-ttu-id="d544e-233">ML.NET 模型管道是一个估算器链。</span><span class="sxs-lookup"><span data-stu-id="d544e-233">An ML.NET model pipeline is a chain of estimators.</span></span> <span data-ttu-id="d544e-234">请注意，管道构造过程中不会发生任何执行。</span><span class="sxs-lookup"><span data-stu-id="d544e-234">Note that no execution happens during pipeline construction.</span></span> <span data-ttu-id="d544e-235">估算器对象已创建，但不会执行。</span><span class="sxs-lookup"><span data-stu-id="d544e-235">The estimator objects are created but not executed.</span></span>

1. <span data-ttu-id="d544e-236">添加生成模型的方法</span><span class="sxs-lookup"><span data-stu-id="d544e-236">Add a method to generate the model</span></span>

    <span data-ttu-id="d544e-237">此方法是教程的核心。</span><span class="sxs-lookup"><span data-stu-id="d544e-237">This method is the heart of the tutorial.</span></span> <span data-ttu-id="d544e-238">它为模型创建管道，并训练管道以生成 ML.NET 模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-238">It creates a pipeline for the model, and trains the pipeline to produce the ML.NET model.</span></span> <span data-ttu-id="d544e-239">它还针对某些以前看不到的测试数据评估模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-239">It also evaluates the model against some previously unseen test data.</span></span>

    <span data-ttu-id="d544e-240">紧跟在 `InceptionSettings` 结构后面且恰好在 `DisplayResults()` 方法前面，使用以下代码创建 `GenerateModel()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d544e-240">Create the `GenerateModel()` method, just after the `InceptionSettings` struct and just before the `DisplayResults()` method, using the following code:</span></span>

    ```csharp
    public static ITransformer GenerateModel(MLContext mlContext)
    {

    }
    ```

1. <span data-ttu-id="d544e-241">添加估算器以从图像数据加载、调整和提取像素：</span><span class="sxs-lookup"><span data-stu-id="d544e-241">Add the estimators to load, resize and extract the pixels from the image data:</span></span>

    [!code-csharp[ImageTransforms](./snippets/image-classification/csharp/Program.cs#ImageTransforms)]

    <span data-ttu-id="d544e-242">图像数据需要处理成 TensorFlow 模型所需的格式。</span><span class="sxs-lookup"><span data-stu-id="d544e-242">The image data needs to be processed into the format that the TensorFlow model expects.</span></span> <span data-ttu-id="d544e-243">在本例中，图像将加载到内存中，将其调整为一致大小，并将像素提取成一个数值矢量。</span><span class="sxs-lookup"><span data-stu-id="d544e-243">In this case, the images are loaded into memory, resized to a consistent size, and the pixels are extracted into a numeric vector.</span></span>

1. <span data-ttu-id="d544e-244">添加估算器以加载 TensorFlow 模型并进行评分：</span><span class="sxs-lookup"><span data-stu-id="d544e-244">Add the estimator to load the TensorFlow model, and score it:</span></span>

    [!code-csharp[ScoreTensorFlowModel](./snippets/image-classification/csharp/Program.cs#ScoreTensorFlowModel)]

    <span data-ttu-id="d544e-245">管道中的此阶段将 TensorFlow 模型加载到内存中，然后通过 TensorFlow 模型网络处理像素值的矢量。</span><span class="sxs-lookup"><span data-stu-id="d544e-245">This stage in the pipeline loads the TensorFlow model into memory, then processes the vector of pixel values through the TensorFlow model network.</span></span> <span data-ttu-id="d544e-246">将输入应用于深度学习模型并使用该模型生成输出的过程称为 **评分**。</span><span class="sxs-lookup"><span data-stu-id="d544e-246">Applying inputs to a deep learning model, and generating an output using the model, is referred to as **Scoring**.</span></span> <span data-ttu-id="d544e-247">当作为一个整体使用模型时，评分将做出推理或预测。</span><span class="sxs-lookup"><span data-stu-id="d544e-247">When using the model in its entirety, scoring makes an inference, or prediction.</span></span>

    <span data-ttu-id="d544e-248">在本例中，将使用除最后一层（这是进行推理的层）之外的全部 TensorFlow 模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-248">In this case, you use all of the TensorFlow model except the last layer, which is the layer that makes the inference.</span></span> <span data-ttu-id="d544e-249">倒数第二层的输出标有 `softmax_2_preactivation`。</span><span class="sxs-lookup"><span data-stu-id="d544e-249">The output of the penultimate layer is labeled `softmax_2_preactivation`.</span></span> <span data-ttu-id="d544e-250">此层的输出实际上是特征矢量，用于描述原始输入图像的特征。</span><span class="sxs-lookup"><span data-stu-id="d544e-250">The output of this layer is effectively a vector of features that characterize the original input images.</span></span>

    <span data-ttu-id="d544e-251">TensorFlow 模型生成的此特征矢量将用作 ML.NET 训练算法的输入。</span><span class="sxs-lookup"><span data-stu-id="d544e-251">This feature vector generated by the TensorFlow model will be used as input to an ML.NET training algorithm.</span></span>

1. <span data-ttu-id="d544e-252">添加估算器以将训练数据中的字符串标签映射到整数键值：</span><span class="sxs-lookup"><span data-stu-id="d544e-252">Add the estimator to map the string labels in the training data to integer key values:</span></span>

    [!code-csharp[MapValueToKey](./snippets/image-classification/csharp/Program.cs#MapValueToKey)]

    <span data-ttu-id="d544e-253">接下来追加的 ML.NET 训练者要求其标签采用 `key` 格式，而不是任意字符串。</span><span class="sxs-lookup"><span data-stu-id="d544e-253">The ML.NET trainer that is appended next requires its labels to be in `key` format rather than arbitrary strings.</span></span> <span data-ttu-id="d544e-254">键是一个数字，一对一映射到字符串值。</span><span class="sxs-lookup"><span data-stu-id="d544e-254">A key is a number that has a one to one mapping to a string value.</span></span>

1. <span data-ttu-id="d544e-255">添加 ML.NET 训练算法：</span><span class="sxs-lookup"><span data-stu-id="d544e-255">Add the ML.NET training algorithm:</span></span>

    [!code-csharp[AddTrainer](./snippets/image-classification/csharp/Program.cs#AddTrainer)]

1. <span data-ttu-id="d544e-256">添加估算器以将预测的键值映射回字符串：</span><span class="sxs-lookup"><span data-stu-id="d544e-256">Add the estimator to map the predicted key value back into a string:</span></span>

    [!code-csharp[MapKeyToValue](./snippets/image-classification/csharp/Program.cs#MapKeyToValue)]

## <a name="train-the-model"></a><span data-ttu-id="d544e-257">定型模型</span><span class="sxs-lookup"><span data-stu-id="d544e-257">Train the model</span></span>

1. <span data-ttu-id="d544e-258">使用 [LoadFromTextFile](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile(Microsoft.ML.DataOperationsCatalog,System.String,Microsoft.ML.Data.TextLoader.Options)) 包装器加载训练数据。</span><span class="sxs-lookup"><span data-stu-id="d544e-258">Load the training data using the [LoadFromTextFile](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile(Microsoft.ML.DataOperationsCatalog,System.String,Microsoft.ML.Data.TextLoader.Options)) wrapper.</span></span> <span data-ttu-id="d544e-259">将以下代码作为下一行添加到 `GenerateModel()` 方法中：</span><span class="sxs-lookup"><span data-stu-id="d544e-259">Add the following code as the next line in the `GenerateModel()` method:</span></span>

    [!code-csharp[LoadData](./snippets/image-classification/csharp/Program.cs#LoadData "Load the data")]

    <span data-ttu-id="d544e-260">ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="d544e-260">Data in ML.NET is represented as an [IDataView class](xref:Microsoft.ML.IDataView).</span></span> <span data-ttu-id="d544e-261">`IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。</span><span class="sxs-lookup"><span data-stu-id="d544e-261">`IDataView` is a flexible, efficient way of describing tabular data (numeric and text).</span></span> <span data-ttu-id="d544e-262">可从文本文件或实时（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。</span><span class="sxs-lookup"><span data-stu-id="d544e-262">Data can be loaded from a text file or in real time (for example, SQL database or log files) to an `IDataView` object.</span></span>

1. <span data-ttu-id="d544e-263">用上面加载的数据训练模型：</span><span class="sxs-lookup"><span data-stu-id="d544e-263">Train the model with the data loaded above:</span></span>

    [!code-csharp[TrainModel](./snippets/image-classification/csharp/Program.cs#TrainModel)]

    <span data-ttu-id="d544e-264">`Fit()` 方法通过将训练数据集应用于管道来训练模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-264">The `Fit()` method trains your model by applying the training dataset to the pipeline.</span></span>

## <a name="evaluate-the-accuracy-of-the-model"></a><span data-ttu-id="d544e-265">评估模型的准确性</span><span class="sxs-lookup"><span data-stu-id="d544e-265">Evaluate the accuracy of the model</span></span>

1. <span data-ttu-id="d544e-266">通过将以下代码添加到 `GenerateModel` 方法的下一行，加载并转换测试数据：</span><span class="sxs-lookup"><span data-stu-id="d544e-266">Load and transform the test data, by adding the following code to the next line of the `GenerateModel` method:</span></span>

    [!code-csharp[LoadAndTransformTestData](./snippets/image-classification/csharp/Program.cs#LoadAndTransformTestData "Load and transform test data")]

    <span data-ttu-id="d544e-267">可以使用几个示例图像来评估模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-267">There are a few sample images that you can use to evaluate the model.</span></span> <span data-ttu-id="d544e-268">与训练数据类似，需要将这些数据加载到 `IDataView` 中，以便模型可以对其进行转换。</span><span class="sxs-lookup"><span data-stu-id="d544e-268">Like the training data, these need to be loaded into an `IDataView`, so that they can be transformed by the model.</span></span>

1. <span data-ttu-id="d544e-269">若要评估模型，请将以下代码添加到 `GenerateModel()` 方法：</span><span class="sxs-lookup"><span data-stu-id="d544e-269">Add the following code to the `GenerateModel()` method to evaluate the model:</span></span>

    [!code-csharp[Evaluate](./snippets/image-classification/csharp/Program.cs#Evaluate)]

    <span data-ttu-id="d544e-270">在你设置预测后，[Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) 方法便能：</span><span class="sxs-lookup"><span data-stu-id="d544e-270">Once you have the prediction set, the [Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) method:</span></span>

    * <span data-ttu-id="d544e-271">评估模型（将预测的值与测试数据集 `labels` 进行比较）。</span><span class="sxs-lookup"><span data-stu-id="d544e-271">Assesses the model (compares the predicted values with the test dataset `labels`).</span></span>
    * <span data-ttu-id="d544e-272">返回模型性能指标。</span><span class="sxs-lookup"><span data-stu-id="d544e-272">Returns the model performance metrics.</span></span>

1. <span data-ttu-id="d544e-273">显示模型准确性指标</span><span class="sxs-lookup"><span data-stu-id="d544e-273">Display the model accuracy metrics</span></span>

    <span data-ttu-id="d544e-274">使用下面的代码显示指标、共享结果，然后处理它们：</span><span class="sxs-lookup"><span data-stu-id="d544e-274">Use the following code to display the metrics, share the results, and then act on them:</span></span>

    [!code-csharp[DisplayMetrics](./snippets/image-classification/csharp/Program.cs#DisplayMetrics)]

    <span data-ttu-id="d544e-275">下面是图像分类评估指标：</span><span class="sxs-lookup"><span data-stu-id="d544e-275">The following metrics are evaluated for image classification:</span></span>

    * <span data-ttu-id="d544e-276">`Log-loss` - 请参阅[对数损失](../resources/glossary.md#log-loss)。</span><span class="sxs-lookup"><span data-stu-id="d544e-276">`Log-loss` - see [Log Loss](../resources/glossary.md#log-loss).</span></span> <span data-ttu-id="d544e-277">通常会希望对数损失尽可能接近 0。</span><span class="sxs-lookup"><span data-stu-id="d544e-277">You want Log-loss to be as close to zero as possible.</span></span>
    * <span data-ttu-id="d544e-278">`Per class Log-loss`.</span><span class="sxs-lookup"><span data-stu-id="d544e-278">`Per class Log-loss`.</span></span> <span data-ttu-id="d544e-279">建议每类别的对数损失尽可能接近 0。</span><span class="sxs-lookup"><span data-stu-id="d544e-279">You want per class Log-loss to be as close to zero as possible.</span></span>

1. <span data-ttu-id="d544e-280">添加以下代码，将经过训练的模型作为下一行代码返回：</span><span class="sxs-lookup"><span data-stu-id="d544e-280">Add the following code to return the trained model as the next line:</span></span>

    [!code-csharp[SaveModel](./snippets/image-classification/csharp/Program.cs#ReturnModel)]

## <a name="run-the-application"></a><span data-ttu-id="d544e-281">运行应用程序！</span><span class="sxs-lookup"><span data-stu-id="d544e-281">Run the application!</span></span>

1. <span data-ttu-id="d544e-282">创建 MLContext 类后，添加对 `Main` 方法中 `GenerateModel` 的调用：</span><span class="sxs-lookup"><span data-stu-id="d544e-282">Add the call to `GenerateModel` in the `Main` method after the creation of the MLContext class:</span></span>

    [!code-csharp[CallGenerateModel](./snippets/image-classification/csharp/Program.cs#CallGenerateModel)]

1. <span data-ttu-id="d544e-283">将对 `ClassifySingleImage()` 方法的调用添加为 `Main` 方法的下一行代码：</span><span class="sxs-lookup"><span data-stu-id="d544e-283">Add the call to the `ClassifySingleImage()` method as the next line of code in the `Main` method:</span></span>

    [!code-csharp[CallClassifySingleImage](./snippets/image-classification/csharp/Program.cs#CallClassifySingleImage)]

1. <span data-ttu-id="d544e-284">运行控制台应用 (Ctrl + F5)。</span><span class="sxs-lookup"><span data-stu-id="d544e-284">Run your console app (Ctrl + F5).</span></span> <span data-ttu-id="d544e-285">结果应如以下输出所示。</span><span class="sxs-lookup"><span data-stu-id="d544e-285">Your results should be similar to the following output.</span></span>  <span data-ttu-id="d544e-286">你可能会看到警告或处理消息，为清楚起见，这些消息已从以下结果中删除。</span><span class="sxs-lookup"><span data-stu-id="d544e-286">You may see warnings or processing messages, but these messages have been removed from the following results for clarity.</span></span>

    ```console
    =============== Training classification model ===============
    Image: broccoli2.jpg predicted as: food with score: 0.8955513
    Image: pizza3.jpg predicted as: food with score: 0.9667718
    Image: teddy6.jpg predicted as: toy with score: 0.9797683
    =============== Classification metrics ===============
    LogLoss is: 0.0653774699265059
    PerClassLogLoss is: 0.110315812569315 , 0.0204391272836966 , 0
    =============== Making single image classification ===============
    Image: toaster3.jpg predicted as: appliance with score: 0.9646884
    ```

<span data-ttu-id="d544e-287">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="d544e-287">Congratulations!</span></span> <span data-ttu-id="d544e-288">现在，通过在 ML.NET 中对 `TensorFlow` 模型应用迁移学习，已成功生成了用于图像分类的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="d544e-288">You've now successfully built a machine learning model for image classification by applying transfer learning to a `TensorFlow` model in ML.NET.</span></span>

<span data-ttu-id="d544e-289">可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TransferLearningTF) 存储库中找到本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="d544e-289">You can find the source code for this tutorial at the [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TransferLearningTF) repository.</span></span>

<span data-ttu-id="d544e-290">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="d544e-290">In this tutorial, you learned how to:</span></span>
> [!div class="checklist"]
>
> * <span data-ttu-id="d544e-291">了解问题</span><span class="sxs-lookup"><span data-stu-id="d544e-291">Understand the problem</span></span>
> * <span data-ttu-id="d544e-292">将经过预先训练的 TensorFlow 模型合并到 ML.NET 管道中</span><span class="sxs-lookup"><span data-stu-id="d544e-292">Incorporate the pre-trained TensorFlow model into the ML.NET pipeline</span></span>
> * <span data-ttu-id="d544e-293">训练和评估 ML.NET 模型</span><span class="sxs-lookup"><span data-stu-id="d544e-293">Train and evaluate the ML.NET model</span></span>
> * <span data-ttu-id="d544e-294">对测试图像进行分类</span><span class="sxs-lookup"><span data-stu-id="d544e-294">Classify a test image</span></span>

<span data-ttu-id="d544e-295">请查看机器学习示例 GitHub 存储库，以探索扩展的图像分类示例。</span><span class="sxs-lookup"><span data-stu-id="d544e-295">Check out the Machine Learning samples GitHub repository to explore an expanded image classification sample.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d544e-296">dotnet/machinelearning-samples GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="d544e-296">dotnet/machinelearning-samples GitHub repository</span></span>](https://github.com/dotnet/machinelearning-samples/)
