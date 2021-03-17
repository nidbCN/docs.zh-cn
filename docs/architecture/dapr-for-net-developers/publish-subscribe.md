---
title: Dapr publish & 订阅构建基块
description: Dapr publish 的说明 & 订阅构造块以及如何应用它
author: edwinvw
ms.date: 02/17/2021
ms.openlocfilehash: 11898430d897ec85b7e367fa0e93ca912279784b
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623819"
---
# <a name="the-dapr-publish--subscribe-building-block"></a><span data-ttu-id="a838d-103">Dapr publish & 订阅构建基块</span><span class="sxs-lookup"><span data-stu-id="a838d-103">The Dapr publish & subscribe building block</span></span>

<span data-ttu-id="a838d-104">[发布-订阅模式](/azure/architecture/patterns/publisher-subscriber) (通常称为 "发布/订阅" ) 是众所周知且广泛使用的消息模式。</span><span class="sxs-lookup"><span data-stu-id="a838d-104">The [Publish-Subscribe pattern](/azure/architecture/patterns/publisher-subscriber) (often referred to as "pub/sub") is a well-known and widely used messaging pattern.</span></span> <span data-ttu-id="a838d-105">架构师通常在分布式应用程序中采用它。</span><span class="sxs-lookup"><span data-stu-id="a838d-105">Architects commonly embrace it in distributed applications.</span></span> <span data-ttu-id="a838d-106">但是，实现此方法的管道可能会很复杂。</span><span class="sxs-lookup"><span data-stu-id="a838d-106">However, the plumbing to implement it can be complex.</span></span> <span data-ttu-id="a838d-107">在不同的消息传送产品中，通常会有细微的功能差异。</span><span class="sxs-lookup"><span data-stu-id="a838d-107">There are often subtle feature differences across different messaging products.</span></span> <span data-ttu-id="a838d-108">Dapr 提供了一个构建基块，可显著简化实现发布/订阅功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-108">Dapr offers a building block that significantly simplifies implementing pub/sub functionality.</span></span>

## <a name="what-it-solves"></a><span data-ttu-id="a838d-109">解决方法</span><span class="sxs-lookup"><span data-stu-id="a838d-109">What it solves</span></span>

<span data-ttu-id="a838d-110">Publish-Subscribe 模式的主要优点是 **松散耦合**，有时称为 [临时分离](/azure/architecture/guide/technology-choices/messaging#decoupling)。</span><span class="sxs-lookup"><span data-stu-id="a838d-110">The primary advantage of the Publish-Subscribe pattern is **loose coupling**, sometimes referred to as [temporal decoupling](/azure/architecture/guide/technology-choices/messaging#decoupling).</span></span> <span data-ttu-id="a838d-111">模式将发送消息的服务分离 (**发布** 服务器上的服务) 从使用 **订阅服务器**)  (消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-111">The pattern decouples services that send messages (the **publishers**) from services that consume messages (the **subscribers**).</span></span> <span data-ttu-id="a838d-112">发布服务器和订阅服务器都不能识别每个订阅服务器，两者都依赖于分散消息的集中式 **消息代理** 。</span><span class="sxs-lookup"><span data-stu-id="a838d-112">Both publishers and subscribers are unaware of each other - both are dependent on a centralized **message broker** that distributes the messages.</span></span>

<span data-ttu-id="a838d-113">图7-1 显示了发布/订阅模式的高级体系结构。</span><span class="sxs-lookup"><span data-stu-id="a838d-113">Figure 7-1 shows the high-level architecture of the pub/sub pattern.</span></span>

![发布/订阅模式](./media/publish-subscribe/pub-sub-pattern.png)

<span data-ttu-id="a838d-115">图 7-1。</span><span class="sxs-lookup"><span data-stu-id="a838d-115">**Figure 7-1**.</span></span> <span data-ttu-id="a838d-116">发布/订阅模式。</span><span class="sxs-lookup"><span data-stu-id="a838d-116">The pub/sub pattern.</span></span>

<span data-ttu-id="a838d-117">从上图中，记下模式的步骤：</span><span class="sxs-lookup"><span data-stu-id="a838d-117">From the previous figure, note the steps of the pattern:</span></span>

1. <span data-ttu-id="a838d-118">发布服务器将消息发送到消息代理。</span><span class="sxs-lookup"><span data-stu-id="a838d-118">Publishers send messages to the message broker.</span></span>
1. <span data-ttu-id="a838d-119">订阅服务器将绑定到消息代理上的订阅。</span><span class="sxs-lookup"><span data-stu-id="a838d-119">Subscribers bind to a subscription on the message broker.</span></span>
1. <span data-ttu-id="a838d-120">消息代理将消息的副本转发给感兴趣的订阅。</span><span class="sxs-lookup"><span data-stu-id="a838d-120">The message broker forwards a copy of the message to interested subscriptions.</span></span>
1. <span data-ttu-id="a838d-121">订阅服务器从其订阅使用消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-121">Subscribers consume messages from their subscriptions.</span></span>

<span data-ttu-id="a838d-122">大多数消息代理封装了一种在收到消息后可以持久保存消息的排队机制。</span><span class="sxs-lookup"><span data-stu-id="a838d-122">Most message brokers encapsulate a queueing mechanism that can persist messages once received.</span></span> <span data-ttu-id="a838d-123">使用它，消息代理可通过存储消息来保证 **持久性** 。</span><span class="sxs-lookup"><span data-stu-id="a838d-123">With it, the message broker guarantees **durability** by storing the message.</span></span> <span data-ttu-id="a838d-124">当发布者发送消息时，订阅服务器无需立即可用，甚至不需要联机。</span><span class="sxs-lookup"><span data-stu-id="a838d-124">Subscribers don't need to be immediately available or even online when a publisher sends a message.</span></span> <span data-ttu-id="a838d-125">订阅者将接收并处理消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-125">Once available, the subscriber receives and processes the message.</span></span>  <span data-ttu-id="a838d-126">Dapr 为消息传递 **至少保证一次** 语义。</span><span class="sxs-lookup"><span data-stu-id="a838d-126">Dapr guarantees **At-Least-Once** semantics for message delivery.</span></span> <span data-ttu-id="a838d-127">发布消息后，该消息将至少传递到任何相关订户。</span><span class="sxs-lookup"><span data-stu-id="a838d-127">Once a message is published, it will be delivered at least once to any interested subscriber.</span></span>

 > <span data-ttu-id="a838d-128">如果服务只能处理一次消息，则需要提供 [幂等性检查](/azure/architecture/microservices/design/api-design#idempotent-operations) ，确保不会多次处理同一消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-128">If your service can only process a message once, you'll need to provide an [idempotency check](/azure/architecture/microservices/design/api-design#idempotent-operations) to ensure that the same message is not processed multiple times.</span></span> <span data-ttu-id="a838d-129">虽然这种逻辑可以编码，但某些消息代理（如 Azure 服务总线）提供内置的 *重复检测* 消息传递功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-129">While such logic can be coded, some message brokers, such as Azure Service Bus, provide built-in *duplicate detection* messaging capabilities.</span></span>

<span data-ttu-id="a838d-130">有多种可用的消息代理产品-商业和开源。</span><span class="sxs-lookup"><span data-stu-id="a838d-130">There are several message broker products available - both commercially and open-source.</span></span> <span data-ttu-id="a838d-131">每个都有优点和缺点。</span><span class="sxs-lookup"><span data-stu-id="a838d-131">Each has advantages and drawbacks.</span></span> <span data-ttu-id="a838d-132">你的工作是将系统要求与相应的 broker 匹配。</span><span class="sxs-lookup"><span data-stu-id="a838d-132">Your job is to match your system requirements to the appropriate broker.</span></span> <span data-ttu-id="a838d-133">选择后，最佳做法是将应用程序与消息代理程序分离。</span><span class="sxs-lookup"><span data-stu-id="a838d-133">Once selected, it's a best practice to decouple your application from message broker plumbing.</span></span> <span data-ttu-id="a838d-134">可以通过在 *抽象* 内包装代理来实现此功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-134">You achieve this functionality by wrapping the broker inside an *abstraction*.</span></span> <span data-ttu-id="a838d-135">抽象封装消息检测并向代码公开一般的发布/订阅操作。</span><span class="sxs-lookup"><span data-stu-id="a838d-135">The abstraction encapsulates the message plumbing and exposes generic pub/sub operations to your code.</span></span> <span data-ttu-id="a838d-136">你的代码与抽象而不是实际的消息代理进行通信。</span><span class="sxs-lookup"><span data-stu-id="a838d-136">Your code communicates with the abstraction, not the actual message broker.</span></span> <span data-ttu-id="a838d-137">在明智的决定下，你必须编写和维护抽象及其基础实现。</span><span class="sxs-lookup"><span data-stu-id="a838d-137">While a wise decision, you'll have to write and maintain the abstraction and its underlying implementation.</span></span> <span data-ttu-id="a838d-138">此方法需要自定义代码，这些代码可能会很复杂、重复性并且容易出错。</span><span class="sxs-lookup"><span data-stu-id="a838d-138">This approach requires custom code that can be complex, repetitive, and error-prone.</span></span>

<span data-ttu-id="a838d-139">Dapr publish & 订阅构建基块提供现成的消息传递抽象和实现。</span><span class="sxs-lookup"><span data-stu-id="a838d-139">The Dapr publish & subscribe building block provides the messaging abstraction and implementation out-of-the-box.</span></span> <span data-ttu-id="a838d-140">您必须编写的自定义代码在 Dapr 构建基块内预生成和封装。</span><span class="sxs-lookup"><span data-stu-id="a838d-140">The custom code you would have had to write is prebuilt and encapsulated inside the Dapr building block.</span></span> <span data-ttu-id="a838d-141">绑定到它并使用它。</span><span class="sxs-lookup"><span data-stu-id="a838d-141">You bind to it and consume it.</span></span> <span data-ttu-id="a838d-142">你和你的团队将重点放在创建可为客户带来价值的业务功能上，而不是编写消息传递管道代码。</span><span class="sxs-lookup"><span data-stu-id="a838d-142">Instead of writing messaging plumbing code, you and your team focus on creating business functionality that adds value to your customers.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="a838d-143">工作原理</span><span class="sxs-lookup"><span data-stu-id="a838d-143">How it works</span></span>

<span data-ttu-id="a838d-144">Dapr publish & 订阅构建基块提供了平台无关的 API 框架来发送和接收消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-144">The Dapr publish & subscribe building block provides a platform-agnostic API framework to send and receive messages.</span></span> <span data-ttu-id="a838d-145">你的服务将消息发布到一个命名 [主题](/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#topics-and-subscriptions)。</span><span class="sxs-lookup"><span data-stu-id="a838d-145">Your services publish messages to a named [topic](/azure/service-bus-messaging/service-bus-queues-topics-subscriptions#topics-and-subscriptions).</span></span> <span data-ttu-id="a838d-146">你的服务订阅主题来使用消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-146">Your services subscribe to a topic to consume messages.</span></span>

<span data-ttu-id="a838d-147">服务在 Dapr 挎斗上调用 pub/sub API。</span><span class="sxs-lookup"><span data-stu-id="a838d-147">The service calls the pub/sub API on the Dapr sidecar.</span></span> <span data-ttu-id="a838d-148">然后，挎斗将调用一个预定义的 Dapr pub/sub 组件来封装特定的消息代理产品。</span><span class="sxs-lookup"><span data-stu-id="a838d-148">The sidecar then makes calls into a pre-defined Dapr pub/sub component that encapsulates a specific message broker product.</span></span> <span data-ttu-id="a838d-149">图7-2 显示了 Dapr pub/sub 消息传递堆栈。</span><span class="sxs-lookup"><span data-stu-id="a838d-149">Figure 7-2 shows the Dapr pub/sub messaging stack.</span></span>

![Pub/sub 堆栈](./media/publish-subscribe/pub-sub-buildingblock.png)

<span data-ttu-id="a838d-151">**图 7-2**。</span><span class="sxs-lookup"><span data-stu-id="a838d-151">**Figure 7-2**.</span></span> <span data-ttu-id="a838d-152">Dapr pub/sub 堆栈。</span><span class="sxs-lookup"><span data-stu-id="a838d-152">The Dapr pub/sub stack.</span></span>

<span data-ttu-id="a838d-153">可以通过多种方式调用 Dapr publish & 订阅构建基块。</span><span class="sxs-lookup"><span data-stu-id="a838d-153">The Dapr publish & subscribe building block can be invoked in many ways.</span></span>

<span data-ttu-id="a838d-154">在最低级别，任何编程平台均可使用 **Dapr 本机 API** 通过 HTTP 或 gRPC 调用构建基块。</span><span class="sxs-lookup"><span data-stu-id="a838d-154">At the lowest level, any programming platform can invoke the building block over HTTP or gRPC using the **Dapr native API**.</span></span> <span data-ttu-id="a838d-155">若要发布消息，请执行以下 API 调用：</span><span class="sxs-lookup"><span data-stu-id="a838d-155">To publish a message, you make the following API call:</span></span>

``` http
http://localhost:<dapr-port>/v1.0/publish/<pub-sub-name>/<topic>
```

<span data-ttu-id="a838d-156">以上调用中有几个 Dapr 特定的 URL 段：</span><span class="sxs-lookup"><span data-stu-id="a838d-156">There are several Dapr specific URL segments in the above call:</span></span>

- <span data-ttu-id="a838d-157">`<dapr-port>` 提供 Dapr 挎斗正在侦听的端口号。</span><span class="sxs-lookup"><span data-stu-id="a838d-157">`<dapr-port>` provides the port number upon which the Dapr sidecar is listening.</span></span>
- <span data-ttu-id="a838d-158">`<pub-sub-name>` 提供选定的 Dapr pub/sub 组件的名称。</span><span class="sxs-lookup"><span data-stu-id="a838d-158">`<pub-sub-name>` provides the name of the selected Dapr pub/sub component.</span></span>
- <span data-ttu-id="a838d-159">`<topic>` 提供消息要发布到的主题的名称。</span><span class="sxs-lookup"><span data-stu-id="a838d-159">`<topic>` provides the name of the topic to which the message is published.</span></span>

<span data-ttu-id="a838d-160">使用 *卷曲* 的命令行工具发布消息，可以尝试一下：</span><span class="sxs-lookup"><span data-stu-id="a838d-160">Using the *curl* command-line tool to publish a message, you can try it out:</span></span>

``` curl
curl -X POST http://localhost:3500/v1.0/publish/pubsub/newOrder \
  -H "Content-Type: application/json" \
  -d '{ "orderId": "1234", "productId": "5678", "amount": 2 }'
```

<span data-ttu-id="a838d-161">通过订阅主题来接收消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-161">You receive messages by subscribing to a topic.</span></span> <span data-ttu-id="a838d-162">在启动时，Dapr 运行时将调用已知终结点上的应用程序来识别和创建所需的订阅：</span><span class="sxs-lookup"><span data-stu-id="a838d-162">At startup, the Dapr runtime will call the application on a well-known endpoint to identify and create the required subscriptions:</span></span>

``` http
http://localhost:<appPort>/dapr/subscribe
```

- <span data-ttu-id="a838d-163">`<appPort>` 通知 Dapr 挎斗应用程序侦听的端口。</span><span class="sxs-lookup"><span data-stu-id="a838d-163">`<appPort>` informs the Dapr sidecar of the port upon which the application is listening.</span></span>

<span data-ttu-id="a838d-164">可以自行实现此终结点。</span><span class="sxs-lookup"><span data-stu-id="a838d-164">You can implement this endpoint yourself.</span></span> <span data-ttu-id="a838d-165">但 Dapr 提供了更直观的实现方法。</span><span class="sxs-lookup"><span data-stu-id="a838d-165">But Dapr provides more intuitive ways of implementing it.</span></span> <span data-ttu-id="a838d-166">本章稍后将介绍此功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-166">We'll address this functionality later in this chapter.</span></span>

<span data-ttu-id="a838d-167">来自调用的响应包含应用程序将订阅的主题的列表。</span><span class="sxs-lookup"><span data-stu-id="a838d-167">The response from the call contains a list of topics to which the applications will subscribe.</span></span> <span data-ttu-id="a838d-168">每个都包括在主题收到消息时要调用的终结点。</span><span class="sxs-lookup"><span data-stu-id="a838d-168">Each includes an endpoint to call when the topic receives a message.</span></span> <span data-ttu-id="a838d-169">下面是响应的示例：</span><span class="sxs-lookup"><span data-stu-id="a838d-169">Here's an example of a response:</span></span>

```json
[
  {
    "pubsubname": "pubsub",
    "topic": "newOrder",
    "route": "/orders"
  },
  {
    "pubsubname": "pubsub",
    "topic": "newProduct",
    "route": "/productCatalog/products"
  }
]
```

<span data-ttu-id="a838d-170">在 JSON 响应中，可以看到应用程序要订阅主题 `newOrder` 和 `newProduct` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-170">In the JSON response, you can see the application wants to subscribe to topics `newOrder` and `newProduct`.</span></span> <span data-ttu-id="a838d-171">`/orders`分别为每个终结点注册终结点 `/productCatalog/products` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-171">It registers the endpoints `/orders` and `/productCatalog/products` for each, respectively.</span></span> <span data-ttu-id="a838d-172">对于这两个订阅，应用程序将绑定到名为的 Dapr 组件 `pubsub` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-172">For both subscriptions, the application is binding to the Dapr component named `pubsub`.</span></span>

<span data-ttu-id="a838d-173">图7-3 显示了该示例的流。</span><span class="sxs-lookup"><span data-stu-id="a838d-173">Figure 7-3 presents the flow of the example.</span></span>

![包含 Dapr 的发布/订阅示例](media/publish-subscribe/pub-sub-flow.png)

<span data-ttu-id="a838d-175">**图 7-3**。</span><span class="sxs-lookup"><span data-stu-id="a838d-175">**Figure 7-3**.</span></span> <span data-ttu-id="a838d-176">具有 Dapr 的发布/订阅流。</span><span class="sxs-lookup"><span data-stu-id="a838d-176">pub/sub flow with Dapr.</span></span>

<span data-ttu-id="a838d-177">在上图中，请注意以下 flow：</span><span class="sxs-lookup"><span data-stu-id="a838d-177">From the previous figure, note the flow:</span></span>

1. <span data-ttu-id="a838d-178">Dapr 挎斗 for Service B `/dapr/subscribe` 从服务 b 调用终结点， (使用者) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-178">The Dapr sidecar for Service B calls the `/dapr/subscribe` endpoint from Service B (the consumer).</span></span> <span data-ttu-id="a838d-179">服务会响应它要创建的订阅。</span><span class="sxs-lookup"><span data-stu-id="a838d-179">The service responds with the subscriptions it wants to create.</span></span>
1. <span data-ttu-id="a838d-180">服务 B 的 Dapr 挎斗在消息代理上创建请求的订阅。</span><span class="sxs-lookup"><span data-stu-id="a838d-180">The Dapr sidecar for Service B creates the requested subscriptions on the message broker.</span></span>
1. <span data-ttu-id="a838d-181">服务 A 在 `/v1.0/publish/<pub-sub-name>/<topic>` Dapr 服务的终结点上发布一条消息挎斗。</span><span class="sxs-lookup"><span data-stu-id="a838d-181">Service A publishes a message at the `/v1.0/publish/<pub-sub-name>/<topic>` endpoint on the Dapr Service A sidecar.</span></span>
1. <span data-ttu-id="a838d-182">服务 A 挎斗将消息发布到消息代理。</span><span class="sxs-lookup"><span data-stu-id="a838d-182">The Service A sidecar publishes the message to the message broker.</span></span>
1. <span data-ttu-id="a838d-183">消息代理将消息副本发送到服务 B 挎斗。</span><span class="sxs-lookup"><span data-stu-id="a838d-183">The message broker sends a copy of the message to the Service B sidecar.</span></span>
1. <span data-ttu-id="a838d-184">服务 B 挎斗调用对应于订阅 (的终结点，在这种情况下 `/orders`) 服务 b 上。服务使用 HTTP 状态代码进行响应 `200 OK` ，以便挎斗将消息视为已成功处理。</span><span class="sxs-lookup"><span data-stu-id="a838d-184">The Service B sidecar calls the endpoint corresponding to the subscription (in this case `/orders`) on Service B. The service responds with an HTTP status-code `200 OK` so the sidecar will consider the message as being handled successfully.</span></span>

<span data-ttu-id="a838d-185">在示例中，消息已成功处理。</span><span class="sxs-lookup"><span data-stu-id="a838d-185">In the example, the message is handled successfully.</span></span> <span data-ttu-id="a838d-186">但是，如果服务 B 处理请求时出现问题，则可以使用响应指定需要对消息执行的操作。</span><span class="sxs-lookup"><span data-stu-id="a838d-186">But if something goes wrong while Service B is handling the request, it can use the response to specify what needs to happen with the message.</span></span> <span data-ttu-id="a838d-187">当它返回 HTTP 状态代码时 `404` ，将记录错误并删除消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-187">When it returns an HTTP status-code `404`, an error is logged and the message is dropped.</span></span> <span data-ttu-id="a838d-188">对于任何其他状态代码 `200` `404` （或），将记录警告，然后重试消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-188">With any other status-code than `200` or `404`, a warning is logged and the message is retried.</span></span> <span data-ttu-id="a838d-189">或者，服务 B 可以通过在响应正文中包含 JSON 有效负载，显式指定需要对消息执行的操作。</span><span class="sxs-lookup"><span data-stu-id="a838d-189">Alternatively, Service B can explicitly specify what needs to happen with the message by including a JSON payload in the body of the response:</span></span>

```json
{
  "status": "<status>"
}
```

<span data-ttu-id="a838d-190">下表显示了可用的 `status` 值：</span><span class="sxs-lookup"><span data-stu-id="a838d-190">The following table shows the available `status` values:</span></span>

| <span data-ttu-id="a838d-191">状态</span><span class="sxs-lookup"><span data-stu-id="a838d-191">Status</span></span>           | <span data-ttu-id="a838d-192">操作</span><span class="sxs-lookup"><span data-stu-id="a838d-192">Action</span></span>                                                       |
| ---------------- | ------------------------------------------------------------ |
| <span data-ttu-id="a838d-193">成功</span><span class="sxs-lookup"><span data-stu-id="a838d-193">SUCCESS</span></span>          | <span data-ttu-id="a838d-194">消息被视为已成功处理和丢弃。</span><span class="sxs-lookup"><span data-stu-id="a838d-194">The message is considered as processed successfully and dropped.</span></span> |
| <span data-ttu-id="a838d-195">重试</span><span class="sxs-lookup"><span data-stu-id="a838d-195">RETRY</span></span>            | <span data-ttu-id="a838d-196">重试消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-196">The message is retried.</span></span>                                      |
| <span data-ttu-id="a838d-197">DROP</span><span class="sxs-lookup"><span data-stu-id="a838d-197">DROP</span></span>             | <span data-ttu-id="a838d-198">将记录警告，并删除消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-198">A warning is logged and the message is dropped.</span></span>              |
| <span data-ttu-id="a838d-199">任何其他状态</span><span class="sxs-lookup"><span data-stu-id="a838d-199">Any other status</span></span> | <span data-ttu-id="a838d-200">重试消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-200">The message is retried.</span></span>                                      |

### <a name="competing-consumers"></a><span data-ttu-id="a838d-201">竞争使用者</span><span class="sxs-lookup"><span data-stu-id="a838d-201">Competing consumers</span></span>

<span data-ttu-id="a838d-202">向外扩展订阅某个主题的应用程序时，必须处理竞争使用者。</span><span class="sxs-lookup"><span data-stu-id="a838d-202">When scaling out an application that subscribes to a topic, you have to deal with competing consumers.</span></span> <span data-ttu-id="a838d-203">只有一个应用程序实例应处理发送到主题的消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-203">Only one application instance should handle a message sent to the topic.</span></span> <span data-ttu-id="a838d-204">幸运的是，Dapr 处理这一问题。</span><span class="sxs-lookup"><span data-stu-id="a838d-204">Luckily, Dapr handles that problem.</span></span> <span data-ttu-id="a838d-205">当具有相同应用程序 id 的服务的多个实例订阅主题时，Dapr 仅将每条消息传递给其中的一个。</span><span class="sxs-lookup"><span data-stu-id="a838d-205">When multiple instances of a service with the same application-id subscribe to a topic, Dapr delivers each message to only one of them.</span></span>

### <a name="sdks"></a><span data-ttu-id="a838d-206">SDK</span><span class="sxs-lookup"><span data-stu-id="a838d-206">SDKs</span></span>

<span data-ttu-id="a838d-207">对本机 Dapr Api 进行 HTTP 调用非常耗时且更抽象。</span><span class="sxs-lookup"><span data-stu-id="a838d-207">Making HTTP calls to the native Dapr APIs is time-consuming and abstract.</span></span> <span data-ttu-id="a838d-208">你的调用是在 HTTP 级别进行定制的，你将需要处理诸如序列化和 HTTP 响应代码这样的管道问题。</span><span class="sxs-lookup"><span data-stu-id="a838d-208">Your calls are crafted at the HTTP level, and you'll need to handle plumbing concerns such as serialization and HTTP response codes.</span></span> <span data-ttu-id="a838d-209">幸运的是，有一种更直观的方式。</span><span class="sxs-lookup"><span data-stu-id="a838d-209">Fortunately, there's a more intuitive way.</span></span> <span data-ttu-id="a838d-210">Dapr 为常用开发平台提供多种语言特定的 Sdk。</span><span class="sxs-lookup"><span data-stu-id="a838d-210">Dapr provides several language-specific SDKs for popular development platforms.</span></span> <span data-ttu-id="a838d-211">撰写本文时，可以使用 Node.js、Python、.NET、Java 和 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="a838d-211">At the time of this writing, Go, Node.js, Python, .NET, Java, and JavaScript are available.</span></span>

## <a name="use-the-dapr-net-sdk"></a><span data-ttu-id="a838d-212">使用 Dapr .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a838d-212">Use the Dapr .NET SDK</span></span>

<span data-ttu-id="a838d-213">对于 .NET 开发人员而言， [Dapr .NET SDK](https://www.nuget.org/packages/Dapr.Client) 提供了更高效的 Dapr 处理方式。</span><span class="sxs-lookup"><span data-stu-id="a838d-213">For .NET Developers, the [Dapr .NET SDK](https://www.nuget.org/packages/Dapr.Client) provides a more productive way of working with Dapr.</span></span> <span data-ttu-id="a838d-214">SDK 公开了一个 `DaprClient` 类，通过该类可以直接调用 Dapr 功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-214">The SDK exposes a `DaprClient` class through which you can directly invoke Dapr functionality.</span></span> <span data-ttu-id="a838d-215">它直观且易于使用。</span><span class="sxs-lookup"><span data-stu-id="a838d-215">It's intuitive and easy to use.</span></span>

<span data-ttu-id="a838d-216">若要发布消息，会 `DaprClient` 公开一个 `PublishEventAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="a838d-216">To publish a message, the `DaprClient` exposes a `PublishEventAsync` method.</span></span>

```csharp
var data = new OrderData
{
  orderId = "123456",
  productId = "67890",
  amount = 2
};

var daprClient = new DaprClientBuilder().Build();

await daprClient.PublishEventAsync<OrderData>("pubsub", "newOrder", data);
```

- <span data-ttu-id="a838d-217">第一个参数 `pubsub` 是提供消息代理实现的 Dapr 组件的名称。</span><span class="sxs-lookup"><span data-stu-id="a838d-217">The first argument `pubsub` is the name of the Dapr component that provides the message broker implementation.</span></span> <span data-ttu-id="a838d-218">本章稍后将介绍这些组件。</span><span class="sxs-lookup"><span data-stu-id="a838d-218">We'll address components later in this chapter.</span></span>
- <span data-ttu-id="a838d-219">第二个参数 `neworder` 提供要向其发送消息的主题的名称。</span><span class="sxs-lookup"><span data-stu-id="a838d-219">The second argument `neworder` provides the name of the topic to send the message to.</span></span>
- <span data-ttu-id="a838d-220">第三个参数是消息的负载。</span><span class="sxs-lookup"><span data-stu-id="a838d-220">The third argument is the payload of the message.</span></span>
- <span data-ttu-id="a838d-221">您可以使用方法的泛型类型参数来指定消息的 .NET 类型。</span><span class="sxs-lookup"><span data-stu-id="a838d-221">You can specify the .NET type of the message using the generic type parameter of the method.</span></span>

<span data-ttu-id="a838d-222">若要接收消息，请将终结点绑定到已注册主题的订阅。</span><span class="sxs-lookup"><span data-stu-id="a838d-222">To receive messages, you bind an endpoint to a subscription for a registered topic.</span></span> <span data-ttu-id="a838d-223">用于 Dapr 的 AspNetCore 库使此功能变得简单。</span><span class="sxs-lookup"><span data-stu-id="a838d-223">The AspNetCore library for Dapr makes this trivial.</span></span> <span data-ttu-id="a838d-224">例如，假设你有一个名为的现有 ASP.NET WebAPI 操作方法 `CreateOrder` ：</span><span class="sxs-lookup"><span data-stu-id="a838d-224">Assume, for example, that you have an existing ASP.NET WebAPI action method entitled `CreateOrder`:</span></span>

```csharp
[HttpPost("/orders")]
public async Task<ActionResult> CreateOrder(Order order)
```

 > <span data-ttu-id="a838d-225">必须在项目中添加对 [Dapr](https://www.nuget.org/packages/Dapr.AspNetCore) NuGet 包的引用，才能使用 Dapr ASP.NET Core 集成。</span><span class="sxs-lookup"><span data-stu-id="a838d-225">You must add a reference to the [Dapr.AspNetCore](https://www.nuget.org/packages/Dapr.AspNetCore) NuGet package in your project to consume the Dapr ASP.NET Core integration.</span></span>

<span data-ttu-id="a838d-226">若要将此操作方法绑定到主题，请使用属性对其进行修饰 `Topic` ：</span><span class="sxs-lookup"><span data-stu-id="a838d-226">To bind this action method to a topic, you decorate it with the `Topic` attribute:</span></span>

```csharp
[Topic("pubsub", "newOrder")]
[HttpPost("/orders")]
public async Task<ActionResult> CreateOrder(Order order)
```

<span data-ttu-id="a838d-227">可以指定具有此属性的两个关键元素：</span><span class="sxs-lookup"><span data-stu-id="a838d-227">You specify two key elements with this attribute:</span></span>

- <span data-ttu-id="a838d-228">在这种情况下，要 (目标的 Dapr pub/sub 组件 `pubsub`) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-228">The Dapr pub/sub component to target (in this case `pubsub`).</span></span>
- <span data-ttu-id="a838d-229">在此示例中订阅 (的主题 `newOrder`) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-229">The topic to subscribe to (in this case `newOrder`).</span></span>

<span data-ttu-id="a838d-230">然后，Dapr 将调用该操作方法，因为它接收该主题的消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-230">Dapr then invokes that action method as it receives messages for that topic.</span></span>

<span data-ttu-id="a838d-231">还需要启用 ASP.NET Core 才能使用 Dapr。</span><span class="sxs-lookup"><span data-stu-id="a838d-231">You'll also need to enable ASP.NET Core to use Dapr.</span></span> <span data-ttu-id="a838d-232">Dapr .NET SDK 提供了可在类中调用的多个扩展方法 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-232">The Dapr .NET SDK provides several extension methods that can be invoked in the `Startup` class.</span></span>

<span data-ttu-id="a838d-233">在 `ConfigureServices` 方法中，必须添加以下扩展方法：</span><span class="sxs-lookup"><span data-stu-id="a838d-233">In the `ConfigureServices` method, you must add the following extension method:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddControllers().AddDapr();
}
```

<span data-ttu-id="a838d-234">将 `AddDapr` extension 方法追加到 `AddControllers` 扩展方法会注册必要的服务，以将 Dapr 集成到 MVC 管道中。</span><span class="sxs-lookup"><span data-stu-id="a838d-234">Appending the `AddDapr` extension-method to the `AddControllers` extension-method registers the necessary services to integrate Dapr into the MVC pipeline.</span></span> <span data-ttu-id="a838d-235">它还将 `DaprClient` 实例注册到依赖关系注入容器，然后可以将该容器注入服务中的任何位置。</span><span class="sxs-lookup"><span data-stu-id="a838d-235">It also registers a `DaprClient` instance into the dependency injection container, which then can be injected anywhere into your service.</span></span>

<span data-ttu-id="a838d-236">在 `Configure` 方法中，必须添加以下中间件组件来启用 Dapr：</span><span class="sxs-lookup"><span data-stu-id="a838d-236">In the `Configure` method, you must add the following middleware components to enable Dapr:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    // ...
    app.UseCloudEvents();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapSubscribeHandler();
        // ...
    });
}
```

<span data-ttu-id="a838d-237">用于 `UseCloudEvents` 将 **CloudEvents** 中间件添加到 ASP.NET Core 中间件管道的调用。</span><span class="sxs-lookup"><span data-stu-id="a838d-237">The call to `UseCloudEvents` adds **CloudEvents** middleware into to the ASP.NET Core middleware pipeline.</span></span> <span data-ttu-id="a838d-238">此中间件将解包使用 CloudEvents 结构化格式的请求，因此接收方法可以直接读取事件负载。</span><span class="sxs-lookup"><span data-stu-id="a838d-238">This middleware will unwrap requests that use the CloudEvents structured format, so the receiving method can read the event payload directly.</span></span>

> <span data-ttu-id="a838d-239">[CloudEvents](https://cloudevents.io/) 是一种标准化的消息传递格式，提供跨平台描述事件信息的常用方法。</span><span class="sxs-lookup"><span data-stu-id="a838d-239">[CloudEvents](https://cloudevents.io/) is a standardized messaging format, providing a common way to describe event information across platforms.</span></span> <span data-ttu-id="a838d-240">Dapr 的 CloudEvents。</span><span class="sxs-lookup"><span data-stu-id="a838d-240">Dapr embraces CloudEvents.</span></span> <span data-ttu-id="a838d-241">有关 CloudEvents 的详细信息，请参阅 [CloudEvents 规范](https://github.com/cloudevents/spec/tree/v1.0)。</span><span class="sxs-lookup"><span data-stu-id="a838d-241">For more information about CloudEvents, see the [cloudevents specification](https://github.com/cloudevents/spec/tree/v1.0).</span></span>

<span data-ttu-id="a838d-242">`MapSubscribeHandler`终结点路由配置中对的调用会将 Dapr 订阅终结点添加到应用程序。</span><span class="sxs-lookup"><span data-stu-id="a838d-242">The call to `MapSubscribeHandler` in the endpoint routing configuration will add a Dapr subscribe endpoint to the application.</span></span> <span data-ttu-id="a838d-243">此终结点将响应上的请求 `/dapr/subscribe` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-243">This endpoint will respond to requests on `/dapr/subscribe`.</span></span> <span data-ttu-id="a838d-244">调用此终结点时，它将自动查找用属性修饰的所有 WebAPI 操作方法， `Topic` 并指示 Dapr 为它们创建订阅。</span><span class="sxs-lookup"><span data-stu-id="a838d-244">When this endpoint is called, it will automatically find all WebAPI action methods decorated with the `Topic` attribute and instruct Dapr to create subscriptions for them.</span></span>

## <a name="pubsub-components"></a><span data-ttu-id="a838d-245">Pub/sub 组件</span><span class="sxs-lookup"><span data-stu-id="a838d-245">Pub/sub components</span></span>

<span data-ttu-id="a838d-246">Dapr [pub/sub 组件](https://github.com/dapr/components-contrib/tree/master/pubsub) 处理消息的实际传输。</span><span class="sxs-lookup"><span data-stu-id="a838d-246">Dapr [pub/sub components](https://github.com/dapr/components-contrib/tree/master/pubsub) handle the actual transport of the messages.</span></span> <span data-ttu-id="a838d-247">有多个可用。</span><span class="sxs-lookup"><span data-stu-id="a838d-247">Several are available.</span></span> <span data-ttu-id="a838d-248">每个都封装特定消息代理产品以实现发布/订阅功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-248">Each encapsulates a specific message broker product to implement the pub/sub functionality.</span></span> <span data-ttu-id="a838d-249">撰写本文时，可以使用以下发布/订阅组件：</span><span class="sxs-lookup"><span data-stu-id="a838d-249">At the time of writing, the following pub/sub components were available:</span></span>

- <span data-ttu-id="a838d-250">Apache Kafka</span><span class="sxs-lookup"><span data-stu-id="a838d-250">Apache Kafka</span></span>
- <span data-ttu-id="a838d-251">Azure 事件中心</span><span class="sxs-lookup"><span data-stu-id="a838d-251">Azure Event Hubs</span></span>
- <span data-ttu-id="a838d-252">Azure 服务总线</span><span class="sxs-lookup"><span data-stu-id="a838d-252">Azure Service Bus</span></span>
- <span data-ttu-id="a838d-253">AWS SNS/SQS</span><span class="sxs-lookup"><span data-stu-id="a838d-253">AWS SNS/SQS</span></span>
- <span data-ttu-id="a838d-254">GCP Pub/Sub</span><span class="sxs-lookup"><span data-stu-id="a838d-254">GCP Pub/Sub</span></span>
- <span data-ttu-id="a838d-255">Hazelcast</span><span class="sxs-lookup"><span data-stu-id="a838d-255">Hazelcast</span></span>
- <span data-ttu-id="a838d-256">MQTT</span><span class="sxs-lookup"><span data-stu-id="a838d-256">MQTT</span></span>
- <span data-ttu-id="a838d-257">NAT</span><span class="sxs-lookup"><span data-stu-id="a838d-257">NATS</span></span>
- <span data-ttu-id="a838d-258">Pulsar</span><span class="sxs-lookup"><span data-stu-id="a838d-258">Pulsar</span></span>
- <span data-ttu-id="a838d-259">RabbitMQ</span><span class="sxs-lookup"><span data-stu-id="a838d-259">RabbitMQ</span></span>
- <span data-ttu-id="a838d-260">Redis 流</span><span class="sxs-lookup"><span data-stu-id="a838d-260">Redis Streams</span></span>

> [!NOTE]
> <span data-ttu-id="a838d-261">Azure 云堆栈) 和事件流 (azure 事件中心) 可用性 (的消息传递功能。</span><span class="sxs-lookup"><span data-stu-id="a838d-261">The Azure cloud stack has both messaging functionality (Azure Service Bus) and event streaming (Azure Event Hub) availability.</span></span>

<span data-ttu-id="a838d-262">这些组件由 [GitHub 上的 contrib 存储库](https://github.com/dapr/components-contrib/tree/master/pubsub)中的社区创建。</span><span class="sxs-lookup"><span data-stu-id="a838d-262">These components are created by the community in a [component-contrib repository on GitHub](https://github.com/dapr/components-contrib/tree/master/pubsub).</span></span> <span data-ttu-id="a838d-263">建议为尚不受支持的消息代理编写自己的 Dapr 组件。</span><span class="sxs-lookup"><span data-stu-id="a838d-263">You're encouraged to write your own Dapr component for a message broker that isn't yet supported.</span></span>

### <a name="configure-pubsub-components"></a><span data-ttu-id="a838d-264">配置发布/订阅组件</span><span class="sxs-lookup"><span data-stu-id="a838d-264">Configure pub/sub components</span></span>

<span data-ttu-id="a838d-265">使用 Dapr 配置文件，您可以指定要使用)  (的发布/订阅组件。</span><span class="sxs-lookup"><span data-stu-id="a838d-265">Using a Dapr configuration file, you can specify the pub/sub component(s) to use.</span></span> <span data-ttu-id="a838d-266">此配置包含多个字段。</span><span class="sxs-lookup"><span data-stu-id="a838d-266">This configuration contains several fields.</span></span> <span data-ttu-id="a838d-267">`name`字段指定要使用的发布/订阅组件。</span><span class="sxs-lookup"><span data-stu-id="a838d-267">The `name` field specifies the pub/sub component that you want to use.</span></span> <span data-ttu-id="a838d-268">发送或接收消息时，需要指定此名称， (如之前在方法签名) 中看到的那样 `PublishEventAsync` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-268">When sending or receiving a message, you need to specify this name (as you saw earlier in the `PublishEventAsync` method signature).</span></span>

<span data-ttu-id="a838d-269">下面你将看到一个用于配置 RabbitMQ 消息代理组件的 Dapr 配置文件示例：</span><span class="sxs-lookup"><span data-stu-id="a838d-269">Below you see an example of a Dapr configuration file for configuring a RabbitMQ message broker component:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pubsub-rq
spec:
  type: pubsub.rabbitmq
  version: v1
  metadata:
  - name: host
    value: "amqp://localhost:5672"
  - name: durable
    value: true
```

<span data-ttu-id="a838d-270">在此示例中，可以看到，可以在块中指定任何特定于消息代理的配置 `metadata` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-270">In this example, you can see that you can specify any message broker-specific configuration in the `metadata` block.</span></span> <span data-ttu-id="a838d-271">在这种情况下，RabbitMQ 配置为创建持久队列。</span><span class="sxs-lookup"><span data-stu-id="a838d-271">In this case, RabbitMQ is configured to create durable queues.</span></span> <span data-ttu-id="a838d-272">但 RabbitMQ 组件具有更多的配置选项。</span><span class="sxs-lookup"><span data-stu-id="a838d-272">But the RabbitMQ component has more configuration options.</span></span> <span data-ttu-id="a838d-273">每个组件的配置将有自己的一组可能的字段。</span><span class="sxs-lookup"><span data-stu-id="a838d-273">Each of the components' configuration will have its own set of possible fields.</span></span> <span data-ttu-id="a838d-274">您可以阅读每个发布 [/订阅组件](https://docs.dapr.io/operations/components/setup-pubsub/supported-pubsub/)的文档中提供的字段。</span><span class="sxs-lookup"><span data-stu-id="a838d-274">You can read which fields are available in the documentation of each [pub/sub component](https://docs.dapr.io/operations/components/setup-pubsub/supported-pubsub/).</span></span>

<span data-ttu-id="a838d-275">在以编程方式从代码订阅主题时，Dapr pub/sub 还提供了一种声明方式来订阅主题。</span><span class="sxs-lookup"><span data-stu-id="a838d-275">Next to the programmatic way of subscribing to a topic from code, Dapr pub/sub also provides a declarative way of subscribing to a topic.</span></span> <span data-ttu-id="a838d-276">此方法会从应用程序代码中删除 Dapr 依赖项。</span><span class="sxs-lookup"><span data-stu-id="a838d-276">This approach removes the Dapr dependency from the application code.</span></span> <span data-ttu-id="a838d-277">因此，它还使现有应用程序能够订阅主题，而不会对代码进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="a838d-277">Therefore, it also enables an existing application to subscribe to topics without any changes to the code.</span></span> <span data-ttu-id="a838d-278">以下示例演示了用于配置订阅的 Dapr 配置文件：</span><span class="sxs-lookup"><span data-stu-id="a838d-278">The following example shows a Dapr configuration file for configuring a subscription:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Subscription
metadata:
  name: newOrder-subscription
spec:
  pubsubname: pubsub
  topic: newOrder
  route: /orders
scopes:
- ServiceB
- ServiceC
```

<span data-ttu-id="a838d-279">必须为每个订阅指定多个元素：</span><span class="sxs-lookup"><span data-stu-id="a838d-279">You have to specify several elements with every subscription:</span></span>

- <span data-ttu-id="a838d-280">在此情况下 (要使用的 Dapr pub/sub 组件的名称 `pubsub`) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-280">The name of the Dapr pub/sub component you want to use (in this case `pubsub`).</span></span>
- <span data-ttu-id="a838d-281">在此示例中订阅 (的主题的名称 `newOrder`) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-281">The name of the topic to subscribe to (in this case `newOrder`).</span></span>
- <span data-ttu-id="a838d-282">在此示例中，需要为本主题调用 API 操作 (`/orders`) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-282">The API operation that needs to be called for this topic (in this case `/orders`).</span></span>
- <span data-ttu-id="a838d-283">[作用域](https://docs.dapr.io/developing-applications/building-blocks/pubsub/pubsub-scopes/)可以指定哪些服务可以发布和订阅主题。</span><span class="sxs-lookup"><span data-stu-id="a838d-283">The [scope](https://docs.dapr.io/developing-applications/building-blocks/pubsub/pubsub-scopes/) can specify which services can publish and subscribe to a topic.</span></span>

## <a name="reference-application-eshopondapr"></a><span data-ttu-id="a838d-284">引用应用程序： eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="a838d-284">Reference application: eShopOnDapr</span></span>

<span data-ttu-id="a838d-285">随附的 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) 应用提供端到端参考体系结构，用于构造实现 Dapr 的微服务应用程序。</span><span class="sxs-lookup"><span data-stu-id="a838d-285">The accompanying [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) app provides an end-to-end reference architecture for constructing a microservices application implementing Dapr.</span></span> <span data-ttu-id="a838d-286">eShopOnDapr 是在几年前创建的广泛流行的 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) 应用程序的演变。</span><span class="sxs-lookup"><span data-stu-id="a838d-286">eShopOnDapr is an evolution of the widely popular [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) app, created several years ago.</span></span> <span data-ttu-id="a838d-287">这两个版本都使用发布/订阅模式来跨微服务传达 [集成事件](https://devblogs.microsoft.com/cesardelatorre/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/#integration-events) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-287">Both versions use the pub/sub pattern for communicating [integration events](https://devblogs.microsoft.com/cesardelatorre/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/#integration-events) across microservices.</span></span> <span data-ttu-id="a838d-288">集成事件包括：</span><span class="sxs-lookup"><span data-stu-id="a838d-288">Integration events include:</span></span>

- <span data-ttu-id="a838d-289">当用户签出购物篮时。</span><span class="sxs-lookup"><span data-stu-id="a838d-289">When a user checks-out a shopping basket.</span></span>
- <span data-ttu-id="a838d-290">订单的付款已成功。</span><span class="sxs-lookup"><span data-stu-id="a838d-290">When a payment for an order has succeeded.</span></span>
- <span data-ttu-id="a838d-291">在一段时间内，购买的宽限期已过期。</span><span class="sxs-lookup"><span data-stu-id="a838d-291">When the grace-period of a purchase has expired.</span></span>

<span data-ttu-id="a838d-292">EShopOnContainers 中的事件基于以下 `IEventBus` 接口：</span><span class="sxs-lookup"><span data-stu-id="a838d-292">Eventing in eShopOnContainers is based on the following `IEventBus` interface:</span></span>

```csharp
public interface IEventBus
{
    void Publish(IntegrationEvent integrationEvent);

    void Subscribe<T, THandler>()
        where TEvent : IntegrationEvent
        where THandler : IIntegrationEventHandler<T>;
}
```

<span data-ttu-id="a838d-293">此接口的具体实现在 RabbitMQ 和 Azure 服务总线的 eShopOnContainers 中存在。</span><span class="sxs-lookup"><span data-stu-id="a838d-293">Concrete implementations of this interface exist in eShopOnContainers for both RabbitMQ and Azure Service Bus.</span></span> <span data-ttu-id="a838d-294">每个实现都包含大量自定义的管道代码，这些代码非常复杂，难于理解和维护。</span><span class="sxs-lookup"><span data-stu-id="a838d-294">Each implementation included a great deal of custom plumbing code that was complex to understand and difficult to maintain.</span></span>

<span data-ttu-id="a838d-295">较新的 eShopOnDapr 通过使用 Dapr 大大简化了发布/订阅行为。</span><span class="sxs-lookup"><span data-stu-id="a838d-295">The newer eShopOnDapr significantly simplifies pub/sub behavior by using Dapr.</span></span> <span data-ttu-id="a838d-296">例如， `IEventBus` 接口被缩减为单一方法：</span><span class="sxs-lookup"><span data-stu-id="a838d-296">For example, the `IEventBus` interface was reduced to a single method:</span></span>

```csharp
public interface IEventBus
{
    Task PublishAsync(IntegrationEvent integrationEvent);
}
```

### <a name="publish-events"></a><span data-ttu-id="a838d-297">发布事件</span><span class="sxs-lookup"><span data-stu-id="a838d-297">Publish events</span></span>

<span data-ttu-id="a838d-298">在更新的 eShopOnDapr 中，一个 `DaprEventBus` 实现可以支持任何 Dapr 支持的消息代理。</span><span class="sxs-lookup"><span data-stu-id="a838d-298">In the updated eShopOnDapr, a single `DaprEventBus` implementation can support any Dapr-supported message broker.</span></span> <span data-ttu-id="a838d-299">下面的代码块显示了简化的发布方法。</span><span class="sxs-lookup"><span data-stu-id="a838d-299">The following code block shows the simplified Publish method.</span></span> <span data-ttu-id="a838d-300">请注意该 `PublishAsync` 方法如何使用 Dapr 客户端来发布事件：</span><span class="sxs-lookup"><span data-stu-id="a838d-300">Note how the `PublishAsync` method uses the Dapr client to publish an event:</span></span>

```csharp
public class DaprEventBus : IEventBus
{
    private const string PubSubName = "pubsub";

    private readonly DaprClient _daprClient;
    private readonly ILogger<DaprEventBus> _logger;

    public DaprEventBus(DaprClient daprClient, ILogger<DaprEventBus> logger)
    {
        _daprClient = daprClient ?? throw new ArgumentNullException(nameof(daprClient));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));
    }

    public async Task PublishAsync(IntegrationEvent integrationEvent)
    {
        var topicName = integrationEvent.GetType().Name;

        // Dapr uses System.Text.Json which does not support serialization of a
        // polymorphic type hierarchy by default. Using object as the type
        // parameter causes all properties to be serialized.
        await _daprClient.PublishEventAsync<object>(PubSubName, topicName, integrationEvent);
    }
}
```

<span data-ttu-id="a838d-301">如代码片段中所示，主题名称是从事件类型的名称派生的。</span><span class="sxs-lookup"><span data-stu-id="a838d-301">As you can see in the code snippet, the topic name is derived from event type's name.</span></span> <span data-ttu-id="a838d-302">因为所有 eShop services 都使用 `IEventBus` 抽象，所以现场修改 Dapr 需要 *完全不更改* 主线应用程序代码。</span><span class="sxs-lookup"><span data-stu-id="a838d-302">Because all eShop services use the `IEventBus` abstraction, retrofitting Dapr required *absolutely no change* to the mainline application code.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a838d-303">Dapr SDK 使用 `System.Text.Json` 来序列化/反序列化消息。</span><span class="sxs-lookup"><span data-stu-id="a838d-303">The Dapr SDK uses `System.Text.Json` to serialize/deserialize messages.</span></span> <span data-ttu-id="a838d-304">但是， `System.Text.Json` 默认情况下不会序列化派生类的属性。</span><span class="sxs-lookup"><span data-stu-id="a838d-304">However, `System.Text.Json` doesn't serialize properties of derived classes by default.</span></span> <span data-ttu-id="a838d-305">在 eShop 代码中，事件有时显式声明为 `IntegrationEvent` （集成事件的基类）。</span><span class="sxs-lookup"><span data-stu-id="a838d-305">In the eShop code, an event is sometimes explicitly declared as an `IntegrationEvent`, the base class for integration events.</span></span> <span data-ttu-id="a838d-306">这是因为具体事件类型是在运行时基于业务逻辑动态确定的。</span><span class="sxs-lookup"><span data-stu-id="a838d-306">This is done because the concrete event type is determined dynamically at run time based on business logic.</span></span> <span data-ttu-id="a838d-307">因此，使用基类而不是派生类的类型信息对事件进行序列化。</span><span class="sxs-lookup"><span data-stu-id="a838d-307">As a result, the event is serialized using the type information of the base class and not the derived class.</span></span> <span data-ttu-id="a838d-308">若要 `System.Text.Json` 在这种情况下强制序列化派生类的所有属性，代码使用 `object` 作为泛型类型参数。</span><span class="sxs-lookup"><span data-stu-id="a838d-308">To force `System.Text.Json` to serialize all properties of the derived class in this case, the code uses `object` as the generic type parameter.</span></span> <span data-ttu-id="a838d-309">有关详细信息，请参阅 [.net 文档](../../standard/serialization/system-text-json-polymorphism.md)。</span><span class="sxs-lookup"><span data-stu-id="a838d-309">For more information, see the [.NET documentation](../../standard/serialization/system-text-json-polymorphism.md).</span></span>

<span data-ttu-id="a838d-310">借助 Dapr，可 **大大简化** 基础结构代码。</span><span class="sxs-lookup"><span data-stu-id="a838d-310">With Dapr, the infrastructure code is **dramatically simplified**.</span></span> <span data-ttu-id="a838d-311">不需要区分不同的消息代理。</span><span class="sxs-lookup"><span data-stu-id="a838d-311">It doesn't need to distinguish between the different message brokers.</span></span> <span data-ttu-id="a838d-312">Dapr 为你提供此抽象。</span><span class="sxs-lookup"><span data-stu-id="a838d-312">Dapr provides this abstraction for you.</span></span> <span data-ttu-id="a838d-313">如果需要，可以轻松地交换消息代理或配置多个消息代理组件。</span><span class="sxs-lookup"><span data-stu-id="a838d-313">And if needed, you can easily swap out message brokers or configure multiple message broker components.</span></span>

### <a name="subscribe-to-events"></a><span data-ttu-id="a838d-314">订阅事件</span><span class="sxs-lookup"><span data-stu-id="a838d-314">Subscribe to events</span></span>

<span data-ttu-id="a838d-315">前面的 eShopOnContainers 应用程序包含 *SubscriptionManagers* 来处理每个消息代理的订阅实现。</span><span class="sxs-lookup"><span data-stu-id="a838d-315">The earlier eShopOnContainers app contains *SubscriptionManagers* to handle the subscription implementation for each message broker.</span></span> <span data-ttu-id="a838d-316">每个管理器都包含用于处理订阅事件的复杂消息代理特定代码。</span><span class="sxs-lookup"><span data-stu-id="a838d-316">Each manager contains complex message broker-specific code for handling subscription events.</span></span> <span data-ttu-id="a838d-317">若要接收事件，每个服务必须为每个事件类型显式注册一个处理程序。</span><span class="sxs-lookup"><span data-stu-id="a838d-317">To receive events, each service has to explicitly register a handler for each event-type.</span></span>

<span data-ttu-id="a838d-318">eShopOnDapr 使用 Dapr ASP.NET Core 库优化了事件订阅的管道。</span><span class="sxs-lookup"><span data-stu-id="a838d-318">eShopOnDapr streamlines the plumbing for event subscriptions by using Dapr ASP.NET Core libraries.</span></span> <span data-ttu-id="a838d-319">每个事件都由控制器中的操作方法处理。</span><span class="sxs-lookup"><span data-stu-id="a838d-319">Each event is handled by an action method in the controller.</span></span> <span data-ttu-id="a838d-320">`Topic`特性使用要订阅的相应主题的名称修饰操作方法。</span><span class="sxs-lookup"><span data-stu-id="a838d-320">A `Topic` attribute decorates the action method with the name of the corresponding topic to subscribe to.</span></span> <span data-ttu-id="a838d-321">下面是从中获取的代码片段 `PaymentService` ：</span><span class="sxs-lookup"><span data-stu-id="a838d-321">Here's a code snippet taken from the `PaymentService`:</span></span>

```csharp
[Route("api/v1/[controller]")]
[ApiController]
public class IntegrationEventController : ControllerBase
{
    private const string DAPR_PUBSUB_NAME = "pubsub";

    private readonly IServiceProvider _serviceProvider;

    public IntegrationEventController(IServiceProvider serviceProvider)
    {
        _serviceProvider = serviceProvider;
    }

    [HttpPost("OrderStatusChangedToValidated")]
    [Topic(DAPR_PUBSUB_NAME, "OrderStatusChangedToValidatedIntegrationEvent")]
    public async Task OrderStarted(OrderStatusChangedToValidatedIntegrationEvent integrationEvent)
    {
        var handler = _serviceProvider.GetRequiredService<OrderStatusChangedToValidatedIntegrationEventHandler>();
        await handler.Handle(integrationEvent);
    }
}
```

<span data-ttu-id="a838d-322">在 `Topic` 特性中，事件的 .net 类型的名称将用作主题名称。</span><span class="sxs-lookup"><span data-stu-id="a838d-322">In the `Topic` attribute, the name of the .NET type of the event is used as the topic name.</span></span> <span data-ttu-id="a838d-323">在处理事件时，将调用以前的 eShopOnContainers 代码库中已经存在的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="a838d-323">For handling the event, an event handler that already existed in the earlier eShopOnContainers code base is invoked.</span></span> <span data-ttu-id="a838d-324">在上面的示例中，从主题接收的消息 `OrderStatusChangedToValidatedIntegrationEvent` 调用了现有的 `OrderStatusChangedToValidatedIntegrationEventHandler` 事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="a838d-324">In the previous example, messages received from the `OrderStatusChangedToValidatedIntegrationEvent` topic invoke the existing `OrderStatusChangedToValidatedIntegrationEventHandler` event-handler.</span></span> <span data-ttu-id="a838d-325">由于 Dapr 实现了订阅和消息代理的基础管道，因此大量原始代码已过时，并已从代码库中删除。</span><span class="sxs-lookup"><span data-stu-id="a838d-325">Because Dapr implements the underlying plumbing for subscriptions and message brokers, a large amount of original code became obsolete and was removed from the code-base.</span></span> <span data-ttu-id="a838d-326">很多代码对于理解和维护非常复杂。</span><span class="sxs-lookup"><span data-stu-id="a838d-326">Much of this code was complex to understand and challenging to maintain.</span></span>

### <a name="use-pubsub-components"></a><span data-ttu-id="a838d-327">使用发布/订阅组件</span><span class="sxs-lookup"><span data-stu-id="a838d-327">Use pub/sub components</span></span>

<span data-ttu-id="a838d-328">在 eShopOnDapr 存储库中， `deployment` 文件夹包含使用不同部署模式部署应用程序的文件： `Docker Compose` 和 `Kubernetes` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-328">Within the eShopOnDapr repository, a `deployment` folder contains files for deploying the application using different deployment modes: `Docker Compose` and `Kubernetes`.</span></span> <span data-ttu-id="a838d-329">`dapr`包含文件夹的每个文件夹中都存在一个文件夹 `components` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-329">A `dapr` folder exists within each of these folders that holds a `components` folder.</span></span> <span data-ttu-id="a838d-330">此文件夹包含一个文件，该文件 `eshop-pubsub.yaml` 包含应用程序将用于发布/订阅行为的 Dapr pub/sub 组件的配置。</span><span class="sxs-lookup"><span data-stu-id="a838d-330">This folder holds a file `eshop-pubsub.yaml` containing the configuration of the Dapr pub/sub component that the application will use for pub/sub behavior.</span></span> <span data-ttu-id="a838d-331">正如您在前面的代码片段中所看到的那样，所使用的 pub/sub 组件的名称为 `pubsub` 。</span><span class="sxs-lookup"><span data-stu-id="a838d-331">As you saw in the earlier code snippets, the name of the pub/sub component used is `pubsub`.</span></span> <span data-ttu-id="a838d-332">下面是 `eshop-pubsub.yaml` 文件夹中文件的内容 `deployment/compose/dapr/components` ：</span><span class="sxs-lookup"><span data-stu-id="a838d-332">Here's the content of the `eshop-pubsub.yaml` file in the `deployment/compose/dapr/components` folder:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pubsub
  namespace: default
spec:
  type: pubsub.nats
  version: v1
  metadata:
  - name: natsURL
    value: nats://demo.nats.io:4222
```

<span data-ttu-id="a838d-333">前面的配置指定此示例所需的 [nat 消息代理](https://nats.io/) 。</span><span class="sxs-lookup"><span data-stu-id="a838d-333">The preceding configuration specifies the desired [NATS message broker](https://nats.io/) for this example.</span></span> <span data-ttu-id="a838d-334">若要更改消息代理，只需配置一个不同的消息代理，如 RabbitMQ 或 Azure 服务总线，并更新 yaml 文件。</span><span class="sxs-lookup"><span data-stu-id="a838d-334">To change message brokers, you need only to configure a different message broker, such as RabbitMQ or Azure Service Bus and update the yaml file.</span></span> <span data-ttu-id="a838d-335">对于 Dapr，切换消息代理时，不会对主线服务代码进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="a838d-335">With Dapr, there are no changes to your mainline service code when switching message brokers.</span></span>

<span data-ttu-id="a838d-336">最后，您可能会问： "为什么需要在一个应用程序中使用多个消息代理？"。</span><span class="sxs-lookup"><span data-stu-id="a838d-336">Finally, you might ask, "Why would I need multiple message brokers in an application?".</span></span> <span data-ttu-id="a838d-337">很多时候系统将处理具有不同特征的工作负荷。</span><span class="sxs-lookup"><span data-stu-id="a838d-337">Many times a system will handle workloads with different characteristics.</span></span> <span data-ttu-id="a838d-338">一个事件可能一天发生10次，但另一个事件每秒发生5000次。</span><span class="sxs-lookup"><span data-stu-id="a838d-338">One event may occur 10 times a day, but another event occurs 5,000 times per second.</span></span> <span data-ttu-id="a838d-339">可以通过将消息传送流量分区给不同消息代理来受益。</span><span class="sxs-lookup"><span data-stu-id="a838d-339">You may benefit by partitioning messaging traffic to different message brokers.</span></span> <span data-ttu-id="a838d-340">使用 Dapr，可以添加多个 pub/sub 组件配置，每个配置都有不同的名称。</span><span class="sxs-lookup"><span data-stu-id="a838d-340">With Dapr, you can add multiple pub/sub component configurations, each with a different name.</span></span>

## <a name="summary"></a><span data-ttu-id="a838d-341">摘要</span><span class="sxs-lookup"><span data-stu-id="a838d-341">Summary</span></span>

<span data-ttu-id="a838d-342">Pub/sub 模式可帮助你分离分布式应用程序中的服务。</span><span class="sxs-lookup"><span data-stu-id="a838d-342">The pub/sub pattern helps you decouple services in a distributed application.</span></span> <span data-ttu-id="a838d-343">Dapr publish & 订阅构建基块可简化在应用程序中实现此行为。</span><span class="sxs-lookup"><span data-stu-id="a838d-343">The Dapr publish & subscribe building block simplifies implementing this behavior in your application.</span></span>

<span data-ttu-id="a838d-344">通过 Dapr pub/sub，你可以将消息发布到特定 *主题*。</span><span class="sxs-lookup"><span data-stu-id="a838d-344">Through Dapr pub/sub, you can publish messages to a specific *topic*.</span></span> <span data-ttu-id="a838d-345">同时，构建基块将查询你的服务，以确定) 订阅 (的主题。</span><span class="sxs-lookup"><span data-stu-id="a838d-345">As well, the building block will query your service to determine which topic(s) to subscribe to.</span></span>

<span data-ttu-id="a838d-346">你可以通过 HTTP 或使用特定于语言的 Sdk 之一（如 .NET SDK for Dapr）使用 Dapr pub/sub。</span><span class="sxs-lookup"><span data-stu-id="a838d-346">You can use Dapr pub/sub natively over HTTP or by using one of the language-specific SDKs, such as the .NET SDK for Dapr.</span></span> <span data-ttu-id="a838d-347">.NET SDK 与 ASP.NET core 平台紧密集成。</span><span class="sxs-lookup"><span data-stu-id="a838d-347">The .NET SDK tightly integrates with the ASP.NET core platform.</span></span>

<span data-ttu-id="a838d-348">使用 Dapr，可以将受支持的消息代理产品插入应用程序。</span><span class="sxs-lookup"><span data-stu-id="a838d-348">With Dapr, you can plug a supported message broker product into your application.</span></span> <span data-ttu-id="a838d-349">然后，你可以在无需对应用程序进行代码更改的情况下交换消息代理。</span><span class="sxs-lookup"><span data-stu-id="a838d-349">You can then swap message brokers without requiring code changes to your application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a838d-350">[上一页](service-invocation.md)
> [下一页](bindings.md)</span><span class="sxs-lookup"><span data-stu-id="a838d-350">[Previous](service-invocation.md)
[Next](bindings.md)</span></span>
