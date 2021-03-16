---
title: Lc.exe（许可证编译器）
description: 使用 Lc.exe（许可证编译器）。 此工具读取包含授权信息的文本文件，并生成一个二进制文件作为资源嵌入到 CLR 可执行文件中。
ms.date: 03/30/2017
helpviewer_keywords:
- Lc.exe
- .licx file
- License Compiler
- control licenses [Windows Forms]
- compiling licenses file
- license file
- .licenses file
- Windows Forms, control licenses
- licensed controls [Windows Forms]
ms.assetid: 2de803b8-495e-4982-b209-19a72aba0460
ms.openlocfilehash: 1a9806cd71f9990d9ce70b35b3af760a22347003
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102258798"
---
# <a name="lcexe-license-compiler"></a><span data-ttu-id="a90a6-104">Lc.exe（许可证编译器）</span><span class="sxs-lookup"><span data-stu-id="a90a6-104">Lc.exe (License Compiler)</span></span>

<span data-ttu-id="a90a6-105">许可证编译器读取包含授权信息的文本文件，并产生一个可作为资源嵌入到公共语言运行时可执行文件中的二进制文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-105">The License Compiler reads text files that contain licensing information and produces a binary file that can be embedded in a common language runtime executable as a resource.</span></span>  
  
 <span data-ttu-id="a90a6-106">每当将一个授权控件添加到窗体中时，Windows 窗体设计器就会自动生成或更新 .licx 文本文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-106">A .licx text file is automatically generated or updated by the Windows Forms Designer whenever a licensed control is added to the form.</span></span> <span data-ttu-id="a90a6-107">作为编译的一部分，项目系统将 .licx 文本文件转换为 .licenses 二进制资源，此资源提供对 .NET 控件授权的支持。</span><span class="sxs-lookup"><span data-stu-id="a90a6-107">As part of compilation, the project system will transform the .licx text file into a .licenses binary resource that provides support for .NET control licensing.</span></span> <span data-ttu-id="a90a6-108">然后该二进制资源将被嵌入到项目输出中。</span><span class="sxs-lookup"><span data-stu-id="a90a6-108">The binary resource will then be embedded in the project output.</span></span>  
  
 <span data-ttu-id="a90a6-109">在生成项目时，如果使用许可证编译器，则不支持在 32 位与 64 位之间进行交叉编译。</span><span class="sxs-lookup"><span data-stu-id="a90a6-109">Cross compilation between 32-bit and 64-bit is not supported when you use the License Compiler when building your project.</span></span> <span data-ttu-id="a90a6-110">这是因为，许可证编译器必须加载程序集，而不允许从 32 位应用程序加载 64 位程序集，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="a90a6-110">This is because the License Compiler has to load assemblies, and loading 64-bit assemblies from a 32-bit application is not allowed, and vice versa.</span></span> <span data-ttu-id="a90a6-111">在这种情况下，使用许可证编译器从命令行手动编译许可证，并指定相应的体系结构。</span><span class="sxs-lookup"><span data-stu-id="a90a6-111">In this case, use the License Compiler from the command line to compile the license manually, and specify the corresponding architecture.</span></span>  
  
 <span data-ttu-id="a90a6-112">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="a90a6-112">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="a90a6-113">若要运行此工具，请使用[开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="a90a6-113">To run the tool, use a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell).</span></span>  
  
 <span data-ttu-id="a90a6-114">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="a90a6-114">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="a90a6-115">语法</span><span class="sxs-lookup"><span data-stu-id="a90a6-115">Syntax</span></span>  
  
```console
      lc /target:  
targetPE /complist:filename [-outdir:path]  
/i:modules [/nologo] [/v]  
```  
  
|<span data-ttu-id="a90a6-116">选项</span><span class="sxs-lookup"><span data-stu-id="a90a6-116">Option</span></span>|<span data-ttu-id="a90a6-117">说明</span><span class="sxs-lookup"><span data-stu-id="a90a6-117">Description</span></span>|  
|------------|-----------------|  
|<span data-ttu-id="a90a6-118">/complist: filename</span><span class="sxs-lookup"><span data-stu-id="a90a6-118">**/complist:** *filename*</span></span>|<span data-ttu-id="a90a6-119">指定包含授权组件列表的文件名，这些授权组件要包含在 .licenses 文件中。</span><span class="sxs-lookup"><span data-stu-id="a90a6-119">Specifies the name of a file that contains the list of licensed components to include in the .licenses file.</span></span> <span data-ttu-id="a90a6-120">每个组件用它的全名引用，并且每行只有一个组件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-120">Each component is referenced using its full name with only one component per line.</span></span><br /><br /> <span data-ttu-id="a90a6-121">命令行用户可为项目中的每个窗体指定一个单独的文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-121">Command-line users can specify a separate file for each form in the project.</span></span> <span data-ttu-id="a90a6-122">Lc.exe 接受多个输入文件并产生一个 .licenses 文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-122">Lc.exe accepts multiple input files and produces a single .licenses file.</span></span>|  
|<span data-ttu-id="a90a6-123">**/h**[**elp**]</span><span class="sxs-lookup"><span data-stu-id="a90a6-123">**/h**[**elp**]</span></span>|<span data-ttu-id="a90a6-124">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="a90a6-124">Displays command syntax and options for the tool.</span></span>|  
|<span data-ttu-id="a90a6-125">/i: module</span><span class="sxs-lookup"><span data-stu-id="a90a6-125">**/i:** *module*</span></span>|<span data-ttu-id="a90a6-126">指定模块，这些模块包含 **/complist** 文件中列出的组件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-126">Specifies the modules that contain the components listed in the **/complist** file.</span></span> <span data-ttu-id="a90a6-127">若要指定多个模块，请使用多个 **/i** 标志。</span><span class="sxs-lookup"><span data-stu-id="a90a6-127">To specify more than one module, use multiple **/i** flags.</span></span>|  
|<span data-ttu-id="a90a6-128">**/nologo**</span><span class="sxs-lookup"><span data-stu-id="a90a6-128">**/nologo**</span></span>|<span data-ttu-id="a90a6-129">取消显示 Microsoft 启动版权标志。</span><span class="sxs-lookup"><span data-stu-id="a90a6-129">Suppresses the Microsoft startup banner display.</span></span>|  
|<span data-ttu-id="a90a6-130">/outdir: path</span><span class="sxs-lookup"><span data-stu-id="a90a6-130">**/outdir:** *path*</span></span>|<span data-ttu-id="a90a6-131">指定用来放置输出 .licenses 文件的目录。</span><span class="sxs-lookup"><span data-stu-id="a90a6-131">Specifies the directory in which to place the output .licenses file.</span></span>|  
|<span data-ttu-id="a90a6-132">/target: targetPE</span><span class="sxs-lookup"><span data-stu-id="a90a6-132">**/target:** *targetPE*</span></span>|<span data-ttu-id="a90a6-133">指定为其生成 .licenses 文件的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-133">Specifies the executable for which the .licenses file is being generated.</span></span>|  
|<span data-ttu-id="a90a6-134">**/v**</span><span class="sxs-lookup"><span data-stu-id="a90a6-134">**/v**</span></span>|<span data-ttu-id="a90a6-135">指定详细模式；显示编译进度信息。</span><span class="sxs-lookup"><span data-stu-id="a90a6-135">Specifies verbose mode; displays compilation progress information.</span></span>|  
|<span data-ttu-id="a90a6-136">@ file </span><span class="sxs-lookup"><span data-stu-id="a90a6-136">**@** *file*</span></span>|<span data-ttu-id="a90a6-137">指定响应 (.rsp) 文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-137">Specifies the response (.rsp) file.</span></span>|  
|<span data-ttu-id="a90a6-138">**/?**</span><span class="sxs-lookup"><span data-stu-id="a90a6-138">**/?**</span></span>|<span data-ttu-id="a90a6-139">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="a90a6-139">Displays command syntax and options for the tool.</span></span>|  
  
## <a name="example"></a><span data-ttu-id="a90a6-140">示例</span><span class="sxs-lookup"><span data-stu-id="a90a6-140">Example</span></span>  
  
1. <span data-ttu-id="a90a6-141">如果使用的是包含在名为 `HostApp.exe` *的应用程序中的 `Samples.DLL` 中的授权控件 `MyCompany.Samples.LicControl1`，* 则可创建包含以下内容的 `HostAppLic.txt`。</span><span class="sxs-lookup"><span data-stu-id="a90a6-141">If you are using a licensed control `MyCompany.Samples.LicControl1` contained in `Samples.DLL` in an application called `HostApp.exe`*,* you can create `HostAppLic.txt` that contains the following.</span></span>  
  
    ```text
    MyCompany.Samples.LicControl1, Samples.DLL  
    ```  
  
2. <span data-ttu-id="a90a6-142">使用以下命令创建名为 `HostApp.exe.licenses` 的 .licenses 文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-142">Create the .licenses file called `HostApp.exe.licenses` using the following command.</span></span>  
  
    ```console  
    lc /target:HostApp.exe /complist:hostapplic.txt /i:Samples.DLL /outdir:c:\bindir  
    ```  
  
3. <span data-ttu-id="a90a6-143">生成将 .licenses 文件作为资源包含在内的 `HostApp.exe`。</span><span class="sxs-lookup"><span data-stu-id="a90a6-143">Build `HostApp.exe` including the .licenses file as a resource.</span></span> <span data-ttu-id="a90a6-144">如果生成的是 C# 应用程序，则应使用以下命令生成应用程序。</span><span class="sxs-lookup"><span data-stu-id="a90a6-144">If you were building a C# application you would use the following command to build your application.</span></span>  
  
    ```console
    csc /res:HostApp.exe.licenses /out:HostApp.exe *.cs  
    ```  
  
 <span data-ttu-id="a90a6-145">以下命令从由 `myApp.licenses`、`hostapplic.txt` 和 `hostapplic2.txt` 指定的授权组件列表来编译 `hostapplic3.txt`。</span><span class="sxs-lookup"><span data-stu-id="a90a6-145">The following command compiles `myApp.licenses` from the lists of licensed components specified by `hostapplic.txt`, `hostapplic2.txt` and `hostapplic3.txt`.</span></span> <span data-ttu-id="a90a6-146">`modulesList` 参数指定包含授权组件的模块。</span><span class="sxs-lookup"><span data-stu-id="a90a6-146">The `modulesList` argument specifies the modules that contain the licensed components.</span></span>  
  
```console  
lc /target:myApp /complist:hostapplic.txt /complist:hostapplic2.txt /complist: hostapplic3.txt /i:modulesList  
```  
  
## <a name="response-file-example"></a><span data-ttu-id="a90a6-147">响应文件示例</span><span class="sxs-lookup"><span data-stu-id="a90a6-147">Response File Example</span></span>  

 <span data-ttu-id="a90a6-148">下面的列表显示响应文件 `response.rsp` 的示例。</span><span class="sxs-lookup"><span data-stu-id="a90a6-148">The following listing shows an example of a response file, `response.rsp`.</span></span> <span data-ttu-id="a90a6-149">有关响应文件的详细信息，请参阅[响应文件](/visualstudio/msbuild/msbuild-response-files)。</span><span class="sxs-lookup"><span data-stu-id="a90a6-149">For more information on response files, see [Response Files](/visualstudio/msbuild/msbuild-response-files).</span></span>  
  
```text  
/target:hostapp.exe  
/complist:hostapplic.txt
/i:WFCPrj.dll
/outdir:"C:\My Folder"  
```  
  
 <span data-ttu-id="a90a6-150">下面的命令行使用 `response.rsp` 文件。</span><span class="sxs-lookup"><span data-stu-id="a90a6-150">The following command line uses the `response.rsp` file.</span></span>  
  
```console  
lc @response.rsp  
```  
  
## <a name="see-also"></a><span data-ttu-id="a90a6-151">请参阅</span><span class="sxs-lookup"><span data-stu-id="a90a6-151">See also</span></span>

- [<span data-ttu-id="a90a6-152">工具</span><span class="sxs-lookup"><span data-stu-id="a90a6-152">Tools</span></span>](index.md)
- [<span data-ttu-id="a90a6-153">Al.exe（程序集链接器）</span><span class="sxs-lookup"><span data-stu-id="a90a6-153">Al.exe (Assembly Linker)</span></span>](al-exe-assembly-linker.md)
- [<span data-ttu-id="a90a6-154">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="a90a6-154">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
