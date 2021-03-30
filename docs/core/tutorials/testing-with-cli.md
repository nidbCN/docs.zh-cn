---
title: 使用 .NET CLI 组织和测试项目
description: 本教程介绍如何通过命令行组织和测试 .NET 项目。
author: cartermp
ms.date: 09/10/2018
ms.openlocfilehash: c668cff48a2418cc1bc34aef78ea26c863d9de6a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872673"
---
# <a name="organizing-and-testing-projects-with-the-net-cli"></a><span data-ttu-id="377a7-103">使用 .NET CLI 组织和测试项目</span><span class="sxs-lookup"><span data-stu-id="377a7-103">Organizing and testing projects with the .NET CLI</span></span>

<span data-ttu-id="377a7-104">本教程遵循[教程：使用 Visual Studio Code 通过 .NET 创建控制台应用程序](with-visual-studio-code.md)，不仅介绍如何创建简单的控制台应用，还将介绍如何开发高级且结构完善的应用程序。</span><span class="sxs-lookup"><span data-stu-id="377a7-104">This tutorial follows [Tutorial: Create a console application with .NET using Visual Studio Code](with-visual-studio-code.md), taking you beyond the creation of a simple console app to develop advanced and well-organized applications.</span></span> <span data-ttu-id="377a7-105">在演示如何使用文件夹来组织代码后，本教程还将说明如何使用 [xUnit](https://xunit.net/) 测试框架扩展控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="377a7-105">After showing you how to use folders to organize your code, this tutorial shows you how to extend a console application with the [xUnit](https://xunit.net/) testing framework.</span></span>

## <a name="using-folders-to-organize-code"></a><span data-ttu-id="377a7-106">使用文件夹组织代码</span><span class="sxs-lookup"><span data-stu-id="377a7-106">Using folders to organize code</span></span>

<span data-ttu-id="377a7-107">如要要在控制台应用中引入新类型，可向该应用添加包含该类型的文件。</span><span class="sxs-lookup"><span data-stu-id="377a7-107">If you want to introduce new types into a console app, you can do so by adding files containing the types to the app.</span></span> <span data-ttu-id="377a7-108">例如，如果向项目添加包含 `AccountInformation` 和 `MonthlyReportRecords` 类型的文件，则项目文件结构是平面的，且易于导航：</span><span class="sxs-lookup"><span data-stu-id="377a7-108">For example if you add files containing `AccountInformation` and `MonthlyReportRecords` types to your project, the project file structure is flat and easy to navigate:</span></span>

```
/MyProject
|__AccountInformation.cs
|__MonthlyReportRecords.cs
|__MyProject.csproj
|__Program.cs
```

<span data-ttu-id="377a7-109">但是，仅在项目规模相对较小时，此方法才适用。</span><span class="sxs-lookup"><span data-stu-id="377a7-109">However, this only works well when the size of your project is relatively small.</span></span> <span data-ttu-id="377a7-110">你能否想象在项目中添加 20 个类型时会发生什么？</span><span class="sxs-lookup"><span data-stu-id="377a7-110">Can you imagine what will happen if you add 20 types to the project?</span></span> <span data-ttu-id="377a7-111">项目的根目录中会散落许多文件，这样的项目必然会难以导航和维护。</span><span class="sxs-lookup"><span data-stu-id="377a7-111">The project definitely wouldn't be easy to navigate and maintain with that many files littering the project's root directory.</span></span>

<span data-ttu-id="377a7-112">要组织项目，请创建一个名为 Models 新文件夹，将其用于保存类型文件。</span><span class="sxs-lookup"><span data-stu-id="377a7-112">To organize the project, create a new folder and name it *Models* to hold the type files.</span></span> <span data-ttu-id="377a7-113">将类型文件放入 Models 文件夹中：</span><span class="sxs-lookup"><span data-stu-id="377a7-113">Place the type files into the *Models* folder:</span></span>

```
/MyProject
|__/Models
   |__AccountInformation.cs
   |__MonthlyReportRecords.cs
|__MyProject.csproj
|__Program.cs
```

<span data-ttu-id="377a7-114">按逻辑将文件分组到文件夹的项目易于导航和维护。</span><span class="sxs-lookup"><span data-stu-id="377a7-114">Projects that logically group files into folders are easy to navigate and maintain.</span></span> <span data-ttu-id="377a7-115">在下一节中，将创建一个更复杂的示例，它包含文件夹和单元测试。</span><span class="sxs-lookup"><span data-stu-id="377a7-115">In the next section, you create a more complex sample with folders and unit testing.</span></span>

## <a name="organizing-and-testing-using-the-newtypes-pets-sample"></a><span data-ttu-id="377a7-116">使用 NewTypes Pets 示例进行组织和测试</span><span class="sxs-lookup"><span data-stu-id="377a7-116">Organizing and testing using the NewTypes Pets Sample</span></span>

### <a name="prerequisites"></a><span data-ttu-id="377a7-117">必备知识</span><span class="sxs-lookup"><span data-stu-id="377a7-117">Prerequisites</span></span>

* <span data-ttu-id="377a7-118">[.NET 5.0 SDK](https://dotnet.microsoft.com/download) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="377a7-118">[.NET 5.0 SDK](https://dotnet.microsoft.com/download) or a later version.</span></span>

### <a name="building-the-sample"></a><span data-ttu-id="377a7-119">生成示例</span><span class="sxs-lookup"><span data-stu-id="377a7-119">Building the sample</span></span>

<span data-ttu-id="377a7-120">对于下列步骤，可使用 [NewTypes Pets 示例](https://github.com/dotnet/samples/tree/main/core/console-apps/NewTypesMsBuild)进行相关操作，也可以创建自己的文件与文件夹进行操作。</span><span class="sxs-lookup"><span data-stu-id="377a7-120">For the following steps, you can either follow along using the [NewTypes Pets Sample](https://github.com/dotnet/samples/tree/main/core/console-apps/NewTypesMsBuild) or create your own files and folders.</span></span> <span data-ttu-id="377a7-121">各类型按逻辑组织为文件夹结构，允许日后加入更多类型，测试也按逻辑放置在文件夹中，允许日后加入更多测试。</span><span class="sxs-lookup"><span data-stu-id="377a7-121">The types are logically organized into a folder structure that permits the addition of more types later, and tests are also logically placed in folders permitting the addition of more tests later.</span></span>

<span data-ttu-id="377a7-122">此示例包含两种类型 `Dog` 和 `Cat`，并使它们实现一个公共接口 `IPet`。</span><span class="sxs-lookup"><span data-stu-id="377a7-122">The sample contains two types, `Dog` and `Cat`, and has them implement a common interface, `IPet`.</span></span> <span data-ttu-id="377a7-123">对于 `NewTypes` 项目，目标是将与宠物相关的类型组织到 Pets 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="377a7-123">For the `NewTypes` project, your goal is to organize the pet-related types into a *Pets* folder.</span></span> <span data-ttu-id="377a7-124">如果之后添加了另一组类型（例如 WildAnimals），则将其与 Pets 文件夹一同放在 NewTypes 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="377a7-124">If another set of types is added later, *WildAnimals* for example, they're placed in the *NewTypes* folder alongside the *Pets* folder.</span></span> <span data-ttu-id="377a7-125">WildAnimals 文件夹可包含不属于宠物的动物类型，如 `Squirrel` 和 `Rabbit` 类型。</span><span class="sxs-lookup"><span data-stu-id="377a7-125">The *WildAnimals* folder may contain types for animals that aren't pets, such as `Squirrel` and `Rabbit` types.</span></span> <span data-ttu-id="377a7-126">按照这种方式添加类型，不会破坏项目的良好组织。</span><span class="sxs-lookup"><span data-stu-id="377a7-126">In this way as types are added, the project remains well organized.</span></span>

<span data-ttu-id="377a7-127">创建以下文件夹结构，并指明文件内容：</span><span class="sxs-lookup"><span data-stu-id="377a7-127">Create the following folder structure with file content indicated:</span></span>

```
/NewTypes
|__/src
   |__/NewTypes
      |__/Pets
         |__Dog.cs
         |__Cat.cs
         |__IPet.cs
      |__Program.cs
      |__NewTypes.csproj
```

<span data-ttu-id="377a7-128">IPet.cs:</span><span class="sxs-lookup"><span data-stu-id="377a7-128">*IPet.cs*:</span></span>

[!code-csharp[IPet interface](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Pets/IPet.cs)]

<span data-ttu-id="377a7-129">Dog.cs:</span><span class="sxs-lookup"><span data-stu-id="377a7-129">*Dog.cs*:</span></span>

[!code-csharp[Dog class](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Pets/Dog.cs)]

<span data-ttu-id="377a7-130">Cat.cs:</span><span class="sxs-lookup"><span data-stu-id="377a7-130">*Cat.cs*:</span></span>

[!code-csharp[Cat class](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Pets/Cat.cs)]

<span data-ttu-id="377a7-131">Program.cs:</span><span class="sxs-lookup"><span data-stu-id="377a7-131">*Program.cs*:</span></span>

[!code-csharp[Main](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/Program.cs)]

<span data-ttu-id="377a7-132">NewTypes.csproj:</span><span class="sxs-lookup"><span data-stu-id="377a7-132">*NewTypes.csproj*:</span></span>

[!code-xml[NewTypes csproj](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/src/NewTypes/NewTypes.csproj)]

<span data-ttu-id="377a7-133">请执行以下命令：</span><span class="sxs-lookup"><span data-stu-id="377a7-133">Execute the following command:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="377a7-134">获得以下输出：</span><span class="sxs-lookup"><span data-stu-id="377a7-134">Obtain the following output:</span></span>

```console
Woof!
Meow!
```

<span data-ttu-id="377a7-135">可选练习：可通过扩展此项目来添加新的宠物类型，例如 `Bird`。</span><span class="sxs-lookup"><span data-stu-id="377a7-135">Optional exercise: You can add a new pet type, such as a `Bird`, by extending this project.</span></span> <span data-ttu-id="377a7-136">使鸟的 `TalkToOwner` 方法向所有者发出 `Tweet!`。</span><span class="sxs-lookup"><span data-stu-id="377a7-136">Make the bird's `TalkToOwner` method give a `Tweet!` to the owner.</span></span> <span data-ttu-id="377a7-137">再次运行应用。</span><span class="sxs-lookup"><span data-stu-id="377a7-137">Run the app again.</span></span> <span data-ttu-id="377a7-138">输出将包含 `Tweet!`</span><span class="sxs-lookup"><span data-stu-id="377a7-138">The output will include `Tweet!`</span></span>

### <a name="testing-the-sample"></a><span data-ttu-id="377a7-139">测试示例</span><span class="sxs-lookup"><span data-stu-id="377a7-139">Testing the sample</span></span>

<span data-ttu-id="377a7-140">`NewTypes` 项目已准备就绪，与宠物相关的类型均置于一个文件夹中，因此具有良好的组织。</span><span class="sxs-lookup"><span data-stu-id="377a7-140">The `NewTypes` project is in place, and you've organized it by keeping the pets-related types in a folder.</span></span> <span data-ttu-id="377a7-141">接下来，创建测试项目，并使用 [xUnit](https://xunit.net/) 测试框架开始编写测试。</span><span class="sxs-lookup"><span data-stu-id="377a7-141">Next, create your test project and start writing tests with the [xUnit](https://xunit.net/) test framework.</span></span> <span data-ttu-id="377a7-142">使用单元测试，可自动检查宠物类型的行为，确认其正常运行。</span><span class="sxs-lookup"><span data-stu-id="377a7-142">Unit testing allows you to automatically check the behavior of your pet types to confirm that they're operating properly.</span></span>

<span data-ttu-id="377a7-143">导航回 src 文件夹并创建“test”文件夹，后者包含 NewTypesTests 文件夹  。</span><span class="sxs-lookup"><span data-stu-id="377a7-143">Navigate back to the *src* folder and create a *test* folder with a *NewTypesTests* folder within it.</span></span> <span data-ttu-id="377a7-144">在 NewTypesTests 文件夹的命令提示符中，执行 `dotnet new xunit`。</span><span class="sxs-lookup"><span data-stu-id="377a7-144">At a command prompt from the *NewTypesTests* folder, execute `dotnet new xunit`.</span></span> <span data-ttu-id="377a7-145">这将生成两个文件：NewTypesTests.csproj 和 UnitTest1.cs 。</span><span class="sxs-lookup"><span data-stu-id="377a7-145">This produces two files: *NewTypesTests.csproj* and *UnitTest1.cs*.</span></span>

<span data-ttu-id="377a7-146">测试项目当前无法测试 `NewTypes` 中的类型，并且需要对 `NewTypes` 项目的项目引用。</span><span class="sxs-lookup"><span data-stu-id="377a7-146">The test project cannot currently test the types in `NewTypes` and requires a project reference to the `NewTypes` project.</span></span> <span data-ttu-id="377a7-147">要添加项目引用，请使用 [`dotnet add reference`](../tools/dotnet-add-reference.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="377a7-147">To add a project reference, use the [`dotnet add reference`](../tools/dotnet-add-reference.md) command:</span></span>

```dotnetcli
dotnet add reference ../../src/NewTypes/NewTypes.csproj
```

<span data-ttu-id="377a7-148">或者，可以选择向 NewTypesTests.csproj 文件添加 `<ItemGroup>` 节点，手动添加项目引用：</span><span class="sxs-lookup"><span data-stu-id="377a7-148">Or, you also have the option of manually adding the project reference by adding an `<ItemGroup>` node to the *NewTypesTests.csproj* file:</span></span>

```xml
<ItemGroup>
  <ProjectReference Include="../../src/NewTypes/NewTypes.csproj" />
</ItemGroup>
```

<span data-ttu-id="377a7-149">NewTypesTests.csproj:</span><span class="sxs-lookup"><span data-stu-id="377a7-149">*NewTypesTests.csproj*:</span></span>

[!code-xml[NewTypesTests csproj](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/test/NewTypesTests/NewTypesTests.csproj)]

<span data-ttu-id="377a7-150">NewTypesTests.csproj 文件包含下列内容：</span><span class="sxs-lookup"><span data-stu-id="377a7-150">The *NewTypesTests.csproj* file contains the following:</span></span>

* <span data-ttu-id="377a7-151">对 .NET 测试基础结构 `Microsoft.NET.Test.Sdk` 的包引用</span><span class="sxs-lookup"><span data-stu-id="377a7-151">Package reference to `Microsoft.NET.Test.Sdk`, the .NET testing infrastructure</span></span>
* <span data-ttu-id="377a7-152">对 xUnit 测试框架 `xunit` 的包引用</span><span class="sxs-lookup"><span data-stu-id="377a7-152">Package reference to `xunit`, the xUnit testing framework</span></span>
* <span data-ttu-id="377a7-153">对测试运行程序 `xunit.runner.visualstudio` 的包引用</span><span class="sxs-lookup"><span data-stu-id="377a7-153">Package reference to `xunit.runner.visualstudio`, the test runner</span></span>
* <span data-ttu-id="377a7-154">对要测试的代码 `NewTypes` 的项目引用</span><span class="sxs-lookup"><span data-stu-id="377a7-154">Project reference to `NewTypes`, the code to test</span></span>

<span data-ttu-id="377a7-155">将 UnitTest1.cs 的名称更改为 PetTests.cs，并将文件中的代码替换为下列内容：</span><span class="sxs-lookup"><span data-stu-id="377a7-155">Change the name of *UnitTest1.cs* to *PetTests.cs* and replace the code in the file with the following:</span></span>

```csharp
using System;
using Xunit;
using Pets;

public class PetTests
{
    [Fact]
    public void DogTalkToOwnerReturnsWoof()
    {
        string expected = "Woof!";
        string actual = new Dog().TalkToOwner();

        Assert.NotEqual(expected, actual);
    }

    [Fact]
    public void CatTalkToOwnerReturnsMeow()
    {
        string expected = "Meow!";
        string actual = new Cat().TalkToOwner();

        Assert.NotEqual(expected, actual);
    }
}
```

<span data-ttu-id="377a7-156">可选练习：如果先前向所有者添加了生成 `Tweet!` 的 `Bird` 类型，请向 PetTests.cs 文件 `BirdTalkToOwnerReturnsTweet` 添加测试方法，以检查对于 `Bird` 类型，`TalkToOwner` 方法是否正常工作。</span><span class="sxs-lookup"><span data-stu-id="377a7-156">Optional exercise: If you added a `Bird` type earlier that yields a `Tweet!` to the owner, add a test method to the *PetTests.cs* file, `BirdTalkToOwnerReturnsTweet`, to check that the `TalkToOwner` method works correctly for the `Bird` type.</span></span>

> [!NOTE]
> <span data-ttu-id="377a7-157">尽管期望 `expected` 和 `actual` 值相等，但使用 `Assert.NotEqual` 检查的初始断言表明这些值并不相等。</span><span class="sxs-lookup"><span data-stu-id="377a7-157">Although you expect that the `expected` and `actual` values are equal, an initial assertion with the `Assert.NotEqual` check specifies that these values are *not equal*.</span></span> <span data-ttu-id="377a7-158">务必最初创建一个失败的测试，以检查测试的逻辑是否正确。</span><span class="sxs-lookup"><span data-stu-id="377a7-158">Always initially create a test to fail in order to check the logic of the test.</span></span> <span data-ttu-id="377a7-159">确认测试失败后，调整断言，使测试通过。</span><span class="sxs-lookup"><span data-stu-id="377a7-159">After you confirm that the test fails, adjust the assertion to allow the test to pass.</span></span>

<span data-ttu-id="377a7-160">下面演示了完整的项目结构：</span><span class="sxs-lookup"><span data-stu-id="377a7-160">The following shows the complete project structure:</span></span>

```
/NewTypes
|__/src
   |__/NewTypes
      |__/Pets
         |__Dog.cs
         |__Cat.cs
         |__IPet.cs
      |__Program.cs
      |__NewTypes.csproj
|__/test
   |__NewTypesTests
      |__PetTests.cs
      |__NewTypesTests.csproj
```

<span data-ttu-id="377a7-161">在 test/NewTypesTests 目录中开始。</span><span class="sxs-lookup"><span data-stu-id="377a7-161">Start in the *test/NewTypesTests* directory.</span></span> <span data-ttu-id="377a7-162">使用 [`dotnet test`](../tools/dotnet-test.md) 命令运行测试。</span><span class="sxs-lookup"><span data-stu-id="377a7-162">Run the tests with the [`dotnet test`](../tools/dotnet-test.md) command.</span></span> <span data-ttu-id="377a7-163">此命令启动项目文件中指定的测试运行程序。</span><span class="sxs-lookup"><span data-stu-id="377a7-163">This command starts the test runner specified in the project file.</span></span>

<span data-ttu-id="377a7-164">测试按预期失败，控制台显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="377a7-164">As expected, testing fails, and the console displays the following output:</span></span>

```output
Test run for C:\Source\dotnet\docs\samples\snippets\core\tutorials\testing-with-cli\csharp\test\NewTypesTests\bin\Debug\net5.0\NewTypesTests.dll (.NETCoreApp,Version=v5.0)
Microsoft (R) Test Execution Command Line Tool Version 16.8.1
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
A total of 1 test files matched the specified pattern.
[xUnit.net 00:00:00.50]     PetTests.DogTalkToOwnerReturnsWoof [FAIL]
  Failed PetTests.DogTalkToOwnerReturnsWoof [6 ms]
  Error Message:
   Assert.NotEqual() Failure
Expected: Not "Woof!"
Actual:   "Woof!"
  Stack Trace:
     at PetTests.DogTalkToOwnerReturnsWoof() in C:\Source\dotnet\docs\samples\snippets\core\tutorials\testing-with-cli\csharp\test\NewTypesTests\PetTests.cs:line 13

Failed!  - Failed:     1, Passed:     1, Skipped:     0, Total:     2, Duration: 8 ms - NewTypesTests.dll (net5.0)
```

<span data-ttu-id="377a7-165">将测试的断言从 `Assert.NotEqual` 更改为 `Assert.Equal`：</span><span class="sxs-lookup"><span data-stu-id="377a7-165">Change the assertions of your tests from `Assert.NotEqual` to `Assert.Equal`:</span></span>

[!code-csharp[PetTests class](../../../samples/snippets/core/tutorials/testing-with-cli/csharp/test/NewTypesTests/PetTests.cs)]

<span data-ttu-id="377a7-166">使用 `dotnet test` 命令重新运行测试，并获得以下输出：</span><span class="sxs-lookup"><span data-stu-id="377a7-166">Re-run the tests with the `dotnet test` command and obtain the following output:</span></span>

```output
Test run for C:\Source\dotnet\docs\samples\snippets\core\tutorials\testing-with-cli\csharp\test\NewTypesTests\bin\Debug\net5.0\NewTypesTests.dll (.NETCoreApp,Version=v5.0)
Microsoft (R) Test Execution Command Line Tool Version 16.8.1
Copyright (c) Microsoft Corporation.  All rights reserved.

Starting test execution, please wait...
A total of 1 test files matched the specified pattern.

Passed!  - Failed:     0, Passed:     2, Skipped:     0, Total:     2, Duration: 2 ms - NewTypesTests.dll (net5.0)
```

<span data-ttu-id="377a7-167">测试通过。</span><span class="sxs-lookup"><span data-stu-id="377a7-167">Testing passes.</span></span> <span data-ttu-id="377a7-168">在与所有者谈话时，宠物类型的方法返回正确的值。</span><span class="sxs-lookup"><span data-stu-id="377a7-168">The pet types' methods return the correct values when talking to the owner.</span></span>

<span data-ttu-id="377a7-169">你已了解使用 xUnit 来组织和测试项目的方法。</span><span class="sxs-lookup"><span data-stu-id="377a7-169">You've learned techniques for organizing and testing projects using xUnit.</span></span> <span data-ttu-id="377a7-170">继续使用这些方法，将它们应用于自己的项目中。</span><span class="sxs-lookup"><span data-stu-id="377a7-170">Go forward with these techniques applying them in your own projects.</span></span> <span data-ttu-id="377a7-171">祝你编码愉快！</span><span class="sxs-lookup"><span data-stu-id="377a7-171">*Happy coding!*</span></span>
