---
description: 了解详细信息： ICorProfilerCallback：： ExceptionSearchCatcherFound 方法
title: ICorProfilerCallback::ExceptionSearchCatcherFound 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionSearchCatcherFound
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionSearchCatcherFound
helpviewer_keywords:
- ExceptionSearchCatcherFound method [.NET Framework profiling]
- ICorProfilerCallback::ExceptionSearchCatcherFound method [.NET Framework profiling]
ms.assetid: 190f424d-5e37-4163-a191-0895686e9476
topic_type:
- apiref
ms.openlocfilehash: 080e9be61e4f10cbfb60696a5089aca0c3f2843f
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759984"
---
# <a name="icorprofilercallbackexceptionsearchcatcherfound-method"></a><span data-ttu-id="6e5c9-103">ICorProfilerCallback::ExceptionSearchCatcherFound 方法</span><span class="sxs-lookup"><span data-stu-id="6e5c9-103">ICorProfilerCallback::ExceptionSearchCatcherFound Method</span></span>

<span data-ttu-id="6e5c9-104">通知探查器，异常处理的搜索阶段已找到引发的异常的处理程序。</span><span class="sxs-lookup"><span data-stu-id="6e5c9-104">Notifies the profiler that the search phase of exception handling has located a handler for the exception that was thrown.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="6e5c9-105">语法</span><span class="sxs-lookup"><span data-stu-id="6e5c9-105">Syntax</span></span>  
  
```cpp  
RESULT ExceptionSearchCatcherFound(  
    [in] FunctionID functionId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="6e5c9-106">parameters</span><span class="sxs-lookup"><span data-stu-id="6e5c9-106">Parameters</span></span>

<span data-ttu-id="6e5c9-107">`functionId` 中包含异常处理程序的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="6e5c9-107">`functionId` [in] The ID of the function that contains the exception handler.</span></span>

## <a name="requirements"></a><span data-ttu-id="6e5c9-108">要求</span><span class="sxs-lookup"><span data-stu-id="6e5c9-108">Requirements</span></span>  

 <span data-ttu-id="6e5c9-109">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="6e5c9-109">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="6e5c9-110">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="6e5c9-110">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="6e5c9-111">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="6e5c9-111">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="6e5c9-112">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="6e5c9-112">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="6e5c9-113">另请参阅</span><span class="sxs-lookup"><span data-stu-id="6e5c9-113">See also</span></span>

- [<span data-ttu-id="6e5c9-114">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="6e5c9-114">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
