---
title: 文档标记分隔符 - C# 编程指南
description: 了解文档标记分隔符。 分隔符可以向编译器指示文档注释开始和结束的位置。
ms.date: 07/20/2015
helpviewer_keywords:
- XML [C#], delimiters
- /** */ delimiters for C# documentation tags
- /// delimiter for C# documentation
ms.assetid: 9b2bdd18-4f5c-4c0b-988e-fb992e0d233e
ms.openlocfilehash: 6135827752cd3f7f43017346103f2043170521b2
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103479158"
---
# <a name="delimiters-for-documentation-tags-c-programming-guide"></a>文档标记分隔符（C# 编程指南）

XML 文档注释需要使用分隔符，用来向编译器指示文档注释开始和结束的位置。 可以使用以下采用 XML 文档标记的分隔符：

- `///`

  单行分隔符。 这是在文档示例中显示的格式，由 C# 项目模板使用。 如果在分隔符后面有一个空格字符，那么此字符不会包括在 XML 输出中。

  > [!NOTE]
  > 在代码编辑器中键入 `///` 分隔符后，Visual Studio 集成开发环境 (IDE) 可自动插入 `<summary>` 和 `</summary>` 标记，并在这些标记中移动游标。 可以在[“选项”对话框](/visualstudio/ide/reference/options-text-editor-csharp-advanced)中打开/关闭此功能。
  
- `/** */`

  多行分隔符。

  使用 `/** */` 分隔符时需遵守一些格式设置规则：
  
  - 在包含 `/**` 分隔符的行上，如果行的其余部分为空格，则不将此行作为注释处理。 如果 `/**` 分隔符后面的第一个字符为空格，则忽略此空格字符，并处理行的其余部分。 否则，将 `/**` 分隔符后面的行的所有文本作为注释的一部分进行处理。

  - 在包含 `*/` 分隔符的行中，如果 `*/` 分隔符前面只有空格，此行将被忽略。 否则，将 `*/` 分隔符之前的行的文本作为注释的一部分进行处理。
  
  - 对于以 `/**` 分隔符开头的行后面的行，编译器在各行的开头寻找共同模式。 此模式可以包含空格和星号 (`*`)，后面跟更多空格。 如果编译器在不以 `/**` 分隔符或 `*/` 分隔符开头的各行开头找到共同模式，则忽略此每个行的模式。

  以下示例说明了这些规则。

  - 以下注释中将被处理的唯一部分是以 `<summary>` 开头的行。 三种标记格式产生的注释相同。

    ```csharp
    /** <summary>text</summary> */

    /**
    <summary>text</summary>
    */

    /**
     * <summary>text</summary>
    */
    ```

  - 编译器识别出第二和第三行开头的共同模式“\*”。 此模式不包括在输出中。

    ```csharp
    /**
     * <summary>
     * text </summary>*/
    ```

  - 编译器在下面的注释中未找到共同模式，因为第三行的第二个字符不是一个星号。 因此，第二和第三行上的所有文本将处理为注释的一部分。

    ```csharp
    /**
     * <summary>
       text </summary>
    */
    ```

  - 编译器在以下注释中未找到模式，原因有两个。 首先，星号前的空格数不一致。 其次，第 5 行以制表符开头，这与空格不匹配。 因此，第二到第五行的所有文本都作为注释的一部分进行处理。

    <!-- markdownlint-disable MD010 -->
    ```csharp
    /**
      * <summary>
      * text
     *  text2
        *  </summary>
    */
    ```
    <!-- markdownlint-enable MD010 -->

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [XML 文档注释](./index.md)
- [DocumentationFile（C# 编译器选项）](../../language-reference/compiler-options/output.md#documentationfile)
