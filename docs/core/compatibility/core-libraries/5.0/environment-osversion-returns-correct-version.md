---
title: 中断性变更：Environment.OSVersion 返回正确的操作系统版本
description: 了解核心 .NET 库中的 .NET 5 中断性变更：Environment.OSVersion 返回操作系统的实际版本而不是为应用程序兼容性选择的 OS。
ms.date: 11/01/2020
ms.openlocfilehash: 05ec886061263ea43a2d956b8ce0ecc06e2bf3ad
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257526"
---
# <a name="environmentosversion-returns-the-correct-operating-system-version"></a><span data-ttu-id="e149c-103">Environment.OSVersion 返回正确的操作系统版本</span><span class="sxs-lookup"><span data-stu-id="e149c-103">Environment.OSVersion returns the correct operating system version</span></span>

<span data-ttu-id="e149c-104"><xref:System.Environment.OSVersion?displayProperty=nameWithType> 返回操作系统 (OS) 的实际版本，而不是为应用程序兼容性而选择的 OS。</span><span class="sxs-lookup"><span data-stu-id="e149c-104"><xref:System.Environment.OSVersion?displayProperty=nameWithType> returns the actual version of the operating system (OS) instead of, for example, the OS that's selected for application compatibility.</span></span>

## <a name="change-description"></a><span data-ttu-id="e149c-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="e149c-105">Change description</span></span>

<span data-ttu-id="e149c-106">在以前的 .NET 版本中，当应用程序在 Windows 兼容模式下运行时，<xref:System.Environment.OSVersion?displayProperty=nameWithType> 返回的 OS 版本可能不正确。</span><span class="sxs-lookup"><span data-stu-id="e149c-106">In previous .NET versions, <xref:System.Environment.OSVersion?displayProperty=nameWithType> returns an OS version that may be incorrect when an application runs under Windows compatibility mode.</span></span> <span data-ttu-id="e149c-107">有关详细信息，请参阅 [GetVersionExA 函数备注](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversionexa#remarks)。</span><span class="sxs-lookup"><span data-stu-id="e149c-107">For more information, see [GetVersionExA function remarks](/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversionexa#remarks).</span></span> <span data-ttu-id="e149c-108">在 macOS 上，<xref:System.Environment.OSVersion?displayProperty=nameWithType> 返回基础 Darwin 内核版本。</span><span class="sxs-lookup"><span data-stu-id="e149c-108">On macOS, <xref:System.Environment.OSVersion?displayProperty=nameWithType> returns the underlying Darwin kernel version.</span></span>

<span data-ttu-id="e149c-109">从 .NET 5 开始，<xref:System.Environment.OSVersion?displayProperty=nameWithType> 返回适用于 Windows 和 macOS 的操作系统的实际版本。</span><span class="sxs-lookup"><span data-stu-id="e149c-109">Starting in .NET 5, <xref:System.Environment.OSVersion?displayProperty=nameWithType> returns the actual version of the operating system for Windows and macOS.</span></span>

<span data-ttu-id="e149c-110">下表显示了行为差异。</span><span class="sxs-lookup"><span data-stu-id="e149c-110">The following table shows the difference in behavior.</span></span>

|  | <span data-ttu-id="e149c-111">以前的 .NET 版本</span><span class="sxs-lookup"><span data-stu-id="e149c-111">Previous .NET versions</span></span> | <span data-ttu-id="e149c-112">.NET 5+</span><span class="sxs-lookup"><span data-stu-id="e149c-112">.NET 5+</span></span> |
|--|------------------------|---------|
| <span data-ttu-id="e149c-113">Windows</span><span class="sxs-lookup"><span data-stu-id="e149c-113">Windows</span></span> | <span data-ttu-id="e149c-114">6.2.9200.0</span><span class="sxs-lookup"><span data-stu-id="e149c-114">6.2.9200.0</span></span> | <span data-ttu-id="e149c-115">10.0.19042.0</span><span class="sxs-lookup"><span data-stu-id="e149c-115">10.0.19042.0</span></span> |
| <span data-ttu-id="e149c-116">macOS</span><span class="sxs-lookup"><span data-stu-id="e149c-116">macOS</span></span> | <span data-ttu-id="e149c-117">19.6.0.0</span><span class="sxs-lookup"><span data-stu-id="e149c-117">19.6.0.0</span></span> | <span data-ttu-id="e149c-118">10.15.7</span><span class="sxs-lookup"><span data-stu-id="e149c-118">10.15.7</span></span> |

## <a name="reason-for-change"></a><span data-ttu-id="e149c-119">更改原因</span><span class="sxs-lookup"><span data-stu-id="e149c-119">Reason for change</span></span>

<span data-ttu-id="e149c-120">此属性的用户希望它返回操作系统的实际版本。</span><span class="sxs-lookup"><span data-stu-id="e149c-120">Users of this property expect it to return the actual version of the operating system.</span></span> <span data-ttu-id="e149c-121">大多数 .NET 应用并未在其应用程序清单中指定其支持的版本，因此会从 dotnet 主机获取默认的支持版本。</span><span class="sxs-lookup"><span data-stu-id="e149c-121">Most .NET apps don't specify their supported version in their application manifest, and thus get the default supported version from the dotnet host.</span></span> <span data-ttu-id="e149c-122">因此，兼容性填充码对于正在运行的应用没有什么意义。</span><span class="sxs-lookup"><span data-stu-id="e149c-122">As a result, the compatibility shim is rarely meaningful for the app that's running.</span></span> <span data-ttu-id="e149c-123">当 Windows 发布新版本时，如果较旧的 dotnet 主机仍在使用，这些应用可能会收到错误的 OS 版本。</span><span class="sxs-lookup"><span data-stu-id="e149c-123">When Windows releases a new version and an older dotnet host is still in use, these apps may get an incorrect OS version.</span></span> <span data-ttu-id="e149c-124">返回实际版本更符合开发人员对此 API 的期望。</span><span class="sxs-lookup"><span data-stu-id="e149c-124">Returning the actual version is more inline with developers' expectations of this API.</span></span>

<span data-ttu-id="e149c-125">随着在 .NET 5 中引入 <xref:System.OperatingSystem.IsWindowsVersionAtLeast%2A?displayProperty=nameWithType>、<xref:System.OperatingSystem.IsMacOSVersionAtLeast%2A?displayProperty=nameWithType> 和 <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute?displayProperty=nameWithType>，<xref:System.Environment.OSVersion?displayProperty=nameWithType> 已更改为与 Windows 和 macOS 一致。</span><span class="sxs-lookup"><span data-stu-id="e149c-125">With the introduction of <xref:System.OperatingSystem.IsWindowsVersionAtLeast%2A?displayProperty=nameWithType>, <xref:System.OperatingSystem.IsMacOSVersionAtLeast%2A?displayProperty=nameWithType>, and <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute?displayProperty=nameWithType> in .NET 5, <xref:System.Environment.OSVersion?displayProperty=nameWithType> was changed to be consistent for Windows and macOS.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="e149c-126">引入的版本</span><span class="sxs-lookup"><span data-stu-id="e149c-126">Version introduced</span></span>

<span data-ttu-id="e149c-127">5.0</span><span class="sxs-lookup"><span data-stu-id="e149c-127">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="e149c-128">建议操作</span><span class="sxs-lookup"><span data-stu-id="e149c-128">Recommended action</span></span>

<span data-ttu-id="e149c-129">查看和测试使用 <xref:System.Environment.OSVersion?displayProperty=nameWithType> 的任何代码，以确保其行为符合预期。</span><span class="sxs-lookup"><span data-stu-id="e149c-129">Review and test any code that uses <xref:System.Environment.OSVersion?displayProperty=nameWithType> to ensure it behaves as desired.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="e149c-130">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="e149c-130">Affected APIs</span></span>

- <xref:System.Environment.OSVersion?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Environment.OSVersion`

-->
