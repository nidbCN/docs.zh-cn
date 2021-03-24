---
title: 在 Azure 上使用 F#
description: 有关结合使用 F# 和 Azure 服务的指南
author: sylvanc
ms.date: 07/29/2020
ms.custom: devx-track-fsharp
ms.openlocfilehash: 406de0cdef67631ddaaa13f686c9b2093ba5d2ff
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873259"
---
# <a name="using-f-on-azure"></a><span data-ttu-id="f7375-103">在 Azure 上使用 F#</span><span class="sxs-lookup"><span data-stu-id="f7375-103">Using F# on Azure</span></span>

<span data-ttu-id="f7375-104">F# 是一种出色的云编程语言，常用于编写 Web 应用程序、云服务、云托管微服务，也可用于可缩放数据处理。</span><span class="sxs-lookup"><span data-stu-id="f7375-104">F# is a superb language for cloud programming and is frequently used to write web applications, cloud services, cloud-hosted microservices, and for scalable data processing.</span></span>

<span data-ttu-id="f7375-105">在以下部分中，你将发现如何结合使用 F# 与一系列 Azure 服务的相关资源。</span><span class="sxs-lookup"><span data-stu-id="f7375-105">In the following sections, you will find resources on how to use a range of Azure services with F#.</span></span>

> [!NOTE]
> <span data-ttu-id="f7375-106">如果某特定 Azure 服务不在此文档集中，请参阅适用于该服务的 Azure Functions 或 .NET 文档。</span><span class="sxs-lookup"><span data-stu-id="f7375-106">If a particular Azure service isn't in this documentation set, please consult either the Azure Functions or .NET documentation for that service.</span></span> <span data-ttu-id="f7375-107">某些 Azure 服务独立于语言，不需要任何特定于语言的文档，未在此处列出。</span><span class="sxs-lookup"><span data-stu-id="f7375-107">Some Azure services are language-independent and require no language-specific documentation and are not listed here.</span></span>

## <a name="using-azure-virtual-machines-with-f"></a><span data-ttu-id="f7375-108">结合使用 Azure 虚拟机和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-108">Using Azure Virtual Machines with F\#</span></span>

<span data-ttu-id="f7375-109">Azure 支持各种不同的虚拟机 (VM) 配置，请参阅 [Linux 和 Azure 虚拟机](https://azure.microsoft.com/services/virtual-machines/)。</span><span class="sxs-lookup"><span data-stu-id="f7375-109">Azure supports a wide range of virtual machine (VM) configurations, see [Linux and Azure Virtual Machines](https://azure.microsoft.com/services/virtual-machines/).</span></span>

<span data-ttu-id="f7375-110">若要在虚拟机上安装 F# 用来执行、编译和/或编写脚本，请参阅[在 Linux 上使用 F#](https://fsharp.org/use/linux) 和[在 Windows 上使用 F#](https://fsharp.org/use/windows)。</span><span class="sxs-lookup"><span data-stu-id="f7375-110">To install F# on a virtual machine for execution, compilation and/or scripting see [Using F# on Linux](https://fsharp.org/use/linux) and [Using F# on Windows](https://fsharp.org/use/windows).</span></span>

## <a name="using-azure-functions-with-f"></a><span data-ttu-id="f7375-111">结合使用 Azure Functions 和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-111">Using Azure Functions with F\#</span></span>

<span data-ttu-id="f7375-112">[Azure Functions](https://azure.microsoft.com/services/functions/) 是一个在云中轻松运行小代码段或“函数”的解决方案。</span><span class="sxs-lookup"><span data-stu-id="f7375-112">[Azure Functions](https://azure.microsoft.com/services/functions/) is a solution for easily running small pieces of code, or "functions," in the cloud.</span></span> <span data-ttu-id="f7375-113">可针对手头的问题编写所需代码，无需担心用来运行该代码的完整应用程序或基础结构。</span><span class="sxs-lookup"><span data-stu-id="f7375-113">You can write just the code you need for the problem at hand, without worrying about a whole application or the infrastructure to run it.</span></span> <span data-ttu-id="f7375-114">函数连接到 Azure 存储和其他云托管资源中的事件。</span><span class="sxs-lookup"><span data-stu-id="f7375-114">Your functions are connected to events in Azure storage and other cloud-hosted resources.</span></span> <span data-ttu-id="f7375-115">数据通过函数参数流入 F# 函数。</span><span class="sxs-lookup"><span data-stu-id="f7375-115">Data flows into your F# functions via function arguments.</span></span> <span data-ttu-id="f7375-116">可以使用所选择的开发语言，信任 Azure 按需进行扩展。</span><span class="sxs-lookup"><span data-stu-id="f7375-116">You can use your development language of choice, trusting Azure to scale as needed.</span></span>

<span data-ttu-id="f7375-117">Azure Functions 支持 F# 作为第一类语言，它执行 F# 代码时高效、反应迅速且可缩放。</span><span class="sxs-lookup"><span data-stu-id="f7375-117">Azure Functions support F# as a first-class language with efficient, reactive, scalable execution of F# code.</span></span> <span data-ttu-id="f7375-118">请参阅 [Azure Functions F# 开发人员参考](/azure/azure-functions/functions-reference-fsharp)，了解如何将 F# 用于 Azure Functions 的相关参考文档。</span><span class="sxs-lookup"><span data-stu-id="f7375-118">See the [Azure Functions F# Developer Reference](/azure/azure-functions/functions-reference-fsharp) for reference documentation on how to use F# with Azure Functions.</span></span>

<span data-ttu-id="f7375-119">结合使用 Azure Functions 和 F# 的其他资源：</span><span class="sxs-lookup"><span data-stu-id="f7375-119">Other resources for using Azure Functions and F#:</span></span>

* <span data-ttu-id="f7375-120">[Scale Up Azure Functions in F# Using Suave](https://blog.tamizhvendan.in/blog/2016/09/19/scale-up-azure-functions-in-f-number-using-suave/)（使用 Suave 以 F# 扩展 Azure Functions）</span><span class="sxs-lookup"><span data-stu-id="f7375-120">[Scale Up Azure Functions in F# Using Suave](https://blog.tamizhvendan.in/blog/2016/09/19/scale-up-azure-functions-in-f-number-using-suave/)</span></span>
* <span data-ttu-id="f7375-121">[How to create Azure function in F#](https://www.mnie.me/azurefunctions)（如何以 F# 创建 Azure Functions）</span><span class="sxs-lookup"><span data-stu-id="f7375-121">[How to create Azure function in F#](https://www.mnie.me/azurefunctions)</span></span>
* [<span data-ttu-id="f7375-122">将 Azure 类型提供程序与 Azure Functions 结合使用</span><span class="sxs-lookup"><span data-stu-id="f7375-122">Using the Azure Type Provider with Azure Functions</span></span>](https://compositional-it.com/blog/2017/08-30-using-the-azure-type-provider-with-azure-functions/index.html)

## <a name="using-azure-storage-with-f"></a><span data-ttu-id="f7375-123">结合使用 Azure 存储和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-123">Using Azure Storage with F\#</span></span>

<span data-ttu-id="f7375-124">Azure 存储是一种基层存储服务，用于依赖于持久性、可用性和可缩放性来满足其客户需求的现代应用程序。</span><span class="sxs-lookup"><span data-stu-id="f7375-124">Azure Storage is a base layer of storage services for modern applications that rely on durability, availability, and scalability to meet the needs of customers.</span></span> <span data-ttu-id="f7375-125">F# 程序可使用以下文章中所述的技术，直接与 Azure 存储服务交互。</span><span class="sxs-lookup"><span data-stu-id="f7375-125">F# programs can interact directly with Azure storage services, using the techniques described in the following articles.</span></span>

* [<span data-ttu-id="f7375-126">通过 F# 实现 Azure Blob 存储入门</span><span class="sxs-lookup"><span data-stu-id="f7375-126">Get started with Azure Blob Storage using F#</span></span>](blob-storage.md)
* [<span data-ttu-id="f7375-127">通过 F# 实现 Azure 文件存储入门</span><span class="sxs-lookup"><span data-stu-id="f7375-127">Get started with Azure File Storage using F#</span></span>](file-storage.md)
* [<span data-ttu-id="f7375-128">通过 F# 实现 Azure 队列存储入门</span><span class="sxs-lookup"><span data-stu-id="f7375-128">Get started with Azure Queue Storage using F#</span></span>](queue-storage.md)
* [<span data-ttu-id="f7375-129">通过 F# 实现 Azure 表存储入门</span><span class="sxs-lookup"><span data-stu-id="f7375-129">Get started with Azure Table Storage using F#</span></span>](table-storage.md)

<span data-ttu-id="f7375-130">Azure 存储还可以通过声明性配置（而非显式 API 调用）与 Azure Functions 结合使用。</span><span class="sxs-lookup"><span data-stu-id="f7375-130">Azure Storage can also be used in conjunction with Azure Functions through declarative configuration rather than explicit API calls.</span></span> <span data-ttu-id="f7375-131">请参阅 [Azure Functions triggers and bindings for Azure Storage](/azure/azure-functions/functions-bindings-storage)（用于 Azure 存储的 Azure Functions 触发器和绑定），其中包括 F# 示例。</span><span class="sxs-lookup"><span data-stu-id="f7375-131">See [Azure Functions triggers and bindings for Azure Storage](/azure/azure-functions/functions-bindings-storage) which includes F# examples.</span></span>

## <a name="using-azure-app-service-with-f"></a><span data-ttu-id="f7375-132">结合使用 Azure 应用服务和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-132">Using Azure App Service with F\#</span></span>

<span data-ttu-id="f7375-133">[Azure 应用服务](https://azure.microsoft.com/services/app-service/)是一个用于构建功能强大的 Web 和移动应用的云平台，这些应用可连接到任何位置（云中或本地）的数据。</span><span class="sxs-lookup"><span data-stu-id="f7375-133">[Azure App Service](https://azure.microsoft.com/services/app-service/) is a cloud platform to build powerful web and mobile apps that connect to data anywhere, in the cloud or on-premises.</span></span>

* [<span data-ttu-id="f7375-134">F# Azure Web API 示例</span><span class="sxs-lookup"><span data-stu-id="f7375-134">F# Azure Web API example</span></span>](https://github.com/fsprojects/azure-webapi-example)
* <span data-ttu-id="f7375-135">[Hosting F# in a web application on Azure](https://github.com/isaacabraham/fsharp-demonstrator)（在 Azure 上的 Web 应用程序中托管 F#）</span><span class="sxs-lookup"><span data-stu-id="f7375-135">[Hosting F# in a web application on Azure](https://github.com/isaacabraham/fsharp-demonstrator)</span></span>

## <a name="using-apache-spark-with-f-on-azure-hdinsight-or-azure-databricks"></a><span data-ttu-id="f7375-136">在 Azure HDInsight 或 Azure Databricks 上将 Apache Spark 与 F# 结合使用</span><span class="sxs-lookup"><span data-stu-id="f7375-136">Using Apache Spark with F# on Azure HDInsight or Azure Databricks</span></span>

<span data-ttu-id="f7375-137">[Apache Spark for Azure HDInsight](/azure/hdinsight/spark/apache-spark-overview) 是一个开放源代码处理框架，用于运行大型数据分析应用程序。</span><span class="sxs-lookup"><span data-stu-id="f7375-137">[Apache Spark for Azure HDInsight](/azure/hdinsight/spark/apache-spark-overview) is an open source processing framework that runs large-scale data analytics applications.</span></span> <span data-ttu-id="f7375-138">[Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks) 是基于 Apache Spark 的分析平台，已针对 Microsoft Azure 云服务平台进行优化。</span><span class="sxs-lookup"><span data-stu-id="f7375-138">[Azure Databricks](/azure/databricks/scenarios/what-is-azure-databricks) is an Apache Spark-based analytics platform optimized for the Microsoft Azure cloud services platform.</span></span> <span data-ttu-id="f7375-139">Azure 使 Apache Spark 的部署简单且经济实惠。</span><span class="sxs-lookup"><span data-stu-id="f7375-139">Azure makes Apache Spark easy and cost effective to deploy.</span></span> <span data-ttu-id="f7375-140">使用 [.NET for Apache Spark](../../spark/what-is-apache-spark-dotnet.md)（一组用于 Apache Spark 的 .NET 绑定）在 F# 中开发 Spark 应用程序。</span><span class="sxs-lookup"><span data-stu-id="f7375-140">Develop your Spark application in F# using [.NET for Apache Spark](../../spark/what-is-apache-spark-dotnet.md), a set of .NET bindings for Apache Spark.</span></span>

* [<span data-ttu-id="f7375-141">.NET for Apache Spark F# 示例</span><span class="sxs-lookup"><span data-stu-id="f7375-141">.NET for Apache Spark F# samples</span></span>](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.FSharp.Examples)
* [<span data-ttu-id="f7375-142">在 Azure HDInsight 中安装 .NET Interactive Jupyter 笔记本</span><span class="sxs-lookup"><span data-stu-id="f7375-142">Install .NET Interactive Jupyter notebooks in Azure HDInsight</span></span>](../../spark/how-to-guides/hdinsight-notebook-installation.md)
* [<span data-ttu-id="f7375-143">将 Apache Spark 作业提交到 Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="f7375-143">Submit Apache Spark jobs to Azure HDInsight</span></span>](../../spark/how-to-guides/hdinsight-deploy-methods.md)
* [<span data-ttu-id="f7375-144">将 Apache Spark 作业提交到 Azure Databricks</span><span class="sxs-lookup"><span data-stu-id="f7375-144">Submit Apache Spark jobs to Azure Databricks</span></span>](../../spark/how-to-guides/databricks-deploy-methods.md)

## <a name="using-azure-cosmos-db-with-f"></a><span data-ttu-id="f7375-145">结合使用 Azure Cosmos DB 和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-145">Using Azure Cosmos DB with F\#</span></span>

<span data-ttu-id="f7375-146">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) 是一种面向高可用性、全球分布式应用的 NoSQL 服务。</span><span class="sxs-lookup"><span data-stu-id="f7375-146">[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db) is a NoSQL service for highly available, globally distributed apps.</span></span>

<span data-ttu-id="f7375-147">可采用两种方式结合使用 Azure Cosmos DB 和 F#：</span><span class="sxs-lookup"><span data-stu-id="f7375-147">Azure Cosmos DB can be used with F# in two ways:</span></span>

1. <span data-ttu-id="f7375-148">通过创建 F# Azure Functions，它可对 Azure Cosmos DB 集合做出响应或导致其更改。</span><span class="sxs-lookup"><span data-stu-id="f7375-148">Through the creation of F# Azure Functions which react to or cause changes to Azure Cosmos DB collections.</span></span> <span data-ttu-id="f7375-149">请参阅[适用于 Azure Functions 的 Azure Cosmos DB 绑定](/azure/azure-functions/functions-bindings-cosmosdb)，或者</span><span class="sxs-lookup"><span data-stu-id="f7375-149">See [Azure Cosmos DB bindings for Azure Functions](/azure/azure-functions/functions-bindings-cosmosdb), or</span></span>
2. <span data-ttu-id="f7375-150">使用[适用于 SQL API 的 Azure Cosmos DB .NET SDK](/azure/cosmos-db/sql-api-sdk-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="f7375-150">By using the [Azure Cosmos DB .NET SDK for SQL API](/azure/cosmos-db/sql-api-sdk-dotnet).</span></span> <span data-ttu-id="f7375-151">C# 中有相关示例。</span><span class="sxs-lookup"><span data-stu-id="f7375-151">The related samples are in C#.</span></span>

## <a name="using-azure-event-hubs-with-f"></a><span data-ttu-id="f7375-152">结合使用 Azure 事件中心和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-152">Using Azure Event Hubs with F\#</span></span>

<span data-ttu-id="f7375-153">[Azure 事件中心](https://azure.microsoft.com/services/event-hubs/)提供来自网站、应用和设备的云规模遥测引入。</span><span class="sxs-lookup"><span data-stu-id="f7375-153">[Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) provide cloud-scale telemetry ingestion from websites, apps, and devices.</span></span>

<span data-ttu-id="f7375-154">可采用两种方式结合使用 Azure 事件中心与 F#：</span><span class="sxs-lookup"><span data-stu-id="f7375-154">Azure Event Hubs can be used with F# in two ways:</span></span>

1. <span data-ttu-id="f7375-155">通过创建由事件触发的 F# Azure Functions。</span><span class="sxs-lookup"><span data-stu-id="f7375-155">Through the creation of F# Azure Functions which are triggered by events.</span></span> <span data-ttu-id="f7375-156">请参阅 [Azure Function 事件中心触发器](/azure/azure-functions/functions-bindings-event-hubs)，或</span><span class="sxs-lookup"><span data-stu-id="f7375-156">See [Azure Function triggers for Event Hubs](/azure/azure-functions/functions-bindings-event-hubs), or</span></span>
2. <span data-ttu-id="f7375-157">通过使用[适用于 Azure 的 .NET SDK](/azure/event-hubs/event-hubs-csharp-ephcs-getstarted)。</span><span class="sxs-lookup"><span data-stu-id="f7375-157">By using the [.NET SDK for Azure](/azure/event-hubs/event-hubs-csharp-ephcs-getstarted).</span></span> <span data-ttu-id="f7375-158">请注意，这些示例使用的是 C#。</span><span class="sxs-lookup"><span data-stu-id="f7375-158">Note these examples are in C#.</span></span>

## <a name="using-azure-notification-hubs-with-f"></a><span data-ttu-id="f7375-159">结合使用 Azure 通知中心和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-159">Using Azure Notification Hubs with F\#</span></span>

<span data-ttu-id="f7375-160">[Azure 通知中心](/azure/notification-hubs/)是多平台扩展式推送基础结构，可使用它从任何后端（云中或本地）向任何移动平台发送移动推送通知。</span><span class="sxs-lookup"><span data-stu-id="f7375-160">[Azure Notification Hubs](/azure/notification-hubs/) are multiplatform, scaled-out push infrastructure that enable you to send mobile push notifications from any backend (in the cloud or on-premises) to any mobile platform.</span></span>

<span data-ttu-id="f7375-161">可采用两种方式结合使用 Azure 通知中心与 F#：</span><span class="sxs-lookup"><span data-stu-id="f7375-161">Azure Notification Hubs can be used with F# in two ways:</span></span>

1. <span data-ttu-id="f7375-162">通过创建向通知中心发送结果的 F# Azure Functions。</span><span class="sxs-lookup"><span data-stu-id="f7375-162">Through the creation of F# Azure Functions which send results to a notification hub.</span></span> <span data-ttu-id="f7375-163">请参阅 [Azure Functions 通知中心输出触发器](/azure/azure-functions/functions-bindings-notification-hubs)，或</span><span class="sxs-lookup"><span data-stu-id="f7375-163">See [Azure Function output triggers for Notification Hubs](/azure/azure-functions/functions-bindings-notification-hubs), or</span></span>
2. <span data-ttu-id="f7375-164">通过使用[适用于 Azure 的 .NET SDK](/archive/blogs/azuremobile/push-notifications-using-notification-hub-and-net-backend)。</span><span class="sxs-lookup"><span data-stu-id="f7375-164">By using the [.NET SDK for Azure](/archive/blogs/azuremobile/push-notifications-using-notification-hub-and-net-backend).</span></span> <span data-ttu-id="f7375-165">请注意，这些示例使用的是 C#。</span><span class="sxs-lookup"><span data-stu-id="f7375-165">Note these examples are in C#.</span></span>

## <a name="implementing-webhooks-on-azure-with-f"></a><span data-ttu-id="f7375-166">通过 F\# 在 Azure 上实现 Webhook</span><span class="sxs-lookup"><span data-stu-id="f7375-166">Implementing WebHooks on Azure with F\#</span></span>

<span data-ttu-id="f7375-167">[Webhook](https://en.wikipedia.org/wiki/Webhook) 是通过 Web 请求触发的回调。</span><span class="sxs-lookup"><span data-stu-id="f7375-167">A [Webhook](https://en.wikipedia.org/wiki/Webhook) is a callback triggered via a web request.</span></span> <span data-ttu-id="f7375-168">Webhook 由 GitHub 等站点使用，用以向事件发送信号。</span><span class="sxs-lookup"><span data-stu-id="f7375-168">Webhooks are used by sites such as GitHub to signal events.</span></span>

<span data-ttu-id="f7375-169">Webhook 可采用 F# 实现，并通过 [Azure Function in F# with a Webhook Binding](/azure/azure-functions/functions-bindings-http-webhook)（具有 Web 绑定的 F# Azure Function）在 Azure 上进行托管。</span><span class="sxs-lookup"><span data-stu-id="f7375-169">Webhooks can be implemented in F# and hosted on Azure via an [Azure Function in F# with a Webhook Binding](/azure/azure-functions/functions-bindings-http-webhook).</span></span>

## <a name="using-webjobs-with-f"></a><span data-ttu-id="f7375-170">结合使用 Webjob 和 F\#</span><span class="sxs-lookup"><span data-stu-id="f7375-170">Using Webjobs with F\#</span></span>

<span data-ttu-id="f7375-171">[Webjob](/azure/app-service-web/web-sites-create-web-jobs) 是可采用以下三种方式在应用服务 Web 应用中运行的程序：按需、连续或按计划。</span><span class="sxs-lookup"><span data-stu-id="f7375-171">[Webjobs](/azure/app-service-web/web-sites-create-web-jobs) are programs you can run in your App Service web app in three ways: on demand, continuously, or on a schedule.</span></span>

[<span data-ttu-id="f7375-172">示例 F# Webjob</span><span class="sxs-lookup"><span data-stu-id="f7375-172">Example F# Webjob</span></span>](https://github.com/jrr/webjob-project-examples)

## <a name="implementing-timers-on-azure-with-f"></a><span data-ttu-id="f7375-173">通过 F\# 在 Azure 上实现计时器</span><span class="sxs-lookup"><span data-stu-id="f7375-173">Implementing Timers on Azure with F\#</span></span>

<span data-ttu-id="f7375-174">计时器触发器基于计划调用一次或重复调用函数。</span><span class="sxs-lookup"><span data-stu-id="f7375-174">Timer triggers call functions based on a schedule, one time or recurring.</span></span>

<span data-ttu-id="f7375-175">计时器可采用 F# 实现，并通过 [Azure Function in F# with a Timer Trigger](/azure/azure-functions/functions-bindings-timer)（具有计时器触发器的 F# Azure Function）在 Azure 上进行托管。</span><span class="sxs-lookup"><span data-stu-id="f7375-175">Timers can be implemented in F# and hosted on Azure via an [Azure Function in F# with a Timer Trigger](/azure/azure-functions/functions-bindings-timer).</span></span>

## <a name="deploying-and-managing-azure-resources-with-f-scripts"></a><span data-ttu-id="f7375-176">使用 F# 脚本部署和管理 Azure 资源</span><span class="sxs-lookup"><span data-stu-id="f7375-176">Deploying and Managing Azure Resources with F# Scripts</span></span>

<span data-ttu-id="f7375-177">可使用 Microsoft.Azure.Management 包和 API，以编程方式通过 F# 脚本部署和管理 Azure VM。</span><span class="sxs-lookup"><span data-stu-id="f7375-177">Azure VMs may be programmatically deployed and managed from F# scripts by using the Microsoft.Azure.Management packages and APIs.</span></span> <span data-ttu-id="f7375-178">有关示例，请参阅 [.NET 管理库入门](/previous-versions/azure/dn722415(v=azure.100))和[使用 Azure 资源管理器](/azure/azure-resource-manager/resource-manager-deployment-model)。</span><span class="sxs-lookup"><span data-stu-id="f7375-178">For example, see [Get Started with the Management Libraries for .NET](/previous-versions/azure/dn722415(v=azure.100)) and [Using Azure Resource Manager](/azure/azure-resource-manager/resource-manager-deployment-model).</span></span>

<span data-ttu-id="f7375-179">同样，其他 Azure 资源也可用相同的组件通过 F# 脚本进行部署和管理。</span><span class="sxs-lookup"><span data-stu-id="f7375-179">Likewise, other Azure resources may also be deployed and managed from F# scripts using the same components.</span></span> <span data-ttu-id="f7375-180">例如，可通过 F# 脚本以编程方式创建存储帐户、部署 Azure 云服务、创建 Azure Cosmos DB 实例以及管理 Azure 通知中心。</span><span class="sxs-lookup"><span data-stu-id="f7375-180">For example, you can create storage accounts, deploy Azure Cloud Services, create Azure Cosmos DB instances and manage Azure Notification Hubs programmatically from F# scripts.</span></span>

<span data-ttu-id="f7375-181">通常，使用 F# 脚本来部署和管理资源并不必要。</span><span class="sxs-lookup"><span data-stu-id="f7375-181">Using F# scripts to deploy and manage resources is not normally necessary.</span></span> <span data-ttu-id="f7375-182">例如，Azure 资源也可直接通过 JSON 模板说明进行部署，可对该模板进行参数化。</span><span class="sxs-lookup"><span data-stu-id="f7375-182">For example, Azure resources may also be deployed directly from JSON template descriptions, which can be parameterized.</span></span> <span data-ttu-id="f7375-183">请参阅 [Azure 资源管理器模板](/azure/azure-resource-manager/resource-manager-template-best-practices)，包括 [Azure 快速入门模板](https://azure.microsoft.com/resources/templates/)等示例。</span><span class="sxs-lookup"><span data-stu-id="f7375-183">See [Azure Resource Manager Templates](/azure/azure-resource-manager/resource-manager-template-best-practices) including examples such as the [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/).</span></span>

## <a name="other-resources"></a><span data-ttu-id="f7375-184">其他资源</span><span class="sxs-lookup"><span data-stu-id="f7375-184">Other resources</span></span>

* [<span data-ttu-id="f7375-185">有关所有 Azure 服务的完整文档</span><span class="sxs-lookup"><span data-stu-id="f7375-185">Full documentation on all Azure services</span></span>](/azure/)
