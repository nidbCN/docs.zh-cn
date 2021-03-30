---
title: 剪裁独立应用程序
description: 了解如何剪裁独立应用以减小其大小。 .NET Core 将运行时与独立发布的应用捆绑在一起，通常包含比所需更多的运行时。
author: jamshedd
ms.author: jamshedd
ms.date: 04/03/2020
ms.openlocfilehash: b5e2650d8240648aa05eaa9026a57b926f63b4de
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872868"
---
# <a name="trim-self-contained-deployments-and-executables"></a><span data-ttu-id="6ebd3-104">剪裁独立部署和可执行文件</span><span class="sxs-lookup"><span data-stu-id="6ebd3-104">Trim self-contained deployments and executables</span></span>

<span data-ttu-id="6ebd3-105">自 .NET 问世以来，[依赖框架的部署模型](index.md#publish-framework-dependent)是最成功的部署模型。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-105">The [framework-dependent deployment model](index.md#publish-framework-dependent) has been the most successful deployment model since the inception of .NET.</span></span> <span data-ttu-id="6ebd3-106">在这种情况下，应用程序开发人员仅将应用程序和第三方程序集捆绑在一起，期望 .NET 运行时库和框架库在客户端计算机中可用。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-106">In this scenario, the application developer bundles only the application and third-party assemblies with the expectation that the .NET runtime and framework libraries will be available in the client machine.</span></span> <span data-ttu-id="6ebd3-107">此部署模型仍是 .NET Core 中的主导模型，但在某些情况下，依赖框架的模型并不是最佳模型。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-107">This deployment model continues to be the dominant one in .NET Core as well but there are some scenarios where the framework-dependent model is not optimal.</span></span> <span data-ttu-id="6ebd3-108">替代方法是发布[自包含的应用程序](index.md#publish-self-contained)，其中 .NET Core 运行时和框架与应用程序和第三方程序集捆绑在一起。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-108">The alternative is to publish a [self-contained application](index.md#publish-self-contained), where the .NET Core runtime and framework are bundled together with the application and third-party assemblies.</span></span>

<span data-ttu-id="6ebd3-109">剪裁自包含部署模型是自包含部署模型的专用版本，该模型已优化以减小部署大小。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-109">The trim-self-contained deployment model is a specialized version of the self-contained deployment model that is optimized to reduce deployment size.</span></span> <span data-ttu-id="6ebd3-110">对于一些客户端场景（如 Blazor 应用程序），最大程度地减小部署大小是很重要的要求。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-110">Minimizing deployment size is a critical requirement for some client-side scenarios like Blazor applications.</span></span> <span data-ttu-id="6ebd3-111">根据应用程序的复杂性，只引用 framework 程序集的子集，运行该应用程序时需要每个程序集中代码的子集。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-111">Depending on the complexity of the application, only a subset of the framework assemblies are referenced, and a subset of the code within each assembly is required to run the application.</span></span> <span data-ttu-id="6ebd3-112">不需要这些未使用的库部分，可从打包的应用程序中进行剪裁。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-112">The unused parts of the libraries are unnecessary and can be trimmed from the packaged application.</span></span>

<span data-ttu-id="6ebd3-113">但是，由于无法可靠地分析各种有问题的代码模式（主要集中在反射使用），应用程序的生成时间分析可能会导致运行时失败。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-113">However, there is a risk that the build time analysis of the application can cause failures at runtime, due to not being able to reliably analyze various problematic code patterns (largely centered on reflection use).</span></span> <span data-ttu-id="6ebd3-114">由于无法保证可靠性，此部署模型仅作为预览功能提供。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-114">Because reliability can't be guaranteed, this deployment model is offered as a preview feature.</span></span>

<span data-ttu-id="6ebd3-115">生成时间分析引擎针对有问题的代码模式向开发人员发出警告，提醒检测所需的其他代码。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-115">The build time analysis engine provides warnings to the developer of code patterns that are problematic to detect which other code is required.</span></span> <span data-ttu-id="6ebd3-116">可使用属性批注代码，以告知裁边器要包含的其他内容。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-116">Code can be annotated with attributes to tell the trimmer what else to include.</span></span> <span data-ttu-id="6ebd3-117">使用[源生成器](https://github.com/dotnet/roslyn/blob/main/docs/features/source-generators.md)可将许多反射模式替换为生成时代码生成。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-117">Many reflection patterns can be replaced with build-time code generation using [Source Generators](https://github.com/dotnet/roslyn/blob/main/docs/features/source-generators.md).</span></span>

<span data-ttu-id="6ebd3-118">使用 `TrimMode` 设置来配置应用程序的剪裁模式。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-118">The trim mode for the applications is configured with the `TrimMode` setting.</span></span> <span data-ttu-id="6ebd3-119">默认值为 `copyused`，并在应用程序中捆绑引用的程序集。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-119">The default value is `copyused` and bundles referenced assemblies with the application.</span></span> <span data-ttu-id="6ebd3-120">`link` 值与 Blazor WebAssembly 应用程序一起使用，并剪裁程序集内未使用的代码。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-120">The `link` value is used with Blazor WebAssembly applications and trims unused code within assemblies.</span></span> <span data-ttu-id="6ebd3-121">如果无法进行完整的依赖项分析，剪裁分析警告会提供代码模式信息。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-121">Trim analysis warnings give information on code patterns where a full dependency analysis was not possible.</span></span> <span data-ttu-id="6ebd3-122">这些警告默认会被取消，可以通过将标志 `SuppressTrimAnalysisWarnings` 设置为 `false` 来启用。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-122">These warnings are suppressed by default and can be turned on by setting the flag `SuppressTrimAnalysisWarnings` to `false`.</span></span> <span data-ttu-id="6ebd3-123">有关可用选项的详细信息，请参阅[剪裁选项](trimming-options.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-123">For more information about the trim options available, see [Trimming options](trimming-options.md).</span></span>

> [!NOTE]
> <span data-ttu-id="6ebd3-124">剪裁是 .NET Core 3.1 和 .NET 5.0 中的实验性功能。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-124">Trimming is an experimental feature in .NET Core 3.1 and .NET 5.0.</span></span> <span data-ttu-id="6ebd3-125">剪裁只能用于独立发布的应用程序。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-125">Trimming is _only_ available to applications that are published self-contained.</span></span>

## <a name="prevent-assemblies-from-being-trimmed"></a><span data-ttu-id="6ebd3-126">防止剪裁程序集</span><span class="sxs-lookup"><span data-stu-id="6ebd3-126">Prevent assemblies from being trimmed</span></span>

<span data-ttu-id="6ebd3-127">在某些情况下，剪裁功能无法检测到引用。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-127">There are scenarios in which the trimming functionality will fail to detect references.</span></span> <span data-ttu-id="6ebd3-128">例如，当应用程序或应用程序引用的库在运行时程序集上使用反射时，都不会直接引用该程序集。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-128">For example, when reflection is used on a runtime assembly, either by your application or a library that is referenced by your application, the assembly isn't directly referenced.</span></span> <span data-ttu-id="6ebd3-129">剪裁不知道这些间接引用，并将从已发布文件夹中排除库。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-129">Trimming is unaware of these indirect references and would exclude the library from the published folder.</span></span>

<span data-ttu-id="6ebd3-130">当代码通过反射间接引用程序集时，可以防止使用 `<TrimmerRootAssembly>` 设置剪裁该程序集。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-130">When the code is indirectly referencing an assembly through reflection, you can prevent the assembly from being trimmed with the `<TrimmerRootAssembly>` setting.</span></span> <span data-ttu-id="6ebd3-131">下面的示例演示如何防止剪裁名为 `System.Security` 程序集的程序集：</span><span class="sxs-lookup"><span data-stu-id="6ebd3-131">The following example shows how to prevent an assembly called `System.Security` assembly from being trimmed:</span></span>

```xml
<ItemGroup>
    <TrimmerRootAssembly Include="System.Security" />
</ItemGroup>
```

### <a name="support-for-ssl-certificates"></a><span data-ttu-id="6ebd3-132">支持 SSL 证书</span><span class="sxs-lookup"><span data-stu-id="6ebd3-132">Support for SSL certificates</span></span>

<span data-ttu-id="6ebd3-133">如果你的应用加载 SSL 证书（如在 ASP.NET Core 应用中），则需要确保在剪裁时阻止剪裁有助于加载 SSL 证书的程序集。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-133">If your app loads SSL certificates, such as in an ASP.NET Core app, you'll want to ensure that when trimming you prevent trimming assemblies that will help with loading SSL certificates.</span></span>

<span data-ttu-id="6ebd3-134">我们可以更新项目文件，使其包含以下 ASP.NET Core 3.1 内容：</span><span class="sxs-lookup"><span data-stu-id="6ebd3-134">We can update our project file to include the following for ASP.NET Core 3.1:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>...</PropertyGroup>
  <!--Include the following for .aspnetcore 3.1-->
  <ItemGroup>
    <TrimmerRootAssembly Include="System.Net" />
    <TrimmerRootAssembly Include="System.Net.Security" />
    <TrimmerRootAssembly Include="System.Security" />
  </ItemGroup>
  ...
</Project>
```

<span data-ttu-id="6ebd3-135">如果使用的是 .Net 5.0，则可以更新项目文件，使其包含以下内容：</span><span class="sxs-lookup"><span data-stu-id="6ebd3-135">If we're using .Net 5.0, we can update our project file to include the following:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
 <PropertyGroup>...</PropertyGroup>
 <!--Include the following for .net 5.0-->
 <ItemGroup>
    <TrimmerRootAssembly Include="System.Net.Security" />
    <TrimmerRootAssembly Include="System.Security" />
  </ItemGroup>
  ...
</Project>
```

## <a name="trim-your-app---cli"></a><span data-ttu-id="6ebd3-136">剪裁应用 - CLI</span><span class="sxs-lookup"><span data-stu-id="6ebd3-136">Trim your app - CLI</span></span>

<span data-ttu-id="6ebd3-137">使用 [dotnet publish](../tools/dotnet-publish.md) 命令剪裁应用程序。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-137">Trim your application using the [dotnet publish](../tools/dotnet-publish.md) command.</span></span> <span data-ttu-id="6ebd3-138">发布应用时，请设置以下属性：</span><span class="sxs-lookup"><span data-stu-id="6ebd3-138">When you publish your app, set the following properties:</span></span>

- <span data-ttu-id="6ebd3-139">对于特定运行时 `-r win-x64`，发布为自包含应用</span><span class="sxs-lookup"><span data-stu-id="6ebd3-139">Publish as a self-contained app for a specific runtime: `-r win-x64`</span></span>
- <span data-ttu-id="6ebd3-140">启用剪裁：`/p:PublishTrimmed=true`</span><span class="sxs-lookup"><span data-stu-id="6ebd3-140">Enable trimming: `/p:PublishTrimmed=true`</span></span>

<span data-ttu-id="6ebd3-141">以下示例将 Windows 应用发布为自包含，并剪裁输出。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-141">The following example publishes an app for Windows as self-contained and trims the output.</span></span>

```xml
<PropertyGroup>
    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
    <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

<span data-ttu-id="6ebd3-142">下面的示例在主动剪裁模式下发布应用，在此模式下，将剪裁程序集中未使用的代码，并启用裁边器警告。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-142">The following example publishes an app in the aggressive trim mode where unused code within assemblies will be trimmed and trimmer warnings enabled.</span></span>

```xml
<PropertyGroup>
    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
    <PublishTrimmed>true</PublishTrimmed>
    <TrimMode>link</TrimMode>
    <SuppressTrimAnalysisWarnings>false</SuppressTrimAnalysisWarnings>
</PropertyGroup>
```

<span data-ttu-id="6ebd3-143">有关详细信息，请参阅[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-143">For more information, see [Publish .NET Core apps with .NET Core CLI](deploy-with-cli.md).</span></span>

## <a name="trim-your-app---visual-studio"></a><span data-ttu-id="6ebd3-144">剪裁应用 - Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6ebd3-144">Trim your app - Visual Studio</span></span>

<span data-ttu-id="6ebd3-145">Visual Studio 创建可重用的发布配置文件，用于控制应用程序的发布方式。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-145">Visual Studio creates reusable publishing profiles that control how your application is published.</span></span>

01. <span data-ttu-id="6ebd3-146">在“解决方案资源管理器”窗格中，右键单击要发布的项目  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-146">On the **Solution Explorer** pane, right-click on the project you want to publish.</span></span> <span data-ttu-id="6ebd3-147">选择“发布…”  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-147">Select **Publish...**.</span></span>

    :::image type="content" source="media/trim-self-contained/visual-studio-solution-explorer.png" alt-text="解决方案资源管理器，其中右键单击菜单突出显示了“发布”选项。":::

    <span data-ttu-id="6ebd3-149">如果还没有发布配置文件，请按照说明创建一个并选择“文件夹”目标类型  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-149">If you don't already have a publishing profile, follow the instructions to create one and choose the **Folder** target-type.</span></span>

01. <span data-ttu-id="6ebd3-150">选择“编辑”  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-150">Choose **Edit**.</span></span>

    :::image type="content" source="media/trim-self-contained/visual-studio-publish-edit-settings.png" alt-text="带有“编辑”按钮的 Visual Studio 发布配置文件。":::

01. <span data-ttu-id="6ebd3-152">在“配置文件设置”对话框中，设置以下选项  ：</span><span class="sxs-lookup"><span data-stu-id="6ebd3-152">In the **Profile settings** dialog, set the following options:</span></span>

    - <span data-ttu-id="6ebd3-153">将“部署模式”设置为“自包含”   。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-153">Set **Deployment mode** to **Self-contained**.</span></span>
    - <span data-ttu-id="6ebd3-154">将“目标运行时”设置为要发布到的平台  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-154">Set **Target runtime** to the platform you want to publish to.</span></span>
    - <span data-ttu-id="6ebd3-155">选择“剪裁未使用的程序集(预览版)”  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-155">Select **Trim unused assemblies (in preview)**.</span></span>

    <span data-ttu-id="6ebd3-156">选择“保存”保存设置并返回到“发布”对话框   。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-156">Choose **Save** to save the settings and return to the **Publish** dialog.</span></span>

    :::image type="content" source="media/trim-self-contained/visual-studio-publish-properties.png" alt-text="“配置文件设置”对话框，其中突出显示了“部署模式”、“目标运行时”和“剪裁未使用的程序集”选项。":::

01. <span data-ttu-id="6ebd3-158">选择“发布”以发布剪裁过的应用  。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-158">Choose **Publish** to publish your app trimmed.</span></span>

<span data-ttu-id="6ebd3-159">有关详细信息，请参阅[使用 Visual Studio 发布 .NET Core 应用](deploy-with-vs.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-159">For more information, see [Publish .NET Core apps with Visual Studio](deploy-with-vs.md).</span></span>

## <a name="trim-your-app---visual-studio-for-mac"></a><span data-ttu-id="6ebd3-160">剪裁应用 - Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="6ebd3-160">Trim your app - Visual Studio for Mac</span></span>

<span data-ttu-id="6ebd3-161">Visual Studio for Mac 不提供在发布期间剪裁应用的选项。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-161">Visual Studio for Mac doesn't provide options to trim your app during publish.</span></span> <span data-ttu-id="6ebd3-162">你需要按照[剪裁应用 - CLI](#trim-your-app---cli) 部分的说明手动发布。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-162">You'll need to publish manually by following the instructions from the [Trim your app - CLI](#trim-your-app---cli) section.</span></span> <span data-ttu-id="6ebd3-163">有关详细信息，请参阅[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-163">For more information, see [Publish .NET Core apps with .NET Core CLI](deploy-with-cli.md).</span></span>

## <a name="see-also"></a><span data-ttu-id="6ebd3-164">请参阅</span><span class="sxs-lookup"><span data-stu-id="6ebd3-164">See also</span></span>

- <span data-ttu-id="6ebd3-165">[.NET Core 应用程序部署](index.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-165">[.NET Core application deployment](index.md).</span></span>
- <span data-ttu-id="6ebd3-166">[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-166">[Publish .NET Core apps with .NET Core CLI](deploy-with-cli.md).</span></span>
- <span data-ttu-id="6ebd3-167">[使用 Visual Studio 发布 .NET Core 应用](deploy-with-vs.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-167">[Publish .NET Core apps with Visual Studio](deploy-with-vs.md).</span></span>
- <span data-ttu-id="6ebd3-168">[dotnet publish 命令](../tools/dotnet-publish.md)。</span><span class="sxs-lookup"><span data-stu-id="6ebd3-168">[dotnet publish command](../tools/dotnet-publish.md).</span></span>
