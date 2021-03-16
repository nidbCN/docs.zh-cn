---
title: 中断性变更：NegotiateStream 和 SslStream 允许连续的“开始”操作
description: 了解 .NET 5 中的中断性变更：关于安全流的错误案例会得到不同的处理，对 BeginAuthenticateAsServer 或 BeginAuthenticateAsClient 的后续调用可能不会再失败。
ms.date: 10/18/2020
ms.openlocfilehash: 5c042be01873849cc154111a31fc007521508c7b
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256434"
---
# <a name="negotiatestream-and-sslstream-allow-successive-begin-operations"></a><span data-ttu-id="9d4ed-103">NegotiateStream 和 SslStream 允许连续的“开始”操作</span><span class="sxs-lookup"><span data-stu-id="9d4ed-103">NegotiateStream and SslStream allow successive Begin operations</span></span>

<span data-ttu-id="9d4ed-104">关于安全流的错误案例会得到不同的处理，对 `BeginAuthenticateAsServer` 或 `BeginAuthenticateAsClient` 的后续调用可能不会再失败。</span><span class="sxs-lookup"><span data-stu-id="9d4ed-104">Error cases on security streams are handled differently, and successive calls to `BeginAuthenticateAsServer` or `BeginAuthenticateAsClient` may no longer fail.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="9d4ed-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="9d4ed-105">Version introduced</span></span>

<span data-ttu-id="9d4ed-106">5.0</span><span class="sxs-lookup"><span data-stu-id="9d4ed-106">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="9d4ed-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="9d4ed-107">Change description</span></span>

<span data-ttu-id="9d4ed-108">在以前的 .NET 版本中，若在未先调用 `EndAuthenticateAsServer` 或 `EndAuthenticateAsClient` 的情况下就连续调用 `BeginAuthenticateAsServer` 或 `BeginAuthenticateAsClient`，这会导致 <xref:System.NotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="9d4ed-108">In previous .NET versions, calling `BeginAuthenticateAsServer` or `BeginAuthenticateAsClient` successively without first calling `EndAuthenticateAsServer` or `EndAuthenticateAsClient` results in a <xref:System.NotSupportedException>.</span></span> <span data-ttu-id="9d4ed-109">从 .NET 5 开始，对 `BeginAuthenticateAsServer` 或 `BeginAuthenticateAsClient` 的连续调用不会再导致 <xref:System.NotSupportedException>，因为这些 API 由基于 <xref:System.Threading.Tasks.Task> 的实现支持。</span><span class="sxs-lookup"><span data-stu-id="9d4ed-109">Starting in .NET 5, successive calls to `BeginAuthenticateAsServer` or `BeginAuthenticateAsClient` no longer result in a <xref:System.NotSupportedException>, because these APIs are backed by a <xref:System.Threading.Tasks.Task>-based implementation.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="9d4ed-110">更改原因</span><span class="sxs-lookup"><span data-stu-id="9d4ed-110">Reason for change</span></span>

<span data-ttu-id="9d4ed-111">将内部实现从基于异步编程模型 (APM) 切换到基于 <xref:System.Threading.Tasks.Task> 会提高性能并降低代码复杂性。</span><span class="sxs-lookup"><span data-stu-id="9d4ed-111">Switching the internal implementation from asynchronous programming model (APM) to <xref:System.Threading.Tasks.Task>-based improves performance and decreases code complexity.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="9d4ed-112">建议操作</span><span class="sxs-lookup"><span data-stu-id="9d4ed-112">Recommended action</span></span>

<span data-ttu-id="9d4ed-113">开发人员一方不需要执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="9d4ed-113">No action is required on the part of the developer.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="9d4ed-114">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="9d4ed-114">Affected APIs</span></span>

- <xref:System.Net.Security.SslStream.BeginAuthenticateAsServer%2A?displayProperty=fullName>
- <xref:System.Net.Security.SslStream.BeginAuthenticateAsClient%2A?displayProperty=fullName>
- <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsServer%2A?displayProperty=fullName>
- <xref:System.Net.Security.NegotiateStream.BeginAuthenticateAsClient%2A?displayProperty=fullName>

<!--

### Affected APIs

- `Overload:M:System.Net.Security.SslStream.BeginAuthenticateAsServer`
- `Overload:M:System.Net.Security.SslStream.BeginAuthenticateAsClient`
- `Overload:M:System.Net.Security.NegotiateStream.BeginAuthenticateAsServer`
- `Overload:M:System.Net.Security.NegotiateStream.BeginAuthenticateAsClient`

### Category

Networking

-->
