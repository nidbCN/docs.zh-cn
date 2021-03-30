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
# <a name="trim-self-contained-deployments-and-executables"></a>剪裁独立部署和可执行文件

自 .NET 问世以来，[依赖框架的部署模型](index.md#publish-framework-dependent)是最成功的部署模型。 在这种情况下，应用程序开发人员仅将应用程序和第三方程序集捆绑在一起，期望 .NET 运行时库和框架库在客户端计算机中可用。 此部署模型仍是 .NET Core 中的主导模型，但在某些情况下，依赖框架的模型并不是最佳模型。 替代方法是发布[自包含的应用程序](index.md#publish-self-contained)，其中 .NET Core 运行时和框架与应用程序和第三方程序集捆绑在一起。

剪裁自包含部署模型是自包含部署模型的专用版本，该模型已优化以减小部署大小。 对于一些客户端场景（如 Blazor 应用程序），最大程度地减小部署大小是很重要的要求。 根据应用程序的复杂性，只引用 framework 程序集的子集，运行该应用程序时需要每个程序集中代码的子集。 不需要这些未使用的库部分，可从打包的应用程序中进行剪裁。

但是，由于无法可靠地分析各种有问题的代码模式（主要集中在反射使用），应用程序的生成时间分析可能会导致运行时失败。 由于无法保证可靠性，此部署模型仅作为预览功能提供。

生成时间分析引擎针对有问题的代码模式向开发人员发出警告，提醒检测所需的其他代码。 可使用属性批注代码，以告知裁边器要包含的其他内容。 使用[源生成器](https://github.com/dotnet/roslyn/blob/main/docs/features/source-generators.md)可将许多反射模式替换为生成时代码生成。

使用 `TrimMode` 设置来配置应用程序的剪裁模式。 默认值为 `copyused`，并在应用程序中捆绑引用的程序集。 `link` 值与 Blazor WebAssembly 应用程序一起使用，并剪裁程序集内未使用的代码。 如果无法进行完整的依赖项分析，剪裁分析警告会提供代码模式信息。 这些警告默认会被取消，可以通过将标志 `SuppressTrimAnalysisWarnings` 设置为 `false` 来启用。 有关可用选项的详细信息，请参阅[剪裁选项](trimming-options.md)。

> [!NOTE]
> 剪裁是 .NET Core 3.1 和 .NET 5.0 中的实验性功能。 剪裁只能用于独立发布的应用程序。

## <a name="prevent-assemblies-from-being-trimmed"></a>防止剪裁程序集

在某些情况下，剪裁功能无法检测到引用。 例如，当应用程序或应用程序引用的库在运行时程序集上使用反射时，都不会直接引用该程序集。 剪裁不知道这些间接引用，并将从已发布文件夹中排除库。

当代码通过反射间接引用程序集时，可以防止使用 `<TrimmerRootAssembly>` 设置剪裁该程序集。 下面的示例演示如何防止剪裁名为 `System.Security` 程序集的程序集：

```xml
<ItemGroup>
    <TrimmerRootAssembly Include="System.Security" />
</ItemGroup>
```

### <a name="support-for-ssl-certificates"></a>支持 SSL 证书

如果你的应用加载 SSL 证书（如在 ASP.NET Core 应用中），则需要确保在剪裁时阻止剪裁有助于加载 SSL 证书的程序集。

我们可以更新项目文件，使其包含以下 ASP.NET Core 3.1 内容：

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

如果使用的是 .Net 5.0，则可以更新项目文件，使其包含以下内容：

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

## <a name="trim-your-app---cli"></a>剪裁应用 - CLI

使用 [dotnet publish](../tools/dotnet-publish.md) 命令剪裁应用程序。 发布应用时，请设置以下属性：

- 对于特定运行时 `-r win-x64`，发布为自包含应用
- 启用剪裁：`/p:PublishTrimmed=true`

以下示例将 Windows 应用发布为自包含，并剪裁输出。

```xml
<PropertyGroup>
    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
    <PublishTrimmed>true</PublishTrimmed>
</PropertyGroup>
```

下面的示例在主动剪裁模式下发布应用，在此模式下，将剪裁程序集中未使用的代码，并启用裁边器警告。

```xml
<PropertyGroup>
    <RuntimeIdentifier>win-x64</RuntimeIdentifier>
    <PublishTrimmed>true</PublishTrimmed>
    <TrimMode>link</TrimMode>
    <SuppressTrimAnalysisWarnings>false</SuppressTrimAnalysisWarnings>
</PropertyGroup>
```

有关详细信息，请参阅[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。

## <a name="trim-your-app---visual-studio"></a>剪裁应用 - Visual Studio

Visual Studio 创建可重用的发布配置文件，用于控制应用程序的发布方式。

01. 在“解决方案资源管理器”窗格中，右键单击要发布的项目  。 选择“发布…”  。

    :::image type="content" source="media/trim-self-contained/visual-studio-solution-explorer.png" alt-text="解决方案资源管理器，其中右键单击菜单突出显示了“发布”选项。":::

    如果还没有发布配置文件，请按照说明创建一个并选择“文件夹”目标类型  。

01. 选择“编辑”  。

    :::image type="content" source="media/trim-self-contained/visual-studio-publish-edit-settings.png" alt-text="带有“编辑”按钮的 Visual Studio 发布配置文件。":::

01. 在“配置文件设置”对话框中，设置以下选项  ：

    - 将“部署模式”设置为“自包含”   。
    - 将“目标运行时”设置为要发布到的平台  。
    - 选择“剪裁未使用的程序集(预览版)”  。

    选择“保存”保存设置并返回到“发布”对话框   。

    :::image type="content" source="media/trim-self-contained/visual-studio-publish-properties.png" alt-text="“配置文件设置”对话框，其中突出显示了“部署模式”、“目标运行时”和“剪裁未使用的程序集”选项。":::

01. 选择“发布”以发布剪裁过的应用  。

有关详细信息，请参阅[使用 Visual Studio 发布 .NET Core 应用](deploy-with-vs.md)。

## <a name="trim-your-app---visual-studio-for-mac"></a>剪裁应用 - Visual Studio for Mac

Visual Studio for Mac 不提供在发布期间剪裁应用的选项。 你需要按照[剪裁应用 - CLI](#trim-your-app---cli) 部分的说明手动发布。 有关详细信息，请参阅[使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。

## <a name="see-also"></a>请参阅

- [.NET Core 应用程序部署](index.md)。
- [使用 .NET Core CLI 发布 .NET Core 应用](deploy-with-cli.md)。
- [使用 Visual Studio 发布 .NET Core 应用](deploy-with-vs.md)。
- [dotnet publish 命令](../tools/dotnet-publish.md)。
