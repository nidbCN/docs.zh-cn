---
title: 部署 .NET for Apache Spark 辅助角色和用户定义的函数二进制文件
description: 了解如何部署 .NET for Apache Spark 辅助角色和用户定义的函数二进制文件。
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: d3e86d22a308c7124812f2c11742c2e18803b13d
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875533"
---
# <a name="deploy-net-for-apache-spark-worker-and-user-defined-function-binaries"></a>部署 .NET for Apache Spark 辅助角色和用户定义的函数二进制文件

本操作说明提供有关如何部署 .NET for Apache Spark 辅助角色和用户定义的函数二进制文件的常规说明。 你将了解要设置的环境变量，还将了解通过 `spark-submit` 启动应用程序的一些常用参数。

## <a name="configurations"></a>配置

配置显示常规环境变量和参数设置，用于部署 .NET for Apache Spark 辅助角色和用户定义的函数二进制文件。

### <a name="environment-variables"></a>环境变量

部署辅助角色和编写 UDF 时，可能需要设置几个常用环境变量：

| 环境变量         | 描述
| :--------------------------- | :----------
| DOTNET_WORKER_DIR            | 生成 <code>Microsoft.Spark.Worker</code> 二进制文件的路径</br>它由 Spark 驱动程序使用，将被传递到 Spark 执行程序。 如果未设置此变量，Spark 执行器将搜索 <code>PATH</code> 环境变量中指定的路径。</br>例如  “C:\bin\Microsoft.Spark.Worker”
| DOTNET_ASSEMBLY_SEARCH_PATHS | 逗号分隔的路径，<code>Microsoft.Spark.Worker</code> 将在这些路径中加载程序集。</br>请注意，如果路径以“.”开头，则将预置工作目录。 如果处于 yarn 模式，  则“.”表示容器的工作目录。</br>例如  “C:\Users\\&lt;用户名&gt;\\&lt;mysparkapp&gt;\bin\Debug\\&lt;dotnet 版本&gt;”
| DOTNET_WORKER_DEBUG          | 如果要<a href="https://github.com/dotnet/spark/blob/main/docs/developer-guide.md#debugging-user-defined-function-udf">调试 UDF</a>，请在运行 <code>spark-submit</code> 之前将此环境变量设置为 <code>1</code>。

### <a name="parameter-options"></a>参数选项

[捆绑](https://spark.apache.org/docs/latest/submitting-applications.html#bundling-your-applications-dependencies) Spark 应用程序后，可以使用 `spark-submit` 启动它。 下表显示了一些常用选项：

| 参数名称        | 描述
| :---------------------| :----------
| --class               | 应用程序的入口点。</br> 例如 org.apache.spark.deploy.dotnet.DotnetRunner
| --master              | 群集的<a href="https://spark.apache.org/docs/latest/submitting-applications.html#master-urls">母版 URL</a>。</br> 例如 yarn
| --deploy-mode         | 是在辅助节点 (<code>cluster</code>) 上还是在本地作为外部客户端 (<code>client</code>) 部署驱动程序。</br>默认值：<code>client</code>
| --conf                | <code>key=value</code> 格式的任意 Spark 配置属性。</br> 例如 spark.yarn.appMasterEnv.DOTNET_WORKER_DIR=.\worker\Microsoft.Spark.Worker
| --files               | 要放入每个执行程序的工作目录中的文件的逗号分隔列表。<br/><ul><li>请注意，此选项仅适用于 yarn 模式。</li><li>它支持通过 # 指定文件名，与 Hadoop 类似。</br></ul>例如 <code>myLocalSparkApp.dll#appSeen.dll</code>。  在 YARN 上运行时，应用程序应使用如 <code>appSeen.dll</code> 的名称引用 <code>myLocalSparkApp.dll</code>。</li>
| --archives          | 要提取到每个执行程序的工作目录中的存档的逗号分隔列表。</br><ul><li>请注意，此选项仅适用于 yarn 模式。</li><li>它支持通过 # 指定文件名，与 Hadoop 类似。</br></ul>例如 <code>hdfs://&lt;path to your worker file&gt;/Microsoft.Spark.Worker.zip#worker</code>。  这会将 zip 文件复制并提取到 <code>worker</code> 文件夹。</li>
| application-jar       | 指向包含应用程序和所有依赖项的捆绑 jar 的路径。</br>例如 hdfs://&lt;jar 路径&gt;/microsoft-spark-&lt;版本&gt;.jar 
| application-arguments | 传递给主类 main 方法的参数（如果有）。</br>例如 hdfs://&lt;应用路径&gt;/&lt;应用&gt;.zip &lt;应用名称&gt; &lt;应用参数&gt; 

> [!NOTE]
> 在 `application-jar` 通过 `spark-submit` 启动应用程序之前指定所有 `--options`，否则将忽略它们。 有关详细信息，请参阅 [`spark-submit` 选项](https://spark.apache.org/docs/latest/submitting-applications.html)和[在 YARN 上运行 Spark 详解](https://spark.apache.org/docs/latest/running-on-yarn.html)。

## <a name="frequently-asked-questions"></a>常见问题

### <a name="when-i-run-a-spark-app-with-udfs-i-get-a-filenotfoundexception-error-what-should-i-do"></a>通过 UDF 运行 Spark 应用时，出现“FileNotFoundException”错误。 应采取何种操作？

>  错误：[Error] [TaskRunner] [0] ProcessStream() 失败，出现异常：System.IO.FileNotFoundException:Assembly 'mySparkApp, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null' 找不到文件: 'mySparkApp.dll'

**答：** 检查是否已正确设置 `DOTNET_ASSEMBLY_SEARCH_PATHS` 环境变量。 它应是包含 `mySparkApp.dll` 的路径。

### <a name="after-i-upgraded-my-net-for-apache-spark-version-and-reset-the-dotnet_worker_dir-environment-variable-why-do-i-still-get-the-following-ioexception-error"></a>升级 .NET for Apache Spark 版本并重置 `DOTNET_WORKER_DIR` 环境变量后，为什么仍会出现以下 `IOException` 错误？

> 错误：  在阶段 11.0 丢失任务0.0（TID 24、localhost、执行程序驱动程序）：java.io.IOException:无法运行程序 "Microsoft.Spark.Worker.exe":CreateProcess error=2，系统找不到指定的文件。

**答：** 首先尝试重启 PowerShell 窗口（或其他命令窗口），使其可以采用最新的环境变量值。 然后启动程序。

### <a name="after-submitting-my-spark-application-i-get-the-error-systemtypeloadexception-could-not-load-type-systemruntimeremotingcontextscontext"></a>提交 Spark 应用程序后，出现 `System.TypeLoadException: Could not load type 'System.Runtime.Remoting.Contexts.Context'` 错误。

>  错误：[Error] [TaskRunner] [0] ProcessStream() 失败，出现异常：System.TypeLoadException 异常:未能从程序集 'mscorlib, Version=4.0.0.0, Culture=neutral, PublicKeyToken=...' 加载类型 'System.Runtime.Remoting.Contexts.Context'。

**答：** 检查你使用的 `Microsoft.Spark.Worker` 版本。 有两个版本：.NET Framework 4.6.1 和 .NET Core 3.1.x 。 在本例中，应使用 `Microsoft.Spark.Worker.net461.win-x64-<version>`（可以[下载](https://github.com/dotnet/spark/releases)），因为 `System.Runtime.Remoting.Contexts.Context` 仅适用于 .NET Framework。

### <a name="how-do-i-run-my-spark-application-with-udfs-on-yarn-which-environment-variables-and-parameters-should-i-use"></a>如何在 YARN 上通过 UDF 运行 Spark 应用程序？ 应该使用哪些环境变量和参数？

**答：** 若要在 YARN 上启动 Spark 应用程序，应将环境变量指定为 `spark.yarn.appMasterEnv.[EnvironmentVariableName]`。 请参阅以下内容，作为使用 `spark-submit` 的示例：

```powershell
spark-submit \
--class org.apache.spark.deploy.dotnet.DotnetRunner \
--master yarn \
--deploy-mode cluster \
--conf spark.yarn.appMasterEnv.DOTNET_WORKER_DIR=./worker/Microsoft.Spark.Worker-<version> \
--conf spark.yarn.appMasterEnv.DOTNET_ASSEMBLY_SEARCH_PATHS=./udfs \
--archives hdfs://<path to your files>/Microsoft.Spark.Worker.net461.win-x64-<version>.zip#worker,hdfs://<path to your files>/mySparkApp.zip#udfs \
hdfs://<path to jar file>/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
hdfs://<path to your files>/mySparkApp.zip mySparkApp
```

## <a name="next-steps"></a>后续步骤

* [.NET for Apache Spark 入门](../tutorials/get-started.md)
* [在 Windows 上调试 .NET for Apache Spark 应用程序](debug.md)
