---
title: .NET Core 3.0 的新增功能
description: 了解 .NET Core 3.0 的新增功能。
dev_langs:
- csharp
author: adegeo
ms.author: adegeo
ms.date: 01/27/2020
ms.openlocfilehash: f401af3ec7f1fc729e6f3af005acbde2ad61f586
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875832"
---
# <a name="whats-new-in-net-core-30"></a>.NET Core 3.0 的新增功能

本文介绍了 .NET Core 3.0 中的新增功能。 最大的增强功能之一是对 Windows 桌面应用程序的支持（仅限 Windows）。 通过使用 .NET Core 3.0 SDK Windows 桌面组件，可移植 Windows 窗体和 Windows Presentation Foundation (WPF) 应用程序。 明确地说，只有在 Windows 上才支持和包含 Windows 桌面组件。 有关详细信息，请参阅本文后面的 [Windows 桌面](#windows-desktop)部分。

.NET Core 3.0 添加了对 C#8.0 的支持。 强烈建议使用 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本、[Visual Studio for Mac 8.3](/visualstudio/mac/install-preview) 或更高版本，或具有最新 C# 扩展的 [Visual Studio Code](https://code.visualstudio.com/)。

立即在 Windows、macOS 或 Linux 上[下载并开始使用 .NET Core 3.0](https://aka.ms/netcore3download)。

有关版本的详细信息，请参阅 [.NET Core 3.0 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/)。

Microsoft 认为 .NET Core RC1 可用于生产环境，且该软件完全受支持。 如果使用的是预览版本，则必须转换为 RTM 版本才能继续获得支持。

## <a name="language-improvements-c-80"></a>语言改进 C# 8.0

C# 8.0 也是该发布的一部分，包含[可为空引用类型](../../csharp/language-reference/builtin-types/nullable-reference-types.md)功能、异步流和更多模式。 有关 C# 8.0 功能的详细信息，请参阅 [C# 8.0 中的新增功能](../../csharp/whats-new/csharp-8.md)。

与 C# 8.0 语言功能相关的教程：

- [教程：使用可为空和不可为空引用类型更清晰地表达设计意图](../../csharp/whats-new/tutorials/nullable-reference-types.md)
- [教程：使用 C# 8.0 和 .NET Core 3.0 生成和使用异步流](../../csharp/whats-new/tutorials/generate-consume-asynchronous-stream.md)
- [教程：使用模式匹配来构建类型驱动和数据驱动的算法](../../csharp/tutorials/pattern-matching.md)

添加了语言增强功能，以支持下面详细说明的 API 功能：

- [范围和索引](#ranges-and-indices)
- [异步流](#async-streams)

## <a name="net-standard-21"></a>.NET Standard 2.1

.NET Core 3.0 已实现 .NET Standard 2.1。 但是，默认的 `dotnet new classlib` 模板还是会生成一个面向 .NET Standard 2.0 的项目。 若要面向 **.NET Standard 2.1**，请编辑项目文件并将 `TargetFramework` 属性更改为 `netstandard2.1`：

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.1</TargetFramework>
  </PropertyGroup>

</Project>
```

如果使用 Visual Studio，则需要 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)，这是因为 Visual Studio 2017 不支持 .NET Standard 2.1 或 .NET Core 3.0 。

## <a name="compiledeploy"></a>编译/部署

### <a name="default-executables"></a>默认可执行文件

.NET Core 现在默认生成[依赖于框架的可执行文件](../deploying/index.md#publish-framework-dependent)。 对于使用全局安装的 .NET Core 版本的应用程序而言，这是一种新行为。 以前，仅[独立部署](../deploying/index.md#publish-self-contained)会生成可执行文件。

在 `dotnet build` 或 `dotnet publish` 期间，将创建一个与你使用的 SDK 的环境和平台相匹配的可执行文件（即 appHost）。 和其他本机可执行文件一样，可以使用这些可执行文件执行相同操作，例如：

- 可以双击可执行文件。
- 可以直接从命令提示符启用应用程序，如 Windows 上的 `myapp.exe`，以及 Linux 和 macOS 上的 `./myapp`。

### <a name="macos-apphost-and-notarization"></a>macOS appHost 和公证

仅 macOS 

从已公证的适用于 macOS 的 .NET Core SDK 3.0 开始，默认已禁用用于生成默认可执行文件（即 appHost）的设置。 有关详细信息，请参阅 [macOS Catalina 公证以及对 .NET Core 下载和项目的影响](../install/macos-notarization-issues.md)。

启用 appHost 设置后，.NET Core 在生成或发布时将生成本机 Mach-O 可执行文件。 如果使用 `dotnet run` 命令从源代码中运行应用，或通过启动 Mach-O 可执行文件直接运行应用，则应用会在 appHost 的上下文中运行。

如果没有 appHost，用户就只能使用 `dotnet <filename.dll>` 命令启动[依赖框架](../deploying/index.md#publish-framework-dependent)的应用。 发布[独立](../deploying/index.md#publish-self-contained)应用时，始终会创建 appHost。

可以在项目级别配置 appHost，或通过 `-p:UseAppHost` 参数切换特定 `dotnet` 命令的 appHost：

- 项目文件

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- 命令行参数

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

有关 `UseAppHost` 设置的详细信息，请参阅 [Microsoft.NET.Sdk 的 MSBuild 属性](../project-sdk/msbuild-props.md#useapphost)。

### <a name="single-file-executables"></a>单文件可执行文件

`dotnet publish` 命令支持将应用打包为特定于平台的单文件可执行文件。 该可执行文件是自解压缩文件，包含运行应用所需的所有依赖项（包括本机依赖项）。 首次运行应用时，应用程序将根据应用名称和生成标识符自解压缩到一个目录中。 再次运行应用程序时，启动速度将变快。 除非使用了新版本，否则应用程序无需再次进行自解压缩。

若要发布单文件可执行文件，请使用 `dotnet publish` 命令在项目或命令行中设置 `PublishSingleFile`：

```xml
<PropertyGroup>
  <RuntimeIdentifier>win10-x64</RuntimeIdentifier>
  <PublishSingleFile>true</PublishSingleFile>
</PropertyGroup>
```

\- 或 -

```dotnetcli
dotnet publish -r win10-x64 -p:PublishSingleFile=true
```

有关单文件发布的详细信息，请参阅[单文件捆绑程序设计文档](https://github.com/dotnet/designs/blob/main/accepted/2020/single-file/design.md)。

### <a name="assembly-linking"></a>程序集链接

.NET core 3.0 SDK 随附了一种工具，可以通过分析 IL 并剪裁未使用的程序集来减小应用的大小。

自包含应用包括运行代码所需的所有内容，而无需在主计算机上安装 .NET。 但是，很多时候应用只需要一小部分框架即可运行，并且可以删除其他未使用的库。

.NET Core 现在包含一个设置，将使用 [IL 链接器](https://github.com/mono/linker)工具扫描应用的 IL。 此工具将检测哪些代码是必需的，然后剪裁未使用的库。 此工具可以显著减少某些应用的部署大小。

要启用此工具，请使用项目中的 `<PublishTrimmed>` 设置并发布自包含应用：

```xml
<PropertyGroup>
  <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

```dotnetcli
dotnet publish -r <rid> -c Release
```

例如，包含的基本“hello world”新控制台项目模板在发布时命中大小约为 70 MB。 通过使用 `<PublishTrimmed>`，其大小将减少到约 30 MB。

请务必考虑到使用反射或相关动态功能的应用程序或框架（包括 ASP.NET Core 和 WPF）通常会在剪裁时损坏。 发生此损坏是因为链接器不知道此动态行为，并且不能确定反射需要哪些框架类型。 可配置 IL 链接器工具以发现这种情况。

最重要的是，剪裁后务必对应用进行测试。

有关 IL 链接器工具的详细信息，请参阅[文档](../deploying/trim-self-contained.md)，或访问 [mono/linker]( https://github.com/mono/linker) 存储库。

### <a name="tiered-compilation"></a>分层编译

.NET Core 3.0 中默认启用了[分层编译](https://github.com/dotnet/runtime/blob/main/docs/design/features/tiered-compilation.md) (TC)。 此功能使运行时能够更适应地使用实时 (JIT) 编译器来实现更好的性能。

分层编译的主要优势是提供两种实现实时的方法，可在低质量快速层或高质量慢速层中编译。 质量是指方法的优化程度。 这有助于提高应用程序在从启动到稳定状态的各个执行阶段的性能。 禁用分层编译后，每种方法都以同一种方式进行编译，这种方式倾向于牺牲启动性能来保证稳定状态性能。

启用 TC 后，以下行为适用于应用启动时的方法编译：

- 如果方法具有预先编译的代码 ([ReadyToRun](#readytorun-images))，将使用预生成的代码。
- 否则，将实时编译该方法。 一般来说，这些方法是泛型而不是值类型。
  - 快速 JIT 可以更快地生成较低质量（优化程度较低）的代码。 在 .NET Core 3.0 中，默认为不包含循环的方法启用了快速 JIT，并且启动过程中首选快速 JIT。
  - 完全优化的 JIT 可生成更高质量（优化程度更高）的代码，但速度更慢。 对于不使用快速 JIT 的方法（例如，如果该方法具有 <xref:System.Runtime.CompilerServices.MethodImplOptions.AggressiveOptimization?displayProperty=nameWithType> 特性），则使用完全优化的 JIT。

对于频繁调用的方法，实时编译器最终会在后台创建完全优化的代码。 然后，优化后的代码将替换该方法的预编译代码。

通过快速 JIT 生成的代码可能会运行较慢、分配更多内存或使用更多堆栈空间。 如果出现问题，可以在项目文件中使用此 MSBuild 属性禁用快速 JIT：

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

若要完全禁用 TC，请在项目文件中使用此 MSBuild 属性：

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

> [!TIP]
> 如果在项目文件中更改这些设置，则可能需要执行干净的生成以反映新的设置（删除 `obj` 和 `bin` 目录并重新生成）。

有关在运行时配置编译的详细信息，请参阅[用于编译的运行时配置选项](../run-time-config/compilation.md)。

### <a name="readytorun-images"></a>ReadyToRun 映像

可以通过将应用程序集编译为 ReadyToRun (R2R) 格式来改进.NET Core 应用程序的启动时间。 R2R 是一种预先 (AOT) 编译形式。

R2R 二进制文件通过减少应用程序加载时实时 (JIT) 编译器需要执行的工作量来改进启动性能。 二进制文件包含与 JIT 将生成的内容类似的本机代码。 但是，R2R 二进制文件更大，因为它们包含中间语言 (IL) 代码（某些情况下仍需要此代码）和相同代码的本机版本。 仅当发布面向特定运行时环境 (RID)（如 Linux x64 或 Windows x64）的自包含应用时 R2R 才可用。

若要将项目编译为 ReadyToRun，请执行以下操作：

01. 向项目中添加 `<PublishReadyToRun>` 设置：

    ```xml
    <PropertyGroup>
      <PublishReadyToRun>true</PublishReadyToRun>
    </PropertyGroup>
    ```

01. 发布自包含应用。 例如，此命令将创建适用于 Windows 64 位版本的自包含应用：

    ```dotnetcli
    dotnet publish -c Release -r win-x64 --self-contained
    ```

#### <a name="cross-platformarchitecture-restrictions"></a>跨平台/体系结构限制

ReadyToRun 编译器当前不支持跨目标。 必须在给定的目标上编译。 例如，如果想要 Windows x64 R2R 映像，需要在该环境中运行发布命令。

跨目标的例外情况：

- 可以使用 Windows x64 编译 Windows ARM32、ARM64 和 x86 映像。
- 可以使用 Windows x86 编译 Windows ARM32 映像。
- 可以使用 Linux x64 编译 Linux ARM32 和 ARM64 映像。

有关详细信息，请参阅[准备好运行](../deploying/ready-to-run.md)。

## <a name="runtimesdk"></a>运行时/SDK

### <a name="major-version-runtime-roll-forward"></a>主要版本运行时前滚

.NET Core 3.0 引入了一项选择加入功能，该功能允许应用前滚到 .NET Core 最新的主要版本。 此外，还添加了一项新设置来控制如何将前滚应用于应用。 这可以通过以下方式配置：

- 项目文件属性：`RollForward`
- 运行时配置文件属性：`rollForward`
- 环境变量：`DOTNET_ROLL_FORWARD`
- 命令行参数：`--roll-forward`

必须指定以下值之一。 如果省略该设置，则默认值为“Minor”。

- **LatestPatch**\
前滚到最高补丁版本。 这会禁用次要版本前滚。
- **Minor**\
如果缺少所请求的次要版本，则前滚到最低的较高次要版本。 如果存在所请求的次要版本，则使用 **LatestPatch** 策略。
- **Major**\
如果缺少所请求的主要版本，则前滚到最低的较高主要版本和最低的次要版本。 如果存在所请求的主要版本，则使用 **Minor** 策略。
- **LatestMinor**\
即使存在所请求的次要版本，仍前滚到最高次要版本。 适用于组件托管方案。
- **LatestMajor**\
即使存在所请求的主要版本，仍前滚到最高主要版本和最高次要版本。 适用于组件托管方案。
- **Disable**\
不前滚。 仅绑定到指定的版本。 建议不要将此策略用于一般用途，因为它会禁用前滚到最新补丁的功能。 该值仅建议用于测试。

除“Disable”设置外，所有设置都将使用可用的最高补丁版本。

默认情况下，如果请求的版本（`.runtimeconfig.json` 中为应用程序指定的版本）为发布版本，则前滚时只考虑发布版本。 将忽略所有预发布版本。 如果没有匹配的发布版本，则会考虑使用预发布版本。 可以通过设置 `DOTNET_ROLL_FORWARD_TO_PRERELEASE=1` 来更改此行为，在这种情况下，将始终考虑所有版本。

### <a name="build-copies-dependencies"></a>生成会复制依赖项

`dotnet build` 命令现在将应用程序的 NuGet 依赖项从 NuGet 缓存复制到生成输出文件夹。 此前，依赖项仅作为 `dotnet publish` 的一部分复制。

有些操作，比如链接和 razor 页面发布仍需要发布。

### <a name="local-tools"></a>本地工具

.NET Core 3.0 引入了本地工具。 本地工具类似于[全局工具](../tools/global-tools.md)，但与磁盘上的特定位置相关联。 本地工具在全局范围内不可用，并作为 NuGet 包进行分发。

> [!WARNING]
> 如果尝试使用过 .NET Core 3.0 预览版 1 中的本地工具，例如运行 `dotnet tool restore` 或 `dotnet tool install`，请删除本地工具缓存文件夹。 否则，本地工具将无法在任何较新的版本上运行。 此文件夹位于：
>
> 在 macOS、Linux 上：`rm -r $HOME/.dotnet/toolResolverCache`
>
> 在 Windows 上：`rmdir /s %USERPROFILE%\.dotnet\toolResolverCache`

本地工具依赖于当前目录中名为 `dotnet-tools.json` 的清单文件。 此清单文件定义在该文件夹和以下文件夹中可用的工具。 你可以随代码一起分发清单文件，以确保使用代码的任何人都可以还原和使用相同的工具。

对于全局工具和本地工具，需要一个兼容的运行时版本。 目前，NuGet.org 上的许多工具都面向 .NET Core Runtime 2.1。 若要在全局范围或本地安装这些工具，仍需要安装 [NET Core 2.1 运行时](https://dotnet.microsoft.com/download/dotnet/2.1)。

### <a name="new-globaljson-options"></a>新 global.json 选项

global.json 文件包含新选项，当你尝试定义所使用的 .NET Core SDK 版本时，这些选项可提供更大的灵活性。 新选项包括：

- `allowPrerelease`：指示在选择要使用的 SDK 版本时，SDK 解析程序是否应考虑预发布版本。
- `rollForward`：指示选择 SDK 版本时要使用的前滚策略，可作为特定 SDK 版本缺失时的回退，或者作为使用更高版本的指令。

有关这些更改的详细信息（包括默认值、支持的值和新的匹配规则），请参阅 [global.json 概述](../tools/global-json.md)。

### <a name="smaller-garbage-collection-heap-sizes"></a>垃圾回收堆大小减小

垃圾回收器的默认堆大小已减小，以使 .NET Core 使用更少的内存。 此更改更符合具有现代处理器缓存大小的第 0 代分配预算。

### <a name="garbage-collection-large-page-support"></a>垃圾回收大型页面支持

大型页面（也称为 Linux 上的巨型页面）是一项功能，其中操作系统能够建立大于本机页面大小（通常为 4K）的内存区域，以提高请求这些大型页面的应用程序的性能。

现在可以使用 **GCLargePages** 设置将垃圾回收器配置为一项选择加入功能，以选择在 Windows 上分配大型页面。

## <a name="windows-desktop--com"></a>Windows 桌面和 COM

### <a name="net-core-sdk-windows-installer"></a>.NET Core SDK Windows Installer

用于 Windows 的 MSI 安装程序已从 .NET Core 3.0 开始更改。 SDK 安装程序现在将对 SDK 功能区段版本进行就地升级。 功能区段在版本号的 *补丁* 部分中的 *百数* 组中定义。 例如，**3.0.101** 和 **3.0.201** 是两个不同功能区段中的版本，而 **3.0.101** 和 **3.0.199** 则属于同一个功能区段。    并且，当安装 .NET Core SDK **3.0.101** 时，将从计算机中删除 .NET Core SDK **3.0.100** （如果存在）。  当 .NET Core SDK **3.0.200** 安装在同一台计算机上时，不会删除 .NET Core SDK **3.0.101** 。 

有关版本控制的详细信息，请参阅 [.NET Core 的版本控制方式概述](../versions/index.md)。

### <a name="windows-desktop"></a>Windows 桌面

.NET Core 3.0 支持使用 Windows Presentation Foundation (WPF) 和 Windows 窗体的 Windows 桌面应用程序。 这些框架还支持通过 [XAML 岛](/windows/uwp/xaml-platform/xaml-host-controls)从 Windows UI XAML 库 (WinUI) 使用新式控件和 Fluent 样式。

Windows 桌面部件是 Windows .NET Core 3.0 SDK 的一部分。

可以使用以下 `dotnet` 命令创建新的 WPF 或 Windows 窗体应用：

```dotnetcli
dotnet new wpf
dotnet new winforms
```

Visual Studio 2019 添加了适用于 .NET Core 3.0 Windows 窗体和 WPF 的“新建项目”模板。

有关如何移植现有 .NET Framework 应用程序的详细信息，请参阅[移植 WPF 项目](/dotnet/desktop/wpf/migration/convert-project-from-net-framework)和[移植 Windows 窗体项目](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)。

#### <a name="winforms-high-dpi"></a>WinForms 高 DPI

.NET Core Windows 窗体应用程序可以使用 <xref:System.Windows.Forms.Application.SetHighDpiMode(System.Windows.Forms.HighDpiMode)?displayProperty=nameWithType> 设置高 DPI 模式。 `SetHighDpiMode` 方法可设置相应的高 DPI 模式，除非该设置已通过其他方式（例如使用 `App.Manifest` 或在 `Application.Run` 前面使用 P/Invoke）进行设置。

由 <xref:System.Windows.Forms.HighDpiMode?displayProperty=nameWithType> 枚举表示的可能的 `highDpiMode` 值包括：

- `DpiUnaware`
- `SystemAware`
- `PerMonitor`
- `PerMonitorV2`
- `DpiUnawareGdiScaled`

有关高 DPI 模式的详细信息，请参阅[在 Windows 上开发高 DPI 桌面应用程序](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)。

### <a name="create-com-components"></a>创建 COM 组件

在 Windows 上，现在可以创建可调用 COM 的托管组件。 在将 .NET Core 与 COM 加载项模型结合使用，以及使用 .NET Framework 提供奇偶校验时，此功能至关重要。

与将 *mscoree.dll* 用作 COM 服务器的 .NET Framework 不同，.NET Core 将在生成 COM 组件时向 *bin* 目录添加本机启动程序 dll。

有关如何创建 COM 组件并使用它的示例，请参阅 [COM 演示](https://github.com/dotnet/samples/tree/main/core/extensions/COMServerDemo)。

### <a name="windows-native-interop"></a>Windows 本机互操作

Windows 提供丰富的本机 API，包括平面 C API、COM 和 WinRT 的形式。 .NET Core 支持 **P/Invoke**, .NET Core 3.0 则增加了 **CoCreate COM API** 和 **Activate WinRT API** 的功能。 有关代码示例，请参阅 [Excel 演示](https://github.com/dotnet/samples/tree/main/core/extensions/ExcelDemo)。

### <a name="msix-deployment"></a>MSIX 部署

[MSIX](/windows/msix/) 是新的 Windows 应用程序包格式。 可以使用它将 .NET Core 3.0 桌面应用程序部署到 Windows 10。

使用 Visual Studio 2019 中的 [Windows 应用打包项目](/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)，可以创建包含[独立式](../deploying/index.md#publish-self-contained) .NET Core 应用的 MSIX 包。

.NET Core 项目文件必须在 `<RuntimeIdentifiers>` 属性中指定支持的运行时：

```xml
<RuntimeIdentifiers>win-x86;win-x64</RuntimeIdentifiers>
```

## <a name="linux-improvements"></a>Linux 改进

### <a name="serialport-for-linux"></a>适用于 Linux 的 SerialPort

.NET Core 3.0 提供对 Linux 上 <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType> 的基本支持。

以前，.NET Core 只支持在 Windows 上使用 `SerialPort`。

有关对 Linux 上串行端口有限支持的详细信息，请参阅 [GitHub 问题 #33146](https://github.com/dotnet/corefx/issues/33146)。

### <a name="docker-and-cgroup-memory-limits"></a>Docker 和 cgroup 内存限制

在 Linux 上使用 Docker 运行 .NET Core 3.0 时，可更好地应对 cgroup 内存限制。 运行具有内存限制的 Docker 容器（例如使用 `docker run -m`）会更改 .NET Core 的行为方式。

- 默认垃圾回收器 (GC) 堆大小：最大为 20 MB 或容器内存限制的 75%。
- 可以将显式大小设置为绝对数或 cgroup 限制的百分比。
- 每个 GC 堆的最小保留段大小为 16 MB。 此大小可减少在计算机上创建的堆数量。

### <a name="gpio-support-for-raspberry-pi"></a>对 Raspberry Pi 的 GPIO 支持

已向 NuGet 发布了两个可用于 GPIO 编程的包：

- [System.Device.Gpio](https://www.nuget.org/packages/System.Device.Gpio)
- [Iot.Device.Bindings](https://www.nuget.org/packages/Iot.Device.Bindings)

GPIO 包包括用于 *GPIO*、*SPI*、*I2C* 和 *PWM* 设备的 API。 IoT 绑定包包括设备绑定。 有关详细信息，请参阅[设备 GitHub 存储库](https://github.com/dotnet/iot/blob/main/src/devices/)。

### <a name="arm64-linux-support"></a>ARM64 Linux 支持

.NET Core 3.0 增加了对 ARM64 for Linux 的支持。 ARM64 的主要用例是当前的 IoT 场景。 有关详细信息，请参阅 [.NET Core ARM64 状态](https://github.com/dotnet/announcements/issues/82)。

[ARM64 上适用于 .NET Core 的 Docker 映像](https://hub.docker.com/r/microsoft/dotnet/)可用于 Alpine、Debian 和 Ubuntu。

> [!NOTE]
> **ARM64** 尚未提供 Windows 支持。

## <a name="security"></a>安全性

### <a name="tls-13--openssl-111-on-linux"></a>Linux 上的 TLS 1.3 和 OpenSSL 1.1.1

.NET Core 现在可以在给定环境中使用 [OpenSSL 1.1.1 中的 TLS 1.3 支持](https://www.openssl.org/blog/blog/2018/09/11/release111/)。 使用 TLS 1.3：

- 通过减少客户端和服务器之间所需的往返次数，提高了连接时间。
- 由于删除了各种过时和不安全的加密算法，提高了安全性。

.NET Core 3.0 在 Linux 系统上使用 **OpenSSL 1.1.1**、**OpenSSL 1.1.0** 或 **OpenSSL 1.0.2**（如果可用）。 当 **OpenSSL 1.1.1** 可用时，<xref:System.Net.Security.SslStream?displayProperty=nameWithType> 和 <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> 类型都将使用 **TLS 1.3**（假定客户端和服务器都支持 **TLS 1.3**）。

> [!IMPORTANT]
> Windows 和 macOS 尚不支持 TLS 1.3。

下面的 C# 8.0 示例演示在 Ubuntu 18.10 上 .NET Core 3.0 如何连接到 <https://www.cloudflare.com>：

[!code-csharp[TLSExample](./snippets/dotnet-core-3-0/csharp/TLS.cs#TLS)]

### <a name="cryptography-ciphers"></a>加密密码

.NET Core 3.0 增加了对 AES-GCM 和 AES-CCM 密码的支持（分别使用 <xref:System.Security.Cryptography.AesGcm?displayProperty=nameWithType> 和 <xref:System.Security.Cryptography.AesCcm?displayProperty=nameWithType> 实现） 。 这些算法都是[用于关联数据的认证加密 (AEAD) 算法](https://en.wikipedia.org/wiki/Authenticated_encryption)。

下面的代码演示了如何使用 `AesGcm` 密码加密和解密随机数据。

[!code-csharp[AesGcm](./snippets/dotnet-core-3-0/csharp/Cipher.cs#AesGcm)]

### <a name="cryptographic-key-importexport"></a>加密密钥导入/导出

.NET Core 3.0 支持从标准格式导入和导出非对称公钥和私钥。 你不需要使用 X.509 证书。

所有密钥类型（例如 *RSA*、*DSA*、*ECDsa* 和 *ECDiffieHellman*）都支持以下格式：

- **公钥**
  - X.509 SubjectPublicKeyInfo

- **私钥**
  - PKCS#8 PrivateKeyInfo
  - PKCS#8 EncryptedPrivateKeyInfo

RSA 密钥还支持：

- **公钥**
  - PKCS#1 RSAPublicKey

- **私钥**
  - PKCS#1 RSAPrivateKey

导出方法生成 DER 编码的二进制数据，导入方法也是如此。 如果密钥以文本友好的 PEM 格式存储，调用方需要在调用导入方法之前对内容进行 base64 解码。

[!code-csharp[RSA](./snippets/dotnet-core-3-0/csharp/RSA.cs#Rsa)]

可以使用 <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo?displayProperty=nameWithType> 检查 **PKCS#8** 文件，使用 <xref:System.Security.Cryptography.Pkcs.Pkcs12Info?displayProperty=nameWithType> 检查 **PFX/PKCS#12** 文件。 可以使用 <xref:System.Security.Cryptography.Pkcs.Pkcs12Builder?displayProperty=nameWithType> 操作 **PFX/PKCS#12** 文件。

## <a name="net-core-30-api-changes"></a>.NET Core 3.0 API 改动

### <a name="ranges-and-indices"></a>范围和索引

新 <xref:System.Index?displayProperty=nameWithType> 类型可用于编制索引。 可从 `int` 创建一个从开头开始计数的索引，也可使用前缀 `^` 运算符 (C#) 创建一个从末尾开始计数的索引：

```csharp
Index i1 = 3;  // number 3 from beginning
Index i2 = ^4; // number 4 from end
int[] a = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
Console.WriteLine($"{a[i1]}, {a[i2]}"); // "3, 6"
```

此外，还有 <xref:System.Range?displayProperty=nameWithType> 类型，它包含两个 `Index` 值，一个用于开头一个用于结尾，可以使用 `x..y` 范围表达式 (C#) 进行编写。 然后可以使用 `Range` 编制索引，以便生成一个切片：

```csharp
var slice = a[i1..i2]; // { 3, 4, 5 }
```

有关详细信息，请参阅[范围和索引教程](../../csharp/whats-new/tutorials/ranges-indexes.md)。

### <a name="async-streams"></a>异步流

<xref:System.Collections.Generic.IAsyncEnumerable%601> 类型是 <xref:System.Collections.Generic.IEnumerable%601> 的新异步版本。 通过该语言，可通过 `IAsyncEnumerable<T>` 执行 `await foreach` 操作来使用其元素，并对其使用 `yield return` 操作来生成元素。

下面的示例演示如何生成和使用异步流。 `foreach` 语句为异步语句，它本身使用 `yield return` 为调用方生成异步流。 此模式（使用 `yield return`）是生成异步流的建议模型。

```csharp
async IAsyncEnumerable<int> GetBigResultsAsync()
{
    await foreach (var result in GetResultsAsync())
    {
        if (result > 20) yield return result;
    }
}
```

除了能够 `await foreach`，还可以创建异步迭代器，例如，一个返回 `IAsyncEnumerable/IAsyncEnumerator` 的迭代器，可以在其中进行 `await` 和 `yield` 操作。 对于需要处理的对象，可以使用各种 BCL 类型（如 `Stream` 和 `Timer`）实现的 `IAsyncDisposable`。

有关详细信息，请参阅[异步流教程](../../csharp/whats-new/tutorials/generate-consume-asynchronous-stream.md)。

### <a name="ieee-floating-point"></a>IEEE 浮点

正在更新浮点 API，以符合 [IEEE 754-2008 修订](https://en.wikipedia.org/wiki/IEEE_754-2008_revision)。 这些更改旨在公开所有 **必需** 操作并确保这些操作在行为上符合 IEEE 规范。有关浮点改进的详细信息，请参阅 [.NET Core 3.0 中的浮点分析和格式化改进](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/)博客文章。

分析和格式化修复包括：

- 正确分析并舍入任何输入长度。
- 正确分析并格式化负零。
- 通过执行不区分大小写的检查并允许在前面使用可选的 `+`（如果适用），正确分析 `Infinity` 和 `NaN`。

新的 <xref:System.Math?displayProperty=nameWithType> API 包括：

- <xref:System.Math.BitIncrement(System.Double)> 和 <xref:System.Math.BitDecrement(System.Double)>\
相当于 `nextUp` 和 `nextDown` IEEE 运算。 它们将返回最小的浮点数，该数字大于或小于输入值（分别）。 例如，`Math.BitIncrement(0.0)` 将返回 `double.Epsilon`。

- <xref:System.Math.MaxMagnitude(System.Double,System.Double)> 和 <xref:System.Math.MinMagnitude(System.Double,System.Double)>\
相当于 `maxNumMag` 和 `minNumMag` IEEE 运算，它们将（分别）返回大于或小于两个输入的量值的值。 例如，`Math.MaxMagnitude(2.0, -3.0)` 将返回 `-3.0`。

- <xref:System.Math.ILogB(System.Double)>\
相当于返回整数值的 `logB` IEEE 运算，它将返回输入参数的整数对数（以 2 为底）。 此方法实际上与 `floor(log2(x))` 相同，但完成后出现最小舍入错误。

- <xref:System.Math.ScaleB(System.Double,System.Int32)>\
相当于采用整数值的 `scaleB` IEEE 运算，它实际返回 `x * pow(2, n)`，但完成后出现最小舍入错误。

- <xref:System.Math.Log2(System.Double)>\
相当于返回（以 2 为底）对数的 `log2` IEEE 运算。 它会最小化舍入错误。

- <xref:System.Math.FusedMultiplyAdd(System.Double,System.Double,System.Double)>\
相当于执行乘法加法混合的 `fma` IEEE 运算。 也就是说，它以单个运算的形式执行 `(x * y) + z`，从而最小化舍入错误。 例如，`FusedMultiplyAdd(1e308, 2.0, -1e308)` 返回 `1e308`。 常规 `(1e308 * 2.0) - 1e308` 返回 `double.PositiveInfinity`。

- <xref:System.Math.CopySign(System.Double,System.Double)>\
相当于 `copySign` IEEE 运算，它返回 `x` 的值但带有符号 `y`。

### <a name="net-platform-dependent-intrinsics"></a>.NET 平台相关内部函数

已添加 API，允许访问某些性能导向的 CPU 指令，例如 SIMD 或位操作指令集 。 这些指令有助于在某些情况下实现显著的性能改进，例如高效地并行处理数据。

在适当的情况下，.NET 库已开始使用这些指令来改进性能。

有关详细信息，请参阅 [.NET Platform Dependent Intrinsics](https://github.com/dotnet/designs/blob/main/accepted/2018/platform-intrinsics.md)（.NET 平台相关内部函数）。

### <a name="improved-net-core-version-apis"></a>改进的 .NET Core 版本 API

从 .NET Core 3.0 开始，.NET Core 提供的版本 API 现在可以返回你预期的信息。 例如：

```csharp
System.Console.WriteLine($"Environment.Version: {System.Environment.Version}");

// Old result
//   Environment.Version: 4.0.30319.42000
//
// New result
//   Environment.Version: 3.0.0
```

```csharp
System.Console.WriteLine($"RuntimeInformation.FrameworkDescription: {System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription}");

// Old result
//   RuntimeInformation.FrameworkDescription: .NET Core 4.6.27415.71
//
// New result (notice the value includes any preview release information)
//   RuntimeInformation.FrameworkDescription: .NET Core 3.0.0-preview4-27615-11
```

> [!WARNING]
> 重大变更。 这在技术上是一个中断性变更，因为版本控制方案已发生变化。

### <a name="fast-built-in-json-support"></a>内置的快速 JSON 支持

.NET 用户在很大程度上依赖于 [Newtonsoft.Json](https://www.newtonsoft.com/json) 和其他常用的 JSON 库，它们仍是很好的选择。 `Newtonsoft.Json` 使用 .NET 字符串作为其基本数据类型，它实际上是 UTF-16。

新的内置 JSON 支持具有高性能、低分配的特点，并且可用于 UTF-8 编码的 JSON 文本。 有关 <xref:System.Text.Json> 命名空间和类型的详细信息，请参阅以下文章：

* [.NET 中的 JSON 序列化 - 概述](../../standard/serialization/system-text-json-overview.md)
* [如何在 .NET 中对 JSON 数据进行序列化和反序列化](../../standard/serialization/system-text-json-how-to.md)。
* [如何从 Newtonsoft.Json 迁移到 System.Text.Json](../../standard/serialization/system-text-json-migrate-from-newtonsoft-how-to.md)

### <a name="http2-support"></a>HTTP/2 支持

<xref:System.Net.Http.HttpClient?displayProperty=nameWithType> 类型支持 HTTP/2 协议。 如果启用 HTTP/2，则将通过 TLS/ALPN 协商 HTTP 协议版本，并在服务器选择使用 HTTP/2 时使用。

默认协议将保留 HTTP/1.1，但可以在两种不同方法中启用 HTTP/2。 首先，可以将 HTTP 请求消息设置为使用 HTTP/2：

[!code-csharp[Http2Request](./snippets/dotnet-core-3-0/csharp/http.cs#Request)]

其次，可以更改 <xref:System.Net.Http.HttpClient> 以默认使用 HTTP/2：

[!code-csharp[Http2Client](./snippets/dotnet-core-3-0/csharp/http.cs#Client)]

很多时候，在你开发应用程序时要使用未加密的连接。 如果你知道目标终结点将使用 HTTP/2，你可以为 HTTP/2 打开未加密的连接。 可以通过将 `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT` 环境变量设置为 `1` 或通过在应用上下文中启用它来将其打开：

[!code-csharp[Http2Context](./snippets/dotnet-core-3-0/csharp/http.cs#AppContext)]

## <a name="next-steps"></a>后续步骤

- [查看 .NET Core 2.2 和 3.0 之间的重大变更。](../compatibility/3.0.md)
- [查看用于 Windows 窗体应用的 .NET Core 3.0 中的中断性变更。](../compatibility/winforms.md#net-core-30)
