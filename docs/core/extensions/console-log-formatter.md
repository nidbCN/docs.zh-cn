---
title: 控制台日志格式设置
description: 了解如何使用可用的控制台日志格式设置，或为 .NET 应用程序实现自定义日志格式设置。
author: IEvangelist
ms.author: dapine
ms.date: 12/17/2020
ms.openlocfilehash: 0ec8fc2018febe4273aa646d1682be197933f925
ms.sourcegitcommit: 3d6d6595a03915f617349781f455f838a44b0f44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2020
ms.locfileid: "102401953"
---
# <a name="console-log-formatting"></a><span data-ttu-id="3f497-103">控制台日志格式设置</span><span class="sxs-lookup"><span data-stu-id="3f497-103">Console log formatting</span></span>

<span data-ttu-id="3f497-104">在 .NET 5 中，已向 `Microsoft.Extensions.Logging.Console` 命名空间中的控制台日志添加了对自定义格式设置的支持。</span><span class="sxs-lookup"><span data-stu-id="3f497-104">In .NET 5, support for custom formatting was added to console logs in the `Microsoft.Extensions.Logging.Console` namespace.</span></span> <span data-ttu-id="3f497-105">有三种预定义的格式设置选项可供选择：[`Simple`](#simple)、[`Systemd`](#systemd) 和 [`Json`](#json)。</span><span class="sxs-lookup"><span data-stu-id="3f497-105">There are three predefined formatting options available: [`Simple`](#simple), [`Systemd`](#systemd), and [`Json`](#json).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3f497-106">以前，<xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat> 枚举允许选择所需的日志格式，可以是人工可读取的 (`Default`)，也可以是单行（也称为 `Systemd`）。</span><span class="sxs-lookup"><span data-stu-id="3f497-106">Previously, the <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat> enum allowed for selecting the desired log format, either human readable which was the `Default`, or single line which is also known as `Systemd`.</span></span> <span data-ttu-id="3f497-107">不过，这些都是不能进行自定义的，现已弃用。</span><span class="sxs-lookup"><span data-stu-id="3f497-107">However, these were **not** customizable, and are now deprecated.</span></span>

<span data-ttu-id="3f497-108">本文介绍了控制台日志格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3f497-108">In this article, you will learn about console log formatters.</span></span> <span data-ttu-id="3f497-109">示例源代码演示了如何执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="3f497-109">The sample source code demonstrates how to:</span></span>

- <span data-ttu-id="3f497-110">注册新的格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-110">Register a new formatter</span></span>
- <span data-ttu-id="3f497-111">选择要使用的已注册格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-111">Select a registered formatter to use</span></span>
  - <span data-ttu-id="3f497-112">通过代码或[配置](configuration.md)</span><span class="sxs-lookup"><span data-stu-id="3f497-112">Either through code, or [configuration](configuration.md)</span></span>
- <span data-ttu-id="3f497-113">实现自定义格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-113">Implement a custom formatter</span></span>
  - <span data-ttu-id="3f497-114">通过 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 更新配置</span><span class="sxs-lookup"><span data-stu-id="3f497-114">Update configuration via <xref:Microsoft.Extensions.Options.IOptionsMonitor%601></span></span>
  - <span data-ttu-id="3f497-115">启用自定义颜色格式设置</span><span class="sxs-lookup"><span data-stu-id="3f497-115">Enable custom color formatting</span></span>

## <a name="register-formatter"></a><span data-ttu-id="3f497-116">注册格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-116">Register formatter</span></span>

<span data-ttu-id="3f497-117">[`Console` 日志记录提供程序](logging-providers.md#console)有几个预定义的格式化程序，并公开了可供你编写自己的自定义格式化程序的功能。</span><span class="sxs-lookup"><span data-stu-id="3f497-117">The [`Console` logging provider](logging-providers.md#console) has several predefined formatters, and exposes the ability to author your own custom formatter.</span></span> <span data-ttu-id="3f497-118">要注册任何可用的格式化程序，请使用相应的 `Add{Type}Console` 扩展方法：</span><span class="sxs-lookup"><span data-stu-id="3f497-118">To register any of the available formatters, use the corresponding `Add{Type}Console` extension method:</span></span>

| <span data-ttu-id="3f497-119">可用类型</span><span class="sxs-lookup"><span data-stu-id="3f497-119">Available types</span></span> | <span data-ttu-id="3f497-120">注册类型的方法</span><span class="sxs-lookup"><span data-stu-id="3f497-120">Method to register type</span></span> |
|--|--|
| <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Json?displayProperty=nameWithType> | <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddJsonConsole%2A?displayProperty=nameWithType> |
| <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType> | <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddSimpleConsole%2A?displayProperty=nameWithType> |
| <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType> | <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddSystemdConsole%2A?displayProperty=nameWithType> |

### <a name="simple"></a><span data-ttu-id="3f497-121">简单</span><span class="sxs-lookup"><span data-stu-id="3f497-121">Simple</span></span>

<span data-ttu-id="3f497-122">要使用 `Simple` 控制台格式化程序，请将其注册到 `AddSimpleConsole`：</span><span class="sxs-lookup"><span data-stu-id="3f497-122">To use the `Simple` console formatter, register it with `AddSimpleConsole`:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-simple/Program.cs" highlight="11-16":::

<span data-ttu-id="3f497-123">在前面的示例源代码中，已经注册了 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType> 格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3f497-123">In the preceding sample source code, the <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType> formatter was registered.</span></span> <span data-ttu-id="3f497-124">它不仅为日志提供了在每条日志消息中包装信息（如时间和日志级别）的功能，而且还允许 ANSI 颜色嵌入和消息缩进。</span><span class="sxs-lookup"><span data-stu-id="3f497-124">It provides logs with the ability to not only wrap information such as time and log level in each log message, but also allows for ANSI color embedding and indentation of messages.</span></span>

### <a name="systemd"></a><span data-ttu-id="3f497-125">Systemd</span><span class="sxs-lookup"><span data-stu-id="3f497-125">Systemd</span></span>

<span data-ttu-id="3f497-126"><xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType> 控制台记录器：</span><span class="sxs-lookup"><span data-stu-id="3f497-126">The <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType> console logger:</span></span>

- <span data-ttu-id="3f497-127">使用“Syslog”日志级别格式和严重性</span><span class="sxs-lookup"><span data-stu-id="3f497-127">Uses the "Syslog" log level format and severities</span></span>
- <span data-ttu-id="3f497-128">不为消息设置颜色格式</span><span class="sxs-lookup"><span data-stu-id="3f497-128">Does **not** format messages with colors</span></span>
- <span data-ttu-id="3f497-129">始终将消息记录在单行中</span><span class="sxs-lookup"><span data-stu-id="3f497-129">Always logs messages in a single line</span></span>

<span data-ttu-id="3f497-130">这对于容器来说通常很有用，因为容器经常使用 `Systemd` 控制台日志记录。</span><span class="sxs-lookup"><span data-stu-id="3f497-130">This is commonly useful for containers, which often make use of `Systemd` console logging.</span></span> <span data-ttu-id="3f497-131">在 .NET 5 中，`Simple` 控制台记录器还可以实现以单行方式进行记录的紧凑版本，并且还允许禁用颜色，如前面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="3f497-131">With .NET 5, the `Simple` console logger also enables a compact version that logs in a single line, and also allows for disabling colors as shown in an earlier sample.</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-systemd/Program.cs" highlight="11-15":::

### <a name="json"></a><span data-ttu-id="3f497-132">Json</span><span class="sxs-lookup"><span data-stu-id="3f497-132">Json</span></span>

<span data-ttu-id="3f497-133">要以 JSON 格式编写日志，需要使用 `Json` 控制台格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3f497-133">To write logs in a JSON format, the `Json` console formatter is used.</span></span> <span data-ttu-id="3f497-134">示例源代码展示了 ASP.NET Core 应用如何注册格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3f497-134">The sample source code shows how an ASP.NET Core app might register it.</span></span> <span data-ttu-id="3f497-135">使用 `webapp` 模板，用 [dotnet new](../tools/dotnet-new.md) 命令创建一个新的 ASP.NET Core 应用：</span><span class="sxs-lookup"><span data-stu-id="3f497-135">Using the `webapp` template, create a new ASP.NET Core app with the [dotnet new](../tools/dotnet-new.md) command:</span></span>

```dotnetcli
dotnet new webapp -o Console.ExampleFormatters.Json
```

<span data-ttu-id="3f497-136">运行此应用时，使用模板代码会获得以下默认日志格式：</span><span class="sxs-lookup"><span data-stu-id="3f497-136">When running the app, using the template code, you get the default log format below:</span></span>

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
```

<span data-ttu-id="3f497-137">默认情况下，默认配置中会选择 `Simple` 控制台日志格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3f497-137">By default, the `Simple` console log formatter is selected with default configuration.</span></span> <span data-ttu-id="3f497-138">可以通过在 Program.cs 中调用 `AddJsonConsole` 来更改此设置：</span><span class="sxs-lookup"><span data-stu-id="3f497-138">You change this by calling `AddJsonConsole` in the *Program.cs*:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-json/Program.cs" highlight="17-26":::

<span data-ttu-id="3f497-139">再次运行此应用，进行上述更改后，现在日志消息的格式为 JSON：</span><span class="sxs-lookup"><span data-stu-id="3f497-139">Run the app again, with the above change, the log message is now formatted as JSON:</span></span>

:::code language="json" source="snippets/logging/console-formatter-json/example-output.json":::

> [!TIP]
> <span data-ttu-id="3f497-140">默认情况下，`Json` 控制台格式化程序将每条消息记录在单行中。</span><span class="sxs-lookup"><span data-stu-id="3f497-140">The `Json` console formatter, by default, logs each message in a single line.</span></span> <span data-ttu-id="3f497-141">为了在配置格式化程序时使其更具可读性，请将 <xref:System.Text.Json.JsonWriterOptions.Indented?displayProperty=nameWithType> 设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="3f497-141">In order to make it more readable while configuring the formatter, set <xref:System.Text.Json.JsonWriterOptions.Indented?displayProperty=nameWithType> to `true`.</span></span>

## <a name="set-formatter-with-configuration"></a><span data-ttu-id="3f497-142">通过配置设置格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-142">Set formatter with configuration</span></span>

<span data-ttu-id="3f497-143">前面的示例展示了如何以编程方式注册格式化程序。</span><span class="sxs-lookup"><span data-stu-id="3f497-143">The previous samples have shown how to register a formatter programmatically.</span></span> <span data-ttu-id="3f497-144">另外，也可以通过[配置](configuration.md)来完成。</span><span class="sxs-lookup"><span data-stu-id="3f497-144">Alternatively, this can be done with [configuration](configuration.md).</span></span> <span data-ttu-id="3f497-145">考虑到之前的 Web 应用示例源代码，如果更新 appsettings.json 文件，而不是调用 Program.cs 文件中的 `ConfigureLogging`，可以得到相同的结果。</span><span class="sxs-lookup"><span data-stu-id="3f497-145">Consider the previous web application sample source code, if you update the *appsettings.json* file rather than calling `ConfigureLogging` in the *Program.cs* file, you could get the same outcome.</span></span> <span data-ttu-id="3f497-146">更新后的 `appsettings.json` 文件将对格式化程序进行如下配置：</span><span class="sxs-lookup"><span data-stu-id="3f497-146">The updated `appsettings.json` file would configure the formatter as follows:</span></span>

:::code language="json" source="snippets/logging/console-formatter-json/appsettings.json" highlight="14-23":::

<span data-ttu-id="3f497-147">需要设置的两个键值为 `"FormatterName"` 和 `"FormatterOptions"`。</span><span class="sxs-lookup"><span data-stu-id="3f497-147">The two key values that need to be set are `"FormatterName"` and `"FormatterOptions"`.</span></span> <span data-ttu-id="3f497-148">如果已经注册了值设置为 `"FormatterName"` 的格式化程序，则会选择此格式化程序，并且只要在 `"FormatterOptions"` 节点中将其属性作为键提供，就可以配置属性。</span><span class="sxs-lookup"><span data-stu-id="3f497-148">If a formatter with the value set for `"FormatterName"` is already registered, that formatter is selected, and its properties can be configured as long as they are provided as a key inside the `"FormatterOptions"` node.</span></span> <span data-ttu-id="3f497-149">预定义的格式化程序名称保留在 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames> 下：</span><span class="sxs-lookup"><span data-stu-id="3f497-149">The predefined formatter names are reserved under <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames>:</span></span>

- <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Json?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType>

## <a name="implement-a-custom-formatter"></a><span data-ttu-id="3f497-150">实现自定义格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-150">Implement a custom formatter</span></span>

<span data-ttu-id="3f497-151">要实现自定义格式化程序，需要执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="3f497-151">To implement a custom formatter, you need to:</span></span>

- <span data-ttu-id="3f497-152">创建一个 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatter> 的子类，这代表你的自定义格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-152">Create a subclass of <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatter>, this represents your custom formatter</span></span>
- <span data-ttu-id="3f497-153">注册以下自定义格式化程序</span><span class="sxs-lookup"><span data-stu-id="3f497-153">Register your custom formatter with</span></span>
  - <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddConsole%2A?displayProperty=nameWithType>
  - <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddConsoleFormatter%60%602(Microsoft.Extensions.Logging.ILoggingBuilder,System.Action{%60%601})?displayProperty=nameWithType>

<span data-ttu-id="3f497-154">创建一个扩展方法来为你进行处理：</span><span class="sxs-lookup"><span data-stu-id="3f497-154">Create an extension method to handle this for you:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/ConsoleLoggerExtensions.cs" highlight="11-12":::

<span data-ttu-id="3f497-155">`CustomOptions` 的定义如下：</span><span class="sxs-lookup"><span data-stu-id="3f497-155">The `CustomOptions` are defined as follows:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomOptions.cs":::

<span data-ttu-id="3f497-156">在前面的代码中，选项是 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterOptions> 的子类。</span><span class="sxs-lookup"><span data-stu-id="3f497-156">In the preceding code, the options are a subclass of <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterOptions>.</span></span>

<span data-ttu-id="3f497-157">`AddConsoleFormatter` API 会执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="3f497-157">The `AddConsoleFormatter` API:</span></span>

- <span data-ttu-id="3f497-158">注册 `ConsoleFormatter` 的子类</span><span class="sxs-lookup"><span data-stu-id="3f497-158">Registers a subclass of `ConsoleFormatter`</span></span>
- <span data-ttu-id="3f497-159">处理配置：</span><span class="sxs-lookup"><span data-stu-id="3f497-159">Handles configuration:</span></span>
  - <span data-ttu-id="3f497-160">基于[选项模式](options.md)和 [IOptionsMonitor](xref:Microsoft.Extensions.Options.IOptionsMonitor%601) 接口，使用更改令牌同步更新</span><span class="sxs-lookup"><span data-stu-id="3f497-160">Uses a change token to synchronize updates, based on the [options pattern](options.md), and the [IOptionsMonitor](xref:Microsoft.Extensions.Options.IOptionsMonitor%601) interface</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/Program.cs" highlight="11-12":::

<span data-ttu-id="3f497-161">定义 `ConsoleFormatter` 的 `CustomerFormatter` 子类：</span><span class="sxs-lookup"><span data-stu-id="3f497-161">Define a `CustomerFormatter` subclass of `ConsoleFormatter`:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomFormatter.cs" highlight="24-45":::

<span data-ttu-id="3f497-162">前面的 `CustomFormatter.Write<TState>` API 指明了每个日志消息包装的文本类型。</span><span class="sxs-lookup"><span data-stu-id="3f497-162">The preceding `CustomFormatter.Write<TState>` API dictates what text gets wrapped around each log message.</span></span> <span data-ttu-id="3f497-163">一个标准的 `ConsoleFormatter` 应至少能包装日志的范围、时间戳和严重性级别。</span><span class="sxs-lookup"><span data-stu-id="3f497-163">A standard `ConsoleFormatter` should be able to wrap around scopes, time stamps, and severity level of logs at a minimum.</span></span> <span data-ttu-id="3f497-164">此外，你还可以对日志消息中的 ANSI 颜色进行编码，并提供所需的缩进。</span><span class="sxs-lookup"><span data-stu-id="3f497-164">Additionally, you can encode ANSI colors in the log messages, and provide desired indentations as well.</span></span> <span data-ttu-id="3f497-165">`CustomFormatter.Write<TState>` 的实现缺少这些功能。</span><span class="sxs-lookup"><span data-stu-id="3f497-165">The implementation of the `CustomFormatter.Write<TState>` lacks these capabilities.</span></span>

<span data-ttu-id="3f497-166">有关进一步自定义格式设置的灵感，请参阅 `Microsoft.Extensions.Logging.Console` 命名空间中的现有实现：</span><span class="sxs-lookup"><span data-stu-id="3f497-166">For inspiration on further customizing formatting, see the existing implementations in the `Microsoft.Extensions.Logging.Console` namespace:</span></span>

- <span data-ttu-id="3f497-167">[SimpleConsoleFormatter](https://github.com/dotnet/runtime/blob/master/src/libraries/Microsoft.Extensions.Logging.Console/src/SimpleConsoleFormatter.cs)。</span><span class="sxs-lookup"><span data-stu-id="3f497-167">[SimpleConsoleFormatter](https://github.com/dotnet/runtime/blob/master/src/libraries/Microsoft.Extensions.Logging.Console/src/SimpleConsoleFormatter.cs).</span></span>
- [<span data-ttu-id="3f497-168">SystemdConsoleFormatter</span><span class="sxs-lookup"><span data-stu-id="3f497-168">SystemdConsoleFormatter</span></span>](https://github.com/dotnet/runtime/blob/master/src/libraries/Microsoft.Extensions.Logging.Console/src/SystemdConsoleFormatter.cs)
- [<span data-ttu-id="3f497-169">JsonConsoleFormatter</span><span class="sxs-lookup"><span data-stu-id="3f497-169">JsonConsoleFormatter</span></span>](https://github.com/dotnet/runtime/blob/master/src/libraries/Microsoft.Extensions.Logging.Console/src/JsonConsoleFormatter.cs)

## <a name="implement-custom-color-formatting"></a><span data-ttu-id="3f497-170">实现自定义颜色格式设置</span><span class="sxs-lookup"><span data-stu-id="3f497-170">Implement custom color formatting</span></span>

<span data-ttu-id="3f497-171">为了在自定义日志记录格式化程序中正确地启用颜色功能，可以扩展 <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions>，因为它具有 <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions.ColorBehavior?displayProperty=nameWithType> 属性，这个属性对于在日志中启用颜色非常有用。</span><span class="sxs-lookup"><span data-stu-id="3f497-171">In order to properly enable color capabilities in your custom logging formatter, you can extend the <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions> as it has a <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions.ColorBehavior?displayProperty=nameWithType> property that can be useful for enabling colors in logs.</span></span>

<span data-ttu-id="3f497-172">创建一个从 `SimpleConsoleFormatterOptions` 派生的 `CustomColorOptions`：</span><span class="sxs-lookup"><span data-stu-id="3f497-172">Create a `CustomColorOptions` that derives from `SimpleConsoleFormatterOptions`:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomColorOptions.cs" highlight="5":::

<span data-ttu-id="3f497-173">接下来，在 `TextWriterExtensions` 类中编写一些扩展方法，以便在经过格式设置的日志消息中方便地嵌入 ANSI 编码的颜色：</span><span class="sxs-lookup"><span data-stu-id="3f497-173">Next, write some extension methods in a `TextWriterExtensions` class that allow for conveniently embedding ANSI coded colors within formatted log messages:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/TextWriterExtensions.cs":::

<span data-ttu-id="3f497-174">可以定义一个处理应用自定义颜色的自定义颜色格式化程序，如下所示：</span><span class="sxs-lookup"><span data-stu-id="3f497-174">A custom color formatter that handles applying custom colors could be defined as follows:</span></span>

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomColorFormatter.cs" highlight="15-18,52-65":::

<span data-ttu-id="3f497-175">当你运行应用程序时，日志会在 `FormatterOptions.ColorBehavior` 为 `Enabled` 时用绿色显示 `CustomPrefix` 消息。</span><span class="sxs-lookup"><span data-stu-id="3f497-175">When you run the application, the logs will show the `CustomPrefix` message in the color green when `FormatterOptions.ColorBehavior` is `Enabled`.</span></span>

> [!NOTE]
> <span data-ttu-id="3f497-176">当 <xref:Microsoft.Extensions.Logging.Console.LoggerColorBehavior> 为 `Disabled` 时，日志消息不会解释日志消息中嵌入的 ANSI 颜色代码。</span><span class="sxs-lookup"><span data-stu-id="3f497-176">When <xref:Microsoft.Extensions.Logging.Console.LoggerColorBehavior> is `Disabled`, log messages do _not_ interpret embedded ANSI color codes within log messages.</span></span> <span data-ttu-id="3f497-177">相反，它们会输出原始消息。</span><span class="sxs-lookup"><span data-stu-id="3f497-177">Instead, they output the raw message.</span></span> <span data-ttu-id="3f497-178">例如，请考虑如下事项：</span><span class="sxs-lookup"><span data-stu-id="3f497-178">For example, consider the following:</span></span>
>
> ```csharp
> logger.LogInformation("Random log \x1B[42mwith green background\x1B[49m message");
> ```
>
> <span data-ttu-id="3f497-179">这会输出逐字字符串，且不会着色。</span><span class="sxs-lookup"><span data-stu-id="3f497-179">This would output the verbatim string, and it is _not_ colorized.</span></span>
>
> ```output
> Random log \x1B[42mwith green background\x1B[49m message
> ```

## <a name="see-also"></a><span data-ttu-id="3f497-180">另请参阅</span><span class="sxs-lookup"><span data-stu-id="3f497-180">See also</span></span>

- [<span data-ttu-id="3f497-181">.NET 中的日志记录</span><span class="sxs-lookup"><span data-stu-id="3f497-181">Logging in .NET</span></span>](logging.md)
- [<span data-ttu-id="3f497-182">在 .NET 中实现自定义日志记录提供程序</span><span class="sxs-lookup"><span data-stu-id="3f497-182">Implement a custom logging provider in .NET</span></span>](custom-logging-provider.md)
- [<span data-ttu-id="3f497-183">.NET 中的高性能日志记录</span><span class="sxs-lookup"><span data-stu-id="3f497-183">High-performance logging in .NET</span></span>](high-performance-logging.md)
