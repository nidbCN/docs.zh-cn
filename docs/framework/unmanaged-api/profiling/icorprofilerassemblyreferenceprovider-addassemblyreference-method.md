---
description: 了解详细信息： ICorProfilerAssemblyReferenceProvider：： AddAssemblyReference 方法
title: ICorProfilerAssemblyReferenceProvider::AddAssemblyReference 方法
ms.date: 03/30/2017
dev_langs:
- cpp
api_name:
- ICorProfilerAssemblyReferenceProvider.AddAssemblyReference
api_location:
- mscorwks.dll
api_type:
- COM
ms.assetid: 3d5af8e7-c337-48f4-9fa6-97c83878b9b1
topic_type:
- apiref
ms.openlocfilehash: e73a6d59b76744dcf9f4991be2589220669e154d
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760739"
---
# <a name="icorprofilerassemblyreferenceprovideraddassemblyreference-method"></a><span data-ttu-id="f7cbd-103">ICorProfilerAssemblyReferenceProvider::AddAssemblyReference 方法</span><span class="sxs-lookup"><span data-stu-id="f7cbd-103">ICorProfilerAssemblyReferenceProvider::AddAssemblyReference Method</span></span>

<span data-ttu-id="f7cbd-104">[仅在 .NET Framework 4.5.2 及更高版本中受支持]</span><span class="sxs-lookup"><span data-stu-id="f7cbd-104">[Supported in the .NET Framework 4.5.2 and later versions]</span></span>  
  
 <span data-ttu-id="f7cbd-105">向公共语言运行 (时通知) 探查器计划在 [ICorProfilerCallback：： ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) 回调中添加的程序集引用的 CLR。</span><span class="sxs-lookup"><span data-stu-id="f7cbd-105">Informs the common language runtime (CLR) of an assembly reference that the profiler plans to add in the [ICorProfilerCallback::ModuleLoadFinished](icorprofilercallback-moduleloadfinished-method.md) callback.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="f7cbd-106">语法</span><span class="sxs-lookup"><span data-stu-id="f7cbd-106">Syntax</span></span>  
  
```cpp
HRESULT AddAssemblyReference(  
        const COR_PRF_ASSEMBLY_REFERENCE_INFO* pAssemblyRefInfo  
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="f7cbd-107">parameters</span><span class="sxs-lookup"><span data-stu-id="f7cbd-107">Parameters</span></span>

<span data-ttu-id="f7cbd-108">`pAssemblyRefInfo` 指向 [COR_PRF_ASSEMBLY_REFERENCE_INFO](cor-prf-assembly-reference-info-structure.md) 结构的指针，该结构为 CLR 提供有关在执行程序集引用闭包审核时应考虑的程序集引用的信息。</span><span class="sxs-lookup"><span data-stu-id="f7cbd-108">`pAssemblyRefInfo` A pointer to a [COR_PRF_ASSEMBLY_REFERENCE_INFO](cor-prf-assembly-reference-info-structure.md) structure that provides the CLR with information about an assembly reference that it should consider when performing an assembly reference closure walk.</span></span>
  
## <a name="remarks"></a><span data-ttu-id="f7cbd-109">备注</span><span class="sxs-lookup"><span data-stu-id="f7cbd-109">Remarks</span></span>  

 <span data-ttu-id="f7cbd-110">探查器为其计划从 `wszAssemblyPath` [ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) 回调的参数中指定的程序集引用的每个目标程序集调用此方法。</span><span class="sxs-lookup"><span data-stu-id="f7cbd-110">The profiler calls this method for each target assembly it plans to reference from the assembly specified in the `wszAssemblyPath` argument of the [ICorProfilerCallback6::GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) callback.</span></span> <span data-ttu-id="f7cbd-111">[ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md)接口对象传递给探查器的[ICorProfilerCallback6：： GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md)回调，同时传递给参数中的程序集路径和名称 `wszAssemblyPath` 。</span><span class="sxs-lookup"><span data-stu-id="f7cbd-111">The [ICorProfilerAssemblyReferenceProvider](icorprofilerassemblyreferenceprovider-interface.md) interface object is passed to the profiler's [ICorProfilerCallback6::GetAssemblyReferences](icorprofilercallback6-getassemblyreferences-method.md) callback, along with the assembly path and name in the `wszAssemblyPath` argument.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="f7cbd-112">要求</span><span class="sxs-lookup"><span data-stu-id="f7cbd-112">Requirements</span></span>  

 <span data-ttu-id="f7cbd-113">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="f7cbd-113">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="f7cbd-114">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="f7cbd-114">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="f7cbd-115">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="f7cbd-115">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="f7cbd-116">**.NET Framework 版本：**[!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="f7cbd-116">**.NET Framework Versions:** [!INCLUDE[net_current_v452plus](../../../../includes/net-current-v452plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="f7cbd-117">另请参阅</span><span class="sxs-lookup"><span data-stu-id="f7cbd-117">See also</span></span>

- [<span data-ttu-id="f7cbd-118">ICorProfilerAssemblyReferenceProvider 方法</span><span class="sxs-lookup"><span data-stu-id="f7cbd-118">ICorProfilerAssemblyReferenceProvider Interface</span></span>](icorprofilerassemblyreferenceprovider-interface.md)
- [<span data-ttu-id="f7cbd-119">GetAssemblyReferences 方法</span><span class="sxs-lookup"><span data-stu-id="f7cbd-119">GetAssemblyReferences Method</span></span>](icorprofilercallback6-getassemblyreferences-method.md)
- [<span data-ttu-id="f7cbd-120">ModuleLoadFinished 方法</span><span class="sxs-lookup"><span data-stu-id="f7cbd-120">ModuleLoadFinished Method</span></span>](icorprofilercallback-moduleloadfinished-method.md)
