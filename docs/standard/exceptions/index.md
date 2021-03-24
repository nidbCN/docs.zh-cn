---
title: 在 .NET 中处理和引发异常
description: 了解如何在 .NET 中处理和抛出异常。 异常是 .NET 操作向应用程序指示失败的方式。
ms.date: 06/19/2018
helpviewer_keywords:
- exceptions [.NET], handling
- runtime, exceptions
- filtering exceptions
- errors [.NET], exceptions
- exceptions [.NET], throwing
- exceptions [.NET]
- common language runtime, exceptions
ms.assetid: f99a1d29-a2a8-47af-9707-9909f9010735
ms.openlocfilehash: 3b64b9121cc784db7d76ff5501aedfd125d0002c
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876469"
---
# <a name="handling-and-throwing-exceptions-in-net"></a><span data-ttu-id="dbe84-104">在 .NET 中处理和引发异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-104">Handling and throwing exceptions in .NET</span></span>

<span data-ttu-id="dbe84-105">应用程序必须能够以一致的方式处理执行期间发生的错误。</span><span class="sxs-lookup"><span data-stu-id="dbe84-105">Applications must be able to handle errors that occur during execution in a consistent manner.</span></span> <span data-ttu-id="dbe84-106">.NET 提供一种以统一方式向应用程序报错的模型：.NET 操作通过引发异常来指示故障。</span><span class="sxs-lookup"><span data-stu-id="dbe84-106">.NET provides a model for notifying applications of errors in a uniform way: .NET operations indicate failure by throwing exceptions.</span></span>

## <a name="exceptions"></a><span data-ttu-id="dbe84-107">异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-107">Exceptions</span></span>

<span data-ttu-id="dbe84-108">异常是执行程序遇到的所有错误条件或意外行为。</span><span class="sxs-lookup"><span data-stu-id="dbe84-108">An exception is any error condition or unexpected behavior that is encountered by an executing program.</span></span> <span data-ttu-id="dbe84-109">异常可能由你的代码或调用的代码（如共享库）中的错误、不可用的操作系统资源、运行时遇到的意外情况（如无法验证的代码）等引发。</span><span class="sxs-lookup"><span data-stu-id="dbe84-109">Exceptions can be thrown because of a fault in your code or in code that you call (such as a shared library), unavailable operating system resources, unexpected conditions that the runtime encounters (such as code that can't be verified), and so on.</span></span> <span data-ttu-id="dbe84-110">应用程序可从这些情况中的一些中恢复，但无法从其他情况中恢复。</span><span class="sxs-lookup"><span data-stu-id="dbe84-110">Your application can recover from some of these conditions, but not from others.</span></span> <span data-ttu-id="dbe84-111">尽管可以从大多数应用程序异常中恢复，但不能从大多数运行时异常中恢复。</span><span class="sxs-lookup"><span data-stu-id="dbe84-111">Although you can recover from most application exceptions, you can't recover from most runtime exceptions.</span></span>

<span data-ttu-id="dbe84-112">在 .NET中，异常是从 <xref:System.Exception?displayProperty=nameWithType> 类继承的对象。</span><span class="sxs-lookup"><span data-stu-id="dbe84-112">In .NET, an exception is an object that inherits from the <xref:System.Exception?displayProperty=nameWithType> class.</span></span> <span data-ttu-id="dbe84-113">异常引发自发生问题的代码区域。</span><span class="sxs-lookup"><span data-stu-id="dbe84-113">An exception is thrown from an area of code where a problem has occurred.</span></span> <span data-ttu-id="dbe84-114">异常在堆栈中向上传递，直到应用程序对其进行处理或者程序终止。</span><span class="sxs-lookup"><span data-stu-id="dbe84-114">The exception is passed up the stack until the application handles it or the program terminates.</span></span>

## <a name="exceptions-vs-traditional-error-handling-methods"></a><span data-ttu-id="dbe84-115">异常与传统的错误处理方法</span><span class="sxs-lookup"><span data-stu-id="dbe84-115">Exceptions vs. traditional error-handling methods</span></span>

<span data-ttu-id="dbe84-116">传统上，语言的错误处理模型依赖语言检测错误和针对错误查找其处理程序的独特方式，或者依赖操作系统提供的错误处理机制。</span><span class="sxs-lookup"><span data-stu-id="dbe84-116">Traditionally, a language's error-handling model relied on either the language's unique way of detecting errors and locating handlers for them, or on the error-handling mechanism provided by the operating system.</span></span> <span data-ttu-id="dbe84-117">.NET 实现异常处理的方式有以下优点：</span><span class="sxs-lookup"><span data-stu-id="dbe84-117">The way .NET implements exception handling provides the following advantages:</span></span>

- <span data-ttu-id="dbe84-118">引发和处理异常的方式与 .NET 编程语言的相同。</span><span class="sxs-lookup"><span data-stu-id="dbe84-118">Exception throwing and handling works the same for .NET programming languages.</span></span>

- <span data-ttu-id="dbe84-119">处理异常不需要任何特定的语言语法，但允许每种语言定义自己的语法。</span><span class="sxs-lookup"><span data-stu-id="dbe84-119">Doesn't require any particular language syntax for handling exceptions, but allows each language to define its own syntax.</span></span>

- <span data-ttu-id="dbe84-120">可跨进程，甚至跨计算机边界引发异常。</span><span class="sxs-lookup"><span data-stu-id="dbe84-120">Exceptions can be thrown across process and even machine boundaries.</span></span>

- <span data-ttu-id="dbe84-121">可向应用程序添加异常处理代码以提高程序的可靠性。</span><span class="sxs-lookup"><span data-stu-id="dbe84-121">Exception-handling code can be added to an application to increase program reliability.</span></span>

<span data-ttu-id="dbe84-122">异常相较于其他错误通知方法（如返回代码）具有多种优势。</span><span class="sxs-lookup"><span data-stu-id="dbe84-122">Exceptions offer advantages over other methods of error notification, such as return codes.</span></span> <span data-ttu-id="dbe84-123">故障不会被忽略掉，因为如果引发了异常且未得到解决，运行时会终止应用程序。</span><span class="sxs-lookup"><span data-stu-id="dbe84-123">Failures don't go unnoticed because if an exception is thrown and you don't handle it, the runtime terminates your application.</span></span> <span data-ttu-id="dbe84-124">因为代码未能检查出是否存在故障返回代码，所以无效值不会继续在系统中传播。</span><span class="sxs-lookup"><span data-stu-id="dbe84-124">Invalid values don't continue to propagate through the system as a result of code that fails to check for a failure return code.</span></span>

## <a name="common-exceptions"></a><span data-ttu-id="dbe84-125">常见异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-125">Common exceptions</span></span>

<span data-ttu-id="dbe84-126">下表列出了一些常见的异常，以及会引发这些异常的原因的示例。</span><span class="sxs-lookup"><span data-stu-id="dbe84-126">The following table lists some common exceptions with examples of what can cause them.</span></span>

| <span data-ttu-id="dbe84-127">异常类型</span><span class="sxs-lookup"><span data-stu-id="dbe84-127">Exception type</span></span> | <span data-ttu-id="dbe84-128">描述</span><span class="sxs-lookup"><span data-stu-id="dbe84-128">Description</span></span> | <span data-ttu-id="dbe84-129">示例</span><span class="sxs-lookup"><span data-stu-id="dbe84-129">Example</span></span> |
| -------------- | ----------- | ------- |
| <xref:System.Exception> | <span data-ttu-id="dbe84-130">所有异常的基类。</span><span class="sxs-lookup"><span data-stu-id="dbe84-130">Base class for all exceptions.</span></span> | <span data-ttu-id="dbe84-131">无（使用此异常的派生类）。</span><span class="sxs-lookup"><span data-stu-id="dbe84-131">None (use a derived class of this exception).</span></span> |
| <xref:System.IndexOutOfRangeException> | <span data-ttu-id="dbe84-132">仅当错误地对数组进行索引时，才由运行时引发。</span><span class="sxs-lookup"><span data-stu-id="dbe84-132">Thrown by the runtime only when an array is indexed improperly.</span></span> | <span data-ttu-id="dbe84-133">在数组的有效范围外对数组进行索引：</span><span class="sxs-lookup"><span data-stu-id="dbe84-133">Indexing an array outside its valid range:</span></span> <br /> `arr[arr.Length+1]` |
| <xref:System.NullReferenceException> | <span data-ttu-id="dbe84-134">仅当引用 null 对象时，才由运行时引发。</span><span class="sxs-lookup"><span data-stu-id="dbe84-134">Thrown by the runtime only when a null object is referenced.</span></span> | `object o = null;` <br /> `o.ToString();` |
| <xref:System.InvalidOperationException> | <span data-ttu-id="dbe84-135">当处于无效状态时，由方法引发。</span><span class="sxs-lookup"><span data-stu-id="dbe84-135">Thrown by methods when in an invalid state.</span></span> | <span data-ttu-id="dbe84-136">从基础集合删除项后调用 `Enumerator.MoveNext()`。</span><span class="sxs-lookup"><span data-stu-id="dbe84-136">Calling `Enumerator.MoveNext()` after removing an item from the underlying collection.</span></span> |
| <xref:System.ArgumentException> | <span data-ttu-id="dbe84-137">所有自变量异常的基类。</span><span class="sxs-lookup"><span data-stu-id="dbe84-137">Base class for all argument exceptions.</span></span> | <span data-ttu-id="dbe84-138">无（使用此异常的派生类）。</span><span class="sxs-lookup"><span data-stu-id="dbe84-138">None (use a derived class of this exception).</span></span> |
| <xref:System.ArgumentNullException> | <span data-ttu-id="dbe84-139">由不允许参数为 null 的方法引发。</span><span class="sxs-lookup"><span data-stu-id="dbe84-139">Thrown by methods that do not allow an argument to be null.</span></span> | `String s = null;` <br /> `"Calculate".IndexOf(s);`|
| <xref:System.ArgumentOutOfRangeException> | <span data-ttu-id="dbe84-140">由验证自变量是否位于给定范围内的方法引发。</span><span class="sxs-lookup"><span data-stu-id="dbe84-140">Thrown by methods that verify that arguments are in a given range.</span></span> | `String s = "string";` <br /> `s.Substring(s.Length+1);` |

## <a name="see-also"></a><span data-ttu-id="dbe84-141">请参阅</span><span class="sxs-lookup"><span data-stu-id="dbe84-141">See also</span></span>

- [<span data-ttu-id="dbe84-142">异常类和属性</span><span class="sxs-lookup"><span data-stu-id="dbe84-142">Exception Class and Properties</span></span>](exception-class-and-properties.md)
- [<span data-ttu-id="dbe84-143">如何：使用 Try-Catch 块捕获异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-143">How to: Use the Try-Catch Block to Catch Exceptions</span></span>](how-to-use-the-try-catch-block-to-catch-exceptions.md)
- [<span data-ttu-id="dbe84-144">如何：在 Catch 块中使用特定异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-144">How to: Use Specific Exceptions in a Catch Block</span></span>](how-to-use-specific-exceptions-in-a-catch-block.md)
- [<span data-ttu-id="dbe84-145">如何：显式抛出异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-145">How to: Explicitly Throw Exceptions</span></span>](how-to-explicitly-throw-exceptions.md)
- [<span data-ttu-id="dbe84-146">如何：创建用户定义异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-146">How to: Create User-Defined Exceptions</span></span>](how-to-create-user-defined-exceptions.md)
- [<span data-ttu-id="dbe84-147">使用用户筛选的异常处理程序</span><span class="sxs-lookup"><span data-stu-id="dbe84-147">Using User-Filtered Exception Handlers</span></span>](using-user-filtered-exception-handlers.md)
- [<span data-ttu-id="dbe84-148">如何：使用 Finally 块</span><span class="sxs-lookup"><span data-stu-id="dbe84-148">How to: Use Finally Blocks</span></span>](how-to-use-finally-blocks.md)
- [<span data-ttu-id="dbe84-149">处理 COM 互操作异常</span><span class="sxs-lookup"><span data-stu-id="dbe84-149">Handling COM Interop Exceptions</span></span>](handling-com-interop-exceptions.md)
- [<span data-ttu-id="dbe84-150">与异常有关的最佳做法</span><span class="sxs-lookup"><span data-stu-id="dbe84-150">Best Practices for Exceptions</span></span>](best-practices-for-exceptions.md)
- [<span data-ttu-id="dbe84-151">每个开发人员都需要了解的有关运行时异常方面的内容</span><span class="sxs-lookup"><span data-stu-id="dbe84-151">What Every Dev needs to Know About Exceptions in the Runtime</span></span>](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/exceptions.md)
