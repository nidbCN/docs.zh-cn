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
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-ubuntu"></a><span data-ttu-id="3f6b1-103">了解如何在 Ubuntu 上生成 .NET for Apache Spark 应用程序</span><span class="sxs-lookup"><span data-stu-id="3f6b1-103">Learn how to build your .NET for Apache Spark application on Ubuntu</span></span>

<span data-ttu-id="3f6b1-104">本文介绍如何在 Ubuntu 上生成 .NET for Apache Spark 应用程序。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-104">This article teaches you how to build your .NET for Apache Spark applications on Ubuntu.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f6b1-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="3f6b1-105">Prerequisites</span></span>

<span data-ttu-id="3f6b1-106">如果具备以下所有先决条件，请跳到[生成](#build)步骤。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-106">If you already have all of the following prerequisites, skip to the [build](#build) steps.</span></span>

1. <span data-ttu-id="3f6b1-107">下载并安装 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1) - 安装 SDK 会将 `dotnet` 工具链添加到路径。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-107">Download and install **[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1)** - installing the SDK adds the `dotnet` toolchain to your path.</span></span>  <span data-ttu-id="3f6b1-108">支持 .NET Core 2.1、2.2 和 3.1。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-108">.NET Core 2.1, 2.2 and 3.1 are supported.</span></span>

2. <span data-ttu-id="3f6b1-109">安装 [OpenJDK 8](https://openjdk.java.net/install/)。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-109">Install **[OpenJDK 8](https://openjdk.java.net/install/)**.</span></span>

   - <span data-ttu-id="3f6b1-110">可以使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-110">You can use the following command:</span></span>

   ```bash
   sudo apt install openjdk-8-jdk
   ```

   * <span data-ttu-id="3f6b1-111">验证是否能够从命令行运行 `java`。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-111">Verify you are able to run `java` from your command-line.</span></span>

      <span data-ttu-id="3f6b1-112">Java 版本输出示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-112">Sample java -version output:</span></span>

      ```bash
      openjdk version "1.8.0_191"
      OpenJDK Runtime Environment (build 1.8.0_191-8u191-b12-2ubuntu0.18.04.1-b12)
      OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
      ```

   * <span data-ttu-id="3f6b1-113">如果已安装多个 OpenJDK 版本，并想要选择 OpenJDK 8，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-113">If you already have multiple OpenJDK versions installed and want to select OpenJDK 8, use the following command:</span></span>

      ```bash
      sudo update-alternatives --config java
      ```

3. <span data-ttu-id="3f6b1-114">安装 [Apache Maven 3.6.0+](https://maven.apache.org/download.cgi)。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-114">Install **[Apache Maven 3.6.0+](https://maven.apache.org/download.cgi)**.</span></span>

   * <span data-ttu-id="3f6b1-115">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-115">Run the following command:</span></span>

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

       <span data-ttu-id="3f6b1-116">请注意，关闭终端时这些环境变量将丢失。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-116">Note that these environment variables will be lost when you close your terminal.</span></span> <span data-ttu-id="3f6b1-117">如果希望更改是永久性的，请将 `export` 行添加到 `~/.bashrc` 文件中。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-117">If you want the changes to be permanent, add the `export` lines to your `~/.bashrc` file.</span></span>

   * <span data-ttu-id="3f6b1-118">验证是否能够从命令行运行 `mvn`</span><span class="sxs-lookup"><span data-stu-id="3f6b1-118">Verify you are able to run `mvn` from your command-line</span></span>

       <span data-ttu-id="3f6b1-119">mvn 版本输出示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-119">Sample mvn -version output:</span></span>

       ```output
       Apache Maven 3.6.0 (97c98ec64a1fdfee7767ce5ffb20918da4f719f3; 2018-10-24T18:41:47Z)
       Maven home: ~/bin/apache-maven-3.6.0
       Java version: 1.8.0_191, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-8-openjdk-amd64/jre
       Default locale: en, platform encoding: UTF-8
       OS name: "linux", version: "4.4.0-17763-microsoft", arch: "amd64", family: "unix"
       ```

4. <span data-ttu-id="3f6b1-120">安装 [Apache Spark 2.3+](https://spark.apache.org/downloads.html)。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-120">Install **[Apache Spark 2.3+](https://spark.apache.org/downloads.html)**.</span></span>
<span data-ttu-id="3f6b1-121">下载 [Apache Spark 2.3+](https://spark.apache.org/downloads.html) 并将其提取到本地文件夹（例如 `~/bin/spark-3.0.1-bin-hadoop2.7`）。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-121">Download [Apache Spark 2.3+](https://spark.apache.org/downloads.html) and extract it into a local folder (e.g., `~/bin/spark-3.0.1-bin-hadoop2.7`).</span></span> <span data-ttu-id="3f6b1-122">（支持的 spark 版本为 2.3.\*、2.4.0、2.4.1、2.4.3、2.4.4、2.4.5、2.4.6、2.4.7、3.0.0 和 3.0.1）</span><span class="sxs-lookup"><span data-stu-id="3f6b1-122">(The supported spark versions are 2.3.\*, 2.4.0, 2.4.1, 2.4.3, 2.4.4, 2.4.5, 2.4.6, 2.4.7, 3.0.0 and 3.0.1)</span></span>

   ```bash
   tar -xvzf /path/to/spark-3.0.1-bin-hadoop2.7.tgz -C ~/bin/spark-3.0.1-bin-hadoop2.7
   ```

   * <span data-ttu-id="3f6b1-123">添加必需的[环境变量](https://www.java.com/en/download/help/path.xml) `SPARK_HOME`（例如 `~/bin/spark-3.0.1-bin-hadoop2.7/`）和 `PATH`（例如 `$SPARK_HOME/bin:$PATH`）</span><span class="sxs-lookup"><span data-stu-id="3f6b1-123">Add the necessary [environment variables](https://www.java.com/en/download/help/path.xml) `SPARK_HOME` (e.g., `~/bin/spark-3.0.1-bin-hadoop2.7/`) and `PATH` (e.g., `$SPARK_HOME/bin:$PATH`)</span></span>

      ```bash
      export SPARK_HOME=~/bin/spark-3.0.1-hadoop2.7
      export PATH="$SPARK_HOME/bin:$PATH"
      source ~/.bashrc
      ```

      <span data-ttu-id="3f6b1-124">请注意，关闭终端时这些环境变量将丢失。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-124">Note that these environment variables will be lost when you close your terminal.</span></span> <span data-ttu-id="3f6b1-125">如果希望更改是永久性的，请将 `export` 行添加到 `~/.bashrc` 文件中。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-125">If you want the changes to be permanent, add the `export` lines to your `~/.bashrc` file.</span></span>

   * <span data-ttu-id="3f6b1-126">验证是否能够从命令行运行 `spark-shell`。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-126">Verify you are able to run `spark-shell` from your command-line.</span></span>

      <span data-ttu-id="3f6b1-127">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-127">Sample console output:</span></span>

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

<span data-ttu-id="3f6b1-128">确保可以从命令行运行 `dotnet`、`java`、`mvn`、`spark-shell`，然后再转到下一部分。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-128">Make sure you are able to run `dotnet`, `java`, `mvn`, `spark-shell` from your command-line before you move to the next section.</span></span> <span data-ttu-id="3f6b1-129">认为还有更好的办法？</span><span class="sxs-lookup"><span data-stu-id="3f6b1-129">Feel there is a better way?</span></span> <span data-ttu-id="3f6b1-130">请[发布问题](https://github.com/dotnet/spark/issues)随意参与。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-130">Please [open an issue](https://github.com/dotnet/spark/issues) and feel free to contribute.</span></span>

## <a name="build"></a><span data-ttu-id="3f6b1-131">生成</span><span class="sxs-lookup"><span data-stu-id="3f6b1-131">Build</span></span>

<span data-ttu-id="3f6b1-132">对于本指南的其余部分，需要将 .NET for Apache Spark 存储库克隆到计算机，例如 `~/dotnet.spark/`。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-132">For the remainder of this guide, you will need to have cloned the .NET for Apache Spark repository into your machine e.g., `~/dotnet.spark/`.</span></span>

```bash
git clone https://github.com/dotnet/spark.git ~/dotnet.spark
```

### <a name="build-net-for-spark-scala-extensions-layer"></a><span data-ttu-id="3f6b1-133">生成 .NET for Spark Scala 扩展层</span><span class="sxs-lookup"><span data-stu-id="3f6b1-133">Build .NET for Spark Scala extensions layer</span></span>

<span data-ttu-id="3f6b1-134">提交 .NET 应用程序时，.NET for Apache Spark 具有在 Scala 中编写的必要逻辑，以指示 Apache Spark 如何处理请求（例如，请求创建新的 Spark 会话，请求将数据从 .NET 端传输到 JVM 端等）。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-134">When you submit a .NET application, .NET for Apache Spark has the necessary logic written in Scala that informs Apache Spark how to handle your requests (e.g., request to create a new Spark Session, request to transfer data from .NET side to JVM side etc.).</span></span> <span data-ttu-id="3f6b1-135">此逻辑可在 [.NET for Apache Spark Scala 源代码](https://github.com/dotnet/spark/tree/main/src/scala)中找到。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-135">This logic can be found in the [.NET for Apache Spark Scala Source Code](https://github.com/dotnet/spark/tree/main/src/scala).</span></span>

<span data-ttu-id="3f6b1-136">下一步是生成 .NET for Apache Spark Scala 扩展层：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-136">The next step is to build the .NET for Apache Spark Scala extension layer:</span></span>

```bash
cd src/scala
mvn clean package
```

<span data-ttu-id="3f6b1-137">应会看到为支持的 Spark 版本创建的 JAR：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-137">You should see JARs created for the supported Spark versions:</span></span>

* `microsoft-spark-2-3\target\microsoft-spark-2-3_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-2-4\target\microsoft-spark-2-4_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-3-0\target\microsoft-spark-3-0_2.12-<spark-dotnet-version>.jar`

### <a name="build-net-sample-applications-using-net-core-cli"></a><span data-ttu-id="3f6b1-138">使用 .NET Core CLI 生成 .NET 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="3f6b1-138">Build .NET sample applications using .NET Core CLI</span></span>

<span data-ttu-id="3f6b1-139">本部分介绍如何为 .NET for Apache Spark 生成[示例应用程序](https://github.com/dotnet/spark/tree/main/examples)。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-139">This section explains how to build the [sample applications](https://github.com/dotnet/spark/tree/main/examples) for .NET for Apache Spark.</span></span> <span data-ttu-id="3f6b1-140">这些步骤有助于了解任何 .NET for Spark 应用程序的整个生成过程。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-140">These steps will help in understanding the overall building process for any .NET for Spark application.</span></span>

1. <span data-ttu-id="3f6b1-141">生成辅助角色：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-141">Build the worker:</span></span>

   ```dotnetcli
   cd ~/dotnet.spark/src/csharp/Microsoft.Spark.Worker/
   dotnet publish -f netcoreapp3.1 -r ubuntu.18.04-x64
   ```

   <span data-ttu-id="3f6b1-142">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-142">Sample console output:</span></span>

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

2. <span data-ttu-id="3f6b1-143">生成示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-143">Build the samples:</span></span>

   ```dotnetcli
   cd ~/dotnet.spark/examples/Microsoft.Spark.CSharp.Examples/
   dotnet publish -f netcoreapp3.1 -r ubuntu.18.04-x64
   ```

   <span data-ttu-id="3f6b1-144">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-144">Sample console output:</span></span>

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

## <a name="run-the-net-for-spark-sample-applications"></a><span data-ttu-id="3f6b1-145">运行 .NET for Spark 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="3f6b1-145">Run the .NET for Spark sample applications</span></span>

<span data-ttu-id="3f6b1-146">生成示例后，可以使用 `spark-submit` 来提交 .NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-146">Once you build the samples, you can use `spark-submit` to submit your .NET Core apps.</span></span> <span data-ttu-id="3f6b1-147">请确保已按照[先决条件](#prerequisites)部分操作并安装 Apache Spark。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-147">Make sure you have followed the [prerequisites](#prerequisites) section and installed Apache Spark.</span></span>

1. <span data-ttu-id="3f6b1-148">设置 `DOTNET_WORKER_DIR` 或 `PATH` 环境变量以包含已生成 `Microsoft.Spark.Worker` 二进制文件的路径（例如 `~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish`）。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-148">Set the `DOTNET_WORKER_DIR` or `PATH` environment variable to include the path where the `Microsoft.Spark.Worker` binary has been generated (e.g., `~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish`).</span></span>

   ```bash
   export DOTNET_WORKER_DIR=~/dotnet.spark/artifacts/bin/Microsoft.Spark.Worker/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish
   ```

2. <span data-ttu-id="3f6b1-149">打开终端，转到生成应用程序二进制文件的目录（例如 `~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish`）。</span><span class="sxs-lookup"><span data-stu-id="3f6b1-149">Open a terminal and go to the directory where your app binary has been generated (e.g., `~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish`).</span></span>

   ```bash
   cd ~/dotnet.spark/artifacts/bin/Microsoft.Spark.CSharp.Examples/Debug/netcoreapp3.1/ubuntu.18.04-x64/publish
   ```

3. <span data-ttu-id="3f6b1-150">运行应用程序的基本结构如下：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-150">Running your app follows the basic structure:</span></span>

   ```bash
   spark-submit \
     [--jars <any-jars-your-app-is-dependent-on>] \
     --class org.apache.spark.deploy.dotnet.DotnetRunner \
     --master local \
     <path-to-microsoft-spark-jar> \
     <path-to-your-app-binary> <argument(s)-to-your-app>
   ```

   <span data-ttu-id="3f6b1-151">下面是可以运行的一些示例：</span><span class="sxs-lookup"><span data-stu-id="3f6b1-151">Here are some examples you can run:</span></span>

   * <span data-ttu-id="3f6b1-152">[Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs) </span><span class="sxs-lookup"><span data-stu-id="3f6b1-152">**[Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs)**</span></span>

      ```bash
      spark-submit \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Batch.Basic $SPARK_HOME/examples/src/main/resources/people.json
      ```

   * <span data-ttu-id="3f6b1-153">[Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs) </span><span class="sxs-lookup"><span data-stu-id="3f6b1-153">**[Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)**</span></span>

      ```bash
      spark-submit \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredNetworkWordCount localhost 9999
      ```

   * <span data-ttu-id="3f6b1-154">[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（可访问 maven）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) </span><span class="sxs-lookup"><span data-stu-id="3f6b1-154">**[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (maven accessible)](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**</span></span>

      ```bash
      spark-submit \
      --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
      ```

   * <span data-ttu-id="3f6b1-155">[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（提供 jar）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) </span><span class="sxs-lookup"><span data-stu-id="3f6b1-155">**[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (jars provided)](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**</span></span>

      ```bash
      spark-submit \
      --jars path/to/net.jpountz.lz4/lz4-1.3.0.jar,path/to/org.apache.kafka/kafka-clients-0.10.0.1.jar,path/to/org.apache.spark/spark-sql-kafka-0-10_2.11-2.3.2.jar,`path/to/org.slf4j/slf4j-api-1.7.6.jar,path/to/org.spark-project.spark/unused-1.0.0.jar,path/to/org.xerial.snappy/snappy-java-1.1.2.6.jar \
      --class org.apache.spark.deploy.dotnet.DotnetRunner \
      --master local \
      ~/dotnet.spark/src/scala/microsoft-spark-<version>/target/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
      Microsoft.Spark.CSharp.Examples Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
       ```
