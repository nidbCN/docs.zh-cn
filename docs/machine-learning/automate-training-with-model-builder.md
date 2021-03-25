---
title: 什么是模型生成器，它的工作原理是怎样的？
description: 如何使用 ML.NET 模型生成器自动训练机器学习模型
ms.date: 06/01/2020
ms.custom: overview, mlnet-tooling
ms.openlocfilehash: d7566a03f83eb76999d995a39aaae408405db2e1
ms.sourcegitcommit: b27645cb378d4e8137a267e5467ff31409acf6c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231415"
---
# <a name="what-is-model-builder-and-how-does-it-work"></a>什么是模型生成器，它的工作原理是怎样的？

ML.NET 模型生成器是一个直观的图形化 Visual Studio 扩展，用于生成、训练和部署自定义机器学习模型。

模型生成器使用自动化的机器学习 (AutoML) 来探索不同的机器学习算法和设置，以帮助找到最合适的方案。

使用模型生成器不需要具备机器学习的专业知识。 只需要一些数据，和确定要解决的问题。 模型生成器会生成将模型添加到 .NET 应用程序的代码。

![模型生成器 Visual Studio 扩展用户界面动画](media/ml-dotnet-model-builder.gif)

> [!NOTE]
> 模型生成器当前为预览版。

## <a name="scenario"></a>方案

可以为模型生成器提供许多不同的方案，从而为应用程序生成一个机器学习模型。

方案就是要使用数据进行预测类型的描述。 例如：

- 根据历史销售数据预测未来的产品销量
- 根据客户评价将情绪分类为正面或负面
- 检测某项银行交易是否存在欺诈性
- 将客户反馈问题传递至公司中合适的团队

每个方案都映射到不同的机器学习任务，其中包括：

- 二元分类
- 多类分类
- 回归
- 群集
- 异常检测
- 排名
- 建议
- 预测

例如，将情绪归类为正面或负面的方案属于二元分类任务。

若要详细了解 ML.NET 支持的不同 ML 任务，请参阅 [ML.NET 中的机器学习任务](resources/tasks.md)。

### <a name="which-machine-learning-scenario-is-right-for-me"></a>哪个机器学习方案最适合我？

在模型生成器中，你需要选择一个方案。 方案类型取决于尝试进行的预测类型。

#### <a name="text-classification"></a>文本分类

分类用于将数据分类为类别。

![显示二元分类示例（包括欺诈检测、风险环节和应用程序屏蔽）的图示](media/binary-classification-examples.png)

![多类分类示例，包括文档和产品分类、支持票证路由以及客户问题优先级](media/multiclass-classification-examples.png)

#### <a name="value-prediction"></a>值预测

值预测属于回归任务，用于预测数值。

![显示回归示例（如价格预测、销售预测和预测性维护）的图示](media/regression-examples.png)

#### <a name="image-classification"></a>图像分类

图像分类用于标识不同类别的图像。 例如，不同类型的地形或动物或制造缺陷。

如果你有一组图像，并且想要将图像分为不同的类别，则可以使用图像分类方案。

#### <a name="object-detection"></a>对象检测

对象检测用于定位图像中的实体并对其进行分类。  例如，定位和识别图像中的汽车和人。

如果图像包含多个不同类型的对象，可使用对象检测。

#### <a name="recommendation"></a>建议

建议的方案根据特定用户的好恶与其他用户的相似程度，为他们预测建议项列表。

当你有一组用户和一组“产品”（如要购买的商品、电影、书籍或电视节目）以及一组用户对这些产品的“评级”时，你可以使用建议的方案。

## <a name="environment"></a>环境

可以在本地计算机上或在 Azure 上的云中训练机器学习模型，具体取决于相应方案。

在本地训练模型时，你将在计算机资源（CPU、内存和磁盘）的约束下工作。 在云中训练模型时，你可以扩展资源来满足你的方案的需求，尤其是对于大型数据集。

除对象检测外，所有方案都支持本地 CPU 训练。

图像分类支持本地 GPU 训练。

图像分类和对象检测支持 Azure 训练。

## <a name="data"></a>数据

选择方案后，模型生成器会要求你提供数据集。 这些数据用于训练、评估和选择最适合方案的的模型。

![显示模型生成器步骤的图示](media/model-builder-steps.png)

模型生成器支持 .tsv、.csv、.txt 格式以及 SQL 数据库格式的数据集。 如果你有一个 .txt 文件，则应使用 `,`、`;` 或 `/t` 分隔列，并且该文件必须具有标题行。

如果数据集由图像组成，则支持的文件类型为 `.jpg` 和 `.png`。

有关详细信息，请参阅[将训练数据加载到模型生成器](how-to-guides/load-data-model-builder.md)。

### <a name="choose-the-output-to-predict-label"></a>选择要预测的输出（标签）

数据集是一个表格，其中，行中含训练示例，列中含特性。 每一行都具有：

- 一个标签，即要预测的特性
- 特征（为预测标签而用作输入的特性）。

在房价预测方案中，特性可能是：

- 房屋的面积
- 卧室和卫生间的数量
- 邮政编码

标签是与其所属行（包含面积、卧室和卫生间值以及邮政编码信息）对应的历史房价。

![表显示房价数据的行和列，其中包含由大小、房间数、邮政编码和价格标签组成的特征](media/model-builder-data.png)

### <a name="example-datasets"></a>示例数据集

如果还没有自己的数据，请试用以下数据集之一：

|方案|示例|数据|Label|特征|
|-|-|-|-|-|
|分类|预测销售异常|[产品销售数据](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/getting-started/AnomalyDetection_Sales/SpikeDetection/Data/product-sales.csv)|产品销售额|月份|
||预测网站评论的情绪|[网站评论数据](https://raw.githubusercontent.com/dotnet/machinelearning/master/test/data/wikipedia-detox-250-line-data.tsv)|标签（负面情绪为 0，正面情绪为 1）|评论、年份|
||预测信用卡欺诈交易|[信用卡数据](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/getting-started/BinaryClassification_CreditCardFraudDetection/CCFraudDetection.Trainer/assets/input/creditcardfraud-dataset.zip)|类（存在欺诈性为 1，否则为 0）|金额，V1-V28（匿名处理后的特征）|
||预测 GitHub 存储库中的问题类型|[GitHub 问题数据](https://github.com/dotnet/machinelearning-samples/blob/master/samples/csharp/end-to-end-apps/MulticlassClassification-GitHubLabeler/GitHubLabeler/Data/corefx-issues-train.tsv)|区域|标题、描述|
|值预测|预测出租车费用价格|[出租车费数据](https://github.com/dotnet/machinelearning-samples/blob/master/datasets/taxi-fare-train.csv)|车费|行程时间、距离|
|图像分类|预测花卉的类别 |[花卉图像](http://download.tensorflow.org/example_images/flower_photos.tgz)|花卉类型：雏菊、蒲公英、玫瑰、向日葵、郁金香|图像数据本身|
|建议|预测他人喜欢的电影|[电影评分](http://files.grouplens.org/datasets/movielens/ml-latest-small.zip)|用户、电影|评级|

## <a name="train"></a>训练

选择方案、环境、数据和标签后，模型生成器会训练该模型。

### <a name="what-is-training"></a>什么是训练？

训练是一个自动的过程，模型生成器通过该过程教模型如何回答方案相关的问题。 训练后，模型可以对其没有见过的输入数据进行预测。 例如，在预测房价时，可以预测新上市的房屋销售价。

因为模型生成器使用自动机器学习 (AutoML)，所以在训练期间不需要任何人工输入或微调操作。

### <a name="how-long-should-i-train-for"></a>训练时长应为多长时间？

模型生成器使用 AutoML 浏览多个模型，以查找性能最佳的模型。

更长的训练周期允许 AutoML 通过更多设置来浏览更多模型。

下表汇总了在本地计算机上为一组示例数据集获取良好性能所花的平均时间。

|数据集大小|训练的平均时间|
|------------|---------------------|
|0 - 10 MB|10 秒|
|10 - 100 MB|10 分钟|
|100 - 500 MB|30 分钟|
|500 - 1 GB|60 分钟|
|1 GB 以上|3 小时以上|

这些数字仅用于指南。 训练的确切长度取决于：

- 用作模型输入的特征（列）数
- 列的类型
- ML 任务
- 用于训练的计算机的 CPU、磁盘和内存性能

通常建议使用的数据集超过 100 行，因为数据集少于 100 行可能不会生成任何结果，并且可能需要花费更长的时间进行训练。

## <a name="evaluate"></a>评估

评估是衡量模型品质的过程。 模型生成器使用经过训练的模型对新的测试数据进行预测，然后度量预测效果的过程。

模型生成器将训练数据拆分为训练集和测试集。 训练数据 (80%) 用于训练模型，测试数据 (20%) 用于评估模型。

### <a name="how-do-i-understand-my-model-performance"></a>如何了解模型性能？

方案映射到机器学习任务。 每个 ML 任务都有其自己的一组评估指标。

#### <a name="value-prediction"></a>值预测

值预测问题的默认指标为 RSquared，RSquared 值的范围介于 0 和 1 之间。 1 是可能的最大值，换句话说，RSquared 的值越接近 1，模型的性能就越好。

报告的其他指标（如绝对损失、平方损失和 RMS 损失）为附加指标，可以用来理解模型的性能，并将其与其他值预测模型进行比较。

#### <a name="classification-2-categories"></a>分类（2 个类）

分类问题的默认指标是“准确性”。 准确性定义的是模型对测试数据集做出的正确预测的比例。 越接近 100% 或 1.0 越好。

报告的其他指标，如 AUC（曲线下面积），用于度量真正率和假正率之间的比例，在高于 0.50 时，才是可以接受的模型。

F1 分数等其他指标可用于控制精准率与召回率之间的平衡。

#### <a name="classification-3-categories"></a>分类（3 个以上类）

多类分类问题的默认指标是微观准确性。 微观准确性越接近 100% 或 1.0 越好。

多类分类的另一个重要指标是宏观准确性，类似于微观准确性，越接近 1.0 越好。 考虑这两种准确性类型的一种好方法是：

- 微观准确性：传入票证分类给正确团队的频率如何？
- 宏观准确性：对于普通团队而言，传入票证符合其业务范围的频率如何？

### <a name="more-information-on-evaluation-metrics"></a>有关评估指标的详细信息

有关详细信息，请参阅[模型评估指标](resources/metrics.md)。

## <a name="improve"></a>改进

如果模型性能评分不符合预期，可以：

- 延长训练时间。 有了更多时间，自动机器学习引擎可以体验更多算法和设置。

- 添加更多数据。 有时，数据量不足以训练高质量的机器学习模型。对于包含少量示例的数据集，尤其如此。

- 均衡分配数据。 对于分类任务，请确保在各个类别间均匀分配训练集。 例如，若有四个类别和 100 个训练示例，前两类（标记 1 和标记 2）包含 90 个记录，而剩下两类（标记 3 和标记 4）只包含 10 个记录，这就存在数据不均衡的问题，可能会导致模型很难正确预测标记 3 或标记 4。

## <a name="code"></a>代码

完成评估阶段后，模型生成器会输出一份模型文件和代码，可以使用该代码将模型添加到应用程序。 ML.NET 模型保存为 zip 文件。 用于加载和使用模型的代码会以新项目的形式添加到解决方案中。 模型生成器还会添加一个示例控制台应用，可以运行该应用来查看工作状态下的模型。

此外，模型生成器还会输出生成模型的代码，以便你能了解生成模型所使用的步骤。 还可以通过模型训练代码使用新的数据重新训练模型。

## <a name="whats-next"></a>后续步骤

[安装](how-to-guides/install-model-builder.md)模型生成器 Visual Studio 扩展

请尝试[价格预测或任何回归方案](tutorials/predict-prices-with-model-builder.md)
