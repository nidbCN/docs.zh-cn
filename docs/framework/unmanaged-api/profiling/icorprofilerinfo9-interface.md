---
description: 了解详细信息： ICorProfilerInfo9 接口
title: ICorProfilerInfo9 接口
ms.date: 08/06/2019
author: davmason
ms.author: davmason
ms.openlocfilehash: 44e3d694b426f87ee4e4bc12181f46322b0d246f
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805670"
---
# <a name="icorprofilerinfo9-interface"></a><span data-ttu-id="7fafd-103">ICorProfilerInfo9 接口</span><span class="sxs-lookup"><span data-stu-id="7fafd-103">ICorProfilerInfo9 Interface</span></span>

<span data-ttu-id="7fafd-104">[ICorProfilerInfo8](icorprofilerinfo8-interface.md)的子类，提供用于查询有关具有多个本机代码版本的函数的信息的方法。</span><span class="sxs-lookup"><span data-stu-id="7fafd-104">A subclass of [ICorProfilerInfo8](icorprofilerinfo8-interface.md) that provides methods to query information about functions with multiple native code versions.</span></span>  

## <a name="methods"></a><span data-ttu-id="7fafd-105">方法</span><span class="sxs-lookup"><span data-stu-id="7fafd-105">Methods</span></span>  

| <span data-ttu-id="7fafd-106">方法</span><span class="sxs-lookup"><span data-stu-id="7fafd-106">Method</span></span>|<span data-ttu-id="7fafd-107">说明</span><span class="sxs-lookup"><span data-stu-id="7fafd-107">Description</span></span>|  
| ------------|-----------------|  
|[<span data-ttu-id="7fafd-108">GetNativeCodeStartAddresses 方法</span><span class="sxs-lookup"><span data-stu-id="7fafd-108">GetNativeCodeStartAddresses Method</span></span>](icorprofilerinfo9-getnativecodestartaddresses-method.md)| <span data-ttu-id="7fafd-109">给定一个 functionId 和 rejitId，枚举此代码当前存在的所有实时编译版本的本机代码起始地址。</span><span class="sxs-lookup"><span data-stu-id="7fafd-109">Given a functionId and rejitId, enumerates the native code start address of all jitted versions of this code that currently exist.</span></span> |
|[<span data-ttu-id="7fafd-110">GetILToNativeMapping3 方法</span><span class="sxs-lookup"><span data-stu-id="7fafd-110">GetILToNativeMapping3 Method</span></span>](icorprofilerinfo9-getiltonativemapping3-method.md)| <span data-ttu-id="7fafd-111">给定本机代码起始地址，返回此实时编译版本的代码的本机到 IL 映射信息。</span><span class="sxs-lookup"><span data-stu-id="7fafd-111">Given the native code start address, returns the native to IL mapping information for this jitted version of the code.</span></span> |
|[<span data-ttu-id="7fafd-112">GetCodeInfo4 方法</span><span class="sxs-lookup"><span data-stu-id="7fafd-112">GetCodeInfo4 Method</span></span>](icorprofilerinfo9-getcodeinfo4-method.md)| <span data-ttu-id="7fafd-113">给定本机代码起始地址，返回存储此代码的虚拟内存块。</span><span class="sxs-lookup"><span data-stu-id="7fafd-113">Given the native code start address, returns the blocks of virtual memory that store this code.</span></span> |

## <a name="requirements"></a><span data-ttu-id="7fafd-114">要求</span><span class="sxs-lookup"><span data-stu-id="7fafd-114">Requirements</span></span>  

<span data-ttu-id="7fafd-115">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="7fafd-115">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>  
<span data-ttu-id="7fafd-116">**头文件：** CorProf.idl、CorProf.h</span><span class="sxs-lookup"><span data-stu-id="7fafd-116">**Header:** CorProf.idl, CorProf.h</span></span>  
<span data-ttu-id="7fafd-117">**.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-21-md.md)]</span><span class="sxs-lookup"><span data-stu-id="7fafd-117">**.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-21-md.md)]</span></span>  

## <a name="see-also"></a><span data-ttu-id="7fafd-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="7fafd-118">See also</span></span>

- [<span data-ttu-id="7fafd-119">分析接口</span><span class="sxs-lookup"><span data-stu-id="7fafd-119">Profiling Interfaces</span></span>](profiling-interfaces.md)
