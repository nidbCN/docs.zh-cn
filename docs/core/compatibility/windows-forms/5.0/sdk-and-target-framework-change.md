---
title: 中断性变更：WinForms 和 WPF 应用使用 Microsoft.NET.Sdk
description: 了解 .NET 5 中的中断性变更：Windows 窗体和 Windows Presentation Framework 应用现使用 .NET SDK，而不使用 .NET Core WinForms 和 WPF SDK。
ms.date: 09/18/2020
ms.openlocfilehash: 408233f6f8801fb3d4e53beab28c26a777a4a3e1
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256200"
---
# <a name="winforms-and-wpf-apps-use-microsoftnetsdk"></a><span data-ttu-id="48588-103">WinForms 和 WPF 应用使用 Microsoft.NET.Sdk</span><span class="sxs-lookup"><span data-stu-id="48588-103">WinForms and WPF apps use Microsoft.NET.Sdk</span></span>

<span data-ttu-id="48588-104">Windows 窗体和 Windows Presentation Framework (WPF) 应用现在使用 .NET SDK (`Microsoft.NET.Sdk`)，而不使用 .NET Core WinForms 和 WPF SDK (`Microsoft.NET.Sdk.WindowsDesktop`)。</span><span class="sxs-lookup"><span data-stu-id="48588-104">Windows Forms and Windows Presentation Framework (WPF) apps now use the .NET SDK (`Microsoft.NET.Sdk`) instead of the .NET Core WinForms and WPF SDK (`Microsoft.NET.Sdk.WindowsDesktop`).</span></span>

## <a name="change-description"></a><span data-ttu-id="48588-105">更改说明</span><span class="sxs-lookup"><span data-stu-id="48588-105">Change description</span></span>

<span data-ttu-id="48588-106">在以前的 .NET Core 版本中，WinForms 和 WPF 应用使用单独的[项目 SDK](../../../project-sdk/overview.md) (`Microsoft.NET.Sdk.WindowsDesktop`)。</span><span class="sxs-lookup"><span data-stu-id="48588-106">In previous .NET Core versions, WinForms and WPF apps used a separate [project SDK](../../../project-sdk/overview.md) (`Microsoft.NET.Sdk.WindowsDesktop`).</span></span> <span data-ttu-id="48588-107">从 .NET 5 开始，WinForms 和 WPF SDK 已与 .NET SDK (`Microsoft.NET.Sdk`) 统一。</span><span class="sxs-lookup"><span data-stu-id="48588-107">Starting in .NET 5, the WinForms and WPF SDK has been unified with the .NET SDK (`Microsoft.NET.Sdk`).</span></span> <span data-ttu-id="48588-108">此外，新的[目标框架名字对象 (TFM)](../../../../standard/frameworks.md) 替换 .NET 5 中的 `netcoreapp` 和 `netstandard`。</span><span class="sxs-lookup"><span data-stu-id="48588-108">In addition, new [target framework monikers (TFM)](../../../../standard/frameworks.md) replace `netcoreapp` and `netstandard` in .NET 5.</span></span> <span data-ttu-id="48588-109">下面的示例显示了在重新面向 .NET 5 或更高版本时，需要对 WPF 项目文件进行的更改。</span><span class="sxs-lookup"><span data-stu-id="48588-109">The following example shows the changes you'd need to make for a WPF project file when retargeting to .NET 5 or later.</span></span>

<span data-ttu-id="48588-110">在以前的 .NET Core 版本中：</span><span class="sxs-lookup"><span data-stu-id="48588-110">In previous .NET Core versions:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>netcoreapp3.1</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

</Project>
```

<span data-ttu-id="48588-111">在 .NET 5 及更高版本中：</span><span class="sxs-lookup"><span data-stu-id="48588-111">In .NET 5 and later versions:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net5.0-windows</TargetFramework>
    <UseWPF>true</UseWPF>
  </PropertyGroup>

</Project>
```

## <a name="version-introduced"></a><span data-ttu-id="48588-112">引入的版本</span><span class="sxs-lookup"><span data-stu-id="48588-112">Version introduced</span></span>

<span data-ttu-id="48588-113">.NET SDK 5.0.100</span><span class="sxs-lookup"><span data-stu-id="48588-113">.NET SDK 5.0.100</span></span>

## <a name="recommended-action"></a><span data-ttu-id="48588-114">建议操作</span><span class="sxs-lookup"><span data-stu-id="48588-114">Recommended action</span></span>

<span data-ttu-id="48588-115">在 WPF 或 Windows 窗体项目文件中：</span><span class="sxs-lookup"><span data-stu-id="48588-115">In your WPF or Windows Forms project file:</span></span>

- <span data-ttu-id="48588-116">将 `Sdk` 属性更新为 `Microsoft.NET.Sdk`。</span><span class="sxs-lookup"><span data-stu-id="48588-116">Update the `Sdk` attribute  to `Microsoft.NET.Sdk`.</span></span>
- <span data-ttu-id="48588-117">将 `TargetFramework` 属性更新为 `net5.0-windows`。</span><span class="sxs-lookup"><span data-stu-id="48588-117">Update the `TargetFramework` property to `net5.0-windows`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="48588-118">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="48588-118">Affected APIs</span></span>

<span data-ttu-id="48588-119">无法通过 API 分析检测到。</span><span class="sxs-lookup"><span data-stu-id="48588-119">Not detectable via API analysis.</span></span>

<!--

### Affected APIs

Not detectable via API analysis.

### Category

- Windows Forms
- Windows Presentation Framework (WPF)

-->
