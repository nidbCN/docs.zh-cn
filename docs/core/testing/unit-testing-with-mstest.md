---
title: 使用 MSTest 和 .NET Core 进行 C# 单元测试
description: 通过使用 dotnet test 和 MSTest 分步生成示例解决方案的交互体验，了解 C# 和 .NET Core 中的单元测试概念。
author: ncarandini
ms.author: wiwagn
ms.date: 10/21/2020
ms.openlocfilehash: e2c3326778d7fc1a492062cff4f2d2ad4a61ac18
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874883"
---
# <a name="unit-testing-c-with-mstest-and-net-core"></a>使用 MSTest 和 .NET Core 进行 C# 单元测试

本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。 如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/)。 有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="create-the-source-project"></a>创建源项目

打开 shell 窗口。 创建一个名为 unit-testing-using-mstest 的目录，用以保存解决方案。 在此新目录中，运行 [`dotnet new sln`](../tools/dotnet-new.md) 为类库和测试项目创建新的解决方案文件。 接下来，创建 PrimeService 目录。 下图显示了当前的目录和文件结构：

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
```

将 *PrimeService* 作为当前目录，然后运行 [`dotnet new classlib`](../tools/dotnet-new.md) 以创建源项目。 将 *Class1.cs* 重命名为 *PrimeService.cs*。 创建 `PrimeService` 类的失败实现：

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

将目录更改回 unit-testing-using-mstest 目录。 运行 [`dotnet sln add PrimeService/PrimeService.csproj`](../tools/dotnet-sln.md) 向解决方案添加类库项目。

## <a name="create-the-test-project"></a>创建测试项目

接下来，创建 PrimeService.Tests 目录。 下图显示了它的目录结构：

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

将 *PrimeService.Tests* 目录作为当前目录，并使用 [`dotnet new mstest`](../tools/dotnet-new.md) 创建一个新项目。 dotnet 新命令会创建一个将 MSTest 用作测试库的测试项目。 生成的模板在 *PrimeServiceTests.csproj* 文件中配置测试运行程序：

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.18" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.18" />
</ItemGroup>
```

测试项目需要其他包创建和运行单元测试。 上一步中的 `dotnet new` 添加了 MSTest SDK、MSTest 测试框架和 MSTest 运行程序。 现在，将 `PrimeService` 类库作为另一个依赖项添加到项目中。 使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService.Tests.csproj)中看到整个文件。

下图显示了最终的解决方案布局：

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
        Test Source Files
        PrimeServiceTests.csproj
```

在 unit-testing-using-mstest 目录中执行 [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.csproj`](../tools/dotnet-sln.md)。

## <a name="create-the-first-test"></a>创建第一个测试

编写一个失败测试，使其通过，然后重复此过程。 从 *PrimeService.Tests* 目录删除 *UnitTest1.cs*，并创建一个名为 *PrimeService_IsPrimeShould.cs* 且包含以下内容的新 C# 文件：

```csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    [TestClass]
    public class PrimeService_IsPrimeShould
    {
        [TestMethod]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var primeService = new PrimeService();
            bool result = primeService.IsPrime(1);

            Assert.IsFalse(result, "1 should not be prime");
        }
    }
}
```

[TestClass 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute)表示包含单元测试的类。 [TestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)指示方法是测试方法。

保存此文件并执行 [`dotnet test`](../tools/dotnet-test.md) 以构建测试和类库，然后运行测试。 MSTest 测试运行程序包含要运行测试的程序入口点。 `dotnet test` 使用已创建的单元测试项目启动测试运行程序。

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

在 unit-testing-using-mstest 目录中，再次运行 `dotnet test`。 `dotnet test` 命令构建 `PrimeService` 项目，然后构建 `PrimeService.Tests` 项目。 构建这两个项目后，该命令将运行此单项测试。 测试通过。

## <a name="add-more-features"></a>添加更多功能

你已经通过了一个测试，现在可以编写更多测试。 质数有其他几种简单情况：0，-1。 可使用 [TestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)添加新测试，但这很快就会变得枯燥乏味。 还有其他 MSTest 属性，使用这些属性可编写类似测试的套件。  [DataTestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataTestMethodAttribute)表示执行相同代码但具有不同输入参数的测试套件。 可以使用 [DataRow 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataRowAttribute)来指定这些输入的值。

可以不使用这两个属性创建新测试，而用来创建单个数据驱动的测试。 数据驱动的测试方法用于测试多个小于 2（即最小质数）的值：

[!code-csharp[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-using-mstest/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

运行 `dotnet test`，两项测试均失败。 若要使所有测试通过，可以更改方法开头的 `if` 子句：

```csharp
if (candidate < 2)
```

通过在主库中添加更多测试、理论和代码继续循环访问。 你将拥有[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.cs)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService/PrimeService.cs)。

你已生成一个小型库和该库的一组单元测试。 你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。 你已将多数的时间和精力集中在解决应用程序的目标上。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualStudio.TestTools.UnitTesting>
- [在单元测试中使用 MSTest 框架](/visualstudio/test/using-microsoft-visualstudio-testtools-unittesting-members-in-unit-tests)
- [MSTest V2 测试框架文档](https://github.com/Microsoft/testfx-docs)
