---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeStopSession 方法
title: ICorProfilerInfo12：： EventPipeStopSession 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeStopSession
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: c1b104f63755bcec52a113be7e2aa9af76ecd34e
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805709"
---
# <a name="icorprofilerinfo12eventpipestopsession-method"></a><span data-ttu-id="0d507-103">ICorProfilerInfo12：： EventPipeStopSession 方法</span><span class="sxs-lookup"><span data-stu-id="0d507-103">ICorProfilerInfo12::EventPipeStopSession Method</span></span>

<span data-ttu-id="0d507-104">停止 EventPipe 会话，阻止将任何将来的事件写入到会话中。</span><span class="sxs-lookup"><span data-stu-id="0d507-104">Stops an EventPipe session, preventing any future events from being written to the session.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="0d507-105">语法</span><span class="sxs-lookup"><span data-stu-id="0d507-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeStopSession(
        [in] EVENTPIPE_SESSION session);
```  
  
## <a name="parameters"></a><span data-ttu-id="0d507-106">参数</span><span class="sxs-lookup"><span data-stu-id="0d507-106">Parameters</span></span>

<span data-ttu-id="0d507-107">`session` 中要停止的会话的 ID。</span><span class="sxs-lookup"><span data-stu-id="0d507-107">`session` [in] The ID of the session to stop.</span></span>

## <a name="requirements"></a><span data-ttu-id="0d507-108">要求</span><span class="sxs-lookup"><span data-stu-id="0d507-108">Requirements</span></span>  

<span data-ttu-id="0d507-109">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="0d507-109">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="0d507-110">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="0d507-110">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="0d507-111">另请参阅</span><span class="sxs-lookup"><span data-stu-id="0d507-111">See also</span></span>

- [<span data-ttu-id="0d507-112">分析接口</span><span class="sxs-lookup"><span data-stu-id="0d507-112">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="0d507-113">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="0d507-113">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="0d507-114">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="0d507-114">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
