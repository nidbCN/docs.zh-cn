---
title: 向 COM 公开 .NET Core 组件
description: 本教程演示如何从 .NET Core 向 COM 公开类。 为无注册表 COM 生成 COM 服务器和并行服务器清单。
ms.date: 07/12/2019
helpviewer_keywords:
- exposing .NET Core components to COM
- interoperation with unmanaged code, exposing .NET Core components
- COM interop, exposing COM components
ms.assetid: 21271167-fe7f-46ba-a81f-a6812ea649d4
author: jkoritzinsky
ms.author: jekoritz
ms.openlocfilehash: 30323113520f3bd148a0f1a6fab9801381a332b3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875143"
---
# <a name="exposing-net-core-components-to-com"></a><span data-ttu-id="05920-104">向 COM 公开 .NET Core 组件</span><span class="sxs-lookup"><span data-stu-id="05920-104">Exposing .NET Core components to COM</span></span>

<span data-ttu-id="05920-105">在 .NET Core 中，与 .NET Framework 相比，向 COM 公开 .NET 对象的过程已明显简化。</span><span class="sxs-lookup"><span data-stu-id="05920-105">In .NET Core, the process for exposing your .NET objects to COM has been significantly streamlined in comparison to .NET Framework.</span></span> <span data-ttu-id="05920-106">下面的过程将引导你如何向 COM 公开类。</span><span class="sxs-lookup"><span data-stu-id="05920-106">The following process will walk you through how to expose a class to COM.</span></span> <span data-ttu-id="05920-107">本教程介绍了如何：</span><span class="sxs-lookup"><span data-stu-id="05920-107">This tutorial shows you how to:</span></span>

- <span data-ttu-id="05920-108">从 .NET Core 向 COM 公开类。</span><span class="sxs-lookup"><span data-stu-id="05920-108">Expose a class to COM from .NET Core.</span></span>
- <span data-ttu-id="05920-109">生成 COM 服务器作为构建 .NET Core 库的一部分。</span><span class="sxs-lookup"><span data-stu-id="05920-109">Generate a COM server as part of building your .NET Core library.</span></span>
- <span data-ttu-id="05920-110">自动为无注册表 COM 生成并行服务器清单。</span><span class="sxs-lookup"><span data-stu-id="05920-110">Automatically generate a side-by-side server manifest for Registry-Free COM.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05920-111">先决条件</span><span class="sxs-lookup"><span data-stu-id="05920-111">Prerequisites</span></span>

- <span data-ttu-id="05920-112">安装 [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="05920-112">Install [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) or a newer version.</span></span>

## <a name="create-the-library"></a><span data-ttu-id="05920-113">创建库</span><span class="sxs-lookup"><span data-stu-id="05920-113">Create the library</span></span>

<span data-ttu-id="05920-114">第一步是创建库。</span><span class="sxs-lookup"><span data-stu-id="05920-114">The first step is to create the library.</span></span>

1. <span data-ttu-id="05920-115">创建新文件夹，并在该文件夹中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="05920-115">Create a new folder, and in that folder run the following command:</span></span>

    ```dotnetcli
    dotnet new classlib
    ```

2. <span data-ttu-id="05920-116">打开 `Class1.cs`。</span><span class="sxs-lookup"><span data-stu-id="05920-116">Open `Class1.cs`.</span></span>
3. <span data-ttu-id="05920-117">将 `using System.Runtime.InteropServices;` 添加到文件顶部。</span><span class="sxs-lookup"><span data-stu-id="05920-117">Add `using System.Runtime.InteropServices;` to the top of the file.</span></span>
4. <span data-ttu-id="05920-118">创建名为 `IServer` 的接口。</span><span class="sxs-lookup"><span data-stu-id="05920-118">Create an interface named `IServer`.</span></span> <span data-ttu-id="05920-119">例如：</span><span class="sxs-lookup"><span data-stu-id="05920-119">For example:</span></span>

   ```csharp
   using System;
   using System.Runtime.InteropServices;

   [ComVisible(true)]
   [Guid(ContractGuids.ServerInterface)]
   [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
   public interface IServer
   {
       /// <summary>
       /// Compute the value of the constant Pi.
       /// </summary>
       double ComputePi();
   }
   ```

5. <span data-ttu-id="05920-120">将 `[Guid("<IID>")]` 属性添加到接口，包含要实现的 COM 接口的接口 GUID。</span><span class="sxs-lookup"><span data-stu-id="05920-120">Add the `[Guid("<IID>")]` attribute to the interface, with the interface GUID for the COM interface you're implementing.</span></span> <span data-ttu-id="05920-121">例如 `[Guid("fe103d6e-e71b-414c-80bf-982f18f6c1c7")]`。</span><span class="sxs-lookup"><span data-stu-id="05920-121">For example, `[Guid("fe103d6e-e71b-414c-80bf-982f18f6c1c7")]`.</span></span> <span data-ttu-id="05920-122">请注意，此 GUID 必须唯一，因为它是 COM 的此接口的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="05920-122">Note that this GUID needs to be unique since it is the only identifier of this interface for COM.</span></span> <span data-ttu-id="05920-123">在 Visual Studio 中，可通过转到“工具”>“创建 GUID”以打开“创建 GUID”工具来生成 GUID。</span><span class="sxs-lookup"><span data-stu-id="05920-123">In Visual Studio, you can generate a GUID by going to Tools > Create GUID to open the Create GUID tool.</span></span>
6. <span data-ttu-id="05920-124">将 `[InterfaceType]` 属性添加到接口，并指定接口应实现的基本 COM 接口。</span><span class="sxs-lookup"><span data-stu-id="05920-124">Add the `[InterfaceType]` attribute to the interface and specify what base COM interfaces your interface should implement.</span></span>
7. <span data-ttu-id="05920-125">创建用于实现 `IServer` 的名为 `Server` 的类。</span><span class="sxs-lookup"><span data-stu-id="05920-125">Create a class named `Server` that implements `IServer`.</span></span>
8. <span data-ttu-id="05920-126">将 `[Guid("<CLSID>")]` 属性添加到类，包含要实现的 COM 类的类标识符 GUID。</span><span class="sxs-lookup"><span data-stu-id="05920-126">Add the `[Guid("<CLSID>")]` attribute to the class, with the class identifier GUID for the COM class you're implementing.</span></span> <span data-ttu-id="05920-127">例如 `[Guid("9f35b6f5-2c05-4e7f-93aa-ee087f6e7ab6")]`。</span><span class="sxs-lookup"><span data-stu-id="05920-127">For example, `[Guid("9f35b6f5-2c05-4e7f-93aa-ee087f6e7ab6")]`.</span></span> <span data-ttu-id="05920-128">与接口 GUID 一样，此 GUID 必须唯一，因为它是 COM 的此接口的唯一标识符。</span><span class="sxs-lookup"><span data-stu-id="05920-128">As with the interface GUID, this GUID must be unique since it is the only identifier of this interface to COM.</span></span>
9. <span data-ttu-id="05920-129">将 `[ComVisible(true)]` 属性添加到接口和类。</span><span class="sxs-lookup"><span data-stu-id="05920-129">Add the `[ComVisible(true)]` attribute to both the interface and the class.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05920-130">与 .NET Framework 不同，.NET Core 要求指定想要通过 COM 激活的任何类的 CLSID。</span><span class="sxs-lookup"><span data-stu-id="05920-130">Unlike in .NET Framework, .NET Core requires you to specify the CLSID of any class you want to be activatable via COM.</span></span>

## <a name="generate-the-com-host"></a><span data-ttu-id="05920-131">生成 COM 主机</span><span class="sxs-lookup"><span data-stu-id="05920-131">Generate the COM host</span></span>

1. <span data-ttu-id="05920-132">打开 `.csproj` 项目文件并在 `<PropertyGroup></PropertyGroup>` 标记中添加 `<EnableComHosting>true</EnableComHosting>`。</span><span class="sxs-lookup"><span data-stu-id="05920-132">Open the `.csproj` project file and add `<EnableComHosting>true</EnableComHosting>` inside a `<PropertyGroup></PropertyGroup>` tag.</span></span>
2. <span data-ttu-id="05920-133">生成项目。</span><span class="sxs-lookup"><span data-stu-id="05920-133">Build the project.</span></span>

<span data-ttu-id="05920-134">生成的输出将具有 `ProjectName.dll`、`ProjectName.deps.json`、`ProjectName.runtimeconfig.json` 和 `ProjectName.comhost.dll` 文件。</span><span class="sxs-lookup"><span data-stu-id="05920-134">The resulting output will have a `ProjectName.dll`, `ProjectName.deps.json`, `ProjectName.runtimeconfig.json` and `ProjectName.comhost.dll` file.</span></span>

## <a name="register-the-com-host-for-com"></a><span data-ttu-id="05920-135">为 COM 注册 COM 主机</span><span class="sxs-lookup"><span data-stu-id="05920-135">Register the COM host for COM</span></span>

<span data-ttu-id="05920-136">打开提升的命令提示符，然后运行 `regsvr32 ProjectName.comhost.dll`。</span><span class="sxs-lookup"><span data-stu-id="05920-136">Open an elevated command prompt and run `regsvr32 ProjectName.comhost.dll`.</span></span> <span data-ttu-id="05920-137">这将使用 COM 注册所有公开的 .NET 对象。</span><span class="sxs-lookup"><span data-stu-id="05920-137">That will register all of your exposed .NET objects with COM.</span></span>

## <a name="enabling-regfree-com"></a><span data-ttu-id="05920-138">启用 RegFree COM</span><span class="sxs-lookup"><span data-stu-id="05920-138">Enabling RegFree COM</span></span>

1. <span data-ttu-id="05920-139">打开 `.csproj` 项目文件并在 `<PropertyGroup></PropertyGroup>` 标记中添加 `<EnableRegFreeCom>true</EnableRegFreeCom>`。</span><span class="sxs-lookup"><span data-stu-id="05920-139">Open the `.csproj` project file and add `<EnableRegFreeCom>true</EnableRegFreeCom>` inside a `<PropertyGroup></PropertyGroup>` tag.</span></span>
2. <span data-ttu-id="05920-140">生成项目。</span><span class="sxs-lookup"><span data-stu-id="05920-140">Build the project.</span></span>

<span data-ttu-id="05920-141">生成的输出现在还将具有 `ProjectName.X.manifest` 文件。</span><span class="sxs-lookup"><span data-stu-id="05920-141">The resulting output will now also have a `ProjectName.X.manifest` file.</span></span> <span data-ttu-id="05920-142">此文件是用于无注册表的 COM 的并行清单。</span><span class="sxs-lookup"><span data-stu-id="05920-142">This file is the side-by-side manifest for use with Registry-Free COM.</span></span>

## <a name="sample"></a><span data-ttu-id="05920-143">示例</span><span class="sxs-lookup"><span data-stu-id="05920-143">Sample</span></span>

<span data-ttu-id="05920-144">GitHub 上的 dotnet/samples 存储库中有一个正常运行的 [COM 服务器示例](https://github.com/dotnet/samples/tree/main/core/extensions/COMServerDemo)。</span><span class="sxs-lookup"><span data-stu-id="05920-144">There is a fully functional [COM server sample](https://github.com/dotnet/samples/tree/main/core/extensions/COMServerDemo) in the dotnet/samples repository on GitHub.</span></span>

## <a name="additional-notes"></a><span data-ttu-id="05920-145">附加说明</span><span class="sxs-lookup"><span data-stu-id="05920-145">Additional notes</span></span>

<span data-ttu-id="05920-146">与 .NET Framework 不同，.NET Core 不支持从 .NET Core 程序集生成 COM 类型库 (TLB)。</span><span class="sxs-lookup"><span data-stu-id="05920-146">Unlike in .NET Framework, there is no support in .NET Core for generating a COM Type Library (TLB) from a .NET Core assembly.</span></span> <span data-ttu-id="05920-147">本指南旨在说明如何为 COM 接口的本机声明手动编写 IDL 文件或 C/C++ 标头。</span><span class="sxs-lookup"><span data-stu-id="05920-147">The guidance is to either manually write an IDL file or a C/C++ header for the native declarations of the COM interfaces.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="05920-148">在 .NET Framework 中，32 位和 64 位客户端可以使用“任何 CPU”程序集。</span><span class="sxs-lookup"><span data-stu-id="05920-148">In .NET Framework, an "Any CPU" assembly can be consumed by both 32-bit and 64-bit clients.</span></span> <span data-ttu-id="05920-149">默认情况下，在 .NET Core、.NET 5 和更高版本中，“任何 CPU”程序集附带了 64 位的 \*.comhost.dll。</span><span class="sxs-lookup"><span data-stu-id="05920-149">By default, in .NET Core, .NET 5, and later versions, "Any CPU" assemblies are accompanied by a 64-bit *\*.comhost.dll*.</span></span> <span data-ttu-id="05920-150">因此，它们只能由 64 位客户端使用。</span><span class="sxs-lookup"><span data-stu-id="05920-150">Because of this, they can only be consumed by 64-bit clients.</span></span> <span data-ttu-id="05920-151">这是默认的，因为这是 SDK 表示的内容。</span><span class="sxs-lookup"><span data-stu-id="05920-151">That is the default because that is what the SDK represents.</span></span> <span data-ttu-id="05920-152">此行为与发布“自包含”功能的方式相同：默认情况下，它使用 SDK 提供的内容。</span><span class="sxs-lookup"><span data-stu-id="05920-152">This behavior is identical to how the "self-contained" feature is published: by default it uses what the SDK provides.</span></span> <span data-ttu-id="05920-153">`NETCoreSdkRuntimeIdentifier` MSBuild 属性确定 \*.comhost.dll 的位数。</span><span class="sxs-lookup"><span data-stu-id="05920-153">The `NETCoreSdkRuntimeIdentifier` MSBuild property determines the bitness of *\*.comhost.dll*.</span></span> <span data-ttu-id="05920-154">正如预期的那样，托管部分的位数实际上是不可知的，而随附的本机资产默认为目标 SDK。</span><span class="sxs-lookup"><span data-stu-id="05920-154">The managed part is actually bitness agnostic as expected, but the accompanying native asset defaults to the targeted SDK.</span></span>

<span data-ttu-id="05920-155">不支持 COM 组件的[自包含部署](../deploying/index.md#publish-self-contained)。</span><span class="sxs-lookup"><span data-stu-id="05920-155">[Self-contained deployments](../deploying/index.md#publish-self-contained) of COM components are not supported.</span></span> <span data-ttu-id="05920-156">仅支持 COM 组件的[依赖框架的部署](../deploying/index.md#publish-framework-dependent)。</span><span class="sxs-lookup"><span data-stu-id="05920-156">Only [framework-dependent deployments](../deploying/index.md#publish-framework-dependent) of COM components are supported.</span></span>

<span data-ttu-id="05920-157">此外，将 .NET Framework 和 .NET Core 同时加载到同一进程具有诊断限制。</span><span class="sxs-lookup"><span data-stu-id="05920-157">Additionally, loading both .NET Framework and .NET Core into the same process does have diagnostic limitations.</span></span> <span data-ttu-id="05920-158">主要限制是调试托管组件，因为不能同时调试 .NET Framework 和 .NET Core。</span><span class="sxs-lookup"><span data-stu-id="05920-158">The primary limitation is the debugging of managed components as it is not possible to debug both .NET Framework and .NET Core at the same time.</span></span> <span data-ttu-id="05920-159">此外，这两个运行时实例不共享托管程序集。</span><span class="sxs-lookup"><span data-stu-id="05920-159">In addition, the two runtime instances don't share managed assemblies.</span></span> <span data-ttu-id="05920-160">这意味着无法在两个运行时之间共享实际的 .NET 类型，所有交互必须仅限于公开的 COM 接口协定。</span><span class="sxs-lookup"><span data-stu-id="05920-160">This means that it isn't possible to share actual .NET types across the two runtimes and instead all interactions must be restricted to the exposed COM interface contracts.</span></span>
