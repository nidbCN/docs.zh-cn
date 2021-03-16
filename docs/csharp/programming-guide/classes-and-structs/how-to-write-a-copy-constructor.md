---
title: 如何编写复制构造函数（C# 编程指南）
description: 了解如何使用 C# 编写复制构造函数，该构造函数采用类的实例并返回具有输入值的新实例。
ms.date: 07/20/2015
helpviewer_keywords:
- C# Language, copy constructor
- copy constructor [C#]
ms.topic: how-to
ms.custom: contperf-fy21q2
ms.assetid: fba899b5-fc41-428e-a745-3ebdbf37990a
ms.openlocfilehash: c61018444dd600d6f33765b104034355d0301667
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255231"
---
# <a name="how-to-write-a-copy-constructor-c-programming-guide"></a><span data-ttu-id="fe74d-103">如何编写复制构造函数（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="fe74d-103">How to write a copy constructor (C# Programming Guide)</span></span>

<span data-ttu-id="fe74d-104">C # [记录](records.md)为对象提供复制构造函数，但对于类，你必须自行编写。</span><span class="sxs-lookup"><span data-stu-id="fe74d-104">C# [records](records.md) provide a copy constructor for objects, but for classes you have to write one yourself.</span></span>  
  
## <a name="example"></a><span data-ttu-id="fe74d-105">示例</span><span class="sxs-lookup"><span data-stu-id="fe74d-105">Example</span></span>  

 <span data-ttu-id="fe74d-106">在下面的示例中，`Person`[类](../../language-reference/keywords/class.md)定义一个复制构造函数，该函数使用 `Person` 的实例作为其参数。</span><span class="sxs-lookup"><span data-stu-id="fe74d-106">In the following example, the `Person`[class](../../language-reference/keywords/class.md) defines a copy constructor that takes, as its argument, an instance of `Person`.</span></span> <span data-ttu-id="fe74d-107">该参数的属性值分配给 `Person` 的新实例的属性。</span><span class="sxs-lookup"><span data-stu-id="fe74d-107">The values of the properties of the argument are assigned to the properties of the new instance of `Person`.</span></span> <span data-ttu-id="fe74d-108">该代码包含一个备用复制构造函数，该函数发送要复制到该类的实例构造函数的实例的 `Name` 和 `Age` 属性。</span><span class="sxs-lookup"><span data-stu-id="fe74d-108">The code contains an alternative copy constructor that sends the `Name` and `Age` properties of the instance that you want to copy to the instance constructor of the class.</span></span>  
  
 [!code-csharp[CopyConstructor](snippets/how-to-write-a-copy-constructor/Program.cs)]

## <a name="see-also"></a><span data-ttu-id="fe74d-109">请参阅</span><span class="sxs-lookup"><span data-stu-id="fe74d-109">See also</span></span>

- <xref:System.ICloneable>
- [<span data-ttu-id="fe74d-110">记录</span><span class="sxs-lookup"><span data-stu-id="fe74d-110">Records</span></span>](records.md)
- [<span data-ttu-id="fe74d-111">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="fe74d-111">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="fe74d-112">类和结构</span><span class="sxs-lookup"><span data-stu-id="fe74d-112">Classes and Structs</span></span>](./index.md)
- [<span data-ttu-id="fe74d-113">构造函数</span><span class="sxs-lookup"><span data-stu-id="fe74d-113">Constructors</span></span>](./constructors.md)
- [<span data-ttu-id="fe74d-114">终结器</span><span class="sxs-lookup"><span data-stu-id="fe74d-114">Finalizers</span></span>](./destructors.md)
