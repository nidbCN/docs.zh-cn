---
title: 应用程序性能管理-适用于 WCF 开发人员的 gRPC
description: ASP.NET Core gRPC 应用程序的日志记录、指标和跟踪。
ms.date: 12/15/2020
ms.openlocfilehash: 06515762e3e5febf2d11dea14524d5e3f586a760
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874545"
---
# <a name="application-performance-management"></a>应用程序性能管理

在生产环境（如 Kubernetes）中，监视应用程序以确保它们以最佳状态运行非常重要。 日志记录和指标特别重要。 ASP.NET Core （包括 gRPC）为生成和管理日志消息和指标数据以及 *跟踪* 数据提供内置支持。

## <a name="the-difference-between-logging-and-metrics"></a>日志记录与度量值之间的差异

*日志记录* 与文本消息相关，这些文本消息记录了有关系统中所发生内容的详细信息。 日志消息可能包含异常数据，如堆栈跟踪或提供消息上下文的结构化数据。 日志记录输出通常写入到可搜索的文本存储。

*度量值* 是指通过在仪表板中使用图表和图形来聚合并显示的数值数据。 该面板提供应用程序的总体运行状况和性能的视图。 指标数据还可用于在超过阈值时触发自动警报。 下面是度量值数据的一些示例：

- 处理请求所用的时间。
- 服务实例每秒处理的请求数。
- 实例上失败的请求数。

## <a name="logging-in-aspnet-core-grpc"></a>ASP.NET Core gRPC 中的日志记录

ASP.NET Core 提供了针对日志记录的内置支持，采用的形式为： [记录](https://www.nuget.org/packages/Microsoft.Extensions.Logging) NuGet 包。 此库的核心部分随 Web SDK 一起提供，因此不需要手动安装它。 默认情况下，将日志消息写入 ("控制台" ) 和任何附加调试器中的标准输出。 若要将日志写入永久性外部数据存储，你可能需要导入 [可选的日志记录接收器包](/aspnet/core/fundamentals/logging/?view=aspnetcore-3.0#third-party-logging-providers)。

ASP.NET Core gRPC 框架将详细的诊断日志记录消息写入到此日志记录框架，以便可以处理这些消息并将其与应用程序的消息一起存储。

### <a name="produce-log-messages"></a>生成日志消息

日志记录扩展会自动注册 ASP.NET Core 的依赖项注入系统，因此，你可以将记录器指定为 gRPC 服务类型上的构造函数参数。

```csharp
public class StockData : Stocks.StocksBase
{
    private readonly ILogger<StockData> _logger;

    public StockData(ILogger<StockData> logger)
    {
        _logger = logger;
    }
}
```

许多日志消息（例如请求和异常）由 ASP.NET Core 和 gRPC 框架组件提供。 添加自己的日志消息，提供有关应用程序逻辑的详细信息和上下文，而不是更低级别的问题。

有关编写日志消息和可用日志记录接收器和目标的详细信息，请参阅  [在 .Net Core 中进行日志记录和 ASP.NET Core](/aspnet/core/fundamentals/logging/)。

## <a name="metrics-in-aspnet-core-grpc"></a>ASP.NET Core gRPC 中的指标

.NET Core 运行时提供一组用于发出和观察指标的组件。 其中包括和类等 Api <xref:System.Diagnostics.Tracing.EventSource> <xref:System.Diagnostics.Tracing.EventCounter> 。 这些 Api 可以发出可由外部进程使用的基本数值数据，如 [dotnet 全局工具](../../core/diagnostics/dotnet-counters.md)或 Windows 事件跟踪。 有关 `EventCounter` 在自己的代码中使用的详细信息，请参阅 [EventCounter 简介](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md)。

对于更高级的指标和将指标数据写入到更广泛的数据存储区，可以尝试使用称为 " [应用指标](https://www.app-metrics.io)" 的开源项目。 此库套件提供了一组广泛的类型来检测你的代码。 它还提供了用于将指标写入不同类型目标的包，这些目标包括时间序列数据库（如 Prometheus 和 InfluxDB）和 [Application Insights](/azure/azure-monitor/app/app-insights-overview)。 [AspNetCore](https://www.nuget.org/packages/App.Metrics.AspNetCore.Mvc/) NuGet 包甚至添加了一组综合的基本指标，这些指标是通过与 ASP.NET Core 框架的集成自动生成的。 项目网站提供了用于通过[Grafana](https://grafana.com/)可视化平台显示这些指标的[模板](https://www.app-metrics.io/samples/grafana/)。

### <a name="produce-metrics"></a>生成指标

大多数度量值平台支持以下类型：

| 度量值类型 | 说明 |
| ----------- | ----------- |
| 计数器     | 跟踪发生某些情况的频率，如请求和错误。 |
| 仪表       | 记录随时间变化的单个值，如活动连接。 |
| 直方图   | 度量跨任意限制的值的分布。 例如，直方图可以跟踪数据集的大小，计算包含多少 <10 条记录、包含的11-100 记录数、包含的101-1000 记录数以及包含的 >1000 记录数。 |
| 计量       | 度量事件在不同时间范围内的发生速率。 |
| 计时器       | 跟踪事件的持续时间以及事件发生的速率（以直方图形式存储）。 |

通过使用 *应用指标*， `IMetrics` 可通过依赖关系注入获取接口，并用于记录 gRPC 服务的任何指标。 下面的示例演示如何计算一段 `Get` 时间内发出的请求数：

```csharp
public class StockData : Stocks.StocksBase
{
    private static readonly CounterOptions GetRequestCounter = new CounterOptions
    {
        Name = "StockData_Get_Requests",
        MeasurementUnit = Unit.Calls
    };

    private readonly IStockRepository _repository;
    private readonly IMetrics _metrics;

    public StockData(IStockRepository repository, IMetrics metrics)
    {
        _repository = repository;
        _metrics = metrics;
    }

    public override async Task<GetResponse> Get(GetRequest request, ServerCallContext context)
    {
        _metrics.Measure.Counter.Increment(GetRequestCounter);

        // Serve request...
    }
}
```

### <a name="store-and-visualize-metrics-data"></a>存储和可视化指标数据

存储度量值数据的最佳方式是在时序 *数据库* 中，这是一个专用的数据存储，用于记录用时间戳标记的数值数据系列。 这些数据库最常见的是 [Prometheus](https://prometheus.io/) 和 [InfluxDB](https://www.influxdata.com/products/influxdb-overview/)。 Microsoft Azure 还通过 [Azure Monitor](/azure/azure-monitor/overview) 服务提供专用的度量值存储。

用于可视化度量值数据的当前走向解决方案是 [Grafana](https://grafana.com)，可用于各种存储提供程序。 下图显示了一个示例 Grafana 仪表板，该仪表板显示运行 StockData 示例的 Linkerd service 网格中的指标：

![Grafana 仪表板的屏幕截图](media/application-performance-management/grafana-screenshot.png)

### <a name="metrics-based-alerting"></a>基于指标的警报

度量值数据的数值性质意味着它非常适合用于驱动警报系统，并在某个值超出某些定义的容差时通知开发人员或支持工程师。 已提到的平台都通过各种选项（包括电子邮件、短信或仪表板中的可视化效果）提供警报支持。

## <a name="distributed-tracing"></a>分布式跟踪

分布式跟踪是一种相对较新的监视开发，它怀疑于不断使用微服务和分布式体系结构。 来自客户端浏览器、应用程序或设备的单个请求可分解为多个步骤和子请求，并涉及到跨网络使用许多服务。 此活动使日志消息和度量值难以与触发它们的特定请求关联起来。 分布式跟踪将标识符应用于请求，并允许日志和指标与特定操作相关联。 此跟踪类似于 [WCF 的端到端跟踪](../../framework/wcf/diagnostics/tracing/end-to-end-tracing.md)，但它在多个平台上应用。

分布式跟踪在受欢迎程度上迅速增长，开始实现标准化。 云本机计算基础创建了 [开放跟踪标准](https://opentracing.io)，尝试为使用后端（例如 [JAEGER](https://www.jaegertracing.io/) 和 [弹性 APM](https://www.elastic.co/products/apm)）提供与供应商无关的库。 同时，Google 创建了 [OpenCensus 项目](https://opencensus.io/) 来处理相同的一组问题。 这两个项目合并到一个新项目 [OpenTelemetry](https://opentelemetry.io)中，该项目旨在成为未来的行业标准。

### <a name="how-distributed-tracing-works"></a>分布式跟踪的工作原理

分布式跟踪基于 *跨越* 的概念：已命名的计时操作，这些操作是单个 *跟踪* 的一部分，它可能涉及在系统的多个节点上进行处理。 当启动新操作时，将使用唯一标识符创建跟踪。 对于每个子操作，会使用其自己的标识符和跟踪标识符来创建一个跨度。 当请求在系统中传递时，各组件可以创建包括其 *父* 范围的标识符的 *子* 范围。 一个范围包含一个 *上下文*，其中包含跟踪和范围标识符，以及 (称为 *行李*) 的键和值对形式的有用数据。

### <a name="distributed-tracing-with-diagnosticsource"></a>分布式跟踪与 `DiagnosticSource`

.NET 有一个内部模块，可以很好地映射到分布式跟踪和跨越： [DiagnosticSource](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#diagnosticsource-users-guide)。 该 `DiagnosticSource` 模块具有 *活动* 的概念，并提供一种简单的方法来在进程内生成和使用诊断。 活动实际上是分布式跟踪的实现或跟踪内的跨度。 模块的内部机制负责父子活动（包括分配标识符）。 有关使用类型的详细信息 `Activity` ，请参阅 [GitHub 上的活动用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md#activity-user-guide)。

由于 `DiagnosticSource` 是核心框架和更高版本的一部分，因此支持多个核心组件。 其中包括 <xref:System.Net.Http.HttpClient> 、Entity Framework Core 和 ASP.NET Core，包括 gRPC 框架中的显式支持。 当 ASP.NET Core 收到请求时，它将检查与 [W3C 跟踪上下文](https://www.w3.org/TR/trace-context) 标准匹配的一对 HTTP 标头。 如果找到了标头，则将使用标头中的标识值和上下文启动活动。 如果未找到任何标头，则会启动一个活动，其中生成的标识值与标准格式匹配。 在此活动的生存期内，由框架或应用程序代码生成的任何诊断，都可以用 trace 和 span 标识符进行标记。 `HttpClient`此支持通过检查每个请求的当前活动并自动将跟踪标头添加到传出请求，进一步扩展了此功能。

ASP.NET Core gRPC 客户端和服务器库包括对和的显式支持 `DiagnosticSource` `Activity` ，并自动创建活动和应用和使用标头信息。

> [!NOTE]
> 仅当侦听器使用诊断信息时，才会发生这种情况。 如果没有侦听器，则不会写入任何诊断，也不会创建任何活动。

### <a name="add-your-own-diagnosticsource-and-activity"></a>添加你自己 `DiagnosticSource` 的和 `Activity`

若要在应用程序代码中添加自己的诊断或创建显式范围，请参阅 [DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#instrumenting-with-diagnosticsourcediagnosticlistener) 和 [活动用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md#activity-usage)。

### <a name="store-distributed-trace-data"></a>存储分布式跟踪数据

撰写本文时，OpenTelemetry 项目仍处于早期阶段，并且只有 alpha 质量的包可用于 .NET 应用程序。 OpenTracing 项目当前提供更成熟的库。

下一节介绍了 OpenTracing API。 如果要改为在应用程序中使用 OpenTelemetry API，请参阅 GitHub 上的 [OpenTelemetry .NET SDK 存储库](https://github.com/open-telemetry/opentelemetry-dotnet) 。

#### <a name="use-the-opentracing-package-to-store-distributed-trace-data"></a>使用 OpenTracing 包存储分布式跟踪数据

[OpenTracing NuGet 包](https://www.nuget.org/packages/OpenTracing/)支持所有 OpenTracing 兼容的后端 (，它们可独立于) 使用 `DiagnosticSource` 。 还有一个来自 OpenTracing API 贡献项目的其他包 [OpenTracing. Contrib. NetCore](https://www.nuget.org/packages/OpenTracing.Contrib.NetCore/)。 此包将添加 `DiagnosticSource` 侦听器，并自动将事件和活动写入后端。 启用此包非常简单，只是从 NuGet 安装它并将其作为服务添加到你的 `Startup` 类中。

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddOpenTracing();
    }
}
```

OpenTracing 包是抽象层，因此它需要特定于后端的实现。 OpenTracing API 实现适用于以下开源后端。

| 名称 | 程序包 | 网站 |
| ---- | ------- | -------- |
| Jaeger | [Jaeger](https://www.nuget.org/packages/Jaeger/) | [jaegertracing.io](https://jaegertracing.io) |
| 弹性 APM | [NetCoreAll](https://www.nuget.org/packages/Elastic.Apm.NetCoreAll/) | [elastic.co/products/apm](https://www.elastic.co/products/apm) |

有关适用于 .NET 的 OpenTracing API 的详细信息，请参阅 GitHub 上的 [OpenTracing For c #](https://github.com/opentracing/opentracing-csharp) 和 [OpenTracing Contrib c #/.NET Core](https://github.com/opentracing-contrib/csharp-netcore) 存储库。

>[!div class="step-by-step"]
>[上一页](load-balancing.md)
>[下一页](appendix.md)
