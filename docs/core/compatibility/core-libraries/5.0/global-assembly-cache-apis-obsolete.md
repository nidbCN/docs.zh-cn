---
title: 中断性变更：全局程序集缓存 API 已过时
description: 了解核心 .NET 库中的 .NET 5 中断性变更：处理 GAC 的 API 要么失败，要么不执行任何操作。
ms.date: 11/01/2020
ms.openlocfilehash: 39c7b092b06754a28723693c0147b7ec3a0b705e
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257474"
---
# <a name="global-assembly-cache-apis-are-obsolete"></a><span data-ttu-id="08010-103">全局程序集缓存 API 已过时</span><span class="sxs-lookup"><span data-stu-id="08010-103">Global assembly cache APIs are obsolete</span></span>

<span data-ttu-id="08010-104">.NET Core 和 .NET 5 及更高版本消除了 .NET Framework 中存在的全局程序集缓存 (GAC) 这一概念。</span><span class="sxs-lookup"><span data-stu-id="08010-104">.NET Core and .NET 5 and later versions eliminate the concept of the global assembly cache (GAC) that was present in .NET Framework.</span></span> <span data-ttu-id="08010-105">因此，所有处理 GAC 的 .NETCore 和 .NET 5+ API 要么失败，要么不执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="08010-105">As such, all .NET Core and .NET 5+ APIs that deal with the GAC either fail or perform no operation.</span></span>

<span data-ttu-id="08010-106">为帮助开发人员摒弃这些 API，一些 GAC 相关的 API 标记为已过时，并在编译时生成 `SYSLIB0005` 警告。</span><span class="sxs-lookup"><span data-stu-id="08010-106">To help steer developers away from these APIs, some GAC-related APIs are marked as obsolete, and generate a `SYSLIB0005` warning at compile time.</span></span> <span data-ttu-id="08010-107">.NET 的未来版本中将删除这些 API。</span><span class="sxs-lookup"><span data-stu-id="08010-107">These APIs may be removed in a future version of .NET.</span></span>

## <a name="change-description"></a><span data-ttu-id="08010-108">更改描述</span><span class="sxs-lookup"><span data-stu-id="08010-108">Change description</span></span>

<span data-ttu-id="08010-109">以下 API 标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="08010-109">The following APIs are marked obsolete.</span></span>

| <span data-ttu-id="08010-110">API</span><span class="sxs-lookup"><span data-stu-id="08010-110">API</span></span> | <span data-ttu-id="08010-111">在以下版本中标记为已过时…</span><span class="sxs-lookup"><span data-stu-id="08010-111">Marked obsolete in...</span></span> |
| - | - |
| <xref:System.Reflection.Assembly.GlobalAssemblyCache?displayProperty=nameWithType> | <span data-ttu-id="08010-112">5.0 RC1</span><span class="sxs-lookup"><span data-stu-id="08010-112">5.0 RC1</span></span> |

<span data-ttu-id="08010-113">在 .NET Framework 2.x - 4.x 中，如果从 GAC 加载了查询的程序集，则 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 属性返回 `true`，如果从磁盘的不同位置加载了该程序集，则返回 `false`。</span><span class="sxs-lookup"><span data-stu-id="08010-113">In .NET Framework 2.x - 4.x, the <xref:System.Reflection.Assembly.GlobalAssemblyCache> property returns `true` if the queried assembly was loaded from the GAC, and `false` if it was loaded from a different location on disk.</span></span> <span data-ttu-id="08010-114">在 .NET Core 2.x - 3.x 中，<xref:System.Reflection.Assembly.GlobalAssemblyCache> 始终返回 `false`，这表明 GAC 在 .NET Core 中不存在。</span><span class="sxs-lookup"><span data-stu-id="08010-114">In .NET Core 2.x - 3.x, the <xref:System.Reflection.Assembly.GlobalAssemblyCache> always returns `false`, reflecting that the GAC does not exist in .NET Core.</span></span>

```csharp
Assembly asm = typeof(object).Assembly;
// Prints 'True' on .NET Framework, 'False' on .NET Core.
Console.WriteLine(asm.GlobalAssemblyCache);
```

<span data-ttu-id="08010-115">在 .NET 5 及更高版本中，<xref:System.Reflection.Assembly.GlobalAssemblyCache> 属性仍始终返回 `false`。</span><span class="sxs-lookup"><span data-stu-id="08010-115">In .NET 5 and later versions, the <xref:System.Reflection.Assembly.GlobalAssemblyCache> property continues to always return `false`.</span></span> <span data-ttu-id="08010-116">然而，属性 Getter 也标记为已过时，以指示调用方应停止访问该属性。</span><span class="sxs-lookup"><span data-stu-id="08010-116">However, the property getter is also marked as obsolete to indicate to callers that they should stop accessing the property.</span></span> <span data-ttu-id="08010-117">库和应用不得使用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> API 来确定运行时行为，因为它在 .NET Core 和 .NET 5 及更高版本中始终返回 `false`。</span><span class="sxs-lookup"><span data-stu-id="08010-117">Libraries and apps should not use the <xref:System.Reflection.Assembly.GlobalAssemblyCache> API to make determinations about run-time behavior, as it always returns `false` in .NET Core and .NET 5 and later versions.</span></span>

```csharp
Assembly asm = typeof(object).Assembly;
// Prints 'False' on .NET 5.0+; also produces warning SYSLIB0005 at compile time.
Console.WriteLine(asm.GlobalAssemblyCache);
```

<span data-ttu-id="08010-118">这是仅在编译时进行的更改。</span><span class="sxs-lookup"><span data-stu-id="08010-118">This is a compile-time only change.</span></span> <span data-ttu-id="08010-119">以前版本的 .NET Core 没有运行时更改。</span><span class="sxs-lookup"><span data-stu-id="08010-119">There is no run-time change from previous versions of .NET Core.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="08010-120">更改原因</span><span class="sxs-lookup"><span data-stu-id="08010-120">Reason for change</span></span>

<span data-ttu-id="08010-121">.NET Core 和 .NET 5 及更高版本中没有全局程序集缓存 (GAC) 这一概念。</span><span class="sxs-lookup"><span data-stu-id="08010-121">The global assembly cache (GAC) does not exist as a concept in .NET Core and .NET 5 and later versions.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="08010-122">引入的版本</span><span class="sxs-lookup"><span data-stu-id="08010-122">Version introduced</span></span>

<span data-ttu-id="08010-123">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="08010-123">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="08010-124">建议操作</span><span class="sxs-lookup"><span data-stu-id="08010-124">Recommended action</span></span>

- <span data-ttu-id="08010-125">如果你的应用程序查询 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 属性，请考虑删除该调用。</span><span class="sxs-lookup"><span data-stu-id="08010-125">If your application queries the <xref:System.Reflection.Assembly.GlobalAssemblyCache> property, consider removing the call.</span></span> <span data-ttu-id="08010-126">如果在运行时使用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 值在“GAC 中的程序集”流与“不在 GAC 中的程序集”流之间进行选择，请重新考虑流对于.NET Core 或 .NET 5.0+ 应用程序是否仍然有意义。</span><span class="sxs-lookup"><span data-stu-id="08010-126">If you use the <xref:System.Reflection.Assembly.GlobalAssemblyCache> value to choose between an "assembly in the GAC"-flow vs. an "assembly not in the GAC"-flow at run time, reconsider whether the flow still makes sense for a .NET Core or .NET 5.0+ application.</span></span>

- <span data-ttu-id="08010-127">如果必须继续使用已过时的 API，可在代码中取消 `SYSLIB0005` 警告。</span><span class="sxs-lookup"><span data-stu-id="08010-127">If you must continue to use the obsolete APIs, you can suppress the `SYSLIB0005` warning in code.</span></span>

  ```csharp
  Assembly asm = typeof(object).Assembly;
  #pragma warning disable SYSLIB0005 // Disable the warning.
  // Prints 'False' on .NET 5.0+.
  Console.WriteLine(asm.GlobalAssemblyCache);
  #pragma warning restore SYSLIB0005 // Re-enable the warning.
  ```

  <span data-ttu-id="08010-128">还可以在项目文件中取消警告，这将对项目中的所有源文件禁用警告。</span><span class="sxs-lookup"><span data-stu-id="08010-128">You can also suppress the warning in your project file, which disables the warning for all source files in the project.</span></span>

  ```xml
  <Project Sdk="Microsoft.NET.Sdk">
    <PropertyGroup>
     <TargetFramework>net5.0</TargetFramework>
     <!-- NoWarn below will suppress SYSLIB0005 project-wide -->
     <NoWarn>$(NoWarn);SYSLIB0005</NoWarn>
    </PropertyGroup>
  </Project>
  ```

  <span data-ttu-id="08010-129">取消 `SYSLIB0005` 仅禁用 <xref:System.Reflection.Assembly.GlobalAssemblyCache> 过时警告。</span><span class="sxs-lookup"><span data-stu-id="08010-129">Suppressing `SYSLIB0005` disables only the <xref:System.Reflection.Assembly.GlobalAssemblyCache> obsoletion warning.</span></span> <span data-ttu-id="08010-130">它不会禁用任何其他警告。</span><span class="sxs-lookup"><span data-stu-id="08010-130">It does not disable any other warnings.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="08010-131">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="08010-131">Affected APIs</span></span>

- <xref:System.Reflection.Assembly.GlobalAssemblyCache?displayProperty=fullName>

<!--

### Category

Core .NET libraries

### Affected APIs

- `P:System.Reflection.Assembly.GlobalAssemblyCache`

-->
