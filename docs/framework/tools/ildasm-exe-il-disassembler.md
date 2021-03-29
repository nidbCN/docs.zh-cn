---
title: Ildasm.exe（IL 反汇编程序）
description: 使用 Ildasm.exe（IL 反汇编程序），它利用包含中间语言 (IL) 代码的可移植可执行 (PE) 文件，并为 Ilasm.exe 创建一个文本文件。
ms.date: 03/30/2017
helpviewer_keywords:
- PE files, MSIL Disassembler
- portable executable files, MSIL Disassembler
- Ildasm.exe
- MSIL Disassembler
- text files produced by MSIL Disassembler
- disassembling file for MSIL Assembler input
ms.assetid: db27f6b2-f1ec-499e-be3a-7eecf95ca42b
ms.openlocfilehash: 128257e8b1879ef712dd5c33239784fd03de31e2
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654110"
---
# <a name="ildasmexe-il-disassembler"></a>Ildasm.exe（IL 反汇编程序）

IL 反汇编程序是 IL 汇编程序 (Ilasm.exe) 的配套工具。 Ildasm.exe 可利用包含中间语言 (IL) 代码的可移植可执行 (PE) 文件，并创建适合输入到 Ilasm.exe 的文本文件 。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
ildasm [options] [PEfilename] [options]
```

## <a name="parameters"></a>参数

下列选项可用于 .exe、.dll、.obj、.lib 和 .winmd 文件    。

| 选项 | 描述 |
| ------ | ----------- |
|/out= `filename`|创建具有指定 `filename` 的输出文件，而不是在图形用户界面中显示结果。|
|/rtf|以 RTF 格式生成输出。 与 /text 选项一起使用时无效。|
|/text|将结果显示到控制台窗口，而不是显示在图形用户界面中或显示为输出文件。|
|/html|以 HTML 格式生成输出。 与 /output 选项一起使用时才有效。|
|**/?**|显示此工具的命令语法和选项。|

下列附加选项可用于 .exe、.dll 和 .winmd 文件  。

| 选项 | 描述 |
| ------ | ----------- |
|/bytes|以十六进制格式显示作为指令注释的实际字节。|
|/caverbal|以文字形式生成自定义特性 Blob。 默认为二进制形式。|
|/linenum|包含对原始源行的引用。|
|/nobar|禁止显示反汇编进度指示器弹出窗口。|
|/noca|禁止显示自定义特性的输出。|
|/project|以托管代码的方式显示元数据，而不是以本机 Windows 运行时的方式显示。 如果 `PEfilename` 不是 Windows 元数据 (.winmd) 文件，此选项将不起任何作用。 请参阅 [Windows 应用商店应用和 Windows 运行时的 .NET Framework 支持](../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)。|
|/pubonly|仅反汇编公共类型和公共成员。 等效于 /visibility:PUB。|
|/quoteallnames|在单引号中包含所有名称。|
|/raweh|以原始格式显示异常处理子句。|
|/source|以注释形式显示原始源行。|
|/tokens|显示类和成员的元数据标记。|
|/visibility: `vis`[+`vis`...]|仅反汇编具有指定可见性的类型或成员。 下面是 `vis` 的有效值：<br /><br /> PUB - 公共<br /><br /> PRI - 私有<br /><br /> FAM - 系列<br /><br /> ASM - 程序集<br /><br /> FAA - 系列和程序集<br /><br /> FOA - 系列或程序集<br /><br /> PSC - 私有范围<br /><br /> 有关这些可见性修饰符的定义，请参见 <xref:System.Reflection.MethodAttributes> 和 <xref:System.Reflection.TypeAttributes>。|

下列选项仅对用于文件或控制台输出的 .exe、.dll 和 .winmd 文件有效  。

| 选项 | 描述 |
| ------ | ----------- |
|**/all**|指定 /header、/bytes、/stats、/classlist 和 /tokens 选项的组合    。|
|/classlist|包含模块中定义的类的列表。|
|/forward|使用前向类声明。|
|/headers|在输出中包含文件头信息。|
|/item: `class`[:: `member`[`(sig`]] |根据所提供的自变量反汇编下列内容：<br /><br /> -   反汇编指定的 `class`。<br />-   反汇编类的指定 `class` 的 `member`。<br />-   反汇编具有指定签名 `sig` 的 `class` 的 `member`。 `sig` 的格式如下：<br />     [`instance`] `returnType`(`parameterType1`, `parameterType2`, …, `parameterTypeN`)<br />     请注意：在 .NET Framework 1.0 和 1.1 版中，`sig` 必须后跟右括号：`(sig)`。 自 .Net Framework 2.0 起，必须省略右括号：`(sig`。|
|/noil|禁止显示 IL 程序集代码输出。|
|/stats|包含映像的统计信息。|
|/typelist|生成类型的完整列表，以便在往返过程中保留类型排序。|
|/unicode|对输出使用 Unicode 编码。|
|/utf8|对输出使用 UTF-8 编码。 ANSI 为默认值。|

下列选项仅对用于文件或控制台输出的 .exe、.dll、.obj、.lib 和 .winmd 文件有效    。

| 选项 | 描述 |
| ------ | ----------- |
|/metadata[=`specifier`]|显示元数据，其中 `specifier` 为：<br /><br /> MDHEADER - 显示元数据头信息和大小。<br /><br /> HEX - 以十六进制形式及文字形式显示信息。<br /><br /> CSV - 显示记录计数和堆大小。<br /><br /> UNREX - 显示无法解析的外部对象。<br /><br /> SCHEMA - 显示元数据头和架构信息。<br /><br /> RAW - 显示原始元数据表。<br /><br /> HEAPS - 显示原始堆。<br /><br /> VALIDATE - 验证元数据的一致性。<br /><br /> 可多次指定 /metadata，并为 `specifier` 指定不同的值。|

下列选项仅对用于文件或控制台输出的 .lib 文件有效。

| 选项 | 描述 |
| ------ | ----------- |
|/objectfile=`filename`|显示指定库中单个对象文件的元数据。|

> [!NOTE]
> Ildasm.exe 的所有选项都不区分大小写，并且根据前三个字母识别。 例如，/quo 等效于 /quoteallnames 。 指定参数的选项既可以用冒号 (:) 也可以用等号 (=) 作为选项和参数之间的分隔符。 例如，/output: filename 等效于 /output= filename。

## <a name="remarks"></a>备注

Ildasm.exe 只对磁盘上的 PE 文件进行操作。 它不对安装在全局程序集缓存中的文件进行操作。

Ildasm.exe 生成的文本文件可以用作 IL 汇编程序 (Ilasm.exe) 的输入 。 这很有用，例如在编译用并非支持所有运行时元数据特性的编程语言编写的代码时。 通过 Ildasm.exe 编译代码并运行其输出后，可以手动编辑生成的 IL 文本文件以添加缺少的属性。 然后可以通过 IL 汇编程序运行此文本文件以生成最终可执行文件。

> [!NOTE]
> 目前，无法对包含嵌入的本机代码的 PE 文件（例如，由 Visual C++ 生成的 PE 文件）使用此技术。  

你可以使用 IL 反汇编程序中的默认 GUI 在分层树视图中查看任何现有 PE 文件的元数据和反汇编代码。 若要使用此 GUI，请在命令行中键入 ildasm，无需提供 PEfilename 参数或任何选项。 可从“文件”菜单导航到要加载到 Ildasm.exe 中的 PE 文件。 若要保存为选定 PE 显示的元数据和反汇编代码，请从“文件”菜单中选择“转储”命令 。 若要仅保存分层树视图，请从“文件”菜单中选择“转储树视图”命令 。 有关将文件加载到 Ildasm.exe 中和解释输出的详细指南，请参见 Windows SDK 附带的 Samples 文件夹中的 Ildasm.exe 教程 。

如果为 Ildasm.exe 提供一个包含嵌入资源的 PEfilename 参数，则此工具生成多个输出文件：一个包含 IL 代码的文本文件，以及与每个嵌入的托管资源对应的 .resources 文件（使用该资源在元数据中的名称生成） 。 如果 PEfilename 中嵌入了非托管资源，则通过 /output 选项使用 IL 输出指定的文件名生成 .res 文件。

> [!NOTE]
> Ildasm.exe 仅显示 .obj 和 .lib 输入文件的元数据说明  。 不反汇编这些文件类型的 IL 代码。

可对 .exe 或 .dll 文件运行 Ildasm.exe 来确定该文件是否是托管的 。 如果该文件不是托管的，则此工具将显示一条信息，说明该文件没有有效的公共语言运行时标头，并且无法反汇编。 如果该文件是托管的，则此工具将成功运行。

## <a name="version-information"></a>版本信息

从 .NET Framework 4.5 开始，Ildasm.exe 将通过显示原始二进制内容来处理无法识别的封送 BLOB（二进制大型对象）。 例如，下面的代码演示 C# 程序生成的封送 BLOB 如何显示：

```csharp
public void Test([MarshalAs((short)70)] int test) { }
```

```il
// IL from Ildasm.exe output
.method public hidebysig instance void Test(int32  marshal({ 46 }) test) cil managed
```

从 .NET Framework 4.5 开始，Ildasm.exe 将显示应用于接口实现的属性，如下方 Ildasm.exe 输出摘录所示 ：

```il
.class public auto ansi beforefieldinit MyClass
  extends [mscorlib]System.Object
  implements IMyInterface
  {
    .interfaceimpl type IMyInterface
    .custom instance void
      [mscorlib]System.Diagnostics.DebuggerNonUserCodeAttribute::.ctor() = ( 01 00 00 00 )
      …
```

## <a name="examples"></a>示例

下面的命令使 PE 文件 `MyHello.exe` 的元数据和反汇编代码显示在 Ildasm.exe 的默认 GUI 中。

```console
ildasm myHello.exe
```

下面的命令对 `MyFile.exe` 文件进行反汇编，并将生成的 IL 汇编程序文本存储在 MyFile.il 文件中。

```console
ildasm MyFile.exe /output:MyFile.il
```

下面的命令对 `MyFile.exe` 文件进行反汇编，并将结果 IL 汇编程序文本显示到控制台窗口中。

```console
ildasm MyFile.exe /text
```

如果文件 `MyApp.exe` 包含嵌入的托管和非托管资源，则下面的命令将生成 4 个文件：MyApp.il、MyApp.res、Icons.resources 和 Message.resources   ：

```console
ildasm MyApp.exe /output:MyApp.il
```

下面的命令对 `MyFile.exe` 的 `MyClass` 类中的 `MyMethod` 方法进行反汇编，并将输出显示到控制台窗口中。

```console
ildasm /item:MyClass::MyMethod MyFile.exe /text
```

在上面的示例中，可能有几个具有不同签名的名为 `MyMethod` 的方法。 下面的命令对返回类型为 void 且参数类型为 int32 和 string 的 `MyMethod` 实例方法进行反汇编  。

```console
ildasm /item:"MyClass::MyMethod(instance void(int32,string)" MyFile.exe /text
```

> [!NOTE]
> 在 .NET Framework 1.0 和 1.1 版中，方法名称之后的左括号必须在签名后面匹配一个右括号：`MyMethod(instance void(int32))`。 从 .NET Framework 2.0 开始，必须省略右括号：(`MyMethod(instance void(int32)`。

若要检索 `static` 方法（在 Visual Basic 中为 `Shared` 方法），则省略关键字 `instance`。 不属于基元类型（例如 `int32` 和 `string`）的类类型必须包括命名空间并且必须在前面加上关键字 `class`。 外部类型的前面必须是用方括号括起来的库名称。 下面的命令对名为 `MyMethod` 的静态方法进行反汇编，该方法具有一个 <xref:System.AppDomain> 类型的参数且其返回类型为 <xref:System.AppDomain>。

```console
ildasm /item:"MyClass::MyMethod(class [mscorlib]System.AppDomain(class [mscorlib]System.AppDomain)" MyFile.exe /text
```

嵌套类型的前面必须是包含它的类，并用正斜杠进行分隔。 例如，如果 `MyNamespace.MyClass` 类包含名为 `NestedClass` 的嵌套类，则按如下方式标识该嵌套类：`class MyNamespace.MyClass/NestedClass`。

## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Ilasm.exe（IL 汇编程序）](ilasm-exe-il-assembler.md)
- [托管执行过程](../../standard/managed-execution-process.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
