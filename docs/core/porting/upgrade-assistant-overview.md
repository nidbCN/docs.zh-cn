---
title: .NET 升级助手概述
description: 简要介绍可帮助从 .NET Framework 进行迁移和将项目升级到 .NET 5 的 .NET 升级助手工具。
author: ardalis
ms.date: 02/25/2021
ms.openlocfilehash: bd1c904586d170d93b76ae058914adb334289f89
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102108157"
---
# <a name="overview-of-the-net-upgrade-assistant"></a><span data-ttu-id="62fca-103">.NET 升级助手概述</span><span class="sxs-lookup"><span data-stu-id="62fca-103">Overview of the .NET Upgrade Assistant</span></span>

<span data-ttu-id="62fca-104">你可能有些应用当前正在 .NET Framework 上运行，而你想将它们移植到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="62fca-104">You might have apps that currently run on the .NET Framework that you're interested in porting to .NET 5.</span></span> <span data-ttu-id="62fca-105">.NET 升级助手工具可帮助完成此过程。</span><span class="sxs-lookup"><span data-stu-id="62fca-105">The .NET Upgrade Assistant tool can assist with this process.</span></span> <span data-ttu-id="62fca-106">本文提供以下内容：</span><span class="sxs-lookup"><span data-stu-id="62fca-106">This article provides:</span></span>

* <span data-ttu-id="62fca-107">.NET 升级助手概述。</span><span class="sxs-lookup"><span data-stu-id="62fca-107">An overview of the .NET Upgrade Assistant.</span></span>
* <span data-ttu-id="62fca-108">如何安装 .NET 升级助手。</span><span class="sxs-lookup"><span data-stu-id="62fca-108">How to install the .NET Upgrade Assistant.</span></span>

## <a name="what-is-the-net-upgrade-assistant"></a><span data-ttu-id="62fca-109">什么是 .NET 升级助手</span><span class="sxs-lookup"><span data-stu-id="62fca-109">What is the .NET Upgrade Assistant</span></span>

<span data-ttu-id="62fca-110">.NET 升级助手是一种命令行工具，可在不同类型的 .NET Framework 应用上运行。</span><span class="sxs-lookup"><span data-stu-id="62fca-110">The .NET Upgrade Assistant is a command-line tool that can be run on different kinds of .NET Framework apps.</span></span> <span data-ttu-id="62fca-111">它旨在帮助将 .NET Framework 应用升级到 .NET 5。</span><span class="sxs-lookup"><span data-stu-id="62fca-111">It's designed to assist with upgrading .NET Framework apps to .NET 5.</span></span> <span data-ttu-id="62fca-112">大多数情况下，在运行该应用后，应用还需要执行多项操作来完成迁移。</span><span class="sxs-lookup"><span data-stu-id="62fca-112">After running the tool, **in most cases the app will require more effort to complete the migration**.</span></span> <span data-ttu-id="62fca-113">该工具包含安装可帮助完成迁移的分析器。</span><span class="sxs-lookup"><span data-stu-id="62fca-113">The tool includes the installation of analyzers that can assist with completing the migration.</span></span>

<span data-ttu-id="62fca-114">该工具目前支持下列 .NET Framework 应用类型：</span><span class="sxs-lookup"><span data-stu-id="62fca-114">Currently the tool supports the following .NET Framework app types:</span></span>

- <span data-ttu-id="62fca-115">.NET Framework Windows 窗体应用</span><span class="sxs-lookup"><span data-stu-id="62fca-115">.NET Framework Windows Forms apps</span></span>
- <span data-ttu-id="62fca-116">.NET Framework WPF 应用</span><span class="sxs-lookup"><span data-stu-id="62fca-116">.NET Framework WPF apps</span></span>
- <span data-ttu-id="62fca-117">.NET Framework ASP.NET MVC 应用</span><span class="sxs-lookup"><span data-stu-id="62fca-117">.NET Framework ASP.NET MVC apps</span></span>
- <span data-ttu-id="62fca-118">.NET Framework 控制台应用</span><span class="sxs-lookup"><span data-stu-id="62fca-118">.NET Framework console apps</span></span>
- <span data-ttu-id="62fca-119">.NET Framework 类库</span><span class="sxs-lookup"><span data-stu-id="62fca-119">.NET Framework class libraries</span></span>

<span data-ttu-id="62fca-120">.NET 升级助手目前为预发行版，且正在频繁接收更新。</span><span class="sxs-lookup"><span data-stu-id="62fca-120">The .NET Upgrade Assistant is currently prerelease and is receiving frequent updates.</span></span> <span data-ttu-id="62fca-121">如果在使用该工具时发现问题，请在工具的 [GitHub 存储库](https://github.com/dotnet/upgrade-assistant)中进行报告。</span><span class="sxs-lookup"><span data-stu-id="62fca-121">If you discover problems using the tool, please report them in the tool's [GitHub repository](https://github.com/dotnet/upgrade-assistant).</span></span>

## <a name="how-to-install-the-net-upgrade-assistant"></a><span data-ttu-id="62fca-122">如何安装 .NET 升级助手</span><span class="sxs-lookup"><span data-stu-id="62fca-122">How to install the .NET Upgrade Assistant</span></span>

<span data-ttu-id="62fca-123">[入门教程](https://aka.ms/dotnet-upgrade-assistant-install)介绍了如何安装和使用 .NET 升级助手。</span><span class="sxs-lookup"><span data-stu-id="62fca-123">The [Get Started tutorial](https://aka.ms/dotnet-upgrade-assistant-install) walks through how to install and use the .NET Upgrade Assistant.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="62fca-124">先决条件</span><span class="sxs-lookup"><span data-stu-id="62fca-124">Prerequisites</span></span>

1. <span data-ttu-id="62fca-125">此工具使用 MSBuild 来处理项目文件。</span><span class="sxs-lookup"><span data-stu-id="62fca-125">This tool uses MSBuild to work with project files.</span></span> <span data-ttu-id="62fca-126">请确保已安装最新版本的 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="62fca-126">Make sure that a recent version of MSBuild is installed.</span></span> <span data-ttu-id="62fca-127">要做到这一点，一个简单的方法是[安装 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="62fca-127">An easy way to do this is to [install Visual Studio 2019](https://visualstudio.microsoft.com/downloads/).</span></span>
1. <span data-ttu-id="62fca-128">此工具依赖于 [try-convert](https://github.com/dotnet/try-convert)。</span><span class="sxs-lookup"><span data-stu-id="62fca-128">This tool depends on [try-convert](https://github.com/dotnet/try-convert).</span></span> <span data-ttu-id="62fca-129">为使该工具正常运行，你必须安装 try-convert 来将项目文件转换为新的 SDK 样式。</span><span class="sxs-lookup"><span data-stu-id="62fca-129">In order for the tool to run correctly, you must install the try-convert tool for converting project files to the new SDK style.</span></span> <span data-ttu-id="62fca-130">如果已安装 try-convert，则转而可能需要更新它（原因是 upgrade-assistant 依赖于 0.7.212201 版或更高版本 ）</span><span class="sxs-lookup"><span data-stu-id="62fca-130">If you already have **try-convert** installed, you may need to update it instead (since **upgrade-assistant** depends on version _0.7.212201_ or later)</span></span>
    1. <span data-ttu-id="62fca-131">若要安装 try-convert：`dotnet tool install -g try-convert`</span><span class="sxs-lookup"><span data-stu-id="62fca-131">To install try-convert: `dotnet tool install -g try-convert`</span></span>
    1. <span data-ttu-id="62fca-132">若要更新 try-convert：`dotnet tool update -g try-convert`</span><span class="sxs-lookup"><span data-stu-id="62fca-132">To update try-convert: `dotnet tool update -g try-convert`</span></span>

### <a name="installation-steps"></a><span data-ttu-id="62fca-133">安装步骤</span><span class="sxs-lookup"><span data-stu-id="62fca-133">Installation steps</span></span>

<span data-ttu-id="62fca-134">可运行以下命令将该工具安装为 .NET CLI 工具：`dotnet tool install -g upgrade-assistant --add-source https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json`</span><span class="sxs-lookup"><span data-stu-id="62fca-134">The tool can be installed as a .NET CLI tool by running: `dotnet tool install -g upgrade-assistant --add-source https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json`</span></span>

<span data-ttu-id="62fca-135">同样地，由于 .NET 升级助手是作为 .NET CLI 工具安装的，可运行以下命令来轻松更新它：`https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json`</span><span class="sxs-lookup"><span data-stu-id="62fca-135">Similarly, because the .NET Upgrade Assistant is installed as a .NET CLI tool, it can be easily updated by running: `https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json`</span></span>

<span data-ttu-id="62fca-136">有关详细的安装说明，请查看项目的 [README](https://github.com/dotnet/upgrade-assistant)。</span><span class="sxs-lookup"><span data-stu-id="62fca-136">For detailed installation instructions, please refer to the project's [README](https://github.com/dotnet/upgrade-assistant).</span></span>

## <a name="see-also"></a><span data-ttu-id="62fca-137">另请参阅</span><span class="sxs-lookup"><span data-stu-id="62fca-137">See also</span></span>

- [<span data-ttu-id="62fca-138">使用 .NET 升级助手将 WPF 应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="62fca-138">Upgrade a WPF App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-wpf-framework.md)
- [<span data-ttu-id="62fca-139">使用 .NET 升级助手将 Windows 窗体应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="62fca-139">Upgrade a Windows Forms App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-winforms-framework.md)
- [<span data-ttu-id="62fca-140">使用 .NET 升级助手将 ASP.NET MVC 应用升级到 .NET 5</span><span class="sxs-lookup"><span data-stu-id="62fca-140">Upgrade an ASP.NET MVC App to .NET 5 with the .NET Upgrade Assistant</span></span>](upgrade-assistant-aspnetmvc.md)
- [<span data-ttu-id="62fca-141">.NET 升级助手 GitHub 存储库</span><span class="sxs-lookup"><span data-stu-id="62fca-141">.NET Upgrade Assistant GitHub Repository</span></span>](https://github.com/dotnet/upgrade-assistant)
