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
# <a name="tutorial-mix-functionality-in-when-creating-classes-using-interfaces-with-default-interface-methods"></a>教程：当通过默认接口方法创建使用接口的类时实现的混入功能

从 .NET Core 3.0 上的 C# 8.0 开始，可以在声明接口成员时定义实现。 此功能提供了一些新功能，可以在其中为接口中声明的功能定义默认实现。 类可以选择何时替代功能、何时使用默认功能以及何时不声明对离散功能的支持。

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 使用描述离散功能的实现创建接口。
> * 创建使用默认实现的类。
> * 创建用于替代部分或全部默认实现的类。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。 自 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本起，开始随附 C# 8.0 编译器。

## <a name="limitations-of-extension-methods"></a>扩展方法的限制

实现作为接口一部分出现的行为的一种方法是：定义可提供默认行为的[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)。 接口声明最少的一组成员，同时为实现该接口的任何类提供更大的外围应用。 例如，<xref:System.Linq.Enumerable> 中的扩展方法提供作为 LINQ 查询源的任何序列的实现。

扩展方法是使用变量的声明类型在编译时解析的。 实现接口的类可以为任何扩展方法提供更好的实现。 变量声明必须与实现类型匹配，以使编译器能够选择该实现。 当编译时类型与接口匹配时，方法调用解析为扩展方法。 扩展方法的另一个问题是，只要可访问包含扩展方法的类，就可以访问这些方法。 类不能声明是否应提供扩展方法中声明的功能。

从 C# 8.0 开始，可以将默认实现声明为接口方法。 然后，每个类将自动使用默认实现。 可提供更好实现的任何类都可以使用更好的算法来替代接口方法定义。 从某种意义上讲，这种技术听起来与你使用[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)的方式类似。

在本文中，你将了解默认接口实现如何启用新方案。

## <a name="design-the-application"></a>设计应用程序

考虑使用一个家庭自动化应用程序。 你可能在整个房子中使用许多不同类型的灯和指示灯。 每个灯都必须支持 API 以使其打开和关闭，以及报告当前状态。 一些灯和指示灯可能支持其他功能，例如：

- 打开灯，然后定时关闭。
- 使灯闪烁一段时间。

在支持最小集的设备中，可以模拟其中的某些扩展功能。 这表示提供了默认实现。 对于内置了更多功能的设备，设备软件将使用本机功能。 对于其他灯，它们可以选择实现接口并使用默认实现。

对于此方案，默认接口成员是比扩展方法更好的解决方案。 类创建者可以控制它们选择实现的接口。 它们选择的接口可用作方法。 此外，由于默认情况下默认的接口方法是虚拟的，因此该方法调度始终选择类中的实现。

我们创建代码来演示这些差异。

## <a name="create-interfaces"></a>创建接口

首先创建用于定义所有灯的行为的接口：

[!code-csharp[Declare base interface](./snippets/mixins-with-default-interface-methods/UnusedExampleCode.cs?name=SnippetILightInterfaceV1)]

基本的高架灯具可能会实现此接口，如下面的代码所示：

[!code-csharp[First overhead light](./snippets/mixins-with-default-interface-methods/UnusedExampleCode.cs?name=SnippetOverheadLightV1)]

在本教程中，代码不驱动 IoT 设备，但会通过将消息写入控制台的方式来模拟这些活动。 可以浏览代码，但不执行房屋的自动化。

接下来，我们定义一个可在超时后自动关闭的灯的接口：

[!code-csharp[pure Timer interface](./snippets/mixins-with-default-interface-methods/UnusedExampleCode.cs?name=SnippetPureTimerInterface)]

可以向高架灯具添加基本实现，但是更好的解决方案是修改此接口定义以提供 `virtual` 默认实现：

[!code-csharp[Timer interface](./snippets/mixins-with-default-interface-methods/ITimerLight.cs?name=SnippetTimerLightFinal)]

添加该更改后，`OverheadLight` 类可以通过声明对接口的支持来实现计时器功能：

```csharp
public class OverheadLight : ITimerLight { }
```

另一种灯类型可能支持更复杂的协议。 它可以为 `TurnOnFor` 提供自己的实现，如以下代码所示：

[!code-csharp[Override the timer function](./snippets/mixins-with-default-interface-methods/HalogenLight.cs?name=SnippetHalogenLight)]

与替代虚拟类方法不同，`HalogenLight` 类中 `TurnOnFor` 的声明不使用 `override` 关键字。

## <a name="mix-and-match-capabilities"></a>混合和匹配功能

当你引入更高级的功能时，默认界面方法的优势变得更加明显。 使用接口让你可以混合和匹配功能。 它还使每个类创建者可以在默认实现和自定义实现之间进行选择。 我们使用闪烁灯的默认实现来添加一个接口：

[!code-csharp[Define the blinking light interface](./snippets/mixins-with-default-interface-methods/IBlinkingLight.cs?name=SnippetBlinkingLight)]

默认实现使任何灯可以闪烁。 高架灯可以使用默认实现添加计时器和闪烁功能：

[!code-csharp[Use the default blink function](./snippets/mixins-with-default-interface-methods/OverheadLight.cs?name=SnippetOverheadLight)]

新的灯类型 `LEDLight` 直接支持计时器功能和闪烁功能。 这种灯样式同时实现 `ITimerLight` 和 `IBlinkingLight` 接口，并替代 `Blink` 方法：

[!code-csharp[Override the blink function](./snippets/mixins-with-default-interface-methods/LEDLight.cs?name=SnippetLEDLight)]

`ExtraFancyLight` 可以直接支持闪烁功能和计时器功能：

[!code-csharp[Override the blink and timer function](./snippets/mixins-with-default-interface-methods/ExtraFancyLight.cs?name=SnippetExtraFancyLight)]

之前创建的 `HalogenLight` 不支持闪烁。 因此，不要将 `IBlinkingLight` 添加到其支持的接口列表。

## <a name="detect-the-light-types-using-pattern-matching"></a>使用模式匹配检测灯类型

接下来，我们编写一些测试代码。 可以使用 C# 的[模式匹配](../../pattern-matching.md)功能，通过检查灯支持的接口来确定灯的功能。  下面的方法将实践每个灯的支持功能：

[!code-csharp[Test a light's capabilities](./snippets/mixins-with-default-interface-methods/Program.cs?name=SnippetTestLightFunctions)]

`Main` 方法中的以下代码按顺序创建每种灯类型，并测试相应的灯：

[!code-csharp[Test a light's capabilities](./snippets/mixins-with-default-interface-methods/Program.cs?name=SnippetMainMethod)]

## <a name="how-the-compiler-determines-best-implementation"></a>编译器如何确定最佳实现

此方案显示了没有任何实现的基本接口。 将方法添加到 `ILight` 接口带来了新的复杂性。 管理默认接口方法的语言规则最大程度地减少了对实现多个派生接口的具体类的影响。 我们使用新方法增强原始接口的功能，以演示如何更改其用法。 每个指示灯都可以将其电源状态报告为枚举值：

[!code-csharp[Enumeration for power status](./snippets/mixins-with-default-interface-methods/ILight.cs?name=SnippetPowerStatus)]

默认实现不使用电源：

[!code-csharp[Report a default power status](./snippets/mixins-with-default-interface-methods/ILight.cs?name=SnippetILightInterface)]

即使 `ExtraFancyLight` 声明支持 `ILight` 接口以及派生接口 `ITimerLight` 和 `IBlinkingLight`，这些更改仍会进行清晰的编译。 在 `ILight` 接口中只有一个声明为“最接近”的实现。 任何声明了替代的类都将成为一个“最接近”的实现。 你在前面的类中看到了替代其他派生接口成员的示例。

避免在多个派生接口中替代相同的方法。 这样做会在类实现两个派生接口时创建不明确的方法调用。 编译器不能选取一种更好的方法，因此会发出错误。 例如，如果 `IBlinkingLight` 和 `ITimerLight` 实现了 `PowerStatus` 的替代，则 `OverheadLight` 需要提供更具体的替代。 否则，编译器无法在两个派生接口的实现之间进行选择。 通常，为避免这种情况，可以使接口定义保持较小并专注于一个功能。 在此方案中，灯的每个功能都是其自己的接口;只有类可以继承多个接口。

此示例演示了一种方案，在该方案中可以定义可混合到类中的离散功能。 通过声明类支持的接口，可以声明任意一组受支持的功能。 使用虚拟默认接口方法使类可以使用或定义任何或所有接口方法的不同实现。 这种语言功能提供了对正在构建的真实系统进行建模的新方法。 默认接口方法提供了一种更清晰的方式来表达相关的类，这些类可能使用这些功能的虚拟实现来混合和匹配不同的功能。
