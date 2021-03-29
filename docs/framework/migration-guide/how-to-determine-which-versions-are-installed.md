---
title: 确定已安装的 .NET Framework 版本
description: 使用代码、regedit.exe 或 PowerShell，通过查询 Windows 注册表来检测在计算机上安装了哪些版本的 .NET Framework。
ms.date: 12/04/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- versions, determining for .NET Framework
- .NET Framework, determining installed versions
ms.assetid: 40a67826-e4df-4f59-a651-d9eb0fdc755d
ms.openlocfilehash: 027c29969b7dd0a2ed0dec62e612754d2d217eed
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653382"
---
# <a name="how-to-determine-which-net-framework-versions-are-installed"></a>如何：确定已安装的 .NET Framework 版本

用户可在他们的计算机上[安装](../install/index.md)和运行 .NET Framework 的多个版本。 当你开发或部署应用时，你可能需要知道用户的计算机上安装了哪些 .NET Framework 版本。 注册表包含计算机上安装的 .NET Framework 版本列表。

> [!NOTE]
> 本文特定于 .NET Framework。 若要确定安装了哪些 .NET Core、.NET 5+ SDK 和运行时，请参阅[如何检查是否已安装 .NET](../../core/install/how-to-detect-installed-versions.md)。

.NET Framework 由两个采用不同版本的主要组件构成：

- 一组程序集，它们是为应用提供功能的类型与资源的集合。 .NET Framework 和程序集使用相同的版本号。 例如，.NET Framework 版本包括 4.5、4.6.1 和 4.7.2。

- 公共语言运行时 (CLR)，可管理并执行应用代码。 单个 CLR 版本通常可支持多个 .NET Framework 版本。 例如，CLR 版本4.0.30319.xxxxx（其中 xxxxx 小于42000）支持 .NET Framework 版本 4 到 4.5.2。  大于或等于4.0.30319.42000 的 CLR 版本支持从 .NET Framework 4.6 开始的 .NET Framework 版本。

可使用社区维护的工具帮助检测安装了哪些 .NET Framework 版本：

- [https://github.com/jmalarcon/DotNetVersions](https://github.com/jmalarcon/DotNetVersions)

  .NET Framework 2.0 命令行工具。

- [https://github.com/EliteLoser/DotNetVersionLister](https://github.com/EliteLoser/DotNetVersionLister)

  PowerShell 2.0 模块。

要了解如何检测已安装的每个 .NET Framework 版本的更新，请参阅[操作说明：确定已安装的 .NET Framework 更新](how-to-determine-which-net-framework-updates-are-installed.md)。

## <a name="determine-which-net-implementation-and-version-an-app-is-running-on"></a>确定应用运行于的 .NET 实现和版本

可以使用 <xref:System.Runtime.InteropServices.RuntimeInformation.FrameworkDescription?displayProperty=nameWithType> 属性来查询应用运行于的 .NET 实现和版本。 如果应用在 .NET Framework 上运行，则输出将类似于：

```output
.NET Framework 4.8.4250.0
```

相比之下，如果应用在 .NET Core 或 .NET 5+ 上运行，则输出将类似于：

```output
.NET Core 3.1.9
.NET 5.0.0
```

## <a name="detect-net-framework-45-and-later-versions"></a>检测 .NET Framework 4.5 及更高版本

计算机上安装的 .NET Framework 版本（4.5 及更高版本）列出在注册表中，位于 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full。 如果缺少 Full 子项，则未安装 .NET Framework 4.5 或更高版本。

> [!NOTE]
> 注册表路径中的 .NET Framework Setup 子项不以句点开头。

注册表中的 **Release** REG_DWORD 值代表已安装的 .NET Framework 版本。

<a name="version_table"></a>

| .NET Framework 版本 | Release 的值 |
| ---------------------- | -------------------------- |
| .NET Framework 4.5     | 所有 Windows 操作系统：378389 |
| .NET Framework 4.5.1   | 在 Windows 8.1 和 Windows Server 2012 R2 上：378675<br />在所有其他 Windows 操作系统上：378758 |
| .NET Framework 4.5.2   | 所有 Windows 操作系统：379893 |
| .NET Framework 4.6     | 在 Windows 10 上：393295<br />在所有其他 Windows 操作系统上：393297 |
| .NET Framework 4.6.1   | 在 Windows 10 11 月更新系统上：394254<br />在所有其他 Windows 操作系统（包括 Windows 10）上：394271 |
| .NET Framework 4.6.2   | 在 Windows 10 周年更新和 Windows Server 2016 上：394802<br />在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：394806 |
| .NET Framework 4.7     | 在 Windows 10 创意者更新上：460798<br />在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：460805 |
| .NET Framework 4.7.1   | 在 Windows 10 Fall Creators Update 和 Windows Server 版本 1709 上：461308<br/>在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：461310 |
| .NET Framework 4.7.2   | 在 Windows 10 2018 年 4 月更新和 Windows Server 版本 1803 上：461808<br/>在除 Windows 10 2018 年 4 月更新和 Windows Server 版本 1803 之外的所有 Windows 操作系统上：461814 |
| .NET Framework 4.8     | 在 Windows 10 2019 年 5 月更新和 Windows 10 2019 年 11 月更新上：528040<br/>在 Windows 10 2020 年 5 月更新和 Windows 10 2020 年 10 月更新中：528372<br/>在所有其他 Windows 操作系统（包括其他 Windows 10 操作系统）上：528049 |

### <a name="minimum-version"></a>最低版本

若要确定是否存在 .NET Framework 的最低版本，请查看大于或等于下列表中列出的相应值的“Release”REG_DWORD 值。 例如，如果应用程序在 .NET Framework 4.8 或更高版本下运行，请测试 Release REG_DWORD 值是否大于或等于 528040。

| .NET Framework 版本 | 最小值 |
| ---------------------- | ------------- |
| .NET Framework 4.5     | 378389 |
| .NET Framework 4.5.1   | 378675 |
| .NET Framework 4.5.2   | 379893 |
| .NET Framework 4.6     | 393295 |
| .NET Framework 4.6.1   | 394254 |
| .NET Framework 4.6.2   | 394802 |
| .NET Framework 4.7     | 460798 |
| .NET Framework 4.7.1   | 461308 |
| .NET Framework 4.7.2   | 461808 |
| .NET Framework 4.8     | 528040 |

### <a name="use-registry-editor"></a>使用注册表编辑器

1. 在“开始”菜单中，选择“运行”，输入“regedit”，然后选择“确定” 。

   （必须具有管理凭据才能运行 regedit。）

1. 在注册表编辑器中，打开以下子项：HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full。 如果“Full”子项不存在，则表示尚未安装 .NET Framework 4.5 或更高版本。

1. 请检查名为“Release”的 REG_DWORD 条目。 如果存在，则已安装 .NET Framework 4.5 或更高版本。 其值对应于 .NET Framework 的特定版本。 以下图为例，“Release”条目的值为 528040，这是 .NET Framework 4.8 的版本密钥。

   ![.NET Framework 4.5 的注册表项](./media/clr-installdir.png )

### <a name="use-powershell-to-check-for-a-minimum-version"></a>使用 PowerShell 检查最低版本

使用 PowerShell 命令检查 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full 子项“Release”条目的值\\。

以下示例检查“Release”条目的值，以确定是否已安装 .NET Framework 4.6.2 或更高版本。 如果安装了此代码，则返回 `True`，否则返回 `False`。

```powershell
(Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full").Release -ge 394802
```

### <a name="query-the-registry-using-code"></a>使用代码查询注册表

01. 使用 <xref:Microsoft.Win32.RegistryKey.OpenBaseKey%2A?displayProperty=nameWithType> 和 <xref:Microsoft.Win32.RegistryKey.OpenSubKey%2A?displayProperty=nameWithType> 方法访问 Windows 注册表中的 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full 子项。

    > [!IMPORTANT]
    > 如果运行的应用是 32 位且在 64 位 Windows 中运行，则注册表路径与前面列出的不同。 可在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\ 子项中找到 64 位注册表。 例如，.NET Framework 4.5 的注册表子项为 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full。

01. 检查 Release REG_DWORD 值以确定已安装的版本。 为了向前兼容，可检查是否有一个值大于或等于 [.NET Framework 版本表](#version_table)中所列的值。

以下示例检查注册表中“Release”条目的值，以查找已安装的 .NET Framework 4.5-4.8：

:::code language="csharp" source="snippets/csharp/versions-installed.cs" id="2":::

:::code language="vb" source="snippets/visual-basic/versions-installed.vb" id="2":::

此示例显示类似以下内容的输出：

```output
.NET Framework Version: 4.6.1
```

此示例遵循版本检查的建议做法：

- 检查“Release”条目的值是否大于或等于已知版本密钥的值。
- 按从最新版本到最早版本的顺序检查。

## <a name="detect-net-framework-10-through-40"></a>检测 .NET Framework 1.0 到 4.0

.NET Framework 1.1 到 4.0 的每个版本都作为子项列出在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP。 下表列出了每个 .NET Framework 版本的路径。 对于大多数版本，都有 Install REG_DWORD 值 `1`，指示已安装此版本。 在这些子项中，还有一个包含版本字符串的 Version REG_SZ 值。

> [!NOTE]
> 注册表路径中的 .NET Framework Setup 子项不以句点开头。

| Framework 版本  | 注册表子项 | “值” |
| ------------------ | --------------- | ----- |
| 1.0                | **HKLM\\Software\\Microsoft\\.NETFramework\\Policy\\v1.0\\3705**     | **Install** REG_SZ 等于 `1` |
| 1.1                | **HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v1.1.4322**   | **Install** REG_DWORD 等于 `1` |
| 2.0                | **HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v2.0.50727**  | **Install** REG_DWORD 等于 `1` |
| 3.0                | **HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v3.0\\Setup** | **InstallSuccess** REG_DWORD 等于 `1` |
| 3.5                | **HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v3.5**        | **Install** REG_DWORD 等于 `1` |
| 4.0 客户端配置文件 | **HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v4\\Client**  | **Install** REG_DWORD 等于 `1` |
| 4.0 完整配置文件   | **HKLM\\Software\\Microsoft\\NET Framework Setup\\NDP\\v4\\Full**    | **Install** REG_DWORD 等于 `1` |

> [!IMPORTANT]
> 如果运行的应用是 32 位且在 64 位 Windows 中运行，则注册表路径与前面列出的不同。 可在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\ 子项中找到 64 位注册表。 例如，.NET Framework 3.5 的注册表子项为 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v3.5。

请注意，.NET Framework 1.0 子项的注册表路径与其他子项不同。

### <a name="use-registry-editor-older-framework-versions"></a>使用注册表编辑器（较旧的框架版本）

01. 在“开始”菜单中，选择“运行”，输入“regedit”，然后选择“确定” 。

    必须具有管理凭据才能运行 regedit。

01. 打开与要检查的版本匹配的子项。 使用[检测 .NET Framework 1.0 到 4.0](#detect-net-framework-10-through-40)部分列出的表。

    下图显示了 .NET Framework 3.5 的子项及其 Version 值。

    ![.NET Framework 3.5 的注册表项。](./media/net-4-and-earlier.png ".NET Framework 3.5 及早期版本")

### <a name="query-the-registry-using-code-older-framework-versions"></a>使用代码查询注册表（较旧的框架版本）

使用 <xref:Microsoft.Win32.RegistryKey?displayProperty=nameWithType> 类访问 Windows 注册表中的 HKEY_LOCAL_MACHINE\\SOFTWARE\\Microsoft\\NET Framework Setup\\NDP 子项。

> [!IMPORTANT]
> 如果运行的应用是 32 位且在 64 位 Windows 中运行，则注册表路径与前面列出的不同。 可在 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\ 子项中找到 64 位注册表。 例如，.NET Framework 3.5 的注册表子项为 HKEY_LOCAL_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\NET Framework Setup\\NDP\\v3.5。

以下示例查找已安装的 .NET Framework 1-4：

:::code language="csharp" source="snippets/csharp/versions-installed.cs" id="1":::

:::code language="vb" source="snippets/visual-basic/versions-installed.vb" id="1":::

该示例显示类似于以下内容的输出：

```output
v2.0.50727  2.0.50727.4927  SP2
v3.0  3.0.30729.4926  SP2
v3.5  3.5.30729.4926  SP1
v4.0
  Client  4.0.0.0
```

## <a name="find-clr-versions"></a>查找 CLR 版本

随 .NET Framework 一起安装 .NET Framework CLR 单独进行版本控制。 可通过两种方式检测 .NET Framework CLR 的版本：

- **Clrver 工具**

  使用 [CLR 版本工具 (Clrver.exe)](../tools/clrver-exe-clr-version-tool.md) 确定计算机上安装了哪些版本的 CLR。 打开 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)，输入 `clrver`。

  示例输出：

  ```console
  Versions installed on the machine:
  v2.0.50727
  v4.0.30319
  ```

- **`Environment` 类**

  > [!IMPORTANT]
  > 对于 .NET Framework 4.5 及更高版本，请勿使用 <xref:System.Environment.Version%2A?displayProperty=nameWithType> 属性来检测 CLR 的版本。 而应按照[检测 .NET Framework 4.5 及更高版本](#detect-net-framework-45-and-later-versions)中所述查询注册表。

  1. 查询 <xref:System.Environment.Version?displayProperty=nameWithType> 属性以检索 <xref:System.Version> 对象。

     返回的 `System.Version` 对象标识当前正在执行代码的运行时版本。 它不返回可能已安装在计算机上的程序集版本或其他运行时版本。

     对于 .NET Framework 版本 4、4.5、4.5.1 和 4.5.2，返回的 <xref:System.Version>  对象的字符串表示形式为 4.0.30319.xxxxx，其中 xxxxx 小于 42000。 对于 .NET Framework 4.6 及更高版本，它的格式为 4.0.30319.42000。

  1. 获得 Version 对象后，按如下方式查询：

     - 对于主要版本标识符（例如，4 表示版本 4.0），请使用 <xref:System.Version.Major%2A?displayProperty=nameWithType> 属性。

     - 对于次要版本标识符（例如，0 表示版本 4.0），请使用 <xref:System.Version.Minor%2A?displayProperty=nameWithType> 属性

     - 对于整个版本字符串（例如，4.0.30319.18010），请使用 <xref:System.Version.ToString%2A?displayProperty=nameWithType> 方法。 此方法返回一个值，该值反映正在执行代码的运行时的版本。 它不返回可能安装在计算机上的程序集版本或其他运行时版本。

  以下示例使用 <xref:System.Environment.Version%2A?displayProperty=nameWithType> 属性检索 CLR 版本信息：

  ```csharp
  Console.WriteLine($"Version: {Environment.Version}");
  ```

  ```vb
  Console.WriteLine($"Version: {Environment.Version}")
  ```

  该示例显示类似于以下内容的输出：

  ```output
  Version: 4.0.30319.18010
  ```

## <a name="see-also"></a>请参阅

- [如何：确定安装了哪些 .NET Framework 更新](how-to-determine-which-net-framework-updates-are-installed.md)
- [安装面向开发人员的 .NET Framework](../install/guide-for-developers.md)
- [.NET Framework 版本和依赖关系](versions-and-dependencies.md)
