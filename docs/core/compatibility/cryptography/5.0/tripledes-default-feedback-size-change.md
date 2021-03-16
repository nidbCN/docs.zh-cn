---
title: 中断性变更：TripleDES.Create 所创建实例的默认 FeedbackSize 值已更改
description: 了解 .NET 5 中的中断性变更：从 TripleDES.Create() 返回的 TripleDES 实例的 FeedbackSize 属性默认值已从 64 更改为 8。
ms.date: 10/16/2020
ms.openlocfilehash: 9d3259da30cce84e83a3f13c610dad5884b445b8
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256759"
---
# <a name="default-feedbacksize-value-for-instances-created-by-tripledescreate-changed"></a><span data-ttu-id="581ed-103">TripleDES.Create 所创建实例的默认 FeedbackSize 值已更改</span><span class="sxs-lookup"><span data-stu-id="581ed-103">Default FeedbackSize value for instances created by TripleDES.Create changed</span></span>

<span data-ttu-id="581ed-104">从 <xref:System.Security.Cryptography.TripleDES.Create?displayProperty=nameWithType> 返回的 <xref:System.Security.Cryptography.TripleDES> 实例的 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize?displayProperty=nameWithType> 属性默认值从 64 改为 8，以便更轻松地从 .NET Framework 迁移。</span><span class="sxs-lookup"><span data-stu-id="581ed-104">The default value for the <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize?displayProperty=nameWithType> property on the <xref:System.Security.Cryptography.TripleDES> instance returned from <xref:System.Security.Cryptography.TripleDES.Create?displayProperty=nameWithType> has changed from 64 to 8 to make migration from .NET Framework easier.</span></span> <span data-ttu-id="581ed-105">仅当 <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode> 属性为 <xref:System.Security.Cryptography.CipherMode.CFB?displayProperty=nameWithType> 时才使用此属性（除非直接在调用方代码中使用）。</span><span class="sxs-lookup"><span data-stu-id="581ed-105">This property, unless used directly in caller code, is used only when the <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode> property is <xref:System.Security.Cryptography.CipherMode.CFB?displayProperty=nameWithType>.</span></span>

<span data-ttu-id="581ed-106">向 .NET 5.0 RC1 版本中首次添加了对 <xref:System.Security.Cryptography.CipherMode.CFB> 模式的支持，因此只有 .NET 5 RC1 和 .NET 5 RC2 应用程序会受到此更改的影响。</span><span class="sxs-lookup"><span data-stu-id="581ed-106">Support for the <xref:System.Security.Cryptography.CipherMode.CFB> mode was first added to .NET for the 5.0 RC1 release, so only .NET 5 RC1 and .NET 5 RC2 applications should be impacted by this change.</span></span>

## <a name="change-description"></a><span data-ttu-id="581ed-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="581ed-107">Change description</span></span>

<span data-ttu-id="581ed-108">在 .NET Core 和早期的 .NET 5 预发行版中，`TripleDES.Create().FeedbackSize` 的默认值为 64。</span><span class="sxs-lookup"><span data-stu-id="581ed-108">In .NET Core and previous pre-release versions of .NET 5, `TripleDES.Create().FeedbackSize` has a default value of 64.</span></span> <span data-ttu-id="581ed-109">从 .NET 5 的 RTM 版本开始，`TripleDES.Create().FeedbackSize` 的默认值为 8。</span><span class="sxs-lookup"><span data-stu-id="581ed-109">Starting in the RTM version of .NET 5, `TripleDES.Create().FeedbackSize` has a default value of 8.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="581ed-110">更改原因</span><span class="sxs-lookup"><span data-stu-id="581ed-110">Reason for change</span></span>

<span data-ttu-id="581ed-111">在 .NET Framework 中，<xref:System.Security.Cryptography.TripleDES> 基类将 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 的值默认为 64，但 <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider> 类将默认值覆盖为 8。</span><span class="sxs-lookup"><span data-stu-id="581ed-111">In .NET Framework, the <xref:System.Security.Cryptography.TripleDES> base class defaults the value of <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> to 64, but the <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider> class overwrites the default to 8.</span></span> <span data-ttu-id="581ed-112">将 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 属性引入到版本 2.0 的 .NET Core 时，将保留此相同行为。</span><span class="sxs-lookup"><span data-stu-id="581ed-112">When the <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> property was introduced to .NET Core in version 2.0, this same behavior was preserved.</span></span> <span data-ttu-id="581ed-113">但是，在 .NET Framework 中，<xref:System.Security.Cryptography.TripleDES.Create?displayProperty=nameWithType> 返回 <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider> 的实例，因此算法中心中的默认值为 8。</span><span class="sxs-lookup"><span data-stu-id="581ed-113">However, in .NET Framework, <xref:System.Security.Cryptography.TripleDES.Create?displayProperty=nameWithType> returns an instance of <xref:System.Security.Cryptography.TripleDESCryptoServiceProvider>, so the default value from the algorithm factory is 8.</span></span> <span data-ttu-id="581ed-114">对于 .NET Core 和 .NET 5 +，算法中心返回非公共实现，在此之前，其默认值为 64。</span><span class="sxs-lookup"><span data-stu-id="581ed-114">For .NET Core and .NET 5+, the algorithm factory returns a non-public implementation, which, until now, had a default value of 64.</span></span>

<span data-ttu-id="581ed-115">如果将 <xref:System.Security.Cryptography.TripleDES> 实现类的 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 值更改为 8，允许为将密码模式指定为 <xref:System.Security.Cryptography.CipherMode.CFB> 但未显式分配 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 属性的 .NET Framework 编写的应用程序继续在 .NET 5 上运行。</span><span class="sxs-lookup"><span data-stu-id="581ed-115">Changing the <xref:System.Security.Cryptography.TripleDES> implementation class' <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> value to 8 allows for applications written for .NET Framework that specified the cipher mode as <xref:System.Security.Cryptography.CipherMode.CFB> but didn't explicitly assign the <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> property, to continue to function on .NET 5.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="581ed-116">引入的版本</span><span class="sxs-lookup"><span data-stu-id="581ed-116">Version introduced</span></span>

<span data-ttu-id="581ed-117">5.0</span><span class="sxs-lookup"><span data-stu-id="581ed-117">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="581ed-118">建议操作</span><span class="sxs-lookup"><span data-stu-id="581ed-118">Recommended action</span></span>

<span data-ttu-id="581ed-119">当满足以下条件时，对 RC1 或 RC2 版本的 .NET 5 中的数据进行加密或解密的应用程序使用 CFB64 执行此操作：</span><span class="sxs-lookup"><span data-stu-id="581ed-119">Applications that encrypt or decrypt data in the RC1 or RC2 versions of .NET 5 do so with CFB64, when the following conditions are met:</span></span>

- <span data-ttu-id="581ed-120">具有 <xref:System.Security.Cryptography.TripleDES.Create?displayProperty=nameWithType> 的 <xref:System.Security.Cryptography.TripleDES> 实例。</span><span class="sxs-lookup"><span data-stu-id="581ed-120">With a <xref:System.Security.Cryptography.TripleDES> instance from <xref:System.Security.Cryptography.TripleDES.Create?displayProperty=nameWithType>.</span></span>
- <span data-ttu-id="581ed-121">使用 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 的默认值。</span><span class="sxs-lookup"><span data-stu-id="581ed-121">Using the default value for <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize>.</span></span>
- <span data-ttu-id="581ed-122"><xref:System.Security.Cryptography.SymmetricAlgorithm.Mode> 属性设置为 <xref:System.Security.Cryptography.CipherMode.CFB?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="581ed-122">With the <xref:System.Security.Cryptography.SymmetricAlgorithm.Mode> property set to <xref:System.Security.Cryptography.CipherMode.CFB?displayProperty=nameWithType>.</span></span>

<span data-ttu-id="581ed-123">若要保持此行为，请将 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 属性分配为 `64`。</span><span class="sxs-lookup"><span data-stu-id="581ed-123">To maintain this behavior, assign the <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> property to `64`.</span></span>

<span data-ttu-id="581ed-124">并非所有 `TripleDES` 实现对 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 都使用相同的默认值。</span><span class="sxs-lookup"><span data-stu-id="581ed-124">Not all `TripleDES` implementations use the same default for <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize>.</span></span> <span data-ttu-id="581ed-125">如果对 <xref:System.Security.Cryptography.TripleDES> 实例使用 <xref:System.Security.Cryptography.CipherMode.CFB> 密码模式，则建议你始终显式分配 <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> 属性值。</span><span class="sxs-lookup"><span data-stu-id="581ed-125">We recommend that if you use the <xref:System.Security.Cryptography.CipherMode.CFB> cipher mode on <xref:System.Security.Cryptography.TripleDES> instances, you should always explicitly assign the <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize> property value.</span></span>

```csharp
TripleDES cipher = TripleDES.Create();
cipher.Mode = CipherMode.CFB;
// Explicitly set the FeedbackSize for CFB to control between CFB8 and CFB64.
cipher.FeedbackSize = 8;
```

## <a name="affected-apis"></a><span data-ttu-id="581ed-126">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="581ed-126">Affected APIs</span></span>

- <xref:System.Security.Cryptography.TripleDES.Create?displayProperty=fullName>
- <xref:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize?displayProperty=fullName>

<!--

### Affected APIs

- `M:System.Security.Cryptography.TripleDES.Create`
- `P:System.Security.Cryptography.SymmetricAlgorithm.FeedbackSize`

### Category

- Cryptography

-->
