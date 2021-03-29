---
title: Mage.exe（清单生成和编辑工具）
description: 开始使用 Mage.exe（清单生成和编辑工具）。 此工具支持创建和编辑应用程序和部署清单。
ms.date: 12/06/2018
helpviewer_keywords:
- Manifest Generation and Editing tool
- Mage.exe
ms.assetid: 77dfe576-2962-407e-af13-82255df725a1
ms.openlocfilehash: df329d121009e519d2e9efd77dc4d2fbc37d198e
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104654006"
---
# <a name="mageexe-manifest-generation-and-editing-tool"></a>Mage.exe（清单生成和编辑工具）

清单生成和编辑工具 (Mage.exe) 是一种命令行工具，可以支持创建和编辑应用程序和部署清单。 可以从批脚本和其他基于 Windows 的应用程序（包括 ASP.NET 应用程序）运行命令行工具 Mage.exe。

也可以使用一种图形应用程序 MageUI.exe 来代替 Mage.exe 。 有关详细信息，请参阅 [MageUI.exe (Manifest Generation and Editing Tool, Graphical Client)](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)。

此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。

Mage.exe 和 MageUI.exe 的两个版本包含在 Visual Studio 中 。 若要查看版本信息，请运行 MageUI.exe，依次选择“帮助”和“关于” 。 本文档介绍 Mage.exe 和 MageUI.exe 的 4.0.x.x 版本 。

## <a name="syntax"></a>语法

```console
Mage [commands] [commandOptions]
```

## <a name="parameters"></a>参数

下表显示了 Mage.exe 所支持的命令。 有关这些命令所支持的选项的详细信息，请参阅[新建和更新命令选项](#new-and-update-command-options)以及[签名命令选项](#sign-command-options)。

|命令|说明|
|-------------|-----------------|
|**-cc, ClearApplicationCache**|清除所有仅联机应用程序的已下载应用程序缓存。|
|-n, -New fileType [newOptions]|创建给定类型的新文件。 有效类型是：<br /><br /> -   `Deployment`：创建新的部署清单。<br />-   `Application`：创建新的应用程序清单。<br /><br /> 如果不使用此命令指定任何其他参数，此命令将使用合适的默认标记和特性值创建合适的类型的文件。<br /><br /> 使用 **-ToFile** 选项（如下表所示）可指定新文件的文件名和路径。<br /><br /> 使用“-FromDirectory”选项（如下表所示）可创建一个应用程序清单，应用程序的所有程序集都添加到该清单的 \<dependency> 部分。|
|-u, -Update [filePath] [updateOptions]|对清单文件进行一项或多项更改。 不需要指定正在编辑的文件的类型。 Mage.exe 将使用一组试探法检查该文件，并确定该文件是部署清单还是应用程序清单。<br /><br /> 如果已经使用证书对某个文件进行签名， **-Update** 将会移除密钥签名块。 这是因为，密钥签名包含该文件的哈希，修改该文件会呈现无效的哈希。<br /><br /> 使用 **-ToFile** 选项（如下表所示）可指定新文件名和路径，而不是覆盖现有文件。|
|-s, -Sign `[signOptions]`|使用密钥对或 X509 证书对文件签名。 签名将作为 XML 元素插入文件内。<br /><br /> 在为指定 **-TimestampUri** 值的清单签名时，必须连接到 Internet。|
|-ver, -Verify [manifest-filename]|验证清单是否已正确签名。 不能与其他命令结合使用。 <br/><br/>**可在 .NET Framework 4.7 及更高版本中使用。**|
|-h, -?, -Help [verbose]|描述所有可用的命令及其选项。 指定 `verbose` 可获得详细的帮助。|

## <a name="new-and-update-command-options"></a>新建和更新命令选项

下表显示了 `-New` 和 `-Update` 命令支持的选项：

|选项|默认值|适用于|描述|
|-------------|-------------------|----------------|-----------------|
|**-a, -Algorithm**|sha1RSA|应用程序清单。<br /><br /> 部署清单。|指定用来生成依赖项摘要的算法。 值必须为“sha256RSA”或“sha1RSA。<br /><br /> 与“-Update”选项一起使用。 在使用“-Sign”选项卡时，此选项将被忽略。|
|-appc, -AppCodeBase `manifestReference`||部署清单。|插入对应用程序清单文件的 URL 或文件路径引用。 此值必须是应用程序清单的完整路径。|
|-appm, -AppManifest `manifestPath`||部署清单。|将对部署的应用程序清单的引用插入其部署清单中。<br /><br /> `manifestPath` 指示的文件必须存在，否则，Mage.exe 将会发出一个错误。 如果 `manifestPath` 引用的文件不是应用程序清单，Mage.exe 将会发出一个错误。|
|-cf, -CertFile `filePath`||所有文件类型。|指定用于签署清单或许可证文件的 X509 数字证书的位置。 如果该证书需要个人信息交换 (PFX) 文件的密码，此选项可以与“-Password”选项结合使用。 自 .NET Framework 4.7 开始，如果文件不包含私钥，则需要结合使用 -CryptoProvider 和 -KeyContainer 选项。<br/><br/>自 .NET Framework 4.6.2 起，Mage.exe 使用 CNG 和 CAPI 证书对清单进行签名。|
|-ch, -CertHash `hashSignature`||所有文件类型。|存储在客户端计算机的个人证书存储中的数字证书的哈希。 此哈希对应于在 Windows 证书控制台中查看的数字证书的指纹字符串。<br /><br /> `hashSignature` 可以大写或小写，可以作为单个字符串提供，或者与指纹的每个八进制数一起提供（指纹的每个八进制数用空格分隔，并且整个指纹用引号引起来）。|
|-csp, -CryptoProvider `provider-name`||所有文件类型。|指定包含私钥容器的加密服务提供程序 (CSP) 的名称。 此选项需要 -KeyContainer 选项。<br/><br/>自 .NET Framework 4.7 起，可以使用此选项。|
|-fd, -FromDirectory `directoryPath`||应用程序清单。|使用 `directoryPath`（包括所有子目录）中的所有程序集和文件的说明填充应用程序清单，其中 `directoryPath` 是包含要部署的应用程序的目录。 对于该目录中的每个文件，Mage.exe 将确定该文件是程序集还是静态文件。 如果该文件是程序集，它将向应用程序添加 `<dependency>` 标记和 `installFrom` 特性以及程序集的名称、基本代码和版本。 如果该文件是静态文件，它将添加 `<file>` 标记。 另外，Mage.exe 将通过一组简单的试探法检测应用程序的主要可执行文件，并在清单中将其标记为 ClickOnce 应用程序的入口点。<br /><br /> Mage.exe 永远不会将某个文件自动标记为“数据”文件。 此项操作必须手动完成。 有关详细信息，请参阅[如何：将数据文件添加到 ClickOnce 应用程序中](/visualstudio/deployment/how-to-include-a-data-file-in-a-clickonce-application)。<br /><br /> Mage.exe 还可以根据每个文件的大小为其生成哈希。 ClickOnce 使用这些哈希确保自创建清单以来无人篡改部署文件。 如果部署中的任何文件发生更改，可以将 Mage.exe 与“-Update”命令和“-FromDirectory”选项一起运行，Mage.exe 将更新所有引用文件的哈希和程序集版本 。<br /><br /> **-FromDirectory** 将包括 `directoryPath`内所有子目录中的全部文件。<br /><br /> 如果将“-FromDirectory”与“-Update”命令结合使用，Mage.exe 会从应用程序清单中移除该目录中不再存在的所有文件 。|
|-if, -IconFile  `filePath`||应用程序清单。|指定 .ICO 图标文件的完整路径。 此图标显示在“开始”菜单中你的应用程序名称的旁边以及其对应的“添加或删除程序”项中。 如果未提供图标，则使用默认图标。|
|-ip, -IncludeProviderURL  `url`|true|部署清单。|指示部署清单是否包含由 **-ProviderURL** 设置的更新位置值。|
|-i, -Install `willInstall`|true|部署清单。|指示 ClickOnce 应用程序是否应该安装到本地计算机上，或者该应用程序是否应通过 Web 运行。 安装某个应用程序后，该应用程序将会显示在 Windows 的 **“开始”** 菜单中。 有效值为“true”或“t”以及“false”或“f”。<br /><br /> 如果指定 **-MinVersion** 选项，并且用户安装的版本低于 **-MinVersion** ，则无论向 **-Install** 传递何值，该选项都会强制安装应用程序。<br /><br /> 此选项不能与 **-BrowserHosted** 选项一起使用。 尝试为同一清单同时指定这两个选项将会导致错误。|
|-kc, -KeyContainer `name`||所有文件类型。|指定包含私钥名称的密钥容器。 此选项需要 CyproProvider 选项。<br/><br/>自 .NET Framework 4.7 起，可以使用此选项。|
|-mv, -MinVersion  `[version]`|由 **-Version** 标志指定的在 ClickOnce 部署清单中列出的版本。|部署清单。|用户可以运行的此应用程序的最低版本。 此标志使得你的应用程序的命名版本成为必需的更新。 如果发布的产品版本存在对重大更改或严重的安全漏洞的更新，则可以使用此标志指定必须安装此更新，而且用户不能继续运行早期版本。<br /><br /> `version` 与 **-Version** 标志的自变量的语义相同。|
|-n, -Name `nameString`|部署|所有文件类型。|用于标识应用程序的名称。 ClickOnce 将使用此名称在“开始”菜单（如果应用程序配置为自行安装）和“权限提升”对话框中标识应用程序。 注意：如果要更新现有的清单，并且未使用此选项指定发布服务器的名称，则 Mage.exe 将使用计算机上定义的组织名称更新清单。 要使用不同的名称，请确保使用此选项，并指定所需的发布服务器名称。|
|-pwd, -Password `passwd`||所有文件类型。|使用数字证书对清单进行签名时所用的密码。 必须与 **-CertFile** 选项结合使用。|
|-p, Processor `processorValue`|Msil|应用程序清单。<br /><br /> 部署清单。|可用于运行此分发的微处理器体系结构。 如果你准备的一个或多个安装的程序集已针对特定微处理器进行了预编译，则此值是必需的。 有效值包括 `msil`、 `x86`、 `ia64`和 `amd64`。 `msil` 是 Microsoft 中间语言，这表示所有程序集都与平台无关，并且首次运行应用程序时，公共语言运行时 (CLR) 将会对所有程序集进行实时编译。|
|-pu, -ProviderURL `url` ||部署清单。|指定 ClickOnce 要检查是否存在应用程序更新的 URL。|
|-pub, -Publisher `publisherName`||应用程序清单。<br /><br /> 部署清单。|将发布服务器名称添加到部署清单或应用程序清单的描述元素中。 在应用程序清单中使用时，还必须使用值 “true” 或 “t” 来指定 **-UseManifestForTrust** ，否则此参数将引发错误。|
|-s, -SupportURL  `url`||应用程序清单。<br /><br /> 部署清单。|指定在“添加或删除程序”对话框中为 ClickOnce 应用程序显示的链接。|
|-ti, -TimestampUri `uri`||应用程序清单。<br /><br /> 部署清单。|数字时间戳服务的 URL。 如果数字证书在部署应用程序的下一个版本之前过期，为清单加盖时间戳可以让你不必对清单进行重新签名。 有关详细信息，请参阅 [Windows 根证书计划成员](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265983(v=ws.11))。|
|-t, -ToFile `filePath`|-   新建：<br />-   部署：deploy.application<br />-   应用程序：application.exe.manifest<br />-   更新：<br />-   输入文件。|所有文件类型。|指定已经创建或修改的文件的输出路径。<br /><br /> 如果使用 **-New** 时未提供 **-ToFile**，则输出将写入到当前工作目录。 如果使用“-Update”时未提供“-ToFile”，Mage.exe 会将该文件写回到输入文件中 。|
|-tr, -TrustLevel `level`|基于应用程序 URL 所在的区域。|应用程序清单。|要为客户端计算机上的应用程序授予的信任级别。 值包括“Internet”、“Intranet”和“FullTrust”。|
|-um, -UseManifestForTrust `willUseForTrust`|False|应用程序清单。|指定当应用程序在客户端上运行时，是否使用应用程序清单的数字签名来做出信任决定。 如果指定“true”或“t”，则指示将使用应用程序清单来做出信任决定。 如果指定“false”或“f”，则指示将使用部署清单的签名。|
|-v, -Version `versionNumber`|1.0.0.0|应用程序清单。<br /><br /> 部署清单。|部署的版本。 参数必须是格式为“*N.N.N.N*”的有效版本字符串，其中，“*N*”是一个无符号的 32 位整数。|
|-wpf, -WPFBrowserApp  `isWPFApp`|false|应用程序清单。<br /><br /> 部署清单。|仅当应用程序是将在 Internet Explorer 内承载的 Windows Presentation Foundation (WPF) 应用程序且不是独立的可执行文件时，才使用此标志。 有效值为“true”或“t”以及“false”或“f”。<br /><br /> 对于应用程序清单，请在应用程序清单的 `hostInBrowser` 元素下插入 `entryPoint` 特性。<br /><br /> 对于部署清单，请将 `install` 元素上的 `deployment` 特性设置为 false，并使用 .xbap 扩展名保存部署清单。 同时指定此自变量与 **-Install** 自变量将产生一个错误，因为浏览器承载的应用程序不可以是已安装的脱机应用程序。|

## <a name="sign-command-options"></a>签名命令选项

下表显示了 `-Sign` 命令支持的选项。该命令适用于所有类型的文件。

|选项|描述|
|-------------|-----------------|
|-cf, -CertFile `filePath`|指定用于对清单进行签名的数字证书的位置。 如果该证书需要个人信息交换 (PFX) 文件的密码，此选项可以与“-Password”选项结合使用。 自 .NET Framework 4.7 开始，如果文件不包含私钥，则需要结合使用 -CryptoProvider 和 -KeyContainer 选项。<br/><br/>自 .NET Framework 4.6.2 起，Mage.exe 使用 CNG 和 CAPI 证书对清单进行签名。|
|-ch, -CertHash `hashSignature`|存储在客户端计算机的个人证书存储中的数字证书的哈希。 此哈希对应于在 Windows 证书控制台中查看的数字证书的指纹属性。<br /><br /> `hashSignature` 可以大写或小写，可以作为单个字符串提供，或者与指纹的每个八进制数一起提供（指纹的每个八进制数用空格分隔，并且整个指纹用引号引起来）。|
-csp, -CryptoProvider `provider-name`|指定包含私钥容器的加密服务提供程序 (CSP) 的名称。 此选项需要 -KeyContainer 选项。<br/><br/>自 .NET Framework 4.7 起，可以使用此选项。|
|-kc, -KeyContainer `name`|指定包含私钥名称的密钥容器。 此选项需要 CyproProvider 选项。<br/><br/>自 .NET Framework 4.7 起，可以使用此选项。|
|-pwd, -Password `passwd`|使用数字证书对清单进行签名时所用的密码。 必须与 **-CertFile** 选项结合使用。|
|-t, -ToFile `filePath`|指定已经创建或修改的文件的输出路径。|

## <a name="remarks"></a>备注

Mage.exe 的所有参数区分大小写。 命令和选项的前缀可以是短划线 (-) 或正斜杠 (/)。

与 **-Sign** 命令结合使用的所有自变量也可以随时与 **-New** 或 **-Update** 命令结合使用。 以下命令是等效的。

```console
mage -Sign c:\HelloWorldDeployment\HelloWorld.deploy -CertFile cert.pfx
mage -Update c:\HelloWorldDeployment\HelloWorld.deploy -CertFile cert.pfx
```

> [!NOTE]
> 从版本 .NET Framework 4.6.2 开始，还支持 CNG 证书。

 签名是应该执行的最后一项任务，因为签名的文档使用文件的哈希验证签名是否为文档的有效签名。 如果对签名的文件做出任何更改，则必须重新对该文件进行签名。 如果对以前已经签名的文档进行签名，Mage.exe 将会使用新的签名替换原有的签名。

 使用“-AppManifest”选项填充某个部署清单时，Mage.exe 会假定应用程序清单与部署清单位于同一个目录中（在一个用当前部署版本命名的子目录中），并对部署清单进行相应地配置。 如果应用程序清单驻留在其他位置，请使用 **-AppCodeBase** 选项设置替代位置。

 部署清单和应用程序清单必须在部署应用程序之前进行签名。 有关清单签名的指导，请参见 [Trusted Application Deployment Overview](/visualstudio/deployment/trusted-application-deployment-overview)。

 应用程序清单的 **-TrustLevel** 选项描述了应用程序在客户端计算机上运行时所需的权限集。 默认情况下，将根据应用程序的 URL 所在的 *区域* 指派应用程序的信任级别。 通过公司网络部署的应用程序一般放置在 Intranet 区域中，而那些通过 Internet 部署的应用程序放置在 Internet 区域中。 这两个安全区域都会限制应用程序对本地资源的访问，Intranet 区域比 Internet 区域的权限稍大。 FullTrust 区域授予应用程序对计算机本地资源的完全访问权限。 如果使用 -TrustLevel 选项将某个应用程序放置在此区域中，CLR 的信任关系管理器组件将会提示用户确定是否授予这种更高的信任级别。 如果通过公司网络部署应用程序，则可以使用“受信任的应用程序部署”提升该应用程序的信任级别，而无需对用户做出提示。

 应用程序清单还支持自定义信任部分。 这有助于应用程序遵守请求最低权限的安全原则，这是因为你可以对清单进行配置，使其只需要应用程序运行所需的那些特定权限。 Mage.exe 不直接支持添加自定义信任部分。 可以使用文本编辑器、XML 分析器或图形工具 MageUI.exe 添加自定义信任部分。 有关如何使用 MageUI.exe 添加自定义信任部分的详细信息，请参阅 [MageUI.exe（图形化客户端中的清单生成和编辑工具）](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)。

Visual Studio 2017 包括 4.6.1 版 Mage.exe。 使用此版本的 Mage.exe 创建的清单适用于 .NET Framework 4。 若要以早期版本的 .NET Framework 为目标，请使用 Mage.exe 的早期版本。

在现有清单中添加或移除程序集时，或者为现有清单重新签名时，Mage.exe 不会将清单更新为针对 .NET Framework 4。

下表显示了这些功能和限制：

|清单版本|操作|Mage v2.0|Mage v4.0|
|----------------------|---------------|---------------|---------------|
|面向 .NET Framework 2.0 版或 3.x. 版的应用程序的清单|打开|确定|确定|
||关闭|确定|确定|
||保存|确定|确定|
||重新签名|确定|确定|
||新建|确定|不支持|
||更新（如下所示）|确定|确定|
|面向 .NET Framework 版本 4 的应用程序的清单|打开|确定|确定|
||关闭|确定|确定|
||保存|确定|确定|
||重新签名|确定|确定|
||新建|不支持|确定|
||更新（如下所示）|不支持|确定|

|清单版本|更新操作详细信息|Mage v2.0|Mage v4.0|
|----------------------|------------------------------|---------------|---------------|
|面向 .NET Framework 2.0 版或 3.x. 版的应用程序的清单|修改程序集|确定|确定|
||添加程序集|确定|确定|
||删除程序集|确定|确定|
|面向 .NET Framework 版本 4 的应用程序的清单|修改程序集|不支持|确定|
||添加程序集|不支持|确定|
||删除程序集|不支持|确定|

 Mage.exe 创建以 .NET Framework 4 Client Profile 为目标的新清单。 以 .NET Framework 4 Client Profile 为目标的 ClickOnce 应用程序可同时在 .NET Framework 4 Client Profile 和完整版的 .NET Framework 4 上运行。 如果你的应用程序面向完整版的 .NET Framework 4 并且不能在 .NET Framework 4 Client Profile 上运行，请通过使用文本编辑器并对清单重新签名来删除客户端 `<framework>` 元素。

下面是一个面向 .NET Framework 4 Client Profile 的示例 `<framework>` 元素：

```xml
<framework targetVersion="4.0" profile="client" supportedRuntime="4.0.20506" />
```

## <a name="examples"></a>示例

下面的示例将打开 Mage (MageUI.exe) 的用户界面。

```console
mage
```

下面的示例将创建默认的部署清单和应用程序清单。 这两个文件都在当前的工作目录中创建，并且分别命名为 deploy.application 和 application.exe.manifest。

```console
mage -New Deployment
mage -New Application
```

下面的示例将创建使用当前目录中的所有程序集和资源文件填充的应用程序清单。

```console
mage -New Application -FromDirectory . -Version 1.0.0.0
```

下面的示例通过指定部署名称和目标微处理器继续前面的示例。 它还指定了 ClickOnce 检查更新时应该依据的 URL。

```console
mage -New Application -FromDirectory . -Name "Hello, World! Application" -Version 1.0.0.0 -Processor "x86" -ProviderUrl http://internalserver/HelloWorld/
```

下面的示例演示如何创建一对清单，用于部署将在 Internet Explorer 中承载的 WPF 应用程序。

```console
mage -New Application -FromDirectory . -Version 1.0.0.0 -WPFBrowserApp true
mage -New Deployment -AppManifest 1.0.0.0\application.manifest -WPFBrowserApp true
```

下面的示例将创建使用当前目录和签名中的所有程序集和资源文件填充的应用程序清单。

```console
mage -New Application -FromDirectory . -Version 1.0.0.0 -KeyContainer keypair.snk -CryptoProvider "Microsoft Enhanced Cryptographic Provider v1.0"
```

下面的示例使用应用程序清单中的信息更新部署清单，并为应用程序清单的位置设置基本代码。

```console
mage -Update HelloWorld.deploy -AppManifest 1.0.0.0\application.manifest -AppCodeBase http://internalserver/HelloWorld.deploy
```

下面的示例对部署清单进行编辑，以便强制更新用户已安装的版本。

```console
mage -Update c:\HelloWorldDeployment\HelloWorld.deploy -MinVersion 1.1.0.0
```

下面的示例指示部署清单从其他目录中检索应用程序清单。

```console
mage -Update HelloWorld.deploy -AppCodeBase http://anotherserver/HelloWorld/1.1.0.0/
```

下面的示例使用当前工作目录中的数字证书对现有的部署清单进行签名。

```console
mage -Sign deploy.application -CertFile cert.pfx -Password <passwd>
```

下面的示例使用当前工作目录中的数字证书和私钥对现有的部署清单进行签名。

```console
mage -Sign deploy.application -CertFile cert.pfx -KeyContainer keyfile.snk -CryptoProvider "Microsoft Enhanced Cryptographic Provider v1.0"
```

## <a name="see-also"></a>请参阅

- [ClickOnce 安全和部署](/visualstudio/deployment/clickonce-security-and-deployment)
- [演练：手动部署 ClickOnce 应用程序](/visualstudio/deployment/walkthrough-manually-deploying-a-clickonce-application)
- [受信任的应用程序部署概述](/visualstudio/deployment/trusted-application-deployment-overview)
- [MageUI.exe（图形化客户端中的清单生成和编辑工具）](mageui-exe-manifest-generation-and-editing-tool-graphical-client.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
