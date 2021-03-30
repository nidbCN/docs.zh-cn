---
title: 收集容器中的诊断
description: 在本文中，你将了解如何在 Docker 容器中使用 .NET Core 诊断工具。
ms.date: 09/01/2020
ms.openlocfilehash: 1d0c9eadca348dad5c4fc0a395c8b371e3821262
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872751"
---
# <a name="collect-diagnostics-in-containers"></a><span data-ttu-id="07f8c-103">收集容器中的诊断</span><span class="sxs-lookup"><span data-stu-id="07f8c-103">Collect diagnostics in containers</span></span>

<span data-ttu-id="07f8c-104">其他场景中用于诊断 .NET Core 问题的相同诊断工具也适用于 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="07f8c-104">The same diagnostics tools that are useful for diagnosing .NET Core issues in other scenarios also work in Docker containers.</span></span> <span data-ttu-id="07f8c-105">但是，某些工具需要特殊步骤才能在容器中运行。</span><span class="sxs-lookup"><span data-stu-id="07f8c-105">However, some of the tools require special steps to work in a container.</span></span> <span data-ttu-id="07f8c-106">本文介绍如何在 Docker 容器中使用收集性能跟踪和收集转储的工具。</span><span class="sxs-lookup"><span data-stu-id="07f8c-106">This article covers how tools for gathering performance traces and collecting dumps can be used in Docker containers.</span></span>

## <a name="using-net-core-cli-tools-in-a-container"></a><span data-ttu-id="07f8c-107">在容器中使用 .NET Core CLI 工具</span><span class="sxs-lookup"><span data-stu-id="07f8c-107">Using .NET Core CLI tools in a container</span></span>

<span data-ttu-id="07f8c-108">**这些工具适用于：✔️** .NET Core 3.0 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="07f8c-108">**These tools apply to: ✔️** .NET Core 3.0 SDK and later versions</span></span>

<span data-ttu-id="07f8c-109">.NET Core 全局 CLI 诊断工具（[`dotnet-counters`](dotnet-counters.md)、[`dotnet-dump`](dotnet-dump.md)、[`dotnet-gcdump`](dotnet-gcdump.md) 和 [`dotnet-trace`](dotnet-trace.md)）设计为可在多种环境中工作，应该都可以直接用于 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="07f8c-109">The .NET Core global CLI diagnostic tools ([`dotnet-counters`](dotnet-counters.md), [`dotnet-dump`](dotnet-dump.md), [`dotnet-gcdump`](dotnet-gcdump.md), and [`dotnet-trace`](dotnet-trace.md)) are designed to work in a wide variety of environments and should all work directly in Docker containers.</span></span> <span data-ttu-id="07f8c-110">因此，这些工具是在容器中收集面向 .NET Core 3.0 或更高版本（对于 `dotnet-gcdump` 则为 3.1 或更高版本）的 .NET Core 方案的诊断信息的首选方法。</span><span class="sxs-lookup"><span data-stu-id="07f8c-110">Because of this, these tools are the preferred method of collecting diagnostic information for .NET Core scenarios targeting .NET Core 3.0 or above (or 3.1 or above in the case of `dotnet-gcdump`) in containers.</span></span>

<span data-ttu-id="07f8c-111">在容器中使用这些工具的唯一复杂化的因素是，它们与 .NET Core SDK 一起安装，而许多 Docker 容器运行时在 .NET Core SDK 不存在时也能运行。</span><span class="sxs-lookup"><span data-stu-id="07f8c-111">The only complicating factor of using these tools in a container is that they are installed with the .NET Core SDK and many Docker containers run without the .NET Core SDK present.</span></span> <span data-ttu-id="07f8c-112">解决此问题的一种简单方法是在初始 Docker 映像中安装这些工具。</span><span class="sxs-lookup"><span data-stu-id="07f8c-112">One easy solution to this problem is to install the tools in the initial Docker image.</span></span> <span data-ttu-id="07f8c-113">这些工具不需要 .NET Core SDK 运行，只需安装它即可。</span><span class="sxs-lookup"><span data-stu-id="07f8c-113">The tools don't need the .NET Core SDK to run, only to be installed.</span></span> <span data-ttu-id="07f8c-114">因此，可以通过[多阶段生成](https://docs.docker.com/develop/develop-images/multistage-build/) 来创建 Dockerfile，在生成阶段（.NET Core SDK 存在时）安装工具，然后将二进制文件复制到最终映像中。</span><span class="sxs-lookup"><span data-stu-id="07f8c-114">Therefore, it's possible to create a Dockerfile with a [multi-stage build](https://docs.docker.com/develop/develop-images/multistage-build/) that installs the tools in a build stage (where the .NET Core SDK is present) and then copies the binaries into the final image.</span></span> <span data-ttu-id="07f8c-115">此方法的唯一缺点是增加了 Docker 映像大小。</span><span class="sxs-lookup"><span data-stu-id="07f8c-115">The only downside to this approach is increased Docker image size.</span></span>

```dockerfile
# In build stage
# Install desired .NET CLI diagnostics tools
RUN dotnet tool install --tool-path /tools dotnet-trace
RUN dotnet tool install --tool-path /tools dotnet-counters
RUN dotnet tool install --tool-path /tools dotnet-dump

...

# In final stage
# Copy diagnostics tools
WORKDIR /tools
COPY --from=build /tools .
```

<span data-ttu-id="07f8c-116">另外，还可以在需要时将在容器中安装 .NET Core SDK，以便安装 CLI 工具。</span><span class="sxs-lookup"><span data-stu-id="07f8c-116">Alternatively, the .NET Core SDK can be installed in a container when needed in order to install the CLI tools.</span></span> <span data-ttu-id="07f8c-117">请注意，安装 .NET Core SDK 有重新安装 .NET Core 运行时的副作用。</span><span class="sxs-lookup"><span data-stu-id="07f8c-117">Be aware that installing the .NET Core SDK will have the side-effect of reinstalling the .NET Core runtime.</span></span> <span data-ttu-id="07f8c-118">因此，请确保安装与容器中存在的运行时匹配的 SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="07f8c-118">So be sure to install the version of the SDK that matches the runtime present in the container.</span></span>

### <a name="using-net-core-global-cli-tools-in-a-sidecar-container"></a><span data-ttu-id="07f8c-119">在挎斗容器中使用 .NET Core 全局 CLI 工具</span><span class="sxs-lookup"><span data-stu-id="07f8c-119">Using .NET Core global CLI tools in a sidecar container</span></span>

<span data-ttu-id="07f8c-120">如果要使用 .NET Core 全局 CLI 诊断工具来诊断其他容器中的进程，请记住以下附加要求：</span><span class="sxs-lookup"><span data-stu-id="07f8c-120">If you would like to use .NET Core global CLI diagnostic tools to diagnose processes in a different container, bear the following additional requirements in mind:</span></span>

1. <span data-ttu-id="07f8c-121">容器必须[共享进程命名空间](https://docs.docker.com/engine/reference/run/#pid-settings---pid)（以便挎斗容器中的工具可以访问目标容器中的进程）。</span><span class="sxs-lookup"><span data-stu-id="07f8c-121">The containers must [share a process namespace](https://docs.docker.com/engine/reference/run/#pid-settings---pid) (so that tools in the sidecar container can access processes in the target container).</span></span>
2. <span data-ttu-id="07f8c-122">.NET Core 全局 CLI 诊断工具需要访问 .NET Core 运行时写入到 /tmp 目录的文件，因此必须通过卷装载在目标容器和挎斗容器之间共享 /tmp 目录。</span><span class="sxs-lookup"><span data-stu-id="07f8c-122">The .NET Core global CLI diagnostic tools need access to files the .NET Core runtime writes to the /tmp directory, so the /tmp directory must be shared between the target and sidecar container via a volume mount.</span></span> <span data-ttu-id="07f8c-123">例如，可以通过让容器共享公用[卷](https://docs.docker.com/storage/volumes/#create-and-manage-volumes)或 Kubernetes [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) 卷来完成此操作。</span><span class="sxs-lookup"><span data-stu-id="07f8c-123">This could be done, for example, by having the containers share a common [volume](https://docs.docker.com/storage/volumes/#create-and-manage-volumes) or a Kubernetes [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) volume.</span></span> <span data-ttu-id="07f8c-124">如果在不共享 /tmp 目录的情况下尝试使用挎斗容器中的诊断工具，将收到有关进程的错误“未运行兼容的 .NET 运行时”。</span><span class="sxs-lookup"><span data-stu-id="07f8c-124">If you attempt to use the diagnostic tools from a sidecar container without sharing the /tmp directory, you will get an error about the process "not running compatible .NET runtime."</span></span>

## <a name="using-perfcollect-in-a-container"></a><span data-ttu-id="07f8c-125">在容器中使用 `PerfCollect`</span><span class="sxs-lookup"><span data-stu-id="07f8c-125">Using `PerfCollect` in a container</span></span>

<span data-ttu-id="07f8c-126">**此工具适用于：✔️** .NET Core 2.1 及更高版本</span><span class="sxs-lookup"><span data-stu-id="07f8c-126">**This tool applies to: ✔️** .NET Core 2.1 and later versions</span></span>

<span data-ttu-id="07f8c-127">[`PerfCollect`](./trace-perfcollect-lttng.md) 脚本对于收集性能跟踪非常有用， .NET Core 3.0 之前建议使用此工具收集跟踪。</span><span class="sxs-lookup"><span data-stu-id="07f8c-127">The [`PerfCollect`](./trace-perfcollect-lttng.md) script is useful for collecting performance traces and is the recommended tool for collecting traces prior to .NET Core 3.0.</span></span> <span data-ttu-id="07f8c-128">如果在容器中使用 `PerfCollect`，请记住以下要求：</span><span class="sxs-lookup"><span data-stu-id="07f8c-128">If using `PerfCollect` in a container, keep the following requirements in mind:</span></span>

1. <span data-ttu-id="07f8c-129">`PerfCollect` 需要 [`SYS_ADMIN` 功能](https://man7.org/linux/man-pages/man7/capabilities.7.html)（以运行 `perf` 工具），因此请确保[通过该功能](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)启动容器。</span><span class="sxs-lookup"><span data-stu-id="07f8c-129">`PerfCollect` requires the [`SYS_ADMIN` capability](https://man7.org/linux/man-pages/man7/capabilities.7.html) (in order to run the `perf` tool), so be sure the container is [started with that capability](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities).</span></span>
2. <span data-ttu-id="07f8c-130">`PerfCollect` 需要在其将分析的应用启动之前设置某些环境变量。</span><span class="sxs-lookup"><span data-stu-id="07f8c-130">`PerfCollect` requires some environment variables be set prior to the app it is profiling starting.</span></span> <span data-ttu-id="07f8c-131">可以在 [Dockerfile](https://docs.docker.com/engine/reference/builder/#env) 中设置这些变量，也可以在[启动容器](https://docs.docker.com/engine/reference/run/#env-environment-variables)时设置。</span><span class="sxs-lookup"><span data-stu-id="07f8c-131">These can be set either in a [Dockerfile](https://docs.docker.com/engine/reference/builder/#env) or when [starting the container](https://docs.docker.com/engine/reference/run/#env-environment-variables).</span></span> <span data-ttu-id="07f8c-132">由于在正常生产环境中不应设置这些变量，因此，在启动要分析的容器时添加它们是常见做法。</span><span class="sxs-lookup"><span data-stu-id="07f8c-132">Because these variables shouldn't be set in normal production environments, it's common to just add them when starting a container that will be profiled.</span></span> <span data-ttu-id="07f8c-133">PerfCollect 需要的两个变量是：</span><span class="sxs-lookup"><span data-stu-id="07f8c-133">The two variables which PerfCollect requires are:</span></span>
    1. <span data-ttu-id="07f8c-134">COMPlus_PerfMapEnabled=1</span><span class="sxs-lookup"><span data-stu-id="07f8c-134">COMPlus_PerfMapEnabled=1</span></span>
    1. <span data-ttu-id="07f8c-135">COMPlus_EnableEventLog=1</span><span class="sxs-lookup"><span data-stu-id="07f8c-135">COMPlus_EnableEventLog=1</span></span>

### <a name="using-perfcollect-in-a-sidecar-container"></a><span data-ttu-id="07f8c-136">在挎斗容器中使用 `PerfCollect`</span><span class="sxs-lookup"><span data-stu-id="07f8c-136">Using `PerfCollect` in a sidecar container</span></span>

<span data-ttu-id="07f8c-137">如果要在一个容器中运行 `PerfCollect` 来分析另一容器中的 .NET Core 进程，体验几乎相同，只是有以下差异：</span><span class="sxs-lookup"><span data-stu-id="07f8c-137">If you would like to run `PerfCollect` in one container to profile a .NET Core process in a different container, the experience is almost the same except for these differences:</span></span>

1. <span data-ttu-id="07f8c-138">必须为目标容器（而不是运行 `PerfCollect` 的容器）设置前面提到的环境变量（COMPlus_PerfMapEnabled 和 COMPlus_EnableEventLog）。</span><span class="sxs-lookup"><span data-stu-id="07f8c-138">The environment variables mentioned previously (COMPlus_PerfMapEnabled and COMPlus_EnableEventLog) must be set for the target container (not the one running `PerfCollect`).</span></span>
2. <span data-ttu-id="07f8c-139">运行 `PerfCollect` 的容器必须具有 `SYS_ADMIN` 功能（而目标容器则不必）。</span><span class="sxs-lookup"><span data-stu-id="07f8c-139">The container running `PerfCollect` must have the `SYS_ADMIN` capability (not the target container).</span></span>
3. <span data-ttu-id="07f8c-140">这两个容器必须[共享进程命名空间](https://docs.docker.com/engine/reference/run/#pid-settings---pid)。</span><span class="sxs-lookup"><span data-stu-id="07f8c-140">The two containers must [share a process namespace](https://docs.docker.com/engine/reference/run/#pid-settings---pid).</span></span>

## <a name="using-createdump-in-a-container"></a><span data-ttu-id="07f8c-141">在容器中使用 `createdump`</span><span class="sxs-lookup"><span data-stu-id="07f8c-141">Using `createdump` in a container</span></span>

<span data-ttu-id="07f8c-142">**此工具适用于：✔️** .NET Core 2.1 及更高版本</span><span class="sxs-lookup"><span data-stu-id="07f8c-142">**This tool applies to: ✔️** .NET Core 2.1 and later versions</span></span>

<span data-ttu-id="07f8c-143">[`createdump`](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md) 是 `dotnet-dump` 的一种替代方法，可用于在包含本机和托管信息的 Linux 上创建核心转储。</span><span class="sxs-lookup"><span data-stu-id="07f8c-143">An alternative to `dotnet-dump`, [`createdump`](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md) can be used for creating core dumps on Linux containing both native and managed information.</span></span> <span data-ttu-id="07f8c-144">`createdump` 工具与 .NET Core 运行时一起安装，可以在 libcoreclr.so（通常位于“/usr/share/dotnet/shared/Microsoft.NETCore.App/[version]”中）旁边找到。</span><span class="sxs-lookup"><span data-stu-id="07f8c-144">The `createdump` tool is installed with the .NET Core runtime and can be found next to libcoreclr.so (typically in "/usr/share/dotnet/shared/Microsoft.NETCore.App/[version]").</span></span> <span data-ttu-id="07f8c-145">该工具在容器中的使用效果与在非容器化 Linux 环境中相同，唯一的差异是该工具需要 [`SYS_PTRACE` 功能](https://man7.org/linux/man-pages/man7/capabilities.7.html)，因此必须[通过该功能启动](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="07f8c-145">The tool works the same in a container as it does in non-containerized Linux environments with the single exception that the tool requires the [`SYS_PTRACE` capability](https://man7.org/linux/man-pages/man7/capabilities.7.html), so the Docker container must be [started with that capability](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities).</span></span>

### <a name="using-createdump-in-a-sidecar-container"></a><span data-ttu-id="07f8c-146">在挎斗容器中使用 `createdump`</span><span class="sxs-lookup"><span data-stu-id="07f8c-146">Using `createdump` in a sidecar container</span></span>

<span data-ttu-id="07f8c-147">如果要使用 `createdump` 由另一容器中的进程创建转储，体验几乎相同，只是有以下差异：</span><span class="sxs-lookup"><span data-stu-id="07f8c-147">If you would like to use `createdump` to create a dump from a process in a different container, the experience is almost the same except for these differences:</span></span>

1. <span data-ttu-id="07f8c-148">运行 `createdump` 的容器必须具有 `SYS_PTRACE` 功能（而目标容器则不必）。</span><span class="sxs-lookup"><span data-stu-id="07f8c-148">The container running `createdump` must have the `SYS_PTRACE` capability (not the target container).</span></span>
2. <span data-ttu-id="07f8c-149">这两个容器必须[共享进程命名空间](https://docs.docker.com/engine/reference/run/#pid-settings---pid)。</span><span class="sxs-lookup"><span data-stu-id="07f8c-149">The two containers must [share a process namespace](https://docs.docker.com/engine/reference/run/#pid-settings---pid).</span></span>
