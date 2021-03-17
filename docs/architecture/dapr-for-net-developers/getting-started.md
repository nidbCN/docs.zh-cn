---
title: Dapr 入门
description: 用于准备本地开发环境和使用 Dapr 构建你的第一个 .NET 应用程序的指南。
author: amolenk
ms.date: 02/25/2021
ms.openlocfilehash: 1b6ecd2cc2bf077375262155f0866cfef2dab708
ms.sourcegitcommit: d623f686701b94bef905ec5e93d8b55d031c5d6f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2021
ms.locfileid: "103623806"
---
# <a name="get-started-with-dapr"></a><span data-ttu-id="bb6ae-103">Dapr 入门</span><span class="sxs-lookup"><span data-stu-id="bb6ae-103">Get started with Dapr</span></span>

<span data-ttu-id="bb6ae-104">前两章介绍了有关 Dapr 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-104">In the first two chapters, you learned basic concepts about Dapr.</span></span> <span data-ttu-id="bb6ae-105">现在可以将其用于 *测试驱动器*。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-105">It's time to take it for a *test drive*.</span></span> <span data-ttu-id="bb6ae-106">本章将指导你完成本地开发环境的准备工作，并构建两个 Dapr .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-106">This chapter will guide you through preparing your local development environment and building two Dapr .NET applications.</span></span>

## <a name="install-dapr-into-your-local-environment"></a><span data-ttu-id="bb6ae-107">将 Dapr 安装到本地环境中</span><span class="sxs-lookup"><span data-stu-id="bb6ae-107">Install Dapr into your local environment</span></span>

<span data-ttu-id="bb6ae-108">首先，在开发计算机上安装 Dapr。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-108">You'll start by installing Dapr on your development computer.</span></span> <span data-ttu-id="bb6ae-109">完成后，可以在 [自承载模式下](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-overview/)生成并运行 Dapr 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-109">Once complete, you can build and run Dapr applications in [self-hosted mode](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-overview/).</span></span>

1. <span data-ttu-id="bb6ae-110">[安装 DAPR CLI](https://docs.dapr.io/getting-started/install-dapr-cli/)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-110">[Install the Dapr CLI](https://docs.dapr.io/getting-started/install-dapr-cli/).</span></span> <span data-ttu-id="bb6ae-111">它使您可以启动、运行和管理 Dapr 实例。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-111">It enables you to launch, run, and manage Dapr instances.</span></span> <span data-ttu-id="bb6ae-112">它还提供调试支持。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-112">It also provides debugging support.</span></span>

1. <span data-ttu-id="bb6ae-113">安装 [Docker Desktop](https://docs.docker.com/get-docker/)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-113">Install [Docker Desktop](https://docs.docker.com/get-docker/).</span></span> <span data-ttu-id="bb6ae-114">如果在 Windows 上运行，请确保将 **适用于 windows 的 Docker Desktop** 配置为使用 Linux 容器。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-114">If you're running on Windows, make sure that **Docker Desktop for Windows** is configured to use Linux containers.</span></span>

> [!NOTE]
> <span data-ttu-id="bb6ae-115">默认情况下，Dapr 使用 Docker 容器为你提供最佳的全新体验。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-115">By default, Dapr uses Docker containers to provide you the best out-of-the-box experience.</span></span> <span data-ttu-id="bb6ae-116">若要在 Docker 外部运行 Dapr，可以跳过此步骤并 [执行 *超薄* 初始化](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-116">To run Dapr outside of Docker, you can skip this step and [execute a *slim* initialization](https://docs.dapr.io/operations/hosting/self-hosted/self-hosted-no-docker/).</span></span> <span data-ttu-id="bb6ae-117">本章中的示例要求使用 Docker 容器。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-117">The examples in this chapter require you use Docker containers.</span></span>

1. <span data-ttu-id="bb6ae-118">[初始化 Dapr](https://docs.dapr.io/getting-started/install-dapr/)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-118">[Initialize Dapr](https://docs.dapr.io/getting-started/install-dapr/).</span></span> <span data-ttu-id="bb6ae-119">此步骤通过安装最新的 Dapr 二进制文件和容器映像来设置开发环境。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-119">This step sets up your development environment by installing the latest Dapr binaries and container images.</span></span>

1. <span data-ttu-id="bb6ae-120">安装 [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-120">Install the [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet/3.1).</span></span>

<span data-ttu-id="bb6ae-121">安装 Dapr 后，就可以构建你的第一个 Dapr 应用程序了！</span><span class="sxs-lookup"><span data-stu-id="bb6ae-121">Now that Dapr is installed, it's time to build your first Dapr application!</span></span>

## <a name="build-your-first-dapr-application"></a><span data-ttu-id="bb6ae-122">构建你的第一个 Dapr 应用程序</span><span class="sxs-lookup"><span data-stu-id="bb6ae-122">Build your first Dapr application</span></span>

<span data-ttu-id="bb6ae-123">首先构建一个使用 [Dapr 状态管理](state-management.md) 构建块的简单 .net 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-123">You'll start by building a simple .NET Console application that consumes the [Dapr state management](state-management.md) building block.</span></span>

### <a name="create-the-application"></a><span data-ttu-id="bb6ae-124">创建应用程序</span><span class="sxs-lookup"><span data-stu-id="bb6ae-124">Create the application</span></span>

1. <span data-ttu-id="bb6ae-125">打开所选的命令行界面或终端。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-125">Open up the command shell or terminal of your choice.</span></span> <span data-ttu-id="bb6ae-126">您可以考虑 [Visual Studio Code](https://code.visualstudio.com/)中的终端功能。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-126">You might consider the terminal capabilities in [Visual Studio Code](https://code.visualstudio.com/).</span></span> <span data-ttu-id="bb6ae-127">导航到要在其中生成应用程序的根文件夹。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-127">Navigate to the root folder in which you want to build your application.</span></span> <span data-ttu-id="bb6ae-128">完成后，输入以下命令以创建新的 .NET 控制台应用程序：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-128">Once there, enter the following command to create a new .NET Console application:</span></span>

    ```dotnetcli
    dotnet new console -o DaprCounter
    ```

    <span data-ttu-id="bb6ae-129">命令基架简单的 "Hello World" .NET Core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-129">The command scaffolds a simple "Hello World" .NET Core application.</span></span>

1. <span data-ttu-id="bb6ae-130">然后，导航到上一个命令所创建的新目录中：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-130">Then, navigate into the new directory created by the previous command:</span></span>

    ```console
    cd DaprCounter
    ```

1. <span data-ttu-id="bb6ae-131">使用命令运行新创建的应用程序 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-131">Run the newly created application using the `dotnet run` command.</span></span> <span data-ttu-id="bb6ae-132">这样做会写入 "Hello World！"</span><span class="sxs-lookup"><span data-stu-id="bb6ae-132">Doing so writes "Hello World!"</span></span> <span data-ttu-id="bb6ae-133">到控制台屏幕：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-133">to the console screen:</span></span>

    ```dotnetcli
    dotnet run
    ```

### <a name="add-dapr-state-management"></a><span data-ttu-id="bb6ae-134">添加 Dapr 状态管理</span><span class="sxs-lookup"><span data-stu-id="bb6ae-134">Add Dapr State Management</span></span>

<span data-ttu-id="bb6ae-135">接下来，你将使用 Dapr [状态管理构建基块](https://docs.dapr.io/developing-applications/building-blocks/state-management/state-management-overview/) 在程序中实现有状态计数器。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-135">Next, you'll use the Dapr [state management building block](https://docs.dapr.io/developing-applications/building-blocks/state-management/state-management-overview/) to implement a stateful counter in the program.</span></span>

<span data-ttu-id="bb6ae-136">可以使用 Dapr 对 HTTP 和 gRPC 的本机支持，在任何开发平台上调用 Dapr Api。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-136">You can invoke Dapr APIs across any development platform using Dapr's native support for HTTP and gRPC.</span></span> <span data-ttu-id="bb6ae-137">然而，.NET 开发人员会发现 Dapr .NET SDK 更自然且更直观。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-137">However, .NET Developers will find the Dapr .NET SDK more natural and intuitive.</span></span> <span data-ttu-id="bb6ae-138">它提供强类型的 .NET 客户端来调用 Dapr Api。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-138">It provides a strongly typed .NET client to call the Dapr APIs.</span></span> <span data-ttu-id="bb6ae-139">.NET SDK 还与 ASP.NET Core 紧密集成。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-139">The .NET SDK also tightly integrates with ASP.NET Core.</span></span>

1. <span data-ttu-id="bb6ae-140">在终端窗口中，将 `Dapr.Client` NuGet 包添加到应用程序：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-140">From the terminal window, add the `Dapr.Client` NuGet package to your application:</span></span>

    ```dotnetcli
    dotnet add package Dapr.Client
    ```

    > [!NOTE]
    > <span data-ttu-id="bb6ae-141">如果使用的是预发行版本的 Dapr，请确保将该 `--prerelease` 标志添加到命令中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-141">If you're working with a pre-release version of Dapr, be sure to add the `--prerelease` flag to the command.</span></span>

1. <span data-ttu-id="bb6ae-142">`Program.cs`在偏好的编辑器中打开文件，并将其内容更新为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-142">Open the `Program.cs` file in your favorite editor and update its contents to the following code:</span></span>

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

    <span data-ttu-id="bb6ae-143">更新的代码实现以下步骤：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-143">The updated code implements the following steps:</span></span>

    - <span data-ttu-id="bb6ae-144">首先 `DaprClient` 实例化一个新实例。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-144">First a new `DaprClient` instance is instantiated.</span></span> <span data-ttu-id="bb6ae-145">此类使你可以与 Dapr 挎斗交互。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-145">This class enables you to interact with the Dapr sidecar.</span></span>
    - <span data-ttu-id="bb6ae-146">从状态存储中 `DaprClient.GetStateAsync` 获取密钥的值 `counter` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-146">From the state store, `DaprClient.GetStateAsync` fetches the value for the `counter` key.</span></span> <span data-ttu-id="bb6ae-147">如果该键不存在，则 `int` 返回)  (默认值 `0` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-147">If the key doesn't exist, the default `int` value (which is `0`) is returned.</span></span>
    - <span data-ttu-id="bb6ae-148">然后，代码将循环访问，将 `counter` 值写入控制台，并将递增的值保存到状态存储中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-148">The code then iterates, writing the `counter` value to the console and saving an incremented value to the state store.</span></span>

1. <span data-ttu-id="bb6ae-149">Dapr CLI `run` 命令启动该应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-149">The Dapr CLI `run` command starts the application.</span></span> <span data-ttu-id="bb6ae-150">它调用基础 Dapr 运行时，并使应用程序和 Dapr 挎斗一起运行。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-150">It invokes the underlying Dapr runtime and enables both the application and Dapr sidecar to run together.</span></span> <span data-ttu-id="bb6ae-151">如果省略，则 `app-id` Dapr 将为应用程序生成唯一的名称。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-151">If you omit the `app-id`, Dapr will generate a unique name for the application.</span></span> <span data-ttu-id="bb6ae-152">命令的最后一段 `dotnet run` 指示 Dapr 运行时运行 .net core 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-152">The final segment of the command, `dotnet run`, instructs the Dapr runtime to run the .NET core application.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="bb6ae-153">`app-id`使用状态管理构建块时，必须注意始终传递显式参数。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-153">Care must be taken to always pass an explicit `app-id` parameter when consuming the state management building block.</span></span> <span data-ttu-id="bb6ae-154">对于每个键/值对，块使用应用程序 id 值作为其状态键的 *前缀* 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-154">The block uses the application id value as a *prefix* for its state key for each key/value pair.</span></span> <span data-ttu-id="bb6ae-155">如果应用程序 id 发生更改，则无法再访问以前存储的状态。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-155">If the application id changes, you can no longer access the previously stored state.</span></span>

    <span data-ttu-id="bb6ae-156">现在，用以下命令运行应用程序：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-156">Now run the application with the following command:</span></span>

    ```console
    dapr run --app-id DaprCounter dotnet run
    ```

    <span data-ttu-id="bb6ae-157">尝试停止并重新启动应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-157">Try stopping and restarting the application.</span></span> <span data-ttu-id="bb6ae-158">你将看到计数器未重置。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-158">You'll see that the counter doesn't reset.</span></span> <span data-ttu-id="bb6ae-159">相反，它会从以前保存的状态继续。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-159">Instead it continues from the previously saved state.</span></span> <span data-ttu-id="bb6ae-160">Dapr 构建基块会使应用程序具有状态。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-160">The Dapr building block makes the application stateful.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bb6ae-161">务必了解你的示例应用程序与预配置状态组件的通信，但没有直接依赖它。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-161">It's important to understand your sample application communicates with a pre-configured state component, but has no direct dependency on it.</span></span> <span data-ttu-id="bb6ae-162">Dapr 提取依赖关系。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-162">Dapr abstracts away the dependency.</span></span> <span data-ttu-id="bb6ae-163">正如您很快会看到的，可以使用简单的配置更新来更改基础状态存储组件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-163">As you'll shortly see, the underlying state store component can be changed with a simple configuration update.</span></span>

<span data-ttu-id="bb6ae-164">您可能想知道在哪里存储状态？</span><span class="sxs-lookup"><span data-stu-id="bb6ae-164">You might be wondering, where exactly is the state stored?</span></span>

## <a name="component-configuration-files"></a><span data-ttu-id="bb6ae-165">组件配置文件</span><span class="sxs-lookup"><span data-stu-id="bb6ae-165">Component configuration files</span></span>

<span data-ttu-id="bb6ae-166">首次为本地环境初始化 Dapr 时，它会自动预配 Redis 容器。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-166">When you first initialized Dapr for your local environment, it automatically provisioned a Redis container.</span></span> <span data-ttu-id="bb6ae-167">然后，Dapr 将 Redis 容器配置为包含组件配置文件的默认状态存储组件（标题为） `statestore.yaml` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-167">Dapr then configured the Redis container as the default state store component with a component configuration file, entitled `statestore.yaml`.</span></span> <span data-ttu-id="bb6ae-168">下面介绍了其内容：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-168">Here's a look at its contents:</span></span>

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
> <span data-ttu-id="bb6ae-169">默认组件配置文件存储在 `$HOME/.dapr/components` Linux/macOS 的文件夹中，并存储在 `%USERPROFILE%\.dapr\components` Windows 上的文件夹中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-169">Default component configuration files are stored in the `$HOME/.dapr/components` folder on Linux/macOS, and in the `%USERPROFILE%\.dapr\components` folder on Windows.</span></span>

<span data-ttu-id="bb6ae-170">请注意上一个组件配置文件的格式：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-170">Note the format of the previous component configuration file:</span></span>

- <span data-ttu-id="bb6ae-171">每个组件都有一个名称。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-171">Each component has a name.</span></span> <span data-ttu-id="bb6ae-172">在上面的示例中，该组件的名称为 `statestore` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-172">In the sample above, the component is named `statestore`.</span></span> <span data-ttu-id="bb6ae-173">我们在第一个代码示例中使用了该名称，告诉 Dapr 挎斗要使用哪个组件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-173">We used that name in our first code example to tell the Dapr sidecar which component to use.</span></span>
- <span data-ttu-id="bb6ae-174">每个组件配置文件都有一个 `spec` 节。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-174">Each component configuration file has a `spec` section.</span></span> <span data-ttu-id="bb6ae-175">它包含 `type` 指定组件类型的字段。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-175">It contains a `type` field that specifies the component type.</span></span> <span data-ttu-id="bb6ae-176">`version`字段指定组件版本。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-176">The `version` field specifies the component version.</span></span> <span data-ttu-id="bb6ae-177">`metadata`字段包含组件需要的信息，例如连接详细信息和其他设置。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-177">The `metadata` field contains information that the component requires, such as connection details and other settings.</span></span> <span data-ttu-id="bb6ae-178">不同类型的组件的元数据值会有所不同。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-178">The metadata values will vary for the different types of components.</span></span>

<span data-ttu-id="bb6ae-179">Dapr 挎斗可以使用在应用程序中配置的任何 Dapr 组件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-179">A Dapr sidecar can consume any Dapr component configured in your application.</span></span> <span data-ttu-id="bb6ae-180">但是，如果您的体系结构理由限制了组件的可访问性，会怎样呢？</span><span class="sxs-lookup"><span data-stu-id="bb6ae-180">But, what if you had an architectural justification to limit the accessibility of a component?</span></span> <span data-ttu-id="bb6ae-181">如何将 Redis 组件限制为仅在生产环境中运行的 Dapr 分支？</span><span class="sxs-lookup"><span data-stu-id="bb6ae-181">How could you restrict the Redis component to Dapr sidecars running only in a production environment?</span></span>

<span data-ttu-id="bb6ae-182">为此，可以 `namespace` 为生产环境定义。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-182">To do so, you could define a `namespace` for the production environment.</span></span> <span data-ttu-id="bb6ae-183">可以将其命名为 `production` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-183">You might name it `production`.</span></span> <span data-ttu-id="bb6ae-184">在自承载模式下，通过设置环境变量来指定 Dapr 挎斗的命名空间 `NAMESPACE` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-184">In self-hosted mode, you specify the namespace of a Dapr sidecar by setting the `NAMESPACE` environment variable.</span></span> <span data-ttu-id="bb6ae-185">配置后，Dapr 挎斗将仅加载与该命名空间匹配的组件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-185">When configured, the Dapr sidecar will only load the components that match the namespace.</span></span> <span data-ttu-id="bb6ae-186">对于 Kubernetes 部署，Kubernetes 命名空间确定加载的组件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-186">For Kubernetes deployments, the Kubernetes namespace determines the components that are loaded.</span></span> <span data-ttu-id="bb6ae-187">下面的示例演示命名空间中的 Redis 组件 `production` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-187">The following sample shows the Redis component placed in a `production` namespace.</span></span> <span data-ttu-id="bb6ae-188">请注意 `namespace` 元素中的声明 `metadata` ：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-188">Note the `namespace` declaration in the `metadata` element:</span></span>

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
> <span data-ttu-id="bb6ae-189">只有在同一命名空间中运行的应用程序才能访问带命名空间组件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-189">A namespaced component is only accessible to applications running in the same namespace.</span></span> <span data-ttu-id="bb6ae-190">如果 Dapr 应用程序无法加载组件，请确保应用程序命名空间与组件命名空间相匹配。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-190">If your Dapr application fails to load a component, make sure that the application namespace matches the component namespace.</span></span> <span data-ttu-id="bb6ae-191">在自承载模式下，这在应用程序命名空间存储于环境变量中的情况下尤其重要 `NAMESPACE` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-191">This can be especially tricky in self-hosted mode where the application namespace is stored in a `NAMESPACE` environment variable.</span></span>

<span data-ttu-id="bb6ae-192">如果需要，可以进一步将组件限制到特定的应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-192">If needed, you could further restrict a component to a particular application.</span></span> <span data-ttu-id="bb6ae-193">在 `production` 命名空间中，你可能需要将 Redis 缓存的访问限制为仅限 `DaprCounter` 应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-193">Within the `production` namespace, you may want to limit access of the Redis cache to only the `DaprCounter` application.</span></span> <span data-ttu-id="bb6ae-194">为此，请 `scopes` 在组件配置中指定。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-194">You do so by specifying `scopes` in the component configuration.</span></span> <span data-ttu-id="bb6ae-195">下面的示例演示如何将对 Redis 组件的访问限制 `statestore` 到 `DaprCounter` 命名空间中的应用程序 `production` ：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-195">The following example shows how to restrict access to the Redis `statestore` component to the application `DaprCounter` in the `production` namespace:</span></span>

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

## <a name="build-a-multi-container-dapr-application"></a><span data-ttu-id="bb6ae-196">构建多容器 Dapr 应用程序</span><span class="sxs-lookup"><span data-stu-id="bb6ae-196">Build a multi-container Dapr application</span></span>

<span data-ttu-id="bb6ae-197">在第一个示例中，创建了一个与 Dapr 挎斗并行运行的简单 .NET 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-197">In the first example, you created a simple .NET console application that ran side-by-side with a Dapr sidecar.</span></span> <span data-ttu-id="bb6ae-198">不过，新式分布式应用程序通常由许多移动部件组成。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-198">Modern distributed applications, however, often consist of many moving parts.</span></span> <span data-ttu-id="bb6ae-199">它们可以同时运行独立微服务。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-199">They can simultaneously run independent microservices.</span></span> <span data-ttu-id="bb6ae-200">这些新式应用程序通常容器化，并需要容器业务流程工具，例如 Docker Compose 或 Kubernetes。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-200">These modern applications are typically containerized and require container orchestration tools such as Docker Compose or Kubernetes.</span></span>

<span data-ttu-id="bb6ae-201">在下一个示例中，将创建多容器应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-201">In the next example, you'll create a multi-container application.</span></span> <span data-ttu-id="bb6ae-202">你还将使用 [Dapr 服务调用](service-invocation.md) 构建基块在服务之间进行通信。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-202">You'll also use the [Dapr service invocation](service-invocation.md) building block to communicate between services.</span></span> <span data-ttu-id="bb6ae-203">解决方案将包含一个 web 应用程序，该应用程序从 web API 中检索天气预报。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-203">The solution will consist of a web application that retrieves weather forecasts from a web API.</span></span> <span data-ttu-id="bb6ae-204">它们每个都在 Docker 容器中运行。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-204">They will each run in a Docker container.</span></span> <span data-ttu-id="bb6ae-205">你将使用 Docker Compose 在本地运行容器并启用调试功能。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-205">You'll use Docker Compose to run the container locally and enable debugging capabilities.</span></span>

<span data-ttu-id="bb6ae-206">请确保已为 Dapr 配置了本地环境并安装了 [.Net Core 3.1 开发工具](https://dotnet.microsoft.com/download/dotnet-core/3.1) (说明可在本章开头) 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-206">Make sure you've configured your local environment for Dapr and installed the [.NET Core 3.1 Development Tools](https://dotnet.microsoft.com/download/dotnet-core/3.1) (instructions are available at the beginning of this chapter).</span></span>

<span data-ttu-id="bb6ae-207">此外，还需要在安装了 **.Net Core 跨平台开发** 工作负载的情况下，使用 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads)完成此示例。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-207">Additionally, you'll need complete this sample using [Visual Studio 2019](https://visualstudio.microsoft.com/downloads) with the **.NET Core cross-platform development** workload installed.</span></span>

### <a name="create-the-application"></a><span data-ttu-id="bb6ae-208">创建应用程序</span><span class="sxs-lookup"><span data-stu-id="bb6ae-208">Create the application</span></span>

1. <span data-ttu-id="bb6ae-209">在 Visual Studio 2019 中创建 **ASP.NET Core Web 应用** 项目：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-209">In Visual Studio 2019, create an **ASP.NET Core Web App** project:</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-createproject.png" alt-text="创建新项目的屏幕截图":::

1. <span data-ttu-id="bb6ae-211">命名项目 `DaprFrontEnd` 和解决方案 `DaprMultiContainer` ：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-211">Name your project `DaprFrontEnd` and your solution `DaprMultiContainer`:</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-configureproject.png" alt-text="配置新项目的屏幕截图":::

1. <span data-ttu-id="bb6ae-213">选择“Web 应用程序”，以创建使用 Razor Pages 的 Web 应用程序  。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-213">Select **Web Application** to create a web application with Razor pages.</span></span> <span data-ttu-id="bb6ae-214">不要选择 " **启用 Docker 支持**"。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-214">Don't select **Enable Docker Support**.</span></span> <span data-ttu-id="bb6ae-215">稍后添加 Docker 支持。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-215">You'll add Docker support later.</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-createwebapp.png" alt-text="创建新 ASP.NET Core web 应用程序的屏幕截图":::

1. <span data-ttu-id="bb6ae-217">将 ASP.NET Core Web API 项目添加到同一个解决方案中，并 _DaprBackEnd_ 调用它。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-217">Add a ASP.NET Core Web API project to the same solution and call it _DaprBackEnd_.</span></span> <span data-ttu-id="bb6ae-218">选择 " **API** " 作为项目类型。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-218">Select **API** as the project type.</span></span> <span data-ttu-id="bb6ae-219">默认情况下，Dapr 挎斗依赖于网络边界来限制对其公共 API 的访问。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-219">By default, a Dapr sidecar relies on the network boundary to limit access to its public API.</span></span> <span data-ttu-id="bb6ae-220">因此，请清除 " **配置 HTTPS**" 复选框。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-220">So, clear the checkbox for **Configure for HTTPS**.</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-createwebapi.png" alt-text="创建 web API 的屏幕截图":::

### <a name="add-dapr-service-invocation"></a><span data-ttu-id="bb6ae-222">添加 Dapr 服务调用</span><span class="sxs-lookup"><span data-stu-id="bb6ae-222">Add Dapr service invocation</span></span>

<span data-ttu-id="bb6ae-223">现在，你将使用 Dapr [服务调用构建基块](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/)配置服务之间的通信。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-223">Now, you'll configure communication between the services using Dapr [service invocation building block](https://docs.dapr.io/developing-applications/building-blocks/service-invocation/service-invocation-overview/).</span></span> <span data-ttu-id="bb6ae-224">允许 web 应用从 web API 中检索天气预报。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-224">You'll enable the web app to retrieve weather forecasts from the web API.</span></span> <span data-ttu-id="bb6ae-225">服务调用构建基块的优点很多。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-225">The service invocation building block features many benefits.</span></span> <span data-ttu-id="bb6ae-226">它包括服务发现、自动重试、 (使用 mTLS) 的消息加密，并改进了可观察性。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-226">It includes service discovery, automatic retries, message encryption (using mTLS), and improved observability.</span></span> <span data-ttu-id="bb6ae-227">你将使用 Dapr .NET SDK 调用 Dapr 挎斗上的服务调用 API。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-227">You'll use the Dapr .NET SDK to invoke the service invocation API on the Dapr sidecar.</span></span>

1. <span data-ttu-id="bb6ae-228">在 Visual Studio 中，打开包管理器控制台 (**工具 > "NuGet 包管理器" > 程序包管理器控制台**) ，并确保这 `DaprFrontEnd` 是默认项目。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-228">In Visual Studio, open the Package Manager Console (**Tools > NuGet Package Manager > Package Manager Console**) and make sure that `DaprFrontEnd` is the default project.</span></span> <span data-ttu-id="bb6ae-229">从控制台中，将 `Dapr.AspNetCore` NuGet 包添加到项目：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-229">From the console, add the `Dapr.AspNetCore` NuGet package to the project:</span></span>

    ```powershell
    Install-Package Dapr.AspNetCore
    ```

    > [!NOTE]
    > <span data-ttu-id="bb6ae-230">如果要以预发行版本为目标，则 `Dapr.AspNetCore` 需要指定 `-Prerelease` 标志。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-230">If you're targeting a version of `Dapr.AspNetCore` that is in prerelease, you need to specify the `-Prerelease` flag.</span></span>

1. <span data-ttu-id="bb6ae-231">在 `DaprFrontEnd` 项目中，打开 *Startup.cs* 文件，并将方法替换 `ConfigureServices` 为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-231">In the `DaprFrontEnd` project, open the *Startup.cs* file, and replace the `ConfigureServices` method with the following code:</span></span>

    ```csharp
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddControllers().AddDapr();
        services.AddRazorPages();
    }
    ```

    <span data-ttu-id="bb6ae-232">对的调用会将 `AddDapr` 类注册到 `DaprClient` ASP.NET Core 依赖关系注入系统。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-232">The call to `AddDapr` registers the `DaprClient` class with the ASP.NET Core dependency injection system.</span></span> <span data-ttu-id="bb6ae-233">在注册客户端的情况下，你现在可以将的实例注入 `DaprClient` 到你的服务代码中，以便与 Dapr 挎斗、构建基块和组件通信。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-233">With the client registered, you can now inject an instance of `DaprClient` into your service code to communicate with the Dapr sidecar, building blocks, and components.</span></span>

1. <span data-ttu-id="bb6ae-234">将名为 *WeatherForecast* 的新 c # 类文件添加到 `DaprFrontEnd` 项目：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-234">Add a new C# class file named *WeatherForecast* to the `DaprFrontEnd` project:</span></span>

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

1. <span data-ttu-id="bb6ae-235">打开 *Pages* 文件夹中的 *Index.cshtml.cs* 文件，并将其内容替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-235">Open the *Index.cshtml.cs* file in the *Pages* folder, and replace its contents with the following code:</span></span>

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

    <span data-ttu-id="bb6ae-236">通过将类注入构造函数，可将 Dapr 功能添加到 web 应用中 `DaprClient` `IndexModel` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-236">You add Dapr capabilities into the web app by injecting the `DaprClient` class into `IndexModel` constructor.</span></span> <span data-ttu-id="bb6ae-237">在 `OnGet` 方法中，通过 Dapr 服务调用构建基块调用 API 服务。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-237">In the `OnGet` method, you call the API service with the Dapr service invocation building block.</span></span> <span data-ttu-id="bb6ae-238">`OnGet`每当用户访问主页时，都会调用方法。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-238">The `OnGet` method is invoked whenever a user visits the home page.</span></span> <span data-ttu-id="bb6ae-239">使用 `DaprClient.InvokeMethodAsync` 方法来调用 `weatherforecast` 服务的方法 `daprbackend` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-239">You use the `DaprClient.InvokeMethodAsync` method to invoke the `weatherforecast` method of the `daprbackend` service.</span></span> <span data-ttu-id="bb6ae-240">稍后，将 web API 配置为 `daprbackend` 在将其配置为使用 Dapr 运行时用作其应用程序 ID。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-240">You'll configure the web API to use `daprbackend` as its application ID later on when configuring it to run with Dapr.</span></span> <span data-ttu-id="bb6ae-241">最后，将服务响应保存在查看数据中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-241">Finally, the service response is saved in view data.</span></span>

1. <span data-ttu-id="bb6ae-242">用以下代码替换 *Pages* 文件夹中的 *索引 cshtml* 文件的内容。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-242">Replace the contents of the *Index.cshtml* file in the *Pages* folder, with the following code.</span></span> <span data-ttu-id="bb6ae-243">它会将查看数据中存储的天气预报显示给用户：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-243">It displays the weather forecasts stored in the view data to the user:</span></span>

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

### <a name="add-container-support"></a><span data-ttu-id="bb6ae-244">添加容器支持</span><span class="sxs-lookup"><span data-stu-id="bb6ae-244">Add container support</span></span>

<span data-ttu-id="bb6ae-245">在此示例的最后一部分，你将添加容器支持，并使用 Docker Compose 运行解决方案。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-245">In the final part of this example, you'll add container support and run the solution using Docker Compose.</span></span>

1. <span data-ttu-id="bb6ae-246">右键单击该 `DaprFrontEnd` 项目，然后选择 "**添加**  >  **容器 Orchestrator 支持**"。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-246">Right-click the `DaprFrontEnd` project, and choose **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="bb6ae-247">此时将显示 " **添加容器 Orchestrator 支持** " 对话框：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-247">The **Add Container Orchestrator Support** dialog appears:</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-addorchestrator.png" alt-text="添加容器 orchestrator 支持的屏幕截图":::

    <span data-ttu-id="bb6ae-249">选择“Docker Compose”。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-249">Choose **Docker Compose**.</span></span>

1. <span data-ttu-id="bb6ae-250">在下一个对话框中，选择 " **Linux** " 作为目标操作系统：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-250">In the next dialog, select **Linux** as the Target OS:</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-targetos.png" alt-text="选择 Docker 目标 OS 的屏幕截图":::

    <span data-ttu-id="bb6ae-252">Visual Studio 将在解决方案的 **docker 撰写** 文件夹中创建 *docker-compose.yml docker-compose.override.yml* 文件和 *.dockerignore* 文件：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-252">Visual Studio creates a *docker-compose.yml* file and a *.dockerignore* file in the **docker-compose** folder in the solution:</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-dockersolution.png" alt-text="Docker 合成项目的屏幕截图":::

    <span data-ttu-id="bb6ae-254">*Docker-compose.yml. docker-compose.override.yml* 文件包含以下内容：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-254">The *docker-compose.yml* file has the following content:</span></span>

    ```yaml
    version: "3.4"

    services:
      daprfrontend:
        image: ${DOCKER_REGISTRY-}daprfrontend
        build:
          context: .
          dockerfile: DaprFrontEnd/Dockerfile
    ```

    <span data-ttu-id="bb6ae-255">.dockerignore 文件包含你不希望 Docker 在容器中包含的文件类型和扩展名。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-255">The *.dockerignore* file contains file types and extensions that you don't want Docker to include in the container.</span></span> <span data-ttu-id="bb6ae-256">这些文件与开发环境和源控件相关联，而不是与要部署的应用程序或服务相关联。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-256">These files are associated with the development environment and source control and not the app or service you're deploying.</span></span>

    <span data-ttu-id="bb6ae-257">在 *DaprFrontEnd* 项目目录的根目录中，创建一个新的 *Dockerfile* 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-257">In the root of the *DaprFrontEnd* project directory, a new *Dockerfile* was created.</span></span> <span data-ttu-id="bb6ae-258">*Dockerfile* 是用于生成图像的一系列命令。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-258">A *Dockerfile* is a sequence of commands that are used to build an image.</span></span> <span data-ttu-id="bb6ae-259">有关详细信息，请参阅 [Dockerfile reference](https://docs.docker.com/engine/reference/builder)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-259">For more information, see [Dockerfile reference](https://docs.docker.com/engine/reference/builder).</span></span>

    <span data-ttu-id="bb6ae-260">*Dockerfile* 包含 YAML：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-260">The *Dockerfile* contains the YAML:</span></span>

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

    <span data-ttu-id="bb6ae-261">在调用时，前面的 *Dockerfile* 按顺序执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-261">The preceding *Dockerfile* sequentially performs the following steps when invoked:</span></span>

    1. <span data-ttu-id="bb6ae-262">提取 `mcr.microsoft.com/dotnet/aspnet:3.1` 映像并将其命名 `base` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-262">Pulls the `mcr.microsoft.com/dotnet/aspnet:3.1` image and names it `base`.</span></span>
    1. <span data-ttu-id="bb6ae-263">将工作目录设置为 */app*。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-263">Sets the working directory to */app*.</span></span>
    1. <span data-ttu-id="bb6ae-264">公开端口 `80` 和 `443` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-264">Exposes port `80` and `443`.</span></span>
    1. <span data-ttu-id="bb6ae-265">提取 `mcr.microsoft.com/dotnet/sdk:3.1` 映像并将其命名 `build` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-265">Pulls the `mcr.microsoft.com/dotnet/sdk:3.1` image and names it `build`.</span></span>
    1. <span data-ttu-id="bb6ae-266">将工作目录设置为 */src*。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-266">Sets the working directory to */src*.</span></span>
    1. <span data-ttu-id="bb6ae-267">将 _DaprFrontEnd/DaprFrontEnd_ 复制到名为 *DaprFrontEnd/*. 的新目录中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-267">Copies the _DaprFrontEnd/DaprFrontEnd.csproj_ to a new directory named *DaprFrontEnd/*.</span></span>
    1. <span data-ttu-id="bb6ae-268">对 [`dotnet restore`](../../core/tools/dotnet-restore.md) 该项目的调用。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-268">Calls [`dotnet restore`](../../core/tools/dotnet-restore.md) on the project.</span></span>
    1. <span data-ttu-id="bb6ae-269">将根目录中的所有内容复制到映像的根目录中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-269">Copies everything from the root directory into the image's root.</span></span>
    1. <span data-ttu-id="bb6ae-270">将工作目录设置为 _/src/DaprFrontEnd_。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-270">Sets the working directory to _/src/DaprFrontEnd_.</span></span>
    1. <span data-ttu-id="bb6ae-271">对 [`dotnet build`](../../core/tools/dotnet-build.md) 该项目的调用。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-271">Calls [`dotnet build`](../../core/tools/dotnet-build.md) on the project.</span></span>
        - <span data-ttu-id="bb6ae-272">将 **发布** 配置和输出定位到 */app/build*。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-272">Targeting the **Release** configuration and outputs to */app/build*.</span></span>
    1. <span data-ttu-id="bb6ae-273">从现有基本映像初始化新的生成阶段 `build` 并为其命名 `publish` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-273">Initializes a new build stage from the existing `build` base image and names it `publish`.</span></span>
    1. <span data-ttu-id="bb6ae-274">对 `dotnet publish` 该项目的调用。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-274">Calls `dotnet publish` on the project.</span></span>
        - <span data-ttu-id="bb6ae-275">将 **发布** 配置和输出定位到 */app/publish*。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-275">Targeting the **Release** configuration and outputs to */app/publish*.</span></span>
    1. <span data-ttu-id="bb6ae-276">从现有基本映像初始化新的生成阶段 `publish` 并为其命名 `final` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-276">Initializes a new build stage from the existing `publish` base image and names it `final`.</span></span>
    1. <span data-ttu-id="bb6ae-277">将工作目录设置为 */app*。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-277">Sets the working directory to */app*.</span></span>
    1. <span data-ttu-id="bb6ae-278">将 `/app/publish` 目录从映像复制 `publish` 到映像的根目录 `final` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-278">Copies the `/app/publish` directory from the `publish` image into the root of the `final` image.</span></span>
    1. <span data-ttu-id="bb6ae-279">将入口点设置为的图像 `dotnet` ，并将 `DaprFrontEnd.dll` 作为参数传递。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-279">Sets the entry point as the image to `dotnet` and passes the `DaprFrontEnd.dll` as an arg.</span></span>

1. <span data-ttu-id="bb6ae-280">在 `DaprBackEnd` web API 项目中，右键单击项目节点，然后选择 "**添加**  >  **容器 Orchestrator 支持**"。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-280">In the `DaprBackEnd` web API project, right-click on the project node, and choose **Add** > **Container Orchestrator Support**.</span></span> <span data-ttu-id="bb6ae-281">选择 " **Docker Compose**"，然后再次选择 " **Linux** " 作为 "目标 OS"。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-281">Choose **Docker Compose**, and then select **Linux** again as the target OS.</span></span>

    <span data-ttu-id="bb6ae-282">在 _DaprBackEnd_ 项目目录的根目录中，创建一个新的 *Dockerfile* 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-282">In the root of the _DaprBackEnd_ project directory, a new *Dockerfile* was created.</span></span> <span data-ttu-id="bb6ae-283">*Dockerfile* 包含以下 YAML：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-283">The *Dockerfile* contains the following YAML:</span></span>

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

    <span data-ttu-id="bb6ae-284">再次打开 *docker-compose.yml docker-compose.override.yml* 文件并检查其内容。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-284">Open the *docker-compose.yml* file again and examine its contents.</span></span> <span data-ttu-id="bb6ae-285">Visual Studio 已更新 **Docker Compose** 文件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-285">Visual Studio has updated the **Docker Compose** file.</span></span> <span data-ttu-id="bb6ae-286">现在包括两个服务：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-286">Now both services are included:</span></span>

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

1. <span data-ttu-id="bb6ae-287">若要在容器化应用程序中使用 Dapr 构建基块，需要将 Dapr 分支容器添加到撰写文件中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-287">To use Dapr building blocks from inside a containerized application, you'll need to add the Dapr sidecars containers to your Compose file.</span></span> <span data-ttu-id="bb6ae-288">仔细更新 *docker-compose.yml* 文件的内容，以匹配以下示例。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-288">Carefully update the content of the *docker-compose.yml* file to match the following example.</span></span> <span data-ttu-id="bb6ae-289">请密切注意格式和间距，不要使用制表符。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-289">Pay close attention to the formatting and spacing and don't use tabs.</span></span>

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

    <span data-ttu-id="bb6ae-290">在更新后的文件中，我们 `daprfrontend-dapr` `daprbackend-dapr` 分别为和服务添加并分支 `daprfrontend` `daprbackend` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-290">In the updated file, we've added `daprfrontend-dapr` and `daprbackend-dapr` sidecars for the `daprfrontend` and `daprbackend` services respectively.</span></span> <span data-ttu-id="bb6ae-291">在更新后的文件中，请注意以下更改：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-291">In the updated file, pay close attention to the following changes:</span></span>

    - <span data-ttu-id="bb6ae-292">分支使用 `daprio/daprd:latest` 容器映像。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-292">The sidecars use the `daprio/daprd:latest` container image.</span></span> <span data-ttu-id="bb6ae-293">`latest`对于生产方案，不建议使用标记。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-293">The use of the `latest` tag isn't recommended for production scenarios.</span></span> <span data-ttu-id="bb6ae-294">对于生产，更好的做法是使用特定的版本号。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-294">For production, it's better to use a specific version number.</span></span>
    - <span data-ttu-id="bb6ae-295">在撰写文件中定义的每个服务都有其自己的网络命名空间，以便进行网络隔离。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-295">Each service defined in the Compose file has its own network namespace for network isolation purposes.</span></span> <span data-ttu-id="bb6ae-296">分支用于 `network_mode: "service:..."` 确保它们在与应用程序相同的网络命名空间中运行。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-296">The sidecars use `network_mode: "service:..."` to ensure they run in the same network namespace as the application.</span></span> <span data-ttu-id="bb6ae-297">这样做可以允许挎斗和应用程序使用进行通信 `localhost` 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-297">Doing so allows the sidecar and the application to communicate using `localhost`.</span></span>
    - <span data-ttu-id="bb6ae-298">默认情况下，Dapr 分支侦听 gRPC 通信 (的端口必须公开 50001) ，以允许分支相互通信。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-298">The ports on which the Dapr sidecars are listening for gRPC communication (by default 50001) must be exposed to allow the sidecars to communicate with each other.</span></span>

1. <span data-ttu-id="bb6ae-299"> (<kbd>F5</kbd> 或 <kbd>Ctrl + F5</kbd>) 运行解决方案，验证它是否按预期方式工作。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-299">Run the solution (<kbd>F5</kbd> or <kbd>Ctrl+F5</kbd>) to verify that it works as expected.</span></span> <span data-ttu-id="bb6ae-300">如果所有内容都已正确配置，则应看到天气预报数据：</span><span class="sxs-lookup"><span data-stu-id="bb6ae-300">If everything is configured correctly, you should see the weather forecast data:</span></span>

    :::image type="content" source="./media/getting-started/multicontainer-result.png" alt-text="显示天气预测数据的最终解决方案的屏幕截图":::

    <span data-ttu-id="bb6ae-302">通过 Docker Compose 和 Visual Studio 2019 在本地运行，可以设置断点并调试到应用程序中。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-302">Running locally with Docker Compose and Visual Studio 2019, you can set breakpoints and debug into the application.</span></span> <span data-ttu-id="bb6ae-303">对于生产方案，建议在 Kubernetes 中托管应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-303">For production scenarios, it's recommended to host your application in Kubernetes.</span></span> <span data-ttu-id="bb6ae-304">本书包括随附的引用应用程序 [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr)，其中包含要部署到 Kubernetes 的脚本。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-304">This book includes an accompanying reference application, [eShopOnDapr](https://github.com/dotnet-architecture/eShopOnDapr), that contains scripts to deploy to Kubernetes.</span></span>

    <span data-ttu-id="bb6ae-305">若要了解有关本演练中使用的 Dapr 服务调用构建基块的详细信息，请参阅 [第6章](service-invocation.md)。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-305">To learn more about the Dapr service invocation building block used in this walkthrough, refer to [chapter 6](service-invocation.md).</span></span>

## <a name="summary"></a><span data-ttu-id="bb6ae-306">摘要</span><span class="sxs-lookup"><span data-stu-id="bb6ae-306">Summary</span></span>

<span data-ttu-id="bb6ae-307">在本章中，你有机会 *测试驱动器* Dapr。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-307">In this chapter, you had an opportunity to *test drive* Dapr.</span></span> <span data-ttu-id="bb6ae-308">使用 Dapr .NET SDK，可以看到 Dapr 如何与 .NET 应用程序平台集成。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-308">Using the Dapr .NET SDK, you saw how Dapr integrates with the .NET application platform.</span></span>

<span data-ttu-id="bb6ae-309">第一个示例是一个简单的有状态 .NET 控制台应用程序，它使用了 Dapr 状态管理构建基块。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-309">The first example was a simple, stateful, .NET Console application that used the Dapr state management building block.</span></span>

<span data-ttu-id="bb6ae-310">第二个示例涉及在 Docker 中运行的多容器应用程序。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-310">The second example involved a multi-container application running in Docker.</span></span> <span data-ttu-id="bb6ae-311">通过将 Visual Studio 与 Docker Compose 结合使用，你可以在所有 .NET 应用中使用熟悉的 *F5 调试体验* 。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-311">By using Visual Studio with Docker Compose, you experienced the familiar *F5 debugging experience* available across all .NET apps.</span></span>

<span data-ttu-id="bb6ae-312">你还可以更深入地了解 Dapr 组件配置文件。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-312">You also got a closer look at Dapr component configuration files.</span></span> <span data-ttu-id="bb6ae-313">它们配置 Dapr 构建基块使用的实际基础结构实现。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-313">They configure the actual infrastructure implementation used by the Dapr building blocks.</span></span> <span data-ttu-id="bb6ae-314">你可以使用命名空间和范围限制对特定环境和应用程序的组件访问。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-314">You can use namespaces and scopes to restrict component access to particular environments and applications.</span></span>

<span data-ttu-id="bb6ae-315">在即将推出的章节中，你将深入了解 Dapr 提供的构建基块。</span><span class="sxs-lookup"><span data-stu-id="bb6ae-315">In the upcoming chapters, you'll dive deep into the building blocks offered by Dapr.</span></span>

### <a name="references"></a><span data-ttu-id="bb6ae-316">参考</span><span class="sxs-lookup"><span data-stu-id="bb6ae-316">References</span></span>

- [<span data-ttu-id="bb6ae-317">Dapr 文档-入门</span><span class="sxs-lookup"><span data-stu-id="bb6ae-317">Dapr documentation - Getting started</span></span>](https://docs.dapr.io/getting-started)
- [<span data-ttu-id="bb6ae-318">eShopOnDapr</span><span class="sxs-lookup"><span data-stu-id="bb6ae-318">eShopOnDapr</span></span>](https://github.com/dotnet-architecture/eShopOnDapr)

> [!div class="step-by-step"]
> <span data-ttu-id="bb6ae-319">[上一页](dapr-at-20000-feet.md)
> [下一页](reference-application.md)</span><span class="sxs-lookup"><span data-stu-id="bb6ae-319">[Previous](dapr-at-20000-feet.md)
[Next](reference-application.md)</span></span>
