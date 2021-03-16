---
title: 中断性变更：面向 .NET 5 时，未定义 NETCOREAPP3_1 预处理器符号
description: 了解 .NET 5 中的中断性变更：项目不再为早期版本定义预处理器符号。
ms.date: 09/17/2020
ms.openlocfilehash: 390c8f5af97510db4652f3f42db577e6de158020
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256525"
---
# <a name="netcoreapp3_1-preprocessor-symbol-is-not-defined-when-targeting-net-5"></a><span data-ttu-id="29568-103">面向 .NET 5 时，未定义 NETCOREAPP3_1 预处理器符号</span><span class="sxs-lookup"><span data-stu-id="29568-103">NETCOREAPP3_1 preprocessor symbol is not defined when targeting .NET 5</span></span>

<span data-ttu-id="29568-104">在 .NET 5 RC2 及更高版本中，项目不再为早期版本（而仅为其目标版本）定义预处理器符号。</span><span class="sxs-lookup"><span data-stu-id="29568-104">In .NET 5 RC2 and later versions, projects no longer define preprocessor symbols for earlier versions, but only for the version that they target.</span></span> <span data-ttu-id="29568-105">这与 .NET Core 1.0 - 3.1 的行为相同。</span><span class="sxs-lookup"><span data-stu-id="29568-105">This is the same behavior as .NET Core 1.0 - 3.1.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="29568-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="29568-106">Version introduced</span></span>

<span data-ttu-id="29568-107">5.0 RC2</span><span class="sxs-lookup"><span data-stu-id="29568-107">5.0 RC2</span></span>

## <a name="change-description"></a><span data-ttu-id="29568-108">更改描述</span><span class="sxs-lookup"><span data-stu-id="29568-108">Change description</span></span>

<span data-ttu-id="29568-109">在 .NET 5 预览版 7 至 RC1 中，面向 `net5.0` 的项目会定义 `NETCOREAPP3_1` 和 `NET5_0` 预处理器符号。</span><span class="sxs-lookup"><span data-stu-id="29568-109">In .NET 5 preview 7 through RC1, projects that target `net5.0` define both `NETCOREAPP3_1` and `NET5_0` preprocessor symbols.</span></span> <span data-ttu-id="29568-110">此行为更改的目的在于，从 .NET 5 开始，条件编译[符号将会累积](https://github.com/dotnet/designs/blob/main/accepted/2020/net5/net5.md#preprocessor-symbols)。</span><span class="sxs-lookup"><span data-stu-id="29568-110">The intent behind this behavior change was that starting with .NET 5, conditional compilation [symbols would be cumulative](https://github.com/dotnet/designs/blob/main/accepted/2020/net5/net5.md#preprocessor-symbols).</span></span>

<span data-ttu-id="29568-111">在 .NET 5 RC2 及更高版本中，项目仅为其面向的目标框架名字对象 (TFM) 定义符号，而不为任何早期版本进行定义。</span><span class="sxs-lookup"><span data-stu-id="29568-111">In .NET 5 RC2 and later, projects only define symbols for the target framework monikers (TFM) that it targets and not for any earlier versions.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="29568-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="29568-112">Reason for change</span></span>

<span data-ttu-id="29568-113">由于客户反馈，已从预览版 7 开始恢复该更改。</span><span class="sxs-lookup"><span data-stu-id="29568-113">The change from preview 7 was reverted due to customer feedback.</span></span> <span data-ttu-id="29568-114">为较早版本的客户定义符号令客户惊讶和困惑，还有人以为这是 C# 编译器中的 bug。</span><span class="sxs-lookup"><span data-stu-id="29568-114">Defining symbols for earlier versions surprised and confused customers, and some assumed it was a bug in the C# compiler.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="29568-115">建议的操作</span><span class="sxs-lookup"><span data-stu-id="29568-115">Recommended action</span></span>

<span data-ttu-id="29568-116">请确保你的 `#if` 逻辑在项目面向 `net5.0` 或更高版本时不假定已定义 `NETCOREAPP3_1`。</span><span class="sxs-lookup"><span data-stu-id="29568-116">Make sure that your `#if` logic doesn't assume that `NETCOREAPP3_1` is defined when the project targets `net5.0` or higher.</span></span> <span data-ttu-id="29568-117">相反，仅在项目明确面向 `netcoreapp3.1` 时才假定已定义 `NETCOREAPP3_1`。</span><span class="sxs-lookup"><span data-stu-id="29568-117">Instead, assume that `NETCOREAPP3_1` is only defined when the project explicitly targets `netcoreapp3.1`.</span></span>

<span data-ttu-id="29568-118">例如，如果你的项目同时以 .NET Core 2.1 和 .NET Core 3.1 为目标，并调用 .NET Core 3.1 中引入的 API，则 `#if` 逻辑应如下所示：</span><span class="sxs-lookup"><span data-stu-id="29568-118">For example, if your project multitargets for .NET Core 2.1 and .NET Core 3.1 and you call APIs that were introduced in .NET Core 3.1, your `#if` logic should look as follows:</span></span>

```csharp
#if NETCOREAPP2_1 || NETCOREAPP3_0
    // Fallback behavior for old versions.
#elif NETCOREAPP
    // Behavior for .NET Core 3.1 and later.
#endif
```

## <a name="affected-apis"></a><span data-ttu-id="29568-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="29568-119">Affected APIs</span></span>

<span data-ttu-id="29568-120">不可用</span><span class="sxs-lookup"><span data-stu-id="29568-120">N/A</span></span>

<!--

### Affected APIs

Not detectable via API analysis.

### Category

MSBuild

-->
