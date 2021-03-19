---
title: 无法根据这些实参推断类型形参的数据类型
description: 了解有关以下内容的详细信息： BC36647 和 BC36644：无法从这些自变量推断类型形参 () s) 的数据类型 (
ms.date: 07/20/2015
f1_keywords:
- bc36644
- bc36647
- vbc36647
- vbc36644
helpviewer_keywords:
- BC36644
- BC36647
ms.assetid: 0e0050f2-2039-4311-b260-f0ebfde84189
ms.openlocfilehash: 26b4358dc2059aaff0a6a72d1489216c854f79a6
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653668"
---
# <a name="bc36647-and-bc36644-data-types-of-the-type-parameters-cannot-be-inferred-from-these-arguments"></a><span data-ttu-id="ca575-103">BC36647 和 BC36644：无法从这些实参推断类型形参 () s) 的数据类型 (</span><span class="sxs-lookup"><span data-stu-id="ca575-103">BC36647 and BC36644: Data type(s) of the type parameter(s) cannot be inferred from these arguments</span></span>

<span data-ttu-id="ca575-104">无法根据这些参数推断出类型形参 () s) 的数据类型 (。</span><span class="sxs-lookup"><span data-stu-id="ca575-104">Data type(s) of the type parameter(s) cannot be inferred from these arguments.</span></span> <span data-ttu-id="ca575-105">显式指定数据类型可更正此错误。</span><span class="sxs-lookup"><span data-stu-id="ca575-105">Specifying the data type(s) explicitly might correct this error.</span></span>

<span data-ttu-id="ca575-106">当重载决策失败时发生此错误。</span><span class="sxs-lookup"><span data-stu-id="ca575-106">This error occurs when overload resolution has failed.</span></span> <span data-ttu-id="ca575-107">它以从属消息的形式发生，该消息指出已消除特定的重载候选。</span><span class="sxs-lookup"><span data-stu-id="ca575-107">It occurs as a subordinate message that states why a particular overload candidate has been eliminated.</span></span> <span data-ttu-id="ca575-108">错误消息说明编译器无法使用类型推理来查找类型参数的数据类型。</span><span class="sxs-lookup"><span data-stu-id="ca575-108">The error message explains that the compiler cannot use type inference to find data types for the type parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="ca575-109">当无法指定实参时（例如，对于查询表达式中的查询运算符），显示的错误消息不包括第二个句子。</span><span class="sxs-lookup"><span data-stu-id="ca575-109">When specifying arguments is not an option (for example, for query operators in query expressions), the error message appears without the second sentence.</span></span>

<span data-ttu-id="ca575-110">下面的代码演示了此错误。</span><span class="sxs-lookup"><span data-stu-id="ca575-110">The following code demonstrates the error.</span></span>

```vb
Module Module1

    Sub Main()

        '' Not Valid.
        'OverloadedGenericMethod("Hello", "World")

    End Sub

    Sub OverloadedGenericMethod(Of T)(ByVal x As String,
                                      ByVal y As InterfaceExample(Of T))
    End Sub

    Sub OverloadedGenericMethod(Of T, R)(ByVal x As T,
                                         ByVal y As InterfaceExample(Of R))
    End Sub

End Module

Interface InterfaceExample(Of T)
End Interface
```

<span data-ttu-id="ca575-111">**错误 ID：** BC36647 和 BC36644</span><span class="sxs-lookup"><span data-stu-id="ca575-111">**Error ID:** BC36647 and BC36644</span></span>

## <a name="to-correct-this-error"></a><span data-ttu-id="ca575-112">更正此错误</span><span class="sxs-lookup"><span data-stu-id="ca575-112">To correct this error</span></span>

<span data-ttu-id="ca575-113">你或许能够指定类型形参的数据类型，而无需依赖类型推断。</span><span class="sxs-lookup"><span data-stu-id="ca575-113">You may be able to specify a data type for the type parameter or parameters instead of relying on type inference.</span></span>

## <a name="see-also"></a><span data-ttu-id="ca575-114">请参阅</span><span class="sxs-lookup"><span data-stu-id="ca575-114">See also</span></span>

- [<span data-ttu-id="ca575-115">宽松委托转换</span><span class="sxs-lookup"><span data-stu-id="ca575-115">Relaxed Delegate Conversion</span></span>](../../programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [<span data-ttu-id="ca575-116">Generic Procedures in Visual Basic</span><span class="sxs-lookup"><span data-stu-id="ca575-116">Generic Procedures in Visual Basic</span></span>](../../programming-guide/language-features/data-types/generic-procedures.md)
- [<span data-ttu-id="ca575-117">Visual Basic 中的类型转换</span><span class="sxs-lookup"><span data-stu-id="ca575-117">Type Conversions in Visual Basic</span></span>](../../programming-guide/language-features/data-types/type-conversions.md)
