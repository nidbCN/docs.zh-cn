---
title: Installutil.exe（安装程序工具）
description: 使用 Installutil.exe（安装程序工具）。 此工具可用于通过执行指定程序集中的安装程序组件，安装或卸载服务器资源。
ms.date: 03/30/2017
helpviewer_keywords:
- uninstalling server resources
- removing server resources
- status information for installation
- installation progress reports
- installing server resources
- Installer tool
- Installutil.exe
- files, Installer tool
- progress information for installation
- reporting installation progress
ms.assetid: 3f9d0533-f895-4897-b4ea-528284e0241d
ms.openlocfilehash: 31420f21cdcbac65f5fc565dbf6db7a8d892fb9e
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654045"
---
# <a name="installutilexe-installer-tool"></a>Installutil.exe（安装程序工具）

安装程序工具是一个命令行实用工具，你可以通过此工具执行指定程序集中的安装程序组件，从而安装和卸载服务器资源。 此工具与 <xref:System.Configuration.Install> 命名空间中的类配合使用。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，键入以下内容：

## <a name="syntax"></a>语法

```console
installutil [/u[ninstall]] [options] assembly [[options] assembly] ...
```

## <a name="parameters"></a>参数

|参数|说明|
|--------------|-----------------|
|`assembly`|要在其中执行安装程序组件的程序集的文件名称。 如果你要通过使用 `/AssemblyName` 选项指定程序集的强名称，则忽略此参数。|

<a name="options"></a>

## <a name="options"></a>选项

|选项|描述|
|------------|-----------------|
|`/h[elp]`<br /><br /> \- 或 -<br /><br /> `/?`|显示该工具的命令语法和选项。|
|`/help` assembly<br /><br /> \- 或 -<br /><br /> `/?` assembly|显示由指定的程序集中的个别安装程序识别的其他选项，以及 InstallUtil.exe 的命令语法和选项。 此选项将各安装程序组件的 <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> 属性返回的文本添加到 InstallUtil.exe 的帮助文本。|
|`/AssemblyName` "*assemblyName*<br /><br /> ,Version=*major.minor.build.revision*<br /><br /> ,Culture=*locale*<br /><br /> ,PublicKeyToken=*publicKeyToken*"|指定必须在全局程序集缓存中注册的程序集的强名称。 必须使用程序集的版本、区域性和公钥标记完全限定程序集名称。 完全限定名必须用引号括起。<br /><br /> 例如，“myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0”是完全限定的程序集名称。|
|`/InstallStateDir=[` directoryName `]`|指定 .InstallState 文件的目录，其中包含用来卸载程序集的数据。 默认为包含程序集的目录。|
|`/LogFile=`[*filename*]|指定在其中记录安装进度的日志文件的名称。 默认情况下，如果省略 `/LogFile` 选项，则会创建名为 *assemblyname*.InstallLog 的日志文件。 如果省略 *filename*，则不生成任何日志文件。|
|`/LogToConsole`={`true`&#124;`false`}|如果为 `true`，则会将输出显示到控制台。 如果为 `false`（默认值），则取消将输出显示到控制台。|
|`/ShowCallStack`|如果在安装过程中的任何时候出现异常，则将调用堆栈输出到日志文件。|
|`/u`[`ninstall`]|卸载指定的程序集。 与其他选项不同，`/u` 选项不论出现在命令行的什么位置，都将应用于所有程序集。|

<a name="cmdline"></a>

## <a name="additional-installer-options"></a>其他安装程序选项

程序集中使用的独立安装程序可识别除[选项](#options)部分列出的选项之外的选项。 要了解有关这些选项的信息，可以在命令行中运行带有程序集路径的 InstallUtil.exe 以及 `/?` 或 `/help` 选项。 要指定这些选项，请将它们与 InstallUtil.exe 可识别的选项一起包含在命令行中。

> [!NOTE]
> 由单独的安装程序组件所支持的选项的帮助文本由 <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> 属性返回。 已经在命令行中输入的单个选项可通过编程方式从 <xref:System.Configuration.Install.Installer.Context%2A?displayProperty=nameWithType> 属性进行访问。

所有选项和命令行参数都将写入到安装日志文件中。 但是，如果你使用 `/Password` 参数（某些安装程序组件可识别这些参数），密码信息将被替换为八个星号 (*)，且不会出现在日志文件中。

> [!IMPORTANT]
> 在某些情况下，传递给安装程序的参数可能包含敏感信息或个人身份信息，默认情况下，此信息将写入到纯文本日志文件中。 要阻止此行为，可以通过在命令行中的 Installutil.exe 后指定 `/LogFile=`（没有 *filename* 参数）来禁止日志文件。

## <a name="remarks"></a>备注

.NET Framework 应用程序由传统的程序文件和关联资源组成，如必须在部署应用程序时创建的消息队列、事件日志和性能计数器。 安装应用程序时可以使用程序集的安装程序组件创建这些资源，而在卸载应用程序时可以使用这些组件删除这些资源。 Installutil.exe 检测并执行这些安装程序组件。

可以在同一个命令行中指定多个程序集。 出现在一个程序集名称前面的任何选项都将应用于该程序集的安装。 除了 `/u` 和 `/AssemblyName` 之外，选项可以是累积的，但也是可重写的。 就是说，为一个程序集指定的选项适用于所有后续程序集，除非为该选项指定新值。

如果对某个程序集运行 Installutil.exe 但不指定任何选项，则 Installutil.exe 会将下面三个文件放到该程序集的目录中：

- InstallUtil.InstallLog - 包含安装进度的常规说明。

- *assemblyname*.InstallLog - 包含特定于安装过程的提交阶段的信息。 关于提交阶段的更多信息，请参见 <xref:System.Configuration.Install.Installer.Commit%2A> 方法。

- *assemblyname*.InstallState - 包含用于卸载该程序集的数据。

Installutil.exe 使用反射检查指定的程序集以及查找将 <xref:System.Configuration.Install.Installer> 特性设置为 <xref:System.ComponentModel.RunInstallerAttribute?displayProperty=nameWithType> 的所有 `true` 类型。 该工具然后对 <xref:System.Configuration.Install.Installer.Install%2A?displayProperty=nameWithType> 类型的每个实例执行 <xref:System.Configuration.Install.Installer.Uninstall%2A?displayProperty=nameWithType> 或 <xref:System.Configuration.Install.Installer> 方法。 Installutil.exe 以事务性方式执行安装；也就是说，如果有一个程序集未能安装，则 Installutil.exe 会回滚其他所有程序集的安装。 卸载不是事务性的。

Installutil.exe 无法安装或卸载延迟签名的程序集，但可以安装或卸载具有强名称的程序集。

从 .NET Framework 2.0 版开始，32 位版本的公共语言运行时 (CLR) 仅随 32 位版本的安装程序工具一起提供，但 64 位版本的 CLR 随 32 位和 64 位版本的安装程序工具一起提供。 当使用 64 位 CLR 时，使用 32 位安装程序工具可安装 32 位程序集，使用 64 位安装程序工具可安装 64 位和 Microsoft 中间语言 (MSIL) 程序集。 这两种版本的安装程序工具的行为方式相同。

你无法使用 Installutil.exe 来部署使用 C++ 创建的 Windows 服务，因为 Installutil.exe 无法识别 C++ 编辑器生成的嵌入式本机代码。 如果尝试使用 Installutil.exe 部署 C++ Windows 服务，则会引发异常（如 <xref:System.BadImageFormatException>）。 要处理这种情况，请将服务代码移到 C++ 模块，然后使用 C# 或 Visual Basic 编写安装程序对象。

## <a name="examples"></a>示例

下列命令显示了 InstallUtil.exe 的命令语法和选项的说明。

```console
installutil /?
```

下列命令显示了 InstallUtil.exe 的命令语法和选项的说明。 如果已向安装程序的 `myAssembly.exe` 属性分配了帮助文本，则它还会在 <xref:System.Configuration.Install.Installer.HelpText%2A?displayProperty=nameWithType> 中显示安装程序组件支持的选项的说明的列表。

```console
installutil /? myAssembly.exe
```

下面的命令执行 `myAssembly.exe` 程序集中的安装程序组件。

```console
installutil myAssembly.exe
```

下面的命令通过使用 `/AssemblyName` 开关和完全限定名执行程序集中的安装程序组件。

```console
installutil /AssemblyName "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0"
```

以下命令执行一个以文件名指定的程序集和一个以强名称指定的程序集中的安装程序组件。 请注意，以文件名称指定的所有程序集必须在以强名称指定的程序集前面，因为无法重写 `/AssemblyName` 选项。

```console
installutil myAssembly.exe /AssemblyName "myAssembly, Culture=neutral, PublicKeyToken=0038abc9deabfle5, Version=4.0.0.0"
```

下面的命令执行 `myAssembly.exe` 程序集中的卸载程序组件。

```console
installutil /u myAssembly.exe
```

以下命令执行程序集 `myAssembly1.exe` 和 `myAssembly2.exe` 中的卸载程序组件。

```console
installutil myAssembly1.exe /u myAssembly2.exe
```

由于 `/u` 选项在命令行中所处的位置不重要，因此这就等效于以下命令。

```console
installutil /u myAssembly1.exe myAssembly2.exe
```

下面的命令执行 `myAssembly.exe` 程序集中的安装程序并指定将进度信息写入到 `myLog.InstallLog` 中。

```console
installutil /LogFile=myLog.InstallLog myAssembly.exe
```

以下命令执行程序集 `myAssembly.exe` 中的安装程序，指定将进度信息写入到 `myLog.InstallLog` 中，并使用安装程序的自定义 `/reg` 选项来指定应对系统注册表进行更新。

```console
installutil /LogFile=myLog.InstallLog /reg=true myAssembly.exe
```

以下命令执行程序集 `myAssembly.exe` 中的安装程序，使用安装程序的自定义选项 `/email` 来指定用户的电子邮件地址，并禁止输出到日志文件。

```console
installutil /LogFile= /email=admin@mycompany.com myAssembly.exe
```

下面的命令将 `myAssembly.exe` 的安装进度写入到 `myLog.InstallLog` 中，并将 `myTestAssembly.exe` 的进度写入到 `myTestLog.InstallLog` 中。

```console
installutil /LogFile=myLog.InstallLog myAssembly.exe /LogFile=myTestLog.InstallLog myTestAssembly.exe
```

## <a name="see-also"></a>请参阅

- <xref:System.Configuration.Install>
- [工具](index.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
