---
description: 了解详细信息： ICorProfilerCallback：： AppDomainShutdownFinished 方法
title: ICorProfilerCallback::AppDomainShutdownFinished 方法
ms.date: 03/30/2017
api_name:
- ICorProfilerCallback.AppDomainShutdownFinished
api_location:
- mscorwks.dll
api_type:
- COM
f1_keywords:
- ICorProfilerCallback::AppDomainShutdownFinished
helpviewer_keywords:
- AppDomainShutdownFinished method [.NET Framework profiling]
- ICorProfilerCallback::AppDomainShutdownFinished method [.NET Framework profiling]
ms.assetid: 52794819-0a59-4bb1-a265-0f158cd5cd65
topic_type:
- apiref
ms.openlocfilehash: 8cd45f73741bd26cb54fa85d7d6a186ebaeab5d8
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760686"
---
# <a name="icorprofilercallbackappdomainshutdownfinished-method"></a><span data-ttu-id="2f126-103">ICorProfilerCallback::AppDomainShutdownFinished 方法</span><span class="sxs-lookup"><span data-stu-id="2f126-103">ICorProfilerCallback::AppDomainShutdownFinished Method</span></span>

<span data-ttu-id="2f126-104">通知探查器已从进程中卸载应用程序域。</span><span class="sxs-lookup"><span data-stu-id="2f126-104">Notifies the profiler that an application domain has been unloaded from a process.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="2f126-105">语法</span><span class="sxs-lookup"><span data-stu-id="2f126-105">Syntax</span></span>  
  
```cpp  
HRESULT AppDomainShutdownFinished(  
    [in] AppDomainID appDomainId,  
    [in] HRESULT     hrStatus);  
```  
  
## <a name="parameters"></a><span data-ttu-id="2f126-106">parameters</span><span class="sxs-lookup"><span data-stu-id="2f126-106">Parameters</span></span>

<span data-ttu-id="2f126-107">`appDomainId` 中标识要在其中存储应用程序的程序集的域。</span><span class="sxs-lookup"><span data-stu-id="2f126-107">`appDomainId` [in] Identifies the domain in which the application's assemblies are stored.</span></span>

<span data-ttu-id="2f126-108">`hrStatus` 中一个 HRESULT，指示是否已成功卸载应用程序域。</span><span class="sxs-lookup"><span data-stu-id="2f126-108">`hrStatus` [in] An HRESULT that indicates whether the application domain was unloaded successfully.</span></span>

## <a name="remarks"></a><span data-ttu-id="2f126-109">备注</span><span class="sxs-lookup"><span data-stu-id="2f126-109">Remarks</span></span>  

 <span data-ttu-id="2f126-110">在 `appDomainId` [ICorProfilerCallback：： AppDomainShutdownStarted](icorprofilercallback-appdomainshutdownstarted-method.md) 方法返回后，的值对信息请求无效。</span><span class="sxs-lookup"><span data-stu-id="2f126-110">The value of `appDomainId` is not valid for an information request after the [ICorProfilerCallback::AppDomainShutdownStarted](icorprofilercallback-appdomainshutdownstarted-method.md) method returns.</span></span>  
  
 <span data-ttu-id="2f126-111">在回调后，卸载应用程序域的某些部分可能会继续 `AppDomainCreationFinished` 。</span><span class="sxs-lookup"><span data-stu-id="2f126-111">Some parts of unloading the application domain might continue after the `AppDomainCreationFinished` callback.</span></span> <span data-ttu-id="2f126-112">中的 HRESULT 失败 `hrStatus` 表示失败。</span><span class="sxs-lookup"><span data-stu-id="2f126-112">A failure HRESULT in `hrStatus` indicates a failure.</span></span> <span data-ttu-id="2f126-113">但是，中的成功 HRESULT `hrStatus` 仅指示卸载应用程序域的第一部分已成功。</span><span class="sxs-lookup"><span data-stu-id="2f126-113">However, a success HRESULT in `hrStatus` indicates only that the first part of unloading the application domain has succeeded.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="2f126-114">要求</span><span class="sxs-lookup"><span data-stu-id="2f126-114">Requirements</span></span>  

 <span data-ttu-id="2f126-115">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="2f126-115">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="2f126-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="2f126-116">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="2f126-117">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="2f126-117">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="2f126-118">**.NET Framework 版本：**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="2f126-118">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="2f126-119">另请参阅</span><span class="sxs-lookup"><span data-stu-id="2f126-119">See also</span></span>

- [<span data-ttu-id="2f126-120">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="2f126-120">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
