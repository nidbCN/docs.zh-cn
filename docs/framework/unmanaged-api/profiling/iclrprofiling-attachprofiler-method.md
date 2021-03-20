---
description: 了解详细信息： ICLRProfiling：： AttachProfiler 方法
title: ICLRProfiling::AttachProfiler 方法
ms.date: 03/30/2017
api_name:
- IClrProfiling.AttachProfiler Method
api_location:
- Mscorwks.dll
api_type:
- COM
f1_keywords:
- IClrProfiling::AttachProfiler
helpviewer_keywords:
- AttachProfiler method [.NET Framework profiling]
- IClrProfiling::AttachProfiler method [.NET Framework profiling]
ms.assetid: 535a6839-c443-405b-a6f4-e2af90725d5b
topic_type:
- apiref
ms.openlocfilehash: 00ae5ca76462d8800a77c2869ef73703a4f1d980
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104760764"
---
# <a name="iclrprofilingattachprofiler-method"></a><span data-ttu-id="b738e-103">ICLRProfiling::AttachProfiler 方法</span><span class="sxs-lookup"><span data-stu-id="b738e-103">ICLRProfiling::AttachProfiler Method</span></span>

<span data-ttu-id="b738e-104">将指定的探查器附加到指定的进程中。</span><span class="sxs-lookup"><span data-stu-id="b738e-104">Attaches the specified profiler to the specified process.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="b738e-105">语法</span><span class="sxs-lookup"><span data-stu-id="b738e-105">Syntax</span></span>  
  
```cpp  
HRESULT AttachProfiler(  
  [in] DWORD dwProfileeProcessID,  
  [in] DWORD dwMillisecondsMax,                     // optional  
  [in] const CLSID * pClsidProfiler,  
  [in] LPCWSTR wszProfilerPath,                     // optional  
  [in] size_is(cbClientData)] void * pvClientData,  // optional  
  [in] UINT cbClientData);                          // optional  
```  
  
## <a name="parameters"></a><span data-ttu-id="b738e-106">parameters</span><span class="sxs-lookup"><span data-stu-id="b738e-106">Parameters</span></span>

<span data-ttu-id="b738e-107">`dwProfileeProcessID` 中应将探查器附加到的进程的进程 ID。</span><span class="sxs-lookup"><span data-stu-id="b738e-107">`dwProfileeProcessID` [in] The process ID of the process to which the profiler should be attached.</span></span> <span data-ttu-id="b738e-108">在 64 位计算机上，被分析进程的位数必须匹配调用 `AttachProfiler` 的触发进程的位数。</span><span class="sxs-lookup"><span data-stu-id="b738e-108">On a 64-bit machine, the profiled process's bitness must match the bitness of the trigger process that is calling `AttachProfiler`.</span></span> <span data-ttu-id="b738e-109">如果调用 `AttachProfiler` 的用户帐户具有管理特权，则目标进程可能是系统上的任何进程。</span><span class="sxs-lookup"><span data-stu-id="b738e-109">If the user account under which `AttachProfiler` is called has administrative privileges, the target process may be any process on the system.</span></span> <span data-ttu-id="b738e-110">否则，相同的用户帐户必须拥有目标进程。</span><span class="sxs-lookup"><span data-stu-id="b738e-110">Otherwise, the target process must be owned by the same user account.</span></span>

<span data-ttu-id="b738e-111">`dwMillisecondsMax` 中完成的持续时间（以毫秒为单位） `AttachProfiler` 。</span><span class="sxs-lookup"><span data-stu-id="b738e-111">`dwMillisecondsMax` [in] The time duration, in milliseconds, for `AttachProfiler` to complete.</span></span> <span data-ttu-id="b738e-112">触发器进程应传递一个特定探查器足以完成其初始化的已知超时。</span><span class="sxs-lookup"><span data-stu-id="b738e-112">The trigger process should pass a timeout that is known to be sufficient for the particular profiler to complete its initialization.</span></span>
  
<span data-ttu-id="b738e-113">`pClsidProfiler` 中一个指针，指向要加载的探查器的 CLSID。</span><span class="sxs-lookup"><span data-stu-id="b738e-113">`pClsidProfiler` [in] A pointer to the CLSID of the profiler to be loaded.</span></span> <span data-ttu-id="b738e-114">`AttachProfiler` 返回后，触发器进程可重用此内存。</span><span class="sxs-lookup"><span data-stu-id="b738e-114">The trigger process can reuse this memory after `AttachProfiler` returns.</span></span>

<span data-ttu-id="b738e-115">`wszProfilerPath` 中要加载的探查器的 DLL 文件的完整路径。</span><span class="sxs-lookup"><span data-stu-id="b738e-115">`wszProfilerPath` [in] The full path to the profiler’s DLL file to be loaded.</span></span> <span data-ttu-id="b738e-116">此字符串应包含不超过 260 个字符，包括 null 终止符。</span><span class="sxs-lookup"><span data-stu-id="b738e-116">This string should contain no more than 260 characters, including the null terminator.</span></span> <span data-ttu-id="b738e-117">如果 `wszProfilerPath` 为 null 或为空字符串，公共语言运行时 (CLR) 将通过在 `pClsidProfiler` 指向的 CLSID 的注册表中查找探查器的 DLL 文件的位置。</span><span class="sxs-lookup"><span data-stu-id="b738e-117">If `wszProfilerPath` is null or an empty string, the common language runtime (CLR) will try to find the location of the profiler’s DLL file by looking in the registry for the CLSID that `pClsidProfiler` points to.</span></span>

<span data-ttu-id="b738e-118">`pvClientData` 中指向由 [ICorProfilerCallback3：： InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) 方法传递到探查器的数据的指针。</span><span class="sxs-lookup"><span data-stu-id="b738e-118">`pvClientData` [in] A pointer to data to be passed to the profiler by the [ICorProfilerCallback3::InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) method.</span></span> <span data-ttu-id="b738e-119">`AttachProfiler` 返回后，触发器进程可重用此内存。</span><span class="sxs-lookup"><span data-stu-id="b738e-119">The trigger process can reuse this memory after `AttachProfiler` returns.</span></span> <span data-ttu-id="b738e-120">如果 `pvClientData` 为 null，`cbClientData` 必须为 0（零）。</span><span class="sxs-lookup"><span data-stu-id="b738e-120">If `pvClientData` is null, `cbClientData` must be 0 (zero).</span></span>

<span data-ttu-id="b738e-121">`cbClientData` 中指向的数据的大小（以字节为单位） `pvClientData` 。</span><span class="sxs-lookup"><span data-stu-id="b738e-121">`cbClientData` [in] The size, in bytes, of the data that `pvClientData` points to.</span></span>

## <a name="return-value"></a><span data-ttu-id="b738e-122">返回值</span><span class="sxs-lookup"><span data-stu-id="b738e-122">Return Value</span></span>  

 <span data-ttu-id="b738e-123">此方法将返回以下 HRESULT。</span><span class="sxs-lookup"><span data-stu-id="b738e-123">This method returns the following HRESULTs.</span></span>  
  
|<span data-ttu-id="b738e-124">HRESULT</span><span class="sxs-lookup"><span data-stu-id="b738e-124">HRESULT</span></span>|<span data-ttu-id="b738e-125">说明</span><span class="sxs-lookup"><span data-stu-id="b738e-125">Description</span></span>|  
|-------------|-----------------|  
|<span data-ttu-id="b738e-126">S_OK</span><span class="sxs-lookup"><span data-stu-id="b738e-126">S_OK</span></span>|<span data-ttu-id="b738e-127">指定的探查器已成功附加到目标进程中。</span><span class="sxs-lookup"><span data-stu-id="b738e-127">The specified profiler has successfully attached to the target process.</span></span>|  
|<span data-ttu-id="b738e-128">CORPROF_E_PROFILER_ALREADY_ACTIVE</span><span class="sxs-lookup"><span data-stu-id="b738e-128">CORPROF_E_PROFILER_ALREADY_ACTIVE</span></span>|<span data-ttu-id="b738e-129">已有活动的或附加到目标进程的探查器。</span><span class="sxs-lookup"><span data-stu-id="b738e-129">There is already a profiler active or attaching to the target process.</span></span>|  
|<span data-ttu-id="b738e-130">CORPROF_E_PROFILER_NOT_ATTACHABLE</span><span class="sxs-lookup"><span data-stu-id="b738e-130">CORPROF_E_PROFILER_NOT_ATTACHABLE</span></span>|<span data-ttu-id="b738e-131">指定的探查器不支持附件。</span><span class="sxs-lookup"><span data-stu-id="b738e-131">The specified profiler does not support attachment.</span></span> <span data-ttu-id="b738e-132">触发进程可能会尝试附加不同的探查器。</span><span class="sxs-lookup"><span data-stu-id="b738e-132">The trigger process may attempt to attach a different profiler.</span></span>|  
|<span data-ttu-id="b738e-133">CORPROF_E_PROFILEE_INCOMPATIBLE_WITH_TRIGGER</span><span class="sxs-lookup"><span data-stu-id="b738e-133">CORPROF_E_PROFILEE_INCOMPATIBLE_WITH_TRIGGER</span></span>|<span data-ttu-id="b738e-134">无法请求探查器附件，因为目标进程的版本与当前正在调用 `AttachProfiler` 的进程不兼容。</span><span class="sxs-lookup"><span data-stu-id="b738e-134">Unable to request a profiler attachment, because the version of the target process is incompatible with the current process that is calling `AttachProfiler`.</span></span>|  
|<span data-ttu-id="b738e-135">HRESULT_FROM_WIN32(ERROR_ACCESS_DENIED)</span><span class="sxs-lookup"><span data-stu-id="b738e-135">HRESULT_FROM_WIN32(ERROR_ACCESS_DENIED)</span></span>|<span data-ttu-id="b738e-136">触发进程的用户没有访问目标进程的权限。</span><span class="sxs-lookup"><span data-stu-id="b738e-136">The user of the trigger process does not have access to the target process.</span></span>|  
|<span data-ttu-id="b738e-137">HRESULT_FROM_WIN32(ERROR_PRIVILEGE_NOT_HELD)</span><span class="sxs-lookup"><span data-stu-id="b738e-137">HRESULT_FROM_WIN32(ERROR_PRIVILEGE_NOT_HELD)</span></span>|<span data-ttu-id="b738e-138">触发进程的用户没有将探查器附加到给定的目标进程所必需的特权。</span><span class="sxs-lookup"><span data-stu-id="b738e-138">The user of the trigger process does not have the privileges necessary to attach a profiler to the given target process.</span></span> <span data-ttu-id="b738e-139">应用程序事件日志可能包含详细信息。</span><span class="sxs-lookup"><span data-stu-id="b738e-139">The application event log may contain more information.</span></span>|  
|<span data-ttu-id="b738e-140">CORPROF_E_IPC_FAILED</span><span class="sxs-lookup"><span data-stu-id="b738e-140">CORPROF_E_IPC_FAILED</span></span>|<span data-ttu-id="b738e-141">与目标进程进行通信时发生错误。</span><span class="sxs-lookup"><span data-stu-id="b738e-141">A failure occurred when communicating with the target process.</span></span> <span data-ttu-id="b738e-142">通常在目标进程已关闭时，会发生此类错误。</span><span class="sxs-lookup"><span data-stu-id="b738e-142">This commonly happens if the target process was shutting down.</span></span>|  
|<span data-ttu-id="b738e-143">HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND)</span><span class="sxs-lookup"><span data-stu-id="b738e-143">HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND)</span></span>|<span data-ttu-id="b738e-144">目标进程中不存在或未运行支持附件的 CLR。</span><span class="sxs-lookup"><span data-stu-id="b738e-144">The target process does not exist or is not running a CLR that supports attachment.</span></span> <span data-ttu-id="b738e-145">这可能表示，自调用运行时枚举方法时，就已卸载 CLR。</span><span class="sxs-lookup"><span data-stu-id="b738e-145">This may indicate that the CLR was unloaded since the call to the runtime enumeration method.</span></span>|  
|<span data-ttu-id="b738e-146">HRESULT_FROM_WIN32(ERROR_TIMEOUT)</span><span class="sxs-lookup"><span data-stu-id="b738e-146">HRESULT_FROM_WIN32(ERROR_TIMEOUT)</span></span>|<span data-ttu-id="b738e-147">尚未开始加载探查器，超时就已过期。</span><span class="sxs-lookup"><span data-stu-id="b738e-147">The timeout expired without beginning to load the profiler.</span></span> <span data-ttu-id="b738e-148">可重试附加操作。</span><span class="sxs-lookup"><span data-stu-id="b738e-148">You can retry the attach operation.</span></span> <span data-ttu-id="b738e-149">当目标进程中的终结器运行时间长于超时值时，就会出现超时。</span><span class="sxs-lookup"><span data-stu-id="b738e-149">Timeouts occur when a finalizer in the target process runs for a longer time than the timeout value.</span></span>|  
|<span data-ttu-id="b738e-150">E_INVALIDARG</span><span class="sxs-lookup"><span data-stu-id="b738e-150">E_INVALIDARG</span></span>|<span data-ttu-id="b738e-151">一个或多个参数具有无效值。</span><span class="sxs-lookup"><span data-stu-id="b738e-151">One or more parameters have invalid values.</span></span>|  
|<span data-ttu-id="b738e-152">E_FAIL</span><span class="sxs-lookup"><span data-stu-id="b738e-152">E_FAIL</span></span>|<span data-ttu-id="b738e-153">出现其他未指定错误。</span><span class="sxs-lookup"><span data-stu-id="b738e-153">Some other, unspecified failure occurred.</span></span>|  
|<span data-ttu-id="b738e-154">其他错误代码</span><span class="sxs-lookup"><span data-stu-id="b738e-154">Other error codes</span></span>|<span data-ttu-id="b738e-155">如果探查器的 [ICorProfilerCallback3：： InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) 方法返回一个指示失败的 hresult，则 `AttachProfiler` 返回相同的 hresult。</span><span class="sxs-lookup"><span data-stu-id="b738e-155">If the profiler’s [ICorProfilerCallback3::InitializeForAttach](icorprofilercallback3-initializeforattach-method.md) method returns an HRESULT that indicates failure, `AttachProfiler` returns that same HRESULT.</span></span> <span data-ttu-id="b738e-156">在这种情况下，E_NOTIMPL 将转换为 CORPROF_E_PROFILER_NOT_ATTACHABLE。</span><span class="sxs-lookup"><span data-stu-id="b738e-156">In this case, E_NOTIMPL is converted to CORPROF_E_PROFILER_NOT_ATTACHABLE.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="b738e-157">备注</span><span class="sxs-lookup"><span data-stu-id="b738e-157">Remarks</span></span>  
  
## <a name="memory-management"></a><span data-ttu-id="b738e-158">内存管理</span><span class="sxs-lookup"><span data-stu-id="b738e-158">Memory Management</span></span>  

 <span data-ttu-id="b738e-159">与 COM 约定一致，`AttachProfiler` 的调用方（例如，由探查器开发人员创作的触发器代码）将负责分配和释放 `pvClientData` 参数指向的数据的内存。</span><span class="sxs-lookup"><span data-stu-id="b738e-159">In keeping with COM conventions, the caller of `AttachProfiler` (for example, the trigger code authored by the profiler developer) is responsible for allocating and de-allocating the memory for the data that the `pvClientData` parameter points to.</span></span> <span data-ttu-id="b738e-160">当 CLR 执行 `AttachProfiler` 调用时，会创建 `pvClientData` 指向的内存的副本，并将其传输到目标进程中。</span><span class="sxs-lookup"><span data-stu-id="b738e-160">When the CLR executes the `AttachProfiler` call, it makes a copy of the memory that `pvClientData` points to and transmits it to the target process.</span></span> <span data-ttu-id="b738e-161">当目标进程内部的 CLR 接收到自己的 `pvClientData` 块副本时，会通过 `InitializeForAttach` 方法将块传递给探查器，然后从目标进程释放其 `pvClientData` 块的副本。</span><span class="sxs-lookup"><span data-stu-id="b738e-161">When the CLR inside the target process receives its own copy of the `pvClientData` block, it passes the block to the profiler through the `InitializeForAttach` method, and then deallocates its copy of the `pvClientData` block from the target process.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="b738e-162">要求</span><span class="sxs-lookup"><span data-stu-id="b738e-162">Requirements</span></span>  

 <span data-ttu-id="b738e-163">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="b738e-163">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="b738e-164">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="b738e-164">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="b738e-165">**库：** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="b738e-165">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="b738e-166">**.NET Framework 版本：**[!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="b738e-166">**.NET Framework Versions:** [!INCLUDE[net_current_v40plus](../../../../includes/net-current-v40plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="b738e-167">另请参阅</span><span class="sxs-lookup"><span data-stu-id="b738e-167">See also</span></span>

- [<span data-ttu-id="b738e-168">ICorProfilerCallback 接口</span><span class="sxs-lookup"><span data-stu-id="b738e-168">ICorProfilerCallback Interface</span></span>](icorprofilercallback-interface.md)
- [<span data-ttu-id="b738e-169">ICorProfilerInfo3 接口</span><span class="sxs-lookup"><span data-stu-id="b738e-169">ICorProfilerInfo3 Interface</span></span>](icorprofilerinfo3-interface.md)
- [<span data-ttu-id="b738e-170">分析接口</span><span class="sxs-lookup"><span data-stu-id="b738e-170">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="b738e-171">分析</span><span class="sxs-lookup"><span data-stu-id="b738e-171">Profiling</span></span>](index.md)
