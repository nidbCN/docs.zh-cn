---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeAddProviderToSession 方法
title: ICorProfilerInfo12：： EventPipeAddProviderToSession 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeAddProviderToSession
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 70654660c496211c20459ef9ba37dfbd96b829b3
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805683"
---
# <a name="icorprofilerinfo12eventpipeaddprovidertosession-method"></a>ICorProfilerInfo12：： EventPipeAddProviderToSession 方法

向现有的 EventPipe 会话添加提供程序。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT EventPipeAddProviderToSession(
        [in] EVENTPIPE_SESSION                 session,
        [in] COR_PRF_EVENTPIPE_PROVIDER_CONFIG providerConfig);
```  
  
## <a name="parameters"></a>参数

`session` 中要将提供程序添加到的会话的 ID。

`providerConfig` 中 `COR_PRF_EVENTPIPE_PROVIDER_CONFIG` 描述要添加到会话中的提供程序的。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [EventPipe 概述](../../../core/diagnostics/eventpipe.md)
- [众所周知的 EventProviders](../../../core/diagnostics/well-known-event-providers.md)
- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
- [COR_PRF_EVENTPIPE_PROVIDER_CONFIG 结构](cor-prf-eventpipe-provider-config-structure.md)
