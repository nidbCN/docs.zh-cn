---
description: 了解详细信息： ICorProfilerInfo6：： EnumNgenModuleMethodsInliningThisMethod 方法
title: ICorProfilerInfo6::EnumNgenModuleMethodsInliningThisMethod 方法
ms.date: 03/30/2017
ms.assetid: b933dfe6-7833-40cb-aad8-40842dc3034f
ms.openlocfilehash: 236aaa820162dcc1d5c6c8ade1e8da78f5f4acb0
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759125"
---
# <a name="icorprofilerinfo6enumngenmodulemethodsinliningthismethod-method"></a><span data-ttu-id="7c9d8-103">ICorProfilerInfo6::EnumNgenModuleMethodsInliningThisMethod 方法</span><span class="sxs-lookup"><span data-stu-id="7c9d8-103">ICorProfilerInfo6::EnumNgenModuleMethodsInliningThisMethod Method</span></span>

<span data-ttu-id="7c9d8-104">返回一个枚举器，该枚举器指向给定的 NGen 模块中定义的所有方法，并内联给定方法。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-104">Returns an enumerator to all the methods that are defined in a given NGen module and inline a given method.</span></span>

## <a name="syntax"></a><span data-ttu-id="7c9d8-105">语法</span><span class="sxs-lookup"><span data-stu-id="7c9d8-105">Syntax</span></span>

```cpp
HRESULT EnumNgenModuleMethodsInliningThisMethod(
        [in] ModuleID inlinersModuleId,
        [in] ModuleID inlineeModuleId,
        [in] mdMethodDef inlineeMethodId,
        [out] BOOL *incompleteData,
        [out] ICorProfilerMethodEnum** ppEnum
);
```

## <a name="parameters"></a><span data-ttu-id="7c9d8-106">parameters</span><span class="sxs-lookup"><span data-stu-id="7c9d8-106">Parameters</span></span>

<span data-ttu-id="7c9d8-107">`inlinersModuleId` 中NGen 模块的标识符。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-107">`inlinersModuleId` [in] The identifier of an NGen module.</span></span>

<span data-ttu-id="7c9d8-108">`inlineeModuleId` 中定义的模块的标识符 `inlineeMethodId` 。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-108">`inlineeModuleId` [in] The identifier of a module that defines `inlineeMethodId`.</span></span> <span data-ttu-id="7c9d8-109">有关详细信息，请参阅备注部分。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-109">See the Remarks section for more information.</span></span>

<span data-ttu-id="7c9d8-110">`inlineeMethodId` 中内联方法的标识符。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-110">`inlineeMethodId` [in] The identifier of an inlined method.</span></span> <span data-ttu-id="7c9d8-111">有关详细信息，请参阅备注部分。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-111">See the Remarks section for more information.</span></span>

<span data-ttu-id="7c9d8-112">`incompleteData` 弄一个标志，该标志指示是否 `ppEnum` 将所有方法内联到给定方法。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-112">`incompleteData` [out] A flag that indicates whether `ppEnum` contains all methods inlining a given method.</span></span>  <span data-ttu-id="7c9d8-113">有关详细信息，请参阅备注部分。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-113">See the Remarks section for more information.</span></span>

<span data-ttu-id="7c9d8-114">`ppEnum` 弄指向枚举器地址的指针</span><span class="sxs-lookup"><span data-stu-id="7c9d8-114">`ppEnum` [out] A pointer to the address of an enumerator</span></span>

## <a name="remarks"></a><span data-ttu-id="7c9d8-115">备注</span><span class="sxs-lookup"><span data-stu-id="7c9d8-115">Remarks</span></span>

<span data-ttu-id="7c9d8-116">`inlineeModuleId` 和 `inlineeMethodId` 共同构成了可能内联的方法的完整标识符。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-116">`inlineeModuleId` and `inlineeMethodId` together form the full identifier for the method that might be inlined.</span></span> <span data-ttu-id="7c9d8-117">例如，假设 module `A` 定义方法 `Simple.Add` ：</span><span class="sxs-lookup"><span data-stu-id="7c9d8-117">For example, assume module `A` defines a method `Simple.Add`:</span></span>

```csharp
Simple.Add(int a, int b)
{ return a + b; }
```

<span data-ttu-id="7c9d8-118">和模块 B 定义 `Fancy.AddTwice` ：</span><span class="sxs-lookup"><span data-stu-id="7c9d8-118">and module B defines `Fancy.AddTwice`:</span></span>

```csharp
Fancy.AddTwice(int a, int b)
{ return Simple.Add(a,b) + Simple.Add(a,b); }
```

<span data-ttu-id="7c9d8-119">还假定 `Fancy.AddTwice` inlines 调用 `SimpleAdd` 。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-119">Lets also assume that `Fancy.AddTwice` inlines the call to `SimpleAdd`.</span></span> <span data-ttu-id="7c9d8-120">探查器可以使用此枚举器查找在模块 B 中定义的内联的所有方法 `Simple.Add` ，并将枚举结果 `AddTwice` 。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-120">A profiler could use this enumerator to find all methods defined in module B which inline `Simple.Add`, and the result would enumerate `AddTwice`.</span></span>  <span data-ttu-id="7c9d8-121">`inlineeModuleId` 是模块的标识符 `A` ，是的 `inlineeMethodId` 标识符 `Simple.Add(int a, int b)` 。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-121">`inlineeModuleId` is the identifier of module `A`, and `inlineeMethodId` is the identifier of `Simple.Add(int a, int b)`.</span></span>

<span data-ttu-id="7c9d8-122">如果在 `incompleteData` 函数返回后为 true，则枚举器不包含内联给定方法的所有方法。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-122">If `incompleteData` is true after the function returns, the enumerator does not contain all methods inlining a given method.</span></span> <span data-ttu-id="7c9d8-123">如果尚未加载 inliners 模块的一个或多个直接或间接依赖项，则可能会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-123">This can happen when one or more direct or indirect dependencies of inliners module haven't been loaded yet.</span></span> <span data-ttu-id="7c9d8-124">如果探查器需要准确的数据，则应在加载更多模块时重试，最好是在每个模块加载时重试。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-124">If a profiler needs accurate data, it should retry later when more modules are loaded, preferably on each module load.</span></span>

<span data-ttu-id="7c9d8-125">`EnumNgenModuleMethodsInliningThisMethod`方法可用于解决 ReJIT 的内联限制。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-125">The `EnumNgenModuleMethodsInliningThisMethod` method can be used to work around limitations on inlining for ReJIT.</span></span> <span data-ttu-id="7c9d8-126">ReJIT 使探查器可以更改方法的实现，然后动态为其创建新代码。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-126">ReJIT lets a profiler change the implementation of a method and then create new code for it on the fly.</span></span> <span data-ttu-id="7c9d8-127">例如，我们可以 `Simple.Add` 按如下所示进行更改：</span><span class="sxs-lookup"><span data-stu-id="7c9d8-127">For example, we could change `Simple.Add` as follows:</span></span>

```csharp
Simple.Add(int a, int b)
{ return 42; }
```

<span data-ttu-id="7c9d8-128">但是 `Fancy.AddTwice` ，因为已内联 `Simple.Add` ，所以它的行为与以前相同。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-128">However because `Fancy.AddTwice` has already inlined `Simple.Add`, it continues to have the same behavior as before.</span></span> <span data-ttu-id="7c9d8-129">若要解决此限制，调用方必须搜索所有在这些方法中内联和使用的模块中的所有方法 `Simple.Add` `ICorProfilerInfo5::RequestRejit` 。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-129">To work around that limitation, the caller has to search for all methods in all modules that inline `Simple.Add` and use `ICorProfilerInfo5::RequestRejit` on each of those methods.</span></span> <span data-ttu-id="7c9d8-130">重新编译这些方法时，它们将具有新的行为， `Simple.Add` 而不是旧的行为。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-130">When the methods are re-compiled, they will have the new behavior of `Simple.Add` instead of the old behavior.</span></span>

## <a name="requirements"></a><span data-ttu-id="7c9d8-131">要求</span><span class="sxs-lookup"><span data-stu-id="7c9d8-131">Requirements</span></span>

<span data-ttu-id="7c9d8-132">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="7c9d8-132">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>

<span data-ttu-id="7c9d8-133">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="7c9d8-133">**Header:** CorProf.idl, CorProf.h</span></span>

<span data-ttu-id="7c9d8-134">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="7c9d8-134">**Library:** CorGuids.lib</span></span>

<span data-ttu-id="7c9d8-135">**.NET Framework 版本：**[!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="7c9d8-135">**.NET Framework Versions:** [!INCLUDE[net_current_v46plus](../../../../includes/net-current-v46plus-md.md)]</span></span>

## <a name="see-also"></a><span data-ttu-id="7c9d8-136">另请参阅</span><span class="sxs-lookup"><span data-stu-id="7c9d8-136">See also</span></span>

- [<span data-ttu-id="7c9d8-137">ICorProfilerInfo6 接口</span><span class="sxs-lookup"><span data-stu-id="7c9d8-137">ICorProfilerInfo6 Interface</span></span>](icorprofilerinfo6-interface.md)
