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
# <a name="use-the-wcf-web-service-reference-provider-tool"></a><span data-ttu-id="a71ae-103">使用 WCF Web Service Reference Provider 工具</span><span class="sxs-lookup"><span data-stu-id="a71ae-103">Use the WCF Web Service Reference Provider Tool</span></span>

<span data-ttu-id="a71ae-104">多年来，许多 Visual Studio 开发者在其. NET Framework 项目需要访问 Web 服务时，都享受到了[添加服务引用](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)工具所带来的工作效率。</span><span class="sxs-lookup"><span data-stu-id="a71ae-104">Over the years, many Visual Studio developers have enjoyed the productivity that the [**Add Service Reference**](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference) tool provided when their .NET Framework projects needed to access web services.</span></span>  <span data-ttu-id="a71ae-105">WCF Web 服务引用工具是 Visual Studio 连接服务的扩展，提供了类似于 .NET Core 和 ASP.NET Core 项目的“添加服务引用”功能的体验  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-105">The **WCF Web Service Reference** tool is a Visual Studio connected service extension that provides an experience like the Add Service Reference functionality for .NET Core and ASP.NET Core projects.</span></span> <span data-ttu-id="a71ae-106">此工具可从网络位置的当前解决方案的 web 服务中或从 WSDL 文件中检索元数据，并生成包含可用于访问 web 服务的 Windows Communication Foundation (WCF) 客户端代理代码的可兼容 .NET Core 的源文件。</span><span class="sxs-lookup"><span data-stu-id="a71ae-106">This tool retrieves metadata from a web service in the current solution, on a network location, or from a WSDL file, and generates a .NET Core compatible source file containing Windows Communication Foundation (WCF) client proxy code that you can use to access the web service.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a71ae-107">应仅从受信任源引用服务。</span><span class="sxs-lookup"><span data-stu-id="a71ae-107">You should only reference services from a trusted source.</span></span> <span data-ttu-id="a71ae-108">从不受信任的源添加引用可能会危及安全性。</span><span class="sxs-lookup"><span data-stu-id="a71ae-108">Adding references from an untrusted source may compromise security.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a71ae-109">系统必备</span><span class="sxs-lookup"><span data-stu-id="a71ae-109">Prerequisites</span></span>

- <span data-ttu-id="a71ae-110">[Visual Studio 2017 版本 15.5](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="a71ae-110">[Visual Studio 2017 version 15.5](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) or later versions</span></span>

## <a name="how-to-use-the-extension"></a><span data-ttu-id="a71ae-111">如何使用扩展</span><span class="sxs-lookup"><span data-stu-id="a71ae-111">How to use the extension</span></span>

> [!NOTE]
> <span data-ttu-id="a71ae-112">“WCF Web 服务引用”选项适用于使用以下项目模板创建的项目  ：</span><span class="sxs-lookup"><span data-stu-id="a71ae-112">The **WCF Web Service Reference** option is applicable to projects created using the following project templates:</span></span>
>
> - <span data-ttu-id="a71ae-113">Visual C# > .NET Core</span><span class="sxs-lookup"><span data-stu-id="a71ae-113">**Visual C#** > **.NET Core**</span></span>
> - <span data-ttu-id="a71ae-114">Visual C# > .NET Standard</span><span class="sxs-lookup"><span data-stu-id="a71ae-114">**Visual C#** > **.NET Standard**</span></span>
> - <span data-ttu-id="a71ae-115">Visual C# > Web > ASP.NET Core Web 应用程序</span><span class="sxs-lookup"><span data-stu-id="a71ae-115">**Visual C#** > **Web** > **ASP.NET Core Web Application**</span></span>

<span data-ttu-id="a71ae-116">以“ASP.NET Core Web 应用程序”项目模板为例，本文将介绍如何向该项目中添加 WCF 服务引用  ：</span><span class="sxs-lookup"><span data-stu-id="a71ae-116">Using the **ASP.NET Core Web Application** project template as an example, this article walks you through adding a WCF service reference to the project:</span></span>

1. <span data-ttu-id="a71ae-117">在解决方案资源管理器中，双击项目的“连接的服务”节点（对于 .NET Core 或 .NET Standard 项目，当在解决方案资源管理器中右键单击项目的“依赖项”节点时，该选项可用）   。</span><span class="sxs-lookup"><span data-stu-id="a71ae-117">In Solution Explorer, double-click the **Connected Services** node of the project (for a .NET Core or .NET Standard project this option is available when you right-click on the **Dependencies** node of the project in Solution Explorer).</span></span>

    <span data-ttu-id="a71ae-118">随即显示“连接的服务”页，如下图所示  ：</span><span class="sxs-lookup"><span data-stu-id="a71ae-118">The **Connected Services** page appears as shown in the following image:</span></span>

    ![.NET Core 的“Visual Studio 连接的服务”选项卡](./media/wcf-web-service-reference-guide/wcfcs-ConnectedServicesPage.png)

2. <span data-ttu-id="a71ae-120">在“连接的服务”页上，单击“Microsoft WCF Web Service Reference Provider”   。</span><span class="sxs-lookup"><span data-stu-id="a71ae-120">On the **Connected Services** page, click **Microsoft WCF Web Service Reference Provider**.</span></span> <span data-ttu-id="a71ae-121">此操作将显示“配置 WCF Web 服务引用”向导  ：</span><span class="sxs-lookup"><span data-stu-id="a71ae-121">This brings up the **Configure WCF Web Service Reference** wizard:</span></span>

    ![.NET Core 的“Visual Studio 服务终结点”选项卡](./media/wcf-web-service-reference-guide/wcfcs-ServiceEndpointPage.png)

3. <span data-ttu-id="a71ae-123">选择服务。</span><span class="sxs-lookup"><span data-stu-id="a71ae-123">Select a service.</span></span>

    <span data-ttu-id="a71ae-124">3a.</span><span class="sxs-lookup"><span data-stu-id="a71ae-124">3a.</span></span> <span data-ttu-id="a71ae-125">“配置 WCF Web 服务引用”向导中提供了多个服务搜索选项  ：</span><span class="sxs-lookup"><span data-stu-id="a71ae-125">There are several services search options available within the **Configure WCF Web Service Reference** wizard:</span></span>

     * <span data-ttu-id="a71ae-126">要搜索当前解决方案中定义的服务，请单击“发现”按钮  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-126">To search for services defined in the current solution, click the **Discover** button.</span></span>
     * <span data-ttu-id="a71ae-127">要搜索在指定地址托管的服务，请在“地址”框中输入服务 URL，然后单击“转到”按钮   。</span><span class="sxs-lookup"><span data-stu-id="a71ae-127">To search for services hosted at a specified address, enter a service URL in the **Address** box and click the **Go** button.</span></span>
     * <span data-ttu-id="a71ae-128">要选择包含 Web 服务元数据信息的 WSDL 文件，请单击“浏览”按钮  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-128">To select a WSDL file that contains the web service metadata information, click the **Browse** button.</span></span>

    <span data-ttu-id="a71ae-129">3b.</span><span class="sxs-lookup"><span data-stu-id="a71ae-129">3b.</span></span> <span data-ttu-id="a71ae-130">从“服务”框内的搜索结果列表中选择服务  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-130">Select the service from the search results list in the **Services** box.</span></span> <span data-ttu-id="a71ae-131">如果需要，请在相应的“名称空间”文本框中为生成的代码输入命名空间  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-131">If needed, enter the namespace for the generated code in the corresponding **Namespace** text box.</span></span>

    <span data-ttu-id="a71ae-132">3c.</span><span class="sxs-lookup"><span data-stu-id="a71ae-132">3c.</span></span> <span data-ttu-id="a71ae-133">单击“下一步”按钮，打开“数据类型选项”页和“客户端选项”页    。</span><span class="sxs-lookup"><span data-stu-id="a71ae-133">Click the **Next** button to open the **Data Type Options** and the **Client Options** pages.</span></span> <span data-ttu-id="a71ae-134">或者，单击“完成”按钮，使用默认选项  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-134">Alternatively, click the **Finish** button to use the default options.</span></span>

4. <span data-ttu-id="a71ae-135">“数据类型选项”窗体可用于优化生成的服务引用配置设置：</span><span class="sxs-lookup"><span data-stu-id="a71ae-135">The **Data Type Options** form enables you to refine the generated service reference configuration settings:</span></span>

    ![.NET Core 的“Visual Studio 数据类型选项”选项卡](./media/wcf-web-service-reference-guide/wcfcs-DataTypesPage.png)

    > [!NOTE]
    > <span data-ttu-id="a71ae-137">如果在项目的引用程序集中定义了服务引用代码生成所需的数据类型，则“重新使用引用程序集中的类型”复选框选项将非常有用  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-137">The **Reuse types in referenced assemblies** check box option is useful when data types needed for service reference code generation are defined in one of your project's referenced assemblies.</span></span>  <span data-ttu-id="a71ae-138">重新使用这些现有数据类型，从而避免编译时类型冲突或运行时问题，这是非常重要的。</span><span class="sxs-lookup"><span data-stu-id="a71ae-138">It's important to reuse those existing data types to avoid compile-time type clash or runtime issues.</span></span>

    <span data-ttu-id="a71ae-139">加载类型信息时可能会有延迟，具体取决于项目依赖项和其他系统性能因素的数量。</span><span class="sxs-lookup"><span data-stu-id="a71ae-139">There may be a delay while type information is loaded, depending on the number of project dependencies and other system performance factors.</span></span> <span data-ttu-id="a71ae-140">加载过程中，“完成”按钮被禁用，除非未选中“重新使用引用程序集中的类型”复选框   。</span><span class="sxs-lookup"><span data-stu-id="a71ae-140">The **Finish** button is disabled during loading unless the **Reuse types in referenced assemblies** check box is unchecked.</span></span>

5. <span data-ttu-id="a71ae-141">完成后，单击“完成”  。</span><span class="sxs-lookup"><span data-stu-id="a71ae-141">Click **Finish** when you are done.</span></span>

<span data-ttu-id="a71ae-142">在显示进度的同时，工具：</span><span class="sxs-lookup"><span data-stu-id="a71ae-142">While displaying progress, the tool:</span></span>

- <span data-ttu-id="a71ae-143">从 WCF 服务下载元数据。</span><span class="sxs-lookup"><span data-stu-id="a71ae-143">Downloads metadata from the WCF service.</span></span>
- <span data-ttu-id="a71ae-144">在名为“reference.cs”的文件中生成服务引用代码，并将其添加到“连接的服务”节点下的项目   。</span><span class="sxs-lookup"><span data-stu-id="a71ae-144">Generates the service reference code in a file named *reference.cs*, and adds it to your project under the **Connected Services** node.</span></span>
- <span data-ttu-id="a71ae-145">使用在目标平台上编译和运行所需的 NuGet 包引用更新项目文件 (.csproj)。</span><span class="sxs-lookup"><span data-stu-id="a71ae-145">Updates the project file (.csproj) with NuGet package references required to compile and run on the target platform.</span></span>

![Visual Studio 进度窗口](./media/wcf-web-service-reference-guide/wcfcs-ProgressWindow.png)

<span data-ttu-id="a71ae-147">进度完成后，可创建生成的 WCF 客户端类型的实例并调用服务操作。</span><span class="sxs-lookup"><span data-stu-id="a71ae-147">When these processes complete, you can create an instance of the generated WCF client type and invoke the service operations.</span></span>

## <a name="see-also"></a><span data-ttu-id="a71ae-148">另请参阅</span><span class="sxs-lookup"><span data-stu-id="a71ae-148">See also</span></span>

- [<span data-ttu-id="a71ae-149">Windows Communication Foundation 应用程序入门</span><span class="sxs-lookup"><span data-stu-id="a71ae-149">Get started with Windows Communication Foundation applications</span></span>](../../framework/wcf/getting-started-tutorial.md)
- [<span data-ttu-id="a71ae-150">Visual Studio 中的 Windows Communication Foundation 服务和 WCF 数据服务</span><span class="sxs-lookup"><span data-stu-id="a71ae-150">Windows Communication Foundation services and WCF data services in Visual Studio</span></span>](/visualstudio/data-tools/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio)
- [<span data-ttu-id="a71ae-151">.NET Core 上 WCF 支持的功能</span><span class="sxs-lookup"><span data-stu-id="a71ae-151">WCF supported features on .NET Core</span></span>](https://github.com/dotnet/wcf/blob/main/release-notes/SupportedFeatures-v2.1.0.md)

## <a name="feedback--questions"></a><span data-ttu-id="a71ae-152">反馈和问题</span><span class="sxs-lookup"><span data-stu-id="a71ae-152">Feedback & questions</span></span>

<span data-ttu-id="a71ae-153">如果你有任何产品反馈，请使用[报告问题](/visualstudio/ide/how-to-report-a-problem-with-visual-studio)工具在[开发者社区](https://aka.ms/feedback/report?space=61)进行报告。</span><span class="sxs-lookup"><span data-stu-id="a71ae-153">If you have any product feedback, report it at [Developer Community](https://aka.ms/feedback/report?space=61) using the [Report a problem](/visualstudio/ide/how-to-report-a-problem-with-visual-studio) tool.</span></span>

## <a name="release-notes"></a><span data-ttu-id="a71ae-154">发行说明</span><span class="sxs-lookup"><span data-stu-id="a71ae-154">Release notes</span></span>

- <span data-ttu-id="a71ae-155">请参阅[发行说明](https://github.com/dotnet/wcf/blob/main/release-notes/WCF-Web-Service-Reference-notes.md)，了解更新的版本信息（包括已知问题）。</span><span class="sxs-lookup"><span data-stu-id="a71ae-155">Refer to the [Release notes](https://github.com/dotnet/wcf/blob/main/release-notes/WCF-Web-Service-Reference-notes.md) for updated release information, including known issues.</span></span>
