---
description: 了解详细信息： ICorProfilerCallback：： AssemblyLoadStarted 方法
title: ICorProfilerCallback::AssemblyLoadStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AssemblyLoadStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AssemblyLoadStarted
helpviewer_keywords:
- ICorProfilerCallback::AssemblyLoadStarted method [.NET Framework profiling]
- AssemblyLoadStarted method [.NET Framework profiling]
ms.assetid: 67e8209d-a0ca-4118-a6e6-c1ee0abc2221
topic_type:
- apiref
ms.openlocfilehash: 19737fc284d49c3690f66e4881450ca5803622e3
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760595"
---
# <a name="icorprofilercallbackassemblyloadstarted-method"></a><span data-ttu-id="d5741-103">ICorProfilerCallback::AssemblyLoadStarted 方法</span><span class="sxs-lookup"><span data-stu-id="d5741-103">ICorProfilerCallback::AssemblyLoadStarted Method</span></span>

<span data-ttu-id="d5741-104">通知探查器正在加载程序集。</span><span class="sxs-lookup"><span data-stu-id="d5741-104">Notifies the profiler that an assembly is being loaded.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="d5741-105">语法</span><span class="sxs-lookup"><span data-stu-id="d5741-105">Syntax</span></span>  
  
```cpp  
HRESULT AssemblyLoadStarted(  
    [in] AssemblyID assemblyId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="d5741-106">parameters</span><span class="sxs-lookup"><span data-stu-id="d5741-106">Parameters</span></span>

<span data-ttu-id="d5741-107">`assemblyId` 中标识要加载的程序集。</span><span class="sxs-lookup"><span data-stu-id="d5741-107">`assemblyId` [in] Identifies the assembly that is being loaded.</span></span>

## <a name="remarks"></a><span data-ttu-id="d5741-108">备注</span><span class="sxs-lookup"><span data-stu-id="d5741-108">Remarks</span></span>  

 <span data-ttu-id="d5741-109">在 `assemblyId` 调用 [ICorProfilerCallback：： AssemblyLoadFinished](icorprofilercallback-assemblyloadfinished-method.md) 方法之前，的值对信息请求无效。</span><span class="sxs-lookup"><span data-stu-id="d5741-109">The value of `assemblyId` is not valid for an information request until the [ICorProfilerCallback::AssemblyLoadFinished](icorprofilercallback-assemblyloadfinished-method.md) method is called.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="d5741-110">要求</span><span class="sxs-lookup"><span data-stu-id="d5741-110">Requirements</span></span>  

 <span data-ttu-id="d5741-111">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="d5741-111">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="d5741-112">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="d5741-112">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="d5741-113">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="d5741-113">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="d5741-114">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="d5741-114">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="d5741-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="d5741-115">See also</span></span>

- [<span data-ttu-id="d5741-116">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="d5741-116">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
