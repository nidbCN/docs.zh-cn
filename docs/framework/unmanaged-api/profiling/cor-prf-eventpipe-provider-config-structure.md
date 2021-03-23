---
description: 了解详细信息： COR_PRF_EVENTPIPE_PROVIDER_CONFIG 枚举
title: COR_PRF_EVENTPIPE_PROVIDER_CONFIG 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_PROVIDER_CONFIG
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 2a7a5e720e9468b44e6504d24a3b9ad836e13693
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805839"
---
# <a name="cor_prf_eventpipe_provider_config-enumeration"></a><span data-ttu-id="3dcc6-103">COR_PRF_EVENTPIPE_PROVIDER_CONFIG 枚举</span><span class="sxs-lookup"><span data-stu-id="3dcc6-103">COR_PRF_EVENTPIPE_PROVIDER_CONFIG Enumeration</span></span>

<span data-ttu-id="3dcc6-104">描述配置 EventPipe 提供程序所必需的字段。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-104">Describes the fields necessary to configure an EventPipe provider.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="3dcc6-105">语法</span><span class="sxs-lookup"><span data-stu-id="3dcc6-105">Syntax</span></span>  
  
```cpp  
typedef struct
{
    const WCHAR* providerName;
    UINT64       keywords;
    UINT32       loggingLevel;
    const WCHAR* filterData;
} COR_PRF_EVENTPIPE_PROVIDER_CONFIG;
```  
  
## <a name="members"></a><span data-ttu-id="3dcc6-106">成员</span><span class="sxs-lookup"><span data-stu-id="3dcc6-106">Members</span></span>  
  
|<span data-ttu-id="3dcc6-107">成员</span><span class="sxs-lookup"><span data-stu-id="3dcc6-107">Member</span></span>|<span data-ttu-id="3dcc6-108">说明</span><span class="sxs-lookup"><span data-stu-id="3dcc6-108">Description</span></span>|  
|------------|-----------------|  
|`providerName`|<span data-ttu-id="3dcc6-109">要启用的提供程序的名称。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-109">The name of the provider to enable.</span></span>|  
|`keywords`|<span data-ttu-id="3dcc6-110">要在提供程序上启用的关键字。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-110">The keywords to enable on the provider.</span></span>|  
|`loggingLevel`|<span data-ttu-id="3dcc6-111">要对提供程序启用的级别。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-111">The level to enable on the provider.</span></span>|  
|`filterData`|<span data-ttu-id="3dcc6-112">包含在启用提供程序时要使用的 filterdata 的宽字符字符串。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-112">A wide character string containing the filterdata to use when enabling the provider.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="3dcc6-113">备注</span><span class="sxs-lookup"><span data-stu-id="3dcc6-113">Remarks</span></span>  

 <span data-ttu-id="3dcc6-114">`COR_PRF_EVENTPIPE_PROVIDER_CONFIG` [ICorProfilerInfo12：： EventPipeAddProviderToSession](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)方法使用结构来指示要添加到会话中的提供程序的配置。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-114">The `COR_PRF_EVENTPIPE_PROVIDER_CONFIG` structure is used by the [ICorProfilerInfo12::EventPipeAddProviderToSession](icorprofilerinfo12-eventpipeaddprovidertosession-method.md) method to indicate the configuration for the provider being added to the session.</span></span>
  
## <a name="requirements"></a><span data-ttu-id="3dcc6-115">要求</span><span class="sxs-lookup"><span data-stu-id="3dcc6-115">Requirements</span></span>  

<span data-ttu-id="3dcc6-116">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="3dcc6-116">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="3dcc6-117">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="3dcc6-117">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="3dcc6-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="3dcc6-118">See also</span></span>

- [<span data-ttu-id="3dcc6-119">分析枚举</span><span class="sxs-lookup"><span data-stu-id="3dcc6-119">Profiling Enumerations</span></span>](profiling-enumerations.md)
- [<span data-ttu-id="3dcc6-120">ICorProfilerInfo12::EventPipeAddProviderToSession</span><span class="sxs-lookup"><span data-stu-id="3dcc6-120">ICorProfilerInfo12::EventPipeAddProviderToSession</span></span>](icorprofilerinfo12-eventpipeaddprovidertosession-method.md)
