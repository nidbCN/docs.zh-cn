---
description: 了解详细信息： ICorProfilerInfo8：： GetDynamicFunctionInfo 方法
title: ICorProfilerInfo8::GetDynamicFunctionInfo
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo8.GetDynamicFunctionInfo
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: b38bd7a4f440edba0a7156176f223ba38c9807cf
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759112"
---
# <a name="icorprofilerinfo8getdynamicfunctioninfo-method"></a><span data-ttu-id="bab85-103">ICorProfilerInfo8：： GetDynamicFunctionInfo 方法</span><span class="sxs-lookup"><span data-stu-id="bab85-103">ICorProfilerInfo8::GetDynamicFunctionInfo Method</span></span>

<span data-ttu-id="bab85-104">检索有关动态方法的信息。</span><span class="sxs-lookup"><span data-stu-id="bab85-104">Retrieves information about dynamic methods.</span></span>

## <a name="syntax"></a><span data-ttu-id="bab85-105">语法</span><span class="sxs-lookup"><span data-stu-id="bab85-105">Syntax</span></span>

```cpp
HRESULT GetDynamicFunctionInfo( [in]  FunctionID              functionId,
                                [out] ModuleID                *moduleId,
                                [out] PCCOR_SIGNATURE         *ppvSig,
                                [out] ULONG                   *pbSig,
                                [in]  ULONG                   cchName,
                                [out] ULONG                   *pcchName,
                                [out] WCHAR                   wszName[]);
```

## <a name="parameters"></a><span data-ttu-id="bab85-106">parameters</span><span class="sxs-lookup"><span data-stu-id="bab85-106">Parameters</span></span>

<span data-ttu-id="bab85-107">`functionId` 中要为其检索信息的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="bab85-107">`functionId` [in] The ID of the function for which to retrieve information.</span></span>

<span data-ttu-id="bab85-108">`moduleId` 中指向定义函数父类的模块的指针。</span><span class="sxs-lookup"><span data-stu-id="bab85-108">`moduleId` [in] A pointer to the module in which the function's parent class is defined.</span></span>

<span data-ttu-id="bab85-109">`ppvSig` 弄指向函数的签名的指针。</span><span class="sxs-lookup"><span data-stu-id="bab85-109">`ppvSig` [out] A pointer to the signature for the function.</span></span>

<span data-ttu-id="bab85-110">`pbSig` 弄指向函数签名的字节计数的指针。</span><span class="sxs-lookup"><span data-stu-id="bab85-110">`pbSig` [out] A pointer to the count of bytes for the function signature.</span></span>

<span data-ttu-id="bab85-111">`cchName` 中数组的最大大小 `wszName` 。</span><span class="sxs-lookup"><span data-stu-id="bab85-111">`cchName` [in] The maximum size of the `wszName` array.</span></span>

<span data-ttu-id="bab85-112">`pcchName` 弄数组中的字符数 `wszName` 。</span><span class="sxs-lookup"><span data-stu-id="bab85-112">`pcchName` [out] The number of characters in the `wszName` array.</span></span>

<span data-ttu-id="bab85-113">`wszName` 弄一个数组 `WCHAR` ，它是函数的名称（如果存在）。</span><span class="sxs-lookup"><span data-stu-id="bab85-113">`wszName` [out] An array of `WCHAR` which is the name of the function, if one exists.</span></span>

## <a name="remarks"></a><span data-ttu-id="bab85-114">备注</span><span class="sxs-lookup"><span data-stu-id="bab85-114">Remarks</span></span>

<span data-ttu-id="bab85-115">某些方法（如 IL 存根或 LCG）没有关联的元数据，可以使用 [IMetaDataImport](../metadata/imetadataimport-interface.md) 和 [IMetaDataImport2](../metadata/imetadataimport2-interface.md) api 来检索这些元数据。</span><span class="sxs-lookup"><span data-stu-id="bab85-115">Certain methods like IL Stubs or LCG do not have associated metadata that can be retrieved using the [IMetaDataImport](../metadata/imetadataimport-interface.md) and [IMetaDataImport2](../metadata/imetadataimport2-interface.md) APIs.</span></span> <span data-ttu-id="bab85-116">探查器可以通过指令指针或通过侦听 [ICorProfilerCallback8：:D ynamicmethodjitcompilationstarted](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)来遇到此类方法。</span><span class="sxs-lookup"><span data-stu-id="bab85-116">Such methods can be encountered by profilers through instruction pointers or by listening to [ICorProfilerCallback8::DynamicMethodJITCompilationStarted](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md).</span></span>

<span data-ttu-id="bab85-117">此 API 可用于检索有关动态方法的信息，包括友好名称（如果可用）。</span><span class="sxs-lookup"><span data-stu-id="bab85-117">This API can be used to retrieve information about dynamic methods, including a friendly name, if available.</span></span>

## <a name="requirements"></a><span data-ttu-id="bab85-118">要求</span><span class="sxs-lookup"><span data-stu-id="bab85-118">Requirements</span></span>

<span data-ttu-id="bab85-119">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="bab85-119">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>

<span data-ttu-id="bab85-120">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="bab85-120">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="bab85-121">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="bab85-121">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="bab85-122">**.NET Framework 版本：**[!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]</span><span class="sxs-lookup"><span data-stu-id="bab85-122">**.NET Framework Versions:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="bab85-123">另请参阅</span><span class="sxs-lookup"><span data-stu-id="bab85-123">See also</span></span>

- [<span data-ttu-id="bab85-124">ICorProfilerInfo8 接口</span><span class="sxs-lookup"><span data-stu-id="bab85-124">ICorProfilerInfo8 Interface</span></span>](icorprofilerinfo8-interface.md)
