---
title: 中断性变更：不在非 Windows 平台上探测 A/W 后缀
description: 了解 .NET 5 中的互操作中断性变更：在非 Windows 平台上探测 P/Invoke 期间，不再将后缀添加到函数导出名称。
ms.date: 08/13/2020
ms.openlocfilehash: 924cbcb6c432e2f7c52c7218d48a938b61306c9c
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256616"
---
# <a name="no-aw-suffix-probing-on-non-windows-platforms"></a><span data-ttu-id="93b59-103">不在非 Windows 平台上探测 A/W 后缀</span><span class="sxs-lookup"><span data-stu-id="93b59-103">No A/W suffix probing on non-Windows platforms</span></span>

<span data-ttu-id="93b59-104">在非 Windows 平台上探测 P/Invoke 期间，.NET 运行时不再向函数导出名称添加 `A` 或 `W` 后缀。</span><span class="sxs-lookup"><span data-stu-id="93b59-104">The .NET runtimes no longer add an `A` or `W` suffix to function export names during probing for P/Invokes on non-Windows platforms.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="93b59-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="93b59-105">Version introduced</span></span>

<span data-ttu-id="93b59-106">5.0</span><span class="sxs-lookup"><span data-stu-id="93b59-106">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="93b59-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="93b59-107">Change description</span></span>

<span data-ttu-id="93b59-108">[Windows 有一项约定](/windows/win32/intl/conventions-for-function-prototypes)，即要求向 Windows SDK 函数名称添加 `A` 或 `W` 后缀，这分别与 Windows 代码页面和 Unicode 版本相对应。</span><span class="sxs-lookup"><span data-stu-id="93b59-108">[Windows has a convention](/windows/win32/intl/conventions-for-function-prototypes) of adding an `A` or `W` suffix to Windows SDK function names, which correspond to the Windows code page and Unicode versions, respectively.</span></span>

<span data-ttu-id="93b59-109">对于之前版本的 .NET，在导出对 P/Invoke 的发现结果的过程中，CoreCLR 和 Mono 运行时在所有平台上都会向导出名称添加 `A` 或 `W` 后缀。</span><span class="sxs-lookup"><span data-stu-id="93b59-109">In previous versions of .NET, both the CoreCLR and Mono runtimes add an `A` or `W` suffix to the export name during export discovery for P/Invokes *on all platforms*.</span></span>

<span data-ttu-id="93b59-110">而对于 .NET 5 及更高版本，在导出发现过程中，仅在 Windows 上向导出名称添加 `A` 或 `W` 后缀。</span><span class="sxs-lookup"><span data-stu-id="93b59-110">In .NET 5 and later versions, an `A` or `W` suffix is added to the export name during export discovery *on Windows only*.</span></span> <span data-ttu-id="93b59-111">在 Unix 平台上，不会添加后缀。</span><span class="sxs-lookup"><span data-stu-id="93b59-111">On Unix platforms, the suffix is not added.</span></span> <span data-ttu-id="93b59-112">在 Windows 平台上，这两种运行时的语义保持不变。</span><span class="sxs-lookup"><span data-stu-id="93b59-112">The semantics of both runtimes on the Windows platform remain unchanged.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="93b59-113">更改原因</span><span class="sxs-lookup"><span data-stu-id="93b59-113">Reason for change</span></span>

<span data-ttu-id="93b59-114">此项更改旨在简化跨平台探测。</span><span class="sxs-lookup"><span data-stu-id="93b59-114">This change was made to simplify cross-platform probing.</span></span> <span data-ttu-id="93b59-115">如果非 Windows 平台不包含此语义，则不得产生此项开销。</span><span class="sxs-lookup"><span data-stu-id="93b59-115">It's overhead that shouldn't be incurred, given that non-Windows platforms don't contain this semantic.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="93b59-116">建议操作</span><span class="sxs-lookup"><span data-stu-id="93b59-116">Recommended action</span></span>

<span data-ttu-id="93b59-117">若要缓解此更改的影响，可在非 Windows 平台上手动添加所需的后缀。</span><span class="sxs-lookup"><span data-stu-id="93b59-117">To mitigate the change, you can manually add the desired suffix on non-Windows platforms.</span></span> <span data-ttu-id="93b59-118">例如：</span><span class="sxs-lookup"><span data-stu-id="93b59-118">For example:</span></span>

```csharp
[DllImport(...)]
extern static void SetWindowTextW();
```

## <a name="affected-apis"></a><span data-ttu-id="93b59-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="93b59-119">Affected APIs</span></span>

- <xref:System.Runtime.InteropServices.DllImportAttribute?displayProperty=fullName>

<!--

### Affected APIs

- `T:System.Runtime.InteropServices.DllImportAttribute`

### Category

Interop

-->
