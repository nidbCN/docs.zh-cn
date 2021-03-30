---
title: 在 SLES 上安装 .NET - .NET
description: 演示在 SLES 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 01/06/2021
ms.openlocfilehash: db8773c82417eda0deac04f95cfe8199621d04c4
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875260"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-sles"></a><span data-ttu-id="0d9f6-103">在 SLES 上安装 .NET SDK 或 .NET 运行时</span><span class="sxs-lookup"><span data-stu-id="0d9f6-103">Install the .NET SDK or the .NET Runtime on SLES</span></span>

<span data-ttu-id="0d9f6-104">SLES 支持 .NET。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-104">.NET is supported on SLES.</span></span> <span data-ttu-id="0d9f6-105">本文介绍如何在 SLES 上安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-105">This article describes how to install .NET on SLES.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="supported-distributions"></a><span data-ttu-id="0d9f6-106">支持的发行版</span><span class="sxs-lookup"><span data-stu-id="0d9f6-106">Supported distributions</span></span>

<span data-ttu-id="0d9f6-107">下表列出了 SLES 12 SP2 和 SLES 15 上当前受支持的 .NET 版本。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-107">The following table is a list of currently supported .NET releases on both SLES 12 SP2 and SLES 15.</span></span> <span data-ttu-id="0d9f6-108">这些版本在 [.NET 达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 SLES 版本不再受到支持之前仍受支持。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-108">These versions remain supported until either the version of [.NET reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of SLES is no longer supported.</span></span>

- <span data-ttu-id="0d9f6-109">✔️ 指示 SLES 或 .NET 版本仍受支持。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-109">A ✔️ indicates that the version of SLES or .NET is still supported.</span></span>
- <span data-ttu-id="0d9f6-110">❌ 指示 SLES 或 .NET 版本在该 SLES 版本上不受支持。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-110">A ❌ indicates that the version of SLES or .NET isn't supported on that SLES release.</span></span>
- <span data-ttu-id="0d9f6-111">当 SLES 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-111">When both a version of SLES and a version of .NET have ✔️, that OS and .NET combination is supported.</span></span>

| <span data-ttu-id="0d9f6-112">SLES</span><span class="sxs-lookup"><span data-stu-id="0d9f6-112">SLES</span></span>                   | <span data-ttu-id="0d9f6-113">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-113">.NET Core 2.1</span></span> | <span data-ttu-id="0d9f6-114">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-114">.NET Core 3.1</span></span> | <span data-ttu-id="0d9f6-115">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="0d9f6-115">.NET 5.0</span></span> |
|------------------------|---------------|---------------|----------------|
| <span data-ttu-id="0d9f6-116">✔️ [15](#sles-15-)</span><span class="sxs-lookup"><span data-stu-id="0d9f6-116">✔️ [15](#sles-15-)</span></span>     | <span data-ttu-id="0d9f6-117">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-117">✔️ 2.1</span></span>        | <span data-ttu-id="0d9f6-118">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-118">✔️ 3.1</span></span>        | <span data-ttu-id="0d9f6-119">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="0d9f6-119">✔️ 5.0</span></span> |
| <span data-ttu-id="0d9f6-120">✔️ [12 SP2](#sles-12-)</span><span class="sxs-lookup"><span data-stu-id="0d9f6-120">✔️ [12 SP2](#sles-12-)</span></span> | <span data-ttu-id="0d9f6-121">✔️ 2.1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-121">✔️ 2.1</span></span>        | <span data-ttu-id="0d9f6-122">✔️ 3.1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-122">✔️ 3.1</span></span>        | <span data-ttu-id="0d9f6-123">✔️ 5.0</span><span class="sxs-lookup"><span data-stu-id="0d9f6-123">✔️ 5.0</span></span> |

<span data-ttu-id="0d9f6-124">以下 .NET Core 版本不再受支持。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-124">The following versions of .NET Core are no longer supported.</span></span> <span data-ttu-id="0d9f6-125">这些版本的下载仍保持发布状态：</span><span class="sxs-lookup"><span data-stu-id="0d9f6-125">The downloads for these still remain published:</span></span>

- <span data-ttu-id="0d9f6-126">3.0</span><span class="sxs-lookup"><span data-stu-id="0d9f6-126">3.0</span></span>
- <span data-ttu-id="0d9f6-127">2.2</span><span class="sxs-lookup"><span data-stu-id="0d9f6-127">2.2</span></span>
- <span data-ttu-id="0d9f6-128">2.0</span><span class="sxs-lookup"><span data-stu-id="0d9f6-128">2.0</span></span>

## <a name="remove-preview-versions"></a><span data-ttu-id="0d9f6-129">删除预览版本</span><span class="sxs-lookup"><span data-stu-id="0d9f6-129">Remove preview versions</span></span>

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="sles-15-"></a><span data-ttu-id="0d9f6-130">SLES 15 ✔️</span><span class="sxs-lookup"><span data-stu-id="0d9f6-130">SLES 15 ✔️</span></span>

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm
```

<span data-ttu-id="0d9f6-131">目前，SLES 15 Microsoft 存储库安装包会将 microsoft-prod.repo 文件安装到错误的目录，从而导致 zypper 找不到 .NET 包。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-131">Currently, the SLES 15 Microsoft repository setup package installs the *microsoft-prod.repo* file to the wrong directory, preventing zypper from finding the .NET packages.</span></span> <span data-ttu-id="0d9f6-132">若要解决此问题，请在正确的目录中创建一个符号链接。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-132">To fix this problem, create a symlink in the correct directory.</span></span>

```bash
sudo ln -s /etc/yum.repos.d/microsoft-prod.repo /etc/zypp/repos.d/microsoft-prod.repo
```

[!INCLUDE [linux-zyp-install-50](includes/linux-install-50-zyp.md)]

## <a name="sles-12-"></a><span data-ttu-id="0d9f6-133">SLES 12 ✔️</span><span class="sxs-lookup"><span data-stu-id="0d9f6-133">SLES 12 ✔️</span></span>

<span data-ttu-id="0d9f6-134">SLES 12 系列的 .NET 需要至少为 SP2。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-134">.NET requires SP2 as a minimum for the SLES 12 family.</span></span>

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm
```

[!INCLUDE [linux-zyp-install-50](includes/linux-install-50-zyp.md)]

## <a name="how-to-install-other-versions"></a><span data-ttu-id="0d9f6-135">如何安装其他版本</span><span class="sxs-lookup"><span data-stu-id="0d9f6-135">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="0d9f6-136">包管理器疑难解答</span><span class="sxs-lookup"><span data-stu-id="0d9f6-136">Troubleshoot the package manager</span></span>

<span data-ttu-id="0d9f6-137">本部分提供有关使用包管理器安装 .NET 时可能会遇到的常见错误的信息。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-137">This section provides information on common errors you may get while using the package manager to install .NET.</span></span>

### <a name="failed-to-fetch"></a><span data-ttu-id="0d9f6-138">未能提取</span><span class="sxs-lookup"><span data-stu-id="0d9f6-138">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="dependencies"></a><span data-ttu-id="0d9f6-139">依赖项</span><span class="sxs-lookup"><span data-stu-id="0d9f6-139">Dependencies</span></span>

<span data-ttu-id="0d9f6-140">使用包管理器进行安装时，将为你安装这些库。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-140">When you install with a package manager, these libraries are installed for you.</span></span> <span data-ttu-id="0d9f6-141">但是，如果手动安装 .NET 或发布自包含的应用，则需要确保已安装以下库：</span><span class="sxs-lookup"><span data-stu-id="0d9f6-141">But, if you manually install .NET or you publish a self-contained app, you'll need to make sure these libraries are installed:</span></span>

- <span data-ttu-id="0d9f6-142">krb5</span><span class="sxs-lookup"><span data-stu-id="0d9f6-142">krb5</span></span>
- <span data-ttu-id="0d9f6-143">libicu</span><span class="sxs-lookup"><span data-stu-id="0d9f6-143">libicu</span></span>
- <span data-ttu-id="0d9f6-144">libopenssl1_1</span><span class="sxs-lookup"><span data-stu-id="0d9f6-144">libopenssl1_1</span></span>

<span data-ttu-id="0d9f6-145">如果目标运行时环境的 OpenSSL 版本为1.1 或更高版本，则需要安装 compat-openssl10。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-145">If the target runtime environment's OpenSSL version is 1.1 or newer, you'll need to install **compat-openssl10**.</span></span>

<span data-ttu-id="0d9f6-146">有关依赖项的详细信息，请参阅[独立式 Linux 应用](https://github.com/dotnet/core/blob/main/Documentation/self-contained-linux-apps.md)。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-146">For more information about the dependencies, see [Self-contained Linux apps](https://github.com/dotnet/core/blob/main/Documentation/self-contained-linux-apps.md).</span></span>

<span data-ttu-id="0d9f6-147">对于使用 System.Drawing.Common 程序集的 .NET 应用，还需要以下依赖项：</span><span class="sxs-lookup"><span data-stu-id="0d9f6-147">For .NET apps that use the *System.Drawing.Common* assembly, you'll also need the following dependency:</span></span>

- [<span data-ttu-id="0d9f6-148">libgdiplus（版本 6.0.1 或更高版本）</span><span class="sxs-lookup"><span data-stu-id="0d9f6-148">libgdiplus (version 6.0.1 or later)</span></span>](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > <span data-ttu-id="0d9f6-149">可以通过将 Mono 存储库添加到系统来安装最新版 libgdiplus。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-149">You can install a recent version of *libgdiplus* by adding the Mono repository to your system.</span></span> <span data-ttu-id="0d9f6-150">有关详细信息，请参阅 <https://www.mono-project.com/download/stable/>。</span><span class="sxs-lookup"><span data-stu-id="0d9f6-150">For more information, see <https://www.mono-project.com/download/stable/>.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0d9f6-151">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0d9f6-151">Next steps</span></span>

- [<span data-ttu-id="0d9f6-152">如何为 .NET CLI 启用 Tab 自动补全</span><span class="sxs-lookup"><span data-stu-id="0d9f6-152">How to enable TAB completion for the .NET CLI</span></span>](../tools/enable-tab-autocomplete.md)
- [<span data-ttu-id="0d9f6-153">教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="0d9f6-153">Tutorial: Create a console application with .NET SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)
