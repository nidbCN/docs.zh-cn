---
description: 了解详细信息： COR_PRF_EVENTPIPE_PARAM_DESC 枚举
title: COR_PRF_EVENTPIPE_PARAM_DESC 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_PARAM_DESC
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: b23897580092cc9f3f887a21e86f7111fecd9f19
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805787"
---
# <a name="cor_prf_eventpipe_param_desc-enumeration"></a><span data-ttu-id="72c40-103">COR_PRF_EVENTPIPE_PARAM_DESC 枚举</span><span class="sxs-lookup"><span data-stu-id="72c40-103">COR_PRF_EVENTPIPE_PARAM_DESC Enumeration</span></span>

<span data-ttu-id="72c40-104">描述 EventPipe 事件的参数名称和类型。</span><span class="sxs-lookup"><span data-stu-id="72c40-104">Describes the parameter name and type for an EventPipe event.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="72c40-105">语法</span><span class="sxs-lookup"><span data-stu-id="72c40-105">Syntax</span></span>  
  
```cpp  
typedef struct
{
    UINT32       type;
    UINT32       elementType;
    const WCHAR *name;
} COR_PRF_EVENTPIPE_PARAM_DESC;
```  
  
## <a name="members"></a><span data-ttu-id="72c40-106">成员</span><span class="sxs-lookup"><span data-stu-id="72c40-106">Members</span></span>  
  
|<span data-ttu-id="72c40-107">成员</span><span class="sxs-lookup"><span data-stu-id="72c40-107">Member</span></span>|<span data-ttu-id="72c40-108">说明</span><span class="sxs-lookup"><span data-stu-id="72c40-108">Description</span></span>|  
|------------|-----------------|  
|`type`|<span data-ttu-id="72c40-109">参数的类型。</span><span class="sxs-lookup"><span data-stu-id="72c40-109">The type of the parameter.</span></span>|  
|`elementType`|<span data-ttu-id="72c40-110">如果此参数是数组类型，则为元素类型。</span><span class="sxs-lookup"><span data-stu-id="72c40-110">The element type if this parameter is an array type.</span></span>|  
|`name`|<span data-ttu-id="72c40-111">包含参数名称的宽字符字符串。</span><span class="sxs-lookup"><span data-stu-id="72c40-111">A wide character string containing the name of the parameter.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="72c40-112">备注</span><span class="sxs-lookup"><span data-stu-id="72c40-112">Remarks</span></span>  

 <span data-ttu-id="72c40-113">`COR_PRF_EVENTPIPE_PARAM_DESC` [ICorProfilerInfo12：： EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md)方法使用结构来定义所定义事件的参数类型。</span><span class="sxs-lookup"><span data-stu-id="72c40-113">The `COR_PRF_EVENTPIPE_PARAM_DESC` structure is used by the [ICorProfilerInfo12::EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md) method to define the parameter types of the event being defined.</span></span>
  
## <a name="requirements"></a><span data-ttu-id="72c40-114">要求</span><span class="sxs-lookup"><span data-stu-id="72c40-114">Requirements</span></span>  

<span data-ttu-id="72c40-115">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="72c40-115">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="72c40-116">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="72c40-116">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="72c40-117">另请参阅</span><span class="sxs-lookup"><span data-stu-id="72c40-117">See also</span></span>

- [<span data-ttu-id="72c40-118">分析枚举</span><span class="sxs-lookup"><span data-stu-id="72c40-118">Profiling Enumerations</span></span>](profiling-enumerations.md)
- [<span data-ttu-id="72c40-119">ICorProfilerInfo12::EventPipeDefineEvent</span><span class="sxs-lookup"><span data-stu-id="72c40-119">ICorProfilerInfo12::EventPipeDefineEvent</span></span>](icorprofilerinfo12-eventpipedefineevent-method.md)
