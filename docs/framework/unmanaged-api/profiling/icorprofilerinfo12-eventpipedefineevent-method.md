---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeDefineEvent 方法
title: ICorProfilerInfo12：： EventPipeDefineEvent 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeDefineEvent
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 845f7f26dce7aa0f4b7d895b9f04cc89e7ac7192
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805748"
---
# <a name="icorprofilerinfo12eventpipedefineevent-method"></a><span data-ttu-id="d65e7-103">ICorProfilerInfo12：： EventPipeDefineEvent 方法</span><span class="sxs-lookup"><span data-stu-id="d65e7-103">ICorProfilerInfo12::EventPipeDefineEvent Method</span></span>

<span data-ttu-id="d65e7-104">定义现有提供程序上的 EventPipe 事件。</span><span class="sxs-lookup"><span data-stu-id="d65e7-104">Defines an EventPipe event on an existing provider.</span></span> <span data-ttu-id="d65e7-105">此提供程序可用于编写其他侦听器可以接收的 EventPipe 事件。</span><span class="sxs-lookup"><span data-stu-id="d65e7-105">This provider can be used to write EventPipe events that other listeners can receive.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="d65e7-106">语法</span><span class="sxs-lookup"><span data-stu-id="d65e7-106">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeDefineEvent(
                [in] EVENTPIPE_PROVIDER     provider,
                [in, string] const WCHAR   *eventName,
                [in] UINT32                 eventID,
                [in] UINT64                 keywords,
                [in] UINT32                 eventVersion,
                [in] UINT32                 level,
                [in] UINT8                  opcode,
                [in] BOOL                   needStack,
                [in] UINT32                 cParamDescs,
                [in, size_is(cParamDescs)]
                     COR_PRF_EVENTPIPE_PARAM_DESC pParamDescs[],
                [out] EVENTPIPE_EVENT      *pEvent);
```  
  
## <a name="parameters"></a><span data-ttu-id="d65e7-107">参数</span><span class="sxs-lookup"><span data-stu-id="d65e7-107">Parameters</span></span>

<span data-ttu-id="d65e7-108">`provider` 中要在其上定义事件的提供程序的 ID。</span><span class="sxs-lookup"><span data-stu-id="d65e7-108">`provider` [in] The ID of the provider to define an event on.</span></span>

<span data-ttu-id="d65e7-109">`eventName` 中指向以 null 结尾的包含事件名称的宽字符字符串的指针。</span><span class="sxs-lookup"><span data-stu-id="d65e7-109">`eventName` [in] A pointer to a null terminated wide character string that contains the event name.</span></span>

<span data-ttu-id="d65e7-110">`eventID` 中正在定义的事件的 ID。</span><span class="sxs-lookup"><span data-stu-id="d65e7-110">`eventID` [in] The ID of the event being defined.</span></span>

<span data-ttu-id="d65e7-111">`keywords` 中正在定义的事件的关键字。</span><span class="sxs-lookup"><span data-stu-id="d65e7-111">`keywords` [in] The keywords of the event being defined.</span></span>

<span data-ttu-id="d65e7-112">`eventVersion` 中正在定义的事件的版本。</span><span class="sxs-lookup"><span data-stu-id="d65e7-112">`eventVersion` [in] The version of the event being defined.</span></span>

<span data-ttu-id="d65e7-113">`level` 中正在定义的事件级别。</span><span class="sxs-lookup"><span data-stu-id="d65e7-113">`level` [in] The level of the event being defined.</span></span>

<span data-ttu-id="d65e7-114">`opcode` 中正在定义的事件的操作码。</span><span class="sxs-lookup"><span data-stu-id="d65e7-114">`opcode` [in] The opcode of the event being defined.</span></span>

<span data-ttu-id="d65e7-115">`needStack` 中一个 `BOOL` ，该值指示是否应在每次激发此事件时收集托管堆栈。</span><span class="sxs-lookup"><span data-stu-id="d65e7-115">`needStack` [in] A `BOOL` indicating whether managed stacks should be collected each time this event fires.</span></span>

<span data-ttu-id="d65e7-116">`cParamDescs` 中中的参数数目的计数 `pParamDescs` 。</span><span class="sxs-lookup"><span data-stu-id="d65e7-116">`cParamDescs` [in] The count of the number of parameters in `pParamDescs`.</span></span>

<span data-ttu-id="d65e7-117">`pParamDescs` 中一个数组， `COR_PRF_EVENTPIPE_PARAM_DESC` 该数组定义要定义的事件的参数类型。</span><span class="sxs-lookup"><span data-stu-id="d65e7-117">`pParamDescs` [in] An array of `COR_PRF_EVENTPIPE_PARAM_DESC` defining the parameter types to the event being defined.</span></span>

<span data-ttu-id="d65e7-118">`pEvent` 弄调用方提供的指针，它将使用函数返回时所定义的事件 ID 进行填充。</span><span class="sxs-lookup"><span data-stu-id="d65e7-118">`pEvent` [out] A caller provided pointer that will be filled with the ID of the event being defined when the function returns.</span></span>

## <a name="requirements"></a><span data-ttu-id="d65e7-119">要求</span><span class="sxs-lookup"><span data-stu-id="d65e7-119">Requirements</span></span>  

<span data-ttu-id="d65e7-120">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="d65e7-120">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="d65e7-121">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="d65e7-121">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="d65e7-122">另请参阅</span><span class="sxs-lookup"><span data-stu-id="d65e7-122">See also</span></span>

- [<span data-ttu-id="d65e7-123">EventPipe 概述</span><span class="sxs-lookup"><span data-stu-id="d65e7-123">EventPipe Overview</span></span>](../../../core/diagnostics/eventpipe.md)
- [<span data-ttu-id="d65e7-124">众所周知的 EventProviders</span><span class="sxs-lookup"><span data-stu-id="d65e7-124">Well Known EventProviders</span></span>](../../../core/diagnostics/well-known-event-providers.md)
- [<span data-ttu-id="d65e7-125">分析接口</span><span class="sxs-lookup"><span data-stu-id="d65e7-125">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="d65e7-126">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="d65e7-126">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="d65e7-127">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="d65e7-127">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="d65e7-128">COR_PRF_EVENTPIPE_PARAM_DESC 结构</span><span class="sxs-lookup"><span data-stu-id="d65e7-128">COR_PRF_EVENTPIPE_PARAM_DESC Structure</span></span>](cor-prf-eventpipe-param-desc-structure.md)
