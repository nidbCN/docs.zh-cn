---
description: 用于控制编译输入文件的 C# 编译器选项。 这些选项指定编译器如何从依赖程序集和模块读取元数据。
title: C# 编译器选项 - 输入文件选项
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- References compiler option [C#]
- AddModules compiler option [C#]
- EmbedInteropTypes compiler option [C#]
ms.openlocfilehash: 819e2322720782b94bd744e00c602221f023c0d8
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103482431"
---
# <a name="c-compiler-options-that-specify-inputs"></a><span data-ttu-id="8f67c-104">可指定输入的 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="8f67c-104">C# Compiler Options that specify inputs</span></span>

<span data-ttu-id="8f67c-105">以下选项控制编译器输入。</span><span class="sxs-lookup"><span data-stu-id="8f67c-105">The following options control compiler inputs.</span></span> <span data-ttu-id="8f67c-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="8f67c-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="8f67c-107">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="8f67c-107">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="8f67c-108">**References** / `-reference` 或 `-references`：引用来自一个或多个指定程序集文件的元数据。</span><span class="sxs-lookup"><span data-stu-id="8f67c-108">**References** / `-reference` or `-references`: Reference metadata from the specified assembly file or files.</span></span>
- <span data-ttu-id="8f67c-109">**AddModules** / `-addmodule`：添加模块（将使用 `target:module` 创建的模块添加到此程序集。）</span><span class="sxs-lookup"><span data-stu-id="8f67c-109">**AddModules** / `-addmodule`: Add a module (created with `target:module` to this assembly.)</span></span>
- <span data-ttu-id="8f67c-110">**EmbedInteropTypes** / `-link`：嵌入指定互操作程序集文件中的元数据。</span><span class="sxs-lookup"><span data-stu-id="8f67c-110">**EmbedInteropTypes** / `-link`: Embed metadata from the specified interop assembly files.</span></span>

## <a name="references"></a><span data-ttu-id="8f67c-111">参考</span><span class="sxs-lookup"><span data-stu-id="8f67c-111">References</span></span>

<span data-ttu-id="8f67c-112">References 选项使编译器将指定文件中的 [public](../keywords/public.md) 类型信息导入当前项目，从而使你可从指定的程序集文件中引用元数据。</span><span class="sxs-lookup"><span data-stu-id="8f67c-112">The **References** option causes the compiler to import [public](../keywords/public.md) type information in the specified file into the current project, enabling you to reference metadata from the specified assembly files.</span></span>

```xml
<Reference Include="filename" />
```

 <span data-ttu-id="8f67c-113">`filename` 是包含程序集清单的文件的名称。</span><span class="sxs-lookup"><span data-stu-id="8f67c-113">`filename` is the name of a file that contains an assembly manifest.</span></span> <span data-ttu-id="8f67c-114">若要导入多个文件，请为每个文件加上一个单独的 Reference 元素。</span><span class="sxs-lookup"><span data-stu-id="8f67c-114">To import more than one file, include a separate **Reference** element for each file.</span></span> <span data-ttu-id="8f67c-115">可以定义一个别名作为 Reference 元素的子元素：</span><span class="sxs-lookup"><span data-stu-id="8f67c-115">You can define an alias as a child element of the **Reference** element:</span></span>

```xml
<Reference Include="filename.dll">
  <Aliases>LS</Aliases>
</Reference>
```

<span data-ttu-id="8f67c-116">在上面的示例中，`LS` 是表示根命名空间的有效 C# 标识符，该根命名空间将包含程序集 filename.dll 中的所有命名空间。</span><span class="sxs-lookup"><span data-stu-id="8f67c-116">In the previous example, `LS` is the valid C# identifier that represents a root namespace that will contain all namespaces in the assembly *filename.dll*.</span></span> <span data-ttu-id="8f67c-117">导入的文件必须包含一个清单。</span><span class="sxs-lookup"><span data-stu-id="8f67c-117">The files you import must contain a manifest.</span></span> <span data-ttu-id="8f67c-118">使用 [AdditionalLibPaths](advanced.md#additionallibpaths) 指定一个或多个程序集引用所在的目录。</span><span class="sxs-lookup"><span data-stu-id="8f67c-118">Use [**AdditionalLibPaths**](advanced.md#additionallibpaths) to specify the directory in which one or more of your assembly references is located.</span></span> <span data-ttu-id="8f67c-119">[AdditionalLibPaths](advanced.md#additionallibpaths) 主题还讨论了编译器在其中搜索程序集的目录。</span><span class="sxs-lookup"><span data-stu-id="8f67c-119">The [**AdditionalLibPaths**](advanced.md#additionallibpaths) topic also discusses the directories in which the compiler searches for assemblies.</span></span> <span data-ttu-id="8f67c-120">为使编译器可以识别程序集（而不是模块）中的某个类型，需要强制解析此类型，这可以通过定义此类型的实例来完成。</span><span class="sxs-lookup"><span data-stu-id="8f67c-120">In order for the compiler to recognize a type in an assembly, and not in a module, it needs to be forced to resolve the type, which you can do by defining an instance of the type.</span></span> <span data-ttu-id="8f67c-121">还可通过其他方法为编译器解析程序集中的类型名称：例如，如果从程序集中继承类型，编译器就能识别类型名称。</span><span class="sxs-lookup"><span data-stu-id="8f67c-121">There are other ways to resolve type names in an assembly for the compiler: for example, if you inherit from a type in an assembly, the type name will then be recognized by the compiler.</span></span> <span data-ttu-id="8f67c-122">有时，需要在一个程序集内引用同一组件的两个不同版本。</span><span class="sxs-lookup"><span data-stu-id="8f67c-122">Sometimes it is necessary to reference two different versions of the same component from within one assembly.</span></span> <span data-ttu-id="8f67c-123">为此，请在每个文件的 References 元素上使用 Aliases 元素，以区分这两个文件。</span><span class="sxs-lookup"><span data-stu-id="8f67c-123">To do this, use the **Aliases** element on the **References** element for each file to distinguish between the two files.</span></span> <span data-ttu-id="8f67c-124">此别名将用作组件名的限定符，并解析为其中一个文件中的组件。</span><span class="sxs-lookup"><span data-stu-id="8f67c-124">This alias will be used as a qualifier for the component name, and will resolve to the component in one of the files.</span></span>

> [!NOTE]
> <span data-ttu-id="8f67c-125">在 Visual Studio 中，请使用“添加引用”命令。</span><span class="sxs-lookup"><span data-stu-id="8f67c-125">In Visual Studio, use the **Add Reference** command.</span></span> <span data-ttu-id="8f67c-126">有关详细信息，请参阅 [How to: Add or Remove References By Using the Reference Manager](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager)。</span><span class="sxs-lookup"><span data-stu-id="8f67c-126">For more information, see [How to: Add or Remove References By Using the Reference Manager](/visualstudio/ide/how-to-add-or-remove-references-by-using-the-reference-manager).</span></span>

## <a name="addmodules"></a><span data-ttu-id="8f67c-127">AddModules</span><span class="sxs-lookup"><span data-stu-id="8f67c-127">AddModules</span></span>

<span data-ttu-id="8f67c-128">此选项将添加一个模块，该模块通过将 `<TargetType>module</TargetType>` 切换到当前编译进行创建：</span><span class="sxs-lookup"><span data-stu-id="8f67c-128">This option adds a module that was created with the `<TargetType>module</TargetType>` switch to the current compilation:</span></span>

```xml
<AddModule Include=file1 />
<AddModule Include=file2 />
```

<span data-ttu-id="8f67c-129">其中 `file`、`file2` 是包含元数据的输出文件。</span><span class="sxs-lookup"><span data-stu-id="8f67c-129">Where `file`, `file2` are output files that contain metadata.</span></span> <span data-ttu-id="8f67c-130">该文件不能包含程序集清单。</span><span class="sxs-lookup"><span data-stu-id="8f67c-130">The file can't contain an assembly manifest.</span></span> <span data-ttu-id="8f67c-131">若要导入多个文件，请用逗号或分号将文件名隔开。</span><span class="sxs-lookup"><span data-stu-id="8f67c-131">To import more than one file, separate file names with either a comma or a semicolon.</span></span> <span data-ttu-id="8f67c-132">通过 AddModules 添加的所有模块在运行时必须位于与输出文件相同的目录中。</span><span class="sxs-lookup"><span data-stu-id="8f67c-132">All modules added with **AddModules** must be in the same directory as the output file at run time.</span></span> <span data-ttu-id="8f67c-133">也就是说，在编译时可在任何目录中指定模块，但在运行时该模块必须位于应用程序目录中。</span><span class="sxs-lookup"><span data-stu-id="8f67c-133">That is, you can specify a module in any directory at compile time but the module must be in the application directory at run time.</span></span> <span data-ttu-id="8f67c-134">如果在运行时该模块不位于应用程序目录中，你将收到 <xref:System.TypeLoadException>。</span><span class="sxs-lookup"><span data-stu-id="8f67c-134">If the module isn't in the application directory at run time, you'll get a <xref:System.TypeLoadException>.</span></span> <span data-ttu-id="8f67c-135">`file` 不能包含程序集。</span><span class="sxs-lookup"><span data-stu-id="8f67c-135">`file` can't contain an assembly.</span></span> <span data-ttu-id="8f67c-136">例如，如果输出文件是使用 module 的 [TargetType](output.md#targettype) 创建的，那么它的元数据可通过 AddModules 导入。</span><span class="sxs-lookup"><span data-stu-id="8f67c-136">For example, if the output file was created with [**TargetType**](output.md#targettype) option of **module**, its metadata can be imported with **AddModules**.</span></span>

<span data-ttu-id="8f67c-137">如果输出文件是使用 [TargetType](output.md#targettype) 选项而不是 module 创建的，则它的元数据不能通过 AddModules 导入，但可以通过 [References](#references) 选项导入。</span><span class="sxs-lookup"><span data-stu-id="8f67c-137">If the output file was created with a [**TargetType**](output.md#targettype) option other than **module**, its metadata cannot be imported with **AddModules** but can be imported with the [**References**](#references) option.</span></span>

## <a name="embedinteroptypes"></a><span data-ttu-id="8f67c-138">EmbedInteropTypes</span><span class="sxs-lookup"><span data-stu-id="8f67c-138">EmbedInteropTypes</span></span>

<span data-ttu-id="8f67c-139">使编译器让指定程序集中的 COM 类型信息可供当前正在编译的项目使用。</span><span class="sxs-lookup"><span data-stu-id="8f67c-139">Causes the compiler to make COM type information in the specified assemblies available to the project that you are currently compiling.</span></span>

```xml
<References>
  <EmbedInteropTypes>file1;file2;file3</EmbedInteropTypes>
</References>
```

<span data-ttu-id="8f67c-140">其中 `file1;file2;file3` 是以分号分隔的程序集文件名的列表。</span><span class="sxs-lookup"><span data-stu-id="8f67c-140">Where  `file1;file2;file3` is a semicolon-delimited list of assembly file names.</span></span> <span data-ttu-id="8f67c-141">如果文件名包含空格，则将名称括在引号内。</span><span class="sxs-lookup"><span data-stu-id="8f67c-141">If the file name contains a space, enclose the name in quotation marks.</span></span> <span data-ttu-id="8f67c-142">使用 EmbedInteropTypes 选项可以部署具有嵌入类型信息的应用程序。</span><span class="sxs-lookup"><span data-stu-id="8f67c-142">The **EmbedInteropTypes** option enables you to deploy an application that has embedded type information.</span></span> <span data-ttu-id="8f67c-143">应用程序随后可以使用运行时程序集中实现嵌入类型信息的类型，而无需引用运行时程序集。</span><span class="sxs-lookup"><span data-stu-id="8f67c-143">The application can then use types in a runtime assembly that implement the embedded type information without requiring a reference to the runtime assembly.</span></span> <span data-ttu-id="8f67c-144">如果发布了各种版本的运行时程序集，则包含嵌入类型信息的应用程序可以使用各种版本，而无需重新编译。</span><span class="sxs-lookup"><span data-stu-id="8f67c-144">If various versions of the runtime assembly are published, the application that contains the embedded type information can work with the various versions without having to be recompiled.</span></span> <span data-ttu-id="8f67c-145">有关示例，请参阅[演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)。</span><span class="sxs-lookup"><span data-stu-id="8f67c-145">For an example, see [Walkthrough: Embedding Types from Managed Assemblies](../../../standard/assembly/embed-types-visual-studio.md).</span></span>

<span data-ttu-id="8f67c-146">当你使用 COM 互操作时，EmbedInteropTypes 选项尤其有用。</span><span class="sxs-lookup"><span data-stu-id="8f67c-146">Using the **EmbedInteropTypes** option is especially useful when you're working with COM interop.</span></span> <span data-ttu-id="8f67c-147">可以嵌入 COM 类型，以便应用程序在目标计算机上不再需要主互操作程序集 (PIA)。</span><span class="sxs-lookup"><span data-stu-id="8f67c-147">You can embed COM types so that your application no longer requires a primary interop assembly (PIA) on the target computer.</span></span> <span data-ttu-id="8f67c-148">EmbedInteropTypes 选项指示编译器将引用的互操作程序集中的 COM 类型信息嵌入到生成的已编译代码中。</span><span class="sxs-lookup"><span data-stu-id="8f67c-148">The **EmbedInteropTypes** option instructs the compiler to embed the COM type information from the referenced interop assembly into the resulting compiled code.</span></span> <span data-ttu-id="8f67c-149">COM 类型由 CLSID (GUID) 值进行标识。</span><span class="sxs-lookup"><span data-stu-id="8f67c-149">The COM type is identified by the CLSID (GUID) value.</span></span> <span data-ttu-id="8f67c-150">因此，应用程序可以在安装了具有相同 CLSID 值的相同 COM 类型的目标计算机上运行。</span><span class="sxs-lookup"><span data-stu-id="8f67c-150">As a result, your application can run on a target computer that has installed the same COM types with the same CLSID values.</span></span> <span data-ttu-id="8f67c-151">自动执行 Microsoft Office 的应用程序是一个很好的示例。</span><span class="sxs-lookup"><span data-stu-id="8f67c-151">Applications that automate Microsoft Office are a good example.</span></span> <span data-ttu-id="8f67c-152">由于 Office 等应用程序通常在不同版本间保持相同的 CLSID 值，因此只要在目标计算机上安装了 .NET Framework 4 或更高版本，并且应用程序使用引用的 COM 类型中包含的方法、属性或事件，应用程序便可以使用引用的 COM 类型。</span><span class="sxs-lookup"><span data-stu-id="8f67c-152">Because applications like Office usually keep the same CLSID value across different versions, your application can use the referenced COM types as long as .NET Framework 4 or later is installed on the target computer and your application uses methods, properties, or events that are included in the referenced COM types.</span></span> <span data-ttu-id="8f67c-153">EmbedInteropTypes 选项只嵌入接口、结构和委托。</span><span class="sxs-lookup"><span data-stu-id="8f67c-153">The **EmbedInteropTypes** option embeds only interfaces, structures, and delegates.</span></span> <span data-ttu-id="8f67c-154">不支持嵌入 COM 类。</span><span class="sxs-lookup"><span data-stu-id="8f67c-154">Embedding COM classes isn't supported.</span></span>

> [!NOTE]
> <span data-ttu-id="8f67c-155">在代码中创建嵌入 COM 类型的实例时，必须使用适当的接口创建该实例。</span><span class="sxs-lookup"><span data-stu-id="8f67c-155">When you create an instance of an embedded COM type in your code, you must create the instance by using the appropriate interface.</span></span> <span data-ttu-id="8f67c-156">尝试使用组件类创建嵌入 COM 类型的实例会导致错误。</span><span class="sxs-lookup"><span data-stu-id="8f67c-156">Attempting to create an instance of an embedded COM type by using the CoClass causes an error.</span></span>

<span data-ttu-id="8f67c-157">与 [References](#references) 编译器选项一样，EmbedInteropTypes 编译器选项使用 Csc.rsp 响应文件，该文件引用常用的 .NET 程序集。</span><span class="sxs-lookup"><span data-stu-id="8f67c-157">Like the [**References**](#references) compiler option, the **EmbedInteropTypes** compiler option uses the Csc.rsp response file, which references frequently used .NET assemblies.</span></span> <span data-ttu-id="8f67c-158">如果你不希望编译器使用 Csc.rsp 文件，可以使用 [NoConfig](miscellaneous.md#noconfig) 编译器选项。</span><span class="sxs-lookup"><span data-stu-id="8f67c-158">Use the [**NoConfig**](miscellaneous.md#noconfig) compiler option if you don't want the compiler to use the Csc.rsp file.</span></span>

[!code-csharp[VbLinkCompilerCS#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#1)]

<span data-ttu-id="8f67c-159">对于具有类型是从互操作程序集嵌入的泛型参数的类型，如果该类型来自外部程序集，则无法使用这种类型。</span><span class="sxs-lookup"><span data-stu-id="8f67c-159">Types that have a generic parameter whose type is embedded from an interop assembly cannot be used if that type is from an external assembly.</span></span> <span data-ttu-id="8f67c-160">此限制不适用于接口。</span><span class="sxs-lookup"><span data-stu-id="8f67c-160">This restriction doesn't apply to interfaces.</span></span> <span data-ttu-id="8f67c-161">例如，考虑在 <xref:Microsoft.Office.Interop.Excel> 程序集中定义的 <xref:Microsoft.Office.Interop.Excel.Range> 接口。</span><span class="sxs-lookup"><span data-stu-id="8f67c-161">For example, consider the <xref:Microsoft.Office.Interop.Excel.Range> interface that is defined in the <xref:Microsoft.Office.Interop.Excel> assembly.</span></span> <span data-ttu-id="8f67c-162">如果某个库从 <xref:Microsoft.Office.Interop.Excel> 程序集嵌入互操作类型，并且公开的一个方法返回具有类型是 <xref:Microsoft.Office.Interop.Excel.Range> 接口的参数的泛型类型，则该方法必须返回泛型接口，如下面的代码示例所示。</span><span class="sxs-lookup"><span data-stu-id="8f67c-162">If a library embeds interop types from the <xref:Microsoft.Office.Interop.Excel> assembly and exposes a method that returns a generic type that has a parameter whose type is the <xref:Microsoft.Office.Interop.Excel.Range> interface, that method must return a generic interface, as shown in the following code example.</span></span>

[!code-csharp[VbLinkCompilerCS#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/utility.cs)]

<span data-ttu-id="8f67c-163">在下面的示例中，客户端代码可以调用返回 <xref:System.Collections.IList> 泛型接口的方法而不会出现错误。</span><span class="sxs-lookup"><span data-stu-id="8f67c-163">In the following example, client code can call the method that returns the <xref:System.Collections.IList> generic interface without error.</span></span>

[!code-csharp[VbLinkCompilerCS#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vblinkcompilercs/cs/program.cs#5)]
