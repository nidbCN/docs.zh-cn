---
title: 在 Ubuntu 上生成 .NET for Apache Spark 应用程序
description: 了解如何在 Ubuntu 上生成 .NET for Apache Spark 应用程序
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 300c858a76ba31650615bde70c951e383e9e0436
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875325"
---
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-ubuntu"></a>了解如何在 Ubuntu 上生成 .NET for Apache Spark 应用程序

本文介绍如何在 Ubuntu 上生成 .NET for Apache Spark 应用程序。

## <a name="prerequisites"></a>先决条件

如果具备以下所有先决条件，请跳到[生成](#build)步骤。

1. 下载并安装 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1) - 安装 SDK 会将 `dotnet` 工具链添加到路径。  支持 .NET Core 2.1、2.2 和 3.1。

2. 安装 [OpenJDK 8](https://openjdk.java.net/install/)。

   - 可以使用以下命令：

   ```bash
   sudo apt install openjdk-8-jdk
   ```

   * 验证是否能够从命令行运行 `java`。

      Java 版本输出示例：

      ```bash
      openjdk version "1.8.0_191"
      OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
      OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
      ```

   * 如果已安装多个 OpenJDK 版本，并想要选择 OpenJDK 8，请使用以下命令：

      ```bash
      sudo update-alternatives --config java
      ```

3. 安装 [Apache Maven 3.6.0+](https://maven.apache.org/download.cgi)。

   * 运行下面的命令：

      ```bash
      mkdir -p ~/bin/maven
      cd ~/bin/maven
      wget https://www-us.apache.org/dist/maven/maven-3/3.6.0/binaries/apache-maven-3.6.0-bin.tar.gz
      tar -xvzf apache-maven-3.6.0-bin.tar.gz
      ln -s apache-maven-3.6.0 current
      export M2_HOME=~/bin/maven/current
      export PATH=${M2_HOME}/bin:${PATH}
      source ~/.bashrc
      ```

       请注意，关闭终端时这些环境变量将丢失。 如果希望更改是永久性的，请将 `export` 行添加到 `~/.bashrc` 文件中。

   * 验证是否能够从命令行运行 `mvn`

       mvn 版本输出示例：

       ```output
       Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-24T18:41:47Z)
       Maven home: ~/bin/apache-maven-3.6.0
       Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
       Default locale: en, platform encoding: UTF-8
       OS name: "linux", version: "4.4.0-17763-microsoft", arch: "amd64", family: "unix"
       ```

4. 安装 [Apache Spark 2.3+](https://spark.apache.org/downloads.html)。
下载 [Apache Spark 2.3+](https://spark.apache.org/downloads.html) 并将其提取到本地文件夹（例如 `~/bin/spark-3.0.1-bin-hadoop2.7`）。 （支持的 spark 版本为 2.3.*、2.4.0、2.4.1、2.4.3、2.4.4、2.4.5、2.4.6、2.4.7、3.0.0 和 3.0.1）

   ```bash
   tar -xvzf /path/to/spark-3.0.1-bin-hadoop2.7.tgz -C ~/bin/spark-3.0.1-bin-hadoop2.7
   ```

   * 添加必需的[环境变量](https://www.java.com/en/download/help/path.xml) `SPARK_HOME`（例如 `~/bin/spark-3.0.1-bin-hadoop2.7/`）和 `PATH`（例如 `$SPARK_HOME/bin:$PATH`）

      ```bash
      export SPARK_HOME=~/bin/spark-3.0.1-hadoop2.7
      export PATH="$SPARK_HOME/bin:$PATH"
      source ~/.bashrc
      ```

      请注意，关闭终端时这些环境变量将丢失。 如果希望更改是永久性的，请将 `export` 行添加到 `~/.bashrc` 文件中。

   * 验证是否能够从命令行运行 `spark-shell`。

      控制台输出示例：

      ```
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

确保可以从命令行运行 `dotnet`、`java`、`mvn`、`spark-shell`，然后再转到下一部分。 认为还有更好的办法？ 请[发布问题](https://github.com/dotnet/spark/issues)随意参与。

## <a name="build"></a>生成

对于本指南的其余部分，需要将 .NET for Apache Spark 存储库克隆到计算机，例如 `~/dotnet.spark/`。

```bash
git clone https://github.com/dotnet/spark.git ~/dotnet.spark
```

### <a name="build-net-for-spark-scala-extensions-layer"></a>生成 .NET for Spark Scala 扩展层

提交 .NET 应用程序时，.NET for Apache Spark 具有在 Scala 中编写的必要逻辑，以指示 Apache Spark 如何处理请求（例如，请求创建新的 Spark 会话，请求将数据从 .NET 端传输到 JVM 端等）。 此逻辑可在 [.NET for Apache Spark Scala 源代码](https://github.com/dotnet/spark/tree/main/src/scala)中找到。

下一步是生成 .NET for Apache Spark Scala 扩展层：

```bash
cd src/scala
mvn clean package
```

应会看到为支持的 Spark 版本创建的 JAR：

* `microsoft-spark-2-3\target\microsoft-spark-2-3_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-2-4\target\microsoft-spark-2-4_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-3-0\target\microsoft-spark-3-0_2.12-<spark-dotnet-version>.jar`

### <a name="build-net-sample-applications-using-net-core-cli"></a>使用 .NET Core CLI 生成 .NET 示例应用程序

本部分介绍如何为 .NET for Apache Spark 生成[示例应用程序](https://github.com/dotnet/spark/tree/main/examples)。 这些步骤有助于了解任何 .NET for Spark 应用程序的整个生成过程。

1. 生成辅助角色：

   ```dotnetcli
   cd ~/dotnet.spark/src/csharp/Microsoft.Spark.Worker/
   dotnet publish -f netcoreapp3.1 -r ubuntu.18.04-x64
   ```

   控制台输出示例：

   ```bash
   user@machine:/home/user/dotnet.spark/src/csharp/Microsoft.Spark.Worker$ dotnet publish -f netcoreapp3.1 -r ubuntu.18.04-x64
   Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

      Restore completed in 36.03 ms for /home/user/dotnet.spark/src/csharp/Microsoft.Spark.Worker/Microsoft.Spark.Worker.csproj.
      Restore completed in 35.94 ms for /home/user/dotnet.spark/src/csharp/Microsoft.Spark/Microsoft.Spark.csproj.
      Microsoft.Spark -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark/Debug/netstandard2.1/Microsoft.Spark.dll
      Microsoft.Spark.Worker -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish/Microsoft.Spark.Worker.dll
      Microsoft.Spark.Worker -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish/
   ```

2. 生成示例：

   ```dotnetcli
   cd ~/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples/
   dotnet publish -f netcoreapp3.1 -r ubuntu.18.04-x64
   ```

   控制台输出示例：

   ```bash
   user@machine:/home/user/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples$ dotnet publish -f netcoreapp3.1 -r ubuntu.18.04-x64
   Microsoft (R) Build Engine version 16.6.0+5ff7b0c9e for .NET Core
   Copyright (C) Microsoft Corporation. All rights reserved.

      Restore completed in 37.11 ms for /home/user/dotnet.spark/src/csharp/Microsoft.Spark/Microsoft.Spark.csproj.
      Restore completed in 281.63 ms for /home/user/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples/Microsoft.Spark.CSharp.Examples.csproj.
      Microsoft.Spark -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark/Debug/netstandard2.1/Microsoft.Spark.dll
      Microsoft.Spark.CSharp.Examples -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish/Microsoft.Spark.CSharp.Examples.dll
      Microsoft.Spark.CSharp.Examples -> /home/user/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish/
   ```  

## <a name="run-the-net-for-spark-sample-applications"></a>运行 .NET for Spark 示例应用程序

生成示例后，可以使用 `spark-submit` 来提交 .NET Core 应用。 请确保已按照[先决条件](#prerequisites)部分操作并安装 Apache Spark。

1. 设置 `DOTNET_WORKER_DIR` 或 `PATH` 环境变量以包含已生成 `Microsoft.Spark.Worker` 二进制文件的路径（例如 `~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish`）。

   ```bash
   export DOTNET_WORKER_DIR=~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish
   ```

2. 打开终端，转到生成应用程序二进制文件的目录（例如 `~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish`）。

   ```bash
   cd ~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish
   ```

3. 运行应用程序的基本结构如下：

   ```bash
   spark-submit \
     [--jars <any-jars-your-app-is-dependent-on>] \
     --class org.apache.spark.deploy.dotnet.DotnetRunner \
     --master local \
     <path-to-microsoft-spark-jar> \
     <path-to-your-app-binary> <argument(s)-to-your-app>
   ```

   下面是可以运行的一些示例：

   * [Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs) 

      ```bash
      spark-submit \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Batch.Basic $SPARK_HOME/examples/src/main/resources/people.json
      ```

   * [Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs) 

      ```bash
      spark-submit \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredNetworkWordCount localhost 9999
      ```

   * [Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（可访问 maven）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) 

      ```bash
      spark-submit \
      --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
      ```

   * [Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（提供 jar）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) 

      ```bash
      spark-submit \
      --jars path/to/net.jpountz.lz4/lz4-1.3.0.jar,path/to/org.apache.kafka/kafka-clients-0.10.0.1.jar,path/to/org.apache.spark/spark-sql-kafka-0-10_2.11-2.3.2.jar,`path/to/org.slf4j/slf4j-api-1.7.6.jar,path/to/org.spark-project.spark/unused-1.0.0.jar,path/to/org.xerial.snappy/snappy-java-1.1.2.6.jar \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
       ```
