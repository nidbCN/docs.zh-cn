---
title: SYSLIB0005 警告
description: 了解有关生成编译时警告 SYSLIB0005 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 9ed36d247d31bcebc499bd7ed3945490d9d901f9
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256358"
---
# <a name="syslib0005-the-global-assembly-cache-gac-is-not-supported"></a><span data-ttu-id="4abaa-103">SYSLIB0005：不支持全局程序集缓存 (GAC)</span><span class="sxs-lookup"><span data-stu-id="4abaa-103">SYSLIB0005: The global assembly cache (GAC) is not supported</span></span>

<span data-ttu-id="4abaa-104">.NET Core 和 .NET 5 及更高版本消除了 .NET Framework 中存在的全局程序集缓存 (GAC) 这一概念。</span><span class="sxs-lookup"><span data-stu-id="4abaa-104">.NET Core and .NET 5 and later versions eliminate the concept of the global assembly cache (GAC) that was present in .NET Framework.</span></span> <span data-ttu-id="4abaa-105">为帮助开发人员摒弃这些 API，从 .NET 5.0 开始，一些 GAC 相关的 API 标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="4abaa-105">To help steer developers away from these APIs, some GAC-related APIs are marked as obsolete, starting in .NET 5.0.</span></span> <span data-ttu-id="4abaa-106">使用这些 API 会在编译时生成警告 `SYSLIB0005`。</span><span class="sxs-lookup"><span data-stu-id="4abaa-106">Using these APIs generates warning `SYSLIB0005` at compile time.</span></span>

<span data-ttu-id="4abaa-107">以下与 GAC 相关的 API 标记为已过时：</span><span class="sxs-lookup"><span data-stu-id="4abaa-107">The following GAC-related APIs are marked obsolete:</span></span>

- <xref:System.Reflection.Assembly.GlobalAssemblyCache?displayProperty=nameWithType>

  <span data-ttu-id="4abaa-108">库和应用不应使用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> API 来确定运行时行为，因为它在 .NET Core 和 .NET 5+ 中始终返回 `false`。</span><span class="sxs-lookup"><span data-stu-id="4abaa-108">Libraries and apps should not use the <xref:System.Reflection.Assembly.GlobalAssemblyCache> API to make determinations about run-time behavior, as it always returns `false` in .NET Core and .NET 5+.</span></span>

## <a name="workarounds"></a><span data-ttu-id="4abaa-109">解决方法</span><span class="sxs-lookup"><span data-stu-id="4abaa-109">Workarounds</span></span>

<span data-ttu-id="4abaa-110">如果你的应用程序查询 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 属性，请考虑删除该调用。</span><span class="sxs-lookup"><span data-stu-id="4abaa-110">If your application queries the <xref:System.Reflection.Assembly.GlobalAssemblyCache> property, consider removing the call.</span></span> <span data-ttu-id="4abaa-111">如果在运行时使用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 值在“GAC 中的程序集”流与“不在 GAC 中的程序集”流之间进行选择，请重新考虑流对于 .NET 5+ 应用程序是否仍然有意义。</span><span class="sxs-lookup"><span data-stu-id="4abaa-111">If you use the <xref:System.Reflection.Assembly.GlobalAssemblyCache> value to choose between an "assembly in the GAC"-flow vs. an "assembly not in the GAC"-flow at run time, reconsider whether the flow still makes sense for a .NET 5+ application.</span></span>

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]

## <a name="see-also"></a><span data-ttu-id="4abaa-112">请参阅</span><span class="sxs-lookup"><span data-stu-id="4abaa-112">See also</span></span>

- [<span data-ttu-id="4abaa-113">全局程序集缓存</span><span class="sxs-lookup"><span data-stu-id="4abaa-113">Global assembly cache</span></span>](../../../framework/app-domains/gac.md)
