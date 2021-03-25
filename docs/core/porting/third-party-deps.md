---
title: 分析依赖项以移植代码
description: 了解如何分析外部依赖项，以便将项目从 .NET Framework 移植到 .NET。
author: cartermp
ms.date: 03/04/2021
ms.openlocfilehash: 4619243cf300e248be45e4b2a4d5541c3b3e1cb5
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102604913"
---
# <a name="analyze-your-dependencies-to-port-code-from-net-framework-to-net"></a>分析依赖项以将代码从 .NET Framework 移植到 .NET 中

若要将代码移植到 .NET 或.NET Standard，必须了解依赖项。 外部依赖项是在项目中引用但没有自行构建的 NuGet 包或 `.dll` 文件。

通过将代码移植到 .NET Standard 2.0 或更低版本，可确保该代码可用于 .NET Framework 和 .NET。 但是，如果你不需要将库用于 .NET Framework，请考虑以最新版本的 .NET 为目标。

## <a name="migrate-your-nuget-packages-to-packagereference"></a>将 NuGet 包迁移到 `PackageReference`

.NET 不能使用 [packages.config](/nuget/reference/packages-config) 文件引用 NuGet。 .NET 和 .NET Framework 都可以使用 [PackageReference](/nuget/consume-packages/package-references-in-project-files) 来指定包依赖项。 如果你使用 packages.config 在项目中指定包，请将其转换为 `PackageReference` 格式。

如需了解如何迁移，请参阅[从 packages.config 迁移到 PackageReference](/nuget/reference/migrate-packages-config-to-package-reference)一文。

## <a name="upgrade-your-nuget-packages"></a>升级 NuGet 包

将项目迁移为 `PackageReference` 格式后，验证包是否与 .NET 兼容。

首先，请将包升级到可以使用的最新版本。 可通过 Visual Studio 中的 NuGet 包管理器 UI 完成此操作。 包依赖项的较新版本可能已经与 .NET Core 兼容。

## <a name="analyze-your-package-dependencies"></a>分析包依赖项

如果尚未验证转换和升级后的包依赖项是否适用于 .NET Core，可通过以下几种方法实现此目的：

### <a name="analyze-nuget-packages-using-nugetorg"></a>使用 nuget.org 分析 NuGet 包

可以在包页面“依赖项”部分的 [nuget.org](https://www.nuget.org/) 上查看每个包所支持的目标框架名字对象 (TFM)  。

尽管使用站点验证兼容性是一种比较简单的方法，但站点上并未提供所有包的“依赖项”信息  。

### <a name="analyze-nuget-packages-using-nuget-package-explorer"></a>使用 NuGet 包资源管理器分析 NuGet 包

NuGet 包本身就是一组包含特定于平台的程序集的文件夹。 检查包中是否有包含兼容程序集的文件夹。

检查 NuGet 包文件夹最简单方法是使用 [NuGet 包资源管理器](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer)工具。 安装完成后，使用以下步骤查看文件夹名称：

1. 打开 NuGet 包资源管理器。
2. 单击“从在线源打开包”  。
3. 搜索包的名称。
4. 从搜索结果中选择包的名称，然后点击“打开”  。
5. 展开右侧的“lib”文件夹并查看文件夹名称  。

查找名称使用以下模式之一的文件夹：`netstandardX.Y`、`netX.Y` 或 `netcoreappX.Y`。

这些值是映射到 [.NET Standard](../../standard/net-standard.md)、.NET 和 .NET Core 各版本的[目标框架名字对象 (TFM)](../../standard/frameworks.md)，它们均与 .NET 兼容。

> [!IMPORTANT]
> 在查看包支持的 TFM 时，请注意，除 `netstandard*` 以外的 TFM 面向的是 .NET 的特定实现，如 .NET 5、.NET Core 或 .NET Framework。 从 .NET 5 开始，`net*` TFM（未指定操作系统）实际上取代了 `netstandard*` 成为[可移植目标](../../standard/net-standard.md#net-5-and-net-standard)。 例如，`net5.0` 面向 .NET 5 API 图面，并且是跨平台友好的，而 `net5.0-windows` 面向在 Windows 操作系统上实现的 .NET 5 API 图面。

## <a name="net-framework-compatibility-mode"></a>.NET Framework 兼容性模式

在分析完 NuGet 包之后，你可能会发现它们仅面向 .NET Framework。

从 .NET Standard 2.0 开始，引入了 .NET Framework 兼容性模式。 此兼容性模式允许 .NET Standard 和 .NET Core 项目引用 .NET Framework 库。 引用 .NET Framework 库不适用于所有项目（如库使用 Windows Presentation Foundation (WPF) API 时），但它的开启了很多移植方案。

在项目中引用面向 .NET Framework 的 NuGet 包时（如 [`Huitian.PowerCollections`](https://www.nuget.org/packages/Huitian.PowerCollections)），你会收到类似于以下示例的包回退警告 ([NU1701](/nuget/reference/errors-and-warnings/nu1701))：

`NU1701: Package ‘Huitian.PowerCollections 1.0.0’ was restored using ‘.NETFramework,Version=v4.6.1’ instead of the project target framework ‘.NETStandard,Version=v2.0’. This package may not be fully compatible with your project.`

当添加包以及每次生成时都会显示该警告，以确保在项目中测试该包。 如果项目按预期工作，可通过在 Visual Studio 中编辑包属性或在最喜欢的代码编辑器中手动编辑项目文件来取消该警告。

若要通过编辑项目文件来取消警告，请找到要取消警告的包的 `PackageReference` 条目并添加 `NoWarn` 属性。 `NoWarn` 属性接受所有警告 ID 的逗号分隔列表。 以下示例显示如何通过手动编辑项目文件来取消 `Huitian.PowerCollections` 包的 `NU1701` 警告：

```xml
<ItemGroup>
  <PackageReference Include="Huitian.PowerCollections" Version="1.0.0" NoWarn="NU1701" />
</ItemGroup>
```

有关如何在 Visual Studio 中取消编译器警告的详细信息，请参阅[取消 NuGet 包的警告](/visualstudio/ide/how-to-suppress-compiler-warnings#suppress-warnings-for-nuget-packages)。

## <a name="if-nuget-packages-wont-run-on-net"></a>如果 NuGet 包无法在 .NET 上运行

如果所依赖的 NuGet 包无法在 .NET Core 上运行，可以执行以下几项操作：

- 如果项目是开放源代码并托管在诸如 GitHub 中，则可以直接与开发人员交流。
- 可直接在 [nuget.org](https://www.nuget.org/) 上联系作者。搜索包并单击包页面左侧的“联系所有者”  。
- 可以搜索在 .NET Core 上运行的其他包，它与所使用的包进行的是相同的任务。
- 可以尝试自己编写包所执行的代码。
- 可以通过更改应用的功能来清除对包的依赖性，至少在该包有可用的兼容性版本之前都能这样做。

请注意，开源项目维护人员和 NuGet 包发布者通常是志愿者。 他们因为关注某个特定领域而免费提供服务，通常从事另外一份职业。 当联系他们请求 .NET Core 支持时请注意这点。

如果使用上述任何选项都无法解决问题，则可能需要移植到最近版本的 .NET Core。

.NET 团队需要了解哪些库最需要使用 .NET Core 支持。 你可以发送电子邮件到 dotnet@microsoft.com，谈谈你想要使用的库。

## <a name="analyze-non-nuget-dependencies"></a>分析非 NuGet 依赖项

用户可能拥有不是 NuGet 包的依赖项，如文件系统中的 DLL。 确定该依赖项是否可移植的唯一方法是运行 [.NET 可移植性分析器](https://github.com/Microsoft/dotnet-apiport)工具。 该工具可以分析面向 .NET Framework 的程序集，并识别不可移植到其他 .NET 平台（如 .NET Core）的 API。 可将该工具作为控制台应用程序或作为 [Visual Studio 扩展](../../standard/analyzers/portability-analyzer.md)来运行。

## <a name="next-steps"></a>后续步骤

- [有关从 .NET Framework 移植到 .NET 的概述](index.md)
- [移植库](libraries.md)
