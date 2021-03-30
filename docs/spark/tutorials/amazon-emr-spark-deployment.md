---
title: 将 .NET for Apache Spark 应用程序部署到 Amazon EMR Spark
description: 了解如何将 .NET for Apache Spark 应用程序部署到 Amazon EMR Spark。
ms.date: 10/09/2020
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: a0904b5b692e1c32ba12a685f2c3a3f7d86c2c58
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874051"
---
# <a name="deploy-a-net-for-apache-spark-application-to-amazon-emr-spark"></a>将 .NET for Apache Spark 应用程序部署到 Amazon EMR Spark

本教程介绍如何将 .NET for Apache Spark 应用程序部署到 Amazon EMR Spark。 [Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html) 是一个托管群集平台，可简化 AWS 上运行的大数据框架。

在本教程中，你将了解如何执行以下操作：

> [!div class="checklist"]
>
> * 准备 Microsoft.Spark.Worker
> * 发布 Spark .NET 应用
> * 将应用部署到 Amazon EMR Spark
> * 运行应用

> [!Note]
> AWS EMR Spark 基于 Linux。 因此，如果要将应用部署到 AWS EMR Spark，请确保应用与 .NET Standard 兼容，并且使用 .NET Core 编译器编译应用。

## <a name="prerequisites"></a>先决条件

开始之前，请执行以下操作：

* 下载 [AWS CLI](https://aws.amazon.com/cli/)。
* 将 [install-worker.sh](https://github.com/dotnet/spark/blob/main/deployment/install-worker.sh) 下载到本地计算机。 这是稍后用于将 .NET for Apache Spark 依赖文件复制到 Spark 群集的工作器节点的帮助程序脚本。

## <a name="prepare-worker-dependencies"></a>准备辅助角色依赖项

Microsoft.Spark.Worker 是后端组件，位于 Spark 群集的单个工作器节点上。 若要执行 C# UDF（用户定义的函数），Spark 需要了解如何启动 .NET CLR 以执行 UDF。 Microsoft.Spark.Worker 向 Spark 提供启用此功能的类集合。

1. 选择要在群集上部署的 [Microsoft.Spark.Worker](https://github.com/dotnet/spark/releases) Linux netcoreapp 版本。

   例如，如果需要使用 `netcoreapp3.1` 的 `.NET for Apache Spark v1.0.0`，则需要下载 [Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz](https://github.com/dotnet/spark/releases/download/v1.0.0/Microsoft.Spark.Worker.netcoreapp3.1.linux-x64-1.0.0.tar.gz)。

2. 将 `Microsoft.Spark.Worker.<release>.tar.gz` 和 [install-worker.sh](https://github.com/dotnet/spark/blob/main/deployment/install-worker.sh) 上传到群集具有访问权限的分布式文件系统（如 S3）。

## <a name="prepare-your-net-for-apache-spark-app"></a>准备 .NET for Apache Spark 应用

1. 按照[入门](get-started.md)教程来生成应用。

2. 发布独立的 Spark .NET 应用。

   在 Linux 上运行以下命令。

   ```dotnetcli
   dotnet publish -c Release -f netcoreapp3.1 -r ubuntu.16.04-x64
   ```

3. 为已发布的文件生成 `<your app>.zip`。

   使用 `zip` 在 Linux 上运行以下命令。

   ```bash
   zip -r <your app>.zip .
   ```

4. 将以下项上传到群集具有访问权限的分布式文件系统（如 S3）：

   * `microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar`：此 jar 作为 [Microsoft.Spark](https://www.nuget.org/packages/Microsoft.Spark/) NuGet 包的一部分包含在内，并且并置在应用的生成输出目录中。
   * `<your app>.zip`
   * 要放在每个执行程序的工作目录中的文件（如每位工作人员都可以访问的依赖文件或公共数据）或程序集（如包含应用所依赖的用户定义的函数或库的 DLL）。

## <a name="deploy-to-amazon-emr-spark"></a>部署到 Amazon EMR Spark

[Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html) 是一个托管群集平台，可简化 AWS 上运行的大数据框架。

> [!NOTE]
> Amazon EMR Spark 基于 Linux。 因此，如果要将应用部署到 Amazon EMR Spark，请确保应用与 .NET Standard 兼容，并且使用 [.NET Core 编译器](https://dotnet.microsoft.com/download)编译应用。

### <a name="deploy-microsoftsparkworker"></a>部署 Microsoft.Spark.Worker

只需在创建群集时执行此步骤。

使用 [Bootstrap 操作](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-plan-bootstrap.html)在创建群集期间运行 `install-worker.sh`。

使用 AWS CLI 在 Linux 上运行以下命令。

```bash
aws emr create-cluster \
--name "Test cluster" \
--release-label emr-5.23.0 \
--use-default-roles \
--ec2-attributes KeyName=myKey \
--applications Name=Spark \
--instance-count 3 \
--instance-type m1.medium \
--bootstrap-actions Path=s3://mybucket/<some dir>/install-worker.sh,Name="Install Microsoft.Spark.Worker",Args=["aws","s3://mybucket/<some dir>/Microsoft.Spark.Worker.<release>.tar.gz","/usr/local/bin"]
```

## <a name="run-your-app"></a>运行应用

可采用两种方式在 Amazon EMR Spark 中运行应用：spark-submit 和 Amazon EMR 步骤。

### <a name="use-spark-submit"></a>使用 spark-submit

可以使用 [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) 命令将 .NET for Apache Spark 作业提交到 Amazon EMR Spark。

1. 群集中某个节点的 `ssh`。

2. 运行 `spark-submit`。

   ```bash
   spark-submit \
   --master yarn \
   --class org.apache.spark.deploy.dotnet.DotnetRunner \
   --files <comma-separated list of assemblies that contain UDF definitions, if any> \
   s3://mybucket/<some dir>/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar \
   s3://mybucket/<some dir>/<your app>.zip <your app> <app args>
   ```

### <a name="use-amazon-emr-steps"></a>使用 Amazon EMR 步骤

[Amazon EMR 步骤](https://docs.aws.amazon.com/emr/latest/ReleaseGuide/emr-spark-submit-step.html)可用于将作业提交到 EMR 群集上安装的 Spark 框架。

使用 AWS CLI 在 Linux 上运行以下命令。

```bash
aws emr add-steps \
--cluster-id j-xxxxxxxxxxxxx \
--steps Type=spark,Name="Spark Program",Args=[--master,yarn,--files,s3://mybucket/<some dir>/<udf assembly>,--class,org.apache.spark.deploy.dotnet.DotnetRunner,s3://mybucket/<some dir>/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar,s3://mybucket/<some dir>/<your app>.zip,<your app>,<app arg 1>,<app arg 2>,...,<app arg n>],ActionOnFailure=CONTINUE
```

## <a name="next-steps"></a>后续步骤

在本教程中，你已将 .NET for Apache Spark 应用程序部署到 Amazon EMR Spark。 对于 .NET for Apache Spark 示例项目，请继续使用 GitHub。

> [!div class="nextstepaction"]
> [.NET for Apache Spark 示例](https://github.com/dotnet/spark/tree/main/examples)
