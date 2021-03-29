---
title: Aximp.exe（Windows 窗体 ActiveX 控件导入程序）
description: 了解 Aximp.exe（Windows 窗体 ActiveX 控件导入程序）。 此工具将 ActiveX 的 COM 类型库中的类型定义转换为 Windows 窗体。
ms.date: 03/30/2017
helpviewer_keywords:
- ActiveX controls, hosting in Windows Forms
- ActiveX Control Importer
- type definitions, converting
- Aximp.exe
- Windows Forms ActiveX Control Importer
ms.assetid: 482c0d83-7144-4497-b626-87d2351b78d0
ms.openlocfilehash: e0ee504f87eba811dab3422a367a60e235b3c51b
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652914"
---
# <a name="aximpexe-windows-forms-activex-control-importer"></a><span data-ttu-id="10dcf-104">Aximp.exe（Windows 窗体 ActiveX 控件导入程序）</span><span class="sxs-lookup"><span data-stu-id="10dcf-104">Aximp.exe (Windows Forms ActiveX Control Importer)</span></span>

<span data-ttu-id="10dcf-105">ActiveX 控件导入程序将 ActiveX 控件的 COM 类型库中的类型定义转换为 Windows 窗体控件。</span><span class="sxs-lookup"><span data-stu-id="10dcf-105">The ActiveX Control Importer converts type definitions in a COM type library for an ActiveX control into a Windows Forms control.</span></span>  
  
 <span data-ttu-id="10dcf-106">Windows 窗体只能承载 Windows 窗体控件，即从 <xref:System.Windows.Forms.Control> 派生的类。</span><span class="sxs-lookup"><span data-stu-id="10dcf-106">Windows Forms can only host Windows Forms controls — that is, classes that are derived from <xref:System.Windows.Forms.Control>.</span></span> <span data-ttu-id="10dcf-107">Aximp.exe 生成可承载于 Windows 窗体上的 ActiveX 控件的包装器类。</span><span class="sxs-lookup"><span data-stu-id="10dcf-107">Aximp.exe generates a wrapper class for an ActiveX control that can be hosted on a Windows Form.</span></span> <span data-ttu-id="10dcf-108">这使你得以使用适用于其他 Windows 窗体控件的同一设计时支持和编程方法。</span><span class="sxs-lookup"><span data-stu-id="10dcf-108">This allows you to use the same design-time support and programming methodology applicable to other Windows Forms controls.</span></span>  
  
 <span data-ttu-id="10dcf-109">若要承载 ActiveX 控件，必须生成从 <xref:System.Windows.Forms.AxHost> 派生的包装器控件。</span><span class="sxs-lookup"><span data-stu-id="10dcf-109">To host the ActiveX control, you must generate a wrapper control that derives from <xref:System.Windows.Forms.AxHost>.</span></span> <span data-ttu-id="10dcf-110">此包装器控件包含基础 ActiveX 控件的一个实例。</span><span class="sxs-lookup"><span data-stu-id="10dcf-110">This wrapper control contains an instance of the underlying ActiveX control.</span></span> <span data-ttu-id="10dcf-111">它知道如何与 ActiveX 控件通信，但它显示为 Windows 窗体控件。</span><span class="sxs-lookup"><span data-stu-id="10dcf-111">It knows how to communicate with the ActiveX control, but it appears as a Windows Forms control.</span></span> <span data-ttu-id="10dcf-112">这个生成的控件承载 ActiveX 控件并将其属性、方法和事件作为生成的控件的属性、方法和事件公开。</span><span class="sxs-lookup"><span data-stu-id="10dcf-112">This generated control hosts the ActiveX control and exposes its properties, methods, and events as those of the generated control.</span></span>  
  
 <span data-ttu-id="10dcf-113">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="10dcf-113">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="10dcf-114">若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="10dcf-114">To run the tool, use [Visual Studio Developer Command Prompt or Visual Studio Developer PowerShell](/visualstudio/ide/reference/command-prompt-powershell).</span></span>
  
 <span data-ttu-id="10dcf-115">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="10dcf-115">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="10dcf-116">语法</span><span class="sxs-lookup"><span data-stu-id="10dcf-116">Syntax</span></span>  
  
```console  
aximp [options]{file.dll | file.ocx}  
```  
  
## <a name="remarks"></a><span data-ttu-id="10dcf-117">备注</span><span class="sxs-lookup"><span data-stu-id="10dcf-117">Remarks</span></span>  
  
|<span data-ttu-id="10dcf-118">参数</span><span class="sxs-lookup"><span data-stu-id="10dcf-118">Argument</span></span>|<span data-ttu-id="10dcf-119">说明</span><span class="sxs-lookup"><span data-stu-id="10dcf-119">Description</span></span>|  
|--------------|-----------------|  
|<span data-ttu-id="10dcf-120">文件</span><span class="sxs-lookup"><span data-stu-id="10dcf-120">*file*</span></span>|<span data-ttu-id="10dcf-121">包含要转换的 ActiveX 控件的源文件的名称。</span><span class="sxs-lookup"><span data-stu-id="10dcf-121">The name of the source file that contains the ActiveX control to convert.</span></span> <span data-ttu-id="10dcf-122">文件参数中必须具有扩展名 .dll 或 .ocx。</span><span class="sxs-lookup"><span data-stu-id="10dcf-122">The file argument must have the extension .dll or .ocx.</span></span>|  
  
|<span data-ttu-id="10dcf-123">选项</span><span class="sxs-lookup"><span data-stu-id="10dcf-123">Option</span></span>|<span data-ttu-id="10dcf-124">描述</span><span class="sxs-lookup"><span data-stu-id="10dcf-124">Description</span></span>|  
|------------|-----------------|  
|`/delaysign`|<span data-ttu-id="10dcf-125">指定 Aximp.exe 使用延迟的签名对生成的控件进行签名。</span><span class="sxs-lookup"><span data-stu-id="10dcf-125">Specifies to Aximp.exe to sign the resulting control using delayed signing.</span></span> <span data-ttu-id="10dcf-126">必须使用 `/keycontainer:`、`/keyfile:` 或 `/publickey:` 选项指定此选项。</span><span class="sxs-lookup"><span data-stu-id="10dcf-126">You must specify this option with either the `/keycontainer:`, `/keyfile:`, or `/publickey:` option.</span></span> <span data-ttu-id="10dcf-127">有关延迟签名过程的更多信息，请参见[延迟为程序集签名](../../standard/assembly/delay-sign.md)。</span><span class="sxs-lookup"><span data-stu-id="10dcf-127">For more information on the delayed signing process, see [Delay Signing an Assembly](../../standard/assembly/delay-sign.md).</span></span>|  
|`/help`|<span data-ttu-id="10dcf-128">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="10dcf-128">Displays command syntax and options for the tool.</span></span>|  
|<span data-ttu-id="10dcf-129">`/keycontainer:` *containerName*</span><span class="sxs-lookup"><span data-stu-id="10dcf-129">`/keycontainer:` *containerName*</span></span>|<span data-ttu-id="10dcf-130">使用在 containerName 指定的密钥容器中找到的公钥/私钥对，对生成的控件进行强名称签名。</span><span class="sxs-lookup"><span data-stu-id="10dcf-130">Signs the resulting control with a strong name using the public/private key pair found in the key container specified by *containerName*.</span></span>|  
|<span data-ttu-id="10dcf-131">`/keyfile:` *filename*</span><span class="sxs-lookup"><span data-stu-id="10dcf-131">`/keyfile:` *filename*</span></span>|<span data-ttu-id="10dcf-132">使用在 filename 中找到的发行者的正式公钥/私钥对，对生成的控件进行强名称签名。</span><span class="sxs-lookup"><span data-stu-id="10dcf-132">Signs the resulting control with a strong name using the publisher's official public/private key pair found in *filename*.</span></span>|  
|`/nologo`|<span data-ttu-id="10dcf-133">取消显示 Microsoft 启动版权标志。</span><span class="sxs-lookup"><span data-stu-id="10dcf-133">Suppresses the Microsoft startup banner display.</span></span>|  
|<span data-ttu-id="10dcf-134">`/out:` *filename*</span><span class="sxs-lookup"><span data-stu-id="10dcf-134">`/out:` *filename*</span></span>|<span data-ttu-id="10dcf-135">指定要创建的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="10dcf-135">Specifies the name of the assembly to create.</span></span>|  
|<span data-ttu-id="10dcf-136">`/publickey:` *filename*</span><span class="sxs-lookup"><span data-stu-id="10dcf-136">`/publickey:` *filename*</span></span>|<span data-ttu-id="10dcf-137">使用在 filename 指定的文件中找到的公钥，对生成的控件进行强名称签名。</span><span class="sxs-lookup"><span data-stu-id="10dcf-137">Signs the resulting control with a strong name using the public key found in the file specified by *filename*.</span></span>|  
|<span data-ttu-id="10dcf-138">`/rcw:` *filename*</span><span class="sxs-lookup"><span data-stu-id="10dcf-138">`/rcw:` *filename*</span></span>|<span data-ttu-id="10dcf-139">使用指定的运行时可调用包装器，而不用生成新的包装器。</span><span class="sxs-lookup"><span data-stu-id="10dcf-139">Uses the specified runtime callable wrapper instead of generating a new one.</span></span> <span data-ttu-id="10dcf-140">你可以指定多个实例。</span><span class="sxs-lookup"><span data-stu-id="10dcf-140">You may specify multiple instances.</span></span> <span data-ttu-id="10dcf-141">当前目录用于相对路径。</span><span class="sxs-lookup"><span data-stu-id="10dcf-141">The current directory is used for relative paths.</span></span> <span data-ttu-id="10dcf-142">有关详细信息，请参阅[运行时可调用包装器](../../standard/native-interop/runtime-callable-wrapper.md)。</span><span class="sxs-lookup"><span data-stu-id="10dcf-142">For more information, see [Runtime Callable Wrapper](../../standard/native-interop/runtime-callable-wrapper.md).</span></span>|  
|`/silent`|<span data-ttu-id="10dcf-143">取消显示成功消息。</span><span class="sxs-lookup"><span data-stu-id="10dcf-143">Suppresses the display of success messages.</span></span>|  
|`/source`|<span data-ttu-id="10dcf-144">生成 Windows 窗体包装器的 C# 源代码。</span><span class="sxs-lookup"><span data-stu-id="10dcf-144">Generates C# source code for the Windows Forms wrapper.</span></span>|  
|`/verbose`|<span data-ttu-id="10dcf-145">指定详细模式；显示更多进度信息。</span><span class="sxs-lookup"><span data-stu-id="10dcf-145">Specifies verbose mode; displays additional progress information.</span></span>|  
|`/?`|<span data-ttu-id="10dcf-146">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="10dcf-146">Displays command syntax and options for the tool.</span></span>|  
  
 <span data-ttu-id="10dcf-147">Aximp.exe 一次转换整个 ActiveX 控件类型库，并生成一组程序集，这些程序集包含在原始类型库中定义的类型的公共语言运行时元数据和控件实现。</span><span class="sxs-lookup"><span data-stu-id="10dcf-147">Aximp.exe converts an entire ActiveX Control type library at one time and produces a set of assemblies that contain the common language runtime metadata and control implementation for the types defined in the original type library.</span></span> <span data-ttu-id="10dcf-148">生成的文件按照下面的模式命名：</span><span class="sxs-lookup"><span data-stu-id="10dcf-148">The generated files are named according to the following pattern:</span></span>  
  
 <span data-ttu-id="10dcf-149">COM 类型的公共语言运行时代理：progid.dll</span><span class="sxs-lookup"><span data-stu-id="10dcf-149">common language runtime proxy for COM types: *progid*.dll</span></span>  
  
 <span data-ttu-id="10dcf-150">ActiveX 控件的 Windows 窗体代理（其中 Ax 表示 ActiveX）：Axprogid.dll</span><span class="sxs-lookup"><span data-stu-id="10dcf-150">Windows Forms proxy for ActiveX controls (where Ax signifies ActiveX): Ax *progid*.dll</span></span>  
  
> [!NOTE]
> <span data-ttu-id="10dcf-151">如果 ActiveX 控件的成员名称与 .NET Framework 中定义的名称匹配，则 Aximp.exe 在创建 AxHost 派生类时，将在成员名称前加上前缀“Ctl”。</span><span class="sxs-lookup"><span data-stu-id="10dcf-151">If the name of a member of the ActiveX control matches a name defined in the .NET Framework, Aximp.exe will prefix the member name with "Ctl" when it creates the AxHost derived class.</span></span> <span data-ttu-id="10dcf-152">例如，如果 ActiveX 控件有一个名为“Layout”的成员，由于在 .NET Framework 中定义了 Layout 事件，因此该成员在 AxHost 派生类中将重命名为“CtlLayout”。</span><span class="sxs-lookup"><span data-stu-id="10dcf-152">For example, if your ActiveX control has a member named "Layout," it is renamed "CtlLayout" in the AxHost derived class because the Layout event is defined within the .NET Framework.</span></span>  
  
 <span data-ttu-id="10dcf-153">可以使用 [Ildasm.exe（IL 反汇编程序）](ildasm-exe-il-disassembler.md)之类的工具检查这些生成的文件。</span><span class="sxs-lookup"><span data-stu-id="10dcf-153">You can examine these generated files with tools such as [Ildasm.exe (IL Disassembler)](ildasm-exe-il-disassembler.md).</span></span>  
  
 <span data-ttu-id="10dcf-154">不支持使用 Aximp.exe 为 ActiveX WebBrowser 控件 (shdocvw.dll) 生成 .NET 程序集。</span><span class="sxs-lookup"><span data-stu-id="10dcf-154">Using Aximp.exe to generate a .NET assembly for the ActiveX WebBrowser control (shdocvw.dll) is not supported.</span></span>  
  
 <span data-ttu-id="10dcf-155">在对 shdocvw.dll 运行 Aximp.exe 时，该工具始终会在运行它的目录中创建另一个名为 shdocvw.dll 的文件。</span><span class="sxs-lookup"><span data-stu-id="10dcf-155">When you run Aximp.exe over shdocvw.dll, it will always create another file named shdocvw.dll in the directory from which the tool is run.</span></span> <span data-ttu-id="10dcf-156">如果将此生成文件放在 Documents and Settings 目录下，则会导致 Microsoft Internet Explorer 和 Windows 资源管理器出现问题。</span><span class="sxs-lookup"><span data-stu-id="10dcf-156">If this generated file is placed in the Documents and Settings directory, it causes problems for Microsoft Internet Explorer and Windows Explorer.</span></span> <span data-ttu-id="10dcf-157">重启计算机时，Windows 会先在 Documents and Settings 目录查找 shdocvw.dll 的副本，然后再在 system32 目录查找。</span><span class="sxs-lookup"><span data-stu-id="10dcf-157">When the computer is rebooted, Windows looks in the Documents and Settings directory before the system32 directory to find a copy of shdocvw.dll.</span></span> <span data-ttu-id="10dcf-158">它将使用在 Documents and Settings 目录中找到的副本，并尝试加载托管的包装器。</span><span class="sxs-lookup"><span data-stu-id="10dcf-158">It will use the copy it finds in Documents and Settings and attempt to load the managed wrappers.</span></span> <span data-ttu-id="10dcf-159">由于 Internet Explorer 和 Windows 资源管理器依赖于 system32 目录中的 shdocvw.dll 版本中的呈现引擎，因此它们将无法正常工作。</span><span class="sxs-lookup"><span data-stu-id="10dcf-159">Internet Explorer and Windows Explorer will not function properly because they rely on the rendering engine in the version of shdocvw.dll located in the system32 directory.</span></span> <span data-ttu-id="10dcf-160">如果出现此问题，请在 Documents and Settings 目录中删除 shdocvw.dll 的副本，然后重启计算机。</span><span class="sxs-lookup"><span data-stu-id="10dcf-160">If this problem occurs, delete the copy of shdocvw.dll in the Documents and Settings directory and reboot the computer.</span></span>  
  
 <span data-ttu-id="10dcf-161">通过对 shdocvw.dll 使用 Aximp.exe 来创建用于应用程序开发的 .NET 程序集也会导致问题。</span><span class="sxs-lookup"><span data-stu-id="10dcf-161">Using Aximp.exe with shdocvw.dll to create a .NET assembly for use in application development can also cause problems.</span></span> <span data-ttu-id="10dcf-162">在这种情况下，应用程序将同时加载 shdocvw.dll 的系统版本和生成版本，并可能为系统版本赋予更高的优先级。</span><span class="sxs-lookup"><span data-stu-id="10dcf-162">In this case, your application will load both the system version of shdocvw.dll and the generated version, and may give the system version priority.</span></span> <span data-ttu-id="10dcf-163">此时，如果尝试在 WebBrowser ActiveX 控件内加载网页，用户可能会收到打开/保存对话框提示。</span><span class="sxs-lookup"><span data-stu-id="10dcf-163">In this case, when you attempt to load a Web page inside the WebBrowser ActiveX control, users may be prompted with an Open/Save dialog box.</span></span> <span data-ttu-id="10dcf-164">用户单击“打开”后，将在 Internet Explorer 中打开该网页。</span><span class="sxs-lookup"><span data-stu-id="10dcf-164">When the user clicks **Open**, the Web page will be opened in Internet Explorer.</span></span> <span data-ttu-id="10dcf-165">此情况只出现在运行 Internet Explorer 版本 6 或更早版本的计算机上。</span><span class="sxs-lookup"><span data-stu-id="10dcf-165">This occurs only with computers that are running Internet Explorer version 6 or earlier.</span></span> <span data-ttu-id="10dcf-166">若要防止出现此问题，请使用托管的 <xref:System.Windows.Forms.WebBrowser> 控件或使用 Visual Studio 生成托管的 shdocvw.dll，如[如何：添加对类型库的引用](../interop/how-to-add-references-to-type-libraries.md)中所述。</span><span class="sxs-lookup"><span data-stu-id="10dcf-166">To prevent this problem, use the managed <xref:System.Windows.Forms.WebBrowser> control or use Visual Studio to generate the managed shdocvw.dll as described in [How to: Add References to Type Libraries](../interop/how-to-add-references-to-type-libraries.md).</span></span>  
  
## <a name="example"></a><span data-ttu-id="10dcf-167">示例</span><span class="sxs-lookup"><span data-stu-id="10dcf-167">Example</span></span>  

 <span data-ttu-id="10dcf-168">下面的命令为媒体播放器控件 `msdxm.ocx` 生成 MediaPlayer.dll 和 AxMediaPlayer.dll。</span><span class="sxs-lookup"><span data-stu-id="10dcf-168">The following command generates MediaPlayer.dll and AxMediaPlayer.dll for the Media Player control `msdxm.ocx`.</span></span>  
  
```console
aximp c:\systemroot\system32\msdxm.ocx  
```  
  
## <a name="see-also"></a><span data-ttu-id="10dcf-169">请参阅</span><span class="sxs-lookup"><span data-stu-id="10dcf-169">See also</span></span>

- [<span data-ttu-id="10dcf-170">工具</span><span class="sxs-lookup"><span data-stu-id="10dcf-170">Tools</span></span>](index.md)
- [<span data-ttu-id="10dcf-171">Ildasm.exe（IL 反汇编程序）</span><span class="sxs-lookup"><span data-stu-id="10dcf-171">Ildasm.exe (IL Disassembler)</span></span>](ildasm-exe-il-disassembler.md)
