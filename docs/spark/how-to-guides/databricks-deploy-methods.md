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
# <a name="submit-a-net-for-apache-spark-job-to-databricks"></a><span data-ttu-id="0ff2f-103">将 .NET for Apache Spark 作业提交到 Databricks</span><span class="sxs-lookup"><span data-stu-id="0ff2f-103">Submit a .NET for Apache Spark job to Databricks</span></span>

<span data-ttu-id="0ff2f-104">可以在 Databricks 群集上运行 .NET for Apache Spark 作业，但它并不是现成可用的。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-104">You can run your .NET for Apache Spark jobs on Databricks clusters, but it is not available out-of-the-box.</span></span> <span data-ttu-id="0ff2f-105">可通过两种方法将 .NET for Apache Spark 作业部署到 Databricks：`spark-submit` 和 Set Jar。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-105">There are two ways to deploy your .NET for Apache Spark job to Databricks: `spark-submit` and Set Jar.</span></span>

## <a name="deploy-using-spark-submit"></a><span data-ttu-id="0ff2f-106">使用 spark-submit 进行部署</span><span class="sxs-lookup"><span data-stu-id="0ff2f-106">Deploy using spark-submit</span></span>

<span data-ttu-id="0ff2f-107">可以使用 [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) 命令将 .NET for Apache Spark 作业提交到 Databricks。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-107">You can use the [spark-submit](https://spark.apache.org/docs/latest/submitting-applications.html) command to submit .NET for Apache Spark jobs to Databricks.</span></span> <span data-ttu-id="0ff2f-108">`spark-submit` 允许仅提交到按需创建的群集。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-108">`spark-submit` allows submission only to a cluster that gets created on-demand.</span></span>

1. <span data-ttu-id="0ff2f-109">导航到 Databricks 工作区并创建一个作业。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-109">Navigate to your Databricks Workspace and create a job.</span></span> <span data-ttu-id="0ff2f-110">选择作业的标题，然后选择“配置 spark-submit”。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-110">Choose a title for your job, and then select **Configure spark-submit**.</span></span> <span data-ttu-id="0ff2f-111">在作业配置中粘贴以下参数，然后选择“确认”。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-111">Paste the following parameters in the job configuration, then select **Confirm**.</span></span>

    ```
    ["--files","/dbfs/<path-to>/<app assembly/file to deploy to worker>","--class","org.apache.spark.deploy.dotnet.DotnetRunner","/dbfs/<path-to>/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar","/dbfs/<path-to>/<app name>.zip","<app bin name>","app arg1","app arg2"]
    ```

    > [!NOTE]
    > <span data-ttu-id="0ff2f-112">根据特定文件和配置更新上述参数的内容。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-112">Update the contents of the above parameter based on your specific files and configuration.</span></span> <span data-ttu-id="0ff2f-113">例如，引用已上传到 DBFS 的 Microsoft.Spark jar 文件的版本，并使用适当的应用名称和已发布的应用 zip 文件。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-113">For instance, reference the version of the Microsoft.Spark jar file that you uploaded to DBFS, and use the appropriate name of your app and published app zip file.</span></span>

2. <span data-ttu-id="0ff2f-114">导航到作业并选择“编辑”以配置作业的群集。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-114">Navigate to your job and select **Edit** to configure your job's cluster.</span></span> <span data-ttu-id="0ff2f-115">根据想要在部署中使用的 Apache Spark 版本设置 Databricks Runtime 版本。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-115">Set the Databricks Runtime Version based on the version of Apache Spark you wish to use in your deployment.</span></span> <span data-ttu-id="0ff2f-116">然后选择“高级选项”>“Init 脚本”，并将“Init 脚本路径”设置为 `dbfs:/spark-dotnet/db-init.sh`。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-116">Then select **Advanced Options > Init Scripts**, and set Init Script Path as `dbfs:/spark-dotnet/db-init.sh`.</span></span> <span data-ttu-id="0ff2f-117">选择“确认”以确认群集设置。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-117">Select **Confirm** to confirm your cluster settings.</span></span>

3. <span data-ttu-id="0ff2f-118">导航到作业并选择“立即运行”，在新配置的 Spark 群集上运行作业。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-118">Navigate to your job and select **Run Now** to run your job on your newly configured Spark cluster.</span></span> <span data-ttu-id="0ff2f-119">创建作业的群集需要几分钟时间。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-119">It takes a few minutes for the job's cluster to be created.</span></span> <span data-ttu-id="0ff2f-120">一旦创建完成，你的作业就会提交。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-120">Once it is created, your job will be submitted.</span></span> <span data-ttu-id="0ff2f-121">可以通过从 Databricks 工作区的左侧菜单中选择“群集”来查看输出，然后选择“驱动程序日志” 。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-121">You can view the output by selecting **Clusters** from the left menu of your Databricks workspace, then select **Driver Logs**.</span></span>

## <a name="deploy-using-set-jar"></a><span data-ttu-id="0ff2f-122">使用 Set Jar 进行部署</span><span class="sxs-lookup"><span data-stu-id="0ff2f-122">Deploy using Set Jar</span></span>

<span data-ttu-id="0ff2f-123">或者，可以在 Databricks 工作区中使用 [Set Jar](/azure/databricks/jobs#--create-a-job) 将 .NET for Apache Spark 作业提交到 Databricks。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-123">Alternatively, you can use [Set Jar](/azure/databricks/jobs#--create-a-job) in your Databricks workspace to submit .NET for Apache Spark jobs to Databricks.</span></span> <span data-ttu-id="0ff2f-124">通过 Set Jar，可将作业提交到现有的有效群集。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-124">*Set Jar* allows job submission to an existing active cluster.</span></span>

### <a name="one-time-setup"></a><span data-ttu-id="0ff2f-125">一次性设置</span><span class="sxs-lookup"><span data-stu-id="0ff2f-125">One-time setup</span></span>

1. <span data-ttu-id="0ff2f-126">导航到 Databricks 群集并选择左侧菜单的“作业”，后跟 Set JAR 。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-126">Navigate to your Databricks cluster and select **Jobs** from the left-side menu, followed by **Set JAR**.</span></span>

2. <span data-ttu-id="0ff2f-127">上传相应的 `microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar`。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-127">Upload the appropriate `microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar`.</span></span>

3. <span data-ttu-id="0ff2f-128">修改以下参数，使其包含为替代 `<your-app-name>` 而发布的可执行文件的正确名称：</span><span class="sxs-lookup"><span data-stu-id="0ff2f-128">Modify the following parameters to include the correct name for the executable that you published in place of `<your-app-name>`:</span></span>

    ```
    Main Class: org.apache.spark.deploy.dotnet.DotnetRunner
    Arguments /dbfs/apps/<your-app-name>.zip <your-app-name>
    ```

4. <span data-ttu-id="0ff2f-129">将群集配置为指向已设置 init 脚本的现有群集。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-129">Configure the cluster to point to an existing cluster for which you have already set the init script.</span></span>

### <a name="publish-and-run-your-app"></a><span data-ttu-id="0ff2f-130">发布并运行应用</span><span class="sxs-lookup"><span data-stu-id="0ff2f-130">Publish and run your app</span></span>

1. <span data-ttu-id="0ff2f-131">请确保已发布应用，并且应用程序代码不使用 `SparkSession.Stop()`。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-131">Ensure you have published your app, and that your application code does not use `SparkSession.Stop()`.</span></span>

2. <span data-ttu-id="0ff2f-132">使用 [Databricks CLI](/azure/databricks/dev-tools/databricks-cli) 将应用程序上传到 Databricks 群集。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-132">Use [Databricks CLI](/azure/databricks/dev-tools/databricks-cli) to upload your application to your Databricks cluster.</span></span> <span data-ttu-id="0ff2f-133">例如，使用以下命令将已发布的应用上传到群集：</span><span class="sxs-lookup"><span data-stu-id="0ff2f-133">For example, use the following command to upload your published app to your cluster:</span></span>

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <your-app-name>.zip dbfs:/apps/<your-app-name>.zip
    ```

3. <span data-ttu-id="0ff2f-134">如果应用程序集（例如包含用户定义的函数及其依赖项的 DLL）中具有任何用户定义的函数，则需要放置在每个 Microsoft.Spark.Worker 的工作目录中。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-134">If you have any user-defined functions in your app, the app assemblies, such as DLLs that contain user-defined functions along with their dependencies, need to be placed in the working directory of each *Microsoft.Spark.Worker*.</span></span>

    <span data-ttu-id="0ff2f-135">将应用程序集上传到 Databricks 群集：</span><span class="sxs-lookup"><span data-stu-id="0ff2f-135">Upload your application assemblies to your Databricks cluster:</span></span>

    ```console
    cd <path-to-your-app-publish-directory>
    databricks fs cp <assembly>.dll dbfs:/apps/dependencies
    ```

    <span data-ttu-id="0ff2f-136">取消评论并将 [db-init.sh](https://github.com/dotnet/spark/blob/main/deployment/db-init.sh) 中的应用依赖项部分修改为指向应用依赖项路径。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-136">Uncomment and modify the app dependencies section in [db-init.sh](https://github.com/dotnet/spark/blob/main/deployment/db-init.sh) to point to your app dependencies path.</span></span> <span data-ttu-id="0ff2f-137">然后，将已更新的 db-init.sh 上传到群集：</span><span class="sxs-lookup"><span data-stu-id="0ff2f-137">Then, upload the updated *db-init.sh* to your cluster:</span></span>

    ```console
    cd <path-to-db-init-and-install-worker>
    databricks fs cp db-init.sh dbfs:/spark-dotnet/db-init.sh
    ```

4. <span data-ttu-id="0ff2f-138">导航到“Databricks 群集”>“作业”>“[作业名称]”>“立即运行”以运行作业。</span><span class="sxs-lookup"><span data-stu-id="0ff2f-138">Navigate to **Databricks cluster > Jobs > [Job-name] > Run Now** to run your job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ff2f-139">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0ff2f-139">Next steps</span></span>

* [<span data-ttu-id="0ff2f-140">.NET for Apache Spark 入门</span><span class="sxs-lookup"><span data-stu-id="0ff2f-140">Get started with .NET for Apache Spark</span></span>](../tutorials/get-started.md)
* [<span data-ttu-id="0ff2f-141">将 .NET for Apache Spark 应用程序部署到 Databricks</span><span class="sxs-lookup"><span data-stu-id="0ff2f-141">Deploy a .NET for Apache Spark application to Databricks</span></span>](../tutorials/databricks-deployment.md)
* [<span data-ttu-id="0ff2f-142">Azure Databricks 文档</span><span class="sxs-lookup"><span data-stu-id="0ff2f-142">Azure Databricks Documentation</span></span>](/azure/azure-databricks/)
