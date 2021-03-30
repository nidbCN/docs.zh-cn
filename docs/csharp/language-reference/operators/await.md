---
description: await 运算符 - C# 参考
title: await 运算符 - C# 参考
ms.date: 07/13/2020
f1_keywords:
- await_CSharpKeyword
helpviewer_keywords:
- await keyword [C#]
- await [C#]
ms.assetid: 50725c24-ac76-4ca7-bca1-dd57642ffedb
ms.openlocfilehash: 7b5f4147968ec928a768d973c4a8cd4e2e7ed02e
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875741"
---
# <a name="await-operator-c-reference"></a>await 运算符（C# 参考）

`await` 运算符暂停对其所属的 [async](../keywords/async.md) 方法的求值，直到其操作数表示的异步操作完成。 异步操作完成后，`await` 运算符将返回操作的结果（如果有）。 当 `await` 运算符应用到表示已完成操作的操作数时，它将立即返回操作的结果，而不会暂停其所属的方法。 `await` 运算符不会阻止计算异步方法的线程。 当 `await` 运算符暂停其所属的异步方法时，控件将返回到方法的调用方。

在下面的示例中，<xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A?displayProperty=nameWithType> 方法返回 `Task<byte[]>` 实例，该实例表示在完成时生成字节数组的异步操作。 在操作完成之前，`await` 运算符将暂停 `DownloadDocsMainPageAsync` 方法。 当 `DownloadDocsMainPageAsync` 暂停时，控件将返回到 `Main` 方法，该方法是 `DownloadDocsMainPageAsync` 的调用方。 `Main` 方法将执行，直至它需要 `DownloadDocsMainPageAsync` 方法执行的异步操作的结果。 当 <xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A> 获取所有字节时，将计算 `DownloadDocsMainPageAsync` 方法的其余部分。 之后，将计算 `Main` 方法的其余部分。

[!code-csharp[await example](snippets/shared/AwaitOperator.cs)]

上一个示例使用[异步 `Main` 方法](../../programming-guide/main-and-command-args/index.md)，该方法从 C# 7.1 开始可用。 有关详细信息，请参阅 [Main 方法中的 await 运算符](#await-operator-in-the-main-method)部分。

> [!NOTE]
> 有关异步编程的介绍，请参阅[使用 async 和 await 的异步编程](../../programming-guide/concepts/async/index.md)。 利用 `async` 和 `await` 的异步编程遵循[基于任务的异步模式](../../../standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)。

只能在通过 [async](../keywords/async.md) 关键字修改的方法、[lambda 表达式](lambda-expressions.md)或[匿名方法](delegate-operator.md)中使用 `await` 运算符。 在异步方法中，不能在同步函数的主体、[lock 语句](../keywords/lock-statement.md)块内以及[不安全](../keywords/unsafe.md)的上下文中使用 `await` 运算符。

`await` 运算符的操作数通常是以下其中一个 .NET 类型：<xref:System.Threading.Tasks.Task>、<xref:System.Threading.Tasks.Task%601>、<xref:System.Threading.Tasks.ValueTask> 或 <xref:System.Threading.Tasks.ValueTask%601>。 但是，任何可等待表达式都可以是 `await` 运算符的操作数。 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[可等待表达式](~/_csharplang/spec/expressions.md#awaitable-expressions)部分。

如果表达式 `t` 的类型为 <xref:System.Threading.Tasks.Task%601> 或 <xref:System.Threading.Tasks.ValueTask%601>，则表达式 `await t` 的类型为 `TResult`。 如果 `t` 的类型为 <xref:System.Threading.Tasks.Task> 或 <xref:System.Threading.Tasks.ValueTask>，则 `await t` 的类型为 `void`。 在这两种情况下，如果 `t` 引发异常，则 `await t` 将重新引发异常。 有关如何处理异常的详细信息，请参阅 [try-catch 语句](../keywords/try-catch.md)一文中的[异步方法中的异常](../keywords/try-catch.md#exceptions-in-async-methods)部分。

`async` 和 `await` 关键字在 C# 5 和更高版本中都可用。

## <a name="asynchronous-streams-and-disposables"></a>异步流和可释放对象

从 C# 8.0 开始，可以使用异步流和可释放对象。

可使用 `await foreach` 语句来使用异步数据流。 有关详细信息，请参阅[`foreach`语句](../keywords/foreach-in.md)文章，和 [C# 8.0 新增功能](../../whats-new/csharp-8.md)文章的[异步流](../../whats-new/csharp-8.md#asynchronous-streams)一节。

可使用 `await using` 语句来处理异步可释放对象，即其类型可实现 <xref:System.IAsyncDisposable> 接口的对象。 有关详细信息，请参阅[实现 DisposeAsync 方法](../../../standard/garbage-collection/implementing-disposeasync.md)一文中的[使用异步可释放对象](../../../standard/garbage-collection/implementing-disposeasync.md#using-async-disposable)部分。

## <a name="await-operator-in-the-main-method"></a>Main 方法中的 await 运算符

从 C# 7.1 开始，作为应用程序入口点的 [`Main` 方法](../../programming-guide/main-and-command-args/index.md)可以返回 `Task` 或 `Task<int>`，使其成为异步的，以便在其主体中使用 `await` 运算符。 在较早的 C# 版本中，为了确保 `Main` 方法等待异步操作完成，可以检索由相应的异步方法返回的 <xref:System.Threading.Tasks.Task%601> 实例的 <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType> 属性值。 对于不生成值的异步操作，可以调用 <xref:System.Threading.Tasks.Task.Wait%2A?displayProperty=nameWithType> 方法。 有关如何选择语言版本的信息，请参阅 [C# 语言版本管理](../configure-language-version.md)。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的 [Await 表达式](~/_csharplang/spec/expressions.md#await-expressions)部分。

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [async](../keywords/async.md)
- [任务异步编程模型](../../programming-guide/concepts/async/task-asynchronous-programming-model.md)
- [异步编程](../../async.md)
- [深入了解异步](../../../standard/async-in-depth.md)
- [演练：使用 async 和 await 访问 Web](../../programming-guide/concepts/async/index.md)
- [教程：使用 C# 8.0 和 .NET Core 3.0 生成和使用异步流](../../whats-new/tutorials/generate-consume-asynchronous-stream.md)
