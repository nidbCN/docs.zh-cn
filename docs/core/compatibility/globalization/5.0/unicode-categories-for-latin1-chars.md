---
title: 中断性变更：某些 Latin-1 字符的 Unicode 类别已更改
description: 了解 .NET 5 中的全球化中断性变更：Char 方法现在为 Latin-1 范围内的字符返回正确的 Unicode 类别。
ms.date: 04/07/2020
ms.openlocfilehash: 03355c488d2bdae78f989e647c9b5b7913d73649
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256668"
---
# <a name="unicode-category-changed-for-some-latin-1-characters"></a><span data-ttu-id="58957-103">某些 Latin-1 字符的 Unicode 类别已更改</span><span class="sxs-lookup"><span data-stu-id="58957-103">Unicode category changed for some Latin-1 characters</span></span>

<span data-ttu-id="58957-104">现在，<xref:System.Char> 方法为 Latin-1 范围内的字符返回正确的 Unicode 类别。</span><span class="sxs-lookup"><span data-stu-id="58957-104"><xref:System.Char> methods now return the correct Unicode category for characters in the Latin-1 range.</span></span> <span data-ttu-id="58957-105">类别与 Unicode 标准的类别匹配。</span><span class="sxs-lookup"><span data-stu-id="58957-105">The category matches that of the Unicode standard.</span></span>

## <a name="change-description"></a><span data-ttu-id="58957-106">更改说明</span><span class="sxs-lookup"><span data-stu-id="58957-106">Change description</span></span>

<span data-ttu-id="58957-107">在以前的 .NET 版本中，<xref:System.Char> 方法对 Latin-1 范围内的字符使用 Unicode 类别的固定列表。</span><span class="sxs-lookup"><span data-stu-id="58957-107">In previous .NET versions, <xref:System.Char> methods used a fixed list of Unicode categories for characters in the Latin-1 range.</span></span> <span data-ttu-id="58957-108">但是，自从实现这些 API 以来，Unicode 标准已更改其中某些字符的类别，从而造成了不一致。</span><span class="sxs-lookup"><span data-stu-id="58957-108">However, the Unicode standard has changed the categories of some of these characters since those APIs were implemented, creating a discrepancy.</span></span> <span data-ttu-id="58957-109">此外，遵循 Unicode 标准的 <xref:System.Char> 和 <xref:System.Globalization.CharUnicodeInfo> API 之间也存在不一致。</span><span class="sxs-lookup"><span data-stu-id="58957-109">In addition, there was also a discrepancy between <xref:System.Char> and <xref:System.Globalization.CharUnicodeInfo> APIs, which follow the Unicode standard.</span></span> <span data-ttu-id="58957-110">在 .NET 5 及更高版本中，<xref:System.Char> 方法使用并返回与所有字符的 Unicode 标准匹配的 Unicode 类别。</span><span class="sxs-lookup"><span data-stu-id="58957-110">In .NET 5 and later versions, <xref:System.Char> methods use and return the Unicode category that matches the Unicode standard for all characters.</span></span>

<span data-ttu-id="58957-111">下表显示了 .NET 5.0 中 Unicode 类别已更改的字符：</span><span class="sxs-lookup"><span data-stu-id="58957-111">The following table shows the characters whose Unicode categories have changed in .NET 5.0:</span></span>

| <span data-ttu-id="58957-112">字符</span><span class="sxs-lookup"><span data-stu-id="58957-112">Character</span></span>    | <span data-ttu-id="58957-113">Unicode 类别</span><span class="sxs-lookup"><span data-stu-id="58957-113">Unicode category</span></span><br><span data-ttu-id="58957-114">在以前的 .NET 版本中</span><span class="sxs-lookup"><span data-stu-id="58957-114">in previous .NET versions</span></span> | <span data-ttu-id="58957-115">Unicode 类别</span><span class="sxs-lookup"><span data-stu-id="58957-115">Unicode category</span></span><br><span data-ttu-id="58957-116">在 .NET 5 及更高版本中</span><span class="sxs-lookup"><span data-stu-id="58957-116">in .NET 5 and later versions</span></span> |
|:------------:|:---------------------------------------------:|:--------------------------------------------------:|
| <span data-ttu-id="58957-117">§ (\u00a7)</span><span class="sxs-lookup"><span data-stu-id="58957-117">§ (\u00a7)</span></span>   | `OtherSymbol`                                 | `OtherPunctuation`                                 |
| <span data-ttu-id="58957-118">ª (\u00aa)</span><span class="sxs-lookup"><span data-stu-id="58957-118">ª (\u00aa)</span></span>   | `LowercaseLetter`                             | `OtherLetter`                                      |
| <span data-ttu-id="58957-119">SHY (\u00ad)</span><span class="sxs-lookup"><span data-stu-id="58957-119">SHY (\u00ad)</span></span> | `DashPunctuation`                             | `Format`                                           |
| <span data-ttu-id="58957-120">¶ (\u00b6)</span><span class="sxs-lookup"><span data-stu-id="58957-120">¶ (\u00b6)</span></span>   | `OtherSymbol`                                 | `OtherPunctuation`                                 |
| <span data-ttu-id="58957-121">º (\u00ba)</span><span class="sxs-lookup"><span data-stu-id="58957-121">º (\u00ba)</span></span>   | `LowercaseLetter`                             | `OtherLetter`                                      |

## <a name="version-introduced"></a><span data-ttu-id="58957-122">引入的版本</span><span class="sxs-lookup"><span data-stu-id="58957-122">Version introduced</span></span>

<span data-ttu-id="58957-123">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="58957-123">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="58957-124">建议操作</span><span class="sxs-lookup"><span data-stu-id="58957-124">Recommended action</span></span>

<span data-ttu-id="58957-125">如果你有任何使用 <xref:System.Char> 类获取 Unicode 字符类别的代码，并且假定该类别永远不会改变，则可能需要对其进行更新。</span><span class="sxs-lookup"><span data-stu-id="58957-125">If you have any code that gets the Unicode character category by using the <xref:System.Char> class and assumes the category will never change, you may need to update it.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="58957-126">更改原因</span><span class="sxs-lookup"><span data-stu-id="58957-126">Reason for change</span></span>

<span data-ttu-id="58957-127">进行此更改是为了使 <xref:System.Char> 类型返回的类别与 Unicode 标准和 <xref:System.Globalization.CharUnicodeInfo> 类型保持一致。</span><span class="sxs-lookup"><span data-stu-id="58957-127">This change was made so that the categories returned by the <xref:System.Char> type are consistent with both the Unicode standard and the <xref:System.Globalization.CharUnicodeInfo> type.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="58957-128">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="58957-128">Affected APIs</span></span>

- <xref:System.Char.GetUnicodeCategory%2A?displayProperty=fullName>
- <xref:System.Char.IsLetter%2A?displayProperty=fullName>
- <xref:System.Char.IsPunctuation%2A?displayProperty=fullName>
- <xref:System.Char.IsSymbol%2A?displayProperty=fullName>
- <xref:System.Char.IsLower%2A?displayProperty=fullName>

<span data-ttu-id="58957-129">此外，此更改会影响依赖于 <xref:System.Char> 来获取 Unicode 字符类别的任何类，例如 <xref:System.Text.RegularExpressions.Regex>。</span><span class="sxs-lookup"><span data-stu-id="58957-129">Additionally, any class that depends on <xref:System.Char> to obtain the Unicode character category, for example, <xref:System.Text.RegularExpressions.Regex>, is affected by this change.</span></span>

<!--

### Affected APIs

- `Overload:System.Char.GetUnicodeCategory`
- `Overload:System.Char.IsLetter`
- `Overload:System.Char.IsPunctuation`
- `Overload:System.Char.IsSymbol`
- `Overload:System.Char.IsLower`

### Category

- Core .NET libraries
- Globalization
-
-->
