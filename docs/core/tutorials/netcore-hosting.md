---
title: 编写自定义 .NET Core 运行时主机
description: 了解从本机代码托管 .NET Core 运行时，以支持需要控制 .NET Core 运行时工作方式的高级方案。
author: mjrousos
ms.topic: how-to
ms.date: 12/21/2018
ms.openlocfilehash: a05e1e1d09f1f988ad8354f98521681e3f6cbea8
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875845"
---
# <a name="write-a-custom-net-core-host-to-control-the-net-runtime-from-your-native-code"></a><span data-ttu-id="9f997-103">编写自定义 .NET Core 主机以从本机代码控制 .NET 运行时</span><span class="sxs-lookup"><span data-stu-id="9f997-103">Write a custom .NET Core host to control the .NET runtime from your native code</span></span>

<span data-ttu-id="9f997-104">像所有的托管代码一样，.NET Core 应用程序也由主机执行。</span><span class="sxs-lookup"><span data-stu-id="9f997-104">Like all managed code, .NET Core applications are executed by a host.</span></span> <span data-ttu-id="9f997-105">主机负责启动运行时（包括 JIT 和垃圾回收器等组件）和调用托管的入口点。</span><span class="sxs-lookup"><span data-stu-id="9f997-105">The host is responsible for starting the runtime (including components like the JIT and garbage collector) and invoking managed entry points.</span></span>

<span data-ttu-id="9f997-106">托管 .NET Core 运行时是高级方案，在大多数情况下，.NET Core 开发人员无需担心托管问题，因为 .NET Core 生成过程会提供默认主机来运行 .NET Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="9f997-106">Hosting the .NET Core runtime is an advanced scenario and, in most cases, .NET Core developers don't need to worry about hosting because .NET Core build processes provide a default host to run .NET Core applications.</span></span> <span data-ttu-id="9f997-107">虽然在某些特殊情况下，它对显式托管 .NET Core 运行时非常有用 - 无论是作为一种在本机进程中调用托管代码的方式还是为了获得对运行时工作原理更好的控制。</span><span class="sxs-lookup"><span data-stu-id="9f997-107">In some specialized circumstances, though, it can be useful to explicitly host the .NET Core runtime, either as a means of invoking managed code in a native process or in order to gain more control over how the runtime works.</span></span>

<span data-ttu-id="9f997-108">本文概述了从本机代码启动 .NET Core 运行时和在其中执行托管代码的必要步骤。</span><span class="sxs-lookup"><span data-stu-id="9f997-108">This article gives an overview of the steps necessary to start the .NET Core runtime from native code and execute managed code in it.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9f997-109">先决条件</span><span class="sxs-lookup"><span data-stu-id="9f997-109">Prerequisites</span></span>

<span data-ttu-id="9f997-110">由于主机是本机应用程序，所以本教程将介绍如何构造 C++ 应用程序以托管 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="9f997-110">Because hosts are native applications, this tutorial covers constructing a C++ application to host .NET Core.</span></span> <span data-ttu-id="9f997-111">将需要一个 C++ 开发环境（例如，[Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) 提供的环境）。</span><span class="sxs-lookup"><span data-stu-id="9f997-111">You will need a C++ development environment (such as that provided by [Visual Studio](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs)).</span></span>

<span data-ttu-id="9f997-112">还将需要一个简单的 .NET Core 应用程序来测试主机，因此应安装 [.NET Core SDK](https://dotnet.microsoft.com/download) 并[构建一个小型的 .NET Core 测试应用](with-visual-studio.md)（例如，“Hello World”应用）。</span><span class="sxs-lookup"><span data-stu-id="9f997-112">You will also want a simple .NET Core application to test the host with, so you should install the [.NET Core SDK](https://dotnet.microsoft.com/download) and [build a small .NET Core test app](with-visual-studio.md) (such as a 'Hello World' app).</span></span> <span data-ttu-id="9f997-113">使用通过新 .NET Core 控制台项目模板创建的“Hello World”应用就足够了。</span><span class="sxs-lookup"><span data-stu-id="9f997-113">The 'Hello World' app created by the new .NET Core console project template is sufficient.</span></span>

## <a name="hosting-apis"></a><span data-ttu-id="9f997-114">承载 API</span><span class="sxs-lookup"><span data-stu-id="9f997-114">Hosting APIs</span></span>

<span data-ttu-id="9f997-115">可以使用两种不同的 API 来承载 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="9f997-115">There are two different APIs that can be used to host .NET Core.</span></span> <span data-ttu-id="9f997-116">本文（及其相关的[示例](https://github.com/dotnet/samples/tree/main/core/hosting)）涵盖这两个选项。</span><span class="sxs-lookup"><span data-stu-id="9f997-116">This article (and its associated [samples](https://github.com/dotnet/samples/tree/main/core/hosting)) covers these 2 options.</span></span>

* <span data-ttu-id="9f997-117">在 .NET Core 3.0 及更高版本中托管 .NET Core 运行时的首选方法是借助 `nethost` 和 `hostfxr` 库的 API。</span><span class="sxs-lookup"><span data-stu-id="9f997-117">The preferred method of hosting the .NET Core runtime in .NET Core 3.0 and above is with the `nethost` and `hostfxr` libraries' APIs.</span></span> <span data-ttu-id="9f997-118">由这些入口点来处理查找和设置运行时进行初始化所遇到的复杂性；通过它们，还可启动托管应用程序和调用静态托管方法。</span><span class="sxs-lookup"><span data-stu-id="9f997-118">These entry points handle the complexity of finding and setting up the runtime for initialization and allow both launching a managed application and calling into a static managed method.</span></span>
* <span data-ttu-id="9f997-119">托管低于 .NET Core 3.0 的 .NET Core 运行时的首选方法是使用 [`coreclrhost.h`](https://github.com/dotnet/runtime/blob/main/src/coreclr/hosts/inc/coreclrhost.h) API。</span><span class="sxs-lookup"><span data-stu-id="9f997-119">The preferred method of hosting the .NET Core runtime prior to .NET Core 3.0 is with the [`coreclrhost.h`](https://github.com/dotnet/runtime/blob/main/src/coreclr/hosts/inc/coreclrhost.h) API.</span></span> <span data-ttu-id="9f997-120">此 API 公开一些函数，用于轻松地启动和停止运行时并调用托管代码（通过启动托管 exe 或通过调用静态托管方法）。</span><span class="sxs-lookup"><span data-stu-id="9f997-120">This API exposes functions for easily starting and stopping the runtime and invoking managed code (either by launching a managed exe or by calling static managed methods).</span></span>

## <a name="sample-hosts"></a><span data-ttu-id="9f997-121">示例主机</span><span class="sxs-lookup"><span data-stu-id="9f997-121">Sample Hosts</span></span>

<span data-ttu-id="9f997-122">有关展示在下面的教程中所述步骤的[示例主机](https://github.com/dotnet/samples/tree/main/core/hosting)，请访问 dotnet/samples GitHub 存储库。</span><span class="sxs-lookup"><span data-stu-id="9f997-122">[Sample hosts](https://github.com/dotnet/samples/tree/main/core/hosting) demonstrating the steps outlined in the tutorials below are available in the dotnet/samples GitHub repository.</span></span> <span data-ttu-id="9f997-123">该示例中的注释清楚地将这些教程中已编号的步骤与它们在示例中的执行位置关联。</span><span class="sxs-lookup"><span data-stu-id="9f997-123">Comments in the samples clearly associate the numbered steps from these tutorials with where they're performed in the sample.</span></span> <span data-ttu-id="9f997-124">有关下载说明，请参阅[示例和教程](../../samples-and-tutorials/index.md#view-and-download-samples)。</span><span class="sxs-lookup"><span data-stu-id="9f997-124">For download instructions, see [Samples and Tutorials](../../samples-and-tutorials/index.md#view-and-download-samples).</span></span>

<span data-ttu-id="9f997-125">请记住，示例主机的用途在于提供学习指导，在纠错方面不甚严谨，其重在可读性而非效率。</span><span class="sxs-lookup"><span data-stu-id="9f997-125">Keep in mind that the sample hosts are meant to be used for learning purposes, so they are light on error checking and are designed to emphasize readability over efficiency.</span></span>

## <a name="create-a-host-using-nethosth-and-hostfxrh"></a><span data-ttu-id="9f997-126">使用 `nethost.h` 和 `hostfxr.h` 创建主机</span><span class="sxs-lookup"><span data-stu-id="9f997-126">Create a host using `nethost.h` and `hostfxr.h`</span></span>

<span data-ttu-id="9f997-127">以下步骤详细说明如何使用 `nethost` 和 `hostfxr` 库在本机应用程序中启动 .NET Core 运行时并调用托管静态方法。</span><span class="sxs-lookup"><span data-stu-id="9f997-127">The following steps detail how to use the `nethost` and `hostfxr` libraries to start the .NET Core runtime in a native application and call into a managed static method.</span></span> <span data-ttu-id="9f997-128">[示例](https://github.com/dotnet/samples/tree/main/core/hosting/HostWithHostFxr)使用了随 .NET SDK 一起安装的 `nethost` 标头和库，以及 [dotnet/runtime](https://github.com/dotnet/runtime) 存储库中的 [`coreclr_delegates.h`](https://github.com/dotnet/runtime/blob/main/src/native/corehost/coreclr_delegates.h) 和 [`hostfxr.h`](https://github.com/dotnet/runtime/blob/main/src/native/corehost/hostfxr.h) 文件的副本。</span><span class="sxs-lookup"><span data-stu-id="9f997-128">The [sample](https://github.com/dotnet/samples/tree/main/core/hosting/HostWithHostFxr) uses the `nethost` header and library installed with the .NET SDK and copies of the [`coreclr_delegates.h`](https://github.com/dotnet/runtime/blob/main/src/native/corehost/coreclr_delegates.h) and [`hostfxr.h`](https://github.com/dotnet/runtime/blob/main/src/native/corehost/hostfxr.h) files from the [dotnet/runtime](https://github.com/dotnet/runtime) repository.</span></span>

### <a name="step-1---load-hostfxr-and-get-exported-hosting-functions"></a><span data-ttu-id="9f997-129">步骤 1 - 加载 `hostfxr` 并获取导出的托管函数</span><span class="sxs-lookup"><span data-stu-id="9f997-129">Step 1 - Load `hostfxr` and get exported hosting functions</span></span>

<span data-ttu-id="9f997-130">`nethost` 库提供用于查找 `hostfxr` 库的 `get_hostfxr_path` 函数。</span><span class="sxs-lookup"><span data-stu-id="9f997-130">The `nethost` library provides the `get_hostfxr_path` function for locating the `hostfxr` library.</span></span> <span data-ttu-id="9f997-131">`hostfxr` 库公开用于托管 .NET Core 运行时的函数。</span><span class="sxs-lookup"><span data-stu-id="9f997-131">The `hostfxr` library exposes functions for hosting the .NET Core runtime.</span></span> <span data-ttu-id="9f997-132">函数的完整列表可在 [`hostfxr.h`](https://github.com/dotnet/runtime/blob/main/src/native/corehost/hostfxr.h) 和[本机托管设计文档](https://github.com/dotnet/runtime/blob/main/docs/design/features/native-hosting.md)中找到。</span><span class="sxs-lookup"><span data-stu-id="9f997-132">The full list of functions can be found in [`hostfxr.h`](https://github.com/dotnet/runtime/blob/main/src/native/corehost/hostfxr.h) and the [native hosting design document](https://github.com/dotnet/runtime/blob/main/docs/design/features/native-hosting.md).</span></span> <span data-ttu-id="9f997-133">示例和本教程使用以下函数：</span><span class="sxs-lookup"><span data-stu-id="9f997-133">The sample and this tutorial use the following:</span></span>

* <span data-ttu-id="9f997-134">`hostfxr_initialize_for_runtime_config`：初始化主机上下文，并使用指定的运行时配置准备初始化 .NET Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="9f997-134">`hostfxr_initialize_for_runtime_config`: Initializes a host context and prepares for initialization of the .NET Core runtime using the specified runtime configuration.</span></span>
* <span data-ttu-id="9f997-135">`hostfxr_get_runtime_delegate`：获取对运行时功能的委托。</span><span class="sxs-lookup"><span data-stu-id="9f997-135">`hostfxr_get_runtime_delegate`: Gets a delegate for runtime functionality.</span></span>
* <span data-ttu-id="9f997-136">`hostfxr_close`：关闭主机上下文。</span><span class="sxs-lookup"><span data-stu-id="9f997-136">`hostfxr_close`: Closes a host context.</span></span>

<span data-ttu-id="9f997-137">使用 `get_hostfxr_path` 找到了 `hostfxr` 库。</span><span class="sxs-lookup"><span data-stu-id="9f997-137">The `hostfxr` library is found using `get_hostfxr_path`.</span></span> <span data-ttu-id="9f997-138">随后加载此库并检索其导出。</span><span class="sxs-lookup"><span data-stu-id="9f997-138">It is then loaded and its exports are retrieved.</span></span>

[!code-cpp[HostFxrHost#LoadHostFxr](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithHostFxr/src/NativeHost/nativehost.cpp#LoadHostFxr)]

### <a name="step-2---initialize-and-start-the-net-core-runtime"></a><span data-ttu-id="9f997-139">步骤 2 - 初始化和启动 .NET Core 运行时</span><span class="sxs-lookup"><span data-stu-id="9f997-139">Step 2 - Initialize and start the .NET Core runtime</span></span>

<span data-ttu-id="9f997-140">`hostfxr_initialize_for_runtime_config` 和 `hostfxr_get_runtime_delegate` 函数使用将加载的托管组件的运行时配置初始化并启动 .NET Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="9f997-140">The `hostfxr_initialize_for_runtime_config` and `hostfxr_get_runtime_delegate` functions initialize and start the .NET Core runtime using the runtime configuration for the managed component that will be loaded.</span></span> <span data-ttu-id="9f997-141">`hostfxr_get_runtime_delegate` 函数用于获取运行时委托，允许加载托管程序集并获取指向该程序集中的静态方法的函数指针。</span><span class="sxs-lookup"><span data-stu-id="9f997-141">The `hostfxr_get_runtime_delegate` function is used to get a runtime delegate that allows loading a managed assembly and getting a function pointer to a static method in that assembly.</span></span>

[!code-cpp[HostFxrHost#Initialize](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithHostFxr/src/NativeHost/nativehost.cpp#Initialize)]

### <a name="step-3---load-managed-assembly-and-get-function-pointer-to-a-managed-method"></a><span data-ttu-id="9f997-142">步骤 3 - 加载托管程序集并获取指向托管方法的函数指针</span><span class="sxs-lookup"><span data-stu-id="9f997-142">Step 3 - Load managed assembly and get function pointer to a managed method</span></span>

<span data-ttu-id="9f997-143">将调用运行时委托以加载托管程序集并获取指向托管方法的函数指针。</span><span class="sxs-lookup"><span data-stu-id="9f997-143">The runtime delegate is called to load the managed assembly and get a function pointer to a managed method.</span></span> <span data-ttu-id="9f997-144">委托需要程序集路径、类型名称和方法名称作为输入，并返回可用于调用托管方法的函数指针。</span><span class="sxs-lookup"><span data-stu-id="9f997-144">The delegate requires the assembly path, type name, and method name as inputs and returns a function pointer that can be used to invoke the managed method.</span></span>

[!code-cpp[HostFxrHost#LoadAndGet](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithHostFxr/src/NativeHost/nativehost.cpp#LoadAndGet)]

<span data-ttu-id="9f997-145">该示例通过在调用运行时委托时将 `nullptr` 作为委托类型名称传递，对托管方法使用默认签名：</span><span class="sxs-lookup"><span data-stu-id="9f997-145">By passing `nullptr` as the delegate type name when calling the runtime delegate, the sample uses a default signature for the managed method:</span></span>

```csharp
public delegate int ComponentEntryPoint(IntPtr args, int sizeBytes);
```

<span data-ttu-id="9f997-146">可以通过在调用运行时委托时指定委托类型名称来使用其他签名。</span><span class="sxs-lookup"><span data-stu-id="9f997-146">A different signature can be used by specifying the delegate type name when calling the runtime delegate.</span></span>

### <a name="step-4---run-managed-code"></a><span data-ttu-id="9f997-147">步骤 4 - 运行托管代码！</span><span class="sxs-lookup"><span data-stu-id="9f997-147">Step 4 - Run managed code!</span></span>

<span data-ttu-id="9f997-148">本机主机现在可以调用托管方法，并向其传递所需的参数。</span><span class="sxs-lookup"><span data-stu-id="9f997-148">The native host can now call the managed method and pass it the desired parameters.</span></span>

[!code-cpp[HostFxrHost#CallManaged](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithHostFxr/src/NativeHost/nativehost.cpp#CallManaged)]

## <a name="create-a-host-using-coreclrhosth"></a><span data-ttu-id="9f997-149">使用 `coreclrhost.h` 创建主机</span><span class="sxs-lookup"><span data-stu-id="9f997-149">Create a host using `coreclrhost.h`</span></span>

<span data-ttu-id="9f997-150">以下步骤详细说明如何使用 `coreclrhost.h` API 在本机应用程序中启动 .NET Core 运行时并调用托管静态方法。</span><span class="sxs-lookup"><span data-stu-id="9f997-150">The following steps detail how to use the `coreclrhost.h` API to start the .NET Core runtime in a native application and call into a managed static method.</span></span> <span data-ttu-id="9f997-151">本文档中的代码片段使用一些特定于 Windows 的 API，但是[完整示例主机](https://github.com/dotnet/samples/tree/main/core/hosting/HostWithCoreClrHost)同时显示 Windows 和 Linux 的代码路径。</span><span class="sxs-lookup"><span data-stu-id="9f997-151">The code snippets in this document use some Windows-specific APIs, but the [full sample host](https://github.com/dotnet/samples/tree/main/core/hosting/HostWithCoreClrHost) shows both Windows and Linux code paths.</span></span>

<span data-ttu-id="9f997-152">[ corerun 主机](https://github.com/dotnet/runtime/tree/main/src/coreclr/hosts/corerun)显示使用 `coreclrhost.h` 的更为复杂的真实跨平台托管示例。</span><span class="sxs-lookup"><span data-stu-id="9f997-152">The [corerun host](https://github.com/dotnet/runtime/tree/main/src/coreclr/hosts/corerun) shows a more complex, real-world, cross-platform example of hosting using `coreclrhost.h`.</span></span>

### <a name="step-1---find-and-load-coreclr"></a><span data-ttu-id="9f997-153">步骤 1 - 查找和加载 CoreCLR</span><span class="sxs-lookup"><span data-stu-id="9f997-153">Step 1 - Find and load CoreCLR</span></span>

<span data-ttu-id="9f997-154">.NET Core 运行时 API 位于 coreclr.dll（对于 Windows）、libcoreclr.so（对于 Linux）或 libcoreclr.dylib（对于 macOS）  。</span><span class="sxs-lookup"><span data-stu-id="9f997-154">The .NET Core runtime APIs are in *coreclr.dll* (on Windows), in *libcoreclr.so* (on Linux), or in *libcoreclr.dylib* (on macOS).</span></span> <span data-ttu-id="9f997-155">承载 .NET Core 的第一步是加载 CoreCLR 库。</span><span class="sxs-lookup"><span data-stu-id="9f997-155">The first step to hosting .NET Core is to load the CoreCLR library.</span></span> <span data-ttu-id="9f997-156">一些主机探测不同的路径或使用输入参数来查找库，而另一些主机能够从某个路径（例如，紧邻主机的路径，或从计算机范围内的位置）加载库。</span><span class="sxs-lookup"><span data-stu-id="9f997-156">Some hosts probe different paths or use input parameters to find the library while others know to load it from a certain path (next to the host, for example, or from a machine-wide location).</span></span>

<span data-ttu-id="9f997-157">找到库之后，系统会使用 `LoadLibraryEx`（对于 Windows）或 `dlopen`（对于 Linux/macOS）加载库。</span><span class="sxs-lookup"><span data-stu-id="9f997-157">Once found, the library is loaded with `LoadLibraryEx` (on Windows) or `dlopen` (on Linux/macOS).</span></span>

[!code-cpp[CoreClrHost#1](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#1)]

### <a name="step-2---get-net-core-hosting-functions"></a><span data-ttu-id="9f997-158">步骤 2 - 获取 .NET Core 承载函数</span><span class="sxs-lookup"><span data-stu-id="9f997-158">Step 2 - Get .NET Core hosting functions</span></span>

<span data-ttu-id="9f997-159">CoreClrHost 有几个可用于承载 .NET Core 的重要方法：</span><span class="sxs-lookup"><span data-stu-id="9f997-159">CoreClrHost has several important methods useful for hosting .NET Core:</span></span>

* <span data-ttu-id="9f997-160">`coreclr_initialize`：启动 .NET Core 运行时并设置默认（且仅设置）AppDomain。</span><span class="sxs-lookup"><span data-stu-id="9f997-160">`coreclr_initialize`: Starts the .NET Core runtime and sets up the default (and only) AppDomain.</span></span>
* <span data-ttu-id="9f997-161">`coreclr_execute_assembly`：执行托管程序集。</span><span class="sxs-lookup"><span data-stu-id="9f997-161">`coreclr_execute_assembly`: Executes a managed assembly.</span></span>
* <span data-ttu-id="9f997-162">`coreclr_create_delegate`：创建指向托管方法的函数指针。</span><span class="sxs-lookup"><span data-stu-id="9f997-162">`coreclr_create_delegate`: Creates a function pointer to a managed method.</span></span>
* <span data-ttu-id="9f997-163">`coreclr_shutdown`：关闭 .NET Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="9f997-163">`coreclr_shutdown`: Shuts down the .NET Core runtime.</span></span>
* <span data-ttu-id="9f997-164">`coreclr_shutdown_2`：如 `coreclr_shutdown`，但还会检索托管代码的退出代码。</span><span class="sxs-lookup"><span data-stu-id="9f997-164">`coreclr_shutdown_2`: Like `coreclr_shutdown`, but also retrieves the managed code's exit code.</span></span>

<span data-ttu-id="9f997-165">加载 CoreCLR 库之后，下一步是使用 `GetProcAddress`（对于 Windows）或 `dlsym`（对于 Linux/macOS）引用这些函数。</span><span class="sxs-lookup"><span data-stu-id="9f997-165">After loading the CoreCLR library, the next step is to get references to these functions using `GetProcAddress` (on Windows) or `dlsym` (on Linux/macOS).</span></span>

[!code-cpp[CoreClrHost#2](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#2)]

### <a name="step-3---prepare-runtime-properties"></a><span data-ttu-id="9f997-166">步骤 3 - 准备运行时属性</span><span class="sxs-lookup"><span data-stu-id="9f997-166">Step 3 - Prepare runtime properties</span></span>

<span data-ttu-id="9f997-167">在启动运行时之前，有必要准备一些属性来指定行为（特别是关于程序集加载器的行为）。</span><span class="sxs-lookup"><span data-stu-id="9f997-167">Before starting the runtime, it is necessary to prepare some properties to specify behavior (especially concerning the assembly loader).</span></span>

<span data-ttu-id="9f997-168">常用属性包括：</span><span class="sxs-lookup"><span data-stu-id="9f997-168">Common properties include:</span></span>

* <span data-ttu-id="9f997-169">`TRUSTED_PLATFORM_ASSEMBLIES` 这是程序集路径列表（对于 Windows，使用“;”分隔，对于 Linux，使用“:”分隔），运行时在默认情况下能够解析这些路径。</span><span class="sxs-lookup"><span data-stu-id="9f997-169">`TRUSTED_PLATFORM_ASSEMBLIES` This is a list of assembly paths (delimited by ';' on Windows and ':' on Linux) which the runtime will be able to resolve by default.</span></span> <span data-ttu-id="9f997-170">一些主机有硬编码清单，其中列出了它们可以加载的程序集。</span><span class="sxs-lookup"><span data-stu-id="9f997-170">Some hosts have hard-coded manifests listing assemblies they can load.</span></span> <span data-ttu-id="9f997-171">其他主机将把任何库放在这个列表上的特定位置（例如 coreclr.dll 旁边）。</span><span class="sxs-lookup"><span data-stu-id="9f997-171">Others will put any library in certain locations (next to *coreclr.dll*, for example) on this list.</span></span>
* <span data-ttu-id="9f997-172">`APP_PATHS` 这是一个用来探测程序集的路径的列表（如果在受信任的平台程序集 (TPA) 列表中找不到程序集）。</span><span class="sxs-lookup"><span data-stu-id="9f997-172">`APP_PATHS` This is a list of paths to probe in for an assembly if it can't be found in the trusted platform assemblies (TPA) list.</span></span> <span data-ttu-id="9f997-173">因为主机使用 TPA 列表可以更好地控制加载哪些程序集，所以对于主机来说，确定要加载的程序集并显式列出它们是最佳做法。</span><span class="sxs-lookup"><span data-stu-id="9f997-173">Because the host has more control over which assemblies are loaded using the TPA list, it is a best practice for hosts to determine which assemblies they expect to load and list them explicitly.</span></span> <span data-ttu-id="9f997-174">但是，如果需要探测运行时，则此属性可以支持该方案。</span><span class="sxs-lookup"><span data-stu-id="9f997-174">If probing at run time is needed, however, this property can enable that scenario.</span></span>
* <span data-ttu-id="9f997-175">`APP_NI_PATHS` 此列表与 APP_PATHS 相似，不同之处在于其中的路径用于探测本机映像。</span><span class="sxs-lookup"><span data-stu-id="9f997-175">`APP_NI_PATHS` This list is similar to APP_PATHS except that it's meant to be paths that will be probed for native images.</span></span>
* <span data-ttu-id="9f997-176">`NATIVE_DLL_SEARCH_DIRECTORIES` 此属性是一个路径列表，加载程序在查找通过 p/invoke 调用的本机库时应使用这些路径进行探测。</span><span class="sxs-lookup"><span data-stu-id="9f997-176">`NATIVE_DLL_SEARCH_DIRECTORIES` This property is a list of paths the loader should probe when looking for native libraries called via p/invoke.</span></span>
* <span data-ttu-id="9f997-177">`PLATFORM_RESOURCE_ROOTS` 此列表包含的路径用于探测资源附属程序集（在区域性特定的子目录中）。</span><span class="sxs-lookup"><span data-stu-id="9f997-177">`PLATFORM_RESOURCE_ROOTS` This list includes paths to probe in for resource satellite assemblies (in culture-specific subdirectories).</span></span>

<span data-ttu-id="9f997-178">在此示例主机中，TPA 列表是通过简单列出当前目录中的所有库来进行构造的：</span><span class="sxs-lookup"><span data-stu-id="9f997-178">In this sample host, the TPA list is constructed by simply listing all libraries in the current directory:</span></span>

[!code-cpp[CoreClrHost#7](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#7)]

<span data-ttu-id="9f997-179">因为该示例简单，所以只需要 `TRUSTED_PLATFORM_ASSEMBLIES` 属性：</span><span class="sxs-lookup"><span data-stu-id="9f997-179">Because the sample is simple, it only needs the `TRUSTED_PLATFORM_ASSEMBLIES` property:</span></span>

[!code-cpp[CoreClrHost#3](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#3)]

### <a name="step-4---start-the-runtime"></a><span data-ttu-id="9f997-180">步骤 4 - 启动运行时</span><span class="sxs-lookup"><span data-stu-id="9f997-180">Step 4 - Start the runtime</span></span>

<span data-ttu-id="9f997-181">`coreclrhost.h` API 启动运行时和创建默认的 AppDomain 都只需要进行一次调用。</span><span class="sxs-lookup"><span data-stu-id="9f997-181">`coreclrhost.h` APIs start the runtime and create the default AppDomain all with a single call.</span></span> <span data-ttu-id="9f997-182">`coreclr_initialize` 函数采用基本路径、名称和前面描述的属性，并通过 `hostHandle` 参数将图柄返回到主机。</span><span class="sxs-lookup"><span data-stu-id="9f997-182">The `coreclr_initialize` function takes a base path, name, and the properties described earlier and returns back a handle to the host via the `hostHandle` parameter.</span></span>

[!code-cpp[CoreClrHost#4](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#4)]

### <a name="step-5---run-managed-code"></a><span data-ttu-id="9f997-183">步骤 5 - 运行托管代码！</span><span class="sxs-lookup"><span data-stu-id="9f997-183">Step 5 - Run managed code!</span></span>

<span data-ttu-id="9f997-184">启动运行时之后，主机可以调用托管代码。</span><span class="sxs-lookup"><span data-stu-id="9f997-184">With the runtime started, the host can call managed code.</span></span> <span data-ttu-id="9f997-185">这可以通过两种不同的方法实现。</span><span class="sxs-lookup"><span data-stu-id="9f997-185">This can be done in a couple of different ways.</span></span> <span data-ttu-id="9f997-186">与本教程相关的示例代码使用 `coreclr_create_delegate` 函数创建静态托管方法的委托。</span><span class="sxs-lookup"><span data-stu-id="9f997-186">The sample code linked to this tutorial uses the `coreclr_create_delegate` function to create a delegate to a static managed method.</span></span> <span data-ttu-id="9f997-187">此 API 采用[程序集名称](../../standard/assembly/names.md)、符合命名空间条件的类型名称和方法名称作为输入，并返回可用于调用该方法的委托。</span><span class="sxs-lookup"><span data-stu-id="9f997-187">This API takes the [assembly name](../../standard/assembly/names.md), namespace-qualified type name, and method name as inputs and returns a delegate that can be used to invoke the method.</span></span>

[!code-cpp[CoreClrHost#5](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#5)]

<span data-ttu-id="9f997-188">在此示例中，主机现在可以调用 `managedDelegate` 来运行 `ManagedWorker.DoWork` 方法。</span><span class="sxs-lookup"><span data-stu-id="9f997-188">In this sample, the host can now call `managedDelegate` to run the `ManagedWorker.DoWork` method.</span></span>

<span data-ttu-id="9f997-189">或者，可以使用 `coreclr_execute_assembly` 函数启动托管可执行文件。</span><span class="sxs-lookup"><span data-stu-id="9f997-189">Alternatively, the `coreclr_execute_assembly` function can be used to launch a managed executable.</span></span> <span data-ttu-id="9f997-190">此 API 采用程序集路径和实参数组作为输入形参。</span><span class="sxs-lookup"><span data-stu-id="9f997-190">This API takes an assembly path and array of arguments as input parameters.</span></span> <span data-ttu-id="9f997-191">它在该路径加载程序集并调用其主方法。</span><span class="sxs-lookup"><span data-stu-id="9f997-191">It loads the assembly at that path and invokes its main method.</span></span>

```c++
int hr = executeAssembly(
        hostHandle,
        domainId,
        argumentCount,
        arguments,
        "HelloWorld.exe",
        (unsigned int*)&exitCode);
```

### <a name="step-6---shutdown-and-clean-up"></a><span data-ttu-id="9f997-192">步骤 6 - 关闭和清理</span><span class="sxs-lookup"><span data-stu-id="9f997-192">Step 6 - Shutdown and clean up</span></span>

<span data-ttu-id="9f997-193">最后，主机完成运行托管代码后，使用 `coreclr_shutdown` 或 `coreclr_shutdown_2` 关闭 .NET Core 运行时。</span><span class="sxs-lookup"><span data-stu-id="9f997-193">Finally, when the host is done running managed code, the .NET Core runtime is shut down with `coreclr_shutdown` or `coreclr_shutdown_2`.</span></span>

[!code-cpp[CoreClrHost#6](~/samples/snippets/core/tutorials/netcore-hosting/csharp/HostWithCoreClrHost/src/SampleHost.cpp#6)]

<span data-ttu-id="9f997-194">CoreCLR 不支持重新初始化或卸载。</span><span class="sxs-lookup"><span data-stu-id="9f997-194">CoreCLR does not support reinitialization or unloading.</span></span> <span data-ttu-id="9f997-195">请勿重新调用 `coreclr_initialize` 或卸载 CoreCLR 库。</span><span class="sxs-lookup"><span data-stu-id="9f997-195">Do not call `coreclr_initialize` again or unload the CoreCLR library.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9f997-196">结束语</span><span class="sxs-lookup"><span data-stu-id="9f997-196">Conclusion</span></span>

<span data-ttu-id="9f997-197">构建主机后，可以通过从命令行运行主机并传递其所需的任何参数来对其进行测试。</span><span class="sxs-lookup"><span data-stu-id="9f997-197">Once your host is built, it can be tested by running it from the command line and passing any arguments the host expects.</span></span> <span data-ttu-id="9f997-198">指定主机要运行的 .NET Core 应用时，请务必使用 `dotnet build` 生成的 .dll。</span><span class="sxs-lookup"><span data-stu-id="9f997-198">When specifying the .NET Core app for the host to run, be sure to use the .dll that is produced by `dotnet build`.</span></span> <span data-ttu-id="9f997-199">`dotnet publish` 为独立应用程序生成的可执行文件（.exe 文件）实际上是默认的 .NET Core 主机（以便可直接从主流方案中的命令行启动应用）；用户代码被编译为具有相同名称的 dll。</span><span class="sxs-lookup"><span data-stu-id="9f997-199">Executables (.exe files) produced by `dotnet publish` for self-contained applications are actually the default .NET Core host (so that the app can be launched directly from the command line in mainline scenarios); user code is compiled into a dll of the same name.</span></span>

<span data-ttu-id="9f997-200">如果开始时操作不起作用，请再次检查 coreclr.dll 是否在主机预期的位置可用、TPA 列表中是否包含了所有必需的框架库以及 CoreCLR 的位数（32 位或 64 位）是否匹配主机的构建方式。</span><span class="sxs-lookup"><span data-stu-id="9f997-200">If things don't work initially, double-check that *coreclr.dll* is available in the location expected by the host, that all necessary Framework libraries are in the TPA list, and that CoreCLR's bitness (32-bit or 64-bit) matches how the host was built.</span></span>

<span data-ttu-id="9f997-201">托管 .NET Core 运行时是高级方案，许多开发人员并不需要实施这一方案，但对于那些需要从本机进程启动托管代码的人员，或需要更好地控制 .NET Core 运行时的行为的人员而言，它会非常有用。</span><span class="sxs-lookup"><span data-stu-id="9f997-201">Hosting the .NET Core runtime is an advanced scenario that many developers won't require, but for those who need to launch managed code from a native process, or who need more control over the .NET Core runtime's behavior, it can be very useful.</span></span>
