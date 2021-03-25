---
title: 演练：Office 编程（C# 和 Visual Basic）
description: 了解 Visual Studio 在 C# 和 Visual Basic 中提供了改进 Microsoft Office 编程的功能。
ms.date: 07/20/2015
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Office, programming in Visual Basic and C#
- Office programming [C#]
- Office programming [Visual Basic]
ms.assetid: 519cff31-f80b-4f0e-a56b-26358d0f8c51
ms.openlocfilehash: 11900aa1ce5296f1cf0e9c271c3a7a45ddba5066
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103480336"
---
# <a name="walkthrough-office-programming-c-and-visual-basic"></a><span data-ttu-id="1a9e1-103">演练：Office 编程（C# 和 Visual Basic）</span><span class="sxs-lookup"><span data-stu-id="1a9e1-103">Walkthrough: Office Programming (C# and Visual Basic)</span></span>

<span data-ttu-id="1a9e1-104">Visual Studio 在 C# 和 Visual Basic 中提供了改进 Microsoft Office 编程的功能。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-104">Visual Studio offers features in C# and Visual Basic that improve Microsoft Office programming.</span></span> <span data-ttu-id="1a9e1-105">有用的 C# 功能包括命名参数和可选参数以及类型为 `dynamic` 的返回值。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-105">Helpful C# features include named and optional arguments and return values of type `dynamic`.</span></span> <span data-ttu-id="1a9e1-106">在 COM 编程中，可以省略 `ref` 关键字并获得索引属性的访问权限。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-106">In COM programming, you can omit the `ref` keyword and gain access to indexed properties.</span></span> <span data-ttu-id="1a9e1-107">Visual Basic 中的功能包括自动实现的属性、Lambda 表达式语句和集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-107">Features in Visual Basic include auto-implemented properties, statements in lambda expressions, and collection initializers.</span></span>

<span data-ttu-id="1a9e1-108">两种语言都支持嵌入类型信息，从而允许在不向用户的计算机部署主互操作程序集 (PIA) 的情况下部署与 COM 组件交互的程序集。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-108">Both languages enable embedding of type information, which allows deployment of assemblies that interact with COM components without deploying primary interop assemblies (PIAs) to the user's computer.</span></span> <span data-ttu-id="1a9e1-109">有关详细信息，请参见[演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-109">For more information, see [Walkthrough: Embedding Types from Managed Assemblies](../../../standard/assembly/embed-types-visual-studio.md).</span></span>

<span data-ttu-id="1a9e1-110">本演练演示 Office 编程上下文中的这些功能，但其中许多功能在常规编程中也极为有用。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-110">This walkthrough demonstrates these features in the context of Office programming, but many of these features are also useful in general programming.</span></span> <span data-ttu-id="1a9e1-111">本演练将使用 Excel 外接应用程序创建 Excel 工作簿。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-111">In the walkthrough, you use an Excel Add-in application to create an Excel workbook.</span></span> <span data-ttu-id="1a9e1-112">然后，将创建包含工作簿链接的 Word 文档。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-112">Next, you create a Word document that contains a link to the workbook.</span></span> <span data-ttu-id="1a9e1-113">最后，将介绍如何启用和禁用 PIA 依赖项。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-113">Finally, you see how to enable and disable the PIA dependency.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1a9e1-114">先决条件</span><span class="sxs-lookup"><span data-stu-id="1a9e1-114">Prerequisites</span></span>

<span data-ttu-id="1a9e1-115">若要完成本演练，计算机上必须安装 Microsoft Office Excel 和 Microsoft Office Word。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-115">You must have Microsoft Office Excel and Microsoft Office Word installed on your computer to complete this walkthrough.</span></span>

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

### <a name="to-set-up-an-excel-add-in-application"></a><span data-ttu-id="1a9e1-116">设置 Excel 外接应用程序</span><span class="sxs-lookup"><span data-stu-id="1a9e1-116">To set up an Excel Add-in application</span></span>

1. <span data-ttu-id="1a9e1-117">启动 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-117">Start Visual Studio.</span></span>

2. <span data-ttu-id="1a9e1-118">在 **“文件”** 菜单上，指向 **“新建”** ，然后单击 **“项目”** 。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-118">On the **File** menu, point to **New**, and then click **Project**.</span></span>

3. <span data-ttu-id="1a9e1-119">在“安装的模板”窗格中，展开“Visual Basic”或“Visual C#”，再展开“Office”，然后单击 Office 产品的版本年份。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-119">In the **Installed Templates** pane, expand **Visual Basic** or **Visual C#**, expand **Office**, and then click the version year of the Office product.</span></span>

4. <span data-ttu-id="1a9e1-120">在“模板”窗格中，单击“Excel \<version> 外接程序”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-120">In the **Templates** pane, click **Excel \<version> Add-in**.</span></span>

5. <span data-ttu-id="1a9e1-121">查看“模板”窗格的顶部，确保“.NET Framework 4”或更高版本出现在“目标框架”框中。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-121">Look at the top of the **Templates** pane to make sure that **.NET Framework 4**, or a later version, appears in the **Target Framework** box.</span></span>

6. <span data-ttu-id="1a9e1-122">如果需要，在“名称”框中键入项目的名称。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-122">Type a name for your project in the **Name** box, if you want to.</span></span>

7. <span data-ttu-id="1a9e1-123">单击 **“确定”** 。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-123">Click **OK**.</span></span>

8. <span data-ttu-id="1a9e1-124">新项目将出现在“解决方案资源管理器”中。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-124">The new project appears in **Solution Explorer**.</span></span>

### <a name="to-add-references"></a><span data-ttu-id="1a9e1-125">添加引用</span><span class="sxs-lookup"><span data-stu-id="1a9e1-125">To add references</span></span>

1. <span data-ttu-id="1a9e1-126">在“解决方案资源管理器”中，右键单击你的项目名称，然后单击“添加引用”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-126">In **Solution Explorer**, right-click your project's name and then click **Add Reference**.</span></span> <span data-ttu-id="1a9e1-127">此时会显示“添加引用”对话框。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-127">The **Add Reference** dialog box appears.</span></span>

2. <span data-ttu-id="1a9e1-128">在“程序集”选项卡上，在“组件名称”列表中选择“Microsoft.Office.Interop.Excel”版本 `<version>.0.0.0`（有关 Office 产品版本号的键，请参阅 [Microsoft 版本](https://en.wikipedia.org/wiki/Microsoft_Office#Versions)），然后按住 Ctrl 键并选择“Microsoft.Office.Interop.Word”，`version <version>.0.0.0`。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-128">On the **Assemblies** tab, select **Microsoft.Office.Interop.Excel**, version `<version>.0.0.0` (for a key to the Office product version numbers, see [Microsoft Versions](https://en.wikipedia.org/wiki/Microsoft_Office#Versions)), in the **Component Name** list, and then hold down the CTRL key and select **Microsoft.Office.Interop.Word**, `version <version>.0.0.0`.</span></span> <span data-ttu-id="1a9e1-129">如果未看到程序集，则可能需要确保安装并显示它们（参阅[如何：安装 Office 主互操作程序集](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies)）。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-129">If you do not see the assemblies, you may need to ensure they are installed and displayed (see [How to: Install Office Primary Interop Assemblies](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies)).</span></span>

3. <span data-ttu-id="1a9e1-130">单击 **“确定”** 。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-130">Click **OK**.</span></span>

### <a name="to-add-necessary-imports-statements-or-using-directives"></a><span data-ttu-id="1a9e1-131">添加必要的 Imports 语句或 using 指令</span><span class="sxs-lookup"><span data-stu-id="1a9e1-131">To add necessary Imports statements or using directives</span></span>

1. <span data-ttu-id="1a9e1-132">在“解决方案资源管理器”中，右键单击“ThisAddIn.vb”或“ThisAddIn.cs”文件，然后单击“查看代码”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-132">In **Solution Explorer**, right-click the **ThisAddIn.vb** or **ThisAddIn.cs** file and then click **View Code**.</span></span>

2. <span data-ttu-id="1a9e1-133">将以下 `Imports` 语句 (Visual Basic) 或 `using` 指令 (C#) 添加到代码文件的顶部（如果不存在）。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-133">Add the following `Imports` statements (Visual Basic) or `using` directives (C#) to the top of the code file if they are not already present.</span></span>

     [!code-csharp[csOfficeWalkthrough#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#1)]

     [!code-vb[csOfficeWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#1)]

### <a name="to-create-a-list-of-bank-accounts"></a><span data-ttu-id="1a9e1-134">创建银行帐户列表</span><span class="sxs-lookup"><span data-stu-id="1a9e1-134">To create a list of bank accounts</span></span>

1. <span data-ttu-id="1a9e1-135">在“解决方案资源管理器”中，右键单击你的项目名称，单击“添加”，然后单击“类”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-135">In **Solution Explorer**, right-click your project's name, click **Add**, and then click **Class**.</span></span> <span data-ttu-id="1a9e1-136">如果使用的是 Visual Basic，则将类命名为 Account.vb；如果使用的是 C#，则将类命名为 Account.cs。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-136">Name the class Account.vb if you are using Visual Basic or Account.cs if you are using C#.</span></span> <span data-ttu-id="1a9e1-137">单击 **添加**。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-137">Click **Add**.</span></span>

2. <span data-ttu-id="1a9e1-138">将 `Account` 类的定义替换为以下代码。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-138">Replace the definition of the `Account` class with the following code.</span></span> <span data-ttu-id="1a9e1-139">类定义使用自动实现的属性。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-139">The class definitions use *auto-implemented properties*.</span></span> <span data-ttu-id="1a9e1-140">有关详细信息，请参阅[自动实现的属性](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-140">For more information, see [Auto-Implemented Properties](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md).</span></span>

     [!code-csharp[csOfficeWalkthrough#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/account.cs#2)]

     [!code-vb[csOfficeWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/account.vb#2)]

3. <span data-ttu-id="1a9e1-141">若要创建包含两个帐户的 `bankAccounts` 列表，请将以下代码添加到 ThisAddIn.vb 或 ThisAddIn.cs 中的 `ThisAddIn_Startup` 方法。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-141">To create a `bankAccounts` list that contains two accounts, add the following code to the `ThisAddIn_Startup` method in *ThisAddIn.vb* or *ThisAddIn.cs*.</span></span> <span data-ttu-id="1a9e1-142">列表声明使用集合初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-142">The list declarations use *collection initializers*.</span></span> <span data-ttu-id="1a9e1-143">有关详细信息，请参阅[集合初始值设定项](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-143">For more information, see [Collection Initializers](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md).</span></span>

     [!code-csharp[csOfficeWalkthrough#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#3)]

     [!code-vb[csOfficeWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#3)]

### <a name="to-export-data-to-excel"></a><span data-ttu-id="1a9e1-144">将数据导出到 Excel</span><span class="sxs-lookup"><span data-stu-id="1a9e1-144">To export data to Excel</span></span>

1. <span data-ttu-id="1a9e1-145">在相同的文件中，将以下方法添加到 `ThisAddIn` 类。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-145">In the same file, add the following method to the `ThisAddIn` class.</span></span> <span data-ttu-id="1a9e1-146">该方法设置 Excel 工作薄并将数据导出到工作簿。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-146">The method sets up an Excel workbook and exports data to it.</span></span>

     [!code-csharp[csOfficeWalkthrough#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#4)]

     [!code-vb[csOfficeWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#4)]

     <span data-ttu-id="1a9e1-147">此方法使用 C# 的两项新功能。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-147">Two new C# features are used in this method.</span></span> <span data-ttu-id="1a9e1-148">Visual Basic 中已存在这两项功能。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-148">Both of these features already exist in Visual Basic.</span></span>

    - <span data-ttu-id="1a9e1-149">方法 [Add](<xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A>) 有一个 *可选参数*，用于指定特定的模板。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-149">Method [Add](<xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A>) has an *optional parameter* for specifying a particular template.</span></span> <span data-ttu-id="1a9e1-150">如果希望使用形参的默认值，你可以借助可选形参（C# 4 中新增）忽略该形参的实参。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-150">Optional parameters, new in C# 4, enable you to omit the argument for that parameter if you want to use the parameter's default value.</span></span> <span data-ttu-id="1a9e1-151">由于上一个示例中未发送任何参数，`Add` 将使用默认模板并创建新的工作簿。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-151">Because no argument is sent in the previous example, `Add` uses the default template and creates a new workbook.</span></span> <span data-ttu-id="1a9e1-152">C# 早期版本中的等效语句要求提供一个占位符参数：`excelApp.Workbooks.Add(Type.Missing)`。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-152">The equivalent statement in earlier versions of C# requires a placeholder argument: `excelApp.Workbooks.Add(Type.Missing)`.</span></span>

         <span data-ttu-id="1a9e1-153">有关详细信息，请参阅[命名参数和可选参数](../classes-and-structs/named-and-optional-arguments.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-153">For more information, see [Named and Optional Arguments](../classes-and-structs/named-and-optional-arguments.md).</span></span>

    - <span data-ttu-id="1a9e1-154">[Range](<xref:Microsoft.Office.Interop.Excel.Range>) 对象的 `Range` 和 `Offset` 属性使用“索引属性”功能。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-154">The `Range` and `Offset` properties of the [Range](<xref:Microsoft.Office.Interop.Excel.Range>) object use the *indexed properties* feature.</span></span> <span data-ttu-id="1a9e1-155">此功能允许你通过以下典型 C# 语法从 COM 类型使用这些属性。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-155">This feature enables you to consume these properties from COM types by using the following typical C# syntax.</span></span> <span data-ttu-id="1a9e1-156">索引属性还允许你使用 `Value` 对象的 `Range` 属性，因此不必使用 `Value2` 属性。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-156">Indexed properties also enable you to use the `Value` property of the `Range` object, eliminating the need to use the `Value2` property.</span></span> <span data-ttu-id="1a9e1-157">`Value` 属性已编入索引，但索引是可选的。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-157">The `Value` property is indexed, but the index is optional.</span></span> <span data-ttu-id="1a9e1-158">在以下示例中，可选自变量和索引属性配合使用。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-158">Optional arguments and indexed properties work together in the following example.</span></span>

         [!code-csharp[csOfficeWalkthrough#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#5)]

         <span data-ttu-id="1a9e1-159">在早期版本的语言中，需要以下特殊语法。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-159">In earlier versions of the language, the following special syntax is required.</span></span>

         [!code-csharp[csOfficeWalkthrough#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#6)]

         <span data-ttu-id="1a9e1-160">你不能创建自己的索引属性。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-160">You cannot create indexed properties of your own.</span></span> <span data-ttu-id="1a9e1-161">该功能仅支持使用现有索引属性。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-161">The feature only supports consumption of existing indexed properties.</span></span>

         <span data-ttu-id="1a9e1-162">有关详细信息，请参阅[如何在 COM 互操作编程中使用索引属性](./how-to-use-indexed-properties-in-com-interop-rogramming.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-162">For more information, see [How to use indexed properties in COM interop programming](./how-to-use-indexed-properties-in-com-interop-rogramming.md).</span></span>

2. <span data-ttu-id="1a9e1-163">在 `DisplayInExcel` 的末尾添加以下代码以将列宽调整为适合内容。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-163">Add the following code at the end of `DisplayInExcel` to adjust the column widths to fit the content.</span></span>

     [!code-csharp[csOfficeWalkthrough#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#7)]

     [!code-vb[csOfficeWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#7)]

     <span data-ttu-id="1a9e1-164">这些新增内容介绍了 C# 中的另一功能：处理从 COM 主机返回的 `Object` 值（如 Office），就像它们具有 [dynamic](../../language-reference/builtin-types/reference-types.md) 类型一样。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-164">These additions demonstrate another feature in C#: treating `Object` values returned from COM hosts such as Office as if they have type [dynamic](../../language-reference/builtin-types/reference-types.md).</span></span> <span data-ttu-id="1a9e1-165">当“嵌入互操作类型”设置为其默认值 `True` 时，或者由 [EmbedInteropTypes](../../language-reference/compiler-options/inputs.md#embedinteroptypes) 编译器选项引用程序集时，会自动发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-165">This happens automatically when **Embed Interop Types** is set to its default value, `True`, or, equivalently, when the assembly is referenced by the [**EmbedInteropTypes**](../../language-reference/compiler-options/inputs.md#embedinteroptypes) compiler option.</span></span> <span data-ttu-id="1a9e1-166">键入 `dynamic` 允许后期绑定（Visual Basic 已提供该功能）并可避免 C# 3.0 以及早期版本的语言中要求的显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-166">Type `dynamic` allows late binding, already available in Visual Basic, and avoids the explicit casting required in C# 3.0 and earlier versions of the language.</span></span>

     <span data-ttu-id="1a9e1-167">例如，`excelApp.Columns[1]` 返回`Object`，并且 `AutoFit` 是 Excel [Range](<xref:Microsoft.Office.Interop.Excel.Range>) 方法。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-167">For example, `excelApp.Columns[1]` returns an `Object`, and `AutoFit` is an Excel  [Range](<xref:Microsoft.Office.Interop.Excel.Range>) method.</span></span> <span data-ttu-id="1a9e1-168">如果没有 `dynamic`，你必须将 `excelApp.Columns[1]` 返回的对象强制转换为 `Range` 的实例，然后才能调用 `AutoFit` 方法。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-168">Without `dynamic`, you must cast the object returned by `excelApp.Columns[1]` as an instance of `Range` before calling method `AutoFit`.</span></span>

     [!code-csharp[csOfficeWalkthrough#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#8)]

     <span data-ttu-id="1a9e1-169">有关嵌入互操作类型的详细信息，请参阅本主题后面部分的“查找 PIA 引用”和“还原 PIA 依赖项”程序。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-169">For more information about embedding interop types, see procedures "To find the PIA reference" and "To restore the PIA dependency" later in this topic.</span></span> <span data-ttu-id="1a9e1-170">有关 `dynamic` 的详细信息，请参阅 [dynamic](../../language-reference/builtin-types/reference-types.md) 或[使用类型 dynamic](../types/using-type-dynamic.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-170">For more information about `dynamic`, see [dynamic](../../language-reference/builtin-types/reference-types.md) or [Using Type dynamic](../types/using-type-dynamic.md).</span></span>

### <a name="to-invoke-displayinexcel"></a><span data-ttu-id="1a9e1-171">调用 DisplayInExcel</span><span class="sxs-lookup"><span data-stu-id="1a9e1-171">To invoke DisplayInExcel</span></span>

1. <span data-ttu-id="1a9e1-172">在 `ThisAddIn_StartUp` 方法的末尾添加以下代码。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-172">Add the following code at the end of the `ThisAddIn_StartUp` method.</span></span> <span data-ttu-id="1a9e1-173">对 `DisplayInExcel` 的调用包含两个参数。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-173">The call to `DisplayInExcel` contains two arguments.</span></span> <span data-ttu-id="1a9e1-174">第一个参数是要处理的帐户列表的名称。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-174">The first argument is the name of the list of accounts to be processed.</span></span> <span data-ttu-id="1a9e1-175">第二个参数是定义如何处理数据的多行 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-175">The second argument is a multiline lambda expression that defines how the data is to be processed.</span></span> <span data-ttu-id="1a9e1-176">每个帐户的 `ID` 和 `balance` 值都显示在相邻的单元格中，如果余额小于零，则相应的行显示为红色。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-176">The `ID` and `balance` values for each account are displayed in adjacent cells, and the row is displayed in red if the balance is less than zero.</span></span> <span data-ttu-id="1a9e1-177">有关详细信息，请参阅 [Lambda 表达式](../../language-reference/operators/lambda-expressions.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-177">For more information, see [Lambda Expressions](../../language-reference/operators/lambda-expressions.md).</span></span>

     [!code-csharp[csOfficeWalkthrough#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#9)]

     [!code-vb[csOfficeWalkthrough#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#9)]

2. <span data-ttu-id="1a9e1-178">若要运行程序，请按 F5。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-178">To run the program, press F5.</span></span> <span data-ttu-id="1a9e1-179">出现包含帐户数据的 Excel 工作表。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-179">An Excel worksheet appears that contains the data from the accounts.</span></span>

### <a name="to-add-a-word-document"></a><span data-ttu-id="1a9e1-180">添加 Word 文档</span><span class="sxs-lookup"><span data-stu-id="1a9e1-180">To add a Word document</span></span>

1. <span data-ttu-id="1a9e1-181">在 `ThisAddIn_StartUp` 方法末尾添加以下代码，以创建包含指向 Excel 工作簿的链接的 Word 文档。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-181">Add the following code at the end of the `ThisAddIn_StartUp` method to create a Word document that contains a link to the Excel workbook.</span></span>

     [!code-csharp[csOfficeWalkthrough#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#10)]

     [!code-vb[csOfficeWalkthrough#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#10)]

     <span data-ttu-id="1a9e1-182">此代码展示 C# 中的几项新功能：省略 COM 编程中的 `ref` 关键字、命名参数以及可选参数的能力。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-182">This code demonstrates several of the new features in C#: the ability to omit the `ref` keyword in COM programming, named arguments, and optional arguments.</span></span> <span data-ttu-id="1a9e1-183">Visual Basic 中已存在这些功能。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-183">These features already exist in Visual Basic.</span></span> <span data-ttu-id="1a9e1-184">[PasteSpecial](<xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A>) 方法有七个参数，所有参数都定义为可选引用参数。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-184">The [PasteSpecial](<xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A>) method has seven parameters, all of which are defined as optional reference parameters.</span></span> <span data-ttu-id="1a9e1-185">通过命名实参和可选实参，你可以指定希望按名称访问的形参并仅将实参发送到这些形参。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-185">Named and optional arguments enable you to designate the parameters you want to access by name and to send arguments to only those parameters.</span></span> <span data-ttu-id="1a9e1-186">在本示例中，发送实参以指示应创建指向剪贴板上工作簿的链接（形参 `Link`）并指示该链接应在 Word 文档中显示为图标（形参 `DisplayAsIcon`）。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-186">In this example, arguments are sent to indicate that a link to the workbook on the Clipboard should be created (parameter `Link`) and that the link is to be displayed in the Word document as an icon (parameter `DisplayAsIcon`).</span></span> <span data-ttu-id="1a9e1-187">Visual C# 还允许忽略这些参数的 `ref` 关键字。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-187">Visual C# also enables you to omit the `ref` keyword for these arguments.</span></span>

### <a name="to-run-the-application"></a><span data-ttu-id="1a9e1-188">要运行应用程序</span><span class="sxs-lookup"><span data-stu-id="1a9e1-188">To run the application</span></span>

1. <span data-ttu-id="1a9e1-189">按 F5 运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-189">Press F5 to run the application.</span></span> <span data-ttu-id="1a9e1-190">Excel 启动并显示包含 `bankAccounts` 中两个帐户的信息的表。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-190">Excel starts and displays a table that contains the information from the two accounts in `bankAccounts`.</span></span> <span data-ttu-id="1a9e1-191">然后，出现包含指向 Excel 表的 Word 文档。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-191">Then a Word document appears that contains a link to the Excel table.</span></span>

### <a name="to-clean-up-the-completed-project"></a><span data-ttu-id="1a9e1-192">清理已完成的项目</span><span class="sxs-lookup"><span data-stu-id="1a9e1-192">To clean up the completed project</span></span>

1. <span data-ttu-id="1a9e1-193">在 Visual Studio 中，单击“生成”菜单上的“清理解决方案”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-193">In Visual Studio, click **Clean Solution** on the **Build** menu.</span></span> <span data-ttu-id="1a9e1-194">否则，每次在计算机上打开 Excel 时都会运行外接应用程序。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-194">Otherwise, the add-in will run every time that you open Excel on your computer.</span></span>

### <a name="to-find-the-pia-reference"></a><span data-ttu-id="1a9e1-195">查找 PIA 引用</span><span class="sxs-lookup"><span data-stu-id="1a9e1-195">To find the PIA reference</span></span>

1. <span data-ttu-id="1a9e1-196">再次运行应用程序，但不单击“清理解决方案”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-196">Run the application again, but do not click **Clean Solution**.</span></span>

2. <span data-ttu-id="1a9e1-197">选择“开始”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-197">Select the **Start**.</span></span> <span data-ttu-id="1a9e1-198">找到“Microsoft Visual Studio \<version>”，然后打开开发人员命令提示。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-198">Locate **Microsoft Visual Studio \<version>** and open a developer command prompt.</span></span>

3. <span data-ttu-id="1a9e1-199">在“Visual Studio 的开发人员命令提示”窗口中键入 `ildasm`，然后按 Enter。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-199">Type `ildasm` in the Developer Command Prompt for Visual Studio window, and then press ENTER.</span></span> <span data-ttu-id="1a9e1-200">此时将出现 IL DASM 窗口。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-200">The IL DASM window appears.</span></span>

4. <span data-ttu-id="1a9e1-201">在 IL DASM 窗口的“文件”菜单上，选择“文件” > “打开”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-201">On the **File** menu in the IL DASM window, select **File** > **Open**.</span></span> <span data-ttu-id="1a9e1-202">双击“Visual Studio \<version>”，然后双击“项目”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-202">Double-click **Visual Studio \<version>**, and then double-click **Projects**.</span></span> <span data-ttu-id="1a9e1-203">打开项目的文件夹，在 bin/Debug 文件夹中查找 *项目名称*.dll。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-203">Open the folder for your project, and look in the bin/Debug folder for *your project name*.dll.</span></span> <span data-ttu-id="1a9e1-204">双击 *项目名称*.dll。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-204">Double-click *your project name*.dll.</span></span> <span data-ttu-id="1a9e1-205">新窗口将显示项目的属性以及对其他模块和程序集的引用。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-205">A new window displays your project's attributes, in addition to references to other modules and assemblies.</span></span> <span data-ttu-id="1a9e1-206">注意，命名空间 `Microsoft.Office.Interop.Excel` 和 `Microsoft.Office.Interop.Word` 包含在程序集中。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-206">Note that namespaces `Microsoft.Office.Interop.Excel` and `Microsoft.Office.Interop.Word` are included in the assembly.</span></span> <span data-ttu-id="1a9e1-207">在 Visual Studio 中，编译器默认将所需的类型从引用的 PIA 导入程序集。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-207">By default in Visual Studio, the compiler imports the types you need from a referenced PIA into your assembly.</span></span>

     <span data-ttu-id="1a9e1-208">有关详细信息，请参阅[如何：查看程序集内容](../../../standard/assembly/view-contents.md)。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-208">For more information, see [How to: View Assembly Contents](../../../standard/assembly/view-contents.md).</span></span>

5. <span data-ttu-id="1a9e1-209">双击“清单”图标。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-209">Double-click the **MANIFEST** icon.</span></span> <span data-ttu-id="1a9e1-210">此时将出现包含程序集列表的窗口，这些程序集包含项目所引用的项。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-210">A window appears that contains a list of assemblies that contain items referenced by the project.</span></span> <span data-ttu-id="1a9e1-211">`Microsoft.Office.Interop.Excel` 和 `Microsoft.Office.Interop.Word` 未包含在列表中。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-211">`Microsoft.Office.Interop.Excel` and `Microsoft.Office.Interop.Word` are not included in the list.</span></span> <span data-ttu-id="1a9e1-212">由于项目需要的类型已导入程序集中，因此不需要引用 PIA。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-212">Because the types your project needs have been imported into your assembly, references to a PIA are not required.</span></span> <span data-ttu-id="1a9e1-213">这使得部署变得更加容易。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-213">This makes deployment easier.</span></span> <span data-ttu-id="1a9e1-214">用户的计算机上不必存在 PIA，因为应用程序不需要部署特定版本的 PIA，应用程序可设计为与多个版本的 Office 配合使用，前提是所有版本中都存在必要的 API。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-214">The PIAs do not have to be present on the user's computer, and because an application does not require deployment of a specific version of a PIA, applications can be designed to work with multiple versions of Office, provided that the necessary APIs exist in all versions.</span></span>

     <span data-ttu-id="1a9e1-215">由于不再需要部署 PIA，你可以提前创建可与多个版本的 Office（包括之前的版本）配合使用的应用程序。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-215">Because deployment of PIAs is no longer necessary, you can create an application in advanced scenarios that works with multiple versions of Office, including earlier versions.</span></span> <span data-ttu-id="1a9e1-216">但是，仅当你的代码不使用你当前所使用 Office 版本中不可用的任何 API 时，此情况才适用。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-216">However, this works only if your code does not use any APIs that are not available in the version of Office you are working with.</span></span> <span data-ttu-id="1a9e1-217">特殊 API 在早期版本中是否可用并不始终明确，因此不建议使用早期版本的 Office。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-217">It is not always clear whether a particular API was available in an earlier version, and for that reason working with earlier versions of Office is not recommended.</span></span>

    > [!NOTE]
    > <span data-ttu-id="1a9e1-218">在 Office 2003 以前，Office 并不发布 PIA。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-218">Office did not publish PIAs before Office 2003.</span></span> <span data-ttu-id="1a9e1-219">因此，生成适用于 Office 2002 或早期版本的互操作程序集的唯一方法是导入 COM 引用。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-219">Therefore, the only way to generate an interop assembly for Office 2002 or earlier versions is by importing the COM reference.</span></span>

6. <span data-ttu-id="1a9e1-220">关闭清单窗口和程序集窗口。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-220">Close the manifest window and the assembly window.</span></span>

### <a name="to-restore-the-pia-dependency"></a><span data-ttu-id="1a9e1-221">还原 PIA 依赖项</span><span class="sxs-lookup"><span data-stu-id="1a9e1-221">To restore the PIA dependency</span></span>

1. <span data-ttu-id="1a9e1-222">在“解决方案资源管理器”中，单击“显示所有文件”按钮。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-222">In **Solution Explorer**, click the **Show All Files** button.</span></span> <span data-ttu-id="1a9e1-223">展开“引用”文件夹并选择“Microsoft.Office.Interop.Excel”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-223">Expand the **References** folder and select **Microsoft.Office.Interop.Excel**.</span></span> <span data-ttu-id="1a9e1-224">按 F4 以显示“属性”窗口。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-224">Press F4 to display the **Properties** window.</span></span>

2. <span data-ttu-id="1a9e1-225">在“属性”窗口中，将“嵌入互操作类型”属性从“True”更改为“False”。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-225">In the **Properties** window, change the **Embed Interop Types** property from **True** to **False**.</span></span>

3. <span data-ttu-id="1a9e1-226">对 `Microsoft.Office.Interop.Word` 重复此程序中的步骤 1 和 2。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-226">Repeat steps 1 and 2 in this procedure for `Microsoft.Office.Interop.Word`.</span></span>

4. <span data-ttu-id="1a9e1-227">在 C# 中，在 `Autofit` 方法的末尾注释掉对 `DisplayInExcel` 的两次调用。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-227">In C#, comment out the two calls to `Autofit` at the end of the `DisplayInExcel` method.</span></span>

5. <span data-ttu-id="1a9e1-228">按 F5 以验证项目是否仍正确运行。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-228">Press F5 to verify that the project still runs correctly.</span></span>

6. <span data-ttu-id="1a9e1-229">重复上一个程序的步骤 1-3 以打开程序集窗口。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-229">Repeat steps 1-3 from the previous procedure to open the assembly window.</span></span> <span data-ttu-id="1a9e1-230">注意，`Microsoft.Office.Interop.Word` 和 `Microsoft.Office.Interop.Excel` 不再位于嵌入程序集列表中。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-230">Notice that `Microsoft.Office.Interop.Word` and `Microsoft.Office.Interop.Excel` are no longer in the list of embedded assemblies.</span></span>

7. <span data-ttu-id="1a9e1-231">双击“清单”图标并滚动引用程序集的列表。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-231">Double-click the **MANIFEST** icon and scroll through the list of referenced assemblies.</span></span> <span data-ttu-id="1a9e1-232">`Microsoft.Office.Interop.Word` 和 `Microsoft.Office.Interop.Excel` 均位于列表中。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-232">Both `Microsoft.Office.Interop.Word` and `Microsoft.Office.Interop.Excel` are in the list.</span></span> <span data-ttu-id="1a9e1-233">由于应用程序引用 Excel 和 Word PIA 并且“嵌入互操作类型”属性设置为“False”，因此最终用户的计算机上必须存在两个程序集。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-233">Because the application references the Excel and Word PIAs, and the **Embed Interop Types** property is set to **False**, both assemblies must exist on the end user's computer.</span></span>

8. <span data-ttu-id="1a9e1-234">在 Visual Studio 中，单击“生成”菜单上的“清理解决方案”以清理完成的项目。</span><span class="sxs-lookup"><span data-stu-id="1a9e1-234">In Visual Studio, click **Clean Solution** on the **Build** menu to clean up the completed project.</span></span>

## <a name="see-also"></a><span data-ttu-id="1a9e1-235">请参阅</span><span class="sxs-lookup"><span data-stu-id="1a9e1-235">See also</span></span>

- [<span data-ttu-id="1a9e1-236">自动实现的属性 (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="1a9e1-236">Auto-Implemented Properties (Visual Basic)</span></span>](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)
- [<span data-ttu-id="1a9e1-237">自动实现的属性 (C#)</span><span class="sxs-lookup"><span data-stu-id="1a9e1-237">Auto-Implemented Properties (C#)</span></span>](../classes-and-structs/auto-implemented-properties.md)
- [<span data-ttu-id="1a9e1-238">集合初始值设定项</span><span class="sxs-lookup"><span data-stu-id="1a9e1-238">Collection Initializers</span></span>](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [<span data-ttu-id="1a9e1-239">对象和集合初始值设定项</span><span class="sxs-lookup"><span data-stu-id="1a9e1-239">Object and Collection Initializers</span></span>](../classes-and-structs/object-and-collection-initializers.md)
- [<span data-ttu-id="1a9e1-240">可选参数</span><span class="sxs-lookup"><span data-stu-id="1a9e1-240">Optional Parameters</span></span>](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)
- [<span data-ttu-id="1a9e1-241">按位置和按名称传递自变量</span><span class="sxs-lookup"><span data-stu-id="1a9e1-241">Passing Arguments by Position and by Name</span></span>](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-position-and-by-name.md)
- [<span data-ttu-id="1a9e1-242">命名参数和可选参数</span><span class="sxs-lookup"><span data-stu-id="1a9e1-242">Named and Optional Arguments</span></span>](../classes-and-structs/named-and-optional-arguments.md)
- [<span data-ttu-id="1a9e1-243">早期绑定和后期绑定</span><span class="sxs-lookup"><span data-stu-id="1a9e1-243">Early and Late Binding</span></span>](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [<span data-ttu-id="1a9e1-244">dynamic</span><span class="sxs-lookup"><span data-stu-id="1a9e1-244">dynamic</span></span>](../../language-reference/builtin-types/reference-types.md)
- [<span data-ttu-id="1a9e1-245">使用类型 dynamic</span><span class="sxs-lookup"><span data-stu-id="1a9e1-245">Using Type dynamic</span></span>](../types/using-type-dynamic.md)
- [<span data-ttu-id="1a9e1-246">Lambda 表达式 (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="1a9e1-246">Lambda Expressions (Visual Basic)</span></span>](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [<span data-ttu-id="1a9e1-247">Lambda 表达式 (C#)</span><span class="sxs-lookup"><span data-stu-id="1a9e1-247">Lambda Expressions (C#)</span></span>](../../language-reference/operators/lambda-expressions.md)
- [<span data-ttu-id="1a9e1-248">如何在 COM 互操作编程中使用索引属性</span><span class="sxs-lookup"><span data-stu-id="1a9e1-248">How to use indexed properties in COM interop programming</span></span>](./how-to-use-indexed-properties-in-com-interop-rogramming.md)
- <span data-ttu-id="1a9e1-249">[演练：在 Visual Studio 中嵌入 Microsoft Office 程序集中的类型信息](/previous-versions/visualstudio/visual-studio-2013/ee317478(v=vs.120))</span><span class="sxs-lookup"><span data-stu-id="1a9e1-249">[Walkthrough: Embedding Type Information from Microsoft Office Assemblies in Visual Studio](/previous-versions/visualstudio/visual-studio-2013/ee317478(v=vs.120))</span></span>
- [<span data-ttu-id="1a9e1-250">演练：嵌入托管程序集中的类型</span><span class="sxs-lookup"><span data-stu-id="1a9e1-250">Walkthrough: Embedding Types from Managed Assemblies</span></span>](../../../standard/assembly/embed-types-visual-studio.md)
- [<span data-ttu-id="1a9e1-251">演练：创建你的第一个 Excel VSTO 外接程序</span><span class="sxs-lookup"><span data-stu-id="1a9e1-251">Walkthrough: Creating Your First VSTO Add-in for Excel</span></span>](/visualstudio/vsto/walkthrough-creating-your-first-vsto-add-in-for-excel)
- [<span data-ttu-id="1a9e1-252">COM 互操作</span><span class="sxs-lookup"><span data-stu-id="1a9e1-252">COM Interop</span></span>](../../../visual-basic/programming-guide/com-interop/index.md)
- [<span data-ttu-id="1a9e1-253">互操作性</span><span class="sxs-lookup"><span data-stu-id="1a9e1-253">Interoperability</span></span>](./index.md)
