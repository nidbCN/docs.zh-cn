---
description: 了解详细信息： ICorProfilerCallback：： ExceptionCatcherEnter 方法
title: ICorProfilerCallback::ExceptionCatcherEnter 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionCatcherEnter
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionCatcherEnter
helpviewer_keywords:
- ICorProfilerCallback::ExceptionCatcherEnter method [.NET Framework profiling]
- ExceptionCatcherEnter method [.NET Framework profiling]
ms.assetid: 41462329-a648-46f0-ae6d-728b94c31aa9
topic_type:
- apiref
ms.openlocfilehash: f9ea2b44e7a783f9b21f4aa385585dfebc48b1d4
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760517"
---
# <a name="icorprofilercallbackexceptioncatcherenter-method"></a><span data-ttu-id="10460-103">ICorProfilerCallback::ExceptionCatcherEnter 方法</span><span class="sxs-lookup"><span data-stu-id="10460-103">ICorProfilerCallback::ExceptionCatcherEnter Method</span></span>

<span data-ttu-id="10460-104">通知探查器控制正在传递到适当的 `catch` 块。</span><span class="sxs-lookup"><span data-stu-id="10460-104">Notifies the profiler that control is being passed to the appropriate `catch` block.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="10460-105">语法</span><span class="sxs-lookup"><span data-stu-id="10460-105">Syntax</span></span>  
  
```cpp  
HRESULT ExceptionCatcherEnter(  
    [in] FunctionID functionId,  
    [in] ObjectID   objectId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="10460-106">parameters</span><span class="sxs-lookup"><span data-stu-id="10460-106">Parameters</span></span>

<span data-ttu-id="10460-107">`functionId` 中包含该块的函数的标识符 `catch` 。</span><span class="sxs-lookup"><span data-stu-id="10460-107">`functionId` [in] The identifier of the function containing the `catch` block.</span></span>
  
<span data-ttu-id="10460-108">`objectId` 中正在处理的异常的标识符。</span><span class="sxs-lookup"><span data-stu-id="10460-108">`objectId` [in] The identifier of the exception being handled.</span></span>

## <a name="remarks"></a><span data-ttu-id="10460-109">备注</span><span class="sxs-lookup"><span data-stu-id="10460-109">Remarks</span></span>  

 <span data-ttu-id="10460-110">`ExceptionCatcherEnter`仅当 catch 点位于用实时 (JIT) 编译器编译的代码中时，才会调用方法。</span><span class="sxs-lookup"><span data-stu-id="10460-110">The `ExceptionCatcherEnter` method is called only if the catch point is in code compiled with the just-in-time (JIT) compiler.</span></span> <span data-ttu-id="10460-111">在非托管代码中或在运行时的内部代码中捕获的异常将不会调用此通知。</span><span class="sxs-lookup"><span data-stu-id="10460-111">An exception that is caught in unmanaged code or in the internal code of the runtime will not call this notification.</span></span> <span data-ttu-id="10460-112">由于 `objectId` 垃圾回收可能已在通知后移动了对象，因此会再次传递该值 `ExceptionThrown` 。</span><span class="sxs-lookup"><span data-stu-id="10460-112">The `objectId` value is passed again since a garbage collection could have moved the object since the `ExceptionThrown` notification.</span></span>  
  
 <span data-ttu-id="10460-113">探查器不应在此方法的实现中被阻止，因为堆栈可能不处于允许垃圾回收的状态，因此无法启用抢先垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="10460-113">The profiler should not block in its implementation of this method because the stack may not be in a state that allows garbage collection, and therefore preemptive garbage collection cannot be enabled.</span></span> <span data-ttu-id="10460-114">如果探查器在此处阻止并且试图进行垃圾回收，则运行时将被阻止，直到此回调返回。</span><span class="sxs-lookup"><span data-stu-id="10460-114">If the profiler blocks here and garbage collection is attempted, the runtime will block until this callback returns.</span></span>  
  
 <span data-ttu-id="10460-115">探查器的此方法的实现不应调入托管代码或以任何方式导致托管内存分配。</span><span class="sxs-lookup"><span data-stu-id="10460-115">The profiler's implementation of this method should not call into managed code or in any way cause a managed-memory allocation.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="10460-116">要求</span><span class="sxs-lookup"><span data-stu-id="10460-116">Requirements</span></span>  

 <span data-ttu-id="10460-117">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="10460-117">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="10460-118">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="10460-118">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="10460-119">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="10460-119">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="10460-120">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="10460-120">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="10460-121">另请参阅</span><span class="sxs-lookup"><span data-stu-id="10460-121">See also</span></span>

- [<span data-ttu-id="10460-122">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="10460-122">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="10460-123">ExceptionCatcherLeave 方法</span><span class="sxs-lookup"><span data-stu-id="10460-123">ExceptionCatcherLeave Method</span></span>](icorprofilercallback-exceptioncatcherleave-method.md)
