---
title: 在对象中使用模式 - C# 教程
description: 本教程介绍了在类成员中使用模式匹配创建更好的对象行为模型的技巧
ms.date: 11/05/2020
ms.openlocfilehash: 072f6f57696504c2d691473e3a43c1cda53f227f
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104878895"
---
# <a name="use-pattern-matching-to-build-your-class-behavior-for-better-code"></a>使用模式匹配生成类行为以获得更好的代码

C# 中的模式匹配功能可提供语法来表示你的算法。 可以使用这些技巧来实现类中的行为。 在为真正的对象建模时，可以结合使用面向对象的类设计与面向数据的实现，来提供简洁的代码。

本教程介绍以下操作：

> [!div class="checklist"]
>
> - 使用数据模式表示面向对象的类。
> - 使用 C# 的模式匹配功能实现这些模式。
> - 利用编译器诊断来验证实现。

## <a name="prerequisites"></a>先决条件

需要将计算机设置为运行 .NET 5，包括 C# 9.0 编译器。 自 [Visual Studio 2019 版本 16.8 预览版](https://visualstudio.microsoft.com/vs/preview/)或 [.NET 5.0 SDK 预览版](https://dotnet.microsoft.com/download/dotnet/5.0) 起，开始随附 C# 9.0 编译器。

## <a name="build-a-simulation-of-a-canal-lock"></a>生成对运河闸的模拟

本教程将生成一个 C# 类，用于模拟[运河闸](https://en.wikipedia.org/wiki/Lock_(water_navigation))。 简而言之，运河闸是指船只在两片不同水位的水域间穿行时用于升降它们的设施。 一个闸有两个闸门和某种用来更改水位的机制。

在闸正常运转的情况下，闸内的水位与船只驶入侧的水位一致时，船只会进入其中的一个闸门。 进入闸内后，水位将发生变化，以与船只驶离闸时所处的水位一致。 水位与驶离侧水位一致后，该侧的闸门将打开。 通过采用安全措施，可以确保操作人员不会在运河中引发危险情况。 只有两个闸门都关闭时，水位才能发生变化。 最多只能打开一个闸门。 闸内的水位必须与即将打开的闸门外部的水位一致，才能打开该闸门。

可以生成 C# 类来为此行为建模。 `CanalLock` 类将支持用于打开或关闭任意一个闸门的命令。 它还会包含其他用于升高或降低水位的命令。 此类还应支持用于读取两个闸门和水位当前状态的属性。 将通过方法实现安全措施。

## <a name="define-a-class"></a>定义类

将生成一个控制台应用程序，用于测试 `CanalLock` 类。 使用 Visual Studio 或 .NET CLI 创建新的 .NET 5 控制台项目。 然后，添加一个新类并将其命名为 `CanalLock`。 接下来，设计公共 API，但不要实现方法：

:::code language="csharp" source="snippets/pattern-objects/InterimSteps.cs" ID="APIDesign":::

前面的代码会初始化对象，以便两个闸门都处于关闭状态，并且水位较低。 接下来，在 `Main` 方法中编写以下测试代码，以在你生成此类的第一个实现时引导你完成操作：

:::code language="csharp" source="snippets/pattern-objects/Program.cs" ID="HappyTests":::

接下来，添加 `CanalLock` 类中每个方法的第一个实现。 以下代码将实现类的方法，你无需担心安全规则。 稍后将添加安全测试：

:::code language="csharp" source="snippets/pattern-objects/InterimSteps.cs" ID="FirstImplementation":::

目前已编写的测试通过。 你已实现基本内容。 现在，针对第一种故障条件编写测试。 前面的测试结束时，两个闸门都已关闭，并且水位设置为较低。 添加一个测试，用于尝试打开上闸门：

:::code language="csharp" source="snippets/pattern-objects/Program.cs" ID="HighGateSafetyTest":::

此测试失败，因为该闸门打开了。 作为第一个实现，可以使用以下代码修复此问题：

:::code language="csharp" source="snippets/pattern-objects/InterimSteps.cs" ID="SecondImplementation":::

测试通过。 但是，添加的测试越多，要添加的 `if` 子句也越来越多，并要测试不同的属性。 这些方法很快就会变得过于复杂，因为添加了更多的条件语句。

## <a name="implement-the-commands-with-patterns"></a>使用模式实现命令

更好的方法是，使用模式确定对象是否处于可执行命令的有效状态。 可以表示是否允许将命令作为以下三个变量的函数：闸门状态、水位和新设置：

| 新设置 | 闸门状态 | 水位 | 结果             |
| ----------- | ---------- | ----------- | ------------------ |
| 关闭      | 关闭     | 高        | 关闭             |
| 关闭      | 关闭     | 低         | 关闭             |
| 关闭      | 打开       | 高        | 打开               |
| ~~已解决~~  | ~~打开~~   | ~~低~~     | ~~已解决~~         |
| 打开        | 已解决     | 高        | 打开               |
| 打开        | 已解决     | 低         | 关闭（错误）     |
| 打开        | 打开       | 高        | 打开               |
| ~~打开~~    | ~~打开~~   | ~~低~~     | ~~关闭（错误）~~ |

表中的第四行和最后一行包含带删除线的文本，因为这些文本无效。 现在，要添加的代码应确保上闸门永远不会在水位低时打开。  可以将这些状态编码为单个开关表达式（请牢记 `false` 表示“关闭”）：

:::code language="csharp" source="snippets/pattern-objects/InterimSteps.cs" ID="ThirdImplementation":::

试用此版本。 验证代码时测试通过。 完整的表格显示了输入与结果的可能组合方式。 这意味着，你和其他开发人员可以快速查看此表，确保已包含所有可能的输入。 也可以使用编译器来帮助实现这一目的，这样更为简单。 添加前面的代码后，可以看到编译器生成以下警告：CS8524 表示开关表达式未包含所有可能的输入。 出现该警告的原因是，某个输入为 `enum` 类型。 编译器会将“所有可能的输入”解释为基础类型的所有输入，此类型通常为 `int`。 此 `switch` 表达式仅检查 `enum` 中声明的值。 若要删除此警告，可以为表达式的最后一个分支添加全部捕获弃用模式。 此条件会引发异常，因为它表示输入无效：

```csharp
_  => throw new InvalidOperationException("Invalid internal state"),
```

前面的开关分支必须是 `switch` 表达式中的最后一个，因为它与所有输入匹配。 通过将它移到序列的较前方进行试验。 这将引发编译器错误 CS8510，因为模式中的代码无法访问。  开关表达式本身的结构允许编译器针对可能的错误生成错误和警告。 借助编译器“安全网格”，可以通过较少的迭代更轻松地生成正确的代码，并可以自由地将开关分支与通配符组合在一起。 如果组合生成了意外的不可访问的分支，编译器将发出错误，如果删除了所需的分支，它将发出警告。

第一个更改用于组合命令为关闭闸门的所有分支；这是始终允许的。 将以下代码添加为开关表达式中的第一个分支：

```csharp
(false, _, _) => false,
```

添加前面的开关分支后，将收到四个编译器错误，每个命令为 `false` 的分支都有一个错误。 新添加的分支已包含这些分支。 可以放心地删除这四行。 你计划使用这个新的开关分支替换这些条件。

接下来，可以简化命令为打开闸门的四个分支。 在水位较高的两种情况下，闸门可以打开。 （在其中一种情况下，闸门已打开。）一种水位较低的情况将引发异常，另一种情况不应发生。 如果水闸已处于无效状态，引发同样的异常应是安全的。 可以对这些分支进行以下简化：

```csharp
(true, _, WaterLevel.High) => true,
(true, false, WaterLevel.Low) => throw new InvalidOperationException("Cannot open high gate when the water is low"),
_ => throw new InvalidOperationException("Invalid internal state"),
```

重新运行测试，测试通过。 以下是 `SetHighGate` 方法的最终版本：

:::code language="csharp" source="snippets/pattern-objects/CanalLock.cs" ID="FinalImplementaton":::

## <a name="implement-patterns-yourself"></a>自行实现模式

现在你已了解了此技巧，接下来请自行填写 `SetLowGate` 和 `SetWaterLevel` 方法。  首先，添加以下代码来测试这些方法上的无效操作：

:::code language="csharp" source="snippets/pattern-objects/Program.cs" ID="FinalTestCode":::

重新运行应用程序。 可以看到新的测试失败，运河闸进入无效状态。 尝试自行实现剩余的方法。 用于设置下闸门的方法应类似于用于设置上闸门的方法。 用于更改水位的方法包含不同的检查，但应采用相似的结构。 将同一过程用于设置水位的方法，你会发现这样很有用。 从所有的四个输入开始：两个闸门的状态、水位的当前状态和请求的新水位。 开关表达式应以下列形式开头：

```csharp
CanalLockWaterLevel = (newLevel, CanalLockWaterLevel, LowWaterGateOpen, HighWaterGateOpen) switch
{
    // elided
};
```

要填写 16 个总开关分支。 然后进行测试和简化。

你生成的方法与此类似吗？

:::code language="csharp" source="snippets/pattern-objects/CanalLock.cs" ID="FinalExercise":::

你的测试应该可以通过，并且运河闸应安全运转。

## <a name="summary"></a>总结

在本教程中，你学习了：在对对象的内部状态应用任何更改前，如何使用模式匹配检查该状态。 你可以检查属性组合。 针对其中任意一种转换生成表格后，测试代码，然后进行简化实现可读性和可维护性。 这些初步的重构可能会表明需要进一步重构，以验证内部状态或管理其他 API 更改。 本教程结合使用了类和对象与更加面向数据的基于模式的方法，来实现这些类。
