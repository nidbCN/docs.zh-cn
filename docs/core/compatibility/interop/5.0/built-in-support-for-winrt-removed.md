---
title: 中断性变更：已从 .NET 中删除对 WinRT 的内置支持
description: 了解 .NET 5 中的互操作中断性变更：已从 .NET 中删除对 WinRT 的内置支持。
ms.date: 10/13/2020
ms.openlocfilehash: 986b810b74c7e7d7514ec2b734bfab45e29b87fa
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256681"
---
# <a name="built-in-support-for-winrt-is-removed-from-net"></a><span data-ttu-id="3deaa-103">已从 .NET 中删除对 WinRT 的内置支持</span><span class="sxs-lookup"><span data-stu-id="3deaa-103">Built-in support for WinRT is removed from .NET</span></span>

<span data-ttu-id="3deaa-104">已删除对使用 .NET 中的 [Windows 运行时 (WinRT)](/uwp/winrt-cref/winrt-type-system) API 的内置支持。</span><span class="sxs-lookup"><span data-stu-id="3deaa-104">Built-in support for consumption of [Windows runtime (WinRT)](/uwp/winrt-cref/winrt-type-system) APIs in .NET is removed.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="3deaa-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="3deaa-105">Version introduced</span></span>

<span data-ttu-id="3deaa-106">5.0</span><span class="sxs-lookup"><span data-stu-id="3deaa-106">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="3deaa-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="3deaa-107">Change description</span></span>

<span data-ttu-id="3deaa-108">以前，CoreCLR 可能会使用 [Windows 元数据 (WinMD) 文件](/uwp/winrt-cref/winmd-files)来激活和使用 WinRT 类型。</span><span class="sxs-lookup"><span data-stu-id="3deaa-108">Previously, CoreCLR could consume [Windows metadata (WinMD) files](/uwp/winrt-cref/winmd-files) to active and consume WinRT types.</span></span> <span data-ttu-id="3deaa-109">从 .NET 5 开始，CoreCLR 不再能够直接使用 WinMD 文件。</span><span class="sxs-lookup"><span data-stu-id="3deaa-109">Starting in .NET 5, CoreCLR can no longer consume WinMD files directly.</span></span>

<span data-ttu-id="3deaa-110">如果尝试引用不受支持的程序集，则会收到 <xref:System.IO.FileNotFoundException>。</span><span class="sxs-lookup"><span data-stu-id="3deaa-110">If you attempt to reference an unsupported assembly, you'll get a <xref:System.IO.FileNotFoundException>.</span></span> <span data-ttu-id="3deaa-111">如果激活 WinRT 类，则会收到 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="3deaa-111">If you activate a WinRT class, you'll get a <xref:System.PlatformNotSupportedException>.</span></span>

<span data-ttu-id="3deaa-112">做出此中断性变更的原因如下：</span><span class="sxs-lookup"><span data-stu-id="3deaa-112">This breaking change was made for the following reasons:</span></span>

- <span data-ttu-id="3deaa-113">便于独立于 .NET 运行时单独开发和改进 WinRT。</span><span class="sxs-lookup"><span data-stu-id="3deaa-113">So WinRT can be developed and improved separately from the .NET runtime.</span></span>
- <span data-ttu-id="3deaa-114">为了与为其他操作系统（例如 iOS 和 Android）提供的互操作系统对称。</span><span class="sxs-lookup"><span data-stu-id="3deaa-114">For symmetry with interop systems provided for other operating systems, such as iOS and Android.</span></span>
- <span data-ttu-id="3deaa-115">为了利用其他 .NET 功能，例如 C# 功能、中间语言 (IL) 链接和预先 (AOT) 编译。</span><span class="sxs-lookup"><span data-stu-id="3deaa-115">To take advantage of other .NET features, such as C# features, intermediate language (IL) linking, and ahead-of-time (AOT) compilation.</span></span>
- <span data-ttu-id="3deaa-116">为了简化 .NET 运行时基本代码。</span><span class="sxs-lookup"><span data-stu-id="3deaa-116">To simplify the .NET runtime codebase.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="3deaa-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="3deaa-117">Recommended action</span></span>

- <span data-ttu-id="3deaa-118">删除对 [Microsoft.Windows.SDK.Contracts 包](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts)的引用。</span><span class="sxs-lookup"><span data-stu-id="3deaa-118">Remove references to the [Microsoft.Windows.SDK.Contracts package](https://www.nuget.org/packages/Microsoft.Windows.SDK.Contracts).</span></span>  <span data-ttu-id="3deaa-119">改为通过项目的 `TargetFramework` 属性指定要访问的 Windows API 版本。</span><span class="sxs-lookup"><span data-stu-id="3deaa-119">Instead, specify the version of the Windows APIs that you want to access via the `TargetFramework` property of the project.</span></span>  <span data-ttu-id="3deaa-120">例如： 。</span><span class="sxs-lookup"><span data-stu-id="3deaa-120">For example:</span></span>

  ```xml
  <TargetFramework>net5.0-windows10.0.19041</TargetFramework>
  ```

- <span data-ttu-id="3deaa-121">使用 [C#/WinRT](/windows/uwp/csharp-winrt/) 工具链生成或自定义针对 .NET 5 及更高版本的 WinRT API 和类型。</span><span class="sxs-lookup"><span data-stu-id="3deaa-121">Use the [C#/WinRT](/windows/uwp/csharp-winrt/) tool chain to generate or customize WinRT APIs and types for .NET 5 and later versions.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="3deaa-122">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="3deaa-122">Affected APIs</span></span>

- <xref:System.IO.WindowsRuntimeStorageExtensions?displayProperty=fullName>
- <xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=fullName>
- <xref:System.Runtime.InteropServices.WindowsRuntime?displayProperty=fullName>
- <xref:System.WindowsRuntimeSystemExtensions?displayProperty=fullName>
- <xref:Windows.Foundation.Point?displayProperty=fullName>
- <xref:Windows.Foundation.Size?displayProperty=fullName>
- <xref:Windows.UI.Color?displayProperty=fullName>

<!--

### Affected APIs

- `T:System.IO.WindowsRuntimeStorageExtensions`
- `T: System.IO.WindowsRuntimeStreamExtensions`
- `N:System.Runtime.InteropServices.WindowsRuntime`
- `T:System.WindowsRuntimeSystemExtensions`
- `T:Windows.Foundation.Point`
- `T:Windows.Foundation.Size`
- `T:Windows.UI.Color`

### Category

Interop

-->
