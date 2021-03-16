---
title: 迁移到 .NET 5 的示例
description: 演示如何将面向 .NET Framework 的示例应用程序迁移到 .NET 5。
ms.date: 01/19/2021
ms.openlocfilehash: 02a45859dfca891598e235e3de1ed968aefb5bf4
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605160"
---
# <a name="example-of-migrating-to-net"></a><span data-ttu-id="50075-103">迁移到 .NET 的示例</span><span class="sxs-lookup"><span data-stu-id="50075-103">Example of migrating to .NET</span></span>

<span data-ttu-id="50075-104">本章节提供了实用的指导原则，可帮助你将现有应用程序从 .NET Framework 迁移到 .NET。</span><span class="sxs-lookup"><span data-stu-id="50075-104">In this chapter, we present practical guidelines to help you perform a migration of your existing application from .NET Framework to .NET.</span></span>

<span data-ttu-id="50075-105">章节中提供了一个结构良好的过程（你可以遵循此过程），以及每一步需要考虑的最重要事项。</span><span class="sxs-lookup"><span data-stu-id="50075-105">We present a well-structured process you can follow and the most important things to consider on each step.</span></span>

<span data-ttu-id="50075-106">接着，章节中记录了示例桌面应用程序从 WinForms 和 WPF 版本的分步迁移过程。</span><span class="sxs-lookup"><span data-stu-id="50075-106">We then document a step-by-step migration process for a sample desktop application, both from WinForms and WPF versions.</span></span>

## <a name="migration-process-overview"></a><span data-ttu-id="50075-107">迁移过程概述</span><span class="sxs-lookup"><span data-stu-id="50075-107">Migration process overview</span></span>

<span data-ttu-id="50075-108">迁移过程包括四个连续步骤：</span><span class="sxs-lookup"><span data-stu-id="50075-108">The migration process consists of four sequential steps:</span></span>

1. <span data-ttu-id="50075-109">准备工作：了解项目所具有的依赖项，对之后的操作有一个概念。</span><span class="sxs-lookup"><span data-stu-id="50075-109">**Preparation**: Understand the dependencies the project has to have an idea of what's ahead.</span></span> <span data-ttu-id="50075-110">在此步骤中，将当前项目带入一种简化迁移启动点的状态。</span><span class="sxs-lookup"><span data-stu-id="50075-110">In this step, you take the current project into a state that simplifies the startup point for the migration.</span></span>

2. <span data-ttu-id="50075-111">迁移项目文件：.NET 项目使用新的 SDK 样式项目格式。</span><span class="sxs-lookup"><span data-stu-id="50075-111">**Migrate Project File:** .NET projects use the new SDK-style project format.</span></span> <span data-ttu-id="50075-112">创建一个采用此格式的新项目文件，或更新必须使用 SDK 样式的项目文件。</span><span class="sxs-lookup"><span data-stu-id="50075-112">Create a new project file with this format or update the one you have to use the SDK style.</span></span>

3. <span data-ttu-id="50075-113">修复代码并生成：在 .NET 中生成代码，解决 .NET Framework 和 .NET 之间的 API 级差异。</span><span class="sxs-lookup"><span data-stu-id="50075-113">**Fix code and build:** Build the code in .NET addressing API-level differences between .NET Framework and .NET.</span></span> <span data-ttu-id="50075-114">如需要，将第三方包更新为支持 .NET 的包。</span><span class="sxs-lookup"><span data-stu-id="50075-114">If needed, update third-party packages to the ones that support .NET.</span></span>

4. <span data-ttu-id="50075-115">运行并测试：可能存在直到运行时才会显示的差异。</span><span class="sxs-lookup"><span data-stu-id="50075-115">**Run and test:** There might be differences that don't show up until run time.</span></span> <span data-ttu-id="50075-116">因此，请勿忘记运行应用程序并测试所有操作是否按预期方式工作。</span><span class="sxs-lookup"><span data-stu-id="50075-116">So, don't forget to run the application and test that everything works as expected.</span></span>

### <a name="preparation"></a><span data-ttu-id="50075-117">准备工作</span><span class="sxs-lookup"><span data-stu-id="50075-117">Preparation</span></span>

#### <a name="migrate-packagesconfig-file"></a><span data-ttu-id="50075-118">迁移 packages.config 文件</span><span class="sxs-lookup"><span data-stu-id="50075-118">Migrate packages.config file</span></span>

<span data-ttu-id="50075-119">在 .NET Framework 应用程序中，所有对外部包的引用都在 packages.config 文件中声明。</span><span class="sxs-lookup"><span data-stu-id="50075-119">In a .NET Framework application, all references to external packages are declared in the *packages.config* file.</span></span> <span data-ttu-id="50075-120">在 .NET 中，不再需要使用 packages.config 文件。</span><span class="sxs-lookup"><span data-stu-id="50075-120">In .NET, there's no longer the need to use the *packages.config* file.</span></span> <span data-ttu-id="50075-121">而请使用项目文件中的 [PackageReference](../../core/project-sdk/msbuild-props.md#packagereference) 属性来指定应用的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="50075-121">Instead, use the [PackageReference](../../core/project-sdk/msbuild-props.md#packagereference) property inside the project file to specify the NuGet packages for your app.</span></span>

<span data-ttu-id="50075-122">因此，你需要从一种格式转换为另一种格式。</span><span class="sxs-lookup"><span data-stu-id="50075-122">So, you need to transition from one format to another.</span></span> <span data-ttu-id="50075-123">可手动进行更新，从而获取 packages.config 文件中包含的依赖项并将其迁移到采用 `PackageReference` 格式的项目文件中。</span><span class="sxs-lookup"><span data-stu-id="50075-123">You can do the update manually, taking the dependencies contained in the *packages.config* file and migrating them to the project file with the `PackageReference` format.</span></span> <span data-ttu-id="50075-124">或者，可让 Visual Studio 为你完成该工作：右键单击 packages.config 文件，然后选择“将 packages.config 迁移到 PackageReference”选项。</span><span class="sxs-lookup"><span data-stu-id="50075-124">Or, you can let Visual Studio do the work for you: right-click on the *packages.config* file and select the **Migrate packages.config to PackageReference** option.</span></span>

#### <a name="verify-every-dependency-compatibility-in-net"></a><span data-ttu-id="50075-125">验证 .NET 中每个依赖项的兼容性</span><span class="sxs-lookup"><span data-stu-id="50075-125">Verify every dependency compatibility in .NET</span></span>

<span data-ttu-id="50075-126">迁移包引用后，必须检查每个引用的兼容性。</span><span class="sxs-lookup"><span data-stu-id="50075-126">Once you've migrated the package references, you must check each reference for compatibility.</span></span> <span data-ttu-id="50075-127">可在 [nuget.org](https://www.nuget.org/) 上浏览应用程序使用的每个 NuGet 包的依赖项。如果包具有 .NET Standard 依赖项，则它将在 .NET 5.0 上运行，因为 .NET [支持](../../standard/net-standard.md#net-implementation-support)所有版本的 .NET Standard。</span><span class="sxs-lookup"><span data-stu-id="50075-127">You can explore the dependencies of each NuGet package your application is using on [nuget.org](https://www.nuget.org/). If the package has .NET Standard dependencies, then it's going to work on .NET 5.0 because .NET [supports](../../standard/net-standard.md#net-implementation-support) all versions of .NET Standard.</span></span> <span data-ttu-id="50075-128">下图显示了 `Castle.Windsor` 包的依赖项：</span><span class="sxs-lookup"><span data-stu-id="50075-128">The following image shows the dependencies for the `Castle.Windsor` package:</span></span>

![Castle.Windsor 包的 NuGet 依赖项的屏幕截图](./media/example-migration-core/nuget-dependencies.png)

<span data-ttu-id="50075-130">若要检查包的兼容性，可使用 <https://fuget.org> ，该工具提供了有关版本和依赖项的更详细信息。</span><span class="sxs-lookup"><span data-stu-id="50075-130">To check the package compatibility, you can use the tool <https://fuget.org> that offers a more detailed information about versions and dependencies.</span></span>

<span data-ttu-id="50075-131">该项目可能引用了不支持 .NET 的旧版包，但你可能会找到支持它的较新版本。</span><span class="sxs-lookup"><span data-stu-id="50075-131">Maybe the project is referencing older package versions that don't support .NET, but you might find newer versions that do support it.</span></span> <span data-ttu-id="50075-132">因此，将包更新到较新版本通常是一个不错的建议。</span><span class="sxs-lookup"><span data-stu-id="50075-132">So, updating packages to newer versions is generally a good recommendation.</span></span> <span data-ttu-id="50075-133">但是，你应该考虑到，更新包版本可能会引入一些中断性变更，这些变更将强制你更新代码。</span><span class="sxs-lookup"><span data-stu-id="50075-133">However, you should consider that updating the package version can introduce some breaking changes that would force you to update your code.</span></span>

<span data-ttu-id="50075-134">如果找不到兼容的版本，会发生什么情况？</span><span class="sxs-lookup"><span data-stu-id="50075-134">What happens if you don't find a compatible version?</span></span> <span data-ttu-id="50075-135">如果你只是因为这些中断性变更而不想更新包的版本，该怎么办？</span><span class="sxs-lookup"><span data-stu-id="50075-135">What if you just don't want to update the version of a package because of these breaking changes?</span></span> <span data-ttu-id="50075-136">请不要担心，因为可以依赖于 .NET 应用程序中的 .NET Framework 程序包。</span><span class="sxs-lookup"><span data-stu-id="50075-136">Don't worry because it's possible to depend on .NET Framework packages from a .NET application.</span></span> <span data-ttu-id="50075-137">请勿忘记对其进行大量测试，因为如果外部包调用的 API 在 .NET 中不可用，则可能会导致运行时错误。</span><span class="sxs-lookup"><span data-stu-id="50075-137">Don't forget to test it extensively because it can cause run-time errors if the external package calls an API that isn't available on .NET.</span></span> <span data-ttu-id="50075-138">在你使用不会更新的旧包时，这非常有用，你只需重新设定目标即可在 .NET 上工作。</span><span class="sxs-lookup"><span data-stu-id="50075-138">This is great for when you're using an old package that isn't going to be updated and you can just retarget to work on the .NET.</span></span>

#### <a name="check-for-api-compatibility"></a><span data-ttu-id="50075-139">检查 API 兼容性</span><span class="sxs-lookup"><span data-stu-id="50075-139">Check for API compatibility</span></span>

<span data-ttu-id="50075-140">由于 .NET Framework 和 .NET 中的 API 图面类似但并不完全相同，因此必须检查 .NET 上可用的 API 和不可用的 API。</span><span class="sxs-lookup"><span data-stu-id="50075-140">Since the API surface in .NET Framework and .NET is similar but not identical, you must check which APIs are available on .NET and which aren't.</span></span> <span data-ttu-id="50075-141">可使用 .NET 可移植性分析器工具来显示 .NET 上不存在的已使用 API。</span><span class="sxs-lookup"><span data-stu-id="50075-141">You can use the .NET Portability Analyzer tool to surface APIs used that aren't present on .NET.</span></span> <span data-ttu-id="50075-142">它会查看应用的二进制级别，提取调用的所有 API，然后列出目标框架（本例中，目标框架是 .NET 5.0）上不可用的 API。</span><span class="sxs-lookup"><span data-stu-id="50075-142">It looks at the binary level of your app, extracts all the APIs that are called, and then lists which APIs aren't available on your target framework (.NET 5.0 in this case).</span></span>

<span data-ttu-id="50075-143">有关此工具的详细信息，请参阅：</span><span class="sxs-lookup"><span data-stu-id="50075-143">You can find more information about this tool at:</span></span>

<https://docs.microsoft.com/dotnet/standard/analyzers/portability-analyzer>

<span data-ttu-id="50075-144">对于此工具来说，有趣的是，它只显示与你自己的代码的差异，而不显示与你不能更改的外部包中代码的差异。</span><span class="sxs-lookup"><span data-stu-id="50075-144">An interesting aspect of this tool is that it only surfaces the differences from your own code and not code from external packages, which you can't change.</span></span> <span data-ttu-id="50075-145">请记住，你应该已经更新了这些包中的大部分包，使它们能够与 .NET 一起工作。</span><span class="sxs-lookup"><span data-stu-id="50075-145">Remember you should have updated most of these packages to make them work with .NET.</span></span>

### <a name="migrate-with-try-convert-tool"></a><span data-ttu-id="50075-146">通过 Try Convert 工具进行迁移</span><span class="sxs-lookup"><span data-stu-id="50075-146">Migrate with Try Convert tool</span></span>

<span data-ttu-id="50075-147">[Try Convert](https://github.com/dotnet/try-convert/releases) 工具非常适合用于迁移项目。</span><span class="sxs-lookup"><span data-stu-id="50075-147">The [Try Convert](https://github.com/dotnet/try-convert/releases) tool is a great way to migrate a project.</span></span> <span data-ttu-id="50075-148">它是一个全局工具，它尝试将项目文件从旧样式升级到新的 SDK 样式，并将适用项目的目标重定为 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="50075-148">It's a global tool that attempts to upgrade your project file from the old style to the new SDK style, and retargets applicable projects to .NET 5.</span></span> <span data-ttu-id="50075-149">安装后，你可运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="50075-149">Once installed, you can run the following commands:</span></span>

```dotnetcli
try-convert -p "<path to your project file>"
```

<span data-ttu-id="50075-150">或：</span><span class="sxs-lookup"><span data-stu-id="50075-150">Or:</span></span>

```dotnetcli
try-convert -w "<path to your solution>"
```

> [!NOTE]
> <span data-ttu-id="50075-151">try-convert 工具作为 [.NET 升级助手工具](https://aka.ms/dotnet-upgrade-assistant)的一部分自动运行。</span><span class="sxs-lookup"><span data-stu-id="50075-151">The try-convert tool is run automatically as part of the [.NET Upgrade Assistant tool](https://aka.ms/dotnet-upgrade-assistant).</span></span> <span data-ttu-id="50075-152">请考虑运行完整的升级助手，而不只是 Try Convert。</span><span class="sxs-lookup"><span data-stu-id="50075-152">Consider running the full Upgrade Assistant and not just Try Convert.</span></span>

<span data-ttu-id="50075-153">在该工具尝试转换后，在 Visual Studio 中重载文件以运行和测试。</span><span class="sxs-lookup"><span data-stu-id="50075-153">After the tool attempts the conversion, reload your files in Visual Studio to run and test.</span></span> <span data-ttu-id="50075-154">由于项目的具体情况，Try Convert 可能无法执行转换。</span><span class="sxs-lookup"><span data-stu-id="50075-154">There's a possibility that Try Convert won't be able to perform the conversion due to the specifics of your project.</span></span> <span data-ttu-id="50075-155">在这种情况下，可参考以下步骤。</span><span class="sxs-lookup"><span data-stu-id="50075-155">In that case, you can refer the below steps.</span></span>

#### <a name="migrate-manually"></a><span data-ttu-id="50075-156">手动迁移</span><span class="sxs-lookup"><span data-stu-id="50075-156">Migrate manually</span></span>

1. <span data-ttu-id="50075-157">创建新的 .NET 项目</span><span class="sxs-lookup"><span data-stu-id="50075-157">Create the new .NET project</span></span>

<span data-ttu-id="50075-158">在大多数情况下，你会希望将现有项目更新为新的 .NET 格式。</span><span class="sxs-lookup"><span data-stu-id="50075-158">In most cases, you'll want to update your existing project to the new .NET format.</span></span> <span data-ttu-id="50075-159">不过，你也可以创建一个新项目，同时保留旧项目。</span><span class="sxs-lookup"><span data-stu-id="50075-159">However, you can also create a new project while maintaining the old one.</span></span> <span data-ttu-id="50075-160">更新旧项目的主要缺点是你会丢失设计器支持，而这对你和你的开发团队来说可能很重要。</span><span class="sxs-lookup"><span data-stu-id="50075-160">The main drawback from updating the old project is that you lose designer support, which may be important to you and your development team.</span></span> <span data-ttu-id="50075-161">如果要继续使用设计器，则必须与旧 .NET 项目并行创建一个新 .NET 项目，并共享资产。</span><span class="sxs-lookup"><span data-stu-id="50075-161">If you want to keep using the designer, you must create a new .NET project in parallel with the old one and share assets.</span></span> <span data-ttu-id="50075-162">如果需要修改设计器中的 UI 元素，可切换到旧项目来执行该操作。</span><span class="sxs-lookup"><span data-stu-id="50075-162">If you need to modify UI elements in the designer, you can switch to the old project to do that.</span></span> <span data-ttu-id="50075-163">由于资源已链接，因此这些更改也会在 .NET 项目中更新。</span><span class="sxs-lookup"><span data-stu-id="50075-163">And since assets are linked, they'll be updated in the .NET project as well.</span></span>

<span data-ttu-id="50075-164">适用于 .NET 的 [SDK 样式项目](../../core/project-sdk/msbuild-props.md)比 .NET Framework 的项目格式简单得多。</span><span class="sxs-lookup"><span data-stu-id="50075-164">The [SDK-style project](../../core/project-sdk/msbuild-props.md) for .NET is a lot simpler than .NET Framework's project format.</span></span> <span data-ttu-id="50075-165">除了前面提到的 `PackageReference` 条目外，无需执行更多操作。</span><span class="sxs-lookup"><span data-stu-id="50075-165">Apart from the previously mentioned `PackageReference` entries, you won't need to do much more.</span></span> <span data-ttu-id="50075-166">新的项目格式[包括具有特定扩展名的文件](../../core/project-sdk/overview.md#default-includes-and-excludes)（如 `.cs` 和 `.xaml` 文件），而无需在项目文件中显式包含这些文件。</span><span class="sxs-lookup"><span data-stu-id="50075-166">The new project format [includes files with certain extensions by default](../../core/project-sdk/overview.md#default-includes-and-excludes), such as `.cs` and `.xaml` files, without the need to explicitly include them in the project file.</span></span>

#### <a name="assemblyinfo-considerations"></a><span data-ttu-id="50075-167">AssemblyInfo 注意事项</span><span class="sxs-lookup"><span data-stu-id="50075-167">AssemblyInfo considerations</span></span>

<span data-ttu-id="50075-168">特性针对 .NET 项目自动生成。</span><span class="sxs-lookup"><span data-stu-id="50075-168">Attributes are autogenerated on .NET projects.</span></span> <span data-ttu-id="50075-169">如果项目包含 AssemblyInfo.cs 文件，则定义将重复，这会导致编译冲突。</span><span class="sxs-lookup"><span data-stu-id="50075-169">If the project contains an *AssemblyInfo.cs* file, the definitions will be duplicated, which will cause compilation conflicts.</span></span> <span data-ttu-id="50075-170">可删除旧版 AssemblyInfo.cs 文件，或通过将以下条目添加到 .NET 项目文件来禁用自动生成：</span><span class="sxs-lookup"><span data-stu-id="50075-170">You can delete the older *AssemblyInfo.cs* file or disable autogeneration by adding the following entry to the .NET project file:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
  </PropertyGroup>
</Project>
```

#### <a name="resources"></a><span data-ttu-id="50075-171">资源</span><span class="sxs-lookup"><span data-stu-id="50075-171">Resources</span></span>

<span data-ttu-id="50075-172">嵌入的资源会自动包含在内，但资源不会，因此需要将资源迁移到新的项目文件。</span><span class="sxs-lookup"><span data-stu-id="50075-172">Embedded resources are included automatically but resources aren't, so you need to migrate the resources to the new project file.</span></span>

#### <a name="package-references"></a><span data-ttu-id="50075-173">包引用</span><span class="sxs-lookup"><span data-stu-id="50075-173">Package references</span></span>

<span data-ttu-id="50075-174">通过“将 packages.config 迁移到 PackageReference”选项，可轻松地将外部包引用移至前面提到的新格式。</span><span class="sxs-lookup"><span data-stu-id="50075-174">With the **Migrate packages.config to PackageReference** option, you can easily move your external package references to the new format as previously mentioned.</span></span>

#### <a name="update-package-references"></a><span data-ttu-id="50075-175">更新包引用</span><span class="sxs-lookup"><span data-stu-id="50075-175">Update package references</span></span>

<span data-ttu-id="50075-176">如前一部分中所示，更新发现的兼容包版本。</span><span class="sxs-lookup"><span data-stu-id="50075-176">Update the versions of the packages you've found to be compatible, as shown in the previous section.</span></span>

### <a name="fix-the-code-and-build"></a><span data-ttu-id="50075-177">修复代码和生成</span><span class="sxs-lookup"><span data-stu-id="50075-177">Fix the code and build</span></span>

#### <a name="microsoftwindowscompatibility"></a><span data-ttu-id="50075-178">Microsoft.Windows.Compatibility</span><span class="sxs-lookup"><span data-stu-id="50075-178">Microsoft.Windows.Compatibility</span></span>

<span data-ttu-id="50075-179">如果应用程序依赖于 .NET 上不可用的 API（如注册表、ACL 或 WCF），则必须包含对 `Microsoft.Windows.Compatibility` 包的引用以添加这些特定于 Windows 的 API。</span><span class="sxs-lookup"><span data-stu-id="50075-179">If your application depends on APIs that aren't available on .NET, such as Registry, ACLs, or WCF, you have to include a reference to the `Microsoft.Windows.Compatibility` package to add these Windows-specific APIs.</span></span> <span data-ttu-id="50075-180">它们可在 .NET 上工作，但由于它们无法跨平台，因此未包含在内。</span><span class="sxs-lookup"><span data-stu-id="50075-180">They work on .NET but aren't included as they aren't cross-platform.</span></span>

<span data-ttu-id="50075-181">有一个名为 API Analyzer (<https://docs.microsoft.com/dotnet/standard/analyzers/api-analyzer>) 的工具，可帮助你识别与你的代码不兼容的 API。</span><span class="sxs-lookup"><span data-stu-id="50075-181">There's a tool called API Analyzer (<https://docs.microsoft.com/dotnet/standard/analyzers/api-analyzer>) that helps you identify APIs that aren't compatible with your code.</span></span>

#### <a name="use-if-directives"></a><span data-ttu-id="50075-182">使用 \#if 指令</span><span class="sxs-lookup"><span data-stu-id="50075-182">Use \#if directives</span></span>

<span data-ttu-id="50075-183">如果在以 .NET Framework 和 .NET 为目标时需要不同的执行路径，则应使用编译常量。</span><span class="sxs-lookup"><span data-stu-id="50075-183">If you need different execution paths when targeting .NET Framework and .NET, you should use compilation constants.</span></span> <span data-ttu-id="50075-184">向代码中添加一些 \#if 指令，为这两个目标保留相同的代码库。</span><span class="sxs-lookup"><span data-stu-id="50075-184">Add some \#if directives to your code to keep the same code base for both targets.</span></span>

#### <a name="technologies-not-available-on-net"></a><span data-ttu-id="50075-185">.NET 上不可用的技术</span><span class="sxs-lookup"><span data-stu-id="50075-185">Technologies not available on .NET</span></span>

<span data-ttu-id="50075-186">某些技术在 .NET 上不可用，例如：</span><span class="sxs-lookup"><span data-stu-id="50075-186">Some technologies aren't available on .NET, such as:</span></span>

* <span data-ttu-id="50075-187">AppDomain</span><span class="sxs-lookup"><span data-stu-id="50075-187">AppDomains</span></span>
* <span data-ttu-id="50075-188">远程处理</span><span class="sxs-lookup"><span data-stu-id="50075-188">Remoting</span></span>
* <span data-ttu-id="50075-189">代码访问安全性</span><span class="sxs-lookup"><span data-stu-id="50075-189">Code Access Security</span></span>
* <span data-ttu-id="50075-190">WCF 服务器</span><span class="sxs-lookup"><span data-stu-id="50075-190">WCF Server</span></span>
* <span data-ttu-id="50075-191">Windows 工作流</span><span class="sxs-lookup"><span data-stu-id="50075-191">Windows Workflow</span></span>

<span data-ttu-id="50075-192">因此，如果在应用程序中使用这些技术，则需要寻找这些技术的替代技术。</span><span class="sxs-lookup"><span data-stu-id="50075-192">That's why you need to find a replacement for these technologies if you're using them in your application.</span></span> <span data-ttu-id="50075-193">有关详细信息，请参阅文章[在 .NET Core 和 .NET 5+ 上不可用的 .NET Framework 技术](../../core/porting/net-framework-tech-unavailable.md)。</span><span class="sxs-lookup"><span data-stu-id="50075-193">For more information, see the [.NET Framework technologies unavailable on .NET Core and .NET 5+](../../core/porting/net-framework-tech-unavailable.md) article.</span></span>

#### <a name="regenerate-autogenerated-clients"></a><span data-ttu-id="50075-194">重新生成自动生成的客户端</span><span class="sxs-lookup"><span data-stu-id="50075-194">Regenerate autogenerated clients</span></span>

<span data-ttu-id="50075-195">如果应用程序使用自动生成的代码（例如 WCF 客户端），则可能需要重新生成此代码，从而以 .NET 为目标。</span><span class="sxs-lookup"><span data-stu-id="50075-195">If your application uses autogenerated code, such as a WCF client, you may need to regenerate this code to target .NET.</span></span> <span data-ttu-id="50075-196">有时，你可以找到某些缺少的引用，因为它们可能不包含在一组默认的 .NET 程序集中。</span><span class="sxs-lookup"><span data-stu-id="50075-196">Sometimes, you can find some missing references since they may not be included as part of the default .NET assemblies set.</span></span> <span data-ttu-id="50075-197">使用 <https://apisof.net/> 等工具，可轻松地找到缺少的引用所在的程序集，并从 NuGet 添加该引用。</span><span class="sxs-lookup"><span data-stu-id="50075-197">Using a tool like <https://apisof.net/>, you can easily locate the assembly the missing reference lives in and add it from NuGet.</span></span>

#### <a name="rolling-back-package-versions"></a><span data-ttu-id="50075-198">回滚包版本</span><span class="sxs-lookup"><span data-stu-id="50075-198">Rolling back package versions</span></span>

<span data-ttu-id="50075-199">一般情况下，我们之前已提到过，你最好更新每个单独的包版本，使其与 .NET 兼容。</span><span class="sxs-lookup"><span data-stu-id="50075-199">As a general rule, we've previously stated that you better update every single package version to be compatible with .NET.</span></span> <span data-ttu-id="50075-200">然而，你会发现以一个程序集的更新版本和兼容版本为目标是不值得的。</span><span class="sxs-lookup"><span data-stu-id="50075-200">However, you can find that targeting an updated and compatible version of an assembly just doesn't pay off.</span></span> <span data-ttu-id="50075-201">如果更改的成本不可接受，可考虑回滚包版本，保留你在 .NET Framework 上使用的版本。</span><span class="sxs-lookup"><span data-stu-id="50075-201">If the cost of change isn't acceptable, you can consider rolling back package versions keeping the ones you use on .NET Framework.</span></span> <span data-ttu-id="50075-202">尽管它们可能不以 .NET 为目标，但它们应该能够正常工作，除非它们调用了某些不受支持的 API。</span><span class="sxs-lookup"><span data-stu-id="50075-202">Although they may not be targeting .NET, they should work well unless they call some unsupported APIs.</span></span>

### <a name="run-and-test"></a><span data-ttu-id="50075-203">运行和测试</span><span class="sxs-lookup"><span data-stu-id="50075-203">Run and test</span></span>

<span data-ttu-id="50075-204">若应用程序生成无错误，便可通过测试每个功能开始迁移的最后一步。</span><span class="sxs-lookup"><span data-stu-id="50075-204">Once you have your application building with no errors, you can start the last step of the migration by testing every functionality.</span></span>

<span data-ttu-id="50075-205">在最后一步中，你会发现一些不同的问题，具体取决于应用程序的复杂性以及使用的依赖项和 API。</span><span class="sxs-lookup"><span data-stu-id="50075-205">In this final step, you can find several different issues depending on the complexity of your application and the dependencies and APIs you're using.</span></span>

<span data-ttu-id="50075-206">例如，如果你使用配置文件 (app.config)，则可能会在运行时发现一些错误，如“配置节”不存在。</span><span class="sxs-lookup"><span data-stu-id="50075-206">For example, if you use configuration files (*app.config*), you may find some errors at run time like Configuration Sections not present.</span></span> <span data-ttu-id="50075-207">使用 `Microsoft.Extensions.Configuration` NuGet 包应可修复该错误。</span><span class="sxs-lookup"><span data-stu-id="50075-207">Using the `Microsoft.Extensions.Configuration` NuGet package should fix that error.</span></span>

<span data-ttu-id="50075-208">出现错误的另一个原因是使用了 `BeginInvoke` 和 `EndInvoke` 方法，因为它们在 .NET 上不受支持。</span><span class="sxs-lookup"><span data-stu-id="50075-208">Another reason for errors is the use of the `BeginInvoke` and `EndInvoke` methods because they aren't supported on .NET.</span></span> <span data-ttu-id="50075-209">.NET 不支持它们，因为它们依赖于远程处理，而远程处理 .NET 上并不存在。</span><span class="sxs-lookup"><span data-stu-id="50075-209">They aren't supported on .NET because they have a dependency on Remoting, which doesn't exist on .NET.</span></span> <span data-ttu-id="50075-210">若要解决此问题，请尝试使用 `await` 关键字（如可用）或 <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="50075-210">To solve this issue, try to use the `await` keyword (when available) or the <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="50075-211">可使用兼容性分析器，识别代码中可能会在 .NET 中运行时引发问题的 API 和代码模式。</span><span class="sxs-lookup"><span data-stu-id="50075-211">You can use compatibility analyzers to let you identify APIs and code patterns in your code that can potentially cause problems at run time with .NET.</span></span> <span data-ttu-id="50075-212">转到 <https://github.com/dotnet/platform-compat>，对你的项目使用 .NET API 分析器。</span><span class="sxs-lookup"><span data-stu-id="50075-212">Go to <https://github.com/dotnet/platform-compat> and use the .NET API analyzer on your project.</span></span>

## <a name="migrating-a-windows-forms-application"></a><span data-ttu-id="50075-213">迁移 Windows 窗体应用程序</span><span class="sxs-lookup"><span data-stu-id="50075-213">Migrating a Windows Forms application</span></span>

<span data-ttu-id="50075-214">为了展示 Windows 窗体应用程序的完整迁移过程，我们选择迁移 <https://github.com/dotnet-architecture/eShopModernizing/tree/master/eShopLegacyNTier/src/eShopWinForms> 处提供的 eShop 示例应用程序。</span><span class="sxs-lookup"><span data-stu-id="50075-214">To showcase a complete migration process of a Windows Forms application, we've chosen to migrate the eShop sample application available at <https://github.com/dotnet-architecture/eShopModernizing/tree/master/eShopLegacyNTier/src/eShopWinForms>.</span></span> <span data-ttu-id="50075-215">可在 <https://github.com/dotnet-architecture/eShopModernizing/tree/master/eShopModernizedNTier/src/eShopWinForms> 处找到迁移的完整结果。</span><span class="sxs-lookup"><span data-stu-id="50075-215">You can find the complete result of the migration at <https://github.com/dotnet-architecture/eShopModernizing/tree/master/eShopModernizedNTier/src/eShopWinForms>.</span></span>

<span data-ttu-id="50075-216">此应用程序显示一个产品目录，并允许用户导航、筛选和搜索产品。</span><span class="sxs-lookup"><span data-stu-id="50075-216">This application shows a product catalog and allows the user to navigate, filter, and search for products.</span></span> <span data-ttu-id="50075-217">从体系结构的角度来看，该应用依赖于充当后端数据库外观的外部 WCF 服务。</span><span class="sxs-lookup"><span data-stu-id="50075-217">From an architecture point of view, the app relies on an external WCF service that serves as a façade to a back-end database.</span></span>

<span data-ttu-id="50075-218">下图中显示了主应用程序窗口：</span><span class="sxs-lookup"><span data-stu-id="50075-218">You can see the main application window in the following picture:</span></span>

![主应用程序窗口](./media/example-migration-core/main-application-window.png)

<span data-ttu-id="50075-220">如果你打开 .csproj 项目文件，则可看到如下所示的内容：</span><span class="sxs-lookup"><span data-stu-id="50075-220">If you open the *.csproj* project file, you can see something like this:</span></span>

![csproj 文件内容的屏幕截图](./media/example-migration-core/csproj-file.png)

<span data-ttu-id="50075-222">如前所述，.NET 项目的样式更精简，你需要将项目结构迁移到新的 .NET SDK 样式。</span><span class="sxs-lookup"><span data-stu-id="50075-222">As previously mentioned, .NET project has a more compact style and you need to migrate the project structure to the new .NET SDK style.</span></span>

<span data-ttu-id="50075-223">在解决方案资源管理器中，右键单击该 Windows 窗体项目，然后选择“卸载项目” > “编辑” 。</span><span class="sxs-lookup"><span data-stu-id="50075-223">In the Solution Explorer, right click on the Windows Forms project and select **Unload Project** > **Edit**.</span></span>

<span data-ttu-id="50075-224">现在，可更新该 .csproj 文件。</span><span class="sxs-lookup"><span data-stu-id="50075-224">Now you can update the .csproj file.</span></span> <span data-ttu-id="50075-225">你将删除整个内容，并将其替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="50075-225">You'll delete the entire content and replace it with the following code:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net5.0-windows</TargetFramework>
    <UseWindowsForms>true</UseWindowsForms>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="50075-226">保存并重载该项目。</span><span class="sxs-lookup"><span data-stu-id="50075-226">Save and reload the project.</span></span> <span data-ttu-id="50075-227">你现在已经完成了项目文件的更新，并且该项目面向 .NET。</span><span class="sxs-lookup"><span data-stu-id="50075-227">You're now done updating the project file and the project is targeting the .NET.</span></span>

<span data-ttu-id="50075-228">如果此时编译项目，你会发现一些与 WCF 客户端引用相关的错误。</span><span class="sxs-lookup"><span data-stu-id="50075-228">If you compile the project at this point, you'll find some errors related to the WCF client reference.</span></span> <span data-ttu-id="50075-229">由于此代码是自动生成的，因此必须重新生成它，使其以 .NET 为目标。</span><span class="sxs-lookup"><span data-stu-id="50075-229">Since this code is autogenerated, you must regenerate it to target .NET.</span></span>

![Visual Studio 中显示错误的错误列表](./media/example-migration-core/winforms-compilation-errors.png)

<span data-ttu-id="50075-231">删除 Reference.cs 文件并生成新的服务客户端。</span><span class="sxs-lookup"><span data-stu-id="50075-231">Delete the *Reference.cs* file and generate a new Service Client.</span></span>

<span data-ttu-id="50075-232">右键单击“连接的服务”，然后选择“添加连接的服务”选项 。</span><span class="sxs-lookup"><span data-stu-id="50075-232">Right-click on **Connected Services** and select the **Add Connected Service** option.</span></span>

![已选中“添加连接的服务”选项的“连接的服务”菜单屏幕截图](./media/example-migration-core/add-connected-service.png)

<span data-ttu-id="50075-234">“连接的服务”窗口随即打开。</span><span class="sxs-lookup"><span data-stu-id="50075-234">The Connected Services window opens.</span></span> <span data-ttu-id="50075-235">选择“Microsoft WCF Web 服务”选项。</span><span class="sxs-lookup"><span data-stu-id="50075-235">Select the **Microsoft WCF Web Service** option.</span></span>

![“连接的服务”窗口的屏幕截图](./media/example-migration-core/connected-services-window.png)

<span data-ttu-id="50075-237">如果你在与本示例相同的解决方案中有 WCF 服务，则可选择“发现”选项，而不是指定服务 URL。</span><span class="sxs-lookup"><span data-stu-id="50075-237">If you have the WCF Service in the same solution as we have in this example, you can select the **Discover** option instead of specifying a service URL.</span></span>

![“配置 WCF Web Service Reference”窗口的屏幕截图](./media/example-migration-core/configure-wcf-reference.png)

<span data-ttu-id="50075-239">找到该服务后，该工具将反映该服务实现的 API 协定。</span><span class="sxs-lookup"><span data-stu-id="50075-239">Once the service is located, the tool reflects the API contract implemented by the service.</span></span> <span data-ttu-id="50075-240">将命名空间的名称更改为 `eShopServiceReference`，如下图所示：</span><span class="sxs-lookup"><span data-stu-id="50075-240">Change the name of the namespace to be `eShopServiceReference` as shown in the following image:</span></span>

![API 协定和更改后的命名空间的屏幕截图](./media/example-migration-core/api-contract-namespace.png)

<span data-ttu-id="50075-242">选择“完成”按钮。</span><span class="sxs-lookup"><span data-stu-id="50075-242">Select the **Finish** button.</span></span> <span data-ttu-id="50075-243">一段时间后，你将看到生成的代码。</span><span class="sxs-lookup"><span data-stu-id="50075-243">After a while, you'll see the generated code.</span></span>

<span data-ttu-id="50075-244">应会显示三个自动生成的文件：</span><span class="sxs-lookup"><span data-stu-id="50075-244">You should see three autogenerated files:</span></span>

1. <span data-ttu-id="50075-245">Getting Started：一个指向 GitHub 的链接，提供某些有关 WCF 的信息。</span><span class="sxs-lookup"><span data-stu-id="50075-245">*Getting Started*: a link to GitHub to provide some information on WCF.</span></span>
2. <span data-ttu-id="50075-246">ConnectedService.json：用于连接到服务的配置参数。</span><span class="sxs-lookup"><span data-stu-id="50075-246">*ConnectedService.json*: configuration parameters to connect to the service.</span></span>
3. <span data-ttu-id="50075-247">Reference.cs：实际 WCF 客户端代码。</span><span class="sxs-lookup"><span data-stu-id="50075-247">*Reference.cs*: the actual WCF client code.</span></span>

![包含三个自动生成的文件的“解决方案资源管理器”窗口的屏幕截图](./media/example-migration-core/autogenerated-files.png)

<span data-ttu-id="50075-249">如果再次编译，你会看到许多来自 Helper 文件夹内的 .cs 文件的错误 。</span><span class="sxs-lookup"><span data-stu-id="50075-249">If you compile again, you'll see many errors coming from *.cs* files inside the *Helper* folder.</span></span> <span data-ttu-id="50075-250">此文件夹存在于 .NET Framework 版本中，但不包含在旧 .csproj 中。</span><span class="sxs-lookup"><span data-stu-id="50075-250">This folder was present in the .NET Framework version but not included in the old *.csproj*.</span></span> <span data-ttu-id="50075-251">但对于新的 SDK 样式项目，默认包括项目文件位置下存在的每个代码文件。</span><span class="sxs-lookup"><span data-stu-id="50075-251">But with the new SDK-style project, every code file present underneath the project file location is included by default.</span></span> <span data-ttu-id="50075-252">也就是说，新的 .NET Core 项目会尝试编译 Helper 文件夹内的文件。</span><span class="sxs-lookup"><span data-stu-id="50075-252">That is, the new .NET Core project tries to compile the files inside the *Helper* folder.</span></span> <span data-ttu-id="50075-253">由于不需要该文件夹，因此你可安全地将其删除。</span><span class="sxs-lookup"><span data-stu-id="50075-253">Since that folder isn't needed, you can safely delete it.</span></span>

<span data-ttu-id="50075-254">如果再次编译该项目并执行它，你将看不到产品图像。</span><span class="sxs-lookup"><span data-stu-id="50075-254">If you compile the project again and execute it, you won't see the product images.</span></span> <span data-ttu-id="50075-255">问题在于，文件的路径现在略有变化。</span><span class="sxs-lookup"><span data-stu-id="50075-255">The problem is that now the path to the files has slightly changed.</span></span> <span data-ttu-id="50075-256">若要解决此问题，需要在路径中添加添加另一个深度级别，更新文件 `CatalogView.cs` 中的以下行：</span><span class="sxs-lookup"><span data-stu-id="50075-256">To fix this issue, you need to add another level of depth in the path, updating in the file `CatalogView.cs` the line:</span></span>

```csharp
string image_name = Environment.CurrentDirectory + "\\..\\..\\Assets\\Images\\Catalog\\" + catalogItems.Picturefilename;
```

<span data-ttu-id="50075-257">to</span><span class="sxs-lookup"><span data-stu-id="50075-257">to</span></span>

```csharp
string image_name = Environment.CurrentDirectory + "\\..\\..\\..\\Assets\\Images\\Catalog\\" + catalogItems.Picturefilename;
```

<span data-ttu-id="50075-258">在此更改后，可检查该应用程序是否在 .NET Core 上按预期启动并运行。</span><span class="sxs-lookup"><span data-stu-id="50075-258">After this change, you can check that the application launches and runs as expected on .NET Core.</span></span>

## <a name="migrating-a-wpf-application"></a><span data-ttu-id="50075-259">迁移 WPF 应用程序</span><span class="sxs-lookup"><span data-stu-id="50075-259">Migrating a WPF application</span></span>

<span data-ttu-id="50075-260">使用 `Shop.ClassicWPF` 示例应用程序来执行迁移。</span><span class="sxs-lookup"><span data-stu-id="50075-260">We'll use the `Shop.ClassicWPF` sample application to perform the migration.</span></span> <span data-ttu-id="50075-261">下图显示了迁移之前的应用的屏幕截图：</span><span class="sxs-lookup"><span data-stu-id="50075-261">The following image shows a screenshot of the app before migration:</span></span>

![迁移之前的示例应用](./media/example-migration-core/app-before-migration.png)

<span data-ttu-id="50075-263">此应用程序使用本地 SQL Server Express 数据库来保存产品目录信息。</span><span class="sxs-lookup"><span data-stu-id="50075-263">This application uses a local SQL Server Express database to hold the product catalog information.</span></span> <span data-ttu-id="50075-264">可直接从 WPF 应用程序访问此数据库。</span><span class="sxs-lookup"><span data-stu-id="50075-264">This database is accessed directly from the WPF application.</span></span>

<span data-ttu-id="50075-265">首先，必须将 .csproj 文件更新为 .NET Core 应用程序使用的新 SDK 样式。</span><span class="sxs-lookup"><span data-stu-id="50075-265">First, you must update the *.csproj* file to the new SDK style used by .NET Core applications.</span></span> <span data-ttu-id="50075-266">按照 Windows 窗体迁移中所述的相同步骤进行操作：卸载项目、打开 .csproj 文件、更新其内容并重载该项目。</span><span class="sxs-lookup"><span data-stu-id="50075-266">You'll follow the same steps described in the Windows Forms migration: you'll unload the project, open the *.csproj* file, update its contents, and reload the project.</span></span>

<span data-ttu-id="50075-267">在本例中，请删除 .csproj 文件的所有内容，并将其替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="50075-267">In this case, delete all the content of the *.csproj* file and replace it with the following code:</span></span>

```xml
 <Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net5.0-windows</TargetFramework>
    <UseWpf>true</UseWpf>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="50075-268">如果重载并编译该项目，你会收到以下错误：</span><span class="sxs-lookup"><span data-stu-id="50075-268">If you reload the project and compile it, you'll get the following error:</span></span>

![Visual Studio 中显示单个错误 CS0234 的错误列表](./media/example-migration-core/wpf-compilation-error.png)

<span data-ttu-id="50075-270">由于已删除所有 .csproj 内容，因此你丢失了旧项目中的项目引用规范。</span><span class="sxs-lookup"><span data-stu-id="50075-270">Since you've deleted all the *.csproj* contents, you've lost a project reference specification present in the old project.</span></span> <span data-ttu-id="50075-271">只需将此行添加到 .csproj 文件，即可再次包含该项目引用：</span><span class="sxs-lookup"><span data-stu-id="50075-271">You just need to add this line to the *.csproj* file to include the project reference back:</span></span>

```xml
<ItemGroup>
    <ProjectReference Include="..\\eShop.SqlProvider\\eShop.SqlProvider.csproj" />
<ItemGroup>
```

<span data-ttu-id="50075-272">你也可通过右键单击“依赖项”节点并选择“添加项目引用”，让 Visual Studio 为你提供帮助 。</span><span class="sxs-lookup"><span data-stu-id="50075-272">You can also let Visual Studio help you by right-clicking on the **Dependencies** node and selecting **Add Project Reference**.</span></span> <span data-ttu-id="50075-273">从解决方案中选择该项目，然后单击“确定”：</span><span class="sxs-lookup"><span data-stu-id="50075-273">Select the project from the solution and click **OK**:</span></span>

![已选中 eShop.SqlProvider 项目的“引用管理器”对话框的屏幕截图](./media/example-migration-core/reference-manager.png)

<span data-ttu-id="50075-275">添加缺少的项目引用后，应用程序将在 .NET 上按预期方式编译和运行。</span><span class="sxs-lookup"><span data-stu-id="50075-275">Once you add the missing project reference, the application compiles and runs as expected on .NET.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="50075-276">[上一页](windows-migration.md)
>[下一页](deploy-modern-applications.md)</span><span class="sxs-lookup"><span data-stu-id="50075-276">[Previous](windows-migration.md)
[Next](deploy-modern-applications.md)</span></span>
