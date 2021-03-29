---
title: Winres.exe（Windows 资源本地化编辑器）
description: 使用 Winres.exe（Windows 资源本地化编辑器）。 此可视布局工具可帮助本地化专家对窗体使用的 Windows 窗体 UI 资源进行本地化。
ms.date: 08/15/2018
helpviewer_keywords:
- Winres.exe
- Windows Forms Resource Editor
- Windows Resource Localization Editor
- localization
- Windows Forms, localization
- forms, localizing
- resx files
- .resx files
ms.assetid: cb8bc835-9221-4888-af53-1a4f5fad6c48
ms.openlocfilehash: 2d66fd0d395f9d03b34c09304ce01d9c859da532
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653343"
---
# <a name="winresexe-windows-resource-localization-editor"></a>Winres.exe（Windows 资源本地化编辑器）

Windows 资源本地化编辑器 Winres.exe 是一种可视布局工具，可以帮助本地化专家对窗体使用的 Windows 窗体用户界面 (UI) 资源进行本地化。 利用可视设计环境（如 Microsoft Visual Studio），可以创建用作 Winres.exe 的输入的 .resx 或 .resources 文件。 有关在 .NET Framework 应用程序中部署资源的信息，请参阅[桌面应用中的资源](../resources/index.md)。

Winres.exe 随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

## <a name="syntax"></a>语法

```console
winres resourceFile
winres /?
```

## <a name="arguments"></a>参数

|参数|说明|
|--------------|-----------------|
|`resourceFile`|要本地化的资源文件。 此文件必须是由 Visual Studio 设计器生成的 Windows 窗体 .resx 或 .resources 文件。 Winres.exe 无法打开一般的 .resx 或 .resources 文件。|

|选项|说明|
|------------|-----------------|
|**/?**|显示该工具的命令语法和选项。|

## <a name="remarks"></a>备注

在 Windows 窗体项目的某个窗体中，UI 元素的状态通常存储在资源文件内。这些资源文件是扩展名为 .resx 的基于 XML 的文件，或者是扩展名为 .resources 的已编译的相应二进制版本文件。 Winres.exe 是一种工具，可用于在 Visual Studio 设计环境之外对上述任何一种类型的文件进行有限编辑。 具体地说，使用这种工具可以执行下面几种编辑操作：

- 可以编辑非区域性特定或区域性特定资源文件，从而更改窗体或其控件的 UI 属性，如文本、大小或位置。

- 可以从默认的资源文件生成非区域性特定或区域性特定资源文件。

- 区域性资源文件可以另存为其他区域性资源文件。 例如，英语（美国）资源文件可以另存为波兰语资源文件。 通常，可以稍后对新文件进行编辑，以使其与新区域性兼容。

另请参阅[用于本地化的资源的分层组织](/previous-versions/visualstudio/visual-studio-2012/756hydy4(v=vs.110))或[用于本地化的资源的分层组织](/previous-versions/visualstudio/visual-studio-2013/756hydy4(v=vs.120))。

Winres.exe 不能将 .resx 文件转换成相应的 .resources 文件；请改用 Resgen.exe 工具。 有关 Resgen.exe 的详细信息，请参阅 [Resgen.exe（资源文件生成器）](resgen-exe-resource-file-generator.md)。

Winres.exe 是一种图形应用程序，它只需使用资源文件即可重新创建 Windows 窗体的设计时版本，而不必访问源代码。 Winres.exe 可托管 Visual Studio 的“Windows 窗体设计器”和“属性”窗口 。 使用这些功能，可以对包含 Windows 窗体的 .resources 或 .resx 文件进行可视编辑。 通常，本地化人员使用 Winres.exe 编辑控件标签并调整控件的位置和大小，以适应目标区域性的标签。

如果 Winres.exe 无法解析某个控件的类型，它会在本地化的 .resx 或 .resources 文件中创建一个占位符控件。 占位符控件将作为一个阴影窗口显示在 Windows 窗体上。 阴影窗口的大小和位置与实际控件的大小和位置一致。 占位符控件的所有可用的可本地化属性都显示在“属性”窗口中。 对占位符控件所做的任何更改均会为实际控件保存下来。

## <a name="winresexe-versus-visual-studio"></a>Winres.exe 对比 Visual Studio

一般而言，在开始对应用程序的 Windows 窗体进行本地化之前，应先确定是使用 Visual Studio 还是使用 Winres.exe 作为本地化工具。 稍后介绍的版本兼容性可能会阻止从一个工具切换到另外一个工具。

Visual Studio 的优点在于，它既可以用来开发应用程序，也可以用来本地化应用程序。 若要对某个窗体进行本地化，在完成开发之后，可以将该窗体的 <xref:System.ComponentModel.LocalizableAttribute>（“属性”编辑器中的 Localizable 属性）设置为 `true`，并将其 Language 属性改为所需的目标区域性 。 然后，编辑字符串并调整控件的位置和大小，以适应目标区域性的字符串。 保存本地化后的 .resx 文件时，Visual Studio 只将可本地化的属性（已在目标区域性中更改的属性）写入该文件。 Visual Studio 将在正确的目录位置为本地化的 .resx 文件自动创建附属程序集。

虽然 Visual Studio 提供了一个集成的开发和本地化环境，但是，如果本地化工作由第三方本地化人员完成，则建议使用 Winres.exe 作为工具。 因为 Winres.exe 只是一个本地化工具，所以使用它可以将应用程序的代码与要本地化的窗体更加彻底地分离开来，这对于管理大型项目更为实用。

## <a name="using-winresexe"></a>使用 Winres.exe

若要使用 Winres.exe 进行本地化，必须先使用 Visual Studio 中“Windows 窗体设计器”之类的可视化设计器来开发应用程序。 开发完成后，将窗体的 <xref:System.ComponentModel.LocalizableAttribute>（“属性”编辑器中的 Localizable属性）设置为 `true`，然后将默认区域性的 .resx 文件交给第三方本地化人员 。 此 .resx 文件包含 Winres.exe 用于重新创建原始窗体的设计时版本的其他信息。

> [!NOTE]
> Winres.exe 不能用于编辑默认的资源文件。 Winres.exe 将所有更改后的属性都阐释为本地化属性，并将其保存到目标区域性资源文件中。

最后，可以使用这些区域性资源文件的最终版本创建应用程序的本地化版本。 有关详细信息，请参阅[桌面应用中的资源](../resources/index.md)。

Winres.exe 具有下列特性和功能：

- Winres 可以在单文件模式 (SFM) 或 Visual Studio 文件模式 (VSFM) 下操作。 SFM 是一种传统模式。在这种模式下，有关窗体及其内容的完整信息存储在资源文件中。 在 VSFM 模式下，在资源文件中只存储区域性更改。

- 错误报告窗口，该窗口停靠在主窗口的左下方。

- 可以通过以下方法检查热键是否重复：在“格式”菜单上单击“检查热键”命令 。

## <a name="version-compatibility"></a>版本兼容性

应使用随所用的 .NET Framework 一起发布的 Winres.exe 版本。 下表列出了可兼容的版本：

|Visual Studio|.NET Framework|Winres.exe|
|-------------------|--------------------|----------------|
|Visual Studio .NET 2002|1.0|1.0|
|Visual Studio .NET 2003|1.1|1.1|
|Visual Studio 2005|2.0|2.0|
|Visual Studio 2008|3.0 和 3.5|3.0 和 3.5|
|Visual Studio 2010|4.0|4.0|
|Visual Studio 2017|4.6|4.6|

> [!NOTE]
> 虽然 VSFM 的优点在于兼容 Visual Studio，但是，因为这种模式只在资源文件中存储更改后的值，所以 Winres.exe 要求当前资源文件的父级位于同一个目录中。 例如，在编辑德语（德国）资源文件 `TestApp.de-DE.resources` 时，需要有默认的资源文件 `TestApp.resx` 以及可能的非区域性特定资源文件 `TestApp.de.resources`。

## <a name="examples"></a>示例

### <a name="to-localize-a-resx-or-resources-file-associated-with-a-form"></a>本地化与窗体关联的 .resx 或 .resources 文件

1. 在开发人员命令提示符处键入 `winres` 以运行 Winres.exe。

2. 若要打开要本地化的窗体的默认资源，请单击“文件”菜单中的“打开”命令，然后导航到该文件打开它。

     \- 或 -

     指定启动 Winres.exe 时在命令行提示符处要打开的文件。

     下面的命令启动 Winres.exe 并在窗体设计器中加载与 `TestApp.resx` 关联的窗体。

    ```console
    winres TestApp.resx
    ```

     下面的命令启动 Winres.exe 并在窗体设计器中加载与 `TestApp.resources` 关联的窗体。

    ```console
    winres TestApp.resources
    ```

    > [!NOTE]
    > 如果正在对其资源进行编辑的窗体是一个被继承的窗体，则包含被继承的窗体的程序集和包含继承（派生的）窗体的程序集都必须在全局程序集缓存 (GAC) 中进行注册，或者必须与 WinRes.exe 位于同一个目录中。 有关将 .NET Framework 组件安装到 GAC 中的详细信息，请参阅[全局程序集缓存](../app-domains/gac.md)。

3. 选择窗体中的控件，然后更改其 <xref:System.Windows.Forms.Control.Text%2A> 和其他属性，以便反映本地化的区域性及其语言。 根据需要移动控件或调整其大小，以适应本地化后的文本。

4. 若要保存 .resx 或 .resources 文件的本地化版本，请单击“保存”图标或“文件”菜单中的相同命令。 该工具会显示“选择区域性”窗口。

5. 选择相应的区域性和文件模式，然后单击“确定”。

   该工具会使用运行时希望用于本地化资源文件的命名约定来保存文件。 例如，如果将 `TestApp.resources` 本地化为德语（德国），则该工具将该文件保存为 `TestApp.de-DE.resources`。 如果将 `TestApp.resx` 本地化为德语（德国），则该工具将该文件保存为 `TestApp.de-DE.resx`。 有关资源命名约定的详细信息，请参阅[打包和部署资源](../resources/packaging-and-deploying-resources-in-desktop-apps.md)。 有关运行时使用的预定义区域性名称的列表，请参阅 <xref:System.Globalization.CultureInfo> 类。

## <a name="see-also"></a>请参阅

- <xref:System.ComponentModel.LocalizableAttribute>
- <xref:System.Globalization.CultureInfo>
- <xref:System.Resources.ResourceManager>
- <xref:System.Resources.ResourceReader>
- <xref:System.Resources.ResourceWriter>
- [工具](index.md)
- [桌面应用中的资源](../resources/index.md)
- [全球化和本地化](../../standard/globalization-localization/index.md)
