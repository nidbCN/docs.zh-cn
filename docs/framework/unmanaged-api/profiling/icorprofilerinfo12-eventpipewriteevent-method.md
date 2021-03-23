---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeWriteEvent 方法
title: ICorProfilerInfo12：： EventPipeWriteEvent 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeWriteEvent
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: a0a06ff0fa1c936518586834f4dfc73810ef0e44
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805735"
---
# <a name="icorprofilerinfo12eventpipewriteevent-method"></a><span data-ttu-id="c3507-103">ICorProfilerInfo12：： EventPipeWriteEvent 方法</span><span class="sxs-lookup"><span data-stu-id="c3507-103">ICorProfilerInfo12::EventPipeWriteEvent Method</span></span>

<span data-ttu-id="c3507-104">将 EventPipe 事件写入已启用此事件的任何侦听器。</span><span class="sxs-lookup"><span data-stu-id="c3507-104">Writes an EventPipe event to any listeners who have enabled this event.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="c3507-105">语法</span><span class="sxs-lookup"><span data-stu-id="c3507-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeWriteEvent(
                [in] EVENTPIPE_EVENT    event,
                [in] UINT32             cData,
                [in, size_is(cData)]
                     COR_PRF_EVENT_DATA data[],
                [in] LPCGUID            pActivityId,
                [in] LPCGUID            pRelatedActivityId);
```  
  
## <a name="parameters"></a><span data-ttu-id="c3507-106">参数</span><span class="sxs-lookup"><span data-stu-id="c3507-106">Parameters</span></span>

<span data-ttu-id="c3507-107">`event` 中要写入的事件的 ID。</span><span class="sxs-lookup"><span data-stu-id="c3507-107">`event` [in] The ID of the event being written.</span></span>

<span data-ttu-id="c3507-108">`cData` 中中的元素数目 `data` 。</span><span class="sxs-lookup"><span data-stu-id="c3507-108">`cData` [in] The number of elements in `data`.</span></span>

<span data-ttu-id="c3507-109">`data` 中 `COR_PRF_EVENT_DATA` 包含事件参数的数组。</span><span class="sxs-lookup"><span data-stu-id="c3507-109">`data` [in] An array of `COR_PRF_EVENT_DATA` containing the event arguments.</span></span>

<span data-ttu-id="c3507-110">`pActivityId` 中指向指定事件的活动 ID 的 GUID 的指针。</span><span class="sxs-lookup"><span data-stu-id="c3507-110">`pActivityId` [in] A pointer to a GUID specifying the event's activity ID.</span></span>

<span data-ttu-id="c3507-111">`pRelatedActivityId` 中指向指定事件相关活动 ID 的 GUID 的指针。</span><span class="sxs-lookup"><span data-stu-id="c3507-111">`pRelatedActivityId` [in] A pointer to a GUID specifying the event's related activity ID.</span></span>

## <a name="requirements"></a><span data-ttu-id="c3507-112">要求</span><span class="sxs-lookup"><span data-stu-id="c3507-112">Requirements</span></span>  

<span data-ttu-id="c3507-113">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="c3507-113">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="c3507-114">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="c3507-114">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="c3507-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c3507-115">See also</span></span>

- [<span data-ttu-id="c3507-116">分析接口</span><span class="sxs-lookup"><span data-stu-id="c3507-116">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="c3507-117">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="c3507-117">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="c3507-118">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="c3507-118">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="c3507-119">COR_PRF_EVENT_DATA 结构</span><span class="sxs-lookup"><span data-stu-id="c3507-119">COR_PRF_EVENT_DATA Structure</span></span>](cor-prf-event-data-structure.md)
