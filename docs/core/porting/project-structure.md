---
title: 组织适用于 .NET Framework 和 .NET Core 的项目
description: 帮助希望针对 .NET Framework 和 .NET Core 并行编译解决方案的项目所有者。
author: conniey
ms.date: 12/07/2018
ms.openlocfilehash: 0d495ce7b21b45d3fabe444f117667e5908ac81a
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104873661"
---
# <a name="organize-your-project-to-support-both-net-framework-and-net-core"></a>组织项目以支持 .NET Framework 和 .NET Core

可以创建一个并行编译 .NET Framework 和 .NET Core 的解决方案。 本文介绍可帮助你实现此目标的多个项目组织选项。 以下是决定如何使用 .NET Core 设置项目布局时要考虑的一些典型方案。 此列表可能无法涵盖所有要求；这些方案的优先级具体取决于项目需求。

- [**将现有项目和 .NET Core 项目合并为单个项目**](#replace-existing-projects-with-a-multi-targeted-net-core-project)

  *此方案的好处：*
  - 通过编译单个项目（而非多个项目）简化生成过程，每个项目针对不同的 .NET Framework 版本或平台。
  - 由于需要管理单个项目文件，因此简化了多目标项目的源文件管理。 添加或删除源文件时，需要手动将备用文件与其他项目进行同步。
  - 轻松生成可供使用的 NuGet 包。
  - 允许通过使用编译器指令为库中特定的 .NET Framework 版本编写代码。

  *不支持的方案：*
  - 要求开发者使用 Visual Studio 2017 或更高版本来打开现有项目。 若要支持 Visual Studio 的早期版本，建议[将项目文件保存在不同的文件夹中](#support-vs)。

- <a name="support-vs"></a>[**将现有项目和新的 .NET Core 项目分离**](#keep-existing-projects-and-create-a-net-core-project)

  *此方案的好处：*
  - 支持没有安装 Visual Studio 2017 或更高版本的开发人员和参与者基于现有项目开发。
  - 减少现有项目中出现新 bug 的可能性，因为这些项目中不需要进行任何代码改动。

## <a name="example"></a>示例

请考虑以下存储库：

![现有项目](./media/project-structure/existing-project-structure.png)

[源代码](https://github.com/dotnet/samples/tree/main/framework/libraries/migrate-library/)

根据现有项目的约束和复杂性，有几种不同的方法可为此存储库添加对 .NET Core 的支持，下面描述了这些方法。

## <a name="replace-existing-projects-with-a-multi-targeted-net-core-project"></a>将现有项目替换为多目标的 .NET Core 项目

重新组织存储库，以便删除任何现有的 \*.csproj 文件，并创建以多个框架为目标的单一 \*.csproj 文件。 这是一项不错的选择，因为单个项目可以编译不同的框架。 它还可以处理每个目标框架的不同编译选项和依赖项。

![创建以多个框架为目标的 csproj](./media/project-structure/multi-targeted-project.png)

[源代码](https://github.com/dotnet/samples/tree/main/framework/libraries/migrate-library-csproj/)

需注意的更改：

- 用新的 [.NET Core *\*.csproj*](https://github.com/dotnet/samples/tree/main/framework/libraries/migrate-library-csproj/src/Car/Car.csproj) 替换 packages.config 和 \*.csproj。 NuGet 包是使用 `<PackageReference> ItemGroup` 指定的。

## <a name="keep-existing-projects-and-create-a-net-core-project"></a>保留现有项目并创建 .NET Core 项目

如果存在以较旧框架为目标的现有项目，可能需要保留这些项目并将 .NET Core 项目用作将来框架的目标。

![位于不同文件夹中的现有项目与 .NET Core 项目](./media/project-structure/separate-projects-same-source.png)

[源代码](https://github.com/dotnet/samples/tree/main/framework/libraries/migrate-library-csproj-keep-existing/)

将 .NET Core 项目和现有项目保存在不同的文件夹中。 将项目保存在不同的文件夹中可以避免强制使用 Visual Studio 2017 或更高版本。 可以创建仅打开旧项目的单独解决方案。

## <a name="see-also"></a>另请参阅

- [.NET Core 移植文档](index.md)
