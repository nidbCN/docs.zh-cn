---
title: 将 Windows 窗体应用升级到 .NET 5
description: 使用 .NET 升级助手将现有的 .NET Framework Windows 窗体应用升级到 .NET 5。 .NET 升级助手是一种 CLI 工具，可帮助将应用从 .NET Framework 迁移到 .NET 5。
author: ardalis
ms.date: 03/08/2021
ms.openlocfilehash: 1ba80964c52cb9b6960efdebbd8d4e8ef33d63e6
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604822"
---
# <a name="upgrade-a-windows-forms-app-to-net-5-with-the-net-upgrade-assistant"></a>使用 .NET 升级助手将 Windows 窗体应用升级到 .NET 5

[.NET 升级助手](upgrade-assistant-overview.md)是一种命令行工具，可帮助将 .NET Framework Windows 窗体 (WinForms) 应用升级到 .NET 5。 本文提供以下内容：

- 演示如何针对 .NET Framework Windows 窗体应用运行该工具
- 故障排除提示

## <a name="upgrade-net-framework-windows-forms-apps"></a>升级 .NET Framework Windows 窗体应用

本部分演示如何针对新创建的面向 .NET Framework 4.6.1 的 Windows 窗体应用运行 NET 升级助手。 若要详细了解如何安装此工具，请查看 [.NET 升级助手概述](upgrade-assistant-overview.md)。

### <a name="initial-demo-setup"></a>初始演示设置

如果你要针对你自己的 .NET Framework 应用运行 .NET 升级助手，可跳过此步骤。 如果你只想试用一下来看看它的工作原理，可在此步骤中了解如何设置示例 .NET Windows 窗体项目以供使用。

借助 Visual Studio，使用 .NET Framework 创建一个新的 Windows 窗体项目。

:::image type="content" source="media/upgrade-assistant-winforms-framework/vs-new-project-winforms.png" alt-text="在 Visual Studio 中新建 Windows 窗体项目":::

将项目命名为 WinformsTest。 将项目配置为使用 .NET Framework 4.6.1。

:::image type="content" source="media/upgrade-assistant-winforms-framework/vs-configure-project-winforms.png" alt-text="在 Visual Studio 中配置 Windows 窗体项目":::

查看所创建的项目及其文件，尤其是它的项目文件。

### <a name="run-upgrade-assistant"></a>运行升级助手

打开终端，导航到目标项目或解决方案所在的文件夹。 运行 `upgrade-assistant` 命令，传入你要针对的项目的名称（可从任意位置运行该命令，只要项目文件的路径有效就行）。

```console
upgrade-assistant .\WinformsTest.csproj
```

该工具将运行并显示它将执行的步骤列表。

:::image type="content" source="media/upgrade-assistant-winforms-framework/step1.png" alt-text=".NET 升级助手初始屏幕":::

完成每个步骤后，该工具都会提供一组命令，用户可应用这些命令，也可跳过下一步骤、查看更多详细信息、配置日志记录或退出该过程。 如果该工具检测到某个步骤将不执行任何操作，它将自动跳过该步骤，转到下一步骤，直到到达有要执行的操作的步骤为止。 如果未进行其他任何选择，那么按 Enter 将执行下一步。

在此示例中，每次都会选择“应用”步骤。 第一步是备份项目。

:::image type="content" source="media/upgrade-assistant-winforms-framework/backup-project.png" alt-text=".NET 升级助手备份项目":::

该工具会提示输入自定义路径进行备份或使用默认路径，后者会将项目备份放在具有 `.backup` 扩展名的同一文件夹中。 此工具接下来做的是将项目文件转换为 SDK 样式。

:::image type="content" source="media/upgrade-assistant-winforms-framework/convert-project.png" alt-text=".NET 升级助手将项目转换为 SDK 样式":::

更新项目格式后，下一步是更新项目的 TFM。

:::image type="content" source="media/upgrade-assistant-winforms-framework/update-tfm.png" alt-text=".NET 升级助手更新 TFM":::

接下来，该工具会更新项目的 NuGet 包。

:::image type="content" source="media/upgrade-assistant-winforms-framework/update-nuget-packages.png" alt-text=".NET 升级助手更新 NuGet 包":::

更新包后，接下来是添加模板文件（如果有）。 在本例中，没有需要添加的模板文件。 该步骤将继续，迁移应用配置文件并更新 C# 源来应用修补程序，如下所示。 此项目无需任何配置文件或源代码更改，因此会自动继续这些步骤。

:::image type="content" source="media/upgrade-assistant-winforms-framework/add-template-files.png" alt-text=".NET 升级助手添加模板文件和迁移配置":::

这是最后一个项目，因此下一步是“移动到新的项目”，它提示完成迁移整个解决方案的过程。

:::image type="content" source="media/upgrade-assistant-winforms-framework/complete-solution.png" alt-text=".NET 升级助手完成解决方案":::

完成此过程后，已迁移的 Windows 窗体项目将如下所示：

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>net5.0-windows</TargetFramework>
    <OutputType>WinExe</OutputType>
    <GenerateAssemblyInfo>false</GenerateAssemblyInfo>
    <UseWindowsForms>true</UseWindowsForms>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Microsoft.CSharp" Version="4.7.0" />
    <PackageReference Include="Microsoft.DotNet.UpgradeAssistant.Extensions.Default.Analyzers" Version="0.2.211730">
      <PrivateAssets>all</PrivateAssets>
    </PackageReference>
    <PackageReference Include="Microsoft.Windows.Compatibility" Version="5.0.2" />
  </ItemGroup>
</Project>
```

请注意，.NET 升级助手还会向项目添加分析器，它可帮助继续执行升级过程。

## <a name="troubleshooting-tips"></a>故障排除提示

使用 .NET 升级助手时，可能会出现一些已知问题。 某些情况下，.NET 升级助手在内部使用的 [try-convert 工具](https://github.com/dotnet/try-convert)会出现问题。 此工具会频繁更新来处理更多场景，因此请务必使用最新版本。

- 必须安装 try-convert 并将其更新到不低于 0.7.212201 版本。
- 更低版本的 try-convert 工具不支持自定义目标或属性文件。 如果无法升级到最新版，则可能需要手动处理这些问题。 如果目标项目文件包含对自定义目标或属性文件的引用，则在对其运行 .NET 升级助手之前，可能需要从文件中手动删除这些引用。

有关其他故障排除提示和已知问题，可查看[此工具的 GitHub 存储库](https://github.com/dotnet/upgrade-assistant#troubleshooting-common-issues)。

## <a name="see-also"></a>另请参阅

- [使用 .NET 升级助手将 WPF 应用升级到 .NET 5](upgrade-assistant-wpf-framework.md)
- [使用 .NET 升级助手将 ASP.NET MVC 应用升级到 .NET 5](upgrade-assistant-aspnetmvc.md)
- [.NET 升级助手概述](upgrade-assistant-overview.md)
- [.NET 升级助手 GitHub 存储库](https://github.com/dotnet/upgrade-assistant)
