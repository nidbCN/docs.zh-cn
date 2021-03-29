---
title: Clrver.exe（CLR 版本工具）
description: 查看 Clrver.exe（CLR 版本工具）。 此工具报告计算机上公共语言运行时 (CLR) 的所有已安装版本。
ms.date: 03/30/2017
helpviewer_keywords:
- Clrver.exe
- CLR Version tool
ms.assetid: cbc2ee86-bdc8-4a65-a8f1-ba23bce3a699
ms.openlocfilehash: 0787cdf09d55a8464db7f5495b5aeed52c22f5d4
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654175"
---
# <a name="clrverexe-clr-version-tool"></a>Clrver.exe（CLR 版本工具）

CLR 版本工具 (Clrver.exe) 报告计算机上的公共语言运行时 (CLR) 的所有已安装版本。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
 在命令提示符处，输入下列命令：  
  
## <a name="syntax"></a>语法  
  
```console  
clrver [option]  
```  
  
## <a name="options"></a>选项  
  
|选项|说明|  
|------------|-----------------|  
|`-all`|显示正在使用的 CLR 的计算机上的所有进程。|  
|pid|显示具有指定的进程 ID (PID) 的进程所使用的 CLR 版本。|  
|`-?`|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 如果你未使用任何选项调用 Clrver.exe，则它将显示所有已安装的 CLR 版本。 如果你指定了另一个用户的 PID，则你必须具有管理权限才能获取版本信息。  
  
> [!NOTE]
> 在 Windows Vista 或更高版本中，用户帐户控制 (UAC) 决定用户的特权。 如果您是内置的 Administrators 组的成员，将为您分配两个运行时访问令牌：一个标准用户访问令牌和一个管理员访问令牌。 默认情况下，您拥有标准用户角色。 要执行需要管理权限的代码，首先必须将你的特权从标准用户提升至管理员。 你可以通过以下方式执行此操作：在启动命令提示符时，右键单击命令提示符图标并指示你希望以管理员身份运行。  
  
 尝试确定 SYSTEM、LOCAL SERVICE 和 NETWORK SERVICE 过程的 CLR 版本会导致出现一条消息，指示该 PID 不存在。  
  
## <a name="examples"></a>示例  

 以下命令显示在计算机上安装的 CLR 的所有版本。  
  
 `clrver`  
  
 以下命令显示进程 128 所用的 CLR 版本。  
  
 `clrver 128`  
  
 以下命令显示所有托管进程及其使用的 CLR 版本。  
  
 `Clrver -all`  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
