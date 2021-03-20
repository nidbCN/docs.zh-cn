---
description: 了解详细信息： ICorProfilerCallback：： AssemblyUnloadStarted 方法
title: ICorProfilerCallback::AssemblyUnloadStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AssemblyUnloadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AssemblyUnloadStarted
helpviewer_keywords:
- ICorProfilerCallback::AssemblyUnloadStarted method [.NET Framework profiling]
- AssemblyUnloadStarted method [.NET Framework profiling]
ms.assetid: 6e47b7e5-0335-4dd3-8c42-d3c07d62b102
topic_type:
- apiref
ms.openlocfilehash: 91c0b6a600a1c7c12905a7a9817e6e7e9601c3c0
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759464"
---
# <a name="icorprofilercallbackassemblyunloadstarted-method"></a><span data-ttu-id="b1bba-103">ICorProfilerCallback::AssemblyUnloadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="b1bba-103">ICorProfilerCallback::AssemblyUnloadStarted Method</span></span>

<span data-ttu-id="b1bba-104">通知探查器正在卸载程序集。</span><span class="sxs-lookup"><span data-stu-id="b1bba-104">Notifies the profiler that an assembly is being unloaded.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="b1bba-105">语法</span><span class="sxs-lookup"><span data-stu-id="b1bba-105">Syntax</span></span>  
  
```cpp  
HRESULT AssemblyUnloadStarted(  
    [in] AssemblyID assemblyId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="b1bba-106">parameters</span><span class="sxs-lookup"><span data-stu-id="b1bba-106">Parameters</span></span>

<span data-ttu-id="b1bba-107">`assemblyId` 中标识正在卸载的程序集。</span><span class="sxs-lookup"><span data-stu-id="b1bba-107">`assemblyId` [in] Identifies the assembly that is being unloaded.</span></span>

## <a name="remarks"></a><span data-ttu-id="b1bba-108">备注</span><span class="sxs-lookup"><span data-stu-id="b1bba-108">Remarks</span></span>  

 <span data-ttu-id="b1bba-109">在 `assemblyId` 方法返回后，的值对信息请求无效 `AssemblyUnloadStarted` -这是探查器获取有关此程序集的相关信息的最后机会。</span><span class="sxs-lookup"><span data-stu-id="b1bba-109">The value of `assemblyId` is not valid for an information request after the `AssemblyUnloadStarted` method returns — this is the profiler's last chance to get information about this assembly.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="b1bba-110">要求</span><span class="sxs-lookup"><span data-stu-id="b1bba-110">Requirements</span></span>  

 <span data-ttu-id="b1bba-111">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="b1bba-111">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="b1bba-112">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="b1bba-112">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="b1bba-113">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="b1bba-113">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="b1bba-114">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="b1bba-114">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="b1bba-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="b1bba-115">See also</span></span>

- [<span data-ttu-id="b1bba-116">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="b1bba-116">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="b1bba-117">AssemblyUnloadFinished 方法</span><span class="sxs-lookup"><span data-stu-id="b1bba-117">AssemblyUnloadFinished Method</span></span>](icorprofilercallback-assemblyunloadfinished-method.md)
