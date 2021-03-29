---
title: CorFlags.exe（CorFlags 转换工具）
description: 了解 CorFlags.exe（CorFlags 转换工具）。 此工具可用于配置可移植可执行映像的标头的 CorFlags 部分。
ms.date: 03/30/2017
helpviewer_keywords:
- CorFlags conversion tool
- CorFlags.exe
- portable executable files, CorFlags section
ms.assetid: ef900f8f-71ca-4dde-9b8c-95ddb0d7d89c
ms.openlocfilehash: 2b801ba28e0513c899123daa98fd649cbc3d9c28
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654162"
---
# <a name="corflagsexe-corflags-conversion-tool"></a>CorFlags.exe（CorFlags 转换工具）

利用 CorFlags 转换工具，你配置可移植可执行映像标头的 CorFlags 部分。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
CorFlags.exe assembly [options]  
```  
  
## <a name="parameters"></a>参数  
  
|必选参数|描述|  
|------------------------|-----------------|  
|`assembly`|要为其配置 CorFlags 的程序集的名称。|  
  
|选项|说明|  
|:------------|-----------------|  
|`-32BIT[REQ]+`|设置 32BITREQUIRED 标志。|  
|`-32BIT[REQ]-`|清除 32BITREQUIRED 标志。|  
|`-32BITPREF+`|设置 32BITPREFERRED 标志。 该应用程序甚至可以作为 32 位进程在 64 位平台上运行。 仅在 EXE 文件中设置此标志。 如果在 DLL 上设置此标志，则 DLL 将无法在 64 位进程中加载，并引发 <xref:System.BadImageFormatException> 异常。 具有此标志的 EXE 文件可以加载到 64 位进程中。<br /><br /> .NET Framework 4.5 中的新增功能。|  
|`-32BITPREF-`|清除 32BITPREFERRED 标志。<br /><br /> .NET Framework 4.5 中的新增功能。|  
|`-?`|显示该工具的命令语法和选项。|  
|`-Force`|强制执行更新，即使程序集具有强名称也是如此。 **重要提示：** 如果更新具有强名称的程序集，则必须在执行其代码之前再次对其签名。|  
|`-help`|显示该工具的命令语法和选项。|  
|`-ILONLY+`|设置 ILONLY 标志。|  
|`-ILONLY-`|清除 ILONLY 标志。|  
|`-nologo`|取消显示 Microsoft 启动版权标志。|  
|`-RevertCLRHeader`|将 CLR 标头版本还原到 2.0。|  
|`-UpgradeCLRHeader`|将 CLR 标头版本升级到 2.5。 **注意：** 程序集必须具有 2.5 版或更高版本的 CLR 标头才能在本机运行。|  
  
## <a name="remarks"></a>备注  

 如果未指定任何选项，则 CorFlags 转换工具将显示指定程序集的标志。  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [64 位应用程序](../64-bit-apps.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
