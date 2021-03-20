---
description: 了解详细信息： ICorProfilerCallback：： ExceptionThrown 方法
title: ICorProfilerCallback::ExceptionThrown 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ExceptionThrown
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ExceptionThrown
helpviewer_keywords:
- ExceptionThrown method [.NET Framework profiling]
- ICorProfilerCallback::ExceptionThrown method [.NET Framework profiling]
ms.assetid: f1a23f3b-ac21-4905-8abf-8ea59f15af53
topic_type:
- apiref
ms.openlocfilehash: aed3d6c4c3c45b546fa8af1db918a6f68eda9bde
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760400"
---
# <a name="icorprofilercallbackexceptionthrown-method"></a><span data-ttu-id="d482f-103">ICorProfilerCallback::ExceptionThrown 方法</span><span class="sxs-lookup"><span data-stu-id="d482f-103">ICorProfilerCallback::ExceptionThrown Method</span></span>

<span data-ttu-id="d482f-104">通知探查器引发了异常。</span><span class="sxs-lookup"><span data-stu-id="d482f-104">Notifies the profiler that an exception has been thrown.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="d482f-105">仅当异常到达托管代码时，才调用此函数。</span><span class="sxs-lookup"><span data-stu-id="d482f-105">This function is called only if the exception reaches managed code.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="d482f-106">语法</span><span class="sxs-lookup"><span data-stu-id="d482f-106">Syntax</span></span>  
  
```cpp  
HRESULT ExceptionThrown(  
    [in] ObjectID thrownObjectId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="d482f-107">parameters</span><span class="sxs-lookup"><span data-stu-id="d482f-107">Parameters</span></span>

<span data-ttu-id="d482f-108">`thrownObjectId` 中导致引发异常的对象的 ID。</span><span class="sxs-lookup"><span data-stu-id="d482f-108">`thrownObjectId` [in] The ID of the object that caused the exception to be thrown.</span></span>
  
## <a name="remarks"></a><span data-ttu-id="d482f-109">备注</span><span class="sxs-lookup"><span data-stu-id="d482f-109">Remarks</span></span>  

 <span data-ttu-id="d482f-110">探查器不应在此方法的实现中被阻止，因为堆栈可能不处于允许垃圾回收的状态，因此无法启用抢先垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="d482f-110">The profiler should not block in its implementation of this method because the stack may not be in a state that allows garbage collection, and therefore preemptive garbage collection cannot be enabled.</span></span> <span data-ttu-id="d482f-111">如果探查器在此处阻止并且试图进行垃圾回收，则运行时将被阻止，直到此回调返回。</span><span class="sxs-lookup"><span data-stu-id="d482f-111">If the profiler blocks here and garbage collection is attempted, the runtime will block until this callback returns.</span></span>  
  
 <span data-ttu-id="d482f-112">探查器的此方法的实现不应调入托管代码或以任何方式导致托管内存分配。</span><span class="sxs-lookup"><span data-stu-id="d482f-112">The profiler's implementation of this method should not call into managed code or in any way cause a managed-memory allocation.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="d482f-113">要求</span><span class="sxs-lookup"><span data-stu-id="d482f-113">Requirements</span></span>  

 <span data-ttu-id="d482f-114">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="d482f-114">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="d482f-115">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="d482f-115">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="d482f-116">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="d482f-116">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="d482f-117">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="d482f-117">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="d482f-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="d482f-118">See also</span></span>

- [<span data-ttu-id="d482f-119">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="d482f-119">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
