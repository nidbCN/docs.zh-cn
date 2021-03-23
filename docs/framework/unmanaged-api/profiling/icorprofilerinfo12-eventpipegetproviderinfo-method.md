---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeGetProviderInfo 方法
title: ICorProfilerInfo12：： EventPipeGetProviderInfo 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeGetProviderInfo
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 7bc7ffe1c31e88dc1c65f1670f9bd179e732abfe
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805696"
---
# <a name="icorprofilerinfo12eventpipegetproviderinfo-method"></a>ICorProfilerInfo12：： EventPipeGetProviderInfo 方法

创建一个 EventPipe 提供程序，探查器可以使用该提供程序为要接收的其他 EventPipe 侦听器编写事件。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT EventPipeGetProviderInfo(
                [in] EVENTPIPE_PROVIDER provider,
                [in]  ULONG      cchName,
                [out] ULONG      *pcchName,
                [out, annotation("_Out_writes_to_(cchName, *pcchName)")]
                      WCHAR      providerName[]);
```  
  
## <a name="parameters"></a>参数

`provider` 中要为其提供名称的提供程序的 ID。

`cchName` 中的大小（以字符为字符） `providerName` 。

`pcchName` 弄指向的总字符长度的指针 `providerName` 。

`providerName` 弄调用方提供宽字符缓冲区。 当函数返回时，缓冲区将包含提供程序的名称。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [EventPipe 概述](../../../core/diagnostics/eventpipe.md)
- [众所周知的 EventProviders](../../../core/diagnostics/well-known-event-providers.md)
- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
