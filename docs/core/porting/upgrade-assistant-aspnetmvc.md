---
title: 将 ASP.NET MVC 应用升级到 .NET 5
description: 使用 .NET 升级助手将现有的 .NET Framework ASP.NET MVC 应用升级到 .NET 5。 .NET 升级助手是一种 CLI 工具，可帮助将应用从 .NET Framework 迁移到 .NET 5。
author: ardalis
ms.date: 03/08/2021
ms.openlocfilehash: 421d8ce16bc1800451ee39c20c4746ea321fafd0
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604952"
---
# <a name="upgrade-an-aspnet-mvc-app-to-net-5-with-the-net-upgrade-assistant"></a><span data-ttu-id="ed4e9-104">使用 .NET 升级助手将 ASP.NET MVC 应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="ed4e9-104">Upgrade an ASP.NET MVC App to .NET 5 with the .NET Upgrade Assistant</span></span>

<span data-ttu-id="ed4e9-105">[.NET 升级助手](upgrade-assistant-overview.md)是一种命令行工具，可帮助将 .NET Framework ASP.NET MVC 应用升级到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-105">The [.NET Upgrade Assistant](upgrade-assistant-overview.md) is a command-line tool that can assist with upgrading .NET Framework ASP.NET MVC apps to .NET 5.</span></span> <span data-ttu-id="ed4e9-106">本文提供以下内容：</span><span class="sxs-lookup"><span data-stu-id="ed4e9-106">This article provides:</span></span>

- <span data-ttu-id="ed4e9-107">演示如何针对 .NET Framework ASP.NET MVC 应用运行该工具</span><span class="sxs-lookup"><span data-stu-id="ed4e9-107">A demonstration of how to run the tool against a .NET Framework ASP.NET MVC app</span></span>
- <span data-ttu-id="ed4e9-108">故障排除提示</span><span class="sxs-lookup"><span data-stu-id="ed4e9-108">Troubleshooting tips</span></span>

## <a name="upgrade-net-framework-aspnet-mvc-apps"></a><span data-ttu-id="ed4e9-109">升级 .NET Framework ASP.NET MVC 应用</span><span class="sxs-lookup"><span data-stu-id="ed4e9-109">Upgrade .NET Framework ASP.NET MVC apps</span></span>

<span data-ttu-id="ed4e9-110">本部分演示如何针对新创建的面向 .NET Framework 4.6.1 的 ASP.NET MVC 应用运行 NET 升级助手。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-110">This section demonstrates running the .NET Upgrade Assistant against a newly created ASP.NET MVC app targeting .NET Framework 4.6.1.</span></span> <span data-ttu-id="ed4e9-111">若要详细了解如何安装此工具，请查看 [.NET 升级助手概述](upgrade-assistant-overview.md)。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-111">For more information on how to install the tool, see [Overview of the .NET Upgrade Assistant](upgrade-assistant-overview.md).</span></span>

### <a name="initial-demo-setup"></a><span data-ttu-id="ed4e9-112">初始演示设置</span><span class="sxs-lookup"><span data-stu-id="ed4e9-112">Initial demo setup</span></span>

<span data-ttu-id="ed4e9-113">如果你要针对你自己的 .NET Framework 应用运行 .NET 升级助手，可跳过此步骤。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-113">If you're running the .NET Upgrade Assistant against your own .NET Framework app, you can skip this step.</span></span> <span data-ttu-id="ed4e9-114">如果你只想试用一下来看看它的工作原理，可在此步骤中了解如何设置示例 ASP.NET MVC 和 Web API (.NET Framework) 项目以供使用。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-114">If you just want to try it out to see how it works, this step shows you how to set up a sample ASP.NET MVC and Web API (.NET Framework) project to use.</span></span>

<span data-ttu-id="ed4e9-115">借助 Visual Studio，使用 .NET Framework 创建一个新的 ASP.NET Web 应用程序项目。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-115">Using Visual Studio, create a new ASP.NET Web Application project using .NET Framework.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/new-project.png" alt-text="在 Visual Studio 中新建 ASP.NET Web 应用程序项目":::

<span data-ttu-id="ed4e9-117">将项目命名为 AspNetMvcTest。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-117">Name the project **AspNetMvcTest**.</span></span> <span data-ttu-id="ed4e9-118">将项目配置为使用 .NET Framework 4.6.1。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-118">Configure the project to use **.NET Framework 4.6.1**.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/configure-project.png" alt-text="在 Visual Studio 中配置 ASP.NET 项目":::

<span data-ttu-id="ed4e9-120">在下一对话框中，选择“MVC”应用程序，然后选择“创建” 。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-120">In the next dialog, choose **MVC** application, then select **Create**.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/create-mvc-webapi.png" alt-text="在 Visual Studio 中创建 ASP.NET MVC 项目":::

<span data-ttu-id="ed4e9-122">查看所创建的项目及其文件，尤其是它的项目文件。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-122">Review the created project and its files, especially its project file(s).</span></span>

### <a name="run-upgrade-assistant"></a><span data-ttu-id="ed4e9-123">运行升级助手</span><span class="sxs-lookup"><span data-stu-id="ed4e9-123">Run upgrade-assistant</span></span>

<span data-ttu-id="ed4e9-124">打开终端，导航到目标项目或解决方案所在的文件夹。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-124">Open a terminal and navigate to the folder where the target project or solution is located.</span></span> <span data-ttu-id="ed4e9-125">运行 `upgrade-assistant` 命令，传入你要针对的项目的名称（可从任意位置运行该命令，只要项目文件的路径有效就行）。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-125">Run the `upgrade-assistant` command, passing in the name of the project you're targeting (you can run the command from anywhere, as long as the path to the project file is valid).</span></span>

```console
upgrade-assistant .\AspNetMvcTest.csproj
```

<span data-ttu-id="ed4e9-126">该工具将运行并显示它将执行的步骤列表。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-126">The tool runs and shows you a list of the steps it will do.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/initial-run.png" alt-text=".NET 升级助手初始屏幕":::

<span data-ttu-id="ed4e9-128">完成每个步骤后，该工具都会提供一组命令，用户可应用这些命令，也可跳过下一步骤、查看更多详细信息、配置日志记录或退出该过程。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-128">As each step is completed, the tool provides a set of commands allowing the user to apply or skip the next step, see more details, configure logging, or exit the process.</span></span> <span data-ttu-id="ed4e9-129">如果该工具检测到某个步骤将不执行任何操作，它会自动跳过该步骤，转到下一步骤，直到到达有要执行的操作的步骤为止。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-129">If the tool detects that a step will perform no actions, it automatically skips that step and continues to the next step until it reaches one that has actions to perform.</span></span> <span data-ttu-id="ed4e9-130">如果未进行其他任何选择，那么按 Enter 将执行下一步<kbd></kbd>。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-130">Pressing <kbd>Enter</kbd> will perform the next step if no other selection is made.</span></span>

<span data-ttu-id="ed4e9-131">在此示例中，每次都会选择“应用”步骤。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-131">In this example, the apply step is chosen each time.</span></span> <span data-ttu-id="ed4e9-132">第一步是备份项目。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-132">The first step is to back up the project.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/backup-project.png" alt-text=".NET 升级助手备份项目":::

<span data-ttu-id="ed4e9-134">该工具会提示输入自定义路径进行备份或使用默认路径，后者会将项目备份放在具有 `.backup` 扩展名的同一文件夹中。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-134">The tool prompts for a custom path for the backup, or to use the default, which will place the project backup in the same folder with a `.backup` extension.</span></span> <span data-ttu-id="ed4e9-135">此工具接下来做的是将项目文件转换为 SDK 样式。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-135">The next step the tool does is to convert the project file to SDK style.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/convert-project.png" alt-text=".NET 升级助手将项目转换为 SDK 样式":::

<span data-ttu-id="ed4e9-137">更新项目格式后，下一步是更新项目的 TFM。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-137">Once the project format has been updated, the next step is to update the TFM of the project.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/update-tfm.png" alt-text=".NET 升级助手更新 TFM":::

<span data-ttu-id="ed4e9-139">接下来，该工具会更新项目的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-139">Next, the tool updates the project's NuGet packages.</span></span> <span data-ttu-id="ed4e9-140">多个包需要更新，且会添加一个新的分析器包。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-140">Several packages need updates, and a new analyzer package is added.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/update-nuget-packages.png" alt-text=".NET 升级助手更新 NuGet 包":::

<span data-ttu-id="ed4e9-142">更新包后，接下来是添加模板文件（如果有）。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-142">Once the packages are updated, the next step is to add template files, if any.</span></span> <span data-ttu-id="ed4e9-143">该工具指示有 4 个必须添加的预期模板项，随后它会添加这些项。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-143">The tool notes there are four expected template items that must be added, and then adds them.</span></span> <span data-ttu-id="ed4e9-144">其中包括以下文件：</span><span class="sxs-lookup"><span data-stu-id="ed4e9-144">These include the following files:</span></span>

- `Program.cs`
- `Startup.cs`
- `appsettings.json`
- `appsettings.Development.json`

<span data-ttu-id="ed4e9-145">ASP.NET Core 会使用这些文件来进行[应用启动](/aspnet/core/fundamentals/startup)和[配置](/aspnet/core/fundamentals/configuration)。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-145">These files are used by ASP.NET Core for [app startup](/aspnet/core/fundamentals/startup) and [configuration](/aspnet/core/fundamentals/configuration).</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/add-template-files.png" alt-text=".NET 升级助手添加模板文件":::

<span data-ttu-id="ed4e9-147">接下来，该工具会迁移配置文件。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-147">Next, the tool migrates config files.</span></span> <span data-ttu-id="ed4e9-148">该工具会标识应用设置并禁用不受支持的配置部分，然后迁移 `appSettings` 配置值。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-148">The tool identifies app settings and disables unsupported configuration sections, then migrates the `appSettings` configuration values.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/migrate-config.png" alt-text=".NET 升级助手迁移配置":::

<span data-ttu-id="ed4e9-150">该工具通过迁移 `system.web.webPages.razor/pages/namespaces` 来完成配置文件的迁移。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-150">The tool completes the migration of config files by migrating `system.web.webPages.razor/pages/namespaces`.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/migrate-config2.png" alt-text=".NET 升级助手迁移配置已完成":::

<span data-ttu-id="ed4e9-152">该工具会应用已知的修补程序来将 C# 引用迁移到其新的对应项。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-152">The tool applies known fixes to migrate C# references to their new counterparts.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/update-csharp.png" alt-text=".NET 升级助手更新 C# 源":::

<span data-ttu-id="ed4e9-154">这是最后一个项目，因此下一步是“移动到新的项目”，它提示完成迁移整个解决方案的过程。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-154">Since this is the last project, the next step, "Move to next project", prompts to complete the process of migrating the entire solution.</span></span>

:::image type="content" source="media/upgrade-assistant-aspnetmvc/complete-solution.png" alt-text=".NET 升级助手完成解决方案":::

<span data-ttu-id="ed4e9-156">完成此过程后，打开项目文件并进行查看。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-156">Once this process has completed, open the project file and review it.</span></span> <span data-ttu-id="ed4e9-157">查找静态文件，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed4e9-157">Look for static files like these:</span></span>

```xml
  <ItemGroup>
    <Content Include="fonts\glyphicons-halflings-regular.woff2" />
    <Content Include="fonts\glyphicons-halflings-regular.woff" />
    <Content Include="fonts\glyphicons-halflings-regular.ttf" />
    <Content Include="fonts\glyphicons-halflings-regular.eot" />
    <Content Include="Content\bootstrap.min.css.map" />
    <Content Include="Content\bootstrap.css.map" />
    <Content Include="Content\bootstrap-theme.min.css.map" />
    <Content Include="Content\bootstrap-theme.css.map" />
    <Content Include="Scripts\jquery-3.4.1.slim.min.map" />
    <Content Include="Scripts\jquery-3.4.1.min.map" />
  </ItemGroup>
```

<span data-ttu-id="ed4e9-158">该由 Web 服务器处理的静态文件应移动到名为 `wwwroot` 的根级别文件夹下适当的文件夹中。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-158">Static files that should be served by the web server should be moved to an appropriate folder within a root level folder named `wwwroot`.</span></span> <span data-ttu-id="ed4e9-159">有关详细信息，请查看 [ASP.NET Core 中的静态文件](/aspnet/core/fundamentals/static-files?view=aspnetcore-5.0&preserve-view=true) 。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-159">See [Static files in ASP.NET Core](/aspnet/core/fundamentals/static-files?view=aspnetcore-5.0&preserve-view=true) for details.</span></span> <span data-ttu-id="ed4e9-160">移动文件后，可删除项目文件中与这些文件对应的 `<Content>` 元素。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-160">Once the files have been moved, the `<Content>` elements in the project file corresponding to these files can be deleted.</span></span> <span data-ttu-id="ed4e9-161">事实上，可删除所有 `<Content>` 元素及其包含组。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-161">In fact, all `<Content>` elements and their containing groups can be removed.</span></span> <span data-ttu-id="ed4e9-162">此外，应删除指向客户端库（如 `bootstrap` 或 `jQuery`）的所有 `<PackageReference>`。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-162">Also, any `<PackageReference>` to a client-side library like `bootstrap` or `jQuery` should be removed.</span></span>

<span data-ttu-id="ed4e9-163">默认情况下，项目将被转换为类库。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-163">By default, the project will be converted as a class library.</span></span> <span data-ttu-id="ed4e9-164">请将第一行的 `Sdk` 属性更改为 `Microsoft.NET.Sdk.Web`，并将 `<TargetFramework>` 设置为 `net5.0`。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-164">Change the first line's `Sdk` attribute to `Microsoft.NET.Sdk.Web` and set the `<TargetFramework>` to `net5.0`.</span></span> <span data-ttu-id="ed4e9-165">编译该项目。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-165">Compile the project.</span></span> <span data-ttu-id="ed4e9-166">此时，错误数应当相当小。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-166">At this point, the number of errors should be fairly small.</span></span> <span data-ttu-id="ed4e9-167">在移植新的 ASP.NET 4.6.1 MVC 项目时，其余错误引用 `App_Start` 文件夹中的文件：</span><span class="sxs-lookup"><span data-stu-id="ed4e9-167">When porting a new ASP.NET 4.6.1 MVC project, the remaining errors refer to files in the `App_Start` folder:</span></span>

- <span data-ttu-id="ed4e9-168">BundleConfig.cs</span><span class="sxs-lookup"><span data-stu-id="ed4e9-168">BundleConfig.cs</span></span>
- <span data-ttu-id="ed4e9-169">FilterConfig.cs</span><span class="sxs-lookup"><span data-stu-id="ed4e9-169">FilterConfig.cs</span></span>
- <span data-ttu-id="ed4e9-170">RouteConfig.cs</span><span class="sxs-lookup"><span data-stu-id="ed4e9-170">RouteConfig.cs</span></span>

<span data-ttu-id="ed4e9-171">可删除这些文件和完整的 `App_Start` 文件夹。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-171">These files - and the entire `App_Start` folder - can be deleted.</span></span> <span data-ttu-id="ed4e9-172">同样，可删除 `Global.asax` 和 `Global.asax.cs` 文件。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-172">Likewise, the `Global.asax` and `Global.asax.cs` files can be removed.</span></span>

<span data-ttu-id="ed4e9-173">此时，只剩下与捆绑相关的错误。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-173">At this point the only errors that remain are related to bundling.</span></span> <span data-ttu-id="ed4e9-174">可[通过多种方式在 SP.NET Core 中配置捆绑和缩减](/aspnet/core/migration/mvc?view=aspnetcore-5.0&preserve-view=true#configure-bundling-and-minification)。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-174">There are [several ways to configure bundling and minification in ASP.NET Core](/aspnet/core/migration/mvc?view=aspnetcore-5.0&preserve-view=true#configure-bundling-and-minification).</span></span> <span data-ttu-id="ed4e9-175">选择最适合你的项目的任何内容。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-175">Choose whatever makes the most sense for your project.</span></span>

## <a name="troubleshooting-tips"></a><span data-ttu-id="ed4e9-176">故障排除提示</span><span class="sxs-lookup"><span data-stu-id="ed4e9-176">Troubleshooting tips</span></span>

<span data-ttu-id="ed4e9-177">使用 .NET 升级助手时，可能会出现一些已知问题。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-177">There are several known problems that can occur when using the .NET Upgrade Assistant.</span></span> <span data-ttu-id="ed4e9-178">某些情况下，.NET 升级助手在内部使用的 [try-convert 工具](https://github.com/dotnet/try-convert)会出现问题。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-178">In some cases, these are problems with the [try-convert tool](https://github.com/dotnet/try-convert) that the .NET Upgrade Assistant uses internally.</span></span> <span data-ttu-id="ed4e9-179">此工具会频繁更新来处理更多场景，因此请务必使用最新版本。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-179">This tool is being frequently updated to address more scenarios, so make sure you're using a recent version.</span></span>

- <span data-ttu-id="ed4e9-180">必须安装 try-convert 并将其更新到不低于 0.7.212201 版本。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-180">The **try-convert** tool must be installed and updated to at least version _0.7.212201_.</span></span>
- <span data-ttu-id="ed4e9-181">更低版本的 try-convert 工具不支持自定义目标或属性文件。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-181">Earlier versions of the **try-convert** tool didn't support custom target or props files.</span></span> <span data-ttu-id="ed4e9-182">如果无法升级到最新版，则可能需要手动处理这些问题。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-182">If you can't upgrade to the latest version, you may need to manually address these issues.</span></span> <span data-ttu-id="ed4e9-183">如果目标项目文件包含对自定义目标或属性文件的引用，则在对其运行 .NET 升级助手之前，可能需要从文件中手动删除这些引用。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-183">If the target project file includes references to custom targets or props files, these references may need to be manually deleted from the file before the .NET Upgrade Assistant is run against it.</span></span>

```xml
<Import Project="packages\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.2.0.1\build\net46\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.props" Condition="Exists('packages\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.2.0.1\build\net46\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.props')" />
```

<span data-ttu-id="ed4e9-184">有关更多故障排除提示和已知问题，可查看[此工具的 GitHub 存储库](https://github.com/dotnet/upgrade-assistant#troubleshooting-common-issues)。</span><span class="sxs-lookup"><span data-stu-id="ed4e9-184">[The tool's GitHub repository](https://github.com/dotnet/upgrade-assistant#troubleshooting-common-issues) has more troubleshooting tips and known issues.</span></span>

## <a name="see-also"></a><span data-ttu-id="ed4e9-185">另请参阅</span><span class="sxs-lookup"><span data-stu-id="ed4e9-185">See also</span></span>

- [<span data-ttu-id="ed4e9-186">使用 .NET 升级助手将 WPF 应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="ed4e9-186">Upgrade a WPF App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-wpf-framework.md)
- [<span data-ttu-id="ed4e9-187">使用 .NET 升级助手将 Windows 窗体应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="ed4e9-187">Upgrade a Windows Forms App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-winforms-framework.md)
- [<span data-ttu-id="ed4e9-188">.NET 升级助手概述</span><span class="sxs-lookup"><span data-stu-id="ed4e9-188">Overview of the .NET Upgrade Assistant</span></span>](upgrade-assistant-overview.md)
- [<span data-ttu-id="ed4e9-189">.NET 升级助手 GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="ed4e9-189">.NET Upgrade Assistant GitHub Repository</span></span>](https://github.com/dotnet/upgrade-assistant)
