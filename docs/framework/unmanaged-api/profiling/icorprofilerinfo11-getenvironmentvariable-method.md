---
description: 了解详细信息： ICorProfilerInfo11：： GetEnvironmentVariable 方法
title: ICorProfilerInfo11：： GetEnvironmentVariable 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo11.GetEnvironmentVariable
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 635c5fb67b880c39f15fbc194a51a706d9ef7fe4
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805826"
---
# <a name="icorprofilerinfo11getenvironmentvariable-method"></a>ICorProfilerInfo11：： GetEnvironmentVariable 方法

获取进程中的环境变量。 在非 Windows 平台上，运行时将保留环境变量的内部缓存，以确保线程安全。 这意味着调用 `getenv` 将不会读取启动后进程中运行的托管代码所设置的任何新的或更新的环境变量。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT GetEnvironmentVariable(
                [in, string] const WCHAR *szName,
                [in]         ULONG cchValue,
                [out]        ULONG *pcchValue,
                [out, annotation("_Out_writes_to_(cchValue, *pcchValue)")]
                             WCHAR szValue[]);
```  
  
## <a name="parameters"></a>参数

`szName` 中指向以 null 结尾的宽字符字符串的指针，该字符串包含要获取的环境变量的名称。

`cchValue` 中的长度，以字符为字符 `szValue` 。

`pcchValue` 弄指向的总字符长度的指针 `szValue` 。

`szValue` 弄调用方提供宽字符缓冲区。 当函数返回时，缓冲区将包含环境变量的值。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-31-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerInfo11 接口](icorprofilerinfo11-interface.md)
