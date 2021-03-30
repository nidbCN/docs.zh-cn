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
# <a name="unit-testing-f-libraries-in-net-core-using-dotnet-test-and-xunit"></a><span data-ttu-id="2525a-103">使用 dotnet test 和 xUnit 在 .NET Core 中进行 F# 库单元测试</span><span class="sxs-lookup"><span data-stu-id="2525a-103">Unit testing F# libraries in .NET Core using dotnet test and xUnit</span></span>

<span data-ttu-id="2525a-104">本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。</span><span class="sxs-lookup"><span data-stu-id="2525a-104">This tutorial takes you through an interactive experience building a sample solution step-by-step to learn unit testing concepts.</span></span> <span data-ttu-id="2525a-105">如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-with-fsharp/)。</span><span class="sxs-lookup"><span data-stu-id="2525a-105">If you prefer to follow the tutorial using a pre-built solution, [view or download the sample code](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-with-fsharp/) before you begin.</span></span> <span data-ttu-id="2525a-106">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="2525a-106">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="creating-the-source-project"></a><span data-ttu-id="2525a-107">创建源项目</span><span class="sxs-lookup"><span data-stu-id="2525a-107">Creating the source project</span></span>

<span data-ttu-id="2525a-108">打开 shell 窗口。</span><span class="sxs-lookup"><span data-stu-id="2525a-108">Open a shell window.</span></span> <span data-ttu-id="2525a-109">创建一个名为 unit-testing-with-fsharp 的目录，以保留该解决方案。</span><span class="sxs-lookup"><span data-stu-id="2525a-109">Create a directory called *unit-testing-with-fsharp* to hold the solution.</span></span>
<span data-ttu-id="2525a-110">在此新目录中，运行 `dotnet new sln` 创建新的解决方案。</span><span class="sxs-lookup"><span data-stu-id="2525a-110">Inside this new directory, run `dotnet new sln` to create a new solution.</span></span> <span data-ttu-id="2525a-111">这样便于管理类库和单元测试项目。</span><span class="sxs-lookup"><span data-stu-id="2525a-111">This makes it easier to manage both the class library and the unit test project.</span></span>
<span data-ttu-id="2525a-112">在解决方案库中，创建 MathService 目录。</span><span class="sxs-lookup"><span data-stu-id="2525a-112">Inside the solution directory, create a *MathService* directory.</span></span> <span data-ttu-id="2525a-113">目录和文件结构目前如下所示：</span><span class="sxs-lookup"><span data-stu-id="2525a-113">The directory and file structure thus far is shown below:</span></span>

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
```

<span data-ttu-id="2525a-114">将 MathService 作为当前目录，然后运行 `dotnet new classlib -lang "F#"` 以创建源项目。</span><span class="sxs-lookup"><span data-stu-id="2525a-114">Make *MathService* the current directory, and run `dotnet new classlib -lang "F#"` to create the source project.</span></span> <span data-ttu-id="2525a-115">创建数学服务的失败实现：</span><span class="sxs-lookup"><span data-stu-id="2525a-115">You'll create a failing implementation of the math service:</span></span>

```fsharp
module MyMath =
    let squaresOfOdds xs = raise (System.NotImplementedException("You haven't written a test yet!"))
```

<span data-ttu-id="2525a-116">将目录更改回 unit-testing-with-fsharp 目录。</span><span class="sxs-lookup"><span data-stu-id="2525a-116">Change the directory back to the *unit-testing-with-fsharp* directory.</span></span> <span data-ttu-id="2525a-117">运行 `dotnet sln add .\MathService\MathService.fsproj` 向解决方案添加类库项目。</span><span class="sxs-lookup"><span data-stu-id="2525a-117">Run `dotnet sln add .\MathService\MathService.fsproj` to add the class library project to the solution.</span></span>

## <a name="creating-the-test-project"></a><span data-ttu-id="2525a-118">创建测试项目</span><span class="sxs-lookup"><span data-stu-id="2525a-118">Creating the test project</span></span>

<span data-ttu-id="2525a-119">接下来，创建 MathService.Tests 目录。</span><span class="sxs-lookup"><span data-stu-id="2525a-119">Next, create the *MathService.Tests* directory.</span></span> <span data-ttu-id="2525a-120">下图显示了它的目录结构：</span><span class="sxs-lookup"><span data-stu-id="2525a-120">The following outline shows the directory structure:</span></span>

```
/unit-testing-with-fsharp
    unit-testing-with-fsharp.sln
    /MathService
        Source Files
        MathService.fsproj
    /MathService.Tests
```

<span data-ttu-id="2525a-121">将 MathService.Tests 目录作为当前目录，并使用 `dotnet new xunit -lang "F#"` 创建一个新项目。</span><span class="sxs-lookup"><span data-stu-id="2525a-121">Make the *MathService.Tests* directory the current directory and create a new project using `dotnet new xunit -lang "F#"`.</span></span> <span data-ttu-id="2525a-122">这会创建将 xUnit 用作测试库的测试项目。</span><span class="sxs-lookup"><span data-stu-id="2525a-122">This creates a test project that uses xUnit as the test library.</span></span> <span data-ttu-id="2525a-123">生成的模板在 MathServiceTests.fsproj 中配置测试运行程序：</span><span class="sxs-lookup"><span data-stu-id="2525a-123">The generated template configures the test runner in the *MathServiceTests.fsproj*:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.NET.Test.Sdk" Version="15.3.0-preview-20170628-02" />
  <PackageReference Include="xunit" Version="2.2.0" />
  <PackageReference Include="xunit.runner.visualstudio" Version="2.2.0" />
</ItemGroup>
```

<span data-ttu-id="2525a-124">测试项目需要其他包创建和运行单元测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-124">The test project requires other packages to create and run unit tests.</span></span> <span data-ttu-id="2525a-125">`dotnet new` 在以前的步骤中已添加 xUnit 和 xUnit 运行程序。</span><span class="sxs-lookup"><span data-stu-id="2525a-125">`dotnet new` in the previous step added xUnit and the xUnit runner.</span></span> <span data-ttu-id="2525a-126">现在，将 `MathService` 类库作为另一个依赖项添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="2525a-126">Now, add the `MathService` class library as another dependency to the project.</span></span> <span data-ttu-id="2525a-127">使用 `dotnet add reference` 命令：</span><span class="sxs-lookup"><span data-stu-id="2525a-127">Use the `dotnet add reference` command:</span></span>

```dotnetcli
dotnet add reference ../MathService/MathService.fsproj
```

<span data-ttu-id="2525a-128">可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj)中看到整个文件。</span><span class="sxs-lookup"><span data-stu-id="2525a-128">You can see the entire file in the [samples repository](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-with-fsharp/MathService.Tests/MathService.Tests.fsproj) on GitHub.</span></span>

<span data-ttu-id="2525a-129">最终的解决方案布局将如下所示：</span><span class="sxs-lookup"><span data-stu-id="2525a-129">You have the following final solution layout:</span></span>

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

<span data-ttu-id="2525a-130">在 unit-testing-with-fsharp 目录中执行 `dotnet sln add .\MathService.Tests\MathService.Tests.fsproj`。</span><span class="sxs-lookup"><span data-stu-id="2525a-130">Execute `dotnet sln add .\MathService.Tests\MathService.Tests.fsproj` in the *unit-testing-with-fsharp* directory.</span></span>

## <a name="creating-the-first-test"></a><span data-ttu-id="2525a-131">创建第一个测试</span><span class="sxs-lookup"><span data-stu-id="2525a-131">Creating the first test</span></span>

<span data-ttu-id="2525a-132">编写一个失败测试，使其通过，然后重复此过程。</span><span class="sxs-lookup"><span data-stu-id="2525a-132">You write one failing test, make it pass, then repeat the process.</span></span> <span data-ttu-id="2525a-133">打开 Tests.fs 并添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="2525a-133">Open *Tests.fs* and add the following code:</span></span>

```fsharp
[<Fact>]
let ``My test`` () =
    Assert.True(true)

[<Fact>]
let ``Fail every time`` () = Assert.True(false)
```

<span data-ttu-id="2525a-134">`[<Fact>]` 属性表示由测试运行程序运行的测试方法。</span><span class="sxs-lookup"><span data-stu-id="2525a-134">The `[<Fact>]` attribute denotes a test method that is run by the test runner.</span></span> <span data-ttu-id="2525a-135">在 unit-testing-with-fsharp中，执行 `dotnet test` 以构建测试和类库，然后运行测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-135">From the *unit-testing-with-fsharp*, execute `dotnet test` to build the tests and the class library and then run the tests.</span></span> <span data-ttu-id="2525a-136">xUnit 测试运行程序包含要运行测试的程序入口点。</span><span class="sxs-lookup"><span data-stu-id="2525a-136">The xUnit test runner contains the program entry point to run your tests.</span></span> <span data-ttu-id="2525a-137">`dotnet test` 使用已创建的单元测试项目启动测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="2525a-137">`dotnet test` starts the test runner using the unit test project you've created.</span></span>

<span data-ttu-id="2525a-138">这两个测试演示了最基本的已通过测试和未通过测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-138">These two tests show the most basic passing and failing tests.</span></span> <span data-ttu-id="2525a-139">`My test` 通过，而 `Fail every time` 未通过。</span><span class="sxs-lookup"><span data-stu-id="2525a-139">`My test` passes, and `Fail every time` fails.</span></span> <span data-ttu-id="2525a-140">现在创建针对 `squaresOfOdds` 方法的测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-140">Now, create a test for the `squaresOfOdds` method.</span></span> <span data-ttu-id="2525a-141">`squaresOfOdds` 方法返回输入序列中所有奇整数值的平方序列。</span><span class="sxs-lookup"><span data-stu-id="2525a-141">The `squaresOfOdds` method returns a sequence of the squares of all odd integer values that are part of the input sequence.</span></span> <span data-ttu-id="2525a-142">可以以迭代的方式创建可验证此功能的测试，而非尝试同时写入所有的函数。</span><span class="sxs-lookup"><span data-stu-id="2525a-142">Rather than trying to write all of those functions at once, you can iteratively create tests that validate the functionality.</span></span> <span data-ttu-id="2525a-143">若要让每个测试都通过，意味着要针对此方法创建必要的功能。</span><span class="sxs-lookup"><span data-stu-id="2525a-143">Making each test pass means creating the necessary functionality for the method.</span></span>

<span data-ttu-id="2525a-144">可以编写的最简单的测试是调用包含所有偶数的 `squaresOfOdds`，它的结果应该是一个空整数序列。</span><span class="sxs-lookup"><span data-stu-id="2525a-144">The simplest test we can write is to call `squaresOfOdds` with all even numbers, where the result should be an empty sequence of integers.</span></span>  <span data-ttu-id="2525a-145">此测试如下所示：</span><span class="sxs-lookup"><span data-stu-id="2525a-145">Here's that test:</span></span>

```fsharp
[<Fact>]
let ``Sequence of Evens returns empty collection`` () =
    let expected = Seq.empty<int>
    let actual = MyMath.squaresOfOdds [2; 4; 6; 8; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

<span data-ttu-id="2525a-146">测试失败。</span><span class="sxs-lookup"><span data-stu-id="2525a-146">Your test fails.</span></span> <span data-ttu-id="2525a-147">尚未创建实现。</span><span class="sxs-lookup"><span data-stu-id="2525a-147">You haven't created the implementation yet.</span></span> <span data-ttu-id="2525a-148">在起作用的 `MathService` 类中编写最简单的代码，使此测试通过：</span><span class="sxs-lookup"><span data-stu-id="2525a-148">Make this test pass by writing the simplest code in the `MathService` class that works:</span></span>

```fsharp
let squaresOfOdds xs =
    Seq.empty<int>
```

<span data-ttu-id="2525a-149">在 unit-testing-with-fsharp 目录中，再次运行 `dotnet test`。</span><span class="sxs-lookup"><span data-stu-id="2525a-149">In the *unit-testing-with-fsharp* directory, run `dotnet test` again.</span></span> <span data-ttu-id="2525a-150">`dotnet test` 命令构建 `MathService` 项目，然后构建 `MathService.Tests` 项目。</span><span class="sxs-lookup"><span data-stu-id="2525a-150">The `dotnet test` command runs a build for the `MathService` project and then for the `MathService.Tests` project.</span></span> <span data-ttu-id="2525a-151">构建这两个项目后，该命令将运行此单项测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-151">After building both projects, it runs this single test.</span></span> <span data-ttu-id="2525a-152">测试通过。</span><span class="sxs-lookup"><span data-stu-id="2525a-152">It passes.</span></span>

## <a name="completing-the-requirements"></a><span data-ttu-id="2525a-153">完成要求</span><span class="sxs-lookup"><span data-stu-id="2525a-153">Completing the requirements</span></span>

<span data-ttu-id="2525a-154">你已经通过了一个测试，现在可以编写更多测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-154">Now that you've made one test pass, it's time to write more.</span></span> <span data-ttu-id="2525a-155">下一个简单示例使用的序列包含的唯一奇数为 `1`。</span><span class="sxs-lookup"><span data-stu-id="2525a-155">The next simple case works with a sequence whose only odd number is `1`.</span></span> <span data-ttu-id="2525a-156">数值 1 较为简单，因为 1 的平方是 1。</span><span class="sxs-lookup"><span data-stu-id="2525a-156">The number 1 is easier because the square of 1 is 1.</span></span> <span data-ttu-id="2525a-157">下一个测试如下所示：</span><span class="sxs-lookup"><span data-stu-id="2525a-157">Here's that next test:</span></span>

```fsharp
[<Fact>]
let ``Sequences of Ones and Evens returns Ones`` () =
    let expected = [1; 1; 1; 1]
    let actual = MyMath.squaresOfOdds [2; 1; 4; 1; 6; 1; 8; 1; 10]
    Assert.Equal<Collections.Generic.IEnumerable<int>>(expected, actual)
```

<span data-ttu-id="2525a-158">执行 `dotnet test` 以运行测试，并显示新测试失败。</span><span class="sxs-lookup"><span data-stu-id="2525a-158">Executing `dotnet test` runs your tests and shows you that the new test fails.</span></span> <span data-ttu-id="2525a-159">现在更新 `squaresOfOdds` 方法，以处理此新测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-159">Now, update the `squaresOfOdds` method to handle this new test.</span></span> <span data-ttu-id="2525a-160">筛选出序列中的所有偶数值，以使此测试通过。</span><span class="sxs-lookup"><span data-stu-id="2525a-160">You filter all the even numbers out of the sequence to make this test pass.</span></span> <span data-ttu-id="2525a-161">可以编写一个小筛选器函数并使用 `Seq.filter` 来实现此目的：</span><span class="sxs-lookup"><span data-stu-id="2525a-161">You can do that by writing a small filter function and using `Seq.filter`:</span></span>

```fsharp
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
```

<span data-ttu-id="2525a-162">还要执行一个步骤：计算每个奇数的平方值。</span><span class="sxs-lookup"><span data-stu-id="2525a-162">There's one more step to go: square each of the odd numbers.</span></span> <span data-ttu-id="2525a-163">从编写新测试开始：</span><span class="sxs-lookup"><span data-stu-id="2525a-163">Start by writing a new test:</span></span>

```fsharp
[<Fact>]
let ``SquaresOfOdds works`` () =
    let expected = [1; 9; 25; 49; 81]
    let actual = MyMath.squaresOfOdds [1; 2; 3; 4; 5; 6; 7; 8; 9; 10]
    Assert.Equal(expected, actual)
```

<span data-ttu-id="2525a-164">可以通过映射操作传递经过筛选的序列来计算每个奇数的平方，以此方式来修复测试的缺陷：</span><span class="sxs-lookup"><span data-stu-id="2525a-164">You can fix the test by piping the filtered sequence through a map operation to compute the square of each odd number:</span></span>

```fsharp
let private square x = x * x
let private isOdd x = x % 2 <> 0

let squaresOfOdds xs =
    xs
    |> Seq.filter isOdd
    |> Seq.map square
```

<span data-ttu-id="2525a-165">你已生成一个小型库和该库的一组单元测试。</span><span class="sxs-lookup"><span data-stu-id="2525a-165">You've built a small library and a set of unit tests for that library.</span></span> <span data-ttu-id="2525a-166">你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。</span><span class="sxs-lookup"><span data-stu-id="2525a-166">You've structured the solution so that adding new packages and tests is part of the normal workflow.</span></span> <span data-ttu-id="2525a-167">你已将多数的时间和精力集中在解决应用程序的目标上。</span><span class="sxs-lookup"><span data-stu-id="2525a-167">You've concentrated most of your time and effort on solving the goals of the application.</span></span>

## <a name="see-also"></a><span data-ttu-id="2525a-168">另请参阅</span><span class="sxs-lookup"><span data-stu-id="2525a-168">See also</span></span>

- [<span data-ttu-id="2525a-169">dotnet new</span><span class="sxs-lookup"><span data-stu-id="2525a-169">dotnet new</span></span>](../tools/dotnet-new.md)
- [<span data-ttu-id="2525a-170">dotnet sln</span><span class="sxs-lookup"><span data-stu-id="2525a-170">dotnet sln</span></span>](../tools/dotnet-new.md)
- [<span data-ttu-id="2525a-171">dotnet add reference</span><span class="sxs-lookup"><span data-stu-id="2525a-171">dotnet add reference</span></span>](../tools/dotnet-add-reference.md)
- [<span data-ttu-id="2525a-172">dotnet test</span><span class="sxs-lookup"><span data-stu-id="2525a-172">dotnet test</span></span>](../tools/dotnet-test.md)
