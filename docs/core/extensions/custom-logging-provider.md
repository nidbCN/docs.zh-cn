---
title: 在 .NET 中实现自定义日志记录提供程序
description: 了解如何在 .NET 应用程序中实现自定义日志记录提供程序。
author: IEvangelist
ms.author: dapine
ms.date: 09/25/2020
ms.topic: how-to
ms.openlocfilehash: 3a0af6296c2ade15ff1b75dce5a5f99bfe235ebf
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "102401932"
---
# <a name="implement-a-custom-logging-provider-in-net"></a><span data-ttu-id="41810-103">在 .NET 中实现自定义日志记录提供程序</span><span class="sxs-lookup"><span data-stu-id="41810-103">Implement a custom logging provider in .NET</span></span>

<span data-ttu-id="41810-104">有很多[日志记录提供程序](logging-providers.md)可用于常见日志记录需求。</span><span class="sxs-lookup"><span data-stu-id="41810-104">There are many [logging providers](logging-providers.md) available for common logging needs.</span></span> <span data-ttu-id="41810-105">如果某个可用的提供程序不满足你的应用程序需要，则你可能需要实现自定义的 <xref:Microsoft.Extensions.Logging.ILoggerProvider>。</span><span class="sxs-lookup"><span data-stu-id="41810-105">You may need to implement a custom <xref:Microsoft.Extensions.Logging.ILoggerProvider> when one of the available providers doesn't suit your application needs.</span></span> <span data-ttu-id="41810-106">在本文中，你将学习如何实现可用于在控制台中为日志着色的自定义日志记录提供程序。</span><span class="sxs-lookup"><span data-stu-id="41810-106">In this article, you'll learn how to implement a custom logging provider that can be used to colorize logs in the console.</span></span>

### <a name="sample-custom-logger-configuration"></a><span data-ttu-id="41810-107">示例自定义记录器配置</span><span class="sxs-lookup"><span data-stu-id="41810-107">Sample custom logger configuration</span></span>

<span data-ttu-id="41810-108">此示例会使用以下配置类型为每个日志级别和事件 ID 创建不同的颜色控制台条目：</span><span class="sxs-lookup"><span data-stu-id="41810-108">The sample creates different color console entries per log level and event ID using the following configuration type:</span></span>

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLoggerConfiguration.cs":::

<span data-ttu-id="41810-109">前面的代码将默认级别设置为 `Information`，将颜色设置为 `Green`，而且 `EventId` 隐式设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="41810-109">The preceding code sets the default level to `Information`, the color to `Green`, and the `EventId` is implicitly `0`.</span></span>

### <a name="create-the-custom-logger"></a><span data-ttu-id="41810-110">创建自定义记录器</span><span class="sxs-lookup"><span data-stu-id="41810-110">Create the custom logger</span></span>

<span data-ttu-id="41810-111">`ILogger` 实现类别名称通常是日志记录源。</span><span class="sxs-lookup"><span data-stu-id="41810-111">The `ILogger` implementation category name is typically the logging source.</span></span> <span data-ttu-id="41810-112">例如，创建记录器的类型：</span><span class="sxs-lookup"><span data-stu-id="41810-112">For example, the type where the logger is created:</span></span>

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLogger.cs":::

<span data-ttu-id="41810-113">前面的代码：</span><span class="sxs-lookup"><span data-stu-id="41810-113">The preceding code:</span></span>

- <span data-ttu-id="41810-114">为每个类别名称创建一个记录器实例。</span><span class="sxs-lookup"><span data-stu-id="41810-114">Creates a logger instance per category name.</span></span>
- <span data-ttu-id="41810-115">在 `IsEnabled` 中检查 `logLevel == _config.LogLevel`，因此每个 `logLevel` 都有一个唯一的记录器。</span><span class="sxs-lookup"><span data-stu-id="41810-115">Checks `logLevel == _config.LogLevel` in `IsEnabled`, so each `logLevel` has a unique logger.</span></span> <span data-ttu-id="41810-116">还应为所有更高的日志级别启用记录器：</span><span class="sxs-lookup"><span data-stu-id="41810-116">Loggers should also be enabled for all higher log levels:</span></span>

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLogger.cs" range="16-17":::

## <a name="custom-logger-provider"></a><span data-ttu-id="41810-117">自定义记录器提供程序</span><span class="sxs-lookup"><span data-stu-id="41810-117">Custom logger provider</span></span>

<span data-ttu-id="41810-118">`ILoggerProvider` 对象负责创建记录器实例。</span><span class="sxs-lookup"><span data-stu-id="41810-118">The `ILoggerProvider` object is responsible for creating logger instances.</span></span> <span data-ttu-id="41810-119">也许不需要为每个类别创建一个记录器实例，但这对于某些记录器（例如 NLog 或 log4net）来说是需要的。</span><span class="sxs-lookup"><span data-stu-id="41810-119">Maybe it is not needed to create a logger instance per category, but this makes sense for some loggers, like NLog or log4net.</span></span> <span data-ttu-id="41810-120">这样，你还可以按需为每个类别选择不同的日志记录输出目标：</span><span class="sxs-lookup"><span data-stu-id="41810-120">Doing this you are also able to choose different logging output targets per category if needed:</span></span>

:::code language="csharp" source="snippets/configuration/console-custom-logging/ColorConsoleLoggerProvider.cs":::

<span data-ttu-id="41810-121">在前面的代码中，<xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger%2A> 会为每个类别名称创建一个 `ColorConsoleLogger` 实例并将其存储在 [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2) 中。</span><span class="sxs-lookup"><span data-stu-id="41810-121">In the preceding code, <xref:Microsoft.Build.Logging.LoggerDescription.CreateLogger%2A> creates a single instance of the `ColorConsoleLogger` per category name and stores it in the [`ConcurrentDictionary<TKey,TValue>`](/dotnet/api/system.collections.concurrent.concurrentdictionary-2).</span></span>

## <a name="usage-and-registration-of-the-custom-logger"></a><span data-ttu-id="41810-122">自定义记录器的使用和注册</span><span class="sxs-lookup"><span data-stu-id="41810-122">Usage and registration of the custom logger</span></span>

<span data-ttu-id="41810-123">若要添加自定义日志记录提供程序和相应的记录器，请从 <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging(Microsoft.Extensions.Hosting.IHostBuilder,System.Action{Microsoft.Extensions.Logging.ILoggingBuilder})?displayProperty=nameWithType> 使用 <xref:Microsoft.Extensions.Logging.ILoggingBuilder> 添加 <xref:Microsoft.Extensions.Logging.ILoggerProvider>：</span><span class="sxs-lookup"><span data-stu-id="41810-123">To add the custom logging provider and corresponding logger, add an <xref:Microsoft.Extensions.Logging.ILoggerProvider> with <xref:Microsoft.Extensions.Logging.ILoggingBuilder> from the <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging(Microsoft.Extensions.Hosting.IHostBuilder,System.Action{Microsoft.Extensions.Logging.ILoggingBuilder})?displayProperty=nameWithType>:</span></span>

:::code language="csharp" source="snippets/configuration/console-custom-logging/Program.cs" range="23-39":::

<span data-ttu-id="41810-124">`ILoggingBuilder` 创建一个或多个 `ILogger` 实例。</span><span class="sxs-lookup"><span data-stu-id="41810-124">The `ILoggingBuilder` creates one or more `ILogger` instances.</span></span> <span data-ttu-id="41810-125">框架使用 `ILogger` 实例记录信息。</span><span class="sxs-lookup"><span data-stu-id="41810-125">The `ILogger` instances are used by the framework to log the information.</span></span>

<span data-ttu-id="41810-126">对于前面的代码，为 `ILoggerFactory` 提供至少一个扩展方法：</span><span class="sxs-lookup"><span data-stu-id="41810-126">For the preceding code, provide at least one extension method for the `ILoggerFactory`:</span></span>

:::code language="csharp" source="snippets/configuration/console-custom-logging/Extensions/ColorConsoleLoggerExtensions.cs":::

<span data-ttu-id="41810-127">运行这个简单的应用程序将显示如下的控制台窗口：</span><span class="sxs-lookup"><span data-stu-id="41810-127">Running this simple application will render similar to the following console window:</span></span>

:::image type="content" source="media/color-console-logger.png" alt-text="为控制台日志器示例输出着色":::

## <a name="see-also"></a><span data-ttu-id="41810-129">另请参阅</span><span class="sxs-lookup"><span data-stu-id="41810-129">See also</span></span>

- <span data-ttu-id="41810-130">[.NET 中的日志记录](logging.md)。</span><span class="sxs-lookup"><span data-stu-id="41810-130">[Logging in .NET](logging.md).</span></span>
- <span data-ttu-id="41810-131">[.NET 中的日志记录提供程序](logging-providers.md)。</span><span class="sxs-lookup"><span data-stu-id="41810-131">[Logging providers in .NET](logging-providers.md).</span></span>
- <span data-ttu-id="41810-132">[.NET 中的高性能日志记录](high-performance-logging.md)。</span><span class="sxs-lookup"><span data-stu-id="41810-132">[High-performance logging in .NET](high-performance-logging.md).</span></span>
