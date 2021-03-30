---
title: 调试分析配置设置
description: 了解为 .NET Core 应用配置调试和分析的运行时设置。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: dd96862582f13adc19df7572b1865800b18d9954
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875013"
---
# <a name="run-time-configuration-options-for-debugging-and-profiling"></a><span data-ttu-id="56b9d-103">用于调试和分析的运行时配置选项</span><span class="sxs-lookup"><span data-stu-id="56b9d-103">Run-time configuration options for debugging and profiling</span></span>

## <a name="enable-diagnostics"></a><span data-ttu-id="56b9d-104">启用诊断</span><span class="sxs-lookup"><span data-stu-id="56b9d-104">Enable diagnostics</span></span>

- <span data-ttu-id="56b9d-105">配置是启用还是禁用调试器、探查器和 EventPipe 诊断。</span><span class="sxs-lookup"><span data-stu-id="56b9d-105">Configures whether the debugger, the profiler, and EventPipe diagnostics are enabled or disabled.</span></span>
- <span data-ttu-id="56b9d-106">如果省略此设置，则会启用诊断。</span><span class="sxs-lookup"><span data-stu-id="56b9d-106">If you omit this setting, diagnostics are enabled.</span></span> <span data-ttu-id="56b9d-107">它等效于将值设置为 `1`。</span><span class="sxs-lookup"><span data-stu-id="56b9d-107">This is equivalent to setting the value to `1`.</span></span>

| | <span data-ttu-id="56b9d-108">设置名</span><span class="sxs-lookup"><span data-stu-id="56b9d-108">Setting name</span></span> | <span data-ttu-id="56b9d-109">值</span><span class="sxs-lookup"><span data-stu-id="56b9d-109">Values</span></span> |
| - | - | - |
| <span data-ttu-id="56b9d-110">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="56b9d-110">**runtimeconfig.json**</span></span> | <span data-ttu-id="56b9d-111">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-111">N/A</span></span> | <span data-ttu-id="56b9d-112">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-112">N/A</span></span> |
| <span data-ttu-id="56b9d-113">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-113">**Environment variable**</span></span> | `COMPlus_EnableDiagnostics` | <span data-ttu-id="56b9d-114">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="56b9d-114">`1` - enabled</span></span><br/><span data-ttu-id="56b9d-115">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="56b9d-115">`0` - disabled</span></span> |

## <a name="enable-profiling"></a><span data-ttu-id="56b9d-116">启用分析</span><span class="sxs-lookup"><span data-stu-id="56b9d-116">Enable profiling</span></span>

- <span data-ttu-id="56b9d-117">配置是否为当前正在运行的进程启用分析。</span><span class="sxs-lookup"><span data-stu-id="56b9d-117">Configures whether profiling is enabled for the currently running process.</span></span>
- <span data-ttu-id="56b9d-118">如果省略此设置，则会禁用分析。</span><span class="sxs-lookup"><span data-stu-id="56b9d-118">If you omit this setting, profiling is disabled.</span></span> <span data-ttu-id="56b9d-119">它等效于将值设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="56b9d-119">This is equivalent to setting the value to `0`.</span></span>

| | <span data-ttu-id="56b9d-120">设置名</span><span class="sxs-lookup"><span data-stu-id="56b9d-120">Setting name</span></span> | <span data-ttu-id="56b9d-121">值</span><span class="sxs-lookup"><span data-stu-id="56b9d-121">Values</span></span> |
| - | - | - |
| <span data-ttu-id="56b9d-122">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="56b9d-122">**runtimeconfig.json**</span></span> | <span data-ttu-id="56b9d-123">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-123">N/A</span></span> | <span data-ttu-id="56b9d-124">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-124">N/A</span></span> |
| <span data-ttu-id="56b9d-125">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-125">**Environment variable**</span></span> | `CORECLR_ENABLE_PROFILING` | <span data-ttu-id="56b9d-126">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="56b9d-126">`0` - disabled</span></span><br/><span data-ttu-id="56b9d-127">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="56b9d-127">`1` - enabled</span></span> |

## <a name="profiler-guid"></a><span data-ttu-id="56b9d-128">探查器 GUID</span><span class="sxs-lookup"><span data-stu-id="56b9d-128">Profiler GUID</span></span>

- <span data-ttu-id="56b9d-129">指定要加载到当前正在运行的进程中的探查器 GUID。</span><span class="sxs-lookup"><span data-stu-id="56b9d-129">Specifies the GUID of the profiler to load into the currently running process.</span></span>

| | <span data-ttu-id="56b9d-130">设置名</span><span class="sxs-lookup"><span data-stu-id="56b9d-130">Setting name</span></span> | <span data-ttu-id="56b9d-131">值</span><span class="sxs-lookup"><span data-stu-id="56b9d-131">Values</span></span> |
| - | - | - |
| <span data-ttu-id="56b9d-132">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="56b9d-132">**runtimeconfig.json**</span></span> | <span data-ttu-id="56b9d-133">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-133">N/A</span></span> | <span data-ttu-id="56b9d-134">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-134">N/A</span></span> |
| <span data-ttu-id="56b9d-135">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-135">**Environment variable**</span></span> | `CORECLR_PROFILER` | <span data-ttu-id="56b9d-136">string-guid</span><span class="sxs-lookup"><span data-stu-id="56b9d-136">*string-guid*</span></span> |

## <a name="profiler-location"></a><span data-ttu-id="56b9d-137">探查器位置</span><span class="sxs-lookup"><span data-stu-id="56b9d-137">Profiler location</span></span>

- <span data-ttu-id="56b9d-138">指定要加载到当前正在运行的进程（或 32 位/64 位进程）的探查器 DLL 路径。</span><span class="sxs-lookup"><span data-stu-id="56b9d-138">Specifies the path to the profiler DLL to load into the currently running process (or 32-bit or 64-bit process).</span></span>
- <span data-ttu-id="56b9d-139">如果设置了多个变量，则优先使用指定位数的变量。</span><span class="sxs-lookup"><span data-stu-id="56b9d-139">If more than one variable is set, the bitness-specific variables take precedence.</span></span> <span data-ttu-id="56b9d-140">它们指定要加载的探查器的位数。</span><span class="sxs-lookup"><span data-stu-id="56b9d-140">They specify which bitness of profiler to load.</span></span>
- <span data-ttu-id="56b9d-141">有关详细信息，请参阅 [Finding the profiler library](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/profiling/Profiler%20Loading.md)（查找探查器库）。</span><span class="sxs-lookup"><span data-stu-id="56b9d-141">For more information, see [Finding the profiler library](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/profiling/Profiler%20Loading.md).</span></span>

| | <span data-ttu-id="56b9d-142">设置名</span><span class="sxs-lookup"><span data-stu-id="56b9d-142">Setting name</span></span> | <span data-ttu-id="56b9d-143">值</span><span class="sxs-lookup"><span data-stu-id="56b9d-143">Values</span></span> |
| - | - | - |
| <span data-ttu-id="56b9d-144">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-144">**Environment variable**</span></span> | `CORECLR_PROFILER_PATH` | <span data-ttu-id="56b9d-145">string-path</span><span class="sxs-lookup"><span data-stu-id="56b9d-145">*string-path*</span></span> |
| <span data-ttu-id="56b9d-146">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-146">**Environment variable**</span></span> | `CORECLR_PROFILER_PATH_32` | <span data-ttu-id="56b9d-147">string-path</span><span class="sxs-lookup"><span data-stu-id="56b9d-147">*string-path*</span></span> |
| <span data-ttu-id="56b9d-148">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-148">**Environment variable**</span></span> | `CORECLR_PROFILER_PATH_64` | <span data-ttu-id="56b9d-149">string-path</span><span class="sxs-lookup"><span data-stu-id="56b9d-149">*string-path*</span></span> |

## <a name="write-perf-map"></a><span data-ttu-id="56b9d-150">写入 Perf 映射</span><span class="sxs-lookup"><span data-stu-id="56b9d-150">Write perf map</span></span>

- <span data-ttu-id="56b9d-151">允许或禁止在 Linux 系统上写入 /tmp/perf-$pid.map。</span><span class="sxs-lookup"><span data-stu-id="56b9d-151">Enables or disables writing */tmp/perf-$pid.map* on Linux systems.</span></span>
- <span data-ttu-id="56b9d-152">如果省略此设置，则会禁止写入 Perf 映射。</span><span class="sxs-lookup"><span data-stu-id="56b9d-152">If you omit this setting, writing the perf map is disabled.</span></span> <span data-ttu-id="56b9d-153">它等效于将值设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="56b9d-153">This is equivalent to setting the value to `0`.</span></span>

| | <span data-ttu-id="56b9d-154">设置名</span><span class="sxs-lookup"><span data-stu-id="56b9d-154">Setting name</span></span> | <span data-ttu-id="56b9d-155">值</span><span class="sxs-lookup"><span data-stu-id="56b9d-155">Values</span></span> |
| - | - | - |
| <span data-ttu-id="56b9d-156">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="56b9d-156">**runtimeconfig.json**</span></span> | <span data-ttu-id="56b9d-157">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-157">N/A</span></span> | <span data-ttu-id="56b9d-158">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-158">N/A</span></span> |
| <span data-ttu-id="56b9d-159">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-159">**Environment variable**</span></span> | `COMPlus_PerfMapEnabled` | <span data-ttu-id="56b9d-160">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="56b9d-160">`0` - disabled</span></span><br/><span data-ttu-id="56b9d-161">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="56b9d-161">`1` - enabled</span></span> |

## <a name="perf-log-markers"></a><span data-ttu-id="56b9d-162">性能日志标记</span><span class="sxs-lookup"><span data-stu-id="56b9d-162">Perf log markers</span></span>

- <span data-ttu-id="56b9d-163">允许或禁止在性能日志中将指定信号作为标记予以接受和忽略。</span><span class="sxs-lookup"><span data-stu-id="56b9d-163">Enables or disables the specified signal to be accepted and ignored as a marker in the perf logs.</span></span>
- <span data-ttu-id="56b9d-164">如果省略此设置，则不会忽略指定的信号。</span><span class="sxs-lookup"><span data-stu-id="56b9d-164">If you omit this setting, the specified signal is not ignored.</span></span> <span data-ttu-id="56b9d-165">它等效于将值设置为 `0`。</span><span class="sxs-lookup"><span data-stu-id="56b9d-165">This is equivalent to setting the value to `0`.</span></span>

| | <span data-ttu-id="56b9d-166">设置名</span><span class="sxs-lookup"><span data-stu-id="56b9d-166">Setting name</span></span> | <span data-ttu-id="56b9d-167">值</span><span class="sxs-lookup"><span data-stu-id="56b9d-167">Values</span></span> |
| - | - | - |
| <span data-ttu-id="56b9d-168">**runtimeconfig.json**</span><span class="sxs-lookup"><span data-stu-id="56b9d-168">**runtimeconfig.json**</span></span> | <span data-ttu-id="56b9d-169">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-169">N/A</span></span> | <span data-ttu-id="56b9d-170">不可用</span><span class="sxs-lookup"><span data-stu-id="56b9d-170">N/A</span></span> |
| <span data-ttu-id="56b9d-171">**环境变量**</span><span class="sxs-lookup"><span data-stu-id="56b9d-171">**Environment variable**</span></span> | `COMPlus_PerfMapIgnoreSignal` | <span data-ttu-id="56b9d-172">`0` - 禁用</span><span class="sxs-lookup"><span data-stu-id="56b9d-172">`0` - disabled</span></span><br/><span data-ttu-id="56b9d-173">`1` - 启用</span><span class="sxs-lookup"><span data-stu-id="56b9d-173">`1` - enabled</span></span> |

> [!NOTE]
> <span data-ttu-id="56b9d-174">如果省略 [COMPlus_PerfMapEnabled](#write-perf-map) 或设置为 `0`（即禁用），则将忽略此设置。</span><span class="sxs-lookup"><span data-stu-id="56b9d-174">This setting is ignored if [COMPlus_PerfMapEnabled](#write-perf-map) is omitted or set to `0` (that is, disabled).</span></span>
