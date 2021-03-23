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
# <a name="dapr-reference-application"></a>Dapr 引用应用程序

本书前面介绍了 Dapr 的基础知识。 你了解了 Dapr 如何帮助你的团队构造分布式应用程序，同时降低体系结构和操作的复杂性。 在此过程中，您有机会构建一些小规模的 Dapr 应用程序。 现在，可以浏览端到端微服务应用程序，该应用程序演示了 Dapr 构建基块和组件。

但首先要记录一些历史记录。

## <a name="eshop-on-containers"></a>容器上的 eShop

几年前，Microsoft （与领先社区专家合作）发布了一个热门的指南书籍， [为容器化 .Net 应用程序提供 .Net 微服务](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf)。 图3-1 显示了本书：

![构建容器化的微服务 .NET 应用程序。](./media/reference-application/architecting-microservices-book.png)

**图 3-1**。 .NET 微服务：容器化 .NET 应用程序的体系结构。

本书 dove 了有关生成分布式应用程序的原则、模式和最佳实践。 它包含一个功能齐全的微服务参考应用程序，可展示体系结构概念。 [EShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)，该应用程序显示了一名电子商务店面，其中销售了各种 .net 项目，包括衣服和咖啡杯子。  内置于 .NET Core，它是跨平台的应用程序，可以在 Linux 或 Windows 容器中运行。 图3-2 显示了原始 eShop 的体系结构。

![eShopOnContainers 参考应用程序体系结构。](./media/reference-application/eshop-on-containers.png)

**图 3-2**。 原始 `ShopOnContainers` 引用应用程序。

如您所见，eShopOnContainers 包含许多移动部件：

1. 三个不同的前端客户端。
1. 用于从前端抽象后端的应用程序网关。
1. 多个后端核心微服务。
1. 启用异步发布/订阅消息传递的事件总线组件。

EShopOnContainers 引用应用程序已在 .NET 社区中广泛接受，并用于对许多大型商业微服务应用程序进行建模。

## <a name="eshop-on-dapr"></a>Dapr 上的 eShop

本书随附了其他版本的 eShop 应用程序。 这称为 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)。 更新后的版本通过集成 Dapr 构建基块和组件来发展早期的 eShopOnContainers 应用程序。 图3-3 显示了新的简化解决方案体系结构：  

![eShopOnDapr 参考应用程序体系结构。](./media/reference-application/eshop-on-dapr.png)

**图 3-3**。 eShopOnDapr 参考应用程序体系结构。

由于 eShopOnDapr 引用应用程序的焦点在 Dapr 上，因此原始应用程序已更新。 该体系结构包括：

1. 用常用角 SPA 框架编写的 [单页面应用程序](/archive/msdn-magazine/2013/november/asp-net-single-page-applications-build-modern-responsive-web-apps-with-asp-net) 前端。 它将用户请求发送到 API 网关微服务。

1. API 网关会从前端客户端抽象后端核心微服务。 它使用 [Envoy](https://www.envoyproxy.io/)（一种高性能的开源服务代理）来实现。 Envoy 将传入请求路由到各种后端微服务。 大多数请求是简单的 CRUD 操作 (例如，从目录获取品牌列表，) 并通过直接调用后端微服务来处理。

1. 其他请求逻辑上比较复杂，并且需要多个微服务一起工作。 在这些情况下，eShopOnDapr 实现了一个 [聚合器微服务](../cloud-native/service-to-service-communication.md#service-aggregator-pattern) ，用于在完成操作所需的微服务间协调工作流。

1. 一组核心后端微服务包含电子商务存储所需的功能。 每个都是独立的，并且独立于其他。 以下是广泛接受的域分解模式，每个微服务都隔离特定的 *业务功能*：

   - 购物篮服务管理客户的购物篮体验。
   - 目录服务管理可用于销售的产品项。
   - 标识服务管理身份验证和标识。
   - 订购服务处理放置和管理订单的所有方面。
   - 支付服务开展客户的付款。

   每个服务都有自己的持久存储。 坚持微服务 [最佳实践](../cloud-native/distributed-data.md#database-per-microservice-why)，不存在所有服务都交互的共享数据存储。

   每个微服务的设计都基于其各自的要求。 简单服务使用基本的 CRUD 操作来访问其基础数据存储区。 高级服务（如排序）使用 Domain-Driven 设计方法来管理业务复杂性。 必要时，可以跨不同的技术堆栈（如 .NET Core、Java、中转、NodeJS 等）生成服务。

1. 最后，事件总线会包装 Dapr 发布/订阅组件。 它通过微服务实现异步发布/订阅消息传送。 开发人员可以插入任何 Dapr 支持的消息代理。

### <a name="application-of-dapr-building-blocks"></a>Dapr 构建基块的应用程序

EShopOnDapr 基本代码比 eShopOnContainers 基本代码更简单。 Dapr 构建基块替换大量易出错的管道代码。

图3-4 显示了 eShop 引用应用程序中的 Dapr 集成。

![eShopOnDapr 参考应用程序体系结构](./media/reference-application/eshop-on-dapr-buildingblocks.png)

**图 3-4**。 EShopOnDapr 中的 Dapr 集成。

在上图中，可以看到哪些服务使用哪些 Dapr 构建基块。

1. 原始 eShopOnContainers 应用程序演示了订单服务中的 DDD 概念和模式。 在更新的 eShopOnDapr 中，订购服务使用执行组件 *构建基块* 作为替代实现。 使用者的基于车的访问模型可以轻松实现具有取消支持的有状态订单过程。
1. 订购服务使用 [绑定构建基块](bindings.md)发送订单确认电子邮件。
1. 后端服务使用 [publish & 订阅构建块](publish-subscribe.md)进行异步通信。
1. 机密管理由 [机密构建块](secrets.md)完成。
1. API 网关和 web 购物聚合器服务使用 [服务调用构建基块](service-invocation.md) 对后端服务调用方法。
1. 购物篮服务使用 [状态管理构建基块](state-management.md) 来存储客户购物篮的状态。

### <a name="benefits-of-applying-dapr-to-eshop"></a>将 Dapr 应用于 eShop 的好处

通常，使用 Dapr 构建基块可向应用程序添加可观察性和灵活性：

1. 可观察性：通过使用 Dapr 构建基块，可为服务之间的两个调用和 Dapr 组件获取丰富的分布式跟踪，而无需编写任何代码。 在 eShopOnContainers 中，将使用大量的自定义日志记录来提供见解。
1. 灵活性：你现在可以通过更改组件配置文件来仅 *替换* 基础结构。 不需要更改代码。

下面是特定构建基块提供的一些优点的更多示例：

- **服务调用**
  - 借助 Dapr 的对 [mTLS](https://blog.cloudflare.com/introducing-tls-client-auth/)的支持，服务现在通过加密通道进行通信。
  - 发生暂时性错误时，服务调用会自动重试。
  - 自动服务发现可减少服务彼此查找所需的配置量。

- **发布/订阅**
  - eShopOnContainer 包含大量自定义代码来支持 Azure 服务总线和 RabbitMQ。 开发人员使用 Azure Service Bus 进行生产和 RabbitMQ，用于本地开发和测试。 创建了一个 `IEventBus` 抽象层，以便在这些消息代理之间进行交换。 此层包括约 *700 行易出错的代码*。 带 Dapr 的更新后的实现仅需 *35 行代码*。 这是原始代码行的 **5%** ！ 更重要的是，实现既简单又易于理解。
  - eShopOnDapr 使用 Dapr 的丰富 ASP.NET Core 集成来使用发布/订阅。 将 `Topic` 属性添加到 ASP.NET Core 控制器方法来订阅消息。 因此，无需为每个消息代理编写单独的消息处理程序循环。
  - 通过 HTTP 调用将消息作为消息路由，可以使用 ASP.NET Core 中间件来添加功能，而不会引入新概念或 Sdk 来了解。

- **绑定**
  - EShopOnContainers 解决方案包含一个待办事项， *用于向客户* 发送订单确认电子邮件。 其思想是最终实现第三方电子邮件 API，如 SendGrid。 使用 Dapr，实现电子邮件通知就像配置资源绑定一样简单。 无需学习外部 Api 或 Sdk。

> [!NOTE]
> 本书的第一版中未介绍执行组件构建基块。 1.1 更新中将包含有关执行组件构建块及其与 eShopOnDapr 的集成的详尽章节。

## <a name="summary"></a>总结

本章介绍了 eShopOnDapr 引用应用程序。 这是广泛流行的 eShopOnContainers 微服务 reference 应用程序的演变。 eShopOnDapr 使用 Dapr 构建基块和组件替换大量自定义功能，大大简化了生成微服务应用程序所需的复杂性。

### <a name="references"></a>参考

- [GitHub 上的 eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)

- [GitHub 上的 eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers)

- [适用于容器化 .NET 应用程序的 .NET 微服务](https://dotnet.microsoft.com/download/e-book/microservices-architecture/pdf)

- [构建适用于 Azure 的 .NET 应用 Cloud-Native](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf)

> [!div class="step-by-step"]
> [上一页](getting-started.md)
> [下一页](state-management.md)
