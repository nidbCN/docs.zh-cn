---
title: 如何：查看全局程序集缓存的内容
description: 了解如何使用全局程序集缓存 (GAC) 工具 (gacutil.exe) 查看 .NET 中的全局程序集缓存的内容。
ms.date: 03/30/2017
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- global assembly cache, viewing contents
- viewing assemblies in global assembly cache
- Gacutil.exe
- strong-named assemblies, global assembly cache
- GAC (global assembly cache), viewing contents
- list of assemblies in global assembly cache
- Global Assembly Cache tool
ms.assetid: c5f786a0-969b-4f14-9f02-e77c3384d9af
ms.openlocfilehash: 56b4750e6b9338a6c136c16505b5da94f51fd72f
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653590"
---
# <a name="how-to-view-the-contents-of-the-global-assembly-cache"></a>如何：查看全局程序集缓存的内容

使用[全局程序集缓存工具 (Gacutil.exe)](../tools/gacutil-exe-gac-tool.md) 可查看全局程序集缓存 (GAC) 的内容。

## <a name="view-the-assemblies-in-the-gac"></a>查看 GAC 中的程序集

要查看全局程序集缓存中的程序集列表，请打开 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)，然后输入以下命令：

```shell
gacutil -l
```

- 或 -

```shell
gacutil /l
```

> [!NOTE]
> 在 .NET Framework 的早期版本中，可通过 [Shfusion.dll](/previous-versions/dotnet/netframework-4.0/34149zk3(v=vs.100)) Windows shell 扩展在文件资源管理器中查看全局程序集缓存。 从 .NET Framework 4 开始，Shfusion.dll 已过时。

## <a name="see-also"></a>请参阅

- [使用程序集和全局程序集缓存](working-with-assemblies-and-the-gac.md)
- [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)
