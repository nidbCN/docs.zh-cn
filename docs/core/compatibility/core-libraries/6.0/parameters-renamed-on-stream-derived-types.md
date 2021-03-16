---
title: 中断性变更：在流派生类型中重命名了参数
description: 了解核心 .NET 库中的 .NET 6.0 中断性变更：流派生类型的方法中的一些参数已发生更改。
ms.date: 03/04/2021
ms.openlocfilehash: c685fae6a7ed20ea47815d5f89a4e066c75e1178
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102260000"
---
# <a name="some-parameters-in-stream-derived-types-are-renamed"></a><span data-ttu-id="ad20a-103">流派生类型中的一些参数已重命名</span><span class="sxs-lookup"><span data-stu-id="ad20a-103">Some parameters in Stream-derived types are renamed</span></span>

<span data-ttu-id="ad20a-104">在 .NET 6 中，派生自 <xref:System.IO.Stream?displayProperty=fullName> 的类型上的方法的某些参数已重命名以匹配基类。</span><span class="sxs-lookup"><span data-stu-id="ad20a-104">In .NET 6, some parameters of methods on types derived from <xref:System.IO.Stream?displayProperty=fullName> have been renamed to match the base class.</span></span>

## <a name="change-description"></a><span data-ttu-id="ad20a-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="ad20a-105">Change description</span></span>

<span data-ttu-id="ad20a-106">在之前的 .NET 版本中，一些派生自 <xref:System.IO.Stream> 的类型会替代方法，但使用与基类使用的名称不同的参数名称。</span><span class="sxs-lookup"><span data-stu-id="ad20a-106">In previous .NET versions, several types derived from <xref:System.IO.Stream> override methods but use different parameter names than those used by the base type.</span></span> <span data-ttu-id="ad20a-107">例如，<xref:System.IO.Compression.DeflateStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=nameWithType> 的字节数组参数命名为 `array`，而基类方法中相应的自变量被命名为 `buffer`。</span><span class="sxs-lookup"><span data-stu-id="ad20a-107">For example, the byte array parameter of <xref:System.IO.Compression.DeflateStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=nameWithType> is named `array` while the corresponding argument in the base class method is named `buffer`.</span></span>

<span data-ttu-id="ad20a-108">在 .NET 6 中，所有派生自 <xref:System.IO.Stream?displayProperty=fullName> 但其参数命名不匹配的类型都已通过使用与基类相同的参数名称，变得与基类保持一致。</span><span class="sxs-lookup"><span data-stu-id="ad20a-108">In .NET 6, all types that derive from <xref:System.IO.Stream?displayProperty=fullName> that had mismatched parameter names have been brought into conformance with the base type by using the same parameter names as the base type.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="ad20a-109">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ad20a-109">Version introduced</span></span>

<span data-ttu-id="ad20a-110">6.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="ad20a-110">6.0 Preview 1</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="ad20a-111">更改原因</span><span class="sxs-lookup"><span data-stu-id="ad20a-111">Reason for change</span></span>

<span data-ttu-id="ad20a-112">进行此更改的几项原因如下所示：</span><span class="sxs-lookup"><span data-stu-id="ad20a-112">There are several reasons for the change:</span></span>

- <span data-ttu-id="ad20a-113">如果传递的自变量无效且引发了异常，则该异常可能包含基本参数的名称或所派生的参数的名称，具体取决于实现。</span><span class="sxs-lookup"><span data-stu-id="ad20a-113">If an invalid argument was passed and an exception was thrown, that exception might have contained the base parameter's name or the derived parameter's name, depending on the implementation.</span></span> <span data-ttu-id="ad20a-114">调用方可能已在使用类型为基类或派生类型的引用，因此异常中的自变量名称没法始终正确。</span><span class="sxs-lookup"><span data-stu-id="ad20a-114">Since the caller may have been using a reference typed as the base or as the derived type, it's impossible for the argument name in the exception to always be correct.</span></span>
- <span data-ttu-id="ad20a-115">具有不同的参数名称使得更难在所有 <xref:System.IO.Stream> 实现中一致地验证行为。</span><span class="sxs-lookup"><span data-stu-id="ad20a-115">Having different parameter names makes it harder to consistently validate behavior across all <xref:System.IO.Stream> implementations.</span></span>
- <span data-ttu-id="ad20a-116">.NET 6 在 <xref:System.IO.Stream> 上添加了一个公共方法用于验证自变量，而需要具有一致的参数名称才能使用该方法。</span><span class="sxs-lookup"><span data-stu-id="ad20a-116">.NET 6 adds a public method on <xref:System.IO.Stream> for validating arguments, and that methods needs to have a consistent parameter name to use.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="ad20a-117">建议的操作</span><span class="sxs-lookup"><span data-stu-id="ad20a-117">Recommended action</span></span>

<span data-ttu-id="ad20a-118">此中断性变更带来的影响微乎其微：</span><span class="sxs-lookup"><span data-stu-id="ad20a-118">The effect of this breaking change is minimal:</span></span>

- <span data-ttu-id="ad20a-119">对于现有的二进制文件，它仅影响使用反射来检查受影响的派生类型上的参数名称的代码。</span><span class="sxs-lookup"><span data-stu-id="ad20a-119">For existing binaries, its impact is limited to code that uses reflection to examine the names of parameters on the affected derived types.</span></span>
- <span data-ttu-id="ad20a-120">对于源代码，它仅影响使用命名参数在派生的流类型上通过具有该派生类型的变量调用方法的代码。</span><span class="sxs-lookup"><span data-stu-id="ad20a-120">For source code, its impact is limited to code that uses named parameters to invoke methods on the derived stream type using a variable typed as that derived type.</span></span>

<span data-ttu-id="ad20a-121">在这两种情况下，都建议一致地使用基本参数名称。</span><span class="sxs-lookup"><span data-stu-id="ad20a-121">In both cases, the recommended action is to consistently use the base parameter name.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="ad20a-122">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ad20a-122">Affected APIs</span></span>

- <xref:System.IO.BufferedStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.BufferedStream.Write(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.Compression.DeflateStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.IO.Compression.DeflateStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.Compression.DeflateStream.ReadAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.Compression.DeflateStream.Write(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.Compression.DeflateStream.WriteAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.Compression.GZipStream.BeginRead(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.IO.Compression.GZipStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.IO.Compression.GZipStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.Compression.GZipStream.ReadAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.Compression.GZipStream.Write(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.Compression.GZipStream.WriteAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.FileStream.BeginRead(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.IO.FileStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.IO.FileStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.IO.FileStream.Write(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.Net.Sockets.NetworkStream.BeginRead(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.Net.Sockets.NetworkStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)?displayProperty=fullName>
- <xref:System.Net.Sockets.NetworkStream.Read(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.Net.Sockets.NetworkStream.ReadAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.Net.Sockets.NetworkStream.Write(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>
- <xref:System.Net.Sockets.NetworkStream.WriteAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `M:System.IO.Compression.DeflateStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.IO.Compression.DeflateStream.Read(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.Compression.DeflateStream.ReadAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)`
- `M:System.IO.Compression.DeflateStream.Write(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.Compression.DeflateStream.WriteAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)`
- `M:System.IO.Compression.GZipStream.BeginRead(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.IO.Compression.GZipStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.IO.Compression.GZipStream.Read(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.Compression.GZipStream.ReadAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)`
- `M:System.IO.Compression.GZipStream.Write(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.Compression.GZipStream.WriteAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)`
- `M:System.IO.BufferedStream.Read(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.BufferedStream.Write(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.FileStream.BeginRead(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.IO.FileStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.IO.FileStream.Read(System.Byte[],System.Int32,System.Int32)`
- `M:System.IO.FileStream.Write(System.Byte[],System.Int32,System.Int32)`
- `M:System.Net.Sockets.NetworkStream.BeginRead(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.Net.Sockets.NetworkStream.BeginWrite(System.Byte[],System.Int32,System.Int32,System.AsyncCallback,System.Object)`
- `M:System.Net.Sockets.NetworkStream.Read(System.Byte[],System.Int32,System.Int32)`
- `M:System.Net.Sockets.NetworkStream.ReadAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)`
- `M:System.Net.Sockets.NetworkStream.Write(System.Byte[],System.Int32,System.Int32)`
- `M:System.Net.Sockets.NetworkStream.WriteAsync(System.Byte[],System.Int32,System.Int32,System.Threading.CancellationToken)`

-->
