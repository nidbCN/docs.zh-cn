---
description: extern 别名 - C# 参考
title: extern 别名 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- alias_CSharpKeyword
helpviewer_keywords:
- extern alias keyword [C#]
- aliases [C#], extern keyword
- aliases, extern keyword
ms.assetid: f487bf4f-c943-4fca-851b-e540c83d9027
ms.openlocfilehash: 230e9d7169e8924749f234af2fa411950f68ce78
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477466"
---
# <a name="extern-alias-c-reference"></a><span data-ttu-id="f7610-103">外部别名（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="f7610-103">extern alias (C# Reference)</span></span>

<span data-ttu-id="f7610-104">有时你可能不得不引用具有相同的完全限定类型名称的程序集的两个版本。</span><span class="sxs-lookup"><span data-stu-id="f7610-104">You might have to reference two versions of assemblies that have the same fully-qualified type names.</span></span> <span data-ttu-id="f7610-105">例如，可能需要在同一应用程序中使用某程序集的两个或多个版本。</span><span class="sxs-lookup"><span data-stu-id="f7610-105">For example, you might have to use two or more versions of an assembly in the same application.</span></span> <span data-ttu-id="f7610-106">通过使用外部程序集别名，可在别名命名的根级别命名空间内包装每个程序集的命名空间，使其能够在同一文件中使用。</span><span class="sxs-lookup"><span data-stu-id="f7610-106">By using an external assembly alias, the namespaces from each assembly can be wrapped inside root-level namespaces named by the alias, which enables them to be used in the same file.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="f7610-107">[外部](./extern.md)关键字还被用作方法修饰符，用于声明在非托管代码中编写的方法。</span><span class="sxs-lookup"><span data-stu-id="f7610-107">The [extern](./extern.md) keyword is also used as a method modifier, declaring a method written in unmanaged code.</span></span>  
  
 <span data-ttu-id="f7610-108">若要引用具有相同的完全限定类型名称的两个程序集，必须在命令提示符处指定别名，如下所示：</span><span class="sxs-lookup"><span data-stu-id="f7610-108">To reference two assemblies with the same fully-qualified type names, an alias must be specified at a command prompt, as follows:</span></span>  
  
 `/r:GridV1=grid.dll`  
  
 `/r:GridV2=grid20.dll`  
  
 <span data-ttu-id="f7610-109">这将创建外部别名 `GridV1` 和 `GridV2`。</span><span class="sxs-lookup"><span data-stu-id="f7610-109">This creates the external aliases `GridV1` and `GridV2`.</span></span> <span data-ttu-id="f7610-110">若要从程序中使用这些别名，请通过使用 `extern` 关键字引用它们。</span><span class="sxs-lookup"><span data-stu-id="f7610-110">To use these aliases from within a program, reference them by using the `extern` keyword.</span></span> <span data-ttu-id="f7610-111">例如:</span><span class="sxs-lookup"><span data-stu-id="f7610-111">For example:</span></span>  
  
 `extern alias GridV1;`  
  
 `extern alias GridV2;`  
  
 <span data-ttu-id="f7610-112">每个外部别名声明都会引入与全局命名空间并行（但不位于其中）的额外根级别命名空间。</span><span class="sxs-lookup"><span data-stu-id="f7610-112">Each extern alias declaration introduces an additional root-level namespace that parallels (but does not lie within) the global namespace.</span></span> <span data-ttu-id="f7610-113">因此，可以使用其完全限定的名称（根植于相应的命名空间别名中）无歧义地引用每个程序集的类型。</span><span class="sxs-lookup"><span data-stu-id="f7610-113">Thus types from each assembly can be referred to without ambiguity by using their fully qualified name, rooted in the appropriate namespace-alias.</span></span>  
  
 <span data-ttu-id="f7610-114">在上一示例中，`GridV1::Grid` 是 `grid.dll` 中的网格控件，`GridV2::Grid` 是 `grid20.dll` 中的网格控件。</span><span class="sxs-lookup"><span data-stu-id="f7610-114">In the previous example, `GridV1::Grid` would be the grid control from `grid.dll`, and `GridV2::Grid` would be the grid control from `grid20.dll`.</span></span>  
  
## <a name="using-visual-studio"></a><span data-ttu-id="f7610-115">使用 Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f7610-115">Using Visual Studio</span></span>

<span data-ttu-id="f7610-116">如果你使用的是 Visual Studio，可以按类似方式提供别名。</span><span class="sxs-lookup"><span data-stu-id="f7610-116">If you are using Visual Studio, aliases can be provided in similar way.</span></span>

<span data-ttu-id="f7610-117">在 Visual Studio 中向项目添加 grid.dll 和 grid20.dll 的引用。</span><span class="sxs-lookup"><span data-stu-id="f7610-117">Add reference of *grid.dll* and *grid20.dll* to your project in Visual Studio.</span></span> <span data-ttu-id="f7610-118">打开“属性”选项卡，并将别名从“全局”分别更改为“GridV1”和“GridV2”。</span><span class="sxs-lookup"><span data-stu-id="f7610-118">Open a property tab and change the Aliases from global to GridV1 and GridV2 respectively.</span></span>

<span data-ttu-id="f7610-119">按上述方式使用这些别名</span><span class="sxs-lookup"><span data-stu-id="f7610-119">Use these aliases the same way above</span></span>

```csharp
 extern alias GridV1;  
  
 extern alias GridV2;  
```

<span data-ttu-id="f7610-120">现在，可以通过使用别名指令为命名空间或类型创建别名。</span><span class="sxs-lookup"><span data-stu-id="f7610-120">Now you can create alias for a namespace or a type by *using alias directive*.</span></span> <span data-ttu-id="f7610-121">有关详细信息，请参阅 [using 指令](using-directive.md)。</span><span class="sxs-lookup"><span data-stu-id="f7610-121">For more information, see [using directive](using-directive.md).</span></span>

```csharp
using Class1V1 = GridV1::Namespace.Class1;

using Class1V2 = GridV2::Namespace.Class1;
```

## <a name="c-language-specification"></a><span data-ttu-id="f7610-122">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="f7610-122">C# Language Specification</span></span>  

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a><span data-ttu-id="f7610-123">请参阅</span><span class="sxs-lookup"><span data-stu-id="f7610-123">See also</span></span>

- [<span data-ttu-id="f7610-124">C# 参考</span><span class="sxs-lookup"><span data-stu-id="f7610-124">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="f7610-125">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="f7610-125">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="f7610-126">C# 关键字</span><span class="sxs-lookup"><span data-stu-id="f7610-126">C# Keywords</span></span>](./index.md)
- [<span data-ttu-id="f7610-127">::运算符</span><span class="sxs-lookup"><span data-stu-id="f7610-127">:: Operator</span></span>](../operators/namespace-alias-qualifier.md)
- [<span data-ttu-id="f7610-128">引用（C# 编译器选项）</span><span class="sxs-lookup"><span data-stu-id="f7610-128">**References** (C# Compiler Options)</span></span>](../compiler-options/inputs.md#references)
