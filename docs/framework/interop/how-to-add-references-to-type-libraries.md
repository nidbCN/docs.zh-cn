---
title: 如何：添加对类型库的引用
description: 了解如何在 Visual Studio 中或对命令行编译添加对类型库的引用。
ms.date: 03/30/2017
helpviewer_keywords:
- importing type library
- interop assemblies, generating
- type libraries
- COM interop, importing type library
ms.assetid: f5cfa6ba-cc25-4017-82cd-ba7391859113
ms.openlocfilehash: b5e58c5943eba8db7497b4db56bfbd99b17b1043
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477623"
---
# <a name="how-to-add-references-to-type-libraries"></a><span data-ttu-id="d1fda-103">如何：添加对类型库的引用</span><span class="sxs-lookup"><span data-stu-id="d1fda-103">How to: Add References to Type Libraries</span></span>

<span data-ttu-id="d1fda-104">当添加对类型库的引用时，Visual Studio 将生成包含元数据的互操作程序集。</span><span class="sxs-lookup"><span data-stu-id="d1fda-104">Visual Studio generates an interop assembly containing metadata when you add a reference to a type library.</span></span> <span data-ttu-id="d1fda-105">如果主互操作程序集可用，则 Visual Studio 在生成新的互操作程序集之前将使用现有程序集。</span><span class="sxs-lookup"><span data-stu-id="d1fda-105">If a primary interop assembly is available, Visual Studio uses the existing assembly before generating a new interop assembly.</span></span>  
  
### <a name="to-add-a-reference-to-a-type-library-in-visual-studio"></a><span data-ttu-id="d1fda-106">在 Visual Studio 中添加对类型库的引用</span><span class="sxs-lookup"><span data-stu-id="d1fda-106">To add a reference to a type library in Visual Studio</span></span>  
  
1. <span data-ttu-id="d1fda-107">在计算机上安装 COM DLL 或 EXE 文件，除非 Windows Setup.exe 文件会为你执行此安装。</span><span class="sxs-lookup"><span data-stu-id="d1fda-107">Install the COM DLL or EXE file on your computer, unless a Windows Setup.exe file performs the installation for you.</span></span>  
  
2. <span data-ttu-id="d1fda-108">选择“项目”、“添加引用” 。</span><span class="sxs-lookup"><span data-stu-id="d1fda-108">Choose **Project**, **Add Reference**.</span></span>  
  
3. <span data-ttu-id="d1fda-109">在引用管理器中，选择“COM”。</span><span class="sxs-lookup"><span data-stu-id="d1fda-109">In the Reference Manager, choose **COM**.</span></span>  
  
4. <span data-ttu-id="d1fda-110">从列表中选择类型库，或通过浏览选择 .tlb 文件。</span><span class="sxs-lookup"><span data-stu-id="d1fda-110">Select the type library from the list, or browse for the .tlb file.</span></span>  
  
5. <span data-ttu-id="d1fda-111">选择 **“确定”** 。</span><span class="sxs-lookup"><span data-stu-id="d1fda-111">Choose **OK**.</span></span>  
  
6. <span data-ttu-id="d1fda-112">在解决方案资源管理器中，打开刚刚添加的引用快捷菜单，然后选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="d1fda-112">In Solution Explorer, open the shortcut menu for the reference you just added, and then choose **Properties**.</span></span>  
  
7. <span data-ttu-id="d1fda-113">在“属性”窗口中，确保将“嵌入互操作类型”属性设置为“True”  。</span><span class="sxs-lookup"><span data-stu-id="d1fda-113">In the **Properties** window, make sure that the **Embed Interop Types** property is set to **True**.</span></span> <span data-ttu-id="d1fda-114">通过此设置，Visual Studio 将会在可执行文件中嵌入 COM 类型的类型信息，从而不必将主互操作程序集与你的应用一起部署。</span><span class="sxs-lookup"><span data-stu-id="d1fda-114">This causes Visual Studio to embed type information for COM types in your executables, eliminating the need to deploy primary interop assemblies with your app.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="d1fda-115">根据你使用的 Visual Studio 版本，菜单和对话框选项可能会有所不同。</span><span class="sxs-lookup"><span data-stu-id="d1fda-115">The menu and dialog box options may vary depending on the version of Visual Studio you're using.</span></span>  
  
### <a name="to-add-a-reference-to-a-type-library-for-command-line-compilation"></a><span data-ttu-id="d1fda-116">添加对类型库的引用以进行命令行编译</span><span class="sxs-lookup"><span data-stu-id="d1fda-116">To add a reference to a type library for command-line compilation</span></span>  
  
1. <span data-ttu-id="d1fda-117">按后列文章中所述，生成一个互操作程序集：[如何：从类型库生成互操作程序集](how-to-generate-interop-assemblies-from-type-libraries.md)。</span><span class="sxs-lookup"><span data-stu-id="d1fda-117">Generate an interop assembly as described in [How to: Generate Interop Assemblies from Type Libraries](how-to-generate-interop-assemblies-from-type-libraries.md).</span></span>  
  
2. <span data-ttu-id="d1fda-118">配合使用 [-link（C# 编译器选项）](../../csharp/language-reference/compiler-options/inputs.md#embedinteroptypes)或 [-link (Visual Basic)](../../visual-basic/reference/command-line-compiler/link.md) 编译器选项与互操作程序集名称，将 COM 类型的类型信息嵌入到可执行文件中。</span><span class="sxs-lookup"><span data-stu-id="d1fda-118">Use the [-link (C# Compiler Options)](../../csharp/language-reference/compiler-options/inputs.md#embedinteroptypes) or [-link (Visual Basic)](../../visual-basic/reference/command-line-compiler/link.md) compiler option with the interop assembly name to embed type information for COM types in your executables.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="d1fda-119">请参阅</span><span class="sxs-lookup"><span data-stu-id="d1fda-119">See also</span></span>

- [<span data-ttu-id="d1fda-120">将类型库作为程序集导入</span><span class="sxs-lookup"><span data-stu-id="d1fda-120">Importing a Type Library as an Assembly</span></span>](importing-a-type-library-as-an-assembly.md)
- [<span data-ttu-id="d1fda-121">向 .NET Framework 公开 COM 组件</span><span class="sxs-lookup"><span data-stu-id="d1fda-121">Exposing COM Components to the .NET Framework</span></span>](exposing-com-components.md)
- [<span data-ttu-id="d1fda-122">演练：在 Visual Studio 中嵌入托管程序集中的类型</span><span class="sxs-lookup"><span data-stu-id="d1fda-122">Walkthrough: Embedding Types from Managed Assemblies in Visual Studio</span></span>](../../standard/assembly/embed-types-visual-studio.md)
- [<span data-ttu-id="d1fda-123">-link（C# 编译器选项）</span><span class="sxs-lookup"><span data-stu-id="d1fda-123">-link (C# Compiler Options)</span></span>](../../csharp/language-reference/compiler-options/inputs.md#embedinteroptypes)
- [<span data-ttu-id="d1fda-124">-link (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="d1fda-124">-link (Visual Basic)</span></span>](../../visual-basic/reference/command-line-compiler/link.md)
