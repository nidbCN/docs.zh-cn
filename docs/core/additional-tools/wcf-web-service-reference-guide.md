---
title: 添加 WCF Web 服务引用
description: Microsoft WCF Web Service Reference Provider 工具概述，该工具添加了 .NET Core 和 ASP.NET Core 项目的功能，类似于 .NET Framework 项目的添加服务引用。
author: dasetser
ms.date: 10/29/2019
ms.custom: mvc
ms.openlocfilehash: fcb7695c904e80d3fcb5ea68cba7ea198b3905f3
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872972"
---
# <a name="use-the-wcf-web-service-reference-provider-tool"></a>使用 WCF Web Service Reference Provider 工具

多年来，许多 Visual Studio 开发者在其. NET Framework 项目需要访问 Web 服务时，都享受到了[添加服务引用](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)工具所带来的工作效率。  WCF Web 服务引用工具是 Visual Studio 连接服务的扩展，提供了类似于 .NET Core 和 ASP.NET Core 项目的“添加服务引用”功能的体验  。 此工具可从网络位置的当前解决方案的 web 服务中或从 WSDL 文件中检索元数据，并生成包含可用于访问 web 服务的 Windows Communication Foundation (WCF) 客户端代理代码的可兼容 .NET Core 的源文件。

> [!IMPORTANT]
> 应仅从受信任源引用服务。 从不受信任的源添加引用可能会危及安全性。

## <a name="prerequisites"></a>系统必备

- [Visual Studio 2017 版本 15.5](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) 或更高版本

## <a name="how-to-use-the-extension"></a>如何使用扩展

> [!NOTE]
> “WCF Web 服务引用”选项适用于使用以下项目模板创建的项目  ：
>
> - Visual C# > .NET Core
> - Visual C# > .NET Standard
> - Visual C# > Web > ASP.NET Core Web 应用程序

以“ASP.NET Core Web 应用程序”项目模板为例，本文将介绍如何向该项目中添加 WCF 服务引用  ：

1. 在解决方案资源管理器中，双击项目的“连接的服务”节点（对于 .NET Core 或 .NET Standard 项目，当在解决方案资源管理器中右键单击项目的“依赖项”节点时，该选项可用）   。

    随即显示“连接的服务”页，如下图所示  ：

    ![.NET Core 的“Visual Studio 连接的服务”选项卡](./media/wcf-web-service-reference-guide/wcfcs-ConnectedServicesPage.png)

2. 在“连接的服务”页上，单击“Microsoft WCF Web Service Reference Provider”   。 此操作将显示“配置 WCF Web 服务引用”向导  ：

    ![.NET Core 的“Visual Studio 服务终结点”选项卡](./media/wcf-web-service-reference-guide/wcfcs-ServiceEndpointPage.png)

3. 选择服务。

    3a. “配置 WCF Web 服务引用”向导中提供了多个服务搜索选项  ：

     * 要搜索当前解决方案中定义的服务，请单击“发现”按钮  。
     * 要搜索在指定地址托管的服务，请在“地址”框中输入服务 URL，然后单击“转到”按钮   。
     * 要选择包含 Web 服务元数据信息的 WSDL 文件，请单击“浏览”按钮  。

    3b. 从“服务”框内的搜索结果列表中选择服务  。 如果需要，请在相应的“名称空间”文本框中为生成的代码输入命名空间  。

    3c. 单击“下一步”按钮，打开“数据类型选项”页和“客户端选项”页    。 或者，单击“完成”按钮，使用默认选项  。

4. “数据类型选项”窗体可用于优化生成的服务引用配置设置：

    ![.NET Core 的“Visual Studio 数据类型选项”选项卡](./media/wcf-web-service-reference-guide/wcfcs-DataTypesPage.png)

    > [!NOTE]
    > 如果在项目的引用程序集中定义了服务引用代码生成所需的数据类型，则“重新使用引用程序集中的类型”复选框选项将非常有用  。  重新使用这些现有数据类型，从而避免编译时类型冲突或运行时问题，这是非常重要的。

    加载类型信息时可能会有延迟，具体取决于项目依赖项和其他系统性能因素的数量。 加载过程中，“完成”按钮被禁用，除非未选中“重新使用引用程序集中的类型”复选框   。

5. 完成后，单击“完成”  。

在显示进度的同时，工具：

- 从 WCF 服务下载元数据。
- 在名为“reference.cs”的文件中生成服务引用代码，并将其添加到“连接的服务”节点下的项目   。
- 使用在目标平台上编译和运行所需的 NuGet 包引用更新项目文件 (.csproj)。

![Visual Studio 进度窗口](./media/wcf-web-service-reference-guide/wcfcs-ProgressWindow.png)

进度完成后，可创建生成的 WCF 客户端类型的实例并调用服务操作。

## <a name="see-also"></a>另请参阅

- [Windows Communication Foundation 应用程序入门](../../framework/wcf/getting-started-tutorial.md)
- [Visual Studio 中的 Windows Communication Foundation 服务和 WCF 数据服务](/visualstudio/data-tools/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio)
- [.NET Core 上 WCF 支持的功能](https://github.com/dotnet/wcf/blob/main/release-notes/SupportedFeatures-v2.1.0.md)

## <a name="feedback--questions"></a>反馈和问题

如果你有任何产品反馈，请使用[报告问题](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)工具在[开发者社区](https://aka.ms/feedback/report?space=61)进行报告。

## <a name="release-notes"></a>发行说明

- 请参阅[发行说明](https://github.com/dotnet/wcf/blob/main/release-notes/WCF-Web-Service-Reference-notes.md)，了解更新的版本信息（包括已知问题）。
