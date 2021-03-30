---
description: 用于控制编译器输出的 C# 编译器选项。 这些选项通过编译控制程序集生成。
title: C# 编译器选项 - 输出选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- DocumentationFile compiler option [C#]
- OutputAssembly compiler option [C#]
- PlatformTarget compiler option [C#]
- ProduceReferenceAssembly compiler option [C#]
- TargetType compiler option [C#]
ms.openlocfilehash: 9caa290a7c9b5fea1b0f896e9443075b4b470f7b
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105636839"
---
# <a name="c-compiler-options-that-control-compiler-output"></a>用于控制编译器输出的 C# 编译器选项

以下选项控制编译器输出生成。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **DocumentationFile** / `-doc`：从 `///` 注释生成 XML 文档文件。
- **OutputAssembly** / `-out`：指定输出程序集文件。
- **PlatformTarget** / `-platform`：指定目标平台 CPU。
- **ProduceReferenceAssembly** / `-refout`：生成引用程序集。
- **TargetType**`-target`：指定输出程序集的类型。

## <a name="documentationfile"></a>DocumentationFile

使用 DocumentationFile 选项可以在 XML 文件中放置文档注释。 若要详细了解如何记录代码，请参阅[建议的文档注释标记](../../programming-guide/xmldoc/recommended-tags-for-documentation-comments.md)。 该值指定输出 XML 文件的路径。 此 XML 文件包含编译的源代码文件中的注释。

```xml
<DocumentationFile>path/to/file.xml</DocumentationFile>
```

包含 Main 或顶级语句的源代码文件首先输出到 XML 中。 通常需要将生成的 .xml 文件与 [IntelliSense](/visualstudio/ide/using-intellisense) 一起使用。 .xml 文件名必须与程序集名称相同。 .xml 文件必须与程序集位于同一目录中。 在 Visual Studio 项目中引用程序集时，也会找到 .xml 文件。 若要详细了解如何生成代码注释，请参阅[提供代码注释](/visualstudio/ide/reference/generate-xml-documentation-comments)。 除非用 [`<TargetType:Module>`](#targettype) 进行编译，否则 `file` 将包含 `<assembly>` 和 `</assembly>` 标记，用于指定包含输出文件的程序集清单的文件名。 例如，请参阅[如何使用 XML 文档功能](../../programming-guide/xmldoc/how-to-use-the-xml-documentation-features.md)。

> [!NOTE]
> DocumentationFile 选项适用于项目中的所有文件。 若要禁用与特定文件或一段代码的文档注释相关的警告，请使用 [#pragma 警告](../preprocessor-directives.md#pragma-warning)。

## <a name="outputassembly"></a>OutputAssembly

OutputAssembly 选项指定输出文件的名称。 输出路径指定放置编译器输出的文件夹。

```xml
<OutputAssembly>folder</OutputAssembly>
```

指定想要创建的文件的完整名称和扩展名。 如果不指定输出文件的名称，MSBuild 将使用项目的名称指定输出程序集的名称。 旧样式的项目使用以下规则：

- .exe 将采用包含 `Main` 方法或顶级语句的源代码文件中的名称。  
- .dll 或者 .netmodule 将从第一个源代码文件中获取其名称。

在编译时生成的任何模块都将成为与编译时生成的程序集关联的文件。 使用 [ildasm.exe](../../../framework/tools/ildasm-exe-il-disassembler.md) 查看程序集清单，了解关联文件。

为使 exe 成为[友元程序集](../../../standard/assembly/friend.md)的目标，OutputAssembly 编译器选项是必需的。

## <a name="platformtarget"></a>PlatformTarget

指定 CLR 的哪个版本可以运行程序集。

```xml
<PlatformTarget>anycpu</PlatformTarget>
```

- anycpu（默认值）将程序集编译成可在任意平台上运行。 您的应用程序将尽可能作为 64 位进程运行；当只有 32 位模式可用时，才会回退到 32 位。
- anycpu32bitpreferred 将程序集编译成可在任意平台上运行。 在同时支持 64 位和 32 位应用程序的系统上，您的应用程序将以32 位模式运行。 只能为面向 .NET Framework 4.5 或更高版本的项目指定此选项。
- ARM 将程序集编译成可以在具有高级 RISC 计算机 (ARM) 处理器的计算机上运行。
- ARM64 编译程序集以在由 64 位 CLR 在具有支持 A64 指令集的高级 RISC 计算机 (ARM) 处理器的计算机上运行。
- x64 将程序集编译成可由支持 AMD64 或 EM64T 指令集的计算机上的 64 位 CLR 运行。
- x86 将程序集编译成可由 32 位、x86 可兼容 CLR 运行。
- Itanium 将程序集编译成可由配有 Itanium 处理器的计算机上的 64 位 CLR 运行。

在 64 位 Windows 操作系统上：

- 用 x86 编译的程序集将在 WOW64 下运行的 32 位 CLR 上执行。
- 用 anycpu 编译的 DLL 将在加载它的进程所在的同一 CLR 上执行。
- 用 anycpu 编译的可执行文件将在 64 位 CLR 上执行。
- 用 anycpu32bitpreferred 编译的可执行文件将在 32 位 CLR 上执行。

anycpu32bitpreferred 设置只对可执行文件 (.EXE) 有效，并且需要 .NET Framework 4.5 或更高版本。 有关开发 Windows 64 位操作系统上运行的应用程序的详细信息，请参阅 [64 位应用程序](../../../framework/64-bit-apps.md)。

在 Visual Studio 中，可以从项目的“生成”属性页中设置 PlatformTarget 选项。

对于 .NET Core 和 .NET 5 及更高版本，anycpu 的行为有一些额外的细微差别。 设置 anycpu 后，请发布应用并将其与 x86 `dotnet.exe` 或 x64 `dotnet.exe` 一起执行。 对于自包含应用，`dotnet publish` 步骤会将配置 RID 的可执行文件打包。

## <a name="producereferenceassembly"></a>ProduceReferenceAssembly

ProduceReferenceAssembly 选项指定应输出引用程序集的文件路径。 它在 Emit API 中转换为 `metadataPeStream`。 `filepath` 指定引用程序集的路径。 通常情况下，应与主程序集的路径匹配。 建议的约定（由 MSBuild 采用）是，将引用程序集放入与主程序集相关的“ref/”子文件夹中。

```xml
<ProduceReferenceAssembly>filepath</ProduceReferenceAssembly>
```

引用程序集是一种特殊类型的程序集，它只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明。 引用程序集不包括所有成员实现以及私有成员的声明。 这些成员对其 API 协定没有明显影响。 有关详细信息，请参阅 .NET 指南中的[引用程序集](../../../standard/assembly/reference-assemblies.md)。

ProduceReferenceAssembly 和 [ProduceOnlyReferenceAssembly](./code-generation.md#produceonlyreferenceassembly) 选项是互斥的。

## <a name="targettype"></a>TargetType

TargetType 编译器选项可指定为以下形式之一：  
  
- library：用于创建代码库。 library 是默认值。
- exe：用于创建 .exe 文件。  
- module：用于创建模块。  
- winexe：用于创建 Windows 程序。
- winmdobj：用于创建 .winmdobj 中间文件。
- appcontainerexe：用于为 Windows 8.x 应用商店应用创建 .exe 文件。
  
> [!NOTE]
> 对于 .NET Framework 目标，除非指定 module，否则，此选项会导致将 .NET Framework 程序集清单放入输出文件中。 有关详细信息，请参阅 [.NET 中的程序集](../../../standard/assembly/index.md)和[公共属性](../attributes/global.md)。

```xml
<TargetType>library</TargetType>
```

编译器每次编译只创建一个程序集清单。 关于编译中所有文件的信息都放在程序集清单中。 在命令行生成多个输出文件时，只能创建一个程序集清单，且该清单必须放置在命令行上指定的第一个输出文件中。

如果你创建了一个程序集，则可以用 <xref:System.CLSCompliantAttribute> 属性指示全部或部分代码是符合 CLS 的。

### <a name="library"></a>库

library 选项会导致编译器创建动态链接库 (DLL) 而不是可执行文件 (EXE)。 将创建具有 .dll 扩展名的 DLL。 除非使用 [OutputAssembly](#outputassembly) 选项指定，否则输出文件的名称采用第一个输入文件的名称。 生成 .dll 文件时，不需要 [`Main`](../../programming-guide/main-and-command-args/index.md) 方法。

### <a name="exe"></a>exe

exe 选项会导致编译器创建一个可执行的 (EXE) 控制台应用程序。 将创建扩展名为 .exe 的可执行文件。 使用 winexe 创建 Windows 程序可执行文件。 除非使用 [OutputAssembly](#outputassembly) 选项进行指定，否则输出文件的名称将采用包含入口点（[Main](../../programming-guide/main-and-command-args/index.md) 方法或顶级语句）的输入文件的名称。 编译为 .exe 文件的源代码文件中只需要一个入口点。 如果代码具有多个附带 `Main` 方法的类，则可使用 [StartupObject](./advanced.md#mainentrypoint-or-startupobject) 编译器选项指定包含 `Main` 方法的类。  

### <a name="module"></a>name

此选项会导致编译器不生成程序集清单。 默认情况下，使用此选项编译时所创建的输出文件具有扩展名 .netmodule。 .NET 运行时无法加载没有程序集清单的文件。 但是，此类文件可以通过 [AddModules](inputs.md#addmodules) 合并到程序集的程序集清单中。 如果在一次编译中创建了多个模块，某个模块中的[内部](../keywords/internal.md)类型将适用于编译中的其他模块。 如果一个模块中的代码引用另一模块中的 `internal` 类型，则两个模块必须通过 [AddModules](inputs.md#addmodules) 合并到一个程序集清单中。 Visual Studio 开发环境中不支持创建模块。

### <a name="winexe"></a>winexe

winexe 选项会导致编译器创建可执行的 (EXE) Windows 程序。 将创建扩展名为 .exe 的可执行文件。 Windows 程序是通过 .NET 库或 Windows API 提供用户界面的程序。 使用 exe 创建控制台应用程序。 除非使用 [OutputAssembly](#outputassembly) 选项进行指定，否则输出文件的名称将采用包含 [`Main`](../../programming-guide/main-and-command-args/index.md) 方法的输入文件的名称。 编译为 .exe 文件的源代码文件中只需要一个 `Main` 方法。 如果代码具有多个附带 `Main` 方法的类，则可使用 [StartupObject](./advanced.md#mainentrypoint-or-startupobject) 选项指定包含 `Main` 方法的类。

### <a name="winmdobj"></a>winmdobj

如果使用 winmdobj 选项，则编译器会创建一个 .winmdobj 中间文件，你可以将此文件转换为 Windows 运行时二进制 (.winmd) 文件。 之后，除了托管语言程序外，JavaScript 和 C++ 程序也可以使用该 .winmd 文件。

**winmdobj** 设置会向编译器发出信号，表示需要中间模块。 随后，.winmdobj 文件可作为 <xref:Microsoft.Build.Tasks.WinMDExp> 导出工具的输入，生成 Windows 元数据 (.winmd) 文件。 .winmd 文件既包含原始库的代码，也包含 JavaScript 或 C++ 以及 Windows 运行时使用的 WinMD 元数据的代码。 使用 winmdobj 编译器选项所编译文件的输出只能用作 WimMDExp 导出工具的输入。 .winmdobj 文件本身并没有被直接引用。 除非使用 [OutputAssembly](#outputassembly) 选项，否则输出文件的名称将采用第一个输入文件的名称。 不需要使用 [`Main`](../../programming-guide/main-and-command-args/index.md) 方法。

### <a name="appcontainerexe"></a>appcontainerexe

如果使用 appcontainerexe 编译器选项，则编译器会创建一个 Windows 可执行 (.exe) 文件，该文件必须在应用容器中运行。 此选项与 [-target:winexe](output.md) 等效，但专门用于 Windows 8.x 应用商店应用。

为了要求应用在应用容器中运行，此选项在[可移植可执行](/windows/desktop/Debug/pe-format) (PE) 文件中设置了一个位。 设置该位时，如果 CreateProcess 方法尝试在应用容器外启动该可执行文件，就会发生错误。 除非使用 [OutputAssembly](#outputassembly) 选项，否则输出文件的名称将采用包含 [`Main`](../../programming-guide/main-and-command-args/index.md) 方法的输入文件的名称。
