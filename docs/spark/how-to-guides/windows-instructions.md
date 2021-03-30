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
# <a name="learn-how-to-build-your-net-for-apache-spark-application-on-windows"></a><span data-ttu-id="d3f52-103">了解如何在 Windows 上生成 .NET for Apache Spark 应用程序</span><span class="sxs-lookup"><span data-stu-id="d3f52-103">Learn how to build your .NET for Apache Spark application on Windows</span></span>

<span data-ttu-id="d3f52-104">本文介绍如何在 Windows 上生成 .NET for Apache Spark 应用程序。</span><span class="sxs-lookup"><span data-stu-id="d3f52-104">This article teaches you how to build your .NET for Apache Spark applications on Windows.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3f52-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="d3f52-105">Prerequisites</span></span>

<span data-ttu-id="d3f52-106">如果具备以下所有先决条件，请跳到[生成](#build)步骤。</span><span class="sxs-lookup"><span data-stu-id="d3f52-106">If you already have all of the following prerequisites, skip to the [build](#build) steps.</span></span>

  1. <span data-ttu-id="d3f52-107">下载并安装 [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet/3.1)  - 安装 SDK 会将 `dotnet` 工具链添加到路径。</span><span class="sxs-lookup"><span data-stu-id="d3f52-107">Download and install the **[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet/3.1)** - installing the SDK will add the `dotnet` toolchain to your path.</span></span> <span data-ttu-id="d3f52-108">支持 .NET Core 2.1、2.2 和 3.1。</span><span class="sxs-lookup"><span data-stu-id="d3f52-108">.NET Core 2.1, 2.2 and 3.1 are supported.</span></span>
  2. <span data-ttu-id="d3f52-109">安装 [Visual Studio 2019](https://www.visualstudio.com/downloads/)  （版本 16.3 或更高版本）。</span><span class="sxs-lookup"><span data-stu-id="d3f52-109">Install **[Visual Studio 2019](https://www.visualstudio.com/downloads/)** (Version 16.3 or later).</span></span> <span data-ttu-id="d3f52-110">Community 版本完全免费。</span><span class="sxs-lookup"><span data-stu-id="d3f52-110">The Community version is completely free.</span></span> <span data-ttu-id="d3f52-111">配置安装时，请至少包含以下组件：</span><span class="sxs-lookup"><span data-stu-id="d3f52-111">When configuring your installation, include these components at minimum:</span></span>
     * <span data-ttu-id="d3f52-112">.NET 桌面开发</span><span class="sxs-lookup"><span data-stu-id="d3f52-112">.NET desktop development</span></span>
       * <span data-ttu-id="d3f52-113">所有必需的组件</span><span class="sxs-lookup"><span data-stu-id="d3f52-113">All Required Components</span></span>
         * <span data-ttu-id="d3f52-114">.NET Framework 4.6.1 开发工具</span><span class="sxs-lookup"><span data-stu-id="d3f52-114">.NET Framework 4.6.1 Development Tools</span></span>
     * <span data-ttu-id="d3f52-115">.NET Core 跨平台开发</span><span class="sxs-lookup"><span data-stu-id="d3f52-115">.NET Core cross-platform development</span></span>
       * <span data-ttu-id="d3f52-116">所有必需的组件</span><span class="sxs-lookup"><span data-stu-id="d3f52-116">All Required Components</span></span>
  3. <span data-ttu-id="d3f52-117">安装 [Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-117">Install **[Java 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)**.</span></span>
     - <span data-ttu-id="d3f52-118">选择适用于操作系统的合适版本。</span><span class="sxs-lookup"><span data-stu-id="d3f52-118">Select the appropriate version for your operating system.</span></span> <span data-ttu-id="d3f52-119">例如，为 Windows x64 计算机选择 jdk-8u201-windows-x64.exe  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-119">For example, *jdk-8u201-windows-x64.exe* for Windows x64 machine.</span></span>
     - <span data-ttu-id="d3f52-120">使用安装程序安装，验证是否能够从命令行运行 `java`。</span><span class="sxs-lookup"><span data-stu-id="d3f52-120">Install using the installer and verify you are able to run `java` from your command line.</span></span>
  4. <span data-ttu-id="d3f52-121">安装 [Apache Maven 3.6.0+](https://maven.apache.org/download.cgi)  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-121">Install **[Apache Maven 3.6.0+](https://maven.apache.org/download.cgi)**.</span></span>
     - <span data-ttu-id="d3f52-122">下载 [Apache Maven 3.6.0](http://mirror.metrocast.net/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip)。</span><span class="sxs-lookup"><span data-stu-id="d3f52-122">Download [Apache Maven 3.6.0](http://mirror.metrocast.net/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip).</span></span>
     - <span data-ttu-id="d3f52-123">提取到本地目录。</span><span class="sxs-lookup"><span data-stu-id="d3f52-123">Extract to a local directory.</span></span> <span data-ttu-id="d3f52-124">例如 \*C:\bin\apache-maven-3.6.0\*。</span><span class="sxs-lookup"><span data-stu-id="d3f52-124">For example, \*C:\bin\apache-maven-3.6.0\*.</span></span>
     - <span data-ttu-id="d3f52-125">将 Apache Maven 添加到 [PATH 环境变量](https://www.java.com/en/download/help/path.xml)。</span><span class="sxs-lookup"><span data-stu-id="d3f52-125">Add Apache Maven to your [PATH environment variable](https://www.java.com/en/download/help/path.xml).</span></span> <span data-ttu-id="d3f52-126">例如 C:\bin\apache-maven-3.6.0\bin  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-126">For example, *C:\bin\apache-maven-3.6.0\bin*.</span></span>
     - <span data-ttu-id="d3f52-127">验证是否能够从命令行运行 `mvn`。</span><span class="sxs-lookup"><span data-stu-id="d3f52-127">Verify you are able to run `mvn` from your command-line.</span></span>
  5. <span data-ttu-id="d3f52-128">安装 [Apache Spark 2.3+](https://spark.apache.org/downloads.html)  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-128">Install **[Apache Spark 2.3+](https://spark.apache.org/downloads.html)**.</span></span>
     - <span data-ttu-id="d3f52-129">下载 [Apache Spark 2.3 +](https://spark.apache.org/downloads.html)，并使用 [7-zip](https://www.7-zip.org/) 将其提取到本地文件夹，例如 C:\bin\spark-3.0.1-bin-hadoop2.7 *\*。* （支持的 spark 版本为 2.3.、2.4.0、2.4.1、2.4.3、2.4.4、2.4.5、2.4.6、2.4.7、3.0.0 和 3.0.1）</span><span class="sxs-lookup"><span data-stu-id="d3f52-129">Download [Apache Spark 2.3+](https://spark.apache.org/downloads.html) and extract it into a local folder (for example, *C:\bin\spark-3.0.1-bin-hadoop2.7\*) using [7-zip](https://www.7-zip.org/). (The supported spark versions are 2.3.*, 2.4.0, 2.4.1, 2.4.3, 2.4.4, 2.4.5, 2.4.6, 2.4.7, 3.0.0 and 3.0.1)</span></span>
     - <span data-ttu-id="d3f52-130">添加[新环境变量](https://www.java.com/en/download/help/path.xml) `SPARK_HOME`。</span><span class="sxs-lookup"><span data-stu-id="d3f52-130">Add a [new environment variable](https://www.java.com/en/download/help/path.xml) `SPARK_HOME`.</span></span> <span data-ttu-id="d3f52-131">例如 \*C:\bin\spark-3.0.1-bin-hadoop2.7\*。</span><span class="sxs-lookup"><span data-stu-id="d3f52-131">For example, \*C:\bin\spark-3.0.1-bin-hadoop2.7\*.</span></span>

       ```powershell
       set SPARK_HOME=C:\bin\spark-3.0.1-bin-hadoop2.7\
       ```

     - <span data-ttu-id="d3f52-132">将 Apache Spark 添加到 [PATH 环境变量](https://www.java.com/en/download/help/path.xml)。</span><span class="sxs-lookup"><span data-stu-id="d3f52-132">Add Apache Spark to your [PATH environment variable](https://www.java.com/en/download/help/path.xml).</span></span> <span data-ttu-id="d3f52-133">例如 C:\bin\spark-3.0.1-bin-hadoop2.7\bin。</span><span class="sxs-lookup"><span data-stu-id="d3f52-133">For example, *C:\bin\spark-3.0.1-bin-hadoop2.7\bin*.</span></span>

       ```powershell
       set PATH=%SPARK_HOME%\bin;%PATH%
       ```

     - <span data-ttu-id="d3f52-134">验证是否能够从命令行运行 `spark-shell`。</span><span class="sxs-lookup"><span data-stu-id="d3f52-134">Verify you are able to run `spark-shell` from your command-line.</span></span>
        <span data-ttu-id="d3f52-135">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="d3f52-135">Sample console output:</span></span>

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

  6. <span data-ttu-id="d3f52-136">安装 [WinUtils](https://github.com/steveloughran/winutils)  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-136">Install **[WinUtils](https://github.com/steveloughran/winutils)**.</span></span>
     - <span data-ttu-id="d3f52-137">从 [WinUtils 存储库](https://github.com/steveloughran/winutils)下载 `winutils.exe` 二进制文件。</span><span class="sxs-lookup"><span data-stu-id="d3f52-137">Download `winutils.exe` binary from [WinUtils repository](https://github.com/steveloughran/winutils).</span></span> <span data-ttu-id="d3f52-138">应选择编译 Spark 发行版时使用的 Hadoop 版本。</span><span class="sxs-lookup"><span data-stu-id="d3f52-138">You should select the version of Hadoop the Spark distribution was compiled with.</span></span> <span data-ttu-id="d3f52-139">例如，将 hadoop-2.7.1 用于 Spark 3.0.1。</span><span class="sxs-lookup"><span data-stu-id="d3f52-139">For exammple, use hadoop-2.7.1 for Spark 3.0.1.</span></span>
     - <span data-ttu-id="d3f52-140">将 `winutils.exe` 二进制文件保存到所选目录。</span><span class="sxs-lookup"><span data-stu-id="d3f52-140">Save `winutils.exe` binary to a directory of your choice.</span></span> <span data-ttu-id="d3f52-141">例如 C:\hadoop\bin  。</span><span class="sxs-lookup"><span data-stu-id="d3f52-141">For example, *C:\hadoop\bin*.</span></span>
     - <span data-ttu-id="d3f52-142">设置 `HADOOP_HOME` 以反映包含 winutils.exe（不带 bin）的目录。</span><span class="sxs-lookup"><span data-stu-id="d3f52-142">Set `HADOOP_HOME` to reflect the directory with winutils.exe (without bin).</span></span> <span data-ttu-id="d3f52-143">例如，使用命令行：</span><span class="sxs-lookup"><span data-stu-id="d3f52-143">For instance, using command-line:</span></span>

       ```powershell
       set HADOOP_HOME=C:\hadoop
       ```

     - <span data-ttu-id="d3f52-144">设置 PATH 环境变量以包含 `%HADOOP_HOME%\bin`。</span><span class="sxs-lookup"><span data-stu-id="d3f52-144">Set PATH environment variable to include `%HADOOP_HOME%\bin`.</span></span> <span data-ttu-id="d3f52-145">例如，使用命令行：</span><span class="sxs-lookup"><span data-stu-id="d3f52-145">For instance, using command line:</span></span>

       ```powershell
       set PATH=%HADOOP_HOME%\bin;%PATH%
       ```

<span data-ttu-id="d3f52-146">确保可以从命令行运行 `dotnet`、`java`、`mvn`、`spark-shell`，然后再进行下一步。</span><span class="sxs-lookup"><span data-stu-id="d3f52-146">Make sure you are able to run `dotnet`, `java`, `mvn`, `spark-shell` from your command line before you move to the next section.</span></span> <span data-ttu-id="d3f52-147">认为还有更好的办法？</span><span class="sxs-lookup"><span data-stu-id="d3f52-147">Feel there is a better way?</span></span> <span data-ttu-id="d3f52-148">请[发布问题](https://github.com/dotnet/spark/issues)并随意参与讨论。</span><span class="sxs-lookup"><span data-stu-id="d3f52-148">[Open an issue](https://github.com/dotnet/spark/issues) and feel free to contribute.</span></span>

> [!NOTE]
> <span data-ttu-id="d3f52-149">如果更新了任何环境变量，可能需要命令行的新实例。</span><span class="sxs-lookup"><span data-stu-id="d3f52-149">A new instance of the command line may be required if any environment variables were updated.</span></span>

## <a name="build"></a><span data-ttu-id="d3f52-150">生成</span><span class="sxs-lookup"><span data-stu-id="d3f52-150">Build</span></span>

<span data-ttu-id="d3f52-151">对于本指南的其余部分，需要将 .NET for Apache Spark 存储库克隆到计算机。</span><span class="sxs-lookup"><span data-stu-id="d3f52-151">For the remainder of this guide, you will need to have cloned the .NET for Apache Spark repository into your machine.</span></span> <span data-ttu-id="d3f52-152">可以为克隆存储库选择任何位置。</span><span class="sxs-lookup"><span data-stu-id="d3f52-152">You can choose any location for the cloned repository.</span></span> <span data-ttu-id="d3f52-153">例如 \*C:\github\dotnet-spark\*。</span><span class="sxs-lookup"><span data-stu-id="d3f52-153">For example, \*C:\github\dotnet-spark\*.</span></span>

```bash
git clone https://github.com/dotnet/spark.git C:\github\dotnet-spark
```

### <a name="build-net-for-apache-spark-scala-extensions-layer"></a><span data-ttu-id="d3f52-154">生成 .NET for Apache Spark Scala 扩展层</span><span class="sxs-lookup"><span data-stu-id="d3f52-154">Build .NET for Apache Spark Scala extensions layer</span></span>

<span data-ttu-id="d3f52-155">提交 .NET 应用程序时，.NET for Apache Spark 具有在 Scala 中编写的必要逻辑，以指示 Apache Spark 如何处理请求（例如，请求创建新的 Spark 会话，请求将数据从 .NET 端传输到 JVM 端等）。</span><span class="sxs-lookup"><span data-stu-id="d3f52-155">When you submit a .NET application, .NET for Apache Spark has the necessary logic written in Scala that informs Apache Spark how to handle your requests (for example, request to create a new Spark Session, request to transfer data from .NET side to JVM side etc.).</span></span> <span data-ttu-id="d3f52-156">此逻辑可在 [.NET for Spark Scala 源代码](https://github.com/dotnet/spark/tree/main/src/scala)中找到。</span><span class="sxs-lookup"><span data-stu-id="d3f52-156">This logic can be found in the [.NET for Spark Scala Source Code](https://github.com/dotnet/spark/tree/main/src/scala).</span></span>

<span data-ttu-id="d3f52-157">无论使用的是 .NET Framework 还是 .NET Core，都需要生成 .NET for Apache Spark Scala 扩展层：</span><span class="sxs-lookup"><span data-stu-id="d3f52-157">Regardless of whether you are using .NET Framework or .NET Core, you will need to build the .NET for Apache Spark Scala extension layer:</span></span>

```powershell
cd src\scala
mvn clean package
```

<span data-ttu-id="d3f52-158">应会看到为支持的 Spark 版本创建的 JAR：</span><span class="sxs-lookup"><span data-stu-id="d3f52-158">You should see JARs created for the supported Spark versions:</span></span>

* `microsoft-spark-2-3\target\microsoft-spark-2-3_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-2-4\target\microsoft-spark-2-4_2.11-<spark-dotnet-version>.jar`
* `microsoft-spark-3-0\target\microsoft-spark-3-0_2.12-<spark-dotnet-version>.jar`

### <a name="build-the-net-for-spark-sample-applications"></a><span data-ttu-id="d3f52-159">生成适用于 .NET for Spark 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="d3f52-159">Build the .NET for Spark sample applications</span></span>

<span data-ttu-id="d3f52-160">本部分介绍如何为 .NET for Apache Spark 生成[示例应用程序](https://github.com/dotnet/spark/tree/main/examples)。</span><span class="sxs-lookup"><span data-stu-id="d3f52-160">This section explains how to build the [sample applications](https://github.com/dotnet/spark/tree/main/examples) for .NET for Apache Spark.</span></span> <span data-ttu-id="d3f52-161">这些步骤有助于了解任何 .NET for Spark 应用程序的整个生成过程。</span><span class="sxs-lookup"><span data-stu-id="d3f52-161">These steps will help in understanding the overall building process for any .NET for Spark application.</span></span>

#### <a name="using-visual-studio-for-net-framework"></a><span data-ttu-id="d3f52-162">使用 Visual Studio for .NET Framework</span><span class="sxs-lookup"><span data-stu-id="d3f52-162">Using Visual Studio for .NET Framework</span></span>

  1. <span data-ttu-id="d3f52-163">在 Visual Studio 中打开 `src\csharp\Microsoft.Spark.sln`，并在 `examples` 文件夹下生成 `Microsoft.Spark.CSharp.Examples` 项目（这也将生成 .NET 绑定项目）。</span><span class="sxs-lookup"><span data-stu-id="d3f52-163">Open `src\csharp\Microsoft.Spark.sln` in Visual Studio and build the `Microsoft.Spark.CSharp.Examples` project under the `examples` folder (this will in turn build the .NET bindings project as well).</span></span> <span data-ttu-id="d3f52-164">如果需要，可以在 `Microsoft.Spark.Examples` 项目中编写自己的代码（本示例中的“input_file json”是包含创建数据帧所需数据的 json 文件）：</span><span class="sxs-lookup"><span data-stu-id="d3f52-164">If you want, you can write your own code in the `Microsoft.Spark.Examples` project (the 'input_file.json' in this example is a json file with the data you want to create the dataframe with):</span></span>
  
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

     <span data-ttu-id="d3f52-165">成功生成后，会在输出目录中看到相应的二进制文件。</span><span class="sxs-lookup"><span data-stu-id="d3f52-165">Once the build is successful, you will see the appropriate binaries produced in the output directory.</span></span>
     <span data-ttu-id="d3f52-166">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="d3f52-166">Sample console output:</span></span>

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

#### <a name="using-net-core-cli-for-net-core"></a><span data-ttu-id="d3f52-167">使用适用于 .NET Core 的 .NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="d3f52-167">Using .NET Core CLI for .NET Core</span></span>

> [!NOTE]
> <span data-ttu-id="d3f52-168">当前我们正致力于自动生成 Spark .NET 的 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="d3f52-168">We are currently working on automating .NET Core builds for Spark .NET.</span></span> <span data-ttu-id="d3f52-169">在此之前，我们非常感谢你能耐心地手动执行一些步骤。</span><span class="sxs-lookup"><span data-stu-id="d3f52-169">Until then, we appreciate your patience in performing some of the steps manually.</span></span>

  1. <span data-ttu-id="d3f52-170">生成辅助角色：</span><span class="sxs-lookup"><span data-stu-id="d3f52-170">Build the worker:</span></span>

      ```powershell
      cd C:\github\dotnet-spark\src\csharp\Microsoft.Spark.Worker\
      dotnet publish -f netcoreapp3.1 -r win-x64
      ```

      <span data-ttu-id="d3f52-171">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="d3f52-171">Sample console output:</span></span>

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

  2. <span data-ttu-id="d3f52-172">生成示例：</span><span class="sxs-lookup"><span data-stu-id="d3f52-172">Build the samples:</span></span>

      ```powershell
      cd C:\github\dotnet-spark\examples\Microsoft.Spark.CSharp.Examples\
      dotnet publish -f netcoreapp3.1 -r win-x64
      ```

      <span data-ttu-id="d3f52-173">控制台输出示例：</span><span class="sxs-lookup"><span data-stu-id="d3f52-173">Sample console output:</span></span>

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

## <a name="run-the-net-for-spark-sample-applications"></a><span data-ttu-id="d3f52-174">运行 .NET for Spark 示例应用程序</span><span class="sxs-lookup"><span data-stu-id="d3f52-174">Run the .NET for Spark sample applications</span></span>

<span data-ttu-id="d3f52-175">生成示例后，无论你是面向 .NET Framework 还是 .NET Core，都可以通过 `spark-submit` 来运行它们。</span><span class="sxs-lookup"><span data-stu-id="d3f52-175">Once you build the samples, running them will be through `spark-submit` regardless of whether you are targeting .NET Framework or .NET Core.</span></span> <span data-ttu-id="d3f52-176">请确保已按照[先决条件](#prerequisites)部分操作并安装 Apache Spark。</span><span class="sxs-lookup"><span data-stu-id="d3f52-176">Make sure you have followed the [prerequisites](#prerequisites) section and installed Apache Spark.</span></span>

  1. <span data-ttu-id="d3f52-177">设置 `DOTNET_WORKER_DIR` 或 `PATH` 环境变量，使其包含已生成 `Microsoft.Spark.Worker` 二进制文件的路径（例如，在 .NET Framework 中为 C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.Worker\Debug\net461，在 .NET Core 中为 C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\publish） ：</span><span class="sxs-lookup"><span data-stu-id="d3f52-177">Set the `DOTNET_WORKER_DIR` or `PATH` environment variable to include the path where the `Microsoft.Spark.Worker` binary has been generated (for example, *C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.Worker\Debug\net461* for .NET Framework, *C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\publish* for .NET Core):</span></span>

      ```powershell
      set DOTNET_WORKER_DIR=C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.Worker\x64\Debug\netcoreapp3.1\win-x64\publish
      ```
  
  2. <span data-ttu-id="d3f52-178">打开 PowerShell 并转到已生成应用二进制文件的目录（例如，在 .NET Framework 中为 C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461，在 .NET Core 在为 C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\publish） ：</span><span class="sxs-lookup"><span data-stu-id="d3f52-178">Open PowerShell and go to the directory where your app binary has been generated (for example, *C:\github\dotnet\spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\Debug\net461* for .NET Framework, *C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\publish* for .NET Core):</span></span>

      ```powershell
      cd C:\github\dotnet-spark\artifacts\bin\Microsoft.Spark.CSharp.Examples\x64\Debug\netcoreapp3.1\win-x64\publish
      ```

  3. <span data-ttu-id="d3f52-179">运行应用程序的基本结构如下：</span><span class="sxs-lookup"><span data-stu-id="d3f52-179">Running your app follows the basic structure:</span></span>

     ```powershell
     spark-submit.cmd `
       [--jars <any-jars-your-app-is-dependent-on>] `
       --class org.apache.spark.deploy.dotnet.DotnetRunner `
       --master local `
       <path-to-microsoft-spark-jar> `
       <path-to-your-app-exe> <argument(s)-to-your-app>
     ```

     <span data-ttu-id="d3f52-180">下面是可以运行的一些示例：</span><span class="sxs-lookup"><span data-stu-id="d3f52-180">Here are some examples you can run:</span></span>

     - <span data-ttu-id="d3f52-181">[Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs) </span><span class="sxs-lookup"><span data-stu-id="d3f52-181">**[Microsoft.Spark.Examples.Sql.Batch.Basic](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/Basic.cs)**</span></span>

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Batch.Basic %SPARK_HOME%\examples\src\main\resources\people.json
         ```

     - <span data-ttu-id="d3f52-182">[Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs) </span><span class="sxs-lookup"><span data-stu-id="d3f52-182">**[Microsoft.Spark.Examples.Sql.Streaming.StructuredNetworkWordCount](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredNetworkWordCount.cs)**</span></span>

         ```powershell
         spark-submit.cmd `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredNetworkWordCount localhost 9999
         ```

     - <span data-ttu-id="d3f52-183">[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（可访问 maven）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) </span><span class="sxs-lookup"><span data-stu-id="d3f52-183">**[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (maven accessible)](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**</span></span>

         ```powershell
         spark-submit.cmd `
         --packages org.apache.spark:spark-sql-kafka-0-10_2.11:2.3.2 `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
         ```

     - <span data-ttu-id="d3f52-184">[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount（提供 jar）](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs) </span><span class="sxs-lookup"><span data-stu-id="d3f52-184">**[Microsoft.Spark.Examples.Sql.Streaming.StructuredKafkaWordCount (jars provided)](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Streaming/StructuredKafkaWordCount.cs)**</span></span>

         ```powershell
         spark-submit.cmd
         --jars path\to\net.jpountz.lz4\lz4-1.3.0.jar,path\to\org.apache.kafka\kafka-clients-0.10.0.1.jar,path\to\org.apache.spark\spark-sql-kafka-0-10_2.11-2.3.2.jar,`path\to\org.slf4j\slf4j-api-1.7.6.jar,path\to\org.spark-project.spark\unused-1.0.0.jar,path\to\org.xerial.snappy\snappy-java-1.1.2.6.jar `
         --class org.apache.spark.deploy.dotnet.DotnetRunner `
         --master local `
         C:\github\dotnet-spark\src\scala\microsoft-spark-<version>\target\microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar `
         Microsoft.Spark.CSharp.Examples.exe Sql.Streaming.StructuredKafkaWordCount localhost:9092 subscribe test
          ```
