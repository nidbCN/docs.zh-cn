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
# <a name="handling-and-throwing-exceptions-in-net"></a>在 .NET 中处理和引发异常

应用程序必须能够以一致的方式处理执行期间发生的错误。 .NET 提供一种以统一方式向应用程序报错的模型：.NET 操作通过引发异常来指示故障。

## <a name="exceptions"></a>异常

异常是执行程序遇到的所有错误条件或意外行为。 异常可能由你的代码或调用的代码（如共享库）中的错误、不可用的操作系统资源、运行时遇到的意外情况（如无法验证的代码）等引发。 应用程序可从这些情况中的一些中恢复，但无法从其他情况中恢复。 尽管可以从大多数应用程序异常中恢复，但不能从大多数运行时异常中恢复。

在 .NET中，异常是从 <xref:System.Exception?displayProperty=nameWithType> 类继承的对象。 异常引发自发生问题的代码区域。 异常在堆栈中向上传递，直到应用程序对其进行处理或者程序终止。

## <a name="exceptions-vs-traditional-error-handling-methods"></a>异常与传统的错误处理方法

传统上，语言的错误处理模型依赖语言检测错误和针对错误查找其处理程序的独特方式，或者依赖操作系统提供的错误处理机制。 .NET 实现异常处理的方式有以下优点：

- 引发和处理异常的方式与 .NET 编程语言的相同。

- 处理异常不需要任何特定的语言语法，但允许每种语言定义自己的语法。

- 可跨进程，甚至跨计算机边界引发异常。

- 可向应用程序添加异常处理代码以提高程序的可靠性。

异常相较于其他错误通知方法（如返回代码）具有多种优势。 故障不会被忽略掉，因为如果引发了异常且未得到解决，运行时会终止应用程序。 因为代码未能检查出是否存在故障返回代码，所以无效值不会继续在系统中传播。

## <a name="common-exceptions"></a>常见异常

下表列出了一些常见的异常，以及会引发这些异常的原因的示例。

| 异常类型 | 描述 | 示例 |
| -------------- | ----------- | ------- |
| <xref:System.Exception> | 所有异常的基类。 | 无（使用此异常的派生类）。 |
| <xref:System.IndexOutOfRangeException> | 仅当错误地对数组进行索引时，才由运行时引发。 | 在数组的有效范围外对数组进行索引： <br /> `arr[arr.Length+1]` |
| <xref:System.NullReferenceException> | 仅当引用 null 对象时，才由运行时引发。 | `object o = null;` <br /> `o.ToString();` |
| <xref:System.InvalidOperationException> | 当处于无效状态时，由方法引发。 | 从基础集合删除项后调用 `Enumerator.MoveNext()`。 |
| <xref:System.ArgumentException> | 所有自变量异常的基类。 | 无（使用此异常的派生类）。 |
| <xref:System.ArgumentNullException> | 由不允许参数为 null 的方法引发。 | `String s = null;` <br /> `"Calculate".IndexOf(s);`|
| <xref:System.ArgumentOutOfRangeException> | 由验证自变量是否位于给定范围内的方法引发。 | `String s = "string";` <br /> `s.Substring(s.Length+1);` |

## <a name="see-also"></a>请参阅

- [异常类和属性](exception-class-and-properties.md)
- [如何：使用 Try-Catch 块捕获异常](how-to-use-the-try-catch-block-to-catch-exceptions.md)
- [如何：在 Catch 块中使用特定异常](how-to-use-specific-exceptions-in-a-catch-block.md)
- [如何：显式抛出异常](how-to-explicitly-throw-exceptions.md)
- [如何：创建用户定义异常](how-to-create-user-defined-exceptions.md)
- [使用用户筛选的异常处理程序](using-user-filtered-exception-handlers.md)
- [如何：使用 Finally 块](how-to-use-finally-blocks.md)
- [处理 COM 互操作异常](handling-com-interop-exceptions.md)
- [与异常有关的最佳做法](best-practices-for-exceptions.md)
- [每个开发人员都需要了解的有关运行时异常方面的内容](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/exceptions.md)
