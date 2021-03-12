---
title: 将 eShopOnContainers 部署到 Azure
description: 使用 Azure Kubernetes 服务、Helm 和 DevSpaces 部署 eShopOnContainers 应用程序。
ms.date: 01/19/2021
ms.openlocfilehash: da68fa1510841b6cef9117b6d84e7ed30c565698
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604562"
---
# <a name="deploying-eshoponcontainers-to-azure"></a><span data-ttu-id="0cf8e-103">将 eShopOnContainers 部署到 Azure</span><span class="sxs-lookup"><span data-stu-id="0cf8e-103">Deploying eShopOnContainers to Azure</span></span>

<span data-ttu-id="0cf8e-104">可以将 eShopOnContainers 应用程序部署到各种 Azure 平台。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-104">The eShopOnContainers application can be deployed to various Azure platforms.</span></span> <span data-ttu-id="0cf8e-105">推荐的方法是将应用程序部署到 Azure Kubernetes Services (AKS) 。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-105">The recommended approach is to deploy the application to Azure Kubernetes Services (AKS).</span></span> <span data-ttu-id="0cf8e-106">Helm 是一种 Kubernetes 部署工具，可用于降低部署复杂性。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-106">Helm, a Kubernetes deployment tool, is available to reduce deployment complexity.</span></span> <span data-ttu-id="0cf8e-107">开发人员可以根据需要实现 Kubernetes 的 Azure Dev Spaces，以简化开发过程。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-107">Optionally, developers may implement Azure Dev Spaces for Kubernetes to streamline their development process.</span></span>

## <a name="azure-kubernetes-service"></a><span data-ttu-id="0cf8e-108">Azure Kubernetes 服务</span><span class="sxs-lookup"><span data-stu-id="0cf8e-108">Azure Kubernetes Service</span></span>

<span data-ttu-id="0cf8e-109">若要将 eShop 托管在 AKS 中，第一步是创建 AKS 群集。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-109">To host eShop in AKS, the first step is to create an AKS cluster.</span></span> <span data-ttu-id="0cf8e-110">为此，你可以使用 Azure 门户，这将指导你完成所需的步骤。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-110">To do so, you might use the Azure portal, which will walk you through the required steps.</span></span> <span data-ttu-id="0cf8e-111">你还可以从 Azure CLI 创建群集，注意启用 Role-Based Access Control (RBAC) 和应用程序路由。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-111">You could also create a cluster from the Azure CLI, taking care to enable Role-Based Access Control (RBAC) and application routing.</span></span> <span data-ttu-id="0cf8e-112">EShopOnContainers 文档详细介绍了创建你自己的 AKS 群集的步骤。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-112">The eShopOnContainers' documentation details the steps for creating your own AKS cluster.</span></span> <span data-ttu-id="0cf8e-113">创建后，可以从 Kubernetes 仪表板访问和管理群集。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-113">Once created, you can access and manage the cluster from the Kubernetes dashboard.</span></span>

<span data-ttu-id="0cf8e-114">你现在可以使用 Helm 将 eShop 应用程序部署到群集。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-114">You can now deploy the eShop application to the cluster using Helm.</span></span>

## <a name="deploying-to-azure-kubernetes-service-using-helm"></a><span data-ttu-id="0cf8e-115">使用 Helm 部署到 Azure Kubernetes 服务</span><span class="sxs-lookup"><span data-stu-id="0cf8e-115">Deploying to Azure Kubernetes Service using Helm</span></span>

<span data-ttu-id="0cf8e-116">Helm 是直接与 Kubernetes 配合使用的应用程序包管理器工具。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-116">Helm is an application package manager tool that works directly with Kubernetes.</span></span> <span data-ttu-id="0cf8e-117">它可帮助你定义、安装和升级 Kubernetes 应用程序。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-117">It helps you define, install, and upgrade Kubernetes applications.</span></span> <span data-ttu-id="0cf8e-118">虽然可通过自定义 CLI 脚本或简单的部署文件将简单应用部署到 AKS，但复杂应用可以包含许多 Kubernetes 对象，并受益于 Helm。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-118">While simple apps can be deployed to AKS with custom CLI scripts or simple deployment files, complex apps can contain many Kubernetes objects and benefit from Helm.</span></span>

<span data-ttu-id="0cf8e-119">使用 Helm，应用程序包括基于文本的配置文件（称为 Helm 图），以声明方式描述 Helm 包中的应用程序和配置。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-119">Using Helm, applications include text-based configuration files, called Helm charts, which declaratively describe the application and configuration in Helm packages.</span></span> <span data-ttu-id="0cf8e-120">图表使用标准 YAML 格式的文件来描述一组相关的 Kubernetes 资源。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-120">Charts use standard YAML-formatted files to describe a related set of Kubernetes resources.</span></span> <span data-ttu-id="0cf8e-121">它们与它们所描述的应用程序代码一起版本化。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-121">They're versioned alongside the application code they describe.</span></span> <span data-ttu-id="0cf8e-122">Helm 图的范围从简单到复杂，具体取决于它们所描述的安装的要求。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-122">Helm Charts range from simple to complex depending on the requirements of the installation they describe.</span></span>

<span data-ttu-id="0cf8e-123">Helm 由命令行客户端工具组成，该工具使用 Helm 图表，并将命令启动到名为 Tiller 的服务器组件。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-123">Helm is composed of a command-line client tool, which consumes helm charts and launches commands to a server component named, Tiller.</span></span> <span data-ttu-id="0cf8e-124">Tiller 与 Kubernetes API 通信，以确保正确预配容器化工作负荷。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-124">Tiller communicates with the Kubernetes API to ensure the correct provisioning of your containerized workloads.</span></span> <span data-ttu-id="0cf8e-125">Helm 由云本机计算基础维护。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-125">Helm is maintained by the Cloud-native Computing Foundation.</span></span>

<span data-ttu-id="0cf8e-126">以下 yaml 文件提供了 Helm 模板：</span><span class="sxs-lookup"><span data-stu-id="0cf8e-126">The following yaml file presents a Helm template:</span></span>

```yaml
apiVersion: v1
kind: Service
metadata:
  name: {{ .Values.app.svc.marketing }}
  labels:
    app: {{ template "marketing-api.name" . }}
    chart: {{ template "marketing-api.chart" . }}
    release: {{ .Release.Name }}
    heritage: {{ .Release.Service }}
spec:
  type: {{ .Values.service.type }}
  ports:
    - port: {{ .Values.service.port }}
      targetPort: http
      protocol: TCP
      name: http
  selector:
    app: {{ template "marketing-api.name" . }}
    release: {{ .Release.Name }}
```

<span data-ttu-id="0cf8e-127">请注意模板如何描述一组动态的键/值对。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-127">Note how the template describes a dynamic set of key/value pairs.</span></span> <span data-ttu-id="0cf8e-128">调用模板时，括在大括号中的值从其他基于 yaml 的配置文件中提取。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-128">When the template is invoked, values that enclosed in curly braces are pulled in from other yaml-based configuration files.</span></span>

<span data-ttu-id="0cf8e-129">你将在/k8s/helm 文件夹中找到 eShopOnContainers helm 图。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-129">You'll find the eShopOnContainers helm charts in the /k8s/helm folder.</span></span> <span data-ttu-id="0cf8e-130">图2-6 显示了如何将应用程序的不同组件组织到 helm 用于定义和管理部署的文件夹结构中。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-130">Figure 2-6 shows how the different components of the application are organized into a folder structure used by helm to define and managed deployments.</span></span>

<span data-ttu-id="0cf8e-131">![EShopOnContainers helm 文件夹 ](./media/eshoponcontainers-helm-folder.png)
 **图 2-6**。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-131">![The eShopOnContainers helm folder](./media/eshoponcontainers-helm-folder.png)
**Figure 2-6**.</span></span> <span data-ttu-id="0cf8e-132">EShopOnContainers helm 文件夹。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-132">The eShopOnContainers helm folder.</span></span>

<span data-ttu-id="0cf8e-133">使用命令安装每个单独的组件 `helm install` 。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-133">Each individual component is installed using a `helm install` command.</span></span> <span data-ttu-id="0cf8e-134">eShop 包括一个 "部署全部" 脚本，该脚本循环遍历并使用各自的 helm 图表安装组件。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-134">eShop includes a "deploy all" script that loops through and installs the components using their respective helm charts.</span></span> <span data-ttu-id="0cf8e-135">结果就是一个可重复执行的过程，该过程与源代码管理中的应用程序进行了版本控制，团队中的任何人都可以使用单行脚本命令将其部署到 AKS 群集。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-135">The result is a repeatable process, versioned with the application in source control, that anyone on the team can deploy to an AKS cluster with a one-line script command.</span></span>

> <span data-ttu-id="0cf8e-136">请注意，Helm 的版本3不需要 Tiller 服务器组件。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-136">Note that version 3 of Helm officially removes the need for the Tiller server component.</span></span> <span data-ttu-id="0cf8e-137">可在 [此处](https://medium.com/better-programming/why-is-tiller-missing-in-helm-3-2347c446714)找到有关此增强功能的详细信息。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-137">More information on this enhancement can be found [here](https://medium.com/better-programming/why-is-tiller-missing-in-helm-3-2347c446714).</span></span>

## <a name="azure-dev-spaces"></a><span data-ttu-id="0cf8e-138">Azure Dev Spaces</span><span class="sxs-lookup"><span data-stu-id="0cf8e-138">Azure Dev Spaces</span></span>

<span data-ttu-id="0cf8e-139">云本机应用程序可以快速增长大而复杂，需要大量计算资源才能运行。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-139">Cloud-native applications can quickly grow large and complex, requiring significant compute resources to run.</span></span> <span data-ttu-id="0cf8e-140">在这些情况下，整个应用程序不能在开发计算机上承载 (尤其是便携式计算机) 。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-140">In these scenarios, the entire application can't be hosted on a development machine (especially a laptop).</span></span> <span data-ttu-id="0cf8e-141">Azure Dev Spaces 旨在使用 AKS 解决此问题。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-141">Azure Dev Spaces is designed to address this problem using AKS.</span></span> <span data-ttu-id="0cf8e-142">它使开发人员能够在 AKS 开发群集中托管应用程序的其余部分，并使用其服务的本地版本。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-142">It enables developers to work with a local version of their services while hosting the rest of the application in an AKS development cluster.</span></span>

<span data-ttu-id="0cf8e-143">开发人员在 AKS 群集中共享正在运行 (开发) 实例，其中包含整个容器化应用程序。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-143">Developers share a running (development) instance in an AKS cluster that contains the entire containerized application.</span></span> <span data-ttu-id="0cf8e-144">但它们使用在计算机上设置的个人空间来本地开发其服务。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-144">But they use personal spaces set up on their machine to locally develop their services.</span></span> <span data-ttu-id="0cf8e-145">准备就绪后，它们将在 AKS 群集中从端到端测试-无需复制依赖项。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-145">When ready, they test from end-to-end in the AKS cluster - without replicating dependencies.</span></span> <span data-ttu-id="0cf8e-146">Azure Dev Spaces 将本地计算机中的代码合并到 AKS 中的服务。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-146">Azure Dev Spaces merges code from the local machine with services in AKS.</span></span> <span data-ttu-id="0cf8e-147">团队成员可以查看其更改在实际 AKS 环境中的行为方式。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-147">Team members can see how their changes will behave in a real AKS environment.</span></span> <span data-ttu-id="0cf8e-148">开发人员可以使用 Visual Studio 2017 或 Visual Studio Code 直接在 Kubernetes 中快速循环访问和调试代码。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-148">Developers can rapidly iterate and debug code directly in Kubernetes using Visual Studio 2017 or Visual Studio Code.</span></span>

<span data-ttu-id="0cf8e-149">在图2-7 中，可以看到，开发人员 Susie 已将自行车微服务的更新版本部署到了其开发人员空间。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-149">In Figure 2-7, you can see that Developer Susie has deployed an updated version of the Bikes microservice into her dev space.</span></span> <span data-ttu-id="0cf8e-150">然后，她就可以使用自定义 URL 以其空间的名称开头 (susie.s.dev.myapp.eus.azds.io) 来测试她的更改。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-150">She's then able to test her changes using a custom URL starting with the name of her space (susie.s.dev.myapp.eus.azds.io).</span></span>

<span data-ttu-id="0cf8e-151">![显示自行车微服务 ](./media/azure-devspaces-one.png)
 **图 2-7** 的 eShopOnContainers 体系结构。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-151">![eShopOnContainers Architecture showing Bikes microservice](./media/azure-devspaces-one.png)
**Figure 2-7**.</span></span> <span data-ttu-id="0cf8e-152">开发人员 Susie 将部署自己的自行车微服务版本并对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-152">Developer Susie deploys her own version of the Bikes microservice and tests it.</span></span>

<span data-ttu-id="0cf8e-153">同时，开发人员 John 正在自定义预订微服务，并需要对其更改进行测试。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-153">At the same time, developer John is customizing the Reservations microservice and needs to test his changes.</span></span> <span data-ttu-id="0cf8e-154">他将其更改部署到自己的开发空间，而不会与 Susie 的更改发生冲突，如图2-8 所示。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-154">He deploys his changes to his own dev space without conflicting with Susie's changes as shown in Figure 2-8.</span></span> <span data-ttu-id="0cf8e-155">John 然后使用自己的 URL 测试其更改，该 URL 以其空间的名称作为前缀 (john.s.dev.myapp.eus.azds.io) 。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-155">John then tests his changes using his own URL that is prefixed with the name of his space (john.s.dev.myapp.eus.azds.io).</span></span>

<span data-ttu-id="0cf8e-156">![显示 John 的保留版本的 eShopOnContainers 体系结构微服务 ](./media/azure-devspaces-two.png)
 **图 2-8**。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-156">![eShopOnContainers Architecture showing John's version of Reservations microservice](./media/azure-devspaces-two.png)
**Figure 2-8**.</span></span> <span data-ttu-id="0cf8e-157">开发人员 John 部署自己的保留版本微服务，并对其进行测试，而不会与其他开发人员发生冲突。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-157">Developer John deploys his own version of the Reservations microservice and tests it without conflicting with other developers.</span></span>

<span data-ttu-id="0cf8e-158">使用 Azure Dev Spaces，团队可以直接使用 AKS，同时对其更改进行单独的更改、部署和测试。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-158">Using Azure Dev Spaces, teams can work directly with AKS while independently changing, deploying, and testing their changes.</span></span> <span data-ttu-id="0cf8e-159">此方法减少了单独专用托管环境的需要，因为每个开发人员都有效地拥有自己的 AKS 环境。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-159">This approach reduces the need for separate dedicated hosted environments since every developer effectively has their own AKS environment.</span></span> <span data-ttu-id="0cf8e-160">开发人员可以使用其 CLI 来处理 Azure Dev Spaces 或启动其应用程序，以便直接从 Visual Studio Azure Dev Spaces。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-160">Developers can work with Azure Dev Spaces using its CLI or launch their application to Azure Dev Spaces directly from Visual Studio.</span></span> [<span data-ttu-id="0cf8e-161">了解有关 Azure Dev Spaces 工作和配置的详细信息。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-161">Learn more about how Azure Dev Spaces works and is configured.</span></span>](/azure/dev-spaces/how-dev-spaces-works)

## <a name="azure-functions-and-logic-apps-serverless"></a><span data-ttu-id="0cf8e-162">Azure Functions 和逻辑应用 (无服务器) </span><span class="sxs-lookup"><span data-stu-id="0cf8e-162">Azure Functions and Logic Apps (Serverless)</span></span>

<span data-ttu-id="0cf8e-163">EShopOnContainers 示例包括对跟踪联机市场营销活动的支持。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-163">The eShopOnContainers sample includes support for tracking online marketing campaigns.</span></span> <span data-ttu-id="0cf8e-164">Azure Function 用于跟踪给定市场活动 ID 的市场营销活动详细信息。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-164">An Azure Function is used to track marketing campaign details for a given campaign ID.</span></span> <span data-ttu-id="0cf8e-165">单个 Azure 功能更简单、更充分，而不是创建完整的微服务。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-165">Rather than creating a full microservice, a single Azure Function is simpler and sufficient.</span></span> <span data-ttu-id="0cf8e-166">Azure Functions 具有一个简单的生成和部署模型，尤其是在配置为在 Kubernetes 中运行时。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-166">Azure Functions have a simple build and deployment model, especially when configured to run in Kubernetes.</span></span> <span data-ttu-id="0cf8e-167">部署函数使用 Azure 资源管理器 (ARM) 模板和 Azure CLI 来编写脚本。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-167">Deploying the function is scripted using Azure Resource Manager (ARM) templates and the Azure CLI.</span></span> <span data-ttu-id="0cf8e-168">此活动服务不是面向客户的，它调用单个操作，使其成为 Azure Functions 的理想候选项。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-168">This campaign service isn't customer-facing and invokes a single operation, making it a great candidate for Azure Functions.</span></span> <span data-ttu-id="0cf8e-169">函数需要最少的配置，包括数据库连接字符串数据和映像基 URI 设置。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-169">The function requires minimal configuration, including a database connection string data and image base URI settings.</span></span> <span data-ttu-id="0cf8e-170">在 Azure 门户中配置 Azure Functions。</span><span class="sxs-lookup"><span data-stu-id="0cf8e-170">You configure Azure Functions in the Azure portal.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="0cf8e-171">[上一页](map-eshoponcontainers-azure-services.md)
>[下一页](centralized-configuration.md)</span><span class="sxs-lookup"><span data-stu-id="0cf8e-171">[Previous](map-eshoponcontainers-azure-services.md)
[Next](centralized-configuration.md)</span></span>
