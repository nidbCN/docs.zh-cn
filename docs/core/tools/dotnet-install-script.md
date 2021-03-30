---
title: dotnet-install 脚本
description: 了解用于安装 .NET SDK 和共享运行时的 dotnet-install 脚本。
ms.date: 09/22/2020
ms.openlocfilehash: 45b5a15bdbaa3f3167dd434e2600e33b69dc9b85
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875156"
---
# <a name="dotnet-install-scripts-reference"></a><span data-ttu-id="ca59c-103">dotnet-install 脚本引用</span><span class="sxs-lookup"><span data-stu-id="ca59c-103">dotnet-install scripts reference</span></span>

## <a name="name"></a><span data-ttu-id="ca59c-104">“属性”</span><span class="sxs-lookup"><span data-stu-id="ca59c-104">Name</span></span>

<span data-ttu-id="ca59c-105">`dotnet-install.ps1` | `dotnet-install.sh` - 用于安装 .NET SDK 和共享运行时的脚本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-105">`dotnet-install.ps1` | `dotnet-install.sh` - Script used to install the .NET SDK and the shared runtime.</span></span>

## <a name="synopsis"></a><span data-ttu-id="ca59c-106">摘要</span><span class="sxs-lookup"><span data-stu-id="ca59c-106">Synopsis</span></span>

<span data-ttu-id="ca59c-107">Windows：</span><span class="sxs-lookup"><span data-stu-id="ca59c-107">Windows:</span></span>

```powershell
dotnet-install.ps1 [-Architecture <ARCHITECTURE>] [-AzureFeed]
    [-Channel <CHANNEL>] [-DryRun] [-FeedCredential]
    [-InstallDir <DIRECTORY>] [-JSonFile <JSONFILE>]
    [-NoCdn] [-NoPath] [-ProxyAddress] [-ProxyBypassList <LIST_OF_URLS>]
    [-ProxyUseDefaultCredentials] [-Runtime <RUNTIME>]
    [-SkipNonVersionedFiles] [-UncachedFeed] [-Verbose]
    [-Version <VERSION>]

Get-Help ./dotnet-install.ps1
```

<span data-ttu-id="ca59c-108">Linux/macOS：</span><span class="sxs-lookup"><span data-stu-id="ca59c-108">Linux/macOS:</span></span>

```bash
dotnet-install.sh  [--architecture <ARCHITECTURE>] [--azure-feed]
    [--channel <CHANNEL>] [--dry-run] [--feed-credential]
    [--install-dir <DIRECTORY>] [--jsonfile <JSONFILE>]
    [--no-cdn] [--no-path] [--runtime <RUNTIME>] [--runtime-id <RID>]
    [--skip-non-versioned-files] [--uncached-feed] [--verbose]
    [--version <VERSION>]

dotnet-install.sh --help
```

<span data-ttu-id="ca59c-109">bash 脚本也读取 PowerShell 开关。因此，可以在 Linux/macOS 系统上将 PowerShell 开关与脚本结合使用。</span><span class="sxs-lookup"><span data-stu-id="ca59c-109">The bash script also reads PowerShell switches, so you can use PowerShell switches with the script on Linux/macOS systems.</span></span>

## <a name="description"></a><span data-ttu-id="ca59c-110">描述</span><span class="sxs-lookup"><span data-stu-id="ca59c-110">Description</span></span>

<span data-ttu-id="ca59c-111">`dotnet-install` 脚本执行 .NET SDK 的非管理员安装，其中包含 .NET CLI 和共享运行时。</span><span class="sxs-lookup"><span data-stu-id="ca59c-111">The `dotnet-install` scripts perform a non-admin installation of the .NET SDK, which includes the .NET CLI and the shared runtime.</span></span> <span data-ttu-id="ca59c-112">有两个脚本：</span><span class="sxs-lookup"><span data-stu-id="ca59c-112">There are two scripts:</span></span>

* <span data-ttu-id="ca59c-113">在 Windows 上运行的 PowerShell 脚本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-113">A PowerShell script that works on Windows.</span></span>
* <span data-ttu-id="ca59c-114">在 Linux/macOS 上运行的 bash 脚本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-114">A bash script that works on Linux/macOS.</span></span>

> [!NOTE]
> <span data-ttu-id="ca59c-115">.NET 会收集遥测数据。</span><span class="sxs-lookup"><span data-stu-id="ca59c-115">.NET collects telemetry data.</span></span> <span data-ttu-id="ca59c-116">若要了解详细信息和退出方式，请查看 [.NET SDK 遥测](telemetry.md)。</span><span class="sxs-lookup"><span data-stu-id="ca59c-116">To learn more and how to opt out, see [.NET SDK telemetry](telemetry.md).</span></span>

### <a name="purpose"></a><span data-ttu-id="ca59c-117">目标</span><span class="sxs-lookup"><span data-stu-id="ca59c-117">Purpose</span></span>

 <span data-ttu-id="ca59c-118">脚本的预期用途是用于持续集成 (CI) 方案，其中：</span><span class="sxs-lookup"><span data-stu-id="ca59c-118">The intended use of the scripts is for Continuous Integration (CI) scenarios, where:</span></span>

* <span data-ttu-id="ca59c-119">需要在无需用户交互和管理员权限的情况下安装 SDK。</span><span class="sxs-lookup"><span data-stu-id="ca59c-119">The SDK needs to be installed without user interaction and without admin rights.</span></span>
* <span data-ttu-id="ca59c-120">无需在多个 CI 运行中保留 SDK 安装。</span><span class="sxs-lookup"><span data-stu-id="ca59c-120">The SDK installation doesn't need to persist across multiple CI runs.</span></span>

  <span data-ttu-id="ca59c-121">典型的事件序列：</span><span class="sxs-lookup"><span data-stu-id="ca59c-121">The typical sequence of events:</span></span>
  * <span data-ttu-id="ca59c-122">触发 CI。</span><span class="sxs-lookup"><span data-stu-id="ca59c-122">CI is triggered.</span></span>
  * <span data-ttu-id="ca59c-123">CI 使用其中一个脚本安装 SDK。</span><span class="sxs-lookup"><span data-stu-id="ca59c-123">CI installs the SDK using one of these scripts.</span></span>
  * <span data-ttu-id="ca59c-124">CI 完成其工作并清除临时数据（包括 SDK 安装）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-124">CI finishes its work and clears temporary data including the SDK installation.</span></span>

<span data-ttu-id="ca59c-125">要设置开发环境或运行应用，请使用安装程序而不是这些脚本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-125">To set up a development environment or to run apps, use the installers rather than these scripts.</span></span>

### <a name="recommended-version"></a><span data-ttu-id="ca59c-126">建议的版本</span><span class="sxs-lookup"><span data-stu-id="ca59c-126">Recommended version</span></span>

<span data-ttu-id="ca59c-127">建议使用脚本的稳定版本：</span><span class="sxs-lookup"><span data-stu-id="ca59c-127">We recommend that you use the stable version of the scripts:</span></span>

- <span data-ttu-id="ca59c-128">Bash (Linux/macOS)：<https://dot.net/v1/dotnet-install.sh></span><span class="sxs-lookup"><span data-stu-id="ca59c-128">Bash (Linux/macOS): <https://dot.net/v1/dotnet-install.sh></span></span>
- <span data-ttu-id="ca59c-129">PowerShell (Windows)：<https://dot.net/v1/dotnet-install.ps1></span><span class="sxs-lookup"><span data-stu-id="ca59c-129">PowerShell (Windows): <https://dot.net/v1/dotnet-install.ps1></span></span>

### <a name="script-behavior"></a><span data-ttu-id="ca59c-130">脚本行为</span><span class="sxs-lookup"><span data-stu-id="ca59c-130">Script behavior</span></span>

<span data-ttu-id="ca59c-131">这两个脚本的行为相同。</span><span class="sxs-lookup"><span data-stu-id="ca59c-131">Both scripts have the same behavior.</span></span> <span data-ttu-id="ca59c-132">它们从 CLI 生成放置下载 ZIP/tarball 文件，并将其安装在默认位置或 `-InstallDir|--install-dir` 所指定的位置。</span><span class="sxs-lookup"><span data-stu-id="ca59c-132">They download the ZIP/tarball file from the CLI build drops and proceed to install it in either the default location or in a location specified by `-InstallDir|--install-dir`.</span></span>

<span data-ttu-id="ca59c-133">默认情况下，安装脚本下载 SDK 并安装它。</span><span class="sxs-lookup"><span data-stu-id="ca59c-133">By default, the installation scripts download the SDK and install it.</span></span> <span data-ttu-id="ca59c-134">如果只想获取共享的运行时，请指定 `-Runtime|--runtime` 参数。</span><span class="sxs-lookup"><span data-stu-id="ca59c-134">If you wish to only obtain the shared runtime, specify the `-Runtime|--runtime` argument.</span></span>

<span data-ttu-id="ca59c-135">默认情况下，该脚本会将安装位置添加到当前会话的 $PATH。</span><span class="sxs-lookup"><span data-stu-id="ca59c-135">By default, the script adds the install location to the $PATH for the current session.</span></span> <span data-ttu-id="ca59c-136">通过指定 `-NoPath|--no-path` 参数覆盖此默认行为。</span><span class="sxs-lookup"><span data-stu-id="ca59c-136">Override this default behavior by specifying the `-NoPath|--no-path` argument.</span></span> <span data-ttu-id="ca59c-137">脚本未设置 `DOTNET_ROOT` 环境变量。</span><span class="sxs-lookup"><span data-stu-id="ca59c-137">The script doesn't set the `DOTNET_ROOT` environment variable.</span></span>

<span data-ttu-id="ca59c-138">运行脚本前，请安装所需的[依赖项](../install/windows.md#dependencies)。</span><span class="sxs-lookup"><span data-stu-id="ca59c-138">Before running the script, install the required [dependencies](../install/windows.md#dependencies).</span></span>

<span data-ttu-id="ca59c-139">可以使用 `-Version|--version` 参数安装特定版本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-139">You can install a specific version using the `-Version|--version` argument.</span></span> <span data-ttu-id="ca59c-140">必须将版本指定为由 3 部分构成的版本号，例如 `2.1.0`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-140">The version must be specified as a three-part version number, such as `2.1.0`.</span></span> <span data-ttu-id="ca59c-141">如果未指定版本，则脚本将安装 `latest` 版本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-141">If the version isn't specified, the script installs the `latest` version.</span></span>

<span data-ttu-id="ca59c-142">安装脚本不会更新 Windows 上的注册表。</span><span class="sxs-lookup"><span data-stu-id="ca59c-142">The install scripts do not update the registry on Windows.</span></span> <span data-ttu-id="ca59c-143">它们只是下载压缩的二进制文件并将其复制到文件夹。</span><span class="sxs-lookup"><span data-stu-id="ca59c-143">They just download the zipped binaries and copy them to a folder.</span></span> <span data-ttu-id="ca59c-144">如果要更新注册表项值，请使用 .NET 安装程序。</span><span class="sxs-lookup"><span data-stu-id="ca59c-144">If you want registry key values to be updated, use the .NET installers.</span></span>

## <a name="options"></a><span data-ttu-id="ca59c-145">选项</span><span class="sxs-lookup"><span data-stu-id="ca59c-145">Options</span></span>

- **`-Architecture|--architecture <ARCHITECTURE>`**

  <span data-ttu-id="ca59c-146">要安装的 .NET 二进制文件的体系结构。</span><span class="sxs-lookup"><span data-stu-id="ca59c-146">Architecture of the .NET binaries to install.</span></span> <span data-ttu-id="ca59c-147">可能值为 `<auto>`、`amd64`、`x64`、`x86`、`arm64` 以及 `arm`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-147">Possible values are `<auto>`, `amd64`, `x64`, `x86`, `arm64`, and `arm`.</span></span> <span data-ttu-id="ca59c-148">默认值为 `<auto>`，它表示当前正在运行的操作系统体系结构。</span><span class="sxs-lookup"><span data-stu-id="ca59c-148">The default value is `<auto>`, which represents the currently running OS architecture.</span></span>

- **`-AzureFeed|--azure-feed`**

  <span data-ttu-id="ca59c-149">指定此安装程序的 Azure 源的 URL。</span><span class="sxs-lookup"><span data-stu-id="ca59c-149">Specifies the URL for the Azure feed to the installer.</span></span> <span data-ttu-id="ca59c-150">建议不要更改该值。</span><span class="sxs-lookup"><span data-stu-id="ca59c-150">We recommended that you don't change this value.</span></span> <span data-ttu-id="ca59c-151">默认值为 `https://dotnetcli.azureedge.net/dotnet`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-151">The default value is `https://dotnetcli.azureedge.net/dotnet`.</span></span>

- **`-Channel|--channel <CHANNEL>`**

  <span data-ttu-id="ca59c-152">指定安装的源通道。</span><span class="sxs-lookup"><span data-stu-id="ca59c-152">Specifies the source channel for the installation.</span></span> <span data-ttu-id="ca59c-153">可能的值为：</span><span class="sxs-lookup"><span data-stu-id="ca59c-153">The possible values are:</span></span>

  - <span data-ttu-id="ca59c-154">`Current` - 最新版本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-154">`Current` - Most current release.</span></span>
  - <span data-ttu-id="ca59c-155">`LTS` - 长期支持频道（最新受支持版本）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-155">`LTS` - Long-Term Support channel (most current supported release).</span></span>
  - <span data-ttu-id="ca59c-156">表示特定版本的由两部分构成的 X.Y 格式的版本（例如 `2.1` 或 `3.0`）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-156">Two-part version in X.Y format representing a specific release (for example, `2.1` or `3.0`).</span></span>
  - <span data-ttu-id="ca59c-157">分支名称：例如 `release/3.1.1xx` 或 `master`（适用于每日测试版本）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-157">Branch name: for example, `release/3.1.1xx` or `master` (for nightly releases).</span></span> <span data-ttu-id="ca59c-158">使用此选项可以从预览通道安装版本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-158">Use this option to install a version from a preview channel.</span></span> <span data-ttu-id="ca59c-159">使用[安装程序和二进制文件](https://github.com/dotnet/core-sdk#installers-and-binaries)中列出的通道名称。</span><span class="sxs-lookup"><span data-stu-id="ca59c-159">Use the name of the channel as listed in [Installers and Binaries](https://github.com/dotnet/core-sdk#installers-and-binaries).</span></span>

  <span data-ttu-id="ca59c-160">默认值为 `LTS`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-160">The default value is `LTS`.</span></span> <span data-ttu-id="ca59c-161">有关 .NET 支持频道的详细信息，请参阅 [.NET 支持策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)页。</span><span class="sxs-lookup"><span data-stu-id="ca59c-161">For more information on .NET support channels, see the [.NET Support Policy](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) page.</span></span>

- **`-DryRun|--dry-run`**

  <span data-ttu-id="ca59c-162">如果设置，脚本将不会执行安装。</span><span class="sxs-lookup"><span data-stu-id="ca59c-162">If set, the script won't perform the installation.</span></span> <span data-ttu-id="ca59c-163">而会显示要使用哪个命令行来持续安装当前请求的 .NET CLI 版本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-163">Instead, it displays what command line to use to consistently install the currently requested version of the .NET CLI.</span></span> <span data-ttu-id="ca59c-164">例如，如果指定版本 `latest`，它将显示特定版本的链接，以便可在生成脚本中明确地使用此命令。</span><span class="sxs-lookup"><span data-stu-id="ca59c-164">For example, if you specify version `latest`, it displays a link with the specific version so that this command can be used deterministically in a build script.</span></span> <span data-ttu-id="ca59c-165">如果想要自行安装或下载，它还会显示二进制文件位置。</span><span class="sxs-lookup"><span data-stu-id="ca59c-165">It also displays the binary's location if you prefer to install or download it yourself.</span></span>

- **`-FeedCredential|--feed-credential`**

  <span data-ttu-id="ca59c-166">用作追加到 Azure 源的查询字符串。</span><span class="sxs-lookup"><span data-stu-id="ca59c-166">Used as a query string to append to the Azure feed.</span></span> <span data-ttu-id="ca59c-167">这允许更改 URL 以使用非公共 blob 存储帐户。</span><span class="sxs-lookup"><span data-stu-id="ca59c-167">It allows changing the URL to use non-public blob storage accounts.</span></span>

- **`--help`**

  <span data-ttu-id="ca59c-168">打印脚本帮助。</span><span class="sxs-lookup"><span data-stu-id="ca59c-168">Prints out help for the script.</span></span> <span data-ttu-id="ca59c-169">仅适用于 bash 脚本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-169">Applies only to bash script.</span></span> <span data-ttu-id="ca59c-170">对于 PowerShell，请使用 `Get-Help ./dotnet-install.ps1`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-170">For PowerShell, use `Get-Help ./dotnet-install.ps1`.</span></span>

- **`-InstallDir|--install-dir <DIRECTORY>`**

  <span data-ttu-id="ca59c-171">指定安装路径。</span><span class="sxs-lookup"><span data-stu-id="ca59c-171">Specifies the installation path.</span></span> <span data-ttu-id="ca59c-172">如果不存在，则会创建该目录。</span><span class="sxs-lookup"><span data-stu-id="ca59c-172">The directory is created if it doesn't exist.</span></span> <span data-ttu-id="ca59c-173">默认值为“%LocalAppData%\Microsoft\dotnet”（在 Windows 上）和“/usr/share/dotnet”（在 Linux/macOS 上）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-173">The default value is *%LocalAppData%\Microsoft\dotnet* on Windows and */usr/share/dotnet* on Linux/macOS.</span></span> <span data-ttu-id="ca59c-174">会将二进制文件直接放入目录中。</span><span class="sxs-lookup"><span data-stu-id="ca59c-174">Binaries are placed directly in this directory.</span></span>

- **`-JSonFile|--jsonfile <JSONFILE>`**

  <span data-ttu-id="ca59c-175">指定将用于确定 SDK 版本的 [global.json](global-json.md) 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="ca59c-175">Specifies a path to a [global.json](global-json.md) file that will be used to determine the SDK version.</span></span> <span data-ttu-id="ca59c-176">global.json 文件必须具有 `sdk:version` 的值。</span><span class="sxs-lookup"><span data-stu-id="ca59c-176">The *global.json* file must have a value for `sdk:version`.</span></span>

- **`-NoCdn|--no-cdn`**

  <span data-ttu-id="ca59c-177">禁止从 [Azure 内容分发网络 (CDN)](/azure/cdn/cdn-overview) 进行下载，并直接使用未缓存源。</span><span class="sxs-lookup"><span data-stu-id="ca59c-177">Disables downloading from the [Azure Content Delivery Network (CDN)](/azure/cdn/cdn-overview) and uses the uncached feed directly.</span></span>

- **`-NoPath|--no-path`**

  <span data-ttu-id="ca59c-178">如果设定，不会将安装文件夹导出到当前会话的路径。</span><span class="sxs-lookup"><span data-stu-id="ca59c-178">If set, the installation folder isn't exported to the path for the current session.</span></span> <span data-ttu-id="ca59c-179">默认情况下，该脚本会修改 PATH，这会使 .NET CLI 在安装后立即可用。</span><span class="sxs-lookup"><span data-stu-id="ca59c-179">By default, the script modifies the PATH, which makes the .NET CLI available immediately after install.</span></span>

- **`-ProxyAddress`**

  <span data-ttu-id="ca59c-180">如果设置，安装程序发出 Web 请求时将使用该代理。</span><span class="sxs-lookup"><span data-stu-id="ca59c-180">If set, the installer uses the proxy when making web requests.</span></span> <span data-ttu-id="ca59c-181">（仅适用于 Windows。）</span><span class="sxs-lookup"><span data-stu-id="ca59c-181">(Only valid for Windows.)</span></span>

- **`-ProxyBypassList <LIST_OF_URLS>`**

  <span data-ttu-id="ca59c-182">如果设置了 `ProxyAddress`，它会提供一个以逗号分隔的 URL 列表，表中的 URL 将绕过代理。</span><span class="sxs-lookup"><span data-stu-id="ca59c-182">If set with `ProxyAddress`, provides a list of comma-separated urls that will bypass the proxy.</span></span> <span data-ttu-id="ca59c-183">（仅适用于 Windows。）</span><span class="sxs-lookup"><span data-stu-id="ca59c-183">(Only valid for Windows.)</span></span>

- **`ProxyUseDefaultCredentials`**

  <span data-ttu-id="ca59c-184">如果设置，在使用代理地址时，安装程序会使用当前用户的凭据。</span><span class="sxs-lookup"><span data-stu-id="ca59c-184">If set, the installer uses the credentials of the current user when using proxy address.</span></span> <span data-ttu-id="ca59c-185">（仅适用于 Windows。）</span><span class="sxs-lookup"><span data-stu-id="ca59c-185">(Only valid for Windows.)</span></span>

- **`-Runtime|--runtime <RUNTIME>`**

  <span data-ttu-id="ca59c-186">仅安装共享运行时，而非整个 SDK。</span><span class="sxs-lookup"><span data-stu-id="ca59c-186">Installs just the shared runtime, not the entire SDK.</span></span> <span data-ttu-id="ca59c-187">可能的值为：</span><span class="sxs-lookup"><span data-stu-id="ca59c-187">The possible values are:</span></span>

  - <span data-ttu-id="ca59c-188">`dotnet` - `Microsoft.NETCore.App` 共享运行时。</span><span class="sxs-lookup"><span data-stu-id="ca59c-188">`dotnet` - the `Microsoft.NETCore.App` shared runtime.</span></span>
  - <span data-ttu-id="ca59c-189">`aspnetcore` - `Microsoft.AspNetCore.App` 共享运行时。</span><span class="sxs-lookup"><span data-stu-id="ca59c-189">`aspnetcore` - the `Microsoft.AspNetCore.App` shared runtime.</span></span>
  - <span data-ttu-id="ca59c-190">`windowsdesktop` - `Microsoft.WindowsDesktop.App` 共享运行时。</span><span class="sxs-lookup"><span data-stu-id="ca59c-190">`windowsdesktop` - the `Microsoft.WindowsDesktop.App` shared runtime.</span></span>

- <span data-ttu-id="ca59c-191">**`--runtime-id <RID>` [已弃用]**</span><span class="sxs-lookup"><span data-stu-id="ca59c-191">**`--runtime-id <RID>` [Deprecated]**</span></span>

  <span data-ttu-id="ca59c-192">指定要为其安装工具的[运行时标识符](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="ca59c-192">Specifies the [runtime identifier](../rid-catalog.md) for which the tools are being installed.</span></span> <span data-ttu-id="ca59c-193">使用适用于可移植 Linux 的 `linux-x64`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-193">Use `linux-x64` for portable Linux.</span></span> <span data-ttu-id="ca59c-194">（仅适用于 Linux/macOS 和低于 .NET Core 2.1 的版本。）</span><span class="sxs-lookup"><span data-stu-id="ca59c-194">(Only valid for Linux/macOS and for versions earlier than .NET Core 2.1.)</span></span>

  **`--os <OPERATING_SYSTEM>`**

  <span data-ttu-id="ca59c-195">指定要为其安装工具的操作系统。</span><span class="sxs-lookup"><span data-stu-id="ca59c-195">Specifies the operating system for which the tools are being installed.</span></span> <span data-ttu-id="ca59c-196">可能的值包括：`osx`、`linux`、`linux-musl`、`freebsd`、`rhel.6`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-196">Possible values are: `osx`, `linux`, `linux-musl`, `freebsd`, `rhel.6`.</span></span> <span data-ttu-id="ca59c-197">（适用于 .NET Core 2.1 及更高版本。）</span><span class="sxs-lookup"><span data-stu-id="ca59c-197">(Valid for .NET Core 2.1 and later.)</span></span>

  <span data-ttu-id="ca59c-198">此参数是可选的，只应在需要替代脚本检测到的操作系统时使用。</span><span class="sxs-lookup"><span data-stu-id="ca59c-198">The parameter is optional and should only be used when it's required to override the operating system that is detected by the script.</span></span>

- **`-SharedRuntime|--shared-runtime`**

  > [!NOTE]
  > <span data-ttu-id="ca59c-199">此参数已过时，可能会在将来版本的脚本中删除。</span><span class="sxs-lookup"><span data-stu-id="ca59c-199">This parameter is obsolete and may be removed in a future version of the script.</span></span> <span data-ttu-id="ca59c-200">建议的替代项为 `-Runtime|--runtime` 选项。</span><span class="sxs-lookup"><span data-stu-id="ca59c-200">The recommended alternative is the `-Runtime|--runtime` option.</span></span>

  <span data-ttu-id="ca59c-201">仅安装共享运行时位，而非整个 SDK。</span><span class="sxs-lookup"><span data-stu-id="ca59c-201">Installs just the shared runtime bits, not the entire SDK.</span></span> <span data-ttu-id="ca59c-202">此选项等效于指定 `-Runtime|--runtime dotnet`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-202">This option is equivalent to specifying `-Runtime|--runtime dotnet`.</span></span>

- **`-SkipNonVersionedFiles|--skip-non-versioned-files`**

  <span data-ttu-id="ca59c-203">跳过安装未添加版本的文件，例如 dotnet.exe（如果它们已经存在）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-203">Skips installing non-versioned files, such as *dotnet.exe*, if they already exist.</span></span>

- **`-UncachedFeed|--uncached-feed`**

  <span data-ttu-id="ca59c-204">允许更改此安装程序使用的未缓存源的 URL。</span><span class="sxs-lookup"><span data-stu-id="ca59c-204">Allows changing the URL for the uncached feed used by this installer.</span></span> <span data-ttu-id="ca59c-205">建议不要更改该值。</span><span class="sxs-lookup"><span data-stu-id="ca59c-205">We recommended that you don't change this value.</span></span>

- **`-Verbose|--verbose`**

  <span data-ttu-id="ca59c-206">显示诊断信息。</span><span class="sxs-lookup"><span data-stu-id="ca59c-206">Displays diagnostics information.</span></span>

- **`-Version|--version <VERSION>`**

  <span data-ttu-id="ca59c-207">表示特定的内部版本。</span><span class="sxs-lookup"><span data-stu-id="ca59c-207">Represents a specific build version.</span></span> <span data-ttu-id="ca59c-208">可能的值为：</span><span class="sxs-lookup"><span data-stu-id="ca59c-208">The possible values are:</span></span>

  - <span data-ttu-id="ca59c-209">`latest` - 频道上的最新内部版本（与 `-Channel` 选项结合使用）。</span><span class="sxs-lookup"><span data-stu-id="ca59c-209">`latest` - Latest build on the channel (used with the `-Channel` option).</span></span>
  - <span data-ttu-id="ca59c-210">由三部分组成的版本，采用 X.Y.Z 格式，表示特定的内部版本；取代 `-Channel` 选项。</span><span class="sxs-lookup"><span data-stu-id="ca59c-210">Three-part version in X.Y.Z format representing a specific build version; supersedes the `-Channel` option.</span></span> <span data-ttu-id="ca59c-211">例如：`2.0.0-preview2-006120`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-211">For example: `2.0.0-preview2-006120`.</span></span>

  <span data-ttu-id="ca59c-212">如果没有指定，`-Version` 默认值为 `latest`。</span><span class="sxs-lookup"><span data-stu-id="ca59c-212">If not specified, `-Version` defaults to `latest`.</span></span>

## <a name="examples"></a><span data-ttu-id="ca59c-213">示例</span><span class="sxs-lookup"><span data-stu-id="ca59c-213">Examples</span></span>

- <span data-ttu-id="ca59c-214">将最新的长期支持 (LTS) 版本安装到默认位置：</span><span class="sxs-lookup"><span data-stu-id="ca59c-214">Install the latest long-term supported (LTS) version to the default location:</span></span>

  <span data-ttu-id="ca59c-215">Windows：</span><span class="sxs-lookup"><span data-stu-id="ca59c-215">Windows:</span></span>

  ```powershell
  ./dotnet-install.ps1 -Channel LTS
  ```

  <span data-ttu-id="ca59c-216">macOS/Linux：</span><span class="sxs-lookup"><span data-stu-id="ca59c-216">macOS/Linux:</span></span>

  ```bash
  ./dotnet-install.sh --channel LTS
  ```

- <span data-ttu-id="ca59c-217">将 3.1 通道中的最新版本安装到指定位置：</span><span class="sxs-lookup"><span data-stu-id="ca59c-217">Install the latest version from 3.1 channel to the specified location:</span></span>

  <span data-ttu-id="ca59c-218">Windows：</span><span class="sxs-lookup"><span data-stu-id="ca59c-218">Windows:</span></span>

  ```powershell
  ./dotnet-install.ps1 -Channel 3.1 -InstallDir C:\cli
  ```

  <span data-ttu-id="ca59c-219">macOS/Linux：</span><span class="sxs-lookup"><span data-stu-id="ca59c-219">macOS/Linux:</span></span>

  ```bash
  ./dotnet-install.sh --channel 3.1 --install-dir ~/cli
  ```

- <span data-ttu-id="ca59c-220">安装 3.0.0 版共享运行时：</span><span class="sxs-lookup"><span data-stu-id="ca59c-220">Install the 3.0.0 version of the shared runtime:</span></span>

  <span data-ttu-id="ca59c-221">Windows：</span><span class="sxs-lookup"><span data-stu-id="ca59c-221">Windows:</span></span>

  ```powershell
  ./dotnet-install.ps1 -Runtime dotnet -Version 3.0.0
  ```

  <span data-ttu-id="ca59c-222">macOS/Linux：</span><span class="sxs-lookup"><span data-stu-id="ca59c-222">macOS/Linux:</span></span>

  ```bash
  ./dotnet-install.sh --runtime dotnet --version 3.0.0
  ```

- <span data-ttu-id="ca59c-223">获取脚本并在公司代理后面安装 2.1.2 版本（仅限 Windows）：</span><span class="sxs-lookup"><span data-stu-id="ca59c-223">Obtain script and install the 2.1.2 version behind a corporate proxy (Windows only):</span></span>

  ```powershell
  Invoke-WebRequest 'https://dot.net/v1/dotnet-install.ps1' -Proxy $env:HTTP_PROXY -ProxyUseDefaultCredentials -OutFile 'dotnet-install.ps1';
  ./dotnet-install.ps1 -InstallDir '~/.dotnet' -Version '2.1.2' -ProxyAddress $env:HTTP_PROXY -ProxyUseDefaultCredentials;
  ```

- <span data-ttu-id="ca59c-224">获取脚本并安装 .NET CLI 单行式命令示例：</span><span class="sxs-lookup"><span data-stu-id="ca59c-224">Obtain script and install .NET CLI one-liner examples:</span></span>

  <span data-ttu-id="ca59c-225">Windows：</span><span class="sxs-lookup"><span data-stu-id="ca59c-225">Windows:</span></span>

  ```powershell
  # Run a separate PowerShell process because the script calls exit, so it will end the current PowerShell session.
  &powershell -NoProfile -ExecutionPolicy unrestricted -Command "[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12; &([scriptblock]::Create((Invoke-WebRequest -UseBasicParsing 'https://dot.net/v1/dotnet-install.ps1'))) <additional install-script args>"
  ```

  <span data-ttu-id="ca59c-226">macOS/Linux：</span><span class="sxs-lookup"><span data-stu-id="ca59c-226">macOS/Linux:</span></span>

  ```bash
  curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin <additional install-script args>
  ```

## <a name="see-also"></a><span data-ttu-id="ca59c-227">请参阅</span><span class="sxs-lookup"><span data-stu-id="ca59c-227">See also</span></span>

- [<span data-ttu-id="ca59c-228">.NET 版本</span><span class="sxs-lookup"><span data-stu-id="ca59c-228">.NET releases</span></span>](https://github.com/dotnet/core/releases)
- [<span data-ttu-id="ca59c-229">.NET 运行时和 SDK 下载存档</span><span class="sxs-lookup"><span data-stu-id="ca59c-229">.NET Runtime and SDK download archive</span></span>](https://github.com/dotnet/core/blob/main/release-notes/download-archive.md)
