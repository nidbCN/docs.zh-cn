---
title: .NET 运行时标识符 (RID) 目录
description: 了解运行时标识符 (RID) 及如何在 .NET 中使用 RID。
ms.date: 01/28/2021
ms.openlocfilehash: 15a2be8d3c2a903011e298d99c838baef731b9c2
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875091"
---
# <a name="net-rid-catalog"></a><span data-ttu-id="f1c76-103">.NET RID 目录</span><span class="sxs-lookup"><span data-stu-id="f1c76-103">.NET RID Catalog</span></span>

<span data-ttu-id="f1c76-104">RID 是运行时标识符的缩写。</span><span class="sxs-lookup"><span data-stu-id="f1c76-104">RID is short for *Runtime Identifier*.</span></span> <span data-ttu-id="f1c76-105">RID 值用于标识应用程序运行所在的目标平台。</span><span class="sxs-lookup"><span data-stu-id="f1c76-105">RID values are used to identify target platforms where the application runs.</span></span>
<span data-ttu-id="f1c76-106">.NET 包使用它们来表示 NuGet 包中特定于平台的资产。</span><span class="sxs-lookup"><span data-stu-id="f1c76-106">They're used by .NET packages to represent platform-specific assets in NuGet packages.</span></span> <span data-ttu-id="f1c76-107">以下值是 RID 的示例：`linux-x64`、`ubuntu.14.04-x64`、`win7-x64` 或 `osx.10.12-x64`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-107">The following values are examples of RIDs: `linux-x64`, `ubuntu.14.04-x64`, `win7-x64`, or `osx.10.12-x64`.</span></span>
<span data-ttu-id="f1c76-108">对于具有本机依赖项的包，RID 将指定在其中可以还原包的平台。</span><span class="sxs-lookup"><span data-stu-id="f1c76-108">For the packages with native dependencies, the RID designates on which platforms the package can be restored.</span></span>

<span data-ttu-id="f1c76-109">可以在项目文件的 `<RuntimeIdentifier>` 元素中设置一个 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-109">A single RID can be set in the `<RuntimeIdentifier>` element of your project file.</span></span> <span data-ttu-id="f1c76-110">可以将多个 RID 定义为项目文件的 `<RuntimeIdentifiers>` 元素中的列表（以分号分隔）。</span><span class="sxs-lookup"><span data-stu-id="f1c76-110">Multiple RIDs can be defined as a semicolon-delimited list in the project file's `<RuntimeIdentifiers>` element.</span></span> <span data-ttu-id="f1c76-111">也可使用以下 [.NET CLI 命令](./tools/index.md) 通过 `--runtime` 选项使用它们：</span><span class="sxs-lookup"><span data-stu-id="f1c76-111">They're also used via the `--runtime` option with the following [.NET CLI commands](./tools/index.md):</span></span>

- [<span data-ttu-id="f1c76-112">dotnet build</span><span class="sxs-lookup"><span data-stu-id="f1c76-112">dotnet build</span></span>](./tools/dotnet-build.md)
- [<span data-ttu-id="f1c76-113">dotnet clean</span><span class="sxs-lookup"><span data-stu-id="f1c76-113">dotnet clean</span></span>](./tools/dotnet-clean.md)
- [<span data-ttu-id="f1c76-114">dotnet pack</span><span class="sxs-lookup"><span data-stu-id="f1c76-114">dotnet pack</span></span>](./tools/dotnet-pack.md)
- [<span data-ttu-id="f1c76-115">dotnet publish</span><span class="sxs-lookup"><span data-stu-id="f1c76-115">dotnet publish</span></span>](./tools/dotnet-publish.md)
- [<span data-ttu-id="f1c76-116">dotnet restore</span><span class="sxs-lookup"><span data-stu-id="f1c76-116">dotnet restore</span></span>](./tools/dotnet-restore.md)
- [<span data-ttu-id="f1c76-117">dotnet run</span><span class="sxs-lookup"><span data-stu-id="f1c76-117">dotnet run</span></span>](./tools/dotnet-run.md)
- [<span data-ttu-id="f1c76-118">dotnet store</span><span class="sxs-lookup"><span data-stu-id="f1c76-118">dotnet store</span></span>](./tools/dotnet-store.md)

<span data-ttu-id="f1c76-119">表示具体操作系统的 RID 通常遵循以下模式：`[os].[version]-[architecture]-[additional qualifiers]`，其中：</span><span class="sxs-lookup"><span data-stu-id="f1c76-119">RIDs that represent concrete operating systems usually follow this pattern: `[os].[version]-[architecture]-[additional qualifiers]` where:</span></span>

- <span data-ttu-id="f1c76-120">`[os]` 是操作系统/平台系统名字对象。</span><span class="sxs-lookup"><span data-stu-id="f1c76-120">`[os]` is the operating/platform system moniker.</span></span> <span data-ttu-id="f1c76-121">例如 `ubuntu`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-121">For example, `ubuntu`.</span></span>

- <span data-ttu-id="f1c76-122">`[version]` 是操作系统版本，使用的格式是以点 (`.`) 分隔的版本号。</span><span class="sxs-lookup"><span data-stu-id="f1c76-122">`[version]` is the operating system version in the form of a dot-separated (`.`) version number.</span></span> <span data-ttu-id="f1c76-123">例如 `15.10`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-123">For example, `15.10`.</span></span>

  - <span data-ttu-id="f1c76-124">版本不应为营销版本，因为它们通常代表该操作系统的多个离散版本，且具有不同的平台 API 外围应用。</span><span class="sxs-lookup"><span data-stu-id="f1c76-124">The version **shouldn't** be marketing versions, as they often represent multiple discrete versions of the operating system with varying platform API surface area.</span></span>

- <span data-ttu-id="f1c76-125">`[architecture]` 是处理器体系结构。</span><span class="sxs-lookup"><span data-stu-id="f1c76-125">`[architecture]` is the processor architecture.</span></span> <span data-ttu-id="f1c76-126">例如：`x86`、`x64`、`arm` 或 `arm64`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-126">For example: `x86`, `x64`, `arm`, or `arm64`.</span></span>

- <span data-ttu-id="f1c76-127">`[additional qualifiers]` 进一步区分了不同的平台。</span><span class="sxs-lookup"><span data-stu-id="f1c76-127">`[additional qualifiers]` further differentiate different platforms.</span></span> <span data-ttu-id="f1c76-128">例如：`aot`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-128">For example: `aot`.</span></span>

## <a name="rid-graph"></a><span data-ttu-id="f1c76-129">RID 图表</span><span class="sxs-lookup"><span data-stu-id="f1c76-129">RID graph</span></span>

<span data-ttu-id="f1c76-130">RID 图表或运行时回退图表是互相兼容的 RID 列表。</span><span class="sxs-lookup"><span data-stu-id="f1c76-130">The RID graph or runtime fallback graph is a list of RIDs that are compatible with each other.</span></span> <span data-ttu-id="f1c76-131">[Microsoft.NETCore.Platforms](https://www.nuget.org/packages/Microsoft.NETCore.Platforms/) 包中定义了 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-131">The RIDs are defined in the [Microsoft.NETCore.Platforms](https://www.nuget.org/packages/Microsoft.NETCore.Platforms/) package.</span></span> <span data-ttu-id="f1c76-132">可以在 `dotnet/runtime` 存储库的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) 文件中查看支持的 RID 列表和 RID 图表。</span><span class="sxs-lookup"><span data-stu-id="f1c76-132">You can see the list of supported RIDs and the RID graph in the [*runtime.json*](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) file, which is located in the `dotnet/runtime` repository.</span></span> <span data-ttu-id="f1c76-133">在此文件中，可以看到除基 RID 以外的所有 RID 均包含 `"#import"` 语句。</span><span class="sxs-lookup"><span data-stu-id="f1c76-133">In this file, you can see that all RIDs, except for the base one, contain an `"#import"` statement.</span></span> <span data-ttu-id="f1c76-134">这些语句指示的是兼容的 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-134">These statements indicate compatible RIDs.</span></span>

<span data-ttu-id="f1c76-135">NuGet 还原包时，它将尝试找到指定运行时的完全匹配项。</span><span class="sxs-lookup"><span data-stu-id="f1c76-135">When NuGet restores packages, it tries to find an exact match for the specified runtime.</span></span>
<span data-ttu-id="f1c76-136">如果未找到完全匹配项，NuGet 将返回此图表，直至它根据 RID 图表找到最相近的兼容系统。</span><span class="sxs-lookup"><span data-stu-id="f1c76-136">If an exact match is not found, NuGet walks back the graph until it finds the closest compatible system according to the RID graph.</span></span>

<span data-ttu-id="f1c76-137">以下示例是 `osx.10.12-x64` RID 的实际条目：</span><span class="sxs-lookup"><span data-stu-id="f1c76-137">The following example is the actual entry for the `osx.10.12-x64` RID:</span></span>

```json
"osx.10.12-x64": {
    "#import": [ "osx.10.12", "osx.10.11-x64" ]
}
```

<span data-ttu-id="f1c76-138">上述 RID 指定 `osx.10.12-x64` 导入 `osx.10.11-x64`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-138">The above RID specifies that `osx.10.12-x64` imports `osx.10.11-x64`.</span></span> <span data-ttu-id="f1c76-139">因此，当 NuGet 还原包时，它将尝试找到包中的 `osx.10.12-x64` 的完全匹配项。</span><span class="sxs-lookup"><span data-stu-id="f1c76-139">So, when NuGet restores packages, it tries to find an exact match for  `osx.10.12-x64` in the package.</span></span> <span data-ttu-id="f1c76-140">例如，如果 NuGet 无法找到特定的运行时，可以还原指定 `osx.10.11-x64` 运行时的包。</span><span class="sxs-lookup"><span data-stu-id="f1c76-140">If NuGet cannot find the specific runtime, it can restore packages that specify `osx.10.11-x64` runtimes, for example.</span></span>

<span data-ttu-id="f1c76-141">以下示例演示了 runtime.json 文件中定义的另一个略大的 RID 图表：</span><span class="sxs-lookup"><span data-stu-id="f1c76-141">The following example shows a slightly bigger RID graph also defined in the *runtime.json*  file:</span></span>

```
    win7-x64    win7-x86
       |   \   /    |
       |   win7     |
       |     |      |
    win-x64  |  win-x86
          \  |  /
            win
             |
            any
```

<span data-ttu-id="f1c76-142">所有 RID 最终都会映射回根 `any` RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-142">All RIDs eventually map back to the root `any` RID.</span></span>

<span data-ttu-id="f1c76-143">使用 RID 时，必须牢记以下几个注意事项：</span><span class="sxs-lookup"><span data-stu-id="f1c76-143">There are some considerations about RIDs that you have to keep in mind when working with them:</span></span>

- <span data-ttu-id="f1c76-144">请勿尝试分析 RID 来检索组件部分。</span><span class="sxs-lookup"><span data-stu-id="f1c76-144">Don't try to parse RIDs to retrieve component parts.</span></span>
- <span data-ttu-id="f1c76-145">请勿以编程方式生成 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-145">Don't build RIDs programmatically.</span></span>
- <span data-ttu-id="f1c76-146">使用已为平台定义的 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-146">Use RIDs that are already defined for the platform.</span></span>
- <span data-ttu-id="f1c76-147">RID 必须具有特定性，因此请勿通过实际的 RID 值假定任何情况。</span><span class="sxs-lookup"><span data-stu-id="f1c76-147">The RIDs need to be specific, so don't assume anything from the actual RID value.</span></span>

## <a name="using-rids"></a><span data-ttu-id="f1c76-148">使用 RID</span><span class="sxs-lookup"><span data-stu-id="f1c76-148">Using RIDs</span></span>

<span data-ttu-id="f1c76-149">若要使用 RID，必须知道有哪些 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-149">To be able to use RIDs, you have to know which RIDs exist.</span></span> <span data-ttu-id="f1c76-150">新值将定期添加到该平台。</span><span class="sxs-lookup"><span data-stu-id="f1c76-150">New values are added regularly to the platform.</span></span>
<span data-ttu-id="f1c76-151">若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) 文件。</span><span class="sxs-lookup"><span data-stu-id="f1c76-151">For the latest and complete version, see the [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) file in the `dotnet/runtime` repository.</span></span>

<span data-ttu-id="f1c76-152">可移植 RID 是添加到 RID 图表的值，未绑定到特定的版本或 OS 发行。</span><span class="sxs-lookup"><span data-stu-id="f1c76-152">Portable RIDs are values added to the RID graph that aren't tied to a specific version or OS distribution.</span></span> <span data-ttu-id="f1c76-153">它们是首选选项，尤其是在处理多个 Linux 发行版时，因为大多数发行版 RID 都映射到可移植的 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-153">They are the preferred choice, especially when dealing with multiple Linux distros since most distribution RIDs are mapped to the portable RIDs.</span></span>

<span data-ttu-id="f1c76-154">以下列表显示了一小部分用于每个 OS 的最常见 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-154">The following list shows a small subset of the most common RIDs used for each OS.</span></span>

## <a name="windows-rids"></a><span data-ttu-id="f1c76-155">Windows RID</span><span class="sxs-lookup"><span data-stu-id="f1c76-155">Windows RIDs</span></span>

<span data-ttu-id="f1c76-156">仅列出了公共值。</span><span class="sxs-lookup"><span data-stu-id="f1c76-156">Only common values are listed.</span></span> <span data-ttu-id="f1c76-157">若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) 文件。</span><span class="sxs-lookup"><span data-stu-id="f1c76-157">For the latest and complete version, see the [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) file in the `dotnet/runtime` repository.</span></span>

- <span data-ttu-id="f1c76-158">可移植</span><span class="sxs-lookup"><span data-stu-id="f1c76-158">Portable</span></span>
  - `win-x64`
  - `win-x86`
  - `win-arm`
  - `win-arm64`
- <span data-ttu-id="f1c76-159">Windows 7 / Windows Server 2008 R2</span><span class="sxs-lookup"><span data-stu-id="f1c76-159">Windows 7 / Windows Server 2008 R2</span></span>
  - `win7-x64`
  - `win7-x86`
- <span data-ttu-id="f1c76-160">Windows 8.1 / Windows Server 2012 R2</span><span class="sxs-lookup"><span data-stu-id="f1c76-160">Windows 8.1 / Windows Server 2012 R2</span></span>
  - `win81-x64`
  - `win81-x86`
  - `win81-arm`
- <span data-ttu-id="f1c76-161">Windows 10 / Windows Server 2016</span><span class="sxs-lookup"><span data-stu-id="f1c76-161">Windows 10 / Windows Server 2016</span></span>
  - `win10-x64`
  - `win10-x86`
  - `win10-arm`
  - `win10-arm64`

<span data-ttu-id="f1c76-162">有关详细信息，请参阅 [.NET 依赖项和要求](./install/windows.md#dependencies)。</span><span class="sxs-lookup"><span data-stu-id="f1c76-162">For more information, see [.NET dependencies and requirements](./install/windows.md#dependencies).</span></span>

## <a name="linux-rids"></a><span data-ttu-id="f1c76-163">Linux RID</span><span class="sxs-lookup"><span data-stu-id="f1c76-163">Linux RIDs</span></span>

<span data-ttu-id="f1c76-164">仅列出了公共值。</span><span class="sxs-lookup"><span data-stu-id="f1c76-164">Only common values are listed.</span></span> <span data-ttu-id="f1c76-165">若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) 文件。</span><span class="sxs-lookup"><span data-stu-id="f1c76-165">For the latest and complete version, see the [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) file in the `dotnet/runtime` repository.</span></span> <span data-ttu-id="f1c76-166">运行以下未列出的发行版的设备可能适用于其中一个可移植 RID。</span><span class="sxs-lookup"><span data-stu-id="f1c76-166">Devices running a distribution not listed below may work with one of the Portable RIDs.</span></span> <span data-ttu-id="f1c76-167">例如，可以将运行未列出的 Linux 发行版的 Raspberry Pi 设备定向为使用 `linux-arm`。</span><span class="sxs-lookup"><span data-stu-id="f1c76-167">For example, Raspberry Pi devices running a Linux distribution not listed can be targeted with `linux-arm`.</span></span>

- <span data-ttu-id="f1c76-168">可移植</span><span class="sxs-lookup"><span data-stu-id="f1c76-168">Portable</span></span>
  - <span data-ttu-id="f1c76-169">`linux-x64`（大多数桌面发行版，如 CentOS、Debian、Fedora、Ubuntu 及派生版本）</span><span class="sxs-lookup"><span data-stu-id="f1c76-169">`linux-x64` (Most desktop distributions like CentOS, Debian, Fedora, Ubuntu, and derivatives)</span></span>
  - <span data-ttu-id="f1c76-170">`linux-musl-x64`（使用 [musl](https://wiki.musl-libc.org/projects-using-musl.html) 的轻量级发行版，如 Alpine Linux）</span><span class="sxs-lookup"><span data-stu-id="f1c76-170">`linux-musl-x64` (Lightweight distributions using [musl](https://wiki.musl-libc.org/projects-using-musl.html) like Alpine Linux)</span></span>
  - <span data-ttu-id="f1c76-171">`linux-arm`（在 ARM 上运行的 Linux 发行版本，如 Raspberry Pi Model 2 及更高版本上的 Raspbian）</span><span class="sxs-lookup"><span data-stu-id="f1c76-171">`linux-arm` (Linux distributions running on ARM like Raspbian on Raspberry Pi Model 2+)</span></span>
  - <span data-ttu-id="f1c76-172">`linux-arm64`（在 64 位 ARM 上运行的 Linux 发行版本，如 Raspberry Pi Model 3 及更高版本上的 Ubuntu 服务器 64 位）</span><span class="sxs-lookup"><span data-stu-id="f1c76-172">`linux-arm64` (Linux distributions running on 64-bit ARM like Ubuntu Server 64-bit on Raspberry Pi Model 3+)</span></span>
- <span data-ttu-id="f1c76-173">Red Hat Enterprise Linux</span><span class="sxs-lookup"><span data-stu-id="f1c76-173">Red Hat Enterprise Linux</span></span>
  - <span data-ttu-id="f1c76-174">`rhel-x64`（被 `linux-x64` 取代，适用于 RHEL 6 以上版本）</span><span class="sxs-lookup"><span data-stu-id="f1c76-174">`rhel-x64` (Superseded by `linux-x64` for RHEL above version 6)</span></span>
  - `rhel.6-x64`
- <span data-ttu-id="f1c76-175">Tizen</span><span class="sxs-lookup"><span data-stu-id="f1c76-175">Tizen</span></span>
  - `tizen`
  - `tizen.4.0.0`
  - `tizen.5.0.0`

<span data-ttu-id="f1c76-176">有关详细信息，请参阅 [.NET 依赖项和要求](./install/linux.md)。</span><span class="sxs-lookup"><span data-stu-id="f1c76-176">For more information, see [.NET dependencies and requirements](./install/linux.md).</span></span>

## <a name="macos-rids"></a><span data-ttu-id="f1c76-177">macOS RID</span><span class="sxs-lookup"><span data-stu-id="f1c76-177">macOS RIDs</span></span>

<span data-ttu-id="f1c76-178">macOS RID 使用较早的“OSX”品牌。</span><span class="sxs-lookup"><span data-stu-id="f1c76-178">macOS RIDs use the older "OSX" branding.</span></span> <span data-ttu-id="f1c76-179">仅列出了公共值。</span><span class="sxs-lookup"><span data-stu-id="f1c76-179">Only common values are listed.</span></span> <span data-ttu-id="f1c76-180">若要获取最新的完整版，请参阅 `dotnet/runtime` 存储库中的 [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) 文件。</span><span class="sxs-lookup"><span data-stu-id="f1c76-180">For the latest and complete version, see the [runtime.json](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/pkg/runtime.json) file in the `dotnet/runtime` repository.</span></span>

- <span data-ttu-id="f1c76-181">可移植</span><span class="sxs-lookup"><span data-stu-id="f1c76-181">Portable</span></span>
  - <span data-ttu-id="f1c76-182">`osx-x64`（最低 OS 版本为 macOS 10.12 Sierra）</span><span class="sxs-lookup"><span data-stu-id="f1c76-182">`osx-x64` (Minimum OS version is macOS 10.12 Sierra)</span></span>
- <span data-ttu-id="f1c76-183">macOS 10.10  Yosemite</span><span class="sxs-lookup"><span data-stu-id="f1c76-183">macOS 10.10  Yosemite</span></span>
  - `osx.10.10-x64`
- <span data-ttu-id="f1c76-184">macOS 10.11 El Capitan</span><span class="sxs-lookup"><span data-stu-id="f1c76-184">macOS 10.11 El Capitan</span></span>
  - `osx.10.11-x64`
- <span data-ttu-id="f1c76-185">macOS 10.12 Sierra</span><span class="sxs-lookup"><span data-stu-id="f1c76-185">macOS 10.12 Sierra</span></span>
  - `osx.10.12-x64`
- <span data-ttu-id="f1c76-186">macOS 10.13 High Sierra</span><span class="sxs-lookup"><span data-stu-id="f1c76-186">macOS 10.13 High Sierra</span></span>
  - `osx.10.13-x64`
- <span data-ttu-id="f1c76-187">macOS 10.14 Mojave</span><span class="sxs-lookup"><span data-stu-id="f1c76-187">macOS 10.14 Mojave</span></span>
  - `osx.10.14-x64`
- <span data-ttu-id="f1c76-188">macOS 10.15 Catalina</span><span class="sxs-lookup"><span data-stu-id="f1c76-188">macOS 10.15 Catalina</span></span>
  - `osx.10.15-x64`
- <span data-ttu-id="f1c76-189">macOS 11.01 Big Sur</span><span class="sxs-lookup"><span data-stu-id="f1c76-189">macOS 11.01 Big Sur</span></span>
  - `osx.11.0-x64`
  - `osx.11.0-arm64`

<span data-ttu-id="f1c76-190">有关详细信息，请参阅 [.NET 依赖项和要求](./install/macos.md#dependencies)。</span><span class="sxs-lookup"><span data-stu-id="f1c76-190">For more information, see [.NET dependencies and requirements](./install/macos.md#dependencies).</span></span>

## <a name="see-also"></a><span data-ttu-id="f1c76-191">请参阅</span><span class="sxs-lookup"><span data-stu-id="f1c76-191">See also</span></span>

- [<span data-ttu-id="f1c76-192">运行时 ID</span><span class="sxs-lookup"><span data-stu-id="f1c76-192">Runtime IDs</span></span>](https://github.com/dotnet/runtime/blob/main/src/libraries/Microsoft.NETCore.Platforms/readme.md)
