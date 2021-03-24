---
title: 应用程序发布
description: 了解发布 .NET 应用程序的方式。 .NET 可以发布特定于平台或跨平台的应用。 你可以将应用发布为独立应用或依赖于框架的应用。 每个模式都会影响用户运行应用的方式。
ms.date: 02/05/2021
ms.openlocfilehash: ca0fa4ff5e17a3fa4c636604ae81b63ffbbc99f6
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874285"
---
# <a name="net-application-publishing-overview"></a><span data-ttu-id="94dec-106">.NET 应用程序发布概述</span><span class="sxs-lookup"><span data-stu-id="94dec-106">.NET application publishing overview</span></span>

<span data-ttu-id="94dec-107">可在两种模式下发布使用 .NET 创建的应用程序，模式会影响用户运行应用的方式。</span><span class="sxs-lookup"><span data-stu-id="94dec-107">Applications you create with .NET can be published in two different modes, and the mode affects how a user runs your app.</span></span>

<span data-ttu-id="94dec-108">将应用作为独立应用，生成的应用程序将包含 .NET 运行时和库，以及该应用程序及其依赖项。</span><span class="sxs-lookup"><span data-stu-id="94dec-108">Publishing your app as *self-contained* produces an application that includes the .NET runtime and libraries, and your application and its dependencies.</span></span> <span data-ttu-id="94dec-109">应用程序的用户可以在未安装 .NET 运行时的计算机上运行该应用程序。</span><span class="sxs-lookup"><span data-stu-id="94dec-109">Users of the application can run it on a machine that doesn't have the .NET runtime installed.</span></span>

<span data-ttu-id="94dec-110">如果将应用发布为依赖于框架的应用，生成的应用程序将仅包含该应用程序本身及其依赖项。</span><span class="sxs-lookup"><span data-stu-id="94dec-110">Publishing your app as *framework-dependent* produces an application that includes only your application itself and its dependencies.</span></span> <span data-ttu-id="94dec-111">应用程序的用户必须单独安装 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-111">Users of the application have to separately install the .NET runtime.</span></span>

<span data-ttu-id="94dec-112">默认情况下，这两种发布模式都会生成特定于平台的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-112">Both publishing modes produce a platform-specific executable by default.</span></span> <span data-ttu-id="94dec-113">不使用可执行文件也可以创建依赖于框架的应用程序，这些应用程序是跨平台的。</span><span class="sxs-lookup"><span data-stu-id="94dec-113">Framework-dependent applications can be created without an executable, and these applications are cross-platform.</span></span>

<span data-ttu-id="94dec-114">生成可执行文件时，可以使用运行时标识符 (RID) 指定目标平台。</span><span class="sxs-lookup"><span data-stu-id="94dec-114">When an executable is produced, you can specify the target platform with a runtime identifier (RID).</span></span> <span data-ttu-id="94dec-115">有关 RID 的详细信息，请参阅 [.NET RID 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="94dec-115">For more information about RIDs, see [.NET RID Catalog](../rid-catalog.md).</span></span>

<span data-ttu-id="94dec-116">下表概述了每个 SDK 版本用于将应用发布为依赖于框架的应用或独立应用的命令：</span><span class="sxs-lookup"><span data-stu-id="94dec-116">The following table outlines the commands used to publish an app as framework-dependent or self-contained, per SDK version:</span></span>

| <span data-ttu-id="94dec-117">类型</span><span class="sxs-lookup"><span data-stu-id="94dec-117">Type</span></span>                                                                                     | <span data-ttu-id="94dec-118">SDK 2.1</span><span class="sxs-lookup"><span data-stu-id="94dec-118">SDK 2.1</span></span> | <span data-ttu-id="94dec-119">SDK 3.1</span><span class="sxs-lookup"><span data-stu-id="94dec-119">SDK 3.1</span></span> | <span data-ttu-id="94dec-120">SDK 5.0</span><span class="sxs-lookup"><span data-stu-id="94dec-120">SDK 5.0</span></span> | <span data-ttu-id="94dec-121">命令</span><span class="sxs-lookup"><span data-stu-id="94dec-121">Command</span></span> |
| ---------------------------------------------------------------------------------------  | ------- | ------- | ------- | ------- |
| <span data-ttu-id="94dec-122">适用于当前平台的[依赖于框架的可执行文件](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-122">[framework-dependent executable](#publish-framework-dependent) for the current platform.</span></span> |         | <span data-ttu-id="94dec-123">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-123">✔️</span></span>      | <span data-ttu-id="94dec-124">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-124">✔️</span></span>      | [`dotnet publish`](../tools/dotnet-publish.md) |
| <span data-ttu-id="94dec-125">适用于特定平台的[依赖于框架的可执行文件](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-125">[framework-dependent executable](#publish-framework-dependent) for a specific platform.</span></span>  |         | <span data-ttu-id="94dec-126">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-126">✔️</span></span>      | <span data-ttu-id="94dec-127">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-127">✔️</span></span>      | [`dotnet publish -r <RID> --self-contained false`](../tools/dotnet-publish.md) |
| <span data-ttu-id="94dec-128">[依赖于框架的跨平台二进制文件](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-128">[framework-dependent cross-platform binary](#publish-framework-dependent).</span></span>               | <span data-ttu-id="94dec-129">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-129">✔️</span></span>      | <span data-ttu-id="94dec-130">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-130">✔️</span></span>      | <span data-ttu-id="94dec-131">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-131">✔️</span></span>      | [`dotnet publish`](../tools/dotnet-publish.md) |
| <span data-ttu-id="94dec-132">[独立可执行文件](#publish-self-contained)。</span><span class="sxs-lookup"><span data-stu-id="94dec-132">[self-contained executable](#publish-self-contained).</span></span>                                    | <span data-ttu-id="94dec-133">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-133">✔️</span></span>      | <span data-ttu-id="94dec-134">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-134">✔️</span></span>      | <span data-ttu-id="94dec-135">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-135">✔️</span></span>      | [`dotnet publish -r <RID>`](../tools/dotnet-publish.md) |

<span data-ttu-id="94dec-136">有关详细信息，请参阅 [.NET dotnet publish 命令](../tools/dotnet-publish.md)。</span><span class="sxs-lookup"><span data-stu-id="94dec-136">For more information, see [.NET dotnet publish command](../tools/dotnet-publish.md).</span></span>

## <a name="produce-an-executable"></a><span data-ttu-id="94dec-137">生成可执行文件</span><span class="sxs-lookup"><span data-stu-id="94dec-137">Produce an executable</span></span>

<span data-ttu-id="94dec-138">可执行文件不是跨平台的。</span><span class="sxs-lookup"><span data-stu-id="94dec-138">Executables aren't cross-platform.</span></span> <span data-ttu-id="94dec-139">它们特定于操作系统和 CPU 体系结构。</span><span class="sxs-lookup"><span data-stu-id="94dec-139">They're specific to an operating system and CPU architecture.</span></span> <span data-ttu-id="94dec-140">发布应用并创建可执行文件时，可以将应用发布为[独立应用](#publish-self-contained)或[依赖于框架的应用](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-140">When publishing your app and creating an executable, you can publish the app as [self-contained](#publish-self-contained) or [framework-dependent](#publish-framework-dependent).</span></span> <span data-ttu-id="94dec-141">将应用发布为独立应用，会在应用中包含 .NET 运行时，该应用的用户无需在运行应用前安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="94dec-141">Publishing an app as self-contained includes the .NET runtime with the app, and users of the app don't have to worry about installing .NET before running the app.</span></span> <span data-ttu-id="94dec-142">如果将应用发布为依赖于框架的应用，则该应用不包含 .NET 运行时和库，而仅包含该应用和第三方依赖项。</span><span class="sxs-lookup"><span data-stu-id="94dec-142">Apps published as framework-dependent don't include the .NET runtime and libraries; only the app and 3rd-party dependencies are included.</span></span>

<span data-ttu-id="94dec-143">以下命令可生成可执行文件：</span><span class="sxs-lookup"><span data-stu-id="94dec-143">The following commands produce an executable:</span></span>

| <span data-ttu-id="94dec-144">类型</span><span class="sxs-lookup"><span data-stu-id="94dec-144">Type</span></span>                                                                                     | <span data-ttu-id="94dec-145">SDK 2.1</span><span class="sxs-lookup"><span data-stu-id="94dec-145">SDK 2.1</span></span> | <span data-ttu-id="94dec-146">SDK 3.1</span><span class="sxs-lookup"><span data-stu-id="94dec-146">SDK 3.1</span></span> | <span data-ttu-id="94dec-147">SDK 5.0</span><span class="sxs-lookup"><span data-stu-id="94dec-147">SDK 5.0</span></span> | <span data-ttu-id="94dec-148">命令</span><span class="sxs-lookup"><span data-stu-id="94dec-148">Command</span></span> |
| ---------------------------------------------------------------------------------------- | ------- | ------- | ------- | ------- |
| <span data-ttu-id="94dec-149">适用于当前平台的[依赖于框架的可执行文件](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-149">[framework-dependent executable](#publish-framework-dependent) for the current platform.</span></span> |         | <span data-ttu-id="94dec-150">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-150">✔️</span></span>      | <span data-ttu-id="94dec-151">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-151">✔️</span></span>      | [`dotnet publish`](../tools/dotnet-publish.md) |
| <span data-ttu-id="94dec-152">适用于特定平台的[依赖于框架的可执行文件](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-152">[framework-dependent executable](#publish-framework-dependent) for a specific platform.</span></span>  |         | <span data-ttu-id="94dec-153">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-153">✔️</span></span>      | <span data-ttu-id="94dec-154">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-154">✔️</span></span>      | [`dotnet publish -r <RID> --self-contained false`](../tools/dotnet-publish.md) |
| <span data-ttu-id="94dec-155">[独立可执行文件](#publish-self-contained)。</span><span class="sxs-lookup"><span data-stu-id="94dec-155">[self-contained executable](#publish-self-contained).</span></span>                                    | <span data-ttu-id="94dec-156">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-156">✔️</span></span>      | <span data-ttu-id="94dec-157">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-157">✔️</span></span>      | <span data-ttu-id="94dec-158">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-158">✔️</span></span>      | [`dotnet publish -r <RID>`](../tools/dotnet-publish.md) |

## <a name="produce-a-cross-platform-binary"></a><span data-ttu-id="94dec-159">生成跨平台二进制文件</span><span class="sxs-lookup"><span data-stu-id="94dec-159">Produce a cross-platform binary</span></span>

<span data-ttu-id="94dec-160">以 dll 文件的形式将应用发布为[依赖于框架的应用](#publish-framework-dependent)时，将创建跨平台二进制文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-160">Cross-platform binaries are created when you publish your app as [framework-dependent](#publish-framework-dependent), in the form of a *dll* file.</span></span> <span data-ttu-id="94dec-161">dll 文件将与项目同名。</span><span class="sxs-lookup"><span data-stu-id="94dec-161">The *dll* file is named after your project.</span></span> <span data-ttu-id="94dec-162">例如，如果有名为 word_reader 的应用，则会创建名为 word_reader.dll 的文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-162">For example, if you have an app named **word_reader**, a file named *word_reader.dll* is created.</span></span> <span data-ttu-id="94dec-163">以这种方式发布的应用可通过 `dotnet <filename.dll>` 命令运行，并且可在任意平台上运行。</span><span class="sxs-lookup"><span data-stu-id="94dec-163">Apps published in this way are run with the `dotnet <filename.dll>` command and can be run on any platform.</span></span>

<span data-ttu-id="94dec-164">只要安装了目标 .NET 运行时，就可以在任何操作系统上运行跨平台二进制文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-164">Cross-platform binaries can be run on any operating system as long as the targeted .NET runtime is already installed.</span></span> <span data-ttu-id="94dec-165">如果未安装目标 .NET 运行时，如果将应用配置为前滚，则它可以使用较新的运行时运行。</span><span class="sxs-lookup"><span data-stu-id="94dec-165">If the targeted .NET runtime isn't installed, the app may run using a newer runtime if the app is configured to roll-forward.</span></span> <span data-ttu-id="94dec-166">有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="94dec-166">For more information, see [framework-dependent apps roll forward](../versions/selection.md#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="94dec-167">以下命令可生成跨平台二进制文件：</span><span class="sxs-lookup"><span data-stu-id="94dec-167">The following command produces a cross-platform binary:</span></span>

| <span data-ttu-id="94dec-168">类型</span><span class="sxs-lookup"><span data-stu-id="94dec-168">Type</span></span>                                                                                 | <span data-ttu-id="94dec-169">SDK 2.1</span><span class="sxs-lookup"><span data-stu-id="94dec-169">SDK 2.1</span></span> | <span data-ttu-id="94dec-170">SDK 3.x</span><span class="sxs-lookup"><span data-stu-id="94dec-170">SDK 3.x</span></span> | <span data-ttu-id="94dec-171">SDK 5.0</span><span class="sxs-lookup"><span data-stu-id="94dec-171">SDK 5.0</span></span> | <span data-ttu-id="94dec-172">命令</span><span class="sxs-lookup"><span data-stu-id="94dec-172">Command</span></span> |
| -----------------------------------------------------------------------------------  | ------- | ------- | ------- | ------- |
| <span data-ttu-id="94dec-173">[依赖于框架的跨平台二进制文件](#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="94dec-173">[framework-dependent cross-platform binary](#publish-framework-dependent).</span></span>           | <span data-ttu-id="94dec-174">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-174">✔️</span></span>      | <span data-ttu-id="94dec-175">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-175">✔️</span></span>      | <span data-ttu-id="94dec-176">✔️</span><span class="sxs-lookup"><span data-stu-id="94dec-176">✔️</span></span>      | [`dotnet publish`](../tools/dotnet-publish.md) |

## <a name="publish-framework-dependent"></a><span data-ttu-id="94dec-177">发布依赖于框架的应用</span><span class="sxs-lookup"><span data-stu-id="94dec-177">Publish framework-dependent</span></span>

<span data-ttu-id="94dec-178">如果将应用发布为依赖于框架的应用，则该应用是跨平台的，且不包含 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-178">Apps published as framework-dependent are cross-platform and don't include the .NET runtime.</span></span> <span data-ttu-id="94dec-179">应用的用户需要安装 .NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-179">The user of your app is required to install the .NET runtime.</span></span>

<span data-ttu-id="94dec-180">如果将应用发布为依赖于框架的应用，会以 dll 文件的形式生成一个[跨平台二进制文件](#produce-a-cross-platform-binary)，还会生成面向当前平台的[特定于平台的可执行文件](#produce-an-executable)。</span><span class="sxs-lookup"><span data-stu-id="94dec-180">Publishing an app as framework-dependent produces a [cross-platform binary](#produce-a-cross-platform-binary) as a *dll* file, and a [platform-specific executable](#produce-an-executable) that targets your current platform.</span></span> <span data-ttu-id="94dec-181">dll 是跨平台的，而可执行文件不是。</span><span class="sxs-lookup"><span data-stu-id="94dec-181">The *dll* is cross-platform while the executable isn't.</span></span> <span data-ttu-id="94dec-182">例如，如果发布名为 word_reader 的应用且面向 Windows，则将创建 word_reader.exe 和 word_reader.dll。</span><span class="sxs-lookup"><span data-stu-id="94dec-182">For example, if you publish an app named **word_reader** and target Windows, a *word_reader.exe* executable is created along with *word_reader.dll*.</span></span> <span data-ttu-id="94dec-183">面向 Linux 或 macOS 时，将创建 word_reader 可执行文件和 word_reader.dll。</span><span class="sxs-lookup"><span data-stu-id="94dec-183">When targeting Linux or macOS, a *word_reader* executable is created along with *word_reader.dll*.</span></span> <span data-ttu-id="94dec-184">有关 RID 的详细信息，请参阅 [.NET RID 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="94dec-184">For more information about RIDs, see [.NET RID Catalog](../rid-catalog.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="94dec-185">当你发布依赖于框架的应用时，.NET SDK 2.1 不会生成特定于平台的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-185">.NET SDK 2.1 doesn't produce platform-specific executables when you publish an app framework-dependent.</span></span>

<span data-ttu-id="94dec-186">可以通过 `dotnet <filename.dll>` 命令运行应用的跨平台二进制文件，并且它可以在任何平台上运行。</span><span class="sxs-lookup"><span data-stu-id="94dec-186">The cross-platform binary of your app can be run with the `dotnet <filename.dll>` command, and can be run on any platform.</span></span> <span data-ttu-id="94dec-187">如果应用使用具有特定于平台的实现的 NuGet 包，则所有平台的依赖项都将连同应用一起复制到发布文件夹。</span><span class="sxs-lookup"><span data-stu-id="94dec-187">If the app uses a NuGet package that has platform-specific implementations, all platforms' dependencies are copied to the publish folder along with the app.</span></span>

<span data-ttu-id="94dec-188">可以通过将 `-r <RID> --self-contained false` 参数传递到 [`dotnet publish`](../tools/dotnet-publish.md) 命令，为特定平台创建可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-188">You can create an executable for a specific platform by passing the `-r <RID> --self-contained false` parameters to the [`dotnet publish`](../tools/dotnet-publish.md) command.</span></span> <span data-ttu-id="94dec-189">省略 `-r` 参数时，将为当前平台创建可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-189">When the `-r` parameter is omitted, an executable is created for your current platform.</span></span> <span data-ttu-id="94dec-190">具有特定于目标平台的依赖项的任何 NuGet 包都将复制到发布文件夹。</span><span class="sxs-lookup"><span data-stu-id="94dec-190">Any NuGet packages that have platform-specific dependencies for the targeted platform are copied to the publish folder.</span></span> <span data-ttu-id="94dec-191">如果不需要特定于平台的可执行文件，则可以在项目文件中指定 `<UseAppHost>False</UseAppHost>`。</span><span class="sxs-lookup"><span data-stu-id="94dec-191">If you don't need a platfrom-specific executable, you can specify `<UseAppHost>False</UseAppHost>` in the project file.</span></span> <span data-ttu-id="94dec-192">有关详细信息，请参阅[适用于 .NET SDK 项目的 MSBuild 参考](../project-sdk/msbuild-props.md#useapphost)。</span><span class="sxs-lookup"><span data-stu-id="94dec-192">For more information, see [MSBuild reference for .NET SDK projects](../project-sdk/msbuild-props.md#useapphost).</span></span>

### <a name="advantages"></a><span data-ttu-id="94dec-193">优点</span><span class="sxs-lookup"><span data-stu-id="94dec-193">Advantages</span></span>

- <span data-ttu-id="94dec-194">小型部署  </span><span class="sxs-lookup"><span data-stu-id="94dec-194">**Small deployment**</span></span>\
<span data-ttu-id="94dec-195">仅分发应用及其依赖项。</span><span class="sxs-lookup"><span data-stu-id="94dec-195">Only your app and its dependencies are distributed.</span></span> <span data-ttu-id="94dec-196">.NET 运行时和库由用户安装，所有应用共享运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-196">The .NET runtime and libraries are installed by the user and all apps share the runtime.</span></span>

- <span data-ttu-id="94dec-197">跨平台  </span><span class="sxs-lookup"><span data-stu-id="94dec-197">**Cross-platform**</span></span>\
<span data-ttu-id="94dec-198">应用和任何基于 .NET 的库都可在不同的操作系统上运行。</span><span class="sxs-lookup"><span data-stu-id="94dec-198">Your app and any .NET-based library runs on other operating systems.</span></span> <span data-ttu-id="94dec-199">无需为应用定义目标平台。</span><span class="sxs-lookup"><span data-stu-id="94dec-199">You don't need to define a target platform for your app.</span></span> <span data-ttu-id="94dec-200">有关 .NET 文件格式的详细信息，请参阅 [.NET 程序集文件格式](../../standard/assembly/file-format.md)。</span><span class="sxs-lookup"><span data-stu-id="94dec-200">For information about the .NET file format, see [.NET Assembly File Format](../../standard/assembly/file-format.md).</span></span>

- <span data-ttu-id="94dec-201">使用最新修补运行时  </span><span class="sxs-lookup"><span data-stu-id="94dec-201">**Uses the latest patched runtime**</span></span>\
<span data-ttu-id="94dec-202">应用会使用目标系统上安装的最新运行时（在 .NET 的目标大小系列中）。</span><span class="sxs-lookup"><span data-stu-id="94dec-202">The app uses the latest runtime (within the targeted major-minor family of .NET) installed on the target system.</span></span> <span data-ttu-id="94dec-203">这意味着应用将自动使用 .NET 运行时的最新修补版本。</span><span class="sxs-lookup"><span data-stu-id="94dec-203">This means your app automatically uses the latest patched version of the .NET runtime.</span></span> <span data-ttu-id="94dec-204">可以重写这一默认行为。</span><span class="sxs-lookup"><span data-stu-id="94dec-204">This default behavior can be overridden.</span></span> <span data-ttu-id="94dec-205">有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="94dec-205">For more information, see [framework-dependent apps roll forward](../versions/selection.md#framework-dependent-apps-roll-forward).</span></span>

### <a name="disadvantages"></a><span data-ttu-id="94dec-206">缺点</span><span class="sxs-lookup"><span data-stu-id="94dec-206">Disadvantages</span></span>

- <span data-ttu-id="94dec-207">**要求预先安装运行时**</span><span class="sxs-lookup"><span data-stu-id="94dec-207">**Requires pre-installing the runtime**</span></span>\
<span data-ttu-id="94dec-208">仅当主机系统上已安装应用设为目标的 .NET 版本时，应用才能运行。</span><span class="sxs-lookup"><span data-stu-id="94dec-208">Your app can run only if the version of .NET your app targets is already installed on the host system.</span></span> <span data-ttu-id="94dec-209">可以为应用配置前滚行为，要求使用特定版本的 .NET 或允许使用较新版本的 .NET。</span><span class="sxs-lookup"><span data-stu-id="94dec-209">You can configure roll-forward behavior for the app to either require a specific version of .NET or allow a newer version of .NET.</span></span> <span data-ttu-id="94dec-210">有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="94dec-210">For more information, see [framework-dependent apps roll forward](../versions/selection.md#framework-dependent-apps-roll-forward).</span></span>

- <span data-ttu-id="94dec-211">**.NET 可更改**</span><span class="sxs-lookup"><span data-stu-id="94dec-211">**.NET may change**</span></span>\
<span data-ttu-id="94dec-212">可以在运行应用的计算机上更新 .NET 运行时和库。</span><span class="sxs-lookup"><span data-stu-id="94dec-212">It's possible for the .NET runtime and libraries to be updated on the machine where the app is run.</span></span> <span data-ttu-id="94dec-213">在极少数情况下，如果使用 .NET 库（大多数应用都会使用），这可能会更改应用的行为。</span><span class="sxs-lookup"><span data-stu-id="94dec-213">In rare cases, this may change the behavior of your app if you use the .NET libraries, which most apps do.</span></span> <span data-ttu-id="94dec-214">可以配置应用如何使用较新版本的 .NET。</span><span class="sxs-lookup"><span data-stu-id="94dec-214">You can configure how your app uses newer versions of .NET.</span></span> <span data-ttu-id="94dec-215">有关详细信息，请参阅[依赖于框架的应用前滚](../versions/selection.md#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="94dec-215">For more information, see [framework-dependent apps roll forward](../versions/selection.md#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="94dec-216">以下缺陷仅限于.NET Core 2.1 SDK。</span><span class="sxs-lookup"><span data-stu-id="94dec-216">The following disadvantage only applies to .NET Core 2.1 SDK.</span></span>

- <span data-ttu-id="94dec-217">使用  `dotnet` 命令启动应用</span><span class="sxs-lookup"><span data-stu-id="94dec-217">**Use the `dotnet` command to start the app**</span></span>\
<span data-ttu-id="94dec-218">用户必须运行 `dotnet <filename.dll>` 命令来启动你的应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-218">Users must run the `dotnet <filename.dll>` command to start your app.</span></span> <span data-ttu-id="94dec-219">如果将应用发布为依赖于框架的应用，则 .NET Core 2.1 SDK 不会生成特定于平台的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-219">.NET Core 2.1 SDK doesn't produce platform-specific executables for apps published framework-dependent.</span></span>

### <a name="examples"></a><span data-ttu-id="94dec-220">示例</span><span class="sxs-lookup"><span data-stu-id="94dec-220">Examples</span></span>

<span data-ttu-id="94dec-221">发布依赖于框架的跨平台应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-221">Publish an app cross-platform framework-dependent.</span></span> <span data-ttu-id="94dec-222">与 *dll* 文件一起创建面向当前平台的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-222">An executable that targets your current platform is created along with the *dll* file.</span></span>

```dotnet
dotnet publish
```

<span data-ttu-id="94dec-223">发布依赖于框架的跨平台应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-223">Publish an app cross-platform framework-dependent.</span></span> <span data-ttu-id="94dec-224">与 *dll* 文件一起创建 Linux 64 位可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-224">A Linux 64-bit executable is created along with the *dll* file.</span></span> <span data-ttu-id="94dec-225">此命令对于 .NET Core SDK 2.1 无效。</span><span class="sxs-lookup"><span data-stu-id="94dec-225">This command doesn't work with .NET Core SDK 2.1.</span></span>

```dotnet
dotnet publish -r linux-x64 --self-contained false
```

## <a name="publish-self-contained"></a><span data-ttu-id="94dec-226">发布独立应用</span><span class="sxs-lookup"><span data-stu-id="94dec-226">Publish self-contained</span></span>

<span data-ttu-id="94dec-227">将应用发布为独立应用，将生成特定于平台的可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-227">Publishing your app as self-contained produces a platform-specific executable.</span></span> <span data-ttu-id="94dec-228">输出发布文件夹包含应用的所有组件，包括 .NET 库和目标运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-228">The output publishing folder contains all components of the app, including the .NET libraries and target runtime.</span></span> <span data-ttu-id="94dec-229">应用独立于其他 .NET 应用，且不使用本地安装的共享运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-229">The app is isolated from other .NET apps and doesn't use a locally installed shared runtime.</span></span> <span data-ttu-id="94dec-230">应用的用户无需下载和安装 .NET。</span><span class="sxs-lookup"><span data-stu-id="94dec-230">The user of your app isn't required to download and install .NET.</span></span>

<span data-ttu-id="94dec-231">针对指定的目标平台生成可执行二进制文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-231">The executable binary is produced for the specified target platform.</span></span> <span data-ttu-id="94dec-232">例如，如果你有一个名为 word_reader 的应用，并发布适用于 Windows 的独立可执行文件，则将创建 word_reader.exe 文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-232">For example, if you have an app named **word_reader**, and you publish a self-contained executable for Windows, a *word_reader.exe* file is created.</span></span> <span data-ttu-id="94dec-233">针对 Linux 或 macOS 发布时，将创建 word_reader  文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-233">Publishing for Linux or macOS, a *word_reader* file is created.</span></span> <span data-ttu-id="94dec-234">用 [`dotnet publish`](../tools/dotnet-publish.md) 命令的 `-r <RID>` 参数指定目标平台和体系结构。</span><span class="sxs-lookup"><span data-stu-id="94dec-234">The target platform and architecture is specified with the `-r <RID>` parameter for the [`dotnet publish`](../tools/dotnet-publish.md) command.</span></span> <span data-ttu-id="94dec-235">有关 RID 的详细信息，请参阅 [.NET RID 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="94dec-235">For more information about RIDs, see [.NET RID Catalog](../rid-catalog.md).</span></span>

<span data-ttu-id="94dec-236">如果应用具有特定于平台的依赖项（例如包含特定于平台的依赖项的 NuGet 包），这些依赖项将与应用一起复制到发布文件夹。</span><span class="sxs-lookup"><span data-stu-id="94dec-236">If the app has platform-specific dependencies, such as a NuGet package containing platform-specific dependencies, these are copied to the publish folder along with the app.</span></span>

### <a name="advantages"></a><span data-ttu-id="94dec-237">优点</span><span class="sxs-lookup"><span data-stu-id="94dec-237">Advantages</span></span>

- <span data-ttu-id="94dec-238">**控制 .NET 版本**</span><span class="sxs-lookup"><span data-stu-id="94dec-238">**Control .NET version**</span></span>\
<span data-ttu-id="94dec-239">你可以控制随应用部署的 .NET 版本。</span><span class="sxs-lookup"><span data-stu-id="94dec-239">You control which version of .NET is deployed with your app.</span></span>

- <span data-ttu-id="94dec-240">特定于平台的定向  </span><span class="sxs-lookup"><span data-stu-id="94dec-240">**Platform-specific targeting**</span></span>\
<span data-ttu-id="94dec-241">由于你必须为每个平台发布应用，因此你知道应用可运行于哪些平台。</span><span class="sxs-lookup"><span data-stu-id="94dec-241">Because you have to publish your app for each platform, you know where your app will run.</span></span> <span data-ttu-id="94dec-242">如果 .NET 引入了新平台，则用户无法在该新平台上运行你的应用，直到你发布面向该平台的版本。</span><span class="sxs-lookup"><span data-stu-id="94dec-242">If .NET introduces a new platform, users can't run your app on that platform until you release a version targeting that platform.</span></span> <span data-ttu-id="94dec-243">在用户在新平台上运行你的应用之前，你可以测试应用以排除兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="94dec-243">You can test your app for compatibility problems before your users run your app on the new platform.</span></span>

### <a name="disadvantages"></a><span data-ttu-id="94dec-244">缺点</span><span class="sxs-lookup"><span data-stu-id="94dec-244">Disadvantages</span></span>

- <span data-ttu-id="94dec-245">大型部署  </span><span class="sxs-lookup"><span data-stu-id="94dec-245">**Larger deployments**</span></span>\
<span data-ttu-id="94dec-246">由于你的应用包含 .NET 运行时和所有应用依赖项，因此下载大小和所需硬盘空间比[依赖于框架的应用](#publish-framework-dependent)的版本要大。</span><span class="sxs-lookup"><span data-stu-id="94dec-246">Because your app includes the .NET runtime and all of your app dependencies, the download size and hard drive space required is greater than a [framework-dependent](#publish-framework-dependent) version.</span></span>

  > [!TIP]
  > <span data-ttu-id="94dec-247">可以通过使用 .NET [全球化固定模式](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md)在 Linux 系统上减少大约 28 MB 的部署大小。</span><span class="sxs-lookup"><span data-stu-id="94dec-247">You can reduce the size of your deployment on Linux systems by approximately 28 MB by using .NET [*globalization invariant mode*](https://github.com/dotnet/runtime/blob/main/docs/design/features/globalization-invariant-mode.md).</span></span> <span data-ttu-id="94dec-248">这会强制应用像处理[固定区域性](xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType)一样处理所有区域性。</span><span class="sxs-lookup"><span data-stu-id="94dec-248">This forces your app to treat all cultures like the [invariant culture](xref:System.Globalization.CultureInfo.InvariantCulture?displayProperty=nameWithType).</span></span>

  > [!TIP]
  > <span data-ttu-id="94dec-249">存在一项[预览剪裁功能](trim-self-contained.md)，它可进一步减小部署的大小。</span><span class="sxs-lookup"><span data-stu-id="94dec-249">There is a [preview Trim feature](trim-self-contained.md) that can further reduce the size of your deployment.</span></span>

- <span data-ttu-id="94dec-250">**较难更新 .NET 版本**</span><span class="sxs-lookup"><span data-stu-id="94dec-250">**Harder to update the .NET version**</span></span>\
<span data-ttu-id="94dec-251">只能通过发布新版本的应用来升级（与应用一起分发的）.NET 运行时。</span><span class="sxs-lookup"><span data-stu-id="94dec-251">.NET Runtime (distributed with your app) can only be upgraded by releasing a new version of your app.</span></span> <span data-ttu-id="94dec-252">但是，.NET 将在运行你的应用的计算机中针对框架库按需更新关键安全修补程序。</span><span class="sxs-lookup"><span data-stu-id="94dec-252">However, .NET will update critical security patches as needed for the framework library in the  machine that your app runs on.</span></span> <span data-ttu-id="94dec-253">由你负责此安全修补程序方案的端到端验证。</span><span class="sxs-lookup"><span data-stu-id="94dec-253">You are responsible for end to end validation for this security patch scenario.</span></span>

### <a name="examples"></a><span data-ttu-id="94dec-254">示例</span><span class="sxs-lookup"><span data-stu-id="94dec-254">Examples</span></span>

<span data-ttu-id="94dec-255">发布独立应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-255">Publish an app self-contained.</span></span> <span data-ttu-id="94dec-256">创建 macOS 64 位可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-256">A macOS 64-bit executable is created.</span></span>

```dotnet
dotnet publish -r osx-x64
```

<span data-ttu-id="94dec-257">发布独立应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-257">Publish an app self-contained.</span></span> <span data-ttu-id="94dec-258">创建 Windows 64 位可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-258">A Windows 64-bit executable is created.</span></span>

```dotnet
dotnet publish -r win-x64
```

## <a name="publish-with-readytorun-images"></a><span data-ttu-id="94dec-259">使用 ReadyToRun 映像发布</span><span class="sxs-lookup"><span data-stu-id="94dec-259">Publish with ReadyToRun images</span></span>

<span data-ttu-id="94dec-260">使用 ReadyToRun 映像发布可以缩短应用程序的启动时间，但代价是增加应用程序的大小。</span><span class="sxs-lookup"><span data-stu-id="94dec-260">Publishing with ReadyToRun images will improve the startup time of your application at the cost of increasing the size of your application.</span></span> <span data-ttu-id="94dec-261">若要使用 ReadyToRun 映像发布，请参阅 [ReadyToRun](ready-to-run.md) 来了解更多详情。</span><span class="sxs-lookup"><span data-stu-id="94dec-261">In order to publish with ReadyToRun see [ReadyToRun](ready-to-run.md) for more details.</span></span>

### <a name="advantages"></a><span data-ttu-id="94dec-262">优点</span><span class="sxs-lookup"><span data-stu-id="94dec-262">Advantages</span></span>

- <span data-ttu-id="94dec-263">缩短了启动时间</span><span class="sxs-lookup"><span data-stu-id="94dec-263">**Improved startup time**</span></span>\
<span data-ttu-id="94dec-264">应用程序运行 JIT 所花费的时间将缩短。</span><span class="sxs-lookup"><span data-stu-id="94dec-264">The application will spend less time running the JIT.</span></span>

### <a name="disadvantages"></a><span data-ttu-id="94dec-265">缺点</span><span class="sxs-lookup"><span data-stu-id="94dec-265">Disadvantages</span></span>

- <span data-ttu-id="94dec-266">增加了大小</span><span class="sxs-lookup"><span data-stu-id="94dec-266">**Larger size**</span></span>\
<span data-ttu-id="94dec-267">应用程序在磁盘上的大小将增加。</span><span class="sxs-lookup"><span data-stu-id="94dec-267">The application will be larger on disk.</span></span>

### <a name="examples"></a><span data-ttu-id="94dec-268">示例</span><span class="sxs-lookup"><span data-stu-id="94dec-268">Examples</span></span>

<span data-ttu-id="94dec-269">使用 ReadyToRun 映像发布独立式应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-269">Publish an app self-contained and ReadyToRun.</span></span> <span data-ttu-id="94dec-270">创建 macOS 64 位可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-270">A macOS 64-bit executable is created.</span></span>

```dotnet
dotnet publish -c Release -r osx-x64 -p:PublishReadyToRun=true
```

<span data-ttu-id="94dec-271">使用 ReadyToRun 映像发布独立式应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-271">Publish an app self-contained and ReadyToRun.</span></span> <span data-ttu-id="94dec-272">创建 Windows 64 位可执行文件。</span><span class="sxs-lookup"><span data-stu-id="94dec-272">A Windows 64-bit executable is created.</span></span>

```dotnet
dotnet publish -c Release -r win-x64 -p:PublishReadyToRun=true
```

## <a name="see-also"></a><span data-ttu-id="94dec-273">请参阅</span><span class="sxs-lookup"><span data-stu-id="94dec-273">See also</span></span>

- [<span data-ttu-id="94dec-274">使用 .NET CLI 部署 .NET 应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-274">Deploying .NET Apps with .NET CLI.</span></span>](deploy-with-cli.md)
- [<span data-ttu-id="94dec-275">使用 Visual Studio 部署 .NET 应用。</span><span class="sxs-lookup"><span data-stu-id="94dec-275">Deploying .NET Apps with Visual Studio.</span></span>](deploy-with-vs.md)
- [<span data-ttu-id="94dec-276">.NET 运行时标识符 (RID) 目录。</span><span class="sxs-lookup"><span data-stu-id="94dec-276">.NET Runtime Identifier (RID) catalog.</span></span>](../rid-catalog.md)
- [<span data-ttu-id="94dec-277">选择要使用的 .NET 版本。</span><span class="sxs-lookup"><span data-stu-id="94dec-277">Select the .NET version to use.</span></span>](../versions/selection.md)
