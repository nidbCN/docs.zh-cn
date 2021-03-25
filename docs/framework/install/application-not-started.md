---
title: 排查“无法启动此应用程序”
description: 了解在出现“无法启动此应用程序”对话框时如何应对。
ms.date: 09/05/2019
ms.openlocfilehash: 92055bf40500eba4f7c892d11af12d8e675ddd5d
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102605238"
---
# <a name="troubleshooting-a-this-application-could-not-be-started-error-message"></a>排查“无法启动此应用程序”错误消息

为 .NET Framework 开发的应用程序通常需要在系统上安装 .NET Framework 的特定版本。 在某些情况下，你可能会尝试在运行应用程序时，不使用已安装的 .NET Framework 版本或所需版本。 这通常会产生如下错误对话框：

![无法启动此应用程序](media/application-not-started/app-could-not-be-started.png)

此错误通常表示存在以下情况之一：

- 系统上的 .NET Framework 安装已损坏。

- 无法检测到应用程序所需的 .NET Framework 版本。

若要解决此问题以便可以运行应用程序，请执行以下操作：

1. 下载 [.NET Framework Repair Tool (NetFxRepairTool.exe)](https://www.microsoft.com/download/details.aspx?id=30135)。 该工具会在下载完成后自动运行。

1. 如果 .NET Framework Repair Tool 建议执行任何其他操作，例如下图所示的操作，请选择“下一步”。

   ![修复工具建议的更改](media/application-not-started/repair-tool-recommended-changes.png)

1. .NET Framework Repair Tool 会显示一个对话框，如下图所示，指出更改已完成。 当尝试重新运行应用程序时，请保持该对话框处于打开状态。 如果 .NET Framework Repair Tool 已识别并更正损坏的 .NET Framework 安装，则应该会成功运行。

   ![修复工具更改完成](media/application-not-started/repair-tool-changes-complete.png)

1. 如果应用程序成功运行，请选择“完成”按钮。 否则，请选择“下一步”按钮。

1. 如果选择了“下一步”按钮，则 .NET Framework 修复工具会显示如下所示的对话框。 选择“完成”按钮，将诊断信息发送给 Microsoft。

   ![无法解决问题](media/application-not-started/repair-tool-no-resolution.png)

1. 如果仍无法运行应用程序，请安装你的 Windows 版本支持的最新版 .NET Framework，如下表所示。

   |Windows 版本|.NET Framework 安装|
   |---|---|
   |Windows 10 周年更新及更高版本|[.NET Framework 4.8 Runtime](https://dotnet.microsoft.com/download/dotnet-framework/net48)|
   |Windows 10、Windows 10 11 月更新|[.NET Framework 4.6.2](https://dotnet.microsoft.com/download/dotnet-framework/net462)|
   |Windows 8.1|[.NET Framework 4.8 Runtime](https://dotnet.microsoft.com/download/dotnet-framework/net48)|
   |Windows 8|[.NET Framework 4.6.1](https://dotnet.microsoft.com/download/dotnet-framework/net461)|
   |Windows 7 SP1|[.NET Framework 4.8 Runtime](https://dotnet.microsoft.com/download/dotnet-framework/net48)|
   |Windows Vista SP2|[.NET Framework 4.6](https://dotnet.microsoft.com/download/dotnet-framework/net46)|

   > [!NOTE]
   > .NET Framework 4.8 预安装在 Windows 10 2019 年 5 月更新上。

1. 尝试启动应用程序。

1. 在某些情况下，可能会显示如下所示的对话框，要求你安装 .NET Framework 3.5。 选择“下载并安装此功能”以安装 .NET Framework 3.5，然后重新启动应用程序。

   ![建议安装 .NET Framework 3.5 的“Windows 功能”对话框](media/application-not-started/install-3-5.png)

## <a name="see-also"></a>另请参阅

- [.NET Framework 系统要求](../get-started/system-requirements.md)
- [.NET Framework 安装指南](index.md)
- [安装和卸载 .NET Framework 受阻疑难解答](troubleshoot-blocked-installations-and-uninstallations.md)
