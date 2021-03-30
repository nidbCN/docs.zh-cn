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
# <a name="unit-testing-visual-basic-net-core-libraries-using-dotnet-test-and-nunit"></a><span data-ttu-id="e52b7-103">使用 dotnet test 和 NUnit 进行 Visual Basic .NET Core 库的单元测试</span><span class="sxs-lookup"><span data-stu-id="e52b7-103">Unit testing Visual Basic .NET Core libraries using dotnet test and NUnit</span></span>

<span data-ttu-id="e52b7-104">本教程介绍分步构建示例解决方案的交互式体验，以了解单元测试概念。</span><span class="sxs-lookup"><span data-stu-id="e52b7-104">This tutorial takes you through an interactive experience building a sample solution step-by-step to learn unit testing concepts.</span></span> <span data-ttu-id="e52b7-105">如果希望使用预构建解决方案学习本教程，请在开始前[查看或下载示例代码](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-vb-nunit/)。</span><span class="sxs-lookup"><span data-stu-id="e52b7-105">If you prefer to follow the tutorial using a pre-built solution, [view or download the sample code](https://github.com/dotnet/samples/tree/main/core/getting-started/unit-testing-vb-nunit/) before you begin.</span></span> <span data-ttu-id="e52b7-106">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="e52b7-106">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

[!INCLUDE [testing an ASP.NET Core project from .NET Core](../../../includes/core-testing-note-aspnet.md)]

## <a name="prerequisites"></a><span data-ttu-id="e52b7-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="e52b7-107">Prerequisites</span></span>

- <span data-ttu-id="e52b7-108">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="e52b7-108">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) or later versions.</span></span>
- <span data-ttu-id="e52b7-109">按需选择的文本编辑器或代码编辑器。</span><span class="sxs-lookup"><span data-stu-id="e52b7-109">A text editor or code editor of your choice.</span></span>

## <a name="creating-the-source-project"></a><span data-ttu-id="e52b7-110">创建源项目</span><span class="sxs-lookup"><span data-stu-id="e52b7-110">Creating the source project</span></span>

<span data-ttu-id="e52b7-111">打开 shell 窗口。</span><span class="sxs-lookup"><span data-stu-id="e52b7-111">Open a shell window.</span></span> <span data-ttu-id="e52b7-112">创建一个名为 unit-testing-vb-nunit 的目录，以保留该解决方案。</span><span class="sxs-lookup"><span data-stu-id="e52b7-112">Create a directory called *unit-testing-vb-nunit* to hold the solution.</span></span> <span data-ttu-id="e52b7-113">在此新目录中，运行以下命令，为类库和测试项目创建新的解决方案文件：</span><span class="sxs-lookup"><span data-stu-id="e52b7-113">Inside this new directory, run the following command to create a new solution file for the class library and the test project:</span></span>

```dotnetcli
dotnet new sln
```

<span data-ttu-id="e52b7-114">接下来，创建 PrimeService 目录。</span><span class="sxs-lookup"><span data-stu-id="e52b7-114">Next, create a *PrimeService* directory.</span></span> <span data-ttu-id="e52b7-115">下图显示了当前的文件结构：</span><span class="sxs-lookup"><span data-stu-id="e52b7-115">The following outline shows the file structure so far:</span></span>

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
```

<span data-ttu-id="e52b7-116">将 PrimeService 作为当前目录，并运行以下命令以创建源项目：</span><span class="sxs-lookup"><span data-stu-id="e52b7-116">Make *PrimeService* the current directory and run the following command to create the source project:</span></span>

```dotnetcli
dotnet new classlib -lang VB
```

<span data-ttu-id="e52b7-117">将 Class1.VB 重命名为 PrimeService.VB。</span><span class="sxs-lookup"><span data-stu-id="e52b7-117">Rename *Class1.VB* to *PrimeService.VB*.</span></span> <span data-ttu-id="e52b7-118">创建 `PrimeService` 类的失败实现：</span><span class="sxs-lookup"><span data-stu-id="e52b7-118">You create a failing implementation of the `PrimeService` class:</span></span>

```vb
Namespace Prime.Services
    Public Class PrimeService
        Public Function IsPrime(candidate As Integer) As Boolean
            Throw New NotImplementedException("Please create a test first.")
        End Function
    End Class
End Namespace
```

<span data-ttu-id="e52b7-119">将目录更改回 unit-testing-vb-using-mstest 目录。</span><span class="sxs-lookup"><span data-stu-id="e52b7-119">Change the directory back to the *unit-testing-vb-using-mstest* directory.</span></span> <span data-ttu-id="e52b7-120">运行以下命令，向解决方案添加类库项目：</span><span class="sxs-lookup"><span data-stu-id="e52b7-120">Run the following command to add the class library project to the solution:</span></span>

```dotnetcli
dotnet sln add .\PrimeService\PrimeService.vbproj
```

## <a name="creating-the-test-project"></a><span data-ttu-id="e52b7-121">创建测试项目</span><span class="sxs-lookup"><span data-stu-id="e52b7-121">Creating the test project</span></span>

<span data-ttu-id="e52b7-122">接下来，创建 PrimeService.Tests 目录。</span><span class="sxs-lookup"><span data-stu-id="e52b7-122">Next, create the *PrimeService.Tests* directory.</span></span> <span data-ttu-id="e52b7-123">下图显示了它的目录结构：</span><span class="sxs-lookup"><span data-stu-id="e52b7-123">The following outline shows the directory structure:</span></span>

```console
/unit-testing-vb-nunit
    unit-testing-vb-nunit.sln
    /PrimeService
        Source Files
        PrimeService.vbproj
    /PrimeService.Tests
```

<span data-ttu-id="e52b7-124">将 PrimeService.Tests 目录作为当前目录，并使用以下命令创建一个新项目：</span><span class="sxs-lookup"><span data-stu-id="e52b7-124">Make the *PrimeService.Tests* directory the current directory and create a new project using the following command:</span></span>

```dotnetcli
dotnet new nunit -lang VB
```

<span data-ttu-id="e52b7-125">[dotnet new](../tools/dotnet-new.md) 命令可创建一个将 NUnit 用作测试库的测试项目。</span><span class="sxs-lookup"><span data-stu-id="e52b7-125">The [dotnet new](../tools/dotnet-new.md) command creates a test project that uses NUnit as the test library.</span></span> <span data-ttu-id="e52b7-126">生成的模板在 PrimeServiceTests.vbproj 文件中配置了测试运行程序：</span><span class="sxs-lookup"><span data-stu-id="e52b7-126">The generated template configures the test runner in the *PrimeServiceTests.vbproj* file:</span></span>

[!code-xml[Packages](~/samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService.Tests.vbproj#Packages)]

<span data-ttu-id="e52b7-127">测试项目需要其他包创建和运行单元测试。</span><span class="sxs-lookup"><span data-stu-id="e52b7-127">The test project requires other packages to create and run unit tests.</span></span> <span data-ttu-id="e52b7-128">在上一步中，`dotnet new` 已添加 NUnit 和 NUnit 测试适配器。</span><span class="sxs-lookup"><span data-stu-id="e52b7-128">`dotnet new` in the previous step added NUnit and the NUnit test adapter.</span></span> <span data-ttu-id="e52b7-129">现在，将 `PrimeService` 类库作为另一个依赖项添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="e52b7-129">Now, add the `PrimeService` class library as another dependency to the project.</span></span> <span data-ttu-id="e52b7-130">使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="e52b7-130">Use the [`dotnet add reference`](../tools/dotnet-add-reference.md) command:</span></span>

```dotnetcli
dotnet add reference ../PrimeService/PrimeService.vbproj
```

<span data-ttu-id="e52b7-131">可以在 GitHub 上的[示例存储库](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService.Tests.vbproj)中看到整个文件。</span><span class="sxs-lookup"><span data-stu-id="e52b7-131">You can see the entire file in the [samples repository](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService.Tests.vbproj) on GitHub.</span></span>

<span data-ttu-id="e52b7-132">最终的解决方案布局将如下所示：</span><span class="sxs-lookup"><span data-stu-id="e52b7-132">You have the following final solution layout:</span></span>

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

<span data-ttu-id="e52b7-133">在 unit-testing-vb-nunit 目录中执行以下命令：</span><span class="sxs-lookup"><span data-stu-id="e52b7-133">Execute the following command in the *unit-testing-vb-nunit* directory:</span></span>

```dotnetcli
dotnet sln add .\PrimeService.Tests\PrimeService.Tests.vbproj
```

## <a name="creating-the-first-test"></a><span data-ttu-id="e52b7-134">创建第一个测试</span><span class="sxs-lookup"><span data-stu-id="e52b7-134">Creating the first test</span></span>

<span data-ttu-id="e52b7-135">编写一个失败测试，使其通过，然后重复此过程。</span><span class="sxs-lookup"><span data-stu-id="e52b7-135">You write one failing test, make it pass, then repeat the process.</span></span> <span data-ttu-id="e52b7-136">在 PrimeService.Tests 目录中，将 UnitTest1.vb 文件重命名为 PrimeService_IsPrimeShould.VB，并将其整个内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="e52b7-136">In the *PrimeService.Tests* directory, rename the *UnitTest1.vb* file to *PrimeService_IsPrimeShould.VB* and replace its entire contents with the following code:</span></span>

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

<span data-ttu-id="e52b7-137">`<TestFixture>` 属性指示包含测试的类。</span><span class="sxs-lookup"><span data-stu-id="e52b7-137">The `<TestFixture>` attribute indicates a class that contains tests.</span></span> <span data-ttu-id="e52b7-138">`<Test>` 属性表示由测试运行程序运行的方法。</span><span class="sxs-lookup"><span data-stu-id="e52b7-138">The `<Test>` attribute denotes a method that is run by the test runner.</span></span> <span data-ttu-id="e52b7-139">在 unit-testing-vb-nunit 中，执行 [`dotnet test`](../tools/dotnet-test.md) 以构建测试和类库，然后运行测试。</span><span class="sxs-lookup"><span data-stu-id="e52b7-139">From the *unit-testing-vb-nunit*, execute [`dotnet test`](../tools/dotnet-test.md) to build the tests and the class library and then run the tests.</span></span> <span data-ttu-id="e52b7-140">NUnit 测试运行程序包含要运行测试的程序入口点。</span><span class="sxs-lookup"><span data-stu-id="e52b7-140">The NUnit test runner contains the program entry point to run your tests.</span></span> <span data-ttu-id="e52b7-141">`dotnet test` 使用已创建的单元测试项目启动测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="e52b7-141">`dotnet test` starts the test runner using the unit test project you've created.</span></span>

<span data-ttu-id="e52b7-142">测试失败。</span><span class="sxs-lookup"><span data-stu-id="e52b7-142">Your test fails.</span></span> <span data-ttu-id="e52b7-143">尚未创建实现。</span><span class="sxs-lookup"><span data-stu-id="e52b7-143">You haven't created the implementation yet.</span></span> <span data-ttu-id="e52b7-144">在起作用的 `PrimeService` 类中编写最简单的代码，使此测试通过：</span><span class="sxs-lookup"><span data-stu-id="e52b7-144">Make this test pass by writing the simplest code in the `PrimeService` class that works:</span></span>

```vb
Public Function IsPrime(candidate As Integer) As Boolean
    If candidate = 1 Then
        Return False
    End If
    Throw New NotImplementedException("Please create a test first.")
End Function
```

<span data-ttu-id="e52b7-145">在 unit-testing-vb-nunit 目录中，再次运行 `dotnet test`。</span><span class="sxs-lookup"><span data-stu-id="e52b7-145">In the *unit-testing-vb-nunit* directory, run `dotnet test` again.</span></span> <span data-ttu-id="e52b7-146">`dotnet test` 命令构建 `PrimeService` 项目，然后构建 `PrimeService.Tests` 项目。</span><span class="sxs-lookup"><span data-stu-id="e52b7-146">The `dotnet test` command runs a build for the `PrimeService` project and then for the `PrimeService.Tests` project.</span></span> <span data-ttu-id="e52b7-147">构建这两个项目后，该命令将运行此单项测试。</span><span class="sxs-lookup"><span data-stu-id="e52b7-147">After building both projects, it runs this single test.</span></span> <span data-ttu-id="e52b7-148">测试通过。</span><span class="sxs-lookup"><span data-stu-id="e52b7-148">It passes.</span></span>

## <a name="adding-more-features"></a><span data-ttu-id="e52b7-149">添加更多功能</span><span class="sxs-lookup"><span data-stu-id="e52b7-149">Adding more features</span></span>

<span data-ttu-id="e52b7-150">你已经通过了一个测试，现在可以编写更多测试。</span><span class="sxs-lookup"><span data-stu-id="e52b7-150">Now that you've made one test pass, it's time to write more.</span></span> <span data-ttu-id="e52b7-151">质数有其他几种简单情况：0，-1。</span><span class="sxs-lookup"><span data-stu-id="e52b7-151">There are a few other simple cases for prime numbers: 0, -1.</span></span> <span data-ttu-id="e52b7-152">可以将这些情况添加为具有 `<Test>` 属性的新测试，但这很快就会变得枯燥乏味。</span><span class="sxs-lookup"><span data-stu-id="e52b7-152">You could add those cases as new tests with the `<Test>` attribute, but that quickly becomes tedious.</span></span> <span data-ttu-id="e52b7-153">还有其他 xUnit 属性，可使你编写类似测试套件。</span><span class="sxs-lookup"><span data-stu-id="e52b7-153">There are other xUnit attributes that enable you to write a suite of similar tests.</span></span>  <span data-ttu-id="e52b7-154">`<TestCase>` 属性表示执行相同代码，但具有不同输入参数一系列测试。</span><span class="sxs-lookup"><span data-stu-id="e52b7-154">A `<TestCase>` attribute represents a suite of tests that execute the same code but have different input arguments.</span></span> <span data-ttu-id="e52b7-155">可以使用 `<TestCase>` 属性来指定这些输入的值。</span><span class="sxs-lookup"><span data-stu-id="e52b7-155">You can use the `<TestCase>` attribute to specify values for those inputs.</span></span>

<span data-ttu-id="e52b7-156">无需创建新测试，而是应用这两个属性来创建一系列测试，用于测试小于 2（最小质数）的几个值：</span><span class="sxs-lookup"><span data-stu-id="e52b7-156">Instead of creating new tests, apply these two attributes to create a series of tests that test several values less than two, which is the lowest prime number:</span></span>

[!code-vb[Sample_TestCode](../../../samples/snippets/core/testing/unit-testing-vb-nunit/vb/PrimeService.Tests/PrimeService_IsPrimeShould.vb?name=Sample_TestCode)]

<span data-ttu-id="e52b7-157">运行 `dotnet test`，两项测试均失败。</span><span class="sxs-lookup"><span data-stu-id="e52b7-157">Run `dotnet test`, and two of these tests fail.</span></span> <span data-ttu-id="e52b7-158">若要使所有测试通过，可以在 PrimeServices.cs 文件中更改 `Main` 方法开头的 `if` 子句：</span><span class="sxs-lookup"><span data-stu-id="e52b7-158">To make all of the tests pass, change the `if` clause at the beginning of the `Main` method in the *PrimeServices.cs* file:</span></span>

```vb
if candidate < 2
```

<span data-ttu-id="e52b7-159">通过在主库中添加更多测试、理论和代码继续循环访问。</span><span class="sxs-lookup"><span data-stu-id="e52b7-159">Continue to iterate by adding more tests, more theories, and more code in the main library.</span></span> <span data-ttu-id="e52b7-160">你将拥有[已完成的测试版本](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.vb)和[库的完整实现](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService/PrimeService.vb)。</span><span class="sxs-lookup"><span data-stu-id="e52b7-160">You have the [finished version of the tests](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService.Tests/PrimeService_IsPrimeShould.vb) and the [complete implementation of the library](https://github.com/dotnet/samples/blob/main/core/getting-started/unit-testing-vb-nunit/PrimeService/PrimeService.vb).</span></span>

<span data-ttu-id="e52b7-161">你已生成一个小型库和该库的一组单元测试。</span><span class="sxs-lookup"><span data-stu-id="e52b7-161">You've built a small library and a set of unit tests for that library.</span></span> <span data-ttu-id="e52b7-162">你已将解决方案结构化，使添加新包和新测试成为了正常工作流的一部分。</span><span class="sxs-lookup"><span data-stu-id="e52b7-162">You've structured the solution so that adding new packages and tests is part of the normal workflow.</span></span> <span data-ttu-id="e52b7-163">你已将多数的时间和精力集中在解决应用程序的目标上。</span><span class="sxs-lookup"><span data-stu-id="e52b7-163">You've concentrated most of your time and effort on solving the goals of the application.</span></span>
