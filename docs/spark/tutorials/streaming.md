---
title: 使用 .NET for Apache Spark 进行结构化流式处理的教程
description: 在本教程中，了解如何使用 .NET for Apache Spark 进行 Spark 结构化流式处理。
author: mamccrea
ms.author: mamccrea
ms.date: 10/09/2020
ms.topic: tutorial
ms.openlocfilehash: 29f90c43ece1f2655a15f759b473f723e712bda8
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873167"
---
# <a name="tutorial-structured-streaming-with-net-for-apache-spark"></a>教程：使用 .NET for Apache Spark 进行结构化流式处理

本教程介绍如何使用 .NET for Apache Spark 调用 Spark 结构化流式处理。 Spark 结构化流式处理是 Apache Spark 对处理实时数据流的支持。 流处理是指在生成实时数据时对其进行分析。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
>
> * 创建和运行 .NET for Apache Spark 应用程序
> * 使用 netcat 创建数据流
> * 使用用户定义的函数和 SparkSQL 分析流式处理数据

## <a name="prerequisites"></a>先决条件

如果这是你的第一个 .NET for Apache Spark 应用程序，请开始使用[入门教程](get-started.md)以熟悉基础知识。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 在命令提示符处，运行以下命令来创建新的控制台应用程序：

   ```dotnetcli
   dotnet new console -o mySparkStreamingApp
   cd mySparkStreamingApp
   ```

   `dotnet` 命令将创建 `console` 类型的 `new` 应用程序。 `-o` 参数将创建名为 mySparkStreamingApp 的目录，其中会存储你的应用并填充必需文件。 `cd mySparkStreamingApp` 命令会将目录更改为你刚才创建的应用目录。

1. 要在应用中使用 .NET for Apache Spark，请安装 Microsoft.Spark 包。 在控制台中运行以下命令：

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="establish-and-connect-to-a-data-stream"></a>建立连接并连接到数据流

测试流处理的一个常用方法是通过 netcat。 netcat（也称为 nc）可用于从网络连接中读取以及向网络连接写入。 通过终端窗口建立与 netcat 的网络连接。

### <a name="create-a-data-stream-with-netcat"></a>使用 netcat 创建数据流

1. [下载 netcat](https://sourceforge.net/projects/nc110/files/)。 然后，从 zip 下载中提取文件并将提取的目录追加到“PATH”环境变量。

2. 若要启动新连接，请打开新的控制台并运行用于连接到端口 9999 上的 localhost 的以下命令。

   在 Windows 上：

   ```console
   nc -vvv -l -p 9999
   ```

   在 Linux 上：

   ```console
   nc -lk 9999
   ```

   Spark 程序侦听在此命令提示符下键入的输入。

### <a name="create-a-sparksession"></a>创建 SparkSession

1. 将以下附加的 `using` 语句添加到 mySparkStreamingApp 中 Program.cs 文件顶部：

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using Microsoft.Spark.Sql.Streaming;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. 将以下代码添加到 `Main` 方法，以创建新的 `SparkSession`。 Spark 会话是使用数据集和数据帧 API 对 Spark 进行编程的入口点。

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("Streaming example with a UDF")
        .GetOrCreate();
   ```

   通过调用上面创建的 spark 对象，可在整个程序中访问 Spark 和数据帧功能  。

### <a name="connect-to-a-stream-with-readstream"></a>使用 ReadStream() 连接到流

`ReadStream()` 方法返回一个 `DataStreamReader`，它可用于以 `DataFrame` 形式读取流式处理数据。 包括主机和端口信息，以告知 Spark 应用获取其流式处理数据的位置。

```csharp
DataFrame lines = spark
    .ReadStream()
    .Format("socket")
    .Option("host", hostname)
    .Option("port", port)
    .Load();
```

## <a name="register-a-user-defined-function"></a>注册用户定义的函数

可以使用 Spark 应用程序中的 UDF（用户定义的函数）来对数据执行计算和分析。

将以下代码添加到 `Main` 方法，以注册名为 `udfArray` 的 UDF。

```csharp
Func<Column, Column> udfArray =
    Udf<string, string[]>((str) => new string[] { str, $"{str} {str.Length}" });
```

此 UDF 处理从 netcat 终端接收的每个字符串，以生成包含原始字符串（包含在 str 中）的数组，后跟与原始字符串的长度串联的原始字符串。

例如，在 netcat 终端中输入“Hello world”会生成一个数组，其中：

* array\[0] = Hello world
* array\[1] = Hello world 11

## <a name="use-sparksql"></a>使用 SparkSQL

使用 SparkSQL 对存储在数据帧中的数据执行各种功能。 通常将 UDF 和 SparkSQL 结合起来以将 UDF 应用于数据帧的每一行。

```csharp
DataFrame arrayDF = lines.Select(Explode(udfArray(lines["value"])));
```

此代码片段将 udfArray 应用于数据帧中的每个值，这表示从 netcat 终端读取的每个字符串。 应用 SparkSQL 方法 <xref:Microsoft.Spark.Sql.Functions.Explode%2A>，将数组的每个项放在其自己的行中。 最后，使用 <xref:Microsoft.Spark.Sql.DataFrame.Select%2A> 放置在新的数据帧 arrayDF 中生成的列。

## <a name="display-your-stream"></a>显示流

使用 <xref:Microsoft.Spark.Sql.DataFrame.WriteStream> 来确定输出的特征，例如将结果打印到控制台并仅显示最新输出。

```csharp
StreamingQuery query = arrayDf
    .WriteStream()
    .Format("console")
    .Start();
```

## <a name="run-your-code"></a>运行代码

Spark 中的结构化流式处理通过一系列小型批处理来处理数据。  运行程序时，建立 netcat 连接的命令提示符允许你开始键入。 每次在该命令提示符下键入数据后按 Enter 键，Spark 都会将你的项视为批处理，并运行 UDF。

### <a name="use-spark-submit-to-run-your-app"></a>使用 spark-submit 运行应用

启动新的 netcat 会话后，打开新终端并运行 `spark-submit` 命令，该命令类似于以下命令：

```powershell
spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /path/to/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkCharacterCount localhost 9999
```

> [!NOTE]
> 请确保将包含实际路径的上述命令更新为 Microsoft Spark jar 文件。 上述命令还假定你的 netcat 服务器正在 localhost 端口 9999 上运行。

## <a name="get-the-code"></a>获取代码

本教程使用 [StructuredNetworkCharacterCount.cs](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkCharacterCount.cs) 示例，但 GitHub 上还有其他三个完整的流处理示例：

* [StructuredNetworkWordCount.cs](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)：从任意源流式传输的数据的字数统计
* [StructuredNetworkWordCountWindowed.cs](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCountWindowed.cs)：包含窗口化逻辑的数据的字数统计
* [StructuredKafkaWordCount.cs](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)：从 Kafka 流式传输的数据的字数统计

## <a name="next-steps"></a>后续步骤

请继续学习下一篇文章，了解如何将 .NET for Apache Spark 应用程序部署到 Databricks。
> [!div class="nextstepaction"]
> [教程：将 .NET for Apache Spark 应用程序部署到 Databricks](databricks-deployment.md)
