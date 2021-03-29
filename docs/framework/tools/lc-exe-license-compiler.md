---
title: Lc.exe（许可证编译器）
description: 使用 Lc.exe（许可证编译器）。 此工具读取包含授权信息的文本文件，并生成一个二进制文件作为资源嵌入到 CLR 可执行文件中。
ms.date: 03/30/2017
helpviewer_keywords:
- Lc.exe
- .licx file
- License Compiler
- control licenses [Windows Forms]
- compiling licenses file
- license file
- .licenses file
- Windows Forms, control licenses
- licensed controls [Windows Forms]
ms.assetid: 2de803b8-495e-4982-b209-19a72aba0460
ms.openlocfilehash: 69e79f4d2a75117f74428fdeee7684048d218e27
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654032"
---
# <a name="lcexe-license-compiler"></a>Lc.exe（许可证编译器）

许可证编译器读取包含授权信息的文本文件，并产生一个可作为资源嵌入到公共语言运行时可执行文件中的二进制文件。  
  
 每当将一个授权控件添加到窗体中时，Windows 窗体设计器就会自动生成或更新 .licx 文本文件。 作为编译的一部分，项目系统将 .licx 文本文件转换为 .licenses 二进制资源，此资源提供对 .NET 控件授权的支持。 然后该二进制资源将被嵌入到项目输出中。  
  
 在生成项目时，如果使用许可证编译器，则不支持在 32 位与 64 位之间进行交叉编译。 这是因为，许可证编译器必须加载程序集，而不允许从 32 位应用程序加载 64 位程序集，反之亦然。 在这种情况下，使用许可证编译器从命令行手动编译许可证，并指定相应的体系结构。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console
      lc /target:  
targetPE /complist:filename [-outdir:path]  
/i:modules [/nologo] [/v]  
```  
  
|选项|说明|  
|------------|-----------------|  
|/complist: filename|指定包含授权组件列表的文件名，这些授权组件要包含在 .licenses 文件中。 每个组件用它的全名引用，并且每行只有一个组件。<br /><br /> 命令行用户可为项目中的每个窗体指定一个单独的文件。 Lc.exe 接受多个输入文件并产生一个 .licenses 文件。|  
|**/h**[**elp**]|显示该工具的命令语法和选项。|  
|/i: module|指定模块，这些模块包含 **/complist** 文件中列出的组件。 若要指定多个模块，请使用多个 **/i** 标志。|  
|**/nologo**|取消显示 Microsoft 启动版权标志。|  
|/outdir: path|指定用来放置输出 .licenses 文件的目录。|  
|/target: targetPE|指定为其生成 .licenses 文件的可执行文件。|  
|**/v**|指定详细模式；显示编译进度信息。|  
|@ file |指定响应 (.rsp) 文件。|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="example"></a>示例  
  
1. 如果使用的是包含在名为 `HostApp.exe` *的应用程序中的 `Samples.DLL` 中的授权控件 `MyCompany.Samples.LicControl1`，* 则可创建包含以下内容的 `HostAppLic.txt`。  
  
    ```text
    MyCompany.Samples.LicControl1, Samples.DLL  
    ```  
  
2. 使用以下命令创建名为 `HostApp.exe.licenses` 的 .licenses 文件。  
  
    ```console  
    lc /target:HostApp.exe /complist:hostapplic.txt /i:Samples.DLL /outdir:c:\bindir  
    ```  
  
3. 生成将 .licenses 文件作为资源包含在内的 `HostApp.exe`。 如果生成的是 C# 应用程序，则应使用以下命令生成应用程序。  
  
    ```console
    csc /res:HostApp.exe.licenses /out:HostApp.exe *.cs  
    ```  
  
 以下命令从由 `myApp.licenses`、`hostapplic.txt` 和 `hostapplic2.txt` 指定的授权组件列表来编译 `hostapplic3.txt`。 `modulesList` 参数指定包含授权组件的模块。  
  
```console  
lc /target:myApp /complist:hostapplic.txt /complist:hostapplic2.txt /complist: hostapplic3.txt /i:modulesList  
```  
  
## <a name="response-file-example"></a>响应文件示例  

 下面的列表显示响应文件 `response.rsp` 的示例。 有关响应文件的详细信息，请参阅[响应文件](/visualstudio/msbuild/msbuild-response-files)。  
  
```text  
/target:hostapp.exe  
/complist:hostapplic.txt
/i:WFCPrj.dll
/outdir:"C:\My Folder"  
```  
  
 下面的命令行使用 `response.rsp` 文件。  
  
```console  
lc @response.rsp  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Al.exe（程序集链接器）](al-exe-assembly-linker.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
