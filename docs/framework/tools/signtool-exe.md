---
title: SignTool.exe（签名工具）
description: 了解 SignTool.exe（签名工具）。 此命令行工具用于对文件进行数字签名，验证文件中的签名，以及将时间戳应用于文件。
ms.date: 03/30/2017
helpviewer_keywords:
- Sign tool
- SignTool.exe
ms.assetid: 0c25ff6c-bff3-422e-b017-146a3ee86cb9
ms.openlocfilehash: 3b4bfa70ed91f98ae49b6df3a5a29f68b30f1e0a
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653850"
---
# <a name="signtoolexe-sign-tool"></a>SignTool.exe（签名工具）

签名工具是一个命令行工具，用于对文件进行数字签名，以及验证文件和时间戳文件中的签名。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

> [!Note]  
> Windows 10 SDK、Windows 10 HLK、Windows 10 WDK 和 Windows 10 ADK 内部版本 20236 及更高版本需要指定摘要算法。 SignTool `sign` 命令需要 `/fd` 文件摘要算法和 `/td` 时间戳摘要算法选项在签名和添加时间戳期间分别指定。 如果在签名期间未指定 `/fd` 并且在添加时间戳期间未指定 `/td`，将引发警告（最初称为错误代码 0）。 在 SignTool 的更高版本中，警告将变成错误。 建议使用 SHA256，它被认为比行业 SHA1 更安全。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
signtool [command] [options] [file_name | ...]  
```  
  
## <a name="parameters"></a>参数  
  
|参数|说明|  
|--------------|-----------------|  
|`command`|指定要对文件执行的操作的四个命令（`catdb`、`sign`、`Timestamp` 或 `Verify`）之一。 有关每个命令的说明，请参见下一个表。|  
|`options`|用于修改命令的选项。 除全局 `/q` 和 `/v` 选项之外，每个命令均支持一组唯一选项。|  
|`file_name`|要进行签名的文件的路径。|  
  
 签名工具支持下列命令。 每个命令均与不同的选项集结合使用，这些选项集已在其各自的节中列出。  
  
|Command|说明|  
|-------------|-----------------|  
|`catdb`|在目录数据库中添加或移除目录文件。 目录数据库用于自动查找目录文件，并由 GUID 标识。 有关 `catdb` 命令支持的选项列表，请参阅 [catdb 命令选项](signtool-exe.md#catdb)。|  
|`sign`|对文件进行数字签名。 数字签名可以阻止文件被篡改，并且使用户能够基于签名证书验证签名者。 有关 `sign` 命令支持的选项列表，请参阅 [sign 命令选项](signtool-exe.md#sign)。|  
|`Timestamp`|为文件添加时间戳。 有关 `TimeStamp` 命令支持的选项列表，请参阅 [TimeStamp 命令选项](signtool-exe.md#TimeStamp)。|  
|`Verify`|通过确定签名证书是否由受信任的颁发机构颁发、是否已撤消签名证书，以及签名证书对于特定策略是否有效（可选）来验证文件的数字签名。 有关 `Verify` 命令支持的选项列表，请参阅 [Verify 命令选项](signtool-exe.md#Verify)。|  
  
 下列选项适用于所有签名工具命令。  
  
|全局选项|描述|  
|-------------------|-----------------|  
|**/q**|如果命令运行成功，则不显示输出；如果命令运行失败，则显示最小输出。|  
|**/v**|无论命令是否运行成功，都显示详细输出，并显示警告消息。|  
|**/debug**|显示调试信息。|  
  
<a name="catdb"></a>

## <a name="catdb-command-options"></a>catdb 命令选项  

 下表列出了可与 `catdb` 命令一起使用的选项。  
  
|Catdb 选项|描述|  
|------------------|-----------------|  
|`/d`|指定更新默认目录数据库。 如果 `/d` 和 `/g` 选项都未使用，则签名工具会更新系统组件和驱动程序数据库。|  
|`/g` GUID|指定由全局唯一标识符 GUID 标识的目录数据库已更新。|  
|`/r`|从目录数据库中移除指定的目录。 如果未指定该选项，签名工具将向目录数据库添加指定目录。|  
|`/u`|指定自动为添加的目录文件生成唯一名称。 如有必要，重命名目录文件以阻止与现有目录文件发生名称冲突。 如果未指定该选项，签名工具将覆盖与所添加的目录同名的任何现有目录。|  
  
<a name="sign"></a>

## <a name="sign-command-options"></a>sign 命令选项  

 下表列出了可与 `sign` 命令一起使用的选项。  
  
|Sign 命令选项|描述|  
|-------------------------|-----------------|  
|`/a`|自动选择最佳签名证书。 签名工具将查找满足所有指定条件的所有有效证书，并选择有效时间最长的证书。 如果未提供该选项，签名工具仅查找一个有效的签名证书。|  
|`/ac`  file|将 file 中的其他证书添加到签名块。|  
|`/as`|追加此签名。 如果不存在主签名，则改为使此签名成为主签名。|  
|`/c`  CertTemplateName|指定用于对证书进行签名的证书模板名（一个 Microsoft 扩展）。|  
|`/csp`  CSPName|指定包含私钥容器的加密服务提供程序 (CSP)。|  
|`/d`  Desc|指定已签名内容的说明。|  
|`/du`  URL|为已签名内容的详细说明指定统一资源定位器 (URL)。|  
|`/f`  SignCertFile|指定文件中的签名证书。 如果文件采用个人信息交换 (PFX) 格式且受密码保护，则使用 `/p` 选项指定密码。 如果文件不包含私钥，则使用 `/csp` 和 `/kc` 选项指定 CSP 和私钥容器名。|  
|`/fd`|指定要用于创建文件签名的文件摘要算法。 </br> **注意：** 如果在签名时未提供 `/fd` 开关，则会生成警告。 默认算法为 SHA1，但建议使用 SHA256。|
|`/fd` certHash|指定字符串 certHash 将默认使用用于签名证书的算法。 </br> **注意：** 仅适用于 Windows 10 工具包内部版本 20236 及更高版本。|  
|`/i`  IssuerName|指定签名证书的颁发者的名称。 该值可以是整个颁发者名称的子字符串。|  
|`/kc`  PrivKeyContainerName|指定私钥容器名。|  
|`/n`  SubjectName|指定签名证书的主题的名称。 该值可以是整个主题名称的子字符串。|  
|`/nph`|如果支持，则取消可执行文件的页面哈希。 默认值由 SIGNTOOL_PAGE_HASHES 环境变量和 wintrust.dll 版本决定。 对于非 PE 文件，忽略此选项。|  
|`/p`  Password|指定打开 PFX 文件时要使用的密码。 （使用 `/f` 选项指定 PFX 文件。）|  
|`/p7` *路径*|指定为每个指定的内容文件生成的公钥加密标准 (PKCS) #7 文件。 PKCS #7 文件命名为 path\\filename.p7 。|  
|`/p7ce` Value|为已签名的 PKCS #7 内容指定选项。 将 Value 设置为“嵌入的”，可将已签名内容嵌入到 PKCS #7 文件中；如果设置为“DetachedSignedData”，则可生成分离的 PKCS #7 文件的已签名数据部分。 如果未使用 `/p7ce` 选项，默认情况下将嵌入已签名的内容。|  
|`/p7co` *\<OID>*|指定标识已签名的 PKCS #7 内容的对象标识符 (OID)。|  
|`/ph`|如果支持，则生成可执行文件的页面哈希。|  
|`/r`  RootSubjectName|指定签名证书必须链接到的根证书的主题名称。 该值可以是根证书的整个主题名称的子字符串。|  
|`/s`  StoreName|指定要在搜索证书时打开的存储。 如果未指定该选项，则打开 `My` 存储。|  
|`/sha1`  Hash|指定签名证书的 SHA1 哈希。 当多个证书满足剩余开关指定的条件时，通常会指定 SHA1 哈希。|  
|`/sm`|指定使用计算机存储，而不是用户存储。|  
|`/t`  URL|指定时间戳服务器的 URL。 如果该选项（或 `/tr`）不存在，将不会对签名文件执行时间戳操作。 如果时间戳操作失败，将生成一个警告。 此选项不能与 `/tr` 选项一起使用。|  
|`/td`  alg|将此选项与 `/tr` 选项一起使用可请求 RFC 3161 时间戳服务器使用的摘要算法。 </br> **注意：** 如果在添加时间戳时未提供 `/td` 开关，则会生成警告。 默认算法为 SHA1，但建议使用 SHA256。 <br/> `/td` 开关必须在 `/tr` 开关之后（而不是之前）声明。 如果 `/td` 开关在 `/tr` 开关之前声明，则返回的时间戳来自 SHA1 算法，而不是来自预期的 SHA256 算法。 |
|`/tr`  URL|指定 RFC 3161 时间戳服务器的 URL。 如果该选项（或 `/t`）不存在，将不会对签名文件执行时间戳操作。 如果时间戳操作失败，将生成一个警告。 此选项不能与 `/t` 选项一起使用。|
|`/u`  Usage|指定签名证书中必须存在的增强型密钥用法 (EKU)。 可以通过 OID 或字符串指定该用法的值。 默认用法为“代码签名”(1.3.6.1.5.5.7.3.3)。|  
|`/uw`|指定“Windows 系统组件验证”(1.3.6.1.4.1.311.10.3.6) 的用法。|  
  
 有关用法示例，请参阅 [Using SignTool to Sign a File](/windows/desktop/SecCrypto/using-signtool-to-sign-a-file)（使用 SignTool 为文件签名）。  
  
<a name="TimeStamp"></a>

## <a name="timestamp-command-options"></a>TimeStamp 命令选项  

 下表列出了可与 `TimeStamp` 命令一起使用的选项。  
  
|TimeStamp 选项|描述|  
|----------------------|-----------------|  
|`/p7`|对 PKCS #7 文件执行时间戳操作。|  
|`/t`  URL|指定时间戳服务器的 URL。 要执行时间戳操作的文件必须在以前已进行签名。 需要 `/t` 或 `/tr` 选项。|  
|`/td`  alg|将此选项与 `/tr` 选项一起使用可请求 RFC 3161 时间戳服务器使用的摘要算法。 </br> **注意：** 如果在添加时间戳时未提供 `/td` 开关，则会生成警告。 默认算法为 SHA1，但建议使用 SHA256。 <br/> `/td` 开关必须在 `/tr` 开关之后（而不是之前）声明。 如果 `/td` 开关在 `/tr` 开关之前声明，则返回的时间戳来自 SHA1 算法，而不是来自预期的 SHA256 算法。 |
|`/tp` index|对 index 处的签名进行时间戳操作。|  
|`/tr`  URL|指定 RFC 3161 时间戳服务器的 URL。 要执行时间戳操作的文件必须在以前已进行签名。 需要 `/tr` 或 `/t` 选项。|  
  
 有关使用示例，请参阅 [Adding Time Stamps to Previously Signed Files](/windows/desktop/SecCrypto/adding-time-stamps-to-previously-signed-files)（向之前已签名的文件添加时间戳）。  
  
<a name="Verify"></a>

## <a name="verify-command-options"></a>Verify 命令选项  
  
|Verify 选项|描述|  
|-------------------|-----------------|  
|`/a`|指定可以使用所有方法来验证文件。 首先，搜索目录数据库以确定是否在目录中对文件进行签名。 如果未在任何目录中对文件进行签名，签名工具将尝试验证文件的嵌入签名。 验证可以或不能在目录中进行签名的文件时，建议使用该选项。 这些文件的示例包括 Windows 文件或驱动程序。|  
|`/ad`|使用默认的目录数据库查找目录。|  
|`/ag` CatDBGUID|在由 CatDBGUID 标识的目录数据库中查找目录。|  
|`/all`|验证包含多个签名的文件中的所有签名。|  
|`/as`|使用系统组件（驱动程序）目录数据库查找目录。|  
|`/c` CatFile|通过名称指定目录文件。|  
|`/d`|指定签名工具应打印描述和描述 URL。|  
|`/ds`  Index|验证指定位置的签名。|  
|`/hash` (`SHA1`&#124;`SHA256`)|指定在目录中搜索文件时要使用的可选哈希算法。|  
|`/kp`|指定应使用内核模式驱动程序签名策略执行验证。|  
|`/ms`|使用多个验证语义。 这是 Windows 8 和更高版本上的 [WinVerifyTrust](/windows/desktop/api/wintrust/nf-wintrust-winverifytrust) 调用的默认行为。|  
|`/o` Version|按操作系统版本验证文件。 版本具有以下格式：PlatformID：VerMajor.VerMinor.BuildNumber   。 PlatformID 表示 <xref:System.PlatformID> 枚举成员的基础值。 **重要提示：** 建议使用 `/o` 开关。 如果未指定 `/o`，SignTool.exe 可能会返回意外的结果。 例如，如果你未将 `/o` 开关包含在内，则能在旧版操作系统上正确验证的系统目录可能在新版操作系统上无法正确验证。|  
|`/p7`|验证 PKCS #7 文件。 无现有策略用于 PKCS #7 验证。 该签名处于选中状态，并为签名证书生成了链。|  
|`/pa`|指定应使用默认认证码验证策略。 如果未指定 `/pa` 选项，签名工具将使用 Windows 驱动程序验证策略。 此选项不能与 `catdb` 选项一起使用。|  
|`/pg` PolicyGUID|通过 GUID 指定验证策略。 PolicyGUID 相当于验证策略的 ActionID。 此选项不能与 `catdb` 选项一起使用。|  
|`/ph`|指定签名工具应打印并验证页面哈希值。|  
|`/r` RootSubjectName|指定签名证书必须链接到的根证书的主题名称。 该值可以是根证书的整个主题名称的子字符串。|  
|`/tw`|指定在未对签名进行时间戳操作时应生成警告。|  
  
 有关用法示例，请参阅 [Using SignTool to Verify a File Signature](/windows/desktop/SecCrypto/using-signtool-to-verify-a-file-signature)（使用 SignTool 验证文件签名）。  
  
## <a name="return-value"></a>返回值  

 当其终止时，签名工具将返回下列退出代码之一。  
  
|退出代码|说明|  
|---------------|-----------------|  
|0|执行成功。|  
|1|执行失败。|  
|2|执行完成，但出现警告。|  

## <a name="examples"></a>示例  

 以下命令将目录文件 MyCatalogFileName.cat 添加到系统组件和驱动程序数据库中。 如有必要阻止替换名为 `/u` 的现有目录文件，`MyCatalogFileName.cat` 选项会生成唯一名称。  
  
```console  
signtool catdb /v /u MyCatalogFileName.cat  
```  
  
 以下命令通过使用最佳证书对文件进行自动签名。  
  
```console
signtool sign /a /fd SHA256 MyFile.exe
```

 以下命令使用存储在受密码保护的 PFX 文件中的证书对文件进行数字签名。  
  
```console  
signtool sign /f MyCert.pfx /p MyPassword /fd SHA256 MyFile.exe
```  
  
 以下命令对文件进行数字签名并加盖时间戳。 用于对文件进行签名的证书存储在 PFX 文件中。  
  
```console  
signtool sign /f MyCert.pfx /t http://timestamp.digicert.com /fd SHA256 MyFile.exe
```  
  
 以下命令通过使用位于 `My` 存储中的证书对文件进行签名，该证书的主题名为 `My Company Certificate`。  
  
```console  
signtool sign /n "My Company Certificate" /fd SHA256 MyFile.exe
```  
  
 以下命令对 ActiveX 控件进行签名，并提供在系统提示用户安装此控件时由 Internet Explorer 显示的信息。  
  
```console  
Signtool sign /f MyCert.pfx /d: "MyControl" /du http://www.example.com/MyControl/info.html /fd SHA256 MyControl.exe
```  
  
 以下命令对已进行数字签名的文件加盖时间戳。  
  
```console  
signtool timestamp /t http://timestamp.digicert.com MyFile.exe
```  

以下命令使用 RFC 3161 时间戳服务器为文件添加时间戳。  
  
```console  
signtool timestamp /tr http://timestamp.digicert.com /td SHA256 MyFile.exe
```  
  
 以下命令确认文件已签名。  
  
```console  
signtool verify MyFile.exe  
```  
  
 以下命令验证可能已在目录中签名的系统文件。  
  
```console  
signtool verify /a SystemFile.dll  
```  
  
 以下命令验证已在名为 `MyCatalog.cat` 目录中签名的系统文件。  
  
```console  
signtool verify /c MyCatalog.cat SystemFile.dll  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
