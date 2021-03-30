---
title: 订阅事件
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解发布和订阅集成事件的详细信息。
ms.date: 01/13/2021
ms.openlocfilehash: d7b68f5c01c21564724e3a00a048a9fa58bb4fb6
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637368"
---
# <a name="subscribing-to-events"></a><span data-ttu-id="674b5-103">订阅事件</span><span class="sxs-lookup"><span data-stu-id="674b5-103">Subscribing to events</span></span>

<span data-ttu-id="674b5-104">使用事件总线的第一步是为微服务订阅它们想要接收的事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-104">The first step for using the event bus is to subscribe the microservices to the events they want to receive.</span></span> <span data-ttu-id="674b5-105">此功能应在接收者微服务中完成。</span><span class="sxs-lookup"><span data-stu-id="674b5-105">That functionality should be done in the receiver microservices.</span></span>

<span data-ttu-id="674b5-106">下面的简单代码展示了每个接收者微服务在启动时（即，在 `Startup` 类中）必须实现哪些内容，才能订阅所需的事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-106">The following simple code shows what each receiver microservice needs to implement when starting the service (that is, in the `Startup` class) so it subscribes to the events it needs.</span></span> <span data-ttu-id="674b5-107">在此示例中，`basket-api` 微服务需要订阅 `ProductPriceChangedIntegrationEvent` 和 `OrderStartedIntegrationEvent` 消息。</span><span class="sxs-lookup"><span data-stu-id="674b5-107">In this case, the `basket-api` microservice needs to subscribe to `ProductPriceChangedIntegrationEvent` and the `OrderStartedIntegrationEvent` messages.</span></span>

<span data-ttu-id="674b5-108">例如，通过订阅 `ProductPriceChangedIntegrationEvent` 事件，购物车微服务可了解产品价格的任何变化，并在用户购物车中有该产品时向该用户发出价格变动警告。</span><span class="sxs-lookup"><span data-stu-id="674b5-108">For instance, when subscribing to the `ProductPriceChangedIntegrationEvent` event, that makes the basket microservice aware of any changes to the product price and lets it warn the user about the change if that product is in the user's basket.</span></span>

```csharp
var eventBus = app.ApplicationServices.GetRequiredService<IEventBus>();

eventBus.Subscribe<ProductPriceChangedIntegrationEvent,
                   ProductPriceChangedIntegrationEventHandler>();

eventBus.Subscribe<OrderStartedIntegrationEvent,
                   OrderStartedIntegrationEventHandler>();

```

<span data-ttu-id="674b5-109">此代码运行后，订阅者微服务将通过 RabbitMQ 通道进行侦听。</span><span class="sxs-lookup"><span data-stu-id="674b5-109">After this code runs, the subscriber microservice will be listening through RabbitMQ channels.</span></span> <span data-ttu-id="674b5-110">当 ProductPriceChangedIntegrationEvent 类型的任何消息到达时，该代码会调用传递给它的事件处理程序并处理该事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-110">When any message of type ProductPriceChangedIntegrationEvent arrives, the code invokes the event handler that is passed to it and processes the event.</span></span>

## <a name="publishing-events-through-the-event-bus"></a><span data-ttu-id="674b5-111">通过事件总线发布事件</span><span class="sxs-lookup"><span data-stu-id="674b5-111">Publishing events through the event bus</span></span>

<span data-ttu-id="674b5-112">最后，消息发送者（源微服务）使用类似于以下示例的代码发布集成事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-112">Finally, the message sender (origin microservice) publishes the integration events with code similar to the following example.</span></span> <span data-ttu-id="674b5-113">（此方法是一个简化的示例，不考虑原子性。）每当必须跨多个微服务传播事件时（通常就在从源微服务提交数据或事务之后），都会实现类似的代码。</span><span class="sxs-lookup"><span data-stu-id="674b5-113">(This approach is a simplified example that does not take atomicity into account.) You would implement similar code whenever an event must be propagated across multiple microservices, usually right after committing data or transactions from the origin microservice.</span></span>

<span data-ttu-id="674b5-114">首先，在控制器构造函数中注入事件总线实现对象（基于 RabbitMQ 或基于服务总线），如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="674b5-114">First, the event bus implementation object (based on RabbitMQ or based on a service bus) would be injected at the controller constructor, as in the following code:</span></span>

```csharp
[Route("api/v1/[controller]")]
public class CatalogController : ControllerBase
{
    private readonly CatalogContext _context;
    private readonly IOptionsSnapshot<Settings> _settings;
    private readonly IEventBus _eventBus;

    public CatalogController(CatalogContext context,
        IOptionsSnapshot<Settings> settings,
        IEventBus eventBus)
    {
        _context = context;
        _settings = settings;
        _eventBus = eventBus;
    }
    // ...
}
```

<span data-ttu-id="674b5-115">然后，从控制器的方法中使用它，例如在 UpdateProduct 方法中：</span><span class="sxs-lookup"><span data-stu-id="674b5-115">Then you use it from your controller's methods, like in the UpdateProduct method:</span></span>

```csharp
[Route("items")]
[HttpPost]
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem product)
{
    var item = await _context.CatalogItems.SingleOrDefaultAsync(
        i => i.Id == product.Id);
    // ...
    if (item.Price != product.Price)
    {
        var oldPrice = item.Price;
        item.Price = product.Price;
        _context.CatalogItems.Update(item);
        var @event = new ProductPriceChangedIntegrationEvent(item.Id,
            item.Price,
            oldPrice);
        // Commit changes in original transaction
        await _context.SaveChangesAsync();
        // Publish integration event to the event bus
        // (RabbitMQ or a service bus underneath)
        _eventBus.Publish(@event);
        // ...
    }
    // ...
}
```

<span data-ttu-id="674b5-116">在此示例中，源微服务是一个简单的 CRUD 微服务，因此，该代码直接放置在 Web API 控制器中。</span><span class="sxs-lookup"><span data-stu-id="674b5-116">In this case, since the origin microservice is a simple CRUD microservice, that code is placed right into a Web API controller.</span></span>

<span data-ttu-id="674b5-117">在更高级的微服务中，比如使用 CQRS 方法时，可以在 `CommandHandler` 类的 `Handle()` 方法中实现。</span><span class="sxs-lookup"><span data-stu-id="674b5-117">In more advanced microservices, like when using CQRS approaches, it can be implemented in the `CommandHandler` class, within the `Handle()` method.</span></span>

### <a name="designing-atomicity-and-resiliency-when-publishing-to-the-event-bus"></a><span data-ttu-id="674b5-118">设计发布到事件总线时的原子性和复原能力</span><span class="sxs-lookup"><span data-stu-id="674b5-118">Designing atomicity and resiliency when publishing to the event bus</span></span>

<span data-ttu-id="674b5-119">如果通过分布式消息传递系统（如事件总线）发布集成事件，在以原子方式更新原始数据库和发布事件时会出现问题（即，两个操作都已完成，或者都没有完成）。</span><span class="sxs-lookup"><span data-stu-id="674b5-119">When you publish integration events through a distributed messaging system like your event bus, you have the problem of atomically updating the original database and publishing an event (that is, either both operations complete or none of them).</span></span> <span data-ttu-id="674b5-120">例如，在前面所示的简化示例中，代码在产品价格发生变动时向数据库提交数据，然后发布 ProductPriceChangedIntegrationEvent 消息。</span><span class="sxs-lookup"><span data-stu-id="674b5-120">For instance, in the simplified example shown earlier, the code commits data to the database when the product price is changed and then publishes a ProductPriceChangedIntegrationEvent message.</span></span> <span data-ttu-id="674b5-121">最开始，这两个操作看起来可能必须以原子方式执行。</span><span class="sxs-lookup"><span data-stu-id="674b5-121">Initially, it might look essential that these two operations be performed atomically.</span></span> <span data-ttu-id="674b5-122">但是，如果像在 [Microsoft 消息队列 (MSMQ)](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)) 等早期系统中那样，使用涉及数据库和消息代理的分布式事务，那么根据 [CAP 定理](https://www.quora.com/What-Is-CAP-Theorem-1)所描述的原因，不建议使用此方法。</span><span class="sxs-lookup"><span data-stu-id="674b5-122">However, if you are using a distributed transaction involving the database and the message broker, as you do in older systems like [Microsoft Message Queuing (MSMQ)](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)), this approach is not recommended for the reasons described by the [CAP theorem](https://www.quora.com/What-Is-CAP-Theorem-1).</span></span>

<span data-ttu-id="674b5-123">微服务主要用于构建可缩放且高度可用的系统。</span><span class="sxs-lookup"><span data-stu-id="674b5-123">Basically, you use microservices to build scalable and highly available systems.</span></span> <span data-ttu-id="674b5-124">简单来说，CAP 定理认为你无法构建一个兼具  持续可用性、强一致性和分区容错性的（分布式）数据库（或拥有其模型的微服务）。</span><span class="sxs-lookup"><span data-stu-id="674b5-124">Simplifying somewhat, the CAP theorem says that you cannot build a (distributed) database (or a microservice that owns its model) that's continually available, strongly consistent, *and* tolerant to any partition.</span></span> <span data-ttu-id="674b5-125">你只能选择这三种属性中的两种。</span><span class="sxs-lookup"><span data-stu-id="674b5-125">You must choose two of these three properties.</span></span>

<span data-ttu-id="674b5-126">在基于微服务的体系结构中，应选择可用性和容错性，而不再强调强一致性。</span><span class="sxs-lookup"><span data-stu-id="674b5-126">In microservices-based architectures, you should choose availability and tolerance, and you should de-emphasize strong consistency.</span></span> <span data-ttu-id="674b5-127">因此，在大多数基于微服务的现代应用程序中，你通常不希望像在使用 [MSMQ](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)) 基于 Windows 分布式事务处理协调器 (DTC) 实现[分布式事务](/previous-versions/windows/desktop/ms681205(v=vs.85))时那样，在消息传递中使用分布式事务。</span><span class="sxs-lookup"><span data-stu-id="674b5-127">Therefore, in most modern microservice-based applications, you usually do not want to use distributed transactions in messaging, as you do when you implement [distributed transactions](/previous-versions/windows/desktop/ms681205(v=vs.85)) based on the Windows Distributed Transaction Coordinator (DTC) with [MSMQ](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)).</span></span>

<span data-ttu-id="674b5-128">让我们回到最初的问题及其示例。</span><span class="sxs-lookup"><span data-stu-id="674b5-128">Let's go back to the initial issue and its example.</span></span> <span data-ttu-id="674b5-129">如果服务在数据库更新之后（在本例中，也就是在运行 `_context.SaveChangesAsync()` 代码行之后）但在发布集成事件之前崩溃，则整个系统会变得不一致。</span><span class="sxs-lookup"><span data-stu-id="674b5-129">If the service crashes after the database is updated (in this case, right after the line of code with `_context.SaveChangesAsync()`), but before the integration event is published, the overall system could become inconsistent.</span></span> <span data-ttu-id="674b5-130">此方法对业务而言可能很关键，具体取决于你正在处理的具体业务操作。</span><span class="sxs-lookup"><span data-stu-id="674b5-130">This approach might be business critical, depending on the specific business operation you are dealing with.</span></span>

<span data-ttu-id="674b5-131">正如前面在体系结构部分中提到的那样，可以通过以下几种方法来处理此问题：</span><span class="sxs-lookup"><span data-stu-id="674b5-131">As mentioned earlier in the architecture section, you can have several approaches for dealing with this issue:</span></span>

- <span data-ttu-id="674b5-132">使用完整[事件溯源模式](/azure/architecture/patterns/event-sourcing)。</span><span class="sxs-lookup"><span data-stu-id="674b5-132">Using the full [Event Sourcing pattern](/azure/architecture/patterns/event-sourcing).</span></span>

- <span data-ttu-id="674b5-133">使用事务日志挖掘。</span><span class="sxs-lookup"><span data-stu-id="674b5-133">Using transaction log mining.</span></span>

- <span data-ttu-id="674b5-134">使用[发件箱模式](https://www.kamilgrzybek.com/design/the-outbox-pattern/)。</span><span class="sxs-lookup"><span data-stu-id="674b5-134">Using the [Outbox pattern](https://www.kamilgrzybek.com/design/the-outbox-pattern/).</span></span> <span data-ttu-id="674b5-135">这是用于存储集成事件（扩展本地事务）的事务表。</span><span class="sxs-lookup"><span data-stu-id="674b5-135">This is a transactional table to store the integration events (extending the local transaction).</span></span>

<span data-ttu-id="674b5-136">在本案例中，使用完整事件溯源 (ES) 模式即使算不上 *最好* 的办法，也是最好的办法之一。</span><span class="sxs-lookup"><span data-stu-id="674b5-136">For this scenario, using the full Event Sourcing (ES) pattern is one of the best approaches, if not *the* best.</span></span> <span data-ttu-id="674b5-137">但是，在许多应用程序案例中，你可能无法实现完整的 ES 系统。</span><span class="sxs-lookup"><span data-stu-id="674b5-137">However, in many application scenarios, you might not be able to implement a full ES system.</span></span> <span data-ttu-id="674b5-138">ES 意味着仅在事务数据库中存储域事件，而不存储当前状态数据。</span><span class="sxs-lookup"><span data-stu-id="674b5-138">ES means storing only domain events in your transactional database, instead of storing current state data.</span></span> <span data-ttu-id="674b5-139">仅存储域事件可以带来很大的好处，例如可以获得系统的历史记录，并且能够确定系统在过去任意时间点的状态。</span><span class="sxs-lookup"><span data-stu-id="674b5-139">Storing only domain events can have great benefits, such as having the history of your system available and being able to determine the state of your system at any moment in the past.</span></span> <span data-ttu-id="674b5-140">但是，实现完整的 ES 系统需要重新构建大部分系统，并引入许多其他复杂性和要求。</span><span class="sxs-lookup"><span data-stu-id="674b5-140">However, implementing a full ES system requires you to rearchitect most of your system and introduces many other complexities and requirements.</span></span> <span data-ttu-id="674b5-141">例如，你希望使用专用于事件溯源的数据库（例如 [Event Store](https://eventstore.org/)）或面向文档的数据库（例如 Azure Cosmos DB、MongoDB、Cassandra、CouchDB 或 RavenDB）。</span><span class="sxs-lookup"><span data-stu-id="674b5-141">For example, you would want to use a database specifically made for event sourcing, such as [Event Store](https://eventstore.org/), or a document-oriented database such as Azure Cosmos DB, MongoDB, Cassandra, CouchDB, or RavenDB.</span></span> <span data-ttu-id="674b5-142">对于此问题，ES 是一种很好的解决方案，但不是最简单的解决方案，除非你已经熟悉事件溯源。</span><span class="sxs-lookup"><span data-stu-id="674b5-142">ES is a great approach for this problem, but not the easiest solution unless you are already familiar with event sourcing.</span></span>

<span data-ttu-id="674b5-143">使用事务日志挖掘的方法最初看起来很透明。</span><span class="sxs-lookup"><span data-stu-id="674b5-143">The option to use transaction log mining initially looks transparent.</span></span> <span data-ttu-id="674b5-144">但是，若要使用此方法，微服务必须与 RDBMS 事务日志（例如 SQL Server 事务日志）耦合。</span><span class="sxs-lookup"><span data-stu-id="674b5-144">However, to use this approach, the microservice has to be coupled to your RDBMS transaction log, such as the SQL Server transaction log.</span></span> <span data-ttu-id="674b5-145">此方法可能是不可取的。</span><span class="sxs-lookup"><span data-stu-id="674b5-145">This approach is probably not desirable.</span></span> <span data-ttu-id="674b5-146">还有一个缺点就是，事务日志中记录的低级别更新可能与高级别集成事件不在同一级别。</span><span class="sxs-lookup"><span data-stu-id="674b5-146">Another drawback is that the low-level updates recorded in the transaction log might not be at the same level as your high-level integration events.</span></span> <span data-ttu-id="674b5-147">如果是这样，可能难以对这些事务日志操作执行反向工程操作。</span><span class="sxs-lookup"><span data-stu-id="674b5-147">If so, the process of reverse-engineering those transaction log operations can be difficult.</span></span>

<span data-ttu-id="674b5-148">较平衡的一种方法是混合使用事务数据库表和简化的 ES 模式。</span><span class="sxs-lookup"><span data-stu-id="674b5-148">A balanced approach is a mix of a transactional database table and a simplified ES pattern.</span></span> <span data-ttu-id="674b5-149">可以使用诸如“准备发布事件”之类的状态，在将原始事件提交到集成事件表时，可以在原始事件中设置该状态。</span><span class="sxs-lookup"><span data-stu-id="674b5-149">You can use a state such as "ready to publish the event," which you set in the original event when you commit it to the integration events table.</span></span> <span data-ttu-id="674b5-150">然后尝试将事件发布到事件总线。</span><span class="sxs-lookup"><span data-stu-id="674b5-150">You then try to publish the event to the event bus.</span></span> <span data-ttu-id="674b5-151">如果 publish-event 操作成功，则在源服务中启动另一个事务，并将状态从“准备发布事件”更改为“已发布事件”。</span><span class="sxs-lookup"><span data-stu-id="674b5-151">If the publish-event action succeeds, you start another transaction in the origin service and move the state from "ready to publish the event" to "event already published."</span></span>

<span data-ttu-id="674b5-152">如果事件总线中的 publish-event 操作失败，则数据在源微服务中仍保持一致，它仍被标记为“准备发布事件”，并且对于其他服务，它将实现最终一致性。</span><span class="sxs-lookup"><span data-stu-id="674b5-152">If the publish-event action in the event bus fails, the data still will not be inconsistent within the origin microservice—it is still marked as "ready to publish the event," and with respect to the rest of the services, it will eventually be consistent.</span></span> <span data-ttu-id="674b5-153">你始终可以通过后台作业来检查事务或集成事件的状态。</span><span class="sxs-lookup"><span data-stu-id="674b5-153">You can always have background jobs checking the state of the transactions or integration events.</span></span> <span data-ttu-id="674b5-154">如果作业找到状态为“准备发布事件”的事件，它可以尝试将该事件重新发布到事件总线。</span><span class="sxs-lookup"><span data-stu-id="674b5-154">If the job finds an event in the "ready to publish the event" state, it can try to republish that event to the event bus.</span></span>

<span data-ttu-id="674b5-155">请注意，使用此方法时，只需保留每个源微服务的集成事件，以及要传播到其他微服务或外部系统的事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-155">Notice that with this approach, you are persisting only the integration events for each origin microservice, and only the events that you want to communicate to other microservices or external systems.</span></span> <span data-ttu-id="674b5-156">而在完整的 ES 系统中，还需要存储所有域事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-156">In contrast, in a full ES system, you store all domain events as well.</span></span>

<span data-ttu-id="674b5-157">因此，这种平衡方法其实就是一个简化的 ES 系统。</span><span class="sxs-lookup"><span data-stu-id="674b5-157">Therefore, this balanced approach is a simplified ES system.</span></span> <span data-ttu-id="674b5-158">你需要一个包含集成事件及其当前状态（“准备发布”与“已发布”）的列表。</span><span class="sxs-lookup"><span data-stu-id="674b5-158">You need a list of integration events with their current state ("ready to publish" versus "published").</span></span> <span data-ttu-id="674b5-159">但你只需为集成事件实现这些状态。</span><span class="sxs-lookup"><span data-stu-id="674b5-159">But you only need to implement these states for the integration events.</span></span> <span data-ttu-id="674b5-160">在此方法中，无需像在完整的 ES 系统中那样，将所有域数据作为事件存储在事务数据库中。</span><span class="sxs-lookup"><span data-stu-id="674b5-160">And in this approach, you do not need to store all your domain data as events in the transactional database, as you would in a full ES system.</span></span>

<span data-ttu-id="674b5-161">如果已经在使用关系数据库，则可以使用事务表来存储集成事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-161">If you are already using a relational database, you can use a transactional table to store integration events.</span></span> <span data-ttu-id="674b5-162">若要在应用程序中实现原子性，可以使用基于本地事务的过程，该过程包含两个步骤。</span><span class="sxs-lookup"><span data-stu-id="674b5-162">To achieve atomicity in your application, you use a two-step process based on local transactions.</span></span> <span data-ttu-id="674b5-163">本质上来说，就是在域实体所在的数据库中创建一个 IntegrationEvent 表。</span><span class="sxs-lookup"><span data-stu-id="674b5-163">Basically, you have an IntegrationEvent table in the same database where you have your domain entities.</span></span> <span data-ttu-id="674b5-164">该表用于保证实现原子性，以便将持久性集成事件添加到提交域数据的相同事务中。</span><span class="sxs-lookup"><span data-stu-id="674b5-164">That table works as an insurance for achieving atomicity so that you include persisted integration events into the same transactions that are committing your domain data.</span></span>

<span data-ttu-id="674b5-165">此进程的分步操作如下：：</span><span class="sxs-lookup"><span data-stu-id="674b5-165">Step by step, the process goes like this:</span></span>

1. <span data-ttu-id="674b5-166">应用程序开始处理一项本地数据库事务。</span><span class="sxs-lookup"><span data-stu-id="674b5-166">The application begins a local database transaction.</span></span>

2. <span data-ttu-id="674b5-167">然后更新域实体的状态，并向集成事件表中插入一个事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-167">It then updates the state of your domain entities and inserts an event into the integration event table.</span></span>

3. <span data-ttu-id="674b5-168">最后，它将提交事务，如此即可获得所需的原子性，然后</span><span class="sxs-lookup"><span data-stu-id="674b5-168">Finally, it commits the transaction, so you get the desired atomicity and then</span></span>

4. <span data-ttu-id="674b5-169">以某种方式发布事件（下一步）。</span><span class="sxs-lookup"><span data-stu-id="674b5-169">You publish the event somehow (next).</span></span>

<span data-ttu-id="674b5-170">在实施事件发布步骤时，你有以下选择：</span><span class="sxs-lookup"><span data-stu-id="674b5-170">When implementing the steps of publishing the events, you have these choices:</span></span>

- <span data-ttu-id="674b5-171">在提交事务后立即发布集成事件，并使用另一个本地事务将表中的事件标记为已发布。</span><span class="sxs-lookup"><span data-stu-id="674b5-171">Publish the integration event right after committing the transaction and use another local transaction to mark the events in the table as being published.</span></span> <span data-ttu-id="674b5-172">然后，将该表用作一个项目，以便在远程微服务出现问题时跟踪集成事件，并根据存储的集成事件执行补偿操作。</span><span class="sxs-lookup"><span data-stu-id="674b5-172">Then, use the table just as an artifact to track the integration events in case of issues in the remote microservices, and perform compensatory actions based on the stored integration events.</span></span>

- <span data-ttu-id="674b5-173">将该表用作一种队列。</span><span class="sxs-lookup"><span data-stu-id="674b5-173">Use the table as a kind of queue.</span></span> <span data-ttu-id="674b5-174">单独的应用程序线程或进程查询集成事件表，将事件发布到事件总线，然后使用本地事务将事件标记为已发布。</span><span class="sxs-lookup"><span data-stu-id="674b5-174">A separate application thread or process queries the integration event table, publishes the events to the event bus, and then uses a local transaction to mark the events as published.</span></span>

<span data-ttu-id="674b5-175">图 6-22 展示了第一种方法的体系结构。</span><span class="sxs-lookup"><span data-stu-id="674b5-175">Figure 6-22 shows the architecture for the first of these approaches.</span></span>

![不使用辅助角色微服务发布时的原子性关系图。](./media/subscribe-events/atomicity-publish-event-bus.png)

<span data-ttu-id="674b5-177">图 6-22  。</span><span class="sxs-lookup"><span data-stu-id="674b5-177">**Figure 6-22**.</span></span> <span data-ttu-id="674b5-178">将事件发布到事件总线时的原子性</span><span class="sxs-lookup"><span data-stu-id="674b5-178">Atomicity when publishing events to the event bus</span></span>

<span data-ttu-id="674b5-179">图 6-22 所示的方法缺少一个附加的辅助微服务，它负责检查和确认已发布的集成事件成功与否。</span><span class="sxs-lookup"><span data-stu-id="674b5-179">The approach illustrated in Figure 6-22 is missing an additional worker microservice that is in charge of checking and confirming the success of the published integration events.</span></span> <span data-ttu-id="674b5-180">如果失败，该附加检查器辅助微服务可以从表中读取事件并重新发布它们，即重复步骤 2。</span><span class="sxs-lookup"><span data-stu-id="674b5-180">In case of failure, that additional checker worker microservice can read events from the table and republish them, that is, repeat step number 2.</span></span>

<span data-ttu-id="674b5-181">关于第二种方法：将 EventLog 表用作队列，并始终使用辅助微服务来发布消息。</span><span class="sxs-lookup"><span data-stu-id="674b5-181">About the second approach: you use the EventLog table as a queue and always use a worker microservice to publish the messages.</span></span> <span data-ttu-id="674b5-182">在这种情况下，其过程如图 6-23 所示。</span><span class="sxs-lookup"><span data-stu-id="674b5-182">In that case, the process is like that shown in Figure 6-23.</span></span> <span data-ttu-id="674b5-183">图中显示了一个附加的微服务，并且该表作为发布事件时的单一事件源。</span><span class="sxs-lookup"><span data-stu-id="674b5-183">This shows an additional microservice, and the table is the single source when publishing events.</span></span>

![使用辅助角色微服务发布时的原子性关系图。](./media/subscribe-events/atomicity-publish-worker-microservice.png)

<span data-ttu-id="674b5-185">图 6-23  。</span><span class="sxs-lookup"><span data-stu-id="674b5-185">**Figure 6-23**.</span></span> <span data-ttu-id="674b5-186">使用辅助微服务将事件发布到事件总线时的原子性</span><span class="sxs-lookup"><span data-stu-id="674b5-186">Atomicity when publishing events to the event bus with a worker microservice</span></span>

<span data-ttu-id="674b5-187">为简单起见，eShopOnContainers 示例使用第一种方法（没有附加进程或检查器微服务）和事件总线。</span><span class="sxs-lookup"><span data-stu-id="674b5-187">For simplicity, the eShopOnContainers sample uses the first approach (with no additional processes or checker microservices) plus the event bus.</span></span> <span data-ttu-id="674b5-188">但 eShopOnContainers 示例并未处理所有可能的故障情况。</span><span class="sxs-lookup"><span data-stu-id="674b5-188">However, the eShopOnContainers sample is not handling all possible failure cases.</span></span> <span data-ttu-id="674b5-189">在部署到云端的实际应用程序中，你必须接受最终会出现问题的事实，并且必须实施该检查并实现重新发送逻辑。</span><span class="sxs-lookup"><span data-stu-id="674b5-189">In a real application deployed to the cloud, you must embrace the fact that issues will arise eventually, and you must implement that check and resend logic.</span></span> <span data-ttu-id="674b5-190">如果在通过事件总线发布事件（使用辅助角色）时，该表是单一的事件源，那么，将该表用作队列可能比第一种方法更有效。</span><span class="sxs-lookup"><span data-stu-id="674b5-190">Using the table as a queue can be more effective than the first approach if you have that table as a single source of events when publishing them (with the worker) through the event bus.</span></span>

### <a name="implementing-atomicity-when-publishing-integration-events-through-the-event-bus"></a><span data-ttu-id="674b5-191">通过事件总线发布集成事件时实现原子性</span><span class="sxs-lookup"><span data-stu-id="674b5-191">Implementing atomicity when publishing integration events through the event bus</span></span>

<span data-ttu-id="674b5-192">以下代码展示如何创建涉及多个 DbContext 对象的单一事务：一个上下文与要更新的原始数据相关，另一个上下文与 IntegrationEventLog 表相关。</span><span class="sxs-lookup"><span data-stu-id="674b5-192">The following code shows how you can create a single transaction involving multiple DbContext objects—one context related to the original data being updated, and the second context related to the IntegrationEventLog table.</span></span>

<span data-ttu-id="674b5-193">如果在代码运行时，数据库连接出现任何问题，以下示例代码中的事务将无法复原。</span><span class="sxs-lookup"><span data-stu-id="674b5-193">The transaction in the example code below will not be resilient if connections to the database have any issue at the time when the code is running.</span></span> <span data-ttu-id="674b5-194">在可能跨服务器移动数据库的基于云的系统（例如 Azure SQL DB）中，可能会出现这种问题。</span><span class="sxs-lookup"><span data-stu-id="674b5-194">This can happen in cloud-based systems like Azure SQL DB, which might move databases across servers.</span></span> <span data-ttu-id="674b5-195">若要在多个上下文中实现可复原的事务，请参阅本指南后面的[实现可复原的 Entity Framework Core SQL 连接](../implement-resilient-applications/implement-resilient-entity-framework-core-sql-connections.md)部分。</span><span class="sxs-lookup"><span data-stu-id="674b5-195">For implementing resilient transactions across multiple contexts, see the [Implementing resilient Entity Framework Core SQL connections](../implement-resilient-applications/implement-resilient-entity-framework-core-sql-connections.md) section later in this guide.</span></span>

<span data-ttu-id="674b5-196">为清楚起见，以下示例用一段单独的代码展示了整个过程。</span><span class="sxs-lookup"><span data-stu-id="674b5-196">For clarity, the following example shows the whole process in a single piece of code.</span></span> <span data-ttu-id="674b5-197">不过，eShopOnContainers 实现是重构的，并将此逻辑拆分成了多个类，因此更容易维护。</span><span class="sxs-lookup"><span data-stu-id="674b5-197">However, the eShopOnContainers implementation is refactored and splits this logic into multiple classes so it's easier to maintain.</span></span>

```csharp
// Update Product from the Catalog microservice
//
public async Task<IActionResult> UpdateProduct([FromBody]CatalogItem productToUpdate)
{
  var catalogItem =
       await _catalogContext.CatalogItems.SingleOrDefaultAsync(i => i.Id ==
                                                               productToUpdate.Id);
  if (catalogItem == null) return NotFound();

  bool raiseProductPriceChangedEvent = false;
  IntegrationEvent priceChangedEvent = null;

  if (catalogItem.Price != productToUpdate.Price)
          raiseProductPriceChangedEvent = true;

  if (raiseProductPriceChangedEvent) // Create event if price has changed
  {
      var oldPrice = catalogItem.Price;
      priceChangedEvent = new ProductPriceChangedIntegrationEvent(catalogItem.Id,
                                                                  productToUpdate.Price,
                                                                  oldPrice);
  }
  // Update current product
  catalogItem = productToUpdate;

  // Just save the updated product if the Product's Price hasn't changed.
  if (!raiseProductPriceChangedEvent)
  {
      await _catalogContext.SaveChangesAsync();
  }
  else  // Publish to event bus only if product price changed
  {
        // Achieving atomicity between original DB and the IntegrationEventLog
        // with a local transaction
        using (var transaction = _catalogContext.Database.BeginTransaction())
        {
           _catalogContext.CatalogItems.Update(catalogItem);
           await _catalogContext.SaveChangesAsync();

           await _integrationEventLogService.SaveEventAsync(priceChangedEvent);

           transaction.Commit();
        }

      // Publish the integration event through the event bus
      _eventBus.Publish(priceChangedEvent);

      _integrationEventLogService.MarkEventAsPublishedAsync(
                                                priceChangedEvent);
  }

  return Ok();
}

```

<span data-ttu-id="674b5-198">在创建 ProductPriceChangedIntegrationEvent 集成事件后，存储原始域操作（更新目录项）的事务也会在 EventLog 表中包括该事件的持久性。</span><span class="sxs-lookup"><span data-stu-id="674b5-198">After the ProductPriceChangedIntegrationEvent integration event is created, the transaction that stores the original domain operation (update the catalog item) also includes the persistence of the event in the EventLog table.</span></span> <span data-ttu-id="674b5-199">这使它成为了单个事务，因此，你始终能够检查是否发送了事件消息。</span><span class="sxs-lookup"><span data-stu-id="674b5-199">This makes it a single transaction, and you will always be able to check whether event messages were sent.</span></span>

<span data-ttu-id="674b5-200">通过对同一数据库使用本地事务，可使用原始数据库操作以原子方式更新事件日志表。</span><span class="sxs-lookup"><span data-stu-id="674b5-200">The event log table is updated atomically with the original database operation, using a local transaction against the same database.</span></span> <span data-ttu-id="674b5-201">如果任何操作失败，将引发异常，并且事务会回滚任何已完成的操作，从而保持域操作与保存到表中的事件消息之间的一致性。</span><span class="sxs-lookup"><span data-stu-id="674b5-201">If any of the operations fail, an exception is thrown and the transaction rolls back any completed operation, thus maintaining consistency between the domain operations and the event messages saved to the table.</span></span>

### <a name="receiving-messages-from-subscriptions-event-handlers-in-receiver-microservices"></a><span data-ttu-id="674b5-202">从订阅接收消息：接收者微服务中的事件处理程序</span><span class="sxs-lookup"><span data-stu-id="674b5-202">Receiving messages from subscriptions: event handlers in receiver microservices</span></span>

<span data-ttu-id="674b5-203">除了事件订阅逻辑外，还需要实现集成事件处理程序的内部代码（例如回调方法）。</span><span class="sxs-lookup"><span data-stu-id="674b5-203">In addition to the event subscription logic, you need to implement the internal code for the integration event handlers (like a callback method).</span></span> <span data-ttu-id="674b5-204">在事件处理程序中，可指定接收和处理某种事件消息的位置。</span><span class="sxs-lookup"><span data-stu-id="674b5-204">The event handler is where you specify where the event messages of a certain type will be received and processed.</span></span>

<span data-ttu-id="674b5-205">事件处理程序首先从事件总线接收事件实例。</span><span class="sxs-lookup"><span data-stu-id="674b5-205">An event handler first receives an event instance from the event bus.</span></span> <span data-ttu-id="674b5-206">然后定位与该集成事件相关的待处理组件，并在接收者微服务中将该事件作为一项状态更改予以传播和保留。</span><span class="sxs-lookup"><span data-stu-id="674b5-206">Then it locates the component to be processed related to that integration event, propagating and persisting the event as a change in state in the receiver microservice.</span></span> <span data-ttu-id="674b5-207">例如，如果 ProductPriceChanged 事件源自目录微服务，则会在购物车微服务中对其进行处理，并在此接收者购物车微服务中更改状态，如以下代码所示。</span><span class="sxs-lookup"><span data-stu-id="674b5-207">For example, if a ProductPriceChanged event originates in the catalog microservice, it is handled in the basket microservice and changes the state in this receiver basket microservice as well, as shown in the following code.</span></span>

```csharp
namespace Microsoft.eShopOnContainers.Services.Basket.API.IntegrationEvents.EventHandling
{
    public class ProductPriceChangedIntegrationEventHandler :
        IIntegrationEventHandler<ProductPriceChangedIntegrationEvent>
    {
        private readonly IBasketRepository _repository;

        public ProductPriceChangedIntegrationEventHandler(
            IBasketRepository repository)
        {
            _repository = repository;
        }

        public async Task Handle(ProductPriceChangedIntegrationEvent @event)
        {
            var userIds = await _repository.GetUsers();
            foreach (var id in userIds)
            {
                var basket = await _repository.GetBasket(id);
                await UpdatePriceInBasketItems(@event.ProductId, @event.NewPrice, basket);
            }
        }

        private async Task UpdatePriceInBasketItems(int productId, decimal newPrice,
            CustomerBasket basket)
        {
            var itemsToUpdate = basket?.Items?.Where(x => int.Parse(x.ProductId) ==
                productId).ToList();
            if (itemsToUpdate != null)
            {
                foreach (var item in itemsToUpdate)
                {
                    if(item.UnitPrice != newPrice)
                    {
                        var originalPrice = item.UnitPrice;
                        item.UnitPrice = newPrice;
                        item.OldUnitPrice = originalPrice;
                    }
                }
                await _repository.UpdateBasket(basket);
            }
        }
    }
}
```

<span data-ttu-id="674b5-208">事件处理程序需要验证该产品是否存在于任何购物车实例中。</span><span class="sxs-lookup"><span data-stu-id="674b5-208">The event handler needs to verify whether the product exists in any of the basket instances.</span></span> <span data-ttu-id="674b5-209">它还会更新每个相关购物车订单项的商品价格。</span><span class="sxs-lookup"><span data-stu-id="674b5-209">It also updates the item price for each related basket line item.</span></span> <span data-ttu-id="674b5-210">最后，它会创建一个警报，以向用户显示价格变化，如图 6-24 所示。</span><span class="sxs-lookup"><span data-stu-id="674b5-210">Finally, it creates an alert to be displayed to the user about the price change, as shown in Figure 6-24.</span></span>

![显示用户购物车的价格变化通知的浏览器屏幕截图。](./media/subscribe-events/display-item-price-change.png)

<span data-ttu-id="674b5-212">图 6-24  。</span><span class="sxs-lookup"><span data-stu-id="674b5-212">**Figure 6-24**.</span></span> <span data-ttu-id="674b5-213">根据集成事件传达的信息显示购物车中商品的价格变化</span><span class="sxs-lookup"><span data-stu-id="674b5-213">Displaying an item price change in a basket, as communicated by integration events</span></span>

## <a name="idempotency-in-update-message-events"></a><span data-ttu-id="674b5-214">更新消息事件中的幂等性</span><span class="sxs-lookup"><span data-stu-id="674b5-214">Idempotency in update message events</span></span>

<span data-ttu-id="674b5-215">更新消息事件的一个重要方面是，通信中任何一点的失败都应导致消息重试。</span><span class="sxs-lookup"><span data-stu-id="674b5-215">An important aspect of update message events is that a failure at any point in the communication should cause the message to be retried.</span></span> <span data-ttu-id="674b5-216">否则，后台任务可能会尝试发布已发布的事件，从而创建竞争条件。</span><span class="sxs-lookup"><span data-stu-id="674b5-216">Otherwise a background task might try to publish an event that has already been published, creating a race condition.</span></span> <span data-ttu-id="674b5-217">请确保更新是幂等的，或者它们提供足够的信息来确保你能检测到重复事件、放弃该事件并仅发回一个响应。</span><span class="sxs-lookup"><span data-stu-id="674b5-217">Make sure that the updates are either idempotent or that they provide enough information to ensure that you can detect a duplicate, discard it, and send back only one response.</span></span>

<span data-ttu-id="674b5-218">如前所述，幂等性意味着可以多次执行某项操作而不改变结果。</span><span class="sxs-lookup"><span data-stu-id="674b5-218">As noted earlier, idempotency means that an operation can be performed multiple times without changing the result.</span></span> <span data-ttu-id="674b5-219">在消息传递环境中，比如在传递事件时，如果一个事件可以被传递多次而不改变接收者微服务的结果，则该事件是幂等的。</span><span class="sxs-lookup"><span data-stu-id="674b5-219">In a messaging environment, as when communicating events, an event is idempotent if it can be delivered multiple times without changing the result for the receiver microservice.</span></span> <span data-ttu-id="674b5-220">事件本身的性质或者系统处理事件的方式可能要求事件必须幂等。</span><span class="sxs-lookup"><span data-stu-id="674b5-220">This may be necessary because of the nature of the event itself, or because of the way the system handles the event.</span></span> <span data-ttu-id="674b5-221">在任何使用消息传递的应用程序中，而不仅仅是在实现事件总线模式的应用程序中，消息幂等性都非常重要。</span><span class="sxs-lookup"><span data-stu-id="674b5-221">Message idempotency is important in any application that uses messaging, not just in applications that implement the event bus pattern.</span></span>

<span data-ttu-id="674b5-222">仅当表中没有该数据时才将其插入表中的 SQL 语句就是幂等操作的一个示例。</span><span class="sxs-lookup"><span data-stu-id="674b5-222">An example of an idempotent operation is a SQL statement that inserts data into a table only if that data is not already in the table.</span></span> <span data-ttu-id="674b5-223">无论运行该 insert SQL 语句多少次都无关紧要；结果都一样：表中将包含该数据。</span><span class="sxs-lookup"><span data-stu-id="674b5-223">It does not matter how many times you run that insert SQL statement; the result will be the same—the table will contain that data.</span></span> <span data-ttu-id="674b5-224">当处理消息时，如果消息可能被发送并因此被处理多次，这样的幂等性也是必要的。</span><span class="sxs-lookup"><span data-stu-id="674b5-224">Idempotency like this can also be necessary when dealing with messages if the messages could potentially be sent and therefore processed more than once.</span></span> <span data-ttu-id="674b5-225">例如，如果重试逻辑导致发送者多次发送完全相同的消息，则需要确保它是幂等的。</span><span class="sxs-lookup"><span data-stu-id="674b5-225">For instance, if retry logic causes a sender to send exactly the same message more than once, you need to make sure that it is idempotent.</span></span>

<span data-ttu-id="674b5-226">设计幂等消息是可行的。</span><span class="sxs-lookup"><span data-stu-id="674b5-226">It is possible to design idempotent messages.</span></span> <span data-ttu-id="674b5-227">例如，可以创建一个指示“将产品价格设置为 $25”而不是“产品价格上涨 $5”的事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-227">For example, you can create an event that says "set the product price to $25" instead of "add $5 to the product price."</span></span> <span data-ttu-id="674b5-228">可以对第一条消息安全地处理任意多次，结果将相同。</span><span class="sxs-lookup"><span data-stu-id="674b5-228">You could safely process the first message any number of times and the result will be the same.</span></span> <span data-ttu-id="674b5-229">第二条消息则非如此。</span><span class="sxs-lookup"><span data-stu-id="674b5-229">That is not true for the second message.</span></span> <span data-ttu-id="674b5-230">但即使在第一种情况下，你可能也不想处理第一个事件，因为系统也可能发送了更新的 price-change 事件，你会覆盖掉新价格。</span><span class="sxs-lookup"><span data-stu-id="674b5-230">But even in the first case, you might not want to process the first event, because the system could also have sent a newer price-change event and you would be overwriting the new price.</span></span>

<span data-ttu-id="674b5-231">另一个示例可能是传播到多个订阅者的 order-completed 事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-231">Another example might be an order-completed event that's propagated to multiple subscribers.</span></span> <span data-ttu-id="674b5-232">应用必须确保订单信息只需在其他系统中更新一次，即使同一 order-completed 事件存在重复的消息事件也是如此。</span><span class="sxs-lookup"><span data-stu-id="674b5-232">The app has to make sure that order information is updated in other systems only once, even if there are duplicated message events for the same order-completed event.</span></span>

<span data-ttu-id="674b5-233">为每个事件设置某种标识会很方便，这样你就可以创建逻辑来强制每个接收者只处理每个事件一次。</span><span class="sxs-lookup"><span data-stu-id="674b5-233">It is convenient to have some kind of identity per event so that you can create logic that enforces that each event is processed only once per receiver.</span></span>

<span data-ttu-id="674b5-234">某些消息处理本质上是幂等的。</span><span class="sxs-lookup"><span data-stu-id="674b5-234">Some message processing is inherently idempotent.</span></span> <span data-ttu-id="674b5-235">例如，如果系统生成图像缩略图，那么，对已生成缩略图的相关消息处理了多少次可能无关紧要；结果就是生成了缩略图，每次都相同。</span><span class="sxs-lookup"><span data-stu-id="674b5-235">For example, if a system generates image thumbnails, it might not matter how many times the message about the generated thumbnail is processed; the outcome is that the thumbnails are generated and they are the same every time.</span></span> <span data-ttu-id="674b5-236">另一方面，诸如调用支付网关来对信用卡收费之类的操作可能完全不幂等。</span><span class="sxs-lookup"><span data-stu-id="674b5-236">On the other hand, operations such as calling a payment gateway to charge a credit card may not be idempotent at all.</span></span> <span data-ttu-id="674b5-237">在这些情况下，你需要确保多次处理某条消息能达到预期的效果。</span><span class="sxs-lookup"><span data-stu-id="674b5-237">In these cases, you need to ensure that processing a message multiple times has the effect that you expect.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="674b5-238">其他资源</span><span class="sxs-lookup"><span data-stu-id="674b5-238">Additional resources</span></span>

- <span data-ttu-id="674b5-239">**遵循消息幂等性** </span><span class="sxs-lookup"><span data-stu-id="674b5-239">**Honoring message idempotency** </span></span>\
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591565(v=pandp.10)#honoring-message-idempotency>

## <a name="deduplicating-integration-event-messages"></a><span data-ttu-id="674b5-240">删除重复的集成事件消息</span><span class="sxs-lookup"><span data-stu-id="674b5-240">Deduplicating integration event messages</span></span>

<span data-ttu-id="674b5-241">你可以确保每个订阅者在不同级别只发送和处理一次消息事件。</span><span class="sxs-lookup"><span data-stu-id="674b5-241">You can make sure that message events are sent and processed only once per subscriber at different levels.</span></span> <span data-ttu-id="674b5-242">一种方法是使用你正在使用的消息传递基础结构提供的重复数据删除功能。</span><span class="sxs-lookup"><span data-stu-id="674b5-242">One way is to use a deduplication feature offered by the messaging infrastructure you are using.</span></span> <span data-ttu-id="674b5-243">另一种方法是在目标微服务中实现自定义逻辑。</span><span class="sxs-lookup"><span data-stu-id="674b5-243">Another is to implement custom logic in your destination microservice.</span></span> <span data-ttu-id="674b5-244">最佳选择是在传输级别和应用程序级别同时实施验证。</span><span class="sxs-lookup"><span data-stu-id="674b5-244">Having validations at both the transport level and the application level is your best bet.</span></span>

### <a name="deduplicating-message-events-at-the-eventhandler-level"></a><span data-ttu-id="674b5-245">在 EventHandler 级别删除重复的消息事件</span><span class="sxs-lookup"><span data-stu-id="674b5-245">Deduplicating message events at the EventHandler level</span></span>

<span data-ttu-id="674b5-246">确保任何接收者对某个事件仅处理一次的一种方法是在事件处理程序中处理消息事件时实现特定逻辑。</span><span class="sxs-lookup"><span data-stu-id="674b5-246">One way to make sure that an event is processed only once by any receiver is by implementing certain logic when processing the message events in event handlers.</span></span> <span data-ttu-id="674b5-247">例如，这是 eShopOnContainers 应用程序中使用的方法，正如 [UserCheckoutAcceptedIntegrationEventHandler 类](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) 收到 `UserCheckoutAcceptedIntegrationEvent` 集成事件时其源代码中所示。</span><span class="sxs-lookup"><span data-stu-id="674b5-247">For example, that is the approach used in the eShopOnContainers application, as you can see in the [source code of the UserCheckoutAcceptedIntegrationEventHandler class](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) when it receives a `UserCheckoutAcceptedIntegrationEvent` integration event.</span></span> <span data-ttu-id="674b5-248">（在这种情况下，`CreateOrderCommand` 会使用 `IdentifiedCommand` 进行包装，并会先将 `eventMsg.RequestId` 用作标识符后，再将其发送到命令处理程序）。</span><span class="sxs-lookup"><span data-stu-id="674b5-248">(In this case, the `CreateOrderCommand` is wrapped with an `IdentifiedCommand`, using the `eventMsg.RequestId` as an identifier, before sending it to the command handler).</span></span>

### <a name="deduplicating-messages-when-using-rabbitmq"></a><span data-ttu-id="674b5-249">使用 RabbitMQ 时删除重复消息</span><span class="sxs-lookup"><span data-stu-id="674b5-249">Deduplicating messages when using RabbitMQ</span></span>

<span data-ttu-id="674b5-250">当发生间歇性网络故障时，可以复制消息，并且消息接收者必须准备好处理这些重复的消息。</span><span class="sxs-lookup"><span data-stu-id="674b5-250">When intermittent network failures happen, messages can be duplicated, and the message receiver must be ready to handle these duplicated messages.</span></span> <span data-ttu-id="674b5-251">如果可能，接收者应以幂等的方式处理消息，这比使用重复数据删除功能显式处理消息要好。</span><span class="sxs-lookup"><span data-stu-id="674b5-251">If possible, receivers should handle messages in an idempotent way, which is better than explicitly handling them with deduplication.</span></span>

<span data-ttu-id="674b5-252">根据 [RabbitMQ 文档](https://www.rabbitmq.com/reliability.html#consumer)，“如果在将某个消息传递给使用者后将其重新排入队列（例如，因为在使用者连接断开之前未确认该消息），RabbitMQ 会在重新传递该消息（无论是传递给相同的使用者还是不同的使用者）时对其设置“已重新传递”标志。</span><span class="sxs-lookup"><span data-stu-id="674b5-252">According to the [RabbitMQ documentation](https://www.rabbitmq.com/reliability.html#consumer), "If a message is delivered to a consumer and then requeued (because it was not acknowledged before the consumer connection dropped, for example) then RabbitMQ will set the redelivered flag on it when it is delivered again (whether to the same consumer or a different one).</span></span>

<span data-ttu-id="674b5-253">如果设置了“已重新传递”标志，接收者必须考虑这一点，因为消息可能已被处理。</span><span class="sxs-lookup"><span data-stu-id="674b5-253">If the "redelivered" flag is set, the receiver must take that into account, because the message might already have been processed.</span></span> <span data-ttu-id="674b5-254">但并不保证一定进行了处理；该消息在离开消息代理后可能由于网络问题从未到达接收者。</span><span class="sxs-lookup"><span data-stu-id="674b5-254">But that is not guaranteed; the message might never have reached the receiver after it left the message broker, perhaps because of network issues.</span></span> <span data-ttu-id="674b5-255">另一方面，如果未设置“已重新传递”标志，则可以保证该消息未发送多次。</span><span class="sxs-lookup"><span data-stu-id="674b5-255">On the other hand, if the "redelivered" flag is not set, it is guaranteed that the message has not been sent more than once.</span></span> <span data-ttu-id="674b5-256">因此，只有在消息中设置了“已重新传递”标志时，接收者才需要以幂等方式删除重复消息或处理消息。</span><span class="sxs-lookup"><span data-stu-id="674b5-256">Therefore, the receiver needs to deduplicate messages or process messages in an idempotent way only if the "redelivered" flag is set in the message.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="674b5-257">其他资源</span><span class="sxs-lookup"><span data-stu-id="674b5-257">Additional resources</span></span>

- <span data-ttu-id="674b5-258">**使用 NServiceBus 的分支 eShopOnContainers（特定软件）**  </span><span class="sxs-lookup"><span data-stu-id="674b5-258">**Forked eShopOnContainers using NServiceBus (Particular Software)** </span></span>\
    <https://go.particular.net/eShopOnContainers>

- <span data-ttu-id="674b5-259">**事件驱动的消息传递** </span><span class="sxs-lookup"><span data-stu-id="674b5-259">**Event Driven Messaging** </span></span>\
    <https://patterns.arcitura.com/soa-patterns/design_patterns/event_driven_messaging>

- <span data-ttu-id="674b5-260">**Jimmy Bogard。重构复原能力：评估耦合度** </span><span class="sxs-lookup"><span data-stu-id="674b5-260">**Jimmy Bogard. Refactoring Towards Resilience: Evaluating Coupling** </span></span>\
    <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

- <span data-ttu-id="674b5-261">**发布-订阅通道** </span><span class="sxs-lookup"><span data-stu-id="674b5-261">**Publish-Subscribe channel** </span></span>\
    <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- <span data-ttu-id="674b5-262">**界定上下文之间的通信** </span><span class="sxs-lookup"><span data-stu-id="674b5-262">**Communicating Between Bounded Contexts** </span></span>\
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- <span data-ttu-id="674b5-263">**最终一致性** </span><span class="sxs-lookup"><span data-stu-id="674b5-263">**Eventual Consistency** </span></span>\
    <https://en.wikipedia.org/wiki/Eventual_consistency>

- <span data-ttu-id="674b5-264">**Philip Brown。集成绑定上下文的策略** </span><span class="sxs-lookup"><span data-stu-id="674b5-264">**Philip Brown. Strategies for Integrating Bounded Contexts** </span></span>\
    <https://www.culttt.com/2014/11/26/strategies-integrating-bounded-contexts/>

- <span data-ttu-id="674b5-265">**Chris Richardson.使用聚合、事件源和 CQRS 开发事务型微服务 - 第 2 部分** </span><span class="sxs-lookup"><span data-stu-id="674b5-265">**Chris Richardson. Developing Transactional Microservices Using Aggregates, Event Sourcing and CQRS - Part 2** </span></span>\
    <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-2-richardson>

- <span data-ttu-id="674b5-266">**Chris Richardson.事件溯源模式** </span><span class="sxs-lookup"><span data-stu-id="674b5-266">**Chris Richardson. Event Sourcing pattern** </span></span>\
    <https://microservices.io/patterns/data/event-sourcing.html>

- <span data-ttu-id="674b5-267">**事件溯源简介** </span><span class="sxs-lookup"><span data-stu-id="674b5-267">**Introducing Event Sourcing** </span></span>\
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591559(v=pandp.10)>

- <span data-ttu-id="674b5-268">**Event Store 数据库**。</span><span class="sxs-lookup"><span data-stu-id="674b5-268">**Event Store database**.</span></span> <span data-ttu-id="674b5-269">官方网站。</span><span class="sxs-lookup"><span data-stu-id="674b5-269">Official site.</span></span> \
    <https://geteventstore.com/>

- <span data-ttu-id="674b5-270">**Patrick Nommensen。微服务的事件驱动数据管理** </span><span class="sxs-lookup"><span data-stu-id="674b5-270">**Patrick Nommensen. Event-Driven Data Management for Microservices** </span></span>\
    <https://dzone.com/articles/event-driven-data-management-for-microservices-1>

- <span data-ttu-id="674b5-271">**CAP 定理** </span><span class="sxs-lookup"><span data-stu-id="674b5-271">**The CAP Theorem** </span></span>\
    <https://en.wikipedia.org/wiki/CAP_theorem>

- <span data-ttu-id="674b5-272">**什么是 CAP 定理？**</span><span class="sxs-lookup"><span data-stu-id="674b5-272">**What is CAP Theorem?**</span></span> \
    <https://www.quora.com/What-Is-CAP-Theorem-1>

- <span data-ttu-id="674b5-273">**数据一致性入门指南** </span><span class="sxs-lookup"><span data-stu-id="674b5-273">**Data Consistency Primer** </span></span>\
    <https://docs.microsoft.com/previous-versions/msp-n-p/dn589800(v=pandp.10)>

- <span data-ttu-id="674b5-274">**Rick Saling。CAP 定理：为什么云和 Internet“一切都不同”**  </span><span class="sxs-lookup"><span data-stu-id="674b5-274">**Rick Saling. The CAP Theorem: Why "Everything is Different" with the Cloud and Internet** </span></span>\
    <https://docs.microsoft.com/archive/blogs/rickatmicrosoft/the-cap-theorem-why-everything-is-different-with-the-cloud-and-internet/>

- <span data-ttu-id="674b5-275">**Eric Brewer。CAP 十二年之后：“规则”更改的方式** </span><span class="sxs-lookup"><span data-stu-id="674b5-275">**Eric Brewer. CAP Twelve Years Later: How the "Rules" Have Changed** </span></span>\
    <https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed>

- <span data-ttu-id="674b5-276">**Azure 服务总线。中转消息传送：重复检测**  </span><span class="sxs-lookup"><span data-stu-id="674b5-276">**Azure Service Bus. Brokered Messaging: Duplicate Detection**  </span></span>\
    <https://code.msdn.microsoft.com/Brokered-Messaging-c0acea25>

- <span data-ttu-id="674b5-277">**可靠性指南**（RabbitMQ 文档）</span><span class="sxs-lookup"><span data-stu-id="674b5-277">**Reliability Guide** (RabbitMQ documentation) </span></span>\
    <https://www.rabbitmq.com/reliability.html#consumer>

> [!div class="step-by-step"]
> <span data-ttu-id="674b5-278">[上一页](rabbitmq-event-bus-development-test-environment.md)
> [下一页](test-aspnet-core-services-web-apps.md)</span><span class="sxs-lookup"><span data-stu-id="674b5-278">[Previous](rabbitmq-event-bus-development-test-environment.md)
[Next](test-aspnet-core-services-web-apps.md)</span></span>
