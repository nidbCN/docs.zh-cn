---
title: 使用 Visual Studio Code 测试 .NET 类库
description: 了解如何使用 Visual Studio Code 和 .NET CLI 来针对 .NET 类库创建和运行单元测试项目。
ms.date: 11/17/2020
ms.openlocfilehash: bc8741e11504f94e54ccc45a5ad93408a3fe9309
ms.sourcegitcommit: 1d3af230ec30d8d061be7a887f6ba38a530c4ece
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2021
ms.locfileid: "102511809"
---
# <a name="tutorial-test-a-net-class-library-using-visual-studio-code"></a><span data-ttu-id="6eca5-103">教程：使用 Visual Studio Code 测试 .NET 类库</span><span class="sxs-lookup"><span data-stu-id="6eca5-103">Tutorial: Test a .NET class library using Visual Studio Code</span></span>

<span data-ttu-id="6eca5-104">本教程演示如何通过将测试项目添加到解决方案来自动执行单元测试。</span><span class="sxs-lookup"><span data-stu-id="6eca5-104">This tutorial shows how to automate unit testing by adding a test project to a solution.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6eca5-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="6eca5-105">Prerequisites</span></span>

- <span data-ttu-id="6eca5-106">本教程适用于在[使用 Visual Studio Code 创建 .NET 类库](library-with-visual-studio-code.md)中创建的解决方案。</span><span class="sxs-lookup"><span data-stu-id="6eca5-106">This tutorial works with the solution that you create in [Create a .NET class library using Visual Studio Code](library-with-visual-studio-code.md).</span></span>

## <a name="create-a-unit-test-project"></a><span data-ttu-id="6eca5-107">创建单元测试项目</span><span class="sxs-lookup"><span data-stu-id="6eca5-107">Create a unit test project</span></span>

<span data-ttu-id="6eca5-108">单元测试在开发和发布期间提供自动化的软件测试。</span><span class="sxs-lookup"><span data-stu-id="6eca5-108">Unit tests provide automated software testing during your development and publishing.</span></span> <span data-ttu-id="6eca5-109">本教程中使用的测试框架是 MSTest。</span><span class="sxs-lookup"><span data-stu-id="6eca5-109">The testing framework that you use in this tutorial is MSTest.</span></span> <span data-ttu-id="6eca5-110">[MSTest](https://github.com/Microsoft/testfx-docs) 是可供选择的三个测试框架之一。</span><span class="sxs-lookup"><span data-stu-id="6eca5-110">[MSTest](https://github.com/Microsoft/testfx-docs) is one of three test frameworks you can choose from.</span></span> <span data-ttu-id="6eca5-111">其他两个是 [xUnit](https://xunit.net/) 和 [nUnit](https://nunit.org/)。</span><span class="sxs-lookup"><span data-stu-id="6eca5-111">The others are [xUnit](https://xunit.net/) and [nUnit](https://nunit.org/).</span></span>

1. <span data-ttu-id="6eca5-112">启动 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="6eca5-112">Start Visual Studio Code.</span></span>

1. <span data-ttu-id="6eca5-113">打开在[使用 Visual Studio Code 创建 .NET 类库](library-with-visual-studio-code.md)中创建的 `ClassLibraryProjects` 解决方案。</span><span class="sxs-lookup"><span data-stu-id="6eca5-113">Open the `ClassLibraryProjects` solution you created in [Create a .NET class library using Visual Studio Code](library-with-visual-studio-code.md).</span></span>

1. <span data-ttu-id="6eca5-114">创建名为“StringLibraryTest”的单元测试项目。</span><span class="sxs-lookup"><span data-stu-id="6eca5-114">Create a unit test project named "StringLibraryTest".</span></span>

   ```dotnetcli
   dotnet new mstest -o StringLibraryTest
   ```

   <span data-ttu-id="6eca5-115">项目模板创建包含以下代码的 UnitTest1.cs 文件：</span><span class="sxs-lookup"><span data-stu-id="6eca5-115">The project template creates a UnitTest1.cs file with the following code:</span></span>

   ```csharp
   using Microsoft.VisualStudio.TestTools.UnitTesting;

   namespace StringLibraryTest
   {
       [TestClass]
       public class UnitTest1
       {
           [TestMethod]
           public void TestMethod1()
           {
           }
       }
   }
   ```

   <span data-ttu-id="6eca5-116">单元测试模板创建的源代码负责执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="6eca5-116">The source code created by the unit test template does the following:</span></span>

   - <span data-ttu-id="6eca5-117">它会导入 <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType> 命名空间，其中包含用于单元测试的类型。</span><span class="sxs-lookup"><span data-stu-id="6eca5-117">It imports the <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType> namespace, which contains the types used for unit testing.</span></span>
   - <span data-ttu-id="6eca5-118">向 `UnitTest1` 类应用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 特性。</span><span class="sxs-lookup"><span data-stu-id="6eca5-118">It applies the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> attribute to the `UnitTest1` class.</span></span>
   - <span data-ttu-id="6eca5-119">它应用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 特性来定义 `TestMethod1`。</span><span class="sxs-lookup"><span data-stu-id="6eca5-119">It applies the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> attribute to define `TestMethod1`.</span></span>

   <span data-ttu-id="6eca5-120">使用 [[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) 标记的测试类中标记有 [[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) 的所有测试方法都会在单元测试运行时自动执行。</span><span class="sxs-lookup"><span data-stu-id="6eca5-120">Each method tagged with [[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) in a test class tagged with [[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) is executed automatically when the unit test is run.</span></span>

1. <span data-ttu-id="6eca5-121">向解决方案添加测试项目。</span><span class="sxs-lookup"><span data-stu-id="6eca5-121">Add the test project to the solution.</span></span>

   ```dotnetcli
   dotnet sln add StringLibraryTest/StringLibraryTest.csproj
   ```

## <a name="add-a-project-reference"></a><span data-ttu-id="6eca5-122">添加项目引用</span><span class="sxs-lookup"><span data-stu-id="6eca5-122">Add a project reference</span></span>

<span data-ttu-id="6eca5-123">对于要使用 `StringLibrary` 类的测试库，请在 `StringLibraryTest` 项目中添加对 `StringLibrary` 项目的引用。</span><span class="sxs-lookup"><span data-stu-id="6eca5-123">For the test project to work with the `StringLibrary` class, add a reference in the `StringLibraryTest` project to the `StringLibrary` project.</span></span>

1. <span data-ttu-id="6eca5-124">运行下面的命令：</span><span class="sxs-lookup"><span data-stu-id="6eca5-124">Run the following command:</span></span>

   ```dotnetcli
   dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj
   ```

## <a name="add-and-run-unit-test-methods"></a><span data-ttu-id="6eca5-125">添加并运行单元测试方法</span><span class="sxs-lookup"><span data-stu-id="6eca5-125">Add and run unit test methods</span></span>

<span data-ttu-id="6eca5-126">运行单元测试时，Visual Studio 执行使用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 特性标记的类中标记有 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 特性的所有方法。</span><span class="sxs-lookup"><span data-stu-id="6eca5-126">When Visual Studio runs a unit test, it executes each method that is marked with the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> attribute in a class that is marked with the  <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> attribute.</span></span> <span data-ttu-id="6eca5-127">当第一次遇到测试不通过或测试方法中的所有测试均已成功通过时，测试方法终止。</span><span class="sxs-lookup"><span data-stu-id="6eca5-127">A test method ends when the first failure is found or when all tests contained in the method have succeeded.</span></span>

<span data-ttu-id="6eca5-128">最常见的测试调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 类的成员。</span><span class="sxs-lookup"><span data-stu-id="6eca5-128">The most common tests call members of the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> class.</span></span> <span data-ttu-id="6eca5-129">许多断言方法至少包含两个参数，其中一个是预期的测试结果，另一个是实际的测试结果。</span><span class="sxs-lookup"><span data-stu-id="6eca5-129">Many assert methods include at least two parameters, one of which is the expected test result and the other of which is the actual test result.</span></span> <span data-ttu-id="6eca5-130">下表显示了 `Assert` 类最常调用的一些方法：</span><span class="sxs-lookup"><span data-stu-id="6eca5-130">Some of the `Assert` class's most frequently called methods are shown in the following table:</span></span>

| <span data-ttu-id="6eca5-131">断言方法</span><span class="sxs-lookup"><span data-stu-id="6eca5-131">Assert methods</span></span>     | <span data-ttu-id="6eca5-132">函数</span><span class="sxs-lookup"><span data-stu-id="6eca5-132">Function</span></span> |
| ------------------ | -------- |
| `Assert.AreEqual`  | <span data-ttu-id="6eca5-133">验证两个值或对象是否相等。</span><span class="sxs-lookup"><span data-stu-id="6eca5-133">Verifies that two values or objects are equal.</span></span> <span data-ttu-id="6eca5-134">如果值或对象不相等，则断言失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-134">The assert fails if the values or objects aren't equal.</span></span> |
| `Assert.AreSame`   | <span data-ttu-id="6eca5-135">验证两个对象变量引用的是否是同一个对象。</span><span class="sxs-lookup"><span data-stu-id="6eca5-135">Verifies that two object variables refer to the same object.</span></span> <span data-ttu-id="6eca5-136">如果这些变量引用不同的对象，则断言失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-136">The assert fails if the variables refer to different objects.</span></span> |
| `Assert.IsFalse`   | <span data-ttu-id="6eca5-137">验证条件是否为 `false`。</span><span class="sxs-lookup"><span data-stu-id="6eca5-137">Verifies that a condition is `false`.</span></span> <span data-ttu-id="6eca5-138">如果条件为 `true`，则断言失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-138">The assert fails if the condition is `true`.</span></span> |
| `Assert.IsNotNull` | <span data-ttu-id="6eca5-139">验证对象是否不为 `null`。</span><span class="sxs-lookup"><span data-stu-id="6eca5-139">Verifies that an object isn't `null`.</span></span> <span data-ttu-id="6eca5-140">如果对象为 `null`，则断言失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-140">The assert fails if the object is `null`.</span></span> |

<span data-ttu-id="6eca5-141">还可以在测试方法中使用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.ThrowsException%2A?displayProperty=nameWithType> 方法来指示它应引发的异常的类型。</span><span class="sxs-lookup"><span data-stu-id="6eca5-141">You can also use the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.ThrowsException%2A?displayProperty=nameWithType> method in a test method to indicate the type of exception it's expected to throw.</span></span> <span data-ttu-id="6eca5-142">如果未引发指定异常，则测试不通过。</span><span class="sxs-lookup"><span data-stu-id="6eca5-142">The test fails if the specified exception isn't thrown.</span></span>

<span data-ttu-id="6eca5-143">测试 `StringLibrary.StartsWithUpper` 方法时，需要提供许多以大写字符开头的字符串。</span><span class="sxs-lookup"><span data-stu-id="6eca5-143">In testing the `StringLibrary.StartsWithUpper` method, you want to provide a number of strings that begin with an uppercase character.</span></span> <span data-ttu-id="6eca5-144">在这种情况下，此方法应返回 `true`，以便可以调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsTrue%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="6eca5-144">You expect the method to return `true` in these cases, so you can call the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsTrue%2A?displayProperty=nameWithType> method.</span></span> <span data-ttu-id="6eca5-145">同样，需要提供许多以非大写字符开头的字符串。</span><span class="sxs-lookup"><span data-stu-id="6eca5-145">Similarly, you want to provide a number of strings that begin with something other than an uppercase character.</span></span> <span data-ttu-id="6eca5-146">在这种情况下，此方法应返回 `false`，以便可以调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsFalse%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="6eca5-146">You expect the method to return `false` in these cases, so you can call the <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsFalse%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="6eca5-147">由于库方法可以处理字符串，因此还需要确保它能够成功处理[空字符串 (`String.Empty`)](xref:System.String.Empty) 和 `null` 字符串。</span><span class="sxs-lookup"><span data-stu-id="6eca5-147">Since your library method handles strings, you also want to make sure that it successfully handles an [empty string (`String.Empty`)](xref:System.String.Empty) and a `null` string.</span></span> <span data-ttu-id="6eca5-148">空字符串不包含任何字符，且 <xref:System.String.Length> 为 0。</span><span class="sxs-lookup"><span data-stu-id="6eca5-148">An empty string is one that has no characters and whose <xref:System.String.Length> is 0.</span></span> <span data-ttu-id="6eca5-149">`null` 字符串是尚未初始化的字符串。</span><span class="sxs-lookup"><span data-stu-id="6eca5-149">A `null` string is one that hasn't been initialized.</span></span> <span data-ttu-id="6eca5-150">可以直接将 `StartsWithUpper` 作为静态方法进行调用，并向其传递一个 <xref:System.String> 自变量。</span><span class="sxs-lookup"><span data-stu-id="6eca5-150">You can call `StartsWithUpper` directly as a static method and pass a single <xref:System.String> argument.</span></span> <span data-ttu-id="6eca5-151">或者，可以对分配给 `null` 的 `string` 变量将 `StartsWithUpper` 作为扩展方法进行调用。</span><span class="sxs-lookup"><span data-stu-id="6eca5-151">Or you can call `StartsWithUpper` as an extension method on a `string` variable assigned to `null`.</span></span>

<span data-ttu-id="6eca5-152">将定义三个方法，每个方法都会对字符串数组中的各个元素调用它的 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 方法。</span><span class="sxs-lookup"><span data-stu-id="6eca5-152">You'll define three methods, each of which calls an <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> method for each element in a string array.</span></span> <span data-ttu-id="6eca5-153">你将调用方法重载，以便指定在测试失败时要显示的错误消息。</span><span class="sxs-lookup"><span data-stu-id="6eca5-153">You'll call a method overload that lets you specify an error message to be displayed in case of test failure.</span></span> <span data-ttu-id="6eca5-154">消息标识导致失败的字符串。</span><span class="sxs-lookup"><span data-stu-id="6eca5-154">The message identifies the string that caused the failure.</span></span>

<span data-ttu-id="6eca5-155">创建测试方法：</span><span class="sxs-lookup"><span data-stu-id="6eca5-155">To create the test methods:</span></span>

1. <span data-ttu-id="6eca5-156">打开 StringLibraryTest/UnitTest1.cs 并将所有代码替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="6eca5-156">Open *StringLibraryTest/UnitTest1.cs* and replace all of the code with the following code.</span></span>

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibraryTest/UnitTest1.cs":::

   <span data-ttu-id="6eca5-157">`TestStartsWithUpper` 方法中的大写字符的测试包括希腊文大写字母 alpha (U+0391) 和西里尔文大写字母 EM (U+041C)。</span><span class="sxs-lookup"><span data-stu-id="6eca5-157">The test of uppercase characters in the `TestStartsWithUpper` method includes the Greek capital letter alpha (U+0391) and the Cyrillic capital letter EM (U+041C).</span></span> <span data-ttu-id="6eca5-158">`TestDoesNotStartWithUpper` 方法中的小写字符的测试包括希腊文小写字母 alpha (U+03B1) 和西里尔文小写字母 Ghe (U+0433)。</span><span class="sxs-lookup"><span data-stu-id="6eca5-158">The test of lowercase characters in the `TestDoesNotStartWithUpper` method includes the Greek small letter alpha (U+03B1) and the Cyrillic small letter Ghe (U+0433).</span></span>

1. <span data-ttu-id="6eca5-159">保存更改。</span><span class="sxs-lookup"><span data-stu-id="6eca5-159">Save your changes.</span></span>

1. <span data-ttu-id="6eca5-160">运行测试：</span><span class="sxs-lookup"><span data-stu-id="6eca5-160">Run the tests:</span></span>

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   <span data-ttu-id="6eca5-161">终端输出显示所有测试都已通过。</span><span class="sxs-lookup"><span data-stu-id="6eca5-161">The terminal output shows that all tests passed.</span></span>

   ```output
   Starting test execution, please wait...
   A total of 1 test files matched the specified pattern.

   Passed!  - Failed:     0, Passed:     3, Skipped:     0, Total:     3, Duration: 3 ms - StringLibraryTest.dll (net5.0)
   ```

## <a name="handle-test-failures"></a><span data-ttu-id="6eca5-162">处理测试失败</span><span class="sxs-lookup"><span data-stu-id="6eca5-162">Handle test failures</span></span>

<span data-ttu-id="6eca5-163">如果进行的是测试驱动开发 (TDD)，请先编写测试，然后测试会在第一次运行时失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-163">If you're doing test-driven development (TDD), you write tests first and they fail the first time you run them.</span></span> <span data-ttu-id="6eca5-164">接着将可以使测试成功的代码添加到应用。</span><span class="sxs-lookup"><span data-stu-id="6eca5-164">Then you add code to the app that makes the test succeed.</span></span> <span data-ttu-id="6eca5-165">在本教程中，先编写了测试要验证的应用代码然后才创建测试，所以没有看到测试失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-165">For this tutorial, you created the test after writing the app code that it validates, so you haven't seen the test fail.</span></span> <span data-ttu-id="6eca5-166">若要验证测试是否在预期失败时失败，请在测试输入中添加无效值。</span><span class="sxs-lookup"><span data-stu-id="6eca5-166">To validate that a test fails when you expect it to fail, add an invalid value to the test input.</span></span>

1. <span data-ttu-id="6eca5-167">通过修改 `TestDoesNotStartWithUpper` 方法中的 `words` 数组来包含字符串“Error”。</span><span class="sxs-lookup"><span data-stu-id="6eca5-167">Modify the `words` array in the `TestDoesNotStartWithUpper` method to include the string "Error".</span></span>

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

1. <span data-ttu-id="6eca5-168">运行测试：</span><span class="sxs-lookup"><span data-stu-id="6eca5-168">Run the tests:</span></span>

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   <span data-ttu-id="6eca5-169">终端输出显示一个测试失败，并提供关于失败测试的错误消息：“Assert.IsFalse 失败。</span><span class="sxs-lookup"><span data-stu-id="6eca5-169">The terminal output shows that one test fails, and it provides an error message for the failed test: "Assert.IsFalse failed.</span></span> <span data-ttu-id="6eca5-170">“Error”应返回 false；实际返回 True”。</span><span class="sxs-lookup"><span data-stu-id="6eca5-170">Expected for 'Error': false; actual: True".</span></span> <span data-ttu-id="6eca5-171">由于此次失败，数组中“Error”之后的所有字符串都未进行测试。</span><span class="sxs-lookup"><span data-stu-id="6eca5-171">Because of the failure, no strings in the array after "Error" were tested.</span></span>

   ```output
   Starting test execution, please wait...
   A total of 1 test files matched the specified pattern.
     Failed TestDoesNotStartWithUpper [28 ms]
     Error Message:
      Assert.IsFalse failed. Expected for 'Error': false; Actual: True
     Stack Trace:
        at StringLibraryTest.UnitTest1.TestDoesNotStartWithUpper() in C:\ClassLibraryProjects\StringLibraryTest\UnitTest1.cs:line 33

   Failed!  - Failed:     1, Passed:     2, Skipped:     0, Total:     3, Duration: 31 ms - StringLibraryTest.dll (net5.0)
   ```

1. <span data-ttu-id="6eca5-172">删除在步骤 1 中添加的字符串“Error”。</span><span class="sxs-lookup"><span data-stu-id="6eca5-172">Remove the string "Error" that you added in step 1.</span></span> <span data-ttu-id="6eca5-173">重新运行测试，测试将通过。</span><span class="sxs-lookup"><span data-stu-id="6eca5-173">Rerun the test and the tests pass.</span></span>

## <a name="test-the-release-version-of-the-library"></a><span data-ttu-id="6eca5-174">测试库的发行版本</span><span class="sxs-lookup"><span data-stu-id="6eca5-174">Test the Release version of the library</span></span>

<span data-ttu-id="6eca5-175">至此，在运行库的调试版本时，测试已全部通过，接下来应对库的发行版本再运行一次这些测试。</span><span class="sxs-lookup"><span data-stu-id="6eca5-175">Now that the tests have all passed when running the Debug build of the library, run the tests an additional time against the Release build of the library.</span></span> <span data-ttu-id="6eca5-176">许多因素（包括编译器优化）有时可能会导致调试版本和发行版本出现行为差异。</span><span class="sxs-lookup"><span data-stu-id="6eca5-176">A number of factors, including compiler optimizations, can sometimes produce different behavior between Debug and Release builds.</span></span>

1. <span data-ttu-id="6eca5-177">使用版本生成配置运行测试：</span><span class="sxs-lookup"><span data-stu-id="6eca5-177">Run the tests with the Release build configuration:</span></span>

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj --configuration Release
   ```

   <span data-ttu-id="6eca5-178">测试通过。</span><span class="sxs-lookup"><span data-stu-id="6eca5-178">The tests pass.</span></span>

## <a name="debug-tests"></a><span data-ttu-id="6eca5-179">调试测试</span><span class="sxs-lookup"><span data-stu-id="6eca5-179">Debug tests</span></span>

<span data-ttu-id="6eca5-180">如果使用 Visual Studio 作为 IDE，则可以使用与[使用 Visual Studio Code 调试 .NET Core 控制台应用程序](debugging-with-visual-studio-code.md)中所示的相同过程，来通过使用单元测试项目调试代码。</span><span class="sxs-lookup"><span data-stu-id="6eca5-180">If you're using Visual Studio Code as your IDE, you can use the same process shown in [Debug a .NET console application using Visual Studio Code](debugging-with-visual-studio-code.md) to debug code using your unit test project.</span></span> <span data-ttu-id="6eca5-181">打开 StringLibraryTest/UnitTest1.cs，然后在第 7 行和第 8 行之间选择“运行所有测试”，而不是启动 ShowCase 应用项目。</span><span class="sxs-lookup"><span data-stu-id="6eca5-181">Instead of starting the *ShowCase* app project, open *StringLibraryTest/UnitTest1.cs*, and select **Run All Tests** between lines 7 and 8.</span></span> <span data-ttu-id="6eca5-182">如果找不到该位置，请按下 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> 打开命令面板，然后输入“重载窗口”。</span><span class="sxs-lookup"><span data-stu-id="6eca5-182">If you're unable to find it, press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> to open the command palette and enter **Reload Window**.</span></span>

<span data-ttu-id="6eca5-183">Visual Studio Code 启动附有调试器的测试项目。</span><span class="sxs-lookup"><span data-stu-id="6eca5-183">Visual Studio Code starts the test project with the debugger attached.</span></span> <span data-ttu-id="6eca5-184">执行将在添加到测试项目的任何断点或基础库代码处停止。</span><span class="sxs-lookup"><span data-stu-id="6eca5-184">Execution will stop at any breakpoint you've added to the test project or the underlying library code.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6eca5-185">其他资源</span><span class="sxs-lookup"><span data-stu-id="6eca5-185">Additional resources</span></span>

* [<span data-ttu-id="6eca5-186">.NET 中的单元测试</span><span class="sxs-lookup"><span data-stu-id="6eca5-186">Unit testing in .NET</span></span>](../testing/index.md)

## <a name="next-steps"></a><span data-ttu-id="6eca5-187">后续步骤</span><span class="sxs-lookup"><span data-stu-id="6eca5-187">Next steps</span></span>

<span data-ttu-id="6eca5-188">在本教程中，你对类库进行了单元测试。</span><span class="sxs-lookup"><span data-stu-id="6eca5-188">In this tutorial, you unit tested a class library.</span></span> <span data-ttu-id="6eca5-189">你可以将库作为包发布到 [NuGet](https://nuget.org)，使其可供其他人使用。</span><span class="sxs-lookup"><span data-stu-id="6eca5-189">You can make the library available to others by publishing it to [NuGet](https://nuget.org) as a package.</span></span> <span data-ttu-id="6eca5-190">若要了解如何操作，请遵循 NuGet 教程：</span><span class="sxs-lookup"><span data-stu-id="6eca5-190">To learn how, follow a NuGet tutorial:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6eca5-191">使用 dotnet CLI 创建和发布包</span><span class="sxs-lookup"><span data-stu-id="6eca5-191">Create and publish a package using the dotnet CLI</span></span>](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

<span data-ttu-id="6eca5-192">如果将库作为 NuGet 包发布，其他人可以安装并使用它。</span><span class="sxs-lookup"><span data-stu-id="6eca5-192">If you publish a library as a NuGet package, others can install and use it.</span></span> <span data-ttu-id="6eca5-193">若要了解如何操作，请遵循 NuGet 教程：</span><span class="sxs-lookup"><span data-stu-id="6eca5-193">To learn how, follow a NuGet tutorial:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6eca5-194">使用 dotnet CLI 安装并使用包</span><span class="sxs-lookup"><span data-stu-id="6eca5-194">Install and use a package using the dotnet CLI</span></span>](/nuget/quickstart/install-and-use-a-package-using-the-dotnet-cli)

<span data-ttu-id="6eca5-195">库并非必须作为包进行分发。</span><span class="sxs-lookup"><span data-stu-id="6eca5-195">A library doesn't have to be distributed as a package.</span></span> <span data-ttu-id="6eca5-196">它还可与使用它的控制台应用捆绑在一起。</span><span class="sxs-lookup"><span data-stu-id="6eca5-196">It can be bundled with a console app that uses it.</span></span> <span data-ttu-id="6eca5-197">若要了解如何发布控制台应用，请参阅本系列中前面的教程：</span><span class="sxs-lookup"><span data-stu-id="6eca5-197">To learn how to publish a console app, see the earlier tutorial in this series:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6eca5-198">使用 Visual Studio Code 发布 .NET 控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="6eca5-198">Publish a .NET console application using Visual Studio Code</span></span>](publishing-with-visual-studio-code.md)
