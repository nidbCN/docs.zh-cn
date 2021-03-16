---
title: 中断性变更：TreeNodeCollection.Item(Int32) 为正在使用的节点抛出 ArgumentException
description: 了解 .NET 6 中的中断性变更：如果要分配的节点已分配给 TreeView，TreeNodeCollection.Item(Int32) 现在会抛出 ArgumentException。
ms.date: 01/19/2021
ms.openlocfilehash: 29727da2e4bc3d6ac89ed88819bf03d058603f77
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255682"
---
# <a name="treenodecollectionitem-throws-exception-if-node-is-assigned-elsewhere"></a><span data-ttu-id="c6f39-103">如果节点被分配到其他地方，则 TreeNodeCollection.Item 抛出异常</span><span class="sxs-lookup"><span data-stu-id="c6f39-103">TreeNodeCollection.Item throws exception if node is assigned elsewhere</span></span>

<span data-ttu-id="c6f39-104">如果要分配的节点已绑定到另一个 <xref:System.Windows.Forms.TreeView> 或位于不同索引上的此 <xref:System.Windows.Forms.TreeView>，则 <xref:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)?displayProperty=nameWithType> 会抛出 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="c6f39-104"><xref:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)?displayProperty=nameWithType> throws an <xref:System.ArgumentException> if the node being assigned is already bound to another <xref:System.Windows.Forms.TreeView> or to this <xref:System.Windows.Forms.TreeView> at a different index.</span></span>

## <a name="change-description"></a><span data-ttu-id="c6f39-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="c6f39-105">Change description</span></span>

<span data-ttu-id="c6f39-106">在旧版 .NET 中，可以将树节点分配到集合，即使它已绑定到 <xref:System.Windows.Forms.TreeView>。</span><span class="sxs-lookup"><span data-stu-id="c6f39-106">In previous .NET versions, you can assign a tree node to a collection even if it's already bound to a <xref:System.Windows.Forms.TreeView>.</span></span> <span data-ttu-id="c6f39-107">这可能会导致重复的节点。</span><span class="sxs-lookup"><span data-stu-id="c6f39-107">This can lead to duplicated nodes.</span></span> <span data-ttu-id="c6f39-108">自 .NET 6 起，如果要分配的节点已绑定到另一个 <xref:System.Windows.Forms.TreeView> 或位于不同索引上的此 <xref:System.Windows.Forms.TreeView>，则 <xref:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)?displayProperty=nameWithType> 会引发 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="c6f39-108">Starting in .NET 6, <xref:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)?displayProperty=nameWithType> throws an <xref:System.ArgumentException> if the node being assigned is already bound to another <xref:System.Windows.Forms.TreeView> or to this <xref:System.Windows.Forms.TreeView> at a different index.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="c6f39-109">更改原因</span><span class="sxs-lookup"><span data-stu-id="c6f39-109">Reason for change</span></span>

<span data-ttu-id="c6f39-110">验证输入参数是否与其他 `TreeNodeCollection` API 的行为一致。</span><span class="sxs-lookup"><span data-stu-id="c6f39-110">Validating the input parameter is consistent with the behavior of other `TreeNodeCollection` APIs.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="c6f39-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c6f39-111">Version introduced</span></span>

<span data-ttu-id="c6f39-112">.NET 6 预览版 1</span><span class="sxs-lookup"><span data-stu-id="c6f39-112">.NET 6 Preview 1</span></span>

## <a name="recommended-action"></a><span data-ttu-id="c6f39-113">建议的操作</span><span class="sxs-lookup"><span data-stu-id="c6f39-113">Recommended action</span></span>

<span data-ttu-id="c6f39-114">在将 `TreeNode` 分配到集合之前，请务必先取消绑定它。</span><span class="sxs-lookup"><span data-stu-id="c6f39-114">Make sure to unbind a `TreeNode` before assigning it to the collection.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="c6f39-115">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="c6f39-115">Affected APIs</span></span>

<xref:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)?displayProperty=fullName>

<!--

### Affected APIs

- `P:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)`

### Category

Windows Forms

-->
