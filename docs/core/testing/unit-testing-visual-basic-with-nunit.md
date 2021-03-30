---
title: 使用 dotnet test 和 NUnit 对 .NET Core 中的 Visual Basic 进行单元测试
description: 使用 NUnit 分步构建一个 Visual Basic 示例解决方案，在此交互式体验中学习 .NET Core 中的单元测试概念。
author: rprouse
ms.date: 10/04/2018
ms.openlocfilehash: 11334951d41c5cca24fcfb4853569e503cff97c5
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874896"
---
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-nunit"></a>使用 dotnet test 和 NUnit 进行 Visual Basic .NET Core 库的单元测试

本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。 如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-vb-nunit/)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>先决条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本。
- 按需选择的文本编辑器或代码编辑器。

## <a name="creating-the-source-project"></a>创建源项目

打开 shell 窗口。 创建一个名为 unit-testing-vb-nunit 的目录，以保留该解决方案。 在此新目录中，运行以下命令，为类库和测试项目创建新的解决方案文件：

```dotnetcli
dotnet new sln
```

接下来，创建 PrimeService 目录。 下图显示了当前的文件结构：

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
```

将 PrimeService 作为当前目录，并运行以下命令以创建源项目：

```dotnetcli
dotnet new classlib -lang VB
```

将 Class1.VB 重命名为 PrimeService.VB。 创建 `PrimeService` 类的失败实现：

```vb
Namespace Prime.Services
    Public Class PrimeService
        Public Function IsPrime(candidate As Integer) As Boolean
            Throw New NotImplementedException("Please create a test first.")
        End Function
    End Class
End Namespace
```

将目录更改回 unit-testing-vb-using-mstest 目录。 运行以下命令，向解决方案添加类库项目：

```dotnetcli
dotnet sln add .\PrimeService\PrimeService.vbproj
```

## <a name="creating-the-test-project"></a>创建测试项目

接下来，创建 PrimeService.Tests 目录。 下图显示了它的目录结构：

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
```

将 PrimeService.Tests 目录作为当前目录，并使用以下命令创建一个新项目：

```dotnetcli
dotnet new nunit -lang VB
```

[dotnet new](../tools/dotnet-new.md) 命令可创建一个将 NUnit 用作测试库的测试项目。 生成的模板在 PrimeServiceTests.vbproj 文件中配置了测试运行程序：

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService.Tests.vbproj#Packages)]

测试项目需要其他包创建和运行单元测试。 在上一步中，`dotnet new` 已添加 NUnit 和 NUnit 测试适配器。 现在，将 `PrimeService` 类库作为另一个依赖项添加到项目中。 使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.vbproj
```

可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService.Tests.vbproj)中看到整个文件。

最终的解决方案布局将如下所示：

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.vbproj
```

在 unit-testing-vb-nunit 目录中执行以下命令：

```dotnetcli
dotnet sln add .\PrimeService.Tests\PrimeService.Tests.vbproj
```

## <a name="creating-the-first-test"></a>创建第一个测试

编写一个失败测试，使其通过，然后重复此过程。 在 PrimeService.Tests 目录中，将 UnitTest1.vb 文件重命名为 PrimeService_IsPrimeShould.VB，并将其整个内容替换为以下代码：

```vb
Imports NUnit.Framework

Namespace PrimeService.Tests
    <TestFixture>
    Public Class PrimeService_IsPrimeShould
        Private _primeService As Prime.Services.PrimeService = New Prime.Services.PrimeService()

        <Test>
        Sub IsPrime_InputIs1_ReturnFalse()
            Dim result As Boolean = _primeService.IsPrime(1)

            Assert.False(result, "1 should not be prime")
        End Sub

    End Class
End Namespace
```

`<TestFixture>` 属性指示包含测试的类。 `<Test>` 属性表示由测试运行程序运行的方法。 在 unit-testing-vb-nunit 中，执行 [`dotnet test`](../tools/dotnet-test.md) 以构建测试和类库，然后运行测试。 NUnit 测试运行程序包含要运行测试的程序入口点。 `dotnet test` 使用已创建的单元测试项目启动测试运行程序。

测试失败。 尚未创建实现。 在起作用的 `PrimeService` 类中编写最简单的代码，使此测试通过：

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Please create a test first.")
End Function
```

在 unit-testing-vb-nunit 目录中，再次运行 `dotnet test`。 `dotnet test` 命令构建 `PrimeService` 项目，然后构建 `PrimeService.Tests` 项目。 构建这两个项目后，该命令将运行此单项测试。 测试通过。

## <a name="adding-more-features"></a>添加更多功能

你已经通过了一个测试，现在可以编写更多测试。 质数有其他几种简单情况：0，-1。 可以将这些情况添加为具有 `<Test>` 属性的新测试，但这很快就会变得枯燥乏味。 还有其他 xUnit 属性，可使你编写类似测试套件。  `<TestCase>` 属性表示执行相同代码，但具有不同输入参数一系列测试。 可以使用 `<TestCase>` 属性来指定这些输入的值。

无需创建新测试，而是应用这两个属性来创建一系列测试，用于测试小于 2（最小质数）的几个值：

[!code-vb[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService_IsPrimeShould.vb?name=Sample_TestCode)]

运行 `dotnet test`，两项测试均失败。 若要使所有测试通过，可以在 PrimeServices.cs 文件中更改 `Main` 方法开头的 `if` 子句：

```vb
if candidate < 2
```

通过在主库中添加更多测试、理论和代码继续循环访问。 你将拥有[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.vb)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService/PrimeService.vb)。

你已生成一个小型库和该库的一组单元测试。 你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。 你已将多数的时间和精力集中在解决应用程序的目标上。
