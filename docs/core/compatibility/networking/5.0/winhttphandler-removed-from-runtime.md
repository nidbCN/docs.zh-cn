---
title: 中断性变更：已从 .NET 运行时中删除 WinHttpHandler
description: 了解 .NET 5 中的中断性变更：WinHttpHandler 已从 .NET 运行时中删除。
ms.date: 10/18/2020
ms.openlocfilehash: 95abcfb4d7670be035bd640dbb85458406c1b0e0
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256408"
---
# <a name="winhttphandler-removed-from-net-runtime"></a><span data-ttu-id="c59d5-103">已从 .NET 运行时中删除 WinHttpHandler</span><span class="sxs-lookup"><span data-stu-id="c59d5-103">WinHttpHandler removed from .NET runtime</span></span>

<span data-ttu-id="c59d5-104">`WinHttpHandler` 类已从 System.Net.Http.dll 程序集删除。</span><span class="sxs-lookup"><span data-stu-id="c59d5-104">The `WinHttpHandler` class was removed from the *System.Net.Http.dll* assembly.</span></span> <span data-ttu-id="c59d5-105">它现在仅作为带外 (OOB) [NuGet 包](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)提供。</span><span class="sxs-lookup"><span data-stu-id="c59d5-105">It's now available only as an out-of-band (OOB) [NuGet package](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="c59d5-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c59d5-106">Version introduced</span></span>

<span data-ttu-id="c59d5-107">5.0</span><span class="sxs-lookup"><span data-stu-id="c59d5-107">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="c59d5-108">更改描述</span><span class="sxs-lookup"><span data-stu-id="c59d5-108">Change description</span></span>

<span data-ttu-id="c59d5-109">在以前的 .NET 版本中，<xref:System.Net.Http.WinHttpHandler> 类作为核心 .NET 库的一部分提供。</span><span class="sxs-lookup"><span data-stu-id="c59d5-109">In previous .NET versions, the <xref:System.Net.Http.WinHttpHandler> class is available as part of the core .NET libraries.</span></span> <span data-ttu-id="c59d5-110">从 .NET 5 开始，<xref:System.Net.Http.WinHttpHandler> 类仅作为单独安装的 [NuGet 包](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)提供。</span><span class="sxs-lookup"><span data-stu-id="c59d5-110">Starting in .NET 5, the <xref:System.Net.Http.WinHttpHandler> class is only available as a separately installed [NuGet package](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/).</span></span>

## <a name="recommended-action"></a><span data-ttu-id="c59d5-111">建议操作</span><span class="sxs-lookup"><span data-stu-id="c59d5-111">Recommended action</span></span>

<span data-ttu-id="c59d5-112">安装 [System.Net.Http.WinHttpHandler NuGet 包](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/)。</span><span class="sxs-lookup"><span data-stu-id="c59d5-112">Install the [System.Net.Http.WinHttpHandler NuGet package](https://www.nuget.org/packages/System.Net.Http.WinHttpHandler/).</span></span> <span data-ttu-id="c59d5-113">或者，如果不需要特定于 WinHTTP 的功能，请改用 <xref:System.Net.Http.SocketsHttpHandler>。</span><span class="sxs-lookup"><span data-stu-id="c59d5-113">Or, if you don't require WinHTTP-specific features, use <xref:System.Net.Http.SocketsHttpHandler> instead.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="c59d5-114">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="c59d5-114">Affected APIs</span></span>

- <xref:System.Net.Http.WinHttpHandler?displayProperty=fullName>

<!--

### Affected APIs

- `T:System.Net.Http.WinHttpHandler`

### Category

Networking

-->
