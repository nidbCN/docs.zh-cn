---
title: 将 .NET for Apache Spark 应用程序部署到 Databricks
description: 了解如何将 .NET for Apache Spark 应用程序部署到 Databricks。
ms.date: 10/09/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: e751953937279a5f9f78f777bac8a5ca510a2f87
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876963"
---
# <a name="tutorial-deploy-a-net-for-apache-spark-application-to-databricks"></a>教程：将 .NET for Apache Spark 应用程序部署到 Databricks

本教程介绍如何使用 Azure Databricks 将应用部署到云。Azure Databricks 是一种基于 Apache Spark 的分析平台，包含一键设置、经过简化的工作流以及支持协作的交互式工作区。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 创建 Azure Databricks 工作区。
> - 发布 .NET for Apache Spark 应用。
> - 创建 Spark 作业和 Spark 群集。
> - 在 Spark 群集上运行应用。

## <a name="prerequisites"></a>先决条件

开始之前，请完成以下任务：

* 如果没有 Azure 帐户，请创建一个[免费帐户](https://azure.microsoft.com/free/dotnet/)。
* 登录 [Azure 门户](https://portal.azure.com/)。
* 完成 [.NET for Apache Spark - 10 分钟入门](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)教程。

## <a name="create-an-azure-databricks-workspace"></a>创建 Azure Databricks 工作区

> [!Note]
> 不能使用 Azure 免费试用订阅完成本教程。
> 如果你有免费帐户，请转到个人资料并将订阅更改为“即用即付”。 有关详细信息，请参阅 [Azure 免费帐户](https://azure.microsoft.com/free/dotnet/)。 然后，[移除支出限制](/azure/billing/billing-spending-limit#why-you-might-want-to-remove-the-spending-limit)，并为你所在区域的 vCPU [请求增加配额](/azure/azure-supportability/resource-manager-core-quotas-request)。 创建 Azure Databricks 工作区时，可以选择“试用版(高级 - 14天免费 DBU)”定价层，让工作区访问免费的高级 Azure Databricks DBU 14 天。

在本部分，使用 Azure 门户创建 Azure Databricks 工作区。

1. 在 Azure 门户中，选择“创建资源” > “分析” > “Azure Databricks”。

   ![在 Azure 门户中创建 Azure Databricks 资源](./media/databricks-deployment/create-databricks-resource.png)

2. 在“Azure Databricks 服务”下，提供所需的值以创建 Databricks 工作区。

    |Property  |说明  |
    |---------|---------|
    |**工作区名称**     | 为 Databricks 工作区提供一个名称。        |
    |**订阅**     | 从下拉列表中选择自己的 Azure 订阅。        |
    |**资源组**     | 指定是要创建新的资源组还是使用现有的资源组。 资源组是用于保存 Azure 解决方案相关资源的容器。 有关详细信息，请参阅 [Azure 资源组概述](/azure/azure-resource-manager/resource-group-overview)。 |
    |**位置**     | 选择首选区域。 有关可用区域的信息，请参阅[各区域推出的 Azure 服务](https://azure.microsoft.com/regions/services/)。        |
    |**定价层**     |  在“标准”、“高级”和“试用”之间进行选择。   有关这些层的详细信息，请参阅 [Databricks 价格页](https://azure.microsoft.com/pricing/details/databricks/)。       |
    |**虚拟网络**     |   否       |

3. 选择“创建”。 创建工作区需要几分钟时间。 创建工作区时，可以在“通知”中查看部署状态。

## <a name="install-azure-databricks-tools"></a>安装 Azure Databricks 工具

可使用 Databricks CLI 连接到 Azure Databricks 群集，然后将文件从本地计算机上传到这些群集。 Databricks 群集通过 DBFS（Databricks 文件系统）访问文件。

1. Databricks CLI 需要使用 Python 3.6 或更高版本。 如果已安装 Python，则可以跳过此步骤。

   **对于 Windows：**

   [下载适用于 Windows 的 Python](https://www.python.org/ftp/python/3.7.4/python-3.7.4.exe)

   **对于 Linux：** 大多数 Linux 发行版上都预安装了 Python。 要查看已安装的版本，请运行以下命令：

   ```bash
   python3 --version
   ```

2. 使用 pip 安装 Databricks CLI。 Python 3.4 及更高版本默认包含 pip。 使用适用于 Python 3 的 pip3。 运行下面的命令：

   ```bash
   pip3 install databricks-cli
   ```

3. 安装 Databricks CLI 后，打开一个新的命令提示符并运行命令 `databricks`。 如果收到“[databricks] 未被识别为内部或外部命令错误”，请确认是否打开了新的命令提示符。

## <a name="set-up-azure-databricks"></a>设置 Azure Databricks

Databricks CLI 安装完毕之后，接下来需要设置身份验证详细信息。

1. 运行 Databricks CLI 命令 `databricks configure --token`。

2. 运行配置命令后，系统会提示你进入主机。 你的主机 URL 使用的格式为 `https://<Location>.azuredatabricks.net`。 例如，如果在创建 Azure Databricks 服务期间选择了“eastus2”，则主机将为 `https://eastus2.azuredatabricks.net`。

3. 进入主机之后，系统会提示你输入令牌。 在 Azure 门户中，选择“启动工作区”以启动 Azure Databricks 工作区。

   ![启动 Azure Databricks 工作区](./media/databricks-deployment/launch-databricks-workspace.png)

4. 在工作区主页上，选择“用户设置”。

   ![Azure Databricks 工作区中的“用户设置”](./media/databricks-deployment/databricks-user-settings.png)

5. 在“用户设置”页上，可以生成新的令牌。 复制生成的令牌，并将其粘贴回命令提示符。

   ![在 Azure Databricks 工作区中生成新的访问令牌](./media/databricks-deployment/generate-token.png)

现在，应可以访问创建的任何 Azure Databricks 群集，并将文件上传到 DBFS。

## <a name="download-worker-dependencies"></a>下载辅助角色依赖项

> [!Note]
> Azure 和 AWS Databricks 都基于 Linux。 因此，如果要将应用部署到 Databricks，请确保应用与 .NET Standard 兼容，并且使用 .NET Core 编译器编译应用。

1. Microsoft.Spark.Worker 可帮助 Apache Spark 执行你的应用，例如你可能已编写的任何用户定义函数 (UDF)。 下载 [Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases/download/v1.0.0/Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz)。

2. install-worker.sh 是一个脚本，可使用该脚本将 .NET for Apache Spark 依赖项文件复制到群集的节点中。

   在本地计算机上创建一个名为 install-worker.sh 的新文件，并粘贴位于 GitHub 上的 [install-worker.sh 内容](https://raw.githubusercontent.com/dotnet/spark/main/deployment/install-worker.sh)。

3. db-init.sh 是用于将依赖项安装到 Databricks Spark 群集上的脚本。

   在本地计算机上创建一个名为 db-init.sh 的新文件，并粘贴位于 GitHub 上的 [db-init.sh 内容](https://github.com/dotnet/spark/blob/main/deployment/db-init.sh)。

   在刚刚创建的文件中，将 `DOTNET_SPARK_RELEASE` 变量设置为 `https://github.com/dotnet/spark/releases/download/v1.0.0/Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz`。 按原样保留 db-init.sh 文件的其余部分。

> [!Note]
> 如果使用 Windows，请验证 install-worker.sh 中的行尾和 db-init.sh 脚本是否为 Unix 样式 (LF) 。 可通过文本编辑器（例如，记事本++ 和 Atom）更改行尾。

## <a name="publish-your-app"></a>发布你的应用

接下来，发布在 [.NET for Apache Spark - 10 分钟入门](https://dotnet.microsoft.com/learn/data/spark-tutorial/intro)教程中创建的 mySparkApp，以确保 Spark 群集可以访问运行应用所需的所有文件。

1. 运行以下命令以发布 mySparkApp：

   ```dotnetcli
   cd mySparkApp
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

2. 执行以下任务以压缩已发布的应用文件，以便你可以轻松地将其上传到 Databricks Spark 群集。

   在 Windows 上：

   导航到 mySparkApp/bin/Release/netcoreapp3.1/ubuntu.16.04-x64。 然后，右键单击“发布”文件夹，再选择“发送到”>“压缩文件夹” 。 将新文件夹命名为“publish.zip”。

   **在 Linux 上，运行以下命令：**

   ```bash
   zip -r publish.zip .
   ```

## <a name="upload-files"></a>上传文件

在本部分中，将多个文件上传到 DBFS，以便群集拥有在云中运行应用所需的一切。 每次将文件上传到 DBFS 时，请确保你位于该文件所在的计算机目录中。

1. 运行以下命令，以将 db-init.sh、install-worker.sh 和 Microsoft.Spark.Worker 上传到 DBFS  ：

   ```console
   databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
   databricks fs cp install-worker.sh dbfs:/spark-dotnet/install-worker.sh
   databricks fs cp Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz dbfs:/spark-dotnet/Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz
   ```

2. 运行以下命令以上传群集运行应用所需的其余文件：已压缩的发布文件夹、input.txt 和 microsoft-spark-2-4_2.11-1.0.0.jar 。

   ```console
   cd mySparkApp
   databricks fs cp input.txt dbfs:/input.txt

   cd mySparkApp\bin\Release\netcoreapp3.1\ubuntu.16.04-x64 directory
   databricks fs cp publish.zip dbfs:/spark-dotnet/publish.zip
   databricks fs cp microsoft-spark-2-4_2.11-1.0.0.jar dbfs:/spark-dotnet/microsoft-spark-2-4_2.11-1.0.0.jar
   ```

## <a name="create-a-job"></a>创建作业

应用通过运行 spark-submit（用于运行 .NET for Apache Spark 作业的命令）的作业在 Azure Databricks 上运行。

1. 在 Azure Databricks 工作区中，选择“作业”图标，然后选择“+ 创建作业” 。

   ![创建 Azure Databricks 作业](./media/databricks-deployment/create-job.png)

2. 选择作业的标题，然后选择“配置 spark-submit”。

   ![为 Databricks 作业配置 spark-submit](./media/databricks-deployment/configure-spark-submit.png)

3. 在作业配置中粘贴以下参数。 然后，选择“确认”。

   ```
   ["--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/spark-dotnet/microsoft-spark-2-4_2.11-1.0.0.jar","/dbfs/spark-dotnet/publish.zip","mySparkApp"]
   ```

## <a name="create-a-cluster"></a>创建群集

1. 导航到作业并选择“编辑”以配置作业的群集。

2. 将群集设置为“Spark 2.4.1”。 然后，选择“高级选项” > “Init 脚本” 。 将 Init 脚本路径设置为 `dbfs:/spark-dotnet/db-init.sh`。

   ![在 Azure Databricks 中配置 Spark 群集](./media/databricks-deployment/cluster-config.png)

3. 选择“确认”以确认群集设置。

## <a name="run-your-app"></a>运行你的应用

1. 导航到作业并选择“立即运行”，在新配置的 Spark 群集上运行作业。

2. 创建作业的群集需要几分钟时间。 一旦创建完成，你的作业就会被提交，你可以查看输出。

3. 从左侧菜单中选择“群集”，然后选择“名称”并运行作业。

4. 选择“驱动程序日志”以查看作业的输出。 应用执行完毕后，你将在写入标准输出控制台的入门本地运行中看到相同的字数表。

   ![Azure Databricks 作业输出表](./media/databricks-deployment/table-output.png)

   恭喜，你已在 Azure Databricks 中运行了第一个 .NET for Apache Spark 应用程序！

## <a name="clean-up-resources"></a>清理资源

如果不再需要 Databricks 工作区，可在 Azure 门户中删除 Azure Databricks 资源。 还可以选择资源组名称来打开“资源组”页，然后选择“删除资源组”。

## <a name="next-steps"></a>后续步骤

在本教程中，你已将 .NET for Apache Spark 应用程序部署到 Databricks。 要了解有关 Databricks 的详细信息，请继续阅读 Azure Databricks 文档。

> [!div class="nextstepaction"]
> [Azure Databricks 文档](/azure/azure-databricks/)
