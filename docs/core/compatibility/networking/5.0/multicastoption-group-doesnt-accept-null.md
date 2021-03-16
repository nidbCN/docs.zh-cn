---
title: 中断性变更：MulticastOption.Group 不接受 null 值
description: 了解 .NET 5 中的中断性变更：MulticastOption.Group 不再接受 null 值。
ms.date: 08/18/2020
ms.openlocfilehash: 09c6415d275361abee8285aabdda13ccd9727043
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256447"
---
# <a name="multicastoptiongroup-doesnt-accept-a-null-value"></a><span data-ttu-id="43b5e-103">MulticastOption.Group 不接受 null 值</span><span class="sxs-lookup"><span data-stu-id="43b5e-103">MulticastOption.Group doesn't accept a null value</span></span>

<span data-ttu-id="43b5e-104"><xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> 不再接受 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="43b5e-104"><xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> no longer accepts a value of `null`.</span></span> <span data-ttu-id="43b5e-105">如果将属性设置为 `null`，则会引发 <xref:System.ArgumentNullException>。</span><span class="sxs-lookup"><span data-stu-id="43b5e-105">If you set the property to `null`, an <xref:System.ArgumentNullException> is thrown.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="43b5e-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="43b5e-106">Version introduced</span></span>

<span data-ttu-id="43b5e-107">5.0</span><span class="sxs-lookup"><span data-stu-id="43b5e-107">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="43b5e-108">更改描述</span><span class="sxs-lookup"><span data-stu-id="43b5e-108">Change description</span></span>

<span data-ttu-id="43b5e-109">在早期版本的 .NET 中，可以将 <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> 属性设置为 `null`。</span><span class="sxs-lookup"><span data-stu-id="43b5e-109">In previous versions of .NET, you can set the <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> property to `null`.</span></span> <span data-ttu-id="43b5e-110">如果 <xref:System.Net.Sockets.MulticastOption> 稍后传递给 <xref:System.Net.Sockets.Socket.SetSocketOption%2A?displayProperty=nameWithType>，则运行时将引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="43b5e-110">If the <xref:System.Net.Sockets.MulticastOption> is later passed to <xref:System.Net.Sockets.Socket.SetSocketOption%2A?displayProperty=nameWithType>, the runtime throws a <xref:System.NullReferenceException>.</span></span>

<span data-ttu-id="43b5e-111">在 .NET 5 及更高版本中，如果将属性设置为 `null`，则会引发 <xref:System.ArgumentNullException>。</span><span class="sxs-lookup"><span data-stu-id="43b5e-111">In .NET 5 and later, an <xref:System.ArgumentNullException> is thrown if you set the property to `null`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="43b5e-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="43b5e-112">Reason for change</span></span>

<span data-ttu-id="43b5e-113">为了与框架设计准则保持一致，属性已更新为在值为 `null` 时引发 <xref:System.ArgumentNullException>。</span><span class="sxs-lookup"><span data-stu-id="43b5e-113">To be consistent with the Framework Design Guidelines, the property has been updated to throw an <xref:System.ArgumentNullException> if the value is `null`.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="43b5e-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="43b5e-114">Recommended action</span></span>

<span data-ttu-id="43b5e-115">请确保未将 <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> 设置为 `null`。</span><span class="sxs-lookup"><span data-stu-id="43b5e-115">Make sure that you don't set <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=nameWithType> to `null`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="43b5e-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="43b5e-116">Affected APIs</span></span>

- <xref:System.Net.Sockets.MulticastOption.Group?displayProperty=fullName>

<!--

### Affected APIs

- `P:System.Net.Sockets.MulticastOption.Group`

### Category

Networking

-->
