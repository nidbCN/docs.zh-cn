---
title: Dapr 入门
description: 用于准备本地开发环境和使用 Dapr 构建你的第一个 .NET 应用程序的指南。
author: amolenk
ms.date: 02/25/2021
ms.openlocfilehash: 1c60f731138911d7d22ff871c9a3849704d81dd6
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104874376"
---
# <a name="get-started-with-dapr"></a>Dapr 入门

前两章介绍了有关 Dapr 的基本概念。 现在可以将其用于 *测试驱动器*。 本章将指导你完成本地开发环境的准备工作，并构建两个 Dapr .NET 应用程序。

## <a name="install-dapr-into-your-local-environment"></a>将 Dapr 安装到本地环境中

首先，在开发计算机上安装 Dapr。 完成后，可以在 [自承载模式下](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-overview/)生成并运行 Dapr 应用程序。

1. [安装 DAPR CLI](https://docs.dapr.io/getting-started/install-dapr-cli/)。 它使您可以启动、运行和管理 Dapr 实例。 它还提供调试支持。

1. 安装 [Docker Desktop](https://docs.docker.com/get-docker/)。 如果在 Windows 上运行，请确保将 **适用于 windows 的 Docker Desktop** 配置为使用 Linux 容器。

> [!NOTE]
> 默认情况下，Dapr 使用 Docker 容器为你提供最佳的全新体验。 若要在 Docker 外部运行 Dapr，可以跳过此步骤并 [执行 *超薄* 初始化](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/)。 本章中的示例要求使用 Docker 容器。

1. [初始化 Dapr](https://docs.dapr.io/getting-started/install-dapr/)。 此步骤通过安装最新的 Dapr 二进制文件和容器映像来设置开发环境。

1. 安装 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1)。

安装 Dapr 后，就可以构建你的第一个 Dapr 应用程序了！

## <a name="build-your-first-dapr-application"></a>构建你的第一个 Dapr 应用程序

首先构建一个使用 [Dapr 状态管理](state-management.md) 构建块的简单 .net 控制台应用程序。

### <a name="create-the-application"></a>创建应用程序

1. 打开所选的命令行界面或终端。 您可以考虑 [Visual Studio Code](https://code.visualstudio.com/)中的终端功能。 导航到要在其中生成应用程序的根文件夹。 完成后，输入以下命令以创建新的 .NET 控制台应用程序：

    ```dotnetcli
    dotnet new console -o DaprCounter
    ```

    命令基架简单的 "Hello World" .NET Core 应用程序。

1. 然后，导航到上一个命令所创建的新目录中：

    ```console
    cd DaprCounter
    ```

1. 使用命令运行新创建的应用程序 `dotnet run` 。 这样做会写入 "Hello World！" 到控制台屏幕：

    ```dotnetcli
    dotnet run
    ```

### <a name="add-dapr-state-management"></a>添加 Dapr 状态管理

接下来，你将使用 Dapr [状态管理构建基块](https://docs.dapr.io/developing-applications/building-blocks/state-management/state-management-overview/) 在程序中实现有状态计数器。

可以使用 Dapr 对 HTTP 和 gRPC 的本机支持，在任何开发平台上调用 Dapr Api。 然而，.NET 开发人员会发现 Dapr .NET SDK 更自然且更直观。 它提供强类型的 .NET 客户端来调用 Dapr Api。 .NET SDK 还与 ASP.NET Core 紧密集成。

1. 在终端窗口中，将 `Dapr.Client` NuGet 包添加到应用程序：

    ```dotnetcli
    dotnet add package Dapr.Client
    ```

    > [!NOTE]
    > 如果使用的是预发行版本的 Dapr，请确保将该 `--prerelease` 标志添加到命令中。

1. `Program.cs`在偏好的编辑器中打开文件，并将其内容更新为以下代码：

    ```csharp
    using System;
    using System.Threading.Tasks;
    using Dapr.Client;

    namespace DaprCounter
    {
        class Program
        {
            static async Task Main(string[] args)
            {
                const string storeName = "statestore";
                const string key = "counter";

                var daprClient = new DaprClientBuilder().Build();
                var counter = await daprClient.GetStateAsync<int>(storeName, key);

                while (true)
                {
                    Console.WriteLine($"Counter = {counter++}");

                    await daprClient.SaveStateAsync(storeName, key, counter);
                    await Task.Delay(1000);
                }
            }
        }
    }
    ```

    更新的代码实现以下步骤：

    - 首先 `DaprClient` 实例化一个新实例。 此类使你可以与 Dapr 挎斗交互。
    - 从状态存储中 `DaprClient.GetStateAsync` 获取密钥的值 `counter` 。 如果该键不存在，则 `int` 返回)  (默认值 `0` 。
    - 然后，代码将循环访问，将 `counter` 值写入控制台，并将递增的值保存到状态存储中。

1. Dapr CLI `run` 命令启动该应用程序。 它调用基础 Dapr 运行时，并使应用程序和 Dapr 挎斗一起运行。 如果省略，则 `app-id` Dapr 将为应用程序生成唯一的名称。 命令的最后一段 `dotnet run` 指示 Dapr 运行时运行 .net core 应用程序。

    > [!IMPORTANT]
    > `app-id`使用状态管理构建块时，必须注意始终传递显式参数。 对于每个键/值对，块使用应用程序 id 值作为其状态键的 *前缀* 。 如果应用程序 id 发生更改，则无法再访问以前存储的状态。

    现在，用以下命令运行应用程序：

    ```console
    dapr run --app-id DaprCounter dotnet run
    ```

    尝试停止并重新启动应用程序。 你将看到计数器未重置。 相反，它会从以前保存的状态继续。 Dapr 构建基块会使应用程序具有状态。

> [!IMPORTANT]
> 务必了解你的示例应用程序与预配置状态组件的通信，但没有直接依赖它。 Dapr 提取依赖关系。 正如您很快会看到的，可以使用简单的配置更新来更改基础状态存储组件。

您可能想知道在哪里存储状态？

## <a name="component-configuration-files"></a>组件配置文件

首次为本地环境初始化 Dapr 时，它会自动预配 Redis 容器。 然后，Dapr 将 Redis 容器配置为包含组件配置文件的默认状态存储组件（标题为） `statestore.yaml` 。 下面介绍了其内容：

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
```

> [!NOTE]
> 默认组件配置文件存储在 `$HOME/.dapr/components` Linux/macOS 的文件夹中，并存储在 `%USERPROFILE%\.dapr\components` Windows 上的文件夹中。

请注意上一个组件配置文件的格式：

- 每个组件都有一个名称。 在上面的示例中，该组件的名称为 `statestore` 。 我们在第一个代码示例中使用了该名称，告诉 Dapr 挎斗要使用哪个组件。
- 每个组件配置文件都有一个 `spec` 节。 它包含 `type` 指定组件类型的字段。 `version`字段指定组件版本。 `metadata`字段包含组件需要的信息，例如连接详细信息和其他设置。 不同类型的组件的元数据值会有所不同。

Dapr 挎斗可以使用在应用程序中配置的任何 Dapr 组件。 但是，如果您的体系结构理由限制了组件的可访问性，会怎样呢？ 如何将 Redis 组件限制为仅在生产环境中运行的 Dapr 分支？

为此，可以 `namespace` 为生产环境定义。 可以将其命名为 `production` 。 在自承载模式下，通过设置环境变量来指定 Dapr 挎斗的命名空间 `NAMESPACE` 。 配置后，Dapr 挎斗将仅加载与该命名空间匹配的组件。 对于 Kubernetes 部署，Kubernetes 命名空间确定加载的组件。 下面的示例演示命名空间中的 Redis 组件 `production` 。 请注意 `namespace` 元素中的声明 `metadata` ：

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: production
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
```

> [!IMPORTANT]
> 只有在同一命名空间中运行的应用程序才能访问带命名空间组件。 如果 Dapr 应用程序无法加载组件，请确保应用程序命名空间与组件命名空间相匹配。 在自承载模式下，这在应用程序命名空间存储于环境变量中的情况下尤其重要 `NAMESPACE` 。

如果需要，可以进一步将组件限制到特定的应用程序。 在 `production` 命名空间中，你可能需要将 Redis 缓存的访问限制为仅限 `DaprCounter` 应用程序。 为此，请 `scopes` 在组件配置中指定。 下面的示例演示如何将对 Redis 组件的访问限制 `statestore` 到 `DaprCounter` 命名空间中的应用程序 `production` ：

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: statestore
  namespace: production
spec:
  type: state.redis
  version: v1
  metadata:
  - name: redisHost
    value: localhost:6379
  - name: redisPassword
    value: ""
  - name: actorStateStore
    value: "true"
  scopes:
  - DaprCounter
```

## <a name="build-a-multi-container-dapr-application"></a>构建多容器 Dapr 应用程序

在第一个示例中，创建了一个与 Dapr 挎斗并行运行的简单 .NET 控制台应用程序。 不过，新式分布式应用程序通常由许多移动部件组成。 它们可以同时运行独立微服务。 这些新式应用程序通常容器化，并需要容器业务流程工具，例如 Docker Compose 或 Kubernetes。

在下一个示例中，将创建多容器应用程序。 你还将使用 [Dapr 服务调用](service-invocation.md) 构建基块在服务之间进行通信。 解决方案将包含一个 web 应用程序，该应用程序从 web API 中检索天气预报。 它们每个都在 Docker 容器中运行。 你将使用 Docker Compose 在本地运行容器并启用调试功能。

请确保已为 Dapr 配置了本地环境并安装了 [.Net Core 3.1 开发工具](https://dotnet.microsoft.com/download/dotnet-core/3.1) (说明可在本章开头) 。

此外，还需要在安装了 **.Net Core 跨平台开发** 工作负载的情况下，使用 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads)完成此示例。

### <a name="create-the-application"></a>创建应用程序

1. 在 Visual Studio 2019 中创建 **ASP.NET Core Web 应用** 项目：

    :::image type="content" source="./media/getting-started/multicontainer-createproject.png" alt-text="创建新项目的屏幕截图":::

1. 命名项目 `DaprFrontEnd` 和解决方案 `DaprMultiContainer` ：

    :::image type="content" source="./media/getting-started/multicontainer-configureproject.png" alt-text="配置新项目的屏幕截图":::

1. 选择“Web 应用程序”，以创建使用 Razor Pages 的 Web 应用程序  。 不要选择 " **启用 Docker 支持**"。 稍后添加 Docker 支持。

    :::image type="content" source="./media/getting-started/multicontainer-createwebapp.png" alt-text="创建新 ASP.NET Core web 应用程序的屏幕截图":::

1. 将 ASP.NET Core Web API 项目添加到同一个解决方案中，并 _DaprBackEnd_ 调用它。 选择 " **API** " 作为项目类型。 默认情况下，Dapr 挎斗依赖于网络边界来限制对其公共 API 的访问。 因此，请清除 " **配置 HTTPS**" 复选框。

    :::image type="content" source="./media/getting-started/multicontainer-createwebapi.png" alt-text="创建 web API 的屏幕截图":::

### <a name="add-dapr-service-invocation"></a>添加 Dapr 服务调用

现在，你将使用 Dapr [服务调用构建基块](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/)配置服务之间的通信。 允许 web 应用从 web API 中检索天气预报。 服务调用构建基块的优点很多。 它包括服务发现、自动重试、 (使用 mTLS) 的消息加密，并改进了可观察性。 你将使用 Dapr .NET SDK 调用 Dapr 挎斗上的服务调用 API。

1. 在 Visual Studio 中，打开包管理器控制台 (**工具 > "NuGet 包管理器" > 程序包管理器控制台**) ，并确保这 `DaprFrontEnd` 是默认项目。 从控制台中，将 `Dapr.AspNetCore` NuGet 包添加到项目：

    ```powershell
    Install-Package Dapr.AspNetCore
    ```

    > [!NOTE]
    > 如果要以预发行版本为目标，则 `Dapr.AspNetCore` 需要指定 `-Prerelease` 标志。

1. 在 `DaprFrontEnd` 项目中，打开 *.cs* 文件，并将 `ConfigureServices` 方法替换为以下代码：

    ```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers().AddDapr();
        services.AddRazorPages();
    }
    ```

    对的调用会将 `AddDapr` 类注册到 `DaprClient` ASP.NET Core 依赖关系注入系统。 在注册客户端的情况下，你现在可以将的实例注入 `DaprClient` 到你的服务代码中，以便与 Dapr 挎斗、构建基块和组件通信。

1. 将名为 *WeatherForecast* 的新 c # 类文件添加到 `DaprFrontEnd` 项目：

    ```csharp
    using System;

    namespace DaprFrontEnd
    {
        public class WeatherForecast
        {
            public DateTime Date { get; set; }

            public int TemperatureC { get; set; }

            public int TemperatureF { get; set; }

            public string Summary { get; set; }
        }
    }
    ```

1. 打开 *Pages* 文件夹中 *的 ""，然后将其* 内容替换为以下代码：

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading.Tasks;
    using Dapr.Client;
    using Microsoft.AspNetCore.Mvc.RazorPages;

    namespace DaprFrontEnd.Pages
    {
        public class IndexModel : PageModel
        {
            private readonly DaprClient _daprClient;

            public IndexModel(DaprClient daprClient)
            {
                _daprClient = daprClient ?? throw new ArgumentNullException(nameof(daprClient));
            }

            public async Task OnGet()
            {
                var forecasts = await _daprClient.InvokeMethodAsync<IEnumerable<WeatherForecast>>(
                    HttpMethod.Get,
                    "daprbackend",
                    "weatherforecast");

                ViewData["WeatherForecastData"] = forecasts;
            }
        }
    }
    ```

    通过将类注入构造函数，可将 Dapr 功能添加到 web 应用中 `DaprClient` `IndexModel` 。 在 `OnGet` 方法中，通过 Dapr 服务调用构建基块调用 API 服务。 `OnGet`每当用户访问主页时，都会调用方法。 使用 `DaprClient.InvokeMethodAsync` 方法来调用 `weatherforecast` 服务的方法 `daprbackend` 。 稍后，将 web API 配置为 `daprbackend` 在将其配置为使用 Dapr 运行时用作其应用程序 ID。 最后，将服务响应保存在查看数据中。

1. 用以下代码替换 *Pages* 文件夹中的 *索引 cshtml* 文件的内容。 它会将查看数据中存储的天气预报显示给用户：

    ```razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }

    <div class="text-center">
        <h1 class="display-4">Welcome</h1>
        <p>Learn about <a href="https://docs.microsoft.com/aspnet/core">building Web apps with ASP.NET Core</a>.</p>
        @foreach (var forecast in (IEnumerable<WeatherForecast>)ViewData["WeatherForecastData"])
        {
            <p>The forecast for @forecast.Date is @forecast.Summary!</p>
        }
    </div>
    ```

### <a name="add-container-support"></a>添加容器支持

在此示例的最后一部分，你将添加容器支持，并使用 Docker Compose 运行解决方案。

1. 右键单击该 `DaprFrontEnd` 项目，然后选择 "**添加**  >  **容器 Orchestrator 支持**"。 此时将显示 " **添加容器 Orchestrator 支持** " 对话框：

    :::image type="content" source="./media/getting-started/multicontainer-addorchestrator.png" alt-text="添加容器 orchestrator 支持的屏幕截图":::

    选择“Docker Compose”。

1. 在下一个对话框中，选择 " **Linux** " 作为目标操作系统：

    :::image type="content" source="./media/getting-started/multicontainer-targetos.png" alt-text="选择 Docker 目标 OS 的屏幕截图":::

    Visual Studio 将在解决方案的 **docker 撰写** 文件夹中创建 *docker-compose.yml docker-compose.override.yml* 文件和 *.dockerignore* 文件：

    :::image type="content" source="./media/getting-started/multicontainer-dockersolution.png" alt-text="Docker 合成项目的屏幕截图":::

    *Docker-compose.yml. docker-compose.override.yml* 文件包含以下内容：

    ```yaml
    version: "3.4"

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile
    ```

    .dockerignore 文件包含你不希望 Docker 在容器中包含的文件类型和扩展名。 这些文件与开发环境和源控件相关联，而不是与要部署的应用程序或服务相关联。

    在 *DaprFrontEnd* 项目目录的根目录中，创建一个新的 *Dockerfile* 。 *Dockerfile* 是用于生成图像的一系列命令。 有关详细信息，请参阅 [Dockerfile reference](https://docs.docker.com/engine/reference/builder)。

    *Dockerfile* 包含 YAML：

    ```yml
    FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
    WORKDIR /app
    EXPOSE 80
    EXPOSE 443

    FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
    WORKDIR /src
    COPY ["DaprFrontEnd/DaprFrontEnd.csproj", "DaprFrontEnd/"]
    RUN dotnet restore "DaprFrontEnd/DaprFrontEnd.csproj"
    COPY . .
    WORKDIR "/src/DaprFrontEnd"
    RUN dotnet build "DaprFrontEnd.csproj" -c Release -o /app/build

    FROM build AS publish
    RUN dotnet publish "DaprFrontEnd.csproj" -c Release -o /app/publish

    FROM base AS final
    WORKDIR /app
    COPY --from=publish /app/publish .
    ENTRYPOINT ["dotnet", "DaprFrontEnd.dll"]
    ```

    在调用时，前面的 *Dockerfile* 按顺序执行以下步骤：

    1. 提取 `mcr.microsoft.com/dotnet/aspnet:3.1` 映像并将其命名 `base` 。
    1. 将工作目录设置为 */app*。
    1. 公开端口 `80` 和 `443` 。
    1. 提取 `mcr.microsoft.com/dotnet/sdk:3.1` 映像并将其命名 `build` 。
    1. 将工作目录设置为 */src*。
    1. 将 _DaprFrontEnd/DaprFrontEnd_ 复制到名为 *DaprFrontEnd/*. 的新目录中。
    1. 对 [`dotnet restore`](../../core/tools/dotnet-restore.md) 该项目的调用。
    1. 将根目录中的所有内容复制到映像的根目录中。
    1. 将工作目录设置为 _/src/DaprFrontEnd_。
    1. 对 [`dotnet build`](../../core/tools/dotnet-build.md) 该项目的调用。
        - 将 **发布** 配置和输出定位到 */app/build*。
    1. 从现有基本映像初始化新的生成阶段 `build` 并为其命名 `publish` 。
    1. 对 `dotnet publish` 该项目的调用。
        - 将 **发布** 配置和输出定位到 */app/publish*。
    1. 从现有基本映像初始化新的生成阶段 `publish` 并为其命名 `final` 。
    1. 将工作目录设置为 */app*。
    1. 将 `/app/publish` 目录从映像复制 `publish` 到映像的根目录 `final` 。
    1. 将入口点设置为的图像 `dotnet` ，并将 `DaprFrontEnd.dll` 作为参数传递。

1. 在 `DaprBackEnd` web API 项目中，右键单击项目节点，然后选择 "**添加**  >  **容器 Orchestrator 支持**"。 选择 " **Docker Compose**"，然后再次选择 " **Linux** " 作为 "目标 OS"。

    在 _DaprBackEnd_ 项目目录的根目录中，创建一个新的 *Dockerfile* 。 *Dockerfile* 包含以下 YAML：

    ```yml
    FROM mcr.microsoft.com/dotnet/aspnet:3.1 AS base
    WORKDIR /app
    EXPOSE 80

    FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build
    WORKDIR /src
    COPY ["DaprBackEnd/DaprBackEnd.csproj", "DaprBackEnd/"]
    RUN dotnet restore "DaprBackEnd/DaprBackEnd.csproj"
    COPY . .
    WORKDIR "/src/DaprBackEnd"
    RUN dotnet build "DaprBackEnd.csproj" -c Release -o /app/build

    FROM build AS publish
    RUN dotnet publish "DaprBackEnd.csproj" -c Release -o /app/publish

    FROM base AS final
    WORKDIR /app
    COPY --from=publish /app/publish .
    ENTRYPOINT ["dotnet", "DaprBackEnd.dll"]
    ```

    再次打开 *docker-compose.yml docker-compose.override.yml* 文件并检查其内容。 Visual Studio 已更新 **Docker Compose** 文件。 现在包括两个服务：

    ```yaml
    version: '3.4'

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile

      daprbackend:
        image: ${DOCKER_REGISTRY-}daprbackend
        build:
          context: .
          dockerfile: DaprBackEnd/Dockerfile
    ```

1. 若要在容器化应用程序中使用 Dapr 构建基块，需要将 Dapr 分支容器添加到撰写文件中。 仔细更新 *docker-compose.yml* 文件的内容，以匹配以下示例。 请密切注意格式和间距，不要使用制表符。

    ```yaml
    version: '3.4'

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile
        ports:
          - "51000:50001"

      daprfrontend-dapr:
        image: "daprio/daprd:latest"
        command: [ "./daprd", "-app-id", "daprfrontend", "-app-port", "80" ]
        depends_on:
          - daprfrontend
        network_mode: "service:daprfrontend"

      daprbackend:
        image: ${DOCKER_REGISTRY-}daprbackend
        build:
          context: .
          dockerfile: DaprBackEnd/Dockerfile
        ports:
          - "52000:50001"

      daprbackend-dapr:
        image: "daprio/daprd:latest"
        command: [ "./daprd", "-app-id", "daprbackend", "-app-port", "80" ]
        depends_on:
          - daprfrontend
        network_mode: "service:daprbackend"
    ```

    在更新后的文件中，我们 `daprfrontend-dapr` `daprbackend-dapr` 分别为和服务添加并分支 `daprfrontend` `daprbackend` 。 在更新后的文件中，请注意以下更改：

    - 分支使用 `daprio/daprd:latest` 容器映像。 `latest`对于生产方案，不建议使用标记。 对于生产，更好的做法是使用特定的版本号。
    - 在撰写文件中定义的每个服务都有其自己的网络命名空间，以便进行网络隔离。 分支用于 `network_mode: "service:..."` 确保它们在与应用程序相同的网络命名空间中运行。 这样做可以允许挎斗和应用程序使用进行通信 `localhost` 。
    - 默认情况下，Dapr 分支侦听 gRPC 通信 (的端口必须公开 50001) ，以允许分支相互通信。

1.  (<kbd>F5</kbd> 或 <kbd>Ctrl + F5</kbd>) 运行解决方案，验证它是否按预期方式工作。 如果所有内容都已正确配置，则应看到天气预报数据：

    :::image type="content" source="./media/getting-started/multicontainer-result.png" alt-text="显示天气预测数据的最终解决方案的屏幕截图":::

    通过 Docker Compose 和 Visual Studio 2019 在本地运行，可以设置断点并调试到应用程序中。 对于生产方案，建议在 Kubernetes 中托管应用程序。 本书包括随附的引用应用程序 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)，其中包含要部署到 Kubernetes 的脚本。

    若要了解有关本演练中使用的 Dapr 服务调用构建基块的详细信息，请参阅 [第6章](service-invocation.md)。

## <a name="summary"></a>总结

在本章中，你有机会 *测试驱动器* Dapr。 使用 Dapr .NET SDK，可以看到 Dapr 如何与 .NET 应用程序平台集成。

第一个示例是一个简单的有状态 .NET 控制台应用程序，它使用了 Dapr 状态管理构建基块。

第二个示例涉及在 Docker 中运行的多容器应用程序。 通过将 Visual Studio 与 Docker Compose 结合使用，你可以在所有 .NET 应用中使用熟悉的 *F5 调试体验* 。

你还可以更深入地了解 Dapr 组件配置文件。 它们配置 Dapr 构建基块使用的实际基础结构实现。 你可以使用命名空间和范围限制对特定环境和应用程序的组件访问。

在即将推出的章节中，你将深入了解 Dapr 提供的构建基块。

### <a name="references"></a>参考

- [Dapr 文档-入门](https://docs.dapr.io/getting-started)
- [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)

> [!div class="step-by-step"]
> [上一页](dapr-at-20000-feet.md)
> [下一页](reference-application.md)
