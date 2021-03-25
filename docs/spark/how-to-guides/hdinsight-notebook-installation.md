---
title: 在 Azure HDInsight Spark 群集的 Jupyter Notebook 中安装 .NET for Apache Spark
description: 了解如何在 Azure HDInsight 的 Jupyter Notebook 中安装 .NET for Apache Spark。
ms.date: 10/09/2020
ms.topic: conceptual
ms.custom: mvc,how-to
ms.openlocfilehash: b84d61c29d2b2aa7a9fee20a8af9f3eee23f7e8b
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605472"
---
# <a name="install-net-for-apache-spark-on-jupyter-notebooks-on-azure-hdinsight-spark-clusters"></a>在 Azure HDInsight Spark 群集的 Jupyter Notebook 中安装 .NET for Apache Spark

本文介绍如何在 Azure HDInsight Spark 群集的 Jupyter Notebook 中安装 .NET for Apache Spark。 可以通过搭配使用命令行和 Azure 门户在 Azure HDInsight 群集中部署 .NET for Apache Spark（有关详细信息，请参阅[如何将 .NET for Apache Spark 应用程序部署到 Azure HDInsight](../tutorials/hdinsight-deployment.md)），但笔记本可提供更好的交互和迭代体验。

Azure HDInsight 群集已附带 Jupyter Notebook，因此只需将 Jupyter Notebook 配置为运行 .NET for Apache Spark。 若要在 Jupyter Notebook 中使用 .NET for Apache Spark，需要提供 C# REPL，以逐行执行 C# 代码并在必要时保留执行状态。 [Try .NET](https://github.com/dotnet/try) 已集成为官方 .NET REPL。

若要通过 Jupyter Notebook 体验启用 .NET for Apache Spark，需要通过 [Ambari](/azure/hdinsight/hdinsight-hadoop-manage-ambari) 完成一些手动步骤，然后从 HDInsight Spark 群集提交[脚本操作](/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)。

> [!NOTE]
> 此功能是试验功能，HDInsight Spark 团队不支持它。 

## <a name="prerequisites"></a>先决条件

创建 [Azure HDInsight Spark](/azure/hdinsight/spark/apache-spark-jupyter-spark-sql-use-portal#create-an-apache-spark-cluster-in-hdinsight) 群集（如果还没有此群集）。

1. 访问 [Azure 门户](https://portal.azure.com)并选择“+ 创建资源”。 

1. 新建一个 Azure HDInsight 群集资源。 在创建群集过程中，选择“Spark 2.4”和“HDI 4.0”。  

## <a name="install-net-for-apache-spark"></a>安装 .NET for Apache Spark

在 Azure 门户中选择上一步中创建的 HDInsight Spark 群集。 

### <a name="stop-the-livy-server"></a>停止 Livy 服务器

1. 从门户选择“概览”，然后选择“Ambari 主页”。   出现提示时，请输入群集的登录凭据。

   ![选择“群集”仪表板下的 Ambari 主页](./media/hdinsight-notebook-installation/select-ambari.png)

2. 从左侧导航菜单选择“Spark2”，然后选择“用于 Spark2 的 Livy 服务器”。  

   ![选择用于 Spark2 的 Livy 服务器](./media/hdinsight-notebook-installation/select-livyserver.png)

3. 选择“hn0... host”。 

   ![显示已选中“hno...”的主机](./media/hdinsight-notebook-installation/select-host.png)

4. 选择“用于 Spark2 的 Livy 服务器”旁边的省略号，然后选择“停止”。   出现提示时，选择“确定”以继续操作  。

   停止用于 Spark2 的 Livy 服务器。
   ![选择省略号，然后选择“停止”](./media/hdinsight-notebook-installation/stop-server.png)

5. 对“hn1... host”重复前面的步骤  。

### <a name="submit-an-hdinsight-script-action"></a>提交 HDInsight 脚本操作

1. `install-interactive-notebook.sh` 是一个脚本，它可安装 .NET for Apache Spark 并可更改 Apache Livy 和 sparkmagic。 必须先创建并上传 `install-interactive-notebook.sh`，然后才可将脚本操作提交到 HDInsight。

   在本地计算机中新建名称为 install-interactive-notebook.sh 的文件，粘贴 [install-interactive-notebook.sh 内容](https://raw.githubusercontent.com/dotnet/spark/master/deployment/HDI-Spark/Notebooks/install-interactive-notebook.sh)中的内容。 

   将此脚本上传到可从 HDInsight 群集访问的 [URI](/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux#understand-script-actions) 上。 例如 `https://<my storage account>.blob.core.windows.net/<my container>/<some dir>/install-interactive-notebook.sh`。

2. 使用 [HDInsight 脚本操作](/azure/hdinsight/hdinsight-hadoop-customize-cluster-linux)运行群集上的 `install-interactive-notebook.sh`。

   返回到 Azure 门户中的 HDI 群集，从左侧的选项中选择“脚本操作”。  提交一个脚本操作，用于在 HDInsight Spark 群集上部署 .NET for Apache Spark REPL。 使用以下设置：

   |Property  |描述  |
   |---------|---------|
   | 脚本类型 | 自定义 |
   | “属性” | *安装 .NET for Apache Spark 交互式笔记本体验* |
   | Bash 脚本 URI | 向其上传 `install-interactive-notebook.sh` 的 URI。 |
   | 节点类型| 头节点和工作节点 |
   | 参数 | .NET for Apache Spark 版本。 可以查看 [.NET for Apache Spark 版本](https://github.com/dotnet/spark/releases)。 例如，如果要安装 Sparkdotnet 版本 1.0.0，则为 `1.0.0`。

   当脚本操作状态旁边出现绿色勾选标记时，转到下一步。

### <a name="start-the-livy-server"></a>启动 Livy 服务器

按照[停止 Livy 服务器](#stop-the-livy-server)部分中的说明操作，来为主机 hn0 和 hn1 启动（而非停止）用于 Spark2 的 Livy 服务器。    

### <a name="set-up-spark-default-configurations"></a>设置 Spark 默认配置

1. 从门户选择“概览”，然后选择“Ambari 主页”。   出现提示时，请输入群集的群集登录凭据。

2. 依次选择“Spark2”和“配置”   。 然后选择“自定义 spark2 默认值”  。

   ![Ambari 中的“配置”选项卡](./media/hdinsight-notebook-installation/spark-configs.png)

3. 选择“添加属性”，添加 Spark 默认设置。

   ![添加属性](./media/hdinsight-notebook-installation/add-property.png)

   有三种独立的属性。 在单一属性添加模式中使用文本属性类型一次添加一个属性。  检查任何键/值前后是否没有任何多余的空格。

   * **属性 1**
       * 键：&ensp;&ensp;`spark.dotnet.shell.command`
       * 值：`/usr/share/dotnet-tools/dotnet-try,kernel-server,--default-kernel,csharp`

   * **属性 2** 使用包含到前文中脚本操作的 .NET for Apache Spark 版本。
       * 键：&ensp;&ensp;`spark.dotnet.packages`
       * 值：`["nuget: Microsoft.Spark, 1.0.0", "nuget: Microsoft.Spark.Extensions.Delta, 1.0.0"]`

   * **属性 3**
       * 键：&ensp;&ensp;`spark.dotnet.interpreter`
       * 值：`try`

   例如，下图捕获了用于添加属性 1 的设置：

   ![添加文本属性](./media/hdinsight-notebook-installation/add-sparkconfig.png)

   添加这三个属性后，选择“保存”。  若看到配置建议警告屏幕，选择“仍然继续”  。

4. 重启受影响的组件。

   添加新属性后，需要重启受更改影响的组件。 在顶部选择“重启”，然后从下拉列表选择“重启所有受影响的组件”。  

   ![突出显示“重启”>“重启所有受影响的组件”的“配置”选项卡](./media/hdinsight-notebook-installation/restart-affected.png)

   出现提示时，选择“确认全部重启”以继续，然后单击“确定”完成操作。  

## <a name="submit-jobs-through-a-jupyter-notebook"></a>通过 Jupyter Notebook 提交作业

完成前面的步骤后，现在可以通过 Jupyter Notebook 提交 .NET for Apache Spark 作业。

1. 新建一个 .NET for Apache Spark 笔记本。 从 Azure 门户的 HDI 群集启动 Jupyter Notebook。

   ![启动 Jupyter Notebook](./media/hdinsight-notebook-installation/launch-notebook.png)

   然后选择“新建” > “.NET Spark (C#)”，创建一个笔记本。  

   ![Jupyter 笔记本](./media/hdinsight-notebook-installation/create-sparkdotnet-notebook.png)

2. 使用 .NET for Apache Spark 提交作业。

   使用以下代码片段创建 DataFrame：

   ```csharp
   var df = spark.Range(0,5);
   df.Show();
   ```

   ![创建将显示命令执行的 DataFrame](./media/hdinsight-notebook-installation/create-df.png)

   使用下面的代码片段注册用户定义的函数 (UDF)，并将此 UDF 与 DataFrame 一起使用：

   ```csharp
   var myawesomeudf = Udf<int, string>((id) => $"hello {id}");
   df.Select(myawesomeudf(df["id"])).Show();
   ```

   ![注册 UDF 并使用它](./media/hdinsight-notebook-installation/run-udf.png)

## <a name="next-steps"></a>后续步骤

* [将 .NET for Apache Spark 应用程序部署到 Azure HDInsight](../tutorials/hdinsight-deployment.md)
* [HDInsight 文档](/azure/hdinsight/)
