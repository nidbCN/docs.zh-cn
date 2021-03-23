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
# <a name="wcf-data-services-client-library"></a><span data-ttu-id="433f1-103">WCF 数据服务客户端库</span><span class="sxs-lookup"><span data-stu-id="433f1-103">WCF Data Services Client Library</span></span>

[!INCLUDE [wcf-deprecated](~/includes/wcf-deprecated.md)]

<span data-ttu-id="433f1-104">如果任何应用程序可以发送 HTTP 请求并处理数据服务返回的 OData 源，则该应用程序可以与 Open Data Protocol (OData 基于 OData) 的数据服务进行交互。</span><span class="sxs-lookup"><span data-stu-id="433f1-104">Any application can interact with an Open Data Protocol (OData)-based data service if it can send an HTTP request and process the OData feed that a data service returns.</span></span> <span data-ttu-id="433f1-105">利用这种互操作性，您可以从各种启用 Web 的应用程序访问基于 OData 的服务。</span><span class="sxs-lookup"><span data-stu-id="433f1-105">This interoperability enables you to access OData-based services from a broad range of Web-enabled applications.</span></span> <span data-ttu-id="433f1-106">WCF Data Services 包括从 .NET Framework 或基于 Silverlight 的应用程序使用 OData 源时提供更丰富的编程体验的客户端库。</span><span class="sxs-lookup"><span data-stu-id="433f1-106">WCF Data Services includes client libraries that provide a richer programming experience when you consume OData feeds from .NET Framework or Silverlight-based applications.</span></span>  
  
 <span data-ttu-id="433f1-107">客户端库的两大主要类为 <xref:System.Data.Services.Client.DataServiceContext> 类和 <xref:System.Data.Services.Client.DataServiceQuery%601> 类。</span><span class="sxs-lookup"><span data-stu-id="433f1-107">The two main classes of the client library are the <xref:System.Data.Services.Client.DataServiceContext> class and the <xref:System.Data.Services.Client.DataServiceQuery%601> class.</span></span> <span data-ttu-id="433f1-108"><xref:System.Data.Services.Client.DataServiceContext> 类封装针对指定数据服务支持的操作。</span><span class="sxs-lookup"><span data-stu-id="433f1-108">The <xref:System.Data.Services.Client.DataServiceContext> class encapsulates operations that are supported against a specified data service.</span></span> <span data-ttu-id="433f1-109">尽管 OData 服务是无状态的，但上下文不是。</span><span class="sxs-lookup"><span data-stu-id="433f1-109">Although OData services are stateless, the context is not.</span></span> <span data-ttu-id="433f1-110">因此，你可以使用 <xref:System.Data.Services.Client.DataServiceContext> 类在与数据服务之间的交互之间维护客户端的状态，以支持更改管理等功能。</span><span class="sxs-lookup"><span data-stu-id="433f1-110">Therefore, you can use the <xref:System.Data.Services.Client.DataServiceContext> class to maintain state on the client between interactions with the data service in order to support features such as change management.</span></span> <span data-ttu-id="433f1-111">该类还对更改的标识和跟踪进行管理。</span><span class="sxs-lookup"><span data-stu-id="433f1-111">This class also manages identities and tracks changes.</span></span> <span data-ttu-id="433f1-112"><xref:System.Data.Services.Client.DataServiceQuery%601> 类表示一个针对特定实体集的查询。</span><span class="sxs-lookup"><span data-stu-id="433f1-112">The <xref:System.Data.Services.Client.DataServiceQuery%601> class represents a query against a specific entity set.</span></span>  
  
 <span data-ttu-id="433f1-113">本节介绍如何使用客户端库从 .NET Framework 客户端应用程序访问和更改数据。</span><span class="sxs-lookup"><span data-stu-id="433f1-113">This section describes how to use client libraries to access and change data from a .NET Framework client application.</span></span> <span data-ttu-id="433f1-114">有关如何使用基于 Silverlight 的应用程序的 WCF Data Services 客户端库的详细信息，请参阅 [WCF Data Services (silverlight) ](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95))。</span><span class="sxs-lookup"><span data-stu-id="433f1-114">For more information about how to use the WCF Data Services client library with a Silverlight-based application, see [WCF Data Services (Silverlight)](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v=vs.95)).</span></span> <span data-ttu-id="433f1-115">其他客户端库可用，可用于在其他类型的应用程序中使用 OData 源。</span><span class="sxs-lookup"><span data-stu-id="433f1-115">Other client libraries are available that enable you to consume an OData feed in other kinds of applications.</span></span> <span data-ttu-id="433f1-116">有关 OData SDK 的详细信息，请参阅 [ODATA sdk-示例代码](https://www.odata.org/ecosystem/#sdk)。</span><span class="sxs-lookup"><span data-stu-id="433f1-116">For more information on OData SDK, see [OData SDK - Sample Code](https://www.odata.org/ecosystem/#sdk).</span></span>
  
## <a name="in-this-section"></a><span data-ttu-id="433f1-117">本节内容</span><span class="sxs-lookup"><span data-stu-id="433f1-117">In This Section</span></span>  

 [<span data-ttu-id="433f1-118">生成数据服务客户端库</span><span class="sxs-lookup"><span data-stu-id="433f1-118">Generating the Data Service Client Library</span></span>](generating-the-data-service-client-library-wcf-data-services.md)  
 <span data-ttu-id="433f1-119">介绍如何生成基于 OData 源的客户端库和客户端数据服务类。</span><span class="sxs-lookup"><span data-stu-id="433f1-119">Describes how to generate a client library and client data service classes that are based on OData feeds.</span></span>  
  
 [<span data-ttu-id="433f1-120">查询数据服务</span><span class="sxs-lookup"><span data-stu-id="433f1-120">Querying the Data Service</span></span>](querying-the-data-service-wcf-data-services.md)  
 <span data-ttu-id="433f1-121">介绍如何使用客户端库从基于 .NET Framework 的应用程序查询数据服务。</span><span class="sxs-lookup"><span data-stu-id="433f1-121">Describes how to query a data service from a .NET Framework-based application by using client libraries.</span></span>  
  
 [<span data-ttu-id="433f1-122">加载延迟的内容</span><span class="sxs-lookup"><span data-stu-id="433f1-122">Loading Deferred Content</span></span>](loading-deferred-content-wcf-data-services.md)  
 <span data-ttu-id="433f1-123">介绍如何加载未包含在初始查询响应中的附加内容。</span><span class="sxs-lookup"><span data-stu-id="433f1-123">Describes how to load additional content not included in the initial query response.</span></span>  
  
 [<span data-ttu-id="433f1-124">更新数据服务</span><span class="sxs-lookup"><span data-stu-id="433f1-124">Updating the Data Service</span></span>](updating-the-data-service-wcf-data-services.md)  
 <span data-ttu-id="433f1-125">介绍如何使用客户端库来创建、修改和删除实体和关系。</span><span class="sxs-lookup"><span data-stu-id="433f1-125">Describes how to create, modify, and delete entities and relationships by using the client libraries.</span></span>  
  
 [<span data-ttu-id="433f1-126">异步操作</span><span class="sxs-lookup"><span data-stu-id="433f1-126">Asynchronous Operations</span></span>](asynchronous-operations-wcf-data-services.md)  
 <span data-ttu-id="433f1-127">介绍客户端库提供的用于以异步方式使用数据服务的功能。</span><span class="sxs-lookup"><span data-stu-id="433f1-127">Describes the facilities provided by the client libraries for working with a data service in an asynchronous manner.</span></span>  
  
 [<span data-ttu-id="433f1-128">批处理操作</span><span class="sxs-lookup"><span data-stu-id="433f1-128">Batching Operations</span></span>](batching-operations-wcf-data-services.md)  
 <span data-ttu-id="433f1-129">介绍如何使用客户端库在一个批处理中向数据服务发送多个请求。</span><span class="sxs-lookup"><span data-stu-id="433f1-129">Describes how to send multiple requests to the data service in a single batch by using the client libraries.</span></span>  
  
 [<span data-ttu-id="433f1-130">将数据绑定到控件</span><span class="sxs-lookup"><span data-stu-id="433f1-130">Binding Data to Controls</span></span>](binding-data-to-controls-wcf-data-services.md)  
 <span data-ttu-id="433f1-131">介绍如何将控件绑定到数据服务返回的 OData 源。</span><span class="sxs-lookup"><span data-stu-id="433f1-131">Describes how to bind controls to a OData feed returned by a data service.</span></span>  
  
 [<span data-ttu-id="433f1-132">调用服务操作</span><span class="sxs-lookup"><span data-stu-id="433f1-132">Calling Service Operations</span></span>](calling-service-operations-wcf-data-services.md)  
 <span data-ttu-id="433f1-133">介绍如何使用客户端库调用服务操作。</span><span class="sxs-lookup"><span data-stu-id="433f1-133">Describes how to use the client library to call service operations.</span></span>  
  
 [<span data-ttu-id="433f1-134">管理数据服务上下文</span><span class="sxs-lookup"><span data-stu-id="433f1-134">Managing the Data Service Context</span></span>](managing-the-data-service-context-wcf-data-services.md)  
 <span data-ttu-id="433f1-135">介绍用于管理客户端库的行为的选项。</span><span class="sxs-lookup"><span data-stu-id="433f1-135">Describes options for managing the behavior of the client library.</span></span>  
  
 [<span data-ttu-id="433f1-136">处理二进制数据</span><span class="sxs-lookup"><span data-stu-id="433f1-136">Working with Binary Data</span></span>](working-with-binary-data-wcf-data-services.md)  
 <span data-ttu-id="433f1-137">介绍如何访问和更改数据服务作为数据流返回的二进制数据。</span><span class="sxs-lookup"><span data-stu-id="433f1-137">Describes how to access and change binary data returned by the data service as a data stream.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="433f1-138">另请参阅</span><span class="sxs-lookup"><span data-stu-id="433f1-138">See also</span></span>

- [<span data-ttu-id="433f1-139">定义 WCF 数据服务</span><span class="sxs-lookup"><span data-stu-id="433f1-139">Defining WCF Data Services</span></span>](defining-wcf-data-services.md)
- [<span data-ttu-id="433f1-140">入门</span><span class="sxs-lookup"><span data-stu-id="433f1-140">Getting Started</span></span>](getting-started-with-wcf-data-services.md)
