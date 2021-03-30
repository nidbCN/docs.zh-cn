---
title: 使用 dotnet test 和 xUnit 在 .NET Core 中进行 C# 代码单元测试
description: 通过使用 dotnet test 和 xUnit 分步生成示例解决方案的交互体验，了解 C# 和 .NET Core 中的单元测试概念。
author: ardalis
ms.author: wiwagn
ms.date: 10/21/2020
ms.openlocfilehash: fb4434fc4eebc009fa0bbe075d4d2290bff3b0bf
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873466"
---
# <a name="unit-testing-c-in-net-core-using-dotnet-test-and-xunit"></a><span data-ttu-id="317c8-103">使用 dotnet test 和 xUnit 在 .NET Core 中进行 C# 单元测试</span><span class="sxs-lookup"><span data-stu-id="317c8-103">Unit testing C# in .NET Core using dotnet test and xUnit</span></span>

<span data-ttu-id="317c8-104">本教程演示如何生成包含单元测试项目和源代码项目的解决方案。</span><span class="sxs-lookup"><span data-stu-id="317c8-104">This tutorial shows how to build a solution containing a unit test project and source code project.</span></span> <span data-ttu-id="317c8-105">若要使用预构建解决方案学习本教程，请[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-using-dotnet-test/)。</span><span class="sxs-lookup"><span data-stu-id="317c8-105">To follow the tutorial using a pre-built solution, [view or download the sample code](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-using-dotnet-test/).</span></span> <span data-ttu-id="317c8-106">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="317c8-106">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

## <a name="create-the-solution"></a><span data-ttu-id="317c8-107">创建解决方案</span><span class="sxs-lookup"><span data-stu-id="317c8-107">Create the solution</span></span>

<span data-ttu-id="317c8-108">在本部分中，将创建包含源和测试项目的解决方案。</span><span class="sxs-lookup"><span data-stu-id="317c8-108">In this section, a solution is created that contains the source and test projects.</span></span> <span data-ttu-id="317c8-109">已完成的解决方案具有以下目录结构：</span><span class="sxs-lookup"><span data-stu-id="317c8-109">The completed solution has the following directory structure:</span></span>

```
/unit-testing-using-dotnet-test
    unit-testing-using-dotnet-test.sln
    /PrimeService
        PrimeService.cs
        PrimeService.csproj
    /PrimeService.Tests
        PrimeService_IsPrimeShould.cs
        PrimeServiceTests.csproj
```

<span data-ttu-id="317c8-110">以下说明提供了创建测试解决方案的步骤。</span><span class="sxs-lookup"><span data-stu-id="317c8-110">The following instructions provide the steps to create the test solution.</span></span> <span data-ttu-id="317c8-111">有关通过一个步骤创建测试解决方案的说明，请参阅[用于创建测试解决方案的命令](#create-test-cmd)。</span><span class="sxs-lookup"><span data-stu-id="317c8-111">See [Commands to create test solution](#create-test-cmd) for instructions to create the test solution in one step.</span></span>

* <span data-ttu-id="317c8-112">打开 shell 窗口。</span><span class="sxs-lookup"><span data-stu-id="317c8-112">Open a shell window.</span></span>
* <span data-ttu-id="317c8-113">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="317c8-113">Run the following command:</span></span>

  ```dotnetcli
  dotnet new sln -o unit-testing-using-dotnet-test
  ```

  <span data-ttu-id="317c8-114">[`dotnet new sln`](../tools/dotnet-new.md) 命令用于在 unit-testing-using-dotnet-test 目录中创建新的解决方案。</span><span class="sxs-lookup"><span data-stu-id="317c8-114">The [`dotnet new sln`](../tools/dotnet-new.md) command creates a new solution in the *unit-testing-using-dotnet-test* directory.</span></span>
* <span data-ttu-id="317c8-115">将目录更改为 unit-testing-using-dotnet-test 文件夹。</span><span class="sxs-lookup"><span data-stu-id="317c8-115">Change directory to the *unit-testing-using-dotnet-test* folder.</span></span>
* <span data-ttu-id="317c8-116">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="317c8-116">Run the following command:</span></span>

  ```dotnetcli
  dotnet new classlib -o PrimeService
  ```

   <span data-ttu-id="317c8-117">[`dotnet new classlib`](../tools/dotnet-new.md) 命令用于在 PrimeService 文件夹中创建新的类库项目。</span><span class="sxs-lookup"><span data-stu-id="317c8-117">The [`dotnet new classlib`](../tools/dotnet-new.md) command creates a new class library project  in the *PrimeService* folder.</span></span> <span data-ttu-id="317c8-118">新的类库将包含要测试的代码。</span><span class="sxs-lookup"><span data-stu-id="317c8-118">The new class library will contain the code to be tested.</span></span>
* <span data-ttu-id="317c8-119">将 *Class1.cs* 重命名为 *PrimeService.cs*。</span><span class="sxs-lookup"><span data-stu-id="317c8-119">Rename *Class1.cs* to *PrimeService.cs*.</span></span>
* <span data-ttu-id="317c8-120">将 PrimeService.cs 中的代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="317c8-120">Replace the code in *PrimeService.cs* with the following code:</span></span>
  
  ```csharp
  using System;

  namespace Prime.Services
  {
      public class PrimeService
      {
          public bool IsPrime(int candidate)
          {
              throw new NotImplementedException("Not implemented.");
          }
      }
  }
  ```

* <span data-ttu-id="317c8-121">前面的代码：</span><span class="sxs-lookup"><span data-stu-id="317c8-121">The preceding code:</span></span>
  * <span data-ttu-id="317c8-122">引发 <xref:System.NotImplementedException>，其中包含一条消息，指示未实现。</span><span class="sxs-lookup"><span data-stu-id="317c8-122">Throws a <xref:System.NotImplementedException> with a message indicating it's not implemented.</span></span>
  * <span data-ttu-id="317c8-123">稍后在教程中更新。</span><span class="sxs-lookup"><span data-stu-id="317c8-123">Is updated later in the tutorial.</span></span>

<!-- preceding code shows an english bias. Message makes no sense outside english -->

* <span data-ttu-id="317c8-124">在 unit-testing-using-dotnet-test 目录下运行以下命令，向解决方案添加类库项目：</span><span class="sxs-lookup"><span data-stu-id="317c8-124">In the *unit-testing-using-dotnet-test* directory, run the following command to add the class library project to the solution:</span></span>

  ```dotnetcli
  dotnet sln add ./PrimeService/PrimeService.csproj
  ```

* <span data-ttu-id="317c8-125">运行以下命令创建 PrimeService.Tests 项目：</span><span class="sxs-lookup"><span data-stu-id="317c8-125">Create the *PrimeService.Tests* project by running the following command:</span></span>

  ```dotnetcli
  dotnet new xunit -o PrimeService.Tests
  ```

* <span data-ttu-id="317c8-126">上面的命令：</span><span class="sxs-lookup"><span data-stu-id="317c8-126">The preceding command:</span></span>
  * <span data-ttu-id="317c8-127">在 PrimeService.Tests 目录中创建 PrimeService.Tests 项目 。</span><span class="sxs-lookup"><span data-stu-id="317c8-127">Creates the *PrimeService.Tests* project in the *PrimeService.Tests* directory.</span></span> <span data-ttu-id="317c8-128">测试项目将 [xUnit](https://xunit.net/) 用作测试库。</span><span class="sxs-lookup"><span data-stu-id="317c8-128">The test project uses [xUnit](https://xunit.net/) as the test library.</span></span>
  * <span data-ttu-id="317c8-129">通过将以下 `<PackageReference />` 元素添加到项目文件来配置测试运行程序：</span><span class="sxs-lookup"><span data-stu-id="317c8-129">Configures the test runner by adding the following `<PackageReference />`elements to the project file:</span></span>
    * <span data-ttu-id="317c8-130">“Microsoft.NET.Test.Sdk”</span><span class="sxs-lookup"><span data-stu-id="317c8-130">"Microsoft.NET.Test.Sdk"</span></span>
    * <span data-ttu-id="317c8-131">“xunit”</span><span class="sxs-lookup"><span data-stu-id="317c8-131">"xunit"</span></span>
    * <span data-ttu-id="317c8-132">“xunit.runner.visualstudio”</span><span class="sxs-lookup"><span data-stu-id="317c8-132">"xunit.runner.visualstudio"</span></span>

* <span data-ttu-id="317c8-133">运行以下命令将测试项目添加到解决方案文件：</span><span class="sxs-lookup"><span data-stu-id="317c8-133">Add the test project to the solution file by running the following command:</span></span>

  ```dotnetcli
  dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
  ```

* <span data-ttu-id="317c8-134">将 `PrimeService` 类库作为一个依赖项添加到 PrimeService.Tests 项目中：</span><span class="sxs-lookup"><span data-stu-id="317c8-134">Add the `PrimeService` class library as a dependency to the *PrimeService.Tests* project:</span></span>

  ```dotnetcli
  dotnet add ./PrimeService.Tests/PrimeService.Tests.csproj reference ./PrimeService/PrimeService.csproj  
  ```

<a name="create-test-cmd"></a>

### <a name="commands-to-create-the-solution"></a><span data-ttu-id="317c8-135">用于创建解决方案的命令</span><span class="sxs-lookup"><span data-stu-id="317c8-135">Commands to create the solution</span></span>

<span data-ttu-id="317c8-136">本部分汇总了上一部分中的所有命令。</span><span class="sxs-lookup"><span data-stu-id="317c8-136">This section summarizes all the commands in the previous section.</span></span> <span data-ttu-id="317c8-137">如果已完成上一部分中的步骤，请跳过本部分。</span><span class="sxs-lookup"><span data-stu-id="317c8-137">Skip this section if you've completed the steps in the previous section.</span></span>

<span data-ttu-id="317c8-138">以下命令用于在 Windows 计算机上创建测试解决方案。</span><span class="sxs-lookup"><span data-stu-id="317c8-138">The following commands create the test solution on a windows machine.</span></span> <span data-ttu-id="317c8-139">对于 macOS 和 Unix，请将 `ren` 命令更新为 OS 版本的 `ren` 以重命名文件：</span><span class="sxs-lookup"><span data-stu-id="317c8-139">For macOS and Unix, update the `ren` command to the OS version of `ren` to rename a file:</span></span>

```dotnetcli
dotnet new sln -o unit-testing-using-dotnet-test
cd unit-testing-using-dotnet-test
dotnet new classlib -o PrimeService
ren .\PrimeService\Class1.cs PrimeService.cs
dotnet sln add ./PrimeService/PrimeService.csproj
dotnet new xunit -o PrimeService.Tests
dotnet add ./PrimeService.Tests/PrimeService.Tests.csproj reference ./PrimeService/PrimeService.csproj
dotnet sln add ./PrimeService.Tests/PrimeService.Tests.csproj
```

<span data-ttu-id="317c8-140">请按照上一部分中的“将 PrimeService.cs 中的代码替换为以下代码”的说明进行操作。</span><span class="sxs-lookup"><span data-stu-id="317c8-140">Follow the instructions for "Replace the code in *PrimeService.cs* with the following code" in the previous section.</span></span>

## <a name="create-a-test"></a><span data-ttu-id="317c8-141">创建测试</span><span class="sxs-lookup"><span data-stu-id="317c8-141">Create a test</span></span>

<span data-ttu-id="317c8-142">测试驱动开发 (TDD) 中的一种常用方法是在实现目标代码之前编写测试。</span><span class="sxs-lookup"><span data-stu-id="317c8-142">A popular approach in test driven development (TDD) is to write a test before implementing the target code.</span></span> <span data-ttu-id="317c8-143">本教程使用 TDD 方法。</span><span class="sxs-lookup"><span data-stu-id="317c8-143">This tutorial uses the TDD approach.</span></span> <span data-ttu-id="317c8-144">`IsPrime` 方法可调用，但未实现。</span><span class="sxs-lookup"><span data-stu-id="317c8-144">The `IsPrime` method is callable, but not implemented.</span></span> <span data-ttu-id="317c8-145">对 `IsPrime` 的测试调用失败。</span><span class="sxs-lookup"><span data-stu-id="317c8-145">A test call to `IsPrime` fails.</span></span> <span data-ttu-id="317c8-146">对于 TDD，会编写已知失败的测试。</span><span class="sxs-lookup"><span data-stu-id="317c8-146">With TDD, a test is written that is known to fail.</span></span> <span data-ttu-id="317c8-147">更新目标代码使测试通过。</span><span class="sxs-lookup"><span data-stu-id="317c8-147">The target code is updated to make the test pass.</span></span> <span data-ttu-id="317c8-148">你可以重复使用此方法，编写失败的测试，然后更新目标代码使测试通过。</span><span class="sxs-lookup"><span data-stu-id="317c8-148">You keep repeating this approach, writing a failing test and then updating the target code to pass.</span></span>

<span data-ttu-id="317c8-149">更新 PrimeService.Tests 项目：</span><span class="sxs-lookup"><span data-stu-id="317c8-149">Update the *PrimeService.Tests* project:</span></span>

* <span data-ttu-id="317c8-150">删除 PrimeService.Tests/UnitTest1.cs。</span><span class="sxs-lookup"><span data-stu-id="317c8-150">Delete *PrimeService.Tests/UnitTest1.cs*.</span></span>
* <span data-ttu-id="317c8-151">创建 PrimeService.Tests/PrimeService_IsPrimeShould.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="317c8-151">Create a *PrimeService.Tests/PrimeService_IsPrimeShould.cs*  file.</span></span>
* <span data-ttu-id="317c8-152">将 PrimeService_IsPrimeShould.cs 中的代码替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="317c8-152">Replace the code in *PrimeService_IsPrimeShould.cs* with the following code:</span></span>

```csharp
using Xunit;
using Prime.Services;

namespace Prime.UnitTests.Services
{
    public class PrimeService_IsPrimeShould
    {
        [Fact]
        public void IsPrime_InputIs1_ReturnFalse()
        {
            var primeService = new PrimeService();
            bool result = primeService.IsPrime(1);

            Assert.False(result, "1 should not be prime");
        }
    }
}
```

<span data-ttu-id="317c8-153">`[Fact]` 属性声明由测试运行程序运行的测试方法。</span><span class="sxs-lookup"><span data-stu-id="317c8-153">The `[Fact]` attribute declares a test method that's run by the test runner.</span></span> <span data-ttu-id="317c8-154">从 PrimeService.Tests 文件夹运行 `dotnet test`。</span><span class="sxs-lookup"><span data-stu-id="317c8-154">From the *PrimeService.Tests* folder, run `dotnet test`.</span></span> <span data-ttu-id="317c8-155">[dotnet test](../tools/dotnet-test.md) 命令生成两个项目并运行测试。</span><span class="sxs-lookup"><span data-stu-id="317c8-155">The [dotnet test](../tools/dotnet-test.md) command builds both projects and runs the tests.</span></span> <span data-ttu-id="317c8-156">xUnit 测试运行程序包含要运行测试的程序入口点。</span><span class="sxs-lookup"><span data-stu-id="317c8-156">The xUnit test runner contains the program entry point to run the tests.</span></span> <span data-ttu-id="317c8-157">`dotnet test` 使用单元测试项目启动测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="317c8-157">`dotnet test` starts the test runner using the unit test project.</span></span>

<span data-ttu-id="317c8-158">测试失败，因为尚未实现 `IsPrime`。</span><span class="sxs-lookup"><span data-stu-id="317c8-158">The test fails because `IsPrime` hasn't been implemented.</span></span> <span data-ttu-id="317c8-159">使用 TDD 方法，只需编写足够的代码即可使此测试通过。</span><span class="sxs-lookup"><span data-stu-id="317c8-159">Using the TDD approach, write only enough code so this test passes.</span></span> <span data-ttu-id="317c8-160">使用以下代码更新 `IsPrime`：</span><span class="sxs-lookup"><span data-stu-id="317c8-160">Update `IsPrime` with the following code:</span></span>

```csharp
public bool IsPrime(int candidate)
{
    if (candidate == 1)
    {
        return false;
    }
    throw new NotImplementedException("Not fully implemented.");
}
```

<span data-ttu-id="317c8-161">运行 `dotnet test`。</span><span class="sxs-lookup"><span data-stu-id="317c8-161">Run `dotnet test`.</span></span> <span data-ttu-id="317c8-162">测试通过。</span><span class="sxs-lookup"><span data-stu-id="317c8-162">The test passes.</span></span>

### <a name="add-more-tests"></a><span data-ttu-id="317c8-163">添加更多测试</span><span class="sxs-lookup"><span data-stu-id="317c8-163">Add more tests</span></span>

<span data-ttu-id="317c8-164">为 0 和 -1 添加素数测试。</span><span class="sxs-lookup"><span data-stu-id="317c8-164">Add prime number tests for 0 and -1.</span></span> <span data-ttu-id="317c8-165">你可以复制上述测试并将以下代码更改为使用 0 和 -1：</span><span class="sxs-lookup"><span data-stu-id="317c8-165">You could copy the preceding test and change the following code to use 0 and -1:</span></span>

```csharp
var primeService = new PrimeService();
bool result = primeService.IsPrime(1);

Assert.False(result, "1 should not be prime");
```

<span data-ttu-id="317c8-166">仅当参数更改代码重复和测试膨胀中的结果时复制测试代码。</span><span class="sxs-lookup"><span data-stu-id="317c8-166">Copying test code when only a parameter changes results in code duplication and test bloat.</span></span> <span data-ttu-id="317c8-167">以下 xUnit 属性允许编写类似测试套件：</span><span class="sxs-lookup"><span data-stu-id="317c8-167">The following xUnit attributes enable writing a suite of similar tests:</span></span>

- <span data-ttu-id="317c8-168">`[Theory]` 表示执行相同代码，但具有不同输入参数的测试套件。</span><span class="sxs-lookup"><span data-stu-id="317c8-168">`[Theory]` represents a suite of tests that execute the same code but have different input arguments.</span></span>
- <span data-ttu-id="317c8-169">`[InlineData]` 属性指定这些输入的值。</span><span class="sxs-lookup"><span data-stu-id="317c8-169">`[InlineData]` attribute specifies values for those inputs.</span></span>

<span data-ttu-id="317c8-170">可以不使用上述 xUnit 属性创建新测试，而是用来创建单个索引。</span><span class="sxs-lookup"><span data-stu-id="317c8-170">Rather than creating new tests, apply the preceding xUnit attributes to create a single theory.</span></span> <span data-ttu-id="317c8-171">替换以下代码：</span><span class="sxs-lookup"><span data-stu-id="317c8-171">Replace the following code:</span></span>

```csharp
[Fact]
public void IsPrime_InputIs1_ReturnFalse()
{
    var primeService = new PrimeService();
    bool result = primeService.IsPrime(1);

    Assert.False(result, "1 should not be prime");
}
```

<span data-ttu-id="317c8-172">替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="317c8-172">with the following code:</span></span>

[!code-csharp[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-using-dotnet-test/csharp/PrimeService.Tests/PrimeService_IsPrimeShould.cs?name=Sample_TestCode)]

<span data-ttu-id="317c8-173">在前面的代码中，`[Theory]` 和 `[InlineData]` 允许测试多个小于 2 的值。</span><span class="sxs-lookup"><span data-stu-id="317c8-173">In the preceding code, `[Theory]` and `[InlineData]` enable testing several values less than two.</span></span> <span data-ttu-id="317c8-174">2 是最小的素数。</span><span class="sxs-lookup"><span data-stu-id="317c8-174">Two is the smallest prime number.</span></span>

<span data-ttu-id="317c8-175">运行 `dotnet test`，其中两个测试失败。</span><span class="sxs-lookup"><span data-stu-id="317c8-175">Run `dotnet test`, two of the tests fail.</span></span> <span data-ttu-id="317c8-176">若要使所有测试通过，请使用以下代码更新 `IsPrime` 方法：</span><span class="sxs-lookup"><span data-stu-id="317c8-176">To make all of the tests pass, update the `IsPrime` method with the following code:</span></span>

```csharp
public bool IsPrime(int candidate)
{
    if (candidate < 2)
    {
        return false;
    }
    throw new NotImplementedException("Not fully implemented.");
}
```

<span data-ttu-id="317c8-177">遵循 TDD 方法，添加更多失败的测试，然后更新目标代码。</span><span class="sxs-lookup"><span data-stu-id="317c8-177">Following the TDD approach, add more failing tests, then update the target code.</span></span> <span data-ttu-id="317c8-178">请参阅[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-dotnet-test/PrimeService.Tests/PrimeService_IsPrimeShould.cs)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-dotnet-test/PrimeService/PrimeService.cs)。</span><span class="sxs-lookup"><span data-stu-id="317c8-178">See the [finished version of the tests](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-dotnet-test/PrimeService.Tests/PrimeService_IsPrimeShould.cs) and the [complete implementation of the library](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-using-dotnet-test/PrimeService/PrimeService.cs).</span></span>

<span data-ttu-id="317c8-179">已完成的 `IsPrime` 方法不是用于测试素性的有效算法。</span><span class="sxs-lookup"><span data-stu-id="317c8-179">The completed `IsPrime` method is not an efficient algorithm for testing primality.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="317c8-180">其他资源</span><span class="sxs-lookup"><span data-stu-id="317c8-180">Additional resources</span></span>

- [<span data-ttu-id="317c8-181">xUnit.net 官方网站</span><span class="sxs-lookup"><span data-stu-id="317c8-181">xUnit.net official site</span></span>](https://xunit.net)
- [<span data-ttu-id="317c8-182">ASP.NET Core 中的测试控制器逻辑</span><span class="sxs-lookup"><span data-stu-id="317c8-182">Testing controller logic in ASP.NET Core</span></span>](/aspnet/core/mvc/controllers/testing)
- [`dotnet add reference`](../tools/dotnet-add-reference.md)
