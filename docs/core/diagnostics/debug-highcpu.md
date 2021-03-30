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
# <a name="debug-high-cpu-usage-in-net-core"></a><span data-ttu-id="0bab9-103">调试 .NET Core 中的高 CPU 使用率</span><span class="sxs-lookup"><span data-stu-id="0bab9-103">Debug high CPU usage in .NET Core</span></span>

<span data-ttu-id="0bab9-104">**本文适用于：** ✔️ .NET Core 3.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="0bab9-104">**This article applies to: ✔️** .NET Core 3.1 SDK and later versions</span></span>

<span data-ttu-id="0bab9-105">本教程将介绍如何调试 CPU 使用率过高的情况。</span><span class="sxs-lookup"><span data-stu-id="0bab9-105">In this tutorial, you'll learn how to debug an excessive CPU usage scenario.</span></span> <span data-ttu-id="0bab9-106">使用提供的示例 [ASP.NET Core Web 应用](/samples/dotnet/samples/diagnostic-scenarios) 源代码存储库，可以故意造成死锁。</span><span class="sxs-lookup"><span data-stu-id="0bab9-106">Using the provided example [ASP.NET Core web app](/samples/dotnet/samples/diagnostic-scenarios) source code repository, you can cause a deadlock intentionally.</span></span> <span data-ttu-id="0bab9-107">终结点将遇到线程挂起和线程累积。</span><span class="sxs-lookup"><span data-stu-id="0bab9-107">The endpoint will experience a hang and thread accumulation.</span></span> <span data-ttu-id="0bab9-108">你将了解如何使用各种工具，通过几条关键的诊断数据诊断此情况。</span><span class="sxs-lookup"><span data-stu-id="0bab9-108">You'll learn how you can use various tools to diagnose this scenario with several key pieces of diagnostics data.</span></span>

<span data-ttu-id="0bab9-109">在本教程中，你将：</span><span class="sxs-lookup"><span data-stu-id="0bab9-109">In this tutorial, you will:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="0bab9-110">调查 CPU 使用率是否过高</span><span class="sxs-lookup"><span data-stu-id="0bab9-110">Investigate high CPU usage</span></span>
> - <span data-ttu-id="0bab9-111">使用 [dotnet-counters](dotnet-counters.md) 确定 CPU 使用率</span><span class="sxs-lookup"><span data-stu-id="0bab9-111">Determine CPU usage with [dotnet-counters](dotnet-counters.md)</span></span>
> - <span data-ttu-id="0bab9-112">使用 [dotnet-trace](dotnet-trace.md) 进行跟踪生成</span><span class="sxs-lookup"><span data-stu-id="0bab9-112">Use [dotnet-trace](dotnet-trace.md) for trace generation</span></span>
> - <span data-ttu-id="0bab9-113">PerfView 中的配置文件性能</span><span class="sxs-lookup"><span data-stu-id="0bab9-113">Profile performance in PerfView</span></span>
> - <span data-ttu-id="0bab9-114">诊断并解决 CPU 使用率过高的问题</span><span class="sxs-lookup"><span data-stu-id="0bab9-114">Diagnose and solve excessive CPU usage</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0bab9-115">先决条件</span><span class="sxs-lookup"><span data-stu-id="0bab9-115">Prerequisites</span></span>

<span data-ttu-id="0bab9-116">本教程使用：</span><span class="sxs-lookup"><span data-stu-id="0bab9-116">The tutorial uses:</span></span>

- <span data-ttu-id="0bab9-117">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="0bab9-117">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) or a later version.</span></span>
- <span data-ttu-id="0bab9-118">[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)以触发场景。</span><span class="sxs-lookup"><span data-stu-id="0bab9-118">[Sample debug target](/samples/dotnet/samples/diagnostic-scenarios) to trigger the scenario.</span></span>
- <span data-ttu-id="0bab9-119">[dotnet-trace](dotnet-trace.md) 以列出进程并生成配置文件。</span><span class="sxs-lookup"><span data-stu-id="0bab9-119">[dotnet-trace](dotnet-trace.md) to list processes and generate a profile.</span></span>
- <span data-ttu-id="0bab9-120">[dotnet-counters](dotnet-counters.md) 以监视 CPU 使用率。</span><span class="sxs-lookup"><span data-stu-id="0bab9-120">[dotnet-counters](dotnet-counters.md) to monitor cpu usage.</span></span>

## <a name="cpu-counters"></a><span data-ttu-id="0bab9-121">CPU 计数器</span><span class="sxs-lookup"><span data-stu-id="0bab9-121">CPU counters</span></span>

<span data-ttu-id="0bab9-122">在尝试收集诊断数据之前，需要观察 CPU 状况是否过高。</span><span class="sxs-lookup"><span data-stu-id="0bab9-122">Before attempting to collect diagnostics data, you need to observe a high CPU condition.</span></span> <span data-ttu-id="0bab9-123">使用以下命令从项目根目录运行[示例应用程序](/samples/dotnet/samples/diagnostic-scenarios)。</span><span class="sxs-lookup"><span data-stu-id="0bab9-123">Run the [sample application](/samples/dotnet/samples/diagnostic-scenarios) using the following command from the project root directory.</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="0bab9-124">若要查找该进程 ID，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="0bab9-124">To find the process ID, use the following command:</span></span>

```dotnetcli
dotnet-trace ps
```

<span data-ttu-id="0bab9-125">注意命令输出中的进程 ID。</span><span class="sxs-lookup"><span data-stu-id="0bab9-125">Take note of the process ID from your command output.</span></span> <span data-ttu-id="0bab9-126">我们的进程 ID 是 `22884`，你的进程 ID 将不同。</span><span class="sxs-lookup"><span data-stu-id="0bab9-126">Our process ID was `22884`, but yours will be different.</span></span> <span data-ttu-id="0bab9-127">若要检查当前的 CPU 使用率，请使用 [dotnet counters](dotnet-counters.md) 工具命令：</span><span class="sxs-lookup"><span data-stu-id="0bab9-127">To check the current CPU usage, use the [dotnet-counters](dotnet-counters.md) tool command:</span></span>

```dotnetcli
dotnet-counters monitor --refresh-interval 1 -p 22884
```

<span data-ttu-id="0bab9-128">`refresh-interval` 是计数器轮询 CPU 值间隔的秒数。</span><span class="sxs-lookup"><span data-stu-id="0bab9-128">The `refresh-interval` is the number of seconds between the counter polling CPU values.</span></span> <span data-ttu-id="0bab9-129">输出应与以下内容类似：</span><span class="sxs-lookup"><span data-stu-id="0bab9-129">The output should be similar to the following:</span></span>

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

<span data-ttu-id="0bab9-130">在 Web 应用运行的情况下，CPU 根本不会在启动后就立即被消耗，且会在 `0%` 进行报告。</span><span class="sxs-lookup"><span data-stu-id="0bab9-130">With the web app running, immediately after startup, the CPU isn't being consumed at all and is reported at `0%`.</span></span> <span data-ttu-id="0bab9-131">使用 `60000` 作为路由参数导航到 `api/diagscenario/highcpu` 路由：</span><span class="sxs-lookup"><span data-stu-id="0bab9-131">Navigate to the `api/diagscenario/highcpu` route with `60000` as the route parameter:</span></span>

`https://localhost:5001/api/diagscenario/highcpu/60000`

<span data-ttu-id="0bab9-132">现在，重新运行 [dotnet-counters](dotnet-counters.md) 命令。</span><span class="sxs-lookup"><span data-stu-id="0bab9-132">Now, rerun the [dotnet-counters](dotnet-counters.md) command.</span></span> <span data-ttu-id="0bab9-133">若要只监视 `cpu-usage`，请在命令中指定 `System.Runtime[cpu-usage]`。</span><span class="sxs-lookup"><span data-stu-id="0bab9-133">To monitor just the `cpu-usage`, specify `System.Runtime[cpu-usage]` as part of the command.</span></span>

```dotnetcli
dotnet-counters monitor --counters System.Runtime[cpu-usage] -p 22884 --refresh-interval 1
```

<span data-ttu-id="0bab9-134">你将看到 CPU 使用率已增加，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0bab9-134">You should see an increase in CPU usage as shown below:</span></span>

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[System.Runtime]
    CPU Usage (%)                                         25
```

<span data-ttu-id="0bab9-135">在整个请求期间，CPU 使用率将徘徊在 25% 左右。</span><span class="sxs-lookup"><span data-stu-id="0bab9-135">Throughout the duration of the request, the CPU usage will hover around 25% .</span></span> <span data-ttu-id="0bab9-136">根据主机的不同，预期 CPU 使用率会有所不同。</span><span class="sxs-lookup"><span data-stu-id="0bab9-136">Depending on the host machine, expect varying CPU usage.</span></span>

> [!TIP]
> <span data-ttu-id="0bab9-137">若要可视化更高的 CPU 使用率，可以在多个浏览器选项卡中同时使用此终结点。</span><span class="sxs-lookup"><span data-stu-id="0bab9-137">To visualize an even higher CPU usage, you can exercise this endpoint in multiple browser tabs simultaneously.</span></span>

<span data-ttu-id="0bab9-138">此时，你可以放心地说 CPU 运行的速度比预期的要高。</span><span class="sxs-lookup"><span data-stu-id="0bab9-138">At this point, you can safely say the CPU is running higher than you expect.</span></span>

## <a name="trace-generation"></a><span data-ttu-id="0bab9-139">跟踪生成</span><span class="sxs-lookup"><span data-stu-id="0bab9-139">Trace generation</span></span>

<span data-ttu-id="0bab9-140">当分析速度较慢的请求时，需要一个诊断工具来提供代码正在执行的操作的见解。</span><span class="sxs-lookup"><span data-stu-id="0bab9-140">When analyzing a slow request, you need a diagnostics tool that can provide insights into what the code is doing.</span></span> <span data-ttu-id="0bab9-141">常见的选择是探查器，并且有不同的探查器选项可供选择。</span><span class="sxs-lookup"><span data-stu-id="0bab9-141">The usual choice is a profiler, and there are different profiler options to choose from.</span></span>

### <a name="linux"></a>[<span data-ttu-id="0bab9-142">Linux</span><span class="sxs-lookup"><span data-stu-id="0bab9-142">Linux</span></span>](#tab/linux)

<span data-ttu-id="0bab9-143">`perf` 工具可用于生成 .NET Core 应用配置文件。</span><span class="sxs-lookup"><span data-stu-id="0bab9-143">The `perf` tool can be used to generate .NET Core app profiles.</span></span> <span data-ttu-id="0bab9-144">退出[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)的上一个实例。</span><span class="sxs-lookup"><span data-stu-id="0bab9-144">Exit the previous instance of the [sample debug target](/samples/dotnet/samples/diagnostic-scenarios).</span></span>

<span data-ttu-id="0bab9-145">设置 `COMPlus_PerfMapEnabled` 环境变量，使 .NET Core 应用在 `/tmp` 目录中创建 `map` 文件。</span><span class="sxs-lookup"><span data-stu-id="0bab9-145">Set the `COMPlus_PerfMapEnabled` environment variable to cause the .NET Core app to create a `map` file in the `/tmp` directory.</span></span> <span data-ttu-id="0bab9-146">`perf` 使用此 `map` 文件按名称将 CPU 地址映射到 JIT 生成的函数。</span><span class="sxs-lookup"><span data-stu-id="0bab9-146">This `map` file is used by `perf` to map CPU address to JIT-generated functions by name.</span></span> <span data-ttu-id="0bab9-147">有关详细信息，请参阅[写入 Perf 映射](../run-time-config/debugging-profiling.md#write-perf-map)。</span><span class="sxs-lookup"><span data-stu-id="0bab9-147">For more information, see [Write perf map](../run-time-config/debugging-profiling.md#write-perf-map).</span></span>

<span data-ttu-id="0bab9-148">在同一终端会话中运行[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)。</span><span class="sxs-lookup"><span data-stu-id="0bab9-148">Run the [sample debug target](/samples/dotnet/samples/diagnostic-scenarios) in the same terminal session.</span></span>

```dotnetcli
export COMPlus_PerfMapEnabled=1
dotnet run
```

<span data-ttu-id="0bab9-149">再次使用高 CPU API (`https://localhost:5001/api/diagscenario/highcpu/60000`) 终结点。</span><span class="sxs-lookup"><span data-stu-id="0bab9-149">Exercise the high CPU API endpoint (`https://localhost:5001/api/diagscenario/highcpu/60000`) again.</span></span> <span data-ttu-id="0bab9-150">当它在 1 分钟请求内运行时，对进程 ID 运行 `perf` 命令：</span><span class="sxs-lookup"><span data-stu-id="0bab9-150">While it's running within the 1-minute request, run the `perf` command with your process ID:</span></span>

```bash
sudo perf record -p 2266 -g
```

<span data-ttu-id="0bab9-151">`perf` 命令将启动性能收集过程。</span><span class="sxs-lookup"><span data-stu-id="0bab9-151">The `perf` command starts the performance collection process.</span></span> <span data-ttu-id="0bab9-152">让它运行大约 20-30 秒，然后按 <kbd>Ctrl+C</kbd> 退出收集过程。</span><span class="sxs-lookup"><span data-stu-id="0bab9-152">Let it run for about 20-30 seconds, then press <kbd>Ctrl+C</kbd> to exit the collection process.</span></span> <span data-ttu-id="0bab9-153">可以使用相同的 `perf` 命令来查看跟踪的输出。</span><span class="sxs-lookup"><span data-stu-id="0bab9-153">You can use the same `perf` command to see the output of the trace.</span></span>

```bash
sudo perf report -f
```

<span data-ttu-id="0bab9-154">还可以使用以下命令生成 flame-graph：</span><span class="sxs-lookup"><span data-stu-id="0bab9-154">You can also generate a _flame-graph_ by using the following commands:</span></span>

```bash
git clone --depth=1 https://github.com/BrendanGregg/FlameGraph
sudo perf script | FlameGraph/stackcollapse-perf.pl | FlameGraph/flamegraph.pl > flamegraph.svg
```

<span data-ttu-id="0bab9-155">此命令生成 `flamegraph.svg`，你可以在浏览器中查看 `flamegraph.svg` 以调查性能问题：</span><span class="sxs-lookup"><span data-stu-id="0bab9-155">This command generates a `flamegraph.svg` that you can view in the browser to investigate the performance problem:</span></span>

<span data-ttu-id="0bab9-156">[![火焰图 SVG 图像](media/flamegraph.jpg)](media/flamegraph.jpg#lightbox)</span><span class="sxs-lookup"><span data-stu-id="0bab9-156">[![Flame graph SVG image](media/flamegraph.jpg)](media/flamegraph.jpg#lightbox)</span></span>

### <a name="windows"></a>[<span data-ttu-id="0bab9-157">Windows</span><span class="sxs-lookup"><span data-stu-id="0bab9-157">Windows</span></span>](#tab/windows)

<span data-ttu-id="0bab9-158">在 Windows 上，可以使用 [dotnet-trace](dotnet-trace.md) 工具作为探查器。</span><span class="sxs-lookup"><span data-stu-id="0bab9-158">On Windows, you can use the [dotnet-trace](dotnet-trace.md) tool as a profiler.</span></span> <span data-ttu-id="0bab9-159">使用之前的[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)，再次使用高 CPU (`https://localhost:5001/api/diagscenario/highcpu/60000`) 终结点。</span><span class="sxs-lookup"><span data-stu-id="0bab9-159">Using the previous [sample debug target](/samples/dotnet/samples/diagnostic-scenarios), exercise the high CPU endpoint (`https://localhost:5001/api/diagscenario/highcpu/60000`) again.</span></span> <span data-ttu-id="0bab9-160">当它在 1 分钟请求内运行时，使用 `collect` 命令，如下所示：</span><span class="sxs-lookup"><span data-stu-id="0bab9-160">While it's running within the 1-minute request, use the `collect` command as follows:</span></span>

```dotnetcli
dotnet-trace collect -p 22884 --providers Microsoft-DotNETCore-SampleProfiler
```

<span data-ttu-id="0bab9-161">让 [dotnet-trace](dotnet-trace.md) 运行大约 20-30 秒，然后按 <kbd>Enter</kbd> 退出收集。</span><span class="sxs-lookup"><span data-stu-id="0bab9-161">Let [dotnet-trace](dotnet-trace.md) run for about 20-30 seconds, and then press the <kbd>Enter</kbd> to exit the collection.</span></span> <span data-ttu-id="0bab9-162">结果是位于同一文件夹中的 `nettrace` 文件。</span><span class="sxs-lookup"><span data-stu-id="0bab9-162">The result is a `nettrace` file located in the same folder.</span></span> <span data-ttu-id="0bab9-163">`nettrace` 文件是在 Windows 上使用现有分析工具的好方法。</span><span class="sxs-lookup"><span data-stu-id="0bab9-163">The `nettrace` files are a great way to use existing analysis tools on Windows.</span></span>

<span data-ttu-id="0bab9-164">使用 [`PerfView`](https://github.com/microsoft/perfview/blob/master/documentation/Downloading.md) 打开 `nettrace`，如下所示。</span><span class="sxs-lookup"><span data-stu-id="0bab9-164">Open the `nettrace` with [`PerfView`](https://github.com/microsoft/perfview/blob/master/documentation/Downloading.md) as shown below.</span></span>

<span data-ttu-id="0bab9-165">[![PerfView 图像](media/perfview.jpg)](media/perfview.jpg#lightbox)</span><span class="sxs-lookup"><span data-stu-id="0bab9-165">[![PerfView image](media/perfview.jpg)](media/perfview.jpg#lightbox)</span></span>

---

## <a name="see-also"></a><span data-ttu-id="0bab9-166">请参阅</span><span class="sxs-lookup"><span data-stu-id="0bab9-166">See also</span></span>

- <span data-ttu-id="0bab9-167">用于列出进程的 [dotnet-trace](dotnet-trace.md)</span><span class="sxs-lookup"><span data-stu-id="0bab9-167">[dotnet-trace](dotnet-trace.md) to list processes</span></span>
- <span data-ttu-id="0bab9-168">用于检查托管内存使用情况的 [dotnet-counters](dotnet-counters.md)</span><span class="sxs-lookup"><span data-stu-id="0bab9-168">[dotnet-counters](dotnet-counters.md) to check managed memory usage</span></span>
- <span data-ttu-id="0bab9-169">用于收集和分析转储文件的 [dotnet-dump](dotnet-dump.md)</span><span class="sxs-lookup"><span data-stu-id="0bab9-169">[dotnet-dump](dotnet-dump.md) to collect and analyze a dump file</span></span>
- [<span data-ttu-id="0bab9-170">dotnet/diagnostics</span><span class="sxs-lookup"><span data-stu-id="0bab9-170">dotnet/diagnostics</span></span>](https://github.com/dotnet/diagnostics/tree/main/documentation/tutorial)

## <a name="next-steps"></a><span data-ttu-id="0bab9-171">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0bab9-171">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0bab9-172">调试 .NET Core 中的死锁</span><span class="sxs-lookup"><span data-stu-id="0bab9-172">Debug a deadlock in .NET Core</span></span>](debug-deadlock.md)
