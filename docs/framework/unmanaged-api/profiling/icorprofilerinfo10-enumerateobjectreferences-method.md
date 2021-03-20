---
description: 了解详细信息： ICorProfilerInfo10：： EnumerateObjectReferences 方法
title: ICorProfilerInfo10::EnumerateObjectReferences
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo10.EnumerateObjectReferences
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: bcd374aec2944977a0745177995ba8adf0cce9b7
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759412"
---
# <a name="icorprofilerinfo10enumerateobjectreferences-method"></a><span data-ttu-id="c3f93-103">ICorProfilerInfo10：： EnumerateObjectReferences 方法</span><span class="sxs-lookup"><span data-stu-id="c3f93-103">ICorProfilerInfo10::EnumerateObjectReferences Method</span></span>

<span data-ttu-id="c3f93-104">给定 ObjectID、callback 和 clientData，如果有任何) ，则枚举 (的每个对象引用。</span><span class="sxs-lookup"><span data-stu-id="c3f93-104">Given an ObjectID, callback and clientData, enumerates each object reference (if any).</span></span>

## <a name="syntax"></a><span data-ttu-id="c3f93-105">语法</span><span class="sxs-lookup"><span data-stu-id="c3f93-105">Syntax</span></span>

```cpp
HRESULT EnumerateObjectReferences( [in] ObjectID objectId,
                                   [in] ObjectReferenceCallback callback,
                                   [in] void* clientData);
```

## <a name="parameters"></a><span data-ttu-id="c3f93-106">parameters</span><span class="sxs-lookup"><span data-stu-id="c3f93-106">Parameters</span></span>

<span data-ttu-id="c3f93-107">`objectId` 中要在其上枚举引用的对象。</span><span class="sxs-lookup"><span data-stu-id="c3f93-107">`objectId` [in] The object to enumerate references on.</span></span>

<span data-ttu-id="c3f93-108">`callback` 中将用对象的引用调用的函数。</span><span class="sxs-lookup"><span data-stu-id="c3f93-108">`callback` [in] The function that will be called with the references for the object.</span></span>

<span data-ttu-id="c3f93-109">`clientData` 中探查器提供的要传递给函数的数据 `callback` 。</span><span class="sxs-lookup"><span data-stu-id="c3f93-109">`clientData` [in] Profiler-provided data to pass to the `callback` function.</span></span>

## <a name="remarks"></a><span data-ttu-id="c3f93-110">备注</span><span class="sxs-lookup"><span data-stu-id="c3f93-110">Remarks</span></span>

<span data-ttu-id="c3f93-111">`EnumerateObjectReferences`方法类似于[ObjectReferences](icorprofilercallback-objectreferences-method.md)，只不过它会按需对探查器进行引用，而不是预先分配用于存储引用的数组。</span><span class="sxs-lookup"><span data-stu-id="c3f93-111">The `EnumerateObjectReferences` method is similar to [ObjectReferences](icorprofilercallback-objectreferences-method.md), except that it walks the references on demand for the profiler instead of pre-allocating an array to store the references.</span></span>

## <a name="requirements"></a><span data-ttu-id="c3f93-112">要求</span><span class="sxs-lookup"><span data-stu-id="c3f93-112">Requirements</span></span>

<span data-ttu-id="c3f93-113">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="c3f93-113">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>

<span data-ttu-id="c3f93-114">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="c3f93-114">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="c3f93-115">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="c3f93-115">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="c3f93-116">**.Net 版本：**[!INCLUDE[net_core_30](../../../../includes/net-core-30-md.md)]</span><span class="sxs-lookup"><span data-stu-id="c3f93-116">**.NET Versions:** [!INCLUDE[net_core_30](../../../../includes/net-core-30-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="c3f93-117">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c3f93-117">See also</span></span>

- [<span data-ttu-id="c3f93-118">ICorProfilerInfo10 接口</span><span class="sxs-lookup"><span data-stu-id="c3f93-118">ICorProfilerInfo10 Interface</span></span>](icorprofilerinfo10-interface.md)
