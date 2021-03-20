---
description: 了解详细信息： ICorProfilerInfo：： SetILInstrumentedCodeMap 方法
title: ICorProfilerInfo::SetILInstrumentedCodeMap 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.SetILInstrumentedCodeMap
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::SetILInstrumentedCodeMap
helpviewer_keywords:
- ICorProfilerInfo::SetILInstrumentedCodeMap method [.NET Framework profiling]
- SetILInstrumentedCodeMap method [.NET Framework profiling]
ms.assetid: bce1dcf8-b4ec-4e73-a917-f2df1ad49c8a
topic_type:
- apiref
ms.openlocfilehash: f9ed01b91f740a8bd95f4ad049076feb02363b8f
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760387"
---
# <a name="icorprofilerinfosetilinstrumentedcodemap-method"></a><span data-ttu-id="b52b2-103">ICorProfilerInfo::SetILInstrumentedCodeMap 方法</span><span class="sxs-lookup"><span data-stu-id="b52b2-103">ICorProfilerInfo::SetILInstrumentedCodeMap Method</span></span>

<span data-ttu-id="b52b2-104">使用指定的 Microsoft 中间语言 (MSIL) 映射项为指定的函数设置代码图。</span><span class="sxs-lookup"><span data-stu-id="b52b2-104">Sets a code map for the specified function using the specified Microsoft intermediate language (MSIL) map entries.</span></span>

> [!NOTE]
> <span data-ttu-id="b52b2-105">在 .NET Framework 版本2.0 中，对 `SetILInstrumentedCodeMap` `FunctionID` 表示特定应用程序域中的泛型函数的调用将影响该函数在应用程序域中的所有实例。</span><span class="sxs-lookup"><span data-stu-id="b52b2-105">In the .NET Framework version 2.0, calling `SetILInstrumentedCodeMap` on a `FunctionID` that represents a generic function in a particular application domain will affect all instances of that function in the application domain.</span></span>

## <a name="syntax"></a><span data-ttu-id="b52b2-106">语法</span><span class="sxs-lookup"><span data-stu-id="b52b2-106">Syntax</span></span>

```cpp
HRESULT SetILInstrumentedCodeMap(
    [in]  FunctionID functionId,
    [in]  BOOL       fStartJit,
    [in]  ULONG      cILMapEntries,
    [in, size_is(cILMapEntries)] COR_IL_MAP rgILMapEntries[]);
```

## <a name="parameters"></a><span data-ttu-id="b52b2-107">parameters</span><span class="sxs-lookup"><span data-stu-id="b52b2-107">Parameters</span></span>

<span data-ttu-id="b52b2-108">`functionId` 中要为其设置代码图的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="b52b2-108">`functionId` [in] The ID of the function for which to set the code map.</span></span>

<span data-ttu-id="b52b2-109">`fStartJit` 中一个布尔值，该值指示对方法的调用是否为 `SetILInstrumentedCodeMap` 特定的第一个 `FunctionID` 。</span><span class="sxs-lookup"><span data-stu-id="b52b2-109">`fStartJit` [in] A Boolean value that indicates whether the call to the `SetILInstrumentedCodeMap` method is the first for a particular `FunctionID`.</span></span> <span data-ttu-id="b52b2-110">`fStartJit` `true` 对于给定的，将在第一次调用中设置为 `SetILInstrumentedCodeMap` `FunctionID` ，并在 `false` 之后设置为。</span><span class="sxs-lookup"><span data-stu-id="b52b2-110">Set `fStartJit` to `true` in the first call to `SetILInstrumentedCodeMap` for a given `FunctionID`, and to `false` thereafter.</span></span>

<span data-ttu-id="b52b2-111">`cILMapEntries` 中数组中元素的数目 `cILMapEntries` 。</span><span class="sxs-lookup"><span data-stu-id="b52b2-111">`cILMapEntries` [in] The number of elements in the `cILMapEntries` array.</span></span>

<span data-ttu-id="b52b2-112">`rgILMapEntries` 中COR_IL_MAP 结构的数组，其中每个结构指定 MSIL 偏移量。</span><span class="sxs-lookup"><span data-stu-id="b52b2-112">`rgILMapEntries` [in] An array of COR_IL_MAP structures, each of which specifies an MSIL offset.</span></span>

## <a name="remarks"></a><span data-ttu-id="b52b2-113">备注</span><span class="sxs-lookup"><span data-stu-id="b52b2-113">Remarks</span></span>

<span data-ttu-id="b52b2-114">探查器通常在方法的源代码中插入语句，以便检测该方法 (例如，在) 到达给定的源行时发出通知。</span><span class="sxs-lookup"><span data-stu-id="b52b2-114">A profiler often inserts statements within the source code of a method in order to instrument that method (for example, to notify when a given source line is reached).</span></span> <span data-ttu-id="b52b2-115">`SetILInstrumentedCodeMap` 使探查器能够将原始 MSIL 指令映射到新位置。</span><span class="sxs-lookup"><span data-stu-id="b52b2-115">`SetILInstrumentedCodeMap` enables a profiler to map the original MSIL instructions to their new locations.</span></span> <span data-ttu-id="b52b2-116">探查器可以使用 [ICorProfilerInfo：： GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md) 方法获取给定本机偏移量的原始 MSIL 偏移量。</span><span class="sxs-lookup"><span data-stu-id="b52b2-116">A profiler can use the [ICorProfilerInfo::GetILToNativeMapping](icorprofilerinfo-getiltonativemapping-method.md) method to get the original MSIL offset for a given native offset.</span></span>

<span data-ttu-id="b52b2-117">调试器将假设每个旧偏移都是在原始的、未修改的 MSIL 代码内引用 MSIL 偏移量，并且每个新偏移指的是新的、经过检测的代码内的 MSIL 偏移量。</span><span class="sxs-lookup"><span data-stu-id="b52b2-117">The debugger will assume that each old offset refers to an MSIL offset within the original, unmodified MSIL code, and that each new offset refers to the MSIL offset within the new, instrumented code.</span></span> <span data-ttu-id="b52b2-118">地图应按递增顺序排序。</span><span class="sxs-lookup"><span data-stu-id="b52b2-118">The map should be sorted in increasing order.</span></span> <span data-ttu-id="b52b2-119">若要单步执行，请遵循以下准则：</span><span class="sxs-lookup"><span data-stu-id="b52b2-119">For stepping to work properly, follow these guidelines:</span></span>

- <span data-ttu-id="b52b2-120">不要对已检测的 MSIL 代码重新排序。</span><span class="sxs-lookup"><span data-stu-id="b52b2-120">Do not reorder instrumented MSIL code.</span></span>

- <span data-ttu-id="b52b2-121">请勿删除原始的 MSIL 代码。</span><span class="sxs-lookup"><span data-stu-id="b52b2-121">Do not remove the original MSIL code.</span></span>

- <span data-ttu-id="b52b2-122">为程序数据库中的所有序列点包含条目 (PDB) 文件。</span><span class="sxs-lookup"><span data-stu-id="b52b2-122">Include entries for all the sequence points from the program database (PDB) file in the map.</span></span> <span data-ttu-id="b52b2-123">地图未插入缺失的条目。</span><span class="sxs-lookup"><span data-stu-id="b52b2-123">The map does not interpolate missing entries.</span></span> <span data-ttu-id="b52b2-124">因此，假设有以下映射：</span><span class="sxs-lookup"><span data-stu-id="b52b2-124">So, given the following map:</span></span>

  <span data-ttu-id="b52b2-125"> (0 个旧的，0个新) </span><span class="sxs-lookup"><span data-stu-id="b52b2-125">(0 old, 0 new)</span></span>

  <span data-ttu-id="b52b2-126"> (5 个旧的、10个新) </span><span class="sxs-lookup"><span data-stu-id="b52b2-126">(5 old, 10 new)</span></span>

  <span data-ttu-id="b52b2-127"> (9 旧版本，20个新) </span><span class="sxs-lookup"><span data-stu-id="b52b2-127">(9 old, 20 new)</span></span>

  - <span data-ttu-id="b52b2-128">早于0、1、2、3或4的偏移将映射到新的偏移量0。</span><span class="sxs-lookup"><span data-stu-id="b52b2-128">An old offset of 0, 1, 2, 3, or 4 will be mapped to new offset 0.</span></span>

  - <span data-ttu-id="b52b2-129">旧偏移量5、6、7或8将映射到新的偏移量10。</span><span class="sxs-lookup"><span data-stu-id="b52b2-129">An old offset of 5, 6, 7, or 8 will be mapped to new offset 10.</span></span>

  - <span data-ttu-id="b52b2-130">旧偏移量9或更高将映射到新偏移量20。</span><span class="sxs-lookup"><span data-stu-id="b52b2-130">An old offset of 9 or higher will be mapped to new offset 20.</span></span>

  - <span data-ttu-id="b52b2-131">新偏移量为0、1、2、3、4、5、6、7、8或9，将映射到旧偏移量0。</span><span class="sxs-lookup"><span data-stu-id="b52b2-131">A new offset of 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9 will be mapped to old offset 0.</span></span>

  - <span data-ttu-id="b52b2-132">新偏移量为10、11、12、13、14、15、16、17、18或19，将映射到旧偏移量5。</span><span class="sxs-lookup"><span data-stu-id="b52b2-132">A new offset of 10, 11, 12, 13, 14, 15, 16, 17, 18, or 19 will be mapped to old offset 5.</span></span>

  - <span data-ttu-id="b52b2-133">20或更高的新偏移量将映射到旧偏移量9。</span><span class="sxs-lookup"><span data-stu-id="b52b2-133">A new offset of 20 or higher will be mapped to old offset 9.</span></span>

<span data-ttu-id="b52b2-134">在 .NET Framework 3.5 和以前的版本中， `rgILMapEntries` 通过调用 [CoTaskMemAlloc](/windows/desktop/api/combaseapi/nf-combaseapi-cotaskmemalloc) 方法来分配数组。</span><span class="sxs-lookup"><span data-stu-id="b52b2-134">In the .NET Framework 3.5 and previous versions, you allocate the `rgILMapEntries` array by calling the [CoTaskMemAlloc](/windows/desktop/api/combaseapi/nf-combaseapi-cotaskmemalloc) method.</span></span> <span data-ttu-id="b52b2-135">由于运行时取得了此内存的所有权，因此探查器不应尝试释放它。</span><span class="sxs-lookup"><span data-stu-id="b52b2-135">Because the runtime takes ownership of this memory, the profiler should not attempt to free it.</span></span>

## <a name="requirements"></a><span data-ttu-id="b52b2-136">要求</span><span class="sxs-lookup"><span data-stu-id="b52b2-136">Requirements</span></span>

<span data-ttu-id="b52b2-137">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="b52b2-137">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>

<span data-ttu-id="b52b2-138">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="b52b2-138">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="b52b2-139">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="b52b2-139">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="b52b2-140">**.NET Framework 版本：**[!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="b52b2-140">**.NET Framework Versions:** [!INCLUDE[net_current_v11plus](../../../../includes/net-current-v11plus-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="b52b2-141">另请参阅</span><span class="sxs-lookup"><span data-stu-id="b52b2-141">See also</span></span>

- [<span data-ttu-id="b52b2-142">ICorProfilerInfo 接口</span><span class="sxs-lookup"><span data-stu-id="b52b2-142">ICorProfilerInfo Interface</span></span>](icorprofilerinfo-interface.md)
