---
title: 分析依赖项以移植代码
description: 了解如何分析外部依赖项，以便将项目从 .NET Framework 移植到 .NET。
author: cartermp
ms.date: 03/04/2021
ms.openlocfilehash: 4619243cf300e248be45e4b2a4d5541c3b3e1cb5
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604913"
---
# <a name="analyze-your-dependencies-to-port-code-from-net-framework-to-net"></a><span data-ttu-id="317b6-103">分析依赖项以将代码从 .NET Framework 移植到 .NET 中</span><span class="sxs-lookup"><span data-stu-id="317b6-103">Analyze your dependencies to port code from .NET Framework to .NET</span></span>

<span data-ttu-id="317b6-104">若要将代码移植到 .NET 或.NET Standard，必须了解依赖项。</span><span class="sxs-lookup"><span data-stu-id="317b6-104">To port your code to .NET or .NET Standard, you must understand your dependencies.</span></span> <span data-ttu-id="317b6-105">外部依赖项是在项目中引用但没有自行构建的 NuGet 包或 `.dll` 文件。</span><span class="sxs-lookup"><span data-stu-id="317b6-105">External dependencies are the NuGet packages or `.dll` files you reference in your project, but that you don't build yourself.</span></span>

<span data-ttu-id="317b6-106">通过将代码移植到 .NET Standard 2.0 或更低版本，可确保该代码可用于 .NET Framework 和 .NET。</span><span class="sxs-lookup"><span data-stu-id="317b6-106">Porting your code to .NET Standard 2.0 or below ensures that it can be used with both .NET Framework and .NET.</span></span> <span data-ttu-id="317b6-107">但是，如果你不需要将库用于 .NET Framework，请考虑以最新版本的 .NET 为目标。</span><span class="sxs-lookup"><span data-stu-id="317b6-107">However, if you don't need to use the library with .NET Framework, consider targeting the latest version of .NET.</span></span>

## <a name="migrate-your-nuget-packages-to-packagereference"></a><span data-ttu-id="317b6-108">将 NuGet 包迁移到 `PackageReference`</span><span class="sxs-lookup"><span data-stu-id="317b6-108">Migrate your NuGet packages to `PackageReference`</span></span>

<span data-ttu-id="317b6-109">.NET 不能使用 [packages.config](/nuget/reference/packages-config) 文件引用 NuGet。</span><span class="sxs-lookup"><span data-stu-id="317b6-109">.NET can't use the [_packages.config_](/nuget/reference/packages-config) file for NuGet references.</span></span> <span data-ttu-id="317b6-110">.NET 和 .NET Framework 都可以使用 [PackageReference](/nuget/consume-packages/package-references-in-project-files) 来指定包依赖项。</span><span class="sxs-lookup"><span data-stu-id="317b6-110">Both .NET and .NET Framework can use [PackageReference](/nuget/consume-packages/package-references-in-project-files) to specify package dependencies.</span></span> <span data-ttu-id="317b6-111">如果你使用 packages.config 在项目中指定包，请将其转换为 `PackageReference` 格式。</span><span class="sxs-lookup"><span data-stu-id="317b6-111">If you're using _packages.config_ to specify your packages in your project, convert it to the `PackageReference` format.</span></span>

<span data-ttu-id="317b6-112">如需了解如何迁移，请参阅[从 packages.config 迁移到 PackageReference](/nuget/reference/migrate-packages-config-to-package-reference)一文。</span><span class="sxs-lookup"><span data-stu-id="317b6-112">To learn how to migrate, see the [Migrate from packages.config to PackageReference](/nuget/reference/migrate-packages-config-to-package-reference) article.</span></span>

## <a name="upgrade-your-nuget-packages"></a><span data-ttu-id="317b6-113">升级 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="317b6-113">Upgrade your NuGet packages</span></span>

<span data-ttu-id="317b6-114">将项目迁移为 `PackageReference` 格式后，验证包是否与 .NET 兼容。</span><span class="sxs-lookup"><span data-stu-id="317b6-114">After you migrate your project to the `PackageReference` format, verify if your packages are compatible with .NET.</span></span>

<span data-ttu-id="317b6-115">首先，请将包升级到可以使用的最新版本。</span><span class="sxs-lookup"><span data-stu-id="317b6-115">First, upgrade your packages to the latest version that you can.</span></span> <span data-ttu-id="317b6-116">可通过 Visual Studio 中的 NuGet 包管理器 UI 完成此操作。</span><span class="sxs-lookup"><span data-stu-id="317b6-116">This can be done with the NuGet Package Manager UI in Visual Studio.</span></span> <span data-ttu-id="317b6-117">包依赖项的较新版本可能已经与 .NET Core 兼容。</span><span class="sxs-lookup"><span data-stu-id="317b6-117">It's likely that newer versions of your package dependencies are already compatible with .NET Core.</span></span>

## <a name="analyze-your-package-dependencies"></a><span data-ttu-id="317b6-118">分析包依赖项</span><span class="sxs-lookup"><span data-stu-id="317b6-118">Analyze your package dependencies</span></span>

<span data-ttu-id="317b6-119">如果尚未验证转换和升级后的包依赖项是否适用于 .NET Core，可通过以下几种方法实现此目的：</span><span class="sxs-lookup"><span data-stu-id="317b6-119">If you haven't already verified that your converted and upgraded package dependencies work on .NET Core, there are a few ways that you can achieve that:</span></span>

### <a name="analyze-nuget-packages-using-nugetorg"></a><span data-ttu-id="317b6-120">使用 nuget.org 分析 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="317b6-120">Analyze NuGet packages using nuget.org</span></span>

<span data-ttu-id="317b6-121">可以在包页面“依赖项”部分的 [nuget.org](https://www.nuget.org/) 上查看每个包所支持的目标框架名字对象 (TFM)  。</span><span class="sxs-lookup"><span data-stu-id="317b6-121">You can see the Target Framework Monikers (TFMs) that each package supports on [nuget.org](https://www.nuget.org/) under the **Dependencies** section of the package page.</span></span>

<span data-ttu-id="317b6-122">尽管使用站点验证兼容性是一种比较简单的方法，但站点上并未提供所有包的“依赖项”信息  。</span><span class="sxs-lookup"><span data-stu-id="317b6-122">Although using the site is an easier method to verify the compatibility, **Dependencies** information isn't available on the site for all packages.</span></span>

### <a name="analyze-nuget-packages-using-nuget-package-explorer"></a><span data-ttu-id="317b6-123">使用 NuGet 包资源管理器分析 NuGet 包</span><span class="sxs-lookup"><span data-stu-id="317b6-123">Analyze NuGet packages using NuGet Package Explorer</span></span>

<span data-ttu-id="317b6-124">NuGet 包本身就是一组包含特定于平台的程序集的文件夹。</span><span class="sxs-lookup"><span data-stu-id="317b6-124">A NuGet package is itself a set of folders that contain platform-specific assemblies.</span></span> <span data-ttu-id="317b6-125">检查包中是否有包含兼容程序集的文件夹。</span><span class="sxs-lookup"><span data-stu-id="317b6-125">Check if there's a folder that contains a compatible assembly inside the package.</span></span>

<span data-ttu-id="317b6-126">检查 NuGet 包文件夹最简单方法是使用 [NuGet 包资源管理器](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)工具。</span><span class="sxs-lookup"><span data-stu-id="317b6-126">The easiest way to inspect NuGet package folders is to use the [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) tool.</span></span> <span data-ttu-id="317b6-127">安装完成后，使用以下步骤查看文件夹名称：</span><span class="sxs-lookup"><span data-stu-id="317b6-127">After installing it, use the following steps to see the folder names:</span></span>

1. <span data-ttu-id="317b6-128">打开 NuGet 包资源管理器。</span><span class="sxs-lookup"><span data-stu-id="317b6-128">Open the NuGet Package Explorer.</span></span>
2. <span data-ttu-id="317b6-129">单击“从在线源打开包”  。</span><span class="sxs-lookup"><span data-stu-id="317b6-129">Click **Open package from online feed**.</span></span>
3. <span data-ttu-id="317b6-130">搜索包的名称。</span><span class="sxs-lookup"><span data-stu-id="317b6-130">Search for the name of the package.</span></span>
4. <span data-ttu-id="317b6-131">从搜索结果中选择包的名称，然后点击“打开”  。</span><span class="sxs-lookup"><span data-stu-id="317b6-131">Select the package name from the search results and click **open**.</span></span>
5. <span data-ttu-id="317b6-132">展开右侧的“lib”文件夹并查看文件夹名称  。</span><span class="sxs-lookup"><span data-stu-id="317b6-132">Expand the *lib* folder on the right-hand side and look at folder names.</span></span>

<span data-ttu-id="317b6-133">查找名称使用以下模式之一的文件夹：`netstandardX.Y`、`netX.Y` 或 `netcoreappX.Y`。</span><span class="sxs-lookup"><span data-stu-id="317b6-133">Look for a folder with names using one the following patterns: `netstandardX.Y`, `netX.Y`, or `netcoreappX.Y`.</span></span>

<span data-ttu-id="317b6-134">这些值是映射到 [.NET Standard](../../standard/net-standard.md)、.NET 和 .NET Core 各版本的[目标框架名字对象 (TFM)](../../standard/frameworks.md)，它们均与 .NET 兼容。</span><span class="sxs-lookup"><span data-stu-id="317b6-134">These values are the [Target Framework Monikers (TFMs)](../../standard/frameworks.md) that map to versions of [.NET Standard](../../standard/net-standard.md), .NET, and .NET Core, which are all compatible with .NET.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="317b6-135">在查看包支持的 TFM 时，请注意，除 `netstandard*` 以外的 TFM 面向的是 .NET 的特定实现，如 .NET 5、.NET Core 或 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="317b6-135">When looking at the TFMs that a package supports, note that a TFM other than `netstandard*` targets a specific implementation of .NET, such as .NET 5, .NET Core, or .NET Framework.</span></span> <span data-ttu-id="317b6-136">从 .NET 5 开始，`net*` TFM（未指定操作系统）实际上取代了 `netstandard*` 成为[可移植目标](../../standard/net-standard.md#net-5-and-net-standard)。</span><span class="sxs-lookup"><span data-stu-id="317b6-136">Starting with .NET 5, the `net*` TFM (without an operating system designation) effectively replaces `netstandard*` as a [portable target](../../standard/net-standard.md#net-5-and-net-standard).</span></span> <span data-ttu-id="317b6-137">例如，`net5.0` 面向 .NET 5 API 图面，并且是跨平台友好的，而 `net5.0-windows` 面向在 Windows 操作系统上实现的 .NET 5 API 图面。</span><span class="sxs-lookup"><span data-stu-id="317b6-137">For example, `net5.0` targets the .NET 5 API surface and is cross-platform friendly, but `net5.0-windows` targets the .NET 5 API surface as implemented on the Windows operating system.</span></span>

## <a name="net-framework-compatibility-mode"></a><span data-ttu-id="317b6-138">.NET Framework 兼容性模式</span><span class="sxs-lookup"><span data-stu-id="317b6-138">.NET Framework compatibility mode</span></span>

<span data-ttu-id="317b6-139">在分析完 NuGet 包之后，你可能会发现它们仅面向 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="317b6-139">After analyzing the NuGet packages, you might find that they only target the .NET Framework.</span></span>

<span data-ttu-id="317b6-140">从 .NET Standard 2.0 开始，引入了 .NET Framework 兼容性模式。</span><span class="sxs-lookup"><span data-stu-id="317b6-140">Starting with .NET Standard 2.0, the .NET Framework compatibility mode was introduced.</span></span> <span data-ttu-id="317b6-141">此兼容性模式允许 .NET Standard 和 .NET Core 项目引用 .NET Framework 库。</span><span class="sxs-lookup"><span data-stu-id="317b6-141">This compatibility mode allows .NET Standard and .NET Core projects to reference .NET Framework libraries.</span></span> <span data-ttu-id="317b6-142">引用 .NET Framework 库不适用于所有项目（如库使用 Windows Presentation Foundation (WPF) API 时），但它的开启了很多移植方案。</span><span class="sxs-lookup"><span data-stu-id="317b6-142">Referencing .NET Framework libraries doesn't work for all projects, such as if the library uses Windows Presentation Foundation (WPF) APIs, but it does unblock many porting scenarios.</span></span>

<span data-ttu-id="317b6-143">在项目中引用面向 .NET Framework 的 NuGet 包时（如 [`Huitian.PowerCollections`](https://www.nuget.org/packages/Huitian.PowerCollections)），你会收到类似于以下示例的包回退警告 ([NU1701](/nuget/reference/errors-and-warnings/nu1701))：</span><span class="sxs-lookup"><span data-stu-id="317b6-143">When you reference NuGet packages that target .NET Framework in your project, such as [`Huitian.PowerCollections`](https://www.nuget.org/packages/Huitian.PowerCollections), you get a package fallback warning ([NU1701](/nuget/reference/errors-and-warnings/nu1701)) similar to the following example:</span></span>

`NU1701: Package ‘Huitian.PowerCollections 1.0.0’ was restored using ‘.NETFramework,Version=v4.6.1’ instead of the project target framework ‘.NETStandard,Version=v2.0’. This package may not be fully compatible with your project.`

<span data-ttu-id="317b6-144">当添加包以及每次生成时都会显示该警告，以确保在项目中测试该包。</span><span class="sxs-lookup"><span data-stu-id="317b6-144">That warning is displayed when you add the package and every time you build to make sure you test that package with your project.</span></span> <span data-ttu-id="317b6-145">如果项目按预期工作，可通过在 Visual Studio 中编辑包属性或在最喜欢的代码编辑器中手动编辑项目文件来取消该警告。</span><span class="sxs-lookup"><span data-stu-id="317b6-145">If your project works as expected, you can suppress that warning by editing the package properties in Visual Studio or by manually editing the project file in your favorite code editor.</span></span>

<span data-ttu-id="317b6-146">若要通过编辑项目文件来取消警告，请找到要取消警告的包的 `PackageReference` 条目并添加 `NoWarn` 属性。</span><span class="sxs-lookup"><span data-stu-id="317b6-146">To suppress the warning by editing the project file, find the `PackageReference` entry for the package you want to suppress the warning for and add the `NoWarn` attribute.</span></span> <span data-ttu-id="317b6-147">`NoWarn` 属性接受所有警告 ID 的逗号分隔列表。</span><span class="sxs-lookup"><span data-stu-id="317b6-147">The `NoWarn` attribute accepts a comma-separated list of all the warning IDs.</span></span> <span data-ttu-id="317b6-148">以下示例显示如何通过手动编辑项目文件来取消 `Huitian.PowerCollections` 包的 `NU1701` 警告：</span><span class="sxs-lookup"><span data-stu-id="317b6-148">The following example shows how to suppress the `NU1701` warning for the `Huitian.PowerCollections` package by editing your project file manually:</span></span>

```xml
<ItemGroup>
  <PackageReference Include="Huitian.PowerCollections" Version="1.0.0" NoWarn="NU1701" />
</ItemGroup>
```

<span data-ttu-id="317b6-149">有关如何在 Visual Studio 中取消编译器警告的详细信息，请参阅[取消 NuGet 包的警告](/visualstudio/ide/how-to-suppress-compiler-warnings#suppress-warnings-for-nuget-packages)。</span><span class="sxs-lookup"><span data-stu-id="317b6-149">For more information on how to suppress compiler warnings in Visual Studio, see [Suppressing warnings for NuGet packages](/visualstudio/ide/how-to-suppress-compiler-warnings#suppress-warnings-for-nuget-packages).</span></span>

## <a name="if-nuget-packages-wont-run-on-net"></a><span data-ttu-id="317b6-150">如果 NuGet 包无法在 .NET 上运行</span><span class="sxs-lookup"><span data-stu-id="317b6-150">If NuGet packages won't run on .NET</span></span>

<span data-ttu-id="317b6-151">如果所依赖的 NuGet 包无法在 .NET Core 上运行，可以执行以下几项操作：</span><span class="sxs-lookup"><span data-stu-id="317b6-151">There are a few things you can do if a NuGet package you depend on doesn't run on .NET Core:</span></span>

- <span data-ttu-id="317b6-152">如果项目是开放源代码并托管在诸如 GitHub 中，则可以直接与开发人员交流。</span><span class="sxs-lookup"><span data-stu-id="317b6-152">If the project is open source and hosted somewhere like GitHub, you can engage the developers directly.</span></span>
- <span data-ttu-id="317b6-153">可直接在 [nuget.org](https://www.nuget.org/) 上联系作者。搜索包并单击包页面左侧的“联系所有者”  。</span><span class="sxs-lookup"><span data-stu-id="317b6-153">You can contact the author directly on [nuget.org](https://www.nuget.org/). Search for the package and click **Contact Owners** on the left-hand side of the package's page.</span></span>
- <span data-ttu-id="317b6-154">可以搜索在 .NET Core 上运行的其他包，它与所使用的包进行的是相同的任务。</span><span class="sxs-lookup"><span data-stu-id="317b6-154">You can search for another package that runs on .NET Core that accomplishes the same task as the package you were using.</span></span>
- <span data-ttu-id="317b6-155">可以尝试自己编写包所执行的代码。</span><span class="sxs-lookup"><span data-stu-id="317b6-155">You can attempt to write the code the package was doing yourself.</span></span>
- <span data-ttu-id="317b6-156">可以通过更改应用的功能来清除对包的依赖性，至少在该包有可用的兼容性版本之前都能这样做。</span><span class="sxs-lookup"><span data-stu-id="317b6-156">You could eliminate the dependency on the package by changing the functionality of your app, at least until a compatible version of the package becomes available.</span></span>

<span data-ttu-id="317b6-157">请注意，开源项目维护人员和 NuGet 包发布者通常是志愿者。</span><span class="sxs-lookup"><span data-stu-id="317b6-157">Remember that open-source project maintainers and NuGet package publishers are often volunteers.</span></span> <span data-ttu-id="317b6-158">他们因为关注某个特定领域而免费提供服务，通常从事另外一份职业。</span><span class="sxs-lookup"><span data-stu-id="317b6-158">They contribute because they care about a given domain, do it for free, and often have a different daytime job.</span></span> <span data-ttu-id="317b6-159">当联系他们请求 .NET Core 支持时请注意这点。</span><span class="sxs-lookup"><span data-stu-id="317b6-159">Be mindful of that when contacting them to ask for .NET Core support.</span></span>

<span data-ttu-id="317b6-160">如果使用上述任何选项都无法解决问题，则可能需要移植到最近版本的 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="317b6-160">If you can't resolve your issue with any of these options, you may have to port to .NET Core at a later date.</span></span>

<span data-ttu-id="317b6-161">.NET 团队需要了解哪些库最需要使用 .NET Core 支持。</span><span class="sxs-lookup"><span data-stu-id="317b6-161">The .NET Team would like to know which libraries are the most important to support with .NET Core.</span></span> <span data-ttu-id="317b6-162">你可以发送电子邮件到 dotnet@microsoft.com，谈谈你想要使用的库。</span><span class="sxs-lookup"><span data-stu-id="317b6-162">You can send an email to dotnet@microsoft.com about the libraries you'd like to use.</span></span>

## <a name="analyze-non-nuget-dependencies"></a><span data-ttu-id="317b6-163">分析非 NuGet 依赖项</span><span class="sxs-lookup"><span data-stu-id="317b6-163">Analyze non-NuGet dependencies</span></span>

<span data-ttu-id="317b6-164">用户可能拥有不是 NuGet 包的依赖项，如文件系统中的 DLL。</span><span class="sxs-lookup"><span data-stu-id="317b6-164">You may have a dependency that isn't a NuGet package, such as a DLL in the file system.</span></span> <span data-ttu-id="317b6-165">确定该依赖项是否可移植的唯一方法是运行 [.NET 可移植性分析器](https://github.com/Microsoft/dotnet-apiport)工具。</span><span class="sxs-lookup"><span data-stu-id="317b6-165">The only way to determine the portability of that dependency is to run the [.NET Portability Analyzer](https://github.com/Microsoft/dotnet-apiport) tool.</span></span> <span data-ttu-id="317b6-166">该工具可以分析面向 .NET Framework 的程序集，并识别不可移植到其他 .NET 平台（如 .NET Core）的 API。</span><span class="sxs-lookup"><span data-stu-id="317b6-166">The tool analyzes assemblies that target the .NET Framework and identifies APIs that aren't portable to other .NET platforms such as .NET Core.</span></span> <span data-ttu-id="317b6-167">可将该工具作为控制台应用程序或作为 [Visual Studio 扩展](../../standard/analyzers/portability-analyzer.md)来运行。</span><span class="sxs-lookup"><span data-stu-id="317b6-167">You can run the tool as a console application or as a [Visual Studio extension](../../standard/analyzers/portability-analyzer.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="317b6-168">后续步骤</span><span class="sxs-lookup"><span data-stu-id="317b6-168">Next steps</span></span>

- [<span data-ttu-id="317b6-169">有关从 .NET Framework 移植到 .NET 的概述</span><span class="sxs-lookup"><span data-stu-id="317b6-169">Overview of porting from .NET Framework to .NET</span></span>](index.md)
- [<span data-ttu-id="317b6-170">移植库</span><span class="sxs-lookup"><span data-stu-id="317b6-170">Port libraries</span></span>](libraries.md)
