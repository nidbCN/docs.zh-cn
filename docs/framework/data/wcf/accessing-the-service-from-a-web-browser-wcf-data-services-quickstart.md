---
title: 从 Web 浏览器访问服务（WCF 数据服务快速入门）
description: 了解如何在 Visual Studio 中开始 WCF Data Services 并在浏览器中禁用源读取。 获取服务定义文档并访问数据服务资源。
ms.date: 03/30/2017
ms.assetid: 5a6fa180-3094-4e6e-ba2b-8c80975d18d1
ms.openlocfilehash: 27b599b6bf28b2ee0810564a3fb73b14274467ff
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805540"
---
# <a name="accessing-the-service-from-a-web-browser-wcf-data-services-quickstart"></a><span data-ttu-id="0a2bd-104">从 Web 浏览器访问服务（WCF 数据服务快速入门）</span><span class="sxs-lookup"><span data-stu-id="0a2bd-104">Accessing the Service from a Web Browser (WCF Data Services Quickstart)</span></span>

[!INCLUDE [wcf-deprecated](~/includes/wcf-deprecated.md)]

<span data-ttu-id="0a2bd-105">这是 WCF Data Services 快速入门的第二个任务。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-105">This is the second task of the WCF Data Services quickstart.</span></span> <span data-ttu-id="0a2bd-106">在此任务中，您将从 Visual Studio 中启动 WCF Data Services，还可以选择在 Web 浏览器中禁用源读取。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-106">In this task, you start the WCF Data Services from Visual Studio and optionally disable feed reading in the Web browser.</span></span> <span data-ttu-id="0a2bd-107">然后，通过 Web 浏览器向公开的资源提交 HTTP GET 请求，检索服务定义文档并访问数据服务资源。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-107">You then retrieve the service definition document as well as access data service resources by submitting HTTP GET requests through a Web browser to the exposed resources.</span></span>

> [!NOTE]
> <span data-ttu-id="0a2bd-108">默认情况下，Visual Studio 自动为计算机上的 `localhost` URI 分配一个端口号。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-108">By default, Visual Studio auto-assigns a port number to the `localhost` URI on your computer.</span></span> <span data-ttu-id="0a2bd-109">本任务在 URI 示例中使用端口号 `12345`。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-109">This task uses the port number `12345` in the URI examples.</span></span> <span data-ttu-id="0a2bd-110">有关如何在 Visual Studio 项目中设置特定端口号的详细信息，请参阅 [创建数据服务](creating-the-data-service.md)。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-110">For more information about how to set a specific port number in your Visual Studio project see [Creating the Data Service](creating-the-data-service.md).</span></span>

## <a name="to-request-the-default-service-document-by-using-internet-explorer"></a><span data-ttu-id="0a2bd-111">使用 Internet Explorer 请求默认服务文档</span><span class="sxs-lookup"><span data-stu-id="0a2bd-111">To request the default service document by using Internet Explorer</span></span>

1. <span data-ttu-id="0a2bd-112">在 Internet Explorer 的 " **工具** " 菜单中，选择 " **Internet 选项**"，单击 " **内容** " 选项卡，单击 " **设置**"，然后清除 **"启用源查看"**。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-112">In Internet Explorer, from the **Tools** menu, select **Internet Options**, click the **Content** tab, click **Settings**, and clear **Turn on feed viewing**.</span></span>

     <span data-ttu-id="0a2bd-113">这可确保禁用源阅读。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-113">This makes sure that feed reading is disabled.</span></span> <span data-ttu-id="0a2bd-114">如果未禁用此功能，则 Web 浏览器会将返回的 AtomPub 编码文档视为 XML 源，而不是显示原始 XML 数据。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-114">If you do not disable this functionality, then the Web browser will treat the returned AtomPub encoded document as an XML feed instead of displaying the raw XML data.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0a2bd-115">当浏览器无法将该源作为原始 XML 数据显示时，你应该仍能够以页面源代码的形式查看该源。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-115">If your browser cannot display the feed as raw XML data, you should still be able to view the feed as the source code for the page.</span></span>

2. <span data-ttu-id="0a2bd-116">在 Visual Studio 中，按 **F5** 键开始调试应用程序。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-116">In Visual Studio, press the **F5** key to start debugging the application.</span></span>

3. <span data-ttu-id="0a2bd-117">在本地计算机上打开 Web 浏览器。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-117">Open a Web browser on the local computer.</span></span> <span data-ttu-id="0a2bd-118">在地址栏中，输入以下 URI：</span><span class="sxs-lookup"><span data-stu-id="0a2bd-118">In the address bar, enter the following URI:</span></span>

    ```http
    http://localhost:12345/northwind.svc
    ```

     <span data-ttu-id="0a2bd-119">这会返回默认服务文档，其中包含由此数据服务公开的实体集的列表。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-119">This returns the default service document, which contains a list of entity sets that are exposed by this data service.</span></span>

## <a name="to-access-entity-set-resources-from-a-web-browser"></a><span data-ttu-id="0a2bd-120">从 Web 浏览器访问实体集资源</span><span class="sxs-lookup"><span data-stu-id="0a2bd-120">To access entity set resources from a Web browser</span></span>

1. <span data-ttu-id="0a2bd-121">在 Web 浏览器的地址栏中，输入以下 URI：</span><span class="sxs-lookup"><span data-stu-id="0a2bd-121">In the address bar of your Web browser, enter the following URI:</span></span>

    ```http
    http://localhost:12345/northwind.svc/Customers
    ```

     <span data-ttu-id="0a2bd-122">这会返回 Northwind 示例数据库中所有客户的集。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-122">This returns a set of all customers in the Northwind sample database.</span></span>

2. <span data-ttu-id="0a2bd-123">在 Web 浏览器的地址栏中，输入以下 URI：</span><span class="sxs-lookup"><span data-stu-id="0a2bd-123">In the address bar of your Web browser, enter the following URI:</span></span>

    ```http
    http://localhost:12345/northwind.svc/Customers('ALFKI')
    ```

     <span data-ttu-id="0a2bd-124">这会返回特定客户 `ALFKI` 的实体实例。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-124">This returns an entity instance for the specific customer, `ALFKI`.</span></span>

3. <span data-ttu-id="0a2bd-125">在 Web 浏览器的地址栏中，输入以下 URI：</span><span class="sxs-lookup"><span data-stu-id="0a2bd-125">In the address bar of your Web browser, enter the following URI:</span></span>

    ```http
    http://localhost:12345/northwind.svc/Customers('ALFKI')/Orders
    ```

     <span data-ttu-id="0a2bd-126">这会遍历客户与订单之间的关系，以返回特定客户 `ALFKI` 的所有订单的集。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-126">This traverses the relationship between customers and orders to return a set of all orders for the specific customer `ALFKI`.</span></span>

4. <span data-ttu-id="0a2bd-127">在 Web 浏览器的地址栏中，输入以下 URI：</span><span class="sxs-lookup"><span data-stu-id="0a2bd-127">In the address bar of your Web browser, enter the following URI:</span></span>

    ```http
    http://localhost:12345/northwind.svc/Customers('ALFKI')/Orders?$filter=OrderID eq 10643
    ```

     <span data-ttu-id="0a2bd-128">这会筛选属于特定客户 `ALFKI` 的订单，以便只根据提供的 `OrderID` 值返回特定订单。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-128">This filters orders that belong to the specific customer `ALFKI` so that only a specific order is returned based on the supplied `OrderID` value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a2bd-129">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0a2bd-129">Next Steps</span></span>

<span data-ttu-id="0a2bd-130">已成功从 Web 浏览器访问 WCF Data Services，浏览器向指定资源发出 HTTP GET 请求。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-130">You have successfully accessed the WCF Data Services from a Web browser, with the browser issuing HTTP GET requests to specified resources.</span></span> <span data-ttu-id="0a2bd-131">使用 Web 浏览器，可以轻松试验请求的寻址语法并查看结果。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-131">A Web browser provides an easy way to experiment with the addressing syntax of requests and view the results.</span></span> <span data-ttu-id="0a2bd-132">不过，通常情况下并不会通过此方式访问生产数据服务。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-132">However, a production data service is not generally accessed by this method.</span></span> <span data-ttu-id="0a2bd-133">通常，应用程序会通过应用程序代码或脚本语言与数据服务进行交互。</span><span class="sxs-lookup"><span data-stu-id="0a2bd-133">Typically, applications interact with the data service through application code or scripting languages.</span></span> <span data-ttu-id="0a2bd-134">接下来，您将创建一个客户端应用程序，该应用程序使用客户端库访问数据服务资源，就像它们是公共语言运行时 (CLR) 对象一样：</span><span class="sxs-lookup"><span data-stu-id="0a2bd-134">Next, you will create a client application that uses client libraries to access data service resources as if they were common language runtime (CLR) objects:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0a2bd-135">创建 .NET Framework 客户端应用程序</span><span class="sxs-lookup"><span data-stu-id="0a2bd-135">Creating the .NET Framework Client Application</span></span>](creating-the-dotnet-client-application-wcf-data-services-quickstart.md)

## <a name="see-also"></a><span data-ttu-id="0a2bd-136">另请参阅</span><span class="sxs-lookup"><span data-stu-id="0a2bd-136">See also</span></span>

- [<span data-ttu-id="0a2bd-137">访问数据服务资源</span><span class="sxs-lookup"><span data-stu-id="0a2bd-137">Accessing Data Service Resources</span></span>](accessing-data-service-resources-wcf-data-services.md)
