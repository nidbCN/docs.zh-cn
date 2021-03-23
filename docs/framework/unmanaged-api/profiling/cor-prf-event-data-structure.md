---
description: 了解详细信息： COR_PRF_EVENT_DATA 枚举
title: COR_PRF_EVENT_DATA 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENT_DATA
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 477f36476deb9c0d74e263703e36b134c91b5327
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805555"
---
# <a name="cor_prf_event_data-enumeration"></a><span data-ttu-id="638c7-103">COR_PRF_EVENT_DATA 枚举</span><span class="sxs-lookup"><span data-stu-id="638c7-103">COR_PRF_EVENT_DATA Enumeration</span></span>

<span data-ttu-id="638c7-104">描述要写入的 EventPipe 事件的事件数据。</span><span class="sxs-lookup"><span data-stu-id="638c7-104">Describes the event data for an EventPipe event being written.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="638c7-105">语法</span><span class="sxs-lookup"><span data-stu-id="638c7-105">Syntax</span></span>  
  
```cpp  
typedef struct
{
    UINT64 ptr;
    UINT32 size;
    UINT32 reserved;
} COR_PRF_EVENT_DATA;
```  
  
## <a name="members"></a><span data-ttu-id="638c7-106">成员</span><span class="sxs-lookup"><span data-stu-id="638c7-106">Members</span></span>  
  
|<span data-ttu-id="638c7-107">成员</span><span class="sxs-lookup"><span data-stu-id="638c7-107">Member</span></span>|<span data-ttu-id="638c7-108">说明</span><span class="sxs-lookup"><span data-stu-id="638c7-108">Description</span></span>|  
|------------|-----------------|  
|`ptr`|<span data-ttu-id="638c7-109">指向数据的指针。</span><span class="sxs-lookup"><span data-stu-id="638c7-109">A pointer to the data.</span></span>|  
|`size`|<span data-ttu-id="638c7-110">指向的数据的大小 `ptr` 。</span><span class="sxs-lookup"><span data-stu-id="638c7-110">The size of the data pointed to by `ptr`.</span></span>|  
|`reserved`|<span data-ttu-id="638c7-111">保留实现特定字段。</span><span class="sxs-lookup"><span data-stu-id="638c7-111">An reserved implementation specific field.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="638c7-112">备注</span><span class="sxs-lookup"><span data-stu-id="638c7-112">Remarks</span></span>  

 <span data-ttu-id="638c7-113">`COR_PRF_EVENT_DATA` [ICorProfilerInfo12：： EventPipeWriteEvent](icorprofilerinfo12-eventpipewriteevent-method.md)方法使用结构来 providate 要写入的事件的数据负载。</span><span class="sxs-lookup"><span data-stu-id="638c7-113">The `COR_PRF_EVENT_DATA` structure is used by the [ICorProfilerInfo12::EventPipeWriteEvent](icorprofilerinfo12-eventpipewriteevent-method.md) method to providate the data payload for the event being written.</span></span>
  
## <a name="requirements"></a><span data-ttu-id="638c7-114">要求</span><span class="sxs-lookup"><span data-stu-id="638c7-114">Requirements</span></span>  

<span data-ttu-id="638c7-115">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="638c7-115">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="638c7-116">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="638c7-116">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="638c7-117">另请参阅</span><span class="sxs-lookup"><span data-stu-id="638c7-117">See also</span></span>

- [<span data-ttu-id="638c7-118">分析枚举</span><span class="sxs-lookup"><span data-stu-id="638c7-118">Profiling Enumerations</span></span>](profiling-enumerations.md)
- [<span data-ttu-id="638c7-119">ICorProfilerInfo12::EventPipeWriteEvent</span><span class="sxs-lookup"><span data-stu-id="638c7-119">ICorProfilerInfo12::EventPipeWriteEvent</span></span>](icorprofilerinfo12-eventpipewriteevent-method.md)
