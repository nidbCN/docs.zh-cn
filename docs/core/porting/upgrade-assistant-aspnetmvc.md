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
# <a name="upgrade-an-aspnet-mvc-app-to-net-5-with-the-net-upgrade-assistant"></a>使用 .NET 升级助手将 ASP.NET MVC 应用升级到 .NET 5

[.NET 升级助手](upgrade-assistant-overview.md)是一种命令行工具，可帮助将 .NET Framework ASP.NET MVC 应用升级到 .NET 5。 本文提供以下内容：

- 演示如何针对 .NET Framework ASP.NET MVC 应用运行该工具
- 故障排除提示

## <a name="upgrade-net-framework-aspnet-mvc-apps"></a>升级 .NET Framework ASP.NET MVC 应用

本部分演示如何针对新创建的面向 .NET Framework 4.6.1 的 ASP.NET MVC 应用运行 NET 升级助手。 若要详细了解如何安装此工具，请查看 [.NET 升级助手概述](upgrade-assistant-overview.md)。

### <a name="initial-demo-setup"></a>初始演示设置

如果你要针对你自己的 .NET Framework 应用运行 .NET 升级助手，可跳过此步骤。 如果你只想试用一下来看看它的工作原理，可在此步骤中了解如何设置示例 ASP.NET MVC 和 Web API (.NET Framework) 项目以供使用。

借助 Visual Studio，使用 .NET Framework 创建一个新的 ASP.NET Web 应用程序项目。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/new-project.png" alt-text="在 Visual Studio 中新建 ASP.NET Web 应用程序项目":::

将项目命名为 AspNetMvcTest。 将项目配置为使用 .NET Framework 4.6.1。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/configure-project.png" alt-text="在 Visual Studio 中配置 ASP.NET 项目":::

在下一对话框中，选择“MVC”应用程序，然后选择“创建” 。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/create-mvc-webapi.png" alt-text="在 Visual Studio 中创建 ASP.NET MVC 项目":::

查看所创建的项目及其文件，尤其是它的项目文件。

### <a name="run-upgrade-assistant"></a>运行升级助手

打开终端，导航到目标项目或解决方案所在的文件夹。 运行 `upgrade-assistant` 命令，传入你要针对的项目的名称（可从任意位置运行该命令，只要项目文件的路径有效就行）。

```console
upgrade-assistant .\AspNetMvcTest.csproj
```

该工具将运行并显示它将执行的步骤列表。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/initial-run.png" alt-text=".NET 升级助手初始屏幕":::

完成每个步骤后，该工具都会提供一组命令，用户可应用这些命令，也可跳过下一步骤、查看更多详细信息、配置日志记录或退出该过程。 如果该工具检测到某个步骤将不执行任何操作，它会自动跳过该步骤，转到下一步骤，直到到达有要执行的操作的步骤为止。 如果未进行其他任何选择，那么按 Enter 将执行下一步<kbd></kbd>。

在此示例中，每次都会选择“应用”步骤。 第一步是备份项目。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/backup-project.png" alt-text=".NET 升级助手备份项目":::

该工具会提示输入自定义路径进行备份或使用默认路径，后者会将项目备份放在具有 `.backup` 扩展名的同一文件夹中。 此工具接下来做的是将项目文件转换为 SDK 样式。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/convert-project.png" alt-text=".NET 升级助手将项目转换为 SDK 样式":::

更新项目格式后，下一步是更新项目的 TFM。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/update-tfm.png" alt-text=".NET 升级助手更新 TFM":::

接下来，该工具会更新项目的 NuGet 包。 多个包需要更新，且会添加一个新的分析器包。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/update-nuget-packages.png" alt-text=".NET 升级助手更新 NuGet 包":::

更新包后，接下来是添加模板文件（如果有）。 该工具指示有 4 个必须添加的预期模板项，随后它会添加这些项。 其中包括以下文件：

- `Program.cs`
- `Startup.cs`
- `appsettings.json`
- `appsettings.Development.json`

ASP.NET Core 会使用这些文件来进行[应用启动](/aspnet/core/fundamentals/startup)和[配置](/aspnet/core/fundamentals/configuration)。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/add-template-files.png" alt-text=".NET 升级助手添加模板文件":::

接下来，该工具会迁移配置文件。 该工具会标识应用设置并禁用不受支持的配置部分，然后迁移 `appSettings` 配置值。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/migrate-config.png" alt-text=".NET 升级助手迁移配置":::

该工具通过迁移 `system.web.webPages.razor/pages/namespaces` 来完成配置文件的迁移。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/migrate-config2.png" alt-text=".NET 升级助手迁移配置已完成":::

该工具会应用已知的修补程序来将 C# 引用迁移到其新的对应项。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/update-csharp.png" alt-text=".NET 升级助手更新 C# 源":::

这是最后一个项目，因此下一步是“移动到新的项目”，它提示完成迁移整个解决方案的过程。

:::image type="content" source="media/upgrade-assistant-aspnetmvc/complete-solution.png" alt-text=".NET 升级助手完成解决方案":::

完成此过程后，打开项目文件并进行查看。 查找静态文件，如下所示：

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

该由 Web 服务器处理的静态文件应移动到名为 `wwwroot` 的根级别文件夹下适当的文件夹中。 有关详细信息，请查看 [ASP.NET Core 中的静态文件](/aspnet/core/fundamentals/static-files?view=aspnetcore-5.0&preserve-view=true) 。 移动文件后，可删除项目文件中与这些文件对应的 `<Content>` 元素。 事实上，可删除所有 `<Content>` 元素及其包含组。 此外，应删除指向客户端库（如 `bootstrap` 或 `jQuery`）的所有 `<PackageReference>`。

默认情况下，项目将被转换为类库。 请将第一行的 `Sdk` 属性更改为 `Microsoft.NET.Sdk.Web`，并将 `<TargetFramework>` 设置为 `net5.0`。 编译该项目。 此时，错误数应当相当小。 在移植新的 ASP.NET 4.6.1 MVC 项目时，其余错误引用 `App_Start` 文件夹中的文件：

- BundleConfig.cs
- FilterConfig.cs
- RouteConfig.cs

可删除这些文件和完整的 `App_Start` 文件夹。 同样，可删除 `Global.asax` 和 `Global.asax.cs` 文件。

此时，只剩下与捆绑相关的错误。 可[通过多种方式在 SP.NET Core 中配置捆绑和缩减](/aspnet/core/migration/mvc?view=aspnetcore-5.0&preserve-view=true#configure-bundling-and-minification)。 选择最适合你的项目的任何内容。

## <a name="troubleshooting-tips"></a>故障排除提示

使用 .NET 升级助手时，可能会出现一些已知问题。 某些情况下，.NET 升级助手在内部使用的 [try-convert 工具](https://github.com/dotnet/try-convert)会出现问题。 此工具会频繁更新来处理更多场景，因此请务必使用最新版本。

- 必须安装 try-convert 并将其更新到不低于 0.7.212201 版本。
- 更低版本的 try-convert 工具不支持自定义目标或属性文件。 如果无法升级到最新版，则可能需要手动处理这些问题。 如果目标项目文件包含对自定义目标或属性文件的引用，则在对其运行 .NET 升级助手之前，可能需要从文件中手动删除这些引用。

```xml
<Import Project="packages\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.2.0.1\build\net46\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.props" Condition="Exists('packages\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.2.0.1\build\net46\Microsoft.CodeDom.Providers.DotNetCompilerPlatform.props')" />
```

有关更多故障排除提示和已知问题，可查看[此工具的 GitHub 存储库](https://github.com/dotnet/upgrade-assistant#troubleshooting-common-issues)。

## <a name="see-also"></a>另请参阅

- [使用 .NET 升级助手将 WPF 应用升级到 .NET 5](upgrade-assistant-wpf-framework.md)
- [使用 .NET 升级助手将 Windows 窗体应用升级到 .NET 5](upgrade-assistant-winforms-framework.md)
- [.NET 升级助手概述](upgrade-assistant-overview.md)
- [.NET 升级助手 GitHub 存储库](https://github.com/dotnet/upgrade-assistant)
