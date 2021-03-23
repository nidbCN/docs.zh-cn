---
description: 了解详细信息： ICorProfilerInfo12：： EventPipeGetProviderInfo 方法
title: ICorProfilerInfo12：： EventPipeGetProviderInfo 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo12.EventPipeGetProviderInfo
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 7bc7ffe1c31e88dc1c65f1670f9bd179e732abfe
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805696"
---
# <a name="icorprofilerinfo12eventpipegetproviderinfo-method"></a><span data-ttu-id="c6bae-103">ICorProfilerInfo12：： EventPipeGetProviderInfo 方法</span><span class="sxs-lookup"><span data-stu-id="c6bae-103">ICorProfilerInfo12::EventPipeGetProviderInfo Method</span></span>

<span data-ttu-id="c6bae-104">创建一个 EventPipe 提供程序，探查器可以使用该提供程序为要接收的其他 EventPipe 侦听器编写事件。</span><span class="sxs-lookup"><span data-stu-id="c6bae-104">Creates an EventPipe provider that the profiler can use to write events for other EventPipe listeners to receive.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="c6bae-105">语法</span><span class="sxs-lookup"><span data-stu-id="c6bae-105">Syntax</span></span>  
  
```cpp  
    HRESULT EventPipeGetProviderInfo(
                [in] EVENTPIPE_PROVIDER provider,
                [in]  ULONG      cchName,
                [out] ULONG      *pcchName,
                [out, annotation("_Out_writes_to_(cchName, *pcchName)")]
                      WCHAR      providerName[]);
```  
  
## <a name="parameters"></a><span data-ttu-id="c6bae-106">参数</span><span class="sxs-lookup"><span data-stu-id="c6bae-106">Parameters</span></span>

<span data-ttu-id="c6bae-107">`provider` 中要为其提供名称的提供程序的 ID。</span><span class="sxs-lookup"><span data-stu-id="c6bae-107">`provider` [in] The ID of the provider to provide the name for.</span></span>

<span data-ttu-id="c6bae-108">`cchName` 中的大小（以字符为字符） `providerName` 。</span><span class="sxs-lookup"><span data-stu-id="c6bae-108">`cchName` [in] The size, in characters, of `providerName`.</span></span>

<span data-ttu-id="c6bae-109">`pcchName` 弄指向的总字符长度的指针 `providerName` 。</span><span class="sxs-lookup"><span data-stu-id="c6bae-109">`pcchName` [out] A pointer to the total character length of `providerName`.</span></span>

<span data-ttu-id="c6bae-110">`providerName` 弄调用方提供宽字符缓冲区。</span><span class="sxs-lookup"><span data-stu-id="c6bae-110">`providerName` [out] A caller provided wide character buffer.</span></span> <span data-ttu-id="c6bae-111">当函数返回时，缓冲区将包含提供程序的名称。</span><span class="sxs-lookup"><span data-stu-id="c6bae-111">When the function returns the buffer will contain the name of the provider.</span></span>

## <a name="requirements"></a><span data-ttu-id="c6bae-112">要求</span><span class="sxs-lookup"><span data-stu-id="c6bae-112">Requirements</span></span>  

<span data-ttu-id="c6bae-113">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="c6bae-113">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="c6bae-114">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="c6bae-114">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="c6bae-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="c6bae-115">See also</span></span>

- [<span data-ttu-id="c6bae-116">EventPipe 概述</span><span class="sxs-lookup"><span data-stu-id="c6bae-116">EventPipe Overview</span></span>](../../../core/diagnostics/eventpipe.md)
- [<span data-ttu-id="c6bae-117">众所周知的 EventProviders</span><span class="sxs-lookup"><span data-stu-id="c6bae-117">Well Known EventProviders</span></span>](../../../core/diagnostics/well-known-event-providers.md)
- [<span data-ttu-id="c6bae-118">分析接口</span><span class="sxs-lookup"><span data-stu-id="c6bae-118">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="c6bae-119">ICorProfilerCallback10 接口</span><span class="sxs-lookup"><span data-stu-id="c6bae-119">ICorProfilerCallback10 Interface</span></span>](icorprofilercallback10-interface.md)
- [<span data-ttu-id="c6bae-120">ICorProfilerInfo12 接口</span><span class="sxs-lookup"><span data-stu-id="c6bae-120">ICorProfilerInfo12 Interface</span></span>](icorprofilerinfo12-interface.md)
