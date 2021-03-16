---
title: 使用 .NET CLI 发布应用
description: 了解如何使用 .NET CLI 命令来发布 .NET 应用程序。
author: adegeo
ms.author: adegeo
ms.date: 02/05/2021
dev_langs:
- csharp
- vb
ms.openlocfilehash: d2695b1f271e57bb44ed3f94acbbe17b830fdd60
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255681"
---
# <a name="publish-net-apps-with-the-net-cli"></a><span data-ttu-id="09d42-103">使用 .NET CLI 发布 .NET 应用</span><span class="sxs-lookup"><span data-stu-id="09d42-103">Publish .NET apps with the .NET CLI</span></span>

<span data-ttu-id="09d42-104">本文展示了如何使用命令行来发布 .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="09d42-104">This article demonstrates how you can publish your .NET application from the command line.</span></span> <span data-ttu-id="09d42-105">.NET 提供了三种发布应用程序的方法。</span><span class="sxs-lookup"><span data-stu-id="09d42-105">.NET provides three ways to publish your applications.</span></span> <span data-ttu-id="09d42-106">依赖于框架的部署会生成一个跨平台 .dll 文件，此文件使用本地安装的 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="09d42-106">Framework-dependent deployment produces a cross-platform .dll file that uses the locally installed .NET runtime.</span></span> <span data-ttu-id="09d42-107">依赖于框架的可执行文件会生成一个特定于平台的可执行文件，此文件使用本地安装的 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="09d42-107">Framework-dependent executable produces a platform-specific executable that uses the locally installed .NET runtime.</span></span> <span data-ttu-id="09d42-108">独立式可执行文件会生成一个特定于平台的可执行文件，并包含 .NET 运行时的本地副本。</span><span class="sxs-lookup"><span data-stu-id="09d42-108">Self-contained executable produces a platform-specific executable and includes a local copy of the .NET runtime.</span></span>

<span data-ttu-id="09d42-109">有关这些发布模式的概述，请参阅 [.NET 应用程序部署](index.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-109">For an overview of these publishing modes, see [.NET Application Deployment](index.md).</span></span>

<span data-ttu-id="09d42-110">正在查找有关 CLI 的快速帮助？</span><span class="sxs-lookup"><span data-stu-id="09d42-110">Looking for some quick help on using the CLI?</span></span> <span data-ttu-id="09d42-111">下表列出了一些关于如何发布应用的示例。</span><span class="sxs-lookup"><span data-stu-id="09d42-111">The following table shows some examples of how to publish your app.</span></span> <span data-ttu-id="09d42-112">可以使用 `-f <TFM>` 参数或通过编辑项目文件来指定目标框架。</span><span class="sxs-lookup"><span data-stu-id="09d42-112">You can specify the target framework with the `-f <TFM>` parameter or by editing the project file.</span></span> <span data-ttu-id="09d42-113">有关详细信息，请参阅[发布基本知识](#publishing-basics)。</span><span class="sxs-lookup"><span data-stu-id="09d42-113">For more information, see [Publishing basics](#publishing-basics).</span></span>

| <span data-ttu-id="09d42-114">发布模式</span><span class="sxs-lookup"><span data-stu-id="09d42-114">Publish Mode</span></span>                   | <span data-ttu-id="09d42-115">SDK 版本</span><span class="sxs-lookup"><span data-stu-id="09d42-115">SDK Version</span></span> | <span data-ttu-id="09d42-116">命令</span><span class="sxs-lookup"><span data-stu-id="09d42-116">Command</span></span>                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| [<span data-ttu-id="09d42-117">依赖框架的部署</span><span class="sxs-lookup"><span data-stu-id="09d42-117">Framework-dependent deployment</span></span>](#framework-dependent-deployment) | <span data-ttu-id="09d42-118">2.1</span><span class="sxs-lookup"><span data-stu-id="09d42-118">2.1</span></span>         | `dotnet publish -c Release`                                 |
|                                | <span data-ttu-id="09d42-119">3.1</span><span class="sxs-lookup"><span data-stu-id="09d42-119">3.1</span></span>         | `dotnet publish -c Release -p:UseAppHost=false`             |
|                                | <span data-ttu-id="09d42-120">5.0</span><span class="sxs-lookup"><span data-stu-id="09d42-120">5.0</span></span>         | `dotnet publish -c Release -p:UseAppHost=false`             |
| [<span data-ttu-id="09d42-121">依赖于框架的可执行文件</span><span class="sxs-lookup"><span data-stu-id="09d42-121">Framework-dependent executable</span></span>](#framework-dependent-executable) | <span data-ttu-id="09d42-122">3.1</span><span class="sxs-lookup"><span data-stu-id="09d42-122">3.1</span></span>         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |
|                                | <span data-ttu-id="09d42-123">5.0</span><span class="sxs-lookup"><span data-stu-id="09d42-123">5.0</span></span>         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |
| [<span data-ttu-id="09d42-124">独立部署</span><span class="sxs-lookup"><span data-stu-id="09d42-124">Self-contained deployment</span></span>](#self-contained-deployment)      | <span data-ttu-id="09d42-125">2.1</span><span class="sxs-lookup"><span data-stu-id="09d42-125">2.1</span></span>         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | <span data-ttu-id="09d42-126">3.1</span><span class="sxs-lookup"><span data-stu-id="09d42-126">3.1</span></span>         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | <span data-ttu-id="09d42-127">5.0</span><span class="sxs-lookup"><span data-stu-id="09d42-127">5.0</span></span>         | `dotnet publish -c Release -r <RID> --self-contained true`  |

<span data-ttu-id="09d42-128">\* 当使用 SDK 版本 3.1 或更高版本时，依赖于框架的可执行文件是运行基本 `dotnet publish` 命令时的默认发布模式。</span><span class="sxs-lookup"><span data-stu-id="09d42-128">\* When using **SDK version 3.1** or higher, framework-dependent executable is the default publishing mode when running the basic `dotnet publish` command.</span></span>

> [!NOTE]
> <span data-ttu-id="09d42-129">`-c Release` 参数不是必需的。</span><span class="sxs-lookup"><span data-stu-id="09d42-129">The `-c Release` parameter isn't required.</span></span> <span data-ttu-id="09d42-130">它作为提醒提供来发布应用的发行内部版本。</span><span class="sxs-lookup"><span data-stu-id="09d42-130">It's provided as a reminder to publish the **Release** build of your app.</span></span>

## <a name="publishing-basics"></a><span data-ttu-id="09d42-131">发布基本知识</span><span class="sxs-lookup"><span data-stu-id="09d42-131">Publishing basics</span></span>

<span data-ttu-id="09d42-132">发布应用时，项目文件的 `<TargetFramework>` 设置指定默认目标框架。</span><span class="sxs-lookup"><span data-stu-id="09d42-132">The `<TargetFramework>` setting of the project file specifies the default target framework when you publish your app.</span></span> <span data-ttu-id="09d42-133">可以将目标框架更改为任意有效[目标框架名字对象 (TFM)](../../standard/frameworks.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-133">You can change the target framework to any valid [Target Framework Moniker (TFM)](../../standard/frameworks.md).</span></span> <span data-ttu-id="09d42-134">例如，如果项目使用 `<TargetFramework>netcoreapp2.1</TargetFramework>`，则会创建以 .NET Core 2.1 为目标的二进制文件。</span><span class="sxs-lookup"><span data-stu-id="09d42-134">For example, if your project uses `<TargetFramework>netcoreapp2.1</TargetFramework>`, a binary that targets .NET Core 2.1 is created.</span></span> <span data-ttu-id="09d42-135">此设置中指定的 TFM 是[`dotnet publish`](../tools/dotnet-publish.md) 命令使用的默认目标。</span><span class="sxs-lookup"><span data-stu-id="09d42-135">The TFM specified in this setting is the default target used by the [`dotnet publish`](../tools/dotnet-publish.md) command.</span></span>

<span data-ttu-id="09d42-136">若要以多个框架为目标，则可以将 `<TargetFrameworks>` 设置设为多个 TFM 值（以分号分隔）。</span><span class="sxs-lookup"><span data-stu-id="09d42-136">If you want to target more than one framework, you can set the `<TargetFrameworks>` setting to multiple TFM values, separated by a semicolon.</span></span> <span data-ttu-id="09d42-137">当你生成应用时，会为每个目标框架生成一个内部版本。</span><span class="sxs-lookup"><span data-stu-id="09d42-137">When you build your app, a build is produced for each target framework.</span></span> <span data-ttu-id="09d42-138">但是，当你发布应用时，必须使用 `dotnet publish -f <TFM>` 命令指定目标框架。</span><span class="sxs-lookup"><span data-stu-id="09d42-138">However, when you publish your app, you must specify the target framework with the `dotnet publish -f <TFM>` command.</span></span>

<span data-ttu-id="09d42-139">除非另有设置，否则 [`dotnet publish`](../tools/dotnet-publish.md) 命令的输出目录为 `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/`。</span><span class="sxs-lookup"><span data-stu-id="09d42-139">Unless otherwise set, the output directory of the [`dotnet publish`](../tools/dotnet-publish.md) command is `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/`.</span></span> <span data-ttu-id="09d42-140">除非使用 `-c` 参数进行更改，否则默认的 BUILD-CONFIGURATION 模式为 Debug。</span><span class="sxs-lookup"><span data-stu-id="09d42-140">The default **BUILD-CONFIGURATION** mode is **Debug** unless changed with the `-c` parameter.</span></span> <span data-ttu-id="09d42-141">例如，`dotnet publish -c Release -f netcoreapp2.1` 发布到 `./bin/Release/netcoreapp2.1/publish/`。</span><span class="sxs-lookup"><span data-stu-id="09d42-141">For example, `dotnet publish -c Release -f netcoreapp2.1` publishes to `./bin/Release/netcoreapp2.1/publish/`.</span></span>

<span data-ttu-id="09d42-142">如果你使用 .NET Core SDK 3.1 或更高版本，则默认发布模式为依赖于框架的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="09d42-142">If you use .NET Core SDK 3.1 or later, the default publish mode is framework-dependent _executable_.</span></span>

<span data-ttu-id="09d42-143">如果你使用 .NET Core SDK 2.1，则默认发布模式为依赖于框架的部署。</span><span class="sxs-lookup"><span data-stu-id="09d42-143">If you use .NET Core SDK 2.1, the default publish mode is framework-dependent _deployment_.</span></span>

### <a name="native-dependencies"></a><span data-ttu-id="09d42-144">本机依赖项</span><span class="sxs-lookup"><span data-stu-id="09d42-144">Native dependencies</span></span>

<span data-ttu-id="09d42-145">如果应用具有本机依赖项，则只能在相同操作系统上运行。</span><span class="sxs-lookup"><span data-stu-id="09d42-145">If your app has native dependencies, it may not run on a different operating system.</span></span> <span data-ttu-id="09d42-146">例如，如果应用使用本机 Windows API，则不能在 macOS 或 Linux 上运行。</span><span class="sxs-lookup"><span data-stu-id="09d42-146">For example, if your app uses the native Windows API, it won't run on macOS or Linux.</span></span> <span data-ttu-id="09d42-147">需要提供特定于平台的代码并为每个平台编译可执行文件。</span><span class="sxs-lookup"><span data-stu-id="09d42-147">You would need to provide platform-specific code and compile an executable for each platform.</span></span>

<span data-ttu-id="09d42-148">另外，如果引用的库具有本机依赖项，则应用可能无法在每个平台上运行。</span><span class="sxs-lookup"><span data-stu-id="09d42-148">Consider also, if a library you referenced has a native dependency, your app may not run on every platform.</span></span> <span data-ttu-id="09d42-149">但是，引用的 NuGet 包可能包含特定于平台的版本，以便处理所需的本机依赖项。</span><span class="sxs-lookup"><span data-stu-id="09d42-149">However, it's possible a NuGet package you're referencing has included platform-specific versions to handle the required native dependencies for you.</span></span>

<span data-ttu-id="09d42-150">使用本机依赖项分发应用时，可能需要使用 `dotnet publish -r <RID>` 开关来指定想要发布的目标平台。</span><span class="sxs-lookup"><span data-stu-id="09d42-150">When distributing an app with native dependencies, you may need to use the `dotnet publish -r <RID>` switch to specify the target platform you want to publish for.</span></span> <span data-ttu-id="09d42-151">有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-151">For a list of runtime identifiers, see [Runtime Identifier (RID) catalog](../rid-catalog.md).</span></span>

<span data-ttu-id="09d42-152">有关特定于平台的二进制文件的详细信息，请参阅[依赖于框架的可执行文件](#framework-dependent-executable)和[独立部署](#self-contained-deployment)部分。</span><span class="sxs-lookup"><span data-stu-id="09d42-152">More information about platform-specific binaries is covered in the [Framework-dependent executable](#framework-dependent-executable) and [Self-contained deployment](#self-contained-deployment) sections.</span></span>

## <a name="sample-app"></a><span data-ttu-id="09d42-153">示例应用</span><span class="sxs-lookup"><span data-stu-id="09d42-153">Sample app</span></span>

<span data-ttu-id="09d42-154">可使用以下应用来探索发布命令。</span><span class="sxs-lookup"><span data-stu-id="09d42-154">You can use the following app to explore the publishing commands.</span></span> <span data-ttu-id="09d42-155">通过在终端中运行以下命令来创建应用：</span><span class="sxs-lookup"><span data-stu-id="09d42-155">The app is created by running the following commands in your terminal:</span></span>

```dotnetcli
mkdir apptest1
cd apptest1
dotnet new console
dotnet add package Figgle
```

<span data-ttu-id="09d42-156">控制台模板生成的 `Program.cs` 或 `Program.vb` 文件需要进行以下更改：</span><span class="sxs-lookup"><span data-stu-id="09d42-156">The `Program.cs` or `Program.vb` file that is generated by the console template needs to be changed to the following:</span></span>

```csharp
using System;

namespace apptest1
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"));
        }
    }
}
```

```vb
Module Program
    Sub Main(args As String())
        Console.WriteLine(Figgle.FiggleFonts.Standard.Render("Hello, World!"))
    End Sub
End Module
```

<span data-ttu-id="09d42-157">运行应用 ([`dotnet run`](../tools/dotnet-run.md)) 时，将显示以下输出：</span><span class="sxs-lookup"><span data-stu-id="09d42-157">When you run the app ([`dotnet run`](../tools/dotnet-run.md)), the following output is displayed:</span></span>

```terminal
  _   _      _ _         __        __         _     _ _
 | | | | ___| | | ___    \ \      / /__  _ __| | __| | |
 | |_| |/ _ \ | |/ _ \    \ \ /\ / / _ \| '__| |/ _` | |
 |  _  |  __/ | | (_) |    \ V  V / (_) | |  | | (_| |_|
 |_| |_|\___|_|_|\___( )    \_/\_/ \___/|_|  |_|\__,_(_)
                     |/
```

## <a name="framework-dependent-deployment"></a><span data-ttu-id="09d42-158">依赖框架的部署</span><span class="sxs-lookup"><span data-stu-id="09d42-158">Framework-dependent deployment</span></span>

<span data-ttu-id="09d42-159">对于 .NET Core 2.1 SDK CLI，依赖于框架的部署 (FDD) 是基本 `dotnet publish` 命令的默认模式。</span><span class="sxs-lookup"><span data-stu-id="09d42-159">For the .NET Core 2.1 SDK CLI, framework-dependent deployment (FDD) is the default mode for the basic `dotnet publish` command.</span></span> <span data-ttu-id="09d42-160">在更高版本的 SDK 中，[依赖于框架的可执行文件](#framework-dependent-executable)是默认模式。</span><span class="sxs-lookup"><span data-stu-id="09d42-160">In newer SDKs, [Framework-dependent executable](#framework-dependent-executable) is the default.</span></span>

<span data-ttu-id="09d42-161">将应用作为 FDD 发布时，会在 `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/` 文件夹中创建 `<PROJECT-NAME>.dll` 文件。</span><span class="sxs-lookup"><span data-stu-id="09d42-161">When you publish your app as an FDD, a `<PROJECT-NAME>.dll` file is created in the `./bin/<BUILD-CONFIGURATION>/<TFM>/publish/` folder.</span></span> <span data-ttu-id="09d42-162">若要运行应用，请导航到输出文件夹并使用 `dotnet <PROJECT-NAME>.dll` 命令。</span><span class="sxs-lookup"><span data-stu-id="09d42-162">To run your app, navigate to the output folder and use the `dotnet <PROJECT-NAME>.dll` command.</span></span>

<span data-ttu-id="09d42-163">你的应用被配置为以特定版本的 .NET 为目标。</span><span class="sxs-lookup"><span data-stu-id="09d42-163">Your app is configured to target a specific version of .NET.</span></span> <span data-ttu-id="09d42-164">目标 .NET 运行时必须在运行应用的任何虚拟机上。</span><span class="sxs-lookup"><span data-stu-id="09d42-164">That targeted .NET runtime is required to be on any machine where your app runs.</span></span> <span data-ttu-id="09d42-165">例如，如果你的应用以 .NET Core 3.1 为目标，则任何运行你的应用的虚拟机都必须安装 .NET Core 3.1 运行时。</span><span class="sxs-lookup"><span data-stu-id="09d42-165">For example, if your app targets .NET Core 3.1, any machine that your app runs on must have the .NET Core 3.1 runtime installed.</span></span> <span data-ttu-id="09d42-166">如[发布基础知识](#publishing-basics)部分中所述，可以编辑项目文件为更改默认目标框架或面向多个框架。</span><span class="sxs-lookup"><span data-stu-id="09d42-166">As stated in the [Publishing basics](#publishing-basics) section, you can edit your project file to change the default target framework or to target more than one framework.</span></span>

<span data-ttu-id="09d42-167">发布 FDD 会创建一个应用，该应用会自动前滚到运行该应用的系统上可用的最新 .NET 安全补丁。</span><span class="sxs-lookup"><span data-stu-id="09d42-167">Publishing an FDD creates an app that automatically rolls-forward to the latest .NET security patch available on the system that runs the app.</span></span> <span data-ttu-id="09d42-168">若要详细了解编译时的版本绑定，请参阅[选择要使用的 .NET 版本](../versions/selection.md#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="09d42-168">For more information on version binding at compile time, see [Select the .NET version to use](../versions/selection.md#framework-dependent-apps-roll-forward).</span></span>

| <span data-ttu-id="09d42-169">发布模式</span><span class="sxs-lookup"><span data-stu-id="09d42-169">Publish Mode</span></span>                   | <span data-ttu-id="09d42-170">SDK 版本</span><span class="sxs-lookup"><span data-stu-id="09d42-170">SDK Version</span></span> | <span data-ttu-id="09d42-171">命令</span><span class="sxs-lookup"><span data-stu-id="09d42-171">Command</span></span>                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| <span data-ttu-id="09d42-172">依赖框架的部署</span><span class="sxs-lookup"><span data-stu-id="09d42-172">Framework-dependent deployment</span></span> | <span data-ttu-id="09d42-173">2.1</span><span class="sxs-lookup"><span data-stu-id="09d42-173">2.1</span></span>         | `dotnet publish -c Release`                                 |
|                                | <span data-ttu-id="09d42-174">3.1</span><span class="sxs-lookup"><span data-stu-id="09d42-174">3.1</span></span>         | `dotnet publish -c Release -p:UseAppHost=false`             |
|                                | <span data-ttu-id="09d42-175">5.0</span><span class="sxs-lookup"><span data-stu-id="09d42-175">5.0</span></span>         | `dotnet publish -c Release -p:UseAppHost=false`             |

## <a name="framework-dependent-executable"></a><span data-ttu-id="09d42-176">依赖于框架的可执行文件</span><span class="sxs-lookup"><span data-stu-id="09d42-176">Framework-dependent executable</span></span>

<span data-ttu-id="09d42-177">对于.NET 5（以及 .NET Core 3.1）SDK CLI，依赖于框架的可执行文件 (FDE) 是基本 `dotnet publish` 命令的默认模式。</span><span class="sxs-lookup"><span data-stu-id="09d42-177">For the .NET 5 (and .NET Core 3.1) SDK CLI, framework-dependent executable (FDE) is the default mode for the basic `dotnet publish` command.</span></span> <span data-ttu-id="09d42-178">只要想要面向当前操作系统，就不需要指定任何其他参数。</span><span class="sxs-lookup"><span data-stu-id="09d42-178">You don't need to specify any other parameters, as long as you want to target the current operating system.</span></span>

<span data-ttu-id="09d42-179">在此模式下，将创建特定于平台的可执行主机来托管跨平台应用。</span><span class="sxs-lookup"><span data-stu-id="09d42-179">In this mode, a platform-specific executable host is created to host your cross-platform app.</span></span> <span data-ttu-id="09d42-180">此模式类似于 FDD，因为 FDD 需要 `dotnet` 命令形式的主机。</span><span class="sxs-lookup"><span data-stu-id="09d42-180">This mode is similar to FDD, as FDD requires a host in the form of the `dotnet` command.</span></span> <span data-ttu-id="09d42-181">每个平台的主机可执行文件名各不相同，其文件名类似于 `<PROJECT-FILE>.exe`。</span><span class="sxs-lookup"><span data-stu-id="09d42-181">The host executable filename varies per platform and is named something similar to `<PROJECT-FILE>.exe`.</span></span> <span data-ttu-id="09d42-182">可以直接运行此可执行文件，而不是调用 `dotnet <PROJECT-FILE>.dll`，这仍然是运行应用的可接受方式。</span><span class="sxs-lookup"><span data-stu-id="09d42-182">You can run this executable directly instead of calling `dotnet <PROJECT-FILE>.dll`, which is still an acceptable way to run the app.</span></span>

<span data-ttu-id="09d42-183">你的应用被配置为以特定版本的 .NET 为目标。</span><span class="sxs-lookup"><span data-stu-id="09d42-183">Your app is configured to target a specific version of .NET.</span></span> <span data-ttu-id="09d42-184">目标 .NET 运行时必须在运行应用的任何虚拟机上。</span><span class="sxs-lookup"><span data-stu-id="09d42-184">That targeted .NET runtime is required to be on any machine where your app runs.</span></span> <span data-ttu-id="09d42-185">例如，如果你的应用以 .NET Core 3.1 为目标，则任何运行你的应用的虚拟机都必须安装 .NET Core 3.1 运行时。</span><span class="sxs-lookup"><span data-stu-id="09d42-185">For example, if your app targets .NET Core 3.1, any machine that your app runs on must have the .NET Core 3.1 runtime installed.</span></span> <span data-ttu-id="09d42-186">如[发布基础知识](#publishing-basics)部分中所述，可以编辑项目文件为更改默认目标框架或面向多个框架。</span><span class="sxs-lookup"><span data-stu-id="09d42-186">As stated in the [Publishing basics](#publishing-basics) section, you can edit your project file to change the default target framework or to target more than one framework.</span></span>

<span data-ttu-id="09d42-187">发布 FDE 会创建一个应用，此应用会自动前滚到运行此应用的系统上可用的最新 .NET 安全补丁。</span><span class="sxs-lookup"><span data-stu-id="09d42-187">Publishing an FDE creates an app that automatically rolls-forward to the latest .NET security patch available on the system that runs the app.</span></span> <span data-ttu-id="09d42-188">若要详细了解编译时的版本绑定，请参阅[选择要使用的 .NET 版本](../versions/selection.md#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="09d42-188">For more information on version binding at compile time, see [Select the .NET version to use](../versions/selection.md#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="09d42-189">对于 .NET 2.1，必须结合使用以下开关和 `dotnet publish` 命令来发布 FDE：</span><span class="sxs-lookup"><span data-stu-id="09d42-189">For .NET 2.1, you must use the following switches with the `dotnet publish` command to publish an FDE:</span></span>

- <span data-ttu-id="09d42-190">`-r <RID>` 此开关使用标识符 (RID) 来指定目标平台。</span><span class="sxs-lookup"><span data-stu-id="09d42-190">`-r <RID>` This switch uses an identifier (RID) to specify the target platform.</span></span> <span data-ttu-id="09d42-191">有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-191">For a list of runtime identifiers, see [Runtime Identifier (RID) catalog](../rid-catalog.md).</span></span>

- <span data-ttu-id="09d42-192">`--self-contained false` 此开关会禁用 `-r` 开关的默认行为，即禁止创建独立部署 (SCD)。</span><span class="sxs-lookup"><span data-stu-id="09d42-192">`--self-contained false` This switch disables the default behavior of the `-r` switch, which is to create a self-contained deployment (SCD).</span></span> <span data-ttu-id="09d42-193">此开关会创建一个 FDE。</span><span class="sxs-lookup"><span data-stu-id="09d42-193">This switch creates an FDE.</span></span>

| <span data-ttu-id="09d42-194">发布模式</span><span class="sxs-lookup"><span data-stu-id="09d42-194">Publish Mode</span></span>                   | <span data-ttu-id="09d42-195">SDK 版本</span><span class="sxs-lookup"><span data-stu-id="09d42-195">SDK Version</span></span> | <span data-ttu-id="09d42-196">命令</span><span class="sxs-lookup"><span data-stu-id="09d42-196">Command</span></span>                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| <span data-ttu-id="09d42-197">依赖于框架的可执行文件</span><span class="sxs-lookup"><span data-stu-id="09d42-197">Framework-dependent executable</span></span> | <span data-ttu-id="09d42-198">3.1</span><span class="sxs-lookup"><span data-stu-id="09d42-198">3.1</span></span>         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |
|                                | <span data-ttu-id="09d42-199">5.0</span><span class="sxs-lookup"><span data-stu-id="09d42-199">5.0</span></span>         | `dotnet publish -c Release -r <RID> --self-contained false`<br>`dotnet publish -c Release` |

<span data-ttu-id="09d42-200">每次使用 `-r` 开关时，输出文件路都将更改为：`./bin/<BUILD-CONFIGURATION>/<TFM>/<RID>/publish/`</span><span class="sxs-lookup"><span data-stu-id="09d42-200">Whenever you use the `-r` switch, the output folder path changes to: `./bin/<BUILD-CONFIGURATION>/<TFM>/<RID>/publish/`</span></span>

<span data-ttu-id="09d42-201">如果使用[示例应用](#sample-app)，请运行 `dotnet publish -f net5.0 -r win10-x64 --self-contained false`。</span><span class="sxs-lookup"><span data-stu-id="09d42-201">If you use the [example app](#sample-app), run `dotnet publish -f net5.0 -r win10-x64 --self-contained false`.</span></span> <span data-ttu-id="09d42-202">此命令将创建以下可执行文件：`./bin/Debug/net5.0/win10-x64/publish/apptest1.exe`</span><span class="sxs-lookup"><span data-stu-id="09d42-202">This command creates the following executable: `./bin/Debug/net5.0/win10-x64/publish/apptest1.exe`</span></span>

> [!NOTE]
> <span data-ttu-id="09d42-203">可以通过启用全局固定模式来降低部署的总大小。</span><span class="sxs-lookup"><span data-stu-id="09d42-203">You can reduce the total size of your deployment by enabling **globalization invariant mode**.</span></span> <span data-ttu-id="09d42-204">此模式适用于不具有全局意识且可以使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的格式约定、大小写约定以及字符串比较和排序顺序的应用程序。</span><span class="sxs-lookup"><span data-stu-id="09d42-204">This mode is useful for applications that are not globally aware and that can use the formatting conventions, casing conventions, and string comparison and sort order of the [invariant culture](xref:System.Globalization.CultureInfo.InvariantCulture).</span></span> <span data-ttu-id="09d42-205">若要详细了解全球化固定模式以及如何启用它，请参阅 [.NET 全球化固定模式](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-205">For more information about **globalization invariant mode** and how to enable it, see [.NET Globalization Invariant Mode](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md).</span></span>

## <a name="self-contained-deployment"></a><span data-ttu-id="09d42-206">独立部署</span><span class="sxs-lookup"><span data-stu-id="09d42-206">Self-contained deployment</span></span>

<span data-ttu-id="09d42-207">当你发布独立式部署 (SCD) 时，.NET SDK 会创建特定于平台的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="09d42-207">When you publish a self-contained deployment (SCD), the .NET SDK creates a platform-specific executable.</span></span> <span data-ttu-id="09d42-208">如果发布 SCD，则会包括运行应用所需的所有 .NET 文件，但不包括 [.NET 的本机依赖项](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-208">Publishing an SCD includes all required .NET files to run your app but it doesn't include the [native dependencies of .NET](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md).</span></span> <span data-ttu-id="09d42-209">这些依赖项必须在应用运行前存在于系统中。</span><span class="sxs-lookup"><span data-stu-id="09d42-209">These dependencies must be present on the system before the app runs.</span></span>

<span data-ttu-id="09d42-210">如果发布 SCD，则会创建一个不前滚到最新可用 .NET 安全补丁的应用。</span><span class="sxs-lookup"><span data-stu-id="09d42-210">Publishing an SCD creates an app that doesn't roll forward to the latest available .NET security patch.</span></span> <span data-ttu-id="09d42-211">若要详细了解编译时的版本绑定，请参阅[选择要使用的 .NET 版本](../versions/selection.md#self-contained-deployments-include-the-selected-runtime)。</span><span class="sxs-lookup"><span data-stu-id="09d42-211">For more information on version binding at compile time, see [Select the .NET version to use](../versions/selection.md#self-contained-deployments-include-the-selected-runtime).</span></span>

<span data-ttu-id="09d42-212">必须通过 `dotnet publish` 命令使用以下开关来发布 SCD：</span><span class="sxs-lookup"><span data-stu-id="09d42-212">You must use the following switches with the `dotnet publish` command to publish an SCD:</span></span>

- <span data-ttu-id="09d42-213">`-r <RID>` 此开关使用标识符 (RID) 来指定目标平台。</span><span class="sxs-lookup"><span data-stu-id="09d42-213">`-r <RID>` This switch uses an identifier (RID) to specify the target platform.</span></span> <span data-ttu-id="09d42-214">有关运行时标识符的详细信息，请参阅[运行时标识符 (RID) 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-214">For a list of runtime identifiers, see [Runtime Identifier (RID) catalog](../rid-catalog.md).</span></span>

- <span data-ttu-id="09d42-215">`--self-contained true` 此开关告知 .NET Core SDK 创建可执行文件作为 SCD。</span><span class="sxs-lookup"><span data-stu-id="09d42-215">`--self-contained true` This switch tells the .NET Core SDK to create an executable as an SCD.</span></span>

| <span data-ttu-id="09d42-216">发布模式</span><span class="sxs-lookup"><span data-stu-id="09d42-216">Publish Mode</span></span>                   | <span data-ttu-id="09d42-217">SDK 版本</span><span class="sxs-lookup"><span data-stu-id="09d42-217">SDK Version</span></span> | <span data-ttu-id="09d42-218">命令</span><span class="sxs-lookup"><span data-stu-id="09d42-218">Command</span></span>                                                     |
|--------------------------------|-------------|-------------------------------------------------------------|
| <span data-ttu-id="09d42-219">独立部署</span><span class="sxs-lookup"><span data-stu-id="09d42-219">Self-contained deployment</span></span>      | <span data-ttu-id="09d42-220">2.1</span><span class="sxs-lookup"><span data-stu-id="09d42-220">2.1</span></span>         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | <span data-ttu-id="09d42-221">3.1</span><span class="sxs-lookup"><span data-stu-id="09d42-221">3.1</span></span>         | `dotnet publish -c Release -r <RID> --self-contained true`  |
|                                | <span data-ttu-id="09d42-222">5.0</span><span class="sxs-lookup"><span data-stu-id="09d42-222">5.0</span></span>         | `dotnet publish -c Release -r <RID> --self-contained true`  |

> [!NOTE]
> <span data-ttu-id="09d42-223">可以通过启用全局固定模式来降低部署的总大小。</span><span class="sxs-lookup"><span data-stu-id="09d42-223">You can reduce the total size of your deployment by enabling **globalization invariant mode**.</span></span> <span data-ttu-id="09d42-224">此模式适用于不具有全局意识且可以使用[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture)的格式约定、大小写约定以及字符串比较和排序顺序的应用程序。</span><span class="sxs-lookup"><span data-stu-id="09d42-224">This mode is useful for applications that are not globally aware and that can use the formatting conventions, casing conventions, and string comparison and sort order of the [invariant culture](xref:System.Globalization.CultureInfo.InvariantCulture).</span></span> <span data-ttu-id="09d42-225">有关全局固定模式及其启用方式的详细信息，请参阅 [.NET Core 全局固定模式](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)。</span><span class="sxs-lookup"><span data-stu-id="09d42-225">For more information about **globalization invariant mode** and how to enable it, see [.NET Core Globalization Invariant Mode](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="09d42-226">另请参阅</span><span class="sxs-lookup"><span data-stu-id="09d42-226">See also</span></span>

- [<span data-ttu-id="09d42-227">.NET 应用程序部署概述</span><span class="sxs-lookup"><span data-stu-id="09d42-227">.NET Application Deployment Overview</span></span>](index.md)
- [<span data-ttu-id="09d42-228">.NET 运行时标识符 (RID) 目录</span><span class="sxs-lookup"><span data-stu-id="09d42-228">.NET Runtime IDentifier (RID) catalog</span></span>](../rid-catalog.md)
