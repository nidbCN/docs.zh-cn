---
title: 应用程序发布
description: 了解发布 .NET 应用程序的方式。 .NET 可以发布特定于平台或跨平台的应用。 你可以将应用发布为独立应用或依赖于框架的应用。 每个模式都会影响用户运行应用的方式。
ms.date: 02/05/2021
ms.openlocfilehash: ca0fa4ff5e17a3fa4c636604ae81b63ffbbc99f6
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874285"
---
# <a name="net-application-publishing-overview"></a>.NET 应用程序发布概述

可在两种模式下发布使用 .NET 创建的应用程序，模式会影响用户运行应用的方式。

将应用作为独立应用，生成的应用程序将包含 .NET 运行时和库，以及该应用程序及其依赖项。 应用程序的用户可以在未安装 .NET 运行时的计算机上运行该应用程序。

如果将应用发布为依赖于框架的应用，生成的应用程序将仅包含该应用程序本身及其依赖项。 应用程序的用户必须单独安装 .NET 运行时。

默认情况下，这两种发布模式都会生成特定于平台的可执行文件。 不使用可执行文件也可以创建依赖于框架的应用程序，这些应用程序是跨平台的。

生成可执行文件时，可以使用运行时标识符 (RID) 指定目标平台。 有关 RID 的详细信息，请参阅 [.NET RID 目录](../rid-catalog.md)。

下表概述了每个 SDK 版本用于将应用发布为依赖于框架的应用或独立应用的命令：

| 类型                                                                                     | SDK 2.1 | SDK 3.1 | SDK 5.0 | 命令 |
| ---------------------------------------------------------------------------------------  | ------- | ------- | ------- | ------- |
| 适用于当前平台的[依赖于框架的可执行文件](#publish-framework-dependent)。 |         | ✔️      | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |
| 适用于特定平台的[依赖于框架的可执行文件](#publish-framework-dependent)。  |         | ✔️      | ✔️      | [`dotnet publish -r <RID> --self-contained false`](../tools/dotnet-publish.md) |
| [依赖于框架的跨平台二进制文件](#publish-framework-dependent)。               | ✔️      | ✔️      | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |
| [独立可执行文件](#publish-self-contained)。                                    | ✔️      | ✔️      | ✔️      | [`dotnet publish -r <RID>`](../tools/dotnet-publish.md) |

有关详细信息，请参阅 [.NET dotnet publish 命令](../tools/dotnet-publish.md)。

## <a name="produce-an-executable"></a>生成可执行文件

可执行文件不是跨平台的。 它们特定于操作系统和 CPU 体系结构。 发布应用并创建可执行文件时，可以将应用发布为[独立应用](#publish-self-contained)或[依赖于框架的应用](#publish-framework-dependent)。 将应用发布为独立应用，会在应用中包含 .NET 运行时，该应用的用户无需在运行应用前安装 .NET。 如果将应用发布为依赖于框架的应用，则该应用不包含 .NET 运行时和库，而仅包含该应用和第三方依赖项。

以下命令可生成可执行文件：

| 类型                                                                                     | SDK 2.1 | SDK 3.1 | SDK 5.0 | 命令 |
| ---------------------------------------------------------------------------------------- | ------- | ------- | ------- | ------- |
| 适用于当前平台的[依赖于框架的可执行文件](#publish-framework-dependent)。 |         | ✔️      | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |
| 适用于特定平台的[依赖于框架的可执行文件](#publish-framework-dependent)。  |         | ✔️      | ✔️      | [`dotnet publish -r <RID> --self-contained false`](../tools/dotnet-publish.md) |
| [独立可执行文件](#publish-self-contained)。                                    | ✔️      | ✔️      | ✔️      | [`dotnet publish -r <RID>`](../tools/dotnet-publish.md) |

## <a name="produce-a-cross-platform-binary"></a>生成跨平台二进制文件

以 dll 文件的形式将应用发布为[依赖于框架的应用](#publish-framework-dependent)时，将创建跨平台二进制文件。 dll 文件将与项目同名。 例如，如果有名为 word_reader 的应用，则会创建名为 word_reader.dll 的文件。 以这种方式发布的应用可通过 `dotnet <filename.dll>` 命令运行，并且可在任意平台上运行。

只要安装了目标 .NET 运行时，就可以在任何操作系统上运行跨平台二进制文件。 如果未安装目标 .NET 运行时，如果将应用配置为前滚，则它可以使用较新的运行时运行。 有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。

以下命令可生成跨平台二进制文件：

| 类型                                                                                 | SDK 2.1 | SDK 3.x | SDK 5.0 | 命令 |
| -----------------------------------------------------------------------------------  | ------- | ------- | ------- | ------- |
| [依赖于框架的跨平台二进制文件](#publish-framework-dependent)。           | ✔️      | ✔️      | ✔️      | [`dotnet publish`](../tools/dotnet-publish.md) |

## <a name="publish-framework-dependent"></a>发布依赖于框架的应用

如果将应用发布为依赖于框架的应用，则该应用是跨平台的，且不包含 .NET 运行时。 应用的用户需要安装 .NET 运行时。

如果将应用发布为依赖于框架的应用，会以 dll 文件的形式生成一个[跨平台二进制文件](#produce-a-cross-platform-binary)，还会生成面向当前平台的[特定于平台的可执行文件](#produce-an-executable)。 dll 是跨平台的，而可执行文件不是。 例如，如果发布名为 word_reader 的应用且面向 Windows，则将创建 word_reader.exe 和 word_reader.dll。 面向 Linux 或 macOS 时，将创建 word_reader 可执行文件和 word_reader.dll。 有关 RID 的详细信息，请参阅 [.NET RID 目录](../rid-catalog.md)。

> [!IMPORTANT]
> 当你发布依赖于框架的应用时，.NET SDK 2.1 不会生成特定于平台的可执行文件。

可以通过 `dotnet <filename.dll>` 命令运行应用的跨平台二进制文件，并且它可以在任何平台上运行。 如果应用使用具有特定于平台的实现的 NuGet 包，则所有平台的依赖项都将连同应用一起复制到发布文件夹。

可以通过将 `-r <RID> --self-contained false` 参数传递到 [`dotnet publish`](../tools/dotnet-publish.md) 命令，为特定平台创建可执行文件。 省略 `-r` 参数时，将为当前平台创建可执行文件。 具有特定于目标平台的依赖项的任何 NuGet 包都将复制到发布文件夹。 如果不需要特定于平台的可执行文件，则可以在项目文件中指定 `<UseAppHost>False</UseAppHost>`。 有关详细信息，请参阅[适用于 .NET SDK 项目的 MSBuild 参考](../project-sdk/msbuild-props.md#useapphost)。

### <a name="advantages"></a>优点

- 小型部署  \
仅分发应用及其依赖项。 .NET 运行时和库由用户安装，所有应用共享运行时。

- 跨平台  \
应用和任何基于 .NET 的库都可在不同的操作系统上运行。 无需为应用定义目标平台。 有关 .NET 文件格式的详细信息，请参阅 [.NET 程序集文件格式](../../standard/assembly/file-format.md)。

- 使用最新修补运行时  \
应用会使用目标系统上安装的最新运行时（在 .NET 的目标大小系列中）。 这意味着应用将自动使用 .NET 运行时的最新修补版本。 可以重写这一默认行为。 有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。

### <a name="disadvantages"></a>缺点

- **要求预先安装运行时**\
仅当主机系统上已安装应用设为目标的 .NET 版本时，应用才能运行。 可以为应用配置前滚行为，要求使用特定版本的 .NET 或允许使用较新版本的 .NET。 有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。

- **.NET 可更改**\
可以在运行应用的计算机上更新 .NET 运行时和库。 在极少数情况下，如果使用 .NET 库（大多数应用都会使用），这可能会更改应用的行为。 可以配置应用如何使用较新版本的 .NET。 有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。

以下缺陷仅限于.NET Core 2.1 SDK。

- 使用  `dotnet` 命令启动应用\
用户必须运行 `dotnet <filename.dll>` 命令来启动你的应用。 如果将应用发布为依赖于框架的应用，则 .NET Core 2.1 SDK 不会生成特定于平台的可执行文件。

### <a name="examples"></a>示例

发布依赖于框架的跨平台应用。 与 *dll* 文件一起创建面向当前平台的可执行文件。

```dotnet
dotnet publish
```

发布依赖于框架的跨平台应用。 与 *dll* 文件一起创建 Linux 64 位可执行文件。 此命令对于 .NET Core SDK 2.1 无效。

```dotnet
dotnet publish -r linux-x64 --self-contained false
```

## <a name="publish-self-contained"></a>发布独立应用

将应用发布为独立应用，将生成特定于平台的可执行文件。 输出发布文件夹包含应用的所有组件，包括 .NET 库和目标运行时。 应用独立于其他 .NET 应用，且不使用本地安装的共享运行时。 应用的用户无需下载和安装 .NET。

针对指定的目标平台生成可执行二进制文件。 例如，如果你有一个名为 word_reader 的应用，并发布适用于 Windows 的独立可执行文件，则将创建 word_reader.exe 文件。 针对 Linux 或 macOS 发布时，将创建 word_reader  文件。 用 [`dotnet publish`](../tools/dotnet-publish.md) 命令的 `-r <RID>` 参数指定目标平台和体系结构。 有关 RID 的详细信息，请参阅 [.NET RID 目录](../rid-catalog.md)。

如果应用具有特定于平台的依赖项（例如包含特定于平台的依赖项的 NuGet 包），这些依赖项将与应用一起复制到发布文件夹。

### <a name="advantages"></a>优点

- **控制 .NET 版本**\
你可以控制随应用部署的 .NET 版本。

- 特定于平台的定向  \
由于你必须为每个平台发布应用，因此你知道应用可运行于哪些平台。 如果 .NET 引入了新平台，则用户无法在该新平台上运行你的应用，直到你发布面向该平台的版本。 在用户在新平台上运行你的应用之前，你可以测试应用以排除兼容性问题。

### <a name="disadvantages"></a>缺点

- 大型部署  \
由于你的应用包含 .NET 运行时和所有应用依赖项，因此下载大小和所需硬盘空间比[依赖于框架的应用](#publish-framework-dependent)的版本要大。

  > [!TIP]
  > 可以通过使用 .NET [全球化固定模式](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md)在 Linux 系统上减少大约 28 MB 的部署大小。 这会强制应用像处理[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType)一样处理所有区域性。

  > [!TIP]
  > 存在一项[预览剪裁功能](trim-self-contained.md)，它可进一步减小部署的大小。

- **较难更新 .NET 版本**\
只能通过发布新版本的应用来升级（与应用一起分发的）.NET 运行时。 但是，.NET 将在运行你的应用的计算机中针对框架库按需更新关键安全修补程序。 由你负责此安全修补程序方案的端到端验证。

### <a name="examples"></a>示例

发布独立应用。 创建 macOS 64 位可执行文件。

```dotnet
dotnet publish -r osx-x64
```

发布独立应用。 创建 Windows 64 位可执行文件。

```dotnet
dotnet publish -r win-x64
```

## <a name="publish-with-readytorun-images"></a>使用 ReadyToRun 映像发布

使用 ReadyToRun 映像发布可以缩短应用程序的启动时间，但代价是增加应用程序的大小。 若要使用 ReadyToRun 映像发布，请参阅 [ReadyToRun](ready-to-run.md) 来了解更多详情。

### <a name="advantages"></a>优点

- 缩短了启动时间\
应用程序运行 JIT 所花费的时间将缩短。

### <a name="disadvantages"></a>缺点

- 增加了大小\
应用程序在磁盘上的大小将增加。

### <a name="examples"></a>示例

使用 ReadyToRun 映像发布独立式应用。 创建 macOS 64 位可执行文件。

```dotnet
dotnet publish -c Release -r osx-x64 -p:PublishReadyToRun=true
```

使用 ReadyToRun 映像发布独立式应用。 创建 Windows 64 位可执行文件。

```dotnet
dotnet publish -c Release -r win-x64 -p:PublishReadyToRun=true
```

## <a name="see-also"></a>请参阅

- [使用 .NET CLI 部署 .NET 应用。](deploy-with-cli.md)
- [使用 Visual Studio 部署 .NET 应用。](deploy-with-vs.md)
- [.NET 运行时标识符 (RID) 目录。](../rid-catalog.md)
- [选择要使用的 .NET 版本。](../versions/selection.md)
