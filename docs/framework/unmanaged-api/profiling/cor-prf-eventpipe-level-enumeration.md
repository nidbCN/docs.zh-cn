---
description: 了解详细信息： COR_PRF_EVENTPIPE_LEVEL 枚举
title: COR_PRF_EVENTPIPE_LEVEL 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_LEVEL
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: da8211da1a9bb6262b078c5e56bee1d0c1f9342e
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805813"
---
# <a name="cor_prf_eventpipe_level-enumeration"></a>COR_PRF_EVENTPIPE_LEVEL 枚举

描述 EventPipe 事件的级别。
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum
{
    COR_PRF_EVENTPIPE_LOGALWAYS = 0,
    COR_PRF_EVENTPIPE_CRITICAL = 1,
    COR_PRF_EVENTPIPE_ERROR = 2,
    COR_PRF_EVENTPIPE_WARNING = 3,
    COR_PRF_EVENTPIPE_INFORMATIONAL = 4,
    COR_PRF_EVENTPIPE_VERBOSE = 5
} COR_PRF_EVENTPIPE_LEVEL;
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`COR_PRF_EVENTPIPE_LOGALWAYS`|始终记录事件。|
|`COR_PRF_EVENTPIPE_CRITICAL`|事件表示关键消息。|
|`COR_PRF_EVENTPIPE_ERROR`|该事件表示一条错误消息。|
|`COR_PRF_EVENTPIPE_WARNING`|该事件表示一条警告消息。|
|`COR_PRF_EVENTPIPE_INFORMATIONAL`|该事件表示信息性消息。|
|`COR_PRF_EVENTPIPE_VERBOSE`|该事件表示详细消息。|
  
## <a name="remarks"></a>备注  

 `COR_PRF_EVENTPIPE_LEVEL` [ICorProfilerInfo12：： EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md)方法使用枚举来指示要定义的事件级别。
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
- [ICorProfilerInfo12::EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md)
