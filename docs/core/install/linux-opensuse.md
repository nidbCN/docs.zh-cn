---
title: 在 openSUSE 上安装 .NET - .NET
description: 演示在 openSUSE 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: d238054a217a7295594db856d5497982572af377
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873947"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-opensuse"></a>在 openSUSE 上安装 .NET SDK 或 .NET Runtime

openSUSE 支持 .NET。 本文介绍如何在 openSUSE 上安装 .NET。

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a>支持的分发

下表列出了 openSUSE 15 上当前受支持的 .NET 版本。 这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 openSUSE 版本不再受支持之前仍受支持。

- ✔️ 指示 openSUSE 或 .NET 版本仍受支持。
- ❌ 指示 openSUSE 或 .NET 版本在该 openSUSE 版本上不受支持。
- 当 openSUSE 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。

| openSUSE                   | .NET Core 2.1 | .NET Core 3.1 | .NET 5.0 |
|----------------------------|---------------|---------------|----------------|
| ✔️ [15](#opensuse-15-)     | ✔️ 2.1        | ✔️ 3.1        | ✔️ 5.0 |

以下 .NET 版本不再受到支持。 这些版本的下载仍保持发布状态：

- 3.0
- 2.2
- 2.0

## <a name="remove-preview-versions"></a>删除预览版本

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="opensuse-15-"></a>openSUSE 15 ✔️

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo zypper install libicu
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
wget https://packages.microsoft.com/config/opensuse/15/prod.repo
sudo mv prod.repo /etc/zypp/repos.d/microsoft-prod.repo
sudo chown root:root /etc/zypp/repos.d/microsoft-prod.repo
```

[!INCLUDE [linux-zyp-install-50](includes/linux-install-50-zyp.md)]

## <a name="how-to-install-other-versions"></a>如何安装其他版本

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a>包管理器疑难解答

本部分提供有关使用包管理器安装 .NET 时可能会遇到的常见错误的信息。

### <a name="unable-to-find-package"></a>找不到包

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

### <a name="failed-to-fetch"></a>未能提取

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="dependencies"></a>依赖项

使用包管理器进行安装时，将为你安装这些库。 但是，如果手动安装 .NET 或发布自包含的应用，则需要确保已安装以下库：

- krb5
- libicu
- libopenssl1_0_0

如果目标运行时环境的 OpenSSL 版本为1.1 或更高版本，则需要安装 compat-openssl10。

有关依赖项的详细信息，请参阅[独立式 Linux 应用](https://github.com/dotnet/core/blob/main/Documentation/self-contained-linux-apps.md)。

对于使用 System.Drawing.Common 程序集的 .NET 应用，还需要以下依赖项：

- [libgdiplus（版本 6.0.1 或更高版本）](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > 可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。 有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。

## <a name="next-steps"></a>后续步骤

- [如何为 .NET CLI 启用 Tab 自动补全](../tools/enable-tab-autocomplete.md)
- [教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序](../tutorials/with-visual-studio-code.md)
