---
title: 在 .NET 中实现自定义配置提供程序
description: 了解如何在 .NET 应用程序中实现自定义配置提供程序。
author: IEvangelist
ms.author: dapine
ms.date: 12/04/2020
ms.topic: how-to
ms.openlocfilehash: 22e46b7df8b02421633d6be251d990879baa8b2b
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "102401949"
---
# <a name="implement-a-custom-configuration-provider-in-net"></a><span data-ttu-id="977b3-103">在 .NET 中实现自定义配置提供程序</span><span class="sxs-lookup"><span data-stu-id="977b3-103">Implement a custom configuration provider in .NET</span></span>

<span data-ttu-id="977b3-104">有许多[配置提供程序](configuration-providers.md)可用于常见的配置源，如 JSON、XML 和 INI 文件。</span><span class="sxs-lookup"><span data-stu-id="977b3-104">There are many [configuration providers](configuration-providers.md) available for common configuration sources such as JSON, XML, and INI files.</span></span> <span data-ttu-id="977b3-105">如果某个可用的提供程序不满足你的应用程序需要，说明你可能需要实现自定义配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="977b3-105">You may need to implement a custom configuration provider when one of the available providers doesn't suit your application needs.</span></span> <span data-ttu-id="977b3-106">在本文中，你将了解如何实现一个依赖数据库作为配置源的自定义配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="977b3-106">In this article, you'll learn how to implement a custom configuration provider that relies on a database as its configuration source.</span></span>

## <a name="custom-configuration-provider"></a><span data-ttu-id="977b3-107">自定义配置提供程序</span><span class="sxs-lookup"><span data-stu-id="977b3-107">Custom configuration provider</span></span>

<span data-ttu-id="977b3-108">此示例应用演示了如何使用[实体框架 (EF) Core](/ef/core) 创建从数据库读取配置键值对的基本配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="977b3-108">The sample app demonstrates how to create a basic configuration provider that reads configuration key-value pairs from a database using [Entity Framework (EF) Core](/ef/core).</span></span>

<span data-ttu-id="977b3-109">提供程序具有以下特征：</span><span class="sxs-lookup"><span data-stu-id="977b3-109">The provider has the following characteristics:</span></span>

- <span data-ttu-id="977b3-110">EF 内存中数据库用于演示目的。</span><span class="sxs-lookup"><span data-stu-id="977b3-110">The EF in-memory database is used for demonstration purposes.</span></span> <span data-ttu-id="977b3-111">若要使用需要连接字符串的数据库，请实现辅助 `ConfigurationBuilder` 以从另一个配置提供程序提供连接字符串。</span><span class="sxs-lookup"><span data-stu-id="977b3-111">To use a database that requires a connection string, implement a secondary `ConfigurationBuilder` to supply the connection string from another configuration provider.</span></span>
- <span data-ttu-id="977b3-112">提供程序在启动时将数据库表读入配置。</span><span class="sxs-lookup"><span data-stu-id="977b3-112">The provider reads a database table into configuration at startup.</span></span> <span data-ttu-id="977b3-113">提供程序不会基于每个键查询数据库。</span><span class="sxs-lookup"><span data-stu-id="977b3-113">The provider doesn't query the database on a per-key basis.</span></span>
- <span data-ttu-id="977b3-114">未实现更改时重载，因此在应用启动后更新数据库对应用的配置没有任何影响。</span><span class="sxs-lookup"><span data-stu-id="977b3-114">Reload-on-change isn't implemented, so updating the database after the app starts has no effect on the app's configuration.</span></span>

<span data-ttu-id="977b3-115">定义一个 `Settings` 记录类型实体，用于在数据库中存储配置值。</span><span class="sxs-lookup"><span data-stu-id="977b3-115">Define a `Settings` record type entity for storing configuration values in the database.</span></span> <span data-ttu-id="977b3-116">例如，可以将 Settings.cs 文件添加到“Models”文件夹中：</span><span class="sxs-lookup"><span data-stu-id="977b3-116">For example, you could add a *Settings.cs* file in your *Models* folder:</span></span>

:::code language="csharp" source="snippets/configuration/custom-provider/Models/Settings.cs":::

<span data-ttu-id="977b3-117">有关记录类型的信息，请参阅 [C# 9 中的记录类型](../../csharp/whats-new/csharp-9.md#record-types)。</span><span class="sxs-lookup"><span data-stu-id="977b3-117">For information on record types, see [Record types in C# 9](../../csharp/whats-new/csharp-9.md#record-types).</span></span>

<span data-ttu-id="977b3-118">添加 `EntityConfigurationContext` 以存储和访问配置的值。</span><span class="sxs-lookup"><span data-stu-id="977b3-118">Add an `EntityConfigurationContext` to store and access the configured values.</span></span>

<span data-ttu-id="977b3-119">Providers/EntityConfigurationContext.cs：</span><span class="sxs-lookup"><span data-stu-id="977b3-119">*Providers/EntityConfigurationContext.cs*:</span></span>

:::code language="csharp" source="snippets/configuration/custom-provider/Providers/EntityConfigurationContext.cs":::

<span data-ttu-id="977b3-120">创建用于实现 <xref:Microsoft.Extensions.Configuration.IConfigurationSource> 的类。</span><span class="sxs-lookup"><span data-stu-id="977b3-120">Create a class that implements <xref:Microsoft.Extensions.Configuration.IConfigurationSource>.</span></span>

<span data-ttu-id="977b3-121">Providers/EntityConfigurationSource.cs：</span><span class="sxs-lookup"><span data-stu-id="977b3-121">*Providers/EntityConfigurationSource.cs*:</span></span>

:::code language="csharp" source="snippets/configuration/custom-provider/Providers/EntityConfigurationSource.cs":::

<span data-ttu-id="977b3-122">通过从 <xref:Microsoft.Extensions.Configuration.ConfigurationProvider> 继承来创建自定义配置提供程序。</span><span class="sxs-lookup"><span data-stu-id="977b3-122">Create the custom configuration provider by inheriting from <xref:Microsoft.Extensions.Configuration.ConfigurationProvider>.</span></span> <span data-ttu-id="977b3-123">当数据库为空时，配置提供程序将对其进行初始化。</span><span class="sxs-lookup"><span data-stu-id="977b3-123">The configuration provider initializes the database when it's empty.</span></span> <span data-ttu-id="977b3-124">由于配置密钥不区分大小写，因此用来初始化数据库的字典是用不区分大小写的比较程序 ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)) 创建的。</span><span class="sxs-lookup"><span data-stu-id="977b3-124">Since configuration keys are case-insensitive, the dictionary used to initialize the database is created with the case-insensitive comparer ([StringComparer.OrdinalIgnoreCase](xref:System.StringComparer.OrdinalIgnoreCase)).</span></span>

<span data-ttu-id="977b3-125">Providers/EntityConfigurationProvider.cs：</span><span class="sxs-lookup"><span data-stu-id="977b3-125">*Providers/EntityConfigurationProvider.cs*:</span></span>

:::code language="csharp" source="snippets/configuration/custom-provider/Providers/EntityConfigurationProvider.cs":::

<span data-ttu-id="977b3-126">可以使用 `AddEntityConfiguration` 扩展方法将配置源添加到 <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> 实例中。</span><span class="sxs-lookup"><span data-stu-id="977b3-126">An `AddEntityConfiguration` extension method permits adding the configuration source to a <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> instance.</span></span>

<span data-ttu-id="977b3-127">Extensions/ConfigurationBuilderExtensions.cs：</span><span class="sxs-lookup"><span data-stu-id="977b3-127">*Extensions/ConfigurationBuilderExtensions.cs*:</span></span>

:::code language="csharp" source="snippets/configuration/custom-provider/Extensions/ConfigurationBuilderExtensions.cs":::

<span data-ttu-id="977b3-128">下面的代码演示如何在 Program.cs 中使用自定义的 `EntityConfigurationProvider`：</span><span class="sxs-lookup"><span data-stu-id="977b3-128">The following code shows how to use the custom `EntityConfigurationProvider` in *Program.cs*:</span></span>

:::code language="csharp" source="snippets/configuration/custom-provider/Program.cs" highlight="27-28":::

## <a name="see-also"></a><span data-ttu-id="977b3-129">另请参阅</span><span class="sxs-lookup"><span data-stu-id="977b3-129">See also</span></span>

- [<span data-ttu-id="977b3-130">.NET 中的配置</span><span class="sxs-lookup"><span data-stu-id="977b3-130">Configuration in .NET</span></span>](configuration.md)
- [<span data-ttu-id="977b3-131">.NET 中的配置提供程序</span><span class="sxs-lookup"><span data-stu-id="977b3-131">Configuration providers in .NET</span></span>](configuration-providers.md)
