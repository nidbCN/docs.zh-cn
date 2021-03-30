---
title: 使用 dotnet test 和 xUnit 在 .NET Core 中对 F# 进行单元测试
description: 通过使用 dotnet test 和 xUnit 分步生成示例解决方案的交互体验，了解 .NET Core 中的 F# 单元测试概念。
author: billwagner
ms.author: wiwagn
ms.date: 08/30/2017
ms.openlocfilehash: 76386097ebe9b0ec6141abd089b219275d989446
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875026"
---
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-xunit"></a>使用 dotnet test 和 xUnit 在 .NET Core 中进行 F# 库单元测试

本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。 如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-with-fsharp/)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="creating-the-source-project"></a>创建源项目

打开 shell 窗口。 创建一个名为 unit-testing-with-fsharp 的目录，以保留该解决方案。
在此新目录中，运行 `dotnet new sln` 创建新的解决方案。 这样便于管理类库和单元测试项目。
在解决方案库中，创建 MathService 目录。 目录和文件结构目前如下所示：

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

将 MathService 作为当前目录，然后运行 `dotnet new classlib -lang "F#"` 以创建源项目。 创建数学服务的失败实现：

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

将目录更改回 unit-testing-with-fsharp 目录。 运行 `dotnet sln add .\MathService\MathService.fsproj` 向解决方案添加类库项目。

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

将 MathService.Tests 目录作为当前目录，并使用 `dotnet new xunit -lang "F#"` 创建一个新项目。 这会创建将 xUnit 用作测试库的测试项目。 生成的模板在 MathServiceTests.fsproj 中配置测试运行程序：

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0-preview-20170628-02" />
  <PackageReference Include="xunit" Version="2.2.0" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0" />
</ItemGroup>
```

测试项目需要其他包创建和运行单元测试。 `dotnet new` 在以前的步骤中已添加 xUnit 和 xUnit 运行程序。 现在，将 `MathService` 类库作为另一个依赖项添加到项目中。 使用 `dotnet add reference` 命令：

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
        MathServiceTests.fsproj
```

在 unit-testing-with-fsharp 目录中执行 `dotnet sln add .\MathService.Tests\MathService.Tests.fsproj`。

## <a name="creating-the-first-test"></a>创建第一个测试

编写一个失败测试，使其通过，然后重复此过程。 打开 Tests.fs 并添加以下代码：

```fsharp
[<Fact>]
let ``My test`` () =
    Assert.True(true)

[<Fact>]
let ``Fail every time`` () = Assert.True(false)
```

`[<Fact>]` 属性表示由测试运行程序运行的测试方法。 在 unit-testing-with-fsharp中，执行 `dotnet test` 以构建测试和类库，然后运行测试。 xUnit 测试运行程序包含要运行测试的程序入口点。 `dotnet test` 使用已创建的单元测试项目启动测试运行程序。

这两个测试演示了最基本的已通过测试和未通过测试。 `My test` 通过，而 `Fail every time` 未通过。 现在创建针对 `squaresOfOdds` 方法的测试。 `squaresOfOdds` 方法返回输入序列中所有奇整数值的平方序列。 可以以迭代的方式创建可验证此功能的测试，而非尝试同时写入所有的函数。 若要让每个测试都通过，意味着要针对此方法创建必要的功能。

可以编写的最简单的测试是调用包含所有偶数的 `squaresOfOdds`，它的结果应该是一个空整数序列。  此测试如下所示：

```fsharp
[<Fact>]
let ``Sequence of Evens returns empty collection`` () =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

测试失败。 尚未创建实现。 在起作用的 `MathService` 类中编写最简单的代码，使此测试通过：

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

在 unit-testing-with-fsharp 目录中，再次运行 `dotnet test`。 `dotnet test` 命令构建 `MathService` 项目，然后构建 `MathService.Tests` 项目。 构建这两个项目后，该命令将运行此单项测试。 测试通过。

## <a name="completing-the-requirements"></a>完成要求

你已经通过了一个测试，现在可以编写更多测试。 下一个简单示例使用的序列包含的唯一奇数为 `1`。 数值 1 较为简单，因为 1 的平方是 1。 下一个测试如下所示：

```fsharp
[<Fact>]
let ``Sequences of Ones and Evens returns Ones`` () =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

执行 `dotnet test` 以运行测试，并显示新测试失败。 现在更新 `squaresOfOdds` 方法，以处理此新测试。 筛选出序列中的所有偶数值，以使此测试通过。 可以编写一个小筛选器函数并使用 `Seq.filter` 来实现此目的：

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

还要执行一个步骤：计算每个奇数的平方值。 从编写新测试开始：

```fsharp
[<Fact>]
let ``SquaresOfOdds works`` () =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.Equal(expected, actual)
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

- [dotnet new](../tools/dotnet-new.md)
- [dotnet sln](../tools/dotnet-new.md)
- [dotnet add reference](../tools/dotnet-add-reference.md)
- [dotnet test](../tools/dotnet-test.md)
