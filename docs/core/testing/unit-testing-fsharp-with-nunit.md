---
title: 使用 dotnet test 和 NUnit 对 .NET Core 中的 F# 进行单元测试
description: 使用 dotnet test 和 NUnit 分步构建一个示例解决方案，在此交互式体验中学习 .NET Core 中的 F# 单元测试概念。
author: rprouse
ms.date: 10/04/2018
dev_langs:
- fsharp
ms.openlocfilehash: 54513d3063ea0a3be8da73bcbd55962e839c6e2c
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875052"
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-nunit"></a>使用 dotnet test 和 NUnit 在 .NET Core 中进行 F# 库的单元测试

本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。 如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-with-fsharp-nunit/)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>先决条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本。
- 按需选择的文本编辑器或代码编辑器。

## <a name="creating-the-source-project"></a>创建源项目

打开 shell 窗口。 创建一个名为 unit-testing-with-fsharp 的目录，以保留该解决方案。
在此新目录中，运行以下命令，为类库和测试项目创建新的解决方案文件：

```dotnetcli
dotnet new sln
```

接下来，创建 MathService 目录。 下图显示了当前的目录和文件结构：

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

将 MathService 作为当前目录，并运行以下命令以创建源项目：

```dotnetcli
dotnet new classlib -lang "F#"
```

创建数学服务的失败实现：

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

将目录更改回 unit-testing-with-fsharp 目录。 运行以下命令，向解决方案添加类库项目：

```dotnetcli
dotnet sln add .\MathService\MathService.fsproj
```

## <a name="creating-the-test-project"></a>创建测试项目

接下来，创建 MathService.Tests 目录。 下图显示了它的目录结构：

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
```

将 MathService.Tests 目录作为当前目录，并使用以下命令创建一个新项目：

```dotnetcli
dotnet new nunit -lang "F#"
```

这会创建一个将 NUnit 用作测试框架的测试项目。 生成的模板在 MathServiceTests.fsproj 中配置测试运行程序：

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="NUnit" Version="3.9.0" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.9.0" />
</ItemGroup>
```

测试项目需要其他包创建和运行单元测试。 在上一步中，`dotnet new` 已添加 NUnit 和 NUnit 测试适配器。 现在，将 `MathService` 类库作为另一个依赖项添加到项目中。 使用 `dotnet add reference` 命令：

```dotnetcli
dotnet add reference ../MathService/MathService.fsproj
```

可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj)中看到整个文件。

最终的解决方案布局将如下所示：

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
        Test Source Files
        MathService.Tests.fsproj
```

在 unit-testing-with-fsharp 目录中执行以下命令：

```dotnetcli
dotnet sln add .\MathService.Tests\MathService.Tests.fsproj
```

## <a name="creating-the-first-test"></a>创建第一个测试

编写一个失败测试，使其通过，然后重复此过程。 打开 UnitTest1.fs 并添加以下代码：

```fsharp
namespace MathService.Tests

open System
open NUnit.Framework
open MathService

[<TestFixture>]
type TestClass () =

    [<Test>]
    member this.TestMethodPassing() =
        Assert.True(true)

    [<Test>]
     member this.FailEveryTime() = Assert.True(false)
```

`[<TestFixture>]` 属性表示包含测试的类。 `[<Test>]` 属性表示由测试运行程序运行的测试方法。 在 unit-testing-with-fsharp 目录中，执行 `dotnet test` 以构建测试和类库，然后运行测试。 NUnit 测试运行程序包含要运行测试的程序入口点。 `dotnet test` 使用已创建的单元测试项目启动测试运行程序。

这两个测试演示了最基本的已通过测试和未通过测试。 `My test` 通过，而 `Fail every time` 未通过。 现在创建针对 `squaresOfOdds` 方法的测试。 `squaresOfOdds` 方法返回输入序列中所有奇整数值的平方序列。 可以以迭代的方式创建可验证此功能的测试，而非尝试同时写入所有的函数。 若要让每个测试都通过，意味着要针对此方法创建必要的功能。

可以编写的最简单的测试是调用包含所有偶数的 `squaresOfOdds`，它的结果应该是一个空整数序列。  此测试如下所示：

```fsharp
[<Test>]
member this.TestEvenSequence() =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

请注意已将 `expected` 序列转换为列表。 NUnit 框架依赖于许多标准 .NET 类型。 此依赖关系表示公共接口和预期结果支持 <xref:System.Collections.ICollection>，而非 <xref:System.Collections.IEnumerable>。

运行此测试时，会看到测试失败。 尚未创建实现。 在起作用的 MathService 项目的 Library.fs 类中编写最简单的代码，使此测试通过：

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

在 unit-testing-with-fsharp 目录中，再次运行 `dotnet test`。 `dotnet test` 命令构建 `MathService` 项目，然后构建 `MathService.Tests` 项目。 构建这两个项目后，该命令将运行测试。 现在两个测试通过。

## <a name="completing-the-requirements"></a>完成要求

你已经通过了一个测试，现在可以编写更多测试。 下一个简单示例使用的序列包含的唯一奇数为 `1`。 数值 1 较为简单，因为 1 的平方是 1。 下一个测试如下所示：

```fsharp
[<Test>]
member public this.TestOnesAndEvens() =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

如果执行 `dotnet test`，新测试将失败。 必须更新 `squaresOfOdds` 方法才能处理此新测试。 必须筛选出序列中的所有偶数值，以使此测试通过。 可以编写一个小筛选器函数并使用 `Seq.filter` 来实现此目的：

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

注意对 `Seq.toList` 的调用。 它会创建一个列表，此列表将实现 <xref:System.Collections.ICollection> 接口。

还要执行一个步骤：计算每个奇数的平方值。 从编写新测试开始：

```fsharp
[<Test>]
member public this.TestSquaresOfOdds() =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

可以通过映射操作传递经过筛选的序列来计算每个奇数的平方，以此方式来修复测试的缺陷：

```fsharp
let private square x = x * x
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
    |> Seq.map square
```

你已生成一个小型库和该库的一组单元测试。 你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。 你已将多数的时间和精力集中在解决应用程序的目标上。

## <a name="see-also"></a>另请参阅

- [dotnet add reference](../tools/dotnet-add-reference.md)
- [dotnet test](../tools/dotnet-test.md)
