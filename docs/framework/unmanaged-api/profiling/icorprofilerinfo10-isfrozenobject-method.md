---
description: 了解详细信息： ICorProfilerInfo10：： IsFrozenObject 方法
title: ICorProfilerInfo10::IsFrozenObject
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo10.IsFrozenObject
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: c4d31c96fd7470a153437ffb0125e81ca8ea77bd
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759750"
---
# <a name="icorprofilerinfo10isfrozenobject-method"></a><span data-ttu-id="f152b-103">ICorProfilerInfo10：： IsFrozenObject 方法</span><span class="sxs-lookup"><span data-stu-id="f152b-103">ICorProfilerInfo10::IsFrozenObject Method</span></span>

<span data-ttu-id="f152b-104">给定 ObjectID，确定对象是否位于只读段。</span><span class="sxs-lookup"><span data-stu-id="f152b-104">Given an ObjectID, determines whether the object is in a read-only segment.</span></span>

## <a name="syntax"></a><span data-ttu-id="f152b-105">语法</span><span class="sxs-lookup"><span data-stu-id="f152b-105">Syntax</span></span>

```cpp
HRESULT IsFrozenObject( [in]  ObjectID objectId,
                        [out] BOOL *pbFrozen);
```

## <a name="parameters"></a><span data-ttu-id="f152b-106">parameters</span><span class="sxs-lookup"><span data-stu-id="f152b-106">Parameters</span></span>

<span data-ttu-id="f152b-107">`objectId` 中要检查的对象。</span><span class="sxs-lookup"><span data-stu-id="f152b-107">`objectId` [in] The object to examine.</span></span>

<span data-ttu-id="f152b-108">`pbFrozen` 弄一个 `BOOL` ，该值指示对象是否位于只读段中。</span><span class="sxs-lookup"><span data-stu-id="f152b-108">`pbFrozen` [out] A `BOOL` indicating if the object is in a read-only segment.</span></span>

## <a name="requirements"></a><span data-ttu-id="f152b-109">要求</span><span class="sxs-lookup"><span data-stu-id="f152b-109">Requirements</span></span>

<span data-ttu-id="f152b-110">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="f152b-110">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>

<span data-ttu-id="f152b-111">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="f152b-111">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="f152b-112">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="f152b-112">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="f152b-113">**.Net 版本：**[!INCLUDE[net_core_30](../../../../includes/net-core-30-md.md)]</span><span class="sxs-lookup"><span data-stu-id="f152b-113">**.NET Versions:** [!INCLUDE[net_core_30](../../../../includes/net-core-30-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="f152b-114">另请参阅</span><span class="sxs-lookup"><span data-stu-id="f152b-114">See also</span></span>

- [<span data-ttu-id="f152b-115">ICorProfilerInfo10 接口</span><span class="sxs-lookup"><span data-stu-id="f152b-115">ICorProfilerInfo10 Interface</span></span>](icorprofilerinfo10-interface.md)
