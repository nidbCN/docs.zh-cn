---
title: .NET 中的配置
description: 了解如何使用配置 API 配置 .NET 应用程序。
author: IEvangelist
ms.author: dapine
ms.date: 09/16/2020
ms.topic: overview
ms.openlocfilehash: 5955e46c2f5acb6776ada4e3fd6a65507d3faa1f
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "102401950"
---
# <a name="configuration-in-net"></a><span data-ttu-id="ed8f3-103">.NET 中的配置</span><span class="sxs-lookup"><span data-stu-id="ed8f3-103">Configuration in .NET</span></span>

<span data-ttu-id="ed8f3-104">.NET 中的配置是使用一个或多个[配置提供程序](#configuration-providers)执行的。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-104">Configuration in .NET is performed using one or more [configuration providers](#configuration-providers).</span></span> <span data-ttu-id="ed8f3-105">配置提供程序使用各种配置源从键值对读取配置数据：</span><span class="sxs-lookup"><span data-stu-id="ed8f3-105">Configuration providers read configuration data from key-value pairs using a variety of configuration sources:</span></span>

- <span data-ttu-id="ed8f3-106">设置文件，例如 appsettings.json</span><span class="sxs-lookup"><span data-stu-id="ed8f3-106">Settings files, such as *appsettings.json*</span></span>
- <span data-ttu-id="ed8f3-107">环境变量</span><span class="sxs-lookup"><span data-stu-id="ed8f3-107">Environment variables</span></span>
- [<span data-ttu-id="ed8f3-108">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ed8f3-108">Azure Key Vault</span></span>](/azure/key-vault/general/overview)
- [<span data-ttu-id="ed8f3-109">Azure 应用配置</span><span class="sxs-lookup"><span data-stu-id="ed8f3-109">Azure App Configuration</span></span>](/azure/azure-app-configuration/overview)
- <span data-ttu-id="ed8f3-110">命令行参数</span><span class="sxs-lookup"><span data-stu-id="ed8f3-110">Command-line arguments</span></span>
- <span data-ttu-id="ed8f3-111">已安装或已创建的自定义提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-111">Custom providers, installed or created</span></span>
- <span data-ttu-id="ed8f3-112">目录文件</span><span class="sxs-lookup"><span data-stu-id="ed8f3-112">Directory files</span></span>
- <span data-ttu-id="ed8f3-113">内存中的 .NET 对象</span><span class="sxs-lookup"><span data-stu-id="ed8f3-113">In-memory .NET objects</span></span>

## <a name="configure-console-apps"></a><span data-ttu-id="ed8f3-114">配置控制台应用</span><span class="sxs-lookup"><span data-stu-id="ed8f3-114">Configure console apps</span></span>

<span data-ttu-id="ed8f3-115">在默认情况下，使用 [dotnet new](../tools/dotnet-new.md) 或 Visual Studio 新建的 .NET 控制台应用程序不会公开配置功能。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-115">New .NET console applications created using [dotnet new](../tools/dotnet-new.md) or Visual Studio by default *do not* expose configuration capabilities.</span></span> <span data-ttu-id="ed8f3-116">若要在新的 .NET 控制台应用程序中添加配置，请[添加](../tools/dotnet-add-package.md)对 `Microsoft.Extensions.Hosting` 的包引用。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-116">To add configuration in a new .NET console application, [add a package reference](../tools/dotnet-add-package.md) to `Microsoft.Extensions.Hosting`.</span></span> <span data-ttu-id="ed8f3-117">修改 Program.cs 文件，使其与以下代码相匹配：</span><span class="sxs-lookup"><span data-stu-id="ed8f3-117">Modify the *Program.cs* file to match the following code:</span></span>

:::code language="csharp" source="snippets/configuration/console/Program.cs" highlight="18":::

<span data-ttu-id="ed8f3-118"><xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder(System.String[])?displayProperty=nameWithType> 方法按照以下顺序为应用提供默认配置：</span><span class="sxs-lookup"><span data-stu-id="ed8f3-118">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder(System.String[])?displayProperty=nameWithType> method provides default configuration for the app in the following order:</span></span>

1. <span data-ttu-id="ed8f3-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource)：添加现有的 `IConfiguration` 作为源。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-119">[ChainedConfigurationProvider](xref:Microsoft.Extensions.Configuration.ChainedConfigurationSource) : Adds an existing `IConfiguration` as a source.</span></span>
1. <span data-ttu-id="ed8f3-120">使用 [JSON 配置提供程序](configuration-providers.md#file-configuration-provider)通过 *appsettings.json* 提供。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-120">*appsettings.json* using the [JSON configuration provider](configuration-providers.md#file-configuration-provider).</span></span>
1. <span data-ttu-id="ed8f3-121">使用 [JSON 配置提供程序](configuration-providers.md#file-configuration-provider)通过 appsettings.`Environment`.json 提供 。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-121">*appsettings.*`Environment`*.json* using the [JSON configuration provider](configuration-providers.md#file-configuration-provider).</span></span> <span data-ttu-id="ed8f3-122">例如，appsettings.Production.json 和 appsettings.Development.json 。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-122">For example, *appsettings*.***Production\*\*_._json* and *appsettings*.\*\*\*Development** _._json\*.</span></span>
1. <span data-ttu-id="ed8f3-123">应用在 `Development` 环境中运行时的应用机密。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-123">App secrets when the app runs in the `Development` environment.</span></span>
1. <span data-ttu-id="ed8f3-124">使用[环境变量配置提供程序](configuration-providers.md#environment-variable-configuration-provider)通过环境变量提供。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-124">Environment variables using the [Environment Variables configuration provider](configuration-providers.md#environment-variable-configuration-provider).</span></span>
1. <span data-ttu-id="ed8f3-125">使用[命令行配置提供程序](configuration-providers.md#command-line-configuration-provider)通过命令行参数提供。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-125">Command-line arguments using the [Command-line configuration provider](configuration-providers.md#command-line-configuration-provider).</span></span>

<span data-ttu-id="ed8f3-126">后来添加的配置提供程序会替代之前的密钥设置。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-126">Configuration providers that are added later override previous key settings.</span></span> <span data-ttu-id="ed8f3-127">例如，如果在 appsettings.json 和环境中设置了 `SomeKey`，则会使用环境值。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-127">For example, if `SomeKey` is set in both *appsettings.json* and the environment, the environment value is used.</span></span> <span data-ttu-id="ed8f3-128">通过默认配置提供程序，[命令行配置提供程序](configuration-providers.md#command-line-configuration-provider)将替代其他所有提供程序。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-128">Using the default configuration providers, the [Command-line configuration provider](configuration-providers.md#command-line-configuration-provider) overrides all other providers.</span></span>

### <a name="binding"></a><span data-ttu-id="ed8f3-129">绑定</span><span class="sxs-lookup"><span data-stu-id="ed8f3-129">Binding</span></span>

<span data-ttu-id="ed8f3-130">.NET 中的配置的其中一个关键优点是，可将配置值绑定到 .NET 对象的实例。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-130">One of the key advantages to configuration in .NET is the ability to bind configuration values to instances of .NET objects.</span></span> <span data-ttu-id="ed8f3-131">例如，JSON 配置提供程序可用于将 appsettings.json 文件映射到 .NET 对象中并与依赖注入一起使用。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-131">For example, the JSON configuration provider can be used to map *appsettings.json* files to .NET objects and is used with dependency injection.</span></span> <span data-ttu-id="ed8f3-132">此可实现选项模式，后者使用类来提供对相关设置组的强类型访问。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-132">This enables the options pattern, the options pattern uses classes to provide strongly typed access to groups of related settings.</span></span>

## <a name="configuration-providers"></a><span data-ttu-id="ed8f3-133">配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-133">Configuration providers</span></span>

<span data-ttu-id="ed8f3-134">下表显示了 .NET Core 应用可用的配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-134">The following table shows the configuration providers available to .NET Core apps.</span></span>

| <span data-ttu-id="ed8f3-135">提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-135">Provider</span></span>                                                                                                               | <span data-ttu-id="ed8f3-136">通过以下对象提供配置</span><span class="sxs-lookup"><span data-stu-id="ed8f3-136">Provides configuration from</span></span>        |
|------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| [<span data-ttu-id="ed8f3-137">Azure 应用配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-137">Azure App configuration provider</span></span>](/azure/azure-app-configuration/quickstart-aspnet-core-app)                          | <span data-ttu-id="ed8f3-138">Azure 应用程序配置</span><span class="sxs-lookup"><span data-stu-id="ed8f3-138">Azure App Configuration</span></span>            |
| [<span data-ttu-id="ed8f3-139">Azure Key Vault 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-139">Azure Key Vault configuration provider</span></span>](/azure/key-vault/general/tutorial-net-virtual-machine)                        | <span data-ttu-id="ed8f3-140">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="ed8f3-140">Azure Key Vault</span></span>                    |
| [<span data-ttu-id="ed8f3-141">命令行配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-141">Command-line configuration provider</span></span>](configuration-providers.md#command-line-configuration-provider)                  | <span data-ttu-id="ed8f3-142">命令行参数</span><span class="sxs-lookup"><span data-stu-id="ed8f3-142">Command-line parameters</span></span>            |
| [<span data-ttu-id="ed8f3-143">自定义配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-143">Custom configuration provider</span></span>](custom-configuration-provider.md)                                                      | <span data-ttu-id="ed8f3-144">自定义源</span><span class="sxs-lookup"><span data-stu-id="ed8f3-144">Custom source</span></span>                      |
| [<span data-ttu-id="ed8f3-145">环境变量配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-145">Environment Variables configuration provider</span></span>](configuration-providers.md#environment-variable-configuration-provider) | <span data-ttu-id="ed8f3-146">环境变量</span><span class="sxs-lookup"><span data-stu-id="ed8f3-146">Environment variables</span></span>              |
| [<span data-ttu-id="ed8f3-147">文件配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-147">File configuration provider</span></span>](configuration-providers.md#file-configuration-provider)                                  | <span data-ttu-id="ed8f3-148">JSON、XML 和 INI 文件</span><span class="sxs-lookup"><span data-stu-id="ed8f3-148">JSON, XML, and INI files</span></span>           |
| [<span data-ttu-id="ed8f3-149">Key-per-file 配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-149">Key-per-file configuration provider</span></span>](configuration-providers.md#key-per-file-configuration-provider)                  | <span data-ttu-id="ed8f3-150">目录文件</span><span class="sxs-lookup"><span data-stu-id="ed8f3-150">Directory files</span></span>                    |
| [<span data-ttu-id="ed8f3-151">内存配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-151">Memory configuration provider</span></span>](configuration-providers.md#memory-configuration-provider)                              | <span data-ttu-id="ed8f3-152">内存中集合</span><span class="sxs-lookup"><span data-stu-id="ed8f3-152">In-memory collections</span></span>              |
| [<span data-ttu-id="ed8f3-153">应用机密（机密管理器）</span><span class="sxs-lookup"><span data-stu-id="ed8f3-153">App secrets (Secret Manager)</span></span>](/aspnet/core/security/app-secrets)                                                      | <span data-ttu-id="ed8f3-154">用户配置文件目录中的文件</span><span class="sxs-lookup"><span data-stu-id="ed8f3-154">File in the user profile directory</span></span> |

<span data-ttu-id="ed8f3-155">若要详细了解各种配置提供程序，请查看 [.NET 中的配置提供程序](configuration-providers.md)。</span><span class="sxs-lookup"><span data-stu-id="ed8f3-155">For more information on various configuration providers, see [Configuration providers in .NET](configuration-providers.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="ed8f3-156">另请参阅</span><span class="sxs-lookup"><span data-stu-id="ed8f3-156">See also</span></span>

- [<span data-ttu-id="ed8f3-157">.NET 中的配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-157">Configuration providers in .NET</span></span>](configuration-providers.md)
- [<span data-ttu-id="ed8f3-158">实现自定义配置提供程序</span><span class="sxs-lookup"><span data-stu-id="ed8f3-158">Implement a custom configuration provider</span></span>](custom-configuration-provider.md)
- <span data-ttu-id="ed8f3-159">配置 bug 应在 [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) 存储库中创建</span><span class="sxs-lookup"><span data-stu-id="ed8f3-159">Configuration bugs should be created in the [github.com/dotnet/extensions](https://github.com/dotnet/extensions/issues) repo</span></span>
