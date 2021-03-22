---
title: .NET 中的配置提供程序
description: 了解如何使用配置提供程序 API 来配置 .NET 应用程序。
author: IEvangelist
ms.author: dapine
ms.date: 03/08/2021
ms.openlocfilehash: 5f248c93de1773a8bbe8209f002806fb196bb260
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605004"
---
# <a name="configuration-providers-in-net"></a><span data-ttu-id="a31f9-103">.NET 中的配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-103">Configuration providers in .NET</span></span>

<span data-ttu-id="a31f9-104">可通过配置提供程序进行 .NET 配置。</span><span class="sxs-lookup"><span data-stu-id="a31f9-104">Configuration in .NET is possible with configuration providers.</span></span> <span data-ttu-id="a31f9-105">存在几种类型的提供程序，它们依赖于不同的配置源。</span><span class="sxs-lookup"><span data-stu-id="a31f9-105">There are several types of providers that rely on various configuration sources.</span></span> <span data-ttu-id="a31f9-106">本文详细介绍了所有不同的配置提供程序及其相应的源。</span><span class="sxs-lookup"><span data-stu-id="a31f9-106">This article details all of the different configuration providers and their corresponding sources.</span></span>

- [<span data-ttu-id="a31f9-107">文件配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-107">File configuration provider</span></span>](#file-configuration-provider)
- [<span data-ttu-id="a31f9-108">环境变量配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-108">Environment variable configuration provider</span></span>](#environment-variable-configuration-provider)
- [<span data-ttu-id="a31f9-109">命令行配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-109">Command-line configuration provider</span></span>](#command-line-configuration-provider)
- [<span data-ttu-id="a31f9-110">Key-per-file 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-110">Key-per-file configuration provider</span></span>](#key-per-file-configuration-provider)
- [<span data-ttu-id="a31f9-111">内存配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-111">Memory configuration provider</span></span>](#memory-configuration-provider)

## <a name="file-configuration-provider"></a><span data-ttu-id="a31f9-112">文件配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-112">File configuration provider</span></span>

<span data-ttu-id="a31f9-113"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> 是从文件系统加载配置的基类。</span><span class="sxs-lookup"><span data-stu-id="a31f9-113"><xref:Microsoft.Extensions.Configuration.FileConfigurationProvider> is the base class for loading configuration from the file system.</span></span> <span data-ttu-id="a31f9-114">以下配置提供程序派生自 `FileConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="a31f9-114">The following configuration providers derive from `FileConfigurationProvider`:</span></span>

- [<span data-ttu-id="a31f9-115">JSON 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-115">JSON configuration provider</span></span>](#json-configuration-provider)
- [<span data-ttu-id="a31f9-116">XML 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-116">XML configuration provider</span></span>](#xml-configuration-provider)
- [<span data-ttu-id="a31f9-117">INI 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-117">INI configuration provider</span></span>](#ini-configuration-provider)

### <a name="json-configuration-provider"></a><span data-ttu-id="a31f9-118">JSON 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-118">JSON configuration provider</span></span>

<span data-ttu-id="a31f9-119"><xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> 类从 JSON 文件加载配置。</span><span class="sxs-lookup"><span data-stu-id="a31f9-119">The <xref:Microsoft.Extensions.Configuration.Json.JsonConfigurationProvider> class loads configuration from a JSON file.</span></span> <span data-ttu-id="a31f9-120">安装 NuGet 包 [`Microsoft.Extensions.Configuration.Json`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json)。</span><span class="sxs-lookup"><span data-stu-id="a31f9-120">Install the [`Microsoft.Extensions.Configuration.Json`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) NuGet package.</span></span>

<span data-ttu-id="a31f9-121">重载可以指定：</span><span class="sxs-lookup"><span data-stu-id="a31f9-121">Overloads can specify:</span></span>

- <span data-ttu-id="a31f9-122">文件是否可选</span><span class="sxs-lookup"><span data-stu-id="a31f9-122">Whether the file is optional</span></span>
- <span data-ttu-id="a31f9-123">如果文件更改，是否重载配置</span><span class="sxs-lookup"><span data-stu-id="a31f9-123">Whether the configuration is reloaded if the file changes</span></span>

<span data-ttu-id="a31f9-124">考虑下列代码：</span><span class="sxs-lookup"><span data-stu-id="a31f9-124">Consider the following code:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/Program.cs" range="1-39,43-44" highlight="23-29":::

<span data-ttu-id="a31f9-125">前面的代码：</span><span class="sxs-lookup"><span data-stu-id="a31f9-125">The preceding code:</span></span>

- <span data-ttu-id="a31f9-126">清除 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder(System.String[])> 方法中默认添加的所有现有的配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a31f9-126">Clears all existing configuration providers that were added by default in the <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder(System.String[])> method.</span></span>
- <span data-ttu-id="a31f9-127">将 JSON 配置提供程序配置为使用以下选项加载 appsettings.json 和 appsettings.`Environment`.json 文件  ：</span><span class="sxs-lookup"><span data-stu-id="a31f9-127">Configures the JSON configuration provider to load the *appsettings.json* and  *appsettings*.`Environment`.*json* files with the following options:</span></span>
  - <span data-ttu-id="a31f9-128">`optional: true`：文件是可选的。</span><span class="sxs-lookup"><span data-stu-id="a31f9-128">`optional: true`: The file is optional.</span></span>
  - <span data-ttu-id="a31f9-129">`reloadOnChange: true`：保存更改后会重载文件。</span><span class="sxs-lookup"><span data-stu-id="a31f9-129">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>

<span data-ttu-id="a31f9-130">JSON 设置会被[环境变量配置提供程序](#environment-variable-configuration-provider)和[命令行配置提供程序](#command-line-configuration-provider)中的设置替代。</span><span class="sxs-lookup"><span data-stu-id="a31f9-130">The JSON settings are overridden by settings in the [Environment variables configuration provider](#environment-variable-configuration-provider) and the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="a31f9-131">下面是一个具有各种配置设置的示例 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="a31f9-131">An example *appsettings.json* file with various configuration settings follows:</span></span>

:::code language="json" source="snippets/configuration/console-json/appsettings.json":::

<span data-ttu-id="a31f9-132">从 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 实例中，在添加配置提供程序后，可调用 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Build?displayProperty=nameWithType> 来获取 <xref:Microsoft.Extensions.Configuration.IConfigurationRoot> 对象。</span><span class="sxs-lookup"><span data-stu-id="a31f9-132">From the <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> instance, after configuration providers have been added you can call <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder.Build?displayProperty=nameWithType> to get the <xref:Microsoft.Extensions.Configuration.IConfigurationRoot> object.</span></span> <span data-ttu-id="a31f9-133">配置根表示配置层次结构的根。</span><span class="sxs-lookup"><span data-stu-id="a31f9-133">The configuration root represents the root of a configuration hierarchy.</span></span> <span data-ttu-id="a31f9-134">可将配置中的节绑定到 .NET 对象的实例，稍后再通过依赖注入将其作为 <xref:Microsoft.Extensions.Options.IOptions%601> 提供。</span><span class="sxs-lookup"><span data-stu-id="a31f9-134">Sections from the configuration can be bound to instances of .NET objects, and later provided as <xref:Microsoft.Extensions.Options.IOptions%601> through dependency injection.</span></span>

<span data-ttu-id="a31f9-135">请考虑如下定义的 `TransientFaultHandlingOptions` 类：</span><span class="sxs-lookup"><span data-stu-id="a31f9-135">Consider the `TransientFaultHandlingOptions` class defined as follows:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/TransientFaultHandlingOptions.cs":::

<span data-ttu-id="a31f9-136">下面的代码会生成配置根，将一个节绑定到类 `TransientFaultHandlingOptions` 类型，并将绑定值输出到控制台窗口：</span><span class="sxs-lookup"><span data-stu-id="a31f9-136">The following code builds the configuration root, binds a section to the `TransientFaultHandlingOptions` class type, and prints the bound values to the console window:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/Program.cs" range="31-38":::

<span data-ttu-id="a31f9-137">应用程序会编写以下示例输出：</span><span class="sxs-lookup"><span data-stu-id="a31f9-137">The application would write the following sample output:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/Program.cs" range="40-42":::

### <a name="xml-configuration-provider"></a><span data-ttu-id="a31f9-138">XML 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-138">XML configuration provider</span></span>

<span data-ttu-id="a31f9-139"><xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> 类在运行时从 XML 文件加载配置。</span><span class="sxs-lookup"><span data-stu-id="a31f9-139">The <xref:Microsoft.Extensions.Configuration.Xml.XmlConfigurationProvider> class loads configuration from an XML file at runtime.</span></span> <span data-ttu-id="a31f9-140">安装 NuGet 包 [`Microsoft.Extensions.Configuration.Xml`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Xml)。</span><span class="sxs-lookup"><span data-stu-id="a31f9-140">Install the [`Microsoft.Extensions.Configuration.Xml`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Xml) NuGet package.</span></span>

<span data-ttu-id="a31f9-141">下面的代码演示如何使用 XML 配置提供程序配置 XML 文件。</span><span class="sxs-lookup"><span data-stu-id="a31f9-141">The following code demonstrates configuration of XML files using the XML configuration provider.</span></span>

:::code language="csharp" source="snippets/configuration/console-xml/Program.cs" range="1-34,52,58-59" highlight="23-34":::

<span data-ttu-id="a31f9-142">上述代码：</span><span class="sxs-lookup"><span data-stu-id="a31f9-142">The preceding code:</span></span>

- <span data-ttu-id="a31f9-143">清除 <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder(System.String[])> 方法中默认添加的所有现有的配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a31f9-143">Clears all existing configuration providers that were added by default in the <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder(System.String[])> method.</span></span>
- <span data-ttu-id="a31f9-144">将 XML 配置提供程序配置为使用以下选项加载 appsettings.xml 和 repeating-example.xml 文件 ：</span><span class="sxs-lookup"><span data-stu-id="a31f9-144">Configures the XML configuration provider to load the *appsettings.xml* and *repeating-example.xml* files with the following options:</span></span>
  - <span data-ttu-id="a31f9-145">`optional: true`：文件是可选的。</span><span class="sxs-lookup"><span data-stu-id="a31f9-145">`optional: true`: The file is optional.</span></span>
  - <span data-ttu-id="a31f9-146">`reloadOnChange: true`：保存更改后会重载文件。</span><span class="sxs-lookup"><span data-stu-id="a31f9-146">`reloadOnChange: true` : The file is reloaded when changes are saved.</span></span>
- <span data-ttu-id="a31f9-147">配置环境变量配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a31f9-147">Configures the environment variables configuration provider.</span></span>
- <span data-ttu-id="a31f9-148">如果给定的 `args` 包含自变量，则配置命令行配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a31f9-148">Configures the command-line configuration provider if the given `args` contains arguments.</span></span>

<span data-ttu-id="a31f9-149">XML 设置会被[环境变量配置提供程序](#environment-variable-configuration-provider)和[命令行配置提供程序](#command-line-configuration-provider)中的设置替代。</span><span class="sxs-lookup"><span data-stu-id="a31f9-149">The XML settings are overridden by settings in the [Environment variables configuration provider](#environment-variable-configuration-provider) and the [Command-line configuration provider](#command-line-configuration-provider).</span></span>

<span data-ttu-id="a31f9-150">下面是一个具有各种配置设置的示例 appsettings.xml 文件：</span><span class="sxs-lookup"><span data-stu-id="a31f9-150">An example *appsettings.xml* file with various configuration settings follows:</span></span>

:::code language="xml" source="snippets/configuration/console-xml/appsettings.xml":::

<span data-ttu-id="a31f9-151">如果使用 `name` 属性来区分元素，则使用相同元素名称的重复元素可以正常工作：</span><span class="sxs-lookup"><span data-stu-id="a31f9-151">Repeating elements that use the same element name work if the `name` attribute is used to distinguish the elements:</span></span>

:::code language="xml" source="snippets/configuration/console-xml/repeating-example.xml":::

<span data-ttu-id="a31f9-152">以下代码会读取前面的配置文件并显示键和值：</span><span class="sxs-lookup"><span data-stu-id="a31f9-152">The following code reads the previous configuration file and displays the keys and values:</span></span>

:::code language="csharp" source="snippets/configuration/console-xml/Program.cs" range="36-51":::

<span data-ttu-id="a31f9-153">该应用程序会编写以下示例输出：</span><span class="sxs-lookup"><span data-stu-id="a31f9-153">The application would write the following sample output:</span></span>

:::code language="csharp" source="snippets/configuration/console-xml/Program.cs" range="53-57":::

<span data-ttu-id="a31f9-154">属性可用于提供值：</span><span class="sxs-lookup"><span data-stu-id="a31f9-154">Attributes can be used to supply values:</span></span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <key attribute="value" />
  <section>
    <key attribute="value" />
  </section>
</configuration>
```

<span data-ttu-id="a31f9-155">以前的配置文件使用 `value` 加载以下键：</span><span class="sxs-lookup"><span data-stu-id="a31f9-155">The previous configuration file loads the following keys with `value`:</span></span>

- `key:attribute`
- `section:key:attribute`

### <a name="ini-configuration-provider"></a><span data-ttu-id="a31f9-156">INI 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-156">INI configuration provider</span></span>

<span data-ttu-id="a31f9-157"><xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> 类在运行时从 INI 文件加载配置。</span><span class="sxs-lookup"><span data-stu-id="a31f9-157">The <xref:Microsoft.Extensions.Configuration.Ini.IniConfigurationProvider> class loads configuration from an INI file at runtime.</span></span> <span data-ttu-id="a31f9-158">安装 NuGet 包 [`Microsoft.Extensions.Configuration.Ini`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Ini)。</span><span class="sxs-lookup"><span data-stu-id="a31f9-158">Install the [`Microsoft.Extensions.Configuration.Ini`](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Ini)  NuGet package.</span></span>

<span data-ttu-id="a31f9-159">以下代码将清除所有配置提供程序，并添加具有两个 INI 文件的 `IniConfigurationProvider` 作为源：</span><span class="sxs-lookup"><span data-stu-id="a31f9-159">The following code clears all the configuration providers and adds the `IniConfigurationProvider` with two INI files as the source:</span></span>

:::code language="csharp" source="snippets/configuration/console-ini/Program.cs" range="1-37,44-45" highlight="24-30":::

<span data-ttu-id="a31f9-160">下面是一个具有各种配置设置的示例 appsettings.ini 文件：</span><span class="sxs-lookup"><span data-stu-id="a31f9-160">An example *appsettings.ini* file with various configuration settings follows:</span></span>

:::code language="ini" source="snippets/configuration/console-ini/appsettings.ini":::

<span data-ttu-id="a31f9-161">以下代码通过将上述配置设置写入控制台窗口来显示这些设置：</span><span class="sxs-lookup"><span data-stu-id="a31f9-161">The following code displays the preceding configuration settings by writing them to the console window:</span></span>

:::code language="csharp" source="snippets/configuration/console-ini/Program.cs" range="32-36":::

<span data-ttu-id="a31f9-162">该应用程序会编写以下示例输出：</span><span class="sxs-lookup"><span data-stu-id="a31f9-162">The application would write the following sample output:</span></span>

:::code language="csharp" source="snippets/configuration/console-ini/Program.cs" range="38-43":::

## <a name="environment-variable-configuration-provider"></a><span data-ttu-id="a31f9-163">环境变量配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-163">Environment variable configuration provider</span></span>

<span data-ttu-id="a31f9-164">通过默认配置，<xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> 会在读取 appsettings.json、appsettings.`Environment`.json 和机密管理器后，从环境变量键值对加载配置  。</span><span class="sxs-lookup"><span data-stu-id="a31f9-164">Using the default configuration, the <xref:Microsoft.Extensions.Configuration.EnvironmentVariables.EnvironmentVariablesConfigurationProvider> loads configuration from environment variable key-value pairs after reading *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span> <span data-ttu-id="a31f9-165">因此，从环境中读取的键值会替代从 appsettings.json、appsettings.`Environment`.json 和机密管理器中读取的值  。</span><span class="sxs-lookup"><span data-stu-id="a31f9-165">Therefore, key values read from the environment override values read from *appsettings.json*, *appsettings.*`Environment`*.json*, and Secret manager.</span></span>

<span data-ttu-id="a31f9-166">所有平台上的环境变量分层键都不支持 `:` 分隔符。</span><span class="sxs-lookup"><span data-stu-id="a31f9-166">The `:` separator doesn't work with environment variable hierarchical keys on all platforms.</span></span> <span data-ttu-id="a31f9-167">双下划线 (`__`) 会自动替换为 `:` 且受各大平台支持。</span><span class="sxs-lookup"><span data-stu-id="a31f9-167">The double underscore (`__`) is automatically replaced by a `:` and is supported by all platforms.</span></span> <span data-ttu-id="a31f9-168">例如，[Bash](https://linuxhint.com/bash-environment-variables) 不支持 `:` 分隔符，但支持 `__`。</span><span class="sxs-lookup"><span data-stu-id="a31f9-168">For example, the `:` separator is not supported by [Bash](https://linuxhint.com/bash-environment-variables), but `__` is.</span></span>

<span data-ttu-id="a31f9-169">以下 `set` 命令：</span><span class="sxs-lookup"><span data-stu-id="a31f9-169">The following `set` commands:</span></span>

- <span data-ttu-id="a31f9-170">在 Windows 上设置上述示例的环境键和值。</span><span class="sxs-lookup"><span data-stu-id="a31f9-170">Set the environment keys and values of the preceding example on Windows.</span></span>
- <span data-ttu-id="a31f9-171">通过将设置更改为非默认值来对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="a31f9-171">Test the settings by changing them from their default values.</span></span> <span data-ttu-id="a31f9-172">`dotnet run` 命令必须在项目目录中运行。</span><span class="sxs-lookup"><span data-stu-id="a31f9-172">The `dotnet run` command must be run in the project directory.</span></span>

```dotnetcli
set SecretKey="Secret key from environment"
set TransientFaultHandlingOptions__Enabled="true"
set TransientFaultHandlingOptions__AutoRetryDelay="00:00:13"

dotnet run
```

<span data-ttu-id="a31f9-173">前面的环境设置：</span><span class="sxs-lookup"><span data-stu-id="a31f9-173">The preceding environment settings:</span></span>

- <span data-ttu-id="a31f9-174">仅在进程中设置，这些进程是从设置进程的命令窗口启动的。</span><span class="sxs-lookup"><span data-stu-id="a31f9-174">Are only set in processes launched from the command window they were set in.</span></span>
- <span data-ttu-id="a31f9-175">不会由通过 Visual Studio 启动的 Web 应用读取。</span><span class="sxs-lookup"><span data-stu-id="a31f9-175">Won't be read by web apps launched with Visual Studio.</span></span>

<span data-ttu-id="a31f9-176">以下 [setx](/windows-server/administration/windows-commands/setx) 命令可用于在 Windows 上设置环境键和值。</span><span class="sxs-lookup"><span data-stu-id="a31f9-176">The following [setx](/windows-server/administration/windows-commands/setx) commands can be used to set the environment keys and values on Windows.</span></span> <span data-ttu-id="a31f9-177">与 `set` 不同，`setx` 设置是持久的。</span><span class="sxs-lookup"><span data-stu-id="a31f9-177">Unlike `set`, `setx` settings are persisted.</span></span> <span data-ttu-id="a31f9-178">`/M` 在系统环境中设置变量。</span><span class="sxs-lookup"><span data-stu-id="a31f9-178">`/M` sets the variable in the system environment.</span></span> <span data-ttu-id="a31f9-179">如果未使用 `/M` 开关，则会设置用户环境变量。</span><span class="sxs-lookup"><span data-stu-id="a31f9-179">If the `/M` switch isn't used, a user environment variable is set.</span></span>

```dotnetcli
setx SecretKey "Secret key from setx environment" /M
setx TransientFaultHandlingOptions__Enabled "true" /M
setx TransientFaultHandlingOptions__AutoRetryDelay "00:00:05" /M

dotnet run
```

<span data-ttu-id="a31f9-180">测试前面的命令是否会替代 appsettings.json 和 appsettings.`Environment`.json：</span><span class="sxs-lookup"><span data-stu-id="a31f9-180">To test that the preceding commands override *appsettings.json* and *appsettings.*`Environment`*.json*:</span></span>

- <span data-ttu-id="a31f9-181">使用 Visual Studio：退出并重启 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a31f9-181">With Visual Studio: Exit and restart Visual Studio.</span></span>
- <span data-ttu-id="a31f9-182">使用 CLI：启动新的命令窗口并输入 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="a31f9-182">With the CLI: Start a new command window and enter `dotnet run`.</span></span>

<span data-ttu-id="a31f9-183">使用字符串调用 <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> 以指定环境变量的前缀：</span><span class="sxs-lookup"><span data-stu-id="a31f9-183">Call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables%2A> with a string to specify a prefix for environment variables:</span></span>

:::code language="csharp" source="snippets/configuration/console-env/Program.cs" highlight="21-22":::

<span data-ttu-id="a31f9-184">在上述代码中：</span><span class="sxs-lookup"><span data-stu-id="a31f9-184">In the preceding code:</span></span>

- <span data-ttu-id="a31f9-185">`config.AddEnvironmentVariables(prefix: "CustomPrefix_")` 被添加到默认配置提供程序的后面。</span><span class="sxs-lookup"><span data-stu-id="a31f9-185">`config.AddEnvironmentVariables(prefix: "CustomPrefix_")` is added after the default configuration providers.</span></span> <span data-ttu-id="a31f9-186">有关对配置提供程序进行排序的示例，请查看 [XML 配置提供程序](#xml-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="a31f9-186">For an example of ordering the configuration providers, see [XML configuration provider](#xml-configuration-provider).</span></span>
- <span data-ttu-id="a31f9-187">设有 `CustomPrefix_` 前缀的环境变量会替代默认配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="a31f9-187">Environment variables set with the `CustomPrefix_` prefix override the default configuration providers.</span></span> <span data-ttu-id="a31f9-188">这包括没有前缀的环境变量。</span><span class="sxs-lookup"><span data-stu-id="a31f9-188">This includes environment variables without the prefix.</span></span>

<span data-ttu-id="a31f9-189">前缀会在读取配置键值对时被去除。</span><span class="sxs-lookup"><span data-stu-id="a31f9-189">The prefix is stripped off when the configuration key-value pairs are read.</span></span>

<span data-ttu-id="a31f9-190">以下命令对自定义前缀进行测试：</span><span class="sxs-lookup"><span data-stu-id="a31f9-190">The following commands test the custom prefix:</span></span>

```dotnetcli
set CustomPrefix__SecretKey="Secret key with CustomPrefix_ environment"
set CustomPrefix__TransientFaultHandlingOptions__Enabled=true
set CustomPrefix__TransientFaultHandlingOptions__AutoRetryDelay=00:00:21

dotnet run
```

<span data-ttu-id="a31f9-191">默认配置会加载前缀为 `DOTNET_` 的环境变量和命令行自变量。</span><span class="sxs-lookup"><span data-stu-id="a31f9-191">The default configuration loads environment variables and command-line arguments prefixed with `DOTNET_`.</span></span> <span data-ttu-id="a31f9-192">`DOTNET_` 前缀由 .NET 用于[主机](generic-host.md#host-configuration)和[应用配置](generic-host.md#app-configuration)，但不用于用户配置。</span><span class="sxs-lookup"><span data-stu-id="a31f9-192">The `DOTNET_` prefix is used by .NET for [host](generic-host.md#host-configuration) and [app configuration](generic-host.md#app-configuration), but not for user configuration.</span></span>

<span data-ttu-id="a31f9-193">有关主机和应用配置的详细信息，请参阅 [.NET 通用主机](generic-host.md)。</span><span class="sxs-lookup"><span data-stu-id="a31f9-193">For more information on host and app configuration, see [.NET Generic Host](generic-host.md).</span></span>

<span data-ttu-id="a31f9-194">在 [Azure 应用服务](https://azure.microsoft.com/services/app-service)上，选择“设置”>“配置”页面上的“新应用程序设置” 。</span><span class="sxs-lookup"><span data-stu-id="a31f9-194">On [Azure App Service](https://azure.microsoft.com/services/app-service), select **New application setting** on the **Settings > Configuration** page.</span></span> <span data-ttu-id="a31f9-195">Azure 应用服务应用程序设置：</span><span class="sxs-lookup"><span data-stu-id="a31f9-195">Azure App Service application settings are:</span></span>

- <span data-ttu-id="a31f9-196">已静态加密且通过加密的通道进行传输。</span><span class="sxs-lookup"><span data-stu-id="a31f9-196">Encrypted at rest and transmitted over an encrypted channel.</span></span>
- <span data-ttu-id="a31f9-197">已作为环境变量公开。</span><span class="sxs-lookup"><span data-stu-id="a31f9-197">Exposed as environment variables.</span></span>

### <a name="connection-string-prefixes"></a><span data-ttu-id="a31f9-198">连接字符串前缀</span><span class="sxs-lookup"><span data-stu-id="a31f9-198">Connection string prefixes</span></span>

<span data-ttu-id="a31f9-199">对于四个连接字符串环境变量，配置 API 具有特殊的处理规则。</span><span class="sxs-lookup"><span data-stu-id="a31f9-199">The Configuration API has special processing rules for four connection string environment variables.</span></span> <span data-ttu-id="a31f9-200">这些连接字符串涉及了为应用环境配置 Azure 连接字符串。</span><span class="sxs-lookup"><span data-stu-id="a31f9-200">These connection strings are involved in configuring Azure connection strings for the app environment.</span></span> <span data-ttu-id="a31f9-201">使用默认配置或没有向 `AddEnvironmentVariables` 应用前缀时，具有表中所示前缀的环境变量将加载到应用中。</span><span class="sxs-lookup"><span data-stu-id="a31f9-201">Environment variables with the prefixes shown in the table are loaded into the app with the default configuration or when no prefix is supplied to `AddEnvironmentVariables`.</span></span>

| <span data-ttu-id="a31f9-202">连接字符串前缀</span><span class="sxs-lookup"><span data-stu-id="a31f9-202">Connection string prefix</span></span> | <span data-ttu-id="a31f9-203">提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-203">Provider</span></span>                                                                |
|--------------------------|-------------------------------------------------------------------------|
| `CUSTOMCONNSTR_`         | <span data-ttu-id="a31f9-204">自定义提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-204">Custom provider</span></span>                                                         |
| `MYSQLCONNSTR_`          | [<span data-ttu-id="a31f9-205">MySQL</span><span class="sxs-lookup"><span data-stu-id="a31f9-205">MySQL</span></span>](https://www.mysql.com)                                          |
| `SQLAZURECONNSTR_`       | [<span data-ttu-id="a31f9-206">Azure SQL 数据库</span><span class="sxs-lookup"><span data-stu-id="a31f9-206">Azure SQL Database</span></span>](https://azure.microsoft.com/services/sql-database) |
| `SQLCONNSTR_`            | [<span data-ttu-id="a31f9-207">SQL Server</span><span class="sxs-lookup"><span data-stu-id="a31f9-207">SQL Server</span></span>](https://www.microsoft.com/sql-server)                      |

<span data-ttu-id="a31f9-208">当发现环境变量并使用表中所示的四个前缀中的任何一个加载到配置中时：</span><span class="sxs-lookup"><span data-stu-id="a31f9-208">When an environment variable is discovered and loaded into configuration with any of the four prefixes shown in the table:</span></span>

- <span data-ttu-id="a31f9-209">通过删除环境变量前缀并添加配置键节 (`ConnectionStrings`) 来创建配置键。</span><span class="sxs-lookup"><span data-stu-id="a31f9-209">The configuration key is created by removing the environment variable prefix and adding a configuration key section (`ConnectionStrings`).</span></span>
- <span data-ttu-id="a31f9-210">创建一个新的配置键值对，表示数据库连接提供程序（`CUSTOMCONNSTR_` 除外，它没有声明的提供程序）。</span><span class="sxs-lookup"><span data-stu-id="a31f9-210">A new configuration key-value pair is created that represents the database connection provider (except for `CUSTOMCONNSTR_`, which has no stated provider).</span></span>

| <span data-ttu-id="a31f9-211">环境变量键</span><span class="sxs-lookup"><span data-stu-id="a31f9-211">Environment variable key</span></span> | <span data-ttu-id="a31f9-212">转换的配置键</span><span class="sxs-lookup"><span data-stu-id="a31f9-212">Converted configuration key</span></span> | <span data-ttu-id="a31f9-213">提供程序配置条目</span><span class="sxs-lookup"><span data-stu-id="a31f9-213">Provider configuration entry</span></span>                                                    |
|--------------------------|-----------------------------|---------------------------------------------------------------------------------|
| `CUSTOMCONNSTR_{KEY}`    | `ConnectionStrings:{KEY}`   | <span data-ttu-id="a31f9-214">配置条目未创建。</span><span class="sxs-lookup"><span data-stu-id="a31f9-214">Configuration entry not created.</span></span>                                                |
| `MYSQLCONNSTR_{KEY}`     | `ConnectionStrings:{KEY}`   | <span data-ttu-id="a31f9-215">键：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="a31f9-215">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="a31f9-216">值：`MySql.Data.MySqlClient`</span><span class="sxs-lookup"><span data-stu-id="a31f9-216">Value: `MySql.Data.MySqlClient`</span></span> |
| `SQLAZURECONNSTR_{KEY}`  | `ConnectionStrings:{KEY}`   | <span data-ttu-id="a31f9-217">键：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="a31f9-217">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="a31f9-218">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a31f9-218">Value: `System.Data.SqlClient`</span></span>  |
| `SQLCONNSTR_{KEY}`       | `ConnectionStrings:{KEY}`   | <span data-ttu-id="a31f9-219">键：`ConnectionStrings:{KEY}_ProviderName`：</span><span class="sxs-lookup"><span data-stu-id="a31f9-219">Key: `ConnectionStrings:{KEY}_ProviderName`:</span></span><br><span data-ttu-id="a31f9-220">值：`System.Data.SqlClient`</span><span class="sxs-lookup"><span data-stu-id="a31f9-220">Value: `System.Data.SqlClient`</span></span>  |

### <a name="environment-variables-set-in-launchsettingsjson"></a><span data-ttu-id="a31f9-221">在 launchSettings.json 中设置的环境变量</span><span class="sxs-lookup"><span data-stu-id="a31f9-221">Environment variables set in launchSettings.json</span></span>

<span data-ttu-id="a31f9-222">在 launchSettings.json 中设置的环境变量将替代在系统环境中设置的变量。</span><span class="sxs-lookup"><span data-stu-id="a31f9-222">Environment variables set in *launchSettings.json* override those set in the system environment.</span></span>

## <a name="command-line-configuration-provider"></a><span data-ttu-id="a31f9-223">命令行配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-223">Command-line configuration provider</span></span>

<span data-ttu-id="a31f9-224">使用默认配置，<xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> 会从以下配置源后的命令行参数键值对中加载配置：</span><span class="sxs-lookup"><span data-stu-id="a31f9-224">Using the default configuration, the <xref:Microsoft.Extensions.Configuration.CommandLine.CommandLineConfigurationProvider> loads configuration from command-line argument key-value pairs after the following configuration sources:</span></span>

- <span data-ttu-id="a31f9-225">appsettings.json 和 appsettings.`Environment`.json 文件  。</span><span class="sxs-lookup"><span data-stu-id="a31f9-225">*appsettings.json* and *appsettings*.`Environment`.*json* files.</span></span>
- <span data-ttu-id="a31f9-226">`Development` 环境中的应用机密（机密管理器）。</span><span class="sxs-lookup"><span data-stu-id="a31f9-226">App secrets (Secret Manager) in the `Development` environment.</span></span>
- <span data-ttu-id="a31f9-227">环境变量。</span><span class="sxs-lookup"><span data-stu-id="a31f9-227">Environment variables.</span></span>

<span data-ttu-id="a31f9-228">默认情况下，在命令行上设置的配置值会替代通过所有其他配置提供程序设置的配置值。</span><span class="sxs-lookup"><span data-stu-id="a31f9-228">By default, configuration values set on the command line override configuration values set with all the other configuration providers.</span></span>

### <a name="command-line-arguments"></a><span data-ttu-id="a31f9-229">命令行参数</span><span class="sxs-lookup"><span data-stu-id="a31f9-229">Command-line arguments</span></span>

<span data-ttu-id="a31f9-230">以下命令使用 `=` 设置键和值：</span><span class="sxs-lookup"><span data-stu-id="a31f9-230">The following command sets keys and values using `=`:</span></span>

```dotnetcli
dotnet run SecretKey="Secret key from command line"
```

<span data-ttu-id="a31f9-231">以下命令使用 `/` 设置键和值：</span><span class="sxs-lookup"><span data-stu-id="a31f9-231">The following command sets keys and values using `/`:</span></span>

```dotnetcli
dotnet run /SecretKey "Secret key set from forward slash"
```

<span data-ttu-id="a31f9-232">以下命令使用 `--` 设置键和值：</span><span class="sxs-lookup"><span data-stu-id="a31f9-232">The following command sets keys and values using `--`:</span></span>

```dotnetcli
dotnet run --SecretKey "Secret key set from double hyphen"
```

<span data-ttu-id="a31f9-233">键值：</span><span class="sxs-lookup"><span data-stu-id="a31f9-233">The key value:</span></span>

- <span data-ttu-id="a31f9-234">必须后跟 `=`，或者当值后跟一个空格时，键必须具有一个 `--` 或 `/` 的前缀。</span><span class="sxs-lookup"><span data-stu-id="a31f9-234">Must follow `=`, or the key must have a prefix of `--` or `/` when the value follows a space.</span></span>
- <span data-ttu-id="a31f9-235">如果使用 `=`，则不是必需的。</span><span class="sxs-lookup"><span data-stu-id="a31f9-235">Isn't required if `=` is used.</span></span> <span data-ttu-id="a31f9-236">例如 `SomeKey=`。</span><span class="sxs-lookup"><span data-stu-id="a31f9-236">For example, `SomeKey=`.</span></span>

<span data-ttu-id="a31f9-237">在同一命令中，请勿将使用 `=` 的命令行参数键值对与使用空格的键值对混合使用。</span><span class="sxs-lookup"><span data-stu-id="a31f9-237">Within the same command, don't mix command-line argument key-value pairs that use `=` with key-value pairs that use a space.</span></span>

## <a name="key-per-file-configuration-provider"></a><span data-ttu-id="a31f9-238">Key-per-file 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-238">Key-per-file configuration provider</span></span>

<span data-ttu-id="a31f9-239"><xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> 使用目录的文件作为配置键值对。</span><span class="sxs-lookup"><span data-stu-id="a31f9-239">The <xref:Microsoft.Extensions.Configuration.KeyPerFile.KeyPerFileConfigurationProvider> uses a directory's files as configuration key-value pairs.</span></span> <span data-ttu-id="a31f9-240">该键是文件名。</span><span class="sxs-lookup"><span data-stu-id="a31f9-240">The key is the file name.</span></span> <span data-ttu-id="a31f9-241">该值是文件的内容。</span><span class="sxs-lookup"><span data-stu-id="a31f9-241">The value is the file's contents.</span></span> <span data-ttu-id="a31f9-242">Key-per-file 配置提供程序用于 Docker 托管方案。</span><span class="sxs-lookup"><span data-stu-id="a31f9-242">The Key-per-file configuration provider is used in Docker hosting scenarios.</span></span>

<span data-ttu-id="a31f9-243">若要激活 Key-per-file 配置，请在 <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 的实例上调用 <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile%2A> 扩展方法。</span><span class="sxs-lookup"><span data-stu-id="a31f9-243">To activate key-per-file configuration, call the <xref:Microsoft.Extensions.Configuration.KeyPerFileConfigurationBuilderExtensions.AddKeyPerFile%2A> extension method on an instance of <xref:Microsoft.Extensions.Configuration.ConfigurationBuilder>.</span></span> <span data-ttu-id="a31f9-244">文件的 `directoryPath` 必须是绝对路径。</span><span class="sxs-lookup"><span data-stu-id="a31f9-244">The `directoryPath` to the files must be an absolute path.</span></span>

<span data-ttu-id="a31f9-245">重载允许指定：</span><span class="sxs-lookup"><span data-stu-id="a31f9-245">Overloads permit specifying:</span></span>

- <span data-ttu-id="a31f9-246">配置源的 `Action<KeyPerFileConfigurationSource>` 委托。</span><span class="sxs-lookup"><span data-stu-id="a31f9-246">An `Action<KeyPerFileConfigurationSource>` delegate that configures the source.</span></span>
- <span data-ttu-id="a31f9-247">目录是否可选以及目录的路径。</span><span class="sxs-lookup"><span data-stu-id="a31f9-247">Whether the directory is optional and the path to the directory.</span></span>

<span data-ttu-id="a31f9-248">双下划线字符 (`__`) 用作文件名中的配置键分隔符。</span><span class="sxs-lookup"><span data-stu-id="a31f9-248">The double-underscore (`__`) is used as a configuration key delimiter in file names.</span></span> <span data-ttu-id="a31f9-249">例如，文件名 `Logging__LogLevel__System` 生成配置键 `Logging:LogLevel:System`。</span><span class="sxs-lookup"><span data-stu-id="a31f9-249">For example, the file name `Logging__LogLevel__System` produces the configuration key `Logging:LogLevel:System`.</span></span>

<span data-ttu-id="a31f9-250">构建主机时调用 `ConfigureAppConfiguration` 以指定应用的配置：</span><span class="sxs-lookup"><span data-stu-id="a31f9-250">Call `ConfigureAppConfiguration` when building the host to specify the app's configuration:</span></span>

```csharp
.ConfigureAppConfiguration((_, configuration) =>
{
    var path = Path.Combine(
        Directory.GetCurrentDirectory(), "path/to/files");

    configuration.AddKeyPerFile(directoryPath: path, optional: true);
})
```

## <a name="memory-configuration-provider"></a><span data-ttu-id="a31f9-251">内存配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-251">Memory configuration provider</span></span>

<span data-ttu-id="a31f9-252"><xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> 使用内存中集合作为配置键值对。</span><span class="sxs-lookup"><span data-stu-id="a31f9-252">The <xref:Microsoft.Extensions.Configuration.Memory.MemoryConfigurationProvider> uses an in-memory collection as configuration key-value pairs.</span></span>

<span data-ttu-id="a31f9-253">以下代码将内存集合添加到配置系统中：</span><span class="sxs-lookup"><span data-stu-id="a31f9-253">The following code adds a memory collection to the configuration system:</span></span>

:::code language="csharp" source="snippets/configuration/console-memory/Program.cs" highlight="22-29":::

<span data-ttu-id="a31f9-254">在上述代码中，<xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection(Microsoft.Extensions.Configuration.IConfigurationBuilder,System.Collections.Generic.IEnumerable{System.Collections.Generic.KeyValuePair{System.String,System.String}})?displayProperty=nameWithType> 会在默认配置提供程序之后添加内存提供程序。</span><span class="sxs-lookup"><span data-stu-id="a31f9-254">In the preceding code, <xref:Microsoft.Extensions.Configuration.MemoryConfigurationBuilderExtensions.AddInMemoryCollection(Microsoft.Extensions.Configuration.IConfigurationBuilder,System.Collections.Generic.IEnumerable{System.Collections.Generic.KeyValuePair{System.String,System.String}})?displayProperty=nameWithType> adds the memory provider after the default configuration providers.</span></span> <span data-ttu-id="a31f9-255">有关对配置提供程序进行排序的示例，请查看 [XML 配置提供程序](#xml-configuration-provider)。</span><span class="sxs-lookup"><span data-stu-id="a31f9-255">For an example of ordering the configuration providers, see [XML configuration provider](#xml-configuration-provider).</span></span>

## <a name="see-also"></a><span data-ttu-id="a31f9-256">另请参阅</span><span class="sxs-lookup"><span data-stu-id="a31f9-256">See also</span></span>

- [<span data-ttu-id="a31f9-257">.NET 中的配置</span><span class="sxs-lookup"><span data-stu-id="a31f9-257">Configuration in .NET</span></span>](configuration.md)
- [<span data-ttu-id="a31f9-258">.NET 通用主机</span><span class="sxs-lookup"><span data-stu-id="a31f9-258">.NET Generic Host</span></span>](generic-host.md)
- [<span data-ttu-id="a31f9-259">实现自定义配置提供程序</span><span class="sxs-lookup"><span data-stu-id="a31f9-259">Implement a custom configuration provider</span></span>](custom-configuration-provider.md)
