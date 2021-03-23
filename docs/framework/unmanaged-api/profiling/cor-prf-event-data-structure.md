---
description: 了解详细信息： COR_PRF_EVENT_DATA 枚举
title: COR_PRF_EVENT_DATA 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENT_DATA
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 477f36476deb9c0d74e263703e36b134c91b5327
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805555"
---
# <a name="cor_prf_event_data-enumeration"></a>COR_PRF_EVENT_DATA 枚举

描述要写入的 EventPipe 事件的事件数据。
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct
{
    UINT64 ptr;
    UINT32 size;
    UINT32 reserved;
} COR_PRF_EVENT_DATA;
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`ptr`|指向数据的指针。|  
|`size`|指向的数据的大小 `ptr` 。|  
|`reserved`|保留实现特定字段。|  
  
## <a name="remarks"></a>备注  

 `COR_PRF_EVENT_DATA` [ICorProfilerInfo12：： EventPipeWriteEvent](icorprofilerinfo12-eventpipewriteevent-method.md)方法使用结构来 providate 要写入的事件的数据负载。
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
- [ICorProfilerInfo12::EventPipeWriteEvent](icorprofilerinfo12-eventpipewriteevent-method.md)
