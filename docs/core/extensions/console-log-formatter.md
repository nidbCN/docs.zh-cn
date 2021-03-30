---
title: 控制台日志格式设置
description: 了解如何使用可用的控制台日志格式设置，或为 .NET 应用程序实现自定义日志格式设置。
author: IEvangelist
ms.author: dapine
ms.date: 12/17/2020
ms.openlocfilehash: 178c66a99fac4d706156894cacffa5d902cb7901
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872686"
---
# <a name="console-log-formatting"></a>控制台日志格式设置

在 .NET 5 中，已向 `Microsoft.Extensions.Logging.Console` 命名空间中的控制台日志添加了对自定义格式设置的支持。 有三种预定义的格式设置选项可供选择：[`Simple`](#simple)、[`Systemd`](#systemd) 和 [`Json`](#json)。

> [!IMPORTANT]
> 以前，<xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat> 枚举允许选择所需的日志格式，可以是人工可读取的 (`Default`)，也可以是单行（也称为 `Systemd`）。 不过，这些都是不能进行自定义的，现已弃用。

本文介绍了控制台日志格式化程序。 示例源代码演示了如何执行以下操作：

- 注册新的格式化程序
- 选择要使用的已注册格式化程序
  - 通过代码或[配置](configuration.md)
- 实现自定义格式化程序
  - 通过 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 更新配置
  - 启用自定义颜色格式设置

## <a name="register-formatter"></a>注册格式化程序

[`Console` 日志记录提供程序](logging-providers.md#console)有几个预定义的格式化程序，并公开了可供你编写自己的自定义格式化程序的功能。 要注册任何可用的格式化程序，请使用相应的 `Add{Type}Console` 扩展方法：

| 可用类型 | 注册类型的方法 |
|--|--|
| <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Json?displayProperty=nameWithType> | <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddJsonConsole%2A?displayProperty=nameWithType> |
| <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType> | <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddSimpleConsole%2A?displayProperty=nameWithType> |
| <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType> | <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddSystemdConsole%2A?displayProperty=nameWithType> |

### <a name="simple"></a>简单

要使用 `Simple` 控制台格式化程序，请将其注册到 `AddSimpleConsole`：

:::code language="csharp" source="snippets/logging/console-formatter-simple/Program.cs" highlight="11-16":::

在前面的示例源代码中，已经注册了 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType> 格式化程序。 它不仅为日志提供了在每条日志消息中包装信息（如时间和日志级别）的功能，而且还允许 ANSI 颜色嵌入和消息缩进。

### <a name="systemd"></a>Systemd

<xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType> 控制台记录器：

- 使用“Syslog”日志级别格式和严重性
- 不为消息设置颜色格式
- 始终将消息记录在单行中

这对于容器来说通常很有用，因为容器经常使用 `Systemd` 控制台日志记录。 在 .NET 5 中，`Simple` 控制台记录器还可以实现以单行方式进行记录的紧凑版本，并且还允许禁用颜色，如前面的示例所示。

:::code language="csharp" source="snippets/logging/console-formatter-systemd/Program.cs" highlight="11-15":::

### <a name="json"></a>Json

要以 JSON 格式编写日志，需要使用 `Json` 控制台格式化程序。 示例源代码展示了 ASP.NET Core 应用如何注册格式化程序。 使用 `webapp` 模板，用 [dotnet new](../tools/dotnet-new.md) 命令创建一个新的 ASP.NET Core 应用：

```dotnetcli
dotnet new webapp -o Console.ExampleFormatters.Json
```

运行此应用时，使用模板代码会获得以下默认日志格式：

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
```

默认情况下，默认配置中会选择 `Simple` 控制台日志格式化程序。 可以通过在 Program.cs 中调用 `AddJsonConsole` 来更改此设置：

:::code language="csharp" source="snippets/logging/console-formatter-json/Program.cs" highlight="17-26":::

再次运行此应用，进行上述更改后，现在日志消息的格式为 JSON：

:::code language="json" source="snippets/logging/console-formatter-json/example-output.json":::

> [!TIP]
> 默认情况下，`Json` 控制台格式化程序将每条消息记录在单行中。 为了在配置格式化程序时使其更具可读性，请将 <xref:System.Text.Json.JsonWriterOptions.Indented?displayProperty=nameWithType> 设置为 `true`。

## <a name="set-formatter-with-configuration"></a>通过配置设置格式化程序

前面的示例展示了如何以编程方式注册格式化程序。 另外，也可以通过[配置](configuration.md)来完成。 考虑到之前的 Web 应用示例源代码，如果更新 appsettings.json 文件，而不是调用 Program.cs 文件中的 `ConfigureLogging`，可以得到相同的结果。 更新后的 `appsettings.json` 文件将对格式化程序进行如下配置：

:::code language="json" source="snippets/logging/console-formatter-json/appsettings.json" highlight="14-23":::

需要设置的两个键值为 `"FormatterName"` 和 `"FormatterOptions"`。 如果已经注册了值设置为 `"FormatterName"` 的格式化程序，则会选择此格式化程序，并且只要在 `"FormatterOptions"` 节点中将其属性作为键提供，就可以配置属性。 预定义的格式化程序名称保留在 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames> 下：

- <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Json?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Simple?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterNames.Systemd?displayProperty=nameWithType>

## <a name="implement-a-custom-formatter"></a>实现自定义格式化程序

要实现自定义格式化程序，需要执行以下操作：

- 创建一个 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatter> 的子类，这代表你的自定义格式化程序
- 注册以下自定义格式化程序
  - <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddConsole%2A?displayProperty=nameWithType>
  - <xref:Microsoft.Extensions.Logging.ConsoleLoggerExtensions.AddConsoleFormatter%60%602(Microsoft.Extensions.Logging.ILoggingBuilder,System.Action{%60%601})?displayProperty=nameWithType>

创建一个扩展方法来为你进行处理：

:::code language="csharp" source="snippets/logging/console-formatter-custom/ConsoleLoggerExtensions.cs" highlight="11-12":::

`CustomOptions` 的定义如下：

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomOptions.cs":::

在前面的代码中，选项是 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterOptions> 的子类。

`AddConsoleFormatter` API 会执行以下操作：

- 注册 `ConsoleFormatter` 的子类
- 处理配置：
  - 基于[选项模式](options.md)和 [IOptionsMonitor](xref:Microsoft.Extensions.Options.IOptionsMonitor%601) 接口，使用更改令牌同步更新

:::code language="csharp" source="snippets/logging/console-formatter-custom/Program.cs" highlight="11-12":::

定义 `ConsoleFormatter` 的 `CustomerFormatter` 子类：

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomFormatter.cs" highlight="24-45":::

前面的 `CustomFormatter.Write<TState>` API 指明了每个日志消息包装的文本类型。 一个标准的 `ConsoleFormatter` 应至少能包装日志的范围、时间戳和严重性级别。 此外，你还可以对日志消息中的 ANSI 颜色进行编码，并提供所需的缩进。 `CustomFormatter.Write<TState>` 的实现缺少这些功能。

有关进一步自定义格式设置的灵感，请参阅 `Microsoft.Extensions.Logging.Console` 命名空间中的现有实现：

- [SimpleConsoleFormatter](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.Extensions.Logging.Console/src/SimpleConsoleFormatter.cs)。
- [SystemdConsoleFormatter](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.Extensions.Logging.Console/src/SystemdConsoleFormatter.cs)
- [JsonConsoleFormatter](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.Extensions.Logging.Console/src/JsonConsoleFormatter.cs)

## <a name="implement-custom-color-formatting"></a>实现自定义颜色格式设置

为了在自定义日志记录格式化程序中正确地启用颜色功能，可以扩展 <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions>，因为它具有 <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions.ColorBehavior?displayProperty=nameWithType> 属性，这个属性对于在日志中启用颜色非常有用。

创建一个从 `SimpleConsoleFormatterOptions` 派生的 `CustomColorOptions`：

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomColorOptions.cs" highlight="5":::

接下来，在 `TextWriterExtensions` 类中编写一些扩展方法，以便在经过格式设置的日志消息中方便地嵌入 ANSI 编码的颜色：

:::code language="csharp" source="snippets/logging/console-formatter-custom/TextWriterExtensions.cs":::

可以定义一个处理应用自定义颜色的自定义颜色格式化程序，如下所示：

:::code language="csharp" source="snippets/logging/console-formatter-custom/CustomColorFormatter.cs" highlight="15-18,52-65":::

当你运行应用程序时，日志会在 `FormatterOptions.ColorBehavior` 为 `Enabled` 时用绿色显示 `CustomPrefix` 消息。

> [!NOTE]
> 当 <xref:Microsoft.Extensions.Logging.Console.LoggerColorBehavior> 为 `Disabled` 时，日志消息不会解释日志消息中嵌入的 ANSI 颜色代码。 相反，它们会输出原始消息。 例如，请考虑如下事项：
>
> ```csharp
> logger.LogInformation("Random log \x1B[42mwith green background\x1B[49m message");
> ```
>
> 这会输出逐字字符串，且不会着色。
>
> ```output
> Random log \x1B[42mwith green background\x1B[49m message
> ```

## <a name="see-also"></a>另请参阅

- [.NET 中的日志记录](logging.md)
- [在 .NET 中实现自定义日志记录提供程序](custom-logging-provider.md)
- [.NET 中的高性能日志记录](high-performance-logging.md)
