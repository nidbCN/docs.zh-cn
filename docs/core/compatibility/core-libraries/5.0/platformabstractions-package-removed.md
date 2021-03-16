---
title: 中断性变更：Microsoft.DotNet.PlatformAbstractions 包已删除
description: 了解核心 .NET 库中的 .NET 5 中断性变更：Microsoft.DotNet.PlatformAbstractions 包已删除。
ms.date: 11/01/2020
ms.openlocfilehash: aff7be816815b016e3ce694c4e9a97410538c08d
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257240"
---
# <a name="microsoftdotnetplatformabstractions-package-removed"></a><span data-ttu-id="95730-103">Microsoft.DotNet.PlatformAbstractions 包已删除</span><span class="sxs-lookup"><span data-stu-id="95730-103">Microsoft.DotNet.PlatformAbstractions package removed</span></span>

<span data-ttu-id="95730-104">将不生成新版本的 [Microsoft.DotNet.PlatformAbstractions NuGet 包](https://www.nuget.org/packages/Microsoft.DotNet.PlatformAbstractions/)。</span><span class="sxs-lookup"><span data-stu-id="95730-104">No new versions of the [Microsoft.DotNet.PlatformAbstractions NuGet package](https://www.nuget.org/packages/Microsoft.DotNet.PlatformAbstractions/) will be produced.</span></span>

## <a name="change-description"></a><span data-ttu-id="95730-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="95730-105">Change description</span></span>

<span data-ttu-id="95730-106">以前，新版本的 <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> 库随新版本的 .NET Core 一起生成。</span><span class="sxs-lookup"><span data-stu-id="95730-106">Previously, new versions of the <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> library were produced alongside new versions of .NET Core.</span></span> <span data-ttu-id="95730-107">今后，将不会向库中添加新功能，也不会发布新的主版本。</span><span class="sxs-lookup"><span data-stu-id="95730-107">Going forward, no new functionality will be added to the library, and no new major versions will be released.</span></span> <span data-ttu-id="95730-108">但是，现有版本的库将继续有效并受到维护。</span><span class="sxs-lookup"><span data-stu-id="95730-108">However, existing versions of the library will continue to work and be serviced.</span></span>

<span data-ttu-id="95730-109"><xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> 库与 System.\* 命名空间中已建立的 API 重叠。</span><span class="sxs-lookup"><span data-stu-id="95730-109">The <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> library overlaps with APIs that are already established in the System.\* namespaces.</span></span> <span data-ttu-id="95730-110">此外，某些 <xref:Microsoft.DotNet.PlatformAbstractions> API 并未设计成具有与 System.\* 的其余部分相同的审查和长期可支持性。访问。</span><span class="sxs-lookup"><span data-stu-id="95730-110">Also, some <xref:Microsoft.DotNet.PlatformAbstractions> APIs weren't designed with the same level of scrutiny and long-term supportability as the rest of the System.\* APIs.</span></span> <span data-ttu-id="95730-111">例如，<xref:Microsoft.DotNet.PlatformAbstractions> 使用 `Platform` 枚举来描述当前的操作系统平台。</span><span class="sxs-lookup"><span data-stu-id="95730-111">For example, <xref:Microsoft.DotNet.PlatformAbstractions> uses the `Platform` enumeration to describe the current operating system platform.</span></span> <span data-ttu-id="95730-112">如果设计了 <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType> API，则会明确拒绝此枚举设计，以支持新的平台和未来的灵活性。</span><span class="sxs-lookup"><span data-stu-id="95730-112">This enumeration design was explicitly rejected when the <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType> API was designed, to allow for new platforms and future flexibility.</span></span>

<span data-ttu-id="95730-113">现在，<xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> 库启用的方案可以在没有它的情况下使用。</span><span class="sxs-lookup"><span data-stu-id="95730-113">The scenarios enabled by the <xref:Microsoft.DotNet.PlatformAbstractions?displayProperty=fullName> library are now possible without it.</span></span> <span data-ttu-id="95730-114">现有版本即使在 .NET 5 和更高版本中也将继续有效，并将与以前版本的 .NET Core 一起受到维护。</span><span class="sxs-lookup"><span data-stu-id="95730-114">Existing versions will continue to work, even in .NET 5 and later, and will be serviced along with previous versions of .NET Core.</span></span> <span data-ttu-id="95730-115">但是，新功能将不会添加到该库中，</span><span class="sxs-lookup"><span data-stu-id="95730-115">However, new functionality won't be added to the library.</span></span> <span data-ttu-id="95730-116">而是将添加到其他库和 API 中。</span><span class="sxs-lookup"><span data-stu-id="95730-116">Instead, new functionality will be added to other libraries and APIs.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="95730-117">引入的版本</span><span class="sxs-lookup"><span data-stu-id="95730-117">Version introduced</span></span>

<span data-ttu-id="95730-118">5.0</span><span class="sxs-lookup"><span data-stu-id="95730-118">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="95730-119">建议操作</span><span class="sxs-lookup"><span data-stu-id="95730-119">Recommended action</span></span>

- <span data-ttu-id="95730-120">如果较旧版本的库可满足需求，则可以继续使用它们。</span><span class="sxs-lookup"><span data-stu-id="95730-120">You can continue to use older versions of the library if they meet your requirements.</span></span>

- <span data-ttu-id="95730-121">如果较旧版本无法满足需求，请使用建议的替换项替换 `PlatformAbstractions` API 的用法。</span><span class="sxs-lookup"><span data-stu-id="95730-121">If the older versions don't meet your requirements, replace usages of the `PlatformAbstractions` APIs with the recommended replacements.</span></span>

  | <span data-ttu-id="95730-122">`PlatformAbstractions` API</span><span class="sxs-lookup"><span data-stu-id="95730-122">`PlatformAbstractions` API</span></span> | <span data-ttu-id="95730-123">推荐的替换控件</span><span class="sxs-lookup"><span data-stu-id="95730-123">Recommended replacement</span></span> |
  |-|-|
  | `ApplicationEnvironment.ApplicationBasePath` | <xref:System.AppContext.BaseDirectory?displayProperty=nameWithType> |
  | <xref:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner> | <xref:System.HashCode?displayProperty=nameWithType> |
  | `RuntimeEnvironment.GetRuntimeIdentifier()` | <xref:System.Runtime.InteropServices.RuntimeInformation.RuntimeIdentifier?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystemPlatform` | <xref:System.Runtime.InteropServices.RuntimeInformation.IsOSPlatform(System.Runtime.InteropServices.OSPlatform)?displayProperty=nameWithType> |
  | `RuntimeEnvironment.RuntimeArchitecture` | <xref:System.Runtime.InteropServices.RuntimeInformation.ProcessArchitecture?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystem` | <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> |
  | `RuntimeEnvironment.OperatingSystemVersion` | <span data-ttu-id="95730-124"><xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> 和 <xref:System.Environment.OSVersion?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="95730-124"><xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> and <xref:System.Environment.OSVersion?displayProperty=nameWithType></span></span> |

  > [!NOTE]
  > <span data-ttu-id="95730-125">`RuntimeEnvironment.OperatingSystem` 和 `RuntimeEnvironment.OperatingSystemVersion` 的大多数用例都用于显示目的，例如，向用户、日志记录和遥测显示。</span><span class="sxs-lookup"><span data-stu-id="95730-125">Most use cases for `RuntimeEnvironment.OperatingSystem` and `RuntimeEnvironment.OperatingSystemVersion` are for display purposes, for example, displaying to a user, logging, and telemetry.</span></span> <span data-ttu-id="95730-126">不建议根据操作系统 (OS) 版本做出运行时决策。</span><span class="sxs-lookup"><span data-stu-id="95730-126">It's not recommended to make run-time decisions based on an operating system (OS) version.</span></span> <span data-ttu-id="95730-127">现在，<xref:System.Environment.OSVersion?displayProperty=nameWithType> 返回适用于 Windows 和 macOS 操作系统的[正确版本](environment-osversion-returns-correct-version.md)。</span><span class="sxs-lookup"><span data-stu-id="95730-127"><xref:System.Environment.OSVersion?displayProperty=nameWithType> now [returns the correct version](environment-osversion-returns-correct-version.md) for Windows and macOS operating systems.</span></span> <span data-ttu-id="95730-128">但是，对于大多数 Unix 发行版，所谓的“OS 版本”并不是那么简单。</span><span class="sxs-lookup"><span data-stu-id="95730-128">However, for most Unix distributions, what is considered to be the "OS version" is not as straightforward.</span></span> <span data-ttu-id="95730-129">例如，它可以是 Linux 内核版本，也可以是发行版本。</span><span class="sxs-lookup"><span data-stu-id="95730-129">For example, it could be the Linux kernel version, or it could be the distro version.</span></span> <span data-ttu-id="95730-130">对于大多数 Unix 平台，<xref:System.Environment.OSVersion?displayProperty=nameWithType> 和 <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> 返回 `uname` 返回的版本。</span><span class="sxs-lookup"><span data-stu-id="95730-130">For most Unix platforms, <xref:System.Environment.OSVersion?displayProperty=nameWithType> and <xref:System.Runtime.InteropServices.RuntimeInformation.OSDescription?displayProperty=nameWithType> return the version that's returned by `uname`.</span></span> <span data-ttu-id="95730-131">要获取 Linux 发行版的名称和版本信息，建议的方法是读取 /etc/os-release 文件。</span><span class="sxs-lookup"><span data-stu-id="95730-131">To get the Linux distro name and version information, the recommended approach is to read the */etc/os-release* file.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="95730-132">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="95730-132">Affected APIs</span></span>

- `Microsoft.DotNet.PlatformAbstractions.ApplicationEnvironment.ApplicationBasePath`
- <xref:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner?displayProperty=fullName>
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.GetRuntimeIdentifier()`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystem`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemPlatform`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemVersion`
- `Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.RuntimeArchitecture`

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:Microsoft.DotNet.PlatformAbstractions.ApplicationEnvironment.ApplicationBasePath`
- `T:Microsoft.DotNet.PlatformAbstractions.HashCodeCombiner`
- `M:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.GetRuntimeIdentifier`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystem`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemPlatform`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.OperatingSystemVersion`
- `P:Microsoft.DotNet.PlatformAbstractions.RuntimeEnvironment.RuntimeArchitecture`

-->
