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
# <a name="walkthrough-office-programming-c-and-visual-basic"></a>演练：Office 编程（C# 和 Visual Basic）

Visual Studio 在 C# 和 Visual Basic 中提供了改进 Microsoft Office 编程的功能。 有用的 C# 功能包括命名参数和可选参数以及类型为 `dynamic` 的返回值。 在 COM 编程中，可以省略 `ref` 关键字并获得索引属性的访问权限。 Visual Basic 中的功能包括自动实现的属性、Lambda 表达式语句和集合初始值设定项。

两种语言都支持嵌入类型信息，从而允许在不向用户的计算机部署主互操作程序集 (PIA) 的情况下部署与 COM 组件交互的程序集。 有关详细信息，请参见[演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)。

本演练演示 Office 编程上下文中的这些功能，但其中许多功能在常规编程中也极为有用。 本演练将使用 Excel 外接应用程序创建 Excel 工作簿。 然后，将创建包含工作簿链接的 Word 文档。 最后，将介绍如何启用和禁用 PIA 依赖项。

## <a name="prerequisites"></a>先决条件

若要完成本演练，计算机上必须安装 Microsoft Office Excel 和 Microsoft Office Word。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

### <a name="to-set-up-an-excel-add-in-application"></a>设置 Excel 外接应用程序

1. 启动 Visual Studio。

2. 在 **“文件”** 菜单上，指向 **“新建”** ，然后单击 **“项目”** 。

3. 在“安装的模板”窗格中，展开“Visual Basic”或“Visual C#”，再展开“Office”，然后单击 Office 产品的版本年份。

4. 在“模板”窗格中，单击“Excel \<version> 外接程序”。

5. 查看“模板”窗格的顶部，确保“.NET Framework 4”或更高版本出现在“目标框架”框中。

6. 如果需要，在“名称”框中键入项目的名称。

7. 单击 **“确定”** 。

8. 新项目将出现在“解决方案资源管理器”中。

### <a name="to-add-references"></a>添加引用

1. 在“解决方案资源管理器”中，右键单击你的项目名称，然后单击“添加引用”。 此时会显示“添加引用”对话框。

2. 在“程序集”选项卡上，在“组件名称”列表中选择“Microsoft.Office.Interop.Excel”版本 `<version>.0.0.0`（有关 Office 产品版本号的键，请参阅 [Microsoft 版本](https://en.wikipedia.org/wiki/Microsoft_Office#Versions)），然后按住 Ctrl 键并选择“Microsoft.Office.Interop.Word”，`version <version>.0.0.0`。 如果未看到程序集，则可能需要确保安装并显示它们（参阅[如何：安装 Office 主互操作程序集](/visualstudio/vsto/how-to-install-office-primary-interop-assemblies)）。

3. 单击 **“确定”** 。

### <a name="to-add-necessary-imports-statements-or-using-directives"></a>添加必要的 Imports 语句或 using 指令

1. 在“解决方案资源管理器”中，右键单击“ThisAddIn.vb”或“ThisAddIn.cs”文件，然后单击“查看代码”。

2. 将以下 `Imports` 语句 (Visual Basic) 或 `using` 指令 (C#) 添加到代码文件的顶部（如果不存在）。

     [!code-csharp[csOfficeWalkthrough#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#1)]

     [!code-vb[csOfficeWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#1)]

### <a name="to-create-a-list-of-bank-accounts"></a>创建银行帐户列表

1. 在“解决方案资源管理器”中，右键单击你的项目名称，单击“添加”，然后单击“类”。 如果使用的是 Visual Basic，则将类命名为 Account.vb；如果使用的是 C#，则将类命名为 Account.cs。 单击 **添加**。

2. 将 `Account` 类的定义替换为以下代码。 类定义使用自动实现的属性。 有关详细信息，请参阅[自动实现的属性](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)。

     [!code-csharp[csOfficeWalkthrough#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/account.cs#2)]

     [!code-vb[csOfficeWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/account.vb#2)]

3. 若要创建包含两个帐户的 `bankAccounts` 列表，请将以下代码添加到 ThisAddIn.vb 或 ThisAddIn.cs 中的 `ThisAddIn_Startup` 方法。 列表声明使用集合初始值设定项。 有关详细信息，请参阅[集合初始值设定项](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)。

     [!code-csharp[csOfficeWalkthrough#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#3)]

     [!code-vb[csOfficeWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#3)]

### <a name="to-export-data-to-excel"></a>将数据导出到 Excel

1. 在相同的文件中，将以下方法添加到 `ThisAddIn` 类。 该方法设置 Excel 工作薄并将数据导出到工作簿。

     [!code-csharp[csOfficeWalkthrough#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#4)]

     [!code-vb[csOfficeWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#4)]

     此方法使用 C# 的两项新功能。 Visual Basic 中已存在这两项功能。

    - 方法 [Add](<xref:Microsoft.Office.Interop.Excel.Workbooks.Add%2A>) 有一个 *可选参数*，用于指定特定的模板。 如果希望使用形参的默认值，你可以借助可选形参（C# 4 中新增）忽略该形参的实参。 由于上一个示例中未发送任何参数，`Add` 将使用默认模板并创建新的工作簿。 C# 早期版本中的等效语句要求提供一个占位符参数：`excelApp.Workbooks.Add(Type.Missing)`。

         有关详细信息，请参阅[命名参数和可选参数](../classes-and-structs/named-and-optional-arguments.md)。

    - [Range](<xref:Microsoft.Office.Interop.Excel.Range>) 对象的 `Range` 和 `Offset` 属性使用“索引属性”功能。 此功能允许你通过以下典型 C# 语法从 COM 类型使用这些属性。 索引属性还允许你使用 `Value` 对象的 `Range` 属性，因此不必使用 `Value2` 属性。 `Value` 属性已编入索引，但索引是可选的。 在以下示例中，可选自变量和索引属性配合使用。

         [!code-csharp[csOfficeWalkthrough#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#5)]

         在早期版本的语言中，需要以下特殊语法。

         [!code-csharp[csOfficeWalkthrough#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#6)]

         你不能创建自己的索引属性。 该功能仅支持使用现有索引属性。

         有关详细信息，请参阅[如何在 COM 互操作编程中使用索引属性](./how-to-use-indexed-properties-in-com-interop-rogramming.md)。

2. 在 `DisplayInExcel` 的末尾添加以下代码以将列宽调整为适合内容。

     [!code-csharp[csOfficeWalkthrough#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#7)]

     [!code-vb[csOfficeWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#7)]

     这些新增内容介绍了 C# 中的另一功能：处理从 COM 主机返回的 `Object` 值（如 Office），就像它们具有 [dynamic](../../language-reference/builtin-types/reference-types.md) 类型一样。 当“嵌入互操作类型”设置为其默认值 `True` 时，或者由 [EmbedInteropTypes](../../language-reference/compiler-options/inputs.md#embedinteroptypes) 编译器选项引用程序集时，会自动发生这种情况。 键入 `dynamic` 允许后期绑定（Visual Basic 已提供该功能）并可避免 C# 3.0 以及早期版本的语言中要求的显式强制转换。

     例如，`excelApp.Columns[1]` 返回`Object`，并且 `AutoFit` 是 Excel [Range](<xref:Microsoft.Office.Interop.Excel.Range>) 方法。 如果没有 `dynamic`，你必须将 `excelApp.Columns[1]` 返回的对象强制转换为 `Range` 的实例，然后才能调用 `AutoFit` 方法。

     [!code-csharp[csOfficeWalkthrough#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#8)]

     有关嵌入互操作类型的详细信息，请参阅本主题后面部分的“查找 PIA 引用”和“还原 PIA 依赖项”程序。 有关 `dynamic` 的详细信息，请参阅 [dynamic](../../language-reference/builtin-types/reference-types.md) 或[使用类型 dynamic](../types/using-type-dynamic.md)。

### <a name="to-invoke-displayinexcel"></a>调用 DisplayInExcel

1. 在 `ThisAddIn_StartUp` 方法的末尾添加以下代码。 对 `DisplayInExcel` 的调用包含两个参数。 第一个参数是要处理的帐户列表的名称。 第二个参数是定义如何处理数据的多行 lambda 表达式。 每个帐户的 `ID` 和 `balance` 值都显示在相邻的单元格中，如果余额小于零，则相应的行显示为红色。 有关详细信息，请参阅 [Lambda 表达式](../../language-reference/operators/lambda-expressions.md)。

     [!code-csharp[csOfficeWalkthrough#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#9)]

     [!code-vb[csOfficeWalkthrough#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#9)]

2. 若要运行程序，请按 F5。 出现包含帐户数据的 Excel 工作表。

### <a name="to-add-a-word-document"></a>添加 Word 文档

1. 在 `ThisAddIn_StartUp` 方法末尾添加以下代码，以创建包含指向 Excel 工作簿的链接的 Word 文档。

     [!code-csharp[csOfficeWalkthrough#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csofficewalkthrough/cs/thisaddin.cs#10)]

     [!code-vb[csOfficeWalkthrough#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csofficewalkthrough/vb/thisaddin.vb#10)]

     此代码展示 C# 中的几项新功能：省略 COM 编程中的 `ref` 关键字、命名参数以及可选参数的能力。 Visual Basic 中已存在这些功能。 [PasteSpecial](<xref:Microsoft.Office.Interop.Word.Selection.PasteSpecial%2A>) 方法有七个参数，所有参数都定义为可选引用参数。 通过命名实参和可选实参，你可以指定希望按名称访问的形参并仅将实参发送到这些形参。 在本示例中，发送实参以指示应创建指向剪贴板上工作簿的链接（形参 `Link`）并指示该链接应在 Word 文档中显示为图标（形参 `DisplayAsIcon`）。 Visual C# 还允许忽略这些参数的 `ref` 关键字。

### <a name="to-run-the-application"></a>要运行应用程序

1. 按 F5 运行该应用程序。 Excel 启动并显示包含 `bankAccounts` 中两个帐户的信息的表。 然后，出现包含指向 Excel 表的 Word 文档。

### <a name="to-clean-up-the-completed-project"></a>清理已完成的项目

1. 在 Visual Studio 中，单击“生成”菜单上的“清理解决方案”。 否则，每次在计算机上打开 Excel 时都会运行外接应用程序。

### <a name="to-find-the-pia-reference"></a>查找 PIA 引用

1. 再次运行应用程序，但不单击“清理解决方案”。

2. 选择“开始”。 找到“Microsoft Visual Studio \<version>”，然后打开开发人员命令提示。

3. 在“Visual Studio 的开发人员命令提示”窗口中键入 `ildasm`，然后按 Enter。 此时将出现 IL DASM 窗口。

4. 在 IL DASM 窗口的“文件”菜单上，选择“文件” > “打开”。 双击“Visual Studio \<version>”，然后双击“项目”。 打开项目的文件夹，在 bin/Debug 文件夹中查找 *项目名称*.dll。 双击 *项目名称*.dll。 新窗口将显示项目的属性以及对其他模块和程序集的引用。 注意，命名空间 `Microsoft.Office.Interop.Excel` 和 `Microsoft.Office.Interop.Word` 包含在程序集中。 在 Visual Studio 中，编译器默认将所需的类型从引用的 PIA 导入程序集。

     有关详细信息，请参阅[如何：查看程序集内容](../../../standard/assembly/view-contents.md)。

5. 双击“清单”图标。 此时将出现包含程序集列表的窗口，这些程序集包含项目所引用的项。 `Microsoft.Office.Interop.Excel` 和 `Microsoft.Office.Interop.Word` 未包含在列表中。 由于项目需要的类型已导入程序集中，因此不需要引用 PIA。 这使得部署变得更加容易。 用户的计算机上不必存在 PIA，因为应用程序不需要部署特定版本的 PIA，应用程序可设计为与多个版本的 Office 配合使用，前提是所有版本中都存在必要的 API。

     由于不再需要部署 PIA，你可以提前创建可与多个版本的 Office（包括之前的版本）配合使用的应用程序。 但是，仅当你的代码不使用你当前所使用 Office 版本中不可用的任何 API 时，此情况才适用。 特殊 API 在早期版本中是否可用并不始终明确，因此不建议使用早期版本的 Office。

    > [!NOTE]
    > 在 Office 2003 以前，Office 并不发布 PIA。 因此，生成适用于 Office 2002 或早期版本的互操作程序集的唯一方法是导入 COM 引用。

6. 关闭清单窗口和程序集窗口。

### <a name="to-restore-the-pia-dependency"></a>还原 PIA 依赖项

1. 在“解决方案资源管理器”中，单击“显示所有文件”按钮。 展开“引用”文件夹并选择“Microsoft.Office.Interop.Excel”。 按 F4 以显示“属性”窗口。

2. 在“属性”窗口中，将“嵌入互操作类型”属性从“True”更改为“False”。

3. 对 `Microsoft.Office.Interop.Word` 重复此程序中的步骤 1 和 2。

4. 在 C# 中，在 `Autofit` 方法的末尾注释掉对 `DisplayInExcel` 的两次调用。

5. 按 F5 以验证项目是否仍正确运行。

6. 重复上一个程序的步骤 1-3 以打开程序集窗口。 注意，`Microsoft.Office.Interop.Word` 和 `Microsoft.Office.Interop.Excel` 不再位于嵌入程序集列表中。

7. 双击“清单”图标并滚动引用程序集的列表。 `Microsoft.Office.Interop.Word` 和 `Microsoft.Office.Interop.Excel` 均位于列表中。 由于应用程序引用 Excel 和 Word PIA 并且“嵌入互操作类型”属性设置为“False”，因此最终用户的计算机上必须存在两个程序集。

8. 在 Visual Studio 中，单击“生成”菜单上的“清理解决方案”以清理完成的项目。

## <a name="see-also"></a>请参阅

- [自动实现的属性 (Visual Basic)](../../../visual-basic/programming-guide/language-features/procedures/auto-implemented-properties.md)
- [自动实现的属性 (C#)](../classes-and-structs/auto-implemented-properties.md)
- [集合初始值设定项](../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [对象和集合初始值设定项](../classes-and-structs/object-and-collection-initializers.md)
- [可选参数](../../../visual-basic/programming-guide/language-features/procedures/optional-parameters.md)
- [按位置和按名称传递自变量](../../../visual-basic/programming-guide/language-features/procedures/passing-arguments-by-position-and-by-name.md)
- [命名参数和可选参数](../classes-and-structs/named-and-optional-arguments.md)
- [早期绑定和后期绑定](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [dynamic](../../language-reference/builtin-types/reference-types.md)
- [使用类型 dynamic](../types/using-type-dynamic.md)
- [Lambda 表达式 (Visual Basic)](../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)
- [Lambda 表达式 (C#)](../../language-reference/operators/lambda-expressions.md)
- [如何在 COM 互操作编程中使用索引属性](./how-to-use-indexed-properties-in-com-interop-rogramming.md)
- [演练：在 Visual Studio 中嵌入 Microsoft Office 程序集中的类型信息](/previous-versions/visualstudio/visual-studio-2013/ee317478(v=vs.120))
- [演练：嵌入托管程序集中的类型](../../../standard/assembly/embed-types-visual-studio.md)
- [演练：创建你的第一个 Excel VSTO 外接程序](/visualstudio/vsto/walkthrough-creating-your-first-vsto-add-in-for-excel)
- [COM 互操作](../../../visual-basic/programming-guide/com-interop/index.md)
- [互操作性](./index.md)
