---
title: 使用 dotnet test 和 xUnit 对 .NET Core 中的 Visual Basic 进行单元测试
description: 通过使用 dotnet test 和 xUnit 分步生成 Visual Basic 示例解决方案的交互体验，了解 .NET Core 中的单元测试概念。
author: billwagner
ms.author: wiwagn
ms.date: 05/18/2020
ms.openlocfilehash: 67a8ea0520a0e38ff1f62cbecf82fe0b40969414
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875000"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-xunit"></a>使用 dotnet test 和 xUnit 进行 Visual Basic .NET Core 库单元测试

本教程演示如何生成包含单元测试项目和库项目的解决方案。 若要使用预构建解决方案学习本教程，请[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-using-dotnet-test/)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

## <a name="create-the-solution"></a>创建解决方案

在本部分中，将创建包含源和测试项目的解决方案。 已完成的解决方案具有以下目录结构：

```
/unit-testing-using-dotnet-test
    unit-testing-using-dotnet-test.sln
    /PrimeService
        PrimeService.vb
        PrimeService.vbproj
    /PrimeService.Tests
        PrimeService_IsPrimeShould.vb
        PrimeServiceTests.vbproj
```

以下说明提供了创建测试解决方案的步骤。 有关通过一个步骤创建测试解决方案的说明，请参阅[用于创建测试解决方案的命令](#create-test-cmd)。

* 打开 shell 窗口。
* 运行下面的命令：

  ```dotnetcli
  dotnet new sln -o unit-testing-using-dotnet-test
  ```

  [`dotnet new sln`](../tools/dotnet-new.md) 命令用于在 unit-testing-using-dotnet-test 目录中创建新的解决方案。
* 将目录更改为 unit-testing-using-dotnet-test 文件夹。
* 运行下面的命令：

  ```dotnetcli
  dotnet new classlib -o PrimeService --lang VB
  ```

   [`dotnet new classlib`](../tools/dotnet-new.md) 命令用于在 PrimeService 文件夹中创建新的类库项目。 新的类库将包含要测试的代码。
* 将 Class1.vb 重命名为 PrimeService.vb。
* 将 PrimeService.vb 中的代码替换为以下代码：
  
  ```vb
  Imports System
  
  Namespace Prime.Services
      Public Class PrimeService
          Public Function IsPrime(candidate As Integer) As Boolean
              Throw New NotImplementedException("Not implemented.")
          End Function
      End Class
  End Namespace
  ```

* 前面的代码：
  * 引发 <xref:System.NotImplementedException>，其中包含一条消息，指示未实现。
  * 稍后在教程中更新。

<!-- preceding code shows an english bias. Message makes no sense outside english -->

* 在 unit-testing-using-dotnet-test 目录下运行以下命令，向解决方案添加类库项目：

  ```dotnetcli
  dotnet sln add ./PrimeService/PrimeService.vbproj
  ```

* 运行以下命令创建 PrimeService.Tests 项目：

  ```dotnetcli
  dotnet new xunit -o PrimeService.Tests
  ```

* 上面的命令：
  * 在 PrimeService.Tests 目录中创建 PrimeService.Tests 项目 。 测试项目将 [xUnit](https://xunit.net/) 用作测试库。
  * 通过将以下 `<PackageReference />` 元素添加到项目文件来配置测试运行程序：
    * “Microsoft.NET.Test.Sdk”
    * “xunit”
    * “xunit.runner.visualstudio”

* 运行以下命令将测试项目添加到解决方案文件：

  ```dotnetcli
  dotnet sln add ./PrimeService.Tests/PrimeService.Tests.vbproj
  ```

* 将 `PrimeService` 类库作为一个依赖项添加到 PrimeService.Tests 项目中：

  ```dotnetcli
  dotnet add ./PrimeService.Tests/PrimeService.Tests.vbproj reference ./PrimeService/PrimeService.vbproj  
  ```

<a name="create-test-cmd"></a>

### <a name="commands-to-create-the-solution"></a>用于创建解决方案的命令

本部分汇总了上一部分中的所有命令。 如果已完成上一部分中的步骤，请跳过本部分。

以下命令用于在 Windows 计算机上创建测试解决方案。 对于 macOS 和 Unix，请将 `ren` 命令更新为 OS 版本的 `ren` 以重命名文件：

```dotnetcli
dotnet new sln -o unit-testing-using-dotnet-test
cd unit-testing-using-dotnet-test
dotnet new classlib -o PrimeService
ren .\PrimeService\Class1.vb PrimeService.vb
dotnet sln add ./PrimeService/PrimeService.vbproj
dotnet new xunit -o PrimeService.Tests
dotnet add ./PrimeService.Tests/PrimeService.Tests.vbproj reference ./PrimeService/PrimeService.vbproj
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.vbproj
```

请按照上一部分中的“将 PrimeService.vb 中的代码替换为以下代码”的说明进行操作。

## <a name="create-a-test"></a>创建测试

测试驱动开发 (TDD) 中的一种常用方法是在实现目标代码之前编写测试。 本教程使用 TDD 方法。 `IsPrime` 方法可调用，但未实现。 对 `IsPrime` 的测试调用失败。 对于 TDD，会编写已知失败的测试。 更新目标代码使测试通过。 你可以重复使用此方法，编写失败的测试，然后更新目标代码使测试通过。

更新 PrimeService.Tests 项目：

* 删除 PrimeService.Tests/UnitTest1.vb。
* 创建 PrimeService.Tests/PrimeService_IsPrimeShould.vb 文件。
* 将 PrimeService_IsPrimeShould.vb 中的代码替换为以下代码：

```vb
Imports Xunit

Namespace PrimeService.Tests
    Public Class PrimeService_IsPrimeShould
        Private ReadOnly _primeService As Prime.Services.PrimeService

        Public Sub New()
            _primeService = New Prime.Services.PrimeService()
        End Sub


        <Fact>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.False(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

`[Fact]` 属性声明由测试运行程序运行的测试方法。 从 PrimeService.Tests 文件夹运行 `dotnet test`。 [dotnet test](../tools/dotnet-test.md) 命令生成两个项目并运行测试。 xUnit 测试运行程序包含要运行测试的程序入口点。 `dotnet test` 使用单元测试项目启动测试运行程序。

测试失败，因为尚未实现 `IsPrime`。 使用 TDD 方法，只需编写足够的代码即可使此测试通过。 使用以下代码更新 `IsPrime`：

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Not implemented.")
End Function
```

运行 `dotnet test`。 测试通过。

### <a name="add-more-tests"></a>添加更多测试

为 0 和 -1 添加素数测试。 你可以复制上述测试并将以下代码更改为使用 0 和 -1：

```vb
Dim result As Boolean = _primeService.IsPrime(1)

Assert.False(result, "1 should not be prime")
```

仅当参数更改代码重复和测试膨胀中的结果时复制测试代码。 以下 xUnit 属性允许编写类似测试套件：

- `[Theory]` 表示执行相同代码，但具有不同输入参数的测试套件。
- `[InlineData]` 属性指定这些输入的值。

可以不使用上述 xUnit 属性创建新测试，而是用来创建单个索引。 替换以下代码：

```vb
<Fact>
Sub IsPrime_InputIs1_ReturnFalse()
    Dim result As Boolean = _primeService.IsPrime(1)

    Assert.False(result, "1 should not be prime")
End Sub
```

替换为以下代码：

```vb
<Theory>
<InlineData(-1)>
<InlineData(0)>
<InlineData(1)>
Sub IsPrime_ValuesLessThan2_ReturnFalse(ByVal value As Integer)
    Dim result As Boolean = _primeService.IsPrime(value)

    Assert.False(result, $"{value} should not be prime")
End Sub
```

在前面的代码中，`[Theory]` 和 `[InlineData]` 允许测试多个小于 2 的值。 2 是最小的素数。

运行 `dotnet test`，其中两个测试失败。 若要使所有测试通过，请使用以下代码更新 `IsPrime` 方法：

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate < 2 Then
        Return False
    End If
    Throw New NotImplementedException("Not fully implemented.")
End Function
```

遵循 TDD 方法，添加更多失败的测试，然后更新目标代码。 请参阅[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-dotnet-test/PrimeService.Tests/PrimeService_IsPrimeShould.vb)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-dotnet-test/PrimeService/PrimeService.vb)。

已完成的 `IsPrime` 方法不是用于测试素性的有效算法。

### <a name="additional-resources"></a>其他资源

- [xUnit.net 官方网站](https://xunit.net/)
- [ASP.NET Core 中的测试控制器逻辑](/aspnet/core/mvc/controllers/testing)
- [`dotnet add reference`](../tools/dotnet-add-reference.md)
