---
title: 中断性变更：已删除的状态栏控件
description: 了解 .NET 5 中的中断性变更：某些 Windows 窗体控件不再可用。
ms.date: 07/18/2020
ms.openlocfilehash: c93b16047896b263248858e807b74c547cfa6d9d
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256186"
---
# <a name="removed-status-bar-controls"></a><span data-ttu-id="b54eb-103">已删除的状态栏控件</span><span class="sxs-lookup"><span data-stu-id="b54eb-103">Removed status bar controls</span></span>

<span data-ttu-id="b54eb-104">从 .NET 5 开始，某些 Windows 窗体控件不再可用。</span><span class="sxs-lookup"><span data-stu-id="b54eb-104">Starting in .NET 5, some Windows Forms controls are no longer available.</span></span>

## <a name="change-description"></a><span data-ttu-id="b54eb-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="b54eb-105">Change description</span></span>

<span data-ttu-id="b54eb-106">从 .NET 5 开始，一些与状态栏相关的 Windows 窗体控件不再可用。</span><span class="sxs-lookup"><span data-stu-id="b54eb-106">Starting with .NET 5, some of the status bar-related Windows Forms controls are no longer available.</span></span> <span data-ttu-id="b54eb-107">.NET Framework 2.0 中引入改进了设计和支持的替换控件。</span><span class="sxs-lookup"><span data-stu-id="b54eb-107">Replacement controls that have better design and support were introduced in .NET Framework 2.0.</span></span> <span data-ttu-id="b54eb-108">弃用的控件之前已从设计器工具箱中删除，但仍可供使用。</span><span class="sxs-lookup"><span data-stu-id="b54eb-108">The deprecated controls were previously removed from designer toolboxes but were still available to be used.</span></span> <span data-ttu-id="b54eb-109">现在，已完全删除这些控件。</span><span class="sxs-lookup"><span data-stu-id="b54eb-109">Now, they have been completely removed.</span></span>

<span data-ttu-id="b54eb-110">以下类型不再可用：</span><span class="sxs-lookup"><span data-stu-id="b54eb-110">The following types are no longer available:</span></span>

* `StatusBar`
* `StatusBarDrawItemEventArgs`
* `StatusBarDrawItemEventHandler`
* `StatusBarPanel`
* `StatusBarPanelAutoSize`
* `StatusBarPanelBorderStyle`
* `StatusBarPanelClickEventArgs`
* `StatusBarPanelClickEventHandler`
* `StatusBarPanelStyle`

## <a name="version-introduced"></a><span data-ttu-id="b54eb-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="b54eb-111">Version introduced</span></span>

<span data-ttu-id="b54eb-112">5.0</span><span class="sxs-lookup"><span data-stu-id="b54eb-112">5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="b54eb-113">建议操作</span><span class="sxs-lookup"><span data-stu-id="b54eb-113">Recommended action</span></span>

<span data-ttu-id="b54eb-114">转到这些控件及其应用场景的替代 API：</span><span class="sxs-lookup"><span data-stu-id="b54eb-114">Move to the replacement APIs for these controls and their scenarios:</span></span>

| <span data-ttu-id="b54eb-115">旧控件 (API)</span><span class="sxs-lookup"><span data-stu-id="b54eb-115">Old Control (API)</span></span> | <span data-ttu-id="b54eb-116">推荐的替换控件</span><span class="sxs-lookup"><span data-stu-id="b54eb-116">Recommended Replacement</span></span>                          |
|-------------------|--------------------------------------------------|
| <span data-ttu-id="b54eb-117">StatusBar</span><span class="sxs-lookup"><span data-stu-id="b54eb-117">StatusBar</span></span>         | <xref:System.Windows.Forms.StatusStrip>          |
| <span data-ttu-id="b54eb-118">StatusBarPanel</span><span class="sxs-lookup"><span data-stu-id="b54eb-118">StatusBarPanel</span></span>    | <xref:System.Windows.Forms.ToolStripStatusLabel> |

## <a name="affected-apis"></a><span data-ttu-id="b54eb-119">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="b54eb-119">Affected APIs</span></span>

- <xref:System.Windows.Forms.StatusBar?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarDrawItemEventArgs?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarDrawItemEventHandler?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarPanel?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarPanelAutoSize?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarPanelBorderStyle?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarPanelClickEventArgs?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarPanelClickEventHandler?displayProperty=fullName>
- <xref:System.Windows.Forms.StatusBarPanelStyle?displayProperty=fullName>

<!--

### Affected APIs

- `T:System.Windows.Forms.StatusBar`
- `T:System.Windows.Forms.StatusBarDrawItemEventArgs`
- `T:System.Windows.Forms.StatusBarDrawItemEventHandler`
- `T:System.Windows.Forms.StatusBarPanel`
- `T:System.Windows.Forms.StatusBarPanelAutoSize`
- `T:System.Windows.Forms.StatusBarPanelBorderStyle`
- `T:System.Windows.Forms.StatusBarPanelClickEventArgs`
- `T:System.Windows.Forms.StatusBarPanelClickEventHandler`
- `T:System.Windows.Forms.StatusBarPanelStyle`

### Category

Windows Forms

-->
