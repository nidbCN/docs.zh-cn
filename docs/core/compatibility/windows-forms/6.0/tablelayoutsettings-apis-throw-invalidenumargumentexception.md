---
title: 中断性变更：某些 TableLayoutSettings 属性会引发 InvalidEnumArgumentException
description: 了解 .NET 6 中的中断性变更：某些 TableLayoutSettings API 现在将针对无效参数引发 InvalidEnumArgumentException。
ms.date: 01/18/2021
ms.openlocfilehash: 2da097122b935ec3a60c2bb009cc8ebbcff6468e
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255719"
---
# <a name="selected-tablelayoutsettings-properties-throw-invalidenumargumentexception"></a><span data-ttu-id="f810f-103">所选的 TableLayoutSettings 属性会引发 InvalidEnumArgumentException</span><span class="sxs-lookup"><span data-stu-id="f810f-103">Selected TableLayoutSettings properties throw InvalidEnumArgumentException</span></span>

<span data-ttu-id="f810f-104">如果尝试分配不正确的值，则所选的 <xref:System.Windows.Forms.TableLayoutSettings> 属性现在会引发 <xref:System.ComponentModel.InvalidEnumArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="f810f-104">Selected <xref:System.Windows.Forms.TableLayoutSettings> properties now throw an <xref:System.ComponentModel.InvalidEnumArgumentException> if you attempt to assign an incorrect value.</span></span>

## <a name="change-description"></a><span data-ttu-id="f810f-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="f810f-105">Change description</span></span>

<span data-ttu-id="f810f-106">在以前的 .NET 版本中，如果尝试分配不正确的值，这些属性会引发 <xref:System.ArgumentOutOfRangeException>。</span><span class="sxs-lookup"><span data-stu-id="f810f-106">In previous .NET versions, these properties throw an <xref:System.ArgumentOutOfRangeException> if you attempt to assign an incorrect value.</span></span> <span data-ttu-id="f810f-107">从 .NET 6 开始，这些属性在此情况下会引发 <xref:System.ComponentModel.InvalidEnumArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="f810f-107">Starting in .NET 6, these properties throw an <xref:System.ComponentModel.InvalidEnumArgumentException> in such cases.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="f810f-108">更改原因</span><span class="sxs-lookup"><span data-stu-id="f810f-108">Reason for change</span></span>

<span data-ttu-id="f810f-109">在类似情况下，与现有的 Windows Forms API 一致，会引发 <xref:System.ComponentModel.InvalidEnumArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="f810f-109">Throwing <xref:System.ComponentModel.InvalidEnumArgumentException> is in line with the existing Windows Forms API in similar situations.</span></span> <span data-ttu-id="f810f-110">引发此异常还会为开发人员提供更好的调试体验。</span><span class="sxs-lookup"><span data-stu-id="f810f-110">Throwing this exception also provides developers with a better debug experience.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="f810f-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="f810f-111">Version introduced</span></span>

<span data-ttu-id="f810f-112">.NET 6.0</span><span class="sxs-lookup"><span data-stu-id="f810f-112">.NET 6.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="f810f-113">建议的操作</span><span class="sxs-lookup"><span data-stu-id="f810f-113">Recommended action</span></span>

- <span data-ttu-id="f810f-114">更新代码以防止分配不正确的值。</span><span class="sxs-lookup"><span data-stu-id="f810f-114">Update the code to prevent assigning incorrect values.</span></span>
- <span data-ttu-id="f810f-115">如有必要，请在访问这些 API 时处理 <xref:System.ComponentModel.InvalidEnumArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="f810f-115">If necessary, handle an <xref:System.ComponentModel.InvalidEnumArgumentException> when accessing these APIs.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="f810f-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="f810f-116">Affected APIs</span></span>

<span data-ttu-id="f810f-117">下表列出了受影响的属性：</span><span class="sxs-lookup"><span data-stu-id="f810f-117">The following table lists the affected properties:</span></span>

| <span data-ttu-id="f810f-118">properties</span><span class="sxs-lookup"><span data-stu-id="f810f-118">Property</span></span> | <span data-ttu-id="f810f-119">版本已更改</span><span class="sxs-lookup"><span data-stu-id="f810f-119">Version changed</span></span> |
|-|-|-|-|
| <xref:System.Windows.Forms.TableLayoutPanel.CellBorderStyle?displayProperty=fullName> | <span data-ttu-id="f810f-120">预览版 1</span><span class="sxs-lookup"><span data-stu-id="f810f-120">Preview 1</span></span> |
| <xref:System.Windows.Forms.TableLayoutPanel.GrowStyle?displayProperty=fullName> | <span data-ttu-id="f810f-121">预览版 1</span><span class="sxs-lookup"><span data-stu-id="f810f-121">Preview 1</span></span> |

<!--

### Affected APIs

- `P:System.Windows.Forms.TableLayoutPanel.CellBorderStyle`
- `P:System.Windows.Forms.TableLayoutPanel.GrowStyle`

### Category

Windows Forms

-->
