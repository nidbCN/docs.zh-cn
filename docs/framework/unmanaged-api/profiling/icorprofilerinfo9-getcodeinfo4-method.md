---
description: 了解详细信息： ICorProfilerInfo9：： GetCodeInfo4 方法
title: ICorProfilerInfo9::GetCodeInfo4
ms.date: 08/06/2019
dev_langs:
- cpp
api_name:
- ICorProfilerInfo9.GetCodeInfo4
api_location:
- mscorwks.dll
api_type:
- COM
author: davmason
ms.author: davmason
ms.openlocfilehash: c7897e266fbb84d44df719c127e24bd375b560bb
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759086"
---
# <a name="icorprofilerinfo9getcodeinfo4-method"></a><span data-ttu-id="1e358-103">ICorProfilerInfo9：： GetCodeInfo4 方法</span><span class="sxs-lookup"><span data-stu-id="1e358-103">ICorProfilerInfo9::GetCodeInfo4 Method</span></span>

<span data-ttu-id="1e358-104">给定本机代码起始地址，返回存储此代码的虚拟内存块。</span><span class="sxs-lookup"><span data-stu-id="1e358-104">Given the native code start address, returns the blocks of virtual memory that store this code.</span></span>

## <a name="syntax"></a><span data-ttu-id="1e358-105">语法</span><span class="sxs-lookup"><span data-stu-id="1e358-105">Syntax</span></span>

```cpp
HRESULT GetCodeInfo4( [in]  UINT_PTR pNativeCodeStartAddress,
                      [in]  ULONG32 cCodeInfos,
                      [out] ULONG32* pcCodeInfos,
                      [out] COR_PRF_CODE_INFO codeInfos[]);
```

## <a name="parameters"></a><span data-ttu-id="1e358-106">parameters</span><span class="sxs-lookup"><span data-stu-id="1e358-106">Parameters</span></span>

<span data-ttu-id="1e358-107">`pNativeCodeStartAddress` 中指向本机函数开头的指针。</span><span class="sxs-lookup"><span data-stu-id="1e358-107">`pNativeCodeStartAddress` [in] A pointer to the start of a native function.</span></span>

<span data-ttu-id="1e358-108">`cCodeInfos` 中数组的大小 `codeInfos` 。</span><span class="sxs-lookup"><span data-stu-id="1e358-108">`cCodeInfos` [in] The size of the `codeInfos` array.</span></span>

<span data-ttu-id="1e358-109">`pcCodeInfos` 弄指向可用 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构总数的指针。</span><span class="sxs-lookup"><span data-stu-id="1e358-109">`pcCodeInfos` [out] A pointer to the total number of [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) structures available.</span></span>

<span data-ttu-id="1e358-110">`codeInfos` 弄调用方提供的缓冲区。</span><span class="sxs-lookup"><span data-stu-id="1e358-110">`codeInfos` [out] A caller-provided buffer.</span></span> <span data-ttu-id="1e358-111">返回此方法后，它包含一个 `COR_PRF_CODE_INFO` 结构数组，每个结构描述一个本机代码块。</span><span class="sxs-lookup"><span data-stu-id="1e358-111">After the method returns, it contains an array of `COR_PRF_CODE_INFO` structures, each of which describes a block of native code.</span></span>

## <a name="remarks"></a><span data-ttu-id="1e358-112">备注</span><span class="sxs-lookup"><span data-stu-id="1e358-112">Remarks</span></span>

<span data-ttu-id="1e358-113">`GetCodeInfo4`方法类似于[GetCodeInfo3](icorprofilerinfo4-getcodeinfo3-method.md)，只不过它可以查找方法的不同本机版本的代码信息。</span><span class="sxs-lookup"><span data-stu-id="1e358-113">The `GetCodeInfo4` method is similar to [GetCodeInfo3](icorprofilerinfo4-getcodeinfo3-method.md), except that it can look up code information for different native versions of a method.</span></span>

> [!NOTE]
> <span data-ttu-id="1e358-114">`GetCodeInfo4` 可以触发垃圾回收。</span><span class="sxs-lookup"><span data-stu-id="1e358-114">`GetCodeInfo4` can trigger a garbage collection.</span></span>

<span data-ttu-id="1e358-115">范围按公共中间语言 (CIL) 偏移递增的顺序进行排序。</span><span class="sxs-lookup"><span data-stu-id="1e358-115">The extents are sorted in order of increasing Common Intermediate Language (CIL) offset.</span></span>

<span data-ttu-id="1e358-116">`GetCodeInfo4`返回后，必须验证 `codeInfos` 缓冲区大小是否足以包含所有[COR_PRF_CODE_INFO](cor-prf-code-info-structure.md)的结构。</span><span class="sxs-lookup"><span data-stu-id="1e358-116">After `GetCodeInfo4` returns, you must verify that the `codeInfos` buffer was large enough to contain all the [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) structures.</span></span> <span data-ttu-id="1e358-117">为此，请将 `cCodeInfos` 的值和 `cchName` 参数的值进行比较。</span><span class="sxs-lookup"><span data-stu-id="1e358-117">To do this, compare the value of `cCodeInfos` with the value of the `cchName` parameter.</span></span> <span data-ttu-id="1e358-118">如果 `cCodeInfos` 除以 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构的大小小于 `pcCodeInfos` ，则分配更大的 `codeInfos` 缓冲区， `cCodeInfos` 用新的、更大的大小更新，然后再次调用 `GetCodeInfo4` 。</span><span class="sxs-lookup"><span data-stu-id="1e358-118">If `cCodeInfos` divided by the size of a [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) structure is smaller than `pcCodeInfos`, allocate a larger `codeInfos` buffer, update `cCodeInfos` with the new, larger size, and call `GetCodeInfo4` again.</span></span>

<span data-ttu-id="1e358-119">或者，可以先用长度为零的 `codeInfos` 缓冲区调用 `GetCodeInfo4` 以获取正确的缓冲区大小。</span><span class="sxs-lookup"><span data-stu-id="1e358-119">Alternatively, you can first call `GetCodeInfo4` with a zero-length `codeInfos` buffer to obtain the correct buffer size.</span></span> <span data-ttu-id="1e358-120">然后，可以将 `codeInfos` 缓冲区大小设置为中返回的值 `pcCodeInfos` ，再乘以 [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) 结构的大小，然后 `GetCodeInfo4` 再次调用。</span><span class="sxs-lookup"><span data-stu-id="1e358-120">You can then set the `codeInfos` buffer size to the value returned in `pcCodeInfos`, multiplied by the size of a [COR_PRF_CODE_INFO](cor-prf-code-info-structure.md) structure, and call `GetCodeInfo4` again.</span></span>

## <a name="requirements"></a><span data-ttu-id="1e358-121">要求</span><span class="sxs-lookup"><span data-stu-id="1e358-121">Requirements</span></span>

<span data-ttu-id="1e358-122">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="1e358-122">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>

<span data-ttu-id="1e358-123">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="1e358-123">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="1e358-124">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="1e358-124">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="1e358-125">**.Net 版本：**[!INCLUDE[net_core_21](../../../../includes/net-core-21-md.md)]</span><span class="sxs-lookup"><span data-stu-id="1e358-125">**.NET Versions:** [!INCLUDE[net_core_21](../../../../includes/net-core-21-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="1e358-126">另请参阅</span><span class="sxs-lookup"><span data-stu-id="1e358-126">See also</span></span>

- [<span data-ttu-id="1e358-127">ICorProfilerInfo9 接口</span><span class="sxs-lookup"><span data-stu-id="1e358-127">ICorProfilerInfo9 Interface</span></span>](ICorProfilerInfo9-interface.md)
