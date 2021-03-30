---
title: 教程：分析情绪 - 二元分类
description: 本教程演示如何创建 Razor Pages 应用程序，该应用程序对网站评论情绪进行分类并采取适当的措施。 二元情绪分类器使用 Visual Studio 中的模型生成器。
ms.date: 11/21/2019
author: luisquintanilla
ms.author: luquinta
ms.topic: tutorial
ms.custom: mvc,mlnet-tooling
ms.openlocfilehash: cb4f18ad9da2e57ee09598183a1226b20a1d7aec
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874610"
---
# <a name="tutorial-analyze-sentiment-of-website-comments-in-a-web-application-using-mlnet-model-builder"></a>教程：使用 ML.NET 模型生成器在 Web 应用程序中分析网站评论的情绪

了解如何在 Web 应用程序中实时分析评论中的情绪。

本教程演示如何创建 ASP.NET Core Razor Pages 应用程序，该应用程序实时对网站评论情绪进行分类并采取适当的措施。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 创建 ASP.NET Core Razor Pages 应用程序
> - 准备和了解数据
> - 选择方案
> - 加载数据
> - 定型模型
> - 评估模型
> - 使用预测模型

> [!NOTE]
> 模型生成器当前为预览版。

可以在 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples) 存储库中找到本教程的源代码。

## <a name="pre-requisites"></a>先决条件

请访问[模型生成器安装指南](../how-to-guides/install-model-builder.md)，查看先决条件和安装说明的列表。

## <a name="create-a-razor-pages-application"></a>创建 Razor Pages 应用程序

1. 创建 ASP.NET Core Razor Pages 应用程序  。

    1. 打开 Visual Studio 并从菜单栏中选择“文件”>“新建”>“项目”  。
    1. 在“新项目”对话框中，依次选择“Visual C#”  节点和“Web”  节点。
    1. 然后，选择“ASP.NET Core Web 应用程序”  项目模板。
    1. 在“名称”  文本框中，键入“SentimentRazor”。
    1. 请确保未选中“将解决方案和项目放置在同一目录中”(VS 2019) 或已选中“创建解决方案的目录”(VS 2017)     。
    1. 选择“确定”  按钮。
    1. 在显示不同类型 ASP.NET Core 项目的窗口中，选择“Web 应用程序”  ，然后选择“确定”  按钮。

## <a name="prepare-and-understand-the-data"></a>准备和了解数据

下载[维基百科 detox 数据集](https://raw.githubusercontent.com/dotnet/machinelearning/main/test/data/wikipedia-detox-250-line-data.tsv)。 打开网页时，右键单击页面，选择“另存为”  ，将文件保存到计算机上的任何位置。

wikipedia-detox-250-line-data.tsv  数据集中的每一行都代表一个用户在维基百科上留下的不同评价。 第一列表示文本的情绪（0 表示 non-toxic (无害)，1 表示 toxic (不良)），第二列表示用户留下的评论。 列由制表符分隔。 数据类似如下所示：

| 情绪 | 情绪文本 |
| :---: | :---: |
1 | ==RUDE== Dude, you are rude upload that carl picture back, or else.
1 | == OK! ==  IM GOING TO VANDALIZE WILD ONES WIKI THEN!!!
0 | 我希望这会有所帮助。

## <a name="choose-a-scenario"></a>选择方案

![Visual Studio 中的“模型生成器”向导](./media/sentiment-analysis-model-builder/model-builder-screen.png)

为了训练模型，需要从模型生成器提供的可用机器学习方案列表中进行选择。

1. 在“解决方案资源管理器”中，右键单击“SentimentRazor”项目，然后选择“添加” > “机器学习”     。
1. 对于本示例，方案为情绪分析。 在模型生成器工具的“方案”步骤中，选择“情绪分析”方案   。

## <a name="load-the-data"></a>加载数据

模型生成器接受来自两个源的数据：SQL Server 数据库或者 `csv` 或 `tsv` 格式的本地文件。

1. 在模型生成器工具的数据步骤中，选择数据源下拉列表中的“文件”  。
1. 选择“选择文件”文本框旁边的按钮，并使用文件资源管理器浏览到 wikipedia-detox-250-line-data.tsv 文件，然后选择该文件   。
1. 在“要预测的列(标签)”下拉列表中选择“情绪”   。
1. 保留“输入列(特性)”下拉列表的默认值。 
1. 选择“训练”  链接以转到模型生成器工具中的下一步。

## <a name="train-the-model"></a>定型模型

在本教程中，用于训练情绪分析模型的机器学习任务是二元分类。 在模型训练过程中，模型生成器使用不同的二元分类算法和设置训练各个模型，以便为数据集找到性能最佳的模型。

模型训练所需的时间与数据量成正比。 模型生成器会根据数据源的大小自动选择“训练时间(秒)”的默认值  。

1. 尽管模型生成器将“训练时间(秒)”  的值设置为 10 秒，但可以将其增加到 30 秒。 通过较长时间段的训练，模型生成器可以在最佳模型的搜索中浏览更多的算法和参数组合。
1. 选择“开始训练”  。

    在训练过程中，进度数据显示在训练步骤中的 `Progress` 部分。

    - “状态”显示训练进程的完成状态。
    - “最高准确性”显示截至目前由模型生成器找到的性能最佳的模型的准确性。 准确性越高，意味着模型对测试数据的预测越准确。
    - “最佳算法”显示截至目前由模型生成器找到的性能最佳的算法的名称。
    - “最新算法”显示模型生成器为了训练模型采用的最新算法名称。

1. 训练完成后，选择“评估”  链接以转到下一步。

## <a name="evaluate-the-model"></a>评估模型

训练步骤的成果将是一个模型，该模型具备最佳的性能。 在模型生成器工具的评估步骤中，输出部分将包含“最佳模型”项中性能最佳模型使用的算法，并包含“最佳模型准确度”中的指标   。 此外还显示一个摘要表格，包含性能最佳的前五种模型以及它们的指标信息。

如果你对自己的准确性指标不满意，则尝试提高模型准确性的简单方法是增加模型的训练时间或使用更多数据。 否则，选择“代码”  链接以转到模型生成器工具中的最后一步。

## <a name="add-the-code-to-make-predictions"></a>添加代码进行预测

训练期间会创建两个项目。

### <a name="reference-the-trained-model"></a>引用已训练的模型

1. 在模型生成器工具的“代码”  步骤中，选择“添加项目”  将自动生成的项目添加到解决方案。

    以下项目应出现在解决方案资源管理器  中：

    - *SentimentRazorML.ConsoleApp*：包含模型训练和预测代码的 .NET Core 控制台应用程序。
    - *SentimentRazorML.Model*：一个 .NET Standard 类库，包含定义输入和输出模型数据架构的数据模型，以及在训练期间性能最佳的模型的保存版本。

    对于本教程，只使用 SentimentRazorML.Model  项目，因为预测将在 SentimentRazor  Web 应用程序中（而不是控制台中）进行。 尽管 SentimentRazorML.ConsoleApp  不用于评分，但它可用于在以后使用新数据重新训练模型。 重新训练不属于本教程的讨论范围。

### <a name="configure-the-predictionengine-pool"></a>配置 PredictionEngine 池

若要进行单一预测，必须创建 <xref:Microsoft.ML.PredictionEngine%602>。 <xref:Microsoft.ML.PredictionEngine%602> 不是线程安全。 此外，必须在应用程序中的每一处所需位置创建它的实例。 随着应用程序的增长，此过程可能会变得难以管理。 为了提高性能和线程安全，请结合使用依赖项注入和 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 <xref:Microsoft.ML.PredictionEngine%602> 对象的 <xref:Microsoft.Extensions.ObjectPool.ObjectPool%601>。

1. 安装 Microsoft.Extensions.ML NuGet 包  ：

    1. 在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”   。
    1. 选择“nuget.org”作为“包源”。
    1. 选择“浏览”  选项卡，搜索“Microsoft.Extensions.ML”  。
    1. 在列表中选择包，然后选择“安装”  按钮。
    1. 选择“预览更改”  对话框中的“确定”  按钮
    1. 如果同意所列包的许可条款，请选择“接受许可”  对话框中的“我接受”  按钮。

1. 打开“SentimentRazor”  项目中的“Startup.cs”  文件。
1. 添加以下 using 语句以引用 Microsoft.Extensions.ML  NuGet 包和 SentimentRazorML.Model  项目：

    ```csharp
    using System.IO;
    using Microsoft.Extensions.ML;
    using SentimentRazorML.Model;
    ```

1. 创建全局变量以存储已训练模型文件的位置。

    ```csharp
    private readonly string _modelPath;
    ```

1. 模型文件与应用程序的程序集文件一起存储在生成目录中。 为了便于访问，在 `Configure` 方法后创建一个称为 `GetAbsolutePath` 的帮助程序方法

    ```csharp
    public static string GetAbsolutePath(string relativePath)
    {
        FileInfo _dataRoot = new FileInfo(typeof(Program).Assembly.Location);
        string assemblyFolderPath = _dataRoot.Directory.FullName;

        string fullPath = Path.Combine(assemblyFolderPath, relativePath);
        return fullPath;
    }
    ```

1. 使用 `Startup` 类构造函数中的 `GetAbsolutePath` 方法来设置 `_modelPath`。

    ```csharp
    _modelPath = GetAbsolutePath("MLModel.zip");
    ```

1. 在 `ConfigureServices` 方法中为应用程序配置 `PredictionEnginePool`：

    ```csharp
    services.AddPredictionEnginePool<ModelInput, ModelOutput>()
            .FromFile(_modelPath);
    ```

### <a name="create-sentiment-analysis-handler"></a>创建情绪分析处理程序

预测将在应用程序的主页内进行。 因此，需要添加一个采用用户输入并使用 `PredictionEnginePool` 来返回预测的方法。

1. 打开位于 *Pages* 目录中的 Index.cshtml.cs  文件，并添加以下 using 语句：

    ```csharp
    using Microsoft.Extensions.ML;
    using SentimentRazorML.Model;
    ```

    要使用 `Startup` 类中已配置的 `PredictionEnginePool`，必须将它插入到要在其中使用它的模型的构造函数中。

1. 添加变量以引用 `IndexModel` 类中的 `PredictionEnginePool`。

    ```csharp
    private readonly PredictionEnginePool<ModelInput, ModelOutput> _predictionEnginePool;
    ```

1. 在 `IndexModel` 类中创建一个构造函数，并将 `PredictionEnginePool` 服务注入到其中。

    ```csharp
    public IndexModel(PredictionEnginePool<ModelInput, ModelOutput> predictionEnginePool)
    {
        _predictionEnginePool = predictionEnginePool;
    }
    ```

1. 创建一个方法处理程序，该处理程序使用 `PredictionEnginePool` 来对从网页接收的用户输入进行预测。

    1. 在 `OnGet` 方法下面，创建一个名为 `OnGetAnalyzeSentiment` 的新方法

        ```csharp
        public IActionResult OnGetAnalyzeSentiment([FromQuery] string text)
        {

        }
        ```

    1. 在 `OnGetAnalyzeSentiment` 方法中，如果用户输入为空或为 null，则返回“中性”情绪  。

        ```csharp
        if (String.IsNullOrEmpty(text)) return Content("Neutral");
        ```

    1. 给定有效的输入，创建新的 `ModelInput` 实例。

        ```csharp
        var input = new ModelInput { SentimentText = text };
        ```

    1. 使用 `PredictionEnginePool` 预测情绪。

        ```csharp
        var prediction = _predictionEnginePool.Predict(input);
        ```

    1. 使用以下代码将预测的 `bool` 值转换为“toxic”或“not toxic”。

        ```csharp
        var sentiment = Convert.ToBoolean(prediction.Prediction) ? "Toxic" : "Not Toxic";
        ```

    1. 最后，将情绪返回到网页上。

        ```csharp
        return Content(sentiment);
        ```

### <a name="configure-the-web-page"></a>配置网页

`OnGetAnalyzeSentiment` 返回的结果将在 `Index` 网页上动态显示。

1. 在“Pages”  目录中打开 Index.cshtml  文件，并将其内容替换为以下代码：

    [!code-cshtml [IndexPage](~/machinelearning-samples/samples/modelbuilder/BinaryClassification_Sentiment_Razor/SentimentRazor/Pages/Index.cshtml)]

1. 接下来，将 css 样式代码添加到 wwwroot\css  目录中的 site.css  页面的末尾：

    [!code-css [CssStyling](~/machinelearning-samples/samples/modelbuilder/BinaryClassification_Sentiment_Razor/SentimentRazor/wwwroot/css/site.css#L61-L105)]

1. 然后，添加代码，将源自网页的输入发送到 `OnGetAnalyzeSentiment` 处理程序。

    1. 在 wwwroot\js  目录中的 site.js  文件中，创建名为 `getSentiment` 的函数，以便向 `OnGetAnalyzeSentiment` 处理程序的用户输入发出 GET HTTP 请求。

        [!code-javascript [GetSentimentMethod](~/machinelearning-samples/samples/modelbuilder/BinaryClassification_Sentiment_Razor/SentimentRazor/wwwroot/js/site.js#L5-L10)]

    1. 在它的下面，添加另一个名为 `updateMarker` 的函数，以便在预测情绪时，动态更新网页上标记的位置。

        [!code-javascript [UpdateMarkerMethod](~/machinelearning-samples/samples/modelbuilder/BinaryClassification_Sentiment_Razor/SentimentRazor/wwwroot/js/site.js#L12-L15)]

    1. 创建一个名为 `updateSentiment` 的事件处理程序函数以获取用户的输入，使用 `getSentiment` 函数将其发送到 `OnGetAnalyzeSentiment` 函数，并使用 `updateMarker` 函数更新标记。

        [!code-javascript [UpdateSentimentMethod](~/machinelearning-samples/samples/modelbuilder/BinaryClassification_Sentiment_Razor/SentimentRazor/wwwroot/js/site.js#L17-L34)]

    1. 最后，注册事件处理程序并将其绑定到具有 `id=Message` 特性的 `textarea` 元素。

        [!code-javascript [UpdateSentimentEvtHandler](~/machinelearning-samples/samples/modelbuilder/BinaryClassification_Sentiment_Razor/SentimentRazor/wwwroot/js/site.js#L36)]

## <a name="run-the-application"></a>运行此应用程序

设置应用程序之后，接下来即可运行应在浏览器中启动的应用程序。

当应用程序启动时，在文本区域中输入“Model Builder is cool!”  。 显示的预测情绪应为“Not Toxic”  。

![正在运行的窗口（包含预测情绪窗口）](./media/sentiment-analysis-model-builder/web-app.png)

如果稍后需要在另一个解决方案中引用模型生成器生成的项目，可以在 `C:\Users\%USERNAME%\AppData\Local\Temp\MLVSTools` 目录中找到它们。

## <a name="next-steps"></a>后续步骤

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 创建 ASP.NET Core Razor Pages 应用程序
> - 准备和了解数据
> - 选择方案
> - 加载数据
> - 定型模型
> - 评估模型
> - 使用预测模型

### <a name="additional-resources"></a>其他资源

若要详细了解本教程中所述的主题，请访问以下资源:

- [模型生成器应用场景](../automate-training-with-model-builder.md#scenario)
- [二元分类](../resources/glossary.md#binary-classification)
- [二元分类模型指标](../resources/metrics.md#evaluation-metrics-for-binary-classification)
