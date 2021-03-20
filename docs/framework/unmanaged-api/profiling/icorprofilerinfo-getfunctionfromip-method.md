---
description: 了解详细信息： ICorProfilerInfo：： GetFunctionFromIP 方法
title: ICorProfilerInfo::GetFunctionFromIP 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerInfo.GetFunctionFromIP
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerInfo::GetFunctionFromIP
helpviewer_keywords:
- GetFunctionFromIP method [.NET Framework profiling]
- ICorProfilerInfo::GetFunctionFromIP method [.NET Framework profiling]
ms.assetid: f069802a-198f-46dd-9f09-4f77adffc9ba
topic_type:
- apiref
ms.openlocfilehash: b21b8ad10c76b2e9eaad773315122c3635845a78
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760244"
---
# <a name="icorprofilerinfogetfunctionfromip-method"></a><span data-ttu-id="83de9-103">ICorProfilerInfo::GetFunctionFromIP 方法</span><span class="sxs-lookup"><span data-stu-id="83de9-103">ICorProfilerInfo::GetFunctionFromIP Method</span></span>

<span data-ttu-id="83de9-104">将托管代码指令指针映射到 `FunctionID` 。</span><span class="sxs-lookup"><span data-stu-id="83de9-104">Maps a managed code instruction pointer to a `FunctionID`.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="83de9-105">语法</span><span class="sxs-lookup"><span data-stu-id="83de9-105">Syntax</span></span>  
  
```cpp  
HRESULT GetFunctionFromIP(  
    [in]  LPCBYTE    ip,  
    [out] FunctionID *pFunctionId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="83de9-106">parameters</span><span class="sxs-lookup"><span data-stu-id="83de9-106">Parameters</span></span>

<span data-ttu-id="83de9-107">`ip` 中托管代码中的指令指针。</span><span class="sxs-lookup"><span data-stu-id="83de9-107">`ip` [in] The instruction pointer in managed code.</span></span>

<span data-ttu-id="83de9-108">`pFunctionId` 弄返回的函数 ID。</span><span class="sxs-lookup"><span data-stu-id="83de9-108">`pFunctionId` [out] The returned function ID.</span></span>

## <a name="requirements"></a><span data-ttu-id="83de9-109">要求</span><span class="sxs-lookup"><span data-stu-id="83de9-109">Requirements</span></span>  

 <span data-ttu-id="83de9-110">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="83de9-110">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="83de9-111">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="83de9-111">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="83de9-112">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="83de9-112">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="83de9-113">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="83de9-113">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="83de9-114">另请参阅</span><span class="sxs-lookup"><span data-stu-id="83de9-114">See also</span></span>

- [<span data-ttu-id="83de9-115">ICorProfilerInfo 接口</span><span class="sxs-lookup"><span data-stu-id="83de9-115">ICorProfilerInfo Interface</span></span>](icorprofilerinfo-interface.md)
