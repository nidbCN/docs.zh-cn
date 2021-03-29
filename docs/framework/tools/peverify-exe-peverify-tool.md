---
title: Peverify.exe（PEVerify 工具）
description: 使用 Peverify.exe（可移植可执行验证）帮助确定 Microsoft 中间语言 (MSIL) 代码和元数据是否满足 .NET 中的类型安全标准。
ms.date: 03/30/2017
helpviewer_keywords:
- portable executable files, PEVerify
- verifying MSIL and metadata
- PEVerify tool
- type safety requirements
- MSIL
- PEverify.exe
- PE files, PEVerify
ms.assetid: f4f46f9e-8d08-4e66-a94b-0c69c9b0bbfa
ms.openlocfilehash: ba8b4cd7f398eee9129d24d25d8fdb754c89a3ee
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653291"
---
# <a name="peverifyexe-peverify-tool"></a>Peverify.exe（PEVerify 工具）

PEVerify 工具有助于生成 Microsoft 中间语言 (MSIL) 的开发人员（如编译器编写者、脚本引擎开发人员）确定其 MSIL 代码及关联的元数据是否满足类型安全要求。 某些编译器仅当你避免使用某些语言构造时才生成可验证的类型安全代码。 如果正在使用此类编译器，则可能需要确认你未危害代码的类型安全性。 可以对文件运行 PEVerify 工具来检查 MSIL 和元数据。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。
  
## <a name="syntax"></a>语法  
  
```console  
peverify filename [options]  
```  
  
## <a name="parameters"></a>参数  
  
|参数|描述|  
|--------------|-----------------|  
|*filename*|要为其检查 MSIL 和元数据的可移植可执行 (PE) 文件。|  
  
|选项|描述|  
|------------|-----------------|  
|/break= maxErrorCount|在出现 maxErrorCount 错误后中止验证。<br /><br /> 此参数在 .NET Framework 2.0 版或更高版本中不受支持。|  
|**/clock**|测量并报告下列验证时间（以毫秒为单位）：<br /><br /> MD Val. cycle<br /> 元数据验证周期<br /><br /> MD Val. pure<br /> 元数据纯验证<br /><br /> IL Ver. cycle<br /> Microsoft 中间语言 (MSIL) 验证周期<br /><br /> IL Ver pure<br /> MSIL 纯验证<br /><br /> MD Val. cycle 和 IL Ver. cycle 时间包括执行必要的启动和关闭过程所需的时间 。 MD Val. pure 和 IL Ver pure 时间反映了只执行验证或检验所需的时间 。|  
|**/help**|显示该工具的命令语法和选项。|  
|/hresult|以十六进制格式显示错误代码。|  
|/ignore= hex.code [, hex.code] |忽略指定的错误代码。|  
|/ignore=@ responseFile|忽略在指定响应文件中列出的错误代码。|  
|/il|针对在由 filename 指定的程序集中实现的方法执行 MSIL 类型安全验证检查。 除非指定 /quiet 选项，否则此工具将为发现的每个问题返回详细的说明。|  
|/md|针对由 filename 指定的程序集执行元数据验证检查。 这将遍历文件中的整个元数据结构并报告遇到的所有验证问题。|  
|**/nologo**|禁止显示产品版本和版权信息。|  
|/nosymbols|在 .NET Framework 2.0 版中，禁止显示行号以便向后兼容。|  
|**/quiet**|指定安静模式；禁止显示验证问题报告的输出。 Peverify.exe 仍将报告文件是否是类型安全的，但不会报告关于阻止类型安全验证的问题的信息。|  
|`/transparent`|仅验证透明方法。|  
|/unique|忽略重复的错误代码。|  
|**/verbose**|在 .NET Framework 2.0 版中，显示 MSIL 验证消息中的附加信息。|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 公共语言运行时依赖应用程序代码的类型安全执行，以帮助强制实施安全性和隔离机制。 通常情况下，虽然可以设置安全策略以允许执行受信任但不可验证的代码，但无法运行不是[可验证类型安全](../../standard/security/key-security-concepts.md#type-safety-and-security)的代码。  
  
 如果既未指定 /md 选项也未指定 /il 选项，则 Peverify.exe 将执行两种类型的检查 。 Peverify.exe 首先执行 /md 检查。 如果没有错误，则执行 /il 检查。 如果同时指定了 /md 和 /il，则即使元数据中存在错误，也执行 /il 检查  。 因此，如果没有元数据错误，则 peverify filename 等效于 peverify filename /md /il 。  
  
 Peverify.exe 基于数据流分析和一个包含数百条有关有效元数据的规则的列表来执行全面的 MSIL 验证检查。 有关 Peverify.exe 执行的检查的详细信息，请参见 Windows SDK 中“Tools Developers Guide”文件夹中的“元数据验证规范”和“MSIL 指令集规范”。  
  
.NET Framework 2.0 版或更高版本支持使用如下 MSIL 指令指定的可验证 `byref` 返回值：`dup`、`ldsflda`、`ldflda`、`ldelema`、`call` 和 `unbox`。  
  
## <a name="examples"></a>示例  

 下面的命令为在 `myAssembly.exe` 程序集中实现的方法执行元数据验证检查和 MSIL 类型安全验证检查。  
  
```console  
peverify myAssembly.exe /md /il  
```  
  
 上述请求成功完成后，Peverify.exe 将显示下面的消息。  
  
```output
All classes and methods in myAssembly.exe Verified  
```  
  
 下面的命令为在 `myAssembly.exe` 程序集中实现的方法执行元数据验证检查和 MSIL 类型安全验证检查。 此工具显示执行这些检查所需的时间。  
  
```console  
peverify myAssembly.exe /md /il /clock  
```  
  
 上述请求成功完成后，Peverify.exe 将显示下面的消息。  
  
```output
All classes and methods in myAssembly.exe Verified  
Timing: Total run     320 msec  
        MD Val.cycle  40 msec  
        MD Val.pure   10 msec  
        IL Ver.cycle  270 msec  
        IL Ver.pure   230 msec  
```  
  
 下面的命令为在 `myAssembly.exe` 程序集中实现的方法执行元数据验证检查和 MSIL 类型安全验证检查。 但是，Peverify.exe 会在达到最大错误计数 100 时停止。 该工具也忽略指定的错误代码。  
  
```console  
peverify myAssembly.exe /break=100 /ignore=0x12345678,0xABCD1234  
```  
  
 下面的命令与上一个示例产生同样的结果，但指定要在响应文件 `ignoreErrors.rsp` 中忽略的错误代码。  
  
```console  
peverify myAssembly.exe /break=100 /ignore@ignoreErrors.rsp  
```  
  
 响应文件可以包含一个用逗号分隔的错误代码列表。  
  
```text
0x12345678, 0xABCD1234  
```  
  
 或者，可将响应文件的格式设置为每行一个错误代码。  
  
```text
0x12345678  
0xABCD1234  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [编写可验证类型安全代码](../misc/code-access-security-basics.md#typesafe_code)
- [类型安全和安全性](../../standard/security/key-security-concepts.md#type-safety-and-security)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
