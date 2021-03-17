---
title: Dapr 绑定构建基块
description: 绑定构建基块及其功能、优点和应用方法的说明
author: edwinvw
ms.date: 02/17/2021
ms.openlocfilehash: d6f8b2aa90b15e5b9cd7b5c29938660d1b2907e9
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623949"
---
# <a name="the-dapr-bindings-building-block"></a><span data-ttu-id="7e525-103">Dapr 绑定构建基块</span><span class="sxs-lookup"><span data-stu-id="7e525-103">The Dapr bindings building block</span></span>

<span data-ttu-id="7e525-104">基于云的 *无服务器* 产品（如 AZURE FUNCTIONS 和 AWS Lambda）已跨分布式体系结构空间获得广泛的采用。</span><span class="sxs-lookup"><span data-stu-id="7e525-104">Cloud-based *serverless* offerings, such as Azure Functions and AWS Lambda, have gained wide adoption across the distributed architecture space.</span></span> <span data-ttu-id="7e525-105">在许多方面，它们使微服务能够 *处理来自* 外部系统的事件或在外部系统 *中调用事件* ，从而消除了底层复杂性和管道问题。</span><span class="sxs-lookup"><span data-stu-id="7e525-105">Among many benefits, they enable a microservice to *handle events from* or *invoke events in* an external system - abstracting away the underlying complexity and plumbing concerns.</span></span> <span data-ttu-id="7e525-106">外部资源很多：它们包括跨不同平台和供应商的数据存储、消息系统和 web 资源。</span><span class="sxs-lookup"><span data-stu-id="7e525-106">External resources are many: They include datastores, message systems, and web resources, across different platforms and vendors.</span></span> <span data-ttu-id="7e525-107">[Dapr 绑定构建块](https://docs.dapr.io/developing-applications/building-blocks/bindings/bindings-overview/)将这些相同的资源绑定功能引入 Dapr 应用程序的 doorstep。</span><span class="sxs-lookup"><span data-stu-id="7e525-107">The [Dapr bindings building block](https://docs.dapr.io/developing-applications/building-blocks/bindings/bindings-overview/) brings these same resource binding capabilities to the doorstep of your Dapr applications.</span></span>

## <a name="what-it-solves"></a><span data-ttu-id="7e525-108">解决方法</span><span class="sxs-lookup"><span data-stu-id="7e525-108">What it solves</span></span>

<span data-ttu-id="7e525-109">通过 Dapr 资源绑定，你的服务可以将业务运营与直接应用程序之外的外部资源集成。</span><span class="sxs-lookup"><span data-stu-id="7e525-109">Dapr resource bindings enable your services to integrate business operations across external resources outside of the immediate application.</span></span> <span data-ttu-id="7e525-110">来自外部系统的事件可能会触发服务中传递上下文信息的操作。</span><span class="sxs-lookup"><span data-stu-id="7e525-110">An event from an external system could trigger an operation in your service passing in contextual information.</span></span> <span data-ttu-id="7e525-111">然后，你的服务可以通过在另一个外部系统中触发事件来展开操作，同时传递上下文有效负载信息。</span><span class="sxs-lookup"><span data-stu-id="7e525-111">Your service could then expand the operation by triggering an event in another external system, passing in contextual payload information.</span></span> <span data-ttu-id="7e525-112">你的服务不需要耦合或识别外部资源就进行通信。</span><span class="sxs-lookup"><span data-stu-id="7e525-112">Your service communicates without coupling or awareness of the external resource.</span></span> <span data-ttu-id="7e525-113">该管道封装在预定义的 Dapr 组件中。</span><span class="sxs-lookup"><span data-stu-id="7e525-113">The plumbing is encapsulated inside pre-defined Dapr components.</span></span> <span data-ttu-id="7e525-114">在运行时，可以轻松地在运行时交换 Dapr 组件，而无需更改代码。</span><span class="sxs-lookup"><span data-stu-id="7e525-114">The Dapr component to use can be easily swapped at runtime without code changes.</span></span>

<span data-ttu-id="7e525-115">例如，当用户推文关键字时，请考虑触发事件的 Twitter 帐户。</span><span class="sxs-lookup"><span data-stu-id="7e525-115">Consider, for example, a Twitter account that triggers an event whenever a user tweets a keyword.</span></span> <span data-ttu-id="7e525-116">服务公开用于接收和处理推文的事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="7e525-116">Your service exposes an event handler that receives and processes the tweet.</span></span> <span data-ttu-id="7e525-117">完成后，服务将触发调用外部 Twilio 服务的事件。</span><span class="sxs-lookup"><span data-stu-id="7e525-117">Once complete, your service triggers an event that invokes an external Twilio service.</span></span> <span data-ttu-id="7e525-118">Twilio 发送包含推文的短信。</span><span class="sxs-lookup"><span data-stu-id="7e525-118">Twilio sends an SMS message that includes the tweet.</span></span> <span data-ttu-id="7e525-119">图8-1 显示了此操作的概念体系结构。</span><span class="sxs-lookup"><span data-stu-id="7e525-119">Figure 8-1 show the conceptual architecture of this operation.</span></span>

![输入绑定](media/bindings/bindings-architecture.png)

<span data-ttu-id="7e525-121">**图 8-1**。</span><span class="sxs-lookup"><span data-stu-id="7e525-121">**Figure 8-1**.</span></span> <span data-ttu-id="7e525-122">Dapr 资源绑定的概念体系结构。</span><span class="sxs-lookup"><span data-stu-id="7e525-122">Conceptual architecture of a Dapr resource binding.</span></span>

<span data-ttu-id="7e525-123">乍一看，资源绑定行为可能与本书前面介绍的 [发布/订阅模式](publish-subscribe.md) 类似。</span><span class="sxs-lookup"><span data-stu-id="7e525-123">At first glance, resource binding behavior may appear similar to the [Publish/Subscribe pattern](publish-subscribe.md) described earlier in this book.</span></span> <span data-ttu-id="7e525-124">尽管它们共享相似之处，但也有不同之处。</span><span class="sxs-lookup"><span data-stu-id="7e525-124">While they share similarities, there are differences.</span></span> <span data-ttu-id="7e525-125">发布/订阅侧重于 Dapr services 之间的异步通信。</span><span class="sxs-lookup"><span data-stu-id="7e525-125">Publish/subscribe focuses on asynchronous communication between Dapr services.</span></span> <span data-ttu-id="7e525-126">资源绑定具有更大的范围。</span><span class="sxs-lookup"><span data-stu-id="7e525-126">Resource binding has a much wider scope.</span></span> <span data-ttu-id="7e525-127">它侧重于软件平台之间的系统互操作性。</span><span class="sxs-lookup"><span data-stu-id="7e525-127">It focuses on system interoperability across software platforms.</span></span> <span data-ttu-id="7e525-128">在不同的应用程序、数据存储和微服务应用程序之外的服务之间交换信息。</span><span class="sxs-lookup"><span data-stu-id="7e525-128">Exchanging information between disparate applications, datastores, and services outside your microservice application.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="7e525-129">工作原理</span><span class="sxs-lookup"><span data-stu-id="7e525-129">How it works</span></span>

<span data-ttu-id="7e525-130">Dapr 资源绑定以组件配置文件开头。</span><span class="sxs-lookup"><span data-stu-id="7e525-130">Dapr resource binding starts with a component configuration file.</span></span> <span data-ttu-id="7e525-131">此 YAML 文件描述要与其配置设置一起绑定的资源类型。</span><span class="sxs-lookup"><span data-stu-id="7e525-131">This YAML file describes the type of resource to which you'll bind along with its configuration settings.</span></span> <span data-ttu-id="7e525-132">配置后，你的服务可以接收来自资源的事件或触发事件的事件。</span><span class="sxs-lookup"><span data-stu-id="7e525-132">Once configured, your service can receive events from the resource or trigger events on it.</span></span>

> [!NOTE]
> <span data-ttu-id="7e525-133">稍后将在 " *组件* " 部分中详细介绍绑定配置。</span><span class="sxs-lookup"><span data-stu-id="7e525-133">Binding configurations are presented in detail later in the *Components* section.</span></span>

### <a name="input-bindings"></a><span data-ttu-id="7e525-134">输入绑定</span><span class="sxs-lookup"><span data-stu-id="7e525-134">Input bindings</span></span>

<span data-ttu-id="7e525-135">输入绑定通过外部资源的传入事件触发代码。</span><span class="sxs-lookup"><span data-stu-id="7e525-135">Input bindings trigger your code with incoming events from external resources.</span></span> <span data-ttu-id="7e525-136">若要接收事件和数据，请从服务中注册一个将成为 *事件处理程序* 的公共终结点。</span><span class="sxs-lookup"><span data-stu-id="7e525-136">To receive events and data, you register a public endpoint from your service that becomes the *event handler*.</span></span> <span data-ttu-id="7e525-137">图8-2 显示了流：</span><span class="sxs-lookup"><span data-stu-id="7e525-137">Figure 8-2 shows the flow:</span></span>

![Dapr 输入绑定流](media/bindings/input-binding-flow.png)

<span data-ttu-id="7e525-139">**图 8-2**。</span><span class="sxs-lookup"><span data-stu-id="7e525-139">**Figure 8-2**.</span></span> <span data-ttu-id="7e525-140">Dapr 输入绑定流。</span><span class="sxs-lookup"><span data-stu-id="7e525-140">Dapr input binding flow.</span></span>

<span data-ttu-id="7e525-141">图8.2 介绍了从外部 Twitter 帐户接收事件的步骤：</span><span class="sxs-lookup"><span data-stu-id="7e525-141">Figure 8.2 describes the steps for receiving events from an external Twitter account:</span></span>

1. <span data-ttu-id="7e525-142">Dapr 挎斗读取绑定配置文件并订阅为外部资源指定的事件。</span><span class="sxs-lookup"><span data-stu-id="7e525-142">The Dapr sidecar reads the binding configuration file and subscribes to the event specified for the external resource.</span></span> <span data-ttu-id="7e525-143">在此示例中，事件源是一个 Twitter 帐户。</span><span class="sxs-lookup"><span data-stu-id="7e525-143">In the example, the event source is a Twitter account.</span></span>
1. <span data-ttu-id="7e525-144">当在 Twitter 上发布了匹配的推文时，在 Dapr 挎斗中运行的绑定组件会选取它并触发一个事件。</span><span class="sxs-lookup"><span data-stu-id="7e525-144">When a matching Tweet is published on Twitter, the binding component running in the Dapr sidecar picks it up and triggers an event.</span></span>
1. <span data-ttu-id="7e525-145">Dapr 挎斗调用 (的终结点，该终结点为绑定配置) 事件处理程序。</span><span class="sxs-lookup"><span data-stu-id="7e525-145">The Dapr sidecar invokes the endpoint (that is, event handler) configured for the binding.</span></span> <span data-ttu-id="7e525-146">在此示例中，服务在 `/tweet` 端口6000上侦听终结点上的 HTTP POST。</span><span class="sxs-lookup"><span data-stu-id="7e525-146">In the example, the service listens for an HTTP POST on the `/tweet` endpoint on port 6000.</span></span> <span data-ttu-id="7e525-147">由于它是 HTTP POST 操作，因此在请求正文中传递事件的 JSON 有效负载。</span><span class="sxs-lookup"><span data-stu-id="7e525-147">Because it's an HTTP POST operation, the JSON payload for the event is passed in the request body.</span></span>
1. <span data-ttu-id="7e525-148">处理事件后，服务将返回 HTTP 状态代码 `200 OK` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-148">After handling the event, the service returns an HTTP status code `200 OK`.</span></span>

<span data-ttu-id="7e525-149">以下 ASP.NET Core 控制器提供了一个处理受 Twitter 绑定触发的事件的示例：</span><span class="sxs-lookup"><span data-stu-id="7e525-149">The following ASP.NET Core controller provides an example of handling an event triggered by the Twitter binding:</span></span>

```csharp
[ApiController]
public class SomeController : ControllerBase
{
    public class TwitterTweet
    {
        [JsonPropertyName("id_str")]
        public string ID {get; set; }

        [JsonPropertyName("text")]
        public string Text {get; set; }
    }

    [HttpPost("/tweet")]
    public ActionResult Post(TwitterTweet tweet)
    {
        // Handle tweet
        Console.WriteLine("Tweet received: {0}: {1}", tweet.ID, tweet.Text);

        // ...

        // Acknowledge message
        return Ok();
    }
}
```

<span data-ttu-id="7e525-150">如果操作错误，则会返回相应的400或500级别的 HTTP 状态代码。</span><span class="sxs-lookup"><span data-stu-id="7e525-150">If the operation should error, you would return the appropriate 400 or 500 level HTTP status code.</span></span> <span data-ttu-id="7e525-151">对于 *至少传递一次* 的绑定，Dapr 挎斗将重试触发器。</span><span class="sxs-lookup"><span data-stu-id="7e525-151">For bindings that feature *at-least-once-delivery* guarantees, the Dapr sidecar will retry the trigger.</span></span> <span data-ttu-id="7e525-152">查看 [有关资源绑定的 Dapr 文档] [1]，以查看它们是否提供了 " *至少一次* *" 或 "一次性交付* 保证"。</span><span class="sxs-lookup"><span data-stu-id="7e525-152">Check out [Dapr documentation for resource bindings][1] to see whether they offer *at-least-once* or *exactly-once* delivery guarantees.</span></span>

### <a name="output-bindings"></a><span data-ttu-id="7e525-153">输出绑定</span><span class="sxs-lookup"><span data-stu-id="7e525-153">Output bindings</span></span>

<span data-ttu-id="7e525-154">Dapr 还包括 *输出绑定* 功能。</span><span class="sxs-lookup"><span data-stu-id="7e525-154">Dapr also includes *output binding* capabilities.</span></span> <span data-ttu-id="7e525-155">它们使服务能够触发调用外部资源的事件。</span><span class="sxs-lookup"><span data-stu-id="7e525-155">They enable your service to trigger an event that invokes an external resource.</span></span> <span data-ttu-id="7e525-156">同样，您首先配置描述输出绑定的绑定配置 YAML 文件。</span><span class="sxs-lookup"><span data-stu-id="7e525-156">Again, you start by configuring a binding configuration YAML file that describes the output binding.</span></span> <span data-ttu-id="7e525-157">一旦准备好，就会触发一个事件，该事件在应用程序的 Dapr 挎斗上调用绑定 API。</span><span class="sxs-lookup"><span data-stu-id="7e525-157">Once in place, you trigger an event that invokes the bindings API on the Dapr sidecar of your application.</span></span> <span data-ttu-id="7e525-158">图8-3 显示了输出绑定的流：</span><span class="sxs-lookup"><span data-stu-id="7e525-158">Figure 8-3 shows the flow of an output binding:</span></span>

![Dapr 输出绑定流](media/bindings/output-binding-flow.png)

<span data-ttu-id="7e525-160">**图 8-3**。</span><span class="sxs-lookup"><span data-stu-id="7e525-160">**Figure 8-3**.</span></span> <span data-ttu-id="7e525-161">Dapr 输出绑定流。</span><span class="sxs-lookup"><span data-stu-id="7e525-161">Dapr output binding flow.</span></span>

1. <span data-ttu-id="7e525-162">Dapr 挎斗读取绑定配置文件，其中包含有关如何连接到外部资源的信息。</span><span class="sxs-lookup"><span data-stu-id="7e525-162">The Dapr sidecar reads the binding configuration file with the information on how to connect to the external resource.</span></span> <span data-ttu-id="7e525-163">在此示例中，外部资源是 Twilio 的 SMS 帐户。</span><span class="sxs-lookup"><span data-stu-id="7e525-163">In the example, the external resource is a Twilio SMS account.</span></span>
1. <span data-ttu-id="7e525-164">应用程序在 `/v1.0/bindings/sms` Dapr 挎斗上调用终结点。</span><span class="sxs-lookup"><span data-stu-id="7e525-164">Your application invokes the `/v1.0/bindings/sms` endpoint on the Dapr sidecar.</span></span> <span data-ttu-id="7e525-165">在这种情况下，它使用 HTTP POST 来调用 API。</span><span class="sxs-lookup"><span data-stu-id="7e525-165">In this case, it uses an HTTP POST to invoke the API.</span></span> <span data-ttu-id="7e525-166">还可以使用 gRPC。</span><span class="sxs-lookup"><span data-stu-id="7e525-166">It's also possible to use gRPC.</span></span>
1. <span data-ttu-id="7e525-167">在 Dapr 挎斗中运行的绑定组件会调用外部消息系统来发送消息。</span><span class="sxs-lookup"><span data-stu-id="7e525-167">The binding component running in the Dapr sidecar calls the external messaging system to send the message.</span></span> <span data-ttu-id="7e525-168">消息将包含 POST 请求中传递的负载。</span><span class="sxs-lookup"><span data-stu-id="7e525-168">The message will contain the payload passed in the POST request.</span></span>

<span data-ttu-id="7e525-169">例如，你可以通过使用卷调用 Dapr API 来调用输出绑定：</span><span class="sxs-lookup"><span data-stu-id="7e525-169">As an example, you can invoke an output binding by invoking the Dapr API using curl:</span></span>

```console
curl -X POST http://localhost:3500/v1.0/bindings/sms \
  -H "Content-Type: application/json" \
  -d '{
        "data": "Welcome to this awesome service",
        "metadata": {
          "toNumber": "555-3277"
        },
        "operation": "create"
      }'
```

<span data-ttu-id="7e525-170">请注意，HTTP 端口与 Dapr 挎斗 (（在本例中为默认 Dapr HTTP 端口) 使用的端口相同 `3500` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-170">Note that the HTTP port is the same as used by the Dapr sidecar (in this case, the default Dapr HTTP port `3500`).</span></span>

<span data-ttu-id="7e525-171">负载 (的结构是，发送) 的消息将因绑定而异。</span><span class="sxs-lookup"><span data-stu-id="7e525-171">The structure of the payload (that is, message sent) will vary per binding.</span></span> <span data-ttu-id="7e525-172">在上面的示例中，负载包含包含 `data` 消息的元素。</span><span class="sxs-lookup"><span data-stu-id="7e525-172">In the example above, the payload contains a `data` element with a message.</span></span> <span data-ttu-id="7e525-173">与其他类型的外部资源的绑定可能不同，尤其是对于发送的元数据。</span><span class="sxs-lookup"><span data-stu-id="7e525-173">Bindings to other types of external resources can be different, especially for the metadata that is sent.</span></span> <span data-ttu-id="7e525-174">每个负载还必须包含一个 `operation` 字段，该字段定义绑定将执行的操作。</span><span class="sxs-lookup"><span data-stu-id="7e525-174">Each payload must also contain an `operation` field, that defines the operation the binding will execute.</span></span> <span data-ttu-id="7e525-175">上面的示例指定了 `create` 创建 SMS 消息的操作。</span><span class="sxs-lookup"><span data-stu-id="7e525-175">The above example specifies a `create` operation that creates the SMS message.</span></span> <span data-ttu-id="7e525-176">常见操作包括：</span><span class="sxs-lookup"><span data-stu-id="7e525-176">Common operations include:</span></span>

- <span data-ttu-id="7e525-177">create</span><span class="sxs-lookup"><span data-stu-id="7e525-177">create</span></span>
- <span data-ttu-id="7e525-178">get</span><span class="sxs-lookup"><span data-stu-id="7e525-178">get</span></span>
- <span data-ttu-id="7e525-179">delete</span><span class="sxs-lookup"><span data-stu-id="7e525-179">delete</span></span>
- <span data-ttu-id="7e525-180">list</span><span class="sxs-lookup"><span data-stu-id="7e525-180">list</span></span>

<span data-ttu-id="7e525-181">绑定由绑定支持的操作的作者。</span><span class="sxs-lookup"><span data-stu-id="7e525-181">It's up to the author of the binding which operations the binding supports.</span></span> <span data-ttu-id="7e525-182">每个绑定的文档描述了可用的操作以及调用方法。</span><span class="sxs-lookup"><span data-stu-id="7e525-182">The documentation for each binding describes the available operations and how to invoke them.</span></span>

## <a name="using-the-dapr-net-sdk"></a><span data-ttu-id="7e525-183">使用 Dapr .NET SDK</span><span class="sxs-lookup"><span data-stu-id="7e525-183">Using the Dapr .NET SDK</span></span>

<span data-ttu-id="7e525-184">Dapr .NET SDK 为 .NET Core 开发人员提供特定于语言的支持。</span><span class="sxs-lookup"><span data-stu-id="7e525-184">The Dapr .NET SDK provides language-specific support for .NET Core developers.</span></span> <span data-ttu-id="7e525-185">在下面的示例中，对的调用 `HttpClient.PostAsync()` 将替换为 `DaprClient.InvokeBindingAsync()` 方法。</span><span class="sxs-lookup"><span data-stu-id="7e525-185">In the following example, the call to the `HttpClient.PostAsync()` is replaced with the `DaprClient.InvokeBindingAsync()` method.</span></span> <span data-ttu-id="7e525-186">此专用方法简化了调用已配置的输出绑定的操作：</span><span class="sxs-lookup"><span data-stu-id="7e525-186">This specialized method simplifies invoking a configured output binding:</span></span>

```csharp
private async Task SendSMSAsync([FromServices] DaprClient daprClient)
{
    var message = "Welcome to this awesome service";
    var metadata = new Dictionary<string, string>
    {
      { "toNumber", "555-3277" }
    };
    await daprClient.InvokeBindingAsync("sms", "create", message, metadata);
}
```

<span data-ttu-id="7e525-187">此方法需要 `metadata` 和 `message` 值。</span><span class="sxs-lookup"><span data-stu-id="7e525-187">The method expects the `metadata` and `message` values.</span></span>

<span data-ttu-id="7e525-188">当用于调用绑定时，将 `DaprClient` 使用 gRPC 来调用 Dapr 挎斗上的 DAPR API。</span><span class="sxs-lookup"><span data-stu-id="7e525-188">When used to invoke a binding, the `DaprClient` uses gRPC to call the Dapr API on the Dapr sidecar.</span></span>

## <a name="binding-components"></a><span data-ttu-id="7e525-189">绑定组件</span><span class="sxs-lookup"><span data-stu-id="7e525-189">Binding components</span></span>

<span data-ttu-id="7e525-190">在后台，资源绑定是通过 Dapr 绑定组件实现的。</span><span class="sxs-lookup"><span data-stu-id="7e525-190">Under the hood, resource bindings are implemented with Dapr binding components.</span></span> <span data-ttu-id="7e525-191">它们由社区提供，并写入。</span><span class="sxs-lookup"><span data-stu-id="7e525-191">They're contributed by the community and written in Go.</span></span> <span data-ttu-id="7e525-192">如果需要将与不存在 Dapr 绑定的外部资源集成，可以自行创建。</span><span class="sxs-lookup"><span data-stu-id="7e525-192">If you need to integrate with an external resource for which no Dapr binding exists yet, you can create it yourself.</span></span> <span data-ttu-id="7e525-193">查看 [Dapr 组件-contrib](https://github.com/dapr/components-contrib) 存储库，了解如何参与绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-193">Check out the [Dapr components-contrib repo](https://github.com/dapr/components-contrib) to see how you can contribute a binding.</span></span>

> [!NOTE]
> <span data-ttu-id="7e525-194">Dapr 及其所有组件都是以 [Golang](https://golang.org/) () 语言编写的。</span><span class="sxs-lookup"><span data-stu-id="7e525-194">Dapr and all of its components are written in the [Golang](https://golang.org/) (Go) language.</span></span> <span data-ttu-id="7e525-195">转向成为新式的云本机编程平台。</span><span class="sxs-lookup"><span data-stu-id="7e525-195">Go is considered a modern, cloud-native programming platform.</span></span>

<span data-ttu-id="7e525-196">使用 YAML 配置文件配置绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-196">You configure bindings using a YAML configuration file.</span></span> <span data-ttu-id="7e525-197">下面是用于 Twitter 绑定的示例配置：</span><span class="sxs-lookup"><span data-stu-id="7e525-197">Here's an example configuration for the Twitter binding:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: twitter-mention
  namespace: default
spec:
  type: bindings.twitter
  version: v1
  metadata:
  - name: consumerKey
    value: "****" # twitter api consumer key, required
  - name: consumerSecret
    value: "****" # twitter api consumer secret, required
  - name: accessToken
    value: "****" # twitter api access token, required
  - name: accessSecret
    value: "****" # twitter api access secret, required
  - name: query
    value: "dapr" # your search query, required
```

<span data-ttu-id="7e525-198">每个绑定配置都包含一个包含 `metadata` 和字段的常规元素 `name` `namespace` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-198">Each binding configuration contains a general `metadata` element with a `name` and `namespace` field.</span></span> <span data-ttu-id="7e525-199">Dapr 将根据配置的字段确定要调用的服务的终结点 `name` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-199">Dapr will determine the endpoint to invoke your service based upon the configured `name` field.</span></span> <span data-ttu-id="7e525-200">在上面的示例中，Dapr 将在 `/twitter-mention` 事件发生时调用在服务中批注的方法。</span><span class="sxs-lookup"><span data-stu-id="7e525-200">In the above example, Dapr will invoke the method annotated with `/twitter-mention` in your service when an event occurs.</span></span>

<span data-ttu-id="7e525-201">在 `spec` 元素中，指定绑定的 `type` 以及绑定特定的 `metadata` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-201">In the `spec` element, you specify the `type` of the binding along with binding specific `metadata`.</span></span> <span data-ttu-id="7e525-202">该示例指定了用于通过其 API 访问 Twitter 帐户的凭据。</span><span class="sxs-lookup"><span data-stu-id="7e525-202">The example specifies credentials for accessing a Twitter account using its API.</span></span> <span data-ttu-id="7e525-203">元数据可在输入绑定和输出绑定之间有所不同。</span><span class="sxs-lookup"><span data-stu-id="7e525-203">The metadata can differ between input and output bindings.</span></span> <span data-ttu-id="7e525-204">例如，若要使用 Twitter 作为输入绑定，需要使用字段指定要在推文中搜索的文本 `query` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-204">For example, to use Twitter as an input binding, you need to specify the text to search for in tweets using the `query` field.</span></span> <span data-ttu-id="7e525-205">每次发送匹配的推文时，Dapr 挎斗将调用 `/twitter-mention` 服务上的终结点。</span><span class="sxs-lookup"><span data-stu-id="7e525-205">Every time a matching tweet is sent, the Dapr sidecar will invoke the `/twitter-mention` endpoint on the service.</span></span> <span data-ttu-id="7e525-206">它还将提供推文的内容。</span><span class="sxs-lookup"><span data-stu-id="7e525-206">It will also deliver the contents of the tweet.</span></span>

<span data-ttu-id="7e525-207">可以将绑定配置为输入和/或输出。</span><span class="sxs-lookup"><span data-stu-id="7e525-207">A binding can be configured for input, output, or both.</span></span> <span data-ttu-id="7e525-208">有趣的是，绑定未显式指定输入或输出配置。</span><span class="sxs-lookup"><span data-stu-id="7e525-208">Interestingly, the binding doesn't explicitly specify input or output configuration.</span></span> <span data-ttu-id="7e525-209">相反，方向通过使用绑定以及配置值进行推断。</span><span class="sxs-lookup"><span data-stu-id="7e525-209">Instead, the direction is inferred by the usage of the binding along with configuration values.</span></span>

<span data-ttu-id="7e525-210">[资源绑定的 Dapr 文档] [1] 提供了可用绑定的完整列表及其特定的配置设置。</span><span class="sxs-lookup"><span data-stu-id="7e525-210">The [Dapr documentation for resource bindings][1] provides a complete list of the available bindings and their specific configuration settings.</span></span>

### <a name="cron-binding"></a><span data-ttu-id="7e525-211">Cron 绑定</span><span class="sxs-lookup"><span data-stu-id="7e525-211">Cron binding</span></span>

<span data-ttu-id="7e525-212">请密切注意 Dapr 的 Cron 绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-212">Pay close attention to Dapr's Cron binding.</span></span> <span data-ttu-id="7e525-213">它不订阅外部系统中的事件。</span><span class="sxs-lookup"><span data-stu-id="7e525-213">It doesn't subscribe to events from an external system.</span></span> <span data-ttu-id="7e525-214">相反，此绑定使用可配置的间隔计划来触发你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="7e525-214">Instead, this binding uses a configurable interval schedule to trigger your application.</span></span> <span data-ttu-id="7e525-215">绑定提供了一种简单的方法来实现后台工作线程唤醒，并定期执行一些工作，而无需实现具有可配置延迟的无限循环。</span><span class="sxs-lookup"><span data-stu-id="7e525-215">The binding provides a simple way to implement a background worker to wake up and do some work at a regular interval, without the need to implement an endless loop with a configurable delay.</span></span> <span data-ttu-id="7e525-216">下面是 Cron 绑定配置的示例：</span><span class="sxs-lookup"><span data-stu-id="7e525-216">Here's an example of a Cron binding configuration:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: checkOrderBacklog
  namespace: default
spec:
  type: bindings.cron
  version: v1
  metadata:
  - name: schedule
    value: "@every 30m"
```

<span data-ttu-id="7e525-217">在此示例中，Dapr 通过 `/checkOrderBacklog` 每隔30分钟调用终结点来触发服务。</span><span class="sxs-lookup"><span data-stu-id="7e525-217">In this example, Dapr triggers a service by invoking the `/checkOrderBacklog` endpoint every 30 minutes.</span></span> <span data-ttu-id="7e525-218">有多种可用于指定值的模式 `schedule` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-218">There are several patterns available for specifying the `schedule` value.</span></span> <span data-ttu-id="7e525-219">有关详细信息，请参阅 [Cron 绑定文档](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/cron/)。</span><span class="sxs-lookup"><span data-stu-id="7e525-219">For more information, see the [Cron binding documentation](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/cron/).</span></span>

## <a name="reference-application-eshopondapr"></a><span data-ttu-id="7e525-220">引用应用程序： eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="7e525-220">Reference application: eShopOnDapr</span></span>

<span data-ttu-id="7e525-221">随附的 eShopOnDapr 引用应用程序实现了输出绑定示例。</span><span class="sxs-lookup"><span data-stu-id="7e525-221">The accompanying eShopOnDapr reference application implements an output binding example.</span></span> <span data-ttu-id="7e525-222">它触发 Dapr [SendGrid](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/sendgrid/) 绑定，以便在发出新订单时向用户发送电子邮件。</span><span class="sxs-lookup"><span data-stu-id="7e525-222">It triggers the Dapr [SendGrid](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/sendgrid/) binding to send a user an email when a new order is placed.</span></span> <span data-ttu-id="7e525-223">可以在 "组件" 文件夹的文件中找到此绑定 `eshop-email.yaml` ：</span><span class="sxs-lookup"><span data-stu-id="7e525-223">You can find this binding in the `eshop-email.yaml` file in the components folder:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: sendmail
  namespace: eshop
spec:
  type: bindings.twilio.sendgrid
  version: v1
  metadata:
  - name: apiKey
    secretKeyRef:
      name: sendGridAPIKey
auth:
  secretStore: eshop-secretstore
```

<span data-ttu-id="7e525-224">此配置使用 [Twilio SendGrid](https://github.com/dapr/components-contrib/tree/master/bindings/twilio) 绑定组件。</span><span class="sxs-lookup"><span data-stu-id="7e525-224">This configuration uses the [Twilio SendGrid](https://github.com/dapr/components-contrib/tree/master/bindings/twilio) binding component.</span></span> <span data-ttu-id="7e525-225">请注意，用于连接到服务的 API 密钥使用 Dapr 机密引用。</span><span class="sxs-lookup"><span data-stu-id="7e525-225">Note how the API key for connecting to the service consumes a Dapr secret reference.</span></span> <span data-ttu-id="7e525-226">此方法保留配置文件之外的机密。</span><span class="sxs-lookup"><span data-stu-id="7e525-226">This approach keeps secrets outside of the configuration file.</span></span> <span data-ttu-id="7e525-227">请参阅 [机密构建块一章](secrets.md) ，详细了解 Dapr 机密。</span><span class="sxs-lookup"><span data-stu-id="7e525-227">Read the [secrets building block chapter](secrets.md) to learn more about Dapr secrets.</span></span>

<span data-ttu-id="7e525-228">绑定配置指定一个绑定组件，该组件可使用 `/sendmail` Dapr 挎斗上的终结点进行调用。</span><span class="sxs-lookup"><span data-stu-id="7e525-228">The binding configuration specifies a binding component that can be invoked using the `/sendmail` endpoint on the Dapr sidecar.</span></span> <span data-ttu-id="7e525-229">以下代码片段将在每次启动订单时发送电子邮件：</span><span class="sxs-lookup"><span data-stu-id="7e525-229">Here's a code snippet in which an email is sent whenever an order is started:</span></span>

```csharp
public Task Handle(OrderStartedDomainEvent notification, CancellationToken cancellationToken)
{
    var string message = CreateEmailBody(notification);
    var metadata = new Dictionary<string, string>
    {
        {"emailFrom", "eShopOn@dapr.io"},
        {"emailTo", notification.UserName},
        {"subject", $"Your eShopOnDapr order #{notification.Order.Id}"}
    };
    return _daprClient.InvokeBindingAsync("sendmail", "create", message, metadata, cancellationToken);
}
```

<span data-ttu-id="7e525-230">如本示例中所示， `message` 包含消息正文。</span><span class="sxs-lookup"><span data-stu-id="7e525-230">As you can see in this example, `message` contains the message body.</span></span> <span data-ttu-id="7e525-231">`CreateEmailBody`方法只是将字符串的格式设置为正文文本。</span><span class="sxs-lookup"><span data-stu-id="7e525-231">The `CreateEmailBody` method simply formats a string with the body text.</span></span> <span data-ttu-id="7e525-232">指定电子邮件 `metadata` 发件人、收件人和电子邮件的主题。</span><span class="sxs-lookup"><span data-stu-id="7e525-232">The `metadata` specifies the email sender, recipient, and the subject for the email message.</span></span> <span data-ttu-id="7e525-233">如果这些值是静态的，则也可以将它们包含在配置文件的元数据字段中。</span><span class="sxs-lookup"><span data-stu-id="7e525-233">If these values are static, they can also be included in the metadata fields in the configuration file.</span></span> <span data-ttu-id="7e525-234">要调用的绑定的名称为 `sendmail` ，并且操作为 `create` 。</span><span class="sxs-lookup"><span data-stu-id="7e525-234">The name of the binding to invoke is `sendmail` and the operation is `create`.</span></span>

## <a name="summary"></a><span data-ttu-id="7e525-235">摘要</span><span class="sxs-lookup"><span data-stu-id="7e525-235">Summary</span></span>

<span data-ttu-id="7e525-236">Dapr 资源绑定使你可以与不同的外部资源和系统集成，而无需依赖于其库或 Sdk。</span><span class="sxs-lookup"><span data-stu-id="7e525-236">Dapr resource bindings enable you to integrate with different external resources and systems without taking dependencies on their libraries or SDKs.</span></span> <span data-ttu-id="7e525-237">这些外部系统不一定必须是消息传递系统，例如服务总线或消息代理。</span><span class="sxs-lookup"><span data-stu-id="7e525-237">These external systems don't necessarily have to be messaging systems like a service bus or message broker.</span></span> <span data-ttu-id="7e525-238">数据存储和 web 资源（如 Twitter 或 SendGrid）还存在绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-238">Bindings also exist for datastores and web resources like Twitter or SendGrid.</span></span>

<span data-ttu-id="7e525-239">输入绑定 (或触发器) 对外部系统中发生的事件做出响应。</span><span class="sxs-lookup"><span data-stu-id="7e525-239">Input bindings (or triggers) react to events occurring in an external system.</span></span> <span data-ttu-id="7e525-240">它们调用在你的应用程序中预先配置的公共 HTTP 终结点。</span><span class="sxs-lookup"><span data-stu-id="7e525-240">They invoke the public HTTP endpoints pre-configured in your application.</span></span> <span data-ttu-id="7e525-241">Dapr 使用配置中的绑定名称来确定要在应用程序中调用的终结点。</span><span class="sxs-lookup"><span data-stu-id="7e525-241">Dapr uses the name of the binding in the configuration to determine the endpoint to call in your application.</span></span>

<span data-ttu-id="7e525-242">输出绑定将消息发送到外部系统。</span><span class="sxs-lookup"><span data-stu-id="7e525-242">Output bindings will send messages to an external system.</span></span> <span data-ttu-id="7e525-243">通过在 `/v1.0/bindings/<binding-name>` Dapr 挎斗上的终结点上执行 HTTP POST 来触发输出绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-243">You trigger an output binding by doing an HTTP POST on the `/v1.0/bindings/<binding-name>` endpoint on the Dapr sidecar.</span></span> <span data-ttu-id="7e525-244">还可以使用 gRPC 来调用绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-244">You can also use gRPC to invoke the binding.</span></span> <span data-ttu-id="7e525-245">.NET SDK 提供 `InvokeBindingAsync` 使用 gRPC 调用 Dapr 绑定的方法。</span><span class="sxs-lookup"><span data-stu-id="7e525-245">The .NET SDK offers a `InvokeBindingAsync` method to invoke Dapr bindings using gRPC.</span></span>

<span data-ttu-id="7e525-246">使用 Dapr 组件实现绑定。</span><span class="sxs-lookup"><span data-stu-id="7e525-246">You implement a binding with a Dapr component.</span></span> <span data-ttu-id="7e525-247">这些组件由社区提供。</span><span class="sxs-lookup"><span data-stu-id="7e525-247">These components are contributed by the community.</span></span> <span data-ttu-id="7e525-248">每个绑定组件的配置都有特定于它所抽象的外部系统的元数据。</span><span class="sxs-lookup"><span data-stu-id="7e525-248">Each binding component's configuration has metadata that is specific for the external system it abstracts.</span></span> <span data-ttu-id="7e525-249">此外，它支持的命令和负载的结构将因绑定组件而异。</span><span class="sxs-lookup"><span data-stu-id="7e525-249">Also, the commands it supports and the structure of the payload will differ per binding component.</span></span>

### <a name="references"></a><span data-ttu-id="7e525-250">参考</span><span class="sxs-lookup"><span data-stu-id="7e525-250">References</span></span>

- [<span data-ttu-id="7e525-251">资源绑定的 Dapr 文档</span><span class="sxs-lookup"><span data-stu-id="7e525-251">Dapr documentation for resource bindings</span></span>](https://docs.dapr.io/operations/components/setup-bindings/supported-bindings/)

>[!div class="step-by-step"]
><span data-ttu-id="7e525-252">[上一页](publish-subscribe.md)
>[下一页](observability.md)</span><span class="sxs-lookup"><span data-stu-id="7e525-252">[Previous](publish-subscribe.md)
[Next](observability.md)</span></span>
