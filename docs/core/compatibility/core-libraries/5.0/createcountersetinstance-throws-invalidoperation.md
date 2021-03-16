---
title: 中断性变更：如果实例已存在，CreateCounterSetInstance 将引发 InvalidOperationException
description: 了解核心 .NET 库中的 .NET 5 中断性变更：如果计数器已存在，CounterSet.CreateCounterSetInstance 将引发不同的异常。
ms.date: 11/01/2020
ms.openlocfilehash: bf59677a85538bc4992c589f8642ab4a0fce54ac
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257565"
---
# <a name="countersetcreatecountersetinstance-now-throws-invalidoperationexception-if-instance-already-exists"></a><span data-ttu-id="6848b-103">如果实例已存在，CounterSet.CreateCounterSetInstance 现在将引发 InvalidOperationException</span><span class="sxs-lookup"><span data-stu-id="6848b-103">CounterSet.CreateCounterSetInstance now throws InvalidOperationException if instance already exists</span></span>

<span data-ttu-id="6848b-104">从 .NET 5 开始，如果计数器集已存在，<xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance(System.String)?displayProperty=nameWithType> 将引发 <xref:System.InvalidOperationException> 而不是 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="6848b-104">Starting in .NET 5, <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance(System.String)?displayProperty=nameWithType> throws an <xref:System.InvalidOperationException> instead of an <xref:System.ArgumentException> if the counter set already exists.</span></span>

## <a name="change-description"></a><span data-ttu-id="6848b-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="6848b-105">Change description</span></span>

<span data-ttu-id="6848b-106">在 .NET Framework 和 .NET Core 1.0 到 3.1 中，可以通过调用 <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> 创建计数器集的实例。</span><span class="sxs-lookup"><span data-stu-id="6848b-106">In .NET Framework and .NET Core 1.0 to 3.1, you can create an instance of the counter set by calling <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A>.</span></span> <span data-ttu-id="6848b-107">但是，如果计数器集已存在，则该方法将引发 <xref:System.ArgumentException> 异常。</span><span class="sxs-lookup"><span data-stu-id="6848b-107">However, if the counter set already exists, the method throws an <xref:System.ArgumentException> exception.</span></span>

<span data-ttu-id="6848b-108">在 .NET 5 及更高版本中，当调用 <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> 且计数器集存在时，将引发 <xref:System.InvalidOperationException> 异常。</span><span class="sxs-lookup"><span data-stu-id="6848b-108">In .NET 5 and later versions, when you call <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> and the counter set exists, an <xref:System.InvalidOperationException> exception is thrown.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="6848b-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="6848b-109">Version introduced</span></span>

<span data-ttu-id="6848b-110">5.0</span><span class="sxs-lookup"><span data-stu-id="6848b-110">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="6848b-111">建议操作</span><span class="sxs-lookup"><span data-stu-id="6848b-111">Recommended action</span></span>

<span data-ttu-id="6848b-112">如果在调用 <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A> 时捕获应用中的 <xref:System.ArgumentException> 异常，还应考虑捕获 <xref:System.InvalidOperationException> 异常。</span><span class="sxs-lookup"><span data-stu-id="6848b-112">If you catch <xref:System.ArgumentException> exceptions in your app when calling <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A>, consider also catching <xref:System.InvalidOperationException> exceptions.</span></span>

> [!NOTE]
> <span data-ttu-id="6848b-113">不建议捕获 <xref:System.ArgumentException> 异常。</span><span class="sxs-lookup"><span data-stu-id="6848b-113">Catching <xref:System.ArgumentException> exceptions is not recommended.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="6848b-114">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="6848b-114">Affected APIs</span></span>

- <xref:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance%2A?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `M:System.Diagnostics.PerformanceData.CounterSet.CreateCounterSetInstance(System.String)`

-->
