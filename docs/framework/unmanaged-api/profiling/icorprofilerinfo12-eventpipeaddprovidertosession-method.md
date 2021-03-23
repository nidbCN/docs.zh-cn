---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeAddProviderToSession 方法
title: ICorProfilerInfo12：： EventPipeAddProviderToSession 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeAddProviderToSession
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 70654660c496211c20459ef9ba37dfbd96b829b3
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805683"
---
# <a name="icorprofilerinfo12eventpipeaddprovidertosession-method"></a><span data-ttu-id="9c72d-103">ICorProfilerInfo12：： EventPipeAddProviderToSession 方法</span><span class="sxs-lookup"><span data-stu-id="9c72d-103">ICorProfilerInfo12::EventPipeAddProviderToSession Method</span></span>

<span data-ttu-id="9c72d-104">向现有的 EventPipe 会话添加提供程序。</span><span class="sxs-lookup"><span data-stu-id="9c72d-104">Adds a provider to an existing EventPipe session.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="9c72d-105">语法</span><span class="sxs-lookup"><span data-stu-id="9c72d-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeAddProviderToSession(
        [in] EVENTPIPE_SESSION                 session,
        [in] COR_PRF_EVENTPIPE_PROVIDER_CONFIG providerConfig);
```  
  
## <a name="parameters"></a><span data-ttu-id="9c72d-106">参数</span><span class="sxs-lookup"><span data-stu-id="9c72d-106">Parameters</span></span>

<span data-ttu-id="9c72d-107">`session` 中要将提供程序添加到的会话的 ID。</span><span class="sxs-lookup"><span data-stu-id="9c72d-107">`session` [in] The ID of the session to add the provider to.</span></span>

<span data-ttu-id="9c72d-108">`providerConfig` 中 `COR_PRF_EVENTPIPE_PROVIDER_CONFIG` 描述要添加到会话中的提供程序的。</span><span class="sxs-lookup"><span data-stu-id="9c72d-108">`providerConfig` [in] A `COR_PRF_EVENTPIPE_PROVIDER_CONFIG` describing the provider to add to the session.</span></span>

## <a name="requirements"></a><span data-ttu-id="9c72d-109">要求</span><span class="sxs-lookup"><span data-stu-id="9c72d-109">Requirements</span></span>  

<span data-ttu-id="9c72d-110">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="9c72d-110">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="9c72d-111">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="9c72d-111">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="9c72d-112">另请参阅</span><span class="sxs-lookup"><span data-stu-id="9c72d-112">See also</span></span>

- [<span data-ttu-id="9c72d-113">EventPipe 概述</span><span class="sxs-lookup"><span data-stu-id="9c72d-113">EventPipe Overview</span></span>](../../../core/diagnostics/eventpipe.md)
- [<span data-ttu-id="9c72d-114">众所周知的 EventProviders</span><span class="sxs-lookup"><span data-stu-id="9c72d-114">Well Known EventProviders</span></span>](../../../core/diagnostics/well-known-event-providers.md)
- [<span data-ttu-id="9c72d-115">分析接口</span><span class="sxs-lookup"><span data-stu-id="9c72d-115">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="9c72d-116">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="9c72d-116">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="9c72d-117">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="9c72d-117">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="9c72d-118">COR_PRF_EVENTPIPE_PROVIDER_CONFIG 结构</span><span class="sxs-lookup"><span data-stu-id="9c72d-118">COR_PRF_EVENTPIPE_PROVIDER_CONFIG Structure</span></span>](cor-prf-eventpipe-provider-config-structure.md)
