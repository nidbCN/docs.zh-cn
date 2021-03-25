---
description: 用于控制编译输入文件的 C# 编译器选项。 这些选项指定编译器如何从依赖程序集和模块读取元数据。
title: C# 编译器选项 - 输入文件选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- References compiler option [C#]
- AddModules compiler option [C#]
- EmbedInteropTypes compiler option [C#]
ms.openlocfilehash: 819e2322720782b94bd744e00c602221f023c0d8
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482431"
---
# <a name="c-compiler-options-that-specify-inputs"></a>可指定输入的 C# 编译器选项

以下选项控制编译器输入。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **References** / `-reference` 或 `-references`：引用来自一个或多个指定程序集文件的元数据。
- **AddModules** / `-addmodule`：添加模块（将使用 `target:module` 创建的模块添加到此程序集。）
- **EmbedInteropTypes** / `-link`：嵌入指定互操作程序集文件中的元数据。

## <a name="references"></a>参考

References 选项使编译器将指定文件中的 [public](../keywords/public.md) 类型信息导入当前项目，从而使你可从指定的程序集文件中引用元数据。

```xml
<Reference Include="filename" />
```

 `filename` 是包含程序集清单的文件的名称。 若要导入多个文件，请为每个文件加上一个单独的 Reference 元素。 可以定义一个别名作为 Reference 元素的子元素：

```xml
<Reference Include="filename.dll">
  <Aliases>LS</Aliases>
</Reference>
```

在上面的示例中，`LS` 是表示根命名空间的有效 C# 标识符，该根命名空间将包含程序集 filename.dll 中的所有命名空间。 导入的文件必须包含一个清单。 使用 [AdditionalLibPaths](advanced.md#additionallibpaths) 指定一个或多个程序集引用所在的目录。 [AdditionalLibPaths](advanced.md#additionallibpaths) 主题还讨论了编译器在其中搜索程序集的目录。 为使编译器可以识别程序集（而不是模块）中的某个类型，需要强制解析此类型，这可以通过定义此类型的实例来完成。 还可通过其他方法为编译器解析程序集中的类型名称：例如，如果从程序集中继承类型，编译器就能识别类型名称。 有时，需要在一个程序集内引用同一组件的两个不同版本。 为此，请在每个文件的 References 元素上使用 Aliases 元素，以区分这两个文件。 此别名将用作组件名的限定符，并解析为其中一个文件中的组件。

> [!NOTE]
> 在 Visual Studio 中，请使用“添加引用”命令。 有关详细信息，请参阅 [How to: Add or Remove References By Using the Reference Manager](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager)。

## <a name="addmodules"></a>AddModules

此选项将添加一个模块，该模块通过将 `<TargetType>module</TargetType>` 切换到当前编译进行创建：

```xml
<AddModule Include=file1 />
<AddModule Include=file2 />
```

其中 `file`、`file2` 是包含元数据的输出文件。 该文件不能包含程序集清单。 若要导入多个文件，请用逗号或分号将文件名隔开。 通过 AddModules 添加的所有模块在运行时必须位于与输出文件相同的目录中。 也就是说，在编译时可在任何目录中指定模块，但在运行时该模块必须位于应用程序目录中。 如果在运行时该模块不位于应用程序目录中，你将收到 <xref:System.TypeLoadException>。 `file` 不能包含程序集。 例如，如果输出文件是使用 module 的 [TargetType](output.md#targettype) 创建的，那么它的元数据可通过 AddModules 导入。

如果输出文件是使用 [TargetType](output.md#targettype) 选项而不是 module 创建的，则它的元数据不能通过 AddModules 导入，但可以通过 [References](#references) 选项导入。

## <a name="embedinteroptypes"></a>EmbedInteropTypes

使编译器让指定程序集中的 COM 类型信息可供当前正在编译的项目使用。

```xml
<References>
  <EmbedInteropTypes>file1;file2;file3</EmbedInteropTypes>
</References>
```

其中 `file1;file2;file3` 是以分号分隔的程序集文件名的列表。 如果文件名包含空格，则将名称括在引号内。 使用 EmbedInteropTypes 选项可以部署具有嵌入类型信息的应用程序。 应用程序随后可以使用运行时程序集中实现嵌入类型信息的类型，而无需引用运行时程序集。 如果发布了各种版本的运行时程序集，则包含嵌入类型信息的应用程序可以使用各种版本，而无需重新编译。 有关示例，请参阅[演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)。

当你使用 COM 互操作时，EmbedInteropTypes 选项尤其有用。 可以嵌入 COM 类型，以便应用程序在目标计算机上不再需要主互操作程序集 (PIA)。 EmbedInteropTypes 选项指示编译器将引用的互操作程序集中的 COM 类型信息嵌入到生成的已编译代码中。 COM 类型由 CLSID (GUID) 值进行标识。 因此，应用程序可以在安装了具有相同 CLSID 值的相同 COM 类型的目标计算机上运行。 自动执行 Microsoft Office 的应用程序是一个很好的示例。 由于 Office 等应用程序通常在不同版本间保持相同的 CLSID 值，因此只要在目标计算机上安装了 .NET Framework 4 或更高版本，并且应用程序使用引用的 COM 类型中包含的方法、属性或事件，应用程序便可以使用引用的 COM 类型。 EmbedInteropTypes 选项只嵌入接口、结构和委托。 不支持嵌入 COM 类。

> [!NOTE]
> 在代码中创建嵌入 COM 类型的实例时，必须使用适当的接口创建该实例。 尝试使用组件类创建嵌入 COM 类型的实例会导致错误。

与 [References](#references) 编译器选项一样，EmbedInteropTypes 编译器选项使用 Csc.rsp 响应文件，该文件引用常用的 .NET 程序集。 如果你不希望编译器使用 Csc.rsp 文件，可以使用 [NoConfig](miscellaneous.md#noconfig) 编译器选项。

[!code-csharp[VbLinkCompilerCS#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#1)]

对于具有类型是从互操作程序集嵌入的泛型参数的类型，如果该类型来自外部程序集，则无法使用这种类型。 此限制不适用于接口。 例如，考虑在 <xref:Microsoft.Office.Interop.Excel> 程序集中定义的 <xref:Microsoft.Office.Interop.Excel.Range> 接口。 如果某个库从 <xref:Microsoft.Office.Interop.Excel> 程序集嵌入互操作类型，并且公开的一个方法返回具有类型是 <xref:Microsoft.Office.Interop.Excel.Range> 接口的参数的泛型类型，则该方法必须返回泛型接口，如下面的代码示例所示。

[!code-csharp[VbLinkCompilerCS#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/utility.cs)]

在下面的示例中，客户端代码可以调用返回 <xref:System.Collections.IList> 泛型接口的方法而不会出现错误。

[!code-csharp[VbLinkCompilerCS#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#5)]
