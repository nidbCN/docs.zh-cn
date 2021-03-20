---
description: 了解详细信息： ICorProfilerCallback：： ClassUnloadFinished 方法
title: ICorProfilerCallback::ClassUnloadFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ClassUnloadFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ClassUnloadFinished
helpviewer_keywords:
- ICorProfilerCallback::ClassUnloadFinished method [.NET Framework profiling]
- ClassUnloadFinished method [.NET Framework profiling]
ms.assetid: 55674b68-678a-4747-ae06-4e91519c7305
topic_type:
- apiref
ms.openlocfilehash: 7d4fd1c85d496b7adea0096b03520a14c2fab11c
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759232"
---
# <a name="icorprofilercallbackclassunloadfinished-method"></a><span data-ttu-id="83099-103">ICorProfilerCallback::ClassUnloadFinished 方法</span><span class="sxs-lookup"><span data-stu-id="83099-103">ICorProfilerCallback::ClassUnloadFinished Method</span></span>

<span data-ttu-id="83099-104">通知探查器类已经完成卸载。</span><span class="sxs-lookup"><span data-stu-id="83099-104">Notifies the profiler that a class has finished unloading.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="83099-105">语法</span><span class="sxs-lookup"><span data-stu-id="83099-105">Syntax</span></span>  
  
```cpp  
HRESULT ClassUnloadFinished(  
    [in] ClassID classId,  
    [in] HRESULT hrStatus);  
```  
  
## <a name="parameters"></a><span data-ttu-id="83099-106">parameters</span><span class="sxs-lookup"><span data-stu-id="83099-106">Parameters</span></span>

<span data-ttu-id="83099-107">`classId` 中标识已卸载的类。</span><span class="sxs-lookup"><span data-stu-id="83099-107">`classId` [in] Identifies the class that was unloaded.</span></span>

<span data-ttu-id="83099-108">`hrStatus` 中指示是否已成功卸载类的 HRESULT。</span><span class="sxs-lookup"><span data-stu-id="83099-108">`hrStatus` [in] An HRESULT that indicates whether the class was unloaded successfully.</span></span>
  
## <a name="remarks"></a><span data-ttu-id="83099-109">备注</span><span class="sxs-lookup"><span data-stu-id="83099-109">Remarks</span></span>  

 <span data-ttu-id="83099-110">卸载类的某些部分可能会在回调后继续 `ClassUnloadFinished` 。</span><span class="sxs-lookup"><span data-stu-id="83099-110">Some parts of unloading the class might continue after the `ClassUnloadFinished` callback.</span></span> <span data-ttu-id="83099-111">中的 HRESULT 失败 `hrStatus` 表示失败。</span><span class="sxs-lookup"><span data-stu-id="83099-111">A failure HRESULT in `hrStatus` indicates a failure.</span></span> <span data-ttu-id="83099-112">但是，中的成功 HRESULT `hrStatus` 仅指示卸载类的第一部分已成功。</span><span class="sxs-lookup"><span data-stu-id="83099-112">However, a success HRESULT in `hrStatus` indicates only that the first part of unloading the class has succeeded.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="83099-113">要求</span><span class="sxs-lookup"><span data-stu-id="83099-113">Requirements</span></span>  

 <span data-ttu-id="83099-114">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="83099-114">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="83099-115">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="83099-115">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="83099-116">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="83099-116">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="83099-117">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="83099-117">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="83099-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="83099-118">See also</span></span>

- [<span data-ttu-id="83099-119">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="83099-119">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="83099-120">ClassUnloadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="83099-120">ClassUnloadStarted Method</span></span>](icorprofilercallback-classunloadstarted-method.md)
