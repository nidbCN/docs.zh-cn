---
title: NETSDK1045：当前的 .NET SDK 不支持将“更新的版本”作为目标。
description: 了解 .NET SDK 错误 NETSDK1045，这是在生成工具找不到请求的 .NET SDK 版本时发生的错误。
ms.topic: error-reference
ms.date: 02/12/2021
f1_keywords:
- NETSDK1045
ms.openlocfilehash: 7f21270fdc7c2db862a49302a302bf8121fc86a5
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102104097"
---
# <a name="netsdk1045-the-current-net-sdk-does-not-support-newer-version-as-a-target"></a><span data-ttu-id="1aa92-103">NETSDK1045：当前的 .NET SDK 不支持将“更新的版本”作为目标。</span><span class="sxs-lookup"><span data-stu-id="1aa92-103">NETSDK1045: The current .NET SDK does not support 'newer version' as a target.</span></span>

<span data-ttu-id="1aa92-104">本文适用于：✔️ .NET Core 2.1.100 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="1aa92-104">**This article applies to:** ✔️ .NET Core 2.1.100 SDK and later versions</span></span>

<span data-ttu-id="1aa92-105">当生成工具找不到生成项目所需的 .NET SDK 版本时，会发生此错误。</span><span class="sxs-lookup"><span data-stu-id="1aa92-105">This error occurs when the build tools can't find the version of the .NET SDK that's needed to build a project.</span></span> <span data-ttu-id="1aa92-106">这通常是由于 .NET Core SDK 安装或配置问题导致的。</span><span class="sxs-lookup"><span data-stu-id="1aa92-106">This is typically due to a .NET Core SDK installation or configuration issue.</span></span> <span data-ttu-id="1aa92-107">完整的错误消息类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="1aa92-107">The full error message is similar to the following example:</span></span>

> <span data-ttu-id="1aa92-108">NETSDK1045：当前的 .NET SDK 不支持将“更新的版本”作为目标。</span><span class="sxs-lookup"><span data-stu-id="1aa92-108">NETSDK1045: The current .NET SDK does not support 'newer version' as a target.</span></span> <span data-ttu-id="1aa92-109">将“更旧的版本”或更低版本作为目标，或者使用支持“更新的版本”的 .NET SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-109">Either target 'older version' or lower, or use a .NET SDK version that supports 'newer version'.</span></span>

<span data-ttu-id="1aa92-110">以下部分介绍此错误的一些可能原因。</span><span class="sxs-lookup"><span data-stu-id="1aa92-110">The following sections describe some of the possible reasons for this error.</span></span> <span data-ttu-id="1aa92-111">查看每个原因并查看哪一项适用于你。</span><span class="sxs-lookup"><span data-stu-id="1aa92-111">Check each one and see which one applies to you.</span></span> <span data-ttu-id="1aa92-112">请记住，在对环境或配置文件进行更改时，可能需要重新启动命令窗口、重新启动 Visual Studio 或重新启动计算机，才能使所做的更改生效。</span><span class="sxs-lookup"><span data-stu-id="1aa92-112">Keep in mind that when making changes to the environment or the configuration files, you might have to restart command windows, restart Visual Studio, or reboot your machine, for your changes to take effect.</span></span>

## <a name="net-sdk-version"></a><span data-ttu-id="1aa92-113">.NET SDK 版本</span><span class="sxs-lookup"><span data-stu-id="1aa92-113">.NET SDK version</span></span>

<span data-ttu-id="1aa92-114">打开项目文件（.csproj、.vbproj 或 .fsproj），并检查目标框架。</span><span class="sxs-lookup"><span data-stu-id="1aa92-114">Open the project file (.csproj, .vbproj, or .fsproj) and check the target framework.</span></span> <span data-ttu-id="1aa92-115">这是应用尝试使用的框架版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-115">This is the version of the framework that your app is trying to use.</span></span>

```xml
<TargetFramework>netcoreapp3.0</TargetFramework>
```

<span data-ttu-id="1aa92-116">确保计算机上已安装列出的 .NET 版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-116">Make sure that the version of .NET listed is installed on the machine.</span></span> <span data-ttu-id="1aa92-117">可以使用以下命令（打开开发人员命令提示并运行此命令）来列出已安装的版本：</span><span class="sxs-lookup"><span data-stu-id="1aa92-117">You can list the installed versions by using the following command (open a Developer Command Prompt and run this command):</span></span>

```dotnetcli
dotnet --list-sdks
```

### <a name="x86-or-x64-architecture"></a><span data-ttu-id="1aa92-118">x86 或 x64 体系结构</span><span class="sxs-lookup"><span data-stu-id="1aa92-118">x86 or x64 architecture</span></span>

<span data-ttu-id="1aa92-119">.NET SDK 的每个版本均可用于 x86 和 x64 体系结构。</span><span class="sxs-lookup"><span data-stu-id="1aa92-119">Each version of the .NET SDK is available in both x86 and x64 architecture.</span></span> <span data-ttu-id="1aa92-120">项目可能会尝试查找适用于错误体系结构的 .NET SDK，或者适用于项目所需体系结构的 .NET SDK 可能未安装。</span><span class="sxs-lookup"><span data-stu-id="1aa92-120">The project might be trying to find the .NET SDK for the wrong architecture, or the .NET SDK for the architecture your project needs might not be installed.</span></span> <span data-ttu-id="1aa92-121">检查所需体系结构的安装文件夹。</span><span class="sxs-lookup"><span data-stu-id="1aa92-121">Check the installation folders for the architecture you need.</span></span> <span data-ttu-id="1aa92-122">例如，在 Windows 上，x86 版本的 .NET SDK 安装在 C:\Program Files (x86)\dotnet 中，而 x64 版本安装在 C:\Program Files\dotnet 中。</span><span class="sxs-lookup"><span data-stu-id="1aa92-122">For example, on Windows, the x86 version of the .NET SDK is installed in *C:\Program Files (x86)\dotnet* and the x64 version is installed in *C:\Program Files\dotnet*.</span></span> <span data-ttu-id="1aa92-123">请参阅[如何检查是否已安装 .NET](../../install/how-to-detect-installed-versions.md) 并选择操作系统，查明如何检测计算机上安装的内容。</span><span class="sxs-lookup"><span data-stu-id="1aa92-123">See [How to check that .NET is already installed](../../install/how-to-detect-installed-versions.md) and choose your operating system to find out how to detect what's installed on your machine.</span></span>

<span data-ttu-id="1aa92-124">如果未安装所需的版本，请在 [.NET 下载](https://dotnet.microsoft.com/download/dotnet)页面找到你需要的版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-124">If the version you need isn't installed, find the one you need at the [.NET Downloads](https://dotnet.microsoft.com/download/dotnet) page.</span></span>

## <a name="preview-not-enabled"></a><span data-ttu-id="1aa92-125">未启用预览版</span><span class="sxs-lookup"><span data-stu-id="1aa92-125">Preview not enabled</span></span>

<span data-ttu-id="1aa92-126">如果已安装所请求的 .NET SDK 版本的预览版，还需要设置用于在 Visual Studio 中启用预览版的选项。</span><span class="sxs-lookup"><span data-stu-id="1aa92-126">If you have a preview installed of the requested .NET SDK version, you also need to set the option to enable previews in Visual Studio.</span></span> <span data-ttu-id="1aa92-127">请转到“工具” > “选项” > “环境” > “预览功能”并确保选中了“使用 .NET Core SDK 的预览版”。</span><span class="sxs-lookup"><span data-stu-id="1aa92-127">Go to **Tools** > **Options** > **Environment** > **Preview Features**, and make sure that **Use previews of the .NET Core SDK** is checked.</span></span>

## <a name="visual-studio-version"></a><span data-ttu-id="1aa92-128">Visual Studio 版本</span><span class="sxs-lookup"><span data-stu-id="1aa92-128">Visual Studio version</span></span>

<span data-ttu-id="1aa92-129">例如，.NET Core 3.0 和更高版本需要 Visual Studio 2019。</span><span class="sxs-lookup"><span data-stu-id="1aa92-129">For example, .NET Core 3.0 and later require Visual Studio 2019.</span></span> <span data-ttu-id="1aa92-130">升级到 [Visual Studio 2019 版本 16.3](https://visualstudio.microsoft.com/downloads) 或更高版本以生成项目。</span><span class="sxs-lookup"><span data-stu-id="1aa92-130">Upgrade to [Visual Studio 2019 version 16.3](https://visualstudio.microsoft.com/downloads) or later to build your project.</span></span>

## <a name="path-environment-variable"></a><span data-ttu-id="1aa92-131">PATH 环境变量</span><span class="sxs-lookup"><span data-stu-id="1aa92-131">PATH environment variable</span></span>

<span data-ttu-id="1aa92-132">生成工具使用 PATH 环境变量查找 .NET 生成工具的正确版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-132">The build tools use the PATH environment variable to find the right version of the .NET build tools.</span></span> <span data-ttu-id="1aa92-133">如果 PATH 环境变量包含指向较旧生成工具的直接路径，则可能出现此错误消息。</span><span class="sxs-lookup"><span data-stu-id="1aa92-133">If the PATH environment variable contains direct paths to older build tools, this error message could appear.</span></span> <span data-ttu-id="1aa92-134">请确保 PATH 环境变量中 .NET 工具的唯一路径为指向顶级 dotnet 文件夹的路径，例如 C:\Program Files\dotnet。</span><span class="sxs-lookup"><span data-stu-id="1aa92-134">Make sure the only path to the .NET tools in the PATH environment variable is to the top-level *dotnet* folder, for example, *C:\Program Files\dotnet*.</span></span> <span data-ttu-id="1aa92-135">错误 PATH 的示例如下所示：C:\Program Files\dotnet\2.1.0\sdks。</span><span class="sxs-lookup"><span data-stu-id="1aa92-135">An example of an incorrect PATH would be something like *C:\Program Files\dotnet\2.1.0\sdks*.</span></span>

## <a name="msbuildsdkpath-environment-variable"></a><span data-ttu-id="1aa92-136">MSBuildSDKPath 环境变量</span><span class="sxs-lookup"><span data-stu-id="1aa92-136">MSBuildSDKPath environment variable</span></span>

<span data-ttu-id="1aa92-137">检查 MSBuildSDKPath 环境变量。</span><span class="sxs-lookup"><span data-stu-id="1aa92-137">Check the MSBuildSDKPath environment variable.</span></span> <span data-ttu-id="1aa92-138">这一可选的环境变量由 MSBuild 识别，如果已设置，则会覆盖默认值。</span><span class="sxs-lookup"><span data-stu-id="1aa92-138">This optional environment variable is recognized by MSBuild and if set, overrides the default value.</span></span> <span data-ttu-id="1aa92-139">它可能会设置为 .NET SDK 的特定版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-139">It might be set to a specific older version of the .NET SDK.</span></span> <span data-ttu-id="1aa92-140">如果已设置它，请尝试删除它并重新生成项目。</span><span class="sxs-lookup"><span data-stu-id="1aa92-140">If it's set, try deleting it and rebuilding your project.</span></span>

## <a name="globaljson-file"></a><span data-ttu-id="1aa92-141">global.json 文件</span><span class="sxs-lookup"><span data-stu-id="1aa92-141">global.json file</span></span>

<span data-ttu-id="1aa92-142">在项目的根文件夹中查找并沿着目录链向上一直到该卷的根目录进行查找，看看是否有 global.json 文件，因为它可存在于此文件夹结构中的任何位置。</span><span class="sxs-lookup"><span data-stu-id="1aa92-142">Check for a *global.json* file in the root folder in your project and up the directory chain to the root of the volume, since it can be anywhere in the folder structure.</span></span> <span data-ttu-id="1aa92-143">如果它包含 SDK 版本，请删除 `sdk` 节点及其所有子节点，或将其更新为所需的较新 .NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="1aa92-143">If it contains an SDK version, delete the `sdk` node and all its children, or update it to the desired newer .NET Core version.</span></span>

```json
{
  "sdk": {
    "version": "2.1.0"
  }
}
```

<span data-ttu-id="1aa92-144">global.json 文件不是必需的，因此，如果除 `sdk` 节点以外，该文件未包含任何内容，则可以删除整个文件。</span><span class="sxs-lookup"><span data-stu-id="1aa92-144">The *global.json* file is not required, so if it doesn't contain anything other than the `sdk` node, you can delete the whole file.</span></span>

## <a name="directorybuildprops-file"></a><span data-ttu-id="1aa92-145">Directory.build.props 文件</span><span class="sxs-lookup"><span data-stu-id="1aa92-145">Directory.build.props file</span></span>

<span data-ttu-id="1aa92-146">Directory.build.props 文件是可选的 MSBuild 文件，它可以设置全局属性。</span><span class="sxs-lookup"><span data-stu-id="1aa92-146">The *Directory.build.props* file is an optional MSBuild file that can set global properties.</span></span> <span data-ttu-id="1aa92-147">在项目的根文件夹中查找并沿着目录链向上一直到该卷的根目录进行查找，看看是否有这些文件，因为它们可存在于此文件夹结构中的任何位置。</span><span class="sxs-lookup"><span data-stu-id="1aa92-147">Check for these files in the solution folder and up the directory chain to the root of the volume, since they can be anywhere in the folder structure.</span></span> <span data-ttu-id="1aa92-148">查找 `TargetFramework` 元素或 `MSBuildSDKPath` 的设置，这些可能会覆盖所需的设置。</span><span class="sxs-lookup"><span data-stu-id="1aa92-148">Look for `TargetFramework` elements, or settings of `MSBuildSDKPath` that could override your desired settings.</span></span>

## <a name="see-also"></a><span data-ttu-id="1aa92-149">请参阅</span><span class="sxs-lookup"><span data-stu-id="1aa92-149">See also</span></span>

- <span data-ttu-id="1aa92-150">[The Current .NET SDK does not support targeting .NET Core 3.0 – Fix](https://www.ryadel.com/current-net-sdk-not-support-net-core-3-0-fix/)（当前的 .NET SDK 不支持将 .NET Core 3.0 作为目标 – 解决）</span><span class="sxs-lookup"><span data-stu-id="1aa92-150">[The Current .NET SDK does not support targeting .NET Core 3.0 – Fix](https://www.ryadel.com/current-net-sdk-not-support-net-core-3-0-fix/)</span></span>
