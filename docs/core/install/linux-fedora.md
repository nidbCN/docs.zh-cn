---
title: 在 Fedora 上安装 .NET - .NET
description: 演示在 Fedora 上安装 .NET SDK 和 .NET 运行时的各种方式。
author: adegeo
ms.author: adegeo
ms.date: 02/17/2021
ms.openlocfilehash: efaad4788db2200b1a47f9b4ae2414730588c6ae
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255747"
---
# <a name="install-the-net-sdk-or-the-net-runtime-on-fedora"></a><span data-ttu-id="fe4f9-103">在 Fedora 上安装 .NET SDK 或 .NET 运行时</span><span class="sxs-lookup"><span data-stu-id="fe4f9-103">Install the .NET SDK or the .NET Runtime on Fedora</span></span>

<span data-ttu-id="fe4f9-104">.NET 在 Fedora 上受支持，本文就将介绍如何在 Fedora 上安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-104">.NET is supported on Fedora and this article describes how to install .NET on Fedora.</span></span> <span data-ttu-id="fe4f9-105">如果 Fedora 版本不受支持，则该版本不再支持 .NET。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-105">When a Fedora version falls out of support, .NET is no longer supported with that version.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

<span data-ttu-id="fe4f9-106">有关在不使用包管理器的情况下安装 .NET 的详细信息，请参阅以下文章之一：</span><span class="sxs-lookup"><span data-stu-id="fe4f9-106">For more information on installing .NET without a package manager, see one of the following articles:</span></span>

- [<span data-ttu-id="fe4f9-107">通过 Snap 安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-107">Install the .NET SDK or the .NET Runtime with Snap.</span></span>](linux-snap.md)
- [<span data-ttu-id="fe4f9-108">使用脚本安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-108">Install the .NET SDK or the .NET Runtime with a script.</span></span>](linux-scripted-manual.md#scripted-install)
- [<span data-ttu-id="fe4f9-109">手动安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-109">Install the .NET SDK or the .NET Runtime manually.</span></span>](linux-scripted-manual.md#manual-install)

## <a name="install-net-50"></a><span data-ttu-id="fe4f9-110">安装 .NET 5.0</span><span class="sxs-lookup"><span data-stu-id="fe4f9-110">Install .NET 5.0</span></span>

<span data-ttu-id="fe4f9-111">Fedora 的默认包存储库中提供的 .NET 最新版是 .NET 5.0。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-111">The latest version of .NET available in the default package repositories for Fedora is .NET 5.0.</span></span>

[!INCLUDE [linux-dnf-install-50](includes/linux-install-50-dnf.md)]

## <a name="install-net-core-31"></a><span data-ttu-id="fe4f9-112">安装 .NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="fe4f9-112">Install .NET Core 3.1</span></span>

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="supported-distributions"></a><span data-ttu-id="fe4f9-113">支持的发行版</span><span class="sxs-lookup"><span data-stu-id="fe4f9-113">Supported distributions</span></span>

<span data-ttu-id="fe4f9-114">下表列出了当前支持的 .NET 版本以及支持它们的 Fedora 版本。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-114">The following table is a list of currently supported .NET releases and the versions of Fedora they're supported on.</span></span> <span data-ttu-id="fe4f9-115">这些版本在 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)或 [Fedora 版本达到生命周期](https://fedoraproject.org/wiki/End_of_life)之前仍受支持。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-115">These versions remain supported until either the version of [.NET reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of [Fedora reaches end-of-life](https://fedoraproject.org/wiki/End_of_life).</span></span>

- <span data-ttu-id="fe4f9-116">✔️ 指示 Fedora 或 .NET 版本仍受支持。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-116">A ✔️ indicates that the version of Fedora or .NET is still supported.</span></span>
- <span data-ttu-id="fe4f9-117">❌ 指示 Fedora 或 .NET 版本在该 Fedora 版本上不受支持。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-117">A ❌ indicates that the version of Fedora or .NET isn't supported on that Fedora release.</span></span>
- <span data-ttu-id="fe4f9-118">当 Fedora 版本和 .NET 版本都有 ✔️ 时，将支持该 OS 和 .NET 组合。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-118">When both a version of Fedora and a version of .NET have ✔️, that OS and .NET combination is supported.</span></span>

| <span data-ttu-id="fe4f9-119">.NET 版本</span><span class="sxs-lookup"><span data-stu-id="fe4f9-119">.NET Version</span></span>  | <span data-ttu-id="fe4f9-120">Fedora 33 ✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-120">Fedora 33 ✔️</span></span> | <span data-ttu-id="fe4f9-121">32 ✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-121">32 ✔️</span></span> | <span data-ttu-id="fe4f9-122">31 ❌</span><span class="sxs-lookup"><span data-stu-id="fe4f9-122">31 ❌</span></span> | <span data-ttu-id="fe4f9-123">30 ❌</span><span class="sxs-lookup"><span data-stu-id="fe4f9-123">30 ❌</span></span> | <span data-ttu-id="fe4f9-124">29 ❌</span><span class="sxs-lookup"><span data-stu-id="fe4f9-124">29 ❌</span></span> | <span data-ttu-id="fe4f9-125">28 ❌</span><span class="sxs-lookup"><span data-stu-id="fe4f9-125">28 ❌</span></span> | <span data-ttu-id="fe4f9-126">27 ❌</span><span class="sxs-lookup"><span data-stu-id="fe4f9-126">27 ❌</span></span> |
| ------------  | ---------: | --: | --: | --: | --: | --: | --: |
| <span data-ttu-id="fe4f9-127">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="fe4f9-127">.NET 5.0</span></span>      | <span data-ttu-id="fe4f9-128">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-128">✔️</span></span>        | <span data-ttu-id="fe4f9-129">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-129">✔️</span></span> | ❌|❌ |❌ |❌  |❌ |
| <span data-ttu-id="fe4f9-130">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="fe4f9-130">.NET Core 3.1</span></span> | <span data-ttu-id="fe4f9-131">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-131">✔️</span></span>        | <span data-ttu-id="fe4f9-132">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-132">✔️</span></span> | <span data-ttu-id="fe4f9-133">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-133">✔️</span></span>|<span data-ttu-id="fe4f9-134">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-134">✔️</span></span> |<span data-ttu-id="fe4f9-135">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-135">✔️</span></span> |❌  |❌ |
| <span data-ttu-id="fe4f9-136">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="fe4f9-136">.NET Core 2.1</span></span> | <span data-ttu-id="fe4f9-137">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-137">✔️</span></span>        | <span data-ttu-id="fe4f9-138">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-138">✔️</span></span> | <span data-ttu-id="fe4f9-139">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-139">✔️</span></span>|<span data-ttu-id="fe4f9-140">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-140">✔️</span></span> |<span data-ttu-id="fe4f9-141">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-141">✔️</span></span> |<span data-ttu-id="fe4f9-142">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-142">✔️</span></span>  |<span data-ttu-id="fe4f9-143">✔️</span><span class="sxs-lookup"><span data-stu-id="fe4f9-143">✔️</span></span> |

<span data-ttu-id="fe4f9-144">以下 .NET 版本不再受到支持。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-144">The following versions of .NET are no longer supported.</span></span> <span data-ttu-id="fe4f9-145">这些版本的下载仍保持发布状态：</span><span class="sxs-lookup"><span data-stu-id="fe4f9-145">The downloads for these still remain published:</span></span>

- <span data-ttu-id="fe4f9-146">3.0</span><span class="sxs-lookup"><span data-stu-id="fe4f9-146">3.0</span></span>
- <span data-ttu-id="fe4f9-147">2.2</span><span class="sxs-lookup"><span data-stu-id="fe4f9-147">2.2</span></span>
- <span data-ttu-id="fe4f9-148">2.0</span><span class="sxs-lookup"><span data-stu-id="fe4f9-148">2.0</span></span>

## <a name="remove-preview-versions"></a><span data-ttu-id="fe4f9-149">删除预览版本</span><span class="sxs-lookup"><span data-stu-id="fe4f9-149">Remove preview versions</span></span>

[!INCLUDE [package-manager uninstall notice](./includes/linux-uninstall-preview-info.md)]

## <a name="dependencies"></a><span data-ttu-id="fe4f9-150">依赖项</span><span class="sxs-lookup"><span data-stu-id="fe4f9-150">Dependencies</span></span>

[!INCLUDE [linux-rpm-install-dependencies](includes/linux-rpm-install-dependencies.md)]

## <a name="install-on-older-distributions"></a><span data-ttu-id="fe4f9-151">在早期的发行版上进行安装</span><span class="sxs-lookup"><span data-stu-id="fe4f9-151">Install on older distributions</span></span>

<span data-ttu-id="fe4f9-152">Fedora 的早期版本的默认包存储库中并不包含 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-152">Older versions of Fedora don't contain .NET Core in the default package repositories.</span></span> <span data-ttu-id="fe4f9-153">可使用 [snap](linux-snap.md) 通过 [dotnet-install.sh 脚本](linux-scripted-manual.md#scripted-install)安装 .NET，或使用 Microsoft 的存储库安装 .NET：</span><span class="sxs-lookup"><span data-stu-id="fe4f9-153">You can install .NET with [snap](linux-snap.md), through the [_dotnet-install.sh_ script](linux-scripted-manual.md#scripted-install), or use Microsoft's repository to install .NET:</span></span>

01. <span data-ttu-id="fe4f9-154">首先，将 Microsoft 签名密钥添加到受信任的密钥列表。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-154">First, add the Microsoft signing key to your list of trusted keys.</span></span>

    ```bash
    sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
    ```

02. <span data-ttu-id="fe4f9-155">接下来，添加 Microsoft 包存储库。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-155">Next, add the Microsoft package repository.</span></span> <span data-ttu-id="fe4f9-156">存储库的源基于你的 Fedora 版本。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-156">The source of the repository is based on your version of Fedora.</span></span>

    | <span data-ttu-id="fe4f9-157">Fedora 版本</span><span class="sxs-lookup"><span data-stu-id="fe4f9-157">Fedora Version</span></span> | <span data-ttu-id="fe4f9-158">包存储库</span><span class="sxs-lookup"><span data-stu-id="fe4f9-158">Package repository</span></span> |
    | -------------- | ------- |
    | <span data-ttu-id="fe4f9-159">31</span><span class="sxs-lookup"><span data-stu-id="fe4f9-159">31</span></span>             | `https://packages.microsoft.com/config/fedora/31/prod.repo` |
    | <span data-ttu-id="fe4f9-160">30</span><span class="sxs-lookup"><span data-stu-id="fe4f9-160">30</span></span>             | `https://packages.microsoft.com/config/fedora/30/prod.repo` |
    | <span data-ttu-id="fe4f9-161">29</span><span class="sxs-lookup"><span data-stu-id="fe4f9-161">29</span></span>             | `https://packages.microsoft.com/config/fedora/29/prod.repo` |
    | <span data-ttu-id="fe4f9-162">28</span><span class="sxs-lookup"><span data-stu-id="fe4f9-162">28</span></span>             | `https://packages.microsoft.com/config/fedora/28/prod.repo` |
    | <span data-ttu-id="fe4f9-163">27</span><span class="sxs-lookup"><span data-stu-id="fe4f9-163">27</span></span>             | `https://packages.microsoft.com/config/fedora/27/prod.repo` |

    ```bash
    sudo wget -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/31/prod.repo
    ```

[!INCLUDE [linux-dnf-install-31](./includes/linux-install-31-dnf.md)]

## <a name="how-to-install-other-versions"></a><span data-ttu-id="fe4f9-164">如何安装其他版本</span><span class="sxs-lookup"><span data-stu-id="fe4f9-164">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="fe4f9-165">包管理器疑难解答</span><span class="sxs-lookup"><span data-stu-id="fe4f9-165">Troubleshoot the package manager</span></span>

<span data-ttu-id="fe4f9-166">本部分提供有关使用包管理器安装 .NET 或 .NET Core 时可能会遇到的常见错误的信息。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-166">This section provides information on common errors you may get while using the package manager to install .NET or .NET Core.</span></span>

### <a name="unable-to-find-package"></a><span data-ttu-id="fe4f9-167">找不到包</span><span class="sxs-lookup"><span data-stu-id="fe4f9-167">Unable to find package</span></span>

<span data-ttu-id="fe4f9-168">有关在不使用包管理器的情况下安装 .NET 的详细信息，请参阅以下文章之一：</span><span class="sxs-lookup"><span data-stu-id="fe4f9-168">For more information on installing .NET without a package manager, see one of the following articles:</span></span>

- [<span data-ttu-id="fe4f9-169">通过 Snap 安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-169">Install the .NET SDK or the .NET Runtime with Snap.</span></span>](linux-snap.md)
- [<span data-ttu-id="fe4f9-170">使用脚本安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-170">Install the .NET SDK or the .NET Runtime with a script.</span></span>](linux-scripted-manual.md#scripted-install)
- [<span data-ttu-id="fe4f9-171">手动安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="fe4f9-171">Install the .NET SDK or the .NET Runtime manually.</span></span>](linux-scripted-manual.md#manual-install)

### <a name="failed-to-fetch"></a><span data-ttu-id="fe4f9-172">未能提取</span><span class="sxs-lookup"><span data-stu-id="fe4f9-172">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="next-steps"></a><span data-ttu-id="fe4f9-173">后续步骤</span><span class="sxs-lookup"><span data-stu-id="fe4f9-173">Next steps</span></span>

- [<span data-ttu-id="fe4f9-174">如何为 .NET CLI 启用 Tab 自动补全</span><span class="sxs-lookup"><span data-stu-id="fe4f9-174">How to enable TAB completion for the .NET CLI</span></span>](../tools/enable-tab-autocomplete.md)
- [<span data-ttu-id="fe4f9-175">教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="fe4f9-175">Tutorial: Create a console application with .NET SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)
