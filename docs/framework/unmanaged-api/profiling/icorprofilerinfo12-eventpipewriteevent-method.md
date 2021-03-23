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
# <a name="icorprofilerinfo12eventpipewriteevent-method"></a>ICorProfilerInfo12：： EventPipeWriteEvent 方法

将 EventPipe 事件写入已启用此事件的任何侦听器。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT EventPipeWriteEvent(
                [in] EVENTPIPE_EVENT    event,
                [in] UINT32             cData,
                [in, size_is(cData)]
                     COR_PRF_EVENT_DATA data[],
                [in] LPCGUID            pActivityId,
                [in] LPCGUID            pRelatedActivityId);
```  
  
## <a name="parameters"></a>参数

`event` 中要写入的事件的 ID。

`cData` 中中的元素数目 `data` 。

`data` 中 `COR_PRF_EVENT_DATA` 包含事件参数的数组。

`pActivityId` 中指向指定事件的活动 ID 的 GUID 的指针。

`pRelatedActivityId` 中指向指定事件相关活动 ID 的 GUID 的指针。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
- [COR_PRF_EVENT_DATA 结构](cor-prf-event-data-structure.md)
