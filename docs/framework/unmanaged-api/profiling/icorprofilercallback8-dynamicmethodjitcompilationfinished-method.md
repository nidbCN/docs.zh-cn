---
description: 了解详细信息： ICorProfilerCallback8：:D ynamicMethodJITCompilationFinished 方法
title: ICorProfilerCallback8：:D ynamicMethodJITCompilationFinished 方法
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback8.DynamicMethodJITCompilationFinished
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: d610024af9959790b37a724c2bdbf4dabc89dd20
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759815"
---
# <a name="icorprofilercallback8dynamicmethodjitcompilationfinished-method"></a><span data-ttu-id="79652-103">ICorProfilerCallback8：:D ynamicMethodJITCompilationFinished 方法</span><span class="sxs-lookup"><span data-stu-id="79652-103">ICorProfilerCallback8::DynamicMethodJITCompilationFinished Method</span></span>

<span data-ttu-id="79652-104">[.NET Framework 4.7 及更高版本中支持]</span><span class="sxs-lookup"><span data-stu-id="79652-104">[Supported in the .NET Framework 4.7 and later versions]</span></span>  
  
<span data-ttu-id="79652-105">当动态方法的 JIT 编译完成时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="79652-105">Notifies the profiler whenever JIT compilation of a dynamic method has completed.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="79652-106">语法</span><span class="sxs-lookup"><span data-stu-id="79652-106">Syntax</span></span>  
  
```cpp  
HRESULT DynamicMethodJITCompilationFinished(  
     [in]  FunctionID  functionId,
     [in]  BOOL        hrStatus,
     [in]  BOOL        fIsSafeToBlock
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="79652-107">parameters</span><span class="sxs-lookup"><span data-stu-id="79652-107">Parameters</span></span>  

`functionId`  
<span data-ttu-id="79652-108">中开始 JIT 编译的内存中函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="79652-108">[in] The identifier of the in-memory function for which JIT compilation is started.</span></span>

<span data-ttu-id="79652-109">`hrStatus` 中一个值，该值指示 JIT 编译是否成功。</span><span class="sxs-lookup"><span data-stu-id="79652-109">`hrStatus` [in] A value that indicates whether the JIT compilation was successful.</span></span>

<span data-ttu-id="79652-110">`fIsSafeToBlock` [in] `true` 指示阻止可能会导致运行时等待调用线程从该回调返回; `false` 指示阻止操作不会影响运行时的操作。</span><span class="sxs-lookup"><span data-stu-id="79652-110">`fIsSafeToBlock` [in] `true` to indicate that blocking may cause the runtime to wait for the calling thread to return from this callback; `false` to indicate that blocking will not affect the operation of the runtime.</span></span>  

## <a name="remarks"></a><span data-ttu-id="79652-111">备注</span><span class="sxs-lookup"><span data-stu-id="79652-111">Remarks</span></span>  

<span data-ttu-id="79652-112">只要动态方法的 JIT 编译完成，就会触发此回调。</span><span class="sxs-lookup"><span data-stu-id="79652-112">This callback is triggered whenever JIT compilation of a dynamic method has finished.</span></span> <span data-ttu-id="79652-113">这包括各种 IL 存根和 LCG 方法。</span><span class="sxs-lookup"><span data-stu-id="79652-113">This includes various IL stubs and LCG methods.</span></span> <span data-ttu-id="79652-114">其目标是为探查器编写者提供足够的信息，以便向用户标识编译的方法。</span><span class="sxs-lookup"><span data-stu-id="79652-114">Its goal is to provide profiler writers with enough information to identify the compiled method to users.</span></span>

> [!NOTE]
> <span data-ttu-id="79652-115">`functionId` 由于动态方法没有元数据，因此不能使用值来解析为其元数据标记。</span><span class="sxs-lookup"><span data-stu-id="79652-115">`functionId` values cannot be used to resolve to their metadata tokens, because dynamic methods have no metadata.</span></span>

## <a name="requirements"></a><span data-ttu-id="79652-116">要求</span><span class="sxs-lookup"><span data-stu-id="79652-116">Requirements</span></span>  

 <span data-ttu-id="79652-117">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="79652-117">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="79652-118">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="79652-118">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="79652-119">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="79652-119">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="79652-120">**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="79652-120">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="79652-121">另请参阅</span><span class="sxs-lookup"><span data-stu-id="79652-121">See also</span></span>

- [<span data-ttu-id="79652-122">DynamicMethodJITCompilationStarted 方法</span><span class="sxs-lookup"><span data-stu-id="79652-122">DynamicMethodJITCompilationStarted Method</span></span>](icorprofilercallback8-dynamicmethodjitcompilationstarted-method.md)
- [<span data-ttu-id="79652-123">ICorProfilerCallback8 接口</span><span class="sxs-lookup"><span data-stu-id="79652-123">ICorProfilerCallback8 Interface</span></span>](icorprofilercallback8-interface.md)
