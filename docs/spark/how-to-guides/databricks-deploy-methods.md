---
title: 将 .NET for Apache Spark 作业提交到 Databricks
description: 了解如何使用 spark-submit 和 Set Jar 将 .NET for Apache Spark 作业提交到 Databricks。
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: 39be961ad67da3f8593cb98e1bad8df354f28893
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875572"
---
# <a name="submit-a-net-for-apache-spark-job-to-databricks"></a>将 .NET for Apache Spark 作业提交到 Databricks

可以在 Databricks 群集上运行 .NET for Apache Spark 作业，但它并不是现成可用的。 可通过两种方法将 .NET for Apache Spark 作业部署到 Databricks：`spark-submit` 和 Set Jar。

## <a name="deploy-using-spark-submit"></a>使用 spark-submit 进行部署

可以使用 [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) 命令将 .NET for Apache Spark 作业提交到 Databricks。 `spark-submit` 允许仅提交到按需创建的群集。

1. 导航到 Databricks 工作区并创建一个作业。 选择作业的标题，然后选择“配置 spark-submit”。 在作业配置中粘贴以下参数，然后选择“确认”。

    ```
    ["--files","/dbfs/<path-to>/<app assembly/file to deploy to worker>","--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/<path-to>/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar","/dbfs/<path-to>/<app name>.zip","<app bin name>","app arg1","app arg2"]
    ```

    > [!NOTE]
    > 根据特定文件和配置更新上述参数的内容。 例如，引用已上传到 DBFS 的 Microsoft.Spark jar 文件的版本，并使用适当的应用名称和已发布的应用 zip 文件。

2. 导航到作业并选择“编辑”以配置作业的群集。 根据想要在部署中使用的 Apache Spark 版本设置 Databricks Runtime 版本。 然后选择“高级选项”>“Init 脚本”，并将“Init 脚本路径”设置为 `dbfs:/spark-dotnet/db-init.sh`。 选择“确认”以确认群集设置。

3. 导航到作业并选择“立即运行”，在新配置的 Spark 群集上运行作业。 创建作业的群集需要几分钟时间。 一旦创建完成，你的作业就会提交。 可以通过从 Databricks 工作区的左侧菜单中选择“群集”来查看输出，然后选择“驱动程序日志” 。

## <a name="deploy-using-set-jar"></a>使用 Set Jar 进行部署

或者，可以在 Databricks 工作区中使用 [Set Jar](/azure/databricks/jobs#--create-a-job) 将 .NET for Apache Spark 作业提交到 Databricks。 通过 Set Jar，可将作业提交到现有的有效群集。

### <a name="one-time-setup"></a>一次性设置

1. 导航到 Databricks 群集并选择左侧菜单的“作业”，后跟 Set JAR 。

2. 上传相应的 `microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar`。

3. 修改以下参数，使其包含为替代 `<your-app-name>` 而发布的可执行文件的正确名称：

    ```
    Main Class: org.apache.spark.deploy.dotnet.DotnetRunner
    Arguments /dbfs/apps/<your-app-name>.zip <your-app-name>
    ```

4. 将群集配置为指向已设置 init 脚本的现有群集。

### <a name="publish-and-run-your-app"></a>发布并运行应用

1. 请确保已发布应用，并且应用程序代码不使用 `SparkSession.Stop()`。

2. 使用 [Databricks CLI](/azure/databricks/dev-tools/databricks-cli) 将应用程序上传到 Databricks 群集。 例如，使用以下命令将已发布的应用上传到群集：

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <your-app-name>.zip dbfs:/apps/<your-app-name>.zip
    ```

3. 如果应用程序集（例如包含用户定义的函数及其依赖项的 DLL）中具有任何用户定义的函数，则需要放置在每个 Microsoft.Spark.Worker 的工作目录中。

    将应用程序集上传到 Databricks 群集：

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <assembly>.dll dbfs:/apps/dependencies
    ```

    取消评论并将 [db-init.sh](https://github.com/dotnet/spark/blob/main/deployment/db-init.sh) 中的应用依赖项部分修改为指向应用依赖项路径。 然后，将已更新的 db-init.sh 上传到群集：

    ```console
    cd <path-to-db-init-and-install-worker>
    databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
    ```

4. 导航到“Databricks 群集”>“作业”>“[作业名称]”>“立即运行”以运行作业。

## <a name="next-steps"></a>后续步骤

* [.NET for Apache Spark 入门](../tutorials/get-started.md)
* [将 .NET for Apache Spark 应用程序部署到 Databricks](../tutorials/databricks-deployment.md)
* [Azure Databricks 文档](/azure/azure-databricks/)
