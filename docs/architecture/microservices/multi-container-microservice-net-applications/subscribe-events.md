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
# <a name="subscribing-to-events"></a>订阅事件

使用事件总线的第一步是为微服务订阅它们想要接收的事件。 此功能应在接收者微服务中完成。

下面的简单代码展示了每个接收者微服务在启动时（即，在 `Startup` 类中）必须实现哪些内容，才能订阅所需的事件。 在此示例中，`basket-api` 微服务需要订阅 `ProductPriceChangedIntegrationEvent` 和 `OrderStartedIntegrationEvent` 消息。

例如，通过订阅 `ProductPriceChangedIntegrationEvent` 事件，购物车微服务可了解产品价格的任何变化，并在用户购物车中有该产品时向该用户发出价格变动警告。

```csharp
var eventBus = app.ApplicationServices.GetRequiredService<IEventBus>();

eventBus.Subscribe<ProductPriceChangedIntegrationEvent,
                   ProductPriceChangedIntegrationEventHandler>();

eventBus.Subscribe<OrderStartedIntegrationEvent,
                   OrderStartedIntegrationEventHandler>();

```

此代码运行后，订阅者微服务将通过 RabbitMQ 通道进行侦听。 当 ProductPriceChangedIntegrationEvent 类型的任何消息到达时，该代码会调用传递给它的事件处理程序并处理该事件。

## <a name="publishing-events-through-the-event-bus"></a>通过事件总线发布事件

最后，消息发送者（源微服务）使用类似于以下示例的代码发布集成事件。 （此方法是一个简化的示例，不考虑原子性。）每当必须跨多个微服务传播事件时（通常就在从源微服务提交数据或事务之后），都会实现类似的代码。

首先，在控制器构造函数中注入事件总线实现对象（基于 RabbitMQ 或基于服务总线），如以下代码所示：

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

然后，从控制器的方法中使用它，例如在 UpdateProduct 方法中：

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

在此示例中，源微服务是一个简单的 CRUD 微服务，因此，该代码直接放置在 Web API 控制器中。

在更高级的微服务中，比如使用 CQRS 方法时，可以在 `CommandHandler` 类的 `Handle()` 方法中实现。

### <a name="designing-atomicity-and-resiliency-when-publishing-to-the-event-bus"></a>设计发布到事件总线时的原子性和复原能力

如果通过分布式消息传递系统（如事件总线）发布集成事件，在以原子方式更新原始数据库和发布事件时会出现问题（即，两个操作都已完成，或者都没有完成）。 例如，在前面所示的简化示例中，代码在产品价格发生变动时向数据库提交数据，然后发布 ProductPriceChangedIntegrationEvent 消息。 最开始，这两个操作看起来可能必须以原子方式执行。 但是，如果像在 [Microsoft 消息队列 (MSMQ)](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)) 等早期系统中那样，使用涉及数据库和消息代理的分布式事务，那么根据 [CAP 定理](https://www.quora.com/What-Is-CAP-Theorem-1)所描述的原因，不建议使用此方法。

微服务主要用于构建可缩放且高度可用的系统。 简单来说，CAP 定理认为你无法构建一个兼具  持续可用性、强一致性和分区容错性的（分布式）数据库（或拥有其模型的微服务）。 你只能选择这三种属性中的两种。

在基于微服务的体系结构中，应选择可用性和容错性，而不再强调强一致性。 因此，在大多数基于微服务的现代应用程序中，你通常不希望像在使用 [MSMQ](/previous-versions/windows/desktop/legacy/ms711472(v=vs.85)) 基于 Windows 分布式事务处理协调器 (DTC) 实现[分布式事务](/previous-versions/windows/desktop/ms681205(v=vs.85))时那样，在消息传递中使用分布式事务。

让我们回到最初的问题及其示例。 如果服务在数据库更新之后（在本例中，也就是在运行 `_context.SaveChangesAsync()` 代码行之后）但在发布集成事件之前崩溃，则整个系统会变得不一致。 此方法对业务而言可能很关键，具体取决于你正在处理的具体业务操作。

正如前面在体系结构部分中提到的那样，可以通过以下几种方法来处理此问题：

- 使用完整[事件溯源模式](/azure/architecture/patterns/event-sourcing)。

- 使用事务日志挖掘。

- 使用[发件箱模式](https://www.kamilgrzybek.com/design/the-outbox-pattern/)。 这是用于存储集成事件（扩展本地事务）的事务表。

在本案例中，使用完整事件溯源 (ES) 模式即使算不上 *最好* 的办法，也是最好的办法之一。 但是，在许多应用程序案例中，你可能无法实现完整的 ES 系统。 ES 意味着仅在事务数据库中存储域事件，而不存储当前状态数据。 仅存储域事件可以带来很大的好处，例如可以获得系统的历史记录，并且能够确定系统在过去任意时间点的状态。 但是，实现完整的 ES 系统需要重新构建大部分系统，并引入许多其他复杂性和要求。 例如，你希望使用专用于事件溯源的数据库（例如 [Event Store](https://eventstore.org/)）或面向文档的数据库（例如 Azure Cosmos DB、MongoDB、Cassandra、CouchDB 或 RavenDB）。 对于此问题，ES 是一种很好的解决方案，但不是最简单的解决方案，除非你已经熟悉事件溯源。

使用事务日志挖掘的方法最初看起来很透明。 但是，若要使用此方法，微服务必须与 RDBMS 事务日志（例如 SQL Server 事务日志）耦合。 此方法可能是不可取的。 还有一个缺点就是，事务日志中记录的低级别更新可能与高级别集成事件不在同一级别。 如果是这样，可能难以对这些事务日志操作执行反向工程操作。

较平衡的一种方法是混合使用事务数据库表和简化的 ES 模式。 可以使用诸如“准备发布事件”之类的状态，在将原始事件提交到集成事件表时，可以在原始事件中设置该状态。 然后尝试将事件发布到事件总线。 如果 publish-event 操作成功，则在源服务中启动另一个事务，并将状态从“准备发布事件”更改为“已发布事件”。

如果事件总线中的 publish-event 操作失败，则数据在源微服务中仍保持一致，它仍被标记为“准备发布事件”，并且对于其他服务，它将实现最终一致性。 你始终可以通过后台作业来检查事务或集成事件的状态。 如果作业找到状态为“准备发布事件”的事件，它可以尝试将该事件重新发布到事件总线。

请注意，使用此方法时，只需保留每个源微服务的集成事件，以及要传播到其他微服务或外部系统的事件。 而在完整的 ES 系统中，还需要存储所有域事件。

因此，这种平衡方法其实就是一个简化的 ES 系统。 你需要一个包含集成事件及其当前状态（“准备发布”与“已发布”）的列表。 但你只需为集成事件实现这些状态。 在此方法中，无需像在完整的 ES 系统中那样，将所有域数据作为事件存储在事务数据库中。

如果已经在使用关系数据库，则可以使用事务表来存储集成事件。 若要在应用程序中实现原子性，可以使用基于本地事务的过程，该过程包含两个步骤。 本质上来说，就是在域实体所在的数据库中创建一个 IntegrationEvent 表。 该表用于保证实现原子性，以便将持久性集成事件添加到提交域数据的相同事务中。

此进程的分步操作如下：：

1. 应用程序开始处理一项本地数据库事务。

2. 然后更新域实体的状态，并向集成事件表中插入一个事件。

3. 最后，它将提交事务，如此即可获得所需的原子性，然后

4. 以某种方式发布事件（下一步）。

在实施事件发布步骤时，你有以下选择：

- 在提交事务后立即发布集成事件，并使用另一个本地事务将表中的事件标记为已发布。 然后，将该表用作一个项目，以便在远程微服务出现问题时跟踪集成事件，并根据存储的集成事件执行补偿操作。

- 将该表用作一种队列。 单独的应用程序线程或进程查询集成事件表，将事件发布到事件总线，然后使用本地事务将事件标记为已发布。

图 6-22 展示了第一种方法的体系结构。

![不使用辅助角色微服务发布时的原子性关系图。](./media/subscribe-events/atomicity-publish-event-bus.png)

图 6-22  。 将事件发布到事件总线时的原子性

图 6-22 所示的方法缺少一个附加的辅助微服务，它负责检查和确认已发布的集成事件成功与否。 如果失败，该附加检查器辅助微服务可以从表中读取事件并重新发布它们，即重复步骤 2。

关于第二种方法：将 EventLog 表用作队列，并始终使用辅助微服务来发布消息。 在这种情况下，其过程如图 6-23 所示。 图中显示了一个附加的微服务，并且该表作为发布事件时的单一事件源。

![使用辅助角色微服务发布时的原子性关系图。](./media/subscribe-events/atomicity-publish-worker-microservice.png)

图 6-23  。 使用辅助微服务将事件发布到事件总线时的原子性

为简单起见，eShopOnContainers 示例使用第一种方法（没有附加进程或检查器微服务）和事件总线。 但 eShopOnContainers 示例并未处理所有可能的故障情况。 在部署到云端的实际应用程序中，你必须接受最终会出现问题的事实，并且必须实施该检查并实现重新发送逻辑。 如果在通过事件总线发布事件（使用辅助角色）时，该表是单一的事件源，那么，将该表用作队列可能比第一种方法更有效。

### <a name="implementing-atomicity-when-publishing-integration-events-through-the-event-bus"></a>通过事件总线发布集成事件时实现原子性

以下代码展示如何创建涉及多个 DbContext 对象的单一事务：一个上下文与要更新的原始数据相关，另一个上下文与 IntegrationEventLog 表相关。

如果在代码运行时，数据库连接出现任何问题，以下示例代码中的事务将无法复原。 在可能跨服务器移动数据库的基于云的系统（例如 Azure SQL DB）中，可能会出现这种问题。 若要在多个上下文中实现可复原的事务，请参阅本指南后面的[实现可复原的 Entity Framework Core SQL 连接](../implement-resilient-applications/implement-resilient-entity-framework-core-sql-connections.md)部分。

为清楚起见，以下示例用一段单独的代码展示了整个过程。 不过，eShopOnContainers 实现是重构的，并将此逻辑拆分成了多个类，因此更容易维护。

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

在创建 ProductPriceChangedIntegrationEvent 集成事件后，存储原始域操作（更新目录项）的事务也会在 EventLog 表中包括该事件的持久性。 这使它成为了单个事务，因此，你始终能够检查是否发送了事件消息。

通过对同一数据库使用本地事务，可使用原始数据库操作以原子方式更新事件日志表。 如果任何操作失败，将引发异常，并且事务会回滚任何已完成的操作，从而保持域操作与保存到表中的事件消息之间的一致性。

### <a name="receiving-messages-from-subscriptions-event-handlers-in-receiver-microservices"></a>从订阅接收消息：接收者微服务中的事件处理程序

除了事件订阅逻辑外，还需要实现集成事件处理程序的内部代码（例如回调方法）。 在事件处理程序中，可指定接收和处理某种事件消息的位置。

事件处理程序首先从事件总线接收事件实例。 然后定位与该集成事件相关的待处理组件，并在接收者微服务中将该事件作为一项状态更改予以传播和保留。 例如，如果 ProductPriceChanged 事件源自目录微服务，则会在购物车微服务中对其进行处理，并在此接收者购物车微服务中更改状态，如以下代码所示。

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

事件处理程序需要验证该产品是否存在于任何购物车实例中。 它还会更新每个相关购物车订单项的商品价格。 最后，它会创建一个警报，以向用户显示价格变化，如图 6-24 所示。

![显示用户购物车的价格变化通知的浏览器屏幕截图。](./media/subscribe-events/display-item-price-change.png)

图 6-24  。 根据集成事件传达的信息显示购物车中商品的价格变化

## <a name="idempotency-in-update-message-events"></a>更新消息事件中的幂等性

更新消息事件的一个重要方面是，通信中任何一点的失败都应导致消息重试。 否则，后台任务可能会尝试发布已发布的事件，从而创建竞争条件。 请确保更新是幂等的，或者它们提供足够的信息来确保你能检测到重复事件、放弃该事件并仅发回一个响应。

如前所述，幂等性意味着可以多次执行某项操作而不改变结果。 在消息传递环境中，比如在传递事件时，如果一个事件可以被传递多次而不改变接收者微服务的结果，则该事件是幂等的。 事件本身的性质或者系统处理事件的方式可能要求事件必须幂等。 在任何使用消息传递的应用程序中，而不仅仅是在实现事件总线模式的应用程序中，消息幂等性都非常重要。

仅当表中没有该数据时才将其插入表中的 SQL 语句就是幂等操作的一个示例。 无论运行该 insert SQL 语句多少次都无关紧要；结果都一样：表中将包含该数据。 当处理消息时，如果消息可能被发送并因此被处理多次，这样的幂等性也是必要的。 例如，如果重试逻辑导致发送者多次发送完全相同的消息，则需要确保它是幂等的。

设计幂等消息是可行的。 例如，可以创建一个指示“将产品价格设置为 $25”而不是“产品价格上涨 $5”的事件。 可以对第一条消息安全地处理任意多次，结果将相同。 第二条消息则非如此。 但即使在第一种情况下，你可能也不想处理第一个事件，因为系统也可能发送了更新的 price-change 事件，你会覆盖掉新价格。

另一个示例可能是传播到多个订阅者的 order-completed 事件。 应用必须确保订单信息只需在其他系统中更新一次，即使同一 order-completed 事件存在重复的消息事件也是如此。

为每个事件设置某种标识会很方便，这样你就可以创建逻辑来强制每个接收者只处理每个事件一次。

某些消息处理本质上是幂等的。 例如，如果系统生成图像缩略图，那么，对已生成缩略图的相关消息处理了多少次可能无关紧要；结果就是生成了缩略图，每次都相同。 另一方面，诸如调用支付网关来对信用卡收费之类的操作可能完全不幂等。 在这些情况下，你需要确保多次处理某条消息能达到预期的效果。

### <a name="additional-resources"></a>其他资源

- **遵循消息幂等性** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591565(v=pandp.10)#honoring-message-idempotency>

## <a name="deduplicating-integration-event-messages"></a>删除重复的集成事件消息

你可以确保每个订阅者在不同级别只发送和处理一次消息事件。 一种方法是使用你正在使用的消息传递基础结构提供的重复数据删除功能。 另一种方法是在目标微服务中实现自定义逻辑。 最佳选择是在传输级别和应用程序级别同时实施验证。

### <a name="deduplicating-message-events-at-the-eventhandler-level"></a>在 EventHandler 级别删除重复的消息事件

确保任何接收者对某个事件仅处理一次的一种方法是在事件处理程序中处理消息事件时实现特定逻辑。 例如，这是 eShopOnContainers 应用程序中使用的方法，正如 [UserCheckoutAcceptedIntegrationEventHandler 类](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/IntegrationEvents/EventHandling/UserCheckoutAcceptedIntegrationEventHandler.cs) 收到 `UserCheckoutAcceptedIntegrationEvent` 集成事件时其源代码中所示。 （在这种情况下，`CreateOrderCommand` 会使用 `IdentifiedCommand` 进行包装，并会先将 `eventMsg.RequestId` 用作标识符后，再将其发送到命令处理程序）。

### <a name="deduplicating-messages-when-using-rabbitmq"></a>使用 RabbitMQ 时删除重复消息

当发生间歇性网络故障时，可以复制消息，并且消息接收者必须准备好处理这些重复的消息。 如果可能，接收者应以幂等的方式处理消息，这比使用重复数据删除功能显式处理消息要好。

根据 [RabbitMQ 文档](https://www.rabbitmq.com/reliability.html#consumer)，“如果在将某个消息传递给使用者后将其重新排入队列（例如，因为在使用者连接断开之前未确认该消息），RabbitMQ 会在重新传递该消息（无论是传递给相同的使用者还是不同的使用者）时对其设置“已重新传递”标志。

如果设置了“已重新传递”标志，接收者必须考虑这一点，因为消息可能已被处理。 但并不保证一定进行了处理；该消息在离开消息代理后可能由于网络问题从未到达接收者。 另一方面，如果未设置“已重新传递”标志，则可以保证该消息未发送多次。 因此，只有在消息中设置了“已重新传递”标志时，接收者才需要以幂等方式删除重复消息或处理消息。

### <a name="additional-resources"></a>其他资源

- **使用 NServiceBus 的分支 eShopOnContainers（特定软件）**  \
    <https://go.particular.net/eShopOnContainers>

- **事件驱动的消息传递** \
    <https://patterns.arcitura.com/soa-patterns/design_patterns/event_driven_messaging>

- **Jimmy Bogard。重构复原能力：评估耦合度** \
    <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

- **发布-订阅通道** \
    <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- **界定上下文之间的通信** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- **最终一致性** \
    <https://en.wikipedia.org/wiki/Eventual_consistency>

- **Philip Brown。集成绑定上下文的策略** \
    <https://www.culttt.com/2014/11/26/strategies-integrating-bounded-contexts/>

- **Chris Richardson.使用聚合、事件源和 CQRS 开发事务型微服务 - 第 2 部分** \
    <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-2-richardson>

- **Chris Richardson.事件溯源模式** \
    <https://microservices.io/patterns/data/event-sourcing.html>

- **事件溯源简介** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/jj591559(v=pandp.10)>

- **Event Store 数据库**。 官方网站。 \
    <https://geteventstore.com/>

- **Patrick Nommensen。微服务的事件驱动数据管理** \
    <https://dzone.com/articles/event-driven-data-management-for-microservices-1>

- **CAP 定理** \
    <https://en.wikipedia.org/wiki/CAP_theorem>

- **什么是 CAP 定理？** \
    <https://www.quora.com/What-Is-CAP-Theorem-1>

- **数据一致性入门指南** \
    <https://docs.microsoft.com/previous-versions/msp-n-p/dn589800(v=pandp.10)>

- **Rick Saling。CAP 定理：为什么云和 Internet“一切都不同”**  \
    <https://docs.microsoft.com/archive/blogs/rickatmicrosoft/the-cap-theorem-why-everything-is-different-with-the-cloud-and-internet/>

- **Eric Brewer。CAP 十二年之后：“规则”更改的方式** \
    <https://www.infoq.com/articles/cap-twelve-years-later-how-the-rules-have-changed>

- **Azure 服务总线。中转消息传送：重复检测**  \
    <https://code.msdn.microsoft.com/Brokered-Messaging-c0acea25>

- **可靠性指南**（RabbitMQ 文档）\
    <https://www.rabbitmq.com/reliability.html#consumer>

> [!div class="step-by-step"]
> [上一页](rabbitmq-event-bus-development-test-environment.md)
> [下一页](test-aspnet-core-services-web-apps.md)
