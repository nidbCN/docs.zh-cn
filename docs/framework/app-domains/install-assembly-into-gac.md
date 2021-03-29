---
title: 如何：将程序集安装到全局程序集缓存
description: 将程序集安装到 .NET 中的全局程序集缓存 (GAC)，以供多个应用程序共享。 使用 Windows Installer 或 GAC 实用程序。
ms.date: 08/20/2019
helpviewer_keywords:
- assemblies [.NET Framework], global assembly cache
- Gacutil.exe
- strong-named assemblies, global assembly cache
- global assembly cache, installing assemblies
- Global Assembly Cache tool
- windows installer, global assembly cache
ms.assetid: a7e6f091-d02c-49ba-b736-7295cb0eb743
ms.openlocfilehash: 0034c6d7a7184e8fc3a6384c829f6bf13aad6cdd
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653460"
---
# <a name="how-to-install-an-assembly-into-the-global-assembly-cache"></a>如何：将程序集安装到全局程序集缓存

全局程序集缓存 (GAC) 存储由多个应用程序共享的程序集。 使用以下任一组件将程序集安装到[全局程序集缓存](gac.md)中：

- [Windows 安装程序](#windows-installer)
- [全局程序集缓存工具](#global-assembly-cache-tool)

> [!IMPORTANT]
> 可以只将强名称程序集安装到全局程序集缓存中。 有关如何创建强名称程序集的信息，请参阅[如何：使用强名称为程序集签名](../../standard/assembly/sign-strong-name.md)。

## <a name="windows-installer"></a>Windows Installer

建议使用 [Windows Installer](/windows/desktop/Msi/installation-of-assemblies-to-the-global-assembly-cache)（即 Windows 安装引擎）将程序集添加到全局程序集缓存。 Windows Installer 可提供全局程序集缓存中程序集的引用计数，还具有其他优点。 若要创建 Windows Installer 的安装程序包，请使用[适用于 Visual Studio 2017 的 WiX 工具集扩展](https://marketplace.visualstudio.com/items?itemName=RobMensching.WixToolsetVisualStudio2017Extension)。

## <a name="global-assembly-cache-tool"></a>全局程序集缓存工具

可以使用 [.NET 全局程序集实用程序 (gacutil.exe)](../tools/gacutil-exe-gac-tool.md) 将程序集添加到全局程序集缓存，并查看全局程序集缓存的内容。

   > [!NOTE]
   > Gacutil.exe 仅用于开发目的。 请勿用于将生产程序集安装到全局程序集缓存。

使用 gacutil.exe 在 GAC 中安装程序集的语法如下：

```cmd
gacutil -i <assembly name>
```

在此命令中，\<assembly name> 是要在全局程序集缓存中安装的程序集的名称。

如果 gacutil.exe 不在系统路径中，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

下面的示例将文件名为 hello.dll 的程序集安装到全局程序集缓存。

```cmd
gacutil -i hello.dll
```

> [!NOTE]
> 在 .NET Framework 的早期版本中，可以使用 Shfusion.dll Windows shell 扩展，通过将程序集拖到“文件资源管理器”来安装这些程序集。 从 .NET Framework 4 开始，Shfusion.dll 已过时。

## <a name="see-also"></a>请参阅

- [使用程序集和全局程序集缓存](working-with-assemblies-and-the-gac.md)
- [如何：从全局程序集缓存中删除程序集](how-to-remove-an-assembly-from-the-gac.md)
- [Gacutil.exe（全局程序集缓存工具）](../tools/gacutil-exe-gac-tool.md)
- [如何：使用强名称为程序集签名](../../standard/assembly/sign-strong-name.md)
