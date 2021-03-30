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
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-nunit"></a><span data-ttu-id="b8a44-103">使用 dotnet test 和 NUnit 在 .NET Core 中进行 F# 库的单元测试</span><span class="sxs-lookup"><span data-stu-id="b8a44-103">Unit testing F# libraries in .NET Core using dotnet test and NUnit</span></span>

<span data-ttu-id="b8a44-104">本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。</span><span class="sxs-lookup"><span data-stu-id="b8a44-104">This tutorial takes you through an interactive experience building a sample solution step-by-step to learn unit testing concepts.</span></span> <span data-ttu-id="b8a44-105">如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-with-fsharp-nunit/)。</span><span class="sxs-lookup"><span data-stu-id="b8a44-105">If you prefer to follow the tutorial using a pre-built solution, [view or download the sample code](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-with-fsharp-nunit/) before you begin.</span></span> <span data-ttu-id="b8a44-106">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="b8a44-106">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a><span data-ttu-id="b8a44-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="b8a44-107">Prerequisites</span></span>

- <span data-ttu-id="b8a44-108">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="b8a44-108">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) or later versions.</span></span>
- <span data-ttu-id="b8a44-109">按需选择的文本编辑器或代码编辑器。</span><span class="sxs-lookup"><span data-stu-id="b8a44-109">A text editor or code editor of your choice.</span></span>

## <a name="creating-the-source-project"></a><span data-ttu-id="b8a44-110">创建源项目</span><span class="sxs-lookup"><span data-stu-id="b8a44-110">Creating the source project</span></span>

<span data-ttu-id="b8a44-111">打开 shell 窗口。</span><span class="sxs-lookup"><span data-stu-id="b8a44-111">Open a shell window.</span></span> <span data-ttu-id="b8a44-112">创建一个名为 unit-testing-with-fsharp 的目录，以保留该解决方案。</span><span class="sxs-lookup"><span data-stu-id="b8a44-112">Create a directory called *unit-testing-with-fsharp* to hold the solution.</span></span>
<span data-ttu-id="b8a44-113">在此新目录中，运行以下命令，为类库和测试项目创建新的解决方案文件：</span><span class="sxs-lookup"><span data-stu-id="b8a44-113">Inside this new directory, run the following command to create a new solution file for the class library and the test project:</span></span>

```dotnetcli
dotnet new sln
```

<span data-ttu-id="b8a44-114">接下来，创建 MathService 目录。</span><span class="sxs-lookup"><span data-stu-id="b8a44-114">Next, create a *MathService* directory.</span></span> <span data-ttu-id="b8a44-115">下图显示了当前的目录和文件结构：</span><span class="sxs-lookup"><span data-stu-id="b8a44-115">The following outline shows the directory and file structure so far:</span></span>

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

<span data-ttu-id="b8a44-116">将 MathService 作为当前目录，并运行以下命令以创建源项目：</span><span class="sxs-lookup"><span data-stu-id="b8a44-116">Make *MathService* the current directory and run the following command to create the source project:</span></span>

```dotnetcli
dotnet new classlib -lang "F#"
```

<span data-ttu-id="b8a44-117">创建数学服务的失败实现：</span><span class="sxs-lookup"><span data-stu-id="b8a44-117">You create a failing implementation of the math service:</span></span>

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

<span data-ttu-id="b8a44-118">将目录更改回 unit-testing-with-fsharp 目录。</span><span class="sxs-lookup"><span data-stu-id="b8a44-118">Change the directory back to the *unit-testing-with-fsharp* directory.</span></span> <span data-ttu-id="b8a44-119">运行以下命令，向解决方案添加类库项目：</span><span class="sxs-lookup"><span data-stu-id="b8a44-119">Run the following command to add the class library project to the solution:</span></span>

```dotnetcli
dotnet sln add .\MathService\MathService.fsproj
```

## <a name="creating-the-test-project"></a><span data-ttu-id="b8a44-120">创建测试项目</span><span class="sxs-lookup"><span data-stu-id="b8a44-120">Creating the test project</span></span>

<span data-ttu-id="b8a44-121">接下来，创建 MathService.Tests 目录。</span><span class="sxs-lookup"><span data-stu-id="b8a44-121">Next, create the *MathService.Tests* directory.</span></span> <span data-ttu-id="b8a44-122">下图显示了它的目录结构：</span><span class="sxs-lookup"><span data-stu-id="b8a44-122">The following outline shows the directory structure:</span></span>

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
```

<span data-ttu-id="b8a44-123">将 MathService.Tests 目录作为当前目录，并使用以下命令创建一个新项目：</span><span class="sxs-lookup"><span data-stu-id="b8a44-123">Make the *MathService.Tests* directory the current directory and create a new project using the following command:</span></span>

```dotnetcli
dotnet new nunit -lang "F#"
```

<span data-ttu-id="b8a44-124">这会创建一个将 NUnit 用作测试框架的测试项目。</span><span class="sxs-lookup"><span data-stu-id="b8a44-124">This creates a test project that uses NUnit as the test framework.</span></span> <span data-ttu-id="b8a44-125">生成的模板在 MathServiceTests.fsproj 中配置测试运行程序：</span><span class="sxs-lookup"><span data-stu-id="b8a44-125">The generated template configures the test runner in the *MathServiceTests.fsproj*:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.5.0" />
  <PackageReference Include="NUnit" Version="3.9.0" />
  <PackageReference Include="NUnit3TestAdapter" Version="3.9.0" />
</ItemGroup>
```

<span data-ttu-id="b8a44-126">测试项目需要其他包创建和运行单元测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-126">The test project requires other packages to create and run unit tests.</span></span> <span data-ttu-id="b8a44-127">在上一步中，`dotnet new` 已添加 NUnit 和 NUnit 测试适配器。</span><span class="sxs-lookup"><span data-stu-id="b8a44-127">`dotnet new` in the previous step added NUnit and the NUnit test adapter.</span></span> <span data-ttu-id="b8a44-128">现在，将 `MathService` 类库作为另一个依赖项添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="b8a44-128">Now, add the `MathService` class library as another dependency to the project.</span></span> <span data-ttu-id="b8a44-129">使用 `dotnet add reference` 命令：</span><span class="sxs-lookup"><span data-stu-id="b8a44-129">Use the `dotnet add reference` command:</span></span>

```dotnetcli
dotnet add reference ../MathService/MathService.fsproj
```

<span data-ttu-id="b8a44-130">可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj)中看到整个文件。</span><span class="sxs-lookup"><span data-stu-id="b8a44-130">You can see the entire file in the [samples repository](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj) on GitHub.</span></span>

<span data-ttu-id="b8a44-131">最终的解决方案布局将如下所示：</span><span class="sxs-lookup"><span data-stu-id="b8a44-131">You have the following final solution layout:</span></span>

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

<span data-ttu-id="b8a44-132">在 unit-testing-with-fsharp 目录中执行以下命令：</span><span class="sxs-lookup"><span data-stu-id="b8a44-132">Execute the following command in the *unit-testing-with-fsharp* directory:</span></span>

```dotnetcli
dotnet sln add .\MathService.Tests\MathService.Tests.fsproj
```

## <a name="creating-the-first-test"></a><span data-ttu-id="b8a44-133">创建第一个测试</span><span class="sxs-lookup"><span data-stu-id="b8a44-133">Creating the first test</span></span>

<span data-ttu-id="b8a44-134">编写一个失败测试，使其通过，然后重复此过程。</span><span class="sxs-lookup"><span data-stu-id="b8a44-134">You write one failing test, make it pass, then repeat the process.</span></span> <span data-ttu-id="b8a44-135">打开 UnitTest1.fs 并添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="b8a44-135">Open *UnitTest1.fs* and add the following code:</span></span>

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

<span data-ttu-id="b8a44-136">`[<TestFixture>]` 属性表示包含测试的类。</span><span class="sxs-lookup"><span data-stu-id="b8a44-136">The `[<TestFixture>]` attribute denotes a class that contains tests.</span></span> <span data-ttu-id="b8a44-137">`[<Test>]` 属性表示由测试运行程序运行的测试方法。</span><span class="sxs-lookup"><span data-stu-id="b8a44-137">The `[<Test>]` attribute denotes a test method that is run by the test runner.</span></span> <span data-ttu-id="b8a44-138">在 unit-testing-with-fsharp 目录中，执行 `dotnet test` 以构建测试和类库，然后运行测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-138">From the *unit-testing-with-fsharp* directory, execute `dotnet test` to build the tests and the class library and then run the tests.</span></span> <span data-ttu-id="b8a44-139">NUnit 测试运行程序包含要运行测试的程序入口点。</span><span class="sxs-lookup"><span data-stu-id="b8a44-139">The NUnit test runner contains the program entry point to run your tests.</span></span> <span data-ttu-id="b8a44-140">`dotnet test` 使用已创建的单元测试项目启动测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="b8a44-140">`dotnet test` starts the test runner using the unit test project you've created.</span></span>

<span data-ttu-id="b8a44-141">这两个测试演示了最基本的已通过测试和未通过测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-141">These two tests show the most basic passing and failing tests.</span></span> <span data-ttu-id="b8a44-142">`My test` 通过，而 `Fail every time` 未通过。</span><span class="sxs-lookup"><span data-stu-id="b8a44-142">`My test` passes, and `Fail every time` fails.</span></span> <span data-ttu-id="b8a44-143">现在创建针对 `squaresOfOdds` 方法的测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-143">Now, create a test for the `squaresOfOdds` method.</span></span> <span data-ttu-id="b8a44-144">`squaresOfOdds` 方法返回输入序列中所有奇整数值的平方序列。</span><span class="sxs-lookup"><span data-stu-id="b8a44-144">The `squaresOfOdds` method returns a sequence of the squares of all odd integer values that are part of the input sequence.</span></span> <span data-ttu-id="b8a44-145">可以以迭代的方式创建可验证此功能的测试，而非尝试同时写入所有的函数。</span><span class="sxs-lookup"><span data-stu-id="b8a44-145">Rather than trying to write all of those functions at once, you can iteratively create tests that validate the functionality.</span></span> <span data-ttu-id="b8a44-146">若要让每个测试都通过，意味着要针对此方法创建必要的功能。</span><span class="sxs-lookup"><span data-stu-id="b8a44-146">Making each test pass means creating the necessary functionality for the method.</span></span>

<span data-ttu-id="b8a44-147">可以编写的最简单的测试是调用包含所有偶数的 `squaresOfOdds`，它的结果应该是一个空整数序列。</span><span class="sxs-lookup"><span data-stu-id="b8a44-147">The simplest test we can write is to call `squaresOfOdds` with all even numbers, where the result should be an empty sequence of integers.</span></span>  <span data-ttu-id="b8a44-148">此测试如下所示：</span><span class="sxs-lookup"><span data-stu-id="b8a44-148">Here's that test:</span></span>

```fsharp
[<Test>]
member this.TestEvenSequence() =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

<span data-ttu-id="b8a44-149">请注意已将 `expected` 序列转换为列表。</span><span class="sxs-lookup"><span data-stu-id="b8a44-149">Notice that the `expected` sequence has been converted to a list.</span></span> <span data-ttu-id="b8a44-150">NUnit 框架依赖于许多标准 .NET 类型。</span><span class="sxs-lookup"><span data-stu-id="b8a44-150">The NUnit framework relies on many standard .NET types.</span></span> <span data-ttu-id="b8a44-151">此依赖关系表示公共接口和预期结果支持 <xref:System.Collections.ICollection>，而非 <xref:System.Collections.IEnumerable>。</span><span class="sxs-lookup"><span data-stu-id="b8a44-151">That dependency means that your public interface and expected results support <xref:System.Collections.ICollection> rather than <xref:System.Collections.IEnumerable>.</span></span>

<span data-ttu-id="b8a44-152">运行此测试时，会看到测试失败。</span><span class="sxs-lookup"><span data-stu-id="b8a44-152">When you run the test, you see that your test fails.</span></span> <span data-ttu-id="b8a44-153">尚未创建实现。</span><span class="sxs-lookup"><span data-stu-id="b8a44-153">You haven't created the implementation yet.</span></span> <span data-ttu-id="b8a44-154">在起作用的 MathService 项目的 Library.fs 类中编写最简单的代码，使此测试通过：</span><span class="sxs-lookup"><span data-stu-id="b8a44-154">Make this test pass by writing the simplest code in the *Library.fs* class in your MathService project that works:</span></span>

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

<span data-ttu-id="b8a44-155">在 unit-testing-with-fsharp 目录中，再次运行 `dotnet test`。</span><span class="sxs-lookup"><span data-stu-id="b8a44-155">In the *unit-testing-with-fsharp* directory, run `dotnet test` again.</span></span> <span data-ttu-id="b8a44-156">`dotnet test` 命令构建 `MathService` 项目，然后构建 `MathService.Tests` 项目。</span><span class="sxs-lookup"><span data-stu-id="b8a44-156">The `dotnet test` command runs a build for the `MathService` project and then for the `MathService.Tests` project.</span></span> <span data-ttu-id="b8a44-157">构建这两个项目后，该命令将运行测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-157">After building both projects, it runs your tests.</span></span> <span data-ttu-id="b8a44-158">现在两个测试通过。</span><span class="sxs-lookup"><span data-stu-id="b8a44-158">Two tests pass now.</span></span>

## <a name="completing-the-requirements"></a><span data-ttu-id="b8a44-159">完成要求</span><span class="sxs-lookup"><span data-stu-id="b8a44-159">Completing the requirements</span></span>

<span data-ttu-id="b8a44-160">你已经通过了一个测试，现在可以编写更多测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-160">Now that you've made one test pass, it's time to write more.</span></span> <span data-ttu-id="b8a44-161">下一个简单示例使用的序列包含的唯一奇数为 `1`。</span><span class="sxs-lookup"><span data-stu-id="b8a44-161">The next simple case works with a sequence whose only odd number is `1`.</span></span> <span data-ttu-id="b8a44-162">数值 1 较为简单，因为 1 的平方是 1。</span><span class="sxs-lookup"><span data-stu-id="b8a44-162">The number 1 is easier because the square of 1 is 1.</span></span> <span data-ttu-id="b8a44-163">下一个测试如下所示：</span><span class="sxs-lookup"><span data-stu-id="b8a44-163">Here's that next test:</span></span>

```fsharp
[<Test>]
member public this.TestOnesAndEvens() =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

<span data-ttu-id="b8a44-164">如果执行 `dotnet test`，新测试将失败。</span><span class="sxs-lookup"><span data-stu-id="b8a44-164">Executing `dotnet test` fails the new test.</span></span> <span data-ttu-id="b8a44-165">必须更新 `squaresOfOdds` 方法才能处理此新测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-165">You must update the `squaresOfOdds` method to handle this new test.</span></span> <span data-ttu-id="b8a44-166">必须筛选出序列中的所有偶数值，以使此测试通过。</span><span class="sxs-lookup"><span data-stu-id="b8a44-166">You must filter all the even numbers out of the sequence to make this test pass.</span></span> <span data-ttu-id="b8a44-167">可以编写一个小筛选器函数并使用 `Seq.filter` 来实现此目的：</span><span class="sxs-lookup"><span data-stu-id="b8a44-167">You can do that by writing a small filter function and using `Seq.filter`:</span></span>

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

<span data-ttu-id="b8a44-168">注意对 `Seq.toList` 的调用。</span><span class="sxs-lookup"><span data-stu-id="b8a44-168">Notice the call to `Seq.toList`.</span></span> <span data-ttu-id="b8a44-169">它会创建一个列表，此列表将实现 <xref:System.Collections.ICollection> 接口。</span><span class="sxs-lookup"><span data-stu-id="b8a44-169">That creates a list, which implements the <xref:System.Collections.ICollection> interface.</span></span>

<span data-ttu-id="b8a44-170">还要执行一个步骤：计算每个奇数的平方值。</span><span class="sxs-lookup"><span data-stu-id="b8a44-170">There's one more step to go: square each of the odd numbers.</span></span> <span data-ttu-id="b8a44-171">从编写新测试开始：</span><span class="sxs-lookup"><span data-stu-id="b8a44-171">Start by writing a new test:</span></span>

```fsharp
[<Test>]
member public this.TestSquaresOfOdds() =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.That(actual, Is.EqualTo(expected))
```

<span data-ttu-id="b8a44-172">可以通过映射操作传递经过筛选的序列来计算每个奇数的平方，以此方式来修复测试的缺陷：</span><span class="sxs-lookup"><span data-stu-id="b8a44-172">You can fix the test by piping the filtered sequence through a map operation to compute the square of each odd number:</span></span>

```fsharp
let private square x = x * x
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
    |> Seq.map square
```

<span data-ttu-id="b8a44-173">你已生成一个小型库和该库的一组单元测试。</span><span class="sxs-lookup"><span data-stu-id="b8a44-173">You've built a small library and a set of unit tests for that library.</span></span> <span data-ttu-id="b8a44-174">你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。</span><span class="sxs-lookup"><span data-stu-id="b8a44-174">You've structured the solution so that adding new packages and tests is part of the normal workflow.</span></span> <span data-ttu-id="b8a44-175">你已将多数的时间和精力集中在解决应用程序的目标上。</span><span class="sxs-lookup"><span data-stu-id="b8a44-175">You've concentrated most of your time and effort on solving the goals of the application.</span></span>

## <a name="see-also"></a><span data-ttu-id="b8a44-176">另请参阅</span><span class="sxs-lookup"><span data-stu-id="b8a44-176">See also</span></span>

- [<span data-ttu-id="b8a44-177">dotnet add reference</span><span class="sxs-lookup"><span data-stu-id="b8a44-177">dotnet add reference</span></span>](../tools/dotnet-add-reference.md)
- [<span data-ttu-id="b8a44-178">dotnet test</span><span class="sxs-lookup"><span data-stu-id="b8a44-178">dotnet test</span></span>](../tools/dotnet-test.md)
