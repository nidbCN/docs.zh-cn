---
title: 在 Windows 上生成 .NET for Apache Spark 应用程序
description: 了解如何在 Windows 上生成 .NET for Apache Spark 应用程序。
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: how-to
ms.openlocfilehash: e3ab62ea8bc493c7e652b66f0e9c7adce7decb56
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876950"
---
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-windows"></a>了解如何在 Windows 上生成 .NET for Apache Spark 应用程序

本文介绍如何在 Windows 上生成 .NET for Apache Spark 应用程序。

## <a name="prerequisites"></a>先决条件

如果具备以下所有先决条件，请跳到[生成](#build)步骤。

  1. 下载并安装 [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet/3.1)  - 安装 SDK 会将 `dotnet` 工具链添加到路径。 支持 .NET Core 2.1、2.2 和 3.1。
  2. 安装 [Visual Studio 2019](https://www.visualstudio.com/downloads/)  （版本 16.3 或更高版本）。 Community 版本完全免费。 配置安装时，请至少包含以下组件：
     * .NET 桌面开发
       * 所有必需的组件
         * .NET Framework 4.6.1 开发工具
     * .NET Core 跨平台开发
       * 所有必需的组件
  3. 安装 [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  。
     - 选择适用于操作系统的合适版本。 例如，为 Windows x64 计算机选择 jdk-8u201-windows-x64.exe  。
     - 使用安装程序安装，验证是否能够从命令行运行 `java`。
  4. 安装 [Apache Maven 3.6.0+](https://maven.apache.org/download.cgi)  。
     - 下载 [Apache Maven 3.6.0](http://mirror.metrocast.net/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip)。
     - 提取到本地目录。 例如 *C:\bin\apache-maven-3.6.0\*。
     - 将 Apache Maven 添加到 [PATH 环境变量](https://www.java.com/en/download/help/path.xml)。 例如 C:\bin\apache-maven-3.6.0\bin  。
     - 验证是否能够从命令行运行 `mvn`。
  5. 安装 [Apache Spark 2.3+](https://spark.apache.org/downloads.html)  。
     - 下载 [Apache Spark 2.3 +](https://spark.apache.org/downloads.html)，并使用 [7-zip](https://www.7-zip.org/) 将其提取到本地文件夹，例如 C:\bin\spark-3.0.1-bin-hadoop2.7 *\*。* （支持的 spark 版本为 2.3.、2.4.0、2.4.1、2.4.3、2.4.4、2.4.5、2.4.6、2.4.7、3.0.0 和 3.0.1）
     - 添加[新环境变量](https://www.java.com/en/download/help/path.xml) `SPARK_HOME`。 例如 *C:\bin\spark-3.0.1-bin-hadoop2.7\*。

       ```powershell
       set SPARK_HOME=C:\bin\spark-3.0.1-bin-hadoop2.7\
       ```

     - 将 Apache Spark 添加到 [PATH 环境变量](https://www.java.com/en/download/help/path.xml)。 例如 C:\bin\spark-3.0.1-bin-hadoop2.7\bin。

       ```powershell
       set PATH=%SPARK_HOME%\bin;%PATH%
       ```

     - 验证是否能够从命令行运行 `spark-shell`。
        控制台输出示例：

        ```output
        Welcome to
              ____              __
             / __/__  ___ _____/ /__
            _\ \/ _ \/ _ `/ __/  '_/
           /___/ .__/\_,_/_/ /_/\_\   version 3.0.1
              /_/

        Using Scala version 2.12.10 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_201)
        Type in expressions to have them evaluated.
        Type :help for more information.

        scala> sc
        res0: org.apache.spark.SparkContext = org.apache.spark.SparkContext@6eaa6b0c
        ```

        </details>

  6. 安装 [WinUtils](https://github.com/steveloughran/winutils)  。
     - 从 [WinUtils 存储库](https://github.com/steveloughran/winutils)下载 `winutils.exe` 二进制文件。 应选择编译 Spark 发行版时使用的 Hadoop 版本。 例如，将 hadoop-2.7.1 用于 Spark 3.0.1。
     - 将 `winutils.exe` 二进制文件保存到所选目录。 例如 C:\hadoop\bin  。
     - 设置 `HADOOP_HOME` 以反映包含 winutils.exe（不带 bin）的目录。 例如，使用命令行：

       ```powershell
       set HADOOP_HOME=C:\hadoop
       ```

     - 设置 PATH 环境变量以包含 `%HADOOP_HOME%\bin`。 例如，使用命令行：

       ```powershell
       set PATH=%HADOOP_HOME%\bin;%PATH%
       ```

确保可以从命令行运行 `dotnet`、`java`、`mvn`、`spark-shell`，然后再进行下一步。 认为还有更好的办法？ 请[发布问题](https://github.com/dotnet/spark/issues)并随意参与讨论。

> [!NOTE]
> 如果更新了任何环境变量，可能需要命令行的新实例。

## <a name="build"></a>生成

对于本指南的其余部分，需要将 .NET for Apache Spark 存储库克隆到计算机。 可以为克隆存储库选择任何位置。 例如 *C:\github\dotnet-spark\*。

```bash
git clone https://github.com/dotnet/spark.git C:\github\dotnet-spark
```

### <a name="build-net-for-apache-spark-scala-extensions-layer"></a>生成 .NET for Apache Spark Scala 扩展层

提交 .NET 应用程序时，.NET for Apache Spark 具有在 Scala 中编写的必要逻辑，以指示 Apache Spark 如何处理请求（例如，请求创建新的 Spark 会话，请求将数据从 .NET 端传输到 JVM 端等）。 此逻辑可在 [.NET for Spark Scala 源代码](https://github.com/dotnet/spark/tree/main/src/scala)中找到。

无论使用的是 .NET Framework 还是 .NET Core，都需要生成 .NET for Apache Spark Scala 扩展层：

```powershell
cd src\scala
mvn clean package
```

应会看到为支持的 Spark 版本创建的 JAR：

* `microsoft-spark-2-3\target\microsoft-spark-2-3_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-2-4\target\microsoft-spark-2-4_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-3-0\target\microsoft-spark-3-0_2.12-<spark-dotnet-version>.jar`

### <a name="build-the-net-for-spark-sample-applications"></a>生成适用于 .NET for Spark 示例应用程序

本部分介绍如何为 .NET for Apache Spark 生成[示例应用程序](https://github.com/dotnet/spark/tree/main/examples)。 这些步骤有助于了解任何 .NET for Spark 应用程序的整个生成过程。

#### <a name="using-visual-studio-for-net-framework"></a>使用 Visual Studio for .NET Framework

  1. 在 Visual Studio 中打开 `src\csharp\Microsoft.Spark.sln`，并在 `examples` 文件夹下生成 `Microsoft.Spark.CSharp.Examples` 项目（这也将生成 .NET 绑定项目）。 如果需要，可以在 `Microsoft.Spark.Examples` 项目中编写自己的代码（本示例中的“input_file json”是包含创建数据帧所需数据的 json 文件）：
  
      ```csharp
        // Instantiate a session
        var spark = SparkSession
            .Builder()
            .AppName("Hello Spark!")
            .GetOrCreate();

        // Create initial DataFrame
        DataFrame df = spark.Read().Json(input_file.json);

        // Print schema
        df.PrintSchema();

        // Apply a filter and show results
        df.Filter(df["age"] > 21).Show();
      ```

     成功生成后，会在输出目录中看到相应的二进制文件。
     控制台输出示例：

      ```powershell
            Directory: C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461


        Mode                LastWriteTime         Length Name
        ----                -------------         ------ ----
        -a----         3/6/2019  12:18 AM         125440 Apache.Arrow.dll
        -a----        3/16/2019  12:00 AM          13824 Microsoft.Spark.CSharp.Examples.exe
        -a----        3/16/2019  12:00 AM          19423 Microsoft.Spark.CSharp.Examples.exe.config
        -a----        3/16/2019  12:00 AM           2720 Microsoft.Spark.CSharp.Examples.pdb
        -a----        3/16/2019  12:00 AM         143360 Microsoft.Spark.dll
        -a----        3/16/2019  12:00 AM          63388 Microsoft.Spark.pdb
        -a----        3/16/2019  12:00 AM          34304 Microsoft.Spark.Worker.exe
        -a----        3/16/2019  12:00 AM          19423 Microsoft.Spark.Worker.exe.config
        -a----        3/16/2019  12:00 AM          11900 Microsoft.Spark.Worker.pdb
        -a----        3/16/2019  12:00 AM          23552 Microsoft.Spark.Worker.xml
        -a----        3/16/2019  12:00 AM         332363 Microsoft.Spark.xml
        ------------------------------------------- More framework files -------------------------------------
      ```

#### <a name="using-net-core-cli-for-net-core"></a>使用适用于 .NET Core 的 .NET Core CLI

> [!NOTE]
> 当前我们正致力于自动生成 Spark .NET 的 .NET Core。 在此之前，我们非常感谢你能耐心地手动执行一些步骤。

  1. 生成辅助角色：

      ```powershell
      cd C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\
      dotnet publish -f netcoreapp3.1 -r win-x64
      ```

      控制台输出示例：

      ```powershell
      PS C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker> dotnet publish -f netcoreapp3.1 -r win-x64
      Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
      Copyright (C) Microsoft Corporation. All rights reserved.

        Restore completed in 299.95 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark\Microsoft.Spark.csproj.
        Restore completed in 306.62 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\Microsoft.Spark.Worker.csproj.
        Microsoft.Spark -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark\Debug\netstandard2.1\Microsoft.Spark.dll
        Microsoft.Spark.Worker -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\Microsoft.Spark.Worker.dll
        Microsoft.Spark.Worker -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\publish\
      ```

  2. 生成示例：

      ```powershell
      cd C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\
      dotnet publish -f netcoreapp3.1 -r win-x64
      ```

      控制台输出示例：

      ```powershell
      PS C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples> dotnet publish -f netcoreapp3.1 -r win-x64
      Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
      Copyright (C) Microsoft Corporation. All rights reserved.

        Restore completed in 44.22 ms for C:\github\dotnet-spark\src\csharp\Microsoft.Spark\Microsoft.Spark.csproj.
        Restore completed in 336.94 ms for C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\Microsoft.Spark.CSharp.Examples.csproj.
        Microsoft.Spark -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark\Debug\netstandard2.1\Microsoft.Spark.dll
        Microsoft.Spark.CSharp.Examples -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\Microsoft.Spark.CSharp.Examples.dll
        Microsoft.Spark.CSharp.Examples -> C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\publish\
      ```

## <a name="run-the-net-for-spark-sample-applications"></a>运行 .NET for Spark 示例应用程序

生成示例后，无论你是面向 .NET Framework 还是 .NET Core，都可以通过 `spark-submit` 来运行它们。 请确保已按照[先决条件](#prerequisites)部分操作并安装 Apache Spark。

  1. 设置 `DOTNET_WORKER_DIR` 或 `PATH` 环境变量，使其包含已生成 `Microsoft.Spark.Worker` 二进制文件的路径（例如，在 .NET Framework 中为 C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.Worker\Debug\net461，在 .NET Core 中为 C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\publish） ：

      ```powershell
      set DOTNET_WORKER_DIR=C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\publish
      ```
  
  2. 打开 PowerShell 并转到已生成应用二进制文件的目录（例如，在 .NET Framework 中为 C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461，在 .NET Core 在为 C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\publish） ：

      ```powershell
      cd C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\publish
      ```

  3. 运行应用程序的基本结构如下：

     ```powershell
     spark-submit.cmd `
       [--jars <any-jars-your-app-is-dependent-on>] `
       --class org.apache.spark.deploy.dotnet.DotnetRunner `
       --master local `
       <path-to-microsoft-spark-jar> `
       <path-to-your-app-exe> <argument(s)-to-your-app>
     ```

     下面是可以运行的一些示例：

     - [Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs) 

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Batch.Basic %SPARK_HOME%\examples\src\main\resources\people.json
         ```

     - [Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs) 

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkWordCount localhost 9999
         ```

     - [Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（可访问 maven）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) 

         ```powershell
         spark-submit.cmd `
         --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
         ```

     - [Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（提供 jar）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) 

         ```powershell
         spark-submit.cmd
         --jars path\to\net.jpountz.lz4\lz4-1.3.0.jar,path\to\org.apache.kafka\kafka-clients-0.10.0.1.jar,path\to\org.apache.spark\spark-sql-kafka-0-10_2.11-2.3.2.jar,`path\to\org.slf4j\slf4j-api-1.7.6.jar,path\to\org.spark-project.spark\unused-1.0.0.jar,path\to\org.xerial.snappy\snappy-java-1.1.2.6.jar `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
          ```
