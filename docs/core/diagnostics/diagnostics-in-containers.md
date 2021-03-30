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
# <a name="collect-diagnostics-in-containers"></a>收集容器中的诊断

其他场景中用于诊断 .NET Core 问题的相同诊断工具也适用于 Docker 容器。 但是，某些工具需要特殊步骤才能在容器中运行。 本文介绍如何在 Docker 容器中使用收集性能跟踪和收集转储的工具。

## <a name="using-net-core-cli-tools-in-a-container"></a>在容器中使用 .NET Core CLI 工具

**这些工具适用于：✔️** .NET Core 3.0 SDK 及更高版本

.NET Core 全局 CLI 诊断工具（[`dotnet-counters`](dotnet-counters.md)、[`dotnet-dump`](dotnet-dump.md)、[`dotnet-gcdump`](dotnet-gcdump.md) 和 [`dotnet-trace`](dotnet-trace.md)）设计为可在多种环境中工作，应该都可以直接用于 Docker 容器。 因此，这些工具是在容器中收集面向 .NET Core 3.0 或更高版本（对于 `dotnet-gcdump` 则为 3.1 或更高版本）的 .NET Core 方案的诊断信息的首选方法。

在容器中使用这些工具的唯一复杂化的因素是，它们与 .NET Core SDK 一起安装，而许多 Docker 容器运行时在 .NET Core SDK 不存在时也能运行。 解决此问题的一种简单方法是在初始 Docker 映像中安装这些工具。 这些工具不需要 .NET Core SDK 运行，只需安装它即可。 因此，可以通过[多阶段生成](https://docs.docker.com/develop/develop-images/multistage-build/) 来创建 Dockerfile，在生成阶段（.NET Core SDK 存在时）安装工具，然后将二进制文件复制到最终映像中。 此方法的唯一缺点是增加了 Docker 映像大小。

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

另外，还可以在需要时将在容器中安装 .NET Core SDK，以便安装 CLI 工具。 请注意，安装 .NET Core SDK 有重新安装 .NET Core 运行时的副作用。 因此，请确保安装与容器中存在的运行时匹配的 SDK 版本。

### <a name="using-net-core-global-cli-tools-in-a-sidecar-container"></a>在挎斗容器中使用 .NET Core 全局 CLI 工具

如果要使用 .NET Core 全局 CLI 诊断工具来诊断其他容器中的进程，请记住以下附加要求：

1. 容器必须[共享进程命名空间](https://docs.docker.com/engine/reference/run/#pid-settings---pid)（以便挎斗容器中的工具可以访问目标容器中的进程）。
2. .NET Core 全局 CLI 诊断工具需要访问 .NET Core 运行时写入到 /tmp 目录的文件，因此必须通过卷装载在目标容器和挎斗容器之间共享 /tmp 目录。 例如，可以通过让容器共享公用[卷](https://docs.docker.com/storage/volumes/#create-and-manage-volumes)或 Kubernetes [emptyDir](https://kubernetes.io/docs/concepts/storage/volumes/#emptydir) 卷来完成此操作。 如果在不共享 /tmp 目录的情况下尝试使用挎斗容器中的诊断工具，将收到有关进程的错误“未运行兼容的 .NET 运行时”。

## <a name="using-perfcollect-in-a-container"></a>在容器中使用 `PerfCollect`

**此工具适用于：✔️** .NET Core 2.1 及更高版本

[`PerfCollect`](./trace-perfcollect-lttng.md) 脚本对于收集性能跟踪非常有用， .NET Core 3.0 之前建议使用此工具收集跟踪。 如果在容器中使用 `PerfCollect`，请记住以下要求：

1. `PerfCollect` 需要 [`SYS_ADMIN` 功能](https://man7.org/linux/man-pages/man7/capabilities.7.html)（以运行 `perf` 工具），因此请确保[通过该功能](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities)启动容器。
2. `PerfCollect` 需要在其将分析的应用启动之前设置某些环境变量。 可以在 [Dockerfile](https://docs.docker.com/engine/reference/builder/#env) 中设置这些变量，也可以在[启动容器](https://docs.docker.com/engine/reference/run/#env-environment-variables)时设置。 由于在正常生产环境中不应设置这些变量，因此，在启动要分析的容器时添加它们是常见做法。 PerfCollect 需要的两个变量是：
    1. COMPlus_PerfMapEnabled=1
    1. COMPlus_EnableEventLog=1

### <a name="using-perfcollect-in-a-sidecar-container"></a>在挎斗容器中使用 `PerfCollect`

如果要在一个容器中运行 `PerfCollect` 来分析另一容器中的 .NET Core 进程，体验几乎相同，只是有以下差异：

1. 必须为目标容器（而不是运行 `PerfCollect` 的容器）设置前面提到的环境变量（COMPlus_PerfMapEnabled 和 COMPlus_EnableEventLog）。
2. 运行 `PerfCollect` 的容器必须具有 `SYS_ADMIN` 功能（而目标容器则不必）。
3. 这两个容器必须[共享进程命名空间](https://docs.docker.com/engine/reference/run/#pid-settings---pid)。

## <a name="using-createdump-in-a-container"></a>在容器中使用 `createdump`

**此工具适用于：✔️** .NET Core 2.1 及更高版本

[`createdump`](https://github.com/dotnet/runtime/blob/main/docs/design/coreclr/botr/xplat-minidump-generation.md) 是 `dotnet-dump` 的一种替代方法，可用于在包含本机和托管信息的 Linux 上创建核心转储。 `createdump` 工具与 .NET Core 运行时一起安装，可以在 libcoreclr.so（通常位于“/usr/share/dotnet/shared/Microsoft.NETCore.App/[version]”中）旁边找到。 该工具在容器中的使用效果与在非容器化 Linux 环境中相同，唯一的差异是该工具需要 [`SYS_PTRACE` 功能](https://man7.org/linux/man-pages/man7/capabilities.7.html)，因此必须[通过该功能启动](https://docs.docker.com/engine/reference/run/#runtime-privilege-and-linux-capabilities) Docker 容器。

### <a name="using-createdump-in-a-sidecar-container"></a>在挎斗容器中使用 `createdump`

如果要使用 `createdump` 由另一容器中的进程创建转储，体验几乎相同，只是有以下差异：

1. 运行 `createdump` 的容器必须具有 `SYS_PTRACE` 功能（而目标容器则不必）。
2. 这两个容器必须[共享进程命名空间](https://docs.docker.com/engine/reference/run/#pid-settings---pid)。
