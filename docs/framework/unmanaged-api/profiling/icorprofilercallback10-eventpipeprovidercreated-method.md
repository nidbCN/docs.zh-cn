---
description: 了解详细信息： ICorProfilerCallback10：： EventPipeProviderCreated 方法
title: ICorProfilerCallback10：： EventPipeProviderCreated 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10.EventPipeProviderCreated
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 4602438148a369f2a2321a2ec2e959cc375e37cf
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805579"
---
# <a name="icorprofilercallback10eventpipeprovidercreated-method"></a>ICorProfilerCallback10：： EventPipeProviderCreated 方法

每当创建 EventPipe 提供程序时，通知探查器。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT EventPipeProviderCreated([in] EVENTPIPE_PROVIDER provider); 
```  
  
## <a name="parameters"></a>参数

`provider` 中已创建的提供程序。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
- [ICorProfilerInfo12. EventPipeAddProviderToSession 方法](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)
