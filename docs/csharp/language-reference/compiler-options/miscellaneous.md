---
description: 其他 C# 编译器选项。 这些选项为编译器提供常规选项。
title: 不适合其他类别的 C# 编译器选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- ResponseFiles compiler option [C#]
- NoLogo compiler option [C#]
- NoConfig compiler option [C#]
ms.openlocfilehash: ec7d9c3685c9acb5ca3cda28aca55abb26b836cf
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482410"
---
# <a name="miscellaneous-c-compiler-options"></a><span data-ttu-id="4589f-104">其他 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="4589f-104">Miscellaneous C# Compiler Options</span></span>

<span data-ttu-id="4589f-105">下面的选项控制其他编译器行为。</span><span class="sxs-lookup"><span data-stu-id="4589f-105">The following options control miscellaneous compiler behavior.</span></span> <span data-ttu-id="4589f-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="4589f-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="4589f-107">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="4589f-107">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="4589f-108">**ResponseFiles** / `-@`：读取响应文件以获取更多选项。</span><span class="sxs-lookup"><span data-stu-id="4589f-108">**ResponseFiles** / `-@`: Read response file for more options.</span></span>
- <span data-ttu-id="4589f-109">**NoLogo** / `-nologo`：禁止显示编译器版权消息。</span><span class="sxs-lookup"><span data-stu-id="4589f-109">**NoLogo** / `-nologo` : Suppress compiler copyright message.</span></span>
- <span data-ttu-id="4589f-110">**NoConfig** / `-noconfig`：不自动包括 CSC.RSP 文件。</span><span class="sxs-lookup"><span data-stu-id="4589f-110">**NoConfig** / `-noconfig`: Don't auto include *CSC.RSP* file.</span></span>

## <a name="responsefiles"></a><span data-ttu-id="4589f-111">ResponseFiles</span><span class="sxs-lookup"><span data-stu-id="4589f-111">ResponseFiles</span></span>

<span data-ttu-id="4589f-112">通过 ResponseFiles 选项，可以指定包含编译器选项和要编译的源代码文件的文件。</span><span class="sxs-lookup"><span data-stu-id="4589f-112">The **ResponseFiles** option lets you specify a file that contains compiler options and source code files to compile.</span></span>

```xml
<ResponseFiles>response_file</ResponseFiles>
```

<span data-ttu-id="4589f-113">`response_file` 指定一个文件来列出编译器选项或要编译的源代码文件。</span><span class="sxs-lookup"><span data-stu-id="4589f-113">The `response_file` specifies the file that lists compiler options or source code files to compile.</span></span> <span data-ttu-id="4589f-114">编译器选项和源代码文件将由编译器处理，就像它们在命令行中被指定一样。</span><span class="sxs-lookup"><span data-stu-id="4589f-114">The compiler options and source code files will be processed by the compiler as if they had been specified on the command line.</span></span> <span data-ttu-id="4589f-115">若要在一次编译中指定多个响应文件，请指定多个响应文件选项。</span><span class="sxs-lookup"><span data-stu-id="4589f-115">To specify more than one response file in a compilation, specify multiple response file options.</span></span> <span data-ttu-id="4589f-116">在响应文件中，多个编译器选项和源代码文件可以出现在同一行中。</span><span class="sxs-lookup"><span data-stu-id="4589f-116">In a response file, multiple compiler options and source code files can appear on one line.</span></span> <span data-ttu-id="4589f-117">单个编译器选项的指定必须出现在同一行中（不能跨行）。</span><span class="sxs-lookup"><span data-stu-id="4589f-117">A single compiler option specification must appear on one line (can't span multiple lines).</span></span> <span data-ttu-id="4589f-118">响应文件的注释可以 # 符号开始。</span><span class="sxs-lookup"><span data-stu-id="4589f-118">Response files can have comments that begin with the # symbol.</span></span> <span data-ttu-id="4589f-119">从响应文件内指定编译器选项就如同在命令行发出这些命令。</span><span class="sxs-lookup"><span data-stu-id="4589f-119">Specifying compiler options from within a response file is just like issuing those commands on the command line.</span></span> <span data-ttu-id="4589f-120">编译器在读取命令选项时会进行处理。</span><span class="sxs-lookup"><span data-stu-id="4589f-120">The compiler processes the command options as they're read.</span></span> <span data-ttu-id="4589f-121">命令行参数可以重写先前在响应文件中列出的选项。</span><span class="sxs-lookup"><span data-stu-id="4589f-121">Command-line arguments can override previously listed options in response files.</span></span> <span data-ttu-id="4589f-122">反之，响应文件中的选项也将重写先前在命令行或其他响应文件中列出的选项。</span><span class="sxs-lookup"><span data-stu-id="4589f-122">Conversely, options in a response file will override options listed previously on the command line or in other response files.</span></span> <span data-ttu-id="4589f-123">C# 提供 csc.rsp 文件，该文件与 csc.exe 文件位于同一目录。</span><span class="sxs-lookup"><span data-stu-id="4589f-123">C# provides the csc.rsp file, which is located in the same directory as the csc.exe file.</span></span> <span data-ttu-id="4589f-124">有关响应文件格式的详细信息，请参阅 [NoConfig](#noconfig)。</span><span class="sxs-lookup"><span data-stu-id="4589f-124">For more information about the response file format, see [**NoConfig**](#noconfig).</span></span> <span data-ttu-id="4589f-125">不能在 Visual Studio 开发环境中设置此编译器选项，也不能以编程方式对其进行更改。</span><span class="sxs-lookup"><span data-stu-id="4589f-125">This compiler option cannot be set in the Visual Studio development environment, nor can it be changed programmatically.</span></span> <span data-ttu-id="4589f-126">以下几行来自示例响应文件：</span><span class="sxs-lookup"><span data-stu-id="4589f-126">The following are a few lines from a sample response file:</span></span>

```console
# build the first output file
-target:exe -out:MyExe.exe source1.cs source2.cs
```

## <a name="nologo"></a><span data-ttu-id="4589f-127">NoLogo</span><span class="sxs-lookup"><span data-stu-id="4589f-127">NoLogo</span></span>

<span data-ttu-id="4589f-128">NoLogo 选项可在编译器启动时禁止显示登录版权标志，并在编译期间禁止显示信息性消息。</span><span class="sxs-lookup"><span data-stu-id="4589f-128">The **NoLogo** option suppresses display of the sign-on banner when the compiler starts up and display of informational messages during compiling.</span></span>

```xml
<NoLogo>true</NoLogo>
```

## <a name="noconfig"></a><span data-ttu-id="4589f-129">NoConfig</span><span class="sxs-lookup"><span data-stu-id="4589f-129">NoConfig</span></span>

<span data-ttu-id="4589f-130">NoConfig 选项会告知编译器不要使用 csc.rsp 文件进行编译。</span><span class="sxs-lookup"><span data-stu-id="4589f-130">The **NoConfig** option tells the compiler not to compile with the *csc.rsp* file.</span></span>

```xml
<NoConfig>true</NoConfig>
```

<span data-ttu-id="4589f-131">csc.rsp 文件引用 .NET Framework 随附的所有程序集。</span><span class="sxs-lookup"><span data-stu-id="4589f-131">The *csc.rsp* file references all the assemblies shipped with .NET Framework.</span></span> <span data-ttu-id="4589f-132">Visual Studio .NET 开发环境包括的实际引用取决于项目类型。</span><span class="sxs-lookup"><span data-stu-id="4589f-132">The actual references that the Visual Studio .NET development environment includes depend on the project type.</span></span> <span data-ttu-id="4589f-133">可以修改 csc.rsp 文件，并指定每次编译时应包括的其他编译器选项。</span><span class="sxs-lookup"><span data-stu-id="4589f-133">You can modify the *csc.rsp* file and specify additional compiler options that should be included in every compilation.</span></span> <span data-ttu-id="4589f-134">如果你不需要编译器查询和使用 csc.rsp 文件中的设置，请指定 NoConfig。</span><span class="sxs-lookup"><span data-stu-id="4589f-134">If you don't want the compiler to look for and use the settings in the *csc.rsp* file, specify **NoConfig**.</span></span> <span data-ttu-id="4589f-135">此编译器选项在 Visual Studio 中不可用，并且无法以编程方式更改。</span><span class="sxs-lookup"><span data-stu-id="4589f-135">This compiler option is unavailable in Visual Studio and cannot be changed programmatically.</span></span>
