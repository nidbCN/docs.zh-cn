---
title: C# 语言版本控制 - C# 指南
description: 了解如何根据项目确定 C# 语言版本，以及背后的原因。 了解如何手动重写默认值。
ms.custom: updateeachrelease
ms.date: 08/11/2020
ms.openlocfilehash: e1b5848f5cd589b0ea61518f0b19efb8fe801337
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477460"
---
# <a name="c-language-versioning"></a><span data-ttu-id="f3ef2-104">C# 语言版本控制</span><span class="sxs-lookup"><span data-stu-id="f3ef2-104">C# language versioning</span></span>

<span data-ttu-id="f3ef2-105">最新的 C# 编译器根据项目的一个或多个目标框架确定默认语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-105">The latest C# compiler determines a default language version based on your project's target framework or frameworks.</span></span> <span data-ttu-id="f3ef2-106">Visual Studio 不提供用于更改值的 UI，但可以通过编辑 .csproj 文件来更改值。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-106">Visual Studio doesn't provide a UI to change the value, but you can change it by editing the *csproj* file.</span></span> <span data-ttu-id="f3ef2-107">此默认选择可确保使用与目标框架兼容的最新语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-107">The choice of default ensures that you use the latest language version compatible with your target framework.</span></span> <span data-ttu-id="f3ef2-108">你将从访问与项目目标兼容的最新语言功能中受益。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-108">You benefit from access to the latest language features compatible with your project's target.</span></span> <span data-ttu-id="f3ef2-109">此默认选择还可确保不会使用需要类型或运行时行为在目标框架中不可用的语言。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-109">This default choice also ensures you don't use a language that requires types or runtime behavior not available in your target framework.</span></span> <span data-ttu-id="f3ef2-110">选择比默认版本更高的语言版本可能导致难以诊断编译时和运行时错误。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-110">Choosing a language version newer than the default can cause hard to diagnose compile-time and runtime errors.</span></span>

<span data-ttu-id="f3ef2-111">本文中的规则适用于随 Visual Studio 2019 或 .NET SDK 一起提供的编译器。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-111">The rules in this article apply to the compiler delivered with Visual Studio 2019 or the .NET SDK.</span></span> <span data-ttu-id="f3ef2-112">默认情况下，Visual Studio 2017 安装或早期 .NET Core SDK 版本中包含的 C# 编译器以 C# 7.0 为目标。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-112">The C# compilers that are part of the Visual Studio 2017 installation or earlier .NET Core SDK versions target C# 7.0 by default.</span></span>

<span data-ttu-id="f3ef2-113">C# 8.0 仅在 .NET Core 3.x 及更高版本上受支持。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-113">C# 8.0 is supported only on .NET Core 3.x and newer versions.</span></span> <span data-ttu-id="f3ef2-114">许多最新功能需要 .NET Core 3.x 中引入的库和运行时功能：</span><span class="sxs-lookup"><span data-stu-id="f3ef2-114">Many of the newest features require library and runtime features introduced in .NET Core 3.x:</span></span>

- <span data-ttu-id="f3ef2-115">[默认接口实现](../whats-new/csharp-8.md#default-interface-methods)需要使用 .NET Core 3.0 CLR 中的新功能。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-115">[Default interface implementation](../whats-new/csharp-8.md#default-interface-methods) requires new features in the .NET Core 3.0 CLR.</span></span>
- <span data-ttu-id="f3ef2-116">[异步流](../whats-new/csharp-8.md#asynchronous-streams)需要使用新类型 <xref:System.IAsyncDisposable?displayProperty=nameWithType>、<xref:System.Collections.Generic.IAsyncEnumerable%601?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.IAsyncEnumerator%601?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-116">[Async streams](../whats-new/csharp-8.md#asynchronous-streams) require the new types <xref:System.IAsyncDisposable?displayProperty=nameWithType>, <xref:System.Collections.Generic.IAsyncEnumerable%601?displayProperty=nameWithType>, and <xref:System.Collections.Generic.IAsyncEnumerator%601?displayProperty=nameWithType>.</span></span>
- <span data-ttu-id="f3ef2-117">[索引和范围](../whats-new/csharp-8.md#indices-and-ranges)需要使用新类型 <xref:System.Index?displayProperty=nameWithType> 和 <xref:System.Range?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-117">[Indices and ranges](../whats-new/csharp-8.md#indices-and-ranges) require the new types <xref:System.Index?displayProperty=nameWithType> and <xref:System.Range?displayProperty=nameWithType>.</span></span>
- <span data-ttu-id="f3ef2-118">[可为 null 的引用类型](../whats-new/csharp-8.md#nullable-reference-types)利用几个[特性](attributes/nullable-analysis.md)来提供更准确的警告。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-118">[Nullable reference types](../whats-new/csharp-8.md#nullable-reference-types) make use of several [attributes](attributes/nullable-analysis.md) to provide better warnings.</span></span> <span data-ttu-id="f3ef2-119">这些特性是在 .NET Core 3.0 中添加的。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-119">Those attributes were added in .NET Core 3.0.</span></span> <span data-ttu-id="f3ef2-120">其他目标框架并未使用这些特性中的任何一种进行批注。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-120">Other target frameworks haven't been annotated with any of these attributes.</span></span> <span data-ttu-id="f3ef2-121">这意味着可为 null 的警告可能无法准确反映潜在问题。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-121">That means nullable warnings may not accurately reflect potential issues.</span></span>

<span data-ttu-id="f3ef2-122">C# 9.0 仅在 .NET 5 及更高版本上受支持。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-122">C# 9.0 is supported only on .NET 5 and newer versions.</span></span>

## <a name="defaults"></a><span data-ttu-id="f3ef2-123">默认值</span><span class="sxs-lookup"><span data-stu-id="f3ef2-123">Defaults</span></span>

<span data-ttu-id="f3ef2-124">编译器根据以下规则确定默认值：</span><span class="sxs-lookup"><span data-stu-id="f3ef2-124">The compiler determines a default based on these rules:</span></span>

| <span data-ttu-id="f3ef2-125">目标框架</span><span class="sxs-lookup"><span data-stu-id="f3ef2-125">Target framework</span></span> | <span data-ttu-id="f3ef2-126">version</span><span class="sxs-lookup"><span data-stu-id="f3ef2-126">version</span></span> | <span data-ttu-id="f3ef2-127">C# 语言版本的默认值</span><span class="sxs-lookup"><span data-stu-id="f3ef2-127">C# language version default</span></span> |
|------------------|---------|-----------------------------|
| <span data-ttu-id="f3ef2-128">.NET</span><span class="sxs-lookup"><span data-stu-id="f3ef2-128">.NET</span></span>             | <span data-ttu-id="f3ef2-129">5.x</span><span class="sxs-lookup"><span data-stu-id="f3ef2-129">5.x</span></span>     | <span data-ttu-id="f3ef2-130">C# 9.0</span><span class="sxs-lookup"><span data-stu-id="f3ef2-130">C# 9.0</span></span>                      |
| <span data-ttu-id="f3ef2-131">.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3ef2-131">.NET Core</span></span>        | <span data-ttu-id="f3ef2-132">3.x</span><span class="sxs-lookup"><span data-stu-id="f3ef2-132">3.x</span></span>     | <span data-ttu-id="f3ef2-133">C# 8.0</span><span class="sxs-lookup"><span data-stu-id="f3ef2-133">C# 8.0</span></span>                      |
| <span data-ttu-id="f3ef2-134">.NET Core</span><span class="sxs-lookup"><span data-stu-id="f3ef2-134">.NET Core</span></span>        | <span data-ttu-id="f3ef2-135">2.x</span><span class="sxs-lookup"><span data-stu-id="f3ef2-135">2.x</span></span>     | <span data-ttu-id="f3ef2-136">C# 7.3</span><span class="sxs-lookup"><span data-stu-id="f3ef2-136">C# 7.3</span></span>                      |
| <span data-ttu-id="f3ef2-137">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="f3ef2-137">.NET Standard</span></span>    | <span data-ttu-id="f3ef2-138">2.1</span><span class="sxs-lookup"><span data-stu-id="f3ef2-138">2.1</span></span>     | <span data-ttu-id="f3ef2-139">C# 8.0</span><span class="sxs-lookup"><span data-stu-id="f3ef2-139">C# 8.0</span></span>                      |
| <span data-ttu-id="f3ef2-140">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="f3ef2-140">.NET Standard</span></span>    | <span data-ttu-id="f3ef2-141">2.0</span><span class="sxs-lookup"><span data-stu-id="f3ef2-141">2.0</span></span>     | <span data-ttu-id="f3ef2-142">C# 7.3</span><span class="sxs-lookup"><span data-stu-id="f3ef2-142">C# 7.3</span></span>                      |
| <span data-ttu-id="f3ef2-143">.NET Standard</span><span class="sxs-lookup"><span data-stu-id="f3ef2-143">.NET Standard</span></span>    | <span data-ttu-id="f3ef2-144">1.x</span><span class="sxs-lookup"><span data-stu-id="f3ef2-144">1.x</span></span>     | <span data-ttu-id="f3ef2-145">C# 7.3</span><span class="sxs-lookup"><span data-stu-id="f3ef2-145">C# 7.3</span></span>                      |
| <span data-ttu-id="f3ef2-146">.NET Framework</span><span class="sxs-lookup"><span data-stu-id="f3ef2-146">.NET Framework</span></span>   | <span data-ttu-id="f3ef2-147">全部</span><span class="sxs-lookup"><span data-stu-id="f3ef2-147">all</span></span>     | <span data-ttu-id="f3ef2-148">C# 7.3</span><span class="sxs-lookup"><span data-stu-id="f3ef2-148">C# 7.3</span></span>                      |

<span data-ttu-id="f3ef2-149">如果你的项目是以具有相应预览语言版本的预览框架为目标，那么使用的语言版本是预览语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-149">When your project targets a preview framework that has a corresponding preview language version, the language version used is the preview language version.</span></span> <span data-ttu-id="f3ef2-150">你可在任何环境中使用该预览版提供的最新功能，而不会影响面向已发布 .NET Core 版本的项目。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-150">You use the latest features with that preview in any environment, without affecting projects that target a released .NET Core version.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f3ef2-151">Visual Studio 2017 向其创建的所有项目文件添加了 `<LangVersion>latest</LangVersion>` 项。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-151">Visual Studio 2017 added a `<LangVersion>latest</LangVersion>` entry to any project files it created.</span></span> <span data-ttu-id="f3ef2-152">添加此项后，该项表示 C# 7.0。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-152">That meant *C# 7.0* when it was added.</span></span> <span data-ttu-id="f3ef2-153">但是，升级到 Visual Studio 2019 后，无论目标框架是什么，该项均表示最新发布的版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-153">However, once you upgrade to Visual Studio 2019, that means the latest released version, regardless of the target framework.</span></span> <span data-ttu-id="f3ef2-154">这些项目现在会[覆盖默认行为](#override-a-default)。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-154">These projects now [override the default behavior](#override-a-default).</span></span> <span data-ttu-id="f3ef2-155">你应编辑项目文件，并删除该节点。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-155">You should edit the project file and remove that node.</span></span> <span data-ttu-id="f3ef2-156">然后项目将使用建议用于目标框架的编译器版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-156">Then, your project will use the compiler version recommended for your target framework.</span></span>

## <a name="override-a-default"></a><span data-ttu-id="f3ef2-157">替代默认值</span><span class="sxs-lookup"><span data-stu-id="f3ef2-157">Override a default</span></span>

<span data-ttu-id="f3ef2-158">如果必须明确指定 C# 版本，可以通过以下几种方式实现：</span><span class="sxs-lookup"><span data-stu-id="f3ef2-158">If you must specify your C# version explicitly, you can do so in several ways:</span></span>

- <span data-ttu-id="f3ef2-159">手动编辑[项目文件](#edit-the-project-file)。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-159">Manually edit your [project file](#edit-the-project-file).</span></span>
- <span data-ttu-id="f3ef2-160">为[子目录中的多个项目](#configure-multiple-projects)设置语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-160">Set the language version [for multiple projects in a subdirectory](#configure-multiple-projects).</span></span>
- <span data-ttu-id="f3ef2-161">配置 [LangVersion 编译器选项](compiler-options/language.md#langversion)。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-161">Configure the [**LangVersion** compiler option](compiler-options/language.md#langversion).</span></span>

> [!TIP]
> <span data-ttu-id="f3ef2-162">若要了解当前使用的语言版本，请在代码中添加 `#error version`（区分大小写）。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-162">To know what language version you're currently using, put `#error version` (case sensitive) in your code.</span></span> <span data-ttu-id="f3ef2-163">这样做可使编译器报告编译器错误 CS8304，并显示一条消息，其中包含正在使用的编译器版本和当前选择的语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-163">This makes the compiler report a compiler error, CS8304, with a message containing the compiler version being used and the current selected language version.</span></span> <span data-ttu-id="f3ef2-164">有关详细信息，请参阅 [#error（C# 参考）](preprocessor-directives/preprocessor-error.md)。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-164">See [#error (C# Reference)](preprocessor-directives/preprocessor-error.md) for more information.</span></span>

### <a name="edit-the-project-file"></a><span data-ttu-id="f3ef2-165">编辑项目文件</span><span class="sxs-lookup"><span data-stu-id="f3ef2-165">Edit the project file</span></span>

<span data-ttu-id="f3ef2-166">可在项目文件中设置语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-166">You can set the language version in your project file.</span></span> <span data-ttu-id="f3ef2-167">例如，如果你明确希望访问预览功能，请添加如下元素：</span><span class="sxs-lookup"><span data-stu-id="f3ef2-167">For example, if you explicitly want access to preview features, add an element like this:</span></span>

```xml
<PropertyGroup>
   <LangVersion>preview</LangVersion>
</PropertyGroup>
```

<span data-ttu-id="f3ef2-168">值 `preview` 使用编译器支持的最新可用的预览 C# 语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-168">The value `preview` uses the latest available preview C# language version that your compiler supports.</span></span>

### <a name="configure-multiple-projects"></a><span data-ttu-id="f3ef2-169">配置多个项目</span><span class="sxs-lookup"><span data-stu-id="f3ef2-169">Configure multiple projects</span></span>

<span data-ttu-id="f3ef2-170">若要配置多个目录，可以创建包含 `<LangVersion>` 元素的 Directory.Build.props 文件。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-170">To configure multiple projects, you can create a **Directory.Build.props** file that contains the `<LangVersion>` element.</span></span> <span data-ttu-id="f3ef2-171">通常是在解决方案目录中完成这件事。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-171">You typically do that in your solution directory.</span></span> <span data-ttu-id="f3ef2-172">将以下内容添加到解决方案目录中的 Directory.Build.props 文件：</span><span class="sxs-lookup"><span data-stu-id="f3ef2-172">Add the following to a **Directory.Build.props** file in your solution directory:</span></span>

```xml
<Project>
 <PropertyGroup>
   <LangVersion>preview</LangVersion>
 </PropertyGroup>
</Project>
```

<span data-ttu-id="f3ef2-173">包含该文件的目录的所有子目录中的版本都将使用 C# 预览版。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-173">Builds in all subdirectories of the directory containing that file will use the preview C# version.</span></span> <span data-ttu-id="f3ef2-174">有关详细信息，请查看[自定义生成](/visualstudio/msbuild/customize-your-build)。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-174">For more information, see [Customize your build](/visualstudio/msbuild/customize-your-build).</span></span>

## <a name="c-language-version-reference"></a><span data-ttu-id="f3ef2-175">C# 语言版本引用</span><span class="sxs-lookup"><span data-stu-id="f3ef2-175">C# language version reference</span></span>

<span data-ttu-id="f3ef2-176">下表显示当前所有 C# 语言版本。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-176">The following table shows all current C# language versions.</span></span> <span data-ttu-id="f3ef2-177">如果编译器较旧，它可能不一定能识别每个值。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-177">Your compiler may not necessarily understand every value if it's older.</span></span> <span data-ttu-id="f3ef2-178">如果安装的是最新的 .NET SDK，则可以访问列出的所有内容。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-178">If you install the latest .NET SDK, then you have access to everything listed.</span></span>

[!INCLUDE [langversion-table](includes/langversion-table.md)]

> [!TIP]
> <span data-ttu-id="f3ef2-179">打开[开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)，并运行以下命令，查看计算机上可用的语言版本的列表。</span><span class="sxs-lookup"><span data-stu-id="f3ef2-179">Open a [developer command-line shell](/visualstudio/ide/reference/command-prompt-powershell), and run the following command to see the listing of language versions available on your machine.</span></span>
>
> ```CMD
> csc -langversion:?
> ```
>
> <span data-ttu-id="f3ef2-180">查询此类 [\*\*LangVersion](compiler-options/language.md#langversion) 编译选项时，会打印如下所示的内容：</span><span class="sxs-lookup"><span data-stu-id="f3ef2-180">Querying the [\*\*LangVersion](compiler-options/language.md#langversion) compile option like this prints something similar to the following:</span></span>
>
> ```CMD
> Supported language versions:
> default
> 1
> 2
> 3
> 4
> 5
> 6
> 7.0
> 7.1
> 7.2
> 7.3
> 8.0
> 9.0 (default)
> latestmajor
> preview
> latest
> ```
