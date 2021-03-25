---
title: 命令行参数 - C# 编程指南
description: 了解命令行参数。 查看在控制台应用程序中使用命令行参数的示例。
ms.date: 03/11/2021
helpviewer_keywords:
- command-line arguments [C#]
ms.assetid: 0e597e0d-ea7a-41ba-a38a-0198122f3c26
ms.openlocfilehash: f495efb3bc2c76a98f74c173d5b777ebe383edcb
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190406"
---
# <a name="command-line-arguments-c-programming-guide"></a>命令行参数（C# 编程指南）

可以通过以下方式之一定义方法来将自变量发送到 `Main` 方法：

| `Main` 方法代码                 | `Main` 签名                             |
|------------------------------------|----------------------------------------------|
| 无返回值，不使用 `await` | `static void Main(string[] args)`            |
| 返回值，不使用 `await`    | `static int Main(string[] args)`             |
| 无返回值，使用 `await`      | `static async Task Main(string[] args)`      |
| 返回值，使用 `await`         | `static async Task<int> Main(string[] args)` |

如果不使用参数，可以从方法签名中省略 `args`，使代码更为简单：

| `Main` 方法代码                 | `Main` 签名                |
|------------------------------------|---------------------------------|
| 无返回值，不使用 `await` | `static void Main()`            |
| 返回值，不使用 `await`    | `static int Main()`             |
| 无返回值，使用 `await`      | `static async Task Main()`      |
| 返回值，使用 `await`         | `static async Task<int> Main()` |

> [!NOTE]
> 若要在 Windows 窗体应用程序的 `Main` 方法中启用命令行参数，必须手动修改 program.cs 中 `Main` 的签名。 Windows 窗体设计器生成的代码创建没有输入参数的 `Main`。 还可使用 <xref:System.Environment.CommandLine%2A?displayProperty=nameWithType> 或 <xref:System.Environment.GetCommandLineArgs%2A?displayProperty=nameWithType> 从控制台或 Windows 应用程序的任意位置访问命令行参数。

`Main` 方法的参数是一个表示命令行参数的 <xref:System.String> 数组。 通常，通过测试 `Length` 属性来确定参数是否存在，例如：

[!code-csharp[csProgGuideMain#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#4)]

> [!TIP]
> `args` 数组不能为 null。 因此，无需进行 null 检查即可放心地访问 `Length` 属性。

还可以使用 <xref:System.Convert> 类或 `Parse` 方法将字符串参数转换为数字类型。 例如，以下语句使用 <xref:System.Int64.Parse%2A> 方法将 `string` 转换为 `long` 数字：

```csharp
long num = Int64.Parse(args[0]);
```

也可以使用 C# 类型 `long`，其别名为 `Int64`：

```csharp
long num = long.Parse(args[0]);
```

还可以使用 `Convert` 类方法 `ToInt64` 来执行同样的操作：

```csharp
long num = Convert.ToInt64(s);
```

有关详细信息，请参阅 <xref:System.Int64.Parse%2A> 和 <xref:System.Convert>。

## <a name="example"></a>示例

以下示例演示如何在控制台应用程序中使用命令行参数。 应用程序在运行时获取一个参数，将该参数转换为整数，并计算数字的阶乘。 如果未提供任何参数，则应用程序会发出一条消息，说明程序的正确用法。

若要在命令提示符下编译并运行该应用程序，请按照下列步骤操作：

1. 将以下代码粘贴到任何文本编辑器，然后将该文件保存为名为“Factorial.cs”的文本文件。

     [!code-csharp[csProgGuideMain#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class1.cs#16)]

2. 从“开始”屏幕或“开始”菜单中，打开 Visual Studio“开发人员命令提示”窗口，然后导航到包含刚刚创建的文件的文件夹。

3. 输入以下命令以编译应用程序。
  
     `csc Factorial.cs`  
  
     如果应用程序不存在编译错误，则会创建一个名为“Factorial.exe”的可执行文件。
  
4. 输入以下命令以计算 3 的阶乘：
  
     `Factorial 3`  
  
5. 该命令将生成以下输出：`The factorial of 3 is 6.`

> [!NOTE]
> 在 Visual Studio 中运行应用程序时，可在[“项目设计器”->“调试”页](/visualstudio/ide/reference/debug-page-project-designer)中指定命令行参数。

## <a name="see-also"></a>请参阅

- <xref:System.Environment?displayProperty=nameWithType>
- [C# 编程指南](../index.md)
- [Main() 和命令行参数](index.md)
- [如何显示命令行参数](how-to-display-command-line-arguments.md)
- [Main() 返回值](main-return-values.md)
- [类](../classes-and-structs/classes.md)
