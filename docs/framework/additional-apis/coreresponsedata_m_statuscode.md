---
title: CoreResponseData.m_StatusCode 字段
description: 阅读有关 .NET 中的 CoreResponseData.m_StatusCode 字段的信息。 此字段是包含 HTTP 响应状态的 HttpStatusCode 类型。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_StatusCode
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 29e46f71fd6e819dd311b214733fe56ff0548d74
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875897"
---
# <a name="coreresponsedatam_statuscode-field"></a><span data-ttu-id="94b6f-104">CoreResponseData \_ StatusCode 字段</span><span class="sxs-lookup"><span data-stu-id="94b6f-104">CoreResponseData.m\_StatusCode Field</span></span>

<span data-ttu-id="94b6f-105">`CoreResponseData.m_StatusCode`<xref:System.Net.HttpStatusCode>包含响应状态的。</span><span class="sxs-lookup"><span data-stu-id="94b6f-105">`CoreResponseData.m_StatusCode` is an <xref:System.Net.HttpStatusCode> containing the status of the response.</span></span>

## <a name="syntax"></a><span data-ttu-id="94b6f-106">语法</span><span class="sxs-lookup"><span data-stu-id="94b6f-106">Syntax</span></span>
  
```csharp
public HttpStatusCode m_StatusCode
```

> [!WARNING]
> <span data-ttu-id="94b6f-107">此 API 不应在代码中直接使用。</span><span class="sxs-lookup"><span data-stu-id="94b6f-107">This API is not meant to be used directly in your code.</span></span> <span data-ttu-id="94b6f-108">相反，应使用 <xref:System.Diagnostics.DiagnosticSource> 挂钩网络代码。</span><span class="sxs-lookup"><span data-stu-id="94b6f-108">Instead, you should use a <xref:System.Diagnostics.DiagnosticSource> to hook networking code.</span></span> <span data-ttu-id="94b6f-109">请参阅 [DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)。</span><span class="sxs-lookup"><span data-stu-id="94b6f-109">See [DiagnosticSource User's Guide](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).</span></span>
>
> <span data-ttu-id="94b6f-110">在任何情况下，Microsoft 不支持在生产应用程序中使用此类。</span><span class="sxs-lookup"><span data-stu-id="94b6f-110">Microsoft does not support the use of this class in a production application under any circumstance.</span></span>

## <a name="requirements"></a><span data-ttu-id="94b6f-111">要求</span><span class="sxs-lookup"><span data-stu-id="94b6f-111">Requirements</span></span>

<span data-ttu-id="94b6f-112">**命名空间：** <xref:System.Net></span><span class="sxs-lookup"><span data-stu-id="94b6f-112">**Namespace:** <xref:System.Net></span></span>

<span data-ttu-id="94b6f-113">**程序集：** System.dll 中的系统 () </span><span class="sxs-lookup"><span data-stu-id="94b6f-113">**Assembly:** System (in System.dll)</span></span>

<span data-ttu-id="94b6f-114">**.NET Framework 版本：** 自2.0 起可用。</span><span class="sxs-lookup"><span data-stu-id="94b6f-114">**.NET Framework versions:** Available since 2.0.</span></span>
