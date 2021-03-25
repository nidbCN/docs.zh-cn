---
title: 建议的文档注释标记 - C# 编程指南
description: 了解建议的文档注释标记。 查看建议的标记列表和其他可用资源。
ms.date: 01/21/2020
helpviewer_keywords:
- XML [C#], tags
- XML documentation [C#], tags
ms.assetid: 6e98f7a9-38f4-4d74-b644-1ff1b23320fd
ms.openlocfilehash: 74fd18a09458c399aa135552e5f6f0bca359f09e
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477831"
---
# <a name="recommended-tags-for-documentation-comments-c-programming-guide"></a><span data-ttu-id="bb50a-104">建议的文档注释标记（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="bb50a-104">Recommended tags for documentation comments (C# programming guide)</span></span>

<span data-ttu-id="bb50a-105">C# 编译器处理代码中的文档注释，并在文件中将其设置为 XML 格式，该文件的名称通过 /doc 命令行选项指定。</span><span class="sxs-lookup"><span data-stu-id="bb50a-105">The C# compiler processes documentation comments in your code and formats them as XML in a file whose name you specify in the **/doc** command-line option.</span></span> <span data-ttu-id="bb50a-106">若要基于编译器生成的文件创建最终文档，可以创建一个自定义工具，也可以使用 [DocFX](https://dotnet.github.io/docfx/) 或 [Sandcastle](https://github.com/EWSoftware/SHFB) 等工具。</span><span class="sxs-lookup"><span data-stu-id="bb50a-106">To create the final documentation based on the compiler-generated file, you can create a custom tool, or use a tool such as [DocFX](https://dotnet.github.io/docfx/) or [Sandcastle](https://github.com/EWSoftware/SHFB).</span></span>

<span data-ttu-id="bb50a-107">在类型和类型成员等代码构造中处理标记。</span><span class="sxs-lookup"><span data-stu-id="bb50a-107">Tags are processed on code constructs such as types and type members.</span></span>

> [!NOTE]
> <span data-ttu-id="bb50a-108">不可对命名空间应用文档注释。</span><span class="sxs-lookup"><span data-stu-id="bb50a-108">Documentation comments cannot be applied to a namespace.</span></span>  
  
 <span data-ttu-id="bb50a-109">编译器将处理属于有效 XML 形式的任何标记。</span><span class="sxs-lookup"><span data-stu-id="bb50a-109">The compiler will process any tag that is valid XML.</span></span> <span data-ttu-id="bb50a-110">下列标记提供用户文档的中常用功能。</span><span class="sxs-lookup"><span data-stu-id="bb50a-110">The following tags provide generally used functionality in user documentation.</span></span>  
  
## <a name="tags"></a><span data-ttu-id="bb50a-111">Tags</span><span class="sxs-lookup"><span data-stu-id="bb50a-111">Tags</span></span>  
  
|||||  
|---|---|---|---|
|[\<c>](./code-inline.md)|[\<para>](./para.md)|[\<see>](./see.md)*|[\<value>](./value.md)  
|[\<code>](./code.md)|[\<param>](./param.md)*|[\<seealso>](./seealso.md)*|  
|[\<example>](./example.md)|[\<paramref>](./paramref.md)|[\<summary>](./summary.md)|  
|[\<exception>](./exception.md)*|[\<permission>](./permission.md)*|[\<typeparam>](./typeparam.md)*|  
|[\<include>](./include.md)*|[\<remarks>](./remarks.md)|[\<typeparamref>](./typeparamref.md)|  
|[\<list>](./list.md)|[\<inheritdoc>](./inheritdoc.md)|[\<returns>](./returns.md)|
  
<span data-ttu-id="bb50a-112">（\* 表示编译器验证语法。）</span><span class="sxs-lookup"><span data-stu-id="bb50a-112">(\* denotes that the compiler verifies syntax.)</span></span>

<span data-ttu-id="bb50a-113">如果希望在文档注释的文本中显示尖括号，请使用 `<` 和 `>` 的 HTML 编码，分别为 `&lt;` 和 `&gt;`。</span><span class="sxs-lookup"><span data-stu-id="bb50a-113">If you want angle brackets to appear in the text of a documentation comment, use the HTML encoding of `<` and `>` which is `&lt;` and `&gt;` respectively.</span></span> <span data-ttu-id="bb50a-114">下面的示例对此编码进行了演示。</span><span class="sxs-lookup"><span data-stu-id="bb50a-114">This encoding is shown in the following example.</span></span>

```csharp
/// <summary>
/// This property always returns a value &lt; 1.
/// </summary>
```

## <a name="see-also"></a><span data-ttu-id="bb50a-115">请参阅</span><span class="sxs-lookup"><span data-stu-id="bb50a-115">See also</span></span>

- [<span data-ttu-id="bb50a-116">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="bb50a-116">C# programming guide</span></span>](../index.md)
- [<span data-ttu-id="bb50a-117">DocumentationFile（C# 编译器选项）</span><span class="sxs-lookup"><span data-stu-id="bb50a-117">**DocumentationFile** (C# compiler options)</span></span>](../../language-reference/compiler-options/output.md#documentationfile)
- [<span data-ttu-id="bb50a-118">XML 文档注释</span><span class="sxs-lookup"><span data-stu-id="bb50a-118">XML documentation comments</span></span>](./index.md)
