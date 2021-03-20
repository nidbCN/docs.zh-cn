---
description: 了解详细信息： ICorProfilerCallback：： AssemblyUnloadFinished 方法
title: ICorProfilerCallback::AssemblyUnloadFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AssemblyUnloadFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AssemblyUnloadFinished
helpviewer_keywords:
- AssemblyUnloadFinished method [.NET Framework profiling]
- ICorProfilerCallback::AssemblyUnloadFinished method [.NET Framework profiling]
ms.assetid: 53fca564-84b1-44d4-9e21-17a492d2aae7
topic_type:
- apiref
ms.openlocfilehash: c3c87644602ede3b849d13624b0cc0a2df71e2fd
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760648"
---
# <a name="icorprofilercallbackassemblyunloadfinished-method"></a><span data-ttu-id="d3445-103">ICorProfilerCallback::AssemblyUnloadFinished 方法</span><span class="sxs-lookup"><span data-stu-id="d3445-103">ICorProfilerCallback::AssemblyUnloadFinished Method</span></span>

<span data-ttu-id="d3445-104">通知探查器已卸载程序集。</span><span class="sxs-lookup"><span data-stu-id="d3445-104">Notifies the profiler that an assembly has been unloaded.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="d3445-105">语法</span><span class="sxs-lookup"><span data-stu-id="d3445-105">Syntax</span></span>  
  
```cpp  
HRESULT AssemblyUnloadFinished(  
    [in] AssemblyID assemblyId,  
    [in] HRESULT    hrStatus);  
```  
  
## <a name="parameters"></a><span data-ttu-id="d3445-106">parameters</span><span class="sxs-lookup"><span data-stu-id="d3445-106">Parameters</span></span>

<span data-ttu-id="d3445-107">`assemblyId` 中标识正在卸载的程序集。</span><span class="sxs-lookup"><span data-stu-id="d3445-107">`assemblyId` [in] Identifies the assembly that is being unloaded.</span></span>

<span data-ttu-id="d3445-108">`hrStatus` 中一个 HRESULT，指示程序集是否已成功卸载。</span><span class="sxs-lookup"><span data-stu-id="d3445-108">`hrStatus` [in] An HRESULT that indicates whether the assembly was unloaded successfully.</span></span>

## <a name="remarks"></a><span data-ttu-id="d3445-109">备注</span><span class="sxs-lookup"><span data-stu-id="d3445-109">Remarks</span></span>  

 <span data-ttu-id="d3445-110">在 `assemblyId` [ICorProfilerCallback：： AssemblyUnloadStarted](icorprofilercallback-assemblyunloadstarted-method.md) 方法返回后，的值对信息请求无效。</span><span class="sxs-lookup"><span data-stu-id="d3445-110">The value of `assemblyId` is not valid for an information request after the [ICorProfilerCallback::AssemblyUnloadStarted](icorprofilercallback-assemblyunloadstarted-method.md) method returns.</span></span>  
  
 <span data-ttu-id="d3445-111">在回调后，卸载程序集的某些部分可能会继续 `AssemblyUnloadFinished` 。</span><span class="sxs-lookup"><span data-stu-id="d3445-111">Some parts of unloading the assembly might continue after the `AssemblyUnloadFinished` callback.</span></span> <span data-ttu-id="d3445-112">中的 HRESULT 失败 `hrStatus` 表示失败。</span><span class="sxs-lookup"><span data-stu-id="d3445-112">A failure HRESULT in `hrStatus` indicates a failure.</span></span> <span data-ttu-id="d3445-113">但是，中的成功 HRESULT `hrStatus` 仅指示卸载程序集的第一部分已成功。</span><span class="sxs-lookup"><span data-stu-id="d3445-113">However, a success HRESULT in `hrStatus` indicates only that the first part of unloading the assembly has succeeded.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="d3445-114">要求</span><span class="sxs-lookup"><span data-stu-id="d3445-114">Requirements</span></span>  

 <span data-ttu-id="d3445-115">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="d3445-115">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="d3445-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="d3445-116">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="d3445-117">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="d3445-117">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="d3445-118">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="d3445-118">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="d3445-119">另请参阅</span><span class="sxs-lookup"><span data-stu-id="d3445-119">See also</span></span>

- [<span data-ttu-id="d3445-120">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="d3445-120">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
