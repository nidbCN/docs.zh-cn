---
title: 中断性变更：Activity.Tags 中的标记顺序是相反的
description: 了解核心 .NET 库中的 .NET 5 中断性变更：Activity.Tags 现在根据项目的添加顺序将项目存储在列表中。
ms.date: 11/01/2020
ms.openlocfilehash: f37f44cbe2ea467f0962cd8971d5bf38ce958dfd
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257123"
---
# <a name="order-of-tags-in-activitytags-is-reversed"></a><span data-ttu-id="8e12f-103">Activity.Tags 中的标记顺序是相反的</span><span class="sxs-lookup"><span data-stu-id="8e12f-103">Order of tags in Activity.Tags is reversed</span></span>

<span data-ttu-id="8e12f-104"><xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> 现根据项目的添加顺序将其存储在列表中，也就是说，添加的第一个项目排在列表第一位。</span><span class="sxs-lookup"><span data-stu-id="8e12f-104"><xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> now stores items in the list according to the order they are added, that is, the first item that's added is first in the list.</span></span> <span data-ttu-id="8e12f-105">此更改是为了与 [OpenTelemetry 属性规范](https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/common/common.md#attributes)一致。</span><span class="sxs-lookup"><span data-stu-id="8e12f-105">This change was made to match the [OpenTelemetry Attributes specification](https://github.com/open-telemetry/opentelemetry-specification/blob/master/specification/common/common.md#attributes).</span></span>

## <a name="change-description"></a><span data-ttu-id="8e12f-106">更改描述</span><span class="sxs-lookup"><span data-stu-id="8e12f-106">Change description</span></span>

<span data-ttu-id="8e12f-107">在以前的 .NET 版本中，<xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> 存储项目的顺序与项目的添加顺序相反。</span><span class="sxs-lookup"><span data-stu-id="8e12f-107">In previous .NET versions, <xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> stores items in the reverse order from which they're added.</span></span> <span data-ttu-id="8e12f-108">也就是说，添加的第一个项目排在列表中的最后一位。</span><span class="sxs-lookup"><span data-stu-id="8e12f-108">That is, the first item added is last in the list.</span></span> <span data-ttu-id="8e12f-109">从 .NET 5 开始，项目的顺序是相反的，添加的第一项始终排在列表第一位。</span><span class="sxs-lookup"><span data-stu-id="8e12f-109">Starting in .NET 5, the order of the items is reversed, and the first item added is always first in the list.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="8e12f-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="8e12f-110">Version introduced</span></span>

<span data-ttu-id="8e12f-111">5.0</span><span class="sxs-lookup"><span data-stu-id="8e12f-111">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="8e12f-112">建议操作</span><span class="sxs-lookup"><span data-stu-id="8e12f-112">Recommended action</span></span>

<span data-ttu-id="8e12f-113">如果应用依赖于 <xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> 列表顺序，并且你要升级到 .NET 5 或更高版本，则需要更改代码的此部分。</span><span class="sxs-lookup"><span data-stu-id="8e12f-113">If your app has a dependency on the <xref:System.Diagnostics.Activity.Tags?displayProperty=nameWithType> list order and you're upgrading to .NET 5 or later, you'll need to change this part of your code.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="8e12f-114">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="8e12f-114">Affected APIs</span></span>

- <xref:System.Diagnostics.Activity.Tags?displayProperty=fullName>

<!--

#### Category

Core .NET libraries

### Affected APIs

- `P:System.Diagnostics.Activity.Tags`

-->
