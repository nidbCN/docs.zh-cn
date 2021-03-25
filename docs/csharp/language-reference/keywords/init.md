---
description: init 关键字 - C# 参考
title: init 关键字 - C# 参考
ms.date: 03/03/2021
f1_keywords:
- init
- init_CSharpKeyword
helpviewer_keywords:
- init keyword [C#]
ms.openlocfilehash: 2271b5332c8bfd3223d0c034a44eca4e2ca0ca54
ms.sourcegitcommit: e3cf8227573e13b8e1f4e3dc007404881cdafe47
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "103190431"
---
# <a name="init-c-reference"></a><span data-ttu-id="72c23-103">init（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="72c23-103">init (C# Reference)</span></span>

<span data-ttu-id="72c23-104">在 C# 9 及更高版本中，`init` 关键字在属性或索引器中定义访问器方法。</span><span class="sxs-lookup"><span data-stu-id="72c23-104">In C# 9 and later, the `init` keyword defines an *accessor* method in a property or indexer.</span></span> <span data-ttu-id="72c23-105">init-only 资源库仅在对象构造期间为属性或索引器元素赋值。</span><span class="sxs-lookup"><span data-stu-id="72c23-105">An init-only setter assigns a value to the property or the indexer element only during object construction.</span></span> <span data-ttu-id="72c23-106">有关详细信息和示例，请参阅[属性](../../programming-guide/classes-and-structs/properties.md)、[自动实现的属性](../../programming-guide/classes-and-structs/auto-implemented-properties.md)和[索引器](../../programming-guide/indexers/index.md)。</span><span class="sxs-lookup"><span data-stu-id="72c23-106">For more information and examples, see [Properties](../../programming-guide/classes-and-structs/properties.md), [Auto-Implemented Properties](../../programming-guide/classes-and-structs/auto-implemented-properties.md), and [Indexers](../../programming-guide/indexers/index.md).</span></span>

<span data-ttu-id="72c23-107">以下示例为名为 `Seconds` 的属性同时定义 `get` 和 `init` 访问器。</span><span class="sxs-lookup"><span data-stu-id="72c23-107">The following example defines both a `get` and an `init` accessor for a property named `Seconds`.</span></span> <span data-ttu-id="72c23-108">它使用名为 `_seconds` 的私有字段备份属性值。</span><span class="sxs-lookup"><span data-stu-id="72c23-108">It uses a private field named `_seconds` to back the property value.</span></span>

[!code-csharp[init#1](snippets/InitExample1.cs)]

<span data-ttu-id="72c23-109">通常，`init` 访问器包含分配一个值的单个语句，如前面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="72c23-109">Often, the `init` accessor consists of a single statement that assigns a value, as it did in the previous example.</span></span> <span data-ttu-id="72c23-110">可以将 `init` 访问器作为表达式主体成员实现。</span><span class="sxs-lookup"><span data-stu-id="72c23-110">You can implement the `init` accessor as an expression-bodied member.</span></span> <span data-ttu-id="72c23-111">下面的示例将 `get` 和 `init` 访问器都作为表达式主体成员实现。</span><span class="sxs-lookup"><span data-stu-id="72c23-111">The following example implements both the `get` and the `init` accessors as expression-bodied members.</span></span>

[!code-csharp[init#3](snippets/InitExample3.cs)]
  
<span data-ttu-id="72c23-112">对于属性的 `get` 和 `init` 访问器不执行除设置或检索私有支持字段中的值以外的任何其他操作的简单情况，可以利用 C# 编译器对自动实现的属性的支持。</span><span class="sxs-lookup"><span data-stu-id="72c23-112">For simple cases in which a property's `get` and `init` accessors perform no other operation than setting or retrieving a value in a private backing field, you can take advantage of the C# compiler's support for auto-implemented properties.</span></span> <span data-ttu-id="72c23-113">以下示例将 `Hours` 作为自动实现的属性来实现。</span><span class="sxs-lookup"><span data-stu-id="72c23-113">The following example implements `Hours` as an auto-implemented property.</span></span>

[!code-csharp[init#2](snippets/InitExample2.cs)]
  
## <a name="c-language-specification"></a><span data-ttu-id="72c23-114">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="72c23-114">C# language specification</span></span>

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a><span data-ttu-id="72c23-115">请参阅</span><span class="sxs-lookup"><span data-stu-id="72c23-115">See also</span></span>

- [<span data-ttu-id="72c23-116">C# 参考</span><span class="sxs-lookup"><span data-stu-id="72c23-116">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="72c23-117">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="72c23-117">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="72c23-118">C# 关键字</span><span class="sxs-lookup"><span data-stu-id="72c23-118">C# Keywords</span></span>](index.md)
- [<span data-ttu-id="72c23-119">属性</span><span class="sxs-lookup"><span data-stu-id="72c23-119">Properties</span></span>](../../programming-guide/classes-and-structs/properties.md)
