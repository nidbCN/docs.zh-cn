---
title: 中断性变更：增加了 NotifyIcon.Text 最大文本长度
description: 了解 .NET 6 中的中断性变更：NotifyIcon.Text 属性的最大文本长度有所增加。
ms.date: 01/19/2021
ms.openlocfilehash: f87b98dd852ce202689ae9360bee9b6cfbf01794
ms.sourcegitcommit: 9c589b25b005b9a7f87327646020eb85c3b6306f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102255771"
---
# <a name="notifyicontext-maximum-text-length-increased"></a><span data-ttu-id="51707-103">增加了 NotifyIcon.Text 最大文本长度</span><span class="sxs-lookup"><span data-stu-id="51707-103">NotifyIcon.Text maximum text length increased</span></span>

<span data-ttu-id="51707-104"><xref:System.Windows.Forms.NotifyIcon.Text?displayProperty=nameWithType> 属性允许的最大文本长度从 63 增加到了 127。</span><span class="sxs-lookup"><span data-stu-id="51707-104">The maximum text length allowed for the <xref:System.Windows.Forms.NotifyIcon.Text?displayProperty=nameWithType> property increased from 63 to 127.</span></span>

## <a name="change-description"></a><span data-ttu-id="51707-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="51707-105">Change description</span></span>

<span data-ttu-id="51707-106">在以前的 .NET 版本中，<xref:System.Windows.Forms.NotifyIcon.Text?displayProperty=nameWithType> 属性允许的最大文本长度为 63 个字符。</span><span class="sxs-lookup"><span data-stu-id="51707-106">In previous .NET versions, the maximum text length allowed for the <xref:System.Windows.Forms.NotifyIcon.Text?displayProperty=nameWithType> property is 63 characters.</span></span> <span data-ttu-id="51707-107">从 .NET 6 开始，允许的最大文本长度为 127 个字符。</span><span class="sxs-lookup"><span data-stu-id="51707-107">Starting in .NET 6, the maximum allowed text length is 127 characters.</span></span> <span data-ttu-id="51707-108">在任何版本中，如果试图设置超过该限制的值，则都会引发 <xref:System.ArgumentException>。</span><span class="sxs-lookup"><span data-stu-id="51707-108">In any version, an <xref:System.ArgumentException> is thrown when you attempt to set a value that's longer than the limit.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="51707-109">更改原因</span><span class="sxs-lookup"><span data-stu-id="51707-109">Reason for change</span></span>

<span data-ttu-id="51707-110">允许的最大文本长度已增加到与[基础 Windows API](/windows/win32/api/shellapi/ns-shellapi-notifyicondataw#nif_showtip-0x00000080) 一致。</span><span class="sxs-lookup"><span data-stu-id="51707-110">The maximum allowed text length was increased to be in line with the [underlying Windows API](/windows/win32/api/shellapi/ns-shellapi-notifyicondataw#nif_showtip-0x00000080).</span></span> <span data-ttu-id="51707-111">Windows API 在 Windows 2000 中已更新，但是由于兼容性原因，.NET Framework 中未更新此属性的大小限制。</span><span class="sxs-lookup"><span data-stu-id="51707-111">The Windows API was updated in Windows 2000, but due to compatibility reasons, the size limit for this property wasn't updated in .NET Framework.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="51707-112">引入的版本</span><span class="sxs-lookup"><span data-stu-id="51707-112">Version introduced</span></span>

<span data-ttu-id="51707-113">.NET 6 预览版 1</span><span class="sxs-lookup"><span data-stu-id="51707-113">.NET 6 Preview 1</span></span>

## <a name="recommended-action"></a><span data-ttu-id="51707-114">建议的操作</span><span class="sxs-lookup"><span data-stu-id="51707-114">Recommended action</span></span>

<span data-ttu-id="51707-115">查看代码，并在必要时放宽任何现有的保护条件。</span><span class="sxs-lookup"><span data-stu-id="51707-115">Review your code and relax any existing guard conditions, if necessary.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="51707-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="51707-116">Affected APIs</span></span>

<xref:System.Windows.Forms.NotifyIcon.Text?displayProperty=fullName>

<!--

### Affected APIs

- `P:System.Windows.Forms.NotifyIcon.Text`

### Category

Windows Forms

-->
