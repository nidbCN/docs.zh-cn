---
description: 用于控制嵌入在 dotnet 应用程序中的 Windows 资源的 C# 编译器选项。
title: C# 编译器选项 - 资源选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- Win32Resource compiler option [C#]
- Win32Icon compiler option [C#]
- Win32Manifest compiler option [C#]
- NoWin32Manifest compiler option [C#]
- Resources compiler option [C#]
- LinkResources compiler option [C#]
ms.openlocfilehash: fe2da8cae67bc597d2b84a395861a0e4c32c9d72
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482405"
---
# <a name="c-compiler-options-that-specify-resources"></a><span data-ttu-id="08518-103">指定资源的 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="08518-103">C# Compiler Options that specify resources</span></span>

<span data-ttu-id="08518-104">以下选项控制 C# 编译器如何创建或导入 Win32 资源。</span><span class="sxs-lookup"><span data-stu-id="08518-104">The following options control how the C# compiler creates or imports Win32 resources.</span></span> <span data-ttu-id="08518-105">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="08518-105">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="08518-106">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="08518-106">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="08518-107">**Win32Resource** / `-win32res`：指定 Win32 资源文件 (.res)。</span><span class="sxs-lookup"><span data-stu-id="08518-107">**Win32Resource** / `-win32res`: Specify a Win32 resource file (.res).</span></span>
- <span data-ttu-id="08518-108">**Win32Icon** / `-win32icon`：引用来自一个或多个指定程序集文件的元数据。</span><span class="sxs-lookup"><span data-stu-id="08518-108">**Win32Icon** / `-win32icon`: Reference metadata from the specified assembly file or files.</span></span>
- <span data-ttu-id="08518-109">**Win32Manifest** / `-win32manifest`：指定 Win32 清单文件 (.xml)。</span><span class="sxs-lookup"><span data-stu-id="08518-109">**Win32Manifest** / `-win32manifest`: Specify a Win32 manifest file (.xml).</span></span>
- <span data-ttu-id="08518-110">**NoWin32Manifest** / `-nowin32manifest`：不包括默认的 Win32 清单。</span><span class="sxs-lookup"><span data-stu-id="08518-110">**NoWin32Manifest** / `-nowin32manifest`: Don't include the default Win32 manifest.</span></span>
- <span data-ttu-id="08518-111">**Resources** / `-resource`：嵌入指定的资源（简短形式：/res）。</span><span class="sxs-lookup"><span data-stu-id="08518-111">**Resources** / `-resource`: Embed the specified resource (Short form: /res).</span></span>
- <span data-ttu-id="08518-112">**LinkResources** / `-linkresources`：将指定的资源链接到此程序集。</span><span class="sxs-lookup"><span data-stu-id="08518-112">**LinkResources** / `-linkresources`: Link the specified resource to this assembly.</span></span>

## <a name="win32resource"></a><span data-ttu-id="08518-113">Win32Resource</span><span class="sxs-lookup"><span data-stu-id="08518-113">Win32Resource</span></span>

<span data-ttu-id="08518-114">Win32Resource 选项会在输出文件中插入 Win32 资源。</span><span class="sxs-lookup"><span data-stu-id="08518-114">The **Win32Resource** option inserts a Win32 resource in the output file.</span></span>

```xml
<Win32Resource>filename</Win32Resource>
```

<span data-ttu-id="08518-115">`filename` 是要添加到输出文件的资源文件。</span><span class="sxs-lookup"><span data-stu-id="08518-115">`filename` is the resource file that you want to add to your output file.</span></span> <span data-ttu-id="08518-116">Win32 资源可以包含版本或位图（图标）信息，这些信息有助于在文件资源管理器中标识您的应用程序。</span><span class="sxs-lookup"><span data-stu-id="08518-116">A Win32 resource can contain version or bitmap (icon) information that would help identify your application in the File Explorer.</span></span> <span data-ttu-id="08518-117">如果不指定此选项，编译器将根据程序集版本生成版本信息。</span><span class="sxs-lookup"><span data-stu-id="08518-117">If you don't specify this option, the compiler will generate version information based on the assembly version.</span></span>

## <a name="win32icon"></a><span data-ttu-id="08518-118">Win32Icon</span><span class="sxs-lookup"><span data-stu-id="08518-118">Win32Icon</span></span>

<span data-ttu-id="08518-119">Win32Icon 选项在输出文件中插入 .ico 文件，为输出文件提供在文件资源管理器中所需的外观。</span><span class="sxs-lookup"><span data-stu-id="08518-119">The **Win32Icon** option inserts an .ico file in the output file, which gives the output file the desired appearance in the File Explorer.</span></span>
  
```xml
<Win32Icon>filename</Win32Icon>
```

<span data-ttu-id="08518-120">`filename` 是要添加到输出文件的 .ico 文件。</span><span class="sxs-lookup"><span data-stu-id="08518-120">`filename` is the *.ico* file that you want to add to your output file.</span></span> <span data-ttu-id="08518-121">可以使用[资源编译器](/windows/desktop/menurc/resource-compiler)创建 .ico 文件。</span><span class="sxs-lookup"><span data-stu-id="08518-121">An *.ico* file can be created with the [Resource Compiler](/windows/desktop/menurc/resource-compiler).</span></span> <span data-ttu-id="08518-122">在编译 Visual C++ 程序时会调用资源编译器；.ico 文件是从 .rc 文件创建的。</span><span class="sxs-lookup"><span data-stu-id="08518-122">The Resource Compiler is invoked when you compile a Visual C++ program; an *.ico* file is created from the *.rc* file.</span></span>

## <a name="win32manifest"></a><span data-ttu-id="08518-123">Win32Manifest</span><span class="sxs-lookup"><span data-stu-id="08518-123">Win32Manifest</span></span>

<span data-ttu-id="08518-124">使用 Win32Manifest 选项可以指定要嵌入到项目的可移植可执行 (PE) 文件中的用户定义的 Win32 应用程序清单文件。</span><span class="sxs-lookup"><span data-stu-id="08518-124">Use the **Win32Manifest** option to specify a user-defined Win32 application manifest file to be embedded into a project's portable executable (PE) file.</span></span>

```xml
<Win32Manifest>filename</Win32Manifest>
```

<span data-ttu-id="08518-125">`filename` 是自定义清单文件的名称和位置。</span><span class="sxs-lookup"><span data-stu-id="08518-125">`filename` is the name and location of the custom manifest file.</span></span> <span data-ttu-id="08518-126">默认情况下，C# 编译器嵌入可指定“asInvoker”的请求执行级别的应用程序清单。</span><span class="sxs-lookup"><span data-stu-id="08518-126">By default, the C# compiler embeds an application manifest that specifies a requested execution level of "asInvoker".</span></span> <span data-ttu-id="08518-127">它在生成该可执行文件的同一文件夹中创建清单。</span><span class="sxs-lookup"><span data-stu-id="08518-127">It creates the manifest in the same folder in which the executable is built.</span></span> <span data-ttu-id="08518-128">如果要提供自定义清单（例如，指定“highestAvailable”或“requireAdministrator”的请求执行级别的清单），请使用此选项指定文件名。</span><span class="sxs-lookup"><span data-stu-id="08518-128">If you want to supply a custom manifest, for example to specify a requested execution level of "highestAvailable" or "requireAdministrator," use this option to specify the name of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="08518-129">此选项和 Win32Resources 选项是互斥的。</span><span class="sxs-lookup"><span data-stu-id="08518-129">This option and the **Win32Resources** option are mutually exclusive.</span></span> <span data-ttu-id="08518-130">如果尝试在同一命令行中同时使用这两个选项，将收到一个生成错误。</span><span class="sxs-lookup"><span data-stu-id="08518-130">If you try to use both options in the same command line you will get a build error.</span></span>

<span data-ttu-id="08518-131">如果应用程序没有用于指定请求执行级别的应用程序清单，将受到 Windows 中“用户帐户控制”功能下的文件和注册表虚拟化的影响。</span><span class="sxs-lookup"><span data-stu-id="08518-131">An application that has no application manifest that specifies a requested execution level will be subject to file and registry virtualization under the User Account Control feature in Windows.</span></span> <span data-ttu-id="08518-132">有关详细信息，请参阅[用户帐户控制](/windows/access-protection/user-account-control/user-account-control-overview)。</span><span class="sxs-lookup"><span data-stu-id="08518-132">For more information, see [User Account Control](/windows/access-protection/user-account-control/user-account-control-overview).</span></span>

<span data-ttu-id="08518-133">如果满足下列任一条件，则应用程序会受到虚拟化的影响：</span><span class="sxs-lookup"><span data-stu-id="08518-133">Your application will be subject to virtualization if either of these conditions is true:</span></span>
  
- <span data-ttu-id="08518-134">使用 NoWin32Manifest 选项，并且在随后的生成步骤中未提供清单，或者没有通过使用 Win32Resource 选项将其包含在 Windows 资源 (.res) 文件中。</span><span class="sxs-lookup"><span data-stu-id="08518-134">You use the **NoWin32Manifest** option and you don't provide a manifest in a later build step or as part of a Windows Resource (*.res*) file by using the **Win32Resource** option.</span></span>
- <span data-ttu-id="08518-135">提供的自定义清单未指定请求执行级别。</span><span class="sxs-lookup"><span data-stu-id="08518-135">You provide a custom manifest that doesn't specify a requested execution level.</span></span>

<span data-ttu-id="08518-136">Visual Studio 创建默认 .manifest 文件，并将它与可执行文件一起存储在“调试”和“发布”目录中。</span><span class="sxs-lookup"><span data-stu-id="08518-136">Visual Studio creates a default *.manifest* file and stores it in the debug and release directories alongside the executable file.</span></span> <span data-ttu-id="08518-137">可以用任意文本编辑器创建一个清单，然后将该文件添加到项目中，从而添加自定义清单。</span><span class="sxs-lookup"><span data-stu-id="08518-137">You can add a custom manifest by creating one in any text editor and then adding the file to the project.</span></span> <span data-ttu-id="08518-138">或者，也可以右键单击“解决方案资源管理器”中的“项目”图标，选择“添加新项”，然后选择“应用程序清单文件”。</span><span class="sxs-lookup"><span data-stu-id="08518-138">Or, you can right-click the **Project** icon in **Solution Explorer**, select **Add New Item**, and then select **Application Manifest File**.</span></span> <span data-ttu-id="08518-139">添加完新的或现有清单文件后，该文件将显示在“清单”下拉列表中。</span><span class="sxs-lookup"><span data-stu-id="08518-139">After you've added your new or existing manifest file, it will appear in the **Manifest** drop down list.</span></span> <span data-ttu-id="08518-140">有关详细信息，请参阅[“项目设计器”->“应用程序”页 (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp)。</span><span class="sxs-lookup"><span data-stu-id="08518-140">For more information, see [Application Page, Project Designer (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp).</span></span>

<span data-ttu-id="08518-141">提供应用程序清单的操作可以作为自定义生成后步骤，也可以通过使用 NoWin32Manifest 选项作为 Win32 资源文件的组成部分。</span><span class="sxs-lookup"><span data-stu-id="08518-141">You can provide the application manifest as a custom post-build step or as part of a Win32 resource file by using the **NoWin32Manifest** option.</span></span> <span data-ttu-id="08518-142">如果希望应用程序受到 Windows Vista 的文件或注册表虚拟化的影响，请使用该选项。</span><span class="sxs-lookup"><span data-stu-id="08518-142">Use that same option if you want your application to be subject to file or registry virtualization on Windows Vista.</span></span>
  
## <a name="nowin32manifest"></a><span data-ttu-id="08518-143">NoWin32Manifest</span><span class="sxs-lookup"><span data-stu-id="08518-143">NoWin32Manifest</span></span>

<span data-ttu-id="08518-144">使用 NoWin32Manifest 选项可指示编译器不将任何应用程序清单嵌入到可执行文件中。</span><span class="sxs-lookup"><span data-stu-id="08518-144">Use the **NoWin32Manifest** option to instruct the compiler not to embed any application manifest into the executable file.</span></span>

```xml  
<NoWin32Manifest />
```

<span data-ttu-id="08518-145">使用此选项时，除非在 Win32 资源文件或以后的生成步骤中提供应用程序清单，否则应用程序会受到 Windows Vista 上虚拟化的影响。</span><span class="sxs-lookup"><span data-stu-id="08518-145">When this option is used, the application will be subject to virtualization on Windows Vista unless you provide an application manifest in a Win32 Resource file or during a later build step.</span></span>

<span data-ttu-id="08518-146">在 Visual Studio 的“应用程序属性”页中，通过在“清单”下拉列表中选择“创建不带清单的应用程序”选项来设置此选项。</span><span class="sxs-lookup"><span data-stu-id="08518-146">In Visual Studio, set this option in the **Application Property** page by selecting the **Create Application Without a Manifest** option in the **Manifest** drop down list.</span></span> <span data-ttu-id="08518-147">有关详细信息，请参阅[“项目设计器”->“应用程序”页 (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp)。</span><span class="sxs-lookup"><span data-stu-id="08518-147">For more information, see [Application Page, Project Designer (C#)](/visualstudio/ide/reference/application-page-project-designer-csharp).</span></span>

## <a name="resources"></a><span data-ttu-id="08518-148">资源</span><span class="sxs-lookup"><span data-stu-id="08518-148">Resources</span></span>

<span data-ttu-id="08518-149">将指定资源嵌入输出文件。</span><span class="sxs-lookup"><span data-stu-id="08518-149">Embeds the specified resource into the output file.</span></span>

```xml
<Resources Include=filename>
  <LogicalName>identifier</LogicalName>
  <Access>accessibility-modifier</Access>
</Resources>
```

<span data-ttu-id="08518-150">`filename` 是要嵌入到输出文件的 .NET 资源文件。</span><span class="sxs-lookup"><span data-stu-id="08518-150">`filename` is the .NET resource file that you want to embed in the output file.</span></span> <span data-ttu-id="08518-151">`identifier`（可选）是资源的逻辑名称；用于加载资源的名称。</span><span class="sxs-lookup"><span data-stu-id="08518-151">`identifier` (optional) is the logical name for the resource; the name that is used to load the resource.</span></span> <span data-ttu-id="08518-152">默认值是文件的名称。</span><span class="sxs-lookup"><span data-stu-id="08518-152">The default is the name of the file.</span></span> <span data-ttu-id="08518-153">`accessibility-modifier`（可选）是资源的可访问性：public 或 private。</span><span class="sxs-lookup"><span data-stu-id="08518-153">`accessibility-modifier` (optional) is the accessibility of the resource: public or private.</span></span> <span data-ttu-id="08518-154">默认值为 public。</span><span class="sxs-lookup"><span data-stu-id="08518-154">The default is public.</span></span> <span data-ttu-id="08518-155">默认情况下，如果使用 C# 编译器创建资源，则这些资源在程序集中是公有的。</span><span class="sxs-lookup"><span data-stu-id="08518-155">By default, resources are public in the assembly when they're created by using the C# compiler.</span></span> <span data-ttu-id="08518-156">若要使资源变为私有，请将 `private` 指定为可访问性修饰符。</span><span class="sxs-lookup"><span data-stu-id="08518-156">To make the resources private, specify `private` as the accessibility modifier.</span></span> <span data-ttu-id="08518-157">不允许使用 `public` 或 `private` 以外的任何其他可访问性。</span><span class="sxs-lookup"><span data-stu-id="08518-157">No other accessibility other than `public` or `private` is allowed.</span></span> <span data-ttu-id="08518-158">例如，如果 `filename` 是由 [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。</span><span class="sxs-lookup"><span data-stu-id="08518-158">If `filename` is a .NET resource file created, for example, by [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) or in the development environment, it can be accessed with members in the <xref:System.Resources> namespace.</span></span> <span data-ttu-id="08518-159">有关详细信息，请参阅 <xref:System.Resources.ResourceManager?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="08518-159">For more information, see <xref:System.Resources.ResourceManager?displayProperty=nameWithType>.</span></span> <span data-ttu-id="08518-160">对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource` 方法在运行时访问资源。</span><span class="sxs-lookup"><span data-stu-id="08518-160">For all other resources, use the `GetManifestResource` methods in the <xref:System.Reflection.Assembly> class to access the resource at run time.</span></span> <span data-ttu-id="08518-161">输出文件中资源的顺序由项目文件中所指定的顺序决定。</span><span class="sxs-lookup"><span data-stu-id="08518-161">The order of the resources in the output file is determined from the order specified in the project file.</span></span>  
  
## <a name="linkresources"></a><span data-ttu-id="08518-162">LinkResources</span><span class="sxs-lookup"><span data-stu-id="08518-162">LinkResources</span></span>

<span data-ttu-id="08518-163">在输出文件中创建指向 .NET 资源的链接。</span><span class="sxs-lookup"><span data-stu-id="08518-163">Creates a link to a .NET resource in the output file.</span></span> <span data-ttu-id="08518-164">不会在输出文件中添加资源文件。</span><span class="sxs-lookup"><span data-stu-id="08518-164">The resource file isn't added to the output file.</span></span> <span data-ttu-id="08518-165">LinkResources 不同于会在输出文件中嵌入资源文件的 Resource 选项。</span><span class="sxs-lookup"><span data-stu-id="08518-165">**LinkResources** differs from the **Resource** option, which does embed a resource file in the output file.</span></span>

```xml
<LinkResources Include=filename>
  <LogicalName>identifier</LogicalName>
  <Access>accessibility-modifier</Access>
</LinkResources>
```

<span data-ttu-id="08518-166">`filename` 是要从程序集链接到的 .NET 资源文件。</span><span class="sxs-lookup"><span data-stu-id="08518-166">`filename` is the .NET resource file to which you want to link from the assembly.</span></span> <span data-ttu-id="08518-167">`identifier`（可选）是资源的逻辑名称；用于加载资源的名称。</span><span class="sxs-lookup"><span data-stu-id="08518-167">`identifier` (optional) is the logical name for the resource; the name that is used to load the resource.</span></span> <span data-ttu-id="08518-168">默认值是文件的名称。</span><span class="sxs-lookup"><span data-stu-id="08518-168">The default is the name of the file.</span></span> <span data-ttu-id="08518-169">`accessibility-modifier`（可选）是资源的可访问性：public 或 private。</span><span class="sxs-lookup"><span data-stu-id="08518-169">`accessibility-modifier` (optional) is the accessibility of the resource: public or private.</span></span> <span data-ttu-id="08518-170">默认值为 public。</span><span class="sxs-lookup"><span data-stu-id="08518-170">The default is public.</span></span> <span data-ttu-id="08518-171">默认情况下，如果使用 C# 编译器创建链接资源，则这些资源在程序集中是公有的。</span><span class="sxs-lookup"><span data-stu-id="08518-171">By default, linked resources are public in the assembly when they're created with the C# compiler.</span></span> <span data-ttu-id="08518-172">若要使资源变为私有，请将 `private` 指定为可访问性修饰符。</span><span class="sxs-lookup"><span data-stu-id="08518-172">To make the resources private, specify `private` as the accessibility modifier.</span></span> <span data-ttu-id="08518-173">不允许使用 `public` 或 `private` 以外的任何其他修饰符。</span><span class="sxs-lookup"><span data-stu-id="08518-173">No other modifier other than `public` or `private` is allowed.</span></span> <span data-ttu-id="08518-174">例如，如果 `filename` 是由 [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) 创建的或在开发环境中创建的 .NET 资源文件，则可使用 <xref:System.Resources> 命名空间中的成员来访问它。</span><span class="sxs-lookup"><span data-stu-id="08518-174">If `filename` is a .NET resource file created, for example, by [Resgen.exe](../../../framework/tools/resgen-exe-resource-file-generator.md) or in the development environment, it can be accessed with members in the <xref:System.Resources> namespace.</span></span> <span data-ttu-id="08518-175">有关详细信息，请参阅 <xref:System.Resources.ResourceManager?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="08518-175">For more information, see <xref:System.Resources.ResourceManager?displayProperty=nameWithType>.</span></span> <span data-ttu-id="08518-176">对于所有其他资源，请使用 <xref:System.Reflection.Assembly> 类中的 `GetManifestResource` 方法在运行时访问资源。</span><span class="sxs-lookup"><span data-stu-id="08518-176">For all other resources, use the `GetManifestResource` methods in the <xref:System.Reflection.Assembly> class to access the resource at run time.</span></span> <span data-ttu-id="08518-177">`filename` 中指定的文件可为任何格式。</span><span class="sxs-lookup"><span data-stu-id="08518-177">The file specified in `filename` can be any format.</span></span> <span data-ttu-id="08518-178">例如，你可能希望生成程序集的本机 DLL 部分，从而可将它安装到全局程序集缓存中，并且可从该程序集中的托管代码访问它。</span><span class="sxs-lookup"><span data-stu-id="08518-178">For example, you may want to make a native DLL part of the assembly, so that it can be installed into the global assembly cache and accessed from managed code in the assembly.</span></span> <span data-ttu-id="08518-179">可在程序集链接器中执行相同的操作。</span><span class="sxs-lookup"><span data-stu-id="08518-179">You can do the same thing in the Assembly Linker.</span></span> <span data-ttu-id="08518-180">有关详细信息，请参阅 [Al.exe（程序集链接器）](../../../framework/tools/al-exe-assembly-linker.md)和[使用程序集和全局程序集缓存](../../../framework/app-domains/working-with-assemblies-and-the-gac.md)。</span><span class="sxs-lookup"><span data-stu-id="08518-180">For more information, see [Al.exe (Assembly Linker)](../../../framework/tools/al-exe-assembly-linker.md) and [Working with Assemblies and the Global Assembly Cache](../../../framework/app-domains/working-with-assemblies-and-the-gac.md).</span></span>
