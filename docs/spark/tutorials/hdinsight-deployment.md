---
title: 将 .NET for Apache Spark 应用程序部署到 Azure HDInsight
description: 了解如何将 .NET for Apache Spark 应用程序部署到 HDInsight。
ms.date: 10/09/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 0c2b7522e0e6790ded418413ef0f128aae1e0884
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875338"
---
# <a name="tutorial-deploy-a-net-for-apache-spark-application-to-azure-hdinsight"></a>教程：将 .NET for Apache Spark 应用程序部署到 Azure HDInsight

本教程介绍如何通过 Azure HDInsight 群集将 .NET for Apache Spark 应用部署到云中。 由于 HDInsight 中的 Spark 群集与 Azure 存储和 Azure Data Lake Storage 兼容，因此使用 HDInsight，你可以更加容易地在 Azure 中创建和配置 Spark 群集。

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 使用 Azure 存储资源管理器访问存储帐户。
> * 创建 Azure HDInsight 群集。
> * 发布 .NET for Apache Spark 应用。
> * 创建并运行 HDInsight 脚本操作。
> * 在 HDInsight 群集上运行 .NET for Apache Spark 应用。

## <a name="prerequisites"></a>先决条件

开始之前，请完成以下任务：

* 如果还没有 Azure 订阅，可以创建一个[免费帐户](https://azure.microsoft.com/free/dotnet/)。
* 登录 [Azure 门户](https://portal.azure.com/)。
* 在 [Windows](https://go.microsoft.com/fwlink/?LinkId=708343&clcid=0x409)、[Linux](https://go.microsoft.com/fwlink/?LinkId=722418&clcid=0x409) 或 [MacOS](https://go.microsoft.com/fwlink/?LinkId=708342&clcid=0x409) 计算机上安装 Azure 存储资源管理器。
* 完成 [.NET for Apache Spark - 10 分钟入门](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)教程。

## <a name="access-your-storage-accounts"></a>访问存储帐户

1. 打开 Azure 存储资源管理器。

2. 在左侧菜单上选择“添加帐户”，并登录到 Azure 帐户  。

    ![从存储资源管理器登录 Azure 帐户](./media/hdinsight-deployment/signin-azure-storage-explorer.png)

   登录后，应会看到你所拥有的所有存储帐户以及上传到存储帐户的所有资源。

## <a name="create-an-hdinsight-cluster"></a>创建 HDInsight 群集

> [!IMPORTANT]
> HDInsight 群集基于分钟按比例收费，即使你未使用它们也是如此。 请务必在使用完之后删除群集。 有关详细信息，请参阅本教程的[清理资源](#clean-up-resources)部分。

1. 访问 [Azure 门户](https://portal.azure.com)。

2. 选择“+ 创建资源”。  然后，从“Analytics”类别中选择“HDInsight”   。

    ![从 Azure 门户创建 HDInsight 资源](./media/hdinsight-deployment/create-hdinsight-resource.png)

3. 在“基本”下，提供以下值  ：

    |Property  |说明  |
    |---------|---------|
    |订阅  | 从下拉列表中选择一个可用 Azure 订阅。 |
    |资源组 | 指定是要创建新的资源组还是使用现有的资源组。 资源组是用于保存 Azure 解决方案相关资源的容器。 |
    |群集名称 | 为 HDInsight Spark 群集命名。|
    |位置   | 选择资源组的位置。 模板将此位置用于创建群集，以及用于默认群集存储。 |
    |群集类型| 选择“Spark”作为群集类型。 |
    |群集版本|选择群集类型后，此字段中将自动填充默认版本。 选择 2.3 或 2.4 版本的 Spark。|
    |群集登录用户名| 输入群集登录用户名。  默认名称为 *admin*。 |
    |群集登录密码| 输入任何登录密码。 |
    |安全外壳 (SSH) 用户名| 输入 SSH 用户名。 默认情况下，此帐户的密码与群集登录用户名帐户的密码相同  。 |

4. 在完成时选择“下一步:  存储 >>”转到“存储”页  。 在“存储”下，提供以下值  ：

    |属性  |说明  |
    |---------|---------|
    |主存储类型|使用默认值“Azure 存储”。 |
    |选择方法|使用默认值“从列表中选择”。 |
    |主存储帐户|选择订阅以及该订阅中的可用存储帐户。|
    |容器|此容器是存储帐户中特定的 blob 容器，群集在该容器中查找文件以在云中运行应用。 可使用任何可用名称为其命名。|

5. 在“查看 + 创建”下，选择“创建”。   创建群集大约需要 20 分钟时间。 必须先创建群集，才能继续执行下一步。

## <a name="publish-your-app"></a>发布你的应用

接下来，发布在 [.NET for Apache Spark - 10 分钟入门](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)教程中创建的 mySparkApp，以便 Spark 群集可以访问运行应用所需的所有文件  。

1. 运行以下命令以发布 mySparkApp  ：

   在 Windows 上： 

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r win-x64
   ```

   **在 Linux 上：**

   ```bash
   cd mySparkApp
   foo@bar:~/path/to/app$ dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

2. 执行以下任务以压缩已发布的应用文件，以便你可以轻松地将其上传到 HDInsight 群集。 压缩发布文件夹（如 publish.zip）的内容，该文件夹是在步骤 1 中创建的。 所有程序集都应位于 ZIP 文件的第一层，并且不应有中间文件夹层。 这意味着，当你解压缩 publish.zip 时，所有程序集都将被提取到当前工作目录中。

   在 Windows 上：

   使用提取程序（如 7-Zip 或 WinZip）将文件与所有已发布的二进制文件一起解压缩到 bin 目录中。

   **在 Linux 上，运行以下命令：**

   ```bash
   zip -r publish.zip
   ```

## <a name="upload-files-to-azure"></a>将文件上传到 Azure

接下来，使用 Azure 存储资源管理器将以下五个文件上传到为群集存储选择的 blob 容器中：

* Microsoft.Spark.Worker
* install-worker.sh
* publish.zip
* microsoft-spark-2-4_2.11-1.0.0.jar
* input.txt

1. 打开 Azure 存储资源管理器，然后从左侧菜单导航到存储帐户。 在存储帐户中的“Blob 容器”下，向下钻取到群集的 blob 容器。

2. Microsoft.Spark.Worker 可帮助 Apache Spark 执行你的应用，例如你可能已编写的任何用户定义函数 (UDF)。 下载 [Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases/download/v1.0.0/Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz)。 然后，在 Azure 存储资源管理器中选择“上传”以上传辅助角色。

   ![将文件上传到 Azure 存储资源管理器](./media/hdinsight-deployment/upload-files-to-storage.png)

3. install-worker.sh 是一个脚本，可使用该脚本将 .NET for Apache Spark 依赖项文件复制到群集的节点中。

   在本地计算机上创建一个名为 install-worker.sh 的新文件，并粘贴位于 GitHub 上的 [install-worker.sh 内容](https://raw.githubusercontent.com/dotnet/spark/main/deployment/install-worker.sh)。 然后，将 install-worker.sh 上传到 blob 容器中。

4. 群集需要 publish.zip 文件，后者包含应用的已发布文件。 导航到已发布文件夹“mySparkApp/bin/Release/netcoreapp3.1/ubuntu.16.04-x64”，并找到“publish.zip” 。 然后，将 publish.zip 上传到 blob 容器。

5. 群集需要打包为 jar 的应用程序代码，作为 [Microsoft.Spark](https://www.nuget.org/packages/Microsoft.Spark/) nuget 的一部分包含在内，并置在应用的生成输出目录中。 导航到已发布文件夹“mySparkApp/bin/Release/netcoreapp3.1/ubuntu.16.04-x64”，并找到“microsoft-spark-2-4_2.11-1.0.0.jar” 。 然后，将 jar 文件上传到 blob 容器。

   对于 2.3.x、2.4.x 和 3.0.x 版的 Spark，可能存在多个 .jar 文件。 你需要选择与在群集创建期间选择的 Spark 版本相匹配的 .jar 文件。 例如，如果在群集创建期间选择了 Spark 2.4，请选择 microsoft-spark-2-4_2.11-1.0.0.jar。

6. 群集需要应用的输入。 导航到 mySparkApp 目录，并找到“input.txt”。 将输入文件上传到 blob 容器中的 user/sshuser 目录。 你将通过 ssh 连接到群集，群集将在此文件夹中查找其输入。 input.txt 文件是上传到特定目录的唯一文件。

## <a name="run-the-hdinsight-script-action"></a>运行 HDInsight 脚本操作

群集运行并将文件上传到 Azure 后，可在群集上运行 install-worker.sh 脚本。

1. 导航到 Azure 门户中的 HDInsight Spark 群集，然后选择“脚本操作”。

2. 选择“+ 提交新脚本”并提供以下值：

   |属性  |描述  |
   |---------|---------|
   | 脚本类型 |自定义|
   | “属性” | 安装辅助角色|
   | Bash 脚本 URI |`https://mystorageaccount.blob.core.windows.net/mycontainer/install-worker.sh` </br> 要确认此 URI，请在 Azure 存储资源管理器中右键单击“install-worker.sh”，然后选择“属性”。 |
   | 节点类型| 辅助角色|
   | 参数 | azure </br> wasbs://mycontainer@myStorageAccount.blob.core.windows.net/Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz </br> /usr/local/bin

3. 选择“创建”，提交脚本。

## <a name="run-your-app"></a>运行应用

1. 导航到 Azure 门户中的 HDInsight Spark 群集，然后选择“SSH + 群集登录名”。

2. 复制 ssh 登录信息，并将登录名粘贴到终端。 使用在群集创建期间设置的密码登录到群集。 应会看到“欢迎使用 Ubuntu 和 Spark”的消息。

3. 使用 spark-submit 命令在 HDInsight 群集上运行应用  。 请记得将示例脚本中的 mycontainer 和 mystorageaccount 替换为 blob 容器和存储帐户的实际名称。

   ```bash
   $SPARK_HOME/bin/spark-submit \
   --master yarn \
   --class org.apache.spark.deploy.dotnet.DotnetRunner \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/microsoft-spark-2-4_2.11-1.0.0.jar \
   wasbs://mycontainer@mystorageaccount.blob.core.windows.net/publish.zip mySparkApp
   ```

   应用运行时，你将在写入控制台的“本地运行入门”中看到相同的字数表。 恭喜，你已在云中运行了第一个 .NET for Apache Spark 应用程序！

## <a name="clean-up-resources"></a>清理资源

HDInsight 将数据保存在 Azure 存储中，因此可以在群集不用时安全地删除群集。 此外，还需要支付 HDInsight 群集费用，即使未使用。 由于群集费用高于存储空间费用数倍，因此在不使用群集时将其删除可以节省费用。

还可以选择资源组名称来打开“资源组”页，然后选择“删除资源组”。 通过删除资源组，可以删除 HDInsight Spark 群集和默认存储帐户。

## <a name="next-steps"></a>后续步骤

在本教程中，你已将 .NET for Apache Spark 应用程序部署到 Azure HDInsight。 要了解有关 HDInsight 的详细信息，请继续阅读 Azure HDInsight 文档。

> [!div class="nextstepaction"]
> [在 Azure HDInsight 上远程提交作业](../how-to-guides/hdinsight-deploy-methods.md)
> [Azure HDInsight 文档](/azure/hdinsight/)
