---
title: .NET Core 3.0 的新增功能
description: 了解 .NET Core 3.0 的新增功能。
dev_langs:
- csharp
author: adegeo
ms.author: adegeo
ms.date: 01/27/2020
ms.openlocfilehash: f401af3ec7f1fc729e6f3af005acbde2ad61f586
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875832"
---
# <a name="whats-new-in-net-core-30"></a><span data-ttu-id="67adf-103">.NET Core 3.0 的新增功能</span><span class="sxs-lookup"><span data-stu-id="67adf-103">What's new in .NET Core 3.0</span></span>

<span data-ttu-id="67adf-104">本文介绍了 .NET Core 3.0 中的新增功能。</span><span class="sxs-lookup"><span data-stu-id="67adf-104">This article describes what is new in .NET Core 3.0.</span></span> <span data-ttu-id="67adf-105">最大的增强功能之一是对 Windows 桌面应用程序的支持（仅限 Windows）。</span><span class="sxs-lookup"><span data-stu-id="67adf-105">One of the biggest enhancements is support for Windows desktop applications (Windows only).</span></span> <span data-ttu-id="67adf-106">通过使用 .NET Core 3.0 SDK Windows 桌面组件，可移植 Windows 窗体和 Windows Presentation Foundation (WPF) 应用程序。</span><span class="sxs-lookup"><span data-stu-id="67adf-106">By using the .NET Core 3.0 SDK component Windows Desktop, you can port your Windows Forms and Windows Presentation Foundation (WPF) applications.</span></span> <span data-ttu-id="67adf-107">明确地说，只有在 Windows 上才支持和包含 Windows 桌面组件。</span><span class="sxs-lookup"><span data-stu-id="67adf-107">To be clear, the Windows Desktop component is only supported and included on Windows.</span></span> <span data-ttu-id="67adf-108">有关详细信息，请参阅本文后面的 [Windows 桌面](#windows-desktop)部分。</span><span class="sxs-lookup"><span data-stu-id="67adf-108">For more information, see the [Windows desktop](#windows-desktop) section later in this article.</span></span>

<span data-ttu-id="67adf-109">.NET Core 3.0 添加了对 C#8.0 的支持。</span><span class="sxs-lookup"><span data-stu-id="67adf-109">.NET Core 3.0 adds support for C# 8.0.</span></span> <span data-ttu-id="67adf-110">强烈建议使用 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本、[Visual Studio for Mac 8.3](/visualstudio/mac/install-preview) 或更高版本，或具有最新 C# 扩展的 [Visual Studio Code](https://code.visualstudio.com/)。</span><span class="sxs-lookup"><span data-stu-id="67adf-110">It's highly recommended that you use [Visual Studio 2019 version 16.3](https://visualstudio.microsoft.com/vs/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or newer, [Visual Studio for Mac 8.3](/visualstudio/mac/install-preview) or newer, or [Visual Studio Code](https://code.visualstudio.com/) with the latest **C# extension**.</span></span>

<span data-ttu-id="67adf-111">立即在 Windows、macOS 或 Linux 上[下载并开始使用 .NET Core 3.0](https://aka.ms/netcore3download)。</span><span class="sxs-lookup"><span data-stu-id="67adf-111">[Download and get started with .NET Core 3.0](https://aka.ms/netcore3download) right now on Windows, macOS, or Linux.</span></span>

<span data-ttu-id="67adf-112">有关版本的详细信息，请参阅 [.NET Core 3.0 公告](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/)。</span><span class="sxs-lookup"><span data-stu-id="67adf-112">For more information about the release, see the [.NET Core 3.0 announcement](https://devblogs.microsoft.com/dotnet/announcing-net-core-3-0/).</span></span>

<span data-ttu-id="67adf-113">Microsoft 认为 .NET Core RC1 可用于生产环境，且该软件完全受支持。</span><span class="sxs-lookup"><span data-stu-id="67adf-113">.NET Core RC1 was considered production ready by Microsoft and was fully supported.</span></span> <span data-ttu-id="67adf-114">如果使用的是预览版本，则必须转换为 RTM 版本才能继续获得支持。</span><span class="sxs-lookup"><span data-stu-id="67adf-114">If you're using a preview release, you must move to the RTM version for continued support.</span></span>

## <a name="language-improvements-c-80"></a><span data-ttu-id="67adf-115">语言改进 C# 8.0</span><span class="sxs-lookup"><span data-stu-id="67adf-115">Language improvements C# 8.0</span></span>

<span data-ttu-id="67adf-116">C# 8.0 也是该发布的一部分，包含[可为空引用类型](../../csharp/language-reference/builtin-types/nullable-reference-types.md)功能、异步流和更多模式。</span><span class="sxs-lookup"><span data-stu-id="67adf-116">C# 8.0 is also part of this release, which includes the [nullable reference types](../../csharp/language-reference/builtin-types/nullable-reference-types.md) feature, async streams, and more patterns.</span></span> <span data-ttu-id="67adf-117">有关 C# 8.0 功能的详细信息，请参阅 [C# 8.0 中的新增功能](../../csharp/whats-new/csharp-8.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-117">For more information about C# 8.0 features, see [What's new in C# 8.0](../../csharp/whats-new/csharp-8.md).</span></span>

<span data-ttu-id="67adf-118">与 C# 8.0 语言功能相关的教程：</span><span class="sxs-lookup"><span data-stu-id="67adf-118">Tutorials related to C# 8.0 language features:</span></span>

- [<span data-ttu-id="67adf-119">教程：使用可为空和不可为空引用类型更清晰地表达设计意图</span><span class="sxs-lookup"><span data-stu-id="67adf-119">Tutorial: Express your design intent more clearly with nullable and non-nullable reference types</span></span>](../../csharp/whats-new/tutorials/nullable-reference-types.md)
- [<span data-ttu-id="67adf-120">教程：使用 C# 8.0 和 .NET Core 3.0 生成和使用异步流</span><span class="sxs-lookup"><span data-stu-id="67adf-120">Tutorial: Generate and consume async streams using C# 8.0 and .NET Core 3.0</span></span>](../../csharp/whats-new/tutorials/generate-consume-asynchronous-stream.md)
- [<span data-ttu-id="67adf-121">教程：使用模式匹配来构建类型驱动和数据驱动的算法</span><span class="sxs-lookup"><span data-stu-id="67adf-121">Tutorial: Use pattern matching to build type-driven and data-driven algorithms</span></span>](../../csharp/tutorials/pattern-matching.md)

<span data-ttu-id="67adf-122">添加了语言增强功能，以支持下面详细说明的 API 功能：</span><span class="sxs-lookup"><span data-stu-id="67adf-122">Language enhancements were added to support the following API features detailed below:</span></span>

- [<span data-ttu-id="67adf-123">范围和索引</span><span class="sxs-lookup"><span data-stu-id="67adf-123">Ranges and indices</span></span>](#ranges-and-indices)
- [<span data-ttu-id="67adf-124">异步流</span><span class="sxs-lookup"><span data-stu-id="67adf-124">Async streams</span></span>](#async-streams)

## <a name="net-standard-21"></a><span data-ttu-id="67adf-125">.NET Standard 2.1</span><span class="sxs-lookup"><span data-stu-id="67adf-125">.NET Standard 2.1</span></span>

<span data-ttu-id="67adf-126">.NET Core 3.0 已实现 .NET Standard 2.1。</span><span class="sxs-lookup"><span data-stu-id="67adf-126">.NET Core 3.0 implements **.NET Standard 2.1**.</span></span> <span data-ttu-id="67adf-127">但是，默认的 `dotnet new classlib` 模板还是会生成一个面向 .NET Standard 2.0 的项目。</span><span class="sxs-lookup"><span data-stu-id="67adf-127">However, the default `dotnet new classlib` template generates a project that still targets **.NET Standard 2.0**.</span></span> <span data-ttu-id="67adf-128">若要面向 **.NET Standard 2.1**，请编辑项目文件并将 `TargetFramework` 属性更改为 `netstandard2.1`：</span><span class="sxs-lookup"><span data-stu-id="67adf-128">To target **.NET Standard 2.1**, edit your project file and change the `TargetFramework` property to `netstandard2.1`:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netstandard2.1</TargetFramework>
  </PropertyGroup>

</Project>
```

<span data-ttu-id="67adf-129">如果使用 Visual Studio，则需要 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)，这是因为 Visual Studio 2017 不支持 .NET Standard 2.1 或 .NET Core 3.0 。</span><span class="sxs-lookup"><span data-stu-id="67adf-129">If you're using Visual Studio, you need [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019), as Visual Studio 2017 doesn't support **.NET Standard 2.1** or **.NET Core 3.0**.</span></span>

## <a name="compiledeploy"></a><span data-ttu-id="67adf-130">编译/部署</span><span class="sxs-lookup"><span data-stu-id="67adf-130">Compile/Deploy</span></span>

### <a name="default-executables"></a><span data-ttu-id="67adf-131">默认可执行文件</span><span class="sxs-lookup"><span data-stu-id="67adf-131">Default executables</span></span>

<span data-ttu-id="67adf-132">.NET Core 现在默认生成[依赖于框架的可执行文件](../deploying/index.md#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="67adf-132">.NET Core now builds [framework-dependent executables](../deploying/index.md#publish-framework-dependent) by default.</span></span> <span data-ttu-id="67adf-133">对于使用全局安装的 .NET Core 版本的应用程序而言，这是一种新行为。</span><span class="sxs-lookup"><span data-stu-id="67adf-133">This behavior is new for applications that use a globally installed version of .NET Core.</span></span> <span data-ttu-id="67adf-134">以前，仅[独立部署](../deploying/index.md#publish-self-contained)会生成可执行文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-134">Previously, only [self-contained deployments](../deploying/index.md#publish-self-contained) would produce an executable.</span></span>

<span data-ttu-id="67adf-135">在 `dotnet build` 或 `dotnet publish` 期间，将创建一个与你使用的 SDK 的环境和平台相匹配的可执行文件（即 appHost）。</span><span class="sxs-lookup"><span data-stu-id="67adf-135">During `dotnet build` or `dotnet publish`, an executable (known as the **appHost**) is created that matches the environment and platform of the SDK you're using.</span></span> <span data-ttu-id="67adf-136">和其他本机可执行文件一样，可以使用这些可执行文件执行相同操作，例如：</span><span class="sxs-lookup"><span data-stu-id="67adf-136">You can expect the same things with these executables as you would other native executables, such as:</span></span>

- <span data-ttu-id="67adf-137">可以双击可执行文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-137">You can double-click on the executable.</span></span>
- <span data-ttu-id="67adf-138">可以直接从命令提示符启用应用程序，如 Windows 上的 `myapp.exe`，以及 Linux 和 macOS 上的 `./myapp`。</span><span class="sxs-lookup"><span data-stu-id="67adf-138">You can launch the application from a command prompt directly, such as `myapp.exe` on Windows, and `./myapp` on Linux and macOS.</span></span>

### <a name="macos-apphost-and-notarization"></a><span data-ttu-id="67adf-139">macOS appHost 和公证</span><span class="sxs-lookup"><span data-stu-id="67adf-139">macOS appHost and notarization</span></span>

<span data-ttu-id="67adf-140">仅 macOS </span><span class="sxs-lookup"><span data-stu-id="67adf-140">*macOS only*</span></span>

<span data-ttu-id="67adf-141">从已公证的适用于 macOS 的 .NET Core SDK 3.0 开始，默认已禁用用于生成默认可执行文件（即 appHost）的设置。</span><span class="sxs-lookup"><span data-stu-id="67adf-141">Starting with the notarized .NET Core SDK 3.0 for macOS, the setting to produce a default executable (known as the appHost) is disabled by default.</span></span> <span data-ttu-id="67adf-142">有关详细信息，请参阅 [macOS Catalina 公证以及对 .NET Core 下载和项目的影响](../install/macos-notarization-issues.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-142">For more information, see [macOS Catalina Notarization and the impact on .NET Core downloads and projects](../install/macos-notarization-issues.md).</span></span>

<span data-ttu-id="67adf-143">启用 appHost 设置后，.NET Core 在生成或发布时将生成本机 Mach-O 可执行文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-143">When the appHost setting is enabled, .NET Core generates a native Mach-O executable when you build or publish.</span></span> <span data-ttu-id="67adf-144">如果使用 `dotnet run` 命令从源代码中运行应用，或通过启动 Mach-O 可执行文件直接运行应用，则应用会在 appHost 的上下文中运行。</span><span class="sxs-lookup"><span data-stu-id="67adf-144">Your app runs in the context of the appHost when it is run from source code with the `dotnet run` command, or by starting the Mach-O executable directly.</span></span>

<span data-ttu-id="67adf-145">如果没有 appHost，用户就只能使用 `dotnet <filename.dll>` 命令启动[依赖框架](../deploying/index.md#publish-framework-dependent)的应用。</span><span class="sxs-lookup"><span data-stu-id="67adf-145">Without the appHost, the only way a user can start a [framework-dependent](../deploying/index.md#publish-framework-dependent) app is with the `dotnet <filename.dll>` command.</span></span> <span data-ttu-id="67adf-146">发布[独立](../deploying/index.md#publish-self-contained)应用时，始终会创建 appHost。</span><span class="sxs-lookup"><span data-stu-id="67adf-146">An appHost is always created when you publish your app [self-contained](../deploying/index.md#publish-self-contained).</span></span>

<span data-ttu-id="67adf-147">可以在项目级别配置 appHost，或通过 `-p:UseAppHost` 参数切换特定 `dotnet` 命令的 appHost：</span><span class="sxs-lookup"><span data-stu-id="67adf-147">You can either configure the appHost at the project level, or toggle the appHost for a specific `dotnet` command with the `-p:UseAppHost` parameter:</span></span>

- <span data-ttu-id="67adf-148">项目文件</span><span class="sxs-lookup"><span data-stu-id="67adf-148">Project file</span></span>

  ```xml
  <PropertyGroup>
    <UseAppHost>true</UseAppHost>
  </PropertyGroup>
  ```

- <span data-ttu-id="67adf-149">命令行参数</span><span class="sxs-lookup"><span data-stu-id="67adf-149">Command-line parameter</span></span>

  ```dotnetcli
  dotnet run -p:UseAppHost=true
  ```

<span data-ttu-id="67adf-150">有关 `UseAppHost` 设置的详细信息，请参阅 [Microsoft.NET.Sdk 的 MSBuild 属性](../project-sdk/msbuild-props.md#useapphost)。</span><span class="sxs-lookup"><span data-stu-id="67adf-150">For more information about the `UseAppHost` setting, see [MSBuild properties for Microsoft.NET.Sdk](../project-sdk/msbuild-props.md#useapphost).</span></span>

### <a name="single-file-executables"></a><span data-ttu-id="67adf-151">单文件可执行文件</span><span class="sxs-lookup"><span data-stu-id="67adf-151">Single-file executables</span></span>

<span data-ttu-id="67adf-152">`dotnet publish` 命令支持将应用打包为特定于平台的单文件可执行文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-152">The `dotnet publish` command supports packaging your app into a platform-specific single-file executable.</span></span> <span data-ttu-id="67adf-153">该可执行文件是自解压缩文件，包含运行应用所需的所有依赖项（包括本机依赖项）。</span><span class="sxs-lookup"><span data-stu-id="67adf-153">The executable is self-extracting and contains all dependencies (including native) that are required to run your app.</span></span> <span data-ttu-id="67adf-154">首次运行应用时，应用程序将根据应用名称和生成标识符自解压缩到一个目录中。</span><span class="sxs-lookup"><span data-stu-id="67adf-154">When the app is first run, the application is extracted to a directory based on the app name and build identifier.</span></span> <span data-ttu-id="67adf-155">再次运行应用程序时，启动速度将变快。</span><span class="sxs-lookup"><span data-stu-id="67adf-155">Startup is faster when the application is run again.</span></span> <span data-ttu-id="67adf-156">除非使用了新版本，否则应用程序无需再次进行自解压缩。</span><span class="sxs-lookup"><span data-stu-id="67adf-156">The application doesn't need to extract itself a second time unless a new version was used.</span></span>

<span data-ttu-id="67adf-157">若要发布单文件可执行文件，请使用 `dotnet publish` 命令在项目或命令行中设置 `PublishSingleFile`：</span><span class="sxs-lookup"><span data-stu-id="67adf-157">To publish a single-file executable, set the `PublishSingleFile` in your project or on the command line with the `dotnet publish` command:</span></span>

```xml
<PropertyGroup>
  <RuntimeIdentifier>win10-x64</RuntimeIdentifier>
  <PublishSingleFile>true</PublishSingleFile>
</PropertyGroup>
```

<span data-ttu-id="67adf-158">\- 或 -</span><span class="sxs-lookup"><span data-stu-id="67adf-158">-or-</span></span>

```dotnetcli
dotnet publish -r win10-x64 -p:PublishSingleFile=true
```

<span data-ttu-id="67adf-159">有关单文件发布的详细信息，请参阅[单文件捆绑程序设计文档](https://github.com/dotnet/designs/blob/main/accepted/2020/single-file/design.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-159">For more information about single-file publishing, see the [single-file bundler design document](https://github.com/dotnet/designs/blob/main/accepted/2020/single-file/design.md).</span></span>

### <a name="assembly-linking"></a><span data-ttu-id="67adf-160">程序集链接</span><span class="sxs-lookup"><span data-stu-id="67adf-160">Assembly linking</span></span>

<span data-ttu-id="67adf-161">.NET core 3.0 SDK 随附了一种工具，可以通过分析 IL 并剪裁未使用的程序集来减小应用的大小。</span><span class="sxs-lookup"><span data-stu-id="67adf-161">The .NET core 3.0 SDK comes with a tool that can reduce the size of apps by analyzing IL and trimming unused assemblies.</span></span>

<span data-ttu-id="67adf-162">自包含应用包括运行代码所需的所有内容，而无需在主计算机上安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="67adf-162">Self-contained apps include everything needed to run your code, without requiring .NET to be installed on the host computer.</span></span> <span data-ttu-id="67adf-163">但是，很多时候应用只需要一小部分框架即可运行，并且可以删除其他未使用的库。</span><span class="sxs-lookup"><span data-stu-id="67adf-163">However, many times the app only requires a small subset of the framework to function, and other unused libraries could be removed.</span></span>

<span data-ttu-id="67adf-164">.NET Core 现在包含一个设置，将使用 [IL 链接器](https://github.com/mono/linker)工具扫描应用的 IL。</span><span class="sxs-lookup"><span data-stu-id="67adf-164">.NET Core now includes a setting that will use the [IL linker](https://github.com/mono/linker) tool to scan the IL of your app.</span></span> <span data-ttu-id="67adf-165">此工具将检测哪些代码是必需的，然后剪裁未使用的库。</span><span class="sxs-lookup"><span data-stu-id="67adf-165">This tool detects what code is required, and then trims unused libraries.</span></span> <span data-ttu-id="67adf-166">此工具可以显著减少某些应用的部署大小。</span><span class="sxs-lookup"><span data-stu-id="67adf-166">This tool can significantly reduce the deployment size of some apps.</span></span>

<span data-ttu-id="67adf-167">要启用此工具，请使用项目中的 `<PublishTrimmed>` 设置并发布自包含应用：</span><span class="sxs-lookup"><span data-stu-id="67adf-167">To enable this tool, add the `<PublishTrimmed>` setting in your project and publish a self-contained app:</span></span>

```xml
<PropertyGroup>
  <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

```dotnetcli
dotnet publish -r <rid> -c Release
```

<span data-ttu-id="67adf-168">例如，包含的基本“hello world”新控制台项目模板在发布时命中大小约为 70 MB。</span><span class="sxs-lookup"><span data-stu-id="67adf-168">As an example, the basic "hello world" new console project template that is included, when published, hits about 70 MB in size.</span></span> <span data-ttu-id="67adf-169">通过使用 `<PublishTrimmed>`，其大小将减少到约 30 MB。</span><span class="sxs-lookup"><span data-stu-id="67adf-169">By using `<PublishTrimmed>`, that size is reduced to about 30 MB.</span></span>

<span data-ttu-id="67adf-170">请务必考虑到使用反射或相关动态功能的应用程序或框架（包括 ASP.NET Core 和 WPF）通常会在剪裁时损坏。</span><span class="sxs-lookup"><span data-stu-id="67adf-170">It's important to consider that applications or frameworks (including ASP.NET Core and WPF) that use reflection or related dynamic features, will often break when trimmed.</span></span> <span data-ttu-id="67adf-171">发生此损坏是因为链接器不知道此动态行为，并且不能确定反射需要哪些框架类型。</span><span class="sxs-lookup"><span data-stu-id="67adf-171">This breakage occurs because the linker doesn't know about this dynamic behavior and can't determine which framework types are required for reflection.</span></span> <span data-ttu-id="67adf-172">可配置 IL 链接器工具以发现这种情况。</span><span class="sxs-lookup"><span data-stu-id="67adf-172">The IL Linker tool can be configured to be aware of this scenario.</span></span>

<span data-ttu-id="67adf-173">最重要的是，剪裁后务必对应用进行测试。</span><span class="sxs-lookup"><span data-stu-id="67adf-173">Above all else, be sure to test your app after trimming.</span></span>

<span data-ttu-id="67adf-174">有关 IL 链接器工具的详细信息，请参阅[文档](../deploying/trim-self-contained.md)，或访问 [mono/linker]( https://github.com/mono/linker) 存储库。</span><span class="sxs-lookup"><span data-stu-id="67adf-174">For more information about the IL Linker tool, see the [documentation](../deploying/trim-self-contained.md) or visit the [mono/linker]( https://github.com/mono/linker) repo.</span></span>

### <a name="tiered-compilation"></a><span data-ttu-id="67adf-175">分层编译</span><span class="sxs-lookup"><span data-stu-id="67adf-175">Tiered compilation</span></span>

<span data-ttu-id="67adf-176">.NET Core 3.0 中默认启用了[分层编译](https://github.com/dotnet/runtime/blob/main/docs/design/features/tiered-compilation.md) (TC)。</span><span class="sxs-lookup"><span data-stu-id="67adf-176">[Tiered compilation](https://github.com/dotnet/runtime/blob/main/docs/design/features/tiered-compilation.md) (TC) is on by default with .NET Core 3.0.</span></span> <span data-ttu-id="67adf-177">此功能使运行时能够更适应地使用实时 (JIT) 编译器来实现更好的性能。</span><span class="sxs-lookup"><span data-stu-id="67adf-177">This feature enables the runtime to more adaptively use the just-in-time (JIT) compiler to achieve better performance.</span></span>

<span data-ttu-id="67adf-178">分层编译的主要优势是提供两种实现实时的方法，可在低质量快速层或高质量慢速层中编译。</span><span class="sxs-lookup"><span data-stu-id="67adf-178">The main benefit of tiered compilation is to provide two ways of jitting methods: in a lower-quality-but-faster tier or a higher-quality-but-slower tier.</span></span> <span data-ttu-id="67adf-179">质量是指方法的优化程度。</span><span class="sxs-lookup"><span data-stu-id="67adf-179">The quality refers to how well the method is optimized.</span></span> <span data-ttu-id="67adf-180">这有助于提高应用程序在从启动到稳定状态的各个执行阶段的性能。</span><span class="sxs-lookup"><span data-stu-id="67adf-180">TC helps to improve the performance of an application as it goes through various stages of execution, from startup through steady state.</span></span> <span data-ttu-id="67adf-181">禁用分层编译后，每种方法都以同一种方式进行编译，这种方式倾向于牺牲启动性能来保证稳定状态性能。</span><span class="sxs-lookup"><span data-stu-id="67adf-181">When tiered compilation is disabled, every method is compiled in a single way that's biased to steady-state performance over startup performance.</span></span>

<span data-ttu-id="67adf-182">启用 TC 后，以下行为适用于应用启动时的方法编译：</span><span class="sxs-lookup"><span data-stu-id="67adf-182">When TC is enabled, the following behavior applies for method compilation when an app starts up:</span></span>

- <span data-ttu-id="67adf-183">如果方法具有预先编译的代码 ([ReadyToRun](#readytorun-images))，将使用预生成的代码。</span><span class="sxs-lookup"><span data-stu-id="67adf-183">If the method has ahead-of-time-compiled code, or [ReadyToRun](#readytorun-images), the pregenerated code is used.</span></span>
- <span data-ttu-id="67adf-184">否则，将实时编译该方法。</span><span class="sxs-lookup"><span data-stu-id="67adf-184">Otherwise, the method is jitted.</span></span> <span data-ttu-id="67adf-185">一般来说，这些方法是泛型而不是值类型。</span><span class="sxs-lookup"><span data-stu-id="67adf-185">Typically, these methods are generics over value types.</span></span>
  - <span data-ttu-id="67adf-186">快速 JIT 可以更快地生成较低质量（优化程度较低）的代码。</span><span class="sxs-lookup"><span data-stu-id="67adf-186">*Quick JIT* produces lower-quality (or less optimized) code more quickly.</span></span> <span data-ttu-id="67adf-187">在 .NET Core 3.0 中，默认为不包含循环的方法启用了快速 JIT，并且启动过程中首选快速 JIT。</span><span class="sxs-lookup"><span data-stu-id="67adf-187">In .NET Core 3.0, Quick JIT is enabled by default for methods that don't contain loops and is preferred during startup.</span></span>
  - <span data-ttu-id="67adf-188">完全优化的 JIT 可生成更高质量（优化程度更高）的代码，但速度更慢。</span><span class="sxs-lookup"><span data-stu-id="67adf-188">The fully optimizing JIT produces higher-quality (or more optimized) code more slowly.</span></span> <span data-ttu-id="67adf-189">对于不使用快速 JIT 的方法（例如，如果该方法具有 <xref:System.Runtime.CompilerServices.MethodImplOptions.AggressiveOptimization?displayProperty=nameWithType> 特性），则使用完全优化的 JIT。</span><span class="sxs-lookup"><span data-stu-id="67adf-189">For methods where Quick JIT would not be used (for example, if the method is attributed with <xref:System.Runtime.CompilerServices.MethodImplOptions.AggressiveOptimization?displayProperty=nameWithType>), the fully optimizing JIT is used.</span></span>

<span data-ttu-id="67adf-190">对于频繁调用的方法，实时编译器最终会在后台创建完全优化的代码。</span><span class="sxs-lookup"><span data-stu-id="67adf-190">For frequently called methods, the just-in-time compiler eventually creates fully optimized code in the background.</span></span> <span data-ttu-id="67adf-191">然后，优化后的代码将替换该方法的预编译代码。</span><span class="sxs-lookup"><span data-stu-id="67adf-191">The optimized code then replaces the pre-compiled code for that method.</span></span>

<span data-ttu-id="67adf-192">通过快速 JIT 生成的代码可能会运行较慢、分配更多内存或使用更多堆栈空间。</span><span class="sxs-lookup"><span data-stu-id="67adf-192">Code generated by Quick JIT may run slower, allocate more memory, or use more stack space.</span></span> <span data-ttu-id="67adf-193">如果出现问题，可以在项目文件中使用此 MSBuild 属性禁用快速 JIT：</span><span class="sxs-lookup"><span data-stu-id="67adf-193">If there are issues, you can disabled Quick JIT using this MSBuild property in the project file:</span></span>

```xml
<PropertyGroup>
  <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
</PropertyGroup>
```

<span data-ttu-id="67adf-194">若要完全禁用 TC，请在项目文件中使用此 MSBuild 属性：</span><span class="sxs-lookup"><span data-stu-id="67adf-194">To disable TC completely, use this MSBuild property in your project file:</span></span>

```xml
<PropertyGroup>
  <TieredCompilation>false</TieredCompilation>
</PropertyGroup>
```

> [!TIP]
> <span data-ttu-id="67adf-195">如果在项目文件中更改这些设置，则可能需要执行干净的生成以反映新的设置（删除 `obj` 和 `bin` 目录并重新生成）。</span><span class="sxs-lookup"><span data-stu-id="67adf-195">If you change these settings in the project file, you may need to perform a clean build for the new settings to be reflected (delete the `obj` and `bin` directories and rebuild).</span></span>

<span data-ttu-id="67adf-196">有关在运行时配置编译的详细信息，请参阅[用于编译的运行时配置选项](../run-time-config/compilation.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-196">For more information about configuring compilation at run time, see [Run-time configuration options for compilation](../run-time-config/compilation.md).</span></span>

### <a name="readytorun-images"></a><span data-ttu-id="67adf-197">ReadyToRun 映像</span><span class="sxs-lookup"><span data-stu-id="67adf-197">ReadyToRun images</span></span>

<span data-ttu-id="67adf-198">可以通过将应用程序集编译为 ReadyToRun (R2R) 格式来改进.NET Core 应用程序的启动时间。</span><span class="sxs-lookup"><span data-stu-id="67adf-198">You can improve the startup time of your .NET Core application by compiling your application assemblies as ReadyToRun (R2R) format.</span></span> <span data-ttu-id="67adf-199">R2R 是一种预先 (AOT) 编译形式。</span><span class="sxs-lookup"><span data-stu-id="67adf-199">R2R is a form of ahead-of-time (AOT) compilation.</span></span>

<span data-ttu-id="67adf-200">R2R 二进制文件通过减少应用程序加载时实时 (JIT) 编译器需要执行的工作量来改进启动性能。</span><span class="sxs-lookup"><span data-stu-id="67adf-200">R2R binaries improve startup performance by reducing the amount of work the just-in-time (JIT) compiler needs to do as your application loads.</span></span> <span data-ttu-id="67adf-201">二进制文件包含与 JIT 将生成的内容类似的本机代码。</span><span class="sxs-lookup"><span data-stu-id="67adf-201">The binaries contain similar native code compared to what the JIT would produce.</span></span> <span data-ttu-id="67adf-202">但是，R2R 二进制文件更大，因为它们包含中间语言 (IL) 代码（某些情况下仍需要此代码）和相同代码的本机版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-202">However, R2R binaries are larger because they contain both intermediate language (IL) code, which is still needed for some scenarios, and the native version of the same code.</span></span> <span data-ttu-id="67adf-203">仅当发布面向特定运行时环境 (RID)（如 Linux x64 或 Windows x64）的自包含应用时 R2R 才可用。</span><span class="sxs-lookup"><span data-stu-id="67adf-203">R2R is only available when you publish a self-contained app that targets specific runtime environments (RID) such as Linux x64 or Windows x64.</span></span>

<span data-ttu-id="67adf-204">若要将项目编译为 ReadyToRun，请执行以下操作：</span><span class="sxs-lookup"><span data-stu-id="67adf-204">To compile your project as ReadyToRun, do the following:</span></span>

01. <span data-ttu-id="67adf-205">向项目中添加 `<PublishReadyToRun>` 设置：</span><span class="sxs-lookup"><span data-stu-id="67adf-205">Add the `<PublishReadyToRun>` setting to your project:</span></span>

    ```xml
    <PropertyGroup>
      <PublishReadyToRun>true</PublishReadyToRun>
    </PropertyGroup>
    ```

01. <span data-ttu-id="67adf-206">发布自包含应用。</span><span class="sxs-lookup"><span data-stu-id="67adf-206">Publish a self-contained app.</span></span> <span data-ttu-id="67adf-207">例如，此命令将创建适用于 Windows 64 位版本的自包含应用：</span><span class="sxs-lookup"><span data-stu-id="67adf-207">For example, this command creates a self-contained app for the 64-bit version of Windows:</span></span>

    ```dotnetcli
    dotnet publish -c Release -r win-x64 --self-contained
    ```

#### <a name="cross-platformarchitecture-restrictions"></a><span data-ttu-id="67adf-208">跨平台/体系结构限制</span><span class="sxs-lookup"><span data-stu-id="67adf-208">Cross platform/architecture restrictions</span></span>

<span data-ttu-id="67adf-209">ReadyToRun 编译器当前不支持跨目标。</span><span class="sxs-lookup"><span data-stu-id="67adf-209">The ReadyToRun compiler doesn't currently support cross-targeting.</span></span> <span data-ttu-id="67adf-210">必须在给定的目标上编译。</span><span class="sxs-lookup"><span data-stu-id="67adf-210">You must compile on a given target.</span></span> <span data-ttu-id="67adf-211">例如，如果想要 Windows x64 R2R 映像，需要在该环境中运行发布命令。</span><span class="sxs-lookup"><span data-stu-id="67adf-211">For example, if you want R2R images for Windows x64, you need to run the publish command on that environment.</span></span>

<span data-ttu-id="67adf-212">跨目标的例外情况：</span><span class="sxs-lookup"><span data-stu-id="67adf-212">Exceptions to cross-targeting:</span></span>

- <span data-ttu-id="67adf-213">可以使用 Windows x64 编译 Windows ARM32、ARM64 和 x86 映像。</span><span class="sxs-lookup"><span data-stu-id="67adf-213">Windows x64 can be used to compile Windows ARM32, ARM64, and x86 images.</span></span>
- <span data-ttu-id="67adf-214">可以使用 Windows x86 编译 Windows ARM32 映像。</span><span class="sxs-lookup"><span data-stu-id="67adf-214">Windows x86 can be used to compile Windows ARM32 images.</span></span>
- <span data-ttu-id="67adf-215">可以使用 Linux x64 编译 Linux ARM32 和 ARM64 映像。</span><span class="sxs-lookup"><span data-stu-id="67adf-215">Linux x64 can be used to compile Linux ARM32 and ARM64 images.</span></span>

<span data-ttu-id="67adf-216">有关详细信息，请参阅[准备好运行](../deploying/ready-to-run.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-216">For more information, see [Ready to Run](../deploying/ready-to-run.md).</span></span>

## <a name="runtimesdk"></a><span data-ttu-id="67adf-217">运行时/SDK</span><span class="sxs-lookup"><span data-stu-id="67adf-217">Runtime/SDK</span></span>

### <a name="major-version-runtime-roll-forward"></a><span data-ttu-id="67adf-218">主要版本运行时前滚</span><span class="sxs-lookup"><span data-stu-id="67adf-218">Major-version runtime roll forward</span></span>

<span data-ttu-id="67adf-219">.NET Core 3.0 引入了一项选择加入功能，该功能允许应用前滚到 .NET Core 最新的主要版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-219">.NET Core 3.0 introduces an opt-in feature that allows your app to roll forward to the latest major version of .NET Core.</span></span> <span data-ttu-id="67adf-220">此外，还添加了一项新设置来控制如何将前滚应用于应用。</span><span class="sxs-lookup"><span data-stu-id="67adf-220">Additionally, a new setting has been added to control how roll forward is applied to your app.</span></span> <span data-ttu-id="67adf-221">这可以通过以下方式配置：</span><span class="sxs-lookup"><span data-stu-id="67adf-221">This can be configured in the following ways:</span></span>

- <span data-ttu-id="67adf-222">项目文件属性：`RollForward`</span><span class="sxs-lookup"><span data-stu-id="67adf-222">Project file property: `RollForward`</span></span>
- <span data-ttu-id="67adf-223">运行时配置文件属性：`rollForward`</span><span class="sxs-lookup"><span data-stu-id="67adf-223">Run-time configuration file property: `rollForward`</span></span>
- <span data-ttu-id="67adf-224">环境变量：`DOTNET_ROLL_FORWARD`</span><span class="sxs-lookup"><span data-stu-id="67adf-224">Environment variable: `DOTNET_ROLL_FORWARD`</span></span>
- <span data-ttu-id="67adf-225">命令行参数：`--roll-forward`</span><span class="sxs-lookup"><span data-stu-id="67adf-225">Command-line argument: `--roll-forward`</span></span>

<span data-ttu-id="67adf-226">必须指定以下值之一。</span><span class="sxs-lookup"><span data-stu-id="67adf-226">One of the following values must be specified.</span></span> <span data-ttu-id="67adf-227">如果省略该设置，则默认值为“Minor”。</span><span class="sxs-lookup"><span data-stu-id="67adf-227">If the setting is omitted, **Minor** is the default.</span></span>

- <span data-ttu-id="67adf-228">**LatestPatch**</span><span class="sxs-lookup"><span data-stu-id="67adf-228">**LatestPatch**</span></span>\
<span data-ttu-id="67adf-229">前滚到最高补丁版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-229">Roll forward to the highest patch version.</span></span> <span data-ttu-id="67adf-230">这会禁用次要版本前滚。</span><span class="sxs-lookup"><span data-stu-id="67adf-230">This disables minor version roll forward.</span></span>
- <span data-ttu-id="67adf-231">**Minor**</span><span class="sxs-lookup"><span data-stu-id="67adf-231">**Minor**</span></span>\
<span data-ttu-id="67adf-232">如果缺少所请求的次要版本，则前滚到最低的较高次要版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-232">Roll forward to the lowest higher minor version, if requested minor version is missing.</span></span> <span data-ttu-id="67adf-233">如果存在所请求的次要版本，则使用 **LatestPatch** 策略。</span><span class="sxs-lookup"><span data-stu-id="67adf-233">If the requested minor version is present, then the **LatestPatch** policy is used.</span></span>
- <span data-ttu-id="67adf-234">**Major**</span><span class="sxs-lookup"><span data-stu-id="67adf-234">**Major**</span></span>\
<span data-ttu-id="67adf-235">如果缺少所请求的主要版本，则前滚到最低的较高主要版本和最低的次要版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-235">Roll forward to lowest higher major version, and lowest minor version, if requested major version is missing.</span></span> <span data-ttu-id="67adf-236">如果存在所请求的主要版本，则使用 **Minor** 策略。</span><span class="sxs-lookup"><span data-stu-id="67adf-236">If the requested major version is present, then the **Minor** policy is used.</span></span>
- <span data-ttu-id="67adf-237">**LatestMinor**</span><span class="sxs-lookup"><span data-stu-id="67adf-237">**LatestMinor**</span></span>\
<span data-ttu-id="67adf-238">即使存在所请求的次要版本，仍前滚到最高次要版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-238">Roll forward to highest minor version, even if requested minor version is present.</span></span> <span data-ttu-id="67adf-239">适用于组件托管方案。</span><span class="sxs-lookup"><span data-stu-id="67adf-239">Intended for component hosting scenarios.</span></span>
- <span data-ttu-id="67adf-240">**LatestMajor**</span><span class="sxs-lookup"><span data-stu-id="67adf-240">**LatestMajor**</span></span>\
<span data-ttu-id="67adf-241">即使存在所请求的主要版本，仍前滚到最高主要版本和最高次要版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-241">Roll forward to highest major and highest minor version, even if requested major is present.</span></span> <span data-ttu-id="67adf-242">适用于组件托管方案。</span><span class="sxs-lookup"><span data-stu-id="67adf-242">Intended for component hosting scenarios.</span></span>
- <span data-ttu-id="67adf-243">**Disable**</span><span class="sxs-lookup"><span data-stu-id="67adf-243">**Disable**</span></span>\
<span data-ttu-id="67adf-244">不前滚。</span><span class="sxs-lookup"><span data-stu-id="67adf-244">Don't roll forward.</span></span> <span data-ttu-id="67adf-245">仅绑定到指定的版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-245">Only bind to specified version.</span></span> <span data-ttu-id="67adf-246">建议不要将此策略用于一般用途，因为它会禁用前滚到最新补丁的功能。</span><span class="sxs-lookup"><span data-stu-id="67adf-246">This policy isn't recommended for general use because it disables the ability to roll forward to the latest patches.</span></span> <span data-ttu-id="67adf-247">该值仅建议用于测试。</span><span class="sxs-lookup"><span data-stu-id="67adf-247">This value is only recommended for testing.</span></span>

<span data-ttu-id="67adf-248">除“Disable”设置外，所有设置都将使用可用的最高补丁版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-248">Besides the **Disable** setting, all settings will use the highest available patch version.</span></span>

<span data-ttu-id="67adf-249">默认情况下，如果请求的版本（`.runtimeconfig.json` 中为应用程序指定的版本）为发布版本，则前滚时只考虑发布版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-249">By default, if the requested version (as specified in `.runtimeconfig.json` for the application) is a release version, only release versions are considered for roll forward.</span></span> <span data-ttu-id="67adf-250">将忽略所有预发布版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-250">Any pre-release versions are ignored.</span></span> <span data-ttu-id="67adf-251">如果没有匹配的发布版本，则会考虑使用预发布版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-251">If there is no matching release version, then pre-release versions are taken into account.</span></span> <span data-ttu-id="67adf-252">可以通过设置 `DOTNET_ROLL_FORWARD_TO_PRERELEASE=1` 来更改此行为，在这种情况下，将始终考虑所有版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-252">This behavior can be changed by setting `DOTNET_ROLL_FORWARD_TO_PRERELEASE=1`, in which case all versions are always considered.</span></span>

### <a name="build-copies-dependencies"></a><span data-ttu-id="67adf-253">生成会复制依赖项</span><span class="sxs-lookup"><span data-stu-id="67adf-253">Build copies dependencies</span></span>

<span data-ttu-id="67adf-254">`dotnet build` 命令现在将应用程序的 NuGet 依赖项从 NuGet 缓存复制到生成输出文件夹。</span><span class="sxs-lookup"><span data-stu-id="67adf-254">The `dotnet build` command now copies NuGet dependencies for your application from the NuGet cache to the build output folder.</span></span> <span data-ttu-id="67adf-255">此前，依赖项仅作为 `dotnet publish` 的一部分复制。</span><span class="sxs-lookup"><span data-stu-id="67adf-255">Previously, dependencies were only copied as part of `dotnet publish`.</span></span>

<span data-ttu-id="67adf-256">有些操作，比如链接和 razor 页面发布仍需要发布。</span><span class="sxs-lookup"><span data-stu-id="67adf-256">There are some operations, like linking and razor page publishing that will still require publishing.</span></span>

### <a name="local-tools"></a><span data-ttu-id="67adf-257">本地工具</span><span class="sxs-lookup"><span data-stu-id="67adf-257">Local tools</span></span>

<span data-ttu-id="67adf-258">.NET Core 3.0 引入了本地工具。</span><span class="sxs-lookup"><span data-stu-id="67adf-258">.NET Core 3.0 introduces local tools.</span></span> <span data-ttu-id="67adf-259">本地工具类似于[全局工具](../tools/global-tools.md)，但与磁盘上的特定位置相关联。</span><span class="sxs-lookup"><span data-stu-id="67adf-259">Local tools are similar to [global tools](../tools/global-tools.md) but are associated with a particular location on disk.</span></span> <span data-ttu-id="67adf-260">本地工具在全局范围内不可用，并作为 NuGet 包进行分发。</span><span class="sxs-lookup"><span data-stu-id="67adf-260">Local tools aren't available globally and are distributed as NuGet packages.</span></span>

> [!WARNING]
> <span data-ttu-id="67adf-261">如果尝试使用过 .NET Core 3.0 预览版 1 中的本地工具，例如运行 `dotnet tool restore` 或 `dotnet tool install`，请删除本地工具缓存文件夹。</span><span class="sxs-lookup"><span data-stu-id="67adf-261">If you tried local tools in .NET Core 3.0 Preview 1, such as running `dotnet tool restore` or `dotnet tool install`, delete the local tools cache folder.</span></span> <span data-ttu-id="67adf-262">否则，本地工具将无法在任何较新的版本上运行。</span><span class="sxs-lookup"><span data-stu-id="67adf-262">Otherwise, local tools won't work on any newer release.</span></span> <span data-ttu-id="67adf-263">此文件夹位于：</span><span class="sxs-lookup"><span data-stu-id="67adf-263">This folder is located at:</span></span>
>
> <span data-ttu-id="67adf-264">在 macOS、Linux 上：`rm -r $HOME/.dotnet/toolResolverCache`</span><span class="sxs-lookup"><span data-stu-id="67adf-264">On macOS, Linux: `rm -r $HOME/.dotnet/toolResolverCache`</span></span>
>
> <span data-ttu-id="67adf-265">在 Windows 上：`rmdir /s %USERPROFILE%\.dotnet\toolResolverCache`</span><span class="sxs-lookup"><span data-stu-id="67adf-265">On Windows: `rmdir /s %USERPROFILE%\.dotnet\toolResolverCache`</span></span>

<span data-ttu-id="67adf-266">本地工具依赖于当前目录中名为 `dotnet-tools.json` 的清单文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-266">Local tools rely on a manifest file name `dotnet-tools.json` in your current directory.</span></span> <span data-ttu-id="67adf-267">此清单文件定义在该文件夹和以下文件夹中可用的工具。</span><span class="sxs-lookup"><span data-stu-id="67adf-267">This manifest file defines the tools to be available at that folder and below.</span></span> <span data-ttu-id="67adf-268">你可以随代码一起分发清单文件，以确保使用代码的任何人都可以还原和使用相同的工具。</span><span class="sxs-lookup"><span data-stu-id="67adf-268">You can distribute the manifest file with your code to ensure that anyone who works with your code can restore and use the same tools.</span></span>

<span data-ttu-id="67adf-269">对于全局工具和本地工具，需要一个兼容的运行时版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-269">For both global and local tools, a compatible version of the runtime is required.</span></span> <span data-ttu-id="67adf-270">目前，NuGet.org 上的许多工具都面向 .NET Core Runtime 2.1。</span><span class="sxs-lookup"><span data-stu-id="67adf-270">Many tools currently on NuGet.org target .NET Core Runtime 2.1.</span></span> <span data-ttu-id="67adf-271">若要在全局范围或本地安装这些工具，仍需要安装 [NET Core 2.1 运行时](https://dotnet.microsoft.com/download/dotnet/2.1)。</span><span class="sxs-lookup"><span data-stu-id="67adf-271">To install these tools globally or locally, you would still need to install the [NET Core 2.1 Runtime](https://dotnet.microsoft.com/download/dotnet/2.1).</span></span>

### <a name="new-globaljson-options"></a><span data-ttu-id="67adf-272">新 global.json 选项</span><span class="sxs-lookup"><span data-stu-id="67adf-272">New global.json options</span></span>

<span data-ttu-id="67adf-273">global.json 文件包含新选项，当你尝试定义所使用的 .NET Core SDK 版本时，这些选项可提供更大的灵活性。</span><span class="sxs-lookup"><span data-stu-id="67adf-273">The *global.json* file has new options that provide more flexibility when you're trying to define which version of the .NET Core SDK is used.</span></span> <span data-ttu-id="67adf-274">新选项包括：</span><span class="sxs-lookup"><span data-stu-id="67adf-274">The new options are:</span></span>

- <span data-ttu-id="67adf-275">`allowPrerelease`：指示在选择要使用的 SDK 版本时，SDK 解析程序是否应考虑预发布版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-275">`allowPrerelease`: Indicates whether the SDK resolver should consider prerelease versions when selecting the SDK version to use.</span></span>
- <span data-ttu-id="67adf-276">`rollForward`：指示选择 SDK 版本时要使用的前滚策略，可作为特定 SDK 版本缺失时的回退，或者作为使用更高版本的指令。</span><span class="sxs-lookup"><span data-stu-id="67adf-276">`rollForward`: Indicates the roll-forward policy to use when selecting an SDK version, either as a fallback when a specific SDK version is missing or as a directive to use a higher version.</span></span>

<span data-ttu-id="67adf-277">有关这些更改的详细信息（包括默认值、支持的值和新的匹配规则），请参阅 [global.json 概述](../tools/global-json.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-277">For more information about the changes including default values, supported values, and new matching rules, see [global.json overview](../tools/global-json.md).</span></span>

### <a name="smaller-garbage-collection-heap-sizes"></a><span data-ttu-id="67adf-278">垃圾回收堆大小减小</span><span class="sxs-lookup"><span data-stu-id="67adf-278">Smaller Garbage Collection heap sizes</span></span>

<span data-ttu-id="67adf-279">垃圾回收器的默认堆大小已减小，以使 .NET Core 使用更少的内存。</span><span class="sxs-lookup"><span data-stu-id="67adf-279">The Garbage Collector's default heap size has been reduced resulting in .NET Core using less memory.</span></span> <span data-ttu-id="67adf-280">此更改更符合具有现代处理器缓存大小的第 0 代分配预算。</span><span class="sxs-lookup"><span data-stu-id="67adf-280">This change better aligns with the generation 0 allocation budget with modern processor cache sizes.</span></span>

### <a name="garbage-collection-large-page-support"></a><span data-ttu-id="67adf-281">垃圾回收大型页面支持</span><span class="sxs-lookup"><span data-stu-id="67adf-281">Garbage Collection Large Page support</span></span>

<span data-ttu-id="67adf-282">大型页面（也称为 Linux 上的巨型页面）是一项功能，其中操作系统能够建立大于本机页面大小（通常为 4K）的内存区域，以提高请求这些大型页面的应用程序的性能。</span><span class="sxs-lookup"><span data-stu-id="67adf-282">Large Pages (also known as Huge Pages on Linux) is a feature where the operating system is able to establish memory regions larger than the native page size (often 4K) to improve performance of the application requesting these large pages.</span></span>

<span data-ttu-id="67adf-283">现在可以使用 **GCLargePages** 设置将垃圾回收器配置为一项选择加入功能，以选择在 Windows 上分配大型页面。</span><span class="sxs-lookup"><span data-stu-id="67adf-283">The Garbage Collector can now be configured with the **GCLargePages** setting as an opt-in feature to choose to allocate large pages on Windows.</span></span>

## <a name="windows-desktop--com"></a><span data-ttu-id="67adf-284">Windows 桌面和 COM</span><span class="sxs-lookup"><span data-stu-id="67adf-284">Windows Desktop & COM</span></span>

### <a name="net-core-sdk-windows-installer"></a><span data-ttu-id="67adf-285">.NET Core SDK Windows Installer</span><span class="sxs-lookup"><span data-stu-id="67adf-285">.NET Core SDK Windows Installer</span></span>

<span data-ttu-id="67adf-286">用于 Windows 的 MSI 安装程序已从 .NET Core 3.0 开始更改。</span><span class="sxs-lookup"><span data-stu-id="67adf-286">The MSI installer for Windows has changed starting with .NET Core 3.0.</span></span> <span data-ttu-id="67adf-287">SDK 安装程序现在将对 SDK 功能区段版本进行就地升级。</span><span class="sxs-lookup"><span data-stu-id="67adf-287">The SDK installers will now upgrade SDK feature-band releases in place.</span></span> <span data-ttu-id="67adf-288">功能区段在版本号的 *补丁* 部分中的 *百数* 组中定义。</span><span class="sxs-lookup"><span data-stu-id="67adf-288">Feature bands are defined in the *hundreds* groups in the *patch* section of the version number.</span></span> <span data-ttu-id="67adf-289">例如，**3.0.101** 和 **3.0.201** 是两个不同功能区段中的版本，而 **3.0.101** 和 **3.0.199** 则属于同一个功能区段。   </span><span class="sxs-lookup"><span data-stu-id="67adf-289">For example, **3.0._101_** and **3.0._201_** are versions in two different feature bands while **3.0._101_** and **3.0._199_** are in the same feature band.</span></span> <span data-ttu-id="67adf-290">并且，当安装 .NET Core SDK **3.0.101** 时，将从计算机中删除 .NET Core SDK **3.0.100** （如果存在）。 </span><span class="sxs-lookup"><span data-stu-id="67adf-290">And, when .NET Core SDK **3.0._101_** is installed, .NET Core SDK **3.0._100_** will be removed from the machine if it exists.</span></span> <span data-ttu-id="67adf-291">当 .NET Core SDK **3.0.200** 安装在同一台计算机上时，不会删除 .NET Core SDK **3.0.101** 。 </span><span class="sxs-lookup"><span data-stu-id="67adf-291">When .NET Core SDK **3.0._200_** is installed on the same machine, .NET Core SDK **3.0._101_** won't be removed.</span></span>

<span data-ttu-id="67adf-292">有关版本控制的详细信息，请参阅 [.NET Core 的版本控制方式概述](../versions/index.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-292">For more information about versioning, see [Overview of how .NET Core is versioned](../versions/index.md).</span></span>

### <a name="windows-desktop"></a><span data-ttu-id="67adf-293">Windows 桌面</span><span class="sxs-lookup"><span data-stu-id="67adf-293">Windows desktop</span></span>

<span data-ttu-id="67adf-294">.NET Core 3.0 支持使用 Windows Presentation Foundation (WPF) 和 Windows 窗体的 Windows 桌面应用程序。</span><span class="sxs-lookup"><span data-stu-id="67adf-294">.NET Core 3.0 supports Windows desktop applications using Windows Presentation Foundation (WPF) and Windows Forms.</span></span> <span data-ttu-id="67adf-295">这些框架还支持通过 [XAML 岛](/windows/uwp/xaml-platform/xaml-host-controls)从 Windows UI XAML 库 (WinUI) 使用新式控件和 Fluent 样式。</span><span class="sxs-lookup"><span data-stu-id="67adf-295">These frameworks also support using modern controls and Fluent styling from the Windows UI XAML Library (WinUI) via [XAML islands](/windows/uwp/xaml-platform/xaml-host-controls).</span></span>

<span data-ttu-id="67adf-296">Windows 桌面部件是 Windows .NET Core 3.0 SDK 的一部分。</span><span class="sxs-lookup"><span data-stu-id="67adf-296">The Windows Desktop component is part of the Windows .NET Core 3.0 SDK.</span></span>

<span data-ttu-id="67adf-297">可以使用以下 `dotnet` 命令创建新的 WPF 或 Windows 窗体应用：</span><span class="sxs-lookup"><span data-stu-id="67adf-297">You can create a new WPF or Windows Forms app with the following `dotnet` commands:</span></span>

```dotnetcli
dotnet new wpf
dotnet new winforms
```

<span data-ttu-id="67adf-298">Visual Studio 2019 添加了适用于 .NET Core 3.0 Windows 窗体和 WPF 的“新建项目”模板。</span><span class="sxs-lookup"><span data-stu-id="67adf-298">Visual Studio 2019 adds **New Project** templates for .NET Core 3.0 Windows Forms and WPF.</span></span>

<span data-ttu-id="67adf-299">有关如何移植现有 .NET Framework 应用程序的详细信息，请参阅[移植 WPF 项目](/dotnet/desktop/wpf/migration/convert-project-from-net-framework)和[移植 Windows 窗体项目](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true)。</span><span class="sxs-lookup"><span data-stu-id="67adf-299">For more information about how to port an existing .NET Framework application, see [Port WPF projects](/dotnet/desktop/wpf/migration/convert-project-from-net-framework) and [Port Windows Forms projects](/dotnet/desktop/winforms/migration/?view=netdesktop-5.0&preserve-view=true).</span></span>

#### <a name="winforms-high-dpi"></a><span data-ttu-id="67adf-300">WinForms 高 DPI</span><span class="sxs-lookup"><span data-stu-id="67adf-300">WinForms high DPI</span></span>

<span data-ttu-id="67adf-301">.NET Core Windows 窗体应用程序可以使用 <xref:System.Windows.Forms.Application.SetHighDpiMode(System.Windows.Forms.HighDpiMode)?displayProperty=nameWithType> 设置高 DPI 模式。</span><span class="sxs-lookup"><span data-stu-id="67adf-301">.NET Core Windows Forms applications can set high DPI mode with <xref:System.Windows.Forms.Application.SetHighDpiMode(System.Windows.Forms.HighDpiMode)?displayProperty=nameWithType>.</span></span> <span data-ttu-id="67adf-302">`SetHighDpiMode` 方法可设置相应的高 DPI 模式，除非该设置已通过其他方式（例如使用 `App.Manifest` 或在 `Application.Run` 前面使用 P/Invoke）进行设置。</span><span class="sxs-lookup"><span data-stu-id="67adf-302">The `SetHighDpiMode` method sets the corresponding high DPI mode unless the setting has been set by other means like `App.Manifest` or P/Invoke before `Application.Run`.</span></span>

<span data-ttu-id="67adf-303">由 <xref:System.Windows.Forms.HighDpiMode?displayProperty=nameWithType> 枚举表示的可能的 `highDpiMode` 值包括：</span><span class="sxs-lookup"><span data-stu-id="67adf-303">The possible `highDpiMode` values, as expressed by the <xref:System.Windows.Forms.HighDpiMode?displayProperty=nameWithType> enum are:</span></span>

- `DpiUnaware`
- `SystemAware`
- `PerMonitor`
- `PerMonitorV2`
- `DpiUnawareGdiScaled`

<span data-ttu-id="67adf-304">有关高 DPI 模式的详细信息，请参阅[在 Windows 上开发高 DPI 桌面应用程序](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows)。</span><span class="sxs-lookup"><span data-stu-id="67adf-304">For more information about high DPI modes, see [High DPI Desktop Application Development on Windows](/windows/desktop/hidpi/high-dpi-desktop-application-development-on-windows).</span></span>

### <a name="create-com-components"></a><span data-ttu-id="67adf-305">创建 COM 组件</span><span class="sxs-lookup"><span data-stu-id="67adf-305">Create COM components</span></span>

<span data-ttu-id="67adf-306">在 Windows 上，现在可以创建可调用 COM 的托管组件。</span><span class="sxs-lookup"><span data-stu-id="67adf-306">On Windows, you can now create COM-callable managed components.</span></span> <span data-ttu-id="67adf-307">在将 .NET Core 与 COM 加载项模型结合使用，以及使用 .NET Framework 提供奇偶校验时，此功能至关重要。</span><span class="sxs-lookup"><span data-stu-id="67adf-307">This capability is critical to use .NET Core with COM add-in models and also to provide parity with .NET Framework.</span></span>

<span data-ttu-id="67adf-308">与将 *mscoree.dll* 用作 COM 服务器的 .NET Framework 不同，.NET Core 将在生成 COM 组件时向 *bin* 目录添加本机启动程序 dll。</span><span class="sxs-lookup"><span data-stu-id="67adf-308">Unlike .NET Framework where the *mscoree.dll* was used as the COM server, .NET Core will add a native launcher dll to the *bin* directory when you build your COM component.</span></span>

<span data-ttu-id="67adf-309">有关如何创建 COM 组件并使用它的示例，请参阅 [COM 演示](https://github.com/dotnet/samples/tree/main/core/extensions/COMServerDemo)。</span><span class="sxs-lookup"><span data-stu-id="67adf-309">For an example of how to create a COM component and consume it, see the [COM Demo](https://github.com/dotnet/samples/tree/main/core/extensions/COMServerDemo).</span></span>

### <a name="windows-native-interop"></a><span data-ttu-id="67adf-310">Windows 本机互操作</span><span class="sxs-lookup"><span data-stu-id="67adf-310">Windows Native Interop</span></span>

<span data-ttu-id="67adf-311">Windows 提供丰富的本机 API，包括平面 C API、COM 和 WinRT 的形式。</span><span class="sxs-lookup"><span data-stu-id="67adf-311">Windows offers a rich native API in the form of flat C APIs, COM, and WinRT.</span></span> <span data-ttu-id="67adf-312">.NET Core 支持 **P/Invoke**, .NET Core 3.0 则增加了 **CoCreate COM API** 和 **Activate WinRT API** 的功能。</span><span class="sxs-lookup"><span data-stu-id="67adf-312">While .NET Core supports **P/Invoke**, .NET Core 3.0 adds the ability to **CoCreate COM APIs** and **Activate WinRT APIs**.</span></span> <span data-ttu-id="67adf-313">有关代码示例，请参阅 [Excel 演示](https://github.com/dotnet/samples/tree/main/core/extensions/ExcelDemo)。</span><span class="sxs-lookup"><span data-stu-id="67adf-313">For a code example, see the [Excel Demo](https://github.com/dotnet/samples/tree/main/core/extensions/ExcelDemo).</span></span>

### <a name="msix-deployment"></a><span data-ttu-id="67adf-314">MSIX 部署</span><span class="sxs-lookup"><span data-stu-id="67adf-314">MSIX Deployment</span></span>

<span data-ttu-id="67adf-315">[MSIX](/windows/msix/) 是新的 Windows 应用程序包格式。</span><span class="sxs-lookup"><span data-stu-id="67adf-315">[MSIX](/windows/msix/) is a new Windows application package format.</span></span> <span data-ttu-id="67adf-316">可以使用它将 .NET Core 3.0 桌面应用程序部署到 Windows 10。</span><span class="sxs-lookup"><span data-stu-id="67adf-316">It can be used to deploy .NET Core 3.0 desktop applications to Windows 10.</span></span>

<span data-ttu-id="67adf-317">使用 Visual Studio 2019 中的 [Windows 应用打包项目](/windows/uwp/porting/desktop-to-uwp-packaging-dot-net)，可以创建包含[独立式](../deploying/index.md#publish-self-contained) .NET Core 应用的 MSIX 包。</span><span class="sxs-lookup"><span data-stu-id="67adf-317">The [Windows Application Packaging Project](/windows/uwp/porting/desktop-to-uwp-packaging-dot-net), available in Visual Studio 2019, allows you to create MSIX packages with [self-contained](../deploying/index.md#publish-self-contained) .NET Core applications.</span></span>

<span data-ttu-id="67adf-318">.NET Core 项目文件必须在 `<RuntimeIdentifiers>` 属性中指定支持的运行时：</span><span class="sxs-lookup"><span data-stu-id="67adf-318">The .NET Core project file must specify the supported runtimes in the `<RuntimeIdentifiers>` property:</span></span>

```xml
<RuntimeIdentifiers>win-x86;win-x64</RuntimeIdentifiers>
```

## <a name="linux-improvements"></a><span data-ttu-id="67adf-319">Linux 改进</span><span class="sxs-lookup"><span data-stu-id="67adf-319">Linux improvements</span></span>

### <a name="serialport-for-linux"></a><span data-ttu-id="67adf-320">适用于 Linux 的 SerialPort</span><span class="sxs-lookup"><span data-stu-id="67adf-320">SerialPort for Linux</span></span>

<span data-ttu-id="67adf-321">.NET Core 3.0 提供对 Linux 上 <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType> 的基本支持。</span><span class="sxs-lookup"><span data-stu-id="67adf-321">.NET Core 3.0 provides basic support for <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType> on Linux.</span></span>

<span data-ttu-id="67adf-322">以前，.NET Core 只支持在 Windows 上使用 `SerialPort`。</span><span class="sxs-lookup"><span data-stu-id="67adf-322">Previously, .NET Core only supported using `SerialPort` on Windows.</span></span>

<span data-ttu-id="67adf-323">有关对 Linux 上串行端口有限支持的详细信息，请参阅 [GitHub 问题 #33146](https://github.com/dotnet/corefx/issues/33146)。</span><span class="sxs-lookup"><span data-stu-id="67adf-323">For more information about the limited support for the serial port on Linux, see [GitHub issue #33146](https://github.com/dotnet/corefx/issues/33146).</span></span>

### <a name="docker-and-cgroup-memory-limits"></a><span data-ttu-id="67adf-324">Docker 和 cgroup 内存限制</span><span class="sxs-lookup"><span data-stu-id="67adf-324">Docker and cgroup memory Limits</span></span>

<span data-ttu-id="67adf-325">在 Linux 上使用 Docker 运行 .NET Core 3.0 时，可更好地应对 cgroup 内存限制。</span><span class="sxs-lookup"><span data-stu-id="67adf-325">Running .NET Core 3.0 on Linux with Docker works better with cgroup memory limits.</span></span> <span data-ttu-id="67adf-326">运行具有内存限制的 Docker 容器（例如使用 `docker run -m`）会更改 .NET Core 的行为方式。</span><span class="sxs-lookup"><span data-stu-id="67adf-326">Running a Docker container with memory limits, such as with `docker run -m`, changes how .NET Core behaves.</span></span>

- <span data-ttu-id="67adf-327">默认垃圾回收器 (GC) 堆大小：最大为 20 MB 或容器内存限制的 75%。</span><span class="sxs-lookup"><span data-stu-id="67adf-327">Default Garbage Collector (GC) heap size: maximum of 20 mb or 75% of the memory limit on the container.</span></span>
- <span data-ttu-id="67adf-328">可以将显式大小设置为绝对数或 cgroup 限制的百分比。</span><span class="sxs-lookup"><span data-stu-id="67adf-328">Explicit size can be set as an absolute number or percentage of cgroup limit.</span></span>
- <span data-ttu-id="67adf-329">每个 GC 堆的最小保留段大小为 16 MB。</span><span class="sxs-lookup"><span data-stu-id="67adf-329">Minimum reserved segment size per GC heap is 16 mb.</span></span> <span data-ttu-id="67adf-330">此大小可减少在计算机上创建的堆数量。</span><span class="sxs-lookup"><span data-stu-id="67adf-330">This size reduces the number of heaps that are created on machines.</span></span>

### <a name="gpio-support-for-raspberry-pi"></a><span data-ttu-id="67adf-331">对 Raspberry Pi 的 GPIO 支持</span><span class="sxs-lookup"><span data-stu-id="67adf-331">GPIO Support for Raspberry Pi</span></span>

<span data-ttu-id="67adf-332">已向 NuGet 发布了两个可用于 GPIO 编程的包：</span><span class="sxs-lookup"><span data-stu-id="67adf-332">Two packages have been released to NuGet that you can use for GPIO programming:</span></span>

- [<span data-ttu-id="67adf-333">System.Device.Gpio</span><span class="sxs-lookup"><span data-stu-id="67adf-333">System.Device.Gpio</span></span>](https://www.nuget.org/packages/System.Device.Gpio)
- [<span data-ttu-id="67adf-334">Iot.Device.Bindings</span><span class="sxs-lookup"><span data-stu-id="67adf-334">Iot.Device.Bindings</span></span>](https://www.nuget.org/packages/Iot.Device.Bindings)

<span data-ttu-id="67adf-335">GPIO 包包括用于 *GPIO*、*SPI*、*I2C* 和 *PWM* 设备的 API。</span><span class="sxs-lookup"><span data-stu-id="67adf-335">The GPIO packages include APIs for *GPIO*, *SPI*, *I2C*, and *PWM* devices.</span></span> <span data-ttu-id="67adf-336">IoT 绑定包包括设备绑定。</span><span class="sxs-lookup"><span data-stu-id="67adf-336">The IoT bindings package includes device bindings.</span></span> <span data-ttu-id="67adf-337">有关详细信息，请参阅[设备 GitHub 存储库](https://github.com/dotnet/iot/blob/main/src/devices/)。</span><span class="sxs-lookup"><span data-stu-id="67adf-337">For more information, see the [devices GitHub repo](https://github.com/dotnet/iot/blob/main/src/devices/).</span></span>

### <a name="arm64-linux-support"></a><span data-ttu-id="67adf-338">ARM64 Linux 支持</span><span class="sxs-lookup"><span data-stu-id="67adf-338">ARM64 Linux support</span></span>

<span data-ttu-id="67adf-339">.NET Core 3.0 增加了对 ARM64 for Linux 的支持。</span><span class="sxs-lookup"><span data-stu-id="67adf-339">.NET Core 3.0 adds support for ARM64 for Linux.</span></span> <span data-ttu-id="67adf-340">ARM64 的主要用例是当前的 IoT 场景。</span><span class="sxs-lookup"><span data-stu-id="67adf-340">The primary use case for ARM64 is currently with IoT scenarios.</span></span> <span data-ttu-id="67adf-341">有关详细信息，请参阅 [.NET Core ARM64 状态](https://github.com/dotnet/announcements/issues/82)。</span><span class="sxs-lookup"><span data-stu-id="67adf-341">For more information, see [.NET Core ARM64 Status](https://github.com/dotnet/announcements/issues/82).</span></span>

<span data-ttu-id="67adf-342">[ARM64 上适用于 .NET Core 的 Docker 映像](https://hub.docker.com/r/microsoft/dotnet/)可用于 Alpine、Debian 和 Ubuntu。</span><span class="sxs-lookup"><span data-stu-id="67adf-342">[Docker images for .NET Core on ARM64](https://hub.docker.com/r/microsoft/dotnet/) are available for Alpine, Debian, and Ubuntu.</span></span>

> [!NOTE]
> <span data-ttu-id="67adf-343">**ARM64** 尚未提供 Windows 支持。</span><span class="sxs-lookup"><span data-stu-id="67adf-343">**ARM64** Windows support isn't yet available.</span></span>

## <a name="security"></a><span data-ttu-id="67adf-344">安全性</span><span class="sxs-lookup"><span data-stu-id="67adf-344">Security</span></span>

### <a name="tls-13--openssl-111-on-linux"></a><span data-ttu-id="67adf-345">Linux 上的 TLS 1.3 和 OpenSSL 1.1.1</span><span class="sxs-lookup"><span data-stu-id="67adf-345">TLS 1.3 & OpenSSL 1.1.1 on Linux</span></span>

<span data-ttu-id="67adf-346">.NET Core 现在可以在给定环境中使用 [OpenSSL 1.1.1 中的 TLS 1.3 支持](https://www.openssl.org/blog/blog/2018/09/11/release111/)。</span><span class="sxs-lookup"><span data-stu-id="67adf-346">.NET Core now takes advantage of [TLS 1.3 support in OpenSSL 1.1.1](https://www.openssl.org/blog/blog/2018/09/11/release111/), when it's available in a given environment.</span></span> <span data-ttu-id="67adf-347">使用 TLS 1.3：</span><span class="sxs-lookup"><span data-stu-id="67adf-347">With TLS 1.3:</span></span>

- <span data-ttu-id="67adf-348">通过减少客户端和服务器之间所需的往返次数，提高了连接时间。</span><span class="sxs-lookup"><span data-stu-id="67adf-348">Connection times are improved with reduced round trips required between the client and server.</span></span>
- <span data-ttu-id="67adf-349">由于删除了各种过时和不安全的加密算法，提高了安全性。</span><span class="sxs-lookup"><span data-stu-id="67adf-349">Improved security because of the removal of various obsolete and insecure cryptographic algorithms.</span></span>

<span data-ttu-id="67adf-350">.NET Core 3.0 在 Linux 系统上使用 **OpenSSL 1.1.1**、**OpenSSL 1.1.0** 或 **OpenSSL 1.0.2**（如果可用）。</span><span class="sxs-lookup"><span data-stu-id="67adf-350">When available, .NET Core 3.0 uses **OpenSSL 1.1.1**, **OpenSSL 1.1.0**, or **OpenSSL 1.0.2** on a Linux system.</span></span> <span data-ttu-id="67adf-351">当 **OpenSSL 1.1.1** 可用时，<xref:System.Net.Security.SslStream?displayProperty=nameWithType> 和 <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> 类型都将使用 **TLS 1.3**（假定客户端和服务器都支持 **TLS 1.3**）。</span><span class="sxs-lookup"><span data-stu-id="67adf-351">When **OpenSSL 1.1.1** is available, both <xref:System.Net.Security.SslStream?displayProperty=nameWithType> and <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> types will use **TLS 1.3** (assuming both the client and server support **TLS 1.3**).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="67adf-352">Windows 和 macOS 尚不支持 TLS 1.3。</span><span class="sxs-lookup"><span data-stu-id="67adf-352">Windows and macOS do not yet support **TLS 1.3**.</span></span>

<span data-ttu-id="67adf-353">下面的 C# 8.0 示例演示在 Ubuntu 18.10 上 .NET Core 3.0 如何连接到 <https://www.cloudflare.com>：</span><span class="sxs-lookup"><span data-stu-id="67adf-353">The following C# 8.0 example demonstrates .NET Core 3.0 on Ubuntu 18.10 connecting to <https://www.cloudflare.com>:</span></span>

[!code-csharp[TLSExample](./snippets/dotnet-core-3-0/csharp/TLS.cs#TLS)]

### <a name="cryptography-ciphers"></a><span data-ttu-id="67adf-354">加密密码</span><span class="sxs-lookup"><span data-stu-id="67adf-354">Cryptography ciphers</span></span>

<span data-ttu-id="67adf-355">.NET Core 3.0 增加了对 AES-GCM 和 AES-CCM 密码的支持（分别使用 <xref:System.Security.Cryptography.AesGcm?displayProperty=nameWithType> 和 <xref:System.Security.Cryptography.AesCcm?displayProperty=nameWithType> 实现） 。</span><span class="sxs-lookup"><span data-stu-id="67adf-355">.NET Core 3.0 adds support for **AES-GCM** and **AES-CCM** ciphers, implemented with <xref:System.Security.Cryptography.AesGcm?displayProperty=nameWithType> and <xref:System.Security.Cryptography.AesCcm?displayProperty=nameWithType> respectively.</span></span> <span data-ttu-id="67adf-356">这些算法都是[用于关联数据的认证加密 (AEAD) 算法](https://en.wikipedia.org/wiki/Authenticated_encryption)。</span><span class="sxs-lookup"><span data-stu-id="67adf-356">These algorithms are both [Authenticated Encryption with Association Data (AEAD) algorithms](https://en.wikipedia.org/wiki/Authenticated_encryption).</span></span>

<span data-ttu-id="67adf-357">下面的代码演示了如何使用 `AesGcm` 密码加密和解密随机数据。</span><span class="sxs-lookup"><span data-stu-id="67adf-357">The following code demonstrates using `AesGcm` cipher to encrypt and decrypt random data.</span></span>

[!code-csharp[AesGcm](./snippets/dotnet-core-3-0/csharp/Cipher.cs#AesGcm)]

### <a name="cryptographic-key-importexport"></a><span data-ttu-id="67adf-358">加密密钥导入/导出</span><span class="sxs-lookup"><span data-stu-id="67adf-358">Cryptographic Key Import/Export</span></span>

<span data-ttu-id="67adf-359">.NET Core 3.0 支持从标准格式导入和导出非对称公钥和私钥。</span><span class="sxs-lookup"><span data-stu-id="67adf-359">.NET Core 3.0 supports the import and export of asymmetric public and private keys from standard formats.</span></span> <span data-ttu-id="67adf-360">你不需要使用 X.509 证书。</span><span class="sxs-lookup"><span data-stu-id="67adf-360">You don't need to use an X.509 certificate.</span></span>

<span data-ttu-id="67adf-361">所有密钥类型（例如 *RSA*、*DSA*、*ECDsa* 和 *ECDiffieHellman*）都支持以下格式：</span><span class="sxs-lookup"><span data-stu-id="67adf-361">All key types, such as *RSA*, *DSA*, *ECDsa*, and *ECDiffieHellman*, support the following formats:</span></span>

- <span data-ttu-id="67adf-362">**公钥**</span><span class="sxs-lookup"><span data-stu-id="67adf-362">**Public Key**</span></span>
  - <span data-ttu-id="67adf-363">X.509 SubjectPublicKeyInfo</span><span class="sxs-lookup"><span data-stu-id="67adf-363">X.509 SubjectPublicKeyInfo</span></span>

- <span data-ttu-id="67adf-364">**私钥**</span><span class="sxs-lookup"><span data-stu-id="67adf-364">**Private key**</span></span>
  - <span data-ttu-id="67adf-365">PKCS#8 PrivateKeyInfo</span><span class="sxs-lookup"><span data-stu-id="67adf-365">PKCS#8 PrivateKeyInfo</span></span>
  - <span data-ttu-id="67adf-366">PKCS#8 EncryptedPrivateKeyInfo</span><span class="sxs-lookup"><span data-stu-id="67adf-366">PKCS#8 EncryptedPrivateKeyInfo</span></span>

<span data-ttu-id="67adf-367">RSA 密钥还支持：</span><span class="sxs-lookup"><span data-stu-id="67adf-367">RSA keys also support:</span></span>

- <span data-ttu-id="67adf-368">**公钥**</span><span class="sxs-lookup"><span data-stu-id="67adf-368">**Public Key**</span></span>
  - <span data-ttu-id="67adf-369">PKCS#1 RSAPublicKey</span><span class="sxs-lookup"><span data-stu-id="67adf-369">PKCS#1 RSAPublicKey</span></span>

- <span data-ttu-id="67adf-370">**私钥**</span><span class="sxs-lookup"><span data-stu-id="67adf-370">**Private key**</span></span>
  - <span data-ttu-id="67adf-371">PKCS#1 RSAPrivateKey</span><span class="sxs-lookup"><span data-stu-id="67adf-371">PKCS#1 RSAPrivateKey</span></span>

<span data-ttu-id="67adf-372">导出方法生成 DER 编码的二进制数据，导入方法也是如此。</span><span class="sxs-lookup"><span data-stu-id="67adf-372">The export methods produce DER-encoded binary data, and the import methods expect the same.</span></span> <span data-ttu-id="67adf-373">如果密钥以文本友好的 PEM 格式存储，调用方需要在调用导入方法之前对内容进行 base64 解码。</span><span class="sxs-lookup"><span data-stu-id="67adf-373">If a key is stored in the text-friendly PEM format, the caller will need to base64-decode the content before calling an import method.</span></span>

[!code-csharp[RSA](./snippets/dotnet-core-3-0/csharp/RSA.cs#Rsa)]

<span data-ttu-id="67adf-374">可以使用 <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo?displayProperty=nameWithType> 检查 **PKCS#8** 文件，使用 <xref:System.Security.Cryptography.Pkcs.Pkcs12Info?displayProperty=nameWithType> 检查 **PFX/PKCS#12** 文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-374">**PKCS#8** files can be inspected with <xref:System.Security.Cryptography.Pkcs.Pkcs8PrivateKeyInfo?displayProperty=nameWithType> and **PFX/PKCS#12** files can be inspected with <xref:System.Security.Cryptography.Pkcs.Pkcs12Info?displayProperty=nameWithType>.</span></span> <span data-ttu-id="67adf-375">可以使用 <xref:System.Security.Cryptography.Pkcs.Pkcs12Builder?displayProperty=nameWithType> 操作 **PFX/PKCS#12** 文件。</span><span class="sxs-lookup"><span data-stu-id="67adf-375">**PFX/PKCS#12** files can be manipulated with <xref:System.Security.Cryptography.Pkcs.Pkcs12Builder?displayProperty=nameWithType>.</span></span>

## <a name="net-core-30-api-changes"></a><span data-ttu-id="67adf-376">.NET Core 3.0 API 改动</span><span class="sxs-lookup"><span data-stu-id="67adf-376">.NET Core 3.0 API changes</span></span>

### <a name="ranges-and-indices"></a><span data-ttu-id="67adf-377">范围和索引</span><span class="sxs-lookup"><span data-stu-id="67adf-377">Ranges and indices</span></span>

<span data-ttu-id="67adf-378">新 <xref:System.Index?displayProperty=nameWithType> 类型可用于编制索引。</span><span class="sxs-lookup"><span data-stu-id="67adf-378">The new <xref:System.Index?displayProperty=nameWithType> type can be used for indexing.</span></span> <span data-ttu-id="67adf-379">可从 `int` 创建一个从开头开始计数的索引，也可使用前缀 `^` 运算符 (C#) 创建一个从末尾开始计数的索引：</span><span class="sxs-lookup"><span data-stu-id="67adf-379">You can create one from an `int` that counts from the beginning, or with a prefix `^` operator (C#) that counts from the end:</span></span>

```csharp
Index i1 = 3;  // number 3 from beginning
Index i2 = ^4; // number 4 from end
int[] a = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
Console.WriteLine($"{a[i1]}, {a[i2]}"); // "3, 6"
```

<span data-ttu-id="67adf-380">此外，还有 <xref:System.Range?displayProperty=nameWithType> 类型，它包含两个 `Index` 值，一个用于开头一个用于结尾，可以使用 `x..y` 范围表达式 (C#) 进行编写。</span><span class="sxs-lookup"><span data-stu-id="67adf-380">There's also the <xref:System.Range?displayProperty=nameWithType> type, which consists of two `Index` values, one for the start and one for the end, and can be written with a `x..y` range expression (C#).</span></span> <span data-ttu-id="67adf-381">然后可以使用 `Range` 编制索引，以便生成一个切片：</span><span class="sxs-lookup"><span data-stu-id="67adf-381">You can then index with a `Range`, which produces a slice:</span></span>

```csharp
var slice = a[i1..i2]; // { 3, 4, 5 }
```

<span data-ttu-id="67adf-382">有关详细信息，请参阅[范围和索引教程](../../csharp/whats-new/tutorials/ranges-indexes.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-382">For more information, see the [ranges and indices tutorial](../../csharp/whats-new/tutorials/ranges-indexes.md).</span></span>

### <a name="async-streams"></a><span data-ttu-id="67adf-383">异步流</span><span class="sxs-lookup"><span data-stu-id="67adf-383">Async streams</span></span>

<span data-ttu-id="67adf-384"><xref:System.Collections.Generic.IAsyncEnumerable%601> 类型是 <xref:System.Collections.Generic.IEnumerable%601> 的新异步版本。</span><span class="sxs-lookup"><span data-stu-id="67adf-384">The <xref:System.Collections.Generic.IAsyncEnumerable%601> type is a new asynchronous version of <xref:System.Collections.Generic.IEnumerable%601>.</span></span> <span data-ttu-id="67adf-385">通过该语言，可通过 `IAsyncEnumerable<T>` 执行 `await foreach` 操作来使用其元素，并对其使用 `yield return` 操作来生成元素。</span><span class="sxs-lookup"><span data-stu-id="67adf-385">The language lets you `await foreach` over `IAsyncEnumerable<T>` to consume their elements, and use `yield return` to them to produce elements.</span></span>

<span data-ttu-id="67adf-386">下面的示例演示如何生成和使用异步流。</span><span class="sxs-lookup"><span data-stu-id="67adf-386">The following example demonstrates both production and consumption of async streams.</span></span> <span data-ttu-id="67adf-387">`foreach` 语句为异步语句，它本身使用 `yield return` 为调用方生成异步流。</span><span class="sxs-lookup"><span data-stu-id="67adf-387">The `foreach` statement is async and itself uses `yield return` to produce an async stream for callers.</span></span> <span data-ttu-id="67adf-388">此模式（使用 `yield return`）是生成异步流的建议模型。</span><span class="sxs-lookup"><span data-stu-id="67adf-388">This pattern (using `yield return`) is the recommended model for producing async streams.</span></span>

```csharp
async IAsyncEnumerable<int> GetBigResultsAsync()
{
    await foreach (var result in GetResultsAsync())
    {
        if (result > 20) yield return result;
    }
}
```

<span data-ttu-id="67adf-389">除了能够 `await foreach`，还可以创建异步迭代器，例如，一个返回 `IAsyncEnumerable/IAsyncEnumerator` 的迭代器，可以在其中进行 `await` 和 `yield` 操作。</span><span class="sxs-lookup"><span data-stu-id="67adf-389">In addition to being able to `await foreach`, you can also create async iterators, for example, an iterator that returns an `IAsyncEnumerable/IAsyncEnumerator` that you can both `await` and `yield` in.</span></span> <span data-ttu-id="67adf-390">对于需要处理的对象，可以使用各种 BCL 类型（如 `Stream` 和 `Timer`）实现的 `IAsyncDisposable`。</span><span class="sxs-lookup"><span data-stu-id="67adf-390">For objects that need to be disposed, you can use `IAsyncDisposable`, which various BCL types implement, such as `Stream` and `Timer`.</span></span>

<span data-ttu-id="67adf-391">有关详细信息，请参阅[异步流教程](../../csharp/whats-new/tutorials/generate-consume-asynchronous-stream.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-391">For more information, see the [async streams tutorial](../../csharp/whats-new/tutorials/generate-consume-asynchronous-stream.md).</span></span>

### <a name="ieee-floating-point"></a><span data-ttu-id="67adf-392">IEEE 浮点</span><span class="sxs-lookup"><span data-stu-id="67adf-392">IEEE Floating-point</span></span>

<span data-ttu-id="67adf-393">正在更新浮点 API，以符合 [IEEE 754-2008 修订](https://en.wikipedia.org/wiki/IEEE_754-2008_revision)。</span><span class="sxs-lookup"><span data-stu-id="67adf-393">Floating point APIs are being updated to comply with [IEEE 754-2008 revision](https://en.wikipedia.org/wiki/IEEE_754-2008_revision).</span></span> <span data-ttu-id="67adf-394">这些更改旨在公开所有 **必需** 操作并确保这些操作在行为上符合 IEEE 规范。有关浮点改进的详细信息，请参阅 [.NET Core 3.0 中的浮点分析和格式化改进](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/)博客文章。</span><span class="sxs-lookup"><span data-stu-id="67adf-394">The goal of these changes is to expose all **required** operations and ensure that they're behaviorally compliant with the IEEE spec. For more information about floating-point improvements, see the [Floating-Point Parsing and Formatting improvements in .NET Core 3.0](https://devblogs.microsoft.com/dotnet/floating-point-parsing-and-formatting-improvements-in-net-core-3-0/) blog post.</span></span>

<span data-ttu-id="67adf-395">分析和格式化修复包括：</span><span class="sxs-lookup"><span data-stu-id="67adf-395">Parsing and formatting fixes include:</span></span>

- <span data-ttu-id="67adf-396">正确分析并舍入任何输入长度。</span><span class="sxs-lookup"><span data-stu-id="67adf-396">Correctly parse and round inputs of any length.</span></span>
- <span data-ttu-id="67adf-397">正确分析并格式化负零。</span><span class="sxs-lookup"><span data-stu-id="67adf-397">Correctly parse and format negative zero.</span></span>
- <span data-ttu-id="67adf-398">通过执行不区分大小写的检查并允许在前面使用可选的 `+`（如果适用），正确分析 `Infinity` 和 `NaN`。</span><span class="sxs-lookup"><span data-stu-id="67adf-398">Correctly parse `Infinity` and `NaN` by doing a case-insensitive check and allowing an optional preceding `+` where applicable.</span></span>

<span data-ttu-id="67adf-399">新的 <xref:System.Math?displayProperty=nameWithType> API 包括：</span><span class="sxs-lookup"><span data-stu-id="67adf-399">New <xref:System.Math?displayProperty=nameWithType> APIs include:</span></span>

- <span data-ttu-id="67adf-400"><xref:System.Math.BitIncrement(System.Double)> 和 <xref:System.Math.BitDecrement(System.Double)></span><span class="sxs-lookup"><span data-stu-id="67adf-400"><xref:System.Math.BitIncrement(System.Double)> and <xref:System.Math.BitDecrement(System.Double)></span></span>\
<span data-ttu-id="67adf-401">相当于 `nextUp` 和 `nextDown` IEEE 运算。</span><span class="sxs-lookup"><span data-stu-id="67adf-401">Corresponds to the `nextUp` and `nextDown` IEEE operations.</span></span> <span data-ttu-id="67adf-402">它们将返回最小的浮点数，该数字大于或小于输入值（分别）。</span><span class="sxs-lookup"><span data-stu-id="67adf-402">They return the smallest floating-point number that compares greater or lesser than the input (respectively).</span></span> <span data-ttu-id="67adf-403">例如，`Math.BitIncrement(0.0)` 将返回 `double.Epsilon`。</span><span class="sxs-lookup"><span data-stu-id="67adf-403">For example, `Math.BitIncrement(0.0)` would return `double.Epsilon`.</span></span>

- <span data-ttu-id="67adf-404"><xref:System.Math.MaxMagnitude(System.Double,System.Double)> 和 <xref:System.Math.MinMagnitude(System.Double,System.Double)></span><span class="sxs-lookup"><span data-stu-id="67adf-404"><xref:System.Math.MaxMagnitude(System.Double,System.Double)> and <xref:System.Math.MinMagnitude(System.Double,System.Double)></span></span>\
<span data-ttu-id="67adf-405">相当于 `maxNumMag` 和 `minNumMag` IEEE 运算，它们将（分别）返回大于或小于两个输入的量值的值。</span><span class="sxs-lookup"><span data-stu-id="67adf-405">Corresponds to the `maxNumMag` and `minNumMag` IEEE operations, they return the value that is greater or lesser in magnitude of the two inputs (respectively).</span></span> <span data-ttu-id="67adf-406">例如，`Math.MaxMagnitude(2.0, -3.0)` 将返回 `-3.0`。</span><span class="sxs-lookup"><span data-stu-id="67adf-406">For example, `Math.MaxMagnitude(2.0, -3.0)` would return `-3.0`.</span></span>

- <xref:System.Math.ILogB(System.Double)>\
<span data-ttu-id="67adf-407">相当于返回整数值的 `logB` IEEE 运算，它将返回输入参数的整数对数（以 2 为底）。</span><span class="sxs-lookup"><span data-stu-id="67adf-407">Corresponds to the `logB` IEEE operation that returns an integral value, it returns the integral base-2 log of the input parameter.</span></span> <span data-ttu-id="67adf-408">此方法实际上与 `floor(log2(x))` 相同，但完成后出现最小舍入错误。</span><span class="sxs-lookup"><span data-stu-id="67adf-408">This method is effectively the same as `floor(log2(x))`, but done with minimal rounding error.</span></span>

- <xref:System.Math.ScaleB(System.Double,System.Int32)>\
<span data-ttu-id="67adf-409">相当于采用整数值的 `scaleB` IEEE 运算，它实际返回 `x * pow(2, n)`，但完成后出现最小舍入错误。</span><span class="sxs-lookup"><span data-stu-id="67adf-409">Corresponds to the `scaleB` IEEE operation that takes an integral value, it returns effectively `x * pow(2, n)`, but is done with minimal rounding error.</span></span>

- <xref:System.Math.Log2(System.Double)>\
<span data-ttu-id="67adf-410">相当于返回（以 2 为底）对数的 `log2` IEEE 运算。</span><span class="sxs-lookup"><span data-stu-id="67adf-410">Corresponds to the `log2` IEEE operation, it returns the base-2 logarithm.</span></span> <span data-ttu-id="67adf-411">它会最小化舍入错误。</span><span class="sxs-lookup"><span data-stu-id="67adf-411">It minimizes rounding error.</span></span>

- <xref:System.Math.FusedMultiplyAdd(System.Double,System.Double,System.Double)>\
<span data-ttu-id="67adf-412">相当于执行乘法加法混合的 `fma` IEEE 运算。</span><span class="sxs-lookup"><span data-stu-id="67adf-412">Corresponds to the `fma` IEEE operation, it performs a fused multiply add.</span></span> <span data-ttu-id="67adf-413">也就是说，它以单个运算的形式执行 `(x * y) + z`，从而最小化舍入错误。</span><span class="sxs-lookup"><span data-stu-id="67adf-413">That is, it does `(x * y) + z` as a single operation, thereby minimizing the rounding error.</span></span> <span data-ttu-id="67adf-414">例如，`FusedMultiplyAdd(1e308, 2.0, -1e308)` 返回 `1e308`。</span><span class="sxs-lookup"><span data-stu-id="67adf-414">An example is `FusedMultiplyAdd(1e308, 2.0, -1e308)`, which returns `1e308`.</span></span> <span data-ttu-id="67adf-415">常规 `(1e308 * 2.0) - 1e308` 返回 `double.PositiveInfinity`。</span><span class="sxs-lookup"><span data-stu-id="67adf-415">The regular `(1e308 * 2.0) - 1e308` returns `double.PositiveInfinity`.</span></span>

- <xref:System.Math.CopySign(System.Double,System.Double)>\
<span data-ttu-id="67adf-416">相当于 `copySign` IEEE 运算，它返回 `x` 的值但带有符号 `y`。</span><span class="sxs-lookup"><span data-stu-id="67adf-416">Corresponds to the `copySign` IEEE operation, it returns the value of `x`, but with the sign of `y`.</span></span>

### <a name="net-platform-dependent-intrinsics"></a><span data-ttu-id="67adf-417">.NET 平台相关内部函数</span><span class="sxs-lookup"><span data-stu-id="67adf-417">.NET Platform-Dependent Intrinsics</span></span>

<span data-ttu-id="67adf-418">已添加 API，允许访问某些性能导向的 CPU 指令，例如 SIMD 或位操作指令集 。</span><span class="sxs-lookup"><span data-stu-id="67adf-418">APIs have been added that allow access to certain perf-oriented CPU instructions, such as the **SIMD** or **Bit Manipulation instruction** sets.</span></span> <span data-ttu-id="67adf-419">这些指令有助于在某些情况下实现显著的性能改进，例如高效地并行处理数据。</span><span class="sxs-lookup"><span data-stu-id="67adf-419">These instructions can help achieve significant performance improvements in certain scenarios, such as processing data efficiently in parallel.</span></span>

<span data-ttu-id="67adf-420">在适当的情况下，.NET 库已开始使用这些指令来改进性能。</span><span class="sxs-lookup"><span data-stu-id="67adf-420">Where appropriate, the .NET libraries have begun using these instructions to improve performance.</span></span>

<span data-ttu-id="67adf-421">有关详细信息，请参阅 [.NET Platform Dependent Intrinsics](https://github.com/dotnet/designs/blob/main/accepted/2018/platform-intrinsics.md)（.NET 平台相关内部函数）。</span><span class="sxs-lookup"><span data-stu-id="67adf-421">For more information, see [.NET Platform-Dependent Intrinsics](https://github.com/dotnet/designs/blob/main/accepted/2018/platform-intrinsics.md).</span></span>

### <a name="improved-net-core-version-apis"></a><span data-ttu-id="67adf-422">改进的 .NET Core 版本 API</span><span class="sxs-lookup"><span data-stu-id="67adf-422">Improved .NET Core Version APIs</span></span>

<span data-ttu-id="67adf-423">从 .NET Core 3.0 开始，.NET Core 提供的版本 API 现在可以返回你预期的信息。</span><span class="sxs-lookup"><span data-stu-id="67adf-423">Starting with .NET Core 3.0, the version APIs provided with .NET Core now return the information you expect.</span></span> <span data-ttu-id="67adf-424">例如：</span><span class="sxs-lookup"><span data-stu-id="67adf-424">For example:</span></span>

```csharp
System.Console.WriteLine($"Environment.Version: {System.Environment.Version}");

// Old result
//   Environment.Version: 4.0.30319.42000
//
// New result
//   Environment.Version: 3.0.0
```

```csharp
System.Console.WriteLine($"RuntimeInformation.FrameworkDescription: {System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription}");

// Old result
//   RuntimeInformation.FrameworkDescription: .NET Core 4.6.27415.71
//
// New result (notice the value includes any preview release information)
//   RuntimeInformation.FrameworkDescription: .NET Core 3.0.0-preview4-27615-11
```

> [!WARNING]
> <span data-ttu-id="67adf-425">重大变更。</span><span class="sxs-lookup"><span data-stu-id="67adf-425">Breaking change.</span></span> <span data-ttu-id="67adf-426">这在技术上是一个中断性变更，因为版本控制方案已发生变化。</span><span class="sxs-lookup"><span data-stu-id="67adf-426">This is technically a breaking change because the versioning scheme has changed.</span></span>

### <a name="fast-built-in-json-support"></a><span data-ttu-id="67adf-427">内置的快速 JSON 支持</span><span class="sxs-lookup"><span data-stu-id="67adf-427">Fast built-in JSON support</span></span>

<span data-ttu-id="67adf-428">.NET 用户在很大程度上依赖于 [Newtonsoft.Json](https://www.newtonsoft.com/json) 和其他常用的 JSON 库，它们仍是很好的选择。</span><span class="sxs-lookup"><span data-stu-id="67adf-428">.NET users have largely relied on [Newtonsoft.Json](https://www.newtonsoft.com/json) and other popular JSON libraries, which continue to be good choices.</span></span> <span data-ttu-id="67adf-429">`Newtonsoft.Json` 使用 .NET 字符串作为其基本数据类型，它实际上是 UTF-16。</span><span class="sxs-lookup"><span data-stu-id="67adf-429">`Newtonsoft.Json` uses .NET strings as its base datatype, which is UTF-16 under the hood.</span></span>

<span data-ttu-id="67adf-430">新的内置 JSON 支持具有高性能、低分配的特点，并且可用于 UTF-8 编码的 JSON 文本。</span><span class="sxs-lookup"><span data-stu-id="67adf-430">The new built-in JSON support is high-performance, low allocation, and works with UTF-8 encoded JSON text.</span></span> <span data-ttu-id="67adf-431">有关 <xref:System.Text.Json> 命名空间和类型的详细信息，请参阅以下文章：</span><span class="sxs-lookup"><span data-stu-id="67adf-431">For more information about the <xref:System.Text.Json> namespace and types, see the following articles:</span></span>

* [<span data-ttu-id="67adf-432">.NET 中的 JSON 序列化 - 概述</span><span class="sxs-lookup"><span data-stu-id="67adf-432">JSON serialization in .NET - overview</span></span>](../../standard/serialization/system-text-json-overview.md)
* <span data-ttu-id="67adf-433">[如何在 .NET 中对 JSON 数据进行序列化和反序列化](../../standard/serialization/system-text-json-how-to.md)。</span><span class="sxs-lookup"><span data-stu-id="67adf-433">[How to serialize and deserialize JSON in .NET](../../standard/serialization/system-text-json-how-to.md).</span></span>
* [<span data-ttu-id="67adf-434">如何从 Newtonsoft.Json 迁移到 System.Text.Json</span><span class="sxs-lookup"><span data-stu-id="67adf-434">How to migrate from Newtonsoft.Json to System.Text.Json</span></span>](../../standard/serialization/system-text-json-migrate-from-newtonsoft-how-to.md)

### <a name="http2-support"></a><span data-ttu-id="67adf-435">HTTP/2 支持</span><span class="sxs-lookup"><span data-stu-id="67adf-435">HTTP/2 support</span></span>

<span data-ttu-id="67adf-436"><xref:System.Net.Http.HttpClient?displayProperty=nameWithType> 类型支持 HTTP/2 协议。</span><span class="sxs-lookup"><span data-stu-id="67adf-436">The <xref:System.Net.Http.HttpClient?displayProperty=nameWithType> type supports the HTTP/2 protocol.</span></span> <span data-ttu-id="67adf-437">如果启用 HTTP/2，则将通过 TLS/ALPN 协商 HTTP 协议版本，并在服务器选择使用 HTTP/2 时使用。</span><span class="sxs-lookup"><span data-stu-id="67adf-437">If HTTP/2 is enabled, the HTTP protocol version is negotiated via TLS/ALPN, and HTTP/2 is used if the server elects to use it.</span></span>

<span data-ttu-id="67adf-438">默认协议将保留 HTTP/1.1，但可以在两种不同方法中启用 HTTP/2。</span><span class="sxs-lookup"><span data-stu-id="67adf-438">The default protocol remains HTTP/1.1, but HTTP/2 can be enabled in two different ways.</span></span> <span data-ttu-id="67adf-439">首先，可以将 HTTP 请求消息设置为使用 HTTP/2：</span><span class="sxs-lookup"><span data-stu-id="67adf-439">First, you can set the HTTP request message to use HTTP/2:</span></span>

[!code-csharp[Http2Request](./snippets/dotnet-core-3-0/csharp/http.cs#Request)]

<span data-ttu-id="67adf-440">其次，可以更改 <xref:System.Net.Http.HttpClient> 以默认使用 HTTP/2：</span><span class="sxs-lookup"><span data-stu-id="67adf-440">Second, you can change <xref:System.Net.Http.HttpClient> to use HTTP/2 by default:</span></span>

[!code-csharp[Http2Client](./snippets/dotnet-core-3-0/csharp/http.cs#Client)]

<span data-ttu-id="67adf-441">很多时候，在你开发应用程序时要使用未加密的连接。</span><span class="sxs-lookup"><span data-stu-id="67adf-441">Many times when you're developing an application, you want to use an unencrypted connection.</span></span> <span data-ttu-id="67adf-442">如果你知道目标终结点将使用 HTTP/2，你可以为 HTTP/2 打开未加密的连接。</span><span class="sxs-lookup"><span data-stu-id="67adf-442">If you know the target endpoint will be using HTTP/2, you can turn on unencrypted connections for HTTP/2.</span></span> <span data-ttu-id="67adf-443">可以通过将 `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT` 环境变量设置为 `1` 或通过在应用上下文中启用它来将其打开：</span><span class="sxs-lookup"><span data-stu-id="67adf-443">You can turn it on by setting the `DOTNET_SYSTEM_NET_HTTP_SOCKETSHTTPHANDLER_HTTP2UNENCRYPTEDSUPPORT` environment variable to `1` or by enabling it in the app context:</span></span>

[!code-csharp[Http2Context](./snippets/dotnet-core-3-0/csharp/http.cs#AppContext)]

## <a name="next-steps"></a><span data-ttu-id="67adf-444">后续步骤</span><span class="sxs-lookup"><span data-stu-id="67adf-444">Next steps</span></span>

- [<span data-ttu-id="67adf-445">查看 .NET Core 2.2 和 3.0 之间的重大变更。</span><span class="sxs-lookup"><span data-stu-id="67adf-445">Review the breaking changes between .NET Core 2.2 and 3.0.</span></span>](../compatibility/3.0.md)
- [<span data-ttu-id="67adf-446">查看用于 Windows 窗体应用的 .NET Core 3.0 中的中断性变更。</span><span class="sxs-lookup"><span data-stu-id="67adf-446">Review the breaking changes in .NET Core 3.0 for Windows Forms apps.</span></span>](../compatibility/winforms.md#net-core-30)
