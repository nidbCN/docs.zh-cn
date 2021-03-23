---
description: 了解详细信息： COR_PRF_EVENTPIPE_LEVEL 枚举
title: COR_PRF_EVENTPIPE_LEVEL 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_LEVEL
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: da8211da1a9bb6262b078c5e56bee1d0c1f9342e
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805813"
---
# <a name="cor_prf_eventpipe_level-enumeration"></a><span data-ttu-id="41efc-103">COR_PRF_EVENTPIPE_LEVEL 枚举</span><span class="sxs-lookup"><span data-stu-id="41efc-103">COR_PRF_EVENTPIPE_LEVEL Enumeration</span></span>

<span data-ttu-id="41efc-104">描述 EventPipe 事件的级别。</span><span class="sxs-lookup"><span data-stu-id="41efc-104">Describes the level of an EventPipe event.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="41efc-105">语法</span><span class="sxs-lookup"><span data-stu-id="41efc-105">Syntax</span></span>  
  
```cpp  
typedef enum
{
    COR_PRF_EVENTPIPE_LOGALWAYS = 0,
    COR_PRF_EVENTPIPE_CRITICAL = 1,
    COR_PRF_EVENTPIPE_ERROR = 2,
    COR_PRF_EVENTPIPE_WARNING = 3,
    COR_PRF_EVENTPIPE_INFORMATIONAL = 4,
    COR_PRF_EVENTPIPE_VERBOSE = 5
} COR_PRF_EVENTPIPE_LEVEL;
```  
  
## <a name="members"></a><span data-ttu-id="41efc-106">成员</span><span class="sxs-lookup"><span data-stu-id="41efc-106">Members</span></span>  
  
|<span data-ttu-id="41efc-107">成员</span><span class="sxs-lookup"><span data-stu-id="41efc-107">Member</span></span>|<span data-ttu-id="41efc-108">说明</span><span class="sxs-lookup"><span data-stu-id="41efc-108">Description</span></span>|  
|------------|-----------------|  
|`COR_PRF_EVENTPIPE_LOGALWAYS`|<span data-ttu-id="41efc-109">始终记录事件。</span><span class="sxs-lookup"><span data-stu-id="41efc-109">The event is always logged.</span></span>|
|`COR_PRF_EVENTPIPE_CRITICAL`|<span data-ttu-id="41efc-110">事件表示关键消息。</span><span class="sxs-lookup"><span data-stu-id="41efc-110">The event represents a critical message.</span></span>|
|`COR_PRF_EVENTPIPE_ERROR`|<span data-ttu-id="41efc-111">该事件表示一条错误消息。</span><span class="sxs-lookup"><span data-stu-id="41efc-111">The event represents an error message.</span></span>|
|`COR_PRF_EVENTPIPE_WARNING`|<span data-ttu-id="41efc-112">该事件表示一条警告消息。</span><span class="sxs-lookup"><span data-stu-id="41efc-112">The event represents a warning message.</span></span>|
|`COR_PRF_EVENTPIPE_INFORMATIONAL`|<span data-ttu-id="41efc-113">该事件表示信息性消息。</span><span class="sxs-lookup"><span data-stu-id="41efc-113">The event represents an informational message.</span></span>|
|`COR_PRF_EVENTPIPE_VERBOSE`|<span data-ttu-id="41efc-114">该事件表示详细消息。</span><span class="sxs-lookup"><span data-stu-id="41efc-114">The event represents a verbose message.</span></span>|
  
## <a name="remarks"></a><span data-ttu-id="41efc-115">备注</span><span class="sxs-lookup"><span data-stu-id="41efc-115">Remarks</span></span>  

 <span data-ttu-id="41efc-116">`COR_PRF_EVENTPIPE_LEVEL` [ICorProfilerInfo12：： EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md)方法使用枚举来指示要定义的事件级别。</span><span class="sxs-lookup"><span data-stu-id="41efc-116">The `COR_PRF_EVENTPIPE_LEVEL` enumeration is used by the [ICorProfilerInfo12::EventPipeDefineEvent](icorprofilerinfo12-eventpipedefineevent-method.md) method to indicate the level of the event being defined.</span></span>
  
## <a name="requirements"></a><span data-ttu-id="41efc-117">要求</span><span class="sxs-lookup"><span data-stu-id="41efc-117">Requirements</span></span>  

<span data-ttu-id="41efc-118">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="41efc-118">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="41efc-119">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="41efc-119">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="41efc-120">另请参阅</span><span class="sxs-lookup"><span data-stu-id="41efc-120">See also</span></span>

- [<span data-ttu-id="41efc-121">分析枚举</span><span class="sxs-lookup"><span data-stu-id="41efc-121">Profiling Enumerations</span></span>](profiling-enumerations.md)
- [<span data-ttu-id="41efc-122">ICorProfilerInfo12::EventPipeDefineEvent</span><span class="sxs-lookup"><span data-stu-id="41efc-122">ICorProfilerInfo12::EventPipeDefineEvent</span></span>](icorprofilerinfo12-eventpipedefineevent-method.md)
