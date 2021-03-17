---
title: Dapr 机密构建基块
description: 机密构建基块、功能、权益和应用方法的描述
author: edwinvw
ms.date: 02/17/2021
ms.openlocfilehash: 52b899b4d496aab6762f69bbee99faecfcd23d59
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623741"
---
# <a name="the-dapr-secrets-building-block"></a><span data-ttu-id="a4800-103">Dapr 机密构建基块</span><span class="sxs-lookup"><span data-stu-id="a4800-103">The Dapr secrets building block</span></span>

<span data-ttu-id="a4800-104">企业应用程序要求机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-104">Enterprise applications require secrets.</span></span> <span data-ttu-id="a4800-105">常见示例包括：</span><span class="sxs-lookup"><span data-stu-id="a4800-105">Common examples include:</span></span>

- <span data-ttu-id="a4800-106">包含用户名和密码的数据库连接字符串。</span><span class="sxs-lookup"><span data-stu-id="a4800-106">A database connection string that contains a username and password.</span></span>
- <span data-ttu-id="a4800-107">用于调用外部 web API 的 API 密钥。</span><span class="sxs-lookup"><span data-stu-id="a4800-107">An API key for calling an external web API.</span></span>
- <span data-ttu-id="a4800-108">用于对外部系统进行身份验证的客户端证书。</span><span class="sxs-lookup"><span data-stu-id="a4800-108">A client certificate for authenticating to an external system.</span></span>

<span data-ttu-id="a4800-109">必须谨慎管理机密，使其不会在应用程序外公开。</span><span class="sxs-lookup"><span data-stu-id="a4800-109">Secrets must be carefully managed so that they're never disclosed outside of the application.</span></span>

<span data-ttu-id="a4800-110">不久前，将应用程序机密存储在应用程序基本代码内的配置文件中是很常见的。</span><span class="sxs-lookup"><span data-stu-id="a4800-110">Not long ago, it was popular to store application secrets in a configuration file inside the application codebase.</span></span> <span data-ttu-id="a4800-111">.NET 开发人员将 fondly 回忆 *web.config* 的文件。</span><span class="sxs-lookup"><span data-stu-id="a4800-111">.NET developers will fondly recall the *web.config* file.</span></span> <span data-ttu-id="a4800-112">虽然实现起来很简单，但将机密与代码集成在一起并不安全。</span><span class="sxs-lookup"><span data-stu-id="a4800-112">While simple to implement, integrating secrets to along with code was far from secure.</span></span> <span data-ttu-id="a4800-113">常见的失误是在推送到公共 GIT 存储库时包括文件，从而向世界公开机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-113">A common misstep was to include the file when pushing to a public GIT repository, exposing the secrets to the world.</span></span>

<span data-ttu-id="a4800-114">用于构造新式分布式应用程序的一种广泛接受的方法是 [Twelve-Factor 应用程序](https://12factor.net/)。</span><span class="sxs-lookup"><span data-stu-id="a4800-114">A widely accepted methodology for constructing modern distributed applications is [The Twelve-Factor App](https://12factor.net/).</span></span> <span data-ttu-id="a4800-115">其中介绍了一系列原则和最佳实践。</span><span class="sxs-lookup"><span data-stu-id="a4800-115">It describes a set of principles and best practices.</span></span> <span data-ttu-id="a4800-116">第三个因素规定， *配置和机密外部化在基本代码外。*</span><span class="sxs-lookup"><span data-stu-id="a4800-116">Its third factor prescribes that *configuration and secrets be externalized outside of the code base.*</span></span>

<span data-ttu-id="a4800-117">为了解决这一问题，.NET Core 平台包含一个 [密钥管理器](/aspnet/core/security/app-secrets#secret-manager) 功能，该功能将敏感数据存储在项目树之外的物理文件夹中。</span><span class="sxs-lookup"><span data-stu-id="a4800-117">To address this concern, the .NET Core platform includes a [Secret Manager](/aspnet/core/security/app-secrets#secret-manager) feature that stores sensitive data in a physical folder outside of the project tree.</span></span> <span data-ttu-id="a4800-118">虽然机密不受源代码管理，但此功能不会对数据进行加密。</span><span class="sxs-lookup"><span data-stu-id="a4800-118">While secrets are outside of source control, this feature doesn't encrypt data.</span></span> <span data-ttu-id="a4800-119">它仅用于 **开发目的** 。</span><span class="sxs-lookup"><span data-stu-id="a4800-119">It's designed for **development purposes** only.</span></span>

<span data-ttu-id="a4800-120">更新式、安全的做法是在 **Hashicorp 保管库** 或 **Azure Key Vault** 等机密管理工具中隔离机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-120">A more modern and secure practice is to isolate secrets in a secrets management tool like **Hashicorp Vault** or **Azure Key Vault**.</span></span>  <span data-ttu-id="a4800-121">这些工具使你能够将机密存储在外部，并跨环境改变凭据，并从应用程序代码中引用它们。</span><span class="sxs-lookup"><span data-stu-id="a4800-121">These tools enable you to store secrets externally, vary credentials across environments, and reference them from application code.</span></span> <span data-ttu-id="a4800-122">然而，每个工具都有其复杂性和学习曲线。</span><span class="sxs-lookup"><span data-stu-id="a4800-122">However, each tool has its complexities and learning curve.</span></span>

<span data-ttu-id="a4800-123">Dapr 提供简化密钥管理的构建基块。</span><span class="sxs-lookup"><span data-stu-id="a4800-123">Dapr offers a building block that simplifies managing secrets.</span></span>

## <a name="what-it-solves"></a><span data-ttu-id="a4800-124">解决方法</span><span class="sxs-lookup"><span data-stu-id="a4800-124">What it solves</span></span>

<span data-ttu-id="a4800-125">Dapr [机密构建块](https://docs.dapr.io/developing-applications/building-blocks/secrets/secrets-overview/) 消除了使用机密和机密管理工具的复杂性。</span><span class="sxs-lookup"><span data-stu-id="a4800-125">The Dapr [secrets building block](https://docs.dapr.io/developing-applications/building-blocks/secrets/secrets-overview/) abstracts away the complexity of working with secrets and secret management tools.</span></span>

- <span data-ttu-id="a4800-126">它通过统一的接口隐藏基础管道。</span><span class="sxs-lookup"><span data-stu-id="a4800-126">It hides the underlying plumbing through a unified interface.</span></span>
- <span data-ttu-id="a4800-127">它支持各种可 *插入* 的机密存储组件，这些组件在开发和生产之间有所不同。</span><span class="sxs-lookup"><span data-stu-id="a4800-127">It supports various *pluggable* secret store components, which can vary between development and production.</span></span>
- <span data-ttu-id="a4800-128">应用程序不要求直接依赖于密钥存储库。</span><span class="sxs-lookup"><span data-stu-id="a4800-128">Applications don't require direct dependencies on secret store libraries.</span></span>
- <span data-ttu-id="a4800-129">开发人员无需详细了解每个密钥存储。</span><span class="sxs-lookup"><span data-stu-id="a4800-129">Developers don't require detailed knowledge of each secret store.</span></span>

<span data-ttu-id="a4800-130">Dapr 处理上述所有问题。</span><span class="sxs-lookup"><span data-stu-id="a4800-130">Dapr handles all of the above concerns.</span></span>

<span data-ttu-id="a4800-131">通过身份验证和授权来保护对机密的访问。</span><span class="sxs-lookup"><span data-stu-id="a4800-131">Access to the secrets is secured through authentication and authorization.</span></span> <span data-ttu-id="a4800-132">只有具有足够权限的应用程序可以访问机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-132">Only an application with sufficient rights can access secrets.</span></span> <span data-ttu-id="a4800-133">在 Kubernetes 中运行的应用程序也可以使用其内置的机密管理机制。</span><span class="sxs-lookup"><span data-stu-id="a4800-133">Applications running in Kubernetes can also use its built-in secrets management mechanism.</span></span>

## <a name="how-it-works"></a><span data-ttu-id="a4800-134">工作原理</span><span class="sxs-lookup"><span data-stu-id="a4800-134">How it works</span></span>

<span data-ttu-id="a4800-135">应用程序通过两种方式使用机密构建基块：</span><span class="sxs-lookup"><span data-stu-id="a4800-135">Applications use the secrets building block in two ways:</span></span>

- <span data-ttu-id="a4800-136">直接从应用程序块检索机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-136">Retrieve a secret directly from the application block.</span></span>
- <span data-ttu-id="a4800-137">从 Dapr 组件配置中间接引用机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-137">Reference a secret indirectly from a Dapr component configuration.</span></span>

<span data-ttu-id="a4800-138">首先介绍如何直接检索机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-138">Retrieving secrets directly is covered first.</span></span> <span data-ttu-id="a4800-139">在后面的部分中对从 Dapr 组件配置文件引用机密进行了说明。</span><span class="sxs-lookup"><span data-stu-id="a4800-139">Referencing a secret from a Dapr component configuration file is addressed in a later section.</span></span>

<span data-ttu-id="a4800-140">使用机密构建块时，应用程序与 Dapr 挎斗交互。</span><span class="sxs-lookup"><span data-stu-id="a4800-140">The application interacts with a Dapr sidecar when using the secrets building block.</span></span> <span data-ttu-id="a4800-141">挎斗公开了机密 API。</span><span class="sxs-lookup"><span data-stu-id="a4800-141">The sidecar exposes the secrets API.</span></span> <span data-ttu-id="a4800-142">可以通过 HTTP 或 gRPC 调用 API。</span><span class="sxs-lookup"><span data-stu-id="a4800-142">The API can be called with either HTTP or gRPC.</span></span> <span data-ttu-id="a4800-143">使用以下 URL 调用 HTTP API：</span><span class="sxs-lookup"><span data-stu-id="a4800-143">Use the following URL to call the HTTP API:</span></span>

```http
http://localhost:<dapr-port>/v1.0/secrets/<store-name>/<name>?<metadata>
```

<span data-ttu-id="a4800-144">URL 包含以下段：</span><span class="sxs-lookup"><span data-stu-id="a4800-144">The URL contains the following segments:</span></span>

- <span data-ttu-id="a4800-145">`<dapr-port>` 指定 Dapr 挎斗正在侦听的端口号。</span><span class="sxs-lookup"><span data-stu-id="a4800-145">`<dapr-port>` specifies the port number upon which the Dapr sidecar is listening.</span></span>
- <span data-ttu-id="a4800-146">`<store-name>` 指定 Dapr 密钥存储的名称。</span><span class="sxs-lookup"><span data-stu-id="a4800-146">`<store-name>` specifies the name of the Dapr secret store.</span></span>
- <span data-ttu-id="a4800-147">`<name>` 指定要检索的机密的名称。</span><span class="sxs-lookup"><span data-stu-id="a4800-147">`<name>` specifies  the name of the secret to retrieve.</span></span>
- <span data-ttu-id="a4800-148">`<metadata>` 提供机密的附加信息。</span><span class="sxs-lookup"><span data-stu-id="a4800-148">`<metadata>` provides additional information for the secret.</span></span> <span data-ttu-id="a4800-149">此段是可选的，每个密钥存储的元数据属性各不相同。</span><span class="sxs-lookup"><span data-stu-id="a4800-149">This segment is optional and metadata properties differ per secret store.</span></span> <span data-ttu-id="a4800-150">有关元数据属性的详细信息，请参阅 [机密 API 参考]([Secrets API reference | Dapr Docs](https://docs.dapr.io/reference/api/secrets_api/))。</span><span class="sxs-lookup"><span data-stu-id="a4800-150">For more information on metadata properties, see the [secrets API reference]([Secrets API reference | Dapr Docs](https://docs.dapr.io/reference/api/secrets_api/)).</span></span>

 > [!NOTE]
 > <span data-ttu-id="a4800-151">上述 URL 表示可用于支持 HTTP 或 gRPC 的任何开发平台的本机 Dapr API 调用。</span><span class="sxs-lookup"><span data-stu-id="a4800-151">The above URL represents the native Dapr API call available to any development platform that supports HTTP or gRPC.</span></span> <span data-ttu-id="a4800-152">.NET、Java 和中转等常用平台都有自己的自定义 Api。</span><span class="sxs-lookup"><span data-stu-id="a4800-152">Popular platforms like .NET, Java, and Go have their own custom APIs.</span></span>

<span data-ttu-id="a4800-153">JSON 响应包含密钥和机密的值。</span><span class="sxs-lookup"><span data-stu-id="a4800-153">The JSON response contains the key and value of the secret.</span></span>

<span data-ttu-id="a4800-154">图10-1 显示了 Dapr 如何处理机密 API 请求：</span><span class="sxs-lookup"><span data-stu-id="a4800-154">Figure 10-1 shows how Dapr handles a request for the secrets API:</span></span>

![使用 Dapr 机密 API 检索机密的示意图。](media/secrets/secrets-flow.png)

<span data-ttu-id="a4800-156">**图 10-1**。</span><span class="sxs-lookup"><span data-stu-id="a4800-156">**Figure 10-1**.</span></span> <span data-ttu-id="a4800-157">使用 Dapr 机密 API 检索机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-157">Retrieving a secret with the Dapr secrets API.</span></span>

1. <span data-ttu-id="a4800-158">服务调用 Dapr 机密 API 以及密钥存储的名称和要检索的机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-158">The service calls the Dapr secrets API, along with the name of the secret store, and secret to retrieve.</span></span>
1. <span data-ttu-id="a4800-159">Dapr 挎斗从机密存储中检索指定的机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-159">The Dapr sidecar retrieves the specified secret from the secret store.</span></span>
1. <span data-ttu-id="a4800-160">Dapr 挎斗将机密信息返回给服务。</span><span class="sxs-lookup"><span data-stu-id="a4800-160">The Dapr sidecar returns the secret information back to the service.</span></span>

<span data-ttu-id="a4800-161">某些机密存储支持在单个机密中存储多个键/值对。</span><span class="sxs-lookup"><span data-stu-id="a4800-161">Some secret stores support storing multiple key/value pairs in a single secret.</span></span> <span data-ttu-id="a4800-162">对于这些方案，响应会在单个 JSON 响应中包含多个键/值对，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="a4800-162">For those scenarios, the response would contain multiple key/value pairs in a single JSON response as in the following example:</span></span>

```http
GET http://localhost:3500/v1.0/secrets/secret-store/interestRates?metadata.version_id=3
```

```json
{
  "tier1-percentage": "2.5",
  "tier2-percentage": "3.8",
  "tier3-percentage": "5.1"
}
```

<span data-ttu-id="a4800-163">Dapr 机密 API 还提供了一个操作，用于检索应用程序有权访问的所有机密：</span><span class="sxs-lookup"><span data-stu-id="a4800-163">The Dapr secrets API also offers an operation to retrieve all the secrets the application has access to:</span></span>

```http
http://localhost:<dapr-port>/v1.0/secrets/<store-name>/bulk
```

## <a name="use-the-dapr-net-sdk"></a><span data-ttu-id="a4800-164">使用 Dapr .NET SDK</span><span class="sxs-lookup"><span data-stu-id="a4800-164">Use the Dapr .NET SDK</span></span>

<span data-ttu-id="a4800-165">对于 .NET 开发人员而言，Dapr .NET SDK 精简了 Dapr 的密钥管理。</span><span class="sxs-lookup"><span data-stu-id="a4800-165">For .NET developers, the Dapr .NET SDK streamlines Dapr secret management.</span></span> <span data-ttu-id="a4800-166">请考虑 `DaprClient.GetSecretAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="a4800-166">Consider the `DaprClient.GetSecretAsync` method.</span></span> <span data-ttu-id="a4800-167">它使你能够以最小的努力直接从任何 Dapr 密钥存储中检索机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-167">It enables you to retrieve a secret directly from any Dapr secret store with minimal effort.</span></span> <span data-ttu-id="a4800-168">下面是一个获取 SQL Server 数据库的连接字符串机密的示例：</span><span class="sxs-lookup"><span data-stu-id="a4800-168">Here's an example of fetching a connection string secret for a SQL Server database:</span></span>

```csharp
var metadata = new Dictionary<string, string> { ["version_id"] = "3" };
Dictionary<string, string> secrets = await daprClient.GetSecretAsync("secret-store", "eshopsecrets", metadata);
string connectionString = secrets["customerdb"];
```

<span data-ttu-id="a4800-169">方法的参数 `GetSecretAsync` 包括：</span><span class="sxs-lookup"><span data-stu-id="a4800-169">Arguments for the `GetSecretAsync` method include:</span></span>

- <span data-ttu-id="a4800-170"> ( "secret store" 的 Dapr 机密存储区组件的名称 ) </span><span class="sxs-lookup"><span data-stu-id="a4800-170">The name of the Dapr secret store component ('secret-store')</span></span>
- <span data-ttu-id="a4800-171">要检索的机密 ( "eshopsecrets" ) </span><span class="sxs-lookup"><span data-stu-id="a4800-171">The secret to retrieve ('eshopsecrets')</span></span>
- <span data-ttu-id="a4800-172">可选的元数据键/值对 ( "version_id = 3" ) </span><span class="sxs-lookup"><span data-stu-id="a4800-172">Optional metadata key/value pairs ('version_id=3')</span></span>

<span data-ttu-id="a4800-173">方法使用字典对象进行响应，因为机密可包含多个键/值对。</span><span class="sxs-lookup"><span data-stu-id="a4800-173">The method responds with a dictionary object as a secret can contain multiple key/value pairs.</span></span> <span data-ttu-id="a4800-174">在上面的示例中，从集合引用名为的机密 `customerdb` 以返回连接字符串。</span><span class="sxs-lookup"><span data-stu-id="a4800-174">In the example above, the secret named `customerdb` is referenced from the collection to return a connection string.</span></span>

<span data-ttu-id="a4800-175">Dapr .NET SDK 还提供 .NET 配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a4800-175">The Dapr .NET SDK also features a .NET configuration provider.</span></span> <span data-ttu-id="a4800-176">它将指定的机密加载到基础 [.Net Core 配置 API](../../core/extensions/configuration.md)。</span><span class="sxs-lookup"><span data-stu-id="a4800-176">It loads specified secrets into the underlying [.NET Core configuration API](../../core/extensions/configuration.md).</span></span> <span data-ttu-id="a4800-177">然后，正在运行的应用程序可以从 `IConfiguration` 在 ASP.NET Core 依赖关系注入中注册的字典中引用机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-177">The running application can then reference secrets from the `IConfiguration` dictionary that is registered in ASP.NET Core dependency injection.</span></span>

<span data-ttu-id="a4800-178">[Dapr.Extensions.Configu](https://www.nuget.org/packages/Dapr.Extensions.Configuration) NuGet 包中提供了机密配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a4800-178">The secrets configuration provider is available from the [Dapr.Extensions.Configuration](https://www.nuget.org/packages/Dapr.Extensions.Configuration) NuGet package.</span></span> <span data-ttu-id="a4800-179">可在 ASP.NET Web API 应用程序的中注册提供程序 `Program.cs` ：</span><span class="sxs-lookup"><span data-stu-id="a4800-179">The provider can be registered in the `Program.cs` of an ASP.NET Web API application:</span></span>  

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration(config =>
        {
            var daprClient = new DaprClientBuilder().Build();
            var secretDescriptors = new List<DaprSecretDescriptor>
            {
                new DaprSecretDescriptor("eshopsecrets")
            };
            config.AddDaprSecretStore("secret-store", secretDescriptors, daprClient);
        })
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="a4800-180">上述示例 `eshopsecrets` 在启动时将机密集合加载到 .net 配置系统中。</span><span class="sxs-lookup"><span data-stu-id="a4800-180">The above example loads the `eshopsecrets` secrets collection into the .NET configuration system at startup.</span></span> <span data-ttu-id="a4800-181">注册提供程序需要的实例 `DaprClient` ，以在 Dapr 挎斗上调用密码 API。</span><span class="sxs-lookup"><span data-stu-id="a4800-181">Registering the provider requires an instance of `DaprClient` to invoke the secrets API on the Dapr sidecar.</span></span> <span data-ttu-id="a4800-182">其他参数包括密钥存储的名称，以及包含 `DaprSecretDescriptor` 机密名称的对象。</span><span class="sxs-lookup"><span data-stu-id="a4800-182">The other arguments include the name of the secret store and a `DaprSecretDescriptor` object with the name of the secret.</span></span>

<span data-ttu-id="a4800-183">加载后，可以直接从应用程序代码中检索机密：</span><span class="sxs-lookup"><span data-stu-id="a4800-183">Once loaded, you can retrieve secrets directly from application code:</span></span>

```csharp
public void GetCustomer(IConfiguration config)
{
    var connectionString = config["eshopsecrets"]["customerdb"];
}
```

## <a name="secret-store-components"></a><span data-ttu-id="a4800-184">机密存储区组件</span><span class="sxs-lookup"><span data-stu-id="a4800-184">Secret store components</span></span>

<span data-ttu-id="a4800-185">机密构建块支持多个机密存储区组件。</span><span class="sxs-lookup"><span data-stu-id="a4800-185">The secrets building block supports several secret store components.</span></span> <span data-ttu-id="a4800-186">撰写本文时，可以使用以下密钥存储：</span><span class="sxs-lookup"><span data-stu-id="a4800-186">At the time of writing, the following secret stores are available:</span></span>

- <span data-ttu-id="a4800-187">环境变量</span><span class="sxs-lookup"><span data-stu-id="a4800-187">Environment Variables</span></span>
- <span data-ttu-id="a4800-188">本地文件</span><span class="sxs-lookup"><span data-stu-id="a4800-188">Local file</span></span>
- <span data-ttu-id="a4800-189">Kubernetes 机密</span><span class="sxs-lookup"><span data-stu-id="a4800-189">Kubernetes secrets</span></span>
- <span data-ttu-id="a4800-190">AWS 机密管理器</span><span class="sxs-lookup"><span data-stu-id="a4800-190">AWS Secrets Manager</span></span>
- <span data-ttu-id="a4800-191">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a4800-191">Azure Key Vault</span></span>
- <span data-ttu-id="a4800-192">GCP 密钥管理器</span><span class="sxs-lookup"><span data-stu-id="a4800-192">GCP Secret Manager</span></span>
- <span data-ttu-id="a4800-193">HashiCorp 保管库</span><span class="sxs-lookup"><span data-stu-id="a4800-193">HashiCorp Vault</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4800-194">环境变量和本地文件组件仅适用于开发工作负荷。</span><span class="sxs-lookup"><span data-stu-id="a4800-194">The environment variables and local file components are designed for development workloads only.</span></span>

<span data-ttu-id="a4800-195">以下部分说明如何配置密钥存储。</span><span class="sxs-lookup"><span data-stu-id="a4800-195">The following sections show how to configure a secret store.</span></span>

### <a name="configuration"></a><span data-ttu-id="a4800-196">Configuration</span><span class="sxs-lookup"><span data-stu-id="a4800-196">Configuration</span></span>

<span data-ttu-id="a4800-197">使用 Dapr 组件配置文件配置密钥存储。</span><span class="sxs-lookup"><span data-stu-id="a4800-197">You configure a secret store using a Dapr component configuration file.</span></span> <span data-ttu-id="a4800-198">文件的典型结构如下所示：</span><span class="sxs-lookup"><span data-stu-id="a4800-198">The typical structure of the file is shown below:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: [component name]
  namespace: [namespace]
spec:
  type: secretstores.[secret store type]
  version: [secret store version]
  metadata:
  - name: [property name]
    value: [property value]
```

<span data-ttu-id="a4800-199">所有 Dapr 组件配置文件都需要 `name` 和可选 `namespace` 值。</span><span class="sxs-lookup"><span data-stu-id="a4800-199">All Dapr component configuration files require a `name` along with an optional `namespace` value.</span></span> <span data-ttu-id="a4800-200">另外， `type` 部分中的字段 `spec` 指定了机密存储区组件的类型。</span><span class="sxs-lookup"><span data-stu-id="a4800-200">Additionally, the `type` field in the `spec` section specifies the type of secret store component.</span></span> <span data-ttu-id="a4800-201">部分中的属性 `metadata` 因机密存储区的不同而异。</span><span class="sxs-lookup"><span data-stu-id="a4800-201">The properties in the `metadata` section differ per secret store.</span></span>

### <a name="indirectly-consume-dapr-secrets"></a><span data-ttu-id="a4800-202">间接使用 Dapr 机密</span><span class="sxs-lookup"><span data-stu-id="a4800-202">Indirectly consume Dapr secrets</span></span>

<span data-ttu-id="a4800-203">如本章前面所述，应用程序还可以通过在组件配置文件中引用机密来使用机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-203">As mentioned earlier in this chapter, applications can also consume secrets by referencing them in component configuration files.</span></span> <span data-ttu-id="a4800-204">请考虑使用 Redis 缓存来存储状态的 [状态管理组件](state-management.md) ：</span><span class="sxs-lookup"><span data-stu-id="a4800-204">Consider a [state management component](state-management.md) that uses Redis cache for storing state:</span></span>

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
    value: localhost:6379
  - name: redisPassword
    value: e$h0p0nD@pr
```

<span data-ttu-id="a4800-205">上述配置文件包含用于连接到 Redis 服务器的 **明文** 密码。</span><span class="sxs-lookup"><span data-stu-id="a4800-205">The above configuration file contains a **clear-text** password for connecting to the Redis server.</span></span> <span data-ttu-id="a4800-206">**硬编码** 密码始终是一个不错的想法。</span><span class="sxs-lookup"><span data-stu-id="a4800-206">**Hardcoded** passwords are always a bad idea.</span></span> <span data-ttu-id="a4800-207">将此配置文件推送到公共存储库将公开密码。</span><span class="sxs-lookup"><span data-stu-id="a4800-207">Pushing this configuration file to a public repository would expose the password.</span></span> <span data-ttu-id="a4800-208">在机密存储区中存储密码会显著改善这种情况。</span><span class="sxs-lookup"><span data-stu-id="a4800-208">Storing the password in a secret store would dramatically improve this scenario.</span></span>

<span data-ttu-id="a4800-209">下面的示例使用几个不同的机密存储区对此进行了演示。</span><span class="sxs-lookup"><span data-stu-id="a4800-209">The following examples demonstrate this using several different secret stores.</span></span>

### <a name="local-file"></a><span data-ttu-id="a4800-210">本地文件</span><span class="sxs-lookup"><span data-stu-id="a4800-210">Local file</span></span>

<span data-ttu-id="a4800-211">本地文件组件设计用于开发方案。</span><span class="sxs-lookup"><span data-stu-id="a4800-211">The local file component is designed for development scenarios.</span></span> <span data-ttu-id="a4800-212">它将机密存储在 JSON 文件中的本地文件系统上。</span><span class="sxs-lookup"><span data-stu-id="a4800-212">It stores secrets on the local filesystem inside a JSON file.</span></span> <span data-ttu-id="a4800-213">下面是一个名为的示例 `eshop-secrets.json` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-213">Here's an example named `eshop-secrets.json`.</span></span> <span data-ttu-id="a4800-214">它包含单个机密-Redis 的密码：</span><span class="sxs-lookup"><span data-stu-id="a4800-214">It contains a single secret - a password for Redis:</span></span>

```json
{
  "eShopRedisPassword": "e$h0p0nD@pr"
}
```

<span data-ttu-id="a4800-215">将此文件放置在 `components` 运行 Dapr 应用程序时指定的文件夹中。</span><span class="sxs-lookup"><span data-stu-id="a4800-215">You place this file in a `components` folder that you specify when running the Dapr application.</span></span>

<span data-ttu-id="a4800-216">以下密钥存储配置文件使用 JSON 文件作为密钥存储。</span><span class="sxs-lookup"><span data-stu-id="a4800-216">The following secret store configuration file consumes the JSON file as a secret store.</span></span> <span data-ttu-id="a4800-217">它还放置在文件夹中 `components` ：</span><span class="sxs-lookup"><span data-stu-id="a4800-217">It's also placed in the `components` folder:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eshop-local-secret-store
  namespace: eshop
spec:
  type: secretstores.local.file
  version: v1
  metadata:
  - name: secretsFile
    value: ./components/eshop-secrets.json
  - name: nestedSeparator
    value: ":"
```

<span data-ttu-id="a4800-218">组件类型为 `secretstore.local.file` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-218">The component type is `secretstore.local.file`.</span></span> <span data-ttu-id="a4800-219">`secretsFile`Metadata 元素指定密钥文件的路径。</span><span class="sxs-lookup"><span data-stu-id="a4800-219">The `secretsFile` metadata element specifies the path to the secrets file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4800-220">机密文件的路径可以是绝对路径或相对路径。</span><span class="sxs-lookup"><span data-stu-id="a4800-220">The path to a secrets file can be a absolute or relative path.</span></span> <span data-ttu-id="a4800-221">相对路径基于应用程序的启动文件夹。</span><span class="sxs-lookup"><span data-stu-id="a4800-221">The relative path is based on the folder in which the application starts.</span></span> <span data-ttu-id="a4800-222">在此示例中， `components` 文件夹是包含 .net 应用程序的目录的子文件夹。</span><span class="sxs-lookup"><span data-stu-id="a4800-222">In the example, the `components` folder is a sub-folder of the directory that contains the .NET application.</span></span>

<span data-ttu-id="a4800-223">从应用程序文件夹中，启动 Dapr 应用程序， `components` 并将路径指定为命令行参数：</span><span class="sxs-lookup"><span data-stu-id="a4800-223">From the application folder, start the Dapr application specifying the `components` path as a command-line argument:</span></span>

```console
dapr run --app-id basket-api --components-path ./components dotnet run
```

> [!NOTE]
> <span data-ttu-id="a4800-224">上面的示例适用于在自承载模式下运行 Dapr。</span><span class="sxs-lookup"><span data-stu-id="a4800-224">This above example applies to running Dapr in self-hosted mode.</span></span> <span data-ttu-id="a4800-225">对于 Kubernetes 托管，请考虑使用卷装入。</span><span class="sxs-lookup"><span data-stu-id="a4800-225">For Kubernetes hosting, consider using volume mounts.</span></span>

<span data-ttu-id="a4800-226">`nestedSeparator`Dapr 配置文件中的指定一个字符来 *平展* JSON 层次结构。</span><span class="sxs-lookup"><span data-stu-id="a4800-226">The `nestedSeparator` in a Dapr configuration file specifies a character to *flatten* a JSON hierarchy.</span></span> <span data-ttu-id="a4800-227">请考虑以下代码片段：</span><span class="sxs-lookup"><span data-stu-id="a4800-227">Consider the following snippet:</span></span>

```json
{
    "redisPassword": "some password",
    "connectionStrings": {
        "customerdb": "some connection string",
        "productdb": "some connection string"
    }
}
```

<span data-ttu-id="a4800-228">使用冒号作为分隔符，可以 `customerdb` 使用键来检索连接字符串 `connectionStrings:customerdb` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-228">Using a colon as a separator, you can retrieve the `customerdb` connection-string using the key `connectionStrings:customerdb`.</span></span>

> [!NOTE]
> <span data-ttu-id="a4800-229">冒号 `:` 为默认分隔符值。</span><span class="sxs-lookup"><span data-stu-id="a4800-229">The colon `:` is the default separator value.</span></span>

<span data-ttu-id="a4800-230">在下一个示例中，状态管理配置文件引用本地机密存储区组件以获取用于连接到 Redis 服务器的密码：</span><span class="sxs-lookup"><span data-stu-id="a4800-230">In the next example, a state management configuration file references the local secret store component to obtain the password for connecting to the Redis server:</span></span>

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
    value: localhost:6379
  - name: redisPassword
    secretKeyRef:
      name: eShopRedisPassword
      key: eShopRedisPassword
auth:
  secretStore: eshop-local-secret-store
```

<span data-ttu-id="a4800-231">`secretKeyRef`元素引用包含密码的机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-231">The `secretKeyRef` element references the secret containing the password.</span></span> <span data-ttu-id="a4800-232">它将替换前面的 *明文* 值。</span><span class="sxs-lookup"><span data-stu-id="a4800-232">It replaces the earlier *clear-text* value.</span></span> <span data-ttu-id="a4800-233">机密名称和密钥名称 `eShopRedisPassword` 引用密钥。</span><span class="sxs-lookup"><span data-stu-id="a4800-233">The secret name and the key name, `eShopRedisPassword`, reference the secret.</span></span> <span data-ttu-id="a4800-234">机密管理组件的名称 `eshop-local-secret-store` 可在 metadata 元素中找到 `auth` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-234">The name of the secret management component `eshop-local-secret-store` is found in the `auth` metadata element.</span></span>

<span data-ttu-id="a4800-235">您可能想知道 `eShopRedisPassword` 密钥引用中名称和密钥的原因是相同的。</span><span class="sxs-lookup"><span data-stu-id="a4800-235">You might wonder why `eShopRedisPassword` is identical for both the name and key in the secret reference.</span></span> <span data-ttu-id="a4800-236">在本地文件密钥存储中，不会使用单独的名称标识机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-236">In the local file secret store, secrets aren't identified with a separate name.</span></span> <span data-ttu-id="a4800-237">在下一个示例中，该方案将不同于使用 Kubernetes 机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-237">The scenario will be different in the next example using Kubernetes secrets.</span></span>

### <a name="kubernetes-secret"></a><span data-ttu-id="a4800-238">Kubernetes 机密</span><span class="sxs-lookup"><span data-stu-id="a4800-238">Kubernetes secret</span></span>

<span data-ttu-id="a4800-239">此第二个示例重点介绍在 Kubernetes 中运行的 Dapr 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a4800-239">This second example focuses on a Dapr application running in Kubernetes.</span></span> <span data-ttu-id="a4800-240">它使用 Kubernetes 提供的标准机密机制。</span><span class="sxs-lookup"><span data-stu-id="a4800-240">It uses the standard secrets mechanism that Kubernetes offers.</span></span> <span data-ttu-id="a4800-241">使用 Kubernetes CLI (`kubectl`) 创建一个 `eshop-redis-secret` 包含密码的名为的机密：</span><span class="sxs-lookup"><span data-stu-id="a4800-241">Use the Kubernetes CLI (`kubectl`) to create a secret named `eshop-redis-secret` that contains the password:</span></span>

```console
kubectl create secret generic eshopsecrets --from-literal=redisPassword=e$h0p0nD@pr -n eshop
```

<span data-ttu-id="a4800-242">创建后，可以在 "状态管理" 的组件配置文件中引用该机密：</span><span class="sxs-lookup"><span data-stu-id="a4800-242">Once created, you can reference the secret in the component configuration file for state management:</span></span>

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
      name: eshopsecrets
      key: redisPassword
auth:
  secretStore: kubernetes
```

<span data-ttu-id="a4800-243">`secretKeyRef`元素分别指定 Kubernetes 机密的名称和机密的密钥、 `eshopsecrets` 和 `redisPassword` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-243">The `secretKeyRef` element specifies the name of the Kubernetes secret and the secret's key, `eshopsecrets`, and `redisPassword` respectively.</span></span> <span data-ttu-id="a4800-244">`auth`Metadata 节指示 Dapr 使用 Kubernetes 机密管理组件。</span><span class="sxs-lookup"><span data-stu-id="a4800-244">The `auth` metadata section instructs Dapr to use the Kubernetes secrets management component.</span></span>

> [!NOTE]
> <span data-ttu-id="a4800-245">身份验证是使用 Kubernetes 机密时的默认值，可以省略。</span><span class="sxs-lookup"><span data-stu-id="a4800-245">Auth is the default value when using Kubernetes secrets and can be omitted.</span></span>

<span data-ttu-id="a4800-246">在生产设置中，通常会将机密作为自动 CI/CD 管道的一部分创建。</span><span class="sxs-lookup"><span data-stu-id="a4800-246">In a production setting, secrets are typically created as part of an automated CI/CD pipeline.</span></span> <span data-ttu-id="a4800-247">这样做可确保只有具有足够权限的人员才能访问和更改机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-247">Doing so ensures only people with sufficient permissions can access and change the secrets.</span></span> <span data-ttu-id="a4800-248">开发人员无需知道机密的实际值即可创建配置文件。</span><span class="sxs-lookup"><span data-stu-id="a4800-248">Developers create configuration files without knowing the actual value of the secrets.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="a4800-249">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="a4800-249">Azure Key Vault</span></span>

<span data-ttu-id="a4800-250">下一个示例适用于实际的生产方案。</span><span class="sxs-lookup"><span data-stu-id="a4800-250">The next example is geared toward a real-world production scenario.</span></span> <span data-ttu-id="a4800-251">它使用 **Azure Key Vault** 作为密钥存储。</span><span class="sxs-lookup"><span data-stu-id="a4800-251">It uses **Azure Key Vault** as the secret store.</span></span> <span data-ttu-id="a4800-252">Azure Key Vault 是一种托管的 Azure 服务，可让机密安全地存储在云中。</span><span class="sxs-lookup"><span data-stu-id="a4800-252">Azure Key Vault is a managed Azure service that enables secrets to be stored securely in the cloud.</span></span>

<span data-ttu-id="a4800-253">要使此示例正常工作，必须满足以下先决条件：</span><span class="sxs-lookup"><span data-stu-id="a4800-253">For this example to work, the following prerequisites must be satisfied:</span></span>

- <span data-ttu-id="a4800-254">已保护对 Azure 订阅的管理访问。</span><span class="sxs-lookup"><span data-stu-id="a4800-254">You've secured administrative access to an Azure subscription.</span></span>
- <span data-ttu-id="a4800-255">你预配了一个名为 `eshopkv` 的 Azure Key Vault，它包含一个名为 `redisPassword` 的机密，其中包含用于连接到 Redis 服务器的密码。</span><span class="sxs-lookup"><span data-stu-id="a4800-255">You've provisioned an Azure Key Vault named `eshopkv` that holds a secret named `redisPassword` that contains the password for connecting to the Redis server.</span></span>
- <span data-ttu-id="a4800-256">已在 Azure Active Directory 中创建 [服务主体](/azure/active-directory/develop/howto-create-service-principal-portal) 。</span><span class="sxs-lookup"><span data-stu-id="a4800-256">You've created [service principal](/azure/active-directory/develop/howto-create-service-principal-portal) in Azure Active Directory.</span></span>
- <span data-ttu-id="a4800-257">已为此服务主体安装了 X509 证书， (包含本地文件系统上的公钥和私钥) 。</span><span class="sxs-lookup"><span data-stu-id="a4800-257">You've installed an X509 certificate for this service principal (containing both the public and private key) on the local filesystem.</span></span>

> [!NOTE]
> <span data-ttu-id="a4800-258">服务主体是应用程序可用于对 Azure 服务进行身份验证的标识。</span><span class="sxs-lookup"><span data-stu-id="a4800-258">A service principal is an identity that can be used by an application to authenticate an Azure service.</span></span> <span data-ttu-id="a4800-259">服务主体使用 X509 证书。</span><span class="sxs-lookup"><span data-stu-id="a4800-259">The service principal uses a X509 certificate.</span></span> <span data-ttu-id="a4800-260">应用程序使用此证书作为凭据对自己进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a4800-260">The application uses this certificate as a credential to authenticate itself.</span></span>

<span data-ttu-id="a4800-261">[Dapr Azure Key Vault 密钥存储文档](https://docs.dapr.io/operations/components/setup-secret-store/supported-secret-stores/azure-keyvault/)提供了创建和配置 Key Vault 环境的分步说明。</span><span class="sxs-lookup"><span data-stu-id="a4800-261">The [Dapr Azure Key Vault secret store documentation](https://docs.dapr.io/operations/components/setup-secret-store/supported-secret-stores/azure-keyvault/) provides step-by-step instructions to create and configure a Key Vault environment.</span></span>

#### <a name="use-key-vault-when-running-in-self-hosted-mode"></a><span data-ttu-id="a4800-262">在自承载模式下运行时使用 Key Vault</span><span class="sxs-lookup"><span data-stu-id="a4800-262">Use Key Vault when running in self-hosted mode</span></span>

<span data-ttu-id="a4800-263">在 Dapr 自承载模式下使用 Azure Key Vault 需要以下配置文件：</span><span class="sxs-lookup"><span data-stu-id="a4800-263">Consuming Azure Key Vault in Dapr self-hosted mode requires the following configuration file:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eshop-azurekv-secret-store
  namespace: eshop
spec:
  type: secretstores.azure.keyvault
  version: v1
  metadata:
  - name: vaultName
    value: eshopkv
  - name: spnTenantId
    value: "619926af-a7c3-4e95-93ed-4ecc4e3e652b"
  - name: spnClientId
    value: "6cf48032-6c38-43be-9d6f-2a43ce736b09"
  - name: spnCertificateFile
    value : "azurekv-spn-cert.pfx"
```

<span data-ttu-id="a4800-264">密钥存储类型为 `secretstores.azure.keyvault` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-264">The secret store type is `secretstores.azure.keyvault`.</span></span> <span data-ttu-id="a4800-265">`metadata`用于配置对 Key Vault 的访问权限的元素需要以下属性：</span><span class="sxs-lookup"><span data-stu-id="a4800-265">The `metadata` element to configure access to Key Vault requires the following properties:</span></span>

- <span data-ttu-id="a4800-266">`vaultName`包含 Azure Key Vault 的名称。</span><span class="sxs-lookup"><span data-stu-id="a4800-266">The `vaultName` contains the name of the Azure Key Vault.</span></span>
- <span data-ttu-id="a4800-267">`spnTenantId`包含用于对 Key Vault 进行身份验证的服务主体的 *租户 ID* 。</span><span class="sxs-lookup"><span data-stu-id="a4800-267">The `spnTenantId` contains the *tenant ID* of the service principal used to authenticate against the Key Vault.</span></span>
- <span data-ttu-id="a4800-268">`spnClientId`包含用于对 Key Vault 进行身份验证的服务主体的 *应用程序 ID* 。</span><span class="sxs-lookup"><span data-stu-id="a4800-268">The `spnClientId` contains the *app ID* of the service principal used to authenticate against the Key Vault.</span></span>
- <span data-ttu-id="a4800-269">`spnCertificateFile`包含服务主体的证书文件路径，以对 Key Vault 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a4800-269">The `spnCertificateFile` contains the path to the certificate file for the service principal to authenticate against the Key Vault.</span></span>

> [!TIP]
> <span data-ttu-id="a4800-270">可以从 Azure 门户或 Azure CLI 复制服务主体信息。</span><span class="sxs-lookup"><span data-stu-id="a4800-270">You can copy the service principal information from the Azure portal or Azure CLI .</span></span>

<span data-ttu-id="a4800-271">现在，应用程序可以从 Azure Key Vault 中检索 Redis 密码。</span><span class="sxs-lookup"><span data-stu-id="a4800-271">Now the application can retrieve the Redis password from the Azure Key Vault.</span></span>

#### <a name="use-key-vault-when-running-on-kubernetes"></a><span data-ttu-id="a4800-272">在 Kubernetes 上运行时使用 Key Vault</span><span class="sxs-lookup"><span data-stu-id="a4800-272">Use Key Vault when running on Kubernetes</span></span>

<span data-ttu-id="a4800-273">使用 Dapr 和 Kubernetes 的 Azure Key Vault 也需要服务主体针对 Azure Key Vault 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a4800-273">Consuming Azure Key Vault with Dapr and Kubernetes also requires a service principal to authenticate against the Azure Key Vault.</span></span>

<span data-ttu-id="a4800-274">首先，使用 kubectl CLI 工具创建包含证书文件的 *Kubernetes 机密* ：</span><span class="sxs-lookup"><span data-stu-id="a4800-274">First, create a *Kubernetes secret* that contains a certificate file using the kubectl CLI tool:</span></span>

```console
kubectl create secret generic [k8s_spn_secret_name] --from-file=[pfx_certificate_file_local_path] -n eshop
```

<span data-ttu-id="a4800-275">此命令需要两个命令行参数：</span><span class="sxs-lookup"><span data-stu-id="a4800-275">The command requires two command-line arguments:</span></span>

- <span data-ttu-id="a4800-276">`[k8s_spn_secret_name]` Kubernetes 密钥存储中的机密名称。</span><span class="sxs-lookup"><span data-stu-id="a4800-276">`[k8s_spn_secret_name]` is the secret name in Kubernetes secret store.</span></span>
- <span data-ttu-id="a4800-277">`[pfx_certificate_file_local_path]` 是 X509 证书文件的路径。</span><span class="sxs-lookup"><span data-stu-id="a4800-277">`[pfx_certificate_file_local_path]` is the path of X509 certificate file.</span></span>

<span data-ttu-id="a4800-278">创建后，可以在密钥存储组件配置文件中引用 Kubernetes 机密：</span><span class="sxs-lookup"><span data-stu-id="a4800-278">Once created, you can reference the Kubernetes secret in the secret store component configuration file:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eshop-azurekv-secret-store
  namespace: eshop
spec:
  type: secretstores.azure.keyvault
  version: v1
  metadata:
  - name: vaultName
    value: [your_keyvault_name]
  - name: spnTenantId
    value: "619926af-a7c3-4e95-93ed-4ecc4e3e652b"
  - name: spnClientId
    value: "6cf48032-6c38-43be-9d6f-2a43ce736b09"
  - name: spnCertificate
    secretKeyRef:
      name: [k8s_spn_secret_name]
      key: [pfx_certificate_file_local_name]
auth:
    secretStore: kubernetes
```

<span data-ttu-id="a4800-279">此时，在 Kubernetes 中运行的应用程序可以从 Azure Key Vault 检索 Redis 密码。</span><span class="sxs-lookup"><span data-stu-id="a4800-279">At this point, an application running in Kubernetes can retrieve the Redis password from the Azure Key Vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a4800-280">将服务主体的 X509 证书文件保存在安全的位置至关重要。</span><span class="sxs-lookup"><span data-stu-id="a4800-280">It's critical to keep the X509 certificate file for the service principal in a safe place.</span></span> <span data-ttu-id="a4800-281">最好将它放在源代码存储库外的已知文件夹中。</span><span class="sxs-lookup"><span data-stu-id="a4800-281">It's best to place it in a well-known folder outside the source-code repository.</span></span> <span data-ttu-id="a4800-282">然后，配置文件可以引用此众所周知的文件夹中的证书文件。</span><span class="sxs-lookup"><span data-stu-id="a4800-282">The configuration file can then reference the certificate file from this well-known folder.</span></span> <span data-ttu-id="a4800-283">在本地开发计算机上，您负责将证书复制到文件夹。</span><span class="sxs-lookup"><span data-stu-id="a4800-283">On a local development machine, you're responsible for copying the certificate to the folder.</span></span> <span data-ttu-id="a4800-284">对于自动部署，管道会将证书复制到部署了该应用程序的计算机。</span><span class="sxs-lookup"><span data-stu-id="a4800-284">For automated deployments, the pipeline will copy the certificate to the machine where the application is deployed.</span></span> <span data-ttu-id="a4800-285">最佳做法是在每个环境中使用不同的服务主体。</span><span class="sxs-lookup"><span data-stu-id="a4800-285">It's a best practice to use a different service principal per environment.</span></span> <span data-ttu-id="a4800-286">这样做可以防止开发环境中的服务主体访问生产环境中的机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-286">Doing so prevents the service principal from a DEVELOPMENT environment to access secrets in a PRODUCTION environment.</span></span>

<span data-ttu-id="a4800-287">在 Azure Kubernetes Service (AKS) 中运行时，最好使用 [Azure 托管标识](/azure/active-directory/managed-identities-azure-resources/overview) 对 Azure Key Vault 进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="a4800-287">When running in  Azure Kubernetes Service (AKS), it's preferable to use an [Azure Managed Identity](/azure/active-directory/managed-identities-azure-resources/overview) for authenticating against Azure Key Vault.</span></span> <span data-ttu-id="a4800-288">托管标识超出了本书的范围，但在 [与托管标识](https://docs.dapr.io/operations/components/setup-secret-store/supported-secret-stores/azure-keyvault-managed-identity/) 相关的 Azure Key Vault 文档中进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="a4800-288">Managed identities are outside of the scope of this book, but explained in the [Azure Key Vault with managed identities](https://docs.dapr.io/operations/components/setup-secret-store/supported-secret-stores/azure-keyvault-managed-identity/) documentation.</span></span>

### <a name="scope-secrets"></a><span data-ttu-id="a4800-289">范围机密</span><span class="sxs-lookup"><span data-stu-id="a4800-289">Scope secrets</span></span>

<span data-ttu-id="a4800-290">机密作用域允许您控制您的应用程序可以访问的机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-290">Secret scopes allow you to control which secrets your application can access.</span></span> <span data-ttu-id="a4800-291">在 Dapr 挎斗配置文件中配置作用域。</span><span class="sxs-lookup"><span data-stu-id="a4800-291">You configure scopes in a Dapr sidecar configuration file.</span></span> <span data-ttu-id="a4800-292">[Dapr 配置文档](https://docs.dapr.io/operations/configuration/configuration-overview/)提供了有关如何对机密进行作用域的说明。</span><span class="sxs-lookup"><span data-stu-id="a4800-292">The [Dapr configuration documentation](https://docs.dapr.io/operations/configuration/configuration-overview/) provides instructions for scoping secrets.</span></span>

<span data-ttu-id="a4800-293">下面是包含机密作用域的 Dapr 挎斗配置文件的示例：</span><span class="sxs-lookup"><span data-stu-id="a4800-293">Here's an example of a Dapr sidecar configuration file that contains secret scopes:</span></span>

```yml
apiVersion: dapr.io/v1alpha1
kind: Configuration
metadata:
  name: dapr-config
  namespace: eshop
spec:
  tracing:
    samplingRate: "1"
  secrets:
    scopes:
      - storeName: eshop-azurekv-secret-store
        defaultAccess: allow
        deniedSecrets: ["redisPassword", "apiKey"]
```

<span data-ttu-id="a4800-294">为每个密钥存储指定作用域。</span><span class="sxs-lookup"><span data-stu-id="a4800-294">You specify scopes per secret store.</span></span> <span data-ttu-id="a4800-295">在上面的示例中，机密存储区命名为 `eshop-azurekv-secret-store` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-295">In the above example, the secret store is named `eshop-azurekv-secret-store`.</span></span> <span data-ttu-id="a4800-296">使用以下属性配置对机密的访问：</span><span class="sxs-lookup"><span data-stu-id="a4800-296">You configure access to secrets using the following properties:</span></span>

| <span data-ttu-id="a4800-297">属性</span><span class="sxs-lookup"><span data-stu-id="a4800-297">Property</span></span>         | <span data-ttu-id="a4800-298">值</span><span class="sxs-lookup"><span data-stu-id="a4800-298">Value</span></span>               | <span data-ttu-id="a4800-299">说明</span><span class="sxs-lookup"><span data-stu-id="a4800-299">Description</span></span>                                                                                                                       |
|------------------|---------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| `defaultAccess`  | <span data-ttu-id="a4800-300">`allow` 或 `deny`</span><span class="sxs-lookup"><span data-stu-id="a4800-300">`allow` or `deny`</span></span>   | <span data-ttu-id="a4800-301">允许或拒绝对指定密钥存储中 *所有* 机密的访问。</span><span class="sxs-lookup"><span data-stu-id="a4800-301">Allows or denies access to *all* secrets in the specified secret store.</span></span> <span data-ttu-id="a4800-302">此属性是可选的，默认值为 `allow` 。</span><span class="sxs-lookup"><span data-stu-id="a4800-302">This property is optional with a default value of `allow`.</span></span> |
| `allowedSecrets` | <span data-ttu-id="a4800-303">机密密钥列表</span><span class="sxs-lookup"><span data-stu-id="a4800-303">List of secret keys</span></span> | <span data-ttu-id="a4800-304">数组中指定的机密将可供访问。</span><span class="sxs-lookup"><span data-stu-id="a4800-304">Secrets specified in the array will be accessible.</span></span> <span data-ttu-id="a4800-305">此属性是可选的。</span><span class="sxs-lookup"><span data-stu-id="a4800-305">This property is optional.</span></span>                                                     |
| `deniedSecrets`  | <span data-ttu-id="a4800-306">机密密钥列表</span><span class="sxs-lookup"><span data-stu-id="a4800-306">List of secret keys</span></span> | <span data-ttu-id="a4800-307">数组中指定的机密将不可访问。</span><span class="sxs-lookup"><span data-stu-id="a4800-307">Secrets specified in the array will NOT be accessible.</span></span> <span data-ttu-id="a4800-308">此属性是可选的。</span><span class="sxs-lookup"><span data-stu-id="a4800-308">This property is optional.</span></span>                                                 |

<span data-ttu-id="a4800-309">`allowedSecrets`和 `deniedSecrets` 属性优先于 `defaultAccess` 属性。</span><span class="sxs-lookup"><span data-stu-id="a4800-309">The `allowedSecrets` and `deniedSecrets` properties take precedence over the `defaultAccess` property.</span></span> <span data-ttu-id="a4800-310">假设指定 `defaultAccess: allowed` 和 `allowedSecrets` 列表。</span><span class="sxs-lookup"><span data-stu-id="a4800-310">Imagine specifying `defaultAccess: allowed` and an `allowedSecrets` list.</span></span> <span data-ttu-id="a4800-311">在这种情况下， `allowedSecrets` 应用程序只能访问列表中的机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-311">In this case, only the secrets in the `allowedSecrets` list would be accessible by the application.</span></span>

## <a name="reference-application-eshopondapr"></a><span data-ttu-id="a4800-312">引用应用程序： eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="a4800-312">Reference application: eShopOnDapr</span></span>

<span data-ttu-id="a4800-313">EShopOnDapr 参考应用程序对两个机密使用机密构建基块：</span><span class="sxs-lookup"><span data-stu-id="a4800-313">The eShopOnDapr reference application uses the secrets building block for two secrets:</span></span>

- <span data-ttu-id="a4800-314">用于连接到 Redis 缓存的密码。</span><span class="sxs-lookup"><span data-stu-id="a4800-314">The password for connecting to the Redis cache.</span></span>
- <span data-ttu-id="a4800-315">用于使用 Twilio Sendgrid API 的 API 密钥。</span><span class="sxs-lookup"><span data-stu-id="a4800-315">The API-key for using the Twilio Sendgrid API.</span></span> <span data-ttu-id="a4800-316">应用程序使用 Twillio 通过 Dapr 输出绑定 (发送电子邮件，如 [绑定构建块章节](bindings.md)) 中所述。</span><span class="sxs-lookup"><span data-stu-id="a4800-316">The application uses Twillio to send emails using a Dapr output binding (as described in the [bindings building block chapter](bindings.md)).</span></span>

<span data-ttu-id="a4800-317">使用 Docker Compose 运行应用程序时，将使用 **本地文件** 密钥存储。</span><span class="sxs-lookup"><span data-stu-id="a4800-317">When running the application using Docker Compose, the **local file** secret store is used.</span></span> <span data-ttu-id="a4800-318">组件配置文件位于 `eshop-secretstore.yaml` `dapr/components` eShopOnDapr 存储库的文件夹中：</span><span class="sxs-lookup"><span data-stu-id="a4800-318">The component configuration file `eshop-secretstore.yaml` is found in the `dapr/components` folder of the eShopOnDapr repository:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: eshop-secretstore
  namespace: eshop
spec:
  type: secretstores.local.file
  version: v1
  metadata:
  - name: secretsFile
    value: ./components/eshop-secretstore.json
```

<span data-ttu-id="a4800-319">配置文件引用 `eshop-secretstore.json` 位于同一文件夹中的本地存储文件：</span><span class="sxs-lookup"><span data-stu-id="a4800-319">The configuration file references the local store file `eshop-secretstore.json` located in the same folder:</span></span>

```json
{
    "redisPassword": "**********",
    "sendgridAPIKey": "**********"
}
```

<span data-ttu-id="a4800-320">在 `components` 命令行中指定文件夹，并将其作为本地文件夹装载到 Dapr 挎斗容器中。</span><span class="sxs-lookup"><span data-stu-id="a4800-320">The `components` folder is specified in the command-line and mounted as a local folder inside the Dapr sidecar container.</span></span> <span data-ttu-id="a4800-321">下面是 `docker-compose.override.yml` 存储库根目录中的文件中用于指定卷装入的代码段：</span><span class="sxs-lookup"><span data-stu-id="a4800-321">Here's a snippet from the `docker-compose.override.yml` file in the repository root that specifies the volume mount:</span></span>

```yaml
  ordering-backgroundtasks-dapr:
    command: ["./daprd",
      "-app-id", "ordering-backgroundtasks",
      "-app-port", "80",
      "-dapr-grpc-port", "50004",
      "-components-path", "/components",
      "-config", "/configuration/eshop-config.yaml"
      ]
    volumes:
      - "./dapr/components/:/components"
      - "./dapr/configuration/:/configuration"
```

> [!NOTE]
> <span data-ttu-id="a4800-322">Docker Compose 重写文件包含特定于环境的配置值。</span><span class="sxs-lookup"><span data-stu-id="a4800-322">The Docker Compose override file contains environmental specific configuration values.</span></span>

<span data-ttu-id="a4800-323">`/components`卷装入和 `--components-path` 命令行参数会传递到 `daprd` 启动命令。</span><span class="sxs-lookup"><span data-stu-id="a4800-323">The `/components` volume mount and `--components-path` command-line argument are passed into the `daprd` startup command.</span></span>

<span data-ttu-id="a4800-324">配置后，其他组件配置文件也可以引用该机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-324">Once configured, other component configuration files can also reference the secrets.</span></span> <span data-ttu-id="a4800-325">下面是发布/订阅组件配置使用机密的示例：</span><span class="sxs-lookup"><span data-stu-id="a4800-325">Here's an example of the Publish/Subscribe component configuration consuming secrets:</span></span>

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: pubsub
  namespace: eshop
spec:
  type: pubsub.redis
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

<span data-ttu-id="a4800-326">在前面的示例中，本地 Redis 存储用于引用机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-326">In the preceding example, the local Redis store is used to reference secrets.</span></span>

## <a name="summary"></a><span data-ttu-id="a4800-327">摘要</span><span class="sxs-lookup"><span data-stu-id="a4800-327">Summary</span></span>

<span data-ttu-id="a4800-328">Dapr 机密构建块提供了存储和检索敏感配置设置（例如密码和连接字符串）的功能。</span><span class="sxs-lookup"><span data-stu-id="a4800-328">The Dapr secrets building block provides capabilities for storing and retrieving sensitive configuration settings like passwords and connection-strings.</span></span> <span data-ttu-id="a4800-329">它将机密保密，防止无意中泄露机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-329">It keeps secrets private and prevents them from being accidentally disclosed.</span></span>

<span data-ttu-id="a4800-330">构建基块支持多个不同的机密存储，并通过 Dapr 机密 API 隐藏其复杂性。</span><span class="sxs-lookup"><span data-stu-id="a4800-330">The building block supports several different secret stores and hides their complexity with the Dapr secrets API.</span></span>

<span data-ttu-id="a4800-331">Dapr .NET SDK 提供了一个 `DaprClient` 用于检索机密的对象。</span><span class="sxs-lookup"><span data-stu-id="a4800-331">The Dapr .NET SDK provides a `DaprClient` object to retrieve secrets.</span></span> <span data-ttu-id="a4800-332">它还包括将机密添加到 .NET Core 配置系统的 .NET 配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a4800-332">It also includes a .NET configuration provider that adds secrets to the .NET Core configuration system.</span></span> <span data-ttu-id="a4800-333">加载后，可以在 .NET 代码中使用这些机密。</span><span class="sxs-lookup"><span data-stu-id="a4800-333">Once loaded, you can consume these secrets in your .NET code.</span></span>

<span data-ttu-id="a4800-334">可以使用机密范围来控制对特定机密的访问。</span><span class="sxs-lookup"><span data-stu-id="a4800-334">You can use secret scopes to control access to specific secrets.</span></span>

## <a name="references"></a><span data-ttu-id="a4800-335">参考</span><span class="sxs-lookup"><span data-stu-id="a4800-335">References</span></span>

- [<span data-ttu-id="a4800-336">超出 Twelve-Factor 应用程序</span><span class="sxs-lookup"><span data-stu-id="a4800-336">Beyond the Twelve-Factor Application</span></span>](https://tanzu.vmware.com/content/blog/beyond-the-twelve-factor-app)

>[!div class="step-by-step"]
><span data-ttu-id="a4800-337">[上一页](observability.md)
>[下一页](summary.md)</span><span class="sxs-lookup"><span data-stu-id="a4800-337">[Previous](observability.md)
[Next](summary.md)</span></span>
