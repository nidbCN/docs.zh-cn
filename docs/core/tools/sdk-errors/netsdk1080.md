---
title: NETSDK1080：针对 Microsoft.AspNetCore.App 的 PackageReference 不是必需的
description: 了解 .NET SDK 错误 NETSDK1080，它针对项目文件中不必要的条目发出警告。
ms.topic: error-reference
ms.date: 02/25/2021
f1_keywords:
- NETSDK1080
ms.openlocfilehash: ebf9484e4a358597a1177f95e92f68330180cd35
ms.sourcegitcommit: bdbf6472de867a0a11aaa5b9384a2506c24f27d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102206786"
---
# <a name="netsdk1080-packagereference-to-microsoftaspnetcoreapp-is-not-necessary"></a><span data-ttu-id="6bd4a-103">NETSDK1080：针对 Microsoft.AspNetCore.App 的 PackageReference 不是必需的</span><span class="sxs-lookup"><span data-stu-id="6bd4a-103">NETSDK1080: PackageReference to Microsoft.AspNetCore.App is not necessary</span></span>

<span data-ttu-id="6bd4a-104">NETSDK1080 警告你注意你的项目文件中 `Microsoft.AspNetCore.App` 的 `PackageReference` 元素不是必需的。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-104">NETSDK1080 warns you that the `PackageReference` element for `Microsoft.AspNetCore.App` in your project file is not necessary.</span></span> <span data-ttu-id="6bd4a-105">完整的错误消息类似于以下示例：</span><span class="sxs-lookup"><span data-stu-id="6bd4a-105">The full error message is similar to the following example:</span></span>

> <span data-ttu-id="6bd4a-106">警告 NETSDK1080：当面向 .NET Core 3.0 或更高版本时，针对 Microsoft.AspNetCore.App 的 PackageReference 不是必需的。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-106">warning NETSDK1080: A PackageReference to Microsoft.AspNetCore.App is not necessary when targeting .NET Core 3.0 or higher.</span></span> <span data-ttu-id="6bd4a-107">如果使用的是 Microsoft.NET.Sdk.Web，则会自动引用共享框架。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-107">If Microsoft.NET.Sdk.Web is used, the shared framework will be referenced automatically.</span></span> <span data-ttu-id="6bd4a-108">否则，应将 PackageReference 替换为 FrameworkReference。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-108">Otherwise, the PackageReference should be replaced with a FrameworkReference.</span></span>

<span data-ttu-id="6bd4a-109">通常，从在项目文件中需要 `PackageReference` 条目的更低版本将项目升级到 .NET Core 3.0 或更高版本后，会出现此错误。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-109">This error typically occurs after you've upgraded a project to .NET Core 3.0 or later, from an earlier version that required `PackageReference` entries in the project file.</span></span>

## <a name="aspnet-core-project-files"></a><span data-ttu-id="6bd4a-110">ASP.NET Core 项目文件</span><span class="sxs-lookup"><span data-stu-id="6bd4a-110">ASP.NET Core project files</span></span>

<span data-ttu-id="6bd4a-111">例如，原始项目文件可能如下例所示：</span><span class="sxs-lookup"><span data-stu-id="6bd4a-111">For example, your original project file might look like this example:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
    <AspNetCoreHostingModel>InProcess</AspNetCoreHostingModel>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App"/>
    <PackageReference Include="Microsoft.AspNetCore.Razor.Design" Version="2.2.0" PrivateAssets="All" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="6bd4a-112">更新到 .NET Core 3.1 后，同一项目的项目文件应如下例所示：</span><span class="sxs-lookup"><span data-stu-id="6bd4a-112">After updating to .NET Core 3.1 the project file for the same project should look like the following example:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>

</Project>
```

<span data-ttu-id="6bd4a-113">若要消除此警告，请进行这些更改，尤其是删除 `PackageReference` 元素。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-113">Make these changes, in particular delete the `PackageReference` element, to eliminate the warning.</span></span> <span data-ttu-id="6bd4a-114">有关详细信息，请查看[删除过时的包引用](/aspnet/core/migration/22-to-30#remove-obsolete-package-references)。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-114">For more information, see [Remove obsolete package references](/aspnet/core/migration/22-to-30#remove-obsolete-package-references).</span></span>

## <a name="class-library-project"></a><span data-ttu-id="6bd4a-115">类库项目</span><span class="sxs-lookup"><span data-stu-id="6bd4a-115">Class library project</span></span>

<span data-ttu-id="6bd4a-116">在使用 ASP.NET Core API 的类库项目中，将 `PackageReference` 替换为 `FrameworkReference`，如下例中所示：</span><span class="sxs-lookup"><span data-stu-id="6bd4a-116">In a class library project that uses ASP.NET Core APIs, replace the `PackageReference` with a `FrameworkReference`, as shown in the following example:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <FrameworkReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="6bd4a-117">有关详细信息，请查看[在类库中使用 ASP.NET Core API](/aspnet/core/fundamentals/target-aspnetcore)。</span><span class="sxs-lookup"><span data-stu-id="6bd4a-117">For more information, see [Use ASP.NET Core APIs in a class library](/aspnet/core/fundamentals/target-aspnetcore).</span></span>
