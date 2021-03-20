---
description: 了解详细信息： FunctionLeave3WithInfo 函数
title: FunctionLeave3WithInfo 函数
ms.date: 03/30/2017
api_name:
- FunctionLeave3WithInfo
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionLeave3WithInfo
helpviewer_keywords:
- FunctionLeave3WithInfo function [.NET Framework profiling]
ms.assetid: 5fa68a67-ced6-41c6-a2c0-467060fd0692
topic_type:
- apiref
ms.openlocfilehash: 617ca023f58a180c198751fea9752fe737249331
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760075"
---
# <a name="functionleave3withinfo-function"></a><span data-ttu-id="615d2-103">FunctionLeave3WithInfo 函数</span><span class="sxs-lookup"><span data-stu-id="615d2-103">FunctionLeave3WithInfo Function</span></span>

<span data-ttu-id="615d2-104">通知探查器控制正在从函数返回，并提供一个可传递给 [ICorProfilerInfo3：： GetFunctionLeave3Info 方法](icorprofilerinfo3-getfunctionleave3info-method.md) 的句柄，以检索堆栈帧和返回值。</span><span class="sxs-lookup"><span data-stu-id="615d2-104">Notifies the profiler that control is being returned from a function, and provides a handle that can be passed to the [ICorProfilerInfo3::GetFunctionLeave3Info method](icorprofilerinfo3-getfunctionleave3info-method.md) to retrieve the stack frame and the return value.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="615d2-105">语法</span><span class="sxs-lookup"><span data-stu-id="615d2-105">Syntax</span></span>  
  
```cpp  
void __stdcall FunctionLeave3WithInfo(  
               [in] FunctionIDOrClientID functionIDOrClientID,  
               [in] COR_PRF_ELT_INFO eltInfo);  
```  
  
## <a name="parameters"></a><span data-ttu-id="615d2-106">parameters</span><span class="sxs-lookup"><span data-stu-id="615d2-106">Parameters</span></span>

<span data-ttu-id="615d2-107">`functionIDOrClientID` 中从中返回控件的函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="615d2-107">`functionIDOrClientID` [in] The identifier of the function from which control is returned.</span></span>

<span data-ttu-id="615d2-108">`eltInfo` 中表示有关给定堆栈帧的信息的不透明的句柄。</span><span class="sxs-lookup"><span data-stu-id="615d2-108">`eltInfo` [in] An opaque handle that represents information about a given stack frame.</span></span> <span data-ttu-id="615d2-109">此句柄仅在其传递到的回调期间有效。</span><span class="sxs-lookup"><span data-stu-id="615d2-109">This handle is valid only during the callback to which it is passed.</span></span>

## <a name="remarks"></a><span data-ttu-id="615d2-110">备注</span><span class="sxs-lookup"><span data-stu-id="615d2-110">Remarks</span></span>  

 <span data-ttu-id="615d2-111">`FunctionLeave3WithInfo`回调方法会在调用函数时通知探查器，并允许探查器使用[ICorProfilerInfo3：： GetFunctionLeave3Info 方法](icorprofilerinfo3-getfunctionleave3info-method.md)来检查返回值。</span><span class="sxs-lookup"><span data-stu-id="615d2-111">The `FunctionLeave3WithInfo` callback method notifies the profiler as functions are called, and allows the profiler to use the [ICorProfilerInfo3::GetFunctionLeave3Info method](icorprofilerinfo3-getfunctionleave3info-method.md) to inspect the return value.</span></span> <span data-ttu-id="615d2-112">若要访问返回值信息，必须 `COR_PRF_ENABLE_FUNCTION_RETVAL` 设置标志。</span><span class="sxs-lookup"><span data-stu-id="615d2-112">To access return value information, the `COR_PRF_ENABLE_FUNCTION_RETVAL` flag has to be set.</span></span> <span data-ttu-id="615d2-113">探查器可以使用 [ICorProfilerInfo：： SetEventMask 方法](icorprofilerinfo-seteventmask-method.md) 来设置事件标志，然后使用 [ICorProfilerInfo3：： SetEnterLeaveFunctionHooks3WithInfo 方法](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md) 来注册此函数的实现。</span><span class="sxs-lookup"><span data-stu-id="615d2-113">The profiler can use the [ICorProfilerInfo::SetEventMask method](icorprofilerinfo-seteventmask-method.md) to set the event flags, and then use the [ICorProfilerInfo3::SetEnterLeaveFunctionHooks3WithInfo method](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md) to register your implementation of this function.</span></span>  
  
 <span data-ttu-id="615d2-114">`FunctionLeave3WithInfo`函数是回调; 必须实现它。</span><span class="sxs-lookup"><span data-stu-id="615d2-114">The `FunctionLeave3WithInfo` function is a callback; you must implement it.</span></span> <span data-ttu-id="615d2-115">实现必须使用 `__declspec(naked)` 存储类特性。</span><span class="sxs-lookup"><span data-stu-id="615d2-115">The implementation must use the `__declspec(naked)` storage-class attribute.</span></span>  
  
 <span data-ttu-id="615d2-116">在调用此函数之前，执行引擎不会保存任何注册。</span><span class="sxs-lookup"><span data-stu-id="615d2-116">The execution engine does not save any registers before calling this function.</span></span>  
  
- <span data-ttu-id="615d2-117">进入时，必须保存使用的所有寄存器，包括 (FPU) 的浮点单元中的寄存器。</span><span class="sxs-lookup"><span data-stu-id="615d2-117">On entry, you must save all registers that you use, including those in the floating-point unit (FPU).</span></span>  
  
- <span data-ttu-id="615d2-118">退出时，必须通过弹出由其调用方推送的所有参数来还原堆栈。</span><span class="sxs-lookup"><span data-stu-id="615d2-118">On exit, you must restore the stack by popping off all the parameters that were pushed by its caller.</span></span>  
  
 <span data-ttu-id="615d2-119">的实现 `FunctionLeave3WithInfo` 不应阻塞，因为它将延迟垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="615d2-119">The implementation of `FunctionLeave3WithInfo` should not block, because it will delay garbage collection.</span></span> <span data-ttu-id="615d2-120">实现不应尝试垃圾回收，因为堆栈可能不处于垃圾回收友好状态。</span><span class="sxs-lookup"><span data-stu-id="615d2-120">The implementation should not attempt a garbage collection, because the stack may not be in a garbage collection-friendly state.</span></span> <span data-ttu-id="615d2-121">如果尝试垃圾回收，则运行时将被阻止，直到 `FunctionLeave3WithInfo` 返回。</span><span class="sxs-lookup"><span data-stu-id="615d2-121">If a garbage collection is attempted, the runtime will block until `FunctionLeave3WithInfo` returns.</span></span>  
  
 <span data-ttu-id="615d2-122">`FunctionLeave3WithInfo`函数不得调入托管代码或以任何方式导致托管内存分配。</span><span class="sxs-lookup"><span data-stu-id="615d2-122">The `FunctionLeave3WithInfo` function must not call into managed code or cause a managed memory allocation in any way.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="615d2-123">要求</span><span class="sxs-lookup"><span data-stu-id="615d2-123">Requirements</span></span>  

 <span data-ttu-id="615d2-124">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="615d2-124">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="615d2-125">**标头：** Corprof.idl .idl</span><span class="sxs-lookup"><span data-stu-id="615d2-125">**Header:** CorProf.idl</span></span>  
  
 <span data-ttu-id="615d2-126">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="615d2-126">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="615d2-127">**.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="615d2-127">**.NET Framework Versions:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="615d2-128">另请参阅</span><span class="sxs-lookup"><span data-stu-id="615d2-128">See also</span></span>

- [<span data-ttu-id="615d2-129">GetFunctionLeave3Info</span><span class="sxs-lookup"><span data-stu-id="615d2-129">GetFunctionLeave3Info</span></span>](icorprofilerinfo3-getfunctionleave3info-method.md)
- [<span data-ttu-id="615d2-130">FunctionEnter3</span><span class="sxs-lookup"><span data-stu-id="615d2-130">FunctionEnter3</span></span>](functionenter3-function.md)
- [<span data-ttu-id="615d2-131">FunctionLeave3</span><span class="sxs-lookup"><span data-stu-id="615d2-131">FunctionLeave3</span></span>](functionleave3-function.md)
- [<span data-ttu-id="615d2-132">FunctionTailcall3</span><span class="sxs-lookup"><span data-stu-id="615d2-132">FunctionTailcall3</span></span>](functiontailcall3-function.md)
- [<span data-ttu-id="615d2-133">FunctionEnter3WithInfo</span><span class="sxs-lookup"><span data-stu-id="615d2-133">FunctionEnter3WithInfo</span></span>](functionenter3withinfo-function.md)
- [<span data-ttu-id="615d2-134">FunctionTailcall3WithInfo</span><span class="sxs-lookup"><span data-stu-id="615d2-134">FunctionTailcall3WithInfo</span></span>](functiontailcall3withinfo-function.md)
- [<span data-ttu-id="615d2-135">SetEnterLeaveFunctionHooks3</span><span class="sxs-lookup"><span data-stu-id="615d2-135">SetEnterLeaveFunctionHooks3</span></span>](icorprofilerinfo3-setenterleavefunctionhooks3-method.md)
- [<span data-ttu-id="615d2-136">SetEnterLeaveFunctionHooks3WithInfo</span><span class="sxs-lookup"><span data-stu-id="615d2-136">SetEnterLeaveFunctionHooks3WithInfo</span></span>](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md)
- [<span data-ttu-id="615d2-137">SetFunctionIDMapper</span><span class="sxs-lookup"><span data-stu-id="615d2-137">SetFunctionIDMapper</span></span>](icorprofilerinfo-setfunctionidmapper-method.md)
- [<span data-ttu-id="615d2-138">SetFunctionIDMapper2</span><span class="sxs-lookup"><span data-stu-id="615d2-138">SetFunctionIDMapper2</span></span>](icorprofilerinfo3-setfunctionidmapper2-method.md)
- [<span data-ttu-id="615d2-139">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="615d2-139">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)
