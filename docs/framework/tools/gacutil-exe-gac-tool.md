---
title: Gacutil.exe（全局程序集缓存工具）
description: 获取有关 Gacutil.exe（全局程序集缓存工具）的详细信息。 可使用此工具查看和操作全局程序集缓存和下载缓存。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, viewing contents
- viewing assemblies in global assembly cache
- global assembly cache, manipulating contents
- GAC (global assembly cache), Gacutil.exe
- Gacutil.exe
- GAC (global assembly cache), viewing contents
- installing assemblies into global assembly cache
- removing assemblies from global assembly cache
- list of assemblies in global assembly cache
- cache [.NET Framework], global assembly cache
- GAC (global assembly cache), manipulating contents
- global assembly cache, Gacutil.exe
- Global Assembly Cache tool
ms.assetid: 4c7be9c8-72ae-481f-a01c-1a4716806e99
ms.openlocfilehash: 68a85b74333e978adc0b9953d408b36d5e91738f
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654136"
---
# <a name="gacutilexe-global-assembly-cache-tool"></a>Gacutil.exe（全局程序集缓存工具）

全局程序集缓存工具使你可以查看和操作全局程序集缓存和下载缓存的内容。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
gacutil [options] [assemblyName | assemblyPath | assemblyListFile]
```

## <a name="parameters"></a>参数

|参数|描述|
|--------------|-----------------|
|*assemblyName*|程序集的名称。 可以提供部分指定的程序集名称（如 `myAssembly`）或完全指定的程序集名称（如 `myAssembly, Version=2.0.0.0, Culture=neutral, PublicKeyToken=0038abc9deabfle5`）。|
|assemblyPath|包含程序集清单的文件的名称。|
|assemblyListFile|列出要安装或卸载的程序集的 ANSI 文本文件的路径。 若要使用文本文件安装程序集，请在文件中使用单独的行分别指定每个程序集的路径。 该工具可解释相对于 assemblyListFile 位置的相对路径。 若要使用文本文件卸载程序集，请在文件中使用单独的行分别为每个程序集指定完全限定的程序集名称。 请参见本主题后面的 assemblyListFile 内容示例。|

|选项|描述|
|------------|-----------------|
|/cdl|删除下载缓存的内容。|
|/f|使用 /i 或 /il 选项来指定此选项将强制重新安装程序集 。 如果全局程序集缓存中已经存在同名的程序集，全局程序集缓存工具将覆盖它。|
|**/h**[**elp**]|显示该工具的命令语法和选项。|
|/i assemblyPath|将程序集安装到全局程序集缓存中。|
|/if  assemblyPath|将程序集安装到全局程序集缓存中。 如果全局程序集缓存中已经存在同名的程序集，全局程序集缓存工具将覆盖它。<br /><br /> 指定此选项相当于同时指定 /i 和 /f 选项 。|
|/il assemblyListFile|将 assemblyListFile 中指定的一个或多个程序集安装到全局程序集缓存中。|
|/ir  assemblyPath<br /><br /> scheme<br /><br /> *id*<br /><br /> description|将程序集安装到全局程序集缓存中，并添加引用以对程序集进行计数。 必须使用此选项指定 assemblyPath、scheme、id 和 description 参数   。 有关可为这些参数指定的有效值的说明，请从参阅 /r 选项。<br /><br /> 指定此选项相当于同时指定 /i 和 /r 选项 。|
|/l [assemblyName]|列出全局程序集缓存的内容。 如果指定了 assemblyName 参数，则全局程序集缓存工具只列出与该名称匹配的程序集。|
|/ldl|列出下载的文件缓存的内容。|
|/lr [assemblyName]|列出所有程序集及其对应的引用计数。 如果指定了 assemblyName 参数，则全局程序集缓存工具只列出与该名称匹配的程序集及其对应的引用计数。|
|**/nologo**|取消显示 Microsoft 启动版权标志。|
|/r [assemblyName &#124; assemblyPath]<br /><br /> scheme<br /><br /> *id*<br /><br /> description|指定对要安装或卸载的一个或多个程序集的跟踪引用。 使用 /i、/il、/u 或 /ul 选项来指定此选项   。<br /><br /> 若要安装程序集，请使用此选项来指定 assemblyPath、scheme、id 和 description 参数   。 若要卸载程序集，请指定 assemblyPath、scheme、id 和 description 参数   。<br /><br /> 若要移除对程序集的引用，指定的 scheme、id 和 description 参数必须与安装程序集时使用 /i 和 /r（或者 /ir）选项指定的参数相同    。 如果卸载某个程序集，则在下列情况下，全局程序集缓存工具还会从全局程序集缓存中移除该程序集：该程序集是最后一个要移除的引用，并且 Windows Installer 没有对该程序集的未处理引用。<br /><br /> scheme 参数指定安装方案的类型。 可以指定以下值之一：<br /><br /> - UNINSTALL_KEY:如果安装程序将应用程序添加到 Microsoft Windows 中的“添加/删除程序”，则指定此值。 应用程序通过将注册表项添加到 HKLM\Software\Microsoft\Windows\CurrentVersion 来将自己添加到“添加/删除程序”。<br />- FILEPATH:如果安装程序没有将应用程序添加到“添加/删除程序”中，则指定此值。<br />- OPAQUE:如果提供的注册表项或文件路径不适于你的安装方案，则指定此值。 通过此值可以为 id 参数指定自定义信息。<br /><br /> 为 id 参数指定的值取决于为 scheme 参数指定的值 ：<br /><br /> - 如果为 scheme 参数指定 UNINSTALL_KEY，请在 HKLM\Software\Microsoft\Windows\CurrentVersion 注册表项中指定应用程序集的名称。 例如，如果注册表项是 HKLM\Software\Microsoft\Windows\CurrentVersion\MyApp，请为 id 参数指定 MyApp。<br />- 如果为 scheme 参数指定 FILEPATH，请指定安装程序集的可执行文件的完整路径作为 id 参数 。<br />- 如果为 scheme 参数指定 OPAQUE，则可以将任何一段数据作为 id 参数提供 。 所指定的数据必须用引号 ("") 括起来。<br /><br /> description 参数可用于指定有关要安装的应用程序的描述性文本。 此信息在枚举引用时显示。|
|**/Silent**|取消所有输出的显示。|
|/u  assemblyName|将某个程序集从全局程序集缓存卸载。|
|/uf  assemblyName|通过移除对程序集的所有引用来强制卸载指定的程序集。<br /><br /> 指定此选项相当于同时指定 /u 和 /f 选项 。 **注意：** 不能使用此选项移除使用 Microsoft Windows Installer 安装的程序集。 如果尝试此操作，则全局程序集缓存工具会显示错误消息。|
|/ul assemblyListFile|从全局程序集缓存中卸载 assemblyListFile 中指定的一个或多个程序集。|
|/u[ngen] assemblyName |从全局程序集缓存中卸载指定的程序集。 如果指定的程序集具有现有引用计数，则全局程序集缓存工具会显示引用计数，而且不会从全局程序集缓存中移除该程序集。 **注意：** 在 .NET Framework 2.0 版中，不支持 `/ungen`。 请改为使用 [Ngen.exe（本机映像生成器）](ngen-exe-native-image-generator.md)的 `uninstall` 命令。 <br /><br /> 在 .NET Framework 1.0 和 1.1 版中，指定 /ungen 会导致 Gacutil.exe 从本机映像缓存中移除该程序集。 此缓存存储了使用 [Ngen.exe（本机映像生成器）](ngen-exe-native-image-generator.md) 创建的程序集的本机印象。|
|/ur  assemblyName<br /><br /> scheme<br /><br /> *id*<br /><br /> description|从全局程序集缓存中卸载对指定程序集的引用。 若要移除对程序集的引用，指定的 scheme、id 和 description 参数必须与安装程序集时使用 /i 和 /r（或者 /ir）选项指定的参数相同    。 有关可为这些参数指定的有效值的说明，请从参阅 /r 选项。<br /><br /> 指定此选项相当于同时指定 /u 和 /r 选项 。|
|**/?**|显示该工具的命令语法和选项。|

## <a name="remarks"></a>备注

> [!NOTE]
> 必须具有管理员特权才能使用 Gacutil.exe。

具体说来，Gacutil.exe 使你可以将程序集安装到缓存中、从缓存中移除程序集以及列出缓存的内容。

Gacutil.exe 提供了支持引用计数的选项，类似于 Windows Installer 所支持的引用计数方案。 你可以使用 Gacutil.exe 安装两个将安装同一程序集的应用程序；全局程序集缓存工具将跟踪对该程序集的引用的数量。 结果是，该程序集将一直保留在计算机上，直到卸载这两个应用程序为止。 如果将 Gacutil.exe 用于实际产品安装，请使用支持引用计数的选项。 同时使用 /i 和 /r 选项可以安装程序集并添加引用以对其进行计数 。 同时使用 /u 和 /r 选项可以移除对程序集的引用计数 。 注意，单独使用 /i 和 /u 选项不支持引用计数 。 这些选项在产品开发期间适用，但不适用于实际的产品安装。

使用 /il 或 /ul 选项可以安装或卸载存储在 ANSI 文本文件中的程序集 。 该文本文件中的内容必须具有正确的格式。 若要使用文本文件安装程序集，请在文件中使用单独的行分别指定每个程序集的路径。 下面的示例说明了包含要安装的程序集的文件的内容。

```text
myAssembly1.dll
myAssembly2.dll
myAssembly3.dll
```

若要使用文本文件卸载程序集，请在文件中使用单独的行分别为每个程序集指定完全限定的程序集名称。 下面的示例说明了包含要卸载的程序集的文件的内容。

```text
myAssembly1,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
myAssembly2,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
myAssembly3,Version=1.1.0.0,Culture=en,PublicKeyToken=874e23ab874e23ab
```

> [!NOTE]
> 尝试安装文件名长度超过 79 到 91 个字符（不包括文件扩展名）的程序集可能会导致以下错误：
>
> ```output
> Failure adding assembly to the cache:   The file name is too long.
> ```
>
> 这是因为 Gacutil.exe 在内部构造了一个最多 MAX_PATH 个字符的路径，该路径由以下元素组成：
>
> - GAC Root - 34 个字符（即 `C:\Windows\Microsoft.NET\assembly\`）
> - Architecture - 7 或 9 个字符（即 `GAC_32\`、`GAC_64\`、`GAC_MSIL`）
> - AssemblyName - 最多 91 个字符，具体取决于其他元素的大小（例如 `System.Xml.Linq\`)
> - AssemblyInfo - 31 到 48 个字符或由以下内容组成的更多字符：
>   - Framework - 5 个字符（例如 `v4.0_`)
>   - AssemblyVersion - 8 到 24 个字符（例如 `9.0.1000.0_`)
>   - AssemblyLanguage - 1 到 8 个字符（例如 `de_`, `sr-Cyrl_`)
>   - PublicKey - 17 个字符（例如 `31bf3856ad364e35\`)
> - DllFileName - 最多 91 + 4 个字符（即 `<AssemblyName>.dll`）

## <a name="examples"></a>示例

以下命令将程序集 `mydll.dll` 安装到全局程序集缓存中。

```console
gacutil /i mydll.dll
```

以下命令从全局程序集缓存中移除程序集 `hello`（只要不存在对该程序集的引用计数）。

```console
gacutil /u hello
```

请注意，由于未指定完全的程序集名称，上面的命令可能会从程序集缓存中移除多个程序集。 例如，如果在该缓存中安装有 `hello` 的 1.0.0.0 和 3.2.2.1 两个版本，则 `gacutil /u hello` 命令会将这两个程序集都移除。

使用下面的示例可避免删除多个程序集。 此命令只删除与完全指定的版本号、区域性和公钥匹配的 `hello` 程序集。

```console
gacutil /u hello, Version=1.0.0.1, Culture="de",PublicKeyToken=45e343aae32233ca
```

以下命令将 `assemblyList.txt` 文件中指定的程序集安装到全局程序集缓存中。

```console
gacutil /il assemblyList.txt
```

以下命令从全局程序集缓存中移除 `assemblyList.txt` 文件中指定的程序集。

```console
gacutil /ul assemblyList.txt
```

以下命令将 `myDll.dll` 安装到全局程序集缓存中并添加引用以对其进行计数。 程序集 `myDll.dll` 由应用程序 `MyApp` 使用。 `UNINSTALL_KEY MyApp` 参数指定可将 `MyApp` 添加到 Windows 的“添加/删除程序”的注册表项。 description 参数被指定为 `My Application Description`。

```console
gacutil /i /r myDll.dll UNINSTALL_KEY MyApp "My Application Description"
```

以下命令将 `myDll.dll` 安装到全局程序集缓存中并添加引用以对其进行计数。 scheme 参数 `FILEPATH` 和 id 参数 `c:\applications\myApp\myApp.exe` 指定要安装 `myDll.dll.` 的应用程序的路径。description 参数被指定为 `MyApp`。

```console
gacutil /i /r myDll.dll FILEPATH c:\applications\myApp\myApp.exe MyApp
```

以下命令将 `myDll.dll` 安装到全局程序集缓存中并添加引用以对其进行计数。 scheme 参数 `OPAQUE` 允许你自定义 id 和 description 参数。

```console
gacutil /i /r mydll.dll OPAQUE "Insert custom application details here" "Insert Custom description information here"
```

以下命令移除应用程序 `myDll.dll` 对 `myApp` 的引用。 如果这是对该程序集的最后一个引用，则将同时从全局程序集缓存中移除该程序集。

```console
gacutil /u /r myDll.dll FILEPATH c:\applications\myApp\myApp.exe MyApp
```

下面的命令列出全局程序集缓存的内容。

```console
gacutil /l
```

## <a name="see-also"></a>请参阅

- [工具](index.md)
- [全局程序集缓存](../app-domains/gac.md)
- [Regasm.exe（程序集注册工具）](regasm-exe-assembly-registration-tool.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
