---
title: Dapr 服务调用构建基块
description: 服务调用构建基块的说明、功能、优点以及应用方法
author: amolenk
ms.date: 02/17/2021
ms.openlocfilehash: f6d5f10ae476d85a9925c4fa387a16d575cacf6a
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103624092"
---
# <a name="the-dapr-service-invocation-building-block"></a><span data-ttu-id="784e4-103">Dapr 服务调用构建基块</span><span class="sxs-lookup"><span data-stu-id="784e4-103">The Dapr service invocation building block</span></span>

<span data-ttu-id="784e4-104">在分布式系统中，一项服务通常需要与其他服务进行通信才能完成业务运营。</span><span class="sxs-lookup"><span data-stu-id="784e4-104">Across a distributed system, one service often needs to communicate with another to complete a business operation.</span></span> <span data-ttu-id="784e4-105">[Dapr 服务调用构建块](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/)可帮助简化服务之间的通信。</span><span class="sxs-lookup"><span data-stu-id="784e4-105">The [Dapr service invocation building block](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/) can help streamline the communication between services.</span></span>

## <a name="what-it-solves"></a><span data-ttu-id="784e4-106">解决方法</span><span class="sxs-lookup"><span data-stu-id="784e4-106">What it solves</span></span>

<span data-ttu-id="784e4-107">在分布式应用程序中的服务之间进行调用可能会很简单，但会涉及到许多挑战。</span><span class="sxs-lookup"><span data-stu-id="784e4-107">Making calls between services in a distributed application may appear easy, but there are many challenges involved.</span></span> <span data-ttu-id="784e4-108">例如：</span><span class="sxs-lookup"><span data-stu-id="784e4-108">For example:</span></span>

- <span data-ttu-id="784e4-109">其他服务的位置。</span><span class="sxs-lookup"><span data-stu-id="784e4-109">Where the other services are located.</span></span>
- <span data-ttu-id="784e4-110">在给定服务地址的情况下，如何安全地调用服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-110">How to call a service securely, given the service address.</span></span>
- <span data-ttu-id="784e4-111">在发生短暂的 [暂时性错误](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) 时如何处理重试。</span><span class="sxs-lookup"><span data-stu-id="784e4-111">How to handle retries when short-lived [transient errors](/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/transient-fault-handling) occur.</span></span>

<span data-ttu-id="784e4-112">最后，随着分布式应用程序编写许多不同的服务，跨服务调用关系图捕获见解对于诊断生产问题至关重要。</span><span class="sxs-lookup"><span data-stu-id="784e4-112">Lastly, as distributed applications compose many different services, capturing insights across service call graphs are critical to diagnosing production issues.</span></span>

<span data-ttu-id="784e4-113">服务调用构建块通过使用 Dapr 挎斗作为服务的 [反向代理](https://kemptechnologies.com/reverse-proxy/reverse-proxy/) 来解决这些难题。</span><span class="sxs-lookup"><span data-stu-id="784e4-113">The service invocation building block addresses these challenges by using a Dapr sidecar as a [reverse proxy](https://kemptechnologies.com/reverse-proxy/reverse-proxy/) for your service.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="784e4-114">工作原理</span><span class="sxs-lookup"><span data-stu-id="784e4-114">How it works</span></span>

<span data-ttu-id="784e4-115">让我们从一个示例开始。</span><span class="sxs-lookup"><span data-stu-id="784e4-115">Let's start with an example.</span></span> <span data-ttu-id="784e4-116">假设有两个服务： "服务 A" 和 "服务 B"。</span><span class="sxs-lookup"><span data-stu-id="784e4-116">Consider two services, "Service A" and "Service B".</span></span> <span data-ttu-id="784e4-117">服务 A 需要 `catalog/items` 在服务 B 上调用 API。尽管服务 A 可能会依赖于服务 B 并直接对其进行调用，但是服务 A 在 Dapr 挎斗上调用服务调用 API。</span><span class="sxs-lookup"><span data-stu-id="784e4-117">Service A needs to call the `catalog/items` API on Service B. While Service A could take a dependency on Service B and make a direct call to it, Service A instead invokes the service invocation API on the Dapr sidecar.</span></span> <span data-ttu-id="784e4-118">图6-1 显示了该操作。</span><span class="sxs-lookup"><span data-stu-id="784e4-118">Figure 6-1 shows the operation.</span></span>

![Dapr 服务调用的工作方式](./media/service-invocation/service-invocation-flow.png)

<span data-ttu-id="784e4-120">**图 6-1**.</span><span class="sxs-lookup"><span data-stu-id="784e4-120">**Figure 6-1**.</span></span> <span data-ttu-id="784e4-121">Dapr 服务调用的工作方式。</span><span class="sxs-lookup"><span data-stu-id="784e4-121">How Dapr service invocation works.</span></span>

<span data-ttu-id="784e4-122">请注意上图中的步骤：</span><span class="sxs-lookup"><span data-stu-id="784e4-122">Note the steps from the previous figure:</span></span>

1. <span data-ttu-id="784e4-123">服务 A `catalog/items` 通过在挎斗的服务上调用服务调用 API，在服务 B 中调用终结点。</span><span class="sxs-lookup"><span data-stu-id="784e4-123">Service A makes a call to the `catalog/items` endpoint in Service B by invoking the service invocation API on the Service A sidecar.</span></span>

    > [!NOTE]
    > <span data-ttu-id="784e4-124">挎斗使用可插接式名称解析机制来解析服务 B 的地址。在自承载模式下，Dapr 使用 [mdn](https://www.ionos.com/digitalguide/server/know-how/multicast-dns/) 来查找它。</span><span class="sxs-lookup"><span data-stu-id="784e4-124">The sidecar uses a pluggable name resolution mechanism to resolve the address of Service B. In self-hosted mode, Dapr uses [mDNS](https://www.ionos.com/digitalguide/server/know-how/multicast-dns/) to find it.</span></span> <span data-ttu-id="784e4-125">在 Kubernetes 模式下运行时，Kubernetes DNS 服务将确定地址。</span><span class="sxs-lookup"><span data-stu-id="784e4-125">When running in Kubernetes mode, the Kubernetes DNS service determines the address.</span></span>  

1. <span data-ttu-id="784e4-126">服务 A 挎斗将请求转发到服务 B 挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-126">The Service A sidecar forwards the request to the Service B sidecar.</span></span>

1. <span data-ttu-id="784e4-127">服务 B 挎斗对 `catalog/items` 服务 b API 发出实际请求。</span><span class="sxs-lookup"><span data-stu-id="784e4-127">The Service B sidecar makes the actual `catalog/items` request against the Service B API.</span></span>

1. <span data-ttu-id="784e4-128">服务 B 执行请求，并将响应返回给其挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-128">Service B executes the request and returns a response back to its sidecar.</span></span>

1. <span data-ttu-id="784e4-129">服务 B 挎斗将响应转发回挎斗的服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-129">The Service B sidecar forwards the response back to the Service A sidecar.</span></span>

1. <span data-ttu-id="784e4-130">服务 A 挎斗将响应返回给服务 A。</span><span class="sxs-lookup"><span data-stu-id="784e4-130">The Service A sidecar returns the response back to Service A.</span></span>

<span data-ttu-id="784e4-131">由于调用流过分支，Dapr 可以注入一些有用的交叉切削行为：</span><span class="sxs-lookup"><span data-stu-id="784e4-131">Because the calls flow through sidecars, Dapr can inject some useful cross-cutting behaviors:</span></span>

- <span data-ttu-id="784e4-132">失败时自动重试调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-132">Automatically retry calls upon failure.</span></span>
- <span data-ttu-id="784e4-133">通过相互 (mTLS) 身份验证（包括自动证书滚动更新），在服务之间进行调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-133">Make calls between services secure with mutual (mTLS) authentication, including automatic certificate rollover.</span></span>
- <span data-ttu-id="784e4-134">使用访问控制策略控制客户端可以执行的操作。</span><span class="sxs-lookup"><span data-stu-id="784e4-134">Control what operations clients can do using access control policies.</span></span>
- <span data-ttu-id="784e4-135">捕获服务间所有调用的跟踪和指标，以提供见解和诊断。</span><span class="sxs-lookup"><span data-stu-id="784e4-135">Capture traces and metrics for all calls between services to provide insights and diagnostics.</span></span>

<span data-ttu-id="784e4-136">任何应用程序都可以通过使用 Dapr 中内置的本机 **调用** API 来调用 Dapr 挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-136">Any application can invoke a Dapr sidecar by using the native **invoke** API built into Dapr.</span></span> <span data-ttu-id="784e4-137">可以通过 HTTP 或 gRPC 调用 API。</span><span class="sxs-lookup"><span data-stu-id="784e4-137">The API can be called with either HTTP or gRPC.</span></span> <span data-ttu-id="784e4-138">使用以下 URL 调用 HTTP API：</span><span class="sxs-lookup"><span data-stu-id="784e4-138">Use the following URL to call the HTTP API:</span></span>

``` http
http://localhost:<dapr-port>/v1.0/invoke/<application-id>/method/<method-name>
```

- <span data-ttu-id="784e4-139">`<dapr-port>` Dapr 正在侦听的 HTTP 端口。</span><span class="sxs-lookup"><span data-stu-id="784e4-139">`<dapr-port>` the HTTP port that Dapr is listening on.</span></span>
- <span data-ttu-id="784e4-140">`<application-id>` 要调用的服务的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="784e4-140">`<application-id>` application ID of the service to call.</span></span>
- <span data-ttu-id="784e4-141">`<method-name>` 要在远程服务上调用的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="784e4-141">`<method-name>` name of the method to invoke on the remote service.</span></span>

<span data-ttu-id="784e4-142">在下面的示例中， 对的 `catalog/items` "GET" 终结点发出了一个卷调用 `Service B` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-142">In the following example, a *curl* call is made to the `catalog/items` 'GET' endpoint of `Service B`:</span></span>

```console
curl http://localhost:3500/v1.0/invoke/serviceb/method/catalog/items
```

> [!NOTE]
> <span data-ttu-id="784e4-143">Dapr Api 启用支持 HTTP 或 gRPC 的任何应用程序堆栈以使用 Dapr 构建基块。</span><span class="sxs-lookup"><span data-stu-id="784e4-143">The Dapr APIs enable any application stack that supports HTTP or gRPC to use Dapr building blocks.</span></span> <span data-ttu-id="784e4-144">因此，服务调用构建块可充当协议之间的桥梁。</span><span class="sxs-lookup"><span data-stu-id="784e4-144">Therefore, the service invocation building block can act as a bridge between protocols.</span></span> <span data-ttu-id="784e4-145">服务可以使用 HTTP、gRPC 或两者的组合互相通信。</span><span class="sxs-lookup"><span data-stu-id="784e4-145">Services can communicate with each other using HTTP, gRPC or a combination of both.</span></span>

<span data-ttu-id="784e4-146">在下一部分中，你将了解如何使用 .NET SDK 来简化服务调用调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-146">In the next section, you'll learn how to use the .NET SDK to simplify service invocation calls.</span></span>

## <a name="use-the-dapr-net-sdk"></a><span data-ttu-id="784e4-147">使用 Dapr .NET SDK</span><span class="sxs-lookup"><span data-stu-id="784e4-147">Use the Dapr .NET SDK</span></span>

<span data-ttu-id="784e4-148">Dapr [.NET SDK](https://github.com/dapr/dotnet-sdk) 为 .net 开发人员提供了直观的、特定于语言的方法来与 Dapr 交互。</span><span class="sxs-lookup"><span data-stu-id="784e4-148">The Dapr [.NET SDK](https://github.com/dapr/dotnet-sdk) provides .NET developers with an intuitive and language-specific way to interact with Dapr.</span></span> <span data-ttu-id="784e4-149">SDK 为开发人员提供了三种建立远程服务调用调用的方法：</span><span class="sxs-lookup"><span data-stu-id="784e4-149">The SDK offers developers three ways of making remote service invocation calls:</span></span>

1. <span data-ttu-id="784e4-150">使用 HttpClient 调用 HTTP 服务</span><span class="sxs-lookup"><span data-stu-id="784e4-150">Invoke HTTP services using HttpClient</span></span>
1. <span data-ttu-id="784e4-151">使用 DaprClient 调用 HTTP 服务</span><span class="sxs-lookup"><span data-stu-id="784e4-151">Invoke HTTP services using DaprClient</span></span>
1. <span data-ttu-id="784e4-152">使用 DaprClient 调用 gRPC 服务</span><span class="sxs-lookup"><span data-stu-id="784e4-152">Invoke gRPC services using DaprClient</span></span>

### <a name="invoke-http-services-using-httpclient"></a><span data-ttu-id="784e4-153">使用 HttpClient 调用 HTTP 服务</span><span class="sxs-lookup"><span data-stu-id="784e4-153">Invoke HTTP services using HttpClient</span></span>

<span data-ttu-id="784e4-154">调用 HTTP 终结点的首选方法是使用 Dapr 与的丰富集成 `HttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-154">The preferred way to call an HTTP endpoint is to use Dapr's rich integration with `HttpClient`.</span></span> <span data-ttu-id="784e4-155">下面的示例通过调用 `submit` 应用程序的方法来提交订单 `orderservice` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-155">The following example submits an order by calling the `submit` method of the `orderservice` application:</span></span>

```csharp
var httpClient = DaprClient.CreateHttpClient();
await httpClient.PostAsJsonAsync("http://orderservice/submit", order);
```

<span data-ttu-id="784e4-156">在此示例中， `DaprClient.CreateHttpClient` 返回 `HttpClient` 用于执行 Dapr 服务调用的实例。</span><span class="sxs-lookup"><span data-stu-id="784e4-156">In the example, `DaprClient.CreateHttpClient` returns an `HttpClient` instance that is used to perform Dapr service invocation.</span></span> <span data-ttu-id="784e4-157">返回的 `HttpClient` 使用特殊的 Dapr 消息处理程序，该处理程序会重写传出请求的 uri。</span><span class="sxs-lookup"><span data-stu-id="784e4-157">The returned `HttpClient` uses a special Dapr message handler that rewrites URIs of outgoing requests.</span></span> <span data-ttu-id="784e4-158">主机名称被解释为要调用的服务的应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="784e4-158">The host name is interpreted as the application ID of the service to call.</span></span> <span data-ttu-id="784e4-159">实际调用的重写请求为：</span><span class="sxs-lookup"><span data-stu-id="784e4-159">The rewritten request that's actually being called is:</span></span>

```http
http://127.0.0.1:3500/v1/invoke/orderservice/method/submit
```

<span data-ttu-id="784e4-160">此示例使用 Dapr HTTP 终结点的默认值，即 `http://127.0.0.1:<dapr-http-port>/` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-160">This example uses the default value for the Dapr HTTP endpoint, which is `http://127.0.0.1:<dapr-http-port>/`.</span></span> <span data-ttu-id="784e4-161">的值 `dapr-http-port` 取自 `DAPR_HTTP_PORT` 环境变量。</span><span class="sxs-lookup"><span data-stu-id="784e4-161">The value of `dapr-http-port` is taken from the `DAPR_HTTP_PORT` environment variable.</span></span> <span data-ttu-id="784e4-162">如果未设置，则使用默认端口号 `3500` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-162">If it's not set, the default port number `3500` is used.</span></span>

<span data-ttu-id="784e4-163">或者，你可以在调用中配置自定义终结点 `DaprClient.CreateHttpClient` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-163">Alternatively, you can configure a custom endpoint in the call to `DaprClient.CreateHttpClient`:</span></span>

```csharp
var httpClient = DaprClient.CreateHttpClient(daprEndpoint = "localhost:4000");
```

<span data-ttu-id="784e4-164">还可以通过指定应用程序 ID 来直接设置基址。</span><span class="sxs-lookup"><span data-stu-id="784e4-164">You can also directly set the base address by specifying the application ID.</span></span> <span data-ttu-id="784e4-165">这样就可以在进行调用时使用相对 Uri：</span><span class="sxs-lookup"><span data-stu-id="784e4-165">This makes it possible to use relative URIs when making a call:</span></span>

```csharp
var httpClient = DaprClient.CreateHttpClient("orderservice");
await httpClient.PostAsJsonAsync("/submit");
```

<span data-ttu-id="784e4-166">`HttpClient`对象旨在长时间生存期。</span><span class="sxs-lookup"><span data-stu-id="784e4-166">The `HttpClient` object is intended to be long-lived.</span></span> <span data-ttu-id="784e4-167">可以在 `HttpClient` 应用程序的生存期内重复使用单个实例。</span><span class="sxs-lookup"><span data-stu-id="784e4-167">A single `HttpClient` instance can be reused for the lifetime of the application.</span></span> <span data-ttu-id="784e4-168">下一个方案演示了 `OrderServiceClient` 类如何重用 Dapr `HttpClient` 实例：</span><span class="sxs-lookup"><span data-stu-id="784e4-168">The next scenario demonstrates how an `OrderServiceClient` class reuses a Dapr `HttpClient` instance:</span></span>  

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // ...
    services.AddSingleton<IOrderServiceClient, OrderServiceClient>(
        _ => new OrderServiceClient(DaprClient.CreateInvokeHttpClient("orderservice")));
}
```

<span data-ttu-id="784e4-169">在上面的代码片段中， `OrderServiceClient` 使用 ASP.NET Core 依赖关系注入系统将注册为单一实例。</span><span class="sxs-lookup"><span data-stu-id="784e4-169">In the snippet above, the `OrderServiceClient` is registered as a singleton with the ASP.NET Core dependency injection system.</span></span> <span data-ttu-id="784e4-170">实现工厂通过调用创建一个新的 `HttpClient` 实例 `DaprClient.CreateInvokeHttpClient` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-170">An implementation factory creates a new `HttpClient` instance by calling `DaprClient.CreateInvokeHttpClient`.</span></span> <span data-ttu-id="784e4-171">然后，它使用新创建的 `HttpClient` 来实例化 `OrderServiceClient` 对象。</span><span class="sxs-lookup"><span data-stu-id="784e4-171">It then uses the newly created `HttpClient` to instantiate the `OrderServiceClient` object.</span></span> <span data-ttu-id="784e4-172">通过将注册 `OrderServiceClient` 为单一实例，它将在应用程序的生存期内重复使用。</span><span class="sxs-lookup"><span data-stu-id="784e4-172">By registering the `OrderServiceClient` as a singleton, it will be reused for the lifetime of the application.</span></span>

<span data-ttu-id="784e4-173">`OrderServiceClient`本身没有特定于 Dapr 的代码。</span><span class="sxs-lookup"><span data-stu-id="784e4-173">The `OrderServiceClient` itself has no Dapr-specific code.</span></span> <span data-ttu-id="784e4-174">即使在后台使用 Dapr 服务调用，你也可以像对待任何其他 HttpClient 一样处理 Dapr HttpClient：</span><span class="sxs-lookup"><span data-stu-id="784e4-174">Even though Dapr service invocation is used under the hood, you can treat the Dapr HttpClient like any other HttpClient:</span></span>

```csharp
public class OrderServiceClient : IOrderServiceClient
{
    private readonly HttpClient _httpClient;

    public OrderServiceClient(HttpClient httpClient)
    {
        _httpClient = httpClient ?? throw new ArgumentNullException(nameof(httpClient));
    }

    public async Task SubmitOrder(Order order)
    {
        var response = await _httpClient.PostAsJsonAsync("submit", order);
        response.EnsureSuccessStatusCode();
    }
}
```

<span data-ttu-id="784e4-175">将 HttpClient 类用于 Dapr 服务调用有很多好处：</span><span class="sxs-lookup"><span data-stu-id="784e4-175">Using the HttpClient class with Dapr service invocation has many benefits:</span></span>

- <span data-ttu-id="784e4-176">HttpClient 是许多开发人员已在其代码中使用的众所周知的类。</span><span class="sxs-lookup"><span data-stu-id="784e4-176">HttpClient is a well-known class that many developers already use in their code.</span></span> <span data-ttu-id="784e4-177">使用 HttpClient for Dapr 服务调用，开发人员可重复使用其现有技能。</span><span class="sxs-lookup"><span data-stu-id="784e4-177">Using HttpClient for Dapr service invocation allows developers to reuse their existing skills.</span></span>
- <span data-ttu-id="784e4-178">HttpClient 支持高级方案，如自定义标头，以及对请求和响应消息的完全控制。</span><span class="sxs-lookup"><span data-stu-id="784e4-178">HttpClient supports advanced scenarios, such as custom headers, and full control over request and response messages.</span></span>
- <span data-ttu-id="784e4-179">在 .NET 5 中，HttpClient 支持使用上的 System.Text.Js自动进行序列化和反序列化。</span><span class="sxs-lookup"><span data-stu-id="784e4-179">In .NET 5, HttpClient supports automatic serialization and deserialization using System.Text.Json.</span></span>
- <span data-ttu-id="784e4-180">HttpClient 集成了许多现有框架和库 [，如重新调整](https://github.com/reactiveui/refit)、 [RestSharp](https://restsharp.dev/getting-started/getting-started.html#basic-usage)和 [Polly](https://github.com/App-vNext/Polly)。</span><span class="sxs-lookup"><span data-stu-id="784e4-180">HttpClient integrates with many existing frameworks and libraries, such as [Refit](https://github.com/reactiveui/refit), [RestSharp](https://restsharp.dev/getting-started/getting-started.html#basic-usage), and [Polly](https://github.com/App-vNext/Polly).</span></span>

### <a name="invoke-http-services-using-daprclient"></a><span data-ttu-id="784e4-181">使用 DaprClient 调用 HTTP 服务</span><span class="sxs-lookup"><span data-stu-id="784e4-181">Invoke HTTP services using DaprClient</span></span>

<span data-ttu-id="784e4-182">尽管 HttpClient 是使用 HTTP 语义调用服务的首选方法，但也可以使用 `DaprClient.InvokeMethodAsync` 方法系列。</span><span class="sxs-lookup"><span data-stu-id="784e4-182">While HttpClient is the preferred way to invoke services using HTTP semantics, you can also use the `DaprClient.InvokeMethodAsync` family of methods.</span></span> <span data-ttu-id="784e4-183">下面的示例通过调用 `submit` 应用程序的方法来提交订单 `orderservice` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-183">The following example submits an order by calling the `submit` method of the `orderservice` application:</span></span>

``` csharp
var daprClient = new DaprClientBuilder().Build();
try
{
    var confirmation =
        await daprClient.InvokeMethodAsync<Order, OrderConfirmation>(
            "orderservice", "submit", order);
}
catch (InvocationException ex)
{
    // Handle error
}
```

<span data-ttu-id="784e4-184">第三个参数（ `order` 对象）在内部序列化 (与 `System.Text.JsonSerializer`) 并作为请求负载发送。</span><span class="sxs-lookup"><span data-stu-id="784e4-184">The third argument, an `order` object, is serialized internally (with `System.Text.JsonSerializer`) and sent as the request payload.</span></span> <span data-ttu-id="784e4-185">.NET SDK 负责调用挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-185">The .NET SDK takes care of the call to the sidecar.</span></span> <span data-ttu-id="784e4-186">它还会对对象的响应进行反序列化 `OrderConfirmation` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-186">It also deserializes the response to an `OrderConfirmation` object.</span></span> <span data-ttu-id="784e4-187">由于未指定 HTTP 方法，因此将以 HTTP POST 的形式执行请求。</span><span class="sxs-lookup"><span data-stu-id="784e4-187">Because no HTTP method is specified, the request is executed as an HTTP POST.</span></span>

<span data-ttu-id="784e4-188">下一个示例演示如何通过指定以下内容来发出 HTTP GET 请求 `HttpMethod` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-188">The next example demonstrates how you can make an HTTP GET request by specifying the `HttpMethod`:</span></span>

```csharp
var catalogItems = await daprClient.InvokeMethodAsync<IEnumerable<CatalogItem>>(HttpMethod.Get, "catalogservice", "items");
```

<span data-ttu-id="784e4-189">在某些情况下，可能需要对请求消息进行更多的控制。</span><span class="sxs-lookup"><span data-stu-id="784e4-189">For some scenarios, you may require more control over the request message.</span></span> <span data-ttu-id="784e4-190">例如，当你需要指定请求标头，或你想要将自定义序列化程序用于有效负载时。</span><span class="sxs-lookup"><span data-stu-id="784e4-190">For example, when you need to specify request headers, or you want to use a custom serializer for the payload.</span></span> <span data-ttu-id="784e4-191">`DaprClient.CreateInvokeMethodRequest` 创建一个 `HttpRequestMessage` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-191">`DaprClient.CreateInvokeMethodRequest` creates an `HttpRequestMessage`.</span></span> <span data-ttu-id="784e4-192">下面的示例演示如何将 HTTP 授权标头添加到请求消息：</span><span class="sxs-lookup"><span data-stu-id="784e4-192">The following example demonstrates how to add an HTTP authorization header to a request message:</span></span>

```csharp
var request = daprClient.CreateInvokeMethodRequest("orderservice", "submit", order);
request.Headers.Authorization = new AuthenticationHeaderValue("bearer", token);
```

<span data-ttu-id="784e4-193">`HttpRequestMessage`现在具有以下属性集：</span><span class="sxs-lookup"><span data-stu-id="784e4-193">The `HttpRequestMessage` now has the following properties set:</span></span>

- <span data-ttu-id="784e4-194">Url = `http://127.0.0.1:3500/v1.0/invoke/orderservice/method/submit`</span><span class="sxs-lookup"><span data-stu-id="784e4-194">Url = `http://127.0.0.1:3500/v1.0/invoke/orderservice/method/submit`</span></span>
- <span data-ttu-id="784e4-195">HttpMethod = POST</span><span class="sxs-lookup"><span data-stu-id="784e4-195">HttpMethod = POST</span></span>
- <span data-ttu-id="784e4-196">Content =  `JsonContent` 包含 JSON 序列化的对象 `order`</span><span class="sxs-lookup"><span data-stu-id="784e4-196">Content =  `JsonContent` object containing the JSON-serialized `order`</span></span>
- <span data-ttu-id="784e4-197">标头。 Authorization = "持有者 \<token> "</span><span class="sxs-lookup"><span data-stu-id="784e4-197">Headers.Authorization = "bearer \<token>"</span></span>

<span data-ttu-id="784e4-198">按所需方式设置请求后，请使用发送此请求 `DaprClient.InvokeMethodAsync` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-198">Once you've got the request set up the way you want, use `DaprClient.InvokeMethodAsync` to send it:</span></span>

```csharp
var orderConfirmation = await daprClient.InvokeMethodAsync<OrderConfirmation>(request);
```

<span data-ttu-id="784e4-199">`DaprClient.InvokeMethodAsync` 如果请求成功，则反序列化对象的响应 `OrderConfirmation` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-199">`DaprClient.InvokeMethodAsync` deserializes the response to an `OrderConfirmation` object if the request is successful.</span></span> <span data-ttu-id="784e4-200">或者，您可以使用 `DaprClient.InvokeMethodWithResponseAsync` 来获取对基础的完全访问权限 `HttpResponseMessage` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-200">Alternatively, you can use `DaprClient.InvokeMethodWithResponseAsync` to get full access to the underlying `HttpResponseMessage`:</span></span>

```csharp
var response = await daprClient.InvokeMethodWithResponseAsync(request);
response.EnsureSuccessStatusCode();

var orderConfirmation = response.Content.ReadFromJsonAsync<OrderConfirmation>();
```

> [!NOTE]
> <span data-ttu-id="784e4-201">对于使用 HTTP 的服务调用调用，有必要考虑使用上一节中介绍的 Dapr HttpClient 集成。</span><span class="sxs-lookup"><span data-stu-id="784e4-201">For service invocation calls using HTTP, it's worth considering using the Dapr HttpClient integration presented in the previous section.</span></span> <span data-ttu-id="784e4-202">使用 HttpClient 为你提供了更多好处，例如与现有框架和库集成。</span><span class="sxs-lookup"><span data-stu-id="784e4-202">Using HttpClient gives you additional benefits such as integration with existing frameworks and libraries.</span></span>

### <a name="invoke-grpc-services-using-daprclient"></a><span data-ttu-id="784e4-203">使用 DaprClient 调用 gRPC 服务</span><span class="sxs-lookup"><span data-stu-id="784e4-203">Invoke gRPC services using DaprClient</span></span>

<span data-ttu-id="784e4-204">DaprClient 提供了一系列 `InvokeMethodGrpcAsync` 方法来调用 gRPC 终结点。</span><span class="sxs-lookup"><span data-stu-id="784e4-204">DaprClient provides a family of `InvokeMethodGrpcAsync` methods for calling gRPC endpoints.</span></span> <span data-ttu-id="784e4-205">与 HTTP 方法的主要区别是使用 Protobuf 序列化程序，而不是 JSON。</span><span class="sxs-lookup"><span data-stu-id="784e4-205">The main difference with the HTTP methods is the use of a Protobuf serializer instead of JSON.</span></span> <span data-ttu-id="784e4-206">下面的示例调用 `submitOrder` Over gRPC 的的方法 `orderservice` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-206">The following example invokes the `submitOrder` method of the `orderservice` over gRPC.</span></span>

```csharp
var daprClient = new DaprClientBuilder().Build();
try
{
    var confirmation = await daprClient.InvokeMethodGrpcAsync<Order, OrderConfirmation>("orderservice", "submitOrder", order);
}
catch (InvocationException ex)
{
    // Handle error
}
```

<span data-ttu-id="784e4-207">在上面的示例中，DaprClient `order` 使用 [Protobuf](https://developers.google.com/protocol-buffers) 序列化给定对象，并使用结果作为 gRPC 请求正文。</span><span class="sxs-lookup"><span data-stu-id="784e4-207">In the example above, DaprClient serializes the given `order` object using [Protobuf](https://developers.google.com/protocol-buffers) and uses the result as the gRPC request body.</span></span> <span data-ttu-id="784e4-208">同样，响应正文 Protobuf 反序列化并返回给调用方。</span><span class="sxs-lookup"><span data-stu-id="784e4-208">Likewise, the response body is Protobuf deserialized and returned to the caller.</span></span> <span data-ttu-id="784e4-209">与 HTTP 服务调用中使用的 JSON 有效负载相比，Protobuf 通常提供更好的性能。</span><span class="sxs-lookup"><span data-stu-id="784e4-209">Protobuf typically provides better performance than the JSON payloads used in HTTP service invocation.</span></span>

## <a name="reference-application-eshopondapr"></a><span data-ttu-id="784e4-210">引用应用程序： eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="784e4-210">Reference application: eShopOnDapr</span></span>

<span data-ttu-id="784e4-211">Microsoft 的原始 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) 微服务参考体系结构使用了混合使用 HTTP/REST 和 gRPC 服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-211">The original [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers) microservice reference architecture from Microsoft used a mix of HTTP/REST and gRPC services.</span></span> <span data-ttu-id="784e4-212">使用 gRPC 仅限于 [聚合器服务](../cloud-native/service-to-service-communication.md#service-aggregator-pattern) 和核心后端服务之间的通信。</span><span class="sxs-lookup"><span data-stu-id="784e4-212">The use of gRPC was limited to communication between an [aggregator service](../cloud-native/service-to-service-communication.md#service-aggregator-pattern) and core back-end services.</span></span> <span data-ttu-id="784e4-213">图6-2 显示体系结构：</span><span class="sxs-lookup"><span data-stu-id="784e4-213">Figure 6-2 show the architecture:</span></span>

![eShopOnContainers 中的 gRPC 和 HTTP/REST 调用](./media/service-invocation/eshop-on-containers.png)

<span data-ttu-id="784e4-215">**图 6-2**.</span><span class="sxs-lookup"><span data-stu-id="784e4-215">**Figure 6-2**.</span></span> <span data-ttu-id="784e4-216">eShopOnContainers 中的 gRPC 和 HTTP/REST 调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-216">gRPC and HTTP/REST calls in eShopOnContainers.</span></span>

<span data-ttu-id="784e4-217">请注意上图中的步骤：</span><span class="sxs-lookup"><span data-stu-id="784e4-217">Note the steps from the previous figure:</span></span>

1. <span data-ttu-id="784e4-218">前端使用 HTTP/REST 调用 [API 网关](/azure/architecture/microservices/design/gateway) 。</span><span class="sxs-lookup"><span data-stu-id="784e4-218">The front end calls the [API gateway](/azure/architecture/microservices/design/gateway) using HTTP/REST.</span></span>

1. <span data-ttu-id="784e4-219">API 网关转发简单的 [CRUD](https://www.sumologic.com/glossary/crud/) (使用 HTTP/REST 将) 请求直接发送到核心后端服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-219">The API gateway forwards simple [CRUD](https://www.sumologic.com/glossary/crud/) (Create, Read, Update, Delete) requests directly to a core back-end service using HTTP/REST.</span></span>

1. <span data-ttu-id="784e4-220">API 网关将涉及多个后端服务的协调调用的复杂请求转发到 web 购物聚合器服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-220">The API gateway forwards complex requests that involve coordinated calls to multiple back-end services to the web shopping aggregator service.</span></span>

1. <span data-ttu-id="784e4-221">聚合器服务使用 gRPC 来调用核心后端服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-221">The aggregator service uses gRPC to call core back-end services.</span></span>

<span data-ttu-id="784e4-222">在最近更新的 eShopOnDapr 实现中，Dapr 分支将添加到服务和 API 网关。</span><span class="sxs-lookup"><span data-stu-id="784e4-222">In the recently updated eShopOnDapr implementation, Dapr sidecars are added to the services and API gateway.</span></span> <span data-ttu-id="784e4-223">图6-3 显示了更新后的体系结构：</span><span class="sxs-lookup"><span data-stu-id="784e4-223">Figure 6-3 show the updated architecture:</span></span>

![eShopOnContainers 中分支的 gRPC 和 HTTP/REST 调用](./media/service-invocation/eshop-on-dapr.png)

<span data-ttu-id="784e4-225">**图 6-3**。</span><span class="sxs-lookup"><span data-stu-id="784e4-225">**Figure 6-3**.</span></span> <span data-ttu-id="784e4-226">使用 Dapr 更新 eShop 体系结构。</span><span class="sxs-lookup"><span data-stu-id="784e4-226">Updated eShop architecture using Dapr.</span></span>

<span data-ttu-id="784e4-227">请注意上图中更新的步骤：</span><span class="sxs-lookup"><span data-stu-id="784e4-227">Note the updated steps from the previous figure:</span></span>

1. <span data-ttu-id="784e4-228">前端仍使用 HTTP/REST 调用 API 网关。</span><span class="sxs-lookup"><span data-stu-id="784e4-228">The front end still uses HTTP/REST to call the API gateway.</span></span>

1. <span data-ttu-id="784e4-229">API 网关将 HTTP 请求转发到其 Dapr 挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-229">The API gateway forwards HTTP requests to its Dapr sidecar.</span></span>

1. <span data-ttu-id="784e4-230">API 网关挎斗将请求发送到聚合器或后端服务的挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-230">The API gateway sidecar sends the request to the sidecar of the aggregator or back-end service.</span></span>

1. <span data-ttu-id="784e4-231">聚合器服务使用 Dapr .NET SDK 通过其挎斗体系结构调用后端服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-231">The aggregator service uses the Dapr .NET SDK to call back-end services through their sidecar architecture.</span></span>

<span data-ttu-id="784e4-232">Dapr 实现分支和 gRPC 之间的调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-232">Dapr implements calls between sidecars with gRPC.</span></span> <span data-ttu-id="784e4-233">因此，即使是使用 HTTP/REST 语义调用远程服务，也仍然使用 gRPC 实现传输的一部分。</span><span class="sxs-lookup"><span data-stu-id="784e4-233">So even if you're invoking a remote service with HTTP/REST semantics, a part of the transport is still implemented using gRPC.</span></span>

<span data-ttu-id="784e4-234">EShopOnDapr reference 应用程序的优点在于 Dapr service 调用构建基块。</span><span class="sxs-lookup"><span data-stu-id="784e4-234">The eShopOnDapr reference application benefits from the Dapr service invocation building block.</span></span> <span data-ttu-id="784e4-235">优点包括服务发现、自动 mTLS 和可观察性。</span><span class="sxs-lookup"><span data-stu-id="784e4-235">The benefits include service discovery, automatic mTLS, and observability.</span></span>

### <a name="forward-http-requests-using-envoy-and-dapr"></a><span data-ttu-id="784e4-236">使用 Envoy 和 Dapr 转发 HTTP 请求</span><span class="sxs-lookup"><span data-stu-id="784e4-236">Forward HTTP requests using Envoy and Dapr</span></span>

<span data-ttu-id="784e4-237">原始 eShop 应用程序和更新的应用程序都将 [Envoy 代理](https://www.envoyproxy.io/) 用作 API 网关。</span><span class="sxs-lookup"><span data-stu-id="784e4-237">Both the original and updated eShop application leverage the [Envoy proxy](https://www.envoyproxy.io/) as an API gateway.</span></span> <span data-ttu-id="784e4-238">Envoy 是一种开放源代理和跨新式分布式应用程序常用的通信总线。</span><span class="sxs-lookup"><span data-stu-id="784e4-238">Envoy is an open-source proxy and communication bus that is popular across modern distributed applications.</span></span> <span data-ttu-id="784e4-239">源自 Lyft，Envoy 由 [云本机计算基础](https://www.cncf.io/)拥有和维护。</span><span class="sxs-lookup"><span data-stu-id="784e4-239">Originating from Lyft, Envoy is owned and maintained by the [Cloud-Native Computing Foundation](https://www.cncf.io/).</span></span>

<span data-ttu-id="784e4-240">在原始 eShopOnContainers 实现中，Envoy API 网关会将传入的 HTTP 请求直接转发到聚合器或后端服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-240">In the original eShopOnContainers implementation, the Envoy API gateway forwarded incoming HTTP requests directly to aggregator or back-end services.</span></span> <span data-ttu-id="784e4-241">在 new eShopOnDapr 中，Envoy 代理将请求转发到 Dapr 挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-241">In the new eShopOnDapr, the Envoy proxy forwards the request to a Dapr sidecar.</span></span> <span data-ttu-id="784e4-242">挎斗提供服务调用、mTLS 和可观察性。</span><span class="sxs-lookup"><span data-stu-id="784e4-242">The sidecar provides service invocation, mTLS, and observability.</span></span>

<span data-ttu-id="784e4-243">使用 YAML 定义文件对 Envoy 进行配置，以控制代理的行为。</span><span class="sxs-lookup"><span data-stu-id="784e4-243">Envoy is configured using a YAML definition file to control the proxy's behavior.</span></span> <span data-ttu-id="784e4-244">为了使 Envoy 能够将 HTTP 请求转发到 Dapr 挎斗容器，会将一个 `dapr` 群集添加到配置中。</span><span class="sxs-lookup"><span data-stu-id="784e4-244">To enable Envoy to forward HTTP requests to a Dapr sidecar container, a `dapr` cluster is added to the configuration.</span></span> <span data-ttu-id="784e4-245">群集配置包含一个主机，该主机指向 Dapr 挎斗正在侦听的 HTTP 端口：</span><span class="sxs-lookup"><span data-stu-id="784e4-245">The cluster configuration contains a host that points to the HTTP port on which the Dapr sidecar is listening:</span></span>

``` yaml
clusters:
- name: dapr
  connect_timeout: 0.25s
  type: strict_dns
  hosts:
  - socket_address:
    address: 127.0.0.1
    port_value: 3500
```

<span data-ttu-id="784e4-246">更新 Envoy 路由配置，以将传入请求重写为对 Dapr 挎斗的调用， (请注意 `prefix_rewrite` 键/值对) ：</span><span class="sxs-lookup"><span data-stu-id="784e4-246">The Envoy routes configuration is updated to rewrite incoming requests as calls to the Dapr sidecar (pay close attention to the `prefix_rewrite` key/value pair):</span></span>

``` yaml
- name: "c-short"
  match:
    prefix: "/c/"
  route:
    auto_host_rewrite: true
    prefix_rewrite: "/v1.0/invoke/catalog-api/method/"
    cluster: dapr
```

<span data-ttu-id="784e4-247">假设前端客户端要检索目录项列表。</span><span class="sxs-lookup"><span data-stu-id="784e4-247">Consider a scenario where the front-end client wants to retrieve a list of catalog items.</span></span> <span data-ttu-id="784e4-248">目录 API 提供用于获取目录项的终结点：</span><span class="sxs-lookup"><span data-stu-id="784e4-248">The Catalog API provides an endpoint for getting the catalog items:</span></span>

``` csharp
[Route("api/v1/[controller]")]
[ApiController]
public class CatalogController : ControllerBase
{
    [HttpGet("items")]
    public async Task<IActionResult> ItemsAsync(
        [FromQuery] int pageSize = 10,
        [FromQuery] int pageIndex = 0)
    {
        // ...
    }
```

<span data-ttu-id="784e4-249">首先，前端直接调用 Envoy API 网关。</span><span class="sxs-lookup"><span data-stu-id="784e4-249">First, the front end makes a direct HTTP call to the Envoy API gateway.</span></span>

```
GET http://<api-gateway>/c/api/v1/catalog/items?pageSize=20
```

<span data-ttu-id="784e4-250">Envoy 代理匹配路由，重写 HTTP 请求，并将其转发到 `invoke` 其 Dapr 挎斗的 API：</span><span class="sxs-lookup"><span data-stu-id="784e4-250">The Envoy proxy matches the route, rewrites the HTTP request, and forwards it to the `invoke` API of its Dapr sidecar:</span></span>

```
GET http://127.0.0.1:3500/v1.0/invoke/catalog-api/method/api/v1/catalog/items?pageSize=20
```

<span data-ttu-id="784e4-251">挎斗处理服务发现并将请求路由到目录 API 挎斗。</span><span class="sxs-lookup"><span data-stu-id="784e4-251">The sidecar handles service discovery and routes the request to the Catalog API sidecar.</span></span> <span data-ttu-id="784e4-252">最后，挎斗调用目录 API 来执行请求、提取目录项并返回响应：</span><span class="sxs-lookup"><span data-stu-id="784e4-252">Finally, the sidecar calls the Catalog API to execute the request, fetch catalog items, and return a response:</span></span>

```
GET http://localhost/api/v1/catalog/items?pageSize=20
```

### <a name="make-aggregated-service-calls-using-the-net-sdk"></a><span data-ttu-id="784e4-253">使用 .NET SDK 进行聚合服务调用</span><span class="sxs-lookup"><span data-stu-id="784e4-253">Make aggregated service calls using the .NET SDK</span></span>

<span data-ttu-id="784e4-254">EShop 前端的大多数调用都是简单的 CRUD 调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-254">Most calls from the eShop front end are simple CRUD calls.</span></span> <span data-ttu-id="784e4-255">API 网关将它们转发给单个服务进行处理。</span><span class="sxs-lookup"><span data-stu-id="784e4-255">The API gateway forwards them to a single service for processing.</span></span> <span data-ttu-id="784e4-256">但在某些情况下，需要多个后端服务一起工作来完成请求。</span><span class="sxs-lookup"><span data-stu-id="784e4-256">Some scenarios, however, require multiple back-end services to work together to complete a request.</span></span> <span data-ttu-id="784e4-257">对于更复杂的调用，eShop 使用 web 购物聚合器服务跨多个服务调解工作流。</span><span class="sxs-lookup"><span data-stu-id="784e4-257">For these more complex calls, eShop uses the web shopping aggregator service to mediate the workflow across multiple services.</span></span> <span data-ttu-id="784e4-258">图6-4 显示了将商品添加到购物篮的处理顺序：</span><span class="sxs-lookup"><span data-stu-id="784e4-258">Figure 6-4 show the processing sequence of adding an item to your shopping basket:</span></span>

![更新购物篮序列图](./media/service-invocation/complex-call.png)

<span data-ttu-id="784e4-260">图 6-4  。</span><span class="sxs-lookup"><span data-stu-id="784e4-260">**Figure 6-4**.</span></span> <span data-ttu-id="784e4-261">更新购物篮序列。</span><span class="sxs-lookup"><span data-stu-id="784e4-261">Update shopping basket sequence.</span></span>

<span data-ttu-id="784e4-262">聚合器服务首先从目录 API 中检索目录项。</span><span class="sxs-lookup"><span data-stu-id="784e4-262">The aggregator service first retrieves catalog items from the Catalog API.</span></span> <span data-ttu-id="784e4-263">然后，它将验证项目的可用性和定价。</span><span class="sxs-lookup"><span data-stu-id="784e4-263">It then validates item availability and pricing.</span></span> <span data-ttu-id="784e4-264">最后，聚合器服务通过调用购物篮 API 来保存更新后的购物篮。</span><span class="sxs-lookup"><span data-stu-id="784e4-264">Finally, the aggregator service saves the updated shopping basket by calling the Basket API.</span></span>

<span data-ttu-id="784e4-265">聚合器服务包含一个 `BasketController` ，它提供终结点用于更新购物篮：</span><span class="sxs-lookup"><span data-stu-id="784e4-265">The aggregator service contains a `BasketController` that provides an endpoint for updating the shopping basket:</span></span>

``` csharp
[Route("api/v1/[controller]")]
[Authorize]
[ApiController]
public class BasketController : ControllerBase
{
    private readonly ICatalogService _catalog;
    private readonly IBasketService _basket;

    [HttpPost]
    [HttpPut]
    public async Task<ActionResult<BasketData>> UpdateAllBasketAsync(
        [FromBody] UpdateBasketRequest data, [FromHeader] string authorization)
    {
        // Get the item details from the catalog API.
        var catalogItems = await _catalog.GetCatalogItemsAsync(
            data.Items.Select(x => x.ProductId));

        // Check item availability and prices; store results in basket object.
        var basket = CreateValidatedBasket(data, catalogItems);

        // Save the shopping basket.
        await _basket.UpdateAsync(basket, authorization);

        return basket;
    }

    // ...
}
```

<span data-ttu-id="784e4-266">`UpdateAllBasketAsync`方法使用特性获取传入请求的 *Authorization* 标头 `FromHeader` 。</span><span class="sxs-lookup"><span data-stu-id="784e4-266">The `UpdateAllBasketAsync` method gets the *Authorization* header of the incoming request using a `FromHeader` attribute.</span></span> <span data-ttu-id="784e4-267">*Authorization* 标头包含调用受保护的后端服务所需的访问令牌。</span><span class="sxs-lookup"><span data-stu-id="784e4-267">The *Authorization* header contains the access token that is needed to call protected back-end services.</span></span>

<span data-ttu-id="784e4-268">收到更新购物篮的请求后，聚合器服务将调用目录 API 以获取项详细信息。</span><span class="sxs-lookup"><span data-stu-id="784e4-268">After receiving a request to update the basket, the aggregator service calls the Catalog API to get the item details.</span></span> <span data-ttu-id="784e4-269">购物篮控制器使用注入 `ICatalogService` 的对象进行调用，并与目录 API 通信。</span><span class="sxs-lookup"><span data-stu-id="784e4-269">The Basket controller uses an injected `ICatalogService` object to make that call and communicate with the Catalog API.</span></span> <span data-ttu-id="784e4-270">接口的原始实现使用 gRPC 进行调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-270">The original implementation of the interface used gRPC to make the call.</span></span> <span data-ttu-id="784e4-271">更新的实现将 Dapr 服务调用与 HttpClient 支持结合使用：</span><span class="sxs-lookup"><span data-stu-id="784e4-271">The updated implementation uses Dapr service invocation with HttpClient support:</span></span>

``` csharp
public class CatalogService : ICatalogService
{
    private readonly HttpClient _httpClient;

    public CatalogService(HttpClient httpClient)
    {
        _httpClient = httpClient ?? throw new ArgumentNullException(nameof(httpClient));
    }

    public Task<IEnumerable<CatalogItem>> GetCatalogItemsAsync(IEnumerable<int> ids)
    {
        var requestUri = $"api/v1/catalog/items?ids={string.Join(",", ids)}";

        return _httpClient.GetFromJsonAsync<IEnumerable<CatalogItem>>(requestUri);
    }

    // ...
}
```

<span data-ttu-id="784e4-272">请注意，不需要 Dapr 特定的代码就可以调用服务调用。</span><span class="sxs-lookup"><span data-stu-id="784e4-272">Notice how no Dapr specific code is required to make the service invocation call.</span></span> <span data-ttu-id="784e4-273">所有通信都是使用标准 HttpClient 对象完成的。</span><span class="sxs-lookup"><span data-stu-id="784e4-273">All communication is done using the standard HttpClient object.</span></span>

<span data-ttu-id="784e4-274">将 Dapr HttpClient 注入 `CatalogService` 方法中的类 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="784e4-274">The Dapr HttpClient is injected into the `CatalogService` class in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddSingleton<ICatalogService, CatalogService>(
    _ => new CatalogService(DaprClient.CreateInvokeHttpClient("catalog-api")));
```

<span data-ttu-id="784e4-275">聚合器服务所做的另一种调用是购物篮 API。</span><span class="sxs-lookup"><span data-stu-id="784e4-275">The other call made by the aggregator service is to the Basket API.</span></span> <span data-ttu-id="784e4-276">它只允许授权的请求。</span><span class="sxs-lookup"><span data-stu-id="784e4-276">It only allows authorized requests.</span></span> <span data-ttu-id="784e4-277">在 *授权* 请求标头中传递访问令牌，以确保调用成功：</span><span class="sxs-lookup"><span data-stu-id="784e4-277">The access token is passed along in an *Authorization* request header to ensure the call succeeds:</span></span>

``` csharp
public class BasketService : IBasketService
{
    public Task UpdateAsync(BasketData currentBasket, string accessToken)
    {
        var request = new HttpRequestMessage(HttpMethod.Post, "api/v1/basket")
        {
            Content = JsonContent.Create(currentBasket)
        };
        request.Headers.Authorization = new AuthenticationHeaderValue(accessToken);

        var response = await _httpClient.SendAsync(request);
        response.EnsureSuccessStatusCode();
    }

    // ...
}
```

<span data-ttu-id="784e4-278">在此示例中，这种情况下，仅使用标准 HttpClient 功能来调用服务。</span><span class="sxs-lookup"><span data-stu-id="784e4-278">In this example too, only standard HttpClient functionality is used to call the service.</span></span> <span data-ttu-id="784e4-279">这样，已经熟悉 HttpClient 的开发人员就可以重复使用其现有技能。</span><span class="sxs-lookup"><span data-stu-id="784e4-279">This allows developers who are already familiar with HttpClient to reuse their existing skills.</span></span> <span data-ttu-id="784e4-280">它甚至使现有的 HttpClient 代码可以使用 Dapr 服务调用，而无需进行任何更改。</span><span class="sxs-lookup"><span data-stu-id="784e4-280">It even enables existing HttpClient code to use Dapr service invocation without making any changes.</span></span>

## <a name="summary"></a><span data-ttu-id="784e4-281">摘要</span><span class="sxs-lookup"><span data-stu-id="784e4-281">Summary</span></span>

<span data-ttu-id="784e4-282">本章介绍了服务调用构建基块。</span><span class="sxs-lookup"><span data-stu-id="784e4-282">In this chapter, you learned about the service invocation building block.</span></span> <span data-ttu-id="784e4-283">你已了解如何通过直接 HTTP 调用 Dapr 挎斗并使用 Dapr .NET SDK 调用远程方法。</span><span class="sxs-lookup"><span data-stu-id="784e4-283">You saw how to invoke remote methods both by making direct HTTP calls to the Dapr sidecar, and by using the Dapr .NET SDK.</span></span>

<span data-ttu-id="784e4-284">Dapr .NET SDK 提供了多种方法来调用远程方法。</span><span class="sxs-lookup"><span data-stu-id="784e4-284">The Dapr .NET SDK provides multiple ways to invoke remote methods.</span></span> <span data-ttu-id="784e4-285">HttpClient 支持对于需要重复使用现有技能的开发人员非常有用，并且与许多现有框架和库兼容。</span><span class="sxs-lookup"><span data-stu-id="784e4-285">HttpClient support is great for developers wanting to reuse existing skills and is compatible with many existing frameworks and libraries.</span></span> <span data-ttu-id="784e4-286">DaprClient 提供使用 HTTP 或 gRPC 语义直接使用 Dapr 服务调用 API 的支持。</span><span class="sxs-lookup"><span data-stu-id="784e4-286">DaprClient offers support for directly using the Dapr service invocation API using either HTTP or gRPC semantics.</span></span>

<span data-ttu-id="784e4-287">EShopOnDapr 参考体系结构显示了如何使用 Dapr 服务调用现代化原始 eShopOnContainers 解决方案。</span><span class="sxs-lookup"><span data-stu-id="784e4-287">The eShopOnDapr reference architecture shows how the original eShopOnContainers solution is modernized by using Dapr service invocation.</span></span> <span data-ttu-id="784e4-288">将 Dapr 添加到 eShop 提供了一些优点，例如自动重试、使用 mTLS 进行消息加密，以及改进的可观察性。</span><span class="sxs-lookup"><span data-stu-id="784e4-288">Adding Dapr to eShop provides benefits such as automatic retries, message encryption using mTLS, and improved observability.</span></span>

### <a name="references"></a><span data-ttu-id="784e4-289">参考</span><span class="sxs-lookup"><span data-stu-id="784e4-289">References</span></span>

- [<span data-ttu-id="784e4-290">Dapr 服务调用构建基块</span><span class="sxs-lookup"><span data-stu-id="784e4-290">Dapr service invocation building block</span></span>](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/)

- [<span data-ttu-id="784e4-291">监视分布式云本机应用程序</span><span class="sxs-lookup"><span data-stu-id="784e4-291">Monitoring distributed cloud-native applications</span></span>](../cloud-native/observability-patterns.md)

> [!div class="step-by-step"]
> <span data-ttu-id="784e4-292">[上一页](state-management.md)
> [下一页](publish-subscribe.md)</span><span class="sxs-lookup"><span data-stu-id="784e4-292">[Previous](state-management.md)
[Next](publish-subscribe.md)</span></span>
