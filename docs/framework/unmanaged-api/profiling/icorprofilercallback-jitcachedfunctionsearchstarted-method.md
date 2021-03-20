---
description: 了解详细信息： ICorProfilerCallback：： JITCachedFunctionSearchStarted 方法
title: ICorProfilerCallback::JITCachedFunctionSearchStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.JITCachedFunctionSearchStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::JITCachedFunctionSearchStarted
helpviewer_keywords:
- JITCachedFunctionSearchStarted method [.NET Framework profiling]
- ICorProfilerCallback::JITCachedFunctionSearchStarted method [.NET Framework profiling]
ms.assetid: 5cba642c-0d80-48ee-889d-198c5044d821
topic_type:
- apiref
ms.openlocfilehash: 3f384c1a02da1747b28701d2eba994b56a85c18f
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759945"
---
# <a name="icorprofilercallbackjitcachedfunctionsearchstarted-method"></a><span data-ttu-id="70e2a-103">ICorProfilerCallback::JITCachedFunctionSearchStarted 方法</span><span class="sxs-lookup"><span data-stu-id="70e2a-103">ICorProfilerCallback::JITCachedFunctionSearchStarted Method</span></span>

<span data-ttu-id="70e2a-104">通知探查器搜索已开始使用本机映像生成器编译的函数)  (NGen.exe。</span><span class="sxs-lookup"><span data-stu-id="70e2a-104">Notifies the profiler that a search has started for a function that was compiled previously using the Native Image Generator (NGen.exe).</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="70e2a-105">语法</span><span class="sxs-lookup"><span data-stu-id="70e2a-105">Syntax</span></span>  
  
```cpp  
HRESULT JITCachedFunctionSearchStarted(  
    [in]  FunctionID functionId,  
    [out] BOOL *pbUseCachedFunction);  
```  
  
## <a name="parameters"></a><span data-ttu-id="70e2a-106">parameters</span><span class="sxs-lookup"><span data-stu-id="70e2a-106">Parameters</span></span>

<span data-ttu-id="70e2a-107">`functionId` 中正在为其执行搜索的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="70e2a-107">`functionId` [in] The ID of the function for which the search is being performed.</span></span>

<span data-ttu-id="70e2a-108">`pbUseCachedFunction` [out] `true` 如果执行引擎应使用函数的缓存版本 (如有) ，则为;否则为 `false` 。</span><span class="sxs-lookup"><span data-stu-id="70e2a-108">`pbUseCachedFunction` [out] `true` if the execution engine should use the cached version of a function (if available); otherwise `false`.</span></span> <span data-ttu-id="70e2a-109">如果值为 `false` ，则执行引擎将对函数进行 JIT 编译，而不是使用未进行 JIT 编译的版本。</span><span class="sxs-lookup"><span data-stu-id="70e2a-109">If the value is `false`, the execution engine JIT-compiles the function instead of using a version that is not JIT-compiled.</span></span>

## <a name="remarks"></a><span data-ttu-id="70e2a-110">备注</span><span class="sxs-lookup"><span data-stu-id="70e2a-110">Remarks</span></span>  

 <span data-ttu-id="70e2a-111">在 .NET Framework 版本2.0 中， `JITCachedFunctionSearchStarted` 对于常规 NGen 映像中的所有函数，不会进行和 [ICorProfilerCallback：： JITCachedFunctionSearchFinished 方法](icorprofilercallback-jitcachedfunctionsearchfinished-method.md) 回调。</span><span class="sxs-lookup"><span data-stu-id="70e2a-111">In the .NET Framework version 2.0, the `JITCachedFunctionSearchStarted` and [ICorProfilerCallback::JITCachedFunctionSearchFinished Method](icorprofilercallback-jitcachedfunctionsearchfinished-method.md) callbacks will not be made for all functions in regular NGen images.</span></span> <span data-ttu-id="70e2a-112">只有针对配置文件优化的 NGen 映像将为映像中的所有函数生成回调。</span><span class="sxs-lookup"><span data-stu-id="70e2a-112">Only NGen images optimized for a profile will generate callbacks for all functions in the image.</span></span> <span data-ttu-id="70e2a-113">但是，由于额外的开销，如果探查器打算使用这些回调来强制将函数实时编译 (JIT) ，则它应请求探查器优化的 NGen 映像。</span><span class="sxs-lookup"><span data-stu-id="70e2a-113">However, due to the additional overhead, a profiler should request profiler-optimized NGen images only if it intends to use these callbacks to force a function to be compiled just-in-time (JIT).</span></span> <span data-ttu-id="70e2a-114">否则，探查器应使用延迟策略来收集函数信息。</span><span class="sxs-lookup"><span data-stu-id="70e2a-114">Otherwise, the profiler should use a lazy strategy for gathering function information.</span></span>  
  
 <span data-ttu-id="70e2a-115">探查器必须支持分析的应用程序的多个线程同时调用同一方法的情况。</span><span class="sxs-lookup"><span data-stu-id="70e2a-115">Profilers must support cases where multiple threads of a profiled application are calling the same method simultaneously.</span></span> <span data-ttu-id="70e2a-116">例如，线程 A 调用 `JITCachedFunctionSearchStarted` 和探查器通过将 *pbUseCachedFunction* 设置为 FALSE 来做出响应，以强制 JIT 编译。</span><span class="sxs-lookup"><span data-stu-id="70e2a-116">For example, thread A calls `JITCachedFunctionSearchStarted` and the profiler responds by setting *pbUseCachedFunction* to FALSE to force JIT compilation.</span></span> <span data-ttu-id="70e2a-117">然后，线程 A 调用 [ICorProfilerCallback：： JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md) 和 [ICorProfilerCallback：： JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md)。</span><span class="sxs-lookup"><span data-stu-id="70e2a-117">Thread A then calls [ICorProfilerCallback::JITCompilationStarted](icorprofilercallback-jitcompilationstarted-method.md) and [ICorProfilerCallback::JITCompilationFinished](icorprofilercallback-jitcompilationfinished-method.md).</span></span>  
  
 <span data-ttu-id="70e2a-118">现在，线程 B 调用 `JITCachedFunctionSearchStarted` 同一函数。</span><span class="sxs-lookup"><span data-stu-id="70e2a-118">Now thread B calls `JITCachedFunctionSearchStarted` for the same function.</span></span> <span data-ttu-id="70e2a-119">即使探查器已经规定了 JIT 编译函数的意图，探查器仍会收到第二次回调，因为线程 B 会在探查器响应线程 A 对的调用之前发送回调 `JITCachedFunctionSearchStarted` 。</span><span class="sxs-lookup"><span data-stu-id="70e2a-119">Even though the profiler has stated its intention to JIT-compile the function, the profiler receives the second callback because thread B sends the callback before the profiler has responded to thread A's call to `JITCachedFunctionSearchStarted`.</span></span> <span data-ttu-id="70e2a-120">线程发出调用的顺序取决于线程的计划方式。</span><span class="sxs-lookup"><span data-stu-id="70e2a-120">The order in which the threads make calls depends on how the threads are scheduled.</span></span>  
  
 <span data-ttu-id="70e2a-121">当探查器收到重复的回调时，它必须将引用的值设置 `pbUseCachedFunction` 为所有重复回调的相同值。</span><span class="sxs-lookup"><span data-stu-id="70e2a-121">When the profiler receives duplicate callbacks, it must set the value referenced by `pbUseCachedFunction` to the same value for all the duplicate callbacks.</span></span> <span data-ttu-id="70e2a-122">也就是说，当 `JITCachedFunctionSearchStarted` 用相同的值调用多次时 `functionId` ，探查器每次都必须响应相同的值。</span><span class="sxs-lookup"><span data-stu-id="70e2a-122">That is, when `JITCachedFunctionSearchStarted` is called multiple times with the same `functionId` value, the profiler must respond the same each time.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="70e2a-123">要求</span><span class="sxs-lookup"><span data-stu-id="70e2a-123">Requirements</span></span>  

 <span data-ttu-id="70e2a-124">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="70e2a-124">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="70e2a-125">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="70e2a-125">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="70e2a-126">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="70e2a-126">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="70e2a-127">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="70e2a-127">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="70e2a-128">另请参阅</span><span class="sxs-lookup"><span data-stu-id="70e2a-128">See also</span></span>

- [<span data-ttu-id="70e2a-129">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="70e2a-129">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
