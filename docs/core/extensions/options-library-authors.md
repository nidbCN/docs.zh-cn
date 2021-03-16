---
title: 面向 .NET 库创建者的选项模式指南
author: IEvangelist
description: 了解如何在 .NET 中以库创建者的身份公开选项模式。
ms.author: dapine
ms.date: 01/28/2021
ms.openlocfilehash: d0da94a8f25c9e5aba6093fab07ccca6a0a7c345
ms.sourcegitcommit: 68c9d9d9a97aab3b59d388914004b5474cf1dbd7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "102401974"
---
# <a name="options-pattern-guidance-for-net-library-authors"></a><span data-ttu-id="db367-103">面向 .NET 库创建者的选项模式指南</span><span class="sxs-lookup"><span data-stu-id="db367-103">Options pattern guidance for .NET library authors</span></span>

<span data-ttu-id="db367-104">借助依赖关系注入，在注册服务及其相应的配置时，可以使用选项模式。</span><span class="sxs-lookup"><span data-stu-id="db367-104">With the help of dependency injection, registering your services and their corresponding configurations can make use of the *options pattern*.</span></span> <span data-ttu-id="db367-105">通过选项模式，库（和服务）的使用者可以要求提供[选项接口](options.md#options-interfaces)的实例，其中 `TOptions` 是你的选项类。</span><span class="sxs-lookup"><span data-stu-id="db367-105">The options pattern enables consumers of your library (and your services) to require instances of [options interfaces](options.md#options-interfaces) where `TOptions` is your options class.</span></span> <span data-ttu-id="db367-106">通过强类型对象使用配置选项有助于确保值表示方法的一致性，让你无需再费力手动分析字符串值。</span><span class="sxs-lookup"><span data-stu-id="db367-106">Consuming configuration options through strongly-typed objects helps to ensure consistent value representation, and removes the burden of manually parsing string values.</span></span> <span data-ttu-id="db367-107">有许多[配置提供程序](configuration-providers.md)可供库使用者使用。</span><span class="sxs-lookup"><span data-stu-id="db367-107">There are many [configuration providers](configuration-providers.md) for consumers of your library to use.</span></span> <span data-ttu-id="db367-108">借助这些提供程序，使用者可以通过多种方式配置库。</span><span class="sxs-lookup"><span data-stu-id="db367-108">With these providers, consumers can configure your library in many ways.</span></span>

<span data-ttu-id="db367-109">作为 .NET 库的创建者，你将了解有关如何正确向库的使用者公开选项模式的一般指南。</span><span class="sxs-lookup"><span data-stu-id="db367-109">As a .NET library author, you'll learn general guidance on how to correctly expose the options pattern to consumers of your library.</span></span> <span data-ttu-id="db367-110">有很多种方法都会达到同样的效果，并有几个注意事项。</span><span class="sxs-lookup"><span data-stu-id="db367-110">There are various ways to achieve the same thing, and several considerations to make.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="db367-111">命名约定</span><span class="sxs-lookup"><span data-stu-id="db367-111">Naming conventions</span></span>

<span data-ttu-id="db367-112">按照约定，负责注册服务的扩展方法被命名为 `Add{Service}`，其中 `{Service}` 是一个有意义的描述性名称。</span><span class="sxs-lookup"><span data-stu-id="db367-112">By convention, extension methods responsible for registering services are named `Add{Service}`, where `{Service}` is a meaningful and descriptive name.</span></span> <span data-ttu-id="db367-113">服务注册可能附带 `Use{Service}` 扩展方法，具体取决于包。</span><span class="sxs-lookup"><span data-stu-id="db367-113">Depending on the package, the registration of services may be accompanied by `Use{Service}` extension methods.</span></span> <span data-ttu-id="db367-114">`Use{Service}` 扩展方法在 [ASP.NET Core](/aspnet) 中是通用的。</span><span class="sxs-lookup"><span data-stu-id="db367-114">The `Use{Service}` extension methods are commonplace in [ASP.NET Core](/aspnet).</span></span>

<span data-ttu-id="db367-115">✔️ 考虑使用将你的服务与其他产品/服务区分开来的名称。</span><span class="sxs-lookup"><span data-stu-id="db367-115">✔️ CONSIDER names that disambiguate your service from other offerings.</span></span>

<span data-ttu-id="db367-116">❌ 不要使用已是 Microsoft 官方包中 .NET 生态系统的一部分的名称。</span><span class="sxs-lookup"><span data-stu-id="db367-116">❌ DO NOT use names that are already part of the .NET ecosystem from official Microsoft packages.</span></span>

<span data-ttu-id="db367-117">✔️ 考虑将公开扩展方法的静态类命名为 `{Type}Extensions`，其中 `{Type}` 是你要扩展的类型。</span><span class="sxs-lookup"><span data-stu-id="db367-117">✔️ CONSIDER naming static classes that expose extension methods as `{Type}Extensions`, where `{Type}` is the type that you're extending.</span></span>

### <a name="namespace-guidance"></a><span data-ttu-id="db367-118">命名空间指南</span><span class="sxs-lookup"><span data-stu-id="db367-118">Namespace guidance</span></span>

<span data-ttu-id="db367-119">Microsoft 包利用 `Microsoft.Extensions.DependencyInjection` 命名空间来统一各种服务产品/服务的注册。</span><span class="sxs-lookup"><span data-stu-id="db367-119">Microsoft packages make use of the `Microsoft.Extensions.DependencyInjection` namespace to unify the registration of various service offerings.</span></span>

<span data-ttu-id="db367-120">✔️ 考虑使用清楚标识包产品/服务的命名空间。</span><span class="sxs-lookup"><span data-stu-id="db367-120">✔️ CONSIDER a namespace that clearly identifies your package offering.</span></span>

<span data-ttu-id="db367-121">❌ 不要将 `Microsoft.Extensions.DependencyInjection` 命名空间用于非官方的 Microsoft 包。</span><span class="sxs-lookup"><span data-stu-id="db367-121">❌ DO NOT use the `Microsoft.Extensions.DependencyInjection` namespace for non-official Microsoft packages.</span></span>

## <a name="parameterless"></a><span data-ttu-id="db367-122">无参数</span><span class="sxs-lookup"><span data-stu-id="db367-122">Parameterless</span></span>

<span data-ttu-id="db367-123">如果你的服务可以用最少的显式配置或不需要显式配置来工作，请考虑使用无参数扩展方法。</span><span class="sxs-lookup"><span data-stu-id="db367-123">If your service can work with minimal or no explicit configuration, consider a parameterless extension method.</span></span>

:::code language="csharp" source="snippets/configuration/options-noparams/ServiceCollectionExtensions.cs" highlight="10-14":::

<span data-ttu-id="db367-124">在上述代码中，`AddMyLibraryService` 执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="db367-124">In the preceding code, the `AddMyLibraryService`:</span></span>

- <span data-ttu-id="db367-125">扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例</span><span class="sxs-lookup"><span data-stu-id="db367-125">Extends an instance of <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span></span>
- <span data-ttu-id="db367-126">调用类型参数为 `LibraryOptions` 的 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection)?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="db367-126">Calls <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection)?displayProperty=nameWithType> with the type parameter of `LibraryOptions`</span></span>
- <span data-ttu-id="db367-127">链接对 <xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A> 的调用，用来指定默认选项值</span><span class="sxs-lookup"><span data-stu-id="db367-127">Chains a call to <xref:Microsoft.Extensions.Options.OptionsBuilder%601.Configure%2A>, which specifies the default option values</span></span>

## <a name="iconfiguration-parameter"></a><span data-ttu-id="db367-128">`IConfiguration` 参数</span><span class="sxs-lookup"><span data-stu-id="db367-128">`IConfiguration` parameter</span></span>

<span data-ttu-id="db367-129">在创建向使用者公开许多选项的库时，可能需要考虑要求使用 `IConfiguration` 参数扩展方法。</span><span class="sxs-lookup"><span data-stu-id="db367-129">When you author a library that exposes many options to consumers, you may want to consider requiring an `IConfiguration` parameter extension method.</span></span> <span data-ttu-id="db367-130">应使用 <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A?displayProperty=nameWithType> 函数，将预期的 `IConfiguration` 实例的作用域限定为此配置的已命名部分。</span><span class="sxs-lookup"><span data-stu-id="db367-130">The expected `IConfiguration` instance should be scoped to a named section of the configuration by using the <xref:Microsoft.Extensions.Configuration.IConfiguration.GetSection%2A?displayProperty=nameWithType> function.</span></span>

:::code language="csharp" source="snippets/configuration/options-configparam/ServiceCollectionExtensions.cs" highlight="10,12-16":::

<span data-ttu-id="db367-131">在上述代码中，`AddMyLibraryService` 执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="db367-131">In the preceding code, the `AddMyLibraryService`:</span></span>

- <span data-ttu-id="db367-132">扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例</span><span class="sxs-lookup"><span data-stu-id="db367-132">Extends an instance of <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span></span>
- <span data-ttu-id="db367-133">定义 <xref:Microsoft.Extensions.Configuration.IConfiguration> 参数 `namedConfigurationSection`</span><span class="sxs-lookup"><span data-stu-id="db367-133">Defines an <xref:Microsoft.Extensions.Configuration.IConfiguration> parameter `namedConfigurationSection`</span></span>
- <span data-ttu-id="db367-134">调用 <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind(Microsoft.Extensions.Configuration.IConfiguration,System.Object)>，传递一个配置绑定到的选项实例</span><span class="sxs-lookup"><span data-stu-id="db367-134">Calls <xref:Microsoft.Extensions.Configuration.ConfigurationBinder.Bind(Microsoft.Extensions.Configuration.IConfiguration,System.Object)> passing an options instance that the configuration binds to</span></span>

<span data-ttu-id="db367-135">此模式中的使用者提供已命名部分已限定作用域的 `IConfiguration` 实例：</span><span class="sxs-lookup"><span data-stu-id="db367-135">Consumers in this pattern provide the scoped `IConfiguration` instance of the named section:</span></span>

:::code language="csharp" source="snippets/configuration/options-configparam/Program.cs" highlight="22-23":::

<span data-ttu-id="db367-136">对 `.AddMyLibraryService` 的调用是在 <xref:Microsoft.Extensions.Hosting.IHostBuilder.ConfigureServices%2A> 方法中进行的。</span><span class="sxs-lookup"><span data-stu-id="db367-136">The call to `.AddMyLibraryService` is made in the <xref:Microsoft.Extensions.Hosting.IHostBuilder.ConfigureServices%2A> method.</span></span> <span data-ttu-id="db367-137">同样地，使用 `Startup` 类时，会在 `ConfigureServices` 中添加注册的服务。</span><span class="sxs-lookup"><span data-stu-id="db367-137">The same is true when using a `Startup` class, the addition of services being registered occurs in `ConfigureServices`.</span></span>

<span data-ttu-id="db367-138">作为库创建者，由你指定默认值。</span><span class="sxs-lookup"><span data-stu-id="db367-138">As the library author, specifying default values is up to you.</span></span>

> [!NOTE]
> <span data-ttu-id="db367-139">可以将配置绑定到选项实例。</span><span class="sxs-lookup"><span data-stu-id="db367-139">It is possible to bind configuration to an options instance.</span></span> <span data-ttu-id="db367-140">但是，存在名称冲突的风险，冲突将导致错误。</span><span class="sxs-lookup"><span data-stu-id="db367-140">However, there is a risk of name collisions - which will cause errors.</span></span> <span data-ttu-id="db367-141">此外，当以这种方式手动绑定时，将选项模式的使用限制为读取一次。</span><span class="sxs-lookup"><span data-stu-id="db367-141">Additionally, when manually binding in this way, you limit the consumption of your options pattern to read-once.</span></span> <span data-ttu-id="db367-142">对设置的更改将不会被重新绑定，因为这样的话，使用者就无法使用 [IOptionsMonitor](options.md#ioptionsmonitor) 接口。</span><span class="sxs-lookup"><span data-stu-id="db367-142">Changes to settings will not be re-bound, as such consumers will not be able to use the [IOptionsMonitor](options.md#ioptionsmonitor) interface.</span></span>
>
> ```csharp
> services.AddOptions<LibraryOptions>()
>     .Configure<IConfiguration>(
>         (options, configuration) =>
>             configuration.GetSection("LibraryOptions").Bind(options));
> ```

## <a name="actiontoptions-parameter"></a><span data-ttu-id="db367-143">`Action<TOptions>` 参数</span><span class="sxs-lookup"><span data-stu-id="db367-143">`Action<TOptions>` parameter</span></span>

<span data-ttu-id="db367-144">库的使用者可能有兴趣提供一个 Lambda 表达式来生成选项类的实例。</span><span class="sxs-lookup"><span data-stu-id="db367-144">Consumers of your library may be interested in providing a lambda expression that yields an instance of your options class.</span></span> <span data-ttu-id="db367-145">在此场景中，你在你的扩展方法中定义了一个 `Action<LibraryOptions>` 参数。</span><span class="sxs-lookup"><span data-stu-id="db367-145">In this scenario, you define an `Action<LibraryOptions>` parameter in your extension method.</span></span>

:::code language="csharp" source="snippets/configuration/options-action/ServiceCollectionExtensions.cs" highlight="10,12":::

<span data-ttu-id="db367-146">在上述代码中，`AddMyLibraryService` 执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="db367-146">In the preceding code, the `AddMyLibraryService`:</span></span>

- <span data-ttu-id="db367-147">扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例</span><span class="sxs-lookup"><span data-stu-id="db367-147">Extends an instance of <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span></span>
- <span data-ttu-id="db367-148">定义一个 <xref:System.Action%601>，其中 `T` 为 `LibraryOptions` 参数 `configureOptions`</span><span class="sxs-lookup"><span data-stu-id="db367-148">Defines an <xref:System.Action%601> where `T` is `LibraryOptions` parameter `configureOptions`</span></span>
- <span data-ttu-id="db367-149">根据 `configureOptions` 操作调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure%2A></span><span class="sxs-lookup"><span data-stu-id="db367-149">Calls <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure%2A> given the `configureOptions` action</span></span>

<span data-ttu-id="db367-150">此模式下的使用者提供一个 Lambda 表达式（或一个符合 `Action<LibraryOptions>` 参数的委托）：</span><span class="sxs-lookup"><span data-stu-id="db367-150">Consumers in this pattern provide a lambda expression (or a delegate that satisfies the `Action<LibraryOptions>` parameter):</span></span>

:::code language="csharp" source="snippets/configuration/options-action/Program.cs" highlight="22-26":::

## <a name="options-instance-parameter"></a><span data-ttu-id="db367-151">选项实例参数</span><span class="sxs-lookup"><span data-stu-id="db367-151">Options instance parameter</span></span>

<span data-ttu-id="db367-152">库的使用者可能更倾向于提供一个内联的选项实例。</span><span class="sxs-lookup"><span data-stu-id="db367-152">Consumers of your library might prefer to provide an inlined options instance.</span></span> <span data-ttu-id="db367-153">在此场景中，你公开一个扩展方法，此方法采用选项对象的实例 `LibraryOptions`。</span><span class="sxs-lookup"><span data-stu-id="db367-153">In this scenario, you expose an extension method that takes an instance of your options object, `LibraryOptions`.</span></span>

:::code language="csharp" source="snippets/configuration/options-object/ServiceCollectionExtensions.cs" highlight="9,11-17":::

<span data-ttu-id="db367-154">在上述代码中，`AddMyLibraryService` 执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="db367-154">In the preceding code, the `AddMyLibraryService`:</span></span>

- <span data-ttu-id="db367-155">扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例</span><span class="sxs-lookup"><span data-stu-id="db367-155">Extends an instance of <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span></span>
- <span data-ttu-id="db367-156">调用类型参数为 `LibraryOptions` 的 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection)?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="db367-156">Calls <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions%60%601(Microsoft.Extensions.DependencyInjection.IServiceCollection)?displayProperty=nameWithType> with the type parameter of `LibraryOptions`</span></span>
- <span data-ttu-id="db367-157">链接对 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure%2A> 的调用，指定可从给定 `userOptions` 实例中替代的默认选项值</span><span class="sxs-lookup"><span data-stu-id="db367-157">Chains a call to <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.Configure%2A>, which specifies default option values that can be overridden from the given `userOptions` instance</span></span>

<span data-ttu-id="db367-158">此模式下的使用者提供 `LibraryOptions` 类的实例，以内联方式定义所需的属性值：</span><span class="sxs-lookup"><span data-stu-id="db367-158">Consumers in this pattern provide an instance of the `LibraryOptions` class, defining desired property values inline:</span></span>

:::code language="csharp" source="snippets/configuration/options-object/Program.cs" highlight="22-26":::

## <a name="post-configuration"></a><span data-ttu-id="db367-159">配置后</span><span class="sxs-lookup"><span data-stu-id="db367-159">Post configuration</span></span>

<span data-ttu-id="db367-160">绑定或指定所有配置选项值后，便可以使用发布配置功能。</span><span class="sxs-lookup"><span data-stu-id="db367-160">After all configuration option values are bound or specified, post configuration functionality is available.</span></span> <span data-ttu-id="db367-161">通过公开前面详述的同一 [`Action<TOptions>` 参数](#actiontoptions-parameter)，可以选择调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigure%2A>。</span><span class="sxs-lookup"><span data-stu-id="db367-161">Exposing the same [`Action<TOptions>` parameter](#actiontoptions-parameter) detailed earlier, you could choose to call <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigure%2A>.</span></span> <span data-ttu-id="db367-162">发布配置在进行所有 `.Configure` 调用之后运行。</span><span class="sxs-lookup"><span data-stu-id="db367-162">Post configure runs after all `.Configure` calls.</span></span>

:::code language="csharp" source="snippets/configuration/options-postconfig/ServiceCollectionExtensions.cs" highlight="10,12":::

<span data-ttu-id="db367-163">在上述代码中，`AddMyLibraryService` 执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="db367-163">In the preceding code, the `AddMyLibraryService`:</span></span>

- <span data-ttu-id="db367-164">扩展 <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection> 的实例</span><span class="sxs-lookup"><span data-stu-id="db367-164">Extends an instance of <xref:Microsoft.Extensions.DependencyInjection.IServiceCollection></span></span>
- <span data-ttu-id="db367-165">定义一个 <xref:System.Action%601>，其中 `T` 为 `LibraryOptions` 参数 `configureOptions`</span><span class="sxs-lookup"><span data-stu-id="db367-165">Defines an <xref:System.Action%601> where `T` is `LibraryOptions` parameter `configureOptions`</span></span>
- <span data-ttu-id="db367-166">根据 `configureOptions` 操作调用 <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigure%2A></span><span class="sxs-lookup"><span data-stu-id="db367-166">Calls <xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.PostConfigure%2A> given the `configureOptions` action</span></span>

<span data-ttu-id="db367-167">此模式下的使用者提供一个 Lambda 表达式（或一个符合 `Action<LibraryOptions>` 参数的委托），就如同在非发布配置场景中，使用者使用 [`Action<TOptions>` 参数] 提供一样：</span><span class="sxs-lookup"><span data-stu-id="db367-167">Consumers in this pattern provide a lambda expression (or a delegate that satisfies the `Action<LibraryOptions>` parameter), just as they would with the [`Action<TOptions>` parameter] in a non-post configuration scenario:</span></span>

:::code language="csharp" source="snippets/configuration/options-postconfig/Program.cs" highlight="22-26":::

## <a name="see-also"></a><span data-ttu-id="db367-168">另请参阅</span><span class="sxs-lookup"><span data-stu-id="db367-168">See also</span></span>

- [<span data-ttu-id="db367-169">.NET 中的选项模式</span><span class="sxs-lookup"><span data-stu-id="db367-169">Options pattern in .NET</span></span>](options.md)
- [<span data-ttu-id="db367-170">.NET 中的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="db367-170">Dependency injection in .NET</span></span>](dependency-injection.md)
- [<span data-ttu-id="db367-171">依赖关系注入指南</span><span class="sxs-lookup"><span data-stu-id="db367-171">Dependency injection guidelines</span></span>](dependency-injection-guidelines.md)
