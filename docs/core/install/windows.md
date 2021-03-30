---
title: 在 Windows 上安装 .NET
description: 了解可在其上安装 .NET 的 Windows 版本。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: 9b0c5c2ea26546a142029b730dfd38923231bae4
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873739"
---
# <a name="install-net-on-windows"></a>在 Windows 上安装 .NET

> [!div class="op_single_selector"]
>
> - [在 Windows 上安装](windows.md)
> - [在 macOS 上安装](macos.md)
> - [在 Linux 上安装](linux.md)

本文介绍如何在 Windows 上安装 .NET。 .NET 由运行时和 SDK 组成。 运行时用于运行 .NET 应用，应用可能包含也可能不包含它。 SDK 用于创建 .NET 应用和库。 .NET 运行时始终随 SDK 一起安装。

最新版本的 .NET 是 5.0。

> [!div class="button"]
> [下载 .NET](https://dotnet.microsoft.com/download/dotnet)

## <a name="supported-releases"></a>支持的版本

下表列出了当前支持的 .NET 版本以及支持它们的 Windows 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Windows 版本达到生命周期](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet)之前仍受支持。

Windows 10 版本终止服务日期按版本分段。 下表中仅考虑家庭版、专业版、专业教育版和专业工作站版。    查看 [Windows 生命周期事实表单](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet)，了解具体的详细信息。

> [!TIP]
> `+` 表示最低版本。

| 操作系统            | .NET Core 2.1 | .NET Core 3.1 | .NET 5 |
|-----------------------------|---------------|---------------|--------|
| Windows 10/Windows Server 版本 20H2    | ✔️           | ✔️            | ✔️    |
| Windows 10/Windows Server 版本 2004    | ✔️           | ✔️            | ✔️    |
| Windows 10/Windows Server 版本 1909    | ✔️           | ✔️            | ✔️    |
| Windows 10/Windows Server 版本 1903    | ✔️           | ✔️            | ✔️    |
| Windows 10 版本 1809    | ✔️           | ✔️            | ✔️    |
|  Windows 10 版本 1803    | ✔️           | ✔️            | ✔️    |
|  Windows 10 版本 1709    | ✔️           | ✔️            | ✔️    |
| Windows 10 版本 1607    | ✔️           | ✔️            | ✔️    |
| Windows 8.1                 | ✔️           | ✔️            | ✔️    |
| Windows 7 SP1 [ESU][esu]    | ✔️           | ✔️            | ✔️    |
| Windows Server 2019<br>Windows Server 2016<br>Windows Server 2012 R2<br>      | ✔️           | ✔️            | ✔️    |
| Windows Server Core 2012 R2 | ✔️           | ✔️            | ✔️    |
| Nano Server 版本 1809+  | ✔️           | ✔️            | ✔️    |
| Nano Server 版本 1803   | ✔️           | ✔️            | ❌    |

## <a name="unsupported-releases"></a>不支持的版本

以下 .NET 版本 ❌ 不再受到支持：

- 3.0
- 2.2
- 2.0

## <a name="runtime-information"></a>运行时信息

运行时用于运行使用 .NET 创建的应用。 应用作者发布应用时，可以在其应用中包含运行时。 如果作者未包含运行时，则由用户安装运行时。

可以在 Windows 上安装三个不同的运行时：

- *ASP.NET Core 运行时*\
  运行 ASP.NET Core 应用。 包括 .NET 运行时。

- *桌面运行时*\
  运行适用于 Windows 的 .NET WPF 和 Windows 窗体桌面应用。 包括 .NET 运行时。

- .NET 运行时\
  此运行时是最简单的运行时，不包括任何其他运行时。 强烈建议同时安装 ASP.NET Core 运行时和桌面运行时，以最大限度地提升与 .NET 应用的兼容性 。

> [!div class="button"]
> [下载 .NET 运行时](https://dotnet.microsoft.com/download/dotnet)

## <a name="sdk-information"></a>SDK 信息

SDK 用于生成和发布 .NET 应用和库。 安装 SDK 会包含三个[运行时](#runtime-information)：ASP.NET Core、桌面和 .NET。

## <a name="dependencies"></a>依赖项

<!-- markdownlint-disable MD025 -->
<!-- markdownlint-disable MD024 -->

# <a name="net-50"></a>[.NET 5.0](#tab/net50)

.NET 5.0 支持下列 Windows 版本：

> [!NOTE]
> `+` 表示最低版本。

| (OS)                  | Version       | 体系结构   |
|---------------------|---------------|-----------------|
| Windows 10 客户端   | 版本 1607+ | x64、x86、ARM64 |
| Windows 客户端      | 7 SP1+、8.1   | x64、x86        |
| Windows Server      | 2012 R2+      | x64、x86        |
| Windows Server 核心 | 2012 R2+      | x64、x86        |
| Nano Server         | 版本 1809+ | X64             |

有关 .NET 5.0 支持的操作系统、发行版和生命周期策略的详细信息，请参阅 [.NET 5.0 支持的 OS 版本](https://github.com/dotnet/core/blob/main/release-notes/5.0/5.0-supported-os.md)。

# <a name="net-core-31"></a>[.NET Core 3.1](#tab/netcore31)

.NET Core 3.1 支持下列 Windows 版本：

> [!NOTE]
> `+` 表示最低版本。

| (OS)                            | Version                        | 体系结构   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows 客户端                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 客户端             | 版本 1607+                  | x64、x86        |
| Windows Server                | 2012 R2+                       | x64、x86        |
| Nano Server                   | 版本 1803+                  | x64、ARM32      |

有关 .NET Core 3.1 支持的操作系统、发行版和生命周期策略的详细信息，请参阅 [.NET Core 3.1 支持的 OS 版本](https://github.com/dotnet/core/blob/main/release-notes/3.1/3.1-supported-os.md)。

# <a name="net-core-30"></a>[.NET Core 3.0](#tab/netcore30)

目前不 ❌ 支持 .NET Core 3.0 *。* 有关详细信息，请参阅 [.NET Core 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。

.NET Core 3.0 支持下列 Windows 版本：

> [!NOTE]
> `+` 表示最低版本。

| (OS)                            | Version                        | 体系结构   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows 客户端                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 客户端             | 版本 1607+                  | x64、x86        |
| Windows Server                | 2012 R2+                       | x64、x86        |
| Nano Server                   | 版本 1803+                  | x64、ARM32      |

有关 .NET Core 3.0 支持的操作系统、发行版和生命周期策略的详细信息，请参阅 [.NET Core 3.0 支持的 OS 版本](https://github.com/dotnet/core/blob/main/release-notes/3.0/3.0-supported-os.md)。

# <a name="net-core-22"></a>[.NET Core 2.2](#tab/netcore22)

目前不 ❌ 支持 .NET Core 2.2。有关详细信息，请参阅 [.NET Core 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。

.NET Core 2.2 支持下列 Windows 版本：

> [!NOTE]
> `+` 表示最低版本。

| (OS)                            | Version                        | 体系结构   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows 客户端                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 客户端             | 版本 1607+                  | x64、x86        |
| Windows Server                | 2008 R2 SP1+                   | x64、x86        |
| Nano Server                   | 版本 1803+                   | x64、ARM32      |

有关 .NET Core 2.2 支持的操作系统、发行版和生命周期策略的详细信息，请参阅 [.NET Core 2.2 支持的 OS 版本](https://github.com/dotnet/core/blob/main/release-notes/2.2/2.2-supported-os.md)。

# <a name="net-core-21"></a>[.NET Core 2.1](#tab/netcore21)

.NET Core 2.1 支持下列 Windows 版本：

> [!NOTE]
> `+` 表示最低版本。

| (OS)                            | Version                        | 体系结构   |
| ----------------------------- | ------------------------------ | --------------- |
| Windows 客户端                | 7 SP1+、8.1                    | x64、x86        |
| Windows 10 客户端             | 版本 1607+                  | x64、x86        |
| Windows Server                | 2008 R2 SP1+                   | x64、x86        |
| Nano Server                   | 版本 1803+                  | x64            |

有关 .NET Core 2.1 支持的操作系统、发行版和生命周期策略的详细信息，请参阅 [.NET Core 2.1 支持的 OS 版本](https://github.com/dotnet/core/blob/main/release-notes/2.1/2.1-supported-os.md)。

### <a name="offline-install-for-windows-7"></a>Windows 7 的脱机安装

在 Windows 7 上执行 .NET Core 2.1 的脱机安装时，首先需要确保目标计算机上已安装最新的 [Microsoft 根证书颁发机构 2011](https://www.microsoft.com/pkiops/Docs/Repository.htm)。

certmgr.exe 工具可以自动安装证书，并从 Visual Studio 或 Windows SDK 获取该证书。 以下命令用于在运行 .NET Core 2.1 安装程序之前安装证书：

```console
certmgr.exe /add MicRooCerAut2011_2011_03_22.crt /s /r localMachine root
```

请务必查看[下面 Windows 7](#additional-deps) 所需的依赖项。

---

<!-- markdownlint-disable MD001 -->

### <a name="windows-7--vista--81--server-2008-r2--server-2012-r2"></a><a name="additional-deps"></a> Windows 7 / Vista / 8.1 / Server 2008 R2 / Server 2012 R2

如果要在以下 Windows 版本上安装 .NET SDK 或运行时，则需要其他依赖项：

| 操作系统         | 先决条件                                                                    |
|--------------------------|----------------------------------------------------------------------------------|
| Windows 7 SP1 [ESU][esu] | - Microsoft Visual C++ 2015-2019 Redistributable [64 位][vcc64] / [32 位][vcc32] <br> - KB3063858 [64 位][kb64] / [32 位][kb32] <br> - [Microsoft 根证书颁发机构 2011](https://www.microsoft.com/pkiops/Docs/Repository.htm)（仅限 .NET Core 2.1 脱机安装程序） |
| Windows Vista SP 2       | Microsoft Visual C++ 2015-2019 Redistributable [64 位][vcc64] / [32 位][vcc32] |
| Windows 8.1              | Microsoft Visual C++ 2015-2019 Redistributable [64 位][vcc64] / [32 位][vcc32] |
| Windows Server 2008 R2   | Microsoft Visual C++ 2015-2019 Redistributable [64 位][vcc64] / [32 位][vcc32] |
| Windows Server 2012 R2   | Microsoft Visual C++ 2015-2019 Redistributable [64 位][vcc64] / [32 位][vcc32] |

如果收到与以下 dll 之一相关的错误，也需要满足上述要求：

- api-ms-win-crt-runtime-l1-1-0.dll
- api-ms-win-cor-timezone-l1-1-0.dll
- hostfxr.dll

## <a name="install-with-powershell-automation"></a>使用 PowerShell 自动化安装

[dotnet-install 脚本](../tools/dotnet-install-script.md)用于运行时的 CI 自动化和非管理员安装。 可从 [dotnet-install 脚本引用页](../tools/dotnet-install-script.md)下载该脚本。

此脚本默认安装最新的[长期支持 (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) 版本，即 .NET Core 3.1。 可通过指定 `Channel` 开关以选择特定版本。 包括 `Runtime` 开关以安装运行时。 否则，该脚本安装 SDK。

```powershell
dotnet-install.ps1 -Channel 5.0 -Runtime aspnetcore
```

通过省略 `-Runtime` 开关来安装 SDK。 在此示例中将 `-Channel` 开关设置为 `Current`，这将安装受支持的最新版本。

```powershell
dotnet-install.ps1 -Channel Current
```

## <a name="install-with-visual-studio"></a>使用 Visual Studio 安装

如果你要使用 Visual Studio 开发 .NET 应用，请参阅下表，了解不同目标 .NET SDK 版本所需的 Visual Studio 最低版本。

| .NET SDK 版本      | Visual Studio 版本                      |
| --------------------- | ------------------------------------------ |
| 5.0                   | Visual Studio 2019 版本 16.8 或更高版本。 |
| 3.1                   | Visual Studio 2019 版本 16.4 或更高版本。 |
| 3.0                   | Visual Studio 2019 版本 16.3 或更高版本。 |
| 2.2                   | Visual Studio 2017 版本 15.9 或更高版本。 |
| 2.1                   | Visual Studio 2017 版本 15.7 或更高版本。 |

如果你已安装 Visual Studio，则可以使用以下步骤检查你的版本。

01. 打开 Visual Studio。
01. 选择“帮助” > “Microsoft Visual Studio”。
01. 从“关于”对话框中读取版本号。

Visual Studio 可安装最新的 .NET SDK 和运行时。

> [!div class="button"]
> [下载 Visual Studio](https://www.visualstudio.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2019)。

### <a name="select-a-workload"></a>选择工作负载

安装或修改 Visual Studio 时，根据要生成的应用程序的类型，选择以下一个或多个工作负载：

- “其他工具集”部分中的“.NET Core 跨平台开发”工作负荷 。
- “Web 和云”部分中的“ASP.NET 和 Web 开发”工作负荷 。
- “Web 和云”部分中的“Azure 开发”工作负载 。
- “桌面和移动”部分中的“NET 桌面开发”工作负载 。

[![具有 .NET Core 工作负载的 Windows Visual Studio 2019](media/install-sdk/windows-install-visual-studio-2019.png)](media/install-sdk/windows-install-visual-studio-2019.png#lightbox)

## <a name="install-alongside-visual-studio-code"></a>随 Visual Studio Code 一起安装

Visual Studio Code 是一个功能强大的轻量级源代码编辑器，可在桌面上运行。 Visual Studio Code 适用于 Windows、macOS 和 Linux。

虽然 Visual Studio Code 不像 Visual Studio 一样附带自动的 .NET Core 安装程序，但添加 .NET Core 支持非常简单。

01. [下载并安装 Visual Studio Code](https://code.visualstudio.com/Download)。
01. [下载并安装 .NET Core SDK](https://dotnet.microsoft.com/download/dotnet)。
01. [从 Visual Studio Code 市场安装 C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)。

## <a name="windows-installer"></a>Windows Installer

适用于 .NET 的[下载页面](https://dotnet.microsoft.com/download/dotnet)提供了 Windows Installer 可执行文件。

使用 Windows 安装程序安装 .NET 时，可以通过设置 `DOTNETHOME_X64` 和 `DOTNETHOME_X86` 参数来自定义安装路径：

```console
dotnet-sdk-3.1.301-win-x64.exe DOTNETHOME_X64="F:\dotnet\x64" DOTNETHOME_X86="F:\dotnet\x86"
```

如果要以无提示方式安装 .NET（例如在生产环境中）或要支持持续集成，请使用以下开关：

- `/install`\
安装 .NET。

- `/quiet`\
禁止显示任何 UI 和提示。

- `norestart`\
禁止任何重启尝试。

```console
dotnet-sdk-3.1.301-win-x64.exe /install /quiet /norestart
```

有关详细信息，请参阅[标准安装程序命令行选项](/windows/win32/msi/standard-installer-command-line-options)。

> [!TIP]
> 安装程序返回退出代码 0 以表示成功，返回退出代码 3010 以表示需要重启。 任何其他值通常都是错误代码。

## <a name="download-and-manually-install"></a>下载并手动安装

除了使用适用于 .NET 的 Windows 安装程序，还可以下载并手动安装 SDK 或运行时。 手动安装通常作为持续集成测试的一部分执行。 对于开发人员或用户，一般使用[安装程序](https://dotnet.microsoft.com/download/dotnet)会更好。

在下载 .NET SDK 和 .NET 运行时后，可以手动安装它们。 如果安装 .NET SDK，则无需安装相应的运行时。 首先，从以下站点之一下载 SDK 或运行时的二进制版本：

- [.NET 5.0 下载](https://dotnet.microsoft.com/download/dotnet/5.0)
- [.NET Core 3.1 下载](https://dotnet.microsoft.com/download/dotnet/3.1)
- [.NET Core 2.1 下载](https://dotnet.microsoft.com/download/dotnet/2.1)
- [所有 .NET Core 下载项](https://dotnet.microsoft.com/download/dotnet)

创建要将 .NET 提取到的目录，例如 `%USERPROFILE%\dotnet`。 然后，将下载的 zip 文件提取到该目录中。

默认情况下，.NET CLI 命令和应用不会使用通过这种方式安装的 .NET，并且你必须显式选择才能使用它。 为此，请更改用于启动应用程序的环境变量：

```console
set DOTNET_ROOT=%USERPROFILE%\dotnet
set PATH=%USERPROFILE%\dotnet;%PATH%
set DOTNET_MULTILEVEL_LOOKUP=0
```

使用此方法可以将多个版本安装到不同的位置，然后通过使用指向安装位置的环境变量运行应用程序来明确选择应用程序应使用哪个安装位置。

将 `DOTNET_MULTILEVEL_LOOKUP` 设置为 `0` 时，.NET 将忽略任何全局安装的 .NET 版本。 删除环境设置，让 .NET 在选择用于运行应用程序的最佳框架时考虑默认的全局安装位置。 默认值通常为 `C:\Program Files\dotnet`，这是安装 .NET 的安装程序所在的位置。

## <a name="docker"></a>Docker

容器提供了一种将应用程序与主机系统的其余部分隔离的轻量级方法。 同一计算机上的容器只共享内核，并使用为应用程序提供的资源。

.NET 可在 Docker 容器中运行。 官方 .NET Docker 映像发布到 Microsoft 容器注册表 (MCR)，用户可在 [Microsoft.NET Docker Hub 存储库](https://hub.docker.com/_/microsoft-dotnet)中找到这些映像。 每个存储库包含 .NET（SDK 或运行时）和可以使用的操作系统的不同组合的映像。

Microsoft 提供适合特定场景的映像。 例如，[ASP.NET Core 存储库](https://hub.docker.com/_/microsoft-dotnet-aspnet)提供针对在生产环境中运行 ASP.NET Core 应用生成的映像。

有关在 Docker 容器中使用 .NET 的详细信息，请参阅 [.NET 和 Docker 简介](../docker/introduction.md)和[示例](https://github.com/dotnet/dotnet-docker/blob/main/samples/README.md)。

## <a name="next-steps"></a>后续步骤

- [如何检查是否已安装 .NET](how-to-detect-installed-versions.md?pivots=os-windows)。
- [教程：Hello World 教程](../tutorials/with-visual-studio.md)。
- [教程：使用 Visual Studio Code 创建一个新应用](../tutorials/with-visual-studio-code.md)。
- [教程：使 .NET Core 应用容器化](../docker/build-container.md)。

[esu]: /troubleshoot/windows-client/windows-7-eos-faq/windows-7-extended-security-updates-faq
[vcc64]: https://aka.ms/vs/16/release/vc_redist.x64.exe
[vcc32]: https://aka.ms/vs/16/release/vc_redist.x86.exe
[kb64]: https://www.microsoft.com/download/details.aspx?id=47442
[kb32]: https://www.microsoft.com/download/details.aspx?id=47409
