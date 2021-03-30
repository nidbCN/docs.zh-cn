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
# <a name="wcf-dotnet-svcutil-tool-for-net-core"></a>.NET Core 的 WCF dotnet-svcutil 工具

Windows Communication Foundation (WCF) dotnet-svcutil 工具是一种 .NET 工具，此工具从网络位置上的 Web 服务中或从 WSDL 文件中检索元数据，并生成包含访问 Web 服务操作的客户端代理方法的 WCF 类。

类似于 .NET Framework 项目的[服务模型元数据 - svcutil](../../framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md) 工具，dotnet svcutil 是用于生成 Web 服务引用的命令行工具，与 .NET Core 和 .NET Standard 项目兼容。

dotnet-svcutil 工具是 [WCF Web 服务引用](wcf-web-service-reference-guide.md) Visual Studio 连接服务提供程序（随 Visual Studio 2017 版本 15.5 首次推出）的替代选项 。 dotnet-svcutil 工具作为一种 .NET 工具，可跨平台用于 Linux、macOS 和 Windows。

> [!IMPORTANT]
> 应仅从受信任源引用服务。 从不受信任的源添加引用可能会危及安全性。

## <a name="prerequisites"></a>先决条件

<!-- markdownlint-disable MD025 -->

# <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

- [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download) 或更高版本
- 你最喜欢的代码编辑器

# <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

- [.NET Core 1.0.4 SDK](https://dotnet.microsoft.com/download) 或更高版本
- 你最喜欢的代码编辑器

---

## <a name="getting-started"></a>入门

下面的示例将指导你完成将 Web 服务引用添加到 .NET Core Web 项目并调用该服务所需的步骤。 将创建名为“HelloSvcutil”的 .NET Core Web 应用程序，并将引用添加到实现以下协定的 Web 服务：

```csharp
[ServiceContract]
public interface ISayHello
{
    [OperationContract]
    string Hello(string name);
}
```

在此示例中，我们假定 Web 服务托管在以下地址中：`http://contoso.com/SayHello.svc`

从 Windows、macOS 或 Linux 命令窗口执行以下步骤：

1. 为项目创建一个名为“HelloSvcutil”的目录，并将其设置为当前目录，如以下示例所示：

    ```console
    mkdir HelloSvcutil
    cd HelloSvcutil
    ```

2. 在该目录中使用 [`dotnet new`](../tools/dotnet-new.md) 命令创建新的 C# Web 项目，如下所示：

    ```dotnetcli
    dotnet new web
    ```

3. 安装 [`dotnet-svcutil` NuGet 包](https://nuget.org/packages/dotnet-svcutil)作为 CLI 工具： <!-- markdownlint-disable MD023 -->
    # <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

    ```dotnetcli
    dotnet tool install --global dotnet-svcutil
    ```

    # <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

    在编辑器中打开 `HelloSvcutil.csproj` 项目文件，编辑 `Project` 元素，并使用下面的代码添加 [`dotnet-svcutil` NuGet 包](https://nuget.org/packages/dotnet-svcutil)作为 CLI 工具引用：

    ```xml
    <ItemGroup>
      <DotNetCliToolReference Include="dotnet-svcutil" Version="1.0.*" />
    </ItemGroup>
    ```

    然后使用 [`dotnet restore`](../tools/dotnet-restore.md) 命令还原 dotnet-svcutil 包，如下所示：

    ```dotnetcli
    dotnet restore
    ```

    ---

4. 运行 dotnet-svcutil 命令生成 Web 服务引用文件，如下所示：

    # <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

    ```dotnetcli
    dotnet-svcutil http://contoso.com/SayHello.svc
    ```

    # <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

    ```dotnetcli
    dotnet svcutil http://contoso.com/SayHello.svc
    ```

    ---

生成的文件保存为 _HelloSvcutil/ServiceReference/Reference.cs_。 dotnet-svcutil 工具还向项目添加代理代码所需的适当 WCF 包作为包引用。

## <a name="using-the-service-reference"></a>使用服务引用

1. 使用 [`dotnet restore`](../tools/dotnet-restore.md) 命令还原 WCF 包，如下所示：

    ```dotnetcli
    dotnet restore
    ```

2. 找到要使用的客户端类和操作的名称。 `Reference.cs` 将包含一个继承自 `System.ServiceModel.ClientBase` 的类，其方法可用于调用服务上的操作。 在本例中，想要调用 SayHello 服务的 Hello 操作。 `ServiceReference.SayHelloClient` 是客户端类的名称，它有一个名为 `HelloAsync` 的方法，可用于调用该操作。

3. 在编辑器中打开 `Startup.cs` 文件，并在顶部为服务引用命名空间添加一个 `using` 指令：

    ```csharp
    using ServiceReference;
    ```

4. 编辑 `Configure` 方法来调用 Web 服务。 为此，可以创建一个继承自 `ClientBase` 的类的实例，并在客户端对象上调用此方法：

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

5. 使用 [`dotnet run`](../tools/dotnet-run.md) 命令运行应用程序，如下所示：

    ```dotnetcli
    dotnet run
    ```

6. 导航到在 Web 浏览器的控制台中列出的 URL（例如，`http://localhost:5000`）。

您应看到以下输出：“Hello dotnet-svcutil!”

有关 `dotnet-svcutil` 工具参数的详细说明，请调用传递帮助参数的工具，如下所示：

# <a name="dotnet-svcutil-2x"></a>[dotnet-svcutil 2.x](#tab/dotnetsvcutil2x)

```dotnetcli
dotnet-svcutil --help
```

# <a name="dotnet-svcutil-1x"></a>[dotnet-svcutil 1.x](#tab/dotnetsvcutil1x)

```dotnetcli
dotnet svcutil --help
```

---

## <a name="feedback--questions"></a>反馈和问题

如果有任何问题或反馈，请[在 GitHub 上提问](https://github.com/dotnet/wcf/issues/new)。 也可以在 [GitHub 上的 WCF 存储库](https://github.com/dotnet/wcf/issues?utf8=%E2%9C%93&q=is:issue%20label:tooling)中查看任何现有问题。

## <a name="release-notes"></a>发行说明

- 请参阅[发行说明](https://github.com/dotnet/wcf/blob/main/release-notes/dotnet-svcutil-notes.md)，了解更新的版本信息（包括已知问题）。

## <a name="information"></a>信息

- [dotnet-svcutil NuGet 包](https://nuget.org/packages/dotnet-svcutil)
