---
ms.openlocfilehash: 370c6eb23a50ed388f0b10a5c5f4779ba2a1b144
ms.sourcegitcommit: 46cfed35d79d70e08c313b9c664c7e76babab39e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/10/2021
ms.locfileid: "102623588"
---
### <a name="project-tools-now-included-in-sdk"></a><span data-ttu-id="ed9ab-101">SDK 现已包含项目工具</span><span class="sxs-lookup"><span data-stu-id="ed9ab-101">Project tools now included in SDK</span></span>

<span data-ttu-id="ed9ab-102">.NET Core 2.1 SDK 现包括常见 CLI 工具，你无需再从项目中引用这些工具。</span><span class="sxs-lookup"><span data-stu-id="ed9ab-102">The .NET Core 2.1 SDK now includes common CLI tooling, and you no longer need to reference these tools from the project.</span></span>

#### <a name="change-description"></a><span data-ttu-id="ed9ab-103">更改描述</span><span class="sxs-lookup"><span data-stu-id="ed9ab-103">Change description</span></span>

<span data-ttu-id="ed9ab-104">在 .NET Core 2.0 中，项目通过 `<DotNetCliToolReference>` 项目设置引用外部 .NET 工具。</span><span class="sxs-lookup"><span data-stu-id="ed9ab-104">In .NET Core 2.0, projects reference external .NET tools with the `<DotNetCliToolReference>` project setting.</span></span> <span data-ttu-id="ed9ab-105">在 .NET Core 2.1 中，其中的某些工具包含在 .NET Core SDK 中，不再需要设置。</span><span class="sxs-lookup"><span data-stu-id="ed9ab-105">In .NET Core 2.1, some of these tools are included with the .NET Core SDK, and the setting is no longer needed.</span></span> <span data-ttu-id="ed9ab-106">如果在项目中包含对这些工具的引用，则会收到如下所示的错误：“Microsoft.EntityFrameworkCore.Tools.DotNet”工具现包含在 .NET Core SDK 中。</span><span class="sxs-lookup"><span data-stu-id="ed9ab-106">If you include references to these tools in your project, you'll receive an error similar to the following: **The tool 'Microsoft.EntityFrameworkCore.Tools.DotNet' is now included in the .NET Core SDK.**</span></span>

<span data-ttu-id="ed9ab-107">.NET Core 2.1 SDK 中现在包含的工具：</span><span class="sxs-lookup"><span data-stu-id="ed9ab-107">Tools now included in .NET Core 2.1 SDK:</span></span>

| <span data-ttu-id="ed9ab-108">\<DotNetCliToolReference> 值</span><span class="sxs-lookup"><span data-stu-id="ed9ab-108">\<DotNetCliToolReference> value</span></span>                   | <span data-ttu-id="ed9ab-109">工具</span><span class="sxs-lookup"><span data-stu-id="ed9ab-109">Tool</span></span>                                                                                                            |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `Microsoft.DotNet.Watcher.Tools`               | `dotnet-watch`               |
| `Microsoft.Extensions.SecretManager.Tools`     | [<span data-ttu-id="ed9ab-110">dotnet-user-secrets</span><span class="sxs-lookup"><span data-stu-id="ed9ab-110">dotnet-user-secrets</span></span>](https://github.com/dotnet/aspnetcore/blob/master/src/Tools/dotnet-user-secrets/README.md) |
| `Microsoft.Extensions.Caching.SqlConfig.Tools` | [<span data-ttu-id="ed9ab-111">dotnet-sql-cache</span><span class="sxs-lookup"><span data-stu-id="ed9ab-111">dotnet-sql-cache</span></span>](https://github.com/dotnet/aspnetcore/blob/master/src/Tools/dotnet-sql-cache/README.md)       |
| `Microsoft.EntityFrameworkCore.Tools.DotNet`   | [<span data-ttu-id="ed9ab-112">dotnet-ef</span><span class="sxs-lookup"><span data-stu-id="ed9ab-112">dotnet-ef</span></span>](/ef/core/miscellaneous/cli/dotnet)                                                                  |

#### <a name="version-introduced"></a><span data-ttu-id="ed9ab-113">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ed9ab-113">Version introduced</span></span>

<span data-ttu-id="ed9ab-114">.NET Core SDK 2.1.300</span><span class="sxs-lookup"><span data-stu-id="ed9ab-114">.NET Core SDK 2.1.300</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="ed9ab-115">建议的操作</span><span class="sxs-lookup"><span data-stu-id="ed9ab-115">Recommended action</span></span>

<span data-ttu-id="ed9ab-116">从项目中删除 `<DotNetCliToolReference>` 设置。</span><span class="sxs-lookup"><span data-stu-id="ed9ab-116">Remove the `<DotNetCliToolReference>` setting from your project.</span></span>

#### <a name="category"></a><span data-ttu-id="ed9ab-117">类别</span><span class="sxs-lookup"><span data-stu-id="ed9ab-117">Category</span></span>

<span data-ttu-id="ed9ab-118">MSBuild</span><span class="sxs-lookup"><span data-stu-id="ed9ab-118">MSBuild</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="ed9ab-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ed9ab-119">Affected APIs</span></span>

<span data-ttu-id="ed9ab-120">不可用</span><span class="sxs-lookup"><span data-stu-id="ed9ab-120">N/A</span></span>
