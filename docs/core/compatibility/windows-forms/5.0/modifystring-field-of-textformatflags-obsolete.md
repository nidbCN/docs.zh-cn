---
title: 中断性变更：TextFormatFlags.ModifyString 已过时
description: 了解 .NET 5 中的中断性变更：TextFormatFlags.ModifyString 字段已过时，显示警告。
ms.date: 11/05/2020
ms.openlocfilehash: 9fe1e04b1ad36070b08af2affdca1e058ec74bb8
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256161"
---
# <a name="textformatflagsmodifystring-is-obsolete"></a><span data-ttu-id="a8f8c-103">TextFormatFlags.ModifyString 已过时</span><span class="sxs-lookup"><span data-stu-id="a8f8c-103">TextFormatFlags.ModifyString is obsolete</span></span>

<span data-ttu-id="a8f8c-104"><xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> 字段已过时（警告），可能会在将来的 .NET 版本中删除。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-104">The <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> field is obsolete, as a warning, and may be removed in a future .NET version.</span></span>

## <a name="change-description"></a><span data-ttu-id="a8f8c-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="a8f8c-105">Change description</span></span>

<span data-ttu-id="a8f8c-106">在以前的 .NET 版本中，<xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> 枚举字段未被标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-106">In previous .NET versions, the <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> enumeration field is not marked obsolete.</span></span> <span data-ttu-id="a8f8c-107">在 .NET 5 及更高版本中，它被标记为已过时（警告）。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-107">In .NET 5 and later versions, it is marked obsolete as a warning.</span></span> <span data-ttu-id="a8f8c-108">此字段可能会在将来的 .NET 版本中删除。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-108">This field may be removed in a future .NET version.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="a8f8c-109">更改原因</span><span class="sxs-lookup"><span data-stu-id="a8f8c-109">Reason for change</span></span>

<span data-ttu-id="a8f8c-110">在某些情况下，将字符串传递到 <xref:System.Windows.Forms.TextRenderer.MeasureText%2A?displayProperty=nameWithType> 和 <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> 会更改字符串。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-110">Passing a string to <xref:System.Windows.Forms.TextRenderer.MeasureText%2A?displayProperty=nameWithType> with <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> alters the string in some situations.</span></span> <span data-ttu-id="a8f8c-111">此行为会中断字符串不可变性承诺，并可能导致致命的 .NET 运行时状态损坏。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-111">This behavior breaks the string immutability promise and may lead to a fatal .NET runtime state corruption.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="a8f8c-112">引入的版本</span><span class="sxs-lookup"><span data-stu-id="a8f8c-112">Version introduced</span></span>

<span data-ttu-id="a8f8c-113">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="a8f8c-113">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="a8f8c-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="a8f8c-114">Recommended action</span></span>

<span data-ttu-id="a8f8c-115">更新依赖于 <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType> 的任何代码。</span><span class="sxs-lookup"><span data-stu-id="a8f8c-115">Update any code that relies on <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=nameWithType>.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="a8f8c-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="a8f8c-116">Affected APIs</span></span>

- <xref:System.Windows.Forms.TextFormatFlags.ModifyString?displayProperty=fullName>

<!--

### Affected APIs

- `F:System.Windows.Forms.TextFormatFlags.ModifyString`

### Category

Windows Forms

-->
