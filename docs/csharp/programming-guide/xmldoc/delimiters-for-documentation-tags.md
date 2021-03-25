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
# <a name="delimiters-for-documentation-tags-c-programming-guide"></a><span data-ttu-id="fdd7a-104">文档标记分隔符（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="fdd7a-104">Delimiters for documentation tags (C# programming guide)</span></span>

<span data-ttu-id="fdd7a-105">XML 文档注释需要使用分隔符，用来向编译器指示文档注释开始和结束的位置。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-105">The use of XML doc comments requires delimiters, which indicate to the compiler where a documentation comment begins and ends.</span></span> <span data-ttu-id="fdd7a-106">可以使用以下采用 XML 文档标记的分隔符：</span><span class="sxs-lookup"><span data-stu-id="fdd7a-106">You can use the following kinds of delimiters with the XML documentation tags:</span></span>

- `///`

  <span data-ttu-id="fdd7a-107">单行分隔符。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-107">Single-line delimiter.</span></span> <span data-ttu-id="fdd7a-108">这是在文档示例中显示的格式，由 C# 项目模板使用。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-108">This is the form that is shown in documentation examples and used by the C# project templates.</span></span> <span data-ttu-id="fdd7a-109">如果在分隔符后面有一个空格字符，那么此字符不会包括在 XML 输出中。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-109">If there is a white space character following the delimiter, that character is not included in the XML output.</span></span>

  > [!NOTE]
  > <span data-ttu-id="fdd7a-110">在代码编辑器中键入 `///` 分隔符后，Visual Studio 集成开发环境 (IDE) 可自动插入 `<summary>` 和 `</summary>` 标记，并在这些标记中移动游标。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-110">The Visual Studio integrated development environment (IDE) automatically inserts the `<summary>` and `</summary>` tags and moves your cursor within these tags after you type the `///` delimiter in the code editor.</span></span> <span data-ttu-id="fdd7a-111">可以在[“选项”对话框](/visualstudio/ide/reference/options-text-editor-csharp-advanced)中打开/关闭此功能。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-111">You can turn this feature on or off in the [Options dialog box](/visualstudio/ide/reference/options-text-editor-csharp-advanced).</span></span>
  
- `/** */`

  <span data-ttu-id="fdd7a-112">多行分隔符。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-112">Multiline delimiters.</span></span>

  <span data-ttu-id="fdd7a-113">使用 `/** */` 分隔符时需遵守一些格式设置规则：</span><span class="sxs-lookup"><span data-stu-id="fdd7a-113">There are some formatting rules to follow when you use the `/** */` delimiters:</span></span>
  
  - <span data-ttu-id="fdd7a-114">在包含 `/**` 分隔符的行上，如果行的其余部分为空格，则不将此行作为注释处理。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-114">On the line that contains the `/**` delimiter, if the remainder of the line is white space, the line is not processed for comments.</span></span> <span data-ttu-id="fdd7a-115">如果 `/**` 分隔符后面的第一个字符为空格，则忽略此空格字符，并处理行的其余部分。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-115">If the first character after the `/**` delimiter is white space, that white space character is ignored and the rest of the line is processed.</span></span> <span data-ttu-id="fdd7a-116">否则，将 `/**` 分隔符后面的行的所有文本作为注释的一部分进行处理。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-116">Otherwise, the entire text of the line after the `/**` delimiter is processed as part of the comment.</span></span>

  - <span data-ttu-id="fdd7a-117">在包含 `*/` 分隔符的行中，如果 `*/` 分隔符前面只有空格，此行将被忽略。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-117">On the line that contains the `*/` delimiter, if there is only white space up to the `*/` delimiter, that line is ignored.</span></span> <span data-ttu-id="fdd7a-118">否则，将 `*/` 分隔符之前的行的文本作为注释的一部分进行处理。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-118">Otherwise, the text on the line up to the `*/` delimiter is processed as part of the comment.</span></span>
  
  - <span data-ttu-id="fdd7a-119">对于以 `/**` 分隔符开头的行后面的行，编译器在各行的开头寻找共同模式。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-119">For the lines after the one that begins with the `/**` delimiter, the compiler looks for a common pattern at the beginning of each line.</span></span> <span data-ttu-id="fdd7a-120">此模式可以包含空格和星号 (`*`)，后面跟更多空格。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-120">The pattern can consist of optional white space and an asterisk (`*`), followed by more optional white space.</span></span> <span data-ttu-id="fdd7a-121">如果编译器在不以 `/**` 分隔符或 `*/` 分隔符开头的各行开头找到共同模式，则忽略此每个行的模式。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-121">If the compiler finds a common pattern at the beginning of each line that does not begin with the `/**` delimiter or the `*/` delimiter, it ignores that pattern for each line.</span></span>

  <span data-ttu-id="fdd7a-122">以下示例说明了这些规则。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-122">The following examples illustrate these rules.</span></span>

  - <span data-ttu-id="fdd7a-123">以下注释中将被处理的唯一部分是以 `<summary>` 开头的行。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-123">The only part of the following comment that's processed is the line that begins with `<summary>`.</span></span> <span data-ttu-id="fdd7a-124">三种标记格式产生的注释相同。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-124">The three tag formats produce the same comments.</span></span>

    ```csharp
    /** <summary>text</summary> */

    /**
    <summary>text</summary>
    */

    /**
     * <summary>text</summary>
    */
    ```

  - <span data-ttu-id="fdd7a-125">编译器识别出第二和第三行开头的共同模式“\*”。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-125">The compiler identifies a common pattern of " \* " at the beginning of the second and third lines.</span></span> <span data-ttu-id="fdd7a-126">此模式不包括在输出中。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-126">The pattern is not included in the output.</span></span>

    ```csharp
    /**
     * <summary>
     * text </summary>*/
    ```

  - <span data-ttu-id="fdd7a-127">编译器在下面的注释中未找到共同模式，因为第三行的第二个字符不是一个星号。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-127">The compiler finds no common pattern in the following comment because the second character on the third line is not an asterisk.</span></span> <span data-ttu-id="fdd7a-128">因此，第二和第三行上的所有文本将处理为注释的一部分。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-128">Therefore, all text on the second and third lines is processed as part of the comment.</span></span>

    ```csharp
    /**
     * <summary>
       text </summary>
    */
    ```

  - <span data-ttu-id="fdd7a-129">编译器在以下注释中未找到模式，原因有两个。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-129">The compiler finds no pattern in the following comment for two reasons.</span></span> <span data-ttu-id="fdd7a-130">首先，星号前的空格数不一致。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-130">First, the number of spaces before the asterisk is not consistent.</span></span> <span data-ttu-id="fdd7a-131">其次，第 5 行以制表符开头，这与空格不匹配。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-131">Second, the fifth line begins with a tab, which does not match spaces.</span></span> <span data-ttu-id="fdd7a-132">因此，第二到第五行的所有文本都作为注释的一部分进行处理。</span><span class="sxs-lookup"><span data-stu-id="fdd7a-132">Therefore, all text from lines two through five is processed as part of the comment.</span></span>

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

## <a name="see-also"></a><span data-ttu-id="fdd7a-133">请参阅</span><span class="sxs-lookup"><span data-stu-id="fdd7a-133">See also</span></span>

- [<span data-ttu-id="fdd7a-134">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="fdd7a-134">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="fdd7a-135">XML 文档注释</span><span class="sxs-lookup"><span data-stu-id="fdd7a-135">XML documentation comments</span></span>](./index.md)
- [<span data-ttu-id="fdd7a-136">DocumentationFile（C# 编译器选项）</span><span class="sxs-lookup"><span data-stu-id="fdd7a-136">**DocumentationFile** (C# compiler options)</span></span>](../../language-reference/compiler-options/output.md#documentationfile)
