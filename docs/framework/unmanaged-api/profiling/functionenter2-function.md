---
description: 了解详细信息： FunctionEnter2 函数
title: FunctionEnter2 函数
ms.date: 03/30/2017
api_name:
- FunctionEnter2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionEnter2
helpviewer_keywords:
- FunctionEnter2 function [.NET Framework profiling]
ms.assetid: ce7a21f9-0ca3-4b92-bc4b-bb803cae3f51
topic_type:
- apiref
ms.openlocfilehash: f68aeffdd63222cd78d7dc361f09e0b4c3e5af51
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759386"
---
# <a name="functionenter2-function"></a><span data-ttu-id="dc5bb-103">FunctionEnter2 函数</span><span class="sxs-lookup"><span data-stu-id="dc5bb-103">FunctionEnter2 Function</span></span>

<span data-ttu-id="dc5bb-104">通知探查器控制正在传递到函数，并提供有关堆栈帧和函数参数的信息。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-104">Notifies the profiler that control is being passed to a function and provides information about the stack frame and function arguments.</span></span> <span data-ttu-id="dc5bb-105">此函数取代了 [FunctionEnter](functionenter-function.md) 函数。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-105">This function supersedes the [FunctionEnter](functionenter-function.md) function.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="dc5bb-106">语法</span><span class="sxs-lookup"><span data-stu-id="dc5bb-106">Syntax</span></span>  
  
```cpp  
void __stdcall FunctionEnter2 (  
    [in]  FunctionID                       funcId,
    [in]  UINT_PTR                         clientData,
    [in]  COR_PRF_FRAME_INFO               func,
    [in]  COR_PRF_FUNCTION_ARGUMENT_INFO  *argumentInfo  
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="dc5bb-107">parameters</span><span class="sxs-lookup"><span data-stu-id="dc5bb-107">Parameters</span></span>

<span data-ttu-id="dc5bb-108">`funcId` 中要传递控制的函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-108">`funcId` [in] The identifier of the function to which control is passed.</span></span>

<span data-ttu-id="dc5bb-109">`clientData` 中使用 [FunctionIDMapper](functionidmapper-function.md) 函数之前指定的探查器的重新映射函数标识符。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-109">`clientData` [in] The remapped function identifier, which the profiler previously specified by using the [FunctionIDMapper](functionidmapper-function.md) function.</span></span>
  
<span data-ttu-id="dc5bb-110">`func` 中一个 `COR_PRF_FRAME_INFO` 值，该值指向有关堆栈帧的信息。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-110">`func` [in] A `COR_PRF_FRAME_INFO` value that points to information about the stack frame.</span></span>
  
<span data-ttu-id="dc5bb-111">探查器应将此视为不透明的句柄，该句柄可传递回 [ICorProfilerInfo2：： GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) 方法中的执行引擎。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-111">The profiler should treat this as an opaque handle that can be passed back to the execution engine in the [ICorProfilerInfo2::GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) method.</span></span>  
  
<span data-ttu-id="dc5bb-112">`argumentInfo` 中指向 [COR_PRF_FUNCTION_ARGUMENT_INFO](cor-prf-function-argument-info-structure.md) 结构的指针，该结构指定函数参数在内存中的位置。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-112">`argumentInfo` [in] A pointer to a [COR_PRF_FUNCTION_ARGUMENT_INFO](cor-prf-function-argument-info-structure.md) structure that specifies the locations in memory of the function's arguments.</span></span>

<span data-ttu-id="dc5bb-113">为了访问参数信息， `COR_PRF_ENABLE_FUNCTION_ARGS` 必须设置标志。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-113">In order to access argument information, the `COR_PRF_ENABLE_FUNCTION_ARGS` flag must be set.</span></span> <span data-ttu-id="dc5bb-114">探查器可以使用 [ICorProfilerInfo：： SetEventMask](icorprofilerinfo-seteventmask-method.md) 方法来设置事件标志。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-114">The profiler can use the [ICorProfilerInfo::SetEventMask](icorprofilerinfo-seteventmask-method.md) method to set the event flags.</span></span>

## <a name="remarks"></a><span data-ttu-id="dc5bb-115">备注</span><span class="sxs-lookup"><span data-stu-id="dc5bb-115">Remarks</span></span>  

 <span data-ttu-id="dc5bb-116">`func` `argumentInfo` 当函数返回后，和参数的值无效， `FunctionEnter2` 因为值可能会更改或被销毁。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-116">The values of the `func` and `argumentInfo` parameters are not valid after the `FunctionEnter2` function returns because the values may change or be destroyed.</span></span>  
  
 <span data-ttu-id="dc5bb-117">`FunctionEnter2`函数是回调; 必须实现它。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-117">The `FunctionEnter2` function is a callback; you must implement it.</span></span> <span data-ttu-id="dc5bb-118">实现必须使用 `__declspec` `naked`) 存储类特性的 (。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-118">The implementation must use the `__declspec`(`naked`) storage-class attribute.</span></span>  
  
 <span data-ttu-id="dc5bb-119">在调用此函数之前，执行引擎不会保存任何注册。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-119">The execution engine does not save any registers before calling this function.</span></span>  
  
- <span data-ttu-id="dc5bb-120">进入时，必须保存使用的所有寄存器，包括 (FPU) 的浮点单元中的寄存器。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-120">On entry, you must save all registers that you use, including those in the floating-point unit (FPU).</span></span>  
  
- <span data-ttu-id="dc5bb-121">退出时，必须通过弹出由其调用方推送的所有参数来还原堆栈。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-121">On exit, you must restore the stack by popping off all the parameters that were pushed by its caller.</span></span>  
  
 <span data-ttu-id="dc5bb-122">的实现 `FunctionEnter2` 不应被阻止，因为它将延迟垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-122">The implementation of `FunctionEnter2` should not block because it will delay garbage collection.</span></span> <span data-ttu-id="dc5bb-123">实现不应尝试垃圾回收，因为堆栈可能不处于垃圾回收友好状态。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-123">The implementation should not attempt a garbage collection because the stack may not be in a garbage collection-friendly state.</span></span> <span data-ttu-id="dc5bb-124">如果尝试垃圾回收，则运行时将被阻止，直到 `FunctionEnter2` 返回。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-124">If a garbage collection is attempted, the runtime will block until `FunctionEnter2` returns.</span></span>  
  
 <span data-ttu-id="dc5bb-125">此外，该 `FunctionEnter2` 函数不得调入托管代码或以任何方式导致托管的内存分配。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-125">Also, the `FunctionEnter2` function must not call into managed code or in any way cause a managed memory allocation.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="dc5bb-126">要求</span><span class="sxs-lookup"><span data-stu-id="dc5bb-126">Requirements</span></span>  

 <span data-ttu-id="dc5bb-127">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="dc5bb-127">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="dc5bb-128">**标头：** Corprof.idl .idl</span><span class="sxs-lookup"><span data-stu-id="dc5bb-128">**Header:** CorProf.idl</span></span>  
  
 <span data-ttu-id="dc5bb-129">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="dc5bb-129">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="dc5bb-130">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="dc5bb-130">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="dc5bb-131">另请参阅</span><span class="sxs-lookup"><span data-stu-id="dc5bb-131">See also</span></span>

- [<span data-ttu-id="dc5bb-132">FunctionLeave2 函数</span><span class="sxs-lookup"><span data-stu-id="dc5bb-132">FunctionLeave2 Function</span></span>](functionleave2-function.md)
- [<span data-ttu-id="dc5bb-133">FunctionTailcall2 函数</span><span class="sxs-lookup"><span data-stu-id="dc5bb-133">FunctionTailcall2 Function</span></span>](functiontailcall2-function.md)
- [<span data-ttu-id="dc5bb-134">SetEnterLeaveFunctionHooks2 方法</span><span class="sxs-lookup"><span data-stu-id="dc5bb-134">SetEnterLeaveFunctionHooks2 Method</span></span>](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [<span data-ttu-id="dc5bb-135">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="dc5bb-135">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)
