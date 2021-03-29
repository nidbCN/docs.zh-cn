---
title: Winmdexp.exe（Windows 运行时元数据导出工具）
description: 了解 Winmdexp.exe（Windows 运行时元数据导出工具）。 此工具将 .NET 模块转换为包含 Windows 运行时元数据的文件。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Runtime Metadata Export Tool
- Winmdexp.exe
ms.assetid: d2ce0683-343d-403e-bb8d-209186f7a19d
ms.openlocfilehash: 450ecf041d2be0ccc9f8b5ab4f1cb848bb4705c2
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652875"
---
# <a name="winmdexpexe-windows-runtime-metadata-export-tool"></a>Winmdexp.exe（Windows 运行时元数据导出工具）

Windows 运行时元数据导出工具 (Winmdexp.exe) 可将 .NET Framework 模块转换为包含 Windows 运行时元数据的文件。 尽管 .NET Framework 程序集和 Windows 运行时元数据文件使用相同的物理格式，但元数据表的内容有差别，这意味着 .NET Framework 程序集不可自动用作 Windows 运行时组件。 将 .NET Framework 模块转换为 Windows 运行时组件的过程称为“导出”。 在 .NET Framework 4.5 和 .NET Framework 4.5.1 中，生成的 Windows 元数据 (.winmd) 文件同时包含元数据和实现。  
  
 使用 Windows 运行时组件模板（在 C# 中，位于“Microsoft Store”下；在 Visual Basic 中，位于 Visual Studio 2013 或 Visual Studio 2012 中）时，编译器目标是 .winmdobj 文件，后续生成步骤将调用 Winmdexp.exe 以将 .winmdobj 文件导出到 .winmd 文件。 这是生成 Windows 运行时组件的推荐方法。 当你对生成过程需要的控制超过 Visual Studio 所能提供的控制时，请直接使用 Winmdexp.exe。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
winmdexp [options] winmdmodule  
```  
  
## <a name="parameters"></a>parameters  
  
|自变量或选项|描述|  
|------------------------|-----------------|  
|`winmdmodule`|指定要导出的模块 (.winmdobj)。 仅允许一个模块。 若要创建此模块，请将 `/target` 编译器选项与 `winmdobj` 目标一起使用。 请参阅 [-target:winmdobj（C# 编译器选项）](../../csharp/language-reference/compiler-options/output.md#targettype)或 [-target (Visual Basic)](../../visual-basic/reference/command-line-compiler/target.md)。|  
|`/docfile:` `docfile`<br /><br /> `/d:` `docfile`|指定 Winmdexp.exe 将生成的输出 XML 文档文件。 在 .NET Framework 4.5 中，输出文件实质上与输入 XML 文档文件相同。|  
|`/moduledoc:` `docfile`<br /><br /> `/md:` `docfile`|指定编译器使用 `winmdmodule` 生成的 XML 文档文件的名称。|  
|`/modulepdb:` `symbolfile`<br /><br /> `/mp:` `symbolfile`|指定包含 `winmdmodule` 的符号的程序数据库 (PDB) 文件的名称。|  
|`/nowarn:` `warning`|禁止显示指定的警告编号。 对于 *警告*，仅提供错误代码的数字部分（不包含前导零）。|  
|`/out:` `file`<br /><br /> `/o:` `file`|指定输出 Windows 元数据 (.winmd) 文件的名称。|  
|`/pdb:` `symbolfile`<br /><br /> `/p:` `symbolfile`|指定将包含导出的 Windows 元数据 (.winmd) 文件的符号的输出程序数据库 (PDB) 文件的名称。|  
|`/reference:` `winmd`<br /><br /> `/r:` `winmd`|指定要在导出过程中引用的元数据文件 (.winmd 或程序集)。 如果使用“Program Files (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5”（在 32 位计算机上为“\Program Files\\...”）中的引用程序集，请包含对 System.Runtime.dll 和 mscorlib.dll 的引用。|  
|`/utf8output`|指定输出消息应采用 UTF-8 编码。|  
|`/warnaserror+`|指定应将所有警告都视为错误。|  
|**@** `responsefile`|指定包含选项的响应 (.rsp) 文件（并且还可以选择指定 `winmdmodule`）。 `responsefile` 中的每行应包含一个参数或选项。|  
  
## <a name="remarks"></a>备注  

 Winmdexp.exe 未被设计将任意 .NET Framework 程序集转换为 .winmd 文件。 它需要使用 `/target:winmdobj` 选项编译的模块，并且其他限制也适用。 这些限制中最重要的是，程序集的 API 图面中公开的所有类型都必须是 Windows 运行时类型。 有关详细信息，请参阅[在 C# 和 Visual Basic 中创建 Windows 运行时组件](/previous-versions/br230301(v=vs.110))一文中的“在 Windows 运行时组件中声明类型”一节。
  
 当你使用 C# 或 Visual Basic 编写 Windows 8.x 应用商店应用或 Windows 运行时组件时，.NET Framework 将提供支持，使得利用 Windows 运行时进行编程更加自然。 [Windows 应用商店应用和 Windows 运行时的 .NET Framework 支持](../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)一文中对此进行了讨论。 在该过程中，某些常用的 Windows 运行时类型将映射到 .NET Framework 类型。 Winmdexp.exe 将反转此过程并生成使用对应的 Windows 运行时类型的 API 图面。 例如，从 <xref:System.Collections.Generic.IList%601> 接口构造的类型将映射到从 Windows 运行时 <xref:Windows.Foundation.Collections.IVector%601> 接口构造的类型。  
  
## <a name="see-also"></a>请参阅

- [.NET Framework 对 Windows 应用商店应用和 Windows 运行时的支持情况](../cross-platform/support-for-windows-store-apps-and-windows-runtime.md)
- [用 C# 和 Visual Basic 创建 Windows 运行时组件](/previous-versions/br230301(v=vs.110))
- [Winmdexp.exe 错误消息](winmdexp-exe-error-messages.md)
- [生成、部署和配置工具 (.NET Framework)](/previous-versions/dotnet/netframework-4.0/dd233108(v=vs.100))
