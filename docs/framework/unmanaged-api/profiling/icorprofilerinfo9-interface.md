---
description: 了解详细信息： ICorProfilerInfo9 接口
title: ICorProfilerInfo9 接口
ms.date: 08/06/2019
author: davmason
ms.author: davmason
ms.openlocfilehash: 44e3d694b426f87ee4e4bc12181f46322b0d246f
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805670"
---
# <a name="icorprofilerinfo9-interface"></a>ICorProfilerInfo9 接口

[ICorProfilerInfo8](icorprofilerinfo8-interface.md)的子类，提供用于查询有关具有多个本机代码版本的函数的信息的方法。  

## <a name="methods"></a>方法  

| 方法|说明|  
| ------------|-----------------|  
|[GetNativeCodeStartAddresses 方法](icorprofilerinfo9-getnativecodestartaddresses-method.md)| 给定一个 functionId 和 rejitId，枚举此代码当前存在的所有实时编译版本的本机代码起始地址。 |
|[GetILToNativeMapping3 方法](icorprofilerinfo9-getiltonativemapping3-method.md)| 给定本机代码起始地址，返回此实时编译版本的代码的本机到 IL 映射信息。 |
|[GetCodeInfo4 方法](icorprofilerinfo9-getcodeinfo4-method.md)| 给定本机代码起始地址，返回存储此代码的虚拟内存块。 |

## <a name="requirements"></a>要求  

**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。  
**头文件：** CorProf.idl、CorProf.h  
**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-21-md.md)]  

## <a name="see-also"></a>另请参阅

- [分析接口](profiling-interfaces.md)
