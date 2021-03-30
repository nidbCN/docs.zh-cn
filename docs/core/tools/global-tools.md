---
title: .NET 工具
description: 如何安装、使用、更新和删除 .NET 工具。 包括全局工具、工具路径工具和本地工具。
author: KathleenDollard
ms.topic: how-to
ms.date: 02/12/2020
ms.openlocfilehash: d4fca6b52b1444cf73efc49a6c50294cc5f6015b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874818"
---
# <a name="how-to-manage-net-tools"></a><span data-ttu-id="db43b-104">如何管理 .NET 工具</span><span class="sxs-lookup"><span data-stu-id="db43b-104">How to manage .NET tools</span></span>

<span data-ttu-id="db43b-105">本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="db43b-105">**This article applies to:** ✔️ .NET Core 2.1 SDK and later versions</span></span>

<span data-ttu-id="db43b-106">.NET 工具是一种特殊的 NuGet 包，其中包含控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="db43b-106">A .NET tool is a special NuGet package that contains a console application.</span></span> <span data-ttu-id="db43b-107">可以通过以下方式在计算机上安装该工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-107">A tool can be installed on your machine in the following ways:</span></span>

* <span data-ttu-id="db43b-108">作为全局工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-108">As a global tool.</span></span>

  <span data-ttu-id="db43b-109">工具二进制文件安装在添加到 PATH 环境变量的默认目录中。</span><span class="sxs-lookup"><span data-stu-id="db43b-109">The tool binaries are installed in a default directory that is added to the PATH environment variable.</span></span> <span data-ttu-id="db43b-110">无需指定工具位置即可从计算机上的任何目录调用该工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-110">You can invoke the tool from any directory on the machine without specifying its location.</span></span> <span data-ttu-id="db43b-111">工具的一个版本用于计算机上的所有目录。</span><span class="sxs-lookup"><span data-stu-id="db43b-111">One version of a tool is used for all directories on the machine.</span></span>

* <span data-ttu-id="db43b-112">作为自定义位置中的全局工具（也称为工具路径工具）。</span><span class="sxs-lookup"><span data-stu-id="db43b-112">As a global tool in a custom location (also known as a tool-path tool).</span></span>

  <span data-ttu-id="db43b-113">工具二进制文件安装在你指定的位置中。</span><span class="sxs-lookup"><span data-stu-id="db43b-113">The tool binaries are installed in a location that you specify.</span></span> <span data-ttu-id="db43b-114">可以从安装目录调用该工具，也可以通过提供具有命令名称的目录或将目录添加到 PATH 环境变量来调用该工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-114">You can invoke the tool from the installation directory or by providing the directory with the command name or by adding the directory to the PATH environment variable.</span></span> <span data-ttu-id="db43b-115">工具的一个版本用于计算机上的所有目录。</span><span class="sxs-lookup"><span data-stu-id="db43b-115">One version of a tool is used for all directories on the machine.</span></span>

* <span data-ttu-id="db43b-116">作为本地工具（适用于 .NET Core SDK 3.0 及更高版本）。</span><span class="sxs-lookup"><span data-stu-id="db43b-116">As a local tool (applies to .NET Core SDK 3.0 and later).</span></span>

  <span data-ttu-id="db43b-117">工具二进制文件安装在默认目录中。</span><span class="sxs-lookup"><span data-stu-id="db43b-117">The tool binaries are installed in a default directory.</span></span> <span data-ttu-id="db43b-118">可以从安装目录或其任何子目录调用该工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-118">You invoke the tool from the installation directory or any of its subdirectories.</span></span> <span data-ttu-id="db43b-119">不同目录可以使用同一工具的不同版本。</span><span class="sxs-lookup"><span data-stu-id="db43b-119">Different directories can use different versions of the same tool.</span></span>
  
  <span data-ttu-id="db43b-120">.NET CLI 使用清单文件跟踪哪些工具作为本地工具安装到目录。</span><span class="sxs-lookup"><span data-stu-id="db43b-120">The .NET CLI uses manifest files to keep track of which tools are installed as local to a directory.</span></span> <span data-ttu-id="db43b-121">将清单文件保存到源代码存储库的根目录中后，参与者可以克隆存储库并调用用于安装清单文件中列出的所有工具的单个 .NET CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="db43b-121">When the manifest file is saved in the root directory of a source code repository, a contributor can clone the repository and invoke a single .NET CLI command that installs all of the tools listed in the manifest files.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="db43b-122">.NET 工具在完全信任环境中运行。</span><span class="sxs-lookup"><span data-stu-id="db43b-122">.NET tools run in full trust.</span></span> <span data-ttu-id="db43b-123">除非你信任工具作者，否则请勿安装 .NET 工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-123">Do not install a .NET tool unless you trust the author.</span></span>

## <a name="find-a-tool"></a><span data-ttu-id="db43b-124">查找工具</span><span class="sxs-lookup"><span data-stu-id="db43b-124">Find a tool</span></span>

<span data-ttu-id="db43b-125">以下是查找工具的一些方法：</span><span class="sxs-lookup"><span data-stu-id="db43b-125">Here are some ways to find tools:</span></span>

* <span data-ttu-id="db43b-126">使用 [dotnet tool search](dotnet-tool-search.md) 命令查找发布到 NuGet.org 的工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-126">Use the [dotnet tool search](dotnet-tool-search.md) command to find a tool that is published to NuGet.org.</span></span>
* <span data-ttu-id="db43b-127">使用“.NET 工具”包类型筛选器搜索 [NuGet](https://www.nuget.org) 网站。</span><span class="sxs-lookup"><span data-stu-id="db43b-127">Search the [NuGet](https://www.nuget.org) website by using the ".NET tool" package type filter.</span></span> <span data-ttu-id="db43b-128">有关详细信息，请参阅[查找和选择包](/nuget/consume-packages/finding-and-choosing-packages)。</span><span class="sxs-lookup"><span data-stu-id="db43b-128">For more information, see [Finding and choosing packages](/nuget/consume-packages/finding-and-choosing-packages).</span></span>
* <span data-ttu-id="db43b-129">在 [dotnet/aspnetcore GitHub 存储库的工具目录](https://github.com/dotnet/aspnetcore/tree/main/src/Tools)中查看 ASP.NET Core 团队创建的工具的源代码。</span><span class="sxs-lookup"><span data-stu-id="db43b-129">See the source code for the tools created by the ASP.NET Core team in the [Tools directory of the dotnet/aspnetcore GitHub repository](https://github.com/dotnet/aspnetcore/tree/main/src/Tools).</span></span>
* <span data-ttu-id="db43b-130">在 [.NET 诊断工具](../diagnostics/index.md#net-core-diagnostic-global-tools)中了解诊断工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-130">Learn about diagnostic tools at [.NET diagnostic tools](../diagnostics/index.md#net-core-diagnostic-global-tools).</span></span>

## <a name="check-the-author-and-statistics"></a><span data-ttu-id="db43b-131">查看作者和统计信息</span><span class="sxs-lookup"><span data-stu-id="db43b-131">Check the author and statistics</span></span>

<span data-ttu-id="db43b-132">由于 .NET 工具在完全信任环境中运行，并且全局工具已添加到 PATH 环境变量，因此它们的功能非常强大。</span><span class="sxs-lookup"><span data-stu-id="db43b-132">Since .NET tools run in full trust, and global tools are added to the PATH environment variable, they can be very powerful.</span></span> <span data-ttu-id="db43b-133">请勿下载不信任的人提供的工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-133">Don't download tools from people you don't trust.</span></span>

<span data-ttu-id="db43b-134">如果该工具在 NuGet 中托管，可以通过搜索该工具来查看作者和统计信息。</span><span class="sxs-lookup"><span data-stu-id="db43b-134">If the tool is hosted on NuGet, you can check the author and statistics by searching for the tool.</span></span>

## <a name="install-a-global-tool"></a><span data-ttu-id="db43b-135">安装全局工具</span><span class="sxs-lookup"><span data-stu-id="db43b-135">Install a global tool</span></span>

<span data-ttu-id="db43b-136">若要将工具作为全局工具安装，请使用 [dotnet tool install](dotnet-tool-install.md) 的 `-g` 或 `--global` 选项，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="db43b-136">To install a tool as a global tool, use the `-g` or `--global` option of [dotnet tool install](dotnet-tool-install.md), as shown in the following example:</span></span>

```dotnetcli
dotnet tool install -g dotnetsay
```

<span data-ttu-id="db43b-137">输出显示用于调用该工具和已安装的版本的命令，类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="db43b-137">The output shows the command used to invoke the tool and the version installed, similar to the following example:</span></span>

```output
You can invoke the tool using the following command: dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
```

<span data-ttu-id="db43b-138">工具二进制文件的默认位置取决于操作系统：</span><span class="sxs-lookup"><span data-stu-id="db43b-138">The default location for a tool's binaries depends on the operating system:</span></span>

| <span data-ttu-id="db43b-139">(OS)</span><span class="sxs-lookup"><span data-stu-id="db43b-139">OS</span></span>          | <span data-ttu-id="db43b-140">路径</span><span class="sxs-lookup"><span data-stu-id="db43b-140">Path</span></span>                          |
|-------------|-------------------------------|
| <span data-ttu-id="db43b-141">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="db43b-141">Linux/macOS</span></span> | `$HOME/.dotnet/tools`         |
| <span data-ttu-id="db43b-142">Windows</span><span class="sxs-lookup"><span data-stu-id="db43b-142">Windows</span></span>     | `%USERPROFILE%\.dotnet\tools` |

<span data-ttu-id="db43b-143">首次运行 SDK 时，会将此位置添加到用户的路径，因此，无需指定工具位置即可从任何目录调用全局工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-143">This location is added to the user's path when the SDK is first run, so global tools can be invoked from any directory without specifying the tool location.</span></span>

<span data-ttu-id="db43b-144">工具访问特定于用户，而不针对计算机全局。</span><span class="sxs-lookup"><span data-stu-id="db43b-144">Tool access is user-specific, not machine global.</span></span> <span data-ttu-id="db43b-145">全局工具仅适用于安装了该工具的用户。</span><span class="sxs-lookup"><span data-stu-id="db43b-145">A global tool is only available to the user that installed the tool.</span></span>

### <a name="install-a-global-tool-in-a-custom-location"></a><span data-ttu-id="db43b-146">安装自定义位置中的全局工具</span><span class="sxs-lookup"><span data-stu-id="db43b-146">Install a global tool in a custom location</span></span>

<span data-ttu-id="db43b-147">若要将工具作为自定义位置中的全局工具安装，请使用 [dotnet tool install](dotnet-tool-install.md) 的 `--tool-path` 选项，如以下示例中所示。</span><span class="sxs-lookup"><span data-stu-id="db43b-147">To install a tool as a global tool in a custom location, use the `--tool-path` option of [dotnet tool install](dotnet-tool-install.md), as shown in the following examples.</span></span>

<span data-ttu-id="db43b-148">在 Windows 上：</span><span class="sxs-lookup"><span data-stu-id="db43b-148">On Windows:</span></span>

```dotnetcli
dotnet tool install dotnetsay --tool-path c:\dotnet-tools
```

<span data-ttu-id="db43b-149">在 Linux 或 macOS 上：</span><span class="sxs-lookup"><span data-stu-id="db43b-149">On Linux or macOS:</span></span>

```dotnetcli
dotnet tool install dotnetsay --tool-path ~/bin
```

<span data-ttu-id="db43b-150">.NET SDK 不将此位置自动添加至 PATH 环境变量。</span><span class="sxs-lookup"><span data-stu-id="db43b-150">The .NET SDK doesn't add this location automatically to the PATH environment variable.</span></span> <span data-ttu-id="db43b-151">若要[调用工具路径工具](#invoke-a-tool-path-tool)，必须确保可使用以下方法之一来调用命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-151">To [invoke a tool-path tool](#invoke-a-tool-path-tool), you have to make sure the command is available by using one of the following methods:</span></span>

* <span data-ttu-id="db43b-152">将安装目录添加到 PATH 环境变量。</span><span class="sxs-lookup"><span data-stu-id="db43b-152">Add the installation directory to the PATH environment variable.</span></span>
* <span data-ttu-id="db43b-153">调用该工具时，指定该工具的完整路径。</span><span class="sxs-lookup"><span data-stu-id="db43b-153">Specify the full path to the tool when you invoke it.</span></span>
* <span data-ttu-id="db43b-154">从安装目录调用该工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-154">Invoke the tool from within the installation directory.</span></span>

## <a name="install-a-local-tool"></a><span data-ttu-id="db43b-155">安装本地工具</span><span class="sxs-lookup"><span data-stu-id="db43b-155">Install a local tool</span></span>

<span data-ttu-id="db43b-156">**适用于 .NET Core 3.0 SDK 及更高版本。**</span><span class="sxs-lookup"><span data-stu-id="db43b-156">**Applies to .NET Core 3.0 SDK and later.**</span></span>

<span data-ttu-id="db43b-157">若要安装仅用于本地访问的工具（对于当前目录和子目录），必须将其添加到工具清单文件。</span><span class="sxs-lookup"><span data-stu-id="db43b-157">To install a tool for local access only (for the current directory and subdirectories), it has to be added to a tool manifest file.</span></span> <span data-ttu-id="db43b-158">若要创建工具清单文件，请运行 `dotnet new tool-manifest` 命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-158">To create a tool manifest file, run the `dotnet new tool-manifest` command:</span></span>

```dotnetcli
dotnet new tool-manifest
```

<span data-ttu-id="db43b-159">此命令在“.config”  目录下创建一个名为“dotnet-tools.json”  的清单文件。</span><span class="sxs-lookup"><span data-stu-id="db43b-159">This command creates a manifest file named *dotnet-tools.json* under the *.config* directory.</span></span> <span data-ttu-id="db43b-160">若要将本地工具添加到清单文件，请使用 [dotnet tool install](dotnet-tool-install.md) 命令并省略 `--global` 和 `--tool-path` 选项，如以下示例中所示  ：</span><span class="sxs-lookup"><span data-stu-id="db43b-160">To add a local tool to the manifest file, use the [dotnet tool install](dotnet-tool-install.md) command and **omit** the `--global` and `--tool-path` options, as shown in the following example:</span></span>

```dotnetcli
dotnet tool install dotnetsay
```

<span data-ttu-id="db43b-161">命令输出显示新安装的工具所在的清单文件，类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="db43b-161">The command output shows which manifest file the newly installed tool is in, similar to the following example:</span></span>

```console
You can invoke the tool from this directory using the following command:
dotnet tool run dotnetsay
Tool 'dotnetsay' (version '2.1.4') was successfully installed.
Entry is added to the manifest file /home/name/botsay/.config/dotnet-tools.json.
```

<span data-ttu-id="db43b-162">以下示例显示安装了两个本地工具的清单文件：</span><span class="sxs-lookup"><span data-stu-id="db43b-162">The following example shows a manifest file with two local tools installed:</span></span>

```json
{
  "version": 1,
  "isRoot": true,
  "tools": {
    "botsay": {
      "version": "1.0.0",
      "commands": [
        "botsay"
      ]
    },
    "dotnetsay": {
      "version": "2.1.3",
      "commands": [
        "dotnetsay"
      ]
    }
  }
}
```

<span data-ttu-id="db43b-163">通常将本地工具添加到存储库的根目录。</span><span class="sxs-lookup"><span data-stu-id="db43b-163">You typically add a local tool to the root directory of the repository.</span></span> <span data-ttu-id="db43b-164">将清单文件签入到存储库后，从存储库中签出代码的开发人员会获得最新的清单文件。</span><span class="sxs-lookup"><span data-stu-id="db43b-164">After you check in the manifest file to the repository, developers who check out code from the repository get the latest manifest file.</span></span> <span data-ttu-id="db43b-165">若要安装清单文件中列出的所有工具，请运行 `dotnet tool restore` 命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-165">To install all of the tools listed in the manifest file, they run the `dotnet tool restore` command:</span></span>

```dotnetcli
dotnet tool restore
```

<span data-ttu-id="db43b-166">输出表明还原了哪些工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-166">The output indicates which tools were restored:</span></span>

```console
Tool 'botsay' (version '1.0.0') was restored. Available commands: botsay
Tool 'dotnetsay' (version '2.1.3') was restored. Available commands: dotnetsay
Restore was successful.
```

## <a name="install-a-specific-tool-version"></a><span data-ttu-id="db43b-167">安装特定工具版本</span><span class="sxs-lookup"><span data-stu-id="db43b-167">Install a specific tool version</span></span>

<span data-ttu-id="db43b-168">若要安装工具的预发布版本或特定版本，请使用 `--version` 选项指定版本号，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="db43b-168">To install a pre-release version or a specific version of a tool, specify the version number by using the `--version` option, as shown in the following example:</span></span>

```dotnetcli
dotnet tool install dotnetsay --version 2.1.3
```

## <a name="use-a-tool"></a><span data-ttu-id="db43b-169">使用工具</span><span class="sxs-lookup"><span data-stu-id="db43b-169">Use a tool</span></span>

<span data-ttu-id="db43b-170">用于调用工具的命令可能不同于安装的包的名称。</span><span class="sxs-lookup"><span data-stu-id="db43b-170">The command that you use to invoke a tool may be different from the name of the package that you install.</span></span> <span data-ttu-id="db43b-171">若要显示计算机上目前安装的所有工具，请使用 [dotnet tool list](dotnet-tool-list.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-171">To display all of the tools currently installed on the machine for the current user, use the [dotnet tool list](dotnet-tool-list.md) command:</span></span>

```dotnetcli
dotnet tool list
```

<span data-ttu-id="db43b-172">输出显示每个工具的版本和命令，类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="db43b-172">The output shows each tool's version and command, similar to the following example:</span></span>

```console
Package Id      Version      Commands       Manifest
-------------------------------------------------------------------------------------------
botsay          1.0.0        botsay         /home/name/repository/.config/dotnet-tools.json
dotnetsay       2.1.3        dotnetsay      /home/name/repository/.config/dotnet-tools.json
```

<span data-ttu-id="db43b-173">如本示例中所示，列表显示了本地工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-173">As shown in this example, the list shows local tools.</span></span> <span data-ttu-id="db43b-174">若要查看全局工具，请使用 `--global` 选项，若要查看工具路径工具，请使用 `--tool-path` 选项。</span><span class="sxs-lookup"><span data-stu-id="db43b-174">To see global tools, use the `--global` option, and to see tool-path tools, use the `--tool-path` option.</span></span>

### <a name="invoke-a-global-tool"></a><span data-ttu-id="db43b-175">调用全局工具</span><span class="sxs-lookup"><span data-stu-id="db43b-175">Invoke a global tool</span></span>

<span data-ttu-id="db43b-176">对于全局工具，请单独使用工具命令。</span><span class="sxs-lookup"><span data-stu-id="db43b-176">For global tools, use the tool command by itself.</span></span> <span data-ttu-id="db43b-177">例如，如果命令为 `dotnetsay` 或 `dotnet-doc`，则可以使用以下命令调用该工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-177">For example, if the command is `dotnetsay` or `dotnet-doc`, that's what you use to invoke the command:</span></span>

```console
dotnetsay
dotnet-doc
```

<span data-ttu-id="db43b-178">如果命令以前缀 `dotnet-` 开头，则调用该工具的另一种方法是使用 `dotnet` 命令并省略工具命令前缀。</span><span class="sxs-lookup"><span data-stu-id="db43b-178">If the command begins with the prefix `dotnet-`, an alternative way to invoke the tool is to use the `dotnet` command and omit the tool command prefix.</span></span> <span data-ttu-id="db43b-179">例如，如果命令为 `dotnet-doc`，则可以使用以下命令调用该工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-179">For example, if the command is `dotnet-doc`, the following command invokes the tool:</span></span>

```dotnetcli
dotnet doc
```

<span data-ttu-id="db43b-180">但是，在以下情况下，不能使用 `dotnet` 命令来调用全局工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-180">However, in the following scenario you can't use the `dotnet` command to invoke a global tool:</span></span>

* <span data-ttu-id="db43b-181">全局工具和本地工具具有以 `dotnet-` 为前缀的相同命令。</span><span class="sxs-lookup"><span data-stu-id="db43b-181">A global tool and a local tool have the same command prefixed by `dotnet-`.</span></span>
* <span data-ttu-id="db43b-182">你希望从本地工具范围内的目录调用全局工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-182">You want to invoke the global tool from a directory that is in scope for the local tool.</span></span>

<span data-ttu-id="db43b-183">在这种情况下，`dotnet doc` 和 `dotnet dotnet-doc` 调用本地工具。</span><span class="sxs-lookup"><span data-stu-id="db43b-183">In this scenario, `dotnet doc` and `dotnet dotnet-doc` invoke the local tool.</span></span> <span data-ttu-id="db43b-184">若要调用全局工具，请单独使用命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-184">To invoke the global tool, use the command by itself:</span></span>

```dotnetcli
dotnet-doc
```

### <a name="invoke-a-tool-path-tool"></a><span data-ttu-id="db43b-185">调用工具路径工具</span><span class="sxs-lookup"><span data-stu-id="db43b-185">Invoke a tool-path tool</span></span>

<span data-ttu-id="db43b-186">若要调用使用 `tool-path` 选项安装的全局工具，请确保该命令可用，如[本文前面](#install-a-global-tool-in-a-custom-location)所述。</span><span class="sxs-lookup"><span data-stu-id="db43b-186">To invoke a global tool that is installed by using the `tool-path` option, make sure the command is available, as explained [earlier in this article](#install-a-global-tool-in-a-custom-location).</span></span>

### <a name="invoke-a-local-tool"></a><span data-ttu-id="db43b-187">调用本地工具</span><span class="sxs-lookup"><span data-stu-id="db43b-187">Invoke a local tool</span></span>

<span data-ttu-id="db43b-188">若要调用本地工具，必须从安装目录使用 `dotnet` 命令。</span><span class="sxs-lookup"><span data-stu-id="db43b-188">To invoke a local tool, you have to use the `dotnet` command from within the installation directory.</span></span> <span data-ttu-id="db43b-189">可以使用长格式 (`dotnet tool run <COMMAND_NAME>`) 或短格式 (`dotnet <COMMAND_NAME>`)，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="db43b-189">You can use the long form (`dotnet tool run <COMMAND_NAME>`) or the short form (`dotnet <COMMAND_NAME>`), as shown in the following examples:</span></span>

```dotnetcli
dotnet tool run dotnetsay
dotnet dotnetsay
```

<span data-ttu-id="db43b-190">如果命令以 `dotnet-` 为前缀，则可以在调用该工具时包括或省略前缀。</span><span class="sxs-lookup"><span data-stu-id="db43b-190">If the command is prefixed by `dotnet-`, you can include or omit the prefix when you invoke the tool.</span></span> <span data-ttu-id="db43b-191">例如，如果命令为 `dotnet-doc`，则可以使用以下任何示例调用本地工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-191">For example, if the command is `dotnet-doc`, any of the following examples invokes the local tool:</span></span>

```dotnetcli
dotnet tool run dotnet-doc
dotnet dotnet-doc
dotnet doc
```

## <a name="update-a-tool"></a><span data-ttu-id="db43b-192">更新工具</span><span class="sxs-lookup"><span data-stu-id="db43b-192">Update a tool</span></span>

<span data-ttu-id="db43b-193">更新工具涉及卸载该工具并重新安装它的最新稳定版。</span><span class="sxs-lookup"><span data-stu-id="db43b-193">Updating a tool involves uninstalling and reinstalling it with the latest stable version.</span></span> <span data-ttu-id="db43b-194">若要更新工具，请使用具有用于安装该工具的相同选项的 [dotnet tool update](dotnet-tool-update.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-194">To update a tool, use the [dotnet tool update](dotnet-tool-update.md) command with the same option that you used to install the tool:</span></span>

```dotnetcli
dotnet tool update --global <packagename>
dotnet tool update --tool-path <packagename>
dotnet tool update <packagename>
```

<span data-ttu-id="db43b-195">对于本地工具，SDK 通过在当前目录和父目录中查找来查找包含包 ID 的第一个清单文件。</span><span class="sxs-lookup"><span data-stu-id="db43b-195">For a local tool, the SDK finds the first manifest file that contains the package ID by looking in the current directory and parent directories.</span></span> <span data-ttu-id="db43b-196">如果任何清单文件中都没有此类包 ID，SDK 会将新条目添加到最近的清单文件。</span><span class="sxs-lookup"><span data-stu-id="db43b-196">If there is no such package ID in any manifest file, the SDK adds a new entry to the closest manifest file.</span></span>

## <a name="uninstall-a-tool"></a><span data-ttu-id="db43b-197">卸载工具</span><span class="sxs-lookup"><span data-stu-id="db43b-197">Uninstall a tool</span></span>

<span data-ttu-id="db43b-198">使用具有用于安装该工具的相同选项的 [dotnet tool uninstall](dotnet-tool-uninstall.md) 命令删除工具：</span><span class="sxs-lookup"><span data-stu-id="db43b-198">Remove a tool by using the [dotnet tool uninstall](dotnet-tool-uninstall.md) command with the same option that you used to install the tool:</span></span>

```dotnetcli
dotnet tool uninstall --global <packagename>
dotnet tool uninstall --tool-path <packagename>
dotnet tool uninstall <packagename>
```

<span data-ttu-id="db43b-199">对于本地工具，SDK 通过在当前目录和父目录中查找来查找包含包 ID 的第一个清单文件。</span><span class="sxs-lookup"><span data-stu-id="db43b-199">For a local tool, the SDK finds the first manifest file that contains the package ID by looking in the current directory and parent directories.</span></span>

## <a name="get-help-and-troubleshoot"></a><span data-ttu-id="db43b-200">获取帮助和疑难解答</span><span class="sxs-lookup"><span data-stu-id="db43b-200">Get help and troubleshoot</span></span>

<span data-ttu-id="db43b-201">若要获取可用 `dotnet tool` 命令的列表，请输入以下命令：</span><span class="sxs-lookup"><span data-stu-id="db43b-201">To get a list of available `dotnet tool` commands, enter the following command:</span></span>

```dotnetcli
dotnet tool --help
```

<span data-ttu-id="db43b-202">若要获取工具使用说明，请输入以下命令之一，或访问工具的网站：</span><span class="sxs-lookup"><span data-stu-id="db43b-202">To get tool usage instructions, enter one of the following commands or see the tool's website:</span></span>

```dotnetcli
<command> --help
dotnet <command> --help
```

<span data-ttu-id="db43b-203">如果工具无法安装或运行，请参阅[排查 .NET 工具使用问题](troubleshoot-usage-issues.md)。</span><span class="sxs-lookup"><span data-stu-id="db43b-203">If a tool fails to install or run, see [Troubleshoot .NET tool usage issues](troubleshoot-usage-issues.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="db43b-204">请参阅</span><span class="sxs-lookup"><span data-stu-id="db43b-204">See also</span></span>

- [<span data-ttu-id="db43b-205">教程：使用 .NET CLI 创建 .NET 工具</span><span class="sxs-lookup"><span data-stu-id="db43b-205">Tutorial: Create a .NET tool using the .NET CLI</span></span>](global-tools-how-to-create.md)
- [<span data-ttu-id="db43b-206">教程：使用 .NET CLI 安装和使用 .NET 全局工具</span><span class="sxs-lookup"><span data-stu-id="db43b-206">Tutorial: Install and use a .NET global tool using the .NET CLI</span></span>](global-tools-how-to-use.md)
- [<span data-ttu-id="db43b-207">教程：使用 .NET CLI 安装和使用 .NET 本地工具</span><span class="sxs-lookup"><span data-stu-id="db43b-207">Tutorial: Install and use a .NET local tool using the .NET CLI</span></span>](local-tools-how-to-use.md)
