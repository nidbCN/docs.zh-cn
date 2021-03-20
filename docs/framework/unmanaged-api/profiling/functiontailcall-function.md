---
description: 了解详细信息： FunctionTailcall 函数
title: FunctionTailcall 函数
ms.date: 03/30/2017
api_name:
- FunctionTailcall
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- FunctionTailcall
helpviewer_keywords:
- FunctionTailcall function [.NET Framework profiling]
ms.assetid: 66347e03-9a97-41e8-8f9d-89b80803f7b5
topic_type:
- apiref
ms.openlocfilehash: aeb6e7dcbf52fc57ebb7b6dca22331c27cadc186
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760023"
---
# <a name="functiontailcall-function"></a><span data-ttu-id="1eb95-103">FunctionTailcall 函数</span><span class="sxs-lookup"><span data-stu-id="1eb95-103">FunctionTailcall Function</span></span>

<span data-ttu-id="1eb95-104">通知探查器，当前正在执行的函数即将对另一个函数执行尾调用。</span><span class="sxs-lookup"><span data-stu-id="1eb95-104">Notifies the profiler that the currently executing function is about to perform a tail call to another function.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="1eb95-105">`FunctionTailcall`.NET Framework 版本2.0 中不推荐使用该函数。</span><span class="sxs-lookup"><span data-stu-id="1eb95-105">The `FunctionTailcall` function is deprecated in the .NET Framework version 2.0.</span></span> <span data-ttu-id="1eb95-106">它将继续运行，但会导致性能下降。</span><span class="sxs-lookup"><span data-stu-id="1eb95-106">It will continue to work, but will incur a performance penalty.</span></span> <span data-ttu-id="1eb95-107">改为使用 [FunctionTailcall2](functiontailcall2-function.md) 函数。</span><span class="sxs-lookup"><span data-stu-id="1eb95-107">Use the [FunctionTailcall2](functiontailcall2-function.md) function instead.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="1eb95-108">语法</span><span class="sxs-lookup"><span data-stu-id="1eb95-108">Syntax</span></span>  
  
```cpp
void __stdcall FunctionTailcall (  
    [in] FunctionID funcID  
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="1eb95-109">parameters</span><span class="sxs-lookup"><span data-stu-id="1eb95-109">Parameters</span></span>

<span data-ttu-id="1eb95-110">`funcID` 中要进行尾调用的当前正在执行的函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="1eb95-110">`funcID` [in] The identifier of the currently executing function that is about to make a tail call.</span></span>

## <a name="remarks"></a><span data-ttu-id="1eb95-111">备注</span><span class="sxs-lookup"><span data-stu-id="1eb95-111">Remarks</span></span>  

 <span data-ttu-id="1eb95-112">尾调用的目标函数将使用当前堆栈帧，并将直接返回到进行尾调用的函数的调用方。</span><span class="sxs-lookup"><span data-stu-id="1eb95-112">The target function of the tail call will use the current stack frame, and will return directly to the caller of the function that made the tail call.</span></span> <span data-ttu-id="1eb95-113">这意味着将不会为作为尾调用目标的函数发出 [FunctionLeave](functionleave-function.md) 回调。</span><span class="sxs-lookup"><span data-stu-id="1eb95-113">This means that a [FunctionLeave](functionleave-function.md) callback will not be issued for a function that is the target of a tail call.</span></span>  
  
 <span data-ttu-id="1eb95-114">`FunctionTailcall`函数是回调; 必须实现它。</span><span class="sxs-lookup"><span data-stu-id="1eb95-114">The `FunctionTailcall` function is a callback; you must implement it.</span></span> <span data-ttu-id="1eb95-115">实现必须使用 `__declspec` `naked`) 存储类特性的 (。</span><span class="sxs-lookup"><span data-stu-id="1eb95-115">The implementation must use the `__declspec`(`naked`) storage-class attribute.</span></span>  
  
 <span data-ttu-id="1eb95-116">在调用此函数之前，执行引擎不会保存任何注册。</span><span class="sxs-lookup"><span data-stu-id="1eb95-116">The execution engine does not save any registers before calling this function.</span></span>  
  
- <span data-ttu-id="1eb95-117">进入时，必须保存使用的所有寄存器，包括 (FPU) 的浮点单元中的寄存器。</span><span class="sxs-lookup"><span data-stu-id="1eb95-117">On entry, you must save all registers that you use, including those in the floating-point unit (FPU).</span></span>  
  
- <span data-ttu-id="1eb95-118">退出时，必须通过弹出由其调用方推送的所有参数来还原堆栈。</span><span class="sxs-lookup"><span data-stu-id="1eb95-118">On exit, you must restore the stack by popping off all the parameters that were pushed by its caller.</span></span>  
  
 <span data-ttu-id="1eb95-119">的实现 `FunctionTailcall` 不应被阻止，因为它将延迟垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="1eb95-119">The implementation of `FunctionTailcall` should not block because it will delay garbage collection.</span></span> <span data-ttu-id="1eb95-120">实现不应尝试垃圾回收，因为堆栈可能不处于垃圾回收友好状态。</span><span class="sxs-lookup"><span data-stu-id="1eb95-120">The implementation should not attempt a garbage collection because the stack may not be in a garbage collection-friendly state.</span></span> <span data-ttu-id="1eb95-121">如果尝试垃圾回收，则运行时将被阻止，直到 `FunctionTailcall` 返回。</span><span class="sxs-lookup"><span data-stu-id="1eb95-121">If a garbage collection is attempted, the runtime will block until `FunctionTailcall` returns.</span></span>  
  
 <span data-ttu-id="1eb95-122">此外，该 `FunctionTailcall` 函数不得调入托管代码或以任何方式导致托管的内存分配。</span><span class="sxs-lookup"><span data-stu-id="1eb95-122">Also, the `FunctionTailcall` function must not call into managed code or in any way cause a managed memory allocation.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="1eb95-123">要求</span><span class="sxs-lookup"><span data-stu-id="1eb95-123">Requirements</span></span>  

 <span data-ttu-id="1eb95-124">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="1eb95-124">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="1eb95-125">**标头：** Corprof.idl .idl</span><span class="sxs-lookup"><span data-stu-id="1eb95-125">**Header:** CorProf.idl</span></span>  
  
 <span data-ttu-id="1eb95-126">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="1eb95-126">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="1eb95-127">**.NET Framework 版本：** 1.1、1。0</span><span class="sxs-lookup"><span data-stu-id="1eb95-127">**.NET Framework Versions:** 1.1, 1.0</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="1eb95-128">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1eb95-128">See also</span></span>

- [<span data-ttu-id="1eb95-129">FunctionEnter2 函数</span><span class="sxs-lookup"><span data-stu-id="1eb95-129">FunctionEnter2 Function</span></span>](functionenter2-function.md)
- [<span data-ttu-id="1eb95-130">FunctionLeave2 函数</span><span class="sxs-lookup"><span data-stu-id="1eb95-130">FunctionLeave2 Function</span></span>](functionleave2-function.md)
- [<span data-ttu-id="1eb95-131">SetEnterLeaveFunctionHooks2 方法</span><span class="sxs-lookup"><span data-stu-id="1eb95-131">SetEnterLeaveFunctionHooks2 Method</span></span>](icorprofilerinfo2-setenterleavefunctionhooks2-method.md)
- [<span data-ttu-id="1eb95-132">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="1eb95-132">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)
