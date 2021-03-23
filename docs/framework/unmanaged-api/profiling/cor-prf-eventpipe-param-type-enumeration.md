---
description: 了解详细信息： COR_PRF_EVENTPIPE_PARAM_TYPE 枚举
title: COR_PRF_EVENTPIPE_PARAM_TYPE 枚举
ms.date: 03/19/2021
api_name:
- COR_PRF_EVENTPIPE_PARAM_TYPE
api_location:
- coreclr.dll
- corprof.idl
api_type:
- COM
ms.openlocfilehash: 29aed86e4a991598d038a60ae086e77e25df24bb
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805774"
---
# <a name="cor_prf_eventpipe_param_type-enumeration"></a><span data-ttu-id="e4955-103">COR_PRF_EVENTPIPE_PARAM_TYPE 枚举</span><span class="sxs-lookup"><span data-stu-id="e4955-103">COR_PRF_EVENTPIPE_PARAM_TYPE Enumeration</span></span>

<span data-ttu-id="e4955-104">描述 EventPipe 事件的参数类型。</span><span class="sxs-lookup"><span data-stu-id="e4955-104">Describes the type of a parameter for an EventPipe event.</span></span>
  
## <a name="syntax"></a><span data-ttu-id="e4955-105">语法</span><span class="sxs-lookup"><span data-stu-id="e4955-105">Syntax</span></span>  
  
```cpp  
typedef enum
{
    COR_PRF_EVENTPIPE_OBJECT = 1,
    COR_PRF_EVENTPIPE_BOOLEAN = 3,
    COR_PRF_EVENTPIPE_CHAR = 4,
    COR_PRF_EVENTPIPE_SBYTE = 5,
    COR_PRF_EVENTPIPE_BYTE = 6,
    COR_PRF_EVENTPIPE_INT16 = 7,
    COR_PRF_EVENTPIPE_UINT16 = 8,
    COR_PRF_EVENTPIPE_INT32 = 9,
    COR_PRF_EVENTPIPE_UINT32 = 10,
    COR_PRF_EVENTPIPE_INT64 = 11,
    COR_PRF_EVENTPIPE_UINT64 = 12,
    COR_PRF_EVENTPIPE_SINGLE = 13,
    COR_PRF_EVENTPIPE_DOUBLE = 14,
    COR_PRF_EVENTPIPE_DECIMAL = 15,
    COR_PRF_EVENTPIPE_DATETIME = 16,
    COR_PRF_EVENTPIPE_GUID = 17,
    COR_PRF_EVENTPIPE_STRING = 18,
    COR_PRF_EVENTPIPE_ARRAY = 19,
} COR_PRF_EVENTPIPE_PARAM_TYPE;
```  
  
## <a name="members"></a><span data-ttu-id="e4955-106">成员</span><span class="sxs-lookup"><span data-stu-id="e4955-106">Members</span></span>  
  
|<span data-ttu-id="e4955-107">成员</span><span class="sxs-lookup"><span data-stu-id="e4955-107">Member</span></span>|<span data-ttu-id="e4955-108">说明</span><span class="sxs-lookup"><span data-stu-id="e4955-108">Description</span></span>|  
|------------|-----------------|  
|`COR_PRF_EVENTPIPE_OBJECT`|<span data-ttu-id="e4955-109">参数类型是一个自描述对象。</span><span class="sxs-lookup"><span data-stu-id="e4955-109">The parameter type is a self describing object.</span></span>|
|`COR_PRF_EVENTPIPE_BOOLEAN`|<span data-ttu-id="e4955-110">参数类型为布尔值。</span><span class="sxs-lookup"><span data-stu-id="e4955-110">The parameter type is a boolean.</span></span>|
|`COR_PRF_EVENTPIPE_CHAR`|<span data-ttu-id="e4955-111">参数类型为16位宽字符。</span><span class="sxs-lookup"><span data-stu-id="e4955-111">The parameter type is a 16 bit wide character.</span></span>|
|`COR_PRF_EVENTPIPE_SBYTE`|<span data-ttu-id="e4955-112">参数类型是有符号的8位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-112">The parameter type is a signed 8 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_BYTE`|<span data-ttu-id="e4955-113">参数类型是一个无符号的8位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-113">The parameter type is an unsigned 8 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_INT16`|<span data-ttu-id="e4955-114">参数类型为带符号的16位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-114">The parameter type is a signed 16 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_UINT16`|<span data-ttu-id="e4955-115">参数类型为16位无符号整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-115">The parameter type is an unsigned 16 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_INT32`|<span data-ttu-id="e4955-116">参数类型是有符号的32位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-116">The parameter type is a signed 32 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_UINT32`|<span data-ttu-id="e4955-117">参数类型是一个无符号的32位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-117">The parameter type is an unsigned 32 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_INT64`|<span data-ttu-id="e4955-118">参数类型是有符号的64位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-118">The parameter type is a signed 64 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_UINT64`|<span data-ttu-id="e4955-119">参数类型是一个无符号的64位整数。</span><span class="sxs-lookup"><span data-stu-id="e4955-119">The parameter type is an unsigned 64 bit integer.</span></span>|
|`COR_PRF_EVENTPIPE_SINGLE`|<span data-ttu-id="e4955-120">参数类型是一个32位浮点数字。</span><span class="sxs-lookup"><span data-stu-id="e4955-120">The parameter type is a 32 bit floating point number.</span></span>|
|`COR_PRF_EVENTPIPE_DOUBLE`|<span data-ttu-id="e4955-121">参数类型是一个64位浮点数字。</span><span class="sxs-lookup"><span data-stu-id="e4955-121">The parameter type is a 64 bit floating point number.</span></span>|
|`COR_PRF_EVENTPIPE_DECIMAL`|<span data-ttu-id="e4955-122">参数类型是一个128位浮点数字。</span><span class="sxs-lookup"><span data-stu-id="e4955-122">The parameter type is a 128 bit floating point number.</span></span>|
|`COR_PRF_EVENTPIPE_DATETIME`|<span data-ttu-id="e4955-123">参数类型是序列化的 DataTime 结构。</span><span class="sxs-lookup"><span data-stu-id="e4955-123">The parameter type is a serialized DataTime structure.</span></span>|
|`COR_PRF_EVENTPIPE_GUID`|<span data-ttu-id="e4955-124">参数类型为 GUID。</span><span class="sxs-lookup"><span data-stu-id="e4955-124">The parameter type is a GUID.</span></span>|
|`COR_PRF_EVENTPIPE_STRING`|<span data-ttu-id="e4955-125">参数类型是一个16位 null 的宽字符字符串。</span><span class="sxs-lookup"><span data-stu-id="e4955-125">The parameter type is a 16 bit null terminated wide character string.</span></span>|
|`COR_PRF_EVENTPIPE_ARRAY`|<span data-ttu-id="e4955-126">参数类型是上述类型之一的数组。</span><span class="sxs-lookup"><span data-stu-id="e4955-126">The parameter type is an array of one of the preceding types.</span></span>|
  
## <a name="remarks"></a><span data-ttu-id="e4955-127">备注</span><span class="sxs-lookup"><span data-stu-id="e4955-127">Remarks</span></span>  

 <span data-ttu-id="e4955-128">`COR_PRF_EVENTPIPE_PARAM_TYPE` [COR_PRF_EVENTPIPE_PARAM_DESC](cor-prf-eventpipe-param-desc-structure.md)结构使用枚举来指示参数的类型。</span><span class="sxs-lookup"><span data-stu-id="e4955-128">The `COR_PRF_EVENTPIPE_PARAM_TYPE` enumeration is used by the [COR_PRF_EVENTPIPE_PARAM_DESC](cor-prf-eventpipe-param-desc-structure.md) structure to indicate the type of the parameter.</span></span>
  
## <a name="requirements"></a><span data-ttu-id="e4955-129">要求</span><span class="sxs-lookup"><span data-stu-id="e4955-129">Requirements</span></span>  

<span data-ttu-id="e4955-130">**平台：** 请参阅 [支持 .Net Core 的操作系统](../../../core/install/windows.md?pivots=os-windows)。</span><span class="sxs-lookup"><span data-stu-id="e4955-130">**Platforms:** See [.NET Core supported operating systems](../../../core/install/windows.md?pivots=os-windows).</span></span>
<span data-ttu-id="e4955-131">**标头：** Corprof.idl，Corprof.idl **.Net 版本：**[!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span><span class="sxs-lookup"><span data-stu-id="e4955-131">**Header:** CorProf.idl, CorProf.h **.NET Versions:** [!INCLUDE[net_core](../../../../includes/net-core-50-md.md)]</span></span>
  
## <a name="see-also"></a><span data-ttu-id="e4955-132">另请参阅</span><span class="sxs-lookup"><span data-stu-id="e4955-132">See also</span></span>

- [<span data-ttu-id="e4955-133">分析枚举</span><span class="sxs-lookup"><span data-stu-id="e4955-133">Profiling Enumerations</span></span>](profiling-enumerations.md)
