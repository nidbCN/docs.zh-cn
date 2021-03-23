---
title: EShopOnDapr 参考应用程序简介
description: EShopOnDapr 引用应用程序及其历史记录的概述。
author: amolenk
ms.date: 02/17/2021
ms.openlocfilehash: 47d99246eec8e3288738c911cccb23bb9cac8df3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874558"
---
# <a name="dapr-reference-application"></a><span data-ttu-id="ef06b-103">Dapr 引用应用程序</span><span class="sxs-lookup"><span data-stu-id="ef06b-103">Dapr reference application</span></span>

<span data-ttu-id="ef06b-104">本书前面介绍了 Dapr 的基础知识。</span><span class="sxs-lookup"><span data-stu-id="ef06b-104">Earlier in the book, you've learned about the foundational benefits of Dapr.</span></span> <span data-ttu-id="ef06b-105">你了解了 Dapr 如何帮助你的团队构造分布式应用程序，同时降低体系结构和操作的复杂性。</span><span class="sxs-lookup"><span data-stu-id="ef06b-105">You saw how Dapr can help your team construct distributed applications while reducing architectural and operational complexity.</span></span> <span data-ttu-id="ef06b-106">在此过程中，您有机会构建一些小规模的 Dapr 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef06b-106">Along the way, you've had the opportunity to build some small Dapr apps.</span></span> <span data-ttu-id="ef06b-107">现在，可以浏览端到端微服务应用程序，该应用程序演示了 Dapr 构建基块和组件。</span><span class="sxs-lookup"><span data-stu-id="ef06b-107">Now, it's time to explore an end-to-end microservice application that demonstrates Dapr building blocks and components.</span></span>

<span data-ttu-id="ef06b-108">但首先要记录一些历史记录。</span><span class="sxs-lookup"><span data-stu-id="ef06b-108">But, first a little history.</span></span>

## <a name="eshop-on-containers"></a><span data-ttu-id="ef06b-109">容器上的 eShop</span><span class="sxs-lookup"><span data-stu-id="ef06b-109">eShop on containers</span></span>

<span data-ttu-id="ef06b-110">几年前，Microsoft （与领先社区专家合作）发布了一个热门的指南书籍， [为容器化 .Net 应用程序提供 .Net 微服务](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf)。</span><span class="sxs-lookup"><span data-stu-id="ef06b-110">Several years ago, Microsoft, in partnership with leading community experts, released a popular guidance book, entitled [.NET Microservices for Containerized .NET Applications](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf).</span></span> <span data-ttu-id="ef06b-111">图3-1 显示了本书：</span><span class="sxs-lookup"><span data-stu-id="ef06b-111">Figure 3-1 shows the book:</span></span>

![构建容器化的微服务 .NET 应用程序。](./media/reference-application/architecting-microservices-book.png)

<span data-ttu-id="ef06b-113">**图 3-1**。</span><span class="sxs-lookup"><span data-stu-id="ef06b-113">**Figure 3-1**.</span></span> <span data-ttu-id="ef06b-114">.NET 微服务：容器化 .NET 应用程序的体系结构。</span><span class="sxs-lookup"><span data-stu-id="ef06b-114">.NET Microservices: Architecture for Containerized .NET Applications.</span></span>

<span data-ttu-id="ef06b-115">本书 dove 了有关生成分布式应用程序的原则、模式和最佳实践。</span><span class="sxs-lookup"><span data-stu-id="ef06b-115">The book dove deep into the principles, patterns, and best practices for building distributed applications.</span></span> <span data-ttu-id="ef06b-116">它包含一个功能齐全的微服务参考应用程序，可展示体系结构概念。</span><span class="sxs-lookup"><span data-stu-id="ef06b-116">It included a full-featured microservice reference application that showcased the architectural concepts.</span></span> <span data-ttu-id="ef06b-117">[EShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)，该应用程序显示了一名电子商务店面，其中销售了各种 .net 项目，包括衣服和咖啡杯子。</span><span class="sxs-lookup"><span data-stu-id="ef06b-117">Entitled, [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers), the application shows an e-Commerce storefront that sells various .NET items, including clothing and coffee mugs.</span></span>  <span data-ttu-id="ef06b-118">内置于 .NET Core，它是跨平台的应用程序，可以在 Linux 或 Windows 容器中运行。</span><span class="sxs-lookup"><span data-stu-id="ef06b-118">Built in .NET Core, the application is cross-platform and can run in either Linux or Windows containers.</span></span> <span data-ttu-id="ef06b-119">图3-2 显示了原始 eShop 的体系结构。</span><span class="sxs-lookup"><span data-stu-id="ef06b-119">Figure 3-2 shows the original eShop architecture.</span></span>

![eShopOnContainers 参考应用程序体系结构。](./media/reference-application/eshop-on-containers.png)

<span data-ttu-id="ef06b-121">**图 3-2**。</span><span class="sxs-lookup"><span data-stu-id="ef06b-121">**Figure 3-2**.</span></span> <span data-ttu-id="ef06b-122">原始 `ShopOnContainers` 引用应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef06b-122">Original `ShopOnContainers` reference application.</span></span>

<span data-ttu-id="ef06b-123">如您所见，eShopOnContainers 包含许多移动部件：</span><span class="sxs-lookup"><span data-stu-id="ef06b-123">As you can see, eShopOnContainers includes many moving parts:</span></span>

1. <span data-ttu-id="ef06b-124">三个不同的前端客户端。</span><span class="sxs-lookup"><span data-stu-id="ef06b-124">Three different front-end clients.</span></span>
1. <span data-ttu-id="ef06b-125">用于从前端抽象后端的应用程序网关。</span><span class="sxs-lookup"><span data-stu-id="ef06b-125">An application gateway to abstract the back end from the front end.</span></span>
1. <span data-ttu-id="ef06b-126">多个后端核心微服务。</span><span class="sxs-lookup"><span data-stu-id="ef06b-126">Several back-end core microservices.</span></span>
1. <span data-ttu-id="ef06b-127">启用异步发布/订阅消息传递的事件总线组件。</span><span class="sxs-lookup"><span data-stu-id="ef06b-127">An event bus component that enables asynchronous pub/sub messaging.</span></span>

<span data-ttu-id="ef06b-128">EShopOnContainers 引用应用程序已在 .NET 社区中广泛接受，并用于对许多大型商业微服务应用程序进行建模。</span><span class="sxs-lookup"><span data-stu-id="ef06b-128">The eShopOnContainers reference application has been widely accepted across the .NET community and used to model many large commercial microservice applications.</span></span>

## <a name="eshop-on-dapr"></a><span data-ttu-id="ef06b-129">Dapr 上的 eShop</span><span class="sxs-lookup"><span data-stu-id="ef06b-129">eShop on Dapr</span></span>

<span data-ttu-id="ef06b-130">本书随附了其他版本的 eShop 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef06b-130">An alternative version of the eShop application accompanies this book.</span></span> <span data-ttu-id="ef06b-131">这称为 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)。</span><span class="sxs-lookup"><span data-stu-id="ef06b-131">It's called [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr).</span></span> <span data-ttu-id="ef06b-132">更新后的版本通过集成 Dapr 构建基块和组件来发展早期的 eShopOnContainers 应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef06b-132">The updated version evolves the earlier eShopOnContainers application by integrating Dapr building blocks and components.</span></span> <span data-ttu-id="ef06b-133">图3-3 显示了新的简化解决方案体系结构：</span><span class="sxs-lookup"><span data-stu-id="ef06b-133">Figure 3-3 shows the new streamlined solution architecture:</span></span>  

![eShopOnDapr 参考应用程序体系结构。](./media/reference-application/eshop-on-dapr.png)

<span data-ttu-id="ef06b-135">**图 3-3**。</span><span class="sxs-lookup"><span data-stu-id="ef06b-135">**Figure 3-3**.</span></span> <span data-ttu-id="ef06b-136">eShopOnDapr 参考应用程序体系结构。</span><span class="sxs-lookup"><span data-stu-id="ef06b-136">eShopOnDapr reference application architecture.</span></span>

<span data-ttu-id="ef06b-137">由于 eShopOnDapr 引用应用程序的焦点在 Dapr 上，因此原始应用程序已更新。</span><span class="sxs-lookup"><span data-stu-id="ef06b-137">As focus of the eShopOnDapr reference application is on Dapr, the original application has been updated.</span></span> <span data-ttu-id="ef06b-138">该体系结构包括：</span><span class="sxs-lookup"><span data-stu-id="ef06b-138">The architecture consists of:</span></span>

1. <span data-ttu-id="ef06b-139">用常用角 SPA 框架编写的 [单页面应用程序](/archive/msdn-magazine/2013/november/asp-net-single-page-applications-build-modern-responsive-web-apps-with-asp-net) 前端。</span><span class="sxs-lookup"><span data-stu-id="ef06b-139">A [Single Page Application](/archive/msdn-magazine/2013/november/asp-net-single-page-applications-build-modern-responsive-web-apps-with-asp-net) front end written in the popular Angular SPA framework.</span></span> <span data-ttu-id="ef06b-140">它将用户请求发送到 API 网关微服务。</span><span class="sxs-lookup"><span data-stu-id="ef06b-140">It sends user requests to an API gateway microservice.</span></span>

1. <span data-ttu-id="ef06b-141">API 网关会从前端客户端抽象后端核心微服务。</span><span class="sxs-lookup"><span data-stu-id="ef06b-141">The API gateway abstracts the back-end core microservices from the front-end client.</span></span> <span data-ttu-id="ef06b-142">它使用 [Envoy](https://www.envoyproxy.io/)（一种高性能的开源服务代理）来实现。</span><span class="sxs-lookup"><span data-stu-id="ef06b-142">It's implemented using [Envoy](https://www.envoyproxy.io/), a high performant, open-source service proxy.</span></span> <span data-ttu-id="ef06b-143">Envoy 将传入请求路由到各种后端微服务。</span><span class="sxs-lookup"><span data-stu-id="ef06b-143">Envoy routes  incoming requests to various back-end microservices.</span></span> <span data-ttu-id="ef06b-144">大多数请求是简单的 CRUD 操作 (例如，从目录获取品牌列表，) 并通过直接调用后端微服务来处理。</span><span class="sxs-lookup"><span data-stu-id="ef06b-144">Most requests are simple CRUD operations (for example, get the list of brands from the catalog) and handled by a direct call to a back-end microservice.</span></span>

1. <span data-ttu-id="ef06b-145">其他请求逻辑上比较复杂，并且需要多个微服务一起工作。</span><span class="sxs-lookup"><span data-stu-id="ef06b-145">Other requests are logically more complex and require multiple microservices to work together.</span></span> <span data-ttu-id="ef06b-146">在这些情况下，eShopOnDapr 实现了一个 [聚合器微服务](../cloud-native/service-to-service-communication.md#service-aggregator-pattern) ，用于在完成操作所需的微服务间协调工作流。</span><span class="sxs-lookup"><span data-stu-id="ef06b-146">For these cases, eShopOnDapr implements an [aggregator microservice](../cloud-native/service-to-service-communication.md#service-aggregator-pattern) that orchestrates a workflow across the microservices needed to complete the operation.</span></span>

1. <span data-ttu-id="ef06b-147">一组核心后端微服务包含电子商务存储所需的功能。</span><span class="sxs-lookup"><span data-stu-id="ef06b-147">The set of core back-end microservices includes functionality required for an e-Commerce store.</span></span> <span data-ttu-id="ef06b-148">每个都是独立的，并且独立于其他。</span><span class="sxs-lookup"><span data-stu-id="ef06b-148">Each is self-contained and independent of the others.</span></span> <span data-ttu-id="ef06b-149">以下是广泛接受的域分解模式，每个微服务都隔离特定的 *业务功能*：</span><span class="sxs-lookup"><span data-stu-id="ef06b-149">Following widely accepted domain decomposing patterns, each microservice isolates a specific *business capability*:</span></span>

   - <span data-ttu-id="ef06b-150">购物篮服务管理客户的购物篮体验。</span><span class="sxs-lookup"><span data-stu-id="ef06b-150">The basket service manages the customer's shopping basket experience.</span></span>
   - <span data-ttu-id="ef06b-151">目录服务管理可用于销售的产品项。</span><span class="sxs-lookup"><span data-stu-id="ef06b-151">The catalog service manages product items available for sale.</span></span>
   - <span data-ttu-id="ef06b-152">标识服务管理身份验证和标识。</span><span class="sxs-lookup"><span data-stu-id="ef06b-152">The identity service manages authentication and identity.</span></span>
   - <span data-ttu-id="ef06b-153">订购服务处理放置和管理订单的所有方面。</span><span class="sxs-lookup"><span data-stu-id="ef06b-153">The ordering service handles all aspects of placing and managing orders.</span></span>
   - <span data-ttu-id="ef06b-154">支付服务开展客户的付款。</span><span class="sxs-lookup"><span data-stu-id="ef06b-154">The payment service transacts the customer's payment.</span></span>

   <span data-ttu-id="ef06b-155">每个服务都有自己的持久存储。</span><span class="sxs-lookup"><span data-stu-id="ef06b-155">Each service has its own persistent storage.</span></span> <span data-ttu-id="ef06b-156">坚持微服务 [最佳实践](../cloud-native/distributed-data.md#database-per-microservice-why)，不存在所有服务都交互的共享数据存储。</span><span class="sxs-lookup"><span data-stu-id="ef06b-156">Adhering to microservice [best practices](../cloud-native/distributed-data.md#database-per-microservice-why), there's not a shared datastore with which all services interact.</span></span>

   <span data-ttu-id="ef06b-157">每个微服务的设计都基于其各自的要求。</span><span class="sxs-lookup"><span data-stu-id="ef06b-157">The design of each microservice is based on its individual requirements.</span></span> <span data-ttu-id="ef06b-158">简单服务使用基本的 CRUD 操作来访问其基础数据存储区。</span><span class="sxs-lookup"><span data-stu-id="ef06b-158">The simple services use basic CRUD operations to access to their underlying data stores.</span></span> <span data-ttu-id="ef06b-159">高级服务（如排序）使用 Domain-Driven 设计方法来管理业务复杂性。</span><span class="sxs-lookup"><span data-stu-id="ef06b-159">Advanced services, like Ordering, use a  Domain-Driven Design approach to manage business complexity.</span></span> <span data-ttu-id="ef06b-160">必要时，可以跨不同的技术堆栈（如 .NET Core、Java、中转、NodeJS 等）生成服务。</span><span class="sxs-lookup"><span data-stu-id="ef06b-160">If necessary, services could be built across different technology stacks, such as .NET Core, Java, Go, NodeJS, and more.</span></span>

1. <span data-ttu-id="ef06b-161">最后，事件总线会包装 Dapr 发布/订阅组件。</span><span class="sxs-lookup"><span data-stu-id="ef06b-161">Finally, the event bus wraps the Dapr publish/subscribe components.</span></span> <span data-ttu-id="ef06b-162">它通过微服务实现异步发布/订阅消息传送。</span><span class="sxs-lookup"><span data-stu-id="ef06b-162">It enables asynchronous publish/subscribe messaging across microservices.</span></span> <span data-ttu-id="ef06b-163">开发人员可以插入任何 Dapr 支持的消息代理。</span><span class="sxs-lookup"><span data-stu-id="ef06b-163">Developers can plug in any Dapr-supported message broker.</span></span>

### <a name="application-of-dapr-building-blocks"></a><span data-ttu-id="ef06b-164">Dapr 构建基块的应用程序</span><span class="sxs-lookup"><span data-stu-id="ef06b-164">Application of Dapr building blocks</span></span>

<span data-ttu-id="ef06b-165">EShopOnDapr 基本代码比 eShopOnContainers 基本代码更简单。</span><span class="sxs-lookup"><span data-stu-id="ef06b-165">The eShopOnDapr codebase is more streamlined than the eShopOnContainers codebase.</span></span> <span data-ttu-id="ef06b-166">Dapr 构建基块替换大量易出错的管道代码。</span><span class="sxs-lookup"><span data-stu-id="ef06b-166">Dapr building blocks replace a large amount of error-prone plumbing code.</span></span>

<span data-ttu-id="ef06b-167">图3-4 显示了 eShop 引用应用程序中的 Dapr 集成。</span><span class="sxs-lookup"><span data-stu-id="ef06b-167">Figure 3-4 shows the Dapr integration in the eShop reference application.</span></span>

![eShopOnDapr 参考应用程序体系结构](./media/reference-application/eshop-on-dapr-buildingblocks.png)

<span data-ttu-id="ef06b-169">**图 3-4**。</span><span class="sxs-lookup"><span data-stu-id="ef06b-169">**Figure 3-4**.</span></span> <span data-ttu-id="ef06b-170">EShopOnDapr 中的 Dapr 集成。</span><span class="sxs-lookup"><span data-stu-id="ef06b-170">Dapr integration in eShopOnDapr.</span></span>

<span data-ttu-id="ef06b-171">在上图中，可以看到哪些服务使用哪些 Dapr 构建基块。</span><span class="sxs-lookup"><span data-stu-id="ef06b-171">In the previous figure, you can see which services use which Dapr building blocks.</span></span>

1. <span data-ttu-id="ef06b-172">原始 eShopOnContainers 应用程序演示了订单服务中的 DDD 概念和模式。</span><span class="sxs-lookup"><span data-stu-id="ef06b-172">The original eShopOnContainers application demonstrates DDD concepts and patterns in the ordering service.</span></span> <span data-ttu-id="ef06b-173">在更新的 eShopOnDapr 中，订购服务使用执行组件 *构建基块* 作为替代实现。</span><span class="sxs-lookup"><span data-stu-id="ef06b-173">In the updated eShopOnDapr, the ordering service uses the *actor building block* as an alternative implementation.</span></span> <span data-ttu-id="ef06b-174">使用者的基于车的访问模型可以轻松实现具有取消支持的有状态订单过程。</span><span class="sxs-lookup"><span data-stu-id="ef06b-174">The turn-based access model of actors makes it easy to implement a stateful ordering process with support for cancellation.</span></span>
1. <span data-ttu-id="ef06b-175">订购服务使用 [绑定构建基块](bindings.md)发送订单确认电子邮件。</span><span class="sxs-lookup"><span data-stu-id="ef06b-175">The ordering service sends order confirmation e-mails using the [bindings building block](bindings.md).</span></span>
1. <span data-ttu-id="ef06b-176">后端服务使用 [publish & 订阅构建块](publish-subscribe.md)进行异步通信。</span><span class="sxs-lookup"><span data-stu-id="ef06b-176">The back-end services communicate asynchronously using the [publish & subscribe building block](publish-subscribe.md).</span></span>
1. <span data-ttu-id="ef06b-177">机密管理由 [机密构建块](secrets.md)完成。</span><span class="sxs-lookup"><span data-stu-id="ef06b-177">Secret management is done by the [secrets building block](secrets.md).</span></span>
1. <span data-ttu-id="ef06b-178">API 网关和 web 购物聚合器服务使用 [服务调用构建基块](service-invocation.md) 对后端服务调用方法。</span><span class="sxs-lookup"><span data-stu-id="ef06b-178">The API gateway and web shopping aggregator services use the [service invocation building block](service-invocation.md) to invoke methods on the back-end services.</span></span>
1. <span data-ttu-id="ef06b-179">购物篮服务使用 [状态管理构建基块](state-management.md) 来存储客户购物篮的状态。</span><span class="sxs-lookup"><span data-stu-id="ef06b-179">The basket service uses the [state management building block](state-management.md) to store the state of the customer's shopping basket.</span></span>

### <a name="benefits-of-applying-dapr-to-eshop"></a><span data-ttu-id="ef06b-180">将 Dapr 应用于 eShop 的好处</span><span class="sxs-lookup"><span data-stu-id="ef06b-180">Benefits of applying Dapr to eShop</span></span>

<span data-ttu-id="ef06b-181">通常，使用 Dapr 构建基块可向应用程序添加可观察性和灵活性：</span><span class="sxs-lookup"><span data-stu-id="ef06b-181">In general, the use of Dapr building blocks add observability and flexibility to the application:</span></span>

1. <span data-ttu-id="ef06b-182">可观察性：通过使用 Dapr 构建基块，可为服务之间的两个调用和 Dapr 组件获取丰富的分布式跟踪，而无需编写任何代码。</span><span class="sxs-lookup"><span data-stu-id="ef06b-182">Observability: By using the Dapr building blocks, you gain rich distributed tracing for both calls between services and to Dapr components without having to write any code.</span></span> <span data-ttu-id="ef06b-183">在 eShopOnContainers 中，将使用大量的自定义日志记录来提供见解。</span><span class="sxs-lookup"><span data-stu-id="ef06b-183">In eShopOnContainers, a large amount of custom logging is used to provide insight.</span></span>
1. <span data-ttu-id="ef06b-184">灵活性：你现在可以通过更改组件配置文件来仅 *替换* 基础结构。</span><span class="sxs-lookup"><span data-stu-id="ef06b-184">Flexibility: You can now *swap out* infrastructure simply by changing a component configuration file.</span></span> <span data-ttu-id="ef06b-185">不需要更改代码。</span><span class="sxs-lookup"><span data-stu-id="ef06b-185">No code changes are necessary.</span></span>

<span data-ttu-id="ef06b-186">下面是特定构建基块提供的一些优点的更多示例：</span><span class="sxs-lookup"><span data-stu-id="ef06b-186">Here are some more examples of benefits offered by specific building blocks:</span></span>

- <span data-ttu-id="ef06b-187">**服务调用**</span><span class="sxs-lookup"><span data-stu-id="ef06b-187">**Service Invocation**</span></span>
  - <span data-ttu-id="ef06b-188">借助 Dapr 的对 [mTLS](https://blog.cloudflare.com/introducing-tls-client-auth/)的支持，服务现在通过加密通道进行通信。</span><span class="sxs-lookup"><span data-stu-id="ef06b-188">With Dapr's support for [mTLS](https://blog.cloudflare.com/introducing-tls-client-auth/), services now communicate through encrypted channels.</span></span>
  - <span data-ttu-id="ef06b-189">发生暂时性错误时，服务调用会自动重试。</span><span class="sxs-lookup"><span data-stu-id="ef06b-189">When transient errors occur, service calls are automatically retried.</span></span>
  - <span data-ttu-id="ef06b-190">自动服务发现可减少服务彼此查找所需的配置量。</span><span class="sxs-lookup"><span data-stu-id="ef06b-190">Automatic service discovery reduces the amount of configuration needed for services to find each other.</span></span>

- <span data-ttu-id="ef06b-191">**发布/订阅**</span><span class="sxs-lookup"><span data-stu-id="ef06b-191">**Publish/Subscribe**</span></span>
  - <span data-ttu-id="ef06b-192">eShopOnContainer 包含大量自定义代码来支持 Azure 服务总线和 RabbitMQ。</span><span class="sxs-lookup"><span data-stu-id="ef06b-192">eShopOnContainer included a large amount of custom code to support both Azure Service Bus and RabbitMQ.</span></span> <span data-ttu-id="ef06b-193">开发人员使用 Azure Service Bus 进行生产和 RabbitMQ，用于本地开发和测试。</span><span class="sxs-lookup"><span data-stu-id="ef06b-193">Developers used Azure Service Bus for production and RabbitMQ for local development and testing.</span></span> <span data-ttu-id="ef06b-194">创建了一个 `IEventBus` 抽象层，以便在这些消息代理之间进行交换。</span><span class="sxs-lookup"><span data-stu-id="ef06b-194">An `IEventBus` abstraction layer was created to enable swapping between these message brokers.</span></span> <span data-ttu-id="ef06b-195">此层包括约 *700 行易出错的代码*。</span><span class="sxs-lookup"><span data-stu-id="ef06b-195">This layer consisted of approximately *700 lines of error-prone code*.</span></span> <span data-ttu-id="ef06b-196">带 Dapr 的更新后的实现仅需 *35 行代码*。</span><span class="sxs-lookup"><span data-stu-id="ef06b-196">The updated implementation with Dapr requires only *35 lines of code*.</span></span> <span data-ttu-id="ef06b-197">这是原始代码行的 **5%** ！</span><span class="sxs-lookup"><span data-stu-id="ef06b-197">That's **5%** of the original lines of code!</span></span> <span data-ttu-id="ef06b-198">更重要的是，实现既简单又易于理解。</span><span class="sxs-lookup"><span data-stu-id="ef06b-198">More importantly, the implementation is straightforward and easy to understand.</span></span>
  - <span data-ttu-id="ef06b-199">eShopOnDapr 使用 Dapr 的丰富 ASP.NET Core 集成来使用发布/订阅。</span><span class="sxs-lookup"><span data-stu-id="ef06b-199">eShopOnDapr uses Dapr's rich ASP.NET Core integration to use pub/sub.</span></span> <span data-ttu-id="ef06b-200">将 `Topic` 属性添加到 ASP.NET Core 控制器方法来订阅消息。</span><span class="sxs-lookup"><span data-stu-id="ef06b-200">You add `Topic` attributes to ASP.NET Core controller methods to subscribe to messages.</span></span> <span data-ttu-id="ef06b-201">因此，无需为每个消息代理编写单独的消息处理程序循环。</span><span class="sxs-lookup"><span data-stu-id="ef06b-201">Therefore, there's no need to write a separate message handler loop for each message broker.</span></span>
  - <span data-ttu-id="ef06b-202">通过 HTTP 调用将消息作为消息路由，可以使用 ASP.NET Core 中间件来添加功能，而不会引入新概念或 Sdk 来了解。</span><span class="sxs-lookup"><span data-stu-id="ef06b-202">Messages routed to the service as HTTP calls enable the use of ASP.NET Core middleware to add functionality, without introducing new concepts or SDKs to learn.</span></span>

- <span data-ttu-id="ef06b-203">**绑定**</span><span class="sxs-lookup"><span data-stu-id="ef06b-203">**Bindings**</span></span>
  - <span data-ttu-id="ef06b-204">EShopOnContainers 解决方案包含一个待办事项， *用于向客户* 发送订单确认电子邮件。</span><span class="sxs-lookup"><span data-stu-id="ef06b-204">The eShopOnContainers solution contained a *to-do* item for e-mailing an order confirmation to the customer.</span></span> <span data-ttu-id="ef06b-205">其思想是最终实现第三方电子邮件 API，如 SendGrid。</span><span class="sxs-lookup"><span data-stu-id="ef06b-205">The thought was to eventually implement a third-party email API such as SendGrid.</span></span> <span data-ttu-id="ef06b-206">使用 Dapr，实现电子邮件通知就像配置资源绑定一样简单。</span><span class="sxs-lookup"><span data-stu-id="ef06b-206">With Dapr, implementing email notification was as easy as configuring a resource binding.</span></span> <span data-ttu-id="ef06b-207">无需学习外部 Api 或 Sdk。</span><span class="sxs-lookup"><span data-stu-id="ef06b-207">There wasn't any need to learn external APIs or SDKs.</span></span>

> [!NOTE]
> <span data-ttu-id="ef06b-208">本书的第一版中未介绍执行组件构建基块。</span><span class="sxs-lookup"><span data-stu-id="ef06b-208">The Actors building block isn't covered in the first version of this book.</span></span> <span data-ttu-id="ef06b-209">1.1 更新中将包含有关执行组件构建块及其与 eShopOnDapr 的集成的详尽章节。</span><span class="sxs-lookup"><span data-stu-id="ef06b-209">An extensive chapter on the Actor building block and its integration with eShopOnDapr will be included in the 1.1 update.</span></span>

## <a name="summary"></a><span data-ttu-id="ef06b-210">总结</span><span class="sxs-lookup"><span data-stu-id="ef06b-210">Summary</span></span>

<span data-ttu-id="ef06b-211">本章介绍了 eShopOnDapr 引用应用程序。</span><span class="sxs-lookup"><span data-stu-id="ef06b-211">In this chapter, you're introduced to the eShopOnDapr reference application.</span></span> <span data-ttu-id="ef06b-212">这是广泛流行的 eShopOnContainers 微服务 reference 应用程序的演变。</span><span class="sxs-lookup"><span data-stu-id="ef06b-212">It's an evolution of the widely popular eShopOnContainers microservice reference application.</span></span> <span data-ttu-id="ef06b-213">eShopOnDapr 使用 Dapr 构建基块和组件替换大量自定义功能，大大简化了生成微服务应用程序所需的复杂性。</span><span class="sxs-lookup"><span data-stu-id="ef06b-213">eShopOnDapr replaces a large amount of custom functionality with Dapr building blocks and components, dramatically simplifying the complexities required to build a microservices application.</span></span>

### <a name="references"></a><span data-ttu-id="ef06b-214">参考</span><span class="sxs-lookup"><span data-stu-id="ef06b-214">References</span></span>

- [<span data-ttu-id="ef06b-215">GitHub 上的 eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="ef06b-215">eShopOnDapr on GitHub</span></span>](https://github.com/dotnet-architecture/eShopOnDapr)

- [<span data-ttu-id="ef06b-216">GitHub 上的 eShopOnContainers</span><span class="sxs-lookup"><span data-stu-id="ef06b-216">eShopOnContainers on GitHub</span></span>](https://github.com/dotnet-architecture/eShopOnContainers)

- [<span data-ttu-id="ef06b-217">适用于容器化 .NET 应用程序的 .NET 微服务</span><span class="sxs-lookup"><span data-stu-id="ef06b-217">.NET Microservices for Containerized .NET Applications</span></span>](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf)

- [<span data-ttu-id="ef06b-218">构建适用于 Azure 的 .NET 应用 Cloud-Native</span><span class="sxs-lookup"><span data-stu-id="ef06b-218">Architecting Cloud-Native .NET Apps for Azure</span></span>](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf)

> [!div class="step-by-step"]
> <span data-ttu-id="ef06b-219">[上一页](getting-started.md)
> [下一页](state-management.md)</span><span class="sxs-lookup"><span data-stu-id="ef06b-219">[Previous](getting-started.md)
[Next](state-management.md)</span></span>
