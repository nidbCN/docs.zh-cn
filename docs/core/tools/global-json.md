---
title: global.json 概述
description: 了解如何在运行 .NET CLI 命令时使用 global.json 文件设置 .NET SDK 版本。
ms.topic: how-to
ms.date: 05/01/2020
ms.custom: updateeachrelease
ms.openlocfilehash: cc471cf5b50cf91c38b46607ccf38bd4d087aa6a
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102104136"
---
# <a name="globaljson-overview"></a><span data-ttu-id="15372-103">global.json 概述</span><span class="sxs-lookup"><span data-stu-id="15372-103">global.json overview</span></span>

<span data-ttu-id="15372-104">本文适用于： ✔️ .NET Core 2.0 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="15372-104">**This article applies to:** ✔️ .NET Core 2.0 SDK and later versions</span></span>

<span data-ttu-id="15372-105">通过 global.json 文件，可定义在运行 .NET CLI 命令时使用的 .NET SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="15372-105">The *global.json* file allows you to define which .NET SDK version is used when you run .NET CLI commands.</span></span> <span data-ttu-id="15372-106">选择 .NET SDK 与指定项目所面向的运行时无关。</span><span class="sxs-lookup"><span data-stu-id="15372-106">Selecting the .NET SDK is independent from specifying the runtime your project targets.</span></span> <span data-ttu-id="15372-107">.NET SDK 版本指示使用哪个版本的 .NET CLI。</span><span class="sxs-lookup"><span data-stu-id="15372-107">The .NET SDK version indicates which versions of the .NET CLI is used.</span></span>

<span data-ttu-id="15372-108">通常会使用最新版 SDK 工具，因此不需要 global.json 文件。</span><span class="sxs-lookup"><span data-stu-id="15372-108">In general, you want to use the latest version of the SDK tools, so no *global.json* file is needed.</span></span> <span data-ttu-id="15372-109">在某些高级方案中，你可能需要控制 SDK 工具的版本，本文对如何完成此操作进行了介绍。</span><span class="sxs-lookup"><span data-stu-id="15372-109">In some advanced scenarios, you might want to control the version of the SDK tools, and this article explains how to do this.</span></span>

<span data-ttu-id="15372-110">有关指定运行时的详细信息，请参阅[目标框架](../../standard/frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="15372-110">For more information about specifying the runtime instead, see [Target frameworks](../../standard/frameworks.md).</span></span>

<span data-ttu-id="15372-111">.NET SDK 在当前工作目录（不必与项目目录相同）或其某个父目录中查找 global.json 文件。</span><span class="sxs-lookup"><span data-stu-id="15372-111">The .NET SDK looks for a *global.json* file in the current working directory (which isn't necessarily the same as the project directory) or one of its parent directories.</span></span>

## <a name="globaljson-schema"></a><span data-ttu-id="15372-112">global.json 架构</span><span class="sxs-lookup"><span data-stu-id="15372-112">global.json schema</span></span>

### <a name="sdk"></a><span data-ttu-id="15372-113">SDK</span><span class="sxs-lookup"><span data-stu-id="15372-113">sdk</span></span>

<span data-ttu-id="15372-114">类型：`object`</span><span class="sxs-lookup"><span data-stu-id="15372-114">Type: `object`</span></span>

<span data-ttu-id="15372-115">指定要选择的 .NET SDK 的相关信息。</span><span class="sxs-lookup"><span data-stu-id="15372-115">Specifies information about the .NET SDK to select.</span></span>

#### <a name="version"></a><span data-ttu-id="15372-116">version</span><span class="sxs-lookup"><span data-stu-id="15372-116">version</span></span>

- <span data-ttu-id="15372-117">类型：`string`</span><span class="sxs-lookup"><span data-stu-id="15372-117">Type: `string`</span></span>

- <span data-ttu-id="15372-118">自 .NET Core 1.0 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="15372-118">Available since: .NET Core 1.0 SDK.</span></span>

<span data-ttu-id="15372-119">要使用的 .NET SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="15372-119">The version of the .NET SDK to use.</span></span>

<span data-ttu-id="15372-120">此字段：</span><span class="sxs-lookup"><span data-stu-id="15372-120">This field:</span></span>

- <span data-ttu-id="15372-121">不支持通配符，也就是说，必须指定完整版本号。</span><span class="sxs-lookup"><span data-stu-id="15372-121">Doesn't have wildcard support, that is, the full version number has to be specified.</span></span>
- <span data-ttu-id="15372-122">不支持版本范围。</span><span class="sxs-lookup"><span data-stu-id="15372-122">Doesn't support version ranges.</span></span>

#### <a name="allowprerelease"></a><span data-ttu-id="15372-123">allowPrerelease</span><span class="sxs-lookup"><span data-stu-id="15372-123">allowPrerelease</span></span>

- <span data-ttu-id="15372-124">类型：`boolean`</span><span class="sxs-lookup"><span data-stu-id="15372-124">Type: `boolean`</span></span>

- <span data-ttu-id="15372-125">自 .NET Core 3.0 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="15372-125">Available since: .NET Core 3.0 SDK.</span></span>

<span data-ttu-id="15372-126">指示在选择要使用的 SDK 版本时，SDK 解析程序是否应考虑预发布版本。</span><span class="sxs-lookup"><span data-stu-id="15372-126">Indicates whether the SDK resolver should consider prerelease versions when selecting the SDK version to use.</span></span>

<span data-ttu-id="15372-127">如果未显式设置此值，则默认值将取决于是否从 Visual Studio 运行：</span><span class="sxs-lookup"><span data-stu-id="15372-127">If you don't set this value explicitly, the default value depends on whether you're running from Visual Studio:</span></span>

- <span data-ttu-id="15372-128">如果未使用 Visual Studio，则默认值为 `true`。</span><span class="sxs-lookup"><span data-stu-id="15372-128">If you're **not** in Visual Studio, the default value is `true`.</span></span>
- <span data-ttu-id="15372-129">如果使用 Visual Studio，它将使用请求的预发布状态。</span><span class="sxs-lookup"><span data-stu-id="15372-129">If you are in Visual Studio, it uses the prerelease status requested.</span></span> <span data-ttu-id="15372-130">也就是说，如果使用 Visual Studio 的预览版本，或者设置了“使用 .NET Core SDK 的预览版”选项（在“工具” > “选项” > “环境” > “预览功能”下方），则默认值为 `true`，否则为 `false`    。</span><span class="sxs-lookup"><span data-stu-id="15372-130">That is, if you're using a Preview version of Visual Studio or you set the **Use previews of the .NET Core SDK** option (under **Tools** > **Options** > **Environment** > **Preview Features**), the default value is `true`; otherwise, `false`.</span></span>

#### <a name="rollforward"></a><span data-ttu-id="15372-131">rollForward</span><span class="sxs-lookup"><span data-stu-id="15372-131">rollForward</span></span>

- <span data-ttu-id="15372-132">类型：`string`</span><span class="sxs-lookup"><span data-stu-id="15372-132">Type: `string`</span></span>

- <span data-ttu-id="15372-133">自 .NET Core 3.0 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="15372-133">Available since: .NET Core 3.0 SDK.</span></span>

<span data-ttu-id="15372-134">选择 SDK 版本时要使用的前滚策略，可作为特定 SDK 版本缺失时的回退，或者作为使用更高版本的指令。</span><span class="sxs-lookup"><span data-stu-id="15372-134">The roll-forward policy to use when selecting an SDK version, either as a fallback when a specific SDK version is missing or as a directive to use a higher version.</span></span> <span data-ttu-id="15372-135">必须使用 `rollForward` 值指定[版本](#version)，除非将其设置为 `latestMajor`。</span><span class="sxs-lookup"><span data-stu-id="15372-135">A [version](#version) must be specified with a `rollForward` value, unless you're setting it to `latestMajor`.</span></span>
<span data-ttu-id="15372-136">默认的前滚行为由[匹配规则](#matching-rules)确定。</span><span class="sxs-lookup"><span data-stu-id="15372-136">The default roll forward behavior is determined by the [matching rules](#matching-rules).</span></span>

<span data-ttu-id="15372-137">要了解可用策略及其行为，请考虑以下格式为 `x.y.znn` 的 SDK 版本定义：</span><span class="sxs-lookup"><span data-stu-id="15372-137">To understand the available policies and their behavior, consider the following definitions for an SDK version in the format `x.y.znn`:</span></span>

- <span data-ttu-id="15372-138">`x` 是主版本。</span><span class="sxs-lookup"><span data-stu-id="15372-138">`x` is the major version.</span></span>
- <span data-ttu-id="15372-139">`y` 是次版本。</span><span class="sxs-lookup"><span data-stu-id="15372-139">`y` is the minor version.</span></span>
- <span data-ttu-id="15372-140">`z` 是功能区段。</span><span class="sxs-lookup"><span data-stu-id="15372-140">`z` is the feature band.</span></span>
- <span data-ttu-id="15372-141">`nn` 是修补程序版本。</span><span class="sxs-lookup"><span data-stu-id="15372-141">`nn` is the patch version.</span></span>

<span data-ttu-id="15372-142">下表列出了 `rollForward` 键的可能值：</span><span class="sxs-lookup"><span data-stu-id="15372-142">The following table shows the possible values for the `rollForward` key:</span></span>

| <span data-ttu-id="15372-143">“值”</span><span class="sxs-lookup"><span data-stu-id="15372-143">Value</span></span>         | <span data-ttu-id="15372-144">行为</span><span class="sxs-lookup"><span data-stu-id="15372-144">Behavior</span></span> |
| ------------- | ---------- |
| `patch`       | <span data-ttu-id="15372-145">使用指定的版本。</span><span class="sxs-lookup"><span data-stu-id="15372-145">Uses the specified version.</span></span> <br> <span data-ttu-id="15372-146">如果找不到，则前滚到最新的修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-146">If not found, rolls forward to the latest patch level.</span></span> <br> <span data-ttu-id="15372-147">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-147">If not found, fails.</span></span> <br><br> <span data-ttu-id="15372-148">此值是早期 SDK 版本中的旧行为。</span><span class="sxs-lookup"><span data-stu-id="15372-148">This value is the legacy behavior from the earlier versions of the SDK.</span></span> |
| `feature`     | <span data-ttu-id="15372-149">对指定的主版本、次版本和功能区段使用最新的修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-149">Uses the latest patch level for the specified major, minor, and feature band.</span></span> <br> <span data-ttu-id="15372-150">如果找不到，则前滚到同一主/次版本中的下一个较高功能区段，并使用该功能区段的最新修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-150">If not found, rolls forward to the next higher feature band within the same major/minor and uses the latest patch level for that feature band.</span></span> <br> <span data-ttu-id="15372-151">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-151">If not found, fails.</span></span> |
| `minor`       | <span data-ttu-id="15372-152">对指定的主版本、次版本和功能区段使用最新的修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-152">Uses the latest patch level for the specified major, minor, and feature band.</span></span> <br> <span data-ttu-id="15372-153">如果找不到，则前滚到同一主/次版本中的下一个较高功能区段，并使用该功能区段的最新修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-153">If not found, rolls forward to the next higher feature band within the same major/minor version and uses the latest patch level for that feature band.</span></span> <br> <span data-ttu-id="15372-154">如果找不到，则前滚到同一主版本中的下一个较高次版本和功能区段，并使用该功能区段的最新修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-154">If not found, rolls forward to the next higher minor and feature band within the same major and uses the latest patch level for that feature band.</span></span> <br> <span data-ttu-id="15372-155">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-155">If not found, fails.</span></span> |
| `major`       | <span data-ttu-id="15372-156">对指定的主版本、次版本和功能区段使用最新的修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-156">Uses the latest patch level for the specified major, minor, and feature band.</span></span> <br> <span data-ttu-id="15372-157">如果找不到，则前滚到同一主/次版本中的下一个较高功能区段，并使用该功能区段的最新修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-157">If not found, rolls forward to the next higher feature band within the same major/minor version and uses the latest patch level for that feature band.</span></span> <br> <span data-ttu-id="15372-158">如果找不到，则前滚到同一主版本中的下一个较高次版本和功能区段，并使用该功能区段的最新修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-158">If not found, rolls forward to the next higher minor and feature band within the same major and uses the latest patch level for that feature band.</span></span> <br> <span data-ttu-id="15372-159">如果找不到，则前滚到下一个较高主版本、次版本和功能区段，并使用该功能区段的最新修补程序级别。</span><span class="sxs-lookup"><span data-stu-id="15372-159">If not found, rolls forward to the next higher major, minor, and feature band and uses the latest patch level for that feature band.</span></span> <br> <span data-ttu-id="15372-160">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-160">If not found, fails.</span></span> |
| `latestPatch` | <span data-ttu-id="15372-161">使用安装的最新修补程序级别，以与请求的主版本、次版本和功能区段匹配，并且该修补程序级别大于或等于指定的值。</span><span class="sxs-lookup"><span data-stu-id="15372-161">Uses the latest installed patch level that matches the requested major, minor, and feature band with a patch level and that is greater or equal than the specified value.</span></span> <br> <span data-ttu-id="15372-162">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-162">If not found, fails.</span></span> |
| `latestFeature` | <span data-ttu-id="15372-163">使用安装的最高功能区段和修补程序级别，以与请求的主版本和次版本匹配，并且该功能区段和修补级别大于或等于指定的值。</span><span class="sxs-lookup"><span data-stu-id="15372-163">Uses the highest installed feature band and patch level that matches the requested major and minor with a feature band and patch level that is greater or equal than the specified value.</span></span> <br> <span data-ttu-id="15372-164">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-164">If not found, fails.</span></span> |
| `latestMinor` | <span data-ttu-id="15372-165">使用安装的最高次版本、功能区段和修补程序级别，以与请求的主版本匹配，并且该次版本、功能区段和修补级别大于或等于指定的值。</span><span class="sxs-lookup"><span data-stu-id="15372-165">Uses the highest installed minor, feature band, and patch level that matches the requested major with a minor, feature band, and patch level that is greater or equal than the specified value.</span></span> <br> <span data-ttu-id="15372-166">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-166">If not found, fails.</span></span> |
| `latestMajor` | <span data-ttu-id="15372-167">使用安装的最高版本的 .NET SDK，并且该版本大于或等于指定的值。</span><span class="sxs-lookup"><span data-stu-id="15372-167">Uses the highest installed .NET SDK with a version that is greater or equal than the specified value.</span></span> <br> <span data-ttu-id="15372-168">如果找不到，则失败。</span><span class="sxs-lookup"><span data-stu-id="15372-168">If not found, fail.</span></span> |
| `disable`     | <span data-ttu-id="15372-169">不前滚。</span><span class="sxs-lookup"><span data-stu-id="15372-169">Doesn't roll forward.</span></span> <span data-ttu-id="15372-170">需要完全匹配。</span><span class="sxs-lookup"><span data-stu-id="15372-170">Exact match required.</span></span> |

### <a name="msbuild-sdks"></a><span data-ttu-id="15372-171">msbuild-sdks</span><span class="sxs-lookup"><span data-stu-id="15372-171">msbuild-sdks</span></span>

<span data-ttu-id="15372-172">类型：`object`</span><span class="sxs-lookup"><span data-stu-id="15372-172">Type: `object`</span></span>

<span data-ttu-id="15372-173">可便于在一个位置（而不是在各个项目中）控制项目 SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="15372-173">Lets you control the project SDK version in one place rather than in each individual project.</span></span> <span data-ttu-id="15372-174">有关详细信息，请参阅[如何解析项目 SDK](/visualstudio/msbuild/how-to-use-project-sdk#how-project-sdks-are-resolved)。</span><span class="sxs-lookup"><span data-stu-id="15372-174">For more information, see [How project SDKs are resolved](/visualstudio/msbuild/how-to-use-project-sdk#how-project-sdks-are-resolved).</span></span>

## <a name="examples"></a><span data-ttu-id="15372-175">示例</span><span class="sxs-lookup"><span data-stu-id="15372-175">Examples</span></span>

<span data-ttu-id="15372-176">下面的示例演示如何不使用预发布版本：</span><span class="sxs-lookup"><span data-stu-id="15372-176">The following example shows how to not use prerelease versions:</span></span>

```json
{
  "sdk": {
    "allowPrerelease": false
  }
}
```

<span data-ttu-id="15372-177">下面的示例展示了如何使用安装的最高版本，此版本大于或等于指定的版本。</span><span class="sxs-lookup"><span data-stu-id="15372-177">The following example shows how to use the highest version installed that is greater or equal than the specified version.</span></span> <span data-ttu-id="15372-178">所示的 JSON 不允许任何低于 2.2.200 的 SDK 版本，而允许 2.2.200 或任何更高版本（包括 3.0.xxx 和 3.1.xxx）。</span><span class="sxs-lookup"><span data-stu-id="15372-178">The JSON shown disallows any SDK version earlier than 2.2.200 and allows 2.2.200 or any later version, including 3.0.xxx and 3.1.xxx.</span></span>

```json
{
  "sdk": {
    "version": "2.2.200",
    "rollForward": "latestMajor"
  }
}
```

<span data-ttu-id="15372-179">下面的示例演示如何使用完全指定的版本：</span><span class="sxs-lookup"><span data-stu-id="15372-179">The following example shows how to use the exact specified version:</span></span>

```json
{
  "sdk": {
    "version": "3.1.100",
    "rollForward": "disable"
  }
}
```

<span data-ttu-id="15372-180">下面的示例展示了如何使用特定主要版本和次要版本的已安装最新功能区段和修补程序版本。</span><span class="sxs-lookup"><span data-stu-id="15372-180">The following example shows how to use the latest feature band and patch version installed of a specific major and minor version.</span></span> <span data-ttu-id="15372-181">所示的 JSON 不允许任何低于 3.1.102 的 SDK 版本，而允许 3.1.102 或任何更高的 3.1.xxx 版本（如 3.1.103 或 3.1.200）。</span><span class="sxs-lookup"><span data-stu-id="15372-181">The JSON shown disallows any SDK version earlier than 3.1.102 and allows 3.1.102 or any later 3.1.xxx version, such as 3.1.103 or 3.1.200.</span></span>

```json
{
  "sdk": {
    "version": "3.1.102",
    "rollForward": "latestFeature"
  }
}
```

<span data-ttu-id="15372-182">下面的示例展示了如何使用特定版本的已安装最高修补程序版本。</span><span class="sxs-lookup"><span data-stu-id="15372-182">The following example shows how to use the highest patch version installed of a specific version.</span></span> <span data-ttu-id="15372-183">所示的 JSON 不允许任何低于 3.1.102 的 SDK 版本，而允许 3.1.102 或任何更高的 3.1.1xx 版本（如 3.1.103 或 3.1.199）。</span><span class="sxs-lookup"><span data-stu-id="15372-183">The JSON shown disallows any SDK version earlier than 3.1.102 and allows 3.1.102 or any later 3.1.1xx version, such as 3.1.103 or 3.1.199.</span></span>

```json
{
  "sdk": {
    "version": "3.1.102",
    "rollForward": "latestPatch"
  }
}
```

## <a name="globaljson-and-the-net-cli"></a><span data-ttu-id="15372-184">global.json 和 .NET CLI</span><span class="sxs-lookup"><span data-stu-id="15372-184">global.json and the .NET CLI</span></span>

<span data-ttu-id="15372-185">最好能知道计算机上安装了哪些 SDK 版本，以便在 global.json 文件中设置相应版本。</span><span class="sxs-lookup"><span data-stu-id="15372-185">It's helpful to know which SDK versions are installed on your machine to set one in the *global.json* file.</span></span> <span data-ttu-id="15372-186">有关如何执行此操作的详细信息，请参阅[如何检查是否已安装 .NET](../install/how-to-detect-installed-versions.md#check-sdk-versions)。</span><span class="sxs-lookup"><span data-stu-id="15372-186">For more information on how to do that, see [How to check that .NET is already installed](../install/how-to-detect-installed-versions.md#check-sdk-versions).</span></span>

<span data-ttu-id="15372-187">若要在计算机上安装其他 .NET SDK 版本，请访问[下载 .NET](https://dotnet.microsoft.com/download/dotnet) 页面。</span><span class="sxs-lookup"><span data-stu-id="15372-187">To install additional .NET SDK versions on your machine, visit the [Download .NET](https://dotnet.microsoft.com/download/dotnet) page.</span></span>

<span data-ttu-id="15372-188">可执行 [dotnet new](dotnet-new.md) 命令在当前目录中创建一个新的 global.json 文件，类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="15372-188">You can create a new the *global.json* file in the current directory by executing the [dotnet new](dotnet-new.md) command, similar to the following example:</span></span>

```dotnetcli
dotnet new globaljson --sdk-version 3.0.100
```

## <a name="matching-rules"></a><span data-ttu-id="15372-189">匹配规则</span><span class="sxs-lookup"><span data-stu-id="15372-189">Matching rules</span></span>

> [!NOTE]
> <span data-ttu-id="15372-190">匹配规则由 `dotnet.exe` 入口点控制，该入口点在所有已安装的 .NET 运行时中很常见。</span><span class="sxs-lookup"><span data-stu-id="15372-190">The matching rules are governed by the `dotnet.exe` entry point, which is common across all installed .NET installed runtimes.</span></span> <span data-ttu-id="15372-191">如果并行安装了多个运行时，或者正在使用 global.json 文件，则使用已安装的最新版 .NET 运行时的匹配规则。</span><span class="sxs-lookup"><span data-stu-id="15372-191">The matching rules for the latest installed version of the .NET Runtime are used when you have multiple runtimes installed side-by-side or if or you're using a *global.json* file.</span></span>

## <a name="net-core-3x"></a>[<span data-ttu-id="15372-192">.NET Core 3.x</span><span class="sxs-lookup"><span data-stu-id="15372-192">.NET Core 3.x</span></span>](#tab/netcore3x)

<span data-ttu-id="15372-193">从 .NET Core 3.0 开始，在确定要使用的 SDK 版本时，适用以下规则：</span><span class="sxs-lookup"><span data-stu-id="15372-193">Starting with .NET Core 3.0, the following rules apply when determining which version of the SDK to use:</span></span>

- <span data-ttu-id="15372-194">如果未找到 global.json 文件，或者 global.json 未指定 SDK 版本和 `allowPrerelease` 值，则使用安装的最高 SDK 版本（相当于将 `rollForward` 设置为 `latestMajor`） 。</span><span class="sxs-lookup"><span data-stu-id="15372-194">If no *global.json* file is found, or *global.json* doesn't specify an SDK version nor an `allowPrerelease` value, the highest installed SDK version is used (equivalent to setting `rollForward` to `latestMajor`).</span></span> <span data-ttu-id="15372-195">是否考虑 SDK 预发布版本取决于 `dotnet` 的调用方式。</span><span class="sxs-lookup"><span data-stu-id="15372-195">Whether prerelease SDK versions are considered depends on how `dotnet` is being invoked.</span></span>
  - <span data-ttu-id="15372-196">如果未使用 Visual Studio，则考虑预发布版本。</span><span class="sxs-lookup"><span data-stu-id="15372-196">If you're **not** in Visual Studio, prerelease versions are considered.</span></span>
  - <span data-ttu-id="15372-197">如果使用 Visual Studio，它将使用请求的预发布状态。</span><span class="sxs-lookup"><span data-stu-id="15372-197">If you are in Visual Studio, it uses the prerelease status requested.</span></span> <span data-ttu-id="15372-198">也就是说，如果使用 Visual Studio 的预览版本，或者设置了“使用 .NET Core SDK 的预览版”选项（在“工具” > “选项” > “环境” > “预览功能”下方），则考虑预发布版本；否则仅考虑发布版本    。</span><span class="sxs-lookup"><span data-stu-id="15372-198">That is, if you're using a Preview version of Visual Studio or you set the **Use previews of the .NET Core SDK** option (under **Tools** > **Options** > **Environment** > **Preview Features**), prerelease versions are considered; otherwise, only release versions are considered.</span></span>
- <span data-ttu-id="15372-199">如果找到了未指定 SDK 版本但指定了 `allowPrerelease` 值的 global.json 文件，则使用安装的最高 SDK 版本（相当于将 `rollForward` 设置为 `latestMajor`）。</span><span class="sxs-lookup"><span data-stu-id="15372-199">If a *global.json* file is found that doesn't specify an SDK version but it specifies an `allowPrerelease` value, the highest installed SDK version is used (equivalent to setting `rollForward` to `latestMajor`).</span></span> <span data-ttu-id="15372-200">最新 SDK 版本是发布版本还是预发布版本取决于 `allowPrerelease` 的值。</span><span class="sxs-lookup"><span data-stu-id="15372-200">Whether the latest SDK version can be release or prerelease depends on the value of `allowPrerelease`.</span></span> <span data-ttu-id="15372-201">`true` 指示考虑预发布版本；`false` 指示仅考虑发布版本。</span><span class="sxs-lookup"><span data-stu-id="15372-201">`true` indicates prerelease versions are considered; `false` indicates that only release versions are considered.</span></span>
- <span data-ttu-id="15372-202">如果找到 global.json 文件，并且该文件指定了 SDK 版本：</span><span class="sxs-lookup"><span data-stu-id="15372-202">If a *global.json* file is found and it specifies an SDK version:</span></span>

  - <span data-ttu-id="15372-203">如果未设置 `rollForward` 值，它将使用 `latestPatch` 作为默认 `rollForward` 策略。</span><span class="sxs-lookup"><span data-stu-id="15372-203">If no `rollForward` value is set, it uses `latestPatch` as the default `rollForward` policy.</span></span> <span data-ttu-id="15372-204">否则，请在 [rollForward](#rollforward) 部分中检查每个值及其行为。</span><span class="sxs-lookup"><span data-stu-id="15372-204">Otherwise, check each value and their behavior in the [rollForward](#rollforward) section.</span></span>
  - <span data-ttu-id="15372-205">有关是否考虑预发布版本以及未设置 `allowPrerelease` 时的默认行为的信息，请参阅 [allowPrerelease](#allowprerelease) 部分。</span><span class="sxs-lookup"><span data-stu-id="15372-205">Whether prerelease versions are considered and what's the default behavior when `allowPrerelease` isn't set is described in the [allowPrerelease](#allowprerelease) section.</span></span>

## <a name="net-core-2x"></a>[<span data-ttu-id="15372-206">.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15372-206">.NET Core 2.x</span></span>](#tab/netcore2x)

<span data-ttu-id="15372-207">在 .NET Core 2.x SDK 中，在确定要使用的 SDK 版本时，适用以下规则：</span><span class="sxs-lookup"><span data-stu-id="15372-207">In .NET Core 2.x SDK, the following rules apply when determining which version of the SDK to use:</span></span>

- <span data-ttu-id="15372-208">如果未找到 global.json 文件或 global.json 未指定 SDK 版本，则使用最新安装的 SDK 版本 。</span><span class="sxs-lookup"><span data-stu-id="15372-208">If no *global.json* file is found or *global.json* doesn't specify an SDK version, the latest installed SDK version is used.</span></span> <span data-ttu-id="15372-209">最新 SDK 版本可能是发布版本或预发布版本 - 以版本号更高的为准。</span><span class="sxs-lookup"><span data-stu-id="15372-209">Latest SDK version can be either release or prerelease - the highest version number wins.</span></span>
- <span data-ttu-id="15372-210">如果 global.json 指定了 SDK 版本：</span><span class="sxs-lookup"><span data-stu-id="15372-210">If *global.json* does specify an SDK version:</span></span>
  - <span data-ttu-id="15372-211">如果计算机上安装了该指定的 SDK 版本，则使用该版本。</span><span class="sxs-lookup"><span data-stu-id="15372-211">If the specified SDK version is found on the machine, that exact version is used.</span></span>
  - <span data-ttu-id="15372-212">如果计算机上未安装指定的 SDK 版本，则使用最新安装的该版本的 SDK 修补程序版本。</span><span class="sxs-lookup"><span data-stu-id="15372-212">If the specified SDK version can't be found on the machine, the latest installed SDK **patch version** of that version is used.</span></span> <span data-ttu-id="15372-213">安装的最新 SDK 修补程序版本可能是发布版本或预发布版本 - 以版本号更高的为准。</span><span class="sxs-lookup"><span data-stu-id="15372-213">Latest installed SDK **patch version** can be either release or prerelease - the highest version number wins.</span></span> <span data-ttu-id="15372-214">在 .NET Core 2.1 及更高版本中，在选择 SDK 版本时将忽略低于指定修补程序版本的修补程序版本 。</span><span class="sxs-lookup"><span data-stu-id="15372-214">In .NET Core 2.1 and higher, the **patch versions** lower than the **patch version** specified are ignored in the SDK selection.</span></span>
  - <span data-ttu-id="15372-215">如果找不到指定的 SDK 版本和相应的 SDK 修补程序版本，会引发错误。</span><span class="sxs-lookup"><span data-stu-id="15372-215">If the specified SDK version and an appropriate SDK **patch version** can't be found, an error is thrown.</span></span>

<span data-ttu-id="15372-216">SDK 版本由以下部分组成：</span><span class="sxs-lookup"><span data-stu-id="15372-216">The SDK version is composed of the following parts:</span></span>

`[.NET Core major version].[.NET Core minor version].[xyz][-optional preview name]`

<span data-ttu-id="15372-217">.NET Core SDK 的功能版本由 SDK 版本 2.1.100 及更高版本所采用的版本号的最后一部分 (`xyz`) 的第一个数字 (`x`) 表示。</span><span class="sxs-lookup"><span data-stu-id="15372-217">The **feature release** of the .NET Core SDK is represented by the first digit (`x`) in the last portion of the number (`xyz`) for SDK versions 2.1.100 and higher.</span></span> <span data-ttu-id="15372-218">通常，.NET Core SDK 的发布周期比 .NET Core 快。</span><span class="sxs-lookup"><span data-stu-id="15372-218">In general, the .NET Core SDK has a faster release cycle than .NET Core.</span></span>

<span data-ttu-id="15372-219">修补程序版本由 SDK 版本 2.1.100 及更高版本所采用的版本号的最后一部分 (`xyz`) 的后两位数字 (`yz`) 表示。</span><span class="sxs-lookup"><span data-stu-id="15372-219">The **patch version** is defined by the last two digits (`yz`) in the last portion of the number (`xyz`) for SDK versions 2.1.100 and higher.</span></span> <span data-ttu-id="15372-220">例如，如果将 `2.1.300` 指定为 SDK 版本，则可选的 SDK 版本号最多到 `2.1.399`，但 `2.1.400` 不视为 `2.1.300` 的一个修补程序版本。</span><span class="sxs-lookup"><span data-stu-id="15372-220">For example, if you specify `2.1.300` as the SDK version, SDK selection finds up to `2.1.399` but `2.1.400` isn't considered a patch version for `2.1.300`.</span></span>

<span data-ttu-id="15372-221">版本号架构过渡期间发布了 .NET Core SDK 版本 `2.1.100` 到 `2.1.201`，但未正确使用 `xyz` 表示法。</span><span class="sxs-lookup"><span data-stu-id="15372-221">.NET Core SDK versions `2.1.100` through `2.1.201` were released during the transition between version number schemes and don't correctly handle the `xyz` notation.</span></span> <span data-ttu-id="15372-222">强烈建议如果在 global.json 文件中指定了这些版本，请确保目标计算机上安装了指定版本。</span><span class="sxs-lookup"><span data-stu-id="15372-222">We highly recommend if you specify these versions in the *global.json* file, that you ensure the specified versions are on the target machines.</span></span>

---

## <a name="troubleshoot-build-warnings"></a><span data-ttu-id="15372-223">针对生成警告的疑难解答</span><span class="sxs-lookup"><span data-stu-id="15372-223">Troubleshoot build warnings</span></span>

* <span data-ttu-id="15372-224">以下警告指示你的项目使用 .NET Core SDK 的预发布版本进行编译：</span><span class="sxs-lookup"><span data-stu-id="15372-224">The following warning indicates that your project was compiled using a prerelease version of the .NET Core SDK:</span></span>

  > <span data-ttu-id="15372-225">使用的是 .NET Core SDK 的预览版本，</span><span class="sxs-lookup"><span data-stu-id="15372-225">You are working with a preview version of the .NET Core SDK.</span></span> <span data-ttu-id="15372-226">可通过当前项目中的 global.json 文件定义 SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="15372-226">You can define the SDK version via a global.json file in the current project.</span></span> <span data-ttu-id="15372-227">有关详细信息，请访问 <https://go.microsoft.com/fwlink/?linkid=869452>。</span><span class="sxs-lookup"><span data-stu-id="15372-227">More at <https://go.microsoft.com/fwlink/?linkid=869452>.</span></span>

  <span data-ttu-id="15372-228">.NET Core SDK 的各种版本均品质优良稳定，在业内口碑良好。</span><span class="sxs-lookup"><span data-stu-id="15372-228">.NET Core SDK versions have a history and commitment of being high quality.</span></span> <span data-ttu-id="15372-229">但是，如果不希望使用预发布版本，请在 [allowPrerelease](#allowprerelease) 部分中查看可用于 .NET Core 3.0 SDK 或更高版本的各种策略。</span><span class="sxs-lookup"><span data-stu-id="15372-229">However, if you don't want to use a prerelease version, check the different strategies you can use with the .NET Core 3.0 SDK or a later version in the [allowPrerelease](#allowprerelease) section.</span></span> <span data-ttu-id="15372-230">对于从未安装 .NET Core 3.0 或更高版本运行时或 SDK 的计算机，需要创建一个 global.json 文件，并指定要使用的确切版本。</span><span class="sxs-lookup"><span data-stu-id="15372-230">For machines that have never had a .NET Core 3.0 or higher Runtime or SDK installed, you need to create a *global.json* file and specify the exact version you want to use.</span></span>

* <span data-ttu-id="15372-231">以下警告指示项目面向 EF Core 1.0 或 1.1，后者与 .NET Core 2.1 SDK 及更高版本不兼容：</span><span class="sxs-lookup"><span data-stu-id="15372-231">The following warning indicates that your project targets EF Core 1.0 or 1.1, which isn't compatible with .NET Core 2.1 SDK and later versions:</span></span>

  > <span data-ttu-id="15372-232">启动项目 '{startupProject}' 面向框架 '.NETCoreApp' 的版本 '{targetFrameworkVersion}'。</span><span class="sxs-lookup"><span data-stu-id="15372-232">Startup project '{startupProject}' targets framework '.NETCoreApp' version '{targetFrameworkVersion}'.</span></span> <span data-ttu-id="15372-233">此版本的 Entity Framework Core .NET 命令行工具仅支持 2.0 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="15372-233">This version of the Entity Framework Core .NET Command-line Tools only supports version 2.0 or higher.</span></span> <span data-ttu-id="15372-234">有关使用旧版工具的信息，请参阅 <https://go.microsoft.com/fwlink/?linkid=871254>。</span><span class="sxs-lookup"><span data-stu-id="15372-234">For information on using older versions of the tools, see <https://go.microsoft.com/fwlink/?linkid=871254>.</span></span>

  <span data-ttu-id="15372-235">从 .NET Core 2.1 SDK（版本 2.1.300）开始，SDK 中包含 `dotnet ef` 命令。</span><span class="sxs-lookup"><span data-stu-id="15372-235">Starting with .NET Core 2.1 SDK (version 2.1.300), the `dotnet ef` command comes included in the SDK.</span></span> <span data-ttu-id="15372-236">若要编译项目，请在计算机上安装 .NET Core 2.0 SDK（版本 2.1.201）或更早版本，并使用 global.json 文件定义所需 SDK 版本。</span><span class="sxs-lookup"><span data-stu-id="15372-236">To compile your project, install .NET Core 2.0 SDK (version 2.1.201) or earlier on your machine and define the desired SDK version using the *global.json* file.</span></span> <span data-ttu-id="15372-237">有关 `dotnet ef` 命令的详细信息，请参阅 [EF Core .NET 命令行工具](/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="15372-237">For more information about the `dotnet ef` command, see [EF Core .NET Command-line Tools](/ef/core/miscellaneous/cli/dotnet).</span></span>

## <a name="see-also"></a><span data-ttu-id="15372-238">请参阅</span><span class="sxs-lookup"><span data-stu-id="15372-238">See also</span></span>

- [<span data-ttu-id="15372-239">如何解析 SDK 项目</span><span class="sxs-lookup"><span data-stu-id="15372-239">How project SDKs are resolved</span></span>](/visualstudio/msbuild/how-to-use-project-sdk#how-project-sdks-are-resolved)
