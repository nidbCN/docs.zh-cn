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
# <a name="icorprofilerinfo12eventpipestopsession-method"></a>ICorProfilerInfo12：： EventPipeStopSession 方法

停止 EventPipe 会话，阻止将任何将来的事件写入到会话中。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT EventPipeStopSession(
        [in] EVENTPIPE_SESSION session);
```  
  
## <a name="parameters"></a>参数

`session` 中要停止的会话的 ID。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
