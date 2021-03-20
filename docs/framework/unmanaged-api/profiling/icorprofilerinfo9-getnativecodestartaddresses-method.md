---
description: 了解详细信息： ICorProfilerInfo9：： GetNativeCodeStartAddresses 方法
title: ICorProfilerInfo9::GetNativeCodeStartAddresses
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo9.GetNativeCodeStartAddresses
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 062aebf6d5bed208ea71b215bd9f857b82483673
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759036"
---
# <a name="icorprofilerinfo9getnativecodestartaddresses-method"></a><span data-ttu-id="1706e-103">ICorProfilerInfo9：： GetNativeCodeStartAddresses 方法</span><span class="sxs-lookup"><span data-stu-id="1706e-103">ICorProfilerInfo9::GetNativeCodeStartAddresses Method</span></span>

<span data-ttu-id="1706e-104">给定一个 functionId 和 rejitId，枚举此代码当前存在的所有实时编译版本的本机代码起始地址。</span><span class="sxs-lookup"><span data-stu-id="1706e-104">Given a functionId and rejitId, enumerates the native code start address of all jitted versions of this code that currently exist.</span></span>

## <a name="syntax"></a><span data-ttu-id="1706e-105">语法</span><span class="sxs-lookup"><span data-stu-id="1706e-105">Syntax</span></span>

```cpp
HRESULT GetNativeCodeStartAddresses( [in]  FunctionID functionID,
                                     [in]  ReJITID reJitId,
                                     [in]  ULONG32 cCodeStartAddresses,
                                     [out] ULONG32 *pcCodeStartAddresses,
                                     [out] UINT_PTR codeStartAddresses[]);
```

## <a name="parameters"></a><span data-ttu-id="1706e-106">parameters</span><span class="sxs-lookup"><span data-stu-id="1706e-106">Parameters</span></span>

<span data-ttu-id="1706e-107">`functionId` 中应返回其本机代码起始地址的函数的 ID。</span><span class="sxs-lookup"><span data-stu-id="1706e-107">`functionId` [in] The ID of the function whose native code start addresses should be returned.</span></span>

<span data-ttu-id="1706e-108">`reJitId` 中JIT 重新编译的函数的标识。</span><span class="sxs-lookup"><span data-stu-id="1706e-108">`reJitId` [in] The identity of the JIT-recompiled function.</span></span>

<span data-ttu-id="1706e-109">`cCodeStartAddresses` 中数组的最大大小 `codeStartAddresses` 。</span><span class="sxs-lookup"><span data-stu-id="1706e-109">`cCodeStartAddresses` [in] The maximum size of the `codeStartAddresses` array.</span></span>

<span data-ttu-id="1706e-110">`pcCodeStartAddresses` 弄可用地址的数目。</span><span class="sxs-lookup"><span data-stu-id="1706e-110">`pcCodeStartAddresses` [out] The number of available addresses.</span></span>

<span data-ttu-id="1706e-111">`codeStartAddresses` 弄的数组 `UINT_PTR` ，其中每个都是指定函数的本机正文的开始地址。</span><span class="sxs-lookup"><span data-stu-id="1706e-111">`codeStartAddresses` [out] An array of `UINT_PTR`, each one of which is the start address for a native body for the specified function.</span></span>

## <a name="remarks"></a><span data-ttu-id="1706e-112">备注</span><span class="sxs-lookup"><span data-stu-id="1706e-112">Remarks</span></span>

<span data-ttu-id="1706e-113">启用分层编译后，函数可能有多个本机代码体。</span><span class="sxs-lookup"><span data-stu-id="1706e-113">When tiered compilation is enabled, a function may have more than one native code body.</span></span>

## <a name="requirements"></a><span data-ttu-id="1706e-114">要求</span><span class="sxs-lookup"><span data-stu-id="1706e-114">Requirements</span></span>

<span data-ttu-id="1706e-115">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="1706e-115">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>

<span data-ttu-id="1706e-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="1706e-116">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="1706e-117">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="1706e-117">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="1706e-118">**.Net 版本：**[!INCLUDE[net_core_21](../../../../includes/net-core-21-md.md)]</span><span class="sxs-lookup"><span data-stu-id="1706e-118">**.NET Versions:** [!INCLUDE[net_core_21](../../../../includes/net-core-21-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="1706e-119">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1706e-119">See also</span></span>

- [<span data-ttu-id="1706e-120">ICorProfilerInfo9 接口</span><span class="sxs-lookup"><span data-stu-id="1706e-120">ICorProfilerInfo9 Interface</span></span>](icorprofilerinfo9-interface.md)
