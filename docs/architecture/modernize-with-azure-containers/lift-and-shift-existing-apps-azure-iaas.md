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
# <a name="lift-and-shift-existing-net-apps-to-azure-iaas-cloud-infrastructure-ready"></a>将现有 .NET 应用直接迁移到 Azure IaaS（云基础结构就绪）

> 愿景：第一步，若要减少本地投资以及硬件和网络维护的总成本，只需在云中重新托管现有应用程序。

在了解如何将现有应用程序迁移到 Azure 基础结构即服务 (IaaS) 平台之前，请务必分析直接迁移到 Azure 中 IaaS 的原因   。 此现代化成熟度级别的方案实质上是开始使用云中的 VM，而不是继续使用当前的本地基础结构。

要分析的另一点是可能想要迁移到纯 IaaS 云，而不只是在 Azure 中添加更高级的托管服务的原因  。 首先确定哪些情况可能需要 IaaS。

图 2-1 将云基础结构就绪应用程序定位在现代化成熟度级别：

![定位云基础结构就绪应用程序](./media/image2-1.png)

**图 2-1.** 定位云基础结构就绪应用程序

## <a name="why-migrate-existing-net-web-applications-to-azure-iaas"></a>为何要将现有 .NET Web 应用程序迁移到 Azure IaaS

迁移到云（即使在初始 IaaS 级别）的主要原因是实现成本降低。 通过使用更多托管基础结构服务，你的组织可以降低对硬件维护、服务器或 VM 预配和部署以及基础结构管理的投资。

决定将应用迁移到云后，选择 IaaS 而不是更高级的选项（如 PaaS）的主要原因就是，IaaS 环境会更熟悉。 迁移到类似于当前本地环境的环境提供较低的学习曲线，这使它成为迁移到云的最快途径。

但是，采用迁移到云的最快途径并不意味着将受益于在云中运行应用程序。 任何组织都将从已推出的云优化和云本机成熟度级别的云迁移中获得最大的好处。

此外，如果应用程序已在云中运行，甚至在 IaaS 上运行，则很明显更易于现代化和重新架构应用程序。 已实现应用程序数据迁移。 此外，你的组织将获得在云中工作所需的技能，并转移为在“云文化”中运营。

## <a name="when-to-migrate-to-iaas-instead-of-to-paas"></a>何时迁移到 IaaS 而不是迁移到 PaaS

接下来的部分讨论大多基于 PaaS 平台和服务的云优化应用程序。 这些应用为你带来了迁移到云的最大好处。

如果你的目标只是将现有应用程序迁移到云，请先确定不需要进行大量修改即可在 Azure 应用服务中运行的现有应用程序。 这些应用应是云优化的第一候选项。

然后，对于仍无法迁移到 Windows 容器和 PaaS（如应用服务）或业务流程协调程序（如 Azure Kubernetes 服务）的应用，请将其迁移到简单的普通 VM (IaaS)。

但请考虑到这一点，即相比于使用 Azure 中的 PaaS 服务，正确配置、保护和维护 VM 需要更多的时间和 IT 专业知识。 如果考虑采用 Azure 虚拟机，请确保将修补、更新和管理 VM 环境所需的持续性维护工作纳入考虑。 Azure 虚拟机是 IaaS。

## <a name="use-azure-migrate-to-analyze-and-migrate-your-existing-applications-to-azure"></a>使用 Azure Migrate 分析现有应用程序并将其迁移到 Azure

迁移到云不一定很困难。 但许多组织很难开始深入了解环境以及应用程序、工作负荷和数据之间的紧密相互依赖性。 如果对此没有了解，则很难规划未来的路。 如果没有关于成功迁移所需条件的详细信息，则无法与组织进行正确的对话。 你并不了解潜在的成本优势，也不了解工作负荷是可以直接迁移，还是需要大量返工才能成功迁移。 难怪许多组织会犹豫。

[Azure Migrate](https://aka.ms/azuremigrate) 是一项新服务，可提供帮助迁移到 Azure 所需的指导、见解和机制。 Azure Migrate 提供：

- 本地虚拟机的发现和评估

- 多层应用程序的高可信度发现的内置依赖项映射

- 对 Azure 虚拟机进行智能适当调整大小

- 包含补救潜在问题的指南的兼容性报告

- 与 Azure 数据库管理服务集成以实现数据库发现和迁移

Azure Migrate 使你确信工作负荷可以在对业务的影响最小的情况下进行迁移，并在 Azure 中按预期运行。 借助正确的工具和指南，可以实现最大投资回报率，同时确保满足关键性能和可靠性需求。

图 2-2 显示了 Azure Migrate 执行的所有服务器和应用程序连接的内置依赖项映射。

![服务器和应用程序连接的依赖项](./media/image2-2.png)

**图 2-2。** 定位云基础结构就绪应用程序

## <a name="use-azure-site-recovery-to-migrate-your-existing-vms-to-azure-vms"></a>使用 Azure Site Recovery 将现有 VM 迁移到 Azure VM

作为端到端 [Azure Migrate](https://aka.ms/azuremigrate) 的一部分，[Azure Site Recovery](/azure/site-recovery/site-recovery-overview) 是一种可用于轻松地将 Web 应用迁移到 Azure 中的 VM 的工具。 可以使用 Site Recovery 将本地 VM 和物理服务器复制到 Azure 或辅助性的本地站点。 甚至可以复制在受支持的 Azure VM、本地 Hyper-V VM、VMware VM 或 Windows 或 Linux 物理服务器上运行的工作负荷   。 将数据复制到 Azure 以后，就不需进行复杂的辅助数据中心维护，从而消除相关成本。

Site Recovery 也专门用于部分在本地、部分在 Azure 上的混合环境。 Site Recovery 可以在站点出现故障时，让应用始终在 VM 上运行，让本地物理服务器始终可用，从而确保业务连续性。 它可以复制在 VM 和物理服务器上运行的工作负荷，因此当主站点不可用时，始终可以在次要位置使用这些工作负荷。 当主站点重新启动并运行时，它会将工作负荷恢复到主站点。

图 2-3 显示了如何使用 Azure Site Recovery 执行多个 VM 迁移。

![使用 Azure Site Recovery 迁移 VM](./media/image2-3.png)

**图 2-3**。 定位云基础结构就绪应用程序

### <a name="additional-resources"></a>其他资源

- **Azure Migrate Datasheet**

    <https://aka.ms/azuremigration\_datasheet>

- **Azure Migrate**

    <https://aka.ms/azuremigrate>

- **Azure 迁移中心**

    <https://azure.microsoft.com/migration/>

- **使用 Site Recovery 迁移到 Azure**

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-migrate-to-azure>

- **Azure Site Recovery 服务概述**

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-overview>

- **将 AWS 中的 VM 迁移到 Azure VM**

    <https://docs.microsoft.com/azure/site-recovery/site-recovery-migrate-aws-to-azure>

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](migrate-your-relational-databases-to-azure.md) <!-- Next Chapter -->
