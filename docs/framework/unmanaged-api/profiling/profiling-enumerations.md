---
description: 了解详细信息：分析枚举
title: 分析枚举
ms.date: 03/30/2017
helpviewer_keywords:
- profiling enumerations [.NET Framework]
- enumerations [.NET Framework profiling]
- unmanaged enumerations [.NET Framework], profiling
ms.assetid: 8d5f9570-9853-4ce8-8101-df235d5b258e
ms.openlocfilehash: a4fcd812642698237d32afff5a681a99bf9cf12f
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759060"
---
# <a name="profiling-enumerations"></a><span data-ttu-id="a053f-103">分析枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-103">Profiling Enumerations</span></span>

<span data-ttu-id="a053f-104">本节描述分析 API 使用的非托管枚举。</span><span class="sxs-lookup"><span data-stu-id="a053f-104">This section describes the unmanaged enumerations that the profiling API uses.</span></span>  
  
## <a name="in-this-section"></a><span data-ttu-id="a053f-105">本节内容</span><span class="sxs-lookup"><span data-stu-id="a053f-105">In This Section</span></span>  

 [<span data-ttu-id="a053f-106">COR_PRF_CLAUSE_TYPE 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-106">COR_PRF_CLAUSE_TYPE Enumeration</span></span>](cor-prf-clause-type-enumeration.md)  
 <span data-ttu-id="a053f-107">指示代码刚进入或离开的异常子句的类型。</span><span class="sxs-lookup"><span data-stu-id="a053f-107">Indicates the type of exception clause that the code has just entered or left.</span></span>  
  
 [<span data-ttu-id="a053f-108">COR_PRF_CODEGEN_FLAGS 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-108">COR_PRF_CODEGEN_FLAGS Enumeration</span></span>](cor-prf-codegen-flags-enumeration.md)  
 <span data-ttu-id="a053f-109">定义可以用 [ICorProfilerFunctionControl：： SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md) 方法设置的代码生成标志。</span><span class="sxs-lookup"><span data-stu-id="a053f-109">Defines the code generation flags that can be set with the [ICorProfilerFunctionControl::SetCodegenFlags](icorprofilerfunctioncontrol-setcodegenflags-method.md) method.</span></span>  
  
 [<span data-ttu-id="a053f-110">COR_PRF_FINALIZER_FLAGS 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-110">COR_PRF_FINALIZER_FLAGS Enumeration</span></span>](cor-prf-finalizer-flags-enumeration.md)  
 <span data-ttu-id="a053f-111">描述对象的终结器。</span><span class="sxs-lookup"><span data-stu-id="a053f-111">Describes the finalizer for an object.</span></span>  
  
 [<span data-ttu-id="a053f-112">COR_PRF_GC_GENERATION 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-112">COR_PRF_GC_GENERATION Enumeration</span></span>](cor-prf-gc-generation-enumeration.md)  
 <span data-ttu-id="a053f-113">标识垃圾回收生成。</span><span class="sxs-lookup"><span data-stu-id="a053f-113">Identifies a garbage collection generation.</span></span>  
  
 [<span data-ttu-id="a053f-114">COR_PRF_GC_REASON 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-114">COR_PRF_GC_REASON Enumeration</span></span>](cor-prf-gc-reason-enumeration.md)  
 <span data-ttu-id="a053f-115">指示当前发生垃圾回收的原因。</span><span class="sxs-lookup"><span data-stu-id="a053f-115">Indicates the reason that garbage collection is occurring.</span></span>  
  
 [<span data-ttu-id="a053f-116">COR_PRF_GC_ROOT_FLAGS 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-116">COR_PRF_GC_ROOT_FLAGS Enumeration</span></span>](cor-prf-gc-root-flags-enumeration.md)  
 <span data-ttu-id="a053f-117">指示垃圾回收器根的属性。</span><span class="sxs-lookup"><span data-stu-id="a053f-117">Indicates properties of a garbage collector root.</span></span>  
  
 [<span data-ttu-id="a053f-118">COR_PRF_GC_ROOT_KIND 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-118">COR_PRF_GC_ROOT_KIND Enumeration</span></span>](cor-prf-gc-root-kind-enumeration.md)  
 <span data-ttu-id="a053f-119">指示由 [ICorProfilerCallback2：： RootReferences2](icorprofilercallback2-rootreferences2-method.md) 回调公开的垃圾回收器根的类型。</span><span class="sxs-lookup"><span data-stu-id="a053f-119">Indicates the kind of garbage collector root that is exposed by the [ICorProfilerCallback2::RootReferences2](icorprofilercallback2-rootreferences2-method.md) callback.</span></span>  
  
 [<span data-ttu-id="a053f-120">COR_PRF_HIGH_MONITOR 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-120">COR_PRF_HIGH_MONITOR Enumeration</span></span>](cor-prf-high-monitor-enumeration.md)  
 <span data-ttu-id="a053f-121">除了在 [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) 枚举中找到的标记外，还提供了标志，在加载时，探查器可以为 [ICorProfilerInfo5：： SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) 方法指定这些标志。</span><span class="sxs-lookup"><span data-stu-id="a053f-121">Provides flags in addition to those found in the [COR_PRF_MONITOR](cor-prf-monitor-enumeration.md) enumeration that the profiler can specify to the [ICorProfilerInfo5::SetEventMask2](icorprofilerinfo5-seteventmask2-method.md) method when it is loading.</span></span>  
  
 [<span data-ttu-id="a053f-122">COR_PRF_JIT_CACHE 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-122">COR_PRF_JIT_CACHE Enumeration</span></span>](cor-prf-jit-cache-enumeration.md)  
 <span data-ttu-id="a053f-123">指示缓存的函数搜索的结果。</span><span class="sxs-lookup"><span data-stu-id="a053f-123">Indicates the result of a cached function search.</span></span>  
  
 [<span data-ttu-id="a053f-124">COR_PRF_MISC 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-124">COR_PRF_MISC Enumeration</span></span>](cor-prf-misc-enumeration.md)  
 <span data-ttu-id="a053f-125">包含指定特殊标识符的常数值。</span><span class="sxs-lookup"><span data-stu-id="a053f-125">Contains constant values that specify special identifiers.</span></span>  
  
 [<span data-ttu-id="a053f-126">COR_PRF_MODULE_FLAGS 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-126">COR_PRF_MODULE_FLAGS Enumeration</span></span>](cor-prf-module-flags-enumeration.md)  
 <span data-ttu-id="a053f-127">指定模块的属性。</span><span class="sxs-lookup"><span data-stu-id="a053f-127">Specifies the properties of a module.</span></span>  
  
 [<span data-ttu-id="a053f-128">COR_PRF_MONITOR 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-128">COR_PRF_MONITOR Enumeration</span></span>](cor-prf-monitor-enumeration.md)  
 <span data-ttu-id="a053f-129">包含用于指定探查器希望订阅的行为、功能或事件的值。</span><span class="sxs-lookup"><span data-stu-id="a053f-129">Contains values that are used to specify behavior, capabilities, or events to which the profiler wishes to subscribe.</span></span>  
  
 [<span data-ttu-id="a053f-130">COR_PRF_RUNTIME_TYPE 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-130">COR_PRF_RUNTIME_TYPE Enumeration</span></span>](cor-prf-runtime-type-enumeration.md)  
 <span data-ttu-id="a053f-131">包含指示公共语言运行时的版本的值。</span><span class="sxs-lookup"><span data-stu-id="a053f-131">Contains values that indicate the version of the common language runtime.</span></span>  
  
 [<span data-ttu-id="a053f-132">COR_PRF_SNAPSHOT_INFO 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-132">COR_PRF_SNAPSHOT_INFO Enumeration</span></span>](cor-prf-snapshot-info-enumeration.md)  
 <span data-ttu-id="a053f-133">指定在每次调用探查器的 `StackSnapshotCallback` 函数时通过堆栈快照传回多少数据。</span><span class="sxs-lookup"><span data-stu-id="a053f-133">Specifies how much data to pass back with a stack snapshot in each call to the profiler's `StackSnapshotCallback` function.</span></span>  
  
 [<span data-ttu-id="a053f-134">COR_PRF_STATIC_TYPE 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-134">COR_PRF_STATIC_TYPE Enumeration</span></span>](cor-prf-static-type-enumeration.md)  
 <span data-ttu-id="a053f-135">指示字段是否为静态的，并在字段为静态字段时指示应用于该字段的静态质量。</span><span class="sxs-lookup"><span data-stu-id="a053f-135">Indicates whether a field is static and, if so, the static quality that applies to the field.</span></span>  
  
 [<span data-ttu-id="a053f-136">COR_PRF_SUSPEND_REASON 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-136">COR_PRF_SUSPEND_REASON Enumeration</span></span>](cor-prf-suspend-reason-enumeration.md)  
 <span data-ttu-id="a053f-137">指示运行时挂起的原因。</span><span class="sxs-lookup"><span data-stu-id="a053f-137">Indicates the reason that the runtime was suspended.</span></span>  
  
 [<span data-ttu-id="a053f-138">COR_PRF_TRANSITION_REASON 枚举</span><span class="sxs-lookup"><span data-stu-id="a053f-138">COR_PRF_TRANSITION_REASON Enumeration</span></span>](cor-prf-transition-reason-enumeration.md)  
 <span data-ttu-id="a053f-139">指示从托管代码向非托管代码转换或从非托管代码向托管代码转换的原因。</span><span class="sxs-lookup"><span data-stu-id="a053f-139">Indicates the reason for a transition from managed to unmanaged code, or vice versa.</span></span>  

 <span data-ttu-id="a053f-140">[COR_PRF_EVENTPIPE_PARAM_TYPE](cor-prf-eventpipe-param-type-enumeration.md) 指示 EventPipe 参数的类型。</span><span class="sxs-lookup"><span data-stu-id="a053f-140">[COR_PRF_EVENTPIPE_PARAM_TYPE](cor-prf-eventpipe-param-type-enumeration.md) Indicates the type of an EventPipe parameter.</span></span>

 <span data-ttu-id="a053f-141">[COR_PRF_EVENTPIPE_LEVEL](cor-prf-eventpipe-level-enumeration.md) Indivates EventPipe 事件的级别。</span><span class="sxs-lookup"><span data-stu-id="a053f-141">[COR_PRF_EVENTPIPE_LEVEL](cor-prf-eventpipe-level-enumeration.md) Indivates the level of an EventPipe event.</span></span>
  
## <a name="related-sections"></a><span data-ttu-id="a053f-142">相关章节</span><span class="sxs-lookup"><span data-stu-id="a053f-142">Related Sections</span></span>  

 [<span data-ttu-id="a053f-143">分析概述</span><span class="sxs-lookup"><span data-stu-id="a053f-143">Profiling Overview</span></span>](profiling-overview.md)  
  
 [<span data-ttu-id="a053f-144">分析接口</span><span class="sxs-lookup"><span data-stu-id="a053f-144">Profiling Interfaces</span></span>](profiling-interfaces.md)  
  
 [<span data-ttu-id="a053f-145">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="a053f-145">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)  
  
 [<span data-ttu-id="a053f-146">分析结构</span><span class="sxs-lookup"><span data-stu-id="a053f-146">Profiling Structures</span></span>](profiling-structures.md)
