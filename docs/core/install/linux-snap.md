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
# <a name="install-the-net-sdk-or-the-net-runtime-with-snap"></a><span data-ttu-id="4e647-103">通过 Snap 安装 .NET SDK 或 .NET Runtime</span><span class="sxs-lookup"><span data-stu-id="4e647-103">Install the .NET SDK or the .NET Runtime with Snap</span></span>

<span data-ttu-id="4e647-104">使用 Snap 包安装 .NET SDK 或 .NET Runtime。</span><span class="sxs-lookup"><span data-stu-id="4e647-104">Use a Snap package to install the .NET SDK or .NET Runtime.</span></span> <span data-ttu-id="4e647-105">对于内置于 Linux 发行版的包管理器而言，Snap 是一种很好的替代方法。</span><span class="sxs-lookup"><span data-stu-id="4e647-105">Snaps are a great alternative to the package manager built into your Linux distribution.</span></span> <span data-ttu-id="4e647-106">本文介绍如何通过 [Snap](https://snapcraft.io/dotnet-sdk) 安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="4e647-106">This article describes how to install .NET through [Snap](https://snapcraft.io/dotnet-sdk).</span></span>

<span data-ttu-id="4e647-107">Snap 是应用及其依赖项的捆绑包，无需修改即可在多个不同的 Linux 发行版中正常运行。</span><span class="sxs-lookup"><span data-stu-id="4e647-107">A snap is a bundle of an app and its dependencies that works without modification across many different Linux distributions.</span></span> <span data-ttu-id="4e647-108">可以从 Snap Store 中发现和安装 Snap。</span><span class="sxs-lookup"><span data-stu-id="4e647-108">Snaps are discoverable and installable from the Snap Store.</span></span> <span data-ttu-id="4e647-109">若要详细了解 Snap，请参阅[开始使用 Snap](https://snapcraft.io/docs/getting-started)。</span><span class="sxs-lookup"><span data-stu-id="4e647-109">For more information about Snap, see [Getting started with Snap](https://snapcraft.io/docs/getting-started).</span></span>

> [!CAUTION]
> <span data-ttu-id="4e647-110">Windows 10 上的 WSL2 不支持 Snap 包。</span><span class="sxs-lookup"><span data-stu-id="4e647-110">Snap packages aren't supported in WSL2 on Windows 10.</span></span> <span data-ttu-id="4e647-111">作为替代方法，可使用 [`dotnet-install` 脚本](linux-scripted-manual.md#scripted-install)或特定 WSL2 发行版的包管理器。</span><span class="sxs-lookup"><span data-stu-id="4e647-111">As an alternative, use the [`dotnet-install` script](linux-scripted-manual.md#scripted-install) or the package manager for the particular WSL2 distribution.</span></span> <span data-ttu-id="4e647-112">虽然可以尝试使用 [Snapcraft 论坛中的替代方法](https://forum.snapcraft.io/t/running-snaps-on-wsl2-insiders-only-for-now/13033)启用 Snap，但该方法不受支持，因此不建议这样做。</span><span class="sxs-lookup"><span data-stu-id="4e647-112">It's not recommended but you can try to enable snap with an [unsupported workaround from the snapcraft forums](https://forum.snapcraft.io/t/running-snaps-on-wsl2-insiders-only-for-now/13033).</span></span>

## <a name="net-releases"></a><span data-ttu-id="4e647-113">.NET 版本</span><span class="sxs-lookup"><span data-stu-id="4e647-113">.NET releases</span></span>

<span data-ttu-id="4e647-114">只有 ✔️ 受支持的 .NET SDK 版本，才能通过 Snap 获取。</span><span class="sxs-lookup"><span data-stu-id="4e647-114">Only ✔️ supported versions of .NET SDK are available through Snap.</span></span> <span data-ttu-id="4e647-115">从版本 2.1 开始，所有.NET Runtime 版本均可通过 Snap 获取。</span><span class="sxs-lookup"><span data-stu-id="4e647-115">All versions of the .NET Runtime are available through snap starting with version 2.1.</span></span> <span data-ttu-id="4e647-116">下表列出了 .NET（和 .NET Core）版本：</span><span class="sxs-lookup"><span data-stu-id="4e647-116">The following table lists the .NET (and .NET Core) releases:</span></span>

| <span data-ttu-id="4e647-117">✔️ 受支持</span><span class="sxs-lookup"><span data-stu-id="4e647-117">✔️ Supported</span></span> | <span data-ttu-id="4e647-118">❌ 不受支持</span><span class="sxs-lookup"><span data-stu-id="4e647-118">❌ Unsupported</span></span> |
|-------------|---------------|
| <span data-ttu-id="4e647-119">5.0</span><span class="sxs-lookup"><span data-stu-id="4e647-119">5.0</span></span>         | <span data-ttu-id="4e647-120">3.0</span><span class="sxs-lookup"><span data-stu-id="4e647-120">3.0</span></span>           |
| <span data-ttu-id="4e647-121">3.1 (LTS)</span><span class="sxs-lookup"><span data-stu-id="4e647-121">3.1 (LTS)</span></span>   | <span data-ttu-id="4e647-122">2.2</span><span class="sxs-lookup"><span data-stu-id="4e647-122">2.2</span></span>           |
| <span data-ttu-id="4e647-123">2.1 (LTS)</span><span class="sxs-lookup"><span data-stu-id="4e647-123">2.1 (LTS)</span></span>   | <span data-ttu-id="4e647-124">2.0</span><span class="sxs-lookup"><span data-stu-id="4e647-124">2.0</span></span>           |
|             | <span data-ttu-id="4e647-125">1.1</span><span class="sxs-lookup"><span data-stu-id="4e647-125">1.1</span></span>           |
|             | <span data-ttu-id="4e647-126">1.0</span><span class="sxs-lookup"><span data-stu-id="4e647-126">1.0</span></span>           |

<span data-ttu-id="4e647-127">有关 .NET 版本的生命周期的详细信息，请参阅 [.NET Core 和 .NET 5 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。</span><span class="sxs-lookup"><span data-stu-id="4e647-127">For more information about the life cycle of .NET releases, see [.NET Core and .NET 5 Support Policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).</span></span>

## <a name="sdk-or-runtime"></a><span data-ttu-id="4e647-128">SDK 或 Runtime</span><span class="sxs-lookup"><span data-stu-id="4e647-128">SDK or Runtime</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="install-the-sdk"></a><span data-ttu-id="4e647-129">安装 SDK</span><span class="sxs-lookup"><span data-stu-id="4e647-129">Install the SDK</span></span>

<span data-ttu-id="4e647-130">适用于 .NET SDK 的 Snap 包都是在同一标识符（即 `dotnet-sdk`）下发布的。</span><span class="sxs-lookup"><span data-stu-id="4e647-130">Snap packages for the .NET SDK are all published under the same identifier: `dotnet-sdk`.</span></span> <span data-ttu-id="4e647-131">可以通过指定通道来安装特定版本的 SDK。</span><span class="sxs-lookup"><span data-stu-id="4e647-131">A specific version of the SDK can be installed by specifying the channel.</span></span> <span data-ttu-id="4e647-132">SDK 包括相应的运行时。</span><span class="sxs-lookup"><span data-stu-id="4e647-132">The SDK includes the corresponding runtime.</span></span> <span data-ttu-id="4e647-133">下表列出了通道：</span><span class="sxs-lookup"><span data-stu-id="4e647-133">The following table lists the channels:</span></span>

| <span data-ttu-id="4e647-134">.NET 版本</span><span class="sxs-lookup"><span data-stu-id="4e647-134">.NET version</span></span> | <span data-ttu-id="4e647-135">Snap 包或通道</span><span class="sxs-lookup"><span data-stu-id="4e647-135">Snap package or channel</span></span>  |
|--------------|--------------------------|
| <span data-ttu-id="4e647-136">5.0</span><span class="sxs-lookup"><span data-stu-id="4e647-136">5.0</span></span>          | <span data-ttu-id="4e647-137">`5.0` 或 `latest/stable`</span><span class="sxs-lookup"><span data-stu-id="4e647-137">`5.0` or `latest/stable`</span></span> |
| <span data-ttu-id="4e647-138">3.1 (LTS)</span><span class="sxs-lookup"><span data-stu-id="4e647-138">3.1 (LTS)</span></span>    | <span data-ttu-id="4e647-139">`3.1` 或 `lts/stable`</span><span class="sxs-lookup"><span data-stu-id="4e647-139">`3.1` or `lts/stable`</span></span>    |
| <span data-ttu-id="4e647-140">2.1 (LTS)</span><span class="sxs-lookup"><span data-stu-id="4e647-140">2.1 (LTS)</span></span>    | `2.1`                    |

<span data-ttu-id="4e647-141">若要安装适用于 .NET SDK 的 Snap 包，请运行 `snap install` 命令。</span><span class="sxs-lookup"><span data-stu-id="4e647-141">Use the `snap install` command to install a .NET SDK snap package.</span></span> <span data-ttu-id="4e647-142">使用 `--channel` 参数来指明要安装哪个版本。</span><span class="sxs-lookup"><span data-stu-id="4e647-142">Use the `--channel` parameter to indicate which version to install.</span></span> <span data-ttu-id="4e647-143">如果省略此参数，则使用 `latest/stable`。</span><span class="sxs-lookup"><span data-stu-id="4e647-143">If this parameter is omitted, `latest/stable` is used.</span></span> <span data-ttu-id="4e647-144">在下面的示例中，指定的是 `5.0`：</span><span class="sxs-lookup"><span data-stu-id="4e647-144">In this example, `5.0` is specified:</span></span>

```bash
sudo snap install dotnet-sdk --classic --channel=5.0
```

<span data-ttu-id="4e647-145">接下来，使用 `snap alias` 命令为系统注册 `dotnet` 命令：</span><span class="sxs-lookup"><span data-stu-id="4e647-145">Next, register the `dotnet` command for the system with the `snap alias` command:</span></span>

```bash
sudo snap alias dotnet-sdk.dotnet dotnet
```

<span data-ttu-id="4e647-146">此命令的格式为 `sudo snap alias {package}.{command} {alias}`。</span><span class="sxs-lookup"><span data-stu-id="4e647-146">This command is formatted as: `sudo snap alias {package}.{command} {alias}`.</span></span> <span data-ttu-id="4e647-147">可以选择所需的任何 `{alias}` 名称。</span><span class="sxs-lookup"><span data-stu-id="4e647-147">You can choose any `{alias}` name you would like.</span></span> <span data-ttu-id="4e647-148">例如，可以根据 Snap 安装的特定版本来命名此命令：`sudo snap alias dotnet-sdk.dotnet dotnet50`。</span><span class="sxs-lookup"><span data-stu-id="4e647-148">For example, you could name the command after the specific version installed by snap: `sudo snap alias dotnet-sdk.dotnet dotnet50`.</span></span> <span data-ttu-id="4e647-149">运行命令 `dotnet50` 时，将调用这一特定版本的 .NET。</span><span class="sxs-lookup"><span data-stu-id="4e647-149">When you use the command `dotnet50`, you'll invoke this specific version of .NET.</span></span> <span data-ttu-id="4e647-150">但选择其他别名与大多数教程和示例不兼容，因为它们需要使用 `dotnet` 命令。</span><span class="sxs-lookup"><span data-stu-id="4e647-150">But choosing a different alias is incompatible with most tutorials and examples as they expect a `dotnet` command to be used.</span></span>

## <a name="install-the-runtime"></a><span data-ttu-id="4e647-151">安装运行时</span><span class="sxs-lookup"><span data-stu-id="4e647-151">Install the runtime</span></span>

<span data-ttu-id="4e647-152">适用于 .NET Runtime 的 Snap 包都是在各自的包标识符下发布的。</span><span class="sxs-lookup"><span data-stu-id="4e647-152">Snap packages for the .NET Runtime are each published under their own package identifier.</span></span> <span data-ttu-id="4e647-153">下表列出了这些包标识符：</span><span class="sxs-lookup"><span data-stu-id="4e647-153">The following table lists the package identifiers:</span></span>

| <span data-ttu-id="4e647-154">.NET 版本</span><span class="sxs-lookup"><span data-stu-id="4e647-154">.NET version</span></span>      | <span data-ttu-id="4e647-155">Snap 包</span><span class="sxs-lookup"><span data-stu-id="4e647-155">Snap package</span></span>        |
|-------------------|---------------------|
| <span data-ttu-id="4e647-156">5.0</span><span class="sxs-lookup"><span data-stu-id="4e647-156">5.0</span></span>               | `dotnet-runtime-50` |
| <span data-ttu-id="4e647-157">3.1 (LTS)</span><span class="sxs-lookup"><span data-stu-id="4e647-157">3.1 (LTS)</span></span>         | `dotnet-runtime-31` |
| <span data-ttu-id="4e647-158">3.0</span><span class="sxs-lookup"><span data-stu-id="4e647-158">3.0</span></span>               | `dotnet-runtime-30` |
| <span data-ttu-id="4e647-159">2.2</span><span class="sxs-lookup"><span data-stu-id="4e647-159">2.2</span></span>               | `dotnet-runtime-22` |
| <span data-ttu-id="4e647-160">2.1 (LTS)</span><span class="sxs-lookup"><span data-stu-id="4e647-160">2.1 (LTS)</span></span>         | `dotnet-runtime-21` |

<span data-ttu-id="4e647-161">若要安装适用于 .NET Runtime 的 Snap 包，请运行 `snap install` 命令。</span><span class="sxs-lookup"><span data-stu-id="4e647-161">Use the `snap install` command to install a .NET Runtime snap package.</span></span> <span data-ttu-id="4e647-162">在下面的示例中，安装的是 .NET 5.0：</span><span class="sxs-lookup"><span data-stu-id="4e647-162">In this example, .NET 5.0 is installed:</span></span>

```bash
sudo snap install dotnet-runtime-50 --classic
```

<span data-ttu-id="4e647-163">接下来，使用 `snap alias` 命令为系统注册 `dotnet` 命令：</span><span class="sxs-lookup"><span data-stu-id="4e647-163">Next, register the `dotnet` command for the system with the `snap alias` command:</span></span>

```bash
sudo snap alias dotnet-runtime-50.dotnet dotnet
```

<span data-ttu-id="4e647-164">此命令的格式为 `sudo snap alias {package}.{command} {alias}`。</span><span class="sxs-lookup"><span data-stu-id="4e647-164">The command is formatted as: `sudo snap alias {package}.{command} {alias}`.</span></span> <span data-ttu-id="4e647-165">可以选择所需的任何 `{alias}` 名称。</span><span class="sxs-lookup"><span data-stu-id="4e647-165">You can choose any `{alias}` name you would like.</span></span> <span data-ttu-id="4e647-166">例如，可以根据 Snap 安装的特定版本来命名此命令：`sudo snap alias dotnet-runtime-50.dotnet dotnet50`。</span><span class="sxs-lookup"><span data-stu-id="4e647-166">For example, you could name the command after the specific version installed by snap: `sudo snap alias dotnet-runtime-50.dotnet dotnet50`.</span></span> <span data-ttu-id="4e647-167">运行命令 `dotnet50` 时，将调用特定版本的 .NET。</span><span class="sxs-lookup"><span data-stu-id="4e647-167">When you use the command `dotnet50`, you'll invoke a specific version of .NET.</span></span> <span data-ttu-id="4e647-168">但选择其他别名与大多数教程和示例不兼容，因为它们需要使用 `dotnet` 命令。</span><span class="sxs-lookup"><span data-stu-id="4e647-168">But choosing a different alias is incompatible with most tutorials and examples as they expect a `dotnet` command to be available.</span></span>

## <a name="export-the-install-location"></a><span data-ttu-id="4e647-169">导出安装位置</span><span class="sxs-lookup"><span data-stu-id="4e647-169">Export the install location</span></span>

<span data-ttu-id="4e647-170">`DOTNET_ROOT` 环境变量经常被工具用来确定 .NET 的安装位置。</span><span class="sxs-lookup"><span data-stu-id="4e647-170">The `DOTNET_ROOT` environment variable is often used by tools to determine where .NET is installed.</span></span> <span data-ttu-id="4e647-171">通过 Snap 安装 .NET 时，不配置此环境变量。</span><span class="sxs-lookup"><span data-stu-id="4e647-171">When .NET is installed through Snap, this environment variable isn't configured.</span></span> <span data-ttu-id="4e647-172">应在配置文件中配置 DOTNET_ROOT 环境变量。</span><span class="sxs-lookup"><span data-stu-id="4e647-172">You should configure the *DOTNET_ROOT* environment variable in your profile.</span></span> <span data-ttu-id="4e647-173">Snap 的路径采用以下格式：`/snap/{package}/current`。</span><span class="sxs-lookup"><span data-stu-id="4e647-173">The path to the snap uses the following format: `/snap/{package}/current`.</span></span> <span data-ttu-id="4e647-174">例如，如果你安装了 `dotnet-sdk` Snap，则使用以下命令将环境变量设置为 .NET 所在的位置：</span><span class="sxs-lookup"><span data-stu-id="4e647-174">For example, if you installed the `dotnet-sdk` snap, use the following command to set the environment variable to where .NET is located:</span></span>

```bash
export DOTNET_ROOT=/snap/dotnet-sdk/current
```

> [!TIP]
> <span data-ttu-id="4e647-175">前面的 `export` 命令只为运行它的终端会话设置环境变量。</span><span class="sxs-lookup"><span data-stu-id="4e647-175">The preceding `export` command only sets the environment variable for the terminal session in which it was run.</span></span>
>
> <span data-ttu-id="4e647-176">你可以编辑 shell 配置文件，永久地添加这些命令。</span><span class="sxs-lookup"><span data-stu-id="4e647-176">You can edit your shell profile to permanently add the commands.</span></span> <span data-ttu-id="4e647-177">Linux 提供了许多不同的 shell，每个都有不同的配置文件。</span><span class="sxs-lookup"><span data-stu-id="4e647-177">There are a number of different shells available for Linux and each has a different profile.</span></span> <span data-ttu-id="4e647-178">例如：</span><span class="sxs-lookup"><span data-stu-id="4e647-178">For example:</span></span>
>
> - <span data-ttu-id="4e647-179">**Bash Shell**：~/.bash_profile、~/.bashrc</span><span class="sxs-lookup"><span data-stu-id="4e647-179">**Bash Shell**: *~/.bash_profile*, *~/.bashrc*</span></span>
> - <span data-ttu-id="4e647-180">**Korn Shell**：~/.kshrc 或 .profile</span><span class="sxs-lookup"><span data-stu-id="4e647-180">**Korn Shell**: *~/.kshrc* or *.profile*</span></span>
> - <span data-ttu-id="4e647-181">**Z Shell**：~/.zshrc 或 .zprofile</span><span class="sxs-lookup"><span data-stu-id="4e647-181">**Z Shell**: *~/.zshrc* or *.zprofile*</span></span>
>
> <span data-ttu-id="4e647-182">为 shell 编辑相应的源文件并添加 `export DOTNET_ROOT=/snap/dotnet-sdk/current`。</span><span class="sxs-lookup"><span data-stu-id="4e647-182">Edit the appropriate source file for your shell and add `export DOTNET_ROOT=/snap/dotnet-sdk/current`.</span></span>

## <a name="tlsssl-certificate-errors"></a><span data-ttu-id="4e647-183">TLS/SSL 证书错误</span><span class="sxs-lookup"><span data-stu-id="4e647-183">TLS/SSL Certificate errors</span></span>

<span data-ttu-id="4e647-184">通过 Snap 安装 .NET 后，可能会在某些发行版上找不到 .NET TLS/SSL 证书，并且可能会在 `restore` 期间看到以下错误：</span><span class="sxs-lookup"><span data-stu-id="4e647-184">When .NET is installed through Snap, it's possible that on some distros the .NET TLS/SSL certificates may not be found and you may receive an error during `restore`:</span></span>

```bash
Processing post-creation actions...
Running 'dotnet restore' on /home/myhome/test/test.csproj...
  Restoring packages for /home/myhome/test/test.csproj...
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error : Unable to load the service index for source https://api.nuget.org/v3/index.json. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The SSL connection could not be established, see inner exception. [/home/myhome/test/test.csproj]
/snap/dotnet-sdk/27/sdk/2.2.103/NuGet.targets(114,5): error :   The remote certificate is invalid according to the validation procedure. [/home/myhome/test/test.csproj]
```

<span data-ttu-id="4e647-185">若要解决此问题，请设置一些环境变量：</span><span class="sxs-lookup"><span data-stu-id="4e647-185">To resolve this problem, set a few environment variables:</span></span>

```bash
export SSL_CERT_FILE=[path-to-certificate-file]
export SSL_CERT_DIR=/dev/null
```

<span data-ttu-id="4e647-186">证书位置因发行版而异。</span><span class="sxs-lookup"><span data-stu-id="4e647-186">The certificate location will vary by distro.</span></span> <span data-ttu-id="4e647-187">下面是我们在发行版中遇到此问题的位置。</span><span class="sxs-lookup"><span data-stu-id="4e647-187">Here are the locations for the distros where the issue has been experienced.</span></span>

| <span data-ttu-id="4e647-188">分发</span><span class="sxs-lookup"><span data-stu-id="4e647-188">Distribution</span></span> | <span data-ttu-id="4e647-189">位置</span><span class="sxs-lookup"><span data-stu-id="4e647-189">Location</span></span>                                            |
|--------------|-----------------------------------------------------|
| <span data-ttu-id="4e647-190">**Fedora**</span><span class="sxs-lookup"><span data-stu-id="4e647-190">**Fedora**</span></span>   | `/etc/pki/ca-trust/extracted/pem/tls-ca-bundle.pem` |
| <span data-ttu-id="4e647-191">**OpenSUSE**</span><span class="sxs-lookup"><span data-stu-id="4e647-191">**OpenSUSE**</span></span> | `/etc/ssl/ca-bundle.pem`                            |
| <span data-ttu-id="4e647-192">**Solus**</span><span class="sxs-lookup"><span data-stu-id="4e647-192">**Solus**</span></span>    | `/etc/ssl/certs/ca-certificates.crt`                |

## <a name="next-steps"></a><span data-ttu-id="4e647-193">后续步骤</span><span class="sxs-lookup"><span data-stu-id="4e647-193">Next steps</span></span>

- [<span data-ttu-id="4e647-194">如何为 .NET CLI 启用 Tab 自动补全</span><span class="sxs-lookup"><span data-stu-id="4e647-194">How to enable TAB completion for the .NET CLI</span></span>](../tools/enable-tab-autocomplete.md)
- [<span data-ttu-id="4e647-195">教程：使用 Visual Studio Code 通过 .NET SDK 创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="4e647-195">Tutorial: Create a console application with .NET SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)
