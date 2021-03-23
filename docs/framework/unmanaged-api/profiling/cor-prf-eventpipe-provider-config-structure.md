---
description: 了解详细信息： COR_PRF_EVENTPIPE_PROVIDER_CONFIG 枚举
title: COR_PRF_EVENTPIPE_PROVIDER_CONFIG 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_PROVIDER_CONFIG
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 2a7a5e720e9468b44e6504d24a3b9ad836e13693
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805839"
---
# <a name="cor_prf_eventpipe_provider_config-enumeration"></a>COR_PRF_EVENTPIPE_PROVIDER_CONFIG 枚举

描述配置 EventPipe 提供程序所必需的字段。
  
## <a name="syntax"></a>语法  
  
```cpp  
typedef struct
{
    const WCHAR* providerName;
    UINT64       keywords;
    UINT32       loggingLevel;
    const WCHAR* filterData;
} COR_PRF_EVENTPIPE_PROVIDER_CONFIG;
```  
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`providerName`|要启用的提供程序的名称。|  
|`keywords`|要在提供程序上启用的关键字。|  
|`loggingLevel`|要对提供程序启用的级别。|  
|`filterData`|包含在启用提供程序时要使用的 filterdata 的宽字符字符串。|  
  
## <a name="remarks"></a>备注  

 `COR_PRF_EVENTPIPE_PROVIDER_CONFIG` [ICorProfilerInfo12：： EventPipeAddProviderToSession](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)方法使用结构来指示要添加到会话中的提供程序的配置。
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [分析枚举](profiling-enumerations.md)
- [ICorProfilerInfo12::EventPipeAddProviderToSession](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)
