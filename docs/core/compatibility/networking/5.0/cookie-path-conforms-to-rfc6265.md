---
title: 中断性变更：Cookie 路径处理现在符合 RFC 6265
description: 了解 .NET 5 中的中断性变更：RFC 6265 中定义的路径处理算法之前针对 Cookie 和 CookieContainer 类而实现。
ms.date: 08/18/2020
ms.openlocfilehash: 5ee1bccc79a5ac271904dd3223b58cc168f18cfa
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256460"
---
# <a name="cookie-path-handling-now-conforms-to-rfc-6265"></a><span data-ttu-id="4765c-103">Cookie 路径处理现在符合 RFC 6265</span><span class="sxs-lookup"><span data-stu-id="4765c-103">Cookie path handling now conforms to RFC 6265</span></span>

<span data-ttu-id="4765c-104">[RFC 6265](https://tools.ietf.org/html/rfc6265) 中定义的路径处理算法是针对 <xref:System.Net.Cookie> 和 <xref:System.Net.CookieContainer> 类实现的。</span><span class="sxs-lookup"><span data-stu-id="4765c-104">Path-handling algorithms defined in [RFC 6265](https://tools.ietf.org/html/rfc6265) were implemented for the <xref:System.Net.Cookie> and <xref:System.Net.CookieContainer> classes.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="4765c-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="4765c-105">Version introduced</span></span>

<span data-ttu-id="4765c-106">5.0</span><span class="sxs-lookup"><span data-stu-id="4765c-106">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="4765c-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="4765c-107">Change description</span></span>

- <span data-ttu-id="4765c-108">无需再将 <xref:System.Net.Cookie.Path> 属性作为请求路径的前缀。</span><span class="sxs-lookup"><span data-stu-id="4765c-108">The <xref:System.Net.Cookie.Path> property is no longer required to be a prefix of the request path.</span></span>
- <span data-ttu-id="4765c-109"><xref:System.Net.Cookie.Path> 的默认值的计算和路径匹配算法是按照 RFC 6265 的[第 5.1.4 节](https://tools.ietf.org/html/rfc6265#section-5.1.4)中的定义实现的。</span><span class="sxs-lookup"><span data-stu-id="4765c-109">The calculation of the default value of <xref:System.Net.Cookie.Path> and path-matching algorithms were implemented as defined in [section 5.1.4](https://tools.ietf.org/html/rfc6265#section-5.1.4) of RFC 6265.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="4765c-110">建议操作</span><span class="sxs-lookup"><span data-stu-id="4765c-110">Recommended action</span></span>

<span data-ttu-id="4765c-111">在大多数情况下，无需执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="4765c-111">In most cases, you won't need to take any action.</span></span> <span data-ttu-id="4765c-112">但是，如果代码依赖于 <xref:System.Net.Cookie.Path> 验证，则需要在代码中实现所需的验证。</span><span class="sxs-lookup"><span data-stu-id="4765c-112">However, if your code was dependent on <xref:System.Net.Cookie.Path> validation, you'll need to implement required validation in your code.</span></span> <span data-ttu-id="4765c-113">如果代码依赖于 <xref:System.Net.Cookie.Path> 的默认值计算，请考虑手动提供 <xref:System.Net.Cookie.Path> 值，而不使用默认值。</span><span class="sxs-lookup"><span data-stu-id="4765c-113">If your code was dependent on default value calculation for <xref:System.Net.Cookie.Path>, consider supplying the <xref:System.Net.Cookie.Path> value manually instead of using the default value.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="4765c-114">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="4765c-114">Affected APIs</span></span>

- <xref:System.Net.Cookie?displayProperty=fullName>
- <xref:System.Net.CookieContainer?displayProperty=fullName>

<!--

### Affected APIs

- `T:System.Net.Cookie`
- `T:System.Net.CookieContainer`

### Category

Networking

-->
