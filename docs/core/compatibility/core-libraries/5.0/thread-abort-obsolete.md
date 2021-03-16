---
title: 中断性变更：Thread.Abort 已过时
description: 了解核心 .NET 库中的 .NET 5 中断性变更：Thread.Abort API 已过时。
ms.date: 11/01/2020
ms.openlocfilehash: 29c688250593801bab9b32b9e787911e561ec000
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102257058"
---
# <a name="threadabort-is-obsolete"></a><span data-ttu-id="25898-103">Thread.Abort 已过时</span><span class="sxs-lookup"><span data-stu-id="25898-103">Thread.Abort is obsolete</span></span>

<span data-ttu-id="25898-104"><xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> API 已过时。</span><span class="sxs-lookup"><span data-stu-id="25898-104">The <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> APIs are obsolete.</span></span> <span data-ttu-id="25898-105">如果调用这些方法，则面向 .NET 5 或更高版本的项目将遇到编译时警告 `SYSLIB0006`。</span><span class="sxs-lookup"><span data-stu-id="25898-105">Projects that target .NET 5 or a later version will encounter compile-time warning `SYSLIB0006` if these methods are called.</span></span>

## <a name="change-description"></a><span data-ttu-id="25898-106">更改说明</span><span class="sxs-lookup"><span data-stu-id="25898-106">Change description</span></span>

<span data-ttu-id="25898-107">以前，对 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 的调用不会生成编译时警告，但该方法确实会在运行时引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="25898-107">Previously, calls to <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> did not produce compile-time warnings, however, the method did throw a <xref:System.PlatformNotSupportedException> at run time.</span></span>

<span data-ttu-id="25898-108">从 .NET 5 开始，<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 以警告的形式标记为已过时。</span><span class="sxs-lookup"><span data-stu-id="25898-108">Starting in .NET 5, <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> is marked obsolete as warning.</span></span> <span data-ttu-id="25898-109">调用此方法将生成编译器警告 `SYSLIB0006`。</span><span class="sxs-lookup"><span data-stu-id="25898-109">Calling this method produces compiler warning `SYSLIB0006`.</span></span> <span data-ttu-id="25898-110">该方法的实现保持不变，并且继续引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="25898-110">The implementation of the method is unchanged, and it continues to throw a <xref:System.PlatformNotSupportedException>.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="25898-111">更改原因</span><span class="sxs-lookup"><span data-stu-id="25898-111">Reason for change</span></span>

<span data-ttu-id="25898-112">假定 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 在除 .NET Framework 以外的所有 .NET 实现中始终引发 <xref:System.PlatformNotSupportedException>，则向该方法添加 <xref:System.ObsoleteAttribute>，以引起人们对其调用位置的注意。</span><span class="sxs-lookup"><span data-stu-id="25898-112">Given that <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> always throws a <xref:System.PlatformNotSupportedException> on all .NET implementations except .NET Framework, <xref:System.ObsoleteAttribute> was added to the method to draw attention to places where it's called.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="25898-113">引入的版本</span><span class="sxs-lookup"><span data-stu-id="25898-113">Version introduced</span></span>

<span data-ttu-id="25898-114">5.0</span><span class="sxs-lookup"><span data-stu-id="25898-114">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="25898-115">建议操作</span><span class="sxs-lookup"><span data-stu-id="25898-115">Recommended action</span></span>

- <span data-ttu-id="25898-116">使用 <xref:System.Threading.CancellationToken> 中止对工作单元的处理，而不是调用 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="25898-116">Use a <xref:System.Threading.CancellationToken> to abort processing of a unit of work instead of calling <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="25898-117">以下示例说明了 <xref:System.Threading.CancellationToken> 的用法。</span><span class="sxs-lookup"><span data-stu-id="25898-117">The following example illustrates the use of <xref:System.Threading.CancellationToken>.</span></span>

  ```csharp
  void ProcessPendingWorkItemsNew(CancellationToken cancellationToken)
  {
      if (QueryIsMoreWorkPending())
      {
          // If the CancellationToken is marked as "needs to cancel",
          // this will throw the appropriate exception.
          cancellationToken.ThrowIfCancellationRequested();

          WorkItem work = DequeueWorkItem();
          ProcessWorkItem(work);
      }
  }
  ```

  <span data-ttu-id="25898-118">有关详细信息，请参阅[托管线程中的取消](../../../../standard/threading/cancellation-in-managed-threads.md)。</span><span class="sxs-lookup"><span data-stu-id="25898-118">For more information, see [Cancellation in managed threads](../../../../standard/threading/cancellation-in-managed-threads.md).</span></span>

- <span data-ttu-id="25898-119">若要禁止显示编译时警告，请禁止显示警告代码 `SYSLIB0006`。</span><span class="sxs-lookup"><span data-stu-id="25898-119">To suppress the compile-time warning, suppress warning code `SYSLIB0006`.</span></span> <span data-ttu-id="25898-120">警告代码特定于 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>，并且禁止显示该代码不会禁止显示代码中的其他过时警告。</span><span class="sxs-lookup"><span data-stu-id="25898-120">The warning code is specific to <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> and suppressing it doesn't suppress other obsoletion warnings in your code.</span></span> <span data-ttu-id="25898-121">但是，建议删除对 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 的调用，而不是禁止显示该警告。</span><span class="sxs-lookup"><span data-stu-id="25898-121">However, we recommend that you remove calls to <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> instead of suppressing the warning.</span></span>

  ```csharp
  void MyMethod()
  {
  #pragma warning disable SYSLIB0006
      Thread.CurrentThread.Abort();
  #pragma warning restore SYSLIB0006
  }
  ```

  <span data-ttu-id="25898-122">另外，还可以在项目文件中取消警告。</span><span class="sxs-lookup"><span data-stu-id="25898-122">You can also suppress the warning in the project file.</span></span>

  ```xml
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0</TargetFramework>
    <!-- Disable "Thread.Abort is obsolete" warnings for entire project. -->
    <NoWarn>$(NoWarn);SYSLIB0006</NoWarn>
  </PropertyGroup>
  ```

## <a name="affected-apis"></a><span data-ttu-id="25898-123">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="25898-123">Affected APIs</span></span>

- <xref:System.Threading.Thread.Abort%2A?displayProperty=fullName>

<!--

#### Category

Core .NET libraries

### Affected APIs

- `Overload:System.Threading.Thread.Abort`

-->
