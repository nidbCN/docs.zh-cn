---
title: 调试高 CPU 使用率 - .NET Core
description: 本教程将演示如何调试 .NETCore 中的高 CPU 使用率。
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: ed22da3d53dfd1197de1f9b3c11ef31389cad690
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872790"
---
# <a name="debug-high-cpu-usage-in-net-core"></a>调试 .NET Core 中的高 CPU 使用率

**本文适用于：** ✔️ .NET Core 3.1 SDK 及更高版本

本教程将介绍如何调试 CPU 使用率过高的情况。 使用提供的示例 [ASP.NET Core Web 应用](/samples/dotnet/samples/diagnostic-scenarios) 源代码存储库，可以故意造成死锁。 终结点将遇到线程挂起和线程累积。 你将了解如何使用各种工具，通过几条关键的诊断数据诊断此情况。

在本教程中，你将：

> [!div class="checklist"]
>
> - 调查 CPU 使用率是否过高
> - 使用 [dotnet-counters](dotnet-counters.md) 确定 CPU 使用率
> - 使用 [dotnet-trace](dotnet-trace.md) 进行跟踪生成
> - PerfView 中的配置文件性能
> - 诊断并解决 CPU 使用率过高的问题

## <a name="prerequisites"></a>先决条件

本教程使用：

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本。
- [示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)以触发场景。
- [dotnet-trace](dotnet-trace.md) 以列出进程并生成配置文件。
- [dotnet-counters](dotnet-counters.md) 以监视 CPU 使用率。

## <a name="cpu-counters"></a>CPU 计数器

在尝试收集诊断数据之前，需要观察 CPU 状况是否过高。 使用以下命令从项目根目录运行[示例应用程序](/samples/dotnet/samples/diagnostic-scenarios)。

```dotnetcli
dotnet run
```

若要查找该进程 ID，请使用以下命令：

```dotnetcli
dotnet-trace ps
```

注意命令输出中的进程 ID。 我们的进程 ID 是 `22884`，你的进程 ID 将不同。 若要检查当前的 CPU 使用率，请使用 [dotnet counters](dotnet-counters.md) 工具命令：

```dotnetcli
dotnet-counters monitor --refresh-interval 1 -p 22884
```

`refresh-interval` 是计数器轮询 CPU 值间隔的秒数。 输出应与以下内容类似：

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    % Time in GC since last GC (%)                         0
    Allocation Rate / 1 sec (B)                            0
    CPU Usage (%)                                          0
    Exception Count / 1 sec                                0
    GC Heap Size (MB)                                      4
    Gen 0 GC Count / 60 sec                                0
    Gen 0 Size (B)                                         0
    Gen 1 GC Count / 60 sec                                0
    Gen 1 Size (B)                                         0
    Gen 2 GC Count / 60 sec                                0
    Gen 2 Size (B)                                         0
    LOH Size (B)                                           0
    Monitor Lock Contention Count / 1 sec                  0
    Number of Active Timers                                1
    Number of Assemblies Loaded                          140
    ThreadPool Completed Work Item Count / 1 sec           3
    ThreadPool Queue Length                                0
    ThreadPool Thread Count                                7
    Working Set (MB)                                      63
```

在 Web 应用运行的情况下，CPU 根本不会在启动后就立即被消耗，且会在 `0%` 进行报告。 使用 `60000` 作为路由参数导航到 `api/diagscenario/highcpu` 路由：

`https://localhost:5001/api/diagscenario/highcpu/60000`

现在，重新运行 [dotnet-counters](dotnet-counters.md) 命令。 若要只监视 `cpu-usage`，请在命令中指定 `System.Runtime[cpu-usage]`。

```dotnetcli
dotnet-counters monitor --counters System.Runtime[cpu-usage] -p 22884 --refresh-interval 1
```

你将看到 CPU 使用率已增加，如下所示：

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    CPU Usage (%)                                         25
```

在整个请求期间，CPU 使用率将徘徊在 25% 左右。 根据主机的不同，预期 CPU 使用率会有所不同。

> [!TIP]
> 若要可视化更高的 CPU 使用率，可以在多个浏览器选项卡中同时使用此终结点。

此时，你可以放心地说 CPU 运行的速度比预期的要高。

## <a name="trace-generation"></a>跟踪生成

当分析速度较慢的请求时，需要一个诊断工具来提供代码正在执行的操作的见解。 常见的选择是探查器，并且有不同的探查器选项可供选择。

### <a name="linux"></a>[Linux](#tab/linux)

`perf` 工具可用于生成 .NET Core 应用配置文件。 退出[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)的上一个实例。

设置 `COMPlus_PerfMapEnabled` 环境变量，使 .NET Core 应用在 `/tmp` 目录中创建 `map` 文件。 `perf` 使用此 `map` 文件按名称将 CPU 地址映射到 JIT 生成的函数。 有关详细信息，请参阅[写入 Perf 映射](../run-time-config/debugging-profiling.md#write-perf-map)。

在同一终端会话中运行[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)。

```dotnetcli
export COMPlus_PerfMapEnabled=1
dotnet run
```

再次使用高 CPU API (`https://localhost:5001/api/diagscenario/highcpu/60000`) 终结点。 当它在 1 分钟请求内运行时，对进程 ID 运行 `perf` 命令：

```bash
sudo perf record -p 2266 -g
```

`perf` 命令将启动性能收集过程。 让它运行大约 20-30 秒，然后按 <kbd>Ctrl+C</kbd> 退出收集过程。 可以使用相同的 `perf` 命令来查看跟踪的输出。

```bash
sudo perf report -f
```

还可以使用以下命令生成 flame-graph：

```bash
git clone --depth=1 https://github.com/BrendanGregg/FlameGraph
sudo perf script | FlameGraph/stackcollapse-perf.pl | FlameGraph/flamegraph.pl > flamegraph.svg
```

此命令生成 `flamegraph.svg`，你可以在浏览器中查看 `flamegraph.svg` 以调查性能问题：

[![火焰图 SVG 图像](media/flamegraph.jpg)](media/flamegraph.jpg#lightbox)

### <a name="windows"></a>[Windows](#tab/windows)

在 Windows 上，可以使用 [dotnet-trace](dotnet-trace.md) 工具作为探查器。 使用之前的[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)，再次使用高 CPU (`https://localhost:5001/api/diagscenario/highcpu/60000`) 终结点。 当它在 1 分钟请求内运行时，使用 `collect` 命令，如下所示：

```dotnetcli
dotnet-trace collect -p 22884 --providers Microsoft-DotNETCore-SampleProfiler
```

让 [dotnet-trace](dotnet-trace.md) 运行大约 20-30 秒，然后按 <kbd>Enter</kbd> 退出收集。 结果是位于同一文件夹中的 `nettrace` 文件。 `nettrace` 文件是在 Windows 上使用现有分析工具的好方法。

使用 [`PerfView`](https://github.com/microsoft/perfview/blob/master/documentation/Downloading.md) 打开 `nettrace`，如下所示。

[![PerfView 图像](media/perfview.jpg)](media/perfview.jpg#lightbox)

---

## <a name="see-also"></a>请参阅

- 用于列出进程的 [dotnet-trace](dotnet-trace.md)
- 用于检查托管内存使用情况的 [dotnet-counters](dotnet-counters.md)
- 用于收集和分析转储文件的 [dotnet-dump](dotnet-dump.md)
- [dotnet/diagnostics](https://github.com/dotnet/diagnostics/tree/main/documentation/tutorial)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [调试 .NET Core 中的死锁](debug-deadlock.md)
