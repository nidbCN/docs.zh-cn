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
# <a name="create-the-data-service"></a>创建数据服务

[!INCLUDE [wcf-deprecated](~/includes/wcf-deprecated.md)]

在本主题中，你将创建一个示例数据服务，该服务使用 WCF Data Services 公开基于 Northwind 示例数据库的 Open Data Protocol (OData) 源。 此任务涉及以下几个基本步骤：

1. 创建 ASP.NET Web 应用程序。

2. 使用实体数据模型工具定义数据模型。

3. 向 Web 应用程序添加数据服务。

4. 启用对数据服务的访问。

## <a name="create-the-aspnet-web-app"></a>创建 ASP.NET web 应用

1. 在 Visual Studio 的“文件”菜单中，依次选择“新建” > “项目”    。

1. 在 " **新建项目** " 对话框中，在 "Visual Basic" 或 "Visual c #" 下选择 " **web** " 类别，然后选择 " **ASP.NET web 应用程序**"。

1. 输入 `NorthwindService` 作为项目名称，然后选择 **"确定"**。

1. 在 " **新建 ASP.NET Web 应用程序** " 对话框中，选择 " **空** "，然后选择 **"确定"**。

1. （可选）为 Web 应用程序指定一个特定的端口号。 注意： `12345` 此系列快速入门主题中使用了端口号。

    1. 在 **解决方案资源管理器** 中，右键单击刚创建的 ASP.NET 项目，然后选择 " **属性**"。

    2. 选择 " **Web** " 选项卡，并将 " **特定端口** " 文本框的值设置为 `12345` 。

## <a name="define-the-data-model"></a>定义数据模型

1. 在 **解决方案资源管理器** 中，右键单击 ASP.NET 项目的名称，然后单击 "**添加**  >  **新项**"。

2. 在 " **添加新项** " 对话框中，选择 " **数据** " 类别，然后选择 " **ADO.NET 实体数据模型**"。

3. 对于数据模型的名称，请输入 `Northwind.edmx` 。

4. 在 **实体数据模型向导** 中，选择 " **数据库中的 EF 设计器**"，然后单击 " **下一步**"。

5. 执行以下步骤之一，将数据模型连接到数据库，然后单击 " **下一步**"：

    - 如果尚未配置数据库连接，请单击 " **新建连接** " 并创建一个新连接。 有关详细信息，请参阅 [How to: Create Connections to SQL Server Databases](/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))。 此 SQL Server 实例必须附加了 Northwind 示例数据库。

         \- 或 -

    - 如果已配置一个连接到 Northwind 数据库的数据库连接，请从连接列表中选择该连接。

6. 在向导的最后一页中，选中数据库中所有表对应的复选框，并清除视图和存储过程对应的复选框。

7. 单击 " **完成** " 关闭向导。

## <a name="create-the-wcf-data-service"></a>创建 WCF 数据服务

1. 在 **解决方案资源管理器** 中，右键单击 ASP.NET 项目，然后选择 "**添加**  >  **新项**"。

2. 在 "**添加新项**" 对话框中，从 " **Web** " 类别中选择 " **WCF 数据服务**" 项模板。

   ![Visual Studio 2015 中的 WCF 数据服务项模板](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > **WCF 数据服务** 模板在 visual studio 2015 中提供，但在 visual studio 2017 或更高版本中不可用。

3. 对于服务的名称，请键入 `Northwind` 。

     Visual Studio 将为新服务创建 XML 标记和代码文件。 默认情况下，代码编辑器窗口将打开。 在 **解决方案资源管理器** 中，服务的名称为 Northwind，扩展名为 *.svc* 或 *.svc*。

4. 在数据服务的代码中，用数据模型的实体容器的类型（在此示例中为 `/* TODO: put your data source class name here */`）替换定义数据服务的类定义中的注释 `NorthwindEntities`。 该类定义应如下所示：

     [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
     [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

## <a name="enable-access-to-data-service-resources"></a>启用对数据服务资源的访问

1. 在数据服务的代码中，用下列代码替换 `InitializeService` 函数中的占位符代码：

     [!code-csharp[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#allreadconfig)]
     [!code-vb[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#allreadconfig)]

     这使得授权客户端能够对指定实体集的资源进行读写访问。

    > [!NOTE]
    > 任何可以访问该 ASP.NET 应用程序的客户端也能够访问由数据服务公开的资源。 在生产数据服务中，为防止对资源进行未经授权的访问，还应保护应用程序本身的安全。 有关更多信息，请参见 [Securing WCF Data Services](securing-wcf-data-services.md)。

## <a name="next-steps"></a>后续步骤

您已经成功地创建了一个公开基于 Northwind 示例数据库的 OData 源的新数据服务，并且您已为对 ASP.NET Web 应用程序具有权限的客户端启用了对源的访问。 接下来，将从 Visual Studio 启动数据服务，并通过 Web 浏览器提交 HTTP GET 请求来访问 OData 数据源：

> [!div class="nextstepaction"]
> [从 web 浏览器访问服务](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)

## <a name="see-also"></a>另请参阅

- [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
