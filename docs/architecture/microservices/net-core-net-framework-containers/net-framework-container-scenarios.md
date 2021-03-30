---
title: 何时为 Docker 容器选择 .NET Framework
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 何时为 Docker 容器选择 .NET Framework
ms.date: 01/13/2021
ms.openlocfilehash: fa644753c3a39f285052aba7a8524ea96442c842
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873128"
---
# <a name="when-to-choose-net-framework-for-docker-containers"></a>何时为 Docker 容器选择 .NET Framework

虽然 .NET 5 为新的应用程序和应用程序模式带来的好处很明显，但在很多现有情况下仍然会选择 .NET Framework。

## <a name="migrating-existing-applications-directly-to-a-windows-server-container"></a>将现有应用程序直接迁移到 Windows Server 容器

你可能只想使用 Docker 容器来简化部署（即便未创建微服务）。 例如，你可能希望使用 Docker 改进 DevOps 工作流 — 容器可以提供更好的独立测试环境，并且还可以消除在迁移到生产环境时由于缺少依赖关系而导致的部署问题。 在这种情况下，即使部署的是整体式应用程序，也可以为当前的 .NET Framework 应用程序使用 Docker 和 Windows 容器。

对于此场景，大多数情况下无需将现有应用程序迁移到 .NET 5；可以使用包含传统 .NET Framework 的 Docker 容器。 不过，在扩展现有的应用程序（例如，在 ASP.NET Core 中编写新服务）时，建议使用 .NET 5。

## <a name="using-third-party-net-libraries-or-nuget-packages-not-available-for-net-5"></a>使用不可用于 .NET 5 的第三方 .NET 库或 NuGet 包

第三方库正在迅速采用 [.NET Standard](../../../standard/net-standard.md)，这样可跨各种 .NET 版本（包括 .NET 5）共享代码。 在 .NET Standard 2.0 及更高版本中，跨不同框架的 API 接口兼容性已大大提高。 甚至 .NET Core 2.x 和更新版本的应用程序也可以直接引用现有的 .NET Framework 库（请参阅[支持 .NET Standard 2.0 的 .NET Framework 4.6.1](https://github.com/dotnet/standard/blob/master/docs/planning/netstandard-2.0/README.md#net-framework-461-supporting-net-standard-20)）。

此外，[Windows 兼容包](../../../core/porting/windows-compat-pack.md)扩展可供 Windows 上的 .NET Standard 2.0 使用的 API 接口。 通过此包，只需略微修改或无需修改即可将大多数现有代码重新编译到 .NET Standard 2.x 以在 Windows 上运行。

但即使 .NET Standard 2.0 和 .NET Core 2.1 或更高版本取得了如此重大的进展，也可能出现某些 NuGet 包需要在 Windows 上运行并且可能不支持 .NET Core 或更高版本的情况。 如果这些软件包对于应用程序至关重要，那么将需要在 Windows 容器上使用 .NET Framework。

## <a name="using-net-technologies-not-available-for-net-5"></a>使用不可用于 .NET 5 的 .NET 技术

当前版本的 .NET（撰写本文时为 5.0 版）中没有提供某些 .NET Framework 技术。 虽然某些技术将在更高版本中可用，但其他技术不适用于 .NET Core 面向的新应用程序模式，因此可能永远不可用。

以下列表展示了在 .NET 5 中不可用的大多数技术：

- ASP.NET Web 窗体。 该技术仅在 .NET Framework 上可用。 目前没有将 ASP.NET Web 窗体引入 .NET 或更高版本的计划。

- WCF 服务。 虽然 [WCF 客户端库](https://github.com/dotnet/wcf)可从 .NET 5 使用 WCF 服务，但从 2021 年 1 月起，WCF 服务器实现仅在 .NET Framework 上可用。

- 与工作流相关的服务。 Windows Workflow Foundation (WF)、工作流服务（WCF + 单个服务中的 WF）和 WCF Data Services（以前称为 ADO.NET Data Services）仅在 .NET Framework 上可用。 尚未计划将其引入 .NET 5。

除了官方 [.NET 路线图](https://github.com/dotnet/core/blob/main/roadmap.md)中列出的技术之外，可能还会将其他功能移植到新的[统一 .NET 平台](https://devblogs.microsoft.com/dotnet/introducing-net-5/)中。 请参与 GitHub 上的讨论，发表你的看法。 如果认为遗漏了某些内容，请在 [dotnet/runtime](https://github.com/dotnet/runtime/issues/new) GitHub 存储库中提出新的问题。

## <a name="using-a-platform-or-api-that-doesnt-support-net-5"></a>使用不支持 .NET 5 的平台或 API

某些 Microsoft 和第三方平台不支持 .NET 5。 例如，某些 Azure 服务提供尚不可用于 .NET 5 的 SDK。 大多数 Azure SDK 最终应移植到 .NET 5/Standard，但有些可能因各种原因而未移植。 可在 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/index.html)页中查看可用的 Azure SDK。

在此期间，如果 Azure 中的任何平台或服务仍然不支持 .NET 5 及其客户端 API，则可以使用 Azure 服务中的等效 REST API 或 .NET Framework 上的客户端 SDK。

### <a name="additional-resources"></a>其他资源

- **.NET 基础知识** \
  [https://docs.microsoft.com/dotnet/fundamentals](../../../fundamentals/index.yml)

- **将项目移植到 .NET 5** \
  [https://channel9.msdn.com/Events/dotnetConf/2020/Porting-Projects-to-NET-5](https://channel9.msdn.com/Events/dotnetConf/2020/Porting-Projects-to-NET-5)

- **Docker 上的 .NET 指南** \
  [https://docs.microsoft.com/dotnet/core/docker/introduction](../../../core/docker/introduction.md)

- **.NET 组件概述** \
  [https://docs.microsoft.com/dotnet/standard/components](../../../standard/components.md)

>[!div class="step-by-step"]
>[上一页](net-core-container-scenarios.md)
>[下一页](container-framework-choice-factors.md)
