---
title: NETSDK1059：项目包含已过时的 .NET CLI 工具
description: 如何解决项目包含已过时的 .NET CLI 工具的问题。
author: sfoslund
ms.topic: error-reference
ms.date: 10/09/2020
f1_keywords:
- NETSDK1059
ms.openlocfilehash: cad546ed1636b71be6b77aa00ef2f3a20095daa5
ms.sourcegitcommit: 1dbe25ff484a02025d5c34146e517c236f7161fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/19/2021
ms.locfileid: "104653031"
---
# <a name="netsdk1059-project-contains-obsolete-net-cli-tool"></a>NETSDK1059：项目包含已过时的 .NET CLI 工具

本文适用于：✔️ .NET Core 2.1.100 SDK 及更高版本

当 .NET SDK 发出警告 NETSDK1059 时，表示项目包含已过时的 .NET CLI 工具。 自 .NET Core 2.1 起，这些工具包含在 .NET SDK 中，无需由项目显式引用。 有关详细信息，请参阅 [SDK 中现已包含的项目工具](../../compatibility/2.1.md#project-tools-now-included-in-sdk)。
