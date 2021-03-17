---
title: 世界 20,000 英尺范围内
description: 分布式应用程序的优点和挑战，同时探讨了单一和 SOA 方法。
author: robvet
ms.date: 02/17/2021
ms.openlocfilehash: df182e81bcf95a84fc283dbb945d20f2218b0d0b
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623936"
---
# <a name="the-world-is-distributed"></a><span data-ttu-id="09da1-103">世界 20,000 英尺范围内</span><span class="sxs-lookup"><span data-stu-id="09da1-103">The world is distributed</span></span>

<span data-ttu-id="09da1-104">只需问 "酷的孩子"： *现代、分布式系统正在进行，而单一应用程序已外出！*</span><span class="sxs-lookup"><span data-stu-id="09da1-104">Just ask any 'cool kid': *Modern, distributed systems are in, and monolithic apps are out!*</span></span>

<span data-ttu-id="09da1-105">但这并不是 "超酷儿童"。</span><span class="sxs-lookup"><span data-stu-id="09da1-105">But, it's not just "cool kids."</span></span> <span data-ttu-id="09da1-106">企业架构师、企业架构师和敏锐开发人员在探索和评估新式分布式应用程序时，会回显这些想法。</span><span class="sxs-lookup"><span data-stu-id="09da1-106">Progressive IT Leaders, corporate architects, and astute developers are echoing these same thoughts as they explore and evaluate modern distributed applications.</span></span> <span data-ttu-id="09da1-107">许多人都购买了。</span><span class="sxs-lookup"><span data-stu-id="09da1-107">Many have bought in.</span></span> <span data-ttu-id="09da1-108">他们正在按照分布式微服务应用程序的原理、模式和做法设计新的和 replatforming 的现有企业应用程序。</span><span class="sxs-lookup"><span data-stu-id="09da1-108">They're designing new and replatforming existing enterprise applications following the principles, patterns, and practices of distributed microservice applications.</span></span>

<span data-ttu-id="09da1-109">但这种发展引发了许多问题 .。。</span><span class="sxs-lookup"><span data-stu-id="09da1-109">But, this evolution raises many questions...</span></span>

- <span data-ttu-id="09da1-110">什么是分布式应用程序？</span><span class="sxs-lookup"><span data-stu-id="09da1-110">What exactly is a distributed application?</span></span>
- <span data-ttu-id="09da1-111">为什么他们会获得欢迎？</span><span class="sxs-lookup"><span data-stu-id="09da1-111">Why are they gaining popularity?</span></span>
- <span data-ttu-id="09da1-112">成本是多少？</span><span class="sxs-lookup"><span data-stu-id="09da1-112">What are the costs?</span></span>
- <span data-ttu-id="09da1-113">重要的是，这是什么折衷因素？</span><span class="sxs-lookup"><span data-stu-id="09da1-113">And, importantly, what are the tradeoffs?</span></span>

<span data-ttu-id="09da1-114">首先，让我们倒带，看看过去15年。</span><span class="sxs-lookup"><span data-stu-id="09da1-114">To start, let's rewind and look at the past 15 years.</span></span> <span data-ttu-id="09da1-115">在此期间，我们通常将应用程序构建为单一单一的单位。</span><span class="sxs-lookup"><span data-stu-id="09da1-115">During this period, we typically constructed applications as a single, monolithic unit.</span></span> <span data-ttu-id="09da1-116">图1-1 显示了此体系结构。</span><span class="sxs-lookup"><span data-stu-id="09da1-116">Figure 1-1 shows the architecture.</span></span>

![整体体系结构。](./media/the-world-is-distributed/monolithic-design.png)

<span data-ttu-id="09da1-118">**图 1-1**。</span><span class="sxs-lookup"><span data-stu-id="09da1-118">**Figure 1-1**.</span></span> <span data-ttu-id="09da1-119">整体体系结构。</span><span class="sxs-lookup"><span data-stu-id="09da1-119">Monolithic architecture.</span></span>

<span data-ttu-id="09da1-120">请注意，用于排序、标识和营销的模块在单服务器进程中的执行方式。</span><span class="sxs-lookup"><span data-stu-id="09da1-120">Note how the modules for Ordering, Identity, and Marketing execute in a single-server process.</span></span> <span data-ttu-id="09da1-121">应用程序数据存储在共享数据库中。</span><span class="sxs-lookup"><span data-stu-id="09da1-121">Application data is stored in a shared database.</span></span> <span data-ttu-id="09da1-122">业务功能通过 HTML 和 RESTful 接口公开。</span><span class="sxs-lookup"><span data-stu-id="09da1-122">Business functionality is exposed via HTML and RESTful interfaces.</span></span>

<span data-ttu-id="09da1-123">在许多方面，单一应用程序是 `straightforward` 。</span><span class="sxs-lookup"><span data-stu-id="09da1-123">In many ways, monolithic apps are `straightforward`.</span></span> <span data-ttu-id="09da1-124">它们非常简单：</span><span class="sxs-lookup"><span data-stu-id="09da1-124">They're straightforward to:</span></span>

- <span data-ttu-id="09da1-125">构建</span><span class="sxs-lookup"><span data-stu-id="09da1-125">Build</span></span>
- <span data-ttu-id="09da1-126">测试</span><span class="sxs-lookup"><span data-stu-id="09da1-126">Test</span></span>
- <span data-ttu-id="09da1-127">部署</span><span class="sxs-lookup"><span data-stu-id="09da1-127">Deploy</span></span>
- <span data-ttu-id="09da1-128">疑难解答</span><span class="sxs-lookup"><span data-stu-id="09da1-128">Troubleshoot</span></span>
- <span data-ttu-id="09da1-129">垂直缩放 (向上扩展) </span><span class="sxs-lookup"><span data-stu-id="09da1-129">Scale vertically (scale up)</span></span>

<span data-ttu-id="09da1-130">但单一体系结构可能会带来重大挑战。</span><span class="sxs-lookup"><span data-stu-id="09da1-130">However, monolithic architectures can present significant challenges.</span></span>

<span data-ttu-id="09da1-131">随着时间的推移，你可能会看到一个开始失去控制的点 .。。</span><span class="sxs-lookup"><span data-stu-id="09da1-131">Over time, you may reach a point where you begin to lose control...</span></span>

- <span data-ttu-id="09da1-132">单体架构已变得非常复杂，因此不会有任何人理解它。</span><span class="sxs-lookup"><span data-stu-id="09da1-132">The monolith has become so overwhelmingly complicated that no single person understands it.</span></span>
- <span data-ttu-id="09da1-133">您担心每个更改都会带来意想不到且昂贵的副作用。</span><span class="sxs-lookup"><span data-stu-id="09da1-133">You fear making changes as each brings unintended and costly side effects.</span></span>
- <span data-ttu-id="09da1-134">新功能/修补程序的实现非常耗时且成本高昂。</span><span class="sxs-lookup"><span data-stu-id="09da1-134">New features/fixes become time-consuming and expensive to implement.</span></span>
- <span data-ttu-id="09da1-135">即使是最小的更改，也需要完全部署整个应用程序-成本高昂且风险巨大。</span><span class="sxs-lookup"><span data-stu-id="09da1-135">Even the smallest change requires full deployment of the entire application - expensive and risky.</span></span>
- <span data-ttu-id="09da1-136">一个不稳定的组件可能会损坏整个系统。</span><span class="sxs-lookup"><span data-stu-id="09da1-136">One unstable component can crash the entire system.</span></span>
- <span data-ttu-id="09da1-137">添加新的技术和框架并不是一个选项。</span><span class="sxs-lookup"><span data-stu-id="09da1-137">Adding new technologies and frameworks aren't an option.</span></span>
- <span data-ttu-id="09da1-138">实现敏捷交付方法非常困难。</span><span class="sxs-lookup"><span data-stu-id="09da1-138">Implementing agile delivery methodologies are difficult.</span></span>
- <span data-ttu-id="09da1-139">体系结构 erosion 在中将设置为代码库 deteriorates，其中包含永不结束的 "特殊情况"。</span><span class="sxs-lookup"><span data-stu-id="09da1-139">Architectural erosion sets in as the code base deteriorates with never-ending "special cases."</span></span>
- <span data-ttu-id="09da1-140">最终，顾问会提供，并告诉你重写它。</span><span class="sxs-lookup"><span data-stu-id="09da1-140">Eventually the consultants come in and tell you to rewrite it.</span></span>

<span data-ttu-id="09da1-141">IT 专业人员调用此条件 `the Fear Cycle` 。</span><span class="sxs-lookup"><span data-stu-id="09da1-141">IT practitioners call this condition `the Fear Cycle`.</span></span> <span data-ttu-id="09da1-142">如果你已在技术业务中花费了很长时间，则很可能会遇到这种情况。</span><span class="sxs-lookup"><span data-stu-id="09da1-142">If you've been in the technology business for any length of time, good chance you've experienced it.</span></span> <span data-ttu-id="09da1-143">它很紧张，用完您的 IT 预算。</span><span class="sxs-lookup"><span data-stu-id="09da1-143">It's stressful and exhausts your IT budget.</span></span> <span data-ttu-id="09da1-144">大多数预算都用来维护旧应用，而不是构建新的创新性解决方案。</span><span class="sxs-lookup"><span data-stu-id="09da1-144">Instead of building new and innovative solutions, most of your budget is spent maintaining legacy apps.</span></span>

<span data-ttu-id="09da1-145">企业需要，而不是担心 `speed and agility` 。</span><span class="sxs-lookup"><span data-stu-id="09da1-145">Instead of fear, businesses require `speed and agility`.</span></span> <span data-ttu-id="09da1-146">他们会寻找一种体系结构样式，使其能够快速响应市场状况。</span><span class="sxs-lookup"><span data-stu-id="09da1-146">They seek an architectural style with which they can rapidly respond to market conditions.</span></span> <span data-ttu-id="09da1-147">他们需要即时更新和单独缩放实时应用程序的小区域。</span><span class="sxs-lookup"><span data-stu-id="09da1-147">They need to instantaneously update and individually scale small areas of a live application.</span></span>

<span data-ttu-id="09da1-148">在 [面向服务的体系结构](https://en.wikipedia.org/wiki/Service-oriented_architecture)（或）的基础上，最早尝试获得速度和灵活性 `SOA` 。</span><span class="sxs-lookup"><span data-stu-id="09da1-148">An early attempt to gain speed and agility came in the form of [Service Oriented Architecture](https://en.wikipedia.org/wiki/Service-oriented_architecture), or `SOA`.</span></span> <span data-ttu-id="09da1-149">在此模型中，服务使用者和服务提供商通过中间件消息传送组件进行协作，通常称为 [企业服务总线](https://en.wikipedia.org/wiki/Enterprise_service_bus)，或 `ESB` 。</span><span class="sxs-lookup"><span data-stu-id="09da1-149">In this model, service consumers and service providers collaborated via middleware messaging components, often referred to as an [Enterprise Service Bus](https://en.wikipedia.org/wiki/Enterprise_service_bus), or `ESB`.</span></span> <span data-ttu-id="09da1-150">图1-2 显示了此体系结构。</span><span class="sxs-lookup"><span data-stu-id="09da1-150">Figure 1-2 shows the architecture.</span></span>

![SOA.](./media/the-world-is-distributed/soa-basic.png)

<span data-ttu-id="09da1-152">**图 1-2**。</span><span class="sxs-lookup"><span data-stu-id="09da1-152">**Figure 1-2**.</span></span> <span data-ttu-id="09da1-153">SOA 体系结构。</span><span class="sxs-lookup"><span data-stu-id="09da1-153">SOA architecture.</span></span>

<span data-ttu-id="09da1-154">对于 SOA，已向 ESB 注册的集中式服务提供商。</span><span class="sxs-lookup"><span data-stu-id="09da1-154">With SOA, centralized service providers registered with the ESB.</span></span> <span data-ttu-id="09da1-155">将业务逻辑内置于 ESB 中以集成提供者和使用者。</span><span class="sxs-lookup"><span data-stu-id="09da1-155">Business logic would be built into the ESB to integrate providers and consumers.</span></span> <span data-ttu-id="09da1-156">然后，服务使用者可以使用 ESB 查找这些提供程序并与其通信。</span><span class="sxs-lookup"><span data-stu-id="09da1-156">Service consumers could then find and communicate with these providers using the ESB.</span></span>

<span data-ttu-id="09da1-157">尽管有 SOA 的承诺，但实现此方法通常会增加复杂性并引入瓶颈。</span><span class="sxs-lookup"><span data-stu-id="09da1-157">Despite the promises of SOA, implementing this approach often increased complexity and introduced bottlenecks.</span></span> <span data-ttu-id="09da1-158">维护成本变得很高，ESB 中间件成本高昂。</span><span class="sxs-lookup"><span data-stu-id="09da1-158">Maintenance costs became high and ESB middleware expensive.</span></span> <span data-ttu-id="09da1-159">服务倾向大。</span><span class="sxs-lookup"><span data-stu-id="09da1-159">Services tended to be large.</span></span> <span data-ttu-id="09da1-160">它们通常是共享的依赖项和数据存储。</span><span class="sxs-lookup"><span data-stu-id="09da1-160">They often shared dependencies and data storage.</span></span> <span data-ttu-id="09da1-161">最终，Soa 通常会产生一个带有集中服务的 "分布式单一结构" 结构，这些服务可经受更改。</span><span class="sxs-lookup"><span data-stu-id="09da1-161">In the end, SOAs often resulted in a 'distributed monolithic' structure with centralized services that were resistant to change.</span></span>

<span data-ttu-id="09da1-162">如今，许多组织通过采用分布式微服务体系结构方法生成系统来实现速度和灵活性。</span><span class="sxs-lookup"><span data-stu-id="09da1-162">Nowadays, many organizations have realized speed and agility by adopting a distributed microservice architectural approach to building systems.</span></span> <span data-ttu-id="09da1-163">图1-3 显示了使用分布式技术和实践构建的同一系统。</span><span class="sxs-lookup"><span data-stu-id="09da1-163">Figure 1-3 shows the same system built using distributed techniques and practices.</span></span>

![分布式体系结构。](./media/the-world-is-distributed/distributed-design.png)

<span data-ttu-id="09da1-165">**图 1-3**。</span><span class="sxs-lookup"><span data-stu-id="09da1-165">**Figure 1-3**.</span></span> <span data-ttu-id="09da1-166">分布式体系结构。</span><span class="sxs-lookup"><span data-stu-id="09da1-166">Distributed architecture.</span></span>

<span data-ttu-id="09da1-167">请注意如何在一组分布式服务中分解同一应用程序。</span><span class="sxs-lookup"><span data-stu-id="09da1-167">Note how the same application is decomposed across a set of distributed services.</span></span> <span data-ttu-id="09da1-168">每个都是自包含的，并封装自己的代码、数据和依赖项。</span><span class="sxs-lookup"><span data-stu-id="09da1-168">Each is self-contained and encapsulates its own code, data, and dependencies.</span></span> <span data-ttu-id="09da1-169">每个部署在软件容器中并由容器 orchestrator 管理。</span><span class="sxs-lookup"><span data-stu-id="09da1-169">Each is deployed in a software container and managed by a container orchestrator.</span></span> <span data-ttu-id="09da1-170">每个服务都拥有一个专用数据库，而不是由多个服务共享的单个数据库。</span><span class="sxs-lookup"><span data-stu-id="09da1-170">Instead of a single database shared by multiple services, each service owns a private database.</span></span> <span data-ttu-id="09da1-171">其他服务不能直接访问此数据库，只能获取通过拥有它的服务的公共 API 公开的数据。</span><span class="sxs-lookup"><span data-stu-id="09da1-171">Other services can't access this database directly and can only get to data that is exposed through the public API of the service that owns it.</span></span> <span data-ttu-id="09da1-172">请注意，某些服务需要完整的关系数据库，而另一些则是 NoSQL 数据存储。</span><span class="sxs-lookup"><span data-stu-id="09da1-172">Note how some services require a full relational database, but others, a NoSQL datastore.</span></span> <span data-ttu-id="09da1-173">购物篮服务将其状态存储在分布式键/值缓存中。</span><span class="sxs-lookup"><span data-stu-id="09da1-173">The basket service stores its state in a distributed key/value cache.</span></span> <span data-ttu-id="09da1-174">请注意入站流量如何通过 API 网关服务路由。</span><span class="sxs-lookup"><span data-stu-id="09da1-174">Note how inbound traffic routes through an API Gateway service.</span></span> <span data-ttu-id="09da1-175">它负责将调用定向到服务并强制执行交叉切削问题。</span><span class="sxs-lookup"><span data-stu-id="09da1-175">It's responsible for directing calls to  services and enforcing cross-cutting concerns.</span></span> <span data-ttu-id="09da1-176">最重要的是，应用程序充分利用了新式云平台中的可伸缩性、可用性和复原功能。</span><span class="sxs-lookup"><span data-stu-id="09da1-176">Most importantly, the application takes full advantage of the scalability, availability, and resiliency features found in modern cloud platforms.</span></span>

<span data-ttu-id="09da1-177">但尽管分布式服务可提供灵活性和速度，但它们却提供了不同的挑战。</span><span class="sxs-lookup"><span data-stu-id="09da1-177">But, while distributed services can provide agility and speed, they present a different set of challenges.</span></span> <span data-ttu-id="09da1-178">请考虑以下事项 .。。</span><span class="sxs-lookup"><span data-stu-id="09da1-178">Consider the following...</span></span>

- <span data-ttu-id="09da1-179">分布式服务如何相互发现并进行同步通信？</span><span class="sxs-lookup"><span data-stu-id="09da1-179">How can distributed services discover each other and communicate synchronously?</span></span>
- <span data-ttu-id="09da1-180">如何实现异步消息传送？</span><span class="sxs-lookup"><span data-stu-id="09da1-180">How can they implement asynchronous messaging?</span></span>
- <span data-ttu-id="09da1-181">如何在事务中维护上下文信息？</span><span class="sxs-lookup"><span data-stu-id="09da1-181">How can they maintain contextual information across a transaction?</span></span>
- <span data-ttu-id="09da1-182">如何才能灵活应对故障？</span><span class="sxs-lookup"><span data-stu-id="09da1-182">How can they become resilient to failure?</span></span>
- <span data-ttu-id="09da1-183">如何缩放以满足变动需求？</span><span class="sxs-lookup"><span data-stu-id="09da1-183">How can they scale to meet fluctuating demand?</span></span>
- <span data-ttu-id="09da1-184">如何监视和观察它们？</span><span class="sxs-lookup"><span data-stu-id="09da1-184">How are they monitored and observed?</span></span>

<span data-ttu-id="09da1-185">对于上述每种挑战，都经常提供多种产品。</span><span class="sxs-lookup"><span data-stu-id="09da1-185">For each of these challenges, multiple products are often available.</span></span> <span data-ttu-id="09da1-186">但会降低应用程序的产品差异，并使代码的可维护性和便携式成为一项挑战。</span><span class="sxs-lookup"><span data-stu-id="09da1-186">But, shielding your application from product differences and keeping code maintainable and portable become a challenge.</span></span>

<span data-ttu-id="09da1-187">本书介绍了 Dapr。</span><span class="sxs-lookup"><span data-stu-id="09da1-187">This book introduces Dapr.</span></span> <span data-ttu-id="09da1-188">Dapr 是分布式应用程序运行时。</span><span class="sxs-lookup"><span data-stu-id="09da1-188">Dapr is a distributed application runtime.</span></span> <span data-ttu-id="09da1-189">它直接解决了分布式应用程序附带的许多挑战。</span><span class="sxs-lookup"><span data-stu-id="09da1-189">It directly addresses many of the challenges found that come along with distributed applications.</span></span> <span data-ttu-id="09da1-190">Dapr，它可能会对分布式应用程序开发产生深远的影响。</span><span class="sxs-lookup"><span data-stu-id="09da1-190">Looking ahead, Dapr has the potential to have a profound impact on distributed application development.</span></span>

## <a name="summary"></a><span data-ttu-id="09da1-191">摘要</span><span class="sxs-lookup"><span data-stu-id="09da1-191">Summary</span></span>

<span data-ttu-id="09da1-192">在本章中，我们讨论了分布式应用程序的采用。</span><span class="sxs-lookup"><span data-stu-id="09da1-192">In this chapter, we discussed the adoption of distributed applications.</span></span> <span data-ttu-id="09da1-193">我们与分布式服务的一种单一系统方法相对比。</span><span class="sxs-lookup"><span data-stu-id="09da1-193">We contrasted a monolithic system approach with that of distributed services.</span></span> <span data-ttu-id="09da1-194">考虑分布式方法时，我们指出了许多常见的难题。</span><span class="sxs-lookup"><span data-stu-id="09da1-194">We pointed out many of the common challenges when considering a distributed approach.</span></span>

<span data-ttu-id="09da1-195">现在，让我们 Dapr，让我们向你介绍新的。</span><span class="sxs-lookup"><span data-stu-id="09da1-195">Now, sit back, relax, and let us introduce you the new world of Dapr.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="09da1-196">[上一页](foreword.md)
>[下一页](dapr-at-20000-feet.md)</span><span class="sxs-lookup"><span data-stu-id="09da1-196">[Previous](foreword.md)
[Next](dapr-at-20000-feet.md)</span></span>
