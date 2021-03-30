---
title: 如何安装 ML.NET 命令行接口 (CLI) 工具
description: 了解如何安装、升级、降级和卸载 ML.NET 命令行接口 (CLI) 工具。
ms.date: 06/08/2020
ms.custom: mlnet-tooling
ms.openlocfilehash: a2fdaa06c1602a2c3986b2f034cd1840e39d16a3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876768"
---
# <a name="how-to-install-the-mlnet-command-line-interface-cli-tool"></a><span data-ttu-id="0c314-103">如何安装 ML.NET 命令行接口 (CLI) 工具</span><span class="sxs-lookup"><span data-stu-id="0c314-103">How to install the ML.NET Command-Line Interface (CLI) tool</span></span>

<span data-ttu-id="0c314-104">了解如何在 Windows、Mac 或 Linux 上安装 ML.NET CLI（命令行接口）。</span><span class="sxs-lookup"><span data-stu-id="0c314-104">Learn how to install the ML.NET CLI (command-line interface) on Windows, Mac, or Linux.</span></span>

<span data-ttu-id="0c314-105">ML.NET CLI 使用自动化机器学习 (AutoML) 和训练数据集生成质量良好的 ML.NET 模型和源代码。</span><span class="sxs-lookup"><span data-stu-id="0c314-105">The ML.NET CLI generates good quality ML.NET models and source code using automated machine learning (AutoML) and a training dataset.</span></span>

> [!NOTE]
> <span data-ttu-id="0c314-106">本主题涉及目前处于预览状态的 ML.NET CLI 和 ML.NET AutoML，且材料可能会有所变化。</span><span class="sxs-lookup"><span data-stu-id="0c314-106">This topic refers to ML.NET CLI and ML.NET AutoML, which are currently in Preview, and material may be subject to change.</span></span>

## <a name="pre-requisites"></a><span data-ttu-id="0c314-107">先决条件</span><span class="sxs-lookup"><span data-stu-id="0c314-107">Pre-requisites</span></span>

- [<span data-ttu-id="0c314-108">.NET Core 3.1 SDK</span><span class="sxs-lookup"><span data-stu-id="0c314-108">.NET Core 3.1 SDK</span></span>](https://dotnet.microsoft.com/download/dotnet/3.1)

- <span data-ttu-id="0c314-109">（可选）[Visual Studio 2019](https://visualstudio.microsoft.com/vs/)</span><span class="sxs-lookup"><span data-stu-id="0c314-109">(Optional) [Visual Studio 2019](https://visualstudio.microsoft.com/vs/)</span></span>

<span data-ttu-id="0c314-110">可以使用 Visual Studio 按 `F5` 键或 `dotnet run` (.NET Core CLI) 运行生成的 C# 代码项目。</span><span class="sxs-lookup"><span data-stu-id="0c314-110">You can run the generated C# code projects with Visual Studio by pressing the `F5` key or with `dotnet run` (.NET Core CLI).</span></span>

<span data-ttu-id="0c314-111">注意：如果在安装 .NET Core SDK 后，`dotnet tool` 命令不起作用，请从 Windows 注销并再次登录。</span><span class="sxs-lookup"><span data-stu-id="0c314-111">Note: If after installing .NET Core SDK the `dotnet tool` command is not working, sign out from Windows and sign in again.</span></span>

## <a name="install"></a><span data-ttu-id="0c314-112">安装</span><span class="sxs-lookup"><span data-stu-id="0c314-112">Install</span></span>

<span data-ttu-id="0c314-113">ML.NET CLI 的安装方式与任何其他 dotnet 全局工具一样。</span><span class="sxs-lookup"><span data-stu-id="0c314-113">The ML.NET CLI is installed like any other dotnet Global Tool.</span></span> <span data-ttu-id="0c314-114">使用 `dotnet tool install` .NET Core CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="0c314-114">You use the `dotnet tool install` .NET Core CLI command.</span></span>

<span data-ttu-id="0c314-115">以下示例显示如何在默认 NuGet 源位置安装 ML.NET CLI：</span><span class="sxs-lookup"><span data-stu-id="0c314-115">The following example shows how to install the ML.NET CLI in the default NuGet feed location:</span></span>

```dotnetcli
dotnet tool install -g mlnet
```

<span data-ttu-id="0c314-116">如果无法安装该工具（即，如果它在默认 NuGet 源中不可用），则会显示错误消息。</span><span class="sxs-lookup"><span data-stu-id="0c314-116">If the tool can't be installed (that is, if it is not available at the default NuGet feed), error messages are displayed.</span></span> <span data-ttu-id="0c314-117">检查所需源是否正在被检查。</span><span class="sxs-lookup"><span data-stu-id="0c314-117">Check that the feeds you expected are being checked.</span></span>

<span data-ttu-id="0c314-118">如果安装成功，会出现一条消息，显示用于调用工具的命令以及所安装的版本，类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="0c314-118">If installation is successful, a message is displayed showing the command used to call the tool and the version installed, similar to the following example:</span></span>

```console
You can invoke the tool using the following command: mlnet
Tool 'mlnet' (version 'X.X.X') was successfully installed.
```

<span data-ttu-id="0c314-119">可以通过键入以下命令来确认安装是否成功：</span><span class="sxs-lookup"><span data-stu-id="0c314-119">You can confirm the installation was successful by typing the following command:</span></span>

```console
mlnet
```

<span data-ttu-id="0c314-120">应看到 mlnet 工具的可用命令帮助，例如“classification”命令。</span><span class="sxs-lookup"><span data-stu-id="0c314-120">You should see the help for available commands for the mlnet tool such as the 'classification' command.</span></span>

## <a name="install-a-specific-release-version"></a><span data-ttu-id="0c314-121">安装特定版本</span><span class="sxs-lookup"><span data-stu-id="0c314-121">Install a specific release version</span></span>

<span data-ttu-id="0c314-122">如果想安装工具的预发布版本或特定版本，可以采用以下格式指定[框架](../../standard/frameworks.md)：</span><span class="sxs-lookup"><span data-stu-id="0c314-122">If you're trying to install a pre-release version or a specific version of the tool, you can specify the [framework](../../standard/frameworks.md) using the following format:</span></span>

```dotnetcli
dotnet tool install -g mlnet --framework <FRAMEWORK>
```

<span data-ttu-id="0c314-123">还可以通过键入以下命令来检查包是否已正确安装：</span><span class="sxs-lookup"><span data-stu-id="0c314-123">You can also check if the package is properly installed by typing the following command:</span></span>

```dotnetcli
dotnet tool list -g
```

## <a name="uninstall-the-cli-package"></a><span data-ttu-id="0c314-124">卸载 CLI 包</span><span class="sxs-lookup"><span data-stu-id="0c314-124">Uninstall the CLI package</span></span>

<span data-ttu-id="0c314-125">键入以下命令，从本地计算机卸载包：</span><span class="sxs-lookup"><span data-stu-id="0c314-125">Type the following command to uninstall the package from your local machine:</span></span>

```dotnetcli
dotnet tool uninstall mlnet -g
```

## <a name="update-the-cli-package"></a><span data-ttu-id="0c314-126">更新 CLI 包</span><span class="sxs-lookup"><span data-stu-id="0c314-126">Update the CLI package</span></span>

<span data-ttu-id="0c314-127">键入以下命令，从本地计算机更新包：</span><span class="sxs-lookup"><span data-stu-id="0c314-127">Type the following command to update the package from your local machine:</span></span>

```dotnetcli
dotnet tool update -g mlnet
```

## <a name="set-up-cli-suggestions-tab-based-auto-completion"></a><span data-ttu-id="0c314-128">设置 CLI 建议（Tab 自动补全）</span><span class="sxs-lookup"><span data-stu-id="0c314-128">Set up CLI suggestions (tab-based auto-completion)</span></span>

<span data-ttu-id="0c314-129">由于 ML.NET CLI 基于 `System.CommandLine`，它具有对 Tab 补全的内置支持。</span><span class="sxs-lookup"><span data-stu-id="0c314-129">Since the ML.NET CLI is based on `System.CommandLine`, it has built-in support for tab completion.</span></span>

<span data-ttu-id="0c314-130">Tab 自动补全工作原理的示例如以下动画所示：</span><span class="sxs-lookup"><span data-stu-id="0c314-130">An example of how tab auto completion works is shown in the following animation:</span></span>

![图像](./media/cli-tab-completion.gif)

<span data-ttu-id="0c314-132">“Tab 自动补全”（参数建议）适用于 *Windows PowerShell* 和 *macOS/Linux bash*，但它不适用于 *Windows CMD*。</span><span class="sxs-lookup"><span data-stu-id="0c314-132">'Tab-based auto-completion' (parameter suggestions) works on *Windows PowerShell* and *macOS/Linux bash* but it won't work on *Windows CMD*.</span></span>

<span data-ttu-id="0c314-133">若要启用它，在当前预览版本中，最终用户必须为每个 shell 执行一次相关步骤，如下所述。</span><span class="sxs-lookup"><span data-stu-id="0c314-133">To enable it, in the current preview version, the end user has to take a few steps once per shell, outlined below.</span></span> <span data-ttu-id="0c314-134">完成此操作后，补全将适用于使用 `System.CommandLine` 编写的所有应用，例如 ML.NET CLI。</span><span class="sxs-lookup"><span data-stu-id="0c314-134">Once this is done, completions will work for all apps written using `System.CommandLine` such as the ML.NET CLI.</span></span>

<span data-ttu-id="0c314-135">在想要启用补全的计算机上，需要执行两个操作。</span><span class="sxs-lookup"><span data-stu-id="0c314-135">On the machine where you'd like to enable completion, you'll need to do two things.</span></span>

1. <span data-ttu-id="0c314-136">通过运行以下命令安装 `dotnet-suggest` 全局工具：</span><span class="sxs-lookup"><span data-stu-id="0c314-136">Install the `dotnet-suggest` global tool by running the following command:</span></span>

    ```dotnetcli
    dotnet tool install dotnet-suggest -g
    ```

2. <span data-ttu-id="0c314-137">将适当的填充码脚本添加到 shell 配置文件中。</span><span class="sxs-lookup"><span data-stu-id="0c314-137">Add the appropriate shim script to your shell profile.</span></span> <span data-ttu-id="0c314-138">可能必须创建一个 shell 配置文件。</span><span class="sxs-lookup"><span data-stu-id="0c314-138">You may have to create a shell profile file.</span></span> <span data-ttu-id="0c314-139">填充码脚本会将补全请求从 shell 转发到 `dotnet-suggest` 工具，其会委托到相应的基于 `System.CommandLine` 的应用。</span><span class="sxs-lookup"><span data-stu-id="0c314-139">The shim script will forward completion requests from your shell to the `dotnet-suggest` tool, which delegates to the appropriate `System.CommandLine`-based app.</span></span>

    - <span data-ttu-id="0c314-140">对于 bash，将 [dotnet-suggest-shim.bash](https://github.com/dotnet/System.CommandLine/blob/main/src/System.CommandLine.Suggest/dotnet-suggest-shim.bash) 的内容添加到 `~/.bash_profile`。</span><span class="sxs-lookup"><span data-stu-id="0c314-140">For bash, add the contents of [dotnet-suggest-shim.bash](https://github.com/dotnet/System.CommandLine/blob/main/src/System.CommandLine.Suggest/dotnet-suggest-shim.bash) to `~/.bash_profile`.</span></span>

    - <span data-ttu-id="0c314-141">对于 PowerShell，将 [dotnet-suggest-shim.ps1](https://github.com/dotnet/System.CommandLine/blob/main/src/System.CommandLine.Suggest/dotnet-suggest-shim.ps1) 的内容添加到 PowerShell 配置文件中。</span><span class="sxs-lookup"><span data-stu-id="0c314-141">For PowerShell, add the contents of [dotnet-suggest-shim.ps1](https://github.com/dotnet/System.CommandLine/blob/main/src/System.CommandLine.Suggest/dotnet-suggest-shim.ps1) to your PowerShell profile.</span></span> <span data-ttu-id="0c314-142">可以通过在控制台中运行以下命令来查找 PowerShell 配置文件的预期路径：</span><span class="sxs-lookup"><span data-stu-id="0c314-142">You can find the expected path to your PowerShell profile by running the following command in your console:</span></span>

    ```console
    echo $profile
    ```

<span data-ttu-id="0c314-143">（对于其他 shell，[查找](https://github.com/dotnet/System.CommandLine/issues?q=is%3Aissue+is%3Aopen+label%3A%22shell+suggestion%22)或创建[问题](https://github.com/dotnet/System.CommandLine/issues)。）</span><span class="sxs-lookup"><span data-stu-id="0c314-143">(For other shells, [look for](https://github.com/dotnet/System.CommandLine/issues?q=is%3Aissue+is%3Aopen+label%3A%22shell+suggestion%22) or open an [issue](https://github.com/dotnet/System.CommandLine/issues).)</span></span>

## <a name="installation-directory"></a><span data-ttu-id="0c314-144">安装目录</span><span class="sxs-lookup"><span data-stu-id="0c314-144">Installation directory</span></span>

<span data-ttu-id="0c314-145">ML.NET CLI 可安装在默认目录或特定位置。</span><span class="sxs-lookup"><span data-stu-id="0c314-145">The ML.NET CLI can be installed in the default directory or in a specific location.</span></span> <span data-ttu-id="0c314-146">默认目录为：</span><span class="sxs-lookup"><span data-stu-id="0c314-146">The default directories are:</span></span>

| <span data-ttu-id="0c314-147">(OS)</span><span class="sxs-lookup"><span data-stu-id="0c314-147">OS</span></span>          | <span data-ttu-id="0c314-148">路径</span><span class="sxs-lookup"><span data-stu-id="0c314-148">Path</span></span>                          |
|-------------|-------------------------------|
| <span data-ttu-id="0c314-149">Linux/macOS</span><span class="sxs-lookup"><span data-stu-id="0c314-149">Linux/macOS</span></span> | `$HOME/.dotnet/tools`         |
| <span data-ttu-id="0c314-150">Windows</span><span class="sxs-lookup"><span data-stu-id="0c314-150">Windows</span></span>     | `%USERPROFILE%\.dotnet\tools` |

<span data-ttu-id="0c314-151">这些位置在首次运行 SDK 时添加到用户的路径，所以可以直接调用安装在这里的全局工具。</span><span class="sxs-lookup"><span data-stu-id="0c314-151">These locations are added to the user's path when the SDK is first run, so Global Tools installed there can be called directly.</span></span>

<span data-ttu-id="0c314-152">注意：全局工具特定于用户，而不针对计算机全局。</span><span class="sxs-lookup"><span data-stu-id="0c314-152">Note: the Global Tools are user-specific, not machine global.</span></span> <span data-ttu-id="0c314-153">特定于用户的意思是无法在一台计算机上安装所有用户都可用的全局工具。</span><span class="sxs-lookup"><span data-stu-id="0c314-153">Being user-specific means you cannot install a Global Tool that is available to all users of the machine.</span></span> <span data-ttu-id="0c314-154">只有安装了该工具的用户配置文件才能使用它。</span><span class="sxs-lookup"><span data-stu-id="0c314-154">The tool is only available for each user profile where the tool was installed.</span></span>

<span data-ttu-id="0c314-155">也可在特定目录安装全局工具。</span><span class="sxs-lookup"><span data-stu-id="0c314-155">Global Tools can also be installed in a specific directory.</span></span> <span data-ttu-id="0c314-156">如果在特定目录安装全局工具，用户必须确保命令是可用的，方法是将该目录添加至路径、使用指定目录调用命令或从特定目录调用该工具。</span><span class="sxs-lookup"><span data-stu-id="0c314-156">When installed in a specific directory, the user must ensure the command is available, by including that directory in the path, by calling the command with the directory specified, or calling the tool from within the specified directory.</span></span>
<span data-ttu-id="0c314-157">在这种情况下，.NET Core CLI 不将此地址自动添加至 PATH 环境变量。</span><span class="sxs-lookup"><span data-stu-id="0c314-157">In this case, the .NET Core CLI doesn't add this location automatically to the PATH environment variable.</span></span>

## <a name="see-also"></a><span data-ttu-id="0c314-158">请参阅</span><span class="sxs-lookup"><span data-stu-id="0c314-158">See also</span></span>

- [<span data-ttu-id="0c314-159">ML.NET CLI 概述</span><span class="sxs-lookup"><span data-stu-id="0c314-159">ML.NET CLI overview</span></span>](../automate-training-with-cli.md)
- [<span data-ttu-id="0c314-160">教程：使用 ML.NET CLI 分析情绪</span><span class="sxs-lookup"><span data-stu-id="0c314-160">Tutorial: Analyze sentiment with the ML.NET CLI</span></span>](../tutorials/sentiment-analysis-cli.md)
- [<span data-ttu-id="0c314-161">ML.NET CLI auto-train 命令参考指南</span><span class="sxs-lookup"><span data-stu-id="0c314-161">ML.NET CLI auto-train command reference guide</span></span>](../reference/ml-net-cli-reference.md)
- [<span data-ttu-id="0c314-162">ML.NET CLI 中的遥测</span><span class="sxs-lookup"><span data-stu-id="0c314-162">Telemetry in ML.NET CLI</span></span>](../resources/ml-net-cli-telemetry.md)
