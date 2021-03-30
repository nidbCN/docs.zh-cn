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
# <a name="install-the-net-sdk-or-the-net-runtime-on-opensuse"></a><span data-ttu-id="f48ca-103">在 openSUSE 上安装 .NET SDK 或 .NET Runtime</span><span class="sxs-lookup"><span data-stu-id="f48ca-103">Install the .NET SDK or the .NET Runtime on openSUSE</span></span>

<span data-ttu-id="f48ca-104">openSUSE 支持 .NET。</span><span class="sxs-lookup"><span data-stu-id="f48ca-104">.NET is supported on openSUSE.</span></span> <span data-ttu-id="f48ca-105">本文介绍如何在 openSUSE 上安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="f48ca-105">This article describes how to install .NET on openSUSE.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a><span data-ttu-id="f48ca-106">支持的分发</span><span class="sxs-lookup"><span data-stu-id="f48ca-106">Supported distributions</span></span>

<span data-ttu-id="f48ca-107">下表列出了 openSUSE 15 上当前受支持的 .NET 版本。</span><span class="sxs-lookup"><span data-stu-id="f48ca-107">The following table is a list of currently supported .NET releases on openSUSE 15.</span></span> <span data-ttu-id="f48ca-108">这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 openSUSE 版本不再受支持之前仍受支持。</span><span class="sxs-lookup"><span data-stu-id="f48ca-108">These versions remain supported until either the version of [.NET reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of openSUSE is no longer supported.</span></span>

- <span data-ttu-id="f48ca-109">✔️ 指示 openSUSE 或 .NET 版本仍受支持。</span><span class="sxs-lookup"><span data-stu-id="f48ca-109">A ✔️ indicates that the version of openSUSE or .NET is still supported.</span></span>
- <span data-ttu-id="f48ca-110">❌ 指示 openSUSE 或 .NET 版本在该 openSUSE 版本上不受支持。</span><span class="sxs-lookup"><span data-stu-id="f48ca-110">A ❌ indicates that the version of openSUSE or .NET isn't supported on that openSUSE release.</span></span>
- <span data-ttu-id="f48ca-111">当 openSUSE 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。</span><span class="sxs-lookup"><span data-stu-id="f48ca-111">When both a version of openSUSE and a version of .NET have ✔️, that OS and .NET combination is supported.</span></span>

| <span data-ttu-id="f48ca-112">openSUSE</span><span class="sxs-lookup"><span data-stu-id="f48ca-112">openSUSE</span></span>                   | <span data-ttu-id="f48ca-113">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="f48ca-113">.NET Core 2.1</span></span> | <span data-ttu-id="f48ca-114">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="f48ca-114">.NET Core 3.1</span></span> | <span data-ttu-id="f48ca-115">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="f48ca-115">.NET 5.0</span></span> |
|----------------------------|---------------|---------------|----------------|
| <span data-ttu-id="f48ca-116">✔️ [15](#opensuse-15-)</span><span class="sxs-lookup"><span data-stu-id="f48ca-116">✔️ [15](#opensuse-15-)</span></span>     | <span data-ttu-id="f48ca-117">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="f48ca-117">✔️ 2.1</span></span>        | <span data-ttu-id="f48ca-118">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="f48ca-118">✔️ 3.1</span></span>        | <span data-ttu-id="f48ca-119">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="f48ca-119">✔️ 5.0</span></span> |

<span data-ttu-id="f48ca-120">以下 .NET 版本不再受到支持。</span><span class="sxs-lookup"><span data-stu-id="f48ca-120">The following versions of .NET are no longer supported.</span></span> <span data-ttu-id="f48ca-121">这些版本的下载仍保持发布状态：</span><span class="sxs-lookup"><span data-stu-id="f48ca-121">The downloads for these still remain published:</span></span>

- <span data-ttu-id="f48ca-122">3.0</span><span class="sxs-lookup"><span data-stu-id="f48ca-122">3.0</span></span>
- <span data-ttu-id="f48ca-123">2.2</span><span class="sxs-lookup"><span data-stu-id="f48ca-123">2.2</span></span>
- <span data-ttu-id="f48ca-124">2.0</span><span class="sxs-lookup"><span data-stu-id="f48ca-124">2.0</span></span>

## <a name="remove-preview-versions"></a><span data-ttu-id="f48ca-125">删除预览版本</span><span class="sxs-lookup"><span data-stu-id="f48ca-125">Remove preview versions</span></span>

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="opensuse-15-"></a><span data-ttu-id="f48ca-126">openSUSE 15 ✔️</span><span class="sxs-lookup"><span data-stu-id="f48ca-126">openSUSE 15 ✔️</span></span>

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo zypper install libicu
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
wget https://packages.microsoft.com/config/opensuse/15/prod.repo
sudo mv prod.repo /etc/zypp/repos.d/microsoft-prod.repo
sudo chown root:root /etc/zypp/repos.d/microsoft-prod.repo
```

[!INCLUDE [linux-zyp-install-50](includes/linux-install-50-zyp.md)]

## <a name="how-to-install-other-versions"></a><span data-ttu-id="f48ca-127">如何安装其他版本</span><span class="sxs-lookup"><span data-stu-id="f48ca-127">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="f48ca-128">包管理器疑难解答</span><span class="sxs-lookup"><span data-stu-id="f48ca-128">Troubleshoot the package manager</span></span>

<span data-ttu-id="f48ca-129">本部分提供有关使用包管理器安装 .NET 时可能会遇到的常见错误的信息。</span><span class="sxs-lookup"><span data-stu-id="f48ca-129">This section provides information on common errors you may get while using the package manager to install .NET.</span></span>

### <a name="unable-to-find-package"></a><span data-ttu-id="f48ca-130">找不到包</span><span class="sxs-lookup"><span data-stu-id="f48ca-130">Unable to find package</span></span>

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

### <a name="failed-to-fetch"></a><span data-ttu-id="f48ca-131">未能提取</span><span class="sxs-lookup"><span data-stu-id="f48ca-131">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="dependencies"></a><span data-ttu-id="f48ca-132">依赖项</span><span class="sxs-lookup"><span data-stu-id="f48ca-132">Dependencies</span></span>

<span data-ttu-id="f48ca-133">使用包管理器进行安装时，将为你安装这些库。</span><span class="sxs-lookup"><span data-stu-id="f48ca-133">When you install with a package manager, these libraries are installed for you.</span></span> <span data-ttu-id="f48ca-134">但是，如果手动安装 .NET 或发布自包含的应用，则需要确保已安装以下库：</span><span class="sxs-lookup"><span data-stu-id="f48ca-134">But, if you manually install .NET or you publish a self-contained app, you'll need to make sure these libraries are installed:</span></span>

- <span data-ttu-id="f48ca-135">krb5</span><span class="sxs-lookup"><span data-stu-id="f48ca-135">krb5</span></span>
- <span data-ttu-id="f48ca-136">libicu</span><span class="sxs-lookup"><span data-stu-id="f48ca-136">libicu</span></span>
- <span data-ttu-id="f48ca-137">libopenssl1_0_0</span><span class="sxs-lookup"><span data-stu-id="f48ca-137">libopenssl1_0_0</span></span>

<span data-ttu-id="f48ca-138">如果目标运行时环境的 OpenSSL 版本为1.1 或更高版本，则需要安装 compat-openssl10。</span><span class="sxs-lookup"><span data-stu-id="f48ca-138">If the target runtime environment's OpenSSL version is 1.1 or newer, you'll need to install **compat-openssl10**.</span></span>

<span data-ttu-id="f48ca-139">有关依赖项的详细信息，请参阅[独立式 Linux 应用](https://github.com/dotnet/core/blob/main/Documentation/self-contained-linux-apps.md)。</span><span class="sxs-lookup"><span data-stu-id="f48ca-139">For more information about the dependencies, see [Self-contained Linux apps](https://github.com/dotnet/core/blob/main/Documentation/self-contained-linux-apps.md).</span></span>

<span data-ttu-id="f48ca-140">对于使用 System.Drawing.Common 程序集的 .NET 应用，还需要以下依赖项：</span><span class="sxs-lookup"><span data-stu-id="f48ca-140">For .NET apps that use the *System.Drawing.Common* assembly, you'll also need the following dependency:</span></span>

- [<span data-ttu-id="f48ca-141">libgdiplus（版本 6.0.1 或更高版本）</span><span class="sxs-lookup"><span data-stu-id="f48ca-141">libgdiplus (version 6.0.1 or later)</span></span>](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > <span data-ttu-id="f48ca-142">可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。</span><span class="sxs-lookup"><span data-stu-id="f48ca-142">You can install a recent version of *libgdiplus* by adding the Mono repository to your system.</span></span> <span data-ttu-id="f48ca-143">有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。</span><span class="sxs-lookup"><span data-stu-id="f48ca-143">For more information, see <https://www.mono-project.com/download/stable/>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f48ca-144">后续步骤</span><span class="sxs-lookup"><span data-stu-id="f48ca-144">Next steps</span></span>

- [<span data-ttu-id="f48ca-145">如何为 .NET CLI 启用 Tab 自动补全</span><span class="sxs-lookup"><span data-stu-id="f48ca-145">How to enable TAB completion for the .NET CLI</span></span>](../tools/enable-tab-autocomplete.md)
- [<span data-ttu-id="f48ca-146">教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="f48ca-146">Tutorial: Create a console application with .NET SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)
