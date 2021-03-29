---
title: Mpgo.exe（按托管配置文件优化工具）
description: 使用 Mpgo.exe（按托管配置文件优化工具）。 使用此工具，可以优化本机映像生成器 (Ngen.exe) 创建的本机映像程序集。
ms.date: 03/30/2017
helpviewer_keywords:
- Mpgo.exe
- training scenarios, generating profiles with
- Managed Profile Guided Optimization Tool
- Ngen.exe
- Ngen.exe, profilers and native images
ms.assetid: f6976502-a000-4fbe-aaf5-a7aab9ce4ec2
ms.openlocfilehash: 0336c3a98fb93a3b3d3689048e5c313b811630d9
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653902"
---
# <a name="mpgoexe-managed-profile-guided-optimization-tool"></a>Mpgo.exe（按托管配置文件优化工具）

托管配置文件引导式优化工具 (Mpgo.exe) 是一种命令行工具，使用常见的最终用户方案优化由[本机映像生成器 (Ngen.exe)](ngen-exe-native-image-generator.md) 创建的本机映像程序集。 利用此工具，你可运行生成配置文件数据的培训方案。 [本机映像生成器 (Ngen.exe)](ngen-exe-native-image-generator.md) 使用此数据优化其生成的本机映像应用程序程序集。 培训方案是应用程序预期用法的一种试运行。 Mpgo.exe 适用于 Visual Studio Ultimate 2012 及更高版本。 从 Visual Studio 2013 开始，还可以使用 Mpgo.exe 优化 Windows 8.x 应用商店应用。

通过收集培训方案的数据并使用它来优化本机映像布局，按配置优化改进了应用程序启动时间、内存利用率（工作集大小）和吞吐量。

当你遇到与中间语言 (IL) 程序集的启动时间和工作集大小相关的性能问题时，建议你先使用 Ngen.exe 消除实时 (JIT) 编译成本和推动代码共享。 如果你需要更多改进，则可使用 Mpgo.exe 进一步优化应用程序。 你可以使用未优化的本机映像程序集的性能数据作为计算性能提升的基准。 使用 Mpgo.exe 可以获得更短的冷启动时间和更小的工作集大小。 Mpgo.exe 将信息到添加 Ngen.exe 用于创建优化的本机映像程序集的 IL 程序集中。 有关详细信息，请参阅 .NET 博客中的 [Improving Launch Performance for your Desktop Applications](https://devblogs.microsoft.com/dotnet/improving-launch-performance-for-your-desktop-applications/)（提高桌面应用程序的启动性能）。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请通过管理员凭据使用[开发人员命令提示或开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

在命令提示符处输入以下命令：

对于桌面应用程序：

```console
mpgo –Scenario <command> [-Import <directory>] –AssemblyList <assembly1>  <assembly2> ... -OutDir <directory> [options]
```

对于 Windows 8.x 应用商店应用：

```console
mpgo –Scenario <packageName> -AppID <appId> -Timeout <seconds>
```

## <a name="parameters"></a>parameters

 Mpgo.exe 的所有自变量都区分大小写。 命令前加上短划线。

> [!NOTE]
> 你可以使用 `–Scenario` 或 `–Import` 作为必需命令，但不能两个都用。 如果指定 `–Reset` 选项，则不使用任何所需参数。

|必选参数|描述|
|------------------------|-----------------|
|`-Scenario` \<*command*><br /><br /> \- 或 -<br /><br /> `-Scenario` \<*packageName*><br /><br /> \- 或 -<br /><br /> `-Import` \<*directory*>|对于桌面应用，使用 `–Scenario` 指定命令来运行你要优化的应用程序，包括任何命令行参数。 如果 command 指定的路径包含空格，则应使用三组双引号将其引起来。例如：`mpgo.exe -scenario """C:\My App\myapp.exe""" -assemblylist """C:\My App\myapp.exe""" -outdir "C:\optimized files"`。 请勿使用双引号；如果 command 包含空格，使用双引号将不能正确发挥作用。<br /><br /> \- 或 -<br /><br /> 对于 Windows 8.x 应用商店 应用，使用 `–Scenario` 指定你要为其生成配置文件信息的包。 如果指定包显示名称或包系列名称而不是完整的包名称，则 Mpgo.exe 将选择与你提供的名称匹配的包（如果只有一个匹配项）。 如果多个包与指定的名称匹配，则 Mpgo.exe 将提示你选择一个包。<br /><br /> \- 或 -<br /><br /> 使用 `-Import` 指定应使用之前优化的程序集中的优化数据来优化 `-AssemblyList` 中的程序集。 directory 指定包含之前优化的文件的目录。 在 `–AssemblyList` 或 `–AssemblyListFile` 中指定的程序集是要使用导入文件中的数据进行优化的程序集的新版本。 通过使用早期版本的程序集中的优化数据，你可以优化较新版本的程序集，而无需重新运行该方案。  但是，如果导入的程序集和目标程序集包含明显不同的代码，则优化数据将无效。 在 `–AssemblyList` 或 `–AssemblyListFile` 中指定的程序集名称必须存在于 `–Import` directory 指定的目录中。 如果 directory 指定的路径包含空格，则应使用三组双引号将其引起来。<br /><br /> 你必须指定 `–Scenario` 或 `–Import`，但是不能同时指定这两个参数。|
|`-OutDir` \<*directory*>|用于放置优化过的程序集的目录。 如果某个程序集已经位于输出目录文件夹中，则将创建新副本并且向其名称追加索引号；例如：assemblyname-1.exe。 如果 directory 指定的路径包含空格，则使用双引号将其引起来。|
|`-AssemblyList` \<*assembly1 assembly2 ...*><br /><br /> \- 或 -<br /><br /> `-AssemblyListFile` \<*file*>|你希望收集与其有关的配置文件信息的程序集（包括 .exe 和 .dll 文件）的列表，各程序集之用空格隔开。 你可以指定 `C:\Dir\*.dll` 或 `*.dll` 来选择指定工作目录或当前工作目录中的所有程序集。 有关详细信息，请参阅备注部分。<br /><br /> \- 或 -<br /><br /> 一个文本文件，其中包含你希望收集与其有关的配置文件信息的程序集的列表，每个程序集在单独的行上列出。 如果程序集名称以连字符 (-) 开始，则使用程序集文件列表或重命名程序集。|
|`-AppID` \<*appId*>|指定包中的应用程序的 ID。 如果使用通配符 (\*)，则 Mpgo.exe 将尝试枚举包中的 AppID，如果失败，则将回退到 \<*package_family_name*>!App。 如果指定前缀为感叹号 (!) 的字符串，则 Mpgo.exe 会将包系列名称与提供的自变量连接。|
|`-Timeout` \<*seconds*>|允许 Windows 8.x 应用商店应用在此应用退出前运行的时间。|

|可选参数|描述|
|------------------------|-----------------|
|`-64bit`|为 64 位系统检测程序集。  你必须为 64 位程序集指定此参数，即使你的程序集声明为 64 位也是如此。|
|`-ExeConfig` \<*filename*>|指定你的方案用于提供版本和加载程序信息的配置文件。|
|`-f`|强制在二进制程序集中包含配置文件数据，即使它已签名也是如此。  如果已对程序集签名，则必须对程序集重新签名；否则，程序集将无法加载并运行。|
|`-Reset`|重置环境以确保中止的分析会话不会影响你的程序集，然后退出。 默认情况下将在分析会话前后重置环境。|
|`-Timeout` \<*time in seconds*>|指定分析持续时间（以秒为单位）。 使用稍高于你观察到的 GUI 应用程序启动时间的值。 在超时期间的最后，尽管应用程序将继续运行，但将记录配置文件数据。 如果未设置此选项，则分析将继续，直到应用程序关闭，此时将记录数据。|
|`-LeaveNativeImages`|指定在运行方案后不应删除检测到的本机映像。 此选项主要在你获取为方案运行指定的应用程序时使用。 它将防止在后续运行 Mpgo.exe 时重新创建本机映像。 完成应用程序的运行后，如果指定此选项，则缓存中可能有孤立的本机映像。 在这种情况下，请用同样的方案和程序集列表运行 Mpgo.exe，并使用 `–RemoveNativeImages` 参数来删除这些本机映像。|
|`-RemoveNativeImages`|通过指定了 `–LeaveNativeImages` 的运行进行清理。 如果指定 `-RemoveNativeImages`，则 Mpgo.exe 将忽略除 `-64bit` 和 `–AssemblyList` 之外的所有参数，然后在删除检测到的所有本机映像后退出。|

## <a name="remarks"></a>备注

 你可以在同一命令行上多次使用 `–AssemblyList` 和 `- AssemblyListFile`。

 如果在指定程序集时未指定完整路径名称，则 Mpgo.exe 将在当前目录中查找。 如果指定了不正确的路径，则 Mpgo.exe 将显示错误消息，但将继续为其他程序集生成数据。 如果指定了在培训方案期间未加载的程序集，则将不会为该程序集生成任何培训数据。

 如果列表中的程序集位于全局程序集缓存中，则将不会更新程序集来包含配置文件信息。  从全局程序集缓存中删除它以收集配置文件信息。

 仅建议大型托管应用程序使用 Ngen.exe 和 Mpgo.exe，因为预编译的本机映像的好处一般只能在它消除了大量运行时 JIT 编译工作的情况下才会显现出来。 在没有大量工作集的“Hello World”样式应用程序上运行 Mpgo.exe 将不会带来任何好处，并且 Mpgo.exe 甚至可能无法收集配置文件数据。

> [!NOTE]
> 对于 ASP.NET 应用程序和 Windows Communication Foundation (WCF) 服务，不推荐 Ngen.exe 和 Mpgo.exe。

## <a name="to-use-mpgoexe"></a>使用 Mpgo.exe

1. 使用安装了 Visual Studio Ultimate 2012 和你的应用程序的计算机。

2. 以管理员身份用必需的参数运行 Mpgo.exe。  有关示例命令，请参见下一节。

     优化的中间语言 (IL) 程序集是在由 `–OutDir` 参数指定的文件夹（在此示例中，这是 `C:\Optimized` 文件夹）中创建的。

3. 将用于 Ngen.exe 的 IL 程序集替换为包含 `–OutDir` 指定的目录中的配置文件信息的新 IL 程序集。

4. 应用程序安装程序（使用 Mpgo.exe 提供的映像）将安装优化的本机映像。

## <a name="suggested-workflow"></a>建议的工作流

1. 通过将 Mpgo.exe 与 `–Scenario` 参数结合使用来创建一组优化的 IL 程序集。

2. 将优化的 IL 程序集签入到源代码管理中。

3. 在生成过程中，使用 `–Import` 参数调用 Mpgo.exe 作为一个后期生成步骤来生成优化的 IL 映像以传递到 Ngen.exe。

 此过程确保所有程序集具有优化数据。 如果更频繁地签入更新的优化程序集（步骤 1 和 2），则性能数字在整个产品开发中将更一致。

## <a name="using-mpgoexe-from-visual-studio"></a>从 Visual Studio 使用 Mpgo.exe

 可以从 Visual Studio 运行 Mpgo.exe（请参见文章[如何：指定生成事件 (C#)](/visualstudio/ide/how-to-specify-build-events-csharp)），其具有下列限制：

- 你不能使用具有尾部反斜杠标记的引用路径，因为在默认情况下，Visual Studio 宏也使用尾部反斜杠标记。 （例如，`–OutDir "C:\Output Folder\"` 无效。）若要解决此限制，你可以转义尾部斜杠。 （例如，请改用 `-OutDir "$(OutDir)\"`。）

- 默认情况下，Mpgo.exe 不在 Visual Studio 生成路径中。 你必须添加 Visual Studio 的路径，或在 Mpgo 命令行上指定完整路径。 你可在 Visual Studio 中的后期生成事件中使用 `–Scenario` 或 `–Import` 参数。 但是，一般的处理方式是从 Visual Studio 开发人员命令提示符处使用一次 `–Scenario`，然后在每次生成后使用 `–Import` 更新优化的程序集，例如：`"C:\Program Files\Microsoft Visual Studio 11.0\Team Tools\Performance Tools\mpgo.exe" -import "$(OutDir)tmp" -assemblylist "$(TargetPath)" -outdir "$(OutDir)\"`。

<a name="samples"></a>

## <a name="examples"></a>示例

 以下来自 Visual Studio 开发人员命令提示处的 Mpgo.exe 命令将优化一个税务应用程序：

```console
mpgo –scenario "C:\MyApp\MyTax.exe /params par" –AssemblyList Mytax.dll MyTaxUtil2011.dll –OutDir C:\Optimized –TimeOut 15
```

 以下 Mpgo.exe 命令将优化一个声音应用程序：

```console
mpgo –scenario "C:\MyApp\wav2wma.exe –input song1.wav –output song1.wma" –AssemblyList transcode.dll –OutDir C:\Optimized –TimeOut 15
```

 以下 Mpgo.exe 命令将使用以前优化的程序集中的数据优化新版本的程序集：

```console
mpgo.exe -import "C:\Optimized" -assemblylist "C:\MyApp\MyTax.dll" "C:\MyApp\MyTaxUtil2011.dll" -outdir C:\ReOptimized
```

## <a name="see-also"></a>请参阅

- [Ngen.exe（本机映像生成器）](ngen-exe-native-image-generator.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
- [Improving Launch Performance for your Desktop Applications](https://devblogs.microsoft.com/dotnet/improving-launch-performance-for-your-desktop-applications/)（提高桌面应用程序的启动性能）
- [.NET Framework 4.5 中的性能改进概述](/archive/msdn-magazine/2012/april/clr-an-overview-of-performance-improvements-in-net-4-5)
