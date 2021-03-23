---
description: 了解详细信息： ICorProfilerInfo11：： GetEnvironmentVariable 方法
title: ICorProfilerInfo11：： GetEnvironmentVariable 方法
ms.date: 03/19/2021
api_name:
- ICorProfilerInfo11.GetEnvironmentVariable
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 635c5fb67b880c39f15fbc194a51a706d9ef7fe4
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805826"
---
# <a name="icorprofilerinfo11getenvironmentvariable-method"></a><span data-ttu-id="8f151-103">ICorProfilerInfo11：： GetEnvironmentVariable 方法</span><span class="sxs-lookup"><span data-stu-id="8f151-103">ICorProfilerInfo11::GetEnvironmentVariable Method</span></span>

<span data-ttu-id="8f151-104">获取进程中的环境变量。</span><span class="sxs-lookup"><span data-stu-id="8f151-104">Gets an environment variable from the process.</span></span> <span data-ttu-id="8f151-105">在非 Windows 平台上，运行时将保留环境变量的内部缓存，以确保线程安全。</span><span class="sxs-lookup"><span data-stu-id="8f151-105">On non-Windows platforms the runtime keeps an internal cache of environment variables to ensure thread safety.</span></span> <span data-ttu-id="8f151-106">这意味着调用 `getenv` 将不会读取启动后进程中运行的托管代码所设置的任何新的或更新的环境变量。</span><span class="sxs-lookup"><span data-stu-id="8f151-106">This means that calling `getenv` will not read any new or updated environment variables set by managed code running in the process after startup.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="8f151-107">语法</span><span class="sxs-lookup"><span data-stu-id="8f151-107">Syntax</span></span>  
  
```cpp  
    HRESULT GetEnvironmentVariable(
                [in, string] const WCHAR *szName,
                [in]         ULONG cchValue,
                [out]        ULONG *pcchValue,
                [out, annotation("_Out_writes_to_(cchValue, *pcchValue)")]
                             WCHAR szValue[]);
```  
  
## <a name="parameters"></a><span data-ttu-id="8f151-108">参数</span><span class="sxs-lookup"><span data-stu-id="8f151-108">Parameters</span></span>

<span data-ttu-id="8f151-109">`szName` 中指向以 null 结尾的宽字符字符串的指针，该字符串包含要获取的环境变量的名称。</span><span class="sxs-lookup"><span data-stu-id="8f151-109">`szName` [in] A pointer to a null terminated wide character string containing the name of the environment variable to get.</span></span>

<span data-ttu-id="8f151-110">`cchValue` 中的长度，以字符为字符 `szValue` 。</span><span class="sxs-lookup"><span data-stu-id="8f151-110">`cchValue` [in] The length, in characters, of `szValue`.</span></span>

<span data-ttu-id="8f151-111">`pcchValue` 弄指向的总字符长度的指针 `szValue` 。</span><span class="sxs-lookup"><span data-stu-id="8f151-111">`pcchValue` [out] A pointer to the total character length of `szValue`.</span></span>

<span data-ttu-id="8f151-112">`szValue` 弄调用方提供宽字符缓冲区。</span><span class="sxs-lookup"><span data-stu-id="8f151-112">`szValue` [out] A caller provided wide character buffer.</span></span> <span data-ttu-id="8f151-113">当函数返回时，缓冲区将包含环境变量的值。</span><span class="sxs-lookup"><span data-stu-id="8f151-113">When the function returns the buffer will contain the value of the environment variable.</span></span>

## <a name="requirements"></a><span data-ttu-id="8f151-114">要求</span><span class="sxs-lookup"><span data-stu-id="8f151-114">Requirements</span></span>  

<span data-ttu-id="8f151-115">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="8f151-115">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="8f151-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="8f151-116">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="8f151-117">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-31-md.md)]</span><span class="sxs-lookup"><span data-stu-id="8f151-117">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-31-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="8f151-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="8f151-118">See also</span></span>

- [<span data-ttu-id="8f151-119">分析接口</span><span class="sxs-lookup"><span data-stu-id="8f151-119">Profiling Interfaces</span></span>](profiling-interfaces.md)
- [<span data-ttu-id="8f151-120">ICorProfilerInfo11 接口</span><span class="sxs-lookup"><span data-stu-id="8f151-120">ICorProfilerInfo11 Interface</span></span>](icorprofilerinfo11-interface.md)
