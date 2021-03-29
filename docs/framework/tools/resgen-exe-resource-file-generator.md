---
title: Resgen.exe（资源文件生成器）
description: 使用 Resgen.exe（资源文件生成器）。 将文本（.txt、.restext）和 XML 资源格式 (.resx) 文件转换为可嵌入的 CLR 运行时二进制文件 (.resources)。
ms.date: 03/30/2017
helpviewer_keywords:
- resource files, .resources files
- resource files, .resx files
- resx files (resource files)
- .resources files
- files, converting
- Resource File Generator
- .resx files
- Resgen.exe
- resource files, converting
- converting files, Resource File Generator
- binary resources files
- embedding files in runtime binary executable
ms.assetid: 8ef159de-b660-4bec-9213-c3fbc4d1c6f4
ms.openlocfilehash: f1ed2fd327511b84ea2751e82c23bf02e283324d
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653863"
---
# <a name="resgenexe-resource-file-generator"></a>Resgen.exe（资源文件生成器）

资源文件生成器 (Resgen.exe) 将文本（.txt 或 .restext）文件和基于 XML 的资源格式 (.resx) 文件转换为公共语言运行时二进制 (.resources) 文件，后者可嵌入到运行时二进制可执行文件或附属程序集中。 （请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)。）  
  
 Resgen.exe 是执行以下任务的通用资源转换实用工具：  
  
- 将 .txt 或 .restext 文件转换为 .resources 或 .resx 文件。 （.restext 文件的格式与 .txt 文件的格式相同。 但是，.restext 扩展名可帮助你更轻松地识别包含资源定义的文本文件。）  
  
- 将 .resources 文件转换为文本文件或 .resx 文件。  
  
- 将 .resx 文件转换为文本文件或 .resources 文件。  
  
- 将程序集中的字符串资源提取到 .resw 文件中，该文件适合在 Windows 8.x 应用商店应用中使用。  
  
- 创建一个强类型类，该类提供对单个名为资源的文件和 <xref:System.Resources.ResourceManager> 实例的访问。  
  
 如果 Resgen.exe 处于任何原因失败，则返回值将为 –1。  
  
 若要获取有关 Resgen.exe 的帮助，可以在不指定选项的情况下使用以下命令显示 Resgen.exe 的命令语法和选项：  
  
```console  
resgen  
```  
  
 也可以使用 `/?` 开关：  
  
```console  
resgen /?  
```  
  
 如果使用 Resgen.exe 生成二进制 .resources 文件，则可以使用语言编译器将二进制文件嵌入到可执行程序集中，或者可以使用[程序集链接器 (Al.exe)](al-exe-assembly-linker.md) 将其编译到附属程序集中。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
resgen  [-define:symbol1[,symbol2,...]] [/useSourcePath] filename.extension  | /compile filename.extension... [outputFilename.extension] [/r:assembly] [/str:lang[,namespace[,class[,file]]] [/publicclass]]
```  
  
```console  
resgen filename.extension [outputDirectory]  
```  
  
## <a name="parameters"></a>参数  
  
|参数或开关|描述|  
|-------------------------|-----------------|  
|`/define:` symbol1[, symbol2,...]|从 .NET Framework 4.5 开始，支持基于文本（.txt 或 .restext）的资源文件中的条件编译。 如果 symbol 对应于 `#ifdef` 构造中的输入文本文件中包含的符号，则关联的字符串资源将包含在 .resources 文件中。 如果输入文本文件包含带符号（此符号未由 `#if !` 开关定义）的 `/define` 语句，则关联的字符串资源将包含在资源文件中。<br /><br /> 如果将 `/define` 与非文本文件一起使用，则它将被忽略。 符号是区分大小写的。<br /><br /> 有关此选项的更多信息，请参阅本主题后面的[条件编译资源](#Conditional)。|  
|`useSourcePath`|指定输入文件的当前目录将用于解析相对文件路径。|  
|`/compile`|使你能够在单个批量操作中指定要转换为多个 .resources 文件的多个 .resx 文件或文本文件。 如果不指定此选项，则只能指定一个输入文件自变量。 输出文件将命名为 filename.resources。<br /><br /> 此选项不能与 `/str:` 选项一起使用。<br /><br /> 有关此选项的更多信息，请参阅本主题后面的[编译或转换多个文件](#Multiple)。|  
|`/r:` `assembly`|从指定的程序集引用元数据。 当转换 .resx 文件并允许 Resgen.exe 序列化或反序列化对象资源时使用此选项。 这类似于 C# 和 Visual Basic 编译器的 `/reference:` 或 `/r:` 选项。|  
|`filename.extension`|指定要转换的输入文件的名称。 如果你使用此表之前呈现的第一个更长的命令行语法，则 `extension` 必须为下列项之一：<br /><br /> .txt 或 .restext<br /> 要转换为 .resources 或 .resx 文件的文本文件。 文本文件只能包含字符串资源。 有关文件格式的信息，请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)中的“文本文件中的资源”部分。<br /><br /> .resx<br /> 要转换为 .resources 或文本（.txt 或 .restext）文件的基于 XML 的资源文件。<br /><br /> .resources<br /> 要转换为 .resx 或文本（.txt 或 .restext）文件的二进制资源文件。<br /><br /> 如果你使用此表之前呈现的第二个更短的命令行语法，则 `extension` 必须是以下项：<br /><br /> .exe 或 .dll<br /> .NET Framework 程序集（可执行文件或库），其字符串资源将提取到用于开发 Windows 8.x 应用商店应用的 .resw 文件中。|  
|`outputFilename.extension`|指定要创建的资源文件的名称和类型。<br /><br /> 在从 .txt、.restext 或 .resx 文件转换到 .resources 文件时，此参数是可选的。 如果未指定 `outputFilename`，则 Resgen.exe 会为输入 `filename` 追加 .resources 扩展名，并将此文件写入包含 `filename,extension` 的目录中。<br /><br /> 从 .resources 文件转换时，`outputFilename.extension` 参数是强制的。 在将 .resources 文件转换为基于 XML 的资源文件时，指定带 .resx 扩展名的文件名。 将 .resources 文件转换为文本文件时，指定带 .txt 或 .restext 扩展名的文件名。 应仅在 .resources 文件仅包含字符串值时将 .resources 文件转换为 .txt 文件。|  
|`outputDirectory`|对于 Windows 8.x 应用商店应用，指定 .resw 文件所在的目录，该文件包含将写入的 `filename.extension` 中的字符串资源。 `outputDirectory` 必须已存在。|  
|`/str:` `language[,namespace[,classname[,filename]]]`|使用 `language` 选项中指定的编程语言创建强类型的资源类文件。 `language` 可包含下列文本之一：<br /><br /> -   对于 C#：`c#`、`cs` 或 `csharp`。<br />-   对于 Visual Basic：`vb` 或 `visualbasic`。<br />-   对于 VBScript：`vbs` 或 `vbscript`。<br />-   对于 C++：`c++`、`mc` 或 `cpp`。<br />-   对于 JavaScript：`js`、`jscript` 或 `javascript`。<br /><br /> `namespace` 选项指定项目的默认命名空间，`classname` 选项指定已生成的类的名称，`filename` 选项指定类文件的名称。<br /><br /> `/str:` 选项只允许一个输入文件，因此该选项不能与 `/compile` 选项一起使用。<br /><br /> 如果指定 `namespace`，但未指定 `classname`，则类名称会从输出文件名派生（例如，下划线将替换句点）。 强类型资源可能无法正常工作。 若要避免此情况，可同时指定类名和输出文件名。<br /><br /> 有关此选项的更多信息，请参阅本主题后面的[生成强类型的资源类](#Strong)。|  
|`/publicClass`|将强类型的资源类作为公共类创建。 默认情况下，资源类是 C# 中的 `internal` 和 Visual Basic 中的`Friend`。<br /><br /> 如果未使用 `/str:` 选项，则忽略此选项。|  
  
## <a name="resgenexe-and-resource-file-types"></a>Resgen.exe 和资源文件类型  

 若要使 Resgen.exe 能够成功转换资源，文本文件和 .resx 文件必须遵循正确的格式。  
  
### <a name="text-txt-and-restext-files"></a>文本（.txt 和 .restext）文件  

 文本（.txt 或 .restext）文件只能包含字符串资源。 如果编写必须具有已翻译成多种语言的字符串的应用程序，则字符串资源很有用。 例如，通过使用适当的字符串资源，可以轻松区域化菜单字符串。 Resgen.exe 读取包含名称/值对的文本文件，其中名称是描述资源的字符串，值是资源字符串本身。  
  
> [!NOTE]
> 有关 .txt 和 .restext 文件的格式信息，请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)中的“文本文件中的资源”部分。  
  
 包含资源的文本文件必须使用 UTF-8 或 Unicode (UTF-16) 编码进行保存，除非它仅包含 Basic Latin 范围（到 U+007F）中的字符。 当 Resgen.exe 处理使用 ANSI 编码保存的文本文件时，它会移除扩展的 ANSI 字符。  
  
 Resgen.exe 检查文本文件中是否存在重复的资源名。 如果文本文件包含重复的资源名，则 Resgen.exe 将发出警告并忽略第二个值。  
  
### <a name="resx-files"></a>.resx 文件  

 .resx 资源文件格式由 XML 项组成。 如同在文本文件中一样，你可以在这些 XML 项内指定字符串资源。 与文本文件相比，.resx 文件的主要优势在于还允许你指定或嵌入对象。 查看 .resx 文件时，如果嵌入对象（如图片）的二进制格式是资源清单的一部分，则可以看见此二进制信息。 如同文本文件一样，可以用文本编辑器（如记事本或 Microsoft Word）打开 .resx 文件，并编写、分析和操作其内容。 请注意，这要求非常熟悉 XML 标记和 .resx 文件结构。 有关 .resx 文件格式的更多详细信息，请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)中的“.resx 文件中的资源”部分。  
  
 若要创建包含嵌入的非字符串对象的 .resources 文件，必须使用 Resgen.exe 转换包含对象的 .resx 文件，或通过调用 <xref:System.Resources.ResourceWriter> 类提供的方法来直接将对象资源添加到文件。  
  
 如果 .resx 或 .resources 文件包含对象并且你使用 Resgen.exe 将其转换为文本文件，则所有字符串资源都将正确转换，但非字符串对象的数据类型也会作为字符串写入该文件。 转换过程中将丢失嵌入的对象，并且 Resgen.exe 在检索资源时将报告发生的错误。  
  
### <a name="converting-between-resources-file-types"></a>在资源文件类型之间转换  

 在不同的资源文件类型之间进行转换时，Resgen.exe 可能无法执行该转换或可能丢失特定资源的相关信息，具体取决于源文件和目标文件的类型。 下表指定在从一个资源文件类型转换到另一个资源文件类型时成功的转换类型。  
  
|转换自|转换为文本文件|转换为 .resx 文件|转换为 .resw 文件|转换为 .resources 文件|  
|------------------|------------------|-------------------|-------------------|------------------------|  
|文本（.txt 或 .restext）文件|--|没有问题|不支持|没有问题|  
|.resx 文件|如果文件包含非字符串资源（包括文件链接），则转换失败。|--|不支持|没有问题|  
|.resources 文件|如果文件包含非字符串资源（包括文件链接），则转换失败。|没有问题|不支持|--|  
|.exe 或 .dll 程序集|不支持|不支持|只有字符串资源（包括路径名）识别为资源|不支持|  
  
## <a name="performing-specific-resgenexe-tasks"></a>执行特定的 Resgen.exe 任务  

 可通过不同的方式使用 Resgen.exe：将基于文本的或基于 XML 的资源文件编译为二进制文件，在资源文件格式之间进行转换以及生成包装 <xref:System.Resources.ResourceManager> 功能并提供对资源的访问的类。 本节提供有关每个任务的详细信息：  
  
- [将资源编译为二进制文件](resgen-exe-resource-file-generator.md#Compiling)  
  
- [在资源文件类型之间转换](resgen-exe-resource-file-generator.md#Convert)  
  
- [编译或转换多个文件](resgen-exe-resource-file-generator.md#Multiple)  
  
- [将资源导入 .resw 文件](resgen-exe-resource-file-generator.md#Exporting)  
  
- [条件编译资源](resgen-exe-resource-file-generator.md#Conditional)  
  
- [生成强类型资源类](resgen-exe-resource-file-generator.md#Strong)  
  
<a name="Compiling"></a>

### <a name="compiling-resources-into-a-binary-file"></a>将资源编译为二进制文件  

 Resgen.exe 的最常见用法是将基于文本的资源文件（.txt 或 .restext 文件）或基于 XML 的资源文件（.resx 文件）编译为二进制 .resources 文件。 然后，输出文件可由语言编译器嵌入到主程序集中，或由[程序集链接器 (AL.exe)](al-exe-assembly-linker.md) 嵌入到附属程序集中。  
  
 用于编译资源文件的语法是：  
  
```console  
resgen inputFilename [outputFilename]
```  
  
 参数的位置是：  
  
 `inputFilename`  
 要编译的资源文件的文件名（包括扩展名）。 Resgen.exe 仅编译扩展名为 .txt、.restext 或 .resx 的文件。  
  
 `outputFilename`  
 输出文件的名称。 如果省略 `outputFilename`，则 Resgen.exe 会在与 `inputFilename` 相同的目录中创建根文件名为 `inputFilename` 的 .resources 文件。 如果 `outputFilename` 包括一个目录路径，则该目录必须存在。  
  
 通过在文件名中指定完全限定的命名空间并通过句点将其与根文件名分隔开，可为 .resources 文件提供该命名空间。 例如，如果 `outputFilename` 是 `MyCompany.Libraries.Strings.resources`，则命名空间为 MyCompany.Libraries。  
  
 下面的命令读取 Resources.txt 中的名称/值对，并编写一个名为 Resources.resources 的二进制 .resources 文件。 由于未显式指定输出文件名，因此默认情况下它将接收与输入文件相同的文件名。  
  
```console  
resgen Resources.txt
```  
  
 下面的命令读取 Resources.restext 中的名称/值对，并编写一个名为 StringResources.resources 的二进制资源文件。  
  
```console  
resgen Resources.restext StringResources.resources  
```  
  
 下面的命令读取名为 Resources.resx 的基于 XML 的输入文件，并编写一个名为 Resources.resources 的二进制 .resources 文件。  
  
```console  
resgen Resources.resx Resources.resources  
```  
  
<a name="Convert"></a>

### <a name="converting-between-resource-file-types"></a>在资源文件类型之间转换  

 除了将基于文本的或基于 XML 的资源文件编译为二进制 .resources 文件之外，Resgen.exe 还可以将任意受支持的文件类型转换为其他任何受支持的文件类型。 这意味着它可以执行以下转换：  
  
- 将 .txt 和 .restext 文件转换为 .resx 文件。  
  
- 将 .resx 文件转换为 .txt 和 .restext 文件。  
  
- 将 .resources 文件转换为 .txt 和 .restext 文件。  
  
- 将 .resources 文件转换为 .resx 文件。  
  
 该语法与上一节中所示的语法相同。  
  
 此外，可以使用 Resgen.exe 将 .NET Framework 程序集中的嵌入资源转换为 Windows 8.x 应用商店应用的 .resw 文件。  
  
 下面的命令读取二进制资源文件 Resources.resources，并编写一个名为 Resources.resx 的基于 XML 的输出文件。  
  
```console  
resgen Resources.resources Resources.resx  
```  
  
 以下命令读取名为 StringResources.txt 的基于文本的资源文件，并编写一个名为 LibraryResources.resx 的基于 XML 的资源文件。 除了包含字符串资源外，.resx 文件还可用于存储非字符串资源。  
  
```console  
resgen StringResources.txt LibraryResources.resx  
```  
  
 以下两个命令读取名为 Resources.resx 的基于 XML 的资源文件，并编写名为 Resources.txt 和 Resources.restext 的文本文件。 请注意，如果 .resx 文件包含任何嵌入的对象，则这些嵌入的对象不会准确地转换为文本文件。  
  
```console  
resgen Resources.resx Resources.txt  
resgen Resources.resx Resources.restext  
```  
  
<a name="Multiple"></a>

### <a name="compiling-or-converting-multiple-files"></a>编译或转换多个文件  

 可以使用 `/compile` 开关通过单次操作将资源文件列表从一种格式转换为另一格式。 语法为：  
  
```console  
resgen /compile filename.extension [filename.extension...]  
```  
  
 以下命令可将三种文件（StringResources.txt、TableResources.resw 和 ImageResources.resw）编译为名为 StringResources.resources、TableResources.resources 和 ImageResources.resources 的单独的 .resources 文件。  
  
```console  
resgen /compile StringResources.txt TableResources.resx ImageResources.resx  
```  
  
<a name="Exporting"></a>

### <a name="exporting-resources-to-a-resw-file"></a>将资源导入 .resw 文件  

 如果正在开发 Windows 8.x 应用商店应用，你可能需要使用现有桌面应用中的资源。 但是，这两种应用程序支持不同的文件格式。 在桌面应用中，文本（.txt 或 .restext）文件或 .resx 文件中的资源将编译为二进制 .resources 文件。 在 Windows 8.x 应用商店应用中，.resw 文件将编译为二进制包资源索引 (PRI) 文件。 可以使用 Resgen.exe 从可执行程序集或附属程序集中提取资源，并将资源写入一个或多个 .resw 文件（在开发 Windows 8.x 应用商店应用时可使用这些文件）中来填补此间隙。  
  
> [!IMPORTANT]
> Visual Studio 自动处理将可移植库中的资源并入 Windows 8.x 应用商店应用中所需的所有转换。 仅需要在 Visual Studio 外部开发 Windows 8.x 应用商店应用的开发人员对直接使用 Resgen.exe 将程序集中的资源转换为 .resw 文件格式感兴趣。  
  
 用于从程序集生成 .resw 文件的语法是：  
  
```console  
resgen filename.extension  [outputDirectory]  
```  
  
 参数的位置是：  
  
 `filename.extension`  
 .NET Framework 程序集的名称（可执行文件或 .DLL）。 如果该文件不包含资源，则 Resgen.exe 不创建任何文件。  
  
 `outputDirectory`  
 要将 .resw 文件写入到的现有目录。 如果省略 `outputDirectory`，则将 .resw 文件写入到当前目录。 Resgen.exe 为程序集中的每个 .resources 文件创建一个 .resw 文件。 .resw 文件的根文件名与 .resources 文件的根名称相同。  
  
 以下命令在 Win8Resources 目录中为嵌入 MyApp.exe 中的每个 .resources 文件创建一个 .resw 文件：  
  
```console  
resgen MyApp.exe Win8Resources  
```  
  
<a name="Conditional"></a>

### <a name="conditionally-compiling-resources"></a>条件编译资源  

 从 .NET Framework 4.5 开始，Resgen.exe 支持在文本（.txt 和 .restext）文件中的字符串资源的条件编译。 这使你能够在多个生成配置中使用单个基于文本的资源文件。  
  
 在 .txt 或 .restext 文件中，可以使用 `#ifdef`…`#endif` 构造来包含二进制 .resources 文件中的资源（如果定义了符号），并可使用 `#if !`...`#endif` 构造来包含资源（如果未定义符号）。 在编译时，随后使用后跟符号的以逗号分隔的列表的 `/define:` 选项来定义符号。 该比较是区分大小写的；`/define` 定义的符号的大熊写必须与要编译的文本文件中的符号的大小写匹配。  
  
 例如，以下名为 UIResources.rext 的文件包含可采用三个值之一的名为 `AppTitle` 的字符串资源，具体取决于是否定义了名为 `PRODUCTION`、`CONSULT` 或 `RETAIL` 的符号。  
  
```text
#ifdef PRODUCTION  
AppTitle=My Software Company Project Manager
#endif  
#ifdef CONSULT  
AppTitle=My Consulting Company Project Manager  
#endif  
#ifdef RETAIL  
AppTitle=My Retail Store Project Manager  
#endif  
FileMenuName=File  
```  
  
 然后，可使用以下命令将该文件编译为二进制 .resources 文件：  
  
```console  
resgen /define:CONSULT UIResources.restext  
```  
  
 这会生成包含两个字符串资源的 .resources 文件。 `AppTitle` 资源的值为“My Consulting Company Project Manager”。  
  
<a name="Strong"></a>

### <a name="generating-a-strongly-typed-resource-class"></a>生成强类型资源类  

 Resgen.exe 支持强类型的资源，它通过创建包含一组静态只读属性的类来封装对资源的访问。 这提供了用于直接调用 <xref:System.Resources.ResourceManager> 类的方法来检索资源的替代方法。 通过在 Resgen.exe（可包装 `/str` 类的功能）中使用 <xref:System.Resources.Tools.StronglyTypedResourceBuilder> 选项，可以启用强类型的资源支持。 在指定 `/str` 选项时，Resgen.exe 的输出是一个包含强类型属性的类，这些属性与输入参数中引用的资源相匹配。 此类提供对已处理的文件中可用的资源的强类型只读访问。  
  
 用于创建强类型资源的语法是：  
  
```console  
resgen inputFilename [outputFilename] /str:language[,namespace,[classname[,filename]]] [/publicClass]  
```  
  
 参数和开关为：  
  
 `inputFilename`  
 要为其生成强类型资源类的资源文件的文件名（包括扩展名）。 该文件可以为基于文本的文件、基于 XML 的文件或二进制 .resources 文件；它可以具有 .txt、.restext、.resw 或 .resources 扩展名。  
  
 `outputFilename`  
 输出文件的名称。 如果 `outputFilename` 包括一个目录路径，则该目录必须存在。 如果省略 `outputFilename`，则 Resgen.exe 会在与 `inputFilename` 相同的目录中创建根文件名为 `inputFilename` 的 .resources 文件。  
  
 `outputFilename` 可以是基于文本的文件、基于 XML 的文件或二进制 .resources 文件。 如果 `outputFilename` 的文件扩展名与 `inputFilename` 的文件扩展名不同，则 Resgen.exe 会执行文件转换。  
  
 如果 `inputFilename` 是 .resources 文件，`outputFilename` 也是 .resources 文件，则 Resgen.exe 会前者。 如果省略 `outputFilename`，则 Resgen.exe 会用相同的 .resources 文件覆盖 `inputFilename`。  
  
 language  
 为强类型资源类生成源代码时要使用的语言。 C# 代码的可能值为 `cs`、`C#` 和 `csharp`；Visual Basic 代码的可能值为 `vb` 和 `visualbasic`；VBScript 代码的可能值为 `vbs` 和 `vbscript`；C++ 代码的可能值为 `c++`、`mc` 和 `cpp`。  
  
 *namespace*  
 包含强类型资源类的命名空间。 .resources 文件和资源类应具有相同的命名空间。 有关在 `outputFilename` 中指定命名空间的信息，请参阅[将资源编译为二进制文件](resgen-exe-resource-file-generator.md#Compiling)。 如果省略 namespace，则资源类不包含在命名空间内。  
  
 classname  
 强类型资源类的名称。 这应对应于 .resources 文件的根名称。 例如，如果 Resgen.exe 生成名为 MyCompany.Libraries.Strings.resources 的 .resources 文件，则强类型资源类的名称为 Strings。 如果省略 classname，则生成的类是从 `outputFilename` 的根名称派生的。 如果省略 `outputFilename`，则生成的类是从 `inputFilename` 的根名称派生的。  
  
 classname 不能包含无效字符（如嵌入的空格）。 如果 classname 包含嵌入的空格，或者从 inputFilename 中生成 classname（默认情况），并且 inputFilename 包含嵌入的空格，则 Resgen.exe 会将所有无效字符替换为下划线 (\_)   。  
  
 *filename*  
 类文件的名称。  
  
 `/publicclass`  
 使强类型资源类成为公共类而不是 `internal`（在 C# 中）或 `Friend`（在 Visual Basic 中）。 这允许从资源嵌入到的程序集外部访问这些资源。  
  
> [!IMPORTANT]
> 创建强类型资源类时，.resources 文件的名称必须与生成的代码的命名空间和类名匹配。 但是，Resgen.exe 允许你指定可生成具有不兼容的名称的 .resources 文件的选项。 若要解决此行为，请在生成输出文件后重命名此文件。  
  
 强类型资源类具有下列成员：  
  
- 无参数构造函数，可用于实例化强类型资源类。  
  
- `static` (C#) 或 `Shared` (Visual Basic) 和只读 `ResourceManager` 属性，该属性返回管理强类型资源的 <xref:System.Resources.ResourceManager> 实例。  
  
- 静态 `Culture` 属性，它允许你设置用于资源检索的区域性。 默认情况下，其值为 `null`，这表示使用了当前 UI 区域性。  
  
- 一个 `static` (C#) 或 `Shared` (Visual Basic) 以及 .resources 文件中的每个资源的只读属性。 属性的名称是该资源的名称。  
  
 例如，下面的命令将名为 StringResources.txt 的资源文件编译为名为 StringResources.resources 的文件，并在可用于访问资源管理器的名为 StringResources.vb 的 Visual Basic 源代码文件中生成名为 `StringResources` 的类。  
  
```console  
resgen StringResources.txt /str:vb,,StringResources
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [桌面应用中的资源](../resources/index.md)
- [创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)
- [Al.exe（程序集链接器）](al-exe-assembly-linker.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
