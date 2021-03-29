---
title: Fuslogvw.exe（程序集绑定日志查看器）
description: 使用 Fuslogvw.exe（程序集绑定日志查看器）。 此查看器显示程序集绑定的详细信息，这有助于诊断 .NET 无法在运行时找到程序集的原因。
ms.date: 03/30/2017
helpviewer_keywords:
- failed assembly binds
- Fuslogvw.exe
- displaying failed assembly bind details
- assemblies [.NET Framework], failed assembly binds
- locating assemblies
- Assembly Binding Log Viewer
ms.assetid: e32fa443-0778-4cc3-bf36-5c8ea297d296
ms.openlocfilehash: 6d6482edd3b8c78ad230cfc34ed84bd5db712fde
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654149"
---
# <a name="fuslogvwexe-assembly-binding-log-viewer"></a>Fuslogvw.exe（程序集绑定日志查看器）

程序集绑定日志查看器显示程序集绑定的详细信息。 这些信息有助于你诊断 .NET Framework 无法在运行时找到程序集的原因。 这些失败通常由以下因素导致：部署到错误位置的程序集、不再有效的本机映像或者版本号或区域性不匹配。 如果公共语言运行时未能找到程序集，则通常会在你的应用程序中表现为 <xref:System.TypeLoadException>。

> [!IMPORTANT]
> 你必须使用管理员特权运行 fuslogvw.exe。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请通过管理员凭据使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处，输入下列命令：

```console
fuslogvw
```

查看器将为每个失败的程序集绑定显示一个条目。 对于每一次失败，查看器都会描述：

- 启动了绑定的应用程序
- 绑定的程序集，包括名称、版本、区域性和公钥
- 失败的日期和时间

## <a name="how-to"></a>如何

- [更改日志位置视图](#change-the-log-location-view)
- [查看有关特定失败的详细信息](#view-details-about-a-specific-failure)
- [删除条目](#delete-entries)
- [刷新用户界面](#refresh-the-user-interface)
- [更改日志设置](#change-the-log-settings)
- [查看“关于”对话框](#view-the-about-dialog)

### <a name="change-the-log-location-view"></a>更改日志位置视图

1. 选择“默认”选项按钮以查看所有应用程序类型的绑定失败。 默认情况下，日志条目存储在磁盘上 wininet 缓存中基于用户的目录中。

2. 选择“自定义”选项按钮以查看指定的自定义目录中的绑定失败。 必须指定希望运行时存储日志的自定义位置，方法是在“日志设置”对话框中将自定义日志位置设置为有效的目录名。 此目录应是干净的，并且仅包含运行时所生成的文件。 如果该目录中包含了一个生成要记录下来的失败的可执行文件，则将不会记录该失败，因为此工具会尝试创建一个与该可执行文件同名的目录。 此外，从日志位置运行可执行文件的尝试也将失败。

    > [!NOTE]
    > 默认的绑定位置优于自定义绑定位置。 运行时将默认绑定位置存储在 wininet 缓存中，因而可以自动清除该位置。如果你指定了自定义绑定位置，则需负责将其清除。

### <a name="view-details-about-a-specific-failure"></a>查看有关特定失败的详细信息

1. 在查看器中选择所需条目的应用程序名称。

2. 单击“查看日志”按钮。 或者，你可以双击所选条目。

    该工具将显示以下有关所选绑定失败的详细信息：

    - 绑定失败的具体原因，例如“未找到文件”或“版本不匹配”。

    - 与启动绑定的应用程序有关的信息，包括其名称、应用程序的根目录 (AppBase) 以及专用搜索路径的说明（如果具有此类路径）。

    - 该工具要查找的程序集的标识。

    - 已经应用的任何应用程序、发行者或管理员版本策略的说明。

    - 是否已在[全局程序集缓存](../app-domains/gac.md)中找到了该程序集。

    - 所有探测 URL 的列表。

以下示例日志条目显示了与失败的程序集绑定有关的详细信息。

```output
*** Assembly Binder Log Entry  (3/5/2007 @ 12:54:20 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  C:\WINNT\Microsoft.NET\Framework\v2.0.50727\fusion.dll
Running under executable  C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\graphicfailtest.exe
--- A detailed error log follows.

=== Pre-bind state information ===
LOG: DisplayName = graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
 (Fully-specified)
LOG: Appbase = C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\
LOG: Initial PrivatePath = NULL
LOG: Dynamic Base = NULL
LOG: Cache Base = NULL
LOG: AppName = NULL
Calling assembly : graphicfailtest, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
===

LOG: Processing DEVPATH.
LOG: DEVPATH is not set. Falling through to regular bind.
LOG: Policy not being applied to reference at this time (private, custom, partial, or location-based assembly bind).
LOG: Post-policy reference: graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.EXE.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.EXE.
LOG: All probing URLs attempted and failed.
```

### <a name="delete-entries"></a>删除条目

从日志中删除一个条目：

1. 在查看器中选择一个条目。

2. 单击“删除条目”按钮。

从日志中删除所有条目：

- 单击“全部删除”按钮。

### <a name="refresh-the-user-interface"></a>刷新用户界面

- 单击“刷新”按钮。 查看器在其运行时不会自动检测新的日志条目。 必须使用“刷新”按钮来显示它们。

### <a name="change-the-log-settings"></a>更改日志设置

单击“设置”按钮以打开“日志设置”对话框 。

### <a name="view-the-about-dialog"></a>查看“关于”对话框

单击“关于”按钮。

## <a name="binding-logs-for-native-images"></a>本机映像的绑定日志

默认情况下，Fuslogvw.exe 将记录普通的程序集绑定请求。 此外，还可以记录使用 [Ngen.exe（本机映像生成器）](ngen-exe-native-image-generator.md)创建的本机映像的程序集绑定。

### <a name="log-assembly-binds-for-native-images"></a>记录本机映像的程序集绑定

- 在“日志类别”组中，选择“本机映像”选项按钮 。

下面的日志显示了一个由于在为应用程序创建本机映像时不存在的依赖项引起的失败。 如果运行时的依赖项不同于运行 Ngen.exe 时的依赖项，则不允许绑定至本机映像。

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:22:07 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\App.exe
--- A detailed error log follows.

LOG: Start binding of native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\App.exe.
LOG: Start validating native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency b, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
WRN: Dependency assembly was not found at ngen time, but is found at binding time. Disallow using this native image.
WRN: No matching native image found.
LOG: Bind to native image assembly did not succeed. Use IL image.
```

下面的日志显示了一个本机映像绑定失败，发生此失败的原因是，应用程序运行时的计算机上的安全设置不同于创建本机映像时的安全设置。

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:29:09 PM) ***

The operation failed.
Bind result: hr = 0x80004005. Unspecified error

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\Application101622.exe
--- A detailed error log follows.

LOG: Start binding of native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\Application101622.exe.
LOG: Start validating native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency Dependency101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Dependency evaluation succeeded.
LOG: Validation of dependencies succeeded.
LOG: Start loading all the dependencies into load context.
LOG: Loading of dependencies succeeded.
LOG: Bind to native image succeeded.
Native image has correct version information.
Attempting to use native image E:\Windows\assembly\NativeImages_v2.0.50727_64\Application101622\1ac7fadabec4f72575d807501e9fdc72\Application101622.ni.exe.
Rejecting native image because it failed the security check. The assembly's permissions must have changed since the time it was ngenned, or it is running with a different security context.
Discarding native image.
```

## <a name="the-log-settings-dialog"></a>“日志设置”对话框

可以使用“日志设置”对话框执行下列操作。

### <a name="to-disable-logging"></a>禁用日志记录

- 选择“已禁用日志”选项按钮。  注意，默认情况下此选项处于选中状态。

### <a name="to-log-assembly-binds-in-exceptions"></a>记录程序集绑定异常

- 选择“记录异常文本”选项按钮。 仅在异常文本中记录最低详细程度的合成日志信息。 若要查看完整信息，请使用其他设置之一。

  请参见有关以非特定域方式加载的程序集的“重要事项”说明。

### <a name="to-log-assembly-bind-failures"></a>记录程序集绑定失败

- 选择“记录失败绑定到磁盘”选项按钮。

  请参见有关以非特定域方式加载的程序集的“重要事项”说明。

### <a name="to-log-all-assembly-binds"></a>记录所有程序集绑定

- 选择“记录所有绑定到磁盘”选项按钮。

  请参见有关以非特定域方式加载的程序集的“重要事项”说明。

> [!IMPORTANT]
> 当程序集以非特定域方式加载时（例如，将 <xref:System.AppDomainSetup.LoaderOptimization%2A> 属性设置为 <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> 或 <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType>），打开日志记录可能会在某些情况下泄露内存。 如果在将非特定域的模块载入一个应用程序域中时记录日志条目，随后又卸载该应用程序域，则将可能出现这种情况。 在进程结束前可能不会释放该日志条目。 一些调试器会自动启用日志记录。

### <a name="to-enable-a-custom-log-path"></a>启用自定义日志路径

1. 选择“启用自定义日志路径”选项按钮。

2. 在“自定义日志路径”文本框中输入路径。

> [!NOTE]
> [程序集绑定日志查看器 (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) 使用 Internet Explorer (IE) 缓存来存储其绑定日志。 由于 IE 缓存中偶尔会出现损坏，因此[程序集绑定日志查看器 (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) 有时会停止在查看窗口中显示新的绑定日志。 受此损坏的影响，.NET 绑定基础结构（合成）将无法写入或读取绑定日志。 （如果使用自定义日志路径，则不会遇到此问题。）若要修复损坏并允许合成再次显示绑定日志，请通过在 IE 的“Internet 选项”对话框中删除临时的 Internet 文件来清除 IE 缓存。
>
> 如果非托管应用程序通过实现 `IHostAssemblyManager` 和 `IHostAssemblyStore` 接口承载公共语言运行时，则不能将日志条目存储在 wininet 缓存中。 若要查看实现这些接口的自定义宿主的日志条目，你必须指定替代日志路径。

### <a name="to-enable-logging-for-apps-running-in-the-windows-app-container"></a>为运行在 Windows 应用程序容器中的应用程序启用日志记录

1. 启用自定义日志路径，如上一过程所述。 默认情况下，在 Windows 应用程序容器中运行的应用程序对硬盘具有有限的访问权限。 你指定的目录将对应用程序容器中的所有应用程序具有读/写权限。

2. 选中“启用沉浸式日志记录”复选框。

    > [!NOTE]
    > 此框只能在 Windows 8 或更高版本上启用。

## <a name="see-also"></a>请参阅

- <xref:System.TypeLoadException>
- [工具](index.md)
- [全局程序集缓存](../app-domains/gac.md)
- [运行时如何定位程序集](../deployment/how-the-runtime-locates-assemblies.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
