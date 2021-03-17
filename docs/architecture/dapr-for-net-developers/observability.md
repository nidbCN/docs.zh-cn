---
title: Dapr 可观察性构建基块
description: 说明可观察性构建基块及其功能、优点以及如何应用它
author: edwinvw
ms.date: 02/17/2021
ms.reviewer: robvet
ms.openlocfilehash: 745b9c07c31cc3ee11d5df945f2ccb87d0c9c2ed
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623975"
---
# <a name="the-dapr-observability-building-block"></a><span data-ttu-id="ba24c-103">Dapr 可观察性构建基块</span><span class="sxs-lookup"><span data-stu-id="ba24c-103">The Dapr observability building block</span></span>

<span data-ttu-id="ba24c-104">新式分布式系统非常复杂。</span><span class="sxs-lookup"><span data-stu-id="ba24c-104">Modern distributed systems are complex.</span></span> <span data-ttu-id="ba24c-105">首先，你可以通过独立的、松散耦合且可部署的服务。</span><span class="sxs-lookup"><span data-stu-id="ba24c-105">You start with small, loosely coupled, independently deployable services.</span></span> <span data-ttu-id="ba24c-106">这些服务跨进程和服务器边界。</span><span class="sxs-lookup"><span data-stu-id="ba24c-106">These services cross process and server boundaries.</span></span> <span data-ttu-id="ba24c-107">然后，它们使用不同类型的基础结构支持服务 (数据库、消息代理、key vault) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-107">They then consume different kinds of infrastructure backing services (databases, message brokers, key vaults).</span></span> <span data-ttu-id="ba24c-108">最后，这些分散的部分组合在一起构成应用程序。</span><span class="sxs-lookup"><span data-stu-id="ba24c-108">Finally, these disparate pieces compose together to form an application.</span></span>

<span data-ttu-id="ba24c-109">由于有许多不同的移动部件，您如何了解会发生什么情况呢？</span><span class="sxs-lookup"><span data-stu-id="ba24c-109">With so many separate, moving parts, how do you make sense of what is going on?</span></span> <span data-ttu-id="ba24c-110">遗憾的是，过去的旧监视方法还不够。</span><span class="sxs-lookup"><span data-stu-id="ba24c-110">Unfortunately, legacy monitoring approaches from the past aren't enough.</span></span> <span data-ttu-id="ba24c-111">相反，系统必须从端到端 **观察** 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-111">Instead, the system must be **observable** from end-to-end.</span></span> <span data-ttu-id="ba24c-112">新式 [可观察性](../cloud-native/observability-patterns.md) 做法可随时了解应用程序的运行状况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-112">Modern [observability](../cloud-native/observability-patterns.md) practices provide visibility and insight into the health of the application at all times.</span></span> <span data-ttu-id="ba24c-113">它们允许您通过观察输出来推断内部状态。</span><span class="sxs-lookup"><span data-stu-id="ba24c-113">They enable you to infer the internal state by observing the output.</span></span> <span data-ttu-id="ba24c-114">可观察性是监视分布式应用程序和排查其问题所必需的。</span><span class="sxs-lookup"><span data-stu-id="ba24c-114">Observability is mandatory for monitoring and troubleshooting distributed applications.</span></span>

<span data-ttu-id="ba24c-115">用于获取可观察性的系统信息称为 **遥测**。</span><span class="sxs-lookup"><span data-stu-id="ba24c-115">The system information used to gain observability is referred to as **telemetry**.</span></span> <span data-ttu-id="ba24c-116">它可以分为四大类：</span><span class="sxs-lookup"><span data-stu-id="ba24c-116">It can be divided into four broad categories:</span></span>

1. <span data-ttu-id="ba24c-117">**分布式跟踪** 提供有关分布式事务中所涉及服务和服务之间的流量的见解。</span><span class="sxs-lookup"><span data-stu-id="ba24c-117">**Distributed tracing** provides insight into the traffic between services and services involved in distributed transactions.</span></span>
1. <span data-ttu-id="ba24c-118">**度量值** 可让你深入了解服务的性能及其资源消耗情况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-118">**Metrics** provides insight into the performance of a service and its resource consumption.</span></span>
1. <span data-ttu-id="ba24c-119">**日志记录** 可提供代码的执行方式以及错误发生的情况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-119">**Logging** provides insight into how the code is executing and if errors have occurred.</span></span>
1. <span data-ttu-id="ba24c-120">**运行状况** 终结点可让你深入了解服务的可用性。</span><span class="sxs-lookup"><span data-stu-id="ba24c-120">**Health** endpoints provide insight into the availability of a service.</span></span>

<span data-ttu-id="ba24c-121">遥测的深度取决于应用程序平台的可观察性功能。</span><span class="sxs-lookup"><span data-stu-id="ba24c-121">The depth of telemetry is determined by the observability features of an application platform.</span></span> <span data-ttu-id="ba24c-122">请考虑 Azure 云。</span><span class="sxs-lookup"><span data-stu-id="ba24c-122">Consider the Azure cloud.</span></span> <span data-ttu-id="ba24c-123">它提供丰富的遥测体验，其中包括所有遥测类别。</span><span class="sxs-lookup"><span data-stu-id="ba24c-123">It provides a rich telemetry experience that includes all of the telemetry categories.</span></span> <span data-ttu-id="ba24c-124">如果没有任何配置，大多数 Azure IaaS 和 PaaS 服务会将遥测传播和发布到 [Azure 应用程序 Insights](/azure/azure-monitor/app/app-insights-overview) 服务。</span><span class="sxs-lookup"><span data-stu-id="ba24c-124">Without any configuration, most Azure IaaS and PaaS services propagate and publish telemetry to the [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview) service.</span></span> <span data-ttu-id="ba24c-125">Application Insights 通过高度可视化的仪表板显示系统日志记录、跟踪和问题区域。</span><span class="sxs-lookup"><span data-stu-id="ba24c-125">Application Insights presents system logging, tracing, and problem areas with highly visual dashboards.</span></span> <span data-ttu-id="ba24c-126">甚至可以呈现一个关系图，其中显示基于服务通信的服务之间的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="ba24c-126">It can even render a diagram showing the dependencies between services based on their communication.</span></span>

<span data-ttu-id="ba24c-127">但是，如果应用程序不能使用 Azure PaaS 和 IaaS 资源，该怎么办？</span><span class="sxs-lookup"><span data-stu-id="ba24c-127">However, what if an application can't use Azure PaaS and IaaS resources?</span></span> <span data-ttu-id="ba24c-128">是否仍可以利用 Application Insights 丰富的遥测体验？</span><span class="sxs-lookup"><span data-stu-id="ba24c-128">Is it still possible to take advantage of the rich telemetry experience of Application Insights?</span></span> <span data-ttu-id="ba24c-129">答案是肯定的。</span><span class="sxs-lookup"><span data-stu-id="ba24c-129">The answer is yes.</span></span> <span data-ttu-id="ba24c-130">非 Azure 应用程序可以导入库、添加配置和检测代码，以便将遥测发出到 Azure 应用程序 Insights。</span><span class="sxs-lookup"><span data-stu-id="ba24c-130">A non-Azure application can import libraries, add configuration, and instrument code to emit telemetry to Azure Application Insights.</span></span> <span data-ttu-id="ba24c-131">但是，这种方法将应用程序 **紧密耦合** 到 Application Insights。</span><span class="sxs-lookup"><span data-stu-id="ba24c-131">However, this approach **tightly couples** the application to Application Insights.</span></span> <span data-ttu-id="ba24c-132">将应用移到其他监视平台可能涉及到昂贵的重构。</span><span class="sxs-lookup"><span data-stu-id="ba24c-132">Moving the app to a different monitoring platform could involve expensive refactoring.</span></span> <span data-ttu-id="ba24c-133">避免在代码外进行紧密耦合并使用可观察性是不是很好吗？</span><span class="sxs-lookup"><span data-stu-id="ba24c-133">Wouldn't it be great to avoid tight coupling and consume observability outside of the code?</span></span>

<span data-ttu-id="ba24c-134">通过 Dapr，你可以。</span><span class="sxs-lookup"><span data-stu-id="ba24c-134">With Dapr, you can.</span></span> <span data-ttu-id="ba24c-135">让我们看看 Dapr 如何可以将可观察性添加到我们的分布式应用程序。</span><span class="sxs-lookup"><span data-stu-id="ba24c-135">Let's look at how Dapr can add observability to our distributed applications.</span></span>

## <a name="what-it-solves"></a><span data-ttu-id="ba24c-136">解决方法</span><span class="sxs-lookup"><span data-stu-id="ba24c-136">What it solves</span></span>

<span data-ttu-id="ba24c-137">Dapr 可观察性构建基块将可观察性与应用程序分离。</span><span class="sxs-lookup"><span data-stu-id="ba24c-137">The Dapr observability building block decouples observability from the application.</span></span> <span data-ttu-id="ba24c-138">它自动捕获由 Dapr 分支和 Dapr 系统服务生成的、构成 Dapr 控制平面的流量。</span><span class="sxs-lookup"><span data-stu-id="ba24c-138">It automatically captures traffic generated by Dapr sidecars and Dapr system services that make up the Dapr control plane.</span></span> <span data-ttu-id="ba24c-139">块将流量与跨多个服务的单个操作关联起来。</span><span class="sxs-lookup"><span data-stu-id="ba24c-139">The block correlates traffic from a single operation that spans multiple services.</span></span> <span data-ttu-id="ba24c-140">它还公开了系统的性能指标、资源使用情况和运行状况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-140">It also exposes performance metrics, resource utilization, and the health of the system.</span></span> <span data-ttu-id="ba24c-141">遥测以开放标准格式发布，使信息可以送入你选择的监视后端。</span><span class="sxs-lookup"><span data-stu-id="ba24c-141">Telemetry is published in open-standard formats enabling information to be fed into your monitoring back end of choice.</span></span> <span data-ttu-id="ba24c-142">在这里，可以对信息进行可视化、查询和分析。</span><span class="sxs-lookup"><span data-stu-id="ba24c-142">There, the information can be visualized, queried, and analyzed.</span></span>

<span data-ttu-id="ba24c-143">由于 Dapr 抽象掉了该管道，因此应用程序不知道如何实现可观察性。</span><span class="sxs-lookup"><span data-stu-id="ba24c-143">As Dapr abstracts away the plumbing, the application is unaware of how observability is implemented.</span></span> <span data-ttu-id="ba24c-144">无需引用库或实现自定义检测代码。</span><span class="sxs-lookup"><span data-stu-id="ba24c-144">There's no need to reference libraries or implement custom instrumentation code.</span></span> <span data-ttu-id="ba24c-145">Dapr 使开发人员能够专注于构建业务逻辑，而不是可观察性的管道。</span><span class="sxs-lookup"><span data-stu-id="ba24c-145">Dapr allows the developer to focus on building business logic and not observability plumbing.</span></span> <span data-ttu-id="ba24c-146">可观察性在 Dapr 级别配置，在服务之间保持一致，即使是由不同的团队创建，并使用不同的技术堆栈构建。</span><span class="sxs-lookup"><span data-stu-id="ba24c-146">Observability is configured at the Dapr level and is consistent across services, even when created by different teams, and built with different technology stacks.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="ba24c-147">工作原理</span><span class="sxs-lookup"><span data-stu-id="ba24c-147">How it works</span></span>

<span data-ttu-id="ba24c-148">Dapr 的 [挎斗体系结构](dapr-at-20000-feet.md#sidecar-architecture) 启用内置可观察性功能。</span><span class="sxs-lookup"><span data-stu-id="ba24c-148">Dapr's [sidecar architecture](dapr-at-20000-feet.md#sidecar-architecture) enables built-in observability features.</span></span> <span data-ttu-id="ba24c-149">服务通信时，Dapr 分支会截获流量并提取跟踪、指标和日志记录信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-149">As services communicate, Dapr sidecars intercept the traffic and extract tracing, metrics, and logging information.</span></span> <span data-ttu-id="ba24c-150">遥测以开放标准格式发布。</span><span class="sxs-lookup"><span data-stu-id="ba24c-150">Telemetry is published in an open standards format.</span></span> <span data-ttu-id="ba24c-151">默认情况下，Dapr 支持 [OpenTelemetry](https://opentelemetry.io/) 和 [Zipkin](https://zipkin.io/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-151">By default, Dapr supports [OpenTelemetry](https://opentelemetry.io/) and [Zipkin](https://zipkin.io/).</span></span>

<span data-ttu-id="ba24c-152">Dapr 提供可将遥测发布到不同后端监视工具的 [收集](https://docs.dapr.io/operations/monitoring/tracing/open-telemetry-collector/) 器。</span><span class="sxs-lookup"><span data-stu-id="ba24c-152">Dapr provides [collectors](https://docs.dapr.io/operations/monitoring/tracing/open-telemetry-collector/) that can publish telemetry to different back-end monitoring tools.</span></span> <span data-ttu-id="ba24c-153">这些工具提供了 Dapr 遥测用于分析和查询。</span><span class="sxs-lookup"><span data-stu-id="ba24c-153">These tools present Dapr telemetry for analysis and querying.</span></span> <span data-ttu-id="ba24c-154">图9-1 显示了 Dapr 可观察性体系结构：</span><span class="sxs-lookup"><span data-stu-id="ba24c-154">Figure 9-1 shows the Dapr observability architecture:</span></span>

![Dapr 可观察性体系结构](media/observability/observability-architecture.png)

<span data-ttu-id="ba24c-156">**图 9-1**。</span><span class="sxs-lookup"><span data-stu-id="ba24c-156">**Figure 9-1**.</span></span> <span data-ttu-id="ba24c-157">Dapr 可观察性体系结构。</span><span class="sxs-lookup"><span data-stu-id="ba24c-157">Dapr observability architecture.</span></span>

1. <span data-ttu-id="ba24c-158">服务 A 调用服务 B 上的操作。调用将从服务 A 的 Dapr 挎斗路由到服务 B 的挎斗。</span><span class="sxs-lookup"><span data-stu-id="ba24c-158">Service A calls an operation on Service B. The call is routed from a Dapr sidecar for Service A to a sidecar for Service B.</span></span>
1. <span data-ttu-id="ba24c-159">当服务 B 完成操作时，响应将通过 Dapr 分支发送回服务 A。</span><span class="sxs-lookup"><span data-stu-id="ba24c-159">When Service B completes the operation, a response is sent back to Service A through the Dapr sidecars.</span></span> <span data-ttu-id="ba24c-160">它们收集并发布每个请求和响应的所有可用遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-160">They gather and publish all available telemetry for every request and response.</span></span>
1. <span data-ttu-id="ba24c-161">配置的收集器引入遥测数据，并将其发送到监视后端。</span><span class="sxs-lookup"><span data-stu-id="ba24c-161">The configured collector ingests the telemetry and sends it to the monitoring back end.</span></span>  

<span data-ttu-id="ba24c-162">作为开发人员，请记住，添加可观察性不同于配置其他 Dapr 构建基块，如发布/订阅或状态管理。</span><span class="sxs-lookup"><span data-stu-id="ba24c-162">As a developer, keep in mind that adding observability is different from configuring other Dapr building blocks, like pub/sub or state management.</span></span> <span data-ttu-id="ba24c-163">添加收集器和监视后端，而不是引用构建基块。</span><span class="sxs-lookup"><span data-stu-id="ba24c-163">Instead of referencing a building block, you add a collector and a monitoring back end.</span></span> <span data-ttu-id="ba24c-164">图9-1 显示了可以配置多个收集器，它与不同的监视后端集成。</span><span class="sxs-lookup"><span data-stu-id="ba24c-164">Figure 9-1 shows it's possible to configure multiple collectors that integrate with different monitoring back ends.</span></span>

<span data-ttu-id="ba24c-165">在本章开头，已标识了四个分类的遥测。</span><span class="sxs-lookup"><span data-stu-id="ba24c-165">At the beginning of this chapter, four categories of telemetry were identified.</span></span> <span data-ttu-id="ba24c-166">以下各节将提供每个类别的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-166">The following sections will provide detail for each category.</span></span> <span data-ttu-id="ba24c-167">其中介绍了如何配置与常用监视后端集成的收集器。</span><span class="sxs-lookup"><span data-stu-id="ba24c-167">They'll include instruction on how to configure collectors that integrate with popular monitoring back ends.</span></span>

### <a name="distributed-tracing"></a><span data-ttu-id="ba24c-168">分布式跟踪</span><span class="sxs-lookup"><span data-stu-id="ba24c-168">Distributed tracing</span></span>

<span data-ttu-id="ba24c-169">分布式跟踪提供了跨分布式应用程序中的服务进行通信的流量。</span><span class="sxs-lookup"><span data-stu-id="ba24c-169">Distributed tracing provides insight into the traffic that flows across services in a distributed application.</span></span> <span data-ttu-id="ba24c-170">交换请求和响应消息的日志是用于解决问题的有用信息源。</span><span class="sxs-lookup"><span data-stu-id="ba24c-170">The log of exchanged request and response messages is an invaluable source of information for troubleshooting issues.</span></span> <span data-ttu-id="ba24c-171">硬部分是对来自相同操作的 *消息进行关联* 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-171">The hard part is *correlating messages* that originate from the same operation.</span></span>

<span data-ttu-id="ba24c-172">Dapr 使用 [W3C 跟踪上下文](https://www.w3.org/TR/trace-context) 来关联相关消息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-172">Dapr uses the [W3C Trace Context](https://www.w3.org/TR/trace-context) to correlate related messages.</span></span> <span data-ttu-id="ba24c-173">它将相同的上下文信息注入到形成唯一操作的请求和响应中。</span><span class="sxs-lookup"><span data-stu-id="ba24c-173">It injects the same context information into requests and responses that form a unique operation.</span></span> <span data-ttu-id="ba24c-174">图9-2 显示了相关的工作方式：</span><span class="sxs-lookup"><span data-stu-id="ba24c-174">Figure 9-2 shows how correlation works:</span></span>

![W3C 跟踪上下文示例](media/observability/w3c-trace-context.png)

<span data-ttu-id="ba24c-176">**图 9-2**。</span><span class="sxs-lookup"><span data-stu-id="ba24c-176">**Figure 9-2**.</span></span> <span data-ttu-id="ba24c-177">W3C 跟踪上下文示例。</span><span class="sxs-lookup"><span data-stu-id="ba24c-177">W3C Trace Context example.</span></span>

1. <span data-ttu-id="ba24c-178">服务 A 在服务 B 上调用操作。当服务 A 启动调用时，Dapr 将创建一个唯一的跟踪上下文并将其注入到请求中。</span><span class="sxs-lookup"><span data-stu-id="ba24c-178">Service A invokes an operation on Service B. As Service A starts the call, Dapr creates a unique trace context and injects it into the request.</span></span>
1. <span data-ttu-id="ba24c-179">服务 B 接收请求，并对服务 C 调用操作。 Dapr 检测传入请求包含跟踪上下文，并通过将其注入到服务 C 的传出请求来传播。</span><span class="sxs-lookup"><span data-stu-id="ba24c-179">Service B receives the request and invokes an operation on Service C. Dapr detects that the incoming request contains a trace context and propagates it by injecting it into the outgoing request to Service C.</span></span>  
1. <span data-ttu-id="ba24c-180">服务 C 接收请求并对其进行处理。</span><span class="sxs-lookup"><span data-stu-id="ba24c-180">Service C receives the request and handles it.</span></span> <span data-ttu-id="ba24c-181">Dapr 检测到传入请求包含跟踪上下文，并通过将其注入到服务 B 的传出响应来传播。</span><span class="sxs-lookup"><span data-stu-id="ba24c-181">Dapr detects that the incoming request contains a trace context and propagates it by injecting it into the outgoing response back to Service B.</span></span>
1. <span data-ttu-id="ba24c-182">服务 B 接收响应并对其进行处理。</span><span class="sxs-lookup"><span data-stu-id="ba24c-182">Service B receives the response and handles it.</span></span> <span data-ttu-id="ba24c-183">然后，它会创建新的响应，并通过将跟踪上下文注入到服务 A 的传出响应来传播跟踪上下文。</span><span class="sxs-lookup"><span data-stu-id="ba24c-183">It then creates a new response and propagates the trace context by injecting it into the outgoing response back to Service A.</span></span>

<span data-ttu-id="ba24c-184">一组共同的请求和响应称为 " *跟踪*"。</span><span class="sxs-lookup"><span data-stu-id="ba24c-184">A set of requests and responses that belong together is called a *trace*.</span></span> <span data-ttu-id="ba24c-185">图9-3 显示了一个跟踪：</span><span class="sxs-lookup"><span data-stu-id="ba24c-185">Figure 9-3 shows a trace:</span></span>

![跟踪和跨越](media/observability/traces-spans.png)

<span data-ttu-id="ba24c-187">**图 9-3**。</span><span class="sxs-lookup"><span data-stu-id="ba24c-187">**Figure 9-3**.</span></span> <span data-ttu-id="ba24c-188">跟踪和跨越。</span><span class="sxs-lookup"><span data-stu-id="ba24c-188">Traces and spans.</span></span>

<span data-ttu-id="ba24c-189">在图中，请注意跟踪如何表示在多个服务之间发生的唯一应用程序事务。</span><span class="sxs-lookup"><span data-stu-id="ba24c-189">In the figure, note how the trace represents a unique application transaction that takes place across many services.</span></span> <span data-ttu-id="ba24c-190">跟踪是 *范围* 的集合。</span><span class="sxs-lookup"><span data-stu-id="ba24c-190">A trace is a collection of *spans*.</span></span> <span data-ttu-id="ba24c-191">每个范围都代表跟踪内完成的单个操作或工作单元。</span><span class="sxs-lookup"><span data-stu-id="ba24c-191">Each span represents a single operation or unit of work done within the trace.</span></span> <span data-ttu-id="ba24c-192">跨越是在实现唯一事务的服务之间发送的请求和响应。</span><span class="sxs-lookup"><span data-stu-id="ba24c-192">Spans are the requests and responses that are sent between services that implement the unique transaction.</span></span>

<span data-ttu-id="ba24c-193">后续部分介绍如何通过将跟踪遥测数据发布到监视后端来检查跟踪遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-193">The next sections discuss how to inspect tracing telemetry by publishing it to a monitoring back end.</span></span>

#### <a name="use-a-zipkin-monitoring-back-end"></a><span data-ttu-id="ba24c-194">使用 Zipkin 监视后端</span><span class="sxs-lookup"><span data-stu-id="ba24c-194">Use a Zipkin monitoring back end</span></span>

<span data-ttu-id="ba24c-195">[Zipkin](https://zipkin.io/) 是开源分布式跟踪系统。</span><span class="sxs-lookup"><span data-stu-id="ba24c-195">[Zipkin](https://zipkin.io/) is an open-source distributed tracing system.</span></span> <span data-ttu-id="ba24c-196">它可以摄取和可视化遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-196">It can ingest and visualize telemetry data.</span></span> <span data-ttu-id="ba24c-197">Dapr 提供对 Zipkin 的默认支持。</span><span class="sxs-lookup"><span data-stu-id="ba24c-197">Dapr offers default support for Zipkin.</span></span> <span data-ttu-id="ba24c-198">下面的示例演示如何将 Zipkin 配置为可视化 Dapr 遥测。</span><span class="sxs-lookup"><span data-stu-id="ba24c-198">The following example demonstrates how to configure Zipkin to visualize Dapr telemetry.</span></span>

##### <a name="enable-and-configure-tracing"></a><span data-ttu-id="ba24c-199">启用和配置跟踪</span><span class="sxs-lookup"><span data-stu-id="ba24c-199">Enable and configure tracing</span></span>

<span data-ttu-id="ba24c-200">若要启动，必须使用 Dapr 配置文件为 Dapr 运行时启用跟踪。</span><span class="sxs-lookup"><span data-stu-id="ba24c-200">To start, tracing must be enabled for the Dapr runtime using a Dapr configuration file.</span></span> <span data-ttu-id="ba24c-201">下面是名为的配置文件的示例 `tracing-config.yaml` ：</span><span class="sxs-lookup"><span data-stu-id="ba24c-201">Here's an example of a configuration file named `tracing-config.yaml`:</span></span>  

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: tracing-config
  namespace: default
spec:
  tracing:
    samplingRate: "1"
    zipkin:
      endpointAddress: "http://zipkin.default.svc.cluster.local:9411/api/v2/spans"
```

<span data-ttu-id="ba24c-202">`samplingRate`属性指定用于发布跟踪的时间间隔。</span><span class="sxs-lookup"><span data-stu-id="ba24c-202">The `samplingRate` attribute specifies the interval used for publishing traces.</span></span> <span data-ttu-id="ba24c-203">该值必须介于 `0` 禁用 (跟踪) 并 `1` (发布每个跟踪) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-203">The value must be between `0` (tracing disabled) and `1` (every trace is published).</span></span> <span data-ttu-id="ba24c-204">例如，如果值为 `0.5` ，则发布每个其他跟踪，大大减少已发布的流量。</span><span class="sxs-lookup"><span data-stu-id="ba24c-204">With a value of `0.5`, for example, every other trace is published, significantly reducing published traffic.</span></span> <span data-ttu-id="ba24c-205">指向在 `endpointAddress` Kubernetes 群集中运行的 Zipkin 服务器上的终结点。</span><span class="sxs-lookup"><span data-stu-id="ba24c-205">The `endpointAddress` points to an endpoint on a Zipkin server running in a Kubernetes cluster.</span></span> <span data-ttu-id="ba24c-206">Zipkin 的默认端口为 `9411` 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-206">The default port for Zipkin is `9411`.</span></span> <span data-ttu-id="ba24c-207">必须使用 Kubernetes CLI 将配置应用到 Kubernetes 群集：</span><span class="sxs-lookup"><span data-stu-id="ba24c-207">The configuration must be applied to the Kubernetes cluster using the Kubernetes CLI:</span></span>

```console
kubectl apply -f tracing-config.yaml
```

##### <a name="install-the-zipkin-server"></a><span data-ttu-id="ba24c-208">安装 Zipkin 服务器</span><span class="sxs-lookup"><span data-stu-id="ba24c-208">Install the Zipkin server</span></span>

<span data-ttu-id="ba24c-209">在自承载模式下安装 Dapr 时，会自动安装 Zipkin 服务器，并在位于 `$HOME/.dapr/config.yaml` 或 Windows 上的默认配置文件中启用跟踪 `%USERPROFILE%\.dapr\config.yaml` 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-209">When installing Dapr in self-hosted mode, a Zipkin server is automatically installed and tracing is enabled in the default configuration file located in `$HOME/.dapr/config.yaml` or `%USERPROFILE%\.dapr\config.yaml` on Windows.</span></span>

<span data-ttu-id="ba24c-210">但是，在 Kubernetes 群集上安装 Dapr 时，默认情况下不会添加 Zipkin。</span><span class="sxs-lookup"><span data-stu-id="ba24c-210">When installing Dapr on a Kubernetes cluster though, Zipkin isn't added by default.</span></span> <span data-ttu-id="ba24c-211">以下名为的 Kubernetes 清单文件将 `zipkin.yaml` 标准 Zipkin 服务器部署到群集：</span><span class="sxs-lookup"><span data-stu-id="ba24c-211">The following Kubernetes manifest file named `zipkin.yaml`, deploys a standard Zipkin server to the cluster:</span></span>

```yaml
kind: Deployment
apiVersion: apps/v1
metadata:
  name: zipkin
  namespace: eshop
  labels:
    service: zipkin
spec:
  replicas: 1
  selector:
    matchLabels:
      service: zipkin
  template:
    metadata:
      labels:
        service: zipkin
    spec:
      containers:
        - name: zipkin
          image: openzipkin/zipkin-slim
          imagePullPolicy: IfNotPresent
          ports:
            - name: http
              containerPort: 9411
              protocol: TCP

---

kind: Service
apiVersion: v1
metadata:
  name: zipkin
  namespace: eshop
  labels:
    service: zipkin
spec:
  type: NodePort
  ports:
    - port: 9411
      targetPort: 9411
      nodePort: 32411
      protocol: TCP
      name: zipkin
  selector:
    service: zipkin

```

<span data-ttu-id="ba24c-212">部署使用标准 `openzipkin/zipkin-slim` 容器映像。</span><span class="sxs-lookup"><span data-stu-id="ba24c-212">The deployment uses the standard `openzipkin/zipkin-slim` container image.</span></span> <span data-ttu-id="ba24c-213">Zipkin 服务公开 Zipkin web 前端，你可以使用它来查看端口上的遥测数据 `32411` 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-213">The Zipkin service exposes the Zipkin web front end, which you can use to view the telemetry on port `32411`.</span></span> <span data-ttu-id="ba24c-214">使用 Kubernetes CLI 将 Zipkin 清单文件应用到 Kubernetes 群集并部署 Zipkin 服务器：</span><span class="sxs-lookup"><span data-stu-id="ba24c-214">Use the Kubernetes CLI to apply the Zipkin manifest file to the Kubernetes cluster and deploy the Zipkin server:</span></span>

```console
kubectl apply -f zipkin.yaml
```

##### <a name="configure-the-services-to-use-the-tracing-configuration"></a><span data-ttu-id="ba24c-215">将服务配置为使用跟踪配置</span><span class="sxs-lookup"><span data-stu-id="ba24c-215">Configure the services to use the tracing configuration</span></span>

<span data-ttu-id="ba24c-216">现在，所有内容都已正确设置，可以开始发布遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-216">Now everything is set up correctly to start publishing telemetry.</span></span> <span data-ttu-id="ba24c-217">在应用程序中部署的每个 Dapr 挎斗都必须在启动时指示发出遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-217">Every Dapr sidecar that is deployed as part of the application must be instructed to emit telemetry when started.</span></span> <span data-ttu-id="ba24c-218">为此，请将 `dapr.io/config` 引用配置的批注添加 `tracing-config` 到每个服务的部署中。</span><span class="sxs-lookup"><span data-stu-id="ba24c-218">To do that, add a `dapr.io/config` annotation that references the `tracing-config` configuration to the deployment of each service.</span></span> <span data-ttu-id="ba24c-219">下面是包含批注的 eShop 排序 API 服务的清单文件的示例：</span><span class="sxs-lookup"><span data-stu-id="ba24c-219">Here's an example of the eShop ordering API service's manifest file containing the annotation:</span></span>

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: ordering-api
  namespace: eshop
  labels:
    app: eshop
spec:
  replicas: 1
  selector:
    matchLabels:
      app: eshop
  template:
    metadata:
      labels:
        app: simulation
      annotations:
        dapr.io/enabled: "true"
        dapr.io/app-id: "ordering-api"
        dapr.io/config: "tracing-config"
    spec:
      containers:
      - name: simulation
        image: eshop/ordering.api:linux-latest
```

##### <a name="inspect-the-telemetry-in-zipkin"></a><span data-ttu-id="ba24c-220">检查 Zipkin 中的遥测数据</span><span class="sxs-lookup"><span data-stu-id="ba24c-220">Inspect the telemetry in Zipkin</span></span>

<span data-ttu-id="ba24c-221">启动应用程序后，Dapr 分支将向 Zipkin 服务器发出遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-221">Once the application is started, the Dapr sidecars will emit telemetry to the Zipkin server.</span></span> <span data-ttu-id="ba24c-222">若要检查此遥测数据，请将 web 浏览器指向 <http://localhost:32411> 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-222">To inspect this telemetry, point a web-browser to <http://localhost:32411>.</span></span> <span data-ttu-id="ba24c-223">你将看到 Zipkin web 前端：</span><span class="sxs-lookup"><span data-stu-id="ba24c-223">You'll see the Zipkin web front end:</span></span>

![Zipkin 起始页](media/observability/zipkin.png)

<span data-ttu-id="ba24c-225">在 " *查找跟踪* " 选项卡上，可以查询跟踪。</span><span class="sxs-lookup"><span data-stu-id="ba24c-225">On the *Find a trace* tab, you can query traces.</span></span> <span data-ttu-id="ba24c-226">按 " *运行查询* " 按钮而不指定任何限制将显示所有引入 *跟踪*：</span><span class="sxs-lookup"><span data-stu-id="ba24c-226">Pressing the *RUN QUERY* button without specifying any restrictions will show all the ingested *traces*:</span></span>

![跟踪列表](media/observability/zipkin-traces-overview.png)

<span data-ttu-id="ba24c-228">单击特定跟踪旁边的 " *显示* " 按钮将显示该跟踪的详细信息：</span><span class="sxs-lookup"><span data-stu-id="ba24c-228">Clicking the *SHOW* button next to a specific trace, will show the details of that trace:</span></span>

![跟踪的详细信息](media/observability/zipkin-trace-details.png)

<span data-ttu-id="ba24c-230">详细信息页上的每一项都是一个范围，该范围表示作为选定跟踪的一部分的请求。</span><span class="sxs-lookup"><span data-stu-id="ba24c-230">Each item on the details page, is a span that represents a request that is part of the selected trace.</span></span>

##### <a name="inspect-the-dependencies-between-services"></a><span data-ttu-id="ba24c-231">检查服务之间的依赖关系</span><span class="sxs-lookup"><span data-stu-id="ba24c-231">Inspect the dependencies between services</span></span>

<span data-ttu-id="ba24c-232">由于 Dapr 分支处理服务之间的流量，因此 Zipkin 可以使用跟踪信息来确定服务之间的依赖关系。</span><span class="sxs-lookup"><span data-stu-id="ba24c-232">Because Dapr sidecars handle traffic between services, Zipkin can use the trace information to determine the dependencies between the services.</span></span> <span data-ttu-id="ba24c-233">若要查看其工作方式，请在 Zipkin 网页上，单击 " *依赖项* " 选项卡，然后选择带有放大镜的按钮。</span><span class="sxs-lookup"><span data-stu-id="ba24c-233">To see it in action, go to the *Dependencies* tab on the Zipkin web page and select the button with the magnifying glass.</span></span> <span data-ttu-id="ba24c-234">Zipkin 将显示服务及其依赖项的概述：</span><span class="sxs-lookup"><span data-stu-id="ba24c-234">Zipkin will show an overview of the services and their dependencies:</span></span>

![Zipkin 中的依赖项关系图](media/observability/zipkin-dependencies.png)

<span data-ttu-id="ba24c-236">服务之间的线条上的动画点表示请求，并从源移动到目标。</span><span class="sxs-lookup"><span data-stu-id="ba24c-236">The animated dots on the lines between the services represent requests and move from source to destination.</span></span> <span data-ttu-id="ba24c-237">红点表示失败的请求。</span><span class="sxs-lookup"><span data-stu-id="ba24c-237">Red dots indicate a failed request.</span></span>

#### <a name="use-a-jaeger-or-new-relic-monitoring-back-end"></a><span data-ttu-id="ba24c-238">使用 Jaeger 或 New Relic 监视后端</span><span class="sxs-lookup"><span data-stu-id="ba24c-238">Use a Jaeger or New Relic monitoring back end</span></span>

<span data-ttu-id="ba24c-239">除了 Zipkin 本身以外，其他监视后端软件还支持使用 Zipkin 格式的引入遥测。</span><span class="sxs-lookup"><span data-stu-id="ba24c-239">Beyond Zipkin itself, other monitoring back-end software also supports ingesting telemetry using the Zipkin format.</span></span> <span data-ttu-id="ba24c-240">[Jaeger](https://www.jaegertracing.io/) 是由 Uber 技术创建的开源跟踪系统。</span><span class="sxs-lookup"><span data-stu-id="ba24c-240">[Jaeger](https://www.jaegertracing.io/) is an open source tracing system created by Uber Technologies.</span></span> <span data-ttu-id="ba24c-241">它用于跟踪分布式服务之间的事务，并对复杂的微服务环境进行故障排除。</span><span class="sxs-lookup"><span data-stu-id="ba24c-241">It's used to trace transactions between distributed services and troubleshoot complex microservices environments.</span></span> <span data-ttu-id="ba24c-242">[New Relic](https://newrelic.com/) 是一个 *全堆栈* 可观察性平台。</span><span class="sxs-lookup"><span data-stu-id="ba24c-242">[New Relic](https://newrelic.com/) is a *full-stack* observability platform.</span></span> <span data-ttu-id="ba24c-243">它链接分布式应用程序中的相关数据，以提供完整的系统画面。</span><span class="sxs-lookup"><span data-stu-id="ba24c-243">It links relevant data from a distributed application to provide a complete picture of your system.</span></span> <span data-ttu-id="ba24c-244">若要试用，请 `endpointAddress` 在 Dapr 配置文件中指定指向 Jaeger 或新 Relic 服务器的。</span><span class="sxs-lookup"><span data-stu-id="ba24c-244">To try them out, specify an `endpointAddress` pointing to either a Jaeger or New Relic server in the Dapr configuration file.</span></span> <span data-ttu-id="ba24c-245">下面是配置文件的一个示例，它将 Dapr 配置为将遥测数据发送到 Jaeger 服务器。</span><span class="sxs-lookup"><span data-stu-id="ba24c-245">Here's an example of a configuration file that configures Dapr to send telemetry to a Jaeger server.</span></span> <span data-ttu-id="ba24c-246">Jaeger 的 URL 与 Zipkin 的 URL 相同。</span><span class="sxs-lookup"><span data-stu-id="ba24c-246">The URL for Jaeger is identical to the URL for the Zipkin.</span></span> <span data-ttu-id="ba24c-247">唯一的区别是服务器运行的端口：</span><span class="sxs-lookup"><span data-stu-id="ba24c-247">The only difference is the port on which the server runs:</span></span>

 ```yaml
 apiVersion: dapr.io/v1alpha1
 kind: Configuration
 metadata:
   name: tracing-config
   namespace: default
 spec:
   tracing:
     samplingRate: "1"
     zipkin:
       endpointAddress: "http://localhost:9415/api/v2/spans"
 ```

<span data-ttu-id="ba24c-248">若要试用 New Relic，请指定新的 Relic API 的终结点。</span><span class="sxs-lookup"><span data-stu-id="ba24c-248">To try out New Relic, specify the endpoint of the New Relic API.</span></span> <span data-ttu-id="ba24c-249">下面是新 Relic 的配置文件示例：</span><span class="sxs-lookup"><span data-stu-id="ba24c-249">Here's an example of a configuration file for New Relic:</span></span>

 ```yaml
apiVersion: dapr.io/v1alpha1
 kind: Configuration
 metadata:
   name: tracing-config
   namespace: default
 spec:
   tracing:
     samplingRate: "1"
     zipkin:
       endpointAddress: "https://trace-api.newrelic.com/trace/v1?Api-Key=<NR-API-KEY>&Data-Format=zipkin&Data-Format-Version=2"
 ```

<span data-ttu-id="ba24c-250">有关如何使用的详细信息，请查看 Jaeger 和新 Relic 网站。</span><span class="sxs-lookup"><span data-stu-id="ba24c-250">Check out the Jaeger and New Relic websites for more information on how to use them.</span></span>

### <a name="metrics"></a><span data-ttu-id="ba24c-251">指标</span><span class="sxs-lookup"><span data-stu-id="ba24c-251">Metrics</span></span>

<span data-ttu-id="ba24c-252">指标提供性能和资源使用情况的见解。</span><span class="sxs-lookup"><span data-stu-id="ba24c-252">Metrics provide insight into performance and resource consumption.</span></span> <span data-ttu-id="ba24c-253">Dapr 会发出大量系统和运行时指标。</span><span class="sxs-lookup"><span data-stu-id="ba24c-253">Under the hood, Dapr emits a wide collection of system and runtime metrics.</span></span> <span data-ttu-id="ba24c-254">Dapr 使用 [Prometheus](https://prometheus.io/) 作为度量标准。</span><span class="sxs-lookup"><span data-stu-id="ba24c-254">Dapr uses [Prometheus](https://prometheus.io/) as a metric standard.</span></span> <span data-ttu-id="ba24c-255">Dapr 分支和系统服务，会在端口上公开指标终结点 `9090` 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-255">Dapr sidecars and system services, expose a metrics endpoint on port `9090`.</span></span> <span data-ttu-id="ba24c-256">*Prometheus scraper* 按预定义的间隔调用此终结点，以收集指标。</span><span class="sxs-lookup"><span data-stu-id="ba24c-256">A *Prometheus scraper* calls this endpoint at a predefined interval to collect metrics.</span></span> <span data-ttu-id="ba24c-257">Scraper 将指标值发送到监视后端。</span><span class="sxs-lookup"><span data-stu-id="ba24c-257">The scraper sends metric values to a monitoring back end.</span></span> <span data-ttu-id="ba24c-258">图9-4 显示了抓取过程：</span><span class="sxs-lookup"><span data-stu-id="ba24c-258">Figure 9-4 shows the scraping process:</span></span>

![抓取 Prometheus 指标](media/observability/prometheus-scraper.png)

<span data-ttu-id="ba24c-260">**图 9-4**。</span><span class="sxs-lookup"><span data-stu-id="ba24c-260">**Figure 9-4**.</span></span> <span data-ttu-id="ba24c-261">抓取 Prometheus 指标。</span><span class="sxs-lookup"><span data-stu-id="ba24c-261">Scraping Prometheus metrics.</span></span>

<span data-ttu-id="ba24c-262">在上图中，每个挎斗和系统服务都公开了侦听端口9090的指标终结点。</span><span class="sxs-lookup"><span data-stu-id="ba24c-262">In the above figure, each sidecar and system service exposes a metric endpoint that listens on port 9090.</span></span> <span data-ttu-id="ba24c-263">Prometheus 指标 Scrapper 从每个终结点捕获指标，并将信息发布到监视后端。</span><span class="sxs-lookup"><span data-stu-id="ba24c-263">The Prometheus Metrics Scrapper captures metrics from each endpoint and published the information to the monitoring back end.</span></span>  

#### <a name="service-discovery"></a><span data-ttu-id="ba24c-264">服务发现</span><span class="sxs-lookup"><span data-stu-id="ba24c-264">Service discovery</span></span>

<span data-ttu-id="ba24c-265">您可能想知道指标 scraper 如何知道收集指标的位置。</span><span class="sxs-lookup"><span data-stu-id="ba24c-265">You might wonder how the metrics scraper knows where to collect metrics.</span></span> <span data-ttu-id="ba24c-266">Prometheus 可以与内置于目标部署环境中的发现机制集成。</span><span class="sxs-lookup"><span data-stu-id="ba24c-266">Prometheus can integrate with discovery mechanisms built into target deployment environments.</span></span> <span data-ttu-id="ba24c-267">例如，在 Kubernetes 中运行时，Prometheus 可与 Kubernetes API 集成，以查找在该环境中运行的所有可用 Kubernetes 资源。</span><span class="sxs-lookup"><span data-stu-id="ba24c-267">For example, when running in Kubernetes, Prometheus can integrate with the Kubernetes API to find all available Kubernetes resources running in the environment.</span></span>

#### <a name="metrics-list"></a><span data-ttu-id="ba24c-268">指标列表</span><span class="sxs-lookup"><span data-stu-id="ba24c-268">Metrics list</span></span>

<span data-ttu-id="ba24c-269">Dapr 为 Dapr 系统服务及其运行时生成一组大量指标。</span><span class="sxs-lookup"><span data-stu-id="ba24c-269">Dapr generates a large set of metrics for Dapr system services and its runtime.</span></span> <span data-ttu-id="ba24c-270">示例包括：</span><span class="sxs-lookup"><span data-stu-id="ba24c-270">Some examples include:</span></span>

| <span data-ttu-id="ba24c-271">指标</span><span class="sxs-lookup"><span data-stu-id="ba24c-271">Metric</span></span>                                         | <span data-ttu-id="ba24c-272">源</span><span class="sxs-lookup"><span data-stu-id="ba24c-272">Source</span></span> | <span data-ttu-id="ba24c-273">说明</span><span class="sxs-lookup"><span data-stu-id="ba24c-273">Description</span></span>                                                  |
| ---------------------------------------------- | :----: | ------------------------------------------------------------ |
| <span data-ttu-id="ba24c-274">dapr_operator_service_created_total</span><span class="sxs-lookup"><span data-stu-id="ba24c-274">dapr_operator_service_created_total</span></span>            | <span data-ttu-id="ba24c-275">系统</span><span class="sxs-lookup"><span data-stu-id="ba24c-275">System</span></span> | <span data-ttu-id="ba24c-276">Dapr Operator service 创建的 Dapr 服务总数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-276">The total number of Dapr services created by the Dapr Operator service.</span></span> |
| <span data-ttu-id="ba24c-277">dapr_injector_sidecar_injection/requests_total</span><span class="sxs-lookup"><span data-stu-id="ba24c-277">dapr_injector_sidecar_injection/requests_total</span></span> | <span data-ttu-id="ba24c-278">系统</span><span class="sxs-lookup"><span data-stu-id="ba24c-278">System</span></span> | <span data-ttu-id="ba24c-279">Dapr Sidecar-Injector 服务收到的挎斗注入请求总数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-279">The total number of sidecar injection requests received by the Dapr Sidecar-Injector service.</span></span> |
| <span data-ttu-id="ba24c-280">dapr_placement_runtimes_total</span><span class="sxs-lookup"><span data-stu-id="ba24c-280">dapr_placement_runtimes_total</span></span>                  | <span data-ttu-id="ba24c-281">系统</span><span class="sxs-lookup"><span data-stu-id="ba24c-281">System</span></span> | <span data-ttu-id="ba24c-282">向 Dapr 放置服务报告的主机总数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-282">The total number of hosts reported to the Dapr Placement service.</span></span> |
| <span data-ttu-id="ba24c-283">dapr_sentry_cert_sign_request_received_total</span><span class="sxs-lookup"><span data-stu-id="ba24c-283">dapr_sentry_cert_sign_request_received_total</span></span>   | <span data-ttu-id="ba24c-284">系统</span><span class="sxs-lookup"><span data-stu-id="ba24c-284">System</span></span> | <span data-ttu-id="ba24c-285">Dapr Sentry 服务)  (收到的证书签名请求数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-285">The number of certificate signing requests (CRSs) received by the Dapr Sentry service.</span></span> |
| <span data-ttu-id="ba24c-286">dapr_runtime_component_loaded</span><span class="sxs-lookup"><span data-stu-id="ba24c-286">dapr_runtime_component_loaded</span></span>      | <span data-ttu-id="ba24c-287">运行时</span><span class="sxs-lookup"><span data-stu-id="ba24c-287">Runtime</span></span> | <span data-ttu-id="ba24c-288">已成功加载的 Dapr 组件数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-288">The number of successfully loaded Dapr components.</span></span>           |
| <span data-ttu-id="ba24c-289">dapr_grpc_io_server_completed_rpcs</span><span class="sxs-lookup"><span data-stu-id="ba24c-289">dapr_grpc_io_server_completed_rpcs</span></span> | <span data-ttu-id="ba24c-290">运行时</span><span class="sxs-lookup"><span data-stu-id="ba24c-290">Runtime</span></span> | <span data-ttu-id="ba24c-291">按方法和状态的 gRPC 调用的计数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-291">Count of gRPC calls by method and status.</span></span>                    |
| <span data-ttu-id="ba24c-292">dapr_http_server_request_count</span><span class="sxs-lookup"><span data-stu-id="ba24c-292">dapr_http_server_request_count</span></span>     | <span data-ttu-id="ba24c-293">运行时</span><span class="sxs-lookup"><span data-stu-id="ba24c-293">Runtime</span></span> | <span data-ttu-id="ba24c-294">HTTP 服务器中启动的 HTTP 请求数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-294">Number of HTTP requests started in an HTTP server.</span></span>           |
| <span data-ttu-id="ba24c-295">dapr_http/client/sent_bytes</span><span class="sxs-lookup"><span data-stu-id="ba24c-295">dapr_http/client/sent_bytes</span></span>        | <span data-ttu-id="ba24c-296">运行时</span><span class="sxs-lookup"><span data-stu-id="ba24c-296">Runtime</span></span> | <span data-ttu-id="ba24c-297">请求正文中发送的总字节数 (不包括 HTTP 客户端) 的标头。</span><span class="sxs-lookup"><span data-stu-id="ba24c-297">Total bytes sent in request body (not including headers) by an HTTP client.</span></span> |

<span data-ttu-id="ba24c-298">有关可用指标的详细信息，请参阅 [Dapr 指标文档](https://docs.dapr.io/operations/monitoring/metrics/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-298">For more information on available metrics, see the [Dapr metrics documentation](https://docs.dapr.io/operations/monitoring/metrics/).</span></span>

#### <a name="configure-dapr-metrics"></a><span data-ttu-id="ba24c-299">配置 Dapr 指标</span><span class="sxs-lookup"><span data-stu-id="ba24c-299">Configure Dapr metrics</span></span>

<span data-ttu-id="ba24c-300">在运行时，可以通过在 `--enable-metrics=false` Dapr 命令中包含自变量来禁用指标集合终结点。</span><span class="sxs-lookup"><span data-stu-id="ba24c-300">At runtime, you can disable the metrics collection endpoint by including the `--enable-metrics=false` argument in the Dapr command.</span></span> <span data-ttu-id="ba24c-301">或者，还可以更改具有参数的终结点的默认端口 `--metrics-port 9090` 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-301">Or, you can also change the default port for the endpoint with the `--metrics-port 9090` argument.</span></span>

<span data-ttu-id="ba24c-302">还可以使用 Dapr 配置文件来静态地启用或禁用运行时指标收集：</span><span class="sxs-lookup"><span data-stu-id="ba24c-302">You can also use a Dapr configuration file to statically enable or disable runtime metrics collection:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: dapr-config
  namespace: eshop
spec:
  tracing:
    samplingRate: "1"
  metric:
    enabled: false
```

#### <a name="visualize-dapr-metrics"></a><span data-ttu-id="ba24c-303">可视化 Dapr 指标</span><span class="sxs-lookup"><span data-stu-id="ba24c-303">Visualize Dapr metrics</span></span>

<span data-ttu-id="ba24c-304">通过 Prometheus scraper 收集指标并将其发布到监视后端，你如何了解原始数据？</span><span class="sxs-lookup"><span data-stu-id="ba24c-304">With the Prometheus scraper collecting and publishing metrics into the monitoring back end, how do you make sense of the raw data?</span></span> <span data-ttu-id="ba24c-305">用于分析指标的常用可视化工具是 [Grafana](https://grafana.com/grafana/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-305">A popular visualization tool for analyzing metrics is [Grafana](https://grafana.com/grafana/).</span></span> <span data-ttu-id="ba24c-306">使用 Grafana 可以从可用指标创建仪表板。</span><span class="sxs-lookup"><span data-stu-id="ba24c-306">With Grafana, you can create dashboards from the available metrics.</span></span> <span data-ttu-id="ba24c-307">下面是一个显示 Dapr 系统服务指标的仪表板示例：</span><span class="sxs-lookup"><span data-stu-id="ba24c-307">Here's an example of a dashboard displaying Dapr system services metrics:</span></span>

![显示 Dapr 系统服务指标的 Grafana 仪表板](media/observability/grafana-sample.png)

<span data-ttu-id="ba24c-309">Dapr 文档包含 [用于安装 Prometheus 和 Grafana 的教程](https://docs.dapr.io/operations/monitoring/metrics/grafana/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-309">The Dapr documentation includes a [tutorial for installing Prometheus and Grafana](https://docs.dapr.io/operations/monitoring/metrics/grafana/).</span></span>

### <a name="logging"></a><span data-ttu-id="ba24c-310">日志记录</span><span class="sxs-lookup"><span data-stu-id="ba24c-310">Logging</span></span>

<span data-ttu-id="ba24c-311">日志记录提供了在运行时服务发生的情况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-311">Logging provides insight into what is happening with a service at runtime.</span></span> <span data-ttu-id="ba24c-312">运行应用程序时，Dapr 会自动从 Dapr 分支和 Dapr 系统服务发出日志条目。</span><span class="sxs-lookup"><span data-stu-id="ba24c-312">When running an application, Dapr automatically emits log entries from Dapr sidecars and Dapr system services.</span></span> <span data-ttu-id="ba24c-313">但是，在应用程序代码中检测到的日志记录项 **不** 会自动包括在内。</span><span class="sxs-lookup"><span data-stu-id="ba24c-313">However, logging entries instrumented in your application code **aren't** automatically included.</span></span> <span data-ttu-id="ba24c-314">若要从应用程序代码发出日志记录，可以导入特定 SDK，如 [OPENTELEMETRY sdk for .net](https://opentelemetry.io/docs/net/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-314">To emit logging from application code, you can import a specific SDK like [OpenTelemetry SDK for .NET](https://opentelemetry.io/docs/net/).</span></span> <span data-ttu-id="ba24c-315">日志记录应用程序代码将在本章后面的 *使用 Dapr .NET SDK* 部分中介绍。</span><span class="sxs-lookup"><span data-stu-id="ba24c-315">Logging application code is covered later in this chapter in the section *Using the Dapr .NET SDK*.</span></span>  

#### <a name="log-entry-structure"></a><span data-ttu-id="ba24c-316">日志项目结构</span><span class="sxs-lookup"><span data-stu-id="ba24c-316">Log entry structure</span></span>

<span data-ttu-id="ba24c-317">Dapr 发出结构化日志记录。</span><span class="sxs-lookup"><span data-stu-id="ba24c-317">Dapr emits structured logging.</span></span> <span data-ttu-id="ba24c-318">每个日志条目都具有以下格式：</span><span class="sxs-lookup"><span data-stu-id="ba24c-318">Each log entry has the following format:</span></span>

| <span data-ttu-id="ba24c-319">字段</span><span class="sxs-lookup"><span data-stu-id="ba24c-319">Field</span></span>    | <span data-ttu-id="ba24c-320">说明</span><span class="sxs-lookup"><span data-stu-id="ba24c-320">Description</span></span>                                          | <span data-ttu-id="ba24c-321">示例</span><span class="sxs-lookup"><span data-stu-id="ba24c-321">Example</span></span>                             |
| -------- | ---------------------------------------------------- | ----------------------------------- |
| <span data-ttu-id="ba24c-322">time</span><span class="sxs-lookup"><span data-stu-id="ba24c-322">time</span></span>     | <span data-ttu-id="ba24c-323">ISO8601 格式的时间戳</span><span class="sxs-lookup"><span data-stu-id="ba24c-323">ISO8601 formatted timestamp</span></span>                          | `2021-01-10T14:19:31.000Z`          |
| <span data-ttu-id="ba24c-324">级别</span><span class="sxs-lookup"><span data-stu-id="ba24c-324">level</span></span>    | <span data-ttu-id="ba24c-325">条目 (`debug` \| `info` \| `warn` \| `error`) 级别  </span><span class="sxs-lookup"><span data-stu-id="ba24c-325">Level of the entry (`debug` \| `info` \| `warn`  \| `error`)</span></span> | `info`                              |
| <span data-ttu-id="ba24c-326">类型</span><span class="sxs-lookup"><span data-stu-id="ba24c-326">type</span></span>     | <span data-ttu-id="ba24c-327">日志类型</span><span class="sxs-lookup"><span data-stu-id="ba24c-327">Log Type</span></span>                                             | `log`                               |
| <span data-ttu-id="ba24c-328">msg</span><span class="sxs-lookup"><span data-stu-id="ba24c-328">msg</span></span>      | <span data-ttu-id="ba24c-329">日志消息</span><span class="sxs-lookup"><span data-stu-id="ba24c-329">Log Message</span></span>                                          | `metrics server started on :62408/` |
| <span data-ttu-id="ba24c-330">scope</span><span class="sxs-lookup"><span data-stu-id="ba24c-330">scope</span></span>    | <span data-ttu-id="ba24c-331">日志范围</span><span class="sxs-lookup"><span data-stu-id="ba24c-331">Logging Scope</span></span>                                        | `dapr.runtime`                      |
| <span data-ttu-id="ba24c-332">instance</span><span class="sxs-lookup"><span data-stu-id="ba24c-332">instance</span></span> | <span data-ttu-id="ba24c-333">Dapr 运行的主机名</span><span class="sxs-lookup"><span data-stu-id="ba24c-333">Hostname where Dapr runs</span></span>                             | <span data-ttu-id="ba24c-334">TSTSRV01</span><span class="sxs-lookup"><span data-stu-id="ba24c-334">TSTSRV01</span></span>                            |
| <span data-ttu-id="ba24c-335">app_id</span><span class="sxs-lookup"><span data-stu-id="ba24c-335">app_id</span></span>   | <span data-ttu-id="ba24c-336">Dapr 应用 ID</span><span class="sxs-lookup"><span data-stu-id="ba24c-336">Dapr App ID</span></span>                                          | <span data-ttu-id="ba24c-337">ordering-api</span><span class="sxs-lookup"><span data-stu-id="ba24c-337">ordering-api</span></span>                        |
| <span data-ttu-id="ba24c-338">ver</span><span class="sxs-lookup"><span data-stu-id="ba24c-338">ver</span></span>      | <span data-ttu-id="ba24c-339">Dapr 运行时版本</span><span class="sxs-lookup"><span data-stu-id="ba24c-339">Dapr Runtime Version</span></span>                                 | <span data-ttu-id="ba24c-340">`1.0.0`-rc. 2</span><span class="sxs-lookup"><span data-stu-id="ba24c-340">`1.0.0`-rc.2</span></span>                        |

<span data-ttu-id="ba24c-341">在故障排除方案中搜索日志记录条目时， `time` 和 `level` 字段特别有用。</span><span class="sxs-lookup"><span data-stu-id="ba24c-341">When searching through logging entries in a troubleshooting scenario, the `time` and `level` fields are especially helpful.</span></span> <span data-ttu-id="ba24c-342">时间字段对日志条目进行排序，以便您可以确定特定时间段。</span><span class="sxs-lookup"><span data-stu-id="ba24c-342">The time field orders log entries so that you can pinpoint specific time periods.</span></span> <span data-ttu-id="ba24c-343">进行故障排除时， *调试级别* 的日志项提供有关代码行为的详细信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-343">When troubleshooting, log entries at the *debug level* provide more information on the behavior of the code.</span></span>

#### <a name="plain-text-versus-json-format"></a><span data-ttu-id="ba24c-344">纯文本与 JSON 格式</span><span class="sxs-lookup"><span data-stu-id="ba24c-344">Plain text versus JSON format</span></span>

<span data-ttu-id="ba24c-345">默认情况下，Dapr 以纯文本格式发出结构化日志记录。</span><span class="sxs-lookup"><span data-stu-id="ba24c-345">By default, Dapr emits structured logging in plain-text format.</span></span> <span data-ttu-id="ba24c-346">每个日志条目的格式为包含键/值对的字符串。</span><span class="sxs-lookup"><span data-stu-id="ba24c-346">Every log entry is formatted as a string containing key/value pairs.</span></span> <span data-ttu-id="ba24c-347">下面是以纯文本形式记录的示例：</span><span class="sxs-lookup"><span data-stu-id="ba24c-347">Here's an example of logging in plain text:</span></span>

```text
== DAPR == time="2021-01-12T16:11:39.4669323+01:00" level=info msg="starting Dapr Runtime -- version 1.0.0-rc.2 -- commit 196483d" app_id=ordering-api instance=TSTSRV03 scope=dapr.runtime type=log ver=1.0.0-rc.2
== DAPR == time="2021-01-12T16:11:39.467933+01:00" level=info msg="log level set to: info" app_id=ordering-api instance=TSTSRV03 scope=dapr.runtime type=log ver=1.0.0-rc.2
== DAPR == time="2021-01-12T16:11:39.467933+01:00" level=info msg="metrics server started on :62408/" app_id=ordering-api instance=TSTSRV03 scope=dapr.metrics type=log ver=1.0.0-rc.2
```

<span data-ttu-id="ba24c-348">尽管简单，但难以分析这种格式。</span><span class="sxs-lookup"><span data-stu-id="ba24c-348">While simple, this format is difficult to parse.</span></span> <span data-ttu-id="ba24c-349">如果使用监视工具查看日志条目，则需要启用 JSON 格式的日志记录。</span><span class="sxs-lookup"><span data-stu-id="ba24c-349">If viewing log entries with a monitoring tool, you'll want to enable JSON formatted logging.</span></span> <span data-ttu-id="ba24c-350">使用 JSON 项，监视工具可以对各个字段进行索引和查询。</span><span class="sxs-lookup"><span data-stu-id="ba24c-350">With JSON entries, a monitoring tool can index and query individual fields.</span></span> <span data-ttu-id="ba24c-351">下面是 JSON 格式的相同日志条目：</span><span class="sxs-lookup"><span data-stu-id="ba24c-351">Here's the same log entries in JSON format:</span></span>

```json
{"app_id": "ordering-api", "instance": "TSTSRV03", "level": "info", "msg": "starting Dapr Runtime -- version 1.0.0-rc.2 -- commit 196483d", "scope": "dapr.runtime", "time": "2021-01-12T16:11:39.4669323+01:00", "type": "log", "ver": "1.0.0-rc.2"}
{"app_id": "ordering-api", "instance": "TSTSRV03", "level": "info", "msg": "log level set to: info", "scope": "dapr.runtime", "type": "log", "time": "2021-01-12T16:11:39.467933+01:00", "ver": "1.0.0-rc.2"}
{"app_id": "ordering-api", "instance": "TSTSRV03", "level": "info", "msg": "metrics server started on :62408/", "scope": "dapr.metrics", "type": "log", "time": "2021-01-12T16:11:39.467933+01:00", "ver": "1.0.0-rc.2"}
```

<span data-ttu-id="ba24c-352">若要启用 JSON 格式设置，需要配置每个 Dapr 挎斗。</span><span class="sxs-lookup"><span data-stu-id="ba24c-352">To enable JSON formatting, you need to configure each Dapr sidecar.</span></span> <span data-ttu-id="ba24c-353">在自承载模式下，可以在 `--log-as-json` 命令行上指定标志：</span><span class="sxs-lookup"><span data-stu-id="ba24c-353">In self-hosted mode, you can specify the flag `--log-as-json` on the command line:</span></span>

```console
dapr run --app-id ordering-api --log-level info --log-as-json dotnet run
```

<span data-ttu-id="ba24c-354">在 Kubernetes 中，你可以 `dapr.io/log-as-json` 为应用程序的每个部署添加批注：</span><span class="sxs-lookup"><span data-stu-id="ba24c-354">In Kubernetes, you can add a `dapr.io/log-as-json` annotation to each deployment for the application:</span></span>

```yaml
annotations:
   dapr.io/enabled: "true"
   dapr.io/app-id: "ordering-api"
   dapr.io/app-port: "80"
   dapr.io/config: "dapr-config"
   dapr.io/log-as-json: "true"
```

<span data-ttu-id="ba24c-355">使用 Helm 在 Kubernetes 群集中安装 Dapr 时，可以为所有 Dapr 系统服务启用 JSON 格式的日志记录：</span><span class="sxs-lookup"><span data-stu-id="ba24c-355">When you install Dapr in a Kubernetes cluster using Helm, you can enable JSON formatted logging for all the Dapr system services:</span></span>

```console
helm repo add dapr https://dapr.github.io/helm-charts/
helm repo update
kubectl create namespace dapr-system
helm install dapr dapr/dapr --namespace dapr-system --set global.logAsJson=true
```

#### <a name="collect-logs"></a><span data-ttu-id="ba24c-356">收集日志</span><span class="sxs-lookup"><span data-stu-id="ba24c-356">Collect logs</span></span>

<span data-ttu-id="ba24c-357">Dapr 发出的日志可以送入监视后端进行分析。</span><span class="sxs-lookup"><span data-stu-id="ba24c-357">The logs emitted by Dapr can be fed into a monitoring back end for analysis.</span></span> <span data-ttu-id="ba24c-358">日志收集器是一个组件，用于从系统收集日志并将其发送到监视后端。</span><span class="sxs-lookup"><span data-stu-id="ba24c-358">A log collector is a component that collects logs from a system and sends them to a monitoring back end.</span></span> <span data-ttu-id="ba24c-359">常用日志收集器是 [Fluentd](https://www.fluentd.org/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-359">A popular log collector is [Fluentd](https://www.fluentd.org/).</span></span> <span data-ttu-id="ba24c-360">请参阅 Dapr 文档中的 [操作方法：设置 Fluentd、弹性搜索和 Kibana In Kubernetes](https://docs.dapr.io/operations/monitoring/logging/fluentd/) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-360">Check out the [How-To: Set up Fluentd, Elastic search and Kibana in Kubernetes](https://docs.dapr.io/operations/monitoring/logging/fluentd/) in the Dapr documentation.</span></span> <span data-ttu-id="ba24c-361">本文包含有关将 Fluentd 设置为日志收集器和 [ELK 堆栈](https://www.elastic.co/elastic-stack) (弹性搜索和 Kibana) 为监视后端的说明。</span><span class="sxs-lookup"><span data-stu-id="ba24c-361">This article contains instructions for setting up Fluentd as log collector and the [ELK Stack](https://www.elastic.co/elastic-stack) (Elastic Search and Kibana) as a monitoring back end.</span></span>

### <a name="health-status"></a><span data-ttu-id="ba24c-362">运行状况</span><span class="sxs-lookup"><span data-stu-id="ba24c-362">Health status</span></span>

<span data-ttu-id="ba24c-363">服务的运行状况提供对可用性的深入了解。</span><span class="sxs-lookup"><span data-stu-id="ba24c-363">The health status of a service provides insight into its availability.</span></span> <span data-ttu-id="ba24c-364">每个 Dapr 挎斗公开一个可供宿主环境用来确定挎斗的运行状况的运行状况 API。</span><span class="sxs-lookup"><span data-stu-id="ba24c-364">Each Dapr sidecar exposes a health API that can be used by the hosting environment to determine the health of the sidecar.</span></span> <span data-ttu-id="ba24c-365">该 API 有一个操作：</span><span class="sxs-lookup"><span data-stu-id="ba24c-365">The API has one operation:</span></span>

```console
GET http://localhost:3500/v1.0/healthz
```

<span data-ttu-id="ba24c-366">操作返回两个 HTTP 状态代码：</span><span class="sxs-lookup"><span data-stu-id="ba24c-366">The operation returns two HTTP status codes:</span></span>

- <span data-ttu-id="ba24c-367">204：挎斗正常运行时</span><span class="sxs-lookup"><span data-stu-id="ba24c-367">204: When the sidecar is healthy</span></span>
- <span data-ttu-id="ba24c-368">500：挎斗不正常时</span><span class="sxs-lookup"><span data-stu-id="ba24c-368">500: when the sidecar isn't healthy</span></span>

<span data-ttu-id="ba24c-369">在自承载模式下运行时，不会自动调用运行状况 API。</span><span class="sxs-lookup"><span data-stu-id="ba24c-369">When running in self-hosted mode, the health API isn't automatically invoked.</span></span> <span data-ttu-id="ba24c-370">可以通过应用程序代码或运行状况监视工具调用 API。</span><span class="sxs-lookup"><span data-stu-id="ba24c-370">You can invoke the API though from application code or a health monitoring tool.</span></span>

<span data-ttu-id="ba24c-371">在 Kubernetes 中运行时，Dapr 挎斗-注入器会自动将 Kubernetes 配置为使用运行状况 API 来执行 *活动探测* 和 *准备情况探测*。</span><span class="sxs-lookup"><span data-stu-id="ba24c-371">When running in Kubernetes, the Dapr sidecar-injector automatically configures Kubernetes to use the health API for executing *liveness probes* and *readiness probes*.</span></span>

<span data-ttu-id="ba24c-372">Kubernetes 使用活动探测来确定容器是否已启动并正在运行。</span><span class="sxs-lookup"><span data-stu-id="ba24c-372">Kubernetes uses liveness probes to determine whether a container is up and running.</span></span> <span data-ttu-id="ba24c-373">如果活动探测返回失败代码，Kubernetes 将假定容器处于死锁，并自动重启它。</span><span class="sxs-lookup"><span data-stu-id="ba24c-373">If a liveness probe returns a failure code, Kubernetes will assume the container is dead and automatically restart it.</span></span> <span data-ttu-id="ba24c-374">此功能提高了应用程序的总体可用性。</span><span class="sxs-lookup"><span data-stu-id="ba24c-374">This feature increases the overall availability of your application.</span></span>

<span data-ttu-id="ba24c-375">Kubernetes 使用就绪探测器来确定容器是否准备好开始接受流量。</span><span class="sxs-lookup"><span data-stu-id="ba24c-375">Kubernetes uses readiness probes to determine whether a container is ready to start accepting traffic.</span></span> <span data-ttu-id="ba24c-376">当容器的所有容器都准备就绪时，它被视为已就绪。</span><span class="sxs-lookup"><span data-stu-id="ba24c-376">A pod is considered ready when all of its containers are ready.</span></span> <span data-ttu-id="ba24c-377">准备情况确定 Kubernetes 服务是否可以将流量定向到负载平衡方案中的 pod。</span><span class="sxs-lookup"><span data-stu-id="ba24c-377">Readiness determines whether a Kubernetes service can direct traffic to a pod in a load-balancing scenario.</span></span> <span data-ttu-id="ba24c-378">未准备就绪的 pod 会自动从负载平衡器中删除。</span><span class="sxs-lookup"><span data-stu-id="ba24c-378">Pods that aren't ready are automatically removed from the load-balancer.</span></span>

<span data-ttu-id="ba24c-379">活动和准备情况探测具有多个可配置参数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-379">Liveness and readiness probes have several configurable parameters.</span></span> <span data-ttu-id="ba24c-380">这两者都在 pod 的清单文件的 "容器规范" 部分中进行配置。</span><span class="sxs-lookup"><span data-stu-id="ba24c-380">Both are configured in the container spec section of a pod's manifest file.</span></span> <span data-ttu-id="ba24c-381">默认情况下，Dapr 对每个挎斗容器使用以下配置：</span><span class="sxs-lookup"><span data-stu-id="ba24c-381">By default, Dapr uses the following configuration for each sidecar container:</span></span>

```yaml
livenessProbe:
      httpGet:
        path: v1.0/healthz
        port: 3500
      initialDelaySeconds: 5
      periodSeconds: 10
      timeoutSeconds : 5
      failureThreshold : 3
readinessProbe:
      httpGet:
        path: v1.0/healthz
        port: 3500
      initialDelaySeconds: 5
      periodSeconds: 10
      timeoutSeconds : 5
      failureThreshold: 3
```

 <span data-ttu-id="ba24c-382">以下参数可用于探测：</span><span class="sxs-lookup"><span data-stu-id="ba24c-382">The following parameters are available for the probes:</span></span>

- <span data-ttu-id="ba24c-383">`path`指定 Dapr HEALTH API 终结点。</span><span class="sxs-lookup"><span data-stu-id="ba24c-383">The `path` specifies the Dapr health API endpoint.</span></span>
- <span data-ttu-id="ba24c-384">`port`指定 Dapr HEALTH API 端口。</span><span class="sxs-lookup"><span data-stu-id="ba24c-384">The `port` specifies the Dapr health API port.</span></span>
- <span data-ttu-id="ba24c-385">`initialDelaySeconds`指定 Kubernetes 第一次开始探测容器前等待的秒数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-385">The `initialDelaySeconds`specifies the number of seconds Kubernetes will wait before it starts probing a container for the first time.</span></span>
- <span data-ttu-id="ba24c-386">`periodSeconds`指定 Kubernetes 将在每个探测之间等待的秒数。</span><span class="sxs-lookup"><span data-stu-id="ba24c-386">The `periodSeconds` specifies the number of seconds Kubernetes will wait between each probe.</span></span>
- <span data-ttu-id="ba24c-387">`timeoutSeconds`指定 Kubernetes 将在超时前等待 API 响应的秒数。超时被解释为失败。</span><span class="sxs-lookup"><span data-stu-id="ba24c-387">The `timeoutSeconds` specifies the number of seconds Kubernetes will wait on a response from the API before timing out. A timeout is interpreted as a failure.</span></span>
- <span data-ttu-id="ba24c-388">`failureThreshold`指定在考虑容器处于不活动状态或尚未准备就绪之前，Kubernetes 将接受的失败状态代码的数目。</span><span class="sxs-lookup"><span data-stu-id="ba24c-388">The `failureThreshold`specifies the number of failed status code Kubernetes will accept before considering the container not alive or not ready.</span></span>

### <a name="dapr-dashboard"></a><span data-ttu-id="ba24c-389">Dapr 仪表板</span><span class="sxs-lookup"><span data-stu-id="ba24c-389">Dapr dashboard</span></span>

<span data-ttu-id="ba24c-390">Dapr 提供了一个仪表板，用于显示有关 Dapr 应用程序、组件和配置的状态信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-390">Dapr offers a dashboard that presents status information on Dapr applications, components, and configurations.</span></span> <span data-ttu-id="ba24c-391">使用 Dapr CLI 将仪表板作为本地计算机上的 web 应用程序在端口8080上启动：</span><span class="sxs-lookup"><span data-stu-id="ba24c-391">Use the Dapr CLI to start the dashboard as a web-application on the local machine on port 8080:</span></span>

```console
dapr dashboard
```

<span data-ttu-id="ba24c-392">对于在 Kubernetes 中运行的 Dapr 应用程序，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="ba24c-392">For Dapr application running in Kubernetes, use the following command:</span></span>

```console
dapr dashboard -k
```

<span data-ttu-id="ba24c-393">此时将打开仪表板，其中概述了应用程序中具有 Dapr 挎斗的所有服务。</span><span class="sxs-lookup"><span data-stu-id="ba24c-393">The dashboard opens with an overview of all services in your application that have a Dapr sidecar.</span></span> <span data-ttu-id="ba24c-394">以下屏幕截图显示了 Kubernetes 中运行的 eShopOnDapr 应用程序的 Dapr 仪表板：</span><span class="sxs-lookup"><span data-stu-id="ba24c-394">The following screenshot shows the Dapr dashboard for the eShopOnDapr application running in Kubernetes:</span></span>

![Dapr 仪表板概述页](media/observability/dapr-dashboard-overview.png)

<span data-ttu-id="ba24c-396">在对 Dapr 应用程序进行故障排除时，Dapr 仪表板非常有用。</span><span class="sxs-lookup"><span data-stu-id="ba24c-396">The Dapr dashboard is invaluable when troubleshooting a Dapr application.</span></span> <span data-ttu-id="ba24c-397">其中提供了有关 Dapr 分支和系统服务的信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-397">It provides information about Dapr sidecars and system services.</span></span> <span data-ttu-id="ba24c-398">可以向下钻取每个服务的配置，包括日志记录条目。</span><span class="sxs-lookup"><span data-stu-id="ba24c-398">You can drill down into the configuration of each service, including the logging entries.</span></span>

<span data-ttu-id="ba24c-399">该仪表板还会显示应用程序的已配置组件 (及其配置) ：</span><span class="sxs-lookup"><span data-stu-id="ba24c-399">The dashboard also shows the configured components (and their configuration) for your application:</span></span>

![Dapr 仪表板组件页面](media/observability/dapr-dashboard-components.png)

<span data-ttu-id="ba24c-401">可以通过仪表板提供大量的信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-401">There's a large amount of information available through the dashboard.</span></span> <span data-ttu-id="ba24c-402">可以通过运行 Dapr 应用程序并浏览仪表板来发现它。</span><span class="sxs-lookup"><span data-stu-id="ba24c-402">You can discover it by running a Dapr application and browsing the dashboard.</span></span> <span data-ttu-id="ba24c-403">您可以使用附带的 eShopOnDapr 应用程序启动。</span><span class="sxs-lookup"><span data-stu-id="ba24c-403">You can use the accompanying eShopOnDapr application to start.</span></span>

<span data-ttu-id="ba24c-404">有关 Dapr 仪表板命令的详细信息，请参阅 Dapr 文档中的 [Dapr 仪表板 CLI 命令参考](https://docs.dapr.io/reference/cli/dapr-dashboard/) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-404">Check out the [Dapr dashboard CLI command reference](https://docs.dapr.io/reference/cli/dapr-dashboard/) in the Dapr docs for more information on the Dapr dashboard commands.</span></span>

## <a name="use-the-dapr-net-sdk"></a><span data-ttu-id="ba24c-405">使用 Dapr .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ba24c-405">Use the Dapr .NET SDK</span></span>

<span data-ttu-id="ba24c-406">Dapr .NET SDK 不包含任何特定的可观察性功能。</span><span class="sxs-lookup"><span data-stu-id="ba24c-406">The Dapr .NET SDK doesn't contain any specific observability features.</span></span> <span data-ttu-id="ba24c-407">所有可观察性功能都在 Dapr 级别提供。</span><span class="sxs-lookup"><span data-stu-id="ba24c-407">All observability features are offered at the Dapr level.</span></span>

<span data-ttu-id="ba24c-408">如果要从 .NET 应用程序代码发出遥测数据，则应考虑适用于 [.net 的 OPENTELEMETRY SDK](https://opentelemetry.io/docs/net/)。</span><span class="sxs-lookup"><span data-stu-id="ba24c-408">If you want to emit telemetry from your .NET application code, you should consider the [OpenTelemetry SDK for .NET](https://opentelemetry.io/docs/net/).</span></span> <span data-ttu-id="ba24c-409">开放式遥测项目是跨平台、开源和供应商不可知的项目。</span><span class="sxs-lookup"><span data-stu-id="ba24c-409">The Open Telemetry project is cross-platform, open source, and vendor agnostic.</span></span> <span data-ttu-id="ba24c-410">它提供端到端的实现，以生成、发出、收集、处理和导出遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-410">It provides an end-to-end implementation to generate, emit, collect, process, and export telemetry data.</span></span> <span data-ttu-id="ba24c-411">每种语言都有一个检测库，支持自动和手动检测。</span><span class="sxs-lookup"><span data-stu-id="ba24c-411">There's a single instrumentation library per language that supports automatic and manual instrumentation.</span></span> <span data-ttu-id="ba24c-412">遥测使用开放式遥测标准进行发布。</span><span class="sxs-lookup"><span data-stu-id="ba24c-412">Telemetry is published using the Open Telemetry standard.</span></span> <span data-ttu-id="ba24c-413">该项目具有广泛的行业支持，并采用云提供商、供应商和最终用户。</span><span class="sxs-lookup"><span data-stu-id="ba24c-413">The project has broad industry support and adoption from cloud providers, vendors, and end users.</span></span>

## <a name="reference-application-eshopondapr"></a><span data-ttu-id="ba24c-414">引用应用程序： eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="ba24c-414">Reference application: eShopOnDapr</span></span>

<span data-ttu-id="ba24c-415">随附的 eShopOnDapr 引用应用程序中的可观察性包含多个部分。</span><span class="sxs-lookup"><span data-stu-id="ba24c-415">Observability in accompanying eShopOnDapr reference application consists of several parts.</span></span> <span data-ttu-id="ba24c-416">捕获所有分支的遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-416">Telemetry from all of the sidecars is captured.</span></span> <span data-ttu-id="ba24c-417">此外，还有其他可观察性功能，这些功能继承自早期的 eShopOnContainers 示例。</span><span class="sxs-lookup"><span data-stu-id="ba24c-417">Additionally, there are other observability features inherited from the earlier eShopOnContainers sample.</span></span>

### <a name="custom-health-dashboard"></a><span data-ttu-id="ba24c-418">自定义运行状况仪表板</span><span class="sxs-lookup"><span data-stu-id="ba24c-418">Custom health dashboard</span></span>

<span data-ttu-id="ba24c-419">EShopOnDapr 中的 **WebStatus** 项目是一个自定义运行状况仪表板，可让你深入了解 eShop 服务的运行状况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-419">The **WebStatus** project in eShopOnDapr is a custom health dashboard that gives insight into the health of the eShop services.</span></span> <span data-ttu-id="ba24c-420">此仪表板不使用 Dapr health API，但使用 ASP.NET Core 的内置 [运行状况检查机制](/aspnet/core/host-and-deploy/health-checks) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-420">This dashboard doesn't use the Dapr health API but uses the built-in [health checks mechanism](/aspnet/core/host-and-deploy/health-checks) of ASP.NET Core.</span></span> <span data-ttu-id="ba24c-421">该仪表板不仅提供服务的运行状况状态，还提供服务依赖项的运行状况。</span><span class="sxs-lookup"><span data-stu-id="ba24c-421">The dashboard not only provides the health status of the services, but also the health of the dependencies of the services.</span></span> <span data-ttu-id="ba24c-422">例如，使用数据库的服务还提供此数据库的运行状况状态，如以下屏幕截图所示：</span><span class="sxs-lookup"><span data-stu-id="ba24c-422">For example, a service that uses a database also provides the health status of this database as shown in the following screenshot:</span></span>

![eShopOnDapr 自定义运行状况仪表板](media/observability/eshop-health-dashboard.png)

### <a name="seq-log-aggregator"></a><span data-ttu-id="ba24c-424">Seq 日志聚合器</span><span class="sxs-lookup"><span data-stu-id="ba24c-424">Seq log aggregator</span></span>

<span data-ttu-id="ba24c-425">[Seq](https://datalust.co/seq) 是在 eShopOnDapr 中用于聚合日志的常用日志聚合器服务器。</span><span class="sxs-lookup"><span data-stu-id="ba24c-425">[Seq](https://datalust.co/seq) is a popular log aggregator server that is used in eShopOnDapr to aggregate logs.</span></span> <span data-ttu-id="ba24c-426">从应用程序服务中 Seq 引入日志记录，而不是从 Dapr 系统服务或分支。</span><span class="sxs-lookup"><span data-stu-id="ba24c-426">Seq ingests logging from application services, but not from Dapr system services or sidecars.</span></span> <span data-ttu-id="ba24c-427">Seq 索引应用程序日志记录，提供一个 web 前端用于分析和查询日志。</span><span class="sxs-lookup"><span data-stu-id="ba24c-427">Seq indexes application logging and offers a web front end for analyzing and querying the logs.</span></span> <span data-ttu-id="ba24c-428">它还提供了用于构建监视仪表板的功能。</span><span class="sxs-lookup"><span data-stu-id="ba24c-428">It also offers functionality for building monitoring dashboards.</span></span>

<span data-ttu-id="ba24c-429">EShopOnDapr 应用程序服务使用 [SeriLog](https://serilog.net/) 日志记录库发出结构化日志记录。</span><span class="sxs-lookup"><span data-stu-id="ba24c-429">The eShopOnDapr application services emit structured logging using the [SeriLog](https://serilog.net/) logging library.</span></span> <span data-ttu-id="ba24c-430">Serilog 将日志事件发布到称为 **接收器** 的构造。</span><span class="sxs-lookup"><span data-stu-id="ba24c-430">Serilog publishes log events to a construct called a **sink**.</span></span> <span data-ttu-id="ba24c-431">接收器只是 Serilog 写入其日志记录事件的目标平台。</span><span class="sxs-lookup"><span data-stu-id="ba24c-431">A sink is simply a target platform to which Serilog writes its logging events.</span></span> <span data-ttu-id="ba24c-432">[许多 Serilog 接收器都可用](https://github.com/serilog/serilog/wiki/Provided-Sinks)，包括用于 Seq 的接收器。</span><span class="sxs-lookup"><span data-stu-id="ba24c-432">[Many Serilog sinks are available](https://github.com/serilog/serilog/wiki/Provided-Sinks), including one for Seq.</span></span> <span data-ttu-id="ba24c-433">Seq 是 eShopOnDapr 中使用的 Serilog 接收器。</span><span class="sxs-lookup"><span data-stu-id="ba24c-433">Seq is the Serilog sink used in eShopOnDapr.</span></span>

### <a name="application-insights"></a><span data-ttu-id="ba24c-434">Application Insights</span><span class="sxs-lookup"><span data-stu-id="ba24c-434">Application Insights</span></span>

<span data-ttu-id="ba24c-435">eShopOnDapr services 还使用适用于 .NET Core 的 Microsoft Application Insights SDK 将遥测直接发送到 Azure 应用程序 Insights。</span><span class="sxs-lookup"><span data-stu-id="ba24c-435">eShopOnDapr services also send telemetry directly to Azure Application Insights using the Microsoft Application Insights SDK for .NET Core.</span></span> <span data-ttu-id="ba24c-436">有关详细信息，请参阅 Microsoft 文档中的 [Azure 应用程序 Insights for ASP.NET Core 应用程序](/azure/azure-monitor/app/asp-net-core) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-436">For more information, see [Azure Application Insights for ASP.NET Core applications](/azure/azure-monitor/app/asp-net-core) in the Microsoft docs.</span></span>

## <a name="summary"></a><span data-ttu-id="ba24c-437">摘要</span><span class="sxs-lookup"><span data-stu-id="ba24c-437">Summary</span></span>

<span data-ttu-id="ba24c-438">在生产环境中运行分布式系统时，良好的可观察性是至关重要的。</span><span class="sxs-lookup"><span data-stu-id="ba24c-438">Good observability is crucial when running a distributed system in production.</span></span>

<span data-ttu-id="ba24c-439">Dapr 提供不同类型的遥测，包括分布式跟踪、日志记录、指标和运行状况状态。</span><span class="sxs-lookup"><span data-stu-id="ba24c-439">Dapr provides different types of telemetry, including distributed tracing, logging, metrics, and health status.</span></span>

<span data-ttu-id="ba24c-440">Dapr 仅生成 Dapr 系统服务和分支的遥测。</span><span class="sxs-lookup"><span data-stu-id="ba24c-440">Dapr only produces telemetry for the Dapr system services and sidecars.</span></span> <span data-ttu-id="ba24c-441">不会自动包含应用程序代码中的遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-441">Telemetry from your application code isn't automatically included.</span></span> <span data-ttu-id="ba24c-442">不过，可以使用特定的 SDK （如 OpenTelemetry SDK for .NET）从应用程序代码发出遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ba24c-442">You can however use a specific SDK like the OpenTelemetry SDK for .NET to emit telemetry from your application code.</span></span>

<span data-ttu-id="ba24c-443">Dapr 遥测采用基于开放标准的格式生成，因此可通过大量可用的监视工具来引入。</span><span class="sxs-lookup"><span data-stu-id="ba24c-443">Dapr telemetry is produced in an open-standards based format so it can be ingested by a large set of available monitoring tools.</span></span> <span data-ttu-id="ba24c-444">一些示例包括： Zipkin、Azure 应用程序 Insights、ELK Stack、New Relic 和 Grafana。</span><span class="sxs-lookup"><span data-stu-id="ba24c-444">Some examples are: Zipkin, Azure Application Insights, the ELK Stack, New Relic, and Grafana.</span></span> <span data-ttu-id="ba24c-445">有关如何监视具有特定监视后端的 Dapr 应用程序的教程，请参阅 Dapr 文档中的 [监视应用程序 Dapr](https://docs.dapr.io/operations/monitoring/) 。</span><span class="sxs-lookup"><span data-stu-id="ba24c-445">See [Monitor your application with Dapr](https://docs.dapr.io/operations/monitoring/) in the Dapr documentation for tutorials on how to monitor your Dapr applications with specific monitoring back ends.</span></span>

<span data-ttu-id="ba24c-446">需要引入遥测的遥测 scraper，并将其发布到监视后端。</span><span class="sxs-lookup"><span data-stu-id="ba24c-446">You'll need a telemetry scraper that ingests telemetry and publishes it to the monitoring back end.</span></span>

<span data-ttu-id="ba24c-447">可以将 Dapr 配置为发出结构化日志记录。</span><span class="sxs-lookup"><span data-stu-id="ba24c-447">Dapr can be configured to emit structured logging.</span></span> <span data-ttu-id="ba24c-448">建议使用结构化日志记录，因为后端监视工具可以对其进行索引。</span><span class="sxs-lookup"><span data-stu-id="ba24c-448">Structured logging is favored as it can be indexed by back-end monitoring tools.</span></span> <span data-ttu-id="ba24c-449">索引日志记录使用户能够在搜索日志记录时执行丰富的查询。</span><span class="sxs-lookup"><span data-stu-id="ba24c-449">Indexed logging enables users to execute rich queries when searching through the logging.</span></span>

<span data-ttu-id="ba24c-450">Dapr 提供了一个仪表板，其中提供了有关 Dapr 服务和配置的信息。</span><span class="sxs-lookup"><span data-stu-id="ba24c-450">Dapr offers a dashboard that presents information about the Dapr services and configuration.</span></span>

## <a name="references"></a><span data-ttu-id="ba24c-451">参考</span><span class="sxs-lookup"><span data-stu-id="ba24c-451">References</span></span>

- [<span data-ttu-id="ba24c-452">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="ba24c-452">Azure Application Insights</span></span>](/azure/azure-monitor/app/app-insights-overview/)
- [<span data-ttu-id="ba24c-453">打开遥测</span><span class="sxs-lookup"><span data-stu-id="ba24c-453">Open Telemetry</span></span>](https://opentelemetry.io/)
- [<span data-ttu-id="ba24c-454">Zipkin</span><span class="sxs-lookup"><span data-stu-id="ba24c-454">Zipkin</span></span>](https://zipkin.io/)
- [<span data-ttu-id="ba24c-455">W3C 跟踪上下文</span><span class="sxs-lookup"><span data-stu-id="ba24c-455">W3C Trace Context</span></span>](https://www.w3.org/TR/trace-context/)
- [<span data-ttu-id="ba24c-456">Jaeger</span><span class="sxs-lookup"><span data-stu-id="ba24c-456">Jaeger</span></span>](https://www.jaegertracing.io/)
- [<span data-ttu-id="ba24c-457">New Relic</span><span class="sxs-lookup"><span data-stu-id="ba24c-457">New Relic</span></span>](https://newrelic.com/)
- [<span data-ttu-id="ba24c-458">Prometheus</span><span class="sxs-lookup"><span data-stu-id="ba24c-458">Prometheus</span></span>](https://prometheus.io/)
- [<span data-ttu-id="ba24c-459">Grafana</span><span class="sxs-lookup"><span data-stu-id="ba24c-459">Grafana</span></span>](https://grafana.com/grafana/)
- [<span data-ttu-id="ba24c-460">打开遥测 SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="ba24c-460">Open Telemetry SDK for .NET</span></span>](https://opentelemetry.io/docs/net/)
- [<span data-ttu-id="ba24c-461">Fluentd</span><span class="sxs-lookup"><span data-stu-id="ba24c-461">Fluentd</span></span>](https://www.fluentd.org/)
- [<span data-ttu-id="ba24c-462">ELK 堆栈</span><span class="sxs-lookup"><span data-stu-id="ba24c-462">ELK stack</span></span>](https://www.elastic.co/elastic-stack)
- [<span data-ttu-id="ba24c-463">序列</span><span class="sxs-lookup"><span data-stu-id="ba24c-463">Seq</span></span>](https://datalust.co/seq)
- [<span data-ttu-id="ba24c-464">Serilog</span><span class="sxs-lookup"><span data-stu-id="ba24c-464">Serilog</span></span>](https://serilog.net/)

> [!div class="step-by-step"]
> <span data-ttu-id="ba24c-465">[上一页](bindings.md)
> [下一页](secrets.md)</span><span class="sxs-lookup"><span data-stu-id="ba24c-465">[Previous](bindings.md)
[Next](secrets.md)</span></span>
