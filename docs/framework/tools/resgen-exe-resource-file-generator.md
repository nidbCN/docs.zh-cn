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
ms.openlocfilehash: 61ae4503876718e63993af5a180dead34540afde
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259102"
---
# <a name="resgenexe-resource-file-generator"></a><span data-ttu-id="3c65d-104">Resgen.exe（资源文件生成器）</span><span class="sxs-lookup"><span data-stu-id="3c65d-104">Resgen.exe (Resource File Generator)</span></span>

<span data-ttu-id="3c65d-105">资源文件生成器 (Resgen.exe) 将文本（.txt 或 .restext）文件和基于 XML 的资源格式 (.resx) 文件转换为公共语言运行时二进制 (.resources) 文件，后者可嵌入到运行时二进制可执行文件或附属程序集中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-105">The Resource File Generator (Resgen.exe) converts text (.txt or .restext) files and XML-based resource format (.resx) files to common language runtime binary (.resources) files that can be embedded in a runtime binary executable or satellite assembly.</span></span> <span data-ttu-id="3c65d-106">（请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)。）</span><span class="sxs-lookup"><span data-stu-id="3c65d-106">(See [Creating Resource Files](../resources/creating-resource-files-for-desktop-apps.md).)</span></span>  
  
 <span data-ttu-id="3c65d-107">Resgen.exe 是执行以下任务的通用资源转换实用工具：</span><span class="sxs-lookup"><span data-stu-id="3c65d-107">Resgen.exe is a general-purpose resource conversion utility that performs the following tasks:</span></span>  
  
- <span data-ttu-id="3c65d-108">将 .txt 或 .restext 文件转换为 .resources 或 .resx 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-108">Converts .txt or .restext files to .resources or .resx files.</span></span> <span data-ttu-id="3c65d-109">（.restext 文件的格式与 .txt 文件的格式相同。</span><span class="sxs-lookup"><span data-stu-id="3c65d-109">(The format of .restext files is identical to the format of .txt files.</span></span> <span data-ttu-id="3c65d-110">但是，.restext 扩展名可帮助你更轻松地识别包含资源定义的文本文件。）</span><span class="sxs-lookup"><span data-stu-id="3c65d-110">However, the .restext extension helps you identify text files that contain resource definitions more easily.)</span></span>  
  
- <span data-ttu-id="3c65d-111">将 .resources 文件转换为文本文件或 .resx 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-111">Converts .resources files to text or .resx files.</span></span>  
  
- <span data-ttu-id="3c65d-112">将 .resx 文件转换为文本文件或 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-112">Converts .resx files to text or .resources files.</span></span>  
  
- <span data-ttu-id="3c65d-113">将程序集中的字符串资源提取到 .resw 文件中，该文件适合在 Windows 8.x 应用商店应用中使用。</span><span class="sxs-lookup"><span data-stu-id="3c65d-113">Extracts the string resources from an assembly into a .resw file that is suitable for use in a Windows 8.x Store app.</span></span>  
  
- <span data-ttu-id="3c65d-114">创建一个强类型类，该类提供对单个名为资源的文件和 <xref:System.Resources.ResourceManager> 实例的访问。</span><span class="sxs-lookup"><span data-stu-id="3c65d-114">Creates a strongly typed class that provides access to individual named resources and to the <xref:System.Resources.ResourceManager> instance.</span></span>  
  
 <span data-ttu-id="3c65d-115">如果 Resgen.exe 处于任何原因失败，则返回值将为 –1。</span><span class="sxs-lookup"><span data-stu-id="3c65d-115">If Resgen.exe fails for any reason, the return value is –1.</span></span>  
  
 <span data-ttu-id="3c65d-116">若要获取有关 Resgen.exe 的帮助，可以在不指定选项的情况下使用以下命令显示 Resgen.exe 的命令语法和选项：</span><span class="sxs-lookup"><span data-stu-id="3c65d-116">To get help with Resgen.exe, you can use the following command, with no options specified, to display the command syntax and options for Resgen.exe:</span></span>  
  
```console  
resgen  
```  
  
 <span data-ttu-id="3c65d-117">也可以使用 `/?` 开关：</span><span class="sxs-lookup"><span data-stu-id="3c65d-117">You can also use the `/?` switch:</span></span>  
  
```console  
resgen /?  
```  
  
 <span data-ttu-id="3c65d-118">如果使用 Resgen.exe 生成二进制 .resources 文件，则可以使用语言编译器将二进制文件嵌入到可执行程序集中，或者可以使用[程序集链接器 (Al.exe)](al-exe-assembly-linker.md) 将其编译到附属程序集中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-118">If you use Resgen.exe to generate binary .resources files, you can use a language compiler to embed the binary files into executable assemblies, or you can use the [Assembly Linker (Al.exe)](al-exe-assembly-linker.md) to compile them into satellite assemblies.</span></span>  
  
 <span data-ttu-id="3c65d-119">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="3c65d-119">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="3c65d-120">若要运行该工具，请[对开发人员使用命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="3c65d-120">To run the tool, use a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell).</span></span>  
  
 <span data-ttu-id="3c65d-121">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="3c65d-121">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="3c65d-122">语法</span><span class="sxs-lookup"><span data-stu-id="3c65d-122">Syntax</span></span>  
  
```console  
resgen  [-define:symbol1[,symbol2,...]] [/useSourcePath] filename.extension  | /compile filename.extension... [outputFilename.extension] [/r:assembly] [/str:lang[,namespace[,class[,file]]] [/publicclass]]
```  
  
```console  
resgen filename.extension [outputDirectory]  
```  
  
## <a name="parameters"></a><span data-ttu-id="3c65d-123">参数</span><span class="sxs-lookup"><span data-stu-id="3c65d-123">Parameters</span></span>  
  
|<span data-ttu-id="3c65d-124">参数或开关</span><span class="sxs-lookup"><span data-stu-id="3c65d-124">Parameter or switch</span></span>|<span data-ttu-id="3c65d-125">描述</span><span class="sxs-lookup"><span data-stu-id="3c65d-125">Description</span></span>|  
|-------------------------|-----------------|  
|<span data-ttu-id="3c65d-126">`/define:` symbol1[, symbol2,...]</span><span class="sxs-lookup"><span data-stu-id="3c65d-126">`/define:` *symbol1*[, *symbol2*,...]</span></span>|<span data-ttu-id="3c65d-127">从 .NET Framework 4.5 开始，支持基于文本（.txt 或 .restext）的资源文件中的条件编译。</span><span class="sxs-lookup"><span data-stu-id="3c65d-127">Starting with the .NET Framework 4.5, supports conditional compilation in text-based (.txt or .restext) resource files.</span></span> <span data-ttu-id="3c65d-128">如果 symbol 对应于 `#ifdef` 构造中的输入文本文件中包含的符号，则关联的字符串资源将包含在 .resources 文件中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-128">If *symbol* corresponds to a symbol included in the input text file within a `#ifdef` construct, the associated string resource is included in the .resources file.</span></span> <span data-ttu-id="3c65d-129">如果输入文本文件包含带符号（此符号未由 `#if !` 开关定义）的 `/define` 语句，则关联的字符串资源将包含在资源文件中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-129">If the input text file includes an `#if !` statement with a symbol that is not defined by the `/define` switch, the associated string resource is included in the resources file.</span></span><br /><br /> <span data-ttu-id="3c65d-130">如果将 `/define` 与非文本文件一起使用，则它将被忽略。</span><span class="sxs-lookup"><span data-stu-id="3c65d-130">`/define` is ignored if it is used with non-text files.</span></span> <span data-ttu-id="3c65d-131">符号是区分大小写的。</span><span class="sxs-lookup"><span data-stu-id="3c65d-131">Symbols are case-sensitive.</span></span><br /><br /> <span data-ttu-id="3c65d-132">有关此选项的更多信息，请参阅本主题后面的[条件编译资源](#Conditional)。</span><span class="sxs-lookup"><span data-stu-id="3c65d-132">For more information about this option, see [Conditionally Compiling Resources](#Conditional) later in this topic.</span></span>|  
|`useSourcePath`|<span data-ttu-id="3c65d-133">指定输入文件的当前目录将用于解析相对文件路径。</span><span class="sxs-lookup"><span data-stu-id="3c65d-133">Specifies that the input file's current directory is to be used to resolve relative file paths.</span></span>|  
|`/compile`|<span data-ttu-id="3c65d-134">使你能够在单个批量操作中指定要转换为多个 .resources 文件的多个 .resx 文件或文本文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-134">Enables you to specify multiple .resx or text files to convert to multiple .resources files in a single bulk operation.</span></span> <span data-ttu-id="3c65d-135">如果不指定此选项，则只能指定一个输入文件自变量。</span><span class="sxs-lookup"><span data-stu-id="3c65d-135">If you do not specify this option, you can specify only one input file argument.</span></span> <span data-ttu-id="3c65d-136">输出文件将命名为 filename.resources。</span><span class="sxs-lookup"><span data-stu-id="3c65d-136">Output files are named *filename*.resources.</span></span><br /><br /> <span data-ttu-id="3c65d-137">此选项不能与 `/str:` 选项一起使用。</span><span class="sxs-lookup"><span data-stu-id="3c65d-137">This option cannot be used with the `/str:` option.</span></span><br /><br /> <span data-ttu-id="3c65d-138">有关此选项的更多信息，请参阅本主题后面的[编译或转换多个文件](#Multiple)。</span><span class="sxs-lookup"><span data-stu-id="3c65d-138">For more information about this option, see [Compiling or Converting Multiple Files](#Multiple) later in this topic.</span></span>|  
|<span data-ttu-id="3c65d-139">`/r:` `assembly`</span><span class="sxs-lookup"><span data-stu-id="3c65d-139">`/r:` `assembly`</span></span>|<span data-ttu-id="3c65d-140">从指定的程序集引用元数据。</span><span class="sxs-lookup"><span data-stu-id="3c65d-140">References metadata from the specified assembly.</span></span> <span data-ttu-id="3c65d-141">当转换 .resx 文件并允许 Resgen.exe 序列化或反序列化对象资源时使用此选项。</span><span class="sxs-lookup"><span data-stu-id="3c65d-141">It is used when converting .resx files and allows Resgen.exe to serialize or deserialize object resources.</span></span> <span data-ttu-id="3c65d-142">这类似于 C# 和 Visual Basic 编译器的 `/reference:` 或 `/r:` 选项。</span><span class="sxs-lookup"><span data-stu-id="3c65d-142">It is similar to the `/reference:` or `/r:` options for the C# and Visual Basic compilers.</span></span>|  
|`filename.extension`|<span data-ttu-id="3c65d-143">指定要转换的输入文件的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-143">Specifies the name of the input file to convert.</span></span> <span data-ttu-id="3c65d-144">如果你使用此表之前呈现的第一个更长的命令行语法，则 `extension` 必须为下列项之一：</span><span class="sxs-lookup"><span data-stu-id="3c65d-144">If you're using the first, lengthier command-line syntax presented before this table,  `extension` must be one of the following:</span></span><br /><br /> <span data-ttu-id="3c65d-145">.txt 或 .restext</span><span class="sxs-lookup"><span data-stu-id="3c65d-145">.txt or .restext</span></span><br /> <span data-ttu-id="3c65d-146">要转换为 .resources 或 .resx 文件的文本文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-146">A text file to convert to a .resources or a .resx file.</span></span> <span data-ttu-id="3c65d-147">文本文件只能包含字符串资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-147">Text files can contain only string resources.</span></span> <span data-ttu-id="3c65d-148">有关文件格式的信息，请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)中的“文本文件中的资源”部分。</span><span class="sxs-lookup"><span data-stu-id="3c65d-148">For information about the file format, see the "Resources in Text Files" section of [Creating Resource Files](../resources/creating-resource-files-for-desktop-apps.md).</span></span><br /><br /> <span data-ttu-id="3c65d-149">.resx</span><span class="sxs-lookup"><span data-stu-id="3c65d-149">.resx</span></span><br /> <span data-ttu-id="3c65d-150">要转换为 .resources 或文本（.txt 或 .restext）文件的基于 XML 的资源文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-150">An XML-based resource file to convert to a .resources or a text (.txt or .restext) file.</span></span><br /><br /> <span data-ttu-id="3c65d-151">.resources</span><span class="sxs-lookup"><span data-stu-id="3c65d-151">.resources</span></span><br /> <span data-ttu-id="3c65d-152">要转换为 .resx 或文本（.txt 或 .restext）文件的二进制资源文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-152">A binary resource file to convert to a .resx or a text (.txt or .restext) file.</span></span><br /><br /> <span data-ttu-id="3c65d-153">如果你使用此表之前呈现的第二个更短的命令行语法，则 `extension` 必须是以下项：</span><span class="sxs-lookup"><span data-stu-id="3c65d-153">If you're using the second, shorter command-line syntax presented before this table, `extension` must be the following:</span></span><br /><br /> <span data-ttu-id="3c65d-154">.exe 或 .dll</span><span class="sxs-lookup"><span data-stu-id="3c65d-154">.exe or .dll</span></span><br /> <span data-ttu-id="3c65d-155">.NET Framework 程序集（可执行文件或库），其字符串资源将提取到用于开发 Windows 8.x 应用商店应用的 .resw 文件中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-155">A .NET Framework assembly (executable or library) whose string resources are to be extracted to a .resw file for use in developing Windows 8.x Store apps.</span></span>|  
|`outputFilename.extension`|<span data-ttu-id="3c65d-156">指定要创建的资源文件的名称和类型。</span><span class="sxs-lookup"><span data-stu-id="3c65d-156">Specifies the name and type of the resource file to create.</span></span><br /><br /> <span data-ttu-id="3c65d-157">在从 .txt、.restext 或 .resx 文件转换到 .resources 文件时，此参数是可选的。</span><span class="sxs-lookup"><span data-stu-id="3c65d-157">This argument is optional when converting from a .txt, .restext, or .resx file to a .resources file.</span></span> <span data-ttu-id="3c65d-158">如果未指定 `outputFilename`，则 Resgen.exe 会为输入 `filename` 追加 .resources 扩展名，并将此文件写入包含 `filename,extension` 的目录中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-158">If you do not specify `outputFilename`, Resgen.exe appends a .resources extension to the input `filename` and writes the file to the directory that contains `filename,extension`.</span></span><br /><br /> <span data-ttu-id="3c65d-159">从 .resources 文件转换时，`outputFilename.extension` 参数是强制的。</span><span class="sxs-lookup"><span data-stu-id="3c65d-159">The `outputFilename.extension` argument is mandatory when converting from a .resources file.</span></span> <span data-ttu-id="3c65d-160">在将 .resources 文件转换为基于 XML 的资源文件时，指定带 .resx 扩展名的文件名。</span><span class="sxs-lookup"><span data-stu-id="3c65d-160">Specify a file name with the .resx extension when converting a .resources file to an XML-based resource file.</span></span> <span data-ttu-id="3c65d-161">将 .resources 文件转换为文本文件时，指定带 .txt 或 .restext 扩展名的文件名。</span><span class="sxs-lookup"><span data-stu-id="3c65d-161">Specify a file name with the .txt or .restext extension when converting a .resources file to a text file.</span></span> <span data-ttu-id="3c65d-162">应仅在 .resources 文件仅包含字符串值时将 .resources 文件转换为 .txt 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-162">You should convert a .resources file to a .txt file only when the .resources file contains only string values.</span></span>|  
|`outputDirectory`|<span data-ttu-id="3c65d-163">对于 Windows 8.x 应用商店应用，指定 .resw 文件所在的目录，该文件包含将写入的 `filename.extension` 中的字符串资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-163">For Windows 8.x Store apps, specifies the directory in which a .resw file that contains the string resources in `filename.extension` will be written.</span></span> <span data-ttu-id="3c65d-164">`outputDirectory` 必须已存在。</span><span class="sxs-lookup"><span data-stu-id="3c65d-164">`outputDirectory` must already exist.</span></span>|  
|<span data-ttu-id="3c65d-165">`/str:` `language[,namespace[,classname[,filename]]]`</span><span class="sxs-lookup"><span data-stu-id="3c65d-165">`/str:` `language[,namespace[,classname[,filename]]]`</span></span>|<span data-ttu-id="3c65d-166">使用 `language` 选项中指定的编程语言创建强类型的资源类文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-166">Creates a strongly typed resource class file in the programming language specified in the `language` option.</span></span> <span data-ttu-id="3c65d-167">`language` 可包含下列文本之一：</span><span class="sxs-lookup"><span data-stu-id="3c65d-167">`language` can consist of one of the following literals:</span></span><br /><br /> <span data-ttu-id="3c65d-168">-   对于 C#：`c#`、`cs` 或 `csharp`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-168">-   For C#: `c#`, `cs`, or `csharp`.</span></span><br /><span data-ttu-id="3c65d-169">-   对于 Visual Basic：`vb` 或 `visualbasic`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-169">-   For Visual Basic: `vb` or `visualbasic`.</span></span><br /><span data-ttu-id="3c65d-170">-   对于 VBScript：`vbs` 或 `vbscript`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-170">-   For VBScript: `vbs` or `vbscript`.</span></span><br /><span data-ttu-id="3c65d-171">-   对于 C++：`c++`、`mc` 或 `cpp`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-171">-   For C++: `c++`, `mc`, or `cpp`.</span></span><br /><span data-ttu-id="3c65d-172">-   对于 JavaScript：`js`、`jscript` 或 `javascript`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-172">-   For JavaScript: `js`, `jscript`, or `javascript`.</span></span><br /><br /> <span data-ttu-id="3c65d-173">`namespace` 选项指定项目的默认命名空间，`classname` 选项指定已生成的类的名称，`filename` 选项指定类文件的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-173">The `namespace` option specifies the project's default namespace, the `classname` option specifies the name of the generated class, and the `filename` option specifies the name of the class file.</span></span><br /><br /> <span data-ttu-id="3c65d-174">`/str:` 选项只允许一个输入文件，因此该选项不能与 `/compile` 选项一起使用。</span><span class="sxs-lookup"><span data-stu-id="3c65d-174">The `/str:` option allows only one input file, so it cannot be used with the `/compile` option.</span></span><br /><br /> <span data-ttu-id="3c65d-175">如果指定 `namespace`，但未指定 `classname`，则类名称会从输出文件名派生（例如，下划线将替换句点）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-175">If `namespace` is specified but `classname` is not, the class name is derived from the output file name (for example, underscores are substituted for periods).</span></span> <span data-ttu-id="3c65d-176">强类型资源可能无法正常工作。</span><span class="sxs-lookup"><span data-stu-id="3c65d-176">The strongly typed resources might not work correctly as a result.</span></span> <span data-ttu-id="3c65d-177">若要避免此情况，可同时指定类名和输出文件名。</span><span class="sxs-lookup"><span data-stu-id="3c65d-177">To avoid this, specify both class name and output file name.</span></span><br /><br /> <span data-ttu-id="3c65d-178">有关此选项的更多信息，请参阅本主题后面的[生成强类型的资源类](#Strong)。</span><span class="sxs-lookup"><span data-stu-id="3c65d-178">For more information about this option, see [Generating a Strongly Typed Resource Class](#Strong) later in this topic.</span></span>|  
|`/publicClass`|<span data-ttu-id="3c65d-179">将强类型的资源类作为公共类创建。</span><span class="sxs-lookup"><span data-stu-id="3c65d-179">Creates a strongly typed resource class as a public class.</span></span> <span data-ttu-id="3c65d-180">默认情况下，资源类是 C# 中的 `internal` 和 Visual Basic 中的`Friend`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-180">By default, the resource class is `internal` in C# and `Friend` in Visual Basic.</span></span><br /><br /> <span data-ttu-id="3c65d-181">如果未使用 `/str:` 选项，则忽略此选项。</span><span class="sxs-lookup"><span data-stu-id="3c65d-181">This option is ignored if the `/str:` option is not used.</span></span>|  
  
## <a name="resgenexe-and-resource-file-types"></a><span data-ttu-id="3c65d-182">Resgen.exe 和资源文件类型</span><span class="sxs-lookup"><span data-stu-id="3c65d-182">Resgen.exe and Resource File Types</span></span>  

 <span data-ttu-id="3c65d-183">若要使 Resgen.exe 能够成功转换资源，文本文件和 .resx 文件必须遵循正确的格式。</span><span class="sxs-lookup"><span data-stu-id="3c65d-183">In order for Resgen.exe to successfully convert resources, text and .resx files must follow the correct format.</span></span>  
  
### <a name="text-txt-and-restext-files"></a><span data-ttu-id="3c65d-184">文本（.txt 和 .restext）文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-184">Text (.txt and .restext) Files</span></span>  

 <span data-ttu-id="3c65d-185">文本（.txt 或 .restext）文件只能包含字符串资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-185">Text (.txt or .restext) files may contain only string resources.</span></span> <span data-ttu-id="3c65d-186">如果编写必须具有已翻译成多种语言的字符串的应用程序，则字符串资源很有用。</span><span class="sxs-lookup"><span data-stu-id="3c65d-186">String resources are useful if you are writing an application that must have strings translated into several languages.</span></span> <span data-ttu-id="3c65d-187">例如，通过使用适当的字符串资源，可以轻松区域化菜单字符串。</span><span class="sxs-lookup"><span data-stu-id="3c65d-187">For example, you can easily regionalize menu strings by using the appropriate string resource.</span></span> <span data-ttu-id="3c65d-188">Resgen.exe 读取包含名称/值对的文本文件，其中名称是描述资源的字符串，值是资源字符串本身。</span><span class="sxs-lookup"><span data-stu-id="3c65d-188">Resgen.exe reads text files that contain name/value pairs, where the name is a string that describes the resource and the value is the resource string itself.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="3c65d-189">有关 .txt 和 .restext 文件的格式信息，请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)中的“文本文件中的资源”部分。</span><span class="sxs-lookup"><span data-stu-id="3c65d-189">For information about the format of .txt and .restext files, see the "Resources in Text Files" section of [Creating Resource Files](../resources/creating-resource-files-for-desktop-apps.md).</span></span>  
  
 <span data-ttu-id="3c65d-190">包含资源的文本文件必须使用 UTF-8 或 Unicode (UTF-16) 编码进行保存，除非它仅包含 Basic Latin 范围（到 U+007F）中的字符。</span><span class="sxs-lookup"><span data-stu-id="3c65d-190">A text file that contains resources must be saved with UTF-8 or Unicode (UTF-16) encoding unless it contains only characters in the Basic Latin range (to U+007F).</span></span> <span data-ttu-id="3c65d-191">当 Resgen.exe 处理使用 ANSI 编码保存的文本文件时，它会移除扩展的 ANSI 字符。</span><span class="sxs-lookup"><span data-stu-id="3c65d-191">Resgen.exe removes extended ANSI characters when it processes a text file that is saved using ANSI encoding.</span></span>  
  
 <span data-ttu-id="3c65d-192">Resgen.exe 检查文本文件中是否存在重复的资源名。</span><span class="sxs-lookup"><span data-stu-id="3c65d-192">Resgen.exe checks the text file for duplicate resource names.</span></span> <span data-ttu-id="3c65d-193">如果文本文件包含重复的资源名，则 Resgen.exe 将发出警告并忽略第二个值。</span><span class="sxs-lookup"><span data-stu-id="3c65d-193">If the text file contains duplicate resource names, Resgen.exe will emit a warning and ignore the second value.</span></span>  
  
### <a name="resx-files"></a><span data-ttu-id="3c65d-194">.resx 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-194">.resx Files</span></span>  

 <span data-ttu-id="3c65d-195">.resx 资源文件格式由 XML 项组成。</span><span class="sxs-lookup"><span data-stu-id="3c65d-195">The .resx resource file format consists of XML entries.</span></span> <span data-ttu-id="3c65d-196">如同在文本文件中一样，你可以在这些 XML 项内指定字符串资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-196">You can specify string resources within these XML entries, as you would in text files.</span></span> <span data-ttu-id="3c65d-197">与文本文件相比，.resx 文件的主要优势在于还允许你指定或嵌入对象。</span><span class="sxs-lookup"><span data-stu-id="3c65d-197">A primary advantage of .resx files over text files is that you can also specify or embed objects.</span></span> <span data-ttu-id="3c65d-198">查看 .resx 文件时，如果嵌入对象（如图片）的二进制格式是资源清单的一部分，则可以看见此二进制信息。</span><span class="sxs-lookup"><span data-stu-id="3c65d-198">When you view a .resx file, you can see the binary form of an embedded object (for example, a picture) when this binary information is a part of the resource manifest.</span></span> <span data-ttu-id="3c65d-199">如同文本文件一样，可以用文本编辑器（如记事本或 Microsoft Word）打开 .resx 文件，并编写、分析和操作其内容。</span><span class="sxs-lookup"><span data-stu-id="3c65d-199">As with text files, you can open a .resx file with a text editor (such as Notepad or Microsoft Word) and write, parse, and manipulate its contents.</span></span> <span data-ttu-id="3c65d-200">请注意，这要求非常熟悉 XML 标记和 .resx 文件结构。</span><span class="sxs-lookup"><span data-stu-id="3c65d-200">Note that this requires a good knowledge of XML tags and the .resx file structure.</span></span> <span data-ttu-id="3c65d-201">有关 .resx 文件格式的更多详细信息，请参阅[创建资源文件](../resources/creating-resource-files-for-desktop-apps.md)中的“.resx 文件中的资源”部分。</span><span class="sxs-lookup"><span data-stu-id="3c65d-201">For more details on the .resx file format, see the "Resources in .resx Files" section of [Creating Resource Files](../resources/creating-resource-files-for-desktop-apps.md).</span></span>  
  
 <span data-ttu-id="3c65d-202">若要创建包含嵌入的非字符串对象的 .resources 文件，必须使用 Resgen.exe 转换包含对象的 .resx 文件，或通过调用 <xref:System.Resources.ResourceWriter> 类提供的方法来直接将对象资源添加到文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-202">In order to create a .resources file that contains embedded nonstring objects, you must either use Resgen.exe to convert a .resx file containing objects or add the object resources to your file directly from code by calling the methods provided by the <xref:System.Resources.ResourceWriter> class.</span></span>  
  
 <span data-ttu-id="3c65d-203">如果 .resx 或 .resources 文件包含对象并且你使用 Resgen.exe 将其转换为文本文件，则所有字符串资源都将正确转换，但非字符串对象的数据类型也会作为字符串写入该文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-203">If your .resx or .resources file contains objects and you use Resgen.exe to convert it to a text file, all the string resources will be converted correctly, but the data types of the nonstring objects will also be written to the file as strings.</span></span> <span data-ttu-id="3c65d-204">转换过程中将丢失嵌入的对象，并且 Resgen.exe 在检索资源时将报告发生的错误。</span><span class="sxs-lookup"><span data-stu-id="3c65d-204">You will lose the embedded objects in the conversion, and Resgen.exe will report that an error occurred in retrieving the resources.</span></span>  
  
### <a name="converting-between-resources-file-types"></a><span data-ttu-id="3c65d-205">在资源文件类型之间转换</span><span class="sxs-lookup"><span data-stu-id="3c65d-205">Converting Between Resources File Types</span></span>  

 <span data-ttu-id="3c65d-206">在不同的资源文件类型之间进行转换时，Resgen.exe 可能无法执行该转换或可能丢失特定资源的相关信息，具体取决于源文件和目标文件的类型。</span><span class="sxs-lookup"><span data-stu-id="3c65d-206">When you convert between different resource file types, Resgen.exe may not be able to perform the conversion or may lose information about specific resources, depending on the source and target file types.</span></span> <span data-ttu-id="3c65d-207">下表指定在从一个资源文件类型转换到另一个资源文件类型时成功的转换类型。</span><span class="sxs-lookup"><span data-stu-id="3c65d-207">The following table specifies the types of conversions that are successful when converting from one resource file type to another.</span></span>  
  
|<span data-ttu-id="3c65d-208">转换自</span><span class="sxs-lookup"><span data-stu-id="3c65d-208">Convert from</span></span>|<span data-ttu-id="3c65d-209">转换为文本文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-209">To text file</span></span>|<span data-ttu-id="3c65d-210">转换为 .resx 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-210">To .resx file</span></span>|<span data-ttu-id="3c65d-211">转换为 .resw 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-211">To .resw file</span></span>|<span data-ttu-id="3c65d-212">转换为 .resources 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-212">To .resources file</span></span>|  
|------------------|------------------|-------------------|-------------------|------------------------|  
|<span data-ttu-id="3c65d-213">文本（.txt 或 .restext）文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-213">Text (.txt or .restext) file</span></span>|--|<span data-ttu-id="3c65d-214">没有问题</span><span class="sxs-lookup"><span data-stu-id="3c65d-214">No issues</span></span>|<span data-ttu-id="3c65d-215">不支持</span><span class="sxs-lookup"><span data-stu-id="3c65d-215">Not supported</span></span>|<span data-ttu-id="3c65d-216">没有问题</span><span class="sxs-lookup"><span data-stu-id="3c65d-216">No issues</span></span>|  
|<span data-ttu-id="3c65d-217">.resx 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-217">.resx file</span></span>|<span data-ttu-id="3c65d-218">如果文件包含非字符串资源（包括文件链接），则转换失败。</span><span class="sxs-lookup"><span data-stu-id="3c65d-218">Conversion fails if file contains non-string resources (including file links)</span></span>|--|<span data-ttu-id="3c65d-219">不支持</span><span class="sxs-lookup"><span data-stu-id="3c65d-219">Not supported</span></span>|<span data-ttu-id="3c65d-220">没有问题</span><span class="sxs-lookup"><span data-stu-id="3c65d-220">No issues</span></span>|  
|<span data-ttu-id="3c65d-221">.resources 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-221">.resources file</span></span>|<span data-ttu-id="3c65d-222">如果文件包含非字符串资源（包括文件链接），则转换失败。</span><span class="sxs-lookup"><span data-stu-id="3c65d-222">Conversion fails if file contains non-string resources (including file links)</span></span>|<span data-ttu-id="3c65d-223">没有问题</span><span class="sxs-lookup"><span data-stu-id="3c65d-223">No issues</span></span>|<span data-ttu-id="3c65d-224">不支持</span><span class="sxs-lookup"><span data-stu-id="3c65d-224">Not supported</span></span>|--|  
|<span data-ttu-id="3c65d-225">.exe 或 .dll 程序集</span><span class="sxs-lookup"><span data-stu-id="3c65d-225">.exe or .dll assembly</span></span>|<span data-ttu-id="3c65d-226">不支持</span><span class="sxs-lookup"><span data-stu-id="3c65d-226">Not supported</span></span>|<span data-ttu-id="3c65d-227">不支持</span><span class="sxs-lookup"><span data-stu-id="3c65d-227">Not supported</span></span>|<span data-ttu-id="3c65d-228">只有字符串资源（包括路径名）识别为资源</span><span class="sxs-lookup"><span data-stu-id="3c65d-228">Only string resources (including path names) are recognized as resources</span></span>|<span data-ttu-id="3c65d-229">不支持</span><span class="sxs-lookup"><span data-stu-id="3c65d-229">Not supported</span></span>|  
  
## <a name="performing-specific-resgenexe-tasks"></a><span data-ttu-id="3c65d-230">执行特定的 Resgen.exe 任务</span><span class="sxs-lookup"><span data-stu-id="3c65d-230">Performing Specific Resgen.exe Tasks</span></span>  

 <span data-ttu-id="3c65d-231">可通过不同的方式使用 Resgen.exe：将基于文本的或基于 XML 的资源文件编译为二进制文件，在资源文件格式之间进行转换以及生成包装 <xref:System.Resources.ResourceManager> 功能并提供对资源的访问的类。</span><span class="sxs-lookup"><span data-stu-id="3c65d-231">You can use Resgen.exe in diverse ways: to compile a text-based or XML-based resource file into a binary file, to convert between resource file formats, and to generate a class that wraps <xref:System.Resources.ResourceManager> functionality and provides access to resources.</span></span> <span data-ttu-id="3c65d-232">本节提供有关每个任务的详细信息：</span><span class="sxs-lookup"><span data-stu-id="3c65d-232">This section provides detailed information about each task:</span></span>  
  
- [<span data-ttu-id="3c65d-233">将资源编译为二进制文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-233">Compiling Resources into a Binary File</span></span>](resgen-exe-resource-file-generator.md#Compiling)  
  
- [<span data-ttu-id="3c65d-234">在资源文件类型之间转换</span><span class="sxs-lookup"><span data-stu-id="3c65d-234">Converting Between Resource File Types</span></span>](resgen-exe-resource-file-generator.md#Convert)  
  
- [<span data-ttu-id="3c65d-235">编译或转换多个文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-235">Compiling or Converting Multiple Files</span></span>](resgen-exe-resource-file-generator.md#Multiple)  
  
- [<span data-ttu-id="3c65d-236">将资源导入 .resw 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-236">Exporting Resources to a .resw File</span></span>](resgen-exe-resource-file-generator.md#Exporting)  
  
- [<span data-ttu-id="3c65d-237">条件编译资源</span><span class="sxs-lookup"><span data-stu-id="3c65d-237">Conditionally Compiling Resources</span></span>](resgen-exe-resource-file-generator.md#Conditional)  
  
- [<span data-ttu-id="3c65d-238">生成强类型资源类</span><span class="sxs-lookup"><span data-stu-id="3c65d-238">Generating a Strongly Typed Resource Class</span></span>](resgen-exe-resource-file-generator.md#Strong)  
  
<a name="Compiling"></a>

### <a name="compiling-resources-into-a-binary-file"></a><span data-ttu-id="3c65d-239">将资源编译为二进制文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-239">Compiling Resources into a Binary File</span></span>  

 <span data-ttu-id="3c65d-240">Resgen.exe 的最常见用法是将基于文本的资源文件（.txt 或 .restext 文件）或基于 XML 的资源文件（.resx 文件）编译为二进制 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-240">The most common use of Resgen.exe is to compile a text-based resource file (a .txt or .restext file) or an XML-based resource file (a .resx file) into a binary .resources file.</span></span> <span data-ttu-id="3c65d-241">然后，输出文件可由语言编译器嵌入到主程序集中，或由[程序集链接器 (AL.exe)](al-exe-assembly-linker.md) 嵌入到附属程序集中。</span><span class="sxs-lookup"><span data-stu-id="3c65d-241">The output file then can be embedded in a main assembly by a language compiler or in a satellite assembly by [Assembly Linker (AL.exe)](al-exe-assembly-linker.md).</span></span>  
  
 <span data-ttu-id="3c65d-242">用于编译资源文件的语法是：</span><span class="sxs-lookup"><span data-stu-id="3c65d-242">The syntax to compile a resource file is:</span></span>  
  
```console  
resgen inputFilename [outputFilename]
```  
  
 <span data-ttu-id="3c65d-243">参数的位置是：</span><span class="sxs-lookup"><span data-stu-id="3c65d-243">where the parameters are:</span></span>  
  
 `inputFilename`  
 <span data-ttu-id="3c65d-244">要编译的资源文件的文件名（包括扩展名）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-244">The file name, including the extension, of the resource file to compile.</span></span> <span data-ttu-id="3c65d-245">Resgen.exe 仅编译扩展名为 .txt、.restext 或 .resx 的文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-245">Resgen.exe only compiles files with extensions of .txt, .restext, or .resx.</span></span>  
  
 `outputFilename`  
 <span data-ttu-id="3c65d-246">输出文件的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-246">The name of the output file.</span></span> <span data-ttu-id="3c65d-247">如果省略 `outputFilename`，则 Resgen.exe 会在与 `inputFilename` 相同的目录中创建根文件名为 `inputFilename` 的 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-247">If you omit `outputFilename`, Resgen.exe creates a .resources file with the root file name of `inputFilename` in the same directory as `inputFilename`.</span></span> <span data-ttu-id="3c65d-248">如果 `outputFilename` 包括一个目录路径，则该目录必须存在。</span><span class="sxs-lookup"><span data-stu-id="3c65d-248">If `outputFilename` includes a directory path, the directory must exist.</span></span>  
  
 <span data-ttu-id="3c65d-249">通过在文件名中指定完全限定的命名空间并通过句点将其与根文件名分隔开，可为 .resources 文件提供该命名空间。</span><span class="sxs-lookup"><span data-stu-id="3c65d-249">You provide a fully qualified namespace for the .resources file by specifying it in the file name and separating it from the root file name by a period.</span></span> <span data-ttu-id="3c65d-250">例如，如果 `outputFilename` 是 `MyCompany.Libraries.Strings.resources`，则命名空间为 MyCompany.Libraries。</span><span class="sxs-lookup"><span data-stu-id="3c65d-250">For example, if `outputFilename` is `MyCompany.Libraries.Strings.resources`, the namespace is MyCompany.Libraries.</span></span>  
  
 <span data-ttu-id="3c65d-251">下面的命令读取 Resources.txt 中的名称/值对，并编写一个名为 Resources.resources 的二进制 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-251">The following command reads the name/value pairs in Resources.txt and writes a binary .resources file named Resources.resources.</span></span> <span data-ttu-id="3c65d-252">由于未显式指定输出文件名，因此默认情况下它将接收与输入文件相同的文件名。</span><span class="sxs-lookup"><span data-stu-id="3c65d-252">Because the output file name is not specified explicitly, it receives the same name as the input file by default.</span></span>  
  
```console  
resgen Resources.txt
```  
  
 <span data-ttu-id="3c65d-253">下面的命令读取 Resources.restext 中的名称/值对，并编写一个名为 StringResources.resources 的二进制资源文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-253">The following command reads the name/value pairs in Resources.restext and writes a binary resources file named StringResources.resources.</span></span>  
  
```console  
resgen Resources.restext StringResources.resources  
```  
  
 <span data-ttu-id="3c65d-254">下面的命令读取名为 Resources.resx 的基于 XML 的输入文件，并编写一个名为 Resources.resources 的二进制 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-254">The following command reads an XML-based input file named Resources.resx and writes a binary .resources file named Resources.resources.</span></span>  
  
```console  
resgen Resources.resx Resources.resources  
```  
  
<a name="Convert"></a>

### <a name="converting-between-resource-file-types"></a><span data-ttu-id="3c65d-255">在资源文件类型之间转换</span><span class="sxs-lookup"><span data-stu-id="3c65d-255">Converting Between Resource File Types</span></span>  

 <span data-ttu-id="3c65d-256">除了将基于文本的或基于 XML 的资源文件编译为二进制 .resources 文件之外，Resgen.exe 还可以将任意受支持的文件类型转换为其他任何受支持的文件类型。</span><span class="sxs-lookup"><span data-stu-id="3c65d-256">In addition to compiling text-based or XML-based resource files into binary .resources files, Resgen.exe can convert any supported file type to any other supported file type.</span></span> <span data-ttu-id="3c65d-257">这意味着它可以执行以下转换：</span><span class="sxs-lookup"><span data-stu-id="3c65d-257">This means that it can perform the following conversions:</span></span>  
  
- <span data-ttu-id="3c65d-258">将 .txt 和 .restext 文件转换为 .resx 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-258">.txt and .restext files to .resx files.</span></span>  
  
- <span data-ttu-id="3c65d-259">将 .resx 文件转换为 .txt 和 .restext 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-259">.resx files to .txt and .restext files.</span></span>  
  
- <span data-ttu-id="3c65d-260">将 .resources 文件转换为 .txt 和 .restext 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-260">.resources files to .txt and .restext files.</span></span>  
  
- <span data-ttu-id="3c65d-261">将 .resources 文件转换为 .resx 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-261">.resources files to .resx files.</span></span>  
  
 <span data-ttu-id="3c65d-262">该语法与上一节中所示的语法相同。</span><span class="sxs-lookup"><span data-stu-id="3c65d-262">The syntax is the same as that shown in the previous section.</span></span>  
  
 <span data-ttu-id="3c65d-263">此外，可以使用 Resgen.exe 将 .NET Framework 程序集中的嵌入资源转换为 Windows 8.x 应用商店应用的 .resw 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-263">In addition, you can use Resgen.exe to convert embedded resources in a .NET Framework assembly to a .resw file tor Windows 8.x Store apps.</span></span>  
  
 <span data-ttu-id="3c65d-264">下面的命令读取二进制资源文件 Resources.resources，并编写一个名为 Resources.resx 的基于 XML 的输出文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-264">The following command reads a binary resources file Resources.resources and writes an XML-based output file named Resources.resx.</span></span>  
  
```console  
resgen Resources.resources Resources.resx  
```  
  
 <span data-ttu-id="3c65d-265">以下命令读取名为 StringResources.txt 的基于文本的资源文件，并编写一个名为 LibraryResources.resx 的基于 XML 的资源文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-265">The following command reads a text-based resources file named StringResources.txt and writes an XML-based resources file named LibraryResources.resx.</span></span> <span data-ttu-id="3c65d-266">除了包含字符串资源外，.resx 文件还可用于存储非字符串资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-266">In addition to containing string resources, the .resx file could also be used to store non-string resources.</span></span>  
  
```console  
resgen StringResources.txt LibraryResources.resx  
```  
  
 <span data-ttu-id="3c65d-267">以下两个命令读取名为 Resources.resx 的基于 XML 的资源文件，并编写名为 Resources.txt 和 Resources.restext 的文本文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-267">The following two commands read an XML-based resources file named Resources.resx and write text files named Resources.txt and Resources.restext.</span></span> <span data-ttu-id="3c65d-268">请注意，如果 .resx 文件包含任何嵌入的对象，则这些嵌入的对象不会准确地转换为文本文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-268">Note that if the .resx file contains any embedded objects, they will not be accurately converted into the text files.</span></span>  
  
```console  
resgen Resources.resx Resources.txt  
resgen Resources.resx Resources.restext  
```  
  
<a name="Multiple"></a>

### <a name="compiling-or-converting-multiple-files"></a><span data-ttu-id="3c65d-269">编译或转换多个文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-269">Compiling or Converting Multiple Files</span></span>  

 <span data-ttu-id="3c65d-270">可以使用 `/compile` 开关通过单次操作将资源文件列表从一种格式转换为另一格式。</span><span class="sxs-lookup"><span data-stu-id="3c65d-270">You can use the `/compile` switch to convert a list of resource files from one format to another in a single operation.</span></span> <span data-ttu-id="3c65d-271">语法为：</span><span class="sxs-lookup"><span data-stu-id="3c65d-271">The syntax is:</span></span>  
  
```console  
resgen /compile filename.extension [filename.extension...]  
```  
  
 <span data-ttu-id="3c65d-272">以下命令可将三种文件（StringResources.txt、TableResources.resw 和 ImageResources.resw）编译为名为 StringResources.resources、TableResources.resources 和 ImageResources.resources 的单独的 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-272">The following command compiles three files, StringResources.txt, TableResources.resw, and ImageResources.resw, into separate .resources files named StringResources.resources, TableResources.resources, and ImageResources.resources.</span></span>  
  
```console  
resgen /compile StringResources.txt TableResources.resx ImageResources.resx  
```  
  
<a name="Exporting"></a>

### <a name="exporting-resources-to-a-resw-file"></a><span data-ttu-id="3c65d-273">将资源导入 .resw 文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-273">Exporting Resources to a .resw File</span></span>  

 <span data-ttu-id="3c65d-274">如果正在开发 Windows 8.x 应用商店应用，你可能需要使用现有桌面应用中的资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-274">If you're developing a Windows 8.x Store app, you may want to use resources from an existing desktop app.</span></span> <span data-ttu-id="3c65d-275">但是，这两种应用程序支持不同的文件格式。</span><span class="sxs-lookup"><span data-stu-id="3c65d-275">However, the two kinds of applications support different file formats.</span></span> <span data-ttu-id="3c65d-276">在桌面应用中，文本（.txt 或 .restext）文件或 .resx 文件中的资源将编译为二进制 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-276">In desktop apps, resources in text (.txt or .restext) or .resx files are compiled into binary .resources files.</span></span> <span data-ttu-id="3c65d-277">在 Windows 8.x 应用商店应用中，.resw 文件将编译为二进制包资源索引 (PRI) 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-277">In Windows 8.x Store apps, .resw files are compiled into binary package resource index (PRI) files.</span></span> <span data-ttu-id="3c65d-278">可以使用 Resgen.exe 从可执行程序集或附属程序集中提取资源，并将资源写入一个或多个 .resw 文件（在开发 Windows 8.x 应用商店应用时可使用这些文件）中来填补此间隙。</span><span class="sxs-lookup"><span data-stu-id="3c65d-278">You can use Resgen.exe to bridge this gap by extracting resources from an executable or a satellite assembly and writing them to one or more .resw files that can be used when developing a Windows 8.x Store app.</span></span>  
  
> [!IMPORTANT]
> <span data-ttu-id="3c65d-279">Visual Studio 自动处理将可移植库中的资源并入 Windows 8.x 应用商店应用中所需的所有转换。</span><span class="sxs-lookup"><span data-stu-id="3c65d-279">Visual Studio automatically handles all conversions necessary for incorporating the resources in a portable library into a Windows 8.x Store app.</span></span> <span data-ttu-id="3c65d-280">仅需要在 Visual Studio 外部开发 Windows 8.x 应用商店应用的开发人员对直接使用 Resgen.exe 将程序集中的资源转换为 .resw 文件格式感兴趣。</span><span class="sxs-lookup"><span data-stu-id="3c65d-280">Using Resgen.exe directly to convert the resources in an assembly to .resw file format is of interest only to developers who want to develop a Windows 8.x Store app outside of Visual Studio.</span></span>  
  
 <span data-ttu-id="3c65d-281">用于从程序集生成 .resw 文件的语法是：</span><span class="sxs-lookup"><span data-stu-id="3c65d-281">The syntax to generate .resw files from an assembly is:</span></span>  
  
```console  
resgen filename.extension  [outputDirectory]  
```  
  
 <span data-ttu-id="3c65d-282">参数的位置是：</span><span class="sxs-lookup"><span data-stu-id="3c65d-282">where the parameters are:</span></span>  
  
 `filename.extension`  
 <span data-ttu-id="3c65d-283">.NET Framework 程序集的名称（可执行文件或 .DLL）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-283">The name of a .NET Framework assembly (an executable or .DLL).</span></span> <span data-ttu-id="3c65d-284">如果该文件不包含资源，则 Resgen.exe 不创建任何文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-284">If the file contains no resources, Resgen.exe does not create any files.</span></span>  
  
 `outputDirectory`  
 <span data-ttu-id="3c65d-285">要将 .resw 文件写入到的现有目录。</span><span class="sxs-lookup"><span data-stu-id="3c65d-285">The existing directory to which to write the .resw files.</span></span> <span data-ttu-id="3c65d-286">如果省略 `outputDirectory`，则将 .resw 文件写入到当前目录。</span><span class="sxs-lookup"><span data-stu-id="3c65d-286">If `outputDirectory` is omitted, .resw files are written to the current directory.</span></span> <span data-ttu-id="3c65d-287">Resgen.exe 为程序集中的每个 .resources 文件创建一个 .resw 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-287">Resgen.exe creates one .resw file for each .resources file in the assembly.</span></span> <span data-ttu-id="3c65d-288">.resw 文件的根文件名与 .resources 文件的根名称相同。</span><span class="sxs-lookup"><span data-stu-id="3c65d-288">The root file name of the .resw file is the same as the root name of the .resources file.</span></span>  
  
 <span data-ttu-id="3c65d-289">以下命令在 Win8Resources 目录中为嵌入 MyApp.exe 中的每个 .resources 文件创建一个 .resw 文件：</span><span class="sxs-lookup"><span data-stu-id="3c65d-289">The following command creates a .resw file in the Win8Resources directory for each .resources file embedded in MyApp.exe:</span></span>  
  
```console  
resgen MyApp.exe Win8Resources  
```  
  
<a name="Conditional"></a>

### <a name="conditionally-compiling-resources"></a><span data-ttu-id="3c65d-290">条件编译资源</span><span class="sxs-lookup"><span data-stu-id="3c65d-290">Conditionally Compiling Resources</span></span>  

 <span data-ttu-id="3c65d-291">从 .NET Framework 4.5 开始，Resgen.exe 支持在文本（.txt 和 .restext）文件中的字符串资源的条件编译。</span><span class="sxs-lookup"><span data-stu-id="3c65d-291">Starting with the .NET Framework 4.5, Resgen.exe supports conditional compilation of string resources in text (.txt and .restext) files.</span></span> <span data-ttu-id="3c65d-292">这使你能够在多个生成配置中使用单个基于文本的资源文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-292">This enables you to use a single text-based resource file in multiple build configurations.</span></span>  
  
 <span data-ttu-id="3c65d-293">在 .txt 或 .restext 文件中，可以使用 `#ifdef`…`#endif`</span><span class="sxs-lookup"><span data-stu-id="3c65d-293">In a .txt or .restext file, you use the `#ifdef`…`#endif`</span></span> <span data-ttu-id="3c65d-294">构造来包含二进制 .resources 文件中的资源（如果定义了符号），并可使用 `#if !`...`#endif` 构造来包含资源（如果未定义符号）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-294">construct to include a resource in the binary .resources file if a symbol is defined, and you use the `#if !`... `#endif` construct to include a resource if a symbol is not defined.</span></span> <span data-ttu-id="3c65d-295">在编译时，随后使用后跟符号的以逗号分隔的列表的 `/define:` 选项来定义符号。</span><span class="sxs-lookup"><span data-stu-id="3c65d-295">At compile time, you then define symbols by using the `/define:` option followed by a comma-delimited list of symbols.</span></span> <span data-ttu-id="3c65d-296">该比较是区分大小写的；`/define` 定义的符号的大熊写必须与要编译的文本文件中的符号的大小写匹配。</span><span class="sxs-lookup"><span data-stu-id="3c65d-296">The comparison is cased-sensitive; the case of symbols defined by `/define` must match the case of symbols in the text files to be compiled.</span></span>  
  
 <span data-ttu-id="3c65d-297">例如，以下名为 UIResources.rext 的文件包含可采用三个值之一的名为 `AppTitle` 的字符串资源，具体取决于是否定义了名为 `PRODUCTION`、`CONSULT` 或 `RETAIL` 的符号。</span><span class="sxs-lookup"><span data-stu-id="3c65d-297">For example, the following file named UIResources.rext includes a string resource named `AppTitle` that can take one of three values, depending on whether symbols named `PRODUCTION`, `CONSULT`, or `RETAIL` are defined.</span></span>  
  
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
  
 <span data-ttu-id="3c65d-298">然后，可使用以下命令将该文件编译为二进制 .resources 文件：</span><span class="sxs-lookup"><span data-stu-id="3c65d-298">The file can then be compiled into a binary .resources file with the following command:</span></span>  
  
```console  
resgen /define:CONSULT UIResources.restext  
```  
  
 <span data-ttu-id="3c65d-299">这会生成包含两个字符串资源的 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-299">This produces a .resources file that contains two string resources.</span></span> <span data-ttu-id="3c65d-300">`AppTitle` 资源的值为“My Consulting Company Project Manager”。</span><span class="sxs-lookup"><span data-stu-id="3c65d-300">The value of the `AppTitle` resource is "My Consulting Company Project Manager".</span></span>  
  
<a name="Strong"></a>

### <a name="generating-a-strongly-typed-resource-class"></a><span data-ttu-id="3c65d-301">生成强类型资源类</span><span class="sxs-lookup"><span data-stu-id="3c65d-301">Generating a Strongly Typed Resource Class</span></span>  

 <span data-ttu-id="3c65d-302">Resgen.exe 支持强类型的资源，它通过创建包含一组静态只读属性的类来封装对资源的访问。</span><span class="sxs-lookup"><span data-stu-id="3c65d-302">Resgen.exe supports strongly typed resources, which encapsulates access to resources by creating classes that contain a set of static read-only properties.</span></span> <span data-ttu-id="3c65d-303">这提供了用于直接调用 <xref:System.Resources.ResourceManager> 类的方法来检索资源的替代方法。</span><span class="sxs-lookup"><span data-stu-id="3c65d-303">This provides an alternative to calling the methods of the <xref:System.Resources.ResourceManager> class directly to retrieve resources.</span></span> <span data-ttu-id="3c65d-304">通过在 Resgen.exe（可包装 `/str` 类的功能）中使用 <xref:System.Resources.Tools.StronglyTypedResourceBuilder> 选项，可以启用强类型的资源支持。</span><span class="sxs-lookup"><span data-stu-id="3c65d-304">You can enable strongly typed resource support by using the `/str` option in Resgen.exe, which wraps the functionality of the <xref:System.Resources.Tools.StronglyTypedResourceBuilder> class.</span></span> <span data-ttu-id="3c65d-305">在指定 `/str` 选项时，Resgen.exe 的输出是一个包含强类型属性的类，这些属性与输入参数中引用的资源相匹配。</span><span class="sxs-lookup"><span data-stu-id="3c65d-305">When you specify the `/str` option, the output of Resgen.exe is a class that contains strongly typed properties that match the resources that are referenced in the input parameter.</span></span> <span data-ttu-id="3c65d-306">此类提供对已处理的文件中可用的资源的强类型只读访问。</span><span class="sxs-lookup"><span data-stu-id="3c65d-306">This class provides strongly typed read-only access to the resources that are available in the file processed.</span></span>  
  
 <span data-ttu-id="3c65d-307">用于创建强类型资源的语法是：</span><span class="sxs-lookup"><span data-stu-id="3c65d-307">The syntax to create a strongly typed resource is:</span></span>  
  
```console  
resgen inputFilename [outputFilename] /str:language[,namespace,[classname[,filename]]] [/publicClass]  
```  
  
 <span data-ttu-id="3c65d-308">参数和开关为：</span><span class="sxs-lookup"><span data-stu-id="3c65d-308">The parameters and switches are:</span></span>  
  
 `inputFilename`  
 <span data-ttu-id="3c65d-309">要为其生成强类型资源类的资源文件的文件名（包括扩展名）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-309">The file name, including the extension, of the resource file for which to generate a strongly typed resource class.</span></span> <span data-ttu-id="3c65d-310">该文件可以为基于文本的文件、基于 XML 的文件或二进制 .resources 文件；它可以具有 .txt、.restext、.resw 或 .resources 扩展名。</span><span class="sxs-lookup"><span data-stu-id="3c65d-310">The file can be a text-based, XML-based, or binary .resources file; it can have an extension of .txt, .restext, .resw, or .resources.</span></span>  
  
 `outputFilename`  
 <span data-ttu-id="3c65d-311">输出文件的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-311">The name of the output file.</span></span> <span data-ttu-id="3c65d-312">如果 `outputFilename` 包括一个目录路径，则该目录必须存在。</span><span class="sxs-lookup"><span data-stu-id="3c65d-312">If `outputFilename` includes a directory path, the directory must exist.</span></span> <span data-ttu-id="3c65d-313">如果省略 `outputFilename`，则 Resgen.exe 会在与 `inputFilename` 相同的目录中创建根文件名为 `inputFilename` 的 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-313">If you omit `outputFilename`, Resgen.exe creates a .resources file with the root file name of `inputFilename` in the same directory as `inputFilename`.</span></span>  
  
 <span data-ttu-id="3c65d-314">`outputFilename` 可以是基于文本的文件、基于 XML 的文件或二进制 .resources 文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-314">`outputFilename` can be a text-based, XML-based, or binary .resources file.</span></span> <span data-ttu-id="3c65d-315">如果 `outputFilename` 的文件扩展名与 `inputFilename` 的文件扩展名不同，则 Resgen.exe 会执行文件转换。</span><span class="sxs-lookup"><span data-stu-id="3c65d-315">If the file extension of `outputFilename` is different from the file extension of `inputFilename`, Resgen.exe performs the file conversion.</span></span>  
  
 <span data-ttu-id="3c65d-316">如果 `inputFilename` 是 .resources 文件，`outputFilename` 也是 .resources 文件，则 Resgen.exe 会前者。</span><span class="sxs-lookup"><span data-stu-id="3c65d-316">If `inputFilename` is a .resources file, Resgen.exe copies the .resources file if `outputFilename` is also a .resources file.</span></span> <span data-ttu-id="3c65d-317">如果省略 `outputFilename`，则 Resgen.exe 会用相同的 .resources 文件覆盖 `inputFilename`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-317">If `outputFilename` is omitted, Resgen.exe overwrites `inputFilename` with an identical .resources file.</span></span>  
  
 <span data-ttu-id="3c65d-318">language</span><span class="sxs-lookup"><span data-stu-id="3c65d-318">*language*</span></span>  
 <span data-ttu-id="3c65d-319">为强类型资源类生成源代码时要使用的语言。</span><span class="sxs-lookup"><span data-stu-id="3c65d-319">The language in which to generate source code for the strongly typed resource class.</span></span> <span data-ttu-id="3c65d-320">C# 代码的可能值为 `cs`、`C#` 和 `csharp`；Visual Basic 代码的可能值为 `vb` 和 `visualbasic`；VBScript 代码的可能值为 `vbs` 和 `vbscript`；C++ 代码的可能值为 `c++`、`mc` 和 `cpp`。</span><span class="sxs-lookup"><span data-stu-id="3c65d-320">Possible values are `cs`, `C#`, and `csharp` for C# code, `vb` and `visualbasic` for Visual Basic code, `vbs` and `vbscript` for VBScript code, and `c++`, `mc`, and `cpp` for C++ code.</span></span>  
  
 <span data-ttu-id="3c65d-321">*namespace*</span><span class="sxs-lookup"><span data-stu-id="3c65d-321">*namespace*</span></span>  
 <span data-ttu-id="3c65d-322">包含强类型资源类的命名空间。</span><span class="sxs-lookup"><span data-stu-id="3c65d-322">The namespace that contains the strongly typed resource class.</span></span> <span data-ttu-id="3c65d-323">.resources 文件和资源类应具有相同的命名空间。</span><span class="sxs-lookup"><span data-stu-id="3c65d-323">The .resources file and the resource class should have the same namespace.</span></span> <span data-ttu-id="3c65d-324">有关在 `outputFilename` 中指定命名空间的信息，请参阅[将资源编译为二进制文件](resgen-exe-resource-file-generator.md#Compiling)。</span><span class="sxs-lookup"><span data-stu-id="3c65d-324">For information about specifying the namespace in the `outputFilename`, see [Compiling Resources into a Binary File](resgen-exe-resource-file-generator.md#Compiling).</span></span> <span data-ttu-id="3c65d-325">如果省略 namespace，则资源类不包含在命名空间内。</span><span class="sxs-lookup"><span data-stu-id="3c65d-325">If *namespace* is omitted, the resource class is not contained in a namespace.</span></span>  
  
 <span data-ttu-id="3c65d-326">classname</span><span class="sxs-lookup"><span data-stu-id="3c65d-326">*classname*</span></span>  
 <span data-ttu-id="3c65d-327">强类型资源类的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-327">The name of the strongly typed resource class.</span></span> <span data-ttu-id="3c65d-328">这应对应于 .resources 文件的根名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-328">This should correspond to the root name of the .resources file.</span></span> <span data-ttu-id="3c65d-329">例如，如果 Resgen.exe 生成名为 MyCompany.Libraries.Strings.resources 的 .resources 文件，则强类型资源类的名称为 Strings。</span><span class="sxs-lookup"><span data-stu-id="3c65d-329">For example, if Resgen.exe generates a .resources file named MyCompany.Libraries.Strings.resources, the name of the strongly typed resource class is Strings.</span></span> <span data-ttu-id="3c65d-330">如果省略 classname，则生成的类是从 `outputFilename` 的根名称派生的。</span><span class="sxs-lookup"><span data-stu-id="3c65d-330">If *classname* is omitted, the generated class is derived from the root name of `outputFilename`.</span></span> <span data-ttu-id="3c65d-331">如果省略 `outputFilename`，则生成的类是从 `inputFilename` 的根名称派生的。</span><span class="sxs-lookup"><span data-stu-id="3c65d-331">If `outputFilename` is omitted, the generated class is derived from the root name of `inputFilename`.</span></span>  
  
 <span data-ttu-id="3c65d-332">classname 不能包含无效字符（如嵌入的空格）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-332">*classname* cannot contain invalid characters such as embedded spaces.</span></span> <span data-ttu-id="3c65d-333">如果 classname 包含嵌入的空格，或者从 inputFilename 中生成 classname（默认情况），并且 inputFilename 包含嵌入的空格，则 Resgen.exe 会将所有无效字符替换为下划线 (\_)   。</span><span class="sxs-lookup"><span data-stu-id="3c65d-333">If *classname* contains embedded spaces, or if *classname* is generated by default from *inputFilename*, and *inputFilename* contains embedded spaces, Resgen.exe replaces all invalid characters with an underscore (\_).</span></span>  
  
 <span data-ttu-id="3c65d-334">*filename*</span><span class="sxs-lookup"><span data-stu-id="3c65d-334">*filename*</span></span>  
 <span data-ttu-id="3c65d-335">类文件的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-335">The name of the class file.</span></span>  
  
 `/publicclass`  
 <span data-ttu-id="3c65d-336">使强类型资源类成为公共类而不是 `internal`（在 C# 中）或 `Friend`（在 Visual Basic 中）。</span><span class="sxs-lookup"><span data-stu-id="3c65d-336">Makes the strongly typed resource class public rather than `internal` (in C#) or `Friend` (in Visual Basic).</span></span> <span data-ttu-id="3c65d-337">这允许从资源嵌入到的程序集外部访问这些资源。</span><span class="sxs-lookup"><span data-stu-id="3c65d-337">This allows the resources to be accessed from outside the assembly in which they are embedded.</span></span>  
  
> [!IMPORTANT]
> <span data-ttu-id="3c65d-338">创建强类型资源类时，.resources 文件的名称必须与生成的代码的命名空间和类名匹配。</span><span class="sxs-lookup"><span data-stu-id="3c65d-338">When you create a strongly typed resource class, the name of your .resources file must match the namespace and class name of the generated code.</span></span> <span data-ttu-id="3c65d-339">但是，Resgen.exe 允许你指定可生成具有不兼容的名称的 .resources 文件的选项。</span><span class="sxs-lookup"><span data-stu-id="3c65d-339">However, Resgen.exe allows you to specify options that produce a .resources file that has an incompatible name.</span></span> <span data-ttu-id="3c65d-340">若要解决此行为，请在生成输出文件后重命名此文件。</span><span class="sxs-lookup"><span data-stu-id="3c65d-340">To work around this behavior, rename the output file after it has been generated.</span></span>  
  
 <span data-ttu-id="3c65d-341">强类型资源类具有下列成员：</span><span class="sxs-lookup"><span data-stu-id="3c65d-341">The strongly typed resource class has the following members:</span></span>  
  
- <span data-ttu-id="3c65d-342">无参数构造函数，可用于实例化强类型资源类。</span><span class="sxs-lookup"><span data-stu-id="3c65d-342">A parameterless constructor, which can be used to instantiate the strongly typed resource class.</span></span>  
  
- <span data-ttu-id="3c65d-343">`static` (C#) 或 `Shared` (Visual Basic) 和只读 `ResourceManager` 属性，该属性返回管理强类型资源的 <xref:System.Resources.ResourceManager> 实例。</span><span class="sxs-lookup"><span data-stu-id="3c65d-343">A `static` (C#) or `Shared` (Visual Basic) and read-only `ResourceManager` property, which returns the <xref:System.Resources.ResourceManager> instance that manages the strongly typed resource.</span></span>  
  
- <span data-ttu-id="3c65d-344">静态 `Culture` 属性，它允许你设置用于资源检索的区域性。</span><span class="sxs-lookup"><span data-stu-id="3c65d-344">A static `Culture` property, which allows you to set the culture used for resource retrieval.</span></span> <span data-ttu-id="3c65d-345">默认情况下，其值为 `null`，这表示使用了当前 UI 区域性。</span><span class="sxs-lookup"><span data-stu-id="3c65d-345">By default, its value is `null`, which means that the current UI culture is used.</span></span>  
  
- <span data-ttu-id="3c65d-346">一个 `static` (C#) 或 `Shared` (Visual Basic) 以及 .resources 文件中的每个资源的只读属性。</span><span class="sxs-lookup"><span data-stu-id="3c65d-346">One `static` (C#) or `Shared` (Visual Basic) and read-only property for each resource in the .resources file.</span></span> <span data-ttu-id="3c65d-347">属性的名称是该资源的名称。</span><span class="sxs-lookup"><span data-stu-id="3c65d-347">The name of the property is the name of the resource.-</span></span>  
  
 <span data-ttu-id="3c65d-348">例如，下面的命令将名为 StringResources.txt 的资源文件编译为名为 StringResources.resources 的文件，并在可用于访问资源管理器的名为 StringResources.vb 的 Visual Basic 源代码文件中生成名为 `StringResources` 的类。</span><span class="sxs-lookup"><span data-stu-id="3c65d-348">For example, the following command compiles a resource file named StringResources.txt into StringResources.resources and generates a class named `StringResources` in a Visual Basic source code file named StringResources.vb that can be used to access the Resource Manager.</span></span>  
  
```console  
resgen StringResources.txt /str:vb,,StringResources
```  
  
## <a name="see-also"></a><span data-ttu-id="3c65d-349">请参阅</span><span class="sxs-lookup"><span data-stu-id="3c65d-349">See also</span></span>

- [<span data-ttu-id="3c65d-350">工具</span><span class="sxs-lookup"><span data-stu-id="3c65d-350">Tools</span></span>](index.md)
- [<span data-ttu-id="3c65d-351">桌面应用中的资源</span><span class="sxs-lookup"><span data-stu-id="3c65d-351">Resources in Desktop Apps</span></span>](../resources/index.md)
- [<span data-ttu-id="3c65d-352">创建资源文件</span><span class="sxs-lookup"><span data-stu-id="3c65d-352">Creating Resource Files</span></span>](../resources/creating-resource-files-for-desktop-apps.md)
- [<span data-ttu-id="3c65d-353">Al.exe（程序集链接器）</span><span class="sxs-lookup"><span data-stu-id="3c65d-353">Al.exe (Assembly Linker)</span></span>](al-exe-assembly-linker.md)
- [<span data-ttu-id="3c65d-354">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="3c65d-354">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
