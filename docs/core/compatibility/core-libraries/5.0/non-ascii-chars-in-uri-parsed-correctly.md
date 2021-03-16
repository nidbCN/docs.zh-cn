---
title: 中断性变更：在 Unix 上正确分析包含非 ASCII 字符的 URI 路径
description: 了解核心 .NET 库中的 .NET 5 中断性变更：包含非 ASCII 字符的绝对 URI 路径现可在 Unix 平台上正确分析。
ms.date: 11/01/2020
ms.openlocfilehash: 50e9b4597a5ac0b73732a38ce662037292777a03
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257409"
---
# <a name="uri-paths-with-non-ascii-characters-parse-correctly-on-unix"></a><span data-ttu-id="24ed3-103">在 Unix 上正确分析包含非 ASCII 字符的 URI 路径</span><span class="sxs-lookup"><span data-stu-id="24ed3-103">URI paths with non-ASCII characters parse correctly on Unix</span></span>

<span data-ttu-id="24ed3-104">在 <xref:System.Uri?displayProperty=fullName> 类中修复了一个 bug，包含非 ASCII 字符的绝对 URI 路径现在可以在 Unix 平台上正确分析。</span><span class="sxs-lookup"><span data-stu-id="24ed3-104">A bug was fixed in the <xref:System.Uri?displayProperty=fullName> class such that absolute URI paths that contain non-ASCII characters now parse correctly on Unix platforms.</span></span>

## <a name="change-description"></a><span data-ttu-id="24ed3-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="24ed3-105">Change description</span></span>

<span data-ttu-id="24ed3-106">在早期版本的 .NET 中，不能在 Unix 平台上正确分析包含非 ASCII 字符的绝对 URI 路径，并且会复制该路径的段。</span><span class="sxs-lookup"><span data-stu-id="24ed3-106">In previous versions of .NET, absolute URI paths that contain non-ASCII characters are parsed incorrectly on Unix platforms, and segments of the path are duplicated.</span></span> <span data-ttu-id="24ed3-107">（绝对路径是以“/”开头的路径。）.NET 5.0 的分析问题已得以修复。</span><span class="sxs-lookup"><span data-stu-id="24ed3-107">(Absolute paths are those that start with "/".) The parsing issue has been fixed for .NET 5.0.</span></span> <span data-ttu-id="24ed3-108">如果从早期版本的 .NET 迁移到 .NET 5 或更高版本，则会获得由 <xref:System.Uri.AbsoluteUri?displayProperty=nameWithType>、<xref:System.Uri.ToString?displayProperty=nameWithType> 和其他 <xref:System.Uri> 成员生成的不同的值。</span><span class="sxs-lookup"><span data-stu-id="24ed3-108">If you move from a previous version of .NET to .NET 5 or later, you'll get different values produced by <xref:System.Uri.AbsoluteUri?displayProperty=nameWithType>, <xref:System.Uri.ToString?displayProperty=nameWithType>, and other <xref:System.Uri> members.</span></span>

<span data-ttu-id="24ed3-109">在 Unix 上运行时，请考虑以下代码的输出。</span><span class="sxs-lookup"><span data-stu-id="24ed3-109">Consider the output of the following code when run on Unix.</span></span>

```csharp
var myUri = new Uri("/üri");

Console.WriteLine($"AbsoluteUri: {myUri.AbsoluteUri}");
Console.WriteLine($"ToString: {myUri.ToString()}");
```

<span data-ttu-id="24ed3-110">先前 .NET 版本的输出：</span><span class="sxs-lookup"><span data-stu-id="24ed3-110">Output on previous .NET version:</span></span>

```text
AbsoluteUri: /%C3%BCri/%C3%BCri
ToString: /üri/üri
```

<span data-ttu-id="24ed3-111">.NET 5 或更高版本的输出：</span><span class="sxs-lookup"><span data-stu-id="24ed3-111">Output on .NET 5 or later version:</span></span>

```text
AbsoluteUri: /%C3%BCri
ToString: /üri
```

## <a name="version-introduced"></a><span data-ttu-id="24ed3-112">引入的版本</span><span class="sxs-lookup"><span data-stu-id="24ed3-112">Version introduced</span></span>

<span data-ttu-id="24ed3-113">5.0</span><span class="sxs-lookup"><span data-stu-id="24ed3-113">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="24ed3-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="24ed3-114">Recommended action</span></span>

<span data-ttu-id="24ed3-115">如果你具有需要和说明重复路径段的代码，则可以删除该代码。</span><span class="sxs-lookup"><span data-stu-id="24ed3-115">If you have code that expects and accounts for the duplicated path segments, you can remove that code.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="24ed3-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="24ed3-116">Affected APIs</span></span>

- <xref:System.Uri?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `T:System.Uri`

-->
