---
title: 中断性变更：嵌套类型的硬件内部 IsSupported 检查可能有所不同
description: 了解核心 .NET 库中的 .NET 5 中断性变更：检查硬件内部函数的 X64.IsSupported 现在可能会生成不同的结果。
ms.date: 11/01/2020
ms.openlocfilehash: 5c6049ad5881c0389870cfd5e1550f8358c67599
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257448"
---
# <a name="hardware-intrinsic-issupported-checks-may-differ-for-nested-types"></a><span data-ttu-id="1f9c4-103">嵌套类型的硬件内部 IsSupported 检查可能有所不同</span><span class="sxs-lookup"><span data-stu-id="1f9c4-103">Hardware intrinsic IsSupported checks may differ for nested types</span></span>

<span data-ttu-id="1f9c4-104">现在，如果检查 `<Isa>.X64.IsSupported`（其中 `<Isa>` 引用了 <xref:System.Runtime.Intrinsics.X86?displayProperty=nameWithType> 命名空间中的类），可能会生成与 .NET 早期版本不同的结果。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-104">Checking `<Isa>.X64.IsSupported`, where `<Isa>` refers to the classes in the <xref:System.Runtime.Intrinsics.X86?displayProperty=nameWithType> namespace, may now produce a different result to previous versions of .NET.</span></span>

> [!TIP]
> <span data-ttu-id="1f9c4-105">ISA 的全称是工业标准体系结构。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-105">*ISA* stands for industry standard architecture.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="1f9c4-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="1f9c4-106">Version introduced</span></span>

<span data-ttu-id="1f9c4-107">5.0</span><span class="sxs-lookup"><span data-stu-id="1f9c4-107">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="1f9c4-108">更改描述</span><span class="sxs-lookup"><span data-stu-id="1f9c4-108">Change description</span></span>

<span data-ttu-id="1f9c4-109">在 .NET 的早期版本中，某些 <xref:System.Runtime.Intrinsics.X86> 硬件内部类型（例如 <xref:System.Runtime.Intrinsics.X86.Aes?displayProperty=nameWithType>）未公开嵌套的 `X64` 类。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-109">In previous versions of .NET, some of the <xref:System.Runtime.Intrinsics.X86> hardware-intrinsic types, for example, <xref:System.Runtime.Intrinsics.X86.Aes?displayProperty=nameWithType>, didn't expose a nested `X64` class.</span></span> <span data-ttu-id="1f9c4-110">对于这些类型，调用 `<Isa>.X64.IsSupported` 解析为父类 `<Isa>` 的嵌套 `X64` 类上的 `IsSupported` 属性。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-110">For these types, calling `<Isa>.X64.IsSupported` resolved to an `IsSupported` property on a nested `X64` class of a parent class of `<Isa>`.</span></span> <span data-ttu-id="1f9c4-111">这意味着即使 `<Isa>.IsSupported` 返回 `false`，该属性也可返回 `true`。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-111">This meant that the property could return `true` even when `<Isa>.IsSupported` returns `false`.</span></span>

<span data-ttu-id="1f9c4-112">在 .NET 5 及更高版本中，所有 <xref:System.Runtime.Intrinsics.X86> 类型都公开适当报告支持的嵌套的 `X64` 类。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-112">In .NET 5 and later versions, all of the <xref:System.Runtime.Intrinsics.X86> types expose a nested `X64` class that appropriately reports support.</span></span> <span data-ttu-id="1f9c4-113">这可确保一般层次结构保持正确；如果 `<Isa>.X64.IsSupported` 为 `true`，则也可假定 `<Isa>.IsSupported` 为 `true`。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-113">This ensures that the general hierarchy remains correct, and that if `<Isa>.X64.IsSupported` is `true`, then `<Isa>.IsSupported` can also be assumed to be `true`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="1f9c4-114">更改原因</span><span class="sxs-lookup"><span data-stu-id="1f9c4-114">Reason for change</span></span>

<span data-ttu-id="1f9c4-115">预期结果是，如果 `<Isa>.X64.IsSupported` 为 `true`，则也暗示 `<Isa>.IsSupported` 为 `true`。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-115">It was intended that if `<Isa>.X64.IsSupported` is `true`, `<Isa>.IsSupported` is also implied to be `true`.</span></span> <span data-ttu-id="1f9c4-116">但是，由于成员解析在 C# 中的工作方式，没有嵌套的 `X64` 类的类会导致并不总是这种结果且用户代码中会出现 bug。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-116">However, due to how member resolution works in C#, classes that didn't have a nested `X64` class exposed a situation where this wasn't always the case and led to bugs in user code.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="1f9c4-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="1f9c4-117">Recommended action</span></span>

<span data-ttu-id="1f9c4-118">如有必要，请调整检查 `IsSupported` 的代码，以检查相应的 ISA。</span><span class="sxs-lookup"><span data-stu-id="1f9c4-118">If necessary, adjust code that checks `IsSupported` to check for the appropriate ISA.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="1f9c4-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="1f9c4-119">Affected APIs</span></span>

- <xref:System.Runtime.Intrinsics.X86.Aes.X64.IsSupported?displayProperty=fullName>
- <xref:System.Runtime.Intrinsics.X86.Avx.X64.IsSupported?displayProperty=fullName>
- <xref:System.Runtime.Intrinsics.X86.Avx2.X64.IsSupported?displayProperty=fullName>
- <xref:System.Runtime.Intrinsics.X86.Fma.X64.IsSupported?displayProperty=fullName>
- <xref:System.Runtime.Intrinsics.X86.Pclmulqdq.X64.IsSupported?displayProperty=fullName>
- <xref:System.Runtime.Intrinsics.X86.Sse3.X64.IsSupported?displayProperty=fullName>
- <xref:System.Runtime.Intrinsics.X86.Ssse3.X64.IsSupported?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Runtime.Intrinsics.X86.Aes.X64.IsSupported`
- `P:System.Runtime.Intrinsics.X86.Avx.X64.IsSupported`
- `P:System.Runtime.Intrinsics.X86.Avx2.X64.IsSupported`
- `P:System.Runtime.Intrinsics.X86.Fma.X64.IsSupported`
- `P:System.Runtime.Intrinsics.X86.Pclmulqdq.X64.IsSupported`
- `P:System.Runtime.Intrinsics.X86.Sse3.X64.IsSupported`
- `P:System.Runtime.Intrinsics.X86.Ssse3.X64.IsSupported`

-->
