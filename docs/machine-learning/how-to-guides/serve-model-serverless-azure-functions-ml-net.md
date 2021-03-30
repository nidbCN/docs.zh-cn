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
# <a name="deploy-a-model-to-azure-functions"></a>将模型部署到 Azure Functions

了解如何通过 Azure Functions 无服务器环境部署预先训练的 ML.NET 机器学习模型，以便通过 HTTP 进行预测。

> [!NOTE]
> 此示例运行 `PredictionEnginePool` 服务的预览版本。

## <a name="prerequisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”和“Azure 部署”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。
- [Azure Functions 工具](/azure/azure-functions/functions-develop-vs#check-your-tools-version)
- PowerShell
- 预先定型的模型。 使用 [ML.NET 情绪分析教程](../tutorials/sentiment-analysis.md)生成自己的模型，或下载此[预先训练的情绪分析机器学习模型](https://github.com/dotnet/samples/blob/main/machine-learning/models/sentimentanalysis/sentiment_model.zip)

## <a name="azure-functions-sample-overview"></a>Azure Functions 示例概述

此示例是 C# HTTP 触发器 Azure Functions 应用程序  ，它使用预先定型的二进制分类模型将文本的情绪分类为消极或积极。 Azure Functions 提供了一种简便的方法，可以在云中托管的无服务器环境中大规模运行少量代码。 若要获取此示例的代码，请参阅 GitHub 上的 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction) 存储库。

## <a name="create-azure-functions-project"></a>创建 Azure Functions 项目

1. 打开 Visual Studio 2017。 从菜单栏中选择“文件”   > “新建”   > “项目”  。 在“新项目”  对话框中，依次选择“Visual C#”  节点和“云”  节点。 然后，选择“Azure Functions”  项目模板。 在“名称”  文本框中，键入“SentimentAnalysisFunctionsApp”，再选择“确定”  按钮。
1. 在“新项目”  对话框中，打开项目选项上方的下拉列表，并选择“Azure Functions v2 (.NET Core)”  。 然后，依次选择“Http 触发器”  项目和“确定”  按钮。
1. 在项目中创建“MLModels”  目录，用于保存模型：

    在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新文件夹”  。 键入“MLModels”，再按 Enter。

1. 安装 Microsoft.ML NuGet 包  版本 1.3.1  ：

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 将“nuget.org”选择为“包源”，选择“浏览”选项卡并搜索“Microsoft.ML”  ，在列表中选择该包，然后选择“安装”  按钮。 选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。

1. 安装 Microsoft.Azure.Functions.Extensions NuGet 包  ：

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.Azure.Functions.Extensions”，在列表中选择该包，再选择“安装”按钮   。 选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。

1. 安装 0.15.1 版本的 Microsoft.Extensions.ML NuGet 包   ：

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.Extensions.ML”，在列表中选择包，再选择“安装”按钮   。 选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。

1. 安装 1.0.31 版本的 Microsoft.NET.Sdk.Functions NuGet 包   ：

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“已安装”选项卡并搜索“Microsoft.NET.Sdk.Functions”，在列表中选择该包，再从“版本”下拉列表中选择“1.0.31”，并选择“更新”按钮   。  选择“预览更改”  对话框上的“确定”  按钮，如果你同意所列包的许可条款，则选择“接受许可”  对话框上的“我接受”  按钮。

## <a name="add-pre-trained-model-to-project"></a>将预先训练的模型添加到项目

1. 将预生成的模型复制到 MLModels  文件夹。
1. 在“解决方案资源管理器”中，右键单击预生成的模型文件，并选择“属性”  。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

## <a name="create-azure-function-to-analyze-sentiment"></a>创建 Azure 函数用于分析情绪

创建情绪预测类。 向项目添加一个新类：

1. 在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新项”  。

1. 在“添加新项”  对话框中，选择“Azure 函数”  ，并将“名称”  字段更改为“AnalyzeSentiment.cs”  。 然后，选择“添加”  按钮。

1. 在“新 Azure 函数”  对话框中，选择“Http 触发器”  。 然后，选择“确定”  按钮。

    此时，AnalyzeSentiment.cs  文件在代码编辑器中打开。 将以下 `using` 语句添加到 *AnalyzeSentiment.cs* 的顶部：

    [!code-csharp [AnalyzeUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L1-L11)]

    默认情况下，`AnalyzeSentiment` 类为 `static`。 确保从类定义中删除 `static` 关键字。

    ```csharp
    public class AnalyzeSentiment
    {

    }
    ```

## <a name="create-data-models"></a>创建数据模型

需要为输入数据和预测创建一些类。 向项目添加一个新类：

1. 在项目中创建“DataModels”  目录，用于保存数据模型：在“解决方案资源管理器”中，右键单击项目，并依次选择“添加”>“新文件夹”  。 键入“DataModels”，再按 Enter。
2. 在“解决方案资源管理器”中，右键单击“DataModels”  目录，再依次选择“添加”>“新项”  。
3. 在“添加新项”  对话框中，选择“类”  并将“名称”  字段更改为“SentimentData.cs”  。 然后，选择“添加”  按钮。

    “SentimentData.cs”  文件随即在代码编辑器中打开。 将以下 using 语句添加到 SentimentData.cs  顶部：

    [!code-csharp [SentimentDataUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentData.cs#L1)]

    删除现有类定义，并将以下代码添加到 SentimentData.cs  文件：

    [!code-csharp [SentimentData](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentData.cs#L5-L13)]

4. 在“解决方案资源管理器”中，右键单击“DataModels”  目录，再依次选择“添加”>“新项”  。
5. 在“添加新项”  对话框中，选择“类”  ，并将“名称”  字段更改为“SentimentPrediction.cs”  。 然后，选择“添加”  按钮。 此时，SentimentPrediction.cs  文件在代码编辑器中打开。 将以下 using 语句添加到 SentimentPrediction.cs  顶部：

    [!code-csharp [SentimentPredictionUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentPrediction.cs#L1)]

    删除现有类定义，并将以下代码添加到 SentimentPrediction.cs  文件：

    [!code-csharp [SentimentPrediction](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/DataModels/SentimentPrediction.cs#L5-L14)]

    `SentimentPrediction` 继承自 `SentimentData`，后者提供对 `SentimentText` 属性中原始数据以及模型生成的输出的访问。

## <a name="register-predictionenginepool-service"></a>注册 PredictionEnginePool 服务

若要进行单一预测，必须创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 此外，必须在应用程序中的每一处所需位置创建它的实例。 随着应用程序的增长，此过程可能会变得难以管理。 为了提高性能和线程安全，请结合使用依赖项注入和 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。

若要详细了解[依赖项注入](https://en.wikipedia.org/wiki/Dependency_injection)，请单击下面的链接。

1. 在“解决方案资源管理器”  中，右键单击项目，然后选择“添加”   > “新项”  。
1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“Startup.cs”     。 然后，选择“添加”  按钮。
1. 将以下 using 语句添加到 Startup.cs 的顶部： 

    [!code-csharp [StartupUsings](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L1-L6)]

1. 删除 using 语句下的现有代码，并添加以下代码：

    ```csharp
    [assembly: FunctionsStartup(typeof(Startup))]
    namespace SentimentAnalysisFunctionsApp
    {
        public class Startup : FunctionsStartup
        {

        }
    }
    ```

1. 在 `Startup` 类中定义变量，以存储运行应用的环境以及模型所在的文件路径

    [!code-csharp [DefineStartupVars](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L13-L14)]

1. 在它下面创建一个构造函数，以设置 `_environment` 和 `_modelPath` 变量的值。 应用程序在本地运行时，默认环境为“开发”。 

    [!code-csharp [StartupCtor](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L16-L29)]

1. 然后，在构造函数下面添加一个名为 `Configure` 的新方法，用于注册 `PredictionEnginePool` 服务。

    [!code-csharp [ConfigureServices](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/Startup.cs#L31-L35)]

概括地讲，此代码在应用程序请求时自动初始化对象和服务供以后使用，无需手动执行初始化。

机器学习模型不是静态的。 随着新的训练数据变得可用，模型将重新训练和重新部署。 将最新版本的模型引入应用程序的一种方法是重新部署整个应用程序。 但这会导致应用程序关闭。 `PredictionEnginePool` 服务提供了一种机制，用于在不使应用程序关闭的情况下重新加载已更新的模型。

将 `watchForChanges` 参数设置为 `true`，则 `PredictionEnginePool` 会启动 [`FileSystemWatcher`](xref:System.IO.FileSystemWatcher)，用于侦听文件系统更改通知并在文件发生更改时引发事件。 这会提示 `PredictionEnginePool` 自动重新加载模型。

模型由 `modelName` 参数标识，因此更改时可以重新加载每个应用程序的多个模型。

> [!TIP]
> 或者，如果使用远程存储的模型，则可以使用 `FromUri` 方法。 `FromUri` 会轮询远程位置以获取更改，而不是监视文件更改事件。 轮询间隔默认为 5 分钟。 你可以根据应用程序的要求，增加或减少轮询间隔。 在下面的代码示例中，`PredictionEnginePool` 每分钟轮询存储在指定 URI 中的模型。
>
>```csharp
>builder.Services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
>   .FromUri(
>       modelName: "SentimentAnalysisModel",
>       uri:"https://github.com/dotnet/samples/raw/main/machine-learning/models/sentimentanalysis/sentiment_model.zip",
>       period: TimeSpan.FromMinutes(1));
>```

## <a name="load-the-model-into-the-function"></a>将模型加载到函数中

在 *AnalyzeSentiment* 类中插入以下代码：

[!code-csharp [AnalyzeCtor](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L18-L24)]

此代码通过将 `PredictionEnginePool` 传递给函数的构造函数（通过依赖项注入获得）来分配它。

## <a name="use-the-model-to-make-predictions"></a>使用模型进行预测

将 AnalyzeSentiment  类中 Run  方法的现有实现替换为以下代码：

[!code-csharp [AnalyzeRunMethod](~/machinelearning-samples/samples/csharp/end-to-end-apps/ScalableMLModelOnAzureFunction/SentimentAnalysisFunctionsApp/AnalyzeSentiment.cs#L26-L45)]

执行 `Run` 方法时，来自 HTTP 请求的传入数据被反序列化并用作 `PredictionEnginePool` 的输入。 然后，调用 `Predict` 方法，使用在 `Startup` 类中注册的 `SentimentAnalysisModel` 进行预测，并在成功时将结果返回给用户。

## <a name="test-locally"></a>在本地测试

至此，已完成所有设置，是时候测试应用程序了：

1. 运行此应用程序
1. 打开 PowerShell，并在提示符中输入代码（其中，PORT 是应用程序运行所在的端口）。 端口通常是 7071。

    ```powershell
    Invoke-RestMethod "http://localhost:<PORT>/api/AnalyzeSentiment" -Method Post -Body (@{SentimentText="This is a very bad steak"} | ConvertTo-Json) -ContentType "application/json"
    ```

    如果成功，输出文本应如下所示：

    ```powershell
    Negative
    ```

祝贺你！ 你已成功使用 Azure Functions 通过 Internet 提供模型进行预测。

## <a name="next-steps"></a>后续步骤

- [部署到 Azure](/azure/azure-functions/functions-develop-vs#publish-to-azure)
