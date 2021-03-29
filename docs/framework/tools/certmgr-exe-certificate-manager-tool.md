---
title: Certmgr.exe（证书管理器工具）
description: Explore Certmgr.exe（证书管理器工具）。 此工具管理证书、证书信任列表 (CTL) 和证书吊销列表 (CRL)。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- certificates, managing
- CRLs
- certificate trust lists
- Certmgr.exe
- Certificate Manager tool
- CTLs
- certificate revocation lists
ms.assetid: 7e953b43-1374-4bbc-814f-53ca1b6b52bb
ms.openlocfilehash: eba8253a52f9dc533848fc7cbb76566c726495a2
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654188"
---
# <a name="certmgrexe-certificate-manager-tool"></a>Certmgr.exe（证书管理器工具）

证书管理器工具 (Certmgr.exe) 管理证书、证书信任列表 (CTL) 和证书吊销列表 (CRL)。  
  
 安装 Visual Studio 时，将会自动安装证书管理器。 若要启动该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。  
  
> [!NOTE]
> 证书管理器工具 (Certmgr.exe) 是命令行实用程序，而“证书”(Certmgr.msc) 则是 Microsoft 管理控制台 (MMC) 管理单元。 由于 Certmgr.msc 通常位于 Windows 系统目录中，因此在命令行上输入 `certmgr` 可加载“证书”MMC 管理单元（即使已打开 Visual Studio 开发人员命令提示）。 出现这种情况的原因是，在 PATH 环境变量中，“证书”管理单元的路径出现在证书管理器工具的路径之前。 如果你遇到此问题，则可以通过指定可执行文件的路径来执行 Certmgr.exe 命令。
  
 有关 X.509 证书的概述，请参阅[使用证书](../wcf/feature-details/working-with-certificates.md)。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
      certmgr [/add | /del | /put] [options]  
[/s[/r registryLocation]] [sourceStorename]  
[/s[/r registryLocation]] [destinationStorename]  
```  
  
## <a name="parameters"></a>参数  
  
|参数|描述|  
|--------------|-----------------|  
|*sourceStorename*|包含要添加、删除、保存或显示的现有证书、CTL 或 CRL 的证书存储。 这可以是一个存储文件，也可以是一个系统存储。|  
|*destinationStorename*|输出证书存储或文件。|  
  
|选项|描述|  
|------------|-----------------|  
|**/add**|将证书、CTL 和 CRL 添加到证书存储中。|  
|**/all**|当与 **/add** 一起使用时添加所有项。 当与 **/del** 一起使用时删除所有项。当不与 **/add** 或 **/del** 选项一起使用时显示所有项。 **/all** 选项不能与 **/put** 一起使用。|  
|**/c**|当与 **/add** 一起使用时添加证书。 当与 **/del** 一起使用时删除证书。当与 **/put** 一起使用时保存证书。 当不与 **/add**、 **/del** 或 **/put** 选项一起使用时显示证书。|  
|**/CRL**|当与 **/add** 一起使用时添加 CRL。 当与 **/del** 一起使用时删除 CRL。当与 **/put** 一起使用时保存 CRL。 当不与 **/add**、 **/del** 或 **/put** 选项一起使用时显示 CRL。|  
|**/CTL**|当与 **/add** 一起使用时添加 CTL。 当与 **/del** 一起使用时删除 CTL。当与 **/put** 一起使用时保存 CTL。 当不与 **/add**、 **/del** 或 **/put** 选项一起使用时显示 CTL。|  
|**/del**|从证书存储中删除证书、CTL 和 CRL。|  
|/e encodingType|指定证书编码类型。 默认值为 `X509_ASN_ENCODING`。|  
|/f dwFlags|指定存储打开标志。 这是传递到 **CertOpenStore** 的 *dwFlags* 参数。 默认值为 CERT_SYSTEM_STORE_CURRENT_USER。 仅当使用 **/y** 选项时才考虑此选项。|  
|**/h**[**elp**]|显示该工具的命令语法和选项。|  
|/n nam|指定要添加、删除或保存的证书的公用名。 此选项只能用于证书，不能用于 CTL 或 CRL。|  
|**/put**|将证书存储中的 X.509 证书、CTL 或 CRL 保存到文件。 文件以 X.509 格式保存。 **/7** 选项可与 **/put** 选项一起使用，以便用 PKCS #7 格式保存文件。 **/put** 选项后面必须有 **/c**、 **/CTL** 或 **/CRL**。 **/all** 选项不能与 **/put** 一起使用。|  
|/r location|标识系统存储的注册表位置。 仅当指定 **/s** 选项时才考虑此选项。 *location* 必须是以下项之一：<br /><br /> -   `currentUser` 指示证书存储在 HKEY_CURRENT_USER 项下。 这是默认设置。<br />-   `localMachine` 指示证书存储在 HKEY_LOCAL_MACHINE 项下。|  
|**/s**|指示证书存储是系统存储。 如果未指定此选项，则会将存储视为 **StoreFile**。|  
|/sha1 sha1Hash|指定要添加、删除或保存的证书、CTL 或 CRL 的 SHA1 哈希。|  
|**/v**|指定详细模式；显示有关证书、CTL 和 CRL 的详细信息。 此选项不能与 **/add**、 **/del** 或 **/put** 选项一起使用。|  
|/y provider|指定存储提供程序名称。|  
|**/7**|将目标存储保存为 PKCS #7 对象。|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 Certmgr.exe 执行下列基本功能：  
  
- 将证书、CTL 和 CRL 显示到控制台。  
  
- 将证书、CTL 和 CRL 添加到证书存储中。  
  
- 从证书存储中删除证书、CTL 和 CRL。  
  
- 将证书存储中的 X.509 证书、CTL 或 CRL 保存到文件。  
  
 Certmgr.exe 适用于两类证书存储：StoreFile 和系统存储。 不必指定证书存储的类型；Certmgr.exe 能够识别存储类型并执行适当的操作。  
  
 运行 Certmgr.exe 时若不指定任何选项，则将启动 certmgr.msc 管理单元，该管理单元具有一个 GUI，可帮助执行也可通过命令行访问的证书管理任务。 该 GUI 提供了一个导入向导，此向导会将证书、CTL 和 CRL 从磁盘复制到证书存储中。  
  
 可以通过编译并运行以下代码来找到 `sourceStorename` 和 `destinationStorename` 参数的 X509 证书存储的名称。  
  
 [!code-csharp[Tools.CertMgr#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tools.certmgr/cs/storenames1.cs#1)]
 [!code-vb[Tools.CertMgr#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tools.certmgr/vb/storenames1.vb#1)]  
  
 有关证书的详细信息，请参阅[使用证书](../wcf/feature-details/working-with-certificates.md)。  
  
## <a name="examples"></a>示例  

 下面的命令显示一个名为 `my` 且包含详细输出的默认系统存储。  
  
```console  
certmgr /v /s my  
```  
  
 下面的命令将名为 `myFile.ext` 的文件中的所有证书添加到一个名为 `newFile.ext` 的新文件中。  
  
```console  
certmgr /add /all /c myFile.ext newFile.ext  
```  
  
 下面的命令将名为 `testcert.cer` 的文件中的证书添加到 `my` 系统存储中。  
  
```console  
certmgr /add /c testcert.cer /s my  
```  
  
 下面的命令将名为 `TrustedCert.cer` 的文件中的证书添加到根证书存储区内。  
  
```console  
certmgr /c /add TrustedCert.cer /s root  
```  
  
 下面的命令将 `myCert` 系统存储中具有公用名 `my` 的证书保存到一个名为 `newCert.cer` 的文件中。  
  
```console  
certmgr /add /c /n myCert /s my newCert.cer  
```  
  
 下面的命令删除 `my` 系统存储中的所有 CTL，并将生成的存储保存到一个名为 `newStore.str` 的文件中。  
  
```console  
certmgr /del /all /ctl /s my newStore.str  
```  
  
 下面的命令将 `my` 系统存储中的一个证书保存到 `newFile` 文件中。 系统将提示你输入 `my` 中的要用于放置 `newFile` 的证书编号。  
  
```console  
certmgr /put /c /s my newFile  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [Makecert.exe（证书创建工具）](/windows/desktop/SecCrypto/makecert)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
