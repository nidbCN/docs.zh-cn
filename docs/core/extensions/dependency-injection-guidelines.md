---
title: 依赖关系注入指南
description: 了解各种依赖关系注入准则和 .NET 应用程序开发的最佳做法。
author: IEvangelist
ms.author: dapine
ms.date: 10/29/2020
ms.topic: guide
ms.openlocfilehash: 105536df873829dc79dcca1b86d080360e71303f
ms.sourcegitcommit: f2ab02d9a780819ca2e5310bbcf5cfe5b7993041
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/03/2021
ms.locfileid: "102401976"
---
# <a name="dependency-injection-guidelines"></a><span data-ttu-id="576d2-103">依赖关系注入指南</span><span class="sxs-lookup"><span data-stu-id="576d2-103">Dependency injection guidelines</span></span>

<span data-ttu-id="576d2-104">本文介绍在 .NET 应用程序中实现依赖关系注入的一般准则和最佳做法。</span><span class="sxs-lookup"><span data-stu-id="576d2-104">This article provides general guidelines and best practices for implementing dependency injection in .NET applications.</span></span>

## <a name="design-services-for-dependency-injection"></a><span data-ttu-id="576d2-105">设计能够进行依赖关系注入的服务</span><span class="sxs-lookup"><span data-stu-id="576d2-105">Design services for dependency injection</span></span>

<span data-ttu-id="576d2-106">在设计能够进行依赖注入的服务时：</span><span class="sxs-lookup"><span data-stu-id="576d2-106">When designing services for dependency injection:</span></span>

- <span data-ttu-id="576d2-107">避免有状态的、静态类和成员。</span><span class="sxs-lookup"><span data-stu-id="576d2-107">Avoid stateful, static classes and members.</span></span> <span data-ttu-id="576d2-108">通过将应用设计为改用单一实例服务，避免创建全局状态。</span><span class="sxs-lookup"><span data-stu-id="576d2-108">Avoid creating global state by designing apps to use singleton services instead.</span></span>
- <span data-ttu-id="576d2-109">避免在服务中直接实例化依赖类。</span><span class="sxs-lookup"><span data-stu-id="576d2-109">Avoid direct instantiation of dependent classes within services.</span></span> <span data-ttu-id="576d2-110">直接实例化会将代码耦合到特定实现。</span><span class="sxs-lookup"><span data-stu-id="576d2-110">Direct instantiation couples the code to a particular implementation.</span></span>
- <span data-ttu-id="576d2-111">不在服务中包含过多内容，确保设计规范，并易于测试。</span><span class="sxs-lookup"><span data-stu-id="576d2-111">Make services small, well-factored, and easily tested.</span></span>

<span data-ttu-id="576d2-112">如果一个类有很多已注入的依赖关系，这可能表明该类拥有过多的责任，并且违反了[单一责任原则 (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility)。</span><span class="sxs-lookup"><span data-stu-id="576d2-112">If a class has many injected dependencies, it might be a sign that the class has too many responsibilities and violates the [Single Responsibility Principle (SRP)](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#single-responsibility).</span></span> <span data-ttu-id="576d2-113">尝试通过将某些职责移动到一个新类来重构类。</span><span class="sxs-lookup"><span data-stu-id="576d2-113">Attempt to refactor the class by moving some of its responsibilities into new classes.</span></span>

### <a name="disposal-of-services"></a><span data-ttu-id="576d2-114">服务释放</span><span class="sxs-lookup"><span data-stu-id="576d2-114">Disposal of services</span></span>

<span data-ttu-id="576d2-115">容器负责清除它创建的类型，并在 <xref:System.IDisposable> 实例上调用 <xref:System.IDisposable.Dispose%2A>。</span><span class="sxs-lookup"><span data-stu-id="576d2-115">The container is responsible for cleanup of types it creates, and calls <xref:System.IDisposable.Dispose%2A> on <xref:System.IDisposable> instances.</span></span> <span data-ttu-id="576d2-116">从容器中解析的服务绝对不应由开发人员释放。</span><span class="sxs-lookup"><span data-stu-id="576d2-116">Services resolved from the container should never be disposed by the developer.</span></span> <span data-ttu-id="576d2-117">如果类型或工厂注册为单一实例，则容器自动释放单一实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-117">If a type or factory is registered as a singleton, the container disposes the singleton automatically.</span></span>

<span data-ttu-id="576d2-118">在下面的示例中，服务由服务容器创建，并自动释放：</span><span class="sxs-lookup"><span data-stu-id="576d2-118">In the following example, the services are created by the service container and disposed automatically:</span></span>

:::code language="csharp" source="snippets/configuration/console-di-disposable/TransientDisposable.cs":::

<span data-ttu-id="576d2-119">前面的可释放服务应具有暂时性生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-119">The preceding disposable is intended to have a transient lifetime.</span></span>

:::code language="csharp" source="snippets/configuration/console-di-disposable/ScopedDisposable.cs":::

<span data-ttu-id="576d2-120">前面的可释放服务应具有作用域内生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-120">The preceding disposable is intended to have a scoped lifetime.</span></span>

:::code language="csharp" source="snippets/configuration/console-di-disposable/SingletonDisposable.cs":::

<span data-ttu-id="576d2-121">前面的可释放服务应具有单一实例生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-121">The preceding disposable is intended to have a singleton lifetime.</span></span>

:::code language="csharp" source="snippets/configuration/console-di-disposable/Program.cs" range="1-21,41-60" highlight="":::

<span data-ttu-id="576d2-122">调试控制台在运行后显示下面的示例输出：</span><span class="sxs-lookup"><span data-stu-id="576d2-122">The debug console shows the following sample output after running:</span></span>

```console
Scope 1...
ScopedDisposable.Dispose()
TransientDisposable.Dispose()

Scope 2...
ScopedDisposable.Dispose()
TransientDisposable.Dispose()

info: Microsoft.Hosting.Lifetime[0]
      Application started.Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
     Hosting environment: Production
info: Microsoft.Hosting.Lifetime[0]
     Content root path: .\configuration\console-di-disposable\bin\Debug\net5.0
info: Microsoft.Hosting.Lifetime[0]
     Application is shutting down...
SingletonDisposable.Dispose()
```

### <a name="services-not-created-by-the-service-container"></a><span data-ttu-id="576d2-123">不由服务容器创建的服务</span><span class="sxs-lookup"><span data-stu-id="576d2-123">Services not created by the service container</span></span>

<span data-ttu-id="576d2-124">考虑下列代码：</span><span class="sxs-lookup"><span data-stu-id="576d2-124">Consider the following code:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSingleton(new ExampleService());

    // ...
}
```

<span data-ttu-id="576d2-125">在上述代码中：</span><span class="sxs-lookup"><span data-stu-id="576d2-125">In the preceding code:</span></span>

- <span data-ttu-id="576d2-126">`ExampleService` 实例不是由服务容器创建的。</span><span class="sxs-lookup"><span data-stu-id="576d2-126">The `ExampleService` instance is **not** created by the service container.</span></span>
- <span data-ttu-id="576d2-127">框架不会自动释放服务。</span><span class="sxs-lookup"><span data-stu-id="576d2-127">The framework does **not** dispose of the services automatically.</span></span>
- <span data-ttu-id="576d2-128">开发人员负责释放服务。</span><span class="sxs-lookup"><span data-stu-id="576d2-128">The developer is responsible for disposing the services.</span></span>

### <a name="idisposable-guidance-for-transient-and-shared-instances"></a><span data-ttu-id="576d2-129">暂时和共享实例的 IDisposable 指南</span><span class="sxs-lookup"><span data-stu-id="576d2-129">IDisposable guidance for Transient and shared instances</span></span>

#### <a name="transient-limited-lifetime"></a><span data-ttu-id="576d2-130">暂时、有限的生存期</span><span class="sxs-lookup"><span data-stu-id="576d2-130">Transient, limited lifetime</span></span>

<span data-ttu-id="576d2-131">**方案**</span><span class="sxs-lookup"><span data-stu-id="576d2-131">**Scenario**</span></span>

<span data-ttu-id="576d2-132">应用需要一个 <xref:System.IDisposable> 实例，该实例在以下任一情况下具有暂时性生存期：</span><span class="sxs-lookup"><span data-stu-id="576d2-132">The app requires an <xref:System.IDisposable> instance with a transient lifetime for either of the following scenarios:</span></span>

- <span data-ttu-id="576d2-133">在根范围（根容器）内解析实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-133">The instance is resolved in the root scope (root container).</span></span>
- <span data-ttu-id="576d2-134">应在作用域结束之前释放实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-134">The instance should be disposed before the scope ends.</span></span>

<span data-ttu-id="576d2-135">**解决方案**</span><span class="sxs-lookup"><span data-stu-id="576d2-135">**Solution**</span></span>

<span data-ttu-id="576d2-136">使用工厂模式在父作用域外创建实例。 </span><span class="sxs-lookup"><span data-stu-id="576d2-136">Use the factory pattern to create an instance outside of the parent scope.</span></span> <span data-ttu-id="576d2-137">在这种情况下，应用通常会使用一个 `Create` 方法，该方法直接调用最终类型的构造函数。</span><span class="sxs-lookup"><span data-stu-id="576d2-137">In this situation, the app would generally have a `Create` method that calls the final type's constructor directly.</span></span> <span data-ttu-id="576d2-138">如果最终类型具有其他依赖项，则工厂可以：</span><span class="sxs-lookup"><span data-stu-id="576d2-138">If the final type has other dependencies, the factory can:</span></span>

- <span data-ttu-id="576d2-139">在其构造函数中接收 <xref:System.IServiceProvider>。</span><span class="sxs-lookup"><span data-stu-id="576d2-139">Receive an <xref:System.IServiceProvider> in its constructor.</span></span>
- <span data-ttu-id="576d2-140">使用 <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> 在容器外部实例化实例，同时将容器用于其依赖项。</span><span class="sxs-lookup"><span data-stu-id="576d2-140">Use <xref:Microsoft.Extensions.DependencyInjection.ActivatorUtilities.CreateInstance%2A?displayProperty=nameWithType> to instantiate the instance outside of the container, while using the container for its dependencies.</span></span>

#### <a name="shared-instance-limited-lifetime"></a><span data-ttu-id="576d2-141">共享实例，有限的生存期</span><span class="sxs-lookup"><span data-stu-id="576d2-141">Shared instance, limited lifetime</span></span>

<span data-ttu-id="576d2-142">**方案**</span><span class="sxs-lookup"><span data-stu-id="576d2-142">**Scenario**</span></span>

<span data-ttu-id="576d2-143">应用需要跨多个服务的共享 <xref:System.IDisposable> 实例，但 <xref:System.IDisposable> 实例应具有有限的生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-143">The app requires a shared <xref:System.IDisposable> instance across multiple services, but the <xref:System.IDisposable> instance should have a limited lifetime.</span></span>

<span data-ttu-id="576d2-144">**解决方案**</span><span class="sxs-lookup"><span data-stu-id="576d2-144">**Solution**</span></span>

<span data-ttu-id="576d2-145">为实例注册作用域生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-145">Register the instance with a scoped lifetime.</span></span> <span data-ttu-id="576d2-146">使用 <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> 创建新 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>。</span><span class="sxs-lookup"><span data-stu-id="576d2-146">Use <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory.CreateScope%2A?displayProperty=nameWithType> to create a new <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>.</span></span> <span data-ttu-id="576d2-147">使用作用域的 <xref:System.IServiceProvider> 获取所需的服务。</span><span class="sxs-lookup"><span data-stu-id="576d2-147">Use the scope's <xref:System.IServiceProvider> to get required services.</span></span> <span data-ttu-id="576d2-148">如果不再需要范围，请将其释放。</span><span class="sxs-lookup"><span data-stu-id="576d2-148">Dispose the scope when it's no longer needed.</span></span>

#### <a name="general-idisposable-guidelines"></a><span data-ttu-id="576d2-149">通用 `IDisposable` 准则</span><span class="sxs-lookup"><span data-stu-id="576d2-149">General `IDisposable` guidelines</span></span>

- <span data-ttu-id="576d2-150">不要为 <xref:System.IDisposable> 实例注册暂时性生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-150">Don't register <xref:System.IDisposable> instances with a transient lifetime.</span></span> <span data-ttu-id="576d2-151">请改用工厂模式。</span><span class="sxs-lookup"><span data-stu-id="576d2-151">Use the factory pattern instead.</span></span>
- <span data-ttu-id="576d2-152">不要在根范围内解析具有暂时性或范围内生存期的 <xref:System.IDisposable> 实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-152">Don't resolve <xref:System.IDisposable> instances with a transient or scoped lifetime in the root scope.</span></span> <span data-ttu-id="576d2-153">唯一的例外是应用创建/重新创建并释放 <xref:System.IServiceProvider> 的情况，但这不是理想模式。</span><span class="sxs-lookup"><span data-stu-id="576d2-153">The only exception to this is if the app creates/recreates and disposes <xref:System.IServiceProvider>, but this isn't an ideal pattern.</span></span>
- <span data-ttu-id="576d2-154">通过 DI 接收 <xref:System.IDisposable> 依赖项不要求接收方自行实现 <xref:System.IDisposable>。</span><span class="sxs-lookup"><span data-stu-id="576d2-154">Receiving an <xref:System.IDisposable> dependency via DI doesn't require that the receiver implement <xref:System.IDisposable> itself.</span></span> <span data-ttu-id="576d2-155"><xref:System.IDisposable> 依赖项的接收方不能对该依赖项调用 <xref:System.IDisposable.Dispose%2A>。</span><span class="sxs-lookup"><span data-stu-id="576d2-155">The receiver of the <xref:System.IDisposable> dependency shouldn't call <xref:System.IDisposable.Dispose%2A> on that dependency.</span></span>
- <span data-ttu-id="576d2-156">使用范围控制服务的生存期。</span><span class="sxs-lookup"><span data-stu-id="576d2-156">Use scopes to control the lifetimes of services.</span></span> <span data-ttu-id="576d2-157"> 作用域不区分层次，并且在各作用域之间没有特定联系。</span><span class="sxs-lookup"><span data-stu-id="576d2-157">Scopes aren't hierarchical, and there's no special connection among scopes.</span></span>

<span data-ttu-id="576d2-158">有关资源清理的详细信息，请参阅[实现 `Dispose` 方法](../../standard/garbage-collection/implementing-dispose.md)或[实现 `DisposeAsync` 方法](../../standard/garbage-collection/implementing-disposeasync.md)。</span><span class="sxs-lookup"><span data-stu-id="576d2-158">For more information on resource cleanup, see [Implement a `Dispose` method](../../standard/garbage-collection/implementing-dispose.md), or [Implement a `DisposeAsync` method](../../standard/garbage-collection/implementing-disposeasync.md).</span></span> <span data-ttu-id="576d2-159">另外，请考虑[容器捕获的可释放的暂时性服务](#disposable-transient-services-captured-by-container)方案，因为它与资源清理相关。</span><span class="sxs-lookup"><span data-stu-id="576d2-159">Additionally, consider the [Disposable transient services captured by container](#disposable-transient-services-captured-by-container) scenario as it relates to resource cleanup.</span></span>

## <a name="default-service-container-replacement"></a><span data-ttu-id="576d2-160">默认服务容器替换</span><span class="sxs-lookup"><span data-stu-id="576d2-160">Default service container replacement</span></span>

<span data-ttu-id="576d2-161">内置的服务容器旨在满足框架和大多数消费者应用的需求。</span><span class="sxs-lookup"><span data-stu-id="576d2-161">The built-in service container is designed to serve the needs of the framework and most consumer apps.</span></span> <span data-ttu-id="576d2-162">我们建议使用内置容器，除非你需要的特定功能不受它支持，例如：</span><span class="sxs-lookup"><span data-stu-id="576d2-162">We recommend using the built-in container unless you need a specific feature that it doesn't support, such as:</span></span>

- <span data-ttu-id="576d2-163">属性注入</span><span class="sxs-lookup"><span data-stu-id="576d2-163">Property injection</span></span>
- <span data-ttu-id="576d2-164">基于名称的注入</span><span class="sxs-lookup"><span data-stu-id="576d2-164">Injection based on name</span></span>
- <span data-ttu-id="576d2-165">子容器</span><span class="sxs-lookup"><span data-stu-id="576d2-165">Child containers</span></span>
- <span data-ttu-id="576d2-166">自定义生存期管理</span><span class="sxs-lookup"><span data-stu-id="576d2-166">Custom lifetime management</span></span>
- <span data-ttu-id="576d2-167">对迟缓初始化的 `Func<T>` 支持</span><span class="sxs-lookup"><span data-stu-id="576d2-167">`Func<T>` support for lazy initialization</span></span>
- <span data-ttu-id="576d2-168">基于约定的注册</span><span class="sxs-lookup"><span data-stu-id="576d2-168">Convention-based registration</span></span>

<span data-ttu-id="576d2-169">以下第三方容器可用于 ASP.NET Core 应用：</span><span class="sxs-lookup"><span data-stu-id="576d2-169">The following third-party containers can be used with ASP.NET Core apps:</span></span>

- [<span data-ttu-id="576d2-170">Autofac</span><span class="sxs-lookup"><span data-stu-id="576d2-170">Autofac</span></span>](https://autofac.readthedocs.io/en/latest/integration/aspnetcore.html)
- [<span data-ttu-id="576d2-171">DryIoc</span><span class="sxs-lookup"><span data-stu-id="576d2-171">DryIoc</span></span>](https://www.nuget.org/packages/DryIoc.Microsoft.DependencyInjection)
- [<span data-ttu-id="576d2-172">Grace</span><span class="sxs-lookup"><span data-stu-id="576d2-172">Grace</span></span>](https://www.nuget.org/packages/Grace.DependencyInjection.Extensions)
- [<span data-ttu-id="576d2-173">LightInject</span><span class="sxs-lookup"><span data-stu-id="576d2-173">LightInject</span></span>](https://github.com/seesharper/LightInject.Microsoft.DependencyInjection)
- [<span data-ttu-id="576d2-174">Lamar</span><span class="sxs-lookup"><span data-stu-id="576d2-174">Lamar</span></span>](https://jasperfx.github.io/lamar/)
- [<span data-ttu-id="576d2-175">Stashbox</span><span class="sxs-lookup"><span data-stu-id="576d2-175">Stashbox</span></span>](https://github.com/z4kn4fein/stashbox-extensions-dependencyinjection)
- [<span data-ttu-id="576d2-176">Unity</span><span class="sxs-lookup"><span data-stu-id="576d2-176">Unity</span></span>](https://www.nuget.org/packages/Unity.Microsoft.DependencyInjection)

## <a name="thread-safety"></a><span data-ttu-id="576d2-177">线程安全</span><span class="sxs-lookup"><span data-stu-id="576d2-177">Thread safety</span></span>

<span data-ttu-id="576d2-178">创建线程安全的单一实例服务。</span><span class="sxs-lookup"><span data-stu-id="576d2-178">Create thread-safe singleton services.</span></span> <span data-ttu-id="576d2-179">如果单一实例服务依赖于一个暂时服务，那么暂时服务可能也需要线程安全，具体取决于单一实例使用它的方式。</span><span class="sxs-lookup"><span data-stu-id="576d2-179">If a singleton service has a dependency on a transient service, the transient service may also require thread safety depending on how it's used by the singleton.</span></span>

<span data-ttu-id="576d2-180">单一实例服务的工厂方法（例如 [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton%2A) 的第二个参数）不必是线程安全的。</span><span class="sxs-lookup"><span data-stu-id="576d2-180">The factory method of a singleton service, such as the second argument to [AddSingleton\<TService>(IServiceCollection, Func\<IServiceProvider,TService>)](xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionServiceExtensions.AddSingleton%2A), doesn't need to be thread-safe.</span></span> <span data-ttu-id="576d2-181">像类型 (`static`) 构造函数一样，它保证仅由单个线程调用一次。</span><span class="sxs-lookup"><span data-stu-id="576d2-181">Like a type (`static`) constructor, it's guaranteed to be called only once by a single thread.</span></span>

## <a name="recommendations"></a><span data-ttu-id="576d2-182">建议</span><span class="sxs-lookup"><span data-stu-id="576d2-182">Recommendations</span></span>

- <span data-ttu-id="576d2-183">不支持基于`async/await` 和 `Task` 的服务解析。</span><span class="sxs-lookup"><span data-stu-id="576d2-183">`async/await` and `Task` based service resolution isn't supported.</span></span> <span data-ttu-id="576d2-184">由于 C# 不支持异步构造函数，因此请在同步解析服务后使用异步方法。</span><span class="sxs-lookup"><span data-stu-id="576d2-184">Because C# doesn't support asynchronous constructors, use asynchronous methods after synchronously resolving the service.</span></span>
- <span data-ttu-id="576d2-185">避免在服务容器中直接存储数据和配置。</span><span class="sxs-lookup"><span data-stu-id="576d2-185">Avoid storing data and configuration directly in the service container.</span></span> <span data-ttu-id="576d2-186">例如，用户的购物车通常不应添加到服务容器中。</span><span class="sxs-lookup"><span data-stu-id="576d2-186">For example, a user's shopping cart shouldn't typically be added to the service container.</span></span> <span data-ttu-id="576d2-187">配置应使用 选项模型。</span><span class="sxs-lookup"><span data-stu-id="576d2-187">Configuration should use the options pattern.</span></span> <span data-ttu-id="576d2-188">同样，避免“数据持有者”对象，也就是仅仅为实现对另一个对象的访问而存在的对象。</span><span class="sxs-lookup"><span data-stu-id="576d2-188">Similarly, avoid "data holder" objects that only exist to allow access to another object.</span></span> <span data-ttu-id="576d2-189">最好通过 DI 请求实际项。</span><span class="sxs-lookup"><span data-stu-id="576d2-189">It's better to request the actual item via DI.</span></span>
- <span data-ttu-id="576d2-190">避免静态访问服务。</span><span class="sxs-lookup"><span data-stu-id="576d2-190">Avoid static access to services.</span></span> <span data-ttu-id="576d2-191">例如，避免将 [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) 捕获为静态字段或属性以便在其他地方使用。</span><span class="sxs-lookup"><span data-stu-id="576d2-191">For example, avoid capturing [IApplicationBuilder.ApplicationServices](xref:Microsoft.AspNetCore.Builder.IApplicationBuilder.ApplicationServices) as a static field or property for use elsewhere.</span></span>
- <span data-ttu-id="576d2-192">使 [DI 工厂](#async-di-factories-can-cause-deadlocks)保持快速且同步。</span><span class="sxs-lookup"><span data-stu-id="576d2-192">Keep [DI factories](#async-di-factories-can-cause-deadlocks) fast and synchronous.</span></span>
- <span data-ttu-id="576d2-193">避免使用[服务定位器模式](#scoped-service-as-singleton)。</span><span class="sxs-lookup"><span data-stu-id="576d2-193">Avoid using the [*service locator pattern*](#scoped-service-as-singleton).</span></span> <span data-ttu-id="576d2-194">例如，可以改为使用 DI 时，不要调用 <xref:System.IServiceProvider.GetService%2A> 来获取服务实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-194">For example, don't invoke <xref:System.IServiceProvider.GetService%2A> to obtain a service instance when you can use DI instead.</span></span>
- <span data-ttu-id="576d2-195">要避免的另一个服务定位器变体是注入需在运行时解析依赖项的工厂。</span><span class="sxs-lookup"><span data-stu-id="576d2-195">Another service locator variation to avoid is injecting a factory that resolves dependencies at runtime.</span></span> <span data-ttu-id="576d2-196">这两种做法混合了[控制反转](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion)策略。</span><span class="sxs-lookup"><span data-stu-id="576d2-196">Both of these practices mix [Inversion of Control](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#dependency-inversion) strategies.</span></span>
- <span data-ttu-id="576d2-197">避免在 `ConfigureServices` 中调用 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A>。</span><span class="sxs-lookup"><span data-stu-id="576d2-197">Avoid calls to <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider%2A> in `ConfigureServices`.</span></span> <span data-ttu-id="576d2-198">当开发人员想要在 `ConfigureServices` 中解析服务时，通常会调用 `BuildServiceProvider`。</span><span class="sxs-lookup"><span data-stu-id="576d2-198">Calling `BuildServiceProvider` typically happens when the developer wants to resolve a service in `ConfigureServices`.</span></span>
- <span data-ttu-id="576d2-199">[可释放的暂时性服务由容器捕获](#disposable-transient-services-captured-by-container)以进行释放。</span><span class="sxs-lookup"><span data-stu-id="576d2-199">[Disposable transient services are captured](#disposable-transient-services-captured-by-container) by the container for disposal.</span></span> <span data-ttu-id="576d2-200">如果从顶级容器解析，这会变为内存泄漏。</span><span class="sxs-lookup"><span data-stu-id="576d2-200">This can turn into a memory leak if resolved from the top-level container.</span></span>
- <span data-ttu-id="576d2-201">启用范围验证，确保应用没有捕获范围内服务的单一实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-201">Enable scope validation to make sure the app doesn't have singletons that capture scoped services.</span></span> <span data-ttu-id="576d2-202">有关详细信息，请参阅[作用域验证](dependency-injection.md#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="576d2-202">For more information, see [Scope validation](dependency-injection.md#scope-validation).</span></span>

<span data-ttu-id="576d2-203">像任何一组建议一样，你可能会遇到需要忽略某建议的情况。</span><span class="sxs-lookup"><span data-stu-id="576d2-203">Like all sets of recommendations, you may encounter situations where ignoring a recommendation is required.</span></span> <span data-ttu-id="576d2-204">例外情况很少见，主要是框架本身内部的特殊情况。</span><span class="sxs-lookup"><span data-stu-id="576d2-204">Exceptions are rare, mostly special cases within the framework itself.</span></span>

<span data-ttu-id="576d2-205">DI 是静态/全局对象访问模式的替代方法。</span><span class="sxs-lookup"><span data-stu-id="576d2-205">DI is an *alternative* to static/global object access patterns.</span></span> <span data-ttu-id="576d2-206">如果将其与静态对象访问混合使用，则可能无法意识到 DI 的优点。</span><span class="sxs-lookup"><span data-stu-id="576d2-206">You may not be able to realize the benefits of DI if you mix it with static object access.</span></span>

## <a name="example-anti-patterns"></a><span data-ttu-id="576d2-207">示例反模式</span><span class="sxs-lookup"><span data-stu-id="576d2-207">Example anti-patterns</span></span>

<span data-ttu-id="576d2-208">除了本文中介绍的指导原则之外，还应避免几种反模式。</span><span class="sxs-lookup"><span data-stu-id="576d2-208">In addition to the guidelines in this article, there are several anti-patterns *you **should** avoid*.</span></span> <span data-ttu-id="576d2-209">其中的某些反模式是开发运行时本身的知识。</span><span class="sxs-lookup"><span data-stu-id="576d2-209">Some of these anti-patterns are learnings from developing the runtimes themselves.</span></span>

> [!WARNING]
> <span data-ttu-id="576d2-210">这些是示例反模式，请不要复制代码，不要使用这些模式，并不惜一切代价避免使用这些模式。</span><span class="sxs-lookup"><span data-stu-id="576d2-210">These are example anti-patterns, *do not* copy the code, *do not* use these patterns, and avoid these patterns at all costs.</span></span>

### <a name="disposable-transient-services-captured-by-container"></a><span data-ttu-id="576d2-211">由容器捕获的可释放的暂时性服务</span><span class="sxs-lookup"><span data-stu-id="576d2-211">Disposable transient services captured by container</span></span>

<span data-ttu-id="576d2-212">当你注册可实现 <xref:System.IDisposable> 的暂时性服务时，默认情况下，DI 容器将捕获这些引用，而不是这些引用的 <xref:System.IDisposable.Dispose>，如果它们是从容器中解析的，则直到应用程序停止且容器被释放，如果它们是从作用域中解析的，则直到作用域被释放。</span><span class="sxs-lookup"><span data-stu-id="576d2-212">When you register *Transient* services that implement <xref:System.IDisposable>, by default the DI container will hold onto these references, and not <xref:System.IDisposable.Dispose> of them until the container is disposed when application stops if they were resolved from the container, or until the scope is disposed if they were resolved from a scope.</span></span> <span data-ttu-id="576d2-213">如果从容器级解析，这会变为内存泄漏。</span><span class="sxs-lookup"><span data-stu-id="576d2-213">This can turn into a memory leak if resolved from container level.</span></span>

:::code language="csharp" source="snippets/configuration/di-anti-patterns/Program.cs" range="18-30":::

<span data-ttu-id="576d2-214">在上述反模式中，1,000 个 `ExampleDisposable` 对象将被实例化并为根对象。</span><span class="sxs-lookup"><span data-stu-id="576d2-214">In the preceding anti-pattern, 1,000 `ExampleDisposable` objects are instantiated and rooted.</span></span> <span data-ttu-id="576d2-215">在释放 `serviceProvider` 实例之前，它们不会被释放。</span><span class="sxs-lookup"><span data-stu-id="576d2-215">They will not be disposed of until the `serviceProvider` instance is disposed.</span></span>

<span data-ttu-id="576d2-216">有关调试内存泄漏的详细信息，请参阅[调试 .NET 中的内存泄漏](../diagnostics/debug-memory-leak.md)。</span><span class="sxs-lookup"><span data-stu-id="576d2-216">For more information on debugging memory leaks, see [Debug a memory leak in .NET](../diagnostics/debug-memory-leak.md).</span></span>

### <a name="async-di-factories-can-cause-deadlocks"></a><span data-ttu-id="576d2-217">异步 DI 工厂可能导致死锁</span><span class="sxs-lookup"><span data-stu-id="576d2-217">Async DI factories can cause deadlocks</span></span>

<span data-ttu-id="576d2-218">术语“DI 工厂”指的是在调用 `Add{LIFETIME}` 时存在的重载方法。</span><span class="sxs-lookup"><span data-stu-id="576d2-218">The term "DI factories" refers to the overload methods that exist when calling `Add{LIFETIME}`.</span></span> <span data-ttu-id="576d2-219">有一些重载接受 `Func<IServiceProvider, T>`，其中 `T` 是要注册的服务，参数命名为 `implementationFactory`。</span><span class="sxs-lookup"><span data-stu-id="576d2-219">There are overloads accepting a `Func<IServiceProvider, T>` where `T` is the service being registered, and the parameter is named `implementationFactory`.</span></span> <span data-ttu-id="576d2-220">`implementationFactory` 可以作为 Lambda 表达式、局部函数或方法提供。</span><span class="sxs-lookup"><span data-stu-id="576d2-220">The `implementationFactory` can be provided as a lambda expression, local function, or method.</span></span> <span data-ttu-id="576d2-221">如果工厂是异步的，并且你使用 <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType>，则会导致死锁。</span><span class="sxs-lookup"><span data-stu-id="576d2-221">If the factory is asynchronous, and you use <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType>, this will cause a deadlock.</span></span>

:::code language="csharp" source="snippets/configuration/di-anti-patterns/Program.cs" range="32-45" highlight="4-8":::

<span data-ttu-id="576d2-222">在前面的代码中，为 `implementationFactory` 指定了一个 Lambda 表达式，其中主体在 `Task<Bar>` 返回方法上调用 <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="576d2-222">In the preceding code, the `implementationFactory` is given a lambda expression where the body calls <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType> on a `Task<Bar>` returning method.</span></span> <span data-ttu-id="576d2-223">这会导致死锁。</span><span class="sxs-lookup"><span data-stu-id="576d2-223">This ***causes a deadlock***.</span></span> <span data-ttu-id="576d2-224">`GetBarAsync` 方法只使用 <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType> 模拟异步工作操作，然后调用 <xref:Microsoft.Extensions.DependencyInjection.ServiceProviderServiceExtensions.GetRequiredService%60%601(System.IServiceProvider)>。</span><span class="sxs-lookup"><span data-stu-id="576d2-224">The `GetBarAsync` method simply emulates an asynchronous work operation with <xref:System.Threading.Tasks.Task.Delay%2A?displayProperty=nameWithType>, and then calls <xref:Microsoft.Extensions.DependencyInjection.ServiceProviderServiceExtensions.GetRequiredService%60%601(System.IServiceProvider)>.</span></span>

:::code language="csharp" source="snippets/configuration/di-anti-patterns/Program.cs" range="47-53":::

<span data-ttu-id="576d2-225">有关异步指南的详细信息，请参阅[异步编程：重要信息和建议](../../csharp/async.md#important-info-and-advice)。</span><span class="sxs-lookup"><span data-stu-id="576d2-225">For more information on asynchronous guidance, see [Asynchronous programming: Important info and advice](../../csharp/async.md#important-info-and-advice).</span></span> <span data-ttu-id="576d2-226">有关调试死锁的详细信息，请参阅[调试 .NET 中的死锁](../diagnostics/debug-deadlock.md)。</span><span class="sxs-lookup"><span data-stu-id="576d2-226">For more information debugging deadlocks, see [Debug a deadlock in .NET](../diagnostics/debug-deadlock.md).</span></span>

<span data-ttu-id="576d2-227">在运行此反模式并发生死锁时，可以从 Visual Studio 的“并行堆栈”窗口查看等待的两个线程。</span><span class="sxs-lookup"><span data-stu-id="576d2-227">When you're running this anti-pattern and the deadlock occurs, you can view the two threads waiting from Visual Studio's Parallel Stacks window.</span></span> <span data-ttu-id="576d2-228">有关详细信息，请参阅[在“并行堆栈”窗口中查看线程和任务](/visualstudio/debugger/using-the-parallel-stacks-window)。</span><span class="sxs-lookup"><span data-stu-id="576d2-228">For more information, see [View threads and tasks in the Parallel Stacks window](/visualstudio/debugger/using-the-parallel-stacks-window).</span></span>

### <a name="captive-dependency"></a><span data-ttu-id="576d2-229">捕获依赖项</span><span class="sxs-lookup"><span data-stu-id="576d2-229">Captive dependency</span></span>

<span data-ttu-id="576d2-230">术语“[捕获依赖项](https://blog.ploeh.dk/2014/06/02/captive-dependency)”由 [Mark Seeman](https://blog.ploeh.dk/about) 提出，指的是服务生存期的配置不正确，其中具有较长生存期的服务捕获了具有较短生存期的服务。</span><span class="sxs-lookup"><span data-stu-id="576d2-230">The term ["captive dependency"](https://blog.ploeh.dk/2014/06/02/captive-dependency) was coined by [Mark Seeman](https://blog.ploeh.dk/about), and refers to the misconfiguration of service lifetimes, where a longer-lived service holds a shorter-lived service captive.</span></span>

:::code language="csharp" source="snippets/configuration/di-anti-patterns/Program.cs" range="55-65":::

<span data-ttu-id="576d2-231">在前面的代码中，`Foo` 注册为单一实例，并确定了 `Bar` 的作用域，这在表面上看似乎是有效的。</span><span class="sxs-lookup"><span data-stu-id="576d2-231">In the preceding code, `Foo` is registered as a singleton and `Bar` is scoped - which on the surface seems valid.</span></span> <span data-ttu-id="576d2-232">不过，请考虑 `Foo` 的实现。</span><span class="sxs-lookup"><span data-stu-id="576d2-232">However, consider the implementation of `Foo`.</span></span>

:::code language="csharp" source="snippets/configuration/di-anti-patterns/Foo.cs" highlight="5":::

<span data-ttu-id="576d2-233">`Foo` 对象需要一个 `Bar` 对象，因为 `Foo` 是单一实例，并且已确定 `Bar` 的作用域 - 这是错误的配置。</span><span class="sxs-lookup"><span data-stu-id="576d2-233">The `Foo` object requires a `Bar` object, and since `Foo` is a singleton, and `Bar` is scoped - this is a misconfiguration.</span></span> <span data-ttu-id="576d2-234">因此，将只会实例化 `Foo` 一次，并且它会在自己的生存期内捕获 `Bar`，这比所需的作用域内的 `Bar` 生存期要长。</span><span class="sxs-lookup"><span data-stu-id="576d2-234">As is, `Foo` would only be instantiated once, and it would hold onto `Bar` for its lifetime, which is longer than the intended scoped lifetime of `Bar`.</span></span> <span data-ttu-id="576d2-235">应考虑通过将 `validateScopes: true` 传递到 <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider(Microsoft.Extensions.DependencyInjection.IServiceCollection,System.Boolean)> 来验证作用域。</span><span class="sxs-lookup"><span data-stu-id="576d2-235">You should consider validating scopes, by passing `validateScopes: true` to the <xref:Microsoft.Extensions.DependencyInjection.ServiceCollectionContainerBuilderExtensions.BuildServiceProvider(Microsoft.Extensions.DependencyInjection.IServiceCollection,System.Boolean)>.</span></span> <span data-ttu-id="576d2-236">验证作用域时，你将收到 <xref:System.InvalidOperationException> 和一条消息，类似于“无法使用来自单一实例‘Foo’的作用域内的服务‘Bar’”。</span><span class="sxs-lookup"><span data-stu-id="576d2-236">When you validate the scopes, you'd get an <xref:System.InvalidOperationException> with a message similar to "Cannot consume scoped service 'Bar' from singleton 'Foo'.".</span></span>

<span data-ttu-id="576d2-237">有关详细信息，请参阅[作用域验证](dependency-injection.md#scope-validation)。</span><span class="sxs-lookup"><span data-stu-id="576d2-237">For more information, see [Scope validation](dependency-injection.md#scope-validation).</span></span>

### <a name="scoped-service-as-singleton"></a><span data-ttu-id="576d2-238">作为单一实例的作用域服务</span><span class="sxs-lookup"><span data-stu-id="576d2-238">Scoped service as singleton</span></span>

<span data-ttu-id="576d2-239">使用作用域内服务时，如果你不是在现有作用域内创建作用域，则该服务将成为单一实例。</span><span class="sxs-lookup"><span data-stu-id="576d2-239">When using scoped services, if you're not creating a scope or within an existing scope - the service becomes a singleton.</span></span>

:::code language="csharp" source="snippets/configuration/di-anti-patterns/Program.cs" range="68-82" highlight="13-14":::

<span data-ttu-id="576d2-240">在前面的代码中，在 <xref:Microsoft.Extensions.DependencyInjection.IServiceScope> 中检索 `Bar`，这是正确的。</span><span class="sxs-lookup"><span data-stu-id="576d2-240">In the preceding code, `Bar` is retrieved within an <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is correct.</span></span> <span data-ttu-id="576d2-241">反模式是作用域外的 `Bar` 检索，变量命名为 `avoid` 以显示不正确的示例检索。</span><span class="sxs-lookup"><span data-stu-id="576d2-241">The anti-pattern is the retrieval of `Bar` outside of the scope, and the variable is named `avoid` to show which example retrieval is incorrect.</span></span>

## <a name="see-also"></a><span data-ttu-id="576d2-242">另请参阅</span><span class="sxs-lookup"><span data-stu-id="576d2-242">See also</span></span>

- [<span data-ttu-id="576d2-243">.NET 中的依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="576d2-243">Dependency injection in .NET</span></span>](dependency-injection.md)
- [<span data-ttu-id="576d2-244">教程：在 .NET 中使用依赖关系注入</span><span class="sxs-lookup"><span data-stu-id="576d2-244">Tutorial: Use dependency injection in .NET</span></span>](dependency-injection-usage.md)
