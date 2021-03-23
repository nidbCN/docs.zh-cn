---
description: 了解详细信息： ICorProfilerCallback10：： EventPipeProviderCreated 方法
title: ICorProfilerCallback10：： EventPipeProviderCreated 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerCallback10.EventPipeProviderCreated
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 4602438148a369f2a2321a2ec2e959cc375e37cf
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805579"
---
# <a name="icorprofilercallback10eventpipeprovidercreated-method"></a><span data-ttu-id="9668f-103">ICorProfilerCallback10：： EventPipeProviderCreated 方法</span><span class="sxs-lookup"><span data-stu-id="9668f-103">ICorProfilerCallback10::EventPipeProviderCreated Method</span></span>

<span data-ttu-id="9668f-104">每当创建 EventPipe 提供程序时，通知探查器。</span><span class="sxs-lookup"><span data-stu-id="9668f-104">Notifies the profiler whenever an EventPipe provider is created.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="9668f-105">语法</span><span class="sxs-lookup"><span data-stu-id="9668f-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeProviderCreated([in] EVENTPIPE_PROVIDER provider); 
```  
  
## <a name="parameters"></a><span data-ttu-id="9668f-106">参数</span><span class="sxs-lookup"><span data-stu-id="9668f-106">Parameters</span></span>

<span data-ttu-id="9668f-107">`provider` 中已创建的提供程序。</span><span class="sxs-lookup"><span data-stu-id="9668f-107">`provider` [in] The provider that was created.</span></span>

## <a name="requirements"></a><span data-ttu-id="9668f-108">要求</span><span class="sxs-lookup"><span data-stu-id="9668f-108">Requirements</span></span>  

<span data-ttu-id="9668f-109">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="9668f-109">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="9668f-110">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="9668f-110">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="9668f-111">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="9668f-111">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="9668f-112">另请参阅</span><span class="sxs-lookup"><span data-stu-id="9668f-112">See also</span></span>

- [<span data-ttu-id="9668f-113">分析接口</span><span class="sxs-lookup"><span data-stu-id="9668f-113">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="9668f-114">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="9668f-114">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="9668f-115">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="9668f-115">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
- [<span data-ttu-id="9668f-116">ICorProfilerInfo12. EventPipeAddProviderToSession 方法</span><span class="sxs-lookup"><span data-stu-id="9668f-116">ICorProfilerInfo12.EventPipeAddProviderToSession Method</span></span>](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)
