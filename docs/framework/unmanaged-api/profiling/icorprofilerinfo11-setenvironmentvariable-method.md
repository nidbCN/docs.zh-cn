---
description: 了解详细信息： ICorProfilerInfo11：： SetEnvironmentVariable 方法
title: ICorProfilerInfo11：： SetEnvironmentVariable 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo11.SetEnvironmentVariable
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 77dc3fc992dec037881573323822dc11481a56be
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805594"
---
# <a name="icorprofilerinfo11setenvironmentvariable-method"></a><span data-ttu-id="6027d-103">ICorProfilerInfo11：： SetEnvironmentVariable 方法</span><span class="sxs-lookup"><span data-stu-id="6027d-103">ICorProfilerInfo11::SetEnvironmentVariable Method</span></span>

<span data-ttu-id="6027d-104">设置进程中的环境变量。</span><span class="sxs-lookup"><span data-stu-id="6027d-104">Sets an environment variable in the process.</span></span> <span data-ttu-id="6027d-105">在非 Windows 平台上，运行时将保留环境变量的内部缓存，以确保线程安全。</span><span class="sxs-lookup"><span data-stu-id="6027d-105">On non-Windows platforms the runtime keeps an internal cache of environment variables to ensure thread safety.</span></span> <span data-ttu-id="6027d-106">这意味着，如果探查器调用 `setenv` 新的环境变量，则在进程中运行的托管代码将不会选取。</span><span class="sxs-lookup"><span data-stu-id="6027d-106">This means that if the profiler calls `setenv` the new environment variable will not be picked up by managed code running in the process.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="6027d-107">语法</span><span class="sxs-lookup"><span data-stu-id="6027d-107">Syntax</span></span>  
  
```cpp  
    HRESULT SetEnvironmentVariable(
                [in, string] const WCHAR *szName,
                [in, string] const WCHAR *szValue);
```  
  
## <a name="parameters"></a><span data-ttu-id="6027d-108">参数</span><span class="sxs-lookup"><span data-stu-id="6027d-108">Parameters</span></span>

<span data-ttu-id="6027d-109">`szName` 中指向以 null 结尾的宽字符字符串的指针，该字符串包含要设置的环境变量的名称。</span><span class="sxs-lookup"><span data-stu-id="6027d-109">`szName` [in] A pointer to a null terminated wide character string containing the name of the environment variable to set.</span></span>

<span data-ttu-id="6027d-110">`szValue` 中指向以 null 结尾的宽字符字符串的指针，该字符串包含要设置的环境变量的值。</span><span class="sxs-lookup"><span data-stu-id="6027d-110">`szValue` [in] A pointer to a null terminated wide character string containing the value of the environment variable to set.</span></span>

## <a name="requirements"></a><span data-ttu-id="6027d-111">要求</span><span class="sxs-lookup"><span data-stu-id="6027d-111">Requirements</span></span>  

<span data-ttu-id="6027d-112">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="6027d-112">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="6027d-113">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="6027d-113">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="6027d-114">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-31-md.md)]</span><span class="sxs-lookup"><span data-stu-id="6027d-114">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-31-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="6027d-115">另请参阅</span><span class="sxs-lookup"><span data-stu-id="6027d-115">See also</span></span>

- [<span data-ttu-id="6027d-116">分析接口</span><span class="sxs-lookup"><span data-stu-id="6027d-116">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="6027d-117">ICorProfilerInfo11 接口</span><span class="sxs-lookup"><span data-stu-id="6027d-117">ICorProfilerInfo11 Interface</span></span>](icorprofilerinfo11-interface.md)
