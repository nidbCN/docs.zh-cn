---
description: C# 编译器选项。 了解控制 C# 编译器行为的选项。
title: C# 编译器选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- compiler options [C#]
- csc.exe
- cl.exe compiler, C# options
- Visual C# compiler
- Visual C#, compiler options
ms.assetid: d3403556-1816-4546-a782-e8223a772e44
ms.openlocfilehash: cbe4db51652e8bfd00c555b6ddd230e124a08360
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478479"
---
# <a name="c-compiler-options"></a><span data-ttu-id="f4306-104">C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="f4306-104">C# Compiler Options</span></span>

<span data-ttu-id="f4306-105">本节介绍 C# 编译器解释的选项。</span><span class="sxs-lookup"><span data-stu-id="f4306-105">This section describes the options interpreted by the C# compiler.</span></span> <span data-ttu-id="f4306-106">可以通过两种不同的方法设置 .NET 项目中的编译器选项：</span><span class="sxs-lookup"><span data-stu-id="f4306-106">There are two different ways to set compiler options in .NET projects:</span></span>

- <span data-ttu-id="f4306-107">在 \*.csproj 文件中指定选项：可以为 \*.csproj 文件中的任何编译器选项添加 XML 元素。</span><span class="sxs-lookup"><span data-stu-id="f4306-107">***Specify option in your \*.csproj file***: You can add XML elements for any compiler option in your *\*.csproj* file.</span></span> <span data-ttu-id="f4306-108">元素名称与编译器选项相同。</span><span class="sxs-lookup"><span data-stu-id="f4306-108">The element name is the same as the compiler option.</span></span> <span data-ttu-id="f4306-109">用 XML 元素的值设置编译器选项的值。</span><span class="sxs-lookup"><span data-stu-id="f4306-109">The value of the XML element sets the value of the compiler option.</span></span> <span data-ttu-id="f4306-110">有关项目文件中设置选项的详细信息，请参阅[适用于 .NET SDK 项目的 MSBuild 属性](../../../core/project-sdk/msbuild-props.md)一文。</span><span class="sxs-lookup"><span data-stu-id="f4306-110">For more information on setting options in project files, see the article [MSBuild properties for .NET SDK Projects](../../../core/project-sdk/msbuild-props.md).</span></span>
- <span data-ttu-id="f4306-111">使用 Visual Studio 属性页：Visual Studio 提供了属性页，可用于编辑生成属性。</span><span class="sxs-lookup"><span data-stu-id="f4306-111">***Using the Visual Studio Property pages***: Visual Studio provides property pages to edit build properties.</span></span> <span data-ttu-id="f4306-112">若要了解有关详细信息，请参阅[管理项目和解决方案属性 - Windows](/visualstudio/ide/managing-project-and-solution-properties#c-visual-basic-and-f-projects) 或[管理项目和解决方案属性 - Mac](/visualstudio/mac/managing-solutions-and-project-properties)。</span><span class="sxs-lookup"><span data-stu-id="f4306-112">To learn more about them, see [Manage project and solution properties - Windows](/visualstudio/ide/managing-project-and-solution-properties#c-visual-basic-and-f-projects) or [Manage project and solution properties - Mac](/visualstudio/mac/managing-solutions-and-project-properties).</span></span>

## <a name="net-framework-projects"></a><span data-ttu-id="f4306-113">.NET Framework 项目</span><span class="sxs-lookup"><span data-stu-id="f4306-113">.NET Framework projects</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f4306-114">本部分仅适用于 .NET Framework 项目。</span><span class="sxs-lookup"><span data-stu-id="f4306-114">This section applies to .NET Framework projects only.</span></span>

<span data-ttu-id="f4306-115">除上述机制以外，还可以使用两种附加方法为 .NET Framework 项目设置编译器选项：</span><span class="sxs-lookup"><span data-stu-id="f4306-115">In addition to the mechanisms described above, you can set compiler options using two additional methods for .NET Framework projects:</span></span>

- <span data-ttu-id="f4306-116">.NET Framework 项目的命令行参数：.NET Framework 项目使用 csc.exe 而不是 `dotnet build` 生成项目。</span><span class="sxs-lookup"><span data-stu-id="f4306-116">**Command line arguments for .NET Framework projects**: .NET Framework projects use *csc.exe* instead of `dotnet build` to build projects.</span></span> <span data-ttu-id="f4306-117">可以为 .NET Framework 项目指定 csc.exe 的命令行参数。</span><span class="sxs-lookup"><span data-stu-id="f4306-117">You can specify command line arguments to *csc.exe* for .NET Framework projects.</span></span>
- <span data-ttu-id="f4306-118">已编译的 ASP.NET 页面：.NET Framework 项目使用 web.config 文件的一部分来编译页面。</span><span class="sxs-lookup"><span data-stu-id="f4306-118">**Compiled ASP.NET pages**: .NET Framework projects use a section of the *web.config* file for compiling pages.</span></span> <span data-ttu-id="f4306-119">对于新的生成系统和 ASP.NET Core 项目，将从项目文件中设置选项。</span><span class="sxs-lookup"><span data-stu-id="f4306-119">For the new build system, and ASP.NET Core projects, options are taken from the project file.</span></span>

<span data-ttu-id="f4306-120">某些编译器选项的单词从 csc.exe 和 .NET Framework 项目更改为新的 MSBuild 系统。</span><span class="sxs-lookup"><span data-stu-id="f4306-120">The word for some compiler options changed from *csc.exe* and .NET Framework projects to the new MSBuild system.</span></span> <span data-ttu-id="f4306-121">本部分使用的是新语法。</span><span class="sxs-lookup"><span data-stu-id="f4306-121">The new syntax is used throughout this section.</span></span> <span data-ttu-id="f4306-122">每个页面顶部同时列出了这两个版本。</span><span class="sxs-lookup"><span data-stu-id="f4306-122">Both versions are listed at the top of each page.</span></span> <span data-ttu-id="f4306-123">对于 csc.exe，所有参数都会在选项和冒号后列出。</span><span class="sxs-lookup"><span data-stu-id="f4306-123">For *csc.exe*, any arguments are listed following the option and a colon.</span></span> <span data-ttu-id="f4306-124">例如，`-doc` 选项将为：</span><span class="sxs-lookup"><span data-stu-id="f4306-124">For example, the `-doc` option would be:</span></span>

```console
-doc:DocFile.xml
```

<span data-ttu-id="f4306-125">通过在命令提示符处键入 C# 编译器的可执行文件名称 (csc.exe)，可调用该编译器。</span><span class="sxs-lookup"><span data-stu-id="f4306-125">You can invoke the C# compiler by typing the name of its executable file (*csc.exe*) at a command prompt.</span></span>

<span data-ttu-id="f4306-126">对于 .NET Framework 项目，还可以从命令行运行 csc.exe。</span><span class="sxs-lookup"><span data-stu-id="f4306-126">For .NET Framework projects, you can also run *csc.exe* from the command line.</span></span> <span data-ttu-id="f4306-127">每个编译器选项均有两种形式： **-option** 和 **/option**。</span><span class="sxs-lookup"><span data-stu-id="f4306-127">Every compiler option is available in two forms: **-option** and **/option**.</span></span> <span data-ttu-id="f4306-128">在 .NET Framework Web 项目中，在 web.config 文件中指定用于编译代码隐藏的选项。</span><span class="sxs-lookup"><span data-stu-id="f4306-128">In .NET Framework web projects, you specify options for compiling code-behind in the *web.config* file.</span></span> <span data-ttu-id="f4306-129">有关详细信息，请参阅 [\<compiler> 元素](../../../framework/configure-apps/file-schema/compiler/compiler-element.md)。</span><span class="sxs-lookup"><span data-stu-id="f4306-129">For more information, see [\<compiler> Element](../../../framework/configure-apps/file-schema/compiler/compiler-element.md).</span></span>

<span data-ttu-id="f4306-130">如果使用“Visual Studio 开发人员命令提示”窗口，系统将设置所有必需的环境变量。</span><span class="sxs-lookup"><span data-stu-id="f4306-130">If you use the **Developer Command Prompt for Visual Studio** window, all the necessary environment variables are set for you.</span></span> <span data-ttu-id="f4306-131">有关如何访问此工具的信息，请参阅 [Visual Studio 开发人员命令提示](../../../framework/tools/developer-command-prompt-for-vs.md)。</span><span class="sxs-lookup"><span data-stu-id="f4306-131">For information on how to access this tool, see [Developer Command Prompt for Visual Studio](../../../framework/tools/developer-command-prompt-for-vs.md).</span></span>

<span data-ttu-id="f4306-132">csc.exe 可执行文件通常位于 Windows 目录下的 Microsoft.NET\Framework\\ *\<Version>* 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="f4306-132">The *csc.exe* executable file is usually located in the Microsoft.NET\Framework\\*\<Version>* folder under the *Windows* directory.</span></span> <span data-ttu-id="f4306-133">根据每台计算机上的具体配置，此位置可能有所不同。</span><span class="sxs-lookup"><span data-stu-id="f4306-133">Its location might vary depending on the exact configuration of a particular computer.</span></span> <span data-ttu-id="f4306-134">如果计算机上安装了不止一个版本的 .NET Framework，你将发现此文件的多个版本。</span><span class="sxs-lookup"><span data-stu-id="f4306-134">If more than one version of .NET Framework is installed on your computer, you'll find multiple versions of this file.</span></span> <span data-ttu-id="f4306-135">有关此类安装的详细信息，请参阅[如何：确定安装的 .NET Framework 版本](../../../framework/migration-guide/how-to-determine-which-versions-are-installed.md)。</span><span class="sxs-lookup"><span data-stu-id="f4306-135">For more information about such installations, see [How to: determine which versions of the .NET Framework are installed](../../../framework/migration-guide/how-to-determine-which-versions-are-installed.md).</span></span>
