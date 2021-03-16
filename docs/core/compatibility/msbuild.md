---
title: MSBuild 中断性变更
description: 列出 .NET Core 2.1-3.1 中的 MSBuild 中断性变更。
ms.date: 02/22/2021
ms.openlocfilehash: 03fcd9807a83c4f679dc659b009c857e351b9b2d
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102105638"
---
# <a name="msbuild-breaking-changes-in-net-core-21---31"></a><span data-ttu-id="eed98-103">.NET Core 2.1-3.1 中的 MSBuild 中断性变更</span><span class="sxs-lookup"><span data-stu-id="eed98-103">MSBuild breaking changes in .NET Core 2.1 - 3.1</span></span>

<span data-ttu-id="eed98-104">本页记录了以下中断性变更：</span><span class="sxs-lookup"><span data-stu-id="eed98-104">The following breaking changes are documented on this page:</span></span>

| <span data-ttu-id="eed98-105">重大更改</span><span class="sxs-lookup"><span data-stu-id="eed98-105">Breaking change</span></span> | <span data-ttu-id="eed98-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="eed98-106">Version introduced</span></span> |
| - | - |
| [<span data-ttu-id="eed98-107">设计时生成仅返回最上层包引用</span><span class="sxs-lookup"><span data-stu-id="eed98-107">Design-time builds only return top-level package references</span></span>](#design-time-builds-only-return-top-level-package-references) | <span data-ttu-id="eed98-108">3.1</span><span class="sxs-lookup"><span data-stu-id="eed98-108">3.1</span></span> |
| [<span data-ttu-id="eed98-109">资源清单文件名更改</span><span class="sxs-lookup"><span data-stu-id="eed98-109">Resource manifest file name change</span></span>](#resource-manifest-file-name-change) | <span data-ttu-id="eed98-110">3.0</span><span class="sxs-lookup"><span data-stu-id="eed98-110">3.0</span></span> |
| [<span data-ttu-id="eed98-111">SDK 现已包含项目工具</span><span class="sxs-lookup"><span data-stu-id="eed98-111">Project tools now included in SDK</span></span>](#project-tools-now-included-in-sdk) | <span data-ttu-id="eed98-112">2.1</span><span class="sxs-lookup"><span data-stu-id="eed98-112">2.1</span></span> |

## <a name="net-core-31"></a><span data-ttu-id="eed98-113">.NET Core 3.1</span><span class="sxs-lookup"><span data-stu-id="eed98-113">.NET Core 3.1</span></span>

[!INCLUDE [design-time-builds-return-top-level-package-refs](../../../includes/core-changes/msbuild/3.1/design-time-builds-return-top-level-package-refs.md)]

***

## <a name="net-core-30"></a><span data-ttu-id="eed98-114">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="eed98-114">.NET Core 3.0</span></span>

[!INCLUDE[Resource file names](../../../includes/core-changes/msbuild/3.0/resource-manifest-name.md)]

***

## <a name="net-core-21"></a><span data-ttu-id="eed98-115">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="eed98-115">.NET Core 2.1</span></span>

[!INCLUDE [DotNetCliToolReference project elements removed for bundled tools](../../../includes/core-changes/msbuild/2.1/dotnetclitoolreference.md)]

***
