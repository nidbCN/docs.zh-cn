---
title: Main() 和命令行参数 - C# 编程指南
description: 了解 Main() 和命令行参数。 “Main”方法是可执行程序的入口点。
ms.date: 03/08/2021
f1_keywords:
- main_CSharpKeyword
- Main
helpviewer_keywords:
- Main method [C#]
- C# language, command-line arguments
- arguments [C#], command-line
- command line [C#], arguments
- command-line arguments [C#], Main method
ms.assetid: 73a17231-cf96-44ea-aa8a-54807c6fb1f4
ms.openlocfilehash: 05b2030133ca83cf87de7110f820eaad38fad756
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103480185"
---
# <a name="main-and-command-line-arguments-c-programming-guide"></a>Main() 和命令行参数（C# 编程指南）

`Main` 方法是 C# 应用程序的入口点。 （库和服务不要求使用 `Main` 方法作为入口点）。`Main` 方法是应用程序启动后调用的第一个方法。

C# 程序中只能有一个入口点。 如果多个类包含 `Main` 方法，必须使用 StartupObject 编译器选项来编译程序，以指定将哪个 `Main` 方法用作入口点。 有关详细信息，请参阅 [StartupObject（C# 编译器选项）](../../language-reference/compiler-options/advanced.md#mainentrypoint-or-startupobject)。

[!code-csharp[csProgGuideMain#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class1.cs#17)]

自 C# 9 起，可以省略 `Main` 方法，并像在 `Main` 方法中一样编写 C# 语句，如下面的示例所示：

:::code language="csharp" source="snippets/top-level-statements-1/Program.cs":::

若要了解如何使用隐式入口点方法编写应用程序代码，请参阅[顶级语句](top-level-statements.md)。

## <a name="overview"></a>概述

- `Main` 方法是可执行程序的入口点，也是程序控制开始和结束的位置。
- `Main` 在类或结构中声明。 `Main` 必须是[静态](../../language-reference/keywords/static.md)方法，不得为[公共](../../language-reference/keywords/public.md)方法。 （在前面的示例中，它获得的是[私有](../../language-reference/keywords/private.md)成员的默认访问权限）。封闭类或结构不一定要是静态的。
- `Main` 可以具有 `void`、`int`，或者以 C# 7.1、`Task` 或 `Task<int>` 返回类型开头。
- 当且仅当 `Main` 返回 `Task` 或 `Task<int>` 时，`Main` 的声明可包括 [`async`](../../language-reference/keywords/async.md) 修饰符。 请注意，该操作可明确排除 `async void Main` 方法。
- 使用或不使用包含命令行自变量的 `string[]` 参数声明 `Main` 方法都行。 使用 Visual Studio 创建 Windows 应用程序时，可以手动添加此形参，也可以使用 <xref:System.Environment.GetCommandLineArgs> 方法来获取[命令行实参](command-line-arguments.md)。 参数被读取为从零开始编制索引的命令行自变量。 与 C 和 C++ 不同，程序的名称不被视为 `args` 数组中的第一个命令行实参，但它是 <xref:System.Environment.GetCommandLineArgs> 方法中的第一个元素。

以下是有效 `Main` 签名的列表：

```csharp
public static void Main() { }
public static int Main() { }
public static void Main(string[] args) { }
public static int Main(string[] args) { }
public static async Task Main() { }
public static async Task<int> Main() { }
public static async Task Main(string[] args) { }
public static async Task<int> Main(string[] args) { }
```

上述示例均使用公共访问器修饰符。 这是典型操作方式，但不是必需操作方式。

添加 `async`、`Task` 和 `Task<int>` 返回类型可简化控制台应用程序需要启动时的程序代码，以及 `Main` 中的 `await` 异步操作。

## <a name="c-language-specification"></a>C# 语言规范

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [方法](../classes-and-structs/methods.md)
- [在 C# 程序内部](../inside-a-program/index.md)
