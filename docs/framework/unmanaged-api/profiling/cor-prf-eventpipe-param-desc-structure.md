---
description: 了解详细信息： COR_PRF_EVENTPIPE_PARAM_DESC 枚举
title: COR_PRF_EVENTPIPE_PARAM_DESC 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_PARAM_DESC
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: b23897580092cc9f3f887a21e86f7111fecd9f19
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805787"
---
# <a name="cor_prf_eventpipe_param_desc-enumeration"></a>COR_PRF_EVENTPIPE_PARAM_DESC 枚举

描述 EventPipe 事件的参数名称和类型。
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct
{
    UINT32       type;
    UINT32       elementType;
    const WCHAR *name;
} COR_PRF_EVENTPIPE_PARAM_DESC;
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`type`|参数的类型。|  
|`elementType`|如果此参数是数组类型，则为元素类型。|  
|`name`|包含参数名称的宽字符字符串。|  
  
## <a name="remarks"></a>备注  

 `COR_PRF_EVENTPIPE_PARAM_DESC` [ICorProfilerInfo12：： EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md)方法使用结构来定义所定义事件的参数类型。
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
- [ICorProfilerInfo12::EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md)
