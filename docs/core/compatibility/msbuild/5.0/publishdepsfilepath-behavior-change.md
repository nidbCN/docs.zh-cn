---
title: 中断性变更：PublishDepsFilePath 行为变更
description: 了解 .NET 5 中的中断性变更：对于单文件应用程序，PublishDepsFilePath MSBuild 属性为空。
ms.date: 09/17/2020
ms.openlocfilehash: 845073e73ec6bdf820f28ace487d9ae4d04d0790
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872920"
---
# <a name="publishdepsfilepath-behavior-change"></a>PublishDepsFilePath 行为变更

对于单文件应用程序，`PublishDepsFilePath` MSBuild 属性为空。 此外，对于非单文件应用程序，可能在生成后期才会将 deps.json 文件复制到输出目录。

## <a name="version-introduced"></a>引入的版本

5.0

## <a name="change-description"></a>更改描述

在以前的 .NET 版本中，对于非单文件应用程序，`PublishDepsFilePath` MSBuild 属性是指向应用的输出目录中 deps.json 文件的路径；对于单文件应用，该属性是中间目录中的路径。

从 .NET 5 开始，对于单文件应用程序，`PublishDepsFilePath` 为空，并且新的 `IntermediateDepsFilePath` 属性指定 deps.json 在中间目录中的位置。 此外，对于非单文件应用程序，可能在生成后期才会将 deps.json 文件复制到输出目录（即 `PublishDepsFilePath` 指定的路径）。

## <a name="reason-for-change"></a>更改原因

进行此更改有以下几个原因：

- 在 .NET 5 中，为了支持[改进的单文件应用](https://github.com/dotnet/designs/blob/main/accepted/2020/single-file/design.md)，重构了发布逻辑。

- 在单文件应用中，为了帮助保护在捆绑 deps.json 后尝试重写 deps.json 文件的目标，从而静默不影响该应用 。 因此，对于单文件应用程序，`PublishDepsFilePath` 为空。

## <a name="recommended-action"></a>建议操作

重写 deps.json 文件的目标通常应使用 `IntermediateDepsFilePath` 属性来执行此操作。

## <a name="affected-apis"></a>受影响的 API

不可用

<!--

### Affected APIs

Not detectable via API analysis.

### Category

MSBuild

-->
