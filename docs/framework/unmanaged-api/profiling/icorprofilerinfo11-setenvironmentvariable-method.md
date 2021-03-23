---
description: 了解详细信息： ICorProfilerInfo11：： SetEnvironmentVariable 方法
title: ICorProfilerInfo11：： SetEnvironmentVariable 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo11.SetEnvironmentVariable
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 77dc3fc992dec037881573323822dc11481a56be
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805594"
---
# <a name="icorprofilerinfo11setenvironmentvariable-method"></a>ICorProfilerInfo11：： SetEnvironmentVariable 方法

设置进程中的环境变量。 在非 Windows 平台上，运行时将保留环境变量的内部缓存，以确保线程安全。 这意味着，如果探查器调用 `setenv` 新的环境变量，则在进程中运行的托管代码将不会选取。
  
## <a name="syntax"></a>语法  
  
```cpp  
    HRESULT SetEnvironmentVariable(
                [in, string] const WCHAR *szName,
                [in, string] const WCHAR *szValue);
```  
  
## <a name="parameters"></a>参数

`szName` 中指向以 null 结尾的宽字符字符串的指针，该字符串包含要设置的环境变量的名称。

`szValue` 中指向以 null 结尾的宽字符字符串的指针，该字符串包含要设置的环境变量的值。

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-31-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
- [ICorProfilerInfo11 接口](icorprofilerinfo11-interface.md)
