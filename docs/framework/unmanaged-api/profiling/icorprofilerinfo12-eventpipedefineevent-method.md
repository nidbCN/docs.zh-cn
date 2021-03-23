---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeDefineEvent 方法
title: ICorProfilerInfo12：： EventPipeDefineEvent 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeDefineEvent
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 845f7f26dce7aa0f4b7d895b9f04cc89e7ac7192
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805748"
---
# <a name="icorprofilerinfo12eventpipedefineevent-method"></a>ICorProfilerInfo12：： EventPipeDefineEvent 方法

定义现有提供程序上的 EventPipe 事件。 此提供程序可用于编写其他侦听器可以接收的 EventPipe 事件。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT EventPipeDefineEvent(
                [in] EVENTPIPE_PROVIDER     provider,
                [in, string] const WCHAR   *eventName,
                [in] UINT32                 eventID,
                [in] UINT64                 keywords,
                [in] UINT32                 eventVersion,
                [in] UINT32                 level,
                [in] UINT8                  opcode,
                [in] BOOL                   needStack,
                [in] UINT32                 cParamDescs,
                [in, size_is(cParamDescs)]
                     COR_PRF_EVENTPIPE_PARAM_DESC pParamDescs[],
                [out] EVENTPIPE_EVENT      *pEvent);
```  
  
## <a name="parameters"></a>参数

`provider` 中要在其上定义事件的提供程序的 ID。

`eventName` 中指向以 null 结尾的包含事件名称的宽字符字符串的指针。

`eventID` 中正在定义的事件的 ID。

`keywords` 中正在定义的事件的关键字。

`eventVersion` 中正在定义的事件的版本。

`level` 中正在定义的事件级别。

`opcode` 中正在定义的事件的操作码。

`needStack` 中一个 `BOOL` ，该值指示是否应在每次激发此事件时收集托管堆栈。

`cParamDescs` 中中的参数数目的计数 `pParamDescs` 。

`pParamDescs` 中一个数组， `COR_PRF_EVENTPIPE_PARAM_DESC` 该数组定义要定义的事件的参数类型。

`pEvent` 弄调用方提供的指针，它将使用函数返回时所定义的事件 ID 进行填充。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。
**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]
  
## <a name="see-also"></a>另请参阅

- [EventPipe 概述](../../../core/diagnostics/eventpipe.md)
- [众所周知的 EventProviders](../../../core/diagnostics/well-known-event-providers.md)
- [分析接口](profiling-interfaces.md)
- [ICorProfilerCallback10 接口](icorprofilercallback10-interface.md)
- [ICorProfilerInfo12 接口](icorprofilerinfo12-interface.md)
- [COR_PRF_EVENTPIPE_PARAM_DESC 结构](cor-prf-eventpipe-param-desc-structure.md)
