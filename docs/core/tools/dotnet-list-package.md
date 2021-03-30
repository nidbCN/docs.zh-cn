---
title: dotnet list package 命令
description: 使用“dotnet list package”命令，可以方便地列出项目或解决方案的包引用。
ms.date: 11/11/2020
ms.openlocfilehash: b51ef5deb8b6418938787003b409803a3c814b08
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873427"
---
# <a name="dotnet-list-package"></a><span data-ttu-id="5f5b7-103">dotnet list package</span><span class="sxs-lookup"><span data-stu-id="5f5b7-103">dotnet list package</span></span>

<span data-ttu-id="5f5b7-104">本文适用于： ✔️ .NET Core 2.2 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="5f5b7-104">**This article applies to:** ✔️ .NET Core 2.2 SDK and later versions</span></span>

## <a name="name"></a><span data-ttu-id="5f5b7-105">“属性”</span><span class="sxs-lookup"><span data-stu-id="5f5b7-105">Name</span></span>

<span data-ttu-id="5f5b7-106">`dotnet list package` - 列出项目或解决方案的包引用。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-106">`dotnet list package` - Lists the package references for a project or solution.</span></span>

## <a name="synopsis"></a><span data-ttu-id="5f5b7-107">摘要</span><span class="sxs-lookup"><span data-stu-id="5f5b7-107">Synopsis</span></span>

```dotnetcli
dotnet list [<PROJECT>|<SOLUTION>] package [--config <SOURCE>]
    [--deprecated]
    [--framework <FRAMEWORK>] [--highest-minor] [--highest-patch]
    [--include-prerelease] [--include-transitive] [--interactive]
    [--outdated] [--source <SOURCE>] [-v|--verbosity <LEVEL>]

dotnet list package -h|--help
```

## <a name="description"></a><span data-ttu-id="5f5b7-108">描述</span><span class="sxs-lookup"><span data-stu-id="5f5b7-108">Description</span></span>

<span data-ttu-id="5f5b7-109">使用 `dotnet list package` 命令，可以方便地列出特定项目或解决方案的所有 NuGet 包引用。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-109">The `dotnet list package` command provides a convenient option to list all NuGet package references for a specific project or a solution.</span></span> <span data-ttu-id="5f5b7-110">首先，需要生成项目，以提供必需资产以供此命令处理。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-110">You first need to build the project in order to have the assets needed for this command to process.</span></span> <span data-ttu-id="5f5b7-111">下面的示例展示了 [SentimentAnalysis](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) 项目的 `dotnet list package` 命令输出：</span><span class="sxs-lookup"><span data-stu-id="5f5b7-111">The following example shows the output of the `dotnet list package` command for the [SentimentAnalysis](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/SentimentAnalysis) project:</span></span>

```output
Project 'SentimentAnalysis' has the following package references
   [netcoreapp2.1]:
   Top-level Package               Requested   Resolved
   > Microsoft.ML                  1.4.0       1.4.0
   > Microsoft.NETCore.App   (A)   [2.1.0, )   2.1.0

(A) : Auto-referenced package.
```

<span data-ttu-id="5f5b7-112">“已请求”列是指项目文件中指定的包版本，可以是一个范围。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-112">The **Requested** column refers to the package version specified in the project file and can be a range.</span></span> <span data-ttu-id="5f5b7-113">“已解析”列列出了项目当前使用的版本，始终都是一个值。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-113">The **Resolved** column lists the version that the project is currently using and is always a single value.</span></span> <span data-ttu-id="5f5b7-114">紧靠名称旁边显示 `(A)` 的包表示从项目设置（`Sdk` 类型、`<TargetFramework>` 或 `<TargetFrameworks>` 属性）推断出的隐式包引用。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-114">The packages displaying an `(A)` right next to their names represent implicit package references that are inferred from your project settings (`Sdk` type, or `<TargetFramework>` or `<TargetFrameworks>` property).</span></span>

<span data-ttu-id="5f5b7-115">使用 `--outdated` 选项，可以确定项目中正在使用的包是否有更高版本。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-115">Use the `--outdated` option to find out if there are newer versions available of the packages you're using in your projects.</span></span> <span data-ttu-id="5f5b7-116">默认情况下，`--outdated` 列出最新稳定包，除非已解析版本也是预发行版本。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-116">By default, `--outdated` lists the latest stable packages unless the resolved version is also a prerelease version.</span></span> <span data-ttu-id="5f5b7-117">若要在列出更高版本时包含预发行版本，还请指定 `--include-prerelease` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-117">To include prerelease versions when listing newer versions, also specify the `--include-prerelease` option.</span></span> <span data-ttu-id="5f5b7-118">下面的示例展示了上一个示例中相同项目的 `dotnet list package --outdated --include-prerelease` 命令输出：</span><span class="sxs-lookup"><span data-stu-id="5f5b7-118">The following examples shows the output of the `dotnet list package --outdated --include-prerelease` command for the same project as the previous example:</span></span>

```output
The following sources were used:
   https://api.nuget.org/v3/index.json
   C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\

Project `SentimentAnalysis` has the following updates to its packages
   [netcoreapp2.1]:
   Top-level Package      Requested   Resolved   Latest
   > Microsoft.ML         1.4.0       1.4.0      1.5.0-preview
```

<span data-ttu-id="5f5b7-119">如果需要确定项目是否有可传递依赖关系，请使用 `--include-transitive` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-119">If you need to find out whether your project has transitive dependencies, use the `--include-transitive` option.</span></span> <span data-ttu-id="5f5b7-120">如果在项目中添加包，它转而又依赖另一个包，就会出现可传递依赖关系。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-120">Transitive dependencies occur when you add a package to your project that in turn relies on another package.</span></span> <span data-ttu-id="5f5b7-121">下面的示例展示了 [HelloPlugin](https://github.com/dotnet/samples/tree/main/core/extensions/AppWithPlugin/HelloPlugin) 项目的 `dotnet list package --include-transitive` 命令运行输出，其中显示顶级包及其依赖的包：</span><span class="sxs-lookup"><span data-stu-id="5f5b7-121">The following example shows the output from running the `dotnet list package --include-transitive` command for the [HelloPlugin](https://github.com/dotnet/samples/tree/main/core/extensions/AppWithPlugin/HelloPlugin) project, which displays top-level packages and the packages they depend on:</span></span>

```output
Project 'HelloPlugin' has the following package references
   [netcoreapp3.0]:
   Transitive Package      Resolved
   > PluginBase            1.0.0
```

## <a name="arguments"></a><span data-ttu-id="5f5b7-122">自变量</span><span class="sxs-lookup"><span data-stu-id="5f5b7-122">Arguments</span></span>

`PROJECT | SOLUTION`

<span data-ttu-id="5f5b7-123">要对其运行命令的项目或解决方案文件。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-123">The project or solution file to operate on.</span></span> <span data-ttu-id="5f5b7-124">如果未指定，此命令会搜索当前目录来获取一个项目文件。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-124">If not specified, the command searches the current directory for one.</span></span> <span data-ttu-id="5f5b7-125">如果找到多个解决方案或项目，便会抛出错误。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-125">If more than one solution or project is found, an error is thrown.</span></span>

## <a name="options"></a><span data-ttu-id="5f5b7-126">选项</span><span class="sxs-lookup"><span data-stu-id="5f5b7-126">Options</span></span>

- **`--config <SOURCE>`**

  <span data-ttu-id="5f5b7-127">在搜索版本更高的包时，要使用的 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-127">The NuGet sources to use when searching for newer packages.</span></span> <span data-ttu-id="5f5b7-128">需要使用 `--outdated` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-128">Requires the `--outdated` option.</span></span>

- **`--deprecated`**

  <span data-ttu-id="5f5b7-129">显示已弃用的包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-129">Displays packages that have been deprecated.</span></span>

- **`--framework <FRAMEWORK>`**

  <span data-ttu-id="5f5b7-130">只显示适用于指定[目标框架](../../standard/frameworks.md)的包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-130">Displays only the packages applicable for the specified [target framework](../../standard/frameworks.md).</span></span> <span data-ttu-id="5f5b7-131">若要指定多个框架，请多次重复此选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-131">To specify multiple frameworks, repeat the option multiple times.</span></span> <span data-ttu-id="5f5b7-132">例如：`--framework netcoreapp2.2 --framework netstandard2.0`。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-132">For example: `--framework netcoreapp2.2 --framework netstandard2.0`.</span></span>

- **`-h|--help`**

  <span data-ttu-id="5f5b7-133">打印出有关命令的简短帮助。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-133">Prints out a short help for the command.</span></span>

- **`--highest-minor`**

  <span data-ttu-id="5f5b7-134">在搜索版本更高的包时，仅考虑有匹配的主版本号的包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-134">Considers only the packages with a matching major version number when searching for newer packages.</span></span> <span data-ttu-id="5f5b7-135">需要使用 `--outdated` 或 `--deprecated` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-135">Requires the `--outdated` or `--deprecated` option.</span></span>

- **`--highest-patch`**

  <span data-ttu-id="5f5b7-136">在搜索版本更高的包时，仅考虑有匹配的主版本号和次要版本号的包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-136">Considers only the packages with a matching major and minor version numbers when searching for newer packages.</span></span> <span data-ttu-id="5f5b7-137">需要使用 `--outdated` 或 `--deprecated` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-137">Requires the `--outdated` or `--deprecated` option.</span></span>

- **`--include-prerelease`**

  <span data-ttu-id="5f5b7-138">在搜索版本更高的包时，考虑有预发行版本的包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-138">Considers packages with prerelease versions when searching for newer packages.</span></span> <span data-ttu-id="5f5b7-139">需要使用 `--outdated` 或 `--deprecated` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-139">Requires the `--outdated` or `--deprecated` option.</span></span>

- **`--include-transitive`**

  <span data-ttu-id="5f5b7-140">除了顶级包之外，还列出可传递包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-140">Lists transitive packages, in addition to the top-level packages.</span></span> <span data-ttu-id="5f5b7-141">如果指定此选项，可以获取顶级包所依赖的包列表。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-141">When specifying this option, you get a list of packages that the top-level packages depend on.</span></span>

- **`--interactive`**

  <span data-ttu-id="5f5b7-142">允许命令停止并等待用户输入或操作。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-142">Allows the command to stop and wait for user input or action.</span></span> <span data-ttu-id="5f5b7-143">例如，完成身份验证。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-143">For example, to complete authentication.</span></span> <span data-ttu-id="5f5b7-144">自 .NET Core 3.0 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-144">Available since .NET Core 3.0 SDK.</span></span>

- **`--outdated`**

  <span data-ttu-id="5f5b7-145">列出版本更高的包。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-145">Lists packages that have newer versions available.</span></span>

- **`-s|--source <SOURCE>`**

  <span data-ttu-id="5f5b7-146">在搜索版本更高的包时，要使用的 NuGet 源。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-146">The NuGet sources to use when searching for newer packages.</span></span> <span data-ttu-id="5f5b7-147">需要使用 `--outdated` 或 `--deprecated` 选项。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-147">Requires the `--outdated` or `--deprecated` option.</span></span>

- **`-v|--verbosity <LEVEL>`**

  <span data-ttu-id="5f5b7-148">设置 MSBuild 详细级别。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-148">Sets the MSBuild verbosity level.</span></span> <span data-ttu-id="5f5b7-149">允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-149">Allowed values are `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]`, and `diag[nostic]`.</span></span> <span data-ttu-id="5f5b7-150">默认值为 `minimal`。</span><span class="sxs-lookup"><span data-stu-id="5f5b7-150">The default is `minimal`.</span></span>

## <a name="examples"></a><span data-ttu-id="5f5b7-151">示例</span><span class="sxs-lookup"><span data-stu-id="5f5b7-151">Examples</span></span>

- <span data-ttu-id="5f5b7-152">列出特定项目的包引用：</span><span class="sxs-lookup"><span data-stu-id="5f5b7-152">List package references of a specific project:</span></span>

  ```dotnetcli
  dotnet list SentimentAnalysis.csproj package
  ```

- <span data-ttu-id="5f5b7-153">列出有更高版本（包括预发行版本）的包引用：</span><span class="sxs-lookup"><span data-stu-id="5f5b7-153">List package references that have newer versions available, including prerelease versions:</span></span>

  ```dotnetcli
  dotnet list package --outdated --include-prerelease
  ```

- <span data-ttu-id="5f5b7-154">列出特定目标框架的包引用：</span><span class="sxs-lookup"><span data-stu-id="5f5b7-154">List package references for a specific target framework:</span></span>

  ```dotnetcli
  dotnet list package --framework netcoreapp3.0
  ```
