---
title: HttpWebRequest._CoreResponse 字段
description: 阅读有关 .NET 中的 HttpWebRequest._CoreResponse 字段的信息。 此字段是包含 HTTP 响应分析结果的 CoreResponseData 或 Exception 对象。
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.HttpWebRequest._CoreResponse
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: f5fb71c21922285c0e18c2d1f28eeaf2353dcaee
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873830"
---
# <a name="httpwebrequest_coreresponse-field"></a>HttpWebRequest。 \_CoreResponse 字段

`HttpWebRequest._CoreResponse` 是 [CoreResponseData](coreresponsedata.md) 或 <xref:System.Exception> 包含 HTTP 响应分析结果的) 的 (对象。

## <a name="syntax"></a>语法
  
```csharp
private object _CoreResponse
```

> [!WARNING]
> 此 API 不应在代码中直接使用。 相反，应使用 <xref:System.Diagnostics.DiagnosticSource> 挂钩网络代码。 请参阅 [DiagnosticSource 用户指南](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md)。
>
> 在任何情况下，Microsoft 不支持在生产应用程序中使用此类。

## <a name="requirements"></a>要求

**命名空间：** <xref:System.Net>

**程序集：** System.dll 中的系统 () 

**.NET Framework 版本：** 自2.0 起可用。
