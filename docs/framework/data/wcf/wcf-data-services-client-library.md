---
title: WCF 数据服务客户端库
description: 了解如何使用 WCF Data Services 客户端库从 .NET Framework 客户端应用程序访问和更改数据。
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
- DataServiceQuery class, about DataServiceQuery class
- DataServiceContext class, about DataServiceContext class
ms.assetid: 21075e50-8917-413e-a8ea-35a0f6e65aa5
ms.openlocfilehash: 3d8a0f869454ce24d847127c2097239886f3395b
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805657"
---
# <a name="wcf-data-services-client-library"></a>WCF 数据服务客户端库

[!INCLUDE [wcf-deprecated](~/includes/wcf-deprecated.md)]

如果任何应用程序可以发送 HTTP 请求并处理数据服务返回的 OData 源，则该应用程序可以与 Open Data Protocol (OData 基于 OData) 的数据服务进行交互。 利用这种互操作性，您可以从各种启用 Web 的应用程序访问基于 OData 的服务。 WCF Data Services 包括从 .NET Framework 或基于 Silverlight 的应用程序使用 OData 源时提供更丰富的编程体验的客户端库。  
  
 客户端库的两大主要类为 <xref:System.Data.Services.Client.DataServiceContext> 类和 <xref:System.Data.Services.Client.DataServiceQuery%601> 类。 <xref:System.Data.Services.Client.DataServiceContext> 类封装针对指定数据服务支持的操作。 尽管 OData 服务是无状态的，但上下文不是。 因此，你可以使用 <xref:System.Data.Services.Client.DataServiceContext> 类在与数据服务之间的交互之间维护客户端的状态，以支持更改管理等功能。 该类还对更改的标识和跟踪进行管理。 <xref:System.Data.Services.Client.DataServiceQuery%601> 类表示一个针对特定实体集的查询。  
  
 本节介绍如何使用客户端库从 .NET Framework 客户端应用程序访问和更改数据。 有关如何使用基于 Silverlight 的应用程序的 WCF Data Services 客户端库的详细信息，请参阅 [WCF Data Services (silverlight) ](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95))。 其他客户端库可用，可用于在其他类型的应用程序中使用 OData 源。 有关 OData SDK 的详细信息，请参阅 [ODATA sdk-示例代码](https://www.odata.org/ecosystem/#sdk)。
  
## <a name="in-this-section"></a>本节内容  

 [生成数据服务客户端库](generating-the-data-service-client-library-wcf-data-services.md)  
 介绍如何生成基于 OData 源的客户端库和客户端数据服务类。  
  
 [查询数据服务](querying-the-data-service-wcf-data-services.md)  
 介绍如何使用客户端库从基于 .NET Framework 的应用程序查询数据服务。  
  
 [加载延迟的内容](loading-deferred-content-wcf-data-services.md)  
 介绍如何加载未包含在初始查询响应中的附加内容。  
  
 [更新数据服务](updating-the-data-service-wcf-data-services.md)  
 介绍如何使用客户端库来创建、修改和删除实体和关系。  
  
 [异步操作](asynchronous-operations-wcf-data-services.md)  
 介绍客户端库提供的用于以异步方式使用数据服务的功能。  
  
 [批处理操作](batching-operations-wcf-data-services.md)  
 介绍如何使用客户端库在一个批处理中向数据服务发送多个请求。  
  
 [将数据绑定到控件](binding-data-to-controls-wcf-data-services.md)  
 介绍如何将控件绑定到数据服务返回的 OData 源。  
  
 [调用服务操作](calling-service-operations-wcf-data-services.md)  
 介绍如何使用客户端库调用服务操作。  
  
 [管理数据服务上下文](managing-the-data-service-context-wcf-data-services.md)  
 介绍用于管理客户端库的行为的选项。  
  
 [处理二进制数据](working-with-binary-data-wcf-data-services.md)  
 介绍如何访问和更改数据服务作为数据流返回的二进制数据。  
  
## <a name="see-also"></a>另请参阅

- [定义 WCF 数据服务](defining-wcf-data-services.md)
- [入门](getting-started-with-wcf-data-services.md)
