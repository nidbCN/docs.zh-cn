---
description: Checked 和 Unchecked - C# 参考
title: Checked 和 Unchecked - C# 参考
ms.date: 05/15/2018
helpviewer_keywords:
- operators [C#], checked and unchecked
- exceptions [C#], overflow checking
- checked statement [C#]
- overflow checking [C#]
- unchecked statement [C#]
- statements [C#], checked and unchecked
ms.assetid: a84bc877-2c7f-4396-8735-1ce97c42f35e
ms.openlocfilehash: 0121090265881bfa8287e2f9e83ad4b886bf17c1
ms.sourcegitcommit: 0bb8074d524e0dcf165430b744bb143461f17026
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2021
ms.locfileid: "103477482"
---
# <a name="checked-and-unchecked-c-reference"></a>Checked 和 Unchecked（C# 参考）

C# 语句既可以在已检查的上下文中执行，也可以在未检查的上下文中执行。 在已检查的上下文中，算法溢出引发异常。 在未选中的上下文中忽略算术溢出并将结果截断，方法是：丢弃任何不适应目标类型的高序位。  
  
- [checked](checked.md) 指定已检查的上下文。  
  
- [unchecked](unchecked.md) 指定未检查的上下文。  
  
 下列操作受溢出检查的影响：  
  
- 表达式在整型上使用下列预定义运算符：  
  
     `++`，`--`，一元 `-`，`+`，`-`，`*`，`/`  
  
- 整型类型之间或从 `float` 或 `double` 到整型类型的显式数字转换。  
  
 如果既未指定 `checked`，也未指定 `unchecked`，则非常量表达式（在运行时计算的表达式）的默认上下文将由 [CheckForOverflowUnderflow](../compiler-options/language.md#checkforoverflowunderflow) 编译器选项的值定义。 默认情况下，该选项的值未设置，且算术运算在未选中的上下文中执行。

 对于常量表达式（可在编译时完全计算的表达式），将始终选中默认上下文。 除非在未选中的上下文中显式放置常量表达式，否则在编译时间计算表达式过程中出现的溢出将导致编译时错误。
  
## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 编程指南](../../programming-guide/index.md)
- [C# 关键字](index.md)
- [语句关键字](statement-keywords.md)
