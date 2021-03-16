---
title: .NET Core 和 .NET 5+ 上不支持的 API
titleSuffix: ''
description: 了解哪些 .NET API 始终在 .NET Core 和 .NET 5 及更高版本上引发异常。
ms.date: 10/13/2020
ms.openlocfilehash: b0dc425bf5f7d8f2a44f51ffe75ffcb0c8a5a7ae
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256252"
---
# <a name="apis-that-always-throw-exceptions-on-net-core-and-net-5"></a><span data-ttu-id="1313a-103">始终在 .NET Core 和 .NET 5+ 上引发异常的 API</span><span class="sxs-lookup"><span data-stu-id="1313a-103">APIs that always throw exceptions on .NET Core and .NET 5+</span></span>

<span data-ttu-id="1313a-104">以下 API 将始终在所有或部分平台上的 .NET 5 及更高版本（包括所有 .NET Core 版本）中引发 <xref:System.PlatformNotSupportedException>。</span><span class="sxs-lookup"><span data-stu-id="1313a-104">The following APIs will always throw a <xref:System.PlatformNotSupportedException> on .NET 5 and later versions (including all versions of .NET Core) on all or a subset of platforms.</span></span>

<span data-ttu-id="1313a-105">本文按命名空间组织受影响的 API。</span><span class="sxs-lookup"><span data-stu-id="1313a-105">This article organizes the affected APIs by namespace.</span></span>

> [!NOTE]
>
> - <span data-ttu-id="1313a-106">本文是当前正在进行的工作。</span><span class="sxs-lookup"><span data-stu-id="1313a-106">This article is a work-in-progress.</span></span> <span data-ttu-id="1313a-107">它不是在 .NET 5+ 上引发异常的 API 的完整列表。</span><span class="sxs-lookup"><span data-stu-id="1313a-107">It is not a complete list of APIs that throw exceptions on .NET 5+.</span></span>
> - <span data-ttu-id="1313a-108">本文不包括在 .NET 5+ 上引发的二进制序列化的显式接口实现。</span><span class="sxs-lookup"><span data-stu-id="1313a-108">This article does not include the explicit interface implementations for binary serialization that throw on .NET 5+.</span></span> <span data-ttu-id="1313a-109">有关详细信息，请参阅 [.NET Core 中的二进制序列化](../../standard/serialization/binary-serialization.md#net-core)。</span><span class="sxs-lookup"><span data-stu-id="1313a-109">For more information, see [Binary serialization in .NET Core](../../standard/serialization/binary-serialization.md#net-core).</span></span>

## <a name="system"></a><span data-ttu-id="1313a-110">系统</span><span class="sxs-lookup"><span data-stu-id="1313a-110">System</span></span>

| <span data-ttu-id="1313a-111">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-111">Member</span></span> | <span data-ttu-id="1313a-112">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-112">Platforms that throw</span></span> |
| - | - |
| <xref:System.AppDomain.CreateDomain%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-113">All</span><span class="sxs-lookup"><span data-stu-id="1313a-113">All</span></span> |
| <xref:System.AppDomain.ExecuteAssembly(System.String,System.String[],System.Byte[],System.Configuration.Assemblies.AssemblyHashAlgorithm)?displayProperty=nameWithType> | <span data-ttu-id="1313a-114">All</span><span class="sxs-lookup"><span data-stu-id="1313a-114">All</span></span> |
| <xref:System.Console.CapsLock?displayProperty=nameWithType> | <span data-ttu-id="1313a-115">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-115">Linux and macOS</span></span> |
| <xref:System.Console.NumberLock?displayProperty=nameWithType> | <span data-ttu-id="1313a-116">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-116">Linux and macOS</span></span> |
| <xref:System.Delegate.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-117">All</span><span class="sxs-lookup"><span data-stu-id="1313a-117">All</span></span> |
| <xref:System.Exception.SerializeObjectState?displayProperty=nameWithType> | <span data-ttu-id="1313a-118">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-118">All</span></span> |
| <xref:System.MarshalByRefObject.GetLifetimeService?displayProperty=nameWithType> | <span data-ttu-id="1313a-119">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-119">All</span></span> |
| <xref:System.MarshalByRefObject.InitializeLifetimeService?displayProperty=nameWithType> | <span data-ttu-id="1313a-120">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-120">All</span></span> |
| <xref:System.OperatingSystem.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-121">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-121">All</span></span> |
| <xref:System.Type.ReflectionOnlyGetType(System.String,System.Boolean,System.Boolean)?displayProperty=nameWithType> | <span data-ttu-id="1313a-122">All</span><span class="sxs-lookup"><span data-stu-id="1313a-122">All</span></span> |

## <a name="systemcodedomcompiler"></a><span data-ttu-id="1313a-123">System.CodeDom.Compiler</span><span class="sxs-lookup"><span data-stu-id="1313a-123">System.CodeDom.Compiler</span></span>

| <span data-ttu-id="1313a-124">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-124">Member</span></span> | <span data-ttu-id="1313a-125">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-125">Platforms that throw</span></span> |
| - | - |
| <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromDom%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-126">All</span><span class="sxs-lookup"><span data-stu-id="1313a-126">All</span></span> |
| <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromFile%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-127">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-127">All</span></span> |
| <xref:System.CodeDom.Compiler.CodeDomProvider.CompileAssemblyFromSource%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-128">All</span><span class="sxs-lookup"><span data-stu-id="1313a-128">All</span></span> |

## <a name="systemcollectionsspecialized"></a><span data-ttu-id="1313a-129">System.Collections.Specialized</span><span class="sxs-lookup"><span data-stu-id="1313a-129">System.Collections.Specialized</span></span>

| <span data-ttu-id="1313a-130">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-130">Member</span></span> | <span data-ttu-id="1313a-131">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-131">Platforms that throw</span></span> |
| - | - |
| <xref:System.Collections.Specialized.NameObjectCollectionBase.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-132">All</span><span class="sxs-lookup"><span data-stu-id="1313a-132">All</span></span> |
| <xref:System.Collections.Specialized.NameObjectCollectionBase.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-133">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-133">All</span></span> |
| <xref:System.Collections.Specialized.NameObjectCollectionBase.OnDeserialization(System.Object)?displayProperty=nameWithType> | <span data-ttu-id="1313a-134">All</span><span class="sxs-lookup"><span data-stu-id="1313a-134">All</span></span> |

## <a name="systemconfiguration"></a><span data-ttu-id="1313a-135">System.Configuration</span><span class="sxs-lookup"><span data-stu-id="1313a-135">System.Configuration</span></span>

| <span data-ttu-id="1313a-136">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-136">Member</span></span> | <span data-ttu-id="1313a-137">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-137">Platforms that throw</span></span> |
| - | - |
| <span data-ttu-id="1313a-138"><xref:System.Configuration.RsaProtectedConfigurationProvider?displayProperty=nameWithType>（所有成员）</span><span class="sxs-lookup"><span data-stu-id="1313a-138"><xref:System.Configuration.RsaProtectedConfigurationProvider?displayProperty=nameWithType> (all members)</span></span> | <span data-ttu-id="1313a-139">All</span><span class="sxs-lookup"><span data-stu-id="1313a-139">All</span></span> |

## <a name="systemconsole"></a><span data-ttu-id="1313a-140">System.Console</span><span class="sxs-lookup"><span data-stu-id="1313a-140">System.Console</span></span>

| <span data-ttu-id="1313a-141">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-141">Member</span></span> | <span data-ttu-id="1313a-142">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-142">Platforms that throw</span></span> |
| - | - |
| <xref:System.Console.Beep?displayProperty=nameWithType> | <span data-ttu-id="1313a-143">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-143">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-144"><xref:System.Console.BufferHeight?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-144"><xref:System.Console.BufferHeight?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-145">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-145">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-146"><xref:System.Console.BufferWidth?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-146"><xref:System.Console.BufferWidth?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-147">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-147">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-148"><xref:System.Console.CursorSize?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-148"><xref:System.Console.CursorSize?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-149">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-149">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-150"><xref:System.Console.CursorVisible?displayProperty=nameWithType>（仅获取）</span><span class="sxs-lookup"><span data-stu-id="1313a-150"><xref:System.Console.CursorVisible?displayProperty=nameWithType> (get only)</span></span> | <span data-ttu-id="1313a-151">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-151">Linux and macOS</span></span> |
| <xref:System.Console.MoveBufferArea%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-152">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-152">Linux and macOS</span></span> |
| <xref:System.Console.SetWindowPosition%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-153">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-153">Linux and macOS</span></span> |
| <xref:System.Console.SetWindowSize%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-154">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-154">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-155"><xref:System.Console.Title?displayProperty=nameWithType>（仅获取）</span><span class="sxs-lookup"><span data-stu-id="1313a-155"><xref:System.Console.Title?displayProperty=nameWithType> (get only)</span></span> | <span data-ttu-id="1313a-156">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-156">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-157"><xref:System.Console.WindowHeight?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-157"><xref:System.Console.WindowHeight?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-158">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-158">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-159"><xref:System.Console.WindowLeft?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-159"><xref:System.Console.WindowLeft?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-160">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-160">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-161"><xref:System.Console.WindowTop?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-161"><xref:System.Console.WindowTop?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-162">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-162">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-163"><xref:System.Console.WindowWidth?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-163"><xref:System.Console.WindowWidth?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-164">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-164">Linux and macOS</span></span> |

## <a name="systemdatacommon"></a><span data-ttu-id="1313a-165">System.Data.Common</span><span class="sxs-lookup"><span data-stu-id="1313a-165">System.Data.Common</span></span>

| <span data-ttu-id="1313a-166">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-166">Member</span></span> | <span data-ttu-id="1313a-167">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-167">Platforms that throw</span></span> |
| - | - |
| <span data-ttu-id="1313a-168"><xref:System.Data.Common.DbDataReader.GetSchemaTable%2A?displayProperty=nameWithType>（引发 <xref:System.NotSupportedException>）</span><span class="sxs-lookup"><span data-stu-id="1313a-168"><xref:System.Data.Common.DbDataReader.GetSchemaTable%2A?displayProperty=nameWithType> (throws <xref:System.NotSupportedException>)</span></span> | <span data-ttu-id="1313a-169">All</span><span class="sxs-lookup"><span data-stu-id="1313a-169">All</span></span> |

## <a name="systemdiagnosticsprocess"></a><span data-ttu-id="1313a-170">System.Diagnostics.Process</span><span class="sxs-lookup"><span data-stu-id="1313a-170">System.Diagnostics.Process</span></span>

| <span data-ttu-id="1313a-171">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-171">Member</span></span> | <span data-ttu-id="1313a-172">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-172">Platforms that throw</span></span> |
| - | - |
| <span data-ttu-id="1313a-173"><xref:System.Diagnostics.Process.MaxWorkingSet?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-173"><xref:System.Diagnostics.Process.MaxWorkingSet?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-174">Linux</span><span class="sxs-lookup"><span data-stu-id="1313a-174">Linux</span></span> |
| <span data-ttu-id="1313a-175"><xref:System.Diagnostics.Process.MinWorkingSet?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-175"><xref:System.Diagnostics.Process.MinWorkingSet?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-176">Linux</span><span class="sxs-lookup"><span data-stu-id="1313a-176">Linux</span></span> |
| <xref:System.Diagnostics.Process.ProcessorAffinity?displayProperty=nameWithType> | <span data-ttu-id="1313a-177">macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-177">macOS</span></span> |
| <xref:System.Diagnostics.Process.MainWindowHandle?displayProperty=nameWithType> | <span data-ttu-id="1313a-178">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-178">Linux and macOS</span></span> |
| <xref:System.Diagnostics.Process.Start(System.String,System.String,System.String,System.Security.SecureString,System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-179">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-179">Linux and macOS</span></span> |
| <xref:System.Diagnostics.Process.Start(System.String,System.String,System.Security.SecureString,System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-180">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-180">Linux and macOS</span></span> |
| <xref:System.Diagnostics.ProcessStartInfo.UserName?displayProperty=nameWithType> | <span data-ttu-id="1313a-181">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-181">Linux and macOS</span></span> |
| <xref:System.Diagnostics.ProcessStartInfo.PasswordInClearText?displayProperty=nameWithType> | <span data-ttu-id="1313a-182">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-182">Linux and macOS</span></span> |
| <xref:System.Diagnostics.ProcessStartInfo.Domain?displayProperty=nameWithType> | <span data-ttu-id="1313a-183">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-183">Linux and macOS</span></span> |
| <xref:System.Diagnostics.ProcessStartInfo.LoadUserProfile?displayProperty=nameWithType> | <span data-ttu-id="1313a-184">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-184">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-185"><xref:System.Diagnostics.ProcessThread.BasePriority?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-185"><xref:System.Diagnostics.ProcessThread.BasePriority?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-186">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-186">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-187"><xref:System.Diagnostics.ProcessThread.BasePriority?displayProperty=nameWithType>（仅获取）</span><span class="sxs-lookup"><span data-stu-id="1313a-187"><xref:System.Diagnostics.ProcessThread.BasePriority?displayProperty=nameWithType> (get only)</span></span> | <span data-ttu-id="1313a-188">macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-188">macOS</span></span> |
| <span data-ttu-id="1313a-189"><xref:System.Diagnostics.ProcessThread.ProcessorAffinity?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-189"><xref:System.Diagnostics.ProcessThread.ProcessorAffinity?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-190">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-190">Linux and macOS</span></span> |

## <a name="systemio"></a><span data-ttu-id="1313a-191">System.IO</span><span class="sxs-lookup"><span data-stu-id="1313a-191">System.IO</span></span>

| <span data-ttu-id="1313a-192">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-192">Member</span></span> | <span data-ttu-id="1313a-193">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-193">Platforms that throw</span></span> |
| - | - |
| <xref:System.IO.FileSystemInfo.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-194">All</span><span class="sxs-lookup"><span data-stu-id="1313a-194">All</span></span> |
| <xref:System.IO.FileSystemInfo.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-195">All</span><span class="sxs-lookup"><span data-stu-id="1313a-195">All</span></span> |

## <a name="systemiopipes"></a><span data-ttu-id="1313a-196">System.IO.Pipes</span><span class="sxs-lookup"><span data-stu-id="1313a-196">System.IO.Pipes</span></span>

| <span data-ttu-id="1313a-197">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-197">Member</span></span> | <span data-ttu-id="1313a-198">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-198">Platforms that throw</span></span> |
| - | - |
| <xref:System.IO.Pipes.NamedPipeClientStream.NumberOfServerInstances?displayProperty=nameWithType> | <span data-ttu-id="1313a-199">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-199">Linux and macOS</span></span> |
| <xref:System.IO.Pipes.NamedPipeServerStream.GetImpersonationUserName?displayProperty=nameWithType> | <span data-ttu-id="1313a-200">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-200">Linux and macOS</span></span> |
| <xref:System.IO.Pipes.PipeStream.InBufferSize?displayProperty=nameWithType> | <span data-ttu-id="1313a-201">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-201">Linux and macOS</span></span> |
| <xref:System.IO.Pipes.PipeStream.OutBufferSize?displayProperty=nameWithType> | <span data-ttu-id="1313a-202">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-202">Linux and macOS</span></span> |
| <span data-ttu-id="1313a-203"><xref:System.IO.Pipes.PipeStream.ReadMode?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-203"><xref:System.IO.Pipes.PipeStream.ReadMode?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-204">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-204">Linux and macOS</span></span> |
| <xref:System.IO.Pipes.PipeStream.WaitForPipeDrain?displayProperty=nameWithType> | <span data-ttu-id="1313a-205">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-205">Linux and macOS</span></span> |

## <a name="systemmedia"></a><span data-ttu-id="1313a-206">System.Media</span><span class="sxs-lookup"><span data-stu-id="1313a-206">System.Media</span></span>

| <span data-ttu-id="1313a-207">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-207">Member</span></span> | <span data-ttu-id="1313a-208">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-208">Platforms that throw</span></span> |
| - | - |
| <xref:System.Media.SoundPlayer.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-209">All</span><span class="sxs-lookup"><span data-stu-id="1313a-209">All</span></span> |

## <a name="systemnet"></a><span data-ttu-id="1313a-210">System.Net</span><span class="sxs-lookup"><span data-stu-id="1313a-210">System.Net</span></span>

| <span data-ttu-id="1313a-211">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-211">Member</span></span> | <span data-ttu-id="1313a-212">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-212">Platforms that throw</span></span> |
| - | - |
| <xref:System.Net.AuthenticationManager.Authenticate(System.String,System.Net.WebRequest,System.Net.ICredentials)?displayProperty=nameWithType> | <span data-ttu-id="1313a-213">All</span><span class="sxs-lookup"><span data-stu-id="1313a-213">All</span></span> |
| <xref:System.Net.AuthenticationManager.PreAuthenticate(System.Net.WebRequest,System.Net.ICredentials)?displayProperty=nameWithType> | <span data-ttu-id="1313a-214">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-214">All</span></span> |
| <xref:System.Net.FileWebRequest.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-215">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-215">All</span></span> |
| <xref:System.Net.FileWebRequest.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-216">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-216">All</span></span> |
| <xref:System.Net.FileWebResponse.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-217">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-217">All</span></span> |
| <xref:System.Net.FileWebResponse.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-218">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-218">All</span></span> |
| <xref:System.Net.HttpWebRequest.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-219">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-219">All</span></span> |
| <xref:System.Net.HttpWebRequest.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-220">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-220">All</span></span> |
| <xref:System.Net.HttpWebResponse.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-221">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-221">All</span></span> |
| <xref:System.Net.HttpWebResponse.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-222">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-222">All</span></span> |
| <xref:System.Net.WebProxy.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-223">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-223">All</span></span> |
| <xref:System.Net.WebProxy.GetDefaultProxy?displayProperty=nameWithType> | <span data-ttu-id="1313a-224">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-224">All</span></span> |
| <xref:System.Net.WebProxy.GetObjectData%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-225">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-225">All</span></span> |
| <xref:System.Net.WebRequest.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-226">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-226">All</span></span> |
| <xref:System.Net.WebRequest.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-227">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-227">All</span></span> |
| <xref:System.Net.WebResponse.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-228">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-228">All</span></span> |
| <xref:System.Net.WebResponse.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-229">All</span><span class="sxs-lookup"><span data-stu-id="1313a-229">All</span></span> |

## <a name="systemnetnetworkinformation"></a><span data-ttu-id="1313a-230">System.Net.NetworkInformation</span><span class="sxs-lookup"><span data-stu-id="1313a-230">System.Net.NetworkInformation</span></span>

| <span data-ttu-id="1313a-231">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-231">Member</span></span> | <span data-ttu-id="1313a-232">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-232">Platforms that throw</span></span> |
| - | - |
| <xref:System.Net.NetworkInformation.Ping.Send%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-233">Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="1313a-233">Windows (UWP)</span></span> |

## <a name="systemnetsockets"></a><span data-ttu-id="1313a-234">System.Net.Sockets</span><span class="sxs-lookup"><span data-stu-id="1313a-234">System.Net.Sockets</span></span>

| <span data-ttu-id="1313a-235">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-235">Member</span></span> | <span data-ttu-id="1313a-236">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-236">Platforms that throw</span></span> |
| - | - |
| <xref:System.Net.Sockets.Socket.%23ctor(System.Net.Sockets.SocketInformation)> | <span data-ttu-id="1313a-237">All</span><span class="sxs-lookup"><span data-stu-id="1313a-237">All</span></span> |
| <xref:System.Net.Sockets.Socket.DuplicateAndClose(System.Int32)?displayProperty=nameWithType> | <span data-ttu-id="1313a-238">All</span><span class="sxs-lookup"><span data-stu-id="1313a-238">All</span></span> |

## <a name="systemnetwebsockets"></a><span data-ttu-id="1313a-239">System.Net.WebSockets</span><span class="sxs-lookup"><span data-stu-id="1313a-239">System.Net.WebSockets</span></span>

| <span data-ttu-id="1313a-240">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-240">Member</span></span> | <span data-ttu-id="1313a-241">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-241">Platforms that throw</span></span> |
| - | - |
| <xref:System.Net.WebSockets.WebSocket.RegisterPrefixes?displayProperty=nameWithType> | <span data-ttu-id="1313a-242">All</span><span class="sxs-lookup"><span data-stu-id="1313a-242">All</span></span> |

## <a name="systemreflection"></a><span data-ttu-id="1313a-243">System.Reflection</span><span class="sxs-lookup"><span data-stu-id="1313a-243">System.Reflection</span></span>

| <span data-ttu-id="1313a-244">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-244">Member</span></span> | <span data-ttu-id="1313a-245">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-245">Platforms that throw</span></span> |
| - | - |
| <xref:System.Reflection.Assembly.ReflectionOnlyLoad%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-246">All</span><span class="sxs-lookup"><span data-stu-id="1313a-246">All</span></span> |
| <xref:System.Reflection.Assembly.ReflectionOnlyLoadFrom(System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-247">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-247">All</span></span> |
| <xref:System.Reflection.AssemblyName.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-248">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-248">All</span></span> |
| <xref:System.Reflection.AssemblyName.OnDeserialization(System.Object)?displayProperty=nameWithType> | <span data-ttu-id="1313a-249">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-249">All</span></span> |
| <xref:System.Reflection.StrongNameKeyPair.%23ctor(System.String)> | <span data-ttu-id="1313a-250">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-250">All</span></span> |
| <xref:System.Reflection.StrongNameKeyPair.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-251">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-251">All</span></span> |
| <xref:System.Reflection.StrongNameKeyPair.PublicKey?displayProperty=nameWithType> | <span data-ttu-id="1313a-252">All</span><span class="sxs-lookup"><span data-stu-id="1313a-252">All</span></span> |

## <a name="systemruntimecompilerservices"></a><span data-ttu-id="1313a-253">System.Runtime.CompilerServices</span><span class="sxs-lookup"><span data-stu-id="1313a-253">System.Runtime.CompilerServices</span></span>

| <span data-ttu-id="1313a-254">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-254">Member</span></span> | <span data-ttu-id="1313a-255">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-255">Platforms that throw</span></span> |
| - | - |
| <xref:System.Runtime.CompilerServices.DebugInfoGenerator.CreatePdbGenerator?displayProperty=nameWithType> | <span data-ttu-id="1313a-256">All</span><span class="sxs-lookup"><span data-stu-id="1313a-256">All</span></span> |

## <a name="systemruntimeinteropservices"></a><span data-ttu-id="1313a-257">System.Runtime.InteropServices</span><span class="sxs-lookup"><span data-stu-id="1313a-257">System.Runtime.InteropServices</span></span>

| <span data-ttu-id="1313a-258">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-258">Member</span></span> | <span data-ttu-id="1313a-259">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-259">Platforms that throw</span></span> |
| - | - |
| <xref:System.Runtime.InteropServices.Marshal.GetIDispatchForObject(System.Object)?displayProperty=nameWithType> | <span data-ttu-id="1313a-260">All</span><span class="sxs-lookup"><span data-stu-id="1313a-260">All</span></span> |
| <xref:System.Runtime.InteropServices.RuntimeEnvironment.SystemConfigurationFile?displayProperty=nameWithType> | <span data-ttu-id="1313a-261">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-261">All</span></span> |
| <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeInterfaceAsIntPtr(System.Guid,System.Guid)?displayProperty=nameWithType> | <span data-ttu-id="1313a-262">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-262">All</span></span> |
| <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeInterfaceAsObject(System.Guid,System.Guid)?displayProperty=nameWithType> | <span data-ttu-id="1313a-263">All</span><span class="sxs-lookup"><span data-stu-id="1313a-263">All</span></span> |
| <xref:System.Runtime.InteropServices.WindowsRuntime.WindowsRuntimeMarshal.StringToHString(System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-264">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-264">Linux and macOS</span></span> |
| <xref:System.Runtime.InteropServices.WindowsRuntime.WindowsRuntimeMarshal.PtrToStringHString(System.IntPtr)?displayProperty=nameWithType> | <span data-ttu-id="1313a-265">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-265">Linux and macOS</span></span> |
| <xref:System.Runtime.InteropServices.WindowsRuntime.WindowsRuntimeMarshal.FreeHString(System.IntPtr)?displayProperty=nameWithType> | <span data-ttu-id="1313a-266">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-266">Linux and macOS</span></span> |

## <a name="systemruntimeserialization"></a><span data-ttu-id="1313a-267">System.Runtime.Serialization</span><span class="sxs-lookup"><span data-stu-id="1313a-267">System.Runtime.Serialization</span></span>

| <span data-ttu-id="1313a-268">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-268">Member</span></span> | <span data-ttu-id="1313a-269">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-269">Platforms that throw</span></span> |
| - | - |
| <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas?displayProperty=nameWithType> | <span data-ttu-id="1313a-270">All</span><span class="sxs-lookup"><span data-stu-id="1313a-270">All</span></span> |

## <a name="systemsecurity"></a><span data-ttu-id="1313a-271">System.Security</span><span class="sxs-lookup"><span data-stu-id="1313a-271">System.Security</span></span>

| <span data-ttu-id="1313a-272">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-272">Member</span></span> | <span data-ttu-id="1313a-273">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-273">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.CodeAccessPermission.Deny?displayProperty=nameWithType> | <span data-ttu-id="1313a-274">All</span><span class="sxs-lookup"><span data-stu-id="1313a-274">All</span></span> |
| <xref:System.Security.CodeAccessPermission.PermitOnly?displayProperty=nameWithType> | <span data-ttu-id="1313a-275">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-275">All</span></span> |
| <xref:System.Security.PermissionSet.ConvertPermissionSet(System.String,System.Byte[],System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-276">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-276">All</span></span> |
| <xref:System.Security.PermissionSet.Deny?displayProperty=nameWithType> | <span data-ttu-id="1313a-277">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-277">All</span></span> |
| <xref:System.Security.PermissionSet.PermitOnly?displayProperty=nameWithType> | <span data-ttu-id="1313a-278">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-278">All</span></span> |
| <xref:System.Security.SecurityContext.Capture?displayProperty=nameWithType> | <span data-ttu-id="1313a-279">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-279">All</span></span> |
| <xref:System.Security.SecurityContext.CreateCopy?displayProperty=nameWithType> | <span data-ttu-id="1313a-280">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-280">All</span></span> |
| <xref:System.Security.SecurityContext.Dispose?displayProperty=nameWithType> | <span data-ttu-id="1313a-281">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-281">All</span></span> |
| <xref:System.Security.SecurityContext.IsFlowSuppressed?displayProperty=nameWithType> | <span data-ttu-id="1313a-282">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-282">All</span></span> |
| <xref:System.Security.SecurityContext.IsWindowsIdentityFlowSuppressed?displayProperty=nameWithType> | <span data-ttu-id="1313a-283">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-283">All</span></span> |
| <xref:System.Security.SecurityContext.RestoreFlow?displayProperty=nameWithType> | <span data-ttu-id="1313a-284">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-284">All</span></span> |
| <xref:System.Security.SecurityContext.Run(System.Security.SecurityContext,System.Threading.ContextCallback,System.Object)?displayProperty=nameWithType> | <span data-ttu-id="1313a-285">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-285">All</span></span> |
| <xref:System.Security.SecurityContext.SuppressFlow?displayProperty=nameWithType> | <span data-ttu-id="1313a-286">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-286">All</span></span> |
| <xref:System.Security.SecurityContext.SuppressFlowWindowsIdentity?displayProperty=nameWithType> | <span data-ttu-id="1313a-287">All</span><span class="sxs-lookup"><span data-stu-id="1313a-287">All</span></span> |

## <a name="systemsecurityclaims"></a><span data-ttu-id="1313a-288">System.Security.Claims</span><span class="sxs-lookup"><span data-stu-id="1313a-288">System.Security.Claims</span></span>

| <span data-ttu-id="1313a-289">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-289">Member</span></span> | <span data-ttu-id="1313a-290">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-290">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.Claims.ClaimsPrincipal.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-291">All</span><span class="sxs-lookup"><span data-stu-id="1313a-291">All</span></span> |
| <xref:System.Security.Claims.ClaimsPrincipal.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-292">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-292">All</span></span> |
| <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Runtime.Serialization.SerializationInfo)> | <span data-ttu-id="1313a-293">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-293">All</span></span> |
| <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-294">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-294">All</span></span> |
| <xref:System.Security.Claims.ClaimsIdentity.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-295">All</span><span class="sxs-lookup"><span data-stu-id="1313a-295">All</span></span> |

## <a name="systemsecuritycryptography"></a><span data-ttu-id="1313a-296">System.Security.Cryptography</span><span class="sxs-lookup"><span data-stu-id="1313a-296">System.Security.Cryptography</span></span>

| <span data-ttu-id="1313a-297">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-297">Member</span></span> | <span data-ttu-id="1313a-298">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-298">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.Cryptography.AsymmetricAlgorithm.Create(System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-299">All</span><span class="sxs-lookup"><span data-stu-id="1313a-299">All</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.%23ctor%2A> | <span data-ttu-id="1313a-300">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-300">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.Accessible?displayProperty=nameWithType> | <span data-ttu-id="1313a-301">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-301">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.Exportable?displayProperty=nameWithType> | <span data-ttu-id="1313a-302">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-302">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.HardwareDevice?displayProperty=nameWithType> | <span data-ttu-id="1313a-303">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-303">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.KeyContainerName?displayProperty=nameWithType> | <span data-ttu-id="1313a-304">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-304">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.KeyNumber?displayProperty=nameWithType> | <span data-ttu-id="1313a-305">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-305">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.MachineKeyStore?displayProperty=nameWithType> | <span data-ttu-id="1313a-306">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-306">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.Protected?displayProperty=nameWithType> | <span data-ttu-id="1313a-307">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-307">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.ProviderName?displayProperty=nameWithType> | <span data-ttu-id="1313a-308">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-308">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.ProviderType?displayProperty=nameWithType> | <span data-ttu-id="1313a-309">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-309">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.RandomlyGenerated?displayProperty=nameWithType> | <span data-ttu-id="1313a-310">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-310">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.Removable?displayProperty=nameWithType> | <span data-ttu-id="1313a-311">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-311">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.CspKeyContainerInfo.UniqueKeyContainerName?displayProperty=nameWithType> | <span data-ttu-id="1313a-312">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-312">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.HashAlgorithm.Create?displayProperty=nameWithType> | <span data-ttu-id="1313a-313">All</span><span class="sxs-lookup"><span data-stu-id="1313a-313">All</span></span> |
| <xref:System.Security.Cryptography.HMAC.Create?displayProperty=nameWithType> | <span data-ttu-id="1313a-314">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-314">All</span></span> |
| <xref:System.Security.Cryptography.HMAC.Create(System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-315">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-315">All</span></span> |
| <xref:System.Security.Cryptography.HMAC.HashCore%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-316">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-316">All</span></span> |
| <xref:System.Security.Cryptography.HMAC.HashFinal%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-317">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-317">All</span></span> |
| <xref:System.Security.Cryptography.HMAC.Initialize%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-318">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-318">All</span></span> |
| <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create?displayProperty=nameWithType> | <span data-ttu-id="1313a-319">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-319">All</span></span> |
| <xref:System.Security.Cryptography.KeyedHashAlgorithm.Create(System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-320">All</span><span class="sxs-lookup"><span data-stu-id="1313a-320">All</span></span> |
| <xref:System.Security.Cryptography.ProtectedData.Protect%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-321">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-321">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.ProtectedData.Unprotect%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-322">Linux 和 macOS</span><span class="sxs-lookup"><span data-stu-id="1313a-322">Linux and macOS</span></span> |
| <xref:System.Security.Cryptography.RSA.FromXmlString%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-323">All</span><span class="sxs-lookup"><span data-stu-id="1313a-323">All</span></span> |
| <xref:System.Security.Cryptography.RSA.ToXmlString%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-324">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-324">All</span></span> |
| <xref:System.Security.Cryptography.SymmetricAlgorithm.Create?displayProperty=nameWithType> | <span data-ttu-id="1313a-325">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-325">All</span></span> |
| <xref:System.Security.Cryptography.SymmetricAlgorithm.Create(System.String)?displayProperty=nameWithType> | <span data-ttu-id="1313a-326">All</span><span class="sxs-lookup"><span data-stu-id="1313a-326">All</span></span> |

## <a name="systemsecuritycryptographypkcs"></a><span data-ttu-id="1313a-327">System.Security.Cryptography.Pkcs</span><span class="sxs-lookup"><span data-stu-id="1313a-327">System.Security.Cryptography.Pkcs</span></span>

| <span data-ttu-id="1313a-328">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-328">Member</span></span> | <span data-ttu-id="1313a-329">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-329">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.Cryptography.Pkcs.CmsSigner.%23ctor(System.Security.Cryptography.CspParameters)> | <span data-ttu-id="1313a-330">All</span><span class="sxs-lookup"><span data-stu-id="1313a-330">All</span></span> |
| <xref:System.Security.Cryptography.Pkcs.SignerInfo.ComputeCounterSignature?displayProperty=nameWithType> | <span data-ttu-id="1313a-331">All</span><span class="sxs-lookup"><span data-stu-id="1313a-331">All</span></span> |

## <a name="systemsecuritycryptographyx509certificates"></a><span data-ttu-id="1313a-332">System.Security.Cryptography.X509Certificates</span><span class="sxs-lookup"><span data-stu-id="1313a-332">System.Security.Cryptography.X509Certificates</span></span>

| <span data-ttu-id="1313a-333">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-333">Member</span></span> | <span data-ttu-id="1313a-334">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-334">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.Cryptography.X509Certificates.X509Certificate.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-335">All</span><span class="sxs-lookup"><span data-stu-id="1313a-335">All</span></span> |
| <xref:System.Security.Cryptography.X509Certificates.X509Certificate.Import%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-336">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-336">All</span></span> |
| <xref:System.Security.Cryptography.X509Certificates.X509Certificate2.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-337">All</span><span class="sxs-lookup"><span data-stu-id="1313a-337">All</span></span> |
| <span data-ttu-id="1313a-338"><xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType>（仅设置）</span><span class="sxs-lookup"><span data-stu-id="1313a-338"><xref:System.Security.Cryptography.X509Certificates.X509Certificate2.PrivateKey?displayProperty=nameWithType> (set only)</span></span> | <span data-ttu-id="1313a-339">All</span><span class="sxs-lookup"><span data-stu-id="1313a-339">All</span></span> |

## <a name="systemsecurityauthenticationextendedprotection"></a><span data-ttu-id="1313a-340">System.Security.Authentication.ExtendedProtection</span><span class="sxs-lookup"><span data-stu-id="1313a-340">System.Security.Authentication.ExtendedProtection</span></span>

| <span data-ttu-id="1313a-341">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-341">Member</span></span> | <span data-ttu-id="1313a-342">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-342">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-343">All</span><span class="sxs-lookup"><span data-stu-id="1313a-343">All</span></span> |

## <a name="systemsecuritypolicy"></a><span data-ttu-id="1313a-344">System.Security.Policy</span><span class="sxs-lookup"><span data-stu-id="1313a-344">System.Security.Policy</span></span>

| <span data-ttu-id="1313a-345">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-345">Member</span></span> | <span data-ttu-id="1313a-346">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-346">Platforms that throw</span></span> |
| - | - |
| <xref:System.Security.Policy.Hash.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-347">All</span><span class="sxs-lookup"><span data-stu-id="1313a-347">All</span></span> |

## <a name="systemserviceprocessservicecontroller"></a><span data-ttu-id="1313a-348">System.ServiceProcess.ServiceController</span><span class="sxs-lookup"><span data-stu-id="1313a-348">System.ServiceProcess.ServiceController</span></span>

| <span data-ttu-id="1313a-349">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-349">Member</span></span> | <span data-ttu-id="1313a-350">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-350">Platforms that throw</span></span> |
| - | - |
| <xref:System.ServiceProcess.TimeoutException.%23ctor(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)> | <span data-ttu-id="1313a-351">All</span><span class="sxs-lookup"><span data-stu-id="1313a-351">All</span></span> |

## <a name="systemtextregularexpressions"></a><span data-ttu-id="1313a-352">System.Text.RegularExpressions</span><span class="sxs-lookup"><span data-stu-id="1313a-352">System.Text.RegularExpressions</span></span>

| <span data-ttu-id="1313a-353">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-353">Member</span></span> | <span data-ttu-id="1313a-354">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-354">Platforms that throw</span></span> |
| - | - |
| <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-355">All</span><span class="sxs-lookup"><span data-stu-id="1313a-355">All</span></span> |

## <a name="systemthreading"></a><span data-ttu-id="1313a-356">System.Threading</span><span class="sxs-lookup"><span data-stu-id="1313a-356">System.Threading</span></span>

| <span data-ttu-id="1313a-357">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-357">Member</span></span> | <span data-ttu-id="1313a-358">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-358">Platforms that throw</span></span> |
| - | - |
| <xref:System.Threading.CompressedStack.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-359">All</span><span class="sxs-lookup"><span data-stu-id="1313a-359">All</span></span> |
| <xref:System.Threading.ExecutionContext.GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | <span data-ttu-id="1313a-360">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-360">All</span></span> |
| <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> | <span data-ttu-id="1313a-361">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-361">All</span></span> |
| <xref:System.Threading.Thread.ResetAbort?displayProperty=nameWithType> | <span data-ttu-id="1313a-362">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-362">All</span></span> |
| <xref:System.Threading.Thread.Resume?displayProperty=nameWithType> | <span data-ttu-id="1313a-363">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-363">All</span></span> |
| <xref:System.Threading.Thread.Suspend?displayProperty=nameWithType> | <span data-ttu-id="1313a-364">All</span><span class="sxs-lookup"><span data-stu-id="1313a-364">All</span></span> |

## <a name="systemxml"></a><span data-ttu-id="1313a-365">System.Xml</span><span class="sxs-lookup"><span data-stu-id="1313a-365">System.Xml</span></span>

| <span data-ttu-id="1313a-366">成员</span><span class="sxs-lookup"><span data-stu-id="1313a-366">Member</span></span> | <span data-ttu-id="1313a-367">引发异常的平台</span><span class="sxs-lookup"><span data-stu-id="1313a-367">Platforms that throw</span></span> |
| - | - |
| <xref:System.Xml.XmlDictionaryReader.CreateMtomReader(System.Byte[],System.Int32,System.Int32,System.Text.Encoding[],System.String,System.Xml.XmlDictionaryReaderQuotas,System.Int32,System.Xml.OnXmlDictionaryReaderClose)?displayProperty=nameWithType> | <span data-ttu-id="1313a-368">All</span><span class="sxs-lookup"><span data-stu-id="1313a-368">All</span></span> |
| <xref:System.Xml.XmlDictionaryReader.CreateMtomReader(System.IO.Stream,System.Text.Encoding[],System.String,System.Xml.XmlDictionaryReaderQuotas,System.Int32,System.Xml.OnXmlDictionaryReaderClose)?displayProperty=nameWithType> | <span data-ttu-id="1313a-369">全部</span><span class="sxs-lookup"><span data-stu-id="1313a-369">All</span></span> |
| <xref:System.Xml.XmlDictionaryWriter.CreateMtomWriter(System.IO.Stream,System.Text.Encoding,System.Int32,System.String,System.String,System.String,System.Boolean,System.Boolean)?displayProperty=nameWithType> | <span data-ttu-id="1313a-370">All</span><span class="sxs-lookup"><span data-stu-id="1313a-370">All</span></span> |

## <a name="see-also"></a><span data-ttu-id="1313a-371">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1313a-371">See also</span></span>

- [<span data-ttu-id="1313a-372">从 .NET Framework 迁移到 .NET Core 的中断性变更</span><span class="sxs-lookup"><span data-stu-id="1313a-372">Breaking changes for migration from .NET Framework to .NET Core</span></span>](fx-core.md)
- [<span data-ttu-id="1313a-373">.NET Core 中的二进制序列化</span><span class="sxs-lookup"><span data-stu-id="1313a-373">Binary serialization in .NET Core</span></span>](../../standard/serialization/binary-serialization.md#net-core)
- [<span data-ttu-id="1313a-374">.NET 可移植性分析器</span><span class="sxs-lookup"><span data-stu-id="1313a-374">.NET portability analyzer</span></span>](../../standard/analyzers/portability-analyzer.md)
