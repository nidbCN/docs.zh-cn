---
description: 了解详细信息： ICorProfilerCallback9：:D ynamicMethodUnloaded 方法
title: ICorProfilerCallback9：:D ynamicMethodUnloaded 方法
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback9.DynamicMethodUnloaded
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: bc7f95b5101658c93eeb9fcef51e9c0f1bd2f2bd
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760192"
---
# <a name="icorprofilercallback9dynamicmethodunloaded-method"></a><span data-ttu-id="e3d26-103">ICorProfilerCallback9：:D ynamicMethodUnloaded 方法</span><span class="sxs-lookup"><span data-stu-id="e3d26-103">ICorProfilerCallback9::DynamicMethodUnloaded Method</span></span>

<span data-ttu-id="e3d26-104">[.NET Framework 4.7.2 和更高版本中支持]</span><span class="sxs-lookup"><span data-stu-id="e3d26-104">[Supported in the .NET Framework 4.7.2 and later versions]</span></span>  
  
<span data-ttu-id="e3d26-105">每当对动态方法进行垃圾回收并随后卸载时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="e3d26-105">Notifies the profiler whenever a dynamic method is garbage collected and subsequently unloaded.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="e3d26-106">语法</span><span class="sxs-lookup"><span data-stu-id="e3d26-106">Syntax</span></span>  
  
```cpp  
HRESULT DynamicMethodUnloaded(  
     [in]  FunctionID  functionId
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="e3d26-107">parameters</span><span class="sxs-lookup"><span data-stu-id="e3d26-107">Parameters</span></span>  

<span data-ttu-id="e3d26-108">`functionId` 中已被垃圾回收和卸载的内存中函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="e3d26-108">`functionId` [in] The identifier of the in-memory function that has been garbage collected and unloaded.</span></span>

## <a name="requirements"></a><span data-ttu-id="e3d26-109">要求</span><span class="sxs-lookup"><span data-stu-id="e3d26-109">Requirements</span></span>  

 <span data-ttu-id="e3d26-110">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="e3d26-110">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="e3d26-111">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="e3d26-111">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="e3d26-112">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="e3d26-112">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="e3d26-113">**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]</span><span class="sxs-lookup"><span data-stu-id="e3d26-113">**.NET Framework Versions:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="e3d26-114">另请参阅</span><span class="sxs-lookup"><span data-stu-id="e3d26-114">See also</span></span>

- [<span data-ttu-id="e3d26-115">ICorProfilerCallback8. DynamicMethodJITCompilationStarted 方法</span><span class="sxs-lookup"><span data-stu-id="e3d26-115">ICorProfilerCallback8.DynamicMethodJITCompilationStarted Method</span></span>](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)
- [<span data-ttu-id="e3d26-116">ICorProfilerCallback8. DynamicMethodJITCompilationFinished 方法</span><span class="sxs-lookup"><span data-stu-id="e3d26-116">ICorProfilerCallback8.DynamicMethodJITCompilationFinished Method</span></span>](icorprofilercallback8-dynamicmethodjitcompilationfinished-method.md)
- [<span data-ttu-id="e3d26-117">ICorProfilerCallback9 接口</span><span class="sxs-lookup"><span data-stu-id="e3d26-117">ICorProfilerCallback9 Interface</span></span>](icorprofilercallback9-interface.md)
- [<span data-ttu-id="e3d26-118">COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS</span><span class="sxs-lookup"><span data-stu-id="e3d26-118">COR_PRF_HIGH_MONITOR_DYNAMIC_FUNCTION_UNLOADS</span></span>](cor-prf-high-monitor-enumeration.md)
