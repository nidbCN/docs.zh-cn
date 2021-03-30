---
title: .NET 工具
description: 如何安装、使用、更新和删除 .NET 工具。 包括全局工具、工具路径工具和本地工具。
author: KathleenDollard
ms.topic: how-to
ms.date: 02/12/2020
ms.openlocfilehash: d4fca6b52b1444cf73efc49a6c50294cc5f6015b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874818"
---
# <a name="how-to-manage-net-tools"></a>如何管理 .NET 工具

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

.NET 工具是一种特殊的 NuGet 包，其中包含控制台应用程序。 可以通过以下方式在计算机上安装该工具：

* 作为全局工具。

  工具二进制文件安装在添加到 PATH 环境变量的默认目录中。 无需指定工具位置即可从计算机上的任何目录调用该工具。 工具的一个版本用于计算机上的所有目录。

* 作为自定义位置中的全局工具（也称为工具路径工具）。

  工具二进制文件安装在你指定的位置中。 可以从安装目录调用该工具，也可以通过提供具有命令名称的目录或将目录添加到 PATH 环境变量来调用该工具。 工具的一个版本用于计算机上的所有目录。

* 作为本地工具（适用于 .NET Core SDK 3.0 及更高版本）。

  工具二进制文件安装在默认目录中。 可以从安装目录或其任何子目录调用该工具。 不同目录可以使用同一工具的不同版本。
  
  .NET CLI 使用清单文件跟踪哪些工具作为本地工具安装到目录。 将清单文件保存到源代码存储库的根目录中后，参与者可以克隆存储库并调用用于安装清单文件中列出的所有工具的单个 .NET CLI 命令。

> [!IMPORTANT]
> .NET 工具在完全信任环境中运行。 除非你信任工具作者，否则请勿安装 .NET 工具。

## <a name="find-a-tool"></a>查找工具

以下是查找工具的一些方法：

* 使用 [dotnet tool search](dotnet-tool-search.md) 命令查找发布到 NuGet.org 的工具。
* 使用“.NET 工具”包类型筛选器搜索 [NuGet](https://www.nuget.org) 网站。 有关详细信息，请参阅[查找和选择包](/nuget/consume-packages/finding-and-choosing-packages)。
* 在 [dotnet/aspnetcore GitHub 存储库的工具目录](https://github.com/dotnet/aspnetcore/tree/main/src/Tools)中查看 ASP.NET Core 团队创建的工具的源代码。
* 在 [.NET 诊断工具](../diagnostics/index.md#net-core-diagnostic-global-tools)中了解诊断工具。

## <a name="check-the-author-and-statistics"></a>查看作者和统计信息

由于 .NET 工具在完全信任环境中运行，并且全局工具已添加到 PATH 环境变量，因此它们的功能非常强大。 请勿下载不信任的人提供的工具。

如果该工具在 NuGet 中托管，可以通过搜索该工具来查看作者和统计信息。

## <a name="install-a-global-tool"></a>安装全局工具

若要将工具作为全局工具安装，请使用 [dotnet tool install](dotnet-tool-install.md) 的 `-g` 或 `--global` 选项，如以下示例中所示：

```dotnetcli
dotnet tool install -g dotnetsay
```

输出显示用于调用该工具和已安装的版本的命令，类似于以下示例：

```output
You can invoke the tool using the following command: dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
```

工具二进制文件的默认位置取决于操作系统：

| (OS)          | 路径                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
| Windows     | `%USERPROFILE%\.dotnet\tools` |

首次运行 SDK 时，会将此位置添加到用户的路径，因此，无需指定工具位置即可从任何目录调用全局工具。

工具访问特定于用户，而不针对计算机全局。 全局工具仅适用于安装了该工具的用户。

### <a name="install-a-global-tool-in-a-custom-location"></a>安装自定义位置中的全局工具

若要将工具作为自定义位置中的全局工具安装，请使用 [dotnet tool install](dotnet-tool-install.md) 的 `--tool-path` 选项，如以下示例中所示。

在 Windows 上：

```dotnetcli
dotnet tool install dotnetsay --tool-path c:\dotnet-tools
```

在 Linux 或 macOS 上：

```dotnetcli
dotnet tool install dotnetsay --tool-path ~/bin
```

.NET SDK 不将此位置自动添加至 PATH 环境变量。 若要[调用工具路径工具](#invoke-a-tool-path-tool)，必须确保可使用以下方法之一来调用命令：

* 将安装目录添加到 PATH 环境变量。
* 调用该工具时，指定该工具的完整路径。
* 从安装目录调用该工具。

## <a name="install-a-local-tool"></a>安装本地工具

**适用于 .NET Core 3.0 SDK 及更高版本。**

若要安装仅用于本地访问的工具（对于当前目录和子目录），必须将其添加到工具清单文件。 若要创建工具清单文件，请运行 `dotnet new tool-manifest` 命令：

```dotnetcli
dotnet new tool-manifest
```

此命令在“.config”  目录下创建一个名为“dotnet-tools.json”  的清单文件。 若要将本地工具添加到清单文件，请使用 [dotnet tool install](dotnet-tool-install.md) 命令并省略 `--global` 和 `--tool-path` 选项，如以下示例中所示  ：

```dotnetcli
dotnet tool install dotnetsay
```

命令输出显示新安装的工具所在的清单文件，类似于以下示例：

```console
You can invoke the tool from this directory using the following command:
dotnet tool run dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
Entry is added to the manifest file /home/name/botsay/.config/dotnet-tools.json.
```

以下示例显示安装了两个本地工具的清单文件：

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    },
    "dotnetsay": {
      "version": "2.1.3",
      "commands": [
        "dotnetsay"
      ]
    }
  }
}
```

通常将本地工具添加到存储库的根目录。 将清单文件签入到存储库后，从存储库中签出代码的开发人员会获得最新的清单文件。 若要安装清单文件中列出的所有工具，请运行 `dotnet tool restore` 命令：

```dotnetcli
dotnet tool restore
```

输出表明还原了哪些工具：

```console
Tool 'botsay' (version '1.0.0') was restored. Available commands: botsay
Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
Restore was successful.
```

## <a name="install-a-specific-tool-version"></a>安装特定工具版本

若要安装工具的预发布版本或特定版本，请使用 `--version` 选项指定版本号，如以下示例中所示：

```dotnetcli
dotnet tool install dotnetsay --version 2.1.3
```

## <a name="use-a-tool"></a>使用工具

用于调用工具的命令可能不同于安装的包的名称。 若要显示计算机上目前安装的所有工具，请使用 [dotnet tool list](dotnet-tool-list.md) 命令：

```dotnetcli
dotnet tool list
```

输出显示每个工具的版本和命令，类似于以下示例：

```console
Package Id      Version      Commands       Manifest
-------------------------------------------------------------------------------------------
botsay          1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
dotnetsay       2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
```

如本示例中所示，列表显示了本地工具。 若要查看全局工具，请使用 `--global` 选项，若要查看工具路径工具，请使用 `--tool-path` 选项。

### <a name="invoke-a-global-tool"></a>调用全局工具

对于全局工具，请单独使用工具命令。 例如，如果命令为 `dotnetsay` 或 `dotnet-doc`，则可以使用以下命令调用该工具：

```console
dotnetsay
dotnet-doc
```

如果命令以前缀 `dotnet-` 开头，则调用该工具的另一种方法是使用 `dotnet` 命令并省略工具命令前缀。 例如，如果命令为 `dotnet-doc`，则可以使用以下命令调用该工具：

```dotnetcli
dotnet doc
```

但是，在以下情况下，不能使用 `dotnet` 命令来调用全局工具：

* 全局工具和本地工具具有以 `dotnet-` 为前缀的相同命令。
* 你希望从本地工具范围内的目录调用全局工具。

在这种情况下，`dotnet doc` 和 `dotnet dotnet-doc` 调用本地工具。 若要调用全局工具，请单独使用命令：

```dotnetcli
dotnet-doc
```

### <a name="invoke-a-tool-path-tool"></a>调用工具路径工具

若要调用使用 `tool-path` 选项安装的全局工具，请确保该命令可用，如[本文前面](#install-a-global-tool-in-a-custom-location)所述。

### <a name="invoke-a-local-tool"></a>调用本地工具

若要调用本地工具，必须从安装目录使用 `dotnet` 命令。 可以使用长格式 (`dotnet tool run <COMMAND_NAME>`) 或短格式 (`dotnet <COMMAND_NAME>`)，如以下示例中所示：

```dotnetcli
dotnet tool run dotnetsay
dotnet dotnetsay
```

如果命令以 `dotnet-` 为前缀，则可以在调用该工具时包括或省略前缀。 例如，如果命令为 `dotnet-doc`，则可以使用以下任何示例调用本地工具：

```dotnetcli
dotnet tool run dotnet-doc
dotnet dotnet-doc
dotnet doc
```

## <a name="update-a-tool"></a>更新工具

更新工具涉及卸载该工具并重新安装它的最新稳定版。 若要更新工具，请使用具有用于安装该工具的相同选项的 [dotnet tool update](dotnet-tool-update.md) 命令：

```dotnetcli
dotnet tool update --global <packagename>
dotnet tool update --tool-path <packagename>
dotnet tool update <packagename>
```

对于本地工具，SDK 通过在当前目录和父目录中查找来查找包含包 ID 的第一个清单文件。 如果任何清单文件中都没有此类包 ID，SDK 会将新条目添加到最近的清单文件。

## <a name="uninstall-a-tool"></a>卸载工具

使用具有用于安装该工具的相同选项的 [dotnet tool uninstall](dotnet-tool-uninstall.md) 命令删除工具：

```dotnetcli
dotnet tool uninstall --global <packagename>
dotnet tool uninstall --tool-path <packagename>
dotnet tool uninstall <packagename>
```

对于本地工具，SDK 通过在当前目录和父目录中查找来查找包含包 ID 的第一个清单文件。

## <a name="get-help-and-troubleshoot"></a>获取帮助和疑难解答

若要获取可用 `dotnet tool` 命令的列表，请输入以下命令：

```dotnetcli
dotnet tool --help
```

若要获取工具使用说明，请输入以下命令之一，或访问工具的网站：

```dotnetcli
<command> --help
dotnet <command> --help
```

如果工具无法安装或运行，请参阅[排查 .NET 工具使用问题](troubleshoot-usage-issues.md)。

## <a name="see-also"></a>请参阅

- [教程：使用 .NET CLI 创建 .NET 工具](global-tools-how-to-create.md)
- [教程：使用 .NET CLI 安装和使用 .NET 全局工具](global-tools-how-to-use.md)
- [教程：使用 .NET CLI 安装和使用 .NET 本地工具](local-tools-how-to-use.md)
