---
title: 中断性变更：ConsoleLoggerOptions 上已过时的属性
description: 了解核心 .NET 库中的 .NET 5 中断性变更：ConsoleLoggerFormat 类型和 ConsoleLoggerOptions 上的某些属性现已过时。
ms.date: 11/01/2020
ms.openlocfilehash: c6ee294f90e304cebd517bd0139c58a6c7a41e0c
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257305"
---
# <a name="obsolete-properties-on-consoleloggeroptions"></a><span data-ttu-id="34775-103">ConsoleLoggerOptions 上已过时的属性</span><span class="sxs-lookup"><span data-stu-id="34775-103">Obsolete properties on ConsoleLoggerOptions</span></span>

<span data-ttu-id="34775-104"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat?displayProperty=nameWithType> 类型和 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions> 上的一些属性现在已过时。</span><span class="sxs-lookup"><span data-stu-id="34775-104">The <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat?displayProperty=nameWithType> type and some properties on <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions> are now obsolete.</span></span>

## <a name="change-description"></a><span data-ttu-id="34775-105">更改说明</span><span class="sxs-lookup"><span data-stu-id="34775-105">Change description</span></span>

<span data-ttu-id="34775-106">从 .NET 5 开始，<xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat?displayProperty=nameWithType> 类型和 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions> 上的很多属性已过时。</span><span class="sxs-lookup"><span data-stu-id="34775-106">Starting in .NET 5, the <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerFormat?displayProperty=nameWithType> type and several properties on <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions> are obsolete.</span></span> <span data-ttu-id="34775-107">已过时的属性是：</span><span class="sxs-lookup"><span data-stu-id="34775-107">The obsolete properties are:</span></span>

- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.IncludeScopes?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.TimestampFormat?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.UseUtcTimestamp?displayProperty=nameWithType>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format?displayProperty=nameWithType>

<span data-ttu-id="34775-108">随着新的格式化程序的引入，这些属性现在单独的格式化程序上可用。</span><span class="sxs-lookup"><span data-stu-id="34775-108">With the introduction of new formatters, these properties are now available on the individual formatters.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="34775-109">更改原因</span><span class="sxs-lookup"><span data-stu-id="34775-109">Reason for change</span></span>

<span data-ttu-id="34775-110"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format> 属性是枚举类型，不能表示自定义格式化程序。</span><span class="sxs-lookup"><span data-stu-id="34775-110">The <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format> property is an enumeration type, which cannot represent a custom formatter.</span></span>

<span data-ttu-id="34775-111">其余属性是在 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions> 上设置的，应用于控制台日志的这两种内置格式。</span><span class="sxs-lookup"><span data-stu-id="34775-111">The remaining properties were set on <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions> and applied to both of the built-in formats for console logs.</span></span> <span data-ttu-id="34775-112">但是，由于引入了新的格式化程序 API，在特定于格式化程序的选项上表示格式更加有意义。</span><span class="sxs-lookup"><span data-stu-id="34775-112">However, with the introduction of a new formatter API, it makes more sense for formatting to be represented on the formatter-specific options.</span></span> <span data-ttu-id="34775-113">这项更改可更好地区分记录器和记录器格式化程序。</span><span class="sxs-lookup"><span data-stu-id="34775-113">This change provides better separation between the logger and logger formatters.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="34775-114">引入的版本</span><span class="sxs-lookup"><span data-stu-id="34775-114">Version introduced</span></span>

<span data-ttu-id="34775-115">5.0</span><span class="sxs-lookup"><span data-stu-id="34775-115">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="34775-116">建议操作</span><span class="sxs-lookup"><span data-stu-id="34775-116">Recommended action</span></span>

- <span data-ttu-id="34775-117">使用新的 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.FormatterName?displayProperty=nameWithType> 属性代替 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format?displayProperty=nameWithType> 属性。</span><span class="sxs-lookup"><span data-stu-id="34775-117">Use the new <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.FormatterName?displayProperty=nameWithType> property in place of the <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format?displayProperty=nameWithType> property.</span></span> <span data-ttu-id="34775-118">例如：</span><span class="sxs-lookup"><span data-stu-id="34775-118">For example:</span></span>

  ```csharp
  loggingBuilder.AddConsole(options =>
  {
    options.FormatterName = ConsoleFormatterNames.Systemd;
  });
  ```

  <span data-ttu-id="34775-119"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.FormatterName> 和 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format> 之间有一些区别：</span><span class="sxs-lookup"><span data-stu-id="34775-119">There are several differences between <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.FormatterName> and <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format>:</span></span>

  - <span data-ttu-id="34775-120"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format> 只有两个可能的选项：`Default` 和 `Systemd`。</span><span class="sxs-lookup"><span data-stu-id="34775-120"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format> has only two possible options: `Default` and `Systemd`.</span></span>
  - <span data-ttu-id="34775-121"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.FormatterName> 不区分大小写，可以是任何字符串。</span><span class="sxs-lookup"><span data-stu-id="34775-121"><xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.FormatterName> is case insensitive and can be any string.</span></span> <span data-ttu-id="34775-122">保留的内置名称为 `Simple`、`Systemd` 和 `Json`（.NET 5 及更高版本）。</span><span class="sxs-lookup"><span data-stu-id="34775-122">The reserved, built-in names are `Simple`, `Systemd`, and `Json` (.NET 5 and later).</span></span>
  - <span data-ttu-id="34775-123">`"Format": "Systemd"` 映射到 `"FormatterName": "Systemd"`。</span><span class="sxs-lookup"><span data-stu-id="34775-123">`"Format": "Systemd"` maps to `"FormatterName": "Systemd"`.</span></span>
  - <span data-ttu-id="34775-124">`"Format": "Default"` 映射到 `"FormatterName": "Simple"`。</span><span class="sxs-lookup"><span data-stu-id="34775-124">`"Format": "Default"` maps to `"FormatterName": "Simple"`.</span></span>

- <span data-ttu-id="34775-125">对于 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors>、<xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.IncludeScopes>、<xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.TimestampFormat> 和 <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.UseUtcTimestamp> 属性，请改为在新的 <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterOptions>、<xref:Microsoft.Extensions.Logging.Console.JsonConsoleFormatterOptions> 或 <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions> 类型上使用相应的属性。</span><span class="sxs-lookup"><span data-stu-id="34775-125">For the <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors>, <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.IncludeScopes>, <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.TimestampFormat>, and <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.UseUtcTimestamp> properties, use the corresponding property on the new <xref:Microsoft.Extensions.Logging.Console.ConsoleFormatterOptions>, <xref:Microsoft.Extensions.Logging.Console.JsonConsoleFormatterOptions>, or <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions> types instead.</span></span> <span data-ttu-id="34775-126">例如，<xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors?displayProperty=nameWithType> 的相应设置为 <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions.ColorBehavior?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="34775-126">For example, the corresponding setting for <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors?displayProperty=nameWithType> is <xref:Microsoft.Extensions.Logging.Console.SimpleConsoleFormatterOptions.ColorBehavior?displayProperty=nameWithType>.</span></span>

  <span data-ttu-id="34775-127">之前的代码：</span><span class="sxs-lookup"><span data-stu-id="34775-127">Previous code:</span></span>

  ```csharp
  loggingBuilder.AddConsole(options =>
  {
      options.DisableColors = true;
  });
  ```

  <span data-ttu-id="34775-128">新代码：</span><span class="sxs-lookup"><span data-stu-id="34775-128">New code:</span></span>

  ```csharp
  loggingBuilder.AddSimpleConsole(options =>
  {
      options.ColorBehavior = LoggerColorBehavior.Disabled;
  });
  ```

<span data-ttu-id="34775-129">以下两个 JSON 代码片段显示了配置文件的更改方式。</span><span class="sxs-lookup"><span data-stu-id="34775-129">The following two JSON snippets show how the configuration file changes.</span></span> <span data-ttu-id="34775-130">旧配置文件：</span><span class="sxs-lookup"><span data-stu-id="34775-130">Old configuration file:</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "None",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    },

    "Console": {
      "LogLevel": {
        "Default": "Information"
      },
      "Format": "Systemd",
      "IncludeScopes": true,
      "TimestampFormat": "HH:mm:ss",
      "UseUtcTimestamp": true
    }
  },
  "AllowedHosts": "*"
}
```

<span data-ttu-id="34775-131">新配置文件：</span><span class="sxs-lookup"><span data-stu-id="34775-131">New configuration file:</span></span>

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "None",
      "Microsoft": "Warning",
      "Microsoft.Hosting.Lifetime": "Information"
    },

    "Console": {
      "LogLevel": {
        "Default": "Information"
      },
      "FormatterName": "Systemd",
      "FormatterOptions": {
        "IncludeScopes": true,
        "TimestampFormat": "HH:mm:ss",
        "UseUtcTimestamp": true
      }
    }
  },
  "AllowedHosts": "*"
}
```

## <a name="affected-apis"></a><span data-ttu-id="34775-132">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="34775-132">Affected APIs</span></span>

- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors?displayProperty=fullName>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.IncludeScopes?displayProperty=fullName>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.TimestampFormat?displayProperty=fullName>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.UseUtcTimestamp?displayProperty=fullName>
- <xref:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format?displayProperty=fullName>

<!--

#### Category

- Core .NET libraries
- ASP.NET

### Affected APIs

- `P:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.DisableColors`
- `P:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.IncludeScopes`
- `P:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.TimestampFormat`
- `P:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.UseUtcTimestamp`
- `P:Microsoft.Extensions.Logging.Console.ConsoleLoggerOptions.Format`

-->
