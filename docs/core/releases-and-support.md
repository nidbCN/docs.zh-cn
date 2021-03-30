---
title: .NET 版本、补丁和支持
description: 了解 .NET 5（包括 .NET Core）和更高版本的版本、补丁和支持。
ms.date: 10/07/2020
ms.topic: overview
ms.author: tdykstra
author: tdykstra
ms.openlocfilehash: 7b207811e0dab8c613b0e43a30301a0fb07f632a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875078"
---
# <a name="releases-and-support-for-net-core-and-net-5"></a>.NET Core 和 .NET 5 的版本和支持

Microsoft 提供 .NET 5.0（和 .NET Core）及更高版本的主要版本、次要版本和服务更新（补丁）。 本文介绍版本类型、服务更新、SDK 功能区段、支持期限和支持选项。

## <a name="release-types"></a>版本类型

关于各版本类型的信息在版本号中以 major.minor.patch 格式进行编码。

例如：

* .NET Core 3.0 和 NET 5.0 是主要版本。
* .NET Core 3.1 是 .NET Core 3.0 主要版本后的第一个次要版本。
* .NET Core 3.1.7 是 .NET Core 3.1 的第七个补丁。

### <a name="major-releases"></a>主要版本

主要版本包括新增功能、新的公共 API 图面和 bug 修复。 示例包括 .NET Core 3.0 和 .NET 5.0。  由于变更的性质，这些版本预计包含中断性变更。 主要版本与先前的主要版本并行安装。

### <a name="minor-releases"></a>次要版本

次要版本也包括新功能、公共 API 图面和 bug 修复，还可能包含中断性变更。 示例包括 .NET Core 2.1 和 .NET Core 3.1。 次要版本与主要版本的区别在于变更量较小。 应用程序从 .NET Core 3.0 升级到 3.1 所经历的变更较小。 次要版本与先前的次要版本并行安装。

### <a name="servicing-updates"></a>服务更新

几乎每个月都会发布服务更新（补丁），这些更新既包含安全性 bug 修复，又包含非安全性 bug 修复。 例如，.NET Core 3.1.8 是 .NET Core 3.1 的第八个更新。 如果这些更新包含安全性修复，则会在“星期二修补日”发布，该日期始终为每月的第二个星期二。 服务更新应保持兼容性。 从 .NET Core 3.1 开始，服务更新会删除之前的更新。 例如，成功安装最新的 3.1 服务更新会删除之前的 3.1 更新。

### <a name="feature-bands-sdk-only"></a>功能区段（仅 SDK）

.NET SDK 的版本控制与 .NET 运行时略有不同。 为了与新的 Visual Studio 版本保持一致，.NET SDK 更新有时包含新的功能或新版组件（如 MSBuild 和 NuGet）。 这些新功能或组件可能与早期 SDK 更新中针对同一主要或次要版本发布的功能或组件版本不兼容。

为了区分此类更新，.NET SDK 使用功能区段的概念。 例如，第一个 .NET Core 3.1 SDK 是 3.1.100。 此版本对应于 3.1.1xx 功能区段。 功能区段在版本号第三部分的百数组中定义。 例如，3.1.101 和 3.1.201 属于两个不同功能区段的版本，而 3.1.101 和 3.1.199 则属于同一个功能区段。 安装 .NET Core SDK 3.1.101 时，会从计算机中删除 .NET Core SDK 3.1.100（如果存在）。 当在同一台计算机上安装 .NET Core SDK 3.1.200 时，不会删除 .NET Core SDK 3.1.101。

### <a name="runtime-roll-forward-and-compatibility"></a>运行时前滚和兼容性

主要和次要更新与先前的版本并行安装。 针对特定 major.minor 版本构建的应用程序会继续使用该目标运行时，即使安装了较新版本也是如此。 应用不会自动前滚以使用运行时的较新 major.minor 版本，除非你选择启用此行为。 针对 .NET Core 3.0 构建的应用程序不会在 .NET Core 3.1 上自动开始运行。 建议在部署到生产环境之前，重新生成应用并针对较新的主要或次要运行时版本进行测试。 有关详细信息，请参阅[依赖于框架的应用前滚](versions/selection.md#framework-dependent-apps-roll-forward)和[独立部署运行时前滚](deploying/runtime-patch-selection.md)。

服务更新的处理方式与主要和次要版本不同。 默认情况下，针对 .NET Core 3.1 构建的应用程序在 3.1.0 运行时上运行。 安装服务更新后，该应用程序会自动前滚，以使用较新的 3.1.1 运行时。 此行为是默认行为，因为我们希望在安装安全性修复后立即使用这些修复，而不需要执行任何其他操作。 你可以选择禁用此默认前滚行为。

## <a name="net-core-and-net-5-version-lifecycles"></a>.NET Core 和 .NET 5 版本生命周期

.NET Core、.NET 5 及更高版本采用 [新式生命周期](/lifecycle/policies/modern)，摒弃了 .NET Framework 版本所采用的[固定生命周期](/lifecycle/policies/fixed)。 具有固定生命周期的产品提供较长的固定支持期，例如 5 年主要支持和 5 年外延支持。 主要支持包括安全性修复和非安全性修复，而外延支持仅提供安全性修复。 采用新式生命周期的产品具有更类似于服务的支持模型，支持周期更短，发布频率更高。

### <a name="release-tracks"></a>版本跟踪

版本有两种支持跟踪：

* 当前版本

  在下一个主要或次要版本发布后 3 个月内，支持这些版本。

  示例：

  * .NET Core 3.0 于 2019 年 9 月发布，随后于 2019 年 12 月发布 .NET Core 3.1。
  * .NET Core 3.0 支持于 3.1 发布后 3 个月（即 2020 年 3 月）结束。

* 长期支持 (LTS) 版本

  这些版本至少支持 3 年，或下一个 LTS 版本发布后 1 年（以较晚发生者为准）。

  示例：

  * .NET Core 2.1 于 2018 年 5 月发布，并于 2018 年 8 月被认定为 LTS 版本。
  * .NET Core 3.1 是下一个 LTS 版本，于 2019 年 12 月发布。
  * 由于 2021 年 8 月（3 年）晚于 2020 年 12 月（3.1 版本发布后一年），因此对 .NET Core 2.1 的支持将持续到 2021 年 8 月。

版本在 LTS 与当前之间交替，因此较早的版本可能比较晚的版本受支持的时间更长。 例如，.NET Core 2.1 是支持时间持续到 2021 年 8 月的 LTS 版本。 3\.0 版本在一年多以后发布，但在 2019 年 12 月结束支持，这早于 2.1。

服务更新每月发布一次，包括安全性和非安全性（可靠性、兼容性和稳定性）修复。 服务更新的支持时间持续到下一次发布服务更新。 服务更新具有运行时前滚行为。 这意味着应用程序默认在最新安装的运行时服务更新上运行。

## <a name="how-to-choose-a-release"></a>如何选择版本

如果你要构建服务并希望继续定期更新服务，则当前版本（如 .NET 5.0）可能是最佳选择，它可让你持续获得 .NET 的最新功能。

如果你要构建将分发给使用者的客户端应用程序，稳定性可能比最新功能的可获得性更重要。 在使用者可以升级到应用程序的下一个版本之前，可能需要在一段时间内支持你的应用程序。 在这种情况下，LTS 版本（如 .NET Core 3.1）可能是正确的选择。

### <a name="servicing-updates"></a>服务更新

.NET 服务更新的支持时间持续到下一次发布服务更新。 服务更新每月发布一次。

你需要定期安装服务更新以确保应用处于安全且受支持的状态。 例如，如果 .NET Core 3.1 的最新服务更新为 3.1.8，而我们发布了 3.1.9，则 3.1.8 不再是最新版本。 3\.1 的受支持服务级别为 3.1.9。

有关每个主要和次要版本的最新服务更新信息，请参阅 [.NET 下载页面](https://dotnet.microsoft.com/download/dotnet)。

## <a name="end-of-support"></a>结束支持

结束支持日期是指 Microsoft 不再为产品版本提供修复、更新或技术协助的日期。 在此日期之前，请确保你已升级为使用受支持的版本。 不受支持的版本不再能接收保护应用程序和数据的安全性更新。

## <a name="supported-operating-systems"></a>支持的操作系统

.NET 5（和 .NET Core）及更高版本可在各种操作系统上运行。 每个操作系统都有一个生命周期，由发起组织（例如 Microsoft、Red Hat 或 Apple）定义。 在添加和删除操作系统版本支持时，我们将考虑这些生命周期计划。

当操作系统版本不受支持时，我们将停止测试该版本并停止对该版本提供支持。 用户需要升级到受支持的操作系统版本才能获得支持。

有关详细信息，请参阅 [.NET OS 生命周期策略](https://github.com/dotnet/core/blob/main/os-lifecycle-policy.md)。

## <a name="get-support"></a>获取支持

你可以选择 Microsoft 辅助支持或社区支持。

### <a name="microsoft-support"></a>Microsoft 支持

若需要辅助支持，请[与 Microsoft 支持专业人员联系](https://support.microsoft.com/supportforbusiness/productselection/?sapid=4fd4947b-15ea-ce01-080f-97f2ca3c76e8)。

你需要处于受支持的服务级别（最新可用的服务更新）才能获得支持。 如果系统运行 3.1，而已发布 3.1.8 服务更新，那么第一步需要安装 3.1.8。

### <a name="community-support"></a>社区支持

有关社区支持，请参阅[社区页面](https://dotnet.microsoft.com/platform/community)。

## <a name="see-also"></a>另请参阅

有关详细信息（包括各 .NET Core 版本和 .NET 5 受支持的日期范围），请参阅[支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。
