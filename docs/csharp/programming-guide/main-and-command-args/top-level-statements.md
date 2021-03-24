---
title: 顶级语句 - C# 编程指南
description: 了解 C# 9 及更高版本中的顶级语句。
ms.date: 03/08/2021
helpviewer_keywords:
- C# language, top-level statements
- C# language, Main method
ms.openlocfilehash: 69ff5fd606f5e12f55bd3e6dfc15fc7e64d8352b
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190435"
---
# <a name="top-level-statements-c-programming-guide"></a>顶级语句（C# 编程指南）

从 C# 9 开始，无需在控制台应用程序项目中显式包含 `Main` 方法。 相反，可以使用顶级语句功能最大程度地减少必须编写的代码。 在这种情况下，编译器将为应用程序生成类和 `Main` 方法入口点。

下面的 Program.cs 文件是 C# 9 中一个完整的 C# 程序：

:::code language="csharp" source="snippets/top-level-statements-1/Program.cs":::

借助顶级语句，可以为小实用程序（如 Azure Functions 和 GitHub Actions）编写简单的程序。 它们还使初次接触 C# 的程序员能够更轻松地开始学习和编写代码。

以下各节介绍了可对顶级语句执行和不能执行的操作的规则。

## <a name="only-one-top-level-file"></a>仅能有一个顶级文件

一个应用程序必须只能有一个入口点，因此一个项目只能有一个包含顶级语句的文件。 在项目中的多个文件中放置顶级语句会导致以下编译器错误：

> CS8802：只有一个编译单元可具有顶级语句。

一个项目可具有任意数量的其他源代码文件，这些文件不包含顶级语句。

## <a name="no-other-entry-points"></a>没有其他入口点

可以显式编写 `Main` 方法，但它不能作为入口点。 编译器将发出以下警告：

> CS7022：程序的入口点是全局代码；忽略“Main()”入口点。

在具有顶级语句的项目中，不能使用 [-main](../../language-reference/compiler-options/main-compiler-option.md) 编译器选项来选择入口点，即使该项目具有一个或多个 `Main` 方法。

## <a name="using-directives"></a>`using` 指令

如果包含 using 指令，则它们必须首先出现在文件中，如以下示例中所示：

:::code language="csharp" source="snippets/top-level-statements-1/Program.cs":::

## <a name="global-namespace"></a>全局命名空间

顶级语句隐式位于全局命名空间中。

## <a name="namespaces-and-type-definitions"></a>命名空间和类型定义

具有顶级语句的文件还可以包含命名空间和类型定义，但它们必须位于顶级语句之后。 例如：

:::code language="csharp" source="snippets/top-level-statements-2/Program.cs":::

## `args`

顶级语句可以引用 `args` 变量来访问输入的任何命令行参数。 `args` 变量永远不会为 null，但如果未提供任何命令行参数，则其 `Length` 将为零。 例如：

:::code language="csharp" source="snippets/top-level-statements-3/Program.cs":::

## `await`

可以通过使用 `await` 来调用异步方法。 例如：

:::code language="csharp" source="snippets/top-level-statements-4/Program.cs":::

## <a name="exit-code-for-the-process"></a>进程的退出代码

若要在应用程序结束时返回 `int` 值，请像在 `Main` 方法中返回 `int` 那样使用 `return` 语句。 例如：

:::code language="csharp" source="snippets/top-level-statements-5/Program.cs":::

## <a name="implicit-entry-point-method"></a>隐式入口点方法

编译器会生成一个方法，作为具有顶级语句的项目的程序入口点。 此方法的名称实际上并不是 `Main`，而是代码无法直接引用的实现细节。 方法的签名取决于顶级语句是包含 `await` 关键字还是 `return` 语句。 下表显示了方法签名的外观，为了方便起见，在表中使用了方法名称 `Main`。

| 顶级代码包含| 隐式 `Main` 签名                    |
|------------------------|----------------------------------------------|
| `await` 和 `return`   | `static async Task<int> Main(string[] args)` |
| `await`                | `static async Task Main(string[] args)`      |
| `return`               | `static int Main(string[] args)`             |
| 否 `await` 或 `return` | `static void Main(string[] args)`            |

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]
[顶级语句](~/_csharplang/proposals/csharp-9.0/top-level-statements.md)

## <a name="see-also"></a>另请参阅

- [C# 编程指南](../index.md)
- [方法](../classes-and-structs/methods.md)
- [在 C# 程序内部](../inside-a-program/index.md)
