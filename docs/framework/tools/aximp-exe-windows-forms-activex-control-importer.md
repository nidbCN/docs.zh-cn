---
title: Aximp.exe（Windows 窗体 ActiveX 控件导入程序）
description: 了解 Aximp.exe（Windows 窗体 ActiveX 控件导入程序）。 此工具将 ActiveX 的 COM 类型库中的类型定义转换为 Windows 窗体。
ms.date: 03/30/2017
helpviewer_keywords:
- ActiveX controls, hosting in Windows Forms
- ActiveX Control Importer
- type definitions, converting
- Aximp.exe
- Windows Forms ActiveX Control Importer
ms.assetid: 482c0d83-7144-4497-b626-87d2351b78d0
ms.openlocfilehash: e0ee504f87eba811dab3422a367a60e235b3c51b
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652914"
---
# <a name="aximpexe-windows-forms-activex-control-importer"></a>Aximp.exe（Windows 窗体 ActiveX 控件导入程序）

ActiveX 控件导入程序将 ActiveX 控件的 COM 类型库中的类型定义转换为 Windows 窗体控件。  
  
 Windows 窗体只能承载 Windows 窗体控件，即从 <xref:System.Windows.Forms.Control> 派生的类。 Aximp.exe 生成可承载于 Windows 窗体上的 ActiveX 控件的包装器类。 这使你得以使用适用于其他 Windows 窗体控件的同一设计时支持和编程方法。  
  
 若要承载 ActiveX 控件，必须生成从 <xref:System.Windows.Forms.AxHost> 派生的包装器控件。 此包装器控件包含基础 ActiveX 控件的一个实例。 它知道如何与 ActiveX 控件通信，但它显示为 Windows 窗体控件。 这个生成的控件承载 ActiveX 控件并将其属性、方法和事件作为生成的控件的属性、方法和事件公开。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
aximp [options]{file.dll | file.ocx}  
```  
  
## <a name="remarks"></a>备注  
  
|参数|说明|  
|--------------|-----------------|  
|文件|包含要转换的 ActiveX 控件的源文件的名称。 文件参数中必须具有扩展名 .dll 或 .ocx。|  
  
|选项|描述|  
|------------|-----------------|  
|`/delaysign`|指定 Aximp.exe 使用延迟的签名对生成的控件进行签名。 必须使用 `/keycontainer:`、`/keyfile:` 或 `/publickey:` 选项指定此选项。 有关延迟签名过程的更多信息，请参见[延迟为程序集签名](../../standard/assembly/delay-sign.md)。|  
|`/help`|显示该工具的命令语法和选项。|  
|`/keycontainer:` *containerName*|使用在 containerName 指定的密钥容器中找到的公钥/私钥对，对生成的控件进行强名称签名。|  
|`/keyfile:` *filename*|使用在 filename 中找到的发行者的正式公钥/私钥对，对生成的控件进行强名称签名。|  
|`/nologo`|取消显示 Microsoft 启动版权标志。|  
|`/out:` *filename*|指定要创建的程序集的名称。|  
|`/publickey:` *filename*|使用在 filename 指定的文件中找到的公钥，对生成的控件进行强名称签名。|  
|`/rcw:` *filename*|使用指定的运行时可调用包装器，而不用生成新的包装器。 你可以指定多个实例。 当前目录用于相对路径。 有关详细信息，请参阅[运行时可调用包装器](../../standard/native-interop/runtime-callable-wrapper.md)。|  
|`/silent`|取消显示成功消息。|  
|`/source`|生成 Windows 窗体包装器的 C# 源代码。|  
|`/verbose`|指定详细模式；显示更多进度信息。|  
|`/?`|显示该工具的命令语法和选项。|  
  
 Aximp.exe 一次转换整个 ActiveX 控件类型库，并生成一组程序集，这些程序集包含在原始类型库中定义的类型的公共语言运行时元数据和控件实现。 生成的文件按照下面的模式命名：  
  
 COM 类型的公共语言运行时代理：progid.dll  
  
 ActiveX 控件的 Windows 窗体代理（其中 Ax 表示 ActiveX）：Axprogid.dll  
  
> [!NOTE]
> 如果 ActiveX 控件的成员名称与 .NET Framework 中定义的名称匹配，则 Aximp.exe 在创建 AxHost 派生类时，将在成员名称前加上前缀“Ctl”。 例如，如果 ActiveX 控件有一个名为“Layout”的成员，由于在 .NET Framework 中定义了 Layout 事件，因此该成员在 AxHost 派生类中将重命名为“CtlLayout”。  
  
 可以使用 [Ildasm.exe（IL 反汇编程序）](ildasm-exe-il-disassembler.md)之类的工具检查这些生成的文件。  
  
 不支持使用 Aximp.exe 为 ActiveX WebBrowser 控件 (shdocvw.dll) 生成 .NET 程序集。  
  
 在对 shdocvw.dll 运行 Aximp.exe 时，该工具始终会在运行它的目录中创建另一个名为 shdocvw.dll 的文件。 如果将此生成文件放在 Documents and Settings 目录下，则会导致 Microsoft Internet Explorer 和 Windows 资源管理器出现问题。 重启计算机时，Windows 会先在 Documents and Settings 目录查找 shdocvw.dll 的副本，然后再在 system32 目录查找。 它将使用在 Documents and Settings 目录中找到的副本，并尝试加载托管的包装器。 由于 Internet Explorer 和 Windows 资源管理器依赖于 system32 目录中的 shdocvw.dll 版本中的呈现引擎，因此它们将无法正常工作。 如果出现此问题，请在 Documents and Settings 目录中删除 shdocvw.dll 的副本，然后重启计算机。  
  
 通过对 shdocvw.dll 使用 Aximp.exe 来创建用于应用程序开发的 .NET 程序集也会导致问题。 在这种情况下，应用程序将同时加载 shdocvw.dll 的系统版本和生成版本，并可能为系统版本赋予更高的优先级。 此时，如果尝试在 WebBrowser ActiveX 控件内加载网页，用户可能会收到打开/保存对话框提示。 用户单击“打开”后，将在 Internet Explorer 中打开该网页。 此情况只出现在运行 Internet Explorer 版本 6 或更早版本的计算机上。 若要防止出现此问题，请使用托管的 <xref:System.Windows.Forms.WebBrowser> 控件或使用 Visual Studio 生成托管的 shdocvw.dll，如[如何：添加对类型库的引用](../interop/how-to-add-references-to-type-libraries.md)中所述。  
  
## <a name="example"></a>示例  

 下面的命令为媒体播放器控件 `msdxm.ocx` 生成 MediaPlayer.dll 和 AxMediaPlayer.dll。  
  
```console
aximp c:\systemroot\system32\msdxm.ocx  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Ildasm.exe（IL 反汇编程序）](ildasm-exe-il-disassembler.md)
