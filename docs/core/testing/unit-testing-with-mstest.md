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
# <a name="unit-testing-c-with-mstest-and-net-core"></a><span data-ttu-id="15207-103">使用 MSTest 和 .NET Core 进行 C# 单元测试</span><span class="sxs-lookup"><span data-stu-id="15207-103">Unit testing C# with MSTest and .NET Core</span></span>

<span data-ttu-id="15207-104">本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。</span><span class="sxs-lookup"><span data-stu-id="15207-104">This tutorial takes you through an interactive experience building a sample solution step-by-step to learn unit testing concepts.</span></span> <span data-ttu-id="15207-105">如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/)。</span><span class="sxs-lookup"><span data-stu-id="15207-105">If you prefer to follow the tutorial using a pre-built solution, [view or download the sample code](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/) before you begin.</span></span> <span data-ttu-id="15207-106">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="15207-106">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="create-the-source-project"></a><span data-ttu-id="15207-107">创建源项目</span><span class="sxs-lookup"><span data-stu-id="15207-107">Create the source project</span></span>

<span data-ttu-id="15207-108">打开 shell 窗口。</span><span class="sxs-lookup"><span data-stu-id="15207-108">Open a shell window.</span></span> <span data-ttu-id="15207-109">创建一个名为 unit-testing-using-mstest 的目录，用以保存解决方案。</span><span class="sxs-lookup"><span data-stu-id="15207-109">Create a directory called *unit-testing-using-mstest* to hold the solution.</span></span> <span data-ttu-id="15207-110">在此新目录中，运行 [`dotnet new sln`](../tools/dotnet-new.md) 为类库和测试项目创建新的解决方案文件。</span><span class="sxs-lookup"><span data-stu-id="15207-110">Inside this new directory, run [`dotnet new sln`](../tools/dotnet-new.md) to create a new solution file for the class library and the test project.</span></span> <span data-ttu-id="15207-111">接下来，创建 PrimeService 目录。</span><span class="sxs-lookup"><span data-stu-id="15207-111">Next, create a *PrimeService* directory.</span></span> <span data-ttu-id="15207-112">下图显示了当前的目录和文件结构：</span><span class="sxs-lookup"><span data-stu-id="15207-112">The following outline shows the directory and file structure thus far:</span></span>

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
```

<span data-ttu-id="15207-113">将 *PrimeService* 作为当前目录，然后运行 [`dotnet new classlib`](../tools/dotnet-new.md) 以创建源项目。</span><span class="sxs-lookup"><span data-stu-id="15207-113">Make *PrimeService* the current directory and run [`dotnet new classlib`](../tools/dotnet-new.md) to create the source project.</span></span> <span data-ttu-id="15207-114">将 *Class1.cs* 重命名为 *PrimeService.cs*。</span><span class="sxs-lookup"><span data-stu-id="15207-114">Rename *Class1.cs* to *PrimeService.cs*.</span></span> <span data-ttu-id="15207-115">创建 `PrimeService` 类的失败实现：</span><span class="sxs-lookup"><span data-stu-id="15207-115">You create a failing implementation of the `PrimeService` class:</span></span>

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

<span data-ttu-id="15207-116">将目录更改回 unit-testing-using-mstest 目录。</span><span class="sxs-lookup"><span data-stu-id="15207-116">Change the directory back to the *unit-testing-using-mstest* directory.</span></span> <span data-ttu-id="15207-117">运行 [`dotnet sln add PrimeService/PrimeService.csproj`](../tools/dotnet-sln.md) 向解决方案添加类库项目。</span><span class="sxs-lookup"><span data-stu-id="15207-117">Run [`dotnet sln add PrimeService/PrimeService.csproj`](../tools/dotnet-sln.md) to add the class library project to the solution.</span></span>

## <a name="create-the-test-project"></a><span data-ttu-id="15207-118">创建测试项目</span><span class="sxs-lookup"><span data-stu-id="15207-118">Create the test project</span></span>

<span data-ttu-id="15207-119">接下来，创建 PrimeService.Tests 目录。</span><span class="sxs-lookup"><span data-stu-id="15207-119">Next, create the *PrimeService.Tests* directory.</span></span> <span data-ttu-id="15207-120">下图显示了它的目录结构：</span><span class="sxs-lookup"><span data-stu-id="15207-120">The following outline shows the directory structure:</span></span>

```console
/unit-testing-using-mstest
    unit-testing-using-mstest.sln
    /PrimeService
        Source Files
        PrimeService.csproj
    /PrimeService.Tests
```

<span data-ttu-id="15207-121">将 *PrimeService.Tests* 目录作为当前目录，并使用 [`dotnet new mstest`](../tools/dotnet-new.md) 创建一个新项目。</span><span class="sxs-lookup"><span data-stu-id="15207-121">Make the *PrimeService.Tests* directory the current directory and create a new project using [`dotnet new mstest`](../tools/dotnet-new.md).</span></span> <span data-ttu-id="15207-122">dotnet 新命令会创建一个将 MSTest 用作测试库的测试项目。</span><span class="sxs-lookup"><span data-stu-id="15207-122">The dotnet new command creates a test project that uses MSTest as the test library.</span></span> <span data-ttu-id="15207-123">生成的模板在 *PrimeServiceTests.csproj* 文件中配置测试运行程序：</span><span class="sxs-lookup"><span data-stu-id="15207-123">The generated template configures the test runner in the *PrimeServiceTests.csproj* file:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0" />
  <PackageReference Include="MSTest.TestAdapter" Version="1.1.18" />
  <PackageReference Include="MSTest.TestFramework" Version="1.1.18" />
</ItemGroup>
```

<span data-ttu-id="15207-124">测试项目需要其他包创建和运行单元测试。</span><span class="sxs-lookup"><span data-stu-id="15207-124">The test project requires other packages to create and run unit tests.</span></span> <span data-ttu-id="15207-125">上一步中的 `dotnet new` 添加了 MSTest SDK、MSTest 测试框架和 MSTest 运行程序。</span><span class="sxs-lookup"><span data-stu-id="15207-125">`dotnet new` in the previous step added the MSTest SDK, the MSTest test framework, and the MSTest runner.</span></span> <span data-ttu-id="15207-126">现在，将 `PrimeService` 类库作为另一个依赖项添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="15207-126">Now, add the `PrimeService` class library as another dependency to the project.</span></span> <span data-ttu-id="15207-127">使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="15207-127">Use the [`dotnet add reference`](../tools/dotnet-add-reference.md) command:</span></span>

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.csproj
```

<span data-ttu-id="15207-128">可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService.Tests.csproj)中看到整个文件。</span><span class="sxs-lookup"><span data-stu-id="15207-128">You can see the entire file in the [samples repository](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService.Tests.csproj) on GitHub.</span></span>

<span data-ttu-id="15207-129">下图显示了最终的解决方案布局：</span><span class="sxs-lookup"><span data-stu-id="15207-129">The following outline shows the final solution layout:</span></span>

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

<span data-ttu-id="15207-130">在 unit-testing-using-mstest 目录中执行 [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.csproj`](../tools/dotnet-sln.md)。</span><span class="sxs-lookup"><span data-stu-id="15207-130">Execute [`dotnet sln add .\PrimeService.Tests\PrimeService.Tests.csproj`](../tools/dotnet-sln.md) in the *unit-testing-using-mstest* directory.</span></span>

## <a name="create-the-first-test"></a><span data-ttu-id="15207-131">创建第一个测试</span><span class="sxs-lookup"><span data-stu-id="15207-131">Create the first test</span></span>

<span data-ttu-id="15207-132">编写一个失败测试，使其通过，然后重复此过程。</span><span class="sxs-lookup"><span data-stu-id="15207-132">You write one failing test, make it pass, then repeat the process.</span></span> <span data-ttu-id="15207-133">从 *PrimeService.Tests* 目录删除 *UnitTest1.cs*，并创建一个名为 *PrimeService_IsPrimeShould.cs* 且包含以下内容的新 C# 文件：</span><span class="sxs-lookup"><span data-stu-id="15207-133">Remove *UnitTest1.cs* from the *PrimeService.Tests* directory and create a new C# file named *PrimeService_IsPrimeShould.cs* with the following content:</span></span>

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

<span data-ttu-id="15207-134">[TestClass 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute)表示包含单元测试的类。</span><span class="sxs-lookup"><span data-stu-id="15207-134">The [TestClass attribute](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) denotes a class that contains unit tests.</span></span> <span data-ttu-id="15207-135">[TestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)指示方法是测试方法。</span><span class="sxs-lookup"><span data-stu-id="15207-135">The [TestMethod attribute](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) indicates a method is a test method.</span></span>

<span data-ttu-id="15207-136">保存此文件并执行 [`dotnet test`](../tools/dotnet-test.md) 以构建测试和类库，然后运行测试。</span><span class="sxs-lookup"><span data-stu-id="15207-136">Save this file and execute [`dotnet test`](../tools/dotnet-test.md) to build the tests and the class library and then run the tests.</span></span> <span data-ttu-id="15207-137">MSTest 测试运行程序包含要运行测试的程序入口点。</span><span class="sxs-lookup"><span data-stu-id="15207-137">The MSTest test runner contains the program entry point to run your tests.</span></span> <span data-ttu-id="15207-138">`dotnet test` 使用已创建的单元测试项目启动测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="15207-138">`dotnet test` starts the test runner using the unit test project you've created.</span></span>

<span data-ttu-id="15207-139">测试失败。</span><span class="sxs-lookup"><span data-stu-id="15207-139">Your test fails.</span></span> <span data-ttu-id="15207-140">尚未创建实现。</span><span class="sxs-lookup"><span data-stu-id="15207-140">You haven't created the implementation yet.</span></span> <span data-ttu-id="15207-141">在起作用的 `PrimeService` 类中编写最简单的代码，使此测试通过：</span><span class="sxs-lookup"><span data-stu-id="15207-141">Make this test pass by writing the simplest code in the `PrimeService` class that works:</span></span>

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

<span data-ttu-id="15207-142">在 unit-testing-using-mstest 目录中，再次运行 `dotnet test`。</span><span class="sxs-lookup"><span data-stu-id="15207-142">In the *unit-testing-using-mstest* directory, run `dotnet test` again.</span></span> <span data-ttu-id="15207-143">`dotnet test` 命令构建 `PrimeService` 项目，然后构建 `PrimeService.Tests` 项目。</span><span class="sxs-lookup"><span data-stu-id="15207-143">The `dotnet test` command runs a build for the `PrimeService` project and then for the `PrimeService.Tests` project.</span></span> <span data-ttu-id="15207-144">构建这两个项目后，该命令将运行此单项测试。</span><span class="sxs-lookup"><span data-stu-id="15207-144">After building both projects, it runs this single test.</span></span> <span data-ttu-id="15207-145">测试通过。</span><span class="sxs-lookup"><span data-stu-id="15207-145">It passes.</span></span>

## <a name="add-more-features"></a><span data-ttu-id="15207-146">添加更多功能</span><span class="sxs-lookup"><span data-stu-id="15207-146">Add more features</span></span>

<span data-ttu-id="15207-147">你已经通过了一个测试，现在可以编写更多测试。</span><span class="sxs-lookup"><span data-stu-id="15207-147">Now that you've made one test pass, it's time to write more.</span></span> <span data-ttu-id="15207-148">质数有其他几种简单情况：0，-1。</span><span class="sxs-lookup"><span data-stu-id="15207-148">There are a few other simple cases for prime numbers: 0, -1.</span></span> <span data-ttu-id="15207-149">可使用 [TestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute)添加新测试，但这很快就会变得枯燥乏味。</span><span class="sxs-lookup"><span data-stu-id="15207-149">You could add new tests with the [TestMethod attribute](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute), but that quickly becomes tedious.</span></span> <span data-ttu-id="15207-150">还有其他 MSTest 属性，使用这些属性可编写类似测试的套件。</span><span class="sxs-lookup"><span data-stu-id="15207-150">There are other MSTest attributes that enable you to write a suite of similar tests.</span></span>  <span data-ttu-id="15207-151">[DataTestMethod 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataTestMethodAttribute)表示执行相同代码但具有不同输入参数的测试套件。</span><span class="sxs-lookup"><span data-stu-id="15207-151">A [DataTestMethod attribute](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataTestMethodAttribute) represents a suite of tests that execute the same code but have different input arguments.</span></span> <span data-ttu-id="15207-152">可以使用 [DataRow 属性](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataRowAttribute)来指定这些输入的值。</span><span class="sxs-lookup"><span data-stu-id="15207-152">You can use the [DataRow attribute](xref:Microsoft.VisualStudio.TestTools.UnitTesting.DataRowAttribute) to specify values for those inputs.</span></span>

<span data-ttu-id="15207-153">可以不使用这两个属性创建新测试，而用来创建单个数据驱动的测试。</span><span class="sxs-lookup"><span data-stu-id="15207-153">Instead of creating new tests, apply these two attributes to create a single data driven test.</span></span> <span data-ttu-id="15207-154">数据驱动的测试方法用于测试多个小于 2（即最小质数）的值：</span><span class="sxs-lookup"><span data-stu-id="15207-154">The data driven test is a method that tests several values less than two, which is the lowest prime number:</span></span>

[!code-csharp[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-using-mstest/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

<span data-ttu-id="15207-155">运行 `dotnet test`，两项测试均失败。</span><span class="sxs-lookup"><span data-stu-id="15207-155">Run `dotnet test`, and two of these tests fail.</span></span> <span data-ttu-id="15207-156">若要使所有测试通过，可以更改方法开头的 `if` 子句：</span><span class="sxs-lookup"><span data-stu-id="15207-156">To make all of the tests pass, change the `if` clause at the beginning of the method:</span></span>

```csharp
if (candidate < 2)
```

<span data-ttu-id="15207-157">通过在主库中添加更多测试、理论和代码继续循环访问。</span><span class="sxs-lookup"><span data-stu-id="15207-157">Continue to iterate by adding more tests, more theories, and more code in the main library.</span></span> <span data-ttu-id="15207-158">你将拥有[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.cs)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService/PrimeService.cs)。</span><span class="sxs-lookup"><span data-stu-id="15207-158">You have the [finished version of the tests](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService.Tests/PrimeService_IsPrimeShould.cs) and the [complete implementation of the library](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-mstest/PrimeService/PrimeService.cs).</span></span>

<span data-ttu-id="15207-159">你已生成一个小型库和该库的一组单元测试。</span><span class="sxs-lookup"><span data-stu-id="15207-159">You've built a small library and a set of unit tests for that library.</span></span> <span data-ttu-id="15207-160">你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。</span><span class="sxs-lookup"><span data-stu-id="15207-160">You've structured the solution so that adding new packages and tests is part of the normal workflow.</span></span> <span data-ttu-id="15207-161">你已将多数的时间和精力集中在解决应用程序的目标上。</span><span class="sxs-lookup"><span data-stu-id="15207-161">You've concentrated most of your time and effort on solving the goals of the application.</span></span>

## <a name="see-also"></a><span data-ttu-id="15207-162">另请参阅</span><span class="sxs-lookup"><span data-stu-id="15207-162">See also</span></span>

- <xref:Microsoft.VisualStudio.TestTools.UnitTesting>
- [<span data-ttu-id="15207-163">在单元测试中使用 MSTest 框架</span><span class="sxs-lookup"><span data-stu-id="15207-163">Use the MSTest framework in unit tests</span></span>](/visualstudio/test/using-microsoft-visualstudio-testtools-unittesting-members-in-unit-tests)
- [<span data-ttu-id="15207-164">MSTest V2 测试框架文档</span><span class="sxs-lookup"><span data-stu-id="15207-164">MSTest V2 test framework docs</span></span>](https://github.com/Microsoft/testfx-docs)
