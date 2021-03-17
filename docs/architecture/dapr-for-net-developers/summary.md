---
title: 摘要和前景
description: 概括介绍了重要的 Dapr 结论，并探讨了领先的路上。
ms.date: 02/17/2021
author: robvet
ms.openlocfilehash: fdbb92d067c29db56aa7449b8fe3c974c8c132b8
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624066"
---
# <a name="summary-and-the-road-ahead"></a><span data-ttu-id="6f945-103">摘要和前景</span><span class="sxs-lookup"><span data-stu-id="6f945-103">Summary and the road ahead</span></span>

<span data-ttu-id="6f945-104">我们正在 Dapr 航班结束。</span><span class="sxs-lookup"><span data-stu-id="6f945-104">We're at the end of our Dapr flight.</span></span> <span data-ttu-id="6f945-105">从 [第2章](dapr-at-20000-feet.md) 开始，从20000英尺开始的 jet 飞机是最终的方法，并是关于地面。</span><span class="sxs-lookup"><span data-stu-id="6f945-105">The jet plane flying at 20,000 feet from [chapter 2](dapr-at-20000-feet.md) is on final approach and about to land.</span></span>

<span data-ttu-id="6f945-106">随着飞机出租车，让我们花点时间来回顾本指南中的一些重要结论：</span><span class="sxs-lookup"><span data-stu-id="6f945-106">As the plane taxis to the gate, let's take a minute to review some important conclusions from this guide:</span></span>

- <span data-ttu-id="6f945-107">**Dapr** -Dapr 是一种 *分布式应用程序运行时* ，它可以简化生成分布式应用程序的方式。</span><span class="sxs-lookup"><span data-stu-id="6f945-107">**Dapr** - Dapr is a *Distributed Application Runtime* that streamlines how you build distributed applications.</span></span> <span data-ttu-id="6f945-108">它公开构建基块和可插入组件的体系结构。</span><span class="sxs-lookup"><span data-stu-id="6f945-108">It exposes an architecture of building blocks and pluggable components.</span></span> <span data-ttu-id="6f945-109">Dapr 提供了一个 **动态胶水** ，它将应用程序与 Dapr 运行时中存在的基础结构功能进行绑定。</span><span class="sxs-lookup"><span data-stu-id="6f945-109">Dapr provides a **dynamic glue** that binds your application with infrastructure capabilities that exist in the Dapr runtime.</span></span> <span data-ttu-id="6f945-110">你和你的团队将精力集中于向客户提供业务功能，而不是构建基础结构管道。</span><span class="sxs-lookup"><span data-stu-id="6f945-110">Instead of building infrastructure plumbing, you and your team focus on delivering business features to customers.</span></span>

- <span data-ttu-id="6f945-111">**开放源和跨平台** -本机 Dapr API 可由支持 HTTP 或 gRPC 的 *任何平台* 使用。</span><span class="sxs-lookup"><span data-stu-id="6f945-111">**Open source and cross-platform** - The native Dapr API can be consumed by *any platform* that supports HTTP or gRPC.</span></span> <span data-ttu-id="6f945-112">Dapr 还为常见开发平台提供特定于语言的 Sdk。</span><span class="sxs-lookup"><span data-stu-id="6f945-112">Dapr also provides language-specific SDKs for popular development platforms.</span></span> <span data-ttu-id="6f945-113">Dapr v1.0 支持中转、Python、.NET、Java、PHP 和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="6f945-113">Dapr v1.0 supports Go, Python, .NET, Java, PHP, and JavaScript.</span></span>

- <span data-ttu-id="6f945-114">**构建基块** -Dapr 构建基块封装分布式应用程序功能。</span><span class="sxs-lookup"><span data-stu-id="6f945-114">**Building blocks** - Dapr building blocks encapsulate distributed application functionality.</span></span> <span data-ttu-id="6f945-115">撰写本文时，Dapr 支持图11-1 中所示的七个构造块。</span><span class="sxs-lookup"><span data-stu-id="6f945-115">At the time of this writing, Dapr supports the seven building blocks shown in figure 11-1.</span></span>

![Dapr 构建基块](./media/dapr-at-20000-feet/building-blocks.png)

<span data-ttu-id="6f945-117">**图 11-1**。</span><span class="sxs-lookup"><span data-stu-id="6f945-117">**Figure 11-1**.</span></span> <span data-ttu-id="6f945-118">Dapr 构建基块。</span><span class="sxs-lookup"><span data-stu-id="6f945-118">Dapr building blocks.</span></span>

- <span data-ttu-id="6f945-119">**组件** -Dapr 组件提供每个 Dapr 构建块功能的具体实现。</span><span class="sxs-lookup"><span data-stu-id="6f945-119">**Components** - Dapr components provide the concrete implementation for each Dapr building block capability.</span></span> <span data-ttu-id="6f945-120">它们公开一个公共接口，使开发人员能够在不更改应用程序代码的情况下交换组件实现。</span><span class="sxs-lookup"><span data-stu-id="6f945-120">They expose a common interface that enables developers to swap out component implementations without changing application code.</span></span> <span data-ttu-id="6f945-121">图11-2 显示组件、构建基块和服务之间的关系。</span><span class="sxs-lookup"><span data-stu-id="6f945-121">Figure 11-2 shows the relationship among components, building blocks, and your service.</span></span>

![Dapr 构建基块集成](./media/dapr-at-20000-feet/building-blocks-integration.png)

<span data-ttu-id="6f945-123">图 11-2。</span><span class="sxs-lookup"><span data-stu-id="6f945-123">**Figure 11-2**.</span></span> <span data-ttu-id="6f945-124">Dapr 构建基块集成。</span><span class="sxs-lookup"><span data-stu-id="6f945-124">Dapr building block integration.</span></span>

- <span data-ttu-id="6f945-125">**分支** -Dapr 在挎斗体系结构中与应用程序一起运行，可以作为容器的单独进程。</span><span class="sxs-lookup"><span data-stu-id="6f945-125">**Sidecars** - Dapr runs alongside your application in a sidecar architecture, either as a separate process of a container.</span></span> <span data-ttu-id="6f945-126">应用程序通过 HTTP 和 gRPC 与 Dapr Api 通信。</span><span class="sxs-lookup"><span data-stu-id="6f945-126">Your application communicates with the Dapr APIs over HTTP and gRPC.</span></span> <span data-ttu-id="6f945-127">分支提供隔离和封装，因为它们不是服务的一部分，而是连接到它。</span><span class="sxs-lookup"><span data-stu-id="6f945-127">Sidecars provide isolation and encapsulation as they aren't part of the service, but connected to it.</span></span> <span data-ttu-id="6f945-128">图11-3 显示了挎斗的体系结构。</span><span class="sxs-lookup"><span data-stu-id="6f945-128">Figure 11-3 shows a sidecar architecture.</span></span>

![挎斗体系结构](./media/dapr-at-20000-feet/sidecar-generic.png)

<span data-ttu-id="6f945-130">图 11-3。</span><span class="sxs-lookup"><span data-stu-id="6f945-130">**Figure 11-3**.</span></span> <span data-ttu-id="6f945-131">挎斗体系结构。</span><span class="sxs-lookup"><span data-stu-id="6f945-131">Sidecar architecture.</span></span>

- <span data-ttu-id="6f945-132">**宿主环境** Dapr 提供跨平台支持，可以在多个环境中运行。</span><span class="sxs-lookup"><span data-stu-id="6f945-132">**Hosting environments** Dapr has cross-platform support and can run in multiple environments.</span></span> <span data-ttu-id="6f945-133">撰写本文时，环境包含本地自承载模式和 Kubernetes。</span><span class="sxs-lookup"><span data-stu-id="6f945-133">At the time of this writing, the environments include a local self-hosted mode and Kubernetes.</span></span>

- <span data-ttu-id="6f945-134">**eShopOnDapr** -此书籍包含一个名为 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)的附属引用应用程序。</span><span class="sxs-lookup"><span data-stu-id="6f945-134">**eShopOnDapr** - This book includes an accompanying reference application entitled [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr).</span></span> <span data-ttu-id="6f945-135">使用常用的电子商务应用程序域，引用应用程序演示了每个构建基块的使用情况。</span><span class="sxs-lookup"><span data-stu-id="6f945-135">Using a popular e-commerce application domain, the reference application demonstrates the usage of each building block.</span></span> <span data-ttu-id="6f945-136">这是一种广泛使用的 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)的演变。</span><span class="sxs-lookup"><span data-stu-id="6f945-136">It's an evolution of the widely popular [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers), released several years ago.</span></span>

## <a name="the-road-ahead"></a><span data-ttu-id="6f945-137">领先的路上</span><span class="sxs-lookup"><span data-stu-id="6f945-137">The road ahead</span></span>

<span data-ttu-id="6f945-138">期待着，Dapr 有可能对分布式应用程序开发产生深远的影响。</span><span class="sxs-lookup"><span data-stu-id="6f945-138">Looking forward, Dapr has the potential to have a profound impact on distributed application development.</span></span> <span data-ttu-id="6f945-139">Dapr 团队及其开源参与者的预期情况如何？</span><span class="sxs-lookup"><span data-stu-id="6f945-139">What can you expect from the Dapr team and its open-source contributors?</span></span>

<span data-ttu-id="6f945-140">撰写本文时，Dapr 的建议增强列表包括：</span><span class="sxs-lookup"><span data-stu-id="6f945-140">At the time of writing, the list of proposed enhancements for Dapr include:</span></span>

- <span data-ttu-id="6f945-141">现有构建基块的功能增强：</span><span class="sxs-lookup"><span data-stu-id="6f945-141">Feature enhancements to existing building blocks:</span></span>
  - <span data-ttu-id="6f945-142">状态管理中的查询功能使你能够检索多个值。</span><span class="sxs-lookup"><span data-stu-id="6f945-142">Query capabilities in state management enabling you to retrieve multiple values.</span></span>
  - <span data-ttu-id="6f945-143">通过 "发布/订阅" 中的主题筛选，您可以基于内容筛选主题。</span><span class="sxs-lookup"><span data-stu-id="6f945-143">Topic filtering in pub/sub enabling you to filter topics based on their content.</span></span>
  - <span data-ttu-id="6f945-144">可观察性中的应用程序跟踪 API，无需绑定到特定库即可直接在应用程序中提供跟踪。</span><span class="sxs-lookup"><span data-stu-id="6f945-144">An application tracing API in observability that provides tracing in the application directly without having to bind to specific libraries.</span></span>
  - <span data-ttu-id="6f945-145">为参与者编程模型提供事件驱动功能的参与者的绑定和发布/订阅支持。</span><span class="sxs-lookup"><span data-stu-id="6f945-145">Binding and pub/sub support for actors providing event driven capabilities to the actor programming model.</span></span> <span data-ttu-id="6f945-146">绑定组件将触发事件和消息调用执行组件中的方法。</span><span class="sxs-lookup"><span data-stu-id="6f945-146">Bound components will trigger events and messages invoke methods in the actor.</span></span>

- <span data-ttu-id="6f945-147">新构建基块：</span><span class="sxs-lookup"><span data-stu-id="6f945-147">New building blocks:</span></span>
  - <span data-ttu-id="6f945-148">用于读取和写入配置数据的配置 API 构建基块。</span><span class="sxs-lookup"><span data-stu-id="6f945-148">Configuration API building block for reading and writing configuration data.</span></span> <span data-ttu-id="6f945-149">该块将绑定到包含 Azure Configuration Manager 或 GCP 配置管理的提供程序。</span><span class="sxs-lookup"><span data-stu-id="6f945-149">The block will bind to providers that include Azure Configuration Manager or GCP Configuration Management.</span></span>
  - <span data-ttu-id="6f945-150">Http 缩放到零自动缩放。</span><span class="sxs-lookup"><span data-stu-id="6f945-150">Http scale-to-zero autoscale.</span></span>
  - <span data-ttu-id="6f945-151">领导选举构建基块提供单一实例和锁定语义功能。</span><span class="sxs-lookup"><span data-stu-id="6f945-151">Leader election building block to provide singleton instances and locking semantic capabilities.</span></span>
  - <span data-ttu-id="6f945-152">用于服务调用的透明代理构建块，使你能够基于网络级别的 Url 或 DNS 地址来路由消息。</span><span class="sxs-lookup"><span data-stu-id="6f945-152">Transparent proxying building block for service invocation, enabling you to route messages based on URLs or DNS addresses at the network level.</span></span>
  - <span data-ttu-id="6f945-153">复原构建块 (断路熔断器，隔舱 & 超时) 。</span><span class="sxs-lookup"><span data-stu-id="6f945-153">Resiliency building block (circuit breakers, bulkheads & timeouts).</span></span>

- <span data-ttu-id="6f945-154">与框架和云本机技术的集成。</span><span class="sxs-lookup"><span data-stu-id="6f945-154">Integration with frameworks and cloud native technologies.</span></span> <span data-ttu-id="6f945-155">示例包括：</span><span class="sxs-lookup"><span data-stu-id="6f945-155">Some examples include:</span></span>
  - <span data-ttu-id="6f945-156">Django</span><span class="sxs-lookup"><span data-stu-id="6f945-156">Django</span></span>
  - <span data-ttu-id="6f945-157">Nodejs</span><span class="sxs-lookup"><span data-stu-id="6f945-157">Nodejs</span></span>
  - <span data-ttu-id="6f945-158">Express</span><span class="sxs-lookup"><span data-stu-id="6f945-158">Express</span></span>
  - <span data-ttu-id="6f945-159">Kyma</span><span class="sxs-lookup"><span data-stu-id="6f945-159">Kyma</span></span>
  - <span data-ttu-id="6f945-160">中途</span><span class="sxs-lookup"><span data-stu-id="6f945-160">Midway</span></span>

- <span data-ttu-id="6f945-161">新语言 Sdk：</span><span class="sxs-lookup"><span data-stu-id="6f945-161">New language SDKs:</span></span>
  - <span data-ttu-id="6f945-162">JavaScript</span><span class="sxs-lookup"><span data-stu-id="6f945-162">JavaScript</span></span>
  - <span data-ttu-id="6f945-163">RUST</span><span class="sxs-lookup"><span data-stu-id="6f945-163">RUST</span></span>
  - <span data-ttu-id="6f945-164">C++</span><span class="sxs-lookup"><span data-stu-id="6f945-164">C++</span></span>

- <span data-ttu-id="6f945-165">新的宿主平台：</span><span class="sxs-lookup"><span data-stu-id="6f945-165">New hosting platforms:</span></span>
  - <span data-ttu-id="6f945-166">VM</span><span class="sxs-lookup"><span data-stu-id="6f945-166">VMs</span></span>
  - <span data-ttu-id="6f945-167">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="6f945-167">Azure IoT Edge</span></span>
  - <span data-ttu-id="6f945-168">Azure Stack Edge</span><span class="sxs-lookup"><span data-stu-id="6f945-168">Azure Stack Edge</span></span>
  - <span data-ttu-id="6f945-169">Azure Service Fabric</span><span class="sxs-lookup"><span data-stu-id="6f945-169">Azure Service Fabric</span></span>

- <span data-ttu-id="6f945-170">开发人员和操作员生产力工具：</span><span class="sxs-lookup"><span data-stu-id="6f945-170">Developer and operator productivity tooling:</span></span>
  - <span data-ttu-id="6f945-171">VS Code 扩展。</span><span class="sxs-lookup"><span data-stu-id="6f945-171">VS Code extension.</span></span>
  - <span data-ttu-id="6f945-172">用于本地调试 DevOps 管道开发的远程开发人员容器。</span><span class="sxs-lookup"><span data-stu-id="6f945-172">Remote Dev Containers for local debugging a DevOps pipeline development.</span></span>
  - <span data-ttu-id="6f945-173">Dapr 操作仪表板增强功能，可更深入地了解管理 Dapr 应用程序的操作问题。</span><span class="sxs-lookup"><span data-stu-id="6f945-173">Dapr operational dashboard enhancements that will provide deeper visibility into the operational concerns of managing Dapr applications.</span></span>

<span data-ttu-id="6f945-174">Dapr 版本1.0 为开发人员提供了引人注目的工具箱来构建分布式应用程序。</span><span class="sxs-lookup"><span data-stu-id="6f945-174">Dapr version 1.0 provides developers with a compelling toolbox for building distributed applications.</span></span> <span data-ttu-id="6f945-175">如建议的增强列表所示，Dapr 处于积极开发下，具有许多新功能。</span><span class="sxs-lookup"><span data-stu-id="6f945-175">As the proposed enhancement list shows, Dapr is under active development with many new capabilities to come.</span></span> <span data-ttu-id="6f945-176">请继续关注 [Dapr 站点](https://dapr.io/) 和 [Dapr 公告博客](https://cloudblogs.microsoft.com/opensource/2019/10/16/announcing-dapr-open-source-project-build-microservice-applications/) ，以供将来更新。</span><span class="sxs-lookup"><span data-stu-id="6f945-176">Stay tuned to the [Dapr site](https://dapr.io/) and [Dapr announcement blog](https://cloudblogs.microsoft.com/opensource/2019/10/16/announcing-dapr-open-source-project-build-microservice-applications/) for future updates.</span></span>

>[!div class="step-by-step"]
>[<span data-ttu-id="6f945-177">上一页</span><span class="sxs-lookup"><span data-stu-id="6f945-177">Previous</span></span>](secrets.md)
