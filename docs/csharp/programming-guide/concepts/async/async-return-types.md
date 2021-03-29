---
title: 异步返回类型 (C#)
description: 了解在 C# 中异步方法可以具有的返回类型，以及每种类型的代码示例和其他资源。
ms.date: 08/19/2020
ms.assetid: ddb2539c-c898-48c1-ad92-245e4a996df8
ms.openlocfilehash: 53eb3bedebb99cd829101eee4c2e190c0fb952bf
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653447"
---
# <a name="async-return-types-c"></a>异步返回类型 (C#)

异步方法可以具有以下返回类型：

- <xref:System.Threading.Tasks.Task>（对于执行操作但不返回任何值的异步方法）。
- <xref:System.Threading.Tasks.Task%601>（对于返回值的异步方法）。
- `void`（对于事件处理程序）。
- 从 C# 7.0 开始，任何具有可访问的 `GetAwaiter` 方法的类型。 `GetAwaiter` 方法返回的对象必须实现 <xref:System.Runtime.CompilerServices.ICriticalNotifyCompletion?displayProperty=nameWithType> 接口。
- 从 C# 8.0 开始，<xref:System.Collections.Generic.IAsyncEnumerable%601> 返回异步流的异步方法  。

有关异步方法的详细信息，请参阅[使用 Async 和 Await 的异步编程 (C#)](./index.md)。

还存在特定于 Windows 工作负载的其他几种类型：

- <xref:System.Windows.Threading.DispatcherOperation>，适用于仅限于 Windows 的异步操作。
- <xref:Windows.Foundation.IAsyncAction>，适用于 UWP 中不返回值的异步操作。
- <xref:Windows.Foundation.IAsyncActionWithProgress%601>，适用于 UWP 中只报告进程但不返回值的异步操作。
- <xref:Windows.Foundation.IAsyncOperation%601>，适用于 UWP 中返回值的异步操作。
- <xref:Windows.Foundation.IAsyncOperationWithProgress%602>，适用于 UWP 中既报告进程又返回值的异步操作。

## <a name="task-return-type"></a>Task 返回类型

不包含 `return` 语句的异步方法或包含不返回操作数的 `return` 语句的异步方法通常具有返回类型 <xref:System.Threading.Tasks.Task>。 如果此类方法同步运行，它们将返回 `void`。 如果在异步方法中使用 <xref:System.Threading.Tasks.Task> 返回类型，调用方法可以使用 `await` 运算符暂停调用方的完成，直至被调用的异步方法结束。

下例中的 `WaitAndApologizeAsync` 方法不包含 `return` 语句，因此该方法会返回 <xref:System.Threading.Tasks.Task> 对象。 返回 `Task` 可等待 `WaitAndApologizeAsync`。 <xref:System.Threading.Tasks.Task> 类型不包含 `Result` 属性，因为它不具有任何返回值。

:::code language="csharp" source="snippets/async-return-types/async-returns2.cs" id="TaskReturn":::

通过使用 await 语句而不是 await 表达式等待 `WaitAndApologizeAsync`，类似于返回 void 的同步方法的调用语句。 Await 运算符的应用程序在这种情况下不生成值。 当 `await` 的右操作数是 <xref:System.Threading.Tasks.Task%601> 时，`await` 表达式生成的结果为 `T`。 当 `await` 的右操作数是 <xref:System.Threading.Tasks.Task> 时，`await` 及其操作数是一个语句。

可从 await 运算符的应用程序中分离对 `WaitAndApologizeAsync` 的调用，如以下代码所示。 但是，请记住，`Task` 没有 `Result` 属性，并且当 await 运算符应用于 `Task` 时不产生值。

以下代码将调用 `WaitAndApologizeAsync` 方法和等待此方法返回的任务分离。

:::code language="csharp" source="snippets/async-return-types/async-returns2a.cs" id="AwaitTask":::

## <a name="tasktresult-return-type"></a>Task\<TResult\> 返回类型

<xref:System.Threading.Tasks.Task%601> 返回类型用于某种异步方法，此异步方法包含 [return](../../../language-reference/keywords/return.md) 语句，其中操作数是 `TResult`。

在下面的示例中，`GetLeisureHoursAsync` 方法包含返回整数的 `return` 语句。 因此，该方法声明必须指定 `Task<int>` 的返回类型。 <xref:System.Threading.Tasks.Task.FromResult%2A> 异步方法是返回 <xref:System.DateTime.DayOfWeek> 的操作的占位符。

:::code language="csharp" source="snippets/async-return-types/async-returns1.cs" id="LeisureHours":::

在 `ShowTodaysInfo` 方法中从 await 表达式内调用 `GetLeisureHoursAsync` 时，await 表达式检索存储在由 `GetLeisureHours` 方法返回的任务中的整数值（`leisureHours` 的值）。 有关 await 表达式的详细信息，请参阅 [await](../../../language-reference/operators/await.md)。

通过从应用程序 `await` 中分离对 `GetLeisureHoursAsync` 的调用，可以更好地理解 `await` 如何从 `Task<T>` 检索结果，如以下代码所示。 对非立即等待的方法 `GetLeisureHoursAsync` 的调用返回 `Task<int>`，正如你从方法声明预料的一样。 该任务指派给示例中的 `getLeisureHoursTask` 变量。 因为 `getLeisureHoursTask` 是 <xref:System.Threading.Tasks.Task%601>，所以它包含类型 `TResult` 的 <xref:System.Threading.Tasks.Task%601.Result> 属性。 在这种情况下，`TResult` 表示整数类型。 `await` 应用于 `getLeisureHoursTask`，await 表达式的计算结果为 `getLeisureHoursTask` 的 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性内容。 此值分配给 `ret` 变量。

> [!IMPORTANT]
> <xref:System.Threading.Tasks.Task%601.Result%2A> 属性为阻止属性。 如果你在其任务完成之前尝试访问它，当前处于活动状态的线程将被阻止，直到任务完成且值为可用。 在大多数情况下，应通过使用 `await` 访问此值，而不是直接访问属性。
>
> 上一示例通过检索 <xref:System.Threading.Tasks.Task%601.Result%2A> 属性的值来阻止主线程，从而使 `Main` 方法可在应用程序结束之前将 `message` 输出到控制台。

:::code language="csharp" source="snippets/async-return-types/async-returns1a.cs" id="StoreTask":::

## <a name="void-return-type"></a>Void 返回类型

在异步事件处理程序中使用 `void` 返回类型，这需要 `void` 返回类型。 对于事件处理程序以外的不返回值的方法，应返回 <xref:System.Threading.Tasks.Task>，因为无法等待返回 `void` 的异步方法。 此类方法的任何调用方都必须继续完成，而无需等待调用的异步方法完成。 调用方必须独立于异步方法生成的任何值或异常。

返回 void 的异步方法的调用方无法捕获从该方法引发的异常，且此类未经处理的异常可能会导致应用程序故障。 如果返回 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601> 的方法引发异常，则该异常存储在返回的任务中。 等待任务时，将重新引发异常。 因此，请确保可以产生异常的任何异步方法都具有返回类型 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.Task%601>，并确保会等待对方法的调用。

有关如何在异步方法中捕获异常的详细信息，请参阅 [try-catch](../../../language-reference/keywords/try-catch.md) 文中的[异步方法中的异常](../../../language-reference/keywords/try-catch.md#exceptions-in-async-methods)部分。

以下示例演示异步事件处理程序的行为。 在本示例代码中，异步事件处理程序必须在完成时通知主线程。 然后，主线程可在退出程序之前等待异步事件处理程序完成。

:::code language="csharp" source="snippets/async-return-types/async-returns3.cs":::

## <a name="generalized-async-return-types-and-valuetasktresult"></a>通用的异步返回类型和 ValueTask\<TResult\>

从 C# 7.0 起，异步方法可以返回具有返回 awaiter 类型实例的可访问 `GetAwaiter` 方法的所有类型。 此外，`GetAwaiter` 方法返回的类型必须具有 <xref:System.Runtime.CompilerServices.AsyncMethodBuilderAttribute?displayProperty=nameWithType> 特性。 有关详细信息，请参阅[类似任务的返回类型](../../../../../_csharplang/proposals/csharp-7.0/task-types.md)的功能说明。

此功能与 [awaitable 表达式](../../../../../_csharplang/spec/expressions.md#awaitable-expressions)相辅相成，后者描述 `await` 操作数的要求。 编译器可以使用通用异步返回类型生成返回不同类型的 `async` 方法。 通用异步返回类型通过 .NET 库实现性能改进。 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 是引用类型，因此，性能关键路径中的内存分配会对性能产生负面影响，尤其当分配出现在紧凑循环中时。 支持通用返回类型意味着可返回轻量值类型（而不是引用类型），从而避免额外的内存分配。

.NET 提供 <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType> 结构作为返回任务的通用值的轻量实现。 要使用 <xref:System.Threading.Tasks.ValueTask%601?displayProperty=nameWithType> 类型，必须向项目添加 `System.Threading.Tasks.Extensions` NuGet 包。 如下示例使用 <xref:System.Threading.Tasks.ValueTask%601> 结构检索两个骰子的值。

:::code language="csharp" source="snippets/async-return-types/async-valuetask.cs":::

编写通用异步返回类型是一种高级方案，旨在用于非常特定的环境中。 请考虑改用 `Task`、`Task<T>` 和 `ValueTask<T>` 类型，它们覆盖了大多数的异步代码方案。

## <a name="async-streams-with-iasyncenumerablet"></a>使用 IAsyncEnumerable\<T\> 的异步流

从 C# 8.0 开始，异步方法可能返回异步流，由 <xref:System.Collections.Generic.IAsyncEnumerable%601> 表示  。 异步流提供了一种方法，来枚举在具有重复异步调用的块中生成元素时从流中读取的项。 以下示例显示生成异步流的异步方法：

:::code language="csharp" source="snippets/async-return-types/AsyncStreams.cs" id="GenerateAsyncStream":::

前面的示例异步读取字符串中的行。 读取每一行后，代码将枚举字符串中的每个单词。 调用方将使用 `await foreach` 语句枚举每个单词。 当需要从源字符串异步读取下一行时，该方法将等待。

## <a name="see-also"></a>请参阅

- <xref:System.Threading.Tasks.Task.FromResult%2A>
- [在异步任务完成时对其进行处理](start-multiple-async-tasks-and-process-them-as-they-complete.md)
- [使用 Async 和 Await 的异步编程 (C#)](index.md)
- [async](../../../language-reference/keywords/async.md)
- [await](../../../language-reference/operators/await.md)
