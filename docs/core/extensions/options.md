---
title: .NET 中的选项模式
author: IEvangelist
description: 了解如何使用选项模式来表示 .NET 应用中的相关设置组。
ms.author: dapine
ms.date: 02/18/2021
ms.openlocfilehash: df30928cdb1832e94f89abbdf8cc41e1304f8e2e
ms.sourcegitcommit: bdbf6472de867a0a11aaa5b9384a2506c24f27d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102401982"
---
# <a name="options-pattern-in-net"></a><span data-ttu-id="4698d-103">.NET 中的选项模式</span><span class="sxs-lookup"><span data-stu-id="4698d-103">Options pattern in .NET</span></span>

<span data-ttu-id="4698d-104">选项模式使用类来提供对相关设置组的强类型访问。</span><span class="sxs-lookup"><span data-stu-id="4698d-104">The options pattern uses classes to provide strongly-typed access to groups of related settings.</span></span> <span data-ttu-id="4698d-105">当[配置设置](configuration.md)由方案隔离到单独的类时，应用遵循两个重要软件工程原则：</span><span class="sxs-lookup"><span data-stu-id="4698d-105">When [configuration settings](configuration.md) are isolated by scenario into separate classes, the app adheres to two important software engineering principles:</span></span>

- <span data-ttu-id="4698d-106">[接口分隔原则 (ISP) 或封装](../../architecture/modern-web-apps-azure/architectural-principles.md#encapsulation)：依赖于配置设置的方案（类）仅依赖于其使用的配置设置。</span><span class="sxs-lookup"><span data-stu-id="4698d-106">The [Interface Segregation Principle (ISP) or Encapsulation](../../architecture/modern-web-apps-azure/architectural-principles.md#encapsulation): Scenarios (classes) that depend on configuration settings depend only on the configuration settings that they use.</span></span>
- <span data-ttu-id="4698d-107">[关注点分离](../../architecture/modern-web-apps-azure/architectural-principles.md#separation-of-concerns)：应用的不同部件的设置不彼此依赖或相互耦合。</span><span class="sxs-lookup"><span data-stu-id="4698d-107">[Separation of Concerns](../../architecture/modern-web-apps-azure/architectural-principles.md#separation-of-concerns): Settings for different parts of the app aren't dependent or coupled to one another.</span></span>

<span data-ttu-id="4698d-108">选项还提供验证配置数据的机制。</span><span class="sxs-lookup"><span data-stu-id="4698d-108">Options also provide a mechanism to validate configuration data.</span></span> <span data-ttu-id="4698d-109">有关详细信息，请参阅[选项验证](#options-validation)部分。</span><span class="sxs-lookup"><span data-stu-id="4698d-109">For more information, see the [Options validation](#options-validation) section.</span></span>

## <a name="bind-hierarchical-configuration"></a><span data-ttu-id="4698d-110">绑定分层配置</span><span class="sxs-lookup"><span data-stu-id="4698d-110">Bind hierarchical configuration</span></span>

<span data-ttu-id="4698d-111">读取相关配置值的首选方法是使用选项模式。</span><span class="sxs-lookup"><span data-stu-id="4698d-111">The preferred way to read related configuration values is using the options pattern.</span></span> <span data-ttu-id="4698d-112">选项模式可以通过 <xref:Microsoft.Extensions.Options.IOptions%601> 接口实现，其中泛型类型参数 `TOptions` 被约束为 `class`。</span><span class="sxs-lookup"><span data-stu-id="4698d-112">The options pattern is possible through the <xref:Microsoft.Extensions.Options.IOptions%601> interface, where the generic type parameter `TOptions` is constrained to `class`.</span></span> <span data-ttu-id="4698d-113">以后可以通过依赖关系注入来提供 `IOptions<TOptions>`。</span><span class="sxs-lookup"><span data-stu-id="4698d-113">The `IOptions<TOptions>` can later be provided through dependency injection.</span></span> <span data-ttu-id="4698d-114">有关详细信息，请参阅 [.NET 中的依赖关系注入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="4698d-114">For more information, see [Dependency injection in .NET](dependency-injection.md).</span></span>

<span data-ttu-id="4698d-115">例如，若要读取以下配置值，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="4698d-115">For example, to read the following configuration values:</span></span>

:::code language="json" source="snippets/configuration/console-json/appsettings.json" range="3-6":::

<span data-ttu-id="4698d-116">创建以下 `TransientFaultHandlingOptions` 类：</span><span class="sxs-lookup"><span data-stu-id="4698d-116">Create the following `TransientFaultHandlingOptions` class:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/TransientFaultHandlingOptions.cs" range="5-9":::

<span data-ttu-id="4698d-117"><span id="options-class"></span>在使用选项模式时，选项类：</span><span class="sxs-lookup"><span data-stu-id="4698d-117"><span id="options-class"></span> When using the options pattern, an options class:</span></span>

- <span data-ttu-id="4698d-118">必须是非抽象类，有一个公共无参数构造函数</span><span class="sxs-lookup"><span data-stu-id="4698d-118">Must be non-abstract with a public parameterless constructor</span></span>
- <span data-ttu-id="4698d-119">包含要绑定的公共读写属性（字段不绑定）</span><span class="sxs-lookup"><span data-stu-id="4698d-119">Contain public read-write properties to bind (fields are ***not*** bound)</span></span>

<span data-ttu-id="4698d-120">下面的代码：</span><span class="sxs-lookup"><span data-stu-id="4698d-120">The following code:</span></span>

* <span data-ttu-id="4698d-121">调用 [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind%2A) 将 `TransientFaultHandlingOptions` 类绑定到 `"TransientFaultHandlingOptions"` 部分。</span><span class="sxs-lookup"><span data-stu-id="4698d-121">Calls [ConfigurationBinder.Bind](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind%2A) to bind the `TransientFaultHandlingOptions` class to the `"TransientFaultHandlingOptions"` section.</span></span>
* <span data-ttu-id="4698d-122">显示配置数据。</span><span class="sxs-lookup"><span data-stu-id="4698d-122">Displays the configuration data.</span></span>

:::code language="csharp" source="snippets/configuration/console-json/Program.cs" range="31-38":::

<span data-ttu-id="4698d-123">在上面的代码中，已读取在应用启动后对 JSON 配置文件所做的更改。</span><span class="sxs-lookup"><span data-stu-id="4698d-123">In the preceding code, changes to the JSON configuration file after the app has started are read.</span></span>

<span data-ttu-id="4698d-124">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get%2A) 绑定并返回指定的类型。</span><span class="sxs-lookup"><span data-stu-id="4698d-124">[`ConfigurationBinder.Get<T>`](xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Get%2A) binds and returns the specified type.</span></span> <span data-ttu-id="4698d-125">使用 `ConfigurationBinder.Get<T>` 可能比使用 `ConfigurationBinder.Bind` 更方便。</span><span class="sxs-lookup"><span data-stu-id="4698d-125">`ConfigurationBinder.Get<T>` may be more convenient than using `ConfigurationBinder.Bind`.</span></span> <span data-ttu-id="4698d-126">下面的代码演示如何将 `ConfigurationBinder.Get<T>` 与 `TransientFaultHandlingOptions` 类配合使用：</span><span class="sxs-lookup"><span data-stu-id="4698d-126">The following code shows how to use `ConfigurationBinder.Get<T>` with the `TransientFaultHandlingOptions` class:</span></span>

```csharp
IConfigurationRoot configurationRoot = configuration.Build();

var options =
    configurationRoot.GetSection(nameof(TransientFaultHandlingOptions))
                     .Get<TransientFaultHandlingOptions>();

Console.WriteLine($"TransientFaultHandlingOptions.Enabled={options.Enabled}");
Console.WriteLine($"TransientFaultHandlingOptions.AutoRetryDelay={options.AutoRetryDelay}");
```

<span data-ttu-id="4698d-127">在上面的代码中，已读取在应用启动后对 JSON 配置文件所做的更改。</span><span class="sxs-lookup"><span data-stu-id="4698d-127">In the preceding code, changes to the JSON configuration file after the app has started are read.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4698d-128"><xref:Microsoft.Extensions.Configuration.ConfigurationBinder> 类公开了几个 API，例如不被约束为 `class` 的 `.Bind(object instance)` 和 `.Get<T>()`。</span><span class="sxs-lookup"><span data-stu-id="4698d-128">The <xref:Microsoft.Extensions.Configuration.ConfigurationBinder> class exposes several APIs, such as `.Bind(object instance)` and `.Get<T>()` that are ***not*** constrained to `class`.</span></span> <span data-ttu-id="4698d-129">使用任何一个[选项接口](#options-interfaces)时，都必须遵守前面提到的[选项类约束](#options-class)。</span><span class="sxs-lookup"><span data-stu-id="4698d-129">When using any of the [Options interfaces](#options-interfaces), you must adhere to aforementioned [options class constraints](#options-class).</span></span>

<span data-ttu-id="4698d-130">使用选项模式时的另一种方法是绑定 `"TransientFaultHandlingOptions"` 部分，并将其添加到[依赖关系注入服务容器](dependency-injection.md)中。</span><span class="sxs-lookup"><span data-stu-id="4698d-130">An alternative approach when using the options pattern is to bind the `"TransientFaultHandlingOptions"` section and add it to the [dependency injection service container](dependency-injection.md).</span></span> <span data-ttu-id="4698d-131">在以下代码中，`TransientFaultHandlingOptions` 已通过 <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure%2A> 被添加到了服务容器并已绑定到了配置：</span><span class="sxs-lookup"><span data-stu-id="4698d-131">In the following code, `TransientFaultHandlingOptions` is added to the service container with <xref:Microsoft.Extensions.DependencyInjection.OptionsConfigurationServiceCollectionExtensions.Configure%2A> and bound to configuration:</span></span>

```csharp
services.Configure<TransientFaultHandlingOptions>(
    configurationRoot.GetSection(
        key: nameof(TransientFaultHandlingOptions)));
```

> [!TIP]
> <span data-ttu-id="4698d-132">`key` 参数是要搜索的配置部分的名称。</span><span class="sxs-lookup"><span data-stu-id="4698d-132">The `key` parameter is the name of the configuration section to search for.</span></span> <span data-ttu-id="4698d-133">它不必与代表它的类型名称相匹配。</span><span class="sxs-lookup"><span data-stu-id="4698d-133">It does *not* have to match the name of the type that represents it.</span></span> <span data-ttu-id="4698d-134">例如，你可以有一个名为 `"FaultHandling"` 的部分，该部分可以由 `TransientFaultHandlingOptions` 类来表示。</span><span class="sxs-lookup"><span data-stu-id="4698d-134">For example, you could have a section named `"FaultHandling"` and it could be represented by the `TransientFaultHandlingOptions` class.</span></span> <span data-ttu-id="4698d-135">在这种情况下，可以改为将 `"FaultHandling"` 传递到 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> 函数。</span><span class="sxs-lookup"><span data-stu-id="4698d-135">In this instance, you'd pass `"FaultHandling"` to the <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A> function instead.</span></span> <span data-ttu-id="4698d-136">在命名部分与其对应的类型相匹配时，为了方便，使用 `nameof` 运算符。</span><span class="sxs-lookup"><span data-stu-id="4698d-136">The `nameof` operator is used as a convenience when the named section matches the type it corresponds to.</span></span>

<span data-ttu-id="4698d-137">通过使用前面的代码，以下代码将读取位置选项：</span><span class="sxs-lookup"><span data-stu-id="4698d-137">Using the preceding code, the following code reads the position options:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/ExampleService.cs":::

<span data-ttu-id="4698d-138">在上面的代码中，不会读取在应用启动后对 JSON 配置文件所做的更改。</span><span class="sxs-lookup"><span data-stu-id="4698d-138">In the preceding code, changes to the JSON configuration file after the app has started are ***not*** read.</span></span> <span data-ttu-id="4698d-139">若要读取在应用启动后的更改，请使用 [IOptionsSnapshot](#use-ioptionssnapshot-to-read-updated-data)。</span><span class="sxs-lookup"><span data-stu-id="4698d-139">To read changes after the app has started, use [IOptionsSnapshot](#use-ioptionssnapshot-to-read-updated-data).</span></span>

## <a name="options-interfaces"></a><span data-ttu-id="4698d-140">选项接口</span><span class="sxs-lookup"><span data-stu-id="4698d-140">Options interfaces</span></span>

<span data-ttu-id="4698d-141"><xref:Microsoft.Extensions.Options.IOptions%601>:</span><span class="sxs-lookup"><span data-stu-id="4698d-141"><xref:Microsoft.Extensions.Options.IOptions%601>:</span></span>

- <span data-ttu-id="4698d-142">不支持：</span><span class="sxs-lookup"><span data-stu-id="4698d-142">Does ***not*** support:</span></span>
  - <span data-ttu-id="4698d-143">在应用启动后读取配置数据。</span><span class="sxs-lookup"><span data-stu-id="4698d-143">Reading of configuration data after the app has started.</span></span>
  - [<span data-ttu-id="4698d-144">命名选项</span><span class="sxs-lookup"><span data-stu-id="4698d-144">Named options</span></span>](#named-options-support-using-iconfigurenamedoptions)
- <span data-ttu-id="4698d-145">注册为[单一实例](dependency-injection.md#singleton)且可以注入到任何[服务生存期](dependency-injection.md#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="4698d-145">Is registered as a [Singleton](dependency-injection.md#singleton) and can be injected into any [service lifetime](dependency-injection.md#service-lifetimes).</span></span>

<span data-ttu-id="4698d-146"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>:</span><span class="sxs-lookup"><span data-stu-id="4698d-146"><xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>:</span></span>

- <span data-ttu-id="4698d-147">对于在[一定范围内或暂时生存期](dependency-injection.md#service-lifetimes)中应对每个注入解析重新计算选项的场景，它非常有用。</span><span class="sxs-lookup"><span data-stu-id="4698d-147">Is useful in scenarios where options should be recomputed on every injection resolution, in [scoped or transient lifetimes](dependency-injection.md#service-lifetimes).</span></span> <span data-ttu-id="4698d-148">有关详细信息，请参阅[使用 IOptionsSnapshot 读取已更新的数据](#use-ioptionssnapshot-to-read-updated-data)。</span><span class="sxs-lookup"><span data-stu-id="4698d-148">For more information, see [Use IOptionsSnapshot to read updated data](#use-ioptionssnapshot-to-read-updated-data).</span></span>
- <span data-ttu-id="4698d-149">注册为[范围内](dependency-injection.md#scoped)，因此无法注入到单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="4698d-149">Is registered as [Scoped](dependency-injection.md#scoped) and therefore cannot be injected into a Singleton service.</span></span>
- <span data-ttu-id="4698d-150">支持[命名选项](#named-options-support-using-iconfigurenamedoptions)</span><span class="sxs-lookup"><span data-stu-id="4698d-150">Supports [named options](#named-options-support-using-iconfigurenamedoptions)</span></span>

<span data-ttu-id="4698d-151"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>:</span><span class="sxs-lookup"><span data-stu-id="4698d-151"><xref:Microsoft.Extensions.Options.IOptionsMonitor%601>:</span></span>

- <span data-ttu-id="4698d-152">用于检索选项并管理 `TOptions` 实例的选项通知。</span><span class="sxs-lookup"><span data-stu-id="4698d-152">Is used to retrieve options and manage options notifications for `TOptions` instances.</span></span>
- <span data-ttu-id="4698d-153">注册为[单一实例](dependency-injection.md#singleton)且可以注入到任何[服务生存期](dependency-injection.md#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="4698d-153">Is registered as a [Singleton](dependency-injection.md#singleton) and can be injected into any [service lifetime](dependency-injection.md#service-lifetimes).</span></span>
- <span data-ttu-id="4698d-154">支持：</span><span class="sxs-lookup"><span data-stu-id="4698d-154">Supports:</span></span>
  - <span data-ttu-id="4698d-155">更改通知</span><span class="sxs-lookup"><span data-stu-id="4698d-155">Change notifications</span></span>
  - [<span data-ttu-id="4698d-156">命名选项</span><span class="sxs-lookup"><span data-stu-id="4698d-156">Named options</span></span>](#named-options-support-using-iconfigurenamedoptions)
  - [<span data-ttu-id="4698d-157">可重载配置</span><span class="sxs-lookup"><span data-stu-id="4698d-157">Reloadable configuration</span></span>](#use-ioptionssnapshot-to-read-updated-data)
  - <span data-ttu-id="4698d-158">选择性选项失效 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span><span class="sxs-lookup"><span data-stu-id="4698d-158">Selective options invalidation (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601>)</span></span>

<span data-ttu-id="4698d-159"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 负责新建选项实例。</span><span class="sxs-lookup"><span data-stu-id="4698d-159"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> is responsible for creating new options instances.</span></span> <span data-ttu-id="4698d-160">它具有单个 <xref:Microsoft.Extensions.Options.IOptionsFactory%601.Create%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="4698d-160">It has a single <xref:Microsoft.Extensions.Options.IOptionsFactory%601.Create%2A> method.</span></span> <span data-ttu-id="4698d-161">默认实现采用所有已注册 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 和 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 并首先运行所有配置，然后才进行后期配置。</span><span class="sxs-lookup"><span data-stu-id="4698d-161">The default implementation takes all registered <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> and runs all the configurations first, followed by the post-configuration.</span></span> <span data-ttu-id="4698d-162">它区分 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 和 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 且仅调用适当的接口。</span><span class="sxs-lookup"><span data-stu-id="4698d-162">It distinguishes between <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and <xref:Microsoft.Extensions.Options.IConfigureOptions%601> and only calls the appropriate interface.</span></span>

<span data-ttu-id="4698d-163"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 由 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> 用于缓存 `TOptions` 实例。</span><span class="sxs-lookup"><span data-stu-id="4698d-163"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> is used by <xref:Microsoft.Extensions.Options.IOptionsMonitor%601> to cache `TOptions` instances.</span></span> <span data-ttu-id="4698d-164"><xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> 可使监视器中的选项实例无效，以便重新计算值 (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601.TryRemove%2A>)。</span><span class="sxs-lookup"><span data-stu-id="4698d-164">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601> invalidates options instances in the monitor so that the value is recomputed (<xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601.TryRemove%2A>).</span></span> <span data-ttu-id="4698d-165">可以通过 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601.TryAdd%2A> 手动引入值。</span><span class="sxs-lookup"><span data-stu-id="4698d-165">Values can be manually introduced with <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601.TryAdd%2A>.</span></span> <span data-ttu-id="4698d-166">在应按需重新创建所有命名实例时使用 <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601.Clear%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="4698d-166">The <xref:Microsoft.Extensions.Options.IOptionsMonitorCache%601.Clear%2A> method is used when all named instances should be recreated on demand.</span></span>

### <a name="options-interfaces-benefits"></a><span data-ttu-id="4698d-167">选项接口优点</span><span class="sxs-lookup"><span data-stu-id="4698d-167">Options interfaces benefits</span></span>

<span data-ttu-id="4698d-168">使用泛型包装器类型，可以将选项的生存期从 DI 容器中解耦出来。</span><span class="sxs-lookup"><span data-stu-id="4698d-168">Using a generic wrapper type gives you the ability to decouple the lifetime of the option from the DI container.</span></span> <span data-ttu-id="4698d-169"><xref:Microsoft.Extensions.Options.IOptions%601.Value?displayProperty=nameWithType> 接口对选项类型提供了一个抽象层，包括泛型约束。</span><span class="sxs-lookup"><span data-stu-id="4698d-169">The <xref:Microsoft.Extensions.Options.IOptions%601.Value?displayProperty=nameWithType> interface provides a layer of abstraction, including generic constraints, on your options type.</span></span> <span data-ttu-id="4698d-170">这提供了以下好处：</span><span class="sxs-lookup"><span data-stu-id="4698d-170">This provides the following benefits:</span></span>

- <span data-ttu-id="4698d-171">`T` 配置实例的评估推迟到在访问 <xref:Microsoft.Extensions.Options.IOptions%601.Value?displayProperty=nameWithType> 时进行，而不是在注入时进行。</span><span class="sxs-lookup"><span data-stu-id="4698d-171">The evaluation of the `T` configuration instance is deferred to the accessing of <xref:Microsoft.Extensions.Options.IOptions%601.Value?displayProperty=nameWithType>, rather than when it is injected.</span></span> <span data-ttu-id="4698d-172">这一点很重要，因为你可以从各种不同的位置使用 `T` 选项，并选择生存期语义，而无需更改关于 `T` 的任何内容。</span><span class="sxs-lookup"><span data-stu-id="4698d-172">This is important because you can consume the `T` option from various places and choose the lifetime semantics without changing anything about `T`.</span></span>
- <span data-ttu-id="4698d-173">注册 `T` 类型的选项时，不需要显式注册 `T` 类型。</span><span class="sxs-lookup"><span data-stu-id="4698d-173">When registering options of type `T`, you do not need to explicitly register the `T` type.</span></span> <span data-ttu-id="4698d-174">如果你使用简单的默认值[创建库](options-library-authors.md)，并且不想强制调用方将选项注册到具有特定生存期的 DI 容器中，这可以带来很多便利。</span><span class="sxs-lookup"><span data-stu-id="4698d-174">This is a convenience when you're [authoring a library](options-library-authors.md) with simple defaults, and you don't want to force the caller to register options into the DI container with a specific lifetime.</span></span>
- <span data-ttu-id="4698d-175">从 API 的角度来看，它允许对类型 `T` 进行约束（在本例中，`T` 被约束为引用类型）。</span><span class="sxs-lookup"><span data-stu-id="4698d-175">From the perspective of the API, it allows for constraints on the type `T` (in this case, `T` is constrained to a reference type).</span></span>

## <a name="use-ioptionssnapshot-to-read-updated-data"></a><span data-ttu-id="4698d-176">使用 IOptionsSnapshot 读取已更新的数据</span><span class="sxs-lookup"><span data-stu-id="4698d-176">Use IOptionsSnapshot to read updated data</span></span>

<span data-ttu-id="4698d-177">当你使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601> 时，在访问每个请求时会计算一次选项，并在请求的生存期内缓存这些选项。</span><span class="sxs-lookup"><span data-stu-id="4698d-177">When you use <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>, options are computed once per request when accessed and are cached for the lifetime of the request.</span></span> <span data-ttu-id="4698d-178">当使用支持读取已更新的配置值的配置提供程序时，将在应用启动后读取对配置所做的更改。</span><span class="sxs-lookup"><span data-stu-id="4698d-178">Changes to the configuration are read after the app starts when using configuration providers that support reading updated configuration values.</span></span>

<span data-ttu-id="4698d-179">`IOptionsMonitor` 和 `IOptionsSnapshot` 之间的区别在于：</span><span class="sxs-lookup"><span data-stu-id="4698d-179">The difference between `IOptionsMonitor` and `IOptionsSnapshot` is that:</span></span>

- <span data-ttu-id="4698d-180">`IOptionsMonitor` 是一种[单一示例服务](dependency-injection.md#singleton)，可随时检索当前选项值，这在单一实例依赖项中尤其有用。</span><span class="sxs-lookup"><span data-stu-id="4698d-180">`IOptionsMonitor` is a [singleton service](dependency-injection.md#singleton) that retrieves current option values at any time, which is especially useful in singleton dependencies.</span></span>
- <span data-ttu-id="4698d-181">`IOptionsSnapshot` 是一种[作用域服务](dependency-injection.md#scoped)，并在构造 `IOptionsSnapshot<T>` 对象时提供选项的快照。</span><span class="sxs-lookup"><span data-stu-id="4698d-181">`IOptionsSnapshot` is a [scoped service](dependency-injection.md#scoped) and provides a snapshot of the options at the time the `IOptionsSnapshot<T>` object is constructed.</span></span> <span data-ttu-id="4698d-182">选项快照旨在用于暂时性和有作用域的依赖项。</span><span class="sxs-lookup"><span data-stu-id="4698d-182">Options snapshots are designed for use with transient and scoped dependencies.</span></span>

<span data-ttu-id="4698d-183">以下代码使用 <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>。</span><span class="sxs-lookup"><span data-stu-id="4698d-183">The following code uses <xref:Microsoft.Extensions.Options.IOptionsSnapshot%601>.</span></span>

:::code language="csharp" source="snippets/configuration/console-json/ScopedService.cs":::

<span data-ttu-id="4698d-184">以下代码注册 `TransientFaultHandlingOptions` 绑定的配置实例：</span><span class="sxs-lookup"><span data-stu-id="4698d-184">The following code registers a configuration instance which `TransientFaultHandlingOptions` binds against:</span></span>

```csharp
services.Configure<TransientFaultHandlingOptions>(
    configurationRoot.GetSection(
        nameof(TransientFaultHandlingOptions)));
```

<span data-ttu-id="4698d-185">在上面的代码中，已读取在应用启动后对 JSON 配置文件所做的更改。</span><span class="sxs-lookup"><span data-stu-id="4698d-185">In the preceding code, changes to the JSON configuration file after the app has started are read.</span></span>

## <a name="ioptionsmonitor"></a><span data-ttu-id="4698d-186">IOptionsMonitor</span><span class="sxs-lookup"><span data-stu-id="4698d-186">IOptionsMonitor</span></span>

<span data-ttu-id="4698d-187">以下代码注册 `TransientFaultHandlingOptions` 绑定的配置实例。</span><span class="sxs-lookup"><span data-stu-id="4698d-187">The following code registers a configuration instance which `TransientFaultHandlingOptions` binds against.</span></span>

```csharp
services.Configure<TransientFaultHandlingOptions>(
    configurationRoot.GetSection(
        nameof(TransientFaultHandlingOptions)));
```

<span data-ttu-id="4698d-188">下面的示例使用 <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>：</span><span class="sxs-lookup"><span data-stu-id="4698d-188">The following example uses <xref:Microsoft.Extensions.Options.IOptionsMonitor%601>:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/MonitorService.cs":::

<span data-ttu-id="4698d-189">在上面的代码中，已读取在应用启动后对 JSON 配置文件所做的更改。</span><span class="sxs-lookup"><span data-stu-id="4698d-189">In the preceding code, changes to the JSON configuration file after the app has started are read.</span></span>

## <a name="named-options-support-using-iconfigurenamedoptions"></a><span data-ttu-id="4698d-190">命名选项支持使用 IConfigureNamedOptions</span><span class="sxs-lookup"><span data-stu-id="4698d-190">Named options support using IConfigureNamedOptions</span></span>

<span data-ttu-id="4698d-191">命名选项：</span><span class="sxs-lookup"><span data-stu-id="4698d-191">Named options:</span></span>

- <span data-ttu-id="4698d-192">当多个配置节绑定到同一属性时有用。</span><span class="sxs-lookup"><span data-stu-id="4698d-192">Are useful when multiple configuration sections bind to the same properties.</span></span>
- <span data-ttu-id="4698d-193">区分大小写。</span><span class="sxs-lookup"><span data-stu-id="4698d-193">Are case-sensitive.</span></span>

<span data-ttu-id="4698d-194">请考虑使用以下 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="4698d-194">Consider the following *appsettings.json* file:</span></span>

```json
{
  "Features": {
    "Personalize": {
      "Enabled": true,
      "ApiKey": "aGEgaGEgeW91IHRob3VnaHQgdGhhdCB3YXMgcmVhbGx5IHNvbWV0aGluZw=="
    },
    "WeatherStation": {
      "Enabled": true,
      "ApiKey": "QXJlIHlvdSBhdHRlbXB0aW5nIHRvIGhhY2sgdXM/"
    }
  }
}
```

<span data-ttu-id="4698d-195">下面的类用于每个节，而不是创建两个类来绑定 `Features:Personalize` 和 `Features:WeatherStation`：</span><span class="sxs-lookup"><span data-stu-id="4698d-195">Rather than creating two classes to bind `Features:Personalize` and `Features:WeatherStation`, the following class is used for each section:</span></span>

```csharp
public class Features
{
    public const string Personalize = nameof(Personalize);
    public const string WeatherStation = nameof(WeatherStation);

    public bool Enabled { get; set; }
    public string ApiKey { get; set; }
}
```

<span data-ttu-id="4698d-196">下面的代码将配置命名选项：</span><span class="sxs-lookup"><span data-stu-id="4698d-196">The following code configures the named options:</span></span>

```csharp
ConfigureServices(services =>
{
    services.Configure<Features>(
        Features.Personalize,
        Configuration.GetSection("Features:Personalize"));

    services.Configure<Features>(
        Features.WeatherStation,
        Configuration.GetSection("Features:WeatherStation"));
});
```

<span data-ttu-id="4698d-197">下面的代码将显示命名选项：</span><span class="sxs-lookup"><span data-stu-id="4698d-197">The following code displays the named options:</span></span>

```csharp
public class Service
{
    private readonly Features _personalizeFeature;
    private readonly Features _weatherStationFeature;

    public Service(IOptionsSnapshot<Features> namedOptionsAccessor)
    {
        _personalizeFeature = namedOptionsAccessor.Get(Features.Personalize);
        _weatherStationFeature = namedOptionsAccessor.Get(Features.WeatherStation);
    }
}
```

<span data-ttu-id="4698d-198">所有选项都是命名实例。</span><span class="sxs-lookup"><span data-stu-id="4698d-198">All options are named instances.</span></span> <span data-ttu-id="4698d-199"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> 实例将被视为面向 `Options.DefaultName` 实例，即 `string.Empty`。</span><span class="sxs-lookup"><span data-stu-id="4698d-199"><xref:Microsoft.Extensions.Options.IConfigureOptions%601> instances are treated as targeting the `Options.DefaultName` instance, which is `string.Empty`.</span></span> <span data-ttu-id="4698d-200"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 还可实现 <xref:Microsoft.Extensions.Options.IConfigureOptions%601>。</span><span class="sxs-lookup"><span data-stu-id="4698d-200"><xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> also implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601>.</span></span> <span data-ttu-id="4698d-201"><xref:Microsoft.Extensions.Options.IOptionsFactory%601> 的默认实现具有适当地使用每个实例的逻辑。</span><span class="sxs-lookup"><span data-stu-id="4698d-201">The default implementation of the <xref:Microsoft.Extensions.Options.IOptionsFactory%601> has logic to use each appropriately.</span></span> <span data-ttu-id="4698d-202">`null` 命名选项用于面向所有命名实例，而不是某一特定命名实例。</span><span class="sxs-lookup"><span data-stu-id="4698d-202">The `null` named option is used to target all of the named instances instead of a specific named instance.</span></span> <span data-ttu-id="4698d-203"><xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll%2A> 和 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll%2A> 使用此约定。</span><span class="sxs-lookup"><span data-stu-id="4698d-203"><xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.ConfigureAll%2A> and <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll%2A> use this convention.</span></span>

## <a name="optionsbuilder-api"></a><span data-ttu-id="4698d-204">OptionsBuilder API</span><span class="sxs-lookup"><span data-stu-id="4698d-204">OptionsBuilder API</span></span>

<span data-ttu-id="4698d-205"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> 用于配置 `TOptions` 实例。</span><span class="sxs-lookup"><span data-stu-id="4698d-205"><xref:Microsoft.Extensions.Options.OptionsBuilder%601> is used to configure `TOptions` instances.</span></span> <span data-ttu-id="4698d-206">`OptionsBuilder` 简化了创建命名选项的过程，因为它只是初始 `AddOptions<TOptions>(string optionsName)` 调用的单个参数，而不会出现在所有后续调用中。</span><span class="sxs-lookup"><span data-stu-id="4698d-206">`OptionsBuilder` streamlines creating named options as it's only a single parameter to the initial `AddOptions<TOptions>(string optionsName)` call instead of appearing in all of the subsequent calls.</span></span> <span data-ttu-id="4698d-207">选项验证和接受服务依赖关系的 `ConfigureOptions` 重载仅可通过 `OptionsBuilder` 获得。</span><span class="sxs-lookup"><span data-stu-id="4698d-207">Options validation and the `ConfigureOptions` overloads that accept service dependencies are only available via `OptionsBuilder`.</span></span>

<span data-ttu-id="4698d-208">`OptionsBuilder` 在[选项验证](#options-validation)部分中使用。</span><span class="sxs-lookup"><span data-stu-id="4698d-208">`OptionsBuilder` is used in the [Options validation](#options-validation) section.</span></span>

## <a name="use-di-services-to-configure-options"></a><span data-ttu-id="4698d-209">使用 DI 服务配置选项</span><span class="sxs-lookup"><span data-stu-id="4698d-209">Use DI services to configure options</span></span>

<span data-ttu-id="4698d-210">在配置选项时，可以通过以下两种方式通过依赖关系注入访问服务：</span><span class="sxs-lookup"><span data-stu-id="4698d-210">Services can be accessed from dependency injection while configuring options in two ways:</span></span>

- <span data-ttu-id="4698d-211">将配置委托传递给 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder%601) 上的 [ Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A)。</span><span class="sxs-lookup"><span data-stu-id="4698d-211">Pass a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A) on [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder%601).</span></span> <span data-ttu-id="4698d-212">`OptionsBuilder<TOptions>` 提供 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A) 的重载，该重载允许使用最多五个服务来配置选项：</span><span class="sxs-lookup"><span data-stu-id="4698d-212">`OptionsBuilder<TOptions>` provides overloads of [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A) that allow use of up to five services to configure options:</span></span>

  ```csharp
  services.AddOptions<MyOptions>("optionalName")
      .Configure<ExampleService, ScopedService, MonitorService>(
          (options, es, ss, ms) =>
              options.Property = DoSomethingWith(es, ss, ms));
  ```

- <span data-ttu-id="4698d-213">创建实现 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 或 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> 的类型，并将该类型注册为服务。</span><span class="sxs-lookup"><span data-stu-id="4698d-213">Create a type that implements <xref:Microsoft.Extensions.Options.IConfigureOptions%601> or <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601> and register the type as a service.</span></span>

<span data-ttu-id="4698d-214">建议将配置委托传递给 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A)，因为创建服务较复杂。</span><span class="sxs-lookup"><span data-stu-id="4698d-214">We recommend passing a configuration delegate to [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A), since creating a service is more complex.</span></span> <span data-ttu-id="4698d-215">在调用 [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A) 时，创建类型等效于框架执行的操作。</span><span class="sxs-lookup"><span data-stu-id="4698d-215">Creating a type is equivalent to what the framework does when calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A).</span></span> <span data-ttu-id="4698d-216">调用[ Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A) 会注册临时泛型 <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>，它具有接受指定的泛型服务类型的构造函数。</span><span class="sxs-lookup"><span data-stu-id="4698d-216">Calling [Configure](xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A) registers a transient generic <xref:Microsoft.Extensions.Options.IConfigureNamedOptions%601>, which has a constructor that accepts the generic service types specified.</span></span>

## <a name="options-validation"></a><span data-ttu-id="4698d-217">选项验证</span><span class="sxs-lookup"><span data-stu-id="4698d-217">Options validation</span></span>

<span data-ttu-id="4698d-218">通过选项验证，可以验证选项值。</span><span class="sxs-lookup"><span data-stu-id="4698d-218">Options validation enables option values to be validated.</span></span>

<span data-ttu-id="4698d-219">请考虑使用以下 appsettings.json 文件：</span><span class="sxs-lookup"><span data-stu-id="4698d-219">Consider the following *appsettings.json* file:</span></span>

```json
{
  "Settings": {
    "SiteTitle": "Amazing docs from Awesome people!",
    "Scale": 10,
    "VerbosityLevel": 32
  }
}
```

<span data-ttu-id="4698d-220">下面的类绑定到 `"Settings"` 配置节，并应用若干 `DataAnnotations` 规则：</span><span class="sxs-lookup"><span data-stu-id="4698d-220">The following class binds to the `"Settings"` configuration section and applies a couple of `DataAnnotations` rules:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/SettingsOptions.cs":::

<span data-ttu-id="4698d-221">下面的代码：</span><span class="sxs-lookup"><span data-stu-id="4698d-221">The following code:</span></span>

- <span data-ttu-id="4698d-222">调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%2A> 以获取绑定到 `SettingsOptions` 类的 [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder%601)。</span><span class="sxs-lookup"><span data-stu-id="4698d-222">Calls <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%2A> to get an [OptionsBuilder\<TOptions>](xref:Microsoft.Extensions.Options.OptionsBuilder%601) that binds to the `SettingsOptions` class.</span></span>
- <span data-ttu-id="4698d-223">调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations%2A> 以使用 `DataAnnotations` 启用验证。</span><span class="sxs-lookup"><span data-stu-id="4698d-223">Calls <xref:Microsoft.Extensions.DependencyInjection.OptionsBuilderDataAnnotationsExtensions.ValidateDataAnnotations%2A> to enable validation using `DataAnnotations`.</span></span>

```csharp
services.AddOptions<SettingsOptions>()
    .Bind(Configuration.GetSection(SettingsOptions.Settings))
    .ValidateDataAnnotations();
```

<span data-ttu-id="4698d-224">`ValidateDataAnnotations` 扩展方法在 [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) NuGet 包中定义。</span><span class="sxs-lookup"><span data-stu-id="4698d-224">The `ValidateDataAnnotations` extension method is defined in the [Microsoft.Extensions.Options.DataAnnotations](https://www.nuget.org/packages/Microsoft.Extensions.Options.DataAnnotations) NuGet package.</span></span>

<span data-ttu-id="4698d-225">下面的代码显示配置值或验证错误：</span><span class="sxs-lookup"><span data-stu-id="4698d-225">The following code displays the configuration values or the validation errors:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/ValidationService.cs":::

<span data-ttu-id="4698d-226">下面的代码使用委托应用更复杂的验证规则：</span><span class="sxs-lookup"><span data-stu-id="4698d-226">The following code applies a more complex validation rule using a delegate:</span></span>

```csharp
services.AddOptions<SettingsOptions>()
    .Bind(Configuration.GetSection(SettingsOptions.Settings))
    .ValidateDataAnnotations()
    .Validate(config =>
    {
        if (config.Scale != 0)
        {
            return config.VerbosityLevel > config.Scale;
        }

        return true;
    }, "VerbosityLevel must be > than Scale.");
```

### <a name="ivalidateoptions-for-complex-validation"></a><span data-ttu-id="4698d-227">用于复杂验证的 IValidateOptions</span><span class="sxs-lookup"><span data-stu-id="4698d-227">IValidateOptions for complex validation</span></span>

<span data-ttu-id="4698d-228">下面的类实现了 <xref:Microsoft.Extensions.Options.IValidateOptions%601>：</span><span class="sxs-lookup"><span data-stu-id="4698d-228">The following class implements <xref:Microsoft.Extensions.Options.IValidateOptions%601>:</span></span>

:::code language="csharp" source="snippets/configuration/console-json/ValidateSettingsOptions.cs":::

<span data-ttu-id="4698d-229">`IValidateOptions` 允许将验证代码移入类中。</span><span class="sxs-lookup"><span data-stu-id="4698d-229">`IValidateOptions` enables moving the validation code into a class.</span></span>

<span data-ttu-id="4698d-230">使用前面的代码，使用以下代码在 `ConfigureServices` 中启用验证：</span><span class="sxs-lookup"><span data-stu-id="4698d-230">Using the preceding code, validation is enabled in `ConfigureServices` with the following code:</span></span>

```csharp
services.Configure<SettingsOptions>(
    Configuration.GetSection(
        SettingsOptions.Settings));
services.TryAddEnumerable(
    ServiceDescriptor.Singleton
        <IValidateOptions<SettingsOptions>, ValidateSettingsOptions>());
```

## <a name="options-post-configuration"></a><span data-ttu-id="4698d-231">选项后期配置</span><span class="sxs-lookup"><span data-stu-id="4698d-231">Options post-configuration</span></span>

<span data-ttu-id="4698d-232">使用 <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601> 设置后期配置。</span><span class="sxs-lookup"><span data-stu-id="4698d-232">Set post-configuration with <xref:Microsoft.Extensions.Options.IPostConfigureOptions%601>.</span></span> <span data-ttu-id="4698d-233">后期配置在所有 <xref:Microsoft.Extensions.Options.IConfigureOptions%601> 配置发生后运行，在需要重写配置的场景中非常有用：</span><span class="sxs-lookup"><span data-stu-id="4698d-233">Post-configuration runs after all <xref:Microsoft.Extensions.Options.IConfigureOptions%601> configuration occurs, and can be useful in scenarios when you need to override configuration:</span></span>

```csharp
services.PostConfigure<CustomOptions>(customOptions =>
{
    customOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="4698d-234"><xref:Microsoft.Extensions.Options.IPostConfigureOptions%601.PostConfigure%2A> 可用于对命名选项进行后期配置：</span><span class="sxs-lookup"><span data-stu-id="4698d-234"><xref:Microsoft.Extensions.Options.IPostConfigureOptions%601.PostConfigure%2A> is available to post-configure named options:</span></span>

```csharp
services.PostConfigure<CustomOptions>("named_options_1", customOptions =>
{
    customOptions.Option1 = "post_configured_option1_value";
});
```

<span data-ttu-id="4698d-235">使用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll%2A> 对所有配置实例进行后期配置：</span><span class="sxs-lookup"><span data-stu-id="4698d-235">Use <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigureAll%2A> to post-configure all configuration instances:</span></span>

```csharp
services.PostConfigureAll<CustomOptions>(customOptions =>
{
    customOptions.Option1 = "post_configured_option1_value";
});
```

## <a name="see-also"></a><span data-ttu-id="4698d-236">另请参阅</span><span class="sxs-lookup"><span data-stu-id="4698d-236">See also</span></span>

- [<span data-ttu-id="4698d-237">.NET 中的配置</span><span class="sxs-lookup"><span data-stu-id="4698d-237">Configuration in .NET</span></span>](configuration.md)
- [<span data-ttu-id="4698d-238">面向 .NET 库创建者的选项模式指南</span><span class="sxs-lookup"><span data-stu-id="4698d-238">Options pattern guidance for .NET library authors</span></span>](options-library-authors.md)
