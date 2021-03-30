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
# <a name="when-to-choose-net-framework-for-docker-containers"></a><span data-ttu-id="f860a-103">何时为 Docker 容器选择 .NET Framework</span><span class="sxs-lookup"><span data-stu-id="f860a-103">When to choose .NET Framework for Docker containers</span></span>

<span data-ttu-id="f860a-104">虽然 .NET 5 为新的应用程序和应用程序模式带来的好处很明显，但在很多现有情况下仍然会选择 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="f860a-104">While .NET 5 offers significant benefits for new applications and application patterns, .NET Framework will continue to be a good choice for many existing scenarios.</span></span>

## <a name="migrating-existing-applications-directly-to-a-windows-server-container"></a><span data-ttu-id="f860a-105">将现有应用程序直接迁移到 Windows Server 容器</span><span class="sxs-lookup"><span data-stu-id="f860a-105">Migrating existing applications directly to a Windows Server container</span></span>

<span data-ttu-id="f860a-106">你可能只想使用 Docker 容器来简化部署（即便未创建微服务）。</span><span class="sxs-lookup"><span data-stu-id="f860a-106">You might want to use Docker containers just to simplify deployment, even if you are not creating microservices.</span></span> <span data-ttu-id="f860a-107">例如，你可能希望使用 Docker 改进 DevOps 工作流 — 容器可以提供更好的独立测试环境，并且还可以消除在迁移到生产环境时由于缺少依赖关系而导致的部署问题。</span><span class="sxs-lookup"><span data-stu-id="f860a-107">For example, perhaps you want to improve your DevOps workflow with Docker—containers can give you better isolated test environments and can also eliminate deployment issues caused by missing dependencies when you move to a production environment.</span></span> <span data-ttu-id="f860a-108">在这种情况下，即使部署的是整体式应用程序，也可以为当前的 .NET Framework 应用程序使用 Docker 和 Windows 容器。</span><span class="sxs-lookup"><span data-stu-id="f860a-108">In cases like these, even if you are deploying a monolithic application, it makes sense to use Docker and Windows Containers for your current .NET Framework applications.</span></span>

<span data-ttu-id="f860a-109">对于此场景，大多数情况下无需将现有应用程序迁移到 .NET 5；可以使用包含传统 .NET Framework 的 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="f860a-109">In most cases for this scenario, you will not need to migrate your existing applications to .NET 5; you can use Docker containers that include the traditional .NET Framework.</span></span> <span data-ttu-id="f860a-110">不过，在扩展现有的应用程序（例如，在 ASP.NET Core 中编写新服务）时，建议使用 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="f860a-110">However, a recommended approach is to use .NET 5 as you extend an existing application, such as writing a new service in ASP.NET Core.</span></span>

## <a name="using-third-party-net-libraries-or-nuget-packages-not-available-for-net-5"></a><span data-ttu-id="f860a-111">使用不可用于 .NET 5 的第三方 .NET 库或 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="f860a-111">Using third-party .NET libraries or NuGet packages not available for .NET 5</span></span>

<span data-ttu-id="f860a-112">第三方库正在迅速采用 [.NET Standard](../../../standard/net-standard.md)，这样可跨各种 .NET 版本（包括 .NET 5）共享代码。</span><span class="sxs-lookup"><span data-stu-id="f860a-112">Third-party libraries are quickly embracing [.NET Standard](../../../standard/net-standard.md), which enables code sharing across all .NET flavors, including .NET 5.</span></span> <span data-ttu-id="f860a-113">在 .NET Standard 2.0 及更高版本中，跨不同框架的 API 接口兼容性已大大提高。</span><span class="sxs-lookup"><span data-stu-id="f860a-113">With .NET Standard 2.0 and later, the API surface compatibility across different frameworks has become significantly larger.</span></span> <span data-ttu-id="f860a-114">甚至 .NET Core 2.x 和更新版本的应用程序也可以直接引用现有的 .NET Framework 库（请参阅[支持 .NET Standard 2.0 的 .NET Framework 4.6.1](https://github.com/dotnet/standard/blob/master/docs/planning/netstandard-2.0/README.md#net-framework-461-supporting-net-standard-20)）。</span><span class="sxs-lookup"><span data-stu-id="f860a-114">Even more, .NET Core 2.x and newer applications can also directly reference existing .NET Framework libraries (see [.NET Framework 4.6.1 supporting .NET Standard 2.0](https://github.com/dotnet/standard/blob/master/docs/planning/netstandard-2.0/README.md#net-framework-461-supporting-net-standard-20)).</span></span>

<span data-ttu-id="f860a-115">此外，[Windows 兼容包](../../../core/porting/windows-compat-pack.md)扩展可供 Windows 上的 .NET Standard 2.0 使用的 API 接口。</span><span class="sxs-lookup"><span data-stu-id="f860a-115">In addition, the [Windows Compatibility Pack](../../../core/porting/windows-compat-pack.md) extends the API surface available for .NET Standard 2.0 on Windows.</span></span> <span data-ttu-id="f860a-116">通过此包，只需略微修改或无需修改即可将大多数现有代码重新编译到 .NET Standard 2.x 以在 Windows 上运行。</span><span class="sxs-lookup"><span data-stu-id="f860a-116">This pack allows recompiling most existing code to .NET Standard 2.x with little or no modification, to run on Windows.</span></span>

<span data-ttu-id="f860a-117">但即使 .NET Standard 2.0 和 .NET Core 2.1 或更高版本取得了如此重大的进展，也可能出现某些 NuGet 包需要在 Windows 上运行并且可能不支持 .NET Core 或更高版本的情况。</span><span class="sxs-lookup"><span data-stu-id="f860a-117">However, even with that exceptional progression since .NET Standard 2.0 and .NET Core 2.1 or later, there might be cases where certain NuGet packages need Windows to run and might not support .NET Core or later.</span></span> <span data-ttu-id="f860a-118">如果这些软件包对于应用程序至关重要，那么将需要在 Windows 容器上使用 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="f860a-118">If those packages are critical for your application, then you will need to use .NET Framework on Windows Containers.</span></span>

## <a name="using-net-technologies-not-available-for-net-5"></a><span data-ttu-id="f860a-119">使用不可用于 .NET 5 的 .NET 技术</span><span class="sxs-lookup"><span data-stu-id="f860a-119">Using .NET technologies not available for .NET 5</span></span>

<span data-ttu-id="f860a-120">当前版本的 .NET（撰写本文时为 5.0 版）中没有提供某些 .NET Framework 技术。</span><span class="sxs-lookup"><span data-stu-id="f860a-120">Some .NET Framework technologies aren't available in the current version of .NET (version 5.0 as of this writing).</span></span> <span data-ttu-id="f860a-121">虽然某些技术将在更高版本中可用，但其他技术不适用于 .NET Core 面向的新应用程序模式，因此可能永远不可用。</span><span class="sxs-lookup"><span data-stu-id="f860a-121">Some of them might become available in later releases, but others don't fit the new application patterns targeted by .NET Core and might never be available.</span></span>

<span data-ttu-id="f860a-122">以下列表展示了在 .NET 5 中不可用的大多数技术：</span><span class="sxs-lookup"><span data-stu-id="f860a-122">The following list shows most of the technologies that aren't available in .NET 5:</span></span>

- <span data-ttu-id="f860a-123">ASP.NET Web 窗体。</span><span class="sxs-lookup"><span data-stu-id="f860a-123">ASP.NET Web Forms.</span></span> <span data-ttu-id="f860a-124">该技术仅在 .NET Framework 上可用。</span><span class="sxs-lookup"><span data-stu-id="f860a-124">This technology is only available on .NET Framework.</span></span> <span data-ttu-id="f860a-125">目前没有将 ASP.NET Web 窗体引入 .NET 或更高版本的计划。</span><span class="sxs-lookup"><span data-stu-id="f860a-125">Currently there are no plans to bring ASP.NET Web Forms to .NET  or later.</span></span>

- <span data-ttu-id="f860a-126">WCF 服务。</span><span class="sxs-lookup"><span data-stu-id="f860a-126">WCF services.</span></span> <span data-ttu-id="f860a-127">虽然 [WCF 客户端库](https://github.com/dotnet/wcf)可从 .NET 5 使用 WCF 服务，但从 2021 年 1 月起，WCF 服务器实现仅在 .NET Framework 上可用。</span><span class="sxs-lookup"><span data-stu-id="f860a-127">Even when a [WCF-Client library](https://github.com/dotnet/wcf) is available to consume WCF services from .NET 5, as of Jan-2021, the WCF server implementation is only available on .NET Framework.</span></span>

- <span data-ttu-id="f860a-128">与工作流相关的服务。</span><span class="sxs-lookup"><span data-stu-id="f860a-128">Workflow-related services.</span></span> <span data-ttu-id="f860a-129">Windows Workflow Foundation (WF)、工作流服务（WCF + 单个服务中的 WF）和 WCF Data Services（以前称为 ADO.NET Data Services）仅在 .NET Framework 上可用。</span><span class="sxs-lookup"><span data-stu-id="f860a-129">Windows Workflow Foundation (WF), Workflow Services (WCF + WF in a single service), and WCF Data Services (formerly known as ADO.NET Data Services) are only available on .NET Framework.</span></span> <span data-ttu-id="f860a-130">尚未计划将其引入 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="f860a-130">There are currently no plans to bring them to .NET 5.</span></span>

<span data-ttu-id="f860a-131">除了官方 [.NET 路线图](https://github.com/dotnet/core/blob/main/roadmap.md)中列出的技术之外，可能还会将其他功能移植到新的[统一 .NET 平台](https://devblogs.microsoft.com/dotnet/introducing-net-5/)中。</span><span class="sxs-lookup"><span data-stu-id="f860a-131">In addition to the technologies listed in the official [.NET roadmap](https://github.com/dotnet/core/blob/main/roadmap.md), other features might be ported to the new [unified .NET platform](https://devblogs.microsoft.com/dotnet/introducing-net-5/).</span></span> <span data-ttu-id="f860a-132">请参与 GitHub 上的讨论，发表你的看法。</span><span class="sxs-lookup"><span data-stu-id="f860a-132">You might consider participating in the discussions on GitHub so that your voice can be heard.</span></span> <span data-ttu-id="f860a-133">如果认为遗漏了某些内容，请在 [dotnet/runtime](https://github.com/dotnet/runtime/issues/new) GitHub 存储库中提出新的问题。</span><span class="sxs-lookup"><span data-stu-id="f860a-133">And if you think something is missing, file a new issue in the [dotnet/runtime](https://github.com/dotnet/runtime/issues/new) GitHub repository.</span></span>

## <a name="using-a-platform-or-api-that-doesnt-support-net-5"></a><span data-ttu-id="f860a-134">使用不支持 .NET 5 的平台或 API</span><span class="sxs-lookup"><span data-stu-id="f860a-134">Using a platform or API that doesn't support .NET 5</span></span>

<span data-ttu-id="f860a-135">某些 Microsoft 和第三方平台不支持 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="f860a-135">Some Microsoft and third-party platforms don't support .NET 5.</span></span> <span data-ttu-id="f860a-136">例如，某些 Azure 服务提供尚不可用于 .NET 5 的 SDK。</span><span class="sxs-lookup"><span data-stu-id="f860a-136">For example, some Azure services provide an SDK that isn't yet available for consumption on .NET 5.</span></span> <span data-ttu-id="f860a-137">大多数 Azure SDK 最终应移植到 .NET 5/Standard，但有些可能因各种原因而未移植。</span><span class="sxs-lookup"><span data-stu-id="f860a-137">Most Azure SDK should eventually be ported to .NET 5/Standard but some might not for various reasons.</span></span> <span data-ttu-id="f860a-138">可在 [Azure SDK 最新版本](https://azure.github.io/azure-sdk/releases/latest/index.html)页中查看可用的 Azure SDK。</span><span class="sxs-lookup"><span data-stu-id="f860a-138">You can see the available Azure SDKs in the [Azure SDK Latest Releases](https://azure.github.io/azure-sdk/releases/latest/index.html) page.</span></span>

<span data-ttu-id="f860a-139">在此期间，如果 Azure 中的任何平台或服务仍然不支持 .NET 5 及其客户端 API，则可以使用 Azure 服务中的等效 REST API 或 .NET Framework 上的客户端 SDK。</span><span class="sxs-lookup"><span data-stu-id="f860a-139">In the meantime, if any platform or service in Azure still doesn't support .NET 5 with its client API, you can use the equivalent REST API from the Azure service or the client SDK on .NET Framework.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="f860a-140">其他资源</span><span class="sxs-lookup"><span data-stu-id="f860a-140">Additional resources</span></span>

- <span data-ttu-id="f860a-141">**.NET 基础知识** </span><span class="sxs-lookup"><span data-stu-id="f860a-141">**.NET fundamentals** </span></span>\
  [https://docs.microsoft.com/dotnet/fundamentals](../../../fundamentals/index.yml)

- <span data-ttu-id="f860a-142">**将项目移植到 .NET 5** </span><span class="sxs-lookup"><span data-stu-id="f860a-142">**Porting Projects to .NET 5** </span></span>\
  [https://channel9.msdn.com/Events/dotnetConf/2020/Porting-Projects-to-NET-5](https://channel9.msdn.com/Events/dotnetConf/2020/Porting-Projects-to-NET-5)

- <span data-ttu-id="f860a-143">**Docker 上的 .NET 指南** </span><span class="sxs-lookup"><span data-stu-id="f860a-143">**.NET on Docker Guide** </span></span>\
  [https://docs.microsoft.com/dotnet/core/docker/introduction](../../../core/docker/introduction.md)

- <span data-ttu-id="f860a-144">**.NET 组件概述** </span><span class="sxs-lookup"><span data-stu-id="f860a-144">**.NET Components Overview** </span></span>\
  [https://docs.microsoft.com/dotnet/standard/components](../../../standard/components.md)

>[!div class="step-by-step"]
><span data-ttu-id="f860a-145">[上一页](net-core-container-scenarios.md)
>[下一页](container-framework-choice-factors.md)</span><span class="sxs-lookup"><span data-stu-id="f860a-145">[Previous](net-core-container-scenarios.md)
[Next](container-framework-choice-factors.md)</span></span>
