---
title: 使用 NUnit 和 .NET Core 进行 C# 单元测试
description: 使用 dotnet test 和 NUnit 分步构建一个示例解决方案，在此交互式体验中学习 C# 和 .NET Core 中的单元测试概念。
author: rprouse
ms.date: 08/31/2018
ms.openlocfilehash: d471521fe324700502415c5e6fb1b28871492b9a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873440"
---
# <a name="unit-testing-c-with-nunit-and-net-core"></a>使用 NUnit 和 .NET Core 进行 C# 单元测试

本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。 如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-nunit/)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a>先决条件

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本。
- 按需选择的文本编辑器或代码编辑器。

## <a name="creating-the-source-project"></a>创建源项目

打开 shell 窗口。 创建一个名为 unit-testing-using-nunit 的目录，以保留该解决方案。 在此新目录中，运行以下命令，为类库和测试项目创建新的解决方案文件：

```dotnetcli
dotnet new sln
```

接下来，创建 PrimeService 目录。 下图显示了当前的目录和文件结构：

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
```

将 PrimeService 作为当前目录，并运行以下命令以创建源项目：

```dotnetcli
dotnet new classlib
```

将 *Class1.cs* 重命名为 *PrimeService.cs*。 创建 `PrimeService` 类的失败实现：

```csharp
using System;

namespace Prime.Services
{
    public class PrimeService
    {
        public bool IsPrime(int candidate)
        {
            throw new NotImplementedException("Please create a test first.");
        }
    }
}
```

将目录更改回 unit-testing-using-nunit 目录。 运行以下命令，向解决方案添加类库项目：

```dotnetcli
dotnet sln add PrimeService/PrimeService.csproj
```

## <a name="creating-the-test-project"></a>创建测试项目

接下来，创建 PrimeService.Tests 目录。 下图显示了它的目录结构：

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

将 PrimeService.Tests 目录作为当前目录，并使用以下命令创建一个新项目：

```dotnetcli
dotnet new nunit
```

[dotnet new](../tools/dotnet-new.md) 命令可创建一个将 NUnit 用作测试库的测试项目。 生成的模板在 PrimeService.Tests.csproj 文件中配置测试运行程序：

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService.Tests.csproj#Packages)]

测试项目需要其他包创建和运行单元测试。 在上一步中，`dotnet new` 已添加 Microsoft 测试 SDK、NUnit 测试框架和 NUnit 测试适配器。 现在，将 `PrimeService` 类库作为另一个依赖项添加到项目中。 使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService.Tests.csproj)中看到整个文件。

下图显示了最终的解决方案布局：

```console
/unit-testing-using-nunit
    unit-testing-using-nunit.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeService.Tests.csproj
```

在 unit-testing-using-nunit 目录中执行以下命令：

```dotnetcli
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
```

## <a name="creating-the-first-test"></a>创建第一个测试

编写一个失败测试，使其通过，然后重复此过程。 在 PrimeService.Tests 目录中，将 UnitTest1.cs 文件重命名为 PrimeService_IsPrimeShould.cs，并将其整个内容替换为以下代码：

```csharp
using NUnit.Framework;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    [TestFixture]
    public class PrimeService_IsPrimeShould
    {
        private PrimeService _primeService;

        [SetUp]
        public void SetUp()
        {
            _primeService = new PrimeService();
        }

        [Test]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var result = _primeService.IsPrime(1);

            Assert.IsFalse(result, "1 should not be prime");
        }
    }
}
```

`[TestFixture]` 属性表示包含单元测试的类。 `[Test]` 属性指示方法是测试方法。

保存此文件并执行 [`dotnet test`](../tools/dotnet-test.md) 以构建测试和类库，然后运行测试。 NUnit 测试运行程序包含要运行测试的程序入口点。 `dotnet test` 使用已创建的单元测试项目启动测试运行程序。

测试失败。 尚未创建实现。 在起作用的 `PrimeService` 类中编写最简单的代码，使此测试通过：

```csharp
public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Please create a test first.");
}
```

在 unit-testing-using-nunit 目录中再次运行 `dotnet test`。 `dotnet test` 命令构建 `PrimeService` 项目，然后构建 `PrimeService.Tests` 项目。 构建这两个项目后，该命令将运行此单项测试。 测试通过。

## <a name="adding-more-features"></a>添加更多功能

你已经通过了一个测试，现在可以编写更多测试。 质数有其他几种简单情况：0，-1。 可以添加具有 `[Test]` 属性的新测试，但这很快就会变得枯燥乏味。 还有其他 NUnit 属性可用于编写一套类似的测试。  `[TestCase]` 属性用于创建一套可执行相同代码但具有不同输入参数的测试。 可以使用 `[TestCase]` 属性来指定这些输入的值。

无需创建新的测试，而是应用此属性来创建数据驱动的单个测试。 数据驱动的测试方法用于测试多个小于 2（即最小质数）的值：

[!code-csharp[Sample_TestCode](~/samples/snippets/core/testing/unit-testing-using-nunit/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

运行 `dotnet test`，两项测试均失败。 若要使所有测试通过，可以在 PrimeService.cs 文件中更改 `Main` 方法开头的 `if` 子句：

```csharp
if (candidate < 2)
```

通过在主库中添加更多测试、理论和代码继续循环访问。 你将拥有[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.cs)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-nunit/PrimeService/PrimeService.cs)。

你已生成一个小型库和该库的一组单元测试。 你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。 你已将多数的时间和精力集中在解决应用程序的目标上。
