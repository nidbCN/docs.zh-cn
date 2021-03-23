---
description: 了解详细信息： COR_PRF_EVENTPIPE_PARAM_TYPE 枚举
title: COR_PRF_EVENTPIPE_PARAM_TYPE 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_PARAM_TYPE
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 29aed86e4a991598d038a60ae086e77e25df24bb
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805774"
---
# <a name="cor_prf_eventpipe_param_type-enumeration"></a>COR_PRF_EVENTPIPE_PARAM_TYPE 枚举

描述 EventPipe 事件的参数类型。
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef enum
{
    COR_PRF_EVENTPIPE_OBJECT = 1,
    COR_PRF_EVENTPIPE_BOOLEAN = 3,
    COR_PRF_EVENTPIPE_CHAR = 4,
    COR_PRF_EVENTPIPE_SBYTE = 5,
    COR_PRF_EVENTPIPE_BYTE = 6,
    COR_PRF_EVENTPIPE_INT16 = 7,
    COR_PRF_EVENTPIPE_UINT16 = 8,
    COR_PRF_EVENTPIPE_INT32 = 9,
    COR_PRF_EVENTPIPE_UINT32 = 10,
    COR_PRF_EVENTPIPE_INT64 = 11,
    COR_PRF_EVENTPIPE_UINT64 = 12,
    COR_PRF_EVENTPIPE_SINGLE = 13,
    COR_PRF_EVENTPIPE_DOUBLE = 14,
    COR_PRF_EVENTPIPE_DECIMAL = 15,
    COR_PRF_EVENTPIPE_DATETIME = 16,
    COR_PRF_EVENTPIPE_GUID = 17,
    COR_PRF_EVENTPIPE_STRING = 18,
    COR_PRF_EVENTPIPE_ARRAY = 19,
} COR_PRF_EVENTPIPE_PARAM_TYPE;
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`COR_PRF_EVENTPIPE_OBJECT`|参数类型是一个自描述对象。|
|`COR_PRF_EVENTPIPE_BOOLEAN`|参数类型为布尔值。|
|`COR_PRF_EVENTPIPE_CHAR`|参数类型为16位宽字符。|
|`COR_PRF_EVENTPIPE_SBYTE`|参数类型是有符号的8位整数。|
|`COR_PRF_EVENTPIPE_BYTE`|参数类型是一个无符号的8位整数。|
|`COR_PRF_EVENTPIPE_INT16`|参数类型为带符号的16位整数。|
|`COR_PRF_EVENTPIPE_UINT16`|参数类型为16位无符号整数。|
|`COR_PRF_EVENTPIPE_INT32`|参数类型是有符号的32位整数。|
|`COR_PRF_EVENTPIPE_UINT32`|参数类型是一个无符号的32位整数。|
|`COR_PRF_EVENTPIPE_INT64`|参数类型是有符号的64位整数。|
|`COR_PRF_EVENTPIPE_UINT64`|参数类型是一个无符号的64位整数。|
|`COR_PRF_EVENTPIPE_SINGLE`|参数类型是一个32位浮点数字。|
|`COR_PRF_EVENTPIPE_DOUBLE`|参数类型是一个64位浮点数字。|
|`COR_PRF_EVENTPIPE_DECIMAL`|参数类型是一个128位浮点数字。|
|`COR_PRF_EVENTPIPE_DATETIME`|参数类型是序列化的 DataTime 结构。|
|`COR_PRF_EVENTPIPE_GUID`|参数类型为 GUID。|
|`COR_PRF_EVENTPIPE_STRING`|参数类型是一个16位 null 的宽字符字符串。|
|`COR_PRF_EVENTPIPE_ARRAY`|参数类型是上述类型之一的数组。|
  
## <a name="remarks"></a>备注  

 `COR_PRF_EVENTPIPE_PARAM_TYPE` [COR_PRF_EVENTPIPE_PARAM_DESC](cor-prf-eventpipe-param-desc-structure.md)结构使用枚举来指示参数的类型。
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
