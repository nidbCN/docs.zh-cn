---
title: 如何：使用跟踪和调试执行有条件编译
description: 了解如何在编译 .NET 应用程序时使用 TRACE 和 DEBUG 条件属性进行条件编译。
ms.date: 03/30/2017
helpviewer_keywords:
- trace compiler options
- trace statements
- compiling source code, trace statements
- tracing [.NET Framework], enabling or disabling
- tracing [.NET Framework], compiling conditionally
- TRACE directive
- conditional compilation, tracing code
ms.assetid: 56d051c3-012c-42c1-9a58-7270edc624aa
ms.openlocfilehash: 90c989bcf21e24cf7ccf410c9a18a44cde81233e
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103478213"
---
# <a name="how-to-compile-conditionally-with-trace-and-debug"></a><span data-ttu-id="ed690-103">如何：使用跟踪和调试执行有条件编译</span><span class="sxs-lookup"><span data-stu-id="ed690-103">How to: Compile Conditionally with Trace and Debug</span></span>

<span data-ttu-id="ed690-104">在开发过程中调试应用程序时，跟踪和调试输出都会出现在 Visual Studio 的“输出”窗口中。</span><span class="sxs-lookup"><span data-stu-id="ed690-104">While you are debugging an application during development, both your tracing and debugging output go to the Output window in Visual Studio.</span></span> <span data-ttu-id="ed690-105">但是，若要在已部署的应用程序中包含跟踪功能，则必须在 TRACE 编译器指令处于启动状态下编译已检测应用程序。</span><span class="sxs-lookup"><span data-stu-id="ed690-105">However, to include tracing features in a deployed application, you must compile your instrumented applications with the **TRACE** compiler directive enabled.</span></span> <span data-ttu-id="ed690-106">这样就可以将跟踪代码编译成应用程序的发布版本。</span><span class="sxs-lookup"><span data-stu-id="ed690-106">This allows tracing code to be compiled into the release version of your application.</span></span> <span data-ttu-id="ed690-107">如果未启用 TRACE 指令，将在编译过程中忽略所有跟踪代码，并且不会将其包含在将部署的可执行代码中。</span><span class="sxs-lookup"><span data-stu-id="ed690-107">If you do not enable the **TRACE** directive, all tracing code is ignored during compilation and is not included in the executable code that you will deploy.</span></span>  
  
 <span data-ttu-id="ed690-108">跟踪方法和调试方法都具有关联的条件属性。</span><span class="sxs-lookup"><span data-stu-id="ed690-108">Both the tracing and debugging methods have associated conditional attributes.</span></span> <span data-ttu-id="ed690-109">例如，如果跟踪的条件属性为 true，则所有跟踪语句都将包含在一个程序集（经过编译的 .exe 文件或 .dll）中；如果 Trace 条件属性为 false，则不会包括跟踪语句。</span><span class="sxs-lookup"><span data-stu-id="ed690-109">For example, if the conditional attribute for tracing is **true**, all trace statements are included within an assembly (a compiled .exe file or .dll); if the **Trace** conditional attribute is **false**, the trace statements are not included.</span></span>  
  
 <span data-ttu-id="ed690-110">可以启用 Trace 或 Debug 条件属性作为一种生成，或启用这两者，或两者都不启用。</span><span class="sxs-lookup"><span data-stu-id="ed690-110">You can have either the **Trace** or **Debug** conditional attribute turned on for a build, or both, or neither.</span></span> <span data-ttu-id="ed690-111">这样就会有四种类型的生成：Debug、Trace、二者都启用，或二者都不启用。</span><span class="sxs-lookup"><span data-stu-id="ed690-111">Thus, there are four types of build: **Debug**, **Trace**, both, or neither.</span></span> <span data-ttu-id="ed690-112">某些用于生产部署的发布版本可能不包含这两种属性；大多数调试版本会同时包含这两种属性。</span><span class="sxs-lookup"><span data-stu-id="ed690-112">Some release builds for production deployment might contain neither; most debugging builds contain both.</span></span>  
  
 <span data-ttu-id="ed690-113">可以通过几种方式来指定应用程序的编译器设置：</span><span class="sxs-lookup"><span data-stu-id="ed690-113">You can specify the compiler settings for your application in several ways:</span></span>  
  
- <span data-ttu-id="ed690-114">属性页</span><span class="sxs-lookup"><span data-stu-id="ed690-114">The property pages</span></span>  
  
- <span data-ttu-id="ed690-115">命令行</span><span class="sxs-lookup"><span data-stu-id="ed690-115">The command line</span></span>  
  
- <span data-ttu-id="ed690-116">#CONST（适用于 Visual Basic）和 #define（适用于 C#）</span><span class="sxs-lookup"><span data-stu-id="ed690-116">**#CONST** (for Visual Basic) and **#define** (for C#)</span></span>  
  
### <a name="to-change-compile-settings-from-the-property-pages-dialog-box"></a><span data-ttu-id="ed690-117">从属性页对话框中更改编译设置</span><span class="sxs-lookup"><span data-stu-id="ed690-117">To change compile settings from the property pages dialog box</span></span>  
  
1. <span data-ttu-id="ed690-118">右键单击“解决方案资源管理器”中的项目节点。</span><span class="sxs-lookup"><span data-stu-id="ed690-118">Right-click the project node in **Solution Explorer**.</span></span>  
  
2. <span data-ttu-id="ed690-119">从快捷菜单中选择“属性”。</span><span class="sxs-lookup"><span data-stu-id="ed690-119">Choose **Properties** from the shortcut menu.</span></span>  
  
    - <span data-ttu-id="ed690-120">在 Visual Basic 中，单击属性页左窗格中的“编译”选项卡，再单击“高级编译选项”按钮，以显示“高级编译器设置”对话框。</span><span class="sxs-lookup"><span data-stu-id="ed690-120">In Visual Basic, click the **Compile** tab in the left pane of the property page, then click the **Advanced Compile Options** button to display the **Advanced Compiler Settings** dialog box.</span></span> <span data-ttu-id="ed690-121">选中想要启用的编译器设置对应的复选框。</span><span class="sxs-lookup"><span data-stu-id="ed690-121">Select the check boxes for the compiler settings you want to enable.</span></span> <span data-ttu-id="ed690-122">清除要禁用的设置的复选框。</span><span class="sxs-lookup"><span data-stu-id="ed690-122">Clear the check boxes for settings you want to disable.</span></span>  
  
    - <span data-ttu-id="ed690-123">在 C# 中，单击属性页左窗格中的“生成”选项卡，然后选中要启用的编译器设置对应的复选框。</span><span class="sxs-lookup"><span data-stu-id="ed690-123">In C#, click the **Build** tab in the left pane of the property page, then select the check boxes for the compiler settings you want to enable.</span></span> <span data-ttu-id="ed690-124">清除要禁用的设置的复选框。</span><span class="sxs-lookup"><span data-stu-id="ed690-124">Clear the check boxes for settings you want to disable.</span></span>  
  
### <a name="to-compile-instrumented-code-using-the-command-line"></a><span data-ttu-id="ed690-125">使用命令行编译已插入检测点的代码</span><span class="sxs-lookup"><span data-stu-id="ed690-125">To compile instrumented code using the command line</span></span>  
  
1. <span data-ttu-id="ed690-126">在命令行上设置条件编译器开关。</span><span class="sxs-lookup"><span data-stu-id="ed690-126">Set a conditional compiler switch on the command line.</span></span> <span data-ttu-id="ed690-127">编译器将在可执行文件中包括跟踪或调试代码。</span><span class="sxs-lookup"><span data-stu-id="ed690-127">The compiler will include trace or debug code in the executable.</span></span>  
  
     <span data-ttu-id="ed690-128">例如，在命令行上输入的以下编译器指令将在经过编译的可执行文件中包含跟踪代码：</span><span class="sxs-lookup"><span data-stu-id="ed690-128">For example, the following compiler instruction entered on the command line would include your tracing code in a compiled executable:</span></span>  
  
     <span data-ttu-id="ed690-129">对于 Visual Basic： **vbc -r:System.dll-d:TRACE = TRUE-d:DEBUG = FALSE MyApplication**</span><span class="sxs-lookup"><span data-stu-id="ed690-129">For Visual Basic: **vbc -r:System.dll -d:TRACE=TRUE -d:DEBUG=FALSE MyApplication.vb**</span></span>  
  
     <span data-ttu-id="ed690-130">对于 c #： **csc -r:System.dll-d:DEBUG = FALSE MyApplication.cs**</span><span class="sxs-lookup"><span data-stu-id="ed690-130">For C#: **csc -r:System.dll -d:TRACE -d:DEBUG=FALSE MyApplication.cs**</span></span>  
  
    > [!TIP]
    > <span data-ttu-id="ed690-131">若要编译多个应用程序文件，请在文件名之间留一个空格，例如 MyApplication1.vb MyApplication2.vb MyApplication3.vb 或 MyApplication1.cs MyApplication2.cs MyApplication3.cs。</span><span class="sxs-lookup"><span data-stu-id="ed690-131">To compile more than one application file, leave a blank space between the file names, for example, **MyApplication1.vb MyApplication2.vb MyApplication3.vb** or **MyApplication1.cs MyApplication2.cs MyApplication3.cs**.</span></span>  
  
     <span data-ttu-id="ed690-132">以上示例所使用的条件编译指令具有如下含义：</span><span class="sxs-lookup"><span data-stu-id="ed690-132">The meaning of the conditional-compilation directives used in the above examples is as follows:</span></span>  
  
    |<span data-ttu-id="ed690-133">指令</span><span class="sxs-lookup"><span data-stu-id="ed690-133">Directive</span></span>|<span data-ttu-id="ed690-134">含义</span><span class="sxs-lookup"><span data-stu-id="ed690-134">Meaning</span></span>|  
    |---------------|-------------|  
    |`vbc`|<span data-ttu-id="ed690-135">Visual Basic 编译器</span><span class="sxs-lookup"><span data-stu-id="ed690-135">Visual Basic compiler</span></span>|  
    |`csc`|<span data-ttu-id="ed690-136">C# 编译器</span><span class="sxs-lookup"><span data-stu-id="ed690-136">C# compiler</span></span>|  
    |`-r:`|<span data-ttu-id="ed690-137">引用外部程序集（EXE 或 DLL）</span><span class="sxs-lookup"><span data-stu-id="ed690-137">References an external assembly (EXE or DLL)</span></span>|  
    |`-d:`|<span data-ttu-id="ed690-138">定义条件编译符号</span><span class="sxs-lookup"><span data-stu-id="ed690-138">Defines a conditional compilation symbol</span></span>|  
  
    > [!NOTE]
    > <span data-ttu-id="ed690-139">必须用大写字母来拼写 TRACE 或 DEBUG。</span><span class="sxs-lookup"><span data-stu-id="ed690-139">You must spell TRACE or DEBUG with uppercase letters.</span></span> <span data-ttu-id="ed690-140">有关条件编译命令的详细信息，请在命令提示处输入 `vbc /?`（对于 Visual Basic）或 `csc /?`（对于 C#）。</span><span class="sxs-lookup"><span data-stu-id="ed690-140">For more information about the conditional compilation commands, enter `vbc /?` (for Visual Basic) or `csc /?` (for C#) at the command prompt.</span></span> <span data-ttu-id="ed690-141">有关详细信息，请参阅[从命令行生成](../../csharp/language-reference/compiler-options/index.md) (C#) 或[调用命令行编译器](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md) (Visual Basic)。</span><span class="sxs-lookup"><span data-stu-id="ed690-141">For more information, see [Building from the Command Line](../../csharp/language-reference/compiler-options/index.md) (C#) or [Invoking the Command-Line Compiler](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md) (Visual Basic).</span></span>  
  
### <a name="to-perform-conditional-compilation-using-const-or-define"></a><span data-ttu-id="ed690-142">使用 #CONST 或 #define 执行条件编译</span><span class="sxs-lookup"><span data-stu-id="ed690-142">To perform conditional compilation using #CONST or #define</span></span>  
  
1. <span data-ttu-id="ed690-143">在源代码文件的顶部键入对应于编程语言的适当语句。</span><span class="sxs-lookup"><span data-stu-id="ed690-143">Type the appropriate statement for your programming language at the top of the source code file.</span></span>  
  
    |<span data-ttu-id="ed690-144">语言</span><span class="sxs-lookup"><span data-stu-id="ed690-144">Language</span></span>|<span data-ttu-id="ed690-145">语句</span><span class="sxs-lookup"><span data-stu-id="ed690-145">Statement</span></span>|<span data-ttu-id="ed690-146">结果</span><span class="sxs-lookup"><span data-stu-id="ed690-146">Result</span></span>|  
    |--------------|---------------|------------|  
    |<span data-ttu-id="ed690-147">**Visual Basic**</span><span class="sxs-lookup"><span data-stu-id="ed690-147">**Visual Basic**</span></span>|<span data-ttu-id="ed690-148">#CONST TRACE = true</span><span class="sxs-lookup"><span data-stu-id="ed690-148">**#CONST TRACE = true**</span></span>|<span data-ttu-id="ed690-149">启用跟踪</span><span class="sxs-lookup"><span data-stu-id="ed690-149">Enables tracing</span></span>|  
    ||<span data-ttu-id="ed690-150">#CONST TRACE = false</span><span class="sxs-lookup"><span data-stu-id="ed690-150">**#CONST TRACE = false**</span></span>|<span data-ttu-id="ed690-151">禁用跟踪</span><span class="sxs-lookup"><span data-stu-id="ed690-151">Disables tracing</span></span>|  
    ||<span data-ttu-id="ed690-152">#CONST DEBUG = true</span><span class="sxs-lookup"><span data-stu-id="ed690-152">**#CONST DEBUG = true**</span></span>|<span data-ttu-id="ed690-153">启用调试</span><span class="sxs-lookup"><span data-stu-id="ed690-153">Enables debugging</span></span>|  
    ||<span data-ttu-id="ed690-154">#CONST DEBUG = false</span><span class="sxs-lookup"><span data-stu-id="ed690-154">**#CONST DEBUG = false**</span></span>|<span data-ttu-id="ed690-155">禁用调试</span><span class="sxs-lookup"><span data-stu-id="ed690-155">Disables debugging</span></span>|  
    |<span data-ttu-id="ed690-156">**C#**</span><span class="sxs-lookup"><span data-stu-id="ed690-156">**C#**</span></span>|<span data-ttu-id="ed690-157">#define TRACE</span><span class="sxs-lookup"><span data-stu-id="ed690-157">**#define TRACE**</span></span>|<span data-ttu-id="ed690-158">启用跟踪</span><span class="sxs-lookup"><span data-stu-id="ed690-158">Enables tracing</span></span>|  
    ||<span data-ttu-id="ed690-159">#undef TRACE</span><span class="sxs-lookup"><span data-stu-id="ed690-159">**#undef TRACE**</span></span>|<span data-ttu-id="ed690-160">禁用跟踪</span><span class="sxs-lookup"><span data-stu-id="ed690-160">Disables tracing</span></span>|  
    ||<span data-ttu-id="ed690-161">#define DEBUG</span><span class="sxs-lookup"><span data-stu-id="ed690-161">**#define DEBUG**</span></span>|<span data-ttu-id="ed690-162">启用调试</span><span class="sxs-lookup"><span data-stu-id="ed690-162">Enables debugging</span></span>|  
    ||<span data-ttu-id="ed690-163">#undef DEBUG</span><span class="sxs-lookup"><span data-stu-id="ed690-163">**#undef DEBUG**</span></span>|<span data-ttu-id="ed690-164">禁用调试</span><span class="sxs-lookup"><span data-stu-id="ed690-164">Disables debugging</span></span>|  
  
### <a name="to-disable-tracing-or-debugging"></a><span data-ttu-id="ed690-165">禁用跟踪或调试</span><span class="sxs-lookup"><span data-stu-id="ed690-165">To disable tracing or debugging</span></span>  
  
<span data-ttu-id="ed690-166">从源代码中删除编译器指令。</span><span class="sxs-lookup"><span data-stu-id="ed690-166">Delete the compiler directive from your source code.</span></span>  
  
<span data-ttu-id="ed690-167">\- 或 -</span><span class="sxs-lookup"><span data-stu-id="ed690-167">\- or -</span></span>  
  
<span data-ttu-id="ed690-168">注释掉编译器指令。</span><span class="sxs-lookup"><span data-stu-id="ed690-168">Comment out the compiler directive.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="ed690-169">准备进行编译时，可从“生成”菜单中选择“生成”，也可使用命令行方法但不键入 d:，以定义条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="ed690-169">When you are ready to compile, you can either choose **Build** from the **Build** menu, or use the command line method but without typing the **d:** to define conditional compilation symbols.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="ed690-170">另请参阅</span><span class="sxs-lookup"><span data-stu-id="ed690-170">See also</span></span>

- [<span data-ttu-id="ed690-171">跟踪应用程序和在应用程序中插入检测点</span><span class="sxs-lookup"><span data-stu-id="ed690-171">Tracing and Instrumenting Applications</span></span>](tracing-and-instrumenting-applications.md)
- [<span data-ttu-id="ed690-172">如何：创建、初始化和配置跟踪开关</span><span class="sxs-lookup"><span data-stu-id="ed690-172">How to: Create, Initialize and Configure Trace Switches</span></span>](how-to-create-initialize-and-configure-trace-switches.md)
- [<span data-ttu-id="ed690-173">跟踪开关</span><span class="sxs-lookup"><span data-stu-id="ed690-173">Trace Switches</span></span>](trace-switches.md)
- [<span data-ttu-id="ed690-174">跟踪侦听器</span><span class="sxs-lookup"><span data-stu-id="ed690-174">Trace Listeners</span></span>](trace-listeners.md)
- [<span data-ttu-id="ed690-175">如何：向应用程序代码添加跟踪语句</span><span class="sxs-lookup"><span data-stu-id="ed690-175">How to: Add Trace Statements to Application Code</span></span>](how-to-add-trace-statements-to-application-code.md)
- [<span data-ttu-id="ed690-176">如何为 Visual Studio 命令行设置环境变量</span><span class="sxs-lookup"><span data-stu-id="ed690-176">How to set environment variables for the Visual Studio Command Line</span></span>](../../csharp/language-reference/compiler-options/index.md)
- [<span data-ttu-id="ed690-177">如何：调用命令行编译器</span><span class="sxs-lookup"><span data-stu-id="ed690-177">How to: Invoke the Command-Line Compiler</span></span>](../../visual-basic/reference/command-line-compiler/how-to-invoke-the-command-line-compiler.md)
