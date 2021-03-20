---
description: 了解详细信息： ICorProfilerInfo9：： GetILToNativeMapping3 方法
title: ICorProfilerInfo9::GetILToNativeMapping3
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo9.GetILToNativeMapping3
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: 865545e2352209447b3942da3a62f3733c165b35
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759321"
---
# <a name="icorprofilerinfo9getiltonativemapping3-method"></a><span data-ttu-id="28ee5-103">ICorProfilerInfo9：： GetILToNativeMapping3 方法</span><span class="sxs-lookup"><span data-stu-id="28ee5-103">ICorProfilerInfo9::GetILToNativeMapping3 Method</span></span>

<span data-ttu-id="28ee5-104">给定本机代码起始地址，返回此实时编译版本的代码的本机到 IL 映射信息。</span><span class="sxs-lookup"><span data-stu-id="28ee5-104">Given the native code start address, returns the native to IL mapping information for this jitted version of the code.</span></span>

## <a name="syntax"></a><span data-ttu-id="28ee5-105">语法</span><span class="sxs-lookup"><span data-stu-id="28ee5-105">Syntax</span></span>

```cpp
HRESULT GetILToNativeMapping3( [in]  UINT_PTR pNativeCodeStartAddress,
                               [in]  ULONG32 cMap,
                               [out] ULONG32 *pcMap,
                               [out] COR_DEBUG_IL_TO_NATIVE_MAP map[]);
```

## <a name="parameters"></a><span data-ttu-id="28ee5-106">parameters</span><span class="sxs-lookup"><span data-stu-id="28ee5-106">Parameters</span></span>

<span data-ttu-id="28ee5-107">`pNativeCodeStartAddress` 中指向本机函数开头的指针。</span><span class="sxs-lookup"><span data-stu-id="28ee5-107">`pNativeCodeStartAddress` [in] A pointer to the start of a native function.</span></span>

<span data-ttu-id="28ee5-108">`cMap` 中数组的最大大小 `map` 。</span><span class="sxs-lookup"><span data-stu-id="28ee5-108">`cMap` [in] The maximum size of the `map` array.</span></span>

<span data-ttu-id="28ee5-109">`pcMap` 弄可用 COR_DEBUG_IL_TO_NATIVE_MAP 结构的总数。</span><span class="sxs-lookup"><span data-stu-id="28ee5-109">`pcMap` [out] The total number of available COR_DEBUG_IL_TO_NATIVE_MAP structures.</span></span>

<span data-ttu-id="28ee5-110">`map` 弄 [COR_DEBUG_IL_TO_NATIVE_MAP](../debugging/cor-debug-il-to-native-map-structure.md) 结构的数组，其中每个结构都指定了偏移量。</span><span class="sxs-lookup"><span data-stu-id="28ee5-110">`map` [out] An array of [COR_DEBUG_IL_TO_NATIVE_MAP](../debugging/cor-debug-il-to-native-map-structure.md) structures, each of which specifies the offsets.</span></span> <span data-ttu-id="28ee5-111">`GetILToNativeMapping3` 方法返回后，`map` 将包含部分或全部 `COR_DEBUG_IL_TO_NATIVE_MAP` 结构。</span><span class="sxs-lookup"><span data-stu-id="28ee5-111">After the `GetILToNativeMapping3` method returns, `map` will contain some or all of the `COR_DEBUG_IL_TO_NATIVE_MAP` structures.</span></span>

## <a name="remarks"></a><span data-ttu-id="28ee5-112">备注</span><span class="sxs-lookup"><span data-stu-id="28ee5-112">Remarks</span></span>

<span data-ttu-id="28ee5-113">当启用分层编译时，一个方法可能有多个本机代码体。</span><span class="sxs-lookup"><span data-stu-id="28ee5-113">When tiered compilation is enabled, a method may have more than one native code body.</span></span> <span data-ttu-id="28ee5-114">[ICorProfilerInfo9：： GetNativeCodeStartAddresses](icorprofilerinfo9-getnativecodestartaddresses-method.md) 将返回所有本机代码正文的开始地址。</span><span class="sxs-lookup"><span data-stu-id="28ee5-114">[ICorProfilerInfo9::GetNativeCodeStartAddresses](icorprofilerinfo9-getnativecodestartaddresses-method.md) will return the start addresses for all of the native code bodies.</span></span>

## <a name="requirements"></a><span data-ttu-id="28ee5-115">要求</span><span class="sxs-lookup"><span data-stu-id="28ee5-115">Requirements</span></span>

<span data-ttu-id="28ee5-116">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="28ee5-116">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>

<span data-ttu-id="28ee5-117">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="28ee5-117">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="28ee5-118">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="28ee5-118">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="28ee5-119">**.NET Framework 版本：**[!INCLUDE[net_core_21](../../../../includes/net-core-21-md.md)]</span><span class="sxs-lookup"><span data-stu-id="28ee5-119">**.NET Framework Versions:** [!INCLUDE[net_core_21](../../../../includes/net-core-21-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="28ee5-120">另请参阅</span><span class="sxs-lookup"><span data-stu-id="28ee5-120">See also</span></span>

- [<span data-ttu-id="28ee5-121">ICorProfilerInfo9 接口</span><span class="sxs-lookup"><span data-stu-id="28ee5-121">ICorProfilerInfo9 Interface</span></span>](icorprofilerinfo9-interface.md)
