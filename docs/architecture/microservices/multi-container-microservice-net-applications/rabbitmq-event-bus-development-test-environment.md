---
title: 使用 RabbitMQ 实现用于开发或测试环境的事件总线
description: 容器化 .NET 应用程序的 .NET 微服务架构 | 使用 RabbitMQ 实现用于开发或测试环境的集成事件的事件总线消息传递。
ms.date: 01/13/2021
ms.openlocfilehash: b67b6cf92ac2c29b9eff07c2c9603206e42968a3
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102258073"
---
# <a name="implementing-an-event-bus-with-rabbitmq-for-the-development-or-test-environment"></a><span data-ttu-id="ab070-103">使用 RabbitMQ 实现用于开发或测试环境的事件总线</span><span class="sxs-lookup"><span data-stu-id="ab070-103">Implementing an event bus with RabbitMQ for the development or test environment</span></span>

<span data-ttu-id="ab070-104">首先应假设基于在容器中运行的 RabbitMQ 创建自定义事件总线，正如 eShopOnContainers 应用程序一样，它应仅用于你的开发和测试环境。</span><span class="sxs-lookup"><span data-stu-id="ab070-104">We should start by saying that if you create your custom event bus based on RabbitMQ running in a container, as the eShopOnContainers application does, it should be used only for your development and test environments.</span></span> <span data-ttu-id="ab070-105">不要将其用于生产环境，除非你要将其构建为生产就绪服务总线的一部分。</span><span class="sxs-lookup"><span data-stu-id="ab070-105">Don't use it for your production environment, unless you are building it as a part of a production-ready service bus.</span></span> <span data-ttu-id="ab070-106">简单的自定义事件总线可能缺少商业服务总线具有的许多生产就绪关键功能。</span><span class="sxs-lookup"><span data-stu-id="ab070-106">A simple custom event bus might be missing many production-ready critical features that a commercial service bus has.</span></span>

<span data-ttu-id="ab070-107">eShopOnContainers 中的事件总线自定义实现之一基本上是一个使用 RabbitMQ API 的库。</span><span class="sxs-lookup"><span data-stu-id="ab070-107">One of the event bus custom implementations in eShopOnContainers is basically a library using the RabbitMQ API.</span></span> <span data-ttu-id="ab070-108">（还有另一个基于 Azure 服务总线的实现）。</span><span class="sxs-lookup"><span data-stu-id="ab070-108">(There's another implementation based on Azure Service Bus.)</span></span>

<span data-ttu-id="ab070-109">借助 RabbitMQ 的事件总线实现，微服务可订阅事件、发布事件和接收事件，如图 6-21 所示。</span><span class="sxs-lookup"><span data-stu-id="ab070-109">The event bus implementation with RabbitMQ lets microservices subscribe to events, publish events, and receive events, as shown in Figure 6-21.</span></span>

![显示消息发送方和消息接收方之间的 RabbitMQ 的关系图。](./media/rabbitmq-event-bus-development-test-environment/rabbitmq-implementation.png)

<span data-ttu-id="ab070-111">**图 6-21。**</span><span class="sxs-lookup"><span data-stu-id="ab070-111">**Figure 6-21.**</span></span> <span data-ttu-id="ab070-112">事件总线的 RabbitMQ 实现</span><span class="sxs-lookup"><span data-stu-id="ab070-112">RabbitMQ implementation of an event bus</span></span>

<span data-ttu-id="ab070-113">RabbitMQ 充当消息发布服务器和订阅者之间的中介，处理分发。</span><span class="sxs-lookup"><span data-stu-id="ab070-113">RabbitMQ functions as an intermediary between message publisher and subscribers, to handle distribution.</span></span> <span data-ttu-id="ab070-114">在代码中，EventBusRabbitMQ 类实现了泛型 IEventBus 接口。</span><span class="sxs-lookup"><span data-stu-id="ab070-114">In the code, the EventBusRabbitMQ class implements the generic IEventBus interface.</span></span> <span data-ttu-id="ab070-115">此实现基于依赖项注入，以便可以从此开发/测试版本交换到生产版本。</span><span class="sxs-lookup"><span data-stu-id="ab070-115">This implementation is based on Dependency Injection so that you can swap from this dev/test version to a production version.</span></span>

```csharp
public class EventBusRabbitMQ : IEventBus, IDisposable
{
    // Implementation using RabbitMQ API
    //...
}
```

<span data-ttu-id="ab070-116">示例开发/测试事件总线的 RabbitMQ 实现是样板代码。</span><span class="sxs-lookup"><span data-stu-id="ab070-116">The RabbitMQ implementation of a sample dev/test event bus is boilerplate code.</span></span> <span data-ttu-id="ab070-117">它必须处理与 RabbitMQ 服务器的连接，并提供用于将消息事件发布到队列的代码。</span><span class="sxs-lookup"><span data-stu-id="ab070-117">It has to handle the connection to the RabbitMQ server and provide code for publishing a message event to the queues.</span></span> <span data-ttu-id="ab070-118">它还必须为每个事件类型实现收集集成事件处理程序的字典；这些事件类型可以对每个接收器微服务具有不同的实例化和不同的订阅，如图 6-21 所示。</span><span class="sxs-lookup"><span data-stu-id="ab070-118">It also has to implement a dictionary of collections of integration event handlers for each event type; these event types can have a different instantiation and different subscriptions for each receiver microservice, as shown in Figure 6-21.</span></span>

## <a name="implementing-a-simple-publish-method-with-rabbitmq"></a><span data-ttu-id="ab070-119">使用 RabbitMQ 实现一个简单的发布方法</span><span class="sxs-lookup"><span data-stu-id="ab070-119">Implementing a simple publish method with RabbitMQ</span></span>

<span data-ttu-id="ab070-120">下面的代码是 RabbitMQ 的事件总线实现的简化版，用以展示整个方案。</span><span class="sxs-lookup"><span data-stu-id="ab070-120">The following code is a ***simplified*** version of an event bus implementation for RabbitMQ, to showcase the whole scenario.</span></span> <span data-ttu-id="ab070-121">你真的不必以这种方式处理连接。</span><span class="sxs-lookup"><span data-stu-id="ab070-121">You don't really handle the connection this way.</span></span> <span data-ttu-id="ab070-122">要查看完整的实现，请参阅 [dotnet-architecture/eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs) 存储库中的实际代码。</span><span class="sxs-lookup"><span data-stu-id="ab070-122">To see the full implementation, see the actual code in the [dotnet-architecture/eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs) repository.</span></span>

```csharp
public class EventBusRabbitMQ : IEventBus, IDisposable
{
    // Member objects and other methods ...
    // ...

    public void Publish(IntegrationEvent @event)
    {
        var eventName = @event.GetType().Name;
        var factory = new ConnectionFactory() { HostName = _connectionString };
        using (var connection = factory.CreateConnection())
        using (var channel = connection.CreateModel())
        {
            channel.ExchangeDeclare(exchange: _brokerName,
                type: "direct");
            string message = JsonConvert.SerializeObject(@event);
            var body = Encoding.UTF8.GetBytes(message);
            channel.BasicPublish(exchange: _brokerName,
                routingKey: eventName,
                basicProperties: null,
                body: body);
       }
    }
}
```

<span data-ttu-id="ab070-123">eShopOnContainers 应用程序中发布方法的[实际代码](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs)通过使用 [Polly](https://github.com/App-vNext/Polly) 重试策略得到了改进，如果 RabbitMQ 容器尚未就绪，该策略会多次重试该任务。</span><span class="sxs-lookup"><span data-stu-id="ab070-123">The [actual code](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/BuildingBlocks/EventBus/EventBusRabbitMQ/EventBusRabbitMQ.cs) of the Publish method in the eShopOnContainers application is improved by using a [Polly](https://github.com/App-vNext/Polly) retry policy, which retries the task some times in case the RabbitMQ container is not ready.</span></span> <span data-ttu-id="ab070-124">在 docker-compose 启动容器时可能出现此情况；例如，RabbitMQ 容器的启动速度可能比其他容器慢。</span><span class="sxs-lookup"><span data-stu-id="ab070-124">This scenario can occur when docker-compose is starting the containers; for example, the RabbitMQ container might start more slowly than the other containers.</span></span>

<span data-ttu-id="ab070-125">如前面所述，RabbitMQ 中有许多可能的配置，因此此代码应仅用于开发/测试环境。</span><span class="sxs-lookup"><span data-stu-id="ab070-125">As mentioned earlier, there are many possible configurations in RabbitMQ, so this code should be used only for dev/test environments.</span></span>

## <a name="implementing-the-subscription-code-with-the-rabbitmq-api"></a><span data-ttu-id="ab070-126">使用 RabbitMQ API 实现订阅代码</span><span class="sxs-lookup"><span data-stu-id="ab070-126">Implementing the subscription code with the RabbitMQ API</span></span>

<span data-ttu-id="ab070-127">与发布代码一样，下面的代码是 RabbitMQ 事件总线实现的简化部分。</span><span class="sxs-lookup"><span data-stu-id="ab070-127">As with the publish code, the following code is a simplification of part of the event bus implementation for RabbitMQ.</span></span> <span data-ttu-id="ab070-128">同样，除非要进行改进，否则通常不需要对其进行编码。</span><span class="sxs-lookup"><span data-stu-id="ab070-128">Again, you usually do not need to change it unless you are improving it.</span></span>

```csharp
public class EventBusRabbitMQ : IEventBus, IDisposable
{
    // Member objects and other methods ...
    // ...

    public void Subscribe<T, TH>()
        where T : IntegrationEvent
        where TH : IIntegrationEventHandler<T>
    {
        var eventName = _subsManager.GetEventKey<T>();

        var containsKey = _subsManager.HasSubscriptionsForEvent(eventName);
        if (!containsKey)
        {
            if (!_persistentConnection.IsConnected)
            {
                _persistentConnection.TryConnect();
            }

            using (var channel = _persistentConnection.CreateModel())
            {
                channel.QueueBind(queue: _queueName,
                                    exchange: BROKER_NAME,
                                    routingKey: eventName);
            }
        }

        _subsManager.AddSubscription<T, TH>();
    }
}
```

<span data-ttu-id="ab070-129">每个事件类型都有一个相关的通道，以获取 RabbitMQ 中的事件。</span><span class="sxs-lookup"><span data-stu-id="ab070-129">Each event type has a related channel to get events from RabbitMQ.</span></span> <span data-ttu-id="ab070-130">然后，可以根据需要在每个通道和事件类型中拥有尽可能多的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="ab070-130">You can then have as many event handlers per channel and event type as needed.</span></span>

<span data-ttu-id="ab070-131">订阅方法接受一个 IIntegrationEventHandler 对象，该对象相当于当前微服务中的回调方法，以及其相关的 IntegrationEvent 对象。</span><span class="sxs-lookup"><span data-stu-id="ab070-131">The Subscribe method accepts an IIntegrationEventHandler object, which is like a callback method in the current microservice, plus its related IntegrationEvent object.</span></span> <span data-ttu-id="ab070-132">然后，代码将该事件处理程序添加到事件处理程序列表，每个客户端微服务的每个集成事件类型都可具有事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="ab070-132">The code then adds that event handler to the list of event handlers that each integration event type can have per client microservice.</span></span> <span data-ttu-id="ab070-133">如果客户端代码尚未订阅事件，该代码将为事件类型创建一个通道，以便在从任何其他服务中发布事件时，它可以从 RabbitMQ 以推送方式接收事件。</span><span class="sxs-lookup"><span data-stu-id="ab070-133">If the client code has not already been subscribed to the event, the code creates a channel for the event type so it can receive events in a push style from RabbitMQ when that event is published from any other service.</span></span>

<span data-ttu-id="ab070-134">如上所述，在 eShopOnContainers 中实现的事件总线仅用于教育目的，因为它只处理主要场景，尚不可用于生产环境。</span><span class="sxs-lookup"><span data-stu-id="ab070-134">As mentioned above, the event bus implemented in eShopOnContainers has only and educational purpose, since it only handles the main scenarios, so it's not ready for production.</span></span>

<span data-ttu-id="ab070-135">对于生产场景，请查看下面 RabbitMQ 特定的附加资源，以及[在微服务器之间实现基于事件的通信](./integration-event-based-microservice-communications.md#additional-resources)部分。</span><span class="sxs-lookup"><span data-stu-id="ab070-135">For production scenarios check the additional resources below, specific for RabbitMQ, and the [Implementing event-based communication between microservices](./integration-event-based-microservice-communications.md#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab070-136">其他资源</span><span class="sxs-lookup"><span data-stu-id="ab070-136">Additional resources</span></span>

<span data-ttu-id="ab070-137">支持 RabbitMQ 的生产就绪型解决方案。</span><span class="sxs-lookup"><span data-stu-id="ab070-137">A production-ready solution with support for RabbitMQ.</span></span>

- <span data-ttu-id="ab070-138">**EasyNetQ** - 为 RabbitMQ 打开源 .NET API 客户端 </span><span class="sxs-lookup"><span data-stu-id="ab070-138">**EasyNetQ** - Open Source .NET API client for RabbitMQ </span></span>\
  <https://easynetq.com/>

- <span data-ttu-id="ab070-139">**MassTransit** </span><span class="sxs-lookup"><span data-stu-id="ab070-139">**MassTransit** </span></span>\
  <https://masstransit-project.com/>
  
- <span data-ttu-id="ab070-140">**Rebus** - 开源 .NET 服务总线<https://github.com/rebus-org/Rebus></span><span class="sxs-lookup"><span data-stu-id="ab070-140">**Rebus** - Open source .NET Service Bus <https://github.com/rebus-org/Rebus></span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ab070-141">[上一页](integration-event-based-microservice-communications.md)
> [下一页](subscribe-events.md)</span><span class="sxs-lookup"><span data-stu-id="ab070-141">[Previous](integration-event-based-microservice-communications.md)
[Next](subscribe-events.md)</span></span>
