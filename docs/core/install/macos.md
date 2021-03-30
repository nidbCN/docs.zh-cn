---
title: 在 macOS 上安装 .NET
description: 了解可在其上安装 .NET 的 macOS 版本。
author: adegeo
ms.author: adegeo
ms.date: 11/10/2020
ms.openlocfilehash: 9961d3e3ddb5f38be1c9c13c01af2e41815f50c9
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875247"
---
# <a name="install-net-on-macos"></a><span data-ttu-id="802bb-103">在 macOS 上安装 .NET</span><span class="sxs-lookup"><span data-stu-id="802bb-103">Install .NET on macOS</span></span>

> [!div class="op_single_selector"]
>
> - [在 Windows 上安装](windows.md)
> - [在 macOS 上安装](macos.md)
> - [在 Linux 上安装](linux.md)

<span data-ttu-id="802bb-107">在本文中，你将了解如何在 macOS 上安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="802bb-107">In this article, you'll learn how to install .NET on macOS.</span></span> <span data-ttu-id="802bb-108">.NET 由运行时和 SDK 组成。</span><span class="sxs-lookup"><span data-stu-id="802bb-108">.NET is made up of the runtime and the SDK.</span></span> <span data-ttu-id="802bb-109">运行时用于运行 .NET 应用，应用可能包含也可能不包含它。</span><span class="sxs-lookup"><span data-stu-id="802bb-109">The runtime is used to run a .NET app and may or may not be included with the app.</span></span> <span data-ttu-id="802bb-110">SDK 用于创建 .NET 应用和库。</span><span class="sxs-lookup"><span data-stu-id="802bb-110">The SDK is used to create .NET apps and libraries.</span></span> <span data-ttu-id="802bb-111">.NET 运行时始终随 SDK 一起安装。</span><span class="sxs-lookup"><span data-stu-id="802bb-111">The .NET runtime is always installed with the SDK.</span></span>

<span data-ttu-id="802bb-112">最新版本的 .NET 是 5.0。</span><span class="sxs-lookup"><span data-stu-id="802bb-112">The latest version of .NET is 5.0.</span></span>

> [!div class="button"]
> [<span data-ttu-id="802bb-113">下载 .NET Core</span><span class="sxs-lookup"><span data-stu-id="802bb-113">Download .NET Core</span></span>](https://dotnet.microsoft.com/download/dotnet)

## <a name="supported-releases"></a><span data-ttu-id="802bb-114">支持的版本</span><span class="sxs-lookup"><span data-stu-id="802bb-114">Supported releases</span></span>

<span data-ttu-id="802bb-115">下表列出了当前支持的 .NET 版本以及支持它们的 macOS 版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-115">The following table is a list of currently supported .NET releases and the versions of macOS they're supported on.</span></span> <span data-ttu-id="802bb-116">这些版本仍受支持，除非任一 [.NET 版本达到支持终止日期](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。</span><span class="sxs-lookup"><span data-stu-id="802bb-116">These versions remain supported either the version of [.NET reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

- <span data-ttu-id="802bb-117">✔️ 指示 .NET Core 版本仍受支持。</span><span class="sxs-lookup"><span data-stu-id="802bb-117">A ✔️ indicates that the version of .NET Core is still supported.</span></span>
- <span data-ttu-id="802bb-118">❌ 指示 .NET Core 版本不受支持。</span><span class="sxs-lookup"><span data-stu-id="802bb-118">A ❌ indicates that the version of .NET Core isn't supported.</span></span>

| <span data-ttu-id="802bb-119">操作系统</span><span class="sxs-lookup"><span data-stu-id="802bb-119">Operating System</span></span>          | <span data-ttu-id="802bb-120">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="802bb-120">.NET Core 2.1</span></span> | <span data-ttu-id="802bb-121">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="802bb-121">.NET Core 3.1</span></span> | <span data-ttu-id="802bb-122">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="802bb-122">.NET 5.0</span></span> |
|---------------------------|---------------|---------------|----------------|
| <span data-ttu-id="802bb-123">macOS 11.0“Big Sur”</span><span class="sxs-lookup"><span data-stu-id="802bb-123">macOS 11.0 "Big Sur"</span></span>        | <span data-ttu-id="802bb-124">✔️ 2.1（[发行说明][release-notes-21]）</span><span class="sxs-lookup"><span data-stu-id="802bb-124">✔️ 2.1 ([Release notes][release-notes-21])</span></span> | <span data-ttu-id="802bb-125">✔️ 3.1（[发行说明][release-notes-31]）</span><span class="sxs-lookup"><span data-stu-id="802bb-125">✔️ 3.1 ([Release notes][release-notes-31])</span></span> | <span data-ttu-id="802bb-126">✔️ 5.0（[发行说明][release-notes-50]）</span><span class="sxs-lookup"><span data-stu-id="802bb-126">✔️ 5.0 ([Release notes][release-notes-50])</span></span> |
| <span data-ttu-id="802bb-127">macOS 10.15“Catalina”</span><span class="sxs-lookup"><span data-stu-id="802bb-127">macOS 10.15 "Catalina"</span></span>    | <span data-ttu-id="802bb-128">✔️ 2.1（[发行说明][release-notes-21]）</span><span class="sxs-lookup"><span data-stu-id="802bb-128">✔️ 2.1 ([Release notes][release-notes-21])</span></span> | <span data-ttu-id="802bb-129">✔️ 3.1（[发行说明][release-notes-31]）</span><span class="sxs-lookup"><span data-stu-id="802bb-129">✔️ 3.1 ([Release notes][release-notes-31])</span></span> | <span data-ttu-id="802bb-130">✔️ 5.0（[发行说明][release-notes-50]）</span><span class="sxs-lookup"><span data-stu-id="802bb-130">✔️ 5.0 ([Release notes][release-notes-50])</span></span> |
| <span data-ttu-id="802bb-131">macOS 10.14“Mojave”</span><span class="sxs-lookup"><span data-stu-id="802bb-131">macOS 10.14 "Mojave"</span></span>      | <span data-ttu-id="802bb-132">✔️ 2.1（[发行说明][release-notes-21]）</span><span class="sxs-lookup"><span data-stu-id="802bb-132">✔️ 2.1 ([Release notes][release-notes-21])</span></span> | <span data-ttu-id="802bb-133">✔️ 3.1（[发行说明][release-notes-31]）</span><span class="sxs-lookup"><span data-stu-id="802bb-133">✔️ 3.1 ([Release notes][release-notes-31])</span></span> | <span data-ttu-id="802bb-134">✔️ 5.0（[发行说明][release-notes-50]）</span><span class="sxs-lookup"><span data-stu-id="802bb-134">✔️ 5.0 ([Release notes][release-notes-50])</span></span> |
| <span data-ttu-id="802bb-135">macOS 10.13“High Sierra”</span><span class="sxs-lookup"><span data-stu-id="802bb-135">macOS 10.13 "High Sierra"</span></span> | <span data-ttu-id="802bb-136">✔️ 2.1（[发行说明][release-notes-21]）</span><span class="sxs-lookup"><span data-stu-id="802bb-136">✔️ 2.1 ([Release notes][release-notes-21])</span></span> | <span data-ttu-id="802bb-137">✔️ 3.1（[发行说明][release-notes-31]）</span><span class="sxs-lookup"><span data-stu-id="802bb-137">✔️ 3.1 ([Release notes][release-notes-31])</span></span> | <span data-ttu-id="802bb-138">✔️ 5.0（[发行说明][release-notes-50]）</span><span class="sxs-lookup"><span data-stu-id="802bb-138">✔️ 5.0 ([Release notes][release-notes-50])</span></span> |
| <span data-ttu-id="802bb-139">macOS 10.12“Sierra”</span><span class="sxs-lookup"><span data-stu-id="802bb-139">macOS 10.12 "Sierra"</span></span>      | <span data-ttu-id="802bb-140">✔️ 2.1（[发行说明][release-notes-21]）</span><span class="sxs-lookup"><span data-stu-id="802bb-140">✔️ 2.1 ([Release notes][release-notes-21])</span></span> | <span data-ttu-id="802bb-141">❌ 3.1（[发行说明][release-notes-31]）</span><span class="sxs-lookup"><span data-stu-id="802bb-141">❌ 3.1 ([Release notes][release-notes-31])</span></span> | <span data-ttu-id="802bb-142">❌ 5.0（[发行说明][release-notes-50]）</span><span class="sxs-lookup"><span data-stu-id="802bb-142">❌ 5.0 ([Release notes][release-notes-50])</span></span> |

## <a name="unsupported-releases"></a><span data-ttu-id="802bb-143">不支持的版本</span><span class="sxs-lookup"><span data-stu-id="802bb-143">Unsupported releases</span></span>

<span data-ttu-id="802bb-144">以下 .NET 版本 ❌ 不再受到支持。</span><span class="sxs-lookup"><span data-stu-id="802bb-144">The following versions of .NET are ❌ no longer supported.</span></span> <span data-ttu-id="802bb-145">这些版本的下载仍保持发布状态：</span><span class="sxs-lookup"><span data-stu-id="802bb-145">The downloads for these still remain published:</span></span>

- <span data-ttu-id="802bb-146">3.0（[发行说明][release-notes-30]）</span><span class="sxs-lookup"><span data-stu-id="802bb-146">3.0 ([Release notes][release-notes-30])</span></span>
- <span data-ttu-id="802bb-147">2.2（[发行说明][release-notes-22]）</span><span class="sxs-lookup"><span data-stu-id="802bb-147">2.2 ([Release notes][release-notes-22])</span></span>
- <span data-ttu-id="802bb-148">2.0（[发行说明][release-notes-20]）</span><span class="sxs-lookup"><span data-stu-id="802bb-148">2.0 ([Release notes][release-notes-20])</span></span>

## <a name="runtime-information"></a><span data-ttu-id="802bb-149">运行时信息</span><span class="sxs-lookup"><span data-stu-id="802bb-149">Runtime information</span></span>

<span data-ttu-id="802bb-150">运行时用于运行使用 .NET 创建的应用。</span><span class="sxs-lookup"><span data-stu-id="802bb-150">The runtime is used to run apps created with .NET.</span></span> <span data-ttu-id="802bb-151">应用作者发布应用时，可以在其应用中包含运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-151">When an app author publishes an app, they can include the runtime with their app.</span></span> <span data-ttu-id="802bb-152">如果作者未包含运行时，则由用户安装运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-152">If they don't include the runtime, it's up to the user to install the runtime.</span></span>

<span data-ttu-id="802bb-153">macOS 上可以安装两种不同的运行时：</span><span class="sxs-lookup"><span data-stu-id="802bb-153">There are two different runtimes you can install on macOS:</span></span>

- <span data-ttu-id="802bb-154">*ASP.NET Core 运行时*</span><span class="sxs-lookup"><span data-stu-id="802bb-154">*ASP.NET Core runtime*</span></span>\
  <span data-ttu-id="802bb-155">运行 ASP.NET Core 应用。</span><span class="sxs-lookup"><span data-stu-id="802bb-155">Runs ASP.NET Core apps.</span></span> <span data-ttu-id="802bb-156">包括 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-156">Includes the .NET runtime.</span></span>

- <span data-ttu-id="802bb-157">.NET 运行时</span><span class="sxs-lookup"><span data-stu-id="802bb-157">*.NET runtime*</span></span>\
  <span data-ttu-id="802bb-158">此运行时是最简单的运行时，不包括任何其他运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-158">This runtime is the simplest runtime and doesn't include any other runtime.</span></span> <span data-ttu-id="802bb-159">强烈建议安装 ASP.NET Core 运行时，以最大限度地提升与 .NET 应用的兼容性。</span><span class="sxs-lookup"><span data-stu-id="802bb-159">It's highly recommended that you install *ASP.NET Core runtime* for the best compatibility with .NET apps.</span></span>

> [!div class="button"]
> [<span data-ttu-id="802bb-160">下载 .NET 运行时</span><span class="sxs-lookup"><span data-stu-id="802bb-160">Download .NET Runtime</span></span>](https://dotnet.microsoft.com/download/dotnet)

## <a name="sdk-information"></a><span data-ttu-id="802bb-161">SDK 信息</span><span class="sxs-lookup"><span data-stu-id="802bb-161">SDK information</span></span>

<span data-ttu-id="802bb-162">SDK 用于生成和发布 .NET 应用和库。</span><span class="sxs-lookup"><span data-stu-id="802bb-162">The SDK is used to build and publish .NET apps and libraries.</span></span> <span data-ttu-id="802bb-163">安装 SDK 会包含两个[运行时](#runtime-information)：ASP.NET Core 和 .NET。</span><span class="sxs-lookup"><span data-stu-id="802bb-163">Installing the SDK includes both [runtimes](#runtime-information): ASP.NET Core and .NET.</span></span>

## <a name="dependencies"></a><span data-ttu-id="802bb-164">依赖项</span><span class="sxs-lookup"><span data-stu-id="802bb-164">Dependencies</span></span>

<span data-ttu-id="802bb-165">以下 macOS 版本支持 .NET：</span><span class="sxs-lookup"><span data-stu-id="802bb-165">.NET is supported on the following macOS releases:</span></span>

> [!NOTE]
> <span data-ttu-id="802bb-166">`+` 表示最低版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-166">A `+` symbol represents the minimum version.</span></span>

| <span data-ttu-id="802bb-167">.NET Core 版本</span><span class="sxs-lookup"><span data-stu-id="802bb-167">.NET Core Version</span></span> | <span data-ttu-id="802bb-168">macOS</span><span class="sxs-lookup"><span data-stu-id="802bb-168">macOS</span></span>                 | <span data-ttu-id="802bb-169">体系结构</span><span class="sxs-lookup"><span data-stu-id="802bb-169">Architectures</span></span> | <span data-ttu-id="802bb-170">详细信息</span><span class="sxs-lookup"><span data-stu-id="802bb-170">More information</span></span>    |
| ----------------- | --------------------- | --------------| --- |
| <span data-ttu-id="802bb-171">5.0</span><span class="sxs-lookup"><span data-stu-id="802bb-171">5.0</span></span>               | <span data-ttu-id="802bb-172">High Sierra (10.13+)</span><span class="sxs-lookup"><span data-stu-id="802bb-172">High Sierra (10.13+)</span></span>  | <span data-ttu-id="802bb-173">X64</span><span class="sxs-lookup"><span data-stu-id="802bb-173">x64</span></span> | [<span data-ttu-id="802bb-174">详细信息</span><span class="sxs-lookup"><span data-stu-id="802bb-174">More information</span></span>](https://github.com/dotnet/core/blob/main/release-notes/5.0/5.0-supported-os.md) |
| <span data-ttu-id="802bb-175">3.1</span><span class="sxs-lookup"><span data-stu-id="802bb-175">3.1</span></span>               | <span data-ttu-id="802bb-176">High Sierra (10.13+)</span><span class="sxs-lookup"><span data-stu-id="802bb-176">High Sierra (10.13+)</span></span>  | <span data-ttu-id="802bb-177">X64</span><span class="sxs-lookup"><span data-stu-id="802bb-177">x64</span></span> | [<span data-ttu-id="802bb-178">详细信息</span><span class="sxs-lookup"><span data-stu-id="802bb-178">More information</span></span>](https://github.com/dotnet/core/blob/main/release-notes/3.1/3.1-supported-os.md) |
| <span data-ttu-id="802bb-179">3.0</span><span class="sxs-lookup"><span data-stu-id="802bb-179">3.0</span></span>               | <span data-ttu-id="802bb-180">High Sierra (10.13+)</span><span class="sxs-lookup"><span data-stu-id="802bb-180">High Sierra (10.13+)</span></span>  | <span data-ttu-id="802bb-181">X64</span><span class="sxs-lookup"><span data-stu-id="802bb-181">x64</span></span> | [<span data-ttu-id="802bb-182">详细信息</span><span class="sxs-lookup"><span data-stu-id="802bb-182">More information</span></span>](https://github.com/dotnet/core/blob/main/release-notes/3.0/3.0-supported-os.md) |
| <span data-ttu-id="802bb-183">2.2</span><span class="sxs-lookup"><span data-stu-id="802bb-183">2.2</span></span>               | <span data-ttu-id="802bb-184">Sierra (10.12+)</span><span class="sxs-lookup"><span data-stu-id="802bb-184">Sierra (10.12+)</span></span>       | <span data-ttu-id="802bb-185">X64</span><span class="sxs-lookup"><span data-stu-id="802bb-185">x64</span></span> | [<span data-ttu-id="802bb-186">详细信息</span><span class="sxs-lookup"><span data-stu-id="802bb-186">More information</span></span>](https://github.com/dotnet/core/blob/main/release-notes/2.2/2.2-supported-os.md) |
| <span data-ttu-id="802bb-187">2.1</span><span class="sxs-lookup"><span data-stu-id="802bb-187">2.1</span></span>               | <span data-ttu-id="802bb-188">Sierra (10.12+)</span><span class="sxs-lookup"><span data-stu-id="802bb-188">Sierra (10.12+)</span></span>       | <span data-ttu-id="802bb-189">X64</span><span class="sxs-lookup"><span data-stu-id="802bb-189">x64</span></span> | [<span data-ttu-id="802bb-190">详细信息</span><span class="sxs-lookup"><span data-stu-id="802bb-190">More information</span></span>](https://github.com/dotnet/core/blob/main/release-notes/2.1/2.1-supported-os.md) |

<span data-ttu-id="802bb-191">自 macOS Catalina（版本10.15）开始，所有在 2019 年 6 月 1 日之后生成并使用开发者 ID 扩散的软件都必须经过公证。</span><span class="sxs-lookup"><span data-stu-id="802bb-191">Beginning with macOS Catalina (version 10.15), all software built after June 1, 2019 that is distributed with Developer ID, must be notarized.</span></span> <span data-ttu-id="802bb-192">此要求适用于 .NET 运行时、.NET SDK 以及使用 .NET 创建的软件。</span><span class="sxs-lookup"><span data-stu-id="802bb-192">This requirement applies to the .NET runtime, .NET SDK, and software created with .NET.</span></span>

<span data-ttu-id="802bb-193">自 2020 年 2 月 18 日起，.NET 5.0 和 .NET Core 3.1、3.0 和 2.1 的运行时和 SDK 安装程序都已经过公证。</span><span class="sxs-lookup"><span data-stu-id="802bb-193">The runtime and SDK installers for .NET 5.0 and .NET Core 3.1, 3.0, and 2.1, have been notarized since February 18, 2020.</span></span> <span data-ttu-id="802bb-194">以前发布的版本没有经过公证。</span><span class="sxs-lookup"><span data-stu-id="802bb-194">Prior released versions aren't notarized.</span></span> <span data-ttu-id="802bb-195">如果运行未经过公证的应用，将看到类似于下图的错误：</span><span class="sxs-lookup"><span data-stu-id="802bb-195">If you run a non-notarized app, you'll see an error similar to the following image:</span></span>

![macOS Catalina 公证警报](media/dependencies/macos-notarized-pkg-warning.png)

<span data-ttu-id="802bb-197">若要详细了解强制执行的公证要求对 .NET 和 .NET 应用的影响，请参阅[处理 macOS Catalina 公证](macos-notarization-issues.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-197">For more information about how enforced-notarization affects .NET (and your .NET apps), see [Working with macOS Catalina Notarization](macos-notarization-issues.md).</span></span>

## <a name="libgdiplus"></a><span data-ttu-id="802bb-198">libgdiplus</span><span class="sxs-lookup"><span data-stu-id="802bb-198">libgdiplus</span></span>

<span data-ttu-id="802bb-199">使用 System.Drawing.Common 程序集的 .NET 应用程序要求安装 libgdiplus。</span><span class="sxs-lookup"><span data-stu-id="802bb-199">.NET applications that use the *System.Drawing.Common* assembly require libgdiplus to be installed.</span></span>

<span data-ttu-id="802bb-200">获取 libgdiplus 的一个简单方法是使用适用于 macOS 的 [Homebrew (“brew”)](https://brew.sh/) 包。</span><span class="sxs-lookup"><span data-stu-id="802bb-200">An easy way to obtain libgdiplus is by using the [Homebrew ("brew")](https://brew.sh/) package manager for macOS.</span></span> <span data-ttu-id="802bb-201">在安装 brew 后，通过在终端（命令）提示符处执行以下命令来安装 libgdiplus：</span><span class="sxs-lookup"><span data-stu-id="802bb-201">After installing *brew*, install libgdiplus by executing the following commands at a Terminal (command) prompt:</span></span>

```console
brew update
brew install mono-libgdiplus
```

## <a name="install-with-an-installer"></a><span data-ttu-id="802bb-202">使用安装程序安装</span><span class="sxs-lookup"><span data-stu-id="802bb-202">Install with an installer</span></span>

<span data-ttu-id="802bb-203">macOS 具有独立的安装程序，可用于安装 .NET 5.0 SDK：</span><span class="sxs-lookup"><span data-stu-id="802bb-203">macOS has standalone installers that can be used to install the .NET 5.0 SDK:</span></span>

- [<span data-ttu-id="802bb-204">x64（64 位）CPU</span><span class="sxs-lookup"><span data-stu-id="802bb-204">x64 (64-bit) CPUs</span></span>](https://dotnet.microsoft.com/download/dotnet/5.0)

## <a name="download-and-manually-install"></a><span data-ttu-id="802bb-205">下载并手动安装</span><span class="sxs-lookup"><span data-stu-id="802bb-205">Download and manually install</span></span>

<!-- Note, this content is taken from includes/linux-install-manual.md but changed for macOS. Any fixes should be applied there too, though content may be different -->

<span data-ttu-id="802bb-206">除了使用适用于 .NET 的 macOS 安装程序，还可以下载并手动安装 SDK 和运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-206">As an alternative to the macOS installers for .NET, you can download and manually install the SDK and runtime.</span></span> <span data-ttu-id="802bb-207">手动安装通常作为持续集成测试的一部分执行。</span><span class="sxs-lookup"><span data-stu-id="802bb-207">Manual install is usually performed as part of continuous integration testing.</span></span> <span data-ttu-id="802bb-208">对于开发人员或用户，一般使用[安装程序](https://dotnet.microsoft.com/download/dotnet)会更好。</span><span class="sxs-lookup"><span data-stu-id="802bb-208">For a developer or user, it's generally better to use an [installer](https://dotnet.microsoft.com/download/dotnet).</span></span>

<span data-ttu-id="802bb-209">如果安装 .NET SDK，则无需安装相应的运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-209">If you install .NET SDK, you don't need to install the corresponding runtime.</span></span> <span data-ttu-id="802bb-210">首先，从以下站点之一下载 SDK 或运行时的二进制版本：</span><span class="sxs-lookup"><span data-stu-id="802bb-210">First, download a **binary** release for either the SDK or the runtime from one of the following sites:</span></span>

- <span data-ttu-id="802bb-211">✔️ [.NET 5.0 下载](https://dotnet.microsoft.com/download/dotnet/5.0)</span><span class="sxs-lookup"><span data-stu-id="802bb-211">✔️ [.NET 5.0 downloads](https://dotnet.microsoft.com/download/dotnet/5.0)</span></span>
- <span data-ttu-id="802bb-212">✔️ [.NET Core 3.1 下载](https://dotnet.microsoft.com/download/dotnet/3.1)</span><span class="sxs-lookup"><span data-stu-id="802bb-212">✔️ [.NET Core 3.1 downloads](https://dotnet.microsoft.com/download/dotnet/3.1)</span></span>
- <span data-ttu-id="802bb-213">✔️ [.NET Core 2.1 下载](https://dotnet.microsoft.com/download/dotnet/2.1)</span><span class="sxs-lookup"><span data-stu-id="802bb-213">✔️ [.NET Core 2.1 downloads](https://dotnet.microsoft.com/download/dotnet/2.1)</span></span>
- [<span data-ttu-id="802bb-214">所有 .NET Core 下载项</span><span class="sxs-lookup"><span data-stu-id="802bb-214">All .NET Core downloads</span></span>](https://dotnet.microsoft.com/download/dotnet)

<span data-ttu-id="802bb-215">接下来，提取已下载的文件并使用 `export` 命令设置 .NET 使用的变量，然后确保 .NET 在 PATH 中。</span><span class="sxs-lookup"><span data-stu-id="802bb-215">Next, extract the downloaded file and use the `export` command to set variables used by .NET and then ensure .NET is in PATH.</span></span>

<span data-ttu-id="802bb-216">若要提取运行时并使 .NET CLI 命令可用于终端，请先下载 .NET 二进制版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-216">To extract the runtime and make the .NET CLI commands available at the terminal, first download a .NET binary release.</span></span> <span data-ttu-id="802bb-217">然后，打开终端并从保存文件的目录运行以下命令。</span><span class="sxs-lookup"><span data-stu-id="802bb-217">Then, open a terminal and run the following commands from the directory where the file was saved.</span></span> <span data-ttu-id="802bb-218">根据下载内容，存档文件名称可能不同。</span><span class="sxs-lookup"><span data-stu-id="802bb-218">The archive file name may be different depending on what you downloaded.</span></span>

<span data-ttu-id="802bb-219">使用以下命令提取已下载的运行时或 SDK。</span><span class="sxs-lookup"><span data-stu-id="802bb-219">**Use the following commands to extract the runtime or SDK that you downloaded.**</span></span> <span data-ttu-id="802bb-220">请记得将 `DOTNET_FILE` 值更改为你的文件名：</span><span class="sxs-lookup"><span data-stu-id="802bb-220">Remember to change the `DOTNET_FILE` value to your file name:</span></span>

```bash
DOTNET_FILE=dotnet-sdk-5.0.102-linux-x64.tar.gz
export DOTNET_ROOT=$HOME/dotnet

mkdir -p "$DOTNET_ROOT" && tar zxf "$DOTNET_FILE" -C "$DOTNET_ROOT"

export PATH=$PATH:$DOTNET_ROOT
```

> [!TIP]
> <span data-ttu-id="802bb-221">前面的 `export` 命令只会使 .NET CLI 命令对运行它的终端会话可用。</span><span class="sxs-lookup"><span data-stu-id="802bb-221">The preceding `export` commands only make the .NET CLI commands available for the terminal session in which it was run.</span></span>
>
> <span data-ttu-id="802bb-222">你可以编辑 shell 配置文件，永久地添加这些命令。</span><span class="sxs-lookup"><span data-stu-id="802bb-222">You can edit your shell profile to permanently add the commands.</span></span> <span data-ttu-id="802bb-223">Linux 提供了许多不同的 shell，每个都有不同的配置文件。</span><span class="sxs-lookup"><span data-stu-id="802bb-223">There are a number of different shells available for Linux and each has a different profile.</span></span> <span data-ttu-id="802bb-224">例如：</span><span class="sxs-lookup"><span data-stu-id="802bb-224">For example:</span></span>
>
> - <span data-ttu-id="802bb-225">**Bash Shell**：~/.bash_profile、~/.bashrc</span><span class="sxs-lookup"><span data-stu-id="802bb-225">**Bash Shell**: *~/.bash_profile*, *~/.bashrc*</span></span>
> - <span data-ttu-id="802bb-226">**Korn Shell**：~/.kshrc 或 .profile</span><span class="sxs-lookup"><span data-stu-id="802bb-226">**Korn Shell**: *~/.kshrc* or *.profile*</span></span>
> - <span data-ttu-id="802bb-227">**Z Shell**：~/.zshrc 或 .zprofile</span><span class="sxs-lookup"><span data-stu-id="802bb-227">**Z Shell**: *~/.zshrc* or *.zprofile*</span></span>
>
> <span data-ttu-id="802bb-228">为 shell 编辑相应的源文件，并将 `:$HOME/dotnet` 添加到现有 `PATH` 语句的末尾。</span><span class="sxs-lookup"><span data-stu-id="802bb-228">Edit the appropriate source file for your shell and add `:$HOME/dotnet` to the end of the existing `PATH` statement.</span></span> <span data-ttu-id="802bb-229">如果不包含 `PATH` 语句，则使用 `export PATH=$PATH:$HOME/dotnet` 添加新行。</span><span class="sxs-lookup"><span data-stu-id="802bb-229">If no `PATH` statement is included, add a new line with `export PATH=$PATH:$HOME/dotnet`.</span></span>
>
> <span data-ttu-id="802bb-230">另外，将 `export DOTNET_ROOT=$HOME/dotnet` 添加至文件的末尾。</span><span class="sxs-lookup"><span data-stu-id="802bb-230">Also, add `export DOTNET_ROOT=$HOME/dotnet` to the end of the file.</span></span>

<span data-ttu-id="802bb-231">使用此方法可以将不同的版本安装到不同的位置，并明确选择应用程序要使用的对应版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-231">This approach lets you install different versions into separate locations and choose explicitly which one to use by which application.</span></span>

## <a name="install-with-visual-studio-for-mac"></a><span data-ttu-id="802bb-232">使用 Visual Studio for Mac 安装</span><span class="sxs-lookup"><span data-stu-id="802bb-232">Install with Visual Studio for Mac</span></span>

<span data-ttu-id="802bb-233">选定 .NET 工作负载后，可使用 Visual Studio for Mac 安装 .NET SDK。</span><span class="sxs-lookup"><span data-stu-id="802bb-233">Visual Studio for Mac installs the .NET SDK when the **.NET** workload is selected.</span></span> <span data-ttu-id="802bb-234">若要开始在 macOS 上进行 .NET 开发，请参阅[安装 Visual Studio 2019 for Mac](/visualstudio/mac/installation)。</span><span class="sxs-lookup"><span data-stu-id="802bb-234">To get started with .NET development on macOS, see [Install Visual Studio 2019 for Mac](/visualstudio/mac/installation).</span></span>

| <span data-ttu-id="802bb-235">.NET SDK 版本</span><span class="sxs-lookup"><span data-stu-id="802bb-235">.NET SDK version</span></span>      | <span data-ttu-id="802bb-236">Visual Studio 版本</span><span class="sxs-lookup"><span data-stu-id="802bb-236">Visual Studio version</span></span>                      |
| --------------------- | ------------------------------------------ |
| <span data-ttu-id="802bb-237">5.0</span><span class="sxs-lookup"><span data-stu-id="802bb-237">5.0</span></span>                   | <span data-ttu-id="802bb-238">Visual Studio 2019 for Mac 版本 8.8 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-238">Visual Studio 2019 for Mac version 8.8 or higher.</span></span> |
| <span data-ttu-id="802bb-239">3.1</span><span class="sxs-lookup"><span data-stu-id="802bb-239">3.1</span></span>                   | <span data-ttu-id="802bb-240">Visual Studio 2019 for Mac 版本 8.4 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-240">Visual Studio 2019 for Mac version 8.4 or higher.</span></span> |
| <span data-ttu-id="802bb-241">2.1</span><span class="sxs-lookup"><span data-stu-id="802bb-241">2.1</span></span>                   | <span data-ttu-id="802bb-242">Visual Studio 2019 for Mac 版本 8.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-242">Visual Studio 2019 for Mac version 8.0 or higher.</span></span> |

<span data-ttu-id="802bb-243">[![具有 .NET 工作负载功能的 macOS Visual Studio 2019 for Mac](media/install-sdk/mac-install-selection.png)](media/install-sdk/mac-install-selection.png#lightbox)</span><span class="sxs-lookup"><span data-stu-id="802bb-243">[![macOS Visual Studio 2019 for Mac with .NET workload feature](media/install-sdk/mac-install-selection.png)](media/install-sdk/mac-install-selection.png#lightbox)</span></span>

## <a name="install-alongside-visual-studio-code"></a><span data-ttu-id="802bb-244">随 Visual Studio Code 一起安装</span><span class="sxs-lookup"><span data-stu-id="802bb-244">Install alongside Visual Studio Code</span></span>

<span data-ttu-id="802bb-245">Visual Studio Code 是一个功能强大的轻量级源代码编辑器，可在桌面上运行。</span><span class="sxs-lookup"><span data-stu-id="802bb-245">Visual Studio Code is a powerful and lightweight source code editor that runs on your desktop.</span></span> <span data-ttu-id="802bb-246">Visual Studio Code 适用于 Windows、macOS 和 Linux。</span><span class="sxs-lookup"><span data-stu-id="802bb-246">Visual Studio Code is available for Windows, macOS, and Linux.</span></span>

<span data-ttu-id="802bb-247">虽然 Visual Studio Code 不像 Visual Studio 一样附带自动的 .NET 安装程序，但添加 .NET 支持非常简单。</span><span class="sxs-lookup"><span data-stu-id="802bb-247">While Visual Studio Code doesn't come with an automated .NET installer like Visual Studio does, adding .NET support is simple.</span></span>

01. <span data-ttu-id="802bb-248">[下载并安装 Visual Studio Code](https://code.visualstudio.com/Download)。</span><span class="sxs-lookup"><span data-stu-id="802bb-248">[Download and install Visual Studio Code](https://code.visualstudio.com/Download).</span></span>
01. <span data-ttu-id="802bb-249">[下载并安装 .NET SDK](https://dotnet.microsoft.com/download/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="802bb-249">[Download and install the .NET SDK](https://dotnet.microsoft.com/download/dotnet).</span></span>
01. <span data-ttu-id="802bb-250">[从 Visual Studio Code 市场安装 C# 扩展](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)。</span><span class="sxs-lookup"><span data-stu-id="802bb-250">[Install the C# extension from the Visual Studio Code marketplace](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp).</span></span>

## <a name="install-with-bash-automation"></a><span data-ttu-id="802bb-251">使用 Bash 自动化安装</span><span class="sxs-lookup"><span data-stu-id="802bb-251">Install with bash automation</span></span>

<span data-ttu-id="802bb-252">[dotnet-install 脚本](../tools/dotnet-install-script.md)用于运行时的自动化和非管理员安装。</span><span class="sxs-lookup"><span data-stu-id="802bb-252">The [dotnet-install scripts](../tools/dotnet-install-script.md) are used for automation and non-admin installs of the runtime.</span></span> <span data-ttu-id="802bb-253">可从 [dotnet-install 脚本引用页](../tools/dotnet-install-script.md)下载该脚本。</span><span class="sxs-lookup"><span data-stu-id="802bb-253">You can download the script from the [dotnet-install script reference page](../tools/dotnet-install-script.md).</span></span>

<span data-ttu-id="802bb-254">此脚本默认安装最新的[长期支持 (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) 版本，即 .NET Core 3.1。</span><span class="sxs-lookup"><span data-stu-id="802bb-254">The script defaults to installing the latest [long term support (LTS)](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) version, which is .NET Core 3.1.</span></span> <span data-ttu-id="802bb-255">可通过指定 `current` 开关以选择特定版本。</span><span class="sxs-lookup"><span data-stu-id="802bb-255">You can choose a specific release by specifying the `current` switch.</span></span> <span data-ttu-id="802bb-256">包括 `runtime` 开关以安装运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-256">Include the `runtime` switch to install a runtime.</span></span> <span data-ttu-id="802bb-257">否则，该脚本安装 [SDK](./windows.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-257">Otherwise, the script installs the [SDK](./windows.md).</span></span>

```bash
./dotnet-install.sh --channel 5.0 --runtime aspnetcore
```

> [!NOTE]
> <span data-ttu-id="802bb-258">可以使用前面的命令安装 ASP.NET Core 运行时，以实现最大的兼容性。</span><span class="sxs-lookup"><span data-stu-id="802bb-258">The previous command installs the ASP.NET Core runtime for maximum compatability.</span></span> <span data-ttu-id="802bb-259">ASP.NET Core 运行时还包括标准 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="802bb-259">The ASP.NET Core runtime also includes the standard .NET runtime.</span></span>

## <a name="docker"></a><span data-ttu-id="802bb-260">Docker</span><span class="sxs-lookup"><span data-stu-id="802bb-260">Docker</span></span>

<span data-ttu-id="802bb-261">容器提供了一种将应用程序与主机系统的其余部分隔离的轻量级方法。</span><span class="sxs-lookup"><span data-stu-id="802bb-261">Containers provide a lightweight way to isolate your application from the rest of the host system.</span></span> <span data-ttu-id="802bb-262">同一计算机上的容器只共享内核，并使用为应用程序提供的资源。</span><span class="sxs-lookup"><span data-stu-id="802bb-262">Containers on the same machine share just the kernel and use resources given to your application.</span></span>

<span data-ttu-id="802bb-263">.NET 可在 Docker 容器中运行。</span><span class="sxs-lookup"><span data-stu-id="802bb-263">.NET can run in a Docker container.</span></span> <span data-ttu-id="802bb-264">官方 .NET Docker 映像发布到 Microsoft 容器注册表 (MCR)，用户可在 [Microsoft.NET Core Docker Hub 存储库](https://hub.docker.com/_/microsoft-dotnet/)中找到这些映像。</span><span class="sxs-lookup"><span data-stu-id="802bb-264">Official .NET Docker images are published to the Microsoft Container Registry (MCR) and are discoverable at the [Microsoft .NET Core Docker Hub repository](https://hub.docker.com/_/microsoft-dotnet/).</span></span> <span data-ttu-id="802bb-265">每个存储库包含 .NET（SDK 或运行时）和可以使用的操作系统的不同组合的映像。</span><span class="sxs-lookup"><span data-stu-id="802bb-265">Each repository contains images for different combinations of the .NET (SDK or Runtime) and OS that you can use.</span></span>

<span data-ttu-id="802bb-266">Microsoft 提供适合特定场景的映像。</span><span class="sxs-lookup"><span data-stu-id="802bb-266">Microsoft provides images that are tailored for specific scenarios.</span></span> <span data-ttu-id="802bb-267">例如，[ASP.NET Core 存储库](https://hub.docker.com/_/microsoft-dotnet-aspnet)提供针对在生产环境中运行 ASP.NET Core 应用生成的映像。</span><span class="sxs-lookup"><span data-stu-id="802bb-267">For example, the [ASP.NET Core repository](https://hub.docker.com/_/microsoft-dotnet-aspnet) provides images that are built for running ASP.NET Core apps in production.</span></span>

<span data-ttu-id="802bb-268">有关在 Docker 容器中使用 .NET Core 的详细信息，请参阅 [.NET 和 Docker 简介](../docker/introduction.md)和[示例](https://github.com/dotnet/dotnet-docker/blob/main/samples/README.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-268">For more information about using .NET Core in a Docker container, see [Introduction to .NET and Docker](../docker/introduction.md) and [Samples](https://github.com/dotnet/dotnet-docker/blob/main/samples/README.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="802bb-269">后续步骤</span><span class="sxs-lookup"><span data-stu-id="802bb-269">Next steps</span></span>

- <span data-ttu-id="802bb-270">[如何检查是否已安装 .NET Core](how-to-detect-installed-versions.md?pivots=os-macos)。</span><span class="sxs-lookup"><span data-stu-id="802bb-270">[How to check if .NET Core is already installed](how-to-detect-installed-versions.md?pivots=os-macos).</span></span>
- <span data-ttu-id="802bb-271">[处理 macOS Catalina 公证](macos-notarization-issues.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-271">[Working with macOS Catalina notarization](macos-notarization-issues.md).</span></span>
- <span data-ttu-id="802bb-272">[教程：开始使用 macOS](../tutorials/with-visual-studio-mac.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-272">[Tutorial: Get started on macOS](../tutorials/with-visual-studio-mac.md).</span></span>
- <span data-ttu-id="802bb-273">[教程：使用 Visual Studio Code 创建一个新应用](../tutorials/with-visual-studio-code.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-273">[Tutorial: Create a new app with Visual Studio Code](../tutorials/with-visual-studio-code.md).</span></span>
- <span data-ttu-id="802bb-274">[教程：使 .NET Core 应用容器化](../docker/build-container.md)。</span><span class="sxs-lookup"><span data-stu-id="802bb-274">[Tutorial: Containerize a .NET Core app](../docker/build-container.md).</span></span>

[release-notes-21]: https://github.com/dotnet/core/blob/main/release-notes/2.1/2.1-supported-os.md
[release-notes-31]: https://github.com/dotnet/core/blob/main/release-notes/3.1/3.1-supported-os.md
[release-notes-50]: https://github.com/dotnet/core/blob/main/release-notes/5.0/5.0-supported-os.md
[release-notes-20]: https://github.com/dotnet/core/blob/main/release-notes/2.0/2.0-supported-os.md
[release-notes-22]: https://github.com/dotnet/core/blob/main/release-notes/2.2/2.2-supported-os.md
[release-notes-30]: https://github.com/dotnet/core/blob/main/release-notes/3.0/3.0-supported-os.md
