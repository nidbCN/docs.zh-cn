---
title: 通过 Polly 实现使用指数退避算法的 HTTP 调用重试
description: 了解如何使用 Polly 和 IHttpClientFactory 处理 HTTP 故障。
ms.date: 01/13/2021
ms.openlocfilehash: c2831f73ed38b48fd32fa241f8fe1792b9adf3d4
ms.sourcegitcommit: 1d3af230ec30d8d061be7a887f6ba38a530c4ece
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511783"
---
# <a name="implement-http-call-retries-with-exponential-backoff-with-ihttpclientfactory-and-polly-policies"></a><span data-ttu-id="4db75-103">通过 IHttpClientFactory 和 Polly 策略实现使用指数退避算法的 HTTP 调用重试</span><span class="sxs-lookup"><span data-stu-id="4db75-103">Implement HTTP call retries with exponential backoff with IHttpClientFactory and Polly policies</span></span>

<span data-ttu-id="4db75-104">建议的使用指数退避算法的重试方法是利用更高级的 .NET 库，如开放源 [Polly](https://github.com/App-vNext/Polly) 库。</span><span class="sxs-lookup"><span data-stu-id="4db75-104">The recommended approach for retries with exponential backoff is to take advantage of more advanced .NET libraries like the open-source [Polly library](https://github.com/App-vNext/Polly).</span></span>

<span data-ttu-id="4db75-105">Polly 是一个 .NET 库，提供恢复能力和瞬态故障处理功能。</span><span class="sxs-lookup"><span data-stu-id="4db75-105">Polly is a .NET library that provides resilience and transient-fault handling capabilities.</span></span> <span data-ttu-id="4db75-106">通过应用 Polly 策略（如重试、断路器、舱壁隔离、超时和回退）即可实现这些功能。</span><span class="sxs-lookup"><span data-stu-id="4db75-106">You can implement those capabilities by applying Polly policies such as Retry, Circuit Breaker, Bulkhead Isolation, Timeout, and Fallback.</span></span> <span data-ttu-id="4db75-107">Polly 面向 .NET Framework 4.x 和 .NET Standard 1.0、1.1 和 2.0（支持 .NET Core 及更高版本）。</span><span class="sxs-lookup"><span data-stu-id="4db75-107">Polly targets .NET Framework 4.x and .NET Standard 1.0, 1.1, and 2.0 (which supports .NET Core and later).</span></span>

<span data-ttu-id="4db75-108">以下步骤说明如何通过集成到 `IHttpClientFactory` 中的 Polly（已在上一部分中说明）使用 Http 重试。</span><span class="sxs-lookup"><span data-stu-id="4db75-108">The following steps show how you can use Http retries with Polly integrated into `IHttpClientFactory`, which is explained in the previous section.</span></span>

<span data-ttu-id="4db75-109">**引用 .NET 5 包**</span><span class="sxs-lookup"><span data-stu-id="4db75-109">**Reference the .NET 5 packages**</span></span>

<span data-ttu-id="4db75-110">自 .NET Core 2.1 起提供 `IHttpClientFactory`，但建议在项目中使用 NuGet 中的最新 .NET 5 包。</span><span class="sxs-lookup"><span data-stu-id="4db75-110">`IHttpClientFactory` is available since .NET Core 2.1 however we recommend you to use the latest .NET 5 packages from NuGet in your project.</span></span> <span data-ttu-id="4db75-111">通常，还需要引用扩展包 `Microsoft.Extensions.Http.Polly`。</span><span class="sxs-lookup"><span data-stu-id="4db75-111">You typically also need to reference the extension package `Microsoft.Extensions.Http.Polly`.</span></span>

<span data-ttu-id="4db75-112">**使用 Polly 的重试策略在 Startup 中配置客户端**</span><span class="sxs-lookup"><span data-stu-id="4db75-112">**Configure a client with Polly's Retry policy, in Startup**</span></span>

<span data-ttu-id="4db75-113">如前面部分中所示，需要在标准 Startup.ConfigureServices(...) 方法中定义已命名或类型化的客户端 HttpClient 配置，但现在，可使用指数退避算法添加用于指定 Http 重试策略的增量代码，如下所示：</span><span class="sxs-lookup"><span data-stu-id="4db75-113">As shown in previous sections, you need to define a named or typed client HttpClient configuration in your standard Startup.ConfigureServices(...) method, but now, you add incremental code specifying the policy for the Http retries with exponential backoff, as below:</span></span>

```csharp
//ConfigureServices()  - Startup.cs
services.AddHttpClient<IBasketService, BasketService>()
        .SetHandlerLifetime(TimeSpan.FromMinutes(5))  //Set lifetime to five minutes
        .AddPolicyHandler(GetRetryPolicy());
```

<span data-ttu-id="4db75-114">将策略添加至将要使用的 `HttpClient` 对象需要使用 AddPolicyHandler() 方法。</span><span class="sxs-lookup"><span data-stu-id="4db75-114">The **AddPolicyHandler()** method is what adds policies to the `HttpClient` objects you'll use.</span></span> <span data-ttu-id="4db75-115">在此示例中，将使用指数退避算法为 Http 重试添加 Polly 策略。</span><span class="sxs-lookup"><span data-stu-id="4db75-115">In this case, it's adding a Polly's policy for Http Retries with exponential backoff.</span></span>

<span data-ttu-id="4db75-116">若要获得更加模块化的方法，可在 `Startup.cs` 文件内的单独方法中定义 Http 重试策略，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="4db75-116">To have a more modular approach, the Http Retry Policy can be defined in a separate method within the `Startup.cs` file, as shown in the following code:</span></span>

```csharp
static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
        .WaitAndRetryAsync(6, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2,
                                                                    retryAttempt)));
}
```

<span data-ttu-id="4db75-117">使用 Polly 可定义一个重试策略，其中包含重试次数、指数退避算法配置以及在出现 HTTP 异常时要采取的操作，例如记录错误。</span><span class="sxs-lookup"><span data-stu-id="4db75-117">With Polly, you can define a Retry policy with the number of retries, the exponential backoff configuration, and the actions to take when there's an HTTP exception, such as logging the error.</span></span> <span data-ttu-id="4db75-118">在此示例中，该策略配置为尝试 6 次（采用指数重试），以 2 秒为起始值。</span><span class="sxs-lookup"><span data-stu-id="4db75-118">In this case, the policy is configured to try six times with an exponential retry, starting at two seconds.</span></span>

## <a name="add-a-jitter-strategy-to-the-retry-policy"></a><span data-ttu-id="4db75-119">将抖动策略添加到重试策略</span><span class="sxs-lookup"><span data-stu-id="4db75-119">Add a jitter strategy to the retry policy</span></span>

<span data-ttu-id="4db75-120">在高并发率、高可伸缩性和高争用的情况下，常规重试策略可能会对系统产生影响。</span><span class="sxs-lookup"><span data-stu-id="4db75-120">A regular Retry policy can impact your system in cases of high concurrency and scalability and under high contention.</span></span> <span data-ttu-id="4db75-121">在部分运行中断的情况下，有可能会有许多客户端同时发出相似的重试操作，从而形成操作高峰，为克服这种情况，一个好办法是向重试算法或策略中添加抖动策略。</span><span class="sxs-lookup"><span data-stu-id="4db75-121">To overcome peaks of similar retries coming from many clients in case of partial outages, a good workaround is to add a jitter strategy to the retry algorithm/policy.</span></span> <span data-ttu-id="4db75-122">由于增加了指数退避的随机性，这可能会改进端到端系统的整体性能。</span><span class="sxs-lookup"><span data-stu-id="4db75-122">This can improve the overall performance of the end-to-end system by adding randomness to the exponential backoff.</span></span> <span data-ttu-id="4db75-123">这样在出现问题时可以分散峰值。</span><span class="sxs-lookup"><span data-stu-id="4db75-123">This spreads out the spikes when issues arise.</span></span> <span data-ttu-id="4db75-124">以下示例说明了这一原理：</span><span class="sxs-lookup"><span data-stu-id="4db75-124">The principle is illustrated by the following example:</span></span>

```csharp
Random jitterer = new Random();
var retryWithJitterPolicy = HttpPolicyExtensions
    .HandleTransientHttpError()
    .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
    .WaitAndRetryAsync(6,    // exponential back-off plus some jitter
        retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt))  
                      + TimeSpan.FromMilliseconds(jitterer.Next(0, 100))
    );
```

<span data-ttu-id="4db75-125">Polly 通过项目网站提供了可用于生产的 jitter 算法。</span><span class="sxs-lookup"><span data-stu-id="4db75-125">Polly provides production-ready jitter algorithms via the project website.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4db75-126">其他资源</span><span class="sxs-lookup"><span data-stu-id="4db75-126">Additional resources</span></span>

- <span data-ttu-id="4db75-127">**重试模式**</span><span class="sxs-lookup"><span data-stu-id="4db75-127">**Retry pattern**</span></span>  
  [https://docs.microsoft.com/azure/architecture/patterns/retry](/azure/architecture/patterns/retry)

- <span data-ttu-id="4db75-128">**Polly 和 IHttpClientFactory**</span><span class="sxs-lookup"><span data-stu-id="4db75-128">**Polly and IHttpClientFactory**</span></span>  
  <https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory>

- <span data-ttu-id="4db75-129">**Polly（.NET 的恢复和暂时性故障处理库）**</span><span class="sxs-lookup"><span data-stu-id="4db75-129">**Polly (.NET resilience and transient-fault-handling library)**</span></span>  
  <https://github.com/App-vNext/Polly>

- <span data-ttu-id="4db75-130">**Polly：使用 Jitter 重试**</span><span class="sxs-lookup"><span data-stu-id="4db75-130">**Polly: Retry with Jitter**</span></span>  
  <https://github.com/App-vNext/Polly/wiki/Retry-with-jitter>

- <span data-ttu-id="4db75-131">**Marc Brooker。抖动：随机性使操作变得更好**</span><span class="sxs-lookup"><span data-stu-id="4db75-131">**Marc Brooker. Jitter: Making Things Better With Randomness**</span></span>  
  <https://brooker.co.za/blog/2015/03/21/backoff.html>

>[!div class="step-by-step"]
><span data-ttu-id="4db75-132">[上一页](use-httpclientfactory-to-implement-resilient-http-requests.md)
>[下一页](implement-circuit-breaker-pattern.md)</span><span class="sxs-lookup"><span data-stu-id="4db75-132">[Previous](use-httpclientfactory-to-implement-resilient-http-requests.md)
[Next](implement-circuit-breaker-pattern.md)</span></span>
