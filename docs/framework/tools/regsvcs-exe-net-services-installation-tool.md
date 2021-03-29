---
title: Regsvcs.exe（.NET 服务安装工具）
description: 使用 Regsvcs.exe（.NET 服务安装工具）。 加载和注册程序集，配置以编程方式添加到类的服务等。
ms.date: 03/30/2017
helpviewer_keywords:
- Regsvcs.exe
- .NET Services Installation tool
- loading assemblies
- assemblies [.NET Framework], registering
- type libraries
- registering assemblies
ms.assetid: 5220fe58-5aaf-4e8e-8bc3-b78c63025804
ms.openlocfilehash: 03787ecacc00c35f31f1fa5101fff5882e5314f6
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653304"
---
# <a name="regsvcsexe-net-services-installation-tool"></a>Regsvcs.exe（.NET 服务安装工具）

.NET 服务安装工具执行下列操作：  
  
- 加载并注册程序集。  
  
- 生成、注册类型库并将其安装到指定的 COM+ 应用程序中。  
  
- 配置以编程方式添加到类的服务。  
  
 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
      regsvcs [/c | /fc | /u] [/tlb:typeLibraryFile] [/extlb]  
[/reconfig] [/componly] [/appname:applicationName]  
[/nologo] [/quiet]assemblyFile.dll
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|--------------|-----------------|  
|assemblyFile.dll|源程序集文件。 此程序集必须用强名称进行签名。 有关详细信息，请参阅[使用强名称为程序集签名](../../standard/assembly/sign-strong-name.md)。|  
  
|选项|描述|  
|------------|-----------------|  
|/appdir: path|指定应用程序的根目录。|  
|/appname: applicationName|指定要查找或创建的 COM+ 应用程序的名称。|  
|**/c**|创建目标应用程序。|  
|/componly|只配置组件；忽略方法和接口。|  
|/exapp|指定此工具需要现有应用程序。|  
|/extlb|使用现有类型库。|  
|/fc|查找或创建目标应用程序。|  
|**/help**|显示该工具的命令语法和选项。|  
|/noreconfig|不重新配置现有的目标应用程序。|  
|**/nologo**|取消显示 Microsoft 启动版权标志。|  
|/parname: name|指定要查找或创建的 COM+ 应用程序的名称或 ID。|  
|/reconfig|重新配置现有目标应用程序。 这是默认设置。|  
|/tlb: typelibraryfile|指定要安装的类型库文件。|  
|/u|卸载目标应用程序。|  
|**/quiet**|指定安静模式；取消显示登录和成功消息。|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 Regsvcs.exe 需要由 assemblyFile.dll 指定的源程序集文件。 此程序集必须用强名称进行签名。 有关强名称签名的更多信息，请参阅[使用强名称为程序集签名](../../standard/assembly/sign-strong-name.md)。 目标应用程序的名称和类型库文件的名称都是可选的。 如果 applicationName 参数尚不存在，则该参数可从源程序集文件生成并且将由 Regsvcs.exe 创建。 typelibraryfile 参数可以指定类型库名称。 如果未指定类型库名称，默认情况下，Regsvcs.exe 将使用程序集名称。  
  
 当 Regsvcs.exe 注册组件的方法时，它需要遵从那些方法的[要求](/previous-versions/dotnet/netframework-4.0/9kc0c6st(v=vs.100))和[链接要求](../misc/link-demands.md)。 因为该工具在完全受信任的环境中执行，所以大多数权限要求都会成功。 但是，如果组件中的方法受 <xref:System.Security.Permissions.StrongNameIdentityPermission> 或 <xref:System.Security.Permissions.PublisherIdentityPermission> 的要求或链接要求保护，则 Regsvcs.exe 无法注册这些组件。  
  
 你必须在本地计算机上具有管理特权才能使用 Regsvcs.exe。  
  
 如果 Regsvcs.exe 在执行上述任何操作时失败，它将显示相应的错误信息。  
  
## <a name="examples"></a>示例  

 下面的命令将 `myTest.dll` 中包含的所有公共类添加到 `myTargetApp`（一个现有的 COM+ 应用程序）中，同时生成 `myTest.tlb` 类型库。  
  
```console  
regsvcs /appname:myTargetApp myTest.dll  
```  
  
 下面的命令将 `myTest.dll` 中包含的所有公共类添加到 `myTargetApp`（一个现有的 COM+ 应用程序）中，同时生成 `newTest.tlb` 类型库。  
  
```console  
regsvcs /appname:myTargetApp /tlb:newTest.tlb myTest.dll  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [如何：使用强名称为程序集签名](../../standard/assembly/sign-strong-name.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
