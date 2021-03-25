---
title: 将现有 .NET 应用直接迁移到 Azure IaaS（云基础结构就绪）
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序。
ms.date: 12/21/2020
ms.openlocfilehash: 69e89702b41cd7388ee6c90e77efafc7cd2bebe8
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605108"
---
# <a name="lift-and-shift-existing-net-apps-to-azure-iaas-cloud-infrastructure-ready"></a><span data-ttu-id="c880e-103">将现有 .NET 应用直接迁移到 Azure IaaS（云基础结构就绪）</span><span class="sxs-lookup"><span data-stu-id="c880e-103">Lift and shift existing .NET apps to Azure IaaS (Cloud Infrastructure-Ready)</span></span>

> <span data-ttu-id="c880e-104">愿景：第一步，若要减少本地投资以及硬件和网络维护的总成本，只需在云中重新托管现有应用程序。</span><span class="sxs-lookup"><span data-stu-id="c880e-104">Vision: As a first step, to reduce your on-premises investment and total cost of hardware and networking maintenance, simply rehost your existing applications in the cloud.</span></span>

<span data-ttu-id="c880e-105">在了解如何将现有应用程序迁移到 Azure 基础结构即服务 (IaaS) 平台之前，请务必分析直接迁移到 Azure 中 IaaS 的原因   。</span><span class="sxs-lookup"><span data-stu-id="c880e-105">Before getting into *how* to migrate your existing applications to the Azure infrastructure as a service (IaaS) platform, it's important to analyze the reasons *why* you'd want to migrate directly to IaaS in Azure.</span></span> <span data-ttu-id="c880e-106">此现代化成熟度级别的方案实质上是开始使用云中的 VM，而不是继续使用当前的本地基础结构。</span><span class="sxs-lookup"><span data-stu-id="c880e-106">The scenario at this modernization maturity level essentially is to start using VMs in the cloud, instead of continuing to use your current, on-premises infrastructure.</span></span>

<span data-ttu-id="c880e-107">要分析的另一点是可能想要迁移到纯 IaaS 云，而不只是在 Azure 中添加更高级的托管服务的原因  。</span><span class="sxs-lookup"><span data-stu-id="c880e-107">Another point to analyze is *why* you might want to migrate to pure IaaS cloud instead of just adding more advanced managed services in Azure.</span></span> <span data-ttu-id="c880e-108">首先确定哪些情况可能需要 IaaS。</span><span class="sxs-lookup"><span data-stu-id="c880e-108">Determine what cases might require IaaS in the first place.</span></span>

<span data-ttu-id="c880e-109">图 2-1 将云基础结构就绪应用程序定位在现代化成熟度级别：</span><span class="sxs-lookup"><span data-stu-id="c880e-109">Figure 2-1 positions Cloud Infrastructure-Ready applications in the modernization maturity levels:</span></span>

![定位云基础结构就绪应用程序](./media/image2-1.png)

<span data-ttu-id="c880e-111">**图 2-1.**</span><span class="sxs-lookup"><span data-stu-id="c880e-111">**Figure 2-1.**</span></span> <span data-ttu-id="c880e-112">定位云基础结构就绪应用程序</span><span class="sxs-lookup"><span data-stu-id="c880e-112">Positioning Cloud Infrastructure-Ready applications</span></span>

## <a name="why-migrate-existing-net-web-applications-to-azure-iaas"></a><span data-ttu-id="c880e-113">为何要将现有 .NET Web 应用程序迁移到 Azure IaaS</span><span class="sxs-lookup"><span data-stu-id="c880e-113">Why migrate existing .NET web applications to Azure IaaS</span></span>

<span data-ttu-id="c880e-114">迁移到云（即使在初始 IaaS 级别）的主要原因是实现成本降低。</span><span class="sxs-lookup"><span data-stu-id="c880e-114">The main reason to migrate to the cloud, even at an initial IaaS level, is to achieve cost reductions.</span></span> <span data-ttu-id="c880e-115">通过使用更多托管基础结构服务，你的组织可以降低对硬件维护、服务器或 VM 预配和部署以及基础结构管理的投资。</span><span class="sxs-lookup"><span data-stu-id="c880e-115">By using more managed infrastructure services, your organization can lower its investment in hardware maintenance, server or VM provisioning and deployment, and infrastructure management.</span></span>

<span data-ttu-id="c880e-116">决定将应用迁移到云后，选择 IaaS 而不是更高级的选项（如 PaaS）的主要原因就是，IaaS 环境会更熟悉。</span><span class="sxs-lookup"><span data-stu-id="c880e-116">After you make the decision to move your apps to the cloud, the main reason why you might choose IaaS instead of more advanced options like PaaS is simply that the IaaS environment will be more familiar.</span></span> <span data-ttu-id="c880e-117">迁移到类似于当前本地环境的环境提供较低的学习曲线，这使它成为迁移到云的最快途径。</span><span class="sxs-lookup"><span data-stu-id="c880e-117">Moving to an environment that's similar to your current, on-premises environment offers a lower learning curve, which makes it the quickest path to the cloud.</span></span>

<span data-ttu-id="c880e-118">但是，采用迁移到云的最快途径并不意味着将受益于在云中运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="c880e-118">However, taking the quickest path to the cloud doesn't mean that you will gain the most benefit from having your applications running in the cloud.</span></span> <span data-ttu-id="c880e-119">任何组织都将从已推出的云优化和云本机成熟度级别的云迁移中获得最大的好处。</span><span class="sxs-lookup"><span data-stu-id="c880e-119">Any organization will gain the most significant benefits from a cloud migration at the already introduced Cloud-Optimized and Cloud-Native maturity levels.</span></span>

<span data-ttu-id="c880e-120">此外，如果应用程序已在云中运行，甚至在 IaaS 上运行，则很明显更易于现代化和重新架构应用程序。</span><span class="sxs-lookup"><span data-stu-id="c880e-120">It also has become evident that applications are easier to modernize and rearchitect in the future when they are already running in the cloud, even on IaaS.</span></span> <span data-ttu-id="c880e-121">已实现应用程序数据迁移。</span><span class="sxs-lookup"><span data-stu-id="c880e-121">Application data migration has already been achieved.</span></span> <span data-ttu-id="c880e-122">此外，你的组织将获得在云中工作所需的技能，并转移为在“云文化”中运营。</span><span class="sxs-lookup"><span data-stu-id="c880e-122">Also, your organization will have gained the skills required for working in the cloud and made the shift to operating in a "cloud culture."</span></span>

## <a name="when-to-migrate-to-iaas-instead-of-to-paas"></a><span data-ttu-id="c880e-123">何时迁移到 IaaS 而不是迁移到 PaaS</span><span class="sxs-lookup"><span data-stu-id="c880e-123">When to migrate to IaaS instead of to PaaS</span></span>

<span data-ttu-id="c880e-124">接下来的部分讨论大多基于 PaaS 平台和服务的云优化应用程序。</span><span class="sxs-lookup"><span data-stu-id="c880e-124">The next sections discuss Cloud-Optimized applications that are mostly based on PaaS platforms and services.</span></span> <span data-ttu-id="c880e-125">这些应用为你带来了迁移到云的最大好处。</span><span class="sxs-lookup"><span data-stu-id="c880e-125">These apps give you the most benefits from migrating to the cloud.</span></span>

<span data-ttu-id="c880e-126">如果你的目标只是将现有应用程序迁移到云，请先确定不需要进行大量修改即可在 Azure 应用服务中运行的现有应用程序。</span><span class="sxs-lookup"><span data-stu-id="c880e-126">If your goal is simply to move existing applications to the cloud, first, identify existing applications that would not require substantial modification to run in Azure App Service.</span></span> <span data-ttu-id="c880e-127">这些应用应是云优化的第一候选项。</span><span class="sxs-lookup"><span data-stu-id="c880e-127">These apps should be the first candidates for Cloud-Optimized.</span></span>

<span data-ttu-id="c880e-128">然后，对于仍无法迁移到 Windows 容器和 PaaS（如应用服务）或业务流程协调程序（如 Azure Kubernetes 服务）的应用，请将其迁移到简单的普通 VM (IaaS)。</span><span class="sxs-lookup"><span data-stu-id="c880e-128">Then, for the apps that still cannot move to Windows Containers and PaaS such as App Service or orchestrators like Azure Kubernetes Service, migrate those apps to simple plain VMs (IaaS).</span></span>

<span data-ttu-id="c880e-129">但请考虑到这一点，即相比于使用 Azure 中的 PaaS 服务，正确配置、保护和维护 VM 需要更多的时间和 IT 专业知识。</span><span class="sxs-lookup"><span data-stu-id="c880e-129">But, keep in mind that correctly configuring, securing, and maintaining VMs requires much more time and IT expertise compared to using PaaS services in Azure.</span></span> <span data-ttu-id="c880e-130">如果考虑采用 Azure 虚拟机，请确保将修补、更新和管理 VM 环境所需的持续性维护工作纳入考虑。</span><span class="sxs-lookup"><span data-stu-id="c880e-130">If you are considering Azure Virtual Machines, make sure that you take into account the ongoing maintenance effort required to patch, update, and manage your VM environment.</span></span> <span data-ttu-id="c880e-131">Azure 虚拟机是 IaaS。</span><span class="sxs-lookup"><span data-stu-id="c880e-131">Azure Virtual Machines is IaaS.</span></span>

## <a name="use-azure-migrate-to-analyze-and-migrate-your-existing-applications-to-azure"></a><span data-ttu-id="c880e-132">使用 Azure Migrate 分析现有应用程序并将其迁移到 Azure</span><span class="sxs-lookup"><span data-stu-id="c880e-132">Use Azure Migrate to analyze and migrate your existing applications to Azure</span></span>

<span data-ttu-id="c880e-133">迁移到云不一定很困难。</span><span class="sxs-lookup"><span data-stu-id="c880e-133">Migrating to the cloud doesn't have to be difficult.</span></span> <span data-ttu-id="c880e-134">但许多组织很难开始深入了解环境以及应用程序、工作负荷和数据之间的紧密相互依赖性。</span><span class="sxs-lookup"><span data-stu-id="c880e-134">But many organizations struggle to get started - to get deep visibility into the environment and the tight interdependencies between applications, workloads, and data.</span></span> <span data-ttu-id="c880e-135">如果对此没有了解，则很难规划未来的路。</span><span class="sxs-lookup"><span data-stu-id="c880e-135">Without that visibility, it can be difficult to plan the path forward.</span></span> <span data-ttu-id="c880e-136">如果没有关于成功迁移所需条件的详细信息，则无法与组织进行正确的对话。</span><span class="sxs-lookup"><span data-stu-id="c880e-136">Without detailed information on what's required for a successful migration, you can't have the right conversations within your organization.</span></span> <span data-ttu-id="c880e-137">你并不了解潜在的成本优势，也不了解工作负荷是可以直接迁移，还是需要大量返工才能成功迁移。</span><span class="sxs-lookup"><span data-stu-id="c880e-137">You don't know enough about the potential cost benefits, or whether workloads could just lift-and-shift or would require significant rework to migrate successfully.</span></span> <span data-ttu-id="c880e-138">难怪许多组织会犹豫。</span><span class="sxs-lookup"><span data-stu-id="c880e-138">No wonder many organizations hesitate.</span></span>

<span data-ttu-id="c880e-139">[Azure Migrate](https://aka.ms/azuremigrate) 是一项新服务，可提供帮助迁移到 Azure 所需的指导、见解和机制。</span><span class="sxs-lookup"><span data-stu-id="c880e-139">[Azure Migrate](https://aka.ms/azuremigrate) is a new service that provides the guidance, insights, and mechanisms needed to assist you in migrating to Azure.</span></span> <span data-ttu-id="c880e-140">Azure Migrate 提供：</span><span class="sxs-lookup"><span data-stu-id="c880e-140">Azure Migrate provides:</span></span>

- <span data-ttu-id="c880e-141">本地虚拟机的发现和评估</span><span class="sxs-lookup"><span data-stu-id="c880e-141">Discovery and assessment for on-premises virtual machines</span></span>

- <span data-ttu-id="c880e-142">多层应用程序的高可信度发现的内置依赖项映射</span><span class="sxs-lookup"><span data-stu-id="c880e-142">Inbuilt dependency mapping for high-confidence discovery of multi-tier applications</span></span>

- <span data-ttu-id="c880e-143">对 Azure 虚拟机进行智能适当调整大小</span><span class="sxs-lookup"><span data-stu-id="c880e-143">Intelligent right sizing to Azure virtual machines</span></span>

- <span data-ttu-id="c880e-144">包含补救潜在问题的指南的兼容性报告</span><span class="sxs-lookup"><span data-stu-id="c880e-144">Compatibility reporting with guidelines for remediating potential issues</span></span>

- <span data-ttu-id="c880e-145">与 Azure 数据库管理服务集成以实现数据库发现和迁移</span><span class="sxs-lookup"><span data-stu-id="c880e-145">Integration with Azure Database Management Service for database discovery and migration</span></span>

<span data-ttu-id="c880e-146">Azure Migrate 使你确信工作负荷可以在对业务的影响最小的情况下进行迁移，并在 Azure 中按预期运行。</span><span class="sxs-lookup"><span data-stu-id="c880e-146">Azure Migrate gives you confidence that your workloads can migrate with minimal impact to the business and run as expected in Azure.</span></span> <span data-ttu-id="c880e-147">借助正确的工具和指南，可以实现最大投资回报率，同时确保满足关键性能和可靠性需求。</span><span class="sxs-lookup"><span data-stu-id="c880e-147">With the right tools and guidance, you can achieve maximum return on investment while assuring that critical performance and reliability needs are met.</span></span>

<span data-ttu-id="c880e-148">图 2-2 显示了 Azure Migrate 执行的所有服务器和应用程序连接的内置依赖项映射。</span><span class="sxs-lookup"><span data-stu-id="c880e-148">Figure 2-2 shows you the built-in dependency mapping for all server and application connections performed by Azure Migrate.</span></span>

![服务器和应用程序连接的依赖项](./media/image2-2.png)

<span data-ttu-id="c880e-150">**图 2-2。**</span><span class="sxs-lookup"><span data-stu-id="c880e-150">**Figure 2-2.**</span></span> <span data-ttu-id="c880e-151">定位云基础结构就绪应用程序</span><span class="sxs-lookup"><span data-stu-id="c880e-151">Positioning Cloud Infrastructure-Ready applications</span></span>

## <a name="use-azure-site-recovery-to-migrate-your-existing-vms-to-azure-vms"></a><span data-ttu-id="c880e-152">使用 Azure Site Recovery 将现有 VM 迁移到 Azure VM</span><span class="sxs-lookup"><span data-stu-id="c880e-152">Use Azure Site Recovery to migrate your existing VMs to Azure VMs</span></span>

<span data-ttu-id="c880e-153">作为端到端 [Azure Migrate](https://aka.ms/azuremigrate) 的一部分，[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) 是一种可用于轻松地将 Web 应用迁移到 Azure 中的 VM 的工具。</span><span class="sxs-lookup"><span data-stu-id="c880e-153">As part of the end-to-end [Azure Migrate](https://aka.ms/azuremigrate), [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) is a tool that you can use to easily migrate your web apps to VMs in Azure.</span></span> <span data-ttu-id="c880e-154">可以使用 Site Recovery 将本地 VM 和物理服务器复制到 Azure 或辅助性的本地站点。</span><span class="sxs-lookup"><span data-stu-id="c880e-154">You can use Site Recovery to replicate on-premises VMs and physical servers to Azure, or to replicate them to a secondary on-premises location.</span></span> <span data-ttu-id="c880e-155">甚至可以复制在受支持的 Azure VM、本地 Hyper-V VM、VMware VM 或 Windows 或 Linux 物理服务器上运行的工作负荷   。</span><span class="sxs-lookup"><span data-stu-id="c880e-155">You can even replicate a workload that's running on a supported Azure VM, on an on-premises *Hyper-V* VM, on a *VMware* VM, or on a Windows or Linux physical server.</span></span> <span data-ttu-id="c880e-156">将数据复制到 Azure 以后，就不需进行复杂的辅助数据中心维护，从而消除相关成本。</span><span class="sxs-lookup"><span data-stu-id="c880e-156">Replication to Azure eliminates the cost and complexity of maintaining a secondary datacenter.</span></span>

<span data-ttu-id="c880e-157">Site Recovery 也专门用于部分在本地、部分在 Azure 上的混合环境。</span><span class="sxs-lookup"><span data-stu-id="c880e-157">Site Recovery is also made specifically for hybrid environments that are partly on-premises and partly on Azure.</span></span> <span data-ttu-id="c880e-158">Site Recovery 可以在站点出现故障时，让应用始终在 VM 上运行，让本地物理服务器始终可用，从而确保业务连续性。</span><span class="sxs-lookup"><span data-stu-id="c880e-158">Site Recovery helps ensure business continuity by keeping your apps that are running on VMs and on-premises physical servers available if a site goes down.</span></span> <span data-ttu-id="c880e-159">它可以复制在 VM 和物理服务器上运行的工作负荷，因此当主站点不可用时，始终可以在次要位置使用这些工作负荷。</span><span class="sxs-lookup"><span data-stu-id="c880e-159">It replicates workloads that are running on VMs and physical servers so that they remain available in a secondary location if the primary site isn't available.</span></span> <span data-ttu-id="c880e-160">当主站点重新启动并运行时，它会将工作负荷恢复到主站点。</span><span class="sxs-lookup"><span data-stu-id="c880e-160">It recovers workloads to the primary site when it's up and running again.</span></span>

<span data-ttu-id="c880e-161">图 2-3 显示了如何使用 Azure Site Recovery 执行多个 VM 迁移。</span><span class="sxs-lookup"><span data-stu-id="c880e-161">Figure 2-3 shows the execution of multiple VM migrations by using Azure Site Recovery.</span></span>

![使用 Azure Site Recovery 迁移 VM](./media/image2-3.png)

<span data-ttu-id="c880e-163">**图 2-3**。</span><span class="sxs-lookup"><span data-stu-id="c880e-163">**Figure 2-3.**</span></span> <span data-ttu-id="c880e-164">定位云基础结构就绪应用程序</span><span class="sxs-lookup"><span data-stu-id="c880e-164">Positioning Cloud Infrastructure-Ready applications</span></span>

### <a name="additional-resources"></a><span data-ttu-id="c880e-165">其他资源</span><span class="sxs-lookup"><span data-stu-id="c880e-165">Additional resources</span></span>

- <span data-ttu-id="c880e-166">**Azure Migrate Datasheet**</span><span class="sxs-lookup"><span data-stu-id="c880e-166">**Azure Migrate Datasheet**</span></span>

    <https://aka.ms/azuremigration\_datasheet>

- <span data-ttu-id="c880e-167">**Azure Migrate**</span><span class="sxs-lookup"><span data-stu-id="c880e-167">**Azure Migrate**</span></span>

    <https://aka.ms/azuremigrate>

- <span data-ttu-id="c880e-168">**Azure 迁移中心**</span><span class="sxs-lookup"><span data-stu-id="c880e-168">**Azure migration center**</span></span>

    <https://azure.microsoft.com/migration/>

- <span data-ttu-id="c880e-169">**使用 Site Recovery 迁移到 Azure**</span><span class="sxs-lookup"><span data-stu-id="c880e-169">**Migrate to Azure with Site Recovery**</span></span>

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-migrate-to-azure>

- <span data-ttu-id="c880e-170">**Azure Site Recovery 服务概述**</span><span class="sxs-lookup"><span data-stu-id="c880e-170">**Azure Site Recovery service overview**</span></span>

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-overview>

- <span data-ttu-id="c880e-171">**将 AWS 中的 VM 迁移到 Azure VM**</span><span class="sxs-lookup"><span data-stu-id="c880e-171">**Migrating VMs in AWS to Azure VMs**</span></span>

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-migrate-aws-to-azure>

>[!div class="step-by-step"]
><span data-ttu-id="c880e-172">[上一页](index.md)
>[下一页](migrate-your-relational-databases-to-azure.md)</span><span class="sxs-lookup"><span data-stu-id="c880e-172">[Previous](index.md)
[Next](migrate-your-relational-databases-to-azure.md)</span></span> <!-- Next Chapter -->
