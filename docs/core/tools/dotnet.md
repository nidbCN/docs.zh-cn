---
title: dotnet 命令
description: 了解 dotnet 命令（.NET CLI 的通用驱动程序）及其用法。
ms.date: 11/11/2020
ms.openlocfilehash: 33c5f9d22166b818f5c860c4f4632d359f686919
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874532"
---
# <a name="dotnet-command"></a><span data-ttu-id="65d97-103">dotnet 命令</span><span class="sxs-lookup"><span data-stu-id="65d97-103">dotnet command</span></span>

<span data-ttu-id="65d97-104">本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="65d97-104">**This article applies to:** ✔️ .NET Core 2.1 SDK and later versions</span></span>

## <a name="name"></a><span data-ttu-id="65d97-105">“属性”</span><span class="sxs-lookup"><span data-stu-id="65d97-105">Name</span></span>

<span data-ttu-id="65d97-106">`dotnet` - .NET CLI 的通用驱动程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-106">`dotnet` - The generic driver for the .NET CLI.</span></span>

## <a name="synopsis"></a><span data-ttu-id="65d97-107">摘要</span><span class="sxs-lookup"><span data-stu-id="65d97-107">Synopsis</span></span>

<span data-ttu-id="65d97-108">获取有关可用命令和环境的信息：</span><span class="sxs-lookup"><span data-stu-id="65d97-108">To get information about the available commands and the environment:</span></span>

```dotnetcli
dotnet [--version] [--info] [--list-runtimes] [--list-sdks]

dotnet -h|--help
```

<span data-ttu-id="65d97-109">运行命令（需要 SDK 安装）：</span><span class="sxs-lookup"><span data-stu-id="65d97-109">To run a command (requires SDK installation):</span></span>

```dotnetcli
dotnet <COMMAND> [-d|--diagnostics] [-h|--help] [--verbosity <LEVEL>]
    [command-options] [arguments]
```

<span data-ttu-id="65d97-110">运行应用程序：</span><span class="sxs-lookup"><span data-stu-id="65d97-110">To run an application:</span></span>

```dotnetcli
dotnet [--additionalprobingpath <PATH>] [--additional-deps <PATH>]
    [--fx-version <VERSION>]  [--roll-forward <SETTING>]
    <PATH_TO_APPLICATION> [arguments]

dotnet exec [--additionalprobingpath] [--additional-deps <PATH>]
    [--fx-version <VERSION>]  [--roll-forward <SETTING>]
    <PATH_TO_APPLICATION> [arguments]
```

<span data-ttu-id="65d97-111">`--roll-forward` 自 .NET Core 3.x 起可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-111">`--roll-forward` is available since .NET Core 3.x.</span></span> <span data-ttu-id="65d97-112">使用 .NET Core 2.x 的 `--roll-forward-on-no-candidate-fx`。</span><span class="sxs-lookup"><span data-stu-id="65d97-112">Use `--roll-forward-on-no-candidate-fx` for .NET Core 2.x.</span></span>

## <a name="description"></a><span data-ttu-id="65d97-113">描述</span><span class="sxs-lookup"><span data-stu-id="65d97-113">Description</span></span>

<span data-ttu-id="65d97-114">`dotnet` 命令有两个函数：</span><span class="sxs-lookup"><span data-stu-id="65d97-114">The `dotnet` command has two functions:</span></span>

- <span data-ttu-id="65d97-115">它提供了用于处理 .NET 项目的命令。</span><span class="sxs-lookup"><span data-stu-id="65d97-115">It provides commands for working with .NET projects.</span></span>

  <span data-ttu-id="65d97-116">例如，[`dotnet build`](dotnet-build.md) 生成项目。</span><span class="sxs-lookup"><span data-stu-id="65d97-116">For example, [`dotnet build`](dotnet-build.md) builds a project.</span></span> <span data-ttu-id="65d97-117">每个命令定义自己的选项和参数。</span><span class="sxs-lookup"><span data-stu-id="65d97-117">Each command defines its own options and arguments.</span></span> <span data-ttu-id="65d97-118">所有命令都支持 `--help` 选项，用于打印有关如何使用命令的简短文档。</span><span class="sxs-lookup"><span data-stu-id="65d97-118">All commands support the `--help` option for printing out brief documentation about how to use the command.</span></span>

- <span data-ttu-id="65d97-119">它运行 .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-119">It runs .NET applications.</span></span>

  <span data-ttu-id="65d97-120">指定应用程序 `.dll` 文件的路径以运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-120">You specify the path to an application `.dll` file to run the application.</span></span>  <span data-ttu-id="65d97-121">运行应用程序即意味着找到并执行入口点，对于控制台应用，入口点是 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="65d97-121">To run the application means to find and execute the entry point, which in the case of console apps is the `Main` method.</span></span> <span data-ttu-id="65d97-122">例如，`dotnet myapp.dll` 运行 `myapp` 应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-122">For example, `dotnet myapp.dll` runs the `myapp` application.</span></span> <span data-ttu-id="65d97-123">若要了解部署选项，请参阅 [.NET 应用程序部署](../deploying/index.md)。</span><span class="sxs-lookup"><span data-stu-id="65d97-123">See [.NET application deployment](../deploying/index.md) to learn about deployment options.</span></span>

## <a name="options"></a><span data-ttu-id="65d97-124">选项</span><span class="sxs-lookup"><span data-stu-id="65d97-124">Options</span></span>

<span data-ttu-id="65d97-125">`dotnet` 本身有不同的选项，可用于运行命令和运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-125">Different options are available for `dotnet` by itself, for running a command, and for running an application.</span></span>

### <a name="options-for-dotnet-by-itself"></a><span data-ttu-id="65d97-126">dotnet 本身的选项</span><span class="sxs-lookup"><span data-stu-id="65d97-126">Options for dotnet by itself</span></span>

<span data-ttu-id="65d97-127">以下是 `dotnet` 本身的选项。</span><span class="sxs-lookup"><span data-stu-id="65d97-127">The following options are for `dotnet` by itself.</span></span> <span data-ttu-id="65d97-128">例如 `dotnet --info`。</span><span class="sxs-lookup"><span data-stu-id="65d97-128">For example, `dotnet --info`.</span></span> <span data-ttu-id="65d97-129">这些选项打印出有关环境的信息。</span><span class="sxs-lookup"><span data-stu-id="65d97-129">They print out information about the environment.</span></span>

- **`--info`**

  <span data-ttu-id="65d97-130">打印出有关 .NET 安装和计算机环境（如当前操作系统）的详细信息，并提交 .NET 版本的 SHA。</span><span class="sxs-lookup"><span data-stu-id="65d97-130">Prints out detailed information about a .NET installation and the machine environment, such as the current operating system, and commit SHA of the .NET version.</span></span>

- **`--version`**

  <span data-ttu-id="65d97-131">打印出使用中的 .NET SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-131">Prints out the version of the .NET SDK in use.</span></span>

- **`--list-runtimes`**

  <span data-ttu-id="65d97-132">打印出已安装的 .NET 运行时的列表。</span><span class="sxs-lookup"><span data-stu-id="65d97-132">Prints out a list of the installed .NET runtimes.</span></span> <span data-ttu-id="65d97-133">x86 版本的 SDK 只列出 x86 运行时，而 x64 版本的 SDK 只列出 x64 运行时。</span><span class="sxs-lookup"><span data-stu-id="65d97-133">An x86 version of the SDK lists only x86 runtimes, and an x64 version of the SDK lists only x64 runtimes.</span></span>

- **`--list-sdks`**

  <span data-ttu-id="65d97-134">打印出已安装的 .NET SDK 的列表。</span><span class="sxs-lookup"><span data-stu-id="65d97-134">Prints out a list of the installed .NET SDKs.</span></span>

- **`-h|--help`**

  <span data-ttu-id="65d97-135">打印可用命令列表。</span><span class="sxs-lookup"><span data-stu-id="65d97-135">Prints out a list of available commands.</span></span>

### <a name="sdk-options-for-running-a-command"></a><span data-ttu-id="65d97-136">用于运行命令的 SDK 选项</span><span class="sxs-lookup"><span data-stu-id="65d97-136">SDK options for running a command</span></span>

<span data-ttu-id="65d97-137">以下选项适用于使用命令的 `dotnet`。</span><span class="sxs-lookup"><span data-stu-id="65d97-137">The following options are for `dotnet` with a command.</span></span> <span data-ttu-id="65d97-138">例如 `dotnet build --help`。</span><span class="sxs-lookup"><span data-stu-id="65d97-138">For example, `dotnet build --help`.</span></span>

- **`-d|--diagnostics`**

  <span data-ttu-id="65d97-139">启用诊断输出。</span><span class="sxs-lookup"><span data-stu-id="65d97-139">Enables diagnostic output.</span></span>

- **`-v|--verbosity <LEVEL>`**

  <span data-ttu-id="65d97-140">设置命令的详细级别。</span><span class="sxs-lookup"><span data-stu-id="65d97-140">Sets the verbosity level of the command.</span></span> <span data-ttu-id="65d97-141">允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。</span><span class="sxs-lookup"><span data-stu-id="65d97-141">Allowed values are `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]`, and `diag[nostic]`.</span></span> <span data-ttu-id="65d97-142">并非在每个命令中均受支持。</span><span class="sxs-lookup"><span data-stu-id="65d97-142">Not supported in every command.</span></span> <span data-ttu-id="65d97-143">请参阅特定的命令页，确定此选项是否可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-143">See specific command page to determine if this option is available.</span></span>

- **`-h|--help`**

  <span data-ttu-id="65d97-144">打印出给定命令的文档，如 `dotnet build --help`。</span><span class="sxs-lookup"><span data-stu-id="65d97-144">Prints out documentation for a given command, such as `dotnet build --help`.</span></span>

- **`command options`**

  <span data-ttu-id="65d97-145">每个命令定义特定于该命令的选项。</span><span class="sxs-lookup"><span data-stu-id="65d97-145">Each command defines options specific to that command.</span></span> <span data-ttu-id="65d97-146">有关可用选项的列表，请参阅特定命令页。</span><span class="sxs-lookup"><span data-stu-id="65d97-146">See specific command page for a list of available options.</span></span>

### <a name="runtime-options"></a><span data-ttu-id="65d97-147">运行时选项</span><span class="sxs-lookup"><span data-stu-id="65d97-147">Runtime options</span></span>

<span data-ttu-id="65d97-148">`dotnet` 运行应用程序时，可以使用以下选项。</span><span class="sxs-lookup"><span data-stu-id="65d97-148">The following options are available when `dotnet` runs an application.</span></span> <span data-ttu-id="65d97-149">例如 `dotnet myapp.dll --roll-forward Major`。</span><span class="sxs-lookup"><span data-stu-id="65d97-149">For example, `dotnet myapp.dll --roll-forward Major`.</span></span>

- **`--additionalprobingpath <PATH>`**

  <span data-ttu-id="65d97-150">包含要进行探测的探测策略和程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="65d97-150">Path containing probing policy and assemblies to probe.</span></span>

- **`--additional-deps <PATH>`**

  <span data-ttu-id="65d97-151">附加 .deps.json 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="65d97-151">Path to an additional *.deps.json* file.</span></span> <span data-ttu-id="65d97-152">deps.json 文件包含依赖项、编译依赖项和用于解决程序集冲突的版本信息列表。</span><span class="sxs-lookup"><span data-stu-id="65d97-152">A *deps.json* file contains a list of dependencies, compilation dependencies, and version information used to address assembly conflicts.</span></span> <span data-ttu-id="65d97-153">有关详细信息，请参阅 GitHub 上的[运行时配置文件](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md)。</span><span class="sxs-lookup"><span data-stu-id="65d97-153">For more information, see [Runtime Configuration Files](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md) on GitHub.</span></span>

- **`--depsfile <PATH_TO_DEPSFILE>`**

  <span data-ttu-id="65d97-154">deps.json 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="65d97-154">Path to the *deps.json* file.</span></span> <span data-ttu-id="65d97-155">.deps.json 文件是一个配置文件，其中包含有关运行应用程序所需的依赖项的信息。</span><span class="sxs-lookup"><span data-stu-id="65d97-155">A *deps.json* file is a configuration file that contains information about dependencies necessary to run the application.</span></span> <span data-ttu-id="65d97-156">此文件由 .NET SDK 生成。</span><span class="sxs-lookup"><span data-stu-id="65d97-156">This file is generated by the .NET SDK.</span></span>

- **`--runtimeconfig`**

  <span data-ttu-id="65d97-157">runtimeconfig.template.json 文件的路径。</span><span class="sxs-lookup"><span data-stu-id="65d97-157">Path to a *runtimeconfig.json* file.</span></span> <span data-ttu-id="65d97-158">runtimeconfig.template.json 文件是包含运行时设置的配置文件。</span><span class="sxs-lookup"><span data-stu-id="65d97-158">A *runtimeconfig.json* file is a configuration file that contains run-time settings.</span></span> <span data-ttu-id="65d97-159">有关详细信息，请参阅 [.NET 运行时配置设置](../run-time-config/index.md#runtimeconfigjson)。</span><span class="sxs-lookup"><span data-stu-id="65d97-159">For more information, see [.NET run-time configuration settings](../run-time-config/index.md#runtimeconfigjson).</span></span>

- <span data-ttu-id="65d97-160">`--roll-forward <SETTING>` 自 .NET Core SDK 3.0 起可用 。</span><span class="sxs-lookup"><span data-stu-id="65d97-160">**`--roll-forward <SETTING>`** **Available starting with .NET Core SDK 3.0.**</span></span>

  <span data-ttu-id="65d97-161">控制将前滚操作应用于应用的方式。</span><span class="sxs-lookup"><span data-stu-id="65d97-161">Controls how roll forward is applied to the app.</span></span> <span data-ttu-id="65d97-162">`SETTING` 可以为下列值之一。</span><span class="sxs-lookup"><span data-stu-id="65d97-162">The `SETTING` can be one of the following values.</span></span> <span data-ttu-id="65d97-163">如果未指定，则 `Minor` 为默认类型。</span><span class="sxs-lookup"><span data-stu-id="65d97-163">If not specified, `Minor` is the default.</span></span>

  - <span data-ttu-id="65d97-164">`LatestPatch` - 前滚到最高补丁版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-164">`LatestPatch` - Roll forward to the highest patch version.</span></span> <span data-ttu-id="65d97-165">这会禁用次要版本前滚。</span><span class="sxs-lookup"><span data-stu-id="65d97-165">This disables minor version roll forward.</span></span>
  - <span data-ttu-id="65d97-166">`Minor` - 如果缺少所请求的次要版本，则前滚到最低的较高次要版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-166">`Minor` - Roll forward to the lowest higher minor version, if requested minor version is missing.</span></span> <span data-ttu-id="65d97-167">如果存在所请求的次要版本，则使用 LatestPatch 策略。</span><span class="sxs-lookup"><span data-stu-id="65d97-167">If the requested minor version is present, then the LatestPatch policy is used.</span></span>
  - <span data-ttu-id="65d97-168">`Major` - 如果缺少所请求的主要版本，则前滚到最低的较高主要版本和最低的次要版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-168">`Major` - Roll forward to lowest higher major version, and lowest minor version, if requested major version is missing.</span></span> <span data-ttu-id="65d97-169">如果存在所请求的主要版本，则使用 Minor 策略。</span><span class="sxs-lookup"><span data-stu-id="65d97-169">If the requested major version is present, then the Minor policy is used.</span></span>
  - <span data-ttu-id="65d97-170">`LatestMinor` - 即使存在所请求的次要版本，仍前滚到最高次要版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-170">`LatestMinor` - Roll forward to highest minor version, even if requested minor version is present.</span></span> <span data-ttu-id="65d97-171">适用于组件托管方案。</span><span class="sxs-lookup"><span data-stu-id="65d97-171">Intended for component hosting scenarios.</span></span>
  - <span data-ttu-id="65d97-172">`LatestMajor` - 即使存在所请求的主要版本，仍前滚到最高主要版本和最高次要版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-172">`LatestMajor` - Roll forward to highest major and highest minor version, even if requested major is present.</span></span> <span data-ttu-id="65d97-173">适用于组件托管方案。</span><span class="sxs-lookup"><span data-stu-id="65d97-173">Intended for component hosting scenarios.</span></span>
  - <span data-ttu-id="65d97-174">`Disable` - 不前滚。</span><span class="sxs-lookup"><span data-stu-id="65d97-174">`Disable` - Don't roll forward.</span></span> <span data-ttu-id="65d97-175">仅绑定到指定的版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-175">Only bind to specified version.</span></span> <span data-ttu-id="65d97-176">建议不要将此策略用于一般用途，因为它会禁用前滚到最新补丁的功能。</span><span class="sxs-lookup"><span data-stu-id="65d97-176">This policy isn't recommended for general use because it disables the ability to roll forward to the latest patches.</span></span> <span data-ttu-id="65d97-177">该值仅建议用于测试。</span><span class="sxs-lookup"><span data-stu-id="65d97-177">This value is only recommended for testing.</span></span>

  <span data-ttu-id="65d97-178">除 `Disable` 外，所有设置都将使用可用的最高补丁版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-178">With the exception of `Disable`, all settings will use the highest available patch version.</span></span>

  <span data-ttu-id="65d97-179">前滚行为还可以在项目文件属性、运行时配置文件属性和环境变量中进行配置。</span><span class="sxs-lookup"><span data-stu-id="65d97-179">Roll forward behavior can also be configured in a project file property, a run-time configuration file property, and an environment variable.</span></span> <span data-ttu-id="65d97-180">有关详细信息，请参阅[主版本运行时前滚](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="65d97-180">For more information, see [Major-version runtime roll forward](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward).</span></span>

- <span data-ttu-id="65d97-181">`--roll-forward-on-no-candidate-fx <N>` 在 .NET Core 2.x SDK 中可用 。</span><span class="sxs-lookup"><span data-stu-id="65d97-181">**`--roll-forward-on-no-candidate-fx <N>`** **Available in .NET Core 2.x SDK.**</span></span>

  <span data-ttu-id="65d97-182">所需的共享框架不可用时，请定义行为。</span><span class="sxs-lookup"><span data-stu-id="65d97-182">Defines behavior when the required shared framework is not available.</span></span> <span data-ttu-id="65d97-183">`N` 可以是：</span><span class="sxs-lookup"><span data-stu-id="65d97-183">`N` can be:</span></span>

  - <span data-ttu-id="65d97-184">`0` - 禁用次要版本前滚。</span><span class="sxs-lookup"><span data-stu-id="65d97-184">`0` - Disable even minor version roll forward.</span></span>
  - <span data-ttu-id="65d97-185">`1` - 前滚次要版本，但不前滚主版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-185">`1` - Roll forward on minor version, but not on major version.</span></span> <span data-ttu-id="65d97-186">这是默认行为。</span><span class="sxs-lookup"><span data-stu-id="65d97-186">This is the default behavior.</span></span>
  - <span data-ttu-id="65d97-187">`2` - 前滚次要和主版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-187">`2` - Roll forward on minor and major versions.</span></span>

  <span data-ttu-id="65d97-188">有关详细信息，请参阅[前滚](../whats-new/dotnet-core-2-1.md#roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="65d97-188">For more information, see [Roll forward](../whats-new/dotnet-core-2-1.md#roll-forward).</span></span>

  <span data-ttu-id="65d97-189">从 .NET Core 3.0 开始，此选项被 `--roll-forward` 取代，应改为使用此取代项。</span><span class="sxs-lookup"><span data-stu-id="65d97-189">Starting with .NET Core 3.0, this option is superseded by `--roll-forward`, and that option should be used instead.</span></span>

- **`--fx-version <VERSION>`**

  <span data-ttu-id="65d97-190">用于运行应用程序的 .NET 运行时版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-190">Version of the .NET runtime to use to run the application.</span></span>

  <span data-ttu-id="65d97-191">此选项将重写应用程序 `.runtimeconfig.json` 文件中第一个框架引用的版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-191">This option overrides the version of the first framework reference in the application's `.runtimeconfig.json` file.</span></span> <span data-ttu-id="65d97-192">这意味着，仅当只有一个框架引用时，它才会按预期方式工作。</span><span class="sxs-lookup"><span data-stu-id="65d97-192">This means it only works as expected if there's just one framework reference.</span></span> <span data-ttu-id="65d97-193">如果应用程序具有多个框架引用，则使用此选项可能会导致错误。</span><span class="sxs-lookup"><span data-stu-id="65d97-193">If the application has more than one framework reference, using this option may cause errors.</span></span>

## <a name="dotnet-commands"></a><span data-ttu-id="65d97-194">dotnet 命令</span><span class="sxs-lookup"><span data-stu-id="65d97-194">dotnet commands</span></span>

### <a name="general"></a><span data-ttu-id="65d97-195">常规</span><span class="sxs-lookup"><span data-stu-id="65d97-195">General</span></span>

| <span data-ttu-id="65d97-196">命令</span><span class="sxs-lookup"><span data-stu-id="65d97-196">Command</span></span>                                       | <span data-ttu-id="65d97-197">函数</span><span class="sxs-lookup"><span data-stu-id="65d97-197">Function</span></span>                                                            |
| --------------------------------------------- | ------------------------------------------------------------------- |
| [<span data-ttu-id="65d97-198">dotnet build</span><span class="sxs-lookup"><span data-stu-id="65d97-198">dotnet build</span></span>](dotnet-build.md)               | <span data-ttu-id="65d97-199">生成 .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-199">Builds a .NET application.</span></span>                                     |
| [<span data-ttu-id="65d97-200">dotnet build-server</span><span class="sxs-lookup"><span data-stu-id="65d97-200">dotnet build-server</span></span>](dotnet-build-server.md) | <span data-ttu-id="65d97-201">与通过生成启动的服务器进行交互。</span><span class="sxs-lookup"><span data-stu-id="65d97-201">Interacts with servers started by a build.</span></span>                          |
| [<span data-ttu-id="65d97-202">dotnet clean</span><span class="sxs-lookup"><span data-stu-id="65d97-202">dotnet clean</span></span>](dotnet-clean.md)               | <span data-ttu-id="65d97-203">清除生成输出。</span><span class="sxs-lookup"><span data-stu-id="65d97-203">Clean build outputs.</span></span>                                                |
| [<span data-ttu-id="65d97-204">dotnet help</span><span class="sxs-lookup"><span data-stu-id="65d97-204">dotnet help</span></span>](dotnet-help.md)                 | <span data-ttu-id="65d97-205">显示命令更详细的在线文档。</span><span class="sxs-lookup"><span data-stu-id="65d97-205">Shows more detailed documentation online for the command.</span></span>           |
| [<span data-ttu-id="65d97-206">dotnet migrate</span><span class="sxs-lookup"><span data-stu-id="65d97-206">dotnet migrate</span></span>](dotnet-migrate.md)           | <span data-ttu-id="65d97-207">将有效的预览版 2 项目迁移到 .NET Core SDK 1.0 项目。</span><span class="sxs-lookup"><span data-stu-id="65d97-207">Migrates a valid Preview 2 project to a .NET Core SDK 1.0 project.</span></span>  |
| [<span data-ttu-id="65d97-208">dotnet msbuild</span><span class="sxs-lookup"><span data-stu-id="65d97-208">dotnet msbuild</span></span>](dotnet-msbuild.md)           | <span data-ttu-id="65d97-209">提供对 MSBuild 命令行的访问权限。</span><span class="sxs-lookup"><span data-stu-id="65d97-209">Provides access to the MSBuild command line.</span></span>                        |
| [<span data-ttu-id="65d97-210">dotnet new</span><span class="sxs-lookup"><span data-stu-id="65d97-210">dotnet new</span></span>](dotnet-new.md)                   | <span data-ttu-id="65d97-211">为给定的模板初始化 C# 或 F# 项目。</span><span class="sxs-lookup"><span data-stu-id="65d97-211">Initializes a C# or F# project for a given template.</span></span>                |
| [<span data-ttu-id="65d97-212">dotnet pack</span><span class="sxs-lookup"><span data-stu-id="65d97-212">dotnet pack</span></span>](dotnet-pack.md)                 | <span data-ttu-id="65d97-213">创建代码的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="65d97-213">Creates a NuGet package of your code.</span></span>                               |
| [<span data-ttu-id="65d97-214">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="65d97-214">dotnet publish</span></span>](dotnet-publish.md)           | <span data-ttu-id="65d97-215">发布 .NET 依赖于框架或独立应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-215">Publishes a .NET framework-dependent or self-contained application.</span></span> |
| [<span data-ttu-id="65d97-216">dotnet restore</span><span class="sxs-lookup"><span data-stu-id="65d97-216">dotnet restore</span></span>](dotnet-restore.md)           | <span data-ttu-id="65d97-217">还原给定应用程序的依赖项。</span><span class="sxs-lookup"><span data-stu-id="65d97-217">Restores the dependencies for a given application.</span></span>                  |
| [<span data-ttu-id="65d97-218">dotnet run</span><span class="sxs-lookup"><span data-stu-id="65d97-218">dotnet run</span></span>](dotnet-run.md)                   | <span data-ttu-id="65d97-219">从源运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-219">Runs the application from source.</span></span>                                   |
| [<span data-ttu-id="65d97-220">dotnet sln</span><span class="sxs-lookup"><span data-stu-id="65d97-220">dotnet sln</span></span>](dotnet-sln.md)                   | <span data-ttu-id="65d97-221">用于添加、删除和列出解决方案文件中项目的选项。</span><span class="sxs-lookup"><span data-stu-id="65d97-221">Options to add, remove, and list projects in a solution file.</span></span>       |
| [<span data-ttu-id="65d97-222">dotnet store</span><span class="sxs-lookup"><span data-stu-id="65d97-222">dotnet store</span></span>](dotnet-store.md)               | <span data-ttu-id="65d97-223">将程序集存储到运行时包存储区。</span><span class="sxs-lookup"><span data-stu-id="65d97-223">Stores assemblies in the runtime package store.</span></span>                     |
| [<span data-ttu-id="65d97-224">dotnet test</span><span class="sxs-lookup"><span data-stu-id="65d97-224">dotnet test</span></span>](dotnet-test.md)                 | <span data-ttu-id="65d97-225">使用测试运行程序运行测试。</span><span class="sxs-lookup"><span data-stu-id="65d97-225">Runs tests using a test runner.</span></span>                                     |

### <a name="project-references"></a><span data-ttu-id="65d97-226">项目引用</span><span class="sxs-lookup"><span data-stu-id="65d97-226">Project references</span></span>

<span data-ttu-id="65d97-227">命令</span><span class="sxs-lookup"><span data-stu-id="65d97-227">Command</span></span> | <span data-ttu-id="65d97-228">函数</span><span class="sxs-lookup"><span data-stu-id="65d97-228">Function</span></span>
--- | ---
[<span data-ttu-id="65d97-229">dotnet add reference</span><span class="sxs-lookup"><span data-stu-id="65d97-229">dotnet add reference</span></span>](dotnet-add-reference.md) | <span data-ttu-id="65d97-230">添加项目引用。</span><span class="sxs-lookup"><span data-stu-id="65d97-230">Adds a project reference.</span></span>
[<span data-ttu-id="65d97-231">dotnet list reference</span><span class="sxs-lookup"><span data-stu-id="65d97-231">dotnet list reference</span></span>](dotnet-list-reference.md) | <span data-ttu-id="65d97-232">列出项目引用。</span><span class="sxs-lookup"><span data-stu-id="65d97-232">Lists project references.</span></span>
[<span data-ttu-id="65d97-233">dotnet remove reference</span><span class="sxs-lookup"><span data-stu-id="65d97-233">dotnet remove reference</span></span>](dotnet-remove-reference.md) | <span data-ttu-id="65d97-234">删除项目引用。</span><span class="sxs-lookup"><span data-stu-id="65d97-234">Removes a project reference.</span></span>

### <a name="nuget-packages"></a><span data-ttu-id="65d97-235">NuGet 包</span><span class="sxs-lookup"><span data-stu-id="65d97-235">NuGet packages</span></span>

<span data-ttu-id="65d97-236">命令</span><span class="sxs-lookup"><span data-stu-id="65d97-236">Command</span></span> | <span data-ttu-id="65d97-237">函数</span><span class="sxs-lookup"><span data-stu-id="65d97-237">Function</span></span>
--- | ---
[<span data-ttu-id="65d97-238">dotnet add package</span><span class="sxs-lookup"><span data-stu-id="65d97-238">dotnet add package</span></span>](dotnet-add-package.md) | <span data-ttu-id="65d97-239">添加 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="65d97-239">Adds a NuGet package.</span></span>
[<span data-ttu-id="65d97-240">dotnet remove package</span><span class="sxs-lookup"><span data-stu-id="65d97-240">dotnet remove package</span></span>](dotnet-remove-package.md) | <span data-ttu-id="65d97-241">删除 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="65d97-241">Removes a NuGet package.</span></span>

### <a name="nuget-commands"></a><span data-ttu-id="65d97-242">NuGet 命令</span><span class="sxs-lookup"><span data-stu-id="65d97-242">NuGet commands</span></span>

<span data-ttu-id="65d97-243">命令</span><span class="sxs-lookup"><span data-stu-id="65d97-243">Command</span></span> | <span data-ttu-id="65d97-244">函数</span><span class="sxs-lookup"><span data-stu-id="65d97-244">Function</span></span>
--- | ---
[<span data-ttu-id="65d97-245">dotnet nuget delete</span><span class="sxs-lookup"><span data-stu-id="65d97-245">dotnet nuget delete</span></span>](dotnet-nuget-delete.md) | <span data-ttu-id="65d97-246">从服务器删除或取消列出包。</span><span class="sxs-lookup"><span data-stu-id="65d97-246">Deletes or unlists a package from the server.</span></span>
[<span data-ttu-id="65d97-247">dotnet nuget push</span><span class="sxs-lookup"><span data-stu-id="65d97-247">dotnet nuget push</span></span>](dotnet-nuget-push.md) | <span data-ttu-id="65d97-248">将包推送到服务器，并将其发布。</span><span class="sxs-lookup"><span data-stu-id="65d97-248">Pushes a package to the server and publishes it.</span></span>
[<span data-ttu-id="65d97-249">dotnet nuget locals</span><span class="sxs-lookup"><span data-stu-id="65d97-249">dotnet nuget locals</span></span>](dotnet-nuget-locals.md) | <span data-ttu-id="65d97-250">清除或列出本地 NuGet 资源，例如 http 请求缓存、临时缓存或计算机范围的全局包文件夹。</span><span class="sxs-lookup"><span data-stu-id="65d97-250">Clears or lists local NuGet resources such as http-request cache, temporary cache, or machine-wide global packages folder.</span></span>
[<span data-ttu-id="65d97-251">dotnet nuget add source</span><span class="sxs-lookup"><span data-stu-id="65d97-251">dotnet nuget add source</span></span>](dotnet-nuget-add-source.md) | <span data-ttu-id="65d97-252">添加 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="65d97-252">Adds a NuGet source.</span></span>
[<span data-ttu-id="65d97-253">dotnet nuget disable source</span><span class="sxs-lookup"><span data-stu-id="65d97-253">dotnet nuget disable source</span></span>](dotnet-nuget-disable-source.md) | <span data-ttu-id="65d97-254">禁用 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="65d97-254">Disables a NuGet source.</span></span>
[<span data-ttu-id="65d97-255">dotnet nuget enable source</span><span class="sxs-lookup"><span data-stu-id="65d97-255">dotnet nuget enable source</span></span>](dotnet-nuget-enable-source.md) | <span data-ttu-id="65d97-256">启用 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="65d97-256">Enables a NuGet source.</span></span>
[<span data-ttu-id="65d97-257">dotnet nuget list source</span><span class="sxs-lookup"><span data-stu-id="65d97-257">dotnet nuget list source</span></span>](dotnet-nuget-list-source.md) | <span data-ttu-id="65d97-258">列出所有已配置的 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="65d97-258">Lists all configured NuGet sources.</span></span>
[<span data-ttu-id="65d97-259">dotnet nuget remove source</span><span class="sxs-lookup"><span data-stu-id="65d97-259">dotnet nuget remove source</span></span>](dotnet-nuget-remove-source.md) | <span data-ttu-id="65d97-260">删除 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="65d97-260">Removes a NuGet source.</span></span>
[<span data-ttu-id="65d97-261">dotnet nuget update source</span><span class="sxs-lookup"><span data-stu-id="65d97-261">dotnet nuget update source</span></span>](dotnet-nuget-update-source.md) | <span data-ttu-id="65d97-262">更新 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="65d97-262">Updates a NuGet source.</span></span>

### <a name="global-tool-path-and-local-tools-commands"></a><span data-ttu-id="65d97-263">全局、工具路径和本地工具命令</span><span class="sxs-lookup"><span data-stu-id="65d97-263">Global, tool-path, and local tools commands</span></span>

<span data-ttu-id="65d97-264">工具是控制台应用程序，它们从 NuGet 包中安装并从命令提示符处进行调用。</span><span class="sxs-lookup"><span data-stu-id="65d97-264">Tools are console applications that are installed from NuGet packages and are invoked from the command prompt.</span></span> <span data-ttu-id="65d97-265">你可自行编写工具，也可安装由第三方编写的工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-265">You can write tools yourself or install tools written by third parties.</span></span> <span data-ttu-id="65d97-266">工具也称为全局工具、工具路径工具和本地工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-266">Tools are also known as global tools, tool-path tools, and local tools.</span></span> <span data-ttu-id="65d97-267">有关详细信息，请参阅 [.NET 工具概述](global-tools.md)。</span><span class="sxs-lookup"><span data-stu-id="65d97-267">For more information, see [.NET tools overview](global-tools.md).</span></span> <span data-ttu-id="65d97-268">全局和工具路径工具从 .NET Core SDK 2.1 开始可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-268">Global and tool-path tools are available starting with .NET Core SDK 2.1.</span></span> <span data-ttu-id="65d97-269">本地工具从 .NET Core SDK 3.0 开始可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-269">Local tools are available starting with .NET Core SDK 3.0.</span></span>

<span data-ttu-id="65d97-270">命令</span><span class="sxs-lookup"><span data-stu-id="65d97-270">Command</span></span> | <span data-ttu-id="65d97-271">函数</span><span class="sxs-lookup"><span data-stu-id="65d97-271">Function</span></span>
--- | ---
[<span data-ttu-id="65d97-272">dotnet tool install</span><span class="sxs-lookup"><span data-stu-id="65d97-272">dotnet tool install</span></span>](dotnet-tool-install.md) | <span data-ttu-id="65d97-273">在计算机上安装工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-273">Installs a tool on your machine.</span></span>
[<span data-ttu-id="65d97-274">dotnet tool list</span><span class="sxs-lookup"><span data-stu-id="65d97-274">dotnet tool list</span></span>](dotnet-tool-list.md) | <span data-ttu-id="65d97-275">列出计算机上当前安装的所有全局、工具路径或本地工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-275">Lists all global, tool-path, or local tools currently installed on your machine.</span></span>
[<span data-ttu-id="65d97-276">dotnet tool search</span><span class="sxs-lookup"><span data-stu-id="65d97-276">dotnet tool search</span></span>](dotnet-tool-list.md) | <span data-ttu-id="65d97-277">在 NuGet.org 中搜索其名称或元数据中具有指定搜索词的工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-277">Searches NuGet.org for tools that have the specified search term in their name or metadata.</span></span>
[<span data-ttu-id="65d97-278">dotnet tool uninstall</span><span class="sxs-lookup"><span data-stu-id="65d97-278">dotnet tool uninstall</span></span>](dotnet-tool-uninstall.md) | <span data-ttu-id="65d97-279">从计算机中卸载工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-279">Uninstalls a tool from your machine.</span></span>
[<span data-ttu-id="65d97-280">dotnet tool update</span><span class="sxs-lookup"><span data-stu-id="65d97-280">dotnet tool update</span></span>](dotnet-tool-update.md) | <span data-ttu-id="65d97-281">更新计算机上安装的工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-281">Updates a tool that is installed on your machine.</span></span>

### <a name="additional-tools"></a><span data-ttu-id="65d97-282">其他工具</span><span class="sxs-lookup"><span data-stu-id="65d97-282">Additional tools</span></span>

<span data-ttu-id="65d97-283">自 .NET Core SDK 2.1.300 开始，许多使用 `DotnetCliToolReference` 且仅在每个项目的基础上可用的工具现作为 .NET SDK 的一部分提供。</span><span class="sxs-lookup"><span data-stu-id="65d97-283">Starting with .NET Core SDK 2.1.300, a number of tools that were available only on a per project basis using `DotnetCliToolReference` are now available as part of the .NET SDK.</span></span> <span data-ttu-id="65d97-284">下表中列出了这些工具：</span><span class="sxs-lookup"><span data-stu-id="65d97-284">These tools are listed in the following table:</span></span>

| <span data-ttu-id="65d97-285">工具</span><span class="sxs-lookup"><span data-stu-id="65d97-285">Tool</span></span>                                              | <span data-ttu-id="65d97-286">函数</span><span class="sxs-lookup"><span data-stu-id="65d97-286">Function</span></span>                                                     |
| ------------------------------------------------- | ------------------------------------------------------------ |
| <span data-ttu-id="65d97-287">dev-certs</span><span class="sxs-lookup"><span data-stu-id="65d97-287">dev-certs</span></span>                                         | <span data-ttu-id="65d97-288">创建和管理开发证书。</span><span class="sxs-lookup"><span data-stu-id="65d97-288">Creates and manages development certificates.</span></span>                |
| [<span data-ttu-id="65d97-289">ef</span><span class="sxs-lookup"><span data-stu-id="65d97-289">ef</span></span>](/ef/core/miscellaneous/cli/dotnet)           | <span data-ttu-id="65d97-290">Entity Framework Core 命令行工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-290">Entity Framework Core command-line tools.</span></span>                    |
| <span data-ttu-id="65d97-291">sql-cache</span><span class="sxs-lookup"><span data-stu-id="65d97-291">sql-cache</span></span>                                         | <span data-ttu-id="65d97-292">SQL Server 缓存命令行工具。</span><span class="sxs-lookup"><span data-stu-id="65d97-292">SQL Server cache command-line tools.</span></span>                         |
| [<span data-ttu-id="65d97-293">user-secrets</span><span class="sxs-lookup"><span data-stu-id="65d97-293">user-secrets</span></span>](/aspnet/core/security/app-secrets) | <span data-ttu-id="65d97-294">管理开发用户机密。</span><span class="sxs-lookup"><span data-stu-id="65d97-294">Manages development user secrets.</span></span>                            |
| [<span data-ttu-id="65d97-295">watch</span><span class="sxs-lookup"><span data-stu-id="65d97-295">watch</span></span>](/aspnet/core/tutorials/dotnet-watch)      | <span data-ttu-id="65d97-296">启动文件观察程序，以在更改文件时运行命令。</span><span class="sxs-lookup"><span data-stu-id="65d97-296">Starts a file watcher that runs a command when files change.</span></span> |

<span data-ttu-id="65d97-297">有关每个工具的详细信息，请键入 `dotnet <tool-name> --help`。</span><span class="sxs-lookup"><span data-stu-id="65d97-297">For more information about each tool, type `dotnet <tool-name> --help`.</span></span>

## <a name="examples"></a><span data-ttu-id="65d97-298">示例</span><span class="sxs-lookup"><span data-stu-id="65d97-298">Examples</span></span>

<span data-ttu-id="65d97-299">创建新的 .NET 控制台应用程序：</span><span class="sxs-lookup"><span data-stu-id="65d97-299">Create a new .NET console application:</span></span>

```dotnetcli
dotnet new console
```

<span data-ttu-id="65d97-300">生成给定目录中的项目及其依赖项：</span><span class="sxs-lookup"><span data-stu-id="65d97-300">Build a project and its dependencies in a given directory:</span></span>

```dotnetcli
dotnet build
```

<span data-ttu-id="65d97-301">运行应用程序：</span><span class="sxs-lookup"><span data-stu-id="65d97-301">Run an application:</span></span>

```dotnetcli
dotnet myapp.dll
```

## <a name="environment-variables"></a><span data-ttu-id="65d97-302">环境变量</span><span class="sxs-lookup"><span data-stu-id="65d97-302">Environment variables</span></span>

- <span data-ttu-id="65d97-303">`DOTNET_ROOT`, `DOTNET_ROOT(x86)`</span><span class="sxs-lookup"><span data-stu-id="65d97-303">`DOTNET_ROOT`, `DOTNET_ROOT(x86)`</span></span>

  <span data-ttu-id="65d97-304">指定 .NET 运行时的位置（如果运行时未安装在默认位置）。</span><span class="sxs-lookup"><span data-stu-id="65d97-304">Specifies the location of the .NET runtimes, if they are not installed in the default location.</span></span> <span data-ttu-id="65d97-305">Windows 上的默认位置为 `C:\Program Files\dotnet`。</span><span class="sxs-lookup"><span data-stu-id="65d97-305">The default location on Windows is `C:\Program Files\dotnet`.</span></span> <span data-ttu-id="65d97-306">Linux 和 macOS 上的默认位置为 `/usr/share/dotnet`。</span><span class="sxs-lookup"><span data-stu-id="65d97-306">The default location on Linux and macOS is `/usr/share/dotnet`.</span></span> <span data-ttu-id="65d97-307">此环境变量仅在通过生成的可执行文件 (apphosts) 运行应用时使用。</span><span class="sxs-lookup"><span data-stu-id="65d97-307">This environment variable is used only when running apps via generated executables (apphosts).</span></span> <span data-ttu-id="65d97-308">在 64 位 OS 上运行 32 位可执行文件时，改用 `DOTNET_ROOT(x86)`。</span><span class="sxs-lookup"><span data-stu-id="65d97-308">`DOTNET_ROOT(x86)` is used instead when running a 32-bit executable on a 64-bit OS.</span></span>

- `NUGET_PACKAGES`

  <span data-ttu-id="65d97-309">全局包文件夹。</span><span class="sxs-lookup"><span data-stu-id="65d97-309">The global packages folder.</span></span> <span data-ttu-id="65d97-310">如果未设置，则默认为 Unix 上的 `~/.nuget/packages` 或 Windows 上的 `%userprofile%\.nuget\packages`。</span><span class="sxs-lookup"><span data-stu-id="65d97-310">If not set, it defaults to `~/.nuget/packages` on Unix or `%userprofile%\.nuget\packages` on Windows.</span></span>

- `DOTNET_SERVICING`

  <span data-ttu-id="65d97-311">指定加载运行时期间共享主机要使用的服务索引的位置。</span><span class="sxs-lookup"><span data-stu-id="65d97-311">Specifies the location of the servicing index to use by the shared host when loading the runtime.</span></span>

- `DOTNET_NOLOGO`

  <span data-ttu-id="65d97-312">指定是否在首次运行时显示 .NET 欢迎消息和遥测消息。</span><span class="sxs-lookup"><span data-stu-id="65d97-312">Specifies whether .NET welcome and telemetry messages are displayed on first run.</span></span> <span data-ttu-id="65d97-313">设置为 `true` 可将这些消息静音（接受 `true`、`1` 或 `yes` 值），或者，设置为 `false` 可允许显示消息（接受 `false`、`0` 或 `no` 值）。</span><span class="sxs-lookup"><span data-stu-id="65d97-313">Set to `true` to mute these messages (values `true`, `1`, or `yes` accepted) or set to `false` to allow (values `false`, `0`, or `no` accepted).</span></span> <span data-ttu-id="65d97-314">如果未设置，则默认值为 `false`，表示在首次运行时将显示消息。</span><span class="sxs-lookup"><span data-stu-id="65d97-314">If not set, the default is `false` and the messages will be displayed on first run.</span></span> <span data-ttu-id="65d97-315">此标志对遥测不起作用（请参阅 `DOTNET_CLI_TELEMETRY_OPTOUT` 中关于如何选择不发送遥测数据的信息）。</span><span class="sxs-lookup"><span data-stu-id="65d97-315">This flag has no effect on telemetry (see `DOTNET_CLI_TELEMETRY_OPTOUT` for opting out of sending telemetry).</span></span>

- `DOTNET_CLI_TELEMETRY_OPTOUT`

  <span data-ttu-id="65d97-316">指定是否收集并向 Microsoft 发送 .NET 工具使用情况的相关数据。</span><span class="sxs-lookup"><span data-stu-id="65d97-316">Specifies whether data about the .NET tools usage is collected and sent to Microsoft.</span></span> <span data-ttu-id="65d97-317">设置为 `true` 以选择退出遥测功能（接受的值为 `true`、`1` 或 `yes`）。</span><span class="sxs-lookup"><span data-stu-id="65d97-317">Set to `true` to opt-out of the telemetry feature (values `true`, `1`, or `yes` accepted).</span></span> <span data-ttu-id="65d97-318">否则，设置为 `false` 以选择加入遥测功能（接受的值为 `false`、`0` 或 `no`）。</span><span class="sxs-lookup"><span data-stu-id="65d97-318">Otherwise, set to `false` to opt into the telemetry features (values `false`, `0`, or `no` accepted).</span></span> <span data-ttu-id="65d97-319">如果未设置，则默认为 `false` 且遥测功能为活动状态。</span><span class="sxs-lookup"><span data-stu-id="65d97-319">If not set, the default is `false` and the telemetry feature is active.</span></span>

- `DOTNET_MULTILEVEL_LOOKUP`

  <span data-ttu-id="65d97-320">指定是否从全局位置解析 .NET 运行时、共享框架或 SDK。</span><span class="sxs-lookup"><span data-stu-id="65d97-320">Specifies whether .NET runtime, shared framework, or SDK are resolved from the global location.</span></span> <span data-ttu-id="65d97-321">如果未设置，则默认为 1（逻辑 `true`）。</span><span class="sxs-lookup"><span data-stu-id="65d97-321">If not set, it defaults to 1 (logical `true`).</span></span> <span data-ttu-id="65d97-322">设置为 0（逻辑 `false`），不从全局位置解析，并且具有独立的 .NET 安装。</span><span class="sxs-lookup"><span data-stu-id="65d97-322">Set to 0 (logical `false`) to not resolve from the global location and have isolated .NET installations.</span></span> <span data-ttu-id="65d97-323">有关多级别查找的详细信息，请参阅 [Multi-level SharedFX Lookup](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/multilevel-sharedfx-lookup.md)（多级别 SharedFX 查找）。</span><span class="sxs-lookup"><span data-stu-id="65d97-323">For more information about multi-level lookup, see [Multi-level SharedFX Lookup](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/multilevel-sharedfx-lookup.md).</span></span>

- <span data-ttu-id="65d97-324">`DOTNET_ROLL_FORWARD` 自 .NET Core 3.x 起可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-324">`DOTNET_ROLL_FORWARD` **Available starting with .NET Core 3.x.**</span></span>

  <span data-ttu-id="65d97-325">确定前滚行为。</span><span class="sxs-lookup"><span data-stu-id="65d97-325">Determines roll forward behavior.</span></span> <span data-ttu-id="65d97-326">有关详细信息，请参阅本文章前面介绍的 `--roll-forward` 选项。</span><span class="sxs-lookup"><span data-stu-id="65d97-326">For more information, see the `--roll-forward` option earlier in this article.</span></span>

- <span data-ttu-id="65d97-327">`DOTNET_ROLL_FORWARD_TO_PRERELEASE` 自 .NET Core 3.x 起可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-327">`DOTNET_ROLL_FORWARD_TO_PRERELEASE` **Available starting with .NET Core 3.x.**</span></span>

  <span data-ttu-id="65d97-328">如果设置为 `1`（已启用），则允许从发布版本前滚到预发行版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-328">If set to `1` (enabled), enables rolling forward to a pre-release version from a release version.</span></span> <span data-ttu-id="65d97-329">默认情况下（`0` - 禁用），请求 .NET 运行时的发行版本时，前滚将仅考虑已安装的发行版本。</span><span class="sxs-lookup"><span data-stu-id="65d97-329">By default (`0` - disabled), when a release version of .NET runtime is requested, roll-forward will only consider installed release versions.</span></span>

  <span data-ttu-id="65d97-330">有关详细信息，请参阅[前滚](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="65d97-330">For more information, see [Roll forward](../whats-new/dotnet-core-3-0.md#major-version-runtime-roll-forward).</span></span>

- <span data-ttu-id="65d97-331">`DOTNET_ROLL_FORWARD_ON_NO_CANDIDATE_FX` 在 .NET Core 2.x 中可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-331">`DOTNET_ROLL_FORWARD_ON_NO_CANDIDATE_FX` **Available in .NET Core 2.x.**</span></span>

  <span data-ttu-id="65d97-332">如果设置为 `0`，则禁用次要版本前滚。</span><span class="sxs-lookup"><span data-stu-id="65d97-332">Disables minor version roll forward, if set to `0`.</span></span> <span data-ttu-id="65d97-333">有关详细信息，请参阅[前滚](../whats-new/dotnet-core-2-1.md#roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="65d97-333">For more information, see [Roll forward](../whats-new/dotnet-core-2-1.md#roll-forward).</span></span>

  <span data-ttu-id="65d97-334">此设置在 .NET Core 3.0 中被 `DOTNET_ROLL_FORWARD` 取代。</span><span class="sxs-lookup"><span data-stu-id="65d97-334">This setting is superseded in .NET Core 3.0 by `DOTNET_ROLL_FORWARD`.</span></span> <span data-ttu-id="65d97-335">应改为使用新设置。</span><span class="sxs-lookup"><span data-stu-id="65d97-335">The new settings should be used instead.</span></span>

- `DOTNET_CLI_UI_LANGUAGE`

  <span data-ttu-id="65d97-336">使用区域设置值（如 `en-us`）设置 CLI UI 的语言。</span><span class="sxs-lookup"><span data-stu-id="65d97-336">Sets the language of the CLI UI using a locale value such as `en-us`.</span></span> <span data-ttu-id="65d97-337">支持的值与 Visual Studio 中的值相同。</span><span class="sxs-lookup"><span data-stu-id="65d97-337">The supported values are the same as for Visual Studio.</span></span> <span data-ttu-id="65d97-338">有关详细信息，请参阅 [Visual Studio 安装文档](/visualstudio/install/install-visual-studio)中有关更改安装程序语言一节。</span><span class="sxs-lookup"><span data-stu-id="65d97-338">For more information, see the section on changing the installer language in the [Visual Studio installation documentation](/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="65d97-339">.NET 资源管理器规则适用，因此你无需选取精确匹配项 &mdash; 你还可以在 `CultureInfo` 树中选取后代。</span><span class="sxs-lookup"><span data-stu-id="65d97-339">The .NET resource manager rules apply, so you don't have to pick an exact match&mdash;you can also pick descendants in the `CultureInfo` tree.</span></span> <span data-ttu-id="65d97-340">例如，如果将其设置为 `fr-CA`，CLI 将查找并使用 `fr` 翻译。</span><span class="sxs-lookup"><span data-stu-id="65d97-340">For example, if you set it to `fr-CA`, the CLI will find and use the `fr` translations.</span></span> <span data-ttu-id="65d97-341">如果你将其设置为不受支持的语言，CLI 会退回到英语。</span><span class="sxs-lookup"><span data-stu-id="65d97-341">If you set it to a language that is not supported, the CLI falls back to English.</span></span>

- `DOTNET_DISABLE_GUI_ERRORS`

  <span data-ttu-id="65d97-342">对于启用了 GUI 的已生成可执行文件 - 禁用对话框弹出窗口，此窗口通常显示某些类错误。</span><span class="sxs-lookup"><span data-stu-id="65d97-342">For GUI-enabled generated executables - disables dialog popup, which normally shows for certain classes of errors.</span></span> <span data-ttu-id="65d97-343">在这些情况下，它仅写入到 `stderr` 并退出。</span><span class="sxs-lookup"><span data-stu-id="65d97-343">It only writes to `stderr` and exits in those cases.</span></span>
  
- `DOTNET_ADDITIONAL_DEPS`

  <span data-ttu-id="65d97-344">等效于 CLI 选项 `--additional-deps`。</span><span class="sxs-lookup"><span data-stu-id="65d97-344">Equivalent to CLI option `--additional-deps`.</span></span>

- `DOTNET_RUNTIME_ID`

  <span data-ttu-id="65d97-345">替代检测到的 RID。</span><span class="sxs-lookup"><span data-stu-id="65d97-345">Overrides the detected RID.</span></span>

- `DOTNET_SHARED_STORE`

  <span data-ttu-id="65d97-346">程序集解析在某些情况下将回退到的“共享存储”的位置。</span><span class="sxs-lookup"><span data-stu-id="65d97-346">Location of the "shared store" which assembly resolution falls back to in some cases.</span></span>

- `DOTNET_STARTUP_HOOKS`

  <span data-ttu-id="65d97-347">要从中加载和执行启动挂钩的程序集列表。</span><span class="sxs-lookup"><span data-stu-id="65d97-347">List of assemblies to load and execute startup hooks from.</span></span>

- <span data-ttu-id="65d97-348">`DOTNET_BUNDLE_EXTRACT_BASE_DIR` 自 .NET Core 3.x 起可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-348">`DOTNET_BUNDLE_EXTRACT_BASE_DIR` **Available starting with .NET Core 3.x.**</span></span>

  <span data-ttu-id="65d97-349">指定在执行单文件应用程序之前将其提取到的目录。</span><span class="sxs-lookup"><span data-stu-id="65d97-349">Specifies a directory to which a single-file application is extracted before it is executed.</span></span>

  <span data-ttu-id="65d97-350">有关详细信息，请参阅[单文件可执行文件](../whats-new/dotnet-core-3-0.md#single-file-executables)。</span><span class="sxs-lookup"><span data-stu-id="65d97-350">For more information, see [Single-file executables](../whats-new/dotnet-core-3-0.md#single-file-executables).</span></span>

- <span data-ttu-id="65d97-351">`COREHOST_TRACE`, `COREHOST_TRACEFILE`, `COREHOST_TRACE_VERBOSITY`</span><span class="sxs-lookup"><span data-stu-id="65d97-351">`COREHOST_TRACE`, `COREHOST_TRACEFILE`, `COREHOST_TRACE_VERBOSITY`</span></span>

  <span data-ttu-id="65d97-352">控制来自托管组件（例如 `dotnet.exe`、`hostfxr` 和 `hostpolicy`）的诊断跟踪。</span><span class="sxs-lookup"><span data-stu-id="65d97-352">Controls diagnostics tracing from the hosting components, such as `dotnet.exe`, `hostfxr`, and `hostpolicy`.</span></span>

  * <span data-ttu-id="65d97-353">`COREHOST_TRACE=[0/1]` -默认值为 `0` - 禁用跟踪。</span><span class="sxs-lookup"><span data-stu-id="65d97-353">`COREHOST_TRACE=[0/1]` - default is `0` - tracing disabled.</span></span> <span data-ttu-id="65d97-354">如果设置为 `1`，则启用诊断跟踪。</span><span class="sxs-lookup"><span data-stu-id="65d97-354">If set to `1`, diagnostics tracing is enabled.</span></span>
  * <span data-ttu-id="65d97-355">`COREHOST_TRACEFILE=<file path>` - 仅当通过 `COREHOST_TRACE=1` 启用跟踪时才会生效。</span><span class="sxs-lookup"><span data-stu-id="65d97-355">`COREHOST_TRACEFILE=<file path>` - only has effect if tracing is enabled via `COREHOST_TRACE=1`.</span></span> <span data-ttu-id="65d97-356">设置后，跟踪信息将写入指定的文件，否则会将跟踪信息写入 `stderr`。</span><span class="sxs-lookup"><span data-stu-id="65d97-356">When set, the tracing information is written to the specified file, otherwise the tracing information is written to `stderr`.</span></span> <span data-ttu-id="65d97-357">自 .NET Core 3.x 起可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-357">**Available starting with .NET Core 3.x.**</span></span>
  * <span data-ttu-id="65d97-358">`COREHOST_TRACE_VERBOSITY=[1/2/3/4]` - 默认值为 `4`。</span><span class="sxs-lookup"><span data-stu-id="65d97-358">`COREHOST_TRACE_VERBOSITY=[1/2/3/4]` - default is `4`.</span></span> <span data-ttu-id="65d97-359">此设置仅在通过 `COREHOST_TRACE=1` 启用跟踪时使用。</span><span class="sxs-lookup"><span data-stu-id="65d97-359">The setting is used only when tracing is enabled via `COREHOST_TRACE=1`.</span></span> <span data-ttu-id="65d97-360">自 .NET Core 3.x 起可用。</span><span class="sxs-lookup"><span data-stu-id="65d97-360">**Available starting with .NET Core 3.x.**</span></span>
    * <span data-ttu-id="65d97-361">`4` - 写入所有跟踪信息</span><span class="sxs-lookup"><span data-stu-id="65d97-361">`4` - all tracing information is written</span></span>
    * <span data-ttu-id="65d97-362">`3` - 仅写入信息性、警告和错误消息</span><span class="sxs-lookup"><span data-stu-id="65d97-362">`3` - only informational, warning and error messages are written</span></span>
    * <span data-ttu-id="65d97-363">`2` - 仅写入警告和错误消息</span><span class="sxs-lookup"><span data-stu-id="65d97-363">`2` - only warning and error messages are written</span></span>
    * <span data-ttu-id="65d97-364">`1` - 仅写入错误消息</span><span class="sxs-lookup"><span data-stu-id="65d97-364">`1` - only error messages are written</span></span>

  <span data-ttu-id="65d97-365">获取有关应用程序启动的详细跟踪信息的典型方法是设置 `COREHOST_TRACE=1` 和 `COREHOST_TRACEFILE=host_trace.txt`，然后运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="65d97-365">The typical way to get detailed trace information about application startup is to set `COREHOST_TRACE=1` and `COREHOST_TRACEFILE=host_trace.txt` and then run the application.</span></span> <span data-ttu-id="65d97-366">将在当前目录中创建一个新文件 `host_trace.txt`，其中包含详细信息。</span><span class="sxs-lookup"><span data-stu-id="65d97-366">A new file `host_trace.txt` will be created in the current directory with the detailed information.</span></span>

## <a name="see-also"></a><span data-ttu-id="65d97-367">请参阅</span><span class="sxs-lookup"><span data-stu-id="65d97-367">See also</span></span>

- [<span data-ttu-id="65d97-368">运行时配置文件</span><span class="sxs-lookup"><span data-stu-id="65d97-368">Runtime Configuration Files</span></span>](https://github.com/dotnet/sdk/blob/main/documentation/specs/runtime-configuration-file.md)
- [<span data-ttu-id="65d97-369">.NET 运行时配置设置</span><span class="sxs-lookup"><span data-stu-id="65d97-369">.NET run-time configuration settings</span></span>](../run-time-config/index.md)
