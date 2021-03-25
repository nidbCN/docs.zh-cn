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
# <a name="tutorial-test-a-net-class-library-using-visual-studio-code"></a>教程：使用 Visual Studio Code 测试 .NET 类库

本教程演示如何通过将测试项目添加到解决方案来自动执行单元测试。

## <a name="prerequisites"></a>先决条件

- 本教程适用于在[使用 Visual Studio Code 创建 .NET 类库](library-with-visual-studio-code.md)中创建的解决方案。

## <a name="create-a-unit-test-project"></a>创建单元测试项目

单元测试在开发和发布期间提供自动化的软件测试。 本教程中使用的测试框架是 MSTest。 [MSTest](https://github.com/Microsoft/testfx-docs) 是可供选择的三个测试框架之一。 其他两个是 [xUnit](https://xunit.net/) 和 [nUnit](https://nunit.org/)。

1. 启动 Visual Studio Code。

1. 打开在[使用 Visual Studio Code 创建 .NET 类库](library-with-visual-studio-code.md)中创建的 `ClassLibraryProjects` 解决方案。

1. 创建名为“StringLibraryTest”的单元测试项目。

   ```dotnetcli
   dotnet new mstest -o StringLibraryTest
   ```

   项目模板创建包含以下代码的 UnitTest1.cs 文件：

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

   单元测试模板创建的源代码负责执行以下操作：

   - 它会导入 <xref:Microsoft.VisualStudio.TestTools.UnitTesting?displayProperty=nameWithType> 命名空间，其中包含用于单元测试的类型。
   - 向 `UnitTest1` 类应用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 特性。
   - 它应用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 特性来定义 `TestMethod1`。

   使用 [[TestClass]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute) 标记的测试类中标记有 [[TestMethod]](xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute) 的所有测试方法都会在单元测试运行时自动执行。

1. 向解决方案添加测试项目。

   ```dotnetcli
   dotnet sln add StringLibraryTest/StringLibraryTest.csproj
   ```

## <a name="add-a-project-reference"></a>添加项目引用

对于要使用 `StringLibrary` 类的测试库，请在 `StringLibraryTest` 项目中添加对 `StringLibrary` 项目的引用。

1. 运行下面的命令：

   ```dotnetcli
   dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj
   ```

## <a name="add-and-run-unit-test-methods"></a>添加并运行单元测试方法

运行单元测试时，Visual Studio 执行使用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestClassAttribute> 特性标记的类中标记有 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.TestMethodAttribute> 特性的所有方法。 当第一次遇到测试不通过或测试方法中的所有测试均已成功通过时，测试方法终止。

最常见的测试调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 类的成员。 许多断言方法至少包含两个参数，其中一个是预期的测试结果，另一个是实际的测试结果。 下表显示了 `Assert` 类最常调用的一些方法：

| 断言方法     | 函数 |
| ------------------ | -------- |
| `Assert.AreEqual`  | 验证两个值或对象是否相等。 如果值或对象不相等，则断言失败。 |
| `Assert.AreSame`   | 验证两个对象变量引用的是否是同一个对象。 如果这些变量引用不同的对象，则断言失败。 |
| `Assert.IsFalse`   | 验证条件是否为 `false`。 如果条件为 `true`，则断言失败。 |
| `Assert.IsNotNull` | 验证对象是否不为 `null`。 如果对象为 `null`，则断言失败。 |

还可以在测试方法中使用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.ThrowsException%2A?displayProperty=nameWithType> 方法来指示它应引发的异常的类型。 如果未引发指定异常，则测试不通过。

测试 `StringLibrary.StartsWithUpper` 方法时，需要提供许多以大写字符开头的字符串。 在这种情况下，此方法应返回 `true`，以便可以调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsTrue%2A?displayProperty=nameWithType> 方法。 同样，需要提供许多以非大写字符开头的字符串。 在这种情况下，此方法应返回 `false`，以便可以调用 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert.IsFalse%2A?displayProperty=nameWithType> 方法。

由于库方法可以处理字符串，因此还需要确保它能够成功处理[空字符串 (`String.Empty`)](xref:System.String.Empty) 和 `null` 字符串。 空字符串不包含任何字符，且 <xref:System.String.Length> 为 0。 `null` 字符串是尚未初始化的字符串。 可以直接将 `StartsWithUpper` 作为静态方法进行调用，并向其传递一个 <xref:System.String> 自变量。 或者，可以对分配给 `null` 的 `string` 变量将 `StartsWithUpper` 作为扩展方法进行调用。

将定义三个方法，每个方法都会对字符串数组中的各个元素调用它的 <xref:Microsoft.VisualStudio.TestTools.UnitTesting.Assert> 方法。 你将调用方法重载，以便指定在测试失败时要显示的错误消息。 消息标识导致失败的字符串。

创建测试方法：

1. 打开 StringLibraryTest/UnitTest1.cs 并将所有代码替换为以下代码。

   :::code language="csharp" source="./snippets/library-with-visual-studio/csharp/StringLibraryTest/UnitTest1.cs":::

   `TestStartsWithUpper` 方法中的大写字符的测试包括希腊文大写字母 alpha (U+0391) 和西里尔文大写字母 EM (U+041C)。 `TestDoesNotStartWithUpper` 方法中的小写字符的测试包括希腊文小写字母 alpha (U+03B1) 和西里尔文小写字母 Ghe (U+0433)。

1. 保存更改。

1. 运行测试：

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   终端输出显示所有测试都已通过。

   ```output
   Starting test execution, please wait...
   A total of 1 test files matched the specified pattern.

   Passed!  - Failed:     0, Passed:     3, Skipped:     0, Total:     3, Duration: 3 ms - StringLibraryTest.dll (net5.0)
   ```

## <a name="handle-test-failures"></a>处理测试失败

如果进行的是测试驱动开发 (TDD)，请先编写测试，然后测试会在第一次运行时失败。 接着将可以使测试成功的代码添加到应用。 在本教程中，先编写了测试要验证的应用代码然后才创建测试，所以没有看到测试失败。 若要验证测试是否在预期失败时失败，请在测试输入中添加无效值。

1. 通过修改 `TestDoesNotStartWithUpper` 方法中的 `words` 数组来包含字符串“Error”。

   ```csharp
   string[] words = { "alphabet", "Error", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                      "1234", ".", ";", " " };
   ```

1. 运行测试：

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj
   ```

   终端输出显示一个测试失败，并提供关于失败测试的错误消息：“Assert.IsFalse 失败。 “Error”应返回 false；实际返回 True”。 由于此次失败，数组中“Error”之后的所有字符串都未进行测试。

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

1. 删除在步骤 1 中添加的字符串“Error”。 重新运行测试，测试将通过。

## <a name="test-the-release-version-of-the-library"></a>测试库的发行版本

至此，在运行库的调试版本时，测试已全部通过，接下来应对库的发行版本再运行一次这些测试。 许多因素（包括编译器优化）有时可能会导致调试版本和发行版本出现行为差异。

1. 使用版本生成配置运行测试：

   ```dotnetcli
   dotnet test StringLibraryTest/StringLibraryTest.csproj --configuration Release
   ```

   测试通过。

## <a name="debug-tests"></a>调试测试

如果使用 Visual Studio 作为 IDE，则可以使用与[使用 Visual Studio Code 调试 .NET Core 控制台应用程序](debugging-with-visual-studio-code.md)中所示的相同过程，来通过使用单元测试项目调试代码。 打开 StringLibraryTest/UnitTest1.cs，然后在第 7 行和第 8 行之间选择“运行所有测试”，而不是启动 ShowCase 应用项目。 如果找不到该位置，请按下 <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> 打开命令面板，然后输入“重载窗口”。

Visual Studio Code 启动附有调试器的测试项目。 执行将在添加到测试项目的任何断点或基础库代码处停止。

## <a name="additional-resources"></a>其他资源

* [.NET 中的单元测试](../testing/index.md)

## <a name="next-steps"></a>后续步骤

在本教程中，你对类库进行了单元测试。 你可以将库作为包发布到 [NuGet](https://nuget.org)，使其可供其他人使用。 若要了解如何操作，请遵循 NuGet 教程：

> [!div class="nextstepaction"]
> [使用 dotnet CLI 创建和发布包](/nuget/quickstart/create-and-publish-a-package-using-the-dotnet-cli)

如果将库作为 NuGet 包发布，其他人可以安装并使用它。 若要了解如何操作，请遵循 NuGet 教程：

> [!div class="nextstepaction"]
> [使用 dotnet CLI 安装并使用包](/nuget/quickstart/install-and-use-a-package-using-the-dotnet-cli)

库并非必须作为包进行分发。 它还可与使用它的控制台应用捆绑在一起。 若要了解如何发布控制台应用，请参阅本系列中前面的教程：

> [!div class="nextstepaction"]
> [使用 Visual Studio Code 发布 .NET 控制台应用程序](publishing-with-visual-studio-code.md)
