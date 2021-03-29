---
description: 了解 C# 中的内置 nint 和 nuint 类型
title: nint 和 nuint 类型 - C# 参考
ms.date: 03/15/2021
f1_keywords:
- nint_CSharpKeyword
- nuint_CSharpKeyword
helpviewer_keywords:
- nint data type [C#]
- nuint data type [C#]
ms.openlocfilehash: fc779731d7f67fd0239f095d1dd17217a56622f6
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760819"
---
# <a name="nint-and-nuint-types-c-reference"></a>`nint` 和 `nuint` 类型（C# 参考）

从 C# 9.0 起，可以使用 `nint` 和 `nuint` 关键字定义本机大小的整数。 在 32 位进程中运行时有 32 位的整数，在 64 位进程中运行时有 64 位的整数。 这些类型可用于互操作方案、低级别的库，可用于在广泛使用整数运算的方案中提高性能。

本机大小的整数类型在内部表示为 .NET 类型 <xref:System.IntPtr?displayProperty=nameWithType> 和 <xref:System.UIntPtr?displayProperty=nameWithType>。 关键字与其他数值类型不同，它们不只是类型的别名。 以下语句并不是等效的：

```csharp
nint a = 1;
System.IntPtr a = 1;
```

编译器为 `nint` 和 `nuint` 提供适用于整数类型的操作和转换。

## <a name="run-time-native-integer-size"></a>运行时本机整数大小

若要在运行时获取本机大小的整数大小，可以使用 `sizeof()`。 但是，必须在不安全的上下文中编译代码。 例如：

:::code language="csharp" source="snippets/shared/NativeIntegerTypes.cs" id="SizeOf":::

也可以通过静态 <xref:System.IntPtr.Size?displayProperty=nameWithType> 和 <xref:System.UIntPtr.Size?displayProperty=nameWithType> 属性获得等效的值。

## <a name="minvalue-and-maxvalue"></a>MinValue 和 MaxValue

若要在运行时获取本机大小的整数的最小值和最大值，请将 `MinValue` 和 `MaxValue` 用作 `nint` 和 `nuint` 关键字的静态属性，如以下示例中所示：

:::code language="csharp" source="snippets/shared/NativeIntegerTypes.cs" id="MinMax":::

## <a name="constants"></a>常量

可以使用以下范围内的常量值：

* 对于 `nint`：<xref:System.Int32.MinValue?displayProperty=nameWithType> 到 <xref:System.Int32.MaxValue?displayProperty=nameWithType>。
* 对于 `nuint`：<xref:System.UInt32.MinValue?displayProperty=nameWithType> 到 <xref:System.UInt32.MaxValue?displayProperty=nameWithType>。

## <a name="conversions"></a>转换

编译器可将这些类型隐式和显式转换为其他数值类型。 有关详细信息，请参阅[内置数值转换](numeric-conversions.md)。

## <a name="literals"></a>文本

没有适用于本机大小整数文本的直接语法。 没有后缀可表示文本是本机大小整数，例如 `L` 表示 `long`。 可以改为使用其他整数值的隐式或显式强制转换。 例如：

```csharp
nint a = 42
nint a = (nint)42;
```

## <a name="unsupported-intptruintptr-members"></a>不支持的 IntPtr/UIntPtr 成员

`nint` 和 `nuint` 类型不支持 <xref:System.IntPtr> 和 <xref:System.UIntPtr> 的以下成员：

* 参数化的构造函数
* <xref:System.IntPtr.Add(System.IntPtr,System.Int32)>
* <xref:System.IntPtr.CompareTo%2A>
* <xref:System.IntPtr.Size> - 改用 [sizeOf()](#run-time-native-integer-size)。 不支持 `nint.Size`，但你可以使用 `IntPtr.Size` 获取等效的值。
* <xref:System.IntPtr.Subtract(System.IntPtr,System.Int32)>
* <xref:System.IntPtr.ToInt32%2A>
* <xref:System.IntPtr.ToInt64%2A>
* <xref:System.IntPtr.ToPointer%2A>
* <xref:System.IntPtr.Zero> - 改用 0。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 C# 9.0 功能建议说明的 [C# 语言规范](~/_csharplang/spec/introduction.md)和[本机大小的整数](~/_csharplang/proposals/csharp-9.0/native-integers.md)部分。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [值类型](value-types.md)
- [整型数值类型](integral-numeric-types.md)
- [内置数值转换](numeric-conversions.md)
