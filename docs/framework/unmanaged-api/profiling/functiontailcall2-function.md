---
description: 了解详细信息： FunctionTailcall2 函数
title: FunctionTailcall2 函数
ms.date: 03/30/2017
api_name:
- FunctionTailcall2
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionTailcall2
helpviewer_keywords:
- FunctionTailcall2 function [.NET Framework profiling]
ms.assetid: 249f9892-b5a9-41e1-b329-28a925904df6
topic_type:
- apiref
ms.openlocfilehash: e06c3bde7ad0700de3d7f08b33159032b31eae8a
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760062"
---
# <a name="functiontailcall2-function"></a><span data-ttu-id="468ee-103">FunctionTailcall2 函数</span><span class="sxs-lookup"><span data-stu-id="468ee-103">FunctionTailcall2 Function</span></span>

<span data-ttu-id="468ee-104">通知探查器，当前正在执行的函数即将对另一个函数执行尾调用，并提供有关堆栈帧的信息。</span><span class="sxs-lookup"><span data-stu-id="468ee-104">Notifies the profiler that the currently executing function is about to perform a tail call to another function and provides information about the stack frame.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="468ee-105">语法</span><span class="sxs-lookup"><span data-stu-id="468ee-105">Syntax</span></span>  
  
```cpp
void __stdcall FunctionTailcall2 (  
    [in] FunctionID         funcId,
    [in] UINT_PTR           clientData,
    [in] COR_PRF_FRAME_INFO func  
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="468ee-106">parameters</span><span class="sxs-lookup"><span data-stu-id="468ee-106">Parameters</span></span>

<span data-ttu-id="468ee-107">`funcId` 中要进行尾调用的当前正在执行的函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="468ee-107">`funcId` [in] The identifier of the currently executing function that is about to make a tail call.</span></span>

<span data-ttu-id="468ee-108">`clientData` 中探查器前面通过 [FunctionIDMapper](functionidmapper-function.md)指定的函数标识符，当前正在执行的函数将进行尾调用。</span><span class="sxs-lookup"><span data-stu-id="468ee-108">`clientData` [in] The remapped function identifier, which the profiler previously specified via [FunctionIDMapper](functionidmapper-function.md), of the currently executing function that is about to make a tail call.</span></span>
  
<span data-ttu-id="468ee-109">`func` 中一个 `COR_PRF_FRAME_INFO` 值，该值指向有关堆栈帧的信息。</span><span class="sxs-lookup"><span data-stu-id="468ee-109">`func` [in] A `COR_PRF_FRAME_INFO` value that points to information about the stack frame.</span></span>

<span data-ttu-id="468ee-110">探查器应将此视为不透明的句柄，该句柄可传递回 [ICorProfilerInfo2：： GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) 方法中的执行引擎。</span><span class="sxs-lookup"><span data-stu-id="468ee-110">The profiler should treat this as an opaque handle that can be passed back to the execution engine in the [ICorProfilerInfo2::GetFunctionInfo2](icorprofilerinfo2-getfunctioninfo2-method.md) method.</span></span>

## <a name="remarks"></a><span data-ttu-id="468ee-111">备注</span><span class="sxs-lookup"><span data-stu-id="468ee-111">Remarks</span></span>  

 <span data-ttu-id="468ee-112">尾调用的目标函数将使用当前堆栈帧，并将直接返回到进行尾调用的函数的调用方。</span><span class="sxs-lookup"><span data-stu-id="468ee-112">The target function of the tail call will use the current stack frame, and will return directly to the caller of the function that made the tail call.</span></span> <span data-ttu-id="468ee-113">这意味着将不会为作为尾调用目标的函数发出 [FunctionLeave2](functionleave2-function.md) 回调。</span><span class="sxs-lookup"><span data-stu-id="468ee-113">This means that a [FunctionLeave2](functionleave2-function.md) callback will not be issued for a function that is the target of a tail call.</span></span>  
  
 <span data-ttu-id="468ee-114">该 `func` 函数返回后，该参数的值无效， `FunctionTailcall2` 因为该值可能会更改或被销毁。</span><span class="sxs-lookup"><span data-stu-id="468ee-114">The value of the `func` parameter is not valid after the `FunctionTailcall2` function returns because the value may change or be destroyed.</span></span>  
  
 <span data-ttu-id="468ee-115">`FunctionTailcall2`函数是回调; 必须实现它。</span><span class="sxs-lookup"><span data-stu-id="468ee-115">The `FunctionTailcall2` function is a callback; you must implement it.</span></span> <span data-ttu-id="468ee-116">实现必须使用 `__declspec` `naked`) 存储类特性的 (。</span><span class="sxs-lookup"><span data-stu-id="468ee-116">The implementation must use the `__declspec`(`naked`) storage-class attribute.</span></span>  
  
 <span data-ttu-id="468ee-117">在调用此函数之前，执行引擎不会保存任何注册。</span><span class="sxs-lookup"><span data-stu-id="468ee-117">The execution engine does not save any registers before calling this function.</span></span>  
  
- <span data-ttu-id="468ee-118">进入时，必须保存使用的所有寄存器，包括 (FPU) 的浮点单元中的寄存器。</span><span class="sxs-lookup"><span data-stu-id="468ee-118">On entry, you must save all registers that you use, including those in the floating-point unit (FPU).</span></span>  
  
- <span data-ttu-id="468ee-119">退出时，必须通过弹出由其调用方推送的所有参数来还原堆栈。</span><span class="sxs-lookup"><span data-stu-id="468ee-119">On exit, you must restore the stack by popping off all the parameters that were pushed by its caller.</span></span>  
  
 <span data-ttu-id="468ee-120">的实现 `FunctionTailcall2` 不应被阻止，因为它将延迟垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="468ee-120">The implementation of `FunctionTailcall2` should not block because it will delay garbage collection.</span></span> <span data-ttu-id="468ee-121">实现不应尝试垃圾回收，因为堆栈可能不处于垃圾回收友好状态。</span><span class="sxs-lookup"><span data-stu-id="468ee-121">The implementation should not attempt a garbage collection because the stack may not be in a garbage collection-friendly state.</span></span> <span data-ttu-id="468ee-122">如果尝试垃圾回收，则运行时将被阻止，直到 `FunctionTailcall2` 返回。</span><span class="sxs-lookup"><span data-stu-id="468ee-122">If a garbage collection is attempted, the runtime will block until `FunctionTailcall2` returns.</span></span>  
  
 <span data-ttu-id="468ee-123">此外，该 `FunctionTailcall2` 函数不得调入托管代码或以任何方式导致托管的内存分配。</span><span class="sxs-lookup"><span data-stu-id="468ee-123">Also, the `FunctionTailcall2` function must not call into managed code or in any way cause a managed memory allocation.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="468ee-124">要求</span><span class="sxs-lookup"><span data-stu-id="468ee-124">Requirements</span></span>  

 <span data-ttu-id="468ee-125">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="468ee-125">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="468ee-126">**标头：** Corprof.idl .idl</span><span class="sxs-lookup"><span data-stu-id="468ee-126">**Header:** CorProf.idl</span></span>  
  
 <span data-ttu-id="468ee-127">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="468ee-127">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="468ee-128">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="468ee-128">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="468ee-129">另请参阅</span><span class="sxs-lookup"><span data-stu-id="468ee-129">See also</span></span>

- [<span data-ttu-id="468ee-130">FunctionEnter2 函数</span><span class="sxs-lookup"><span data-stu-id="468ee-130">FunctionEnter2 Function</span></span>](functionenter2-function.md)
- [<span data-ttu-id="468ee-131">FunctionLeave2 函数</span><span class="sxs-lookup"><span data-stu-id="468ee-131">FunctionLeave2 Function</span></span>](functionleave2-function.md)
- [<span data-ttu-id="468ee-132">SetEnterLeaveFunctionHooks2 方法</span><span class="sxs-lookup"><span data-stu-id="468ee-132">SetEnterLeaveFunctionHooks2 Method</span></span>](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [<span data-ttu-id="468ee-133">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="468ee-133">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)
