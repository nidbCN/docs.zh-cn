---
description: 了解 C# 中内置数值转换间的隐式转换和显式转换
title: 内置数值转换 - C# 参考
ms.date: 03/17/2021
helpviewer_keywords:
- implicit numeric conversions [C#]
- explicit numeric conversion [C#]
- numeric conversions [C#], implicit
- numeric conversions [C#], explicit
- conversions [C#], implicit numeric
- conversions [C#], explicit numeric
ms.openlocfilehash: 5ff0289f5365a7d3d334dd0130b3b0efcdf34c60
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759685"
---
# <a name="built-in-numeric-conversions-c-reference"></a>内置数值转换（C# 参考）

C# 提供了一组[整型](integral-numeric-types.md)和[浮点](floating-point-numeric-types.md)数值类型。 任何两种数值类型之间都可以进行隐式或显式转换。 必须使用[强制转换表达式](../operators/type-testing-and-cast.md#cast-expression)来执行显式转换。

## <a name="implicit-numeric-conversions"></a>隐式数值转换

下表显示内置数值类型之间的预定义隐式转换：

|From|到|
|----------|--------|
|[sbyte](integral-numeric-types.md)|`short`、`int`、`long`、`float`、`double`、`decimal` 或 `nint`。|
|[byte](integral-numeric-types.md)|`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`、`nint` 或 `nuint`|
|[short](integral-numeric-types.md)|`int`、`long`、`float`、`double`、`decimal` 或 `nint`|
|[ushort](integral-numeric-types.md)|`int`、`uint`、`long`、`ulong`、`float`、`double`、`decimal`、`nint` 或 `nuint`|
|[int](integral-numeric-types.md)|`long`、`float`、`double`、`decimal` 或 `nint`|
|[uint](integral-numeric-types.md)|`long`、`ulong`、`float`、`double`、`decimal` 或 `nuint`|
|[long](integral-numeric-types.md)|`float`、`double` 或 `decimal`|
|[ulong](integral-numeric-types.md)|`float`、`double` 或 `decimal`|
|[float](floating-point-numeric-types.md)|`double`|
|[nint](nint-nuint.md)|`long`、`float`、`double` 或 `decimal`|
|[nuint](nint-nuint.md)|`ulong`、`float`、`double` 或 `decimal`|

> [!NOTE]
> 从 `int`、`uint`、`long`、`ulong`、`nint` 或 `nuint` 到 `float` 的隐式转换以及从 `long`、`ulong`、`nint` 或 `nuint` 到 `double` 的隐式转换可能会丢失精准率，但绝不会丢失一个数量级。 其他隐式数值转换不会丢失任何信息。

另请注意

- 任何[整型数值类型](integral-numeric-types.md)都可以隐式转换为任何[浮点数值类型](floating-point-numeric-types.md)。

- 不存在针对 `byte` 和 `sbyte` 类型的隐式转换。 不存在从 `double` 和 `decimal` 类型的隐式转换。

- `decimal` 类型和 `float` 或 `double` 类型之间不存在隐式转换。

- 类型 `int` 的常量表达式的值（例如，由整数文本所表示的值）如果在目标类型的范围内，则可隐式转换为 `sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong`、`nint` 或 `nuint`：

  ```csharp
  byte a = 13;
  byte b = 300;  // CS0031: Constant value '300' cannot be converted to a 'byte'
  ```

  如前面的示例所示，如果该常量值不在目标类型的范围内，则发生编译器错误 [CS0031](../../misc/cs0031.md)。

## <a name="explicit-numeric-conversions"></a>显式数值转换

下表显示不存在[隐式转换](#implicit-numeric-conversions)的内置数值类型之间的预定义显式转换：

|From|到|
|----------|--------|
|[sbyte](integral-numeric-types.md)|`byte`、`ushort`、`uint`、`ulong` 或 `nuint`|
|[byte](integral-numeric-types.md)|`sbyte`|
|[short](integral-numeric-types.md)|`sbyte`、`byte`、`ushort`、`uint`、`ulong` 或 `nuint`|
|[ushort](integral-numeric-types.md)|`sbyte`、`byte` 或 `short`|
|[int](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`uint`、`ulong` 或 `nuint`。|
|[uint](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort` 或 `int`|
|[long](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong`、`nint` 或 `nuint`|
|[ulong](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`nint` 或 `nuint`|
|[float](floating-point-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`decimal`、`nint` 或 `nuint`|
|[double](floating-point-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`decimal`、`nint` 或 `nuint`|
|[decimal](floating-point-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、`nint` 或 `nuint`|
|[nint](nint-nuint.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`ulong` 或 `nuint`|
|[nuint](nint-nuint.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long` 或 `nint`|

> [!NOTE]
> 显式数值转换可能会导致数据丢失或引发异常，通常为 <xref:System.OverflowException>。

另请注意

- 将整数类型的值转换为另一个整数类型时，结果取决于溢出[检查上下文](../keywords/checked-and-unchecked.md)。 在已检查的上下文中，如果源值在目标类型的范围内，则转换成功。 否则会引发 <xref:System.OverflowException>。 在未检查的上下文中，转换始终成功，并按如下方式进行：

  - 如果源类型大于目标类型，则通过放弃其“额外”最高有效位来截断源值。 结果会被视为目标类型的值。

  - 如果源类型小于目标类型，则源值是符号扩展或零扩展，以使其与目标类型的大小相同。 如果源类型带符号，则是符号扩展；如果源类型是无符号的，则是零扩展。 结果会被视为目标类型的值。

  - 如果源类型与目标类型的大小相同，则源值将被视为目标类型的值。

- 将 `decimal` 值转换为整型类型时，此值会向零舍入到最接近的整数值。 如果生成的整数值处于目标类型的范围之外，则会引发 <xref:System.OverflowException>。

- 将 `double` 或 `float` 值转换为整型类型时，此值会向零舍入到最接近的整数值。 如果生成的整数值处于目标类型范围之外，则结果会取决于溢出[上下文](../keywords/checked-and-unchecked.md)。 在已检查的上下文中，引发 <xref:System.OverflowException>；而在未检查的上下文中，结果是目标类型的未指定值。

- 将 `double` 转换为 `float` 时，`double` 值舍入为最接近的 `float` 值。 如果 `double` 值太小或太大，无法匹配 `float` 类型，结果将为零或无穷大。

- 将 `float` 或 `double` 转换为 `decimal` 时，源值转换为 `decimal` 表示形式，并并五入到第 28 位小数后最接近的数（如果需要）。 根据源值的值，可能出现以下结果之一：

  - 如果源值太小，无法表示为 `decimal`，结果则为零。

  - 如果源值为 NaN（非数值）、无穷大或太大而无法表示为 `decimal`，则引发 <xref:System.OverflowException>。

- 将 `decimal` 转换为 `float` 或 `double` 时，源值分别舍入为最接近的 `float` 或 `double` 值。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [隐式数值转换](~/_csharplang/spec/conversions.md#implicit-numeric-conversions)
- [显式数值转换](~/_csharplang/spec/conversions.md#explicit-numeric-conversions)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [强制转换和类型转换](../../programming-guide/types/casting-and-type-conversions.md)
