---
description: 了解详细信息： ICorProfilerCallback10：： EventPipeEventDelivered 方法
title: ICorProfilerCallback10：： EventPipeEventDelivered 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10.EventPipeEventDelivered
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 7b2ca813d610c2b41d97d8cd76dac22ca38802d7
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805631"
---
# <a name="icorprofilercallback10eventpipeeventdelivered-method"></a><span data-ttu-id="784ce-103">ICorProfilerCallback10：： EventPipeEventDelivered 方法</span><span class="sxs-lookup"><span data-stu-id="784ce-103">ICorProfilerCallback10::EventPipeEventDelivered Method</span></span>

<span data-ttu-id="784ce-104">每当 EventPipe 事件传递到探查器的当前活动会话时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="784ce-104">Notifies the profiler whenever an EventPipe event has been delivered to the profiler's currently active session.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="784ce-105">语法</span><span class="sxs-lookup"><span data-stu-id="784ce-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeEventDelivered(
        [in] EVENTPIPE_PROVIDER provider,
        [in] DWORD eventId,
        [in] DWORD eventVersion,
        [in] ULONG cbMetadataBlob,
        [in, size_is(cbMetadataBlob)] LPCBYTE metadataBlob,
        [in] ULONG cbEventData,
        [in, size_is(cbEventData)] LPCBYTE eventData,
        [in] LPCGUID pActivityId,
        [in] LPCGUID pRelatedActivityId,
        [in] ThreadID eventThread,
        [in] ULONG numStackFrames,
        [in, length_is(numStackFrames)] UINT_PTR stackFrames[]); 
```  
  
## <a name="parameters"></a><span data-ttu-id="784ce-106">参数</span><span class="sxs-lookup"><span data-stu-id="784ce-106">Parameters</span></span>

<span data-ttu-id="784ce-107">`provider` 中此事件源自的提供程序。</span><span class="sxs-lookup"><span data-stu-id="784ce-107">`provider` [in] The provider that this event originated from.</span></span>

<span data-ttu-id="784ce-108">`eventId` 中要传递的事件的 ID。</span><span class="sxs-lookup"><span data-stu-id="784ce-108">`eventId` [in] The ID of the event being delivered.</span></span>

<span data-ttu-id="784ce-109">`eventVersion` 中要传递的事件的版本。</span><span class="sxs-lookup"><span data-stu-id="784ce-109">`eventVersion` [in] The version of the event being delivered.</span></span>

<span data-ttu-id="784ce-110">`cbMetadataBlob` 中的长度（以字节为单位） `metadataBlob` 。</span><span class="sxs-lookup"><span data-stu-id="784ce-110">`cbMetadataBlob` [in] The length, in bytes, of `metadataBlob`.</span></span>

<span data-ttu-id="784ce-111">`metadataBlob` 中指向事件的元数据 blob 的指针。</span><span class="sxs-lookup"><span data-stu-id="784ce-111">`metadataBlob` [in] A pointer to the metadata blob for the event.</span></span>

<span data-ttu-id="784ce-112">`cbEventData` 中的长度（以字节为单位） `eventData` 。</span><span class="sxs-lookup"><span data-stu-id="784ce-112">`cbEventData` [in] The length, in bytes, of `eventData`.</span></span>

<span data-ttu-id="784ce-113">`eventData` 中事件的负载。</span><span class="sxs-lookup"><span data-stu-id="784ce-113">`eventData` [in] The payload for the event.</span></span>

<span data-ttu-id="784ce-114">`pActivityId` 中指向表示事件的活动 ID 的 GUID 的指针，或为 NULL。</span><span class="sxs-lookup"><span data-stu-id="784ce-114">`pActivityId` [in] A pointer to the GUID that represents the event's activity ID, or NULL.</span></span>

<span data-ttu-id="784ce-115">`pRelatedActivityId` 中指向表示事件相关活动 ID 的 GUID 的指针，或为 NULL。</span><span class="sxs-lookup"><span data-stu-id="784ce-115">`pRelatedActivityId` [in] A pointer to the GUID that represents the event's related activity ID, or NULL.</span></span>

<span data-ttu-id="784ce-116">`eventThread` 中发生事件的线程的 ID。</span><span class="sxs-lookup"><span data-stu-id="784ce-116">`eventThread` [in] The ID of the thread the event occurred on.</span></span>

<span data-ttu-id="784ce-117">`numStackFrames` 中数组中元素的数目 `stackFrames` 。</span><span class="sxs-lookup"><span data-stu-id="784ce-117">`numStackFrames` [in] The number of elements in the `stackFrames` array.</span></span>

<span data-ttu-id="784ce-118">`stackFrames` 中一个代码地址数组，表示事件的托管调用堆栈。</span><span class="sxs-lookup"><span data-stu-id="784ce-118">`stackFrames` [in] An array of code addresses representing the managed callstack of the event.</span></span>

## <a name="requirements"></a><span data-ttu-id="784ce-119">要求</span><span class="sxs-lookup"><span data-stu-id="784ce-119">Requirements</span></span>  

<span data-ttu-id="784ce-120">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="784ce-120">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="784ce-121">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="784ce-121">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="784ce-122">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="784ce-122">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="784ce-123">另请参阅</span><span class="sxs-lookup"><span data-stu-id="784ce-123">See also</span></span>

- [<span data-ttu-id="784ce-124">分析接口</span><span class="sxs-lookup"><span data-stu-id="784ce-124">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="784ce-125">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="784ce-125">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="784ce-126">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="784ce-126">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="784ce-127">ICorProfilerInfo12. EventPipeStartSession 方法</span><span class="sxs-lookup"><span data-stu-id="784ce-127">ICorProfilerInfo12.EventPipeStartSession Method</span></span>](icorprofilerinfo12-eventpipestartsession-method.md)
