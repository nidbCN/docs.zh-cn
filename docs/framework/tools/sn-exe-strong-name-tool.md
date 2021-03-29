---
title: Sn.exe（强名称工具）
description: 开始使用 Sn.exe（强名称工具）。 使用强名称对程序集进行签名。 管理密钥、生成和验证签名。
ms.date: 03/30/2017
helpviewer_keywords:
- public keys, signing files
- Strong Name tool
- Sn.exe
- assemblies [.NET Framework], signing
- signing files
- strong-named assemblies, signing files
- key pairs for signing files
ms.assetid: c1d2b532-1b8e-4c7a-8ac5-53b801135ec6
ms.openlocfilehash: ca985d808da4f048376774f413cb779b967f3c2c
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653772"
---
# <a name="snexe-strong-name-tool"></a>Sn.exe（强名称工具）

强名称工具 (Sn.exe) 有助于使用[强名称](../../standard/assembly/strong-named.md)对程序集进行签名。 Sn.exe 提供了用于密钥管理、签名生成和签名验证的选项。  
  
> [!WARNING]
> 不要依赖于通过强名称实现安全性。 它们仅提供唯一的标识。

 有关强命名和强命名的程序集的详细信息，请参阅[具有强命名的程序集](../../standard/assembly/strong-named.md)和[如何：使用强名称为程序集签名](../../standard/assembly/sign-strong-name.md)。  
  
 强名称工具自动随 Visual Studio 一起安装。 若要启动该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  

> [!NOTE]
> 在 64 位计算机上，可通过使用 Visual Studio 开发人员命令提示运行 32 位版本的 Sn.exe，也可使用 Visual Studio x64 Win64 命令提示运行 64 位版本的 Sn.exe。
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
sn [-quiet][option [parameter(s)]]  
```  
  
## <a name="parameters"></a>参数  
  
|选项|说明|  
|------------|-----------------|  
|`-a identityKeyPairFile signaturePublicKeyFile`|生成 <xref:System.Reflection.AssemblySignatureKeyAttribute> 数据以将标识密钥从一个文件迁移到签名密钥。|  
|`-ac identityPublicKeyFile identityKeyPairContainer signaturePublicKeyFile`|生成 <xref:System.Reflection.AssemblySignatureKeyAttribute> 数据以将标识密钥从一个密钥容器迁移到签名密钥。|  
|`-c [csp]`|将默认加密服务提供程序 (CSP) 设置为用于强名称签名。 此设置将应用于整台计算机。 如果不指定 CSP 名称，则 Sn.exe 将清除当前设置。|  
|`-d container`|从强名称 CSP 中删除指定的密钥容器。|  
|`-D assembly1 assembly2`|验证两个程序集是否只是签名不同。 在使用不同的密钥对重新为程序集创建签名之后，经常使用这种方式来进行检查。|  
|`-e assembly outfile`|从 assembly 中提取公钥并将其存储在 outfile 中 。|  
|`-h`|显示该工具的命令语法和选项。|  
|`-i infile container`|从指定密钥容器中的 infile 安装密钥对。 密钥容器位于强名称 CSP 中。|  
|`-k [keysize] outfile`|生成一个指定大小的新 <xref:System.Security.Cryptography.RSACryptoServiceProvider> 密钥并将其写入指定的文件。  公钥和私钥都会写入该文件。<br /><br /> 如果不指定密钥大小，并且已安装了 Microsoft 增强加密提供程序，则默认情况下生成 1,024 位的密钥；否则，生成 512 位的密钥。<br /><br /> 如果安装了 Microsoft 增强加密提供程序，则 keysize 参数支持 384 位至 16,384 位（增量为 8 位）的密钥长度。  如果安装了 Microsoft 基本加密提供程序，则它支持长度为 384 位至 512 位（增量为 8 位）的密钥。|  
|`-m [y|n]`|指定密钥容器是特定于计算机的还是特定于用户的。 如果指定 y，则密钥容器是特定于计算机的。 如果指定 n，则密钥容器是特定于用户的。<br /><br /> 如果既没有指定 y 也没有指定 n，则此选项显示当前设置。|  
|`-o infile [outfile]`|从 infile 中提取公钥并将其存储在 .csv 文件中。 公钥的每一字节都由逗号分隔。 这种格式可以用来在源代码中将对密钥的引用硬编码为已初始化的数组。 如果不指定 outfile，则此选项将输出放到剪贴板上。 **注意：** 此选项不验证输入是否只是公钥。 如果 `infile` 包含带有私钥的密钥对，则会同时提取私钥。|  
|`-p infile outfile [hashalg]`|从 infile 中的密钥对中提取公钥，并将其存储在 outfile 中，可选择使用 hashalg 指定的 RSA 算法  。 此公钥可用于通过[程序集链接器 (Al.exe)](al-exe-assembly-linker.md) 的 /delaysign+ 和 /keyfile 选项，对程序集进行延迟签名 。 如果对程序集进行延迟签名，则在编译时只设置公钥，并在文件中为要在以后添加的签名（当私钥已知时）保留空间。|  
|`-pc container outfile [hashalg]`|从 container 中的密钥对中提取公钥并将其存储在 outfile 中 。 如果使用 hashalg 选项，则使用 RSA 算法提取公钥。|  
|`-Pb [y|n]`|指定是否强制执行强名称跳过策略。 如果指定 y，则在将完全信任程序集加载到完全信任 <xref:System.AppDomain> 时，不验证这些程序集的强名称。 如果指定 n，则会验证强名称是否正确，但不会验证是否具有特定强名称。 <xref:System.Security.Permissions.StrongNameIdentityPermission> 对完全信任程序集不起作用。 你必须自己对强名称是否匹配进行检查。<br /><br /> 如果既没有指定 `y` 也没有指定 `n`，则此选项显示当前设置。 默认值为 `y`。 **注意：** 在 64 位计算机上，必须同时在 Sn.exe 的 32 位和 64 位实例中设置此参数。|  
|`-q[uiet]`|指定安静模式；取消显示成功消息。|  
|`-R[a] assembly infile`|使用 infile 中的密钥对，为先前已签名的程序集或延迟签名的程序集重新签名。<br /><br /> 如果使用了 -Ra，则重新计算程序集中所有文件的哈希值。|  
|`-Rc[a] assembly container`|使用 container 中的密钥对，为之前已签名的程序集或延迟签名的程序集重新签名。<br /><br /> 如果使用了 -Rca，则重新计算程序集中所有文件的哈希值。|  
|`-Rh assembly`|重新计算程序集中所有文件的哈希值。|  
|`-t[p] infile`|显示存储在 infile 中的公钥的标记。 infile 的内容必须是以前使用 -p 从密钥对文件生成的公钥。  不要使用 -t[p] 选项直接从密钥对文件提取标记。<br /><br /> Sn.exe 将使用公钥的哈希函数计算标记。 为节省空间，公共语言运行时在记录对具有强名称的程序集的依赖性时，将公钥标记存储在清单中，作为对另一个程序集的引用的一部分。 -tp 选项除显示标记外还显示公钥。 如果 <xref:System.Reflection.AssemblySignatureKeyAttribute> 特性已应用于程序集，则标记用于标识密钥，并显示哈希算法和标识密钥的名称。<br /><br /> 请注意，此选项不验证程序集签名，而且不应用于做出信任决策。  此选项仅显示原始公钥标记数据。|  
|`-T[p] assembly`|显示 assembly 的公钥标记。 assembly 必须是包含程序集清单的文件的名称。<br /><br /> Sn.exe 将使用公钥的哈希函数计算标记。 为节省空间，公共语言运行时在记录对具有强名称的程序集的依赖性时，将公钥标记存储在清单中，作为对另一个程序集的引用的一部分。 -Tp 选项除显示标记外还显示公钥。 如果 <xref:System.Reflection.AssemblySignatureKeyAttribute> 特性已应用于程序集，则标记用于标识密钥，并显示哈希算法和标识密钥的名称。<br /><br /> 请注意，此选项不验证程序集签名，而且不应用于做出信任决策。  此选项仅显示原始公钥标记数据。|  
|`-TS assembly infile`|使用 infile 中的密钥对，对已签名或部分签名的程序集进行测试签名 。|  
|`-TSc assembly container`|使用密钥容器 container 中的密钥对，对已签名或部分签名的程序集进行测试签名 。|
|`-v assembly`|验证 assembly 中的强名称，其中 assembly 是包含程序集清单的文件的名称 。|  
|`-vf assembly`|验证 assembly 中的强名称。 与 -v 选项不同，-vf 会强制实施验证，即使已使用 -Vr 选项禁用了验证也是如此  。|  
|`-Vk regfile.reg assembly [userlist] [infile]`|创建一个注册项 (.reg) 文件，你可以使用它注册要跳过验证的指定程序集。 应用于 -Vr 选项的程序集命名规则也应用于 -Vk 。 有关 userlist 和 infile 选项的信息，请参阅 -Vr 选项 。|  
|`-Vl`|列出此计算机上的强名称验证的当前设置。|  
|`-Vr  assembly [userlist] [infile]`|注册要跳过验证的 assembly。 或者，还可以指定应跳过验证的用户名的逗号分隔的列表。 如果指定 infile，则验证仍会启用，但在验证操作中将使用 infile 中的公钥 。 可以以 \*, strongname 的形式指定 assembly，以注册所有具有指定强名称的程序集 。 对于 strongname，指定表示标记形式的公钥的十六进制数字的字符串。 参见 -t 和 -T 选项以显示公钥标记 。 注意：仅在开发期间使用此选项。 将程序集添加到跳过验证列表会产生安全漏洞。 如果将某程序集添加到跳过验证列表中，则恶意程序集可以使用该程序集的完全指定程序集名称来隐藏身份，完全指定程序集名称由程序集名称、版本、区域性和公钥标记组成。 这使恶意程序集也可以跳过验证。|  
|||  
|`-Vu  assembly`|注销要跳过验证的 assembly。 应用于 -Vr 的相同程序集命名规则也应用于 -Vu 。|  
|`-Vx`|移除所有验证跳过项。|  
|`-?`|显示该工具的命令语法和选项。|  
  
> [!NOTE]
> 所有 Sn.exe 选项都区分大小写，必须完全按所示的形式键入才能被该工具识别。  
  
## <a name="remarks"></a>备注  

 -R 和 -Rc 选项对延迟签名的程序集很有用 。 在这种情况下，编译时只设置公钥，并在以后执行签名（当私钥已知时）。  
  
> [!NOTE]
> 对于写入到受保护资源（如注册表）的参数（例如，–Vr），请以管理员身份运行 SN.exe。  
  
强名称工具假定使用 `AT_SIGNATURE` 算法标识符生成公钥/私钥对。 使用 `AT_KEYEXCHANGE` 算法生成的公钥/私钥对会生成错误。

## <a name="examples"></a>示例  

 下面的命令创建一个新的随机密钥对，并将其存储在 `keyPair.snk` 中。  
  
```console  
sn -k keyPair.snk  
```  
  
 下面的命令将 `keyPair.snk` 中的密钥存储在强名称 CSP 中的容器 `MyContainer` 中。  
  
```console  
sn -i keyPair.snk MyContainer  
```  
  
 下面的命令从 `keyPair.snk` 中提取公钥并将其存储在 `publicKey.snk` 中。  
  
```console  
sn -p keyPair.snk publicKey.snk  
```  
  
 下面的命令显示公钥和 `publicKey.snk` 中包含的公钥的标记。  
  
```console  
sn -tp publicKey.snk  
```  
  
 下面的命令验证程序集 `MyAsm.dll`。  
  
```console  
sn -v MyAsm.dll  
```  
  
 下面的命令从默认 CSP 中删除 `MyContainer`。  
  
```console  
sn -d MyContainer  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Al.exe（程序集链接器）](al-exe-assembly-linker.md)
- [具有强名称的程序集](../../standard/assembly/strong-named.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
