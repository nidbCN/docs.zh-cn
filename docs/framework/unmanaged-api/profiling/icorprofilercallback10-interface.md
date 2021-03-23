---
description: 了解详细信息： ICorProfilerCallback10 接口
title: ICorProfilerCallback10 接口
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: d9091dc81307e2dcb83e74154a8217dffaa1e7a2
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805644"
---
# <a name="icorprofilercallback10-interface"></a><span data-ttu-id="8468c-103">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="8468c-103">ICorProfilerCallback10 Interface</span></span>

 <span data-ttu-id="8468c-104">[ICorProfilerCallback9](icorprofilercallback9-interface.md)的子类，提供回调方法，以通知探查器 EventPipe 事件已传递到探查器的当前活动会话。</span><span class="sxs-lookup"><span data-stu-id="8468c-104">A subclass of [ICorProfilerCallback9](icorprofilercallback9-interface.md) that provides callback methods to notify the profiler that EventPipe events have been delivered to the profiler's currently active session.</span></span>
  
## <a name="methods"></a><span data-ttu-id="8468c-105">方法</span><span class="sxs-lookup"><span data-stu-id="8468c-105">Methods</span></span>  
  
|<span data-ttu-id="8468c-106">方法</span><span class="sxs-lookup"><span data-stu-id="8468c-106">Method</span></span>|<span data-ttu-id="8468c-107">说明</span><span class="sxs-lookup"><span data-stu-id="8468c-107">Description</span></span>|  
|------------|-----------------|  
|[<span data-ttu-id="8468c-108">EventPipeEventDelivered 方法</span><span class="sxs-lookup"><span data-stu-id="8468c-108">EventPipeEventDelivered Method</span></span>](icorprofilercallback10-eventpipeeventdelivered-method.md)|<span data-ttu-id="8468c-109">通知探查器 EventPipe 事件已传递到探查器已打开的会话。</span><span class="sxs-lookup"><span data-stu-id="8468c-109">Notifies the profiler that an EventPipe event has been delivered to the session that the profiler has open.</span></span>|
|[<span data-ttu-id="8468c-110">EventPipeProviderCreated 方法</span><span class="sxs-lookup"><span data-stu-id="8468c-110">EventPipeProviderCreated Method</span></span>](icorprofilercallback10-eventpipeprovidercreated-method.md)|<span data-ttu-id="8468c-111">通知探查器已创建 EventPipe 提供程序，从而允许探查器将该提供程序添加到其会话中。</span><span class="sxs-lookup"><span data-stu-id="8468c-111">Notifies the profiler that an EventPipe provider has been created, allowing the profiler to add that provider the their session.</span></span>|  
  
## <a name="requirements"></a><span data-ttu-id="8468c-112">要求</span><span class="sxs-lookup"><span data-stu-id="8468c-112">Requirements</span></span>  

<span data-ttu-id="8468c-113">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="8468c-113">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="8468c-114">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="8468c-114">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="8468c-115">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="8468c-115">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="8468c-116">另请参阅</span><span class="sxs-lookup"><span data-stu-id="8468c-116">See also</span></span>

- [<span data-ttu-id="8468c-117">EventPipe 概述</span><span class="sxs-lookup"><span data-stu-id="8468c-117">EventPipe Overview</span></span>](../../../core/diagnostics/eventpipe.md)
- [<span data-ttu-id="8468c-118">众所周知的 EventProviders</span><span class="sxs-lookup"><span data-stu-id="8468c-118">Well Known EventProviders</span></span>](../../../core/diagnostics/well-known-event-providers.md)
- [<span data-ttu-id="8468c-119">分析接口</span><span class="sxs-lookup"><span data-stu-id="8468c-119">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="8468c-120">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="8468c-120">ICorProfilerInfo12 Interface</span></span>](ICorProfilerInfo12-interface.md)
- [<span data-ttu-id="8468c-121">ICorProfilerInfo12. EventPipeStartSession 方法</span><span class="sxs-lookup"><span data-stu-id="8468c-121">ICorProfilerInfo12.EventPipeStartSession method</span></span>](ICorProfilerInfo12-eventpipestartsession-method.md)
- [<span data-ttu-id="8468c-122">ICorProfilerInfo12. EventPipeStopSession 方法</span><span class="sxs-lookup"><span data-stu-id="8468c-122">ICorProfilerInfo12.EventPipeStopSession method</span></span>](ICorProfilerInfo12-eventpipestopsession-method.md)
