---
ms.openlocfilehash: 49aa98edec8ed1ce35dd501c57aa82e4ca4cf286
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104879652"
---
### <a name="project-tools-now-included-in-sdk"></a><span data-ttu-id="657d2-101">SDK 现已包含项目工具</span><span class="sxs-lookup"><span data-stu-id="657d2-101">Project tools now included in SDK</span></span>

<span data-ttu-id="657d2-102">.NET Core 2.1 SDK 现包括常见 CLI 工具，你无需再从项目中引用这些工具。</span><span class="sxs-lookup"><span data-stu-id="657d2-102">The .NET Core 2.1 SDK now includes common CLI tooling, and you no longer need to reference these tools from the project.</span></span>

#### <a name="change-description"></a><span data-ttu-id="657d2-103">更改描述</span><span class="sxs-lookup"><span data-stu-id="657d2-103">Change description</span></span>

<span data-ttu-id="657d2-104">在 .NET Core 2.0 中，项目通过 `<DotNetCliToolReference>` 项目设置引用外部 .NET 工具。</span><span class="sxs-lookup"><span data-stu-id="657d2-104">In .NET Core 2.0, projects reference external .NET tools with the `<DotNetCliToolReference>` project setting.</span></span> <span data-ttu-id="657d2-105">在 .NET Core 2.1 中，其中的某些工具包含在 .NET Core SDK 中，不再需要设置。</span><span class="sxs-lookup"><span data-stu-id="657d2-105">In .NET Core 2.1, some of these tools are included with the .NET Core SDK, and the setting is no longer needed.</span></span> <span data-ttu-id="657d2-106">如果在项目中包含对这些工具的引用，则会收到如下所示的错误：“Microsoft.EntityFrameworkCore.Tools.DotNet”工具现包含在 .NET Core SDK 中。</span><span class="sxs-lookup"><span data-stu-id="657d2-106">If you include references to these tools in your project, you'll receive an error similar to the following: **The tool 'Microsoft.EntityFrameworkCore.Tools.DotNet' is now included in the .NET Core SDK.**</span></span>

<span data-ttu-id="657d2-107">.NET Core 2.1 SDK 中现在包含的工具：</span><span class="sxs-lookup"><span data-stu-id="657d2-107">Tools now included in .NET Core 2.1 SDK:</span></span>

| <span data-ttu-id="657d2-108">\<DotNetCliToolReference> 值</span><span class="sxs-lookup"><span data-stu-id="657d2-108">\<DotNetCliToolReference> value</span></span>                   | <span data-ttu-id="657d2-109">工具</span><span class="sxs-lookup"><span data-stu-id="657d2-109">Tool</span></span>                                                                                                            |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `Microsoft.DotNet.Watcher.Tools`               | `dotnet-watch`               |
| `Microsoft.Extensions.SecretManager.Tools`     | [<span data-ttu-id="657d2-110">dotnet-user-secrets</span><span class="sxs-lookup"><span data-stu-id="657d2-110">dotnet-user-secrets</span></span>](https://github.com/dotnet/aspnetcore/blob/main/src/Tools/dotnet-user-secrets/README.md) |
| `Microsoft.Extensions.Caching.SqlConfig.Tools` | [<span data-ttu-id="657d2-111">dotnet-sql-cache</span><span class="sxs-lookup"><span data-stu-id="657d2-111">dotnet-sql-cache</span></span>](https://github.com/dotnet/aspnetcore/blob/main/src/Tools/dotnet-sql-cache/README.md)       |
| `Microsoft.EntityFrameworkCore.Tools.DotNet`   | [<span data-ttu-id="657d2-112">dotnet-ef</span><span class="sxs-lookup"><span data-stu-id="657d2-112">dotnet-ef</span></span>](/ef/core/miscellaneous/cli/dotnet)                                                                  |

#### <a name="version-introduced"></a><span data-ttu-id="657d2-113">引入的版本</span><span class="sxs-lookup"><span data-stu-id="657d2-113">Version introduced</span></span>

<span data-ttu-id="657d2-114">.NET Core SDK 2.1.300</span><span class="sxs-lookup"><span data-stu-id="657d2-114">.NET Core SDK 2.1.300</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="657d2-115">建议的操作</span><span class="sxs-lookup"><span data-stu-id="657d2-115">Recommended action</span></span>

<span data-ttu-id="657d2-116">从项目中删除 `<DotNetCliToolReference>` 设置。</span><span class="sxs-lookup"><span data-stu-id="657d2-116">Remove the `<DotNetCliToolReference>` setting from your project.</span></span>

#### <a name="category"></a><span data-ttu-id="657d2-117">类别</span><span class="sxs-lookup"><span data-stu-id="657d2-117">Category</span></span>

<span data-ttu-id="657d2-118">MSBuild</span><span class="sxs-lookup"><span data-stu-id="657d2-118">MSBuild</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="657d2-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="657d2-119">Affected APIs</span></span>

<span data-ttu-id="657d2-120">不可用</span><span class="sxs-lookup"><span data-stu-id="657d2-120">N/A</span></span>
