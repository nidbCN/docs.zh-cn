---
title: 使用 .NET CLI 组织和测试项目
description: 本教程介绍如何通过命令行组织和测试 .NET 项目。
author: cartermp
ms.date: 09/10/2018
ms.openlocfilehash: c668cff48a2418cc1bc34aef78ea26c863d9de6a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872673"
---
# <a name="organizing-and-testing-projects-with-the-net-cli"></a>使用 .NET CLI 组织和测试项目

本教程遵循[教程：使用 Visual Studio Code 通过 .NET 创建控制台应用程序](with-visual-studio-code.md)，不仅介绍如何创建简单的控制台应用，还将介绍如何开发高级且结构完善的应用程序。 在演示如何使用文件夹来组织代码后，本教程还将说明如何使用 [xUnit](https://xunit.net/) 测试框架扩展控制台应用程序。

## <a name="using-folders-to-organize-code"></a>使用文件夹组织代码

如要要在控制台应用中引入新类型，可向该应用添加包含该类型的文件。 例如，如果向项目添加包含 `AccountInformation` 和 `MonthlyReportRecords` 类型的文件，则项目文件结构是平面的，且易于导航：

```
/MyProject
|__AccountInformation.cs
|__MonthlyReportRecords.cs
|__MyProject.csproj
|__Program.cs
```

但是，仅在项目规模相对较小时，此方法才适用。 你能否想象在项目中添加 20 个类型时会发生什么？ 项目的根目录中会散落许多文件，这样的项目必然会难以导航和维护。

要组织项目，请创建一个名为 Models 新文件夹，将其用于保存类型文件。 将类型文件放入 Models 文件夹中：

```
/MyProject
|__/Models
   |__AccountInformation.cs
   |__MonthlyReportRecords.cs
|__MyProject.csproj
|__Program.cs
```

按逻辑将文件分组到文件夹的项目易于导航和维护。 在下一节中，将创建一个更复杂的示例，它包含文件夹和单元测试。

## <a name="organizing-and-testing-using-the-newtypes-pets-sample"></a>使用 NewTypes Pets 示例进行组织和测试

### <a name="prerequisites"></a>必备知识

* [.NET 5.0 SDK](https://dotnet.microsoft.com/download) 或更高版本。

### <a name="building-the-sample"></a>生成示例

对于下列步骤，可使用 [NewTypes Pets 示例](https://github.com/dotnet/samples/tree/main/core/console-apps/NewTypesMsBuild)进行相关操作，也可以创建自己的文件与文件夹进行操作。 各类型按逻辑组织为文件夹结构，允许日后加入更多类型，测试也按逻辑放置在文件夹中，允许日后加入更多测试。

此示例包含两种类型 `Dog` 和 `Cat`，并使它们实现一个公共接口 `IPet`。 对于 `NewTypes` 项目，目标是将与宠物相关的类型组织到 Pets 文件夹中。 如果之后添加了另一组类型（例如 WildAnimals），则将其与 Pets 文件夹一同放在 NewTypes 文件夹中。 WildAnimals 文件夹可包含不属于宠物的动物类型，如 `Squirrel` 和 `Rabbit` 类型。 按照这种方式添加类型，不会破坏项目的良好组织。

创建以下文件夹结构，并指明文件内容：

```
/NewTypes
|__/src
   |__/NewTypes
      |__/Pets
         |__Dog.cs
         |__Cat.cs
         |__IPet.cs
      |__Program.cs
      |__NewTypes.csproj
```

IPet.cs:

[!code-csharp[IPet interface](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Pets/IPet.cs)]

Dog.cs:

[!code-csharp[Dog class](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Pets/Dog.cs)]

Cat.cs:

[!code-csharp[Cat class](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Pets/Cat.cs)]

Program.cs:

[!code-csharp[Main](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Program.cs)]

NewTypes.csproj:

[!code-xml[NewTypes csproj](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/NewTypes.csproj)]

请执行以下命令：

```dotnetcli
dotnet run
```

获得以下输出：

```console
Woof!
Meow!
```

可选练习：可通过扩展此项目来添加新的宠物类型，例如 `Bird`。 使鸟的 `TalkToOwner` 方法向所有者发出 `Tweet!`。 再次运行应用。 输出将包含 `Tweet!`

### <a name="testing-the-sample"></a>测试示例

`NewTypes` 项目已准备就绪，与宠物相关的类型均置于一个文件夹中，因此具有良好的组织。 接下来，创建测试项目，并使用 [xUnit](https://xunit.net/) 测试框架开始编写测试。 使用单元测试，可自动检查宠物类型的行为，确认其正常运行。

导航回 src 文件夹并创建“test”文件夹，后者包含 NewTypesTests 文件夹  。 在 NewTypesTests 文件夹的命令提示符中，执行 `dotnet new xunit`。 这将生成两个文件：NewTypesTests.csproj 和 UnitTest1.cs 。

测试项目当前无法测试 `NewTypes` 中的类型，并且需要对 `NewTypes` 项目的项目引用。 要添加项目引用，请使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：

```dotnetcli
dotnet add reference ../../src/NewTypes/NewTypes.csproj
```

或者，可以选择向 NewTypesTests.csproj 文件添加 `<ItemGroup>` 节点，手动添加项目引用：

```xml
<ItemGroup>
  <ProjectReference Include="../../src/NewTypes/NewTypes.csproj" />
</ItemGroup>
```

NewTypesTests.csproj:

[!code-xml[NewTypesTests csproj](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/test/NewTypesTests/NewTypesTests.csproj)]

NewTypesTests.csproj 文件包含下列内容：

* 对 .NET 测试基础结构 `Microsoft.NET.Test.Sdk` 的包引用
* 对 xUnit 测试框架 `xunit` 的包引用
* 对测试运行程序 `xunit.runner.visualstudio` 的包引用
* 对要测试的代码 `NewTypes` 的项目引用

将 UnitTest1.cs 的名称更改为 PetTests.cs，并将文件中的代码替换为下列内容：

```csharp
using System;
using Xunit;
using Pets;

public class PetTests
{
    [Fact]
    public void DogTalkToOwnerReturnsWoof()
    {
        string expected = "Woof!";
        string actual = new Dog().TalkToOwner();

        Assert.NotEqual(expected, actual);
    }

    [Fact]
    public void CatTalkToOwnerReturnsMeow()
    {
        string expected = "Meow!";
        string actual = new Cat().TalkToOwner();

        Assert.NotEqual(expected, actual);
    }
}
```

可选练习：如果先前向所有者添加了生成 `Tweet!` 的 `Bird` 类型，请向 PetTests.cs 文件 `BirdTalkToOwnerReturnsTweet` 添加测试方法，以检查对于 `Bird` 类型，`TalkToOwner` 方法是否正常工作。

> [!NOTE]
> 尽管期望 `expected` 和 `actual` 值相等，但使用 `Assert.NotEqual` 检查的初始断言表明这些值并不相等。 务必最初创建一个失败的测试，以检查测试的逻辑是否正确。 确认测试失败后，调整断言，使测试通过。

下面演示了完整的项目结构：

```
/NewTypes
|__/src
   |__/NewTypes
      |__/Pets
         |__Dog.cs
         |__Cat.cs
         |__IPet.cs
      |__Program.cs
      |__NewTypes.csproj
|__/test
   |__NewTypesTests
      |__PetTests.cs
      |__NewTypesTests.csproj
```

在 test/NewTypesTests 目录中开始。 使用 [`dotnet test`](../tools/dotnet-test.md) 命令运行测试。 此命令启动项目文件中指定的测试运行程序。

测试按预期失败，控制台显示以下输出：

```output
Test run for C:\Source\dotnet\docs\samples\snippets\core\tutorials\testing-with-cli\csharp\test\NewTypesTests\bin\Debug\net5.0\NewTypesTests.dll (.NETCoreApp,Version=v5.0)
Microsoft (R) Test Execution Command Line Tool Version 16.8.1
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
A total of 1 test files matched the specified pattern.
[xUnit.net 00:00:00.50]     PetTests.DogTalkToOwnerReturnsWoof [FAIL]
  Failed PetTests.DogTalkToOwnerReturnsWoof [6 ms]
  Error Message:
   Assert.NotEqual() Failure
Expected: Not "Woof!"
Actual:   "Woof!"
  Stack Trace:
     at PetTests.DogTalkToOwnerReturnsWoof() in C:\Source\dotnet\docs\samples\snippets\core\tutorials\testing-with-cli\csharp\test\NewTypesTests\PetTests.cs:line 13

Failed!  - Failed:     1, Passed:     1, Skipped:     0, Total:     2, Duration: 8 ms - NewTypesTests.dll (net5.0)
```

将测试的断言从 `Assert.NotEqual` 更改为 `Assert.Equal`：

[!code-csharp[PetTests class](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/test/NewTypesTests/PetTests.cs)]

使用 `dotnet test` 命令重新运行测试，并获得以下输出：

```output
Test run for C:\Source\dotnet\docs\samples\snippets\core\tutorials\testing-with-cli\csharp\test\NewTypesTests\bin\Debug\net5.0\NewTypesTests.dll (.NETCoreApp,Version=v5.0)
Microsoft (R) Test Execution Command Line Tool Version 16.8.1
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
A total of 1 test files matched the specified pattern.

Passed!  - Failed:     0, Passed:     2, Skipped:     0, Total:     2, Duration: 2 ms - NewTypesTests.dll (net5.0)
```

测试通过。 在与所有者谈话时，宠物类型的方法返回正确的值。

你已了解使用 xUnit 来组织和测试项目的方法。 继续使用这些方法，将它们应用于自己的项目中。 祝你编码愉快！
