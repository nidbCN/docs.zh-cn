---
title: Fuslogvw.exe（程序集绑定日志查看器）
description: 使用 Fuslogvw.exe（程序集绑定日志查看器）。 此查看器显示程序集绑定的详细信息，这有助于诊断 .NET 无法在运行时找到程序集的原因。
ms.date: 03/30/2017
helpviewer_keywords:
- failed assembly binds
- Fuslogvw.exe
- displaying failed assembly bind details
- assemblies [.NET Framework], failed assembly binds
- locating assemblies
- Assembly Binding Log Viewer
ms.assetid: e32fa443-0778-4cc3-bf36-5c8ea297d296
ms.openlocfilehash: d9c028507c19ef8599e58b38dcdf15af2ede1dee
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102259273"
---
# <a name="fuslogvwexe-assembly-binding-log-viewer"></a><span data-ttu-id="30efb-104">Fuslogvw.exe（程序集绑定日志查看器）</span><span class="sxs-lookup"><span data-stu-id="30efb-104">Fuslogvw.exe (Assembly Binding Log Viewer)</span></span>

<span data-ttu-id="30efb-105">程序集绑定日志查看器显示程序集绑定的详细信息。</span><span class="sxs-lookup"><span data-stu-id="30efb-105">The Assembly Binding Log Viewer displays details for assembly binds.</span></span> <span data-ttu-id="30efb-106">这些信息有助于你诊断 .NET Framework 无法在运行时找到程序集的原因。</span><span class="sxs-lookup"><span data-stu-id="30efb-106">This information helps you diagnose why the .NET Framework cannot locate an assembly at run time.</span></span> <span data-ttu-id="30efb-107">这些失败通常由以下因素导致：部署到错误位置的程序集、不再有效的本机映像或者版本号或区域性不匹配。</span><span class="sxs-lookup"><span data-stu-id="30efb-107">These failures are usually the result of an assembly deployed to the wrong location, a native image that is no longer valid, or a mismatch in version numbers or cultures.</span></span> <span data-ttu-id="30efb-108">如果公共语言运行时未能找到程序集，则通常会在你的应用程序中表现为 <xref:System.TypeLoadException>。</span><span class="sxs-lookup"><span data-stu-id="30efb-108">The common language runtime's failure to locate an assembly typically shows up as a <xref:System.TypeLoadException> in your application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30efb-109">你必须使用管理员特权运行 fuslogvw.exe。</span><span class="sxs-lookup"><span data-stu-id="30efb-109">You must run fuslogvw.exe with administrator privileges.</span></span>

<span data-ttu-id="30efb-110">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="30efb-110">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="30efb-111">若要运行此工具，请通过管理员凭据使用[开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="30efb-111">To run the tool, use a [command-line shell for developers](/visualstudio/ide/reference/command-prompt-powershell) with administrator credentials.</span></span>

<span data-ttu-id="30efb-112">在命令提示符处，输入下列命令：</span><span class="sxs-lookup"><span data-stu-id="30efb-112">At the command prompt, enter the following command:</span></span>

```console
fuslogvw
```

<span data-ttu-id="30efb-113">查看器将为每个失败的程序集绑定显示一个条目。</span><span class="sxs-lookup"><span data-stu-id="30efb-113">The viewer displays an entry for each failed assembly bind.</span></span> <span data-ttu-id="30efb-114">对于每一次失败，查看器都会描述：</span><span class="sxs-lookup"><span data-stu-id="30efb-114">For each failure, the viewer describes:</span></span>

- <span data-ttu-id="30efb-115">启动了绑定的应用程序</span><span class="sxs-lookup"><span data-stu-id="30efb-115">the application that initiated the bind</span></span>
- <span data-ttu-id="30efb-116">绑定的程序集，包括名称、版本、区域性和公钥</span><span class="sxs-lookup"><span data-stu-id="30efb-116">the assembly the bind is for, including name, version, culture and public key</span></span>
- <span data-ttu-id="30efb-117">失败的日期和时间</span><span class="sxs-lookup"><span data-stu-id="30efb-117">the date and time of the failure</span></span>

## <a name="how-to"></a><span data-ttu-id="30efb-118">如何</span><span class="sxs-lookup"><span data-stu-id="30efb-118">How to...</span></span>

- [<span data-ttu-id="30efb-119">更改日志位置视图</span><span class="sxs-lookup"><span data-stu-id="30efb-119">Change the log location view</span></span>](#change-the-log-location-view)
- [<span data-ttu-id="30efb-120">查看有关特定失败的详细信息</span><span class="sxs-lookup"><span data-stu-id="30efb-120">View details about a specific failure</span></span>](#view-details-about-a-specific-failure)
- [<span data-ttu-id="30efb-121">删除条目</span><span class="sxs-lookup"><span data-stu-id="30efb-121">Delete entries</span></span>](#delete-entries)
- [<span data-ttu-id="30efb-122">刷新用户界面</span><span class="sxs-lookup"><span data-stu-id="30efb-122">Refresh the user interface</span></span>](#refresh-the-user-interface)
- [<span data-ttu-id="30efb-123">更改日志设置</span><span class="sxs-lookup"><span data-stu-id="30efb-123">Change the log settings</span></span>](#change-the-log-settings)
- [<span data-ttu-id="30efb-124">查看“关于”对话框</span><span class="sxs-lookup"><span data-stu-id="30efb-124">View the About dialog</span></span>](#view-the-about-dialog)

### <a name="change-the-log-location-view"></a><span data-ttu-id="30efb-125">更改日志位置视图</span><span class="sxs-lookup"><span data-stu-id="30efb-125">Change the log location view</span></span>

1. <span data-ttu-id="30efb-126">选择“默认”选项按钮以查看所有应用程序类型的绑定失败。</span><span class="sxs-lookup"><span data-stu-id="30efb-126">Select the **Default** option button to view bind failures for all application types.</span></span> <span data-ttu-id="30efb-127">默认情况下，日志条目存储在磁盘上 wininet 缓存中基于用户的目录中。</span><span class="sxs-lookup"><span data-stu-id="30efb-127">By default, log entries are stored in per-user directories on disk in the wininet cache.</span></span>

2. <span data-ttu-id="30efb-128">选择“自定义”选项按钮以查看指定的自定义目录中的绑定失败。</span><span class="sxs-lookup"><span data-stu-id="30efb-128">Select the **Custom** option button to view bind failures in a custom directory that you specify.</span></span> <span data-ttu-id="30efb-129">必须指定希望运行时存储日志的自定义位置，方法是在“日志设置”对话框中将自定义日志位置设置为有效的目录名。</span><span class="sxs-lookup"><span data-stu-id="30efb-129">You must specify the custom location where you want the runtime to store the logs by setting the custom log location in the **Log Settings** dialog to a valid directory name.</span></span> <span data-ttu-id="30efb-130">此目录应是干净的，并且仅包含运行时所生成的文件。</span><span class="sxs-lookup"><span data-stu-id="30efb-130">This directory should be clean, and only contain files that the runtime generates.</span></span> <span data-ttu-id="30efb-131">如果该目录中包含了一个生成要记录下来的失败的可执行文件，则将不会记录该失败，因为此工具会尝试创建一个与该可执行文件同名的目录。</span><span class="sxs-lookup"><span data-stu-id="30efb-131">If it contains an executable that generates a failure to be logged, the failure will not be logged because the tool tries to create a directory with the same name as the executable.</span></span> <span data-ttu-id="30efb-132">此外，从日志位置运行可执行文件的尝试也将失败。</span><span class="sxs-lookup"><span data-stu-id="30efb-132">In addition, an attempt to run an executable from the log location will fail.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30efb-133">默认的绑定位置优于自定义绑定位置。</span><span class="sxs-lookup"><span data-stu-id="30efb-133">The default bind location is preferable to the custom bind location.</span></span> <span data-ttu-id="30efb-134">运行时将默认绑定位置存储在 wininet 缓存中，因而可以自动清除该位置。如果你指定了自定义绑定位置，则需负责将其清除。</span><span class="sxs-lookup"><span data-stu-id="30efb-134">The runtime stores the default bind location in the wininet cache, and therefore automatically cleans it out. If you specify a custom bind location, you are responsible for cleaning it out.</span></span>

### <a name="view-details-about-a-specific-failure"></a><span data-ttu-id="30efb-135">查看有关特定失败的详细信息</span><span class="sxs-lookup"><span data-stu-id="30efb-135">View details about a specific failure</span></span>

1. <span data-ttu-id="30efb-136">在查看器中选择所需条目的应用程序名称。</span><span class="sxs-lookup"><span data-stu-id="30efb-136">Select the application name of the desired entry in the viewer.</span></span>

2. <span data-ttu-id="30efb-137">单击“查看日志”按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-137">Click the **View Log** button.</span></span> <span data-ttu-id="30efb-138">或者，你可以双击所选条目。</span><span class="sxs-lookup"><span data-stu-id="30efb-138">Alternately, you can double-click the selected entry.</span></span>

    <span data-ttu-id="30efb-139">该工具将显示以下有关所选绑定失败的详细信息：</span><span class="sxs-lookup"><span data-stu-id="30efb-139">The tool displays the following details about the selected bind failure:</span></span>

    - <span data-ttu-id="30efb-140">绑定失败的具体原因，例如“未找到文件”或“版本不匹配”。</span><span class="sxs-lookup"><span data-stu-id="30efb-140">The specific reason the bind failed, such as "file not found" or "version mismatch".</span></span>

    - <span data-ttu-id="30efb-141">与启动绑定的应用程序有关的信息，包括其名称、应用程序的根目录 (AppBase) 以及专用搜索路径的说明（如果具有此类路径）。</span><span class="sxs-lookup"><span data-stu-id="30efb-141">Information about the application that initiated the bind, including its name, the application's root directory (AppBase), and a description of the private search path, if there is one.</span></span>

    - <span data-ttu-id="30efb-142">该工具要查找的程序集的标识。</span><span class="sxs-lookup"><span data-stu-id="30efb-142">The identity of the assembly the tool is looking for.</span></span>

    - <span data-ttu-id="30efb-143">已经应用的任何应用程序、发行者或管理员版本策略的说明。</span><span class="sxs-lookup"><span data-stu-id="30efb-143">A description of any Application, Publisher, or Administrator version policies that have been applied.</span></span>

    - <span data-ttu-id="30efb-144">是否已在[全局程序集缓存](../app-domains/gac.md)中找到了该程序集。</span><span class="sxs-lookup"><span data-stu-id="30efb-144">Whether the assembly was found in the [global assembly cache](../app-domains/gac.md).</span></span>

    - <span data-ttu-id="30efb-145">所有探测 URL 的列表。</span><span class="sxs-lookup"><span data-stu-id="30efb-145">A list of all probing URLs.</span></span>

<span data-ttu-id="30efb-146">以下示例日志条目显示了与失败的程序集绑定有关的详细信息。</span><span class="sxs-lookup"><span data-stu-id="30efb-146">The following sample log entry shows detailed information about a failed assembly bind.</span></span>

```output
*** Assembly Binder Log Entry  (3/5/2007 @ 12:54:20 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  C:\WINNT\Microsoft.NET\Framework\v2.0.50727\fusion.dll
Running under executable  C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\graphicfailtest.exe
--- A detailed error log follows.

=== Pre-bind state information ===
LOG: DisplayName = graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
 (Fully-specified)
LOG: Appbase = C:\Program Files\Microsoft.NET\FrameworkSDK\Samples\Tutorials\resourcesandlocalization\graphic\cs\
LOG: Initial PrivatePath = NULL
LOG: Dynamic Base = NULL
LOG: Cache Base = NULL
LOG: AppName = NULL
Calling assembly : graphicfailtest, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
===

LOG: Processing DEVPATH.
LOG: DEVPATH is not set. Falling through to regular bind.
LOG: Policy not being applied to reference at this time (private, custom, partial, or location-based assembly bind).
LOG: Post-policy reference: graphicfailtest.resources, Version=0.0.0.0, Culture=en-US, PublicKeyToken=null
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.DLL.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources.EXE.
LOG: Attempting download of new URL file:///C:/Program Files/Microsoft.NET/FrameworkSDK/Samples/Tutorials/resourcesandlocalization/graphic/cs/graphicfailtest.resources/graphicfailtest.resources.EXE.
LOG: All probing URLs attempted and failed.
```

### <a name="delete-entries"></a><span data-ttu-id="30efb-147">删除条目</span><span class="sxs-lookup"><span data-stu-id="30efb-147">Delete entries</span></span>

<span data-ttu-id="30efb-148">从日志中删除一个条目：</span><span class="sxs-lookup"><span data-stu-id="30efb-148">To delete a single entry from the log:</span></span>

1. <span data-ttu-id="30efb-149">在查看器中选择一个条目。</span><span class="sxs-lookup"><span data-stu-id="30efb-149">Select an entry in the viewer.</span></span>

2. <span data-ttu-id="30efb-150">单击“删除条目”按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-150">Click the **Delete Entry** button.</span></span>

<span data-ttu-id="30efb-151">从日志中删除所有条目：</span><span class="sxs-lookup"><span data-stu-id="30efb-151">To delete all entries from the log:</span></span>

- <span data-ttu-id="30efb-152">单击“全部删除”按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-152">Click the **Delete All** button.</span></span>

### <a name="refresh-the-user-interface"></a><span data-ttu-id="30efb-153">刷新用户界面</span><span class="sxs-lookup"><span data-stu-id="30efb-153">Refresh the user interface</span></span>

- <span data-ttu-id="30efb-154">单击“刷新”按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-154">Click the **Refresh** button.</span></span> <span data-ttu-id="30efb-155">查看器在其运行时不会自动检测新的日志条目。</span><span class="sxs-lookup"><span data-stu-id="30efb-155">The viewer does not automatically detect new log entries while it is running.</span></span> <span data-ttu-id="30efb-156">必须使用“刷新”按钮来显示它们。</span><span class="sxs-lookup"><span data-stu-id="30efb-156">You must use the **Refresh** button to display them.</span></span>

### <a name="change-the-log-settings"></a><span data-ttu-id="30efb-157">更改日志设置</span><span class="sxs-lookup"><span data-stu-id="30efb-157">Change the log settings</span></span>

<span data-ttu-id="30efb-158">单击“设置”按钮以打开“日志设置”对话框 。</span><span class="sxs-lookup"><span data-stu-id="30efb-158">Click the **Settings** button to open the **Log Settings** dialog.</span></span>

### <a name="view-the-about-dialog"></a><span data-ttu-id="30efb-159">查看“关于”对话框</span><span class="sxs-lookup"><span data-stu-id="30efb-159">View the About dialog</span></span>

<span data-ttu-id="30efb-160">单击“关于”按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-160">Click the **About** button.</span></span>

## <a name="binding-logs-for-native-images"></a><span data-ttu-id="30efb-161">本机映像的绑定日志</span><span class="sxs-lookup"><span data-stu-id="30efb-161">Binding logs for native images</span></span>

<span data-ttu-id="30efb-162">默认情况下，Fuslogvw.exe 将记录普通的程序集绑定请求。</span><span class="sxs-lookup"><span data-stu-id="30efb-162">By default, Fuslogvw.exe logs normal assembly bind requests.</span></span> <span data-ttu-id="30efb-163">此外，还可以记录使用 [Ngen.exe（本机映像生成器）](ngen-exe-native-image-generator.md)创建的本机映像的程序集绑定。</span><span class="sxs-lookup"><span data-stu-id="30efb-163">Alternatively, you can log assembly binds for native images that were created using the [Ngen.exe (Native Image Generator)](ngen-exe-native-image-generator.md).</span></span>

### <a name="log-assembly-binds-for-native-images"></a><span data-ttu-id="30efb-164">记录本机映像的程序集绑定</span><span class="sxs-lookup"><span data-stu-id="30efb-164">Log assembly binds for native images</span></span>

- <span data-ttu-id="30efb-165">在“日志类别”组中，选择“本机映像”选项按钮 。</span><span class="sxs-lookup"><span data-stu-id="30efb-165">In the **Log Categories** group, select the **Native Images** option button.</span></span>

<span data-ttu-id="30efb-166">下面的日志显示了一个由于在为应用程序创建本机映像时不存在的依赖项引起的失败。</span><span class="sxs-lookup"><span data-stu-id="30efb-166">The following log shows a failure caused by a dependency that did not exist when the native image was created for the application.</span></span> <span data-ttu-id="30efb-167">如果运行时的依赖项不同于运行 Ngen.exe 时的依赖项，则不允许绑定至本机映像。</span><span class="sxs-lookup"><span data-stu-id="30efb-167">If the dependencies at run time differ from the dependencies when Ngen.exe is run, binding to a native image is not allowed.</span></span>

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:22:07 PM) ***

The operation failed.
Bind result: hr = 0x80070002. The system cannot find the file specified.

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\App.exe
--- A detailed error log follows.

LOG: Start binding of native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\App.exe.
LOG: Start validating native image App, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency b, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
WRN: Dependency assembly was not found at ngen time, but is found at binding time. Disallow using this native image.
WRN: No matching native image found.
LOG: Bind to native image assembly did not succeed. Use IL image.
```

<span data-ttu-id="30efb-168">下面的日志显示了一个本机映像绑定失败，发生此失败的原因是，应用程序运行时的计算机上的安全设置不同于创建本机映像时的安全设置。</span><span class="sxs-lookup"><span data-stu-id="30efb-168">The following log shows a native image binding failure that occurred because the security settings on the computer when the application was run were different from the security settings at the time the native image was created.</span></span>

```output
*** Assembly Binder Log Entry  (12/8/2006 @ 5:29:09 PM) ***

The operation failed.
Bind result: hr = 0x80004005. Unspecified error

Assembly manager loaded from:  E:\Windows\Microsoft.NET\Framework64\v2.0.50727\mscorwks.dll
Running under executable  E:\test\Application101622.exe
--- A detailed error log follows.

LOG: Start binding of native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: IL assembly loaded from E:\test\Application101622.exe.
LOG: Start validating native image Application101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Start validating all the dependencies.
LOG: [Level 1]Start validating native image dependency mscorlib, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089.
LOG: Dependency evaluation succeeded.
LOG: [Level 1]Start validating IL dependency Dependency101622, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null.
LOG: Dependency evaluation succeeded.
LOG: Validation of dependencies succeeded.
LOG: Start loading all the dependencies into load context.
LOG: Loading of dependencies succeeded.
LOG: Bind to native image succeeded.
Native image has correct version information.
Attempting to use native image E:\Windows\assembly\NativeImages_v2.0.50727_64\Application101622\1ac7fadabec4f72575d807501e9fdc72\Application101622.ni.exe.
Rejecting native image because it failed the security check. The assembly's permissions must have changed since the time it was ngenned, or it is running with a different security context.
Discarding native image.
```

## <a name="the-log-settings-dialog"></a><span data-ttu-id="30efb-169">“日志设置”对话框</span><span class="sxs-lookup"><span data-stu-id="30efb-169">The Log Settings dialog</span></span>

<span data-ttu-id="30efb-170">可以使用“日志设置”对话框执行下列操作。</span><span class="sxs-lookup"><span data-stu-id="30efb-170">You can use the **Log Settings** dialog to perform the following actions.</span></span>

### <a name="to-disable-logging"></a><span data-ttu-id="30efb-171">禁用日志记录</span><span class="sxs-lookup"><span data-stu-id="30efb-171">To disable logging</span></span>

- <span data-ttu-id="30efb-172">选择“已禁用日志”选项按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-172">Select the **Log disabled** option button.</span></span>  <span data-ttu-id="30efb-173">注意，默认情况下此选项处于选中状态。</span><span class="sxs-lookup"><span data-stu-id="30efb-173">Note that this option is selected by default.</span></span>

### <a name="to-log-assembly-binds-in-exceptions"></a><span data-ttu-id="30efb-174">记录程序集绑定异常</span><span class="sxs-lookup"><span data-stu-id="30efb-174">To log assembly binds in exceptions</span></span>

- <span data-ttu-id="30efb-175">选择“记录异常文本”选项按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-175">Select the **Log in exception text** option button.</span></span> <span data-ttu-id="30efb-176">仅在异常文本中记录最低详细程度的合成日志信息。</span><span class="sxs-lookup"><span data-stu-id="30efb-176">Only the least detailed fusion log information is logged in exception text.</span></span> <span data-ttu-id="30efb-177">若要查看完整信息，请使用其他设置之一。</span><span class="sxs-lookup"><span data-stu-id="30efb-177">To view full information, use one of the other settings.</span></span>

  <span data-ttu-id="30efb-178">请参见有关以非特定域方式加载的程序集的“重要事项”说明。</span><span class="sxs-lookup"><span data-stu-id="30efb-178">See the Important note regarding assemblies that are loaded as domain neutral.</span></span>

### <a name="to-log-assembly-bind-failures"></a><span data-ttu-id="30efb-179">记录程序集绑定失败</span><span class="sxs-lookup"><span data-stu-id="30efb-179">To log assembly bind failures</span></span>

- <span data-ttu-id="30efb-180">选择“记录失败绑定到磁盘”选项按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-180">Select the **Log bind failures to disk** option button.</span></span>

  <span data-ttu-id="30efb-181">请参见有关以非特定域方式加载的程序集的“重要事项”说明。</span><span class="sxs-lookup"><span data-stu-id="30efb-181">See the Important note regarding assemblies that are loaded as domain neutral.</span></span>

### <a name="to-log-all-assembly-binds"></a><span data-ttu-id="30efb-182">记录所有程序集绑定</span><span class="sxs-lookup"><span data-stu-id="30efb-182">To log all assembly binds</span></span>

- <span data-ttu-id="30efb-183">选择“记录所有绑定到磁盘”选项按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-183">Select the **Log all binds to disk** option button.</span></span>

  <span data-ttu-id="30efb-184">请参见有关以非特定域方式加载的程序集的“重要事项”说明。</span><span class="sxs-lookup"><span data-stu-id="30efb-184">See the Important note regarding assemblies that are loaded as domain neutral.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="30efb-185">当程序集以非特定域方式加载时（例如，将 <xref:System.AppDomainSetup.LoaderOptimization%2A> 属性设置为 <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> 或 <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType>），打开日志记录可能会在某些情况下泄露内存。</span><span class="sxs-lookup"><span data-stu-id="30efb-185">When an assembly is loaded as domain neutral, for example by setting the <xref:System.AppDomainSetup.LoaderOptimization%2A> property to <xref:System.LoaderOptimization.MultiDomain?displayProperty=nameWithType> or <xref:System.LoaderOptimization.MultiDomainHost?displayProperty=nameWithType>, turning on logging might leak memory in some cases.</span></span> <span data-ttu-id="30efb-186">如果在将非特定域的模块载入一个应用程序域中时记录日志条目，随后又卸载该应用程序域，则将可能出现这种情况。</span><span class="sxs-lookup"><span data-stu-id="30efb-186">This can happen if a log entry is made when a domain-neutral module is loaded into an application domain, and later the application domain is unloaded.</span></span> <span data-ttu-id="30efb-187">在进程结束前可能不会释放该日志条目。</span><span class="sxs-lookup"><span data-stu-id="30efb-187">The log entry might not be released until the process ends.</span></span> <span data-ttu-id="30efb-188">一些调试器会自动启用日志记录。</span><span class="sxs-lookup"><span data-stu-id="30efb-188">Some debuggers automatically turn on logging.</span></span>

### <a name="to-enable-a-custom-log-path"></a><span data-ttu-id="30efb-189">启用自定义日志路径</span><span class="sxs-lookup"><span data-stu-id="30efb-189">To enable a custom log path</span></span>

1. <span data-ttu-id="30efb-190">选择“启用自定义日志路径”选项按钮。</span><span class="sxs-lookup"><span data-stu-id="30efb-190">Select the **Enable custom log path** option button.</span></span>

2. <span data-ttu-id="30efb-191">在“自定义日志路径”文本框中输入路径。</span><span class="sxs-lookup"><span data-stu-id="30efb-191">Enter the path into the **Custom log path** text box.</span></span>

> [!NOTE]
> <span data-ttu-id="30efb-192">[程序集绑定日志查看器 (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) 使用 Internet Explorer (IE) 缓存来存储其绑定日志。</span><span class="sxs-lookup"><span data-stu-id="30efb-192">The [Assembly Binding Log Viewer (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) uses the Internet Explorer (IE) cache to store its binding log.</span></span> <span data-ttu-id="30efb-193">由于 IE 缓存中偶尔会出现损坏，因此[程序集绑定日志查看器 (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) 有时会停止在查看窗口中显示新的绑定日志。</span><span class="sxs-lookup"><span data-stu-id="30efb-193">Due to occasional corruption in the IE cache, the [Assembly Binding Log Viewer (Fuslogvw.exe)](fuslogvw-exe-assembly-binding-log-viewer.md) can sometimes stop showing new binding logs in the viewing window.</span></span> <span data-ttu-id="30efb-194">受此损坏的影响，.NET 绑定基础结构（合成）将无法写入或读取绑定日志。</span><span class="sxs-lookup"><span data-stu-id="30efb-194">As a result of this corruption, the .NET binding infrastructure (fusion) cannot write to or read from the binding log.</span></span> <span data-ttu-id="30efb-195">（如果使用自定义日志路径，则不会遇到此问题。）若要修复损坏并允许合成再次显示绑定日志，请通过在 IE 的“Internet 选项”对话框中删除临时的 Internet 文件来清除 IE 缓存。</span><span class="sxs-lookup"><span data-stu-id="30efb-195">(This issue is not encountered if you use a custom log path.)  To fix the corruption and allow fusion to show binding logs again, clear the IE cache by deleting temporary internet files from within the IE Internet Options dialog.</span></span>
>
> <span data-ttu-id="30efb-196">如果非托管应用程序通过实现 `IHostAssemblyManager` 和 `IHostAssemblyStore` 接口承载公共语言运行时，则不能将日志条目存储在 wininet 缓存中。</span><span class="sxs-lookup"><span data-stu-id="30efb-196">If your unmanaged application hosts the common language runtime by implementing the `IHostAssemblyManager` and `IHostAssemblyStore` interfaces, log entries can't be stored in the wininet cache.</span></span> <span data-ttu-id="30efb-197">若要查看实现这些接口的自定义宿主的日志条目，你必须指定替代日志路径。</span><span class="sxs-lookup"><span data-stu-id="30efb-197">To view log entries for custom hosts that implement these interfaces, you must specify an alternate log path.</span></span>

### <a name="to-enable-logging-for-apps-running-in-the-windows-app-container"></a><span data-ttu-id="30efb-198">为运行在 Windows 应用程序容器中的应用程序启用日志记录</span><span class="sxs-lookup"><span data-stu-id="30efb-198">To enable logging for apps running in the Windows app container</span></span>

1. <span data-ttu-id="30efb-199">启用自定义日志路径，如上一过程所述。</span><span class="sxs-lookup"><span data-stu-id="30efb-199">Enable a custom log path, as described in the previous procedure.</span></span> <span data-ttu-id="30efb-200">默认情况下，在 Windows 应用程序容器中运行的应用程序对硬盘具有有限的访问权限。</span><span class="sxs-lookup"><span data-stu-id="30efb-200">By default, apps that are running in the Windows app container have limited access to the hard disk.</span></span> <span data-ttu-id="30efb-201">你指定的目录将对应用程序容器中的所有应用程序具有读/写权限。</span><span class="sxs-lookup"><span data-stu-id="30efb-201">The directory you specify will have read/write access for all apps in the app container.</span></span>

2. <span data-ttu-id="30efb-202">选中“启用沉浸式日志记录”复选框。</span><span class="sxs-lookup"><span data-stu-id="30efb-202">Select the **Enable immersive logging** check box.</span></span>

    > [!NOTE]
    > <span data-ttu-id="30efb-203">此框只能在 Windows 8 或更高版本上启用。</span><span class="sxs-lookup"><span data-stu-id="30efb-203">This box is enabled only on Windows 8 or later.</span></span>

## <a name="see-also"></a><span data-ttu-id="30efb-204">请参阅</span><span class="sxs-lookup"><span data-stu-id="30efb-204">See also</span></span>

- <xref:System.TypeLoadException>
- [<span data-ttu-id="30efb-205">工具</span><span class="sxs-lookup"><span data-stu-id="30efb-205">Tools</span></span>](index.md)
- [<span data-ttu-id="30efb-206">全局程序集缓存</span><span class="sxs-lookup"><span data-stu-id="30efb-206">Global Assembly Cache</span></span>](../app-domains/gac.md)
- [<span data-ttu-id="30efb-207">运行时如何定位程序集</span><span class="sxs-lookup"><span data-stu-id="30efb-207">How the Runtime Locates Assemblies</span></span>](../deployment/how-the-runtime-locates-assemblies.md)
- [<span data-ttu-id="30efb-208">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="30efb-208">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
