---
title: 使用 .NET CLI 发布应用
description: 了解如何使用 .NET CLI 命令来发布 .NET 应用程序。
author: adegeo
ms.author: adegeo
ms.date: 02/05/2021
dev_langs:
- csharp
- vb
ms.openlocfilehash: 65c9c4929d9ed8ab4005677fbcb0a3745ed14190
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874298"
---
# <a name="publish-net-apps-with-the-net-cli"></a>使用 .NET CLI 发布 .NET 应用

本文展示了如何使用命令行来发布 .NET 应用程序。 .NET 提供了三种发布应用程序的方法。 依赖于框架的部署会生成一个跨平台 .dll 文件，此文件使用本地安装的 .NET 运行时。 依赖于框架的可执行文件会生成一个特定于平台的可执行文件，此文件使用本地安装的 .NET 运行时。 独立式可执行文件会生成一个特定于平台的可执行文件，并包含 .NET 运行时的本地副本。

有关这些发布模式的概述，请参阅 [.NET 应用程序部署](index.md)。

正在查找有关 CLI 的快速帮助？ 下表列出了一些关于如何发布应用的示例。 可以使用 `-f <TFM>` 参数或通过编辑项目文件来指定目标框架。 有关详细信息，请参阅[发布基本知识](#publishing-basics)。

| 发布模式                   | SDK 版本 | 命令                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| [依赖框架的部署](#framework-dependent-deployment) | 2.1         | `dotnet publish -c Release`                                 |
|                                | 3.1         | `dotnet publish -c Release -p:UseAppHost=false`             |
|                                | 5.0         | `dotnet publish -c Release -p:UseAppHost=false`             |
| [依赖于框架的可执行文件](#framework-dependent-executable) | 3.1         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |
|                                | 5.0         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |
| [独立部署](#self-contained-deployment)      | 2.1         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | 3.1         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | 5.0         | `dotnet publish -c Release -r <RID> --self-contained true`  |

\* 当使用 SDK 版本 3.1 或更高版本时，依赖于框架的可执行文件是运行基本 `dotnet publish` 命令时的默认发布模式。

> [!NOTE]
> `-c Release` 参数不是必需的。 它作为提醒提供来发布应用的发行内部版本。

## <a name="publishing-basics"></a>发布基本知识

发布应用时，项目文件的 `<TargetFramework>` 设置指定默认目标框架。 可以将目标框架更改为任意有效[目标框架名字对象 (TFM)](../../standard/frameworks.md)。 例如，如果项目使用 `<TargetFramework>netcoreapp2.1</TargetFramework>`，则会创建以 .NET Core 2.1 为目标的二进制文件。 此设置中指定的 TFM 是[`dotnet publish`](../tools/dotnet-publish.md) 命令使用的默认目标。

若要以多个框架为目标，则可以将 `<TargetFrameworks>` 设置设为多个 TFM 值（以分号分隔）。 当你生成应用时，会为每个目标框架生成一个内部版本。 但是，当你发布应用时，必须使用 `dotnet publish -f <TFM>` 命令指定目标框架。

除非另有设置，否则 [`dotnet publish`](../tools/dotnet-publish.md) 命令的输出目录为 `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/`。 除非使用 `-c` 参数进行更改，否则默认的 BUILD-CONFIGURATION 模式为 Debug。 例如，`dotnet publish -c Release -f netcoreapp2.1` 发布到 `./bin/Release/netcoreapp2.1/publish/`。

如果你使用 .NET Core SDK 3.1 或更高版本，则默认发布模式为依赖于框架的可执行文件。

如果你使用 .NET Core SDK 2.1，则默认发布模式为依赖于框架的部署。

### <a name="native-dependencies"></a>本机依赖项

如果应用具有本机依赖项，则只能在相同操作系统上运行。 例如，如果应用使用本机 Windows API，则不能在 macOS 或 Linux 上运行。 需要提供特定于平台的代码并为每个平台编译可执行文件。

另外，如果引用的库具有本机依赖项，则应用可能无法在每个平台上运行。 但是，引用的 NuGet 包可能包含特定于平台的版本，以便处理所需的本机依赖项。

使用本机依赖项分发应用时，可能需要使用 `dotnet publish -r <RID>` 开关来指定想要发布的目标平台。 有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。

有关特定于平台的二进制文件的详细信息，请参阅[依赖于框架的可执行文件](#framework-dependent-executable)和[独立部署](#self-contained-deployment)部分。

## <a name="sample-app"></a>示例应用

可使用以下应用来探索发布命令。 通过在终端中运行以下命令来创建应用：

```dotnetcli
mkdir apptest1
cd apptest1
dotnet new console
dotnet add package Figgle
```

控制台模板生成的 `Program.cs` 或 `Program.vb` 文件需要进行以下更改：

```csharp
using System;

namespace apptest1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"));
        }
    }
}
```

```vb
Module Program
    Sub Main(args As String())
        Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"))
    End Sub
End Module
```

运行应用 ([`dotnet run`](../tools/dotnet-run.md)) 时，将显示以下输出：

```terminal
  _   _      _ _         __        __         _     _ _
 | | | | ___| | | ___    \ \      / /__  _ __| | __| | |
 | |_| |/ _ \ | |/ _ \    \ \ /\ / / _ \| '__| |/ _` | |
 |  _  |  __/ | | (_) |    \ V  V / (_) | |  | | (_| |_|
 |_| |_|\___|_|_|\___( )    \_/\_/ \___/|_|  |_|\__,_(_)
                     |/
```

## <a name="framework-dependent-deployment"></a>依赖框架的部署

对于 .NET Core 2.1 SDK CLI，依赖于框架的部署 (FDD) 是基本 `dotnet publish` 命令的默认模式。 在更高版本的 SDK 中，[依赖于框架的可执行文件](#framework-dependent-executable)是默认模式。

将应用作为 FDD 发布时，会在 `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/` 文件夹中创建 `<PROJECT-NAME>.dll` 文件。 若要运行应用，请导航到输出文件夹并使用 `dotnet <PROJECT-NAME>.dll` 命令。

你的应用被配置为以特定版本的 .NET 为目标。 目标 .NET 运行时必须在运行应用的任何虚拟机上。 例如，如果你的应用以 .NET Core 3.1 为目标，则任何运行你的应用的虚拟机都必须安装 .NET Core 3.1 运行时。 如[发布基础知识](#publishing-basics)部分中所述，可以编辑项目文件为更改默认目标框架或面向多个框架。

发布 FDD 会创建一个应用，该应用会自动前滚到运行该应用的系统上可用的最新 .NET 安全补丁。 若要详细了解编译时的版本绑定，请参阅[选择要使用的 .NET 版本](../versions/selection.md#framework-dependent-apps-roll-forward)。

| 发布模式                   | SDK 版本 | 命令                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| 依赖框架的部署 | 2.1         | `dotnet publish -c Release`                                 |
|                                | 3.1         | `dotnet publish -c Release -p:UseAppHost=false`             |
|                                | 5.0         | `dotnet publish -c Release -p:UseAppHost=false`             |

## <a name="framework-dependent-executable"></a>依赖于框架的可执行文件

对于.NET 5（以及 .NET Core 3.1）SDK CLI，依赖于框架的可执行文件 (FDE) 是基本 `dotnet publish` 命令的默认模式。 只要想要面向当前操作系统，就不需要指定任何其他参数。

在此模式下，将创建特定于平台的可执行主机来托管跨平台应用。 此模式类似于 FDD，因为 FDD 需要 `dotnet` 命令形式的主机。 每个平台的主机可执行文件名各不相同，其文件名类似于 `<PROJECT-FILE>.exe`。 可以直接运行此可执行文件，而不是调用 `dotnet <PROJECT-FILE>.dll`，这仍然是运行应用的可接受方式。

你的应用被配置为以特定版本的 .NET 为目标。 目标 .NET 运行时必须在运行应用的任何虚拟机上。 例如，如果你的应用以 .NET Core 3.1 为目标，则任何运行你的应用的虚拟机都必须安装 .NET Core 3.1 运行时。 如[发布基础知识](#publishing-basics)部分中所述，可以编辑项目文件为更改默认目标框架或面向多个框架。

发布 FDE 会创建一个应用，此应用会自动前滚到运行此应用的系统上可用的最新 .NET 安全补丁。 若要详细了解编译时的版本绑定，请参阅[选择要使用的 .NET 版本](../versions/selection.md#framework-dependent-apps-roll-forward)。

对于 .NET 2.1，必须结合使用以下开关和 `dotnet publish` 命令来发布 FDE：

- `-r <RID>` 此开关使用标识符 (RID) 来指定目标平台。 有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。

- `--self-contained false` 此开关会禁用 `-r` 开关的默认行为，即禁止创建独立部署 (SCD)。 此开关会创建一个 FDE。

| 发布模式                   | SDK 版本 | 命令                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| 依赖于框架的可执行文件 | 3.1         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |
|                                | 5.0         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |

每次使用 `-r` 开关时，输出文件路都将更改为：`./bin/<BUILD-CONFIGURATION>/<TFM>/<RID>/publish/`

如果使用[示例应用](#sample-app)，请运行 `dotnet publish -f net5.0 -r win10-x64 --self-contained false`。 此命令将创建以下可执行文件：`./bin/Debug/net5.0/win10-x64/publish/apptest1.exe`

> [!NOTE]
> 可以通过启用全局固定模式来降低部署的总大小。 此模式适用于不具有全局意识且可以使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的格式约定、大小写约定以及字符串比较和排序顺序的应用程序。 若要详细了解全球化固定模式以及如何启用它，请参阅 [.NET 全球化固定模式](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md)。

## <a name="self-contained-deployment"></a>独立部署

当你发布独立式部署 (SCD) 时，.NET SDK 会创建特定于平台的可执行文件。 如果发布 SCD，则会包括运行应用所需的所有 .NET 文件，但不包括 [.NET 的本机依赖项](https://github.com/dotnet/core/blob/main/Documentation/prereqs.md)。 这些依赖项必须在应用运行前存在于系统中。

如果发布 SCD，则会创建一个不前滚到最新可用 .NET 安全补丁的应用。 若要详细了解编译时的版本绑定，请参阅[选择要使用的 .NET 版本](../versions/selection.md#self-contained-deployments-include-the-selected-runtime)。

必须通过 `dotnet publish` 命令使用以下开关来发布 SCD：

- `-r <RID>` 此开关使用标识符 (RID) 来指定目标平台。 有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。

- `--self-contained true` 此开关告知 .NET Core SDK 创建可执行文件作为 SCD。

| 发布模式                   | SDK 版本 | 命令                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| 独立部署      | 2.1         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | 3.1         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | 5.0         | `dotnet publish -c Release -r <RID> --self-contained true`  |

> [!NOTE]
> 可以通过启用全局固定模式来降低部署的总大小。 此模式适用于不具有全局意识且可以使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的格式约定、大小写约定以及字符串比较和排序顺序的应用程序。 有关全局固定模式及其启用方式的详细信息，请参阅 [.NET Core 全局固定模式](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md)。

## <a name="see-also"></a>另请参阅

- [.NET 应用程序部署概述](index.md)
- [.NET 运行时标识符 (RID) 目录](../rid-catalog.md)
