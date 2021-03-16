---
title: 中断性变更：TextInfo.ListSeparator 值更改
description: 了解 .NET 5 的中断性变更：版本 5.0 和 5.0.1 之间更改了 TextInfo.ListSeparator 的默认值。
ms.date: 12/10/2020
ms.openlocfilehash: 9a319da8ea8e3cbf62cbf4730e553b03f5bfdc89
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256720"
---
# <a name="textinfolistseparator-values-changed"></a><span data-ttu-id="f212e-103">TextInfo.ListSeparator 值已更改</span><span class="sxs-lookup"><span data-stu-id="f212e-103">TextInfo.ListSeparator values changed</span></span>

<span data-ttu-id="f212e-104">在所有操作系统上，不同区域性的默认 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值均已更改。</span><span class="sxs-lookup"><span data-stu-id="f212e-104">The default <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values for different cultures have changed on all operating systems.</span></span>

## <a name="change-description"></a><span data-ttu-id="f212e-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="f212e-105">Change description</span></span>

<span data-ttu-id="f212e-106">在 .NET 5.0.0 中，作为[从 NLS 切换到 ICU 库](icu-globalization-api.md)的一部分，不同区域性的默认 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值在 Windows 上已更改。</span><span class="sxs-lookup"><span data-stu-id="f212e-106">In .NET 5.0.0, as part of the [switch from NLS to ICU libraries](icu-globalization-api.md), the default <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values for different cultures changed on Windows.</span></span> <span data-ttu-id="f212e-107">从 International Components for Unicode (ICU) 获得的十进制分隔符值用作 <xref:System.Globalization.TextInfo.ListSeparator> 值。</span><span class="sxs-lookup"><span data-stu-id="f212e-107">Decimal separator values, obtained from International Components for Unicode (ICU), were used as the <xref:System.Globalization.TextInfo.ListSeparator> values.</span></span> <span data-ttu-id="f212e-108">在 Linux 和 macOS 上，<xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值没有变化，也就是说，它们继续使用十进制分隔符值。</span><span class="sxs-lookup"><span data-stu-id="f212e-108">On Linux and macOS, there was no change in <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values; that is, they continued to use decimal separator values.</span></span>

<span data-ttu-id="f212e-109">对于使用 .NET 5.0.1 及更高版本的所有操作系统，<xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 的值等于从 NLS 获得的值。</span><span class="sxs-lookup"><span data-stu-id="f212e-109">For all operating systems in .NET 5.0.1 and later versions, the values for <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> are equivalent to the values that would be obtained from NLS.</span></span> <span data-ttu-id="f212e-110">对于 Windows，这意味着这些值等于 .NET Framework 和 .NET Core 1.0-3.1 中的值。</span><span class="sxs-lookup"><span data-stu-id="f212e-110">For Windows, this means the values are equivalent to what they were in .NET Framework and .NET Core 1.0 - 3.1.</span></span> <span data-ttu-id="f212e-111">对于 Linux 和 macOS，<xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值现在与 Windows 的 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值匹配。</span><span class="sxs-lookup"><span data-stu-id="f212e-111">For Linux and macOS, the <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values now match the <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values for Windows.</span></span>

<span data-ttu-id="f212e-112">下表总结了对 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值所做的更改。</span><span class="sxs-lookup"><span data-stu-id="f212e-112">The following table summarizes the changes for <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values.</span></span>

| | <span data-ttu-id="f212e-113">.NET framework</span><span class="sxs-lookup"><span data-stu-id="f212e-113">.NET Framework</span></span><br/><span data-ttu-id="f212e-114">.NET Core 1.0-3.1</span><span class="sxs-lookup"><span data-stu-id="f212e-114">.NET Core 1.0 - 3.1</span></span> | <span data-ttu-id="f212e-115">.NET 5</span><span class="sxs-lookup"><span data-stu-id="f212e-115">.NET 5</span></span> | <span data-ttu-id="f212e-116">.NET 5.0.1</span><span class="sxs-lookup"><span data-stu-id="f212e-116">.NET 5.0.1</span></span> |
-|-|-|-
| <span data-ttu-id="f212e-117">Windows</span><span class="sxs-lookup"><span data-stu-id="f212e-117">**Windows**</span></span> | <span data-ttu-id="f212e-118">从 NLS 获取</span><span class="sxs-lookup"><span data-stu-id="f212e-118">Obtain from NLS</span></span> | <span data-ttu-id="f212e-119">ICU 中的十进制分隔符。</span><span class="sxs-lookup"><span data-stu-id="f212e-119">Decimal separator from ICU.</span></span><br/><span data-ttu-id="f212e-120">可切换回 NLS。</span><span class="sxs-lookup"><span data-stu-id="f212e-120">Can switch back to NLS.</span></span> | <span data-ttu-id="f212e-121">等于 NLS</span><span class="sxs-lookup"><span data-stu-id="f212e-121">Equivalent to NLS</span></span> |
| <span data-ttu-id="f212e-122">**Linux 和 macOS**</span><span class="sxs-lookup"><span data-stu-id="f212e-122">**Linux and macOS**</span></span> | <span data-ttu-id="f212e-123">ICU 中的十进制分隔符</span><span class="sxs-lookup"><span data-stu-id="f212e-123">Decimal separator from ICU</span></span> | <span data-ttu-id="f212e-124">ICU 中的十进制分隔符</span><span class="sxs-lookup"><span data-stu-id="f212e-124">Decimal separator from ICU</span></span> | <span data-ttu-id="f212e-125">等于 NLS</span><span class="sxs-lookup"><span data-stu-id="f212e-125">Equivalent to NLS</span></span> |

## <a name="version-introduced"></a><span data-ttu-id="f212e-126">引入的版本</span><span class="sxs-lookup"><span data-stu-id="f212e-126">Version introduced</span></span>

<span data-ttu-id="f212e-127">5.0.1</span><span class="sxs-lookup"><span data-stu-id="f212e-127">5.0.1</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="f212e-128">更改原因</span><span class="sxs-lookup"><span data-stu-id="f212e-128">Reason for change</span></span>

<span data-ttu-id="f212e-129">开发人员报告说，他们在分析逗号分隔值 (CSV) 文件时使用 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 属性，而新的 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值破坏了该分析。</span><span class="sxs-lookup"><span data-stu-id="f212e-129">Developers reported that they use the <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> property when parsing comma-separated value (CSV) files, and the new <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values broke that parsing.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="f212e-130">建议的操作</span><span class="sxs-lookup"><span data-stu-id="f212e-130">Recommended action</span></span>

<span data-ttu-id="f212e-131">如果代码依赖于先前的十进制分隔符值，则可以对所需的 <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> 值进行硬编码。</span><span class="sxs-lookup"><span data-stu-id="f212e-131">If your code relies on the previous decimal separator values, you can hardcode the desired <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=nameWithType> values.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="f212e-132">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="f212e-132">Affected APIs</span></span>

- <xref:System.Globalization.TextInfo.ListSeparator?displayProperty=fullName>

<!--

#### Category

- Globalization

### Affected APIs

- `P:System.Globalization.TextInfo.ListSeparator`

-->
