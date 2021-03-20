---
description: 了解详细信息： ICorProfilerCallback：： AppDomainCreationStarted 方法
title: ICorProfilerCallback::AppDomainCreationStarted 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AppDomainCreationStarted
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AppDomainCreationStarted
helpviewer_keywords:
- AppDomainCreationStarted method [.NET Framework profiling]
- ICorProfilerCallback::AppDomainCreationStarted method [.NET Framework profiling]
ms.assetid: b2a8240b-07fe-4859-bb2b-7d3adbfa0a9f
topic_type:
- apiref
ms.openlocfilehash: b783bb652bed3b600c1e4524810620bab68f5f7a
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760699"
---
# <a name="icorprofilercallbackappdomaincreationstarted-method"></a><span data-ttu-id="13084-103">ICorProfilerCallback::AppDomainCreationStarted 方法</span><span class="sxs-lookup"><span data-stu-id="13084-103">ICorProfilerCallback::AppDomainCreationStarted Method</span></span>

<span data-ttu-id="13084-104">通知探查器正在创建应用程序域。</span><span class="sxs-lookup"><span data-stu-id="13084-104">Notifies the profiler that an application domain is being created.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="13084-105">语法</span><span class="sxs-lookup"><span data-stu-id="13084-105">Syntax</span></span>  
  
```cpp  
HRESULT AppDomainCreationStarted(  
    [in] AppDomainID appDomainId);  
```  
  
## <a name="parameters"></a><span data-ttu-id="13084-106">parameters</span><span class="sxs-lookup"><span data-stu-id="13084-106">Parameters</span></span>

<span data-ttu-id="13084-107">`appDomainId` 中标识正在创建的域。</span><span class="sxs-lookup"><span data-stu-id="13084-107">`appDomainId` [in] Identifies the domain which is being created.</span></span>
  
## <a name="remarks"></a><span data-ttu-id="13084-108">备注</span><span class="sxs-lookup"><span data-stu-id="13084-108">Remarks</span></span>  

 <span data-ttu-id="13084-109">在调用 [ICorProfilerCallback：： AppDomainCreationFinished](icorprofilercallback-appdomaincreationfinished-method.md) 方法之前，ID 对任何信息请求都无效。</span><span class="sxs-lookup"><span data-stu-id="13084-109">The ID is not valid for any information request until the [ICorProfilerCallback::AppDomainCreationFinished](icorprofilercallback-appdomaincreationfinished-method.md) method is called.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="13084-110">要求</span><span class="sxs-lookup"><span data-stu-id="13084-110">Requirements</span></span>  

 <span data-ttu-id="13084-111">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="13084-111">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="13084-112">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="13084-112">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="13084-113">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="13084-113">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="13084-114">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="13084-114">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="13084-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="13084-115">See also</span></span>

- [<span data-ttu-id="13084-116">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="13084-116">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
