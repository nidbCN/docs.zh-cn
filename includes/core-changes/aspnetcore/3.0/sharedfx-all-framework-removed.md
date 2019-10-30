---
ms.openlocfilehash: 959f3959c28c7d0159be7a213986345e2865b9a2
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394439"
---
### <a name="shared-framework-removed-microsoftaspnetcoreall"></a><span data-ttu-id="dd304-101">共享框架：已删除 Microsoft.AspNetCore.All</span><span class="sxs-lookup"><span data-stu-id="dd304-101">Shared framework: Removed Microsoft.AspNetCore.All</span></span>

<span data-ttu-id="dd304-102">从 ASP.NET Core 3.0 开始，不再生成 `Microsoft.AspNetCore.All` 元包和匹配的 `Microsoft.AspNetCore.All` 共享框架。</span><span class="sxs-lookup"><span data-stu-id="dd304-102">Starting in ASP.NET Core 3.0, the `Microsoft.AspNetCore.All` metapackage and the matching `Microsoft.AspNetCore.All` shared framework are no longer produced.</span></span> <span data-ttu-id="dd304-103">此包在 ASP.NET Core 2.2 中提供，并将继续接收 ASP.NET Core 2.1 中的服务更新。</span><span class="sxs-lookup"><span data-stu-id="dd304-103">This package is available in ASP.NET Core 2.2 and will continue to receive servicing updates in ASP.NET Core 2.1.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="dd304-104">引入的版本</span><span class="sxs-lookup"><span data-stu-id="dd304-104">Version introduced</span></span>

<span data-ttu-id="dd304-105">3.0</span><span class="sxs-lookup"><span data-stu-id="dd304-105">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="dd304-106">旧行为</span><span class="sxs-lookup"><span data-stu-id="dd304-106">Old behavior</span></span>

<span data-ttu-id="dd304-107">应用可以使用 `Microsoft.AspNetCore.All` 元包来针对 .NET Core 上的 `Microsoft.AspNetCore.All` 共享框架。</span><span class="sxs-lookup"><span data-stu-id="dd304-107">Apps could use the `Microsoft.AspNetCore.All` metapackage to target the `Microsoft.AspNetCore.All` shared framework on .NET Core.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="dd304-108">新行为</span><span class="sxs-lookup"><span data-stu-id="dd304-108">New behavior</span></span>

<span data-ttu-id="dd304-109">.NET Core 3.0 不包含 `Microsoft.AspNetCore.All` 共享框架。</span><span class="sxs-lookup"><span data-stu-id="dd304-109">.NET Core 3.0 doesn't include a `Microsoft.AspNetCore.All` shared framework.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="dd304-110">更改原因</span><span class="sxs-lookup"><span data-stu-id="dd304-110">Reason for change</span></span>

<span data-ttu-id="dd304-111">`Microsoft.AspNetCore.All` 元包包含了大量外部依赖项。</span><span class="sxs-lookup"><span data-stu-id="dd304-111">The `Microsoft.AspNetCore.All` metapackage included a large number of external dependencies.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="dd304-112">建议的操作</span><span class="sxs-lookup"><span data-stu-id="dd304-112">Recommended action</span></span>

<span data-ttu-id="dd304-113">迁移项目以使用 `Microsoft.AspNetCore.App` 框架。</span><span class="sxs-lookup"><span data-stu-id="dd304-113">Migrate your project to use the `Microsoft.AspNetCore.App` framework.</span></span> <span data-ttu-id="dd304-114">以前在 `Microsoft.AspNetCore.All` 中可用的组件在 NuGet 上仍然可用。</span><span class="sxs-lookup"><span data-stu-id="dd304-114">Components that were previously available in `Microsoft.AspNetCore.All` are still available on NuGet.</span></span> <span data-ttu-id="dd304-115">这些组件现可与应用一起部署，而不是包含在共享框架中。</span><span class="sxs-lookup"><span data-stu-id="dd304-115">Those components are now deployed with your app instead of being included in the shared framework.</span></span>

#### <a name="category"></a><span data-ttu-id="dd304-116">类别</span><span class="sxs-lookup"><span data-stu-id="dd304-116">Category</span></span>

<span data-ttu-id="dd304-117">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="dd304-117">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="dd304-118">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="dd304-118">Affected APIs</span></span>

<span data-ttu-id="dd304-119">无</span><span class="sxs-lookup"><span data-stu-id="dd304-119">None</span></span>

<!-- 

#### Affected APIs

Not detectable via API analysis

-->