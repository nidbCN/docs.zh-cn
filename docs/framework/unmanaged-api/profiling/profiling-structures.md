---
description: 了解详细信息：分析结构
title: 分析结构
ms.date: 03/30/2017
helpviewer_keywords:
- profiling structures [.NET Framework]
- unmanaged structures [.NET Framework], profiling
- structures [.NET Framework profiling]
ms.assetid: 750385f2-f365-41b1-939f-ca2f2ff9b466
ms.openlocfilehash: 176830cac519f22864ba004b176cb575d80e50e2
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760231"
---
# <a name="profiling-structures"></a><span data-ttu-id="af3ea-103">分析结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-103">Profiling Structures</span></span>

<span data-ttu-id="af3ea-104">本节描述分析 API 使用的非托管结构。</span><span class="sxs-lookup"><span data-stu-id="af3ea-104">This section describes the unmanaged structures that the profiling API uses.</span></span>  
  
## <a name="in-this-section"></a><span data-ttu-id="af3ea-105">本节内容</span><span class="sxs-lookup"><span data-stu-id="af3ea-105">In This Section</span></span>  

 [<span data-ttu-id="af3ea-106">COR_PRF_ASSEMBLY_REFERENCE_INFO 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-106">COR_PRF_ASSEMBLY_REFERENCE_INFO Structure</span></span>](cor-prf-assembly-reference-info-structure.md)  
 <span data-ttu-id="af3ea-107">向公共语言运行时提供有关在执行程序集引用闭包审核时应考虑的引用程序集的信息。</span><span class="sxs-lookup"><span data-stu-id="af3ea-107">Provides the common language runtime with information about a reference assembly that it should consider when performing an assembly reference closure walk.</span></span>  
  
 [<span data-ttu-id="af3ea-108">COR_PRF_CODE_INFO 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-108">COR_PRF_CODE_INFO Structure</span></span>](cor-prf-code-info-structure.md)  
 <span data-ttu-id="af3ea-109">表示存储在内存中的一个本机代码连续块。</span><span class="sxs-lookup"><span data-stu-id="af3ea-109">Represents one contiguous block of native code stored in memory.</span></span>  
  
 [<span data-ttu-id="af3ea-110">COR_PRF_EX_CLAUSE_INFO 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-110">COR_PRF_EX_CLAUSE_INFO Structure</span></span>](cor-prf-ex-clause-info-structure.md)  
 <span data-ttu-id="af3ea-111">存储有关特定的异常子句实例及其关联的帧的信息。</span><span class="sxs-lookup"><span data-stu-id="af3ea-111">Stores information about a specific exception clause instance and its associated frame.</span></span>  
  
 [<span data-ttu-id="af3ea-112">COR_PRF_FUNCTION 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-112">COR_PRF_FUNCTION Structure</span></span>](cor-prf-function-structure.md)  
 <span data-ttu-id="af3ea-113">通过将函数的 ID 与其重新编译的 ID 版本组合起来，提供该函数的唯一表示形式。</span><span class="sxs-lookup"><span data-stu-id="af3ea-113">Provides a unique representation of a function by combining its ID with the ID of its recompiled version.</span></span>  
  
 [<span data-ttu-id="af3ea-114">COR_PRF_FUNCTION_ARGUMENT_INFO 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-114">COR_PRF_FUNCTION_ARGUMENT_INFO Structure</span></span>](cor-prf-function-argument-info-structure.md)  
 <span data-ttu-id="af3ea-115">按从左向右的顺序表示函数的参数。</span><span class="sxs-lookup"><span data-stu-id="af3ea-115">Represents a function's arguments, in left-to-right order.</span></span>  
  
 [<span data-ttu-id="af3ea-116">COR_PRF_FUNCTION_ARGUMENT_RANGE 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-116">COR_PRF_FUNCTION_ARGUMENT_RANGE Structure</span></span>](cor-prf-function-argument-range-structure.md)  
 <span data-ttu-id="af3ea-117">表示内存中按从左向右的顺序连续存储的函数自变量块。</span><span class="sxs-lookup"><span data-stu-id="af3ea-117">Represents a block of function arguments stored contiguously in left-to-right order in memory.</span></span>  
  
 [<span data-ttu-id="af3ea-118">COR_PRF_GC_GENERATION_RANGE 结构</span><span class="sxs-lookup"><span data-stu-id="af3ea-118">COR_PRF_GC_GENERATION_RANGE Structure</span></span>](cor-prf-gc-generation-range-structure.md)  
 <span data-ttu-id="af3ea-119">描述一个正进行垃圾回收的内存范围（即块）。</span><span class="sxs-lookup"><span data-stu-id="af3ea-119">Describes a range (that is, block) of memory that is undergoing garbage collection.</span></span>  

 <span data-ttu-id="af3ea-120">[COR_PRF_EVENTPIPE_PROVIDER_CONFIG 结构](cor-prf-eventpipe-provider-config-structure.md) 描述配置 EventPipe 提供程序所必需的字段。</span><span class="sxs-lookup"><span data-stu-id="af3ea-120">[COR_PRF_EVENTPIPE_PROVIDER_CONFIG Structure](cor-prf-eventpipe-provider-config-structure.md) Describes the fields necessary to configure an EventPipe provider.</span></span>

 <span data-ttu-id="af3ea-121">[COR_PRF_EVENTPIPE_PARAM_DESC 结构](cor-prf-eventpipe-param-desc-structure.md) 描述 EventPipe 事件的参数名称和类型。</span><span class="sxs-lookup"><span data-stu-id="af3ea-121">[COR_PRF_EVENTPIPE_PARAM_DESC Structure](cor-prf-eventpipe-param-desc-structure.md) Describes the parameter name and type for an EventPipe event.</span></span>

 <span data-ttu-id="af3ea-122">[COR_PRF_EVENT_DATA 结构](cor-prf-event-data-structure.md) 描述要写入的 EventPipe 事件的事件数据。</span><span class="sxs-lookup"><span data-stu-id="af3ea-122">[COR_PRF_EVENT_DATA Structure](cor-prf-event-data-structure.md) Describes the event data for an EventPipe event being written.</span></span>
  
## <a name="related-sections"></a><span data-ttu-id="af3ea-123">相关章节</span><span class="sxs-lookup"><span data-stu-id="af3ea-123">Related Sections</span></span>  

 <span data-ttu-id="af3ea-124">COR_DEBUG_IL_TO_NATIVE_MAP</span><span class="sxs-lookup"><span data-stu-id="af3ea-124">COR_DEBUG_IL_TO_NATIVE_MAP</span></span>  
  
 <span data-ttu-id="af3ea-125">COR_IL_MAP</span><span class="sxs-lookup"><span data-stu-id="af3ea-125">COR_IL_MAP</span></span>  
  
 [<span data-ttu-id="af3ea-126">分析概述</span><span class="sxs-lookup"><span data-stu-id="af3ea-126">Profiling Overview</span></span>](profiling-overview.md)  
  
 [<span data-ttu-id="af3ea-127">分析接口</span><span class="sxs-lookup"><span data-stu-id="af3ea-127">Profiling Interfaces</span></span>](profiling-interfaces.md)  
  
 [<span data-ttu-id="af3ea-128">分析全局静态函数</span><span class="sxs-lookup"><span data-stu-id="af3ea-128">Profiling Global Static Functions</span></span>](profiling-global-static-functions.md)  
  
 [<span data-ttu-id="af3ea-129">分析枚举</span><span class="sxs-lookup"><span data-stu-id="af3ea-129">Profiling Enumerations</span></span>](profiling-enumerations.md)
