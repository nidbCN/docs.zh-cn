---
title: .NET 通用主机
author: IEvangelist
description: 了解 .NET 通用主机，该主机负责应用启动和生存期管理。
ms.author: dapine
ms.date: 12/18/2020
ms.openlocfilehash: bf6d5ad624bbed46994633abace0af64712757f3
ms.sourcegitcommit: 3d6d6595a03915f617349781f455f838a44b0f44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2020
ms.locfileid: "102401954"
---
# <a name="net-generic-host"></a><span data-ttu-id="3e11c-103">.NET 通用主机</span><span class="sxs-lookup"><span data-stu-id="3e11c-103">.NET Generic Host</span></span>

<span data-ttu-id="3e11c-104">辅助角色服务模板会创建一个 .NET 通用主机 <xref:Microsoft.Extensions.Hosting.HostBuilder>。</span><span class="sxs-lookup"><span data-stu-id="3e11c-104">The Worker Service templates create a .NET Generic Host, <xref:Microsoft.Extensions.Hosting.HostBuilder>.</span></span> <span data-ttu-id="3e11c-105">通用主机可用于其他类型的 .NET 应用程序，如控制台应用。</span><span class="sxs-lookup"><span data-stu-id="3e11c-105">The Generic Host can be used with other types of .NET applications, such as Console apps.</span></span>

<span data-ttu-id="3e11c-106">主机是封装应用资源的对象，例如：</span><span class="sxs-lookup"><span data-stu-id="3e11c-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

- <span data-ttu-id="3e11c-107">依赖关系注入 (DI)</span><span class="sxs-lookup"><span data-stu-id="3e11c-107">Dependency injection (DI)</span></span>
- <span data-ttu-id="3e11c-108">Logging</span><span class="sxs-lookup"><span data-stu-id="3e11c-108">Logging</span></span>
- <span data-ttu-id="3e11c-109">Configuration</span><span class="sxs-lookup"><span data-stu-id="3e11c-109">Configuration</span></span>
- <span data-ttu-id="3e11c-110">`IHostedService` 实现</span><span class="sxs-lookup"><span data-stu-id="3e11c-110">`IHostedService` implementations</span></span>

<span data-ttu-id="3e11c-111">当主机启动时，它将对在托管服务的服务容器集合中注册的 <xref:Microsoft.Extensions.Hosting.IHostedService> 的每个实现调用 <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="3e11c-111">When a host starts, it calls <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync%2A?displayProperty=nameWithType> on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> registered in the service container's collection of hosted services.</span></span> <span data-ttu-id="3e11c-112">在辅助角色服务应用中，包含 <xref:Microsoft.Extensions.Hosting.BackgroundService> 实例的所有 `IHostedService` 实现都调用其 <xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="3e11c-112">In a worker service app, all `IHostedService` implementations that contain <xref:Microsoft.Extensions.Hosting.BackgroundService> instances have their <xref:Microsoft.Extensions.Hosting.BackgroundService.ExecuteAsync%2A?displayProperty=nameWithType> methods called.</span></span>

<span data-ttu-id="3e11c-113">一个对象中包含所有应用的相互依赖资源的主要原因是生存期管理：控制应用启动和正常关闭。</span><span class="sxs-lookup"><span data-stu-id="3e11c-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span> <span data-ttu-id="3e11c-114">这是通过 [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) NuGet 包实现的。</span><span class="sxs-lookup"><span data-stu-id="3e11c-114">This is achieved with the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) NuGet package.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="3e11c-115">设置主机</span><span class="sxs-lookup"><span data-stu-id="3e11c-115">Set up a host</span></span>

<span data-ttu-id="3e11c-116">主机通常由 `Program` 类中的代码配置、生成和运行。</span><span class="sxs-lookup"><span data-stu-id="3e11c-116">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="3e11c-117">`Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="3e11c-117">The `Main` method:</span></span>

- <span data-ttu-id="3e11c-118">调用 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder> 方法以创建和配置生成器对象。</span><span class="sxs-lookup"><span data-stu-id="3e11c-118">Calls a <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder> method to create and configure a builder object.</span></span>
- <span data-ttu-id="3e11c-119">调用 <xref:Microsoft.Extensions.Hosting.IHostBuilder.Build> 以创建 <xref:Microsoft.Extensions.Hosting.IHost> 实例。</span><span class="sxs-lookup"><span data-stu-id="3e11c-119">Calls <xref:Microsoft.Extensions.Hosting.IHostBuilder.Build> to create an <xref:Microsoft.Extensions.Hosting.IHost> instance.</span></span>
- <span data-ttu-id="3e11c-120">对主机对象调用 <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run%2A> 或 <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="3e11c-120">Calls <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run%2A> or <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync%2A> method on the host object.</span></span>

<span data-ttu-id="3e11c-121">.NET 辅助角色服务模板会生成以下代码来创建通用主机：</span><span class="sxs-lookup"><span data-stu-id="3e11c-121">The .NET Worker Service templates generate the following code to create a Generic Host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
                services.AddHostedService<Worker>();
            });
}
```

## <a name="default-builder-settings"></a><span data-ttu-id="3e11c-122">默认生成器设置</span><span class="sxs-lookup"><span data-stu-id="3e11c-122">Default builder settings</span></span>

<span data-ttu-id="3e11c-123"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> 方法：</span><span class="sxs-lookup"><span data-stu-id="3e11c-123">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A> method:</span></span>

- <span data-ttu-id="3e11c-124">将内容根路径设置为由 <xref:System.IO.Directory.GetCurrentDirectory> 返回的路径。</span><span class="sxs-lookup"><span data-stu-id="3e11c-124">Sets the content root to the path returned by <xref:System.IO.Directory.GetCurrentDirectory>.</span></span>
- <span data-ttu-id="3e11c-125">通过以下对象加载[主机配置](#host-configuration)：</span><span class="sxs-lookup"><span data-stu-id="3e11c-125">Loads [host configuration](#host-configuration) from:</span></span>
  - <span data-ttu-id="3e11c-126">前缀为 `DOTNET_` 的环境变量。</span><span class="sxs-lookup"><span data-stu-id="3e11c-126">Environment variables prefixed with `DOTNET_`.</span></span>
  - <span data-ttu-id="3e11c-127">命令行参数。</span><span class="sxs-lookup"><span data-stu-id="3e11c-127">Command-line arguments.</span></span>
- <span data-ttu-id="3e11c-128">通过以下对象加载应用配置：</span><span class="sxs-lookup"><span data-stu-id="3e11c-128">Loads app configuration from:</span></span>
  - <span data-ttu-id="3e11c-129">appsettings.json。</span><span class="sxs-lookup"><span data-stu-id="3e11c-129">*appsettings.json*.</span></span>
  - <span data-ttu-id="3e11c-130">appsettings.{Environment}.json。</span><span class="sxs-lookup"><span data-stu-id="3e11c-130">*appsettings.{Environment}.json*.</span></span>
  - <span data-ttu-id="3e11c-131">密钥管理器 当应用在 `Development` 环境中运行时。</span><span class="sxs-lookup"><span data-stu-id="3e11c-131">Secret Manager when the app runs in the `Development` environment.</span></span>
  - <span data-ttu-id="3e11c-132">环境变量。</span><span class="sxs-lookup"><span data-stu-id="3e11c-132">Environment variables.</span></span>
  - <span data-ttu-id="3e11c-133">命令行参数。</span><span class="sxs-lookup"><span data-stu-id="3e11c-133">Command-line arguments.</span></span>
- <span data-ttu-id="3e11c-134">添加以下日志记录提供程序：</span><span class="sxs-lookup"><span data-stu-id="3e11c-134">Adds the following logging providers:</span></span>
  - <span data-ttu-id="3e11c-135">控制台</span><span class="sxs-lookup"><span data-stu-id="3e11c-135">Console</span></span>
  - <span data-ttu-id="3e11c-136">调试</span><span class="sxs-lookup"><span data-stu-id="3e11c-136">Debug</span></span>
  - <span data-ttu-id="3e11c-137">EventSource</span><span class="sxs-lookup"><span data-stu-id="3e11c-137">EventSource</span></span>
  - <span data-ttu-id="3e11c-138">EventLog（仅当在 Windows 上运行时）</span><span class="sxs-lookup"><span data-stu-id="3e11c-138">EventLog (only when running on Windows)</span></span>
- <span data-ttu-id="3e11c-139">当环境为 `Development` 时，启用范围验证和[依赖关系验证](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild)。</span><span class="sxs-lookup"><span data-stu-id="3e11c-139">Enables scope validation and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is `Development`.</span></span>

<span data-ttu-id="3e11c-140">`ConfigureServices` 方法公开了向 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection?displayProperty=nameWithType> 实例添加服务的功能。</span><span class="sxs-lookup"><span data-stu-id="3e11c-140">The `ConfigureServices` method exposes the ability to add services to the <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection?displayProperty=nameWithType> instance.</span></span> <span data-ttu-id="3e11c-141">以后，可以通过依赖关系注入获取这些服务。</span><span class="sxs-lookup"><span data-stu-id="3e11c-141">Later, these services can be made available from dependency injection.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="3e11c-142">框架提供的服务</span><span class="sxs-lookup"><span data-stu-id="3e11c-142">Framework-provided services</span></span>

<span data-ttu-id="3e11c-143">自动注册以下服务：</span><span class="sxs-lookup"><span data-stu-id="3e11c-143">The following services are registered automatically:</span></span>

- [<span data-ttu-id="3e11c-144">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="3e11c-144">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
- [<span data-ttu-id="3e11c-145">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="3e11c-145">IHostLifetime</span></span>](#ihostlifetime)
- [<span data-ttu-id="3e11c-146">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="3e11c-146">IHostEnvironment</span></span>](#ihostenvironment)

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="3e11c-147">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="3e11c-147">IHostApplicationLifetime</span></span>

<span data-ttu-id="3e11c-148">将 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> 服务注入任何类以处理启动后和正常关闭任务。</span><span class="sxs-lookup"><span data-stu-id="3e11c-148">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="3e11c-149">接口上的三个属性是用于注册应用启动和应用停止事件处理程序方法的取消令牌。</span><span class="sxs-lookup"><span data-stu-id="3e11c-149">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="3e11c-150">该接口还包括 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication> 方法。</span><span class="sxs-lookup"><span data-stu-id="3e11c-150">The interface also includes a <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication> method.</span></span>

<span data-ttu-id="3e11c-151">以下示例是注册 `IHostApplicationLifetime` 事件的 `IHostedService` 实现：</span><span class="sxs-lookup"><span data-stu-id="3e11c-151">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

:::code language="csharp" source="snippets/configuration/app-lifetime/ExampleHostedService.cs" highlight="18-20":::

<span data-ttu-id="3e11c-152">可以修改辅助角色服务模板以添加 `ExampleHostedService` 实现：</span><span class="sxs-lookup"><span data-stu-id="3e11c-152">The Worker Service template could be modified to add the `ExampleHostedService` implementation:</span></span>

:::code language="csharp" source="snippets/configuration/app-lifetime/Program.cs" range="1-16,36" highlight="15":::

<span data-ttu-id="3e11c-153">应用程序会编写以下示例输出：</span><span class="sxs-lookup"><span data-stu-id="3e11c-153">The application would write the following sample output:</span></span>

:::code language="csharp" source="snippets/configuration/app-lifetime/Program.cs" range="17-35":::

## <a name="ihostlifetime"></a><span data-ttu-id="3e11c-154">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="3e11c-154">IHostLifetime</span></span>

<span data-ttu-id="3e11c-155"><xref:Microsoft.Extensions.Hosting.IHostLifetime> 实现控制主机何时启动和何时停止。</span><span class="sxs-lookup"><span data-stu-id="3e11c-155">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="3e11c-156">使用了已注册的最后一个实现。</span><span class="sxs-lookup"><span data-stu-id="3e11c-156">The last implementation registered is used.</span></span>

<span data-ttu-id="3e11c-157">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` 是默认的 `IHostLifetime` 实现。</span><span class="sxs-lookup"><span data-stu-id="3e11c-157">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="3e11c-158">`ConsoleLifetime`:</span><span class="sxs-lookup"><span data-stu-id="3e11c-158">`ConsoleLifetime`:</span></span>

- <span data-ttu-id="3e11c-159">侦听 <kbd>Ctrl</kbd>+<kbd>C</kbd>、[SIGINT](https://en.wikipedia.org/wiki/Signal_(IPC)#SIGINT) 或 [SIGTERM](https://en.wikipedia.org/wiki/Signal_(IPC)#SIGTERM) 并调用 <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication%2A> 来启动关闭进程。</span><span class="sxs-lookup"><span data-stu-id="3e11c-159">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>, [SIGINT](https://en.wikipedia.org/wiki/Signal_(IPC)#SIGINT), or [SIGTERM](https://en.wikipedia.org/wiki/Signal_(IPC)#SIGTERM) and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication%2A> to start the shutdown process.</span></span>
- <span data-ttu-id="3e11c-160">解除阻止 `RunAsync` 和 `WaitForShutdownAsync` 等扩展。</span><span class="sxs-lookup"><span data-stu-id="3e11c-160">Unblocks extensions such as `RunAsync` and `WaitForShutdownAsync`.</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="3e11c-161">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="3e11c-161">IHostEnvironment</span></span>

<span data-ttu-id="3e11c-162">将 <xref:Microsoft.Extensions.Hosting.IHostEnvironment> 服务注册到一个类，获取关于以下设置的信息：</span><span class="sxs-lookup"><span data-stu-id="3e11c-162">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

- <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootFileProvider?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName?displayProperty=nameWithType>

## <a name="host-configuration"></a><span data-ttu-id="3e11c-163">主机配置</span><span class="sxs-lookup"><span data-stu-id="3e11c-163">Host configuration</span></span>

<span data-ttu-id="3e11c-164">主机配置用于配置 [IHostEnvironment](#ihostenvironment) 实现的属性。</span><span class="sxs-lookup"><span data-stu-id="3e11c-164">Host configuration is used to configure properties of the [IHostEnvironment](#ihostenvironment) implementation.</span></span>

<span data-ttu-id="3e11c-165">主机配置在 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration%2A> 方法的 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) 中可用。</span><span class="sxs-lookup"><span data-stu-id="3e11c-165">The host configuration is available in [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) within the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration%2A> method.</span></span> <span data-ttu-id="3e11c-166">调用 `ConfigureAppConfiguration` 方法时，`HostBuilderContext` 和 `IConfigurationBuilder` 将传递到 `configureDelegate` 中。</span><span class="sxs-lookup"><span data-stu-id="3e11c-166">When calling the `ConfigureAppConfiguration` method, the `HostBuilderContext` and `IConfigurationBuilder` are passed into the `configureDelegate`.</span></span> <span data-ttu-id="3e11c-167">`configureDelegate` 被定义为 `Action<HostBuilderContext, IConfigurationBuilder>`。</span><span class="sxs-lookup"><span data-stu-id="3e11c-167">The `configureDelegate` is defined as an `Action<HostBuilderContext, IConfigurationBuilder>`.</span></span> <span data-ttu-id="3e11c-168">主机生成器上下文公开 `.Configuration` 属性，该属性是 `IConfiguration` 的一个实例。</span><span class="sxs-lookup"><span data-stu-id="3e11c-168">The host builder context exposes the `.Configuration` property, which is an instance of `IConfiguration`.</span></span> <span data-ttu-id="3e11c-169">它表示从主机生成的配置，而 `IConfigurationBuilder` 是用于配置应用的生成器对象。</span><span class="sxs-lookup"><span data-stu-id="3e11c-169">It represents the configuration built from the host, whereas the `IConfigurationBuilder` is the builder object used to configure the app.</span></span>

> [!TIP]
> <span data-ttu-id="3e11c-170">在调用 `ConfigureAppConfiguration` 后，`HostBuilderContext.Configuration` 被替换为[应用配置](#app-configuration)。</span><span class="sxs-lookup"><span data-stu-id="3e11c-170">After `ConfigureAppConfiguration` is called the `HostBuilderContext.Configuration` is replaced with the [app config](#app-configuration).</span></span>

<span data-ttu-id="3e11c-171">若要添加主机配置，请对 `IHostBuilder` 调用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration%2A>。</span><span class="sxs-lookup"><span data-stu-id="3e11c-171">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration%2A> on `IHostBuilder`.</span></span> <span data-ttu-id="3e11c-172">可多次调用 `ConfigureHostConfiguration`，并得到累计结果。</span><span class="sxs-lookup"><span data-stu-id="3e11c-172">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="3e11c-173">主机使用上一次在一个给定键上设置值的选项。</span><span class="sxs-lookup"><span data-stu-id="3e11c-173">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="3e11c-174">以下示例创建主机配置：</span><span class="sxs-lookup"><span data-stu-id="3e11c-174">The following example creates host configuration:</span></span>

:::code language="csharp" source="snippets/configuration/console-host/Program.cs" highlight="19-25":::

## <a name="app-configuration"></a><span data-ttu-id="3e11c-175">应用配置</span><span class="sxs-lookup"><span data-stu-id="3e11c-175">App configuration</span></span>

<span data-ttu-id="3e11c-176">通过对 `IHostBuilder` 调用 <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration%2A> 创建应用配置。</span><span class="sxs-lookup"><span data-stu-id="3e11c-176">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration%2A> on `IHostBuilder`.</span></span> <span data-ttu-id="3e11c-177">可多次调用 `ConfigureAppConfiguration`，并得到累计结果。</span><span class="sxs-lookup"><span data-stu-id="3e11c-177">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="3e11c-178">应用使用上一次在一个给定键上设置值的选项。</span><span class="sxs-lookup"><span data-stu-id="3e11c-178">The app uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="3e11c-179">可以在 [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration%2A) 中获取由 `ConfigureAppConfiguration` 创建的配置以用于后续操作，并将其作为 DI 的服务。</span><span class="sxs-lookup"><span data-stu-id="3e11c-179">The configuration created by `ConfigureAppConfiguration` is available in [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration%2A) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="3e11c-180">主机配置也会添加到应用配置。</span><span class="sxs-lookup"><span data-stu-id="3e11c-180">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="3e11c-181">有关详细信息，请参阅 [.NET 中的配置](configuration.md)。</span><span class="sxs-lookup"><span data-stu-id="3e11c-181">For more information, see [Configuration in .NET](configuration.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="3e11c-182">另请参阅</span><span class="sxs-lookup"><span data-stu-id="3e11c-182">See also</span></span>

- [<span data-ttu-id="3e11c-183">.NET 中的配置</span><span class="sxs-lookup"><span data-stu-id="3e11c-183">Configuration in .NET</span></span>](configuration.md)
- [<span data-ttu-id="3e11c-184">ASP.NET Core Web 主机</span><span class="sxs-lookup"><span data-stu-id="3e11c-184">ASP.NET Core Web Host</span></span>](/aspnet/core/fundamentals/host/web-host)
- <span data-ttu-id="3e11c-185">应在 [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) 存储库中创建通用主机 bug</span><span class="sxs-lookup"><span data-stu-id="3e11c-185">Generic host bugs should be created in the [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) repo</span></span>
