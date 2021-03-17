---
title: Dapr 状态管理构建基块
description: 说明状态管理构建基块、功能、优点以及如何应用它。
author: amolenk
ms.date: 02/17/2021
ms.reviewer: robvet
ms.openlocfilehash: 67b7f839ccbe24752281fb537b0473d4984d9e37
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623923"
---
# <a name="the-dapr-state-management-building-block"></a><span data-ttu-id="e8466-103">Dapr 状态管理构建基块</span><span class="sxs-lookup"><span data-stu-id="e8466-103">The Dapr state management building block</span></span>

<span data-ttu-id="e8466-104">分布式应用程序由独立的服务组成。</span><span class="sxs-lookup"><span data-stu-id="e8466-104">Distributed applications are composed of independent services.</span></span> <span data-ttu-id="e8466-105">虽然每个服务都应该是无状态的，但某些服务必须跟踪状态才能完成业务运营。</span><span class="sxs-lookup"><span data-stu-id="e8466-105">While each service should be stateless, some services must track state to complete business operations.</span></span> <span data-ttu-id="e8466-106">请考虑电子商务网站的购物篮服务。</span><span class="sxs-lookup"><span data-stu-id="e8466-106">Consider a shopping basket service for an e-Commerce site.</span></span> <span data-ttu-id="e8466-107">如果服务无法跟踪状态，则客户可能会通过离开网站使购物篮内容松动，导致销售丢失和客户不满。</span><span class="sxs-lookup"><span data-stu-id="e8466-107">If the service can't track state, the customer could loose the shopping basket content by leaving the website, resulting in a lost sale and an unhappy customer experience.</span></span> <span data-ttu-id="e8466-108">对于这些情况，需要将状态保存到分布式状态存储中。</span><span class="sxs-lookup"><span data-stu-id="e8466-108">For these scenarios, state needs to be persisted to a distributed state store.</span></span> <span data-ttu-id="e8466-109">[Dapr 状态管理构建块](https://docs.dapr.io/developing-applications/building-blocks/state-management/)简化了状态跟踪，并提供了跨各种数据存储的高级功能。</span><span class="sxs-lookup"><span data-stu-id="e8466-109">The [Dapr state management building block](https://docs.dapr.io/developing-applications/building-blocks/state-management/) simplifies state tracking and offers advanced features across various data stores.</span></span>

<span data-ttu-id="e8466-110">若要试用状态管理构建基块，请参阅 [第3章中的计数器应用程序示例](getting-started.md)。</span><span class="sxs-lookup"><span data-stu-id="e8466-110">To try out the state management building block, have a look at the [counter application sample in chapter 3](getting-started.md).</span></span>

## <a name="what-it-solves"></a><span data-ttu-id="e8466-111">解决方法</span><span class="sxs-lookup"><span data-stu-id="e8466-111">What it solves</span></span>

<span data-ttu-id="e8466-112">分布式应用程序中的跟踪状态可能具有挑战性。</span><span class="sxs-lookup"><span data-stu-id="e8466-112">Tracking state in a distributed application can be challenging.</span></span> <span data-ttu-id="e8466-113">例如：</span><span class="sxs-lookup"><span data-stu-id="e8466-113">For example:</span></span>

- <span data-ttu-id="e8466-114">应用程序可能需要不同类型的数据存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-114">The application may require different types of data stores.</span></span>
- <span data-ttu-id="e8466-115">访问和更新数据时可能需要不同的一致性级别。</span><span class="sxs-lookup"><span data-stu-id="e8466-115">Different consistency levels may be required for accessing and updating data.</span></span>
- <span data-ttu-id="e8466-116">多个用户可以同时更新数据，需要冲突解决。</span><span class="sxs-lookup"><span data-stu-id="e8466-116">Multiple users may update data at the same time, requiring  conflict resolution.</span></span>
- <span data-ttu-id="e8466-117">在与数据存储交互时，服务必须重试发生的任何短暂的 [暂时性错误](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) 。</span><span class="sxs-lookup"><span data-stu-id="e8466-117">Services must retry any short-lived [transient errors](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) that  occur while interacting with the data store.</span></span>

<span data-ttu-id="e8466-118">Dapr 状态管理构建块解决了这些难题。</span><span class="sxs-lookup"><span data-stu-id="e8466-118">The Dapr state management building block addresses these challenges.</span></span> <span data-ttu-id="e8466-119">它简化了跟踪状态，而无需依赖关系或学习曲线在第三方存储 Sdk 上。</span><span class="sxs-lookup"><span data-stu-id="e8466-119">It streamlines tracking state without dependencies or a learning curve on third-party storage SDKs.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e8466-120">Dapr 状态管理提供 [密钥/值](/azure/architecture/guide/technology-choices/data-store-overview#keyvalue-stores) API。</span><span class="sxs-lookup"><span data-stu-id="e8466-120">Dapr state management offers a [key/value](/azure/architecture/guide/technology-choices/data-store-overview#keyvalue-stores) API.</span></span> <span data-ttu-id="e8466-121">此功能不支持关系数据存储或图形数据存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-121">The feature doesn't support relational or graph data storage.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="e8466-122">工作原理</span><span class="sxs-lookup"><span data-stu-id="e8466-122">How it works</span></span>

<span data-ttu-id="e8466-123">应用程序与 Dapr 挎斗交互，以存储和检索键/值数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-123">The application interacts with a Dapr sidecar to store and retrieve key/value data.</span></span> <span data-ttu-id="e8466-124">在后台，挎斗 API 使用可配置的状态存储组件来持久保存数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-124">Under the hood, the sidecar API consumes a configurable state store component to persist data.</span></span> <span data-ttu-id="e8466-125">开发人员可以从不断增长的 [支持状态存储](https://docs.dapr.io/operations/components/setup-state-store/supported-state-stores/) 集合中进行选择，其中包括 Azure Cosmos DB、SQL Server 和 Cassandra。</span><span class="sxs-lookup"><span data-stu-id="e8466-125">Developers can choose from a growing collection of [supported state stores](https://docs.dapr.io/operations/components/setup-state-store/supported-state-stores/) that include Azure Cosmos DB, SQL Server, and Cassandra.</span></span>

<span data-ttu-id="e8466-126">可以通过 HTTP 或 gRPC 调用 API。</span><span class="sxs-lookup"><span data-stu-id="e8466-126">The API can be called with either HTTP or gRPC.</span></span> <span data-ttu-id="e8466-127">使用以下 URL 调用 HTTP API：</span><span class="sxs-lookup"><span data-stu-id="e8466-127">Use the following URL to call the HTTP API:</span></span>

```http
http://localhost:<dapr-port>/v1.0/state/<store-name>/
```

- <span data-ttu-id="e8466-128">`<dapr-port>`： Dapr 侦听的 HTTP 端口。</span><span class="sxs-lookup"><span data-stu-id="e8466-128">`<dapr-port>`: the HTTP port that Dapr listens on.</span></span>
- <span data-ttu-id="e8466-129">`<store-name>`：要使用的状态存储组件的名称。</span><span class="sxs-lookup"><span data-stu-id="e8466-129">`<store-name>`: the name of the state store component to use.</span></span>

<span data-ttu-id="e8466-130">图5-1 显示了 Dapr 的购物篮服务如何使用名为的 Dapr 状态存储组件来存储键/值对 `statestore` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-130">Figure 5-1 shows how a Dapr-enabled shopping basket service stores a key/value pair using the Dapr state store component named `statestore`.</span></span>

![在 Dapr 状态存储中存储键/值对的关系图。](media/state-management/state-management-flow.png)

<span data-ttu-id="e8466-132">**图 5-1**。</span><span class="sxs-lookup"><span data-stu-id="e8466-132">**Figure 5-1**.</span></span> <span data-ttu-id="e8466-133">在 Dapr 状态存储中存储键/值对。</span><span class="sxs-lookup"><span data-stu-id="e8466-133">Storing a key/value pair in a Dapr state store.</span></span>

<span data-ttu-id="e8466-134">请注意上图中的步骤：</span><span class="sxs-lookup"><span data-stu-id="e8466-134">Note the steps in the previous figure:</span></span>

1. <span data-ttu-id="e8466-135">购物篮服务在 Dapr 挎斗上调用状态管理 API。</span><span class="sxs-lookup"><span data-stu-id="e8466-135">The basket service calls the state management API on the Dapr sidecar.</span></span> <span data-ttu-id="e8466-136">请求正文包含一个可包含多个键/值对的 JSON 数组。</span><span class="sxs-lookup"><span data-stu-id="e8466-136">The body of the request encloses a JSON array that can contain multiple key/value pairs.</span></span>
1. <span data-ttu-id="e8466-137">Dapr 挎斗基于组件配置文件确定状态存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-137">The Dapr sidecar determines the state store based on the component configuration file.</span></span> <span data-ttu-id="e8466-138">在这种情况下，它是一个 Redis 缓存状态存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-138">In this case, it's a Redis cache state store.</span></span>
1. <span data-ttu-id="e8466-139">挎斗将数据保留到 Redis 缓存。</span><span class="sxs-lookup"><span data-stu-id="e8466-139">The sidecar persists the data to the Redis cache.</span></span>

<span data-ttu-id="e8466-140">检索存储的数据是类似的 API 调用。</span><span class="sxs-lookup"><span data-stu-id="e8466-140">Retrieving the stored data is a similar API call.</span></span> <span data-ttu-id="e8466-141">在下面的示例中， *卷* 命令通过调用 DAPR 挎斗 API 来检索数据：</span><span class="sxs-lookup"><span data-stu-id="e8466-141">In the example below, a *curl* command retrieves the data by calling the Dapr sidecar API:</span></span>

```console
curl http://localhost:3500/v1.0/state/statestore/basket1
```

<span data-ttu-id="e8466-142">此命令将在响应正文中返回已存储状态：</span><span class="sxs-lookup"><span data-stu-id="e8466-142">The command returns the stored state in the response body:</span></span>

```json
{
  "items": [
    {
      "itemId": "DaprHoodie",
      "quantity": 1
    }
  ],
  "customerId": 1
}
```

<span data-ttu-id="e8466-143">以下各节介绍如何使用状态管理构建基块的更高级的功能。</span><span class="sxs-lookup"><span data-stu-id="e8466-143">The following sections explain how to use the more advanced features of the state management building block.</span></span>

### <a name="consistency"></a><span data-ttu-id="e8466-144">一致性</span><span class="sxs-lookup"><span data-stu-id="e8466-144">Consistency</span></span>

<span data-ttu-id="e8466-145">[CAP 定理](https://en.wikipedia.org/wiki/CAP_theorem)是一组适用于存储状态的分布式系统的原则。</span><span class="sxs-lookup"><span data-stu-id="e8466-145">The [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) is a set of principles that apply to distributed systems that store state.</span></span> <span data-ttu-id="e8466-146">图5-2 显示了 CAP 定理的三个属性。</span><span class="sxs-lookup"><span data-stu-id="e8466-146">Figure 5-2 shows the three properties of the CAP theorem.</span></span>

![CAP 定理。](media/state-management/cap-theorem.png)

<span data-ttu-id="e8466-148">**图 5-2**。</span><span class="sxs-lookup"><span data-stu-id="e8466-148">**Figure 5-2**.</span></span> <span data-ttu-id="e8466-149">CAP 定理。</span><span class="sxs-lookup"><span data-stu-id="e8466-149">The CAP theorem.</span></span>

<span data-ttu-id="e8466-150">定理指出，分布式数据系统提供一致性、可用性和分区容差之间的权衡。</span><span class="sxs-lookup"><span data-stu-id="e8466-150">The theorem states that distributed data systems offer a trade-off between consistency, availability, and partition tolerance.</span></span> <span data-ttu-id="e8466-151">而且，任何数据存储只能 *保证三个属性中的两个*：</span><span class="sxs-lookup"><span data-stu-id="e8466-151">And, that any datastore can only *guarantee two of the three properties*:</span></span>

- <span data-ttu-id="e8466-152">*一致性* (**C**) 。</span><span class="sxs-lookup"><span data-stu-id="e8466-152">*Consistency* (**C**).</span></span> <span data-ttu-id="e8466-153">群集中的每个节点都将使用最新的数据做出响应，即使在所有副本更新之前，系统都必须阻止请求。</span><span class="sxs-lookup"><span data-stu-id="e8466-153">Every node in the cluster responds with the most recent data, even if the system must block the request until all replicas update.</span></span> <span data-ttu-id="e8466-154">如果查询当前正在更新的项的 "一致性系统"，则在所有副本都成功更新之前，将不会收到响应。</span><span class="sxs-lookup"><span data-stu-id="e8466-154">If you query a "consistent system" for an item that is currently updating, you won't get a response until all replicas successfully update.</span></span> <span data-ttu-id="e8466-155">但是，您将始终接收最新的数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-155">However, you'll always receive the most current data.</span></span>

- <span data-ttu-id="e8466-156">*可用性* (**)** 。</span><span class="sxs-lookup"><span data-stu-id="e8466-156">*Availability* (**A**).</span></span> <span data-ttu-id="e8466-157">即使该响应不是最新的数据，每个节点都将返回立即响应。</span><span class="sxs-lookup"><span data-stu-id="e8466-157">Every node returns an immediate response, even if that response isn't the most recent data.</span></span> <span data-ttu-id="e8466-158">如果您在 "可用系统" 中查询正在更新的项，则您将获得该服务在此时可以提供的最佳可能的答案。</span><span class="sxs-lookup"><span data-stu-id="e8466-158">If you query an "available system" for an item that is updating, you'll get the best possible answer the service can provide at that moment.</span></span>

- <span data-ttu-id="e8466-159">*分区容差* (**P**) 。</span><span class="sxs-lookup"><span data-stu-id="e8466-159">*Partition Tolerance* (**P**).</span></span> <span data-ttu-id="e8466-160">即使复制的数据节点发生故障或失去与其他复制的数据节点的连接，保证系统仍可继续运行。</span><span class="sxs-lookup"><span data-stu-id="e8466-160">Guarantees the system continues to operate even if a replicated data node fails or loses connectivity with other replicated data nodes.</span></span>

<span data-ttu-id="e8466-161">分布式应用程序必须处理 **P** 属性。</span><span class="sxs-lookup"><span data-stu-id="e8466-161">Distributed applications must handle the **P** property.</span></span> <span data-ttu-id="e8466-162">随着服务彼此间的网络调用通信，会发生网络中断 (**P**) 。</span><span class="sxs-lookup"><span data-stu-id="e8466-162">As services communicate among each other with network calls, network disruptions (**P**) will occur.</span></span> <span data-ttu-id="e8466-163">考虑到这一点，分布式应用程序必须是 **AP** 或 **CP**。</span><span class="sxs-lookup"><span data-stu-id="e8466-163">With that in mind, distributed applications must either be **AP** or **CP**.</span></span>

<span data-ttu-id="e8466-164">**AP** 应用程序选择 "可用性一致性"。</span><span class="sxs-lookup"><span data-stu-id="e8466-164">**AP** applications choose availability over consistency.</span></span> <span data-ttu-id="e8466-165">Dapr 通过其 **最终一致性** 策略支持此选择。</span><span class="sxs-lookup"><span data-stu-id="e8466-165">Dapr supports this choice with its **eventual consistency** strategy.</span></span> <span data-ttu-id="e8466-166">请考虑使用基础数据存储（例如 Azure CosmosDB）将冗余数据存储在多个副本上。</span><span class="sxs-lookup"><span data-stu-id="e8466-166">Consider an underlying data store, such as Azure CosmosDB, which stores redundant data on multiple replicas.</span></span> <span data-ttu-id="e8466-167">对于最终一致性，状态存储会将更新写入一个副本并完成与客户端的写入请求。</span><span class="sxs-lookup"><span data-stu-id="e8466-167">With eventual consistency, the state store writes the update to one replica and completes the write request with the client.</span></span> <span data-ttu-id="e8466-168">此时间过后，存储将以异步方式更新其副本。</span><span class="sxs-lookup"><span data-stu-id="e8466-168">After this time, the store will asynchronously update its replicas.</span></span> <span data-ttu-id="e8466-169">读取请求可以返回任何副本的数据，包括尚未收到最新更新的副本。</span><span class="sxs-lookup"><span data-stu-id="e8466-169">Read requests can return data from any of the replicas, including those replicas that haven't yet received the latest update.</span></span>

<span data-ttu-id="e8466-170">**CP** 应用程序选择一致性和可用性。</span><span class="sxs-lookup"><span data-stu-id="e8466-170">**CP** applications choose consistency over availability.</span></span> <span data-ttu-id="e8466-171">Dapr 通过其 **强一致性** 策略支持此选择。</span><span class="sxs-lookup"><span data-stu-id="e8466-171">Dapr supports this choice with its **strong consistency** strategy.</span></span> <span data-ttu-id="e8466-172">在此方案中，状态存储将同步更新 *所有* (或在某些情况下，在完成写入请求 *之前*) 必需副本的 *仲裁*。</span><span class="sxs-lookup"><span data-stu-id="e8466-172">In this scenario, the state store will synchronously update *all* (or, in some cases, a *quorum* of) required replicas *before* completing the write request.</span></span> <span data-ttu-id="e8466-173">读取操作将跨副本持续返回最新数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-173">Read operations will return the most up-to-date data consistently across replicas.</span></span>

<span data-ttu-id="e8466-174">通过将 *一致性提示* 附加到操作来指定状态操作的一致性级别。</span><span class="sxs-lookup"><span data-stu-id="e8466-174">The consistency level for a state operation is specified by attaching a *consistency hint* to the operation.</span></span> <span data-ttu-id="e8466-175">以下 *卷* 命令 `Hello=World` 使用强一致性提示向状态存储写入一个键/值对：</span><span class="sxs-lookup"><span data-stu-id="e8466-175">The following *curl* command writes a `Hello=World` key/value pair to a state store using a strong consistency hint:</span></span>

```console
curl -X POST http://localhost:3500/v1.0/state/<store-name> \
  -H "Content-Type: application/json" \
  -d '[
        {
          "key": "Hello",
          "value": "World",
          "options": {
            "consistency": "strong"
          }
        }
      ]'
```

> [!IMPORTANT]
> <span data-ttu-id="e8466-176">它用于满足附加到操作的一致性提示，由 Dapr 状态存储组件完成。</span><span class="sxs-lookup"><span data-stu-id="e8466-176">It is up to the Dapr state store component to fulfill the consistency hint attached to the operation.</span></span> <span data-ttu-id="e8466-177">并非所有数据存储都支持这两种一致性级别。</span><span class="sxs-lookup"><span data-stu-id="e8466-177">Not all data stores support both consistency levels.</span></span> <span data-ttu-id="e8466-178">如果未设置任何一致性提示，则默认行为为 **最终** 状态。</span><span class="sxs-lookup"><span data-stu-id="e8466-178">If no consistency hint is set, the default behavior is **eventual**.</span></span>

### <a name="concurrency"></a><span data-ttu-id="e8466-179">并发</span><span class="sxs-lookup"><span data-stu-id="e8466-179">Concurrency</span></span>

<span data-ttu-id="e8466-180">在多用户应用程序中，有可能多个用户同时更新同一时间)  (相同的数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-180">In a multi-user application, there's a chance that multiple users will update the same data concurrently (at the same time).</span></span> <span data-ttu-id="e8466-181">Dapr 支持乐观并发控制 (OCC) 来管理冲突。</span><span class="sxs-lookup"><span data-stu-id="e8466-181">Dapr supports optimistic concurrency control (OCC) to manage conflicts.</span></span> <span data-ttu-id="e8466-182">OCC 基于一个假设，因为用户处理数据的不同部分，所以更新冲突很少见。</span><span class="sxs-lookup"><span data-stu-id="e8466-182">OCC is based on an assumption that update conflicts are uncommon because users work on different parts of the data.</span></span> <span data-ttu-id="e8466-183">更有效的方法是将更新成功，如果不成功，则重试。</span><span class="sxs-lookup"><span data-stu-id="e8466-183">It's more efficient to assume an update will succeed and retry if it doesn't.</span></span> <span data-ttu-id="e8466-184">实现悲观锁定的替代方法可能会影响长时间运行的锁定，导致数据争用。</span><span class="sxs-lookup"><span data-stu-id="e8466-184">The alternative, implementing pessimistic locking, can affect performance with long-running locking causing data contention.</span></span>

<span data-ttu-id="e8466-185">Dapr 支持使用 Etag)  (OCC 的乐观并发控制。</span><span class="sxs-lookup"><span data-stu-id="e8466-185">Dapr supports optimistic concurrency control (OCC) using ETags.</span></span> <span data-ttu-id="e8466-186">ETag 是与存储的键/值对的特定版本相关联的值。</span><span class="sxs-lookup"><span data-stu-id="e8466-186">An ETag is a value associated with a specific version of a stored key/value pair.</span></span> <span data-ttu-id="e8466-187">键/值对的每次更新时，ETag 值也会更新。</span><span class="sxs-lookup"><span data-stu-id="e8466-187">Each time a key/value pair updates, the ETag value updates as well.</span></span> <span data-ttu-id="e8466-188">当客户端检索键/值对时，响应包括当前 ETag 值。</span><span class="sxs-lookup"><span data-stu-id="e8466-188">When a client retrieves a key/value pair, the response includes the current ETag value.</span></span> <span data-ttu-id="e8466-189">当客户端更新或删除键/值对时，它必须在请求正文中发送回该 ETag 值。</span><span class="sxs-lookup"><span data-stu-id="e8466-189">When a client updates or deletes a key/value pair, it must send that ETag value back in the request body.</span></span> <span data-ttu-id="e8466-190">如果其他客户端同时更新了数据，则 Etag 不会匹配，请求将失败。</span><span class="sxs-lookup"><span data-stu-id="e8466-190">If another client has updated the data in the meantime, the ETags won't match and the request will fail.</span></span> <span data-ttu-id="e8466-191">此时，客户端必须检索更新的数据，重新进行更改，然后重新提交更新。</span><span class="sxs-lookup"><span data-stu-id="e8466-191">At this point, the client must retrieve the updated data, make the change again, and resubmit the update.</span></span> <span data-ttu-id="e8466-192">此策略称为 **第一次写入-wins**。</span><span class="sxs-lookup"><span data-stu-id="e8466-192">This strategy is called **first-write-wins**.</span></span>

<span data-ttu-id="e8466-193">Dapr 还支持 **最后写入 wins** 策略。</span><span class="sxs-lookup"><span data-stu-id="e8466-193">Dapr also supports a **last-write-wins** strategy.</span></span> <span data-ttu-id="e8466-194">使用此方法时，客户端不会将 ETag 附加到写入请求。</span><span class="sxs-lookup"><span data-stu-id="e8466-194">With this approach, the client doesn't attach an ETag to the write request.</span></span> <span data-ttu-id="e8466-195">状态存储组件将始终允许更新，即使基础值在会话期间已更改也是如此。</span><span class="sxs-lookup"><span data-stu-id="e8466-195">The state store component will always allow the update, even if the underlying value has changed during the session.</span></span> <span data-ttu-id="e8466-196">最后写入-wins 对于数据争用较少的高吞吐量写入方案非常有用。</span><span class="sxs-lookup"><span data-stu-id="e8466-196">Last-write-wins is useful for high-throughput write scenarios with low data contention.</span></span> <span data-ttu-id="e8466-197">同样，可以容忍偶尔的用户更新。</span><span class="sxs-lookup"><span data-stu-id="e8466-197">As well, overwriting an occasional user update can be tolerated.</span></span>

### <a name="transactions"></a><span data-ttu-id="e8466-198">事务</span><span class="sxs-lookup"><span data-stu-id="e8466-198">Transactions</span></span>

<span data-ttu-id="e8466-199">Dapr 可以将 *多项更改* 作为一个作为事务实现的操作写入数据存储区。</span><span class="sxs-lookup"><span data-stu-id="e8466-199">Dapr can write *multi-item changes* to a data store as a single operation implemented as a transaction.</span></span> <span data-ttu-id="e8466-200">此功能仅适用于支持 [ACID](https://en.wikipedia.org/wiki/ACID) 事务的数据存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-200">This functionality is only available for data stores that support [ACID](https://en.wikipedia.org/wiki/ACID) transactions.</span></span> <span data-ttu-id="e8466-201">撰写本文时，这些存储包括 Redis、MongoDB、PostgreSQL、SQL Server 和 Azure CosmosDB。</span><span class="sxs-lookup"><span data-stu-id="e8466-201">At the time of this writing, these stores include Redis, MongoDB, PostgreSQL, SQL Server, and Azure CosmosDB.</span></span>

<span data-ttu-id="e8466-202">在下面的示例中，多项操作将发送到单个事务中的状态存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-202">In the example below, a multi-item operation is sent to the state store in a single transaction.</span></span> <span data-ttu-id="e8466-203">所有操作必须成功才能提交事务。</span><span class="sxs-lookup"><span data-stu-id="e8466-203">All operations must succeed for the transaction to commit.</span></span> <span data-ttu-id="e8466-204">如果一个或多个操作失败，则将回滚整个事务。</span><span class="sxs-lookup"><span data-stu-id="e8466-204">If one or more of the operations fail, the entire transaction rolls back.</span></span>

```console
curl -X POST http://localhost:3500/v1.0/state/<store-name>/transaction \
  -H "Content-Type: application/json" \
  -d '{
        "operations": [
          {
            "operation": "upsert",
            "request": { "key": "Key1", "value": "Value1"
            }
          },
          {
            "operation": "delete",
            "request": { "key": "Key2" }
          }
        ]
      }'
```

<span data-ttu-id="e8466-205">对于不支持事务的数据存储，还可以将多个键作为单个请求发送。</span><span class="sxs-lookup"><span data-stu-id="e8466-205">For data stores that don't support transactions, multiple keys can still be sent as a single request.</span></span> <span data-ttu-id="e8466-206">下面的示例演示了一个 **大容量** 写入操作：</span><span class="sxs-lookup"><span data-stu-id="e8466-206">The following example shows a **bulk** write operation:</span></span>

```console
curl -X POST http://localhost:3500/v1.0/state/<store-name> \
  -H "Content-Type: application/json" \
  -d '[
        { "key": "Key1", "value": "Value1" },
        { "key": "Key2", "value": "Value2" }
      ]'
```

<span data-ttu-id="e8466-207">对于大容量操作，Dapr 会将每个键/值对更新作为单独的请求提交到数据存储区。</span><span class="sxs-lookup"><span data-stu-id="e8466-207">For bulk operations, Dapr will submit each key/value pair update as a separate request to the data store.</span></span>

## <a name="use-the-dapr-net-sdk"></a><span data-ttu-id="e8466-208">使用 Dapr .NET SDK</span><span class="sxs-lookup"><span data-stu-id="e8466-208">Use the Dapr .NET SDK</span></span>

<span data-ttu-id="e8466-209">Dapr .NET SDK 为 .NET Core 平台提供特定于语言的支持。</span><span class="sxs-lookup"><span data-stu-id="e8466-209">The Dapr .NET SDK provides language-specific support for .NET Core platform.</span></span> <span data-ttu-id="e8466-210">开发人员可以使用 `DaprClient` [第3章](getting-started.md) 中引入的类来读取和写入数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-210">Developers can use the `DaprClient` class introduced in [chapter 3](getting-started.md) to read and write data.</span></span> <span data-ttu-id="e8466-211">下面的示例演示如何使用 `DaprClient.GetStateAsync<TValue>` 方法从状态存储中读取数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-211">The following example shows how to use the `DaprClient.GetStateAsync<TValue>` method to read data from a state store.</span></span> <span data-ttu-id="e8466-212">此方法需要将存储名称、 `statestore` 和键 `AMS` 作为参数：</span><span class="sxs-lookup"><span data-stu-id="e8466-212">The method expects the store name, `statestore`, and key, `AMS`, as parameters:</span></span>

```csharp
var weatherForecast = await daprClient.GetStateAsync<WeatherForecast>("statestore", "AMS");
```

<span data-ttu-id="e8466-213">如果状态存储不包含键的任何数据 `AMS` ，则结果将为 `default(WeatherForecast)` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-213">If the state store contains no data for key `AMS`, the result will be `default(WeatherForecast)`.</span></span>

<span data-ttu-id="e8466-214">若要将数据写入数据存储，请使用 `DaprClient.SaveStateAsync<TValue>` 方法：</span><span class="sxs-lookup"><span data-stu-id="e8466-214">To write data to the data store, use the `DaprClient.SaveStateAsync<TValue>` method:</span></span>

```csharp
daprClient.SaveStateAsync("statestore", "AMS", weatherForecast);
```

<span data-ttu-id="e8466-215">该示例使用 **最后一个写入 wins** 策略，因为 ETag 值不传递到状态存储组件。</span><span class="sxs-lookup"><span data-stu-id="e8466-215">The example uses the **last-write-wins** strategy as an ETag value isn't passed to the state store component.</span></span> <span data-ttu-id="e8466-216">若要将乐观并发控制 (使用 **第一写 wins** 策略的 OCC) ，请首先使用方法检索当前 ETag `DaprClient.GetStateAndETagAsync` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-216">To use optimistic concurrency control (OCC) with a **first-write-wins** strategy, first retrieve the current ETag using the `DaprClient.GetStateAndETagAsync` method.</span></span> <span data-ttu-id="e8466-217">然后，使用方法编写更新的值并传递检索到的 ETag `DaprClient.TrySaveStateAsync` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-217">Then write the updated value and pass along the retrieved ETag using the `DaprClient.TrySaveStateAsync` method.</span></span>

```csharp
var (weatherForecast, etag) = await daprClient.GetStateAndETagAsync<WeatherForecast>("statestore", city);

// ... make some changes to the retrieved weather forecast

var result = await daprClient.TrySaveStateAsync("statestore", city, weatherForecast, etag);
```

<span data-ttu-id="e8466-218">`DaprClient.TrySaveStateAsync`如果在检索数据后状态存储中更改了数据 (和关联的 ETag) ，则该方法将失败。</span><span class="sxs-lookup"><span data-stu-id="e8466-218">The `DaprClient.TrySaveStateAsync` method fails when the data (and associated ETag) has been changed in the state store after the data was retrieved.</span></span> <span data-ttu-id="e8466-219">方法返回一个布尔值，指示调用是否成功。</span><span class="sxs-lookup"><span data-stu-id="e8466-219">The method returns a boolean value to indicate whether the call succeeded.</span></span> <span data-ttu-id="e8466-220">处理故障的策略是只需从状态存储重新加载已更新的数据，重新进行更改，然后重新提交更新。</span><span class="sxs-lookup"><span data-stu-id="e8466-220">A strategy to handle the failure is to simply reload the updated data from the state store, make the change again, and resubmit the update.</span></span>

<span data-ttu-id="e8466-221">如果你始终希望写入成功，而不考虑数据的其他更改，请使用 **最后一个写入 wins** 策略。</span><span class="sxs-lookup"><span data-stu-id="e8466-221">If you always want a write to succeed regardless of other changes to the data, use the **last-write-wins** strategy.</span></span>

<span data-ttu-id="e8466-222">SDK 提供了用于批量检索数据、删除数据和执行事务的其他方法。</span><span class="sxs-lookup"><span data-stu-id="e8466-222">The SDK provides other methods to retrieve data in bulk, delete data, and execute transactions.</span></span> <span data-ttu-id="e8466-223">有关详细信息，请参阅 [Dapr .NET SDK 存储库](https://github.com/dapr/dotnet-sdk)。</span><span class="sxs-lookup"><span data-stu-id="e8466-223">For more information, see the [Dapr .NET SDK repository](https://github.com/dapr/dotnet-sdk).</span></span>

### <a name="aspnet-core-integration"></a><span data-ttu-id="e8466-224">ASP.NET Core 集成</span><span class="sxs-lookup"><span data-stu-id="e8466-224">ASP.NET Core integration</span></span>

<span data-ttu-id="e8466-225">Dapr 还支持 ASP.NET Core，这是一个跨平台框架，用于生成基于云的新式 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="e8466-225">Dapr also supports ASP.NET Core, a cross-platform framework for building modern cloud-based web applications.</span></span> <span data-ttu-id="e8466-226">Dapr SDK 直接将状态管理功能集成到 [ASP.NET Core 模型绑定](/aspnet/core/mvc/models/model-binding) 功能。</span><span class="sxs-lookup"><span data-stu-id="e8466-226">The Dapr SDK integrates state management capabilities directly into the [ASP.NET Core model binding](/aspnet/core/mvc/models/model-binding) capabilities.</span></span> <span data-ttu-id="e8466-227">配置非常简单。</span><span class="sxs-lookup"><span data-stu-id="e8466-227">Configuration is simple.</span></span> <span data-ttu-id="e8466-228">`IMVCBuilder.AddDapr`通过 `.AddDapr` 在类中追加扩展方法来添加， `Startup.cs` 如下面的示例中所示：</span><span class="sxs-lookup"><span data-stu-id="e8466-228">Add the `IMVCBuilder.AddDapr` by appending the `.AddDapr` extension method in your `Startup.cs` class as shown in the next example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddControllers().AddDapr();
}
```

<span data-ttu-id="e8466-229">配置后，Dapr 可以使用 ASP.NET Core 特性将键/值对直接注入控制器操作 `FromState` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-229">Once configured, Dapr can inject a key/value pair directly into a controller action using the ASP.NET Core `FromState` attribute.</span></span> <span data-ttu-id="e8466-230">`DaprClient`不再需要引用对象。</span><span class="sxs-lookup"><span data-stu-id="e8466-230">Referencing the `DaprClient` object is no longer necessary.</span></span> <span data-ttu-id="e8466-231">下一个示例显示了一个 Web API，该 API 返回给定城市的天气预报：</span><span class="sxs-lookup"><span data-stu-id="e8466-231">The next example shows a Web API that returns the weather forecast for a given city:</span></span>

```csharp
[HttpGet("{city}")]
public ActionResult<WeatherForecast> Get([FromState("statestore", "city")] StateEntry<WeatherForecast> forecast)
{
    if (forecast.Value == null)
    {
      return NotFound();
    }

    return forecast.Value;
}
```

<span data-ttu-id="e8466-232">在此示例中，控制器使用属性加载天气预报 `FromState` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-232">In the example, the controller loads the weather forecast using the `FromState` attribute.</span></span> <span data-ttu-id="e8466-233">第一个属性参数是状态存储， `statestore` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-233">The first attribute parameter is the state store, `statestore`.</span></span> <span data-ttu-id="e8466-234">第二个特性参数 `city` 是用于获取状态键的 [路由模板](/aspnet/core/mvc/controllers/routing#route-templates) 变量的名称。</span><span class="sxs-lookup"><span data-stu-id="e8466-234">The second attribute parameter, `city`, is the name of the [route template](/aspnet/core/mvc/controllers/routing#route-templates) variable to get the state key.</span></span> <span data-ttu-id="e8466-235">如果省略第二个参数，则使用绑定方法参数的名称 (`forecast`) 用于查找路由模板变量。</span><span class="sxs-lookup"><span data-stu-id="e8466-235">If you omit the second parameter, the name of the bound method parameter (`forecast`) is used to look up the route template variable.</span></span>

<span data-ttu-id="e8466-236">`StateEntry`类包含为单个键/值对检索的所有信息的属性： `StoreName` 、 `Key` 、 `Value` 和 `ETag` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-236">The `StateEntry` class contains properties for all the information that is retrieved for a single key/value pair: `StoreName`, `Key`, `Value`, and `ETag`.</span></span> <span data-ttu-id="e8466-237">ETag 有助于实现 (OCC) 策略的开放式并发控制。</span><span class="sxs-lookup"><span data-stu-id="e8466-237">The ETag is useful for implementing optimistic concurrency control (OCC) strategy.</span></span> <span data-ttu-id="e8466-238">类还提供了删除或更新检索到的键/值数据的方法，无需使用 `DaprClient` 实例。</span><span class="sxs-lookup"><span data-stu-id="e8466-238">The class also provides methods to delete or update retrieved key/value data without requiring a `DaprClient` instance.</span></span> <span data-ttu-id="e8466-239">在下一个示例中， `TrySaveAsync` 方法用于使用 OCC 更新检索到的天气预报。</span><span class="sxs-lookup"><span data-stu-id="e8466-239">In the next example, the `TrySaveAsync` method is used to update the retrieved weather forecast using OCC.</span></span>

```csharp
[HttpPut("{city}")]
public async Task Put(WeatherForecast updatedForecast, [FromState("statestore", "city")] StateEntry<WeatherForecast> currentForecast)
{
    // update cached current forecast with updated forecast passed into service endpoint
    currentForecast.Value = updatedForecast;

    // update state store
    var success = await currentForecast.TrySaveAsync();

    // ... check result
}
```

## <a name="state-store-components"></a><span data-ttu-id="e8466-240">状态存储组件</span><span class="sxs-lookup"><span data-stu-id="e8466-240">State store components</span></span>

<span data-ttu-id="e8466-241">在撰写本文时，Dapr 为以下事务状态存储提供支持：</span><span class="sxs-lookup"><span data-stu-id="e8466-241">At the time of this writing, Dapr provides support for the following transactional state stores:</span></span>

- <span data-ttu-id="e8466-242">Azure CosmosDB</span><span class="sxs-lookup"><span data-stu-id="e8466-242">Azure CosmosDB</span></span>
- <span data-ttu-id="e8466-243">Azure SQL Server</span><span class="sxs-lookup"><span data-stu-id="e8466-243">Azure SQL Server</span></span>
- <span data-ttu-id="e8466-244">MongoDB</span><span class="sxs-lookup"><span data-stu-id="e8466-244">MongoDB</span></span>
- <span data-ttu-id="e8466-245">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="e8466-245">PostgreSQL</span></span>
- <span data-ttu-id="e8466-246">Redis</span><span class="sxs-lookup"><span data-stu-id="e8466-246">Redis</span></span>

<span data-ttu-id="e8466-247">Dapr 还包括对支持 CRUD 操作的状态存储的支持，但不支持事务功能：</span><span class="sxs-lookup"><span data-stu-id="e8466-247">Dapr also includes support for state stores that support CRUD operations, but not transactional capabilities:</span></span>

- <span data-ttu-id="e8466-248">Aerospike</span><span class="sxs-lookup"><span data-stu-id="e8466-248">Aerospike</span></span>
- <span data-ttu-id="e8466-249">Azure Blob 存储</span><span class="sxs-lookup"><span data-stu-id="e8466-249">Azure Blob Storage</span></span>
- <span data-ttu-id="e8466-250">Azure 表存储</span><span class="sxs-lookup"><span data-stu-id="e8466-250">Azure Table Storage</span></span>
- <span data-ttu-id="e8466-251">Cassandra</span><span class="sxs-lookup"><span data-stu-id="e8466-251">Cassandra</span></span>
- <span data-ttu-id="e8466-252">Cloudstate</span><span class="sxs-lookup"><span data-stu-id="e8466-252">Cloudstate</span></span>
- <span data-ttu-id="e8466-253">Couchbase</span><span class="sxs-lookup"><span data-stu-id="e8466-253">Couchbase</span></span>
- <span data-ttu-id="e8466-254">etcd</span><span class="sxs-lookup"><span data-stu-id="e8466-254">etcd</span></span>
- <span data-ttu-id="e8466-255">Google Cloud Firestore</span><span class="sxs-lookup"><span data-stu-id="e8466-255">Google Cloud Firestore</span></span>
- <span data-ttu-id="e8466-256">Hashicorp Consul</span><span class="sxs-lookup"><span data-stu-id="e8466-256">Hashicorp Consul</span></span>
- <span data-ttu-id="e8466-257">Hazelcast</span><span class="sxs-lookup"><span data-stu-id="e8466-257">Hazelcast</span></span>
- <span data-ttu-id="e8466-258">Memcached</span><span class="sxs-lookup"><span data-stu-id="e8466-258">Memcached</span></span>
- <span data-ttu-id="e8466-259">Zookeeper</span><span class="sxs-lookup"><span data-stu-id="e8466-259">Zookeeper</span></span>

### <a name="configuration"></a><span data-ttu-id="e8466-260">Configuration</span><span class="sxs-lookup"><span data-stu-id="e8466-260">Configuration</span></span>

<span data-ttu-id="e8466-261">在为本地自承载开发初始化时，Dapr 将 Redis 注册为默认的状态存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-261">When initialized for local, self-hosted development, Dapr registers Redis as the default state store.</span></span> <span data-ttu-id="e8466-262">下面是默认状态存储配置的示例。</span><span class="sxs-lookup"><span data-stu-id="e8466-262">Here's an example of the default state store configuration.</span></span> <span data-ttu-id="e8466-263">请注意默认名称 `statestore` ：</span><span class="sxs-lookup"><span data-stu-id="e8466-263">Note the default name, `statestore`:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
```

 > [!NOTE]
 > <span data-ttu-id="e8466-264">许多状态存储可以注册到单个应用程序，每个应用程序都有不同的名称。</span><span class="sxs-lookup"><span data-stu-id="e8466-264">Many state stores can be registered to a single application each with a different name.</span></span>

<span data-ttu-id="e8466-265">Redis 状态存储需要 `redisHost` 和 `redisPassword` 元数据来连接到 Redis 实例。</span><span class="sxs-lookup"><span data-stu-id="e8466-265">The Redis state store requires `redisHost` and `redisPassword` metadata to connect to the Redis instance.</span></span> <span data-ttu-id="e8466-266">在上面的示例中，Redis 密码 (默认值为空字符串) 以纯字符串形式存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-266">In the example above, the Redis password (which is an empty string by default) is stored as a plain string.</span></span> <span data-ttu-id="e8466-267">最佳做法是避免使用明文引用并始终使用机密引用。</span><span class="sxs-lookup"><span data-stu-id="e8466-267">The best practice is to avoid clear-text strings and always use secret references.</span></span> <span data-ttu-id="e8466-268">若要了解有关机密管理的详细信息，请参阅第 [10 章](secrets.md)。</span><span class="sxs-lookup"><span data-stu-id="e8466-268">To learn more about secret management, see [chapter 10](secrets.md).</span></span>

<span data-ttu-id="e8466-269">其他元数据字段指示执行组件 `actorStateStore` 生成块是否可以使用状态存储区。</span><span class="sxs-lookup"><span data-stu-id="e8466-269">The other metadata field, `actorStateStore`, indicates whether the state store can be consumed by the actors building block.</span></span>

### <a name="key-prefix-strategies"></a><span data-ttu-id="e8466-270">密钥前缀策略</span><span class="sxs-lookup"><span data-stu-id="e8466-270">Key prefix strategies</span></span>

<span data-ttu-id="e8466-271">状态存储组件允许不同的策略在底层存储中存储键/值对。</span><span class="sxs-lookup"><span data-stu-id="e8466-271">State store components enable different strategies to store key/value pairs in the underlying store.</span></span> <span data-ttu-id="e8466-272">回忆一项更早的示例，其中存储了客户希望购买的商品：</span><span class="sxs-lookup"><span data-stu-id="e8466-272">Recall the earlier example of a shopping basket service storing items a customer wishes to purchase:</span></span>

```console
curl -X POST http://localhost:3500/v1.0/state/statestore \
  -H "Content-Type: application/json" \
  -d '[{
        "key": "basket1",
        "value": {
          "customerId": 1,
          "items": [
            { "itemId": "DaprHoodie", "quantity": 1 }
          ]
        }
     }]'
```

<span data-ttu-id="e8466-273">使用 Redis 控制台工具，在 Redis 缓存中查看 Redis 状态存储组件如何保存数据：</span><span class="sxs-lookup"><span data-stu-id="e8466-273">Using the Redis Console tool, look inside the Redis cache to see how the Redis state store component persisted the data:</span></span>

```
127.0.0.1:6379> KEYS *
1) "basketservice||basket1"

127.0.0.1:6379> HGETALL basketservice||basket1
1) "data"
2) "{\"items\":[{\"itemId\":\"DaprHoodie\",\"quantity\":1}],\"customerId\":1}"
3) "version"
4) "1"
```

<span data-ttu-id="e8466-274">输出显示数据的完整 Redis **键** `basketservice||basket1` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-274">The output shows the full Redis **key** for the data as `basketservice||basket1`.</span></span> <span data-ttu-id="e8466-275">默认情况下，Dapr 使用 `application id` Dapr 实例的 `basketservice` 作为密钥的前缀 () 。</span><span class="sxs-lookup"><span data-stu-id="e8466-275">By default, Dapr uses the `application id` of the Dapr instance (`basketservice`) as a prefix for the key.</span></span> <span data-ttu-id="e8466-276">此命名约定允许多个 Dapr 实例共享相同的数据存储，而不会发生键名称冲突。</span><span class="sxs-lookup"><span data-stu-id="e8466-276">This naming convention enables multiple Dapr instances to share the same data store without key name collisions.</span></span> <span data-ttu-id="e8466-277">对于开发人员来说， `application id` 在运行具有 Dapr 的应用程序时，始终必须指定相同的。</span><span class="sxs-lookup"><span data-stu-id="e8466-277">For the developer, it's critical always to specify the same `application id` when running the application with Dapr.</span></span> <span data-ttu-id="e8466-278">如果省略，则 Dapr 将生成唯一的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="e8466-278">If omitted, Dapr will generate a unique application ID.</span></span> <span data-ttu-id="e8466-279">如果 `application id` 更改，应用程序将无法再访问使用上一个密钥前缀存储的状态。</span><span class="sxs-lookup"><span data-stu-id="e8466-279">If the `application id` changes, the application can no longer access the state stored with the previous key prefix.</span></span>

<span data-ttu-id="e8466-280">也就是说，可以在状态存储组件文件的 "元数据" 字段中为密钥前缀配置 *常量值* `keyPrefix` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-280">That said, it's possible to configure a *constant value* for the key prefix in the `keyPrefix` metadata field in the state store component file.</span></span> <span data-ttu-id="e8466-281">请看下面的示例：</span><span class="sxs-lookup"><span data-stu-id="e8466-281">Consider the following example:</span></span>

```yaml
spec:
  metadata:
  - name: keyPrefix
  - value: MyPrefix
```

<span data-ttu-id="e8466-282">使用常量键前缀可以跨多个 Dapr 应用程序访问状态存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-282">A constant key prefix enables the state store to be accessed across multiple Dapr applications.</span></span> <span data-ttu-id="e8466-283">更多操作，将设置 `keyPrefix` 为 `none` 完全省略前缀。</span><span class="sxs-lookup"><span data-stu-id="e8466-283">What's more, setting the `keyPrefix` to `none` omits the prefix completely.</span></span>

## <a name="reference-application-eshopondapr"></a><span data-ttu-id="e8466-284">引用应用程序： eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="e8466-284">Reference application: eShopOnDapr</span></span>

<span data-ttu-id="e8466-285">本书包括一个名为的参考应用程序 `eShopOnDapr` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-285">This book includes a reference application entitled `eShopOnDapr`.</span></span> <span data-ttu-id="e8466-286">它从早期的 Microsoft 微服务引用应用程序进行建模 `eShopOnContainers` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-286">It's modeled from an earlier Microsoft microservices reference application, `eShopOnContainers`.</span></span>

<span data-ttu-id="e8466-287">原始 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) 体系结构使用 `IBasketRepository` 接口来读取和写入购物篮服务的数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-287">The original [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) architecture used an `IBasketRepository` interface to read and write data for the basket service.</span></span> <span data-ttu-id="e8466-288">`RedisBasketRepository`类提供了使用 Redis 作为基础数据存储的实现：</span><span class="sxs-lookup"><span data-stu-id="e8466-288">The `RedisBasketRepository` class provided the implementation using Redis as the underlying data store:</span></span>

```csharp
public class RedisBasketRepository : IBasketRepository
{
    private readonly ConnectionMultiplexer _redis;
    private readonly IDatabase _database;

    public RedisBasketRepository(ConnectionMultiplexer redis)
    {
        _redis = redis;
        _database = redis.GetDatabase();
    }

    public async Task<CustomerBasket> GetBasketAsync(string customerId)
    {
        var data = await _database.StringGetAsync(customerId);

        if (data.IsNullOrEmpty)
        {
            return null;
        }

        return JsonConvert.DeserializeObject<CustomerBasket>(data);
    }

    // ...
}
```

<span data-ttu-id="e8466-289">此代码使用第三方 `StackExchange.Redis` NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="e8466-289">This code uses the third-party `StackExchange.Redis` NuGet package.</span></span> <span data-ttu-id="e8466-290">若要为给定客户加载购物篮，需执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="e8466-290">The following steps are required to load the shopping basket for a given customer:</span></span>

1. <span data-ttu-id="e8466-291">将插入 `ConnectionMultiplexer` 到构造函数中。</span><span class="sxs-lookup"><span data-stu-id="e8466-291">Inject a `ConnectionMultiplexer` into the constructor.</span></span> <span data-ttu-id="e8466-292">`ConnectionMultiplexer`向文件中的依赖关系注入框架注册 `Startup.cs` ：</span><span class="sxs-lookup"><span data-stu-id="e8466-292">The `ConnectionMultiplexer` is registered with the dependency injection framework in the `Startup.cs` file:</span></span>

   ```csharp
   services.AddSingleton<ConnectionMultiplexer>(sp =>
   {
       var settings = sp.GetRequiredService<IOptions<BasketSettings>>().Value;
       var configuration = ConfigurationOptions.Parse(settings.ConnectionString, true);
       configuration.ResolveDns = true;
       return ConnectionMultiplexer.Connect(configuration);
   });
   ```

1. <span data-ttu-id="e8466-293">使用在 `ConnectionMultiplexer` `IDatabase` 每个使用类中创建实例。</span><span class="sxs-lookup"><span data-stu-id="e8466-293">Use the `ConnectionMultiplexer` to create an `IDatabase` instance in each consuming class.</span></span>

1. <span data-ttu-id="e8466-294">使用 `IDatabase` 给定的作为键，使用实例执行 Redis StringGet 调用 `customerId` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-294">Use the `IDatabase` instance to execute a Redis StringGet call using the given `customerId` as the key.</span></span>

1. <span data-ttu-id="e8466-295">检查是否已从 Redis 加载数据;如果不是，则返回 `null` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-295">Check if data is loaded from Redis; if not, return `null`.</span></span>

1. <span data-ttu-id="e8466-296">将 Redis 中的数据反序列化为 `CustomerBasket` 对象并返回结果。</span><span class="sxs-lookup"><span data-stu-id="e8466-296">Deserialize the data from Redis to a `CustomerBasket` object and return the result.</span></span>

<span data-ttu-id="e8466-297">在更新后的 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) 引用应用程序中，新 `DaprBasketRepository` 类将替换 `RedisBasketRepository` 类：</span><span class="sxs-lookup"><span data-stu-id="e8466-297">In the updated [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr) reference application, a new `DaprBasketRepository` class replaces the `RedisBasketRepository` class:</span></span>

```csharp
public class DaprBasketRepository : IBasketRepository
{
    private const string StoreName = "eshop-basket-statestore";

    private readonly DaprClient _daprClient;

    public DaprBasketRepository(DaprClient daprClient)
    {
        _daprClient = daprClient ?? throw new ArgumentNullException(nameof(daprClient));;
    }

    public async Task<CustomerBasket> GetBasketAsync(string customerId)
    {
        return await _daprClient.GetStateAsync<CustomerBasket>(StoreName, customerId);
    }

    // ...
}
```

<span data-ttu-id="e8466-298">已更新的代码使用 Dapr .NET SDK，通过状态管理构建块读取和写入数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-298">The updated code uses the Dapr .NET SDK to read and write data using the state management building block.</span></span> <span data-ttu-id="e8466-299">为客户加载购物篮的新步骤大大简化：</span><span class="sxs-lookup"><span data-stu-id="e8466-299">The new steps to load the basket for a customer are dramatically simplified:</span></span>

1. <span data-ttu-id="e8466-300">将插入 `DaprClient` 到构造函数中。</span><span class="sxs-lookup"><span data-stu-id="e8466-300">Inject a `DaprClient` into the constructor.</span></span> <span data-ttu-id="e8466-301">`DaprClient`注册到文件中的依赖关系注入框架 `Startup.cs` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-301">The `DaprClient` is registered with the dependency injection framework in the `Startup.cs` file.</span></span>
1. <span data-ttu-id="e8466-302">使用 `DaprClient.GetStateAsync` 方法从已配置的状态存储中加载客户的购物篮项，并返回结果。</span><span class="sxs-lookup"><span data-stu-id="e8466-302">Use the `DaprClient.GetStateAsync` method to load the customer's shopping basket items from the configured state store and return the result.</span></span>

<span data-ttu-id="e8466-303">更新后的实现仍然使用 Redis 作为基础数据存储。</span><span class="sxs-lookup"><span data-stu-id="e8466-303">The updated implementation still uses Redis as the underlying data store.</span></span> <span data-ttu-id="e8466-304">但是，Dapr 将 `StackExchange.Redis` 从应用程序中抽象化引用和复杂性。</span><span class="sxs-lookup"><span data-stu-id="e8466-304">But, Dapr abstracts the `StackExchange.Redis` references and complexity from the application.</span></span> <span data-ttu-id="e8466-305">Dapr 配置文件是必需的：</span><span class="sxs-lookup"><span data-stu-id="e8466-305">A Dapr configuration file is all that's needed:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eshop-basket-statestore
  namespace: eshop
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: redis:6379
  - name: redisPassword
    secretKeyRef:
      name: redisPassword
auth:
  secretStore: eshop-secretstore
```

<span data-ttu-id="e8466-306">Dapr 实现还简化了基础数据存储的更改。</span><span class="sxs-lookup"><span data-stu-id="e8466-306">The Dapr implementation also simplifies changing the underlying data store.</span></span> <span data-ttu-id="e8466-307">例如，若要切换到 Azure 表存储，只需更改配置文件的内容。</span><span class="sxs-lookup"><span data-stu-id="e8466-307">For example, switching to Azure Table Storage requires only changing the contents of the configuration file.</span></span> <span data-ttu-id="e8466-308">不需要更改代码。</span><span class="sxs-lookup"><span data-stu-id="e8466-308">No code changes are necessary.</span></span>

## <a name="summary"></a><span data-ttu-id="e8466-309">摘要</span><span class="sxs-lookup"><span data-stu-id="e8466-309">Summary</span></span>

<span data-ttu-id="e8466-310">Dapr 状态管理构建块提供了一个 API，用于在各种数据存储区中存储键/值数据。</span><span class="sxs-lookup"><span data-stu-id="e8466-310">The Dapr state management building block offers an API for storing key/value data across various data stores.</span></span> <span data-ttu-id="e8466-311">API 为以下内容提供支持：</span><span class="sxs-lookup"><span data-stu-id="e8466-311">The API provides support for:</span></span>

- <span data-ttu-id="e8466-312">批量操作</span><span class="sxs-lookup"><span data-stu-id="e8466-312">Bulk operations</span></span>
- <span data-ttu-id="e8466-313">强一致性和最终一致性</span><span class="sxs-lookup"><span data-stu-id="e8466-313">Strong and eventual consistency</span></span>
- <span data-ttu-id="e8466-314">乐观并发控制</span><span class="sxs-lookup"><span data-stu-id="e8466-314">Optimistic concurrency control</span></span>
- <span data-ttu-id="e8466-315">多项事务</span><span class="sxs-lookup"><span data-stu-id="e8466-315">Multi-item transactions</span></span>

<span data-ttu-id="e8466-316">.NET SDK 为 .NET Core 和 ASP.NET Core 提供特定于语言的支持。</span><span class="sxs-lookup"><span data-stu-id="e8466-316">The .NET SDK provides language-specific support for .NET Core and ASP.NET Core.</span></span> <span data-ttu-id="e8466-317">模型绑定集成简化了访问和更新 ASP.NET Core 控制器操作方法的状态。</span><span class="sxs-lookup"><span data-stu-id="e8466-317">Model binding integration simplifies accessing and updating state from ASP.NET Core controller action methods.</span></span>

<span data-ttu-id="e8466-318">在 eShopOnDapr reference 应用程序中，转向 Dapr 状态管理的好处是显而易见的：</span><span class="sxs-lookup"><span data-stu-id="e8466-318">In the eShopOnDapr reference application, the benefits to moving to Dapr state management are clear:</span></span>

1. <span data-ttu-id="e8466-319">新实现使用更少的代码行。</span><span class="sxs-lookup"><span data-stu-id="e8466-319">The new implementation uses fewer lines of code.</span></span>
1. <span data-ttu-id="e8466-320">它消除了第三方 API 的复杂性 `StackExchange.Redis` 。</span><span class="sxs-lookup"><span data-stu-id="e8466-320">It abstracts away the complexity of the third-party `StackExchange.Redis` API.</span></span>
1. <span data-ttu-id="e8466-321">将基础 Redis 缓存替换为不同类型的数据存储现在只需要更改状态存储配置文件。</span><span class="sxs-lookup"><span data-stu-id="e8466-321">Replacing the underlying Redis cache with a different type of data store now only requires changes to the state store configuration file.</span></span>

### <a name="references"></a><span data-ttu-id="e8466-322">参考</span><span class="sxs-lookup"><span data-stu-id="e8466-322">References</span></span>

- [<span data-ttu-id="e8466-323">Dapr 支持的状态存储</span><span class="sxs-lookup"><span data-stu-id="e8466-323">Dapr supported state stores</span></span>](https://docs.dapr.io/operations/components/setup-state-store/supported-state-stores/)

> [!div class="step-by-step"]
> <span data-ttu-id="e8466-324">[上一页](reference-application.md)
> [下一页](service-invocation.md)</span><span class="sxs-lookup"><span data-stu-id="e8466-324">[Previous](reference-application.md)
[Next](service-invocation.md)</span></span>
