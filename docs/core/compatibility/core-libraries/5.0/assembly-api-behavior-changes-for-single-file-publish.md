---
title: 中断性变更：适用于单文件发布格式的与程序集相关 API 行为更改
description: 了解核心 .NET 库中的 .NET 5 中断性变更：与程序集的文件位置相关的多个 API 在以单文件发布格式调用时行为发生更改。
ms.date: 11/01/2020
ms.openlocfilehash: 3810a71fac481a42ccf2c8e64149d171f31c6821
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257682"
---
# <a name="assembly-related-api-behavior-changes-for-single-file-publishing-format"></a><span data-ttu-id="c0821-103">适用于单文件发布格式的与程序集相关 API 行为更改</span><span class="sxs-lookup"><span data-stu-id="c0821-103">Assembly-related API behavior changes for single-file publishing format</span></span>

<span data-ttu-id="c0821-104">与程序集的文件位置相关的多个 API 在以单文件发布格式调用时行为发生更改。</span><span class="sxs-lookup"><span data-stu-id="c0821-104">Multiple APIs related to an assembly's file location have behavior changes when they're invoked in a single-file publishing format.</span></span>

## <a name="change-description"></a><span data-ttu-id="c0821-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="c0821-105">Change description</span></span>

<span data-ttu-id="c0821-106">在针对 .NET 5 及更高版本的单文件发布中，从内存中加载捆绑程序集，而不是提取到磁盘。</span><span class="sxs-lookup"><span data-stu-id="c0821-106">In single-file publishing for .NET 5 and later versions, bundled assemblies are loaded from memory instead of extracted to disk.</span></span> <span data-ttu-id="c0821-107">对于单文件发布的应用，这意味着某些与位置相关的 API 在 .NET 5 及更高版本上返回的值与在以前版本的 .NET 上返回的值不同。</span><span class="sxs-lookup"><span data-stu-id="c0821-107">For single-file published apps, this means that certain location-related APIs return different values on .NET 5 and later than on previous versions of .NET.</span></span> <span data-ttu-id="c0821-108">更改如下：</span><span class="sxs-lookup"><span data-stu-id="c0821-108">The changes are as follows:</span></span>

| <span data-ttu-id="c0821-109">API</span><span class="sxs-lookup"><span data-stu-id="c0821-109">API</span></span> | <span data-ttu-id="c0821-110">以前的版本</span><span class="sxs-lookup"><span data-stu-id="c0821-110">Previous versions</span></span> | <span data-ttu-id="c0821-111">.NET 5 及更高版本</span><span class="sxs-lookup"><span data-stu-id="c0821-111">.NET 5 and later</span></span> |
| - | - | - |
| <xref:System.Reflection.Assembly.Location?displayProperty=nameWithType> | <span data-ttu-id="c0821-112">返回提取的 DLL 文件路径</span><span class="sxs-lookup"><span data-stu-id="c0821-112">Returns extracted DLL file path</span></span> | <span data-ttu-id="c0821-113">为捆绑的程序集返回空字符串</span><span class="sxs-lookup"><span data-stu-id="c0821-113">Returns empty string for bundled assemblies</span></span> |
| <xref:System.Reflection.Assembly.CodeBase?displayProperty=nameWithType> | <span data-ttu-id="c0821-114">返回提取的 DLL 文件路径</span><span class="sxs-lookup"><span data-stu-id="c0821-114">Returns extracted DLL file path</span></span> | <span data-ttu-id="c0821-115">引发捆绑的程序集的异常</span><span class="sxs-lookup"><span data-stu-id="c0821-115">Throws exception for bundled assemblies</span></span> |
| <xref:System.Reflection.Assembly.GetFile(System.String)?displayProperty=nameWithType> | <span data-ttu-id="c0821-116">为捆绑的程序集返回 `null`</span><span class="sxs-lookup"><span data-stu-id="c0821-116">Returns `null` for bundled assemblies</span></span> | <span data-ttu-id="c0821-117">引发捆绑的程序集的异常</span><span class="sxs-lookup"><span data-stu-id="c0821-117">Throws exception for bundled assemblies</span></span> |
| `Environment.GetCommandLineArgs()[0]` | <span data-ttu-id="c0821-118">值是入口点 DLL 的名称</span><span class="sxs-lookup"><span data-stu-id="c0821-118">Value is the name of the entry point DLL</span></span> | <span data-ttu-id="c0821-119">值是主机可执行文件的名称</span><span class="sxs-lookup"><span data-stu-id="c0821-119">Value is the name of the host executable</span></span> |
| <xref:System.AppContext.BaseDirectory?displayProperty=nameWithType> | <span data-ttu-id="c0821-120">值是临时提取目录</span><span class="sxs-lookup"><span data-stu-id="c0821-120">Value is the temporary extraction directory</span></span> | <span data-ttu-id="c0821-121">值是主机可执行文件的包含目录</span><span class="sxs-lookup"><span data-stu-id="c0821-121">Value is the containing directory of the host executable</span></span> |

## <a name="version-introduced"></a><span data-ttu-id="c0821-122">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c0821-122">Version introduced</span></span>

<span data-ttu-id="c0821-123">5.0</span><span class="sxs-lookup"><span data-stu-id="c0821-123">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="c0821-124">建议操作</span><span class="sxs-lookup"><span data-stu-id="c0821-124">Recommended action</span></span>

<span data-ttu-id="c0821-125">作为单个文件发布时，避免依赖程序集的文件位置。</span><span class="sxs-lookup"><span data-stu-id="c0821-125">Avoid dependencies on the file location of assemblies when publishing as a single file.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="c0821-126">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="c0821-126">Affected APIs</span></span>

- <xref:System.Reflection.Assembly.Location?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.CodeBase?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.GetFile(System.String)?displayProperty=nameWithType>
- <xref:System.Environment.GetCommandLineArgs?displayProperty=nameWithType>
- <xref:System.AppContext.BaseDirectory?displayProperty=nameWithType>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Reflection.Assembly.Location`
- `P:System.Reflection.Assembly.CodeBase`
- `M:System.Reflection.Assembly.GetFile(System.String)`
- `M:System.Environment.GetCommandLineArgs`
- `P:System.AppContext.BaseDirectory`

-->
