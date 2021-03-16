---
title: 中断性变更：默认导入 Directory.Packages.props 文件
description: 了解 .NET 5 中的中断性变更：如果在项目文件夹中找到名为 Directory.Packages.props 的文件，则 NuGet 的 .props 文件会自动导入该文件。
ms.date: 09/17/2020
ms.openlocfilehash: a031d9b2bd832be06dedb20495c24075d1239b02
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256577"
---
# <a name="directorypackagesprops-files-is-imported-by-default"></a><span data-ttu-id="b5bed-103">默认导入 Directory.Packages.props 文件</span><span class="sxs-lookup"><span data-stu-id="b5bed-103">Directory.Packages.props files is imported by default</span></span>

<span data-ttu-id="b5bed-104">如果在项目文件夹或其任何上级中找到名为 Directory.Packages.props 的文件，则 NuGet 的 .props 文件会自动导入该文件 。</span><span class="sxs-lookup"><span data-stu-id="b5bed-104">NuGet's *.props* files automatically import a file named *Directory.Packages.props* if it's found in the project folder or any of its ancestors.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="b5bed-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="b5bed-105">Version introduced</span></span>

<span data-ttu-id="b5bed-106">5.0</span><span class="sxs-lookup"><span data-stu-id="b5bed-106">5.0</span></span>

## <a name="change-description"></a><span data-ttu-id="b5bed-107">更改描述</span><span class="sxs-lookup"><span data-stu-id="b5bed-107">Change description</span></span>

<span data-ttu-id="b5bed-108">在以前的 .NET 版本中，项目文件中有一个名为 Directory.Packages.props 的文件，并且 NuGet 的 .props 文件不会在生成时自动导入该文件 。</span><span class="sxs-lookup"><span data-stu-id="b5bed-108">In previous .NET versions, you could have a file named *Directory.Packages.props* in your project file and it wouldn't be automatically imported by NuGet's *.props* file at build time.</span></span>

<span data-ttu-id="b5bed-109">从 .NET 5 开始，如果项目文件夹或其任何上级中存在这样的文件，将自动导入它。</span><span class="sxs-lookup"><span data-stu-id="b5bed-109">Starting in .NET 5, such a file *is* automatically imported if it exists in the project folder or any of its ancestors.</span></span> <span data-ttu-id="b5bed-110">如果项目文件夹中有一个具有此名称的文件，则此自动导入可能会更改生成的行为。</span><span class="sxs-lookup"><span data-stu-id="b5bed-110">If you have a file with this name in your project folder, this automatic import could change behavior of the build.</span></span> <span data-ttu-id="b5bed-111">例如，将导入该文件，但是与之前有所不同，如果你特意导入该文件，则导入顺序可能会发生变化。</span><span class="sxs-lookup"><span data-stu-id="b5bed-111">For example, the file will be imported but it wasn't before, or the order of when it's imported could change if you specifically import it.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="b5bed-112">更改原因</span><span class="sxs-lookup"><span data-stu-id="b5bed-112">Reason for change</span></span>

<span data-ttu-id="b5bed-113">进行此更改是为了支持 NuGet 的[中央包版本控制](https://github.com/NuGet/Home/wiki/Centrally-managing-NuGet-package-versions)。</span><span class="sxs-lookup"><span data-stu-id="b5bed-113">This change was made in order to support [central package versioning](https://github.com/NuGet/Home/wiki/Centrally-managing-NuGet-package-versions) for NuGet.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="b5bed-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="b5bed-114">Recommended action</span></span>

<span data-ttu-id="b5bed-115">如果不应自动导入现有 Directory.Packages.props 文件，请重命名此文件。</span><span class="sxs-lookup"><span data-stu-id="b5bed-115">Rename the existing *Directory.Packages.props* file if it should not be imported automatically.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="b5bed-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="b5bed-116">Affected APIs</span></span>

<span data-ttu-id="b5bed-117">不可用</span><span class="sxs-lookup"><span data-stu-id="b5bed-117">N/A</span></span>

<!--

### Affected APIs

Not detectable via API analysis.

### Category

MSBuild

-->
