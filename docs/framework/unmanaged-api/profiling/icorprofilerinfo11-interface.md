---
description: 了解详细信息： ICorProfilerInfo11 接口
title: ICorProfilerInfo11 接口
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo11
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 083714b67d83291f1faada3f673df13f16ef3856
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805618"
---
# <a name="icorprofilerinfo11-interface"></a>ICorProfilerInfo11 接口

 [ICorProfilerInfo10](icorprofilerinfo10-interface.md)的子类，提供用于获取和设置进程中的环境变量的方法。
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[GetEnvironmentVariable 方法](icorprofilerinfo11-getenvironmentvariable-method.md)|获取环境变量。|
|[SetEnvironmentVariable 方法](icorprofilerinfo11-setenvironmentvariable-method.md)|设置环境变量。|  
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]  

## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
