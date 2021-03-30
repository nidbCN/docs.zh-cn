---
title: 在 C# 中使用默认接口方法安全地更新接口
description: 本高级教程探讨了如何安全地向现有接口定义添加新功能，而不破坏实现该接口的所有类和结构。
ms.date: 05/06/2019
ms.technlogy: csharp-advanced-concepts
ms.custom: mvc
ms.openlocfilehash: 43af25cee765ba18543b0c7bfe0069542a90e0e5
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878875"
---
# <a name="tutorial-update-interfaces-with-default-interface-methods-in-c-80"></a>教程：在 C# 8.0 中使用默认接口方法更新接口

从 .NET Core 3.0 上的 C# 8.0 开始，可以在声明接口成员时定义实现。 最常见的方案是安全地将成员添加到已经由无数客户端发布并使用的接口。

在本教程中，你将了解：

> [!div class="checklist"]
>
> * 通过使用实现添加方法，安全地扩展接口。
> * 创建参数化实现以提供更大的灵活性。
> * 使实现器能够以替代的形式提供更具体的实现。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET Core，包括 C# 8.0 编译器。 自 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 起，开始随附 C# 8.0 编译器。

## <a name="scenario-overview"></a>方案概述

本教程从客户关系库版本 1 开始。 可以在 [GitHub 上的示例存储库](https://github.com/dotnet/samples/tree/main/csharp/tutorials/default-interface-members-versions/starter/customer-relationship)中获取入门应用程序。 生成此库的公司希望拥有现有应用程序的客户采用其库。 他们为使用其库的用户提供最小接口定义供其实现。 以下是客户的接口定义：

[!code-csharp[InitialCustomerInterface](~/samples/snippets/csharp/tutorials/default-interface-members-versions/starter/customer-relationship/ICustomer.cs?name=SnippetICustomerVersion1)]

他们定义了表示订单的第二个接口：

[!code-csharp[InitialOrderInterface](~/samples/snippets/csharp/tutorials/default-interface-members-versions/starter/customer-relationship/IOrder.cs?name=SnippetIorderVersion1)]

通过这些接口，团队可以为其用户生成一个库，以便为其客户创造更好的体验。 他们的目标是与现有客户建立更深入的关系，并改善他们与新客户的关系。

现在，是时候为下一版本升级库了。 其中一个请求的功能可以为拥有大量订单的客户提供忠实客户折扣。 无论客户何时下单，都会应用这一新的忠实客户折扣。 该特定折扣是每位客户的财产。 `ICustomer` 的每个实现都可以为忠实客户折扣设置不同的规则。

添加此功能的最自然方式是使用用于应用任何忠实客户折扣的方法来增强 `ICustomer` 接口。 此设计建议引起了经验丰富的开发人员的关注：“一旦发布，接口就是固定不变的！ 这是一项突破性的变革！” C# 8.0 添加了 *默认接口实现* 用于升级接口。 库作者可以向接口添加新成员，并为这些成员提供默认实现。

默认接口实现使开发人员能够升级接口，同时仍允许任何实现器替代该实现。 库的用户可以接受默认实现作为非中断性变更。 如果他们的业务规则不同，则可以进行替代。

## <a name="upgrade-with-default-interface-methods"></a>使用默认接口方法升级

团队就最有可能的默认实现达成一致：针对客户的忠实客户折扣。

升级应提供用于设置两个属性的功能：符合折扣条件所需的订单数量以及折扣百分比。 这使其成为用于默认接口成员的完美方案。 可以向 `ICustomer` 接口添加方法，并提供最有可能的实现。 所有现有的和任何新的实现都可以使用默认实现，或者提供其自己的实现。

首先，将新方法添加到接口，包括方法的主体：

[!code-csharp[InitialOrderInterface](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetLoyaltyDiscountVersionOne)]

库作者编写了用于检查实现的第一个测试：

[!code-csharp[TestDefaultImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetTestDefaultImplementation)]

注意测试的以下部分：

[!code-csharp[TestDefaultImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetHighlightCast)]

从 `SampleCustomer` 到 `ICustomer` 的强制转换是必需的。 `SampleCustomer` 类不需要为 `ComputeLoyaltyDiscount` 提供实现；这由 `ICustomer` 接口提供。 但是，`SampleCustomer` 类不会从其接口继承成员。 该规则没有更改。 若要调用在接口中声明和实现的任何方法，该变量的类型必须是接口的类型，在本示例中为 `ICustomer`。

## <a name="provide-parameterization"></a>提供参数化

这是一个好的开始。 但是，默认实现存在太多限制。 此系统的许多使用者可能会选择不同的购买数量阈值、不同的会员资格时长或不同的折扣百分比。 通过提供用于设置这些参数的方法，可为更多客户提供更好的升级体验。 让我们添加一个静态方法，该方法可设置控制默认实现的三个参数：

[!code-csharp[VersionTwoImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetLoyaltyDiscountVersionTwo)]

这个小代码片段中展示了许多新的语言功能。 接口现在可以包含静态成员，其中包括字段和方法。 还启用了不同的访问修饰符。 其他字段是专用的，新方法是公共的。 接口成员允许使用任何修饰符。

使用常规公式计算忠实客户折扣但参数有所不同的应用程序不需要提供自定义实现；它们可以通过静态方法设置自变量。 例如，以下代码设置“客户答谢”，奖励任何成为会员超过一个月的客户：

[!code-csharp[SetLoyaltyThresholds](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetSetLoyaltyThresholds)]

## <a name="extend-the-default-implementation"></a>扩展默认实现

目前添加的代码提供了方便的实现，可用于用户需要类似默认实现的项目的方案，或用于提供一组不相关的规则。 对于最后一个功能，让我们稍微重构一下代码，以实现用户可能需要基于默认实现进行生成的方案。

假设有一家想要吸引新客户的初创企业。 他们为新客户的第一笔订单提供 50% 的折扣， 而现有客户则会获得标准折扣。 库作者需要将默认实现移入 `protected static` 方法，以便实现此接口的任何类都可以在其实现中重用代码。 接口成员的默认实现也调用此共享方法：

[!code-csharp[VersionTwoImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetFinalVersion)]

在实现此接口的类的实现中，替代可以调用静态帮助程序方法，并扩展该逻辑以提供“新客户”折扣：

[!code-csharp[VersionTwoImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/SampleCustomer.cs?name=SnippetOverrideAndExtend)]

可以在 [GitHub 上的示例存储库](https://github.com/dotnet/samples/tree/main/csharp/tutorials/default-interface-members-versions/finished/customer-relationship)中查看整个完成的代码。 可以在 [GitHub 上的示例存储库](https://github.com/dotnet/samples/tree/main/csharp/tutorials/default-interface-members-versions/starter/customer-relationship)中获取入门应用程序。

这些新功能意味着，当这些新成员拥有合理的默认实现时，接口可以安全地更新。 精心设计接口，以表达可由多个类实现的单个功能概念。 这样一来，在发现针对同一功能概念的新要求时，可以更轻松地升级这些接口定义。
