---
title: 如何安装 ML.NET 命令行接口 (CLI) 工具
description: 了解如何安装、升级、降级和卸载 ML.NET 命令行接口 (CLI) 工具。
ms.date: 06/08/2020
ms.custom: mlnet-tooling
ms.openlocfilehash: a2fdaa06c1602a2c3986b2f034cd1840e39d16a3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876768"
---
# <a name="how-to-install-the-mlnet-command-line-interface-cli-tool"></a>如何安装 ML.NET 命令行接口 (CLI) 工具

了解如何在 Windows、Mac 或 Linux 上安装 ML.NET CLI（命令行接口）。

ML.NET CLI 使用自动化机器学习 (AutoML) 和训练数据集生成质量良好的 ML.NET 模型和源代码。

> [!NOTE]
> 本主题涉及目前处于预览状态的 ML.NET CLI 和 ML.NET AutoML，且材料可能会有所变化。

## <a name="pre-requisites"></a>先决条件

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1)

- （可选）[Visual Studio 2019](https://visualstudio.microsoft.com/vs/)

可以使用 Visual Studio 按 `F5` 键或 `dotnet run` (.NET Core CLI) 运行生成的 C# 代码项目。

注意：如果在安装 .NET Core SDK 后，`dotnet tool` 命令不起作用，请从 Windows 注销并再次登录。

## <a name="install"></a>安装

ML.NET CLI 的安装方式与任何其他 dotnet 全局工具一样。 使用 `dotnet tool install` .NET Core CLI 命令。

以下示例显示如何在默认 NuGet 源位置安装 ML.NET CLI：

```dotnetcli
dotnet tool install -g mlnet
```

如果无法安装该工具（即，如果它在默认 NuGet 源中不可用），则会显示错误消息。 检查所需源是否正在被检查。

如果安装成功，会出现一条消息，显示用于调用工具的命令以及所安装的版本，类似于以下示例：

```console
You can invoke the tool using the following command: mlnet
Tool 'mlnet' (version 'X.X.X') was successfully installed.
```

可以通过键入以下命令来确认安装是否成功：

```console
mlnet
```

应看到 mlnet 工具的可用命令帮助，例如“classification”命令。

## <a name="install-a-specific-release-version"></a>安装特定版本

如果想安装工具的预发布版本或特定版本，可以采用以下格式指定[框架](../../standard/frameworks.md)：

```dotnetcli
dotnet tool install -g mlnet --framework <FRAMEWORK>
```

还可以通过键入以下命令来检查包是否已正确安装：

```dotnetcli
dotnet tool list -g
```

## <a name="uninstall-the-cli-package"></a>卸载 CLI 包

键入以下命令，从本地计算机卸载包：

```dotnetcli
dotnet tool uninstall mlnet -g
```

## <a name="update-the-cli-package"></a>更新 CLI 包

键入以下命令，从本地计算机更新包：

```dotnetcli
dotnet tool update -g mlnet
```

## <a name="set-up-cli-suggestions-tab-based-auto-completion"></a>设置 CLI 建议（Tab 自动补全）

由于 ML.NET CLI 基于 `System.CommandLine`，它具有对 Tab 补全的内置支持。

Tab 自动补全工作原理的示例如以下动画所示：

![图像](./media/cli-tab-completion.gif)

“Tab 自动补全”（参数建议）适用于 *Windows PowerShell* 和 *macOS/Linux bash*，但它不适用于 *Windows CMD*。

若要启用它，在当前预览版本中，最终用户必须为每个 shell 执行一次相关步骤，如下所述。 完成此操作后，补全将适用于使用 `System.CommandLine` 编写的所有应用，例如 ML.NET CLI。

在想要启用补全的计算机上，需要执行两个操作。

1. 通过运行以下命令安装 `dotnet-suggest` 全局工具：

    ```dotnetcli
    dotnet tool install dotnet-suggest -g
    ```

2. 将适当的填充码脚本添加到 shell 配置文件中。 可能必须创建一个 shell 配置文件。 填充码脚本会将补全请求从 shell 转发到 `dotnet-suggest` 工具，其会委托到相应的基于 `System.CommandLine` 的应用。

    - 对于 bash，将 [dotnet-suggest-shim.bash](https://github.com/dotnet/System.CommandLine/blob/main/src/System.CommandLine.Suggest/dotnet-suggest-shim.bash) 的内容添加到 `~/.bash_profile`。

    - 对于 PowerShell，将 [dotnet-suggest-shim.ps1](https://github.com/dotnet/System.CommandLine/blob/main/src/System.CommandLine.Suggest/dotnet-suggest-shim.ps1) 的内容添加到 PowerShell 配置文件中。 可以通过在控制台中运行以下命令来查找 PowerShell 配置文件的预期路径：

    ```console
    echo $profile
    ```

（对于其他 shell，[查找](https://github.com/dotnet/System.CommandLine/issues?q=is%3Aissue+is%3Aopen+label%3A%22shell+suggestion%22)或创建[问题](https://github.com/dotnet/System.CommandLine/issues)。）

## <a name="installation-directory"></a>安装目录

ML.NET CLI 可安装在默认目录或特定位置。 默认目录为：

| (OS)          | 路径                          |
|-------------|-------------------------------|
| Linux/macOS | `$HOME/.dotnet/tools`         |
| Windows     | `%USERPROFILE%\.dotnet\tools` |

这些位置在首次运行 SDK 时添加到用户的路径，所以可以直接调用安装在这里的全局工具。

注意：全局工具特定于用户，而不针对计算机全局。 特定于用户的意思是无法在一台计算机上安装所有用户都可用的全局工具。 只有安装了该工具的用户配置文件才能使用它。

也可在特定目录安装全局工具。 如果在特定目录安装全局工具，用户必须确保命令是可用的，方法是将该目录添加至路径、使用指定目录调用命令或从特定目录调用该工具。
在这种情况下，.NET Core CLI 不将此地址自动添加至 PATH 环境变量。

## <a name="see-also"></a>请参阅

- [ML.NET CLI 概述](../automate-training-with-cli.md)
- [教程：使用 ML.NET CLI 分析情绪](../tutorials/sentiment-analysis-cli.md)
- [ML.NET CLI auto-train 命令参考指南](../reference/ml-net-cli-reference.md)
- [ML.NET CLI 中的遥测](../resources/ml-net-cli-telemetry.md)
