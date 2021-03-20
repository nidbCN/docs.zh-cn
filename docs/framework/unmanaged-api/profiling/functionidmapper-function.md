---
description: 了解详细信息： FunctionIDMapper 函数
title: FunctionIDMapper 函数
ms.date: 03/30/2017
api_name:
- FunctionIDMapper
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionIDMapper
helpviewer_keywords:
- FunctionIDMapper function [.NET Framework profiling]
ms.assetid: b8205b60-1893-4303-8cff-7ac5a00892aa
topic_type:
- apiref
ms.openlocfilehash: 2db616509bf5dcb5b8aee9cea76a9841369ec49d
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759231"
---
# <a name="functionidmapper-function"></a><span data-ttu-id="96a28-103">FunctionIDMapper 函数</span><span class="sxs-lookup"><span data-stu-id="96a28-103">FunctionIDMapper Function</span></span>

<span data-ttu-id="96a28-104">通知探查器可能会将函数的给定标识符重新映射到备用 ID，以在该函数的 [FunctionEnter2](functionenter2-function.md)、 [FunctionLeave2](functionleave2-function.md)和 [FunctionTailcall2](functiontailcall2-function.md) 回调中使用。</span><span class="sxs-lookup"><span data-stu-id="96a28-104">Notifies the profiler that the given identifier of a function may be remapped to an alternative ID to be used in the [FunctionEnter2](functionenter2-function.md), [FunctionLeave2](functionleave2-function.md), and [FunctionTailcall2](functiontailcall2-function.md) callbacks for that function.</span></span> <span data-ttu-id="96a28-105">`FunctionIDMapper` 此外还要使探查器指示它是否想要接收该函数的回调。</span><span class="sxs-lookup"><span data-stu-id="96a28-105">`FunctionIDMapper` also enables the profiler to indicate whether it wants to receive callbacks for that function.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="96a28-106">语法</span><span class="sxs-lookup"><span data-stu-id="96a28-106">Syntax</span></span>  
  
```cpp  
UINT_PTR __stdcall FunctionIDMapper (  
    [in]  FunctionID  funcId,
    [out] BOOL       *pbHookFunction  
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="96a28-107">parameters</span><span class="sxs-lookup"><span data-stu-id="96a28-107">Parameters</span></span>

<span data-ttu-id="96a28-108">`funcId` 中要重新映射的函数标识符。</span><span class="sxs-lookup"><span data-stu-id="96a28-108">`funcId` [in] The function identifier to be remapped.</span></span>

<span data-ttu-id="96a28-109">`pbHookFunction` 弄一个指针，指向探查器设置为的值 `true` `FunctionEnter2` `FunctionLeave2` `FunctionTailcall2` ; 否则，它会将此值设置为 `false` 。</span><span class="sxs-lookup"><span data-stu-id="96a28-109">`pbHookFunction` [out] A pointer to a value that the profiler sets to `true` if it wants to receive `FunctionEnter2`, `FunctionLeave2`, and `FunctionTailcall2` callbacks; otherwise, it sets this value to `false`.</span></span>

## <a name="return-value"></a><span data-ttu-id="96a28-110">返回值</span><span class="sxs-lookup"><span data-stu-id="96a28-110">Return Value</span></span>  

 <span data-ttu-id="96a28-111">探查器返回一个执行引擎用作替代函数标识符的值。</span><span class="sxs-lookup"><span data-stu-id="96a28-111">The profiler returns a value that the execution engine uses as an alternative function identifier.</span></span> <span data-ttu-id="96a28-112">返回值不能为 null，除非在 `pbHookFunction` 中返回 `false`。</span><span class="sxs-lookup"><span data-stu-id="96a28-112">The return value cannot be null unless `false` is returned in `pbHookFunction`.</span></span> <span data-ttu-id="96a28-113">否则，空返回值将产生不可预知的结果，包括可能停止进程。</span><span class="sxs-lookup"><span data-stu-id="96a28-113">Otherwise, a null return value will produce unpredictable results, including possibly halting the process.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="96a28-114">备注</span><span class="sxs-lookup"><span data-stu-id="96a28-114">Remarks</span></span>  

 <span data-ttu-id="96a28-115">`FunctionIDMapper`函数是回调。</span><span class="sxs-lookup"><span data-stu-id="96a28-115">The `FunctionIDMapper` function is a callback.</span></span> <span data-ttu-id="96a28-116">它由探查器实现，用于将函数 ID 重新映射到其他更适用于探查器的标识符。</span><span class="sxs-lookup"><span data-stu-id="96a28-116">It is implemented by the profiler to remap a function ID to some other identifier that is more useful for the profiler.</span></span> <span data-ttu-id="96a28-117">`FunctionIDMapper`返回要用于任何给定函数的备用 ID。</span><span class="sxs-lookup"><span data-stu-id="96a28-117">The `FunctionIDMapper` returns the alternate ID to be used for any given function.</span></span> <span data-ttu-id="96a28-118">然后，执行引擎通过将此备用 ID （除了传统函数 ID）传递回探查器的参数中的探查器来实现探查器的请求，以 `clientData` `FunctionEnter2` `FunctionLeave2` `FunctionTailcall2` 标识调用挂钩的函数。</span><span class="sxs-lookup"><span data-stu-id="96a28-118">The execution engine then honors the profiler's request by passing this alternate ID, in addition to the traditional function ID, back to the profiler in the `clientData` parameter of the `FunctionEnter2`, `FunctionLeave2`, and `FunctionTailcall2` hooks, to identify the function for which the hook is being called.</span></span>  
  
 <span data-ttu-id="96a28-119">您可以使用 [ICorProfilerInfo：： SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md) 方法来指定函数的实现 `FunctionIDMapper` 。</span><span class="sxs-lookup"><span data-stu-id="96a28-119">You can use the [ICorProfilerInfo::SetFunctionIDMapper](icorprofilerinfo-setfunctionidmapper-method.md) method to specify the implementation of the `FunctionIDMapper` function.</span></span> <span data-ttu-id="96a28-120">只能调用 `ICorProfilerInfo::SetFunctionIDMapper` 一次该方法，我们建议在 [ICorProfilerCallback：： Initialize](icorprofilercallback-initialize-method.md) 回调中执行此操作。</span><span class="sxs-lookup"><span data-stu-id="96a28-120">You can call the `ICorProfilerInfo::SetFunctionIDMapper` method only once, and we recommend that you do so in the [ICorProfilerCallback::Initialize](icorprofilercallback-initialize-method.md) callback.</span></span>  
  
 <span data-ttu-id="96a28-121">默认情况下，假定探查器通过使用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md)设置 COR_PRF_MONITOR_ENTERLEAVE 标志，并通过 [ICorProfilerInfo：： SetEnterLeaveFunctionHooks](icorprofilerinfo-setenterleavefunctionhooks-method.md) 或 [ICorProfilerInfo2：： SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)设置挂钩，应接收 `FunctionEnter2` `FunctionLeave2` `FunctionTailcall2` 每个函数的、和回调。</span><span class="sxs-lookup"><span data-stu-id="96a28-121">By default, it is assumed that a profiler that sets the COR_PRF_MONITOR_ENTERLEAVE flag by using [ICorProfilerInfo::SetEventMask](icorprofilerinfo-seteventmask-method.md), and which sets hooks via [ICorProfilerInfo::SetEnterLeaveFunctionHooks](icorprofilerinfo-setenterleavefunctionhooks-method.md) or [ICorProfilerInfo2::SetEnterLeaveFunctionHooks2](icorprofilerinfo2-setenterleavefunctionhooks2-method.md), should receive the `FunctionEnter2`, `FunctionLeave2`, and `FunctionTailcall2` callbacks for every function.</span></span> <span data-ttu-id="96a28-122">但是，探查器可 `FunctionIDMapper` 通过将设置为来有选择地避免为某些函数接收这些回调 `pbHookFunction` `false` 。</span><span class="sxs-lookup"><span data-stu-id="96a28-122">However, profilers may implement `FunctionIDMapper` to selectively avoid receiving these callbacks for certain functions by setting `pbHookFunction` to `false`.</span></span>  
  
 <span data-ttu-id="96a28-123">在分析的应用程序的多个线程同时调用相同的方法/函数的情况下，探查器应该是一种容错方法。</span><span class="sxs-lookup"><span data-stu-id="96a28-123">Profilers should be tolerant of cases where multiple threads of a profiled application are calling the same method/function simultaneously.</span></span> <span data-ttu-id="96a28-124">在这种情况下，探查器可能会收到相同的多个 `FunctionIDMapper` 回调 `FunctionID` 。</span><span class="sxs-lookup"><span data-stu-id="96a28-124">In such cases, the profiler may receive multiple `FunctionIDMapper` callbacks for the same `FunctionID`.</span></span> <span data-ttu-id="96a28-125">如果多次调用相同的，则探查器应确保从此回调返回相同的值 `FunctionID` 。</span><span class="sxs-lookup"><span data-stu-id="96a28-125">The profiler should be certain to return the same values from this callback when it is called multiple times with the same `FunctionID`.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="96a28-126">要求</span><span class="sxs-lookup"><span data-stu-id="96a28-126">Requirements</span></span>  

 <span data-ttu-id="96a28-127">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="96a28-127">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="96a28-128">**标头：** Corprof.idl .idl</span><span class="sxs-lookup"><span data-stu-id="96a28-128">**Header:** CorProf.idl</span></span>  
  
 <span data-ttu-id="96a28-129">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="96a28-129">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="96a28-130">**.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="96a28-130">**.NET Framework Versions:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="96a28-131">另请参阅</span><span class="sxs-lookup"><span data-stu-id="96a28-131">See also</span></span>

- [<span data-ttu-id="96a28-132">SetFunctionIDMapper 方法</span><span class="sxs-lookup"><span data-stu-id="96a28-132">SetFunctionIDMapper Method</span></span>](icorprofilerinfo-setfunctionidmapper-method.md)
- [<span data-ttu-id="96a28-133">FunctionIDMapper2 函数</span><span class="sxs-lookup"><span data-stu-id="96a28-133">FunctionIDMapper2 Function</span></span>](functionidmapper2-function.md)
- [<span data-ttu-id="96a28-134">FunctionEnter2 函数</span><span class="sxs-lookup"><span data-stu-id="96a28-134">FunctionEnter2 Function</span></span>](functionenter2-function.md)
- [<span data-ttu-id="96a28-135">FunctionLeave2 函数</span><span class="sxs-lookup"><span data-stu-id="96a28-135">FunctionLeave2 Function</span></span>](functionleave2-function.md)
- [<span data-ttu-id="96a28-136">FunctionTailcall2 函数</span><span class="sxs-lookup"><span data-stu-id="96a28-136">FunctionTailcall2 Function</span></span>](functiontailcall2-function.md)
- [<span data-ttu-id="96a28-137">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="96a28-137">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)
