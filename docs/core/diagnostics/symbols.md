---
title: .NET 中的符号
description: .NET 中的符号和可移植 PDB 简介
ms.date: 02/08/2021
ms.openlocfilehash: 8f217bf8b62ff12a6ea1dc6a5b14b34d8037dd2d
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759893"
---
# <a name="symbols"></a><span data-ttu-id="3b74a-103">符号</span><span class="sxs-lookup"><span data-stu-id="3b74a-103">Symbols</span></span>

<span data-ttu-id="3b74a-104">符号可用于调试和其他诊断工具。</span><span class="sxs-lookup"><span data-stu-id="3b74a-104">Symbols are useful for debugging and other diagnostic tools.</span></span> <span data-ttu-id="3b74a-105">符号文件的内容在语言、编译器和平台之间各有不同。</span><span class="sxs-lookup"><span data-stu-id="3b74a-105">The contents of symbol files vary between languages, compilers, and platforms.</span></span> <span data-ttu-id="3b74a-106">以非常概要的角度来看，符号是源代码和编译器生成的二进制文件之间的映射。</span><span class="sxs-lookup"><span data-stu-id="3b74a-106">At a very high level symbols are a mapping between the source code and the binary produced by the compiler.</span></span> <span data-ttu-id="3b74a-107">[Visual Studio](/visualstudio/debugger/what-is-debugging) 和 [Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging) 等工具会使用这些映射来解析源行号信息或本地变量名称。</span><span class="sxs-lookup"><span data-stu-id="3b74a-107">These mappings are used by tools like [Visual Studio](/visualstudio/debugger/what-is-debugging) and [Visual Studio Code](https://code.visualstudio.com/Docs/editor/debugging) to resolve source line number information or local variable names.</span></span>

<span data-ttu-id="3b74a-108">可在[有关符号的 Windows 文档](/windows/win32/dxtecharts/debugging-with-symbols)中更详细地了解适用于 Windows 的符号，不过其中很多概念也不用于其他平台。</span><span class="sxs-lookup"><span data-stu-id="3b74a-108">The [Windows documentation on symbols](/windows/win32/dxtecharts/debugging-with-symbols) contain more detailed information on symbols for Windows, although many of the concepts apply to other platforms as well.</span></span>

## <a name="learn-about-nets-portable-pdb-format"></a><span data-ttu-id="3b74a-109">了解 .NET 的可移植 PDB 格式</span><span class="sxs-lookup"><span data-stu-id="3b74a-109">Learn about .NET's Portable PDB format</span></span>

<span data-ttu-id="3b74a-110">.NET Core 引入了一种新的符号文件 (PDB) 格式，即可移植 PDB。</span><span class="sxs-lookup"><span data-stu-id="3b74a-110">.NET Core introduces a new symbol file (PDB) format - the portable PDB.</span></span> <span data-ttu-id="3b74a-111">与仅限 Windows 的传统 PDB 不同，可在任意平台上创建和读取可移植 PDB。</span><span class="sxs-lookup"><span data-stu-id="3b74a-111">Unlike traditional PDBs which are Windows-only, portable PDBs can be created and read on all platforms.</span></span>

### <a name="what-is-a-pdb"></a><span data-ttu-id="3b74a-112">什么是 PDB？</span><span class="sxs-lookup"><span data-stu-id="3b74a-112">What is a PDB?</span></span>

<span data-ttu-id="3b74a-113">PDB 文件是编译器生成的辅助文件，目的是向其他工具（尤其是调试程序）提供主可执行文件中的内容及其生成方式的相关信息。</span><span class="sxs-lookup"><span data-stu-id="3b74a-113">A PDB file is an auxiliary file produced by a compiler to provide other tools, especially debuggers, information about what is in the main executable file and how it was produced.</span></span> <span data-ttu-id="3b74a-114">例如，调试程序读取 PDB 来将 foo.cs 第 12 行映射到适当的可执行文件位置，以便它可设置断点。</span><span class="sxs-lookup"><span data-stu-id="3b74a-114">For example, a debugger reads a PDB to map foo.cs line 12 to the right executable location so that it can set a breakpoint.</span></span> <span data-ttu-id="3b74a-115">Windows PDB 格式已存在很长时间，它是从甚至更久远的其他本机调试符号格式演变而来的。</span><span class="sxs-lookup"><span data-stu-id="3b74a-115">The Windows PDB format has been around a long time, and it evolved from other native debugging symbol formats which were even older.</span></span> <span data-ttu-id="3b74a-116">它最初是用作本机 (C/C++) 程序的一种格式。</span><span class="sxs-lookup"><span data-stu-id="3b74a-116">It started out its life as a format for native (C/C++) programs.</span></span> <span data-ttu-id="3b74a-117">针对 .NET Framework 的首次发布，Windows PDB 格式进行了扩展以支持 .NET。</span><span class="sxs-lookup"><span data-stu-id="3b74a-117">For the first release of the .NET Framework, the Windows PDB format was extended to support .NET.</span></span>

## <a name="use-the-correct-pdb-format-for-your-scenario"></a><span data-ttu-id="3b74a-118">使用适合你的方案的 PDB 格式</span><span class="sxs-lookup"><span data-stu-id="3b74a-118">Use the correct PDB format for your scenario</span></span>

<span data-ttu-id="3b74a-119">任何位置都不支持可移植 PDB 和 Windows PDB，因此你需要思考要在哪里使用和调试你的项目来确定要使用的具体格式。</span><span class="sxs-lookup"><span data-stu-id="3b74a-119">Neither portable PDBs nor Windows PDBs are supported everywhere, so you need to consider where your project will want to be used and debugged to decide which format to use.</span></span> <span data-ttu-id="3b74a-120">如果你有一个项目，而你希望它能在这两种格式中使用和调试，那么可使用不同的生成配置，并生成项目两次以同时支持这两种类型的使用者。</span><span class="sxs-lookup"><span data-stu-id="3b74a-120">If you have a project that you want to be able to use and debug in both formats, you can use different build configurations and build the project twice to support both types of consumer.</span></span>

### <a name="support-for-portable-pdbs"></a><span data-ttu-id="3b74a-121">支持可移植 PDB</span><span class="sxs-lookup"><span data-stu-id="3b74a-121">Support for Portable PDBs</span></span>

<span data-ttu-id="3b74a-122">可在任意操作系统上读取可移植 PDB，建议对托管代码使用这种符号格式，但存在一些旧版工具和应用程序不支持这种格式：</span><span class="sxs-lookup"><span data-stu-id="3b74a-122">Portable PDBs can be read on any operating systems and is the recommended symbol format for managed code, but there are a number of legacy tools and applications where they aren't supported:</span></span>

* <span data-ttu-id="3b74a-123">面向 .NET Framework 4.7.1 或更低版本的应用程序：将带有映射的堆栈跟踪打印回行号（例如在 ASP.NET 错误页面中）。</span><span class="sxs-lookup"><span data-stu-id="3b74a-123">Applications targeting .NET Framework 4.7.1 or earlier: printing stack traces with mappings back to line numbers (such as in an ASP.NET error page).</span></span> <span data-ttu-id="3b74a-124">方法的名称不受影响，只有源文件名和行号不受支持。</span><span class="sxs-lookup"><span data-stu-id="3b74a-124">The name of methods is unaffected, only the source file names and line numbers are unsupported.</span></span>

* <span data-ttu-id="3b74a-125">使用 .NET 反编译程序（例如 ildasm 或 .NET 反射器），且预期看到源行映射或本地参数名称。</span><span class="sxs-lookup"><span data-stu-id="3b74a-125">Using .NET decompilers such as ildasm or .NET reflector and expecting to see source line mappings or local parameter names.</span></span>

* <span data-ttu-id="3b74a-126">最新版本的 [DIA](/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk) 和工具用它来读取符号（例如 WinDBG 支持可移植 PDB），但更低的版本不这样做。</span><span class="sxs-lookup"><span data-stu-id="3b74a-126">The latest versions of [DIA](/visualstudio/debugger/debug-interface-access/debug-interface-access-sdk) and tools using it for reading symbols, such as WinDBG support Portable PDBs, but older version do not.</span></span>

* <span data-ttu-id="3b74a-127">可能存在不支持可移植 PDB 的更低版本的探查器。</span><span class="sxs-lookup"><span data-stu-id="3b74a-127">There may be older versions of profilers that do not support portable PDBs.</span></span>

<span data-ttu-id="3b74a-128">若要在不支持可移植 PDB 的工具上使用这些格式，可尝试使用 [Pdb2Pdb](https://github.com/dotnet/symreader-converter#pdb2pdb)，它会在可移植 PDB 和 Windows PDB 之间进行转换。</span><span class="sxs-lookup"><span data-stu-id="3b74a-128">To use portable PDBs on tools that do not support them, you can try using [Pdb2Pdb](https://github.com/dotnet/symreader-converter#pdb2pdb) which converts between Portable PDBs and Windows PDBs.</span></span>

### <a name="support-for-windows-pdbs"></a><span data-ttu-id="3b74a-129">支持 Windows PDB</span><span class="sxs-lookup"><span data-stu-id="3b74a-129">Support for Windows PDBs</span></span>

<span data-ttu-id="3b74a-130">仅可在 Windows 上编写或读取 Windows PDB。</span><span class="sxs-lookup"><span data-stu-id="3b74a-130">Windows PDBs can only be written or read on Windows.</span></span> <span data-ttu-id="3b74a-131">对托管代码使用 Windows PDB 的操作已过时，且只有旧版工具需要此操作。</span><span class="sxs-lookup"><span data-stu-id="3b74a-131">Using Windows PDBs for managed code is obsolete and is only needed for legacy tools.</span></span> <span data-ttu-id="3b74a-132">建议使用可移植 PDB 而不是 Windows PDB，原因是一些更新的编译器功能仅支持可移植 PDB。</span><span class="sxs-lookup"><span data-stu-id="3b74a-132">It is recommended that you use portable PDBs instead of Windows PDBs as some newer compiler features that are implemented for only portable PDBs.</span></span>

## <a name="see-also"></a><span data-ttu-id="3b74a-133">另请参阅</span><span class="sxs-lookup"><span data-stu-id="3b74a-133">See also</span></span>

* <span data-ttu-id="3b74a-134">[dotnet-symbol](./dotnet-symbol.md) 可用于下载框架二进制文件的符号文件</span><span class="sxs-lookup"><span data-stu-id="3b74a-134">[dotnet-symbol](./dotnet-symbol.md) can be used to download symbol files for framework binaries</span></span>

* [<span data-ttu-id="3b74a-135">有关符号的 Windows 文档</span><span class="sxs-lookup"><span data-stu-id="3b74a-135">Windows documentation on symbols</span></span>](/windows/win32/dxtecharts/debugging-with-symbols)
