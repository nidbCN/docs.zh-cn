---
title: Ilasm.exe（IL 汇编程序）
description: 开始使用 Ilasm.exe（IL 汇编程序）。 此工具从中间语言 (IL) 程序集生成可移植可执行 (PE) 文件。
ms.date: 03/30/2017
helpviewer_keywords:
- MSIL generators
- metadata, MSIL Assembler
- MSIL Assembler
- portable executable files, MSIL Assembler
- PE files, MSIL Assembler
- MSIL
- Ilasm.exe
- verifying MSIL performance
ms.assetid: 4ca3a4f0-4400-47ce-8936-8e219961c76f
ms.openlocfilehash: 6057c8dcaaa8a8f8873e5807009e1b0422f4ff33
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654123"
---
# <a name="ilasmexe-il-assembler"></a>Ilasm.exe（IL 汇编程序）

IL 汇编程序可利用中间语言 (IL) 程序集生成可移植可执行 (PE) 文件。 （有关 IL 的详细信息，请参阅[托管执行过程](../../standard/managed-execution-process.md)。）可以运行生成的可执行文件（包含 IL 和所需的元数据）以确定 IL 是否按预期执行。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
ilasm [options] filename [[options]filename...]
```

## <a name="parameters"></a>参数

| 参数 | 说明 |
| -------- | ----------- |
|`filename`|.il 源文件的名称。 该文件包含元数据声明指令和符号化 IL 指令。 可以提供多个源文件参数，以便用 Ilasm.exe 生成单个 PE 文件。 **注意：** 确保 .il 源文件中的最后一行代码具有尾随空格或行尾字符。|

| 选项 | 描述 |
| ------ | ----------- |
|**/32bitpreferred**|创建 32 位首选映像 (PE32+)。|
|/alignment: `integer`|将 FileAlignment 设置为由 NT Optional 标头中的 `integer` 指定的值。 如果在文件中指定了 .alignment IL 指令，则此选项将重写它。|
|**/appcontainer**|生成在 Windows 应用容器中运行的 .dll 或 .exe 文件作为输出 。|
|**/arm**|指定高级 RISC 计算机 (ARM) 作为目标处理器。<br /><br /> 如果未指定映像位数，则默认值为 **/32bitpreferred**。|
|/base: `integer`|将 ImageBase 设置为由 NT Optional 标头中的 `integer` 指定的值。 如果在文件指定了 .imagebase IL 指令，则此选项将重写它。|
|**/clock**|为指定的 .il 源文件测量并报告下列编译时间（以毫秒为单位）：<br /><br /> 总运行时间：执行后面的所有特定操作所花费的总时间。<br /><br /> 启动：加载并打开文件。<br /><br /> 发出 MD：发出元数据。<br /><br /> 定义引用解析：解析对文件中的定义的引用。<br /><br /> CEE 文件生成：在内存中生成文件映像。<br /><br /> PE 文件写入：将映像写入 PE 文件。|
|**/debug**[:**IMPL**&#124;**OPT**]|包括调试信息（局部变量名和参数名以及行号）。 创建 PDB 文件。<br /><br /> 不带任何附加值的 **/debug** 禁用 JIT 优化，并使用 PDB 文件中的序列点。<br /><br /> **IMPL** 禁用 JIT 优化，并使用隐式序列点。<br /><br /> **OPT** 启用 JIT 优化，并使用隐式序列点。|
|**/dll**|生成 .dll 文件作为输出。|
|/enc: `file`|从指定的源文件创建“编辑并继续”增量。<br /><br /> 此参数仅可用于教学目的，不支持商业使用。|
|**/exe**|生成可执行文件作为输出。 这是默认设置。|
|**/flags:** `integer`|将 ImageFlags 设置为由公共语言运行时标头中的 `integer` 指定的值。 如果在文件中指定了 .corflags IL 指令，则此选项将重写它。 有关 *integer* 的有效值的列表，请参见 CorHdr.h 中的 COMIMAGE_FLAGS。|
|**/fold**|将相同的方法体合并为一体。|
|/**highentropyva**|生成支持高熵地址空间布局随机化 (ASLR) 的可执行输出。 （ **/appcontainer** 的默认值。）|
|/include: `includePath`|设置要在其中搜索 `#include`包含的文件的路径。|
|**/itanium**|指定 Intel 的 Itanium 作为目标处理器。<br /><br /> 如果未指定映像位数，则默认值为 **/pe64**。|
|/key: `keyFile`|使用 `keyFile` 中包含的私钥编译具有强签名的 `filename`。|
|**/key:**  @`keySource`|使用 `keySource` 中生成的私钥编译具有强签名的 `filename`。|
|**/listing**|在标准输出上生成列表文件。 如果省略此选项，则不生成列表文件。<br /><br /> 此参数在 .NET Framework 2.0 或更高版本中不受支持。|
|/mdv: `versionString`|设置元数据版本字符串。|
|/msv: `major`.`minor`|设置元数据流版本，其中 `major` 和 `minor` 是整数。|
|**/noautoinherit**|当未指定基类时，禁用从 <xref:System.Object> 的默认继承。|
|**/nocorstub**|取消生成 CORExeMain 存根。|
|**/nologo**|取消显示 Microsoft 启动版权标志。|
|/output: `file.ext`|指定输出文件名和扩展名。 默认情况下，输出文件名与第一个源文件的名称相同。 默认扩展名为 .exe。 如果指定 /dll 选项，则默认扩展名为 .dll。 **注意：** 指定 /output:myfile.dll 并不会设置 /dll 选项 。 如果不指定 /dll，结果将会是名为 myfile.dll 的可执行文件。|
|**/optimize**|将长指令优化为短指令。 例如，将 `br` 优化为 `br.s`。|
|**/pe64**|创建 64 位映像 (PE32+)。<br /><br /> 如果未指定目标处理器，则默认值为 `/itanium`。|
|**/pdb**|创建 PDB 文件但不启用调试信息跟踪。|
|**/quiet**|指定安静模式；不报告程序集进度。|
|/resource: `file.res`|在生成的 .exe 或 .dll 文件中包括 \*.res 格式的指定资源文件 。 使用 **/resource** 选项只能指定一个 .res 文件。|
|/ssver: `int`.`int`|在 NT Optional 标头中设置子系统版本号。 对于 **/appcontainer** 和 **/arm** ，最低版本号为 6.02。|
|/stack: `stackSize`|将 NT Optional 标头中的 SizeOfStackReserve 值设置为 `stackSize`。|
|**/stripreloc**|指定不需要基重定位。|
|/subsystem: `integer`|将 subsystem 设置为由 NT Optional 标头中的 `integer` 指定的值。 如果在文件中指定了 .subsystem IL 指令，则此命令将重写它。 有关 `integer` 的有效值的列表，请参见 winnt.h 中的 IMAGE_SUBSYSTEM。|
|**/x64**|指定 64 位 AMD 处理器作为目标处理器。<br /><br /> 如果未指定映像位数，则默认值为 **/pe64**。|
|**/?**|显示该工具的命令语法和选项。|

> [!NOTE]
> Ilasm.exe 的所有选项都不区分大小写，并且根据前三个字母识别。 例如， /lis 等效于 /listing，/res:myresfile.res 等效于 /resource:myresfile.res   。指定参数的选项既可以用冒号 (:) 也可以用等号 (=) 作为选项和参数之间的分隔符。 例如，/output:file.ext 等效于 /output=file.ext。

## <a name="remarks"></a>备注

IL 汇编程序有助于工具供应商设计和实现 IL 生成器。 通过使用 Ilasm.exe，工具和编译器开发人员可以专注于生成 IL 和元数据，而无需考虑如何以 PE 文件格式发出 IL。

与面向运行时的其他编译器（如 C# 和 Visual Basic）类似，Ilasm.exe 不产生中间对象文件，并且不需要链接阶段即可形成 PE 文件。

IL 汇编程序可以展现以运行时为目标的编程语言的所有现有元数据和 IL 功能。 这使得用上面任何编程语言编写的托管代码都可以在 IL 汇编程序中充分展现并且可以用 Ilasm.exe 编译。

> [!NOTE]
> 如果 .il 源文件中的最后一行代码不具有尾随空格或行尾字符，则编译可能会失败。

可以将 Ilasm.exe 与它的配套工具 [Ildasm.exe](ildasm-exe-il-disassembler.md)一起使用 。 Ildasm.exe 利用包含 IL 代码的 PE 文件，创建适合输入到 Ilasm.exe 的文本文件 。 这很有用，例如在编译用并非支持所有运行时元数据特性的编程语言编写的代码时。 通过 Ildasm.exe 编译该代码并运行输出后，可以手动编辑生成的 IL 文本文件以添加缺少的属性。 然后可通过 Ilasm.exe 运行此文本文件，生成最终的可执行文件。

也可以使用此方法从最初由不同的编译器生成的数个 PE 文件生成一个 PE 文件。

> [!NOTE]
> 目前，无法对包含嵌入的本机代码的 PE 文件（例如，由 Visual C++ 生成的 PE 文件）使用此技术。

为尽可能准确地使用 Ildasm.exe 和 Ilasm.exe 的这种组合，默认情况下汇编程序不会将可能已在 IL 源中写入（或可能由其他编译程序发出）的长编码替换为短编码 。 请尽可能使用 **/optimize** 选项替代为短编码。

> [!NOTE]
> Ildasm.exe 只对磁盘上的文件进行操作。 它不对安装在全局程序集缓存中的文件进行操作。

有关 IL 语法的更多信息，请参见 Windows SDK 中的 asmparse.grammar 文件。

## <a name="version-information"></a>版本信息

从 .NET Framework 4.5 开始，可以使用类似于下面的代码将自定义特性附加到接口实现：

```il
.class interface public abstract auto ansi IMyInterface
{
  .method public hidebysig newslot abstract virtual
    instance int32 method1() cil managed
  {
  } // end of method IMyInterface::method1
} // end of class IMyInterface
.class public auto ansi beforefieldinit MyClass
  extends [mscorlib]System.Object
  implements IMyInterface
  {
    .interfaceimpl type IMyInterface
    .custom instance void
      [mscorlib]System.Diagnostics.DebuggerNonUserCodeAttribute::.ctor() = ( 01 00 00 00 )
      …
```

从 .NET Framework 4.5 开始，可以指定任意封送 BLOB（二进制大型对象），方法是使用其原始二进制表示形式，如以下代码所示：

```il
.method public hidebysig abstract virtual
        instance void
        marshal({ 38 01 02 FF })
        Test(object A_1) cil managed
```

有关 IL 语法的更多信息，请参见 Windows SDK 中的 asmparse.grammar 文件。

## <a name="examples"></a>示例

下面的命令对 IL 文件 myTestFile.il 进行汇编并生成可执行文件 myTestFile.exe 。

```console
ilasm myTestFile
```

下面的命令对 IL 文件 myTestFile.il 进行汇编并生成 .dll 文件 myTestFile.dll  。

```console
ilasm myTestFile /dll
```

下面的命令对 IL 文件 myTestFile.il 进行汇编并生成 .dll 文件 myNewTestFile.dll  。

```console
ilasm myTestFile /dll /output:myNewTestFile.dll
```

下面的代码示例演示了一个向控制台显示“Hello World!”的极其简单的 “Hello World!”。 可编译此代码，然后使用 [Ildasm.exe](ildasm-exe-il-disassembler.md) 工具生成 IL 文件。

```csharp
using System;

public class Hello
{
    public static void Main(String[] args)
    {
        Console.WriteLine("Hello World!");
    }
}
```

下面的 IL 代码示例对应于前面的 C# 代码示例。 可使用 IL Assembler 工具将此代码编译为程序集。 IL 和 C# 代码示例都向控制台显示 “Hello World!”。

```il
// Metadata version: v2.0.50215
.assembly extern mscorlib
{
  .publickeytoken = (B7 7A 5C 56 19 34 E0 89 )                         // .z\V.4..
  .ver 2:0:0:0
}
.assembly sample
{
  .custom instance void [mscorlib]System.Runtime.CompilerServices.CompilationRelaxationsAttribute::.ctor(int32) = ( 01 00 08 00 00 00 00 00 )
  .hash algorithm 0x00008004
  .ver 0:0:0:0
}
.module sample.exe
// MVID: {A224F460-A049-4A03-9E71-80A36DBBBCD3}
.imagebase 0x00400000
.file alignment 0x00000200
.stackreserve 0x00100000
.subsystem 0x0003       // WINDOWS_CUI
.corflags 0x00000001    //  ILONLY
// Image base: 0x02F20000

// =============== CLASS MEMBERS DECLARATION ===================

.class public auto ansi beforefieldinit Hello
       extends [mscorlib]System.Object
{
  .method public hidebysig static void  Main(string[] args) cil managed
  {
    .entrypoint
    // Code size       13 (0xd)
    .maxstack  8
    IL_0000:  nop
    IL_0001:  ldstr      "Hello World!"
    IL_0006:  call       void [mscorlib]System.Console::WriteLine(string)
    IL_000b:  nop
    IL_000c:  ret
  } // end of method Hello::Main

  .method public hidebysig specialname rtspecialname
          instance void  .ctor() cil managed
  {
    // Code size       7 (0x7)
    .maxstack  8
    IL_0000:  ldarg.0
    IL_0001:  call       instance void [mscorlib]System.Object::.ctor()
    IL_0006:  ret
  } // end of method Hello::.ctor

} // end of class Hello
```

## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Ildasm.exe（IL 反汇编程序）](ildasm-exe-il-disassembler.md)
- [托管执行过程](../../standard/managed-execution-process.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
