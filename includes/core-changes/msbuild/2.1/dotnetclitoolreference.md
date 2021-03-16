---
ms.openlocfilehash: d9489df1ba096109e60d663e3a3f4442d30b2bb1
ms.sourcegitcommit: 42d436ebc2a7ee02fc1848c7742bc7d80e13fc2f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "102105457"
---
### <a name="project-tools-now-included-in-sdk"></a><span data-ttu-id="ea59e-101">SDK 现已包含项目工具</span><span class="sxs-lookup"><span data-stu-id="ea59e-101">Project tools now included in SDK</span></span>

<span data-ttu-id="ea59e-102">.NET Core 2.1 SDK 现包括常见 CLI 工具，你无需再从项目中引用这些工具。</span><span class="sxs-lookup"><span data-stu-id="ea59e-102">The .NET Core 2.1 SDK now includes common CLI tooling, and you no longer need to reference these tools from the project.</span></span>

#### <a name="change-description"></a><span data-ttu-id="ea59e-103">更改描述</span><span class="sxs-lookup"><span data-stu-id="ea59e-103">Change description</span></span>

<span data-ttu-id="ea59e-104">在 .NET Core 2.0 中，项目通过 `<DotNetCliToolReference>` 项目设置引用外部 .NET 工具。</span><span class="sxs-lookup"><span data-stu-id="ea59e-104">In .NET Core 2.0, projects reference external .NET tools with the `<DotNetCliToolReference>` project setting.</span></span> <span data-ttu-id="ea59e-105">在 .NET Core 2.1 中，其中的某些工具包含在 .NET Core SDK 中，不再需要设置。</span><span class="sxs-lookup"><span data-stu-id="ea59e-105">In .NET Core 2.1, some of these tools are included with the .NET Core SDK, and the setting is no longer needed.</span></span> <span data-ttu-id="ea59e-106">如果在项目中包含对这些工具的引用，则会收到如下所示的错误：“Microsoft.EntityFrameworkCore.Tools.DotNet”工具现包含在 .NET Core SDK 中。</span><span class="sxs-lookup"><span data-stu-id="ea59e-106">If you include references to these tools in your project, you'll receive an error similar to the following: **The tool 'Microsoft.EntityFrameworkCore.Tools.DotNet' is now included in the .NET Core SDK.**</span></span>

<span data-ttu-id="ea59e-107">.NET Core 2.1 SDK 中现在包含的工具：</span><span class="sxs-lookup"><span data-stu-id="ea59e-107">Tools now included in .NET Core 2.1 SDK:</span></span>

| <span data-ttu-id="ea59e-108">\<DotNetCliToolReference> 值</span><span class="sxs-lookup"><span data-stu-id="ea59e-108">\<DotNetCliToolReference> value</span></span>                   | <span data-ttu-id="ea59e-109">工具</span><span class="sxs-lookup"><span data-stu-id="ea59e-109">Tool</span></span>                                                                                                            |
|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------|
| `Microsoft.DotNet.Watcher.Tools`               | [<span data-ttu-id="ea59e-110">dotnet-watch</span><span class="sxs-lookup"><span data-stu-id="ea59e-110">dotnet-watch</span></span>](https://github.com/dotnet/aspnetcore/blob/master/src/Tools/dotnet-watch/README.md)               |
| `Microsoft.Extensions.SecretManager.Tools`     | [<span data-ttu-id="ea59e-111">dotnet-user-secrets</span><span class="sxs-lookup"><span data-stu-id="ea59e-111">dotnet-user-secrets</span></span>](https://github.com/dotnet/aspnetcore/blob/master/src/Tools/dotnet-user-secrets/README.md) |
| `Microsoft.Extensions.Caching.SqlConfig.Tools` | [<span data-ttu-id="ea59e-112">dotnet-sql-cache</span><span class="sxs-lookup"><span data-stu-id="ea59e-112">dotnet-sql-cache</span></span>](https://github.com/dotnet/aspnetcore/blob/master/src/Tools/dotnet-sql-cache/README.md)       |
| `Microsoft.EntityFrameworkCore.Tools.DotNet`   | [<span data-ttu-id="ea59e-113">dotnet-ef</span><span class="sxs-lookup"><span data-stu-id="ea59e-113">dotnet-ef</span></span>](/ef/core/miscellaneous/cli/dotnet)                                                                  |

#### <a name="version-introduced"></a><span data-ttu-id="ea59e-114">引入的版本</span><span class="sxs-lookup"><span data-stu-id="ea59e-114">Version introduced</span></span>

<span data-ttu-id="ea59e-115">.NET Core SDK 2.1.300</span><span class="sxs-lookup"><span data-stu-id="ea59e-115">.NET Core SDK 2.1.300</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="ea59e-116">建议的操作</span><span class="sxs-lookup"><span data-stu-id="ea59e-116">Recommended action</span></span>

<span data-ttu-id="ea59e-117">从项目中删除 `<DotNetCliToolReference>` 设置。</span><span class="sxs-lookup"><span data-stu-id="ea59e-117">Remove the `<DotNetCliToolReference>` setting from your project.</span></span>

#### <a name="category"></a><span data-ttu-id="ea59e-118">类别</span><span class="sxs-lookup"><span data-stu-id="ea59e-118">Category</span></span>

<span data-ttu-id="ea59e-119">MSBuild</span><span class="sxs-lookup"><span data-stu-id="ea59e-119">MSBuild</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="ea59e-120">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="ea59e-120">Affected APIs</span></span>

<span data-ttu-id="ea59e-121">不可用</span><span class="sxs-lookup"><span data-stu-id="ea59e-121">N/A</span></span>
