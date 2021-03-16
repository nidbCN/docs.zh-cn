---
title: .NET 中的日志记录提供程序
description: 了解如何在 .NET 应用程序中使用日志记录提供程序 API。
author: IEvangelist
ms.author: dapine
ms.date: 02/16/2021
ms.openlocfilehash: 7265e51a4d92cd99abebef2ebf0bc5db37b306e3
ms.sourcegitcommit: 456b3cd82a87b453fa737b4661295070d1b6d684
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/17/2021
ms.locfileid: "102401978"
---
# <a name="logging-providers-in-net"></a><span data-ttu-id="c2bc6-103">.NET 中的日志记录提供程序</span><span class="sxs-lookup"><span data-stu-id="c2bc6-103">Logging providers in .NET</span></span>

<span data-ttu-id="c2bc6-104">日志提供程序会保留日志，但 `Console` 提供程序除外，后者仅将日志显示为标准输出。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-104">Logging providers persist logs, except for the `Console` provider, which only displays logs as standard output.</span></span> <span data-ttu-id="c2bc6-105">例如，Azure Application Insights 提供程序将日志存储在 Azure Application Insights 中。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-105">For example, the Azure Application Insights provider stores logs in Azure Application Insights.</span></span> <span data-ttu-id="c2bc6-106">可以启用多个提供程序。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-106">Multiple providers can be enabled.</span></span>

<span data-ttu-id="c2bc6-107">默认的 .NET 辅助角色应用模板：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-107">The default .NET Worker app templates:</span></span>

- <span data-ttu-id="c2bc6-108">使用[通用主机](generic-host.md)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-108">Use the [Generic Host](generic-host.md).</span></span>
- <span data-ttu-id="c2bc6-109">调用 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>，这将添加以下日志记录提供程序：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-109">Call <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder%2A>, which adds the following logging providers:</span></span>
  - [<span data-ttu-id="c2bc6-110">控制台</span><span class="sxs-lookup"><span data-stu-id="c2bc6-110">Console</span></span>](#console)
  - [<span data-ttu-id="c2bc6-111">调试</span><span class="sxs-lookup"><span data-stu-id="c2bc6-111">Debug</span></span>](#debug)
  - [<span data-ttu-id="c2bc6-112">EventSource</span><span class="sxs-lookup"><span data-stu-id="c2bc6-112">EventSource</span></span>](#event-source)
  - <span data-ttu-id="c2bc6-113">[EventLog](#windows-eventlog)：仅限 Windows</span><span class="sxs-lookup"><span data-stu-id="c2bc6-113">[EventLog](#windows-eventlog): Windows only</span></span>

:::code language="csharp" source="snippets/configuration/console/Program.cs" highlight="18":::

<span data-ttu-id="c2bc6-114">上面的代码显示了使用 .NET 辅助角色应用模板创建的 `Program` 类。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-114">The preceding code shows the `Program` class created with the .NET Worker app templates.</span></span> <span data-ttu-id="c2bc6-115">接下来的几个部分提供了一些示例，它们基于使用通用主机的 .NET 辅助角色应用模板。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-115">The next several sections provide samples based on the .NET Worker app templates, which use the Generic Host.</span></span>

<span data-ttu-id="c2bc6-116">若要替代 `Host.CreateDefaultBuilder` 添加的默认日志记录提供程序集，请调用 `ClearProviders` 并添加所需的日志记录提供程序。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-116">To override the default set of logging providers added by `Host.CreateDefaultBuilder`, call `ClearProviders` and add the logging providers you want.</span></span> <span data-ttu-id="c2bc6-117">例如，以下代码：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-117">For example, the following code:</span></span>

- <span data-ttu-id="c2bc6-118">调用 <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> 以从生成器中删除所有 <xref:Microsoft.Extensions.Logging.ILoggerProvider> 实例。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-118">Calls <xref:Microsoft.Extensions.Logging.LoggingBuilderExtensions.ClearProviders%2A> to remove all the <xref:Microsoft.Extensions.Logging.ILoggerProvider> instances from the builder.</span></span>
- <span data-ttu-id="c2bc6-119">添加[控制台](#console)日志记录提供程序。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-119">Adds the [Console](#console) logging provider.</span></span>

```csharp
static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureLogging(logging =>
        {
            logging.ClearProviders();
            logging.AddConsole();
        });
```

<span data-ttu-id="c2bc6-120">有关其他提供程序，请参阅：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-120">For additional providers, see:</span></span>

- <span data-ttu-id="c2bc6-121">[内置日志记录提供程序](#built-in-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-121">[Built-in logging providers](#built-in-logging-providers).</span></span>
- <span data-ttu-id="c2bc6-122">[第三方日志记录提供程序](#third-party-logging-providers)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-122">[Third-party logging providers](#third-party-logging-providers).</span></span>

## <a name="configure-a-service-that-depends-on-ilogger"></a><span data-ttu-id="c2bc6-123">配置依赖于 ILogger 的服务</span><span class="sxs-lookup"><span data-stu-id="c2bc6-123">Configure a service that depends on ILogger</span></span>

<span data-ttu-id="c2bc6-124">若要配置依赖于 `ILogger<T>` 的服务，请使用构造函数注入或提供工厂方法。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-124">To configure a service that depends on `ILogger<T>`, use constructor injection or provide a factory method.</span></span> <span data-ttu-id="c2bc6-125">只有在没有其他选择的情况下，才建议使用工厂方法。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-125">The factory method approach is recommended only if there is no other option.</span></span> <span data-ttu-id="c2bc6-126">例如，假设某个服务需要由 DI 提供的 `ILogger<T>` 实例：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-126">For example, consider a service that needs an `ILogger<T>` instance provided by DI:</span></span>

```csharp
.ConfigureServices(services =>
    services.AddSingleton<IExampleService>(container =>
        new DefaultExampleService
        {
            Logger = container.GetRequiredService<ILogger<IExampleService>>()
        }));
```

<span data-ttu-id="c2bc6-127">上述代码是 [Func<IServiceProvider, IExampleService>](/dotnet/api/system.func-2)，它在 DI 容器首次需要构造 `IExampleService` 实例时运行。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-127">The preceding code is a [Func<IServiceProvider, IExampleService>](/dotnet/api/system.func-2) that runs the first time the DI container needs to construct an instance of `IExampleService`.</span></span> <span data-ttu-id="c2bc6-128">可以用这种方式访问任何已注册的服务。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-128">You can access any of the registered services in this way.</span></span>

## <a name="built-in-logging-providers"></a><span data-ttu-id="c2bc6-129">内置日志记录提供程序</span><span class="sxs-lookup"><span data-stu-id="c2bc6-129">Built-in logging providers</span></span>

<span data-ttu-id="c2bc6-130">Microsoft 扩展包含以下日志记录提供程序作为运行时库的一部分：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-130">Microsoft Extensions include the following logging providers as part of the runtime libraries:</span></span>

- [<span data-ttu-id="c2bc6-131">控制台</span><span class="sxs-lookup"><span data-stu-id="c2bc6-131">Console</span></span>](#console)
- [<span data-ttu-id="c2bc6-132">调试</span><span class="sxs-lookup"><span data-stu-id="c2bc6-132">Debug</span></span>](#debug)
- [<span data-ttu-id="c2bc6-133">EventSource</span><span class="sxs-lookup"><span data-stu-id="c2bc6-133">EventSource</span></span>](#event-source)
- [<span data-ttu-id="c2bc6-134">EventLog</span><span class="sxs-lookup"><span data-stu-id="c2bc6-134">EventLog</span></span>](#windows-eventlog)

<span data-ttu-id="c2bc6-135">以下日志记录提供程序由 Microsoft 提供，但不是运行时库的一部分。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-135">The following logging providers are shipped by Microsoft, but not as part of the runtime libraries.</span></span> <span data-ttu-id="c2bc6-136">它们必须作为附加 NuGet 包安装。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-136">They must be installed as additional NuGet packages.</span></span>

- [<span data-ttu-id="c2bc6-137">AzureAppServicesFile 和 AzureAppServicesBlob</span><span class="sxs-lookup"><span data-stu-id="c2bc6-137">AzureAppServicesFile and AzureAppServicesBlob</span></span>](#azure-app-service)
- [<span data-ttu-id="c2bc6-138">ApplicationInsights</span><span class="sxs-lookup"><span data-stu-id="c2bc6-138">ApplicationInsights</span></span>](#azure-application-insights)

### <a name="console"></a><span data-ttu-id="c2bc6-139">控制台</span><span class="sxs-lookup"><span data-stu-id="c2bc6-139">Console</span></span>

<span data-ttu-id="c2bc6-140">`Console` 提供程序将输出记录到控制台。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-140">The `Console` provider logs output to the console.</span></span>

### <a name="debug"></a><span data-ttu-id="c2bc6-141">调试</span><span class="sxs-lookup"><span data-stu-id="c2bc6-141">Debug</span></span>

<span data-ttu-id="c2bc6-142">`Debug` 提供程序通过使用 [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) 类来写入日志输出。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-142">The `Debug` provider writes log output by using the [System.Diagnostics.Debug](/dotnet/api/system.diagnostics.debug) class.</span></span> <span data-ttu-id="c2bc6-143">对 `System.Diagnostics.Debug.WriteLine` 的调用写入到 `Debug` 提供程序。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-143">Calls to `System.Diagnostics.Debug.WriteLine` write to the `Debug` provider.</span></span>

<span data-ttu-id="c2bc6-144">在 Linux 上，`Debug` 提供程序日志位置取决于分发，并且可以是以下位置之一：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-144">On Linux, the `Debug` provider log location is distribution-dependent and may be one of the following:</span></span>

- <span data-ttu-id="c2bc6-145">*/var/log/message*</span><span class="sxs-lookup"><span data-stu-id="c2bc6-145">*/var/log/message*</span></span>
- <span data-ttu-id="c2bc6-146">*/var/log/syslog*</span><span class="sxs-lookup"><span data-stu-id="c2bc6-146">*/var/log/syslog*</span></span>

### <a name="event-source"></a><span data-ttu-id="c2bc6-147">事件来源</span><span class="sxs-lookup"><span data-stu-id="c2bc6-147">Event Source</span></span>

<span data-ttu-id="c2bc6-148">`EventSource` 提供程序写入名称为 `Microsoft-Extensions-Logging` 的跨平台事件源。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-148">The `EventSource` provider writes to a cross-platform event source with the name `Microsoft-Extensions-Logging`.</span></span> <span data-ttu-id="c2bc6-149">在 Windows 上，提供程序使用的是 [ETW](/windows/win32/etw/event-tracing-portal)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-149">On Windows, the provider uses [ETW](/windows/win32/etw/event-tracing-portal).</span></span>

#### <a name="dotnet-trace-tooling"></a><span data-ttu-id="c2bc6-150">dotnet 跟踪工具</span><span class="sxs-lookup"><span data-stu-id="c2bc6-150">dotnet trace tooling</span></span>

<span data-ttu-id="c2bc6-151">[dotnet-trace](../diagnostics/dotnet-trace.md) 工具是一种跨平台 CLI 全局工具，可用于收集正在运行的进程的 .NET Core 跟踪。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-151">The [dotnet-trace](../diagnostics/dotnet-trace.md) tool is a cross-platform CLI global tool that enables the collection of .NET Core traces of a running process.</span></span> <span data-ttu-id="c2bc6-152">该工具会使用 <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource> 收集 <xref:Microsoft.Extensions.Logging.EventSource> 提供程序数据。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-152">The tool collects <xref:Microsoft.Extensions.Logging.EventSource> provider data using a <xref:Microsoft.Extensions.Logging.EventSource.LoggingEventSource>.</span></span>

<span data-ttu-id="c2bc6-153">有关安装说明，请参阅 [dotnet-trace](../diagnostics/dotnet-trace.md)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-153">See [dotnet-trace](../diagnostics/dotnet-trace.md) for installation instructions.</span></span> <span data-ttu-id="c2bc6-154">有关使用 `dotnet-trace` 的诊断教程，请查看[在 .NET Core 中调试 CPU 使用率高的问题](../diagnostics/debug-highcpu.md)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-154">For a diagnostic tutorial using `dotnet-trace`, see [Debug high CPU usage in .NET Core](../diagnostics/debug-highcpu.md).</span></span>

### <a name="windows-eventlog"></a><span data-ttu-id="c2bc6-155">Windows 事件日志</span><span class="sxs-lookup"><span data-stu-id="c2bc6-155">Windows EventLog</span></span>

<span data-ttu-id="c2bc6-156">`EventLog` 提供程序将日志输出发送到 Windows 事件日志。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-156">The `EventLog` provider sends log output to the Windows Event Log.</span></span> <span data-ttu-id="c2bc6-157">与其他提供程序不同，`EventLog` 提供程序不继承默认的非提供程序设置。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-157">Unlike the other providers, the `EventLog` provider does ***not*** inherit the default non-provider settings.</span></span> <span data-ttu-id="c2bc6-158">如果未指定 `EventLog` 日志设置，则它们默认为 `LogLevel.Warning`。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-158">If `EventLog` log settings aren't specified, they default to `LogLevel.Warning`.</span></span>

<span data-ttu-id="c2bc6-159">若要记录低于 <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType> 的事件，请显式设置日志级别。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-159">To log events lower than <xref:Microsoft.Extensions.Logging.LogLevel.Warning?displayProperty=nameWithType>, explicitly set the log level.</span></span> <span data-ttu-id="c2bc6-160">以下示例将事件日志的默认日志级别设置为 <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-160">The following example sets the Event Log default log level to <xref:Microsoft.Extensions.Logging.LogLevel.Information?displayProperty=nameWithType>:</span></span>

```json
"Logging": {
  "EventLog": {
    "LogLevel": {
      "Default": "Information"
    }
  }
}
```

<span data-ttu-id="c2bc6-161">[AddEventLog 重载](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions)可以传入 <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-161">[AddEventLog overloads](xref:Microsoft.Extensions.Logging.EventLoggerFactoryExtensions) can pass in <xref:Microsoft.Extensions.Logging.EventLog.EventLogSettings>.</span></span> <span data-ttu-id="c2bc6-162">如果为 `null` 或未指定，则使用以下默认设置：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-162">If `null` or not specified, the following default settings are used:</span></span>

- <span data-ttu-id="c2bc6-163">`LogName`：“Application”</span><span class="sxs-lookup"><span data-stu-id="c2bc6-163">`LogName`: "Application"</span></span>
- <span data-ttu-id="c2bc6-164">`SourceName`：“.NET Runtime”</span><span class="sxs-lookup"><span data-stu-id="c2bc6-164">`SourceName`: ".NET Runtime"</span></span>
- <span data-ttu-id="c2bc6-165">`MachineName`：使用本地计算机名称。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-165">`MachineName`: The local machine name is used.</span></span>

<span data-ttu-id="c2bc6-166">以下代码将 `SourceName` 从默认值 `".NET Runtime"` 更改为 `CustomLogs`：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-166">The following code changes the `SourceName` from the default value of `".NET Runtime"` to `CustomLogs`:</span></span>

```csharp
public class Program
{
    static async Task Main(string[] args)
    {
        using IHost host = CreateHostBuilder(args).Build();

        // Application code should start here.

        await host.RunAsync();
    }

    static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureLogging(logging =>
                logging.AddEventLog(configuration =>
                    configuration.SourceName = "CustomLogs"));
}
```

### <a name="azure-app-service"></a><span data-ttu-id="c2bc6-167">Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="c2bc6-167">Azure App Service</span></span>

<span data-ttu-id="c2bc6-168">[Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) 提供程序包将日志写入 Azure App Service 应用的文件系统，以及 Azure 存储帐户中的 [blob 存储](/azure/storage/blobs/storage-quickstart-blobs-dotnet#what-is-blob-storage)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-168">The [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) provider package writes logs to text files in an Azure App Service app's file system and to [blob storage](/azure/storage/blobs/storage-quickstart-blobs-dotnet#what-is-blob-storage) in an Azure Storage account.</span></span>

<span data-ttu-id="c2bc6-169">运行时库中不包括该提供程序包。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-169">The provider package isn't included in the runtime libraries.</span></span> <span data-ttu-id="c2bc6-170">若要使用提供程序，请将提供程序包添加到项目。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-170">To use the provider, add the provider package to the project.</span></span>

<span data-ttu-id="c2bc6-171">要配置提供程序设置，请使用 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> 和 <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-171">To configure provider settings, use <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureFileLoggerOptions> and <xref:Microsoft.Extensions.Logging.AzureAppServices.AzureBlobLoggerOptions>, as shown in the following example:</span></span>

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        using IHost host = CreateHostBuilder(args).Build();

        // Application code should start here.

        await host.RunAsync();
    }

    static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureLogging(logging =>
                logging.AddAzureWebAppDiagnostics())
            .ConfigureServices(services =>
                services.Configure<AzureFileLoggerOptions>(options =>
                {
                    options.FileName = "azure-diagnostics-";
                    options.FileSizeLimit = 50 * 1024;
                    options.RetainedFileCountLimit = 5;
                })
                .Configure<AzureBlobLoggerOptions>(options =>
                {
                    options.BlobName = "log.txt";
                }));
}
```

<span data-ttu-id="c2bc6-172">部署到 Azure 应用服务时，应用使用 Azure 门户的“应用服务”页面的[应用服务日志](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows)部分中的设置。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-172">When deployed to Azure App Service, the app uses the settings in the [App Service logs](/azure/app-service/web-sites-enable-diagnostic-log/#enable-application-logging-windows) section of the **App Service** page of the Azure portal.</span></span> <span data-ttu-id="c2bc6-173">更新以下设置后，更改立即生效，无需重启或重新部署应用。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-173">When the following settings are updated, the changes take effect immediately without requiring a restart or redeployment of the app.</span></span>

- <span data-ttu-id="c2bc6-174">应用程序日志记录(Filesystem)</span><span class="sxs-lookup"><span data-stu-id="c2bc6-174">**Application Logging (Filesystem)**</span></span>
- <span data-ttu-id="c2bc6-175">应用程序日志记录(Blob)</span><span class="sxs-lookup"><span data-stu-id="c2bc6-175">**Application Logging (Blob)**</span></span>

<span data-ttu-id="c2bc6-176">日志文件的默认位置是 D:\\home\\LogFiles\\Application 文件夹，默认文件名为 diagnostics-yyyymmdd.txt   。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-176">The default location for log files is in the *D:\\home\\LogFiles\\Application* folder, and the default file name is *diagnostics-yyyymmdd.txt*.</span></span> <span data-ttu-id="c2bc6-177">默认文件大小上限为 10 MB，默认最大保留文件数为 2。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-177">The default file size limit is 10 MB, and the default maximum number of files retained is 2.</span></span> <span data-ttu-id="c2bc6-178">默认 blob 名为 {app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt  。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-178">The default blob name is *{app-name}{timestamp}/yyyy/mm/dd/hh/{guid}-applicationLog.txt*.</span></span>

<span data-ttu-id="c2bc6-179">仅当项目在 Azure 环境中运行时，此提供程序才记录日志。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-179">This provider only logs when the project runs in the Azure environment.</span></span>

#### <a name="azure-log-streaming"></a><span data-ttu-id="c2bc6-180">Azure 日志流式处理</span><span class="sxs-lookup"><span data-stu-id="c2bc6-180">Azure log streaming</span></span>

<span data-ttu-id="c2bc6-181">Azure 日志流式处理支持从以下位置实时查看日志活动：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-181">Azure log streaming supports viewing log activity in real time from:</span></span>

- <span data-ttu-id="c2bc6-182">应用服务器</span><span class="sxs-lookup"><span data-stu-id="c2bc6-182">The app server</span></span>
- <span data-ttu-id="c2bc6-183">Web 服务器</span><span class="sxs-lookup"><span data-stu-id="c2bc6-183">The web server</span></span>
- <span data-ttu-id="c2bc6-184">请求跟踪失败</span><span class="sxs-lookup"><span data-stu-id="c2bc6-184">Failed request tracing</span></span>

<span data-ttu-id="c2bc6-185">要配置 Azure 日志流式处理，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-185">To configure Azure log streaming:</span></span>

- <span data-ttu-id="c2bc6-186">从应用的门户页导航到“应用服务日志”页。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-186">Navigate to the **App Service logs** page from the app's portal page.</span></span>
- <span data-ttu-id="c2bc6-187">将“应用程序日志记录(Filesystem)”设置为“开”   。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-187">Set **Application Logging (Filesystem)** to **On**.</span></span>
- <span data-ttu-id="c2bc6-188">选择日志级别  。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-188">Choose the log **Level**.</span></span> <span data-ttu-id="c2bc6-189">此设置仅适用于 Azure 日志流式处理。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-189">This setting only applies to Azure log streaming.</span></span>

<span data-ttu-id="c2bc6-190">导航到“日志流”页面以查看日志。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-190">Navigate to the **Log Stream** page to view logs.</span></span> <span data-ttu-id="c2bc6-191">记录的消息使用 `ILogger` 接口进行记录。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-191">The logged messages are logged with the `ILogger` interface.</span></span>

### <a name="azure-application-insights"></a><span data-ttu-id="c2bc6-192">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="c2bc6-192">Azure Application Insights</span></span>

<span data-ttu-id="c2bc6-193">[Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) 提供程序包将日志写入 [Azure Application Insights](/azure/azure-monitor/app/cloudservices)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-193">The [Microsoft.Extensions.Logging.ApplicationInsights](https://www.nuget.org/packages/Microsoft.Extensions.Logging.ApplicationInsights) provider package writes logs to [Azure Application Insights](/azure/azure-monitor/app/cloudservices).</span></span> <span data-ttu-id="c2bc6-194">Application Insights 是一项服务，可监视 Web 应用并提供用于查询和分析遥测数据的工具。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-194">Application Insights is a service that monitors a web app and provides tools for querying and analyzing the telemetry data.</span></span> <span data-ttu-id="c2bc6-195">如果使用此提供程序，则可以使用 Application Insights 工具来查询和分析日志。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-195">If you use this provider, you can query and analyze your logs by using the Application Insights tools.</span></span>

<span data-ttu-id="c2bc6-196">有关更多信息，请参见以下资源：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-196">For more information, see the following resources:</span></span>

- [<span data-ttu-id="c2bc6-197">Application Insights 概述</span><span class="sxs-lookup"><span data-stu-id="c2bc6-197">Application Insights overview</span></span>](/azure/application-insights/app-insights-overview)
- <span data-ttu-id="c2bc6-198">[.NET Core ILogger 日志的 ApplicationInsightsLoggerProvider](/azure/azure-monitor/app/ilogger) - 如果想要在没有其他 Application Insights 遥测的情况下实现日志记录提供程序，请从这里开始。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-198">[ApplicationInsightsLoggerProvider for .NET Core ILogger logs](/azure/azure-monitor/app/ilogger) - Start here if you want to implement the logging provider without the rest of Application Insights telemetry.</span></span>
- <span data-ttu-id="c2bc6-199">[Application Insights 日志记录适配器](/azure/azure-monitor/app/asp-net-trace-logs)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-199">[Application Insights logging adapters](/azure/azure-monitor/app/asp-net-trace-logs).</span></span>
- <span data-ttu-id="c2bc6-200">[安装、配置和初始化 Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Microsoft Learn 网站上的交互式教程。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-200">[Install, configure, and initialize the Application Insights SDK](/learn/modules/instrument-web-app-code-with-application-insights) - Interactive tutorial on the Microsoft Learn site.</span></span>

## <a name="third-party-logging-providers"></a><span data-ttu-id="c2bc6-201">第三方日志记录提供程序</span><span class="sxs-lookup"><span data-stu-id="c2bc6-201">Third-party logging providers</span></span>

<span data-ttu-id="c2bc6-202">下面是适用于 .NET 工作负载的一些第三方日志记录提供程序：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-202">Here are some third-party logging frameworks that work with various .NET workloads:</span></span>

- <span data-ttu-id="c2bc6-203">[elmah.io](https://elmah.io)（[GitHub 存储库](https://github.com/elmahio/Elmah.Io.Extensions.Logging)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-203">[elmah.io](https://elmah.io) ([GitHub repo](https://github.com/elmahio/Elmah.Io.Extensions.Logging))</span></span>
- <span data-ttu-id="c2bc6-204">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html)（[GitHub 存储库](https://github.com/mattwcole/gelf-extensions-logging)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-204">[Gelf](https://docs.graylog.org/en/2.3/pages/gelf.html) ([GitHub repo](https://github.com/mattwcole/gelf-extensions-logging))</span></span>
- <span data-ttu-id="c2bc6-205">[JSNLog](http://jsnlog.com)（[GitHub 存储库](https://github.com/mperdeck/jsnlog)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-205">[JSNLog](http://jsnlog.com) ([GitHub repo](https://github.com/mperdeck/jsnlog))</span></span>
- <span data-ttu-id="c2bc6-206">[KissLog.net](https://kisslog.net)（[GitHub 存储库](https://github.com/catalingavan/KissLog-net)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-206">[KissLog.net](https://kisslog.net) ([GitHub repo](https://github.com/catalingavan/KissLog-net))</span></span>
- <span data-ttu-id="c2bc6-207">[Log4Net](https://logging.apache.org/log4net)（[GitHub 存储库](https://github.com/apache/logging-log4net)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-207">[Log4Net](https://logging.apache.org/log4net) ([GitHub repo](https://github.com/apache/logging-log4net))</span></span>
- <span data-ttu-id="c2bc6-208">[Loggr](https://loggr.net)（[GitHub 存储库](https://github.com/imobile3/Loggr.Extensions.Logging)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-208">[Loggr](https://loggr.net) ([GitHub repo](https://github.com/imobile3/Loggr.Extensions.Logging))</span></span>
- <span data-ttu-id="c2bc6-209">[NLog](https://nlog-project.org)（[GitHub 存储库](https://github.com/NLog/NLog.Extensions.Logging)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-209">[NLog](https://nlog-project.org) ([GitHub repo](https://github.com/NLog/NLog.Extensions.Logging))</span></span>
- <span data-ttu-id="c2bc6-210">[NReco.Logging](https://github.com/nreco/logging/blob/master/README.md)（[GitHub 存储库](https://github.com/nreco/logging)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-210">[NReco.Logging](https://github.com/nreco/logging/blob/master/README.md) ([GitHub repo](https://github.com/nreco/logging))</span></span>
- <span data-ttu-id="c2bc6-211">[Sentry](https://sentry.io/welcome)（[GitHub 存储库](https://github.com/getsentry/sentry-dotnet)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-211">[Sentry](https://sentry.io/welcome) ([GitHub repo](https://github.com/getsentry/sentry-dotnet))</span></span>
- <span data-ttu-id="c2bc6-212">[Serilog](https://serilog.net)（[GitHub 存储库](https://github.com/serilog/serilog-sinks-console)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-212">[Serilog](https://serilog.net) ([GitHub repo](https://github.com/serilog/serilog-sinks-console))</span></span>
- <span data-ttu-id="c2bc6-213">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging)（[GitHub 存储库](https://github.com/googleapis/google-cloud-dotnet)）</span><span class="sxs-lookup"><span data-stu-id="c2bc6-213">[Stackdriver](https://cloud.google.com/dotnet/docs/stackdriver#logging) ([GitHub repo](https://github.com/googleapis/google-cloud-dotnet))</span></span>

<span data-ttu-id="c2bc6-214">某些第三方框架可以执行[语义日志记录（又称结构化日志记录）](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-214">Some third-party frameworks can perform [semantic logging, also known as structured logging](https://softwareengineering.stackexchange.com/questions/312197/benefits-of-structured-logging-vs-basic-logging).</span></span>

<span data-ttu-id="c2bc6-215">使用第三方框架类似于使用以下内置提供程序之一：</span><span class="sxs-lookup"><span data-stu-id="c2bc6-215">Using a third-party framework is similar to using one of the built-in providers:</span></span>

1. <span data-ttu-id="c2bc6-216">将 NuGet 包添加到你的项目。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-216">Add a NuGet package to your project.</span></span>
1. <span data-ttu-id="c2bc6-217">调用日志记录框架提供的 `ILoggerFactory` 或 `ILoggingBuilder` 扩展方法。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-217">Call an `ILoggerFactory` or `ILoggingBuilder` extension method provided by the logging framework.</span></span>

<span data-ttu-id="c2bc6-218">有关详细信息，请参阅各提供程序的相关文档。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-218">For more information, see each provider's documentation.</span></span> <span data-ttu-id="c2bc6-219">Microsoft 不支持第三方日志记录提供程序。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-219">Third-party logging providers aren't supported by Microsoft.</span></span>

## <a name="see-also"></a><span data-ttu-id="c2bc6-220">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c2bc6-220">See also</span></span>

- <span data-ttu-id="c2bc6-221">[.NET 中的日志记录](logging.md)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-221">[Logging in .NET](logging.md).</span></span>
- <span data-ttu-id="c2bc6-222">[在 .NET 中实现自定义日志记录提供程序](custom-logging-provider.md)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-222">[Implement a custom logging provider in .NET](custom-logging-provider.md).</span></span>
- <span data-ttu-id="c2bc6-223">[.NET 中的高性能日志记录](high-performance-logging.md)。</span><span class="sxs-lookup"><span data-stu-id="c2bc6-223">[High-performance logging in .NET](high-performance-logging.md).</span></span>
