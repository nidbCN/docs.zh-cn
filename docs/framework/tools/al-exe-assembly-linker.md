---
title: Al.exe（程序集链接器）
description: 使用 AI.exe（程序集链接器工具）。 此工具从一个或多个文件（这些文件可以是模块或资源文件）生成一个具有程序集清单的文件。
ms.date: 03/30/2017
helpviewer_keywords:
- Al.exe
- Assembly Linker
- modules, Assembly Linker
- assembly manifest, Assembly Linker
ms.assetid: b5382965-0053-47cf-b92f-862860275a01
ms.openlocfilehash: dff7ab7ccc6543b1c13cb2b49cbdf712eba301ae
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652953"
---
# <a name="alexe-assembly-linker"></a>Al.exe（程序集链接器）

程序集链接器从一个或多个文件（这些文件可以是模块或资源文件）生成一个具有程序集清单的文件。 模块是不含程序集清单的中间语言 (IL) 文件。

> [!NOTE]
> 从 Visual Studio 2008 开始，C# 和 Visual Basic 编译器都自动将 Win32 清单嵌入到程序集中。 有关详细信息，请参阅 [-win32manifest（C# 编译器选项）](../../csharp/language-reference/compiler-options/resources.md#win32manifest)。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
al sources options
```

## <a name="parameters"></a>参数

你可以指定以下一个或多个 `sources`。

| 源 | 描述 |
| ------ | ----------- |
|`file`[,`target`]|将 `file`（模块）的内容复制到 `target` 指定的文件名。 复制后，Al.exe 将 `target` 编译为程序集。|
|**/embed[resource]:** `file`[,`name`[,`private`]]|将 `file` 指定的资源嵌入到包含程序集清单的映像中；Al.exe 将 `file` 的内容复制到可移植的可执行 (PE) 映像中。<br /><br /> `name` 参数是资源的内部标识符。 默认情况下，资源在程序集中是公共的（对于其他程序集可见）。 指定 `private` 会使该资源对于其他程序集不可见。<br /><br /> 例如，如果 `file` 是由[资源文件生成器 (Resgen.exe)](resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET Framework 资源文件，则可使用 <xref:System.Resources> 中的成员来访问它。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager>。 对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 中的 `GetManifestResource*` 方法在运行时访问此资源。<br /><br /> 如果只将资源文件传递给 Al.exe，则输出文件为附属资源程序集。|
|**/link[resource]:** `file`[,`name`[,`target`[,`private`]]]|将资源文件链接到程序集。 `file` 指定的资源成为程序集的组成部分；不复制该文件。 `file` 参数可以是任何文件格式。 例如，可以指定本机 DLL 作为 `file` 参数。 这将使本机 DLL 成为此程序集的组成部分，从而可将它安装到全局程序集缓存中，并且可从该程序集中的托管代码访问它。 也可以通过使用 **/linkresource** 编译器选项实现该目的。 有关详细信息，请参阅 [-linkresource（C# 编译器选项）](../../csharp/language-reference/compiler-options/resources.md#linkresources)。<br /><br /> `name` 参数是资源的内部标识符。 `target` 参数指定 Al.exe 将 `file` 复制到其中的路径和文件名。 复制后，Al.exe 将 `target` 编译为程序集。 默认情况下，资源在程序集中是公共的（对于其他程序集可见）。 指定 `private` 会使该资源对于其他程序集不可见。<br /><br /> 例如，如果 `file` 是由资源文件生成器 (Resgen.exe) 创建的或在开发环境中创建的 .NET Framework 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。 有关详细信息，请参阅 <xref:System.Resources.ResourceManager>。 对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource*` 方法在运行时访问资源。<br /><br /> 如果只将资源文件传递给 Al.exe，则输出文件为附属资源程序集。|

可以指定以下 `options`；必须指定 **/out**。

| 选项 | 描述 |
| ------ | ----------- |
|**/algid:** `id`|指定一种算法来对多文件程序集中的所有文件（包含程序集清单的文件除外）进行哈希处理。 默认算法是 CALG_SHA1。 有关其他算法，请参见平台 SDK 文档中的 ALG_ID。 对于 .NET Framework 的第一版，只有 CALG_SHA1 和 CALG_MD5 是有效的。<br /><br /> 哈希值存储在程序集清单的文件表中。 在安装和加载时，会对照相应的哈希值检查程序集文件。<br /><br /> 还可以将此选项指定为任何模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyAlgorithmIdAttribute>)。|
|**/base[address]:** `addr`|指定一个地址，运行时在用户计算机上在该地址加载 DLL。 如果指定 DLL 的基址，而不是让操作系统在进程空间内重新定位 DLL，则应用程序的加载速度会更快。|
|**/bugreport**: `filename`|创建包含有关报告 Bug 的信息的文件 (`filename`)。|
|**/comp[any]:** `text`|为程序集中的“公司”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 `text` 会在文件资源管理器中显示为该文件的 `Company` 属性。 如果指定 **/win32res**，则所指定资源文件中的公司信息将在文件资源管理器中显示为 `Company` 属性。<br /><br /> 如果文本是空字符串 ("")，则 Win32 `Company` 资源会显示为一个空格。<br /><br /> 如果指定 **/win32res**，则 **/company** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyCompanyAttribute>)。|
|**/config[uration]:** `text`|为程序集中的“配置”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果文本是空字符串，则 Win32“配置”资源将显示为一个空格。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyConfigurationAttribute>)。|
|**/copy[right]:** `text`|为程序集中的“版权”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/copyright** 在文件资源管理器中将显示为 Win32“版权”资源。<br /><br /> 如果文本是空字符串，则 Win32 Copyright 资源将显示为一个空格。<br /><br /> 如果指定 **/win32res**，则 **/copyright** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyCopyrightAttribute>)。|
|**/c[ulture]:** `text`|指定要与程序集关联的区域性字符串。 区域性的有效值是名为“Tags for the Identification of Languages”的 Internet Requests for Comments (RFC) 文档 1766 定义的那些值。<br /><br /> 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 没有默认的区域性字符串。 使用反射可以查看此字符串。<br /><br /> 有关有效的 `text` 字符串的信息，请参见 <xref:System.Globalization.CultureInfo>。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyCultureAttribute>)。|
|**/delay[sign][+&#124;-]**|指定程序集是完全签名的还是部分签名的。 如果需要完全签名的程序集，请使用 **/delaysign-** 。 如果仅需要将公钥包含在程序集中，则使用 **/delaysign+** 。<br /><br /> 请求完全签名的程序集时，Al.exe 会对包含清单（程序集元数据）的文件进行哈希处理，并使用私钥对哈希进行签名。 产生的数字签名存储在包含清单的文件中。 在对程序集延迟签名时，Al.exe 不会计算和存储签名，而只是在文件中保留空间以便稍后可添加该签名。<br /><br /> 默认值为 **/delaysign-** 。<br /><br /> 除非与 **/keyfile** 或 **/keyname** 一同使用，否则 **/delaysign** 选项将不起作用。<br /><br /> 例如，使用 **/delaysign+** 可允许测试人员将程序集放入全局缓存中。 测试完成后，可以通过将私钥包含在程序集中来对程序集进行完全签名。<br /><br /> 注意：使用 [Gacutil.exe（全局程序集缓存工具）](gacutil-exe-gac-tool.md)将延迟签名的程序集放入全局缓存之前，请使用 [Sn.exe（强名称工具）](sn-exe-strong-name-tool.md)来注册该程序集以跳过验证 。 例如 `Sn.exe –Vr delaySignedAssembly`。 仅将它用于开发。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyDelaySignAttribute>)。|
|**/descr[iption]:** `text`|为程序集中的 <xref:System.Reflection.AssemblyDescriptionAttribute.Description%2A> 字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/description** 在文件资源管理器中将显示为 Win32“注释”资源。<br /><br /> 如果文本是空字符串，则 Win32“注释”资源将显示为一个空格。<br /><br /> 如果指定 **/win32res**，则 **/description** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyDescriptionAttribute.Description%2A>)。|
|**/e[vidence]:** `file`|使用 Security.Evidence 的资源名将 `file` 嵌入程序集中。<br /><br /> 不能对常规资源使用 Security.Evidence。|
|**/fileversion:** `version`|为程序集中的“文件版本”字段指定字符串。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/fileversion** 将用作 Win32“文件版本”资源。 如果不指定 **/fileversion**，则 Win32“文件版本”资源将由 Win32“程序集版本”资源填充。<br /><br /> 如果指定 **/win32res**，则 **/fileversion** 不会影响 Win32 资源。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (AssemblyFileVersionAttribute)。|
|**/flags:** `flags`|为程序集中的 `Flags` 字段指定一个值。 `flags` 的可能的值有：<br /><br /> 0x0000<br /> 程序集是相邻兼容的。<br /><br /> 0x0010<br /> 程序集无法与其他版本在同一应用程序域中一起执行。<br /><br /> 0x0020<br /> 程序集无法与其他版本在同一进程中一起执行。<br /><br /> 0x0030<br /> 程序集无法与其他版本在同一计算机上一起执行。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyFlagsAttribute>)。|
|**/fullpaths**|使 Al.exe 对错误消息中报告的任何文件使用绝对路径。|
|**/help**|显示该工具的命令语法和选项。|
|**/keyf[ile]:** `filename`|指定一个文件 (`filename`)，该文件包含密钥对或只包含用于对程序集进行签名的公钥。 编译器在程序集清单中插入公钥，然后使用私钥对最终的程序集进行签名。 有关生成密钥文件并将密钥对安装到密钥容器中的信息，请参见[强名称工具 (Sn.exe)](sn-exe-strong-name-tool.md)。<br /><br /> 如果使用延迟签名，此文件通常会具有公钥而不是私钥。<br /><br /> （密钥对的）公钥信息显示在程序集的 .publickey 字段中。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyKeyFileAttribute>)。<br /><br /> 如果在同一编译中同时指定 /keyfile 和 /keyname（通过命令行选项或通过自定义属性），则 Al.exe 将首先尝试用 /keyname 指定的容器 。 如果成功，则使用密钥容器中的信息对程序集签名。 如果 Al.exe 没有找到密钥容器，它将尝试用 /keyfile 指定的文件。 如果成功，则使用密钥文件中的信息对程序集签名，并且将密钥信息安装到密钥容器中（类似于 [Sn.exe](sn-exe-strong-name-tool.md) 中的 -i 选项），以便在下一次编译中，/keyname 选项将生效。|
|**/keyn[ame]:** `text`|指定保存密钥对的容器。 这样将会通过将公钥插入程序集清单来对程序集签名（为它指定一个强名称）。 然后，Al.exe 使用私钥对最终程序集进行签名。<br /><br /> 使用 Sn.exe 生成密钥对。<br /><br /> 密钥信息显示在程序集的 .publickey 字段中。<br /><br /> 如果有嵌入的空格，请用双引号 (" ") 将 `text` 引起来。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyKeyNameAttribute>)。|
|**/main:** `method`|指定方法的完全限定名称 (`class`.`method`)，以用作将模块转换为可执行文件时的入口点。|
|**/nologo**|调用 Al.exe 时，在命令行取消显示横幅或徽标。|
|**/out:** `filename`|指定 Al.exe 生成的文件的名称。 这是必需选项。|
|**/platform:** `text`|限制可以运行该代码的平台；必须为 x86、Itanium、x64、anycpu（默认值）之一，或 anycpu32bitpreferred。|
|**/prod[uct]:** `text`|为程序集中的“产品”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/product** 在文件资源管理器中将显示为 Win32“产品名称”资源。<br /><br /> 如果文本是空字符串，则 Win32“产品名称”资源将会显示为一个空格。<br /><br /> 如果指定 **/win32res**，则 **/product** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyProductAttribute>)。|
|**/productv[ersion]:** `text`|为程序集中的“产品版本”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/productversion** 将用作 Win32“产品版本”资源。 如果不指定 **/productversion**，则 Win32“产品版本”资源将由 Win32“文件版本”资源填充。<br /><br /> 如果指定 **/win32res**，则 **/productversion** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyInformationalVersionAttribute>)。|
|**/t[arget]:** `lib[rary]` &#124; `exe` &#124; `win[exe]`|指定输出文件的文件格式：`lib[rary]`（代码库）、`exe`（控制台应用程序）或 `win[exe]`（基于 Windows 的应用程序）。 默认值为 `lib[rary]`。|
|**/template:** `filename`|指定程序集 `filename`，除区域性字段之外的所有程序集元数据都从该程序集继承。<br /><br /> 使用 **/template** 创建的程序集将成为附属程序集。|
|**/title:** `text`|为程序集中的“标题”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/title** 在文件资源管理器中将显示为 Win32“说明”资源，shell 将其用作应用程序的友好名称。 如果某个文件类型有多个支持应用程序，则该字符串也会出现在此文件类型的快捷菜单的“打开方式”子菜单中。<br /><br /> 如果文本是空字符串，则 Win32“说明”资源将显示为一个空格。<br /><br /> 如果指定 **/win32res**，则 **/title** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyTitleAttribute>)。|
|**/trade[mark]:** `text`|为程序集中的“商标”字段指定字符串。 如果 `text` 包含空格，则将字符串放置在双引号 (" ") 中。 此字符串是程序集上的自定义特性，可以使用反射进行查看。<br /><br /> 如果不指定 **/win32res**，则 **/trademark** 在文件资源管理器中将显示为 Win32“商标”资源。<br /><br /> 如果文本是空字符串，则 Win32“商标”资源将显示为一个空格。<br /><br /> 如果指定 **/win32res**，则 **/trademark** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyTrademarkAttribute>)。|
|**/v[ersion]:** `version`|指定此程序集的版本信息。 版本字符串的格式为 `major`.`minor`.`build`.`revision`。默认值为 0。<br /><br /> 如果指定 **/version**，则必须指定 `major`。 如果指定 `major` 和 `minor`，则可以为 `build` 指定星号 (\*)。 这会使 `build` 等于从当地时间 2000 年 1 月 1 日算起的天数，使 `revision` 等于从当地时间当日午夜算起的秒数的一半。<br /><br /> 如果指定 `major`、`minor` 和 `build`，则可以指定一个星号作为 `revision`。 这会使 `revision` 等于从当地时间当地午夜算起的秒数的一半。<br /><br /> 概括而言，有效的版本字符串如下：<br /><br /> X<br /><br /> X.X<br /><br /> X.X.\*<br /><br /> X.X.X<br /><br /> X.X.X.\*<br /><br /> X.X.X.X<br /><br /> 其中，X 是 0 至 65534 之间（不含 65535）的任何一个无符号短常数。<br /><br /> 如果不指定 **/win32res**，则 **/version** 将用作 Win32“程序集版本”资源。<br /><br /> 如果不指定 **/win32res**、 **/productversion** 和 **/fileversion**，则 **/version** 将用于“程序集版本”、文件版本和“产品版本”Win32 资源。<br /><br /> 如果指定 **/win32res**，则 **/version** 将不会影响 Win32 资源信息。<br /><br /> 还可以将此选项指定为任何 MSIL 模块的源代码中的自定义特性 (<xref:System.Reflection.AssemblyVersionAttribute>)。|
|**/win32icon:** `filename`|在程序集中插入 .ico 文件。 .ico 文件在文件资源管理器中赋予输出文件所需的外观。|
|**/win32res:** `filename`|在输出文件中插入 Win32 资源（.res 文件）。 可使用资源编译器创建 Win32 资源文件。 在编译 Visual C++ 程序时会调用资源编译器；.res 文件是从 .rc 文件创建的。|
|`@filename`|指定包含 Al.exe 命令的响应文件。<br /><br /> 响应文件中的命令既可以每行显示一个，也可以显示在同一行中，用一个或多个空格分隔。|
|**/?**|显示该工具的命令语法和选项。|

## <a name="remarks"></a>备注

所有 Visual Studio 编译器都产生程序集。 但是，如果有一个或多个模块（没有清单的元数据），则可使用 Al.exe 在单独的文件中创建带清单的程序集。

若要在缓存中安装程序集，从缓存中删除程序集或列出缓存内容，请使用[全局程序集缓存工具 (Gacutil.exe)](gacutil-exe-gac-tool.md)。

## <a name="errors-and-warnings"></a>错误和警告

下表列出了 Al.exe 生成的错误。

| 错误 | 描述 |
| ----- | ----------- |
|al1001|内部编译器错误<br /><br /> 尝试确定 Al.exe 是否因为无法分析意外语法而失败。 然后，请与 Microsoft 产品支持服务联系。|
|al1002|内存不足<br /><br /> Al.exe 内存不足，已停止。 增加可用内存量。|
|al1003|编译器选项“option”后面必须有参数<br /><br /> Al.exe 预期一个自变量被传递到命令行选项。 例如，如果指定 **/algid:** ，则必须传递算法标识符。|
|al1004|意外的公共语言运行时初始化错误 --“reason”<br /><br /> Al.exe 报告了 Visual Studio 的安装错误，或者特定原因导致的公共语言运行时错误。|
|al1005|文件“file”太大，无法打开<br /><br /> Al.exe 打开的所有文件都必须小于 4 GB。|
|al1006|已包含响应文件“file”<br /><br /> 在命令行上多次指定了同一个响应文件 (`@file`)。 响应文件只能包含一次。|
|al1007|打开响应文件“file”时出错 —“reason”<br /><br /> Al.exe 因特定原因无法打开指定的响应文件。|
|al1008|缺少“option”命令行选项的文件规范<br /><br /> Al.exe 预期一个文件被传递到命令行选项。 例如，如果指定 **/out** 选项，则必须指定文件。|
|al1009|无法打开“file”以进行写入<br /><br /> Al.exe 无法写入文件，例如输出程序集文件。 磁盘可能已满，该文件可能是只读的，或者你可能不具有对该文件的权限。|
|al1010|命令行语法错误：“选项”选项缺少“:text”<br /><br /> Al.exe 预期一个自变量被传递到命令行选项。 例如，如果指定 **/title** 选项，则必须传递字符串。|
|al1011|文件“file”是可执行文件，无法作为文本文件打开<br /><br /> 在预期为文本文件之处指定了二进制文件。 例如，如果将二进制文件作为响应文件传递到命令行上，将发生此错误。|
|al1012|“value”不是选项“option”的有效设置<br /><br /> 意外值被传递到命令行选项。 例如，如果将无效值指定到 **/target** 选项，则将发生此错误。|
|al1013|无法识别的命令行选项:“option”<br /><br /> 已指定无效的命令行选项。|
|al1014|意外的初始化错误 —“reason”<br /><br /> Al.exe 检测到 COM 初始化失败。 这可能是由于缺少内存所致，但更可能是因系统 DLL 文件所致。 如果运行任何自动化感知或 COM 感知的程序（如 Microsoft Visual Studio），应会发生类似的错误。<br /><br /> 重新安装操作系统。|
|al1015|无法找到消息文件“alinkui.dll”<br /><br /> Al.exe 需要 Alinkui.dll 。 确保此文件位于你的路径。 如有必要，请从产品 CD 将其复制。|
|al1016|未指定有效输入文件<br /><br /> Al.exe 要求不具有程序集信息的一个或多个输入文件。|
|al1017|未指定目标文件名<br /><br /> 缺少指定目标文件名必需的 **/out** 选项。|
|al1018|无法加载所需的文件“file”<br /><br /> 无法加载某些 DLL 文件。 重新安装 Visual Studio 或 Windows SDK。|
|al1019|创建程序集时元数据失败 — 原因<br /><br /> 由于特定原因，程序集生成中断。 例如，如果未找到用 **/win32res** 选项指定的文件，将发生此错误。|
|al1020|忽略包含的程序集“file”<br /><br /> 指定了包含程序集的输入文件。 Al.exe 输入文件无法包含程序集。|
|al1021|“setting”: 重写以前的设置<br /><br /> 模块具有特定设置的值，该值可能是通过自定义属性分配，并通过使用 Al.exe 命令行选项传递的值进行重写。|
|al1022|读取嵌入资源“file”时出错 — 原因<br /><br /> 由于特定原因，Al.exe 无法读取传递到 /embedresource 选项的文件。|
|al1023|嵌入资源“file”时出错 — 原因<br /><br /> 由于特定原因，操作系统无法在程序集中嵌入资源文件。|
|al1025|ComType 记录“record”指向无效的文件记录“record”<br /><br /> 输入模块中的元数据无效。 必须修复生成该模块的工具。|
|al1026|指定的版本“version”无效<br /><br /> 查看有关有效格式的 **/version** 选项的信息。|
|al1028|密钥文件“file”缺少签名所需的私钥<br /><br /> 已将仅包含公钥的密钥文件传递到 **/keyfile** 选项。 如以下命令所示，使用[强名称工具 (Sn.exe)](sn-exe-strong-name-tool.md) 以生成同时具有公钥和私钥的文件。<br /><br /> `sn -k keypair.snk.`|
|al1029|密钥容器名称“container”不存在<br /><br /> 传递到 **/keyname** 选项的值不是有效的容器。 使用[强名称工具 (Sn.exe)](sn-exe-strong-name-tool.md) 创建容器。|
|al1030|未正确安装加密服务或其不具有合适的密钥提供程序<br /><br /> 可能需要重新安装操作系统，或者安装一些用于创建该密钥的加密实用工具。|
|al1031|读取图标“file”时出错 — 原因<br /><br /> 由于特定原因，Al.exe 无法读取传递到 /win32icon 选项的文件|
|al1032|为“file”生成资源时出错 — 原因<br /><br /> 由于没有足够的磁盘空间或某些其他错误，Al.exe 无法创建文件。 当指定 **/win32icon** 选项（生成 .ico 文件）或不指定 **/win32res** 选项（生成具有资源信息的文件）时，将发生此错误。<br /><br /> 如果无法解决文件生成问题，则使用 **/win32res**，可指定可以包含版本或位图（图标）信息的文件。|
|al1033|多次使用不同值指定了程序集自定义特性“attribute”<br /><br /> 已将不同的值传递给源模型中同一自定义属性的两个匹配项，此源模型指定为 Al.exe 的输入。|
|al1034|无法复制或重命名程序集“file”<br /><br /> 使用既可使用户指定输入文件，又可复制输入文件的 Al.exe 语法时，会产生名称冲突，导致编译器停止。 例如，如果指定 `input.dll,somename.dll /out:somename.dll`，将产生此错误。|
|al1035|库不能有一个入口点<br /><br /> 不能同时指定 **/target:lib** 选项（默认）和 **/main** 选项。|
|al1036|可执行应用程序所需的入口点<br /><br /> 当使用 **/target:exe** 或 **/target:win** 选项时，还必须指定 **/main** 选项。|
|al1037|无法找到入口点方法“main”<br /><br /> Al.exe 在 /main 选项指定的位置找不到 `Main` 方法。|
|al1039|全局程序集缓存管理器的初始化失败 — 原因<br /><br /> 重新安装 Visual Studio 或 Windows SDK。|
|al1040|未能将程序集安装到缓存 — 原因<br /><br /> 仅签名的程序集可安装到缓存中。 有关详细信息，请参阅[全局程序集缓存](../app-domains/gac.md)。|
|al1041|“method”: 不能为入口点，因为签名或可见性不正确，或者它是泛型<br /><br /> 使用 **/main** 选项指定了一种方法，但该方法不是静态的，不会返回 `int` 或 `void`，该方法是泛型类型，或者具有无效自变量。|
|al1042|“exe”：不能向 EXE 添加模块<br /><br /> 已将不具有程序集的 .exe 文件指定为 Al.exe 的输入文件 。 Al.exe 只能接受不具有程序集的 .dll 文件作为输入文件 。|
|al1043|清单文件名“name”不能和任何模块相同<br /><br /> 使用 /out 选项指定的名称不能与任何一个指定为 Al.exe 的输入文件的名称相同。|
|al1044|读取密钥文件“file”时出错 — 原因<br /><br /> 从使用 /keyfile 或 <xref:System.Reflection.AssemblyKeyFileAttribute> 指定的文件打开或读取时出错。|
|al1045|文件名“file”太长或无效<br /><br /> 将长于 260 个字符的文件名传递给 Al.exe。 选择字符较少或路径较短的文件名或重命名该文件。|
|al1046|此程序集中已使用了资源标识符“ID”<br /><br /> 内嵌或链接的两个资源具有相同的标识符或名称（第二个参数）。 删除或重命名其中一个冲突资源。|
|al1047|导入文件“file”时出错 — 原因<br /><br /> 由于特定原因，无法打开模块文件。|
|al1048|导入程序集“assembly”的模块“module”时出错 — 原因<br /><br /> 打开多文件程序集的非清单文件时出错。 此错误不直接由 Al.exe 发出，但可将它以编程的方式传递给使用 Al.exe 的进程 。|
|al1049|不能自动生成日期在 2000 年 1 月 1 日之前的生成版本号和修订版本号<br /><br /> 你计算机上的系统时钟设置为早于 2000 年 1 月 1 日的日期。|
|al1050|不再支持使用“旧功能”；请改为使用“新功能”<br /><br /> 以前由 Al.exe 支持的一项功能现已过时。 请改用推荐的功能。|
|al1051|发出“attribute”特性时出错 — 原因<br /><br /> 由于特定原因，Al.exe 未处理一项程序集自定义属性。|
|al1052|文件“filename”不是程序集<br /><br /> 使用 **/template** 指定的文件必须包含程序集元数据。 此错误指示 **/template** 指定的文件未包含程序集。|
|al1053|为“option”指定的版本“version”不是常规的“major.minor.build.revision”格式<br /><br /> Al.exe 检测到使用 /fileversion 或 /productversion 选项指定的格式不正确的版本信息 。|
|al1054|为“option”指定的版本“version”不是常规的“major.minor.build.revision”格式<br /><br /> Al.exe 检测到使用 <xref:System.Resources.SatelliteContractVersionAttribute> 指定的格式不正确的版本信息。|
|al1055|引用的程序集“filename”没有强名称<br /><br /> 当构建具有强名称的程序集，并且引用不具有强名称的程序集时，会发出此错误。 若要解决此问题，必须再生成具有强名称的程序集，或通过使用 sn.exe 将强名称附加到程序集（请参阅 [sn.exe](sn-exe-strong-name-tool.md) 的文档） 。<br /><br /> 此错误通常发生在通过包装程序集使用 COM 对象时，如通过 Visual Studio IDE 将对 COM 模块的引用添加到 C# 项目时。 若要避免此错误，可以在项目属性“包装程序集密钥文件/名称”中指定 COM 包装程序集的强名称密钥文件<br /><br /> 如果要通过 tlbimp 创建包装程序集，请参阅 [tlbimp](tlbimp-exe-type-library-importer.md) 文档了解有关如何向包装程序集分配强名称的信息。<br /><br /> 如果程序集具有强名称，则它可以安装在全局程序集缓存中。 由此，引用的程序集也将进入全局程序集缓存。 只有具有强名称的程序集才可进入全局程序集缓存。|
|al1056|引用的程序集“filename”是已本地化的附属程序集<br /><br /> 在创建当前程序集时引用了通过使用 <xref:System.Reflection.AssemblyCultureAttribute> 特性创建的程序集。 <xref:System.Reflection.AssemblyCultureAttribute> 特性指示该文件是已本地化的附属程序集，并且不合适引用附属程序集。 改为引用主要父程序集。|
|al1057|可执行文件不能进行本地化；区域性应始终为空<br /><br /> 正在使用 **/target:exe** 创建程序集，但指定了 **/culture**。 .exe 中的程序集不能具有区域性字段中的信息。|
|al1058|“file”是一个程序集，不能作为模块添加<br /><br /> 在 C++ 编译中，向 **/assemblymodule**（链接器选项）传递了包含程序集的文件。|
|al1059|未知错误（代码）<br /><br /> Al.exe 收到了未知错误代码 (`code`)。<br /><br /> 可能的解决方案包括以下措施：<br /><br /> 重新安装 Visual Studio。<br /><br /> 重新安装 Windows SDK。<br /><br /> 检查是否缺少文件。<br /><br /> 检查是否有足够的磁盘空间。<br /><br /> 检查是否有足够的内存。<br /><br /> 停止可能正在访问这些文件的其他进程。<br /><br /> 重新启动计算机。|
|al1060|创建哈希时加密失败 — 原因<br /><br /> 为多文件程序集创建文件哈希值时出错。|
|al1061|由于“reason”，无法设置选项“option”<br /><br /> 由于指定原因，为此选项指定的值无效。|
|al1062|已多次指定了模块“module”；它将仅被包含一次<br /><br /> 当在命令行上多次指定相同的源、输入或模块文件时，将生成此警告。 确保仅指定该文件名称一次。|
|al1063|在此程序集的多个位置定义了公共类型“type”:“file1”和“file2”<br /><br /> 在程序集的不止一个模块中找到相同类型。 每种类型在一个程序集中只能有一个版本。|
|al1064|无法指定多个 /bugreport 选项。<br /><br /> 仅允许一个 **/bugreport** 选项。|
|al1065|文件名“File Name”太长或无效<br /><br /> 指定的文件名长于允许的最大长度。|
|al1066|在命令行上或响应文件中不允许有字符“character”<br /><br /> 在命令行上或在文件中找到了无效的字符。|
|al1067|“filename”是二进制文件而非文本文件<br /><br /> 该文件是二进制格式，而不是文本格式。|
|al1068|已在此程序集中定义了模块“ModuleName”。 每个链接的资源和模块必须具有唯一的文件名。<br /><br /> 该模块在此程序集中不止出现一次。|
|al1069|当已存在具有相同短文件名的长文件名时，无法创建短文件名“filename”<br /><br /> 当前文件的名称为已存在的文件的名称的较短形式。 例如，编译 LongFileName.cs，然后使用名称 LongFi~1.cs 重新编译将导致类似于此的编译器错误。 如果已删除了具有长名称的编译器输出文件，但依然存在类似的链接器文件，则可能会出现此错误。|
|al1070|不可知的程序集不能具有特定于处理器的模块“Module Name”<br /><br /> 如果正在使用 /platform:agnostic（或没有指定 /platform）进行构建，此时若尝试添加一个并非是不可知的模块（使用 /addmodule），将生成错误。 这就像尝试将 i386 obj 文件链接到 ia64 obj。<br /><br /> 非不可知模块的主要来源是 C++。 如果正在使用 **/addmodule** 与 C++ 模块，可能需要修改生成脚本以指定合适的 **/platform** 设置。|
|al1072|程序集和模块“Module Name”不能以不同处理器为目标<br /><br /> 不能链接针对不同处理器的程序集和模块，因为必须在单个处理器上运行结果。|
|al1073|引用的程序集“assembly”面向的是另一个处理器<br /><br /> 不能链接针对不同处理器的程序集，因为必须在单个处理器上运行结果。|
|al1074|存储在“File Name”中的模块名称“Module Name”必须与其文件名匹配<br /><br /> 这是链接器所必需的。 若要解决此问题，请使两个名称匹配。|
|al1075|已请求延迟签名，但未提供密钥<br /><br /> 在对程序集延迟签名时，编译器不会计算和存储签名，而只是在文件中保留空间以便稍后可添加签名。<br /><br /> 例如，使用 **/delaysign+** 可允许测试人员将程序集放入全局缓存中。 测试完成后，可使用“程序集链接器”实用工具将私钥添加到程序集，从而对程序集进行完整的签名。|
|al1076|将类型“type”转发到多个程序集:“assembly”和“assembly”。<br /><br /> 一种类型只能转发到一个程序集。|
|al1077|在“assembly”中定义公共类型“type”，并将其转发到“assembly”。<br /><br /> 正在生成的程序集中具有重复的公共类型。 一个是有效的类型定义，另一个是类型转发器。|

## <a name="example"></a>示例

以下命令使用 `t2.netmodule` 模块中的程序集创建可执行文件 t2a.exe。 入口点是 `Main` 中的 `MyClass` 方法。

```console
al t2.netmodule /target:exe /out:t2a.exe /main:MyClass.Main
```

## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Sn.exe（强名称工具）](sn-exe-strong-name-tool.md)
- [Gacutil.exe（全局程序集缓存工具）](gacutil-exe-gac-tool.md)
- [使用程序集编程](../../standard/assembly/index.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
