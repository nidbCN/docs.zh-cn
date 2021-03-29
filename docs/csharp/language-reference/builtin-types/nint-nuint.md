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
# <a name="nint-and-nuint-types-c-reference"></a><span data-ttu-id="c9592-103">`nint` 和 `nuint` 类型（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="c9592-103">`nint` and `nuint` types (C# reference)</span></span>

<span data-ttu-id="c9592-104">从 C# 9.0 起，可以使用 `nint` 和 `nuint` 关键字定义本机大小的整数。</span><span class="sxs-lookup"><span data-stu-id="c9592-104">Starting in C# 9.0, you can use the `nint` and `nuint` keywords to define *native-sized integers*.</span></span> <span data-ttu-id="c9592-105">在 32 位进程中运行时有 32 位的整数，在 64 位进程中运行时有 64 位的整数。</span><span class="sxs-lookup"><span data-stu-id="c9592-105">These are 32-bit integers when running in a 32-bit process, or 64-bit integers when running in a 64-bit process.</span></span> <span data-ttu-id="c9592-106">这些类型可用于互操作方案、低级别的库，可用于在广泛使用整数运算的方案中提高性能。</span><span class="sxs-lookup"><span data-stu-id="c9592-106">They can be used for interop scenarios, low-level libraries, and to optimize performance in scenarios where integer math is used extensively.</span></span>

<span data-ttu-id="c9592-107">本机大小的整数类型在内部表示为 .NET 类型 <xref:System.IntPtr?displayProperty=nameWithType> 和 <xref:System.UIntPtr?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="c9592-107">The native-sized integer types are represented internally as the .NET types <xref:System.IntPtr?displayProperty=nameWithType> and <xref:System.UIntPtr?displayProperty=nameWithType>.</span></span> <span data-ttu-id="c9592-108">关键字与其他数值类型不同，它们不只是类型的别名。</span><span class="sxs-lookup"><span data-stu-id="c9592-108">Unlike other numeric types, the keywords are not simply aliases for the types.</span></span> <span data-ttu-id="c9592-109">以下语句并不是等效的：</span><span class="sxs-lookup"><span data-stu-id="c9592-109">The following statements are not equivalent:</span></span>

```csharp
nint a = 1;
System.IntPtr a = 1;
```

<span data-ttu-id="c9592-110">编译器为 `nint` 和 `nuint` 提供适用于整数类型的操作和转换。</span><span class="sxs-lookup"><span data-stu-id="c9592-110">The compiler provides operations and conversions for `nint` and `nuint` that are appropriate for integer types.</span></span>

## <a name="run-time-native-integer-size"></a><span data-ttu-id="c9592-111">运行时本机整数大小</span><span class="sxs-lookup"><span data-stu-id="c9592-111">Run-time native integer size</span></span>

<span data-ttu-id="c9592-112">若要在运行时获取本机大小的整数大小，可以使用 `sizeof()`。</span><span class="sxs-lookup"><span data-stu-id="c9592-112">To get the size of a native-sized integer at run time, you can use `sizeof()`.</span></span> <span data-ttu-id="c9592-113">但是，必须在不安全的上下文中编译代码。</span><span class="sxs-lookup"><span data-stu-id="c9592-113">However, the code must be compiled in an unsafe context.</span></span> <span data-ttu-id="c9592-114">例如：</span><span class="sxs-lookup"><span data-stu-id="c9592-114">For example:</span></span>

:::code language="csharp" source="snippets/shared/NativeIntegerTypes.cs" id="SizeOf":::

<span data-ttu-id="c9592-115">也可以通过静态 <xref:System.IntPtr.Size?displayProperty=nameWithType> 和 <xref:System.UIntPtr.Size?displayProperty=nameWithType> 属性获得等效的值。</span><span class="sxs-lookup"><span data-stu-id="c9592-115">You can also get the equivalent value from the static <xref:System.IntPtr.Size?displayProperty=nameWithType> and <xref:System.UIntPtr.Size?displayProperty=nameWithType> properties.</span></span>

## <a name="minvalue-and-maxvalue"></a><span data-ttu-id="c9592-116">MinValue 和 MaxValue</span><span class="sxs-lookup"><span data-stu-id="c9592-116">MinValue and MaxValue</span></span>

<span data-ttu-id="c9592-117">若要在运行时获取本机大小的整数的最小值和最大值，请将 `MinValue` 和 `MaxValue` 用作 `nint` 和 `nuint` 关键字的静态属性，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="c9592-117">To get the minimum and maximum values of native-sized integers at run time, use `MinValue` and `MaxValue` as static properties with the `nint` and `nuint` keywords, as in the following example:</span></span>

:::code language="csharp" source="snippets/shared/NativeIntegerTypes.cs" id="MinMax":::

## <a name="constants"></a><span data-ttu-id="c9592-118">常量</span><span class="sxs-lookup"><span data-stu-id="c9592-118">Constants</span></span>

<span data-ttu-id="c9592-119">可以使用以下范围内的常量值：</span><span class="sxs-lookup"><span data-stu-id="c9592-119">You can use constant values in the following ranges:</span></span>

* <span data-ttu-id="c9592-120">对于 `nint`：<xref:System.Int32.MinValue?displayProperty=nameWithType> 到 <xref:System.Int32.MaxValue?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="c9592-120">For `nint`: <xref:System.Int32.MinValue?displayProperty=nameWithType> to <xref:System.Int32.MaxValue?displayProperty=nameWithType>.</span></span>
* <span data-ttu-id="c9592-121">对于 `nuint`：<xref:System.UInt32.MinValue?displayProperty=nameWithType> 到 <xref:System.UInt32.MaxValue?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="c9592-121">For `nuint`: <xref:System.UInt32.MinValue?displayProperty=nameWithType> to <xref:System.UInt32.MaxValue?displayProperty=nameWithType>.</span></span>

## <a name="conversions"></a><span data-ttu-id="c9592-122">转换</span><span class="sxs-lookup"><span data-stu-id="c9592-122">Conversions</span></span>

<span data-ttu-id="c9592-123">编译器可将这些类型隐式和显式转换为其他数值类型。</span><span class="sxs-lookup"><span data-stu-id="c9592-123">The compiler provides implicit and explicit conversions to other numeric types.</span></span> <span data-ttu-id="c9592-124">有关详细信息，请参阅[内置数值转换](numeric-conversions.md)。</span><span class="sxs-lookup"><span data-stu-id="c9592-124">For more information, see [Built-in numeric conversions](numeric-conversions.md).</span></span>

## <a name="literals"></a><span data-ttu-id="c9592-125">文本</span><span class="sxs-lookup"><span data-stu-id="c9592-125">Literals</span></span>

<span data-ttu-id="c9592-126">没有适用于本机大小整数文本的直接语法。</span><span class="sxs-lookup"><span data-stu-id="c9592-126">There's no direct syntax for native-sized integer literals.</span></span> <span data-ttu-id="c9592-127">没有后缀可表示文本是本机大小整数，例如 `L` 表示 `long`。</span><span class="sxs-lookup"><span data-stu-id="c9592-127">There's no suffix to indicate that a literal is a native-sized integer, such as `L` to indicate a `long`.</span></span> <span data-ttu-id="c9592-128">可以改为使用其他整数值的隐式或显式强制转换。</span><span class="sxs-lookup"><span data-stu-id="c9592-128">You can use implicit or explicit casts of other integer values instead.</span></span> <span data-ttu-id="c9592-129">例如：</span><span class="sxs-lookup"><span data-stu-id="c9592-129">For example:</span></span>

```csharp
nint a = 42
nint a = (nint)42;
```

## <a name="unsupported-intptruintptr-members"></a><span data-ttu-id="c9592-130">不支持的 IntPtr/UIntPtr 成员</span><span class="sxs-lookup"><span data-stu-id="c9592-130">Unsupported IntPtr/UIntPtr members</span></span>

<span data-ttu-id="c9592-131">`nint` 和 `nuint` 类型不支持 <xref:System.IntPtr> 和 <xref:System.UIntPtr> 的以下成员：</span><span class="sxs-lookup"><span data-stu-id="c9592-131">The following members of <xref:System.IntPtr> and <xref:System.UIntPtr> aren't supported for `nint` and `nuint` types:</span></span>

* <span data-ttu-id="c9592-132">参数化的构造函数</span><span class="sxs-lookup"><span data-stu-id="c9592-132">Parameterized constructors</span></span>
* <xref:System.IntPtr.Add(System.IntPtr,System.Int32)>
* <xref:System.IntPtr.CompareTo%2A>
* <span data-ttu-id="c9592-133"><xref:System.IntPtr.Size> - 改用 [sizeOf()](#run-time-native-integer-size)。</span><span class="sxs-lookup"><span data-stu-id="c9592-133"><xref:System.IntPtr.Size> - Use [sizeOf()](#run-time-native-integer-size) instead.</span></span> <span data-ttu-id="c9592-134">不支持 `nint.Size`，但你可以使用 `IntPtr.Size` 获取等效的值。</span><span class="sxs-lookup"><span data-stu-id="c9592-134">Although `nint.Size` isn't supported, you can use `IntPtr.Size` to get an equivalent value.</span></span>
* <xref:System.IntPtr.Subtract(System.IntPtr,System.Int32)>
* <xref:System.IntPtr.ToInt32%2A>
* <xref:System.IntPtr.ToInt64%2A>
* <xref:System.IntPtr.ToPointer%2A>
* <span data-ttu-id="c9592-135"><xref:System.IntPtr.Zero> - 改用 0。</span><span class="sxs-lookup"><span data-stu-id="c9592-135"><xref:System.IntPtr.Zero> - Use 0 instead.</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="c9592-136">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="c9592-136">C# language specification</span></span>

<span data-ttu-id="c9592-137">有关详细信息，请参阅 C# 9.0 功能建议说明的 [C# 语言规范](~/_csharplang/spec/introduction.md)和[本机大小的整数](~/_csharplang/proposals/csharp-9.0/native-integers.md)部分。</span><span class="sxs-lookup"><span data-stu-id="c9592-137">For more information, see the [C# language specification](~/_csharplang/spec/introduction.md) and the [Native-sized integers](~/_csharplang/proposals/csharp-9.0/native-integers.md) section of the C# 9.0 feature proposal notes.</span></span>

## <a name="see-also"></a><span data-ttu-id="c9592-138">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c9592-138">See also</span></span>

- [<span data-ttu-id="c9592-139">C# 参考</span><span class="sxs-lookup"><span data-stu-id="c9592-139">C# reference</span></span>](../index.md)
- [<span data-ttu-id="c9592-140">值类型</span><span class="sxs-lookup"><span data-stu-id="c9592-140">Value types</span></span>](value-types.md)
- [<span data-ttu-id="c9592-141">整型数值类型</span><span class="sxs-lookup"><span data-stu-id="c9592-141">Integral numeric types</span></span>](integral-numeric-types.md)
- [<span data-ttu-id="c9592-142">内置数值转换</span><span class="sxs-lookup"><span data-stu-id="c9592-142">Built-in numeric conversions</span></span>](numeric-conversions.md)
