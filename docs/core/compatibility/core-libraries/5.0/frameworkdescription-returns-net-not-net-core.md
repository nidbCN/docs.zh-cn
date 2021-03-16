---
title: 中断性变更：FrameworkDescription 的值是 .NET 而不是 .NET Core
description: 了解核心 .NET 库中的 .NET 5 中断性变更：RuntimeInformation.FrameworkDescription 现返回“.NET”而不是“.NET Core”。
ms.date: 11/01/2020
ms.openlocfilehash: 18aa9a30a149b3c38d4bbfe4a0c99446f4372f07
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257513"
---
# <a name="frameworkdescriptions-value-is-net-instead-of-net-core"></a><span data-ttu-id="1db08-103">FrameworkDescription 的值是 .NET 而不是 .NET Core</span><span class="sxs-lookup"><span data-stu-id="1db08-103">FrameworkDescription's value is .NET instead of .NET Core</span></span>

<span data-ttu-id="1db08-104"><xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 现在返回“.NET”而不是“.NET Core”。</span><span class="sxs-lookup"><span data-stu-id="1db08-104"><xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> now returns ".NET" instead of ".NET Core".</span></span>

## <a name="change-description"></a><span data-ttu-id="1db08-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="1db08-105">Change description</span></span>

<span data-ttu-id="1db08-106">在以前的 .NET 版本中，<xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 返回“.NET Core”作为描述字符串的一部分，例如 `.NET Core 3.1.1`。</span><span class="sxs-lookup"><span data-stu-id="1db08-106">In previous .NET versions, <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> returns ".NET Core" as part of the description string, for example, `.NET Core 3.1.1`.</span></span>

<span data-ttu-id="1db08-107">从 .NET 5 开始，<xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 返回“.NET”作为描述字符串的一部分，例如 `.NET 5.0.0`。</span><span class="sxs-lookup"><span data-stu-id="1db08-107">Starting in .NET 5, <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> returns ".NET" as part of the description string, for example, `.NET 5.0.0`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="1db08-108">更改原因</span><span class="sxs-lookup"><span data-stu-id="1db08-108">Reason for change</span></span>

<span data-ttu-id="1db08-109">对于 .NET 5，`netcoreapp` 替换为 `net`，作为简短的目标框架名字对象。</span><span class="sxs-lookup"><span data-stu-id="1db08-109">With .NET 5, `netcoreapp` is replaced by `net` as the short target-framework moniker.</span></span> <span data-ttu-id="1db08-110">为了保持一致性，框架描述也进行了更新。</span><span class="sxs-lookup"><span data-stu-id="1db08-110">For consistency, the framework's description has also been updated.</span></span> <span data-ttu-id="1db08-111">该更改只浮于表面，因为除了在 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 属性中，`FrameworkName` 未在其他任何地方进行编码。</span><span class="sxs-lookup"><span data-stu-id="1db08-111">The change is cosmetic, as the `FrameworkName` isn't encoded anywhere else than in the <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> property.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="1db08-112">引入的版本</span><span class="sxs-lookup"><span data-stu-id="1db08-112">Version introduced</span></span>

<span data-ttu-id="1db08-113">5.0</span><span class="sxs-lookup"><span data-stu-id="1db08-113">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="1db08-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="1db08-114">Recommended action</span></span>

<span data-ttu-id="1db08-115">更新搜索 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription> 返回的字符串中“.NET Core”的所有代码。</span><span class="sxs-lookup"><span data-stu-id="1db08-115">Update any code that searches for ".NET Core" in the string returned by <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription>.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="1db08-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="1db08-116">Affected APIs</span></span>

- <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription`

-->
