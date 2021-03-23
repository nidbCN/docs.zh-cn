---
title: 在 Visual Studio 中创建 WCF 数据服务
description: 了解如何创建一个示例数据服务，该服务使用 WCF Data Services 来公开基于示例数据库的 OData 源。
ms.date: 08/24/2018
dev_langs:
- csharp
- vb
ms.assetid: 34d1d971-5e18-4c22-9bf6-d3612e27ea59
ms.openlocfilehash: 04c91483587c8976e40584474ced2f5474ab89f4
ms.sourcegitcommit: 20b4565974d185c7716656a6c63e3cfdbdf4bf41
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104805761"
---
# <a name="create-the-data-service"></a><span data-ttu-id="a2ab5-103">创建数据服务</span><span class="sxs-lookup"><span data-stu-id="a2ab5-103">Create the data service</span></span>

[!INCLUDE [wcf-deprecated](~/includes/wcf-deprecated.md)]

<span data-ttu-id="a2ab5-104">在本主题中，你将创建一个示例数据服务，该服务使用 WCF Data Services 公开基于 Northwind 示例数据库的 Open Data Protocol (OData) 源。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-104">In this topic, you create a sample data service that uses WCF Data Services to expose an Open Data Protocol (OData) feed that's based on the Northwind sample database.</span></span> <span data-ttu-id="a2ab5-105">此任务涉及以下几个基本步骤：</span><span class="sxs-lookup"><span data-stu-id="a2ab5-105">The task involves the following basic steps:</span></span>

1. <span data-ttu-id="a2ab5-106">创建 ASP.NET Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-106">Create an ASP.NET Web application.</span></span>

2. <span data-ttu-id="a2ab5-107">使用实体数据模型工具定义数据模型。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-107">Define the data model by using the Entity Data Model tools.</span></span>

3. <span data-ttu-id="a2ab5-108">向 Web 应用程序添加数据服务。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-108">Add the data service to the Web application.</span></span>

4. <span data-ttu-id="a2ab5-109">启用对数据服务的访问。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-109">Enable access to the data service.</span></span>

## <a name="create-the-aspnet-web-app"></a><span data-ttu-id="a2ab5-110">创建 ASP.NET web 应用</span><span class="sxs-lookup"><span data-stu-id="a2ab5-110">Create the ASP.NET web app</span></span>

1. <span data-ttu-id="a2ab5-111">在 Visual Studio 的“文件”菜单中，依次选择“新建” > “项目”    。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-111">In Visual Studio, on the **File** menu, select **New** > **Project**.</span></span>

1. <span data-ttu-id="a2ab5-112">在 " **新建项目** " 对话框中，在 "Visual Basic" 或 "Visual c #" 下选择 " **web** " 类别，然后选择 " **ASP.NET web 应用程序**"。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-112">In the **New Project** dialog box, under either Visual Basic or Visual C# select the **Web** category, and then select **ASP.NET Web Application**.</span></span>

1. <span data-ttu-id="a2ab5-113">输入 `NorthwindService` 作为项目名称，然后选择 **"确定"**。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-113">Enter `NorthwindService` as the name of the project and then select **OK**.</span></span>

1. <span data-ttu-id="a2ab5-114">在 " **新建 ASP.NET Web 应用程序** " 对话框中，选择 " **空** "，然后选择 **"确定"**。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-114">In the **New ASP.NET Web Application** dialog, select **Empty** and then select **OK**.</span></span>

1. <span data-ttu-id="a2ab5-115">（可选）为 Web 应用程序指定一个特定的端口号。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-115">(Optional) Specify a specific port number for your Web application.</span></span> <span data-ttu-id="a2ab5-116">注意： `12345` 此系列快速入门主题中使用了端口号。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-116">Note: the port number `12345` is used in this series of quickstart topics.</span></span>

    1. <span data-ttu-id="a2ab5-117">在 **解决方案资源管理器** 中，右键单击刚创建的 ASP.NET 项目，然后选择 " **属性**"。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-117">In **Solution Explorer**, right-click on the ASP.NET project that you just created, and then choose **Properties**.</span></span>

    2. <span data-ttu-id="a2ab5-118">选择 " **Web** " 选项卡，并将 " **特定端口** " 文本框的值设置为 `12345` 。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-118">Select the **Web** tab, and set the value of the **Specific port** text box to `12345`.</span></span>

## <a name="define-the-data-model"></a><span data-ttu-id="a2ab5-119">定义数据模型</span><span class="sxs-lookup"><span data-stu-id="a2ab5-119">Define the data model</span></span>

1. <span data-ttu-id="a2ab5-120">在 **解决方案资源管理器** 中，右键单击 ASP.NET 项目的名称，然后单击 "**添加**  >  **新项**"。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-120">In **Solution Explorer**, right-click the name of the ASP.NET project, and then click **Add** > **New Item**.</span></span>

2. <span data-ttu-id="a2ab5-121">在 " **添加新项** " 对话框中，选择 " **数据** " 类别，然后选择 " **ADO.NET 实体数据模型**"。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-121">In the **Add New Item** dialog box, select the **Data** category, and then select **ADO.NET Entity Data Model**.</span></span>

3. <span data-ttu-id="a2ab5-122">对于数据模型的名称，请输入 `Northwind.edmx` 。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-122">For the name of the data model, enter `Northwind.edmx`.</span></span>

4. <span data-ttu-id="a2ab5-123">在 **实体数据模型向导** 中，选择 " **数据库中的 EF 设计器**"，然后单击 " **下一步**"。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-123">In the **Entity Data Model Wizard**, select **EF Designer from Database**, and then click **Next**.</span></span>

5. <span data-ttu-id="a2ab5-124">执行以下步骤之一，将数据模型连接到数据库，然后单击 " **下一步**"：</span><span class="sxs-lookup"><span data-stu-id="a2ab5-124">Connect the data model to the database by doing one of the following steps, and then click **Next**:</span></span>

    - <span data-ttu-id="a2ab5-125">如果尚未配置数据库连接，请单击 " **新建连接** " 并创建一个新连接。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-125">If you don't have a database connection already configured, click **New Connection** and create a new connection.</span></span> <span data-ttu-id="a2ab5-126">有关详细信息，请参阅 [How to: Create Connections to SQL Server Databases](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-126">For more information, see [How to: Create Connections to SQL Server Databases](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90)).</span></span> <span data-ttu-id="a2ab5-127">此 SQL Server 实例必须附加了 Northwind 示例数据库。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-127">This SQL Server instance must have the Northwind sample database attached.</span></span>

         <span data-ttu-id="a2ab5-128">\- 或 -</span><span class="sxs-lookup"><span data-stu-id="a2ab5-128">\- or -</span></span>

    - <span data-ttu-id="a2ab5-129">如果已配置一个连接到 Northwind 数据库的数据库连接，请从连接列表中选择该连接。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-129">If you have a database connection already configured to connect to the Northwind database, select that connection from the list of connections.</span></span>

6. <span data-ttu-id="a2ab5-130">在向导的最后一页中，选中数据库中所有表对应的复选框，并清除视图和存储过程对应的复选框。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-130">On the final page of the wizard, select the check boxes for all tables in the database, and clear the check boxes for views and stored procedures.</span></span>

7. <span data-ttu-id="a2ab5-131">单击 " **完成** " 关闭向导。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-131">Click **Finish** to close the wizard.</span></span>

## <a name="create-the-wcf-data-service"></a><span data-ttu-id="a2ab5-132">创建 WCF 数据服务</span><span class="sxs-lookup"><span data-stu-id="a2ab5-132">Create the WCF data service</span></span>

1. <span data-ttu-id="a2ab5-133">在 **解决方案资源管理器** 中，右键单击 ASP.NET 项目，然后选择 "**添加**  >  **新项**"。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-133">In **Solution Explorer**, right-click on the ASP.NET project, and then choose **Add** > **New Item**.</span></span>

2. <span data-ttu-id="a2ab5-134">在 "**添加新项**" 对话框中，从 " **Web** " 类别中选择 " **WCF 数据服务**" 项模板。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-134">In the **Add New Item** dialog box, select the **WCF Data Service** item template from the **Web** category.</span></span>

   ![Visual Studio 2015 中的 WCF 数据服务项模板](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > <span data-ttu-id="a2ab5-136">**WCF 数据服务** 模板在 visual studio 2015 中提供，但在 visual studio 2017 或更高版本中不可用。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-136">The **WCF Data Service** template is available in Visual Studio 2015, but not in Visual Studio 2017 or later.</span></span>

3. <span data-ttu-id="a2ab5-137">对于服务的名称，请键入 `Northwind` 。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-137">For the name of the service, type `Northwind`.</span></span>

     <span data-ttu-id="a2ab5-138">Visual Studio 将为新服务创建 XML 标记和代码文件。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-138">Visual Studio creates the XML markup and code files for the new service.</span></span> <span data-ttu-id="a2ab5-139">默认情况下，代码编辑器窗口将打开。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-139">By default, the code-editor window opens.</span></span> <span data-ttu-id="a2ab5-140">在 **解决方案资源管理器** 中，服务的名称为 Northwind，扩展名为 *.svc* 或 *.svc*。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-140">In **Solution Explorer**, the service has the name Northwind with the extension *.svc.cs* or *.svc.vb*.</span></span>

4. <span data-ttu-id="a2ab5-141">在数据服务的代码中，用数据模型的实体容器的类型（在此示例中为 `/* TODO: put your data source class name here */`）替换定义数据服务的类定义中的注释 `NorthwindEntities`。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-141">In the code for the data service, replace the comment `/* TODO: put your data source class name here */` in the definition of the class that defines the data service with the type that is the entity container of the data model, which in this case is `NorthwindEntities`.</span></span> <span data-ttu-id="a2ab5-142">该类定义应如下所示：</span><span class="sxs-lookup"><span data-stu-id="a2ab5-142">The class definition should look this the following:</span></span>

     [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
     [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

## <a name="enable-access-to-data-service-resources"></a><span data-ttu-id="a2ab5-143">启用对数据服务资源的访问</span><span class="sxs-lookup"><span data-stu-id="a2ab5-143">Enable access to data service resources</span></span>

1. <span data-ttu-id="a2ab5-144">在数据服务的代码中，用下列代码替换 `InitializeService` 函数中的占位符代码：</span><span class="sxs-lookup"><span data-stu-id="a2ab5-144">In the code for the data service, replace the placeholder code in the `InitializeService` function with the following:</span></span>

     [!code-csharp[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#allreadconfig)]
     [!code-vb[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#allreadconfig)]

     <span data-ttu-id="a2ab5-145">这使得授权客户端能够对指定实体集的资源进行读写访问。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-145">This enables authorized clients to have read and write access to resources for the specified entity sets.</span></span>

    > [!NOTE]
    > <span data-ttu-id="a2ab5-146">任何可以访问该 ASP.NET 应用程序的客户端也能够访问由数据服务公开的资源。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-146">Any client that can access the ASP.NET application can also access the resources exposed by the data service.</span></span> <span data-ttu-id="a2ab5-147">在生产数据服务中，为防止对资源进行未经授权的访问，还应保护应用程序本身的安全。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-147">In a production data service, to prevent unauthorized access to resources you should also secure the application itself.</span></span> <span data-ttu-id="a2ab5-148">有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-148">For more information, see [Securing WCF Data Services](securing-wcf-data-services.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a2ab5-149">后续步骤</span><span class="sxs-lookup"><span data-stu-id="a2ab5-149">Next steps</span></span>

<span data-ttu-id="a2ab5-150">您已经成功地创建了一个公开基于 Northwind 示例数据库的 OData 源的新数据服务，并且您已为对 ASP.NET Web 应用程序具有权限的客户端启用了对源的访问。</span><span class="sxs-lookup"><span data-stu-id="a2ab5-150">You have successfully created a new data service that exposes an OData feed that is based on the Northwind sample database, and you have enabled access to the feed for clients that have permissions on the ASP.NET Web application.</span></span> <span data-ttu-id="a2ab5-151">接下来，将从 Visual Studio 启动数据服务，并通过 Web 浏览器提交 HTTP GET 请求来访问 OData 数据源：</span><span class="sxs-lookup"><span data-stu-id="a2ab5-151">Next, you'll start the data service from Visual Studio and access the OData feed by submitting HTTP GET requests through a Web browser:</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a2ab5-152">从 web 浏览器访问服务</span><span class="sxs-lookup"><span data-stu-id="a2ab5-152">Access the service from a web browser</span></span>](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)

## <a name="see-also"></a><span data-ttu-id="a2ab5-153">另请参阅</span><span class="sxs-lookup"><span data-stu-id="a2ab5-153">See also</span></span>

- <span data-ttu-id="a2ab5-154">[ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))</span><span class="sxs-lookup"><span data-stu-id="a2ab5-154">[ADO.NET Entity Data Model Tools](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))</span></span>
