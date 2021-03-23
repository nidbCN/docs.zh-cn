---
title: CoreResponseData 类
description: 了解 CoreResponseData 类，该类表示 HTTP 标头和响应正文的分析。 它位于 .NET 中的 System.Net 命名空间中。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 0c787481a57abc755e6f61b1cc96d74b86458b62
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873921"
---
# <a name="coreresponsedata-class"></a><span data-ttu-id="8bc66-104">CoreResponseData 类</span><span class="sxs-lookup"><span data-stu-id="8bc66-104">CoreResponseData Class</span></span>

<span data-ttu-id="8bc66-105">`CoreResponseData`类表示 HTTP 标头和响应正文的分析。</span><span class="sxs-lookup"><span data-stu-id="8bc66-105">The `CoreResponseData` class represents the parsing of the HTTP headers and the response body.</span></span>

## <a name="syntax"></a><span data-ttu-id="8bc66-106">语法</span><span class="sxs-lookup"><span data-stu-id="8bc66-106">Syntax</span></span>
  
```csharp
internal class CoreResponseData
```

> [!WARNING]
> <span data-ttu-id="8bc66-107">此 API 是内部的，不应在代码中直接使用。</span><span class="sxs-lookup"><span data-stu-id="8bc66-107">This API is internal, and it is not meant to be used directly in your code.</span></span> <span data-ttu-id="8bc66-108">相反，应使用 <xref:System.Diagnostics.DiagnosticSource> 挂钩网络代码。</span><span class="sxs-lookup"><span data-stu-id="8bc66-108">Instead, you should use a <xref:System.Diagnostics.DiagnosticSource> to hook networking code.</span></span> <span data-ttu-id="8bc66-109">请参阅 [DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)。</span><span class="sxs-lookup"><span data-stu-id="8bc66-109">See [DiagnosticSource User's Guide](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).</span></span>
>
> <span data-ttu-id="8bc66-110">在任何情况下，Microsoft 不支持在生产应用程序中使用此类。</span><span class="sxs-lookup"><span data-stu-id="8bc66-110">Microsoft does not support the use of this class in a production application under any circumstance.</span></span>

## <a name="requirements"></a><span data-ttu-id="8bc66-111">要求</span><span class="sxs-lookup"><span data-stu-id="8bc66-111">Requirements</span></span>

<span data-ttu-id="8bc66-112">**命名空间：** <xref:System.Net></span><span class="sxs-lookup"><span data-stu-id="8bc66-112">**Namespace:** <xref:System.Net></span></span>

<span data-ttu-id="8bc66-113">**程序集：** System.dll 中的系统 () </span><span class="sxs-lookup"><span data-stu-id="8bc66-113">**Assembly:** System (in System.dll)</span></span>

<span data-ttu-id="8bc66-114">**.NET Framework 版本：** 自2.0 起可用。</span><span class="sxs-lookup"><span data-stu-id="8bc66-114">**.NET Framework versions:** Available since 2.0.</span></span>
