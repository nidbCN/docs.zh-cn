---
title: 中断性变更：已重命名或已删除 OSPlatform 属性
description: 了解核心 .NET 库中的 .NET 5 中断性变更：预览版本中引入的 OS 平台属性已被删除或已被重命名。
ms.date: 11/01/2020
ms.openlocfilehash: 118c5360eb02c1b4d57fccbd3b2cfdeff0b9fe12
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257253"
---
# <a name="osplatform-attributes-renamed-or-removed"></a><span data-ttu-id="2aee6-103">已重命名或已删除 OSPlatform 属性</span><span class="sxs-lookup"><span data-stu-id="2aee6-103">OSPlatform attributes renamed or removed</span></span>

<span data-ttu-id="2aee6-104">已删除或已重命名 .NET 5 预览版 8 中引入的以下属性：`MinimumOSPlatformAttribute`、`RemovedInOSPlatformAttribute` 和 `ObsoletedInOSPlatformAttribute`。</span><span class="sxs-lookup"><span data-stu-id="2aee6-104">The following attributes that were introduced in .NET 5 Preview 8 have been removed or renamed: `MinimumOSPlatformAttribute`, `RemovedInOSPlatformAttribute`, and `ObsoletedInOSPlatformAttribute`.</span></span>

## <a name="change-description"></a><span data-ttu-id="2aee6-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="2aee6-105">Change description</span></span>

<span data-ttu-id="2aee6-106">.NET 5 预览版 8 在 <xref:System.Runtime.Versioning> 命名空间中引入了以下属性：</span><span class="sxs-lookup"><span data-stu-id="2aee6-106">.NET 5 Preview 8 introduced the following attributes in the <xref:System.Runtime.Versioning> namespace:</span></span>

- `MinimumOSPlatformAttribute`
- `RemovedInOSPlatformAttribute`
- `ObsoletedInOSPlatformAttribute`

<span data-ttu-id="2aee6-107">在 .NET 5 预览版 8 中，当某个项目通过使用[目标框架名字对象](../../../../standard/frameworks.md)（如 `net5.0-windows`）以特定于 OS 风格的 .NET 5 为目标时，生成将添加程序集级别的 `System.Runtime.Versioning.MinimumOSPlatformAttribute` 属性。</span><span class="sxs-lookup"><span data-stu-id="2aee6-107">In .NET 5 Preview 8, when a project targets an OS-specific flavor of .NET 5 by using a [target framework moniker](../../../../standard/frameworks.md) such as `net5.0-windows`, the build adds an assembly-level `System.Runtime.Versioning.MinimumOSPlatformAttribute` attribute.</span></span>

<span data-ttu-id="2aee6-108">在 .NET 5 RC1 中，已删除 `ObsoletedInOSPlatformAttribute`，并已将 `MinimumOSPlatformAttribute` 和 `RemovedInOSPlatformAttribute` 重命名为以下内容：</span><span class="sxs-lookup"><span data-stu-id="2aee6-108">In .NET 5 RC1, the `ObsoletedInOSPlatformAttribute` has been removed, and `MinimumOSPlatformAttribute` and `RemovedInOSPlatformAttribute` have been renamed as follows:</span></span>

| <span data-ttu-id="2aee6-109">预览版 8 名称</span><span class="sxs-lookup"><span data-stu-id="2aee6-109">Preview 8 name</span></span> | <span data-ttu-id="2aee6-110">RC1 及更高版本名称</span><span class="sxs-lookup"><span data-stu-id="2aee6-110">RC1 and later name</span></span> |
| - | - |
| `MinimumOSPlatformAttribute` | <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> |
| `RemovedInOSPlatformAttribute` | <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute> |

<span data-ttu-id="2aee6-111">在 .NET 5 RC1 及更高版本中，当某个项目通过使用[目标框架名字对象](../../../../standard/frameworks.md)（如 `net5.0-windows`）以特定于 OS 风格的 .NET 5 为目标时，生成将添加程序集级别的 <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> 属性。</span><span class="sxs-lookup"><span data-stu-id="2aee6-111">In .NET 5 RC1 and later, when a project targets an OS-specific flavor of .NET 5 by using a [target framework moniker](../../../../standard/frameworks.md) such as `net5.0-windows`, the build adds an assembly-level <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute> attribute.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="2aee6-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="2aee6-112">Reason for change</span></span>

<span data-ttu-id="2aee6-113">.NET 5 预览版 8 在 <xref:System.Runtime.Versioning> 中引入了多个属性，用于指定支持 API 的平台。</span><span class="sxs-lookup"><span data-stu-id="2aee6-113">.NET 5 Preview 8 introduced attributes in <xref:System.Runtime.Versioning> to specify supported platforms for APIs.</span></span> <span data-ttu-id="2aee6-114">将特定于平台的 API 用于不支持这些 API 的平台时，[平台兼容性分析器](../../code-analysis/5.0/ca1416-platform-compatibility-analyzer.md)使用这些属性生成生成警告。</span><span class="sxs-lookup"><span data-stu-id="2aee6-114">The attributes are consumed by the [Platform compatibility analyzer](../../code-analysis/5.0/ca1416-platform-compatibility-analyzer.md) to produce build warnings when platform-specific APIs are consumed on platforms that don't support those APIs.</span></span>

<span data-ttu-id="2aee6-115">对于 .NET 5 RC1，向平台兼容性分析器添加了一项附加功能，用于排除平台。</span><span class="sxs-lookup"><span data-stu-id="2aee6-115">For .NET 5 RC1, an additional feature was added to the platform compatibility analyzer for platform exclusion.</span></span> <span data-ttu-id="2aee6-116">此功能允许将 API 标记为在 OS 平台上完全不受支持。</span><span class="sxs-lookup"><span data-stu-id="2aee6-116">The feature allows APIs to be marked as entirely unsupported on OS platforms.</span></span> <span data-ttu-id="2aee6-117">此功能会提示更改属性，包括使用更合适的名称。</span><span class="sxs-lookup"><span data-stu-id="2aee6-117">That feature prompted changes to the attributes, including using more suitable names.</span></span> <span data-ttu-id="2aee6-118">由于不再需要 `ObsoletedInOSPlatformAttribute`，已将其删除。</span><span class="sxs-lookup"><span data-stu-id="2aee6-118">The `ObsoletedInOSPlatformAttribute` was removed because it was no longer needed.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="2aee6-119">引入的版本</span><span class="sxs-lookup"><span data-stu-id="2aee6-119">Version introduced</span></span>

<span data-ttu-id="2aee6-120">5.0 RC1</span><span class="sxs-lookup"><span data-stu-id="2aee6-120">5.0 RC1</span></span>

## <a name="recommended-action"></a><span data-ttu-id="2aee6-121">建议操作</span><span class="sxs-lookup"><span data-stu-id="2aee6-121">Recommended action</span></span>

<span data-ttu-id="2aee6-122">将项目从 .NET 5 预览版 8 重定目标到 .NET 5 RC1 时，可能会遇到由于这些更改而导致的生成或运行时错误。</span><span class="sxs-lookup"><span data-stu-id="2aee6-122">When you retarget your project from .NET 5 Preview 8 to .NET 5 RC1, you might encounter build or run-time errors due to these changes.</span></span> <span data-ttu-id="2aee6-123">例如，重命名 `MinimumOSPlatformAttribute` 可能会产生错误，因为该属性在生成时会应用于特定于平台的程序集，而旧的生成项目仍将引用旧的 API 名称。</span><span class="sxs-lookup"><span data-stu-id="2aee6-123">For example, the renaming of `MinimumOSPlatformAttribute` is likely to produce errors, because the attribute is applied to platform-specific assemblies at build time, and old build artifacts will still reference the old API name.</span></span>

<span data-ttu-id="2aee6-124">生成时错误示例：</span><span class="sxs-lookup"><span data-stu-id="2aee6-124">Example build-time errors:</span></span>

- <span data-ttu-id="2aee6-125">**错误 CS0246：找不到类型名称或命名空间名称“MinimumOSPlatformAttribute”（是否缺少 using 指令或程序集引用？）**</span><span class="sxs-lookup"><span data-stu-id="2aee6-125">**error CS0246: The type or namespace name 'MinimumOSPlatformAttribute' could not be found (are you missing a using directive or an assembly reference?)**</span></span>
- <span data-ttu-id="2aee6-126">**错误 CS0246：找不到类型名称或命名空间名称“RemovedInOSPlatformAttribute”（是否缺少 using 指令或程序集引用？）**</span><span class="sxs-lookup"><span data-stu-id="2aee6-126">**error CS0246: The type or namespace name 'RemovedInOSPlatformAttribute' could not be found (are you missing a using directive or an assembly reference?)**</span></span>
- <span data-ttu-id="2aee6-127">**错误 CS0246：找不到类型名称或命名空间名称“ObsoletedInOSPlatformAttribute”（是否缺少 using 指令或程序集引用？）**</span><span class="sxs-lookup"><span data-stu-id="2aee6-127">**error CS0246: The type or namespace name 'ObsoletedInOSPlatformAttribute' could not be found (are you missing a using directive or an assembly reference?)**</span></span>

<span data-ttu-id="2aee6-128">运行时错误示例：</span><span class="sxs-lookup"><span data-stu-id="2aee6-128">Example run-time error:</span></span>

<span data-ttu-id="2aee6-129">**未经处理的异常。System.TypeLoadException 异常:无法从程序集“System.Runtime, Version=5.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a”中找到加载类型“System.Runtime.Versioning.MinimumOSPlatformAttribute”。**</span><span class="sxs-lookup"><span data-stu-id="2aee6-129">**Unhandled exception. System.TypeLoadException: Could not load type 'System.Runtime.Versioning.MinimumOSPlatformAttribute' from assembly 'System.Runtime, Version=5.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a'.**</span></span>

<span data-ttu-id="2aee6-130">若要解决这些错误：</span><span class="sxs-lookup"><span data-stu-id="2aee6-130">To resolve these errors:</span></span>

- <span data-ttu-id="2aee6-131">将 `MinimumOSPlatformAttribute` 的任何引用更新为 <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute>。</span><span class="sxs-lookup"><span data-stu-id="2aee6-131">Update any references of `MinimumOSPlatformAttribute` to <xref:System.Runtime.Versioning.SupportedOSPlatformAttribute>.</span></span>
- <span data-ttu-id="2aee6-132">将 `RemovedInOSPlatformAttribute` 的任何引用更新为 <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute>。</span><span class="sxs-lookup"><span data-stu-id="2aee6-132">Update any references of `RemovedInOSPlatformAttribute` to <xref:System.Runtime.Versioning.UnsupportedOSPlatformAttribute>.</span></span>
- <span data-ttu-id="2aee6-133">删除 `ObsoletedInOSPlatformAttribute` 的任何引用。</span><span class="sxs-lookup"><span data-stu-id="2aee6-133">Remove any references to `ObsoletedInOSPlatformAttribute`.</span></span>
- <span data-ttu-id="2aee6-134">重新生成项目（或执行“清理 + 生成”）以删除旧的生成项目。</span><span class="sxs-lookup"><span data-stu-id="2aee6-134">Rebuild your project (or perform clean + build) to delete old build artifacts.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="2aee6-135">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="2aee6-135">Affected APIs</span></span>

- `System.Runtime.Versioning.MinimumOSPlatformAttribute`
- `System.Runtime.Versioning.ObsoletedInOSPlatformAttribute`
- `System.Runtime.Versioning.RemovedInOSPlatformAttribute`

<!--

### Category

Core .NET libraries

### Affected APIs

- `T:System.Runtime.Versioning.MinimumOSPlatformAttribute`
- `T:System.Runtime.Versioning.ObsoletedInOSPlatformAttribute`
- `T:System.Runtime.Versioning.RemovedInOSPlatformAttribute`

-->
