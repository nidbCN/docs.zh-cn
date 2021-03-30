---
description: 用于控制代码生成的 C# 编译器选项。 这些选项会影响编译器为给定编译生成的代码。
title: 'C # 编译器选项 - 代码生成选项'
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- DebugType compiler option [C#]
- Optimize compiler option [C#]
- Deterministic compiler option [C#]
- ProduceOnlyReferenceAssembly compiler option [C#]
ms.openlocfilehash: a846bc515c501ec5a14069dd3b312b5e2df43d25
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881130"
---
# <a name="c-compiler-options-that-control-code-generation"></a><span data-ttu-id="4441f-104">控制代码生成的 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="4441f-104">C# Compiler Options that control code generation</span></span>

<span data-ttu-id="4441f-105">下面的选项控制编译器生成的代码。</span><span class="sxs-lookup"><span data-stu-id="4441f-105">The following options control code generation by the compiler.</span></span> <span data-ttu-id="4441f-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="4441f-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="4441f-107">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="4441f-107">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="4441f-108">**DebugType** / `-debug`：发出（或不发出）调试信息。</span><span class="sxs-lookup"><span data-stu-id="4441f-108">**DebugType** / `-debug`: Emit (or don't emit) debugging information.</span></span>
- <span data-ttu-id="4441f-109">**Optimize** / `-optimize`：启用优化。</span><span class="sxs-lookup"><span data-stu-id="4441f-109">**Optimize** / `-optimize`: Enable optimizations.</span></span>
- <span data-ttu-id="4441f-110">**Deterministic** / `-deterministic`：从相同的输入源生成每字节对等的输出。</span><span class="sxs-lookup"><span data-stu-id="4441f-110">**Deterministic** / `-deterministic`: Produce byte-for-byte equivalent output from the same input source.</span></span>
- <span data-ttu-id="4441f-111">**ProduceOnlyReferenceAssembly** / `-refonly`：生成引用程序集作为主输出，而非生成完整程序集。</span><span class="sxs-lookup"><span data-stu-id="4441f-111">**ProduceOnlyReferenceAssembly** / `-refonly`: Produce a reference assembly, instead of a full assembly, as the primary output.</span></span>

## <a name="debugtype"></a><span data-ttu-id="4441f-112">DebugType</span><span class="sxs-lookup"><span data-stu-id="4441f-112">DebugType</span></span>

<span data-ttu-id="4441f-113">DebugType 选项将使编译器生成调试信息，并将此信息放置在一个或多个输出文件中。</span><span class="sxs-lookup"><span data-stu-id="4441f-113">The **DebugType** option causes the compiler to generate debugging information and place it in the output file or files.</span></span> <span data-ttu-id="4441f-114">默认情况下，将为调试生成配置添加调试信息。</span><span class="sxs-lookup"><span data-stu-id="4441f-114">Debugging information is added by default for the *Debug* build configuration.</span></span> <span data-ttu-id="4441f-115">默认情况下，为发布生成配置禁用调试信息。</span><span class="sxs-lookup"><span data-stu-id="4441f-115">It is off by default for the *Release* build configuration.</span></span>

```xml
<DebugType>pdbonly</DebugType>
```

<span data-ttu-id="4441f-116">自 C# 6.0 起，对于所有编译器版本而言，pdbonly 与 full 之间没有任何区别。</span><span class="sxs-lookup"><span data-stu-id="4441f-116">For all compiler versions starting with C# 6.0, there is no difference between *pdbonly* and *full*.</span></span> <span data-ttu-id="4441f-117">请选择 pdbonly。</span><span class="sxs-lookup"><span data-stu-id="4441f-117">Choose *pdbonly*.</span></span> <span data-ttu-id="4441f-118">若要更改 .pdb 文件的位置，请参阅 [PdbFile](./advanced.md#pdbfile)。</span><span class="sxs-lookup"><span data-stu-id="4441f-118">To change the location of the *.pdb* file, see [**PdbFile**](./advanced.md#pdbfile).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="4441f-119">本节内容仅适用于 C# 6.0 以前的编译器。</span><span class="sxs-lookup"><span data-stu-id="4441f-119">This section applies only to compilers older than C# 6.0.</span></span>
> <span data-ttu-id="4441f-120">此元素的值可以是 `full` 或 `pdbonly`。</span><span class="sxs-lookup"><span data-stu-id="4441f-120">The value of this element can be either `full` or `pdbonly`.</span></span> <span data-ttu-id="4441f-121">full 参数（在不指定 pdbonly 时生效）允许将调试器附加到正在运行的程序。</span><span class="sxs-lookup"><span data-stu-id="4441f-121">The *full* argument, which is in effect if you don't specify *pdbonly*, enables attaching a debugger to the running program.</span></span> <span data-ttu-id="4441f-122">指定 pdbonly 后，可以在调试器中启动程序时进行源代码调试，但仅在正在运行的程序附加到调试器时才显示汇编程序。</span><span class="sxs-lookup"><span data-stu-id="4441f-122">Specifying *pdbonly* allows source code debugging when the program is started in the debugger but will only display assembler when the running program is attached to the debugger.</span></span> <span data-ttu-id="4441f-123">使用此选项创建调试版本。</span><span class="sxs-lookup"><span data-stu-id="4441f-123">Use this option to create debug builds.</span></span> <span data-ttu-id="4441f-124">如果使用 Full，请注意，对经过优化的 JIT 代码的速度和大小会存在一定影响，使用 full 时对代码质量的影响较小 。</span><span class="sxs-lookup"><span data-stu-id="4441f-124">If you use *Full*, be aware that there's some impact on the speed and size of JIT optimized code and a small impact on code quality with *full*.</span></span> <span data-ttu-id="4441f-125">建议使用 pdbonly 或不使用 PDB 生成发布代码。</span><span class="sxs-lookup"><span data-stu-id="4441f-125">We recommend *pdbonly* or no PDB for generating release code.</span></span> <span data-ttu-id="4441f-126">pdbonly 和 full 之间的一个区别在于，使用 full，编译器将发出 <xref:System.Diagnostics.DebuggableAttribute>，用于告知 JIT 编译器有可用调试信息  。</span><span class="sxs-lookup"><span data-stu-id="4441f-126">One difference between *pdbonly* and *full* is that with *full* the compiler emits a <xref:System.Diagnostics.DebuggableAttribute>, which is used to tell the JIT compiler that debug information is available.</span></span> <span data-ttu-id="4441f-127">因此，在使用 full 时，如果代码包含设置为 false 的 <xref:System.Diagnostics.DebuggableAttribute>，将出现错误。</span><span class="sxs-lookup"><span data-stu-id="4441f-127">Therefore, you will get an error if your code contains the <xref:System.Diagnostics.DebuggableAttribute> set to false if you use *full*.</span></span> <span data-ttu-id="4441f-128">有关如何配置应用程序的调试性能的详细信息，请参阅[令映像更易于调试](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md)。</span><span class="sxs-lookup"><span data-stu-id="4441f-128">For more information on how to configure the debug performance of an application, see [Making an Image Easier to Debug](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md).</span></span>

## <a name="optimize"></a><span data-ttu-id="4441f-129">优化</span><span class="sxs-lookup"><span data-stu-id="4441f-129">Optimize</span></span>

<span data-ttu-id="4441f-130">Optimize 选项启用或禁用编译器执行的优化，使输出文件更小、更快、更有效。</span><span class="sxs-lookup"><span data-stu-id="4441f-130">The **Optimize** option enables or disables optimizations performed by the compiler to make your output file smaller, faster, and more efficient.</span></span> <span data-ttu-id="4441f-131">默认情况下，为发布生成配置启用 Optimize 选项。</span><span class="sxs-lookup"><span data-stu-id="4441f-131">The *Optimize* option is enabled by default for a *Release* build configuration.</span></span> <span data-ttu-id="4441f-132">默认情况下，为调试生成配置禁用此选项。</span><span class="sxs-lookup"><span data-stu-id="4441f-132">It is off by default for a *Debug* build configuration.</span></span>

```xml
<Optimize>true</Optimize>
```

<span data-ttu-id="4441f-133">在 Visual Studio 中，可以从项目的“生成”属性页中设置 Optimize 选项。</span><span class="sxs-lookup"><span data-stu-id="4441f-133">You set the **Optimize** option from **Build** properties page for your project in Visual Studio.</span></span>

<span data-ttu-id="4441f-134">Optimize 还指示公共语言运行时在运行时优化代码。</span><span class="sxs-lookup"><span data-stu-id="4441f-134">**Optimize** also tells the common language runtime to optimize code at runtime.</span></span> <span data-ttu-id="4441f-135">默认情况下，禁用优化。</span><span class="sxs-lookup"><span data-stu-id="4441f-135">By default, optimizations are disabled.</span></span> <span data-ttu-id="4441f-136">指定 Optimize+ 可启用优化。</span><span class="sxs-lookup"><span data-stu-id="4441f-136">Specify **Optimize+** to enable optimizations.</span></span> <span data-ttu-id="4441f-137">生成程序集使用的模块时，请使用与程序集所使用的相同 Optimize 设置。</span><span class="sxs-lookup"><span data-stu-id="4441f-137">When building a module to be used by an assembly, use the same **Optimize** settings as used by the assembly.</span></span> <span data-ttu-id="4441f-138">可以将 Optimize 和 [Debug](#debugtype) 选项组合使用。</span><span class="sxs-lookup"><span data-stu-id="4441f-138">It's possible to combine the **Optimize** and [**Debug**](#debugtype) options.</span></span>

## <a name="deterministic"></a><span data-ttu-id="4441f-139">具有确定性</span><span class="sxs-lookup"><span data-stu-id="4441f-139">Deterministic</span></span>

<span data-ttu-id="4441f-140">如果输入相同，则会导致编译器生成的程序集其逐字节输出在整个编译期间中相同。</span><span class="sxs-lookup"><span data-stu-id="4441f-140">Causes the compiler to produce an assembly whose byte-for-byte output is identical across compilations for identical inputs.</span></span>

```xml
<Deterministic>true</Deterministic>
```

<span data-ttu-id="4441f-141">默认情况下，一组给定输入的编译器输出是唯一的，因为编译器会添加时间戳和随意数字生成的 MVID。</span><span class="sxs-lookup"><span data-stu-id="4441f-141">By default, compiler output from a given set of inputs is unique, since the compiler adds a timestamp and an MVID that is generated from random numbers.</span></span> <span data-ttu-id="4441f-142">使用 `<Deterministic>` 选项生成确定性的程序集，只要输入保持不变，该程序集的二进制内容在整个编译中都是相同的  。</span><span class="sxs-lookup"><span data-stu-id="4441f-142">You use the `<Deterministic>` option to produce a *deterministic assembly*, one whose binary content is identical across compilations as long as the input remains the same.</span></span> <span data-ttu-id="4441f-143">在此类生成中，时间戳和 MVID 字段会被替换为从所有编译输入的哈希派生的值。</span><span class="sxs-lookup"><span data-stu-id="4441f-143">In such a build, the timestamp and MVID fields will be replaced with values derived from a hash of all the compilation inputs.</span></span> <span data-ttu-id="4441f-144">编译器会考虑影响确定性的以下输入：</span><span class="sxs-lookup"><span data-stu-id="4441f-144">The compiler considers the following inputs that affect determinism:</span></span>

- <span data-ttu-id="4441f-145">命令行参数序列。</span><span class="sxs-lookup"><span data-stu-id="4441f-145">The sequence of command-line parameters.</span></span>
- <span data-ttu-id="4441f-146">编译器 .rsp 响应文件的内容。</span><span class="sxs-lookup"><span data-stu-id="4441f-146">The contents of the compiler's .rsp response file.</span></span>
- <span data-ttu-id="4441f-147">所用编译器的精确版本及其引用的程序集。</span><span class="sxs-lookup"><span data-stu-id="4441f-147">The precise version of the compiler used, and its referenced assemblies.</span></span>
- <span data-ttu-id="4441f-148">当前目录路径。</span><span class="sxs-lookup"><span data-stu-id="4441f-148">The current directory path.</span></span>
- <span data-ttu-id="4441f-149">直接或间接地显式传递到编译器的所有文件的二进制内容，包括：</span><span class="sxs-lookup"><span data-stu-id="4441f-149">The binary contents of all files explicitly passed to the compiler either directly or indirectly, including:</span></span>
  - <span data-ttu-id="4441f-150">源文件</span><span class="sxs-lookup"><span data-stu-id="4441f-150">Source files</span></span>
  - <span data-ttu-id="4441f-151">引用的程序集</span><span class="sxs-lookup"><span data-stu-id="4441f-151">Referenced assemblies</span></span>
  - <span data-ttu-id="4441f-152">引用的模块</span><span class="sxs-lookup"><span data-stu-id="4441f-152">Referenced modules</span></span>
  - <span data-ttu-id="4441f-153">资源</span><span class="sxs-lookup"><span data-stu-id="4441f-153">Resources</span></span>
  - <span data-ttu-id="4441f-154">强名称密钥文件</span><span class="sxs-lookup"><span data-stu-id="4441f-154">The strong name key file</span></span>
  - <span data-ttu-id="4441f-155">@ 响应文件</span><span class="sxs-lookup"><span data-stu-id="4441f-155">@ response files</span></span>
  - <span data-ttu-id="4441f-156">分析器</span><span class="sxs-lookup"><span data-stu-id="4441f-156">Analyzers</span></span>
  - <span data-ttu-id="4441f-157">规则集</span><span class="sxs-lookup"><span data-stu-id="4441f-157">Rulesets</span></span>
  - <span data-ttu-id="4441f-158">分析器可能使用的其他文件</span><span class="sxs-lookup"><span data-stu-id="4441f-158">Other files that may be used by analyzers</span></span>
- <span data-ttu-id="4441f-159">当前区域性（针对生成诊断和异常消息的语言）。</span><span class="sxs-lookup"><span data-stu-id="4441f-159">The current culture (for the language in which diagnostics and exception messages are produced).</span></span>
- <span data-ttu-id="4441f-160">在未指定编码情况下使用的默认编码（或当前代码页）。</span><span class="sxs-lookup"><span data-stu-id="4441f-160">The default encoding (or the current code page) if the encoding isn't specified.</span></span>
- <span data-ttu-id="4441f-161">编译器搜索路径（例如，由`-lib` 或 `-recurse` 指定）上文件是否存在及其内容。</span><span class="sxs-lookup"><span data-stu-id="4441f-161">The existence, non-existence, and contents of files on the compiler's search paths (specified, for example, by `-lib` or `-recurse`).</span></span>
- <span data-ttu-id="4441f-162">运行编译器的公共语言运行时 (CLR) 平台。</span><span class="sxs-lookup"><span data-stu-id="4441f-162">The Common Language Runtime (CLR) platform on which the compiler is run.</span></span>
- <span data-ttu-id="4441f-163">`%LIBPATH%` 的值，该值会影响分析器的依赖项加载。</span><span class="sxs-lookup"><span data-stu-id="4441f-163">The value of `%LIBPATH%`, which can affect analyzer dependency loading.</span></span>

<span data-ttu-id="4441f-164">可使用确定性编译来确定是否从可信源编译二进制内容。</span><span class="sxs-lookup"><span data-stu-id="4441f-164">Deterministic compilation can be used for establishing whether a binary is compiled from a trusted source.</span></span> <span data-ttu-id="4441f-165">当源公开可用时，确定性输出会很有用。</span><span class="sxs-lookup"><span data-stu-id="4441f-165">Deterministic output can be useful when the source is publicly available.</span></span> <span data-ttu-id="4441f-166">它还可以确定生成步骤是否依赖于生成过程中使用的二进制文件的更改。</span><span class="sxs-lookup"><span data-stu-id="4441f-166">It can also determine whether build steps that are dependent on changes to binary used in the build process.</span></span>

## <a name="produceonlyreferenceassembly"></a><span data-ttu-id="4441f-167">ProduceOnlyReferenceAssembly</span><span class="sxs-lookup"><span data-stu-id="4441f-167">ProduceOnlyReferenceAssembly</span></span>

<span data-ttu-id="4441f-168">ProduceOnlyReferenceAssembly 选项表示应输出引用程序集（而不是实现程序集）作为主输出。</span><span class="sxs-lookup"><span data-stu-id="4441f-168">The **ProduceOnlyReferenceAssembly** option indicates that a reference assembly should be output instead of an implementation assembly, as the primary output.</span></span> <span data-ttu-id="4441f-169">ProduceOnlyReferenceAssembly 参数以无提示方式禁用输出 PDB，因为无法执行引用程序集。</span><span class="sxs-lookup"><span data-stu-id="4441f-169">The **ProduceOnlyReferenceAssembly** parameter silently disables outputting PDBs, as reference assemblies cannot be executed.</span></span>

```xml
<ProduceOnlyReferenceAssembly>true</ProduceOnlyReferenceAssembly>
```

<span data-ttu-id="4441f-170">引用程序集是一种特殊类型的程序集。</span><span class="sxs-lookup"><span data-stu-id="4441f-170">Reference assemblies are a special type of assembly.</span></span> <span data-ttu-id="4441f-171">引用程序集只包含表示库的公共 API 外围应用所需的最少元数据量。</span><span class="sxs-lookup"><span data-stu-id="4441f-171">Reference assemblies contain only the minimum amount of metadata required to represent the library's public API surface.</span></span> <span data-ttu-id="4441f-172">它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。</span><span class="sxs-lookup"><span data-stu-id="4441f-172">They include declarations for all members that are significant when referencing an assembly in build tools, but exclude all member implementations and declarations of private members that have no observable impact on their API contract.</span></span> <span data-ttu-id="4441f-173">有关详细信息，请参阅[引用程序集](../../../standard/assembly/reference-assemblies.md)。</span><span class="sxs-lookup"><span data-stu-id="4441f-173">For more information, see [Reference assemblies](../../../standard/assembly/reference-assemblies.md).</span></span>

<span data-ttu-id="4441f-174">ProduceOnlyReferenceAssembly 和 [ProduceReferenceAssembly](output.md#producereferenceassembly) 选项是互斥的。</span><span class="sxs-lookup"><span data-stu-id="4441f-174">The **ProduceOnlyReferenceAssembly** and [**ProduceReferenceAssembly**](output.md#producereferenceassembly) options are mutually exclusive.</span></span>
