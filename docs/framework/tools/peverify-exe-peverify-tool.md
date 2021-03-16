---
title: Peverify.exe（PEVerify 工具）
description: 使用 Peverify.exe（可移植可执行验证）帮助确定 Microsoft 中间语言 (MSIL) 代码和元数据是否满足 .NET 中的类型安全标准。
ms.date: 03/30/2017
helpviewer_keywords:
- portable executable files, PEVerify
- verifying MSIL and metadata
- PEVerify tool
- type safety requirements
- MSIL
- PEverify.exe
- PE files, PEVerify
ms.assetid: f4f46f9e-8d08-4e66-a94b-0c69c9b0bbfa
ms.openlocfilehash: b51b01e639719df7ecfde53819e3137813f7c46f
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259235"
---
# <a name="peverifyexe-peverify-tool"></a><span data-ttu-id="2f097-103">Peverify.exe（PEVerify 工具）</span><span class="sxs-lookup"><span data-stu-id="2f097-103">Peverify.exe (PEVerify tool)</span></span>

<span data-ttu-id="2f097-104">PEVerify 工具有助于生成 Microsoft 中间语言 (MSIL) 的开发人员（如编译器编写者、脚本引擎开发人员）确定其 MSIL 代码及关联的元数据是否满足类型安全要求。</span><span class="sxs-lookup"><span data-stu-id="2f097-104">The PEVerify tool helps developers who generate Microsoft intermediate language (MSIL) (such as compiler writers and script engine developers) to determine whether their MSIL code and associated metadata meet type safety requirements.</span></span> <span data-ttu-id="2f097-105">某些编译器仅当你避免使用某些语言构造时才生成可验证的类型安全代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-105">Some compilers generate verifiably type-safe code only if you avoid using certain language constructs.</span></span> <span data-ttu-id="2f097-106">如果正在使用此类编译器，则可能需要确认你未危害代码的类型安全性。</span><span class="sxs-lookup"><span data-stu-id="2f097-106">If you're using such a compiler, you may want to verify that you have not compromised the type safety of your code.</span></span> <span data-ttu-id="2f097-107">可以对文件运行 PEVerify 工具来检查 MSIL 和元数据。</span><span class="sxs-lookup"><span data-stu-id="2f097-107">You can run the PEVerify tool on your files to check the MSIL and metadata.</span></span>  
  
 <span data-ttu-id="2f097-108">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="2f097-108">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="2f097-109">若要运行该工具，请[对开发人员使用命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="2f097-109">To run the tool, use a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell).</span></span>
  
## <a name="syntax"></a><span data-ttu-id="2f097-110">语法</span><span class="sxs-lookup"><span data-stu-id="2f097-110">Syntax</span></span>  
  
```console  
peverify filename [options]  
```  
  
## <a name="parameters"></a><span data-ttu-id="2f097-111">参数</span><span class="sxs-lookup"><span data-stu-id="2f097-111">Parameters</span></span>  
  
|<span data-ttu-id="2f097-112">参数</span><span class="sxs-lookup"><span data-stu-id="2f097-112">Argument</span></span>|<span data-ttu-id="2f097-113">描述</span><span class="sxs-lookup"><span data-stu-id="2f097-113">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="2f097-114">*filename*</span><span class="sxs-lookup"><span data-stu-id="2f097-114">*filename*</span></span>|<span data-ttu-id="2f097-115">要为其检查 MSIL 和元数据的可移植可执行 (PE) 文件。</span><span class="sxs-lookup"><span data-stu-id="2f097-115">The portable executable (PE) file for which to check the MSIL and metadata.</span></span>|  
  
|<span data-ttu-id="2f097-116">选项</span><span class="sxs-lookup"><span data-stu-id="2f097-116">Option</span></span>|<span data-ttu-id="2f097-117">描述</span><span class="sxs-lookup"><span data-stu-id="2f097-117">Description</span></span>|  
|------------|-----------------|  
|<span data-ttu-id="2f097-118">/break= maxErrorCount</span><span class="sxs-lookup"><span data-stu-id="2f097-118">**/break=** *maxErrorCount*</span></span>|<span data-ttu-id="2f097-119">在出现 maxErrorCount 错误后中止验证。</span><span class="sxs-lookup"><span data-stu-id="2f097-119">Aborts verification after *maxErrorCount* errors.</span></span><br /><br /> <span data-ttu-id="2f097-120">此参数在 .NET Framework 2.0 版或更高版本中不受支持。</span><span class="sxs-lookup"><span data-stu-id="2f097-120">This parameter is not supported in .NET Framework version 2.0 or later.</span></span>|  
|<span data-ttu-id="2f097-121">**/clock**</span><span class="sxs-lookup"><span data-stu-id="2f097-121">**/clock**</span></span>|<span data-ttu-id="2f097-122">测量并报告下列验证时间（以毫秒为单位）：</span><span class="sxs-lookup"><span data-stu-id="2f097-122">Measures and reports the following verification times in milliseconds:</span></span><br /><br /> <span data-ttu-id="2f097-123">MD Val. cycle</span><span class="sxs-lookup"><span data-stu-id="2f097-123">**MD Val. cycle**</span></span><br /> <span data-ttu-id="2f097-124">元数据验证周期</span><span class="sxs-lookup"><span data-stu-id="2f097-124">Metadata validation cycle</span></span><br /><br /> <span data-ttu-id="2f097-125">MD Val. pure</span><span class="sxs-lookup"><span data-stu-id="2f097-125">**MD Val. pure**</span></span><br /> <span data-ttu-id="2f097-126">元数据纯验证</span><span class="sxs-lookup"><span data-stu-id="2f097-126">Metadata validation pure</span></span><br /><br /> <span data-ttu-id="2f097-127">IL Ver. cycle</span><span class="sxs-lookup"><span data-stu-id="2f097-127">**IL Ver. cycle**</span></span><br /> <span data-ttu-id="2f097-128">Microsoft 中间语言 (MSIL) 验证周期</span><span class="sxs-lookup"><span data-stu-id="2f097-128">Microsoft intermediate language (MSIL) verification cycle</span></span><br /><br /> <span data-ttu-id="2f097-129">IL Ver pure</span><span class="sxs-lookup"><span data-stu-id="2f097-129">**IL Ver pure**</span></span><br /> <span data-ttu-id="2f097-130">MSIL 纯验证</span><span class="sxs-lookup"><span data-stu-id="2f097-130">MSIL verification pure</span></span><br /><br /> <span data-ttu-id="2f097-131">MD Val. cycle 和 IL Ver. cycle 时间包括执行必要的启动和关闭过程所需的时间 。</span><span class="sxs-lookup"><span data-stu-id="2f097-131">The **MD Val. cycle** and **IL Ver. cycle** times include the time required to perform necessary startup and shutdown procedures.</span></span> <span data-ttu-id="2f097-132">MD Val. pure 和 IL Ver pure 时间反映了只执行验证或检验所需的时间 。</span><span class="sxs-lookup"><span data-stu-id="2f097-132">The **MD Val. pure** and **IL Ver pure** times reflect the time required to perform the validation or verification only.</span></span>|  
|<span data-ttu-id="2f097-133">**/help**</span><span class="sxs-lookup"><span data-stu-id="2f097-133">**/help**</span></span>|<span data-ttu-id="2f097-134">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="2f097-134">Displays command syntax and options for the tool.</span></span>|  
|<span data-ttu-id="2f097-135">/hresult</span><span class="sxs-lookup"><span data-stu-id="2f097-135">**/hresult**</span></span>|<span data-ttu-id="2f097-136">以十六进制格式显示错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-136">Displays error codes in hexadecimal format.</span></span>|  
|<span data-ttu-id="2f097-137">/ignore= hex.code [, hex.code] </span><span class="sxs-lookup"><span data-stu-id="2f097-137">**/ignore=** *hex.code* [, *hex.code*]</span></span>|<span data-ttu-id="2f097-138">忽略指定的错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-138">Ignores the specified error codes.</span></span>|  
|<span data-ttu-id="2f097-139">/ignore=@ responseFile</span><span class="sxs-lookup"><span data-stu-id="2f097-139">**/ignore=@** *responseFile*</span></span>|<span data-ttu-id="2f097-140">忽略在指定响应文件中列出的错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-140">Ignores the error codes listed in the specified response file.</span></span>|  
|<span data-ttu-id="2f097-141">/il</span><span class="sxs-lookup"><span data-stu-id="2f097-141">**/il**</span></span>|<span data-ttu-id="2f097-142">针对在由 filename 指定的程序集中实现的方法执行 MSIL 类型安全验证检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-142">Performs MSIL type safety verification checks for methods implemented in the assembly specified by *filename*.</span></span> <span data-ttu-id="2f097-143">除非指定 /quiet 选项，否则此工具将为发现的每个问题返回详细的说明。</span><span class="sxs-lookup"><span data-stu-id="2f097-143">The tool returns detailed descriptions for each problem found unless you specify the **/quiet** option.</span></span>|  
|<span data-ttu-id="2f097-144">/md</span><span class="sxs-lookup"><span data-stu-id="2f097-144">**/md**</span></span>|<span data-ttu-id="2f097-145">针对由 filename 指定的程序集执行元数据验证检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-145">Performs metadata validation checks on the assembly specified by *filename*.</span></span> <span data-ttu-id="2f097-146">这将遍历文件中的整个元数据结构并报告遇到的所有验证问题。</span><span class="sxs-lookup"><span data-stu-id="2f097-146">This option walks the full metadata structure within the file and reports all validation problems encountered.</span></span>|  
|<span data-ttu-id="2f097-147">**/nologo**</span><span class="sxs-lookup"><span data-stu-id="2f097-147">**/nologo**</span></span>|<span data-ttu-id="2f097-148">禁止显示产品版本和版权信息。</span><span class="sxs-lookup"><span data-stu-id="2f097-148">Suppresses the display of product version and copyright information.</span></span>|  
|<span data-ttu-id="2f097-149">/nosymbols</span><span class="sxs-lookup"><span data-stu-id="2f097-149">**/nosymbols**</span></span>|<span data-ttu-id="2f097-150">在 .NET Framework 2.0 版中，禁止显示行号以便向后兼容。</span><span class="sxs-lookup"><span data-stu-id="2f097-150">In the .NET Framework version 2.0, suppresses line numbers for backward compatibility.</span></span>|  
|<span data-ttu-id="2f097-151">**/quiet**</span><span class="sxs-lookup"><span data-stu-id="2f097-151">**/quiet**</span></span>|<span data-ttu-id="2f097-152">指定安静模式；禁止显示验证问题报告的输出。</span><span class="sxs-lookup"><span data-stu-id="2f097-152">Specifies quiet mode; suppresses output of the verification problem reports.</span></span> <span data-ttu-id="2f097-153">Peverify.exe 仍将报告文件是否是类型安全的，但不会报告关于阻止类型安全验证的问题的信息。</span><span class="sxs-lookup"><span data-stu-id="2f097-153">Peverify.exe still reports whether the file is type safe, but does not report information on problems preventing type safety verification.</span></span>|  
|`/transparent`|<span data-ttu-id="2f097-154">仅验证透明方法。</span><span class="sxs-lookup"><span data-stu-id="2f097-154">Verify only the transparent methods.</span></span>|  
|<span data-ttu-id="2f097-155">/unique</span><span class="sxs-lookup"><span data-stu-id="2f097-155">**/unique**</span></span>|<span data-ttu-id="2f097-156">忽略重复的错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-156">Ignores repeating error codes.</span></span>|  
|<span data-ttu-id="2f097-157">**/verbose**</span><span class="sxs-lookup"><span data-stu-id="2f097-157">**/verbose**</span></span>|<span data-ttu-id="2f097-158">在 .NET Framework 2.0 版中，显示 MSIL 验证消息中的附加信息。</span><span class="sxs-lookup"><span data-stu-id="2f097-158">In the .NET Framework version 2.0, displays additional information in MSIL verification messages.</span></span>|  
|<span data-ttu-id="2f097-159">**/?**</span><span class="sxs-lookup"><span data-stu-id="2f097-159">**/?**</span></span>|<span data-ttu-id="2f097-160">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="2f097-160">Displays command syntax and options for the tool.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="2f097-161">备注</span><span class="sxs-lookup"><span data-stu-id="2f097-161">Remarks</span></span>  

 <span data-ttu-id="2f097-162">公共语言运行时依赖应用程序代码的类型安全执行，以帮助强制实施安全性和隔离机制。</span><span class="sxs-lookup"><span data-stu-id="2f097-162">The common language runtime relies on the type-safe execution of application code to help enforce security and isolation mechanisms.</span></span> <span data-ttu-id="2f097-163">通常情况下，虽然可以设置安全策略以允许执行受信任但不可验证的代码，但无法运行不是[可验证类型安全](../../standard/security/key-security-concepts.md#type-safety-and-security)的代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-163">Normally, code that is not [verifiably type safe](../../standard/security/key-security-concepts.md#type-safety-and-security) cannot run, although you can set security policy to allow the execution of trusted but unverifiable code.</span></span>  
  
 <span data-ttu-id="2f097-164">如果既未指定 /md 选项也未指定 /il 选项，则 Peverify.exe 将执行两种类型的检查 。</span><span class="sxs-lookup"><span data-stu-id="2f097-164">If neither the **/md** nor **/il** options are specified, Peverify.exe performs both types of checks.</span></span> <span data-ttu-id="2f097-165">Peverify.exe 首先执行 /md 检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-165">Peverify.exe performs **/md** checks first.</span></span> <span data-ttu-id="2f097-166">如果没有错误，则执行 /il 检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-166">If there are no errors, **/il** checks are made.</span></span> <span data-ttu-id="2f097-167">如果同时指定了 /md 和 /il，则即使元数据中存在错误，也执行 /il 检查  。</span><span class="sxs-lookup"><span data-stu-id="2f097-167">If you specify both **/md** and **/il**, **/il** checks are made even if there are errors in the metadata.</span></span> <span data-ttu-id="2f097-168">因此，如果没有元数据错误，则 peverify filename 等效于 peverify filename /md /il 。</span><span class="sxs-lookup"><span data-stu-id="2f097-168">Thus, if there are no metadata errors, **peverify** *filename* is equivalent to **peverify** *filename* **/md** **/il**.</span></span>  
  
 <span data-ttu-id="2f097-169">Peverify.exe 基于数据流分析和一个包含数百条有关有效元数据的规则的列表来执行全面的 MSIL 验证检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-169">Peverify.exe performs comprehensive MSIL verification checks based on dataflow analysis plus a list of several hundred rules on valid metadata.</span></span> <span data-ttu-id="2f097-170">有关 Peverify.exe 执行的检查的详细信息，请参见 Windows SDK 中“Tools Developers Guide”文件夹中的“元数据验证规范”和“MSIL 指令集规范”。</span><span class="sxs-lookup"><span data-stu-id="2f097-170">For detailed information on the checks Peverify.exe performs, see the "Metadata Validation Specification" and the "MSIL Instruction Set Specification" in the Tools Developers Guide folder in the Windows SDK.</span></span>  
  
<span data-ttu-id="2f097-171">.NET Framework 2.0 版或更高版本支持使用如下 MSIL 指令指定的可验证 `byref` 返回值：`dup`、`ldsflda`、`ldflda`、`ldelema`、`call` 和 `unbox`。</span><span class="sxs-lookup"><span data-stu-id="2f097-171">.NET Framework version 2.0 or later supports verifiable `byref` returns specified using the following MSIL instructions: `dup`, `ldsflda`, `ldflda`, `ldelema`, `call`, and `unbox`.</span></span>  
  
## <a name="examples"></a><span data-ttu-id="2f097-172">示例</span><span class="sxs-lookup"><span data-stu-id="2f097-172">Examples</span></span>  

 <span data-ttu-id="2f097-173">下面的命令为在 `myAssembly.exe` 程序集中实现的方法执行元数据验证检查和 MSIL 类型安全验证检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-173">The following command performs metadata validation checks and MSIL type safety verification checks for methods implemented in the assembly `myAssembly.exe`.</span></span>  
  
```console  
peverify myAssembly.exe /md /il  
```  
  
 <span data-ttu-id="2f097-174">上述请求成功完成后，Peverify.exe 将显示下面的消息。</span><span class="sxs-lookup"><span data-stu-id="2f097-174">Upon successful completion of the above request, Peverify.exe displays the following message.</span></span>  
  
```output
All classes and methods in myAssembly.exe Verified  
```  
  
 <span data-ttu-id="2f097-175">下面的命令为在 `myAssembly.exe` 程序集中实现的方法执行元数据验证检查和 MSIL 类型安全验证检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-175">The following command performs metadata validation checks and MSIL type safety verification checks for methods implemented in the assembly `myAssembly.exe`.</span></span> <span data-ttu-id="2f097-176">此工具显示执行这些检查所需的时间。</span><span class="sxs-lookup"><span data-stu-id="2f097-176">The tool displays the time required to perform these checks.</span></span>  
  
```console  
peverify myAssembly.exe /md /il /clock  
```  
  
 <span data-ttu-id="2f097-177">上述请求成功完成后，Peverify.exe 将显示下面的消息。</span><span class="sxs-lookup"><span data-stu-id="2f097-177">Upon successful completion of the above request, Peverify.exe displays the following message.</span></span>  
  
```output
All classes and methods in myAssembly.exe Verified  
Timing: Total run     320 msec  
        MD Val.cycle  40 msec  
        MD Val.pure   10 msec  
        IL Ver.cycle  270 msec  
        IL Ver.pure   230 msec  
```  
  
 <span data-ttu-id="2f097-178">下面的命令为在 `myAssembly.exe` 程序集中实现的方法执行元数据验证检查和 MSIL 类型安全验证检查。</span><span class="sxs-lookup"><span data-stu-id="2f097-178">The following command performs metadata validation checks and MSIL type safety verification checks for methods implemented in the assembly `myAssembly.exe`.</span></span> <span data-ttu-id="2f097-179">但是，Peverify.exe 会在达到最大错误计数 100 时停止。</span><span class="sxs-lookup"><span data-stu-id="2f097-179">Peverify.exe stops, however, when it reaches the maximum error count of 100.</span></span> <span data-ttu-id="2f097-180">该工具也忽略指定的错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-180">The tool also ignores the specified error codes.</span></span>  
  
```console  
peverify myAssembly.exe /break=100 /ignore=0x12345678,0xABCD1234  
```  
  
 <span data-ttu-id="2f097-181">下面的命令与上一个示例产生同样的结果，但指定要在响应文件 `ignoreErrors.rsp` 中忽略的错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-181">The following command produces the same result as the above previous example, but specifies the error codes to ignore in the response file `ignoreErrors.rsp`.</span></span>  
  
```console  
peverify myAssembly.exe /break=100 /ignore@ignoreErrors.rsp  
```  
  
 <span data-ttu-id="2f097-182">响应文件可以包含一个用逗号分隔的错误代码列表。</span><span class="sxs-lookup"><span data-stu-id="2f097-182">The response file can contain a comma-separated list of error codes.</span></span>  
  
```text
0x12345678, 0xABCD1234  
```  
  
 <span data-ttu-id="2f097-183">或者，可将响应文件的格式设置为每行一个错误代码。</span><span class="sxs-lookup"><span data-stu-id="2f097-183">Alternatively, the response file can be formatted with one error code per line.</span></span>  
  
```text
0x12345678  
0xABCD1234  
```  
  
## <a name="see-also"></a><span data-ttu-id="2f097-184">请参阅</span><span class="sxs-lookup"><span data-stu-id="2f097-184">See also</span></span>

- [<span data-ttu-id="2f097-185">工具</span><span class="sxs-lookup"><span data-stu-id="2f097-185">Tools</span></span>](index.md)
- [<span data-ttu-id="2f097-186">编写可验证类型安全代码</span><span class="sxs-lookup"><span data-stu-id="2f097-186">Writing Verifiably Type-Safe Code</span></span>](../misc/code-access-security-basics.md#typesafe_code)
- [<span data-ttu-id="2f097-187">类型安全和安全性</span><span class="sxs-lookup"><span data-stu-id="2f097-187">Type Safety and Security</span></span>](../../standard/security/key-security-concepts.md#type-safety-and-security)
- [<span data-ttu-id="2f097-188">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="2f097-188">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
