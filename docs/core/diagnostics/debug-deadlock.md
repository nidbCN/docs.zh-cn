---
title: 调试死锁 - .NET Core
description: 本教程演示如何调试 .NET Core 中的锁定问题。
ms.topic: tutorial
ms.date: 07/20/2020
ms.openlocfilehash: a268884740ff38d7975303a5a5e5f2c24f16778d
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874261"
---
# <a name="debug-a-deadlock-in-net-core"></a><span data-ttu-id="67b64-103">调试 .NET Core 中的死锁</span><span class="sxs-lookup"><span data-stu-id="67b64-103">Debug a deadlock in .NET Core</span></span>

<span data-ttu-id="67b64-104">**本文适用于：** ✔️ .NET Core 3.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="67b64-104">**This article applies to: ✔️** .NET Core 3.1 SDK and later versions</span></span>

<span data-ttu-id="67b64-105">本教程将介绍如何调试死锁情况。</span><span class="sxs-lookup"><span data-stu-id="67b64-105">In this tutorial, you'll learn how to debug a deadlock scenario.</span></span> <span data-ttu-id="67b64-106">使用提供的示例 [ASP.NET Core Web 应用](/samples/dotnet/samples/diagnostic-scenarios) 源代码存储库，可以故意造成死锁。</span><span class="sxs-lookup"><span data-stu-id="67b64-106">Using the provided example [ASP.NET Core web app](/samples/dotnet/samples/diagnostic-scenarios) source code repository, you can cause a deadlock intentionally.</span></span> <span data-ttu-id="67b64-107">终结点将遇到线程挂起和线程累积。</span><span class="sxs-lookup"><span data-stu-id="67b64-107">The endpoint will experience a hang and thread accumulation.</span></span> <span data-ttu-id="67b64-108">你将了解如何使用各种工具来分析问题，例如核心转储、核心转储分析和进程跟踪。</span><span class="sxs-lookup"><span data-stu-id="67b64-108">You'll learn how you can use various tools to analyze the problem, such as core dumps, core dump analysis, and process tracing.</span></span>

<span data-ttu-id="67b64-109">在本教程中，你将：</span><span class="sxs-lookup"><span data-stu-id="67b64-109">In this tutorial, you will:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="67b64-110">调查应用挂起</span><span class="sxs-lookup"><span data-stu-id="67b64-110">Investigate an app hang</span></span>
> - <span data-ttu-id="67b64-111">生成核心转储文件</span><span class="sxs-lookup"><span data-stu-id="67b64-111">Generate a core dump file</span></span>
> - <span data-ttu-id="67b64-112">分析转储文件中的进程线程</span><span class="sxs-lookup"><span data-stu-id="67b64-112">Analyze process threads in the dump file</span></span>
> - <span data-ttu-id="67b64-113">分析调用堆栈和同步块</span><span class="sxs-lookup"><span data-stu-id="67b64-113">Analyze callstacks and sync blocks</span></span>
> - <span data-ttu-id="67b64-114">诊断并解决死锁</span><span class="sxs-lookup"><span data-stu-id="67b64-114">Diagnose and solve a deadlock</span></span>

## <a name="prerequisites"></a><span data-ttu-id="67b64-115">先决条件</span><span class="sxs-lookup"><span data-stu-id="67b64-115">Prerequisites</span></span>

<span data-ttu-id="67b64-116">本教程使用：</span><span class="sxs-lookup"><span data-stu-id="67b64-116">The tutorial uses:</span></span>

- <span data-ttu-id="67b64-117">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="67b64-117">[.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet) or a later version</span></span>
- <span data-ttu-id="67b64-118">用于触发场景的[示例调试目标 - Web 应用](/samples/dotnet/samples/diagnostic-scenarios)</span><span class="sxs-lookup"><span data-stu-id="67b64-118">[Sample debug target - web app](/samples/dotnet/samples/diagnostic-scenarios) to trigger the scenario</span></span>
- <span data-ttu-id="67b64-119">用于列出进程的 [dotnet-trace](dotnet-trace.md)</span><span class="sxs-lookup"><span data-stu-id="67b64-119">[dotnet-trace](dotnet-trace.md) to list processes</span></span>
- <span data-ttu-id="67b64-120">收集和分析转储文件的 [dotnet-dump](dotnet-dump.md)</span><span class="sxs-lookup"><span data-stu-id="67b64-120">[dotnet-dump](dotnet-dump.md) to collect, and analyze a dump file</span></span>

## <a name="core-dump-generation"></a><span data-ttu-id="67b64-121">核心转储生成</span><span class="sxs-lookup"><span data-stu-id="67b64-121">Core dump generation</span></span>

<span data-ttu-id="67b64-122">为了调查应用程序无响应问题，核心转储或内存转储允许你检查其线程的状态以及任何可能存在争用问题的锁定状态。</span><span class="sxs-lookup"><span data-stu-id="67b64-122">To investigate application unresponsiveness, a core dump or memory dump allows you to inspect the state of its threads and any possible locks that may have contention issues.</span></span> <span data-ttu-id="67b64-123">使用以下命令从示例根目录运行[示例调试](/samples/dotnet/samples/diagnostic-scenarios)应用程序：</span><span class="sxs-lookup"><span data-stu-id="67b64-123">Run the [sample debug](/samples/dotnet/samples/diagnostic-scenarios) application using the following command from the sample root directory:</span></span>

```dotnetcli
dotnet run
```

<span data-ttu-id="67b64-124">若要查找进程 ID，请使用以下命令：</span><span class="sxs-lookup"><span data-stu-id="67b64-124">To find the process ID, use the following command:</span></span>

```dotnetcli
dotnet-trace ps
```

<span data-ttu-id="67b64-125">注意命令输出中的进程 ID。</span><span class="sxs-lookup"><span data-stu-id="67b64-125">Take note of the process ID from your command output.</span></span> <span data-ttu-id="67b64-126">我们的进程 ID 是 `4807`，你的进程 ID 将不同。</span><span class="sxs-lookup"><span data-stu-id="67b64-126">Our process ID was `4807`, but yours will be different.</span></span> <span data-ttu-id="67b64-127">导航到以下 URL，该 URL 是示例站点上的 API 终结点：</span><span class="sxs-lookup"><span data-stu-id="67b64-127">Navigate to the following URL, which is an API endpoint on the sample site:</span></span>

`https://localhost:5001/api/diagscenario/deadlock`

<span data-ttu-id="67b64-128">向站点发出的 API 请求将挂起并且不会响应。</span><span class="sxs-lookup"><span data-stu-id="67b64-128">The API request to the site will hang and not respond.</span></span> <span data-ttu-id="67b64-129">让请求运行大约 10-15 秒。</span><span class="sxs-lookup"><span data-stu-id="67b64-129">Let the request run for about 10-15 seconds.</span></span> <span data-ttu-id="67b64-130">然后使用以下命令创建核心转储：</span><span class="sxs-lookup"><span data-stu-id="67b64-130">Then create the core dump using the following command:</span></span>

### <a name="linux"></a>[<span data-ttu-id="67b64-131">Linux</span><span class="sxs-lookup"><span data-stu-id="67b64-131">Linux</span></span>](#tab/linux)

```bash
sudo dotnet-dump collect -p 4807
```

### <a name="windows"></a>[<span data-ttu-id="67b64-132">Windows</span><span class="sxs-lookup"><span data-stu-id="67b64-132">Windows</span></span>](#tab/windows)

```console
dotnet-dump collect -p 4807
```

---

## <a name="analyze-the-core-dump"></a><span data-ttu-id="67b64-133">分析核心转储</span><span class="sxs-lookup"><span data-stu-id="67b64-133">Analyze the core dump</span></span>

<span data-ttu-id="67b64-134">若要启动核心转储分析，请使用以下 `dotnet-dump analyze` 命令打开核心转储。</span><span class="sxs-lookup"><span data-stu-id="67b64-134">To start the core dump analysis, open the core dump using the following `dotnet-dump analyze` command.</span></span> <span data-ttu-id="67b64-135">参数是先前收集的核心转储文件的路径。</span><span class="sxs-lookup"><span data-stu-id="67b64-135">The argument is the path to the core dump file that was collected earlier.</span></span>

```dotnetcli
dotnet-dump analyze  ~/.dotnet/tools/core_20190513_143916
```

<span data-ttu-id="67b64-136">由于要查看潜在的挂起，因此需要对进程中的线程活动有一个总体了解。</span><span class="sxs-lookup"><span data-stu-id="67b64-136">Since you're looking at a potential hang, you want an overall feel for the thread activity in the process.</span></span> <span data-ttu-id="67b64-137">可以按如下所示使用 `threads` 命令：</span><span class="sxs-lookup"><span data-stu-id="67b64-137">You can use the `threads` command as shown below:</span></span>

```console
> threads
*0 0x1DBFF (121855)
 1 0x1DC01 (121857)
 2 0x1DC02 (121858)
 3 0x1DC03 (121859)
 4 0x1DC04 (121860)
 5 0x1DC05 (121861)
 6 0x1DC06 (121862)
 7 0x1DC07 (121863)
 8 0x1DC08 (121864)
 9 0x1DC09 (121865)
 10 0x1DC0A (121866)
 11 0x1DC0D (121869)
 12 0x1DC0E (121870)
 13 0x1DC10 (121872)
 14 0x1DC11 (121873)
 15 0x1DC12 (121874)
 16 0x1DC13 (121875)
 17 0x1DC14 (121876)
 18 0x1DC15 (121877)
 19 0x1DC1C (121884)
 20 0x1DC1D (121885)
 21 0x1DC1E (121886)
 22 0x1DC21 (121889)
 23 0x1DC22 (121890)
 24 0x1DC23 (121891)
 25 0x1DC24 (121892)
 26 0x1DC25 (121893)
...
...
 317 0x1DD48 (122184)
 318 0x1DD49 (122185)
 319 0x1DD4A (122186)
 320 0x1DD4B (122187)
 321 0x1DD4C (122188)
 ```

<span data-ttu-id="67b64-138">该输出显示进程中当前运行的所有线程及其关联的调试器线程 ID 和操作系统线程 ID。</span><span class="sxs-lookup"><span data-stu-id="67b64-138">The output shows all the threads currently running in the process with their associated debugger thread ID and operating system thread ID.</span></span> <span data-ttu-id="67b64-139">基于输出，有超过 300 个线程。</span><span class="sxs-lookup"><span data-stu-id="67b64-139">Based on the output, there are over 300 threads.</span></span>

<span data-ttu-id="67b64-140">下一步是通过获取每个线程的调用堆栈来更好地了解线程当前正在执行的操作。</span><span class="sxs-lookup"><span data-stu-id="67b64-140">The next step is to get a better understanding of what the threads are currently doing by getting each thread's callstack.</span></span> <span data-ttu-id="67b64-141">`clrstack` 命令可用于输出调用堆栈。</span><span class="sxs-lookup"><span data-stu-id="67b64-141">The `clrstack` command can be used to output callstacks.</span></span> <span data-ttu-id="67b64-142">它既可以输出单个调用堆栈，也可以输出所有调用堆栈。</span><span class="sxs-lookup"><span data-stu-id="67b64-142">It can either output a single callstack or all the callstacks.</span></span> <span data-ttu-id="67b64-143">使用以下命令输出进程中所有线程的所有调用堆栈：</span><span class="sxs-lookup"><span data-stu-id="67b64-143">Use the following command to output all the callstacks for all the threads in the process:</span></span>

```console
clrstack -all
```

<span data-ttu-id="67b64-144">输出的典型部分如下所示：</span><span class="sxs-lookup"><span data-stu-id="67b64-144">A representative portion of the output looks like:</span></span>

```console
  ...
  ...
  ...
        Child SP               IP Call Site
00007F2AE37B5680 00007f305abc6360 [GCFrame: 00007f2ae37b5680]
00007F2AE37B5770 00007f305abc6360 [GCFrame: 00007f2ae37b5770]
00007F2AE37B57D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae37b57d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE37B5920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE37B5950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE37B5CA0 00007f30593044af [GCFrame: 00007f2ae37b5ca0]
00007F2AE37B5D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae37b5d70]
OS Thread Id: 0x1dc82
        Child SP               IP Call Site
00007F2AE2FB4680 00007f305abc6360 [GCFrame: 00007f2ae2fb4680]
00007F2AE2FB4770 00007f305abc6360 [GCFrame: 00007f2ae2fb4770]
00007F2AE2FB47D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae2fb47d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE2FB4920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE2FB4950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE2FB4CA0 00007f30593044af [GCFrame: 00007f2ae2fb4ca0]
00007F2AE2FB4D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae2fb4d70]
OS Thread Id: 0x1dc83
        Child SP               IP Call Site
00007F2AE27B3680 00007f305abc6360 [GCFrame: 00007f2ae27b3680]
00007F2AE27B3770 00007f305abc6360 [GCFrame: 00007f2ae27b3770]
00007F2AE27B37D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae27b37d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE27B3920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE27B3950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE27B3CA0 00007f30593044af [GCFrame: 00007f2ae27b3ca0]
00007F2AE27B3D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae27b3d70]
OS Thread Id: 0x1dc84
        Child SP               IP Call Site
00007F2AE1FB2680 00007f305abc6360 [GCFrame: 00007f2ae1fb2680]
00007F2AE1FB2770 00007f305abc6360 [GCFrame: 00007f2ae1fb2770]
00007F2AE1FB27D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae1fb27d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE1FB2920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE1FB2950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE1FB2CA0 00007f30593044af [GCFrame: 00007f2ae1fb2ca0]
00007F2AE1FB2D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae1fb2d70]
OS Thread Id: 0x1dc85
        Child SP               IP Call Site
00007F2AE17B1680 00007f305abc6360 [GCFrame: 00007f2ae17b1680]
00007F2AE17B1770 00007f305abc6360 [GCFrame: 00007f2ae17b1770]
00007F2AE17B17D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae17b17d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE17B1920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE17B1950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE17B1CA0 00007f30593044af [GCFrame: 00007f2ae17b1ca0]
00007F2AE17B1D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae17b1d70]
OS Thread Id: 0x1dc86
        Child SP               IP Call Site
00007F2AE0FB0680 00007f305abc6360 [GCFrame: 00007f2ae0fb0680]
00007F2AE0FB0770 00007f305abc6360 [GCFrame: 00007f2ae0fb0770]
00007F2AE0FB07D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae0fb07d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE0FB0920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE0FB0950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE0FB0CA0 00007f30593044af [GCFrame: 00007f2ae0fb0ca0]
00007F2AE0FB0D70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae0fb0d70]
OS Thread Id: 0x1dc87
        Child SP               IP Call Site
00007F2AE07AF680 00007f305abc6360 [GCFrame: 00007f2ae07af680]
00007F2AE07AF770 00007f305abc6360 [GCFrame: 00007f2ae07af770]
00007F2AE07AF7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2ae07af7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2AE07AF920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2AE07AF950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2AE07AFCA0 00007f30593044af [GCFrame: 00007f2ae07afca0]
00007F2AE07AFD70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2ae07afd70]
OS Thread Id: 0x1dc88
        Child SP               IP Call Site
00007F2ADFFAE680 00007f305abc6360 [GCFrame: 00007f2adffae680]
00007F2ADFFAE770 00007f305abc6360 [GCFrame: 00007f2adffae770]
00007F2ADFFAE7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2adffae7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2ADFFAE920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2ADFFAE950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2ADFFAECA0 00007f30593044af [GCFrame: 00007f2adffaeca0]
00007F2ADFFAED70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2adffaed70]
...
...
```

<span data-ttu-id="67b64-145">观察所有 300 多个线程的调用堆栈可以发现一个模式，其中大多数线程共享一个公共调用堆栈：</span><span class="sxs-lookup"><span data-stu-id="67b64-145">Observing the callstacks for all 300+ threads shows a pattern where a majority of the threads share a common callstack:</span></span>

```console
OS Thread Id: 0x1dc88
        Child SP               IP Call Site
00007F2ADFFAE680 00007f305abc6360 [GCFrame: 00007f2adffae680]
00007F2ADFFAE770 00007f305abc6360 [GCFrame: 00007f2adffae770]
00007F2ADFFAE7D0 00007f305abc6360 [HelperMethodFrame_1OBJ: 00007f2adffae7d0] System.Threading.Monitor.ReliableEnter(System.Object, Boolean ByRef)
00007F2ADFFAE920 00007F2FE392B31F testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_1() [/home/marioh/webapi/Controllers/diagscenario.cs @ 36]
00007F2ADFFAE950 00007F2FE392B46D System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/__w/3/s/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
00007F2ADFFAECA0 00007f30593044af [GCFrame: 00007f2adffaeca0]
00007F2ADFFAED70 00007f30593044af [DebuggerU2MCatchHandlerFrame: 00007f2adffaed70]
```

<span data-ttu-id="67b64-146">该调用堆栈似乎显示请求传入了死锁方法，而死锁方法继而又调用了 `Monitor.ReliableEnter`。</span><span class="sxs-lookup"><span data-stu-id="67b64-146">The callstack seems to show that the request arrived in our deadlock method that in turn makes a call to `Monitor.ReliableEnter`.</span></span> <span data-ttu-id="67b64-147">此方法表示这些线程正试图进入监视器锁定。</span><span class="sxs-lookup"><span data-stu-id="67b64-147">This method indicates that the threads are trying to enter a monitor lock.</span></span> <span data-ttu-id="67b64-148">它们正在等待该锁定的可用性。</span><span class="sxs-lookup"><span data-stu-id="67b64-148">They're waiting on the availability of the lock.</span></span> <span data-ttu-id="67b64-149">它可能已被其他线程锁定。</span><span class="sxs-lookup"><span data-stu-id="67b64-149">It's likely locked by a different thread.</span></span>

<span data-ttu-id="67b64-150">下一步是找出实际持有监视器锁定的线程。</span><span class="sxs-lookup"><span data-stu-id="67b64-150">The next step then is to find out which thread is actually holding the monitor lock.</span></span> <span data-ttu-id="67b64-151">由于监视器通常将锁定信息存储在同步块表中，因此我们可以使用 `syncblk` 命令来获取更多信息：</span><span class="sxs-lookup"><span data-stu-id="67b64-151">Since monitors typically store lock information in the sync block table, we can use the `syncblk` command to get more information:</span></span>

```console
> syncblk
Index         SyncBlock MonitorHeld Recursion Owning Thread Info          SyncBlock Owner
   43 00000246E51268B8          603         1 0000024B713F4E30 5634  28   00000249654b14c0 System.Object
   44 00000246E5126908            3         1 0000024B713F47E0 51d4  29   00000249654b14d8 System.Object
-----------------------------
Total           344
CCW             1
RCW             2
ComClassFactory 0
Free            0
```

<span data-ttu-id="67b64-152">两个有趣的列是“MonitorHeld”和“Owning Thread Info” 。</span><span class="sxs-lookup"><span data-stu-id="67b64-152">The two interesting columns are **MonitorHeld** and **Owning Thread Info**.</span></span> <span data-ttu-id="67b64-153">“MonitorHeld”列显示线程是否获取了监视器锁定以及正在等待的线程的数量。</span><span class="sxs-lookup"><span data-stu-id="67b64-153">The **MonitorHeld** column shows whether a monitor lock is acquired by a thread and the number of waiting threads.</span></span> <span data-ttu-id="67b64-154">“Owning Thread Info”列显示当前拥有监视器锁定的线程。</span><span class="sxs-lookup"><span data-stu-id="67b64-154">The **Owning Thread Info** column shows which thread currently owns the monitor lock.</span></span> <span data-ttu-id="67b64-155">线程信息有三个不同的子列。</span><span class="sxs-lookup"><span data-stu-id="67b64-155">The thread info has three different subcolumns.</span></span> <span data-ttu-id="67b64-156">第二个子列显示操作系统线程 ID。</span><span class="sxs-lookup"><span data-stu-id="67b64-156">The second subcolumn shows operating system thread ID.</span></span>

<span data-ttu-id="67b64-157">此时，我们知道两个不同的线程（0x5634 和 0x51d4）持有监视器锁定。</span><span class="sxs-lookup"><span data-stu-id="67b64-157">At this point, we know two different threads (0x5634 and 0x51d4) hold a monitor lock.</span></span> <span data-ttu-id="67b64-158">下一步是查看这些线程正在执行的操作。</span><span class="sxs-lookup"><span data-stu-id="67b64-158">The next step is to take a look at what those threads are doing.</span></span> <span data-ttu-id="67b64-159">我们需要检查它们是否无限期陷入持有锁定。</span><span class="sxs-lookup"><span data-stu-id="67b64-159">We need to check if they're stuck indefinitely holding the lock.</span></span> <span data-ttu-id="67b64-160">让我们使用 `setthread` 和 `clrstack` 命令切换到每个线程并显示调用堆栈。</span><span class="sxs-lookup"><span data-stu-id="67b64-160">Let's use the `setthread` and `clrstack` commands to switch to each of the threads and display the callstacks.</span></span>

<span data-ttu-id="67b64-161">若要查看第一个线程，请运行 `setthread` 命令，并找到 0x5634 线程的索引（我们的索引是 28）。</span><span class="sxs-lookup"><span data-stu-id="67b64-161">To look at the first thread, run the `setthread` command, and find the index of the 0x5634 thread (our index was 28).</span></span> <span data-ttu-id="67b64-162">死锁函数正在等待获取某个锁定，但它已拥有该锁定。</span><span class="sxs-lookup"><span data-stu-id="67b64-162">The deadlock function is waiting to acquire a lock, but it already owns the lock.</span></span> <span data-ttu-id="67b64-163">该函数处于正在等待它已经持有的锁定的死锁状态。</span><span class="sxs-lookup"><span data-stu-id="67b64-163">It's in deadlock waiting for the lock it already holds.</span></span>

```console
> setthread 28
> clrstack
OS Thread Id: 0x5634 (28)
        Child SP               IP Call Site
0000004E46AFEAA8 00007fff43a5cbc4 [GCFrame: 0000004e46afeaa8]
0000004E46AFEC18 00007fff43a5cbc4 [GCFrame: 0000004e46afec18]
0000004E46AFEC68 00007fff43a5cbc4 [HelperMethodFrame_1OBJ: 0000004e46afec68] System.Threading.Monitor.Enter(System.Object)
0000004E46AFEDC0 00007FFE5EAF9C80 testwebapi.Controllers.DiagScenarioController.DeadlockFunc() [C:\Users\dapine\Downloads\Diagnostic_scenarios_sample_debug_target\Controllers\DiagnosticScenarios.cs @ 58]
0000004E46AFEE30 00007FFE5EAF9B8C testwebapi.Controllers.DiagScenarioController.<deadlock>b__3_0() [C:\Users\dapine\Downloads\Diagnostic_scenarios_sample_debug_target\Controllers\DiagnosticScenarios.cs @ 26]
0000004E46AFEE80 00007FFEBB251A5B System.Threading.ThreadHelper.ThreadStart_Context(System.Object) [/_/src/System.Private.CoreLib/src/System/Threading/Thread.CoreCLR.cs @ 44]
0000004E46AFEEB0 00007FFE5EAEEEEC System.Threading.ExecutionContext.RunInternal(System.Threading.ExecutionContext, System.Threading.ContextCallback, System.Object) [/_/src/System.Private.CoreLib/shared/System/Threading/ExecutionContext.cs @ 201]
0000004E46AFEF20 00007FFEBB234EAB System.Threading.ThreadHelper.ThreadStart() [/_/src/System.Private.CoreLib/src/System/Threading/Thread.CoreCLR.cs @ 93]
0000004E46AFF138 00007ffebdcc6b63 [GCFrame: 0000004e46aff138]
0000004E46AFF3A0 00007ffebdcc6b63 [DebuggerU2MCatchHandlerFrame: 0000004e46aff3a0]
```

<span data-ttu-id="67b64-164">第二个线程类似。</span><span class="sxs-lookup"><span data-stu-id="67b64-164">The second thread is similar.</span></span> <span data-ttu-id="67b64-165">它还尝试获取已拥有的锁定。</span><span class="sxs-lookup"><span data-stu-id="67b64-165">It's also trying to acquire a lock that it already owns.</span></span> <span data-ttu-id="67b64-166">其余 300 多个正在等待的线程很可能也在等待导致死锁的锁定之一。</span><span class="sxs-lookup"><span data-stu-id="67b64-166">The remaining 300+ threads that are all waiting are most likely also waiting on one of the locks that caused the deadlock.</span></span>

## <a name="see-also"></a><span data-ttu-id="67b64-167">请参阅</span><span class="sxs-lookup"><span data-stu-id="67b64-167">See also</span></span>

- <span data-ttu-id="67b64-168">用于列出进程的 [dotnet-trace](dotnet-trace.md)</span><span class="sxs-lookup"><span data-stu-id="67b64-168">[dotnet-trace](dotnet-trace.md) to list processes</span></span>
- <span data-ttu-id="67b64-169">用于检查托管内存使用情况的 [dotnet-counters](dotnet-counters.md)</span><span class="sxs-lookup"><span data-stu-id="67b64-169">[dotnet-counters](dotnet-counters.md) to check managed memory usage</span></span>
- <span data-ttu-id="67b64-170">用于收集和分析转储文件的 [dotnet-dump](dotnet-dump.md)</span><span class="sxs-lookup"><span data-stu-id="67b64-170">[dotnet-dump](dotnet-dump.md) to collect and analyze a dump file</span></span>
- [<span data-ttu-id="67b64-171">dotnet/diagnostics</span><span class="sxs-lookup"><span data-stu-id="67b64-171">dotnet/diagnostics</span></span>](https://github.com/dotnet/diagnostics/tree/main/documentation/tutorial)

## <a name="next-steps"></a><span data-ttu-id="67b64-172">后续步骤</span><span class="sxs-lookup"><span data-stu-id="67b64-172">Next steps</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="67b64-173">.NET Core 中提供哪些诊断工具</span><span class="sxs-lookup"><span data-stu-id="67b64-173">What diagnostic tools are available in .NET Core</span></span>](index.md)
