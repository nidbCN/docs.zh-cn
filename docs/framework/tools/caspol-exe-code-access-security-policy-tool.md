---
title: Caspol.exe（代码访问安全策略工具）
description: 查看 Caspol.exe，即代码访问安全 (CAS) 策略工具。 此工具可让用户和管理员修改不同策略级别的安全策略。
ms.date: 03/30/2017
helpviewer_keywords:
- permission sets, modifying security policy
- security policy [.NET Framework], Code Access Security Policy tool
- enterprise security policy
- referencing code groups and permission sets
- user security policy
- Caspol.exe
- Code Access Security Policy tool
- code groups, modifying security policy
- application development [.NET Framework], security
- machine security policy
- security policy [.NET Framework], modifying
- manually editing security configuration files
ms.assetid: d2bf6123-7b0c-4e60-87ad-a39a1c3eb2e0
ms.openlocfilehash: 402f9e2c4ca378af4b6ed0cc0c8c39e2d337876d
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104652888"
---
# <a name="caspolexe-code-access-security-policy-tool"></a>Caspol.exe（代码访问安全策略工具）

代码访问安全性 (CAS) 策略工具 (Caspol.exe) 使用户和管理员可修改计算机策略级别、用户策略级别和企业策略级别的安全策略。  
  
> [!IMPORTANT]
> 从 .NET Framework 4 开始，Caspol.exe 不再影响 CAS 策略，除非将 [\<legacyCasPolicy> 元素](../configure-apps/file-schema/runtime/netfx40-legacysecuritypolicy-element.md)设置为 `true`。 CasPol.exe 显示或修改的任何设置将只会影响选择使用 CAS 策略的应用程序。 有关详细信息，请参阅[安全更改](/previous-versions/dotnet/framework/security/security-changes)。  
  
> [!NOTE]
> 64 位计算机同时包括 64 位和 32 位版本的安全策略。 若要确保你的策略更改同时应用于 32 位和 64 位应用程序，请同时运行 Caspol.exe 的 32 位和 64 位版本。  
  
 代码访问安全策略工具自动随 .NET Framework 和 Visual Studio 一起安装。 可以在 32 位系统的 %windir%\Microsoft.NET\Framework\\*version* 中或是 64 位系统的 %windir%\Microsoft.NET\Framework64\\*version* 中找到 Caspol.exe。 （例如，对于 64 位系统上的 .NET Framework 4，相应的位置是 %windir%\Microsoft.NET\Framework64\v4.030319\caspol.exe。）如果计算机并行运行多个 .NET Framework 版本，则可能会安装该工具的多个版本。 可以从安装目录运行该工具。 但是，我们建议你使用 [Visual studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)，它们不需要导航到安装文件夹。  
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console
caspol [options]  
```  
  
## <a name="parameters"></a>参数  
  
|选项|描述|  
|------------|-----------------|  
|-addfulltrust assembly_file<br /><br /> or<br /><br /> -af assembly_file|将实现自定义安全对象（如自定义权限或自定义成员资格条件）的程序集添加到特定策略级别的完全信任程序集列表中。 *assembly_file* 参数指定要添加的程序集。 此文件必须用[强名称](../../standard/assembly/strong-named.md)签名。 可以通过[强名称工具 (Sn.exe)](sn-exe-strong-name-tool.md) 使用强名称对程序集进行签名。<br /><br /> 每当将包含自定义权限的权限集添加到策略时，必须将实现该自定义权限的程序集添加到该策略级别的完全信任列表中。 对于实现用于安全策略（如计算机策略）的自定义安全对象（如自定义代码组或成员资格条件）的程序集，应该总是将其添加到完全信任程序集列表中。 **注意：** 如果实现自定义安全对象的程序集引用了其他程序集，则必须首先将被引用的程序集添加到完全信任程序集列表中。 使用 Visual Basic、C++ 和 JScript 创建的自定义安全对象分别引用 Microsoft.VisualBasic.dll、Microsoft.VisualC.dll 和 Microsoft.JScript.dll。 默认情况下，完全信任程序集列表中不包含这些程序集。 在添加自定义安全对象之前，必须将相应的程序集添加到完全信任列表中。 如果不这样做，将会破坏安全系统，导致所有程序集都无法加载。 在这种情况中，Caspol.exe **-all -reset** 选项不会修复安全系统。 若要修复安全系统，必须手动编辑安全文件，移除自定义安全对象。|  
|**-addgroup** {*parent_label &#124; parent_name*} *mship pset_name* [*flags*]<br /><br /> or<br /><br /> **-ag** {*parent_label &#124; parent_name*} *mship pset_name* [*flags*]|将新的代码组添加到代码组层次结构中。 可以指定 *parent_label* 或 *parent_name*。 *parent_label* 参数指定代码组的标签（如 1. 或 1.1.），该代码组是要添加的代码组的父级。 *parent_name* 参数指定代码组的名称，该代码组是要添加的代码组的父级。 因为 *parent_label* 和 *parent_name* 可互换使用，所以 Caspol.exe 必须能够区分它们。 因此，*parent_name* 不能以数字开头。 此外，*parent_name* 只能包含 A-Z、0-9 和下划线字符。<br /><br /> *mship* 参数指定新代码组的成员资格条件。 有关详细信息，请参见参阅本节后面的 *mship* 参数表。<br /><br /> *pset_name* 参数是将与新代码组关联的权限集的名称。 还可以为新代码组设置一个或多个 *flags*。 有关详细信息，请参阅本节后面的 *flags* 参数表。|  
|**-addpset** {*psfile* &#124; *psfile* p *set_name*}<br /><br /> or<br /><br /> -ap {named_psfile &#124; psfile pset_name}   |将新的命名权限集添加到策略。 权限集必须用 XML 编写并存储在 .xml 文件中。 如果 XML 文件包含权限集的名称，则只指定该文件 (*psfile*)。 如果 XML 文件不包含权限集名称，则必须同时指定 XML 文件名 (*psfile*) 和权限集名称 (*pset_name*)。<br /><br /> 请注意，权限集中使用的所有权限都必须在全局程序集缓存中包含的程序集中进行定义。|  
|**-a**[**ll**]|指示此选项后面的所有选项都应用于计算机策略、用户策略和企业策略。 **-all** 选项始终引用当前登录的用户的策略。 查看 **-customall** 选项，引用当前用户以外的用户的用户策略。|  
|**-chggroup** {*label &#124;name*} {*mship* &#124; *pset_name* &#124;<br /><br /> flags `}`<br /><br /> or<br /><br /> **-cg** {*label &#124;name*} {*mship* &#124; *pset_name* &#124;<br /><br /> flags `}`|更改代码组的成员资格条件、权限集或者 **exclusive**、**levelfinal**、**name** 或 **description** 标志的设置。 可以指定 *label* 和 *name* 中的任意一个。 *label* 参数指定代码组的标签（如 1. 或 1.1.）。 *name* 参数指定要更改的代码组的名称。 因为 *label* 和 *name* 可互换使用，所以 Caspol.exe 必须能够区分它们。 因此，*name* 不能以数字开头。 此外，*name* 只能包含 A-Z、0-9 和下划线字符。<br /><br /> *pset_name* 参数指定与代码组关联的权限集的名称。 有关 *mship* 和 *flags* 参数的信息，请参见本节后面的表。|  
|-chgpset  psfile pset_name<br /><br /> or<br /><br /> -cp psfile pset_name|更改命名权限集。 *psfile* 参数为权限集提供新的定义；它是 XML 格式的序列化权限集文件。 *pset_name* 参数指定要更改的权限集的名称。|  
|-customall  path<br /><br /> or<br /><br /> -ca  path|指示此选项后面的所有选项都应用于计算机策略、企业策略和指定的自定义用户策略。 必须用 *path* 参数指定自定义用户的安全配置文件的位置。|  
|**-cu**[**stomuser**] *path*|允许管理不属于当前 Caspol.exe 正代表其运行的用户的自定义用户策略。 必须用 *path* 参数指定自定义用户的安全配置文件的位置。|  
|**-enterprise**<br /><br /> or<br /><br /> **-en**|指示此选项后面的所有选项都应用于企业级策略。 非企业管理员用户尽管可以查看企业级策略，但没有足够的权限修改它。 在非企业方案中，默认情况下此策略不干预计算机策略和用户策略。|  
|**-e**[**xecution**] {**on** &#124; **off**}|打开或关闭在代码开始执行前检查运行权限的机制。 **注意：** 在 .NET Framework 4 及更高版本中，此开关已删除。 有关详细信息，请参阅[安全更改](/previous-versions/dotnet/framework/security/security-changes)。|  
|**-f**[**orce**]|取消此工具的自销毁测试并按照用户的指定更改策略。 通常，Caspol.exe 会检查任何策略的更改是否会妨碍 Caspol.exe 本身的正常运行；如果是，则 Caspol.exe 不会保存策略更改，并会输出错误消息。 若即使在策略更改会妨碍 Caspol.exe 本身运行的情况下也要强制 Caspol.exe 更改策略，则使用 **–force** 选项。|  
|**-h**[**elp**]|显示 Caspol.exe 的命令语法和选项。|  
|**-l**[**ist**]|列出代码组层次结构及指定的计算机、用户、企业或所有策略级别的权限集。 Caspol.exe 首先显示代码组的标签，如果名称不是 null 的话，后面接着显示名称。|  
|**-listdescription**<br /><br /> or<br /><br /> **-ld**|列出指定策略级别的所有代码组描述。|  
|**-listfulltrust**<br /><br /> or<br /><br /> **-lf**|列出指定策略级别的完全信任程序集列表的内容。|  
|**-listgroups**<br /><br /> or<br /><br /> **-lg**|显示指定策略级别或全部策略级别的代码组。 Caspol.exe 首先显示代码组的标签，如果名称不是 null 的话，后面接着显示名称。|  
|**-listpset** or **-lp**|显示指定策略级别或全部策略级别的权限集。|  
|**-m**[**achine**]|指示此选项后面的所有选项都应用于计算机级别策略。 非管理员用户尽管可以查看计算机策略，但没有足够的权限修改它。 对于管理员来说， **-machine** 是默认选项。|  
|**-polchgprompt** {**on** &#124; **off**}<br /><br /> or<br /><br /> **-pp** {**on** &#124; **off**}|启用或禁用每当 Caspol.exe 使用可能会导致策略更改的选项运行时所显示的提示。|  
|**-quiet**<br /><br /> or<br /><br /> **-q**|暂时禁用通常会为导致策略更改的选项显示的提示。 全局更改提示设置不会发生更改。 应逐个命令地使用此选项以避免对所有的 Caspol.exe 命令禁用提示。|  
|**-r**[**ecover**]|从备份文件恢复策略。 每当策略更改时，Caspol.exe 会将旧的策略存储在备份文件中。|  
|-remfulltrust assembly_file<br /><br /> or<br /><br /> -rf  assembly_file|从策略级别的完全信任列表中移除程序集。 如果包含自定义权限的权限集不再由策略使用，则应该执行此操作。 但是，仅当程序集不再实现任何其他仍在使用的自定义权限时，才应从完全信任列表中移除实现自定义权限的程序集。 当从列表中移除程序集时，应同时移除它依赖的任何其他程序集。|  
|**-remgroup** {*label &#124;name*}<br /><br /> or<br /><br /> **-rg** {l *abel &#124; name*}|移除由本身的标签或名称指定的代码组。 如果指定的代码组有子代码组，则 Caspol.exe 还将移除所有子代码组。|  
|-rempset pset_name<br /><br /> or<br /><br /> -rp pset_name|从策略中移除指定的权限集。 *pset_name* 参数指示要移除的权限集。 仅当权限集不与任何代码组关联时，Caspol.exe 才会将其移除。 无法移除默认（内置）权限集。|  
|**-reset**<br /><br /> or<br /><br /> **-rs**|使策略返回到默认状态并将其保存到磁盘上。 每当已更改的策略似乎无法修复并且你想使用安装默认值重新开始时，这非常有用。 当要使用默认策略作为修改特定安全配置文件的起点时，重置也非常方便。 有关详细信息，请参阅[手动编辑安全配置文件](#cpgrfcodeaccesssecuritypolicyutilitycaspolexeanchor1)。|  
|**-resetlockdown**<br /><br /> or<br /><br /> **-rsld**|将策略返回到限制更多的默认状态版本，并将其保存到磁盘；创建以前的计算机策略的备份，并将其保存到名为 `security.config.bac` 的文件中。  锁定的策略类似于默认策略，但锁定的策略不向 `Local Intranet`、`Trusted Sites` 和 `Internet` 区域中的代码以及没有子代码组的对应代码组授予权限。|  
|-resolvegroup assembly_file<br /><br /> or<br /><br /> -rsg  assembly_file|显示特定的程序集 (*assembly_file*) 所属的代码组。 默认情况下，此选项显示程序集所属的计算机、用户和企业策略级别。 若要只查看一个策略级别，请将此选项与 **-machine**、 **-user** 或 **-enterprise** 选项之一一起使用。|  
|-resolveperm assembly_file<br /><br /> or<br /><br /> -rsp assembly_file|显示在允许运行程序集的情况下指定的（或默认的）安全策略级别将会授予程序集的所有权限。 *assembly_file* 参数指定程序集。 如果指定 **-all** 选项，则 Caspol.exe 将基于用户策略、计算机策略和企业策略计算程序集的权限；否则，应用默认的行为规则。|  
|**-s**[**ecurity**] {**on** &#124; **off**}|打开或关闭代码访问安全性。 指定 **-s off** 选项不会禁用基于角色的安全性。 **注意：** 在 .NET Framework 4 及更高版本中，此开关已删除。 有关详细信息，请参阅[安全更改](/previous-versions/dotnet/framework/security/security-changes)。 注意：当禁用代码访问安全性时，所有代码访问要求都会成功。 禁用代码访问安全性会使系统容易受到恶意代码（如病毒和蠕虫）的攻击。 关闭安全性会在某些方面提高性能，但应该只有在已采取其他安全性措施以确保整个系统安全性不受破坏时才可以使用。 其他的安全防范措施包括与公共网络断开连接、从物理上保证计算机的安全等等。|  
|**-u**[**ser**]|指示此选项后面的所有选项都应用于 Caspol.exe 正在代表其运行的用户的用户级别策略。 对于非管理员用户来说， **-user** 是默认选项。|  
|**-?**|显示 Caspol.exe 的命令语法和选项。|  
  
 指定代码组的成员资格条件的 *mship* 参数可以与 **-addgroup** 选项和 **-chggroup** 选项一起使用。 每个 *mship* 参数都作为 .NET Framework 类实现。 若要指定 *mship*，请使用下列参数之一。  
  
|参数|描述|  
|--------------|-----------------|  
|**-allcode**|指定所有代码。 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.AllMembershipCondition?displayProperty=nameWithType>。|  
|**-appdir**|指定应用程序目录。 如果指定 **–appdir** 作为成员资格条件，则代码的 URL 证据将与代码的应用程序目录证据进行比较。 如果两个证据值相同，则满足此成员资格条件。 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.ApplicationDirectoryMembershipCondition?displayProperty=nameWithType>。|  
|-custom  xmlfile|添加自定义成员资格条件。 强制性 *xmlfile* 参数指定包含自定义成员资格条件的 XML 序列化的 .xml 文件。|  
|-hash hashAlg {-hex hashValue &#124; -file assembly_file }|指定具有给定程序集哈希的代码。 若要使用哈希作为代码组成员资格条件，则必须指定哈希值或程序集文件。 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.HashMembershipCondition?displayProperty=nameWithType>。|  
|-pub { -cert cert_file_name &#124; <br /><br /> -file signed_file_name &#124; -hex  hex_string }|指定具有给定软件发行者的代码，软件发行者由证书文件、文件上的签名或 X509 证书的十六进制表示形式来指示。 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.PublisherMembershipCondition?displayProperty=nameWithType>。|  
|-site website|指定具有给定源站点的代码。 例如：<br /><br /> `-site** www.proseware.com`<br /><br /> 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.SiteMembershipCondition?displayProperty=nameWithType>。|  
|-strong -file file_name {name &#124; -noname} {version &#124; -noversion} |指定具有特定强名称的代码，强名称由文件名、字符串形式的程序集名称和 *major*.*minor*.*build*.*revision* 格式的程序集版本指示。 例如：<br /><br /> **-strong -file** myAssembly.exe myAssembly 1.2.3.4<br /><br /> 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.StrongNameMembershipCondition?displayProperty=nameWithType>。|  
|-url URL|指定源自给定 URL 的代码。 URL 必须包含一个协议，如 `http://` 或 `ftp://`。 此外，可以使用通配符 (\*) 指定来自特定 URL 的多个程序集。 **注意：** 由于 URL 可以用多个名称标识，使用 URL 作为成员条件不是确定代码标识的安全方式。 应尽可能使用强名称成员条件、发行者成员条件或哈希成员条件。 <br /><br /> 有关此成员资格条件的详细信息，请参阅 <xref:System.Security.Policy.UrlMembershipCondition?displayProperty=nameWithType>。|  
|-zone zonename|指定具有给定源区域的代码。 *Zonename* 参数可以是下列值之一：**MyComputer**、**Intranet**、**Trusted**、**Internet** 或 **Untrusted**。 有关此成员资格条件的更多信息，请参见 <xref:System.Security.Policy.ZoneMembershipCondition> 类。|  
  
 可与 **–addgroup** 和 **–chggroup** 选项一起使用的 *flags* 参数，可使用下列参数之一指定。  
  
|参数|描述|  
|--------------|-----------------|  
|-description “description”|与 **–addgroup** 选项一起使用时，指定要添加的代码组的描述。 与 **–chggroup** 选项一起使用时，指定要编辑的代码组的描述。 *description* 参数必须用双引号引起来。|  
|**-exclusive** {**on**&#124;**off**}|设置为 **on** 时，指示当某些代码符合代码组的成员资格条件时，只考虑与正在添加或修改的代码组关联的权限集。 当此选项设置为 **off** 时，Caspol.exe 考虑策略级别中所有匹配的代码组的权限集。|  
|**-levelfinal** {**on**&#124;**off**}|当设置为 **on** 时，指示不考虑低于已添加或修改的代码组所在的级别的策略级别。 此选项通常在计算机策略级别上使用。 例如，如果在计算机级别上为代码组设置此标志，并且某个代码与此代码组的成员资格条件匹配，则 Caspol.exe 不会计算或应用此代码的用户级别策略。|  
|-name “name”|与 **–addgroup** 选项一起使用时，指定要添加的代码组的脚本名称。 与 **-chggroup** 选项一起使用时，指定要编辑的代码组的脚本名称。 *name* 参数必须用双引号引起。 *name* 参数不能以数字开头，只能包含 A-Z、0-9 和下划线字符。 代码组可以按此 *name* 而非其数字标签引用。 *name* 对于撰写脚本也非常有用。|  
  
## <a name="remarks"></a>备注  

 安全策略使用三个策略级别来表示：计算机策略、用户策略和企业策略。 程序集收到的权限集由这三个策略级别允许的权限集的交集确定。 每个策略级别都用代码组的分层结构表示。 每个代码组都有一个确定哪个代码是该组成员的成员资格条件。 命名权限集也与每个代码组关联。 此权限集指定运行时允许满足成员资格条件的代码拥有的权限。 代码组层次结构连同其关联的命名权限集一起定义并维护每个安全策略级别。 可以使用 **–user**、 **-customuser**、 **–machine** 和 **-enterprise** 选项设置安全策略级别。  
  
 有关安全策略以及运行时如何确定授予代码何种权限的更多信息，请参见[安全策略管理](/previous-versions/dotnet/netframework-4.0/c1k0eed6(v=vs.100))。  
  
## <a name="referencing-code-groups-and-permission-sets"></a>引用代码组和权限集  

 为使在层次结构中引用代码组更容易， **-list** 选项显示了代码组的缩进式列表及其数字标签（1、1.1、1.1.1 依此类推）。 其他以代码组为目标的命令行操作也使用数字标签来引用特定的代码组。  
  
 命名权限集按其名称引用。 **–list** 选项显示代码组的列表，后面紧跟在该策略中可用的命名权限集的列表。  
  
## <a name="caspolexe-behavior"></a>Caspol.exe 行为  

 **-s**[**ecurity**] {**on** &#124; **off**} 之外的所有选项使用与 Caspol.exe 一起安装的 .NET Framework 的版本。 如果运行的 Caspol.exe 是与运行时的 *X* 版本一起安装的，则更改只应用于该版本。 运行时的其他并行安装（如果有）不受影响。 如果不是在特定运行时版本的目录中从命令行运行 Caspol.exe，则从路径中的第一个运行时版本目录（通常是安装的最新的运行时版本）中执行此工具。  
  
 **-s**[**ecurity**] {**on** &#124; **off**} 选项是整个计算机范围的操作。 关闭代码访问安全性将终止对计算机上的所有托管代码和所有用户的安全性检查。 如果同时安装了 .NET Framework 的多个版本，则此命令将关闭计算机上安装的每个版本的安全性。 尽管 **-list** 选项表明已经关闭安全性，但对于其他用户，没有任何其他指示清楚表明安全性已关闭。  
  
 当没有管理权限的用户运行 Caspol.exe 时，除非指定了 **–machine** 选项，否则所有选项都将引用用户级别策略。 当管理员运行 Caspol.exe 时，除非指定了 **–user** 选项，否则所有选项都将引用计算机策略。  
  
 Caspol.exe 必须被授予等效于 **Everything** 权限集的权限才能运行。 该工具有保护机制，可防止以阻碍授予 Caspol.exe 运行所需权限的方式修改策略。 如果尝试做这种更改，Caspol.exe 将通知你所请求的策略更改会中断该工具的运行，并且策略更改被拒绝。 可以通过使用 **–force** 选项为给定的命令关闭此保护机制。  
  
<a name="cpgrfcodeaccesssecuritypolicyutilitycaspolexeanchor1"></a>

## <a name="manually-editing-the-security-configuration-files"></a>手动编辑安全配置文件  

 三个安全配置文件与 Caspol.exe 支持的三个策略级别相对应：一个用于计算机策略，一个用于给定用户的策略，一个用于企业策略。 仅当使用 Caspol.exe 更改了计算机、用户或企业策略时，才会在磁盘上创建这些文件。 如果需要，可以使用 Caspol.exe 中的 **–reset** 选项将默认安全策略保存到磁盘。  
  
 多数情况下，不建议手动编辑安全配置文件。 但在某些情况下可能需要修改这些文件，如当管理员想编辑特定用户的安全配置文件时。  
  
## <a name="examples"></a>示例  

 **-addfulltrust**  
  
 假设已经将一个包含自定义权限的权限集添加到计算机策略。 该自定义权限在 `MyPerm.exe` 中实现，而 `MyPerm.exe` 引用 `MyOther.exe` 中的类。 这两个程序集都必须添加到完全信任程序集列表中。 下面的命令将 `MyPerm.exe` 程序集添加到计算机策略的完全信任列表中。  
  
```console  
caspol -machine -addfulltrust MyPerm.exe  
```  
  
 下面的命令将 `MyOther.exe` 程序集添加到计算机策略的完全信任列表中。  
  
```console  
caspol -machine -addfulltrust MyOther.exe  
```  
  
 **-addgroup**  
  
 下面的命令将子代码组添加到计算机策略代码组层次结构的根位置。 新的代码组是 **Internet** 区域的成员，并与 **Execution** 权限集关联。  
  
```console  
caspol -machine -addgroup 1.  -zone Internet Execution  
```  
  
 下面的命令将添加一个子代码组，该代码组授予共享 \\\netserver\netshare 本地 Intranet 权限。  
  
```console  
caspol -machine -addgroup 1. -url \\netserver\netshare\* LocalIntranet  
```  
  
 **-addpset**  
  
 下面的命令将 `Mypset` 权限集添加到用户策略。  
  
```console  
caspol -user -addpset Mypset.xml Mypset  
```  
  
 **-chggroup**  
  
 下面的命令将标记为 1.2. 的代码组的用户策略中的权限集 更改为 **Execution** 权限集。  
  
```console  
caspol -user -chggroup 1.2. Execution  
```  
  
 下面的命令更改标记为 1.2.1. 的代码组的默认策略中的成员资格条件， 并更改 **exclusive** 标志的设置。 该成员资格条件被定义为源自 **Internet** 区域的代码，并且 **exclusive** 标志已打开。  
  
```console  
caspol -chggroup 1.2.1. -zone Internet -exclusive on  
```  
  
 **-chgpset**  
  
 下面的命令将名为 `Mypset` 的权限集更改为包含在 `newpset.xml` 中的权限集。 请注意，当前版本不支持更改正在由代码组层次结构使用的权限集。  
  
```console  
caspol -chgpset Mypset newpset.xml  
```  
  
 **-force**  
  
 下面的命令使用户策略的根代码组（标记为 1）与 **Nothing** 命名权限集关联。 这将阻止 Caspol.exe 运行。  
  
```console  
caspol -force -user -chggroup 1 Nothing  
```  
  
 **-recover**  
  
 下面的命令恢复最近保存的计算机策略。  
  
```console  
caspol -machine -recover  
```  
  
 **-remgroup**  
  
 下面的命令移除标记为 1.1 的代码组。 如果该代码组有任何子代码组，则这些代码组也将被删除。  
  
```console  
caspol -remgroup 1.1.  
```  
  
 **-rempset**  
  
 下面的命令从用户策略中移除 **Execution** 权限集。  
  
```console  
caspol -user -rempset Execution  
```  
  
 下面的命令从用户策略级别中移除 `Mypset`。  
  
```console  
caspol -rempset MyPset  
```  
  
 **-resolvegroup**  
  
 下面的命令显示 `myassembly` 所属的计算机策略的所有代码组。  
  
```console  
caspol -machine -resolvegroup myassembly  
```  
  
 下面的命令显示 `myassembly` 所属的计算机策略、企业策略和指定的自定义用户策略的所有代码组。  
  
```console  
caspol -customall "c:\config_test\security.config" -resolvegroup myassembly  
```  
  
 **-resolveperm**  
  
 下面的命令基于计算机策略级别和用户策略级别计算 `testassembly` 的权限。  
  
```console  
caspol -all -resolveperm testassembly  
```  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
