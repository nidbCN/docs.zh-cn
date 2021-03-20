---
description: 了解详细信息： ICorProfilerCallback：： ClassUnloadStarted 方法
title: ICorProfilerCallback::ClassUnloadStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.ClassUnloadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::ClassUnloadStarted
helpviewer_keywords:
- ClassUnloadStarted method [.NET Framework profiling]
- ICorProfilerCallback::ClassUnloadStarted method [.NET Framework profiling]
ms.assetid: bc93bead-f3a9-415c-b919-ddd3ca80facc
topic_type:
- apiref
ms.openlocfilehash: 91de255b2214ad0c6ce6911d9533df593142a191
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760655"
---
# <a name="icorprofilercallbackclassunloadstarted-method"></a><span data-ttu-id="a0270-103">ICorProfilerCallback::ClassUnloadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="a0270-103">ICorProfilerCallback::ClassUnloadStarted Method</span></span>

<span data-ttu-id="a0270-104">通知探查器正在卸载某个类。</span><span class="sxs-lookup"><span data-stu-id="a0270-104">Notifies the profiler that a class is being unloaded.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="a0270-105">语法</span><span class="sxs-lookup"><span data-stu-id="a0270-105">Syntax</span></span>  
  
```cpp  
HRESULT ClassUnloadStarted(  
    [in] ClassID classId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="a0270-106">parameters</span><span class="sxs-lookup"><span data-stu-id="a0270-106">Parameters</span></span>

<span data-ttu-id="a0270-107">`classId` 中标识正在卸载的类。</span><span class="sxs-lookup"><span data-stu-id="a0270-107">`classId` [in] Identifies the class that is being unloaded.</span></span>

## <a name="remarks"></a><span data-ttu-id="a0270-108">备注</span><span class="sxs-lookup"><span data-stu-id="a0270-108">Remarks</span></span>  

 <span data-ttu-id="a0270-109">在 `classId` 方法返回后，的值对信息请求无效 `ClassUnloadStarted` -这是探查器获取有关此类的信息的最后机会。</span><span class="sxs-lookup"><span data-stu-id="a0270-109">The value of `classId` is not valid for an information request after the `ClassUnloadStarted` method returns — this is the profiler's last chance to obtain information about this class.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="a0270-110">要求</span><span class="sxs-lookup"><span data-stu-id="a0270-110">Requirements</span></span>  

 <span data-ttu-id="a0270-111">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="a0270-111">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="a0270-112">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="a0270-112">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="a0270-113">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="a0270-113">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="a0270-114">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="a0270-114">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="a0270-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="a0270-115">See also</span></span>

- [<span data-ttu-id="a0270-116">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="a0270-116">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="a0270-117">ClassUnloadFinished 方法</span><span class="sxs-lookup"><span data-stu-id="a0270-117">ClassUnloadFinished Method</span></span>](icorprofilercallback-classunloadfinished-method.md)
