---
title: .NET 中的高性能日志记录
author: IEvangelist
description: 了解如何使用 LoggerMessage 创建可缓存的委托。对于高性能日志记录方案，这些委托需要更少的对象分配。
ms.author: dapine
ms.date: 01/04/2021
ms.openlocfilehash: 0031ff7a9f70cb0cf724fdf6dfa4fbe0a44af7c1
ms.sourcegitcommit: 5d9cee27d9ffe8f5670e5f663434511e81b8ac38
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "102401972"
---
# <a name="high-performance-logging-in-net"></a><span data-ttu-id="1611c-103">.NET 中的高性能日志记录</span><span class="sxs-lookup"><span data-stu-id="1611c-103">High-performance logging in .NET</span></span>

<span data-ttu-id="1611c-104"><xref:Microsoft.Extensions.Logging.LoggerMessage> 类公开了用于创建可缓存委托的功能，该功能比[记录器扩展方法](xref:Microsoft.Extensions.Logging.LoggerExtensions)（例如 <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation%2A> 和 <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug%2A>）需要的对象分配和计算开销要少。</span><span class="sxs-lookup"><span data-stu-id="1611c-104">The <xref:Microsoft.Extensions.Logging.LoggerMessage> class exposes functionality to create cacheable delegates that require fewer object allocations and reduced computational overhead compared to [logger extension methods](xref:Microsoft.Extensions.Logging.LoggerExtensions), such as <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogInformation%2A> and <xref:Microsoft.Extensions.Logging.LoggerExtensions.LogDebug%2A>.</span></span> <span data-ttu-id="1611c-105">对于高性能日志记录方案，请使用 <xref:Microsoft.Extensions.Logging.LoggerMessage> 模式。</span><span class="sxs-lookup"><span data-stu-id="1611c-105">For high-performance logging scenarios, use the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

<span data-ttu-id="1611c-106">与记录器扩展方法相比，<xref:Microsoft.Extensions.Logging.LoggerMessage> 具有以下性能优势：</span><span class="sxs-lookup"><span data-stu-id="1611c-106"><xref:Microsoft.Extensions.Logging.LoggerMessage> provides the following performance advantages over Logger extension methods:</span></span>

- <span data-ttu-id="1611c-107">记录器扩展方法需要将值类型（例如 `int`）“装箱”（转换）到 `object` 中。</span><span class="sxs-lookup"><span data-stu-id="1611c-107">Logger extension methods require "boxing" (converting) value types, such as `int`, into `object`.</span></span> <span data-ttu-id="1611c-108"><xref:Microsoft.Extensions.Logging.LoggerMessage> 模式使用带强类型参数的静态 <xref:System.Action> 字段和扩展方法来避免装箱。</span><span class="sxs-lookup"><span data-stu-id="1611c-108">The <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern avoids boxing by using static <xref:System.Action> fields and extension methods with strongly-typed parameters.</span></span>
- <span data-ttu-id="1611c-109">记录器扩展方法每次写入日志消息时必须分析消息模板（命名的格式字符串）。</span><span class="sxs-lookup"><span data-stu-id="1611c-109">Logger extension methods must parse the message template (named format string) every time a log message is written.</span></span> <span data-ttu-id="1611c-110">如果已定义消息，那么 <xref:Microsoft.Extensions.Logging.LoggerMessage> 只需分析一次模板即可。</span><span class="sxs-lookup"><span data-stu-id="1611c-110"><xref:Microsoft.Extensions.Logging.LoggerMessage> only requires parsing a template once when the message is defined.</span></span>

<span data-ttu-id="1611c-111">示例应用展示了具有优先级队列处理辅助角色服务的 <xref:Microsoft.Extensions.Logging.LoggerMessage> 功能。</span><span class="sxs-lookup"><span data-stu-id="1611c-111">The sample app demonstrates <xref:Microsoft.Extensions.Logging.LoggerMessage> features with a priority queue processing worker service.</span></span> <span data-ttu-id="1611c-112">应用按优先级顺序处理工作项。</span><span class="sxs-lookup"><span data-stu-id="1611c-112">The app processes work items in priority order.</span></span> <span data-ttu-id="1611c-113">发生这些操作时，通过 <xref:Microsoft.Extensions.Logging.LoggerMessage> 模式生成日志消息。</span><span class="sxs-lookup"><span data-stu-id="1611c-113">As these operations occur, log messages are generated using the <xref:Microsoft.Extensions.Logging.LoggerMessage> pattern.</span></span>

## <a name="define-a-logger-message"></a><span data-ttu-id="1611c-114">定义记录器消息</span><span class="sxs-lookup"><span data-stu-id="1611c-114">Define a logger message</span></span>

<span data-ttu-id="1611c-115">使用 [Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A) 创建一个用于记录消息的 <xref:System.Action> 委托。</span><span class="sxs-lookup"><span data-stu-id="1611c-115">Use [Define(LogLevel, EventId, String)](xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A) to create an <xref:System.Action> delegate for logging a message.</span></span> <span data-ttu-id="1611c-116"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A> 重载允许向命名的格式字符串（模板）传递最多六个类型参数。</span><span class="sxs-lookup"><span data-stu-id="1611c-116"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A> overloads permit passing up to six type parameters to a named format string (template).</span></span>

<span data-ttu-id="1611c-117">提供给 <xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A> 方法的字符串是一个模板，而不是内插字符串。</span><span class="sxs-lookup"><span data-stu-id="1611c-117">The string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A> method is a template and not an interpolated string.</span></span> <span data-ttu-id="1611c-118">占位符按照指定类型的顺序填充。</span><span class="sxs-lookup"><span data-stu-id="1611c-118">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="1611c-119">模板中的占位符名称在各个模板中应当具备描述性和一致性。</span><span class="sxs-lookup"><span data-stu-id="1611c-119">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="1611c-120">它们在结构化的日志数据中充当属性名称。</span><span class="sxs-lookup"><span data-stu-id="1611c-120">They serve as property names within structured log data.</span></span> <span data-ttu-id="1611c-121">对于占位符名称，建议使用[帕斯卡拼写法](../../standard/design-guidelines/capitalization-conventions.md)。</span><span class="sxs-lookup"><span data-stu-id="1611c-121">We recommend [Pascal casing](../../standard/design-guidelines/capitalization-conventions.md) for placeholder names.</span></span> <span data-ttu-id="1611c-122">例如：`{Item}`、`{DateTime}`。</span><span class="sxs-lookup"><span data-stu-id="1611c-122">For example, `{Item}`, `{DateTime}`.</span></span>

<span data-ttu-id="1611c-123">每条日志消息都是一个 <xref:System.Action>，保存在由 [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A) 创建的静态字段中。</span><span class="sxs-lookup"><span data-stu-id="1611c-123">Each log message is an <xref:System.Action> held in a static field created by [LoggerMessage.Define](xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A).</span></span> <span data-ttu-id="1611c-124">例如，示例应用创建一个字段来描述处理工作项的日志消息：</span><span class="sxs-lookup"><span data-stu-id="1611c-124">For example, the sample app creates a field to describe a log message for the processing of work items:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="FailedProcessingField":::

<span data-ttu-id="1611c-125">对于 <xref:System.Action>，指定：</span><span class="sxs-lookup"><span data-stu-id="1611c-125">For the <xref:System.Action>, specify:</span></span>

- <span data-ttu-id="1611c-126">日志级别。</span><span class="sxs-lookup"><span data-stu-id="1611c-126">The log level.</span></span>
- <span data-ttu-id="1611c-127">具有静态扩展方法名称的唯一事件标识符 (<xref:Microsoft.Extensions.Logging.EventId>)。</span><span class="sxs-lookup"><span data-stu-id="1611c-127">A unique event identifier (<xref:Microsoft.Extensions.Logging.EventId>) with the name of the static extension method.</span></span>
- <span data-ttu-id="1611c-128">消息模板（命名的格式字符串）。</span><span class="sxs-lookup"><span data-stu-id="1611c-128">The message template (named format string).</span></span>

<span data-ttu-id="1611c-129">由于对工作项处理取消排队，辅助角色服务应用会进行下列设置：</span><span class="sxs-lookup"><span data-stu-id="1611c-129">As work items are dequeued for processing the worker service app sets the:</span></span>

- <span data-ttu-id="1611c-130">将日志级别设置为 <xref:Microsoft.Extensions.Logging.LogLevel.Critical?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="1611c-130">Log level to <xref:Microsoft.Extensions.Logging.LogLevel.Critical?displayProperty=nameWithType>.</span></span>
- <span data-ttu-id="1611c-131">将事件 ID 设置为具有 `FailedToProcessWorkItem` 方法名称的 `13`。</span><span class="sxs-lookup"><span data-stu-id="1611c-131">Event id to `13` with the name of the `FailedToProcessWorkItem` method.</span></span>
- <span data-ttu-id="1611c-132">将消息模板（命名的格式字符串）设置为字符串。</span><span class="sxs-lookup"><span data-stu-id="1611c-132">Message template (named format string) to a string.</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="FailedProcessingAssignment":::

<span data-ttu-id="1611c-133">结构化日志记录存储可以使用事件名称（当它获得事件 ID 时）来丰富日志记录。</span><span class="sxs-lookup"><span data-stu-id="1611c-133">Structured logging stores may use the event name when it's supplied with the event id to enrich logging.</span></span> <span data-ttu-id="1611c-134">例如，[Serilog](https://github.com/serilog/serilog-extensions-logging) 使用该事件名称。</span><span class="sxs-lookup"><span data-stu-id="1611c-134">For example, [Serilog](https://github.com/serilog/serilog-extensions-logging) uses the event name.</span></span>

<span data-ttu-id="1611c-135">通过强类型扩展方法调用 <xref:System.Action>。</span><span class="sxs-lookup"><span data-stu-id="1611c-135">The <xref:System.Action> is invoked through a strongly-typed extension method.</span></span> <span data-ttu-id="1611c-136">`PriorityItemProcessed` 方法会在每次处理工作项时记录一条消息。</span><span class="sxs-lookup"><span data-stu-id="1611c-136">The `PriorityItemProcessed` method logs a message every time a work item is being processed.</span></span> <span data-ttu-id="1611c-137">而在（如果）发生异常时则调用 `FailedToProcessWorkItem`：</span><span class="sxs-lookup"><span data-stu-id="1611c-137">Whereas, `FailedToProcessWorkItem` is called when (and if) an exception occurs:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Worker.cs" range="18-39" highlight="15-18":::

<span data-ttu-id="1611c-138">检查应用的控制台输出：</span><span class="sxs-lookup"><span data-stu-id="1611c-138">Inspect the app's console output:</span></span>

```console
crit: WorkerServiceOptions.Example.Worker[13]
      Epic failure processing item!
      System.Exception: Failed to verify communications.
         at WorkerServiceOptions.Example.Worker.ExecuteAsync(CancellationToken stoppingToken) in
         ..\Worker.cs:line 27
```

<span data-ttu-id="1611c-139">要将参数传递给日志消息，创建静态字段时最多定义六种类型。</span><span class="sxs-lookup"><span data-stu-id="1611c-139">To pass parameters to a log message, define up to six types when creating the static field.</span></span> <span data-ttu-id="1611c-140">在通过定义 <xref:System.Action> 字段的 `WorkItem` 类型来处理项时，示例应用会记录工作项的详细信息：</span><span class="sxs-lookup"><span data-stu-id="1611c-140">The sample app logs the work item details when processing items by defining a `WorkItem` type for the <xref:System.Action> field:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="ProcessingItemField":::

<span data-ttu-id="1611c-141">委托的日志消息模板从提供的类型接收其占位符值。</span><span class="sxs-lookup"><span data-stu-id="1611c-141">The delegate's log message template receives its placeholder values from the types provided.</span></span> <span data-ttu-id="1611c-142">示例应用定义一个委托，用于在 item 参数是 `WorkItem` 的位置添加一个工作项：</span><span class="sxs-lookup"><span data-stu-id="1611c-142">The sample app defines a delegate for adding a work item where the item parameter is a `WorkItem`:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="ProcessingItemAssignment":::

<span data-ttu-id="1611c-143">用于记录正在处理工作项的静态扩展方法 `PriorityItemProcessed` 接收工作项参数值并将其传递给 <xref:System.Action> 委托：</span><span class="sxs-lookup"><span data-stu-id="1611c-143">The static extension method for logging that a work item is being processed, `PriorityItemProcessed`, receives the work item argument value and passes it to the <xref:System.Action> delegate:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="ProcessingItemMethod":::

<span data-ttu-id="1611c-144">在辅助角色服务的 `ExecuteAsync` 方法中，调用 `PriorityItemProcessed` 以记录消息：</span><span class="sxs-lookup"><span data-stu-id="1611c-144">In the worker service's `ExecuteAsync` method, `PriorityItemProcessed` is called to log the message:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Worker.cs" range="18-39" highlight="12":::

<span data-ttu-id="1611c-145">检查应用的控制台输出：</span><span class="sxs-lookup"><span data-stu-id="1611c-145">Inspect the app's console output:</span></span>

```console
info: WorkerServiceOptions.Example.Worker[1]
      Processing priority item: Priority-Extreme (50db062a-9732-4418-936d-110549ad79e4): 'Verify communications'
```

## <a name="define-logger-message-scope"></a><span data-ttu-id="1611c-146">指定记录器消息作用域</span><span class="sxs-lookup"><span data-stu-id="1611c-146">Define logger message scope</span></span>

<span data-ttu-id="1611c-147">[DefineScope(string)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A) 方法创建一个用于定义[日志作用域](logging.md#log-scopes)的 <xref:System.Func%601> 委托。</span><span class="sxs-lookup"><span data-stu-id="1611c-147">The [DefineScope(string)](xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A) method creates a <xref:System.Func%601> delegate for defining a [log scope](logging.md#log-scopes).</span></span> <span data-ttu-id="1611c-148"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> 重载允许向命名的格式字符串（模板）传递最多三个类型参数。</span><span class="sxs-lookup"><span data-stu-id="1611c-148"><xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> overloads permit passing up to three type parameters to a named format string (template).</span></span>

<span data-ttu-id="1611c-149"><xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A> 方法也一样，提供给 <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> 方法的字符串是一个模板，而不是内插字符串。</span><span class="sxs-lookup"><span data-stu-id="1611c-149">As is the case with the <xref:Microsoft.Extensions.Logging.LoggerMessage.Define%2A> method, the string provided to the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> method is a template and not an interpolated string.</span></span> <span data-ttu-id="1611c-150">占位符按照指定类型的顺序填充。</span><span class="sxs-lookup"><span data-stu-id="1611c-150">Placeholders are filled in the order that the types are specified.</span></span> <span data-ttu-id="1611c-151">模板中的占位符名称在各个模板中应当具备描述性和一致性。</span><span class="sxs-lookup"><span data-stu-id="1611c-151">Placeholder names in the template should be descriptive and consistent across templates.</span></span> <span data-ttu-id="1611c-152">它们在结构化的日志数据中充当属性名称。</span><span class="sxs-lookup"><span data-stu-id="1611c-152">They serve as property names within structured log data.</span></span> <span data-ttu-id="1611c-153">对于占位符名称，建议使用[帕斯卡拼写法](../../standard/design-guidelines/capitalization-conventions.md)。</span><span class="sxs-lookup"><span data-stu-id="1611c-153">We recommend [Pascal casing](../../standard/design-guidelines/capitalization-conventions.md) for placeholder names.</span></span> <span data-ttu-id="1611c-154">例如：`{Item}`、`{DateTime}`。</span><span class="sxs-lookup"><span data-stu-id="1611c-154">For example, `{Item}`, `{DateTime}`.</span></span>

<span data-ttu-id="1611c-155">使用 <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> 方法定义一个[日志作用域](logging.md#log-scopes)，以应用到一系列日志消息中。</span><span class="sxs-lookup"><span data-stu-id="1611c-155">Define a [log scope](logging.md#log-scopes) to apply to a series of log messages using the <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> method.</span></span> <span data-ttu-id="1611c-156">在 appsettings.json 的控制台记录器部分启用 `IncludeScopes`：</span><span class="sxs-lookup"><span data-stu-id="1611c-156">Enable `IncludeScopes` in the console logger section of *appsettings.json*:</span></span>

:::code language="json" source="snippets/configuration/worker-service-options/appsettings.json" highlight="3-5":::

<span data-ttu-id="1611c-157">要创建日志作用域，请添加一个字段来保存该作用域的 <xref:System.Func%601> 委托。</span><span class="sxs-lookup"><span data-stu-id="1611c-157">To create a log scope, add a field to hold a <xref:System.Func%601> delegate for the scope.</span></span> <span data-ttu-id="1611c-158">示例应用创建一个名为 `_processingWorkScope` (Internal/LoggerExtensions.cs) 的字段：</span><span class="sxs-lookup"><span data-stu-id="1611c-158">The sample app creates a field called `_processingWorkScope` (*Internal/LoggerExtensions.cs*):</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="ProcessingWorkField":::

<span data-ttu-id="1611c-159">使用 <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> 来创建委托。</span><span class="sxs-lookup"><span data-stu-id="1611c-159">Use <xref:Microsoft.Extensions.Logging.LoggerMessage.DefineScope%2A> to create the delegate.</span></span> <span data-ttu-id="1611c-160">调用委托时最多可以指定三种类型作为模板参数使用。</span><span class="sxs-lookup"><span data-stu-id="1611c-160">Up to three types can be specified for use as template arguments when the delegate is invoked.</span></span> <span data-ttu-id="1611c-161">示例应用使用包含处理开始的日期时间的消息模板：</span><span class="sxs-lookup"><span data-stu-id="1611c-161">The sample app uses a message template that includes the date time in which processing started:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="ProcessingWorkAssignment":::

<span data-ttu-id="1611c-162">为日志消息提供一种静态扩展方法。</span><span class="sxs-lookup"><span data-stu-id="1611c-162">Provide a static extension method for the log message.</span></span> <span data-ttu-id="1611c-163">包含已命名属性的任何类型参数（这些参数出现在消息模板中）。</span><span class="sxs-lookup"><span data-stu-id="1611c-163">Include any type parameters for named properties that appear in the message template.</span></span> <span data-ttu-id="1611c-164">示例应用接受自定义时间戳的 `DateTime` 以记录和返回 `_processingWorkScope`：</span><span class="sxs-lookup"><span data-stu-id="1611c-164">The sample app takes in a `DateTime` for a custom time stamp to log and returns `_processingWorkScope`:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Extensions/LoggerExtensions.cs" id="ProcessingWorkMethod":::

<span data-ttu-id="1611c-165">该作用域将日志记录扩展调用包装在 [using](../../csharp/language-reference/keywords/using-statement.md) 块中：</span><span class="sxs-lookup"><span data-stu-id="1611c-165">The scope wraps the logging extension calls in a [using](../../csharp/language-reference/keywords/using-statement.md) block:</span></span>

:::code language="csharp" source="snippets/configuration/worker-service-options/Worker.cs" range="18-39" highlight="4":::

<span data-ttu-id="1611c-166">检查应用控制台输出中的日志消息。</span><span class="sxs-lookup"><span data-stu-id="1611c-166">Inspect the log messages in the app's console output.</span></span> <span data-ttu-id="1611c-167">以下结果显示日志消息的优先级排序，其中包括日志作用域消息：</span><span class="sxs-lookup"><span data-stu-id="1611c-167">The following result shows priority ordering of log messages with the log scope message included:</span></span>

```console
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-Extreme (f5090ede-a337-4041-b914-f6bc0db5ae64): 'Verify communications'
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
info: Microsoft.Hosting.Lifetime[0]
      Content root path: ..\worker-service-options
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-High (496d440f-2007-4391-b179-09d75ab52373): 'Validate collection'
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-Medium (dea9e3f4-d7df-46d2-b7cd-5e0232eb98a5): 'Propagate selections'
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-Medium (089d7f0d-da72-4b55-92fe-57b147838056): 'Enter pooling [contention]'
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-Low (6e68c4be-089f-4450-9080-1ea63fcbb686): 'Health check network'
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-Deferred (6f324134-6bb6-455f-81d4-553ab307c421): 'Ping weather service'
info: WorkerServiceOptions.Example.Worker[1]
      => Processing work, started at: 09/25/2020 14:30:45
      Processing priority item: Priority-Deferred (37bf736c-7a26-4a2a-9e56-e89bcf3b8f35): 'Set process state'
```

## <a name="see-also"></a><span data-ttu-id="1611c-168">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1611c-168">See also</span></span>

- [<span data-ttu-id="1611c-169">.NET 中的日志记录</span><span class="sxs-lookup"><span data-stu-id="1611c-169">Logging in .NET</span></span>](logging.md)
