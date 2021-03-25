---
title: 在 Linux 上通过 Snap 安装 .NET - .NET
description: 演示如何通过 Snap 在 Linux 上安装 .NET SDK 或 .NET Runtime。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: 0d91f5049c92df240e2c3e26bc67952abe17fedc
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190094"
---
# <a name="install-the-net-sdk-or-the-net-runtime-with-snap"></a>通过 Snap 安装 .NET SDK 或 .NET Runtime

使用 Snap 包安装 .NET SDK 或 .NET Runtime。 对于内置于 Linux 发行版的包管理器而言，Snap 是一种很好的替代方法。 本文介绍如何通过 [Snap](https://snapcraft.io/dotnet-sdk) 安装 .NET。

Snap 是应用及其依赖项的捆绑包，无需修改即可在多个不同的 Linux 发行版中正常运行。 可以从 Snap Store 中发现和安装 Snap。 若要详细了解 Snap，请参阅[开始使用 Snap](https://snapcraft.io/docs/getting-started)。

> [!CAUTION]
> Windows 10 上的 WSL2 不支持 Snap 包。 作为替代方法，可使用 [`dotnet-install` 脚本](linux-scripted-manual.md#scripted-install)或特定 WSL2 发行版的包管理器。 虽然可以尝试使用 [Snapcraft 论坛中的替代方法](https://forum.snapcraft.io/t/running-snaps-on-wsl2-insiders-only-for-now/13033)启用 Snap，但该方法不受支持，因此不建议这样做。

## <a name="net-releases"></a>.NET 版本

只有 ✔️ 受支持的 .NET SDK 版本，才能通过 Snap 获取。 从版本 2.1 开始，所有.NET Runtime 版本均可通过 Snap 获取。 下表列出了 .NET（和 .NET Core）版本：

| ✔️ 受支持 | ❌ 不受支持 |
|-------------|---------------|
| 5.0         | 3.0           |
| 3.1 (LTS)   | 2.2           |
| 2.1 (LTS)   | 2.0           |
|             | 1.1           |
|             | 1.0           |

有关 .NET 版本的生命周期的详细信息，请参阅 [.NET Core 和 .NET 5 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。

## <a name="sdk-or-runtime"></a>SDK 或 Runtime

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="install-the-sdk"></a>安装 SDK

适用于 .NET SDK 的 Snap 包都是在同一标识符（即 `dotnet-sdk`）下发布的。 可以通过指定通道来安装特定版本的 SDK。 SDK 包括相应的运行时。 下表列出了通道：

| .NET 版本 | Snap 包或通道  |
|--------------|--------------------------|
| 5.0          | `5.0` 或 `latest/stable` |
| 3.1 (LTS)    | `3.1` 或 `lts/stable`    |
| 2.1 (LTS)    | `2.1`                    |

若要安装适用于 .NET SDK 的 Snap 包，请运行 `snap install` 命令。 使用 `--channel` 参数来指明要安装哪个版本。 如果省略此参数，则使用 `latest/stable`。 在下面的示例中，指定的是 `5.0`：

```bash
sudo snap install dotnet-sdk --classic --channel=5.0
```

接下来，使用 `snap alias` 命令为系统注册 `dotnet` 命令：

```bash
sudo snap alias dotnet-sdk.dotnet dotnet
```

此命令的格式为 `sudo snap alias {package}.{command} {alias}`。 可以选择所需的任何 `{alias}` 名称。 例如，可以根据 Snap 安装的特定版本来命名此命令：`sudo snap alias dotnet-sdk.dotnet dotnet50`。 运行命令 `dotnet50` 时，将调用这一特定版本的 .NET。 但选择其他别名与大多数教程和示例不兼容，因为它们需要使用 `dotnet` 命令。

## <a name="install-the-runtime"></a>安装运行时

适用于 .NET Runtime 的 Snap 包都是在各自的包标识符下发布的。 下表列出了这些包标识符：

| .NET 版本      | Snap 包        |
|-------------------|---------------------|
| 5.0               | `dotnet-runtime-50` |
| 3.1 (LTS)         | `dotnet-runtime-31` |
| 3.0               | `dotnet-runtime-30` |
| 2.2               | `dotnet-runtime-22` |
| 2.1 (LTS)         | `dotnet-runtime-21` |

若要安装适用于 .NET Runtime 的 Snap 包，请运行 `snap install` 命令。 在下面的示例中，安装的是 .NET 5.0：

```bash
sudo snap install dotnet-runtime-50 --classic
```

接下来，使用 `snap alias` 命令为系统注册 `dotnet` 命令：

```bash
sudo snap alias dotnet-runtime-50.dotnet dotnet
```

此命令的格式为 `sudo snap alias {package}.{command} {alias}`。 可以选择所需的任何 `{alias}` 名称。 例如，可以根据 Snap 安装的特定版本来命名此命令：`sudo snap alias dotnet-runtime-50.dotnet dotnet50`。 运行命令 `dotnet50` 时，将调用特定版本的 .NET。 但选择其他别名与大多数教程和示例不兼容，因为它们需要使用 `dotnet` 命令。

## <a name="export-the-install-location"></a>导出安装位置

`DOTNET_ROOT` 环境变量经常被工具用来确定 .NET 的安装位置。 通过 Snap 安装 .NET 时，不配置此环境变量。 应在配置文件中配置 DOTNET_ROOT 环境变量。 Snap 的路径采用以下格式：`/snap/{package}/current`。 例如，如果你安装了 `dotnet-sdk` Snap，则使用以下命令将环境变量设置为 .NET 所在的位置：

```bash
export DOTNET_ROOT=/snap/dotnet-sdk/current
```

> [!TIP]
> 前面的 `export` 命令只为运行它的终端会话设置环境变量。
>
> 你可以编辑 shell 配置文件，永久地添加这些命令。 Linux 提供了许多不同的 shell，每个都有不同的配置文件。 例如：
>
> - **Bash Shell**：~/.bash_profile、~/.bashrc
> - **Korn Shell**：~/.kshrc 或 .profile
> - **Z Shell**：~/.zshrc 或 .zprofile
>
> 为 shell 编辑相应的源文件并添加 `export DOTNET_ROOT=/snap/dotnet-sdk/current`。

## <a name="tlsssl-certificate-errors"></a>TLS/SSL 证书错误

通过 Snap 安装 .NET 后，可能会在某些发行版上找不到 .NET TLS/SSL 证书，并且可能会在 `restore` 期间看到以下错误：

```bash
Processing post-creation actions...
Running 'dotnet restore' on /home/myhome/test/test.csproj...
  Restoring packages for /home/myhome/test/test.csproj...
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error : Unable to load the service index for source https://api.nuget.org/v3/index.json. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The SSL connection could not be established, see inner exception. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The remote certificate is invalid according to the validation procedure. [/home/myhome/test/test.csproj]
```

若要解决此问题，请设置一些环境变量：

```bash
export SSL_CERT_FILE=[path-to-certificate-file]
export SSL_CERT_DIR=/dev/null
```

证书位置因发行版而异。 下面是我们在发行版中遇到此问题的位置。

| 分发 | 位置                                            |
|--------------|-----------------------------------------------------|
| **Fedora**   | `/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem` |
| **OpenSUSE** | `/etc/ssl/ca-bundle.pem`                            |
| **Solus**    | `/etc/ssl/certs/ca-certificates.crt`                |

## <a name="next-steps"></a>后续步骤

- [如何为 .NET CLI 启用 Tab 自动补全](../tools/enable-tab-autocomplete.md)
- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
