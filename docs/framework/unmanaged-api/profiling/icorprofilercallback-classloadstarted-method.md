---
description: 了解详细信息： ICorProfilerCallback：： ClassLoadStarted 方法
title: ICorProfilerCallback::ClassLoadStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ClassLoadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ClassLoadStarted
helpviewer_keywords:
- ICorProfilerCallback::ClassLoadStarted method [.NET Framework profiling]
- ClassLoadStarted method [.NET Framework profiling]
ms.assetid: 9f728de8-45c2-45a5-ac4a-45660bd36ecf
topic_type:
- apiref
ms.openlocfilehash: 4950f1d806efb304a860fa6fce18f8655bdc0976
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759233"
---
# <a name="icorprofilercallbackclassloadstarted-method"></a><span data-ttu-id="2fcc7-103">ICorProfilerCallback::ClassLoadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="2fcc7-103">ICorProfilerCallback::ClassLoadStarted Method</span></span>

<span data-ttu-id="2fcc7-104">通知探查器正在加载某个类。</span><span class="sxs-lookup"><span data-stu-id="2fcc7-104">Notifies the profiler that a class is being loaded.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="2fcc7-105">语法</span><span class="sxs-lookup"><span data-stu-id="2fcc7-105">Syntax</span></span>  
  
```cpp  
HRESULT ClassLoadStarted(  
    [in] ClassID classId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="2fcc7-106">parameters</span><span class="sxs-lookup"><span data-stu-id="2fcc7-106">Parameters</span></span>

<span data-ttu-id="2fcc7-107">`classId` 中标识要加载的类。</span><span class="sxs-lookup"><span data-stu-id="2fcc7-107">`classId` [in] Identifies the class that is being loaded.</span></span>

## <a name="remarks"></a><span data-ttu-id="2fcc7-108">备注</span><span class="sxs-lookup"><span data-stu-id="2fcc7-108">Remarks</span></span>  

 <span data-ttu-id="2fcc7-109">在 `classId` 调用 [ICorProfilerCallback：： ClassLoadFinished](icorprofilercallback-classloadfinished-method.md) 方法之前，的值对信息请求无效。</span><span class="sxs-lookup"><span data-stu-id="2fcc7-109">The value of `classId` is not valid for an information request until the [ICorProfilerCallback::ClassLoadFinished](icorprofilercallback-classloadfinished-method.md) method is called.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="2fcc7-110">要求</span><span class="sxs-lookup"><span data-stu-id="2fcc7-110">Requirements</span></span>  

 <span data-ttu-id="2fcc7-111">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="2fcc7-111">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="2fcc7-112">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="2fcc7-112">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="2fcc7-113">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="2fcc7-113">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="2fcc7-114">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="2fcc7-114">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="2fcc7-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="2fcc7-115">See also</span></span>

- [<span data-ttu-id="2fcc7-116">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="2fcc7-116">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
