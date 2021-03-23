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
# <a name="coreresponsedatam_statuscode-field"></a>CoreResponseData \_ StatusCode 字段

`CoreResponseData.m_StatusCode`<xref:System.Net.HttpStatusCode>包含响应状态的。

## <a name="syntax"></a>语法
  
```csharp
public HttpStatusCode m_StatusCode
```

> [!WARNING]
> 此 API 不应在代码中直接使用。 相反，应使用 <xref:System.Diagnostics.DiagnosticSource> 挂钩网络代码。 请参阅 [DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** System.dll 中的系统 () 

**.NET Framework 版本：** 自2.0 起可用。
