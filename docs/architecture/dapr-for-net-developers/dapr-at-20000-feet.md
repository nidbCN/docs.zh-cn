---
title: 均分布有 Dapr
description: Dapr 是什么、它的作用以及它的工作原理的简要概述。
author: robvet
ms.date: 02/17/2021
ms.openlocfilehash: 6e5d73f8c49ecb3eec518986e2af60c68195c0ab
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881076"
---
# <a name="dapr-at-20000-feet"></a>均分布有 Dapr

第1章介绍了分布式微服务应用程序的吸引力。 但我们也指出，它们大幅增加了体系结构和操作的复杂性。 考虑到这一点，问题变成了，您如何 "让您的蛋糕更好？"。 也就是说，如何充分利用分布式体系结构的灵活性，并将其复杂性降到最低？

Dapr 或 *分布式应用程序运行时* 是一种用于构建新式分布式应用程序的新方法。

作为原型开始的内容发展为一个高度成功的开源项目。 Microsoft 的赞助商与客户和开源社区密切合作，设计和生成 Dapr。 Dapr 项目将所有背景的开发人员组合在一起，以解决开发分布式应用程序的一些棘手挑战。

本书从 .NET 开发人员的角度探讨了 Dapr。 在本章中，你将构建概念上了解 Dapr 及其工作原理。 稍后，我们将提供有关如何在应用程序中使用 Dapr 的实际实践说明。

以20000英尺的速度想象飞行。 查看窗口，从大角度看看下面的布局。 让我们对 Dapr 执行相同的操作。 在20000英尺内通过 Dapr 实现自己的飞行。 你会看到什么？

## <a name="dapr-and-the-problem-it-solves"></a>Dapr 以及它解决的问题

Dapr 解决了新式分布式应用程序固有的巨大挑战： **复杂性**。

通过可插入组件的体系结构，Dapr 大大简化了分布式应用程序背后的管道。 它提供一个使用 Dapr 运行时中的基础结构功能来绑定应用程序的 **动态胶水** 。

考虑是否需要使你的某个服务有状态？ 设计方式。 可以编写面向状态存储（如 Redis 缓存）的自定义代码。 但是，Dapr 提供现成的状态管理功能。 服务通过 Dapr **组件配置** yaml 文件调用动态绑定到状态存储 **组件** 的 Dapr 状态管理 **构建基块**。 Dapr 附带几个预建的状态存储组件，包括 Redis。 在此模型中，服务将状态管理委托给 Dapr 运行时。 你的服务没有 SDK、库或对基础组件的直接引用。 你甚至可以将状态存储（例如，Redis）更改为 MySQL 或 Cassandra，而不会更改代码。

图2-1 显示了20000英尺的 Dapr。

![Dapr 20000 ](./media/dapr-at-20000-feet/dapr-high-level.png)
 **2-1** 英尺。 Dapr 20000 英尺。

请注意，在该图的第一行中，Dapr 如何为常见开发平台提供特定于语言的 Sdk。 Dapr 1.0 包括支持中转、Node.js、Python、.NET、Java 和 JavaScript。 本书重点介绍 Dapr .NET SDK，该 SDK 还提供对 ASP.NET Core 集成的直接支持。

尽管语言特定的 Sdk 增强了开发人员体验，但 Dapr 不限平台。 在后台，Dapr 的编程模型通过标准 HTTP/gRPC 通信协议公开功能。 任何编程平台都可以通过其本机 HTTP 和 gRPC Api 调用 Dapr。  

图中心的蓝色框表示 Dapr 构建基块。 每个都公开您的应用程序可以使用的分布式应用程序功能。

下一行重点介绍了 Dapr 的可移植性，以及它可在其上运行的不同环境。

## <a name="dapr-architecture"></a>Dapr 体系结构

此时，jet 会在 Dapr 中翻过来并向下飞行，使您更深入地了解 Dapr 的工作方式。

### <a name="building-blocks"></a>构建基块

从新的角度来看，您可以看到 Dapr **构建基块** 的更详细视图。

构建基块封装分布式基础结构功能。 可以通过 HTTP 或 gRPC Api 访问该功能。 图2-2 显示了 1.0 Dapr 的可用块。

![Dapr 构建基块](./media/dapr-at-20000-feet/building-blocks.png)

**图 2-2**。 Dapr 构建基块。

下表描述了每个块提供的基础结构服务。

| 构建基块 | 说明 |
|----------------|-------------|
| [状态管理](state-management.md) | 支持长时间运行有状态服务的上下文信息。 |
| [服务调用](service-invocation.md) | 使用平台不可知的协议和众所周知的终结点调用直接、安全的服务到服务调用。 |
| [发布和订阅](publish-subscribe.md) | 在服务之间实现安全的可缩放发布/订阅消息传送。 |
| [绑定](bindings.md) | 通过与双向通信的外部资源引发的事件触发代码。 |
| [可观察性](observability.md) | 监视和度量跨网络服务的消息调用。 |
| [机密](secrets.md) | 安全访问外部密钥存储。 |
| 执行组件 | 在可重用的执行组件对象中封装逻辑和数据。 |

构建基块从服务中抽象出分布式应用程序功能的实现。 图2-3 显示了这种交互。

![Dapr 构建基块集成](./media/dapr-at-20000-feet/building-blocks-integration.png)

**图 2-3**。 Dapr 构建基块集成。

构建基块调用为每个资源提供具体实现的 Dapr 组件。 服务的代码仅知道构建基块。 它不会依赖于外部 Sdk 或库，Dapr 会为你处理管道。 每个构建基块都是独立的。 你可以在应用程序中使用一个、部分或全部。 作为增值，Dapr 构建块制作为行业最佳实践，包括综合性可观察性。

我们在后面的章节中提供每个 Dapr 构建基块的详细说明和代码示例。 此时，jet 更进一步。 从新的角度来看，你现在可以更深入地了解 Dapr 组件层。

### <a name="components"></a>组件

尽管构建块公开了一个 API 来调用分布式应用程序功能，但 Dapr 组件提供了具体的实现以使其发生。

请考虑 Dapr **状态存储** 组件。 它提供一种统一的方法来管理 CRUD 操作的状态。 如果你的服务代码没有任何更改，你可以在以下任何 Dapr 状态组件之间切换：

- AWS DynamoDB
- Aerospike
- Azure Blob 存储
- Azure CosmosDB
- Azure 表存储
- Cassandra
- Cloud Firestore (数据存储模式) 
- CloudState
- Couchbase
- Etcd
- HashiCorp Consul
- Hazelcast
- Memcached
- MongoDB
- PostgreSQL
- Redis
- RethinkDB
- SQL Server
- Zookeeper

每个组件都通过通用状态管理接口提供必要的实现：

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
> Dapr 运行时以及所有 Dapr 组件都已使用 Golang 或中转语言编写。 转向在开源社区中使用一种流行的语言，并证实 Dapr 的跨平台承诺。

也许你开始将 Azure Redis 缓存用作状态存储。 用以下配置指定它：

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

在 " **规范** " 部分中，将 Dapr 配置为使用 Redis 缓存进行状态管理。 节还包含组件特定的元数据。 在这种情况下，可以使用它来配置其他 Redis 设置。

稍后，应用程序就可以进入生产环境了。 对于生产环境，可能需要将状态管理更改为 Azure 表存储。 Azure 表存储提供经济实惠且持久的状态管理功能。

撰写本文时，Dapr 提供以下组件类型：

| 组件 | 说明 |
|-----------|-------------|
| [服务发现](https://github.com/dapr/components-contrib/tree/master/nameresolution) | 由服务调用构建基块用于与宿主环境集成以提供服务到服务发现。 |
| [State](https://github.com/dapr/components-contrib/tree/master/state) | 提供统一的接口以与各种状态存储实现交互。 |
| [发布/订阅](https://github.com/dapr/components-contrib/tree/master/pubsub) | 提供统一的接口以与各种消息总线实现交互。 |
| [绑定](https://github.com/dapr/components-contrib/tree/master/bindings) | 提供了一个统一的接口，用于从外部系统触发应用程序事件并调用具有可选数据负载的外部系统。 |
| [中间件](https://github.com/dapr/components-contrib/tree/master/middleware) | 允许自定义中间件插入请求处理管道，并调用请求或响应的其他操作。 |
| [密钥存储](https://github.com/dapr/components-contrib/tree/master/secretstores) | 提供统一的接口以与外部机密存储区交互，包括云、边缘、商业和开源服务。 |

当 jet 在 Dapr 上完成其工作时，您再次看一遍，可以看到它如何连接在一起。

### <a name="sidecar-architecture"></a>挎斗体系结构

Dapr 通过 [挎斗体系结构](/azure/architecture/patterns/sidecar)公开其构建基块和组件。 挎斗使 Dapr 能够在单独的内存进程中运行，或者在单独的容器中运行。 分支提供隔离和封装，因为它们不是服务的一部分，而是连接到它。 这种隔离使每个都具有其自己的运行时环境，并在不同的编程平台上构建。 图2-4 显示了挎斗模式。

![挎斗体系结构](./media/dapr-at-20000-feet/sidecar-generic.png)

**图 2-4**。 挎斗体系结构。

此模式之所以称作“挎斗”(Sidecar)，是因为它类似于三轮摩托车上的挎斗。 在上图中，请注意 Dapr 挎斗如何附加到服务以提供分布式应用程序功能。

### <a name="hosting-environments"></a>宿主环境

Dapr 提供跨平台支持，可以在许多不同的环境中运行。 这些环境包括 Kubernetes、一组 Vm 或边缘环境，如 Azure IoT Edge。

对于本地开发，开始的最简单方法是采用 [自承载模式](https://docs.dapr.io/concepts/overview/#self-hosted)。 在自承载模式下，微服务和 Dapr 分支在无容器 orchestrator （如 Kubernetes）的独立本地进程中运行。 有关详细信息，请参阅 [下载并安装 DAPR CLI](https://docs.dapr.io/getting-started/install-dapr/)。

图2-5 显示了一个应用程序和 Dapr，它托管在两个独立的内存进程中，通过 HTTP 或 gRPC 进行通信。

![自承载挎斗体系结构](./media/dapr-at-20000-feet/self-hosted-dapr-sidecar.png)

**图 2-5**。 自承载的 Dapr 挎斗。

默认情况下，Dapr 安装用于 Redis 和 Zipkin 的 Docker 容器，提供默认状态管理和可观察性。 如果不想在本地计算机上安装 Docker，甚至可以 [在没有任何 Docker 容器的自承载模式下运行 Dapr](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/)。 但是，必须手动安装 Redis 的默认组件，例如状态管理和发布/订阅。

Dapr 也在 [容器化环境](https://docs.dapr.io/concepts/overview/#kubernetes-hosted)（如 Kubernetes）中运行。 图2-6 显示了在单独的侧面车载容器中运行的 Dapr，以及同一 Kubernetes pod 中的应用程序容器。

![Kubernetes 托管的挎斗体系结构](./media/dapr-at-20000-feet/kubernetes-hosted-dapr-sidecar.png)

**图 2-6**。 Kubernetes 托管的 Dapr 挎斗。

## <a name="dapr-performance-considerations"></a>Dapr 性能注意事项

如您所见，Dapr 公开了挎斗体系结构，以便将应用程序与分布式应用程序功能分离。 调用 Dapr 操作需要至少一个进程外网络调用。 图2-7 显示了 Dapr 流量模式的示例。

![Dapr 流量模式](./media/dapr-at-20000-feet/dapr-traffic-patterns.png)

**图 2-7**。 Dapr 流量模式。

查看上一个图，可能会询问每次调用产生的延迟和开销。  

Dapr 团队投入了很高的性能。 大量的工程努力使 Dapr 的效率更高。 Dapr 分支之间的调用始终与 gRPC 一起建立，后者提供高性能和小型二进制负载。 在大多数情况下，额外的开销应为毫秒。

若要提高性能，开发人员可通过 gRPC 调用 Dapr 构建基块。

gRPC 是一种新式的高性能框架， (RPC) 协议演变旧的 [远程过程调用 ](https://en.wikipedia.org/wiki/Remote_procedure_call) 。 gRPC 使用 HTTP/2 作为其传输协议，通过 HTTP RESTFul 服务提供显著的性能改进，包括：

- 多路复用支持通过同一连接发送多个并行请求-HTTP 1.1 限制处理一次请求/响应消息。
- 同时发送客户端请求和服务器响应的双向全双工通信。
- 内置流式处理可对大型数据集进行异步流式处理的请求和响应。

若要了解详细信息，请参阅 Azure 电子书的[架构 Cloud-Native .Net 应用](../cloud-native/index.md)中的[gRPC 概述](../cloud-native/grpc.md#what-is-grpc)。  

## <a name="dapr-and-service-meshes"></a>Dapr 和服务网格

服务网格是针对分布式应用程序的另一种快速发展的技术。

服务网格是一个可配置的基础结构层，其中内置了用于处理服务到服务通信、复原、负载平衡和遥测捕获的功能。 它会将这些问题的责任转移到服务，并将其移到服务网格层中。 与 Dapr 一样，服务网格还遵循挎斗体系结构。

图2-8 显示了实现服务网格技术的应用程序。

![服务网格](./media/dapr-at-20000-feet/service-mesh-with-side-car.png)

**图 2-8**。 带有侧面汽车的服务网格。

上图显示了通过与每个服务一起运行的挎斗代理来截获消息的方式。 每个代理都可以配置特定于该服务的流量规则。 它了解消息，并可以将消息路由到您的服务和外界。

那么，问题变成 "正在 Dapr 服务网格？"。

尽管这两种方法都使用挎斗体系结构，但每种技术都有不同的用途。 Dapr 提供分布式应用程序功能。 服务网格提供专用的网络基础结构层。

每个在不同级别工作时，都可以在同一应用程序中协同工作。 例如，服务网格可以提供服务之间的网络通信。 Dapr 可以提供应用程序服务，例如状态管理或执行组件服务。

图2-9 显示了实现 Dapr 和服务网格技术的应用程序。

![Dapr 和 Service 网格一起](./media/dapr-at-20000-feet/dapr-and-service-mesh.png)

**图 2-9**。 将 Dapr 和 service 网格组合在一起。

[Dapr 联机文档](https://docs.dapr.io/concepts/faq/#networking-and-service-meshes)介绍 Dapr 和服务网格集成。

## <a name="summary"></a>总结

本章介绍了 Dapr 的分布式应用程序运行时。

Dapr 是由 Microsoft 赞助的开源项目，可与客户和开源社区密切合作。

Dapr 的核心有助于降低分布式微服务应用程序的固有复杂性。 它以构建块 Api 的概念为基础构建。 Dapr 构建基块公开常见的分布式应用程序功能，例如状态管理、服务到服务调用和发布/订阅消息传送。 Dapr 组件位于构建基块下，并为每个功能提供具体的实现。 应用程序通过配置文件绑定到各种组件。

在下面的章节中，我们提供了有关如何在应用程序中使用 Dapr 的实用实践说明。

### <a name="references"></a>参考

- [Dapr 文档](https://dapr.io/)
- [学习 Dapr](https://www.amazon.com/Learning-Dapr-Building-Distributed-Applications/dp/1492072427/ref=sr_1_1?dchild=1&keywords=dapr&qid=1604794794&sr=8-1)
- [.NET 微服务：容器化 .NET 应用程序的体系结构](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)
- [构建适用于 Azure 的 .NET 应用 Cloud-Native](https://dotnet.microsoft.com/download/e-book/cloud-native-azure/pdf)

> [!div class="step-by-step"]
> [上一页](the-world-is-distributed.md)
> [下一页](getting-started.md)
