---
description: 了解详细信息： ICorProfilerCallback10 接口
title: ICorProfilerCallback10 接口
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: d9091dc81307e2dcb83e74154a8217dffaa1e7a2
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805644"
---
# <a name="icorprofilercallback10-interface"></a>ICorProfilerCallback10 接口

 [ICorProfilerCallback9](icorprofilercallback9-interface.md)的子类，提供回调方法，以通知探查器 EventPipe 事件已传递到探查器的当前活动会话。
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EventPipeEventDelivered 方法](icorprofilercallback10-eventpipeeventdelivered-method.md)|通知探查器 EventPipe 事件已传递到探查器已打开的会话。|
|[EventPipeProviderCreated 方法](icorprofilercallback10-eventpipeprovidercreated-method.md)|通知探查器已创建 EventPipe 提供程序，从而允许探查器将该提供程序添加到其会话中。|  
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]  

## <a name="see-also"></a>另请参阅

- [EventPipe 概述](../../../core/diagnostics/eventpipe.md)
- [众所周知的 EventProviders](../../../core/diagnostics/well-known-event-providers.md)
- [分析接口](profiling-interfaces.md)
- [ICorProfilerInfo12 接口](ICorProfilerInfo12-interface.md)
- [ICorProfilerInfo12. EventPipeStartSession 方法](ICorProfilerInfo12-eventpipestartsession-method.md)
- [ICorProfilerInfo12. EventPipeStopSession 方法](ICorProfilerInfo12-eventpipestopsession-method.md)
