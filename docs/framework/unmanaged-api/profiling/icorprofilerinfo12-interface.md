---
description: 了解详细信息： ICorProfilerInfo12 接口
title: ICorProfilerInfo12 接口
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: a8b91eba686478c3e825ae15d6ae95bd0ffad944
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805722"
---
# <a name="icorprofilerinfo12-interface"></a><span data-ttu-id="c6322-103">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="c6322-103">ICorProfilerInfo12 Interface</span></span>

 <span data-ttu-id="c6322-104">[ICorProfilerInfo11](icorprofilerinfo11-interface.md)的子类，提供创建 EventPipe 会话、事件和提供程序的方法。</span><span class="sxs-lookup"><span data-stu-id="c6322-104">A subclass of [ICorProfilerInfo11](icorprofilerinfo11-interface.md) that provides methods to create EventPipe sessions, events, and providers.</span></span>
  
## <a name="methods"></a><span data-ttu-id="c6322-105">方法</span><span class="sxs-lookup"><span data-stu-id="c6322-105">Methods</span></span>  
  
|<span data-ttu-id="c6322-106">方法</span><span class="sxs-lookup"><span data-stu-id="c6322-106">Method</span></span>|<span data-ttu-id="c6322-107">说明</span><span class="sxs-lookup"><span data-stu-id="c6322-107">Description</span></span>|  
|------------|-----------------|  
|[<span data-ttu-id="c6322-108">EventPipeStartSession 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-108">EventPipeStartSession Method</span></span>](icorprofilerinfo12-eventpipestartsession-method.md)|<span data-ttu-id="c6322-109">启动探查器 EventPipe 会话。</span><span class="sxs-lookup"><span data-stu-id="c6322-109">Starts a profiler EventPipe session.</span></span>|
|[<span data-ttu-id="c6322-110">EventPipeAddProviderToSession 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-110">EventPipeAddProviderToSession Method</span></span>](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)|<span data-ttu-id="c6322-111">向现有的 EventPipe 会话添加提供程序。</span><span class="sxs-lookup"><span data-stu-id="c6322-111">Adds a provider to an existing EventPipe session.</span></span>|
|[<span data-ttu-id="c6322-112">EventPipeStopSession 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-112">EventPipeStopSession Method</span></span>](icorprofilerinfo12-eventpipestopsession-method.md)|<span data-ttu-id="c6322-113">停止 EventPipe 会话。</span><span class="sxs-lookup"><span data-stu-id="c6322-113">Stops an EventPipe session.</span></span>|
|[<span data-ttu-id="c6322-114">EventPipeCreateProvider 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-114">EventPipeCreateProvider Method</span></span>](icorprofilerinfo12-eventpipecreateprovider-method.md)|<span data-ttu-id="c6322-115">创建 EventPipe 提供程序。</span><span class="sxs-lookup"><span data-stu-id="c6322-115">Creates an EventPipe provider.</span></span>|  
|[<span data-ttu-id="c6322-116">EventPipeGetProviderInfo 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-116">EventPipeGetProviderInfo Method</span></span>](icorprofilerinfo12-eventpipegetproviderinfo-method.md)|<span data-ttu-id="c6322-117">从其 ID 获取 EventPipe 提供程序的名称。</span><span class="sxs-lookup"><span data-stu-id="c6322-117">Gets the name of an EventPipe provider from its ID.</span></span>|
|[<span data-ttu-id="c6322-118">EventPipeDefineEvent 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-118">EventPipeDefineEvent Method</span></span>](icorprofilerinfo12-eventpipedefineevent-method.md)|<span data-ttu-id="c6322-119">定义现有 EventPipe 提供程序上的事件。</span><span class="sxs-lookup"><span data-stu-id="c6322-119">Defines an event on an existing EventPipe provider.</span></span>|  
|[<span data-ttu-id="c6322-120">EventPipeWriteEvent 方法</span><span class="sxs-lookup"><span data-stu-id="c6322-120">EventPipeWriteEvent Method</span></span>](icorprofilerinfo12-eventpipewriteevent-method.md)|<span data-ttu-id="c6322-121">写入 EventPipe 事件。</span><span class="sxs-lookup"><span data-stu-id="c6322-121">Writes an EventPipe event.</span></span>|
  
## <a name="requirements"></a><span data-ttu-id="c6322-122">要求</span><span class="sxs-lookup"><span data-stu-id="c6322-122">Requirements</span></span>  

<span data-ttu-id="c6322-123">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="c6322-123">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="c6322-124">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="c6322-124">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="c6322-125">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="c6322-125">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="c6322-126">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c6322-126">See also</span></span>

- [<span data-ttu-id="c6322-127">EventPipe 概述</span><span class="sxs-lookup"><span data-stu-id="c6322-127">EventPipe Overview</span></span>](../../../core/diagnostics/eventpipe.md)
- [<span data-ttu-id="c6322-128">众所周知的 EventProviders</span><span class="sxs-lookup"><span data-stu-id="c6322-128">Well Known EventProviders</span></span>](../../../core/diagnostics/well-known-event-providers.md)
- [<span data-ttu-id="c6322-129">分析接口</span><span class="sxs-lookup"><span data-stu-id="c6322-129">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="c6322-130">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="c6322-130">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
