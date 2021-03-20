---
description: 了解详细信息： ICorProfilerCallback：： ClassLoadFinished 方法
title: ICorProfilerCallback::ClassLoadFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ClassLoadFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ClassLoadFinished
helpviewer_keywords:
- ClassLoadFinished method [.NET Framework profiling]
- ICorProfilerCallback::ClassLoadFinished method [.NET Framework profiling]
ms.assetid: 3dd80fbe-d62d-4d4d-acf8-5b7d0efe607e
topic_type:
- apiref
ms.openlocfilehash: 52fd26c1efaf9b85caeb5af7184ae70e1d29b9ff
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760207"
---
# <a name="icorprofilercallbackclassloadfinished-method"></a><span data-ttu-id="2a951-103">ICorProfilerCallback::ClassLoadFinished 方法</span><span class="sxs-lookup"><span data-stu-id="2a951-103">ICorProfilerCallback::ClassLoadFinished Method</span></span>

<span data-ttu-id="2a951-104">通知探查器类已经完成加载。</span><span class="sxs-lookup"><span data-stu-id="2a951-104">Notifies the profiler that a class has finished loading.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="2a951-105">语法</span><span class="sxs-lookup"><span data-stu-id="2a951-105">Syntax</span></span>  
  
```cpp  
HRESULT ClassLoadFinished(  
    [in] ClassID classId,  
    [in] HRESULT hrStatus);  
```  
  
## <a name="parameters"></a><span data-ttu-id="2a951-106">parameters</span><span class="sxs-lookup"><span data-stu-id="2a951-106">Parameters</span></span>

<span data-ttu-id="2a951-107">`classId` 中标识已加载的类。</span><span class="sxs-lookup"><span data-stu-id="2a951-107">`classId` [in] Identifies the class that was loaded.</span></span>

<span data-ttu-id="2a951-108">`hrStatus` 中一个 HRESULT，指示类是否已成功加载。</span><span class="sxs-lookup"><span data-stu-id="2a951-108">`hrStatus` [in] An HRESULT that indicates whether the class loaded successfully.</span></span>

## <a name="remarks"></a><span data-ttu-id="2a951-109">备注</span><span class="sxs-lookup"><span data-stu-id="2a951-109">Remarks</span></span>  

 <span data-ttu-id="2a951-110">在 `classId` 调用方法之前，的值对信息请求无效 `ClassLoadFinished` 。</span><span class="sxs-lookup"><span data-stu-id="2a951-110">The value of `classId` is not valid for an information request until the `ClassLoadFinished` method is called.</span></span>  
  
 <span data-ttu-id="2a951-111">在回调后，某些加载类的部分可能会继续 `ClassLoadFinished` 。</span><span class="sxs-lookup"><span data-stu-id="2a951-111">Some parts of loading the class might continue after the `ClassLoadFinished` callback.</span></span> <span data-ttu-id="2a951-112">中的 HRESULT 失败 `hrStatus` 表示失败。</span><span class="sxs-lookup"><span data-stu-id="2a951-112">A failure HRESULT in `hrStatus` indicates a failure.</span></span> <span data-ttu-id="2a951-113">但是，中的成功 HRESULT `hrStatus` 仅指示加载类的第一部分已成功。</span><span class="sxs-lookup"><span data-stu-id="2a951-113">However, a success HRESULT in `hrStatus` indicates only that the first part of loading the class has succeeded.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="2a951-114">要求</span><span class="sxs-lookup"><span data-stu-id="2a951-114">Requirements</span></span>  

 <span data-ttu-id="2a951-115">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="2a951-115">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="2a951-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="2a951-116">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="2a951-117">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="2a951-117">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="2a951-118">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="2a951-118">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="2a951-119">另请参阅</span><span class="sxs-lookup"><span data-stu-id="2a951-119">See also</span></span>

- [<span data-ttu-id="2a951-120">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="2a951-120">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="2a951-121">ClassLoadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="2a951-121">ClassLoadStarted Method</span></span>](icorprofilercallback-classloadstarted-method.md)
