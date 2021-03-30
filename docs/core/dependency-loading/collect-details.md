---
title: 收集详细的程序集加载信息 - .NET Core
description: 介绍了如何在 .NET Core 中收集程序集加载信息
author: elinor-fung
ms.author: elfung
ms.date: 11/17/2020
ms.openlocfilehash: 505fc827021fe4d34675b2ef5a7fc5746ada1af6
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872933"
---
# <a name="collect-detailed-assembly-loading-information"></a>收集详细的程序集加载信息

自 .NET 5.0 起，运行时可以通过 `EventPipe` 发出事件，其中包含关于[托管程序集加载](loading-managed.md)的详细信息，以帮助诊断程序集加载问题。 这些[事件](../../fundamentals/diagnostics/runtime-loader-binder-events.md)是由 `Microsoft-Windows-DotNETRuntime` 提供程序在 `AssemblyLoader` 关键字 (`0x4`) 下发出的。

## <a name="prerequisites"></a>先决条件

- [.NET 5.0 SDK](https://dotnet.microsoft.com/download) 或更高版本
- [dotnet-trace](../diagnostics/dotnet-trace.md) 工具。

## <a name="collect-a-trace-with-assembly-loading-events"></a>收集包含程序集加载事件的跟踪

若要在运行时中启用程序集加载事件并收集它们的跟踪，请使用 `dotnet-trace` 和以下命令：

```console
dotnet-trace collect --providers Microsoft-Windows-DotNETRuntime:4 --process-id <pid>
```

这将收集指定 `<pid>` 的跟踪，从而在 `Microsoft-Windows-DotNETRuntime` 提供程序中启用 `AssemblyLoader` 事件。 结果是 `.nettrace` 文件。

## <a name="view-a-trace"></a>查看跟踪

可以使用 [PerfView](https://github.com/microsoft/perfview)中的“事件”视图在 Windows 上查看所收集的跟踪文件。 所有程序集加载事件都将以 `Microsoft-Windows-DotNETRuntime/AssemblyLoader` 为前缀。

## <a name="example-on-windows"></a>示例（在 Windows 上）

本例使用[程序集加载扩展点示例](https://docs.microsoft.com/samples/dotnet/samples/assembly-loading-extension-points/)。 应用程序尝试加载程序集 `MyLibrary`（应用程序未引用的程序集），因此需要在程序集加载扩展点中进行处理才能成功加载。

### <a name="collect-the-trace"></a>收集跟踪

01. 导航到包含下载的示例的目录。 通过以下方式生成应用程序：

    ```console
    dotnet build
    ```

01. 使用指明应用程序应暂停的参数来启动应用程序，并等待按键。 恢复后，它将尝试在默认 `AssemblyLoadContext` 中加载程序集，不需要进行成功加载所需的处理。 导航到输出目录并运行：

    ```console
    AssemblyLoading.exe /d default
    ```

01. 找到应用程序的进程 ID。

    ```console
    dotnet-trace ps
    ```

    输出将列出可用进程。 例如： 。

    ```console
    35832 AssemblyLoading C:\src\AssemblyLoading\bin\Debug\net5.0\AssemblyLoading.exe
    ```

01. 将 `dotnet-trace` 附加到正在运行的应用程序。

    ```console
    dotnet-trace collect --providers Microsoft-Windows-DotNETRuntime:4 --process-id 35832
    ```

01. 在运行应用程序的窗口中，按任意键让程序继续运行。 一旦应用程序退出，跟踪将自动停止。

### <a name="view-the-trace"></a>查看跟踪

在 [PerfView](https://github.com/microsoft/perfview) 中打开所收集的跟踪，然后打开“事件”视图。 将“事件”列表筛选为 `Microsoft-Windows-DotNETRuntime/AssemblyLoader` 事件。

:::image type="content" source="media/collect-details/assembly-loader-filter.png" alt-text="PerfView 程序集加载程序筛选器图像":::

将显示在跟踪启动后应用程序中发生的所有程序集加载。 若要检查此示例中关注的程序集 (`MyLibrary`) 的加载操作，可以执行更多筛选。

### <a name="assembly-loads"></a>程序集加载

使用左侧的“事件”列表，将视图筛选为 `Microsoft-Windows-DotNETRuntime/AssemblyLoader` 下的 [`Start`](../../fundamentals/diagnostics/runtime-loader-binder-events.md#assemblyloadstart-event) 和 [`Stop`](../../fundamentals/diagnostics/runtime-loader-binder-events.md#assemblyloadstop-event) 事件。 将列 `AssemblyName`、`ActivityID` 和 `Success` 添加到视图中。 筛选为包含 `MyLibrary` 的事件。

:::image type="content" source="media/collect-details/start-stop.png" alt-text="PerfView Start 和 Stop 事件图像":::

| 事件名称             | AssemblyName                                      | ActivityID | Success |
| ---------------------- | ------------------------------------------------- | ---------- | ------- |
| `AssemblyLoader/Start` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | //1/2/     |         |
| `AssemblyLoader/Stop`  | `MyLibrary, Culture=neutral, PublicKeyToken=null` | //1/2/     | False   |

你应该会在 `Stop` 事件上看到一个带有 `Success=False` 的 `Start`/`Stop` 对，表明加载操作失败。 请注意，这两个事件有相同的活动 ID。 活动 ID 可用于将其他所有程序集加载程序事件筛选为仅与此加载操作相对应的事件。

### <a name="breakdown-of-attempt-to-load"></a>加载尝试明细

有关加载操作的更详细明细，请使用左侧的“事件”列表，将视图筛选为 `Microsoft-Windows-DotNETRuntime/AssemblyLoader` 下的 [`ResolutionAttempted` 事件](../../fundamentals/diagnostics/runtime-loader-binder-events.md#resolutionattempted-event)。 将列 `AssemblyName`、`Stage` 和 `Result` 添加到视图中。 筛选为包含 `Start`/`Stop` 对中的活动 ID 的事件。

:::image type="content" source="media/collect-details/resolution-attempted.png" alt-text="PerfView ResolutionAttempted 事件图像":::

| 事件名称                           | AssemblyName                                      | 阶段                               | 结果             |
| ------------------------------------ | ------------------------------------------------- | ----------------------------------- | ------------------ |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `FindInLoadContext`                 | `AssemblyNotFound` |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `ApplicationAssemblies`             | `AssemblyNotFound` |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `AssemblyLoadContextResolvingEvent` | `AssemblyNotFound` |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `AppDomainAssemblyResolveEvent`     | `AssemblyNotFound` |

上面的事件表明，程序集加载程序试图通过以下方式解析程序集：在当前加载上下文中查找、运行托管应用程序程序集的默认探测逻辑、调用 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 事件的处理程序，以及调用 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 的处理程序。 对于所有这些步骤，都没有找到程序集。

### <a name="extension-points"></a>扩展点

若要查看调用了哪些扩展点，请使用左侧的“事件”列表，将视图筛选为 `Microsoft-Windows-DotNETRuntime/AssemblyLoader` 下的 [`AssemblyLoadContextResolvingHandlerInvoked`](../../fundamentals/diagnostics/runtime-loader-binder-events.md#assemblyloadcontextresolvinghandlerinvoked-event) 和 [`AppDomainAssemblyResolveHandlerInvoked`](../../fundamentals/diagnostics/runtime-loader-binder-events.md#appdomainassemblyresolvehandlerinvoked-event)。 将列 `AssemblyName` 和 `HandlerName` 添加到视图中。 筛选为包含 `Start`/`Stop` 对中的活动 ID 的事件。

:::image type="content" source="media/collect-details/extension-point.png" alt-text="PerfView 扩展点事件图像":::

| 事件名称                                                  | AssemblyName                                      | HandlerName                      |
| ----------------------------------------------------------- | ------------------------------------------------- | -------------------------------- |
| `AssemblyLoader/AssemblyLoadContextResolvingHandlerInvoked` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `OnAssemblyLoadContextResolving` |
| `AssemblyLoader/AppDomainAssemblyResolveHandlerInvoked`     | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `OnAppDomainAssemblyResolve`     |

上面的事件表明，为 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 事件调用了名为 `OnAssemblyLoadContextResolving` 的处理程序，为 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件调用了名为 `OnAppDomainAssemblyResolve` 的处理程序。

### <a name="collect-another-trace"></a>收集另一个跟踪

使用参数运行应用程序，以便 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 事件的处理程序将加载 `MyLibrary` 程序集。

```console
AssemblyLoading /d default alc-resolving
```

收集并使用[上面的步骤](#collect-the-trace)打开另一个 `.nettrace` 文件。

再次筛选为 `MyLibrary` 的 `Start` 和 `Stop` 事件。 你应该会看到 `Start`/`Stop` 对，以及另一个 `Start`/`Stop` 介于二者之间。 内部加载操作表示 <xref:System.Runtime.Loader.AssemblyLoadContext.Resolving?displayProperty=nameWithType> 的处理程序在调用 <xref:System.Runtime.Loader.AssemblyLoadContext.LoadFromAssemblyPath%2A?displayProperty=nameWithType> 时触发的加载。 这一次，你应该会在 `Stop` 事件上看到 `Success=True`，表明加载操作成功。 `ResultAssemblyPath` 字段显示生成的程序集的路径。

:::image type="content" source="media/collect-details/start-stop-success.png" alt-text="PerfView 成功 Start 和 Stop 事件图像":::

| 事件名称             | AssemblyName                                                       | ActivityID | Success | ResultAssemblyPath                                      |
| ---------------------- | ------------------------------------------------------------------ |------------|---------| ------------------------------------------------------- |
| `AssemblyLoader/Start` |                  `MyLibrary, Culture=neutral, PublicKeyToken=null` | //1/2/     |         |                                                         |
| `AssemblyLoader/Start` | `MyLibrary, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null` | //1/2/1/   |         |                                                         |
| `AssemblyLoader/Stop`  | `MyLibrary, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null` | //1/2/1/   | True    | C:\src\AssemblyLoading\bin\Debug\net5.0\MyLibrary.dll |
| `AssemblyLoader/Stop`  |                  `MyLibrary, Culture=neutral, PublicKeyToken=null` | //1/2/     | True    | C:\src\AssemblyLoading\bin\Debug\net5.0\MyLibrary.dll |

然后，我们可以查看带有外部负载的活动 ID 的 `ResolutionAttempted` 事件，以确定成功解析程序集的步骤。 这一次，事件会显示 `AssemblyLoadContextResolvingEvent` 阶段已成功完成。 `ResultAssemblyPath` 字段显示生成的程序集的路径。

:::image type="content" source="media/collect-details/resolution-attempted-success.png" alt-text="PerfView 成功 ResolutionAttempted 事件图像":::

| 事件名称                           | AssemblyName                                      | 阶段                               | 结果             | ResultAssemblyPath |
| ------------------------------------ | ------------------------------------------------- | ----------------------------------- | ------------------ | ------------------ |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `FindInLoadContext`                 | `AssemblyNotFound` |                    |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `ApplicationAssemblies`             | `AssemblyNotFound` |                    |
| `AssemblyLoader/ResolutionAttempted` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `AssemblyLoadContextResolvingEvent` | `Success`          | C:\src\AssemblyLoading\bin\Debug\net5.0\MyLibrary.dll |

查看 `AssemblyLoadContextResolvingHandlerInvoked` 事件将看到调用了名为 `OnAssemblyLoadContextResolving` 的处理程序。 `ResultAssemblyPath` 字段显示由处理程序返回的程序集的路径。

:::image type="content" source="media/collect-details/extension-point-success.png" alt-text="PerfView 成功扩展点事件图像":::

| 事件名称                                                  | AssemblyName                                      | HandlerName                      | ResultAssemblyPath |
| ----------------------------------------------------------- | ------------------------------------------------- | -------------------------------- | ------------------ |
| `AssemblyLoader/AssemblyLoadContextResolvingHandlerInvoked` | `MyLibrary, Culture=neutral, PublicKeyToken=null` | `OnAssemblyLoadContextResolving` | C:\src\AssemblyLoading\bin\Debug\net5.0\MyLibrary.dll |

请注意，不再存在带有 `AppDomainAssemblyResolveEvent` 阶段的 `ResolutionAttempted` 事件或任何 `AppDomainAssemblyResolveHandlerInvoked` 事件，因为程序集在到达引发 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 事件的加载算法步骤之前已成功加载。

## <a name="see-also"></a>另请参阅

- [程序集加载程序事件](../../fundamentals/diagnostics/runtime-loader-binder-events.md)
- [dotnet-trace](../diagnostics/dotnet-trace.md)
- [PerfView](https://github.com/microsoft/perfview)
