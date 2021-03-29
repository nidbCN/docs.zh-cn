---
title: SecAnnotate.exe（.NET 安全批注器工具）
description: 使用 SecAnnotate.exe（.NET 安全批注器工具）。 标识一个或多个程序集的 SecurityCritical 和 SecuritySafeCritical 部分。
ms.date: 03/30/2017
helpviewer_keywords:
- SecAnnotate.exe
- Security Annotator tool
ms.assetid: 8104d208-7813-4a1d-8a75-58f9a7bcb8c9
ms.openlocfilehash: 07c41a50292e55af57effec821756f0ffbc2142d
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653824"
---
# <a name="secannotateexe-net-security-annotator-tool"></a>SecAnnotate.exe（.NET 安全批注器工具）

.NET 安全性批注器工具 (SecAnnotate.exe) 是标识一个或多个程序集的 `SecurityCritical` 和 `SecuritySafeCritical` 部分的命令行应用程序。  
  
 [安全性批注器](https://marketplace.visualstudio.com/items?itemName=sheldonb.SecurityAnnotator)是一个 Visual Studio 扩展，提供了 SecAnnotate.exe 的图形用户界面，使用户能够从 Visual Studio 中运行该工具。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
 在命令提示符处，键入以下内容，其中，parameters 将在下一部分进行介绍，assemblies 由一个或多个由空格分隔的程序集名称组成 ：  
  
## <a name="syntax"></a>语法  
  
```console  
SecAnnotate.exe [parameters] [assemblies]  
```  
  
## <a name="parameters"></a>参数  
  
|选项|说明|  
|------------|-----------------|  
|`/a`<br /><br /> or<br /><br /> `/showstatistics`|显示与正在分析的程序集中的透明度使用有关的统计信息。|  
|`/d:` directory<br /><br /> or<br /><br /> `/referencedir:` directory|指定在批注过程中搜索依赖程序集的目录。|  
|`/i`<br /><br /> 或<br /><br /> `/includesignatures`|包括批注报告文件中的扩展签名信息。|  
|`/n`<br /><br /> or<br /><br /> `/nogac`|禁止搜索全局程序集缓存中引用的程序集。|  
|`/o:` output.xml<br /><br /> or<br /><br /> `/out:` output.xml|指定输出批注文件。|  
|`/p:` maxpasses<br /><br /> or<br /><br /> `/maximumpasses:` maxpasses|指定停止生成新的批注前要对程序集进行的最大批注轮数。|  
|`/q`<br /><br /> or<br /><br /> `/quiet`|指定安静模式，在此模式下，批注器不会输出状态消息；它只会输出错误信息。|  
|`/r:` assembly<br /><br /> or<br /><br /> `/referenceassembly:` assembly|当在批注过程中解析依赖程序集时，包含指定的程序集。 引用程序集获得的优先级高于在引用路径中找到的程序集。|  
|`/s:` rulename<br /><br /> or<br /><br /> `/suppressrule:` rulename|禁止对输入程序集运行指定的透明度规则。|  
|`/t`<br /><br /> or<br /><br /> `/forcetransparent`|强制批准器工具将所有没有任何透明度批注的程序集作为完全透明来处理。|  
|`/t`:assembly<br /><br /> or<br /><br /> `/forcetransparent`:assembly|将给定程序集强制为透明，而无论其当前程序集级别批注如何。|  
|||  
|`/v`<br /><br /> 或<br /><br /> `/verify`|仅验证程序集的批注是否正确；不会尝试进行多轮验证来查找所有需要的批注（前提是程序集不会验证）。|  
|`/x`<br /><br /> or<br /><br /> `/verbose`|指定批注时的详细输出。|  
|`/y:` directory<br /><br /> or<br /><br /> `/symbolpath:` directory|当在批注过程中搜索符号文件时，包含指定的目录。|  
  
## <a name="remarks"></a>备注  

 参数和程序集也可能在命令行上指定的响应文件中提供，且带有前缀 at 符号 (@)。 响应文件中的每行应包含单个参数或程序集名称。  
  
 有关 .NET 安全性批注器的详细信息，请参阅 .NET 安全性博客中的[使用 SecAnnotate 分析程序集中的透明度冲突](/archive/blogs/shawnfa/using-secannotate-to-analyze-your-assemblies-for-transparency-violations-an-example)。  
  
## <a name="examples"></a>示例
