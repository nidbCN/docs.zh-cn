---
title: WCF svcutil 工具概述
description: Microsoft WCF dotnet-svcutil 工具概述，该工具添加了 .NET Core 和 ASP.NET Core 项目的功能，类似于 .NET Framework 项目的 WCF svcutil 工具。
author: honggit
ms.date: 02/22/2019
ms.openlocfilehash: 6af04c4fb5e824ffce594b6e21b720246c67bf66
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873024"
---
# <a name="wcf-dotnet-svcutil-tool-for-net-core"></a><span data-ttu-id="40169-103">.NET Core 的 WCF dotnet-svcutil 工具</span><span class="sxs-lookup"><span data-stu-id="40169-103">WCF dotnet-svcutil tool for .NET Core</span></span>

<span data-ttu-id="40169-104">Windows Communication Foundation (WCF) dotnet-svcutil 工具是一种 .NET 工具，此工具从网络位置上的 Web 服务中或从 WSDL 文件中检索元数据，并生成包含访问 Web 服务操作的客户端代理方法的 WCF 类。</span><span class="sxs-lookup"><span data-stu-id="40169-104">The Windows Communication Foundation (WCF) **dotnet-svcutil** tool is a .NET tool that retrieves metadata from a web service on a network location or from a WSDL file, and generates a WCF class containing client proxy methods that access the web service operations.</span></span>

<span data-ttu-id="40169-105">类似于 .NET Framework 项目的[服务模型元数据 - svcutil](../../framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) 工具，dotnet svcutil 是用于生成 Web 服务引用的命令行工具，与 .NET Core 和 .NET Standard 项目兼容。</span><span class="sxs-lookup"><span data-stu-id="40169-105">Similar to the [**Service Model Metadata - svcutil**](../../framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) tool for .NET Framework projects, the **dotnet-svcutil** is a command-line tool for generating a web service reference compatible with .NET Core and .NET Standard projects.</span></span>

<span data-ttu-id="40169-106">dotnet-svcutil 工具是 [WCF Web 服务引用](wcf-web-service-reference-guide.md) Visual Studio 连接服务提供程序（随 Visual Studio 2017 版本 15.5 首次推出）的替代选项 。</span><span class="sxs-lookup"><span data-stu-id="40169-106">The **dotnet-svcutil** tool is an alternative option to the [**WCF Web Service Reference**](wcf-web-service-reference-guide.md) Visual Studio connected service provider that first shipped with Visual Studio 2017 version 15.5.</span></span> <span data-ttu-id="40169-107">dotnet-svcutil 工具作为一种 .NET 工具，可跨平台用于 Linux、macOS 和 Windows。</span><span class="sxs-lookup"><span data-stu-id="40169-107">The **dotnet-svcutil** tool as a .NET tool, is available cross-platform on Linux, macOS, and Windows.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="40169-108">应仅从受信任源引用服务。</span><span class="sxs-lookup"><span data-stu-id="40169-108">You should only reference services from a trusted source.</span></span> <span data-ttu-id="40169-109">从不受信任的源添加引用可能会危及安全性。</span><span class="sxs-lookup"><span data-stu-id="40169-109">Adding references from an untrusted source may compromise security.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40169-110">先决条件</span><span class="sxs-lookup"><span data-stu-id="40169-110">Prerequisites</span></span>

<!-- markdownlint-disable MD025 -->

# <a name="dotnet-svcutil-2x"></a>[<span data-ttu-id="40169-111">dotnet-svcutil 2.x</span><span class="sxs-lookup"><span data-stu-id="40169-111">dotnet-svcutil 2.x</span></span>](#tab/dotnetsvcutil2x)

- <span data-ttu-id="40169-112">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="40169-112">[.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) or later versions</span></span>
- <span data-ttu-id="40169-113">你最喜欢的代码编辑器</span><span class="sxs-lookup"><span data-stu-id="40169-113">Your favorite code editor</span></span>

# <a name="dotnet-svcutil-1x"></a>[<span data-ttu-id="40169-114">dotnet-svcutil 1.x</span><span class="sxs-lookup"><span data-stu-id="40169-114">dotnet-svcutil 1.x</span></span>](#tab/dotnetsvcutil1x)

- <span data-ttu-id="40169-115">[.NET Core 1.0.4 SDK](https://dotnet.microsoft.com/download) 或更高版本</span><span class="sxs-lookup"><span data-stu-id="40169-115">[.NET Core 1.0.4 SDK](https://dotnet.microsoft.com/download) or later versions</span></span>
- <span data-ttu-id="40169-116">你最喜欢的代码编辑器</span><span class="sxs-lookup"><span data-stu-id="40169-116">Your favorite code editor</span></span>

---

## <a name="getting-started"></a><span data-ttu-id="40169-117">入门</span><span class="sxs-lookup"><span data-stu-id="40169-117">Getting started</span></span>

<span data-ttu-id="40169-118">下面的示例将指导你完成将 Web 服务引用添加到 .NET Core Web 项目并调用该服务所需的步骤。</span><span class="sxs-lookup"><span data-stu-id="40169-118">The following example walks you through the steps required to add a web service reference to a .NET Core web project and invoke the service.</span></span> <span data-ttu-id="40169-119">将创建名为“HelloSvcutil”的 .NET Core Web 应用程序，并将引用添加到实现以下协定的 Web 服务：</span><span class="sxs-lookup"><span data-stu-id="40169-119">You'll create a .NET Core web application named *HelloSvcutil* and add a reference to a web service that implements the following contract:</span></span>

```csharp
[ServiceContract]
public interface ISayHello
{
    [OperationContract]
    string Hello(string name);
}
```

<span data-ttu-id="40169-120">在此示例中，我们假定 Web 服务托管在以下地址中：`http://contoso.com/SayHello.svc`</span><span class="sxs-lookup"><span data-stu-id="40169-120">For this example, let's assume the web service will be hosted at the following address: `http://contoso.com/SayHello.svc`</span></span>

<span data-ttu-id="40169-121">从 Windows、macOS 或 Linux 命令窗口执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="40169-121">From a Windows, macOS, or Linux command window perform the following steps:</span></span>

1. <span data-ttu-id="40169-122">为项目创建一个名为“HelloSvcutil”的目录，并将其设置为当前目录，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="40169-122">Create a directory named _HelloSvcutil_ for your project and make it your current directory, as in the following example:</span></span>

    ```console
    mkdir HelloSvcutil
    cd HelloSvcutil
    ```

2. <span data-ttu-id="40169-123">在该目录中使用 [`dotnet new`](../tools/dotnet-new.md) 命令创建新的 C# Web 项目，如下所示：</span><span class="sxs-lookup"><span data-stu-id="40169-123">Create a new C# web project in that directory using the [`dotnet new`](../tools/dotnet-new.md) command as follows:</span></span>

    ```dotnetcli
    dotnet new web
    ```

3. <span data-ttu-id="40169-124">安装 [`dotnet-svcutil` NuGet 包](https://nuget.org/packages/dotnet-svcutil)作为 CLI 工具： </span><span class="sxs-lookup"><span data-stu-id="40169-124">Install the [`dotnet-svcutil` NuGet package](https://nuget.org/packages/dotnet-svcutil) as a CLI tool:  </span></span><!-- markdownlint-disable MD023 -->
    # <a name="dotnet-svcutil-2x"></a>[<span data-ttu-id="40169-125">dotnet-svcutil 2.x</span><span class="sxs-lookup"><span data-stu-id="40169-125">dotnet-svcutil 2.x</span></span>](#tab/dotnetsvcutil2x)

    ```dotnetcli
    dotnet tool install --global dotnet-svcutil
    ```

    # <a name="dotnet-svcutil-1x"></a>[<span data-ttu-id="40169-126">dotnet-svcutil 1.x</span><span class="sxs-lookup"><span data-stu-id="40169-126">dotnet-svcutil 1.x</span></span>](#tab/dotnetsvcutil1x)

    <span data-ttu-id="40169-127">在编辑器中打开 `HelloSvcutil.csproj` 项目文件，编辑 `Project` 元素，并使用下面的代码添加 [`dotnet-svcutil` NuGet 包](https://nuget.org/packages/dotnet-svcutil)作为 CLI 工具引用：</span><span class="sxs-lookup"><span data-stu-id="40169-127">Open the `HelloSvcutil.csproj` project file in your editor, edit the `Project` element, and add the [`dotnet-svcutil` NuGet package](https://nuget.org/packages/dotnet-svcutil) as a CLI tool reference, using the following code:</span></span>

    ```xml
    <ItemGroup>
      <DotNetCliToolReference Include="dotnet-svcutil" Version="1.0.*" />
    </ItemGroup>
    ```

    <span data-ttu-id="40169-128">然后使用 [`dotnet restore`](../tools/dotnet-restore.md) 命令还原 dotnet-svcutil 包，如下所示：</span><span class="sxs-lookup"><span data-stu-id="40169-128">Then restore the _dotnet-svcutil_ package using the [`dotnet restore`](../tools/dotnet-restore.md) command as follows:</span></span>

    ```dotnetcli
    dotnet restore
    ```

    ---

4. <span data-ttu-id="40169-129">运行 dotnet-svcutil 命令生成 Web 服务引用文件，如下所示：</span><span class="sxs-lookup"><span data-stu-id="40169-129">Run the _dotnet-svcutil_ command to generate the web service reference file as follows:</span></span>

    # <a name="dotnet-svcutil-2x"></a>[<span data-ttu-id="40169-130">dotnet-svcutil 2.x</span><span class="sxs-lookup"><span data-stu-id="40169-130">dotnet-svcutil 2.x</span></span>](#tab/dotnetsvcutil2x)

    ```dotnetcli
    dotnet-svcutil http://contoso.com/SayHello.svc
    ```

    # <a name="dotnet-svcutil-1x"></a>[<span data-ttu-id="40169-131">dotnet-svcutil 1.x</span><span class="sxs-lookup"><span data-stu-id="40169-131">dotnet-svcutil 1.x</span></span>](#tab/dotnetsvcutil1x)

    ```dotnetcli
    dotnet svcutil http://contoso.com/SayHello.svc
    ```

    ---

<span data-ttu-id="40169-132">生成的文件保存为 _HelloSvcutil/ServiceReference/Reference.cs_。</span><span class="sxs-lookup"><span data-stu-id="40169-132">The generated file is saved as _HelloSvcutil/ServiceReference/Reference.cs_.</span></span> <span data-ttu-id="40169-133">dotnet-svcutil 工具还向项目添加代理代码所需的适当 WCF 包作为包引用。</span><span class="sxs-lookup"><span data-stu-id="40169-133">The _dotnet-svcutil_ tool also adds to the project the appropriate WCF packages required by the proxy code as package references.</span></span>

## <a name="using-the-service-reference"></a><span data-ttu-id="40169-134">使用服务引用</span><span class="sxs-lookup"><span data-stu-id="40169-134">Using the Service Reference</span></span>

1. <span data-ttu-id="40169-135">使用 [`dotnet restore`](../tools/dotnet-restore.md) 命令还原 WCF 包，如下所示：</span><span class="sxs-lookup"><span data-stu-id="40169-135">Restore the WCF packages using the [`dotnet restore`](../tools/dotnet-restore.md) command as follows:</span></span>

    ```dotnetcli
    dotnet restore
    ```

2. <span data-ttu-id="40169-136">找到要使用的客户端类和操作的名称。</span><span class="sxs-lookup"><span data-stu-id="40169-136">Find the name of the client class and operation you want to use.</span></span> <span data-ttu-id="40169-137">`Reference.cs` 将包含一个继承自 `System.ServiceModel.ClientBase` 的类，其方法可用于调用服务上的操作。</span><span class="sxs-lookup"><span data-stu-id="40169-137">`Reference.cs` will contain a class that inherits from `System.ServiceModel.ClientBase`, with methods that can be used to call operations on the service.</span></span> <span data-ttu-id="40169-138">在本例中，想要调用 SayHello 服务的 Hello 操作。</span><span class="sxs-lookup"><span data-stu-id="40169-138">In this example, you want to call the _SayHello_ service's _Hello_ operation.</span></span> <span data-ttu-id="40169-139">`ServiceReference.SayHelloClient` 是客户端类的名称，它有一个名为 `HelloAsync` 的方法，可用于调用该操作。</span><span class="sxs-lookup"><span data-stu-id="40169-139">`ServiceReference.SayHelloClient` is the name of the client class, and has a method called `HelloAsync` that can be used to call the operation.</span></span>

3. <span data-ttu-id="40169-140">在编辑器中打开 `Startup.cs` 文件，并在顶部为服务引用命名空间添加一个 `using` 指令：</span><span class="sxs-lookup"><span data-stu-id="40169-140">Open the `Startup.cs` file in your editor, and add a `using` directive for the service reference namespace at the top:</span></span>

    ```csharp
    using ServiceReference;
    ```

4. <span data-ttu-id="40169-141">编辑 `Configure` 方法来调用 Web 服务。</span><span class="sxs-lookup"><span data-stu-id="40169-141">Edit the `Configure` method to invoke the web service.</span></span> <span data-ttu-id="40169-142">为此，可以创建一个继承自 `ClientBase` 的类的实例，并在客户端对象上调用此方法：</span><span class="sxs-lookup"><span data-stu-id="40169-142">You do this by creating an instance of the class that inherits from `ClientBase` and calling the method on the client object:</span></span>

    ```csharp
    public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.Run(async (context) =>
        {
            var client = new SayHelloClient();
            var response = await client.HelloAsync();
            await context.Response.WriteAsync(response);
        });
    }

    ```

5. <span data-ttu-id="40169-143">使用 [`dotnet run`](../tools/dotnet-run.md) 命令运行应用程序，如下所示：</span><span class="sxs-lookup"><span data-stu-id="40169-143">Run the application using the [`dotnet run`](../tools/dotnet-run.md) command as follows:</span></span>

    ```dotnetcli
    dotnet run
    ```

6. <span data-ttu-id="40169-144">导航到在 Web 浏览器的控制台中列出的 URL（例如，`http://localhost:5000`）。</span><span class="sxs-lookup"><span data-stu-id="40169-144">Navigate to the URL listed in the console (for example, `http://localhost:5000`) in your web browser.</span></span>

<span data-ttu-id="40169-145">您应看到以下输出：“Hello dotnet-svcutil!”</span><span class="sxs-lookup"><span data-stu-id="40169-145">You should see the following output: "Hello dotnet-svcutil!"</span></span>

<span data-ttu-id="40169-146">有关 `dotnet-svcutil` 工具参数的详细说明，请调用传递帮助参数的工具，如下所示：</span><span class="sxs-lookup"><span data-stu-id="40169-146">For a detailed description of the `dotnet-svcutil` tool parameters, invoke the tool passing the help parameter as follows:</span></span>

# <a name="dotnet-svcutil-2x"></a>[<span data-ttu-id="40169-147">dotnet-svcutil 2.x</span><span class="sxs-lookup"><span data-stu-id="40169-147">dotnet-svcutil 2.x</span></span>](#tab/dotnetsvcutil2x)

```dotnetcli
dotnet-svcutil --help
```

# <a name="dotnet-svcutil-1x"></a>[<span data-ttu-id="40169-148">dotnet-svcutil 1.x</span><span class="sxs-lookup"><span data-stu-id="40169-148">dotnet-svcutil 1.x</span></span>](#tab/dotnetsvcutil1x)

```dotnetcli
dotnet svcutil --help
```

---

## <a name="feedback--questions"></a><span data-ttu-id="40169-149">反馈和问题</span><span class="sxs-lookup"><span data-stu-id="40169-149">Feedback & questions</span></span>

<span data-ttu-id="40169-150">如果有任何问题或反馈，请[在 GitHub 上提问](https://github.com/dotnet/wcf/issues/new)。</span><span class="sxs-lookup"><span data-stu-id="40169-150">If you have any questions or feedback, [open an issue on GitHub](https://github.com/dotnet/wcf/issues/new).</span></span> <span data-ttu-id="40169-151">也可以在 [GitHub 上的 WCF 存储库](https://github.com/dotnet/wcf/issues?utf8=%E2%9C%93&q=is:issue%20label:tooling)中查看任何现有问题。</span><span class="sxs-lookup"><span data-stu-id="40169-151">You can also review any existing questions or issues [at the WCF repo on GitHub](https://github.com/dotnet/wcf/issues?utf8=%E2%9C%93&q=is:issue%20label:tooling).</span></span>

## <a name="release-notes"></a><span data-ttu-id="40169-152">发行说明</span><span class="sxs-lookup"><span data-stu-id="40169-152">Release notes</span></span>

- <span data-ttu-id="40169-153">请参阅[发行说明](https://github.com/dotnet/wcf/blob/main/release-notes/dotnet-svcutil-notes.md)，了解更新的版本信息（包括已知问题）。</span><span class="sxs-lookup"><span data-stu-id="40169-153">Refer to the [Release notes](https://github.com/dotnet/wcf/blob/main/release-notes/dotnet-svcutil-notes.md) for updated release information, including known issues.</span></span>

## <a name="information"></a><span data-ttu-id="40169-154">信息</span><span class="sxs-lookup"><span data-stu-id="40169-154">Information</span></span>

- [<span data-ttu-id="40169-155">dotnet-svcutil NuGet 包</span><span class="sxs-lookup"><span data-stu-id="40169-155">dotnet-svcutil NuGet Package</span></span>](https://nuget.org/packages/dotnet-svcutil)
