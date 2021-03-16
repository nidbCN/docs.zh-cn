---
title: dotnet tool install 命令
description: Dotnet tool install 命令在计算机上安装指定的 .NET 工具。
ms.date: 02/14/2020
ms.openlocfilehash: b04e7fd24edce5d5da67cdd83fbea797118689b4
ms.sourcegitcommit: bdbf6472de867a0a11aaa5b9384a2506c24f27d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102206476"
---
# <a name="dotnet-tool-install"></a><span data-ttu-id="4a176-103">dotnet tool install</span><span class="sxs-lookup"><span data-stu-id="4a176-103">dotnet tool install</span></span>

<span data-ttu-id="4a176-104">本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="4a176-104">**This article applies to:** ✔️ .NET Core 2.1 SDK and later versions</span></span>

## <a name="name"></a><span data-ttu-id="4a176-105">“属性”</span><span class="sxs-lookup"><span data-stu-id="4a176-105">Name</span></span>

<span data-ttu-id="4a176-106">`dotnet tool install` - 在计算机上安装指定的 [.NET 工具](global-tools.md)。</span><span class="sxs-lookup"><span data-stu-id="4a176-106">`dotnet tool install` - Installs the specified [.NET tool](global-tools.md) on your machine.</span></span>

## <a name="synopsis"></a><span data-ttu-id="4a176-107">摘要</span><span class="sxs-lookup"><span data-stu-id="4a176-107">Synopsis</span></span>

```dotnetcli
dotnet tool install <PACKAGE_NAME> -g|--global
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--framework <FRAMEWORK>] [-v|--verbosity <LEVEL>]
    [--version <VERSION_NUMBER>]

dotnet tool install <PACKAGE_NAME> --tool-path <PATH>
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--framework <FRAMEWORK>] [-v|--verbosity <LEVEL>]
    [--version <VERSION_NUMBER>]

dotnet tool install <PACKAGE_NAME>
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--framework <FRAMEWORK>] [-v|--verbosity <LEVEL>]
    [--version <VERSION_NUMBER>]

dotnet tool install -h|--help
```

## <a name="description"></a><span data-ttu-id="4a176-108">描述</span><span class="sxs-lookup"><span data-stu-id="4a176-108">Description</span></span>

<span data-ttu-id="4a176-109">`dotnet tool install` 命令提供一种在计算机上安装 .NET 工具的方法。</span><span class="sxs-lookup"><span data-stu-id="4a176-109">The `dotnet tool install` command provides a way for you to install .NET tools on your machine.</span></span> <span data-ttu-id="4a176-110">若要使用命令，请指定以下安装选项之一：</span><span class="sxs-lookup"><span data-stu-id="4a176-110">To use the command, you specify one of the following installation options:</span></span>

* <span data-ttu-id="4a176-111">若要在默认位置中安装全局工具，请使用 `--global` 选项。</span><span class="sxs-lookup"><span data-stu-id="4a176-111">To install a global tool in the default location, use the `--global` option.</span></span>
* <span data-ttu-id="4a176-112">若要在自定义位置中安装全局工具，请使用 `--tool-path` 选项。</span><span class="sxs-lookup"><span data-stu-id="4a176-112">To install a global tool in a custom location,  use the `--tool-path` option.</span></span>
* <span data-ttu-id="4a176-113">若要安装本地工具，请省略 `--global` 和 `--tool-path` 选项。</span><span class="sxs-lookup"><span data-stu-id="4a176-113">To install a local tool, omit the `--global` and `--tool-path` options.</span></span>

<span data-ttu-id="4a176-114">**本地工具从 .NET Core SDK 3.0 开始可用。**</span><span class="sxs-lookup"><span data-stu-id="4a176-114">**Local tools are available starting with .NET Core SDK 3.0.**</span></span>

<span data-ttu-id="4a176-115">指定 `-g` 或 `--global` 选项时，全局工具默认安装在以下目录中：</span><span class="sxs-lookup"><span data-stu-id="4a176-115">Global tools are installed in the following directories by default when you specify the `-g` or `--global` option:</span></span>

| <span data-ttu-id="4a176-116">(OS)</span><span class="sxs-lookup"><span data-stu-id="4a176-116">OS</span></span>          | <span data-ttu-id="4a176-117">路径</span><span class="sxs-lookup"><span data-stu-id="4a176-117">Path</span></span>                          |
|-------------|-------------------------------|
| <span data-ttu-id="4a176-118">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="4a176-118">Linux/macOS</span></span> | `$HOME/.dotnet/tools`         |
| <span data-ttu-id="4a176-119">Windows</span><span class="sxs-lookup"><span data-stu-id="4a176-119">Windows</span></span>     | `%USERPROFILE%\.dotnet\tools` |

<span data-ttu-id="4a176-120">本地工具将添加到当前目录下 .config 目录中的 dotnet-tools.json 文件中。</span><span class="sxs-lookup"><span data-stu-id="4a176-120">Local tools are added to a *dotnet-tools.json* file in a *.config* directory under the current directory.</span></span> <span data-ttu-id="4a176-121">如果清单文件尚不存在，请通过运行以下命令来创建它：</span><span class="sxs-lookup"><span data-stu-id="4a176-121">If a manifest file doesn't exist yet, create it by running the following command:</span></span>

```dotnetcli
dotnet new tool-manifest
```

<span data-ttu-id="4a176-122">有关详细信息，请参阅[安装本地工具](global-tools.md#install-a-local-tool)。</span><span class="sxs-lookup"><span data-stu-id="4a176-122">For more information, see [Install a local tool](global-tools.md#install-a-local-tool).</span></span>

## <a name="arguments"></a><span data-ttu-id="4a176-123">自变量</span><span class="sxs-lookup"><span data-stu-id="4a176-123">Arguments</span></span>

- **`PACKAGE_NAME`**

  <span data-ttu-id="4a176-124">包含要安装的 .NET 工具的 NuGet 包的名称/ID。</span><span class="sxs-lookup"><span data-stu-id="4a176-124">Name/ID of the NuGet package that contains the .NET tool to install.</span></span>

## <a name="options"></a><span data-ttu-id="4a176-125">选项</span><span class="sxs-lookup"><span data-stu-id="4a176-125">Options</span></span>

- **`--add-source <SOURCE>`**

  <span data-ttu-id="4a176-126">添加安装过程中要使用的其他 NuGet 包源。</span><span class="sxs-lookup"><span data-stu-id="4a176-126">Adds an additional NuGet package source to use during installation.</span></span> <span data-ttu-id="4a176-127">系统会并行访问这些源，而不是按某种优先级顺序依次访问。</span><span class="sxs-lookup"><span data-stu-id="4a176-127">Feeds are accessed in parallel, not sequentially in some order of precedence.</span></span> <span data-ttu-id="4a176-128">如果同一个包和版本在多个源中，则选取速度最快的源。</span><span class="sxs-lookup"><span data-stu-id="4a176-128">If the same package and version is in multiple feeds, the fastest feed wins.</span></span> <span data-ttu-id="4a176-129">有关详细信息，请查看[安装 NuGet 包时会发生什么情况？](/nuget/concepts/package-installation-process)。</span><span class="sxs-lookup"><span data-stu-id="4a176-129">For more information, see [What happens when a NuGet package is installed?](/nuget/concepts/package-installation-process).</span></span>

- **`--configfile <FILE>`**

  <span data-ttu-id="4a176-130">要使用的 NuGet 配置 (nuget.config) 文件。</span><span class="sxs-lookup"><span data-stu-id="4a176-130">The NuGet configuration (*nuget.config*) file to use.</span></span>

- **`--framework <FRAMEWORK>`**

  <span data-ttu-id="4a176-131">指定要安装工具的[目标框架](../../standard/frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="4a176-131">Specifies the [target framework](../../standard/frameworks.md) to install the tool for.</span></span> <span data-ttu-id="4a176-132">默认情况下，.NET SDK 尝试选择最合适的目标框架。</span><span class="sxs-lookup"><span data-stu-id="4a176-132">By default, the .NET SDK tries to choose the most appropriate target framework.</span></span>

- **`-g|--global`**

  <span data-ttu-id="4a176-133">指定安装是用户范围的。</span><span class="sxs-lookup"><span data-stu-id="4a176-133">Specifies that the installation is user wide.</span></span> <span data-ttu-id="4a176-134">不能与 `--tool-path` 选项一起使用。</span><span class="sxs-lookup"><span data-stu-id="4a176-134">Can't be combined with the `--tool-path` option.</span></span> <span data-ttu-id="4a176-135">省略 `--global` 和 `--tool-path` 指定本地工具安装。</span><span class="sxs-lookup"><span data-stu-id="4a176-135">Omitting both `--global` and `--tool-path` specifies a local tool installation.</span></span>

- **`-h|--help`**

  <span data-ttu-id="4a176-136">打印出有关命令的简短帮助。</span><span class="sxs-lookup"><span data-stu-id="4a176-136">Prints out a short help for the command.</span></span>

- **`--tool-path <PATH>`**

  <span data-ttu-id="4a176-137">指定全局工具的安装位置。</span><span class="sxs-lookup"><span data-stu-id="4a176-137">Specifies the location where to install the Global Tool.</span></span> <span data-ttu-id="4a176-138">路径可以是绝对的，也可以是相对的。</span><span class="sxs-lookup"><span data-stu-id="4a176-138">PATH can be absolute or relative.</span></span> <span data-ttu-id="4a176-139">如果路径不存在，命令会尝试创建它。</span><span class="sxs-lookup"><span data-stu-id="4a176-139">If PATH doesn't exist, the command tries to create it.</span></span> <span data-ttu-id="4a176-140">省略 `--global` 和 `--tool-path` 指定本地工具安装。</span><span class="sxs-lookup"><span data-stu-id="4a176-140">Omitting both `--global` and `--tool-path` specifies a local tool installation.</span></span>

- **`-v|--verbosity <LEVEL>`**

  <span data-ttu-id="4a176-141">设置命令的详细级别。</span><span class="sxs-lookup"><span data-stu-id="4a176-141">Sets the verbosity level of the command.</span></span> <span data-ttu-id="4a176-142">允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。</span><span class="sxs-lookup"><span data-stu-id="4a176-142">Allowed values are `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]`, and `diag[nostic]`.</span></span>

- **`--version <VERSION_NUMBER>`**

  <span data-ttu-id="4a176-143">要安装的工具版本。</span><span class="sxs-lookup"><span data-stu-id="4a176-143">The version of the tool to install.</span></span> <span data-ttu-id="4a176-144">默认情况下，安装最新的稳定包版本。</span><span class="sxs-lookup"><span data-stu-id="4a176-144">By default, the latest stable package version is installed.</span></span> <span data-ttu-id="4a176-145">使用此选项安装工具的预览版或较旧版本。</span><span class="sxs-lookup"><span data-stu-id="4a176-145">Use this option to install preview or older versions of the tool.</span></span>

## <a name="examples"></a><span data-ttu-id="4a176-146">示例</span><span class="sxs-lookup"><span data-stu-id="4a176-146">Examples</span></span>

- **`dotnet tool install -g dotnetsay`**

  <span data-ttu-id="4a176-147">在默认位置中安装 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。</span><span class="sxs-lookup"><span data-stu-id="4a176-147">Installs [dotnetsay](https://www.nuget.org/packages/dotnetsay/) as a global tool in the default location.</span></span>

- **`dotnet tool install dotnetsay --tool-path c:\global-tools`**

  <span data-ttu-id="4a176-148">在特定 Windows 目录中安装 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。</span><span class="sxs-lookup"><span data-stu-id="4a176-148">Installs [dotnetsay](https://www.nuget.org/packages/dotnetsay/) as a global tool in a specific Windows directory.</span></span>

- **`dotnet tool install dotnetsay --tool-path ~/bin`**

  <span data-ttu-id="4a176-149">在特定 Linux/macOS 目录中安装 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。</span><span class="sxs-lookup"><span data-stu-id="4a176-149">Installs [dotnetsay](https://www.nuget.org/packages/dotnetsay/) as a global tool in a specific Linux/macOS directory.</span></span>

- **`dotnet tool install -g dotnetsay --version 2.0.0`**

  <span data-ttu-id="4a176-150">安装 2.0.0 版的 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。</span><span class="sxs-lookup"><span data-stu-id="4a176-150">Installs version 2.0.0 of [dotnetsay](https://www.nuget.org/packages/dotnetsay/) as a global tool.</span></span>

- **`dotnet tool install dotnetsay`**

  <span data-ttu-id="4a176-151">在当前目录中安装 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 本地工具。</span><span class="sxs-lookup"><span data-stu-id="4a176-151">Installs [dotnetsay](https://www.nuget.org/packages/dotnetsay/) as a local tool for the current directory.</span></span>

## <a name="see-also"></a><span data-ttu-id="4a176-152">请参阅</span><span class="sxs-lookup"><span data-stu-id="4a176-152">See also</span></span>

- [<span data-ttu-id="4a176-153">.NET 工具</span><span class="sxs-lookup"><span data-stu-id="4a176-153">.NET tools</span></span>](global-tools.md)
- [<span data-ttu-id="4a176-154">教程：使用 .NET CLI 安装和使用 .NET 全局工具</span><span class="sxs-lookup"><span data-stu-id="4a176-154">Tutorial: Install and use a .NET global tool using the .NET CLI</span></span>](global-tools-how-to-use.md)
- [<span data-ttu-id="4a176-155">教程：使用 .NET CLI 安装和使用 .NET 本地工具</span><span class="sxs-lookup"><span data-stu-id="4a176-155">Tutorial: Install and use a .NET local tool using the .NET CLI</span></span>](local-tools-how-to-use.md)
