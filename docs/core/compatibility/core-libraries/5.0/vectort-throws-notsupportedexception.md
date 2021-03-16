---
title: 中断性变更：向量<T> 引发 NotSupportedException
description: 了解核心 .NET 库中的 .NET 5 中断性变更：Vector<T> 始终会对不受支持的类型参数引发异常。
ms.date: 11/01/2020
ms.openlocfilehash: dccd39c01f4debd7d1432195e7f3cb14aeda5f65
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257006"
---
# <a name="vectort-always-throws-notsupportedexception-for-unsupported-types"></a><span data-ttu-id="d64c2-103">对于不支持的类型，Vector\<T> 始终引发 NotSupportedException</span><span class="sxs-lookup"><span data-stu-id="d64c2-103">Vector\<T> always throws NotSupportedException for unsupported types</span></span>

<span data-ttu-id="d64c2-104">现在，对于不支持的类型参数，<xref:System.Numerics.Vector%601?displayProperty=nameWithType> 始终引发 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="d64c2-104"><xref:System.Numerics.Vector%601?displayProperty=nameWithType> now always throws a <xref:System.NotSupportedException> for unsupported type parameters.</span></span>

## <a name="change-description"></a><span data-ttu-id="d64c2-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="d64c2-105">Change description</span></span>

<span data-ttu-id="d64c2-106">以前，如果 `T` 是[不支持的类型](#unsupported-types)，<xref:System.Numerics.Vector%601> 的成员将不会始终引发 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="d64c2-106">Previously, members of <xref:System.Numerics.Vector%601> would not always throw a <xref:System.NotSupportedException> when `T` was an [unsupported type](#unsupported-types).</span></span> <span data-ttu-id="d64c2-107">并非总是因支持硬件加速的代码路径而引发异常。</span><span class="sxs-lookup"><span data-stu-id="d64c2-107">The exception wasn't always thrown because of code paths that supported hardware acceleration.</span></span> <span data-ttu-id="d64c2-108">例如，`Vector<bool> + Vector<bool>` 返回了 `default`，而不是在没有硬件加速的平台（如 ARM32）上引发异常。</span><span class="sxs-lookup"><span data-stu-id="d64c2-108">For example, `Vector<bool> + Vector<bool>` returned `default` instead of throwing an exception on platforms that have no hardware acceleration, such as ARM32.</span></span> <span data-ttu-id="d64c2-109">对于不支持的类型，<xref:System.Numerics.Vector%601> 成员在不同的平台和硬件配置中表现出不一致的行为。</span><span class="sxs-lookup"><span data-stu-id="d64c2-109">For unsupported types, <xref:System.Numerics.Vector%601> members exhibited inconsistent behavior across different platforms and hardware configurations.</span></span>

<span data-ttu-id="d64c2-110">从 .NET 5 开始，如果 `T` 类型不受支持，那么 <xref:System.Numerics.Vector%601> 成员始终会对所有硬件配置引发 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="d64c2-110">Starting in .NET 5, <xref:System.Numerics.Vector%601> members always throw a <xref:System.NotSupportedException> on all hardware configurations when `T` is not a supported type.</span></span>

### <a name="unsupported-types"></a><span data-ttu-id="d64c2-111">不支持的类型</span><span class="sxs-lookup"><span data-stu-id="d64c2-111">Unsupported types</span></span>

<span data-ttu-id="d64c2-112"><xref:System.Numerics.Vector%601> 的类型参数支持的类型如下：</span><span class="sxs-lookup"><span data-stu-id="d64c2-112">The supported types for the type parameter of <xref:System.Numerics.Vector%601> are:</span></span>

- `byte`
- `sbyte`
- `short`
- `ushort`
- `int`
- `uint`
- `long`
- `ulong`
- `float`
- `double`

<span data-ttu-id="d64c2-113">支持的类型并无变化，但将来可能会更改。</span><span class="sxs-lookup"><span data-stu-id="d64c2-113">The supported types have not changed, however, they may change in the future.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="d64c2-114">引入的版本</span><span class="sxs-lookup"><span data-stu-id="d64c2-114">Version introduced</span></span>

<span data-ttu-id="d64c2-115">5.0</span><span class="sxs-lookup"><span data-stu-id="d64c2-115">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="d64c2-116">建议操作</span><span class="sxs-lookup"><span data-stu-id="d64c2-116">Recommended action</span></span>

<span data-ttu-id="d64c2-117">不要对 <xref:System.Numerics.Vector%601> 的类型参数使用不受支持的类型。</span><span class="sxs-lookup"><span data-stu-id="d64c2-117">Don't use an unsupported type for the type parameter of <xref:System.Numerics.Vector%601>.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="d64c2-118">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="d64c2-118">Affected APIs</span></span>

- <span data-ttu-id="d64c2-119"><xref:System.Numerics.Vector%601?displayProperty=fullName> 及其所有成员</span><span class="sxs-lookup"><span data-stu-id="d64c2-119"><xref:System.Numerics.Vector%601?displayProperty=fullName> and all its members</span></span>

<!--

#### Category

Core .NET libraries

### Affected APIs

- ``T:System.Numerics.Vector`1``

-->
