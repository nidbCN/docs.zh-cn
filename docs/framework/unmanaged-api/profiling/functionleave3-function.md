---
description: 了解详细信息： FunctionLeave3 函数
title: FunctionLeave3 函数
ms.date: 03/30/2017
api_name:
- FunctionLeave3
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionLeave3
helpviewer_keywords:
- FunctionLeave3 function [.NET Framework profiling]
ms.assetid: 5d798088-7992-48a0-ae55-d2a7ee31913f
topic_type:
- apiref
ms.openlocfilehash: 0409a9969fddbbbcb8fcb5d870acccf1ae3a809b
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760101"
---
# <a name="functionleave3-function"></a><span data-ttu-id="f501b-103">FunctionLeave3 函数</span><span class="sxs-lookup"><span data-stu-id="f501b-103">FunctionLeave3 Function</span></span>

<span data-ttu-id="f501b-104">通知探查器控制正在从函数返回。</span><span class="sxs-lookup"><span data-stu-id="f501b-104">Notifies the profiler that control is being returned from a function.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="f501b-105">语法</span><span class="sxs-lookup"><span data-stu-id="f501b-105">Syntax</span></span>  
  
```cpp  
void __stdcall FunctionLeave3(FunctionOrRemappedID functionOrRemappedID);  
```  
  
## <a name="parameters"></a><span data-ttu-id="f501b-106">parameters</span><span class="sxs-lookup"><span data-stu-id="f501b-106">Parameters</span></span>  

<span data-ttu-id="f501b-107">`functionOrRemappedID` 中从中返回控件的函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="f501b-107">`functionOrRemappedID` [in] The identifier of the function from which control is returned.</span></span>
  
## <a name="remarks"></a><span data-ttu-id="f501b-108">备注</span><span class="sxs-lookup"><span data-stu-id="f501b-108">Remarks</span></span>  

 <span data-ttu-id="f501b-109">`FunctionLeave3`回调函数将在调用函数时通知探查器，但不支持返回值检查。</span><span class="sxs-lookup"><span data-stu-id="f501b-109">The `FunctionLeave3` callback function notifies the profiler as functions are being called, but does not support return value inspection.</span></span> <span data-ttu-id="f501b-110">使用 [ICorProfilerInfo3：： SetEnterLeaveFunctionHooks3 方法](icorprofilerinfo3-setenterleavefunctionhooks3-method.md) 注册此函数的实现。</span><span class="sxs-lookup"><span data-stu-id="f501b-110">Use the [ICorProfilerInfo3::SetEnterLeaveFunctionHooks3 method](icorprofilerinfo3-setenterleavefunctionhooks3-method.md) to register your implementation of this function.</span></span>  
  
 <span data-ttu-id="f501b-111">`FunctionLeave3`函数是回调; 必须实现它。</span><span class="sxs-lookup"><span data-stu-id="f501b-111">The `FunctionLeave3` function is a callback; you must implement it.</span></span> <span data-ttu-id="f501b-112">实现必须使用 `__declspec(naked)` 存储类特性。</span><span class="sxs-lookup"><span data-stu-id="f501b-112">The implementation must use the `__declspec(naked)` storage-class attribute.</span></span>  
  
 <span data-ttu-id="f501b-113">在调用此函数之前，执行引擎不会保存任何注册。</span><span class="sxs-lookup"><span data-stu-id="f501b-113">The execution engine does not save any registers before calling this function.</span></span>  
  
- <span data-ttu-id="f501b-114">进入时，必须保存使用的所有寄存器，包括 (FPU) 的浮点单元中的寄存器。</span><span class="sxs-lookup"><span data-stu-id="f501b-114">On entry, you must save all registers that you use, including those in the floating-point unit (FPU).</span></span>  
  
- <span data-ttu-id="f501b-115">退出时，必须通过弹出由其调用方推送的所有参数来还原堆栈。</span><span class="sxs-lookup"><span data-stu-id="f501b-115">On exit, you must restore the stack by popping off all the parameters that were pushed by its caller.</span></span>  
  
 <span data-ttu-id="f501b-116">的实现 `FunctionLeave3` 不应阻塞，因为它将延迟垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="f501b-116">The implementation of `FunctionLeave3` should not block, because it will delay garbage collection.</span></span> <span data-ttu-id="f501b-117">实现不应尝试垃圾回收，因为堆栈可能不处于垃圾回收友好状态。</span><span class="sxs-lookup"><span data-stu-id="f501b-117">The implementation should not attempt a garbage collection, because the stack may not be in a garbage collection-friendly state.</span></span> <span data-ttu-id="f501b-118">如果尝试垃圾回收，则运行时将被阻止，直到 `FunctionLeave3` 返回。</span><span class="sxs-lookup"><span data-stu-id="f501b-118">If a garbage collection is attempted, the runtime will block until `FunctionLeave3` returns.</span></span>  
  
 <span data-ttu-id="f501b-119">`FunctionLeave3`函数不得调入托管代码或以任何方式导致托管内存分配。</span><span class="sxs-lookup"><span data-stu-id="f501b-119">The `FunctionLeave3` function must not call into managed code or cause a managed memory allocation in any way.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="f501b-120">要求</span><span class="sxs-lookup"><span data-stu-id="f501b-120">Requirements</span></span>  

 <span data-ttu-id="f501b-121">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="f501b-121">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="f501b-122">**标头：** Corprof.idl .idl</span><span class="sxs-lookup"><span data-stu-id="f501b-122">**Header:** CorProf.idl</span></span>  
  
 <span data-ttu-id="f501b-123">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="f501b-123">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="f501b-124">**.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="f501b-124">**.NET Framework Versions:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="f501b-125">另请参阅</span><span class="sxs-lookup"><span data-stu-id="f501b-125">See also</span></span>

- [<span data-ttu-id="f501b-126">FunctionEnter3</span><span class="sxs-lookup"><span data-stu-id="f501b-126">FunctionEnter3</span></span>](functionenter3-function.md)
- [<span data-ttu-id="f501b-127">FunctionTailcall3</span><span class="sxs-lookup"><span data-stu-id="f501b-127">FunctionTailcall3</span></span>](functiontailcall3-function.md)
- [<span data-ttu-id="f501b-128">FunctionEnter3WithInfo</span><span class="sxs-lookup"><span data-stu-id="f501b-128">FunctionEnter3WithInfo</span></span>](functiontailcall3-function.md)
- [<span data-ttu-id="f501b-129">FunctionLeave3WithInfo</span><span class="sxs-lookup"><span data-stu-id="f501b-129">FunctionLeave3WithInfo</span></span>](functionleave3withinfo-function.md)
- [<span data-ttu-id="f501b-130">FunctionTailcall3WithInfo</span><span class="sxs-lookup"><span data-stu-id="f501b-130">FunctionTailcall3WithInfo</span></span>](functiontailcall3withinfo-function.md)
- [<span data-ttu-id="f501b-131">SetEnterLeaveFunctionHooks3</span><span class="sxs-lookup"><span data-stu-id="f501b-131">SetEnterLeaveFunctionHooks3</span></span>](icorprofilerinfo3-setenterleavefunctionhooks3-method.md)
- [<span data-ttu-id="f501b-132">SetEnterLeaveFunctionHooks3WithInfo</span><span class="sxs-lookup"><span data-stu-id="f501b-132">SetEnterLeaveFunctionHooks3WithInfo</span></span>](icorprofilerinfo3-setenterleavefunctionhooks3withinfo-method.md)
- [<span data-ttu-id="f501b-133">SetFunctionIDMapper</span><span class="sxs-lookup"><span data-stu-id="f501b-133">SetFunctionIDMapper</span></span>](icorprofilerinfo-setfunctionidmapper-method.md)
- [<span data-ttu-id="f501b-134">SetFunctionIDMapper2</span><span class="sxs-lookup"><span data-stu-id="f501b-134">SetFunctionIDMapper2</span></span>](icorprofilerinfo3-setfunctionidmapper2-method.md)
- [<span data-ttu-id="f501b-135">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="f501b-135">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)
