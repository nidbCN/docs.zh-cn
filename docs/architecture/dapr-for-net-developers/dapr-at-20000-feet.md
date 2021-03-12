---
title: 均分布有 Dapr
description: Dapr 是什么、它的作用以及它的工作原理的简要概述。
author: robvet
ms.date: 02/07/2021
ms.openlocfilehash: f0efb4652aaa35961d59979cb561941e5280a575
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604744"
---
# <a name="dapr-at-20000-feet"></a><span data-ttu-id="58b0a-103">均分布有 Dapr</span><span class="sxs-lookup"><span data-stu-id="58b0a-103">Dapr at 20,000 feet</span></span>

<span data-ttu-id="58b0a-104">第1章介绍了分布式微服务应用程序的吸引力。</span><span class="sxs-lookup"><span data-stu-id="58b0a-104">In chapter 1, we discussed the appeal of distributed microservice applications.</span></span> <span data-ttu-id="58b0a-105">但我们也指出，它们大幅增加了体系结构和操作的复杂性。</span><span class="sxs-lookup"><span data-stu-id="58b0a-105">But, we also pointed out that they dramatically increase architectural and operational complexity.</span></span> <span data-ttu-id="58b0a-106">考虑到这一点，问题变成了，您如何 "让您的蛋糕更好？"。</span><span class="sxs-lookup"><span data-stu-id="58b0a-106">With that in mind, the question becomes, how can you "have your cake" and "eat it too?".</span></span> <span data-ttu-id="58b0a-107">也就是说，如何充分利用分布式体系结构的灵活性，并将其复杂性降到最低？</span><span class="sxs-lookup"><span data-stu-id="58b0a-107">That is, how can you take advantage of the agility of distributed architecture, and minimize its complexity?</span></span>

<span data-ttu-id="58b0a-108">Dapr 或 *分布式应用程序运行时* 是一种用于构建新式分布式应用程序的新方法。</span><span class="sxs-lookup"><span data-stu-id="58b0a-108">Dapr, or *Distributed Application Runtime*, is a new way to build modern distributed applications.</span></span>

<span data-ttu-id="58b0a-109">作为原型开始的内容发展为一个高度成功的开源项目。</span><span class="sxs-lookup"><span data-stu-id="58b0a-109">What started as a prototype has evolved into a highly successful open-source project.</span></span> <span data-ttu-id="58b0a-110">Microsoft 的赞助商与客户和开源社区密切合作，设计和生成 Dapr。</span><span class="sxs-lookup"><span data-stu-id="58b0a-110">Its sponsor, Microsoft, has closely partnered with customers and the open-source community to design and build Dapr.</span></span> <span data-ttu-id="58b0a-111">Dapr 项目将所有背景的开发人员组合在一起，以解决开发分布式应用程序的一些棘手挑战。</span><span class="sxs-lookup"><span data-stu-id="58b0a-111">The Dapr project brings together developers from all backgrounds to solve some of the toughest challenges of developing distributed applications.</span></span>

<span data-ttu-id="58b0a-112">本书从 .NET 开发人员的角度探讨了 Dapr。</span><span class="sxs-lookup"><span data-stu-id="58b0a-112">This book looks at Dapr from the viewpoint of a .NET developer.</span></span> <span data-ttu-id="58b0a-113">在本章中，你将构建概念上了解 Dapr 及其工作原理。</span><span class="sxs-lookup"><span data-stu-id="58b0a-113">In this chapter, you'll build a conceptual understanding of Dapr and how it works.</span></span> <span data-ttu-id="58b0a-114">稍后，我们将提供有关如何在应用程序中使用 Dapr 的实际实践说明。</span><span class="sxs-lookup"><span data-stu-id="58b0a-114">Later on, we present practical, hands-on instruction on how you can use Dapr in your applications.</span></span>

<span data-ttu-id="58b0a-115">以20000英尺的速度想象飞行。</span><span class="sxs-lookup"><span data-stu-id="58b0a-115">Imagine flying in a jet at 20,000 feet.</span></span> <span data-ttu-id="58b0a-116">查看窗口，从大角度看看下面的布局。</span><span class="sxs-lookup"><span data-stu-id="58b0a-116">You look out the window and see the landscape below from a wide perspective.</span></span> <span data-ttu-id="58b0a-117">让我们对 Dapr 执行相同的操作。</span><span class="sxs-lookup"><span data-stu-id="58b0a-117">Let's do the same for Dapr.</span></span> <span data-ttu-id="58b0a-118">在20000英尺内通过 Dapr 实现自己的飞行。</span><span class="sxs-lookup"><span data-stu-id="58b0a-118">Visualize yourself flying over Dapr at 20,000 feet.</span></span> <span data-ttu-id="58b0a-119">你会看到什么？</span><span class="sxs-lookup"><span data-stu-id="58b0a-119">What would you see?</span></span>

## <a name="dapr-and-the-problem-it-solves"></a><span data-ttu-id="58b0a-120">Dapr 以及它解决的问题</span><span class="sxs-lookup"><span data-stu-id="58b0a-120">Dapr and the problem it solves</span></span>

<span data-ttu-id="58b0a-121">Dapr 解决了新式分布式应用程序固有的巨大挑战： **复杂性**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-121">Dapr addresses a large challenge inherent in modern distributed applications: **Complexity**.</span></span>

<span data-ttu-id="58b0a-122">通过可插入组件的体系结构，Dapr 大大简化了分布式应用程序背后的管道。</span><span class="sxs-lookup"><span data-stu-id="58b0a-122">Through an architecture of pluggable components, Dapr greatly simplifies the plumbing behind distributed applications.</span></span> <span data-ttu-id="58b0a-123">它提供一个使用 Dapr 运行时中的基础结构功能来绑定应用程序的 **动态胶水** 。</span><span class="sxs-lookup"><span data-stu-id="58b0a-123">It provides a **dynamic glue** that binds your application with infrastructure capabilities from the Dapr runtime.</span></span> <span data-ttu-id="58b0a-124">例如，你的应用程序可能需要状态存储。</span><span class="sxs-lookup"><span data-stu-id="58b0a-124">For example, your application may require a state store.</span></span> <span data-ttu-id="58b0a-125">可以编写自定义代码来面向 Redis 缓存，并在运行时将其插入服务中。</span><span class="sxs-lookup"><span data-stu-id="58b0a-125">You could write custom code to target Redis Cache and inject it into your service at runtime.</span></span> <span data-ttu-id="58b0a-126">不过，Dapr 提供现成的分布式缓存功能，从而简化了你的体验。</span><span class="sxs-lookup"><span data-stu-id="58b0a-126">However, Dapr simplifies your experience by providing a distributed cache capability out-of-the-box.</span></span> <span data-ttu-id="58b0a-127">服务调用通过 Dapr **配置** 动态绑定到 Redis 缓存 **组件** 的 Dapr **生成块**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-127">Your service invokes a Dapr **building block** that dynamically binds to Redis Cache **component** via a Dapr **configuration**.</span></span> <span data-ttu-id="58b0a-128">在此模型中，服务将对 Dapr 的调用委托，后者代表您调用 Redis。</span><span class="sxs-lookup"><span data-stu-id="58b0a-128">With this model, your service delegates the call to Dapr, which calls Redis on your behalf.</span></span> <span data-ttu-id="58b0a-129">你的服务没有 SDK、库或对 Redis 的直接引用。</span><span class="sxs-lookup"><span data-stu-id="58b0a-129">Your service has no SDK, library, or direct reference to Redis.</span></span> <span data-ttu-id="58b0a-130">针对常见的 Dapr 状态管理 API （而不是 Redis 缓存 API）进行编码。</span><span class="sxs-lookup"><span data-stu-id="58b0a-130">You code against the common Dapr state management API, not the Redis Cache API.</span></span>

<span data-ttu-id="58b0a-131">图2-1 显示了20000英尺的 Dapr。</span><span class="sxs-lookup"><span data-stu-id="58b0a-131">Figure 2-1 shows Dapr from 20,000 feet.</span></span>

<span data-ttu-id="58b0a-132">![Dapr 20000 ](./media/dapr-at-20000-feet/dapr-high-level.png)
 **2-1** 英尺。</span><span class="sxs-lookup"><span data-stu-id="58b0a-132">![Dapr at 20,000 feet](./media/dapr-at-20000-feet/dapr-high-level.png)
**Figure 2-1**.</span></span> <span data-ttu-id="58b0a-133">Dapr 20000 英尺。</span><span class="sxs-lookup"><span data-stu-id="58b0a-133">Dapr at 20,000 feet.</span></span>

<span data-ttu-id="58b0a-134">请注意，在该图的第一行中，Dapr 如何为常见开发平台提供特定于语言的 Sdk。</span><span class="sxs-lookup"><span data-stu-id="58b0a-134">In the top row of the figure, note how Dapr provides language-specific SDKs for popular development platforms.</span></span> <span data-ttu-id="58b0a-135">Dapr 1.0 包括支持中转、Node.js、Python、.NET、Java 和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="58b0a-135">Dapr v 1.0 includes supports Go, Node.js, Python, .NET, Java, and JavaScript.</span></span> <span data-ttu-id="58b0a-136">本书重点介绍 Dapr .NET SDK，该 SDK 还提供对 ASP.NET Core 集成的直接支持。</span><span class="sxs-lookup"><span data-stu-id="58b0a-136">This book focuses on the Dapr .NET SDK, which also provides direct support for ASP.NET Core integration.</span></span>

<span data-ttu-id="58b0a-137">尽管语言特定的 Sdk 增强了开发人员体验，但 Dapr 不限平台。</span><span class="sxs-lookup"><span data-stu-id="58b0a-137">While language-specific SDKs enhance the developer experience, Dapr is platform agnostic.</span></span> <span data-ttu-id="58b0a-138">在后台，Dapr 的编程模型通过标准 HTTP/gRPC 通信协议公开功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-138">Under the hood, Dapr's programming model exposes capabilities through standard HTTP/gRPC communication protocols.</span></span> <span data-ttu-id="58b0a-139">任何编程平台都可以通过其本机 HTTP 和 gRPC Api 调用 Dapr。</span><span class="sxs-lookup"><span data-stu-id="58b0a-139">Any programming platform can call Dapr via its native HTTP and gRPC APIs.</span></span>  

<span data-ttu-id="58b0a-140">图中心的蓝色框表示 Dapr 构建基块。</span><span class="sxs-lookup"><span data-stu-id="58b0a-140">The blue boxes across the center of the figure represent the Dapr building blocks.</span></span> <span data-ttu-id="58b0a-141">每个都公开您的应用程序可以使用的分布式应用程序功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-141">Each exposes a distributed application capability that your application can consume.</span></span>

<span data-ttu-id="58b0a-142">下一行重点介绍了 Dapr 的可移植性，以及它可在其上运行的不同环境。</span><span class="sxs-lookup"><span data-stu-id="58b0a-142">The bottom row highlights the portability of Dapr and the diverse environments across which it can run.</span></span>

## <a name="dapr-architecture"></a><span data-ttu-id="58b0a-143">Dapr 体系结构</span><span class="sxs-lookup"><span data-stu-id="58b0a-143">Dapr architecture</span></span>

<span data-ttu-id="58b0a-144">此时，jet 会在 Dapr 中翻过来并向下飞行，使您更深入地了解 Dapr 的工作方式。</span><span class="sxs-lookup"><span data-stu-id="58b0a-144">At this point, the jet turns around and flies back over Dapr, descending in altitude, giving you a closer look at how Dapr works.</span></span>

### <a name="building-blocks"></a><span data-ttu-id="58b0a-145">构建基块</span><span class="sxs-lookup"><span data-stu-id="58b0a-145">Building blocks</span></span>

<span data-ttu-id="58b0a-146">从新的角度来看，您可以看到 Dapr **构建基块** 的更详细视图。</span><span class="sxs-lookup"><span data-stu-id="58b0a-146">From the new perspective, you see a more detailed view of the Dapr **building blocks**.</span></span>

<span data-ttu-id="58b0a-147">构建基块封装分布式基础结构功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-147">A building block encapsulates a distributed infrastructure capability.</span></span> <span data-ttu-id="58b0a-148">可以通过 HTTP 或 gRPC Api 访问该功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-148">You can access the functionality through the HTTP or gRPC APIs.</span></span> <span data-ttu-id="58b0a-149">图2-2 显示了 1.0 Dapr 的可用块。</span><span class="sxs-lookup"><span data-stu-id="58b0a-149">Figure 2-2 shows the available blocks for Dapr v 1.0.</span></span>

![Dapr 构建基块](./media/dapr-at-20000-feet/building-blocks.png)

<span data-ttu-id="58b0a-151">**图 2-2**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-151">**Figure 2-2**.</span></span> <span data-ttu-id="58b0a-152">Dapr 构建基块。</span><span class="sxs-lookup"><span data-stu-id="58b0a-152">Dapr building blocks.</span></span>

<span data-ttu-id="58b0a-153">下表描述了每个块提供的基础结构服务。</span><span class="sxs-lookup"><span data-stu-id="58b0a-153">The following table describes the infrastructure services provided by each block.</span></span>

| <span data-ttu-id="58b0a-154">构建基块</span><span class="sxs-lookup"><span data-stu-id="58b0a-154">Building block</span></span> | <span data-ttu-id="58b0a-155">说明</span><span class="sxs-lookup"><span data-stu-id="58b0a-155">Description</span></span> |
|----------------|-------------|
| [<span data-ttu-id="58b0a-156">状态管理</span><span class="sxs-lookup"><span data-stu-id="58b0a-156">State management</span></span>](state-management.md) | <span data-ttu-id="58b0a-157">支持长时间运行有状态服务的上下文信息。</span><span class="sxs-lookup"><span data-stu-id="58b0a-157">Support contextual information for long running stateful services.</span></span> |
| [<span data-ttu-id="58b0a-158">服务调用</span><span class="sxs-lookup"><span data-stu-id="58b0a-158">Service invocation</span></span>](service-invocation.md) | <span data-ttu-id="58b0a-159">使用平台不可知的协议和众所周知的终结点调用直接、安全的服务到服务调用。</span><span class="sxs-lookup"><span data-stu-id="58b0a-159">Invoke direct, secure service-to-service calls using platform agnostic protocols and well-known endpoints.</span></span> |
| [<span data-ttu-id="58b0a-160">发布和订阅</span><span class="sxs-lookup"><span data-stu-id="58b0a-160">Publish and subscribe</span></span>](publish-subscribe.md) | <span data-ttu-id="58b0a-161">在服务之间实现安全的可缩放发布/订阅消息传送。</span><span class="sxs-lookup"><span data-stu-id="58b0a-161">Implement secure, scalable pub/sub messaging between services.</span></span> |
| [<span data-ttu-id="58b0a-162">绑定</span><span class="sxs-lookup"><span data-stu-id="58b0a-162">Bindings</span></span>](bindings.md) | <span data-ttu-id="58b0a-163">通过与双向通信的外部资源引发的事件触发代码。</span><span class="sxs-lookup"><span data-stu-id="58b0a-163">Trigger code from events raised by external resources with bi-directional communication.</span></span> |
| [<span data-ttu-id="58b0a-164">可观察性</span><span class="sxs-lookup"><span data-stu-id="58b0a-164">Observability</span></span>](observability.md) | <span data-ttu-id="58b0a-165">监视和度量跨网络服务的消息调用。</span><span class="sxs-lookup"><span data-stu-id="58b0a-165">Monitor and measure message calls across networked services.</span></span> |
| [<span data-ttu-id="58b0a-166">机密</span><span class="sxs-lookup"><span data-stu-id="58b0a-166">Secrets</span></span>](secrets.md) | <span data-ttu-id="58b0a-167">安全访问外部密钥存储。</span><span class="sxs-lookup"><span data-stu-id="58b0a-167">Securely access external secret stores.</span></span> |
| <span data-ttu-id="58b0a-168">执行组件</span><span class="sxs-lookup"><span data-stu-id="58b0a-168">Actors</span></span> | <span data-ttu-id="58b0a-169">在可重用的执行组件对象中封装逻辑和数据。</span><span class="sxs-lookup"><span data-stu-id="58b0a-169">Encapsulate logic and data in reusable actor objects.</span></span> |

<span data-ttu-id="58b0a-170">构建基块从服务中抽象出分布式应用程序功能的实现。</span><span class="sxs-lookup"><span data-stu-id="58b0a-170">Building blocks abstract the implementation of distributed application capabilities from your services.</span></span> <span data-ttu-id="58b0a-171">图2-3 显示了这种交互。</span><span class="sxs-lookup"><span data-stu-id="58b0a-171">Figure 2-3 shows this interaction.</span></span>

![Dapr 构建基块集成](./media/dapr-at-20000-feet/building-blocks-integration.png)

<span data-ttu-id="58b0a-173">**图 2-3**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-173">**Figure 2-3**.</span></span> <span data-ttu-id="58b0a-174">Dapr 构建基块集成。</span><span class="sxs-lookup"><span data-stu-id="58b0a-174">Dapr building block integration.</span></span>

<span data-ttu-id="58b0a-175">构建基块调用为每个资源提供具体实现的 Dapr 组件。</span><span class="sxs-lookup"><span data-stu-id="58b0a-175">Building blocks invoke Dapr components that provide the concrete implementation for each resource.</span></span> <span data-ttu-id="58b0a-176">服务的代码仅知道构建基块。</span><span class="sxs-lookup"><span data-stu-id="58b0a-176">The code for your service is only aware of the building block.</span></span> <span data-ttu-id="58b0a-177">它不会依赖于外部 Sdk 或库，Dapr 会为你处理管道。</span><span class="sxs-lookup"><span data-stu-id="58b0a-177">It takes no dependencies on external SDKs or libraries - Dapr handles the plumbing for you.</span></span> <span data-ttu-id="58b0a-178">每个构建基块都是独立的。</span><span class="sxs-lookup"><span data-stu-id="58b0a-178">Each building block is independent.</span></span> <span data-ttu-id="58b0a-179">你可以在应用程序中使用一个、部分或全部。</span><span class="sxs-lookup"><span data-stu-id="58b0a-179">You can use one, some, or all of them in your application.</span></span> <span data-ttu-id="58b0a-180">作为增值，Dapr 构建块制作为行业最佳实践，包括综合性可观察性。</span><span class="sxs-lookup"><span data-stu-id="58b0a-180">As a value-add, Dapr building blocks bake in industry best practices including comprehensive observability.</span></span>

<span data-ttu-id="58b0a-181">我们在后面的章节中提供每个 Dapr 构建基块的详细说明和代码示例。</span><span class="sxs-lookup"><span data-stu-id="58b0a-181">We provide detailed explanation and code samples for each Dapr building block in the upcoming chapters.</span></span> <span data-ttu-id="58b0a-182">此时，jet 更进一步。</span><span class="sxs-lookup"><span data-stu-id="58b0a-182">At this point, the jet descends even more.</span></span> <span data-ttu-id="58b0a-183">从新的角度来看，你现在可以更深入地了解 Dapr 组件层。</span><span class="sxs-lookup"><span data-stu-id="58b0a-183">From the new perspective, you now have a closer look at the Dapr components layer.</span></span>

### <a name="components"></a><span data-ttu-id="58b0a-184">组件</span><span class="sxs-lookup"><span data-stu-id="58b0a-184">Components</span></span>

<span data-ttu-id="58b0a-185">尽管构建块公开了一个 API 来调用分布式应用程序功能，但 Dapr 组件提供了具体的实现以使其发生。</span><span class="sxs-lookup"><span data-stu-id="58b0a-185">While building blocks expose an API to invoke distributed application capabilities, Dapr components provide the concrete implementation to make it happen.</span></span>

<span data-ttu-id="58b0a-186">请考虑 Dapr **状态存储** 组件。</span><span class="sxs-lookup"><span data-stu-id="58b0a-186">Consider, the Dapr **state store** component.</span></span> <span data-ttu-id="58b0a-187">它提供一种统一的方法来管理 CRUD 操作的状态。</span><span class="sxs-lookup"><span data-stu-id="58b0a-187">It provides a uniform way to manage state for CRUD operations.</span></span> <span data-ttu-id="58b0a-188">如果你的服务代码没有任何更改，你可以在以下任何 Dapr 状态组件之间切换：</span><span class="sxs-lookup"><span data-stu-id="58b0a-188">Without any change to your service code, you could switch between any of the following Dapr state components:</span></span>

- <span data-ttu-id="58b0a-189">AWS DynamoDB</span><span class="sxs-lookup"><span data-stu-id="58b0a-189">AWS DynamoDB</span></span>
- <span data-ttu-id="58b0a-190">Aerospike</span><span class="sxs-lookup"><span data-stu-id="58b0a-190">Aerospike</span></span>
- <span data-ttu-id="58b0a-191">Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="58b0a-191">Azure Blob Storage</span></span>
- <span data-ttu-id="58b0a-192">Azure CosmosDB</span><span class="sxs-lookup"><span data-stu-id="58b0a-192">Azure CosmosDB</span></span>
- <span data-ttu-id="58b0a-193">Azure 表存储</span><span class="sxs-lookup"><span data-stu-id="58b0a-193">Azure Table Storage</span></span>
- <span data-ttu-id="58b0a-194">Cassandra</span><span class="sxs-lookup"><span data-stu-id="58b0a-194">Cassandra</span></span>
- <span data-ttu-id="58b0a-195">Cloud Firestore (数据存储模式) </span><span class="sxs-lookup"><span data-stu-id="58b0a-195">Cloud Firestore (Datastore mode)</span></span>
- <span data-ttu-id="58b0a-196">CloudState</span><span class="sxs-lookup"><span data-stu-id="58b0a-196">CloudState</span></span>
- <span data-ttu-id="58b0a-197">Couchbase</span><span class="sxs-lookup"><span data-stu-id="58b0a-197">Couchbase</span></span>
- <span data-ttu-id="58b0a-198">Etcd</span><span class="sxs-lookup"><span data-stu-id="58b0a-198">Etcd</span></span>
- <span data-ttu-id="58b0a-199">HashiCorp Consul</span><span class="sxs-lookup"><span data-stu-id="58b0a-199">HashiCorp Consul</span></span>
- <span data-ttu-id="58b0a-200">Hazelcast</span><span class="sxs-lookup"><span data-stu-id="58b0a-200">Hazelcast</span></span>
- <span data-ttu-id="58b0a-201">Memcached</span><span class="sxs-lookup"><span data-stu-id="58b0a-201">Memcached</span></span>
- <span data-ttu-id="58b0a-202">MongoDB</span><span class="sxs-lookup"><span data-stu-id="58b0a-202">MongoDB</span></span>
- <span data-ttu-id="58b0a-203">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="58b0a-203">PostgreSQL</span></span>
- <span data-ttu-id="58b0a-204">Redis</span><span class="sxs-lookup"><span data-stu-id="58b0a-204">Redis</span></span>
- <span data-ttu-id="58b0a-205">RethinkDB</span><span class="sxs-lookup"><span data-stu-id="58b0a-205">RethinkDB</span></span>
- <span data-ttu-id="58b0a-206">SQL Server</span><span class="sxs-lookup"><span data-stu-id="58b0a-206">SQL Server</span></span>
- <span data-ttu-id="58b0a-207">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="58b0a-207">Zookeeper</span></span>

<span data-ttu-id="58b0a-208">每个组件都通过通用状态管理接口提供必要的实现：</span><span class="sxs-lookup"><span data-stu-id="58b0a-208">Each component provides the necessary implementation through a common state management interface:</span></span>

```go
 type Store interface {
   Init(metadata Metadata) error
   Delete(req *DeleteRequest) error
   BulkDelete(req []DeleteRequest) error
   Get(req *GetRequest) (*GetResponse, error)
   Set(req *SetRequest) error
   BulkSet(req []SetRequest) error
}
```

> [!TIP]
> <span data-ttu-id="58b0a-209">Dapr 运行时以及所有 Dapr 组件都已使用 Golang 或中转语言编写。</span><span class="sxs-lookup"><span data-stu-id="58b0a-209">The Dapr runtime as well as all of the Dapr components have been written in the Golang, or Go, language.</span></span> <span data-ttu-id="58b0a-210">转向在开源社区中使用一种流行的语言，并证实 Dapr 的跨平台承诺。</span><span class="sxs-lookup"><span data-stu-id="58b0a-210">Go is a popular language across the open source community and attests to cross-platform commitment of Dapr.</span></span>

<span data-ttu-id="58b0a-211">也许你开始将 Azure Redis 缓存用作状态存储。</span><span class="sxs-lookup"><span data-stu-id="58b0a-211">Perhaps you start with Azure Redis Cache as your state store.</span></span> <span data-ttu-id="58b0a-212">用以下配置指定它：</span><span class="sxs-lookup"><span data-stu-id="58b0a-212">You specify it with the following configuration:</span></span>

 ```yaml
 apiVersion: dapr.io/v1alpha1
 kind: Component
 metadata:
   name: statestore
   namespace: default
 spec:
   type: state.redis
   version: v1
   metadata:
   - name: redisHost
     value: <HOST>
   - name: redisPassword
     value: <PASSWORD>
   - name: enableTLS
     value: <bool> # Optional. Allowed: true, false.
   - name: failover
     value: <bool> # Optional. Allowed: true, false.
 ```

<span data-ttu-id="58b0a-213">在 " **规范** " 部分中，将 Dapr 配置为使用 Redis 缓存进行状态管理。</span><span class="sxs-lookup"><span data-stu-id="58b0a-213">In the **spec** section, you configure Dapr to use the Redis Cache for state management.</span></span> <span data-ttu-id="58b0a-214">节还包含组件特定的元数据。</span><span class="sxs-lookup"><span data-stu-id="58b0a-214">The section also contains component-specific metadata.</span></span> <span data-ttu-id="58b0a-215">在这种情况下，可以使用它来配置其他 Redis 设置。</span><span class="sxs-lookup"><span data-stu-id="58b0a-215">In this case, you can use it to configure additional Redis settings.</span></span>

<span data-ttu-id="58b0a-216">稍后，应用程序就可以进入生产环境了。</span><span class="sxs-lookup"><span data-stu-id="58b0a-216">At a later time, the application is ready to go to production.</span></span> <span data-ttu-id="58b0a-217">对于生产环境，可能需要将状态管理更改为 Azure 表存储。</span><span class="sxs-lookup"><span data-stu-id="58b0a-217">For the production environment, you may want to change your state management to Azure Table Storage.</span></span> <span data-ttu-id="58b0a-218">Azure 表存储提供经济实惠且持久的状态管理功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-218">Azure Table Storage provides state management capabilities that are affordable and highly durable.</span></span>

<span data-ttu-id="58b0a-219">撰写本文时，Dapr 提供以下组件类型：</span><span class="sxs-lookup"><span data-stu-id="58b0a-219">At the time of this writing, the following component types are provided by Dapr:</span></span>

| <span data-ttu-id="58b0a-220">组件</span><span class="sxs-lookup"><span data-stu-id="58b0a-220">Component</span></span> | <span data-ttu-id="58b0a-221">说明</span><span class="sxs-lookup"><span data-stu-id="58b0a-221">Description</span></span> |
|-----------|-------------|
| [<span data-ttu-id="58b0a-222">服务发现</span><span class="sxs-lookup"><span data-stu-id="58b0a-222">Service discovery</span></span>](https://github.com/dapr/components-contrib/tree/master/nameresolution) | <span data-ttu-id="58b0a-223">由服务调用构建基块用于与宿主环境集成以提供服务到服务发现。</span><span class="sxs-lookup"><span data-stu-id="58b0a-223">Used by the service invocation building block to integrate with the hosting environment to provide service-to-service discovery.</span></span> |
| [<span data-ttu-id="58b0a-224">State</span><span class="sxs-lookup"><span data-stu-id="58b0a-224">State</span></span>](https://github.com/dapr/components-contrib/tree/master/state) | <span data-ttu-id="58b0a-225">提供统一的接口以与各种状态存储实现交互。</span><span class="sxs-lookup"><span data-stu-id="58b0a-225">Provides a uniform interface to interact with a wide variety of state store implementations.</span></span> |
| [<span data-ttu-id="58b0a-226">发布/订阅</span><span class="sxs-lookup"><span data-stu-id="58b0a-226">Pub/sub</span></span>](https://github.com/dapr/components-contrib/tree/master/pubsub) | <span data-ttu-id="58b0a-227">提供统一的接口以与各种消息总线实现交互。</span><span class="sxs-lookup"><span data-stu-id="58b0a-227">Provides a uniform interface to interact with a wide variety of message bus implementations.</span></span> |
| [<span data-ttu-id="58b0a-228">绑定</span><span class="sxs-lookup"><span data-stu-id="58b0a-228">Bindings</span></span>](https://github.com/dapr/components-contrib/tree/master/bindings) | <span data-ttu-id="58b0a-229">提供了一个统一的接口，用于从外部系统触发应用程序事件并调用具有可选数据负载的外部系统。</span><span class="sxs-lookup"><span data-stu-id="58b0a-229">Provides a uniform interface to trigger application events from external systems and invoke external systems with optional data payloads.</span></span> |
| [<span data-ttu-id="58b0a-230">中间件</span><span class="sxs-lookup"><span data-stu-id="58b0a-230">Middleware</span></span>](https://github.com/dapr/components-contrib/tree/master/middleware) | <span data-ttu-id="58b0a-231">允许自定义中间件插入请求处理管道，并调用请求或响应的其他操作。</span><span class="sxs-lookup"><span data-stu-id="58b0a-231">Allows custom middleware to plug into the request processing pipeline and invoke additional actions on a request or response.</span></span> |
| [<span data-ttu-id="58b0a-232">密钥存储</span><span class="sxs-lookup"><span data-stu-id="58b0a-232">Secret stores</span></span>](https://github.com/dapr/components-contrib/tree/master/secretstores) | <span data-ttu-id="58b0a-233">提供统一的接口以与外部机密存储区交互，包括云、边缘、商业和开源服务。</span><span class="sxs-lookup"><span data-stu-id="58b0a-233">Provides a uniform interface to interact with external secret stores, including cloud, edge, commercial, open-source services.</span></span> |

<span data-ttu-id="58b0a-234">当 jet 在 Dapr 上完成其工作时，您再次看一遍，可以看到它如何连接在一起。</span><span class="sxs-lookup"><span data-stu-id="58b0a-234">As the jet completes its fly over of Dapr, you look back once more and can see how it connects together.</span></span>

### <a name="sidecar-architecture"></a><span data-ttu-id="58b0a-235">挎斗体系结构</span><span class="sxs-lookup"><span data-stu-id="58b0a-235">Sidecar architecture</span></span>

<span data-ttu-id="58b0a-236">Dapr 通过 [挎斗体系结构](/azure/architecture/patterns/sidecar)公开其构建基块和组件。</span><span class="sxs-lookup"><span data-stu-id="58b0a-236">Dapr exposes its building blocks and components through a [sidecar architecture](/azure/architecture/patterns/sidecar).</span></span> <span data-ttu-id="58b0a-237">挎斗使 Dapr 能够在单独的内存进程中运行，或者在单独的容器中运行。</span><span class="sxs-lookup"><span data-stu-id="58b0a-237">A sidecar enables Dapr to run in a separate memory process or separate container alongside your service.</span></span> <span data-ttu-id="58b0a-238">分支提供隔离和封装，因为它们不是服务的一部分，而是连接到它。</span><span class="sxs-lookup"><span data-stu-id="58b0a-238">Sidecars provide isolation and encapsulation as they aren't part of the service, but connected to it.</span></span> <span data-ttu-id="58b0a-239">这种隔离使每个都具有其自己的运行时环境，并在不同的编程平台上构建。</span><span class="sxs-lookup"><span data-stu-id="58b0a-239">This separation enables each to have its own runtime environment and be built upon different programming platforms.</span></span> <span data-ttu-id="58b0a-240">图2-4 显示了挎斗模式。</span><span class="sxs-lookup"><span data-stu-id="58b0a-240">Figure 2-4 shows a sidecar pattern.</span></span>

![挎斗体系结构](./media/dapr-at-20000-feet/sidecar-generic.png)

<span data-ttu-id="58b0a-242">**图 2-4**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-242">**Figure 2-4**.</span></span> <span data-ttu-id="58b0a-243">挎斗体系结构。</span><span class="sxs-lookup"><span data-stu-id="58b0a-243">Sidecar architecture.</span></span>

<span data-ttu-id="58b0a-244">此模式之所以称作“挎斗”(Sidecar)，是因为它类似于三轮摩托车上的挎斗。</span><span class="sxs-lookup"><span data-stu-id="58b0a-244">This pattern is named Sidecar because it resembles a sidecar attached to a motorcycle.</span></span> <span data-ttu-id="58b0a-245">在上图中，请注意 Dapr 挎斗如何附加到服务以提供分布式应用程序功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-245">In the previous figure, note how the Dapr sidecar is attached to your service to provide distributed application capabilities.</span></span>

### <a name="hosting-environments"></a><span data-ttu-id="58b0a-246">宿主环境</span><span class="sxs-lookup"><span data-stu-id="58b0a-246">Hosting environments</span></span>

<span data-ttu-id="58b0a-247">Dapr 提供跨平台支持，可以在许多不同的环境中运行。</span><span class="sxs-lookup"><span data-stu-id="58b0a-247">Dapr has cross-platform support and can run in many different environments.</span></span> <span data-ttu-id="58b0a-248">这些环境包括 Kubernetes、一组 Vm 或边缘环境，如 Azure IoT Edge。</span><span class="sxs-lookup"><span data-stu-id="58b0a-248">These environments include Kubernetes, a group of VMs, or edge environments such as Azure IoT Edge.</span></span>

<span data-ttu-id="58b0a-249">对于本地开发，开始的最简单方法是采用 [自承载模式](https://docs.dapr.io/concepts/overview/#self-hosted)。</span><span class="sxs-lookup"><span data-stu-id="58b0a-249">For local development, the easiest way to get started is with [self-hosted mode](https://docs.dapr.io/concepts/overview/#self-hosted).</span></span> <span data-ttu-id="58b0a-250">在自承载模式下，微服务和 Dapr 分支在无容器 orchestrator （如 Kubernetes）的独立本地进程中运行。</span><span class="sxs-lookup"><span data-stu-id="58b0a-250">In self-hosted mode, the microservices and Dapr sidecars run in separate local processes without a container orchestrator such as Kubernetes.</span></span> <span data-ttu-id="58b0a-251">有关详细信息，请参阅 [下载并安装 DAPR CLI](https://docs.dapr.io/getting-started/install-dapr/)。</span><span class="sxs-lookup"><span data-stu-id="58b0a-251">For more information, see [download and install the Dapr CLI](https://docs.dapr.io/getting-started/install-dapr/).</span></span>

<span data-ttu-id="58b0a-252">图2-5 显示了一个应用程序和 Dapr，它托管在两个独立的内存进程中，通过 HTTP 或 gRPC 进行通信。</span><span class="sxs-lookup"><span data-stu-id="58b0a-252">Figure 2-5 shows an application and Dapr hosted in two separate memory processes communicating via HTTP or gRPC.</span></span>

![自承载挎斗体系结构](./media/dapr-at-20000-feet/self-hosted-dapr-sidecar.png)

<span data-ttu-id="58b0a-254">**图 2-5**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-254">**Figure 2-5**.</span></span> <span data-ttu-id="58b0a-255">自承载的 Dapr 挎斗。</span><span class="sxs-lookup"><span data-stu-id="58b0a-255">Self-hosted Dapr sidecar.</span></span>

<span data-ttu-id="58b0a-256">默认情况下，Dapr 安装用于 Redis 和 Zipkin 的 Docker 容器，提供默认状态管理和可观察性。</span><span class="sxs-lookup"><span data-stu-id="58b0a-256">By default, Dapr installs Docker containers for Redis and Zipkin to provide default state management and observability.</span></span> <span data-ttu-id="58b0a-257">如果不想在本地计算机上安装 Docker，甚至可以 [在没有任何 Docker 容器的自承载模式下运行 Dapr](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/)。</span><span class="sxs-lookup"><span data-stu-id="58b0a-257">If you don't want to install Docker on your local machine, you can even [run Dapr in self-hosted mode without any Docker containers](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/).</span></span> <span data-ttu-id="58b0a-258">但是，必须手动安装 Redis 的默认组件，例如状态管理和发布/订阅。</span><span class="sxs-lookup"><span data-stu-id="58b0a-258">However, you must install default components such as Redis for state management and pub/sub manually.</span></span>

<span data-ttu-id="58b0a-259">Dapr 也在 [容器化环境](https://docs.dapr.io/concepts/overview/#kubernetes-hosted)（如 Kubernetes）中运行。</span><span class="sxs-lookup"><span data-stu-id="58b0a-259">Dapr also runs in [containerized environments](https://docs.dapr.io/concepts/overview/#kubernetes-hosted), such as Kubernetes.</span></span> <span data-ttu-id="58b0a-260">图2-6 显示了在单独的侧面车载容器中运行的 Dapr，以及同一 Kubernetes pod 中的应用程序容器。</span><span class="sxs-lookup"><span data-stu-id="58b0a-260">Figure 2-6 shows Dapr running in a separate side-car container along with the application container in the same Kubernetes pod.</span></span>

![Kubernetes 托管的挎斗体系结构](./media/dapr-at-20000-feet/kubernetes-hosted-dapr-sidecar.png)

<span data-ttu-id="58b0a-262">**图 2-6**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-262">**Figure 2-6**.</span></span> <span data-ttu-id="58b0a-263">Kubernetes 托管的 Dapr 挎斗。</span><span class="sxs-lookup"><span data-stu-id="58b0a-263">Kubernetes-hosted Dapr sidecar.</span></span>

## <a name="dapr-performance-considerations"></a><span data-ttu-id="58b0a-264">Dapr 性能注意事项</span><span class="sxs-lookup"><span data-stu-id="58b0a-264">Dapr performance considerations</span></span>

<span data-ttu-id="58b0a-265">如您所见，Dapr 公开了挎斗体系结构，以便将应用程序与分布式应用程序功能分离。</span><span class="sxs-lookup"><span data-stu-id="58b0a-265">As you've seen, Dapr exposes a sidecar architecture to decouple your application from distributed application capabilities.</span></span> <span data-ttu-id="58b0a-266">调用 Dapr 操作需要至少一个进程外网络调用。</span><span class="sxs-lookup"><span data-stu-id="58b0a-266">Invoking a Dapr operation requires at least one out-of-process network call.</span></span> <span data-ttu-id="58b0a-267">图2-7 显示了 Dapr 流量模式的示例。</span><span class="sxs-lookup"><span data-stu-id="58b0a-267">Figure 2-7 presents an example of a Dapr traffic pattern.</span></span>

![Dapr 流量模式](./media/dapr-at-20000-feet/dapr-traffic-patterns.png)

<span data-ttu-id="58b0a-269">**图 2-7**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-269">**Figure 2-7**.</span></span> <span data-ttu-id="58b0a-270">Dapr 流量模式。</span><span class="sxs-lookup"><span data-stu-id="58b0a-270">Dapr traffic patterns.</span></span>

<span data-ttu-id="58b0a-271">查看上一个图，可能会询问每次调用产生的延迟和开销。</span><span class="sxs-lookup"><span data-stu-id="58b0a-271">Looking at the previous figure, one might question the latency and overhead incurred for each call.</span></span>  

<span data-ttu-id="58b0a-272">Dapr 团队投入了很高的性能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-272">The Dapr team has invested heavily in performance.</span></span> <span data-ttu-id="58b0a-273">大量的工程努力使 Dapr 的效率更高。</span><span class="sxs-lookup"><span data-stu-id="58b0a-273">A tremendous amount of engineering effort has gone into making Dapr efficient.</span></span> <span data-ttu-id="58b0a-274">Dapr 分支之间的调用始终与 gRPC 一起建立，后者提供高性能和小型二进制负载。</span><span class="sxs-lookup"><span data-stu-id="58b0a-274">Calls between Dapr sidecars are always made with gRPC, which delivers high performance and small binary payloads.</span></span> <span data-ttu-id="58b0a-275">在大多数情况下，额外的开销应为毫秒。</span><span class="sxs-lookup"><span data-stu-id="58b0a-275">In most cases, the additional overhead should be sub-millisecond.</span></span>

<span data-ttu-id="58b0a-276">若要提高性能，开发人员可通过 gRPC 调用 Dapr 构建基块。</span><span class="sxs-lookup"><span data-stu-id="58b0a-276">To increase performance, developers can call the Dapr building blocks with gRPC.</span></span>

<span data-ttu-id="58b0a-277">gRPC 是一种新式的高性能框架， (RPC) 协议演变旧的 [远程过程调用 ](https://en.wikipedia.org/wiki/Remote_procedure_call) 。</span><span class="sxs-lookup"><span data-stu-id="58b0a-277">gRPC is a modern, high-performance framework that evolves the age-old [remote procedure call (RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call) protocol.</span></span> <span data-ttu-id="58b0a-278">gRPC 使用 HTTP/2 作为其传输协议，通过 HTTP RESTFul 服务提供显著的性能改进，包括：</span><span class="sxs-lookup"><span data-stu-id="58b0a-278">gRPC uses HTTP/2 for its transport protocol, which provides significant performance enhancements over HTTP RESTFul service, including:</span></span>

- <span data-ttu-id="58b0a-279">多路复用支持通过同一连接发送多个并行请求-HTTP 1.1 限制处理一次请求/响应消息。</span><span class="sxs-lookup"><span data-stu-id="58b0a-279">Multiplexing support for sending multiple parallel requests over the same connection - HTTP 1.1 limits processing to one request/response message at a time.</span></span>
- <span data-ttu-id="58b0a-280">同时发送客户端请求和服务器响应的双向全双工通信。</span><span class="sxs-lookup"><span data-stu-id="58b0a-280">Bidirectional full-duplex communication for sending both client requests and server responses simultaneously.</span></span>
- <span data-ttu-id="58b0a-281">内置流式处理可对大型数据集进行异步流式处理的请求和响应。</span><span class="sxs-lookup"><span data-stu-id="58b0a-281">Built-in streaming enabling requests and responses to asynchronously stream large data sets.</span></span>

## <a name="dapr-and-service-meshes"></a><span data-ttu-id="58b0a-282">Dapr 和服务网格</span><span class="sxs-lookup"><span data-stu-id="58b0a-282">Dapr and service meshes</span></span>

<span data-ttu-id="58b0a-283">服务网格是针对分布式应用程序的另一种快速发展的技术。</span><span class="sxs-lookup"><span data-stu-id="58b0a-283">Service mesh is another rapidly evolving technology for distributed applications.</span></span>

<span data-ttu-id="58b0a-284">服务网格是一个可配置的基础结构层，其中内置了用于处理服务到服务通信、复原、负载平衡和遥测捕获的功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-284">A service mesh is a configurable infrastructure layer with built-in capabilities to handle service-to-service communication, resiliency, load balancing, and telemetry capture.</span></span> <span data-ttu-id="58b0a-285">它会将这些问题的责任转移到服务，并将其移到服务网格层中。</span><span class="sxs-lookup"><span data-stu-id="58b0a-285">It moves the responsibility for these concerns out of the services and into the service mesh layer.</span></span> <span data-ttu-id="58b0a-286">与 Dapr 一样，服务网格还遵循挎斗体系结构。</span><span class="sxs-lookup"><span data-stu-id="58b0a-286">Like Dapr, a service mesh also follows a sidecar architecture.</span></span>

<span data-ttu-id="58b0a-287">图2-8 显示了实现服务网格技术的应用程序。</span><span class="sxs-lookup"><span data-stu-id="58b0a-287">Figure 2-8 shows an application that implements service mesh technology.</span></span>

![服务网格](./media/dapr-at-20000-feet/service-mesh-with-side-car.png)

<span data-ttu-id="58b0a-289">**图 2-8**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-289">**Figure 2-8**.</span></span> <span data-ttu-id="58b0a-290">带有侧面汽车的服务网格。</span><span class="sxs-lookup"><span data-stu-id="58b0a-290">Service mesh with a side car.</span></span>

<span data-ttu-id="58b0a-291">上图显示了通过与每个服务一起运行的代理来截获消息的方式。</span><span class="sxs-lookup"><span data-stu-id="58b0a-291">The previous figure shows how messages are intercepted by a proxy that runs alongside each service.</span></span> <span data-ttu-id="58b0a-292">每个代理都可以配置特定于该服务的流量规则。</span><span class="sxs-lookup"><span data-stu-id="58b0a-292">Each proxy can be configured with traffic rules specific to the service.</span></span> <span data-ttu-id="58b0a-293">它了解消息，并可以将消息路由到您的服务和外界。</span><span class="sxs-lookup"><span data-stu-id="58b0a-293">It understands messages and can route them across your services and the outside world.</span></span>

<span data-ttu-id="58b0a-294">那么，问题变成 "正在 Dapr 服务网格？"。</span><span class="sxs-lookup"><span data-stu-id="58b0a-294">So the question becomes, "Is Dapr a service mesh?".</span></span>

<span data-ttu-id="58b0a-295">尽管这两种方法都使用挎斗体系结构，但每种技术都有不同的用途。</span><span class="sxs-lookup"><span data-stu-id="58b0a-295">While both use a sidecar architecture, each technology has a different purpose.</span></span> <span data-ttu-id="58b0a-296">Dapr 提供分布式应用程序功能。</span><span class="sxs-lookup"><span data-stu-id="58b0a-296">Dapr provides distributed application features.</span></span> <span data-ttu-id="58b0a-297">服务网格提供专用的网络基础结构层。</span><span class="sxs-lookup"><span data-stu-id="58b0a-297">A service mesh provides a dedicated network infrastructure layer.</span></span>

<span data-ttu-id="58b0a-298">每个在不同级别工作时，都可以在同一应用程序中协同工作。</span><span class="sxs-lookup"><span data-stu-id="58b0a-298">As each works at a different level, both can work together in the same application.</span></span> <span data-ttu-id="58b0a-299">例如，服务网格可以提供服务之间的网络通信。</span><span class="sxs-lookup"><span data-stu-id="58b0a-299">For example, a service mesh could provide networking communication between services.</span></span> <span data-ttu-id="58b0a-300">Dapr 可以提供应用程序服务，例如状态管理或执行组件服务。</span><span class="sxs-lookup"><span data-stu-id="58b0a-300">Dapr could provide application services such as state management or actor services.</span></span>

<span data-ttu-id="58b0a-301">图2-9 显示了实现 Dapr 和服务网格技术的应用程序。</span><span class="sxs-lookup"><span data-stu-id="58b0a-301">Figure 2-9 shows an application that implements both Dapr and service mesh technology.</span></span>

![Dapr 和 Service 网格一起](./media/dapr-at-20000-feet/dapr-and-service-mesh.png)

<span data-ttu-id="58b0a-303">**图 2-9**。</span><span class="sxs-lookup"><span data-stu-id="58b0a-303">**Figure 2-9**.</span></span> <span data-ttu-id="58b0a-304">将 Dapr 和 service 网格组合在一起。</span><span class="sxs-lookup"><span data-stu-id="58b0a-304">Dapr and service mesh together.</span></span>

<span data-ttu-id="58b0a-305">在本书中， [学习 Dapr](https://www.amazon.com/Learning-Dapr-Building-Distributed-Applications/dp/1492072427/ref=sr_1_1?dchild=1&keywords=dapr&qid=1604794794&sr=8-1)、作者 Haishi Bai 和 Yaron Schneider，涵盖了 Dapr 和 service 网格的集成。</span><span class="sxs-lookup"><span data-stu-id="58b0a-305">In the book, [Learning Dapr](https://www.amazon.com/Learning-Dapr-Building-Distributed-Applications/dp/1492072427/ref=sr_1_1?dchild=1&keywords=dapr&qid=1604794794&sr=8-1), authors Haishi Bai and Yaron Schneider, cover the integration of Dapr and service mesh.</span></span>

## <a name="summary"></a><span data-ttu-id="58b0a-306">总结</span><span class="sxs-lookup"><span data-stu-id="58b0a-306">Summary</span></span>

<span data-ttu-id="58b0a-307">本章介绍了 Dapr 的分布式应用程序运行时。</span><span class="sxs-lookup"><span data-stu-id="58b0a-307">This chapter introduced you to Dapr, a Distributed Application Runtime.</span></span>

<span data-ttu-id="58b0a-308">Dapr 是由 Microsoft 赞助的开源项目，可与客户和开源社区密切合作。</span><span class="sxs-lookup"><span data-stu-id="58b0a-308">Dapr is an open-source project sponsored by Microsoft with close collaboration from customers and the open-source community.</span></span>

<span data-ttu-id="58b0a-309">Dapr 的核心有助于降低分布式微服务应用程序的固有复杂性。</span><span class="sxs-lookup"><span data-stu-id="58b0a-309">At its core, Dapr helps reduce the inherent complexity of distributed microservice applications.</span></span> <span data-ttu-id="58b0a-310">它以构建块 Api 的概念为基础构建。</span><span class="sxs-lookup"><span data-stu-id="58b0a-310">It's built upon a concept of building block APIs.</span></span> <span data-ttu-id="58b0a-311">Dapr 构建基块公开常见的分布式应用程序功能，例如状态管理、服务到服务调用和发布/订阅消息传送。</span><span class="sxs-lookup"><span data-stu-id="58b0a-311">Dapr building blocks expose common distributed application capabilities, such as state management, service-to-service invocation, and pub/sub messaging.</span></span> <span data-ttu-id="58b0a-312">Dapr 组件位于构建基块下，并为每个功能提供具体的实现。</span><span class="sxs-lookup"><span data-stu-id="58b0a-312">Dapr components lie beneath the building blocks and provide the concrete implementation for each capability.</span></span> <span data-ttu-id="58b0a-313">应用程序通过配置文件绑定到各种组件。</span><span class="sxs-lookup"><span data-stu-id="58b0a-313">Applications bind to various components through configuration files.</span></span>

<span data-ttu-id="58b0a-314">在下面的章节中，我们提供了有关如何在应用程序中使用 Dapr 的实用实践说明。</span><span class="sxs-lookup"><span data-stu-id="58b0a-314">In the next chapters, we present practical, hands-on instruction on how to use Dapr in your applications.</span></span>

### <a name="references"></a><span data-ttu-id="58b0a-315">参考</span><span class="sxs-lookup"><span data-stu-id="58b0a-315">References</span></span>

- [<span data-ttu-id="58b0a-316">Dapr 文档</span><span class="sxs-lookup"><span data-stu-id="58b0a-316">Dapr documentation</span></span>](https://dapr.io/)
- [<span data-ttu-id="58b0a-317">学习 Dapr</span><span class="sxs-lookup"><span data-stu-id="58b0a-317">Learning Dapr</span></span>](https://www.amazon.com/Learning-Dapr-Building-Distributed-Applications/dp/1492072427/ref=sr_1_1?dchild=1&keywords=dapr&qid=1604794794&sr=8-1)
- [<span data-ttu-id="58b0a-318">.NET 微服务：容器化 .NET 应用程序的体系结构</span><span class="sxs-lookup"><span data-stu-id="58b0a-318">.NET Microservices: Architecture for Containerized .NET applications</span></span>](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)
- [<span data-ttu-id="58b0a-319">构建适用于 Azure 的 .NET 应用 Cloud-Native</span><span class="sxs-lookup"><span data-stu-id="58b0a-319">Architecting Cloud-Native .NET Apps for Azure</span></span>](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf)

> [!div class="step-by-step"]
> <span data-ttu-id="58b0a-320">[上一页](the-world-is-distributed.md)
> [下一页](getting-started.md)</span><span class="sxs-lookup"><span data-stu-id="58b0a-320">[Previous](the-world-is-distributed.md)
[Next](getting-started.md)</span></span>
