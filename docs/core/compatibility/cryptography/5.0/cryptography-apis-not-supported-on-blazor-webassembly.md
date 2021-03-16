---
title: 中断性变更：Blazor WebAssembly 不支持的 System.Security.Cryptography API
description: 了解 .NET 5 中的中断性变更：加密 API 在浏览器上运行时会引发异常。
ms.date: 09/16/2020
ms.openlocfilehash: ecbdda4c04642af6b1737e888491eb6565ba7479
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256876"
---
# <a name="systemsecuritycryptography-apis-not-supported-on-blazor-webassembly"></a><span data-ttu-id="bd54c-103">Blazor WebAssembly 不支持的 System.Security.Cryptography API</span><span class="sxs-lookup"><span data-stu-id="bd54c-103">System.Security.Cryptography APIs not supported on Blazor WebAssembly</span></span>

<span data-ttu-id="bd54c-104">在浏览器上运行时，<xref:System.Security.Cryptography> API 在运行时引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="bd54c-104"><xref:System.Security.Cryptography> APIs throw a <xref:System.PlatformNotSupportedException> at run time when run on a browser.</span></span>

## <a name="change-description"></a><span data-ttu-id="bd54c-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="bd54c-105">Change description</span></span>

<span data-ttu-id="bd54c-106">在以前的 .NET 版本中，大多数 <xref:System.Security.Cryptography> API 不可用于 Blazor WebAssembly 应用。</span><span class="sxs-lookup"><span data-stu-id="bd54c-106">In previous .NET versions, most of the <xref:System.Security.Cryptography> APIs aren't available to Blazor WebAssembly apps.</span></span> <span data-ttu-id="bd54c-107">从 .NET 5 开始，Blazor WebAssembly 应用面向整个 .NET 5 API 外围应用，但由于浏览器沙盒约束，并非所有 .NET 5 API 都受支持。</span><span class="sxs-lookup"><span data-stu-id="bd54c-107">Starting in .NET 5, Blazor WebAssembly apps target the full .NET 5 API surface area, however, not all .NET 5 APIs are supported due to browser sandbox constraints.</span></span> <span data-ttu-id="bd54c-108">在 .NET 5 及更高版本中，不支持的 <xref:System.Security.Cryptography> API 在 WebAssembly 上运行时引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="bd54c-108">In .NET 5 and later versions, the unsupported <xref:System.Security.Cryptography> APIs throw a <xref:System.PlatformNotSupportedException> when running on WebAssembly.</span></span>

> [!TIP]
> <span data-ttu-id="bd54c-109">生成支持浏览器平台的项目时，[平台兼容性分析器](../../code-analysis/5.0/ca1416-platform-compatibility-analyzer.md)将标记对受影响的 API 的任何调用。</span><span class="sxs-lookup"><span data-stu-id="bd54c-109">The [Platform compatibility analyzer](../../code-analysis/5.0/ca1416-platform-compatibility-analyzer.md) will flag any calls to the affected APIs when you build a project that supports the browser platform.</span></span> <span data-ttu-id="bd54c-110">默认情况下，此分析器在 .NET 5 及更高版本的应用中运行。</span><span class="sxs-lookup"><span data-stu-id="bd54c-110">This analyzer runs by default in .NET 5 and later apps.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="bd54c-111">更改原因</span><span class="sxs-lookup"><span data-stu-id="bd54c-111">Reason for change</span></span>

<span data-ttu-id="bd54c-112">Microsoft 无法将 OpenSSL 作为依赖项提供在 Blazor WebAssembly 配置中。</span><span class="sxs-lookup"><span data-stu-id="bd54c-112">Microsoft is unable to ship OpenSSL as a dependency in the Blazor WebAssembly configuration.</span></span> <span data-ttu-id="bd54c-113">通过尝试与浏览器的 `SubtleCrypto` API 集成来尝试解决此问题。</span><span class="sxs-lookup"><span data-stu-id="bd54c-113">We attempted to work around this by trying to integrate with the browser's `SubtleCrypto` API.</span></span> <span data-ttu-id="bd54c-114">遗憾的是，这需要大量的 API 更改，因此难以集成。</span><span class="sxs-lookup"><span data-stu-id="bd54c-114">Unfortunately, it required significant API changes that made it too hard to integrate.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="bd54c-115">引入的版本</span><span class="sxs-lookup"><span data-stu-id="bd54c-115">Version introduced</span></span>

<span data-ttu-id="bd54c-116">5.0</span><span class="sxs-lookup"><span data-stu-id="bd54c-116">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="bd54c-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="bd54c-117">Recommended action</span></span>

<span data-ttu-id="bd54c-118">目前没有建议任何好的解决方法。</span><span class="sxs-lookup"><span data-stu-id="bd54c-118">There are no good workarounds to suggest at this time.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="bd54c-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="bd54c-119">Affected APIs</span></span>

<span data-ttu-id="bd54c-120">除以下 API 之外的所有 <xref:System.Security.Cryptography?displayProperty=fullName> API：</span><span class="sxs-lookup"><span data-stu-id="bd54c-120">All <xref:System.Security.Cryptography?displayProperty=fullName> APIs except the following:</span></span>

- `System.Security.Cryptography.RandomNumberGenerator`
- `System.Security.Cryptography.IncrementalHash`
- `System.Security.Cryptography.SHA1`
- `System.Security.Cryptography.SHA256`
- `System.Security.Cryptography.SHA384`
- `System.Security.Cryptography.SHA512`
- `System.Security.Cryptography.SHA1Managed`
- `System.Security.Cryptography.SHA256Managed`
- `System.Security.Cryptography.SHA384Managed`
- `System.Security.Cryptography.SHA512Managed`

<!--

### Affected APIs

- `T:System.Security.Cryptography`

### Category

- ASP.NET Core
- Cryptography

-->
