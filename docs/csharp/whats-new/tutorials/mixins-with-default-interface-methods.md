---
title: 使用默认接口方法创建 mixin 类型
description: 使用默认接口成员，可以通过实现器的可选默认实现来扩展接口。
ms.technology: csharp-advanced-concepts
ms.date: 10/04/2019
ms.openlocfilehash: 9137d42b1795b20385d2dde16548452eeb9807c3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878872"
---
# <a name="tutorial-mix-functionality-in-when-creating-classes-using-interfaces-with-default-interface-methods"></a><span data-ttu-id="ffc3f-103">教程：当通过默认接口方法创建使用接口的类时实现的混入功能</span><span class="sxs-lookup"><span data-stu-id="ffc3f-103">Tutorial: Mix functionality in when creating classes using interfaces with default interface methods</span></span>

<span data-ttu-id="ffc3f-104">从 .NET Core 3.0 上的 C# 8.0 开始，可以在声明接口成员时定义实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-104">Beginning with C# 8.0 on .NET Core 3.0, you can define an implementation when you declare a member of an interface.</span></span> <span data-ttu-id="ffc3f-105">此功能提供了一些新功能，可以在其中为接口中声明的功能定义默认实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-105">This feature provides new capabilities where you can define default implementations for features declared in interfaces.</span></span> <span data-ttu-id="ffc3f-106">类可以选择何时替代功能、何时使用默认功能以及何时不声明对离散功能的支持。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-106">Classes can pick when to override functionality, when to use the default functionality, and when not to declare support for discrete features.</span></span>

<span data-ttu-id="ffc3f-107">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-107">In this tutorial, you'll learn how to:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="ffc3f-108">使用描述离散功能的实现创建接口。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-108">Create interfaces with implementations that describe discrete features.</span></span>
> * <span data-ttu-id="ffc3f-109">创建使用默认实现的类。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-109">Create classes that use the default implementations.</span></span>
> * <span data-ttu-id="ffc3f-110">创建用于替代部分或全部默认实现的类。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-110">Create classes that override some or all of the default implementations.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ffc3f-111">先决条件</span><span class="sxs-lookup"><span data-stu-id="ffc3f-111">Prerequisites</span></span>

<span data-ttu-id="ffc3f-112">需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-112">You’ll need to set up your machine to run .NET Core, including the C# 8.0 compiler.</span></span> <span data-ttu-id="ffc3f-113">自 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本起，开始随附 C# 8.0 编译器。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-113">The C# 8.0 compiler is available starting with [Visual Studio 2019 version 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019), or the [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet) or later.</span></span>

## <a name="limitations-of-extension-methods"></a><span data-ttu-id="ffc3f-114">扩展方法的限制</span><span class="sxs-lookup"><span data-stu-id="ffc3f-114">Limitations of extension methods</span></span>

<span data-ttu-id="ffc3f-115">实现作为接口一部分出现的行为的一种方法是：定义可提供默认行为的[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-115">One way you can implement behavior that appears as part of an interface is to define [extension methods](../../programming-guide/classes-and-structs/extension-methods.md) that provide the default behavior.</span></span> <span data-ttu-id="ffc3f-116">接口声明最少的一组成员，同时为实现该接口的任何类提供更大的外围应用。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-116">Interfaces declare a minimum set of members while providing a greater surface area for any class that implements that interface.</span></span> <span data-ttu-id="ffc3f-117">例如，<xref:System.Linq.Enumerable> 中的扩展方法提供作为 LINQ 查询源的任何序列的实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-117">For example, the extension methods in <xref:System.Linq.Enumerable> provide the implementation for any sequence to be the source of a LINQ query.</span></span>

<span data-ttu-id="ffc3f-118">扩展方法是使用变量的声明类型在编译时解析的。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-118">Extension methods are resolved at compile time, using the declared type of the variable.</span></span> <span data-ttu-id="ffc3f-119">实现接口的类可以为任何扩展方法提供更好的实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-119">Classes that implement the interface can provide a better implementation for any extension method.</span></span> <span data-ttu-id="ffc3f-120">变量声明必须与实现类型匹配，以使编译器能够选择该实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-120">Variable declarations must match the implementing type to enable the compiler to choose that implementation.</span></span> <span data-ttu-id="ffc3f-121">当编译时类型与接口匹配时，方法调用解析为扩展方法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-121">When the compile-time type matches the interface, method calls resolve to the extension method.</span></span> <span data-ttu-id="ffc3f-122">扩展方法的另一个问题是，只要可访问包含扩展方法的类，就可以访问这些方法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-122">Another concern with extension methods is that those methods are accessible wherever the class containing the extension methods is accessible.</span></span> <span data-ttu-id="ffc3f-123">类不能声明是否应提供扩展方法中声明的功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-123">Classes cannot declare if they should or should not provide features declared in extension methods.</span></span>

<span data-ttu-id="ffc3f-124">从 C# 8.0 开始，可以将默认实现声明为接口方法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-124">Starting with C# 8.0, you can declare the default implementations as interface methods.</span></span> <span data-ttu-id="ffc3f-125">然后，每个类将自动使用默认实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-125">Then, every class automatically uses the default implementation.</span></span> <span data-ttu-id="ffc3f-126">可提供更好实现的任何类都可以使用更好的算法来替代接口方法定义。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-126">Any class that can provide a better implementation can override the interface method definition with a better algorithm.</span></span> <span data-ttu-id="ffc3f-127">从某种意义上讲，这种技术听起来与你使用[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)的方式类似。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-127">In one sense, this technique sounds similar to how you could use [extension methods](../../programming-guide/classes-and-structs/extension-methods.md).</span></span>

<span data-ttu-id="ffc3f-128">在本文中，你将了解默认接口实现如何启用新方案。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-128">In this article, you'll learn how default interface implementations enable new scenarios.</span></span>

## <a name="design-the-application"></a><span data-ttu-id="ffc3f-129">设计应用程序</span><span class="sxs-lookup"><span data-stu-id="ffc3f-129">Design the application</span></span>

<span data-ttu-id="ffc3f-130">考虑使用一个家庭自动化应用程序。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-130">Consider a home automation application.</span></span> <span data-ttu-id="ffc3f-131">你可能在整个房子中使用许多不同类型的灯和指示灯。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-131">You probably have many different types of lights and indicators that could be used throughout the house.</span></span> <span data-ttu-id="ffc3f-132">每个灯都必须支持 API 以使其打开和关闭，以及报告当前状态。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-132">Every light must support APIs to turn them on and off, and to report the current state.</span></span> <span data-ttu-id="ffc3f-133">一些灯和指示灯可能支持其他功能，例如：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-133">Some lights and indicators may support other features, such as:</span></span>

- <span data-ttu-id="ffc3f-134">打开灯，然后定时关闭。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-134">Turn light on, then turn it off after a timer.</span></span>
- <span data-ttu-id="ffc3f-135">使灯闪烁一段时间。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-135">Blink the light for a period of time.</span></span>

<span data-ttu-id="ffc3f-136">在支持最小集的设备中，可以模拟其中的某些扩展功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-136">Some of these extended capabilities could be emulated in devices that support the minimal set.</span></span> <span data-ttu-id="ffc3f-137">这表示提供了默认实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-137">That indicates providing a default implementation.</span></span> <span data-ttu-id="ffc3f-138">对于内置了更多功能的设备，设备软件将使用本机功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-138">For those devices that have more capabilities built in, the device software would use the native capabilities.</span></span> <span data-ttu-id="ffc3f-139">对于其他灯，它们可以选择实现接口并使用默认实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-139">For other lights, they could choose to implement the interface and use the default implementation.</span></span>

<span data-ttu-id="ffc3f-140">对于此方案，默认接口成员是比扩展方法更好的解决方案。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-140">Default interface members is a better solution for this scenario than extension methods.</span></span> <span data-ttu-id="ffc3f-141">类创建者可以控制它们选择实现的接口。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-141">Class authors can control which interfaces they choose to implement.</span></span> <span data-ttu-id="ffc3f-142">它们选择的接口可用作方法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-142">Those interfaces they choose are available as methods.</span></span> <span data-ttu-id="ffc3f-143">此外，由于默认情况下默认的接口方法是虚拟的，因此该方法调度始终选择类中的实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-143">In addition, because default interface methods are virtual by default, the method dispatch always chooses the implementation in the class.</span></span>

<span data-ttu-id="ffc3f-144">我们创建代码来演示这些差异。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-144">Let's create the code to demonstrate these differences.</span></span>

## <a name="create-interfaces"></a><span data-ttu-id="ffc3f-145">创建接口</span><span class="sxs-lookup"><span data-stu-id="ffc3f-145">Create interfaces</span></span>

<span data-ttu-id="ffc3f-146">首先创建用于定义所有灯的行为的接口：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-146">Start by creating the interface that defines the behavior for all lights:</span></span>

[!code-csharp[Declare base interface](./snippets/mixins-with-default-interface-methods/UnusedExampleCode.cs?name=SnippetILightInterfaceV1)]

<span data-ttu-id="ffc3f-147">基本的高架灯具可能会实现此接口，如下面的代码所示：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-147">A basic overhead light fixture might implement this interface as shown in the following code:</span></span>

[!code-csharp[First overhead light](./snippets/mixins-with-default-interface-methods/UnusedExampleCode.cs?name=SnippetOverheadLightV1)]

<span data-ttu-id="ffc3f-148">在本教程中，代码不驱动 IoT 设备，但会通过将消息写入控制台的方式来模拟这些活动。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-148">In this tutorial, the code doesn't drive IoT devices, but emulates those activities by writing messages to the console.</span></span> <span data-ttu-id="ffc3f-149">可以浏览代码，但不执行房屋的自动化。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-149">You can explore the code without automating your house.</span></span>

<span data-ttu-id="ffc3f-150">接下来，我们定义一个可在超时后自动关闭的灯的接口：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-150">Next, let's define the interface for a light that can automatically turn off after a timeout:</span></span>

[!code-csharp[pure Timer interface](./snippets/mixins-with-default-interface-methods/UnusedExampleCode.cs?name=SnippetPureTimerInterface)]

<span data-ttu-id="ffc3f-151">可以向高架灯具添加基本实现，但是更好的解决方案是修改此接口定义以提供 `virtual` 默认实现：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-151">You could add a basic implementation to the overhead light, but a better solution is to modify this interface definition to provide a `virtual` default implementation:</span></span>

[!code-csharp[Timer interface](./snippets/mixins-with-default-interface-methods/ITimerLight.cs?name=SnippetTimerLightFinal)]

<span data-ttu-id="ffc3f-152">添加该更改后，`OverheadLight` 类可以通过声明对接口的支持来实现计时器功能：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-152">By adding that change, the `OverheadLight` class can implement the timer function by declaring support for the interface:</span></span>

```csharp
public class OverheadLight : ITimerLight { }
```

<span data-ttu-id="ffc3f-153">另一种灯类型可能支持更复杂的协议。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-153">A different light type may support a more sophisticated protocol.</span></span> <span data-ttu-id="ffc3f-154">它可以为 `TurnOnFor` 提供自己的实现，如以下代码所示：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-154">It can provide its own implementation for `TurnOnFor`, as shown in the following code:</span></span>

[!code-csharp[Override the timer function](./snippets/mixins-with-default-interface-methods/HalogenLight.cs?name=SnippetHalogenLight)]

<span data-ttu-id="ffc3f-155">与替代虚拟类方法不同，`HalogenLight` 类中 `TurnOnFor` 的声明不使用 `override` 关键字。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-155">Unlike overriding virtual class methods, the declaration of `TurnOnFor` in the `HalogenLight` class does not use the `override` keyword.</span></span>

## <a name="mix-and-match-capabilities"></a><span data-ttu-id="ffc3f-156">混合和匹配功能</span><span class="sxs-lookup"><span data-stu-id="ffc3f-156">Mix and match capabilities</span></span>

<span data-ttu-id="ffc3f-157">当你引入更高级的功能时，默认界面方法的优势变得更加明显。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-157">The advantages of default interface methods become clearer as you introduce more advanced capabilities.</span></span> <span data-ttu-id="ffc3f-158">使用接口让你可以混合和匹配功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-158">Using interfaces enables you to mix and match capabilities.</span></span> <span data-ttu-id="ffc3f-159">它还使每个类创建者可以在默认实现和自定义实现之间进行选择。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-159">It also enables each class author to choose between the default implementation and a custom implementation.</span></span> <span data-ttu-id="ffc3f-160">我们使用闪烁灯的默认实现来添加一个接口：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-160">Let's add an interface with a default implementation for a blinking light:</span></span>

[!code-csharp[Define the blinking light interface](./snippets/mixins-with-default-interface-methods/IBlinkingLight.cs?name=SnippetBlinkingLight)]

<span data-ttu-id="ffc3f-161">默认实现使任何灯可以闪烁。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-161">The default implementation enables any light to blink.</span></span> <span data-ttu-id="ffc3f-162">高架灯可以使用默认实现添加计时器和闪烁功能：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-162">The overhead light can add both timer and blink capabilities using the default implementation:</span></span>

[!code-csharp[Use the default blink function](./snippets/mixins-with-default-interface-methods/OverheadLight.cs?name=SnippetOverheadLight)]

<span data-ttu-id="ffc3f-163">新的灯类型 `LEDLight` 直接支持计时器功能和闪烁功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-163">A new light type, the `LEDLight` supports both the timer function and the blink function directly.</span></span> <span data-ttu-id="ffc3f-164">这种灯样式同时实现 `ITimerLight` 和 `IBlinkingLight` 接口，并替代 `Blink` 方法：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-164">This light style implements both the `ITimerLight` and `IBlinkingLight` interfaces, and overrides the `Blink` method:</span></span>

[!code-csharp[Override the blink function](./snippets/mixins-with-default-interface-methods/LEDLight.cs?name=SnippetLEDLight)]

<span data-ttu-id="ffc3f-165">`ExtraFancyLight` 可以直接支持闪烁功能和计时器功能：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-165">An `ExtraFancyLight` might support both blink and timer functions directly:</span></span>

[!code-csharp[Override the blink and timer function](./snippets/mixins-with-default-interface-methods/ExtraFancyLight.cs?name=SnippetExtraFancyLight)]

<span data-ttu-id="ffc3f-166">之前创建的 `HalogenLight` 不支持闪烁。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-166">The `HalogenLight` you created earlier doesn't support blinking.</span></span> <span data-ttu-id="ffc3f-167">因此，不要将 `IBlinkingLight` 添加到其支持的接口列表。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-167">So, don't add the `IBlinkingLight` to the list of its supported interfaces.</span></span>

## <a name="detect-the-light-types-using-pattern-matching"></a><span data-ttu-id="ffc3f-168">使用模式匹配检测灯类型</span><span class="sxs-lookup"><span data-stu-id="ffc3f-168">Detect the light types using pattern matching</span></span>

<span data-ttu-id="ffc3f-169">接下来，我们编写一些测试代码。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-169">Next, let's write some test code.</span></span> <span data-ttu-id="ffc3f-170">可以使用 C# 的[模式匹配](../../pattern-matching.md)功能，通过检查灯支持的接口来确定灯的功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-170">You can make use of C#'s [pattern matching](../../pattern-matching.md) feature to determine a light's capabilities by examining which interfaces it supports.</span></span>  <span data-ttu-id="ffc3f-171">下面的方法将实践每个灯的支持功能：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-171">The following method exercises the supported capabilities of each light:</span></span>

[!code-csharp[Test a light's capabilities](./snippets/mixins-with-default-interface-methods/Program.cs?name=SnippetTestLightFunctions)]

<span data-ttu-id="ffc3f-172">`Main` 方法中的以下代码按顺序创建每种灯类型，并测试相应的灯：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-172">The following code in your `Main` method creates each light type in sequence and tests that light:</span></span>

[!code-csharp[Test a light's capabilities](./snippets/mixins-with-default-interface-methods/Program.cs?name=SnippetMainMethod)]

## <a name="how-the-compiler-determines-best-implementation"></a><span data-ttu-id="ffc3f-173">编译器如何确定最佳实现</span><span class="sxs-lookup"><span data-stu-id="ffc3f-173">How the compiler determines best implementation</span></span>

<span data-ttu-id="ffc3f-174">此方案显示了没有任何实现的基本接口。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-174">This scenario shows a base interface without any implementations.</span></span> <span data-ttu-id="ffc3f-175">将方法添加到 `ILight` 接口带来了新的复杂性。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-175">Adding a method into the `ILight` interface introduces new complexities.</span></span> <span data-ttu-id="ffc3f-176">管理默认接口方法的语言规则最大程度地减少了对实现多个派生接口的具体类的影响。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-176">The language rules governing default interface methods minimize the effect on the concrete classes that implement multiple derived interfaces.</span></span> <span data-ttu-id="ffc3f-177">我们使用新方法增强原始接口的功能，以演示如何更改其用法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-177">Let's enhance the original interface with a new method to show how that changes its use.</span></span> <span data-ttu-id="ffc3f-178">每个指示灯都可以将其电源状态报告为枚举值：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-178">Every indicator light can report its power status as an enumerated value:</span></span>

[!code-csharp[Enumeration for power status](./snippets/mixins-with-default-interface-methods/ILight.cs?name=SnippetPowerStatus)]

<span data-ttu-id="ffc3f-179">默认实现不使用电源：</span><span class="sxs-lookup"><span data-stu-id="ffc3f-179">The default implementation assumes no power:</span></span>

[!code-csharp[Report a default power status](./snippets/mixins-with-default-interface-methods/ILight.cs?name=SnippetILightInterface)]

<span data-ttu-id="ffc3f-180">即使 `ExtraFancyLight` 声明支持 `ILight` 接口以及派生接口 `ITimerLight` 和 `IBlinkingLight`，这些更改仍会进行清晰的编译。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-180">These changes compile cleanly, even though the `ExtraFancyLight` declares support for the `ILight` interface and both derived interfaces, `ITimerLight` and `IBlinkingLight`.</span></span> <span data-ttu-id="ffc3f-181">在 `ILight` 接口中只有一个声明为“最接近”的实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-181">There's only one "closest" implementation declared in the `ILight` interface.</span></span> <span data-ttu-id="ffc3f-182">任何声明了替代的类都将成为一个“最接近”的实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-182">Any class that declared an override would become the one "closest" implementation.</span></span> <span data-ttu-id="ffc3f-183">你在前面的类中看到了替代其他派生接口成员的示例。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-183">You saw examples in the preceding classes that overrode the members of other derived interfaces.</span></span>

<span data-ttu-id="ffc3f-184">避免在多个派生接口中替代相同的方法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-184">Avoid overriding the same method in multiple derived interfaces.</span></span> <span data-ttu-id="ffc3f-185">这样做会在类实现两个派生接口时创建不明确的方法调用。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-185">Doing so creates an ambiguous method call whenever a class implements both derived interfaces.</span></span> <span data-ttu-id="ffc3f-186">编译器不能选取一种更好的方法，因此会发出错误。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-186">The compiler can't pick a single better method so it issues an error.</span></span> <span data-ttu-id="ffc3f-187">例如，如果 `IBlinkingLight` 和 `ITimerLight` 实现了 `PowerStatus` 的替代，则 `OverheadLight` 需要提供更具体的替代。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-187">For example, if both the `IBlinkingLight` and `ITimerLight` implemented an override of `PowerStatus`, the `OverheadLight` would need to provide a more specific override.</span></span> <span data-ttu-id="ffc3f-188">否则，编译器无法在两个派生接口的实现之间进行选择。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-188">Otherwise, the compiler can't pick between the implementations in the two derived interfaces.</span></span> <span data-ttu-id="ffc3f-189">通常，为避免这种情况，可以使接口定义保持较小并专注于一个功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-189">You can usually avoid this situation by keeping interface definitions small and focused on one feature.</span></span> <span data-ttu-id="ffc3f-190">在此方案中，灯的每个功能都是其自己的接口;只有类可以继承多个接口。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-190">In this scenario, each capability of a light is its own interface; multiple interfaces are only inherited by classes.</span></span>

<span data-ttu-id="ffc3f-191">此示例演示了一种方案，在该方案中可以定义可混合到类中的离散功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-191">This sample shows one scenario where you can define discrete features that can be mixed into classes.</span></span> <span data-ttu-id="ffc3f-192">通过声明类支持的接口，可以声明任意一组受支持的功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-192">You declare any set of supported functionality by declaring which interfaces a class supports.</span></span> <span data-ttu-id="ffc3f-193">使用虚拟默认接口方法使类可以使用或定义任何或所有接口方法的不同实现。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-193">The use of virtual default interface methods enables classes to use or define a different implementation for any or all the interface methods.</span></span> <span data-ttu-id="ffc3f-194">这种语言功能提供了对正在构建的真实系统进行建模的新方法。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-194">This language capability provides new ways to model the real-world systems you're building.</span></span> <span data-ttu-id="ffc3f-195">默认接口方法提供了一种更清晰的方式来表达相关的类，这些类可能使用这些功能的虚拟实现来混合和匹配不同的功能。</span><span class="sxs-lookup"><span data-stu-id="ffc3f-195">Default interface methods provide a clearer way to express related classes that may mix and match different features using virtual implementations of those capabilities.</span></span>
