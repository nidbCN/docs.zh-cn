---
description: 了解详细信息： ICorProfilerCallback：： FunctionUnloadStarted 方法
title: ICorProfilerCallback::FunctionUnloadStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.FunctionUnloadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::FunctionUnloadStarted
helpviewer_keywords:
- FunctionUnloadStarted method [.NET Framework profiling]
- ICorProfilerCallback::FunctionUnloadStarted method [.NET Framework profiling]
ms.assetid: d6a5fa8b-09c6-47a5-b60e-6cf2e355df30
topic_type:
- apiref
ms.openlocfilehash: ce3cf406b8d2f91613bce878db8a4f9e0838c052
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760426"
---
# <a name="icorprofilercallbackfunctionunloadstarted-method"></a><span data-ttu-id="a7087-103">ICorProfilerCallback::FunctionUnloadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="a7087-103">ICorProfilerCallback::FunctionUnloadStarted Method</span></span>

<span data-ttu-id="a7087-104">通知探查器运行时已开始卸载某个函数。</span><span class="sxs-lookup"><span data-stu-id="a7087-104">Notifies the profiler that the runtime has started to unload a function.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="a7087-105">语法</span><span class="sxs-lookup"><span data-stu-id="a7087-105">Syntax</span></span>  
  
```cpp  
HRESULT FunctionUnloadStarted(  
    [in] FunctionID functionId);
```  
  
## <a name="parameters"></a><span data-ttu-id="a7087-106">parameters</span><span class="sxs-lookup"><span data-stu-id="a7087-106">Parameters</span></span>

<span data-ttu-id="a7087-107">`functionId` 中正在卸载的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="a7087-107">`functionId` [in] The ID of the function that is being unloaded.</span></span>

## <a name="remarks"></a><span data-ttu-id="a7087-108">备注</span><span class="sxs-lookup"><span data-stu-id="a7087-108">Remarks</span></span>  

 <span data-ttu-id="a7087-109">`functionId`此方法返回到调用方后，该参数的值不再有效。</span><span class="sxs-lookup"><span data-stu-id="a7087-109">The value of the `functionId` parameter is no longer valid after this method returns to the caller.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="a7087-110">要求</span><span class="sxs-lookup"><span data-stu-id="a7087-110">Requirements</span></span>  

 <span data-ttu-id="a7087-111">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="a7087-111">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="a7087-112">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="a7087-112">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="a7087-113">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="a7087-113">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="a7087-114">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="a7087-114">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="a7087-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="a7087-115">See also</span></span>

- [<span data-ttu-id="a7087-116">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="a7087-116">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
