---
title: 中断性变更：IntPtr 和 UIntPtr 实现 IFormattable
description: 了解核心 .NET 库中的 .NET 5 中断性变更：IntPtr 和 UIntPtr 现在实现了 IFormattable。
ms.date: 11/01/2020
ms.openlocfilehash: adfb68807d5fa5f0c750fb41ef75951f63a4b2d5
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257435"
---
# <a name="intptr-and-uintptr-implement-iformattable"></a><span data-ttu-id="327ba-103">IntPtr 和 UIntPtr 实现 IFormattable</span><span class="sxs-lookup"><span data-stu-id="327ba-103">IntPtr and UIntPtr implement IFormattable</span></span>

<span data-ttu-id="327ba-104"><xref:System.IntPtr> 和 <xref:System.UIntPtr> 现在实现 <xref:System.IFormattable>。</span><span class="sxs-lookup"><span data-stu-id="327ba-104"><xref:System.IntPtr> and <xref:System.UIntPtr> now implement <xref:System.IFormattable>.</span></span> <span data-ttu-id="327ba-105">检查 <xref:System.IFormattable> 支持的功能现在可能返回这些类型的不同结果，因为它们可能以格式说明符和区域性的形式传递。</span><span class="sxs-lookup"><span data-stu-id="327ba-105">Functions that check for <xref:System.IFormattable> support may now return different results for these types, because they may pass in a format specifier and a culture.</span></span>

## <a name="change-description"></a><span data-ttu-id="327ba-106">更改描述</span><span class="sxs-lookup"><span data-stu-id="327ba-106">Change description</span></span>

<span data-ttu-id="327ba-107">在早期版本的 .NET 中，<xref:System.IntPtr> 和 <xref:System.UIntPtr> 不实现 <xref:System.IFormattable>。</span><span class="sxs-lookup"><span data-stu-id="327ba-107">In previous versions of .NET, <xref:System.IntPtr> and <xref:System.UIntPtr> do not implement <xref:System.IFormattable>.</span></span> <span data-ttu-id="327ba-108">检查 <xref:System.IFormattable> 的函数可回退到仅调用 <xref:System.IntPtr.ToString%2A?displayProperty=nameWithType> 或 <xref:System.UIntPtr.ToString%2A?displayProperty=nameWithType>，这意味着不会遵循格式说明符和区域性。</span><span class="sxs-lookup"><span data-stu-id="327ba-108">Functions that check for <xref:System.IFormattable> may fall back to just calling <xref:System.IntPtr.ToString%2A?displayProperty=nameWithType> or <xref:System.UIntPtr.ToString%2A?displayProperty=nameWithType>, which means that format specifiers and cultures are not respected.</span></span>

<span data-ttu-id="327ba-109">在 .NET 5 及更高版本中，<xref:System.IntPtr> 和 <xref:System.UIntPtr> 实现 <xref:System.IFormattable>。</span><span class="sxs-lookup"><span data-stu-id="327ba-109">In .NET 5 and later versions, <xref:System.IntPtr> and <xref:System.UIntPtr> implement <xref:System.IFormattable>.</span></span> <span data-ttu-id="327ba-110">检查 <xref:System.IFormattable> 支持的功能现在可能返回这些类型的不同结果，因为它们可能以格式说明符和区域性的形式传递。</span><span class="sxs-lookup"><span data-stu-id="327ba-110">Functions that check for <xref:System.IFormattable> support may now return different results for these types, because they may pass in a format specifier and a culture.</span></span>

<span data-ttu-id="327ba-111">此更改会影响内插字符串和 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 等方案。</span><span class="sxs-lookup"><span data-stu-id="327ba-111">This change impacts scenarios like interpolated strings and <xref:System.Console.WriteLine%2A?displayProperty=nameWithType>, among others.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="327ba-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="327ba-112">Reason for change</span></span>

<span data-ttu-id="327ba-113"><xref:System.IntPtr> 和 <xref:System.UIntPtr> 现在通过 `nint` 和 `nuint` 关键字获得 C# 语言支持。</span><span class="sxs-lookup"><span data-stu-id="327ba-113"><xref:System.IntPtr> and <xref:System.UIntPtr> now have language support in C# through the `nint` and `nuint` keywords.</span></span> <span data-ttu-id="327ba-114">更新了后备类型，以通过其他基元类型（例如 <xref:System.Int32?displayProperty=nameWithType>）公开的功能提供接近奇偶校验（如果可能）。</span><span class="sxs-lookup"><span data-stu-id="327ba-114">The backing types were updated to provide near parity (where possible) with functionality exposed by other primitive types, such as <xref:System.Int32?displayProperty=nameWithType>.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="327ba-115">引入的版本</span><span class="sxs-lookup"><span data-stu-id="327ba-115">Version introduced</span></span>

<span data-ttu-id="327ba-116">5.0</span><span class="sxs-lookup"><span data-stu-id="327ba-116">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="327ba-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="327ba-117">Recommended action</span></span>

<span data-ttu-id="327ba-118">如果不希望在显示这些类型的值时使用格式说明符或自定义区域性，则可以调用 `ToString()` 的 <xref:System.IntPtr.ToString?displayProperty=nameWithType> 和 <xref:System.UIntPtr.ToString?displayProperty=nameWithType> 重载。</span><span class="sxs-lookup"><span data-stu-id="327ba-118">If you don't want a format specifier or custom culture to be used when displaying values of these types, you can call the <xref:System.IntPtr.ToString?displayProperty=nameWithType> and <xref:System.UIntPtr.ToString?displayProperty=nameWithType> overloads of `ToString()`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="327ba-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="327ba-119">Affected APIs</span></span>

<span data-ttu-id="327ba-120">无法通过 API 分析检测到。</span><span class="sxs-lookup"><span data-stu-id="327ba-120">Not detectable via API analysis.</span></span>

<!--

### Category

Core .NET libraries

### Affected APIs

Not detectable via API analysis.

-->
