---
description: 了解详细信息： ICorProfilerCallback：： ExceptionUnwindFinallyEnter 方法
title: ICorProfilerCallback::ExceptionUnwindFinallyEnter 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionUnwindFinallyEnter
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionUnwindFinallyEnter
helpviewer_keywords:
- ICorProfilerCallback::ExceptionUnwindFinallyEnter method [.NET Framework profiling]
- ExceptionUnwindFinallyEnter method [.NET Framework profiling]
ms.assetid: c7fab986-b69f-4ec8-b7b7-91dcfc239cd0
topic_type:
- apiref
ms.openlocfilehash: a142aa39f1c601c8c814d2f760cb6414d84b8494
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759438"
---
# <a name="icorprofilercallbackexceptionunwindfinallyenter-method"></a><span data-ttu-id="478d8-103">ICorProfilerCallback::ExceptionUnwindFinallyEnter 方法</span><span class="sxs-lookup"><span data-stu-id="478d8-103">ICorProfilerCallback::ExceptionUnwindFinallyEnter Method</span></span>

<span data-ttu-id="478d8-104">通知探查器异常处理的展开阶段正在输入 `finally` 指定函数中包含的子句。</span><span class="sxs-lookup"><span data-stu-id="478d8-104">Notifies the profiler that the unwind phase of exception handling is entering a `finally` clause contained in the specified function.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="478d8-105">语法</span><span class="sxs-lookup"><span data-stu-id="478d8-105">Syntax</span></span>  
  
```cpp  
HRESULT ExceptionUnwindFinallyEnter(  
    [in] FunctionID functionId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="478d8-106">parameters</span><span class="sxs-lookup"><span data-stu-id="478d8-106">Parameters</span></span>

<span data-ttu-id="478d8-107">`functionId` 中包含子句的函数的 ID `finally` 。</span><span class="sxs-lookup"><span data-stu-id="478d8-107">`functionId` [in] The ID of the function that contains the `finally` clause.</span></span>

## <a name="remarks"></a><span data-ttu-id="478d8-108">备注</span><span class="sxs-lookup"><span data-stu-id="478d8-108">Remarks</span></span>  

 <span data-ttu-id="478d8-109">探查器不应在此方法的实现中被阻止，因为堆栈可能不处于允许垃圾回收的状态，因此无法启用抢先垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="478d8-109">The profiler should not block in its implementation of this method because the stack may not be in a state that allows garbage collection, and therefore preemptive garbage collection cannot be enabled.</span></span> <span data-ttu-id="478d8-110">如果探查器在此处阻止并且试图进行垃圾回收，则运行时将被阻止，直到此回调返回。</span><span class="sxs-lookup"><span data-stu-id="478d8-110">If the profiler blocks here and garbage collection is attempted, the runtime will block until this callback returns.</span></span>  
  
 <span data-ttu-id="478d8-111">探查器的此方法的实现不应调入托管代码或以任何方式导致托管内存分配。</span><span class="sxs-lookup"><span data-stu-id="478d8-111">The profiler's implementation of this method should not call into managed code or in any way cause a managed-memory allocation.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="478d8-112">要求</span><span class="sxs-lookup"><span data-stu-id="478d8-112">Requirements</span></span>  

 <span data-ttu-id="478d8-113">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="478d8-113">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="478d8-114">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="478d8-114">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="478d8-115">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="478d8-115">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="478d8-116">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="478d8-116">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="478d8-117">另请参阅</span><span class="sxs-lookup"><span data-stu-id="478d8-117">See also</span></span>

- [<span data-ttu-id="478d8-118">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="478d8-118">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="478d8-119">GetNotifiedExceptionClauseInfo 方法</span><span class="sxs-lookup"><span data-stu-id="478d8-119">GetNotifiedExceptionClauseInfo Method</span></span>](icorprofilerinfo2-getnotifiedexceptionclauseinfo-method.md)
- [<span data-ttu-id="478d8-120">ExceptionUnwindFinallyLeave 方法</span><span class="sxs-lookup"><span data-stu-id="478d8-120">ExceptionUnwindFinallyLeave Method</span></span>](icorprofilercallback-exceptionunwindfinallyleave-method.md)
