---
title: 教程：检测电话呼叫中的异常
description: 了解如何为时序数据构建异常检测应用程序。 本教程将使用 Visual Studio 2019 和 C# 创建 .NET Core 控制台应用程序。
ms.date: 12/04/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 52ce0a60c91ef7a82f7ef1a0709701302b9e11d9
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874766"
---
# <a name="tutorial-detect-anomalies-in-time-series-with-mlnet"></a>教程：使用 ML.NET 检测时序中的异常

了解如何为时序数据构建异常检测应用程序。 本教程将使用 Visual Studio 2019 和 C# 创建 .NET Core 控制台应用程序。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 加载数据
> * 检测时序的周期
> * 检测周期性时序的异常

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/PhoneCallsAnomalyDetection) 存储库中找到本教程的源代码。

## <a name="prerequisites"></a>先决条件

* 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019 16.7.8 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。

* [phone-calls.csv 数据集](https://github.com/dotnet/machinelearning-samples/blob/main/samples/csharp/getting-started/AnomalyDetection_PhoneCalls/SrEntireDetection/Data/phone-calls.csv)。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 创建一个名为“PhoneCallsAnomalyDetection”的 C# .NET Core 控制台应用程序。

2. 在项目中创建名为“Data”的目录，用于保存数据集文件。

3. 安装“Microsoft.ML NuGet 包”  ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。 选择“nuget.org”作为包源，然后选择“浏览”选项卡并搜索“Microsoft.ML”，再选择“安装”按钮 。 选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。 对“Microsoft.ML.TimeSeries”重复这些步骤。

4. 在 Program.cs 文件的顶部添加以下 `using` 语句：

    [!code-csharp[AddUsings](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#AddUsings "Add necessary usings")]

### <a name="download-your-data"></a>下载数据

1. 下载数据集并将其保存到之前创建的 *Data* 文件夹中：

    右键单击 [phone-calls.csv](https://raw.githubusercontent.com/dotnet/machinelearning-samples/main/samples/csharp/getting-started/AnomalyDetection_PhoneCalls/SrEntireDetection/Data/phone-calls.csv) 并选择“将链接(或目标)另存为…”

     确保将 \*.csv 文件保存到 *Data* 文件夹，或者在将其保存到其他位置后，将 \*.csv 文件移动到 *Data* 文件夹。

2. 在解决方案资源管理器中，右键单击 \*.csv 文件并选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”  。

下表是来自 \*.csv 文件的数据预览：

| timestamp  | 值 |
|--------|--------------|
| 2018/9/3  | 36.69670857  |
| 2018/9/4  | 35.74160571  |
| .....  | .....  |
| 2018/10/3  | 34.49893429  |
| ...    | ....   |

此文件表示时序。 文件中的每一行都是一个数据点。 每个数据点都有两个属性，分别是 `timestamp` 和 `value`，表示每天的电话呼叫次数。 电话呼叫次数转换为非敏感数据。

### <a name="create-classes-and-define-paths"></a>创建类和定义路径

接下来，定义输入和预测类数据结构。

向项目添加一个新类：

1. 在“解决方案资源管理器”中，右键单击该项目，然后选择“添加”>“新项” 。

2. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为 PhoneCallsData.cs  。 然后，选择“添加”  按钮。

   此时，将在代码编辑器中打开 PhoneCallsData.cs 文件。

3. 将以下 `using` 语句添加到 PhoneCallsData.cs 顶部：

   ```csharp
   using Microsoft.ML.Data;
   ```

4. 删除现有类定义并向 PhoneCallsData.cs 文件添加以下代码，其中包含两个类 `PhoneCallsData` 和 `PhoneCallsPrediction`：

    [!code-csharp[DeclareTypes](./snippets/phone-calls-anomaly-detection/csharp/PhoneCallsData.cs#DeclareTypes "Declare data record types")]

    `PhoneCallsData` 指定输入数据类。 [LoadColumn](xref:Microsoft.ML.Data.LoadColumnAttribute.%23ctor%28System.Int32%29) 属性指定应加载数据集中的哪些列（按列索引）。 它包含两个属性：`timestamp` 和 `value`，分别对应于数据文件中的相同属性。

    `PhoneCallsPrediction` 指定预测数据类。 对于 SR-CNN 检测器，预测取决于指定的[检测模式](xref:Microsoft.ML.TimeSeries.SrCnnDetectMode)。 在此示例中，我们选择 `AnomalyAndMargin` 模式。 输出包含七列。 在大多数情况下，`IsAnomaly`、`ExpectedValue`、`UpperBoundary` 和 `LowerBoundary` 能够提供足够的信息。 它们会告诉你某个点是否异常，该点的预期值以及该点的上下边界区域。

5. 将以下代码添加到 `Main` 方法正上方的行中，以指定数据文件的路径：

    [!code-csharp[Declare global variables](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DeclareGlobalVariables "Declare global variables")]

### <a name="initialize-variables-in-main"></a>在 Main 中初始化变量

1. 使用以下代码替换 `Main` 方法中的 `Console.WriteLine("Hello World!")` 行，以声明和初始化 `mlContext` 变量：

    [!code-csharp[CreateMLContext](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CreateMLContext "Create the ML Context")]

    执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

### <a name="load-the-data"></a>加载数据

ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。 `IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。 可从文本文件或其他源（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。

1. 添加以下代码作为 `Main` 方法的下一行：

    [!code-csharp[LoadData](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#LoadData "loading dataset")]

    [LoadFromTextFile()](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile%60%601%28Microsoft.ML.DataOperationsCatalog,System.String,System.Char,System.Boolean,System.Boolean,System.Boolean,System.Boolean%29) 用于定义数据架构并读取文件。 它使用数据路径变量并返回 `IDataView`。

## <a name="time-series-anomaly-detection"></a>时序异常情况检测

时序异常情况检测是检测时序数据离群值的过程；在给定的输入时序上指向“怪异”或不是预期行为的行为。 这些异常通常表示问题领域中的一些重要事件：用户帐户受到网络攻击、断电、服务器上的突发 RPS、内存泄漏等。

要找到时序上的异常，应首先确定时序的周期。 然后，时序可以分为几个部分，以 `Y = T + S + R` 为例，其中 `Y` 是最初序列，`T` 是趋势部分，`S` 是周期性部分，而 `R` 是该时序的剩余部分。 此步骤称为[分解](https://en.wikipedia.org/wiki/Decomposition_of_time_series)。 最后，对剩余部分进行检测以发现异常。 在 ML.NET 中，SR-CNN 算法是一种新颖的高级算法，它基于频谱残差 (SR) 和卷积神经网络 (CNN) 来检测时序上的异常。 有关此算法的详细信息，请参阅 [Microsoft 的时序异常检测服务](https://arxiv.org/pdf/1906.03821.pdf)。

在本教程中，你将了解可以使用两个函数来完成这些过程。

## <a name="detect-period"></a>检测周期

在第一步中，我们调用 `DetectSeasonality` 函数来确定时序的周期。

### <a name="create-the-detectperiod-method"></a>创建 DetectPeriod 方法

1. 使用以下代码在 `Main` 方法下创建 `DetectPeriod` 方法：

    ```csharp
    static void DetectPeriod(MLContext mlContext, IDataView phoneCalls)
    {

    }
    ```

2. 使用 <xref:Microsoft.ML.TimeSeriesCatalog.DetectSeasonality%2A> 函数检测周期。 使用以下代码将其添加到 `DetectPeriod` 方法中：

    [!code-csharp[DetectSeasonality](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DetectSeasonality)]

3. 将以下内容添加为 `DetectPeriod` 方法的下一行代码，以显示周期值：

    [!code-csharp[DisplayPeriod](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DisplayPeriod)]

4. 将以下调用添加到 `Main` 方法中的 `DetectPeriod` 方法：

    [!code-csharp[CallDetectPeriod](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CallDetectPeriod)]

### <a name="period-detection-results"></a>周期检测结果

运行应用程序。 结果应如下所示。

```console
Period of the series is: 7.
```

## <a name="detect-anomaly"></a>检测异常

在此步骤中，你将使用 <xref:Microsoft.ML.TimeSeriesCatalog.DetectEntireAnomalyBySrCnn%2A> 方法来查找异常。

### <a name="create-the-detectanomaly-method"></a>创建 DetectAnomaly 方法

1. 使用以下代码在 `DetectPeriod` 方法下创建 `DetectAnomaly` 方法：

    ```csharp
    static void DetectAnomaly(MLContext mlContext, IDataView phoneCalls, int period)
    {

    }
    ```

2. 使用以下代码在 `DetectAnomaly` 方法中设置 <xref:Microsoft.ML.TimeSeries.SrCnnEntireAnomalyDetectorOptions>：

    [!code-csharp[SetupSrCnnParameters](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#SetupSrCnnParameters)]

3. 在 `DetectAnomaly` 方法中添加以下代码行，以使用 SR-CNN 算法检测异常：

    [!code-csharp[DetectAnomaly](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DetectAnomaly)]

4. 使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法和以下代码将输出数据视图转换为强类型 `IEnumerable`，以方便显示：

    [!code-csharp[CreateEnumerableForResult](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CreateEnumerableForResult)]

5. 使用以下代码创建显示标头，用作 `DetectAnomaly` 方法中的下一行：

    [!code-csharp[DisplayHeader](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DisplayHeader)]

    将在更改点检测结果中显示以下信息：

    * `Index` 是每个点的索引。
    * `Anomaly` 是判断每个点是否被检测为异常的指标。
    * `ExpectedValue` 是每个点的估计值。
    * `LowerBoundary` 是非异常的每个点的最小值。
    * `UpperBoundary` 是非异常的每个点的最大值。

6. 使用以下代码循环访问 `predictions` `IEnumerable` 并显示结果：

    [!code-csharp[DisplayAnomalyDetectionResults](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#DisplayAnomalyDetectionResults)]

7. 将以下调用添加到 `Main` 方法中的 `DetectAnomaly` 方法：

    [!code-csharp[CallDetectAnomaly](./snippets/phone-calls-anomaly-detection/csharp/Program.cs#CallDetectAnomaly)]

## <a name="anomaly-detection-results"></a>异常情况检测结果

运行应用程序。 结果应如下所示。 处理期间将显示消息。 你可能会看到警告或处理消息。 为清楚起见，已从以下结果中删除某些消息。

```console
Detect period of the series
Period of the series is: 7.
Detect anomaly points in the series
Index   Data    Anomaly AnomalyScore    Mag     ExpectedValue   BoundaryUnit    UpperBoundary   LowerBoundary
0,0,36.841787256739266,41.14206982401966,32.541504689458876
1,0,35.67303618137362,39.97331874865401,31.372753614093227
2,0,34.710132999891826,39.029491313022824,30.390774686760828
3,0,33.44765248883495,37.786086547816545,29.10921842985335
4,0,28.937110922276364,33.25646923540736,24.61775260914537
5,0,5.143895892785781,9.444178460066171,0.843613325505391
6,0,5.163325228419392,9.463607795699783,0.8630426611390014
7,0,36.76414836240396,41.06443092968435,32.46386579512357
8,0,35.77908590657007,40.07936847385046,31.478803339289676
9,0,34.547259536635245,38.847542103915636,30.246976969354854
10,0,33.55193524820608,37.871293561337076,29.23257693507508
11,0,29.091800129624648,33.392082696905035,24.79151756234426
12,0,5.154836630338823,9.455119197619213,0.8545540630584334
13,0,5.234332502492464,9.534615069772855,0.934049935212073
14,0,36.54992549471526,40.85020806199565,32.24964292743487
15,0,35.79526470980883,40.095547277089224,31.494982142528443
16,0,34.34099013096804,38.64127269824843,30.040707563687647
17,0,33.61201516582131,37.9122977331017,29.31173259854092
18,0,29.223563320561812,33.5238458878422,24.923280753281425
19,0,5.170512168851533,9.470794736131923,0.8702296015711433
20,0,5.2614938889462834,9.561776456226674,0.9612113216658926
21,0,36.37103858487317,40.67132115215356,32.07075601759278
22,0,35.813544599026855,40.113827166307246,31.513262031746464
23,0,34.05600492733225,38.356287494612644,29.755722360051863
24,0,33.65828319077884,37.95856575805923,29.358000623498448
25,0,29.381125690882463,33.681408258162854,25.080843123602072
26,0,5.261543539820418,9.561826107100808,0.9612609725400283
27,0,5.4873712582971805,9.787653825577571,1.1870886910167897
28,1,36.504694001629254,40.804976568909645,32.20441143434886  <-- alert is on, detected anomaly
...
```

祝贺你！ 你现在已成功构建机器学习模型，用于检测周期性时序中的周期和异常。

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/PhoneCallsAnomalyDetection) 存储库中找到本教程的源代码。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 加载数据
> * 检测时序数据的周期
> * 检测时序数据中的异常

## <a name="next-steps"></a>后续步骤

请查看机器学习示例 GitHub 存储库，以探索能耗异常情况检测示例。
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub 存储库](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/AnomalyDetection_PowerMeterReadings)
