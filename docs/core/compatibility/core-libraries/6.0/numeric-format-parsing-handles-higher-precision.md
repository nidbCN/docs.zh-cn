---
title: 中断性变更：标准数字格式分析精度
description: 了解核心 .NET 库中的 .NET 6 中断性变更：标准数字格式分析现可处理更高的精度。
ms.date: 02/26/2021
ms.openlocfilehash: ad1555493bbc6198541365132cc421db7c01a5a2
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256915"
---
# <a name="standard-numeric-format-parsing-precision"></a><span data-ttu-id="74de4-103">标准数字格式分析精度</span><span class="sxs-lookup"><span data-stu-id="74de4-103">Standard numeric format parsing precision</span></span>

<span data-ttu-id="74de4-104">在使用 `ToString` 和 `TryFormat` 将数字格式化为字符串时，.NET 现支持使用更高的精度值。</span><span class="sxs-lookup"><span data-stu-id="74de4-104">.NET now supports greater precision values when formatting numbers as strings using `ToString` and `TryFormat`.</span></span>

## <a name="change-description"></a><span data-ttu-id="74de4-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="74de4-105">Change description</span></span>

<span data-ttu-id="74de4-106">在将数字格式化为字符串，[格式字符串](../../../../standard/base-types/standard-numeric-format-strings.md)中的精度说明符表示所生成的字符串中的位数。</span><span class="sxs-lookup"><span data-stu-id="74de4-106">When formatting numbers as strings, the *precision specifier* in the [format string](../../../../standard/base-types/standard-numeric-format-strings.md) represents the number of digits in the resulting string.</span></span> <span data-ttu-id="74de4-107">根据格式说明符（即[字符串开头处的字符](../../../../standard/base-types/standard-numeric-format-strings.md#standard-format-specifiers)），精度可表示总位数、有效位数或小数位数。</span><span class="sxs-lookup"><span data-stu-id="74de4-107">Depending on the *format specifier*, which is the [character at the beginning of the string](../../../../standard/base-types/standard-numeric-format-strings.md#standard-format-specifiers), the precision can represent the total number of digits, the number of significant digits, or the number of decimal digits.</span></span>

<span data-ttu-id="74de4-108">在之前的 .NET 版本中，标准数字格式分析逻辑限制为 99 或更低的精度。</span><span class="sxs-lookup"><span data-stu-id="74de4-108">In previous .NET versions, the standard numeric format parsing logic is limited to a precision of 99 or less.</span></span> <span data-ttu-id="74de4-109">某些数字类型具有更高的精度，但一些 `ToString(string format)` 不会正确公开它。</span><span class="sxs-lookup"><span data-stu-id="74de4-109">Some numeric types have more precision, but `ToString(string format)` does not expose it correctly.</span></span> <span data-ttu-id="74de4-110">如果你指定高于 99 的精度（例如 `32.ToString("C100")`），格式字符串将被解释为[自定义数字格式字符串](../../../../standard/base-types/custom-numeric-format-strings.md)而不是“精度为 100 的货币”。</span><span class="sxs-lookup"><span data-stu-id="74de4-110">If you specify a precision greater than 99, for example, `32.ToString("C100")`, the format string is interpreted as a [custom numeric format string](../../../../standard/base-types/custom-numeric-format-strings.md) instead of "currency with precision 100".</span></span> <span data-ttu-id="74de4-111">在自定义数字格式字符串中，字符将被解释为[字符文本](../../../../standard/base-types/custom-numeric-format-strings.md#character-literals)。</span><span class="sxs-lookup"><span data-stu-id="74de4-111">In custom numeric format strings, characters are interpreted as [character literals](../../../../standard/base-types/custom-numeric-format-strings.md#character-literals).</span></span> <span data-ttu-id="74de4-112">此外，根据精度值，包含无效格式说明符的格式字符串将以不同的方式解释。</span><span class="sxs-lookup"><span data-stu-id="74de4-112">In addition, a format string that contains an invalid format specifier is interpreted differently depending on the precision value.</span></span> <span data-ttu-id="74de4-113">`H99` 会对无效的格式说明符引发 <xref:System.FormatException>，而 `H100` 被解释为自定义数字格式字符串。</span><span class="sxs-lookup"><span data-stu-id="74de4-113">`H99` throws a <xref:System.FormatException> for the invalid format specifier, while `H100` is interpreted as a custom numeric format string.</span></span>

<span data-ttu-id="74de4-114">从 .NET 6 开始，.NET 支持高达 <xref:System.Int32.MaxValue?displayProperty=nameWithType> 的精度。</span><span class="sxs-lookup"><span data-stu-id="74de4-114">Starting in .NET 6, .NET supports precision up to <xref:System.Int32.MaxValue?displayProperty=nameWithType>.</span></span> <span data-ttu-id="74de4-115">由带有任意位数的格式说明符组成的格式字符串会被解释为带有精度的标准数字格式字符串。</span><span class="sxs-lookup"><span data-stu-id="74de4-115">A format string that consists of a format specifier with any number of digits is interpreted as a standard numeric format string with precision.</span></span> <span data-ttu-id="74de4-116">对于下述任一或两种情况，会引发 <xref:System.FormatException>：</span><span class="sxs-lookup"><span data-stu-id="74de4-116">A <xref:System.FormatException> is thrown for either or both of the following conditions:</span></span>

- <span data-ttu-id="74de4-117">格式说明符字符不是[标准格式说明符](../../../../standard/base-types/standard-numeric-format-strings.md#standard-format-specifiers)。</span><span class="sxs-lookup"><span data-stu-id="74de4-117">The format specifier character is not a [standard format specifier](../../../../standard/base-types/standard-numeric-format-strings.md#standard-format-specifiers).</span></span>
- <span data-ttu-id="74de4-118">精度大于 <xref:System.Int32.MaxValue?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="74de4-118">The precision is greater than <xref:System.Int32.MaxValue?displayProperty=nameWithType>.</span></span>

<span data-ttu-id="74de4-119">此更改是在影响所有数字类型的分析逻辑中实现的。</span><span class="sxs-lookup"><span data-stu-id="74de4-119">This change was implemented in the parsing logic that affects all numeric types.</span></span>

<span data-ttu-id="74de4-120">下表显示了各种格式字符串的行为更改。</span><span class="sxs-lookup"><span data-stu-id="74de4-120">The following table shows the behavior changes for various format strings.</span></span>

| <span data-ttu-id="74de4-121">格式字符串</span><span class="sxs-lookup"><span data-stu-id="74de4-121">Format string</span></span> | <span data-ttu-id="74de4-122">旧行为</span><span class="sxs-lookup"><span data-stu-id="74de4-122">Previous behavior</span></span> | <span data-ttu-id="74de4-123">.NET 6 及更高版本中的行为</span><span class="sxs-lookup"><span data-stu-id="74de4-123">.NET 6+ behavior</span></span> |
| - | - | - |
| `C2` | <span data-ttu-id="74de4-124">表示带有两个小数位数的货币</span><span class="sxs-lookup"><span data-stu-id="74de4-124">Denotes currency with two decimal digits</span></span> | <span data-ttu-id="74de4-125">表示带有两个小数位数的货币（无更改）</span><span class="sxs-lookup"><span data-stu-id="74de4-125">Denotes currency with two decimal digits (*no change*)</span></span> |
| `C100` | <span data-ttu-id="74de4-126">表示打印“C100”的自定义数字格式字符串</span><span class="sxs-lookup"><span data-stu-id="74de4-126">Denotes custom numeric format string that prints "C100"</span></span> | <span data-ttu-id="74de4-127">表示带有 100 个小数位数的货币</span><span class="sxs-lookup"><span data-stu-id="74de4-127">Denotes currency with 100 decimal digits</span></span> |
| `H99` | <span data-ttu-id="74de4-128">由于存在无效的标准格式说明符“H”，引发 <xref:System.FormatException></span><span class="sxs-lookup"><span data-stu-id="74de4-128">Throws <xref:System.FormatException> due to invalid standard format specifier "H"</span></span> | <span data-ttu-id="74de4-129">由于存在无效的标准格式说明符“H”，引发 <xref:System.FormatException>（无更改）</span><span class="sxs-lookup"><span data-stu-id="74de4-129">Throws <xref:System.FormatException> due to invalid standard format specifier "H" (*no change*)</span></span> |
| `H100` | <span data-ttu-id="74de4-130">表示自定义数字格式字符串</span><span class="sxs-lookup"><span data-stu-id="74de4-130">Denotes custom numeric format string</span></span> | <span data-ttu-id="74de4-131">由于存在无效的标准格式说明符“H”，引发 <xref:System.FormatException></span><span class="sxs-lookup"><span data-stu-id="74de4-131">Throws <xref:System.FormatException> due to invalid standard format specifier "H"</span></span> |

## <a name="version-introduced"></a><span data-ttu-id="74de4-132">引入的版本</span><span class="sxs-lookup"><span data-stu-id="74de4-132">Version introduced</span></span>

<span data-ttu-id="74de4-133">6.0 预览版 2</span><span class="sxs-lookup"><span data-stu-id="74de4-133">6.0 Preview 2</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="74de4-134">更改原因</span><span class="sxs-lookup"><span data-stu-id="74de4-134">Reason for change</span></span>

<span data-ttu-id="74de4-135">此更改会更正在对数字格式分析使用更高的精度时出现的意外行为。</span><span class="sxs-lookup"><span data-stu-id="74de4-135">This change corrects unexpected behavior when using higher precision for numeric format parsing.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="74de4-136">建议的操作</span><span class="sxs-lookup"><span data-stu-id="74de4-136">Recommended action</span></span>

<span data-ttu-id="74de4-137">在大多数情况下，无需执行任何操作，生成的字符串中会显示正确的精度。</span><span class="sxs-lookup"><span data-stu-id="74de4-137">In most cases, no action is necessary and the correct precision will be shown in the resulting strings.</span></span>

<span data-ttu-id="74de4-138">不过，如果想要还原到之前的行为（即当精度高于 99 时，格式说明符被解释为文本字符），可将字符用单引号引起来或使用反斜杠对其进行转义。</span><span class="sxs-lookup"><span data-stu-id="74de4-138">However, if you want to revert to the previous behavior where the format specifier is interpreted as a literal character when the precision is greater than 99, you can wrap that character in single quotes or escape it with a backslash.</span></span> <span data-ttu-id="74de4-139">例如，在之前的 .NET 版本中，`42.ToString("G999")` 会返回 `G999`。</span><span class="sxs-lookup"><span data-stu-id="74de4-139">For example, in previous .NET versions, `42.ToString("G999")` returns `G999`.</span></span> <span data-ttu-id="74de4-140">若要保持该行为，请将格式字符串更改为 `"'G'999"` 或 `"\\G999"`。</span><span class="sxs-lookup"><span data-stu-id="74de4-140">To maintain that behavior, change the format string to `"'G'999"` or `"\\G999"`.</span></span> <span data-ttu-id="74de4-141">这将适用于 .NET Framework、.NET Core 和 .NET 5（及更高版本）。</span><span class="sxs-lookup"><span data-stu-id="74de4-141">This will work on .NET Framework, .NET Core, and .NET 5+.</span></span>

<span data-ttu-id="74de4-142">以下格式字符串将继续被解释为自定义数字格式字符串：</span><span class="sxs-lookup"><span data-stu-id="74de4-142">The following format strings will continue to be interpreted as custom numeric format strings:</span></span>

- <span data-ttu-id="74de4-143">以任何非 ASCII 字母字符开头，例如 `$` 或 `è`。</span><span class="sxs-lookup"><span data-stu-id="74de4-143">Start with any character that is not an ASCII alphabetical character, for example, `$` or `è`.</span></span>
- <span data-ttu-id="74de4-144">以 ASCII 字母字符开头但后面不跟 ASCII 数字，例如 `A$`。</span><span class="sxs-lookup"><span data-stu-id="74de4-144">Start with an ASCII alphabetical character that's not followed by an ASCII digit, for example, `A$`.</span></span>
- <span data-ttu-id="74de4-145">以 ASCII 字母字符开头，后跟 ASCII 数字序列，然后再跟任何非 ASCII 数字字符，例如 `A12A`。</span><span class="sxs-lookup"><span data-stu-id="74de4-145">Start with an ASCII alphabetical character, followed by an ASCII digit sequence, and then any character that is not an ASCII digit character, for example, `A12A`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="74de4-146">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="74de4-146">Affected APIs</span></span>

<span data-ttu-id="74de4-147">此更改是在影响所有数字类型的分析逻辑中实现的。</span><span class="sxs-lookup"><span data-stu-id="74de4-147">This change was implemented in the parsing logic that affects all numeric types.</span></span>

- <xref:System.Numerics.BigInteger.ToString(System.String)?displayProperty=fullName>
- <xref:System.Numerics.BigInteger.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Numerics.BigInteger.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Int32.ToString(System.String)?displayProperty=fullName>
- <xref:System.Int32.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Int32.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.UInt32.ToString(System.String)?displayProperty=fullName>
- <xref:System.UInt32.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.UInt32.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Byte.ToString(System.String)?displayProperty=fullName>
- <xref:System.Byte.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Byte.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.SByte.ToString(System.String)?displayProperty=fullName>
- <xref:System.SByte.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.SByte.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Int16.ToString(System.String)?displayProperty=fullName>
- <xref:System.Int16.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Int16.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.UInt16.ToString(System.String)?displayProperty=fullName>
- <xref:System.UInt16.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.UInt16.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Int64.ToString(System.String)?displayProperty=fullName>
- <xref:System.Int64.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Int64.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.UInt64.ToString(System.String)?displayProperty=fullName>
- <xref:System.UInt64.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.UInt64.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Half.ToString(System.String)?displayProperty=fullName>
- <xref:System.Half.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Half.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Single.ToString(System.String)?displayProperty=fullName>
- <xref:System.Single.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Single.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Double.ToString(System.String)?displayProperty=fullName>
- <xref:System.Double.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Double.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Decimal.ToString(System.String)?displayProperty=fullName>
- <xref:System.Decimal.ToString(System.String,System.IFormatProvider)?displayProperty=fullName>
- <xref:System.Decimal.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)?displayProperty=fullName>

## <a name="see-also"></a><span data-ttu-id="74de4-148">另请参阅</span><span class="sxs-lookup"><span data-stu-id="74de4-148">See also</span></span>

- [<span data-ttu-id="74de4-149">标准数字格式字符串</span><span class="sxs-lookup"><span data-stu-id="74de4-149">Standard numeric format strings</span></span>](../../../../standard/base-types/standard-numeric-format-strings.md)
- [<span data-ttu-id="74de4-150">自定义格式字符串中的字符文本</span><span class="sxs-lookup"><span data-stu-id="74de4-150">Character literals in custom format strings</span></span>](../../../../standard/base-types/custom-numeric-format-strings.md#character-literals)

<!--

### Category

Core .NET libraries

### Affected APIs

- `M:System.Numerics.BigInteger.ToString(System.String)`
- `M:System.Numerics.BigInteger.ToString(System.String,System.IFormatProvider)`
- `M:System.Numerics.BigInteger.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Int32.ToString(System.String)`
- `M:System.Int32.ToString(System.String,System.IFormatProvider)`
- `M:System.Int32.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.UInt32.ToString(System.String)`
- `M:System.UInt32.ToString(System.String,System.IFormatProvider)`
- `M:System.UInt32.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Byte.ToString(System.String)`
- `M:System.Byte.ToString(System.String,System.IFormatProvider)`
- `M:System.Byte.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.SByte.ToString(System.String)`
- `M:System.SByte.ToString(System.String,System.IFormatProvider)`
- `M:System.SByte.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Int16.ToString(System.String)`
- `M:System.Int16.ToString(System.String,System.IFormatProvider)`
- `M:System.Int16.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.UInt16.ToString(System.String)`
- `M:System.UInt16.ToString(System.String,System.IFormatProvider)`
- `M:System.UInt16.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Int64.ToString(System.String)`
- `M:System.Int64.ToString(System.String,System.IFormatProvider)`
- `M:System.Int64.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.UInt64.ToString(System.String)`
- `M:System.UInt64.ToString(System.String,System.IFormatProvider)`
- `M:System.UInt64.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Half.ToString(System.String)`
- `M:System.Half.ToString(System.String,System.IFormatProvider)`
- `M:System.Half.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Single.ToString(System.String)`
- `M:System.Single.ToString(System.String,System.IFormatProvider)`
- `M:System.Single.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Double.ToString(System.String)`
- `M:System.Double.ToString(System.String,System.IFormatProvider)`
- `M:System.Double.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`
- `M:System.Decimal.ToString(System.String)`
- `M:System.Decimal.ToString(System.String,System.IFormatProvider)`
- `M:System.Decimal.TryFormat(System.Span{System.Char},System.Int32@,System.ReadOnlySpan{System.Char},System.IFormatProvider)`

-->
