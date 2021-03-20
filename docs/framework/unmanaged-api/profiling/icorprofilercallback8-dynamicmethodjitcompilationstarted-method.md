---
description: 了解详细信息： ICorProfilerCallback8：:D ynamicMethodJITCompilationStarted 方法
title: ICorProfilerCallback8：:D ynamicMethodJITCompilationStarted 方法
ms.date: 04/10/2018
api_name:
- ICorProfilerCallback8.DynamicMethodJITCompilationStarted
api_location:
- mscorwks.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: c03251ab3120fd93cbb8e6c2f1bb62a4527a92bb
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104759919"
---
# <a name="icorprofilercallback8dynamicmethodjitcompilationstarted-method"></a><span data-ttu-id="b9db3-103">ICorProfilerCallback8：:D ynamicMethodJITCompilationStarted 方法</span><span class="sxs-lookup"><span data-stu-id="b9db3-103">ICorProfilerCallback8::DynamicMethodJITCompilationStarted Method</span></span>

<span data-ttu-id="b9db3-104">[.NET Framework 4.7 及更高版本中支持]</span><span class="sxs-lookup"><span data-stu-id="b9db3-104">[Supported in the .NET Framework 4.7 and later versions]</span></span>  
  
<span data-ttu-id="b9db3-105">当动态方法的 JIT 编译开始时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="b9db3-105">Notifies the profiler whenever JIT compilation of a dynamic method has started.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="b9db3-106">语法</span><span class="sxs-lookup"><span data-stu-id="b9db3-106">Syntax</span></span>  
  
```cpp  
HRESULT DynamicMethodJITCompilationStarted(  
     [in]  FunctionID  functionId,
     [in]  BOOL        fIsSafeToBlock,
     [in]  LPCBYTE     pILHeader,
     [in]  LONG        cbILHeader
);  
```  
  
## <a name="parameters"></a><span data-ttu-id="b9db3-107">parameters</span><span class="sxs-lookup"><span data-stu-id="b9db3-107">Parameters</span></span>  

`functionId`  
<span data-ttu-id="b9db3-108">中开始 JIT 编译的内存中函数的标识符。</span><span class="sxs-lookup"><span data-stu-id="b9db3-108">[in] The identifier of the in-memory function for which JIT compilation is started.</span></span>

<span data-ttu-id="b9db3-109">`fIsSafeToBlock` [in] `true` 指示阻止可能会导致运行时等待调用线程从该回调返回; `false` 指示阻止操作不会影响运行时的操作。</span><span class="sxs-lookup"><span data-stu-id="b9db3-109">`fIsSafeToBlock` [in] `true` to indicate that blocking may cause the runtime to wait for the calling thread to return from this callback; `false` to indicate that blocking will not affect the operation of the runtime.</span></span>  

<span data-ttu-id="b9db3-110">`pILHeader` 中指向该方法的 IL 标头的第一个字节的指针。</span><span class="sxs-lookup"><span data-stu-id="b9db3-110">`pILHeader` [in] A pointer to the first byte of the method's IL header.</span></span>

<span data-ttu-id="b9db3-111">`cbILHeader` 中IL 标头中的字节数。</span><span class="sxs-lookup"><span data-stu-id="b9db3-111">`cbILHeader` [in] The number of bytes in the IL header.</span></span>

## <a name="remarks"></a><span data-ttu-id="b9db3-112">备注</span><span class="sxs-lookup"><span data-stu-id="b9db3-112">Remarks</span></span>  

<span data-ttu-id="b9db3-113">只要 JIT 编译动态方法，就会触发此回调。</span><span class="sxs-lookup"><span data-stu-id="b9db3-113">This callback is triggered whenever a dynamic method is JIT-compiled.</span></span> <span data-ttu-id="b9db3-114">这包括各种 IL 存根和 LCG 方法。</span><span class="sxs-lookup"><span data-stu-id="b9db3-114">This includes various IL stubs and LCG methods.</span></span> <span data-ttu-id="b9db3-115">其目标是为探查器编写者提供足够的信息，以便向用户标识编译的方法。</span><span class="sxs-lookup"><span data-stu-id="b9db3-115">Its goal is to provide profiler writers with enough information to identify the compiled method to users.</span></span>

> [!NOTE]
> <span data-ttu-id="b9db3-116">`functionId` 由于动态方法没有元数据，因此不能使用值来解析为其元数据标记。</span><span class="sxs-lookup"><span data-stu-id="b9db3-116">`functionId` values cannot be used to resolve to their metadata tokens, because dynamic methods have no metadata.</span></span>

<span data-ttu-id="b9db3-117">`pILHeader`指针仅在回调期间有效。</span><span class="sxs-lookup"><span data-stu-id="b9db3-117">The `pILHeader` pointer is only valid during the callback.</span></span>

## <a name="requirements"></a><span data-ttu-id="b9db3-118">要求</span><span class="sxs-lookup"><span data-stu-id="b9db3-118">Requirements</span></span>  

 <span data-ttu-id="b9db3-119">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="b9db3-119">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="b9db3-120">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="b9db3-120">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="b9db3-121">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="b9db3-121">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="b9db3-122">**.NET Framework 版本：**[!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span><span class="sxs-lookup"><span data-stu-id="b9db3-122">**.NET Framework Versions:** [!INCLUDE[net_current_v47plus](../../../../includes/net-current-v47plus.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="b9db3-123">另请参阅</span><span class="sxs-lookup"><span data-stu-id="b9db3-123">See also</span></span>

- [<span data-ttu-id="b9db3-124">DynamicMethodJITCompilationFinished 方法</span><span class="sxs-lookup"><span data-stu-id="b9db3-124">DynamicMethodJITCompilationFinished Method</span></span>](icorprofilercallback8-dynamicmethodjitcompilationfinished-method.md)
- [<span data-ttu-id="b9db3-125">ICorProfilerCallback8 接口</span><span class="sxs-lookup"><span data-stu-id="b9db3-125">ICorProfilerCallback8 Interface</span></span>](icorprofilercallback8-interface.md)
