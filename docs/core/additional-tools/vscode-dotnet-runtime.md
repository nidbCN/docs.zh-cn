---
title: 适用于 VS Code 扩展创建者的 .NET 安装工具
description: 对适用于扩展创建者的 .NET 安装工具的概述，该工具是用于安装 .NET 运行时的 Visual Studio Code 扩展。
author: sfoslund
ms.date: 11/18/2020
ms.openlocfilehash: a372d0cc728956920d013dac9bc0da1bcd3edc0b
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104872985"
---
# <a name="net-install-tool-for-extension-authors"></a><span data-ttu-id="e1090-103">适用于扩展创建者的 .NET 安装工具</span><span class="sxs-lookup"><span data-stu-id="e1090-103">.NET install tool for extension authors</span></span>

<span data-ttu-id="e1090-104">[适用于扩展创建者的 .NET 安装工具](https://github.com/dotnet/vscode-dotnet-runtime)是一种专门供 VS Code 扩展创建者获取 .NET 运行时的 Visual Studio Code 扩展。</span><span class="sxs-lookup"><span data-stu-id="e1090-104">The [.NET install tool for extension authors](https://github.com/dotnet/vscode-dotnet-runtime) is a Visual Studio Code extension that allows acquisition of the .NET runtime specifically for VS Code extension authors.</span></span> <span data-ttu-id="e1090-105">此工具专门供采用 .NET 编写并且需要 .NET 启动其各个部分的扩展（例如语言服务器）使用。</span><span class="sxs-lookup"><span data-stu-id="e1090-105">This tool is intended to be leveraged in extensions that are written in .NET and require .NET to boot pieces of the extension (for example, a language server).</span></span> <span data-ttu-id="e1090-106">此扩展并非直接供用户用来安装用于开发的 .NET。</span><span class="sxs-lookup"><span data-stu-id="e1090-106">The extension is not intended to be used directly by users to install .NET for development.</span></span>

## <a name="getting-started-extension-authors"></a><span data-ttu-id="e1090-107">入门指南：扩展创建者</span><span class="sxs-lookup"><span data-stu-id="e1090-107">Getting started: extension authors</span></span>

<span data-ttu-id="e1090-108">为确保适用于扩展创建者的 .NET 安装工具适合你的方案，请先从 GitHub 页查看[此扩展的目标](https://github.com/dotnet/vscode-dotnet-runtime#goals-acquiring-net-core-for-extensions)。</span><span class="sxs-lookup"><span data-stu-id="e1090-108">To ensure that the .NET install tool for extension authors is the right fit for your scenario, start by reviewing the [goals of this extension](https://github.com/dotnet/vscode-dotnet-runtime#goals-acquiring-net-core-for-extensions) on our GitHub page.</span></span>

> [!NOTE]
> <span data-ttu-id="e1090-109">此工具只可用于安装 .NET 运行时，当前无法安装 .NET SDK。</span><span class="sxs-lookup"><span data-stu-id="e1090-109">This tool can be used to install the .NET runtime only, it currently does not have the capability to install the .NET SDK.</span></span>

<span data-ttu-id="e1090-110">验证适用于扩展创建者的 .NET 安装工具符合你的需求后，即可在[扩展清单](https://code.visualstudio.com/api/references/extension-manifest)中利用对它的依赖关系，并开始通过 [VS Code API](https://code.visualstudio.com/api/extension-guides/command#programmatically-executing-a-command) 使用我们所公开的命令。</span><span class="sxs-lookup"><span data-stu-id="e1090-110">Once you have verified that the .NET install tool for extension authors fits your needs, you can take a dependency on it in your [extension manifest](https://code.visualstudio.com/api/references/extension-manifest) and begin using the commands we expose with the [VS Code API](https://code.visualstudio.com/api/extension-guides/command#programmatically-executing-a-command).</span></span> <span data-ttu-id="e1090-111">你可找到此扩展在 [GitHub](https://github.com/dotnet/vscode-dotnet-runtime/blob/main/Documentation/commands.md) 上公开的命令列表。</span><span class="sxs-lookup"><span data-stu-id="e1090-111">You can find the list of commands this extension exposes on our [GitHub](https://github.com/dotnet/vscode-dotnet-runtime/blob/main/Documentation/commands.md).</span></span>

<span data-ttu-id="e1090-112">请查看此[扩展示例](https://github.com/dotnet/vscode-dotnet-runtime/tree/main/sample)，了解相关操作步骤。</span><span class="sxs-lookup"><span data-stu-id="e1090-112">Check out this [sample extension](https://github.com/dotnet/vscode-dotnet-runtime/tree/main/sample) to see these steps in action.</span></span>

<span data-ttu-id="e1090-113">若要获取更多示例，请查看当前利用了此工具的以下开源扩展：</span><span class="sxs-lookup"><span data-stu-id="e1090-113">For more examples, check out these open source extensions that currently leverage this tool:</span></span>

- [<span data-ttu-id="e1090-114">适用于 Visual Studio Code 的 Azure 资源管理器 (ARM) 工具</span><span class="sxs-lookup"><span data-stu-id="e1090-114">Azure Resource Manager (ARM) Tools for Visual Studio Code</span></span>](https://github.com/microsoft/vscode-azurearmtools)

- [<span data-ttu-id="e1090-115">.NET 交互式笔记本</span><span class="sxs-lookup"><span data-stu-id="e1090-115">.NET Interactive Notebooks</span></span>](https://github.com/dotnet/interactive/tree/main/src/dotnet-interactive-vscode)

## <a name="getting-started-end-users"></a><span data-ttu-id="e1090-116">入门指南：最终用户</span><span class="sxs-lookup"><span data-stu-id="e1090-116">Getting started: end users</span></span>

<span data-ttu-id="e1090-117">最终用户通常完全不需要与适用于扩展创建者的 .NET 安装工具交互。</span><span class="sxs-lookup"><span data-stu-id="e1090-117">In general, the end user should not need to interact with the .NET install tool for extension authors at all.</span></span> <span data-ttu-id="e1090-118">如果你在使用此扩展时遇到问题，请查看[“故障排除”页](https://github.com/dotnet/vscode-dotnet-runtime/blob/main/Documentation/troubleshooting-runtime.md)或通过 [GitHub](https://github.com/dotnet/vscode-dotnet-runtime/issues) 提交问题。</span><span class="sxs-lookup"><span data-stu-id="e1090-118">If you are having problems with the extension, check out our [troubleshooting page](https://github.com/dotnet/vscode-dotnet-runtime/blob/main/Documentation/troubleshooting-runtime.md) or file an issue on our [GitHub](https://github.com/dotnet/vscode-dotnet-runtime/issues).</span></span>
