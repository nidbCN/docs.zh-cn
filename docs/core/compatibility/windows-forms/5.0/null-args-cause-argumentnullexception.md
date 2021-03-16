---
title: 中断性变更：WinForms 方法现在会引发 ArgumentNullException
description: 了解 .NET 5 中的中断性变更：某些 Windows 窗体方法现将针对 null 参数引发 ArgumentNullException。
ms.date: 09/18/2020
ms.openlocfilehash: 9d72f8e3320430396132de20c252cd5e8759dce3
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102256087"
---
# <a name="winforms-methods-now-throw-argumentnullexception"></a><span data-ttu-id="7a103-103">WinForms 方法现在会引发 ArgumentNullException</span><span class="sxs-lookup"><span data-stu-id="7a103-103">WinForms methods now throw ArgumentNullException</span></span>

<span data-ttu-id="7a103-104">某些 Windows 窗体方法现在将针对 null 参数引发 <xref:System.ArgumentNullException>，而之前它们会引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="7a103-104">Some Windows Forms methods now throw an <xref:System.ArgumentNullException> for null arguments, where previously they threw a <xref:System.NullReferenceException>.</span></span>

## <a name="change-description"></a><span data-ttu-id="7a103-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="7a103-105">Change description</span></span>

<span data-ttu-id="7a103-106">以前，如果传递 null 参数，某些 Windows 窗体方法将引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="7a103-106">Previously, certain Windows Forms methods threw a <xref:System.NullReferenceException> if passed an argument that was null.</span></span> <span data-ttu-id="7a103-107">从 .NET 5 开始，这些方法现将针对 null 参数引发 <xref:System.ArgumentNullException>。</span><span class="sxs-lookup"><span data-stu-id="7a103-107">Starting in .NET 5, these methods now throw an <xref:System.ArgumentNullException> for null arguments, instead.</span></span>

<span data-ttu-id="7a103-108">引发 <xref:System.ArgumentNullException> 符合 .NET 运行时的行为。</span><span class="sxs-lookup"><span data-stu-id="7a103-108">Throwing an <xref:System.ArgumentNullException> conforms to the behavior of the .NET runtime.</span></span> <span data-ttu-id="7a103-109">它还通过清楚地指示参数为 null 和具体参数来改进调试体验。</span><span class="sxs-lookup"><span data-stu-id="7a103-109">It also improves the debugging experience by clearly communicating that an argument is null and which argument it is.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="7a103-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="7a103-110">Version introduced</span></span>

<span data-ttu-id="7a103-111">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="7a103-111">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="7a103-112">建议操作</span><span class="sxs-lookup"><span data-stu-id="7a103-112">Recommended action</span></span>

<span data-ttu-id="7a103-113">如果调用这些方法中的任何一种，你的代码当前都会针对 null 参数捕获 <xref:System.NullReferenceException>，请改为捕获 <xref:System.ArgumentNullException>。</span><span class="sxs-lookup"><span data-stu-id="7a103-113">If you call any of these methods and your code currently catches a <xref:System.NullReferenceException> for null arguments, catch an <xref:System.ArgumentNullException> instead.</span></span> <span data-ttu-id="7a103-114">此外，请考虑更新代码，以避免将 null 参数传递到列出的方法。</span><span class="sxs-lookup"><span data-stu-id="7a103-114">In addition, consider updating the code to prevent passing null arguments to the listed methods.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="7a103-115">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="7a103-115">Affected APIs</span></span>

<span data-ttu-id="7a103-116">下表列出了受影响的方法和参数：</span><span class="sxs-lookup"><span data-stu-id="7a103-116">The following table lists the affected methods and parameters:</span></span>

> [!div class="mx-tdBreakAll"]
> | <span data-ttu-id="7a103-117">方法</span><span class="sxs-lookup"><span data-stu-id="7a103-117">Method</span></span> | <span data-ttu-id="7a103-118">参数名称</span><span class="sxs-lookup"><span data-stu-id="7a103-118">Parameter name</span></span> | <span data-ttu-id="7a103-119">新增的版本</span><span class="sxs-lookup"><span data-stu-id="7a103-119">Version added</span></span> |
> |-|-|-|
> | <xref:System.Windows.Forms.Control.ControlCollection.%23ctor(System.Windows.Forms.Control)> | `owner` | <span data-ttu-id="7a103-120">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-120">Preview 1</span></span> |
> | <xref:System.Windows.Forms.TabControl.GetToolTipText(System.Object)?displayProperty=nameWithType> | `item` | <span data-ttu-id="7a103-121">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-121">Preview 1</span></span> |
> | <xref:System.Windows.Forms.TableLayoutControlCollection.%23ctor(System.Windows.Forms.TableLayoutPanel)> | `container` | <span data-ttu-id="7a103-122">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-122">Preview 1</span></span> |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderArrow(System.Windows.Forms.ToolStripArrowRenderEventArgs)?displayProperty=nameWithType> | `e` | <span data-ttu-id="7a103-123">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-123">Preview 1</span></span> |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderItemCheck(System.Windows.Forms.ToolStripItemImageRenderEventArgs)?displayProperty=nameWithType> | `e` | <span data-ttu-id="7a103-124">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-124">Preview 1</span></span> |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderItemImage(System.Windows.Forms.ToolStripItemImageRenderEventArgs)?displayProperty=nameWithType> | `e` | <span data-ttu-id="7a103-125">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-125">Preview 1</span></span> |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderItemText(System.Windows.Forms.ToolStripItemTextRenderEventArgs)?displayProperty=nameWithType> | `e` | <span data-ttu-id="7a103-126">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-126">Preview 1</span></span> |
> | <xref:System.Windows.Forms.ToolStripRenderer.OnRenderStatusStripSizingGrip(System.Windows.Forms.ToolStripRenderEventArgs)?displayProperty=nameWithType> > | `e` | <span data-ttu-id="7a103-127">预览版 1</span><span class="sxs-lookup"><span data-stu-id="7a103-127">Preview 1</span></span> |
> | <xref:System.Windows.Forms.DataGridViewComboBoxEditingControl.ApplyCellStyleToEditingControl(System.Windows.Forms.DataGridViewCellStyle)?displayProperty=nameWithType> | `dataGridViewCellStyle` | <span data-ttu-id="7a103-128">预览版 2</span><span class="sxs-lookup"><span data-stu-id="7a103-128">Preview 2</span></span> |
> | <xref:System.Windows.Forms.RichTextBox.LoadFile(System.IO.Stream,System.Windows.Forms.RichTextBoxStreamType)?displayProperty=nameWithType> | `data` | <span data-ttu-id="7a103-129">预览版 2</span><span class="sxs-lookup"><span data-stu-id="7a103-129">Preview 2</span></span> |
> | <xref:System.Windows.Forms.ListBox.IntegerCollection.%23ctor(System.Windows.Forms.ListBox)> | `owner` | <span data-ttu-id="7a103-130">预览版 5</span><span class="sxs-lookup"><span data-stu-id="7a103-130">Preview 5</span></span> |
> | <xref:System.Windows.Forms.ListBox.IntegerCollection.CopyTo(System.Array,System.Int32)?displayProperty=nameWithType> | `destination` | <span data-ttu-id="7a103-131">预览版 5</span><span class="sxs-lookup"><span data-stu-id="7a103-131">Preview 5</span></span> |
> | <xref:System.Windows.Forms.ListViewGroup.System%23Runtime%23Serialization%23ISerializable%23GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)?displayProperty=nameWithType> | `info` | <span data-ttu-id="7a103-132">预览版 5</span><span class="sxs-lookup"><span data-stu-id="7a103-132">Preview 5</span></span> |
> | <xref:System.Windows.Forms.VisualStyles.VisualStyleRenderer.%23ctor(System.String,System.Int32,System.Int32)> | `className` | <span data-ttu-id="7a103-133">预览版 5</span><span class="sxs-lookup"><span data-stu-id="7a103-133">Preview 5</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox)> | `owner` | <span data-ttu-id="7a103-134">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-134">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox,System.Object[])> | <span data-ttu-id="7a103-135">`owner`, `value`</span><span class="sxs-lookup"><span data-stu-id="7a103-135">`owner`, `value`</span></span> | <span data-ttu-id="7a103-136">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-136">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.%23ctor(System.Windows.Forms.ListBox,System.Windows.Forms.ListBox.ObjectCollection)> | <span data-ttu-id="7a103-137">`owner`, `value`</span><span class="sxs-lookup"><span data-stu-id="7a103-137">`owner`, `value`</span></span> | <span data-ttu-id="7a103-138">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-138">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Object[])?displayProperty=nameWithType> | `items` | <span data-ttu-id="7a103-139">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-139">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Windows.Forms.ListBox.ObjectCollection)?displayProperty=nameWithType> | `value` | <span data-ttu-id="7a103-140">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-140">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.CopyTo(System.Object[],System.Int32)?displayProperty=nameWithType> | `destination` | <span data-ttu-id="7a103-141">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-141">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListBox.ObjectCollection.System%23Collections%23ICollection%23CopyTo(System.Array,System.Int32)?displayProperty=nameWithType> | `destination` | <span data-ttu-id="7a103-142">预览版 6</span><span class="sxs-lookup"><span data-stu-id="7a103-142">Preview 6</span></span> |
> | <xref:System.Windows.Forms.ListView.SelectedIndexCollection.%23ctor(System.Windows.Forms.ListView)> | `owner` | <span data-ttu-id="7a103-143">预览版 7</span><span class="sxs-lookup"><span data-stu-id="7a103-143">Preview 7</span></span> |
> | <xref:System.Windows.Forms.TreeNodeCollection.Find(System.String,System.Boolean)?displayProperty=nameWithType> | <span data-ttu-id="7a103-144">`key` 为 `null` 或空</span><span class="sxs-lookup"><span data-stu-id="7a103-144">`key` is `null` or empty</span></span> | <span data-ttu-id="7a103-145">预览版 8</span><span class="sxs-lookup"><span data-stu-id="7a103-145">Preview 8</span></span> |
> | <xref:System.Windows.Forms.ListView.ListViewItemCollection.Find(System.String,System.Boolean)?displayProperty=nameWithType> | <span data-ttu-id="7a103-146">`key` 为 `null` 或空</span><span class="sxs-lookup"><span data-stu-id="7a103-146">`key` is `null` or empty</span></span> | <span data-ttu-id="7a103-147">RC1</span><span class="sxs-lookup"><span data-stu-id="7a103-147">RC1</span></span> |
> | <xref:System.Windows.Forms.ScrollableControl.OnPaintBackground(System.Windows.Forms.PaintEventArgs)?displayProperty=nameWithType> | `e` | <span data-ttu-id="7a103-148">RC1</span><span class="sxs-lookup"><span data-stu-id="7a103-148">RC1</span></span> |

<!--

### Affected APIs

- `M:System.Windows.Forms.Control.ControlCollection.#ctor(System.Windows.Forms.Control)`
- `M:System.Windows.Forms.TabControl.GetToolTipText(System.Object)`
- `M:System.Windows.Forms.TableLayoutControlCollection.#ctor(System.Windows.Forms.TableLayoutPanel)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderArrow(System.Windows.Forms.ToolStripArrowRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderItemImage(System.Windows.Forms.ToolStripItemImageRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderItemCheck(System.Windows.Forms.ToolStripItemImageRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderItemText(System.Windows.Forms.ToolStripItemTextRenderEventArgs)`
- `M:System.Windows.Forms.ToolStripRenderer.OnRenderStatusStripSizingGrip(System.Windows.Forms.ToolStripRenderEventArgs)`
- `M:System.Windows.Forms.DataGridViewComboBoxEditingControl.ApplyCellStyleToEditingControl(System.Windows.Forms.DataGridViewCellStyle)`
- `M:System.Windows.Forms.RichTextBox.LoadFile(System.IO.Stream,System.Windows.Forms.RichTextBoxStreamType)`
- `M:System.Windows.Forms.ListViewGroup.System#Runtime#Serialization#ISerializable#GetObjectData(System.Runtime.Serialization.SerializationInfo,System.Runtime.Serialization.StreamingContext)`
- `M:System.Windows.Forms.VisualStyles.VisualStyleRenderer.#ctor(System.String,System.Int32,System.Int32)`
- `M:System.Windows.Forms.ListBox.IntegerCollection.#ctor(System.Windows.Forms.ListBox)`
- `M:System.Windows.Forms.ListBox.IntegerCollection.CopyTo(System.Array,System.Int32)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.#ctor(System.Windows.Forms.ListBox)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.#ctor(System.Windows.Forms.ListBox,System.Object[])`
- `M:System.Windows.Forms.ListBox.ObjectCollection.#ctor(System.Windows.Forms.ListBox,System.Windows.Forms.ListBox.ObjectCollection)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Object[])`
- `M:System.Windows.Forms.ListBox.ObjectCollection.AddRange(System.Windows.Forms.ListBox.ObjectCollection)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.CopyTo(System.Object[],System.Int32)`
- `M:System.Windows.Forms.ListBox.ObjectCollection.System#Collections#ICollection#CopyTo(System.Array,System.Int32)`
- `M:System.Windows.Forms.ListView.SelectedIndexCollection.#ctor(System.Windows.Forms.ListView)`
- `M:System.Windows.Forms.TreeNodeCollection.Find(System.String,System.Boolean)`
- `M:System.Windows.Forms.ListView.ListViewItemCollection.Find(System.String,System.Boolean)`
- `M:System.Windows.Forms.ScrollableControl.OnPaintBackground(System.Windows.Forms.PaintEventArgs)`

### Category

Windows Forms

-->
