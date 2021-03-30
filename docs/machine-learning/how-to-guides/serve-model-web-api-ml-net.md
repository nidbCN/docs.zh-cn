---
title: 在 ASP.NET Core Web API 中部署模型
description: 使用 ASP.NET Core Web API 通过 Internet 提供 ML.NET 情绪分析机器学习模型
ms.date: 11/07/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc,how-to
ms.openlocfilehash: 1d05bc0ac6a874d539794d81fd742d973724dbef
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874129"
---
# <a name="deploy-a-model-in-an-aspnet-core-web-api"></a>在 ASP.NET Core Web API 中部署模型

了解如何使用 ASP.NET Core Web API 在 Web 上提供预先训练的 ML.NET 机器学习模型。 通过 Web API 提供模型可实现通过标准 HTTP 方法进行预测。

## <a name="prerequisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。
- PowerShell。
- 预先定型的模型。 使用 [ML.NET 情绪分析教程](../tutorials/sentiment-analysis.md)生成自己的模型，或下载此[预先训练的情绪分析机器学习模型](https://github.com/dotnet/samples/blob/main/machine-learning/models/sentimentanalysis/sentiment_model.zip)

## <a name="create-aspnet-core-web-api-project"></a>创建 ASP.NET Core Web API 项目

1. 打开 Visual Studio 2017。 从菜单栏中依次选择“文件”>“新建”>“项目”  。 在“新项目”对话框中，依次选择“Visual C#”  节点和“Web”  节点。 然后，选择“ASP.NET Core Web 应用程序”  项目模板。 在“名称”  文本框中，键入“SentimentAnalysisWebAPI”，再选择“确定”  按钮。

1. 在显示不同类型 ASP.NET Core 项目的窗口中，依次选择“API”  和“确定”  按钮。

1. 在项目中创建“MLModels”  目录，用于保存预生成的机器学习模型文件：

    在“解决方案资源管理器”中，右键单击项目，并依次选择“添加”>“新文件夹”。 键入“MLModels”，再按 Enter。

1. 安装“Microsoft.ML NuGet 包”  ：

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为“包源”，选择“浏览”选项卡并搜索“Microsoft.ML”  ，再依次选择列表中的相应包和“安装”按钮。 选择“预览更改”  对话框中的“确定”  按钮；如果同意所列包的许可条款，请选择“接受许可”对话框中的“我接受”  按钮。

1. 安装 **Microsoft.Extensions.ML Nuget 包**：

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.Extensions.ML”，在列表中选择包，再选择“安装”按钮  。 选择“预览更改”  对话框中的“确定”  按钮；如果同意所列包的许可条款，请选择“接受许可”对话框中的“我接受”  按钮。

### <a name="add-model-to-aspnet-core-web-api-project"></a>将模型添加到 ASP.NET Core Web API 项目

1. 将预生成的模型复制到 MLModels  目录
1. 在“解决方案资源管理器”中，右键单击模型 zip 文件，并选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”。

## <a name="create-data-models"></a>创建数据模型

需要为输入数据和预测创建一些类。 向项目添加一个新类：

1. 在项目中创建“DataModels”  目录，用于保存数据模型：

    在“解决方案资源管理器”中，右键单击项目，并依次选择“添加”>“新文件夹”。 键入“DataModels”，再按 Enter  。

2. 在“解决方案资源管理器”中，右键单击“DataModels”  目录，再依次选择“添加”>“新项”。
3. 在“添加新项”  对话框中，选择“类”  并将“名称”  字段更改为“SentimentData.cs”  。 然后，选择“添加”  按钮。 “SentimentData.cs”  文件随即在代码编辑器中打开。 将以下 using 语句添加到 SentimentData.cs  顶部：

    ```csharp
    using Microsoft.ML.Data;
    ```

    删除现有类定义，并将以下代码添加到 SentimentData.cs  文件：

    ```csharp
    public class SentimentData
    {
        [LoadColumn(0)]
        public string SentimentText;

        [LoadColumn(1)]
        [ColumnName("Label")]
        public bool Sentiment;
    }
    ```

4. 在“解决方案资源管理器”中，右键单击“DataModels”  目录，再依次选择“添加”>“新项”  。
5. 在“添加新项”  对话框中，选择“类”  ，并将“名称”  字段更改为“SentimentPrediction.cs”  。 然后，选择“添加”按钮。 此时，SentimentPrediction.cs  文件在代码编辑器中打开。 将以下 using 语句添加到 SentimentPrediction.cs  顶部：

    ```csharp
    using Microsoft.ML.Data;
    ```

    删除现有类定义，并将以下代码添加到 SentimentPrediction.cs  文件：

    ```csharp
    public class SentimentPrediction : SentimentData
    {

        [ColumnName("PredictedLabel")]
        public bool Prediction { get; set; }

        public float Probability { get; set; }

        public float Score { get; set; }
    }
    ```

    `SentimentPrediction` 继承自 `SentimentData`。 这样可以更轻松地查看 `SentimentText` 属性中的原始数据以及模型生成的输出。

## <a name="register-predictionenginepool-for-use-in-the-application"></a>注册 PredictionEnginePool 用于应用程序

若要进行单一预测，必须创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 此外，必须在应用程序中的每一处所需位置创建它的实例。 随着应用程序的增长，此过程可能会变得难以管理。 为了提高性能和线程安全，请使用依赖项注入和 `PredictionEnginePool` 服务的组合，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。

如果想要详细了解 [ASP.NET Core 中的依赖项注入](/aspnet/core/fundamentals/dependency-injection?view=aspnetcore-2.1)，以下链接提供详细信息。

1. 打开 Startup.cs  类，并将以下 using 语句添加到文件顶部：

    ```csharp
    using Microsoft.AspNetCore.Builder;
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.DependencyInjection;
    using Microsoft.Extensions.ML;
    using SentimentAnalysisWebAPI.DataModels;
    ```

2. 将以下代码添加到 *ConfigureServices* 方法中：

    ```csharp
    services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
        .FromFile(modelName: "SentimentAnalysisModel", filePath:"MLModels/sentiment_model.zip", watchForChanges: true);
    ```

概括地讲，此代码在应用程序请求时自动初始化对象和服务以供稍后使用，你无需手动执行初始化。

机器学习模型不是静态的。 随着新的训练数据变得可用，模型将重新训练和重新部署。 将最新版本的模型引入应用程序的一种方法是重新部署整个应用程序。 但这会导致应用程序关闭。 `PredictionEnginePool` 服务提供了一种机制，用于在不使应用程序关闭的情况下重新加载已更新的模型。

将 `watchForChanges` 参数设置为 `true`，则 `PredictionEnginePool` 会启动 [`FileSystemWatcher`](xref:System.IO.FileSystemWatcher)，用于侦听文件系统更改通知并在文件发生更改时引发事件。 这会提示 `PredictionEnginePool` 自动重新加载模型。

模型由 `modelName` 参数标识，因此更改时可以重新加载每个应用程序的多个模型。

> [!TIP]
> 或者，如果使用远程存储的模型，则可以使用 `FromUri` 方法。 `FromUri` 会轮询远程位置以获取更改，而不是监视文件更改事件。 轮询间隔默认为 5 分钟。 你可以根据应用程序的要求，增加或减少轮询间隔。 在下面的代码示例中，`PredictionEnginePool` 每分钟轮询存储在指定 URI 中的模型。
>
>```csharp
>services.AddPredictionEnginePool<SentimentData, SentimentPrediction>()
>   .FromUri(
>       modelName: "SentimentAnalysisModel",
>       uri:"https://github.com/dotnet/samples/raw/main/machine-learning/models/sentimentanalysis/sentiment_model.zip",
>       period: TimeSpan.FromMinutes(1));
>```

## <a name="create-predict-controller"></a>创建预测控制器

若要处理传入的 HTTP 请求，请创建控制器。

1. 在“解决方案资源管理器”中，右键单击“Controllers”  目录，再依次选择“添加”>“控制器”  。
1. 在“添加新项”  对话框中，依次选择“空 API 控制器”  和“添加”  。
1. 在提示窗口中，将“控制器名称”  字段更改为“PredictController.cs”  。 然后，选择“添加”按钮。 此时，PredictController.cs  文件在代码编辑器中打开。 将以下 using 语句添加到 PredictController.cs  顶部：

    ```csharp
    using System;
    using Microsoft.AspNetCore.Mvc;
    using Microsoft.Extensions.ML;
    using SentimentAnalysisWebAPI.DataModels;
    ```

    删除现有类定义，并将以下代码添加到 PredictController.cs  文件：

    ```csharp
    public class PredictController : ControllerBase
    {
        private readonly PredictionEnginePool<SentimentData, SentimentPrediction> _predictionEnginePool;

        public PredictController(PredictionEnginePool<SentimentData,SentimentPrediction> predictionEnginePool)
        {
            _predictionEnginePool = predictionEnginePool;
        }

        [HttpPost]
        public ActionResult<string> Post([FromBody] SentimentData input)
        {
            if(!ModelState.IsValid)
            {
                return BadRequest();
            }

            SentimentPrediction prediction = _predictionEnginePool.Predict(modelName: "SentimentAnalysisModel", example: input);

            string sentiment = Convert.ToBoolean(prediction.Prediction) ? "Positive" : "Negative";

            return Ok(sentiment);
        }
    }
    ```

此代码通过将 `PredictionEnginePool` 传递给控制器的构造函数（通过依赖项注入获得）来分配它。 然后，`Predict` 控制器的 `Post` 方法使用 `PredictionEnginePool`，来通过在 `Startup` 类中注册的 `SentimentAnalysisModel` 进行预测，并在成功时将结果返回给用户。

## <a name="test-web-api-locally"></a>在本地测试 Web API

完成所有设置后，是时候测试应用程序了。

1. 运行该应用程序。
1. 打开 PowerShell，并输入以下代码（其中，PORT 是应用程序正在侦听的端口）。

    ```powershell
    Invoke-RestMethod "https://localhost:<PORT>/api/predict" -Method Post -Body (@{SentimentText="This was a very bad steak"} | ConvertTo-Json) -ContentType "application/json"
    ```

    如果成功，输出文本应如下所示：

    ```powershell
    Negative
    ```

祝贺你！ 已成功提供模型用于使用 ASP.NET Core Web API 通过 Internet 进行预测。

## <a name="next-steps"></a>后续步骤

- [部署到 Azure](/aspnet/core/tutorials/publish-to-azure-webapp-using-vs#deploy-the-app-to-azure)
