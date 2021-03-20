---
description: 了解详细信息： ICorProfilerCallback：： JITCachedFunctionSearchFinished 方法
title: ICorProfilerCallback::JITCachedFunctionSearchFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCachedFunctionSearchFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCachedFunctionSearchFinished
helpviewer_keywords:
- JITCachedFunctionSearchFinished method [.NET Framework profiling]
- ICorProfilerCallback::JITCachedFunctionSearchFinished method [.NET Framework profiling]
ms.assetid: 3c325c82-cddd-4b00-b3da-e450c36abf62
topic_type:
- apiref
ms.openlocfilehash: 3dc655c2a049a2cac08f6e4856aab98ee690b9df
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759952"
---
# <a name="icorprofilercallbackjitcachedfunctionsearchfinished-method"></a><span data-ttu-id="1c2de-103">ICorProfilerCallback::JITCachedFunctionSearchFinished 方法</span><span class="sxs-lookup"><span data-stu-id="1c2de-103">ICorProfilerCallback::JITCachedFunctionSearchFinished Method</span></span>

<span data-ttu-id="1c2de-104">通知探查器搜索已完成先前使用本机映像生成器编译的函数)  (NGen.exe。</span><span class="sxs-lookup"><span data-stu-id="1c2de-104">Notifies the profiler that a search has finished for a function that was compiled previously using the Native Image Generator (NGen.exe).</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="1c2de-105">语法</span><span class="sxs-lookup"><span data-stu-id="1c2de-105">Syntax</span></span>  
  
```cpp  
HRESULT JITCachedFunctionSearchFinished(  
    [in] FunctionID        functionId,  
    [in] COR_PRF_JIT_CACHE result);  
```  
  
## <a name="parameters"></a><span data-ttu-id="1c2de-106">parameters</span><span class="sxs-lookup"><span data-stu-id="1c2de-106">Parameters</span></span>

<span data-ttu-id="1c2de-107">`functionId` 中执行搜索的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c2de-107">`functionId` [in] The ID of the function for which the search was performed.</span></span>

<span data-ttu-id="1c2de-108">`result` 中指示搜索结果的 [COR_PRF_JIT_CACHE](cor-prf-jit-cache-enumeration.md) 枚举的值。</span><span class="sxs-lookup"><span data-stu-id="1c2de-108">`result` [in] A value of the [COR_PRF_JIT_CACHE](cor-prf-jit-cache-enumeration.md) enumeration that indicates the result of the search.</span></span>

## <a name="remarks"></a><span data-ttu-id="1c2de-109">备注</span><span class="sxs-lookup"><span data-stu-id="1c2de-109">Remarks</span></span>  

 <span data-ttu-id="1c2de-110">在 .NET Framework 版本2.0 中，将不会对常规 NGen 映像中的所有函数执行 [ICorProfilerCallback：： JITCachedFunctionSearchStarted](icorprofilercallback-jitcachedfunctionsearchstarted-method.md) 和 `JITCachedFunctionSearchFinished` 回调。</span><span class="sxs-lookup"><span data-stu-id="1c2de-110">In the .NET Framework version 2.0, the [ICorProfilerCallback::JITCachedFunctionSearchStarted](icorprofilercallback-jitcachedfunctionsearchstarted-method.md) and `JITCachedFunctionSearchFinished` callbacks will not be made for all functions in regular NGen images.</span></span> <span data-ttu-id="1c2de-111">只有针对探查器优化的 NGen 映像将为映像中的所有函数生成回调。</span><span class="sxs-lookup"><span data-stu-id="1c2de-111">Only NGen images optimized for a profiler will generate callbacks for all functions in the image.</span></span> <span data-ttu-id="1c2de-112">但是，由于额外的开销，如果探查器打算使用这些回调来强制将函数实时编译 (JIT) ，则它应请求探查器优化的 NGen 映像。</span><span class="sxs-lookup"><span data-stu-id="1c2de-112">However, due to the additional overhead, a profiler should request profiler-optimized NGen images only if it intends to use these callbacks to force a function to be compiled just-in-time (JIT).</span></span> <span data-ttu-id="1c2de-113">否则，探查器应使用延迟策略来收集函数信息。</span><span class="sxs-lookup"><span data-stu-id="1c2de-113">Otherwise, the profiler should use a lazy strategy for gathering function information.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="1c2de-114">要求</span><span class="sxs-lookup"><span data-stu-id="1c2de-114">Requirements</span></span>  

 <span data-ttu-id="1c2de-115">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="1c2de-115">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="1c2de-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="1c2de-116">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="1c2de-117">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="1c2de-117">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="1c2de-118">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="1c2de-118">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="1c2de-119">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1c2de-119">See also</span></span>

- [<span data-ttu-id="1c2de-120">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="1c2de-120">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
