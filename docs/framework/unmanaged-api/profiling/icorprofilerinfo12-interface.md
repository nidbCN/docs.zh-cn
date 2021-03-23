---
description: 了解详细信息： ICorProfilerInfo12 接口
title: ICorProfilerInfo12 接口
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: a8b91eba686478c3e825ae15d6ae95bd0ffad944
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805722"
---
# <a name="icorprofilerinfo12-interface"></a>ICorProfilerInfo12 接口

 [ICorProfilerInfo11](icorprofilerinfo11-interface.md)的子类，提供创建 EventPipe 会话、事件和提供程序的方法。
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|------------|-----------------|  
|[EventPipeStartSession 方法](icorprofilerinfo12-eventpipestartsession-method.md)|启动探查器 EventPipe 会话。|
|[EventPipeAddProviderToSession 方法](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)|向现有的 EventPipe 会话添加提供程序。|
|[EventPipeStopSession 方法](icorprofilerinfo12-eventpipestopsession-method.md)|停止 EventPipe 会话。|
|[EventPipeCreateProvider 方法](icorprofilerinfo12-eventpipecreateprovider-method.md)|创建 EventPipe 提供程序。|  
|[EventPipeGetProviderInfo 方法](icorprofilerinfo12-eventpipegetproviderinfo-method.md)|从其 ID 获取 EventPipe 提供程序的名称。|
|[EventPipeDefineEvent 方法](icorprofilerinfo12-eventpipedefineevent-method.md)|定义现有 EventPipe 提供程序上的事件。|  
|[EventPipeWriteEvent 方法](icorprofilerinfo12-eventpipewriteevent-method.md)|写入 EventPipe 事件。|
  
## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]  

## <a name="see-also"></a>另请参阅

- [EventPipe 概述](../../../core/diagnostics/eventpipe.md)
- [众所周知的 EventProviders](../../../core/diagnostics/well-known-event-providers.md)
- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
